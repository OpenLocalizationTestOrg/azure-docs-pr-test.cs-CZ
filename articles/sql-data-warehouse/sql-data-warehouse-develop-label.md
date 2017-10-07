---
title: "aaaUse popisků tooinstrument dotazů v SQL Data Warehouse | Microsoft Docs"
description: "Tipy pro používání dotazy tooinstrument popisky v Azure SQL Data Warehouse pro vývoj řešení."
services: sql-data-warehouse
documentationcenter: NA
author: jrowlandjones
manager: jhubbard
editor: 
ms.assetid: 44988de8-04c1-4fed-92be-e1935661a4e8
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: queries
ms.date: 10/31/2016
ms.author: jrj;barbkess
ms.openlocfilehash: 82e7ea98e1417134227f1d7c529fdaf2f1df3853
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="use-labels-tooinstrument-queries-in-sql-data-warehouse"></a>Pomocí popisků tooinstrument dotazů v SQL Data Warehouse
SQL Data Warehouse podporuje koncept názvem popisky dotazu. Před přechodem do jakékoli hloubka umožňuje podívejte se na příklad jednoho:

```sql
SELECT *
FROM sys.tables
OPTION (LABEL = 'My Query Label')
;
```

Tento poslední řádek značky hello řetězec popisek Moje dotazu toohello dotazu. To je zvláště užitečné, protože popisek hello je dotaz může prostřednictvím hello zobrazení dynamické správy. To nám poskytuje mechanismus tootrack dolů problém dotazy a také toohelp identifikovat průběh prostřednictvím spustit ETL.

Dobrý zásady vytváření názvů pomáhá skutečně sem. Například něco jako ' projektu: postup: příkaz: komentář se pomohou toouniquely identifikovat hello dotaz mezi všechny hello kódu ve správě zdrojového kódu.

toosearch popiskem, můžete použít následující dotaz, který používá hello hello zobrazení dynamické správy:

```sql
SELECT  *
FROM    sys.dm_pdw_exec_requests r
WHERE   r.[label] = 'My Query Label'
;
```

> [!NOTE]
> Je nezbytné při dotazování zabalení hranaté závorky a dvojité uvozovky kolem hello word popisek. Popisek je vyhrazené slovo a způsobilo chybu, pokud nebyla oddělené.
> 
> 

## <a name="next-steps"></a>Další kroky
Další tipy pro vývoj, najdete v části [přehled vývoje][development overview].

<!--Image references-->

<!--Article references-->
[development overview]: sql-data-warehouse-overview-develop.md

<!--MSDN references-->

<!--Other Web references-->
