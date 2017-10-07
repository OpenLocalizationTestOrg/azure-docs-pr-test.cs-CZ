---
title: "aaaAzure protokolování a auditování | Microsoft Docs"
description: "Informace o použití hlubšímu porozumění toogain protokolování data o vaší aplikaci."
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
ms.openlocfilehash: d0e817b071962ad9bef6250267092b5f9282bc7a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-logging-and-auditing"></a>Azure protokolování a auditování
## <a name="introduction"></a>Úvod
### <a name="overview"></a>Přehled
tooassist aktuální potenciální Azure zákazníky a vysvětlující Princip fungování a pomocí hello různé související se zabezpečením možnosti dostupné v a které obaluje hello platformě Azure, společnost Microsoft vyvinula řadu dokumenty white paper, zabezpečení přehledy, osvědčené postupy a kontrolní seznamy. témata Hello rozsahu z hlediska spektra a hloubky a jsou pravidelně aktualizovány. Tento dokument je součástí této řady dle souhrnu v následující části abstraktní hello.
### <a name="azure-platform"></a>Platformy Azure
Azure je platforma otevřené a flexibilní cloudové služby, která podporuje hello nejširší výběr operačních systémů, programovací jazyky, rozhraní, nástroje, databází a zařízení.

Můžete například provést následující věci:
-   Spusťte Linux kontejnery s integrace Dockeru.

-   Vývoj aplikací pomocí jazyka JavaScript, Python, .NET, PHP, Java a Node.js

-   Sestavení back EndY pro iOS, Android a Windows zařízení.

Služby veřejného cloudu Azure podporovat hello stejné technologie miliony vývojářů a IT profesionály už spoléhají na a vztah důvěryhodnosti.

Při vytvoření ve nebo migrovat prostředky IT, poskytovatele cloudových služeb, se spoléhat na dané organizace dalo tooprotect aplikace a data s hello služeb a hello ovládací prvky poskytují zabezpečení hello toomanage vaše cloudové prostředky.

Infrastruktury Azure a je určená z hello tooapplications zařízení pro hostování miliony zákazníků současně, a poskytuje trustworthy foundation, na kterém může podnikům podle jejich potřeb zabezpečení. Kromě toho Azure poskytuje širokou škálu toocontrol možnost konfigurovat zabezpečení možnosti a hello je tak, aby si můžete přizpůsobit toomeet hello jedinečné požadavky na zabezpečení vašich nasazení. Tento dokument umožňuje splňovat tyto požadavky.

### <a name="abstract"></a>Abstraktní
Auditování a protokolování událostí souvisejících se zabezpečením a související výstrahy, jsou důležité součásti v strategie ochrany počáteční datum. Protokoly zabezpečení a sestavy poskytují záznam elektronické podezřelé aktivity a nápovědu zjišťování struktur, které můžou signalizovat úspěšné nebo neúspěšné pokusy o externí průnikům hello sítě, jakož i interní útoky. Můžete použít aktivity uživatelů auditování toomonitor, dodržování legislativních dokumentu, provádět forenzní analýzu a další. Výstrahy zadejte okamžité odeslání oznámení, když dojde k události zabezpečení.

Služby Microsoft Azure a produktů umožňují konfigurovat zabezpečení, auditování a protokolování možnosti toohelp zjistí nesrovnalosti v vaše zásady a mechanismy zabezpečení a vyřešte tyto mezery toohelp zabránit narušení. Služby Microsoft nabízejí některé (a v některých případech všechny) z hello následující možnosti: centralizované sledování, protokolování a analýza systémy tooprovide průběžné viditelnost; včas výstrah; a sestavy toohelp spravujete hello velké množství informací generované zařízení a služeb.

Data protokolu Microsoft Azure může být exportovaný tooSecurity systémy Incident a událostí správy (SIEM) pro analýzu a umožňuje integraci s řešeními třetích stran auditování.

Tento dokument White Paper je úvodem pro generování, shromažďování a analýzu protokolů zabezpečení ze služby hostované v Azure a umožní vám získat přehled o zabezpečení do Azure nasazení. Hello rozsah tohoto dokumentu white paper služby vytvořené a nasazené v Azure a je omezený tooapplications.

> [!Note]
> Některá doporučení v nich obsažených může vést k vyšší data, síťové nebo využití výpočetních prostředků a zvýšit náklady na licence nebo předplatné.

## <a name="types-of-logs-in-azure"></a>Typy protokolů v Azure
Cloudové aplikace jsou komplexní s mnoha přesunutí částmi. Protokoly poskytují tooensure data, která vaše aplikace zůstává nahoru a spuštěn v dobrém stavu. Pomáhá také můžete toostave vypnout potenciální problémy a řešení potíží s uplynulou těch, které jsou. Kromě toho můžete hlubšímu porozumění toogain protokolování data o vaší aplikaci. Dané znalosti můžete vám pomohou tooimprove výkon aplikace nebo udržovatelnosti nebo automatizaci akcí, které by jinak vyžadují ruční zásah.

Azure vytvoří rozsáhlé protokolování pro všechny služby Azure. Tyto protokoly jsou rozdělené podle těchto hlavních typů:
-   **Řízení nebo protokoly** poskytují přehled o hello operace vytvoření správce prostředků Azure, aktualizace a odstranění. [Azure protokoly aktivity](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-activity-logs) je příkladem tohoto typu protokolu.

-   **Data roviny protokoly** poskytují přehled o hello událostí vyvolaných jako součást hello využití prostředek služby Azure. Příkladem tohoto typu protokolu jsou hello událostí systému Windows systém, zabezpečení, a aplikace přihlásí na virtuálním počítači a hello [protokolů diagnostiky](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs) nakonfigurovaný pomocí Azure monitorování


-   **Zpracování události** poskytnout informace o analyzovaných události a výstrahy, které byly zpracovány vaším jménem. Příkladem tohoto typu jsou [Azure Security Center výstrahy](https://docs.microsoft.com/azure/security-center/security-center-managing-and-responding-alerts) kde [Azure Security Center](https://docs.microsoft.com/azure/security-center/security-center-intro) je zpracován a analyzovat vaše předplatné a poskytuje výstrahy stručným zabezpečení

Hello následující tabulce je nejdůležitější typ seznamu protokolů dostupných v Azure.

| Kategorie protokolu | Typ protokolu | Použití | Integrace |
| ------------ | -------- | ------ | ----------- |
|[Protokoly aktivity](https://docs.microsoft.com/en-us/azure/monitoring-and-diagnostics/monitoring-overview-activity-logs)|Události rovině řízení na prostředky Azure Resource Manager| Získáte přehled o hello operace, které byly provedeny na prostředky v rámci vašeho předplatného.|   REST API & [Azure monitorování](https://docs.microsoft.com/en-us/azure/monitoring-and-diagnostics/monitoring-overview-activity-logs)|
|[Azure diagnostických protokolů](https://docs.microsoft.com/en-us/azure/monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs)|časté data o operaci hello Azure Resource Manager prostředků v předplatném| Získat přehled o operace, aby prostředku provedeny sám sebe| Azure monitorování [datového proudu](https://docs.microsoft.com/en-us/azure/monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs)|
|[Vytváření sestav AAD](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-reporting-azure-portal)|Protokoly a sestavy|Uživatelské přihlašovací aktivity & systému aktivity informace o uživatelích a Správa skupin|[Graph API](https://docs.microsoft.com/en-us/azure/active-directory/develop/active-directory-graph-api-quickstart)|
|[Virtuální počítač & cloudové služby](https://docs.microsoft.com/en-us/azure/cloud-services/cloud-services-dotnet-diagnostics-storage)|Protokolu událostí systému Windows a Linux Syslog|  Zaznamená data systému a data protokolování hello virtuálními počítači a přenosy dat do účtu úložiště podle vašeho výběru.| Pomocí Windows [WAD](https://docs.microsoft.com/en-us/azure/azure-diagnostics) (Windows Azure Diagnostics úložiště) a Linux v nástroji Azure sledování|
|[Storage Analytics](https://docs.microsoft.com/en-us/rest/api/storageservices/fileservices/storage-analytics)|Protokolování úložiště a poskytuje data metriky pro účet úložiště|Poskytuje vhled do trasování požadavků, analyzovat trendy využití a diagnostikovat problémy s vaším účtem úložiště.|  REST API nebo hello [klientské knihovny](https://msdn.microsoft.com/en-us/library/azure/mt347887.aspx)|
|[Tok protokolů NSG (skupina zabezpečení sítě)](https://docs.microsoft.com/en-us/azure/network-watcher/network-watcher-nsg-flow-logging-overview)|Formát JSON a zobrazuje toky odchozí a příchozí pravidlo za den|Zobrazení informací o příchozí a odchozí provoz IP prostřednictvím skupinu zabezpečení sítě|[Sledovací proces sítě](https://docs.microsoft.com/en-us/azure/network-watcher/network-watcher-monitoring-overview)|
|[Přehled aplikace](https://docs.microsoft.com/en-us/azure/application-insights/app-insights-overview)|Protokoly, výjimky a vlastní diagnostiky|  Služba správy výkonu (APM) aplikace pro web vývojářů ve více platformách.| Rozhraní REST API, [Power BI](https://powerbi.microsoft.com/en-us/documentation/powerbi-azure-and-power-bi/)|
|Zpracování dat nebo výstrahy zabezpečení| Výstraha Azure Security Center, OMS výstrahy| Informace o zabezpečení a výstrahy.|   Rozhraní REST API, JSON|

### <a name="activity-log"></a>Protokol aktivit
Hello [protokol činnosti Azure](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-activity-logs), poskytuje vhled do hello operací, které byly provedeny v prostředky ve vašem předplatném. Hello protokol aktivit se dřív označovala jako "Protokoly auditu" nebo "Provozní protokoly," vzhledem k tomu, že se hlásí [rovině řízení události](https://driftboatdave.com/2016/10/13/azure-auditing-options-for-your-custom-reporting-needs/) pro vaše předplatné. Pomocí hello protokolu činnosti, můžete určit hello ", kdo a kdy" pro všechny zápisu operace (PUT, POST, DELETE) pořízené hello prostředky ve vašem předplatném. Můžete také chápou hello stav operace hello a další relevantní vlastnosti. Hello protokol aktivit nezahrnuje operace čtení (GET).

PUT, POST, DELETE zde označují operace zápisu hello tooall, který obsahuje protokol aktivit na prostředcích hello. Například můžete použít hello aktivity protokoly toofind k chybě při odstraňování problémů s nebo toomonitor jak uživatele ve vaší organizaci upravit prostředek.

![Protokol aktivit](./media/azure-log-audit/azure-log-audit-fig1.png)


Můžete načíst události z protokolu aktivit pomocí hello portál Azure, [rozhraní příkazového řádku](https://docs.microsoft.com/azure/storage/storage-azure-cli), rutiny prostředí PowerShell a [REST API služby Azure monitorování](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-rest-api-walkthrough). Protokoly aktivity mají období uchovávání dat dne 19.

Scénáře integrace
-   [Vytvořte výstrahu e-mailu nebo webhooku, která aktivuje vypnout aktivity protokolu události.](https://docs.microsoft.com/azure/monitoring-and-diagnostics/insights-auditlog-to-webhook-email)

-   [Streamovat ho tooan centra událostí](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-stream-activity-logs-event-hubs) pro přijímání službu třetí strany nebo řešení vlastní analýzy, jako je například PowerBI.

-   Analyzovat v Power BI pomocí hello [balíček obsahu Power BI.](https://powerbi.microsoft.com/documentation/powerbi-content-pack-azure-audit-logs/)

-   [Uložte tooa účet úložiště pro archivaci nebo ruční kontroly.](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-archive-activity-log) Můžete zadat hello dobu uchování (ve dnech) pomocí protokolu profilů.

-   Dotaz a zobrazit v hello portálu Azure.

-   Dotaz je pomocí rutiny prostředí PowerShell, rozhraní příkazového řádku nebo REST API.

-   Export hello protokol aktivit pomocí protokolu profilů příliš[protokolu analýzy](https://docs.microsoft.com/azure/log-analytics/log-analytics-overview).

Můžete použít účet úložiště nebo [názvový prostor události rozbočovače](https://docs.microsoft.com/azure/event-hubs/event-hubs-resource-manager-namespace-event-hub-enable-archive) není v hello stejného předplatného jako hello generování jednoho protokolu. Hello uživatel, který konfiguruje nastavení hello musí mít odpovídající hello [RBAC](https://docs.microsoft.com/azure/active-directory/role-based-access-control-configure) přístup tooboth odběrů
### <a name="azure-diagnostic-logs"></a>Azure diagnostických protokolů
Azure diagnostické protokoly jsou vysílaných prostředků, které poskytují bohatou a často data o operaci hello tohoto prostředku. obsah Hello tyto protokoly se liší podle typu prostředku (například [protokoly událostí systému Windows](https://docs.microsoft.com/azure/log-analytics/log-analytics-data-sources-windows-events)jsou jednu kategorii protokolů diagnostiky pro virtuální počítače a [objektů blob, tabulky a fronty protokoly](https://docs.microsoft.com/azure/storage/storage-monitor-storage-account) kategorií diagnostických protokolů pro účty úložiště) a liší od hello protokol aktivit, které poskytuje vhled do hello operací, které byly provedeny v prostředky ve vašem předplatném.

![Azure diagnostických protokolů](./media/azure-log-audit/azure-log-audit-fig2.png)

Azure Diagnostics protokoly nabízí několik možností konfigurace, které je, portál Azure, pomocí prostředí PowerShell, rozhraní příkazového řádku (CLI) a rozhraní REST API.

**Scénáře integrace**
-   Uložíte tooa [účet úložiště](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-archive-diagnostic-logs) pro auditování nebo ruční kontroly. Můžete zadat hello dobu uchování (ve dnech) pomocí nastavení pro diagnostiku hello.

-   [Stream je tooEvent Hubs](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-stream-diagnostic-logs-to-event-hubs) pro přijímání službu třetí strany nebo řešení vlastní analýzy, jako [PowerBI.](https://powerbi.microsoft.com/documentation/powerbi-azure-and-power-bi/)

-   Analyzovat, s [analýzy protokolů OMS.](https://docs.microsoft.com/azure/log-analytics/log-analytics-overview)

**Podporované služby, schéma pro diagnostické protokoly a kategorie podporované protokolu na typ prostředku**


| Služba | Schéma & dokumentace | Typ prostředku | Kategorie |
| ------- | ------------- | ------------- | -------- |
|Load Balancer| [Analýzy protokolů pro vyrovnávání zatížení Azure (Preview)](https://docs.microsoft.com/en-us/azure/load-balancer/load-balancer-monitor-log)|Microsoft.Network/loadBalancers|  LoadBalancerAlertEvent|
|||Microsoft.Network/loadBalancers| LoadBalancerProbeHealthStatus
|Network Security Groups (Skupiny zabezpečení sítě)|[Analýza protokolu pro skupiny zabezpečení sítě (NSG)](https://docs.microsoft.com/en-us/azure/virtual-network/virtual-network-nsg-manage-log)|Microsoft.Network/networksecuritygroups|NetworkSecurityGroupEvent|
|||Microsoft.Network/networksecuritygroups|NetworkSecurityGroupRuleCounter|
|Brány Application Gateway|[Protokolování diagnostiky pro službu Application Gateway](https://docs.microsoft.com/en-us/azure/application-gateway/application-gateway-diagnostics)|Microsoft.Network/applicationGateways|ApplicationGatewayAccessLog|
|||Microsoft.Network/applicationGateways|ApplicationGatewayPerformanceLog|
|||Microsoft.Network/applicationGateways|ApplicationGatewayFirewallLog|
|Key Vault|[Protokolování v Azure Key Vault](https://docs.microsoft.com/en-us/azure/key-vault/key-vault-logging)|Microsoft.KeyVault/vaults|AuditEvent|
|Azure Search|[Povolení a používání Analýza provozu vyhledávání](https://docs.microsoft.com/en-us/azure/search/search-traffic-analytics)|Microsoft.Search/searchServices|OperationLogs|
|Data Lake Store|[Přístup k diagnostickým protokolům pro Azure Data Lake Store](https://docs.microsoft.com/en-us/azure/data-lake-store/data-lake-store-diagnostic-logs)|Microsoft.DataLakeStore/accounts|Auditování|
|Data Lake Analytics|[Přístup k protokolům diagnostiky pro Azure Data Lake Analytics](https://docs.microsoft.com/en-us/azure/data-lake-analytics/data-lake-analytics-diagnostic-logs)|Microsoft.DataLakeAnalytics/accounts|Auditování|
|||Microsoft.DataLakeAnalytics/accounts|Požadavky|
|||Microsoft.DataLakeStore/accounts|Požadavky|
|Logic Apps|[Vlastní schéma sledování B2B Logic Apps](https://docs.microsoft.com/en-us/azure/logic-apps/logic-apps-track-integration-account-custom-tracking-schema)|Microsoft.Logic/workflows|Modul runtime pracovního postupu|
|||Microsoft.Logic/integrationAccounts|IntegrationAccountTrackingEvents|
|Azure Batch|[Protokolování diagnostiky Azure Batch](https://docs.microsoft.com/en-us/azure/batch/batch-diagnostics)|Microsoft.Batch/batchAccounts|ServiceLog|
|Azure Automation|[Analýzy protokolů pro Azure Automation.](https://docs.microsoft.com/en-us/azure/automation/automation-manage-send-joblogs-log-analytics)|Microsoft.Automation/automationAccounts|JobLogs|
|||Microsoft.Automation/automationAccounts|JobStreams|
|Event Hubs|[Diagnostické protokoly služby Azure Event Hubs](https://docs.microsoft.com/en-us/azure/event-hubs/event-hubs-diagnostic-logs)|Microsoft.EventHub/namespaces|ArchiveLogs|
|||Microsoft.EventHub/namespaces|OperationalLogs|
|Stream Analytics|[Diagnostické protokoly úlohy](https://docs.microsoft.com/en-us/azure/stream-analytics/stream-analytics-job-diagnostic-logs)|Microsoft.StreamAnalytics/streamingjobs|Provádění|
|||Microsoft.StreamAnalytics/streamingjobs|Vytváření obsahu|
|Service Bus|[Diagnostické protokoly služby Azure Service Bus](https://docs.microsoft.com/en-us/azure/service-bus-messaging/service-bus-diagnostic-logs)|Microsoft.ServiceBus/namespaces|OperationalLogs|

### <a name="azure-active-directory-reporting"></a>Generování sestav Azure Active Directory
Azure Active Directory (Azure AD) obsahuje různé sestavy zabezpečení, aktivit a auditu pro váš adresář. Hello [Azure Active Directory sestava auditu](https://docs.microsoft.com/azure/active-directory/active-directory-reporting-guide) pomáhá zákazníkům tooidentify privilegované akce, které došlo k chybě v Azure Active Directory. Privilegované akce zahrnují změny zvýšení oprávnění (například role vytvoření nebo resetování hesla), změna konfigurace zásad (třeba zásady pro hesla) nebo změny toodirectory konfigurace (například změny toodomain federation nastavení).

Hello sestavy poskytují hello záznam auditu pro název události hello, objektu actor hello, který provedl hello akce, cílový prostředek hello vliv hello změnu a hello datum a čas (ve formátu UTC). Je možné tooretrieve hello seznam událostí auditu pro Azure Active Directory prostřednictvím hello zákazníků [portál Azure](https://portal.azure.com/), jak je popsáno v [zobrazit protokoly auditu](https://docs.microsoft.com/azure/active-directory/active-directory-reporting-azure-portal). Tady je seznam hello sestavy zahrnuté:

| Sestavy zabezpečení | Sestavy aktivit | Sestavy auditu |
| :--------------- | :--------------- | :------------ |
|Přihlášení z neznámých zdrojů| Využití aplikací: souhrn| Sestava auditu adresáře|
|Přihlášení po několika neúspěších|  Využití aplikací: podrobnosti||
|Přihlášení z více geografických poloh|    Řídicí panel aplikací||
|Přihlášení z IP adres s podezřelou aktivitou|   Chyby zřizování účtů||
|Nestandardní přihlašovací aktivita|    Zařízení jednotlivých uživatelů||
|Přihlášení z možných nakažených zařízení|   Aktivity jednotlivých uživatelů||
|Uživatelé s neobvyklou přihlašovací aktivitou| Sestava aktivit skupin||
||Sestava aktivit registrace resetování hesla||
||Aktivity resetování hesla|||

Hello data z těchto sestav mohou být užitečné tooyour aplikace, jako je například systémů SIEM, auditování a nástroje business intelligence. rozhraní API poskytují programový přístup k datům toohello pomocí sady založené na REST API, vytváření sestav Hello Azure AD. Můžete volat tyto [rozhraní API](https://docs.microsoft.com/azure/active-directory/active-directory-reporting-api-getting-started) z různých programovacích jazyků a nástroje.

Události v hello sestava auditu Azure AD jsou uchovány na 180 dní.

> [!Note]
> Další informace o uchovávání dat v sestavách najdete v tématu [zásady uchování sestav Azure ve službě Active Directory.](https://docs.microsoft.com/azure/active-directory/active-directory-reporting-retention)

Pro zákazníky ukládat svoje události auditu delší dobu uchování, hello Reporting rozhraní API může být použít vyžádání tooregularly [události auditu](https://docs.microsoft.com/azure/active-directory/active-directory-reporting-audit-events) do samostatné datové úložiště.

### <a name="virtual-machine-logs-using-azure-diagnostics"></a>Virtuální počítač protokolů pomocí Azure Diagnostics
[Azure Diagnostics](https://docs.microsoft.com/azure/azure-diagnostics) je funkce hello v rámci Azure, která umožňuje hello shromažďování diagnostických dat na nasazené aplikace. Můžete použít rozšíření diagnostiky hello z několika různých zdrojů. Aktuálně podporované jsou [webové služby Azure Cloud a rolí pracovního procesu](https://docs.microsoft.com/azure/cloud-services/cloud-services-choose-me),

![Virtuální počítač protokolů pomocí Azure Diagnostics](./media/azure-log-audit/azure-log-audit-fig3.png)

[Virtuální počítače Azure](https://azure.microsoft.com/documentation/learning-paths/virtual-machines/) systémem Microsoft Windows a [Service Fabric](https://docs.microsoft.com/azure/service-fabric/service-fabric-overview).

Můžete povolit diagnostiky Azure na virtuální počítač pomocí následující:

-   Pomocí sady Visual Studio, najdete v části [pomocí aplikace Visual Studio tootrace virtuální počítače Azure](https://docs.microsoft.com/azure/vs-azure-tools-debug-cloud-services-virtual-machines)

-   [Nastavení Azure Diagnostics na služby Azure Virtual Machine vzdáleně](https://docs.microsoft.com/azure/virtual-machines-dotnet-diagnostics)

-   [Použití prostředí PowerShell tooset až diagnostiky ve virtuálních počítačích Azure](https://docs.microsoft.com/azure/virtual-machines/virtual-machines-windows-ps-extensions-diagnostics?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)

-   [Vytvoření Windows virtuálního počítače s monitorování a Diagnostika pomocí šablony Azure Resource Manageru](https://docs.microsoft.com/azure/virtual-machines/virtual-machines-windows-extensions-diagnostics-template?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)

### <a name="storage-analytics"></a>Storage Analytics
[Azure Storage Analytics](https://docs.microsoft.com/rest/api/storageservices/fileservices/storage-analytics) provádí protokolování a poskytuje data metriky pro účet úložiště. Můžete použít tento tootrace požadavky na data, analyzovat trendy využití a diagnostikovat problémy s vaším účtem úložiště. Analýza protokolování úložiště je k dispozici pro hello [službám Blob, fronty a tabulky.](https://docs.microsoft.com/azure/storage/storage-introduction) Úložiště analýzy protokolů podrobné informace o službě úložiště tooa úspěšné i neúspěšné požadavky.

Tato informace může být použité toomonitor jednotlivých požadavků a toodiagnose problémy se službou úložiště. Na základě typu best effort protokolované požadavky. Položky protokolu se vytvoří pouze v případě, že existují požadavků na koncový bod služby hello přístup. Například pokud má aktivita v svůj koncový bod objektu Blob účet úložiště, ale není v jeho tabulku nebo frontu koncových bodů, pouze protokoly, která se týkají se vytvoří toohello služby objektů Blob.

toouse analytika úložiště, musíte povolit samostatně pro každou službu chcete toomonitor. Můžete ji povolit v hello [portál Azure](https://portal.azure.com/); podrobnosti najdete v tématu [monitorování účtu úložiště v hello portálu Azure.](https://docs.microsoft.com/azure/storage/storage-monitor-storage-account) Můžete také povolit Storage Analytics prostřednictvím kódu programu prostřednictvím hello REST API nebo knihovny klienta hello. Použijte hello nastavit vlastnosti služby operaci tooenable Storage Analytics samostatně pro každou službu.

Hello agregovaná data se ukládají v dobře známé objektu blob (pro protokolování) a dobře známé tabulky (pro metriky), které můžete získat přístup pomocí služby objektů Blob hello a služby Table rozhraní API.

Analytika úložiště může mít 20 TB na hello množství uložených dat, která je nezávislá hello celkový limit pro váš účet úložiště. Všechny protokoly jsou uloženy v [objekty BLOB bloků](https://docs.microsoft.com/azure/storage/storage-analytics) v kontejneru nazvaném $logs, které automaticky vytvářejí, když je pro účet úložiště povolená analytika úložiště.

> [!Note]
> Další informace o fakturaci a zásad uchovávání dat najdete v tématu [Storage Analytics a fakturace.](https://docs.microsoft.com/rest/api/storageservices/fileservices/storage-analytics-and-billing)
>
> [!Note]
> Další informace o limity účtu úložiště najdete v tématu [Azure úložiště škálovatelnost a cíle výkonnosti.](https://docs.microsoft.com/azure/storage/storage-scalability-targets)

jsou protokolovány Hello následující typy požadavků na ověření a anonymní.



| Ověření  | Anonymní|
| :------------- | :-------------|
| Úspěšné požadavky | Úspěšné požadavky |
|Neúspěšné požadavky, včetně vypršení časového limitu, omezení šířky pásma, sítě, autorizace a dalších chyb | Požadavky pomocí sdíleného přístupového podpisu (SAS), včetně žádostí úspěšné a neúspěšné |
| Požadavky pomocí sdíleného přístupového podpisu (SAS), včetně žádostí úspěšné a neúspěšné |Chyby časového limitu pro klienta a serveru |
|   Data tooanalytics požadavků |    Neúspěšné požadavky GET s kódem chyby 304 (upraveno) |
| Požadavky na úložiště Analytics samostatně, jako je například protokol vytvoření nebo odstranění, se neprotokolují. Úplný seznam hello protokolovat data je popsána v hello [stavové zprávy a Storage Analytics protokolované](https://docs.microsoft.com/rest/api/storageservices/fileservices/storage-analytics-logged-operations-and-status-messages) a [úložiště analýzy protokolů formátu](https://docs.microsoft.com/rest/api/storageservices/fileservices/storage-analytics-log-format) témata. | Všechny ostatní selhání anonymních požadavků se neprotokolují. Úplný seznam hello protokolovat data je popsána v hello [stavové zprávy a Storage Analytics protokolované](https://docs.microsoft.com/rest/api/storageservices/fileservices/storage-analytics-logged-operations-and-status-messages) a [úložiště analýzy protokolů formátu](https://docs.microsoft.com/rest/api/storageservices/fileservices/storage-analytics-log-format). |

### <a name="azure-networking-logs"></a>Azure síťové protokoly
Sítě protokolování a monitorování v Azure je komplexní a zahrnuje dvě rozsáhlé kategorie:

-   [Sledovací proces sítě](https://docs.microsoft.com/azure/network-watcher/network-watcher-monitoring-overview#network-watcher) -monitorování na základě scénáře sítě se poskytuje s funkcí hello v sledovací proces sítě. Tato služba obsahuje zachytáváním paketů, další směrování, IP tok ověření, zobrazení skupiny zabezpečení, tok protokolů NSG. Scénář úrovně monitorování obsahuje zobrazení tooend end síťovým prostředkům v monitorování kontrast tooindividual sítě prostředků.

-   [Sledování prostředků](https://docs.microsoft.com/azure/network-watcher/network-watcher-monitoring-overview#network-resource-level-monitoring) -sledování na úrovni prostředků se skládá ze čtyř funkce, diagnostické protokoly, metriky, řešení potíží s a stav prostředků. Všechny tyto funkce jsou postavené na úrovni prostředků síťového hello.

![Azure síťové protokoly](./media/azure-log-audit/azure-log-audit-fig4.png)

Sledovací proces sítě je místní služba, která umožňuje vám toomonitor a diagnostikovat podmínky na úrovni scénář sítě, do a z Azure. Diagnostika sítě a k dispozici sledovací proces sítě vizualizace nástroje vám pomůžou pochopit, diagnostikovat a získat statistiky tooyour sítě v Azure.

**Protokolování toku NSG** -toku protokoly pro skupinu zabezpečení sítě umožňují protokoly toocapture související se tootraffic, které jsou povolené nebo zakázané pravidla zabezpečení hello ve skupině hello. Tyto protokoly toku jsou zapsané ve formátu JSON a zobrazit odchozí a příchozí tok na základě za pravidlo hello toku hello síťový adaptér se vztahuje na 5 řazené kolekce členů informace o toku hello (zdroj nebo cíl, zdrojový nebo cílový Port, protokol IP) a pokud hello bylo povolené přenosy nebo byl odepřen.

### <a name="network-security-group-flow-logging"></a>Protokolování toku skupinu zabezpečení sítě

[Skupina zabezpečení sítě toku protokoly](https://docs.microsoft.com/azure/network-watcher/network-watcher-nsg-flow-logging-overview) jsou funkce sledovací proces sítě, který vám umožní tooview informace o příchozí a odchozí provoz IP prostřednictvím skupinu zabezpečení sítě. Tyto protokoly toku jsou zapsané ve formátu JSON a zobrazit odchozí a příchozí tok na základě za pravidlo hello toku hello síťový adaptér se vztahuje na 5 řazené kolekce členů informace o toku hello (zdroj nebo cíl, zdrojový nebo cílový Port, protokol IP) a pokud hello bylo povolené přenosy nebo byl odepřen.

Při toku protokoluje cílové skupiny zabezpečení sítě, nejsou zobrazeny hello stejné jako hello další protokoly. Tok protokoly se ukládají pouze v rámci účtu úložiště.

Hello stejné zásady uchovávání informací, jak je vidět další záznamy použít tooflow protokoly. Protokoly mít zásady uchovávání informací, můžete nastavit od 1 den too365 dnů. Pokud není nastavena zásady uchovávání informací, jsou navždy udržovat hello protokoly.

**Diagnostické protokoly**

Pravidelné a spontánních události jsou vytvořené síťové prostředky a protokolovány v účtech úložiště, odeslané tooan centra událostí nebo analýzy protokolů. Tyto protokoly poskytují přehled o stavu hello prostředku. Tyto protokoly můžete zobrazit v nástrojů, jako je Power BI a analýzy protokolů. jak tooview diagnostické protokoly, navštivte toolearn [analýzy protokolů.](https://docs.microsoft.com/azure/log-analytics/log-analytics-azure-networking-analytics)

![Diagnostické protokoly](./media/azure-log-audit/azure-log-audit-fig5.png)

Diagnostické protokoly jsou k dispozici pro [nástroj pro vyrovnávání zatížení](https://docs.microsoft.com/azure/load-balancer/load-balancer-monitor-log), [skupin zabezpečení sítě](https://docs.microsoft.com/azure/virtual-network/virtual-network-nsg-manage-log), trasy a [Application Gateway.](https://docs.microsoft.com/azure/application-gateway/application-gateway-diagnostics)

Sledovací proces sítě poskytuje že diagnostické protokoly zobrazení. Toto zobrazení obsahuje všechny síťové prostředky, které podporují protokolování diagnostiky. Z tohoto hlediska můžete povolit nebo zakázat síťových prostředků, snadno a rychle.


V možnosti protokolování toopreceding přidání sledovací proces sítě má aktuálně hello následující možnosti:
- [Topologie](https://docs.microsoft.com/azure/network-watcher/network-watcher-topology-overview) -poskytuje síti úrovně zobrazení zobrazující hello různé propojení a přidružení mezi síťovým prostředkům ve skupině prostředků.

- [Proměnné zachytáváním paketů](https://docs.microsoft.com/azure/network-watcher/network-watcher-packet-capture-overview) -zaznamená data paketů do/z virtuálního počítače. Pokročilé možnosti filtrování a podrobně nastavit ovládací prvky, jako je například se může tooset poskytují čas a velikost omezení versatility.hello paket dat může být uložená v úložišti objektů blob nebo na místní disk hello ve formátu CAP.

-   [Ověřuje IP toku](https://docs.microsoft.com/azure/network-watcher/network-watcher-ip-flow-verify-overview) – ověří, zda je paket povolený nebo zakázaný na základě toku informace 5 řazené kolekce členů paketu parametrů (cílovou IP adresu, zdrojové IP adresy, cílový Port, zdrojový Port a protokol). Pokud paket hello je zakázané skupiny zabezpečení, hello pravidlo a skupiny, který odepřen hello paketů je vrácena.

-   [Další směrování](https://docs.microsoft.com/azure/network-watcher/network-watcher-next-hop-overview) -určuje hello další segment směrování paketů směrovány v hello prostředky infrastruktury sítě Azure, umožňuje toodiagnose všechny nesprávně nakonfigurované uživatelem definované trasy.

-   [Zobrazení skupiny zabezpečení](https://docs.microsoft.com/azure/network-watcher/network-watcher-security-group-view-overview) -získá hello zabezpečení efektivní a použitých pravidel, která se použijí na virtuálním počítači.

-   [Brána virtuální sítě a řešení potíží s připojení](https://docs.microsoft.com/azure/network-watcher/network-watcher-troubleshoot-manage-rest) -poskytuje možnost tootroubleshoot hello brány virtuální sítě a připojení.

-   [Sítě limity předplatného](https://docs.microsoft.com/azure/network-watcher/network-watcher-monitoring-overview#network-subscription-limits) -umožňuje využití prostředků sítě tooview omezení.

### <a name="application-insight"></a>Přehled aplikace

[Application Insights](https://docs.microsoft.com/azure/application-insights/app-insights-overview) je rozšiřitelný služba Správa výkonu aplikace (APM) pro vývojáře, kteří ve více platformách. Použít toomonitor za provozu webové aplikace. Je automaticky zjišťovat anomálie výkonu. Zahrnuje výkonné analytics nástroje toohelp diagnostikovat problémy a toounderstand uživatelů ve skutečnosti při práci s vaší aplikací.

 Je navržen toohelp neustále průběžně zlepšují výkon a použitelnost.

 Funguje pro aplikace na širokou škálu platforem včetně .NET, Node.js a J2EE, hostovaný místně nebo v cloudu hello. Se integruje s váš proces devOps a nástroje pro vývoj toovarious body připojení.

![Přehled aplikace](./media/azure-log-audit/azure-log-audit-fig6.png)

Application Insights je zaměřen na hello vývojový tým toohelp pochopit, jaký je výkon aplikace a jak je používán. Monitoruje tyto parametry:

-   **Frekvence požadavků, doby odezvy a míra selhání** – Zjistěte, které stránky jsou nejoblíbenější a v kterou denní dobu a kde jsou vaši uživatelé. Zjistíte, která stránka si vede nejlépe. Pokud se při zvýšení počtu požadavků zvýší i doba odezvy a míra selhání, máte pravděpodobně potíže s prostředky.

-   **Míra závislosti, doby odezvy a míra selhání** – Zjistěte, jestli vás nezpomalují externí služby.

-   **Výjimky** – analyzovat statistiku hello agregován, nebo vyberte určité instance a přejít k podrobnostem hello trasování zásobníku a související požadavky. Hlásí se výjimky serveru i prohlížeče.

-   **Zobrazení a načítání stránek** – Tyto informace hlásí prohlížeče uživatelů.

-   **Volání AJAX** z webových stránek – frekvence, doby odezvy a míry selhání.

-   **Počty uživatelů a relací**.

-   **Čítače výkonu** ze serverových počítačů s Windows nebo Linuxem, jako je třeba CPU, paměť a využití sítě.

-   **Diagnostika hostitele** z Dockeru nebo Azure.

-   **Protokoly trasování diagnostiky** z vaší aplikace – umožňují zjistit korelaci mezi požadavky a událostmi trasování.

-   **Vlastní události a metriky** , že napíšete sami v hello klienta nebo serveru kódu tootrack obchodní události, jako jsou položek prodaných nebo hry won.

**Seznam scénáře integrace a popis:**

| Scénáře integrace | Popis |
| --------------------- | :---------- |
|[Mapa aplikace](https://docs.microsoft.com/en-us/azure/application-insights/app-insights-app-map)|Hello součásti vaší aplikace, pomocí klíčové metriky a výstrahy.||
|[Diagnostické vyhledávání pro instanci dat](https://docs.microsoft.com/en-us/azure/application-insights/app-insights-diagnostic-search)| Události vyhledávání a filtrování, jako jsou třeba požadavky, výjimky, volání závislosti, trasování protokolů a zobrazení stránek.||
|[Průzkumníku metrik pro agregovaná data](https://docs.microsoft.com/en-us/azure/application-insights/app-insights-metrics-explorer)|Prozkoumání, filtrování a segmentace agregovaných dat, jako jsou třeba frekvence požadavků, selhání a výjimek, doby odezvy a časy načtení stránek.||
|[Řídicí panely](https://docs.microsoft.com/en-us/azure/application-insights/app-insights-dashboards#dashboards)|Propojení dat z různých zdrojů a jejich sdílení s ostatními. Ideální pro více součásti aplikace a pro nepřetržitou zobrazení v týmové místnosti hello.||
|[Živý datový proud metriky](https://docs.microsoft.com/en-us/azure/application-insights/app-insights-live-stream)|Při nasazování nového sestavení, podívejte se na tyto ukazatele výkonu téměř v reálném čase toomake se, že vše funguje podle očekávání.||
|[Analýzy](https://docs.microsoft.com/en-us/azure/application-insights/app-insights-analytics)|Tento výkonný dotazovací jazyk umožňuje odpovědět na složité dotazy týkající se využití a výkonu vaší aplikace.||
|[Automatickou a ruční výstrahy](https://docs.microsoft.com/en-us/azure/application-insights/app-insights-alerts)|Automatické výstrahy přizpůsobit tooyour aplikace normální vzorce telemetrie a aktivační událost, pokud má něco mimo obvykle vzor hello. Výstrahy se také dají nastavit pro konkrétní úrovně vlastních nebo standardních metrik.||
|[Visual Studio](https://docs.microsoft.com/en-us/azure/application-insights/app-insights-visual-studio)|Zobrazit data výkonu v kódu hello. Přechod toocode z trasování zásobníku.||
|[Power BI](https://docs.microsoft.com/en-us/azure/application-insights/app-insights-export-power-bi)|Integrujte metriky využití s ostatními funkcemi business intelligence.||
|[REST API](https://dev.applicationinsights.io/)|Napište kód toorun dotazy přes metriky a nezpracovaná data.||
|[Průběžný export](https://docs.microsoft.com/en-us/azure/application-insights/app-insights-export-telemetry)|Export dat ve formátu raw toostorage hromadně, jakmile bude přijata.||

### <a name="azure-security-center-alerts"></a>Výstrahy Azure Security Center
[Azure Security Center](https://docs.microsoft.com/azure/security-center/security-center-intro) automaticky shromažďuje, analyzuje a integruje data protokolu z vaše prostředky Azure, sítě hello a připojených partnerských řešení, jako jsou brány firewall a endpoint protection řešení, toodetect skutečné hrozby a snížit falešně pozitivních zjištění. Seznam upřednostňovaných výstrah zabezpečení se zobrazí v Centru zabezpečení společně s hello informace, které potřebujete tooquickly prozkoumat hello problému a doporučení, jak tooremediate útoku.

Detekce hrozeb Security Center funguje tak, že automaticky shromažďování informací o zabezpečení z vaše prostředky Azure, sítě hello a připojených partnerských řešení. Analyzuje tyto informace, často korelace informace z více zdrojů, tooidentify hrozeb. Výstrahy zabezpečení budou mít vyšší prioritu v Centru zabezpečení včetně doporučení, na tom, jak tooremediate hello hrozeb.

![Azure Security Center](./media/azure-log-audit/azure-log-audit-fig7.png)

Služba Security Center využívá pokročilou analýzu zabezpečení, která daleko překračuje možnosti detekce založené na signaturách či příznacích. Změnám ve velkých objemů dat a [strojového učení](https://azure.microsoft.com/blog/machine-learning-in-azure-security-center/) technologie jsou události použité tooevaluate napříč hello celý cloudových prostředků infrastruktury – zjišťování hrozeb, které by bylo možné tooidentify ruční přístupy a predikci hello vývoj útoků. Do této analýzy zabezpečení patří:

-   **Integrované analýzou hrozeb:** vypadá pro hello známé nesprávnými účastníky použitím globální analýzou hrozeb z produktů společnosti Microsoft a služeb společnosti Microsoft digitální činů jednotky (DCU), hello Microsoft Security Response Center (MSRC) a externí informační kanály.

-   **Pro vypracování analýzy:** platí známé vzorce toodiscover škodlivé chování.

-   **Detekce anomálií:** používá statistické profilace toobuild historických směrného plánu. Zobrazuje výstrahy na odchylky od zavedených standardních hodnot, které odpovídají tooa potenciální vektor útoku.


Mnoho operací zabezpečení a reakce na incidenty týmy závisí na informace o zabezpečení a událostí správy (SIEM) řešení jako výchozí bod pro triaging a prošetřování výstrah zabezpečení hello. S integrací protokolů Azure můžou zákazníci synchronizovat výstrahy Security Center a události zabezpečení virtuálního počítače, shromážděné pomocí diagnostiky Azure a protokoly auditu Azure pomocí jejich analýzy protokolů nebo řešení SIEM skoro v reálném čase.


## <a name="log-analytics"></a>Log Analytics

Analýzy protokolů je služba v [Operations Management Suite (OMS)](https://docs.microsoft.com/azure/operations-management-suite/operations-management-suite-overview) který vám pomůže shromažďovat a analyzovat data generována prostředky ve vašem cloudu a místní prostředí. Získáte přehledy v reálném čase pomocí integrovaného hledání a vlastní řídicí panely tooreadily analyzovat miliony záznamů ve všech úloh a servery bez ohledu na jejich fyzické umístění.

![Log Analytics](./media/azure-log-audit/azure-log-audit-fig8.png)

V hello center Log Analytics je hello OMS úložiště, který je hostován v hello cloudu Azure. Data jsou shromažďována do úložiště hello z připojených zdrojů tak, že konfigurace zdroje dat a přidání předplatného tooyour řešení. Zdroje dat a řešení se vytvoří každý jiný záznam typy, které mají své vlastní sadu vlastností, ale může stále analyzovat společně v úložišti toohello dotazy. To vám umožní toouse hello stejné nástroje a metody toowork s různými druhy dat shromažďovaných pomocí různých zdrojů.

Připojené zdroje jsou hello počítače a další prostředky, které generují data shromažďovaná společností analýzy protokolů. To může zahrnovat agentů nainstalovaných na [Windows](https://docs.microsoft.com/azure/log-analytics/log-analytics-windows-agents) a [Linux](https://docs.microsoft.com/azure/log-analytics/log-analytics-linux-agents) počítače, které se připojují přímo nebo agentů v [připojené skupiny pro správu System Center Operations Manager.](https://docs.microsoft.com/azure/log-analytics/log-analytics-om-agents) Analýzy protokolů může taky shromažďovat data z [úložiště Azure.](https://docs.microsoft.com/azure/log-analytics/log-analytics-azure-storage)

[Zdroje dat](https://docs.microsoft.com/azure/log-analytics/log-analytics-data-sources) jsou hello různé druhy data shromážděná z každého připojeného zdroje. To zahrnuje události a [údaje o výkonu](https://docs.microsoft.com/azure/log-analytics/log-analytics-data-sources-performance-counters) z [Windows](https://docs.microsoft.com/azure/log-analytics/log-analytics-data-sources-windows-events) a agenty Linux v přidání toosources jako [protokoly služby IIS](https://docs.microsoft.com/azure/log-analytics/log-analytics-data-sources-iis-logs), a [protokoly vlastní text.](https://docs.microsoft.com/azure/log-analytics/log-analytics-data-sources-custom-logs) Nakonfigurujete každý zdroj dat, že chcete toocollect a konfigurace hello je automaticky doručené tooeach připojené zdroje.

Existují čtyři různé způsoby [shromažďování protokolů a metriky pro služby Azure:](https://docs.microsoft.com/azure/log-analytics/log-analytics-azure-storage)
1.  Azure diagnostics přímé tooLog Analytics (Diagnostika v hello následující tabulka)

2.  Azure diagnostics tooAzure úložiště tooLog Analytics (úložiště v hello následující tabulka)

3.  Konektory pro služby Azure (konektorů v hello následující tabulka)

4.  Skriptů toocollect a pak následná data do analýzy protokolů (prázdné buňky v následující tabulce hello a pro služby, které nejsou uvedené)

| Služba | Typ prostředku | Logs | Metriky | Řešení |
| :------ | :------------ | :--- | :------ | :------- |
|Application Gateway|  Microsoft.Network/<br>applicationGateways|  Diagnostika|Diagnostika|    [Aplikace Azure](https://docs.microsoft.com/en-us/azure/log-analytics/log-analytics-azure-networking-analytics#azure-application-gateway-analytics-solution-in-log-analytics) [Analytics brány](https://docs.microsoft.com/en-us/azure/log-analytics/log-analytics-azure-networking-analytics#azure-application-gateway-analytics-solution-in-log-analytics)|
|Application insights||     konektor|  konektor|  [Application Insights](https://blogs.technet.microsoft.com/msoms/2016/09/26/application-insights-connector-in-oms/) [Connectoru (Preview)](https://blogs.technet.microsoft.com/msoms/2016/09/26/application-insights-connector-in-oms/)|
|Účty Automation|   Microsoft.Automation/<br>AutomationAccounts|    Diagnostika||       [Další informace](https://docs.microsoft.com/en-us/azure/automation/automation-manage-send-joblogs-log-analytics)|
|Účty batch|    Microsoft.Batch/<br>batchAccounts|  Diagnostika|    Diagnostika||
|Classic cloudové služby||       Úložiště||       [Další informace](https://docs.microsoft.com/en-us/azure/log-analytics/log-analytics-azure-storage-iis-table)|
|Kognitivní služby|    Microsoft.CognitiveServices/<br>accounts|       Diagnostika|||
|Data Lake analytics|   Microsoft.DataLakeAnalytics/<br>accounts|   Diagnostika|||
|Úložiště data Lake store|   Microsoft.DataLakeStore/<br>accounts|   Diagnostika|||
|Názvový prostor události rozbočovače|   Microsoft.EventHub/<br>obory názvů|  Diagnostika|    Diagnostika||
|Centra IoT|  Microsoft.Devices/<br>IotHubs||     Diagnostika||
|Key Vault| Microsoft.KeyVault/<br>trezory|  Diagnostika  || [KeyVault Analytics](https://docs.microsoft.com/en-us/azure/log-analytics/log-analytics-azure-key-vault)|
|Nástroje pro vyrovnávání zatížení|    Microsoft.Network/<br>loadBalancers|    Diagnostika|||
|Logic Apps|    Microsoft.Logic/<br>Pracovní postupy|  Diagnostika|    Diagnostika||
||Microsoft.Logic/<br>integrationAccounts||||
|Network Security Groups (Skupiny zabezpečení sítě)|   Microsoft.Network/<br>networksecuritygroups|Diagnostika||   [Skupina zabezpečení sítě Azure Analytics](https://docs.microsoft.com/en-us/azure/log-analytics/log-analytics-azure-networking-analytics#azure-network-security-group-analytics-solution-in-log-analytics)|
|Trezory zotavení|   Microsoft.RecoveryServices/<br>trezory|||[Azure Recovery Services Analytics (Preview)](https://github.com/krnese/AzureDeploy/blob/master/OMS/MSOMS/Solutions/recoveryservices/)|
|Služby hledání|   Microsoft.Search/<br>searchServices|    Diagnostika|    Diagnostika||
|Obor názvů Service Bus| Microsoft.ServiceBus/<br>obory názvů|    Diagnostika|Diagnostika|    [Service Bus Analytics (Preview)](https://github.com/Azure/azure-quickstart-templates/tree/master/oms-servicebus-solution)|
|Service Fabric||       Úložiště||    [Služba Fabric Analytics (Preview)](https://docs.microsoft.com/en-us/azure/log-analytics/log-analytics-service-fabric)|
|SQL (v12)| Microsoft.Sql/<br>servery nebo<br>databáze||       Diagnostika||
||Microsoft.Sql/<br>servery nebo<br>elasticPools||||
|Úložiště|||         Skript| [Azure Storage Analytics (Preview)](https://github.com/Azure/azure-quickstart-templates/tree/master/oms-azure-storage-analytics-solution)|
|Virtuální počítače|  Microsoft.Compute/<br>virtuálních počítačů|  Linka|  Linka||
||||Diagnostika||
|Sady škálování virtuálních počítačů|   Microsoft.Compute/<br>virtuálních počítačů    ||Diagnostika||
||Microsoft.Compute/<br>virtualMachineScaleSets /<br>virtuálních počítačů||||
|Webové serverové farmy|Microsoft.Web/<br>serverfarms||   Diagnostika
|Weby| Microsoft.Web/<br>weby ||      Diagnostika|    [Další informace](https://github.com/Azure/azure-quickstart-templates/tree/master/101-webappazure-oms-monitoring)|
||Microsoft.Web/<br>weby nebo<br>sloty|||||


## <a name="log-integration-with-on-premises-siem-systems"></a>Integrace protokolu s místních systémů SIEM
[Integrace Azure protokolu](https://www.microsoft.com/download/details.aspx?id=53324) vám umožní toointegrate nezpracovaná protokoly z vašich prostředků Azure v tooyour místním **systémy informace o zabezpečení a událostí správy (SIEM)**.

![Integrace protokolu](./media/azure-log-audit/azure-log-audit-fig9.png)

Integrace Azure protokolu shromažďuje Azure Diagnostics ze systému Windows (WAD) virtuálních počítačů, protokoly aktivity Azure, Azure Security Center výstrahy, a protokoly poskytovatel prostředků Azure. Tato integrace poskytuje jednotný řídicí panel pro všechny vaše prostředky, místně nebo v cloudu hello, takže můžete agregovat, korelovat, analyzovat a výstrahy pro události zabezpečení.



Integrace protokolů Azure aktuálně podporuje integraci Azure aktivity protokolů, protokolu událostí systému Windows z virtuálního počítače Windows ve vašem předplatném Azure, Azure Security Center výstrahy, Azure diagnostické protokoly a Azure Active Directory protokoly auditu.

| Typ protokolu | Podpora formátu JSON (Splunk, ArcSight, Qradar) analýzy protokolů |
| :------- | :-------------------------------------------------------- |
|Protokoly auditu AAD|    Ano|
|Protokoly aktivit| Ano|
|ASC výstrahy |Ano|
|Diagnostické protokoly (protokoly prostředků)|  Ano|
|Protokoly virtuálních počítačů|   Ano prostřednictvím předané události a ne prostřednictvím formátu JSON|


Hello následující tabulka popisuje kategorie hello protokolu a podrobnosti integrace systému SIEM.

[Začínáme s Azure protokolu integrace](https://docs.microsoft.com/azure/security/security-azure-log-integration-get-started) – kurz vás provede procesem instalace integrace protokolů Azure a integrace protokoly z úložiště Azure WAD, protokoly aktivity Azure, Azure Security Center výstrahy a Azure Active Directory protokoly auditu.

Scénáře integrace

-   [Partner kroky konfigurace](https://blogs.msdn.microsoft.com/azuresecurity/2016/08/23/azure-log-siem-configuration-steps/) – tento příspěvek blogu ukazuje, jak tooconfigure Azure protokolu toowork integrace s partnerských řešení Splunk, HP ArcSight a IBM QRadar.

-   [Protokolů Azure integrace nejčastější dotazy (FAQ)](https://docs.microsoft.com/azure/security/security-azure-log-integration-faq) – nejčastější dotazy týkající se tento odpovědi dotazy týkající se integrace protokolů Azure.

-   [Integrace Security Center výstrahy s Azure protokolu integrace](https://docs.microsoft.com/azure/security-center/security-center-integrating-alerts-with-log-integration) – tento dokument ukazuje, jak toosync Security Center výstrahy, společně s shromážděných pomocí diagnostiky Azure a protokoly auditu Azure s vaší analýzy protokolů událostí zabezpečení virtuálního počítače nebo Řešení SIEM.

## <a name="next-steps"></a>Další kroky

- [Auditování a protokolování](https://www.microsoft.com/trustcenter/security/auditingandlogging)

Ochrana dat údržbou viditelnost a zajistit rychlou tootimely výstrahy zabezpečení

- [Protokolování zabezpečení a kolekce protokolu auditování v rámci Azure](https://azure.microsoft.com/resources/videos/security-logging-and-audit-log-collection/)

Nastavení, které budete potřebovat tooenforce toomake se, že jsou vaše Azure instance shromažďování hello správné zabezpečení a protokoly auditu.

- [Konfigurovat nastavení auditování pro kolekci webů](https://support.office.com/article/Configure-audit-settings-for-a-site-collection-A9920C97-38C0-44F2-8BCB-4CF1E2AE22D2?ui=&rs=&ad=US)

Jako správce kolekce webů jeden můžete načíst hello historii akcí provedených konkrétním uživatelem a můžete také načíst historii hello akcích provedených během určité období. 

- [Protokol auditování hello vyhledávání v hello zabezpečení Office 365 & centru dodržování předpisů](https://support.office.com/article/Search-the-audit-log-in-the-Office-365-Security-Compliance-Center-0d4d0f35-390b-4518-800e-0c7ec95e946c?ui=&rs=&ad=US)

Jeden můžete použít hello centru dodržování předpisů a zabezpečení Office 365 toosearch hello jednotná auditu protokolu tooview uživatele a správce aktivity ve vaší organizaci služeb Office 365.


