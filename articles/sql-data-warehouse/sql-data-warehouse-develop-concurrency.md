---
title: "Souběžnost a úlohy správy v SQL Data Warehouse | Microsoft Docs"
description: "Porozumět souběžnosti a úlohy správy v Azure SQL Data Warehouse pro vývoj řešení."
services: sql-data-warehouse
documentationcenter: NA
author: sqlmojo
manager: jhubbard
editor: 
ms.assetid: ef170f39-ae24-4b04-af76-53bb4c4d16d3
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: performance
ms.date: 08/23/2017
ms.author: joeyong;barbkess;kavithaj
ms.openlocfilehash: eaf2d43286dbaa52ada1430fbb7ce1e37f41c0d4
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="concurrency-and-workload-management-in-sql-data-warehouse"></a><span data-ttu-id="cd346-103">Souběžnost a úlohy správy v SQL Data Warehouse</span><span class="sxs-lookup"><span data-stu-id="cd346-103">Concurrency and workload management in SQL Data Warehouse</span></span>
<span data-ttu-id="cd346-104">Zajistit předvídatelný výkon ve velkém měřítku, Microsoft Azure SQL Data Warehouse pomáhá řídit souběžnosti úrovně a přidělení prostředků jako paměti a procesoru stanovení priorit.</span><span class="sxs-lookup"><span data-stu-id="cd346-104">To deliver predictable performance at scale, Microsoft Azure SQL Data Warehouse helps you control concurrency levels and resource allocations like memory and CPU prioritization.</span></span> <span data-ttu-id="cd346-105">Tento článek vás seznámí s koncepty souběžnosti a úlohy správy, která vysvětluje, jak oba implementována a jak je můžete řídit v datovém skladu.</span><span class="sxs-lookup"><span data-stu-id="cd346-105">This article introduces you to the concepts of concurrency and workload management, explaining how both features have been implemented and how you can control them in your data warehouse.</span></span> <span data-ttu-id="cd346-106">Úlohy správy datového skladu SQL je určený můžete podporovat prostředí s více uživateli.</span><span class="sxs-lookup"><span data-stu-id="cd346-106">SQL Data Warehouse workload management is intended to help you support multi-user environments.</span></span> <span data-ttu-id="cd346-107">Rozhraní není určeno pro více klientů úlohy.</span><span class="sxs-lookup"><span data-stu-id="cd346-107">It is not intended for multi-tenant workloads.</span></span>

## <a name="concurrency-limits"></a><span data-ttu-id="cd346-108">Omezení souběžnosti</span><span class="sxs-lookup"><span data-stu-id="cd346-108">Concurrency limits</span></span>
<span data-ttu-id="cd346-109">SQL Data Warehouse umožňuje až 1 024 souběžných připojení.</span><span class="sxs-lookup"><span data-stu-id="cd346-109">SQL Data Warehouse allows up to 1,024 concurrent connections.</span></span> <span data-ttu-id="cd346-110">Všechna 1 024 připojení mohou souběžně odesílat dotazy.</span><span class="sxs-lookup"><span data-stu-id="cd346-110">All 1,024 connections can submit queries concurrently.</span></span> <span data-ttu-id="cd346-111">Optimalizovat výkon, ale může SQL Data Warehouse fronty některé dotazy zajistit, že každý dotaz přidělení minimální paměti.</span><span class="sxs-lookup"><span data-stu-id="cd346-111">However, to optimize throughput, SQL Data Warehouse may queue some queries to ensure that each query receives a minimal memory grant.</span></span> <span data-ttu-id="cd346-112">Služba Řízení front dojde v době provedení dotazu.</span><span class="sxs-lookup"><span data-stu-id="cd346-112">Queuing occurs at query execution time.</span></span> <span data-ttu-id="cd346-113">Řízení front dotazy po dosažení souběžnosti omezení, SQL Data Warehouse může zvýšit celkovou propustnost zajistíte, že aktivní dotazy získat přístup k prostředkům zásadní potřebnou paměť.</span><span class="sxs-lookup"><span data-stu-id="cd346-113">By queuing queries when concurrency limits are reached, SQL Data Warehouse can increase total throughput by ensuring that active queries get access to critically needed memory resources.</span></span>  

<span data-ttu-id="cd346-114">Omezení souběžnosti se řídí dvěma konceptů: *souběžných dotazů* a *souběžnosti sloty*.</span><span class="sxs-lookup"><span data-stu-id="cd346-114">Concurrency limits are governed by two concepts: *concurrent queries* and *concurrency slots*.</span></span> <span data-ttu-id="cd346-115">Pro dotaz provést musí provést v rámci omezení souběžnosti dotazu a přidělení slotu souběžnosti.</span><span class="sxs-lookup"><span data-stu-id="cd346-115">For a query to execute, it must execute within both the query concurrency limit and the concurrency slot allocation.</span></span>

* <span data-ttu-id="cd346-116">Počet souběžných dotazů jsou dotazy spuštění ve stejnou dobu.</span><span class="sxs-lookup"><span data-stu-id="cd346-116">Concurrent queries are the queries executing at the same time.</span></span> <span data-ttu-id="cd346-117">SQL Data Warehouse podporuje až 32 souběžné dotazy větší velikostí DWU.</span><span class="sxs-lookup"><span data-stu-id="cd346-117">SQL Data Warehouse supports up to 32 concurrent queries on the larger DWU sizes.</span></span>
* <span data-ttu-id="cd346-118">Sloty souběžnosti mají při přidělování podle DWU.</span><span class="sxs-lookup"><span data-stu-id="cd346-118">Concurrency slots are allocated based on DWU.</span></span> <span data-ttu-id="cd346-119">Každý 100 DWU poskytuje 4 sloty souběžnosti.</span><span class="sxs-lookup"><span data-stu-id="cd346-119">Each 100 DWU provides 4 concurrency slots.</span></span> <span data-ttu-id="cd346-120">Například od DW100 přiděluje 4 sloty souběžnosti a DW1000 přiděluje 40.</span><span class="sxs-lookup"><span data-stu-id="cd346-120">For example, a DW100 allocates 4 concurrency slots and DW1000 allocates 40.</span></span> <span data-ttu-id="cd346-121">Každý dotaz využívá jednu nebo více souběžnosti sloty, závisí na [Třída prostředků](#resource-classes) dotazu.</span><span class="sxs-lookup"><span data-stu-id="cd346-121">Each query consumes one or more concurrency slots, dependent on the [resource class](#resource-classes) of the query.</span></span> <span data-ttu-id="cd346-122">Dotazy spuštěnými ve třídě prostředků smallrc využívat jeden slot souběžnosti.</span><span class="sxs-lookup"><span data-stu-id="cd346-122">Queries running in the smallrc resource class consume one concurrency slot.</span></span> <span data-ttu-id="cd346-123">Dotazy spuštěnými v vyšší Třída prostředků využívat další sloty souběžnosti.</span><span class="sxs-lookup"><span data-stu-id="cd346-123">Queries running in a higher resource class consume more concurrency slots.</span></span>

<span data-ttu-id="cd346-124">Následující tabulka popisuje limity pro souběžné dotazy a sloty souběžnosti v různých velikostí DWU.</span><span class="sxs-lookup"><span data-stu-id="cd346-124">The following table describes the limits for both concurrent queries and concurrency slots at the various DWU sizes.</span></span>

### <a name="concurrency-limits"></a><span data-ttu-id="cd346-125">Omezení souběžnosti</span><span class="sxs-lookup"><span data-stu-id="cd346-125">Concurrency limits</span></span>
| <span data-ttu-id="cd346-126">DWU</span><span class="sxs-lookup"><span data-stu-id="cd346-126">DWU</span></span> | <span data-ttu-id="cd346-127">Maximální počet souběžných dotazů</span><span class="sxs-lookup"><span data-stu-id="cd346-127">Max concurrent queries</span></span> | <span data-ttu-id="cd346-128">Přidělené sloty souběžnosti</span><span class="sxs-lookup"><span data-stu-id="cd346-128">Concurrency slots allocated</span></span> |
|:--- |:---:|:---:|
| <span data-ttu-id="cd346-129">OD DW100</span><span class="sxs-lookup"><span data-stu-id="cd346-129">DW100</span></span> |<span data-ttu-id="cd346-130">4</span><span class="sxs-lookup"><span data-stu-id="cd346-130">4</span></span> |<span data-ttu-id="cd346-131">4</span><span class="sxs-lookup"><span data-stu-id="cd346-131">4</span></span> |
| <span data-ttu-id="cd346-132">DW200</span><span class="sxs-lookup"><span data-stu-id="cd346-132">DW200</span></span> |<span data-ttu-id="cd346-133">8</span><span class="sxs-lookup"><span data-stu-id="cd346-133">8</span></span> |<span data-ttu-id="cd346-134">8</span><span class="sxs-lookup"><span data-stu-id="cd346-134">8</span></span> |
| <span data-ttu-id="cd346-135">DW300</span><span class="sxs-lookup"><span data-stu-id="cd346-135">DW300</span></span> |<span data-ttu-id="cd346-136">12</span><span class="sxs-lookup"><span data-stu-id="cd346-136">12</span></span> |<span data-ttu-id="cd346-137">12</span><span class="sxs-lookup"><span data-stu-id="cd346-137">12</span></span> |
| <span data-ttu-id="cd346-138">DW400</span><span class="sxs-lookup"><span data-stu-id="cd346-138">DW400</span></span> |<span data-ttu-id="cd346-139">16</span><span class="sxs-lookup"><span data-stu-id="cd346-139">16</span></span> |<span data-ttu-id="cd346-140">16</span><span class="sxs-lookup"><span data-stu-id="cd346-140">16</span></span> |
| <span data-ttu-id="cd346-141">DW500</span><span class="sxs-lookup"><span data-stu-id="cd346-141">DW500</span></span> |<span data-ttu-id="cd346-142">20</span><span class="sxs-lookup"><span data-stu-id="cd346-142">20</span></span> |<span data-ttu-id="cd346-143">20</span><span class="sxs-lookup"><span data-stu-id="cd346-143">20</span></span> |
| <span data-ttu-id="cd346-144">DW600</span><span class="sxs-lookup"><span data-stu-id="cd346-144">DW600</span></span> |<span data-ttu-id="cd346-145">24</span><span class="sxs-lookup"><span data-stu-id="cd346-145">24</span></span> |<span data-ttu-id="cd346-146">24</span><span class="sxs-lookup"><span data-stu-id="cd346-146">24</span></span> |
| <span data-ttu-id="cd346-147">DW1000</span><span class="sxs-lookup"><span data-stu-id="cd346-147">DW1000</span></span> |<span data-ttu-id="cd346-148">32</span><span class="sxs-lookup"><span data-stu-id="cd346-148">32</span></span> |<span data-ttu-id="cd346-149">40</span><span class="sxs-lookup"><span data-stu-id="cd346-149">40</span></span> |
| <span data-ttu-id="cd346-150">DW1200</span><span class="sxs-lookup"><span data-stu-id="cd346-150">DW1200</span></span> |<span data-ttu-id="cd346-151">32</span><span class="sxs-lookup"><span data-stu-id="cd346-151">32</span></span> |<span data-ttu-id="cd346-152">48</span><span class="sxs-lookup"><span data-stu-id="cd346-152">48</span></span> |
| <span data-ttu-id="cd346-153">DW1500</span><span class="sxs-lookup"><span data-stu-id="cd346-153">DW1500</span></span> |<span data-ttu-id="cd346-154">32</span><span class="sxs-lookup"><span data-stu-id="cd346-154">32</span></span> |<span data-ttu-id="cd346-155">60</span><span class="sxs-lookup"><span data-stu-id="cd346-155">60</span></span> |
| <span data-ttu-id="cd346-156">DW2000</span><span class="sxs-lookup"><span data-stu-id="cd346-156">DW2000</span></span> |<span data-ttu-id="cd346-157">32</span><span class="sxs-lookup"><span data-stu-id="cd346-157">32</span></span> |<span data-ttu-id="cd346-158">80</span><span class="sxs-lookup"><span data-stu-id="cd346-158">80</span></span> |
| <span data-ttu-id="cd346-159">DW3000</span><span class="sxs-lookup"><span data-stu-id="cd346-159">DW3000</span></span> |<span data-ttu-id="cd346-160">32</span><span class="sxs-lookup"><span data-stu-id="cd346-160">32</span></span> |<span data-ttu-id="cd346-161">120</span><span class="sxs-lookup"><span data-stu-id="cd346-161">120</span></span> |
| <span data-ttu-id="cd346-162">DW6000</span><span class="sxs-lookup"><span data-stu-id="cd346-162">DW6000</span></span> |<span data-ttu-id="cd346-163">32</span><span class="sxs-lookup"><span data-stu-id="cd346-163">32</span></span> |<span data-ttu-id="cd346-164">240</span><span class="sxs-lookup"><span data-stu-id="cd346-164">240</span></span> |

<span data-ttu-id="cd346-165">Když je splněna jedna z těchto prahových hodnot, nové dotazy jsou zařazeny do fronty a jsou prováděny na základě ven first-in.</span><span class="sxs-lookup"><span data-stu-id="cd346-165">When one of these thresholds is met, new queries are queued and executed on a first-in, first-out basis.</span></span>  <span data-ttu-id="cd346-166">Dokončení dotazy a počet dotazů a sloty klesne pod omezení, jsou vydávány ve frontě dotazů.</span><span class="sxs-lookup"><span data-stu-id="cd346-166">As a queries finishes and the number of queries and slots falls below the limits, queued queries are released.</span></span> 

> [!NOTE]  
> <span data-ttu-id="cd346-167">*Vyberte* dotazy na spuštění výhradně na zobrazení dynamické správy (zobrazení dynamické správy) nebo zobrazení katalogu se neřídí žádné omezení souběžnosti.</span><span class="sxs-lookup"><span data-stu-id="cd346-167">*Select* queries executing exclusively on dynamic management views (DMVs) or catalog views are not governed by any of the concurrency limits.</span></span> <span data-ttu-id="cd346-168">Můžete monitorovat systém bez ohledu na počet dotazy na spuštění v něm.</span><span class="sxs-lookup"><span data-stu-id="cd346-168">You can monitor the system regardless of the number of queries executing on it.</span></span>
> 
> 

## <a name="resource-classes"></a><span data-ttu-id="cd346-169">Třídy prostředků</span><span class="sxs-lookup"><span data-stu-id="cd346-169">Resource classes</span></span>
<span data-ttu-id="cd346-170">Třídy prostředků pomáhají řídit přidělování paměti a cyklů procesoru pro dotaz.</span><span class="sxs-lookup"><span data-stu-id="cd346-170">Resource classes help you control memory allocation and CPU cycles given to a query.</span></span> <span data-ttu-id="cd346-171">Dva typy prostředků třídy můžete přiřadit uživatele ve formě role databáze.</span><span class="sxs-lookup"><span data-stu-id="cd346-171">You can assign two types of resource classes to a user in the form of database roles.</span></span> <span data-ttu-id="cd346-172">Oba typy prostředků třídy jsou následující:</span><span class="sxs-lookup"><span data-stu-id="cd346-172">The two types of resource classes are as follows:</span></span>
1. <span data-ttu-id="cd346-173">Dynamické třídy prostředků (**smallrc, mediumrc, largerc, xlargerc**) přidělit proměnné množství paměti v závislosti na aktuální DWU.</span><span class="sxs-lookup"><span data-stu-id="cd346-173">Dynamic Resource Classes (**smallrc, mediumrc, largerc, xlargerc**) allocate a variable amount of memory depending on the current DWU.</span></span> <span data-ttu-id="cd346-174">To znamená, že při změně měřítka na větší DWU, vaše dotazy automaticky získat více paměti.</span><span class="sxs-lookup"><span data-stu-id="cd346-174">This means that when you scale up to a larger DWU, your queries automatically get more memory.</span></span> 
2. <span data-ttu-id="cd346-175">Statické třídy prostředků (**staticrc10, staticrc20, staticrc30, staticrc40, staticrc50, staticrc60, staticrc70, staticrc80**) přidělit stejnou velikost paměti, bez ohledu na aktuální DWU (za předpokladu, že DWU sám sebe dostatek paměti).</span><span class="sxs-lookup"><span data-stu-id="cd346-175">Static Resource Classes (**staticrc10, staticrc20, staticrc30, staticrc40, staticrc50, staticrc60, staticrc70, staticrc80**) allocate the same amount of memory regardless of the current DWU (provided that the DWU itself has enough memory).</span></span> <span data-ttu-id="cd346-176">To znamená, že na větší počet Dwu, můžete spouštět další dotazy v každé třídě prostředků současně.</span><span class="sxs-lookup"><span data-stu-id="cd346-176">This means that on larger DWUs, you can run more queries in each resource class concurrently.</span></span>

<span data-ttu-id="cd346-177">Uživatelé v **smallrc** a **staticrc10** mají menší množství paměti a můžete využít výhod vyšší souběžnosti.</span><span class="sxs-lookup"><span data-stu-id="cd346-177">Users in **smallrc** and **staticrc10** are given a smaller amount of memory and can take advantage of higher concurrency.</span></span> <span data-ttu-id="cd346-178">Naproti tomu uživatelé přiřazeni k **xlargerc** nebo **staticrc80** mají velké množství paměti, a proto méně jejich dotazů můžou běžet souběžně.</span><span class="sxs-lookup"><span data-stu-id="cd346-178">In contrast, users assigned to **xlargerc** or **staticrc80** are given large amounts of memory, and therefore fewer of their queries can run concurrently.</span></span>

<span data-ttu-id="cd346-179">Ve výchozím nastavení, každý uživatel je členem Třída prostředků se malé, **smallrc**.</span><span class="sxs-lookup"><span data-stu-id="cd346-179">By default, each user is a member of the small resource class, **smallrc**.</span></span> <span data-ttu-id="cd346-180">Postup `sp_addrolemember` slouží ke zvýšení Třída prostředků, a `sp_droprolemember` se používá k snížit Třída prostředků.</span><span class="sxs-lookup"><span data-stu-id="cd346-180">The procedure `sp_addrolemember` is used to increase the resource class, and `sp_droprolemember` is used to decrease the resource class.</span></span> <span data-ttu-id="cd346-181">Tento příkaz by například zvýšit Třída prostředků loaduser k **largerc**:</span><span class="sxs-lookup"><span data-stu-id="cd346-181">For example, this command would increase the resource class of loaduser to **largerc**:</span></span>

```sql
EXEC sp_addrolemember 'largerc', 'loaduser'
```


### <a name="queries-that-do-not-honor-resource-classes"></a><span data-ttu-id="cd346-182">Dotazy, které nerespektují třídy prostředků</span><span class="sxs-lookup"><span data-stu-id="cd346-182">Queries that do not honor resource classes</span></span>

<span data-ttu-id="cd346-183">Existuje několik typů dotazy, které nejsou nijak přínosné větší přidělení paměti.</span><span class="sxs-lookup"><span data-stu-id="cd346-183">There are a few types of queries that do not benefit from a larger memory allocation.</span></span> <span data-ttu-id="cd346-184">Systém ignoruje jejich přidělení Třída prostředků a vždy spusťte tyto dotazy ve třídě malé prostředků místo.</span><span class="sxs-lookup"><span data-stu-id="cd346-184">The system ignores their resource class allocation and always run these queries in the small resource class instead.</span></span> <span data-ttu-id="cd346-185">Pokud tyto dotazy spustit vždy ve třídě malé prostředků, mohou spouštět při souběžnosti sloty jsou přetížena a jejich nebude využívat další slotů, než je potřeba.</span><span class="sxs-lookup"><span data-stu-id="cd346-185">If these queries always run in the small resource class, they can run when concurrency slots are under pressure and they won't consume more slots than needed.</span></span> <span data-ttu-id="cd346-186">V tématu [výjimky třídy prostředků](#query-exceptions-to-concurrency-limits) Další informace.</span><span class="sxs-lookup"><span data-stu-id="cd346-186">See [Resource class exceptions](#query-exceptions-to-concurrency-limits) for more information.</span></span>

## <a name="details-on-resource-class-assignment"></a><span data-ttu-id="cd346-187">Informace o přiřazení třídy prostředků</span><span class="sxs-lookup"><span data-stu-id="cd346-187">Details on resource class assignment</span></span>


<span data-ttu-id="cd346-188">Několik další podrobnosti o Třída prostředků:</span><span class="sxs-lookup"><span data-stu-id="cd346-188">A few more details on resource class:</span></span>

* <span data-ttu-id="cd346-189">*Příkaz ALTER role* oprávnění je potřeba změnit Třída prostředků uživatele.</span><span class="sxs-lookup"><span data-stu-id="cd346-189">*Alter role* permission is required to change the resource class of a user.</span></span>
* <span data-ttu-id="cd346-190">I když můžete přidat uživatele k jedné nebo několika vyšší třídy prostředků, prostředků dynamické třídy mají přednost před prostředků statické třídy.</span><span class="sxs-lookup"><span data-stu-id="cd346-190">Although you can add a user to one or more of the higher resource classes, dynamic resource classes take precedence over static resource classes.</span></span> <span data-ttu-id="cd346-191">To znamená pokud uživatel je přiřazen k obě **mediumrc**(dynamické) a **staticrc80**(statické), **mediumrc** je třída prostředků, která je dodržení.</span><span class="sxs-lookup"><span data-stu-id="cd346-191">That is, if a user is assigned to both **mediumrc**(dynamic) and **staticrc80**(static), **mediumrc** is the resource class that is honored.</span></span>
 * <span data-ttu-id="cd346-192">Když uživatel je přiřazen více než jedna třída prostředků v na konkrétní třídy typ prostředku (více než jeden dynamický prostředků nebo více než jeden statický prostředků třída), je dodržení nejvyšší Třída prostředků.</span><span class="sxs-lookup"><span data-stu-id="cd346-192">When a user is assigned to more than one resource class in a specific resource class type (more than one dynamic resource class or more than one static resource class), the highest resource class is honored.</span></span> <span data-ttu-id="cd346-193">To znamená pokud uživatel je přiřazen k mediumrc a largerc, je dodržení vyšší Třída prostředků (largerc).</span><span class="sxs-lookup"><span data-stu-id="cd346-193">That is, if a user is assigned to both mediumrc and largerc, the higher resource class (largerc) is honored.</span></span> <span data-ttu-id="cd346-194">A pokud je uživatel přiřazený k obě **staticrc20** a **statirc80**, **staticrc80** je přijmout.</span><span class="sxs-lookup"><span data-stu-id="cd346-194">And if a user is assigned to both **staticrc20** and **statirc80**, **staticrc80** is honored.</span></span>
* <span data-ttu-id="cd346-195">Třída prostředků uživatele pro správu systému, nelze změnit.</span><span class="sxs-lookup"><span data-stu-id="cd346-195">The resource class of the system administrative user cannot be changed.</span></span>

<span data-ttu-id="cd346-196">Podrobný příklad najdete v tématu [příklad třídy prostředků uživatele změna](#changing-user-resource-class-example).</span><span class="sxs-lookup"><span data-stu-id="cd346-196">For a detailed example, see [Changing user resource class example](#changing-user-resource-class-example).</span></span>

## <a name="memory-allocation"></a><span data-ttu-id="cd346-197">Přidělení paměti</span><span class="sxs-lookup"><span data-stu-id="cd346-197">Memory allocation</span></span>
<span data-ttu-id="cd346-198">Existují výhody a nevýhody na zvýšení Třída prostředků uživatele.</span><span class="sxs-lookup"><span data-stu-id="cd346-198">There are pros and cons to increasing a user's resource class.</span></span> <span data-ttu-id="cd346-199">Zvýšení Třída prostředků pro uživatele, poskytuje jejich dotazy přístup k více paměti, což může znamenat, že dotazy spouštět rychleji.</span><span class="sxs-lookup"><span data-stu-id="cd346-199">Increasing a resource class for a user, gives their queries access to more memory, which may mean queries execute faster.</span></span>  <span data-ttu-id="cd346-200">Vyšší třídy prostředků však také snížit počet souběžných dotazů, které můžou běžet.</span><span class="sxs-lookup"><span data-stu-id="cd346-200">However, higher resource classes also reduce the number of concurrent queries that can run.</span></span> <span data-ttu-id="cd346-201">Jedná se o kompromisu mezi přidělování velké množství paměti na jeden dotaz nebo povolením jiné dotazy, které je také třeba přidělení paměti spuštěny současně.</span><span class="sxs-lookup"><span data-stu-id="cd346-201">This is the trade-off between allocating large amounts of memory to a single query or allowing other queries, which also need memory allocations, to run concurrently.</span></span> <span data-ttu-id="cd346-202">Pokud jeden uživatel vysoké přidělení paměti pro dotaz, ostatní uživatelé nebudete mít přístup k této stejné paměti ke spuštění dotazu.</span><span class="sxs-lookup"><span data-stu-id="cd346-202">If one user is given high allocations of memory for a query, other users will not have access to that same memory to run a query.</span></span>

<span data-ttu-id="cd346-203">Následující tabulka mapuje je paměť přidělená pro každý distribuční třídou DWU a prostředků.</span><span class="sxs-lookup"><span data-stu-id="cd346-203">The following table maps the memory allocated to each distribution by DWU and resource class.</span></span>

### <a name="memory-allocations-per-distribution-for-dynamic-resource-classes-mb"></a><span data-ttu-id="cd346-204">Přidělení paměti za distribuci pro dynamické prostředků třídy (MB)</span><span class="sxs-lookup"><span data-stu-id="cd346-204">Memory allocations per distribution for dynamic resource classes (MB)</span></span>
| <span data-ttu-id="cd346-205">DWU</span><span class="sxs-lookup"><span data-stu-id="cd346-205">DWU</span></span> | <span data-ttu-id="cd346-206">smallrc</span><span class="sxs-lookup"><span data-stu-id="cd346-206">smallrc</span></span> | <span data-ttu-id="cd346-207">mediumrc</span><span class="sxs-lookup"><span data-stu-id="cd346-207">mediumrc</span></span> | <span data-ttu-id="cd346-208">largerc</span><span class="sxs-lookup"><span data-stu-id="cd346-208">largerc</span></span> | <span data-ttu-id="cd346-209">xlargerc</span><span class="sxs-lookup"><span data-stu-id="cd346-209">xlargerc</span></span> |
|:--- |:---:|:---:|:---:|:---:|
| <span data-ttu-id="cd346-210">OD DW100</span><span class="sxs-lookup"><span data-stu-id="cd346-210">DW100</span></span> |<span data-ttu-id="cd346-211">100</span><span class="sxs-lookup"><span data-stu-id="cd346-211">100</span></span> |<span data-ttu-id="cd346-212">100</span><span class="sxs-lookup"><span data-stu-id="cd346-212">100</span></span> |<span data-ttu-id="cd346-213">200</span><span class="sxs-lookup"><span data-stu-id="cd346-213">200</span></span> |<span data-ttu-id="cd346-214">400</span><span class="sxs-lookup"><span data-stu-id="cd346-214">400</span></span> |
| <span data-ttu-id="cd346-215">DW200</span><span class="sxs-lookup"><span data-stu-id="cd346-215">DW200</span></span> |<span data-ttu-id="cd346-216">100</span><span class="sxs-lookup"><span data-stu-id="cd346-216">100</span></span> |<span data-ttu-id="cd346-217">200</span><span class="sxs-lookup"><span data-stu-id="cd346-217">200</span></span> |<span data-ttu-id="cd346-218">400</span><span class="sxs-lookup"><span data-stu-id="cd346-218">400</span></span> |<span data-ttu-id="cd346-219">800</span><span class="sxs-lookup"><span data-stu-id="cd346-219">800</span></span> |
| <span data-ttu-id="cd346-220">DW300</span><span class="sxs-lookup"><span data-stu-id="cd346-220">DW300</span></span> |<span data-ttu-id="cd346-221">100</span><span class="sxs-lookup"><span data-stu-id="cd346-221">100</span></span> |<span data-ttu-id="cd346-222">200</span><span class="sxs-lookup"><span data-stu-id="cd346-222">200</span></span> |<span data-ttu-id="cd346-223">400</span><span class="sxs-lookup"><span data-stu-id="cd346-223">400</span></span> |<span data-ttu-id="cd346-224">800</span><span class="sxs-lookup"><span data-stu-id="cd346-224">800</span></span> |
| <span data-ttu-id="cd346-225">DW400</span><span class="sxs-lookup"><span data-stu-id="cd346-225">DW400</span></span> |<span data-ttu-id="cd346-226">100</span><span class="sxs-lookup"><span data-stu-id="cd346-226">100</span></span> |<span data-ttu-id="cd346-227">400</span><span class="sxs-lookup"><span data-stu-id="cd346-227">400</span></span> |<span data-ttu-id="cd346-228">800</span><span class="sxs-lookup"><span data-stu-id="cd346-228">800</span></span> |<span data-ttu-id="cd346-229">1,600</span><span class="sxs-lookup"><span data-stu-id="cd346-229">1,600</span></span> |
| <span data-ttu-id="cd346-230">DW500</span><span class="sxs-lookup"><span data-stu-id="cd346-230">DW500</span></span> |<span data-ttu-id="cd346-231">100</span><span class="sxs-lookup"><span data-stu-id="cd346-231">100</span></span> |<span data-ttu-id="cd346-232">400</span><span class="sxs-lookup"><span data-stu-id="cd346-232">400</span></span> |<span data-ttu-id="cd346-233">800</span><span class="sxs-lookup"><span data-stu-id="cd346-233">800</span></span> |<span data-ttu-id="cd346-234">1,600</span><span class="sxs-lookup"><span data-stu-id="cd346-234">1,600</span></span> |
| <span data-ttu-id="cd346-235">DW600</span><span class="sxs-lookup"><span data-stu-id="cd346-235">DW600</span></span> |<span data-ttu-id="cd346-236">100</span><span class="sxs-lookup"><span data-stu-id="cd346-236">100</span></span> |<span data-ttu-id="cd346-237">400</span><span class="sxs-lookup"><span data-stu-id="cd346-237">400</span></span> |<span data-ttu-id="cd346-238">800</span><span class="sxs-lookup"><span data-stu-id="cd346-238">800</span></span> |<span data-ttu-id="cd346-239">1,600</span><span class="sxs-lookup"><span data-stu-id="cd346-239">1,600</span></span> |
| <span data-ttu-id="cd346-240">DW1000</span><span class="sxs-lookup"><span data-stu-id="cd346-240">DW1000</span></span> |<span data-ttu-id="cd346-241">100</span><span class="sxs-lookup"><span data-stu-id="cd346-241">100</span></span> |<span data-ttu-id="cd346-242">800</span><span class="sxs-lookup"><span data-stu-id="cd346-242">800</span></span> |<span data-ttu-id="cd346-243">1,600</span><span class="sxs-lookup"><span data-stu-id="cd346-243">1,600</span></span> |<span data-ttu-id="cd346-244">3,200</span><span class="sxs-lookup"><span data-stu-id="cd346-244">3,200</span></span> |
| <span data-ttu-id="cd346-245">DW1200</span><span class="sxs-lookup"><span data-stu-id="cd346-245">DW1200</span></span> |<span data-ttu-id="cd346-246">100</span><span class="sxs-lookup"><span data-stu-id="cd346-246">100</span></span> |<span data-ttu-id="cd346-247">800</span><span class="sxs-lookup"><span data-stu-id="cd346-247">800</span></span> |<span data-ttu-id="cd346-248">1,600</span><span class="sxs-lookup"><span data-stu-id="cd346-248">1,600</span></span> |<span data-ttu-id="cd346-249">3,200</span><span class="sxs-lookup"><span data-stu-id="cd346-249">3,200</span></span> |
| <span data-ttu-id="cd346-250">DW1500</span><span class="sxs-lookup"><span data-stu-id="cd346-250">DW1500</span></span> |<span data-ttu-id="cd346-251">100</span><span class="sxs-lookup"><span data-stu-id="cd346-251">100</span></span> |<span data-ttu-id="cd346-252">800</span><span class="sxs-lookup"><span data-stu-id="cd346-252">800</span></span> |<span data-ttu-id="cd346-253">1,600</span><span class="sxs-lookup"><span data-stu-id="cd346-253">1,600</span></span> |<span data-ttu-id="cd346-254">3,200</span><span class="sxs-lookup"><span data-stu-id="cd346-254">3,200</span></span> |
| <span data-ttu-id="cd346-255">DW2000</span><span class="sxs-lookup"><span data-stu-id="cd346-255">DW2000</span></span> |<span data-ttu-id="cd346-256">100</span><span class="sxs-lookup"><span data-stu-id="cd346-256">100</span></span> |<span data-ttu-id="cd346-257">1,600</span><span class="sxs-lookup"><span data-stu-id="cd346-257">1,600</span></span> |<span data-ttu-id="cd346-258">3,200</span><span class="sxs-lookup"><span data-stu-id="cd346-258">3,200</span></span> |<span data-ttu-id="cd346-259">6,400</span><span class="sxs-lookup"><span data-stu-id="cd346-259">6,400</span></span> |
| <span data-ttu-id="cd346-260">DW3000</span><span class="sxs-lookup"><span data-stu-id="cd346-260">DW3000</span></span> |<span data-ttu-id="cd346-261">100</span><span class="sxs-lookup"><span data-stu-id="cd346-261">100</span></span> |<span data-ttu-id="cd346-262">1,600</span><span class="sxs-lookup"><span data-stu-id="cd346-262">1,600</span></span> |<span data-ttu-id="cd346-263">3,200</span><span class="sxs-lookup"><span data-stu-id="cd346-263">3,200</span></span> |<span data-ttu-id="cd346-264">6,400</span><span class="sxs-lookup"><span data-stu-id="cd346-264">6,400</span></span> |
| <span data-ttu-id="cd346-265">DW6000</span><span class="sxs-lookup"><span data-stu-id="cd346-265">DW6000</span></span> |<span data-ttu-id="cd346-266">100</span><span class="sxs-lookup"><span data-stu-id="cd346-266">100</span></span> |<span data-ttu-id="cd346-267">3,200</span><span class="sxs-lookup"><span data-stu-id="cd346-267">3,200</span></span> |<span data-ttu-id="cd346-268">6,400</span><span class="sxs-lookup"><span data-stu-id="cd346-268">6,400</span></span> |<span data-ttu-id="cd346-269">12,800</span><span class="sxs-lookup"><span data-stu-id="cd346-269">12,800</span></span> |

<span data-ttu-id="cd346-270">Následující tabulka mapuje je paměť přidělená pro každý distribuční DWU a třída statické prostředků.</span><span class="sxs-lookup"><span data-stu-id="cd346-270">The following table maps the memory allocated to each distribution by DWU and static resource class.</span></span> <span data-ttu-id="cd346-271">Všimněte si, že vyšší prostředků třídy mají jejich paměti snížen na respektovat globální omezení DWU.</span><span class="sxs-lookup"><span data-stu-id="cd346-271">Note that the higher resource classes have their memory reduced to honor the global DWU limits.</span></span>

### <a name="memory-allocations-per-distribution-for-static-resource-classes-mb"></a><span data-ttu-id="cd346-272">Přidělení paměti za distribuci pro statické prostředků třídy (MB)</span><span class="sxs-lookup"><span data-stu-id="cd346-272">Memory allocations per distribution for static resource classes (MB)</span></span>
| <span data-ttu-id="cd346-273">DWU</span><span class="sxs-lookup"><span data-stu-id="cd346-273">DWU</span></span> | <span data-ttu-id="cd346-274">staticrc10</span><span class="sxs-lookup"><span data-stu-id="cd346-274">staticrc10</span></span> | <span data-ttu-id="cd346-275">staticrc20</span><span class="sxs-lookup"><span data-stu-id="cd346-275">staticrc20</span></span> | <span data-ttu-id="cd346-276">staticrc30</span><span class="sxs-lookup"><span data-stu-id="cd346-276">staticrc30</span></span> | <span data-ttu-id="cd346-277">staticrc40</span><span class="sxs-lookup"><span data-stu-id="cd346-277">staticrc40</span></span> | <span data-ttu-id="cd346-278">staticrc50</span><span class="sxs-lookup"><span data-stu-id="cd346-278">staticrc50</span></span> | <span data-ttu-id="cd346-279">staticrc60</span><span class="sxs-lookup"><span data-stu-id="cd346-279">staticrc60</span></span> | <span data-ttu-id="cd346-280">staticrc70</span><span class="sxs-lookup"><span data-stu-id="cd346-280">staticrc70</span></span> | <span data-ttu-id="cd346-281">staticrc80</span><span class="sxs-lookup"><span data-stu-id="cd346-281">staticrc80</span></span> |
|:--- |:---:|:---:|:---:|:---:|:---:|:---:|:---:|:---:|
| <span data-ttu-id="cd346-282">OD DW100</span><span class="sxs-lookup"><span data-stu-id="cd346-282">DW100</span></span> |<span data-ttu-id="cd346-283">100</span><span class="sxs-lookup"><span data-stu-id="cd346-283">100</span></span> |<span data-ttu-id="cd346-284">200</span><span class="sxs-lookup"><span data-stu-id="cd346-284">200</span></span> |<span data-ttu-id="cd346-285">400</span><span class="sxs-lookup"><span data-stu-id="cd346-285">400</span></span> |<span data-ttu-id="cd346-286">400</span><span class="sxs-lookup"><span data-stu-id="cd346-286">400</span></span> |<span data-ttu-id="cd346-287">400</span><span class="sxs-lookup"><span data-stu-id="cd346-287">400</span></span> |<span data-ttu-id="cd346-288">400</span><span class="sxs-lookup"><span data-stu-id="cd346-288">400</span></span> |<span data-ttu-id="cd346-289">400</span><span class="sxs-lookup"><span data-stu-id="cd346-289">400</span></span> |<span data-ttu-id="cd346-290">400</span><span class="sxs-lookup"><span data-stu-id="cd346-290">400</span></span> |
| <span data-ttu-id="cd346-291">DW200</span><span class="sxs-lookup"><span data-stu-id="cd346-291">DW200</span></span> |<span data-ttu-id="cd346-292">100</span><span class="sxs-lookup"><span data-stu-id="cd346-292">100</span></span> |<span data-ttu-id="cd346-293">200</span><span class="sxs-lookup"><span data-stu-id="cd346-293">200</span></span> |<span data-ttu-id="cd346-294">400</span><span class="sxs-lookup"><span data-stu-id="cd346-294">400</span></span> |<span data-ttu-id="cd346-295">800</span><span class="sxs-lookup"><span data-stu-id="cd346-295">800</span></span> |<span data-ttu-id="cd346-296">800</span><span class="sxs-lookup"><span data-stu-id="cd346-296">800</span></span> |<span data-ttu-id="cd346-297">800</span><span class="sxs-lookup"><span data-stu-id="cd346-297">800</span></span> |<span data-ttu-id="cd346-298">800</span><span class="sxs-lookup"><span data-stu-id="cd346-298">800</span></span> |<span data-ttu-id="cd346-299">800</span><span class="sxs-lookup"><span data-stu-id="cd346-299">800</span></span> |
| <span data-ttu-id="cd346-300">DW300</span><span class="sxs-lookup"><span data-stu-id="cd346-300">DW300</span></span> |<span data-ttu-id="cd346-301">100</span><span class="sxs-lookup"><span data-stu-id="cd346-301">100</span></span> |<span data-ttu-id="cd346-302">200</span><span class="sxs-lookup"><span data-stu-id="cd346-302">200</span></span> |<span data-ttu-id="cd346-303">400</span><span class="sxs-lookup"><span data-stu-id="cd346-303">400</span></span> |<span data-ttu-id="cd346-304">800</span><span class="sxs-lookup"><span data-stu-id="cd346-304">800</span></span> |<span data-ttu-id="cd346-305">800</span><span class="sxs-lookup"><span data-stu-id="cd346-305">800</span></span> |<span data-ttu-id="cd346-306">800</span><span class="sxs-lookup"><span data-stu-id="cd346-306">800</span></span> |<span data-ttu-id="cd346-307">800</span><span class="sxs-lookup"><span data-stu-id="cd346-307">800</span></span> |<span data-ttu-id="cd346-308">800</span><span class="sxs-lookup"><span data-stu-id="cd346-308">800</span></span> |
| <span data-ttu-id="cd346-309">DW400</span><span class="sxs-lookup"><span data-stu-id="cd346-309">DW400</span></span> |<span data-ttu-id="cd346-310">100</span><span class="sxs-lookup"><span data-stu-id="cd346-310">100</span></span> |<span data-ttu-id="cd346-311">200</span><span class="sxs-lookup"><span data-stu-id="cd346-311">200</span></span> |<span data-ttu-id="cd346-312">400</span><span class="sxs-lookup"><span data-stu-id="cd346-312">400</span></span> |<span data-ttu-id="cd346-313">800</span><span class="sxs-lookup"><span data-stu-id="cd346-313">800</span></span> |<span data-ttu-id="cd346-314">1,600</span><span class="sxs-lookup"><span data-stu-id="cd346-314">1,600</span></span> |<span data-ttu-id="cd346-315">1,600</span><span class="sxs-lookup"><span data-stu-id="cd346-315">1,600</span></span> |<span data-ttu-id="cd346-316">1,600</span><span class="sxs-lookup"><span data-stu-id="cd346-316">1,600</span></span> |<span data-ttu-id="cd346-317">1,600</span><span class="sxs-lookup"><span data-stu-id="cd346-317">1,600</span></span> |
| <span data-ttu-id="cd346-318">DW500</span><span class="sxs-lookup"><span data-stu-id="cd346-318">DW500</span></span> |<span data-ttu-id="cd346-319">100</span><span class="sxs-lookup"><span data-stu-id="cd346-319">100</span></span> |<span data-ttu-id="cd346-320">200</span><span class="sxs-lookup"><span data-stu-id="cd346-320">200</span></span> |<span data-ttu-id="cd346-321">400</span><span class="sxs-lookup"><span data-stu-id="cd346-321">400</span></span> |<span data-ttu-id="cd346-322">800</span><span class="sxs-lookup"><span data-stu-id="cd346-322">800</span></span> |<span data-ttu-id="cd346-323">1,600</span><span class="sxs-lookup"><span data-stu-id="cd346-323">1,600</span></span> |<span data-ttu-id="cd346-324">1,600</span><span class="sxs-lookup"><span data-stu-id="cd346-324">1,600</span></span> |<span data-ttu-id="cd346-325">1,600</span><span class="sxs-lookup"><span data-stu-id="cd346-325">1,600</span></span> |<span data-ttu-id="cd346-326">1,600</span><span class="sxs-lookup"><span data-stu-id="cd346-326">1,600</span></span> |
| <span data-ttu-id="cd346-327">DW600</span><span class="sxs-lookup"><span data-stu-id="cd346-327">DW600</span></span> |<span data-ttu-id="cd346-328">100</span><span class="sxs-lookup"><span data-stu-id="cd346-328">100</span></span> |<span data-ttu-id="cd346-329">200</span><span class="sxs-lookup"><span data-stu-id="cd346-329">200</span></span> |<span data-ttu-id="cd346-330">400</span><span class="sxs-lookup"><span data-stu-id="cd346-330">400</span></span> |<span data-ttu-id="cd346-331">800</span><span class="sxs-lookup"><span data-stu-id="cd346-331">800</span></span> |<span data-ttu-id="cd346-332">1,600</span><span class="sxs-lookup"><span data-stu-id="cd346-332">1,600</span></span> |<span data-ttu-id="cd346-333">1,600</span><span class="sxs-lookup"><span data-stu-id="cd346-333">1,600</span></span> |<span data-ttu-id="cd346-334">1,600</span><span class="sxs-lookup"><span data-stu-id="cd346-334">1,600</span></span> |<span data-ttu-id="cd346-335">1,600</span><span class="sxs-lookup"><span data-stu-id="cd346-335">1,600</span></span> |
| <span data-ttu-id="cd346-336">DW1000</span><span class="sxs-lookup"><span data-stu-id="cd346-336">DW1000</span></span> |<span data-ttu-id="cd346-337">100</span><span class="sxs-lookup"><span data-stu-id="cd346-337">100</span></span> |<span data-ttu-id="cd346-338">200</span><span class="sxs-lookup"><span data-stu-id="cd346-338">200</span></span> |<span data-ttu-id="cd346-339">400</span><span class="sxs-lookup"><span data-stu-id="cd346-339">400</span></span> |<span data-ttu-id="cd346-340">800</span><span class="sxs-lookup"><span data-stu-id="cd346-340">800</span></span> |<span data-ttu-id="cd346-341">1,600</span><span class="sxs-lookup"><span data-stu-id="cd346-341">1,600</span></span> |<span data-ttu-id="cd346-342">3,200</span><span class="sxs-lookup"><span data-stu-id="cd346-342">3,200</span></span> |<span data-ttu-id="cd346-343">3,200</span><span class="sxs-lookup"><span data-stu-id="cd346-343">3,200</span></span> |<span data-ttu-id="cd346-344">3,200</span><span class="sxs-lookup"><span data-stu-id="cd346-344">3,200</span></span> |
| <span data-ttu-id="cd346-345">DW1200</span><span class="sxs-lookup"><span data-stu-id="cd346-345">DW1200</span></span> |<span data-ttu-id="cd346-346">100</span><span class="sxs-lookup"><span data-stu-id="cd346-346">100</span></span> |<span data-ttu-id="cd346-347">200</span><span class="sxs-lookup"><span data-stu-id="cd346-347">200</span></span> |<span data-ttu-id="cd346-348">400</span><span class="sxs-lookup"><span data-stu-id="cd346-348">400</span></span> |<span data-ttu-id="cd346-349">800</span><span class="sxs-lookup"><span data-stu-id="cd346-349">800</span></span> |<span data-ttu-id="cd346-350">1,600</span><span class="sxs-lookup"><span data-stu-id="cd346-350">1,600</span></span> |<span data-ttu-id="cd346-351">3,200</span><span class="sxs-lookup"><span data-stu-id="cd346-351">3,200</span></span> |<span data-ttu-id="cd346-352">3,200</span><span class="sxs-lookup"><span data-stu-id="cd346-352">3,200</span></span> |<span data-ttu-id="cd346-353">3,200</span><span class="sxs-lookup"><span data-stu-id="cd346-353">3,200</span></span> |
| <span data-ttu-id="cd346-354">DW1500</span><span class="sxs-lookup"><span data-stu-id="cd346-354">DW1500</span></span> |<span data-ttu-id="cd346-355">100</span><span class="sxs-lookup"><span data-stu-id="cd346-355">100</span></span> |<span data-ttu-id="cd346-356">200</span><span class="sxs-lookup"><span data-stu-id="cd346-356">200</span></span> |<span data-ttu-id="cd346-357">400</span><span class="sxs-lookup"><span data-stu-id="cd346-357">400</span></span> |<span data-ttu-id="cd346-358">800</span><span class="sxs-lookup"><span data-stu-id="cd346-358">800</span></span> |<span data-ttu-id="cd346-359">1,600</span><span class="sxs-lookup"><span data-stu-id="cd346-359">1,600</span></span> |<span data-ttu-id="cd346-360">3,200</span><span class="sxs-lookup"><span data-stu-id="cd346-360">3,200</span></span> |<span data-ttu-id="cd346-361">3,200</span><span class="sxs-lookup"><span data-stu-id="cd346-361">3,200</span></span> |<span data-ttu-id="cd346-362">3,200</span><span class="sxs-lookup"><span data-stu-id="cd346-362">3,200</span></span> |
| <span data-ttu-id="cd346-363">DW2000</span><span class="sxs-lookup"><span data-stu-id="cd346-363">DW2000</span></span> |<span data-ttu-id="cd346-364">100</span><span class="sxs-lookup"><span data-stu-id="cd346-364">100</span></span> |<span data-ttu-id="cd346-365">200</span><span class="sxs-lookup"><span data-stu-id="cd346-365">200</span></span> |<span data-ttu-id="cd346-366">400</span><span class="sxs-lookup"><span data-stu-id="cd346-366">400</span></span> |<span data-ttu-id="cd346-367">800</span><span class="sxs-lookup"><span data-stu-id="cd346-367">800</span></span> |<span data-ttu-id="cd346-368">1,600</span><span class="sxs-lookup"><span data-stu-id="cd346-368">1,600</span></span> |<span data-ttu-id="cd346-369">3,200</span><span class="sxs-lookup"><span data-stu-id="cd346-369">3,200</span></span> |<span data-ttu-id="cd346-370">6,400</span><span class="sxs-lookup"><span data-stu-id="cd346-370">6,400</span></span> |<span data-ttu-id="cd346-371">6,400</span><span class="sxs-lookup"><span data-stu-id="cd346-371">6,400</span></span> |
| <span data-ttu-id="cd346-372">DW3000</span><span class="sxs-lookup"><span data-stu-id="cd346-372">DW3000</span></span> |<span data-ttu-id="cd346-373">100</span><span class="sxs-lookup"><span data-stu-id="cd346-373">100</span></span> |<span data-ttu-id="cd346-374">200</span><span class="sxs-lookup"><span data-stu-id="cd346-374">200</span></span> |<span data-ttu-id="cd346-375">400</span><span class="sxs-lookup"><span data-stu-id="cd346-375">400</span></span> |<span data-ttu-id="cd346-376">800</span><span class="sxs-lookup"><span data-stu-id="cd346-376">800</span></span> |<span data-ttu-id="cd346-377">1,600</span><span class="sxs-lookup"><span data-stu-id="cd346-377">1,600</span></span> |<span data-ttu-id="cd346-378">3,200</span><span class="sxs-lookup"><span data-stu-id="cd346-378">3,200</span></span> |<span data-ttu-id="cd346-379">6,400</span><span class="sxs-lookup"><span data-stu-id="cd346-379">6,400</span></span> |<span data-ttu-id="cd346-380">6,400</span><span class="sxs-lookup"><span data-stu-id="cd346-380">6,400</span></span> |
| <span data-ttu-id="cd346-381">DW6000</span><span class="sxs-lookup"><span data-stu-id="cd346-381">DW6000</span></span> |<span data-ttu-id="cd346-382">100</span><span class="sxs-lookup"><span data-stu-id="cd346-382">100</span></span> |<span data-ttu-id="cd346-383">200</span><span class="sxs-lookup"><span data-stu-id="cd346-383">200</span></span> |<span data-ttu-id="cd346-384">400</span><span class="sxs-lookup"><span data-stu-id="cd346-384">400</span></span> |<span data-ttu-id="cd346-385">800</span><span class="sxs-lookup"><span data-stu-id="cd346-385">800</span></span> |<span data-ttu-id="cd346-386">1,600</span><span class="sxs-lookup"><span data-stu-id="cd346-386">1,600</span></span> |<span data-ttu-id="cd346-387">3,200</span><span class="sxs-lookup"><span data-stu-id="cd346-387">3,200</span></span> |<span data-ttu-id="cd346-388">6,400</span><span class="sxs-lookup"><span data-stu-id="cd346-388">6,400</span></span> |<span data-ttu-id="cd346-389">12,800</span><span class="sxs-lookup"><span data-stu-id="cd346-389">12,800</span></span> |

<span data-ttu-id="cd346-390">Z předchozí tabulky, můžete uvidíte, že dotaz spuštěný na DW2000 v **xlargerc** Třída prostředků bude mít přístup k 6 400 MB paměti v rámci každé 60 distribuované databáze.</span><span class="sxs-lookup"><span data-stu-id="cd346-390">From the preceding table, you can see that a query running on a DW2000 in the **xlargerc** resource class would have access to 6,400 MB of memory within each of the 60 distributed databases.</span></span>  <span data-ttu-id="cd346-391">V SQL Data Warehouse jsou 60 distribuce.</span><span class="sxs-lookup"><span data-stu-id="cd346-391">In SQL Data Warehouse, there are 60 distributions.</span></span> <span data-ttu-id="cd346-392">Vypočítat celkové paměti přidělení pro dotaz v třídě daný prostředek, proto by výše uvedené hodnoty násobí hodnotou 60.</span><span class="sxs-lookup"><span data-stu-id="cd346-392">Therefore, to calculate the total memory allocation for a query in a given resource class, the above values should be multiplied by 60.</span></span>

### <a name="memory-allocations-system-wide-gb"></a><span data-ttu-id="cd346-393">Paměť přidělení systémové (GB)</span><span class="sxs-lookup"><span data-stu-id="cd346-393">Memory allocations system-wide (GB)</span></span>
| <span data-ttu-id="cd346-394">DWU</span><span class="sxs-lookup"><span data-stu-id="cd346-394">DWU</span></span> | <span data-ttu-id="cd346-395">smallrc</span><span class="sxs-lookup"><span data-stu-id="cd346-395">smallrc</span></span> | <span data-ttu-id="cd346-396">mediumrc</span><span class="sxs-lookup"><span data-stu-id="cd346-396">mediumrc</span></span> | <span data-ttu-id="cd346-397">largerc</span><span class="sxs-lookup"><span data-stu-id="cd346-397">largerc</span></span> | <span data-ttu-id="cd346-398">xlargerc</span><span class="sxs-lookup"><span data-stu-id="cd346-398">xlargerc</span></span> |
|:--- |:---:|:---:|:---:|:---:|
| <span data-ttu-id="cd346-399">OD DW100</span><span class="sxs-lookup"><span data-stu-id="cd346-399">DW100</span></span> |<span data-ttu-id="cd346-400">6</span><span class="sxs-lookup"><span data-stu-id="cd346-400">6</span></span> |<span data-ttu-id="cd346-401">6</span><span class="sxs-lookup"><span data-stu-id="cd346-401">6</span></span> |<span data-ttu-id="cd346-402">12</span><span class="sxs-lookup"><span data-stu-id="cd346-402">12</span></span> |<span data-ttu-id="cd346-403">23</span><span class="sxs-lookup"><span data-stu-id="cd346-403">23</span></span> |
| <span data-ttu-id="cd346-404">DW200</span><span class="sxs-lookup"><span data-stu-id="cd346-404">DW200</span></span> |<span data-ttu-id="cd346-405">6</span><span class="sxs-lookup"><span data-stu-id="cd346-405">6</span></span> |<span data-ttu-id="cd346-406">12</span><span class="sxs-lookup"><span data-stu-id="cd346-406">12</span></span> |<span data-ttu-id="cd346-407">23</span><span class="sxs-lookup"><span data-stu-id="cd346-407">23</span></span> |<span data-ttu-id="cd346-408">47</span><span class="sxs-lookup"><span data-stu-id="cd346-408">47</span></span> |
| <span data-ttu-id="cd346-409">DW300</span><span class="sxs-lookup"><span data-stu-id="cd346-409">DW300</span></span> |<span data-ttu-id="cd346-410">6</span><span class="sxs-lookup"><span data-stu-id="cd346-410">6</span></span> |<span data-ttu-id="cd346-411">12</span><span class="sxs-lookup"><span data-stu-id="cd346-411">12</span></span> |<span data-ttu-id="cd346-412">23</span><span class="sxs-lookup"><span data-stu-id="cd346-412">23</span></span> |<span data-ttu-id="cd346-413">47</span><span class="sxs-lookup"><span data-stu-id="cd346-413">47</span></span> |
| <span data-ttu-id="cd346-414">DW400</span><span class="sxs-lookup"><span data-stu-id="cd346-414">DW400</span></span> |<span data-ttu-id="cd346-415">6</span><span class="sxs-lookup"><span data-stu-id="cd346-415">6</span></span> |<span data-ttu-id="cd346-416">23</span><span class="sxs-lookup"><span data-stu-id="cd346-416">23</span></span> |<span data-ttu-id="cd346-417">47</span><span class="sxs-lookup"><span data-stu-id="cd346-417">47</span></span> |<span data-ttu-id="cd346-418">94</span><span class="sxs-lookup"><span data-stu-id="cd346-418">94</span></span> |
| <span data-ttu-id="cd346-419">DW500</span><span class="sxs-lookup"><span data-stu-id="cd346-419">DW500</span></span> |<span data-ttu-id="cd346-420">6</span><span class="sxs-lookup"><span data-stu-id="cd346-420">6</span></span> |<span data-ttu-id="cd346-421">23</span><span class="sxs-lookup"><span data-stu-id="cd346-421">23</span></span> |<span data-ttu-id="cd346-422">47</span><span class="sxs-lookup"><span data-stu-id="cd346-422">47</span></span> |<span data-ttu-id="cd346-423">94</span><span class="sxs-lookup"><span data-stu-id="cd346-423">94</span></span> |
| <span data-ttu-id="cd346-424">DW600</span><span class="sxs-lookup"><span data-stu-id="cd346-424">DW600</span></span> |<span data-ttu-id="cd346-425">6</span><span class="sxs-lookup"><span data-stu-id="cd346-425">6</span></span> |<span data-ttu-id="cd346-426">23</span><span class="sxs-lookup"><span data-stu-id="cd346-426">23</span></span> |<span data-ttu-id="cd346-427">47</span><span class="sxs-lookup"><span data-stu-id="cd346-427">47</span></span> |<span data-ttu-id="cd346-428">94</span><span class="sxs-lookup"><span data-stu-id="cd346-428">94</span></span> |
| <span data-ttu-id="cd346-429">DW1000</span><span class="sxs-lookup"><span data-stu-id="cd346-429">DW1000</span></span> |<span data-ttu-id="cd346-430">6</span><span class="sxs-lookup"><span data-stu-id="cd346-430">6</span></span> |<span data-ttu-id="cd346-431">47</span><span class="sxs-lookup"><span data-stu-id="cd346-431">47</span></span> |<span data-ttu-id="cd346-432">94</span><span class="sxs-lookup"><span data-stu-id="cd346-432">94</span></span> |<span data-ttu-id="cd346-433">188</span><span class="sxs-lookup"><span data-stu-id="cd346-433">188</span></span> |
| <span data-ttu-id="cd346-434">DW1200</span><span class="sxs-lookup"><span data-stu-id="cd346-434">DW1200</span></span> |<span data-ttu-id="cd346-435">6</span><span class="sxs-lookup"><span data-stu-id="cd346-435">6</span></span> |<span data-ttu-id="cd346-436">47</span><span class="sxs-lookup"><span data-stu-id="cd346-436">47</span></span> |<span data-ttu-id="cd346-437">94</span><span class="sxs-lookup"><span data-stu-id="cd346-437">94</span></span> |<span data-ttu-id="cd346-438">188</span><span class="sxs-lookup"><span data-stu-id="cd346-438">188</span></span> |
| <span data-ttu-id="cd346-439">DW1500</span><span class="sxs-lookup"><span data-stu-id="cd346-439">DW1500</span></span> |<span data-ttu-id="cd346-440">6</span><span class="sxs-lookup"><span data-stu-id="cd346-440">6</span></span> |<span data-ttu-id="cd346-441">47</span><span class="sxs-lookup"><span data-stu-id="cd346-441">47</span></span> |<span data-ttu-id="cd346-442">94</span><span class="sxs-lookup"><span data-stu-id="cd346-442">94</span></span> |<span data-ttu-id="cd346-443">188</span><span class="sxs-lookup"><span data-stu-id="cd346-443">188</span></span> |
| <span data-ttu-id="cd346-444">DW2000</span><span class="sxs-lookup"><span data-stu-id="cd346-444">DW2000</span></span> |<span data-ttu-id="cd346-445">6</span><span class="sxs-lookup"><span data-stu-id="cd346-445">6</span></span> |<span data-ttu-id="cd346-446">94</span><span class="sxs-lookup"><span data-stu-id="cd346-446">94</span></span> |<span data-ttu-id="cd346-447">188</span><span class="sxs-lookup"><span data-stu-id="cd346-447">188</span></span> |<span data-ttu-id="cd346-448">375</span><span class="sxs-lookup"><span data-stu-id="cd346-448">375</span></span> |
| <span data-ttu-id="cd346-449">DW3000</span><span class="sxs-lookup"><span data-stu-id="cd346-449">DW3000</span></span> |<span data-ttu-id="cd346-450">6</span><span class="sxs-lookup"><span data-stu-id="cd346-450">6</span></span> |<span data-ttu-id="cd346-451">94</span><span class="sxs-lookup"><span data-stu-id="cd346-451">94</span></span> |<span data-ttu-id="cd346-452">188</span><span class="sxs-lookup"><span data-stu-id="cd346-452">188</span></span> |<span data-ttu-id="cd346-453">375</span><span class="sxs-lookup"><span data-stu-id="cd346-453">375</span></span> |
| <span data-ttu-id="cd346-454">DW6000</span><span class="sxs-lookup"><span data-stu-id="cd346-454">DW6000</span></span> |<span data-ttu-id="cd346-455">6</span><span class="sxs-lookup"><span data-stu-id="cd346-455">6</span></span> |<span data-ttu-id="cd346-456">188</span><span class="sxs-lookup"><span data-stu-id="cd346-456">188</span></span> |<span data-ttu-id="cd346-457">375</span><span class="sxs-lookup"><span data-stu-id="cd346-457">375</span></span> |<span data-ttu-id="cd346-458">750</span><span class="sxs-lookup"><span data-stu-id="cd346-458">750</span></span> |

<span data-ttu-id="cd346-459">Z této tabulky přidělených systémové paměti, uvidíte, že dotaz spuštěný na DW2000 ve třídě xlargerc prostředků je přidělen celkem 375 GB paměti (6 400 MB * 60 distribuce / 1 024 převést GB) přes celého SQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="cd346-459">From this table of system-wide memory allocations, you can see that a query running on a DW2000 in the xlargerc resource class is allocated a total of 375 GB of memory (6,400 MB * 60 distributions / 1,024 to convert to GB) over the entirety of your SQL Data Warehouse.</span></span>

<span data-ttu-id="cd346-460">Stejný výpočet platí pro třídy statické prostředků.</span><span class="sxs-lookup"><span data-stu-id="cd346-460">The same calculation applies to static resource classes.</span></span>
 
## <a name="concurrency-slot-consumption"></a><span data-ttu-id="cd346-461">Spotřeba slotu souběžnosti</span><span class="sxs-lookup"><span data-stu-id="cd346-461">Concurrency slot consumption</span></span>  
<span data-ttu-id="cd346-462">SQL Data Warehouse uděluje více paměti na dotazy, které jsou spuštěné v vyšší třídy prostředků.</span><span class="sxs-lookup"><span data-stu-id="cd346-462">SQL Data Warehouse grants more memory to queries running in higher resource classes.</span></span> <span data-ttu-id="cd346-463">Velikost paměti je pevné prostředků.</span><span class="sxs-lookup"><span data-stu-id="cd346-463">Memory is a fixed resource.</span></span>  <span data-ttu-id="cd346-464">Proto je další paměť přidělená na dotaz, méně souběžných dotazů můžete spustit.</span><span class="sxs-lookup"><span data-stu-id="cd346-464">Therefore, the more memory allocated per query, the fewer concurrent queries can execute.</span></span> <span data-ttu-id="cd346-465">V následující tabulce opětovně uvádí, že všechny předchozí konceptů v rámci jednoho zobrazení, která zobrazuje počet souběžnosti sloty dostupné podle DWU a sloty spotřebovávají každá třída prostředků.</span><span class="sxs-lookup"><span data-stu-id="cd346-465">The following table reiterates all of the previous concepts in a single view that shows the number of concurrency slots available by DWU and the slots consumed by each resource class.</span></span>  

### <a name="allocation-and-consumption-of-concurrency-slots-for-dynamic-resource-classes"></a><span data-ttu-id="cd346-466">Přidělení a využití souběžnosti přihrádky pro dynamické prostředků třídy</span><span class="sxs-lookup"><span data-stu-id="cd346-466">Allocation and consumption of concurrency slots for dynamic resource classes</span></span>  
| <span data-ttu-id="cd346-467">DWU</span><span class="sxs-lookup"><span data-stu-id="cd346-467">DWU</span></span> | <span data-ttu-id="cd346-468">Maximální počet souběžných dotazů</span><span class="sxs-lookup"><span data-stu-id="cd346-468">Maximum concurrent queries</span></span> | <span data-ttu-id="cd346-469">Přidělené sloty souběžnosti</span><span class="sxs-lookup"><span data-stu-id="cd346-469">Concurrency slots allocated</span></span> | <span data-ttu-id="cd346-470">Sloty používané smallrc</span><span class="sxs-lookup"><span data-stu-id="cd346-470">Slots used by smallrc</span></span> | <span data-ttu-id="cd346-471">Sloty používané mediumrc</span><span class="sxs-lookup"><span data-stu-id="cd346-471">Slots used by mediumrc</span></span> | <span data-ttu-id="cd346-472">Sloty používané largerc</span><span class="sxs-lookup"><span data-stu-id="cd346-472">Slots used by largerc</span></span> | <span data-ttu-id="cd346-473">Sloty používané xlargerc</span><span class="sxs-lookup"><span data-stu-id="cd346-473">Slots used by xlargerc</span></span> |
|:--- |:---:|:---:|:---:|:---:|:---:|:---:|
| <span data-ttu-id="cd346-474">OD DW100</span><span class="sxs-lookup"><span data-stu-id="cd346-474">DW100</span></span> |<span data-ttu-id="cd346-475">4</span><span class="sxs-lookup"><span data-stu-id="cd346-475">4</span></span> |<span data-ttu-id="cd346-476">4</span><span class="sxs-lookup"><span data-stu-id="cd346-476">4</span></span> |<span data-ttu-id="cd346-477">1</span><span class="sxs-lookup"><span data-stu-id="cd346-477">1</span></span> |<span data-ttu-id="cd346-478">1</span><span class="sxs-lookup"><span data-stu-id="cd346-478">1</span></span> |<span data-ttu-id="cd346-479">2</span><span class="sxs-lookup"><span data-stu-id="cd346-479">2</span></span> |<span data-ttu-id="cd346-480">4</span><span class="sxs-lookup"><span data-stu-id="cd346-480">4</span></span> |
| <span data-ttu-id="cd346-481">DW200</span><span class="sxs-lookup"><span data-stu-id="cd346-481">DW200</span></span> |<span data-ttu-id="cd346-482">8</span><span class="sxs-lookup"><span data-stu-id="cd346-482">8</span></span> |<span data-ttu-id="cd346-483">8</span><span class="sxs-lookup"><span data-stu-id="cd346-483">8</span></span> |<span data-ttu-id="cd346-484">1</span><span class="sxs-lookup"><span data-stu-id="cd346-484">1</span></span> |<span data-ttu-id="cd346-485">2</span><span class="sxs-lookup"><span data-stu-id="cd346-485">2</span></span> |<span data-ttu-id="cd346-486">4</span><span class="sxs-lookup"><span data-stu-id="cd346-486">4</span></span> |<span data-ttu-id="cd346-487">8</span><span class="sxs-lookup"><span data-stu-id="cd346-487">8</span></span> |
| <span data-ttu-id="cd346-488">DW300</span><span class="sxs-lookup"><span data-stu-id="cd346-488">DW300</span></span> |<span data-ttu-id="cd346-489">12</span><span class="sxs-lookup"><span data-stu-id="cd346-489">12</span></span> |<span data-ttu-id="cd346-490">12</span><span class="sxs-lookup"><span data-stu-id="cd346-490">12</span></span> |<span data-ttu-id="cd346-491">1</span><span class="sxs-lookup"><span data-stu-id="cd346-491">1</span></span> |<span data-ttu-id="cd346-492">2</span><span class="sxs-lookup"><span data-stu-id="cd346-492">2</span></span> |<span data-ttu-id="cd346-493">4</span><span class="sxs-lookup"><span data-stu-id="cd346-493">4</span></span> |<span data-ttu-id="cd346-494">8</span><span class="sxs-lookup"><span data-stu-id="cd346-494">8</span></span> |
| <span data-ttu-id="cd346-495">DW400</span><span class="sxs-lookup"><span data-stu-id="cd346-495">DW400</span></span> |<span data-ttu-id="cd346-496">16</span><span class="sxs-lookup"><span data-stu-id="cd346-496">16</span></span> |<span data-ttu-id="cd346-497">16</span><span class="sxs-lookup"><span data-stu-id="cd346-497">16</span></span> |<span data-ttu-id="cd346-498">1</span><span class="sxs-lookup"><span data-stu-id="cd346-498">1</span></span> |<span data-ttu-id="cd346-499">4</span><span class="sxs-lookup"><span data-stu-id="cd346-499">4</span></span> |<span data-ttu-id="cd346-500">8</span><span class="sxs-lookup"><span data-stu-id="cd346-500">8</span></span> |<span data-ttu-id="cd346-501">16</span><span class="sxs-lookup"><span data-stu-id="cd346-501">16</span></span> |
| <span data-ttu-id="cd346-502">DW500</span><span class="sxs-lookup"><span data-stu-id="cd346-502">DW500</span></span> |<span data-ttu-id="cd346-503">20</span><span class="sxs-lookup"><span data-stu-id="cd346-503">20</span></span> |<span data-ttu-id="cd346-504">20</span><span class="sxs-lookup"><span data-stu-id="cd346-504">20</span></span> |<span data-ttu-id="cd346-505">1</span><span class="sxs-lookup"><span data-stu-id="cd346-505">1</span></span> |<span data-ttu-id="cd346-506">4</span><span class="sxs-lookup"><span data-stu-id="cd346-506">4</span></span> |<span data-ttu-id="cd346-507">8</span><span class="sxs-lookup"><span data-stu-id="cd346-507">8</span></span> |<span data-ttu-id="cd346-508">16</span><span class="sxs-lookup"><span data-stu-id="cd346-508">16</span></span> |
| <span data-ttu-id="cd346-509">DW600</span><span class="sxs-lookup"><span data-stu-id="cd346-509">DW600</span></span> |<span data-ttu-id="cd346-510">24</span><span class="sxs-lookup"><span data-stu-id="cd346-510">24</span></span> |<span data-ttu-id="cd346-511">24</span><span class="sxs-lookup"><span data-stu-id="cd346-511">24</span></span> |<span data-ttu-id="cd346-512">1</span><span class="sxs-lookup"><span data-stu-id="cd346-512">1</span></span> |<span data-ttu-id="cd346-513">4</span><span class="sxs-lookup"><span data-stu-id="cd346-513">4</span></span> |<span data-ttu-id="cd346-514">8</span><span class="sxs-lookup"><span data-stu-id="cd346-514">8</span></span> |<span data-ttu-id="cd346-515">16</span><span class="sxs-lookup"><span data-stu-id="cd346-515">16</span></span> |
| <span data-ttu-id="cd346-516">DW1000</span><span class="sxs-lookup"><span data-stu-id="cd346-516">DW1000</span></span> |<span data-ttu-id="cd346-517">32</span><span class="sxs-lookup"><span data-stu-id="cd346-517">32</span></span> |<span data-ttu-id="cd346-518">40</span><span class="sxs-lookup"><span data-stu-id="cd346-518">40</span></span> |<span data-ttu-id="cd346-519">1</span><span class="sxs-lookup"><span data-stu-id="cd346-519">1</span></span> |<span data-ttu-id="cd346-520">8</span><span class="sxs-lookup"><span data-stu-id="cd346-520">8</span></span> |<span data-ttu-id="cd346-521">16</span><span class="sxs-lookup"><span data-stu-id="cd346-521">16</span></span> |<span data-ttu-id="cd346-522">32</span><span class="sxs-lookup"><span data-stu-id="cd346-522">32</span></span> |
| <span data-ttu-id="cd346-523">DW1200</span><span class="sxs-lookup"><span data-stu-id="cd346-523">DW1200</span></span> |<span data-ttu-id="cd346-524">32</span><span class="sxs-lookup"><span data-stu-id="cd346-524">32</span></span> |<span data-ttu-id="cd346-525">48</span><span class="sxs-lookup"><span data-stu-id="cd346-525">48</span></span> |<span data-ttu-id="cd346-526">1</span><span class="sxs-lookup"><span data-stu-id="cd346-526">1</span></span> |<span data-ttu-id="cd346-527">8</span><span class="sxs-lookup"><span data-stu-id="cd346-527">8</span></span> |<span data-ttu-id="cd346-528">16</span><span class="sxs-lookup"><span data-stu-id="cd346-528">16</span></span> |<span data-ttu-id="cd346-529">32</span><span class="sxs-lookup"><span data-stu-id="cd346-529">32</span></span> |
| <span data-ttu-id="cd346-530">DW1500</span><span class="sxs-lookup"><span data-stu-id="cd346-530">DW1500</span></span> |<span data-ttu-id="cd346-531">32</span><span class="sxs-lookup"><span data-stu-id="cd346-531">32</span></span> |<span data-ttu-id="cd346-532">60</span><span class="sxs-lookup"><span data-stu-id="cd346-532">60</span></span> |<span data-ttu-id="cd346-533">1</span><span class="sxs-lookup"><span data-stu-id="cd346-533">1</span></span> |<span data-ttu-id="cd346-534">8</span><span class="sxs-lookup"><span data-stu-id="cd346-534">8</span></span> |<span data-ttu-id="cd346-535">16</span><span class="sxs-lookup"><span data-stu-id="cd346-535">16</span></span> |<span data-ttu-id="cd346-536">32</span><span class="sxs-lookup"><span data-stu-id="cd346-536">32</span></span> |
| <span data-ttu-id="cd346-537">DW2000</span><span class="sxs-lookup"><span data-stu-id="cd346-537">DW2000</span></span> |<span data-ttu-id="cd346-538">32</span><span class="sxs-lookup"><span data-stu-id="cd346-538">32</span></span> |<span data-ttu-id="cd346-539">80</span><span class="sxs-lookup"><span data-stu-id="cd346-539">80</span></span> |<span data-ttu-id="cd346-540">1</span><span class="sxs-lookup"><span data-stu-id="cd346-540">1</span></span> |<span data-ttu-id="cd346-541">16</span><span class="sxs-lookup"><span data-stu-id="cd346-541">16</span></span> |<span data-ttu-id="cd346-542">32</span><span class="sxs-lookup"><span data-stu-id="cd346-542">32</span></span> |<span data-ttu-id="cd346-543">64</span><span class="sxs-lookup"><span data-stu-id="cd346-543">64</span></span> |
| <span data-ttu-id="cd346-544">DW3000</span><span class="sxs-lookup"><span data-stu-id="cd346-544">DW3000</span></span> |<span data-ttu-id="cd346-545">32</span><span class="sxs-lookup"><span data-stu-id="cd346-545">32</span></span> |<span data-ttu-id="cd346-546">120</span><span class="sxs-lookup"><span data-stu-id="cd346-546">120</span></span> |<span data-ttu-id="cd346-547">1</span><span class="sxs-lookup"><span data-stu-id="cd346-547">1</span></span> |<span data-ttu-id="cd346-548">16</span><span class="sxs-lookup"><span data-stu-id="cd346-548">16</span></span> |<span data-ttu-id="cd346-549">32</span><span class="sxs-lookup"><span data-stu-id="cd346-549">32</span></span> |<span data-ttu-id="cd346-550">64</span><span class="sxs-lookup"><span data-stu-id="cd346-550">64</span></span> |
| <span data-ttu-id="cd346-551">DW6000</span><span class="sxs-lookup"><span data-stu-id="cd346-551">DW6000</span></span> |<span data-ttu-id="cd346-552">32</span><span class="sxs-lookup"><span data-stu-id="cd346-552">32</span></span> |<span data-ttu-id="cd346-553">240</span><span class="sxs-lookup"><span data-stu-id="cd346-553">240</span></span> |<span data-ttu-id="cd346-554">1</span><span class="sxs-lookup"><span data-stu-id="cd346-554">1</span></span> |<span data-ttu-id="cd346-555">32</span><span class="sxs-lookup"><span data-stu-id="cd346-555">32</span></span> |<span data-ttu-id="cd346-556">64</span><span class="sxs-lookup"><span data-stu-id="cd346-556">64</span></span> |<span data-ttu-id="cd346-557">128</span><span class="sxs-lookup"><span data-stu-id="cd346-557">128</span></span> |

### <a name="allocation-and-consumption-of-concurrency-slots-for-static-resource-classes"></a><span data-ttu-id="cd346-558">Přidělení a využití souběžnosti přihrádky pro statické prostředků třídy</span><span class="sxs-lookup"><span data-stu-id="cd346-558">Allocation and consumption of concurrency slots for static resource classes</span></span>  
| <span data-ttu-id="cd346-559">DWU</span><span class="sxs-lookup"><span data-stu-id="cd346-559">DWU</span></span> | <span data-ttu-id="cd346-560">Maximální počet souběžných dotazů</span><span class="sxs-lookup"><span data-stu-id="cd346-560">Maximum concurrent queries</span></span> | <span data-ttu-id="cd346-561">Přidělené sloty souběžnosti</span><span class="sxs-lookup"><span data-stu-id="cd346-561">Concurrency slots allocated</span></span> |<span data-ttu-id="cd346-562">staticrc10</span><span class="sxs-lookup"><span data-stu-id="cd346-562">staticrc10</span></span> | <span data-ttu-id="cd346-563">staticrc20</span><span class="sxs-lookup"><span data-stu-id="cd346-563">staticrc20</span></span> | <span data-ttu-id="cd346-564">staticrc30</span><span class="sxs-lookup"><span data-stu-id="cd346-564">staticrc30</span></span> | <span data-ttu-id="cd346-565">staticrc40</span><span class="sxs-lookup"><span data-stu-id="cd346-565">staticrc40</span></span> | <span data-ttu-id="cd346-566">staticrc50</span><span class="sxs-lookup"><span data-stu-id="cd346-566">staticrc50</span></span> | <span data-ttu-id="cd346-567">staticrc60</span><span class="sxs-lookup"><span data-stu-id="cd346-567">staticrc60</span></span> | <span data-ttu-id="cd346-568">staticrc70</span><span class="sxs-lookup"><span data-stu-id="cd346-568">staticrc70</span></span> | <span data-ttu-id="cd346-569">staticrc80</span><span class="sxs-lookup"><span data-stu-id="cd346-569">staticrc80</span></span> |
|:--- |:---:|:---:|:---:|:---:|:---:|:---:|:---:|:---:|:---:|:---:|
| <span data-ttu-id="cd346-570">OD DW100</span><span class="sxs-lookup"><span data-stu-id="cd346-570">DW100</span></span> |<span data-ttu-id="cd346-571">4</span><span class="sxs-lookup"><span data-stu-id="cd346-571">4</span></span> |<span data-ttu-id="cd346-572">4</span><span class="sxs-lookup"><span data-stu-id="cd346-572">4</span></span> |<span data-ttu-id="cd346-573">1</span><span class="sxs-lookup"><span data-stu-id="cd346-573">1</span></span> |<span data-ttu-id="cd346-574">2</span><span class="sxs-lookup"><span data-stu-id="cd346-574">2</span></span> |<span data-ttu-id="cd346-575">4</span><span class="sxs-lookup"><span data-stu-id="cd346-575">4</span></span> |<span data-ttu-id="cd346-576">4</span><span class="sxs-lookup"><span data-stu-id="cd346-576">4</span></span> |<span data-ttu-id="cd346-577">4</span><span class="sxs-lookup"><span data-stu-id="cd346-577">4</span></span> |<span data-ttu-id="cd346-578">4</span><span class="sxs-lookup"><span data-stu-id="cd346-578">4</span></span> |<span data-ttu-id="cd346-579">4</span><span class="sxs-lookup"><span data-stu-id="cd346-579">4</span></span> |<span data-ttu-id="cd346-580">4</span><span class="sxs-lookup"><span data-stu-id="cd346-580">4</span></span> |
| <span data-ttu-id="cd346-581">DW200</span><span class="sxs-lookup"><span data-stu-id="cd346-581">DW200</span></span> |<span data-ttu-id="cd346-582">8</span><span class="sxs-lookup"><span data-stu-id="cd346-582">8</span></span> |<span data-ttu-id="cd346-583">8</span><span class="sxs-lookup"><span data-stu-id="cd346-583">8</span></span> |<span data-ttu-id="cd346-584">1</span><span class="sxs-lookup"><span data-stu-id="cd346-584">1</span></span> |<span data-ttu-id="cd346-585">2</span><span class="sxs-lookup"><span data-stu-id="cd346-585">2</span></span> |<span data-ttu-id="cd346-586">4</span><span class="sxs-lookup"><span data-stu-id="cd346-586">4</span></span> |<span data-ttu-id="cd346-587">8</span><span class="sxs-lookup"><span data-stu-id="cd346-587">8</span></span> |<span data-ttu-id="cd346-588">8</span><span class="sxs-lookup"><span data-stu-id="cd346-588">8</span></span> |<span data-ttu-id="cd346-589">8</span><span class="sxs-lookup"><span data-stu-id="cd346-589">8</span></span> |<span data-ttu-id="cd346-590">8</span><span class="sxs-lookup"><span data-stu-id="cd346-590">8</span></span> |<span data-ttu-id="cd346-591">8</span><span class="sxs-lookup"><span data-stu-id="cd346-591">8</span></span> |
| <span data-ttu-id="cd346-592">DW300</span><span class="sxs-lookup"><span data-stu-id="cd346-592">DW300</span></span> |<span data-ttu-id="cd346-593">12</span><span class="sxs-lookup"><span data-stu-id="cd346-593">12</span></span> |<span data-ttu-id="cd346-594">12</span><span class="sxs-lookup"><span data-stu-id="cd346-594">12</span></span> |<span data-ttu-id="cd346-595">1</span><span class="sxs-lookup"><span data-stu-id="cd346-595">1</span></span> |<span data-ttu-id="cd346-596">2</span><span class="sxs-lookup"><span data-stu-id="cd346-596">2</span></span> |<span data-ttu-id="cd346-597">4</span><span class="sxs-lookup"><span data-stu-id="cd346-597">4</span></span> |<span data-ttu-id="cd346-598">8</span><span class="sxs-lookup"><span data-stu-id="cd346-598">8</span></span> |<span data-ttu-id="cd346-599">8</span><span class="sxs-lookup"><span data-stu-id="cd346-599">8</span></span> |<span data-ttu-id="cd346-600">8</span><span class="sxs-lookup"><span data-stu-id="cd346-600">8</span></span> |<span data-ttu-id="cd346-601">8</span><span class="sxs-lookup"><span data-stu-id="cd346-601">8</span></span> |<span data-ttu-id="cd346-602">8</span><span class="sxs-lookup"><span data-stu-id="cd346-602">8</span></span> |
| <span data-ttu-id="cd346-603">DW400</span><span class="sxs-lookup"><span data-stu-id="cd346-603">DW400</span></span> |<span data-ttu-id="cd346-604">16</span><span class="sxs-lookup"><span data-stu-id="cd346-604">16</span></span> |<span data-ttu-id="cd346-605">16</span><span class="sxs-lookup"><span data-stu-id="cd346-605">16</span></span> |<span data-ttu-id="cd346-606">1</span><span class="sxs-lookup"><span data-stu-id="cd346-606">1</span></span> |<span data-ttu-id="cd346-607">2</span><span class="sxs-lookup"><span data-stu-id="cd346-607">2</span></span> |<span data-ttu-id="cd346-608">4</span><span class="sxs-lookup"><span data-stu-id="cd346-608">4</span></span> |<span data-ttu-id="cd346-609">8</span><span class="sxs-lookup"><span data-stu-id="cd346-609">8</span></span> |<span data-ttu-id="cd346-610">16</span><span class="sxs-lookup"><span data-stu-id="cd346-610">16</span></span> |<span data-ttu-id="cd346-611">16</span><span class="sxs-lookup"><span data-stu-id="cd346-611">16</span></span> |<span data-ttu-id="cd346-612">16</span><span class="sxs-lookup"><span data-stu-id="cd346-612">16</span></span> |<span data-ttu-id="cd346-613">16</span><span class="sxs-lookup"><span data-stu-id="cd346-613">16</span></span> |
| <span data-ttu-id="cd346-614">DW500</span><span class="sxs-lookup"><span data-stu-id="cd346-614">DW500</span></span> | <span data-ttu-id="cd346-615">20</span><span class="sxs-lookup"><span data-stu-id="cd346-615">20</span></span>| <span data-ttu-id="cd346-616">20</span><span class="sxs-lookup"><span data-stu-id="cd346-616">20</span></span>| <span data-ttu-id="cd346-617">1</span><span class="sxs-lookup"><span data-stu-id="cd346-617">1</span></span>| <span data-ttu-id="cd346-618">2</span><span class="sxs-lookup"><span data-stu-id="cd346-618">2</span></span>| <span data-ttu-id="cd346-619">4</span><span class="sxs-lookup"><span data-stu-id="cd346-619">4</span></span>| <span data-ttu-id="cd346-620">8</span><span class="sxs-lookup"><span data-stu-id="cd346-620">8</span></span>| <span data-ttu-id="cd346-621">16</span><span class="sxs-lookup"><span data-stu-id="cd346-621">16</span></span>| <span data-ttu-id="cd346-622">16</span><span class="sxs-lookup"><span data-stu-id="cd346-622">16</span></span>| <span data-ttu-id="cd346-623">16</span><span class="sxs-lookup"><span data-stu-id="cd346-623">16</span></span>| <span data-ttu-id="cd346-624">16</span><span class="sxs-lookup"><span data-stu-id="cd346-624">16</span></span>|
| <span data-ttu-id="cd346-625">DW600</span><span class="sxs-lookup"><span data-stu-id="cd346-625">DW600</span></span> | <span data-ttu-id="cd346-626">24</span><span class="sxs-lookup"><span data-stu-id="cd346-626">24</span></span>| <span data-ttu-id="cd346-627">24</span><span class="sxs-lookup"><span data-stu-id="cd346-627">24</span></span>| <span data-ttu-id="cd346-628">1</span><span class="sxs-lookup"><span data-stu-id="cd346-628">1</span></span>| <span data-ttu-id="cd346-629">2</span><span class="sxs-lookup"><span data-stu-id="cd346-629">2</span></span>| <span data-ttu-id="cd346-630">4</span><span class="sxs-lookup"><span data-stu-id="cd346-630">4</span></span>| <span data-ttu-id="cd346-631">8</span><span class="sxs-lookup"><span data-stu-id="cd346-631">8</span></span>| <span data-ttu-id="cd346-632">16</span><span class="sxs-lookup"><span data-stu-id="cd346-632">16</span></span>| <span data-ttu-id="cd346-633">16</span><span class="sxs-lookup"><span data-stu-id="cd346-633">16</span></span>| <span data-ttu-id="cd346-634">16</span><span class="sxs-lookup"><span data-stu-id="cd346-634">16</span></span>| <span data-ttu-id="cd346-635">16</span><span class="sxs-lookup"><span data-stu-id="cd346-635">16</span></span>|
| <span data-ttu-id="cd346-636">DW1000</span><span class="sxs-lookup"><span data-stu-id="cd346-636">DW1000</span></span> | <span data-ttu-id="cd346-637">32</span><span class="sxs-lookup"><span data-stu-id="cd346-637">32</span></span>| <span data-ttu-id="cd346-638">40</span><span class="sxs-lookup"><span data-stu-id="cd346-638">40</span></span>| <span data-ttu-id="cd346-639">1</span><span class="sxs-lookup"><span data-stu-id="cd346-639">1</span></span>| <span data-ttu-id="cd346-640">2</span><span class="sxs-lookup"><span data-stu-id="cd346-640">2</span></span>| <span data-ttu-id="cd346-641">4</span><span class="sxs-lookup"><span data-stu-id="cd346-641">4</span></span>| <span data-ttu-id="cd346-642">8</span><span class="sxs-lookup"><span data-stu-id="cd346-642">8</span></span>| <span data-ttu-id="cd346-643">16</span><span class="sxs-lookup"><span data-stu-id="cd346-643">16</span></span>| <span data-ttu-id="cd346-644">32</span><span class="sxs-lookup"><span data-stu-id="cd346-644">32</span></span>| <span data-ttu-id="cd346-645">32</span><span class="sxs-lookup"><span data-stu-id="cd346-645">32</span></span>| <span data-ttu-id="cd346-646">32</span><span class="sxs-lookup"><span data-stu-id="cd346-646">32</span></span>|
| <span data-ttu-id="cd346-647">DW1200</span><span class="sxs-lookup"><span data-stu-id="cd346-647">DW1200</span></span> | <span data-ttu-id="cd346-648">32</span><span class="sxs-lookup"><span data-stu-id="cd346-648">32</span></span>| <span data-ttu-id="cd346-649">48</span><span class="sxs-lookup"><span data-stu-id="cd346-649">48</span></span>| <span data-ttu-id="cd346-650">1</span><span class="sxs-lookup"><span data-stu-id="cd346-650">1</span></span>| <span data-ttu-id="cd346-651">2</span><span class="sxs-lookup"><span data-stu-id="cd346-651">2</span></span>| <span data-ttu-id="cd346-652">4</span><span class="sxs-lookup"><span data-stu-id="cd346-652">4</span></span>| <span data-ttu-id="cd346-653">8</span><span class="sxs-lookup"><span data-stu-id="cd346-653">8</span></span>| <span data-ttu-id="cd346-654">16</span><span class="sxs-lookup"><span data-stu-id="cd346-654">16</span></span>| <span data-ttu-id="cd346-655">32</span><span class="sxs-lookup"><span data-stu-id="cd346-655">32</span></span>| <span data-ttu-id="cd346-656">32</span><span class="sxs-lookup"><span data-stu-id="cd346-656">32</span></span>| <span data-ttu-id="cd346-657">32</span><span class="sxs-lookup"><span data-stu-id="cd346-657">32</span></span>|
| <span data-ttu-id="cd346-658">DW1500</span><span class="sxs-lookup"><span data-stu-id="cd346-658">DW1500</span></span> | <span data-ttu-id="cd346-659">32</span><span class="sxs-lookup"><span data-stu-id="cd346-659">32</span></span>| <span data-ttu-id="cd346-660">60</span><span class="sxs-lookup"><span data-stu-id="cd346-660">60</span></span>| <span data-ttu-id="cd346-661">1</span><span class="sxs-lookup"><span data-stu-id="cd346-661">1</span></span>| <span data-ttu-id="cd346-662">2</span><span class="sxs-lookup"><span data-stu-id="cd346-662">2</span></span>| <span data-ttu-id="cd346-663">4</span><span class="sxs-lookup"><span data-stu-id="cd346-663">4</span></span>| <span data-ttu-id="cd346-664">8</span><span class="sxs-lookup"><span data-stu-id="cd346-664">8</span></span>| <span data-ttu-id="cd346-665">16</span><span class="sxs-lookup"><span data-stu-id="cd346-665">16</span></span>| <span data-ttu-id="cd346-666">32</span><span class="sxs-lookup"><span data-stu-id="cd346-666">32</span></span>| <span data-ttu-id="cd346-667">32</span><span class="sxs-lookup"><span data-stu-id="cd346-667">32</span></span>| <span data-ttu-id="cd346-668">32</span><span class="sxs-lookup"><span data-stu-id="cd346-668">32</span></span>|
| <span data-ttu-id="cd346-669">DW2000</span><span class="sxs-lookup"><span data-stu-id="cd346-669">DW2000</span></span> | <span data-ttu-id="cd346-670">32</span><span class="sxs-lookup"><span data-stu-id="cd346-670">32</span></span>| <span data-ttu-id="cd346-671">80</span><span class="sxs-lookup"><span data-stu-id="cd346-671">80</span></span>| <span data-ttu-id="cd346-672">1</span><span class="sxs-lookup"><span data-stu-id="cd346-672">1</span></span>| <span data-ttu-id="cd346-673">2</span><span class="sxs-lookup"><span data-stu-id="cd346-673">2</span></span>| <span data-ttu-id="cd346-674">4</span><span class="sxs-lookup"><span data-stu-id="cd346-674">4</span></span>| <span data-ttu-id="cd346-675">8</span><span class="sxs-lookup"><span data-stu-id="cd346-675">8</span></span>| <span data-ttu-id="cd346-676">16</span><span class="sxs-lookup"><span data-stu-id="cd346-676">16</span></span>| <span data-ttu-id="cd346-677">32</span><span class="sxs-lookup"><span data-stu-id="cd346-677">32</span></span>| <span data-ttu-id="cd346-678">64</span><span class="sxs-lookup"><span data-stu-id="cd346-678">64</span></span>| <span data-ttu-id="cd346-679">64</span><span class="sxs-lookup"><span data-stu-id="cd346-679">64</span></span>|
| <span data-ttu-id="cd346-680">DW3000</span><span class="sxs-lookup"><span data-stu-id="cd346-680">DW3000</span></span> | <span data-ttu-id="cd346-681">32</span><span class="sxs-lookup"><span data-stu-id="cd346-681">32</span></span>| <span data-ttu-id="cd346-682">120</span><span class="sxs-lookup"><span data-stu-id="cd346-682">120</span></span>| <span data-ttu-id="cd346-683">1</span><span class="sxs-lookup"><span data-stu-id="cd346-683">1</span></span>| <span data-ttu-id="cd346-684">2</span><span class="sxs-lookup"><span data-stu-id="cd346-684">2</span></span>| <span data-ttu-id="cd346-685">4</span><span class="sxs-lookup"><span data-stu-id="cd346-685">4</span></span>| <span data-ttu-id="cd346-686">8</span><span class="sxs-lookup"><span data-stu-id="cd346-686">8</span></span>| <span data-ttu-id="cd346-687">16</span><span class="sxs-lookup"><span data-stu-id="cd346-687">16</span></span>| <span data-ttu-id="cd346-688">32</span><span class="sxs-lookup"><span data-stu-id="cd346-688">32</span></span>| <span data-ttu-id="cd346-689">64</span><span class="sxs-lookup"><span data-stu-id="cd346-689">64</span></span>| <span data-ttu-id="cd346-690">64</span><span class="sxs-lookup"><span data-stu-id="cd346-690">64</span></span>|
| <span data-ttu-id="cd346-691">DW6000</span><span class="sxs-lookup"><span data-stu-id="cd346-691">DW6000</span></span> | <span data-ttu-id="cd346-692">32</span><span class="sxs-lookup"><span data-stu-id="cd346-692">32</span></span>| <span data-ttu-id="cd346-693">240</span><span class="sxs-lookup"><span data-stu-id="cd346-693">240</span></span>| <span data-ttu-id="cd346-694">1</span><span class="sxs-lookup"><span data-stu-id="cd346-694">1</span></span>| <span data-ttu-id="cd346-695">2</span><span class="sxs-lookup"><span data-stu-id="cd346-695">2</span></span>| <span data-ttu-id="cd346-696">4</span><span class="sxs-lookup"><span data-stu-id="cd346-696">4</span></span>| <span data-ttu-id="cd346-697">8</span><span class="sxs-lookup"><span data-stu-id="cd346-697">8</span></span>| <span data-ttu-id="cd346-698">16</span><span class="sxs-lookup"><span data-stu-id="cd346-698">16</span></span>| <span data-ttu-id="cd346-699">32</span><span class="sxs-lookup"><span data-stu-id="cd346-699">32</span></span>| <span data-ttu-id="cd346-700">64</span><span class="sxs-lookup"><span data-stu-id="cd346-700">64</span></span>| <span data-ttu-id="cd346-701">128</span><span class="sxs-lookup"><span data-stu-id="cd346-701">128</span></span>|

<span data-ttu-id="cd346-702">Z těchto tabulek zobrazí se systémem SQL Data Warehouse jako DW1000 přiděluje maximálně 32 souběžných dotazů a celkem 40 sloty souběžnosti.</span><span class="sxs-lookup"><span data-stu-id="cd346-702">From these tables, you can see that SQL Data Warehouse running as DW1000 allocates a maximum of 32 concurrent queries and a total of 40 concurrency slots.</span></span> <span data-ttu-id="cd346-703">Pokud všichni uživatelé používají v smallrc, 32 souběžných dotazů by být povolena, protože každý dotaz by používat 1 slot souběžnosti.</span><span class="sxs-lookup"><span data-stu-id="cd346-703">If all users are running in smallrc, 32 concurrent queries would be allowed because each query would consume 1 concurrency slot.</span></span> <span data-ttu-id="cd346-704">Pokud všechny uživatele DW1000 byly spuštěny v mediumrc, by se každý dotaz přidělit 800 MB za distribuci pro přidělení celkové paměti 47 GB na jeden dotaz a bude concurrency omezená na 5 uživatele (40 sloty souběžnosti 8 přihrádek na mediumrc uživatele /).</span><span class="sxs-lookup"><span data-stu-id="cd346-704">If all users on a DW1000 were running in mediumrc, each query would be allocated 800 MB per distribution for a total memory allocation of 47 GB per query, and concurrency would be limited to 5 users (40 concurrency slots / 8 slots per mediumrc user).</span></span>

## <a name="selecting-proper-resource-class"></a><span data-ttu-id="cd346-705">Výběr třídy odpovídající prostředek</span><span class="sxs-lookup"><span data-stu-id="cd346-705">Selecting proper resource class</span></span>  
<span data-ttu-id="cd346-706">Doporučeným postupem je trvale přiřazovat uživatele k Třída prostředků, nemusíte měnit jejich třídy prostředků.</span><span class="sxs-lookup"><span data-stu-id="cd346-706">A good practice is to permanently assign users to a resource class rather than changing their resource classes.</span></span> <span data-ttu-id="cd346-707">Můžete například vytvořit zatížení na Clusterované tabulky columnstore kvalitnější indexy při přidělení více paměti.</span><span class="sxs-lookup"><span data-stu-id="cd346-707">For example, loads to clustered columnstore tables create higher-quality indexes when allocated more memory.</span></span> <span data-ttu-id="cd346-708">Zajistit, aby zatížení měly přístup k vyšší paměti, vytvořte uživatele speciálně pro načítání dat a trvale tomuto uživateli přiřadit vyšší Třída prostředků.</span><span class="sxs-lookup"><span data-stu-id="cd346-708">To ensure that loads have access to higher memory, create a user specifically for loading data and permanently assign this user to a higher resource class.</span></span>
<span data-ttu-id="cd346-709">Existuje několik osvědčených postupů použít postup popsaný v tomto poli.</span><span class="sxs-lookup"><span data-stu-id="cd346-709">There are a couple of best practices to follow here.</span></span> <span data-ttu-id="cd346-710">Jak je uvedeno nahoře, SQL DW podporuje dva druhy třída typy prostředků: prostředků statické třídy a třídy dynamického prostředku.</span><span class="sxs-lookup"><span data-stu-id="cd346-710">As mentioned above, SQL DW supports two kinds of resource class types: static resource classes and dynamic resource classes.</span></span>
### <a name="loading-best-practices"></a><span data-ttu-id="cd346-711">Načítání osvědčené postupy</span><span class="sxs-lookup"><span data-stu-id="cd346-711">Loading best practices</span></span>
1.  <span data-ttu-id="cd346-712">Pokud očekávání zatížení s regulární množství dat, třída statické prostředků je vhodné použít.</span><span class="sxs-lookup"><span data-stu-id="cd346-712">If the expectations are loads with regular amount of data, a static resource class is a good choice.</span></span> <span data-ttu-id="cd346-713">Později při škálování Chcete-li získat další výpočetní výkon, datového skladu bude moci spustit více souběžných dotazů out-of-the-box, protože uživatel zatížení nespotřebovává více paměti.</span><span class="sxs-lookup"><span data-stu-id="cd346-713">Later, when scaling up to get more computational power, the data warehouse will be able to run more concurrent queries out-of-the-box, as the load user does not consume more memory.</span></span>
2.  <span data-ttu-id="cd346-714">Pokud očekávání větší zatížení v některých případech, třída dynamické prostředků je vhodné použít.</span><span class="sxs-lookup"><span data-stu-id="cd346-714">If the expectations are bigger loads in some occasions, a dynamic resource class is a good choice.</span></span> <span data-ttu-id="cd346-715">Později, při škálování Chcete-li získat další výpočetní výkon, zatížení uživatel dostane další paměť out-of-the-box, proto povolení zatížení rychlejší.</span><span class="sxs-lookup"><span data-stu-id="cd346-715">Later, when scaling up to get more computational power, the load user will get more memory out-of-the-box, hence allowing the load to perform faster.</span></span>

<span data-ttu-id="cd346-716">Paměť potřebnému ke zpracování zatížení efektivně závisí na povahu načíst tabulky a množství dat, zpracování.</span><span class="sxs-lookup"><span data-stu-id="cd346-716">The memory needed to process loads efficiently depends on the nature of the table loaded and the amount of data processed.</span></span> <span data-ttu-id="cd346-717">Načítání dat do tabulek KÚS například vyžaduje některé paměti umožníte KÚS rowgroups dosáhnout optimality.</span><span class="sxs-lookup"><span data-stu-id="cd346-717">For instance, loading data into CCI tables requires some memory to let CCI rowgroups reach optimality.</span></span> <span data-ttu-id="cd346-718">Další podrobnosti najdete v tématu indexy Columnstore - data načítání pokyny.</span><span class="sxs-lookup"><span data-stu-id="cd346-718">For more details, see the Columnstore indexes - data loading guidance.</span></span>

<span data-ttu-id="cd346-719">Jako osvědčený postup doporučujeme používat pro zatížení alespoň 200MB paměti.</span><span class="sxs-lookup"><span data-stu-id="cd346-719">As a best practice, we advise you to use at least 200MB of memory for loads.</span></span>

### <a name="querying-best-practices"></a><span data-ttu-id="cd346-720">Dotazování osvědčené postupy</span><span class="sxs-lookup"><span data-stu-id="cd346-720">Querying best practices</span></span>
<span data-ttu-id="cd346-721">Dotazy mají různé požadavky v závislosti na jejich složitost.</span><span class="sxs-lookup"><span data-stu-id="cd346-721">Queries have different requirements depending on their complexity.</span></span> <span data-ttu-id="cd346-722">Zvýšení paměti na jeden dotaz nebo zvýšení souběžnost jsou obě platné způsoby posílení celkovou propustnost podle potřeb dotazu.</span><span class="sxs-lookup"><span data-stu-id="cd346-722">Increasing memory per query or increasing the concurrency are both valid ways to augment overall throughput depending on the query needs.</span></span>
1.  <span data-ttu-id="cd346-723">Pokud očekávání regulární a složité dotazy (například ke generování sestav denní nebo týdenní) a není nutné využívat výhod souběžnosti, třída dynamické prostředků je vhodné použít.</span><span class="sxs-lookup"><span data-stu-id="cd346-723">If the expectations are regular, complex queries (for instance, to generate daily and weekly reports) and do not need to take advantage of concurrency, a dynamic resource class is a good choice.</span></span> <span data-ttu-id="cd346-724">Pokud má systém další data ke zpracování, vertikálním navýšení kapacity datového skladu se proto automaticky poskytnout další paměť uživatel, který spouští dotaz.</span><span class="sxs-lookup"><span data-stu-id="cd346-724">If the system has more data to process, scaling up the data warehouse will therefore automatically provide more memory to the user running the query.</span></span>
2.  <span data-ttu-id="cd346-725">Pokud očekávání vzory proměnnou nebo křivka souběžnosti (pro instanci Pokud databáze je dotazován prostřednictvím webového uživatelského rozhraní široce dostupné), třída statické prostředků je vhodné použít.</span><span class="sxs-lookup"><span data-stu-id="cd346-725">If the expectations are variable or diurnal concurrency patterns (for instance if the database is queried through a web UI broadly accessible), a static resource class is a good choice.</span></span> <span data-ttu-id="cd346-726">Později při rozšiřování prostředků do datového skladu, uživatel přidružený k třídě statické prostředků automaticky bude možné spouštět více souběžných dotazů.</span><span class="sxs-lookup"><span data-stu-id="cd346-726">Later, when scaling up to data warehouse, the user associated with the static resource class will automatically be able to run more concurrent queries.</span></span>

<span data-ttu-id="cd346-727">Výběr správné paměti grant podle potřeb vašeho dotazu je netriviální, protože závisí na mnoha faktorech, jako je množství dat získaných, povaha schémata tabulek a různé spojení, výběru a predikáty skupiny.</span><span class="sxs-lookup"><span data-stu-id="cd346-727">Selecting proper memory grant depending on the need of your query is non-trivial, as it depends on many factors, such as the amount of data queried, the nature of the table schemas, and various join, selection, and group predicates.</span></span> <span data-ttu-id="cd346-728">Z hlediska obecné přidělení více paměti se povolit dotazy na rychlejší, ale by snížení celkového souběžnosti.</span><span class="sxs-lookup"><span data-stu-id="cd346-728">From a general standpoint, allocating more memory will allow queries to complete faster, but would reduce the overall concurrency.</span></span> <span data-ttu-id="cd346-729">Pokud souběžnosti není problém, přepsání přidělování paměti není poškodit.</span><span class="sxs-lookup"><span data-stu-id="cd346-729">If concurrency is not an issue, over-allocating memory does not harm.</span></span> <span data-ttu-id="cd346-730">A systém doladit propustnost, při různých typů prostředků třídy mohou být vyžadovány.</span><span class="sxs-lookup"><span data-stu-id="cd346-730">To fine-tune throughput, trying various flavors of resource classes may be required.</span></span>

<span data-ttu-id="cd346-731">Můžete provádět následující uložené procedury a pokuste se zjistit grant souběžnosti a paměti na třídě prostředků v dané SLO a nejbližší nejlepší Třída prostředků pro operace náročné na prostředky KÚS paměti na bez oddílů tabulky KÚS na třídy daného prostředku:</span><span class="sxs-lookup"><span data-stu-id="cd346-731">You can use the following stored procedure to figure out concurrency and memory grant per resource class at a given SLO and the closest best resource class for memory intensive CCI operations on non-partitioned CCI table at a given resource class:</span></span>

#### <a name="description"></a><span data-ttu-id="cd346-732">Popis:</span><span class="sxs-lookup"><span data-stu-id="cd346-732">Description:</span></span>  
<span data-ttu-id="cd346-733">Tady je účelem tuto uloženou proceduru:</span><span class="sxs-lookup"><span data-stu-id="cd346-733">Here's the purpose of this stored procedure:</span></span>  
1. <span data-ttu-id="cd346-734">Který pomůže zjistit uživateli souběžnosti a paměť udělit za Třída prostředků v daném objektu SLO.</span><span class="sxs-lookup"><span data-stu-id="cd346-734">To help user figure out concurrency and memory grant per resource class at a given SLO.</span></span> <span data-ttu-id="cd346-735">Uživatel musí zadejte hodnotu NULL pro schéma a název tabulky pro tento, jak je znázorněno v následujícím příkladu.</span><span class="sxs-lookup"><span data-stu-id="cd346-735">User needs to provide NULL for both schema and tablename for this as shown in the example below.</span></span>  
2. <span data-ttu-id="cd346-736">Abyste uživatele pochopit nejbližší nejlepší Třída prostředků pro intensed paměti KÚS operací (zatížení, kopie tabulku znovu sestavit index, atd.) na jiný dělenou tabulku KÚS na třídu daného prostředku.</span><span class="sxs-lookup"><span data-stu-id="cd346-736">To help user figure out the closest best resource class for the memory intensed CCI operations (load, copy table, rebuild index, etc.) on non partitioned CCI table at a given resource class.</span></span> <span data-ttu-id="cd346-737">Uložená procedura používá schéma tabulky a zjistěte, udělte požadovanou paměť pro tento.</span><span class="sxs-lookup"><span data-stu-id="cd346-737">The stored proc uses table schema to find out the required memory grant for this.</span></span>

#### <a name="dependencies--restrictions"></a><span data-ttu-id="cd346-738">Závislosti & omezení:</span><span class="sxs-lookup"><span data-stu-id="cd346-738">Dependencies & Restrictions:</span></span>
- <span data-ttu-id="cd346-739">Tato uložená procedura není určen k výpočtu tabulka rozdělena na oddíly KÚS požadavek na paměť.</span><span class="sxs-lookup"><span data-stu-id="cd346-739">This stored proc is not designed to calculate memory requirement for partitioned-cci table.</span></span>    
- <span data-ttu-id="cd346-740">Tato uložená procedura neberou požadavek na paměť v úvahu vyberte součást funkce CTAS/INSERT-výběr a předpokládá, že bude jednoduché vyberte.</span><span class="sxs-lookup"><span data-stu-id="cd346-740">This stored proc doesn't take memory requirement into account for the SELECT part of CTAS/INSERT-SELECT and assumes it to be a simple SELECT.</span></span>
- <span data-ttu-id="cd346-741">Tato uložená procedura používá dočasnou tabulku, takže toto lze použít v relaci kde byl vytvořen tento uložené procedury.</span><span class="sxs-lookup"><span data-stu-id="cd346-741">This stored proc uses a temp table so this can be used in the session where this stored proc was created.</span></span>    
- <span data-ttu-id="cd346-742">Tato uložená procedura závisí na aktuální nabídky (např. konfiguraci hardwaru, konfigurace DMS) a pokud se některé této změny pak tato uložená procedura nebude fungovat správně.</span><span class="sxs-lookup"><span data-stu-id="cd346-742">This stored proc depends on the current offerings (e.g. hardware configuration, DMS config) and if any of that changes then this stored proc would not work correctly.</span></span>  
- <span data-ttu-id="cd346-743">Tato uložená procedura závisí na existující nabízený souběžnosti limit a pokud se změní, pak tato uložená procedura nebude fungovat správně.</span><span class="sxs-lookup"><span data-stu-id="cd346-743">This stored proc depends on existing offered concurrency limit and if that changes then this stored proc would not work correctly.</span></span>  
- <span data-ttu-id="cd346-744">Tato uložená procedura závisí na stávající nabídky Třída prostředků a který změní-li se pak to uložené procedura zamýšlenému není pracovní správně.</span><span class="sxs-lookup"><span data-stu-id="cd346-744">This stored proc depends on existing resource class offerings and if that changes then this stored proc wuold not work correctly.</span></span>  

>  [!NOTE]  
>  <span data-ttu-id="cd346-745">Pokud se nezobrazují výstupu po spuštění uložené procedury s parametry zadané, pak může být dvěma případy.</span><span class="sxs-lookup"><span data-stu-id="cd346-745">If you are not getting output after executing stored procedure with parameters provided then there could be two cases.</span></span> <br /><span data-ttu-id="cd346-746">1. Buď parametr datového skladu obsahuje neplatnou hodnotu SLO</span><span class="sxs-lookup"><span data-stu-id="cd346-746">1. Either DW Parameter contains invalid SLO value</span></span> <br /><span data-ttu-id="cd346-747">2. NEBO pokud byl poskytnut název tabulky neexistují žádné odpovídající Třída prostředků pro operaci KÚS.</span><span class="sxs-lookup"><span data-stu-id="cd346-747">2. OR there are no matching resource class for CCI operation if table name was provided.</span></span> <br /><span data-ttu-id="cd346-748">Například v od DW100, nejvyšší přidělení paměti k dispozici je 400MB a pokud je schéma tabulky dost široké překříží požadavek 400MB.</span><span class="sxs-lookup"><span data-stu-id="cd346-748">For example, at DW100, highest memory grant available is 400MB and if table schema is wide enough to cross the requirement of 400MB.</span></span>
      
#### <a name="usage-example"></a><span data-ttu-id="cd346-749">Příklad použití:</span><span class="sxs-lookup"><span data-stu-id="cd346-749">Usage example:</span></span>
<span data-ttu-id="cd346-750">Syntaxe:</span><span class="sxs-lookup"><span data-stu-id="cd346-750">Syntax:</span></span>  
`EXEC dbo.prc_workload_management_by_DWU @DWU VARCHAR(7), @SCHEMA_NAME VARCHAR(128), @TABLE_NAME VARCHAR(128)`  
1. <span data-ttu-id="cd346-751">@DWU:Buď zadejte parametr hodnotu NULL pro rozbalení aktuální DWU z databáze datového skladu nebo zadejte všechny podporované DWU ve tvaru "od DW100.</span><span class="sxs-lookup"><span data-stu-id="cd346-751">@DWU: Either provide a NULL parameter to extract the current DWU from the DW DB or provide any supported DWU in the form of 'DW100'</span></span>
2. <span data-ttu-id="cd346-752">@SCHEMA_NAME:Zadejte název schématu tabulky</span><span class="sxs-lookup"><span data-stu-id="cd346-752">@SCHEMA_NAME: Provide a schema name of the table</span></span>
3. <span data-ttu-id="cd346-753">@TABLE_NAME:Zadejte název tabulky zájmu</span><span class="sxs-lookup"><span data-stu-id="cd346-753">@TABLE_NAME: Provide a table name of the interest</span></span>

<span data-ttu-id="cd346-754">Příklady provádění této uložené procedury:</span><span class="sxs-lookup"><span data-stu-id="cd346-754">Examples executing this stored proc:</span></span>  
```sql  
EXEC dbo.prc_workload_management_by_DWU 'DW2000', 'dbo', 'Table1';  
EXEC dbo.prc_workload_management_by_DWU NULL, 'dbo', 'Table1';  
EXEC dbo.prc_workload_management_by_DWU 'DW6000', NULL, NULL;  
EXEC dbo.prc_workload_management_by_DWU NULL, NULL, NULL;  
```

<span data-ttu-id="cd346-755">Tabulky1 použít v uvedených příkladech by bylo možné vytvořit, jak je uvedeno níže:</span><span class="sxs-lookup"><span data-stu-id="cd346-755">Table1 used in the above examples could be created as below:</span></span>  
`CREATE TABLE Table1 (a int, b varchar(50), c decimal (18,10), d char(10), e varbinary(15), f float, g datetime, h date);`

#### <a name="heres-the-stored-procedure-definition"></a><span data-ttu-id="cd346-756">Tady je definici uložené procedury:</span><span class="sxs-lookup"><span data-stu-id="cd346-756">Here's the stored procedure definition:</span></span>
```sql  
-------------------------------------------------------------------------------
-- Dropping prc_workload_management_by_DWU procedure if it exists.
-------------------------------------------------------------------------------
IF EXISTS (SELECT * FROM sys.objects WHERE type = 'P' AND name = 'prc_workload_management_by_DWU')
DROP PROCEDURE dbo.prc_workload_management_by_DWU
GO

-------------------------------------------------------------------------------
-- Creating prc_workload_management_by_DWU.
-------------------------------------------------------------------------------
CREATE PROCEDURE dbo.prc_workload_management_by_DWU
(   @DWU VARCHAR(7),
      @SCHEMA_NAME VARCHAR(128),
       @TABLE_NAME VARCHAR(128)
)
AS
IF @DWU IS NULL
BEGIN
-- Selecting proper DWU for the current DB if not specified.
SET @DWU = (
  SELECT 'DW'+CAST(COUNT(*)*100 AS VARCHAR(10))
  FROM sys.dm_pdw_nodes
  WHERE type = 'COMPUTE')
END

DECLARE @DWU_NUM INT
SET @DWU_NUM = CAST (SUBSTRING(@DWU, 3, LEN(@DWU)-2) AS INT)

-- Raise error if either schema name or table name is supplied but not both them supplied
--IF ((@SCHEMA_NAME IS NOT NULL AND @TABLE_NAME IS NULL) OR (@TABLE_NAME IS NULL AND @SCHEMA_NAME IS NOT NULL))
--     RAISEERROR('User need to supply either both Schema Name and Table Name or none of them')
       
-- Dropping temp table if exists.
IF OBJECT_ID('tempdb..#ref') IS NOT NULL
BEGIN
  DROP TABLE #ref;
END

-- Creating ref. temptable (CTAS) to hold mapping info.
-- CREATE TABLE #ref
CREATE TABLE #ref
WITH (DISTRIBUTION = ROUND_ROBIN)
AS 
WITH
-- Creating concurrency slots mapping for various DWUs.
alloc
AS
(
  SELECT 'DW100' AS DWU, 4 AS max_queries, 4 AS max_slots, 1 AS slots_used_smallrc, 1 AS slots_used_mediumrc,
        2 AS slots_used_largerc, 4 AS slots_used_xlargerc, 1 AS slots_used_staticrc10, 2 AS slots_used_staticrc20,
        4 AS slots_used_staticrc30, 4 AS slots_used_staticrc40, 4 AS slots_used_staticrc50,
        4 AS slots_used_staticrc60, 4 AS slots_used_staticrc70, 4 AS slots_used_staticrc80
  UNION ALL
    SELECT 'DW200', 8, 8, 1, 2, 4, 8, 1, 2, 4, 8, 8, 8, 8, 8
  UNION ALL
    SELECT 'DW300', 12, 12, 1, 2, 4, 8, 1, 2, 4, 8, 8, 8, 8, 8
  UNION ALL
    SELECT 'DW400', 16, 16, 1, 4, 8, 16, 1, 2, 4, 8, 16, 16, 16, 16
  UNION ALL
     SELECT 'DW500', 20, 20, 1, 4, 8, 16, 1, 2, 4, 8, 16, 16, 16, 16
  UNION ALL
    SELECT 'DW600', 24, 24, 1, 4, 8, 16, 1, 2, 4, 8, 16, 16, 16, 16
  UNION ALL
    SELECT 'DW1000', 32, 40, 1, 8, 16, 32, 1, 2, 4, 8, 16, 32, 32, 32
  UNION ALL
    SELECT 'DW1200', 32, 48, 1, 8, 16, 32, 1, 2, 4, 8, 16, 32, 32, 32
  UNION ALL
    SELECT 'DW1500', 32, 60, 1, 8, 16, 32, 1, 2, 4, 8, 16, 32, 32, 32
  UNION ALL
    SELECT 'DW2000', 32, 80, 1, 16, 32, 64, 1, 2, 4, 8, 16, 32, 64, 64
  UNION ALL
   SELECT 'DW3000', 32, 120, 1, 16, 32, 64, 1, 2, 4, 8, 16, 32, 64, 64
  UNION ALL
    SELECT 'DW6000', 32, 240, 1, 32, 64, 128, 1, 2, 4, 8, 16, 32, 64, 128
)
-- Creating workload mapping to their corresponding slot consumption and default memory grant.
,map
AS
(
  SELECT 'SloDWGroupC00' AS wg_name,1 AS slots_used,100 AS tgt_mem_grant_MB
  UNION ALL
    SELECT 'SloDWGroupC01',2,200
  UNION ALL
    SELECT 'SloDWGroupC02',4,400
  UNION ALL
    SELECT 'SloDWGroupC03',8,800
  UNION ALL
    SELECT 'SloDWGroupC04',16,1600
  UNION ALL
    SELECT 'SloDWGroupC05',32,3200
  UNION ALL
    SELECT 'SloDWGroupC06',64,6400
  UNION ALL
    SELECT 'SloDWGroupC07',128,12800
)
-- Creating ref based on current / asked DWU.
, ref
AS
(
  SELECT  a1.*
  ,       m1.wg_name          AS wg_name_smallrc
  ,       m1.tgt_mem_grant_MB AS tgt_mem_grant_MB_smallrc
  ,       m2.wg_name          AS wg_name_mediumrc
  ,       m2.tgt_mem_grant_MB AS tgt_mem_grant_MB_mediumrc
  ,       m3.wg_name          AS wg_name_largerc
  ,       m3.tgt_mem_grant_MB AS tgt_mem_grant_MB_largerc
  ,       m4.wg_name          AS wg_name_xlargerc
  ,       m4.tgt_mem_grant_MB AS tgt_mem_grant_MB_xlargerc
  ,       m5.wg_name          AS wg_name_staticrc10
  ,       m5.tgt_mem_grant_MB AS tgt_mem_grant_MB_staticrc10
  ,       m6.wg_name          AS wg_name_staticrc20
  ,       m6.tgt_mem_grant_MB AS tgt_mem_grant_MB_staticrc20
  ,       m7.wg_name          AS wg_name_staticrc30
  ,       m7.tgt_mem_grant_MB AS tgt_mem_grant_MB_staticrc30
  ,       m8.wg_name          AS wg_name_staticrc40
  ,       m8.tgt_mem_grant_MB AS tgt_mem_grant_MB_staticrc40
  ,       m9.wg_name          AS wg_name_staticrc50
  ,       m9.tgt_mem_grant_MB AS tgt_mem_grant_MB_staticrc50
  ,       m10.wg_name          AS wg_name_staticrc60
  ,       m10.tgt_mem_grant_MB AS tgt_mem_grant_MB_staticrc60
  ,       m11.wg_name          AS wg_name_staticrc70
  ,       m11.tgt_mem_grant_MB AS tgt_mem_grant_MB_staticrc70
  ,       m12.wg_name          AS wg_name_staticrc80
  ,       m12.tgt_mem_grant_MB AS tgt_mem_grant_MB_staticrc80
  FROM alloc a1
  JOIN map   m1  ON a1.slots_used_smallrc     = m1.slots_used
  JOIN map   m2  ON a1.slots_used_mediumrc    = m2.slots_used
  JOIN map   m3  ON a1.slots_used_largerc     = m3.slots_used
  JOIN map   m4  ON a1.slots_used_xlargerc    = m4.slots_used
  JOIN map   m5  ON a1.slots_used_staticrc10    = m5.slots_used
  JOIN map   m6  ON a1.slots_used_staticrc20    = m6.slots_used
  JOIN map   m7  ON a1.slots_used_staticrc30    = m7.slots_used
  JOIN map   m8  ON a1.slots_used_staticrc40    = m8.slots_used
  JOIN map   m9  ON a1.slots_used_staticrc50    = m9.slots_used
  JOIN map   m10  ON a1.slots_used_staticrc60    = m10.slots_used
  JOIN map   m11  ON a1.slots_used_staticrc70    = m11.slots_used
  JOIN map   m12  ON a1.slots_used_staticrc80    = m12.slots_used
-- WHERE   a1.DWU = @DWU
  WHERE   a1.DWU = UPPER(@DWU)
)
SELECT  DWU
,       max_queries
,       max_slots
,       slots_used
,       wg_name
,       tgt_mem_grant_MB
,       up1 as rc
,       (ROW_NUMBER() OVER(PARTITION BY DWU ORDER BY DWU)) as rc_id
FROM
(
    SELECT  DWU
    ,       max_queries
    ,       max_slots
    ,       slots_used
    ,       wg_name
    ,       tgt_mem_grant_MB
    ,       REVERSE(SUBSTRING(REVERSE(wg_names),1,CHARINDEX('_',REVERSE(wg_names),1)-1)) as up1
    ,       REVERSE(SUBSTRING(REVERSE(tgt_mem_grant_MBs),1,CHARINDEX('_',REVERSE(tgt_mem_grant_MBs),1)-1)) as up2
    ,       REVERSE(SUBSTRING(REVERSE(slots_used_all),1,CHARINDEX('_',REVERSE(slots_used_all),1)-1)) as up3
    FROM    ref AS r1
    UNPIVOT
    (
        wg_name FOR wg_names IN (wg_name_smallrc,wg_name_mediumrc,wg_name_largerc,wg_name_xlargerc,
        wg_name_staticrc10, wg_name_staticrc20, wg_name_staticrc30, wg_name_staticrc40, wg_name_staticrc50,
        wg_name_staticrc60, wg_name_staticrc70, wg_name_staticrc80)
    ) AS r2
    UNPIVOT
    (
        tgt_mem_grant_MB FOR tgt_mem_grant_MBs IN (tgt_mem_grant_MB_smallrc,tgt_mem_grant_MB_mediumrc,
        tgt_mem_grant_MB_largerc,tgt_mem_grant_MB_xlargerc, tgt_mem_grant_MB_staticrc10, tgt_mem_grant_MB_staticrc20,
        tgt_mem_grant_MB_staticrc30, tgt_mem_grant_MB_staticrc40, tgt_mem_grant_MB_staticrc50,
        tgt_mem_grant_MB_staticrc60, tgt_mem_grant_MB_staticrc70, tgt_mem_grant_MB_staticrc80)
    ) AS r3
    UNPIVOT
    (
        slots_used FOR slots_used_all IN (slots_used_smallrc,slots_used_mediumrc,slots_used_largerc,
        slots_used_xlargerc, slots_used_staticrc10, slots_used_staticrc20, slots_used_staticrc30,
        slots_used_staticrc40, slots_used_staticrc50, slots_used_staticrc60, slots_used_staticrc70,
        slots_used_staticrc80)
    ) AS r4
) a
WHERE   up1 = up2
AND     up1 = up3
;
-- Getting current info about workload groups.
WITH  
dmv  
AS  
(
  SELECT
          rp.name                                           AS rp_name
  ,       rp.max_memory_kb*1.0/1048576                      AS rp_max_mem_GB
  ,       (rp.max_memory_kb*1.0/1024)
          *(request_max_memory_grant_percent/100)           AS max_memory_grant_MB
  ,       (rp.max_memory_kb*1.0/1048576)
          *(request_max_memory_grant_percent/100)           AS max_memory_grant_GB
  ,       wg.name                                           AS wg_name
  ,       wg.importance                                     AS importance
  ,       wg.request_max_memory_grant_percent               AS request_max_memory_grant_percent
  FROM    sys.dm_pdw_nodes_resource_governor_workload_groups wg
  JOIN    sys.dm_pdw_nodes_resource_governor_resource_pools rp    ON  wg.pdw_node_id  = rp.pdw_node_id
                                                                  AND wg.pool_id      = rp.pool_id
  WHERE   rp.name = 'SloDWPool'
  GROUP BY
          rp.name
  ,       rp.max_memory_kb
  ,       wg.name
  ,       wg.importance
  ,       wg.request_max_memory_grant_percent
)
-- Creating resource class name mapping.
,names
AS
(
  SELECT 'smallrc' as resource_class, 1 as rc_id
  UNION ALL
    SELECT 'mediumrc', 2
  UNION ALL
    SELECT 'largerc', 3
  UNION ALL
    SELECT 'xlargerc', 4
  UNION ALL
    SELECT 'staticrc10', 5
  UNION ALL
    SELECT 'staticrc20', 6
  UNION ALL
    SELECT 'staticrc30', 7
  UNION ALL
    SELECT 'staticrc40', 8
  UNION ALL
    SELECT 'staticrc50', 9
  UNION ALL
    SELECT 'staticrc60', 10
  UNION ALL
    SELECT 'staticrc70', 11
  UNION ALL
    SELECT 'staticrc80', 12
)
,base AS
(   SELECT  schema_name
    ,       table_name
    ,       SUM(column_count)                   AS column_count
    ,       ISNULL(SUM(short_string_column_count),0)   AS short_string_column_count
    ,       ISNULL(SUM(long_string_column_count),0)    AS long_string_column_count
    FROM    (   SELECT  sm.name                                             AS schema_name
                ,       tb.name                                             AS table_name
                ,       COUNT(co.column_id)                                 AS column_count
                           ,       CASE    WHEN co.system_type_id IN (36,43,106,108,165,167,173,175,231,239) 
                                AND  co.max_length <= 32 
                                THEN COUNT(co.column_id) 
                        END                                                 AS short_string_column_count
                ,       CASE    WHEN co.system_type_id IN (165,167,173,175,231,239) 
                                AND  co.max_length > 32 and co.max_length <=8000
                                THEN COUNT(co.column_id) 
                        END                                                 AS long_string_column_count
                FROM    sys.schemas AS sm
                JOIN    sys.tables  AS tb   on sm.[schema_id] = tb.[schema_id]
                JOIN    sys.columns AS co   ON tb.[object_id] = co.[object_id]
                           WHERE tb.name = @TABLE_NAME AND sm.name = @SCHEMA_NAME
                GROUP BY sm.name
                ,        tb.name
                ,        co.system_type_id
                ,        co.max_length            ) a
GROUP BY schema_name
,        table_name
)
, size AS
(
SELECT  schema_name
,       table_name
,       75497472                                            AS table_overhead

,       column_count*1048576*8                              AS column_size
,       short_string_column_count*1048576*32                       AS short_string_size,       (long_string_column_count*16777216) AS long_string_size
FROM    base
UNION
SELECT CASE WHEN COUNT(*) = 0 THEN 'EMPTY' END as schema_name
         ,CASE WHEN COUNT(*) = 0 THEN 'EMPTY' END as table_name
         ,CASE WHEN COUNT(*) = 0 THEN 0 END as table_overhead
         ,CASE WHEN COUNT(*) = 0 THEN 0 END as column_size
         ,CASE WHEN COUNT(*) = 0 THEN 0 END as short_string_size

,CASE WHEN COUNT(*) = 0 THEN 0 END as long_string_size
FROM   base
)
, load_multiplier as 
(
SELECT  CASE 
                     WHEN FLOOR(8 * (CAST (@DWU_NUM AS FLOAT)/6000)) > 0 THEN FLOOR(8 * (CAST (@DWU_NUM AS FLOAT)/6000)) 
                     ELSE 1 
              END AS multipliplication_factor
) 
       SELECT  r1.DWU
       , schema_name
       , table_name
       , rc.resource_class as closest_rc_in_increasing_order
       , max_queries_at_this_rc = CASE
             WHEN (r1.max_slots / r1.slots_used > r1.max_queries)
                  THEN r1.max_queries
             ELSE r1.max_slots / r1.slots_used
                  END
       , r1.max_slots as max_concurrency_slots
       , r1.slots_used as required_slots_for_the_rc
       , r1.tgt_mem_grant_MB  as rc_mem_grant_MB
       , CAST((table_overhead*1.0+column_size+short_string_size+long_string_size)*multipliplication_factor/1048576    AS DECIMAL(18,2)) AS est_mem_grant_required_for_cci_operation_MB       
       FROM    size, load_multiplier, #ref r1, names  rc
       WHERE r1.rc_id=rc.rc_id
                     AND CAST((table_overhead*1.0+column_size+short_string_size+long_string_size)*multipliplication_factor/1048576    AS DECIMAL(18,2)) < r1.tgt_mem_grant_MB
       ORDER BY ABS(CAST((table_overhead*1.0+column_size+short_string_size+long_string_size)*multipliplication_factor/1048576    AS DECIMAL(18,2)) - r1.tgt_mem_grant_MB)
GO
```

## <a name="query-importance"></a><span data-ttu-id="cd346-757">Význam dotazu</span><span class="sxs-lookup"><span data-stu-id="cd346-757">Query importance</span></span>
<span data-ttu-id="cd346-758">SQL Data Warehouse implementuje třídy prostředků pomocí skupiny úloh.</span><span class="sxs-lookup"><span data-stu-id="cd346-758">SQL Data Warehouse implements resource classes by using workload groups.</span></span> <span data-ttu-id="cd346-759">Existují celkem osm skupiny úloh, které ovládání chování třídy prostředků pomocí různých velikostí DWU.</span><span class="sxs-lookup"><span data-stu-id="cd346-759">There are a total of eight workload groups that control the behavior of the resource classes across the various DWU sizes.</span></span> <span data-ttu-id="cd346-760">Pro všechny DWU SQL Data Warehouse používá jenom čtyři skupiny osm úloh.</span><span class="sxs-lookup"><span data-stu-id="cd346-760">For any DWU, SQL Data Warehouse uses only four of the eight workload groups.</span></span> <span data-ttu-id="cd346-761">To dává smysl, protože každé skupiny úlohy je přiřazen k jedné ze čtyř tříd prostředků: smallrc, mediumrc, largerc, nebo xlargerc.</span><span class="sxs-lookup"><span data-stu-id="cd346-761">This makes sense because each workload group is assigned to one of four resource classes: smallrc, mediumrc, largerc, or xlargerc.</span></span> <span data-ttu-id="cd346-762">Význam pochopení skupiny úloh je, že některé z těchto skupin úloh jsou nastaveny na vyšší *důležitosti*.</span><span class="sxs-lookup"><span data-stu-id="cd346-762">The importance of understanding the workload groups is that some of these workload groups are set to higher *importance*.</span></span> <span data-ttu-id="cd346-763">Důležitost se používá pro procesoru plánování.</span><span class="sxs-lookup"><span data-stu-id="cd346-763">Importance is used for CPU scheduling.</span></span> <span data-ttu-id="cd346-764">Dotazy spustit s vysokou důležitostí získají třikrát více cyklů procesoru než ty, které se střední důležitostí.</span><span class="sxs-lookup"><span data-stu-id="cd346-764">Queries run with high importance will get three times more CPU cycles than those with medium importance.</span></span> <span data-ttu-id="cd346-765">Proto souběžnosti slotu mapování taky určit Priorita procesoru.</span><span class="sxs-lookup"><span data-stu-id="cd346-765">Therefore, concurrency slot mappings also determine CPU priority.</span></span> <span data-ttu-id="cd346-766">Pokud je dotaz spotřebovává 16 nebo víc sloty, běží jako vysokou důležitostí.</span><span class="sxs-lookup"><span data-stu-id="cd346-766">When a query consumes 16 or more slots, it runs as high importance.</span></span>

<span data-ttu-id="cd346-767">Následující tabulka uvádí důležitosti mapování pro každou skupinu úloh.</span><span class="sxs-lookup"><span data-stu-id="cd346-767">The following table shows the importance mappings for each workload group.</span></span>

### <a name="workload-group-mappings-to-concurrency-slots-and-importance"></a><span data-ttu-id="cd346-768">Mapování skupin úloh souběžnosti sloty a význam</span><span class="sxs-lookup"><span data-stu-id="cd346-768">Workload group mappings to concurrency slots and importance</span></span>
| <span data-ttu-id="cd346-769">Skupiny úloh</span><span class="sxs-lookup"><span data-stu-id="cd346-769">Workload groups</span></span> | <span data-ttu-id="cd346-770">Mapování slotu souběžnosti</span><span class="sxs-lookup"><span data-stu-id="cd346-770">Concurrency slot mapping</span></span> | <span data-ttu-id="cd346-771">MB / distribuce</span><span class="sxs-lookup"><span data-stu-id="cd346-771">MB / Distribution</span></span> | <span data-ttu-id="cd346-772">Mapování důležitostí</span><span class="sxs-lookup"><span data-stu-id="cd346-772">Importance mapping</span></span> |
|:--- |:---:|:---:|:--- |
| <span data-ttu-id="cd346-773">SloDWGroupC00</span><span class="sxs-lookup"><span data-stu-id="cd346-773">SloDWGroupC00</span></span> |<span data-ttu-id="cd346-774">1</span><span class="sxs-lookup"><span data-stu-id="cd346-774">1</span></span> |<span data-ttu-id="cd346-775">100</span><span class="sxs-lookup"><span data-stu-id="cd346-775">100</span></span> |<span data-ttu-id="cd346-776">Střednědobé používání</span><span class="sxs-lookup"><span data-stu-id="cd346-776">Medium</span></span> |
| <span data-ttu-id="cd346-777">SloDWGroupC01</span><span class="sxs-lookup"><span data-stu-id="cd346-777">SloDWGroupC01</span></span> |<span data-ttu-id="cd346-778">2</span><span class="sxs-lookup"><span data-stu-id="cd346-778">2</span></span> |<span data-ttu-id="cd346-779">200</span><span class="sxs-lookup"><span data-stu-id="cd346-779">200</span></span> |<span data-ttu-id="cd346-780">Střednědobé používání</span><span class="sxs-lookup"><span data-stu-id="cd346-780">Medium</span></span> |
| <span data-ttu-id="cd346-781">SloDWGroupC02</span><span class="sxs-lookup"><span data-stu-id="cd346-781">SloDWGroupC02</span></span> |<span data-ttu-id="cd346-782">4</span><span class="sxs-lookup"><span data-stu-id="cd346-782">4</span></span> |<span data-ttu-id="cd346-783">400</span><span class="sxs-lookup"><span data-stu-id="cd346-783">400</span></span> |<span data-ttu-id="cd346-784">Střednědobé používání</span><span class="sxs-lookup"><span data-stu-id="cd346-784">Medium</span></span> |
| <span data-ttu-id="cd346-785">SloDWGroupC03</span><span class="sxs-lookup"><span data-stu-id="cd346-785">SloDWGroupC03</span></span> |<span data-ttu-id="cd346-786">8</span><span class="sxs-lookup"><span data-stu-id="cd346-786">8</span></span> |<span data-ttu-id="cd346-787">800</span><span class="sxs-lookup"><span data-stu-id="cd346-787">800</span></span> |<span data-ttu-id="cd346-788">Střednědobé používání</span><span class="sxs-lookup"><span data-stu-id="cd346-788">Medium</span></span> |
| <span data-ttu-id="cd346-789">SloDWGroupC04</span><span class="sxs-lookup"><span data-stu-id="cd346-789">SloDWGroupC04</span></span> |<span data-ttu-id="cd346-790">16</span><span class="sxs-lookup"><span data-stu-id="cd346-790">16</span></span> |<span data-ttu-id="cd346-791">1,600</span><span class="sxs-lookup"><span data-stu-id="cd346-791">1,600</span></span> |<span data-ttu-id="cd346-792">Vysoký</span><span class="sxs-lookup"><span data-stu-id="cd346-792">High</span></span> |
| <span data-ttu-id="cd346-793">SloDWGroupC05</span><span class="sxs-lookup"><span data-stu-id="cd346-793">SloDWGroupC05</span></span> |<span data-ttu-id="cd346-794">32</span><span class="sxs-lookup"><span data-stu-id="cd346-794">32</span></span> |<span data-ttu-id="cd346-795">3,200</span><span class="sxs-lookup"><span data-stu-id="cd346-795">3,200</span></span> |<span data-ttu-id="cd346-796">Vysoký</span><span class="sxs-lookup"><span data-stu-id="cd346-796">High</span></span> |
| <span data-ttu-id="cd346-797">SloDWGroupC06</span><span class="sxs-lookup"><span data-stu-id="cd346-797">SloDWGroupC06</span></span> |<span data-ttu-id="cd346-798">64</span><span class="sxs-lookup"><span data-stu-id="cd346-798">64</span></span> |<span data-ttu-id="cd346-799">6,400</span><span class="sxs-lookup"><span data-stu-id="cd346-799">6,400</span></span> |<span data-ttu-id="cd346-800">Vysoký</span><span class="sxs-lookup"><span data-stu-id="cd346-800">High</span></span> |
| <span data-ttu-id="cd346-801">SloDWGroupC07</span><span class="sxs-lookup"><span data-stu-id="cd346-801">SloDWGroupC07</span></span> |<span data-ttu-id="cd346-802">128</span><span class="sxs-lookup"><span data-stu-id="cd346-802">128</span></span> |<span data-ttu-id="cd346-803">12,800</span><span class="sxs-lookup"><span data-stu-id="cd346-803">12,800</span></span> |<span data-ttu-id="cd346-804">Vysoký</span><span class="sxs-lookup"><span data-stu-id="cd346-804">High</span></span> |

<span data-ttu-id="cd346-805">Z **přidělení a využití souběžnosti přihrádky** grafu, zjistíte, že DW500 používá 1, 4, 8 nebo 16 souběžnosti sloty pro smallrc, mediumrc, largerc a xlargerc, v uvedeném pořadí.</span><span class="sxs-lookup"><span data-stu-id="cd346-805">From the **Allocation and consumption of concurrency slots** chart, you can see that a DW500 uses 1, 4, 8 or 16 concurrency slots for smallrc, mediumrc, largerc, and xlargerc, respectively.</span></span> <span data-ttu-id="cd346-806">Tyto hodnoty lze vyhledat v předchozí tabulce najít význam pro každou třídu prostředků.</span><span class="sxs-lookup"><span data-stu-id="cd346-806">You can look those values up in the preceding chart to find the importance for each resource class.</span></span>

### <a name="dw500-mapping-of-resource-classes-to-importance"></a><span data-ttu-id="cd346-807">DW500 mapování třídy prostředků na význam</span><span class="sxs-lookup"><span data-stu-id="cd346-807">DW500 mapping of resource classes to importance</span></span>
| <span data-ttu-id="cd346-808">Třída prostředků</span><span class="sxs-lookup"><span data-stu-id="cd346-808">Resource class</span></span> | <span data-ttu-id="cd346-809">Skupina úlohy</span><span class="sxs-lookup"><span data-stu-id="cd346-809">Workload group</span></span> | <span data-ttu-id="cd346-810">Použít sloty souběžnosti</span><span class="sxs-lookup"><span data-stu-id="cd346-810">Concurrency slots used</span></span> | <span data-ttu-id="cd346-811">MB / distribuce</span><span class="sxs-lookup"><span data-stu-id="cd346-811">MB / Distribution</span></span> | <span data-ttu-id="cd346-812">Význam</span><span class="sxs-lookup"><span data-stu-id="cd346-812">Importance</span></span> |
|:--- |:--- |:---:|:---:|:--- |
| <span data-ttu-id="cd346-813">smallrc</span><span class="sxs-lookup"><span data-stu-id="cd346-813">smallrc</span></span> |<span data-ttu-id="cd346-814">SloDWGroupC00</span><span class="sxs-lookup"><span data-stu-id="cd346-814">SloDWGroupC00</span></span> |<span data-ttu-id="cd346-815">1</span><span class="sxs-lookup"><span data-stu-id="cd346-815">1</span></span> |<span data-ttu-id="cd346-816">100</span><span class="sxs-lookup"><span data-stu-id="cd346-816">100</span></span> |<span data-ttu-id="cd346-817">Střednědobé používání</span><span class="sxs-lookup"><span data-stu-id="cd346-817">Medium</span></span> |
| <span data-ttu-id="cd346-818">mediumrc</span><span class="sxs-lookup"><span data-stu-id="cd346-818">mediumrc</span></span> |<span data-ttu-id="cd346-819">SloDWGroupC02</span><span class="sxs-lookup"><span data-stu-id="cd346-819">SloDWGroupC02</span></span> |<span data-ttu-id="cd346-820">4</span><span class="sxs-lookup"><span data-stu-id="cd346-820">4</span></span> |<span data-ttu-id="cd346-821">400</span><span class="sxs-lookup"><span data-stu-id="cd346-821">400</span></span> |<span data-ttu-id="cd346-822">Střednědobé používání</span><span class="sxs-lookup"><span data-stu-id="cd346-822">Medium</span></span> |
| <span data-ttu-id="cd346-823">largerc</span><span class="sxs-lookup"><span data-stu-id="cd346-823">largerc</span></span> |<span data-ttu-id="cd346-824">SloDWGroupC03</span><span class="sxs-lookup"><span data-stu-id="cd346-824">SloDWGroupC03</span></span> |<span data-ttu-id="cd346-825">8</span><span class="sxs-lookup"><span data-stu-id="cd346-825">8</span></span> |<span data-ttu-id="cd346-826">800</span><span class="sxs-lookup"><span data-stu-id="cd346-826">800</span></span> |<span data-ttu-id="cd346-827">Střednědobé používání</span><span class="sxs-lookup"><span data-stu-id="cd346-827">Medium</span></span> |
| <span data-ttu-id="cd346-828">xlargerc</span><span class="sxs-lookup"><span data-stu-id="cd346-828">xlargerc</span></span> |<span data-ttu-id="cd346-829">SloDWGroupC04</span><span class="sxs-lookup"><span data-stu-id="cd346-829">SloDWGroupC04</span></span> |<span data-ttu-id="cd346-830">16</span><span class="sxs-lookup"><span data-stu-id="cd346-830">16</span></span> |<span data-ttu-id="cd346-831">1,600</span><span class="sxs-lookup"><span data-stu-id="cd346-831">1,600</span></span> |<span data-ttu-id="cd346-832">Vysoký</span><span class="sxs-lookup"><span data-stu-id="cd346-832">High</span></span> |
| <span data-ttu-id="cd346-833">staticrc10</span><span class="sxs-lookup"><span data-stu-id="cd346-833">staticrc10</span></span> |<span data-ttu-id="cd346-834">SloDWGroupC00</span><span class="sxs-lookup"><span data-stu-id="cd346-834">SloDWGroupC00</span></span> |<span data-ttu-id="cd346-835">1</span><span class="sxs-lookup"><span data-stu-id="cd346-835">1</span></span> |<span data-ttu-id="cd346-836">100</span><span class="sxs-lookup"><span data-stu-id="cd346-836">100</span></span> |<span data-ttu-id="cd346-837">Střednědobé používání</span><span class="sxs-lookup"><span data-stu-id="cd346-837">Medium</span></span> |
| <span data-ttu-id="cd346-838">staticrc20</span><span class="sxs-lookup"><span data-stu-id="cd346-838">staticrc20</span></span> |<span data-ttu-id="cd346-839">SloDWGroupC01</span><span class="sxs-lookup"><span data-stu-id="cd346-839">SloDWGroupC01</span></span> |<span data-ttu-id="cd346-840">2</span><span class="sxs-lookup"><span data-stu-id="cd346-840">2</span></span> |<span data-ttu-id="cd346-841">200</span><span class="sxs-lookup"><span data-stu-id="cd346-841">200</span></span> |<span data-ttu-id="cd346-842">Střednědobé používání</span><span class="sxs-lookup"><span data-stu-id="cd346-842">Medium</span></span> |
| <span data-ttu-id="cd346-843">staticrc30</span><span class="sxs-lookup"><span data-stu-id="cd346-843">staticrc30</span></span> |<span data-ttu-id="cd346-844">SloDWGroupC02</span><span class="sxs-lookup"><span data-stu-id="cd346-844">SloDWGroupC02</span></span> |<span data-ttu-id="cd346-845">4</span><span class="sxs-lookup"><span data-stu-id="cd346-845">4</span></span> |<span data-ttu-id="cd346-846">400</span><span class="sxs-lookup"><span data-stu-id="cd346-846">400</span></span> |<span data-ttu-id="cd346-847">Střednědobé používání</span><span class="sxs-lookup"><span data-stu-id="cd346-847">Medium</span></span> |
| <span data-ttu-id="cd346-848">staticrc40</span><span class="sxs-lookup"><span data-stu-id="cd346-848">staticrc40</span></span> |<span data-ttu-id="cd346-849">SloDWGroupC03</span><span class="sxs-lookup"><span data-stu-id="cd346-849">SloDWGroupC03</span></span> |<span data-ttu-id="cd346-850">8</span><span class="sxs-lookup"><span data-stu-id="cd346-850">8</span></span> |<span data-ttu-id="cd346-851">800</span><span class="sxs-lookup"><span data-stu-id="cd346-851">800</span></span> |<span data-ttu-id="cd346-852">Střednědobé používání</span><span class="sxs-lookup"><span data-stu-id="cd346-852">Medium</span></span> |
| <span data-ttu-id="cd346-853">staticrc50</span><span class="sxs-lookup"><span data-stu-id="cd346-853">staticrc50</span></span> |<span data-ttu-id="cd346-854">SloDWGroupC03</span><span class="sxs-lookup"><span data-stu-id="cd346-854">SloDWGroupC03</span></span> |<span data-ttu-id="cd346-855">16</span><span class="sxs-lookup"><span data-stu-id="cd346-855">16</span></span> |<span data-ttu-id="cd346-856">1,600</span><span class="sxs-lookup"><span data-stu-id="cd346-856">1,600</span></span> |<span data-ttu-id="cd346-857">Vysoký</span><span class="sxs-lookup"><span data-stu-id="cd346-857">High</span></span> |
| <span data-ttu-id="cd346-858">staticrc60</span><span class="sxs-lookup"><span data-stu-id="cd346-858">staticrc60</span></span> |<span data-ttu-id="cd346-859">SloDWGroupC03</span><span class="sxs-lookup"><span data-stu-id="cd346-859">SloDWGroupC03</span></span> |<span data-ttu-id="cd346-860">16</span><span class="sxs-lookup"><span data-stu-id="cd346-860">16</span></span> |<span data-ttu-id="cd346-861">1,600</span><span class="sxs-lookup"><span data-stu-id="cd346-861">1,600</span></span> |<span data-ttu-id="cd346-862">Vysoký</span><span class="sxs-lookup"><span data-stu-id="cd346-862">High</span></span> |
| <span data-ttu-id="cd346-863">staticrc70</span><span class="sxs-lookup"><span data-stu-id="cd346-863">staticrc70</span></span> |<span data-ttu-id="cd346-864">SloDWGroupC03</span><span class="sxs-lookup"><span data-stu-id="cd346-864">SloDWGroupC03</span></span> |<span data-ttu-id="cd346-865">16</span><span class="sxs-lookup"><span data-stu-id="cd346-865">16</span></span> |<span data-ttu-id="cd346-866">1,600</span><span class="sxs-lookup"><span data-stu-id="cd346-866">1,600</span></span> |<span data-ttu-id="cd346-867">Vysoký</span><span class="sxs-lookup"><span data-stu-id="cd346-867">High</span></span> |
| <span data-ttu-id="cd346-868">staticrc80</span><span class="sxs-lookup"><span data-stu-id="cd346-868">staticrc80</span></span> |<span data-ttu-id="cd346-869">SloDWGroupC03</span><span class="sxs-lookup"><span data-stu-id="cd346-869">SloDWGroupC03</span></span> |<span data-ttu-id="cd346-870">16</span><span class="sxs-lookup"><span data-stu-id="cd346-870">16</span></span> |<span data-ttu-id="cd346-871">1,600</span><span class="sxs-lookup"><span data-stu-id="cd346-871">1,600</span></span> |<span data-ttu-id="cd346-872">Vysoký</span><span class="sxs-lookup"><span data-stu-id="cd346-872">High</span></span> |

<span data-ttu-id="cd346-873">Následující dotaz DMV můžete se podívat na rozdíly v přidělování prostředků paměti podrobně z pohledu správce zdrojů, nebo pro analýzu aktivní a historického využití skupin zatížení při řešení potíží s.</span><span class="sxs-lookup"><span data-stu-id="cd346-873">You can use the following DMV query to look at the differences in memory resource allocation in detail from the perspective of the resource governor, or to analyze active and historic usage of the workload groups when troubleshooting.</span></span>

```sql
WITH rg
AS
(   SELECT  
     pn.name                        AS node_name
    ,pn.[type]                        AS node_type
    ,pn.pdw_node_id                    AS node_id
    ,rp.name                        AS pool_name
    ,rp.max_memory_kb*1.0/1024                AS pool_max_mem_MB
    ,wg.name                        AS group_name
    ,wg.importance                    AS group_importance
    ,wg.request_max_memory_grant_percent        AS group_request_max_memory_grant_pcnt
    ,wg.max_dop                        AS group_max_dop
    ,wg.effective_max_dop                AS group_effective_max_dop
    ,wg.total_request_count                AS group_total_request_count
    ,wg.total_queued_request_count            AS group_total_queued_request_count
    ,wg.active_request_count                AS group_active_request_count
    ,wg.queued_request_count                AS group_queued_request_count
    FROM    sys.dm_pdw_nodes_resource_governor_workload_groups wg
    JOIN    sys.dm_pdw_nodes_resource_governor_resource_pools rp    
            ON  wg.pdw_node_id  = rp.pdw_node_id
            AND wg.pool_id      = rp.pool_id
    JOIN    sys.dm_pdw_nodes pn
            ON    wg.pdw_node_id    = pn.pdw_node_id
    WHERE   wg.name like 'SloDWGroup%'
        AND     rp.name = 'SloDWPool'
)
SELECT    pool_name
,        pool_max_mem_MB
,        group_name
,        group_importance
,        (pool_max_mem_MB/100)*group_request_max_memory_grant_pcnt AS max_memory_grant_MB
,        node_name
,        node_type
,       group_total_request_count
,       group_total_queued_request_count
,       group_active_request_count
,       group_queued_request_count
FROM    rg
ORDER BY
    node_name
,    group_request_max_memory_grant_pcnt
,    group_importance
;
```

## <a name="queries-that-honor-concurrency-limits"></a><span data-ttu-id="cd346-874">Dotazy, které respektovat omezení souběžnosti</span><span class="sxs-lookup"><span data-stu-id="cd346-874">Queries that honor concurrency limits</span></span>
<span data-ttu-id="cd346-875">Třídy prostředků se řídí většina dotazů.</span><span class="sxs-lookup"><span data-stu-id="cd346-875">Most queries are governed by resource classes.</span></span> <span data-ttu-id="cd346-876">Tyto dotazy se musí vejít do obou souběžných dotazů a souběžnost slotu prahové hodnoty.</span><span class="sxs-lookup"><span data-stu-id="cd346-876">These queries must fit inside both the concurrent query and concurrency slot thresholds.</span></span> <span data-ttu-id="cd346-877">Uživatel nemohou zvolit, aby vyloučili dotaz z modelu concurrency slot.</span><span class="sxs-lookup"><span data-stu-id="cd346-877">A user cannot choose to exclude a query from the concurrency slot model.</span></span>

<span data-ttu-id="cd346-878">Chcete-li znovu opakují, následující příkazy respektovat třídy prostředků:</span><span class="sxs-lookup"><span data-stu-id="cd346-878">To reiterate, the following statements honor resource classes:</span></span>

* <span data-ttu-id="cd346-879">PŘÍKAZ INSERT SELECT</span><span class="sxs-lookup"><span data-stu-id="cd346-879">INSERT-SELECT</span></span>
* <span data-ttu-id="cd346-880">AKTUALIZACE</span><span class="sxs-lookup"><span data-stu-id="cd346-880">UPDATE</span></span>
* <span data-ttu-id="cd346-881">ODSTRANIT</span><span class="sxs-lookup"><span data-stu-id="cd346-881">DELETE</span></span>
* <span data-ttu-id="cd346-882">Vyberte (při dotazování tabulky uživatelů)</span><span class="sxs-lookup"><span data-stu-id="cd346-882">SELECT (when querying user tables)</span></span>
* <span data-ttu-id="cd346-883">PŘÍKAZ ALTER INDEX OPĚTOVNÉ SESTAVENÍ</span><span class="sxs-lookup"><span data-stu-id="cd346-883">ALTER INDEX REBUILD</span></span>
* <span data-ttu-id="cd346-884">PŘÍKAZ ALTER INDEX REORGANIZE</span><span class="sxs-lookup"><span data-stu-id="cd346-884">ALTER INDEX REORGANIZE</span></span>
* <span data-ttu-id="cd346-885">PŘÍKAZ ALTER TABLE OPĚTOVNÉ SESTAVENÍ</span><span class="sxs-lookup"><span data-stu-id="cd346-885">ALTER TABLE REBUILD</span></span>
* <span data-ttu-id="cd346-886">VYTVOŘENÍ INDEXU</span><span class="sxs-lookup"><span data-stu-id="cd346-886">CREATE INDEX</span></span>
* <span data-ttu-id="cd346-887">VYTVOŘIT CLUSTEROVANÝ INDEX COLUMNSTORE</span><span class="sxs-lookup"><span data-stu-id="cd346-887">CREATE CLUSTERED COLUMNSTORE INDEX</span></span>
* <span data-ttu-id="cd346-888">VYTVOŘENÍ TABLE AS SELECT (FUNKCE CTAS)</span><span class="sxs-lookup"><span data-stu-id="cd346-888">CREATE TABLE AS SELECT (CTAS)</span></span>
* <span data-ttu-id="cd346-889">Načítání dat</span><span class="sxs-lookup"><span data-stu-id="cd346-889">Data loading</span></span>
* <span data-ttu-id="cd346-890">Operace přesunu dat prováděné pomocí služby přesun dat (DMS)</span><span class="sxs-lookup"><span data-stu-id="cd346-890">Data movement operations conducted by the Data Movement Service (DMS)</span></span>

## <a name="query-exceptions-to-concurrency-limits"></a><span data-ttu-id="cd346-891">Dotaz výjimky souběžnosti omezení</span><span class="sxs-lookup"><span data-stu-id="cd346-891">Query exceptions to concurrency limits</span></span>
<span data-ttu-id="cd346-892">Některé dotazy nerespektují Třída prostředků, ke kterému je přiřazena uživateli.</span><span class="sxs-lookup"><span data-stu-id="cd346-892">Some queries do not honor the resource class to which the user is assigned.</span></span> <span data-ttu-id="cd346-893">Tyto výjimky souběžnosti limitům jsou vytvářeny, pokud paměťové prostředky potřebné pro konkrétní příkaz nízká, často vzhledem k tomu, že příkaz je operace metadat.</span><span class="sxs-lookup"><span data-stu-id="cd346-893">These exceptions to the concurrency limits are made when the memory resources needed for a particular command are low, often because the command is a metadata operation.</span></span> <span data-ttu-id="cd346-894">Cílem tyto výjimky se vyhnete větší přidělení paměti pro dotazy, které je nikdy nebudete potřebovat.</span><span class="sxs-lookup"><span data-stu-id="cd346-894">The goal of these exceptions is to avoid larger memory allocations for queries that will never need them.</span></span> <span data-ttu-id="cd346-895">V těchto případech je výchozí třídu malé prostředků (smallrc) vždycky použijí bez ohledu na to třída skutečné prostředků, které jsou přiřazeny uživateli.</span><span class="sxs-lookup"><span data-stu-id="cd346-895">In these cases, the default small resource class (smallrc) is always used regardless of the actual resource class assigned to the user.</span></span> <span data-ttu-id="cd346-896">Například `CREATE LOGIN` bude vždy spuštěn v smallrc.</span><span class="sxs-lookup"><span data-stu-id="cd346-896">For example, `CREATE LOGIN` will always run in smallrc.</span></span> <span data-ttu-id="cd346-897">Prostředky potřebný ke splnění této operace jsou velmi nízkou, takže ho nemá smysl chcete zahrnout do modelu concurrency slotu dotazu.</span><span class="sxs-lookup"><span data-stu-id="cd346-897">The resources required to fulfill this operation are very low, so it does not make sense to include the query in the concurrency slot model.</span></span>  <span data-ttu-id="cd346-898">Tyto dotazy nejsou také omezena souběžnosti limit 32 uživatele, na omezení relace 1 024 relací, které můžete spustit neomezený počet tyto dotazy.</span><span class="sxs-lookup"><span data-stu-id="cd346-898">These queries are also not limited by the 32 user concurrency limit, an unlimited number of these queries can run up to the session limit of 1,024 sessions.</span></span>

<span data-ttu-id="cd346-899">Následující příkazy nerespektují třídy prostředků:</span><span class="sxs-lookup"><span data-stu-id="cd346-899">The following statements do not honor resource classes:</span></span>

* <span data-ttu-id="cd346-900">Vytvořit nebo VYŘADIT tabulku</span><span class="sxs-lookup"><span data-stu-id="cd346-900">CREATE or DROP TABLE</span></span>
* <span data-ttu-id="cd346-901">PŘÍKAZ ALTER TABLE... PŘEPÍNAČE, rozdělení nebo sloučit oddíl</span><span class="sxs-lookup"><span data-stu-id="cd346-901">ALTER TABLE ... SWITCH, SPLIT, or MERGE PARTITION</span></span>
* <span data-ttu-id="cd346-902">PŘÍKAZ ALTER INDEX ZAKÁZAT</span><span class="sxs-lookup"><span data-stu-id="cd346-902">ALTER INDEX DISABLE</span></span>
* <span data-ttu-id="cd346-903">DROP INDEX</span><span class="sxs-lookup"><span data-stu-id="cd346-903">DROP INDEX</span></span>
* <span data-ttu-id="cd346-904">Vytvoření, aktualizace a použít příkaz DROP STATISTICS</span><span class="sxs-lookup"><span data-stu-id="cd346-904">CREATE, UPDATE, or DROP STATISTICS</span></span>
* <span data-ttu-id="cd346-905">ZKRÁCENÍ TABULKY</span><span class="sxs-lookup"><span data-stu-id="cd346-905">TRUNCATE TABLE</span></span>
* <span data-ttu-id="cd346-906">PŘÍKAZ ALTER AUTORIZACE</span><span class="sxs-lookup"><span data-stu-id="cd346-906">ALTER AUTHORIZATION</span></span>
* <span data-ttu-id="cd346-907">VYTVOŘIT PŘIHLÁŠENÍ</span><span class="sxs-lookup"><span data-stu-id="cd346-907">CREATE LOGIN</span></span>
* <span data-ttu-id="cd346-908">CREATE, ALTER nebo DROP USER</span><span class="sxs-lookup"><span data-stu-id="cd346-908">CREATE, ALTER or DROP USER</span></span>
* <span data-ttu-id="cd346-909">CREATE, ALTER nebo VYŘADIT postup</span><span class="sxs-lookup"><span data-stu-id="cd346-909">CREATE, ALTER or DROP PROCEDURE</span></span>
* <span data-ttu-id="cd346-910">Vytvořit nebo VYŘADIT zobrazení</span><span class="sxs-lookup"><span data-stu-id="cd346-910">CREATE or DROP VIEW</span></span>
* <span data-ttu-id="cd346-911">VLOŽENÍ HODNOTY</span><span class="sxs-lookup"><span data-stu-id="cd346-911">INSERT VALUES</span></span>
* <span data-ttu-id="cd346-912">Vyberte z systémová zobrazení a zobrazení dynamické správy</span><span class="sxs-lookup"><span data-stu-id="cd346-912">SELECT from system views and DMVs</span></span>
* <span data-ttu-id="cd346-913">VYSVĚTLUJÍ</span><span class="sxs-lookup"><span data-stu-id="cd346-913">EXPLAIN</span></span>
* <span data-ttu-id="cd346-914">PŘÍKAZ DBCC</span><span class="sxs-lookup"><span data-stu-id="cd346-914">DBCC</span></span>

<!--
Removed as these two are not confirmed / supported under SQLDW
- CREATE REMOTE TABLE AS SELECT
- CREATE EXTERNAL TABLE AS SELECT
- REDISTRIBUTE
-->

##  <span data-ttu-id="cd346-915"><a name="changing-user-resource-class-example"></a>Změnit v příkladu třída prostředků uživatele</span><span class="sxs-lookup"><span data-stu-id="cd346-915"><a name="changing-user-resource-class-example"></a> Change a user resource class example</span></span>
1. <span data-ttu-id="cd346-916">**Vytvořit přihlášení:** otevřít připojení k vaší **hlavní** databáze na SQL server hostující databázi SQL Data Warehouse a spusťte následující příkazy.</span><span class="sxs-lookup"><span data-stu-id="cd346-916">**Create login:** Open a connection to your **master** database on the SQL server hosting your SQL Data Warehouse database and execute the following commands.</span></span>
   
    ```sql
    CREATE LOGIN newperson WITH PASSWORD = 'mypassword';
    CREATE USER newperson for LOGIN newperson;
    ```
   
   > [!NOTE]
   > <span data-ttu-id="cd346-917">Je vhodné vytvořit uživateli v hlavní databázi pro uživatele Azure SQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="cd346-917">It is a good idea to create a user in the master database for Azure SQL Data Warehouse users.</span></span> <span data-ttu-id="cd346-918">Vytváření uživatele v předloze umožňuje uživateli přihlášení pomocí nástroje, například aplikace SSMS bez určení názvu databáze.</span><span class="sxs-lookup"><span data-stu-id="cd346-918">Creating a user in master allows a user to login using tools like SSMS without specifying a database name.</span></span>  <span data-ttu-id="cd346-919">Také to umožňuje, aby uživatelé používali Průzkumník objektů pokud chcete zobrazit všechny databáze na serveru SQL server.</span><span class="sxs-lookup"><span data-stu-id="cd346-919">It also allows them to use the object explorer to view all databases on a SQL server.</span></span>  <span data-ttu-id="cd346-920">Další informace o vytváření a správě uživatelů najdete v tématu [zabezpečení databáze v SQL Data Warehouse][Secure a database in SQL Data Warehouse].</span><span class="sxs-lookup"><span data-stu-id="cd346-920">For more details about creating and managing users, see [Secure a database in SQL Data Warehouse][Secure a database in SQL Data Warehouse].</span></span>
   > 
   > 
2. <span data-ttu-id="cd346-921">**Vytvoření SQL Data Warehouse uživatele:** otevřít připojení k **SQL Data Warehouse** databáze a spusťte následující příkaz.</span><span class="sxs-lookup"><span data-stu-id="cd346-921">**Create SQL Data Warehouse user:** Open a connection to the **SQL Data Warehouse** database and execute the following command.</span></span>
   
    ```sql
    CREATE USER newperson FOR LOGIN newperson;
    ```
3. <span data-ttu-id="cd346-922">**Udělení oprávnění:** následující příklad povolí `CONTROL` na **SQL Data Warehouse** databáze.</span><span class="sxs-lookup"><span data-stu-id="cd346-922">**Grant permissions:** The following example grants `CONTROL` on the **SQL Data Warehouse** database.</span></span> <span data-ttu-id="cd346-923">`CONTROL`v databázi úroveň je ekvivalentem db_owner v systému SQL Server.</span><span class="sxs-lookup"><span data-stu-id="cd346-923">`CONTROL` at the database level is the equivalent of db_owner in SQL Server.</span></span>
   
    ```sql
    GRANT CONTROL ON DATABASE::MySQLDW to newperson;
    ```
4. <span data-ttu-id="cd346-924">**Zvýšit Třída prostředků:** následující dotaz použít k přidání uživatele do role vyšší úlohy správy.</span><span class="sxs-lookup"><span data-stu-id="cd346-924">**Increase resource class:** Use the following query to add a user to a higher workload management role.</span></span>
   
    ```sql
    EXEC sp_addrolemember 'largerc', 'newperson'
    ```
5. <span data-ttu-id="cd346-925">**Třída prostředků snížit:** odebrat uživatele z role, úlohy správy pomocí následujícího dotazu.</span><span class="sxs-lookup"><span data-stu-id="cd346-925">**Decrease resource class:** Use the following query to remove a user from a workload management role.</span></span>
   
    ```sql
    EXEC sp_droprolemember 'largerc', 'newperson';
    ```
   
   > [!NOTE]
   > <span data-ttu-id="cd346-926">Není možné odebrat uživatele z smallrc.</span><span class="sxs-lookup"><span data-stu-id="cd346-926">It is not possible to remove a user from smallrc.</span></span>
   > 
   > 

## <a name="queued-query-detection-and-other-dmvs"></a><span data-ttu-id="cd346-927">Detekce ve frontě dotazů a jiné zobrazení dynamické správy</span><span class="sxs-lookup"><span data-stu-id="cd346-927">Queued query detection and other DMVs</span></span>
<span data-ttu-id="cd346-928">Můžete použít `sys.dm_pdw_exec_requests` DMV k identifikaci dotazy, které jsou čekajících ve frontě souběžnosti.</span><span class="sxs-lookup"><span data-stu-id="cd346-928">You can use the `sys.dm_pdw_exec_requests` DMV to identify queries that are waiting in a concurrency queue.</span></span> <span data-ttu-id="cd346-929">Dotazuje čekání slot souběžnosti bude mít stav **pozastaveno**.</span><span class="sxs-lookup"><span data-stu-id="cd346-929">Queries waiting for a concurrency slot will have a status of **suspended**.</span></span>

```sql
SELECT      r.[request_id]                 AS Request_ID
        ,r.[status]                 AS Request_Status
        ,r.[submit_time]             AS Request_SubmitTime
        ,r.[start_time]                 AS Request_StartTime
        ,DATEDIFF(ms,[submit_time],[start_time]) AS Request_InitiateDuration_ms
        ,r.resource_class                         AS Request_resource_class
FROM    sys.dm_pdw_exec_requests r;
```

<span data-ttu-id="cd346-930">Role správy úloh lze zobrazit pomocí `sys.database_principals`.</span><span class="sxs-lookup"><span data-stu-id="cd346-930">Workload management roles can be viewed with `sys.database_principals`.</span></span>

```sql
SELECT  ro.[name]           AS [db_role_name]
FROM    sys.database_principals ro
WHERE   ro.[type_desc]      = 'DATABASE_ROLE'
AND     ro.[is_fixed_role]  = 0;
```

<span data-ttu-id="cd346-931">Následující dotaz uvádí role, které každý uživatel je přiřazen k.</span><span class="sxs-lookup"><span data-stu-id="cd346-931">The following query shows which role each user is assigned to.</span></span>

```sql
SELECT     r.name AS role_principal_name
        ,m.name AS member_principal_name
FROM    sys.database_role_members rm
JOIN    sys.database_principals AS r            ON rm.role_principal_id        = r.principal_id
JOIN    sys.database_principals AS m            ON rm.member_principal_id    = m.principal_id
WHERE    r.name IN ('mediumrc','largerc', 'xlargerc');
```

<span data-ttu-id="cd346-932">SQL Data Warehouse má následující počkejte typy:</span><span class="sxs-lookup"><span data-stu-id="cd346-932">SQL Data Warehouse has the following wait types:</span></span>

* <span data-ttu-id="cd346-933">**LocalQueriesConcurrencyResourceType**: dotazy, které se nacházejí mimo rozhraní slotu souběžnosti.</span><span class="sxs-lookup"><span data-stu-id="cd346-933">**LocalQueriesConcurrencyResourceType**: Queries that sit outside of the concurrency slot framework.</span></span> <span data-ttu-id="cd346-934">DMV dotazy a systému funkce, jako `SELECT @@VERSION` jsou příklady místní dotazů.</span><span class="sxs-lookup"><span data-stu-id="cd346-934">DMV queries and system functions such as `SELECT @@VERSION` are examples of local queries.</span></span>
* <span data-ttu-id="cd346-935">**UserConcurrencyResourceType**: dotazy, které se nacházejí v rozhraní framework slotu souběžnosti.</span><span class="sxs-lookup"><span data-stu-id="cd346-935">**UserConcurrencyResourceType**: Queries that sit inside the concurrency slot framework.</span></span> <span data-ttu-id="cd346-936">Dotazy pro koncového uživatele tabulky představují příklady, které byste použili tento typ prostředku.</span><span class="sxs-lookup"><span data-stu-id="cd346-936">Queries against end-user tables represent examples that would use this resource type.</span></span>
* <span data-ttu-id="cd346-937">**DmsConcurrencyResourceType**: počká vyplývající z operace přesunu dat.</span><span class="sxs-lookup"><span data-stu-id="cd346-937">**DmsConcurrencyResourceType**: Waits resulting from data movement operations.</span></span>
* <span data-ttu-id="cd346-938">**BackupConcurrencyResourceType**: Tento čekání označuje, že se záloha databáze.</span><span class="sxs-lookup"><span data-stu-id="cd346-938">**BackupConcurrencyResourceType**: This wait indicates that a database is being backed up.</span></span> <span data-ttu-id="cd346-939">Maximální hodnota pro tento typ prostředku je 1.</span><span class="sxs-lookup"><span data-stu-id="cd346-939">The maximum value for this resource type is 1.</span></span> <span data-ttu-id="cd346-940">Vyžádání více záloh ve stejnou dobu jiné fronty.</span><span class="sxs-lookup"><span data-stu-id="cd346-940">If multiple backups have been requested at the same time, the others will queue.</span></span>

<span data-ttu-id="cd346-941">`sys.dm_pdw_waits` DMV lze použít na prostředcích, které se čeká na žádost.</span><span class="sxs-lookup"><span data-stu-id="cd346-941">The `sys.dm_pdw_waits` DMV can be used to see which resources a request is waiting for.</span></span>

```sql
SELECT  w.[wait_id]
,       w.[session_id]
,       w.[type]            AS Wait_type
,       w.[object_type]
,       w.[object_name]
,       w.[request_id]
,       w.[request_time]
,       w.[acquire_time]
,       w.[state]
,       w.[priority]
,    SESSION_ID()            AS Current_session
,    s.[status]            AS Session_status
,    s.[login_name]
,    s.[query_count]
,    s.[client_id]
,    s.[sql_spid]
,    r.[command]            AS Request_command
,    r.[label]
,    r.[status]            AS Request_status
,    r.[submit_time]
,    r.[start_time]
,    r.[end_compile_time]
,    r.[end_time]
,    DATEDIFF(ms,r.[submit_time],r.[start_time])        AS Request_queue_time_ms
,    DATEDIFF(ms,r.[start_time],r.[end_compile_time])    AS Request_compile_time_ms
,    DATEDIFF(ms,r.[end_compile_time],r.[end_time])        AS Request_execution_time_ms
,    r.[total_elapsed_time]
FROM    sys.dm_pdw_waits w
JOIN    sys.dm_pdw_exec_sessions s  ON w.[session_id] = s.[session_id]
JOIN    sys.dm_pdw_exec_requests r  ON w.[request_id] = r.[request_id]
WHERE    w.[session_id] <> SESSION_ID();
```

<span data-ttu-id="cd346-942">`sys.dm_pdw_resource_waits` DMV zobrazuje pouze prostředků čeká spotřebovávají daný dotaz.</span><span class="sxs-lookup"><span data-stu-id="cd346-942">The `sys.dm_pdw_resource_waits` DMV shows only the resource waits consumed by a given query.</span></span> <span data-ttu-id="cd346-943">Doba čekání prostředků pouze měří doba čekání na prostředky, které mají být poskytovány a doba čekání signál, což je čas potřebný pro základní servery SQL při plánování dotaz na procesor.</span><span class="sxs-lookup"><span data-stu-id="cd346-943">Resource wait time only measures the time waiting for resources to be provided, as opposed to signal wait time, which is the time it takes for the underlying SQL servers to schedule the query onto the CPU.</span></span>

```sql
SELECT  [session_id]
,       [type]
,       [object_type]
,       [object_name]
,       [request_id]
,       [request_time]
,       [acquire_time]
,       DATEDIFF(ms,[request_time],[acquire_time])  AS acquire_duration_ms
,       [concurrency_slots_used]                    AS concurrency_slots_reserved
,       [resource_class]
,       [wait_id]                                   AS queue_position
FROM    sys.dm_pdw_resource_waits
WHERE    [session_id] <> SESSION_ID();
```

<span data-ttu-id="cd346-944">`sys.dm_pdw_wait_stats` DMV lze použít pro analýzu Historický trend čeká.</span><span class="sxs-lookup"><span data-stu-id="cd346-944">The `sys.dm_pdw_wait_stats` DMV can be used for historic trend analysis of waits.</span></span>

```sql
SELECT    w.[pdw_node_id]
,        w.[wait_name]
,        w.[max_wait_time]
,        w.[request_count]
,        w.[signal_time]
,        w.[completed_count]
,        w.[wait_time]
FROM    sys.dm_pdw_wait_stats w;
```

## <a name="next-steps"></a><span data-ttu-id="cd346-945">Další kroky</span><span class="sxs-lookup"><span data-stu-id="cd346-945">Next steps</span></span>
<span data-ttu-id="cd346-946">Další informace o správě uživatelů a zabezpečení najdete v tématu [zabezpečení databáze v SQL Data Warehouse][Secure a database in SQL Data Warehouse].</span><span class="sxs-lookup"><span data-stu-id="cd346-946">For more information about managing database users and security, see [Secure a database in SQL Data Warehouse][Secure a database in SQL Data Warehouse].</span></span> <span data-ttu-id="cd346-947">Další informace o tom, jak větší třídy prostředků můžete zlepšení kvality indexu columnstore clusteru, najdete v části [nové sestavení indexů ke zlepšení kvality segment].</span><span class="sxs-lookup"><span data-stu-id="cd346-947">For more information about how larger resource classes can improve clustered columnstore index quality, see [Rebuilding indexes to improve segment quality].</span></span>

<!--Image references-->

<!--Article references-->
[Secure a database in SQL Data Warehouse]: ./sql-data-warehouse-overview-manage-security.md
[nové sestavení indexů ke zlepšení kvality segment]: ./sql-data-warehouse-tables-index.md#rebuilding-indexes-to-improve-segment-quality
[Secure a database in SQL Data Warehouse]: ./sql-data-warehouse-overview-manage-security.md

<!--MSDN references-->
[Managing Databases and Logins in Azure SQL Database]:https://msdn.microsoft.com/library/azure/ee336235.aspx

<!--Other Web references-->
