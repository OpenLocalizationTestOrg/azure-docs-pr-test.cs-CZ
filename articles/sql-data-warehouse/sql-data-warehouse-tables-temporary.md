---
title: aaaTemporary tabulek v SQL Data Warehouse | Microsoft Docs
description: "Začínáme s dočasných tabulek v Azure SQL Data Warehouse."
services: sql-data-warehouse
documentationcenter: NA
author: shivaniguptamsft
manager: jhubbard
editor: 
ms.assetid: 9b1119eb-7f54-46d0-ad74-19c85a2a555a
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: tables
ms.date: 10/31/2016
ms.author: shigu;barbkess
ms.openlocfilehash: 2e8b122eb6d71d5bc0a99ce8a2ecab5dbe2d1b49
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="temporary-tables-in-sql-data-warehouse"></a><span data-ttu-id="2585a-103">Dočasné tabulky v SQL Data Warehouse</span><span class="sxs-lookup"><span data-stu-id="2585a-103">Temporary tables in SQL Data Warehouse</span></span>
> [!div class="op_single_selector"]
> * <span data-ttu-id="2585a-104">[Přehled][Overview]</span><span class="sxs-lookup"><span data-stu-id="2585a-104">[Overview][Overview]</span></span>
> * <span data-ttu-id="2585a-105">[Datové typy][Data Types]</span><span class="sxs-lookup"><span data-stu-id="2585a-105">[Data Types][Data Types]</span></span>
> * <span data-ttu-id="2585a-106">[Distribuce][Distribute]</span><span class="sxs-lookup"><span data-stu-id="2585a-106">[Distribute][Distribute]</span></span>
> * <span data-ttu-id="2585a-107">[Index][Index]</span><span class="sxs-lookup"><span data-stu-id="2585a-107">[Index][Index]</span></span>
> * <span data-ttu-id="2585a-108">[Oddíl][Partition]</span><span class="sxs-lookup"><span data-stu-id="2585a-108">[Partition][Partition]</span></span>
> * <span data-ttu-id="2585a-109">[Statistiky][Statistics]</span><span class="sxs-lookup"><span data-stu-id="2585a-109">[Statistics][Statistics]</span></span>
> * <span data-ttu-id="2585a-110">[Dočasné][Temporary]</span><span class="sxs-lookup"><span data-stu-id="2585a-110">[Temporary][Temporary]</span></span>
> 
> 

<span data-ttu-id="2585a-111">Dočasné tabulky jsou velmi užitečné při zpracování dat – zejména při transformace, které jsou přechodné hello mezilehlých výsledků.</span><span class="sxs-lookup"><span data-stu-id="2585a-111">Temporary tables are very useful when processing data - especially during transformation where hello intermediate results are transient.</span></span> <span data-ttu-id="2585a-112">V SQL Data Warehouse existovat dočasné tabulky na úrovni relace hello.</span><span class="sxs-lookup"><span data-stu-id="2585a-112">In SQL Data Warehouse temporary tables exist at hello session level.</span></span>  <span data-ttu-id="2585a-113">Jsou to jenom viditelné toohello relace ve kterém byly vytvořeny a jsou automaticky zrušeny při odhlášení této relaci.</span><span class="sxs-lookup"><span data-stu-id="2585a-113">They are only visible toohello session in which they were created and are automatically dropped when that session logs off.</span></span>  <span data-ttu-id="2585a-114">Dočasné tabulky nabízejí výhody výkonu, protože jejich výsledky jsou zapsány toolocal spíše než vzdálené úložiště.</span><span class="sxs-lookup"><span data-stu-id="2585a-114">Temporary tables offer a performance benefit because their results are written toolocal rather than remote storage.</span></span>  <span data-ttu-id="2585a-115">Dočasné tabulky jsou v Azure SQL Data Warehouse poněkud liší od Azure SQL Database, jako jsou dostupné z kdekoli uvnitř hello relace, včetně uvnitř i mimo ni uložené procedury.</span><span class="sxs-lookup"><span data-stu-id="2585a-115">Temporary tables are slightly different in Azure SQL Data Warehouse than Azure SQL Database as they can be accessed from anywhere inside hello session, including both inside and outside of a stored procedure.</span></span>

<span data-ttu-id="2585a-116">Tento článek obsahuje základní pokyny pro používání dočasných tabulek a zvýrazňuje hello principů relace úrovni dočasných tabulek.</span><span class="sxs-lookup"><span data-stu-id="2585a-116">This article contains essential guidance for using temporary tables and highlights hello principles of session level temporary tables.</span></span> <span data-ttu-id="2585a-117">Pomocí hello informace v tomto článku vám může pomoct rozčlenění kódu, vylepšení – opětovné použití a usnadnění údržby kódu na moduly.</span><span class="sxs-lookup"><span data-stu-id="2585a-117">Using hello information in this article can help you modularize your code, improving both reusability and ease of maintenance of your code.</span></span>

## <a name="create-a-temporary-table"></a><span data-ttu-id="2585a-118">Vytvořit dočasnou tabulku.</span><span class="sxs-lookup"><span data-stu-id="2585a-118">Create a temporary table</span></span>
<span data-ttu-id="2585a-119">Dočasné tabulky se vytváří pomocí jednoduše prefixu název tabulky s `#`.</span><span class="sxs-lookup"><span data-stu-id="2585a-119">Temporary tables are created by simply prefixing your table name with a `#`.</span></span>  <span data-ttu-id="2585a-120">Například:</span><span class="sxs-lookup"><span data-stu-id="2585a-120">For example:</span></span>

```sql
CREATE TABLE #stats_ddl
(
    [schema_name]        NVARCHAR(128) NOT NULL
,    [table_name]            NVARCHAR(128) NOT NULL
,    [stats_name]            NVARCHAR(128) NOT NULL
,    [stats_is_filtered]     BIT           NOT NULL
,    [seq_nmbr]              BIGINT        NOT NULL
,    [two_part_name]         NVARCHAR(260) NOT NULL
,    [three_part_name]       NVARCHAR(400) NOT NULL
)
WITH
(
    DISTRIBUTION = HASH([seq_nmbr])
,    HEAP
)
```

<span data-ttu-id="2585a-121">Dočasné tabulky můžete také vytvořit s `CTAS` pomocí přesně hello stejný přístup:</span><span class="sxs-lookup"><span data-stu-id="2585a-121">Temporary tables can also be created with a `CTAS` using exactly hello same approach:</span></span>

```sql
CREATE TABLE #stats_ddl
WITH
(
    DISTRIBUTION = HASH([seq_nmbr])
,    HEAP
)
AS
(
SELECT
        sm.[name]                                                                AS [schema_name]
,        tb.[name]                                                                AS [table_name]
,        st.[name]                                                                AS [stats_name]
,        st.[has_filter]                                                            AS [stats_is_filtered]
,       ROW_NUMBER()
        OVER(ORDER BY (SELECT NULL))                                            AS [seq_nmbr]
,                                 QUOTENAME(sm.[name])+'.'+QUOTENAME(tb.[name])  AS [two_part_name]
,        QUOTENAME(DB_NAME())+'.'+QUOTENAME(sm.[name])+'.'+QUOTENAME(tb.[name])  AS [three_part_name]
FROM    sys.objects            AS ob
JOIN    sys.stats            AS st    ON    ob.[object_id]        = st.[object_id]
JOIN    sys.stats_columns    AS sc    ON    st.[stats_id]        = sc.[stats_id]
                                    AND st.[object_id]        = sc.[object_id]
JOIN    sys.columns            AS co    ON    sc.[column_id]        = co.[column_id]
                                    AND    sc.[object_id]        = co.[object_id]
JOIN    sys.tables            AS tb    ON    co.[object_id]        = tb.[object_id]
JOIN    sys.schemas            AS sm    ON    tb.[schema_id]        = sm.[schema_id]
WHERE    1=1
AND        st.[user_created]   = 1
GROUP BY
        sm.[name]
,        tb.[name]
,        st.[name]
,        st.[filter_definition]
,        st.[has_filter]
)
SELECT
    CASE @update_type
    WHEN 1
    THEN 'UPDATE STATISTICS '+[two_part_name]+'('+[stats_name]+');'
    WHEN 2
    THEN 'UPDATE STATISTICS '+[two_part_name]+'('+[stats_name]+') WITH FULLSCAN;'
    WHEN 3
    THEN 'UPDATE STATISTICS '+[two_part_name]+'('+[stats_name]+') WITH SAMPLE '+CAST(@sample_pct AS VARCHAR(20))+' PERCENT;'
    WHEN 4
    THEN 'UPDATE STATISTICS '+[two_part_name]+'('+[stats_name]+') WITH RESAMPLE;'
    END AS [update_stats_ddl]
,   [seq_nmbr]
FROM    t1
;
``` 

> [!NOTE]
> <span data-ttu-id="2585a-122">`CTAS`je velmi výkonný příkazu a přidala hello výhod se velmi efektivní v jeho použití místa protokolu transakcí.</span><span class="sxs-lookup"><span data-stu-id="2585a-122">`CTAS` is a very powerful command and has hello added advantage of being very efficient in its use of transaction log space.</span></span> 
> 
> 

## <a name="dropping-temporary-tables"></a><span data-ttu-id="2585a-123">Vyřazení dočasných tabulek</span><span class="sxs-lookup"><span data-stu-id="2585a-123">Dropping temporary tables</span></span>
<span data-ttu-id="2585a-124">Když je vytvořena nová relace, musí existovat žádné dočasné tabulky.</span><span class="sxs-lookup"><span data-stu-id="2585a-124">When a new session is created, no temporary tables should exist.</span></span>  <span data-ttu-id="2585a-125">Však stejné hello při volání uložené procedury, která vytvoří dočasný s hello stejný název, tooensure, vaše `CREATE TABLE` příkazy jsou úspěšné kontrolu jednoduché předběžné existence s `DROP` lze použít jako v následujícím příkladu hello:</span><span class="sxs-lookup"><span data-stu-id="2585a-125">However, if you are calling hello same stored procedure, which creates a temporary with hello same name, tooensure that your `CREATE TABLE` statements are successful a simple pre-existence check with a `DROP` can be used as in hello below example:</span></span>

```sql
IF OBJECT_ID('tempdb..#stats_ddl') IS NOT NULL
BEGIN
    DROP TABLE #stats_ddl
END
```

<span data-ttu-id="2585a-126">Kódování konzistence, je dobrou praxi toouse tento vzor pro tabulky a dočasných tabulek.</span><span class="sxs-lookup"><span data-stu-id="2585a-126">For coding consistency, it is a good practice toouse this pattern for both tables and temporary tables.</span></span>  <span data-ttu-id="2585a-127">Je také vhodné toouse `DROP TABLE` tooremove dočasných tabulek po dokončení s nimi ve vašem kódu.</span><span class="sxs-lookup"><span data-stu-id="2585a-127">It is also a good idea toouse `DROP TABLE` tooremove temporary tables when you have finished with them in your code.</span></span>  <span data-ttu-id="2585a-128">V uložené proceduře vývoj je celkem běžné toosee hello příkazů drop sdruženy na konci hello tohoto postupu tooensure, že jsou tyto objekty vyčistit.</span><span class="sxs-lookup"><span data-stu-id="2585a-128">In stored procedure development it is quite common toosee hello drop commands bundled together at hello end of a procedure tooensure these objects are cleaned up.</span></span>

```sql
DROP TABLE #stats_ddl
```

## <a name="modularizing-code"></a><span data-ttu-id="2585a-129">Modularizing kódu</span><span class="sxs-lookup"><span data-stu-id="2585a-129">Modularizing code</span></span>
<span data-ttu-id="2585a-130">Vzhledem k tomu, že dočasných tabulek se zobrazí kdekoli v relaci uživatele, může to být odstranění zneužité toohelp rozčlenění na moduly kódu aplikace.</span><span class="sxs-lookup"><span data-stu-id="2585a-130">Since temporary tables can be seen anywhere in a user session, this can be exploited toohelp you modularize your application code.</span></span>  <span data-ttu-id="2585a-131">Například hello uložené procedury níže spojuje hello doporučené postupy pro výše toogenerate DDL, který se bude aktualizovat všechny statistiky v databázi hello podle názvu statistiky.</span><span class="sxs-lookup"><span data-stu-id="2585a-131">For example, hello stored procedure below brings together hello recommended practices from above toogenerate DDL which will update all statistics in hello database by statistic name.</span></span>

```sql
CREATE PROCEDURE    [dbo].[prc_sqldw_update_stats]
(   @update_type    tinyint -- 1 default 2 fullscan 3 sample 4 resample
    ,@sample_pct     tinyint
)
AS

IF @update_type NOT IN (1,2,3,4)
BEGIN;
    THROW 151000,'Invalid value for @update_type parameter. Valid range 1 (default), 2 (fullscan), 3 (sample) or 4 (resample).',1;
END;

IF @sample_pct IS NULL
BEGIN;
    SET @sample_pct = 20;
END;

IF OBJECT_ID('tempdb..#stats_ddl') IS NOT NULL
BEGIN
    DROP TABLE #stats_ddl
END

CREATE TABLE #stats_ddl
WITH
(
    DISTRIBUTION = HASH([seq_nmbr])
)
AS
(
SELECT
        sm.[name]                                                                AS [schema_name]
,        tb.[name]                                                                AS [table_name]
,        st.[name]                                                                AS [stats_name]
,        st.[has_filter]                                                            AS [stats_is_filtered]
,       ROW_NUMBER()
        OVER(ORDER BY (SELECT NULL))                                            AS [seq_nmbr]
,                                 QUOTENAME(sm.[name])+'.'+QUOTENAME(tb.[name])  AS [two_part_name]
,        QUOTENAME(DB_NAME())+'.'+QUOTENAME(sm.[name])+'.'+QUOTENAME(tb.[name])  AS [three_part_name]
FROM    sys.objects            AS ob
JOIN    sys.stats            AS st    ON    ob.[object_id]        = st.[object_id]
JOIN    sys.stats_columns    AS sc    ON    st.[stats_id]        = sc.[stats_id]
                                    AND st.[object_id]        = sc.[object_id]
JOIN    sys.columns            AS co    ON    sc.[column_id]        = co.[column_id]
                                    AND    sc.[object_id]        = co.[object_id]
JOIN    sys.tables            AS tb    ON    co.[object_id]        = tb.[object_id]
JOIN    sys.schemas            AS sm    ON    tb.[schema_id]        = sm.[schema_id]
WHERE    1=1
AND        st.[user_created]   = 1
GROUP BY
        sm.[name]
,        tb.[name]
,        st.[name]
,        st.[filter_definition]
,        st.[has_filter]
)
SELECT
    CASE @update_type
    WHEN 1
    THEN 'UPDATE STATISTICS '+[two_part_name]+'('+[stats_name]+');'
    WHEN 2
    THEN 'UPDATE STATISTICS '+[two_part_name]+'('+[stats_name]+') WITH FULLSCAN;'
    WHEN 3
    THEN 'UPDATE STATISTICS '+[two_part_name]+'('+[stats_name]+') WITH SAMPLE '+CAST(@sample_pct AS VARCHAR(20))+' PERCENT;'
    WHEN 4
    THEN 'UPDATE STATISTICS '+[two_part_name]+'('+[stats_name]+') WITH RESAMPLE;'
    END AS [update_stats_ddl]
,   [seq_nmbr]
FROM    t1
;
GO
```

<span data-ttu-id="2585a-132">V této fázi je hello jedinou akcí, které došlo k vytvoření hello uložené procedury, která bude jednoduše vygenerovaným do dočasné tabulky #stats_ddl s příkazy DDL.</span><span class="sxs-lookup"><span data-stu-id="2585a-132">At this stage hello only action that has occurred is hello creation of a stored procedure which will simply generated a temporary table, #stats_ddl, with DDL statements.</span></span>  <span data-ttu-id="2585a-133">Tuto uloženou proceduru bude vyřadit #stats_ddl, pokud již existuje tooensure, které neselže, je-li spustit více než jednou v rámci relace.</span><span class="sxs-lookup"><span data-stu-id="2585a-133">This stored procedure will drop #stats_ddl if it already exists tooensure it does not fail if run more than once within a session.</span></span>  <span data-ttu-id="2585a-134">Ale vzhledem k tomu, že neexistuje žádná `DROP TABLE` na konci hello hello uložené procedury, když hello dokončení uložené procedury je ponechá hello vytvořili tabulku tak, aby ho mohou číst mimo hello uložené procedury.</span><span class="sxs-lookup"><span data-stu-id="2585a-134">However, since there is no `DROP TABLE` at hello end of hello stored procedure, when hello stored procedure completes, it will leave hello created table so that it can be read outside of hello stored procedure.</span></span>  <span data-ttu-id="2585a-135">V SQL Data Warehouse na rozdíl od jiných databází systému SQL Server, je možné toouse hello dočasné tabulky mimo hello procedury, která ji vytvořila.</span><span class="sxs-lookup"><span data-stu-id="2585a-135">In SQL Data Warehouse, unlike other SQL Server databases, it is possible toouse hello temporary table outside of hello procedure that created it.</span></span>  <span data-ttu-id="2585a-136">Je možné dočasných tabulek SQL Data Warehouse **kdekoli** uvnitř hello relace.</span><span class="sxs-lookup"><span data-stu-id="2585a-136">SQL Data Warehouse temporary tables can be used **anywhere** inside hello session.</span></span> <span data-ttu-id="2585a-137">To může vést toomore modulární a spravovat kód jako hello následujícím příkladu:</span><span class="sxs-lookup"><span data-stu-id="2585a-137">This can lead toomore modular and manageable code as in hello below example:</span></span>

```sql
EXEC [dbo].[prc_sqldw_update_stats] @update_type = 1, @sample_pct = NULL;

DECLARE @i INT              = 1
,       @t INT              = (SELECT COUNT(*) FROM #stats_ddl)
,       @s NVARCHAR(4000)   = N''

WHILE @i <= @t
BEGIN
    SET @s=(SELECT update_stats_ddl FROM #stats_ddl WHERE seq_nmbr = @i);

    PRINT @s
    EXEC sp_executesql @s
    SET @i+=1;
END

DROP TABLE #stats_ddl;
```

## <a name="temporary-table-limitations"></a><span data-ttu-id="2585a-138">Dočasné tabulky omezení</span><span class="sxs-lookup"><span data-stu-id="2585a-138">Temporary table limitations</span></span>
<span data-ttu-id="2585a-139">SQL Data Warehouse zavádí několik omezení při implementaci dočasných tabulek.</span><span class="sxs-lookup"><span data-stu-id="2585a-139">SQL Data Warehouse does impose a couple of limitations when implementing temporary tables.</span></span>  <span data-ttu-id="2585a-140">V současné době pouze relace vymezená dočasných tabulek jsou podporovány.</span><span class="sxs-lookup"><span data-stu-id="2585a-140">Currently, only session scoped temporary tables are supported.</span></span>  <span data-ttu-id="2585a-141">Globální dočasné tabulky nejsou podporovány.</span><span class="sxs-lookup"><span data-stu-id="2585a-141">Global Temporary Tables are not supported.</span></span>  <span data-ttu-id="2585a-142">Kromě toho zobrazení nelze vytvořit v dočasných tabulkách.</span><span class="sxs-lookup"><span data-stu-id="2585a-142">In addition, views cannot be created on temporary tables.</span></span>

## <a name="next-steps"></a><span data-ttu-id="2585a-143">Další kroky</span><span class="sxs-lookup"><span data-stu-id="2585a-143">Next steps</span></span>
<span data-ttu-id="2585a-144">články hello toolearn více, najdete na [tabulky přehled][Overview], [tabulky datové typy][Data Types], [distribuci tabulku] [ Distribute], [Indexování tabulku][Index], [vytváření oddílů tabulky] [ Partition] a [ Zachování statistiky tabulky][Statistics].</span><span class="sxs-lookup"><span data-stu-id="2585a-144">toolearn more, see hello articles on [Table Overview][Overview], [Table Data Types][Data Types], [Distributing a Table][Distribute], [Indexing a Table][Index],  [Partitioning a Table][Partition] and [Maintaining Table Statistics][Statistics].</span></span>  <span data-ttu-id="2585a-145">Další informace o osvědčených postupech najdete v tématu [SQL Data Warehouse osvědčené postupy][SQL Data Warehouse Best Practices].</span><span class="sxs-lookup"><span data-stu-id="2585a-145">For more about best practices, see [SQL Data Warehouse Best Practices][SQL Data Warehouse Best Practices].</span></span>

<!--Image references-->

<!--Article references-->
[Overview]: ./sql-data-warehouse-tables-overview.md
[Data Types]: ./sql-data-warehouse-tables-data-types.md
[Distribute]: ./sql-data-warehouse-tables-distribute.md
[Index]: ./sql-data-warehouse-tables-index.md
[Partition]: ./sql-data-warehouse-tables-partition.md
[Statistics]: ./sql-data-warehouse-tables-statistics.md
[Temporary]: ./sql-data-warehouse-tables-temporary.md
[SQL Data Warehouse Best Practices]: ./sql-data-warehouse-best-practices.md

<!--MSDN references-->

<!--Other Web references-->
