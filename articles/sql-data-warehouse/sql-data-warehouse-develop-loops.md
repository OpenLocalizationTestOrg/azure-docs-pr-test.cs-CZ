---
title: aaaLeverage cyklu T-SQL v Azure SQL Data Warehouse | Microsoft Docs
description: "Tipy pro smyčky Transact-SQL a nahraďte kurzory v Azure SQL Data Warehouse na vývoj řešení."
services: sql-data-warehouse
documentationcenter: NA
author: jrowlandjones
manager: jhubbard
editor: 
ms.assetid: f3384b81-b943-431b-bc73-90e47e4c195f
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: t-sql
ms.date: 10/31/2016
ms.author: jrj;barbkess
ms.openlocfilehash: c7e8f71b910d00d0dfc30f6e5eba190fd05014b3
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="loops-in-sql-data-warehouse"></a>Smyčky v SQL Data Warehouse
SQL Data Warehouse podporuje hello [při][při] smyčky pro opakovaně provádění blok příkazu. To bude pokračovat pro tak dlouho, dokud hello zadán, jsou splněny podmínky, nebo dokud se kód hello konkrétně ukončí hello smyčky pomocí hello `BREAK` – klíčové slovo. Smyčky jsou obzvláště užitečná pro nahrazení kurzory definované v kódu SQL. Naštěstí téměř všechny kurzory, které jsou zapsány v kódu SQL jsou z hello rychlé předat dál, přečtěte si pouze různých. Proto [při] smyčky jsou skvělý alternativní, pokud se přistihnete s tooreplace jeden.

## <a name="leveraging-loops-and-replacing-cursors-in-sql-data-warehouse"></a>Využití smyčky a nahrazení kurzory v SQL Data Warehouse
Nicméně, než začnete v head nejdřív měli byste požádat sami hello následující otázku: "může být tento kurzor znovu napsané toouse sady na základě operations?". V mnoha případech hello odpovědí bude Ano a je často nejlepším postupem hello. Operace set na základě často provádí výrazně rychlejší než přístup iterativní, řádek po řádku.

Rychloposuv vpřed kurzory jen pro čtení můžete snadno nahradit opakování konstrukce. Níže je jednoduchý příklad. Tento příklad kódu aktualizuje hello statistiky pro každou tabulku v databázi hello. Podle iterování přes hello tabulky ve smyčce hello jsme jsou možné tooexecute každý příkaz v pořadí.

Nejprve vytvořte dočasnou tabulku obsahující jedinečný řádek číslo používané tooidentify hello jednotlivé příkazy:

```
CREATE TABLE #tbl
WITH
( DISTRIBUTION = ROUND_ROBIN
)
AS
SELECT  ROW_NUMBER() OVER(ORDER BY (SELECT NULL)) AS Sequence
,       [name]
,       'UPDATE STATISTICS '+QUOTENAME([name]) AS sql_code
FROM    sys.tables
;
```

Druhý inicializace hello proměnné požadované tooperform hello smyčka:

```
DECLARE @nbr_statements INT = (SELECT COUNT(*) FROM #tbl)
,       @i INT = 1
;
```

Nyní smyčku příkazy provádění jeden současně:

```
WHILE   @i <= @nbr_statements
BEGIN
    DECLARE @sql_code NVARCHAR(4000) = (SELECT sql_code FROM #tbl WHERE Sequence = @i);
    EXEC    sp_executesql @sql_code;
    SET     @i +=1;
END
```

Nakonec vyřaďte hello dočasné tabulky vytvořili v prvním kroku hello

```
DROP TABLE #tbl;
```


<!--Every topic should have next steps and links toohello next logical set of content tookeep hello customer engaged-->

## <a name="next-steps"></a>Další kroky
Další tipy pro vývoj, najdete v části [přehled vývoje][development overview].

<!--Image references-->

<!--Article references-->
[development overview]: sql-data-warehouse-overview-develop.md

<!--MSDN references-->
[při]: https://msdn.microsoft.com/library/ms178642.aspx


<!--Other Web references-->
