---
title: "Přehled řešení řady aaaStorSimple 8000 | Microsoft Docs"
description: "Popisuje vrstvení StorSimple, hello zařízení, virtuální zařízení, služby a správu úložiště a zavádí klíčových termínů používaných v zařízení StorSimple."
services: storsimple
documentationcenter: NA
author: SharS
manager: timlt
editor: 
ms.assetid: 7144d218-db21-4495-88fb-e3b24bbe45d1
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 07/10/2017
ms.author: v-sharos@microsoft.com
ms.openlocfilehash: 0891841186dcd4c46f48d13ed829b98fab7bdf67
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="storsimple-8000-series-a-hybrid-cloud-storage-solution"></a>Řady StorSimple 8000: řešení hybridní cloudové úložiště
## <a name="overview"></a>Přehled
Vítejte tooMicrosoft Azure StorSimple, o řešení integrované úložiště, který spravuje úlohy úložiště mezi místními zařízeními a cloudového úložiště Microsoft Azure. StorSimple je řešení sítě SAN oblasti efektivní, nákladově efektivní a snadno spravovatelné úložiště, které eliminuje řadu hello problémy a náklady spojené s enterprise úložiště a ochranu dat. Používá zařízení řady StorSimple 8000 proprietární hello, integruje s cloudovými službami a poskytuje sadu nástrojů pro správu pro bezproblémové zobrazení všech podnikového úložiště, včetně cloudového úložiště. (informace o nasazení hello StorSimple publikované na webu Microsoft Azure hello platí tooStorSimple 8000 řady jenom zařízení. Pokud používáte zařízení řady StorSimple 5000/7000, přejděte příliš[StorSimple nápovědy](http://onlinehelp.storsimple.com/).)

Používá StorSimple [vrstvení úložiště](#automatic-storage-tiering) toomanage ukládat data v rámci různých úložná média. aktuální pracovní sada Hello je uloženého místně na jednotky SSD (Solid-State Drive), data, která se používá méně často je uložené na pevných disků (HDD) a archivace data odesílána toohello cloudu. Kromě toho StorSimple používá odstranění duplicitních dat a odebírá komprese tooreduce hello velikost úložiště, které hello data. Další informace, přejděte příliš[odstraňování duplicitních dat a komprese](#deduplication-and-compression). Definice jiných klíče podmínky a koncepty používané v dokumentaci řady StorSimple 8000 hello přejděte příliš[StorSimple terminologie](#storsimple-terminology) na konci hello tohoto článku.

Kromě správy toostorage StorSimple funkcím ochrany dat. povolit můžete toocreate na vyžádání a naplánovaných záloh a úložiště, který je místně nebo v hello cloudu. Zálohování se provádějí v podobě hello přírůstkové snímků, což znamená, že může být vytvořen a rychle obnovit. Cloudové snímky může být důležité ve scénářích zotavení po havárii, protože nahradit sekundární úložných systémů (například zálohování na pásku) a povolit tak toorestore data tooyour datacenter nebo tooalternate lokality v případě potřeby.

![Ikona video](./media/storsimple-overview/video_icon.png) Podívejte se na video hello rychlý úvod tooMicrosoft Azure StorSimple.

> [!VIDEO https://channel9.msdn.com/Blogs/Azure/StorSimple-Hybrid-Cloud-Storage-Solution/player]

## <a name="why-use-storsimple"></a>Proč používat StorSimple?
Hello následující tabulka popisuje některé hello klíčové výhody, které poskytuje Microsoft Azure StorSimple.

| Funkce | Výhoda |
| --- | --- |
| Transparentní integrace |Používá hello iSCSI protokol tooinvisibly odkaz datové úložiště zařízení. To zajistí, že data uložená v cloudu hello v datovém centru hello, nebo na vzdálených serverech se zobrazí toobe uložené na jednom místě. |
| Náklady na menší úložiště |Přiděluje dostatečná místní nebo cloudové toomeet aktuální nároky na úložiště a rozšiřuje pouze v případě potřeby úložiště v cloudu. Dále snižuje požadavky na úložiště a náklady odstraněním redundantní verzích hello stejná data (odstranění duplicitních dat) a pomocí komprese. |
| Jednodušší správu úložišť |Poskytuje tooconfigure nástroje pro správu systému a spravovat data uložená místně na vzdáleném serveru a v cloudu hello. Kromě toho můžete spravovat zálohování a obnovení funkce z modul snap-in konzoly Microsoft Management Console (MMC).|
| Zotavení po havárii vylepšené a dodržování předpisů |Nevyžaduje žádný čas rozšířené obnovení. Místo toho obnoví data, protože je potřeba. To znamená, že můžete pokračovat normální provozní podmínky s minimálním dopadem. Kromě toho můžete nakonfigurovat zásady plánů zálohování toospecify a uchovávání dat. |
| Data nastavení mobilních zařízení |Data nahrát tooMicrosoft cloudu Azure, které služby jsou přístupné z jiných webů pro účely obnovení a migrace. Kromě toho můžete StorSimple tooconfigure StorSimple cloudu zařízení na virtuálních počítačích (VM) s v Microsoft Azure. Hello virtuální počítače pak můžete použít virtuální zařízení tooaccess uložená data pro účely testovací nebo obnovení. |
| Kontinuita podnikových procesů |Umožňuje toomigrate uživatelé řady StorSimple 5000 7000 jejich zařízení řady StorSimple 8000 tooa data. |
| Dostupnost v hello portálu Azure Government |StorSimple je k dispozici v hello portálu Azure Government. Další informace najdete v tématu [nasazení místního zařízení StorSimple v hello Government portál](storsimple-8000-deployment-walkthrough-gov-u2.md). |
| Ochrana dat a dostupnost |Hello řady StorSimple 8000 podporuje zóny redundantní úložiště (ZRS), v přidání tooLocally redundantní úložiště (LRS) a geograficky redundantní úložiště (GRS). Odkazovat příliš[Tento článek na možnosti redundance úložiště Azure](https://azure.microsoft.com/documentation/articles/storage-redundancy/) podrobnosti ZRS. |
| Podpora pro kritické aplikace |StorSimple umožňuje identifikovat odpovídajících svazků podle místně vázaný, což zajistí zase, data požadovaných důležitých aplikací nejsou vrstvené toohello cloudu. Místně vázaných svazků nejsou subjektu toocloud latenci nebo problémy s připojením. Další informace o místně vázaných svazků najdete v tématu [použít hello Správce zařízení StorSimple služby toomanage svazky](storsimple-8000-manage-volumes-u2.md). |
| S nízkou latencí a vysoký výkon |Můžete vytvořit cloudu zařízení, které využívá výhod hello vysoký výkon, nízká latence funkce úložiště Azure premium Storage. Další informace o StorSimple premium cloudu zařízení najdete v tématu [nasadit a spravovat o cloudu zařízení StorSimple v Azure](storsimple-8000-cloud-appliance-u2.md). |


## <a name="storsimple-components"></a>Součásti StorSimple
Hello řešení Microsoft Azure StorSimple zahrnuje hello následující součásti:

* **Microsoft Azure StorSimple zařízení** – místní hybridní diskové pole, které obsahuje SSD a HDD, společně s redundantní řadiče a možnosti automatické převzetí služeb při selhání. Hello řadiče spravovat úložiště vrstvení, umístění aktuálně používá (nebo aktivní) data na místní úložiště (v hello zařízení nebo na místní servery), při přesouvání toohello méně často používaných dat v cloudu.
* **Zařízení StorSimple cloudu** – taky známé jako hello virtuální zařízení StorSimple se jedná o verzi softwaru zařízení StorSimple hello, která se replikují hello architektura a většina možností hello fyzické hybridní úložné zařízení. Hello cloudu zařízení StorSimple běží na jednom uzlu ve virtuálním počítači Azure. Virtuální zařízení Premium, které můžete využít Azure premium storage, jsou k dispozici v aktualizaci Update 2 nebo novější.
* **Služba StorSimple Manager zařízení** – na rozšíření hello portálu Azure, který vám umožní spravovat zařízení StorSimple nebo zařízení StorSimple cloudu z jedné webové rozhraní. Můžete použít toocreate služby StorSimple Manager zařízení hello a spravovat služby, zobrazit a spravovat zařízení, Zobrazit výstrahy, správu svazků a zobrazit a spravovat zásady zálohování a zálohování katalog hello.
* **Prostředí Windows PowerShell pro StorSimple** – rozhraní příkazového řádku, které můžete použít toomanage hello zařízení StorSimple. Prostředí Windows PowerShell pro StorSimple je funkce, které vám umožňují tooregister zařízení StorSimple, nakonfigurujte rozhraní sítě hello na vašem zařízení, instalace určité typy aktualizací, řešení potíží s zařízení s přístupem k relaci podpory hello a změnit hello Stav zařízení. Prostředí Windows PowerShell pro StorSimple můžete přistupovat pomocí konzoly sériového portu připojování toohello nebo pomocí vzdálené komunikace Windows Powershellu.
* **Rutiny Azure PowerShell StorSimple** – kolekci rutin prostředí Windows PowerShell, které umožňují tooautomate úrovně služby a úlohy migrace z příkazového řádku hello. Další informace o hello rutin Azure Powershellu pro StorSimple přejděte toohello [reference k rutině](/powershell/module/azure/?view=azuresmps-3.7.0#azure).
* **StorSimple Snapshot Manager** – modul snap-in konzoly MMC, využívající skupiny svazku a zálohování konzistentní s aplikací toogenerate služby Stínová kopie svazku Windows hello. Kromě toho můžete použít StorSimple Snapshot Manager toocreate plánů zálohování a klonování nebo obnovit svazky.
* **Adaptér StorSimple pro službu SharePoint** – nástroj, který transparentně rozšiřuje ochranu Microsoft Azure StorSimple úložiště a dat. tooSharePoint serverové farmy, při vytváření úložišti StorSimple lze zobrazit a spravovat z hello SharePoint – střed Portálu pro správu.

Hello obrázek poskytuje podrobný pohled hello Microsoft Azure StorSimple architektura a komponenty.

![Architektura StorSimple](./media/storsimple-overview/overview-big-picture.png)

Hello následující části popisují každou z těchto součástí podrobněji a popisují, jak řešení hello uspořádá data, přidělí úložiště a usnadňuje správu úložiště a ochranu dat. poslední část Hello obsahuje definice pro některé důležité termíny hello a součásti související tooStorSimple koncepty a jejich správu.

## <a name="storsimple-device"></a>Zařízení StorSimple
Microsoft Azure StorSimple zařízení Hello je místní hybridní diskové pole, které poskytuje primárního úložiště a toodata přístup iSCSI v něm uložená. Spravuje komunikace s cloudovým úložištěm a pomáhá tooensure hello zabezpečení a důvěrnost všechna data uložená na hello řešení Microsoft Azure StorSimple.

zařízení StorSimple Hello zahrnuje SSD a HDD jednotky pevného disku a také podporu pro clustering a automatické převzetí služeb při selhání. Obsahuje sdílené procesor, sdíleného úložiště a dvě zrcadlené řadiče. Každý adaptér poskytuje hello následující:

* Připojení tooa hostitelském počítači
* Až toosix síťové porty tooconnect toohello místní sítě (LAN)
* Monitorování hardwaru
* Paměť s náhodným přístupem non-volatile (paměti NVRAM), která uchovává informace i v případě, že bude přerušen přívod energie
* Clustery aktualizace toomanage aktualizace softwaru na serverech v clusteru s podporou převzetí služeb při selhání tak, aby aktualizace hello minimální nebo žádný vliv na dostupnost služby
* Clusterová služba, která funguje jako back-end clusteru poskytuje vysokou dostupnost a současně minimalizujete její nežádoucí účinky, ke kterým může dojít, pokud na pevný disk nebo SSD selže nebo je uveden do režimu offline

Jenom jeden řadič je aktivní v libovolném bodě v čase. Pokud se řadič active hello nezdaří, bude hello druhého řadiče aktivní automaticky.

Další informace, přejděte příliš[StorSimple hardwarové součásti a stav](storsimple-8000-monitor-hardware-status.md).

## <a name="storsimple-cloud-appliance"></a>StorSimple Cloud Appliance
Můžete vytvořit StorSimple toocreate cloudu zařízení, která replikuje hello architektura a možností hello fyzické hybridní úložné zařízení. Hello cloudu zařízení StorSimple (také označované jako virtuální zařízení StorSimple hello) spouští v jednom uzlu ve virtuálním počítači Azure. (Cloudu zařízení lze vytvořit pouze na virtuální počítač Azure. Není možné ji vytvořit na zařízení StorSimple nebo na místním serveru.)

Hello cloudu zařízení má hello následující funkce:

* Se chová jako fyzické zařízení a nabízejí iSCSI rozhraní toovirtual počítačů v cloudu hello.
* Můžete vytvořit neomezený počet zařízení cloudu v cloudu hello a zapnout je zapnout a vypnout podle potřeby.
* Může pomoci simulaci prostředí místní zotavení po havárii, vývoj a testovací scénáře a může pomoct s načítání na úrovni položek ze zálohy.

Hello cloudu zařízení StorSimple je k dispozici v dva modely: zařízení hello 8010 (dříve označované jako model hello 1100) a hello 8020 zařízení. Hello zařízení 8010 má maximální kapacitu 30 TB. zařízení 8020 Hello, který využívá úložiště Azure premium Storage, má maximální kapacitu 64 TB. (V místních vrstvách Azure premium storage ukládá data na jednotkách SSD zatímco úložiště standard storage ukládá data na HDD.) Všimněte si, že je nutné mít storage úrovně premium účtu toouse Azure premium storage. Další informace o storage úrovně premium, přejděte příliš[úložiště Premium: vysoce výkonné úložiště pro úlohy virtuálních počítačů Azure](../storage/common/storage-premium-storage.md).

Další informace o hello cloudu zařízení StorSimple, přejděte příliš[nasadit a spravovat o cloudu zařízení StorSimple v Azure](storsimple-8000-cloud-appliance-u2.md).

## <a name="storsimple-device-manager-service"></a>Služba StorSimple Manager zařízení
Microsoft Azure StorSimple toocentrally poskytuje webové uživatelské rozhraní (hello služby StorSimple Manager zařízení), která vám umožní spravovat datacenter a cloudového úložiště. Můžete použít hello Správce zařízení StorSimple služby tooperform hello následující úlohy:

* Nakonfigurujte nastavení systému pro zařízení StorSimple.
* Konfigurovat a spravovat nastavení zabezpečení pro zařízení StorSimple.
* Nakonfigurujte pověření cloudu a vlastnosti.
* Konfigurovat a spravovat svazky na serveru.
* Nakonfigurujte skupiny svazku.
* Zálohování a obnovení dat.
* Sledování výkonu.
* Zkontrolujte nastavení systému a identifikovat možné problémy.

Můžete vytvořit tooperform služby StorSimple Manager zařízení hello všechny úkoly správy kromě těch, které vyžadují systém výpadek, jako je počáteční nastavení a instalace aktualizací.

Další informace, přejděte příliš[použití hello tooadminister service Manager zařízení StorSimple zařízení StorSimple](storsimple-8000-manager-service-administration.md).

## <a name="windows-powershell-for-storsimple"></a>Prostředí Windows PowerShell pro StorSimple
Prostředí Windows PowerShell pro StorSimple poskytuje rozhraní příkazového řádku, můžete použít toocreate a spravovat službu Microsoft Azure StorSimple hello a nastavit a monitorování zařízení StorSimple. Je založených na prostředí Windows PowerShell, rozhraní příkazového řádku, která zahrnuje vyhrazené rutiny pro správu zařízení StorSimple. Prostředí Windows PowerShell pro StorSimple je funkce, které vám umožní:

* Registrovat zařízení.
* Nakonfigurujte rozhraní sítě hello na zařízení.
* Nainstalujte určité typy aktualizací.
* Řešení potíží s zařízení s přístupem k relaci podpory hello.
* Změnit stav zařízení hello.

Prostředí Windows PowerShell pro StorSimple můžete otevřít z konzoly sériového portu (na hostiteli počítače přímo připojené zařízení toohello) nebo vzdáleně přes vzdálenou komunikaci prostředí Windows PowerShell. Všimněte si, že některé Windows Powershellu pro StorSimple úkoly, jako je registrace počáteční zařízení, můžete provést pouze na hello konzoly sériového portu.

Další informace, přejděte příliš[pomocí Windows Powershellu pro StorSimple tooadminister zařízení](storsimple-8000-windows-powershell-administration.md).

## <a name="azure-powershell-storsimple-cmdlets"></a>Rutiny Azure PowerShell StorSimple
jsou rutiny Azure PowerShell StorSimple Hello kolekci rutin prostředí Windows PowerShell, které umožní tooautomate úrovně služeb a úloh migrace z příkazového řádku hello. Další informace o hello rutin Azure Powershellu pro StorSimple přejděte toohello [reference k rutině](/powershell/module/azure/?view=azuresmps-3.7.0).

## <a name="storsimple-snapshot-manager"></a>StorSimple Snapshot Manager
Snapshot Manager zařízení StorSimple je modul snap-in konzoly Microsoft Management Console (MMC), které můžete použít toocreate konzistentní, v okamžiku záložní kopie místní a Cloudová data. modul snap-in Hello běží na hostiteli se systémem Windows Server. Můžete použít StorSimple Snapshot Manager do:

* Konfigurace, zálohovat a odstraňte svazky.
* Konfigurace svazku skupiny tooensure zálohovaných dat je konzistentní s aplikací.
* Spravovat zásady zálohování tak, aby se data zálohují na předem určeného plánu a uložené v určené umístění (místně nebo v cloudu hello).
* Obnovení svazků a jednotlivé soubory.

Zálohování se zaznamená jako snímky, které záznam pouze hello změny, protože byl proveden poslední snímek hello a vyžadovat mnohem méně místa než úplné zálohy. Můžete vytvořit plánů zálohování nebo provést okamžitou zálohování podle potřeby. Kromě toho můžete použít zásady uchovávání informací tooestablish Snapshot Manager zařízení StorSimple, řídíte, kolik snímků se uloží. Pokud je následně nutné toorestore data ze zálohy, umožňuje Snapshot Manager zařízení StorSimple, vyberte z katalogu hello místní nebo cloudové snímky. 

Pokud dojde k havárii, nebo pokud potřebujete toorestore data z jiného důvodu, StorSimple Snapshot Manager obnoví se postupně dle potřeby. Obnovení dat nevyžaduje při obnovení souboru, nahraďte zařízení nebo přesunutí operations tooanother webu vypnout hello celý systém.

Další informace, přejděte příliš[co je StorSimple Snapshot Manager?](storsimple-what-is-snapshot-manager.md)

## <a name="storsimple-adapter-for-sharepoint"></a>StorSimple Adapter pro SharePoint
Microsoft Azure StorSimple zahrnuje hello adaptér StorSimple pro službu SharePoint, volitelná komponenta, která StorSimple úložiště a ochranu dat transparentně rozšiřuje funkce tooSharePoint serverové farmy. adaptér Hello pracuje s zprostředkovatele vzdáleného úložiště objektů Blob (RBS) a funkce SQL Server RBS hello, což umožňuje vám toomove objekty BLOB tooa serveru zálohovat systém Microsoft Azure StorSimple hello. Microsoft Azure StorSimple uloží data objektu BLOB hello místně nebo v cloudu hello na základě využití.

Hello adaptér StorSimple pro službu SharePoint je spravovat z portálu hello Centrální správa SharePoint. V důsledku toho zůstává Centrální správa služby SharePoint, a všechny úložiště zobrazuje toobe ve farmě služby SharePoint hello.

Další informace, přejděte příliš[StorSimple adaptéru pro službu SharePoint](storsimple-adapter-for-sharepoint.md). 

## <a name="storage-management-technologies"></a>Technologie správy úložiště
Kromě vyhrazené toohello zařízení StorSimple, virtuální zařízení a další součásti, Microsoft Azure StorSimple používá hello následující software technologie tooprovide rychlý přístup toodata a tooreduce spotřeba úložiště:

* [Automatické úložiště vrstvení](#automatic-storage-tiering) 
* [Dynamické zajišťování](#thin-provisioning) 
* [Odstranění duplicitních dat a komprese](#deduplication-and-compression) 

### <a name="automatic-storage-tiering"></a>Automatické úložiště vrstvení
Microsoft Azure StorSimple automaticky uspořádá data v logické vrstvy na základě aktuálního využití, stáří a data tooother vztahu. Data, která je většina aktivní uložené lokálně, při nižší aktivní, neaktivní data se automaticky migrovat toohello cloudu. Hello následující diagram znázorňuje tento přístup úložiště.

![Vrstvy úložiště StorSimple](./media/storsimple-overview/hcs-data-services-storsimple-components-tiers.png)

Rychlý přístup tooenable, StorSimple ukládá velmi aktivní data (aktivní data) na jednotkách SSD v zařízení StorSimple hello. Ukládá data, která se někdy používá (tedy dat) na HDD hello zařízení nebo na serverech v datovém centru hello. Přesune neaktivní data zálohovaná data a data uchovávat archivace nebo dodržování předpisů účely toohello cloudu. 

> [!NOTE]
> V aktualizaci Update 2 nebo novější můžete zadat jako místně vázaný svazek, v takovém případě hello data zůstane na místním zařízení hello a není vrstvené toohello cloudu. 


StorSimple upraví a změní uspořádání dat a změnit přiřazení úložiště jako vzorce používání. Některé informace mohou být například míň aktivní v čase. Jakmile je k progresivnímu míň aktivní, proběhne jeho migrace z tooHDDs SSD a potom toohello cloudu. Pokud tato data zase aktivní, je migrované back toohello úložné zařízení.

proces vrstvení úložiště Hello dojde k následujícím způsobem:

1. Nastaví správce systému účet cloudového úložiště Microsoft Azure.
2. Správce Hello používá hello sériové konzoly a hello Správce zařízení StorSimple služby (spuštěna v hello portál Azure) tooconfigure hello zařízení a soubor server, vytvářet svazky a data zásady ochrany. Místní počítače (například souborové servery) používat zařízení StorSimple hello Internet Small Computer System Interface (iSCSI) tooaccess hello.
3. Na začátku StorSimple ukládá data na hello rychlé vrstvy SSD hello zařízení.
4. Jako hello přístupy kapacity vrstvy SSD StorSimple deduplicates a komprimaci hello nejstarší datové bloky a přesunou vrstvy HDD toohello.
5. Jako hello kapacity přístupy vrstvy HDD StorSimple šifruje datové bloky nejstarší hello a odešle je bezpečně toohello účtu úložiště Microsoft Azure prostřednictvím protokolu HTTPS.
6. Microsoft Azure vytvoří víc replik hello dat svého datového centra a ve vzdáleném datacentru, zajistíte, že lze obnovit hello data, pokud dojde k havárii.
7. Když souborový server hello vyžádá data uložená v cloudu hello, StorSimple vrátí ji bezproblémově a ukládá kopie ve vrstvě SSD hello hello zařízení StorSimple.

#### <a name="how-storsimple-manages-cloud-data"></a>Jak StorSimple spravuje data v cloudu

StorSimple deduplicates zákaznické údaje ve všech hello snímky a hello primární daty (data zapsána hostitelé). Při odstraňování duplicitních dat je skvělá pro efektivity úložiště, vytvoří hello otázku "co je v cloudu hello" komplikovanější. Hello vrstvené primární data a data snímku hello překrývat mezi sebou. Jeden bloků dat v cloudu hello by se použil jako vrstvené primární data a také označit několik snímků. Každý cloudový snímek zajistí, že kopii všechna data v daném okamžiku hello je uzamčen do cloudu hello, dokud nebude tento snímek se odstraní.

Data se odstraní pouze z cloudu hello, pokud nejsou žádná data toothat odkazy. Například pokud jsme cloudový snímek všechna data hello, který je v zařízení StorSimple hello a pak odstraňte některá primární data, by vidíme hello _primární data_ vyřadit okamžitě. Hello _Cloudová data_ což zahrnuje hello vrstvené dat a hello zálohy, zůstane hello stejné. Je to proto, že je stále odkazující na data v cloudu hello snímku. Po hello cloudu je odstranění snímku (a další snímek, který odkazuje hello stejná data), cloudové vyřazuje spotřeby. Před jsme odebrat data v cloudu, zkontrolujte jsme, že žádné snímky stále referenční data. Tento proces se nazývá _uvolňování paměti_ a na zařízení hello je spuštěna služba na pozadí. Odebrání data v cloudu není okamžitý, protože služba kolekce paměti hello kontroluje dalších odkazů na toothat data před odstraněním hello. rychlost Hello uvolňování paměti závisí na hello celkový počet snímků a celková data hello. Data v cloudu hello je obvykle vyčištěna za méně než týden.


### <a name="thin-provisioning"></a>Dynamické zajišťování
Dynamické zajišťování je technologie virtualizace, ve kterém úložiště k dispozici, zobrazí se tooexceed fyzické prostředky. Místo předem rezervování dostatečné úložiště, StorSimple používá dynamického zřizování tooallocate akorát toomeet aktuální požadavky na volné místo. Hello elastické povaha cloudového úložiště usnadňuje tento přístup, protože StorSimple můžete zvýšit nebo snížit cloudové úložiště toomeet splněné měnící se požadavky.

> [!NOTE]
> Místně vázaných svazků nezřídil dynamicky. Úložiště přidělené tooa pouze místní svazek je zajištěna v celé jeho šíři při vytváření svazku hello.


### <a name="deduplication-and-compression"></a>Odstranění duplicitních dat a komprese
Odstranění duplicitních dat používá Microsoft Azure StorSimple a toofurther komprese dat snížit požadavky na úložiště.

Odstranění duplicitních dat snižuje hello celkové množství dat uložených odstraněním redundance v sadě dat hello uložené. Informace o změní, StorSimple ignoruje data hello beze změny a zachycení pouze hello změny. Kromě toho StorSimple snižuje hello množství uložených dat identifikace a odebráním nepotřebných informace. 

> [!NOTE]
> Není data na místně vázaných svazků s odstraněním duplicitních dat nebo komprimován. Však jsou zálohy místně vázaných svazků s odstraněním duplicitních dat a komprimované.


## <a name="storsimple-workload-summary"></a>Souhrn úloh StorSimple
Souhrn úlohy StorSimple hello podporované v následující tabulce.

| Scénář | Úloha | Podporuje se | Omezení | Verze |
| --- | --- | --- | --- | --- |
| Spolupráce |Sdílení souborů |Ano | |Všechny verze |
| Spolupráce |Sdílení souborů DFS |Ano | |Všechny verze |
| Spolupráce |SharePoint |Ano* |Podporuje jenom s místně vázaných svazků |Update 2 nebo novější |
| Archivace |Archivace jednoduchého souboru |Ano | |Všechny verze |
| Virtualizace |Virtuální počítače |Ano* |Podporuje jenom s místně vázaných svazků |Update 2 nebo novější |
| Databáze |SQL |Ano* |Podporuje jenom s místně vázaných svazků |Update 2 nebo novější |
| Video sledováním. |Video sledováním. |Ano* |Podporované, když zařízení StorSimple je vyhrazené jenom toothis zatížení |Update 2 nebo novější |
| Zálohování |Primární cíl zálohování |Ano* |Podporované, když zařízení StorSimple je vyhrazené jenom toothis zatížení |Aktualizace 3 nebo novější |
| Zálohování |Sekundární cíl zálohování |Ano* |Podporované, když zařízení StorSimple je vyhrazené jenom toothis zatížení |Aktualizace 3 nebo novější |

*Ano &#42; -Řešení pokyny a omezení bude použito.*

Hello následující úlohy nejsou podporovány řadu zařízení StorSimple 8000. Pokud se nasadí na zařízení StorSimple, povede tato zatížení nepodporované konfigurace.

* Lékařské vytvoření bitové kopie
* Výměna
* INFRASTRUKTURY VIRTUÁLNÍCH KLIENTSKÝCH POČÍTAČŮ
* Oracle
* SAP
* Velký objem dat
* Distribuce obsahu
* Spuštění z rozhraní SCSI

Následuje seznam součástí infrastruktury hello StorSimple podporována.

| Scénář | Úloha | Podporuje se | Omezení | Verze |
| --- | --- | --- | --- | --- |
| Obecné |ExpressRoute |Ano | |Všechny verze |
| Obecné |DataCore FC |Ano* |Podporované s DataCore SANsymphony |Všechny verze |
| Obecné |DFSR |Ano* |Podporuje jenom s místně vázaných svazků |Všechny verze |
| Obecné |Indexování |Ano* |Pro vrstvené svazky, je podporováno pouze metadata indexování (žádná data).<br>Pro místně vázaných svazků je podporováno dokončení indexování. |Všechny verze |
| Obecné |Ochrana proti virům |Ano* |Pro vrstvené svazky je podporováno pouze kontroly při otevření a zavřete.<br> Úplné prohledávání místně vázaných svazků, je podporováno. |Všechny verze |

*Ano &#42; -Řešení pokyny a omezení bude použito.*

Následuje seznam dalšího softwaru, které se používají s řešeními toobuild StorSimple.

| Typ úlohy | Software použít s StorSimple | Podporované verze|Průvodce toosolution odkaz| 
| --- | --- | --- | --- |
| Cíl zálohy |Veeam |V Veeam 9 a novějším |[StorSimple jako cíl zálohování s Veaam](storsimple-configure-backup-target-veeam.md)|
| Cíl zálohy |Exec – zálohování této společnosti |Zálohování Exec 16 a novější |[StorSimple jako cíl zálohování pomocí zálohování Exec](storsimple-configure-backup-target-using-backup-exec.md)|
| Cíl zálohy |NetBackup této společnosti |NetBackup 7.7.x a novější  |[StorSimple jako cíl zálohování s NetBackup](storsimple-configure-backuptarget-netbackup.md)|
| Sdílení souborů globální <br></br> Spolupráce |Talon  |[StorSimple s Talon](https://www.talonstorage.com/products/fast-deployment-azure-storsimple) | |

## <a name="storsimple-terminology"></a>Terminologie služby StorSimple
Před nasazením řešení Microsoft Azure StorSimple, doporučujeme, abyste si prošli následující hello termíny a definice.

### <a name="key-terms-and-definitions"></a>Klíče termíny a definice
| Termín (zkratka nebo zkratka) | Popis |
| --- | --- |
| záznam řízení přístupu (ACR) |Záznam přidružený svazek v zařízení Microsoft Azure StorSimple, která určuje, které hostitele může připojit tooit. určení Hello je založeno na hello iSCSI kvalifikovaný název (IQN) hostitele hello (obsažené v hello ACR), kteří se připojují tooyour zařízení StorSimple. |
| AES 256 |Algoritmus Advanced Encryption (Standard AES) 256 bitů pro šifrování dat při jejich přesunu tooand z cloudu hello. |
| velikost alokační jednotky (Austrálie) |Hello nejmenší velikost místa na disku, který může být přidělen toohold soubor v vaše systémy souborů Windows. Pokud velikost souboru není násobkem hello velikost clusteru, místo navíc musí být soubor hello použité toohold (až toohello další násobkem velikost clusteru hello) což vede ke ztrátě místa a fragmentace hello pevného disku. <br>Hello doporučuje Austrálie pro svazky Azure StorSimple je 64 KB, protože ho pracuje s algoritmy pro odstranění duplicit hello. |
| automatizované úložiště vrstvení |Automaticky přesunutí míň aktivní data z tooHDDs SSD a potom tooa vrstvy hello cloudu a pak povolení správy všech úložiště z centrální uživatelské rozhraní. |
| Zálohování katalogu |Kolekce zálohování, které jsou obvykle spojené hello typu aplikace, která byla použita. Tato kolekce se zobrazí v hello katalog zálohování okno hello služby StorSimple Manager zařízení uživatelského rozhraní. |
| soubor zálohy katalogu |Soubor obsahující seznam dostupných snímky, které jsou aktuálně uloženy ve hello zálohování databáze služby StorSimple Snapshot Manager. |
| zásady zálohování |Výběr svazků, typ zálohy a časový plán, který vám umožní toocreate zálohování podle předdefinovaného plánu. |
| binární rozsáhlé objekty (objekty BLOB) |Kolekce binární data uložená jako jedna entita v systému pro správu databáze. Objekty BLOB jsou obvykle bitové kopie, zvuk nebo jiné multimédií objekty, i když někdy binární kód spustitelný soubor je uložen jako objekt BLOB. |
| Challenge Handshake Authentication Protocol (CHAP) |Protokol použít tooauthenticate hello sdílené připojení k podle hello sdílené sdílení hesla a tajný klíč. CHAP mohou být jednosměrné nebo vzájemné. S jednosměrný CHAP ověřuje hello cíl iniciátor. Vzájemné CHAP vyžaduje, aby hello cíl ověření hello iniciátor a že iniciátor hello ověřuje hello cíl. |
| klonování |Duplicitní kopie svazku. |
| Cloud jako vrstvu (CaaT) |Cloudového úložiště, které jsou integrovány jako vrstvy v rámci hello architektura úložiště tak, aby všechny úložiště zobrazuje toobe součástí sítě úložiště jednoho podniku. |
| poskytovatele cloudové služby (CSP) |Zprostředkovatel cloud computing služby. |
| cloudový snímek |V okamžiku kopii svazku data, která je uložená v cloudu hello. Cloudový snímek je ekvivalentní tooa snímku replikují na jiný, odlehlého úložiště systému. Cloudové snímky jsou obzvláště užitečná ve scénářích zotavení po havárii. |
| Šifrovací klíč cloudového úložiště |Heslo nebo klíč používá vaše zařízení StorSimple zařízení tooaccess hello zašifrovaná data odeslaná vaše zařízení toohello cloud. |
| aktualizace pro clustery |Správa aktualizací softwaru na serverech v clusteru s podporou převzetí služeb při selhání tak, aby aktualizace hello minimální nebo žádný vliv na dostupnost služeb. |
| DataPath |Kolekce funkční jednotek, které provádějí operace propojených zpracování dat. |
| Deaktivace |Trvalé, zalomení hello připojení mezi hello zařízení StorSimple a hello související cloudové služby. Cloudové snímky hello zařízení zůstaly po tento proces a může být klonovat nebo použít pro zotavení po havárii. |
| zrcadlení disku |Replikace pro logické diskové svazky na samostatné pevné disky v reálném čase tooensure trvalou dostupnost. |
| zrcadlení dynamický disk |Replikace pro logické diskové svazky na dynamické disky. |
| dynamické disky |Svazek formát disku, používá hello toostore Správce logických disků (LDM) a spravovat data mezi několik fyzických disků. Dynamické disky může být zvětšeným tooprovide více volného místa. |
| Rozšířené skříň Bunch disky (EBOD) |Sekundární skříň vašeho zařízení Microsoft Azure StorSimple, který obsahuje disky velmi pevného disku pro další úložiště. |
| FAT zřizování |Konvenční zajišťování úložiště v úložišti, které je přiděleno místo na základě očekává potřebám (a je obvykle mimo aktuální potřebě hello). Viz také *dynamické zajišťování*. |
| jednotku pevného disku (HDD) |Jednotku, která používá rotační několik ploten toostore data. |
| hybridní cloudové úložiště |Architektura úložiště, který používá místní a odlehlého prostředků, včetně cloudového úložiště. |
| Internet Small Computer System Interface (iSCSI) |Úložišť založených na protokolu IP síťového standard pro propojení zařízení úložiště dat nebo zařízení. |
| Iniciátor iSCSI |Součástí softwaru, která umožňuje hostitelský počítač se systémem Windows tooconnect tooan externího úložiště iSCSI prostřednictvím sítě. |
| iSCSI kvalifikovaný název IQN () |Jedinečný název, který identifikuje cíle iSCSI nebo iniciátor. |
| cíl iSCSI |Softwarová součást, která poskytuje centralizovanou iSCSI diskových podsystémů v SAN. |
| Live archivováním |O přístupu úložiště, ve kterém archivaci dat je přístupný všechny hello čas (nebude uložen mimo server na pásku, třeba). Microsoft Azure StorSimple používá archivace za provozu. |
| místně vázaný svazek |svazek, který se nachází na hello zařízení a to se nikdy vrstvené toohello cloudu. |
| místní snímek |V okamžiku kopii svazku data uložená na zařízení hello Microsoft Azure StorSimple. |
| Microsoft Azure StorSimple |Výkonné řešení skládající se z datacenter úložiště zařízení a softwaru, který umožňuje IT organizacím tooleverage cloudového úložiště, jako by šlo úložišti datacentra. StorSimple zjednodušuje ochrany dat a správu dat při současném snížení nákladů. řešení Hello konsoliduje primární úložiště, archivace, zálohování a zotavení po havárii (DR) prostřednictvím bezproblémovou integraci s hello cloudu. Kombinací SAN úložiště a cloudové správy dat na platformě podnikových zařízení StorSimple povolit rychlosti, jednoduchost a spolehlivost pro všechny požadavky související s úložištěm. |
| Napájení a chlazení modulu (PCM) |Hardwarové součásti zařízení StorSimple, který se skládá z hello napájení a chladicí ventilátory, hello proto hello název energii a chlazení modulu. primární skříň Hello hello zařízení má dva PCMs 764W, zatímco hello EBOD skříň má dva PCMs 580W. |
| Primární skříň |Hlavní skříň zařízení StorSimple, který obsahuje hello aplikace platformy řadiče. |
| Cíli času obnovení (RTO) |maximální množství času, které by měl být použito před obchodní proces nebo systém Hello úplné obnovení po havárii. |
| sériově připojené SCSI (SAS) |Typ jednotky pevného disku (HDD). |
| šifrovací klíč dat služby |Klíč provedené tooany k dispozici nové zařízení StorSimple, zaregistruje se hello služby StorSimple Manager zařízení. Hello konfiguračních dat přenesených mezi hello zařízení a služby StorSimple Manager zařízení hello je zašifrován pomocí veřejného klíče a pak mohou ho dešifrovat jenom na zařízení hello pomocí soukromého klíče. Šifrovací klíč dat služby umožňuje hello služby tooobtain tento soukromý klíč pro dešifrování. |
| Registrační klíč služby |Klíč, který pomáhá zařízení StorSimple hello se hello služby StorSimple Manager zařízení zaregistrovat, aby se zobrazuje v hello portál Azure pro další akce správy. |
| Small Computer System Interface (SCSI) |Sada standardů pro fyzicky propojení počítačů a předávání dat mezi nimi. |
| jednotky SSD (SSD) |Disk, který neobsahuje žádné přesunutí části; například flash disk. |
| Účet úložiště |Sadu pověření pro přístup k propojený účet úložiště tooyour pro daný cloud poskytovatele služeb. |
| StorSimple Adapter pro SharePoint |Komponenty Microsoft Azure StorSimple, která rozšiřuje transparentně StorSimple úložiště a ochranu dat tooSharePoint serverové farmy. |
| Služba StorSimple Manager zařízení |Rozšíření hello portálu Azure, který vám umožní toomanage Azure StorSimple místní a virtuální zařízení. |
| StorSimple Snapshot Manager |Modul Microsoft Management Console (MMC) snap-in pro správu operace zálohování a obnovení v Microsoft Azure StorSimple. |
| proveďte zálohování |Funkce, která umožňuje hello uživatele tootake interaktivní zálohu svazku. Jedná se o alternativní způsob pořízení ručního zálohování svazku jako názvem na rozdíl od tootaking automatizované zálohování prostřednictvím definovanou zásadu. |
| Dynamické zajišťování |Metoda optimalizace hello efektivitu, pomocí které hello je prostor úložiště k dispozici používaných v systémech úložiště. V dynamickém zřizování se hello úložiště je rozdělena mezi více uživatelů, které jsou založené na minimální místo hello požadované každý uživatel, v každém okamžiku. Viz také *fat zřizování*. |
| Vrstvení |Uspořádání dat v logické seskupení podle aktuálního využití, stáří a data tooother vztahu. StorSimple automaticky uspořádá data na úrovních. |
| Svazek |Oblasti logické úložiště uvedené v podobě hello jednotek. Svazky zařízení StorSimple odpovídají toohello svazky připojené hello hostitele, včetně těch, které zjištěný prostřednictvím použití hello iSCSI a zařízení StorSimple. |
| kontejner svazků |Seskupení svazky a hello nastavení, které se vztahují toothem. Všechny svazky v zařízení StorSimple se seskupují do kontejnery svazků. Nastavení kontejneru svazku obsahovat účty úložiště, nastavení šifrování pro data odesílána toocloud s přidružených šifrovacích klíčů a šířky pásma využité pro operací zahrnujících hello cloudu. |
| svazek skupiny |V zařízení StorSimple Snapshot Manager skupinu svazek je kolekce zálohování zpracování toofacilitate svazky, které jsou nakonfigurované. |
| Služby Stínová kopie svazku (VSS) |Služba operačního systému Windows Server, která usnadňuje konzistence aplikací tím, že komunikaci s toocoordinate aplikace používající stínovou kopii svazku hello vytváření přírůstkové snímků. Služby Stínová kopie svazku zajišťuje, aby byly aplikace hello dočasně neaktivní při pořizování snímků. |
| Prostředí Windows PowerShell pro StorSimple |Rozhraní příkazového řádku založených na prostředí Windows PowerShell používá toooperate a správě zařízení StorSimple. Toto rozhraní při zachování některé základní možnosti hello prostředí Windows PowerShell, má další vyhrazený rutin, které jsou s ohledem na správu zařízení StorSimple. |

## <a name="next-steps"></a>Další kroky
Další informace o [zabezpečení zařízení StorSimple](storsimple-8000-security.md).

