---
title: "Příručka pro používání funkce PolyBase v SQL Data Warehouse | Microsoft Docs"
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
ms.openlocfilehash: 6938b92d8e5b46d908dc5b2155bdfdc89bb1dc8c
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="guide-for-using-polybase-in-sql-data-warehouse"></a><span data-ttu-id="06d17-103">Příručka pro používání funkce PolyBase v SQL Data Warehouse</span><span class="sxs-lookup"><span data-stu-id="06d17-103">Guide for using PolyBase in SQL Data Warehouse</span></span>
<span data-ttu-id="06d17-104">Tato příručka poskytuje praktické informace pro používání funkce PolyBase v SQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="06d17-104">This guide gives practical information for using PolyBase in SQL Data Warehouse.</span></span>

<span data-ttu-id="06d17-105">Abyste mohli začít, najdete v článku [načtení dat pomocí funkce PolyBase] [ Load data with PolyBase] kurzu.</span><span class="sxs-lookup"><span data-stu-id="06d17-105">To get started, see the [Load data with PolyBase][Load data with PolyBase] tutorial.</span></span>

## <a name="rotating-storage-keys"></a><span data-ttu-id="06d17-106">Rotaci klíčů k úložišti</span><span class="sxs-lookup"><span data-stu-id="06d17-106">Rotating storage keys</span></span>
<span data-ttu-id="06d17-107">Čas od času budete chtít změnit přístupový klíč do úložiště objektů blob z bezpečnostních důvodů.</span><span class="sxs-lookup"><span data-stu-id="06d17-107">From time to time you will want to change the access key to your blob storage for security reasons.</span></span>

<span data-ttu-id="06d17-108">Většina elegantní způsob k provedení této úlohy je postupujte podle tento proces se označuje jako "rotaci klíčů".</span><span class="sxs-lookup"><span data-stu-id="06d17-108">The most elegant way to perform this task is to follow a process known as "rotating the keys".</span></span> <span data-ttu-id="06d17-109">Jste si všimli, že máte dva klíče úložiště pro váš účet úložiště objektů blob.</span><span class="sxs-lookup"><span data-stu-id="06d17-109">You may have noticed that you have two storage keys for your blob storage account.</span></span> <span data-ttu-id="06d17-110">Toto je tak, aby můžete přejít</span><span class="sxs-lookup"><span data-stu-id="06d17-110">This is so that you can transition</span></span>

<span data-ttu-id="06d17-111">Otáčení klíče účtu úložiště Azure je jednoduchý třech krocích</span><span class="sxs-lookup"><span data-stu-id="06d17-111">Rotating your Azure storage account keys is a simple three step process</span></span>

1. <span data-ttu-id="06d17-112">Vytvoření druhého oboru databáze pověření podle přístupového klíče sekundární úložiště.</span><span class="sxs-lookup"><span data-stu-id="06d17-112">Create second database scoped credential based on the secondary storage access key</span></span>
2. <span data-ttu-id="06d17-113">Vytvořte druhý externí zdroj dat založena na této nových přihlašovacích údajů</span><span class="sxs-lookup"><span data-stu-id="06d17-113">Create second external data source based off this new credential</span></span>
3. <span data-ttu-id="06d17-114">Vyřaďte a vytvořte externí tabulky či tabulek odkazující na nový zdroj externích dat</span><span class="sxs-lookup"><span data-stu-id="06d17-114">Drop and create the external table(s) pointing to the new external data source</span></span>

<span data-ttu-id="06d17-115">Pokud jste migrovali všechny externí tabulky na nový zdroj externích dat a poté můžete provádět úlohy čištění:</span><span class="sxs-lookup"><span data-stu-id="06d17-115">When you have migrated all your external tables to the new external data source then you can perform the clean up tasks:</span></span>

1. <span data-ttu-id="06d17-116">Vyřaďte první externí zdroj dat</span><span class="sxs-lookup"><span data-stu-id="06d17-116">Drop first external data source</span></span>
2. <span data-ttu-id="06d17-117">První vyřaďte databázi vašich přihlašovacích údajů podle přístupový klíč primárního úložiště</span><span class="sxs-lookup"><span data-stu-id="06d17-117">Drop first database scoped credential based on the primary storage access key</span></span>
3. <span data-ttu-id="06d17-118">Přihlaste se k Azure a znovu vygenerovat primární přístupový klíč připravené pro příští</span><span class="sxs-lookup"><span data-stu-id="06d17-118">Log into Azure and regenerate the primary access key ready for the next time</span></span>

## <a name="query-azure-blob-storage-data"></a><span data-ttu-id="06d17-119">Dotaz na data úložiště objektů blob v Azure</span><span class="sxs-lookup"><span data-stu-id="06d17-119">Query Azure blob storage data</span></span>
<span data-ttu-id="06d17-120">Dotazy na externí tabulky jednoduše použijte název tabulky, jako kdyby byl relační tabulku.</span><span class="sxs-lookup"><span data-stu-id="06d17-120">Queries against external tables simply use the table name as though it was a relational table.</span></span>

```sql
-- Query Azure storage resident data via external table.
SELECT * FROM [ext].[CarSensor_Data]
;
```

> [!NOTE]
> <span data-ttu-id="06d17-121">Dotaz na externí tabulky může selhat s chybou *"dotaz byl zrušen--při čtení z externího zdroje bylo dosaženo maximální odmítněte prahovou hodnotu"*.</span><span class="sxs-lookup"><span data-stu-id="06d17-121">A query on an external table can fail with the error *"Query aborted-- the maximum reject threshold was reached while reading from an external source"*.</span></span> <span data-ttu-id="06d17-122">To znamená, že externích dat obsahuje *nekonzistence* záznamy.</span><span class="sxs-lookup"><span data-stu-id="06d17-122">This indicates that your external data contains *dirty* records.</span></span> <span data-ttu-id="06d17-123">Záznam dat se považuje za 'nekonzistence' Pokud skutečná data typy nebo počet sloupců neodpovídají žádné definice sloupců externí tabulky, nebo pokud data neodpovídají zadaný externí soubor formátu.</span><span class="sxs-lookup"><span data-stu-id="06d17-123">A data record is considered 'dirty' if the actual data types/number of columns do not match the column definitions of the external table or if the data doesn't conform to the specified external file format.</span></span> <span data-ttu-id="06d17-124">Chcete-li odstranit tento problém, zajistěte, aby externí tabulky a definice formátu externího souboru jsou správné a externích dat odpovídá tyto definice.</span><span class="sxs-lookup"><span data-stu-id="06d17-124">To fix this, ensure that your external table and external file format definitions are correct and your external data conforms to these definitions.</span></span> <span data-ttu-id="06d17-125">V případě, že podmnožinu záznamů externích dat jsou chybná, můžete tak, aby zamítal tyto záznamy pro své dotazy pomocí možnosti odmítněte ve vytvoření externí tabulky DDL.</span><span class="sxs-lookup"><span data-stu-id="06d17-125">In case a subset of external data records are dirty, you can choose to reject these records for your queries by using the reject options in CREATE EXTERNAL TABLE DDL.</span></span>
> 
> 

## <a name="load-data-from-azure-blob-storage"></a><span data-ttu-id="06d17-126">Načtení dat z Azure Blob Storage</span><span class="sxs-lookup"><span data-stu-id="06d17-126">Load data from Azure blob storage</span></span>
<span data-ttu-id="06d17-127">Tento příklad načte data z Azure blob storage do SQL Data Warehouse databáze.</span><span class="sxs-lookup"><span data-stu-id="06d17-127">This example loads data from Azure blob storage to SQL Data Warehouse database.</span></span>

<span data-ttu-id="06d17-128">Ukládání dat přímo odebere doba přenosu dat pro dotazy.</span><span class="sxs-lookup"><span data-stu-id="06d17-128">Storing data directly removes the data transfer time for queries.</span></span> <span data-ttu-id="06d17-129">Ukládání dat s indexem columnstore zlepšuje výkon dotazů pro analýzu dotazy pomocí až 10 x.</span><span class="sxs-lookup"><span data-stu-id="06d17-129">Storing data with a columnstore index improves query performance for analysis queries by up to 10x.</span></span>

<span data-ttu-id="06d17-130">Tento příklad používá příkaz CREATE TABLE AS SELECT načíst data.</span><span class="sxs-lookup"><span data-stu-id="06d17-130">This example uses the CREATE TABLE AS SELECT statement to load data.</span></span> <span data-ttu-id="06d17-131">Nová tabulka dědí sloupce pojmenované v dotazu.</span><span class="sxs-lookup"><span data-stu-id="06d17-131">The new table inherits the columns named in the query.</span></span> <span data-ttu-id="06d17-132">Datové typy tyto sloupce dědí z definici externí tabulky.</span><span class="sxs-lookup"><span data-stu-id="06d17-132">It inherits the data types of those columns from the external table definition.</span></span>

<span data-ttu-id="06d17-133">CREATE TABLE AS SELECT je vysoce původce příkazu Transact-SQL, který načte data souběžně na výpočetní uzly z SQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="06d17-133">CREATE TABLE AS SELECT is a highly performant Transact-SQL statement that loads the data in parallel to all the compute nodes of your SQL Data Warehouse.</span></span>  <span data-ttu-id="06d17-134">To byl původně vytvořen pro modul (MPP) massively parallel processing v Analytics Platform System a je nyní v SQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="06d17-134">It was originally developed for  the massively parallel processing (MPP) engine in Analytics Platform System and is now in SQL Data Warehouse.</span></span>

```sql
-- Load data from Azure blob storage to SQL Data Warehouse

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

<span data-ttu-id="06d17-135">V tématu [CREATE TABLE AS SELECT (Transact-SQL)][CREATE TABLE AS SELECT (Transact-SQL)].</span><span class="sxs-lookup"><span data-stu-id="06d17-135">See [CREATE TABLE AS SELECT (Transact-SQL)][CREATE TABLE AS SELECT (Transact-SQL)].</span></span>

## <a name="create-statistics-on-newly-loaded-data"></a><span data-ttu-id="06d17-136">Vytvoření statistiky pro nově načtená data</span><span class="sxs-lookup"><span data-stu-id="06d17-136">Create Statistics on newly loaded data</span></span>
<span data-ttu-id="06d17-137">Azure SQL Data Warehouse zatím nepodporuje automatické vytváření ani automatickou aktualizaci statistik.</span><span class="sxs-lookup"><span data-stu-id="06d17-137">Azure SQL Data Warehouse does not yet support auto create or auto update statistics.</span></span>  <span data-ttu-id="06d17-138">Aby vám dotazy vracely co nejlepší výsledky, je důležité, aby se statistiky vytvořily pro všechny sloupce všech tabulek po prvním načtením nebo kdykoli, kdy v datech dojde k podstatným změnám.</span><span class="sxs-lookup"><span data-stu-id="06d17-138">In order to get the best performance from your queries, it's important that statistics be created on all columns of all tables after the first load or any substantial changes occur in the data.</span></span>  <span data-ttu-id="06d17-139">Podrobné vysvětlení statistiky najdete v tématu [Statistika][Statistics] ve skupině témat věnovaných vývoji.</span><span class="sxs-lookup"><span data-stu-id="06d17-139">For a detailed explanation of statistics, see the [Statistics][Statistics] topic in the Develop group of topics.</span></span>  <span data-ttu-id="06d17-140">Níže je zběžný příklad vytvoření statistik pro tabulku načtenou v tomto příkladě.</span><span class="sxs-lookup"><span data-stu-id="06d17-140">Below is a quick example of how to create statistics on the tabled loaded in this example.</span></span>

```sql
create statistics [SensorKey] on [Customer_Speed] ([SensorKey]);
create statistics [CustomerKey] on [Customer_Speed] ([CustomerKey]);
create statistics [GeographyKey] on [Customer_Speed] ([GeographyKey]);
create statistics [Speed] on [Customer_Speed] ([Speed]);
create statistics [YearMeasured] on [Customer_Speed] ([YearMeasured]);
```

## <a name="export-data-to-azure-blob-storage"></a><span data-ttu-id="06d17-141">Exportovat data do Azure blob storage</span><span class="sxs-lookup"><span data-stu-id="06d17-141">Export data to Azure blob storage</span></span>
<span data-ttu-id="06d17-142">V této části ukazuje, jak exportovat data z SQL Data Warehouse do Azure blob storage.</span><span class="sxs-lookup"><span data-stu-id="06d17-142">This section shows how to export data from SQL Data Warehouse to Azure blob storage.</span></span> <span data-ttu-id="06d17-143">Tento příklad používá vytvořit externí TABLE AS SELECT, který je vysoce původce příkazu Transact-SQL k exportu dat paralelně z výpočetních uzlů.</span><span class="sxs-lookup"><span data-stu-id="06d17-143">This example uses CREATE EXTERNAL TABLE AS SELECT which is a highly performant Transact-SQL statement to export the data in parallel from all the compute nodes.</span></span>

<span data-ttu-id="06d17-144">Následující příklad vytvoří externí tabulku Weblogs2014 pomocí definice sloupců a data z dbo. Tabulka Weblogů.</span><span class="sxs-lookup"><span data-stu-id="06d17-144">The following example creates an external table Weblogs2014 using column definitions and data from dbo.Weblogs table.</span></span> <span data-ttu-id="06d17-145">Definici externí tabulky je uložená v SQL Data Warehouse a k adresáři "/ archivaci/log2014 /" v kontejneru objektů blob zadaný zdroj dat exportují se výsledky příkazu SELECT.</span><span class="sxs-lookup"><span data-stu-id="06d17-145">The external table definition is stored in SQL Data Warehouse and the results of the SELECT statement are exported to the "/archive/log2014/" directory under the blob container specified by the data source.</span></span> <span data-ttu-id="06d17-146">Data se exportují ve formátu zadaný text.</span><span class="sxs-lookup"><span data-stu-id="06d17-146">The data is exported in the specified text file format.</span></span>

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
## <a name="isolate-loading-users"></a><span data-ttu-id="06d17-147">Izolovat načítání uživatelů</span><span class="sxs-lookup"><span data-stu-id="06d17-147">Isolate Loading Users</span></span>
<span data-ttu-id="06d17-148">Je často potřeba mít více uživatelů, které můžete načíst data do datového skladu SQL.</span><span class="sxs-lookup"><span data-stu-id="06d17-148">There is often a need to have multiple users that can load data into a SQL DW.</span></span> <span data-ttu-id="06d17-149">Protože [CREATE TABLE AS SELECT (Transact-SQL)] [ CREATE TABLE AS SELECT (Transact-SQL)] vyžaduje oprávnění pro řízení databáze, budete mít s více uživateli pomocí řízení přístupu přes všechny schémat.</span><span class="sxs-lookup"><span data-stu-id="06d17-149">Because the [CREATE TABLE AS SELECT (Transact-SQL)][CREATE TABLE AS SELECT (Transact-SQL)] requires CONTROL permissions of the database, you will end up with multiple users with control access over all schemas.</span></span> <span data-ttu-id="06d17-150">Pokud chcete omezit to, můžete použít příkaz ODEPŘÍT ovládacího PRVKU.</span><span class="sxs-lookup"><span data-stu-id="06d17-150">To limit this, you can use the DENY CONTROL statement.</span></span>

<span data-ttu-id="06d17-151">Příklad: Vzít schema_A schémat databáze pro oddělení A a schema_B pro oddělení B umožní user_A databáze uživatelů a user_B uživatelů pro PolyBase načtení z oddělení A a B, v uvedeném pořadí.</span><span class="sxs-lookup"><span data-stu-id="06d17-151">Example: Consider database schemas schema_A for dept A, and schema_B for dept B Let database users user_A and user_B be users for PolyBase loading in dept A and B, respectively.</span></span> <span data-ttu-id="06d17-152">Oba mají udělené oprávnění pro řízení databáze.</span><span class="sxs-lookup"><span data-stu-id="06d17-152">They both have been granted CONTROL database permissions.</span></span>
<span data-ttu-id="06d17-153">Tvůrci zámek schématu A a B nyní dolů jejich schémata pomocí ODEPŘÍT:</span><span class="sxs-lookup"><span data-stu-id="06d17-153">The creators of schema A and B now lock down their schemas using DENY:</span></span>

```sql
   DENY CONTROL ON SCHEMA :: schema_A TO user_B;
   DENY CONTROL ON SCHEMA :: schema_B TO user_A;
```   
 <span data-ttu-id="06d17-154">Pomocí tohoto user_A a user_B by měl nyní uzamčen z jiných oddělení schématu.</span><span class="sxs-lookup"><span data-stu-id="06d17-154">With this, user_A and user_B should now be locked out from the other dept’s schema.</span></span>
 


## <a name="next-steps"></a><span data-ttu-id="06d17-155">Další kroky</span><span class="sxs-lookup"><span data-stu-id="06d17-155">Next steps</span></span>
<span data-ttu-id="06d17-156">Další informace o přesun dat do SQL Data Warehouse, najdete v článku [Přehled migrace dat][data migration overview].</span><span class="sxs-lookup"><span data-stu-id="06d17-156">To learn more about moving data to SQL Data Warehouse, see the [data migration overview][data migration overview].</span></span>

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
