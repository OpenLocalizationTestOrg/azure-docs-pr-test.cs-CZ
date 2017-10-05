---
title: "Použití popisků na nástrojích dotazy v SQL Data Warehouse | Microsoft Docs"
description: "Tipy pro používání popisky na nástrojích dotazy v Azure SQL Data Warehouse na vývoj řešení."
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
ms.openlocfilehash: 9e75bbe528a427724a623305fbd45e2277e9d0af
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="use-labels-to-instrument-queries-in-sql-data-warehouse"></a><span data-ttu-id="e85df-103">Použití popisků k nástroji dotazů v SQL Data Warehouse</span><span class="sxs-lookup"><span data-stu-id="e85df-103">Use labels to instrument queries in SQL Data Warehouse</span></span>
<span data-ttu-id="e85df-104">SQL Data Warehouse podporuje koncept názvem popisky dotazu.</span><span class="sxs-lookup"><span data-stu-id="e85df-104">SQL Data Warehouse supports a concept called query labels.</span></span> <span data-ttu-id="e85df-105">Před přechodem do jakékoli hloubka umožňuje podívejte se na příklad jednoho:</span><span class="sxs-lookup"><span data-stu-id="e85df-105">Before going into any depth let's look at an example of one:</span></span>

```sql
SELECT *
FROM sys.tables
OPTION (LABEL = 'My Query Label')
;
```

<span data-ttu-id="e85df-106">Tento poslední řádek značky řetězec popisek Moje dotazu pro dotaz.</span><span class="sxs-lookup"><span data-stu-id="e85df-106">This last line tags the string 'My Query Label' to the query.</span></span> <span data-ttu-id="e85df-107">To je zvláště užitečné, protože popisek je dotaz může prostřednictvím zobrazení dynamické správy.</span><span class="sxs-lookup"><span data-stu-id="e85df-107">This is particularly helpful as the label is query-able through the DMVs.</span></span> <span data-ttu-id="e85df-108">To poskytuje nám mechanismus sledovat problém dotazy a také k identifikaci průběh prostřednictvím spustit ETL.</span><span class="sxs-lookup"><span data-stu-id="e85df-108">This provides us with a mechanism to track down problem queries and also to help identify progress through an ETL run.</span></span>

<span data-ttu-id="e85df-109">Dobrý zásady vytváření názvů pomáhá skutečně sem.</span><span class="sxs-lookup"><span data-stu-id="e85df-109">A good naming convention really helps here.</span></span> <span data-ttu-id="e85df-110">Například něco jako ' projektu: postup: příkaz: komentář se pomohou k jednoznačné identifikaci dotazu v mezi všechny kód ve správě zdrojového kódu.</span><span class="sxs-lookup"><span data-stu-id="e85df-110">For example something like ' PROJECT : PROCEDURE : STATEMENT : COMMENT' would help to uniquely identify the query in amongst all the code in source control.</span></span>

<span data-ttu-id="e85df-111">Pokud chcete hledat podle popisku můžete použít následující dotaz, který používá zobrazení dynamické správy:</span><span class="sxs-lookup"><span data-stu-id="e85df-111">To search by label you can use the following query that uses the dynamic management views:</span></span>

```sql
SELECT  *
FROM    sys.dm_pdw_exec_requests r
WHERE   r.[label] = 'My Query Label'
;
```

> [!NOTE]
> <span data-ttu-id="e85df-112">Je nezbytné při dotazování zabalení hranaté závorky a dvojité uvozovky kolem popisek aplikace word.</span><span class="sxs-lookup"><span data-stu-id="e85df-112">It is essential that you wrap square brackets or double quotes around the word label when querying.</span></span> <span data-ttu-id="e85df-113">Popisek je vyhrazené slovo a způsobilo chybu, pokud nebyla oddělené.</span><span class="sxs-lookup"><span data-stu-id="e85df-113">Label is a reserved word and will caused an error if it has not been delimited.</span></span>
> 
> 

## <a name="next-steps"></a><span data-ttu-id="e85df-114">Další kroky</span><span class="sxs-lookup"><span data-stu-id="e85df-114">Next steps</span></span>
<span data-ttu-id="e85df-115">Další tipy pro vývoj, najdete v části [přehled vývoje][development overview].</span><span class="sxs-lookup"><span data-stu-id="e85df-115">For more development tips, see [development overview][development overview].</span></span>

<!--Image references-->

<!--Article references-->
[development overview]: sql-data-warehouse-overview-develop.md

<!--MSDN references-->

<!--Other Web references-->
