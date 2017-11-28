---
title: "zobrazení aaaUsing T-SQL v Azure SQL Data Warehouse | Microsoft Docs"
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
ms.openlocfilehash: 3990b133946621691bdfa4b09523d21867470c74
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="views-in-sql-data-warehouse"></a><span data-ttu-id="cabbd-103">Zobrazení v SQL Data Warehouse</span><span class="sxs-lookup"><span data-stu-id="cabbd-103">Views in SQL Data Warehouse</span></span>
<span data-ttu-id="cabbd-104">Zobrazení jsou obzvláště užitečná v SQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="cabbd-104">Views are particularly useful in SQL Data Warehouse.</span></span> <span data-ttu-id="cabbd-105">Použitím mnoha různými způsoby tooimprove hello kvality vašeho řešení.</span><span class="sxs-lookup"><span data-stu-id="cabbd-105">They can be used in a number of different ways tooimprove hello quality of your solution.</span></span>  <span data-ttu-id="cabbd-106">V tomto článku klade důraz několik příkladů jak tooenrich řešení pomocí zobrazení, jakož i hello omezení, které je třeba toobe považována za.</span><span class="sxs-lookup"><span data-stu-id="cabbd-106">This article highlights a few examples of how tooenrich your solution with views, as well as hello limitations that need toobe considered.</span></span>

> [!NOTE]
> <span data-ttu-id="cabbd-107">Syntaxe pro `CREATE VIEW` není popsané v tomto článku.</span><span class="sxs-lookup"><span data-stu-id="cabbd-107">Syntax for `CREATE VIEW` is not discussed in this article.</span></span> <span data-ttu-id="cabbd-108">Podrobnosti najdete toohello [vytvořit zobrazení] [ CREATE VIEW] článku na webu MSDN pro tuto referenční informace.</span><span class="sxs-lookup"><span data-stu-id="cabbd-108">Please refer toohello [CREATE VIEW][CREATE VIEW] article on MSDN for this reference information.</span></span>
> 
> 

## <a name="architectural-abstraction"></a><span data-ttu-id="cabbd-109">Abstrakce architektury</span><span class="sxs-lookup"><span data-stu-id="cabbd-109">Architectural abstraction</span></span>
<span data-ttu-id="cabbd-110">Je velmi běžné vzor aplikací toore-vytvořit pomocí vytvoření tabulky AS vyberte funkce CTAS () a potom objektem přejmenování vzor, zatímco načítání dat tabulky.</span><span class="sxs-lookup"><span data-stu-id="cabbd-110">A very common application pattern is toore-create tables using CREATE TABLE AS SELECT (CTAS) followed by an object renaming pattern whilst loading data.</span></span>

<span data-ttu-id="cabbd-111">Následující příklad Hello přidá nové datum záznamy tooa datum dimenze.</span><span class="sxs-lookup"><span data-stu-id="cabbd-111">hello example below adds new date records tooa date dimension.</span></span> <span data-ttu-id="cabbd-112">Všimněte si, jak nové tabble, DimDate_New, je poprvé vytvořen a pak přejmenovat tooreplace hello původní verzi hello tabulky.</span><span class="sxs-lookup"><span data-stu-id="cabbd-112">Note how a new tabble, DimDate_New, is first created and then renamed tooreplace hello original version of hello table.</span></span>

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

RENAME OBJECT DimDate tooDimDate_Old;
RENAME OBJECT DimDate_New tooDimDate;

```

<span data-ttu-id="cabbd-113">Tento přístup může však způsobit tabulky zobrazování a ztrácejí ze zobrazení uživatele, jakož i "tabulka neexistuje" chybové zprávy.</span><span class="sxs-lookup"><span data-stu-id="cabbd-113">However, this approach can result in tables appearing and disappearing from a user's view as well as "table does not exist" error messages.</span></span> <span data-ttu-id="cabbd-114">Zobrazení může být použité tooprovide uživatelům konzistentní prezentační vrstvy, i když jsou přejmenovány hello základní objekty.</span><span class="sxs-lookup"><span data-stu-id="cabbd-114">Views can be used tooprovide users with a consistent presentation layer whilst hello underlying objects are renamed.</span></span> <span data-ttu-id="cabbd-115">Tím, že poskytuje uživatelům přístup k datům toohello prostřednictvím zobrazení, znamená, že uživatelé nepotřebují toohave viditelnost hello základní tabulky.</span><span class="sxs-lookup"><span data-stu-id="cabbd-115">By providing users access toohello data through a views, means users don't need toohave visibility of hello underlying tables.</span></span> <span data-ttu-id="cabbd-116">To nabízí konzistentní uživatelské prostředí a zajistit, že můžete hello data warehouse Designer momentální hello datový model a maximalizovat výkon pomocí funkce CTAS během procesu načítání dat hello.</span><span class="sxs-lookup"><span data-stu-id="cabbd-116">This provides a consistent user experience while ensuring that hello data warehouse designers can evolve hello data model and maximize performance by using CTAS during hello data loading process.</span></span>    

## <a name="performance-optimization"></a><span data-ttu-id="cabbd-117">Optimalizace výkonu</span><span class="sxs-lookup"><span data-stu-id="cabbd-117">Performance optimization</span></span>
<span data-ttu-id="cabbd-118">Zobrazení může být také využívané tooenforce výkonu optimalizované spojení mezi tabulkami.</span><span class="sxs-lookup"><span data-stu-id="cabbd-118">Views can also be utilized tooenforce performance optimized joins between tables.</span></span> <span data-ttu-id="cabbd-119">Například zobrazení můžete začlenit redundantní distribučního klíče jako součást hello připojení přesun dat toominimize kritéria.</span><span class="sxs-lookup"><span data-stu-id="cabbd-119">For example, a view can incorporate a redundant distribution key as part of hello joining criteria toominimize data movement.</span></span>  <span data-ttu-id="cabbd-120">Další výhodou zobrazení může být spojující pomocný parametr nebo tooforce specifického dotazu.</span><span class="sxs-lookup"><span data-stu-id="cabbd-120">Another benefit of a view could be tooforce a specific query or joining hint.</span></span> <span data-ttu-id="cabbd-121">Použití zobrazení tímto způsobem zaručuje, že vždy spojení optimální způsobem zabraňující hello potřebu uživatelé tooremember hello správné konstrukce pro jejich spojení.</span><span class="sxs-lookup"><span data-stu-id="cabbd-121">Using views in this manner guarantees that joins are always performed in an optimal fashion avoiding hello need for users tooremember hello correct construct for their joins.</span></span>

## <a name="limitations"></a><span data-ttu-id="cabbd-122">Omezení</span><span class="sxs-lookup"><span data-stu-id="cabbd-122">Limitations</span></span>
<span data-ttu-id="cabbd-123">Zobrazení v SQL Data Warehouse jsou pouze metadata.</span><span class="sxs-lookup"><span data-stu-id="cabbd-123">Views in SQL Data Warehouse are metadata only.</span></span>  <span data-ttu-id="cabbd-124">V důsledku toho nejsou k dispozici hello následující možnosti:</span><span class="sxs-lookup"><span data-stu-id="cabbd-124">Consequently hello following options are not available:</span></span>

* <span data-ttu-id="cabbd-125">Neexistuje žádná vazba možnost schématu</span><span class="sxs-lookup"><span data-stu-id="cabbd-125">There is no schema binding option</span></span>
* <span data-ttu-id="cabbd-126">Základní tabulky nelze aktualizovat prostřednictvím zobrazení hello</span><span class="sxs-lookup"><span data-stu-id="cabbd-126">Base tables cannot be updated through hello view</span></span>
* <span data-ttu-id="cabbd-127">Zobrazení nelze vytvořit přes dočasných tabulek</span><span class="sxs-lookup"><span data-stu-id="cabbd-127">Views cannot be created over temporary tables</span></span>
* <span data-ttu-id="cabbd-128">Neexistuje žádná podpora pro hello ROZBALTE / NOEXPAND pomocné parametry</span><span class="sxs-lookup"><span data-stu-id="cabbd-128">There is no support for hello EXPAND / NOEXPAND hints</span></span>
* <span data-ttu-id="cabbd-129">Nejsou žádná indexované zobrazení v SQL Data Warehouse</span><span class="sxs-lookup"><span data-stu-id="cabbd-129">There are no indexed views in SQL Data Warehouse</span></span>

## <a name="next-steps"></a><span data-ttu-id="cabbd-130">Další kroky</span><span class="sxs-lookup"><span data-stu-id="cabbd-130">Next steps</span></span>
<span data-ttu-id="cabbd-131">Další tipy pro vývoj najdete v části [Přehled vývoje SQL Data Warehouse][SQL Data Warehouse development overview].</span><span class="sxs-lookup"><span data-stu-id="cabbd-131">For more development tips, see [SQL Data Warehouse development overview][SQL Data Warehouse development overview].</span></span>
<span data-ttu-id="cabbd-132">Pro `CREATE VIEW` syntaxe naleznete příliš[vytvořit zobrazení][CREATE VIEW].</span><span class="sxs-lookup"><span data-stu-id="cabbd-132">For `CREATE VIEW` syntax please refer too[CREATE VIEW][CREATE VIEW].</span></span>

<!--Image references-->

<!--Article references-->
[SQL Data Warehouse development overview]: ./sql-data-warehouse-overview-develop.md

<!--MSDN references-->
[CREATE VIEW]: https://msdn.microsoft.com/en-us/library/ms187956.aspx

<!--Other Web references-->
