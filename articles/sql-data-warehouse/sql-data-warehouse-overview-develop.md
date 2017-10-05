---
title: "Prostředky pro vývoj datového skladu v Azure | Microsoft Docs"
description: "Vývoj koncepty, rozhodnutí o návrhu, doporučení a kódování techniky pro SQL Data Warehouse."
services: sql-data-warehouse
documentationcenter: NA
author: jrowlandjones
manager: barbkess
editor: 
ms.assetid: 996e3afc-c21c-4e21-b9df-997f953f6dfd
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: develop
ms.date: 10/31/2016
ms.author: jrj;barbkess
ms.openlocfilehash: b85a4f09e561e429aa5bf46ec680014487fb40c7
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="design-decisions-and-coding-techniques-for-sql-data-warehouse"></a><span data-ttu-id="9fefe-103">Rozhodnutí o návrhu a kódování techniky pro SQL Data Warehouse</span><span class="sxs-lookup"><span data-stu-id="9fefe-103">Design decisions and coding techniques for SQL Data Warehouse</span></span>
<span data-ttu-id="9fefe-104">Prohlédněte si prostřednictvím tyto články vývoj, abyste lépe pochopili rozhodnutí o návrhu klíče, doporučení a kódování techniky pro SQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="9fefe-104">Take a look through these development articles to better understand key design decisions, recommendations and coding techniques for SQL Data Warehouse.</span></span>

## <a name="key-design-decisions"></a><span data-ttu-id="9fefe-105">Rozhodnutí o návrhu klíče</span><span class="sxs-lookup"><span data-stu-id="9fefe-105">Key design decisions</span></span>
<span data-ttu-id="9fefe-106">V následujících článcích zvýrazněte některé klíčové koncepty a rozhodnutí o návrhu, které je třeba porozumět pro vývoj distribuované datového skladu pomocí SQL Data Warehouse:</span><span class="sxs-lookup"><span data-stu-id="9fefe-106">The following articles highlight some of the key concepts and design decisions you will need to understand for the development of your distributed data warehouse using SQL Data Warehouse:</span></span>

* <span data-ttu-id="9fefe-107">[připojení][connections]</span><span class="sxs-lookup"><span data-stu-id="9fefe-107">[connections][connections]</span></span>
* <span data-ttu-id="9fefe-108">[souběžnosti][concurrency]</span><span class="sxs-lookup"><span data-stu-id="9fefe-108">[concurrency][concurrency]</span></span>
* <span data-ttu-id="9fefe-109">[transakce][transactions]</span><span class="sxs-lookup"><span data-stu-id="9fefe-109">[transactions][transactions]</span></span>
* <span data-ttu-id="9fefe-110">[uživatelem definované schémata][user-defined schemas]</span><span class="sxs-lookup"><span data-stu-id="9fefe-110">[user-defined schemas][user-defined schemas]</span></span>
* <span data-ttu-id="9fefe-111">[distribuce tabulky][table distribution]</span><span class="sxs-lookup"><span data-stu-id="9fefe-111">[table distribution][table distribution]</span></span>
* <span data-ttu-id="9fefe-112">[indexy tabulek][table indexes]</span><span class="sxs-lookup"><span data-stu-id="9fefe-112">[table indexes][table indexes]</span></span>
* <span data-ttu-id="9fefe-113">[Tabulka oddílů][table partitions]</span><span class="sxs-lookup"><span data-stu-id="9fefe-113">[table partitions][table partitions]</span></span>
* <span data-ttu-id="9fefe-114">[FUNKCE CTAS][CTAS]</span><span class="sxs-lookup"><span data-stu-id="9fefe-114">[CTAS][CTAS]</span></span>
* <span data-ttu-id="9fefe-115">[statistiky][statistics]</span><span class="sxs-lookup"><span data-stu-id="9fefe-115">[statistics][statistics]</span></span>

## <a name="development-recommendations-and-coding-techniques"></a><span data-ttu-id="9fefe-116">Doporučení pro vývoj a techniky kódování</span><span class="sxs-lookup"><span data-stu-id="9fefe-116">Development recommendations and coding techniques</span></span>
<span data-ttu-id="9fefe-117">Tyto články zvýraznit konkrétní kódování techniky, tipy a doporučení pro vývoj SQL Data Warehouse:</span><span class="sxs-lookup"><span data-stu-id="9fefe-117">These articles highlight specific coding techniques, tips and recommendations for developing your SQL Data Warehouse:</span></span>

* <span data-ttu-id="9fefe-118">[uložené procedury][stored procedures]</span><span class="sxs-lookup"><span data-stu-id="9fefe-118">[stored procedures][stored procedures]</span></span>
* <span data-ttu-id="9fefe-119">[popisky][labels]</span><span class="sxs-lookup"><span data-stu-id="9fefe-119">[labels][labels]</span></span>
* <span data-ttu-id="9fefe-120">[zobrazení][views]</span><span class="sxs-lookup"><span data-stu-id="9fefe-120">[views][views]</span></span>
* <span data-ttu-id="9fefe-121">[dočasné tabulky][temporary tables]</span><span class="sxs-lookup"><span data-stu-id="9fefe-121">[temporary tables][temporary tables]</span></span>
* <span data-ttu-id="9fefe-122">[dynamické SQL][dynamic SQL]</span><span class="sxs-lookup"><span data-stu-id="9fefe-122">[dynamic SQL][dynamic SQL]</span></span>
* <span data-ttu-id="9fefe-123">[ve smyčce][looping]</span><span class="sxs-lookup"><span data-stu-id="9fefe-123">[looping][looping]</span></span>
* <span data-ttu-id="9fefe-124">[Seskupit podle možností][group by options]</span><span class="sxs-lookup"><span data-stu-id="9fefe-124">[group by options][group by options]</span></span>
* <span data-ttu-id="9fefe-125">[přiřazení proměnné][variable assignment]</span><span class="sxs-lookup"><span data-stu-id="9fefe-125">[variable assignment][variable assignment]</span></span>

## <a name="next-steps"></a><span data-ttu-id="9fefe-126">Další kroky</span><span class="sxs-lookup"><span data-stu-id="9fefe-126">Next steps</span></span>
<span data-ttu-id="9fefe-127">Jakmile jste prostřednictvím články vývoj prohlédněte prostřednictvím [referenční dokumentace jazyka Transact-SQL] [ Transact-SQL reference] stránka Další informace o podporovaných syntaxe pro SQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="9fefe-127">Once you have been through the development articles take a look through the [Transact-SQL reference][Transact-SQL reference] page for more details on the supported syntax for SQL Data Warehouse.</span></span>

<!--Image references-->

<!--Article references-->
[concurrency]: ./sql-data-warehouse-develop-concurrency.md
[connections]: ./sql-data-warehouse-connect-overview.md
[CTAS]: ./sql-data-warehouse-develop-ctas.md
[dynamic SQL]: ./sql-data-warehouse-develop-dynamic-sql.md
[group by options]: ./sql-data-warehouse-develop-group-by-options.md
[labels]: ./sql-data-warehouse-develop-label.md
[looping]: ./sql-data-warehouse-develop-loops.md
[statistics]: ./sql-data-warehouse-tables-statistics.md
[stored procedures]: ./sql-data-warehouse-develop-stored-procedures.md
[table distribution]: ./sql-data-warehouse-tables-distribute.md
[table indexes]: ./sql-data-warehouse-tables-index.md
[table partitions]: ./sql-data-warehouse-tables-partition.md
[temporary tables]: ./sql-data-warehouse-tables-temporary.md
[transactions]: ./sql-data-warehouse-develop-transactions.md
[user-defined schemas]: ./sql-data-warehouse-develop-user-defined-schemas.md
[variable assignment]: ./sql-data-warehouse-develop-variable-assignment.md
[views]: ./sql-data-warehouse-develop-views.md
[Transact-SQL reference]: ./sql-data-warehouse-overview-reference.md

<!--MSDN references-->
[renaming objects]: https://msdn.microsoft.com/library/mt631611.aspx

<!--Other Web references-->
