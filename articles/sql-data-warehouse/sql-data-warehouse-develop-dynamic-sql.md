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
# <a name="dynamic-sql-in-sql-data-warehouse"></a><span data-ttu-id="4c83c-103">Dynamické SQL v SQL Data Warehouse</span><span class="sxs-lookup"><span data-stu-id="4c83c-103">Dynamic SQL in SQL Data Warehouse</span></span>
<span data-ttu-id="4c83c-104">Při vývoji aplikace kód pro SQL Data Warehouse mohou potřebovat toouse dynamické sql toohelp poskytovat flexibilní, obecné a modulární řešení.</span><span class="sxs-lookup"><span data-stu-id="4c83c-104">When developing application code for SQL Data Warehouse you may need toouse dynamic sql toohelp deliver flexible, generic and modular solutions.</span></span> <span data-ttu-id="4c83c-105">SQL Data Warehouse nepodporuje datové typy objektů blob v tuto chvíli.</span><span class="sxs-lookup"><span data-stu-id="4c83c-105">SQL Data Warehouse does not support blob data types at this time.</span></span> <span data-ttu-id="4c83c-106">To může omezit hello velikost vašeho řetězce jako objekt blob typy patří typy varchar(max) a nvarchar(max).</span><span class="sxs-lookup"><span data-stu-id="4c83c-106">This may limit hello size of your strings as blob types include both varchar(max) and nvarchar(max) types.</span></span> <span data-ttu-id="4c83c-107">Pokud jste použili tyto typy v kódu aplikace při sestavování velké řetězce, budete potřebovat toobreak hello kódu do bloků a použijte příkaz EXEC hello místo.</span><span class="sxs-lookup"><span data-stu-id="4c83c-107">If you have used these types in your application code when building very large strings, you will need toobreak hello code into chunks and use hello EXEC statement instead.</span></span>

<span data-ttu-id="4c83c-108">Jednoduchý příklad:</span><span class="sxs-lookup"><span data-stu-id="4c83c-108">A simple example:</span></span>

```sql
DECLARE @sql_fragment1 VARCHAR(8000)=' SELECT name '
,       @sql_fragment2 VARCHAR(8000)=' FROM sys.system_views '
,       @sql_fragment3 VARCHAR(8000)=' WHERE name like ''%table%''';

EXEC( @sql_fragment1 + @sql_fragment2 + @sql_fragment3);
```

<span data-ttu-id="4c83c-109">Pokud je řetězec hello krátké můžete použít [sp_executesql] [ sp_executesql] jako normální.</span><span class="sxs-lookup"><span data-stu-id="4c83c-109">If hello string is short you can use [sp_executesql][sp_executesql] as normal.</span></span>

> [!NOTE]
> <span data-ttu-id="4c83c-110">Příkazy provést, protože dynamické SQL bude stále subjektu tooall TSQL ověřovacích pravidel.</span><span class="sxs-lookup"><span data-stu-id="4c83c-110">Statements executed as dynamic SQL will still be subject tooall TSQL validation rules.</span></span>
> 
> 

## <a name="next-steps"></a><span data-ttu-id="4c83c-111">Další kroky</span><span class="sxs-lookup"><span data-stu-id="4c83c-111">Next steps</span></span>
<span data-ttu-id="4c83c-112">Další tipy pro vývoj, najdete v části [přehled vývoje][development overview].</span><span class="sxs-lookup"><span data-stu-id="4c83c-112">For more development tips, see [development overview][development overview].</span></span>

<!--Image references-->

<!--Article references-->
[development overview]: sql-data-warehouse-overview-develop.md

<!--MSDN references-->
[sp_executesql]: https://msdn.microsoft.com/library/ms188001.aspx

<!--Other Web references-->
