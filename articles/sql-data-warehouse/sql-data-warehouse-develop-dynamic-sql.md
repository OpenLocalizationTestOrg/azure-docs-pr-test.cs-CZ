---
title: "Dynamické SQL v SQL Data Warehouse | Microsoft Docs"
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
ms.openlocfilehash: 29228676373aee8dbc7b1b2a7d92ffc978333804
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="dynamic-sql-in-sql-data-warehouse"></a><span data-ttu-id="21225-103">Dynamické SQL v SQL Data Warehouse</span><span class="sxs-lookup"><span data-stu-id="21225-103">Dynamic SQL in SQL Data Warehouse</span></span>
<span data-ttu-id="21225-104">Při vývoji aplikace kód pro SQL Data Warehouse možná budete muset použít dynamické sql které nám pomáhají doručovat flexibilní, obecné a modulární řešení.</span><span class="sxs-lookup"><span data-stu-id="21225-104">When developing application code for SQL Data Warehouse you may need to use dynamic sql to help deliver flexible, generic and modular solutions.</span></span> <span data-ttu-id="21225-105">SQL Data Warehouse nepodporuje datové typy objektů blob v tuto chvíli.</span><span class="sxs-lookup"><span data-stu-id="21225-105">SQL Data Warehouse does not support blob data types at this time.</span></span> <span data-ttu-id="21225-106">To může omezit velikost vašeho řetězce jako objekt blob typy patří typy varchar(max) a nvarchar(max).</span><span class="sxs-lookup"><span data-stu-id="21225-106">This may limit the size of your strings as blob types include both varchar(max) and nvarchar(max) types.</span></span> <span data-ttu-id="21225-107">Pokud jste použili tyto typy v kódu aplikace při sestavování velké řetězce, musíte přerušit kód do bloků, a místo toho použít příkaz EXEC.</span><span class="sxs-lookup"><span data-stu-id="21225-107">If you have used these types in your application code when building very large strings, you will need to break the code into chunks and use the EXEC statement instead.</span></span>

<span data-ttu-id="21225-108">Jednoduchý příklad:</span><span class="sxs-lookup"><span data-stu-id="21225-108">A simple example:</span></span>

```sql
DECLARE @sql_fragment1 VARCHAR(8000)=' SELECT name '
,       @sql_fragment2 VARCHAR(8000)=' FROM sys.system_views '
,       @sql_fragment3 VARCHAR(8000)=' WHERE name like ''%table%''';

EXEC( @sql_fragment1 + @sql_fragment2 + @sql_fragment3);
```

<span data-ttu-id="21225-109">Pokud je řetězec krátké můžete použít [sp_executesql] [ sp_executesql] jako normální.</span><span class="sxs-lookup"><span data-stu-id="21225-109">If the string is short you can use [sp_executesql][sp_executesql] as normal.</span></span>

> [!NOTE]
> <span data-ttu-id="21225-110">Příkazy provést, protože dynamické SQL budou platit stále všech ověřovacích pravidel TSQL.</span><span class="sxs-lookup"><span data-stu-id="21225-110">Statements executed as dynamic SQL will still be subject to all TSQL validation rules.</span></span>
> 
> 

## <a name="next-steps"></a><span data-ttu-id="21225-111">Další kroky</span><span class="sxs-lookup"><span data-stu-id="21225-111">Next steps</span></span>
<span data-ttu-id="21225-112">Další tipy pro vývoj, najdete v části [přehled vývoje][development overview].</span><span class="sxs-lookup"><span data-stu-id="21225-112">For more development tips, see [development overview][development overview].</span></span>

<!--Image references-->

<!--Article references-->
[development overview]: sql-data-warehouse-overview-develop.md

<!--MSDN references-->
[sp_executesql]: https://msdn.microsoft.com/library/ms188001.aspx

<!--Other Web references-->
