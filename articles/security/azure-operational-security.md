---
title: "aaaAzure provozního zabezpečení | Microsoft Docs"
description: "Další informace o Microsoft Operations Management Suite (OMS), jeho služby a jak to funguje."
services: security
documentationcenter: na
author: UnifyCloud
manager: swadhwa
editor: TomSh
ms.assetid: 
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 04/27/2017
ms.author: TomSh
ms.openlocfilehash: 85e0c74314ed97a53d395b209e348b779a5d14c8
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-operational-security"></a>Provozní zabezpečení Azure
## <a name="introduction"></a>Úvod

### <a name="overview"></a>Přehled
Víme, že je zabezpečení úlohy, jeden v cloudu hello a jak důležité je, že zjistíte přesné a aktuální informace o zabezpečení Azure. Jedním z hello nejlepší toouse důvodů Azure pro vaše aplikace a služby je tootake výhod hello širokou škálu zabezpečení nástroje a možnosti, které jsou k dispozici. Tyto nástroje a možnosti pomáhat při jeho možné toocreate zabezpečené řešení na platformu Azure zabezpečené hello. Windows Azure, musíte zadat utajení, integrita a dostupnost dat zákazníka, a zároveň umožnit transparentní odpovědnosti za.

Zákazníci toohelp lépe pochopit hello pole ovládacích prvků zabezpečení, které jsou implementované v rámci Microsoft Azure z obou hello zákazníka a Microsoft provozní perspektivy, tento dokument white paper, "Provozní zabezpečení Azure", je zapsán, který poskytuje komplexní pohled na hello provozního zabezpečení systému Windows Azure k dispozici.

### <a name="azure-platform"></a>Platformy Azure
Azure je platforma služby veřejného cloudu, která podporuje široký výběr operačních systémů, programovací jazyky, rozhraní, nástroje, databází a zařízení. Může probíhat Linux kontejnery s integrace Dockeru; vývoj aplikací pomocí jazyka JavaScript, Python, .NET, PHP, Java a Node.js; sestavení back EndY pro iOS, Android a Windows zařízení. Cloudovou službu Azure podporuje hello stejné technologie miliony vývojářů a IT profesionály už spoléhají na a vztah důvěryhodnosti.

Pokud sestavení nebo IT prostředky se mají migrovat, poskytovatele služeb veřejného cloudu se spoléhat na dané organizace dalo tooprotect vaší aplikace a data se službami hello a ovládací prvky hello poskytují toomanage hello zabezpečení vašeho cloudu prostředky.

Infrastruktury Azure a je určená z hello tooapplications zařízení pro hostování miliony zákazníků současně, a poskytuje trustworthy foundation, na kterém může podnikům splňovat požadavky jejich zabezpečení. Kromě toho Azure poskytuje širokou škálu toocontrol možnost konfigurovat zabezpečení možnosti a hello je tak, aby si můžete přizpůsobit toomeet hello jedinečné požadavky na zabezpečení vaší organizace nasazení. Tento dokument se pomůže pochopit, jak Azure zabezpečení funkce vám může pomoct splnění těchto požadavků.

### <a name="abstract"></a>Abstraktní
Zabezpečení provozu Azure odkazuje toohello služeb, ovládací prvky a funkce dostupné toousers pro ochranu svá data, aplikace a dalších prostředků ve službě Microsoft Azure. Zabezpečení provozu Azure je založený na rozhraní, které zahrnuje hello poznatky získané při různých možnostech, které jsou jedinečné tooMicrosoft, včetně hello Microsoft SDL Security Development Lifecycle (), hello Microsoft Security Response Center program a hloubkové povědomí o šířku threat počítačové bezpečnosti hello.

Tento dokument popisuje společnosti Microsoft přístup tooAzure provozního zabezpečení v rámci Cloudová platforma Microsoft Azure hello a zahrnuje následující služby:
1.  [Azure Operations Management Suite](https://docs.microsoft.com/azure/operations-management-suite/operations-management-suite-overview)

2.  [Azure Security Center](https://docs.microsoft.com/azure/security-center/security-center-intro)

3.  [Azure Monitor](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview)

4.  [Azure sledovací proces sítě](https://docs.microsoft.com/azure/network-watcher/network-watcher-monitoring-overview)

5.  [Azure Storage analytics](https://docs.microsoft.com/rest/api/storageservices/fileservices/storage-analytics)

6.  [Azure Active directory](https://docs.microsoft.com/azure/active-directory/active-directory-whatis)


## <a name="microsoft-operations-management-suite"></a>Microsoft Operations Management Suite

Microsoft Operations Management Suite (OMS) je hello řešení pro správu IT pro hello hybridní cloud. Použít samostatně nebo tooextend, které vám vaše stávající nasazení produktu System Center, OMS hello maximální flexibilitu a řízení pro správu cloudové infrastruktury.

![Microsoft Operations Management Suite](./media/azure-operational-security/azure-operational-security-fig1.png)

S OMS můžete spravovat libovolnou instancí ve všech cloudu, včetně místní, Azure, AWS, Windows Server, Linux, VMware a OpenStack, při nižších nákladech, než konkurenční řešení. Vytvořené pro první cloudu hello, world, OMS nabízí nový způsob toomanaging při vaší organizace, která je hello nejrychlejší, nákladově nejefektivnější způsob nové obchodní toomeet vyzve a zohlednit nové úlohy, aplikace a cloudové prostředí.

### <a name="oms-services"></a>Služby OMS

Hello základní funkce služby OMS poskytuje sadu služby, které běží v Azure. Každá služba poskytuje funkce správy specifických a můžete kombinovat scénářů tooachieve různých správy služeb.

| Služba  | Popis|
| :------------- | :-------------|
| Log Analytics | Monitorovat a analyzovat hello dostupnosti a výkonu různých prostředků, včetně fyzických a virtuálních počítačů. |
|Automation | Automatizace ručních procesů a vynucení konfigurací pro fyzické a virtuální počítače |
| Zálohování | Zálohování a obnovení důležitá data. |
| Site Recovery | Poskytnutí vysoké dostupnosti pro důležitá data |

### <a name="log-analytics"></a>Log Analytics

[Log Analytics](http://azure.microsoft.com/documentation/services/log-analytics) poskytuje služby monitorování pro OMS získáváním dat ze spravovaných prostředků do centrálního úložiště. Tato data můžou zahrnovat události, údaje o výkonu nebo vlastní data poskytnutá prostřednictvím hello rozhraní API. Jakmile získány, je k dispozici pro výstrahy, analýzu a export dat hello.


Tato metoda vám umožní tooconsolidate data z různých zdrojů, takže můžete kombinovat data ze služeb Azure s vaší stávající místní prostředí. Také jasně odděluje hello shromažďování dat hello od hello akce prováděné na tato data tak, aby všechny akce jsou k dispozici tooall druhy dat.


![Log Analytics](./media/azure-operational-security/azure-operational-security-fig2.png)

Hello analýzy protokolů služby bezpečně spravuje vaše data založená na cloudu pomocí hello následující metody:
-   Oddělení dat
-   uchovávání dat
-   Fyzické zabezpečení
-   Správa incidentů
-   Dodržování předpisů
-   certifikace standardy zabezpečení

### <a name="azure-backup"></a>Azure Backup

[Zálohování Azure](http://azure.microsoft.com/documentation/services/backup) poskytuje data zálohování a obnovení služby a je součástí sady hello OMS produktů a služeb.
Chrání data vaší aplikace a dlouhá léta je uchovává bez nutnosti velkých investic a s minimálními provozními náklady. Ho můžete zálohovat data ze fyzickými a virtuálními serverů Windows kromě tooapplication úlohy, jako například SQL Server a SharePoint. Můžete použít také pomocí [System Center Data Protection Manager (DPM)](https://en.wikipedia.org/wiki/System_Center_Data_Protection_Manager) tooreplicate chráněná data tooAzure redundanci a dlouhodobé uložení.


Chráněná data ve službě Azure Backup se ukládají do trezoru záloh umístěného v konkrétní geografické oblasti. Hello data se replikují uvnitř hello stejné oblasti a v závislosti na typu hello trezoru, může být také replikované tooanother oblast pro další odolnost proti chybám.

### <a name="management-solutions"></a>Řešení pro správu
[Microsoft Operations Management Suite (OMS)](https://docs.microsoft.com/azure/operations-management-suite/oms-security-getting-started) je společnosti Microsoft založená na cloudu IT řešení správy, které pomáhá spravovat a chránit místní a cloudové infrastruktury.


[Řešení pro správu](https://docs.microsoft.com/azure/operations-management-suite/operations-management-suite-solutions) jsou hotových sady logics, které implementují scénáři konkrétní správy pomocí jedné nebo více služeb OMS. Různá řešení jsou dostupné od Microsoftu a partnerů, můžete snadno přidat tooyour předplatného Azure tooincrease hello hodnotu investice v OMS. Jako partner můžete vytvořit vlastní řešení toosupport vašim aplikacím a službám a poskytněte toousers prostřednictvím hello Azure Marketplace nebo šablony rychlý Start.


![Řešení pro správu](./media/azure-operational-security/azure-operational-security-fig4.png)

Dobrým příkladem řešení, které využívá několik dalších funkcí služby tooprovide je hello [řešení pro správu aktualizací](https://docs.microsoft.com/azure/operations-management-suite/oms-solution-update-management). Toto řešení používá hello [analýzy protokolů](https://docs.microsoft.com/azure/log-analytics/log-analytics-overview) agenta pro Windows a Linux toocollect informace o požadovaných aktualizací na každého agenta. Zapíše úložiště analýzy protokolů toohello tato data kde můžete analyzovat s zahrnuté řídicího panelu.

Při vytváření nasazení sady runbook v [Azure Automation](https://docs.microsoft.com/azure/automation/automation-intro) jsou použité tooinstall požadované aktualizace. Spravovat tento celý proces hello portálu a není nutné tooworry o základní podrobnosti hello.

## <a name="azure-security-center"></a>Azure Security Center

Azure Security Center pomáhá chránit prostředky v Azure. Poskytuje integrované bezpečnostní sledování a správu zásad ve vašich předplatných Azure. V rámci služby hello, budete moct toodefine zásady nejen pro svá předplatná Azure, ale i proti [skupiny prostředků](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview#resource-groups), aby mohli být podrobnější.

### <a name="security-policies-and-recommendations"></a>Zásady a doporučení zabezpečení

Zásady zabezpečení definuje hello sadu ovládacích prvků, které se doporučují pro prostředky v rámci zadané předplatné nebo prostředek skupiny hello.

V Security Center určíte zásady podle tooyour požadavky na zabezpečení a společnosti hello typu aplikací nebo citlivosti dat hello.

![Zásady a doporučení zabezpečení](./media/azure-operational-security/azure-operational-security-fig5.png)


Zásady, které jsou povoleny v úrovni předplatného hello automaticky rozšíří tooall skupin prostředků v rámci předplatného hello, jak je vidět v diagramu hello na pravé straně hello:


### <a name="data-collection"></a>Shromažďování dat

Security Center shromažďuje data z vaší virtuální počítače (VM) tooassess jejich stavu zabezpečení, zadejte doporučení zabezpečení a výstrah toothreats. Pokud je povolen vaše první přístup Security Center, shromažďování dat na všech virtuálních počítačích v rámci vašeho předplatného. Doporučuje se shromažďování dat, ale můžete chodit tak, že zakážete shromažďování dat v hello zásad Security Center.

### <a name="data-sources"></a>Zdroje dat

- Azure Security Center analyzuje data z hello následující zdroje tooprovide viditelnost do vaší stavu zabezpečení, zjištění chyb zabezpečení a doporučujeme způsoby zmírnění rizik a detekovat hrozby aktivní:

-   Azure Services: Používá informace o konfiguraci hello služeb Azure, že jste nasadili navázat komunikaci s poskytovatelem prostředků dané služby.

- Síťový provoz: Využívá vzorkovaná metadata síťového provozu z infrastruktury společnosti Microsoft, jako je třeba zdrojová a cílová IP adresa/port, velikost paketu nebo síťový protokol.

-   Partnerská řešení: Využívá výstrahy zabezpečení ze všech integrovaných partnerských řešení, jako jsou třeba brány firewall a antimalwarová řešení.

-   Virtuální počítače: Využívá z vašich virtuálních počítačů konfigurační informace a informace o událostech zabezpečení, jako jsou třeba protokoly událostí a auditů systému Windows, protokoly IIS, zprávy syslog a soubory se stavem systému.

### <a name="data-protection"></a>Ochrana dat

toohelp zákazníkům zabránit, zjistit a reagovat toothreats, Azure Security Center shromažďuje a zpracovává data týkající se zabezpečení, včetně informací o konfiguraci, metadata, protokoly událostí, soubory se stavem systému a další. Microsoft dodržuje pravidla dodržování předpisů a zabezpečení toostrict – z kódování toooperating služby.

-   **Oddělení dat**: Data se ukládají na jednotlivých součástí v rámci služby hello logicky samostatné. Všechna data jsou označená podle organizace. Toto značení přetrvává v průběhu cyklu hello dat a je požadováno v jednotlivých vrstvách služby hello.

-   **Přístup k datům**: tooprovide doporučení zabezpečení a prozkoumat potenciální ohrožení zabezpečení, může Microsoft pracovníky přístup k informace shromážděné nebo analyzovat služby Azure, včetně soubory se stavem systému, zpracování událostí vytváření, disku Virtuálního snímky a artefaktů, které můžou neúmyslně obsahovat Data zákazníků nebo osobní data z virtuálních počítačů. Jsme splňovat toohello [Microsoft Online Services podmínky a prohlášení o ochraně osobních údajů](http://www.microsoftvolumelicensing.com/DocumentSearch.aspx?Mode=3&DocumentTypeId=31), která stanoví, že společnost Microsoft není používá Data zákazníků nebo odvození informací z něj pro obchodní účely reklamy nebo podobné.

-   **Data použití**: Společnost Microsoft používá vzory a analýzou hrozeb vidět napříč více klienty tooenhance naše funkce prevence a detekce; jsme učinit v souladu s závazky týkajícími se ochrany osobních údajů hello popsané v našem [ochrany osobních údajů Příkaz](https://www.microsoft.com/privacystatement/OnlineServices/Default.aspx).

### <a name="data-location"></a>Umístění dat

Azure Security Center shromažďuje dočasné kopie souborů se stavem systému a analyzuje je za účelem detekce stop pokusů o napadení zabezpečení, neúspěšných i úspěšných. Azure Security Center provede tuto analýzu v rámci hello stejném geograficky redundantním jako hello prostoru a odstranění hello dočasné kopie po dokončení analýzy. Artefakty počítače jsou uložené v centrálně hello stejné oblasti jako hello virtuálních počítačů.

-   **Účty úložiště**: účet úložiště je zadán pro každou oblast, kde jsou virtuální počítače spuštěné. Díky této může hello je toostore data ve stejné oblasti jako hello virtuální počítač, ze které hello je shromažďovat data.

-   **Úložiště Azure Security Center**: informace o výstrahách zabezpečení, včetně výstrahy partnera, doporučení a stav zabezpečení jsou uloženy centrálně, aktuálně ve Spojených státech amerických hello. Tyto informace můžou obsahovat informace týkajících se konfigurace a zabezpečení události shromážděné z virtuálních počítačů jako potřebné tooprovide můžete pomocí hello zabezpečení, doporučení nebo výstrah zabezpečení stav.


## <a name="azure-monitor"></a>Azure Monitor

Hello [OMS zabezpečení](https://docs.microsoft.com/azure/operations-management-suite/oms-security-monitoring-resources) a auditu řešení umožňuje IT tooactively monitorovat všechny prostředky, které může pomoci minimalizovat dopad incidentů, zabezpečení hello. OMS zabezpečení a Audit mít zabezpečení domény, které lze použít ke sledování prostředků. Hello zabezpečení domény poskytuje rychlý přístup toooptions, monitorování hello zabezpečení následujících domén jsou popsané v další podrobnosti:

-   posouzením malwaru
-   Posouzení aktualizací
-   Identit a přístupu.

Monitorování Azure poskytuje tooinformation ukazatele na konkrétní typy prostředků. Nabízí vizualizace, dotaz, směrování, výstrahy, automatické škálování a automatizace na data z hello infrastrukturu Azure (protokol aktivit) a každý jednotlivých prostředků Azure (diagnostických protokolů).

![Azure Monitor](./media/azure-operational-security/azure-operational-security-fig6.png)


Cloudové aplikace jsou komplexní s mnoha přesunutí částmi. Monitorování poskytuje tooensure data, která vaše aplikace zůstává nahoru a spuštěn v dobrém stavu. Pomáhá také můžete toostave vypnout potenciální problémy a řešení potíží s uplynulou těch, které jsou.

Kromě toho můžete použít monitorování hlubšímu porozumění toogain data o vaší aplikaci. Dané znalosti můžete vám pomohou tooimprove výkon aplikace nebo udržovatelnosti nebo automatizaci akcí, které by jinak vyžadují ruční zásah.

### <a name="azure-activity-log"></a>Protokol činnosti Azure


Je protokol, který poskytuje vhled do hello operací, které byly provedeny v prostředky ve vašem předplatném. Hello protokol aktivit byl dřív označovala jako "Protokoly auditu" nebo "Provozní protokoly,", protože oznámí události rovině řízení pro vaše předplatné.

![Protokol činnosti Azure](./media/azure-operational-security/azure-operational-security-fig7.png)

Pomocí hello protokolu činnosti, můžete určit hello ", kdo a kdy se pro všechny zápisu operace (PUT, POST, DELETE) pořízené hello prostředky ve vašem předplatném. Můžete také chápou hello stav operace hello a další relevantní vlastnosti. Hello protokol aktivit nezahrnuje čtení operací (GET) nebo operace pro prostředky, které používají hello Classic model.

### <a name="azure-diagnostic-logs"></a>Azure diagnostických protokolů

Tyto protokoly jsou vygenerované prostředek a nabízí bohatou a často data o operaci hello tohoto prostředku. obsah Hello tyto protokoly se liší podle typu prostředku.

Například protokoly událostí systému Windows jsou jednu kategorii protokolů diagnostiky pro virtuální počítače a objektů blob, table a queue protokoly jsou kategorie diagnostické protokoly pro účty úložiště.

Diagnostické protokoly se liší od hello [protokol aktivit (dříve označované jako protokol auditů nebo operační protokol)](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-activity-logs). Protokol aktivit Hello poskytuje náhled do hello operací, které byly provedeny v prostředky ve vašem předplatném. Diagnostické protokoly získat přehled o operace, aby prostředku provedeny sám sebe.

### <a name="metrics"></a>Metriky

Azure monitorování umožňuje tooconsume telemetrie toogain přehled hello výkonu a stavu úlohy v Azure. Hello nejdůležitější typ Azure telemetrická data je hello metriky (také nazývané čítače výkonu) vysílaných prostředků nejvíce Azure. Monitorování Azure poskytuje několik způsobů tooconfigure a využívat tyto [metriky](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-metrics) pro monitorování a řešení potíží. Metriky jsou cenné zdroj telemetrie a umožňují toodo hello následující úlohy:

-   **Sledování výkonu hello** vaše prostředku (například počítač, web nebo logiku aplikace) vykreslení jeho metriky na portálu graf a Připnutí tento řídicí panel tooa grafu.

-   **Upozorňování problému** ovlivňuje když metriky překračuje prahovou hodnotu určité text hello výkonu prostředku.

-   **Konfigurovat automatické akce**, jako je automatické škálování prostředku nebo když metriky překračuje určité prahovou hodnotu, která iniciovala sady runbook.

-   **Provádět pokročilé analýzy** nebo generování sestav na trendy výkonu a využití vaší prostředku.

-   **Archiv** hello výkon nebo stav historii prostředku pro dodržování předpisů nebo účely auditování.

### <a name="azure-diagnostics"></a>Diagnostika Azure

Je funkce hello v rámci Azure, která umožňuje hello shromažďování diagnostických dat na nasazené aplikace. Můžete použít rozšíření diagnostiky hello z různých různých zdrojů. Aktuálně podporované jsou [webové služby Azure Cloud a rolí pracovního procesu](https://docs.microsoft.com/azure/vs-azure-tools-configure-roles-for-cloud-service), [virtuální počítače Azure](https://docs.microsoft.com/azure/virtual-machines/windows/overview) systémem Microsoft Windows, a [Service Fabric](https://docs.microsoft.com/azure/monitoring-and-diagnostics/azure-diagnostics). Jinými službami Azure mají své vlastní samostatné diagnostiky.

## <a name="azure-network-watcher"></a>Sledovací proces sítě Azure

Auditování zabezpečení sítě je důležité pro zjišťování chyb zabezpečení sítě a zajištění dodržování zabezpečení IT a modelu regulačních zásad správného řízení. Zobrazení skupiny zabezpečení můžete načíst skupinu zabezpečení sítě a zabezpečení pravidla hello nakonfigurované a hello pravidla efektivní zabezpečení. Hello seznam pravidel použít můžete určit hello porty, které jsou otevřené a vyhodnocení ohrožení zabezpečení sítě.

[Sledovací proces sítě](https://docs.microsoft.com/azure/network-watcher/network-watcher-monitoring-overview#network-watcher) je místní služba, která vám umožní toomonitor a diagnostikovat podmínky na úrovni sítě v, do a z Azure. Diagnostika sítě a k dispozici sledovací proces sítě vizualizace nástroje vám pomůžou pochopit, diagnostikovat a získat statistiky tooyour sítě v Azure. Tato služba obsahuje zachytáváním paketů, další směrování, IP tok ověření, zobrazení skupiny zabezpečení, tok protokolů NSG. Scénář úrovně monitorování obsahuje zobrazení tooend end síťovým prostředkům v monitorování kontrast tooindividual sítě prostředků.

![Sledovací proces sítě Azure](./media/azure-operational-security/azure-operational-security-fig8.png)

Sledovací proces sítě má aktuálně hello následující možnosti:

-   **<a href="https://docs.microsoft.com/azure/network-watcher/network-watcher-monitoring-overview">Protokoly auditu</a>**-operace provedené v rámci konfigurace hello sítí přihlášeni. Tyto protokoly můžete zobrazit v hello portál Azure nebo pomocí nástroje Microsoft, jako je Power BI nebo nástroje třetích stran. Protokoly auditu jsou k dispozici prostřednictvím portálu hello, prostředí PowerShell, rozhraní příkazového řádku a Rest API. Další informace o protokolů auditu najdete v tématu auditu operace s Resource Managerem. Protokoly auditu jsou k dispozici pro operace udělat na všechny síťové prostředky.


-   **<a href="https://docs.microsoft.com/azure/network-watcher/network-watcher-ip-flow-verify-overview">Ověřuje IP toku </a>**  – ověří, zda je paket povolený nebo zakázaný na základě toku informace 5 řazené kolekce členů paketu parametrů (cílovou IP adresu, zdrojové IP adresy, cílový Port, zdrojový Port a protokol). Pokud paket hello je zakázané skupinu zabezpečení sítě, je vrácena hello pravidlo a skupinu zabezpečení sítě, který odepřen hello paketů.

-   **<a href="https://docs.microsoft.com/azure/network-watcher/network-watcher-next-hop-overview">Další směrování</a>**  -určuje hello další segment směrování paketů směrovány v hello prostředky infrastruktury sítě Azure, umožňuje toodiagnose všechny nesprávně nakonfigurované uživatelem definované trasy.

-   **<a href="https://docs.microsoft.com/azure/network-watcher/network-watcher-security-group-view-overview">Zobrazení skupiny zabezpečení</a>**  -získá hello zabezpečení efektivní a použitých pravidel, která se použijí na virtuálním počítači.

-   **<a href="https://docs.microsoft.com/azure/network-watcher/network-watcher-nsg-flow-logging-overview">Protokolování toku NSG</a>**  -toku protokoly pro skupinu zabezpečení sítě umožňují protokoly toocapture související se tootraffic, které jsou povolené nebo zakázané pravidla zabezpečení hello ve skupině hello. tok Hello je definována informací o 5-n-tice – zdrojové adresy IP, cílovou IP adresu, zdrojový Port, cílový Port a protokol.

## <a name="azure-storage-analytics"></a>Azure Storage Analytics

[Analytika úložiště](https://docs.microsoft.com/rest/api/storageservices/fileservices/storage-analytics) můžete ukládat metriky, které zahrnují data statistiky a kapacity agregované transakcí týkající se požadavků tooa úložiště služby. Transakce jsou v úrovni operaci hello rozhraní API a na úrovni služby úložiště hello a hlásí kapacity na úrovni služby úložiště hello. Metriky dat můžete použít tooanalyze využití služby úložiště, diagnostikovat problémy s požadavků na službu úložiště hello a tooimprove hello výkon aplikací, které používají služby přístup.

[Azure Storage Analytics](https://docs.microsoft.com/rest/api/storageservices/fileservices/storage-analytics) provádí protokolování a poskytuje data metriky pro účet úložiště. Můžete použít tento tootrace požadavky na data, analyzovat trendy využití a diagnostikovat problémy s vaším účtem úložiště. Analýza protokolování úložiště je k dispozici pro hello [službám Blob, fronty a tabulky](https://docs.microsoft.com/azure/storage/storage-introduction). Úložiště analýzy protokolů podrobné informace o službě úložiště tooa úspěšné i neúspěšné požadavky.

Tato informace může být použité toomonitor jednotlivých požadavků a toodiagnose problémy se službou úložiště. Na základě typu best effort protokolované požadavky. Položky protokolu se vytvoří pouze v případě, že existují požadavků na koncový bod služby hello přístup. Pro příklad Pokud je účet úložiště svůj koncový bod objektu Blob, ale není v jeho tabulku nebo frontu koncových bodů, pouze protokoly týkající se vytvoří toohello služby objektů Blob.

toouse analytika úložiště, musíte povolit samostatně pro každou službu chcete toomonitor. Můžete ji povolit v hello [portál Azure](https://portal.azure.com/); podrobnosti najdete v tématu [monitorování účtu úložiště v hello portál Azure](https://docs.microsoft.com/azure/storage/storage-monitor-storage-account). Můžete také povolit Storage Analytics prostřednictvím kódu programu prostřednictvím hello REST API nebo knihovny klienta hello. Použijte hello nastavit vlastnosti služby operaci tooenable Storage Analytics samostatně pro každou službu.

Hello agregovaná data se ukládají v dobře známé objektu blob (pro protokolování) a dobře známé tabulky (pro metriky), které můžete získat přístup pomocí služby objektů Blob hello a služby Table rozhraní API.

Analytika úložiště může mít 20 TB na hello množství uložených dat, která je nezávislá hello celkový limit pro váš účet úložiště. Všechny protokoly jsou uloženy v [objekty BLOB bloků](https://docs.microsoft.com/azure/storage/storage-analytics) v kontejneru nazvaném $logs, které automaticky vytvářejí, když je pro účet úložiště povolená analytika úložiště.

Hello následující akce prováděné Storage Analytics jsou fakturovatelné:

-   Objekty BLOB toocreate požadavky pro protokolování
-   Požadavky toocreate tabulka entity pro metriky.

> [!Note]
> Další informace o fakturaci a zásad uchovávání dat najdete v tématu [Storage Analytics a fakturace](https://docs.microsoft.com/rest/api/storageservices/fileservices/storage-analytics-and-billing).
> Pro optimální výkon budete chtít toolimit hello počet vysoce využívané disky připojené toohello virtuální počítač tooavoid možných omezení. Pokud všechny disky nejsou využívání vysoce v hello současně, účet úložiště hello může podporovat větší počet disků.

> [!Note]
> Další informace o limity účtu úložiště najdete v tématu [a cíle výkonnosti služby Azure Storage Scalability](https://docs.microsoft.com/azure/storage/storage-scalability-targets).


jsou protokolovány Hello následující typy požadavků na ověření a anonymní.

| Ověření  | Anonymní|
| :------------- | :-------------|
| Úspěšné požadavky | Úspěšné požadavky |
|Neúspěšné požadavky, včetně vypršení časového limitu, omezení šířky pásma, sítě, autorizace a dalších chyb | Požadavky pomocí sdíleného přístupového podpisu (SAS), včetně žádostí úspěšné a neúspěšné |
| Požadavky pomocí sdíleného přístupového podpisu (SAS), včetně žádostí úspěšné a neúspěšné |Chyby časového limitu pro klienta a serveru |
|   Data tooanalytics požadavků |    Neúspěšné požadavky GET s kódem chyby 304 (upraveno) |
| Požadavky na úložiště Analytics samostatně, jako je například protokol vytvoření nebo odstranění, se neprotokolují. Úplný seznam hello protokolovat data je popsána v hello [stavové zprávy a Storage Analytics protokolované](https://docs.microsoft.com/rest/api/storageservices/fileservices/storage-analytics-logged-operations-and-status-messages) a [úložiště analýzy protokolů formátu](https://docs.microsoft.com/rest/api/storageservices/fileservices/storage-analytics-log-format) témata. | Všechny ostatní selhání anonymních požadavků se neprotokolují. Úplný seznam hello protokolovat data je popsána v hello [stavové zprávy a Storage Analytics protokolované](https://docs.microsoft.com/rest/api/storageservices/fileservices/storage-analytics-logged-operations-and-status-messages) a [úložiště analýzy protokolů formátu](https://docs.microsoft.com/rest/api/storageservices/fileservices/storage-analytics-log-format). |
## <a name="azure-active-directory"></a>Azure Active Directory

Azure AD také zahrnuje úplná sada funkcí správy identit, včetně vícefaktorového ověřování, registrace zařízení, hesla pomocí samoobslužné služby správy, samoobslužnou správu skupin, správy privilegovaného účtu, přístup na základě rolí ovládací prvek, sledování využití aplikací, bohaté auditování a monitorování zabezpečení a výstrahy.

-   Vylepšení aplikace zabezpečení pomocí vícefaktorového ověřování Azure AD a podmíněného přístupu.

-   Sledování využití aplikací a chránit vaši firmu před hrozbami pokročilé zabezpečení, monitorování a vytváření sestav.

Azure Active Directory (Azure AD) obsahuje různé sestavy zabezpečení, aktivit a auditu pro váš adresář. [Hello Azure Active Directory sestava auditu](https://docs.microsoft.com/azure/active-directory/active-directory-reporting-guide) pomáhá zákazníkům tooidentify privilegované akce, které došlo k chybě v Azure Active Directory. Privilegované akce zahrnují změny zvýšení oprávnění (například role vytvoření nebo resetování hesla), změna konfigurace zásad (třeba zásady pro hesla) nebo změny toodirectory konfigurace (například změny toodomain federation nastavení).

Hello sestavy poskytují hello záznam auditu pro název události hello, objektu actor hello, který provedl hello akce, cílový prostředek hello vliv hello změnu a hello datum a čas (ve formátu UTC). Je možné tooretrieve hello seznam událostí auditu pro Azure Active Directory prostřednictvím hello zákazníků [portál Azure](https://portal.azure.com/), jak je popsáno v [zobrazit protokoly auditu](https://docs.microsoft.com/azure/active-directory/active-directory-reporting-azure-portal). Tady je seznam hello sestavy zahrnuté:

| Sestavy zabezpečení  | Sestavy aktivit| Sestavy auditu |
| :------------- | :-------------| :-------------|
|Přihlášení z neznámých zdrojů | Využití aplikací: souhrn | Sestava auditu adresáře |
|Přihlášení po několika neúspěších | Využití aplikací: podrobnosti |   |
|Přihlášení z více geografických poloh | Řídicí panel aplikací |  |
|Přihlášení z IP adres s podezřelou aktivitou |Chyby zřizování účtů |  |
|Nestandardní přihlašovací aktivita |Zařízení jednotlivých uživatelů |  |
|Přihlášení z možných nakažených zařízení |Aktivity jednotlivých uživatelů |   |
|Uživatelé s neobvyklou přihlašovací aktivitou |Sestava aktivit skupin |   |
| |Sestava aktivit registrace resetování hesla |   |
| |Aktivity resetování hesla |   | |



Hello data z těchto sestav mohou být užitečné tooyour aplikace, jako je například systémů SIEM, auditování a nástroje business intelligence. vytváření sestav Azure AD Hello [rozhraní API](https://docs.microsoft.com/azure/active-directory/active-directory-reporting-api-getting-started) poskytují programový přístup toohello data pomocí sady založené na REST API. Tato rozhraní API můžete volat z různých programovacích jazyků a nástroje.

Události v hello sestava auditu Azure AD jsou uchovány na 180 dní.

> [!Note]
> Další informace o uchovávání dat v sestavách najdete v tématu [zásady uchování sestav Azure ve službě Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-reporting-retention).

Pro zákazníky ukládat jejich [události auditu](https://docs.microsoft.com/azure/active-directory/active-directory-reporting-audit-events) dobu uchování delší hello Reporting rozhraní API může být použité tooregularly vyžádání obsahu události auditu do samostatné datové úložiště.

## <a name="summary"></a>Souhrn

Tento článek souhrny ochranu vašich osobních údajů a zabezpečení vašich dat při dodávání softwaru a služeb, které vám pomohou spravovat hello infrastruktury IT v organizaci. Microsoft rozpozná, že při jejich svěřit tooothers svá data, vyžaduje tento vztah důvěryhodnosti přísných zabezpečení. Microsoft dodržuje pravidla dodržování předpisů a zabezpečení toostrict – z kódování toooperating služby. Zabezpečení a ochrana dat je nejvyšší prioritou ve společnosti Microsoft.

Tento článek vysvětluje

-   Jak dat shromažďovaných, zpracování a zabezpečená hello Operations Management Suite (OMS).

-   Rychlá analýza událostí z různých zdrojů dat Zjistit rizika zabezpečení a porozumět hello oboru a dopad hrozby a útoky poškození hello toomitigate porušení zabezpečení.

-   Identifikace vzorů útoků vizualizací odchozího škodlivého síťového provozu a typů bezpečnostních hrozeb Pochopení postavení zabezpečení hello celé prostředí bez ohledu na platformu.

-   Zaznamenejte všechny hello protokolů a událostí data potřebná pro audit zabezpečení nebo dodržování předpisů. Lomítko hello čas a prostředky potřebné toosupply auditu zabezpečení s kompletní, vyhledávat a exportovatelný protokolů a událostí datové sady.

<ul>
<li>Shromažďování událostí zabezpečení, auditování a analýzu porušení tookeep zavřít oční vaše prostředky:</li>
<ul>
<li>Zabezpečení</li>
<li>Významné problém</li>
<li>Souhrny hrozeb</li>
</ul>
</ul>

## <a name="next-steps"></a>Další kroky

- [Návrh a provozního zabezpečení](https://www.microsoft.com/trustcenter/security/designopsecurity)

Microsoft návrhy jejích služeb a softwaru se zabezpečením v paměti toohelp zajistěte, aby byl jeho cloudové infrastruktury odolné a před útoky.

- [Služby Operations Management Suite | Zabezpečení a dodržování předpisů](https://www.microsoft.com/cloud-platform/security-and-compliance)

Použít Microsoft zabezpečení dat a analýzu tooperform více inteligentního a efektivní detekce hrozby.

- [Operace a plánování Azure Security Center](https://docs.microsoft.com/azure/security-center/security-center-planning-and-operations-guide) sadu kroků a úloh, které vám pomůžou toooptimize použití služby Security Center na základě vaší organizace požadavky na zabezpečení a modelu správy cloudu.

