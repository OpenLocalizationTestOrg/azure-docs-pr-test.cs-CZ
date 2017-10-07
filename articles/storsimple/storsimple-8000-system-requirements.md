---
title: "požadavky na systém řady aaaStorSimple 8000 | Microsoft Docs"
description: "Popisuje softwaru, sítě a vysokou dostupnost požadavky a osvědčené postupy pro řešení Microsoft Azure StorSimple."
services: storsimple
documentationcenter: NA
author: alkohli
manager: timlt
editor: 
ms.assetid: 
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 07/10/2017
ms.author: alkohli
ms.openlocfilehash: f13ccdb7cb317d72c60e9c2fe49937764d10b43e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="storsimple-8000-series-software-high-availability-and-networking-requirements"></a>Software řady StorSimple 8000, vysokou dostupnost a požadavky na síť

## <a name="overview"></a>Přehled

Vítejte tooMicrosoft Azure StorSimple. Tento článek popisuje důležité systémové požadavky a osvědčené postupy pro zařízení StorSimple a hello úložiště klientům přístup k zařízení hello. Doporučujeme, abyste si prošli hello informace pečlivě před nasazením systému StorSimple a pak odkazovat zpět tooit podle potřeby během nasazení a další operace.

Hello systémové požadavky:

* **Požadavky na software pro klienty úložiště** -popisuje hello podporované operační systémy a veškeré další požadavky pro tyto operační systémy.
* **Požadavky sítě pro zařízení StorSimple hello** – poskytuje informace o portech hello této toobe potřeba otevřít v tooallow vaší brány firewall pro přenosy iSCSI, cloudu nebo správu.
* **Požadavky na vysokou dostupnost pro StorSimple** – popisuje požadavky na vysokou dostupnost a osvědčených postupů pro StorSimple zařízení a hostitele počítače.

## <a name="software-requirements-for-storage-clients"></a>Požadavky na software pro klienty úložiště

Hello následující požadavky na software jsou pro klienty hello úložiště, které přístup k zařízení StorSimple.

| Podporované operační systémy | Požadovaná verze | Další požadavky a poznámky |
| --- | --- | --- |
| Windows Server |2008 R2 SP1, 2012, 2012 R2, 2016 |Svazky zařízení StorSimple iSCSI jsou podporovány pro použití v pouze hello následující typy disků systému Windows:<ul><li>Jednoduchý svazek na základním disku</li><li>Jednoduché a zrcadlený svazek na dynamickém disku</li></ul>Jsou podporovány pouze hello softwaru iniciátory iSCSI nativně součástí hello operačního systému. Iniciátory iSCSI hardwaru nejsou podporovány.<br></br>Windows Server 2012 a dynamické zajišťování 2016 a ODX funkce jsou podporované, pokud používáte svazek StorSimple iSCSI.<br><br>StorSimple můžete vytvořit dynamicky zajištěné a zcela zřizované svazky. Ji nelze vytvářet částečně zřizované svazky.<br><br>Přeformátování dynamicky zajištěné svazku může trvat dlouhou dobu. Doporučujeme odstranit hello svazku a vytvořit novou místo přeformátování. Ale pokud stále upřednostňovat tooreformat svazku:<ul><li>Spusťte následující příkaz před hello přeformátovat tooavoid místo recyklaci zpoždění hello: <br>`fsutil behavior set disabledeletenotify 1`</br></li><li>Po dokončení formátování hello použití hello následující příkaz toore povolit recyklace místa:<br>`fsutil behavior set disabledeletenotify 0`</br></li><li>Opravu hotfix hello systému Windows Server 2012, jak je popsáno v [KB 2878635](https://support.microsoft.com/kb/2870270) tooyour počítače systému Windows Server.</li></ul></li></ul></ul> Pokud konfigurujete Snapshot Manager zařízení StorSimple nebo adaptér StorSimple pro službu SharePoint, přejděte příliš[požadavky na Software pro volitelné součásti](#software-requirements-for-optional-components). |
| VMware ESX |5.5 a 6.0 |U VMware vSphere podporovány jako klient iSCSI. Funkce VAAI-block je podporovaná s VMware vSphere na zařízení StorSimple. |
| Linux RHEL nebo CentOS |5, 6 a 7 |Podpora pro Linux iSCSI klienty s verzemi iniciátor iSCSI otevřete 5, 6 a 7. |
| Linux |SUSE Linux 11 | |

> [!NOTE]
> IBM AIX se aktuálně nepodporuje s StorSimple.


## <a name="software-requirements-for-optional-components"></a>Požadavky na software pro volitelné součásti

Hello následující požadavky na software se hello volitelné StorSimple součásti (Snapshot Manager zařízení StorSimple a StorSimple adaptéru pro službu SharePoint).

| Komponenta | Platforma hostitele | Další požadavky a poznámky |
| --- | --- | --- |
| StorSimple Snapshot Manager |Windows Server 2008 R2 SP1, 2012, 2012 R2 |Použití služby StorSimple Snapshot Manager v systému Windows Server je nutné pro zálohování a obnovení zrcadlené dynamických disků a všechny zálohování konzistentní s aplikací.<br> Snapshot Manager zařízení StorSimple je podporována pouze na Windows Server 2008 R2 SP1 (64 bitů), Windows Server 2012 R2 a Windows Server 2012.<ul><li>Pokud používáte okno Server 2012, musíte nainstalovat rozhraní .NET 3.5 – 4.5 před instalací StorSimple Snapshot Manager.</li><li>Pokud používáte Windows Server 2008 R2 SP1, musíte před instalací StorSimple Snapshot Manager nainstalovat Windows Management Framework 3.0.</li></ul> |
| StorSimple Adapter pro SharePoint |Windows Server 2008 R2 SP1, 2012, 2012 R2 |<ul><li>Adaptér StorSimple pro službu SharePoint je podporována pouze na serveru SharePoint 2010 a SharePoint 2013.</li><li>RBS vyžaduje SQL Server Enterprise Edition, verze 2008 R2 nebo 2012.</li></ul> |

## <a name="networking-requirements-for-your-storsimple-device"></a>Požadavky sítě pro zařízení StorSimple

Zařízení StorSimple je zařízení uzamčené. Však musí porty toobe otevřít v tooallow vaší brány firewall pro iSCSI, cloud a přenosy pro správu. Hello následující tabulka uvádí hello porty, které je třeba toobe otevřít v bráně firewall. V této tabulce *v* nebo *příchozí* odkazuje toohello směr, ve kterém příchozí žádosti klientů, přístup k zařízení. *Out* nebo *odchozí* odkazuje toohello směr, ve kterém zařízení StorSimple odešle data externě, kromě nasazení hello: například odchozí toohello Internetu.

| Číslo portu<sup>1,2</sup> | Příchozí nebo odchozí | Rozsah portů | Požaduje se | Poznámky |
| --- | --- | --- | --- | --- |
| TCP 80 (HTTP)<sup>3</sup> |na více systémů |WAN |Ne |<ul><li>Odchozí port je používán pro aktualizace tooretrieve přístup k Internetu.</li><li>odchozí webový proxy server Hello je konfigurovatelná uživatelem.</li><li>tooallow aktualizací systému, tento port je také třeba otevřít pro pevné IP adresy řadiče hello.</li></ul> |
| TCP 443 (HTTPS)<sup>3</sup> |na více systémů |WAN |Ano |<ul><li>Odchozí port se používá pro přístup k datům v cloudu hello.</li><li>odchozí webový proxy server Hello je konfigurovatelná uživatelem.</li><li>tooallow aktualizací systému, tento port je také třeba otevřít pro pevné IP adresy řadiče hello.</li><li>Tento port se také používá na oba hello řadiče pro uvolňování paměti.</li></ul> |
| UDP 53 (DNS) |na více systémů |WAN |V některých případech; v části poznámky. |Tento port je povinný, jenom v případě, že používáte adresu serveru DNS pro internetový. |
| UDP 123 (NTP) |na více systémů |WAN |V některých případech; v části poznámky. |Tento port je povinný, jenom v případě, že používáte server NTP založené na Internetu. |
| TCP 9354 |na více systémů |WAN |Ano |Hello odchozí port je používán toocommunicate zařízení StorSimple hello s hello služby StorSimple Manager zařízení. |
| 3260 (iSCSI) |V |LAN |Ne |Tento port je použité tooaccess data přes iSCSI. |
| 5985 |V |LAN |Ne |Příchozí port je používán toocommunicate StorSimple Snapshot Manager zařízení StorSimple hello.<br>Tento port se používá také při vzdálené připojení tooWindows Powershellu pro StorSimple přes protokol HTTP. |
| 5986 |V |LAN |Ne |Tento port se používá při vzdálené připojení tooWindows Powershellu pro StorSimple přes protokol HTTPS. |

<sup>1</sup> musí toobe otevřít na žádné příchozí porty hello veřejného Internetu.

<sup>2</sup> Pokud víc portů provedení konfigurace brány, hello odchozí provoz směrovaný pořadí se určí na základě hello port směrování pořadí popsané v [Port směrování](#routing-metric), níže.

<sup>3</sup> pevné IP adresy v zařízení StorSimple hello řadiče musí být směrovatelné a mít tooconnect toohello Internet přímo nebo prostřednictvím hello nakonfigurovat webový proxy server. Hello pevné IP adresy se používají pro obsluhu hello aktualizace toohello zařízení. Pokud řadiče zařízení hello se nemůže připojit toohello Internetu prostřednictvím hello pevné IP adresy, nebudete moct tooupdate zařízení StorSimple.

> [!IMPORTANT]
> Zajistěte, aby brána firewall hello upravit nebo dešifrovat veškerou komunikaci SSL mezi hello zařízení StorSimple a Azure.


### <a name="url-patterns-for-firewall-rules"></a>Adresa URL vzory pro pravidla brány firewall

Správci sítě můžete nakonfigurovat často rozšířené brány firewall pravidla založená na hello URL vzory toofilter hello příchozí a odchozí přenosy hello. Vaše zařízení StorSimple a hello služby StorSimple Manager zařízení závisí na jiných aplikací společnosti Microsoft, například Azure Service Bus, Azure Active Directory řízení přístupu, účty úložiště a servery Microsoft Update. vzorů adresy URL Hello související s těmito aplikacemi se dá použít tooconfigure pravidla brány firewall. Je důležité toounderstand, který můžete změnit vzorů adresy URL hello související s těmito aplikacemi. To zase vyžadují toomonitor správce sítě hello a aktualizovat pravidla brány firewall pro vaše zařízení StorSimple jako a v případě potřeby.

Doporučujeme vám, že nastavíte vašich pravidlech brány firewall pro odchozí přenosy, založené na StorSimple liberally pevné IP adresy, ve většině případů. Ale můžete použít informace hello níže tooset advanced pravidla brány firewall, které jsou potřebné toocreate zabezpečené prostředí.

> [!NOTE]
> zařízení Hello (zdroj) IP adresy musí být vždy nastavená tooall hello povoleno síťových rozhraní. Hello cílové IP adresy by mělo být nastavené příliš[rozsahy IP adres Azure datacenter](https://www.microsoft.com/en-us/download/confirmation.aspx?id=41653).


#### <a name="url-patterns-for-azure-portal"></a>Vzory adres URL pro portál Azure

| Vzor adresy URL | Komponenta nebo funkce | IP adresy zařízení |
| --- | --- | --- |
| `https://*.storsimple.windowsazure.com/*`<br>`https://*.accesscontrol.windows.net/*`<br>`https://*.servicebus.windows.net/*`<br>`https://login.windows.net` |Služba StorSimple Manager zařízení<br>Access Control Service<br>Azure Service Bus<br>Služba ověřování |Povolenou podporu cloudu síťová rozhraní |
| `https://*.backup.windowsazure.com` |Registrace zařízení |Pouze DATA 0 |
| `http://crl.microsoft.com/pki/*`<br>`http://www.microsoft.com/pki/*` |Odvolání certifikátu |Povolenou podporu cloudu síťová rozhraní |
| `https://*.core.windows.net/*` <br>`https://*.data.microsoft.com`<br>`http://*.msftncsi.com` |Monitorování a účty úložiště Azure |Povolenou podporu cloudu síťová rozhraní |
| `http://*.windowsupdate.microsoft.com`<br>`https://*.windowsupdate.microsoft.com`<br>`http://*.update.microsoft.com`<br> `https://*.update.microsoft.com`<br>`http://*.windowsupdate.com`<br>`http://download.microsoft.com`<br>`http://wustat.windows.com`<br>`http://ntservicepack.microsoft.com` |Servery Microsoft Update<br> |Pevné IP adresy řadiče pouze |
| `http://*.deploy.akamaitechnologies.com` |Akamai CDN |Pevné IP adresy řadiče pouze |
| `https://*.partners.extranet.microsoft.com/*`<br>`https://dcupload.microsoft.com/`<br>`https://*.support.microsoft.com/` |Balíček pro podporu |Povolenou podporu cloudu síťová rozhraní |

#### <a name="url-patterns-for-azure-government-portal"></a>Adresa URL vzory pro portál Azure Government.

| Vzor adresy URL | Komponenta nebo funkce | IP adresy zařízení |
| --- | --- | --- |
| `https://*.storsimple.windowsazure.us/*`<br>`https://*.accesscontrol.usgovcloudapi.net/*`<br>`https://*.servicebus.usgovcloudapi.net/*`<br>`https://login-us.microsoftonline.com` |Služba StorSimple Manager zařízení<br>Access Control Service<br>Azure Service Bus<br>Služba ověřování |Povolenou podporu cloudu síťová rozhraní |
| `https://*.backup.windowsazure.us` |Registrace zařízení |Pouze DATA 0 |
| `http://crl.microsoft.com/pki/*`<br>`http://www.microsoft.com/pki/*` |Odvolání certifikátu |Povolenou podporu cloudu síťová rozhraní |
| `https://*.core.usgovcloudapi.net/*` <br>`https://*.data.microsoft.com`<br>`http://*.msftncsi.com` |Monitorování a účty úložiště Azure |Povolenou podporu cloudu síťová rozhraní |
| `http://*.windowsupdate.microsoft.com`<br>`https://*.windowsupdate.microsoft.com`<br>`http://*.update.microsoft.com`<br> `https://*.update.microsoft.com`<br>`http://*.windowsupdate.com`<br>`http://download.microsoft.com`<br>`http://wustat.windows.com`<br>`http://ntservicepack.microsoft.com` |Servery Microsoft Update<br> |Pevné IP adresy řadiče pouze |
| `http://*.deploy.akamaitechnologies.com` |Akamai CDN |Pevné IP adresy řadiče pouze |
| `https://*.partners.extranet.microsoft.com/*`<br>`https://dcupload.microsoft.com/`<br>`https://*.support.microsoft.com/` |Balíček pro podporu |Povolenou podporu cloudu síťová rozhraní |

### <a name="routing-metric"></a>Metrika směrování

Směrování metrika je přidružen hello rozhraní a brána hello, který trasu toohello data hello zadaný sítě. Směrování metrika hello směrovací protokol toocalculate hello nejlepší cesta tooa zadaný cíl, pokud se zjišťuje, že existuje více cest toohello stejný cíl. Hello nižší hello směrování metriky, hello vyšší hello předvoleb.

V hello kontextu zařízení storsimple, pokud jsou víc síťových rozhraní a bran nakonfigurován toochannel provoz, směrování metriky hello se začalo play toodetermine hello relativní pořadí, ve které hello rozhraní bude získat použít. Metrika směrování Hello nelze změnit uživatelem hello. Můžete ale použít hello `Get-HcsRoutingTable` rutiny tooprint hello směrovací tabulky (a metriky) v zařízení StorSimple. Další informace o rutině Get-HcsRoutingTable v [nasazení řešení potíží s StorSimple](storsimple-troubleshoot-deployment.md).

Takto lze vysvětlit Hello směrování metriky algoritmem použitým pro Update 2 nebo novější.

* Sadu předem určený hodnoty byly přiřazeny toonetwork rozhraní.
* Vezměte v úvahu tabulkou aplikace příklad vidíte níže s toohello přiřazených hodnot různých síťových rozhraní. Pokud jsou využívajících cloud nebo cloud zakázaný, ale s nakonfigurované brány. Poznámka: hello hodnoty přiřazené tady jsou pouze ukázkové hodnoty.

    | Síťové rozhraní | Povolenou podporu cloudu | Cloud zakázaný s bránou |
    |-----|---------------|---------------------------|
    | Data 0  | 1            | -                        |
    | Data 1  | 2            | 20                       |
    | Data 2  | 3            | 30                       |
    | Data 3  | 4            | 40                       |
    | Data 4  | 5            | 50                       |
    | Data 5  | 6            | 60                       |


* Hello pořadí, ve kterém budou směrovány přenosy dat cloudové hello přes hello síťových rozhraní je:
  
    *Data 0 > Data 1 > datum 2 > dat 3 > dat 4 > Data 5*
  
    To lze vysvětlit hello následující ukázka.
  
    Vezměte v úvahu zařízení StorSimple se dvěma povolenou podporu cloudu síťovými rozhraními, Data 0 a Data 5. Data 1 až 4 dat jsou cloudu zakázaný, ale mít nakonfigurované brány. Hello pořadí, ve kterém budou směrovány přenosy pro toto zařízení bude:
  
    *Data 0 (1) > Data 5 (6) > Data 1 (20) > Data 2 (30) > dat 3 (40) > dat 4 (50)*
  
    *Hello čísla v závorkách označují hello příslušných směrování metriky.*
  
    Pokud se Data 0 nezdaří, bude získat hello cloudu provoz směruje až Data 5. Vzhledem k tomu, že brána je nakonfigurovaná v jiné síti, pokud Data 0 a Data 5 byly toofail, bude provoz cloudové hello projít Data 1.
* Pokud se nezdaří povolenou podporu cloudu síťové rozhraní, pak jsou 3 opakování s rozhraním 30 toohello druhý tooconnect zpoždění. Pokud selžou všechny hello opakování, provoz hello je směrované toohello další dostupné povolenou podporu cloudu rozhraní určeného hello směrovací tabulky. Pokud všechny hello povolenou podporu cloudu síťových rozhraní nezdaří, pak hello zařízení bude převzetí služeb při selhání toohello jiný řadič (bez restartování v tomto případě).
* Pokud dojde k selhání VIP pro rozhraní sítě iSCSI povolený, budou existovat 3 opakování s zpožděním 2 sekundy. Toto chování se zastaví, hello stejné z předchozích verzí hello. Všechna síťová rozhraní iSCSI hello selžou, převzetí služeb řadiče se provedou (připojí restartování).
* Výstrahu se také vyvolá zařízení StorSimple, když dojde k selhání VIP. Další informace, přejděte příliš[výstrahy Stručná referenční příručka](storsimple-8000-manage-alerts.md).
* Z hlediska opakovaných pokusů, které bude iSCSI mají přednost před cloudu.
  
    Vezměte v úvahu hello následující ukázka: StorSimple A zařízení má dvě rozhraní sítě povolené, Data 0 a Data 1. Rozhraní data 0 má povolenou podporu cloudu vzhledem k tomu Data 1 je i cloudu a podporou iSCSI. Žádné jiné síťová rozhraní na tomto zařízení jsou povolené pro cloud nebo iSCSI.
  
    Pokud Data 1 selže, je zadána hello poslední iSCSI síťové rozhraní, výsledkem bude tooData převzetí služeb při selhání řadič 1 hello na jiný řadič.

### <a name="networking-best-practices"></a>Osvědčené postupy sítě

Kromě toho toohello výše sítě požadavky pro optimální výkon hello vašeho řešení StorSimple prosím splňovat toohello následující osvědčené postupy:

* Zajistěte, aby zařízení StorSimple vyhrazené šířky pásma 40 MB/s (nebo více) vždy k dispozici. Neměl by se sdílet tento šířky pásma (nebo přidělení by mělo být zaručeno prostřednictvím hello použití zásady QoS) s jinými aplikacemi.
* Zajistěte, aby toohello síťové připojení k Internetu je k dispozici za všech okolností. Výskyt občasný, nebo nespolehlivé internetové připojení toohello zařízení, včetně jakkoli, bez připojení k Internetu bude mít za následek nepodporované konfigurace.
* Izolujte přenosy iSCSI a cloudu hello nutnosti mít vyhrazené síťových rozhraní na vašem zařízení pro přístup k iSCSI a cloudu. Další informace najdete v tématu Jak příliš[upravit síťových rozhraní](storsimple-8000-modify-device-config.md#modify-network-interfaces) zařízení StorSimple.
* Nepoužívejte konfiguraci odkaz agregace řízení Protokol LACP () pro síťová rozhraní. Toto je Nepodporovaná konfigurace.

## <a name="high-availability-requirements-for-storsimple"></a>Požadavky na vysokou dostupnost pro StorSimple

Hello hardwarová platforma, která je součástí řešení StorSimple hello obsahuje dostupnost a spolehlivost funkcí, které tvoří základ pro vysoce dostupné a odolné proti chybám úložiště infrastruktury ve vašem datovém centru. Ale existují požadavky a osvědčené postupy, které by měly splňovat toohelp zajištění hello dostupnosti vašeho řešení StorSimple. Před nasazením zařízení StorSimple, pečlivě zkontrolujte hello následující požadavky a osvědčené postupy pro zařízení StorSimple hello a připojené hostitelských počítačích.

Další informace o monitorování a údržbu hello hardwarové součásti zařízení StorSimple, přejděte příliš[použít hello Správce zařízení StorSimple služby toomonitor hardwarové součásti a stav](storsimple-8000-monitor-hardware-status.md) a [ Výměna hardwaru součástí StorSimple](storsimple-8000-hardware-component-replacement.md).

### <a name="high-availability-requirements-and-procedures-for-your-storsimple-device"></a>Požadavky na vysokou dostupnost a postupy pro zařízení StorSimple

Hello zkontrolujte následující informace pečlivě tooensure hello vysokou dostupnost zařízení StorSimple.

#### <a name="pcms"></a>PCMs

Zařízení StorSimple zahrnují za provozu, redundantní napájení a chlazení moduly (PCMs). Každý PCM má dostatek kapacita tooprovide služby pro celé skříně hello. tooensure vysokou dostupnost, oba PCMs musí být nainstalován.

* Připojte vaše PCMs toodifferent power zdroje tooprovide dostupnosti, pokud se nezdaří zdroji napájení.
* Pokud se nezdaří PCM, žádost náhradní okamžitě.
* Odebrat selhání PCM jenom v případě, že máte hello nahrazení a jsou připravené tooinstall ho.
* Současně neodebírejte obou PCMs. modul PCM Hello obsahuje modul zálohování baterie hello. Odebrání i z hello PCMs způsobí vypnutí bez ochrany baterie a stavu zařízení hello se neuloží. Další informace o hello baterie přejděte příliš[zachování hello zálohování baterie modulu](storsimple-8000-battery-replacement.md#maintain-the-backup-battery-module).

#### <a name="controller-modules"></a>Moduly řadiče

Zařízení StorSimple zahrnují moduly řadiče redundantní, za provozu. moduly řadiče Hello pracovat způsobem aktivní nebo pasivní. V každém okamžiku jeden modul řadiče je aktivní a poskytuje služby, při hello jiné řadiče modul je pasivní. modul pasivní řadiče Hello je zapnutý a že lze použít, pokud modul active řadiče hello selže nebo je odebrat. Každý modul řadiče má dostatek kapacita tooprovide služby pro celé skříně hello. Oba řadiče moduly musí být nainstalované tooensure vysokou dostupnost.

* Ověřte, že oba řadiče moduly jsou nainstalovány za všech okolností.
* Pokud modul řadiče selže, žádost náhradní okamžitě.
* Odebrat modul selhání řadiče jenom v případě, že máte hello nahrazení a jsou připravené tooinstall ho. Odebrání modul delší dobu ovlivní hello vzduchu a proto hello chlazení hello systému.
* Ujistěte se, že hello síťové připojení tooboth řadiče moduly jsou identické a hello připojených síťových rozhraní má konfigurace aplikace identických síťových.
* Pokud modul řadiče selže nebo potřebuje nahrazení, ujistěte se, že hello jiné řadiče modul je v aktivním stavu před výměnou hello selhání řadiče modulu. tooverify, zda je řadič aktivní, přejděte příliš[řadič active identifikace hello na vašem zařízení](storsimple-8000-controller-replacement.md#identify-the-active-controller-on-your-device).
* Neodebírejte oba řadiče modulů na hello stejnou dobu. Pokud probíhá selhání řadiče, vypněte hello pohotovostní řadiče modulu nebo ji odeberte z hello skříň.
* Po selhání řadiče počkejte alespoň pět minut před odebráním buď řadiče modulu.

#### <a name="network-interfaces"></a>Síťová rozhraní

StorSimple zařízení řadiče moduly každý mají čtyři 1 Gigabit a 10 dva adaptéry Gigabit Ethernet síťových rozhraní.

* Ujistěte se, že hello síťové připojení tooboth řadiče moduly jsou identické a hello síťová rozhraní, že rozhraní modulu hello řadiče jsou připojené toohave konfigurace aplikace identických síťových.
* Pokud je to možné, nasaďte připojení k síti přes různé přepínače tooensure dostupnost služeb v případě hello selhání zařízení sítě.
* Při odpojování hello pouze nebo hello poslední zbývající rozhraní povolit iSCSI (s IP přiřazené), nejdřív zakázat hello rozhraní a potom odpojte kabely hello. Pokud nejprve je odpojené hello rozhraní, pak může to způsobit toofail hello řadič služby active přes toohello pasivní řadiče. Má-li hello pasivní zařízení také jeho odpovídající rozhraní byl odpojen, pak oba řadiče hello se restartuje více než jednou. před spuštěním na jednom řadiči.
* Připojte alespoň dva síť toohello rozhraní DATA z každého řadiče modulu.
* Pokud jste povolili hello dvě 10 GbE rozhraní, nasaďte těch s jednosměrným různým přepínačům.
* Pokud je to možné, použijte funkci MPIO na servery tooensure, zda text hello servery budou tolerovat odkaz, sítě nebo selhání rozhraní.

Další informace o zařízení pro vysokou dostupnost a výkon sítě, přejděte příliš[instalace zařízení StorSimple 8100](storsimple-8100-hardware-installation.md#cable-your-storsimple-8100-device) nebo [instalace zařízení StorSimple 8600](storsimple-8600-hardware-installation.md#cable-your-storsimple-8600-device).

#### <a name="ssds-and-hdds"></a>SSD a HDD

Zařízení StorSimple obsahovat disky SSD (Solid-State Drive) a pevných disků (HDD) chráněné pomocí zrcadlení prostory. Použití zrcadlených prostorech zajišťuje, že toto zařízení hello je možné tootolerate hello selhání jednotky SSD nebo pevné disky.

* Ujistěte se, zda jsou nainstalovány všechny moduly SSD a HDD.
* Pokud na SSD nebo HDD selže, žádost o nahrazení okamžitě.
* Pokud na SSD nebo HDD selže nebo vyžaduje nahrazení, ujistěte se, odebrat pouze hello SSD nebo pevný disk, který vyžaduje nahrazení.
* Neodebírejte více než jeden SSD a HDD z hello systému v libovolném bodě v čase.
  Selhání 2 nebo více disků určité typu (HDD, SSD) nebo po sobě jdoucích selhání v rámci krátkého času může dojít ke ztrátě dat systému selhání a potenciální. Pokud k tomu dojde, [kontaktovat Microsoft Support](storsimple-8000-contact-microsoft-support.md) o pomoc.
* Při nahrazení, monitorovat hello **sdílené součásti** v hello **stavu hardwaru** okně hello jednotek v hello SSD a HDD. Zelená zkontrolujte stav označuje, že hello disky jsou v pořádku nebo OK zatímco červený vykřičník bodu indikuje selhání SSD nebo pevný disk.
* Doporučujeme nakonfigurovat cloudových snímků pro všechny svazky, je nutné, aby tooprotect v případě selhání systému.

#### <a name="ebod-enclosure"></a>EBOD skříň

Model zařízení StorSimple 8600 obsahuje skříň pro rozšířené Bunch disky (EBOD) v přidání toohello primární skříň. EBOD obsahuje EBOD řadiče a pevných disků (HDD) chráněné pomocí zrcadlení prostory. Použití zrcadlených prostorech zajišťuje, že toto zařízení hello je možné tootolerate hello selhání jedné nebo více pevných disků. Hello EBOD skříň je primární skříň připojené toohello prostřednictvím redundantní SAS kabely.

* Ujistěte se, že oba EBOD skříň řadiče moduly SAS kabely a všechny hello pevné disky jsou nainstalovány za všech okolností.
* Pokud modul EBOD skříň řadiče selže, žádost o nahrazení okamžitě.
* Pokud modul EBOD skříň řadiče selže, ujistěte se, že hello jiné řadiče modulu je aktivní, před nahrazením hello modulu se nezdařilo. tooverify, zda je řadič aktivní, přejděte příliš[řadič active identifikace hello na vašem zařízení](storsimple-8000-controller-replacement.md#identify-the-active-controller-on-your-device).
* Během EBOD řadiče modulu nahrazení, neustále monitorovat stav hello hello součásti v hello služby StorSimple Manager zařízení díky přístupu k **monitorování** > **stavu hardwaru** .
* Pokud je kabel SAS selže nebo vyžaduje nahrazení (Microsoft Support by měl být související se situací toomake toto zjištění), ujistěte se, odebrat pouze kabel SAS hello, která vyžaduje nahrazení.
* Neodebírejte současně oba SAS kabely ze systému hello v libovolném bodě v čase.

### <a name="high-availability-recommendations-for-your-host-computers"></a>Vysoká dostupnost doporučení pro počítače hostitele

Pečlivě zkontrolujte tyto osvědčené postupy tooensure hello vysokou dostupnost zařízení StorSimple připojené tooyour hostitele.

* Konfigurace zařízení StorSimple s [dvěma uzly souborového serveru clusteru konfigurace][1]. Odebráním jediný bod selhání a sestavování v redundance na straně hostitele hello stane vysoce dostupný hello celé řešení.
* Pro zajištění vysoké dostupnosti při převzetí služeb při selhání řadiče úložiště hello použijte nepřetržitě dostupné (CA) dostupné sdílené složky v systému Windows Server 2012 (SMB 3.0). Další informace o konfiguraci clusterů souborových serverů a nepřetržitě dostupné sdílené složky v systému Windows Server 2012, najdete v části toothis [Videoukázka](http://channel9.msdn.com/Events/IT-Camps/IT-Camps-On-Demand-Windows-Server-2012/DEMO-Continuously-Available-File-Shares).

## <a name="next-steps"></a>Další kroky

* [Další informace o omezení systému StorSimple](storsimple-8000-limits.md).
* [Zjistěte, jak toodeploy vašeho řešení StorSimple](storsimple-8000-deployment-walkthrough-u2.md).

<!--Reference links-->
[1]: https://technet.microsoft.com/library/cc731844(v=WS.10).aspx
