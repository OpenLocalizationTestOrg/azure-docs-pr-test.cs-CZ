---
title: "zařízení StorSimple 8000 tootroubleshoot nástroj aaaDiagnostics | Microsoft Docs"
description: "Popisuje režimy zařízení StorSimple hello a vysvětluje, jak toouse Windows Powershellu pro StorSimple toochange hello režim zařízení."
services: storsimple
documentationcenter: 
author: alkohli
manager: timlt
editor: 
ms.assetid: 
ms.service: storsimple
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/27/2017
ms.author: alkohli
ms.openlocfilehash: e8b7fdbc44d2533973b63da841335ba73ba0014b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="use-hello-storsimple-diagnostics-tool-tootroubleshoot-8000-series-device-issues"></a>Použít hello StorSimple Diagnostika tootroubleshoot 8000 řady zařízení problémy

## <a name="overview"></a>Přehled

Hello StorSimple Diagnostika diagnostikuje problémy související toosystem, výkon, sítě a stavu součásti hardwaru pro zařízení StorSimple. Nástroj diagnostiky Hello lze použít v různých situacích. Mezi tyto scénáře patří úlohy plánování, nasazení zařízení StorSimple, vyhodnocování hello síťové prostředí a určení hello výkon provozní zařízení. Tento článek obsahuje přehled hello Diagnostika a popisuje, jak nástroj hello lze použít s zařízení StorSimple.

Nástroj diagnostiky Hello je primárně určený pro řadu zařízení StorSimple 8000 místní (8100 a 8600).

## <a name="run-diagnostics-tool"></a>Spustit diagnostické nástroje

Tento nástroj můžete spustit pomocí rozhraní Windows PowerShell hello zařízení StorSimple. Existují dva způsoby tooaccess hello místního rozhraní zařízení:

* [Konzole sériového portu toohello zařízení PuTTY tooconnect pro použití](storsimple-deployment-walkthrough-u2.md#use-putty-to-connect-to-the-device-serial-console).
* [Vzdálený přístup hello nástroj prostřednictvím hello Windows Powershellu pro StorSimple](storsimple-remote-connect.md).

V tomto článku předpokládáme, že jste připojení toohello konzoly sériového portu zařízení prostřednictvím PuTTY.

#### <a name="toorun-hello-diagnostics-tool"></a>Nástroj diagnostiky toorun hello

Po připojení rozhraní Windows PowerShell toohello hello zařízení, proveďte následující kroky toorun hello rutiny hello.
1. Přihlaste se pomocí následujících kroků hello v konzole sériového portu zařízení toohello [konzoly sériového portu toohello zařízení tooconnect pro použití klienta PuTTY](storsimple-deployment-walkthrough-u2.md#use-putty-to-connect-to-the-device-serial-console).

2. Zadejte hello následující příkaz:

    `Invoke-HcsDiagnostics`

    Pokud není zadán parametr oboru hello, spustí rutina hello hello diagnostické testy. Tyto testy zahrnují systému, stav součásti hardwaru, síť a výkonu. 
    
    toorun pouze konkrétní test, zadejte parametr oboru hello. Například toorun pouze hello test sítě, typ

    `Invoke-HcsDiagnostics -Scope Network`

3. Vyberte a zkopírujte výstup hello z hello PuTTY okna do textového souboru k další analýze.

## <a name="scenarios-toouse-hello-diagnostics-tool"></a>Scénáře toouse hello diagnostického nástroje

Použijte hello diagnostiky nástroj tootroubleshoot hello sítě, výkon, systém a hardwaru stav hello systému. Tady je několik možných scénářů:

* **Zařízení do offline režimu** -zařízení řady StorSimple 8000 si je offline. Z rozhraní Windows PowerShell text hello, ale zdá, že jsou oba řadiče hello spuštěná.
    * Tento nástroj můžete použít toothen určení stavu sítě hello.
         
         > [!NOTE]
         > Nepoužívejte tento nástroj tooassess výkonu a nastavení sítě na zařízení než registrace hello (nebo konfigurace prostřednictvím Průvodce instalací). Platnou IP je přiřazen toohello zařízení během registrace a Průvodce instalací. Tuto rutinu můžete spustit na zařízení, který není zaregistrován pro stav hardwaru a systému. Používejte hello parametr rozsahu, například:
         >
         > `Invoke-HcsDiagnostics -Scope Hardware`
         >
         > `Invoke-HcsDiagnostics -Scope System`

* **Problémy s trvalé zařízení** -dochází k problémům zařízení, které pravděpodobně toopersist. Například registrace se nedaří. Můžete také vykazuje problémy zařízení po hello zařízení je úspěšně registrovaná a funkční nějakou dobu.
    * V takovém případě použijte tento nástroj pro předběžné odstraňování před přihlášením žádost o služby s Microsoft Support. Doporučujeme vám, že spustíte tento výstup hello nástroj a zachycení tento nástroj. Potom můžete zadat Tento výstup tooSupport tooexpedite řešení potíží.
    * Pokud jsou všechny součásti nebo clusteru selhání hardwaru, je zapotřebí přihlásit v žádosti o podporu.

* **Nedostatek výkonu zařízení** -zařízení StorSimple si je pomalé.
    * V takovém případě spusťte tuto rutinu s tooperformance sadu parametr oboru. Analýza výstup hello. Získat hello cloudu latenci pro čtení a zápis. Použití hello hlášené zohlednit v některé zatížení pro vnitřní zpracování dat hello latence maximální dosažitelný cíl a pak nasadit hello zatížení systému hello. Další informace, přejděte příliš[použít hello sítě testování tootroubleshoot zařízení výkonu](#network-test).


## <a name="diagnostics-test-and-sample-outputs"></a>Výstupy testů a ukázkové diagnostiky

### <a name="hardware-test"></a>Test na hardwaru

Tento test Určuje stav hello hello hardwarové součásti, hello Seznam USM firmware a firmware disku hello spuštěny v systému.

* hardwarové součásti Hello hlášené jsou tyto součásti se nezdařilo hello testu nebo nejsou k dispozici v systému hello.
* Hello Seznam USM firmwaru a disku verzí firmwaru jsou hlášené pro hello řadič 0, 1 řadiče a sdílené součásti v systému. Úplný seznam hardwarové součásti přejděte na:

    * [Součásti v primární skříň](storsimple-monitor-hardware-status.md#component-list-for-primary-enclosure-of-storsimple-device)
    * [Součásti v EBOD skříň](storsimple-monitor-hardware-status.md#component-list-for-ebod-enclosure-of-storsimple-device)

> [!NOTE]
> Pokud test na hardwaru hello ohlásí selhání součásti, [protokolu v žádosti o služby se Microsoft Support](storsimple-contact-microsoft-support.md).

#### <a name="sample-output-of-hardware-test-run-on-an-8100-device"></a>Ukázkový výstup spustí na zařízení s 8100 test na hardwaru

Tady je ukázkový výstup ze zařízení StorSimple 8100. V zařízení 8100 modelu hello hello EBOD skříň není nainstalována. Proto nejsou hlášené hello EBOD řadiče součásti.

```
Controller0>Invoke-HcsDiagnostics -Scope Hardware
Running hardware diagnostics ...
--------------------------------------------------
Hardware components failed or not present
----------------------

           Type           State      Controller           Index     EnclosureId
           ----           -----      ----------           -----     -----------
...rVipResource      NotPresent            None               1            None
...rVipResource      NotPresent            None               2            None
...rVipResource      NotPresent            None               3            None
...rVipResource      NotPresent            None               4            None
...rVipResource      NotPresent            None               5            None
...rVipResource      NotPresent            None               6            None
...rVipResource      NotPresent            None               7            None
...rVipResource      NotPresent            None               8            None
...rVipResource      NotPresent            None               9            None
...rVipResource      NotPresent            None              10            None
...rVipResource      NotPresent            None              11            None

Firmware information
----------------------
TalladegaController : ActiveBIOS:0.45.0010
                      BackupBIOS:0.45.0006
                      MainCPLD:17.0.000b
                      ActiveBMCRoot:2.0.001F
                      BackupBMCRoot:2.0.001F
                      BMCBoot:2.0.0002
                      LsiFirmware:20.00.04.00
                      LsiBios:07.37.00.00
                      Battery1Firmware:06.2C
                      Battery2Firmware:06.2C
                      DomFirmware:X231600
                      CanisterFirmware:3.5.0.56
                      CanisterBootloader:5.03
                      CanisterConfigCRC:0x9134777A
                      CanisterVPDStructure:0x06
                      CanisterGEMCPLD:0x19
                      CanisterVPDCRC:0x142F7DC2
                      MidplaneVPDStructure:0x0C
                      MidplaneVPDCRC:0xA6BD4F64
                      MidplaneCPLD:0x10
                      PCM1Firmware:1.00|1.05
                      PCM1VPDStructure:0x05
                      PCM1VPDCRC:0x41BEF99C
                      PCM2Firmware:1.00|1.05
                      PCM2VPDStructure:0x05
                      PCM2VPDCRC:0x41BEF99C

EbodController      :
DisksFirmware       : SmrtStor:TXA2D20400GA6XYR:KZ50
                      SmrtStor:TXA2D20400GA6XYR:KZ50
                      SmrtStor:TXA2D20400GA6XYR:KZ50
                      SmrtStor:TXA2D20400GA6XYR:KZ50
                      WD:WD4001FYYG-01SL3:VR08
                      WD:WD4001FYYG-01SL3:VR08
                      WD:WD4001FYYG-01SL3:VR08
                      WD:WD4001FYYG-01SL3:VR08
                      WD:WD4001FYYG-01SL3:VR08
                      WD:WD4001FYYG-01SL3:VR08
                      WD:WD4001FYYG-01SL3:VR08
                      WD:WD4001FYYG-01SL3:VR08

TalladegaController : ActiveBIOS:0.45.0010
                      BackupBIOS:0.45.0006
                      MainCPLD:17.0.000b
                      ActiveBMCRoot:2.0.001F
                      BackupBMCRoot:2.0.001F
                      BMCBoot:2.0.0002
                      LsiFirmware:20.00.04.00
                      LsiBios:07.37.00.00
                      Battery1Firmware:06.2C
                      Battery2Firmware:06.2C
                      DomFirmware:X231600
                      CanisterFirmware:3.5.0.56
                      CanisterBootloader:5.03
                      CanisterConfigCRC:0x9134777A
                      CanisterVPDStructure:0x06
                      CanisterGEMCPLD:0x19
                      CanisterVPDCRC:0x142F7DC2
                      MidplaneVPDStructure:0x0C
                      MidplaneVPDCRC:0xA6BD4F64
                      MidplaneCPLD:0x10
                      PCM1Firmware:1.00|1.05
                      PCM1VPDStructure:0x05
                      PCM1VPDCRC:0x41BEF99C
                      PCM2Firmware:1.00|1.05
                      PCM2VPDStructure:0x05
                      PCM2VPDCRC:0x41BEF99C

EbodController      :
DisksFirmware       : SmrtStor:TXA2D20400GA6XYR:KZ50
                      SmrtStor:TXA2D20400GA6XYR:KZ50
                      SmrtStor:TXA2D20400GA6XYR:KZ50
                      SmrtStor:TXA2D20400GA6XYR:KZ50
                      WD:WD4001FYYG-01SL3:VR08
                      WD:WD4001FYYG-01SL3:VR08
                      WD:WD4001FYYG-01SL3:VR08
                      WD:WD4001FYYG-01SL3:VR08
                      WD:WD4001FYYG-01SL3:VR08
                      WD:WD4001FYYG-01SL3:VR08
                      WD:WD4001FYYG-01SL3:VR08
                      WD:WD4001FYYG-01SL3:VR08

--------------------------------------------------
```

### <a name="system-test"></a>Testovací systém

Tento test sestav hello systémové informace, hello aktualizace k dispozici, informace o clusteru hello a informace o službě hello pro vaše zařízení.

* informace o systému Hello zahrnuje hello modelu, sériové číslo zařízení, časové pásmo, stav řadiče a verze podrobné softwaru hello běžící na systému hello. hello toounderstand různé parametry systému hlášené jako výstup hello přejděte příliš[interpretace informace o systému](#appendix-interpreting-system-information).

* Dostupnost aktualizace Hello ohlásí, zda je k dispozici režimy hello regular a údržby a jejich názvy přidruženého balíčku. Pokud `RegularUpdates` a `MaintenanceModeUpdates` jsou `false`, znamená to, zda text hello aktualizace nejsou k dispozici. Zařízení je aktuální.
* informace o clusteru Hello obsahuje hello informace o různých logické součásti všechny skupiny clusteru HCS hello a jejich příslušné stavy. Pokud uvidíte skupinu offline clusteru v této části sestavy hello [kontaktovat Microsoft Support](storsimple-contact-microsoft-support.md).
* informace o službě Hello zahrnuje hello názvy a stavy všech hello HCS a položek konfigurace služby spuštěné na zařízení. Tyto informace jsou užitečné pro hello Microsoft Support při řešení potíží problém hello zařízení.

#### <a name="sample-output-of-system-test-run-on-an-8100-device"></a>Ukázkový výstup testu systému spustí na zařízení s 8100

Tady je ukázkový výstup testu systému hello spustí na zařízení s 8100.

```
Controller0>Invoke-HcsDiagnostics -Scope System
Running system diagnostics ...
--------------------------------------------------

System information
----------------------
Controller0:

InstanceId              : 7382407f-a56b-4622-8f3f-756fe04cfd38
Name                    : 8100-SHX0991003G467K
Model                   : 8100
SerialNumber            : SHX0991003G467K
TimeZone                : (UTC-08:00) Pacific Time (US & Canada)
CurrentController       : Controller0
ActiveController        : Controller0
Controller0Status       : Normal
Controller1Status       : Normal
SystemMode              : Normal
FriendlySoftwareVersion : StorSimple 8000 Series Update 4.0
HcsSoftwareVersion      : 6.3.9600.17820
ApiVersion              : 9.0.0.0
VhdVersion              : 6.3.9600.17759
OSVersion               : 6.3.9600.0
CisAgentVersion         : 1.0.9441.0
MdsAgentVersion         : 35.2.2.0
Lsisas2Version          : 2.0.78.0
Capacity                : 219902325555200
RemoteManagementMode    : Disabled
FipsMode                : Enabled

Controller1:
InstanceId              : 7382407f-a56b-4622-8f3f-756fe04cfd38
Name                    : 8100-SHX0991003G467K
Model                   : 8100
SerialNumber            : SHX0991003G467K
TimeZone                :
CurrentController       : Controller0
ActiveController        : Controller0
Controller0Status       : Normal
Controller1Status       : Normal
SystemMode              : Normal
FriendlySoftwareVersion : StorSimple 8000 Series Update 4.0
HcsSoftwareVersion      : 6.3.9600.17820
ApiVersion              : 9.0.0.0
VhdVersion              : 6.3.9600.17759
OSVersion               : 6.3.9600.0
CisAgentVersion         : 1.0.9441.0
MdsAgentVersion         : 35.2.2.0
Lsisas2Version          : 2.0.78.0
Capacity                : 219902325555200
RemoteManagementMode    : HttpsAndHttpEnabled
FipsMode                : Enabled

Update availability
----------------------
RegularUpdates              : False
MaintenanceModeUpdates      : False
RegularUpdatesTitle         : {}
MaintenanceModeUpdatesTitle : {}

Cluster information
----------------------
Name                          State OwnerGroup
----                          ----- ----------
ApplicationHostRLUA           Online HCS Cluster Group
Data0v4                       Online HCS Cluster Group
HCS Vnic Resource             Online HCS Cluster Group
hcs_cloud_connectivity_...    Online HCS Cluster Group
hcs_controller_replacement    Online HCS Cluster Group
hcs_datapath_service          Online HCS Cluster Group
hcs_management_servic         Online HCS Cluster Group
hcs_nvram_service             Online HCS Cluster Group
hcs_passive_datapath          Online HCS Passive Cluster Group
hcs_platform_service          Online HCS Cluster Group
hcs_saas_agent_service        Online HCS Cluster Group
HddDataClusterDisk            Online HCS Cluster Group
HddMgmtClusterDisk            Online HCS Cluster Group
HddReplClusterDisk            Online HCS Cluster Group
iSCSI Target Server           Online HCS Cluster Group
NvramClusterDisk              Online HCS Cluster Group
SSAdminRLUA                   Online HCS Cluster Group
SsdDataClusterDisk            Online HCS Cluster Group
SsdNvramClusterDisk           Online HCS Cluster Group

Service information
----------------------
Name                                          Status DisplayName
----                                          ------ -----------
CiSAgentSvc                                   Stopped CiS Service Agent
hcs_cloud_connectivity_...                    Running hcs_cloud_connectivity...
hcs_controller_replacement                    Running HCS Controller Replace...
hcs_datapath_service                          Running HCS Datapath Service
hcs_management_service                        Running HCS Management Service
hcs_minishell                                 Running hcs_minishell
HCS_NVRAM_Service                             Running HCS NVRAM Service
hcs_passive_datapath                          Stopped HCS Passive Datapath S...
hcs_platform_service                          Running HCS Platform Monitor S...
hcs_saas_agent_service                        Running hcs_saas_agent_service
hcs_startup                                   Stopped hcs_startup
--------------------------------------------------
```

### <a name="network-test"></a>Test sítě

Tento test ověřuje stav hello hello síťových rozhraní, porty, DNS a NTP připojení k serveru, SSL certifikátů, přihlašovacích údajů účtu úložiště, servery pro aktualizaci toohello připojení a připojení k proxy serveru webu na zařízení StorSimple.

#### <a name="sample-output-of-network-test-when-only-data0-is-enabled"></a>Ukázkový výstup sítě testů, pokud je povoleno pouze DATA0

Tady je ukázkový výstup hello zařízení 8100. Zobrazí se ve výstupu hello který:
* Pouze DATA 0 a síťová rozhraní DATA 1 jsou povolené a nakonfigurované.
* DATA 2 až 5 nejsou povolené hello portálu.
* Konfigurace serveru DNS Hello je platný a hello zařízení můžete připojit přes hello DNS server.
* připojení k serveru NTP Hello je také v pořádku.
* Jsou otevřené porty 80 a 443. Port 9354 však je blokován. Podle hello [požadavky na systém sítě](storsimple-system-requirements.md), musíte tento port pro tooopen pro hello service bus komunikaci.
* certifikáty SSL Hello je platný.
* Hello zařízení může připojit účet úložiště toohello: _myss8000storageacct_.
* servery tooUpdate připojení Hello je platný.
* na toto zařízení není nakonfigurován Hello webový proxy server.

#### <a name="sample-output-of-network-test-when-data0-and-data1-are-enabled"></a>Ukázkový výstup testu sítě, pokud jsou povolené DATA0 a DATA1

```
Controller0>Invoke-HcsDiagnostics -Scope Network
Running network diagnostics ....
--------------------------------------------------
Validating networks .....
Name                Entity              Result              Details
----                ------              ------              -------
Network interface   Data0               Valid
Network interface   Data1               Valid
Network interface   Data2               Not enabled
Network interface   Data3               Not enabled
Network interface   Data4               Not enabled
Network interface   Data5               Not enabled
DNS                 10.222.118.154      Valid
NTP                 time.windows.com    Valid
Port                80                  Open
Port                443                 Open
Port                9354                Blocked
SSL certificate     https://myss8000... Valid
Storage account ... myss8000storageacct Valid
URL                 http://download.... Valid
URL                 http://download.... Valid
Web proxy                               Not enabled         Web proxy is not...
--------------------------------------------------
```

### <a name="performance-test"></a>Test výkonnosti

Tento test hlásí hello cloudu výkonu prostřednictvím latenci pro čtení a zápis hello cloudu pro vaše zařízení. Tento nástroj se dá použít tooestablish základní úroveň výkonu hello cloudu, který můžete dosáhnout s StorSimple. Hello nástroj sestavy hello maximální výkon (nejlepší scénář pro latenci pro čtení a zápis), které můžete získat pro připojení.

Jak nástroj hello sestavy hello maximální možná výkonu, můžeme použít hello oznamuje latenci pro čtení a zápis jako cíle při nasazování hello úlohy.

Hello test simuluje velikosti objektu blob hello přidružené typy hello jiný svazek v zařízení hello. Víceúrovňová Regular a zálohování místně vázaných svazků velikost objektu blob 64 KB. Vrstvené svazky s možností archivu zaškrtnuta možnost použít velikost dat blob 512 KB. Pokud vaše zařízení má vrstvené a místně vázaných svazků konfigurace, pouze hello testovací odpovídající too64 KB objekt blob, který je velikost dat spustit.

toouse tento nástroj, proveďte následující kroky hello:

1.  Nejprve vytvořte směs vrstvené svazky a vrstvené svazky s archivovaný možnost zaškrtnutí. Tato akce zajistí, že tento nástroj hello spustí testy hello 64 KB a 512 KB velikosti objektu blob.

2. Spusťte rutinu hello po po vytvoření a konfiguraci hello svazky. Zadejte:

    `Invoke-HcsDiagnostics -Scope Performance`

3. Poznamenejte si latenci pro čtení a zápis hello hlášené nástrojem hello. Tento test může trvat několik minut toorun předtím, než se oznámí hello výsledky.

4. Pokud latence připojení hello všechny pod hello očekávaný rozsah a potom hello latence hlášené hello nástroj lze použít jako maximální dosažitelný cíl při nasazování hello úlohy. Dvoufaktorového v některé zatížení pro vnitřní zpracování dat.

    Pokud latence pro čtení a zápis hello hlášené jsou vysoké hello diagnostického nástroje:

    1. Konfigurace pro služby blob Storage Analytics a analyzujte hello výstup toounderstand hello latence pro hello účtu úložiště Azure. Podrobné pokyny najdete příliš[povolení a konfigurace úložiště analýzy](../storage/common/storage-enable-and-view-metrics.md). Pokud jsou tyto latence také vysoké a porovnatelný z hlediska toohello čísla, který jste dostali od hello StorSimple Diagnostika, musíte toolog žádost o služby s Azure storage.

    2. Pokud jsou nízkou latenci účet úložiště hello, obraťte se na tooinvestigate správce vaší sítě žádné latence problémy ve vaší síti.

#### <a name="sample-output-of-performance-test-run-on-an-8100-device"></a>Ukázkový výstup testu výkonnosti spustí na zařízení s 8100

```
Controller0>Invoke-HcsDiagnostics -Scope Performance
Running performance diagnostics...
--------------------------------------------------
Cloud performance: writing blobs
Cloud write latency: 194 ms using credential 'myss8000storageacct', blob size '64KB'
Cloud performance: reading blobs..
Cloud read latency: 544 ms using credential 'myss8000storageacct', blob size '64KB'
Cloud performance: writing blobs.
Cloud write latency: 369 ms using credential 'myss8000storageacct', blob size '512KB'
Cloud performance: reading blobs...
Cloud read latency: 4924 ms using credential 'myss8000storageacct', blob size '512KB'
--------------------------------------------------
Controller0>
```

## <a name="appendix-interpreting-system-information"></a>Dodatek: interpretace informace o systému

Zde je tabulku popisující jaké hello mapovat různé parametry prostředí Windows PowerShell hello systémové informace. 

| Parametr prostředí PowerShell    | Popis  |
|-------------------------|------------------|
| ID instance             | Každý řadič má jedinečný identifikátor nebo identifikátor GUID s ním spojená.|
| Name (Název)                    | Hello popisný název zařízení hello jako nakonfigurovaný pomocí portálu Azure hello během nasazování zařízení. popisný název výchozí Hello je sériové číslo zařízení hello. |
| Model                   | Hello model zařízení řady StorSimple 8000. Hello modelu může být 8100 nebo 8600.|
| sériové číslo            | sériové číslo zařízení Hello je přiřazena na hello objekt pro vytváření a 15 znaků. Například 8600 SHX0991003G44HT určuje:<br> 8600 – je model zařízení hello.<br>TVX – je hello výrobní lokality.<br> 0991003 - je konkrétní produkt. <br> Sériová čísla jedinečná toocreate zvýší G44HT-hello jsou posledních 5 číslic. Toto nemusí být sekvenční sada.|
| Časové pásmo                | Hello zařízení časové pásmo jako nakonfigurovaný v hello portál Azure během nasazování zařízení.|
| CurrentController       | Hello řadiče, že jste rozhraní Windows PowerShell připojené toothrough hello zařízení StorSimple.|
| ActiveController        | Hello řadiče, který je aktivní na vašem zařízení a řídí všechny operace hello sítě a disku. To může být řadič 0 nebo 1 řadiče.  |
| Controller0Status       | Stav Hello řadič 0 na vašem zařízení. Stav řadiče Hello může být normálního v režimu obnovení, nebo je nedostupný.|
| Controller1Status       | Hello stav řadič 1 na vašem zařízení.  Stav řadiče Hello může být normálního v režimu obnovení, nebo je nedostupný.|
| SystemMode              | Hello celkového stavu zařízení StorSimple. Stav zařízení Hello může být normální údržby, nebo vyřazení (odpovídá toodeactivated v hello portál Azure).|
| FriendlySoftwareVersion | Hello popisný řetězec, který odpovídá verzi softwaru toohello zařízení. U systému, který používá aktualizace 4 verze hello popisný softwaru bude StorSimple 8000 řady aktualizace 4.0.|
| HcsSoftwareVersion      | verze softwaru HCS Hello spuštěné na vašem zařízení. Například hello HCS software verze odpovídající tooStorSimple 8000 je 6.3.9600.17820 4.0 Update řady. |
| ApiVersion              | verze softwaru Hello hello rozhraní API prostředí PowerShell systému Windows hello HCS zařízení.|
| VhdVersion              | verze softwaru Hello hello tovární image, která hello zařízení byl součástí. Pokud obnovíte výchozí nastavení vašeho zařízení toofactory, pak spustí tato verze softwaru.|
| OSVersion               | verze softwaru Hello spuštěné na zařízení hello hello operační systém Windows Server. zařízení StorSimple Hello je založena na Windows Server 2012 R2, která odpovídá too6.3.9600 hello.|
| CisAgentVersion         | Hello verze pro agenta položek konfigurace pro spuštění v zařízení StorSimple. Tento agent pomáhá komunikovat se službou StorSimple Manager hello běžící v Azure.|
| MdsAgentVersion         | Hello verze odpovídající toohello Mds agenta spuštěného v zařízení StorSimple. Tento agent přesune data toohello monitorování a Diagnostika služby MDS ().|
| Lsisas2Version          | Hello verze odpovídající toohello LSI ovladače zařízení StorSimple.|
| Kapacita                | Celková kapacita Hello hello zařízení v bajtech.|
| RemoteManagementMode    | Určuje, zda hello zařízení můžete vzdáleně spravovat přes jeho rozhraní Windows PowerShell. |
| FipsMode                | Určuje, zda hello Spojených států informace o zpracování Standard FIPS (Federal) režimu povolena na vašem zařízení. standard Hello FIPS 140 definuje kryptografické algoritmy pro použití schváleno nám Federal government počítačových systémů hello ochranu citlivá data. Pro zařízení se systémem Update 4 nebo novější je ve výchozím nastavení povolen režim FIPS. |

## <a name="next-steps"></a>Další kroky

* Další informace hello [syntaxe rutiny hello Invoke-HcsDiagnostics](https://technet.microsoft.com/library/mt795371.aspx).

* Další informace o příliš[potíží s nasazením](storsimple-troubleshoot-deployment.md) zařízení StorSimple.
