---
title: "aaaResources pro vývoj datového skladu v Azure | Microsoft Docs"
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
ms.openlocfilehash: 67e3a6a3e2664919c3445ea5d5eba251054de020
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="design-decisions-and-coding-techniques-for-sql-data-warehouse"></a><span data-ttu-id="5dc3d-103">Rozhodnutí o návrhu a kódování techniky pro SQL Data Warehouse</span><span class="sxs-lookup"><span data-stu-id="5dc3d-103">Design decisions and coding techniques for SQL Data Warehouse</span></span>
<span data-ttu-id="5dc3d-104">Prohlédněte si prostřednictvím tyto články vývoj toobetter pochopit rozhodnutí o návrhu klíče, doporučení a kódování techniky pro SQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="5dc3d-104">Take a look through these development articles toobetter understand key design decisions, recommendations and coding techniques for SQL Data Warehouse.</span></span>

## <a name="key-design-decisions"></a><span data-ttu-id="5dc3d-105">Rozhodnutí o návrhu klíče</span><span class="sxs-lookup"><span data-stu-id="5dc3d-105">Key design decisions</span></span>
<span data-ttu-id="5dc3d-106">Hello následující články zvýrazněte některé hello klíčové koncepty a rozhodnutí o návrhu, že které budete potřebovat toounderstand pro vývoj hello distribuované datového skladu pomocí SQL Data Warehouse:</span><span class="sxs-lookup"><span data-stu-id="5dc3d-106">hello following articles highlight some of hello key concepts and design decisions you will need toounderstand for hello development of your distributed data warehouse using SQL Data Warehouse:</span></span>

* <span data-ttu-id="5dc3d-107">[připojení][connections]</span><span class="sxs-lookup"><span data-stu-id="5dc3d-107">[connections][connections]</span></span>
* <span data-ttu-id="5dc3d-108">[souběžnosti][concurrency]</span><span class="sxs-lookup"><span data-stu-id="5dc3d-108">[concurrency][concurrency]</span></span>
* <span data-ttu-id="5dc3d-109">[transakce][transactions]</span><span class="sxs-lookup"><span data-stu-id="5dc3d-109">[transactions][transactions]</span></span>
* <span data-ttu-id="5dc3d-110">[uživatelem definované schémata][user-defined schemas]</span><span class="sxs-lookup"><span data-stu-id="5dc3d-110">[user-defined schemas][user-defined schemas]</span></span>
* <span data-ttu-id="5dc3d-111">[distribuce tabulky][table distribution]</span><span class="sxs-lookup"><span data-stu-id="5dc3d-111">[table distribution][table distribution]</span></span>
* <span data-ttu-id="5dc3d-112">[indexy tabulek][table indexes]</span><span class="sxs-lookup"><span data-stu-id="5dc3d-112">[table indexes][table indexes]</span></span>
* <span data-ttu-id="5dc3d-113">[Tabulka oddílů][table partitions]</span><span class="sxs-lookup"><span data-stu-id="5dc3d-113">[table partitions][table partitions]</span></span>
* <span data-ttu-id="5dc3d-114">[FUNKCE CTAS][CTAS]</span><span class="sxs-lookup"><span data-stu-id="5dc3d-114">[CTAS][CTAS]</span></span>
* <span data-ttu-id="5dc3d-115">[statistiky][statistics]</span><span class="sxs-lookup"><span data-stu-id="5dc3d-115">[statistics][statistics]</span></span>

## <a name="development-recommendations-and-coding-techniques"></a><span data-ttu-id="5dc3d-116">Doporučení pro vývoj a techniky kódování</span><span class="sxs-lookup"><span data-stu-id="5dc3d-116">Development recommendations and coding techniques</span></span>
<span data-ttu-id="5dc3d-117">Tyto články zvýraznit konkrétní kódování techniky, tipy a doporučení pro vývoj SQL Data Warehouse:</span><span class="sxs-lookup"><span data-stu-id="5dc3d-117">These articles highlight specific coding techniques, tips and recommendations for developing your SQL Data Warehouse:</span></span>

* <span data-ttu-id="5dc3d-118">[uložené procedury][stored procedures]</span><span class="sxs-lookup"><span data-stu-id="5dc3d-118">[stored procedures][stored procedures]</span></span>
* <span data-ttu-id="5dc3d-119">[popisky][labels]</span><span class="sxs-lookup"><span data-stu-id="5dc3d-119">[labels][labels]</span></span>
* <span data-ttu-id="5dc3d-120">[zobrazení][views]</span><span class="sxs-lookup"><span data-stu-id="5dc3d-120">[views][views]</span></span>
* <span data-ttu-id="5dc3d-121">[dočasné tabulky][temporary tables]</span><span class="sxs-lookup"><span data-stu-id="5dc3d-121">[temporary tables][temporary tables]</span></span>
* <span data-ttu-id="5dc3d-122">[dynamické SQL][dynamic SQL]</span><span class="sxs-lookup"><span data-stu-id="5dc3d-122">[dynamic SQL][dynamic SQL]</span></span>
* <span data-ttu-id="5dc3d-123">[ve smyčce][looping]</span><span class="sxs-lookup"><span data-stu-id="5dc3d-123">[looping][looping]</span></span>
* <span data-ttu-id="5dc3d-124">[Seskupit podle možností][group by options]</span><span class="sxs-lookup"><span data-stu-id="5dc3d-124">[group by options][group by options]</span></span>
* <span data-ttu-id="5dc3d-125">[přiřazení proměnné][variable assignment]</span><span class="sxs-lookup"><span data-stu-id="5dc3d-125">[variable assignment][variable assignment]</span></span>

## <a name="next-steps"></a><span data-ttu-id="5dc3d-126">Další kroky</span><span class="sxs-lookup"><span data-stu-id="5dc3d-126">Next steps</span></span>
<span data-ttu-id="5dc3d-127">Jakmile jste prostřednictvím hello vývoj články prohlédněte prostřednictvím hello [referenční dokumentace jazyka Transact-SQL] [ Transact-SQL reference] stránka Další informace o syntaxi hello podporované pro SQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="5dc3d-127">Once you have been through hello development articles take a look through hello [Transact-SQL reference][Transact-SQL reference] page for more details on hello supported syntax for SQL Data Warehouse.</span></span>

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
