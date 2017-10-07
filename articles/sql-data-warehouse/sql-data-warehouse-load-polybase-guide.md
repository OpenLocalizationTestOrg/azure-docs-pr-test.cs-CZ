---
title: "aaaGuide pro používání funkce PolyBase v SQL Data Warehouse | Microsoft Docs"
description: "Pokyny a doporučení pro používání funkce PolyBase v SQL Data Warehouse scénáře."
services: sql-data-warehouse
documentationcenter: NA
author: ckarst
manager: barbkess
editor: 
ms.assetid: 4757fce1-96b3-48ea-8a51-be1385705f9f
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.date: 6/5/2016
ms.custom: loading
ms.author: cakarst;barbkess
ms.openlocfilehash: b05e4c5d528f2fe1c60d6855b5333065f0c908ab
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="guide-for-using-polybase-in-sql-data-warehouse"></a><span data-ttu-id="5863c-103">Příručka pro používání funkce PolyBase v SQL Data Warehouse</span><span class="sxs-lookup"><span data-stu-id="5863c-103">Guide for using PolyBase in SQL Data Warehouse</span></span>
<span data-ttu-id="5863c-104">Tato příručka poskytuje praktické informace pro používání funkce PolyBase v SQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="5863c-104">This guide gives practical information for using PolyBase in SQL Data Warehouse.</span></span>

<span data-ttu-id="5863c-105">tooget začít, najdete v části hello [načtení dat pomocí funkce PolyBase] [ Load data with PolyBase] kurzu.</span><span class="sxs-lookup"><span data-stu-id="5863c-105">tooget started, see hello [Load data with PolyBase][Load data with PolyBase] tutorial.</span></span>

## <a name="rotating-storage-keys"></a><span data-ttu-id="5863c-106">Rotaci klíčů k úložišti</span><span class="sxs-lookup"><span data-stu-id="5863c-106">Rotating storage keys</span></span>
<span data-ttu-id="5863c-107">Z času tootime chcete úložiště objektů blob klíče tooyour přístup hello toochange z bezpečnostních důvodů.</span><span class="sxs-lookup"><span data-stu-id="5863c-107">From time tootime you will want toochange hello access key tooyour blob storage for security reasons.</span></span>

<span data-ttu-id="5863c-108">Hello nejvíce elegantní tooperform způsob, jak tuto úlohu je toofollow tento proces se označuje jako "rotaci klíčů hello".</span><span class="sxs-lookup"><span data-stu-id="5863c-108">hello most elegant way tooperform this task is toofollow a process known as "rotating hello keys".</span></span> <span data-ttu-id="5863c-109">Jste si všimli, že máte dva klíče úložiště pro váš účet úložiště objektů blob.</span><span class="sxs-lookup"><span data-stu-id="5863c-109">You may have noticed that you have two storage keys for your blob storage account.</span></span> <span data-ttu-id="5863c-110">Toto je tak, aby můžete přejít</span><span class="sxs-lookup"><span data-stu-id="5863c-110">This is so that you can transition</span></span>

<span data-ttu-id="5863c-111">Otáčení klíče účtu úložiště Azure je jednoduchý třech krocích</span><span class="sxs-lookup"><span data-stu-id="5863c-111">Rotating your Azure storage account keys is a simple three step process</span></span>

1. <span data-ttu-id="5863c-112">Vytvoření druhého oboru databáze pověření podle hello úložiště sekundární přístupový klíč</span><span class="sxs-lookup"><span data-stu-id="5863c-112">Create second database scoped credential based on hello secondary storage access key</span></span>
2. <span data-ttu-id="5863c-113">Vytvořte druhý externí zdroj dat založena na této nových přihlašovacích údajů</span><span class="sxs-lookup"><span data-stu-id="5863c-113">Create second external data source based off this new credential</span></span>
3. <span data-ttu-id="5863c-114">Vyřaďte a vytvořte hello externích tabulek odkazující toohello nový zdroj externích dat</span><span class="sxs-lookup"><span data-stu-id="5863c-114">Drop and create hello external table(s) pointing toohello new external data source</span></span>

<span data-ttu-id="5863c-115">Pokud jste migrovali všechny externí tabulky toohello nového externí zdroje dat. pak můžete provádět hello vyčištění úlohy:</span><span class="sxs-lookup"><span data-stu-id="5863c-115">When you have migrated all your external tables toohello new external data source then you can perform hello clean up tasks:</span></span>

1. <span data-ttu-id="5863c-116">Vyřaďte první externí zdroj dat</span><span class="sxs-lookup"><span data-stu-id="5863c-116">Drop first external data source</span></span>
2. <span data-ttu-id="5863c-117">První vyřaďte databázi vašich přihlašovacích údajů podle hello primárního úložiště přístupového klíče</span><span class="sxs-lookup"><span data-stu-id="5863c-117">Drop first database scoped credential based on hello primary storage access key</span></span>
3. <span data-ttu-id="5863c-118">Přihlaste se k Azure a znovu vygenerovat primární přístupový klíč hello připravené pro hello příště</span><span class="sxs-lookup"><span data-stu-id="5863c-118">Log into Azure and regenerate hello primary access key ready for hello next time</span></span>

## <a name="query-azure-blob-storage-data"></a><span data-ttu-id="5863c-119">Dotaz na data úložiště objektů blob v Azure</span><span class="sxs-lookup"><span data-stu-id="5863c-119">Query Azure blob storage data</span></span>
<span data-ttu-id="5863c-120">Dotazy na externí tabulky jednoduše použijte název tabulky hello, jako kdyby byl relační tabulku.</span><span class="sxs-lookup"><span data-stu-id="5863c-120">Queries against external tables simply use hello table name as though it was a relational table.</span></span>

```sql
-- Query Azure storage resident data via external table.
SELECT * FROM [ext].[CarSensor_Data]
;
```

> [!NOTE]
> <span data-ttu-id="5863c-121">Dotaz na externí tabulky může selhat s chybou hello *"dotaz byl zrušen--při čtení z externího zdroje bylo dosaženo prahové hodnoty pro maximální odmítněte k hello"*.</span><span class="sxs-lookup"><span data-stu-id="5863c-121">A query on an external table can fail with hello error *"Query aborted-- hello maximum reject threshold was reached while reading from an external source"*.</span></span> <span data-ttu-id="5863c-122">To znamená, že externích dat obsahuje *nekonzistence* záznamy.</span><span class="sxs-lookup"><span data-stu-id="5863c-122">This indicates that your external data contains *dirty* records.</span></span> <span data-ttu-id="5863c-123">Záznam dat se považuje za 'nekonzistence' Pokud hello skutečná data typy nebo počet sloupců neodpovídají žádné definice sloupců hello hello externí tabulky nebo pokud hello data neodpovídají toohello zadaný externí soubor formátu.</span><span class="sxs-lookup"><span data-stu-id="5863c-123">A data record is considered 'dirty' if hello actual data types/number of columns do not match hello column definitions of hello external table or if hello data doesn't conform toohello specified external file format.</span></span> <span data-ttu-id="5863c-124">toofix, zajistěte, aby externí tabulky a definice formátu externího souboru jsou správné a externích dat vyhovuje toothese definice.</span><span class="sxs-lookup"><span data-stu-id="5863c-124">toofix this, ensure that your external table and external file format definitions are correct and your external data conforms toothese definitions.</span></span> <span data-ttu-id="5863c-125">V případě, že podmnožinu záznamů externích dat jsou chybná, můžete tooreject tyto záznamy pro své dotazy pomocí možnosti odmítněte hello ve vytvoření externí tabulky DDL.</span><span class="sxs-lookup"><span data-stu-id="5863c-125">In case a subset of external data records are dirty, you can choose tooreject these records for your queries by using hello reject options in CREATE EXTERNAL TABLE DDL.</span></span>
> 
> 

## <a name="load-data-from-azure-blob-storage"></a><span data-ttu-id="5863c-126">Načtení dat z Azure Blob Storage</span><span class="sxs-lookup"><span data-stu-id="5863c-126">Load data from Azure blob storage</span></span>
<span data-ttu-id="5863c-127">Tento příklad načte data z databáze datového skladu tooSQL úložiště objektů blob v Azure.</span><span class="sxs-lookup"><span data-stu-id="5863c-127">This example loads data from Azure blob storage tooSQL Data Warehouse database.</span></span>

<span data-ttu-id="5863c-128">Ukládání dat přímo odebere hello doba přenosu dat pro dotazy.</span><span class="sxs-lookup"><span data-stu-id="5863c-128">Storing data directly removes hello data transfer time for queries.</span></span> <span data-ttu-id="5863c-129">Ukládání dat s indexem columnstore zlepšuje výkon dotazů pro analýzu dotazy pomocí až too10x.</span><span class="sxs-lookup"><span data-stu-id="5863c-129">Storing data with a columnstore index improves query performance for analysis queries by up too10x.</span></span>

<span data-ttu-id="5863c-130">Tento příklad používá hello CREATE TABLE AS SELECT příkaz tooload data.</span><span class="sxs-lookup"><span data-stu-id="5863c-130">This example uses hello CREATE TABLE AS SELECT statement tooload data.</span></span> <span data-ttu-id="5863c-131">Nová tabulka Hello dědí hello sloupce pojmenované v dotazu hello.</span><span class="sxs-lookup"><span data-stu-id="5863c-131">hello new table inherits hello columns named in hello query.</span></span> <span data-ttu-id="5863c-132">Zdědil hello datové typy tyto sloupce z hello definici externí tabulky.</span><span class="sxs-lookup"><span data-stu-id="5863c-132">It inherits hello data types of those columns from hello external table definition.</span></span>

<span data-ttu-id="5863c-133">CREATE TABLE AS SELECT je vysoce původce příkazu Transact-SQL, který načítá data hello v paralelní tooall hello výpočetní uzly SQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="5863c-133">CREATE TABLE AS SELECT is a highly performant Transact-SQL statement that loads hello data in parallel tooall hello compute nodes of your SQL Data Warehouse.</span></span>  <span data-ttu-id="5863c-134">To byl původně vytvořen pro modul hello (MPP) massively parallel processing v Analytics Platform System a je nyní v SQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="5863c-134">It was originally developed for  hello massively parallel processing (MPP) engine in Analytics Platform System and is now in SQL Data Warehouse.</span></span>

```sql
-- Load data from Azure blob storage tooSQL Data Warehouse

CREATE TABLE [dbo].[Customer_Speed]
WITH
(   
    CLUSTERED COLUMNSTORE INDEX
,    DISTRIBUTION = HASH([CarSensor_Data].[CustomerKey])
)
AS
SELECT *
FROM   [ext].[CarSensor_Data]
;
```

<span data-ttu-id="5863c-135">V tématu [CREATE TABLE AS SELECT (Transact-SQL)][CREATE TABLE AS SELECT (Transact-SQL)].</span><span class="sxs-lookup"><span data-stu-id="5863c-135">See [CREATE TABLE AS SELECT (Transact-SQL)][CREATE TABLE AS SELECT (Transact-SQL)].</span></span>

## <a name="create-statistics-on-newly-loaded-data"></a><span data-ttu-id="5863c-136">Vytvoření statistiky pro nově načtená data</span><span class="sxs-lookup"><span data-stu-id="5863c-136">Create Statistics on newly loaded data</span></span>
<span data-ttu-id="5863c-137">Azure SQL Data Warehouse zatím nepodporuje automatické vytváření ani automatickou aktualizaci statistik.</span><span class="sxs-lookup"><span data-stu-id="5863c-137">Azure SQL Data Warehouse does not yet support auto create or auto update statistics.</span></span>  <span data-ttu-id="5863c-138">V pořadí tooget hello nejlepší výkon ze své dotazy je důležité, aby se statistiky vytvořily pro všechny sloupce všech tabulek po prvním načtením hello nebo dojít k významné změny v datech hello.</span><span class="sxs-lookup"><span data-stu-id="5863c-138">In order tooget hello best performance from your queries, it's important that statistics be created on all columns of all tables after hello first load or any substantial changes occur in hello data.</span></span>  <span data-ttu-id="5863c-139">Podrobné vysvětlení statistiky najdete v tématu hello [statistiky] [ Statistics] tématu ve skupině témat věnovaných vývoji hello.</span><span class="sxs-lookup"><span data-stu-id="5863c-139">For a detailed explanation of statistics, see hello [Statistics][Statistics] topic in hello Develop group of topics.</span></span>  <span data-ttu-id="5863c-140">Níže je zběžný příklad jak načíst toocreate statistiku hello sestavily v tomto příkladu.</span><span class="sxs-lookup"><span data-stu-id="5863c-140">Below is a quick example of how toocreate statistics on hello tabled loaded in this example.</span></span>

```sql
create statistics [SensorKey] on [Customer_Speed] ([SensorKey]);
create statistics [CustomerKey] on [Customer_Speed] ([CustomerKey]);
create statistics [GeographyKey] on [Customer_Speed] ([GeographyKey]);
create statistics [Speed] on [Customer_Speed] ([Speed]);
create statistics [YearMeasured] on [Customer_Speed] ([YearMeasured]);
```

## <a name="export-data-tooazure-blob-storage"></a><span data-ttu-id="5863c-141">Export úložiště objektů blob tooAzure dat</span><span class="sxs-lookup"><span data-stu-id="5863c-141">Export data tooAzure blob storage</span></span>
<span data-ttu-id="5863c-142">Tato část uvádí, jak tooexport dat z SQL Data Warehouse tooAzure blob storage.</span><span class="sxs-lookup"><span data-stu-id="5863c-142">This section shows how tooexport data from SQL Data Warehouse tooAzure blob storage.</span></span> <span data-ttu-id="5863c-143">Tento příklad používá vytvořit externí TABLE AS SELECT což je vysoce původce Transact-SQL příkaz tooexport hello dat paralelně ze všech hello výpočetních uzlů.</span><span class="sxs-lookup"><span data-stu-id="5863c-143">This example uses CREATE EXTERNAL TABLE AS SELECT which is a highly performant Transact-SQL statement tooexport hello data in parallel from all hello compute nodes.</span></span>

<span data-ttu-id="5863c-144">Hello následující příklad vytvoří externí tabulku Weblogs2014 pomocí definice sloupců a data z dbo. Tabulka Weblogů.</span><span class="sxs-lookup"><span data-stu-id="5863c-144">hello following example creates an external table Weblogs2014 using column definitions and data from dbo.Weblogs table.</span></span> <span data-ttu-id="5863c-145">definici externí tabulky Hello je uložená v SQL Data Warehouse a hello výsledky příkazu SELECT hello jsou exportovaný toohello "/ archivaci/log2014 /" adresář zadaný zdroj dat hello kontejner objektů blob hello.</span><span class="sxs-lookup"><span data-stu-id="5863c-145">hello external table definition is stored in SQL Data Warehouse and hello results of hello SELECT statement are exported toohello "/archive/log2014/" directory under hello blob container specified by hello data source.</span></span> <span data-ttu-id="5863c-146">Hello data se exportují ve formátu souboru zadaným textem hello.</span><span class="sxs-lookup"><span data-stu-id="5863c-146">hello data is exported in hello specified text file format.</span></span>

```sql
CREATE EXTERNAL TABLE Weblogs2014 WITH
(
    LOCATION='/archive/log2014/',
    DATA_SOURCE=azure_storage,
    FILE_FORMAT=text_file_format
)
AS
SELECT
    Uri,
    DateRequested
FROM
    dbo.Weblogs
WHERE
    1=1
    AND DateRequested > '12/31/2013'
    AND DateRequested < '01/01/2015';
```
## <a name="isolate-loading-users"></a><span data-ttu-id="5863c-147">Izolovat načítání uživatelů</span><span class="sxs-lookup"><span data-stu-id="5863c-147">Isolate Loading Users</span></span>
<span data-ttu-id="5863c-148">Je často nutné toohave více uživatelů, které můžete načíst data do datového skladu SQL.</span><span class="sxs-lookup"><span data-stu-id="5863c-148">There is often a need toohave multiple users that can load data into a SQL DW.</span></span> <span data-ttu-id="5863c-149">Protože hello [CREATE TABLE AS SELECT (Transact-SQL)] [ CREATE TABLE AS SELECT (Transact-SQL)] vyžaduje oprávnění pro řízení hello databáze, budete mít s více uživateli pomocí řízení přístupu přes všechny schémat.</span><span class="sxs-lookup"><span data-stu-id="5863c-149">Because hello [CREATE TABLE AS SELECT (Transact-SQL)][CREATE TABLE AS SELECT (Transact-SQL)] requires CONTROL permissions of hello database, you will end up with multiple users with control access over all schemas.</span></span> <span data-ttu-id="5863c-150">toolimit, můžete použít příkaz ODEPŘÍT řízení hello.</span><span class="sxs-lookup"><span data-stu-id="5863c-150">toolimit this, you can use hello DENY CONTROL statement.</span></span>

<span data-ttu-id="5863c-151">Příklad: Vzít schema_A schémat databáze pro oddělení A a schema_B pro oddělení B umožní user_A databáze uživatelů a user_B uživatelů pro PolyBase načtení z oddělení A a B, v uvedeném pořadí.</span><span class="sxs-lookup"><span data-stu-id="5863c-151">Example: Consider database schemas schema_A for dept A, and schema_B for dept B Let database users user_A and user_B be users for PolyBase loading in dept A and B, respectively.</span></span> <span data-ttu-id="5863c-152">Oba mají udělené oprávnění pro řízení databáze.</span><span class="sxs-lookup"><span data-stu-id="5863c-152">They both have been granted CONTROL database permissions.</span></span>
<span data-ttu-id="5863c-153">Tvůrci Hello schématu A a B nyní zamknout jejich schémata pomocí ODEPŘÍT:</span><span class="sxs-lookup"><span data-stu-id="5863c-153">hello creators of schema A and B now lock down their schemas using DENY:</span></span>

```sql
   DENY CONTROL ON SCHEMA :: schema_A toouser_B;
   DENY CONTROL ON SCHEMA :: schema_B toouser_A;
```   
 <span data-ttu-id="5863c-154">Pomocí tohoto user_A a user_B by měl nyní být uzamčený hello schématu jiných oddělení.</span><span class="sxs-lookup"><span data-stu-id="5863c-154">With this, user_A and user_B should now be locked out from hello other dept’s schema.</span></span>
 


## <a name="next-steps"></a><span data-ttu-id="5863c-155">Další kroky</span><span class="sxs-lookup"><span data-stu-id="5863c-155">Next steps</span></span>
<span data-ttu-id="5863c-156">toolearn Další informace o přesunutí dat tooSQL datového skladu, najdete v části hello [Přehled migrace dat][data migration overview].</span><span class="sxs-lookup"><span data-stu-id="5863c-156">toolearn more about moving data tooSQL Data Warehouse, see hello [data migration overview][data migration overview].</span></span>

<!--Image references-->

<!--Article references-->
[Load data with bcp]: ./sql-data-warehouse-load-with-bcp.md
[Load data with PolyBase]: ./sql-data-warehouse-get-started-load-with-polybase.md
[Statistics]: ./sql-data-warehouse-tables-statistics.md
[data migration overview]: ./sql-data-warehouse-overview-migrate.md

<!--MSDN references-->
[supported source/sink]: https://msdn.microsoft.com/library/dn894007.aspx
[copy activity]: https://msdn.microsoft.com/library/dn835035.aspx
[SQL Server destination adapter]: https://msdn.microsoft.com/library/ms141095.aspx
[SSIS]: https://msdn.microsoft.com/library/ms141026.aspx

[CREATE EXTERNAL DATA SOURCE (Transact-SQL)]: https://msdn.microsoft.com/library/dn935022.aspx
[CREATE EXTERNAL FILE FORMAT (Transact-SQL)]: https://msdn.microsoft.com/library/dn935026.aspx
[CREATE EXTERNAL TABLE (Transact-SQL)]: https://msdn.microsoft.com/library/dn935021.aspx

[DROP EXTERNAL DATA SOURCE (Transact-SQL)]: https://msdn.microsoft.com/library/mt146367.aspx
[DROP EXTERNAL FILE FORMAT (Transact-SQL)]: https://msdn.microsoft.com/library/mt146379.aspx
[DROP EXTERNAL TABLE (Transact-SQL)]: https://msdn.microsoft.com/library/mt130698.aspx

[CREATE TABLE AS SELECT (Transact-SQL)]: https://msdn.microsoft.com/library/mt204041.aspx
[INSERT...SELECT (Transact-SQL)]: https://msdn.microsoft.com/library/ms174335.aspx
[CREATE MASTER KEY (Transact-SQL)]: https://msdn.microsoft.com/library/ms174382.aspx
[CREATE CREDENTIAL (Transact-SQL)]: https://msdn.microsoft.com/library/ms189522.aspx
[CREATE DATABASE SCOPED CREDENTIAL (Transact-SQL)]: https://msdn.microsoft.com/library/mt270260.aspx
[DROP CREDENTIAL (Transact-SQL)]: https://msdn.microsoft.com/library/ms189450.aspx

<!-- External Links -->
