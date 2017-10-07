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
# <a name="use-labels-tooinstrument-queries-in-sql-data-warehouse"></a><span data-ttu-id="8f24f-103">Pomocí popisků tooinstrument dotazů v SQL Data Warehouse</span><span class="sxs-lookup"><span data-stu-id="8f24f-103">Use labels tooinstrument queries in SQL Data Warehouse</span></span>
<span data-ttu-id="8f24f-104">SQL Data Warehouse podporuje koncept názvem popisky dotazu.</span><span class="sxs-lookup"><span data-stu-id="8f24f-104">SQL Data Warehouse supports a concept called query labels.</span></span> <span data-ttu-id="8f24f-105">Před přechodem do jakékoli hloubka umožňuje podívejte se na příklad jednoho:</span><span class="sxs-lookup"><span data-stu-id="8f24f-105">Before going into any depth let's look at an example of one:</span></span>

```sql
SELECT *
FROM sys.tables
OPTION (LABEL = 'My Query Label')
;
```

<span data-ttu-id="8f24f-106">Tento poslední řádek značky hello řetězec popisek Moje dotazu toohello dotazu.</span><span class="sxs-lookup"><span data-stu-id="8f24f-106">This last line tags hello string 'My Query Label' toohello query.</span></span> <span data-ttu-id="8f24f-107">To je zvláště užitečné, protože popisek hello je dotaz může prostřednictvím hello zobrazení dynamické správy.</span><span class="sxs-lookup"><span data-stu-id="8f24f-107">This is particularly helpful as hello label is query-able through hello DMVs.</span></span> <span data-ttu-id="8f24f-108">To nám poskytuje mechanismus tootrack dolů problém dotazy a také toohelp identifikovat průběh prostřednictvím spustit ETL.</span><span class="sxs-lookup"><span data-stu-id="8f24f-108">This provides us with a mechanism tootrack down problem queries and also toohelp identify progress through an ETL run.</span></span>

<span data-ttu-id="8f24f-109">Dobrý zásady vytváření názvů pomáhá skutečně sem.</span><span class="sxs-lookup"><span data-stu-id="8f24f-109">A good naming convention really helps here.</span></span> <span data-ttu-id="8f24f-110">Například něco jako ' projektu: postup: příkaz: komentář se pomohou toouniquely identifikovat hello dotaz mezi všechny hello kódu ve správě zdrojového kódu.</span><span class="sxs-lookup"><span data-stu-id="8f24f-110">For example something like ' PROJECT : PROCEDURE : STATEMENT : COMMENT' would help toouniquely identify hello query in amongst all hello code in source control.</span></span>

<span data-ttu-id="8f24f-111">toosearch popiskem, můžete použít následující dotaz, který používá hello hello zobrazení dynamické správy:</span><span class="sxs-lookup"><span data-stu-id="8f24f-111">toosearch by label you can use hello following query that uses hello dynamic management views:</span></span>

```sql
SELECT  *
FROM    sys.dm_pdw_exec_requests r
WHERE   r.[label] = 'My Query Label'
;
```

> [!NOTE]
> <span data-ttu-id="8f24f-112">Je nezbytné při dotazování zabalení hranaté závorky a dvojité uvozovky kolem hello word popisek.</span><span class="sxs-lookup"><span data-stu-id="8f24f-112">It is essential that you wrap square brackets or double quotes around hello word label when querying.</span></span> <span data-ttu-id="8f24f-113">Popisek je vyhrazené slovo a způsobilo chybu, pokud nebyla oddělené.</span><span class="sxs-lookup"><span data-stu-id="8f24f-113">Label is a reserved word and will caused an error if it has not been delimited.</span></span>
> 
> 

## <a name="next-steps"></a><span data-ttu-id="8f24f-114">Další kroky</span><span class="sxs-lookup"><span data-stu-id="8f24f-114">Next steps</span></span>
<span data-ttu-id="8f24f-115">Další tipy pro vývoj, najdete v části [přehled vývoje][development overview].</span><span class="sxs-lookup"><span data-stu-id="8f24f-115">For more development tips, see [development overview][development overview].</span></span>

<!--Image references-->

<!--Article references-->
[development overview]: sql-data-warehouse-overview-develop.md

<!--MSDN references-->

<!--Other Web references-->
