---
title: "Datové typy pokyny - Azure SQL Data Warehouse | Microsoft Docs"
description: "Doporučení k definování typů dat, které jsou kompatibilní s SQL Data Warehouse."
services: sql-data-warehouse
documentationcenter: NA
author: shivaniguptamsft
manager: barbkess
editor: 
ms.assetid: d4a1f0a3-ba9f-44b9-95f6-16a4f30746d6
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: tables
ms.date: 06/02/2017
ms.author: shigu;barbkess
ms.openlocfilehash: 5c24c71af16bd9851d9caf15fecfa4bb76f5f77e
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="guidance-for-defining-data-types-for-tables-in-sql-data-warehouse"></a><span data-ttu-id="e05cb-103">Pokyny pro definování typů dat pro tabulky v SQL Data Warehouse</span><span class="sxs-lookup"><span data-stu-id="e05cb-103">Guidance for defining data types for tables in SQL Data Warehouse</span></span>
<span data-ttu-id="e05cb-104">Tato doporučení použijte k definování typů tabulek dat, které jsou kompatibilní s datovým skladem SQL.</span><span class="sxs-lookup"><span data-stu-id="e05cb-104">Use these recommendations to define table data types that are compatible with SQL Data Warehouse.</span></span> <span data-ttu-id="e05cb-105">Kromě kompatibility minimalizovat velikost datových typů zlepšuje výkon dotazů.</span><span class="sxs-lookup"><span data-stu-id="e05cb-105">In addition to compatibility, minimizing the size of data types improves query performance.</span></span>

<span data-ttu-id="e05cb-106">SQL Data Warehouse podporuje nejčastěji používané datové typy.</span><span class="sxs-lookup"><span data-stu-id="e05cb-106">SQL Data Warehouse supports the most commonly used data types.</span></span> <span data-ttu-id="e05cb-107">Seznam podporované datové typy, naleznete v části [datové typy](/sql/docs/t-sql/statements/create-table-azure-sql-data-warehouse.md#datatypes) v příkazu CREATE TABLE.</span><span class="sxs-lookup"><span data-stu-id="e05cb-107">For a list of the supported data types, see [data types](/sql/docs/t-sql/statements/create-table-azure-sql-data-warehouse.md#datatypes) in the CREATE TABLE statement.</span></span> 


## <a name="minimize-row-length"></a><span data-ttu-id="e05cb-108">Minimalizovat délka řádku</span><span class="sxs-lookup"><span data-stu-id="e05cb-108">Minimize row length</span></span>
<span data-ttu-id="e05cb-109">Minimalizace velikost datových typů zkracuje délku řádku, což vede k lepší výkon dotazů.</span><span class="sxs-lookup"><span data-stu-id="e05cb-109">Minimizing the size of data types shortens the row length, which leads to better query performance.</span></span> <span data-ttu-id="e05cb-110">Použijte nejmenší datový typ, který funguje pro vaše data.</span><span class="sxs-lookup"><span data-stu-id="e05cb-110">Use the smallest data type that works for your data.</span></span> 

- <span data-ttu-id="e05cb-111">Nedefinujte znak sloupce s délkou velké výchozí.</span><span class="sxs-lookup"><span data-stu-id="e05cb-111">Avoid defining character columns with a large default length.</span></span> <span data-ttu-id="e05cb-112">Například pokud nejdelší hodnota je 25 znaků, pak definujte vaše sloupec jako VARCHAR(25).</span><span class="sxs-lookup"><span data-stu-id="e05cb-112">For example, if the longest value is 25 characters, then define your column as VARCHAR(25).</span></span> 
- <span data-ttu-id="e05cb-113">Nepoužívejte [NVARCHAR] [ NVARCHAR] když potřebujete jenom VARCHAR.</span><span class="sxs-lookup"><span data-stu-id="e05cb-113">Avoid using [NVARCHAR][NVARCHAR] when you only need VARCHAR.</span></span>
- <span data-ttu-id="e05cb-114">Pokud je to možné, použijte místo NVARCHAR(MAX) nebo VARCHAR(MAX) NVARCHAR(4000) nebo VARCHAR(8000).</span><span class="sxs-lookup"><span data-stu-id="e05cb-114">When possible, use NVARCHAR(4000) or VARCHAR(8000) instead of NVARCHAR(MAX) or VARCHAR(MAX).</span></span>

<span data-ttu-id="e05cb-115">Pokud používáte Polybase načíst vaše tabulky, nesmí překročit definované délka řádku tabulky 1 MB.</span><span class="sxs-lookup"><span data-stu-id="e05cb-115">If you are using Polybase to load your tables, the defined length of the table row cannot exceed 1 MB.</span></span> <span data-ttu-id="e05cb-116">Pokud řádek s proměnnou délkou dat překročí 1 MB, můžete načíst řádek pomocí BCP, ale ne pomocí funkce PolyBase.</span><span class="sxs-lookup"><span data-stu-id="e05cb-116">When a row with variable-length data exceeds 1 MB, you can load the row with BCP, but not with PolyBase.</span></span>

## <a name="identify-unsupported-data-types"></a><span data-ttu-id="e05cb-117">Identifikovat nepodporované datové typy</span><span class="sxs-lookup"><span data-stu-id="e05cb-117">Identify unsupported data types</span></span>
<span data-ttu-id="e05cb-118">Pokud migrujete z jiné databáze SQL databáze, můžete se setkat datové typy, které nejsou podporované v SQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="e05cb-118">If you are migrating your database from another SQL database, you might encounter data types that are not supported in SQL Data Warehouse.</span></span> <span data-ttu-id="e05cb-119">Pomocí tohoto dotazu ke zjištění nepodporované datové typy ve vaší stávající schématu SQL.</span><span class="sxs-lookup"><span data-stu-id="e05cb-119">Use this query to discover unsupported data types in your existing SQL schema.</span></span>

```sql
SELECT  t.[name], c.[name], c.[system_type_id], c.[user_type_id], y.[is_user_defined], y.[name]
FROM sys.tables  t
JOIN sys.columns c on t.[object_id]    = c.[object_id]
JOIN sys.types   y on c.[user_type_id] = y.[user_type_id]
WHERE y.[name] IN ('geography','geometry','hierarchyid','image','text','ntext','sql_variant','timestamp','xml')
 AND  y.[is_user_defined] = 1;
```


## <span data-ttu-id="e05cb-120"><a name="unsupported-data-types"></a>Alternativní řešení pomocí nepodporované datové typy</span><span class="sxs-lookup"><span data-stu-id="e05cb-120"><a name="unsupported-data-types"></a>Use workarounds for unsupported data types</span></span>

<span data-ttu-id="e05cb-121">Následující seznam obsahuje typy dat, že SQL Data Warehouse nepodporuje a alternativy, které můžete použít místo nepodporované datové typy.</span><span class="sxs-lookup"><span data-stu-id="e05cb-121">The following list shows the data types that SQL Data Warehouse does not support and gives alternatives that you can use instead of the unsupported data types.</span></span>

| <span data-ttu-id="e05cb-122">Nepodporovaný datový typ.</span><span class="sxs-lookup"><span data-stu-id="e05cb-122">Unsupported data type</span></span> | <span data-ttu-id="e05cb-123">Alternativní řešení</span><span class="sxs-lookup"><span data-stu-id="e05cb-123">Workaround</span></span> |
| --- | --- |
| <span data-ttu-id="e05cb-124">[geometrie][geometry]</span><span class="sxs-lookup"><span data-stu-id="e05cb-124">[geometry][geometry]</span></span> |<span data-ttu-id="e05cb-125">[varbinary][varbinary]</span><span class="sxs-lookup"><span data-stu-id="e05cb-125">[varbinary][varbinary]</span></span> |
| <span data-ttu-id="e05cb-126">[Geography][geography]</span><span class="sxs-lookup"><span data-stu-id="e05cb-126">[geography][geography]</span></span> |<span data-ttu-id="e05cb-127">[varbinary][varbinary]</span><span class="sxs-lookup"><span data-stu-id="e05cb-127">[varbinary][varbinary]</span></span> |
| <span data-ttu-id="e05cb-128">[ID hierarchie][hierarchyid]</span><span class="sxs-lookup"><span data-stu-id="e05cb-128">[hierarchyid][hierarchyid]</span></span> |<span data-ttu-id="e05cb-129">[nvarchar][nvarchar](4000)</span><span class="sxs-lookup"><span data-stu-id="e05cb-129">[nvarchar][nvarchar](4000)</span></span> |
| <span data-ttu-id="e05cb-130">[bitové kopie][ntext,text,image]</span><span class="sxs-lookup"><span data-stu-id="e05cb-130">[image][ntext,text,image]</span></span> |<span data-ttu-id="e05cb-131">[varbinary][varbinary]</span><span class="sxs-lookup"><span data-stu-id="e05cb-131">[varbinary][varbinary]</span></span> |
| <span data-ttu-id="e05cb-132">[text][ntext,text,image]</span><span class="sxs-lookup"><span data-stu-id="e05cb-132">[text][ntext,text,image]</span></span> |<span data-ttu-id="e05cb-133">[varchar][varchar]</span><span class="sxs-lookup"><span data-stu-id="e05cb-133">[varchar][varchar]</span></span> |
| <span data-ttu-id="e05cb-134">[ntext][ntext,text,image]</span><span class="sxs-lookup"><span data-stu-id="e05cb-134">[ntext][ntext,text,image]</span></span> |<span data-ttu-id="e05cb-135">[nvarchar][nvarchar]</span><span class="sxs-lookup"><span data-stu-id="e05cb-135">[nvarchar][nvarchar]</span></span> |
| <span data-ttu-id="e05cb-136">[SQL_VARIANT][sql_variant]</span><span class="sxs-lookup"><span data-stu-id="e05cb-136">[sql_variant][sql_variant]</span></span> |<span data-ttu-id="e05cb-137">Rozdělí sloupec do několika sloupců silného typu.</span><span class="sxs-lookup"><span data-stu-id="e05cb-137">Split column into several strongly typed columns.</span></span> |
| <span data-ttu-id="e05cb-138">[Tabulka][table]</span><span class="sxs-lookup"><span data-stu-id="e05cb-138">[table][table]</span></span> |<span data-ttu-id="e05cb-139">Převeďte na dočasné tabulky.</span><span class="sxs-lookup"><span data-stu-id="e05cb-139">Convert to temporary tables.</span></span> |
| <span data-ttu-id="e05cb-140">[časové razítko][timestamp]</span><span class="sxs-lookup"><span data-stu-id="e05cb-140">[timestamp][timestamp]</span></span> |<span data-ttu-id="e05cb-141">Přepracování kódu pro použití [datetime2] [ datetime2] a `CURRENT_TIMESTAMP` funkce.</span><span class="sxs-lookup"><span data-stu-id="e05cb-141">Rework code to use [datetime2][datetime2] and `CURRENT_TIMESTAMP` function.</span></span>  <span data-ttu-id="e05cb-142">Jsou podporovány pouze konstanty jako výchozí, proto current_timestamp nelze definovat jako výchozí omezení.</span><span class="sxs-lookup"><span data-stu-id="e05cb-142">Only constants are supported as defaults, therefore current_timestamp cannot be defined as a default constraint.</span></span> <span data-ttu-id="e05cb-143">Pokud potřebujete migrovat hodnoty verze řádků z typu sloupec časového razítka, použijte [binární][BINARY](8) nebo [VARBINARY][BINARY](8) pro NOT NULL nebo Řádek verze hodnoty NULL.</span><span class="sxs-lookup"><span data-stu-id="e05cb-143">If you need to migrate row version values from a timestamp typed column, then use [BINARY][BINARY](8) or [VARBINARY][BINARY](8) for NOT NULL or NULL row version values.</span></span> |
| <span data-ttu-id="e05cb-144">[XML][xml]</span><span class="sxs-lookup"><span data-stu-id="e05cb-144">[xml][xml]</span></span> |<span data-ttu-id="e05cb-145">[varchar][varchar]</span><span class="sxs-lookup"><span data-stu-id="e05cb-145">[varchar][varchar]</span></span> |
| <span data-ttu-id="e05cb-146">[uživatelsky definovaný typ.][user defined types]</span><span class="sxs-lookup"><span data-stu-id="e05cb-146">[user-defined type][user defined types]</span></span> |<span data-ttu-id="e05cb-147">Převeďte zpátky na nativní datový typ, pokud je to možné.</span><span class="sxs-lookup"><span data-stu-id="e05cb-147">Convert back to the native data type when possible.</span></span> |
| <span data-ttu-id="e05cb-148">výchozí hodnoty</span><span class="sxs-lookup"><span data-stu-id="e05cb-148">default values</span></span> | <span data-ttu-id="e05cb-149">Výchozí hodnoty podporují literály a pouze konstanty.</span><span class="sxs-lookup"><span data-stu-id="e05cb-149">Default values support literals and constants only.</span></span>  <span data-ttu-id="e05cb-150">Nedeterministické výrazy nebo funkce, jako například `GETDATE()` nebo `CURRENT_TIMESTAMP`, nejsou podporovány.</span><span class="sxs-lookup"><span data-stu-id="e05cb-150">Non-deterministic expressions or functions, such as `GETDATE()` or `CURRENT_TIMESTAMP`, are not supported.</span></span> |


## <a name="next-steps"></a><span data-ttu-id="e05cb-151">Další kroky</span><span class="sxs-lookup"><span data-stu-id="e05cb-151">Next steps</span></span>
<span data-ttu-id="e05cb-152">Další informace naleznete v tématu:</span><span class="sxs-lookup"><span data-stu-id="e05cb-152">To learn more, see:</span></span>

- <span data-ttu-id="e05cb-153">[SQL Data Warehouse osvědčené postupy][SQL Data Warehouse Best Practices]</span><span class="sxs-lookup"><span data-stu-id="e05cb-153">[SQL Data Warehouse Best Practices][SQL Data Warehouse Best Practices]</span></span>
- <span data-ttu-id="e05cb-154">[Tabulka – přehled][Overview]</span><span class="sxs-lookup"><span data-stu-id="e05cb-154">[Table Overview][Overview]</span></span>
- <span data-ttu-id="e05cb-155">[Distribuce tabulky][Distribute]</span><span class="sxs-lookup"><span data-stu-id="e05cb-155">[Distributing a Table][Distribute]</span></span>
- <span data-ttu-id="e05cb-156">[Indexování tabulky][Index]</span><span class="sxs-lookup"><span data-stu-id="e05cb-156">[Indexing a Table][Index]</span></span>
- <span data-ttu-id="e05cb-157">[Vytváření oddílů tabulky][Partition]</span><span class="sxs-lookup"><span data-stu-id="e05cb-157">[Partitioning a Table][Partition]</span></span>
- <span data-ttu-id="e05cb-158">[Zachování statistiky tabulky][Statistics]</span><span class="sxs-lookup"><span data-stu-id="e05cb-158">[Maintaining Table Statistics][Statistics]</span></span>
- <span data-ttu-id="e05cb-159">[Dočasné tabulky][Temporary]</span><span class="sxs-lookup"><span data-stu-id="e05cb-159">[Temporary Tables][Temporary]</span></span>

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
[create table]: https://msdn.microsoft.com/library/mt203953.aspx
[bigint]: https://msdn.microsoft.com/library/ms187745.aspx
[binary]: https://msdn.microsoft.com/library/ms188362.aspx
[bit]: https://msdn.microsoft.com/library/ms177603.aspx
[char]: https://msdn.microsoft.com/library/ms176089.aspx
[date]: https://msdn.microsoft.com/library/bb630352.aspx
[datetime]: https://msdn.microsoft.com/library/ms187819.aspx
[datetime2]: https://msdn.microsoft.com/library/bb677335.aspx
[datetimeoffset]: https://msdn.microsoft.com/library/bb630289.aspx
[decimal]: https://msdn.microsoft.com/library/ms187746.aspx
[float]: https://msdn.microsoft.com/library/ms173773.aspx
[geometry]: https://msdn.microsoft.com/library/cc280487.aspx
[geography]: https://msdn.microsoft.com/library/cc280766.aspx
[hierarchyid]: https://msdn.microsoft.com/library/bb677290.aspx
[int]: https://msdn.microsoft.com/library/ms187745.aspx
[money]: https://msdn.microsoft.com/library/ms179882.aspx
[nchar]: https://msdn.microsoft.com/library/ms186939.aspx
[nvarchar]: https://msdn.microsoft.com/library/ms186939.aspx
[ntext,text,image]: https://msdn.microsoft.com/library/ms187993.aspx
[real]: https://msdn.microsoft.com/library/ms173773.aspx
[smalldatetime]: https://msdn.microsoft.com/library/ms182418.aspx
[smallint]: https://msdn.microsoft.com/library/ms187745.aspx
[smallmoney]: https://msdn.microsoft.com/library/ms179882.aspx
[sql_variant]: https://msdn.microsoft.com/library/ms173829.aspx
[sysname]: https://msdn.microsoft.com/library/ms186939.aspx
[table]: https://msdn.microsoft.com/library/ms175010.aspx
[time]: https://msdn.microsoft.com/library/bb677243.aspx
[timestamp]: https://msdn.microsoft.com/library/ms182776.aspx
[tinyint]: https://msdn.microsoft.com/library/ms187745.aspx
[uniqueidentifier]: https://msdn.microsoft.com/library/ms187942.aspx
[varbinary]: https://msdn.microsoft.com/library/ms188362.aspx
[varchar]: https://msdn.microsoft.com/library/ms186939.aspx
[xml]: https://msdn.microsoft.com/library/ms187339.aspx
[user defined types]: https://msdn.microsoft.com/library/ms131694.aspx
