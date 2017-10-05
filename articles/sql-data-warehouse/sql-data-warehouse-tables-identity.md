---
title: "Vytvoření klíčů náhradní pomocí IDENTITY | Microsoft Docs"
description: "Další informace o použití IDENTITY k vytváření klíčů náhradní na tabulky."
services: sql-data-warehouse
documentationcenter: NA
author: jrowlandjones
manager: jhubbard
editor: 
ms.assetid: faa1034d-314c-4f9d-af81-f5a9aedf33e4
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.date: 06/13/2017
ms.author: jrj;barbkess
ms.openlocfilehash: 3ab5d159e6eaeb830135fe134e108b0e4de4b7d6
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="create-surrogate-keys-by-using-identity"></a><span data-ttu-id="069c1-103">Vytvoření klíčů náhradní pomocí IDENTITY</span><span class="sxs-lookup"><span data-stu-id="069c1-103">Create surrogate keys by using IDENTITY</span></span>
> [!div class="op_single_selector"]
> * <span data-ttu-id="069c1-104">[Přehled][Overview]</span><span class="sxs-lookup"><span data-stu-id="069c1-104">[Overview][Overview]</span></span>
> * <span data-ttu-id="069c1-105">[Datové typy][Data Types]</span><span class="sxs-lookup"><span data-stu-id="069c1-105">[Data types][Data Types]</span></span>
> * <span data-ttu-id="069c1-106">[Distribuce][Distribute]</span><span class="sxs-lookup"><span data-stu-id="069c1-106">[Distribute][Distribute]</span></span>
> * <span data-ttu-id="069c1-107">[Index][Index]</span><span class="sxs-lookup"><span data-stu-id="069c1-107">[Index][Index]</span></span>
> * <span data-ttu-id="069c1-108">[Oddíl][Partition]</span><span class="sxs-lookup"><span data-stu-id="069c1-108">[Partition][Partition]</span></span>
> * <span data-ttu-id="069c1-109">[Statistiky][Statistics]</span><span class="sxs-lookup"><span data-stu-id="069c1-109">[Statistics][Statistics]</span></span>
> * <span data-ttu-id="069c1-110">[Dočasné][Temporary]</span><span class="sxs-lookup"><span data-stu-id="069c1-110">[Temporary][Temporary]</span></span>
> * <span data-ttu-id="069c1-111">[Identity][Identity]</span><span class="sxs-lookup"><span data-stu-id="069c1-111">[Identity][Identity]</span></span>
> 
> 

<span data-ttu-id="069c1-112">Mnoho modelování dat, například k vytváření klíčů náhradní na jejich tabulky při návrhu modely datového skladu.</span><span class="sxs-lookup"><span data-stu-id="069c1-112">Many data modelers like to create surrogate keys on their tables when they design data warehouse models.</span></span> <span data-ttu-id="069c1-113">Vlastnost IDENTITY můžete použít k dosažení tohoto cíle jednoduše a efektivně, aniž by to ovlivnilo zatížení výkonu.</span><span class="sxs-lookup"><span data-stu-id="069c1-113">You can use the IDENTITY property to achieve this goal simply and effectively without affecting load performance.</span></span> 

## <a name="get-started-with-identity"></a><span data-ttu-id="069c1-114">Začínáme s identitou</span><span class="sxs-lookup"><span data-stu-id="069c1-114">Get started with IDENTITY</span></span>
<span data-ttu-id="069c1-115">Můžete definovat tabulku tak, že má vlastnost IDENTITY při prvním vytvoření tabulky pomocí syntaxe, který je podobný následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="069c1-115">You can define a table as having the IDENTITY property when you first create the table by using syntax that is similar to the following statement:</span></span>

```sql
CREATE TABLE dbo.T1
(   C1 INT IDENTITY(1,1) NOT NULL
,   C2 INT NULL
)
WITH
(   DISTRIBUTION = HASH(C2)
,   CLUSTERED COLUMNSTORE INDEX
)
;
```

<span data-ttu-id="069c1-116">Pak můžete použít `INSERT..SELECT` k vyplnění tabulky.</span><span class="sxs-lookup"><span data-stu-id="069c1-116">You can then use `INSERT..SELECT` to populate the table.</span></span>

## <a name="behavior"></a><span data-ttu-id="069c1-117">Chování</span><span class="sxs-lookup"><span data-stu-id="069c1-117">Behavior</span></span>
<span data-ttu-id="069c1-118">Vlastnost IDENTITY je určena pro škálovat napříč všechny distribuce v datovém skladu, aniž by to ovlivnilo zatížení výkonu.</span><span class="sxs-lookup"><span data-stu-id="069c1-118">The IDENTITY property is designed to scale out across all the distributions in the data warehouse without affecting load performance.</span></span> <span data-ttu-id="069c1-119">Implementace IDENTITY je proto orientované na dosažení těchto cílů.</span><span class="sxs-lookup"><span data-stu-id="069c1-119">Therefore, the implementation of IDENTITY is oriented toward achieving these goals.</span></span> <span data-ttu-id="069c1-120">V této části jsou zdůrazněné drobné odlišnosti ve implementaci vám pomohou pochopit je podrobněji.</span><span class="sxs-lookup"><span data-stu-id="069c1-120">This section highlights the nuances of the implementation to help you understand them more fully.</span></span>  

### <a name="allocation-of-values"></a><span data-ttu-id="069c1-121">Přidělování hodnot</span><span class="sxs-lookup"><span data-stu-id="069c1-121">Allocation of values</span></span>
<span data-ttu-id="069c1-122">Vlastnost IDENTITY nezaručuje pořadí, ve kterém jsou přiděleny náhradní hodnoty, které odráží chování systému SQL Server a databáze SQL Azure.</span><span class="sxs-lookup"><span data-stu-id="069c1-122">The IDENTITY property doesn't guarantee the order in which the surrogate values are allocated, which reflects the behavior of SQL Server and Azure SQL Database.</span></span> <span data-ttu-id="069c1-123">Ale v Azure SQL Data Warehouse absenci záruku je mnohem výraznější.</span><span class="sxs-lookup"><span data-stu-id="069c1-123">However, in Azure SQL Data Warehouse, the absence of a guarantee is more pronounced.</span></span> 

<span data-ttu-id="069c1-124">V následujícím příkladu je obrázek:</span><span class="sxs-lookup"><span data-stu-id="069c1-124">The following example is an illustration:</span></span>

```sql
CREATE TABLE dbo.T1
(   C1 INT IDENTITY(1,1)    NOT NULL
,   C2 VARCHAR(30)              NULL
)
WITH
(   DISTRIBUTION = HASH(C2)
,   CLUSTERED COLUMNSTORE INDEX
)
;

INSERT INTO dbo.T1
VALUES (NULL);

INSERT INTO dbo.T1
VALUES (NULL);

SELECT *
FROM dbo.T1;

DBCC PDW_SHOWSPACEUSED('dbo.T1');
```

<span data-ttu-id="069c1-125">V předchozím příkladu dva řádky dostali distribuční 1.</span><span class="sxs-lookup"><span data-stu-id="069c1-125">In the preceding example, two rows landed in distribution 1.</span></span> <span data-ttu-id="069c1-126">První řádek má náhradní hodnotu 1 ve sloupci `C1`, a druhý řádek obsahuje náhradní hodnota 61.</span><span class="sxs-lookup"><span data-stu-id="069c1-126">The first row has the surrogate value of 1 in column `C1`, and the second row has the surrogate value of 61.</span></span> <span data-ttu-id="069c1-127">Obě tyto hodnoty byly vygenerovány vlastnost IDENTITY.</span><span class="sxs-lookup"><span data-stu-id="069c1-127">Both of these values were generated by the IDENTITY property.</span></span> <span data-ttu-id="069c1-128">Přidělení hodnoty však není souvislé.</span><span class="sxs-lookup"><span data-stu-id="069c1-128">However, the allocation of the values is not contiguous.</span></span> <span data-ttu-id="069c1-129">Toto chování je záměrné.</span><span class="sxs-lookup"><span data-stu-id="069c1-129">This behavior is by design.</span></span>

### <a name="skewed-data"></a><span data-ttu-id="069c1-130">Zkreslilo dat</span><span class="sxs-lookup"><span data-stu-id="069c1-130">Skewed data</span></span> 
<span data-ttu-id="069c1-131">Rozsah hodnot pro datový typ jsou rovnoměrně rozloženy distribuce.</span><span class="sxs-lookup"><span data-stu-id="069c1-131">The range of values for the data type are spread evenly across the distributions.</span></span> <span data-ttu-id="069c1-132">Pokud se vyskytne distribuované tabulku z zkreslilo dat, můžete rozsah hodnot, které jsou k dispozici pro datový typ vyčerpán předčasně.</span><span class="sxs-lookup"><span data-stu-id="069c1-132">If a distributed table suffers from skewed data, then the range of values available to the datatype can be exhausted prematurely.</span></span> <span data-ttu-id="069c1-133">Například pokud všechna data skončilo v jednom distribučním, pak efektivně tabulka má přístup k jedné pouze šedesátině hodnot datového typu.</span><span class="sxs-lookup"><span data-stu-id="069c1-133">For example, if all the data ends up in a single distribution, then effectively the table has access to only one-sixtieth of the values of the data type.</span></span> <span data-ttu-id="069c1-134">Z tohoto důvodu je omezena na vlastnost IDENTITY `INT` a `BIGINT` datové typy jenom.</span><span class="sxs-lookup"><span data-stu-id="069c1-134">For this reason, the IDENTITY property is limited to `INT` and `BIGINT` data types only.</span></span>

### <a name="selectinto"></a><span data-ttu-id="069c1-135">VYBERTE... DO</span><span class="sxs-lookup"><span data-stu-id="069c1-135">SELECT..INTO</span></span>
<span data-ttu-id="069c1-136">Pokud do nové tabulky je vybraná jako existující sloupec IDENTITY, nového sloupce, který dědí vlastnost Identita, pokud je splněna jedna z následujících podmínek:</span><span class="sxs-lookup"><span data-stu-id="069c1-136">When an existing IDENTITY column is selected into a new table, the new column inherits the IDENTITY property, unless one of the following conditions is true:</span></span>
- <span data-ttu-id="069c1-137">Příkaz SELECT obsahuje spojení.</span><span class="sxs-lookup"><span data-stu-id="069c1-137">The SELECT statement contains a join.</span></span>
- <span data-ttu-id="069c1-138">Více příkazů SELECT připojeni pomocí UNION.</span><span class="sxs-lookup"><span data-stu-id="069c1-138">Multiple SELECT statements are joined by using UNION.</span></span>
- <span data-ttu-id="069c1-139">Sloupec IDENTITY je uveden více než jednou v seznamu SELECT.</span><span class="sxs-lookup"><span data-stu-id="069c1-139">The IDENTITY column is listed more than one time in the SELECT list.</span></span>
- <span data-ttu-id="069c1-140">Sloupec IDENTITY je součástí výrazu.</span><span class="sxs-lookup"><span data-stu-id="069c1-140">The IDENTITY column is part of an expression.</span></span>
    
<span data-ttu-id="069c1-141">Pokud platí jedna z těchto podmínek, vytvoří se sloupci NOT NULL místo dědí vlastnost Identita.</span><span class="sxs-lookup"><span data-stu-id="069c1-141">If any one of these conditions is true, the column is created NOT NULL instead of inheriting the IDENTITY property.</span></span>

### <a name="create-table-as-select"></a><span data-ttu-id="069c1-142">VYTVOŘENÍ TABLE AS SELECT</span><span class="sxs-lookup"><span data-stu-id="069c1-142">CREATE TABLE AS SELECT</span></span>
<span data-ttu-id="069c1-143">Vytvoření tabulky AS vyberte funkce CTAS () odpovídá stejné chování systému SQL Server, která je popsána vyberte... DO.</span><span class="sxs-lookup"><span data-stu-id="069c1-143">CREATE TABLE AS SELECT (CTAS) follows the same SQL Server behavior that's documented for SELECT..INTO.</span></span> <span data-ttu-id="069c1-144">V definici sloupce však nelze určit vlastnost IDENTITY `CREATE TABLE` část příkazu.</span><span class="sxs-lookup"><span data-stu-id="069c1-144">However, you can't specify an IDENTITY property in the column definition of the `CREATE TABLE` part of the statement.</span></span> <span data-ttu-id="069c1-145">Funkci IDENTITY v také nelze použít `SELECT` součástí funkce CTAS.</span><span class="sxs-lookup"><span data-stu-id="069c1-145">You also can't use the IDENTITY function in the `SELECT` part of the CTAS.</span></span> <span data-ttu-id="069c1-146">Vyplnění tabulky, budete muset použít `CREATE TABLE` definovat v tabulce, za nímž následuje `INSERT..SELECT` k jejímu naplnění.</span><span class="sxs-lookup"><span data-stu-id="069c1-146">To populate a table, you need to use `CREATE TABLE` to define the table followed by `INSERT..SELECT` to populate it.</span></span>

## <a name="explicitly-insert-values-into-an-identity-column"></a><span data-ttu-id="069c1-147">Explicitně vložení hodnoty do sloupce IDENTITY</span><span class="sxs-lookup"><span data-stu-id="069c1-147">Explicitly insert values into an IDENTITY column</span></span> 
<span data-ttu-id="069c1-148">SQL Data Warehouse podporuje `SET IDENTITY_INSERT <your table> ON|OFF` syntaxe.</span><span class="sxs-lookup"><span data-stu-id="069c1-148">SQL Data Warehouse supports `SET IDENTITY_INSERT <your table> ON|OFF` syntax.</span></span> <span data-ttu-id="069c1-149">Pomocí této syntaxe explicitně vložení hodnoty do sloupce IDENTITY.</span><span class="sxs-lookup"><span data-stu-id="069c1-149">You can use this syntax to explicitly insert values into the IDENTITY column.</span></span>

<span data-ttu-id="069c1-150">Mnoho modelování dat se chtěli použít předdefinované záporné hodnoty pro některé řádky v jejich dimenzí.</span><span class="sxs-lookup"><span data-stu-id="069c1-150">Many data modelers like to use predefined negative values for certain rows in their dimensions.</span></span> <span data-ttu-id="069c1-151">Příkladem je "neznámého člena" řádek nebo hodnotu -1.</span><span class="sxs-lookup"><span data-stu-id="069c1-151">An example is the -1 or "unknown member" row.</span></span> 

<span data-ttu-id="069c1-152">Další skriptu ukazuje, jak chcete explicitně přidat pomocí nastavení IDENTITY_INSERT tento řádek:</span><span class="sxs-lookup"><span data-stu-id="069c1-152">The next script shows how to explicitly add this row by using SET IDENTITY_INSERT:</span></span>

```sql
SET IDENTITY_INSERT dbo.T1 ON;

INSERT INTO dbo.T1
(   C1
,   C2
)
VALUES (-1,'UNKNOWN')
;

SET IDENTITY_INSERT dbo.T1 OFF;

SELECT  *
FROM    dbo.T1
;
```    

## <a name="load-data-into-a-table-with-identity"></a><span data-ttu-id="069c1-153">Načtení dat do tabulky s identitou</span><span class="sxs-lookup"><span data-stu-id="069c1-153">Load data into a table with IDENTITY</span></span>

<span data-ttu-id="069c1-154">Přítomnost vlastnost IDENTITY má některé důsledky načítání dat kódu.</span><span class="sxs-lookup"><span data-stu-id="069c1-154">The presence of the IDENTITY property has some implications to your data-loading code.</span></span> <span data-ttu-id="069c1-155">V této části jsou zdůrazněné některé základní vzory pro načítání dat do tabulky pomocí IDENTITY.</span><span class="sxs-lookup"><span data-stu-id="069c1-155">This section highlights some basic patterns for loading data into tables by using IDENTITY.</span></span> 

### <a name="load-data-with-polybase"></a><span data-ttu-id="069c1-156">Načítání dat pomocí PolyBase</span><span class="sxs-lookup"><span data-stu-id="069c1-156">Load data with PolyBase</span></span>
<span data-ttu-id="069c1-157">Načtení dat do tabulky a vygenerovat klíč náhradní pomocí IDENTITY, vytvořit v tabulce a pak použít příkaz INSERT... Vyberte nebo VLOŽTE... HODNOTY k provedení zatížení.</span><span class="sxs-lookup"><span data-stu-id="069c1-157">To load data into a table and generate a surrogate key by using IDENTITY, create the table and then use INSERT..SELECT or INSERT..VALUES to perform the load.</span></span>

<span data-ttu-id="069c1-158">V následujícím příkladu jsou uvedeny základní vzor:</span><span class="sxs-lookup"><span data-stu-id="069c1-158">The following example highlights the basic pattern:</span></span>
 
```sql
--CREATE TABLE with IDENTITY
CREATE TABLE dbo.T1
(   C1 INT IDENTITY(1,1)
,   C2 VARCHAR(30)
)
WITH
(   DISTRIBUTION = HASH(C2)
,   CLUSTERED COLUMNSTORE INDEX
)
;

--Use INSERT..SELECT to populate the table from an external table
INSERT INTO dbo.T1
(C2)
SELECT  C2
FROM    ext.T1
;

SELECT  *
FROM    dbo.T1
;

DBCC PDW_SHOWSPACEUSED('dbo.T1');
```

> [!NOTE] 
> <span data-ttu-id="069c1-159">Není možné použít `CREATE TABLE AS SELECT` aktuálně při načítání dat do tabulky se sloupcem IDENTITY.</span><span class="sxs-lookup"><span data-stu-id="069c1-159">It's not possible to use `CREATE TABLE AS SELECT` currently when loading data into a table with an IDENTITY column.</span></span>
> 

<span data-ttu-id="069c1-160">Další informace o načítání dat pomocí nástroje hromadné kopírování (BCP) programu najdete v následujících článcích:</span><span class="sxs-lookup"><span data-stu-id="069c1-160">For more information on loading data by using the bulk copy program (BCP) tool, see the following articles:</span></span>

- <span data-ttu-id="069c1-161">[Načíst pomocí funkce PolyBase][]</span><span class="sxs-lookup"><span data-stu-id="069c1-161">[Load with PolyBase][]</span></span>
- <span data-ttu-id="069c1-162">[PolyBase osvědčené postupy][]</span><span class="sxs-lookup"><span data-stu-id="069c1-162">[PolyBase best practices][]</span></span>

### <a name="load-data-with-bcp"></a><span data-ttu-id="069c1-163">Načítání dat pomocí BCP</span><span class="sxs-lookup"><span data-stu-id="069c1-163">Load data with BCP</span></span>
<span data-ttu-id="069c1-164">BCP je nástroj příkazového řádku, který slouží k načtení dat do SQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="069c1-164">BCP is a command-line tool that you can use to load data into SQL Data Warehouse.</span></span> <span data-ttu-id="069c1-165">Jeden z jeho parametry (-E) řídí chování BCP při načítání dat do tabulky se sloupcem IDENTITY.</span><span class="sxs-lookup"><span data-stu-id="069c1-165">One of its parameters (-E) controls the behavior of BCP when loading data into a table with an IDENTITY column.</span></span> 

<span data-ttu-id="069c1-166">Pokud je zadána -E, se zachovají hodnoty uchovávat ve vstupním souboru pro sloupec s identitou.</span><span class="sxs-lookup"><span data-stu-id="069c1-166">When -E is specified, the values held in the input file for the column with IDENTITY are retained.</span></span> <span data-ttu-id="069c1-167">Pokud je -E *není* určeno, hodnoty v tomto sloupci se ignorují.</span><span class="sxs-lookup"><span data-stu-id="069c1-167">If -E is *not* specified, then the values in this column are ignored.</span></span> <span data-ttu-id="069c1-168">Pokud sloupec identity neuvedete, je načíst normální data.</span><span class="sxs-lookup"><span data-stu-id="069c1-168">If the identity column is not included, then the data is loaded as normal.</span></span> <span data-ttu-id="069c1-169">Hodnoty jsou generovány podle zásad přírůstek a počáteční hodnoty vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="069c1-169">The values are generated according to the increment and seed policy of the property.</span></span>

<span data-ttu-id="069c1-170">Další informace o načítání dat pomocí BCP najdete v následujících článcích:</span><span class="sxs-lookup"><span data-stu-id="069c1-170">For more information on loading data by using BCP, see the following articles:</span></span>

- <span data-ttu-id="069c1-171">[Načíst pomocí BCP][]</span><span class="sxs-lookup"><span data-stu-id="069c1-171">[Load with BCP][]</span></span>
- <span data-ttu-id="069c1-172">[BCP v MSDN][]</span><span class="sxs-lookup"><span data-stu-id="069c1-172">[BCP in MSDN][]</span></span>

## <a name="catalog-views"></a><span data-ttu-id="069c1-173">Zobrazení katalogu</span><span class="sxs-lookup"><span data-stu-id="069c1-173">Catalog views</span></span>
<span data-ttu-id="069c1-174">Podporuje SQL Data Warehouse `sys.identity_columns` katalogu zobrazení.</span><span class="sxs-lookup"><span data-stu-id="069c1-174">SQL Data Warehouse supports the `sys.identity_columns` catalog view.</span></span> <span data-ttu-id="069c1-175">Toto zobrazení můžete použít k identifikaci sloupec, který má vlastnost IDENTITY.</span><span class="sxs-lookup"><span data-stu-id="069c1-175">This view can be used to identify a column that has the IDENTITY property.</span></span>

<span data-ttu-id="069c1-176">Vám pomůžou lépe pochopit schématu databáze, tento příklad ukazuje, jak integrovat `sys.identity_columns` s dalšími zobrazeními katalogu systému:</span><span class="sxs-lookup"><span data-stu-id="069c1-176">To help you better understand the database schema, this example shows how to integrate `sys.identity_columns` with other system catalog views:</span></span>

```sql
SELECT  sm.name
,       tb.name
,       co.name
,       CASE WHEN ic.column_id IS NOT NULL
             THEN 1
        ELSE 0
        END AS is_identity 
FROM        sys.schemas AS sm
JOIN        sys.tables  AS tb           ON  sm.schema_id = tb.schema_id
JOIN        sys.columns AS co           ON  tb.object_id = co.object_id
LEFT JOIN   sys.identity_columns AS ic  ON  co.object_id = ic.object_id
                                        AND co.column_id = ic.column_id
WHERE   sm.name = 'dbo'
AND     tb.name = 'T1'
;
```

## <a name="limitations"></a><span data-ttu-id="069c1-177">Omezení</span><span class="sxs-lookup"><span data-stu-id="069c1-177">Limitations</span></span>
<span data-ttu-id="069c1-178">Vlastnost IDENTITY nelze použít v následujících scénářích:</span><span class="sxs-lookup"><span data-stu-id="069c1-178">The IDENTITY property can't be used in the following scenarios:</span></span>
- <span data-ttu-id="069c1-179">Kde je datový typ sloupce není INT nebo BIGINT</span><span class="sxs-lookup"><span data-stu-id="069c1-179">Where the column data type is not INT or BIGINT</span></span>
- <span data-ttu-id="069c1-180">Kde sloupec je také distribučního klíče</span><span class="sxs-lookup"><span data-stu-id="069c1-180">Where the column is also the distribution key</span></span>
- <span data-ttu-id="069c1-181">Kde je v tabulce externí tabulky</span><span class="sxs-lookup"><span data-stu-id="069c1-181">Where the table is an external table</span></span> 

<span data-ttu-id="069c1-182">Následující související funkce nejsou podporovány v SQL Data Warehouse:</span><span class="sxs-lookup"><span data-stu-id="069c1-182">The following related functions are not supported in SQL Data Warehouse:</span></span>

- <span data-ttu-id="069c1-183">[IDENTITY()][]</span><span class="sxs-lookup"><span data-stu-id="069c1-183">[IDENTITY()][]</span></span>
- <span data-ttu-id="069c1-184">[@@IDENTITY][]</span><span class="sxs-lookup"><span data-stu-id="069c1-184">[@@IDENTITY][]</span></span>
- <span data-ttu-id="069c1-185">[SCOPE_IDENTITY][]</span><span class="sxs-lookup"><span data-stu-id="069c1-185">[SCOPE_IDENTITY][]</span></span>
- <span data-ttu-id="069c1-186">[FUNKCE IDENT_CURRENT][]</span><span class="sxs-lookup"><span data-stu-id="069c1-186">[IDENT_CURRENT][]</span></span>
- <span data-ttu-id="069c1-187">[IDENT_INCR][]</span><span class="sxs-lookup"><span data-stu-id="069c1-187">[IDENT_INCR][]</span></span>
- <span data-ttu-id="069c1-188">[IDENT_SEED][]</span><span class="sxs-lookup"><span data-stu-id="069c1-188">[IDENT_SEED][]</span></span>
- <span data-ttu-id="069c1-189">[PŘÍKAZ DBCC CHECK_IDENT()][]</span><span class="sxs-lookup"><span data-stu-id="069c1-189">[DBCC CHECK_IDENT()][]</span></span>

## <a name="tasks"></a><span data-ttu-id="069c1-190">Úlohy</span><span class="sxs-lookup"><span data-stu-id="069c1-190">Tasks</span></span>

<span data-ttu-id="069c1-191">Tato část obsahuje některé ukázkový kód, který slouží k provádění běžných úloh při práci s sloupců IDENTITY.</span><span class="sxs-lookup"><span data-stu-id="069c1-191">This section provides some sample code you can use to perform common tasks when you work with IDENTITY columns.</span></span>

> [!NOTE] 
> <span data-ttu-id="069c1-192">Sloupec C1 je identita v následující úlohy.</span><span class="sxs-lookup"><span data-stu-id="069c1-192">Column C1 is the IDENTITY in all the following tasks.</span></span>
> 
 
### <a name="find-the-highest-allocated-value-for-a-table"></a><span data-ttu-id="069c1-193">Vyhledá nejvyšší hodnotu přidělené pro tabulku</span><span class="sxs-lookup"><span data-stu-id="069c1-193">Find the highest allocated value for a table</span></span>
<span data-ttu-id="069c1-194">Použití `MAX()` funkce lze určit nejvyšší hodnotu přidělené pro distribuované tabulku:</span><span class="sxs-lookup"><span data-stu-id="069c1-194">Use the `MAX()` function to determine the highest value allocated for a distributed table:</span></span>

```sql
SELECT  MAX(C1)
FROM    dbo.T1
``` 

### <a name="find-the-seed-and-increment-for-the-identity-property"></a><span data-ttu-id="069c1-195">Vyhledejte počáteční hodnoty a přírůstku pro vlastnost IDENTITY</span><span class="sxs-lookup"><span data-stu-id="069c1-195">Find the seed and increment for the IDENTITY property</span></span>
<span data-ttu-id="069c1-196">Zobrazení katalogu můžete použít ke zjištění hodnoty identity přírůstek a počáteční konfigurace pro tabulku s použitím následujícího dotazu:</span><span class="sxs-lookup"><span data-stu-id="069c1-196">You can use the catalog views to discover the identity increment and seed configuration values for a table by using the following query:</span></span> 

```sql
SELECT  sm.name
,       tb.name
,       co.name
,       ic.seed_value
,       ic.increment_value 
FROM        sys.schemas AS sm
JOIN        sys.tables  AS tb           ON  sm.schema_id = tb.schema_id
JOIN        sys.columns AS co           ON  tb.object_id = co.object_id
JOIN        sys.identity_columns AS ic  ON  co.object_id = ic.object_id
                                        AND co.column_id = ic.column_id
WHERE   sm.name = 'dbo'
AND     tb.name = 'T1'
;
```

## <a name="next-steps"></a><span data-ttu-id="069c1-197">Další kroky</span><span class="sxs-lookup"><span data-stu-id="069c1-197">Next steps</span></span>

* <span data-ttu-id="069c1-198">Další informace o vývoji tabulky najdete v tématu [tabulky přehled][Overview], [tabulky datové typy][Data Types], [distribuovat tabulku] [ Distribute], [Indexu tabulky][Index], [oddílu tabulky][Partition], a [ Dočasné tabulky][Temporary].</span><span class="sxs-lookup"><span data-stu-id="069c1-198">To learn more about developing tables, see [Table overview][Overview], [Table data types][Data Types], [Distribute a table][Distribute], [Index a table][Index], [Partition a table][Partition], and [Temporary tables][Temporary].</span></span> 
* <span data-ttu-id="069c1-199">Další informace o osvědčených postupech najdete v tématu [osvědčené postupy pro SQL Data Warehouse][SQL Data Warehouse Best Practices].</span><span class="sxs-lookup"><span data-stu-id="069c1-199">For more information about best practices, see [SQL Data Warehouse best practices][SQL Data Warehouse Best Practices].</span></span>  

<!--Image references-->

<!--Article references-->
[Overview]: ./sql-data-warehouse-tables-overview.md
[Data Types]: ./sql-data-warehouse-tables-data-types.md
[Distribute]: ./sql-data-warehouse-tables-distribute.md
[Index]: ./sql-data-warehouse-tables-index.md
[Partition]: ./sql-data-warehouse-tables-partition.md
[Statistics]: ./sql-data-warehouse-tables-statistics.md
[Temporary]: ./sql-data-warehouse-tables-temporary.md
[Identity]: ./sql-data-warehouse-tables-identity.md
[SQL Data Warehouse Best Practices]: ./sql-data-warehouse-best-practices.md

<span data-ttu-id="069c1-200">[Načíst pomocí bcp]: https://docs.microsoft.com/azure/sql-data-warehouse/sql-data-warehouse-load-with-bcp/</span><span class="sxs-lookup"><span data-stu-id="069c1-200">[Load with bcp]: https://docs.microsoft.com/azure/sql-data-warehouse/sql-data-warehouse-load-with-bcp/</span></span>
<span data-ttu-id="069c1-201">[Načíst pomocí funkce PolyBase]: https://docs.microsoft.com/azure/sql-data-warehouse/sql-data-warehouse-load-from-azure-blob-storage-with-polybase/</span><span class="sxs-lookup"><span data-stu-id="069c1-201">[Load with PolyBase]: https://docs.microsoft.com/azure/sql-data-warehouse/sql-data-warehouse-load-from-azure-blob-storage-with-polybase/</span></span>
<span data-ttu-id="069c1-202">[PolyBase osvědčené postupy]: https://docs.microsoft.com/azure/sql-data-warehouse/sql-data-warehouse-load-polybase-guide/</span><span class="sxs-lookup"><span data-stu-id="069c1-202">[PolyBase best practices]: https://docs.microsoft.com/azure/sql-data-warehouse/sql-data-warehouse-load-polybase-guide/</span></span>


<!--MSDN references-->
[Identity property]: https://msdn.microsoft.com/library/ms186775.aspx
[sys.identity_columns]: https://msdn.microsoft.com/library/ms187334.aspx
<span data-ttu-id="069c1-203">[IDENTITY()]: https://msdn.microsoft.com/library/ms189838.aspx</span><span class="sxs-lookup"><span data-stu-id="069c1-203">[IDENTITY()]: https://msdn.microsoft.com/library/ms189838.aspx</span></span>
<span data-ttu-id="069c1-204">[@@IDENTITY]: https://msdn.microsoft.com/library/ms187342.aspx</span><span class="sxs-lookup"><span data-stu-id="069c1-204">[@@IDENTITY]: https://msdn.microsoft.com/library/ms187342.aspx</span></span>
<span data-ttu-id="069c1-205">[SCOPE_IDENTITY]: https://msdn.microsoft.com/library/ms190315.aspx</span><span class="sxs-lookup"><span data-stu-id="069c1-205">[SCOPE_IDENTITY]: https://msdn.microsoft.com/library/ms190315.aspx</span></span>
<span data-ttu-id="069c1-206">[FUNKCE IDENT_CURRENT]: https://msdn.microsoft.com/library/ms175098.aspx</span><span class="sxs-lookup"><span data-stu-id="069c1-206">[IDENT_CURRENT]: https://msdn.microsoft.com/library/ms175098.aspx</span></span>
<span data-ttu-id="069c1-207">[IDENT_INCR]: https://msdn.microsoft.com/library/ms189795.aspx</span><span class="sxs-lookup"><span data-stu-id="069c1-207">[IDENT_INCR]: https://msdn.microsoft.com/library/ms189795.aspx</span></span>
<span data-ttu-id="069c1-208">[IDENT_SEED]: https://msdn.microsoft.com/library/ms189834.aspx</span><span class="sxs-lookup"><span data-stu-id="069c1-208">[IDENT_SEED]: https://msdn.microsoft.com/library/ms189834.aspx</span></span>
<span data-ttu-id="069c1-209">[PŘÍKAZ DBCC CHECK_IDENT()]: https://msdn.microsoft.com/library/ms176057.aspx</span><span class="sxs-lookup"><span data-stu-id="069c1-209">[DBCC CHECK_IDENT()]: https://msdn.microsoft.com/library/ms176057.aspx</span></span>

<span data-ttu-id="069c1-210">[BCP v MSDN]: https://msdn.microsoft.com/library/ms162802.aspx</span><span class="sxs-lookup"><span data-stu-id="069c1-210">[bcp in MSDN]: https://msdn.microsoft.com/library/ms162802.aspx</span></span>
  

<!--Other Web references-->  
