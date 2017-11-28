---
title: aaaData typy pokyny - Azure SQL Data Warehouse | Microsoft Docs
description: "Doporučení, že toodefine datové typy, které jsou kompatibilní s datovým skladem SQL."
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
ms.openlocfilehash: a2f7a394feb73d273b25101735b00eb12db2b292
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="guidance-for-defining-data-types-for-tables-in-sql-data-warehouse"></a><span data-ttu-id="490ea-103">Pokyny pro definování typů dat pro tabulky v SQL Data Warehouse</span><span class="sxs-lookup"><span data-stu-id="490ea-103">Guidance for defining data types for tables in SQL Data Warehouse</span></span>
<span data-ttu-id="490ea-104">Pomocí těchto doporučení toodefine tabulky typy dat, které jsou kompatibilní s SQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="490ea-104">Use these recommendations toodefine table data types that are compatible with SQL Data Warehouse.</span></span> <span data-ttu-id="490ea-105">Kromě toho toocompatibility, minimalizovat hello velikost datových typů zlepšuje výkon dotazů.</span><span class="sxs-lookup"><span data-stu-id="490ea-105">In addition toocompatibility, minimizing hello size of data types improves query performance.</span></span>

<span data-ttu-id="490ea-106">SQL Data Warehouse podporuje hello nejčastěji používané datové typy.</span><span class="sxs-lookup"><span data-stu-id="490ea-106">SQL Data Warehouse supports hello most commonly used data types.</span></span> <span data-ttu-id="490ea-107">Seznam hello podporované datové typy, naleznete v části [datové typy](/sql/docs/t-sql/statements/create-table-azure-sql-data-warehouse.md#datatypes) v hello příkazu CREATE TABLE.</span><span class="sxs-lookup"><span data-stu-id="490ea-107">For a list of hello supported data types, see [data types](/sql/docs/t-sql/statements/create-table-azure-sql-data-warehouse.md#datatypes) in hello CREATE TABLE statement.</span></span> 


## <a name="minimize-row-length"></a><span data-ttu-id="490ea-108">Minimalizovat délka řádku</span><span class="sxs-lookup"><span data-stu-id="490ea-108">Minimize row length</span></span>
<span data-ttu-id="490ea-109">Minimalizace hello velikost datových typů zkracuje délku řádku hello, což vede toobetter výkon dotazů.</span><span class="sxs-lookup"><span data-stu-id="490ea-109">Minimizing hello size of data types shortens hello row length, which leads toobetter query performance.</span></span> <span data-ttu-id="490ea-110">Použití hello nejmenší datový typ, který se dá použít pro vaše data.</span><span class="sxs-lookup"><span data-stu-id="490ea-110">Use hello smallest data type that works for your data.</span></span> 

- <span data-ttu-id="490ea-111">Nedefinujte znak sloupce s délkou velké výchozí.</span><span class="sxs-lookup"><span data-stu-id="490ea-111">Avoid defining character columns with a large default length.</span></span> <span data-ttu-id="490ea-112">Například pokud hello nejdelší hodnota je 25 znaků, pak definujte vaše sloupec jako VARCHAR(25).</span><span class="sxs-lookup"><span data-stu-id="490ea-112">For example, if hello longest value is 25 characters, then define your column as VARCHAR(25).</span></span> 
- <span data-ttu-id="490ea-113">Nepoužívejte [NVARCHAR] [ NVARCHAR] když potřebujete jenom VARCHAR.</span><span class="sxs-lookup"><span data-stu-id="490ea-113">Avoid using [NVARCHAR][NVARCHAR] when you only need VARCHAR.</span></span>
- <span data-ttu-id="490ea-114">Pokud je to možné, použijte místo NVARCHAR(MAX) nebo VARCHAR(MAX) NVARCHAR(4000) nebo VARCHAR(8000).</span><span class="sxs-lookup"><span data-stu-id="490ea-114">When possible, use NVARCHAR(4000) or VARCHAR(8000) instead of NVARCHAR(MAX) or VARCHAR(MAX).</span></span>

<span data-ttu-id="490ea-115">Pokud používáte Polybase tooload vaše tabulky, nesmí překročit délka hello definované řádku tabulky hello 1 MB.</span><span class="sxs-lookup"><span data-stu-id="490ea-115">If you are using Polybase tooload your tables, hello defined length of hello table row cannot exceed 1 MB.</span></span> <span data-ttu-id="490ea-116">Pokud řádek s proměnnou délkou dat překročí 1 MB, můžete načíst řádek hello pomocí BCP, ale ne pomocí funkce PolyBase.</span><span class="sxs-lookup"><span data-stu-id="490ea-116">When a row with variable-length data exceeds 1 MB, you can load hello row with BCP, but not with PolyBase.</span></span>

## <a name="identify-unsupported-data-types"></a><span data-ttu-id="490ea-117">Identifikovat nepodporované datové typy</span><span class="sxs-lookup"><span data-stu-id="490ea-117">Identify unsupported data types</span></span>
<span data-ttu-id="490ea-118">Pokud migrujete z jiné databáze SQL databáze, můžete se setkat datové typy, které nejsou podporované v SQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="490ea-118">If you are migrating your database from another SQL database, you might encounter data types that are not supported in SQL Data Warehouse.</span></span> <span data-ttu-id="490ea-119">Použijte tento dotaz toodiscover nepodporované datové typy ve vaší stávající schématu SQL.</span><span class="sxs-lookup"><span data-stu-id="490ea-119">Use this query toodiscover unsupported data types in your existing SQL schema.</span></span>

```sql
SELECT  t.[name], c.[name], c.[system_type_id], c.[user_type_id], y.[is_user_defined], y.[name]
FROM sys.tables  t
JOIN sys.columns c on t.[object_id]    = c.[object_id]
JOIN sys.types   y on c.[user_type_id] = y.[user_type_id]
WHERE y.[name] IN ('geography','geometry','hierarchyid','image','text','ntext','sql_variant','timestamp','xml')
 AND  y.[is_user_defined] = 1;
```


## <span data-ttu-id="490ea-120"><a name="unsupported-data-types"></a>Alternativní řešení pomocí nepodporované datové typy</span><span class="sxs-lookup"><span data-stu-id="490ea-120"><a name="unsupported-data-types"></a>Use workarounds for unsupported data types</span></span>

<span data-ttu-id="490ea-121">Hello následujícím seznamu jsou uvedeny hello typy dat, které nepodporuje SQL Data Warehouse a poskytuje alternativy, které můžete použít místo hello nepodporované datové typy.</span><span class="sxs-lookup"><span data-stu-id="490ea-121">hello following list shows hello data types that SQL Data Warehouse does not support and gives alternatives that you can use instead of hello unsupported data types.</span></span>

| <span data-ttu-id="490ea-122">Nepodporovaný datový typ.</span><span class="sxs-lookup"><span data-stu-id="490ea-122">Unsupported data type</span></span> | <span data-ttu-id="490ea-123">Alternativní řešení</span><span class="sxs-lookup"><span data-stu-id="490ea-123">Workaround</span></span> |
| --- | --- |
| <span data-ttu-id="490ea-124">[geometrie][geometry]</span><span class="sxs-lookup"><span data-stu-id="490ea-124">[geometry][geometry]</span></span> |<span data-ttu-id="490ea-125">[varbinary][varbinary]</span><span class="sxs-lookup"><span data-stu-id="490ea-125">[varbinary][varbinary]</span></span> |
| <span data-ttu-id="490ea-126">[Geography][geography]</span><span class="sxs-lookup"><span data-stu-id="490ea-126">[geography][geography]</span></span> |<span data-ttu-id="490ea-127">[varbinary][varbinary]</span><span class="sxs-lookup"><span data-stu-id="490ea-127">[varbinary][varbinary]</span></span> |
| <span data-ttu-id="490ea-128">[ID hierarchie][hierarchyid]</span><span class="sxs-lookup"><span data-stu-id="490ea-128">[hierarchyid][hierarchyid]</span></span> |<span data-ttu-id="490ea-129">[nvarchar][nvarchar](4000)</span><span class="sxs-lookup"><span data-stu-id="490ea-129">[nvarchar][nvarchar](4000)</span></span> |
| <span data-ttu-id="490ea-130">[bitové kopie][ntext,text,image]</span><span class="sxs-lookup"><span data-stu-id="490ea-130">[image][ntext,text,image]</span></span> |<span data-ttu-id="490ea-131">[varbinary][varbinary]</span><span class="sxs-lookup"><span data-stu-id="490ea-131">[varbinary][varbinary]</span></span> |
| <span data-ttu-id="490ea-132">[text][ntext,text,image]</span><span class="sxs-lookup"><span data-stu-id="490ea-132">[text][ntext,text,image]</span></span> |<span data-ttu-id="490ea-133">[varchar][varchar]</span><span class="sxs-lookup"><span data-stu-id="490ea-133">[varchar][varchar]</span></span> |
| <span data-ttu-id="490ea-134">[ntext][ntext,text,image]</span><span class="sxs-lookup"><span data-stu-id="490ea-134">[ntext][ntext,text,image]</span></span> |<span data-ttu-id="490ea-135">[nvarchar][nvarchar]</span><span class="sxs-lookup"><span data-stu-id="490ea-135">[nvarchar][nvarchar]</span></span> |
| <span data-ttu-id="490ea-136">[SQL_VARIANT][sql_variant]</span><span class="sxs-lookup"><span data-stu-id="490ea-136">[sql_variant][sql_variant]</span></span> |<span data-ttu-id="490ea-137">Rozdělí sloupec do několika sloupců silného typu.</span><span class="sxs-lookup"><span data-stu-id="490ea-137">Split column into several strongly typed columns.</span></span> |
| <span data-ttu-id="490ea-138">[Tabulka][table]</span><span class="sxs-lookup"><span data-stu-id="490ea-138">[table][table]</span></span> |<span data-ttu-id="490ea-139">Převeďte tootemporary tabulky.</span><span class="sxs-lookup"><span data-stu-id="490ea-139">Convert tootemporary tables.</span></span> |
| <span data-ttu-id="490ea-140">[časové razítko][timestamp]</span><span class="sxs-lookup"><span data-stu-id="490ea-140">[timestamp][timestamp]</span></span> |<span data-ttu-id="490ea-141">Přepracování kódu toouse [datetime2] [ datetime2] a `CURRENT_TIMESTAMP` funkce.</span><span class="sxs-lookup"><span data-stu-id="490ea-141">Rework code toouse [datetime2][datetime2] and `CURRENT_TIMESTAMP` function.</span></span>  <span data-ttu-id="490ea-142">Jsou podporovány pouze konstanty jako výchozí, proto current_timestamp nelze definovat jako výchozí omezení.</span><span class="sxs-lookup"><span data-stu-id="490ea-142">Only constants are supported as defaults, therefore current_timestamp cannot be defined as a default constraint.</span></span> <span data-ttu-id="490ea-143">Pokud potřebujete hodnoty verze toomigrate řádků ze zadaných sloupec časového razítka, použijte [binární][BINARY](8) nebo [VARBINARY][BINARY](8) pro NOT NULL nebo Řádek verze hodnoty NULL.</span><span class="sxs-lookup"><span data-stu-id="490ea-143">If you need toomigrate row version values from a timestamp typed column, then use [BINARY][BINARY](8) or [VARBINARY][BINARY](8) for NOT NULL or NULL row version values.</span></span> |
| <span data-ttu-id="490ea-144">[XML][xml]</span><span class="sxs-lookup"><span data-stu-id="490ea-144">[xml][xml]</span></span> |<span data-ttu-id="490ea-145">[varchar][varchar]</span><span class="sxs-lookup"><span data-stu-id="490ea-145">[varchar][varchar]</span></span> |
| <span data-ttu-id="490ea-146">[uživatelsky definovaný typ.][user defined types]</span><span class="sxs-lookup"><span data-stu-id="490ea-146">[user-defined type][user defined types]</span></span> |<span data-ttu-id="490ea-147">Převeďte zpět toohello nativní datový typ, pokud je to možné.</span><span class="sxs-lookup"><span data-stu-id="490ea-147">Convert back toohello native data type when possible.</span></span> |
| <span data-ttu-id="490ea-148">výchozí hodnoty</span><span class="sxs-lookup"><span data-stu-id="490ea-148">default values</span></span> | <span data-ttu-id="490ea-149">Výchozí hodnoty podporují literály a pouze konstanty.</span><span class="sxs-lookup"><span data-stu-id="490ea-149">Default values support literals and constants only.</span></span>  <span data-ttu-id="490ea-150">Nedeterministické výrazy nebo funkce, jako například `GETDATE()` nebo `CURRENT_TIMESTAMP`, nejsou podporovány.</span><span class="sxs-lookup"><span data-stu-id="490ea-150">Non-deterministic expressions or functions, such as `GETDATE()` or `CURRENT_TIMESTAMP`, are not supported.</span></span> |


## <a name="next-steps"></a><span data-ttu-id="490ea-151">Další kroky</span><span class="sxs-lookup"><span data-stu-id="490ea-151">Next steps</span></span>
<span data-ttu-id="490ea-152">toolearn více, najdete v části:</span><span class="sxs-lookup"><span data-stu-id="490ea-152">toolearn more, see:</span></span>

- <span data-ttu-id="490ea-153">[SQL Data Warehouse osvědčené postupy][SQL Data Warehouse Best Practices]</span><span class="sxs-lookup"><span data-stu-id="490ea-153">[SQL Data Warehouse Best Practices][SQL Data Warehouse Best Practices]</span></span>
- <span data-ttu-id="490ea-154">[Tabulka – přehled][Overview]</span><span class="sxs-lookup"><span data-stu-id="490ea-154">[Table Overview][Overview]</span></span>
- <span data-ttu-id="490ea-155">[Distribuce tabulky][Distribute]</span><span class="sxs-lookup"><span data-stu-id="490ea-155">[Distributing a Table][Distribute]</span></span>
- <span data-ttu-id="490ea-156">[Indexování tabulky][Index]</span><span class="sxs-lookup"><span data-stu-id="490ea-156">[Indexing a Table][Index]</span></span>
- <span data-ttu-id="490ea-157">[Vytváření oddílů tabulky][Partition]</span><span class="sxs-lookup"><span data-stu-id="490ea-157">[Partitioning a Table][Partition]</span></span>
- <span data-ttu-id="490ea-158">[Zachování statistiky tabulky][Statistics]</span><span class="sxs-lookup"><span data-stu-id="490ea-158">[Maintaining Table Statistics][Statistics]</span></span>
- <span data-ttu-id="490ea-159">[Dočasné tabulky][Temporary]</span><span class="sxs-lookup"><span data-stu-id="490ea-159">[Temporary Tables][Temporary]</span></span>

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
