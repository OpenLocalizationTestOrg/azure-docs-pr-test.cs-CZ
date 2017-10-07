---
title: "aaaCreate náhradní klíče pomocí IDENTITY | Microsoft Docs"
description: "Zjistěte, jak toouse IDENTITY toocreate náhradní klíče na tabulky."
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
ms.openlocfilehash: 502cdd2b510b229b2a19c1f78b11862a7386ae3b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-surrogate-keys-by-using-identity"></a><span data-ttu-id="9734b-103">Vytvoření klíčů náhradní pomocí IDENTITY</span><span class="sxs-lookup"><span data-stu-id="9734b-103">Create surrogate keys by using IDENTITY</span></span>
> [!div class="op_single_selector"]
> * <span data-ttu-id="9734b-104">[Přehled][Overview]</span><span class="sxs-lookup"><span data-stu-id="9734b-104">[Overview][Overview]</span></span>
> * <span data-ttu-id="9734b-105">[Datové typy][Data Types]</span><span class="sxs-lookup"><span data-stu-id="9734b-105">[Data types][Data Types]</span></span>
> * <span data-ttu-id="9734b-106">[Distribuce][Distribute]</span><span class="sxs-lookup"><span data-stu-id="9734b-106">[Distribute][Distribute]</span></span>
> * <span data-ttu-id="9734b-107">[Index][Index]</span><span class="sxs-lookup"><span data-stu-id="9734b-107">[Index][Index]</span></span>
> * <span data-ttu-id="9734b-108">[Oddíl][Partition]</span><span class="sxs-lookup"><span data-stu-id="9734b-108">[Partition][Partition]</span></span>
> * <span data-ttu-id="9734b-109">[Statistiky][Statistics]</span><span class="sxs-lookup"><span data-stu-id="9734b-109">[Statistics][Statistics]</span></span>
> * <span data-ttu-id="9734b-110">[Dočasné][Temporary]</span><span class="sxs-lookup"><span data-stu-id="9734b-110">[Temporary][Temporary]</span></span>
> * <span data-ttu-id="9734b-111">[Identity][Identity]</span><span class="sxs-lookup"><span data-stu-id="9734b-111">[Identity][Identity]</span></span>
> 
> 

<span data-ttu-id="9734b-112">Mnoho modelování dat jako toocreate náhradní klíče na jejich tabulky při návrhu modely datového skladu.</span><span class="sxs-lookup"><span data-stu-id="9734b-112">Many data modelers like toocreate surrogate keys on their tables when they design data warehouse models.</span></span> <span data-ttu-id="9734b-113">Můžete vytvořit tooachieve vlastnost IDENTITY hello tento cíl jednoduše a efektivně bez ovlivnění výkonu zatížení.</span><span class="sxs-lookup"><span data-stu-id="9734b-113">You can use hello IDENTITY property tooachieve this goal simply and effectively without affecting load performance.</span></span> 

## <a name="get-started-with-identity"></a><span data-ttu-id="9734b-114">Začínáme s identitou</span><span class="sxs-lookup"><span data-stu-id="9734b-114">Get started with IDENTITY</span></span>
<span data-ttu-id="9734b-115">Můžete definovat tabulku tak, že má vlastnost IDENTITY hello při prvním vytvoření tabulky hello pomocí syntaxe, který je podobný toohello následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="9734b-115">You can define a table as having hello IDENTITY property when you first create hello table by using syntax that is similar toohello following statement:</span></span>

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

<span data-ttu-id="9734b-116">Pak můžete použít `INSERT..SELECT` toopopulate hello tabulky.</span><span class="sxs-lookup"><span data-stu-id="9734b-116">You can then use `INSERT..SELECT` toopopulate hello table.</span></span>

## <a name="behavior"></a><span data-ttu-id="9734b-117">Chování</span><span class="sxs-lookup"><span data-stu-id="9734b-117">Behavior</span></span>
<span data-ttu-id="9734b-118">Hello vlastnost IDENTITY je navrženou tooscale se mezi všechny hello distribuce v datovém skladu hello bez ovlivnění výkonu zatížení.</span><span class="sxs-lookup"><span data-stu-id="9734b-118">hello IDENTITY property is designed tooscale out across all hello distributions in hello data warehouse without affecting load performance.</span></span> <span data-ttu-id="9734b-119">Implementace hello identity je proto orientované na dosažení těchto cílů.</span><span class="sxs-lookup"><span data-stu-id="9734b-119">Therefore, hello implementation of IDENTITY is oriented toward achieving these goals.</span></span> <span data-ttu-id="9734b-120">V této části jsou zdůrazněné drobné odlišnosti hello z hello implementace toohelp porozumíte je podrobněji.</span><span class="sxs-lookup"><span data-stu-id="9734b-120">This section highlights hello nuances of hello implementation toohelp you understand them more fully.</span></span>  

### <a name="allocation-of-values"></a><span data-ttu-id="9734b-121">Přidělování hodnot</span><span class="sxs-lookup"><span data-stu-id="9734b-121">Allocation of values</span></span>
<span data-ttu-id="9734b-122">Hello vlastnost IDENTITY nezaručuje hello pořadí v hello, které jsou přiděleny náhradní hodnoty, které odráží hello chování systému SQL Server a databáze SQL Azure.</span><span class="sxs-lookup"><span data-stu-id="9734b-122">hello IDENTITY property doesn't guarantee hello order in which hello surrogate values are allocated, which reflects hello behavior of SQL Server and Azure SQL Database.</span></span> <span data-ttu-id="9734b-123">Ale v Azure SQL Data Warehouse, hello absenci záruku je mnohem výraznější.</span><span class="sxs-lookup"><span data-stu-id="9734b-123">However, in Azure SQL Data Warehouse, hello absence of a guarantee is more pronounced.</span></span> 

<span data-ttu-id="9734b-124">Následující ukázka Hello je obrázek:</span><span class="sxs-lookup"><span data-stu-id="9734b-124">hello following example is an illustration:</span></span>

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

<span data-ttu-id="9734b-125">V předchozím příkladu hello dva řádky dostali distribuční 1.</span><span class="sxs-lookup"><span data-stu-id="9734b-125">In hello preceding example, two rows landed in distribution 1.</span></span> <span data-ttu-id="9734b-126">první řádek Hello má hello náhradní hodnotu 1 ve sloupci `C1`, a druhý řádek hello má hodnotu náhradní hello 61.</span><span class="sxs-lookup"><span data-stu-id="9734b-126">hello first row has hello surrogate value of 1 in column `C1`, and hello second row has hello surrogate value of 61.</span></span> <span data-ttu-id="9734b-127">Obě tyto hodnoty byly vygenerovány hello vlastnost IDENTITY.</span><span class="sxs-lookup"><span data-stu-id="9734b-127">Both of these values were generated by hello IDENTITY property.</span></span> <span data-ttu-id="9734b-128">Přidělení hello hodnot hello však není souvislé.</span><span class="sxs-lookup"><span data-stu-id="9734b-128">However, hello allocation of hello values is not contiguous.</span></span> <span data-ttu-id="9734b-129">Toto chování je záměrné.</span><span class="sxs-lookup"><span data-stu-id="9734b-129">This behavior is by design.</span></span>

### <a name="skewed-data"></a><span data-ttu-id="9734b-130">Zkreslilo dat</span><span class="sxs-lookup"><span data-stu-id="9734b-130">Skewed data</span></span> 
<span data-ttu-id="9734b-131">Hello rozsahu hodnot pro datový typ hello jsou rovnoměrně rozloženy hello distribuce.</span><span class="sxs-lookup"><span data-stu-id="9734b-131">hello range of values for hello data type are spread evenly across hello distributions.</span></span> <span data-ttu-id="9734b-132">Pokud se vyskytne distribuované tabulku z zkreslilo dat, pak hello rozsah hodnot, které jsou k dispozici toohello datatype může dojít k vyčerpání předčasně.</span><span class="sxs-lookup"><span data-stu-id="9734b-132">If a distributed table suffers from skewed data, then hello range of values available toohello datatype can be exhausted prematurely.</span></span> <span data-ttu-id="9734b-133">Například pokud všechna data hello skončilo v jednom distribučním, pak efektivně hello tabulka má přístup tooonly jedné šedesátině hodnot hello hello datového typu.</span><span class="sxs-lookup"><span data-stu-id="9734b-133">For example, if all hello data ends up in a single distribution, then effectively hello table has access tooonly one-sixtieth of hello values of hello data type.</span></span> <span data-ttu-id="9734b-134">Z tohoto důvodu hello vlastnost IDENTITY je omezená příliš`INT` a `BIGINT` datové typy jenom.</span><span class="sxs-lookup"><span data-stu-id="9734b-134">For this reason, hello IDENTITY property is limited too`INT` and `BIGINT` data types only.</span></span>

### <a name="selectinto"></a><span data-ttu-id="9734b-135">VYBERTE... DO</span><span class="sxs-lookup"><span data-stu-id="9734b-135">SELECT..INTO</span></span>
<span data-ttu-id="9734b-136">Pokud do nové tabulky je vybraná jako existující sloupec IDENTITY, nový sloupec hello dědí vlastnost IDENTITY hello, pokud je splněna jedna z hello následující podmínky:</span><span class="sxs-lookup"><span data-stu-id="9734b-136">When an existing IDENTITY column is selected into a new table, hello new column inherits hello IDENTITY property, unless one of hello following conditions is true:</span></span>
- <span data-ttu-id="9734b-137">příkaz SELECT Hello obsahuje spojení.</span><span class="sxs-lookup"><span data-stu-id="9734b-137">hello SELECT statement contains a join.</span></span>
- <span data-ttu-id="9734b-138">Více příkazů SELECT připojeni pomocí UNION.</span><span class="sxs-lookup"><span data-stu-id="9734b-138">Multiple SELECT statements are joined by using UNION.</span></span>
- <span data-ttu-id="9734b-139">sloupec IDENTITY Hello je uvedena v seznamu SELECT hello více než jednou.</span><span class="sxs-lookup"><span data-stu-id="9734b-139">hello IDENTITY column is listed more than one time in hello SELECT list.</span></span>
- <span data-ttu-id="9734b-140">sloupec IDENTITY Hello je součástí výrazu.</span><span class="sxs-lookup"><span data-stu-id="9734b-140">hello IDENTITY column is part of an expression.</span></span>
    
<span data-ttu-id="9734b-141">Pokud platí jedna z těchto podmínek, se vytvoří sloupec hello NOT NULL místo dědí vlastnost IDENTITY hello.</span><span class="sxs-lookup"><span data-stu-id="9734b-141">If any one of these conditions is true, hello column is created NOT NULL instead of inheriting hello IDENTITY property.</span></span>

### <a name="create-table-as-select"></a><span data-ttu-id="9734b-142">VYTVOŘENÍ TABLE AS SELECT</span><span class="sxs-lookup"><span data-stu-id="9734b-142">CREATE TABLE AS SELECT</span></span>
<span data-ttu-id="9734b-143">Vytvoření tabulky AS vyberte funkce CTAS () způsobem hello stejné chování systému SQL Server, která je popsána vyberte... DO.</span><span class="sxs-lookup"><span data-stu-id="9734b-143">CREATE TABLE AS SELECT (CTAS) follows hello same SQL Server behavior that's documented for SELECT..INTO.</span></span> <span data-ttu-id="9734b-144">V definici sloupce hello hello však nelze určit vlastnost IDENTITY `CREATE TABLE` součástí příkaz hello.</span><span class="sxs-lookup"><span data-stu-id="9734b-144">However, you can't specify an IDENTITY property in hello column definition of hello `CREATE TABLE` part of hello statement.</span></span> <span data-ttu-id="9734b-145">Také v nemůžete použít funkci IDENTITY hello hello `SELECT` součástí hello funkce CTAS.</span><span class="sxs-lookup"><span data-stu-id="9734b-145">You also can't use hello IDENTITY function in hello `SELECT` part of hello CTAS.</span></span> <span data-ttu-id="9734b-146">toopopulate tabulky, je nutné toouse `CREATE TABLE` následuje tabulka hello toodefine `INSERT..SELECT` toopopulate ho.</span><span class="sxs-lookup"><span data-stu-id="9734b-146">toopopulate a table, you need toouse `CREATE TABLE` toodefine hello table followed by `INSERT..SELECT` toopopulate it.</span></span>

## <a name="explicitly-insert-values-into-an-identity-column"></a><span data-ttu-id="9734b-147">Explicitně vložení hodnoty do sloupce IDENTITY</span><span class="sxs-lookup"><span data-stu-id="9734b-147">Explicitly insert values into an IDENTITY column</span></span> 
<span data-ttu-id="9734b-148">SQL Data Warehouse podporuje `SET IDENTITY_INSERT <your table> ON|OFF` syntaxe.</span><span class="sxs-lookup"><span data-stu-id="9734b-148">SQL Data Warehouse supports `SET IDENTITY_INSERT <your table> ON|OFF` syntax.</span></span> <span data-ttu-id="9734b-149">Můžete použít tuto syntaxi tooexplicitly vložení hodnoty do sloupce IDENTITY hello.</span><span class="sxs-lookup"><span data-stu-id="9734b-149">You can use this syntax tooexplicitly insert values into hello IDENTITY column.</span></span>

<span data-ttu-id="9734b-150">Mnoho data modelování jako toouse předdefinované záporné hodnoty pro některé řádky v jejich dimenzí.</span><span class="sxs-lookup"><span data-stu-id="9734b-150">Many data modelers like toouse predefined negative values for certain rows in their dimensions.</span></span> <span data-ttu-id="9734b-151">Příkladem je řádek "neznámého člena" nebo hello -1.</span><span class="sxs-lookup"><span data-stu-id="9734b-151">An example is hello -1 or "unknown member" row.</span></span> 

<span data-ttu-id="9734b-152">Hello další skript je ukázkou, jak tooexplicitly přidat tento řádek pomocí nastavení IDENTITY_INSERT:</span><span class="sxs-lookup"><span data-stu-id="9734b-152">hello next script shows how tooexplicitly add this row by using SET IDENTITY_INSERT:</span></span>

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

## <a name="load-data-into-a-table-with-identity"></a><span data-ttu-id="9734b-153">Načtení dat do tabulky s identitou</span><span class="sxs-lookup"><span data-stu-id="9734b-153">Load data into a table with IDENTITY</span></span>

<span data-ttu-id="9734b-154">Hello přítomnost hello vlastnost IDENTITY má nějaký kód načítání dat tooyour dopad.</span><span class="sxs-lookup"><span data-stu-id="9734b-154">hello presence of hello IDENTITY property has some implications tooyour data-loading code.</span></span> <span data-ttu-id="9734b-155">V této části jsou zdůrazněné některé základní vzory pro načítání dat do tabulky pomocí IDENTITY.</span><span class="sxs-lookup"><span data-stu-id="9734b-155">This section highlights some basic patterns for loading data into tables by using IDENTITY.</span></span> 

### <a name="load-data-with-polybase"></a><span data-ttu-id="9734b-156">Načítání dat pomocí PolyBase</span><span class="sxs-lookup"><span data-stu-id="9734b-156">Load data with PolyBase</span></span>
<span data-ttu-id="9734b-157">tooload data do tabulky a vygenerovat klíč náhradní pomocí IDENTITY, vytvořit tabulku hello a pak použít příkaz INSERT... Vyberte nebo VLOŽTE... HODNOTY tooperform hello zatížení.</span><span class="sxs-lookup"><span data-stu-id="9734b-157">tooload data into a table and generate a surrogate key by using IDENTITY, create hello table and then use INSERT..SELECT or INSERT..VALUES tooperform hello load.</span></span>

<span data-ttu-id="9734b-158">Následující příklad označuje hello základní vzor Hello:</span><span class="sxs-lookup"><span data-stu-id="9734b-158">hello following example highlights hello basic pattern:</span></span>
 
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

--Use INSERT..SELECT toopopulate hello table from an external table
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
> <span data-ttu-id="9734b-159">Není možné toouse `CREATE TABLE AS SELECT` aktuálně při načítání dat do tabulky se sloupcem IDENTITY.</span><span class="sxs-lookup"><span data-stu-id="9734b-159">It's not possible toouse `CREATE TABLE AS SELECT` currently when loading data into a table with an IDENTITY column.</span></span>
> 

<span data-ttu-id="9734b-160">Další informace k načítání dat pomocí hello hromadné kopírování (BCP) programu nástroje najdete v tématu hello následující články:</span><span class="sxs-lookup"><span data-stu-id="9734b-160">For more information on loading data by using hello bulk copy program (BCP) tool, see hello following articles:</span></span>

- <span data-ttu-id="9734b-161">[Načíst pomocí funkce PolyBase][]</span><span class="sxs-lookup"><span data-stu-id="9734b-161">[Load with PolyBase][]</span></span>
- <span data-ttu-id="9734b-162">[PolyBase osvědčené postupy][]</span><span class="sxs-lookup"><span data-stu-id="9734b-162">[PolyBase best practices][]</span></span>

### <a name="load-data-with-bcp"></a><span data-ttu-id="9734b-163">Načítání dat pomocí BCP</span><span class="sxs-lookup"><span data-stu-id="9734b-163">Load data with BCP</span></span>
<span data-ttu-id="9734b-164">BCP je nástroj příkazového řádku, které můžete tooload dat do SQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="9734b-164">BCP is a command-line tool that you can use tooload data into SQL Data Warehouse.</span></span> <span data-ttu-id="9734b-165">Jeden z jeho parametry (-E) ovládacích prvků hello chování BCP při načítání dat do tabulky se sloupcem IDENTITY.</span><span class="sxs-lookup"><span data-stu-id="9734b-165">One of its parameters (-E) controls hello behavior of BCP when loading data into a table with an IDENTITY column.</span></span> 

<span data-ttu-id="9734b-166">Pokud je zadána -E, se zachovají hodnoty hello uchovávat v hello vstupní soubor pro hello sloupec s identitou.</span><span class="sxs-lookup"><span data-stu-id="9734b-166">When -E is specified, hello values held in hello input file for hello column with IDENTITY are retained.</span></span> <span data-ttu-id="9734b-167">Pokud je -E *není* určeno, hello hodnoty v tomto sloupci se ignorují.</span><span class="sxs-lookup"><span data-stu-id="9734b-167">If -E is *not* specified, then hello values in this column are ignored.</span></span> <span data-ttu-id="9734b-168">Sloupec identity hello neuvedete, hello data je načíst jako normální.</span><span class="sxs-lookup"><span data-stu-id="9734b-168">If hello identity column is not included, then hello data is loaded as normal.</span></span> <span data-ttu-id="9734b-169">Hello hodnoty jsou generovány podle zásad toohello přírůstek a počáteční hodnoty vlastnosti hello.</span><span class="sxs-lookup"><span data-stu-id="9734b-169">hello values are generated according toohello increment and seed policy of hello property.</span></span>

<span data-ttu-id="9734b-170">Další informace o načítání dat pomocí BCP najdete v části hello následující články:</span><span class="sxs-lookup"><span data-stu-id="9734b-170">For more information on loading data by using BCP, see hello following articles:</span></span>

- <span data-ttu-id="9734b-171">[Načíst pomocí BCP][]</span><span class="sxs-lookup"><span data-stu-id="9734b-171">[Load with BCP][]</span></span>
- <span data-ttu-id="9734b-172">[BCP v MSDN][]</span><span class="sxs-lookup"><span data-stu-id="9734b-172">[BCP in MSDN][]</span></span>

## <a name="catalog-views"></a><span data-ttu-id="9734b-173">Zobrazení katalogu</span><span class="sxs-lookup"><span data-stu-id="9734b-173">Catalog views</span></span>
<span data-ttu-id="9734b-174">SQL Data Warehouse podporuje hello `sys.identity_columns` katalogu zobrazení.</span><span class="sxs-lookup"><span data-stu-id="9734b-174">SQL Data Warehouse supports hello `sys.identity_columns` catalog view.</span></span> <span data-ttu-id="9734b-175">Toto zobrazení lze použít tooidentify sloupec, který má vlastnost IDENTITY hello.</span><span class="sxs-lookup"><span data-stu-id="9734b-175">This view can be used tooidentify a column that has hello IDENTITY property.</span></span>

<span data-ttu-id="9734b-176">toohelp lépe pochopit hello schématu databáze, tento příklad ukazuje, jak toointegrate `sys.identity_columns` s dalšími zobrazeními katalogu systému:</span><span class="sxs-lookup"><span data-stu-id="9734b-176">toohelp you better understand hello database schema, this example shows how toointegrate `sys.identity_columns` with other system catalog views:</span></span>

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

## <a name="limitations"></a><span data-ttu-id="9734b-177">Omezení</span><span class="sxs-lookup"><span data-stu-id="9734b-177">Limitations</span></span>
<span data-ttu-id="9734b-178">Hello vlastnost IDENTITY nelze použít v hello následující scénáře:</span><span class="sxs-lookup"><span data-stu-id="9734b-178">hello IDENTITY property can't be used in hello following scenarios:</span></span>
- <span data-ttu-id="9734b-179">Kde je datový typ sloupce hello není INT nebo BIGINT</span><span class="sxs-lookup"><span data-stu-id="9734b-179">Where hello column data type is not INT or BIGINT</span></span>
- <span data-ttu-id="9734b-180">Kde hello sloupec je také hello distribučního klíče</span><span class="sxs-lookup"><span data-stu-id="9734b-180">Where hello column is also hello distribution key</span></span>
- <span data-ttu-id="9734b-181">Kde hello tabulka je externí tabulky</span><span class="sxs-lookup"><span data-stu-id="9734b-181">Where hello table is an external table</span></span> 

<span data-ttu-id="9734b-182">Hello následující související funkce nejsou podporovány v SQL Data Warehouse:</span><span class="sxs-lookup"><span data-stu-id="9734b-182">hello following related functions are not supported in SQL Data Warehouse:</span></span>

- <span data-ttu-id="9734b-183">[IDENTITY()][]</span><span class="sxs-lookup"><span data-stu-id="9734b-183">[IDENTITY()][]</span></span>
- <span data-ttu-id="9734b-184">[@@IDENTITY][]</span><span class="sxs-lookup"><span data-stu-id="9734b-184">[@@IDENTITY][]</span></span>
- <span data-ttu-id="9734b-185">[SCOPE_IDENTITY][]</span><span class="sxs-lookup"><span data-stu-id="9734b-185">[SCOPE_IDENTITY][]</span></span>
- <span data-ttu-id="9734b-186">[FUNKCE IDENT_CURRENT][]</span><span class="sxs-lookup"><span data-stu-id="9734b-186">[IDENT_CURRENT][]</span></span>
- <span data-ttu-id="9734b-187">[IDENT_INCR][]</span><span class="sxs-lookup"><span data-stu-id="9734b-187">[IDENT_INCR][]</span></span>
- <span data-ttu-id="9734b-188">[IDENT_SEED][]</span><span class="sxs-lookup"><span data-stu-id="9734b-188">[IDENT_SEED][]</span></span>
- <span data-ttu-id="9734b-189">[PŘÍKAZ DBCC CHECK_IDENT()][]</span><span class="sxs-lookup"><span data-stu-id="9734b-189">[DBCC CHECK_IDENT()][]</span></span>

## <a name="tasks"></a><span data-ttu-id="9734b-190">Úlohy</span><span class="sxs-lookup"><span data-stu-id="9734b-190">Tasks</span></span>

<span data-ttu-id="9734b-191">Tato část obsahuje ukázkový kód tooperform běžných úloh můžete použít při práci s sloupců IDENTITY.</span><span class="sxs-lookup"><span data-stu-id="9734b-191">This section provides some sample code you can use tooperform common tasks when you work with IDENTITY columns.</span></span>

> [!NOTE] 
> <span data-ttu-id="9734b-192">Sloupec C1 je hello IDENTITY ve všech hello následující úlohy.</span><span class="sxs-lookup"><span data-stu-id="9734b-192">Column C1 is hello IDENTITY in all hello following tasks.</span></span>
> 
 
### <a name="find-hello-highest-allocated-value-for-a-table"></a><span data-ttu-id="9734b-193">Vyhledá nejvyšší hello přidělené hodnotu pro tabulku</span><span class="sxs-lookup"><span data-stu-id="9734b-193">Find hello highest allocated value for a table</span></span>
<span data-ttu-id="9734b-194">Použití hello `MAX()` funkce toodetermine hello nejvyšší hodnotou přidělené pro distribuované tabulku:</span><span class="sxs-lookup"><span data-stu-id="9734b-194">Use hello `MAX()` function toodetermine hello highest value allocated for a distributed table:</span></span>

```sql
SELECT  MAX(C1)
FROM    dbo.T1
``` 

### <a name="find-hello-seed-and-increment-for-hello-identity-property"></a><span data-ttu-id="9734b-195">Najít hello zdrojovou a přírůstkovou pro hello vlastnost IDENTITY</span><span class="sxs-lookup"><span data-stu-id="9734b-195">Find hello seed and increment for hello IDENTITY property</span></span>
<span data-ttu-id="9734b-196">Hello katalogu zobrazení toodiscover hello identity přírůstek a počáteční hodnoty konfigurace můžete použít pro tabulku s použitím hello následující dotaz:</span><span class="sxs-lookup"><span data-stu-id="9734b-196">You can use hello catalog views toodiscover hello identity increment and seed configuration values for a table by using hello following query:</span></span> 

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

## <a name="next-steps"></a><span data-ttu-id="9734b-197">Další kroky</span><span class="sxs-lookup"><span data-stu-id="9734b-197">Next steps</span></span>

* <span data-ttu-id="9734b-198">toolearn Další informace o vývoji tabulky, najdete v části [tabulky přehled][Overview], [tabulky datové typy][Data Types], [distribuovat tabulku] [ Distribute], [Indexu tabulky][Index], [oddílu tabulky][Partition], a [ Dočasné tabulky][Temporary].</span><span class="sxs-lookup"><span data-stu-id="9734b-198">toolearn more about developing tables, see [Table overview][Overview], [Table data types][Data Types], [Distribute a table][Distribute], [Index a table][Index], [Partition a table][Partition], and [Temporary tables][Temporary].</span></span> 
* <span data-ttu-id="9734b-199">Další informace o osvědčených postupech najdete v tématu [osvědčené postupy pro SQL Data Warehouse][SQL Data Warehouse Best Practices].</span><span class="sxs-lookup"><span data-stu-id="9734b-199">For more information about best practices, see [SQL Data Warehouse best practices][SQL Data Warehouse Best Practices].</span></span>  

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

[Načíst pomocí bcp]: https://docs.microsoft.com/azure/sql-data-warehouse/sql-data-warehouse-load-with-bcp/
[Načíst pomocí funkce PolyBase]: https://docs.microsoft.com/azure/sql-data-warehouse/sql-data-warehouse-load-from-azure-blob-storage-with-polybase/
[PolyBase osvědčené postupy]: https://docs.microsoft.com/azure/sql-data-warehouse/sql-data-warehouse-load-polybase-guide/


<!--MSDN references-->
[Identity property]: https://msdn.microsoft.com/library/ms186775.aspx
[sys.identity_columns]: https://msdn.microsoft.com/library/ms187334.aspx
[IDENTITY()]: https://msdn.microsoft.com/library/ms189838.aspx
[@@IDENTITY]: https://msdn.microsoft.com/library/ms187342.aspx
[SCOPE_IDENTITY]: https://msdn.microsoft.com/library/ms190315.aspx
[FUNKCE IDENT_CURRENT]: https://msdn.microsoft.com/library/ms175098.aspx
[IDENT_INCR]: https://msdn.microsoft.com/library/ms189795.aspx
[IDENT_SEED]: https://msdn.microsoft.com/library/ms189834.aspx
[PŘÍKAZ DBCC CHECK_IDENT()]: https://msdn.microsoft.com/library/ms176057.aspx

[BCP v MSDN]: https://msdn.microsoft.com/library/ms162802.aspx
  

<!--Other Web references-->  
