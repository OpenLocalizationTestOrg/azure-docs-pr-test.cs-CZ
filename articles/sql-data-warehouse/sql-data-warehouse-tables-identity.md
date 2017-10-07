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
# <a name="create-surrogate-keys-by-using-identity"></a>Vytvoření klíčů náhradní pomocí IDENTITY
> [!div class="op_single_selector"]
> * [Přehled][Overview]
> * [Datové typy][Data Types]
> * [Distribuce][Distribute]
> * [Index][Index]
> * [Oddíl][Partition]
> * [Statistiky][Statistics]
> * [Dočasné][Temporary]
> * [Identity][Identity]
> 
> 

Mnoho modelování dat jako toocreate náhradní klíče na jejich tabulky při návrhu modely datového skladu. Můžete vytvořit tooachieve vlastnost IDENTITY hello tento cíl jednoduše a efektivně bez ovlivnění výkonu zatížení. 

## <a name="get-started-with-identity"></a>Začínáme s identitou
Můžete definovat tabulku tak, že má vlastnost IDENTITY hello při prvním vytvoření tabulky hello pomocí syntaxe, který je podobný toohello následující příkaz:

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

Pak můžete použít `INSERT..SELECT` toopopulate hello tabulky.

## <a name="behavior"></a>Chování
Hello vlastnost IDENTITY je navrženou tooscale se mezi všechny hello distribuce v datovém skladu hello bez ovlivnění výkonu zatížení. Implementace hello identity je proto orientované na dosažení těchto cílů. V této části jsou zdůrazněné drobné odlišnosti hello z hello implementace toohelp porozumíte je podrobněji.  

### <a name="allocation-of-values"></a>Přidělování hodnot
Hello vlastnost IDENTITY nezaručuje hello pořadí v hello, které jsou přiděleny náhradní hodnoty, které odráží hello chování systému SQL Server a databáze SQL Azure. Ale v Azure SQL Data Warehouse, hello absenci záruku je mnohem výraznější. 

Následující ukázka Hello je obrázek:

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

V předchozím příkladu hello dva řádky dostali distribuční 1. první řádek Hello má hello náhradní hodnotu 1 ve sloupci `C1`, a druhý řádek hello má hodnotu náhradní hello 61. Obě tyto hodnoty byly vygenerovány hello vlastnost IDENTITY. Přidělení hello hodnot hello však není souvislé. Toto chování je záměrné.

### <a name="skewed-data"></a>Zkreslilo dat 
Hello rozsahu hodnot pro datový typ hello jsou rovnoměrně rozloženy hello distribuce. Pokud se vyskytne distribuované tabulku z zkreslilo dat, pak hello rozsah hodnot, které jsou k dispozici toohello datatype může dojít k vyčerpání předčasně. Například pokud všechna data hello skončilo v jednom distribučním, pak efektivně hello tabulka má přístup tooonly jedné šedesátině hodnot hello hello datového typu. Z tohoto důvodu hello vlastnost IDENTITY je omezená příliš`INT` a `BIGINT` datové typy jenom.

### <a name="selectinto"></a>VYBERTE... DO
Pokud do nové tabulky je vybraná jako existující sloupec IDENTITY, nový sloupec hello dědí vlastnost IDENTITY hello, pokud je splněna jedna z hello následující podmínky:
- příkaz SELECT Hello obsahuje spojení.
- Více příkazů SELECT připojeni pomocí UNION.
- sloupec IDENTITY Hello je uvedena v seznamu SELECT hello více než jednou.
- sloupec IDENTITY Hello je součástí výrazu.
    
Pokud platí jedna z těchto podmínek, se vytvoří sloupec hello NOT NULL místo dědí vlastnost IDENTITY hello.

### <a name="create-table-as-select"></a>VYTVOŘENÍ TABLE AS SELECT
Vytvoření tabulky AS vyberte funkce CTAS () způsobem hello stejné chování systému SQL Server, která je popsána vyberte... DO. V definici sloupce hello hello však nelze určit vlastnost IDENTITY `CREATE TABLE` součástí příkaz hello. Také v nemůžete použít funkci IDENTITY hello hello `SELECT` součástí hello funkce CTAS. toopopulate tabulky, je nutné toouse `CREATE TABLE` následuje tabulka hello toodefine `INSERT..SELECT` toopopulate ho.

## <a name="explicitly-insert-values-into-an-identity-column"></a>Explicitně vložení hodnoty do sloupce IDENTITY 
SQL Data Warehouse podporuje `SET IDENTITY_INSERT <your table> ON|OFF` syntaxe. Můžete použít tuto syntaxi tooexplicitly vložení hodnoty do sloupce IDENTITY hello.

Mnoho data modelování jako toouse předdefinované záporné hodnoty pro některé řádky v jejich dimenzí. Příkladem je řádek "neznámého člena" nebo hello -1. 

Hello další skript je ukázkou, jak tooexplicitly přidat tento řádek pomocí nastavení IDENTITY_INSERT:

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

## <a name="load-data-into-a-table-with-identity"></a>Načtení dat do tabulky s identitou

Hello přítomnost hello vlastnost IDENTITY má nějaký kód načítání dat tooyour dopad. V této části jsou zdůrazněné některé základní vzory pro načítání dat do tabulky pomocí IDENTITY. 

### <a name="load-data-with-polybase"></a>Načítání dat pomocí PolyBase
tooload data do tabulky a vygenerovat klíč náhradní pomocí IDENTITY, vytvořit tabulku hello a pak použít příkaz INSERT... Vyberte nebo VLOŽTE... HODNOTY tooperform hello zatížení.

Následující příklad označuje hello základní vzor Hello:
 
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
> Není možné toouse `CREATE TABLE AS SELECT` aktuálně při načítání dat do tabulky se sloupcem IDENTITY.
> 

Další informace k načítání dat pomocí hello hromadné kopírování (BCP) programu nástroje najdete v tématu hello následující články:

- [Načíst pomocí funkce PolyBase][]
- [PolyBase osvědčené postupy][]

### <a name="load-data-with-bcp"></a>Načítání dat pomocí BCP
BCP je nástroj příkazového řádku, které můžete tooload dat do SQL Data Warehouse. Jeden z jeho parametry (-E) ovládacích prvků hello chování BCP při načítání dat do tabulky se sloupcem IDENTITY. 

Pokud je zadána -E, se zachovají hodnoty hello uchovávat v hello vstupní soubor pro hello sloupec s identitou. Pokud je -E *není* určeno, hello hodnoty v tomto sloupci se ignorují. Sloupec identity hello neuvedete, hello data je načíst jako normální. Hello hodnoty jsou generovány podle zásad toohello přírůstek a počáteční hodnoty vlastnosti hello.

Další informace o načítání dat pomocí BCP najdete v části hello následující články:

- [Načíst pomocí BCP][]
- [BCP v MSDN][]

## <a name="catalog-views"></a>Zobrazení katalogu
SQL Data Warehouse podporuje hello `sys.identity_columns` katalogu zobrazení. Toto zobrazení lze použít tooidentify sloupec, který má vlastnost IDENTITY hello.

toohelp lépe pochopit hello schématu databáze, tento příklad ukazuje, jak toointegrate `sys.identity_columns` s dalšími zobrazeními katalogu systému:

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

## <a name="limitations"></a>Omezení
Hello vlastnost IDENTITY nelze použít v hello následující scénáře:
- Kde je datový typ sloupce hello není INT nebo BIGINT
- Kde hello sloupec je také hello distribučního klíče
- Kde hello tabulka je externí tabulky 

Hello následující související funkce nejsou podporovány v SQL Data Warehouse:

- [IDENTITY()][]
- [@@IDENTITY][]
- [SCOPE_IDENTITY][]
- [FUNKCE IDENT_CURRENT][]
- [IDENT_INCR][]
- [IDENT_SEED][]
- [PŘÍKAZ DBCC CHECK_IDENT()][]

## <a name="tasks"></a>Úlohy

Tato část obsahuje ukázkový kód tooperform běžných úloh můžete použít při práci s sloupců IDENTITY.

> [!NOTE] 
> Sloupec C1 je hello IDENTITY ve všech hello následující úlohy.
> 
 
### <a name="find-hello-highest-allocated-value-for-a-table"></a>Vyhledá nejvyšší hello přidělené hodnotu pro tabulku
Použití hello `MAX()` funkce toodetermine hello nejvyšší hodnotou přidělené pro distribuované tabulku:

```sql
SELECT  MAX(C1)
FROM    dbo.T1
``` 

### <a name="find-hello-seed-and-increment-for-hello-identity-property"></a>Najít hello zdrojovou a přírůstkovou pro hello vlastnost IDENTITY
Hello katalogu zobrazení toodiscover hello identity přírůstek a počáteční hodnoty konfigurace můžete použít pro tabulku s použitím hello následující dotaz: 

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

## <a name="next-steps"></a>Další kroky

* toolearn Další informace o vývoji tabulky, najdete v části [tabulky přehled][Overview], [tabulky datové typy][Data Types], [distribuovat tabulku] [ Distribute], [Indexu tabulky][Index], [oddílu tabulky][Partition], a [ Dočasné tabulky][Temporary]. 
* Další informace o osvědčených postupech najdete v tématu [osvědčené postupy pro SQL Data Warehouse][SQL Data Warehouse Best Practices].  

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
