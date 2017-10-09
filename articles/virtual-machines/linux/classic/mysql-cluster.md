---
title: "aaaClusterize MySQL s vyrovnáváním zatížení sad | Microsoft Docs"
description: "Nastavení pro zařízení s vyrovnáváním zatížení, vysokou dostupnost clusteru Linux MySQL vytvořené pomocí modelu nasazení classic hello v Azure"
services: virtual-machines-linux
documentationcenter: 
author: bureado
manager: timlt
editor: 
tags: azure-service-management
ms.assetid: 6c413a16-e9b5-4ffe-a8a3-ae67046bbdf3
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 04/14/2015
ms.author: jparrel
ms.openlocfilehash: 1829fd877c4b0ed177b23a8e3404dbb3db746561
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="use-load-balanced-sets-tooclusterize-mysql-on-linux"></a>Použití vyrovnávání zatížení sítě nastaví tooclusterize MySQL v systému Linux
> [!IMPORTANT]
> Azure má dva různé modely nasazení pro vytváření a práci s prostředky: [Azure Resource Manager](../../../resource-manager-deployment-model.md) a classic. Tento článek se zabývá pomocí modelu nasazení classic hello. Společnost Microsoft doporučuje, aby většina nových nasazení používala model Resource Manager hello. A [šablony Resource Manageru](https://azure.microsoft.com/documentation/templates/mysql-replication/) je k dispozici, pokud potřebujete toodeploy MySQL cluster.

V tomto článku jsou zde popsány a ukazuje hello různý přístup k dispozici toodeploy systémem Linux služeb s vysokou dostupností v Microsoft Azure, prohlížení vysoké dostupnosti serveru MySQL jako základy. Video ilustrující tento přístup je k dispozici na [Channel 9](http://channel9.msdn.com/Blogs/Open/Load-balancing-highly-available-Linux-services-on-Windows-Azure-OpenLDAP-and-MySQL).

Nemůžeme se popisují shared nothing, dvěma uzly, jeden hlavní řešení vysoké dostupnosti databáze MySQL na základě DRBD, Corosync a kardiostimulátor. Databáze MySQL byla najednou spuštěna pouze jeden uzel. Čtení a zápis z hello DRBD prostředků je také omezena tooonly jednoho uzlu současně.

Není nutné pro VIP řešení jako LVS, protože budete používat sady Vyrovnávání zatížení sítě v tooprovide kruhového dotazování funkce a koncový bod zjišťování, odebrání a úspěšné obnovení hello VIP Microsoft Azure. Hello VIP je globálně směrovatelné adresu IPv4 přiřazené službou Microsoft Azure při prvním vytváření hello cloudové služby.

Nejsou k dispozici jako virtuální počítač na další možné architektury pro databázi MySQL, včetně NBD clusteru, Percona, Galera a několik řešení middleware, včetně alespoň jeden [skladu virtuálních počítačů](http://vmdepot.msopentech.com). Dokud tato řešení můžete replikovat na jednosměrové a vícesměrové vysílání nebo všesměrové vysílání a nejsou spoléhají na sdílené úložiště nebo více síťových rozhraní, hello scénáře by měl být snadno toodeploy v Microsoft Azure.

Tyto clustering architektury lze rozšířit produkty tooother jako PostgreSQL a OpenLDAP podobným způsobem. Například tento postup Vyrovnávání zatížení s nesdílená byla úspěšně testována s více hlavní OpenLDAP a můžete ho sledovat na našem blogu Channel 9.

## <a name="get-ready"></a>Příprava
Budete potřebovat následující hello prostředky a dalo:

  - Účet Microsoft Azure s platným předplatným, možné toocreate aspoň dva virtuální počítače (XS byl použit v tomto příkladu)
  - Síť a podsíť
  - Skupiny vztahů
  - Nastavení dostupnosti
  - Hello možnost toocreate virtuální pevné disky ve stejné oblasti jako cloudová služba hello hello a připojte je toohello virtuální počítače s Linuxem

### <a name="tested-environment"></a>Otestované prostředí
* Ubuntu 13.10
  * DRBD
  * Server databáze MySQL
  * Corosync a kardiostimulátor

### <a name="affinity-group"></a>Skupina vztahů
Vytvořit skupinu vztahů pro řešení hello přihlášením toohello portál Azure classic, vyberte **nastavení**a vytváření skupiny vztahů. Skupina vztahů toothis bude přiřazen přidělené prostředky vytvořit později.

### <a name="networks"></a>Networks
Se vytvoří nová síť a podsíť je vytvořit uvnitř sítě hello. Tento příklad používá 10.10.10.0/24 síť s podsítí pouze jeden /24 uvnitř.

### <a name="virtual-machines"></a>Virtuální počítače
Hello první virtuální počítač 13.10 Ubuntu je vytvořená pomocí image Galerie Endorsed Ubuntu a se nazývá `hadb01`. Nová Cloudová služba je vytvořená ve hello proces se nazývá hadb. Tento název znázorňuje hello sdílené, povaze Vyrovnávání zatížení, která hello služba bude mít, když se přidají další prostředky. Hello vytvoření `hadb01` je bezproblémové a dokončených pomocí portálu hello. Koncový bod SSH je automaticky vytvořen a hello novou síť je vybrána. Nyní můžete vytvořit sadu dostupnosti pro hello virtuálních počítačů.

Po hello první virtuální počítač je vytvořen (technicky při hello cloudové služby), vytvořte druhý virtuální počítač, hello `hadb02`. Pro hello druhé virtuálních počítačů, použít virtuálního počítače s Ubuntu 13.10 z hello Galerie pomocí hello portálu, ale použít stávající cloudovou službu, `hadb.cloudapp.net`, místo vytvoření nové. měla by být automaticky vybrána Hello sítě a dostupnost sady. Koncový bod SSH bude vytvořen, příliš.

Po vytvoření oba virtuální počítače, poznamenejte si hello portu SSH pro `hadb01` (port TCP 22) a `hadb02` (automaticky přiřazené Azure).

### <a name="attached-storage"></a>Připojené úložiště
Připojte nový disk tooboth virtuálních počítačů a vytvořit 5 GB disky v procesu hello. Hello disky jsou hostované v kontejneru VHD hello používá pro disky hlavní operační systém. Po vytvoření a připojené disky nejsou bez nutnosti toorestart Linux, protože hello jádra uvidí hello nové zařízení. Toto zařízení je většinou `/dev/sdc`. Zkontrolujte `dmesg` pro výstup hello.

Na každý virtuální počítač vytvořit oddíl pomocí `cfdisk` (primární, Linux oddíl) a zápis hello nový oddíl tabulky. Nevytvářejte systém souborů na tento oddíl.

## <a name="set-up-hello-cluster"></a>Nastavení clusteru hello
Použijte VÝSTIŽNÝ tooinstall Corosync, kardiostimulátor a DRBD na oba Ubuntu virtuální počítače. toodo Ano s `apt-get`spusťte hello následující kód:

    sudo apt-get install corosync pacemaker drbd8-utils.

V tuto chvíli neinstalujte MySQL. Debian a Ubuntu instalační skripty inicializuje datový adresář MySQL na `/var/lib/mysql`, ale protože hello adresář bude nahrazena systémem souborů DRBD, budete potřebovat později tooinstall MySQL.

Ověření (pomocí `/sbin/ifconfig`) že jsou oba virtuální počítače pomocí adresy v podsíti 10.10.10.0/24 hello a že se navzájem ping podle názvu. Můžete také použít `ssh-keygen` a `ssh-copy-id` toomake, že oba virtuální počítače mohou komunikovat pomocí protokolu SSH bez nutnosti heslo.

### <a name="set-up-drbd"></a>Nastavit DRBD
Vytvořte prostředek DRBD používající základní hello `/dev/sdc1` oddílu tooproduce `/dev/drbd1` prostředků, kterou můžete naformátovat pomocí ext3 a používá primární i sekundární uzly.

1. Otevřete `/etc/drbd.d/r0.res` a kopírování hello následující definice prostředků na oba virtuální počítače:

        resource r0 {
          on `hadb01` {
            device  /dev/drbd1;
            disk   /dev/sdc1;
            address  10.10.10.4:7789;
            meta-disk internal;
          }
          on `hadb02` {
            device  /dev/drbd1;
            disk   /dev/sdc1;
            address  10.10.10.5:7789;
            meta-disk internal;
          }
        }

2. Inicializace hello prostředků pomocí `drbdadm` na oba virtuální počítače:

        sudo drbdadm -c /etc/drbd.conf role r0
        sudo drbdadm up r0

3. Na hello primárního virtuálního počítače (`hadb01`), vynutit vlastnictví (primární) hello DRBD prostředku:

        sudo drbdadm primary --force r0

Pokud si projdete hello obsah nebo proc/drbd (`sudo cat /proc/drbd`) na oba virtuální počítače, měli byste vidět `Primary/Secondary` na `hadb01` a `Secondary/Primary` na `hadb02`a konzistentní s hello řešení v tomto okamžiku. Hello 5-GB místa na disku se synchronizují přes síť 10.10.10.0/24 hello v žádné toocustomers zdarma.

Po synchronizaci hello disku můžete vytvořit systém souborů hello na `hadb01`. Pro účely testování jsme použili ext2, ale hello následující kód vytvoří systém souborů ext3:

    mkfs.ext3 /dev/drbd1

### <a name="mount-hello-drbd-resource"></a>Připojte prostředek DRBD hello
Jste nyní připraven toomount hello DRBD prostředky na `hadb01`. Použití debian a odvozené konfigurace `/var/lib/mysql` jako adresář data na MySQL. Protože jste nenainstalovali MySQL, vytvořit adresář hello a přípojných hello DRBD prostředků. tooperform tuto možnost, spusťte následující kód hello `hadb01`:

    sudo mkdir /var/lib/mysql
    sudo mount /dev/drbd1 /var/lib/mysql

## <a name="set-up-mysql"></a>Nastavit MySQL
Nyní jste připravené tooinstall MySQL na `hadb01`:

    sudo apt-get install mysql-server

Pro `hadb02`, máte dvě možnosti. Můžete nainstalovat mysql-server, který bude vytvářet /var/lib/mysql, vyplnit nový adresář dat a pak odeberte hello obsah. tooperform tuto možnost, spusťte následující kód hello `hadb02`:

    sudo apt-get install mysql-server
    sudo service mysql stop
    sudo rm –rf /var/lib/mysql/*

Druhá možnost Hello je toofailover příliš`hadb02` a pak nainstalujte mysql-server existuje. Skripty instalace si všimněte hello existující instalaci a nebude touch.

Spuštění hello následující kód na `hadb01`:

    sudo drbdadm secondary –force r0

Spuštění hello následující kód na `hadb02`:

    sudo drbdadm primary –force r0
    sudo apt-get install mysql-server

Pokud neplánujete toofailover DRBD nyní, první možnost hello je jednodušší, i když pravděpodobně méně elegantní. Po nastavit tuto možnost, můžete začít pracovat ve vaší databázi MySQL. Spuštění hello následující kód na `hadb02` (nebo libovolného jeden z hello serverů je aktivní, podle tooDRBD):

    mysql –u root –p
    CREATE DATABASE azureha;
    CREATE TABLE things ( id SERIAL, name VARCHAR(255) );
    INSERT INTO things VALUES (1, "Yet another entity");
    GRANT ALL ON things.\* tooroot;

> [!WARNING]
> Tento poslední příkaz efektivně zakáže ověřování pro uživatele root hello v této tabulce. To by měl být nahrazen produkční úrovni udělit příkazy a je jen pro ilustraci zahrnuty.

Pokud chcete, aby dotazy toomake z virtuálních počítačů mimo hello (což je hello účel tohoto průvodce), musíte taky tooenable sítě pro databázi MySQL. Na oba virtuální počítače, otevřete `/etc/mysql/my.cnf` a přejděte příliš`bind-address`. Změna adresy hello z adresy 127.0.0.1 too0.0.0.0. Po uložení souboru hello, vydávání `sudo service mysql restart` na váš aktuální primární.

### <a name="create-hello-mysql-load-balanced-set"></a>Vytvoření sady s vyrovnáváním zatížení MySQL hello
Přejděte zpět toohello portálu, přejděte příliš`hadb01`a zvolte **koncové body**. toocreate na koncový bod, MySQL (TCP 3306) vybírat hello rozevíracího seznamu a vyberte **s vyrovnáváním zatížení vytvořit nové**. Koncový bod Vyrovnávání zatížení na název hello `lb-mysql`. Nastavit **čas** too5 sekund, minimální.

Po vytvoření koncového bodu hello přejděte příliš`hadb02`, zvolte **koncové body**a vytvořit koncový bod. Zvolte `lb-mysql`a potom vyberte z rozevíracího seznamu hello MySQL. Můžete taky hello rozhraní příkazového řádku Azure pro tento krok.

Nyní máte všechny potřebné pro ruční operaci hello clusteru.

### <a name="test-hello-load-balanced-set"></a>Testovací sady hello vyrovnáváním zatížení
Testy můžete provést z mimo počítač, pomocí libovolného klienta, MySQL, nebo pomocí některých aplikací, jako je phpMyAdmin spuštěna jako web Azure. V takovém případě použít nástroj příkazového řádku na MySQL na jiného pole Linux:

    mysql azureha –u root –h hadb.cloudapp.net –e "select * from things;"

### <a name="manually-failing-over"></a>Ručně přebírání služeb při selhání
Převzetí služeb při selhání můžete simulovat vypínání databáze MySQL, přepnutí na DRBD primární a znovu spustit MySQL.

tooperform této úlohy, spusťte následující kód na hadb01 hello:

    service mysql stop && umount /var/lib/mysql ; drbdadm secondary r0

Pak klikněte na hadb02:

    drbdadm primary r0 ; mount /dev/drbd1 /var/lib/mysql && service mysql start

Po selhání ručně, můžete opakovat vzdálený dotaz a měli perfektně fungovat.

## <a name="set-up-corosync"></a>Nastavit Corosync
Corosync je základní infrastruktura clusteru hello požadované pro kardiostimulátor toowork. Pro zjišťování prezenčního signálu (a další metody jako Ultramonkey) je Corosync rozdělení hello CRM funkce, když kardiostimulátor zůstane více podobné tooHeartbeat funkcí.

Hello hlavní omezení pro Corosync v Azure je Corosync upřednostní vícesměrového vysílání přes všesměrové vysílání přes komunikace jednosměrového vysílání, že Microsoft Azure sítě podporuje pouze jednosměrového vysílání.

Naštěstí Corosync má pracovní režim jednosměrového vysílání. pouze skutečné omezení Hello je, že vzhledem k tomu, že všechny uzly nejsou komunikaci mezi sebou, toodefine hello uzly v konfiguračních souborech, včetně jejich IP adresy. Můžeme použít hello Corosync příklad soubory pro jednosměrového vysílání a změňte vazby adresu, uzel seznamy a protokolování adresáře (Ubuntu používá `/var/log/corosync` při hello například souborů použijte `/var/log/cluster`) a povolit nástroje kvora.

> [!NOTE]
> Použijte hello `transport: udpu` směrnice a hello ručně definovaná IP adresy pro oba uzly.

Spuštění hello následující kód na `/etc/corosync/corosync.conf` pro oba uzly:

    totem {
      version: 2
      crypto_cipher: none
      crypto_hash: none
      interface {
        ringnumber: 0
        bindnetaddr: 10.10.10.0
        mcastport: 5405
        ttl: 1
      }
      transport: udpu
    }

    logging {
      fileline: off
      to_logfile: yes
      to_syslog: yes
      logfile: /var/log/corosync/corosync.log
      debug: off
      timestamp: on
      logger_subsys {
        subsys: QUORUM
        debug: off
        }
      }

    nodelist {
      node {
        ring0_addr: 10.10.10.4
        nodeid: 1
      }

      node {
        ring0_addr: 10.10.10.5
        nodeid: 2
      }
    }

    quorum {
      provider: corosync_votequorum
    }

Zkopírujte tento konfigurační soubor na oba virtuální počítače a spustit Corosync v obou uzlů:

    sudo service start corosync

Krátce po spuštění služby hello, by se mělo vytvořit hello cluster v aktuální prstenec hello a by měl být vytvářen kvora. Tato funkce jsme můžete zkontrolovat kontrolou protokolů nebo spuštěním hello následující kód:

    sudo corosync-quorumtool –l

Zobrazí se výstup podobný toohello následující bitové kopie:

![corosync quorumtool - l ukázkový výstup](./media/mysql-cluster/image001.png)

## <a name="set-up-pacemaker"></a>Nastavit kardiostimulátor
Používá kardiostimulátor hello toomonitor clusteru pro prostředky, definují, kdy základní barvy přejděte a přepínače toosecondaries tyto prostředky. Prostředky lze definovat ze sady skriptů, k dispozici nebo z LSB skripty (init jako), mezi další možnosti.

Chceme kardiostimulátor prostředek DRBD příliš "vlastní" hello, hello přípojného bodu a službu MySQL hello. Pokud kardiostimulátor můžete zapnout a vypnout DRBD, připojit a odpojit a pak spustit a zastavit MySQL v hello správné pořadí při něco chybný se stane s hello primární, instalace byla dokončena.

Při první instalaci kardiostimulátor, musí být dostatečně jednoduchá něco jako konfiguraci:

    node $id="1" hadb01
      attributes standby="off"
    node $id="2" hadb02
      attributes standby="off"

1. Zkontrolujte konfiguraci hello spuštěním `sudo crm configure show`.
2. Pak vytvořte soubor (například `/tmp/cluster.conf`) s hello následující prostředky:

        primitive drbd_mysql ocf:linbit:drbd \
              params drbd_resource="r0" \
              op monitor interval="29s" role="Master" \
              op monitor interval="31s" role="Slave"

        ms ms_drbd_mysql drbd_mysql \
              meta master-max="1" master-node-max="1" \
                clone-max="2" clone-node-max="1" \
                notify="true"

        primitive fs_mysql ocf:heartbeat:Filesystem \
              params device="/dev/drbd/by-res/r0" \
              directory="/var/lib/mysql" fstype="ext3"

        primitive mysqld lsb:mysql

        group mysql fs_mysql mysqld

        colocation mysql_on_drbd \
               inf: mysql ms_drbd_mysql:Master

        order mysql_after_drbd \
               inf: ms_drbd_mysql:promote mysql:start

        property stonith-enabled=false

        property no-quorum-policy=ignore

3. Načtení souboru hello do konfigurace hello. Potřebujete jenom toodo to v jednom uzlu.

        sudo crm configure
          load update /tmp/cluster.conf
          commit
          exit

4. Ujistěte se, že kardiostimulátor začíná na spouštění v obou uzlů:

        sudo update-rc.d pacemaker defaults

5. Pomocí `sudo crm_mon –L`, ověřte, že jeden z uzlů se stal hello hlavní hello clusteru a běží všechny prostředky hello. Můžete vytvořit připojení a ps toocheck spuštěným hello prostředky.

Následující snímek obrazovky ukazuje Hello `crm_mon` s jedním uzlem zastavena (ukončení tak, že vyberete kombinaci kláves Ctrl + C):

![uzel crm_mon zastavena](./media/mysql-cluster/image002.png)

Tento snímek obrazovky ukazuje uzly, jeden z nich a jeden podřízený:

![podřízený provozní hlavní crm_mon](./media/mysql-cluster/image003.png)

## <a name="testing"></a>Testování
Jste připraveni simulaci automatické převzetí služeb při selhání. Existují dva způsoby toodo to: a nepodmíněných.

Hello logicky způsob využívá funkce vypnutí hello cluster: ``crm_standby -U `uname -n` -v on``. Pokud použijete toto na hlavní server hello, podřízený hello má. Mějte na paměti tooset tento back toooff. Pokud to neuděláte, crm_mon zobrazí jeden uzel do úsporného režimu.

Hello pevný způsob, jak se vypíná dolů hello primárního virtuálního počítače (hadb01) přes portál hello nebo změnou hello runlevel na hello virtuálního počítače (tj, zastavení, vypnutí). To pomáhá Corosync a kardiostimulátor podle signalizace danou hello předlohu probíhající dolů. Toto můžete otestovat (vhodný pro údržbu), ale můžete vynutit hello scénář zmrazené hello virtuálních počítačů.

## <a name="stonith"></a>STONITH
By mělo být možné tooissue vypnutí virtuálního počítače prostřednictvím rozhraní příkazového řádku Azure hello místo STONITH skript, který řídí fyzického zařízení. Můžete použít `/usr/lib/stonith/plugins/external/ssh` jako základní a povolit STONITH v konfiguraci clusteru hello. Rozhraní příkazového řádku Azure by měly být globálně nainstalovány a hello nastavení publikování a by měly být načteny profilu pro uživatele hello clusteru.

Ukázkový kód pro prostředek hello je k dispozici na [Githubu](https://github.com/bureado/aztonith). Změnit konfiguraci clusteru hello přidáním hello následující příliš`sudo crm configure`:

    primitive st-azure stonith:external/azure \
      params hostlist="hadb01 hadb02" \
      clone fencing st-azure \
      property stonith-enabled=true \
      commit

> [!NOTE]
> skript Hello neprovede nahoru/dolů kontroly. původní prostředků SSH Hello měl 15 příkaz ping kontroluje, ale čas obnovení pro virtuální počítač Azure může být další proměnná.

## <a name="limitations"></a>Omezení
použít Hello následující omezení:

* Hello linbit DRBD prostředků skript, který spravuje DRBD jako prostředek v kardiostimulátor používá `drbdadm down` při vypnutí uzlu, i v případě, že uzel hello se právě děje pohotovostní režim. Toto není ideální protože hello podřízený nebude možné synchronizaci hello DRBD prostředků při hello hlavní získá zápisy. Pokud hlavní hello neselže zdvořile, podřízený hello může trvat přes starší stav systému souborů. Existují dva způsoby potenciální toto řešení:
  * Vynucení `drbdadm up r0` ve všech uzlech clusteru prostřednictvím místní sledovací zařízení (ne clusterized)
  * Úpravy hello linbit DRBD skript, a ověřte, zda `down` není volán`/usr/lib/ocf/resource.d/linbit/drbd`
* Nástroj pro vyrovnávání zatížení Hello potřebuje toorespond alespoň pět sekund, takže aplikace by měla být clustery a být větší toleranci vůči časový limit. Jiné architektury, jako v aplikaci fronty a middlewares dotaz, může také pomoct.
* Ladění MySQL je nezbytné tooensure, které se provádí zápis spravovat tempem a mezipaměti jsou vyprázdněn toodisk často toominimize možné ztrátě paměti.
* Zápis výkonu je závislý na virtuální počítač propojení ve virtuálním přepínači hello, protože se jedná hello mechanismus používaný DRBD tooreplicate hello zařízením.
