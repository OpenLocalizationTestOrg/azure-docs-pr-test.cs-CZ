---
title: "Použití zobrazení T-SQL v Azure SQL Data Warehouse | Microsoft Docs"
description: "Tipy pro používání zobrazení jazyka Transact-SQL v Azure SQL Data Warehouse na vývoj řešení."
services: sql-data-warehouse
documentationcenter: NA
author: jrowlandjones
manager: jhubbard
editor: 
ms.assetid: b5208f32-8f4a-4056-8788-2adbb253d9fd
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: t-sql
ms.date: 10/31/2016
ms.author: jrj;barbkess
ms.openlocfilehash: d2a03be810bd7f792876607ec735eb578b65a3b5
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="views-in-sql-data-warehouse"></a><span data-ttu-id="652f9-103">Zobrazení v SQL Data Warehouse</span><span class="sxs-lookup"><span data-stu-id="652f9-103">Views in SQL Data Warehouse</span></span>
<span data-ttu-id="652f9-104">Zobrazení jsou obzvláště užitečná v SQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="652f9-104">Views are particularly useful in SQL Data Warehouse.</span></span> <span data-ttu-id="652f9-105">Použitím mnoha různými způsoby ke zlepšení kvality vašeho řešení.</span><span class="sxs-lookup"><span data-stu-id="652f9-105">They can be used in a number of different ways to improve the quality of your solution.</span></span>  <span data-ttu-id="652f9-106">V tomto článku klade důraz několik příkladů, jak zlepšit komunikaci oddělení řešení pomocí zobrazení a také omezení, které je potřeba zvážit.</span><span class="sxs-lookup"><span data-stu-id="652f9-106">This article highlights a few examples of how to enrich your solution with views, as well as the limitations that need to be considered.</span></span>

> [!NOTE]
> <span data-ttu-id="652f9-107">Syntaxe pro `CREATE VIEW` není popsané v tomto článku.</span><span class="sxs-lookup"><span data-stu-id="652f9-107">Syntax for `CREATE VIEW` is not discussed in this article.</span></span> <span data-ttu-id="652f9-108">Podrobnosti najdete [vytvořit zobrazení] [ CREATE VIEW] článku na webu MSDN pro tuto referenční informace.</span><span class="sxs-lookup"><span data-stu-id="652f9-108">Please refer to the [CREATE VIEW][CREATE VIEW] article on MSDN for this reference information.</span></span>
> 
> 

## <a name="architectural-abstraction"></a><span data-ttu-id="652f9-109">Abstrakce architektury</span><span class="sxs-lookup"><span data-stu-id="652f9-109">Architectural abstraction</span></span>
<span data-ttu-id="652f9-110">Velmi běžný vzor aplikací je znovu vytvořit pomocí vytvoření tabulky AS vyberte funkce CTAS () a potom objektem přejmenování vzor, zatímco načítání dat tabulky.</span><span class="sxs-lookup"><span data-stu-id="652f9-110">A very common application pattern is to re-create tables using CREATE TABLE AS SELECT (CTAS) followed by an object renaming pattern whilst loading data.</span></span>

<span data-ttu-id="652f9-111">Následující příklad přidá nové datum záznamy do dimenze kalendářního data.</span><span class="sxs-lookup"><span data-stu-id="652f9-111">The example below adds new date records to a date dimension.</span></span> <span data-ttu-id="652f9-112">Všimněte si, jak je nové tabble, DimDate_New, nejprve vytvořit a pak přejmenovat nahradit původní verzi v tabulce.</span><span class="sxs-lookup"><span data-stu-id="652f9-112">Note how a new tabble, DimDate_New, is first created and then renamed to replace the original version of the table.</span></span>

```sql
CREATE TABLE dbo.DimDate_New
WITH (DISTRIBUTION = ROUND_ROBIN
, CLUSTERED INDEX (DateKey ASC)
)
AS
SELECT *
FROM   dbo.DimDate  AS prod
UNION ALL
SELECT *
FROM   dbo.DimDate_stg AS stg
;

RENAME OBJECT DimDate TO DimDate_Old;
RENAME OBJECT DimDate_New TO DimDate;

```

<span data-ttu-id="652f9-113">Tento přístup může však způsobit tabulky zobrazování a ztrácejí ze zobrazení uživatele, jakož i "tabulka neexistuje" chybové zprávy.</span><span class="sxs-lookup"><span data-stu-id="652f9-113">However, this approach can result in tables appearing and disappearing from a user's view as well as "table does not exist" error messages.</span></span> <span data-ttu-id="652f9-114">Zobrazení umožňuje uživatelům konzistentní prezentační vrstvou a přitom se základní objekty jsou přejmenovat.</span><span class="sxs-lookup"><span data-stu-id="652f9-114">Views can be used to provide users with a consistent presentation layer whilst the underlying objects are renamed.</span></span> <span data-ttu-id="652f9-115">Tím, že poskytuje uživatelům přístup k datům prostřednictvím zobrazení, znamená, že uživatelé nemusí mít viditelnost základní tabulky.</span><span class="sxs-lookup"><span data-stu-id="652f9-115">By providing users access to the data through a views, means users don't need to have visibility of the underlying tables.</span></span> <span data-ttu-id="652f9-116">To nabízí konzistentní uživatelské prostředí, a zajistíte, že datový sklad Designer můžete vyvíjet datový model a maximalizovat výkon pomocí funkce CTAS během procesu načítání dat.</span><span class="sxs-lookup"><span data-stu-id="652f9-116">This provides a consistent user experience while ensuring that the data warehouse designers can evolve the data model and maximize performance by using CTAS during the data loading process.</span></span>    

## <a name="performance-optimization"></a><span data-ttu-id="652f9-117">Optimalizace výkonu</span><span class="sxs-lookup"><span data-stu-id="652f9-117">Performance optimization</span></span>
<span data-ttu-id="652f9-118">Zobrazení, můžete použít také k vynucení výkonu optimalizované spojení mezi tabulkami.</span><span class="sxs-lookup"><span data-stu-id="652f9-118">Views can also be utilized to enforce performance optimized joins between tables.</span></span> <span data-ttu-id="652f9-119">Například jako součást kritérií pro spojující minimalizovat přesun dat zobrazení začlenit redundantní distribučního klíče.</span><span class="sxs-lookup"><span data-stu-id="652f9-119">For example, a view can incorporate a redundant distribution key as part of the joining criteria to minimize data movement.</span></span>  <span data-ttu-id="652f9-120">Další výhodou zobrazení může být vynutit spojující pomocný parametr nebo specifického dotazu.</span><span class="sxs-lookup"><span data-stu-id="652f9-120">Another benefit of a view could be to force a specific query or joining hint.</span></span> <span data-ttu-id="652f9-121">Použití zobrazení tímto způsobem zaručuje, že vždy spojení optimální způsobem vyloučení uživatelé pamatovat správné konstrukce pro jejich spojení.</span><span class="sxs-lookup"><span data-stu-id="652f9-121">Using views in this manner guarantees that joins are always performed in an optimal fashion avoiding the need for users to remember the correct construct for their joins.</span></span>

## <a name="limitations"></a><span data-ttu-id="652f9-122">Omezení</span><span class="sxs-lookup"><span data-stu-id="652f9-122">Limitations</span></span>
<span data-ttu-id="652f9-123">Zobrazení v SQL Data Warehouse jsou pouze metadata.</span><span class="sxs-lookup"><span data-stu-id="652f9-123">Views in SQL Data Warehouse are metadata only.</span></span>  <span data-ttu-id="652f9-124">V důsledku toho nejsou k dispozici následující možnosti:</span><span class="sxs-lookup"><span data-stu-id="652f9-124">Consequently the following options are not available:</span></span>

* <span data-ttu-id="652f9-125">Neexistuje žádná vazba možnost schématu</span><span class="sxs-lookup"><span data-stu-id="652f9-125">There is no schema binding option</span></span>
* <span data-ttu-id="652f9-126">Základní tabulky nelze aktualizovat prostřednictvím zobrazení</span><span class="sxs-lookup"><span data-stu-id="652f9-126">Base tables cannot be updated through the view</span></span>
* <span data-ttu-id="652f9-127">Zobrazení nelze vytvořit přes dočasných tabulek</span><span class="sxs-lookup"><span data-stu-id="652f9-127">Views cannot be created over temporary tables</span></span>
* <span data-ttu-id="652f9-128">Neexistuje žádná podpora pro ROZBALENÍ nebo NOEXPAND pomocné parametry</span><span class="sxs-lookup"><span data-stu-id="652f9-128">There is no support for the EXPAND / NOEXPAND hints</span></span>
* <span data-ttu-id="652f9-129">Nejsou žádná indexované zobrazení v SQL Data Warehouse</span><span class="sxs-lookup"><span data-stu-id="652f9-129">There are no indexed views in SQL Data Warehouse</span></span>

## <a name="next-steps"></a><span data-ttu-id="652f9-130">Další kroky</span><span class="sxs-lookup"><span data-stu-id="652f9-130">Next steps</span></span>
<span data-ttu-id="652f9-131">Další tipy pro vývoj najdete v části [Přehled vývoje SQL Data Warehouse][SQL Data Warehouse development overview].</span><span class="sxs-lookup"><span data-stu-id="652f9-131">For more development tips, see [SQL Data Warehouse development overview][SQL Data Warehouse development overview].</span></span>
<span data-ttu-id="652f9-132">Pro `CREATE VIEW` syntaxe naleznete [vytvořit zobrazení][CREATE VIEW].</span><span class="sxs-lookup"><span data-stu-id="652f9-132">For `CREATE VIEW` syntax please refer to [CREATE VIEW][CREATE VIEW].</span></span>

<!--Image references-->

<!--Article references-->
[SQL Data Warehouse development overview]: ./sql-data-warehouse-overview-develop.md

<!--MSDN references-->
[CREATE VIEW]: https://msdn.microsoft.com/en-us/library/ms187956.aspx

<!--Other Web references-->
