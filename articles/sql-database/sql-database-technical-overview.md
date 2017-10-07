---
title: "aaaWhat je hello služby Azure SQL Database? | Dokumentace Microsoftu"
description: "Získat Úvod tooSQL databáze: technické podrobnosti a možností společnosti Microsoft relační databáze systému správy (RDBMS) v cloudu hello."
keywords: "Úvod toosql, toosql úvod, co je sql database"
services: sql-database
documentationcenter: 
author: CarlRabeler
manager: jhubbard
editor: cgronlun
ms.assetid: c561f600-a292-4e3b-b1d4-8ab89b81db48
ms.service: sql-database
ms.custom: overview
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: data-management
ms.date: 06/30/2017
ms.author: carlrab
ms.openlocfilehash: 24ca383ad7e140133d19debc8899f172ee4aa424
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="what-is-hello-azure-sql-database-service"></a>Co je služba Azure SQL Database hello? 

SQL Database je služba relačních databází pro obecné účely v Microsoft Azure, která podporuje struktury, jako jsou relační data, JSON, prostorová data a XML. Zajišťuje [dynamicky škálovatelný výkon](sql-database-service-tiers.md) a nabízí možnosti jako [indexy columnstore](https://docs.microsoft.com/sql/relational-databases/indexes/columnstore-indexes-overview) pro extrémní analytické analýzy a generování sestav nebo [OLTP v paměti](sql-database-in-memory.md) pro extrémní zpracování transakcí. Společnost Microsoft zpracovává všechny opravy a aktualizace kódu SQL hello základní bezproblémově a rychle abstrahuje veškerá Správa hello základní infrastruktury. 

Databáze SQL sdílí svůj kód s hello [databázový stroj Microsoft SQL Server](https://docs.microsoft.com/sql/sql-server/sql-server-technical-documentation). Strategie první cloudu společnosti Microsoft hello nejnovější schopnosti systému SQL Server jsou vydaná první tooSQL databáze a pak tooSQL samotný Server. Tento přístup vám poskytne hello nejnovější funkce SQL Server s bez režie pro opravy nebo upgrade - a tyto nové funkce testovány mezi miliony databáze. Pokud se chcete o nových funkcích dozvědět hned po jejich oznámení, podívejte se na:

- **[Pro databázi SQL Azure plán](https://azure.microsoft.com/roadmap/?category=databases)**: místní toofind co je nového a co Připravujeme Další. 
- **[Blog o službě Azure SQL Database:](https://azure.microsoft.com/blog/topics/database)** Místo, kde členové produktového týmu SQL Serveru píší o novinkách a funkcích služby SQL Database. 

SQL Database nabízí předvídatelný výkon na několika úrovních služeb, které poskytují dynamickou škálovatelnost bez přerušení provozu, integrovanou inteligentní optimalizaci, globální škálovatelnost a dostupnost a pokročilé možnosti zabezpečení – to vše téměř bez nutnosti jakékoli správy. Tyto funkce umožňují toofocus na rychlý vývoj aplikací a urychlení vaší toomarket času, než přidělování drahocenný čas a prostředky toomanaging virtuálních počítačů a infrastruktury. Hello SQL Database služba je aktuálně v 38 dat soustředí kolem hello, world, s více datovými centry pravidelně uveden do režimu online, které vám umožní toorun databáze v datovém centru okolo vás.

> [!NOTE]
> Informace o bezpečnosti samotné platformy Azure najdete v [Centru zabezpečení Azure](https://azure.microsoft.com/support/trust-center/security/).
>

## <a name="scalable-performance-and-pools"></a>Škálovatelnost výkonu a fondy

Ve službě SQL Database je každá databáze izolovaná od všech ostatních a má vlastní [úroveň služeb](sql-database-service-tiers.md) s garantovanou úrovní výkonu. Databáze SQL poskytuje úrovně různých výkonu pro různé požadavky a umožňuje databáze ve fondu toomaximize hello toobe využití prostředků a ušetřit peníze.

### <a name="adjust-performance-and-scale-without-downtime"></a>Úprava výkonu a škálování bez výpadků

SQL Database nabízí čtyři úrovně služeb toosupport lightweight tooheavyweight nejnáročnějším: Basic, Standard, Premium a Premium RS. Můžete sestavit svoji první aplikaci na malé, jedné databáze za nízkou cenu za měsíc a potom změnit jeho úroveň služby ručně nebo z programu na všechny čas toomeet hello potřebám vašeho řešení. Můžete upravit výkonu bez výpadku tooyour aplikace nebo tooyour zákazníků. Umožňuje dynamické škálovatelnost vaší databáze tootransparently reagovat toorapidly měnící se požadavky na prostředky a umožňuje vám tooonly platit hello prostředky, je nutné, když je potřebujete.

   ![škálování](./media/sql-database-what-is-a-dtu/single_db_dtus.png)

### <a name="elastic-pools-toomaximize-resource-utilization"></a>Využití prostředků toomaximize elastické fondy

Mnoha firmám a aplikacím je možné toocreate izolované databáze a vyžádané volání výkonu nahoru nebo dolů o dostatečný údaj, zejména v případě, že jsou vzorce používání relativně předvídatelné. Ale pokud máte vzorce nepředvídatelné použití, může být pevný toomanage náklady a obchodní model. [Elastické fondy](sql-database-elastic-pool.md) jsou navrženou toosolve tento problém. Hello princip je jednoduchý. Přidělte fond tooa prostředky výkonu než může jedna databáze a platit pro prostředky celkový výkon poskytovaný hello hello fondu a nikoli pro výkon jednotlivých databází. 

   ![elastické fondy](./media/sql-database-what-is-a-dtu/sqldb_elastic_pools.png)

Elastické fondy nemusíte toofocus na vytáčení výkonu databáze vertikálně a horizontálně podle požadavků na prostředky kolísá. Hello databáze ve fondu spotřebovávat prostředky výkonu hello hello elastického fondu podle potřeby. Ve fondu databáze využívat, ale nepřekračují omezení hello hello fondu, takže vaše náklady budou předvídatelné, i v případě, že využití jednotlivé databáze nepodporuje. Co je více, můžete [přidávat a odebírat databáze toohello fondu](sql-database-elastic-pool-manage-portal.md), škálovat aplikace od několik databází toothousands vše v rámci rozpočtu, který ovládáte. Můžete také ovládací prvek hello minimální a maximální prostředky k dispozici toodatabases v tooensure fondu hello žádná databáze ve fondu hello používá veškerou hello fondu prostředků a že má každá databáze ve fondu zaručené minimální objem prostředků. toolearn Další informace o návrhových schématech aplikací SaaS pomocí elastické fondy, najdete v části [návrhová schémata pro víceklientské aplikace SaaS ve službě SQL Database](sql-database-design-patterns-multi-tenancy-saas-applications.md).

### <a name="blend-single-databases-with-pooled-databases"></a>Kombinace izolovaných databází s databázemi ve fondu

Ať už si vyberete kteroukoli cestu – izolované databáze nebo elastické fondy – nejste k ní odsouzeni na věčné časy. Inovativně izolované databáze s elastické fondy a snadno a rychle změnit hello úrovně služeb izolovaných databází i elastické fondy tooadapt tooyour situaci. Pomocí hello výkonem a dostupností Azure je možné kombinace a match jiných Azure services s SQL Database toomeet potřebám, jednotky náklady a efektivity prostředků návrhu aplikace jedinečný a odemknutí nové obchodní příležitosti.

### <a name="extensive-monitoring-and-alerting-capabilities"></a>Rozsáhlé monitorování a možnosti upozorňování

Ale jak můžeme srovnávat relativní výkon hello izolované databáze i elastické fondy? Jak poznáme hello klikněte pravým tlačítkem na zastavení při přidávání nebo ubírání? Použít hello [předdefinované performance monitoring pro aplikace](sql-database-performance.md) a [výstrahy](sql-database-insights-alerts-portal.md) nástrojů, v kombinaci s hello hodnocení výkonu na základě [jednotky transakcí databáze (Dtu) pro izolované databáze a elastické jednotky Dtu (Edtu) pro elastické fondy](sql-database-what-is-a-dtu.md). Používání těchto nástrojů, můžete rychle posoudit dopad hello škálování nahoru nebo dolů v závislosti na vaší stávající nebo projektu požadavkům na výkon. Podrobnosti viz téma [Výkon a možnosti služby SQL Database: Co je k dispozici v jednotlivých úrovních služeb](sql-database-service-tiers.md).

Kromě toho může SQL Database [generovat metriky a diagnostické protokoly](sql-database-metrics-diag-logging.md) pro snazší monitorování. Můžete nakonfigurovat využití prostředků toostore SQL Database, pracovníků a relací a připojení do jednoho z těchto prostředků Azure:

- **Azure Storage:** Pro archivaci obrovských objemů telemetrických dat za nízkou cenu.
- **Centrum událostí Azure:** Pro integraci telemetrických dat služby SQL Database s vlastními řešeními monitorování nebo aktivními kanály.
- **Azure Log Analytics:** Pro integrované řešení monitorování s možnostmi pro generování sestav, upozorňování a omezení rizik.

    ![Architektura](./media/sql-database-metrics-diag-logging/architecture.png)

## <a name="availability-capabilities"></a>Možnosti dostupnosti

Dostupnost služby Azure se smlouvou o úrovní služeb [(SLA)](http://azure.microsoft.com/support/legal/sla/) dosahuje špičkové hodnoty 99,99 %, protože staví na globální síti Microsoftem spravovaných datových center. Může tedy udržet vaše aplikace v nepřetržitém provozu každý den po celý rok. Kromě toho SQL Database nabízí integrované funkce pro [provozní kontinuitu a globální škálovatelnost](sql-database-business-continuity.md), mezi které patří:

- **[Automatické zálohování:](sql-database-automated-backups.md)** SQL Database automaticky provádí úplné a rozdílové zálohování a zálohování protokolů transakcí.
- **[Obnovení bodu v čase](sql-database-recovery-using-backups.md)**: databáze SQL podporuje obnovení tooany bod v čase za dobu automatické zálohování uchování hello.
- **[Aktivní geografickou replikací](sql-database-geo-replication-overview.md)**: SQL Database umožňuje tooconfigure až toofour čitelný sekundární databáze v buď hello stejné nebo globálně distribuované datových center Azure.  Například pokud máte aplikaci SaaS katalogu databázi, která má k velkému počtu souběžných transakcí jen pro čtení, používá aktivní geografickou replikací tooenable globální číst škálování a odeberte kritická místa na hello primární, které jsou z důvodu tooread úlohy. 
- **[Převzetí služeb při selhání skupiny](sql-database-geo-replication-overview.md)**: SQL Database vám umožní tooenable vysokou dostupnost a vyrovnávání zátěže v globálním měřítku, včetně transparentní geografická replikace a převzetí služeb při selhání velkých sad databáze i elastické fondy. Převzetí služeb při selhání skupiny a aktivní geografickou replikací umožňuje vytvoření globálně distribuované aplikace SaaS ve službě minimální správu režie ponechat všechny hello komplexní monitorování, směrování a tooSQL orchestration převzetí služeb při selhání databáze.

## <a name="built-in-intelligence"></a>Integrované inteligentní funkce

Pomocí SQL Database získáte vestavěné inteligentní, která umožňuje výrazně snížit náklady hello, spouštění a správu databází a maximalizuje výkon a zabezpečení vaší aplikace. Spuštění úlohy around-the-clock miliony zákazníka, databáze SQL shromažďuje a zpracovává obrovské množství telemetrická data, při také plně respektování ochrana osobních údajů zákazníků pozadí hello. Různé algoritmy nepřetržitě hodnocení hello telemetrická data, aby mohli hello služby Další a přizpůsobit s vaší aplikací. Na základě této analýzy hello služby se zobrazí s výkonem zlepšení doporučení šité na míru tooyour konkrétní úlohu. 

### <a name="automatic-performance-tuning"></a>Automatická optimalizace výkonu

Databáze SQL obsahuje podrobný přehled o hello dotazy, je nutné, aby toomonitor. Databáze SQL dozví o vašim vzorům databáze a umožňuje vám tooadapt úlohy tooyour schématu databáze. SQL Database poskytuje doporučení pro optimalizaci výkonu pomocí funkce [SQL Database Advisor](sql-database-advisor.md), kde můžete zkontrolovat akce optimalizace a použít je. Neustálé monitorování databáze je však náročný a zdlouhavý úkol, zejména při práci s mnoha databázemi. Správa velký počet databáze může být možné toodo efektivně i se všemi dostupnou nástroje a sestavy, které obsahují databáze SQL a portálu Azure. Místo sledování a ladění databázi ručně, můžete zvážit některé hello sledování a ladění akce tooSQL delegování databáze pomocí funkce Automatické ladění. SQL Database automaticky používat doporučení, testů a ověří, že každý z jeho vyladění výkonu hello tooensure akce udržuje zlepšení. Tímto způsobem přizpůsobuje SQL Database automaticky tooyour zatížení řízené a bezpečné způsobem. Automatické ladění znamená, že pečlivě monitorovat a před a po každé vyladění akce hello výkon vaší databáze, a pokud nedojde ke zlepšení výkonu hello, je vrácen hello ladění akce.

Dnes, mnoho našich partnerů systémem [víceklientské aplikace SaaS](sql-database-design-patterns-multi-tenancy-saas-applications.md) nad databáze SQL se spoléhat na automatické ladění toomake svých aplikací mít vždycky stabilní a předvídatelné výkonu výkonu. Pro ně tato funkce vytváření vyžadovalo nadměrné snižuje riziko hello použití výkonu incidentu hello uprostřed noci hello. Navíc vzhledem k tomu, že součást jejich zákaznická základna také používá systém SQL Server, používají hello stejná doporučení pro indexování poskytované SQL Database toohelp zákazníků systému SQL Server.

Ve službě SQL Database existují dva dostupné aspekty automatické optimalizace:

- **[Automatická správa indexů:](sql-database-automatic-tuning.md#automatic-index-management)** Identifikuje indexy, které by se měly do databáze přidat nebo z ní naopak odebrat.
- **[Automatická oprava plánů:](sql-database-automatic-tuning.md#automatic-plan-choice-correction)** Identifikuje problematické plány a řeší problémy s výkonem plánu SQL (již brzy, aktuálně k dispozici v SQL Serveru 2017).

### <a name="adaptive-query-processing"></a>Adaptivní zpracování dotazů

Také přidáváme hello [zpracování adaptivní dotazu](/sql/relational-databases/performance/adaptive-query-processing) řadu funkcí tooSQL databázi, včetně prokládaná spuštění vícepříkazové funkce vracející tabulku, dávky režimu paměti grant zpětnou vazbu a dávky režimu adaptivní spojení . Každý z těchto funkcí zpracování adaptivní dotazu platí podobné "Další informace a přizpůsobit" techniky, pomáhá další problémy optimalizace intractable dotazu související toohistorically problémy s výkonem adresu.

### <a name="intelligent-threat-detection"></a>Inteligentní detekce hrozeb

 [Detekce hrozeb SQL](sql-database-threat-detection.md) využívá [auditování databáze SQL](sql-database-auditing.md) toocontinuously monitorování Azure SQL databáze pro pokusy o potenciálně škodlivé tooaccess citlivá data. Detekce hrozeb SQL poskytuje novou vrstvu zabezpečení, což umožňuje zákazníkům toodetect a toopotential hrozeb reagovat, když k nim dojde tím, že poskytuje výstrahy zabezpečení na neobvyklé aktivity. Uživatelé obdrží upozornění při podezřelých databázových aktivitách, potenciálních ohroženích zabezpečení, útocích prostřednictvím injektáže SQL a neobvyklých vzorcích přístupu k databázi. SQL threat výstrahy zjištění uveďte podrobné informace o podezřelé aktivity a doporučujeme akce jak tooinvestigate a zmírnit hrozby hello. Uživatele můžete prozkoumat toodetermine podezřelé události hello, pokud hello: výsledky události z tooaccess pokusu o porušení nebo zneužití data v databázi hello. Detekce hrozeb je jednoduchý tooaddress potenciální hrozby toohello databáze bez hello nutné toobe expert zabezpečení nebo spravovat pokročilým zabezpečením monitorování systémů.

## <a name="advanced-security-and-compliance"></a>Pokročilé zabezpečení a dodržování předpisů

SQL Database nabízí celou řadu [integrované funkce zabezpečení a dodržování předpisů](sql-database-security-overview.md) toohelp aplikace splňovat různé požadavky na zabezpečení a dodržování předpisů. 

### <a name="auditing-for-compliance-and-security"></a>Auditování dodržování předpisů a zabezpečení

[Auditování databáze SQL](sql-database-auditing.md) sleduje události databáze a zapisuje tooan protokolu auditování v účtu úložiště Azure. Auditování pomáhá zajistit dodržování předpisů, porozumět databázové aktivitě a získat přehled o nesrovnalostech a anomáliích, které můžou značit problémy obchodního charakteru nebo vzbuzovat podezření na narušení zabezpečení.

### <a name="data-encryption-at-rest"></a>Šifrování v klidovém stavu

Databáze SQL [transparentní šifrování dat](https://docs.microsoft.com/sql/relational-databases/security/encryption/transparent-data-encryption-with-azure-sql-database) pomáhá chránit před ohrožením hello škodlivých aktivit provedením v reálném čase šifrování a dešifrování hello databáze, přidružených záloh a soubory protokolu transakcí v klidovém stavu bez nutnosti změny toohello aplikace. Od května 2017 jsou všechny nově vytvořené databáze Azure SQL automaticky chráněné pomocí transparentního šifrování dat (TDE). Šifrování TDE je SQL ověřené technologie šifrování na rest, která vyžadují mnoho tooprotect standardy dodržování předpisů proti krádeži úložného média. Zákazníci mohou spravovat hello TDE šifrovacích klíčů a jiné tajné způsobem zabezpečení a dodržování pomocí Azure Key Vault.

### <a name="data-encryption-in-motion"></a>Šifrování přenášených dat

Databáze SQL je hello pouze databáze systému toooffer Ochrana citlivých dat na cestě, v klidovém stavu a během zpracování pomocí dotazů [vždy šifrována](https://docs.microsoft.com/sql/relational-databases/security/encryption/always-encrypted-database-engine). Vždy je šifrovaný má odvětví první, která nabízí bezkonkurenční data zabezpečení proti porušení zahrnující hello krádež důležitá data. Například se vždycky šifrovaná, čísla platebních karet zákazníků ukládají zašifrovaná v databázi hello vždy, i během zpracování dotazu povolení dešifrování okamžiku hello použití oprávnění zaměstnanci nebo aplikace, které potřebují tooprocess tato data.

### <a name="dynamic-data-masking"></a>Dynamické maskování dat

[Databáze SQL dynamická data maskování](sql-database-dynamic-data-masking-get-started.md) omezuje zranitelnost citlivá data pomocí maskování ji uživatelé toonon úrovní oprávnění. Dynamická data maskování pomáhá zabránit neoprávněnému přístupu toosensitive data tím, že umožňuje zákazníkům toodesignate kolik hello citlivá data tooreveal s minimálním dopadem na aplikační vrstvu hello. Je funkce založená na zásadách zabezpečení, která skryje hello citlivá data v hello sadu výsledků dotazu přes pole určené databáze, zatímco hello data v databázi hello se nezmění.

### <a name="row-level-security"></a>Zabezpečení na úrovni řádku

[Zabezpečení na úrovni řádků](https://docs.microsoft.com/sql/relational-databases/security/row-level-security) umožňuje zákazníkům toocontrol přístup toorows v tabulce databáze na základě charakteristik hello hello uživatele provádění dotazu (například podle kontextu skupiny členství nebo spuštění). Zabezpečení na úrovni řádků (RLS) zjednodušuje hello návrhu a kódování zabezpečení ve vaší aplikaci. RLS umožňuje tooimplement omezení přístupu k datům řádek. Například pracovníci přístup pouze tyto řádky dat, které jsou příslušné tootheir oddělení zajistí nebo omezení zákazníka data přístup tooonly hello data relevantní tootheir společnosti.

### <a name="azure-active-directory-integration-and-multi-factor-authentication"></a>Integrace s Azure Active Directory a vícefaktorové ověřování

Databáze SQL umožňuje toocentrally můžete spravovat identity uživatele databáze a dalších služeb společnosti Microsoft s [integraci služby Azure Active Directory](sql-database-aad-authentication.md). Tato možnost zjednodušuje správu oprávnění a zvyšuje zabezpečení. Azure Active Directory podporuje [služby Multi-Factor authentication](sql-database-ssms-mfa-authentication.md) zabezpečení dat a aplikací tooincrease (MFA) zároveň podporuje jeden sing v procesu.

### <a name="compliance-certification"></a>Certifikace dodržování předpisů

Služba SQL Database se účastní pravidelných auditů a byla certifikována pro řadu standardů dodržování předpisů. Další informace najdete v tématu hello [Microsoft Azure Trust Center](https://azure.microsoft.com/support/trust-center/), kde můžete najít hello aktuální seznam [dodržování předpisů certifikátů databáze SQL](https://azure.microsoft.com/support/trust-center/services/).

## <a name="easy-to-use-tools"></a>Snadno použitelné nástroje

SQL Database zjednodušuje a zefektivňuje vytváření a správu aplikací. Databáze SQL můžete toofocus na tom, co vám nejlépe: vytváření kvalitních aplikací. Ve službě SQL Database můžete vyvíjet a provádět správu pomocí nástrojů a dovedností, které už máte.

- **[portál Azure Hello](https://portal.azure.com/)**: webová aplikace pro správu všech služeb Azure 
- **[SQL Server Management Studio](https://docs.microsoft.com/sql/ssms/download-sql-server-management-studio-ssms)**: stáhnout zdarma a klientskou aplikaci pro správu jakékoliv infrastruktury, SQL, z tooSQL systému SQL Server databáze
- **[SQL Server Data Tools v sadě Visual Studio:](https://docs.microsoft.com/sql/ssdt/download-sql-server-data-tools-ssdt)** Bezplatná klientská aplikace ke stažení pro vývoj relačních databází SQL Serveru, databází Azure SQL, balíčků služby SSIS, datových modelů služby Analysis Services a sestav služby Reporting Services.
- **[Visual Studio Code](https://code.visualstudio.com/docs)**: zdarma ke stažení open source, editor kódu pro Windows, systému macOS a Linux, který podporuje rozšíření, včetně hello [mssql rozšíření](https://aka.ms/mssql-marketplace) k dotazování systému Microsoft SQL Server Databáze Azure SQL a SQL Data Warehouse.

Databáze SQL podporuje vytváření aplikací s Python, Java, Node.js, PHP, Ruby a .NET na hello systému MacOS, Linux a Windows. Databáze SQL podporuje hello stejné [knihovny připojení](sql-database-libraries.md) jako systém SQL Server.

## <a name="engage-with-hello-sql-server-engineering-team"></a>Spojte s technickému týmu systému SQL Server hello

- [DBA na webu Stack Exchange](https://dba.stackexchange.com/questions/tagged/sql-server): Pokládání dotazů týkajících se správy databází
- [Stack Overflow](http://stackoverflow.com/questions/tagged/sql-server): Pokládání dotazů týkajících se vývoje
- [Fóra na webu MSDN](https://social.msdn.microsoft.com/Forums/en-US/home?category=sqlserver): Pokládání technických dotazů
- [Microsoft Connect](https://connect.microsoft.com/SQLServer/Feedback): Hlášení chyb a žádosti o funkce
- [Reddit](https://www.reddit.com/r/SQLServer/): Diskuze o SQL Serveru

## <a name="next-steps"></a>Další kroky

- V tématu hello [stránce s cenami](https://azure.microsoft.com/pricing/details/sql-database/) pro izolované databáze a porovnání náklady elastické fondy a kalkulačky.

- Zjistit že tyto rychlé spuštění tooget, kterou jste zahájili:

  - [Vytvoření databáze SQL v hello portálu Azure](sql-database-get-started-portal.md)  
  - [Vytvoření databáze SQL pomocí hello rozhraní příkazového řádku Azure](sql-database-get-started-cli.md)
  - [Vytvoření databáze SQL pomocí PowerShellu](sql-database-get-started-powershell.md)

- Řadu ukázek v Azure CLI a PowerShellu najdete tady:
  - [Ukázky v Azure CLI pro službu SQL Database](sql-database-cli-samples.md)
  - [Ukázky v Azure PowerShellu pro službu SQL Database](sql-database-powershell-samples.md)
