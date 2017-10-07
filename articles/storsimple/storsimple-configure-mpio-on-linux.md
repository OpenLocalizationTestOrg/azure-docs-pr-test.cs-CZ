---
title: aaaConfigure funkci MPIO na hostitele platformy StorSimple Linux | Microsoft Docs
description: "Konfigurace funkce MPIO na StorSimple připojené tooa Linux hostitele se systémem CentOS 6.6"
services: storsimple
documentationcenter: NA
author: alkohli
manager: carmonm
editor: tysonn
ms.assetid: ca289eed-12b7-4e2e-9117-adf7e2034f2f
ms.service: storsimple
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 12/01/2016
ms.author: alkohli
ms.openlocfilehash: d9f7e02903243494c909313fb2c33ac690764274
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="configure-mpio-on-a-storsimple-host-running-centos"></a>Konfigurace funkce MPIO na StorSimple hostitele se systémem CentOS
Tento článek vysvětluje hello kroky požadované tooconfigure více cest vstupně-výstupní operace (MPIO) na serveru hostitele Centos 6.6. hostitelský server Hello je zařízení připojené tooyour Microsoft Azure StorSimple pro vysokou dostupnost prostřednictvím iniciátory iSCSI. Popisuje podrobnosti hello automatického zjišťování vícenásobný zařízení a hello konkrétní nastavení pouze pro svazky zařízení StorSimple.

Tento postup je použít tooall hello modely řadu zařízení StorSimple 8000.

> [!NOTE]
> Tento postup nelze použít pro virtuální zařízení StorSimple. Další informace najdete v tématu Jak tooconfigure hostitelské servery pro vaše virtuální zařízení.
> 
> 

## <a name="about-multipathing"></a>O vytváření více cest
Funkce vytváření více cest Hello vám umožní tooconfigure více vstupně-výstupních cest mezi hostitelským serverem a zařízením úložiště. Tyto cesty vstupně-výstupní operace jsou fyzické připojení SAN, která může zahrnovat samostatné kabely, přepínače, síťových rozhraní a řadiče. Více cest agreguje hello vstupně-výstupních cest tooconfigure nové zařízení, která souvisí s všechny cesty hello agregovat.

účelem Hello více cest je dvojí:

* **Vysoká dostupnost**: poskytuje alternativní cestu, pokud se nezdaří libovolný element hello vstupně-výstupní cestu (například kabel, přepínače, síťové nebo řadič).
* **Vyrovnávání zatížení**: v závislosti na konfiguraci hello zařízení úložiště, se může zlepšit výkon hello zjišťuje zatížením na cesty hello vstupně-výstupních operací a dynamicky vyrovnává těchto zatížení.

### <a name="about-multipathing-components"></a>O komponentách více cest
Více cest v systému Linux se skládá z jádra a komponenty uživatelského prostoru jako v následující tabulce.

* **Jádra**: součást hlavní hello je hello *zařízení mapper* , bude přesměrována vstupně-výstupních operací a podporuje převzetí služeb při selhání pro cesty a cesty skupiny.

* **Uživatel místo**: Jedná se o *multipath nástroje* , správě multipathed zařízení instruující hello zařízení mapper vícenásobný modulu co toodo. Nástroje pro Hello obsahovat:
   
   * **Funkce Multipath**: uvádí a konfiguruje multipathed zařízení.
   * **Multipathd**: démon procesu, který spouští funkci multipath a monitorování hello cesty.
   * **Název Devmap**: poskytuje smysluplný název zařízení tooudev pro devmaps.
   * **Kpartx**: mapuje lineární devmaps toodevice oddíly toomake vícenásobný mapy rozdělený.
   * **Multipath.conf**: konfigurační soubor pro více cest démon, který je použité toooverwrite hello předdefinované konfigurace tabulky.

### <a name="about-hello-multipathconf-configuration-file"></a>O hello multipath.conf konfiguračního souboru
konfigurační soubor Hello `/etc/multipath.conf` provede mnoho hello více cest funkce uživatelsky konfigurovatelného. Hello `multipath` příkaz a hello démon jádra `multipathd` použijte informace v tomto souboru. soubor Hello je konzultaci pouze během konfigurace hello hello vícenásobný zařízení. Ujistěte se, že jsou všechny změny provedené před spuštěním hello `multipath` příkaz. Pokud upravíte soubor hello můžete později, bude nutné toostop a spusťte multipathd znovu pro hello změny tootake vliv.

Hello multipath.conf má pět částí:

- **Výchozí nastavení na úrovni systému** *(výchozí)*: můžete přepsat výchozí nastavení na úrovni systému.
- **Zakázáno zařízení** *(černý list)*: můžete zadat hello seznam zařízení, které by neměly být řízené zařízení mapper.
- **Blokovaných výjimky** *(blacklist_exceptions)*: můžete identifikovat považovat za zařízení s funkcí multipath i v případě, že uvedené v hello zakázaných toobe konkrétní zařízení.
- **Konkrétní nastavení řadiče úložiště** *(zařízení)*: můžete zadat nastavení konfigurace, které budou použité toodevices, které mají dodavatele a informace o produktu.
- **Nastavení pro konkrétní zařízení** *(multipaths)*: nastavení konfigurace hello toofine tune této části můžete použít pro jednotlivé logické jednotky.

## <a name="configure-multipathing-on-storsimple-connected-toolinux-host"></a>Na hostiteli StorSimple připojené tooLinux nakonfigurovat více cest
Hostitele platformy Linux tooa připojeno zařízení StorSimple lze nakonfigurovat pro vysokou dostupnost a vyrovnávání zatížení. Například pokud hello Linux hostitele má dvě rozhraní zařízení sítě SAN a hello připojené toohello má dvě rozhraní připojené toohello sítě SAN, které tato rozhraní jsou ve stejné podsíti hello, pak bude 4 cesty k dispozici. Ale pokud každé rozhraní DATA na zařízení a hostitele rozhraní hello jsou v jiné podsíti protokolu IP (a ne směrovatelné), pak pouze 2 cesty nebudou k dispozici. Můžete nakonfigurovat více cest tooautomatically zjistit všechny cesty k dispozici hello, zvolte algoritmu Vyrovnávání zatížení u těchto cest, použít specifické nastavení pro svazky jen StorSimple a pak povolte a ověřte více cest.

Hello následující postup popisuje, jak tooconfigure více cest, když zařízení StorSimple se dvěma síťovými rozhraními připojené tooa hostitele se dvěma síťovými rozhraními.

## <a name="prerequisites"></a>Požadavky
Tato část podrobně hello požadavky konfigurace pro CentOS server a zařízení StorSimple.

### <a name="on-centos-host"></a>Na hostiteli CentOS
1. Ujistěte se, že má váš hostitel CentOS 2 síťových rozhraní, které jsou povolené. Zadejte:
   
    `ifconfig`
   
    Hello následující příklad ukazuje výstup hello při dva síťové rozhraní (`eth0` a `eth1`) jsou k dispozici na hostiteli hello.
   
        [root@centosSS ~]# ifconfig
        eth0  Link encap:Ethernet  HWaddr 00:15:5D:A2:33:41  
          inet addr:10.126.162.65  Bcast:10.126.163.255  Mask:255.255.252.0
          inet6 addr: 2001:4898:4010:3012:215:5dff:fea2:3341/64 Scope:Global
          inet6 addr: fe80::215:5dff:fea2:3341/64 Scope:Link
          UP BROADCAST RUNNING MULTICAST  MTU:1500  Metric:1
         RX packets:36536 errors:0 dropped:0 overruns:0 frame:0
          TX packets:6312 errors:0 dropped:0 overruns:0 carrier:0
          collisions:0 txqueuelen:1000
          RX bytes:13994127 (13.3 MiB)  TX bytes:645654 (630.5 KiB)
   
        eth1  Link encap:Ethernet  HWaddr 00:15:5D:A2:33:42  
          inet addr:10.126.162.66  Bcast:10.126.163.255  Mask:255.255.252.0
          inet6 addr: 2001:4898:4010:3012:215:5dff:fea2:3342/64 Scope:Global
          inet6 addr: fe80::215:5dff:fea2:3342/64 Scope:Link
          UP BROADCAST RUNNING MULTICAST  MTU:1500  Metric:1
          RX packets:25962 errors:0 dropped:0 overruns:0 frame:0
          TX packets:11 errors:0 dropped:0 overruns:0 carrier:0
          collisions:0 txqueuelen:1000
          RX bytes:2597350 (2.4 MiB)  TX bytes:754 (754.0 b)
   
        loLink encap:Local Loopback  
          inet addr:127.0.0.1  Mask:255.0.0.0
          inet6 addr: ::1/128 Scope:Host
          UP LOOPBACK RUNNING  MTU:65536  Metric:1
          RX packets:12 errors:0 dropped:0 overruns:0 frame:0
          TX packets:12 errors:0 dropped:0 overruns:0 carrier:0
          collisions:0 txqueuelen:0
          RX bytes:720 (720.0 b)  TX bytes:720 (720.0 b)
2. Nainstalujte *iSCSI. iniciátoru utils* na vašem serveru CentOS. Proveďte následující kroky tooinstall hello *iSCSI. iniciátoru utils*.
   
   1. Přihlaste se jako `root` do svého CentOS hostitele.
   2. Nainstalujte hello *iSCSI. iniciátoru utils*. Zadejte:
      
       `yum install iscsi-initiator-utils`
   3. Po hello *iSCSI. iniciátoru utils* je úspěšně nainstalován, spuštění služby iSCSI hello. Zadejte:
      
       `service iscsid start`
      
       V případech `iscsid` může ve skutečnosti neumožňují spuštění a hello `--force` možnost může být potřeba.
   4. tooensure, že vaše iniciátor iSCSI je povolena při spuštění, použijte hello `chkconfig` příkaz tooenable hello služby.
      
       `chkconfig iscsi on`
   5. tooverify, které bylo správně nastavit, spusťte příkaz hello:
      
       `chkconfig --list | grep iscsi`
      
       Ukázkový výstup najdete níž.
      
           iscsi   0:off   1:off   2:on3:on4:on5:on6:off
           iscsid  0:off   1:off   2:on3:on4:on5:on6:off
      
       Z hello výše příklad uvidíte, že vaše prostředí iSCSI bude spouštět na spuštění spuštění úrovně 2, 3, 4 a 5.
3. Nainstalujte *zařízení. mapper multipath*. Zadejte:
   
    `yum install device-mapper-multipath`
   
    Hello instalace začne. Typ **Y** toocontinue po zobrazení výzvy k potvrzení.

### <a name="on-storsimple-device"></a>Na zařízení StorSimple
Zařízení StorSimple musí mít:

* Minimálně dvě rozhraní povolená pro iSCSI. tooverify, že jsou dvě rozhraní iSCSI povolený v zařízení StorSimple, proveďte následující kroky v hello klasický portál Azure pro zařízení StorSimple hello:
  
  1. Přihlaste se k portálu classic hello pro zařízení StorSimple.
  2. Vybrat služby StorSimple Manager, klikněte na tlačítko **zařízení** a zvolte konkrétní zařízení StorSimple hello. Klikněte na tlačítko **konfigurace** a ověřte nastavení síťového rozhraní hello. Snímek obrazovky s dvě rozhraní iSCSI povolený sítě jsou uvedeny níže. Sem DATA 2 a DATA 3, oba 10 GbE je povoleno rozhraní iSCSI.
     
      ![Konfigurace funkce MPIO StorsSimple DATA 2](./media/storsimple-configure-mpio-on-linux/IC761347.png)
     
      ![Funkce MPIO StorSimple dat 3 konfigurace](./media/storsimple-configure-mpio-on-linux/IC761348.png)
     
      V hello **konfigurace** stránky
     
     1. Zajistěte, aby obě síťová rozhraní iSCSI povolený. Hello **iSCSI povoleno** pole musí být nastavené příliš**Ano**.
     2. Zkontrolujte, zda text hello síťových rozhraní hello stejnou rychlostí, jak by měla být 1 GbE nebo 10 GbE.
     3. Poznamenejte si hello IPv4 adresy rozhraní iSCSI povolený hello a uložit pro pozdější použití na hostiteli hello.
* rozhraní iSCSI Hello zařízení StorSimple by měly být dostupné z hello CentOS serveru.
      tooverify, je nutné tooprovide hello IP adresy vašich rozhraní iSCSI povolený sítě StorSimple na hostitelském serveru. Hello příkazy používané a hello odpovídající výstup s DATA2 (10.126.162.25) a DATA3 (10.126.162.26) jsou uvedeny níže:
  
        [root@centosSS ~]# iscsiadm -m discovery -t sendtargets -p 10.126.162.25:3260
        10.126.162.25:3260,1 iqn.1991-05.com.microsoft:storsimple8100-shx0991003g44mt-target
        10.126.162.26:3260,1 iqn.1991-05.com.microsoft:storsimple8100-shx0991003g44mt-target

### <a name="hardware-configuration"></a>Konfigurace hardwaru
Doporučujeme vám, že připojíte hello dvě iSCSI síťová rozhraní na samostatné cesty pro redundanci. Hello obrázek níže znázorňuje hello doporučené hardwarové konfigurace pro vysokou dostupnost a vyrovnávaní zatížení více cest pro váš CentOS server a zařízení StorSimple.

![Hardwarová konfigurace funkce MPIO pro hostitele tooLinux StorSimple](./media/storsimple-configure-mpio-on-linux/MPIOHardwareConfigurationStorSimpleToLinuxHost2M.png)

Jak je znázorněno v předchozích obrázek hello:

* Zařízení StorSimple je v konfiguraci aktivní pasivní s dva řadiče.
* Dva přepínače sítě SAN jsou připojené tooyour řadiče zařízení.
* Dva iniciátory iSCSI jsou povolené v zařízení StorSimple.
* Na hostiteli CentOS jsou povolené dvě síťová rozhraní.

Hello výše konfigurace předá 4 samostatné cest mezi hostiteli zařízení a hello, pokud jsou směrovatelné hello data hostitelů a rozhraní.

> [!IMPORTANT]
> * Doporučujeme vám, že nemíchat 1 GbE a 10 GbE síťová rozhraní pro vytváření více cest. Při použití dvou síťových rozhraní, musí být obě rozhraní hello hello identické typu.
> * V zařízení StorSimple, DATA0, DATA1, DATA4 a DATA5 jsou 1 GbE rozhraní zatímco DATA2 a DATA3 10 GbE síťových rozhraní. |
> 
> 

## <a name="configuration-steps"></a>Kroky konfigurace
Hello kroky konfigurace pro více cest zahrnovat konfigurace hello k dispozici cesty pro automatické zjišťování, zadání hello Vyrovnávání zatížení algoritmus toouse, povolení více cest a nakonec ověření konfigurace hello. Každý z těchto kroků je podrobněji v následující části hello.

### <a name="step-1-configure-multipathing-for-automatic-discovery"></a>Krok 1: Konfigurace používání více cest pro automatické zjišťování
zařízení podporující funkci multipath Hello lze automaticky zjistit a nakonfigurovaná.

1. Inicializace `/etc/multipath.conf` souboru. Zadejte:
   
     `mpathconf --enable`
   
    Hello výše příkaz vytvoří `sample/etc/multipath.conf` souboru.
2. Vícenásobný službu spusťte. Zadejte:
   
    `service multipathd start`
   
    Zobrazí se následující výstup hello:
   
    `Starting multipathd daemon:`
3. Povolte automatické zjišťování multipaths. Zadejte:
   
    `mpathconf --find_multipaths y`
   
    To slouží k úpravě hello výchozí část vaší `multipath.conf` jak je uvedeno níže:
   
        defaults {
        find_multipaths yes
        user_friendly_names yes
        path_grouping_policy multibus
        }

### <a name="step-2-configure-multipathing-for-storsimple-volumes"></a>Krok 2: Konfigurace používání více cest pro svazky zařízení StorSimple
Ve výchozím nastavení všechna zařízení jsou černé uvedené v souboru multipath.conf hello a bude možné obejít. Pro svazky zařízení StorSimple budete potřebovat více cest tooallow toocreate blacklist výjimky.

1. Upravit hello `/etc/mulitpath.conf` souboru. Zadejte:
   
    `vi /etc/multipath.conf`
2. Vyhledejte v souboru multipath.conf hello hello blacklist_exceptions oddíl. Zařízení StorSimple musí toobe uveden jako zakázaných výjimka v této části. Můžete Odkomentujte, relevantní řádků tento soubor toomodify ho takto (použijte pouze hello konkrétní model hello zařízení, který používáte):
   
        blacklist_exceptions {
            device {
                       vendor  "MSFT"
                       product "STORSIMPLE 8100*"
            }
            device {
                       vendor  "MSFT"
                       product "STORSIMPLE 8600*"
            }
           }

### <a name="step-3-configure-round-robin-multipathing"></a>Krok 3: Konfigurace pomocí kruhového dotazování více cest
Tento algoritmus Vyrovnávání zatížení používá všechny aktivního řadiče k dispozici multipaths toohello hello vyrovnáváním, kruhové dotazování způsobem.

1. Upravit hello `/etc/multipath.conf` souboru. Zadejte:
   
    `vi /etc/multipath.conf`
2. V části hello `defaults` část, sada hello `path_grouping_policy` příliš`multibus`. Hello `path_grouping_policy` určuje hello výchozí cestu seskupení zásad tooapply toounspecified multipaths. část výchozí hodnoty Hello bude vypadat, jak je uvedeno níže.
   
        defaults {
                user_friendly_names yes
                path_grouping_policy multibus
        }

> [!NOTE]
> Hello nejběžnější hodnoty `path_grouping_policy` zahrnují:
> 
> * převzetí služeb při selhání = 1 cesty na skupinu s prioritou
> * multibus = všechny platné cesty ve skupině s prioritou 1
> 
> 

### <a name="step-4-enable-multipathing"></a>Krok 4: Povolení používání více cest
1. Restartujte hello `multipathd` démona. Zadejte:
   
    `service multipathd restart`
2. výstup Hello bude, jak je uvedeno níže:
   
        [root@centosSS ~]# service multipathd start
        Starting multipathd daemon:  [OK]

### <a name="step-5-verify-multipathing"></a>Krok 5: Ověření více cest
1. Nejdříve se ujistěte, že iSCSI se naváže připojení zařízení StorSimple hello následujícím způsobem:
   
   a. Zjistit zařízení StorSimple. Zadejte:
      
    ```
    iscsiadm -m discovery -t sendtargets -p  <IP address of network interface on hello device>:<iSCSI port on StorSimple device>
    ```
    
    výstup Hello, pokud je IP adresa pro DATA0 10.126.162.25 a otevřít port 3260 v zařízení StorSimple hello pro přenosy iSCSI odchozí je, jak je uvedeno níže:
    
    ```
    10.126.162.25:3260,1 iqn.1991-05.com.microsoft:storsimple8100-shx0991003g00dv-target
    10.126.162.26:3260,1 iqn.1991-05.com.microsoft:storsimple8100-shx0991003g00dv-target
    ```

    Kopírování hello IQN zařízení StorSimple `iqn.1991-05.com.microsoft:storsimple8100-shx0991003g00dv-target`, z předchozích výstup hello.

   b. Připojte zařízení toohello pomocí cíl IQN. zařízení StorSimple Hello je služby iSCSI target hello. Zadejte:

    ```
    iscsiadm -m node --login -T <IQN of iSCSI target>
    ```

    Hello následující příklad ukazuje výstup s cílem IQN systému `iqn.1991-05.com.microsoft:storsimple8100-shx0991003g00dv-target`. výstup Hello označuje, že jste úspěšně připojení dvě rozhraní iSCSI povolený sítě toohello na vašem zařízení.

    ```
    Logging in too[iface: eth0, target: iqn.1991-05.com.microsoft:storsimple8100-shx0991003g00dv-target, portal: 10.126.162.25,3260] (multiple)
    Logging in too[iface: eth1, target: iqn.1991-05.com.microsoft:storsimple8100-shx0991003g00dv-target, portal: 10.126.162.25,3260] (multiple)
    Logging in too[iface: eth0, target: iqn.1991-05.com.microsoft:storsimple8100-shx0991003g00dv-target, portal: 10.126.162.26,3260] (multiple)
    Logging in too[iface: eth1, target: iqn.1991-05.com.microsoft:storsimple8100-shx0991003g00dv-target, portal: 10.126.162.26,3260] (multiple)
    Login too[iface: eth0, target: iqn.1991-05.com.microsoft:storsimple8100-shx0991003g00dv-target, portal: 10.126.162.25,3260] successful.
    Login too[iface: eth1, target: iqn.1991-05.com.microsoft:storsimple8100-shx0991003g00dv-target, portal: 10.126.162.25,3260] successful.
    Login too[iface: eth0, target: iqn.1991-05.com.microsoft:storsimple8100-shx0991003g00dv-target, portal: 10.126.162.26,3260] successful.
    Login too[iface: eth1, target: iqn.1991-05.com.microsoft:storsimple8100-shx0991003g00dv-target, portal: 10.126.162.26,3260] successful.
    ```

    Pokud se zobrazí pouze jednoho hostitele rozhraní a dvě cesty sem, budete potřebovat tooenable obě hello rozhraní na hostiteli pro iSCSI. Můžete postupovat podle hello [podrobné pokyny v dokumentaci k systému Linux](https://access.redhat.com/documentation/Red_Hat_Enterprise_Linux/5/html/Online_Storage_Reconfiguration_Guide/iscsioffloadmain.html).

2. Svazek je zveřejněné toohello CentOS server ze zařízení StorSimple hello. Další informace najdete v tématu [krok 6: vytvoření svazku](storsimple-deployment-walkthrough.md#step-6-create-a-volume) prostřednictvím hello portál Azure classic na zařízení StorSimple.

3. Ověření cesty k dispozici hello. Zadejte:

      ```
      multipath –l
      ```

      Hello následující příklad ukazuje výstup hello dvě síťová rozhraní na StorSimple zařízení připojených tooa jeden hostitel síťové rozhraní se dvě cesty k dispozici.

        ```
        mpathb (36486fd20cc081f8dcd3fccb992d45a68) dm-3 MSFT,STORSIMPLE 8100
        size=100G features='0' hwhandler='0' wp=rw
        `-+- policy='round-robin 0' prio=0 status=active
        |- 7:0:0:1 sdc 8:32 active undef running
        `- 6:0:0:1 sdd 8:48 active undef running
        ```

        hello following example shows hello output for two network interfaces on a StorSimple device connected tootwo host network interfaces with four available paths.

        ```
        mpathb (36486fd27a23feba1b096226f11420f6b) dm-2 MSFT,STORSIMPLE 8100
        size=100G features='0' hwhandler='0' wp=rw
        `-+- policy='round-robin 0' prio=0 status=active
        |- 17:0:0:0 sdb 8:16 active undef running
        |- 15:0:0:0 sdd 8:48 active undef running
        |- 14:0:0:0 sdc 8:32 active undef running
        `- 16:0:0:0 sde 8:64 active undef running
        ```

        After hello paths are configured, refer toohello specific instructions on your host operating system (Centos 6.6) toomount and format this volume.

## <a name="troubleshoot-multipathing"></a>Řešení potíží s více cest
Tato část obsahuje některé užitečné tipy, pokud narazíte na potíže během konfigurace více cest.

OTÁZKY. Nevidím hello změny v `multipath.conf` souboru neprojeví.

A. Pokud jste provedli jakékoli změny toohello `multipath.conf` soubor, budete potřebovat toorestart hello více cest služby. Zadejte hello následující příkaz:

    service multipathd restart

OTÁZKY. Je povoleno dvě síťová rozhraní na zařízení StorSimple hello a dvě síťová rozhraní na hostiteli hello. Při zobrazení seznamu cest k dispozici hello, vidím pouze dvě cesty. Očekávání toosee čtyři dostupné cesty.

A. Ujistěte se, že jsou hello dvě cesty na hello stejné podsíti a směrovatelné. Pokud hello síťových rozhraní jsou na různých sítí VLAN a není směrovatelné, zobrazí se pouze dvě cesty. Jedním ze způsobů tooverify jde toomake jistotu, že se lze připojit i rozhraní hello hostitele ze síťového rozhraní na zařízení StorSimple hello. Budete potřebovat příliš[kontaktovat Microsoft Support](storsimple-contact-microsoft-support.md) jako toto ověření provést pouze prostřednictvím podpory relace.

OTÁZKY. Při zobrazení seznamu cest k dispozici, nezobrazí žádný výstup.

A. Obvykle není zobrazuje všechny cesty multipathed naznačuje problém s více cest démon hello a bude s největší pravděpodobností, jakýkoli problém s zde spočívá v hello `multipath.conf` souboru.

Toto nastavení by také být vhodné kontrola, zda se ve skutečnosti zobrazí některé disky po připojení toohello cíl, jako žádná odpověď z vícecestného výpisů hello může také znamená, že nemáte žádné disky.

* Použijte následující příkaz toorescan hello ke sběrnici SCSI hello:
  
    `$ rescan-scsi-bus.sh `(součást balíčku sg3_utils)
* Zadejte hello následující příkazy:
  
    `$ dmesg | grep sd*`
     
     Nebo
  
    `$ fdisk –l`
  
    Tyto vrátí podrobnosti o nedávno přidané disky.
* toodetermine toho, jestli je StorSimple disk, použijte hello následující příkazy:
  
    `cat /sys/block/<DISK>/device/model`
  
    Tato možnost vrátí řetězec, který bude zjistit, jestli je StorSimple disk.

Méně pravděpodobné, ale možná příčina A mohou být také zastaralé iscsid pid. Použijte následující příkaz toolog vypnout z relace iSCSI hello hello:

    iscsiadm -m node --logout -p <Target_IP>

Tento příkaz opakujte u všech síťových rozhraní hello připojení na cíli iSCSI hello, což je zařízení StorSimple. Jakmile jste se odhlásili z všechny relace iSCSI hello, použijte hello iSCSI target relace iSCSI IQN tooreestablish hello. Zadejte hello následující příkaz:

    iscsiadm -m node --login -T <TARGET_IQN>


OTÁZKY. Nejste si jisti, pokud zařízení je seznam povolených adres.

A. tooverify jestli zařízení je seznam povolených adres, použijte následující řešení potíží interaktivního příkazu hello:

    multipathd –k
    multipathd> show devices
    available block devices:
    ram0 devnode blacklisted, unmonitored
    ram1 devnode blacklisted, unmonitored
    ram2 devnode blacklisted, unmonitored
    ram3 devnode blacklisted, unmonitored
    ram4 devnode blacklisted, unmonitored
    ram5 devnode blacklisted, unmonitored
    ram6 devnode blacklisted, unmonitored
    ram7 devnode blacklisted, unmonitored
    ram8 devnode blacklisted, unmonitored
    ram9 devnode blacklisted, unmonitored
    ram10 devnode blacklisted, unmonitored
    ram11 devnode blacklisted, unmonitored
    ram12 devnode blacklisted, unmonitored
    ram13 devnode blacklisted, unmonitored
    ram14 devnode blacklisted, unmonitored
    ram15 devnode blacklisted, unmonitored
    loop0 devnode blacklisted, unmonitored
    loop1 devnode blacklisted, unmonitored
    loop2 devnode blacklisted, unmonitored
    loop3 devnode blacklisted, unmonitored
    loop4 devnode blacklisted, unmonitored
    loop5 devnode blacklisted, unmonitored
    loop6 devnode blacklisted, unmonitored
    loop7 devnode blacklisted, unmonitored
    sr0 devnode blacklisted, unmonitored
    sda devnode whitelisted, monitored
    dm-0 devnode blacklisted, unmonitored
    dm-1 devnode blacklisted, unmonitored
    dm-2 devnode blacklisted, unmonitored
    sdb devnode whitelisted, monitored
    sdc devnode whitelisted, monitored
    dm-3 devnode blacklisted, unmonitored


Další informace, přejděte příliš[použít řešení potíží s interaktivního příkazu pro více cest](http://www.centos.org/docs/5/html/5.1/DM_Multipath/multipath_config_confirm.html).

## <a name="list-of-useful-commands"></a>Seznam užitečné příkazy
| Typ | Příkaz | Popis |
| --- | --- | --- |
| **iSCSI** |`service iscsid start` |Spuštění služby iSCSI |
| &nbsp; |`service iscsid stop` |Zastavit službu iSCSI |
| &nbsp; |`service iscsid restart` |Restartujte službu iSCSI |
| &nbsp; |`iscsiadm -m discovery -t sendtargets -p <TARGET_IP>` |Zjišťování dostupných cílů na hello zadaná adresa |
| &nbsp; |`iscsiadm -m node --login -T <TARGET_IQN>` |Přihlaste se toohello cíle iSCSI |
| &nbsp; |`iscsiadm -m node --logout -p <Target_IP>` |Odhlaste se z cíle iSCSI hello |
| &nbsp; |`cat /etc/iscsi/initiatorname.iscsi` |Tisk – název iniciátoru iSCSI |
| &nbsp; |`iscsiadm –m session –s <sessionid> -P 3` |Zkontrolujte stav hello relace iSCSI hello a svazek zjištěných na hostiteli hello |
| &nbsp; |`iscsi –m session` |Zobrazí všechny relace iSCSI hello navázat mezi hostiteli hello a hello zařízení StorSimple |
|  | | |
| **Více cest** |`service multipathd start` |Démon spuštění více cest |
| &nbsp; |`service multipathd stop` |Zastavit vícenásobný démon |
| &nbsp; |`service multipathd restart` |Restartujte vícenásobný démon |
| &nbsp; |`chkconfig multipathd on` </br> NEBO </br> `mpathconf –with_chkconfig y` |Povolení funkce multipath démon toostart při spuštění |
| &nbsp; |`multipathd –k` |Spuštění hello interaktivní konzoly pro řešení potíží |
| &nbsp; |`multipath –l` |Připojení více cest seznamu a zařízení |
| &nbsp; |`mpathconf --enable` |Vytvořte soubor ukázka mulitpath.conf v`/etc/mulitpath.conf` |
|  | | |

## <a name="next-steps"></a>Další kroky
Jak na hostiteli systému Linux jsou konfigurace funkce MPIO, budete pravděpodobně potřebovat toorefer toohello následující dokumenty CentoS 6.6:

* [Nastavení funkce MPIO na CentOS](http://www.centos.org/docs/5/html/5.1/DM_Multipath/setup_procedure.html)
* [Průvodce školení Linux](http://linux-training.be/files/books/LinuxAdm.pdf)

