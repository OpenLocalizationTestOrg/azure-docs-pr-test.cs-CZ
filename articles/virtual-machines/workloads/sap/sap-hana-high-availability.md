---
title: "aaaHigh dostupnosti SAP HANA ve virtuálních počítačích Azure (VM) | Microsoft Docs"
description: "Vytvořte vysokou dostupnost SAP HANA na virtuálních počítačích Azure (VM)."
services: virtual-machines-linux
documentationcenter: 
author: MSSedusch
manager: timlt
editor: 
ms.service: virtual-machines-linux
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 04/25/2017
ms.author: sedusch
ms.openlocfilehash: dcb9bb70594f9d97f8a888cec76300bcbe0bf1ac
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="high-availability-of-sap-hana-on-azure-virtual-machines-vms"></a>Vysoká dostupnost SAP HANA na virtuálních počítačích Azure (VM)

[dbms-guide]:dbms-guide.md
[deployment-guide]:deployment-guide.md
[planning-guide]:planning-guide.md

[2205917]:https://launchpad.support.sap.com/#/notes/2205917
[1944799]:https://launchpad.support.sap.com/#/notes/1944799
[1928533]:https://launchpad.support.sap.com/#/notes/1928533
[2015553]:https://launchpad.support.sap.com/#/notes/2015553
[2178632]:https://launchpad.support.sap.com/#/notes/2178632
[2191498]:https://launchpad.support.sap.com/#/notes/2191498
[2243692]:https://launchpad.support.sap.com/#/notes/2243692
[1984787]:https://launchpad.support.sap.com/#/notes/1984787
[1999351]:https://launchpad.support.sap.com/#/notes/1999351

[hana-ha-guide-replication]:sap-hana-high-availability.md#14c19f65-b5aa-4856-9594-b81c7e4df73d
[hana-ha-guide-shared-storage]:sap-hana-high-availability.md#498de331-fa04-490b-997c-b078de457c9d

[suse-hana-ha-guide]:https://www.suse.com/docrep/documents/ir8w88iwu7/suse_linux_enterprise_server_for_sap_applications_12_sp1.pdf
[sap-swcenter]:https://launchpad.support.sap.com/#/softwarecenter
[template-multisid-db]:https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2Fsap-3-tier-marketplace-image-multi-sid-db%2Fazuredeploy.json
[template-converged]:https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2Fsap-3-tier-marketplace-image-converged%2Fazuredeploy.json

Na místě, můžete použít buď replikaci HANA systému nebo používat sdílené úložiště tooestablish vysokou dostupnost pro SAP HANA.
Momentálně podporujeme jenom nastavení replikace systému HANA v Azure. SAP HANA replikace se skládá z jednoho hlavní uzel a alespoň jeden podřízený uzel. Změny toohello, jsou data na hlavní uzel hello replikují toohello podřízené uzly synchronně nebo asynchronně.

Tento článek popisuje, jak toodeploy hello virtuálních počítačů, konfiguraci hello virtuálních počítačů, nainstalujte rozhraní framework hello clusteru, nainstalovat a nakonfigurovat replikaci systému SAP HANA.
V hello příklad konfigurace instalace příkazy čísla instance atd 03 a HDB ID HANA systému se používá.

Přečtěte si následující poznámky k SAP a dokumenty Paper nejprve hello

* Poznámka SAP [1928533], na kterém je:
  * Seznam velikostí virtuálních počítačů Azure, které jsou podporovány pro hello nasazení SAP softwaru
  * Kapacita důležité informace o velikosti virtuálního počítače Azure
  * Podporované SAP software a operační systém (OS) a kombinace databáze
  * Požadovaná verze SAP jádra pro Windows a Linux v Microsoft Azure
* Poznámka SAP [2015553] uvádí požadavky pro nasazení softwaru podporovaných SAP SAP v Azure.
* Poznámka SAP [2205917] se doporučuje nastavení operačního systému SUSE Linux Enterprise Server pro aplikace SAP
* Poznámka SAP [1944799] má SAP HANA pokyny pro SUSE Linux Enterprise Server pro aplikace SAP
* Poznámka SAP [2178632] obsahuje podrobné informace o veškeré monitorování metriky pro SAP v Azure.
* Poznámka SAP [2191498] hello vyžaduje verze SAP hostitele agenta pro Linux v Azure.
* Poznámka SAP [2243692] obsahuje informace o licencích SAP v systému Linux v Azure.
* Poznámka SAP [1984787] má obecné informace o SUSE Linux Enterprise Server 12.
* Poznámka SAP [1999351] obsahuje další informace o řešení potíží pro hello Azure rozšířené monitorování rozšíření pro SAP.
* [SAP komunity WIKI](https://wiki.scn.sap.com/wiki/display/HOME/SAPonLinuxNotes) má všechny požadované SAP poznámky pro Linux.
* [Azure virtuálních počítačů, plánování a implementace pro SAP v systému Linux][planning-guide]
* [Nasazení virtuálních počítačů Azure pro SAP v systému Linux (v tomto článku)][deployment-guide]
* [Nasazení virtuálních počítačů databázového systému Azure pro SAP v systému Linux][dbms-guide]
* [SAP HANA SR výkonu optimalizované scénář] [ suse-hana-ha-guide] hello Průvodce obsahuje všechny požadované informace tooset replikaci systému SAP HANA místně. Tohoto průvodce použijte jako Směrný plán.

## <a name="deploying-linux"></a>Nasazení Linux

agent Hello prostředků pro SAP HANA je součástí systému SUSE Linux Enterprise Server pro aplikace SAP.
Hello Azure Marketplace obsahuje bitovou kopii pro SUSE Linux Enterprise Server pro SAP 12 aplikace s BYOS (přineste si vlastní předplatné), které můžete použít toodeploy nových virtuálních počítačů.

### <a name="manual-deployment"></a>Ruční nasazení

1. Vytvoření skupiny prostředků
1. Vytvoření virtuální sítě
1. Vytvořte dva účty úložiště
1. Vytvořit skupinu dostupnosti  
   Sada maximální aktualizace domény
1. Vytvořit nástroj pro vyrovnávání zatížení (interní)  
   Vyberte virtuální síť kroku výše
1. Vytvoření virtuálního počítače 1  
   https://Portal.Azure.com/#Create/SuSE-byos.SLES-for-SAP-byos12-SP1  
   SLES pro SAP aplikace 12 SP1 (BYOS)  
   Vyberte účet úložiště 1  
   Vyberte sady dostupnosti.  
1. Vytvoření virtuálního počítače 2  
   https://Portal.Azure.com/#Create/SuSE-byos.SLES-for-SAP-byos12-SP1  
   SLES pro SAP aplikace 12 SP1 (BYOS)  
   Vyberte účet úložiště 2   
   Vyberte sady dostupnosti.  
1. Přidat datových disků
1. Konfigurace pro vyrovnávání zatížení hello
    1. Vytvořte fond IP front-endu
        1. Otevřete nástroj pro vyrovnávání zatížení hello, vyberte fond IP front-endu a klikněte na tlačítko Přidat
        1. Zadejte název hello hello nové front-endu fond IP adres (například hana-front-endu)
       1. Klikněte na tlačítko OK
        1. Jakmile je vytvořen fond IP front-endu nové hello, zapište si jeho IP adresu
    1. Vytvořte fond back-end
        1. Otevřete nástroj pro vyrovnávání zatížení hello, zvolte back-endové fondy a klikněte na tlačítko Přidat
        1. Zadejte název hello hello nový back-end fondu (například hana-back-end)
        1. Klikněte na tlačítko Přidat virtuální počítač
        1. Vyberte hello jste dříve vytvořili sadu dostupnosti
        1. Vyberte hello virtuální počítače clusteru hello SAP HANA
        1. Klikněte na tlačítko OK
    1. Vytvoření test stavu
       1. Otevřete nástroj pro vyrovnávání zatížení hello, zvolte sondy stavu služby a klikněte na tlačítko Přidat
        1. Zadejte název hello hello nové kontroly stavu (například hana-hp)
        1. Vyberte TCP jako protokol, port 625**03**, zachovat Interval 5 a prahová hodnota špatného stavu 2
        1. Klikněte na tlačítko OK
    1. Vytvoření pravidel vyrovnávání zatížení
        1. Otevřete nástroj pro vyrovnávání zatížení hello, zvolte pravidla Vyrovnávání zatížení a klikněte na tlačítko Přidat
        1. Zadejte název hello hello nové pravidlo Vyrovnávání zatížení (například hana-lb-3**03**15)
        1. Vyberte hello front-endovou IP adresu, back-endový fond a stav testu jste vytvořili dříve (například hana-front-endu)
        1. Zachovat protokol TCP, zadejte port 3**03**15
        1. Zvýšit časový limit nečinnosti too30 minut
       1. **Ujistěte se, že tooenable plovoucí IP adresa**
        1. Klikněte na tlačítko OK
        1. Opakujte kroky hello výše pro port 3**03**17

### <a name="deploy-with-template"></a>Nasazení pomocí šablony
Můžete vytvořit jednu z šablon úvodní hello na githubu toodeploy všechny požadované prostředky. Šablona Hello nasadí hello virtuální počítače, nástroj pro vyrovnávání zatížení hello, dostupnosti atd. Postupujte podle těchto kroků toodeploy hello šablony:

1. Otevřete hello [databáze šablony] [ template-multisid-db] nebo hello [konvergované šablony] [ template-converged] na hello portálu Azure vytvoří šablona databáze hello pouze hello pravidla Vyrovnávání zatížení pro databázi zatímco hello sblížené šablona vytvoří také hello pravidla Vyrovnávání zatížení pro služby ASC nebo SCS a instance YBRAT (pouze Linux). Pokud máte v plánu tooinstall SAP NetWeaver na základě systému a chcete tooinstall hello ASC nebo SCS instance na hello stejné počítače, použijte hello [konvergované šablony][template-converged].
1. Zadejte následující parametry hello
    1. Id systému SAP  
       Zadejte systému SAP hello Id hello chcete tooinstall systému SAP. Hello Id se použije jako předpona pro hello prostředky, které jsou nasazeny.
    1. Typ zásobníku (platí pouze pokud použijete šablonu sblížené hello)  
       Vyberte typ zásobníku SAP NetWeaver hello
    1. Typ operačního systému  
       Vyberte jednu z Linuxových distribucích hello. V tomto příkladu vyberte SLES 12 BYOS
    1. Typ databázového  
       Vyberte HANA
    1. Velikost systému SAP  
       poskytne Hello množství protokoly SAP hello nový systém. Pokud si nejste jisti, kolik systému hello protokoly SAP bude vyžadovat, požádejte SAP technologie partnera nebo systémový integrátor
    1. Dostupnost systému  
       Vyberte HA
    1. Uživatelské jméno správce a heslo správce  
       Po vytvoření nového uživatele, který lze použít toolog na toohello počítači.
    1. Nový nebo existující podsíť  
       Určuje, zda mají být vytvořeny nové virtuální sítě a podsítě nebo by měl použít existující podsítí. Pokud již máte virtuální síť, která je připojená tooyour do místní sítě, vyberte existující.
    1. Id podsítě  
    ID Hello hello podsíť toowhich hello virtuálních počítačů musí být připojené k. Vyberte podsíť hello VPN nebo Express Route virtuální sítě tooconnect hello virtuálního počítače tooyour místní sítě. Hello ID obvykle vypadá /subscriptions/`<subscription id`> /resourceGroups/`<resource group name`> /providers/Microsoft.Network/virtualNetworks/`<virtual network name`> /subnets/`<subnet name`>

## <a name="setting-up-linux-ha"></a>Nastavení Linux HA

Hello následující položky jsou předponou buď [A] - použít tooall uzly, [1] - platí jenom toonode, 1 nebo [2] - platí jenom toonode 2.

1. [A] SLES pro SAP BYOS jenom - zaregistrovat SLES toobe možné toouse hello úložiště
1. [A] SLES pro SAP BYOS pouze – přidání modulu veřejného cloudu
1. [A] aktualizace SLES
    ```bash
    sudo zypper update

    ```

1. [1] přístupu ssh
    ```bash
    sudo ssh-keygen -tdsa
    
    # Enter file in which toosave hello key (/root/.ssh/id_dsa): -> ENTER
    # Enter passphrase (empty for no passphrase): -> ENTER
    # Enter same passphrase again: -> ENTER
    
    # copy hello public key
    sudo cat /root/.ssh/id_dsa.pub
    ```

2. [2] přístupu ssh
    ```bash
    sudo ssh-keygen -tdsa

    # insert hello public key you copied in hello last step into hello authorized keys file on hello second server
    sudo vi /root/.ssh/authorized_keys
    
    # Enter file in which toosave hello key (/root/.ssh/id_dsa): -> ENTER
    # Enter passphrase (empty for no passphrase): -> ENTER
    # Enter same passphrase again: -> ENTER
    
    # copy hello public key    
    sudo cat /root/.ssh/id_dsa.pub
    ```

1. [1] přístupu ssh
    ```bash
    # insert hello public key you copied in hello last step into hello authorized keys file on hello first server
    sudo vi /root/.ssh/authorized_keys
    
    ```

1. [A] nainstalovat rozšíření HA
    ```bash
    sudo zypper install sle-ha-release fence-agents
    
    ```

1. [A] rozložení disku instalační program
    1. LVM  
    Obecně doporučujeme toouse LVM pro svazky, které ukládají data a soubory protokolu. Následující příklad Hello předpokládá, že máte hello virtuální počítače čtyři datových disků připojených, které by měly být použité toocreate dva svazky.
        * Vytvořte pro všechny disky, které chcete toouse fyzických svazků.
    <pre><code>
    sudo pvcreate /dev/sdc
    sudo pvcreate /dev/sdd
    sudo pvcreate /dev/sde
    sudo pvcreate /dev/sdf
    </code></pre>
        * Vytvořit skupinu svazku pro hello datové soubory, jedna skupina svazku pro soubory protokolu hello a jeden pro sdílený adresář hello SAP HANA
    <pre><code>
    sudo vgcreate vg_hana_data /dev/sdc /dev/sdd
    sudo vgcreate vg_hana_log /dev/sde
    sudo vgcreate vg_hana_shared /dev/sdf
    </code></pre>
        * Vytvoření logické svazky hello
    <pre><code>
    sudo lvcreate -l 100%FREE -n hana_data vg_hana_data
    sudo lvcreate -l 100%FREE -n hana_log vg_hana_log
    sudo lvcreate -l 100%FREE -n hana_shared vg_hana_shared
    sudo mkfs.xfs /dev/vg_hana_data/hana_data
    sudo mkfs.xfs /dev/vg_hana_log/hana_log
    sudo mkfs.xfs /dev/vg_hana_shared/hana_shared
    </code></pre>
        * Vytvořte hello připojení adresáře a zkopírujte hello UUID všechny logické svazky
    <pre><code>
    sudo mkdir -p /hana/data
    sudo mkdir -p /hana/log
    sudo mkdir -p /hana/shared
    # write down hello id of /dev/vg_hana_data/hana_data, /dev/vg_hana_log/hana_log and /dev/vg_hana_shared/hana_shared
    sudo blkid
    </code></pre>
        * Vytvořit záznamy fstab pro hello tři logické svazky
    <pre><code>
    sudo vi /etc/fstab
    </code></pre>
    Vložit tento řádek příliš/etc/fstab
    <pre><code>
    /dev/disk/by-uuid/<b>&lt;UUID of /dev/vg_hana_data/hana_data&gt;</b> /hana/data xfs  defaults,nofail  0  2
    /dev/disk/by-uuid/<b>&lt;UUID of /dev/vg_hana_log/hana_log&gt;</b> /hana/log xfs  defaults,nofail  0  2
    /dev/disk/by-uuid/<b>&lt;UUID of /dev/vg_hana_shared/hana_shared&gt;</b> /hana/shared xfs  defaults,nofail  0  2
    </code></pre>
        * Připojit nové svazky hello
    <pre><code>
    sudo mount -a
    </code></pre>
    1. Nešifrovaná disky  
       Pro malé nebo ukázku systémů, můžete umístit HANA soubory protokolu a data na jeden disk. Hello následující příkazy na /dev/sdc vytvořit oddíl a naformátovat ho s xfs.
    ```bash
    sudo fdisk /dev/sdc
    sudo mkfs.xfs /dev/sdc1
    
    # <a name="write-down-hello-id-of-devsdc1"></a>Poznamenejte si hello id /dev/sdc1
    sudo/sbin/blkid sudo vi/etc/fstab
    ```

    Insert this line too/etc/fstab
    <pre><code>
    /dev/disk/by-uuid/<b>&lt;UUID&gt;</b> /hana xfs  defaults,nofail  0  2
    </code></pre>

    Create hello target directory and mount hello disk.

    ```bash
    sudo mkdir /hana
    sudo mount -a
    ```

1. [A] instalační program rozlišení názvu hostitele pro všechny hostitele  
    Můžete buď použít DNS server nebo úpravám hello/etc/hosts na všech uzlech. Tento příklad ukazuje, jak toouse hello soubor/etc/hosts.
   Nahraďte hello IP adresu a název hostitele hello v hello následující příkazy
    ```bash
    sudo vi /etc/hosts
    ```
    Následující hello vložení řádků příliš/etc/hosts. Změnit hello IP adresu a název hostitele toomatch prostředí    
    
    <pre><code>
    <b>&lt;IP address of host 1&gt; &lt;hostname of host 1&gt;</b>
    <b>&lt;IP address of host 2&gt; &lt;hostname of host 2&gt;</b>
    </code></pre>

1. [1] instalaci clusteru
    ```bash
    sudo ha-cluster-init
    
    # Do you want toocontinue anyway? [y/N] -> y
    # Network address toobind too(e.g.: 192.168.1.0) [10.79.227.0] -> ENTER
    # Multicast address (e.g.: 239.x.x.x) [239.174.218.125] -> ENTER
    # Multicast port [5405] -> ENTER
    # Do you wish toouse SBD? [y/N] -> N
    # Do you wish tooconfigure an administration IP? [y/N] -> N
    ```
        
1. [2] přidat uzel toocluster
    ```bash
    sudo ha-cluster-join
        
    # WARNING: NTP is not configured toostart at system boot.
    # WARNING: No watchdog device found. If SBD is used, hello cluster will be unable toostart without a watchdog.
    # Do you want toocontinue anyway? [y/N] -> y
    # IP address or hostname of existing node (e.g.: 192.168.1.1) [] -> IP address of node 1 e.g. 10.0.0.5
    # /root/.ssh/id_dsa already exists - overwrite? [y/N] N
    ```

1. [A] změna hacluster heslo toohello stejné heslo
    ```bash
    sudo passwd hacluster
    
    ```

1. [A] konfigurace corosync toouse jiných přenosu a přidání seznamu. V opačném případě nebude fungovat clusteru.
    ```bash
    sudo vi /etc/corosync/corosync.conf    
    
    ```

    Přidejte následující soubor tučné obsahu toohello hello.
    
    <pre><code> 
    [...]
      interface { 
          [...] 
      }
      <b>transport:      udpu</b>
    } 
    <b>nodelist {
      node {
        ring0_addr:     < ip address of node 1 >
      }
      node {
        ring0_addr:     < ip address of node 2 > 
      } 
    }</b>
    logging {
      [...]
    </code></pre>

    Potom restartujte službu corosync hello

    ```bash
    sudo service corosync restart
    
    ```

1. [A] instalovat balíčky HANA HA  
    ```bash
    sudo zypper install SAPHanaSR
    
    ```

## <a name="installing-sap-hana"></a>Instalace SAP HANA

Postupujte podle kapitoly 4 hello [SAP HANA SR výkonu optimalizované scénář průvodce] [ suse-hana-ha-guide] tooinstall replikaci systému SAP HANA.

1. [A] spusťte hdblcm z hello HANA DVD
    * Zvolte instalace-1 >
    * Vyberte další součásti k instalaci -> 1
    * Zadejte instalační cestu [/ hana/sdílené]: -> zadejte
    * Zadejte název místního hostitele [.]: -> zadejte
    * Chcete, aby tooadd další hostitele toohello systému? (Ano/Ne) [n]: -> zadejte
    * Zadejte ID systému SAP HANA:<SID of HANA e.g. HDB>
    * Zadejte čísla Instance [00]:   
  Čísla HANA Instance. Použijte 03, pokud používá hello šablony Azure nebo postupovali podle výše uvedeném příkladu hello
    * Vyberte režim databáze / zadejte Index [1]: -> zadejte
    * Vyberte použití systému / zadejte Index [4]:  
  Vyberte systém hello využití
    * Zadejte umístění datových svazků [/ hana/data/HDB]: -> zadejte
    * Zadejte umístění protokolu svazků [/ hana/log/HDB]: -> zadejte
    * Omezení přidělení paměti maximální? [n]: -> zadejte
    * Zadejte název hostitele certifikát pro hostitele,..." [...]: -> Zadejte
    * Zadejte SAP hostitele agenta uživatele (sapadm) heslo:
    * Potvrďte SAP hostitele agenta uživatele (sapadm) heslo:
    * Zadejte správce systému (hdbadm) heslo:
    * Potvrzení správce systému (hdbadm) heslo:
    * Zadejte domovský adresář správce systému [/ usr/sap nebo HDB/domovské]: -> zadejte
    * Zadejte prostředí přihlášení správce systému [/ bin/dílet]: -> zadejte
    * Zadejte ID uživatele správce systému [1001]: -> zadejte
    * Zadejte ID ze skupiny uživatelů (sapsys) [79]: -> zadejte
    * Zadejte heslo k databázi uživatelů (systém):
    * Potvrďte heslo k databázi uživatelů (systém):
    * Restartování systému po restartování počítače? [n]: -> zadejte
    * Chcete toocontinue? (Ano/Ne):  
  Ověřit hello souhrnné a zadejte y toocontinue
1. [A] Agent hostitele upgradu SAP  
  Stáhnout nejnovější archivu Agent hostitele SAP hello z hello [SAP Softwarecenter] [ sap-swcenter] a spusťte hello následující příkaz tooupgrade hello agenta. Nahraďte hello cesta toohello toopoint toohello soubor archivu, které jste stáhli.
    ```bash
    sudo /usr/sap/hostctrl/exe/saphostexec -upgrade -archive <path tooSAP Host Agent SAR>
    ```

1. [1] vytvoření HANA replikace (jako uživatel root)  
    Spusťte následující příkaz hello. Ujistěte se, že tooreplace tučné řetězce (HANA systému ID HDB a číslo instance 03) s hodnotami hello instalace SAP HANA.
    <pre><code>
    PATH="$PATH:/usr/sap/<b>HDB</b>/HDB<b>03</b>/exe"
    hdbsql -u system -i <b>03</b> 'CREATE USER <b>hdb</b>hasync PASSWORD "<b>passwd</b>"' 
    hdbsql -u system -i <b>03</b> 'GRANT DATA ADMIN too<b>hdb</b>hasync' 
    hdbsql -u system -i <b>03</b> 'ALTER USER <b>hdb</b>hasync DISABLE PASSWORD LIFETIME' 
    </code></pre>

1. [A] vytvoření položky úložiště klíčů (jako uživatel root)
    <pre><code>
    PATH="$PATH:/usr/sap/<b>HDB</b>/HDB<b>03</b>/exe"
    hdbuserstore SET <b>hdb</b>haloc localhost:3<b>03</b>15 <b>hdb</b>hasync <b>passwd</b>
    </code></pre>
1. [1] zálohy databáze (jako uživatel root)
    <pre><code>
    PATH="$PATH:/usr/sap/<b>HDB</b>/HDB<b>03</b>/exe"
    hdbsql -u system -i <b>03</b> "BACKUP DATA USING FILE ('<b>initialbackup</b>')" 
    </code></pre>
1. [1] přepněte toohello sapsid uživatele (například hdbadm) a vytvořit hello primární lokality.
    <pre><code>
    su - <b>hdb</b>adm
    hdbnsutil -sr_enable –-name=<b>SITE1</b>
    </code></pre>
1. [2] přepněte toohello sapsid uživatele (například hdbadm) a vytvořit hello sekundární lokality.
    <pre><code>
    su - <b>hdb</b>adm
    sapcontrol -nr <b>03</b> -function StopWait 600 10
    hdbnsutil -sr_register --remoteHost=<b>saphanavm1</b> --remoteInstance=<b>03</b> --replicationMode=sync --name=<b>SITE2</b> 
    </code></pre>

## <a name="configure-cluster-framework"></a>Konfigurace architektury clusteru

Změňte výchozí nastavení hello

<pre>
sudo vi crm-defaults.txt
# enter hello following toocrm-defaults.txt
<code>
property $id="cib-bootstrap-options" \
  no-quorum-policy="ignore" \
  stonith-enabled="true" \
  stonith-action="reboot" \
  stonith-timeout="150s"
rsc_defaults $id="rsc-options" \
  resource-stickiness="1000" \
  migration-threshold="5000"
op_defaults $id="op-options" \
  timeout="600"
</code>

# <a name="now-we-load-hello-file-toohello-cluster"></a>Nyní se nám načíst hello souboru toohello clusteru
sudo crm nakonfigurovat aktualizace zatížení crm-defaults.txt
</pre>

### <a name="create-stonith-device"></a>Vytvoření STONITH zařízení

zařízení STONITH Hello používá tooauthorize objekt služby pro Microsoft Azure. Postupujte podle těchto kroků toocreate hlavní název služby.

1. Přejděte příliš<https://portal.azure.com>
1. Okno Azure Active Directory otevřete hello  
   Přejděte tooProperties a zapište hello ID adresáře. Toto je hello **id klienta**.
1. Klikněte na možnost registrace aplikace
1. Klikněte na tlačítko Přidat.
1. Zadejte název, vyberte typ aplikace "Aplikace webového rozhraní API", zadejte přihlašovací adresu URL (například http://localhost) a klikněte na možnost vytvořit
1. Hello přihlašovací adresa URL se nepoužívá a může být libovolná platná adresa URL
1. Vyberte hello nové aplikace a klikněte na tlačítko klíče na kartě nastavení hello
1. Zadejte popis pro nový klíč, vyberte "Je platné stále" a klikněte na Uložit
1. Poznamenejte si hodnotu hello. Použije se jako hello **heslo** pro hello instančního objektu
1. Zapište hello ID aplikace. Použije se jako hello uživatelské jméno (**přihlašovacího id** v následujících kroků hello) z hello instančního objektu

Hello instanční objekt nemá oprávnění tooaccess vašich prostředků Azure ve výchozím nastavení. Je třeba toogive hello instanční objekt oprávnění toostart a zastavení (zrušit přidělení) všechny virtuální počítače clusteru hello.

1. Přejděte toohttps://portal.azure.com
1. Otevřete hello okno všechny prostředky
1. Vyberte virtuální počítač hello
1. Klikněte na řízení přístupu (IAM)
1. Klikněte na tlačítko Přidat.
1. Vyberte roli hello vlastníka
1. Zadejte název hello hello aplikace, kterou jste vytvořili výše
1. Klikněte na tlačítko OK

Poté, co jste upravili hello oprávnění pro hello virtuální počítače, můžete nakonfigurovat zařízení STONITH hello v clusteru hello.

<pre>
sudo vi crm-fencing.txt
# enter hello following toocrm-fencing.txt
# replace hello bold string with your subscription id, resource group, tenant id, service principal id and password
<code>
primitive rsc_st_azure_1 stonith:fence_azure_arm \
    params subscriptionId="<b>subscription id</b>" resourceGroup="<b>resource group</b>" tenantId="<b>tenant id</b>" login="<b>login id</b>" passwd="<b>password</b>"

primitive rsc_st_azure_2 stonith:fence_azure_arm \
    params subscriptionId="<b>subscription id</b>" resourceGroup="<b>resource group</b>" tenantId="<b>tenant id</b>" login="<b>login id</b>" passwd="<b>password</b>"

colocation col_st_azure -2000: rsc_st_azure_1:Started rsc_st_azure_2:Started
</code>

# <a name="now-we-load-hello-file-toohello-cluster"></a>Nyní se nám načíst hello souboru toohello clusteru
sudo crm nakonfigurovat aktualizace zatížení crm-fencing.txt
</pre>

### <a name="create-sap-hana-resources"></a>Vytvořit prostředky SAP HANA

<pre>
sudo vi crm-saphanatop.txt
# enter hello following toocrm-saphana.txt
# replace hello bold string with your instance number and HANA system id
<code>
primitive rsc_SAPHanaTopology_<b>HDB</b>_HDB<b>03</b> ocf:suse:SAPHanaTopology \
    operations $id="rsc_sap2_<b>HDB</b>_HDB<b>03</b>-operations" \
    op monitor interval="10" timeout="600" \
    op start interval="0" timeout="600" \
    op stop interval="0" timeout="300" \
    params SID="<b>HDB</b>" InstanceNumber="<b>03</b>"

clone cln_SAPHanaTopology_<b>HDB</b>_HDB<b>03</b> rsc_SAPHanaTopology_<b>HDB</b>_HDB<b>03</b> \
    meta is-managed="true" clone-node-max="1" target-role="Started" interleave="true"
</code>

# <a name="now-we-load-hello-file-toohello-cluster"></a>Nyní se nám načíst hello souboru toohello clusteru
sudo crm nakonfigurovat aktualizace zatížení crm-saphanatop.txt
</pre>

<pre>
sudo vi crm-saphana.txt
# enter hello following toocrm-saphana.txt
# replace hello bold string with your instance number, HANA system id and hello frontend IP address of hello Azure load balancer. 
<code>
primitive rsc_SAPHana_<b>HDB</b>_HDB<b>03</b> ocf:suse:SAPHana \
    operations $id="rsc_sap_<b>HDB</b>_HDB<b>03</b>-operations" \
    op start interval="0" timeout="3600" \
    op stop interval="0" timeout="3600" \
    op promote interval="0" timeout="3600" \
    op monitor interval="60" role="Master" timeout="700" \
    op monitor interval="61" role="Slave" timeout="700" \
    params SID="<b>HDB</b>" InstanceNumber="<b>03</b>" PREFER_SITE_TAKEOVER="true" \
    DUPLICATE_PRIMARY_TIMEOUT="7200" AUTOMATED_REGISTER="false"

ms msl_SAPHana_<b>HDB</b>_HDB<b>03</b> rsc_SAPHana_<b>HDB</b>_HDB<b>03</b> \
    meta is-managed="true" notify="true" clone-max="2" clone-node-max="1" \
    target-role="Started" interleave="true"

primitive rsc_ip_<b>HDB</b>_HDB<b>03</b> ocf:heartbeat:IPaddr2 \ 
    meta target-role="Started" is-managed="true" \ 
    operations $id="rsc_ip_<b>HDB</b>_HDB<b>03</b>-operations" \ 
    op monitor interval="10s" timeout="20s" \ 
    params ip="<b>10.0.0.21</b>" 
primitive rsc_nc_<b>HDB</b>_HDB<b>03</b> anything \ 
    params binfile="/usr/bin/nc" cmdline_options="-l -k 625<b>03</b>" \ 
    op monitor timeout=20s interval=10 depth=0 
group g_ip_<b>HDB</b>_HDB<b>03</b> rsc_ip_<b>HDB</b>_HDB<b>03</b> rsc_nc_<b>HDB</b>_HDB<b>03</b>
 
colocation col_saphana_ip_<b>HDB</b>_HDB<b>03</b> 2000: g_ip_<b>HDB</b>_HDB<b>03</b>:Started \ 
    msl_SAPHana_<b>HDB</b>_HDB<b>03</b>:Master  
order ord_SAPHana_<b>HDB</b>_HDB<b>03</b> 2000: cln_SAPHanaTopology_<b>HDB</b>_HDB<b>03</b> \ 
    msl_SAPHana_<b>HDB</b>_HDB<b>03</b>
</code>

# <a name="now-we-load-hello-file-toohello-cluster"></a>Nyní se nám načíst hello souboru toohello clusteru
sudo crm nakonfigurovat aktualizace zatížení crm-saphana.txt
</pre>

### <a name="test-cluster-setup"></a>Nastavení clusteru s podporou testu
Hello následující kapitole popisují, jak můžete otestovat vašeho nastavení. Každý test předpokládá, že jsou kořenové a hlavní SAP HANA hello běží na saphanavm1 hello virtuálního počítače.

#### <a name="fencing-test"></a>Vymezení testu

Instalační program hello hello vymezení agenta můžete otestovat zakázáním hello síťové rozhraní na uzlu saphanavm1.

<pre><code>
sudo ifdown eth0
</code></pre>

Hello virtuální počítač by měl nyní restartovat nebo byla zastavena v závislosti na konfiguraci clusteru.
Pokud jste nastavili hello stonith akce toooff, hello virtuálního počítače se zastaví a hello prostředky jsou migrované toohello spuštění virtuálního počítače.

Po spuštění virtuálního počítače hello, hello SAP HANA prostředků se nezdaří toostart jako sekundární, pokud jste nastavili AUTOMATED_REGISTER = "false". V takovém případě je třeba tooconfigure hello HANA instance jako sekundární spuštěním hello následující příkaz:

<pre><code>
su - <b>hdb</b>adm

# Stop hello HANA instance just in case it is running
sapcontrol -nr <b>03</b> -function StopWait 600 10
hdbnsutil -sr_register --remoteHost=<b>saphanavm2</b> --remoteInstance=<b>03</b> --replicationMode=sync --name=<b>SITE1</b>

# switch back tooroot and cleanup hello failed state
exit
crm resource cleanup msl_SAPHana_<b>HDB</b>_HDB<b>03</b> <b>saphanavm1</b>
</code></pre>

#### <a name="testing-a-manual-failover"></a>Testování ruční převzetí služeb při selhání

Zastavování služby kardiostimulátor hello na uzlu saphanavm1, můžete otestovat ruční převzetí služeb při selhání.
<pre><code>
service pacemaker stop
</code></pre>

Po hello převzetí služeb při selhání můžete službu hello spustit znovu. Hello SAP HANA prostředek na saphanavm1 se nezdaří toostart jako sekundární, pokud jste nastavili AUTOMATED_REGISTER = "false". V takovém případě je třeba tooconfigure hello HANA instance jako sekundární spuštěním hello následující příkaz:

<pre><code>
service pacemaker start
su - <b>hdb</b>adm

# Stop hello HANA instance just in case it is running
sapcontrol -nr <b>03</b> -function StopWait 600 10
hdbnsutil -sr_register --remoteHost=<b>saphanavm2</b> --remoteInstance=<b>03</b> --replicationMode=sync --name=<b>SITE1</b> 


# switch back tooroot and cleanup hello failed state
exit
crm resource cleanup msl_SAPHana_<b>HDB</b>_HDB<b>03</b> <b>saphanavm1</b>
</code></pre>

#### <a name="testing-a-migration"></a>Testování migrace

Můžete migrovat hello SAP HANA hlavního uzlu tak, že spustíte následující příkaz hello
<pre><code>
crm resource migrate msl_SAPHana_<b>HDB</b>_HDB<b>03</b> <b>saphanavm2</b>
crm resource migrate g_ip_<b>HDB</b>_HDB<b>03</b> <b>saphanavm2</b>
</code></pre>

To je potřeba migrovat hello SAP HANA hlavní uzel a hello skupinu, která obsahuje toosaphanavm2 hello virtuální IP adresy.
Hello SAP HANA prostředek na saphanavm1 se nezdaří toostart jako sekundární, pokud jste nastavili AUTOMATED_REGISTER = "false". V takovém případě je třeba tooconfigure hello HANA instance jako sekundární spuštěním hello následující příkaz:

<pre><code>
su - <b>hdb</b>adm

# Stop hello HANA instance just in case it is running
sapcontrol -nr <b>03</b> -function StopWait 600 10
hdbnsutil -sr_register --remoteHost=<b>saphanavm2</b> --remoteInstance=<b>03</b> --replicationMode=sync --name=<b>SITE1</b> 
</code></pre>

migrace Hello vytvoří omezení umístění, vyžadující toobe znovu odstranit.

<pre><code>
crm configure edited

# delete location contraints that are named like hello following contraint. You should have two contraints, one for hello SAP HANA resource and one for hello IP address group.
location cli-prefer-g_ip_<b>HDB</b>_HDB<b>03</b> g_ip_<b>HDB</b>_HDB<b>03</b> role=Started inf: <b>saphanavm2</b>
</code></pre>

Musíte taky toocleanup hello stav prostředku sekundární uzel hello

<pre><code>
# switch back tooroot and cleanup hello failed state
exit
crm resource cleanup msl_SAPHana_<b>HDB</b>_HDB<b>03</b> <b>saphanavm1</b>
</code></pre>

## <a name="next-steps"></a>Další kroky
* [Azure virtuálních počítačů, plánování a implementace pro SAP][planning-guide]
* [Nasazení virtuálních počítačů Azure pro SAP][deployment-guide]
* [Nasazení virtuálních počítačů databázového systému Azure pro SAP][dbms-guide]
* jak tooestablish vysokou dostupnost a plán pro zotavení po havárii SAP HANA v Azure (velké instance), najdete v části toolearn [SAP HANA (velké instance) vysoké dostupnosti a zotavení po havárii v Azure](hana-overview-high-availability-disaster-recovery.md). 
