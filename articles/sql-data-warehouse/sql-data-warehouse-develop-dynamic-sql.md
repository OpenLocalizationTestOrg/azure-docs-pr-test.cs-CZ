---
title: aaaDynamic SQL v SQL Data Warehouse | Microsoft Docs
description: "Tipy pro používání dynamických SQL v Azure SQL Data Warehouse pro vývoj řešení."
services: sql-data-warehouse
documentationcenter: NA
author: jrowlandjones
manager: jhubbard
editor: 
ms.assetid: a948c2c3-3cd1-4373-90a9-79e59414b778
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: queries
ms.date: 10/31/2016
ms.author: jrj;barbkess
ms.openlocfilehash: 4d66eecb37621510f657d1ec9a2a935daaa16052
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="dynamic-sql-in-sql-data-warehouse"></a>Dynamické SQL v SQL Data Warehouse
Při vývoji aplikace kód pro SQL Data Warehouse mohou potřebovat toouse dynamické sql toohelp poskytovat flexibilní, obecné a modulární řešení. SQL Data Warehouse nepodporuje datové typy objektů blob v tuto chvíli. To může omezit hello velikost vašeho řetězce jako objekt blob typy patří typy varchar(max) a nvarchar(max). Pokud jste použili tyto typy v kódu aplikace při sestavování velké řetězce, budete potřebovat toobreak hello kódu do bloků a použijte příkaz EXEC hello místo.

Jednoduchý příklad:

```sql
DECLARE @sql_fragment1 VARCHAR(8000)=' SELECT name '
,       @sql_fragment2 VARCHAR(8000)=' FROM sys.system_views '
,       @sql_fragment3 VARCHAR(8000)=' WHERE name like ''%table%''';

EXEC( @sql_fragment1 + @sql_fragment2 + @sql_fragment3);
```

Pokud je řetězec hello krátké můžete použít [sp_executesql] [ sp_executesql] jako normální.

> [!NOTE]
> Příkazy provést, protože dynamické SQL bude stále subjektu tooall TSQL ověřovacích pravidel.
> 
> 

## <a name="next-steps"></a>Další kroky
Další tipy pro vývoj, najdete v části [přehled vývoje][development overview].

<!--Image references-->

<!--Article references-->
[development overview]: sql-data-warehouse-overview-develop.md

<!--MSDN references-->
[sp_executesql]: https://msdn.microsoft.com/library/ms188001.aspx

<!--Other Web references-->
