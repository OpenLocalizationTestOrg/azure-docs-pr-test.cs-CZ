---
title: "pokyny, aaaDesign replikovaných tabulek – Azure SQL Data Warehouse | Microsoft Docs"
description: "Doporučení k návrhu replikovaných tabulek v Azure SQL Data Warehouse schéma."
services: sql-data-warehouse
documentationcenter: NA
author: ronortloff
manager: jhubbard
editor: 
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: tables
ms.date: 07/14/2017
ms.author: rortloff;barbkess
ms.openlocfilehash: 5d405b8c404c65177b387ba959126839c1cf8799
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="design-guidance-for-using-replicated-tables-in-azure-sql-data-warehouse"></a>Pokyny k návrhu pro používání replikovaných tabulek v Azure SQL Data Warehouse
Tento článek obsahuje doporučení pro návrh replikovaných tabulek v SQL Data Warehouse schéma. Použijte tyto výkon dotazů tooimprove doporučení pomocí snižuje složitost dat přesouvání a dotazu.

> [!NOTE]
> Funkce replikované tabulky Hello je aktuálně ve verzi public preview. Některé chování jsou toochange subjektu.
> 

## <a name="prerequisites"></a>Požadavky
Tento článek předpokládá, že jste obeznámeni s distribuci dat a koncepty přesun dat v SQL Data Warehouse.  Další informace najdete v tématu [Distributed data](sql-data-warehouse-distributed-data.md). 

Jako součást návrh tabulky Pochopte, co nejvíce o vašich dat a jak je dotazován hello data.  Například zvažte tyto otázky:

- Jak velká je tabulka hello?   
- Jak často se aktualizují hello tabulky?   
- Je nutné provést tabulkami faktů a dimenzí v datovém skladu?   

## <a name="what-is-a-replicated-table"></a>Co je replikované tabulky?
Replikované tabulky obsahuje úplnou kopii hello tabulky, které jsou přístupné na každém výpočetním uzlu. Replikace tabulku odebere hello nutné tootransfer data mezi výpočetní uzly před spojení nebo agregace. Protože hello tabulka obsahuje více kopií, replikované tabulky fungují lépe, když velikost tabulky hello je menší než 2 GB komprimované.

Hello následující diagram znázorňuje replikované tabulky, která je přístupná na každém výpočetním uzlu. Hello replikované tabulky v SQL Data Warehouse je plně zkopírovaný tooa distribuční databázi na každém výpočetním uzlu. 

![Replikované tabulky](media/guidance-for-using-replicated-tables/replicated-table.png "replikované tabulky")  

Replikované tabulky pracovní i pro malý dimenze tabulky v hvězdicové schéma. Tabulky dimenzí obvykle jsou velikosti, která je vhodná toostore a udržovat více kopií. Dimenze ukládat popisný data, která změní pomalu, jako je například jméno zákazníka a adresu a podrobnosti o produktu. Hello pomalu změna povaha hello dat vede znovu sestaví toofewer hello replikované tabulky. 

Zvažte použití replikované tabulky, když:

- velikost Hello tabulky na disku je menší než 2 GB, bez ohledu na to hello počet řádků. velikost hello toofind tabulky, můžete použít hello [DBCC PDW_SHOWSPACEUSED](https://docs.microsoft.com/en-us/sql/t-sql/database-console-commands/dbcc-pdw-showspaceused-transact-sql) příkaz: `DBCC PDW_SHOWSPACEUSED('ReplTableCandidate')`. 
- Tabulka Hello se používá ve spojení, které by jinak vyžadovaly přesun dat. Například připojení k síti na distribuovat algoritmu hash tabulky při hello spojující sloupce nejsou hello stejný sloupec distribuční vyžaduje přesun dat. Pokud jeden z tabulky distribuovat algoritmu hash hello je malý, vezměte v úvahu replikované tabulky. Spojení v tabulce kruhového dotazování vyžaduje přesun dat. Doporučujeme používat replikované tabulky místo kruhového dotazování tabulky ve většině případů. 


Vezměte v úvahu převod existující distribuované tabulky tooa replikované tabulky, když:

- Dotaz plány pomocí operace přesunu dat, které vysílají hello data tooall hello výpočetních uzlů. Hello BroadcastMoveOperation je nákladné a zpomalí výkon dotazů. operace přesunu dat tooview v plánech dotaz, použít [sys.dm_pdw_request_steps](https://docs.microsoft.com/sql/relational-databases/system-dynamic-management-views/sys-dm-pdw-request-steps-transact-sql).
 
Replikované tabulky nemusí poskytne nejlepší výkon dotazů hello při:

- Tabulka Hello má časté vložit, aktualizovat a odstraňovat operace. Tyto operace jazyk (DML) manipulaci dat vyžadují opětovné sestavení hello replikované tabulky. Opětovné sestavení často může způsobit snížení výkonu.
- často je škálovat Hello datového skladu. Změna měřítka datového skladu změní hello počet výpočetních uzlů, které způsobuje opětovném sestavení.
- Tabulka Hello má velký počet sloupců, ale operace dat obvykle přístup pouze malý počet sloupců. V tomto scénáři, namísto replikace hello celou tabulku, může být efektivnější toohash distribuovat hello tabulky a pak vytvořit index sloupce hello často používají. Pokud je dotaz vyžaduje přesunu dat SQL Data Warehouse jen přesun dat v hello požadované sloupce. 



## <a name="use-replicated-tables-with-simple-query-predicates"></a>Použití replikované tabulky s predikáty jednoduchý dotaz
Než zvolte toodistribute nebo replikovat tabulku, vezměte v úvahu hello typy dotazů, že máte v plánu toorun s tabulkou hello. Pokud je to možné,

- Pro dotazy s predikáty jednoduchý dotaz, jako je například rovnosti nebo nerovnosti použijte replikované tabulky.
- Pro dotazy s predikáty složitý dotaz, jako je například jako použijte distribuované tabulky nebo není jako.

Náročná na prostředky procesoru dotazů se nejlépe provést, když pracovní hello je distribuován do všech hello výpočetních uzlů. Například dotazy, které běží výpočtů na každý řádek tabulky poskytují lepší výkon v distribuované tabulek než replikované tabulky. Vzhledem k tomu, že replikované tabulky je uložený ve plně na každém výpočetním uzlu, náročná na prostředky procesoru dotazy na replikované tabulky spouští celou tabulku hello na každý uzel výpočty. Hello navíc výpočetní můžou způsobit snížení výkonnosti dotazu.

Tento dotaz má například komplexní predikátu.  Rychleji spustí po dodavatele distribuované tabulku místo replikované tabulky. V tomto příkladu dodavatele lze distribuovat algoritmu hash nebo distribuované kruhového dotazování.

```sql

SELECT EnglishProductName 
FROM DimProduct 
WHERE EnglishDescription LIKE '%frame%comfortable%'

```

## <a name="convert-existing-round-robin-tables-tooreplicated-tables"></a>Převést stávající tabulky tooreplicated kruhového dotazování tabulky
Pokud již máte kruhového dotazování tabulky, doporučujeme, abyste je převod tooreplicated tabulky, pokud splňují s kritérii uvedených v tomto článku. Replikované tabulky zlepšit výkon na kruhového dotazování tabulky, protože se vyhnete hello potřebu přesun dat.  Přesun dat pomocí kruhového dotazování tabulky vždy vyžaduje pro spojení. 

Tento příklad používá [funkce CTAS](https://docs.microsoft.com/sql/t-sql/statements/create-table-as-select-azure-sql-data-warehouse) toochange hello DimSalesTerritory tabulky tooa replikované tabulky. Tento příklad funguje bez ohledu na to, jestli je DimSalesTerritory distribuovat algoritmu hash nebo kruhového dotazování.

```sql
CREATE TABLE [dbo].[DimSalesTerritory_REPLICATE]   
WITH   
  (   
    CLUSTERED COLUMNSTORE INDEX,  
    DISTRIBUTION = REPLICATE  
  )  
AS SELECT * FROM [dbo].[DimSalesTerritory]
OPTION  (LABEL  = 'CTAS : DimSalesTerritory_REPLICATE') 

--Create statistics on new table
CREATE STATISTICS [SalesTerritoryKey] ON [DimSalesTerritory_REPLICATE] ([SalesTerritoryKey]);
CREATE STATISTICS [SalesTerritoryAlternateKey] ON [DimSalesTerritory_REPLICATE] ([SalesTerritoryAlternateKey]);
CREATE STATISTICS [SalesTerritoryRegion] ON [DimSalesTerritory_REPLICATE] ([SalesTerritoryRegion]);
CREATE STATISTICS [SalesTerritoryCountry] ON [DimSalesTerritory_REPLICATE] ([SalesTerritoryCountry]);
CREATE STATISTICS [SalesTerritoryGroup] ON [DimSalesTerritory_REPLICATE] ([SalesTerritoryGroup]);

-- Switch table names
RENAME OBJECT [dbo].[DimSalesTerritory] too[DimSalesTerritory_old];
RENAME OBJECT [dbo].[DimSalesTerritory_REPLICATE] too[DimSalesTerritory];

DROP TABLE [dbo].[DimSalesTerritory_old];
```  

### <a name="query-performance-example-for-round-robin-versus-replicated"></a>Příklad výkonu dotazu, kruhové dotazování versus replikovat 

Replikované tabulky nevyžaduje žádné přesun dat pro spojení, protože hello celá tabulka je již na každém výpočetním uzlu. Pokud tabulky dimenzí hello distribuované kruhového dotazování, a připojte zkopíruje hello tabulce dimenze v úplné tooeach výpočetním uzlu. toomove hello data, plán dotazu hello obsahuje operace názvem BroadcastMoveOperation. Tento typ operace přesunu dat zpomalí výkon dotazů a je eliminována použití replikované tabulky. kroky plánování tooview dotaz, použít hello [sys.dm_pdw_request_steps](https://docs.microsoft.com/sql/relational-databases/system-dynamic-management-views/sys-dm-pdw-request-steps-transact-sql) zobrazení katalogu systému. 

Například v následujícím dotazu pro schéma AdventureWorks hello hello ` FactInternetSales` tabulka je distribuovat algoritmu hash. Hello `DimDate` a `DimSalesTerritory` tabulky jsou menší tabulky dimenzí. Tento dotaz vrátí celkový prodej hello v Severní Americe pro fiskálního roku 2004:
 
```sql
SELECT [TotalSalesAmount] = SUM(SalesAmount)
FROM dbo.FactInternetSales s
INNER JOIN dbo.DimDate d
  ON d.DateKey = s.OrderDateKey
INNER JOIN dbo.DimSalesTerritory t
  ON t.SalesTerritoryKey = s.SalesTerritoryKey
WHERE d.FiscalYear = 2004
  AND t.SalesTerritoryGroup = 'North America'
```
Znovu vytvořit `DimDate` a `DimSalesTerritory` jako kruhového dotazování tabulky. V důsledku toho hello dotazu vám ukázal hello následující plán dotazu, který má více vysílání přesunout operace: 
 
![Plán dotazu pomocí kruhového dotazování](media/design-guidance-for-replicated-tables/round-robin-tables-query-plan.jpg) 

Znovu vytvořit `DimDate` a `DimSalesTerritory` jako replikovaných tabulek a znovu se spustil hello dotazu. Plán dotazu výsledné Hello je mnohem kratší a mít žádné nevysílá přesune.

![Replikovat plán dotazu](media/design-guidance-for-replicated-tables/replicated-tables-query-plan.jpg) 


## <a name="performance-considerations-for-modifying-replicated-tables"></a>Důležité informace o výkonu pro úpravy replikované tabulky
SQL Data Warehouse implementuje replikované tabulky udržováním hlavní verze tabulky hello. Kopíruje hello hlavní verze tooone distribuční databázi na každém výpočetním uzlu. Když dojde ke změně, aktualizuje SQL Data Warehouse nejprve hello hlavní tabulka. Potom vyžaduje opětovné sestavení hello tabulek na každém výpočetním uzlu. Opětovné sestavení replikované tabulky zahrnuje kopírování hello tabulky tooeach výpočetní uzel a pak znovu sestavit indexy hello.

Znovu sestaví je potřeba po:
- Data jsou načíst nebo upravit
- datový sklad Hello je jiné nastavení DWU tooa škálovat.
- Aktualizace definice tabulky

Znovu sestaví nejsou nutné po:
- Operace pozastavení
- Operace obnovení

opětovné sestavení Hello neodehrává ihned po data je upravit. Místo toho se aktivuje opětovné sestavení hello hello prvním dotazu vybere z tabulky hello.  V rámci hello počáteční příkazu select z tabulky hello jsou kroky toorebuild hello replikované tabulky.  Protože opětovné sestavení hello se provádí v rámci dotazu hello, může být důležité v závislosti na velikosti hello hello tabulky hello dopad toohello počáteční příkazu select.  Pokud více replikované tabulky se podílejí vyžadující opětovném sestavení, každá kopie je znovu sestavit sériově jako kroky v rámci příkazu hello.  toomaintain konzistenci dat během hello opakované hello replikované tabulky pořízení v tabulce hello výhradní zámek.  Hello uzamčení brání všechny tabulky toohello přístup hello dobu hello přestavení. 

### <a name="use-indexes-conservatively"></a>Můžete použít indexy
Standardní postupy indexování použít tooreplicated tabulky. SQL Data Warehouse znovu sestaví každý index replikované tabulky v rámci opětovné sestavení hello. Indexy používejte jenom hello výkonnější převáží hello náklady na nové sestavení indexů hello.  
 
### <a name="batch-data-loads"></a>Načítání dat dávky
Při načítání dat do replikované tabulky, zkuste znovu sestaví toominimize dávkování zatížení dohromady. Proveďte všechny zatížení hello zpracovat v dávce před spuštěním příkazů select.

Tento vzor zatížení například načte data ze čtyř zdrojů a vyvolá čtyři znovu sestaví. 

- Načíst ze zdroje 1.
- Příkaz SELECT aktivační události znovu sestavit 1.
- Načíst ze zdroje. 2.
- Příkaz SELECT aktivační události znovu sestavit 2.
- Načíst ze zdroje 3.
- Příkaz SELECT aktivační události znovu sestavit 3.
- Načíst ze zdroje 4.
- Příkaz SELECT aktivační události znovu sestavit 4.

Tento vzor zatížení například načte data ze čtyř zdrojů, ale pouze vyvolá jeden opětovné sestavení.

- Načíst ze zdroje 1.
- Načíst ze zdroje. 2.
- Načíst ze zdroje 3.
- Načíst ze zdroje 4.
- Příkaz SELECT aktivační události znovu sestavit.


### <a name="rebuild-a-replicated-table-after-a-batch-load"></a>Znovu sestavte replikované tabulky po zatížení batch
dobu provádění konzistentní dotazu tooensure, doporučujeme vynutit aktualizaci hello replikované tabulky po batch zatížení. První dotaz hello musí počkat, jinak hodnota toorefresh hello tabulky, která zahrnuje nové sestavení indexů hello. V závislosti na velikosti hello a počet replikované tabulky vliv může být důležité hello vlivu na výkon.  

Tento dotaz používá hello [sys.pdw_replicated_table_cache_state](https://docs.microsoft.com/sql/relational-databases/system-catalog-views/sys-pdw-replicated-table-cache-state-transact-sql) DMV toolist hello replikovaných tabulek, které byla upravena, ale není znovu sestavit.

```sql 
SELECT [ReplicatedTable] = t.[name]
  FROM sys.tables t  
  JOIN sys.pdw_replicated_table_cache_state c  
    ON c.object_id = t.object_id 
  JOIN sys.pdw_table_distribution_properties p 
    ON p.object_id = t.object_id 
  WHERE c.[state] = 'NotReady'
    AND p.[distribution_policy_desc] = 'REPLICATE'
```
 
tooforce opětovném sestavení, spusťte následující příkaz pro každou tabulku v předcházející výstup hello hello. 

```sql
SELECT TOP 1 * FROM [ReplicatedTable]
``` 
 
## <a name="next-steps"></a>Další kroky 
toocreate replikované tabulky, použijte jednu z těchto příkazů:

- [Vytvoření tabulky (Azure SQL Data Warehouse)](https://docs.microsoft.com/sql/t-sql/statements/create-table-azure-sql-data-warehouse)
- [Vytvoření TABLE AS SELECT (Azure SQL Data Warehouse](https://docs.microsoft.com/sql/t-sql/statements/create-table-as-select-azure-sql-data-warehouse)

Přehled distribuované tabulek, najdete v tématu [distribuované tabulky](sql-data-warehouse-tables-distribute.md).



