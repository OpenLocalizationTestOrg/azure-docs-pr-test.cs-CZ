---
title: aaaWhat je Azure SQL Data Warehouse? | Dokumentace Microsoftu
description: "Distribuovaná databáze podnikové třídy schopná zpracovávat petabajtové objemy relačních a nerelačních dat. Je hello odvětví první cloudový datový sklad zvětšit, zmenšit a pozastavení v sekundách."
services: sql-data-warehouse
documentationcenter: NA
author: jrowlandjones
manager: bjhubbard
editor: 
ms.assetid: 4006c201-ec71-4982-b8ba-24bba879d7bb
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: hero-article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: overview
ms.date: 2/28/2017
ms.author: jrj;barbkess
ms.openlocfilehash: 5fefe40879230f123c2e4a90b9c20a35779cf711
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="what-is-azure-sql-data-warehouse"></a>Co je Azure SQL Data Warehouse?
Azure SQL Data Warehouse je cloudová, škálovatelná, relační databáze, která dokáže zpracovávat ohromné objemy dat, postavená na architektuře MPP (Massively Parallel Processing). 

SQL Data Warehouse:

* Kombinuje relační databáze systému SQL Server hello s možností škálování cloudu Azure. 
* Odděluje úložiště od výpočetních prostředků.
* Umožňuje navýšení, snížení, pozastavení nebo obnovení výpočetních prostředků. 
* Integruje napříč hello platformy Azure.
* Využívá jazyk Transact-SQL (T-SQL) a nástroje SQL Serveru.
* Je v souladu s různými zákonnými a podnikovými požadavky na zabezpečení, jako je například SOC a ISO.

Tento článek popisuje hello klíčových funkcích služby SQL Data Warehouse.

## <a name="massively-parallel-processing-architecture"></a>Architektura MPP (Massively parallel processing)
Řešení SQL Data Warehouse je distribuovaný databázový systém, postavený na architektuře MPP (Massively Parallel Processing). Pozadí se děje hello SQL Data Warehouse rozprostírá data mezi mnoha shared nothing úložných a procesorových jednotek. Hello data se ukládají ve vrstvě Premium místně redundantní úložiště, na který dynamicky propojené výpočetní uzly spouštět dotazy. Načte "rozdělení a dobytí" toorunning přístup má SQL Data Warehouse a složitých dotazů. Požadavky jsou přijímány řídicí uzel optimalizované pro distribuci a potom předána tooCompute uzly toodo práci paralelně.

Díky oddělenému úložišti a výpočetním prostředkům může SQL Data Warehouse:

* Zvětšovat a zmenšovat úložiště nezávisle na výpočetních prostředcích
* Zvětšovat a zmenšovat výpočetní výkon bez přesouvání dat
* Pozastavit výpočetní kapacitu a zachovat neporušená data, zatímco platíte pouze za úložiště
* Obnovit výpočetní kapacitu za provozu

Hello následující diagram znázorňuje architekturu hello podrobněji.

![Architektura služby SQL Data Warehouse][1]

**Řídicí uzel:** hello řídicí uzel spravuje a optimalizuje dotazy. Je hello front-end, který komunikuje se všemi aplikacemi a připojeními. V SQL Data Warehouse hello řídicí uzel používá technologii SQL Database a připojení tooit vypadá a pracuje hello stejné. V části hello prostor hello řídicí uzel koordinuje všechny přesuny dat hello a výpočty potřebné toorun paralelní dotazy na distribuovaných datech. Při odesílání tooSQL dotazu T-SQL Data Warehouse hello řídicí uzel ho transformuje na samostatné dotazy, které běží na každém výpočetním uzlu paralelně.

**Výpočetní uzly:** hello výpočetní uzly zajišťují výkon hello služby SQL Data Warehouse. Jsou to databáze SQL, které ukládají vaše data a zpracovávají vaše dotazy. Při přidání dat SQL Data Warehouse distribuuje hello řádky tooyour výpočetních uzlů. Hello výpočetní uzly jsou hello dělníky, kteří spouští paralelní dotazy hello na vaše data. Po zpracování předají hello výsledky zpět toohello řídicí uzel. dotaz hello toofinish, hello řídicí uzel agreguje hello výsledky a vrátí hello konečný výsledek.

**Úložiště:** Data se ukládají do Úložiště objektů blob v Azure. Výpočetní uzly práci s daty, zápisu a čtení tooand přímo z úložiště objektů blob. Vzhledem k tomu, že úložiště Azure se může transparentně a významně, můžete provést SQL Data Warehouse stejné hello. Výpočty a úložiště jsou nezávislé, takže služba SQL Data Warehouse může automaticky škálovat úložiště nezávisle na škálování výpočtů, a naopak. Azure Blob storage je taky plně odolná proti chybám a zjednodušuje hello zálohování a obnovení.

**Služba pro přesun dat:** služby přesun dat (DMS) přesouvá data mezi uzly hello. Služba DMS dává hello výpočetní uzly přístup toodata, které potřebují pro spojování a agregaci. DMS není jednou ze služeb Azure. Je služba systému Windows, které běží souběžně s SQL Database na všech uzlech hello. DMS je proces na pozadí, se kterým nelze přímo interagovat. Nicméně, můžete se podívat na dotaz plány toosee při výskytu operace DMS, protože přesun dat je nutné toorun každý dotaz paralelně.

## <a name="optimized-for-data-warehouse-workloads"></a>Optimalizováno pro úlohy datového skladu
je Hello přístup MPP opírá několik datových skladů optimalizací výkonu specifických, včetně:

* Optimalizátor distribuovaných dotazů a soubor komplexních statistik napříč všemi daty. Pomocí informací na velikosti a distribuci dat, služba hello je možné toooptimize dotazy tím, že posoudí náklady hello na konkrétní operace distribuovaných dotazů.
* Pokročilé algoritmy a postupy integrované do dat hello přesun proces tooefficiently přesunout data mezi výpočetními prostředky jako nezbytné tooperform hello dotaz. Tyto operace přesunu dat jsou integrované v a všechny optimalizace toohello služba pro přesun dat dojít automaticky.
* Clusterované indexy **columnstore** ve výchozím nastavení. SQL Data Warehouse pomocí úložiště založeného na sloupcích získá v průměru 5 x větší zvýšení komprese přes tradiční úložiště orientovaným na řádky a až too10x nebo další větší zvýšení výkonu dotazů. Analytické dotazy, které je třeba tooscan velký počet řádků lépe spolupracovat s indexy columnstore.


## <a name="predictable-and-scalable-performance-with-data-warehouse-units"></a>Předvídatelný a škálovatelný výkon s jednotkami datového skladu
Služba SQL Data Warehouse je postavena na podobných technologiích jako SQL Database – to znamená, že uživatelé můžou očekávat konzistentní a předvídatelný výkon pro analytické dotazy. Uživatelé by měl očekávat toosee výkonu škálování lineárně jako jejich přidání nebo odečtena výpočetních uzlů. Přidělení prostředků tooyour SQL Data Warehouse se měří v jednotky datového skladu (Dwu). Jednotky Dwu jsou měřítkem základního prostředkům, například CPU, paměť, IOPS, které jsou přiděleny tooyour SQL Data Warehouse. Zvýšením počtu hello jednotek Dwu se zvyšuje prostředků a výkonu. Jednotky DWU přináší především tyto výhody:

* Budete mít tooscale vašeho datového skladu bez obav o hello základní hardware nebo software.
* Můžete předvídání zlepšení výkonu pro úroveň DWU před změnou hello výpočetní datového skladu.
* Hello základní hardware a software instanci služby můžete změnit nebo přesunout bez ovlivnění výkonu úloh.
* Microsoft může zvýšit hello základní architektury služby hello bez ovlivnění hello výkon vašich úloh.
* Microsoft může rychle zlepšit výkon v SQL Data Warehouse způsobem, který je škálovatelná a rovnoměrně důsledky hello systému.

Jednotky datového skladu poskytují měřítko tří metrik, které vysoce korelují s výkonem úloh datového skladu. Hello následující klíčové metriky škálování zatížení lineárně s hello Dwu.

**Prohledávání/agregace:** Standardní dotaz datového skladu, který prohledá velký počet řádků a potom provede komplexní agregaci. Tato operace je náročná na oblast vstup/výstup a prostředky procesoru.

**Zatížení:** hello možnost tooingest data do služby hello. Načtení se nejlépe provádí pomocí funkce PolyBase z Azure Storage Blob do služby Azure Data Lake. Tato metrika je navrženou toostress sítě a procesoru aspektů služby hello.

**Vytvořit tabulku jako vyberte (funkce CTAS):** funkce CTAS měří schopnost toocopy hello tabulku. To zahrnuje čtení dat z úložiště, distribuci mezi uzly hello hello zařízení a zápis toostorage znovu. Tato operace je náročná na prostředky procesoru, vstup/výstup a síťové prostředky.

## <a name="built-on-sql-server"></a>Vytvořené na serveru SQL Server
SQL Data Warehouse je založená na relační databázový stroj SQL Server hello a zahrnuje celou řadu hello funkce, které očekáváte od podnikového datového skladu. Pokud už znáte T-SQL, je snadné tootransfer vaše znalostní báze tooSQL datového skladu. Jestli máte pokročilé, nebo jenom začátek, příklady hello napříč hello dokumentaci vám pomůžou začít. Obecně se dá chápat hello způsobem, že způsob konstrukce prvků jazyka hello služby SQL Data Warehouse následujícím způsobem:

* SQL Data Warehouse používá syntaxi T-SQL pro mnoho operací. Také podporuje širokou škálu tradičních SQL konstruktorů, jako jsou uložené procedury, uživatelem definované funkce, vytváření oddílů tabulky, indexy a kolace.
* SQL Data Warehouse také obsahuje různé novější funkce SQL Serveru, k nimž patří clusterované indexy **columnstore**, integrace PolyBase a auditování dat (včetně hodnocení hrozeb).
* Některé prvky jazyka T-SQL, které jsou méně častých pro datových skladů úlohy, nebo jsou novější tooSQL serveru, nemusí být aktuálně k dispozici. Další informace najdete v tématu hello [dokumentace k migraci][Migration documentation].

S hello Transact-SQL a společného funkce SQL Server, SQL datového skladu, databáze SQL a Analytics Platform System můžete vyvinout řešení, které vyhovuje vašim datovým potřebám. Můžete rozhodnout, kde tookeep data, na základě výkonu, zabezpečení a požadavky rozsahu a potom přenos dat v případě potřeby mezi různými systémy.

## <a name="data-protection"></a>Ochrana dat
SQL Data Warehouse ukládá všechna data v úložišti Azure úrovně Premium pomocí místně redundantního úložiště. Několik synchronních kopií dat hello jsou zachována ve hello místní data center tooguarantee transparentní ochrana dat před lokálních selhání. Kromě toho SQL Data Warehouse v pravidelných intervalech automaticky zálohuje aktivní (nepozastavené) databáze pomocí snímků služby Azure Storage. toolearn informace o zálohování a obnovení funguje, najdete v části hello [Přehled zálohování a obnovení][Backup and restore overview].

## <a name="integrated-with-microsoft-tools"></a>Integrováno s nástroji společnosti Microsoft
SQL Data Warehouse taky integruje řadu nástrojů hello uživatele může být obeznámeni s systém SQL Server. Mezi tyto nástroje patří:

**Tradiční nástroje systému SQL Server:** Služba SQL Data Warehouse je plně integrovaná se Službou Analysis Services serveru SQL, SSIS a Reporting Services.

**Cloudové nástroje:** Službu SQL Data Warehouse je možné integrovat s různými službami v Azure, včetně služeb Data Factory, Stream Analytics, Machine Learning a Power BI. Úplnější seznam viz [Přehled integrovaných nástrojů][Integrated tools overview].

**Nástroje třetích stran:** Mnoho poskytovatelů nástrojů třetích stran disponuje certifikovanou integrací svých nástrojů se službou SQL Data Warehouse. Úplný seznam najdete v tématu [Partneři řešení SQL Data Warehouse][SQL Data Warehouse solution partners].

## <a name="hybrid-data-sources-scenarios"></a>Scénáře hybridních zdrojů dat
Polybase umožňuje tooleverage data z různých zdrojů pomocí známých příkazů T-SQL. Polybase umožňuje tooquery nerelační data ukládaná v úložišti objektů Blob v Azure, jako by šlo o běžnou tabulku. Použijte Polybase tooquery nerelační data, nebo tooimport nerelační data do SQL Data Warehouse.

* PolyBase používá externí tabulky tooaccess nerelační data. Hello definice tabulek jsou uložené v SQL Data Warehouse a můžete je přístup pomocí SQL a nástroje, jako je by k normálním relačním datům.
* PolyBase nedělá při integraci žádné rozdíly. Ji vystavuje hello stejné funkce a funkce tooall hello zdrojům, které podporuje. Hello data načtená polybase může být v různých formátech, včetně souborů s oddělovači a souborů ORC.
* PolyBase se dá použít tooaccess úložiště objektů blob, který se také používá jako úložiště pro cluster služby HDInsight. Toto dává vám přístup toohello stejným datům pomocí relačních a nerelačních nástrojů.

## <a name="sla"></a>SLA
SQL Data Warehouse nabízí smlouvu o úrovni služeb (SLA) na úrovni produktu jako součást smlouvy SLA pro Microsoft Online Services. Další informace najdete na stránkách [SLA pro SQL Data Warehouse][SLA for SQL Data Warehouse]. SLA informace o všech ostatních produktů můžete navštívit hello [smlouvy o úrovni služeb] Azure stránky nebo je stáhnout na hello [Volume Licensing] [ Volume Licensing] stránky. 

## <a name="next-steps"></a>Další kroky
Teď, když víte o něco o SQL Data Warehouse, zjistěte, jak tooquickly [vytvořit SQL Data Warehouse] [ create a SQL Data Warehouse] a [načíst ukázková data][load sample data]. Pokud jste nový tooAzure, můžete zjistit hello [Azure Glosář] [ Azure glossary] užitečné, protože dojde k nové terminologie. Můžete se také podívat na některé z těchto dalších zdrojů ke službě SQL Data Warehouse.  

* [Úspěšné zákaznické implementace]
* [Blogy]
* [Žádosti o funkce]
* [Videa]
* [Blogy zákaznického poradního týmu]
* [Vytvoření lístku podpory]
* [Fórum MSDN]
* [Fórum Stack Overflow]
* [Twitter]

<!--Image references-->
[1]: ./media/sql-data-warehouse-overview-what-is/dwarchitecture.png

<!--Article references-->
[Vytvoření lístku podpory]: ./sql-data-warehouse-get-started-create-support-ticket.md
[load sample data]: ./sql-data-warehouse-load-sample-databases.md
[create a SQL Data Warehouse]: ./sql-data-warehouse-get-started-provision.md
[Migration documentation]: ./sql-data-warehouse-overview-migrate.md
[SQL Data Warehouse solution partners]: ./sql-data-warehouse-partner-business-intelligence.md
[Integrated tools overview]: ./sql-data-warehouse-overview-integrate.md
[Backup and restore overview]: ./sql-data-warehouse-restore-database-overview.md
[Azure glossary]: ../azure-glossary-cloud-terminology.md

<!--MSDN references-->

<!--Other Web references-->
[Úspěšné zákaznické implementace]: https://azure.microsoft.com/case-studies/?service=sql-data-warehouse
[Blogy]: https://azure.microsoft.com/blog/tag/azure-sql-data-warehouse/
[Blogy zákaznického poradního týmu]: https://blogs.msdn.microsoft.com/sqlcat/tag/sql-dw/
[Žádosti o funkce]: https://feedback.azure.com/forums/307516-sql-data-warehouse
[Fórum MSDN]: https://social.msdn.microsoft.com/Forums/azure/en-US/home?forum=AzureSQLDataWarehouse
[Fórum Stack Overflow]: http://stackoverflow.com/questions/tagged/azure-sqldw
[Twitter]: https://twitter.com/hashtag/SQLDW
[Videa]: https://azure.microsoft.com/documentation/videos/index/?services=sql-data-warehouse
[SLA for SQL Data Warehouse]: https://azure.microsoft.com/en-us/support/legal/sla/sql-data-warehouse/v1_0/
[Volume Licensing]: http://www.microsoftvolumelicensing.com/DocumentSearch.aspx?Mode=3&DocumentTypeId=37
[smlouvy o úrovni služeb]: https://azure.microsoft.com/en-us/support/legal/sla/
