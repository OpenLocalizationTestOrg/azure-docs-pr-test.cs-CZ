---
title: "aaaConcurrency a úlohy správy v SQL Data Warehouse | Microsoft Docs"
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
ms.openlocfilehash: 7f7e77aa687760252aed16573b609817ed9111c3
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="concurrency-and-workload-management-in-sql-data-warehouse"></a><span data-ttu-id="ccea7-103">Souběžnost a úlohy správy v SQL Data Warehouse</span><span class="sxs-lookup"><span data-stu-id="ccea7-103">Concurrency and workload management in SQL Data Warehouse</span></span>
<span data-ttu-id="ccea7-104">předvídatelný výkon toodeliver ve velkém měřítku, Microsoft Azure SQL Data Warehouse umožňuje řídit souběžnosti úrovně a přidělení prostředků jako paměti a procesoru stanovení priorit.</span><span class="sxs-lookup"><span data-stu-id="ccea7-104">toodeliver predictable performance at scale, Microsoft Azure SQL Data Warehouse helps you control concurrency levels and resource allocations like memory and CPU prioritization.</span></span> <span data-ttu-id="ccea7-105">Tento článek představuje koncepty toohello souběžnosti a úlohy správy, která vysvětluje, jak oba implementována a jak je můžete řídit v datovém skladu.</span><span class="sxs-lookup"><span data-stu-id="ccea7-105">This article introduces you toohello concepts of concurrency and workload management, explaining how both features have been implemented and how you can control them in your data warehouse.</span></span> <span data-ttu-id="ccea7-106">Úlohy správy datového skladu SQL je určený toohelp podporu prostředí s více uživateli.</span><span class="sxs-lookup"><span data-stu-id="ccea7-106">SQL Data Warehouse workload management is intended toohelp you support multi-user environments.</span></span> <span data-ttu-id="ccea7-107">Rozhraní není určeno pro více klientů úlohy.</span><span class="sxs-lookup"><span data-stu-id="ccea7-107">It is not intended for multi-tenant workloads.</span></span>

## <a name="concurrency-limits"></a><span data-ttu-id="ccea7-108">Omezení souběžnosti</span><span class="sxs-lookup"><span data-stu-id="ccea7-108">Concurrency limits</span></span>
<span data-ttu-id="ccea7-109">SQL Data Warehouse umožňuje až too1 024 souběžných připojení.</span><span class="sxs-lookup"><span data-stu-id="ccea7-109">SQL Data Warehouse allows up too1,024 concurrent connections.</span></span> <span data-ttu-id="ccea7-110">Všechna 1 024 připojení mohou souběžně odesílat dotazy.</span><span class="sxs-lookup"><span data-stu-id="ccea7-110">All 1,024 connections can submit queries concurrently.</span></span> <span data-ttu-id="ccea7-111">Toooptimize propustnost, SQL Data Warehouse může však fronty tooensure některé dotazy, že každý dotaz obdrží grant minimální paměti.</span><span class="sxs-lookup"><span data-stu-id="ccea7-111">However, toooptimize throughput, SQL Data Warehouse may queue some queries tooensure that each query receives a minimal memory grant.</span></span> <span data-ttu-id="ccea7-112">Služba Řízení front dojde v době provedení dotazu.</span><span class="sxs-lookup"><span data-stu-id="ccea7-112">Queuing occurs at query execution time.</span></span> <span data-ttu-id="ccea7-113">Řízení front dotazy při může zvýšit souběžnost dosaženo omezení SQL Data Warehouse celková propustnost tím zajistí, že aktivní dotazy získat přístup toocritically potřeba paměťových prostředků.</span><span class="sxs-lookup"><span data-stu-id="ccea7-113">By queuing queries when concurrency limits are reached, SQL Data Warehouse can increase total throughput by ensuring that active queries get access toocritically needed memory resources.</span></span>  

<span data-ttu-id="ccea7-114">Omezení souběžnosti se řídí dvěma konceptů: *souběžných dotazů* a *souběžnosti sloty*.</span><span class="sxs-lookup"><span data-stu-id="ccea7-114">Concurrency limits are governed by two concepts: *concurrent queries* and *concurrency slots*.</span></span> <span data-ttu-id="ccea7-115">Pro tooexecute dotaz musí provést v rámci omezení souběžnosti hello dotazu a hello souběžnosti slotu přidělení.</span><span class="sxs-lookup"><span data-stu-id="ccea7-115">For a query tooexecute, it must execute within both hello query concurrency limit and hello concurrency slot allocation.</span></span>

* <span data-ttu-id="ccea7-116">Počet souběžných dotazů jsou hello dotazy na spuštění v hello stejný čas.</span><span class="sxs-lookup"><span data-stu-id="ccea7-116">Concurrent queries are hello queries executing at hello same time.</span></span> <span data-ttu-id="ccea7-117">SQL Data Warehouse podporuje až too32 souběžných dotazů hello větší velikostí DWU.</span><span class="sxs-lookup"><span data-stu-id="ccea7-117">SQL Data Warehouse supports up too32 concurrent queries on hello larger DWU sizes.</span></span>
* <span data-ttu-id="ccea7-118">Sloty souběžnosti mají při přidělování podle DWU.</span><span class="sxs-lookup"><span data-stu-id="ccea7-118">Concurrency slots are allocated based on DWU.</span></span> <span data-ttu-id="ccea7-119">Každý 100 DWU poskytuje 4 sloty souběžnosti.</span><span class="sxs-lookup"><span data-stu-id="ccea7-119">Each 100 DWU provides 4 concurrency slots.</span></span> <span data-ttu-id="ccea7-120">Například od DW100 přiděluje 4 sloty souběžnosti a DW1000 přiděluje 40.</span><span class="sxs-lookup"><span data-stu-id="ccea7-120">For example, a DW100 allocates 4 concurrency slots and DW1000 allocates 40.</span></span> <span data-ttu-id="ccea7-121">Každý dotaz využívá jednu nebo více souběžnosti sloty, závisí na hello [Třída prostředků](#resource-classes) hello dotazu.</span><span class="sxs-lookup"><span data-stu-id="ccea7-121">Each query consumes one or more concurrency slots, dependent on hello [resource class](#resource-classes) of hello query.</span></span> <span data-ttu-id="ccea7-122">Dotazy na spouštění v Třída prostředků smallrc hello zabrat jeden slot souběžnosti.</span><span class="sxs-lookup"><span data-stu-id="ccea7-122">Queries running in hello smallrc resource class consume one concurrency slot.</span></span> <span data-ttu-id="ccea7-123">Dotazy spuštěnými v vyšší Třída prostředků využívat další sloty souběžnosti.</span><span class="sxs-lookup"><span data-stu-id="ccea7-123">Queries running in a higher resource class consume more concurrency slots.</span></span>

<span data-ttu-id="ccea7-124">Hello následující tabulka popisuje hello limity pro souběžné dotazy a sloty souběžnosti v hello různých velikostí DWU.</span><span class="sxs-lookup"><span data-stu-id="ccea7-124">hello following table describes hello limits for both concurrent queries and concurrency slots at hello various DWU sizes.</span></span>

### <a name="concurrency-limits"></a><span data-ttu-id="ccea7-125">Omezení souběžnosti</span><span class="sxs-lookup"><span data-stu-id="ccea7-125">Concurrency limits</span></span>
| <span data-ttu-id="ccea7-126">DWU</span><span class="sxs-lookup"><span data-stu-id="ccea7-126">DWU</span></span> | <span data-ttu-id="ccea7-127">Maximální počet souběžných dotazů</span><span class="sxs-lookup"><span data-stu-id="ccea7-127">Max concurrent queries</span></span> | <span data-ttu-id="ccea7-128">Přidělené sloty souběžnosti</span><span class="sxs-lookup"><span data-stu-id="ccea7-128">Concurrency slots allocated</span></span> |
|:--- |:---:|:---:|
| <span data-ttu-id="ccea7-129">OD DW100</span><span class="sxs-lookup"><span data-stu-id="ccea7-129">DW100</span></span> |<span data-ttu-id="ccea7-130">4</span><span class="sxs-lookup"><span data-stu-id="ccea7-130">4</span></span> |<span data-ttu-id="ccea7-131">4</span><span class="sxs-lookup"><span data-stu-id="ccea7-131">4</span></span> |
| <span data-ttu-id="ccea7-132">DW200</span><span class="sxs-lookup"><span data-stu-id="ccea7-132">DW200</span></span> |<span data-ttu-id="ccea7-133">8</span><span class="sxs-lookup"><span data-stu-id="ccea7-133">8</span></span> |<span data-ttu-id="ccea7-134">8</span><span class="sxs-lookup"><span data-stu-id="ccea7-134">8</span></span> |
| <span data-ttu-id="ccea7-135">DW300</span><span class="sxs-lookup"><span data-stu-id="ccea7-135">DW300</span></span> |<span data-ttu-id="ccea7-136">12</span><span class="sxs-lookup"><span data-stu-id="ccea7-136">12</span></span> |<span data-ttu-id="ccea7-137">12</span><span class="sxs-lookup"><span data-stu-id="ccea7-137">12</span></span> |
| <span data-ttu-id="ccea7-138">DW400</span><span class="sxs-lookup"><span data-stu-id="ccea7-138">DW400</span></span> |<span data-ttu-id="ccea7-139">16</span><span class="sxs-lookup"><span data-stu-id="ccea7-139">16</span></span> |<span data-ttu-id="ccea7-140">16</span><span class="sxs-lookup"><span data-stu-id="ccea7-140">16</span></span> |
| <span data-ttu-id="ccea7-141">DW500</span><span class="sxs-lookup"><span data-stu-id="ccea7-141">DW500</span></span> |<span data-ttu-id="ccea7-142">20</span><span class="sxs-lookup"><span data-stu-id="ccea7-142">20</span></span> |<span data-ttu-id="ccea7-143">20</span><span class="sxs-lookup"><span data-stu-id="ccea7-143">20</span></span> |
| <span data-ttu-id="ccea7-144">DW600</span><span class="sxs-lookup"><span data-stu-id="ccea7-144">DW600</span></span> |<span data-ttu-id="ccea7-145">24</span><span class="sxs-lookup"><span data-stu-id="ccea7-145">24</span></span> |<span data-ttu-id="ccea7-146">24</span><span class="sxs-lookup"><span data-stu-id="ccea7-146">24</span></span> |
| <span data-ttu-id="ccea7-147">DW1000</span><span class="sxs-lookup"><span data-stu-id="ccea7-147">DW1000</span></span> |<span data-ttu-id="ccea7-148">32</span><span class="sxs-lookup"><span data-stu-id="ccea7-148">32</span></span> |<span data-ttu-id="ccea7-149">40</span><span class="sxs-lookup"><span data-stu-id="ccea7-149">40</span></span> |
| <span data-ttu-id="ccea7-150">DW1200</span><span class="sxs-lookup"><span data-stu-id="ccea7-150">DW1200</span></span> |<span data-ttu-id="ccea7-151">32</span><span class="sxs-lookup"><span data-stu-id="ccea7-151">32</span></span> |<span data-ttu-id="ccea7-152">48</span><span class="sxs-lookup"><span data-stu-id="ccea7-152">48</span></span> |
| <span data-ttu-id="ccea7-153">DW1500</span><span class="sxs-lookup"><span data-stu-id="ccea7-153">DW1500</span></span> |<span data-ttu-id="ccea7-154">32</span><span class="sxs-lookup"><span data-stu-id="ccea7-154">32</span></span> |<span data-ttu-id="ccea7-155">60</span><span class="sxs-lookup"><span data-stu-id="ccea7-155">60</span></span> |
| <span data-ttu-id="ccea7-156">DW2000</span><span class="sxs-lookup"><span data-stu-id="ccea7-156">DW2000</span></span> |<span data-ttu-id="ccea7-157">32</span><span class="sxs-lookup"><span data-stu-id="ccea7-157">32</span></span> |<span data-ttu-id="ccea7-158">80</span><span class="sxs-lookup"><span data-stu-id="ccea7-158">80</span></span> |
| <span data-ttu-id="ccea7-159">DW3000</span><span class="sxs-lookup"><span data-stu-id="ccea7-159">DW3000</span></span> |<span data-ttu-id="ccea7-160">32</span><span class="sxs-lookup"><span data-stu-id="ccea7-160">32</span></span> |<span data-ttu-id="ccea7-161">120</span><span class="sxs-lookup"><span data-stu-id="ccea7-161">120</span></span> |
| <span data-ttu-id="ccea7-162">DW6000</span><span class="sxs-lookup"><span data-stu-id="ccea7-162">DW6000</span></span> |<span data-ttu-id="ccea7-163">32</span><span class="sxs-lookup"><span data-stu-id="ccea7-163">32</span></span> |<span data-ttu-id="ccea7-164">240</span><span class="sxs-lookup"><span data-stu-id="ccea7-164">240</span></span> |

<span data-ttu-id="ccea7-165">Když je splněna jedna z těchto prahových hodnot, nové dotazy jsou zařazeny do fronty a jsou prováděny na základě ven first-in.</span><span class="sxs-lookup"><span data-stu-id="ccea7-165">When one of these thresholds is met, new queries are queued and executed on a first-in, first-out basis.</span></span>  <span data-ttu-id="ccea7-166">Dokončení dotazy a počet hello dotazy a sloty klesne pod hello omezení, jsou vydávány ve frontě dotazů.</span><span class="sxs-lookup"><span data-stu-id="ccea7-166">As a queries finishes and hello number of queries and slots falls below hello limits, queued queries are released.</span></span> 

> [!NOTE]  
> <span data-ttu-id="ccea7-167">*Vyberte* dotazy na spuštění výhradně na zobrazení dynamické správy (zobrazení dynamické správy) nebo zobrazení katalogu se neřídí žádné omezení souběžnosti hello.</span><span class="sxs-lookup"><span data-stu-id="ccea7-167">*Select* queries executing exclusively on dynamic management views (DMVs) or catalog views are not governed by any of hello concurrency limits.</span></span> <span data-ttu-id="ccea7-168">Můžete monitorovat hello systém bez ohledu na počet hello dotazy na spuštění v něm.</span><span class="sxs-lookup"><span data-stu-id="ccea7-168">You can monitor hello system regardless of hello number of queries executing on it.</span></span>
> 
> 

## <a name="resource-classes"></a><span data-ttu-id="ccea7-169">Třídy prostředků</span><span class="sxs-lookup"><span data-stu-id="ccea7-169">Resource classes</span></span>
<span data-ttu-id="ccea7-170">Prostředek třídy pomáhá řídit přidělování paměti a cyklů procesoru zadané tooa dotazu.</span><span class="sxs-lookup"><span data-stu-id="ccea7-170">Resource classes help you control memory allocation and CPU cycles given tooa query.</span></span> <span data-ttu-id="ccea7-171">Můžete přiřadit dva typy prostředků třídy tooa uživatele v podobě hello rolí databáze.</span><span class="sxs-lookup"><span data-stu-id="ccea7-171">You can assign two types of resource classes tooa user in hello form of database roles.</span></span> <span data-ttu-id="ccea7-172">Hello dva typy prostředků třídy jsou následující:</span><span class="sxs-lookup"><span data-stu-id="ccea7-172">hello two types of resource classes are as follows:</span></span>
1. <span data-ttu-id="ccea7-173">Dynamické třídy prostředků (**smallrc, mediumrc, largerc, xlargerc**) přidělit proměnné množství paměti v závislosti na hello aktuální DWU.</span><span class="sxs-lookup"><span data-stu-id="ccea7-173">Dynamic Resource Classes (**smallrc, mediumrc, largerc, xlargerc**) allocate a variable amount of memory depending on hello current DWU.</span></span> <span data-ttu-id="ccea7-174">To znamená, že při škálování tooa větší DWU, vaše dotazy automaticky získat více paměti.</span><span class="sxs-lookup"><span data-stu-id="ccea7-174">This means that when you scale up tooa larger DWU, your queries automatically get more memory.</span></span> 
2. <span data-ttu-id="ccea7-175">Statické třídy prostředků (**staticrc10, staticrc20, staticrc30, staticrc40, staticrc50, staticrc60, staticrc70, staticrc80**) přidělit hello stejné množství paměti, bez ohledu na to hello aktuální DWU (za předpokladu, že hello DWU sám sebe má dostatek paměti).</span><span class="sxs-lookup"><span data-stu-id="ccea7-175">Static Resource Classes (**staticrc10, staticrc20, staticrc30, staticrc40, staticrc50, staticrc60, staticrc70, staticrc80**) allocate hello same amount of memory regardless of hello current DWU (provided that hello DWU itself has enough memory).</span></span> <span data-ttu-id="ccea7-176">To znamená, že na větší počet Dwu, můžete spouštět další dotazy v každé třídě prostředků současně.</span><span class="sxs-lookup"><span data-stu-id="ccea7-176">This means that on larger DWUs, you can run more queries in each resource class concurrently.</span></span>

<span data-ttu-id="ccea7-177">Uživatelé v **smallrc** a **staticrc10** mají menší množství paměti a můžete využít výhod vyšší souběžnosti.</span><span class="sxs-lookup"><span data-stu-id="ccea7-177">Users in **smallrc** and **staticrc10** are given a smaller amount of memory and can take advantage of higher concurrency.</span></span> <span data-ttu-id="ccea7-178">Naproti tomu uživatelům přiřazena příliš**xlargerc** nebo **staticrc80** mají velké množství paměti, a proto méně jejich dotazů můžou běžet souběžně.</span><span class="sxs-lookup"><span data-stu-id="ccea7-178">In contrast, users assigned too**xlargerc** or **staticrc80** are given large amounts of memory, and therefore fewer of their queries can run concurrently.</span></span>

<span data-ttu-id="ccea7-179">Ve výchozím nastavení, každý uživatel je členem hello Třída prostředků se malé, **smallrc**.</span><span class="sxs-lookup"><span data-stu-id="ccea7-179">By default, each user is a member of hello small resource class, **smallrc**.</span></span> <span data-ttu-id="ccea7-180">Hello postup `sp_addrolemember` je používá třída prostředků hello tooincrease, a `sp_droprolemember` se používá třída prostředků toodecrease hello.</span><span class="sxs-lookup"><span data-stu-id="ccea7-180">hello procedure `sp_addrolemember` is used tooincrease hello resource class, and `sp_droprolemember` is used toodecrease hello resource class.</span></span> <span data-ttu-id="ccea7-181">Například tento příkaz by zvýšit hello prostředků třídu loaduser příliš**largerc**:</span><span class="sxs-lookup"><span data-stu-id="ccea7-181">For example, this command would increase hello resource class of loaduser too**largerc**:</span></span>

```sql
EXEC sp_addrolemember 'largerc', 'loaduser'
```


### <a name="queries-that-do-not-honor-resource-classes"></a><span data-ttu-id="ccea7-182">Dotazy, které nerespektují třídy prostředků</span><span class="sxs-lookup"><span data-stu-id="ccea7-182">Queries that do not honor resource classes</span></span>

<span data-ttu-id="ccea7-183">Existuje několik typů dotazy, které nejsou nijak přínosné větší přidělení paměti.</span><span class="sxs-lookup"><span data-stu-id="ccea7-183">There are a few types of queries that do not benefit from a larger memory allocation.</span></span> <span data-ttu-id="ccea7-184">systém Hello ignoruje jejich přidělení Třída prostředků a vždy spusťte tyto dotazy v třídě malé prostředků hello místo.</span><span class="sxs-lookup"><span data-stu-id="ccea7-184">hello system ignores their resource class allocation and always run these queries in hello small resource class instead.</span></span> <span data-ttu-id="ccea7-185">Pokud tyto dotazy spustit vždy v třída hello malé prostředků, mohou spouštět při souběžnosti sloty jsou přetížena a jejich nebude využívat další slotů, než je potřeba.</span><span class="sxs-lookup"><span data-stu-id="ccea7-185">If these queries always run in hello small resource class, they can run when concurrency slots are under pressure and they won't consume more slots than needed.</span></span> <span data-ttu-id="ccea7-186">V tématu [výjimky třídy prostředků](#query-exceptions-to-concurrency-limits) Další informace.</span><span class="sxs-lookup"><span data-stu-id="ccea7-186">See [Resource class exceptions](#query-exceptions-to-concurrency-limits) for more information.</span></span>

## <a name="details-on-resource-class-assignment"></a><span data-ttu-id="ccea7-187">Informace o přiřazení třídy prostředků</span><span class="sxs-lookup"><span data-stu-id="ccea7-187">Details on resource class assignment</span></span>


<span data-ttu-id="ccea7-188">Několik další podrobnosti o Třída prostředků:</span><span class="sxs-lookup"><span data-stu-id="ccea7-188">A few more details on resource class:</span></span>

* <span data-ttu-id="ccea7-189">*Příkaz ALTER role* oprávnění jsou nutná Třída prostředků hello toochange uživatele.</span><span class="sxs-lookup"><span data-stu-id="ccea7-189">*Alter role* permission is required toochange hello resource class of a user.</span></span>
* <span data-ttu-id="ccea7-190">I když můžete přidat uživatele tooone nebo více prostředků třídy vyšší hello, dynamické prostředků třídy mají přednost před třídy statické prostředků.</span><span class="sxs-lookup"><span data-stu-id="ccea7-190">Although you can add a user tooone or more of hello higher resource classes, dynamic resource classes take precedence over static resource classes.</span></span> <span data-ttu-id="ccea7-191">To znamená pokud uživatel přiřazený tooboth **mediumrc**(dynamické) a **staticrc80**(statické), **mediumrc** je hello Třída prostředků, která je dodržení.</span><span class="sxs-lookup"><span data-stu-id="ccea7-191">That is, if a user is assigned tooboth **mediumrc**(dynamic) and **staticrc80**(static), **mediumrc** is hello resource class that is honored.</span></span>
 * <span data-ttu-id="ccea7-192">Když se uživatel je přiřazený toomore než jeden prostředek třídy v na konkrétní třídy typ prostředku (více než jeden dynamický prostředků nebo více než jeden statický prostředků třída), je dodržení hello nejvyšší Třída prostředků.</span><span class="sxs-lookup"><span data-stu-id="ccea7-192">When a user is assigned toomore than one resource class in a specific resource class type (more than one dynamic resource class or more than one static resource class), hello highest resource class is honored.</span></span> <span data-ttu-id="ccea7-193">To znamená pokud uživatel přiřazený tooboth mediumrc a largerc, je dodržení Třída prostředků vyšší hello (largerc).</span><span class="sxs-lookup"><span data-stu-id="ccea7-193">That is, if a user is assigned tooboth mediumrc and largerc, hello higher resource class (largerc) is honored.</span></span> <span data-ttu-id="ccea7-194">A pokud uživatel přiřazený tooboth **staticrc20** a **statirc80**, **staticrc80** je přijmout.</span><span class="sxs-lookup"><span data-stu-id="ccea7-194">And if a user is assigned tooboth **staticrc20** and **statirc80**, **staticrc80** is honored.</span></span>
* <span data-ttu-id="ccea7-195">Třída prostředků Hello hello systému administrativní uživatele nelze změnit.</span><span class="sxs-lookup"><span data-stu-id="ccea7-195">hello resource class of hello system administrative user cannot be changed.</span></span>

<span data-ttu-id="ccea7-196">Podrobný příklad najdete v tématu [příklad třídy prostředků uživatele změna](#changing-user-resource-class-example).</span><span class="sxs-lookup"><span data-stu-id="ccea7-196">For a detailed example, see [Changing user resource class example](#changing-user-resource-class-example).</span></span>

## <a name="memory-allocation"></a><span data-ttu-id="ccea7-197">Přidělení paměti</span><span class="sxs-lookup"><span data-stu-id="ccea7-197">Memory allocation</span></span>
<span data-ttu-id="ccea7-198">Existují výhody a nevýhody tooincreasing Třída prostředků uživatele.</span><span class="sxs-lookup"><span data-stu-id="ccea7-198">There are pros and cons tooincreasing a user's resource class.</span></span> <span data-ttu-id="ccea7-199">Zvýšení Třída prostředků pro uživatele, dává přístup toomore paměti, což může znamenat, že dotazy spouštět rychleji jejich dotazů.</span><span class="sxs-lookup"><span data-stu-id="ccea7-199">Increasing a resource class for a user, gives their queries access toomore memory, which may mean queries execute faster.</span></span>  <span data-ttu-id="ccea7-200">Vyšší třídy prostředků však také snížit hello počet souběžných dotazů, které můžou běžet.</span><span class="sxs-lookup"><span data-stu-id="ccea7-200">However, higher resource classes also reduce hello number of concurrent queries that can run.</span></span> <span data-ttu-id="ccea7-201">Toto je hello kompromis mezi přidělování velké objemy paměti tooa jeden dotaz nebo povolením jiné dotazy, které je také třeba přidělení paměti, toorun současně.</span><span class="sxs-lookup"><span data-stu-id="ccea7-201">This is hello trade-off between allocating large amounts of memory tooa single query or allowing other queries, which also need memory allocations, toorun concurrently.</span></span> <span data-ttu-id="ccea7-202">Pokud jeden uživatel vysoké přidělení paměti pro dotaz, ostatní uživatelé nebudou mít přístup toothat stejné paměti toorun dotazu.</span><span class="sxs-lookup"><span data-stu-id="ccea7-202">If one user is given high allocations of memory for a query, other users will not have access toothat same memory toorun a query.</span></span>

<span data-ttu-id="ccea7-203">Hello následující tabulka mapuje hello paměti přidělené tooeach distribuční třídou DWU a prostředků.</span><span class="sxs-lookup"><span data-stu-id="ccea7-203">hello following table maps hello memory allocated tooeach distribution by DWU and resource class.</span></span>

### <a name="memory-allocations-per-distribution-for-dynamic-resource-classes-mb"></a><span data-ttu-id="ccea7-204">Přidělení paměti za distribuci pro dynamické prostředků třídy (MB)</span><span class="sxs-lookup"><span data-stu-id="ccea7-204">Memory allocations per distribution for dynamic resource classes (MB)</span></span>
| <span data-ttu-id="ccea7-205">DWU</span><span class="sxs-lookup"><span data-stu-id="ccea7-205">DWU</span></span> | <span data-ttu-id="ccea7-206">smallrc</span><span class="sxs-lookup"><span data-stu-id="ccea7-206">smallrc</span></span> | <span data-ttu-id="ccea7-207">mediumrc</span><span class="sxs-lookup"><span data-stu-id="ccea7-207">mediumrc</span></span> | <span data-ttu-id="ccea7-208">largerc</span><span class="sxs-lookup"><span data-stu-id="ccea7-208">largerc</span></span> | <span data-ttu-id="ccea7-209">xlargerc</span><span class="sxs-lookup"><span data-stu-id="ccea7-209">xlargerc</span></span> |
|:--- |:---:|:---:|:---:|:---:|
| <span data-ttu-id="ccea7-210">OD DW100</span><span class="sxs-lookup"><span data-stu-id="ccea7-210">DW100</span></span> |<span data-ttu-id="ccea7-211">100</span><span class="sxs-lookup"><span data-stu-id="ccea7-211">100</span></span> |<span data-ttu-id="ccea7-212">100</span><span class="sxs-lookup"><span data-stu-id="ccea7-212">100</span></span> |<span data-ttu-id="ccea7-213">200</span><span class="sxs-lookup"><span data-stu-id="ccea7-213">200</span></span> |<span data-ttu-id="ccea7-214">400</span><span class="sxs-lookup"><span data-stu-id="ccea7-214">400</span></span> |
| <span data-ttu-id="ccea7-215">DW200</span><span class="sxs-lookup"><span data-stu-id="ccea7-215">DW200</span></span> |<span data-ttu-id="ccea7-216">100</span><span class="sxs-lookup"><span data-stu-id="ccea7-216">100</span></span> |<span data-ttu-id="ccea7-217">200</span><span class="sxs-lookup"><span data-stu-id="ccea7-217">200</span></span> |<span data-ttu-id="ccea7-218">400</span><span class="sxs-lookup"><span data-stu-id="ccea7-218">400</span></span> |<span data-ttu-id="ccea7-219">800</span><span class="sxs-lookup"><span data-stu-id="ccea7-219">800</span></span> |
| <span data-ttu-id="ccea7-220">DW300</span><span class="sxs-lookup"><span data-stu-id="ccea7-220">DW300</span></span> |<span data-ttu-id="ccea7-221">100</span><span class="sxs-lookup"><span data-stu-id="ccea7-221">100</span></span> |<span data-ttu-id="ccea7-222">200</span><span class="sxs-lookup"><span data-stu-id="ccea7-222">200</span></span> |<span data-ttu-id="ccea7-223">400</span><span class="sxs-lookup"><span data-stu-id="ccea7-223">400</span></span> |<span data-ttu-id="ccea7-224">800</span><span class="sxs-lookup"><span data-stu-id="ccea7-224">800</span></span> |
| <span data-ttu-id="ccea7-225">DW400</span><span class="sxs-lookup"><span data-stu-id="ccea7-225">DW400</span></span> |<span data-ttu-id="ccea7-226">100</span><span class="sxs-lookup"><span data-stu-id="ccea7-226">100</span></span> |<span data-ttu-id="ccea7-227">400</span><span class="sxs-lookup"><span data-stu-id="ccea7-227">400</span></span> |<span data-ttu-id="ccea7-228">800</span><span class="sxs-lookup"><span data-stu-id="ccea7-228">800</span></span> |<span data-ttu-id="ccea7-229">1,600</span><span class="sxs-lookup"><span data-stu-id="ccea7-229">1,600</span></span> |
| <span data-ttu-id="ccea7-230">DW500</span><span class="sxs-lookup"><span data-stu-id="ccea7-230">DW500</span></span> |<span data-ttu-id="ccea7-231">100</span><span class="sxs-lookup"><span data-stu-id="ccea7-231">100</span></span> |<span data-ttu-id="ccea7-232">400</span><span class="sxs-lookup"><span data-stu-id="ccea7-232">400</span></span> |<span data-ttu-id="ccea7-233">800</span><span class="sxs-lookup"><span data-stu-id="ccea7-233">800</span></span> |<span data-ttu-id="ccea7-234">1,600</span><span class="sxs-lookup"><span data-stu-id="ccea7-234">1,600</span></span> |
| <span data-ttu-id="ccea7-235">DW600</span><span class="sxs-lookup"><span data-stu-id="ccea7-235">DW600</span></span> |<span data-ttu-id="ccea7-236">100</span><span class="sxs-lookup"><span data-stu-id="ccea7-236">100</span></span> |<span data-ttu-id="ccea7-237">400</span><span class="sxs-lookup"><span data-stu-id="ccea7-237">400</span></span> |<span data-ttu-id="ccea7-238">800</span><span class="sxs-lookup"><span data-stu-id="ccea7-238">800</span></span> |<span data-ttu-id="ccea7-239">1,600</span><span class="sxs-lookup"><span data-stu-id="ccea7-239">1,600</span></span> |
| <span data-ttu-id="ccea7-240">DW1000</span><span class="sxs-lookup"><span data-stu-id="ccea7-240">DW1000</span></span> |<span data-ttu-id="ccea7-241">100</span><span class="sxs-lookup"><span data-stu-id="ccea7-241">100</span></span> |<span data-ttu-id="ccea7-242">800</span><span class="sxs-lookup"><span data-stu-id="ccea7-242">800</span></span> |<span data-ttu-id="ccea7-243">1,600</span><span class="sxs-lookup"><span data-stu-id="ccea7-243">1,600</span></span> |<span data-ttu-id="ccea7-244">3,200</span><span class="sxs-lookup"><span data-stu-id="ccea7-244">3,200</span></span> |
| <span data-ttu-id="ccea7-245">DW1200</span><span class="sxs-lookup"><span data-stu-id="ccea7-245">DW1200</span></span> |<span data-ttu-id="ccea7-246">100</span><span class="sxs-lookup"><span data-stu-id="ccea7-246">100</span></span> |<span data-ttu-id="ccea7-247">800</span><span class="sxs-lookup"><span data-stu-id="ccea7-247">800</span></span> |<span data-ttu-id="ccea7-248">1,600</span><span class="sxs-lookup"><span data-stu-id="ccea7-248">1,600</span></span> |<span data-ttu-id="ccea7-249">3,200</span><span class="sxs-lookup"><span data-stu-id="ccea7-249">3,200</span></span> |
| <span data-ttu-id="ccea7-250">DW1500</span><span class="sxs-lookup"><span data-stu-id="ccea7-250">DW1500</span></span> |<span data-ttu-id="ccea7-251">100</span><span class="sxs-lookup"><span data-stu-id="ccea7-251">100</span></span> |<span data-ttu-id="ccea7-252">800</span><span class="sxs-lookup"><span data-stu-id="ccea7-252">800</span></span> |<span data-ttu-id="ccea7-253">1,600</span><span class="sxs-lookup"><span data-stu-id="ccea7-253">1,600</span></span> |<span data-ttu-id="ccea7-254">3,200</span><span class="sxs-lookup"><span data-stu-id="ccea7-254">3,200</span></span> |
| <span data-ttu-id="ccea7-255">DW2000</span><span class="sxs-lookup"><span data-stu-id="ccea7-255">DW2000</span></span> |<span data-ttu-id="ccea7-256">100</span><span class="sxs-lookup"><span data-stu-id="ccea7-256">100</span></span> |<span data-ttu-id="ccea7-257">1,600</span><span class="sxs-lookup"><span data-stu-id="ccea7-257">1,600</span></span> |<span data-ttu-id="ccea7-258">3,200</span><span class="sxs-lookup"><span data-stu-id="ccea7-258">3,200</span></span> |<span data-ttu-id="ccea7-259">6,400</span><span class="sxs-lookup"><span data-stu-id="ccea7-259">6,400</span></span> |
| <span data-ttu-id="ccea7-260">DW3000</span><span class="sxs-lookup"><span data-stu-id="ccea7-260">DW3000</span></span> |<span data-ttu-id="ccea7-261">100</span><span class="sxs-lookup"><span data-stu-id="ccea7-261">100</span></span> |<span data-ttu-id="ccea7-262">1,600</span><span class="sxs-lookup"><span data-stu-id="ccea7-262">1,600</span></span> |<span data-ttu-id="ccea7-263">3,200</span><span class="sxs-lookup"><span data-stu-id="ccea7-263">3,200</span></span> |<span data-ttu-id="ccea7-264">6,400</span><span class="sxs-lookup"><span data-stu-id="ccea7-264">6,400</span></span> |
| <span data-ttu-id="ccea7-265">DW6000</span><span class="sxs-lookup"><span data-stu-id="ccea7-265">DW6000</span></span> |<span data-ttu-id="ccea7-266">100</span><span class="sxs-lookup"><span data-stu-id="ccea7-266">100</span></span> |<span data-ttu-id="ccea7-267">3,200</span><span class="sxs-lookup"><span data-stu-id="ccea7-267">3,200</span></span> |<span data-ttu-id="ccea7-268">6,400</span><span class="sxs-lookup"><span data-stu-id="ccea7-268">6,400</span></span> |<span data-ttu-id="ccea7-269">12,800</span><span class="sxs-lookup"><span data-stu-id="ccea7-269">12,800</span></span> |

<span data-ttu-id="ccea7-270">Hello následující tabulka mapuje hello paměti přidělené tooeach distribuční DWU a třída statické prostředků.</span><span class="sxs-lookup"><span data-stu-id="ccea7-270">hello following table maps hello memory allocated tooeach distribution by DWU and static resource class.</span></span> <span data-ttu-id="ccea7-271">Všimněte si, že hello vyšší prostředků třídy mají jejich paměti snížit toohonor hello globální DWU omezení.</span><span class="sxs-lookup"><span data-stu-id="ccea7-271">Note that hello higher resource classes have their memory reduced toohonor hello global DWU limits.</span></span>

### <a name="memory-allocations-per-distribution-for-static-resource-classes-mb"></a><span data-ttu-id="ccea7-272">Přidělení paměti za distribuci pro statické prostředků třídy (MB)</span><span class="sxs-lookup"><span data-stu-id="ccea7-272">Memory allocations per distribution for static resource classes (MB)</span></span>
| <span data-ttu-id="ccea7-273">DWU</span><span class="sxs-lookup"><span data-stu-id="ccea7-273">DWU</span></span> | <span data-ttu-id="ccea7-274">staticrc10</span><span class="sxs-lookup"><span data-stu-id="ccea7-274">staticrc10</span></span> | <span data-ttu-id="ccea7-275">staticrc20</span><span class="sxs-lookup"><span data-stu-id="ccea7-275">staticrc20</span></span> | <span data-ttu-id="ccea7-276">staticrc30</span><span class="sxs-lookup"><span data-stu-id="ccea7-276">staticrc30</span></span> | <span data-ttu-id="ccea7-277">staticrc40</span><span class="sxs-lookup"><span data-stu-id="ccea7-277">staticrc40</span></span> | <span data-ttu-id="ccea7-278">staticrc50</span><span class="sxs-lookup"><span data-stu-id="ccea7-278">staticrc50</span></span> | <span data-ttu-id="ccea7-279">staticrc60</span><span class="sxs-lookup"><span data-stu-id="ccea7-279">staticrc60</span></span> | <span data-ttu-id="ccea7-280">staticrc70</span><span class="sxs-lookup"><span data-stu-id="ccea7-280">staticrc70</span></span> | <span data-ttu-id="ccea7-281">staticrc80</span><span class="sxs-lookup"><span data-stu-id="ccea7-281">staticrc80</span></span> |
|:--- |:---:|:---:|:---:|:---:|:---:|:---:|:---:|:---:|
| <span data-ttu-id="ccea7-282">OD DW100</span><span class="sxs-lookup"><span data-stu-id="ccea7-282">DW100</span></span> |<span data-ttu-id="ccea7-283">100</span><span class="sxs-lookup"><span data-stu-id="ccea7-283">100</span></span> |<span data-ttu-id="ccea7-284">200</span><span class="sxs-lookup"><span data-stu-id="ccea7-284">200</span></span> |<span data-ttu-id="ccea7-285">400</span><span class="sxs-lookup"><span data-stu-id="ccea7-285">400</span></span> |<span data-ttu-id="ccea7-286">400</span><span class="sxs-lookup"><span data-stu-id="ccea7-286">400</span></span> |<span data-ttu-id="ccea7-287">400</span><span class="sxs-lookup"><span data-stu-id="ccea7-287">400</span></span> |<span data-ttu-id="ccea7-288">400</span><span class="sxs-lookup"><span data-stu-id="ccea7-288">400</span></span> |<span data-ttu-id="ccea7-289">400</span><span class="sxs-lookup"><span data-stu-id="ccea7-289">400</span></span> |<span data-ttu-id="ccea7-290">400</span><span class="sxs-lookup"><span data-stu-id="ccea7-290">400</span></span> |
| <span data-ttu-id="ccea7-291">DW200</span><span class="sxs-lookup"><span data-stu-id="ccea7-291">DW200</span></span> |<span data-ttu-id="ccea7-292">100</span><span class="sxs-lookup"><span data-stu-id="ccea7-292">100</span></span> |<span data-ttu-id="ccea7-293">200</span><span class="sxs-lookup"><span data-stu-id="ccea7-293">200</span></span> |<span data-ttu-id="ccea7-294">400</span><span class="sxs-lookup"><span data-stu-id="ccea7-294">400</span></span> |<span data-ttu-id="ccea7-295">800</span><span class="sxs-lookup"><span data-stu-id="ccea7-295">800</span></span> |<span data-ttu-id="ccea7-296">800</span><span class="sxs-lookup"><span data-stu-id="ccea7-296">800</span></span> |<span data-ttu-id="ccea7-297">800</span><span class="sxs-lookup"><span data-stu-id="ccea7-297">800</span></span> |<span data-ttu-id="ccea7-298">800</span><span class="sxs-lookup"><span data-stu-id="ccea7-298">800</span></span> |<span data-ttu-id="ccea7-299">800</span><span class="sxs-lookup"><span data-stu-id="ccea7-299">800</span></span> |
| <span data-ttu-id="ccea7-300">DW300</span><span class="sxs-lookup"><span data-stu-id="ccea7-300">DW300</span></span> |<span data-ttu-id="ccea7-301">100</span><span class="sxs-lookup"><span data-stu-id="ccea7-301">100</span></span> |<span data-ttu-id="ccea7-302">200</span><span class="sxs-lookup"><span data-stu-id="ccea7-302">200</span></span> |<span data-ttu-id="ccea7-303">400</span><span class="sxs-lookup"><span data-stu-id="ccea7-303">400</span></span> |<span data-ttu-id="ccea7-304">800</span><span class="sxs-lookup"><span data-stu-id="ccea7-304">800</span></span> |<span data-ttu-id="ccea7-305">800</span><span class="sxs-lookup"><span data-stu-id="ccea7-305">800</span></span> |<span data-ttu-id="ccea7-306">800</span><span class="sxs-lookup"><span data-stu-id="ccea7-306">800</span></span> |<span data-ttu-id="ccea7-307">800</span><span class="sxs-lookup"><span data-stu-id="ccea7-307">800</span></span> |<span data-ttu-id="ccea7-308">800</span><span class="sxs-lookup"><span data-stu-id="ccea7-308">800</span></span> |
| <span data-ttu-id="ccea7-309">DW400</span><span class="sxs-lookup"><span data-stu-id="ccea7-309">DW400</span></span> |<span data-ttu-id="ccea7-310">100</span><span class="sxs-lookup"><span data-stu-id="ccea7-310">100</span></span> |<span data-ttu-id="ccea7-311">200</span><span class="sxs-lookup"><span data-stu-id="ccea7-311">200</span></span> |<span data-ttu-id="ccea7-312">400</span><span class="sxs-lookup"><span data-stu-id="ccea7-312">400</span></span> |<span data-ttu-id="ccea7-313">800</span><span class="sxs-lookup"><span data-stu-id="ccea7-313">800</span></span> |<span data-ttu-id="ccea7-314">1,600</span><span class="sxs-lookup"><span data-stu-id="ccea7-314">1,600</span></span> |<span data-ttu-id="ccea7-315">1,600</span><span class="sxs-lookup"><span data-stu-id="ccea7-315">1,600</span></span> |<span data-ttu-id="ccea7-316">1,600</span><span class="sxs-lookup"><span data-stu-id="ccea7-316">1,600</span></span> |<span data-ttu-id="ccea7-317">1,600</span><span class="sxs-lookup"><span data-stu-id="ccea7-317">1,600</span></span> |
| <span data-ttu-id="ccea7-318">DW500</span><span class="sxs-lookup"><span data-stu-id="ccea7-318">DW500</span></span> |<span data-ttu-id="ccea7-319">100</span><span class="sxs-lookup"><span data-stu-id="ccea7-319">100</span></span> |<span data-ttu-id="ccea7-320">200</span><span class="sxs-lookup"><span data-stu-id="ccea7-320">200</span></span> |<span data-ttu-id="ccea7-321">400</span><span class="sxs-lookup"><span data-stu-id="ccea7-321">400</span></span> |<span data-ttu-id="ccea7-322">800</span><span class="sxs-lookup"><span data-stu-id="ccea7-322">800</span></span> |<span data-ttu-id="ccea7-323">1,600</span><span class="sxs-lookup"><span data-stu-id="ccea7-323">1,600</span></span> |<span data-ttu-id="ccea7-324">1,600</span><span class="sxs-lookup"><span data-stu-id="ccea7-324">1,600</span></span> |<span data-ttu-id="ccea7-325">1,600</span><span class="sxs-lookup"><span data-stu-id="ccea7-325">1,600</span></span> |<span data-ttu-id="ccea7-326">1,600</span><span class="sxs-lookup"><span data-stu-id="ccea7-326">1,600</span></span> |
| <span data-ttu-id="ccea7-327">DW600</span><span class="sxs-lookup"><span data-stu-id="ccea7-327">DW600</span></span> |<span data-ttu-id="ccea7-328">100</span><span class="sxs-lookup"><span data-stu-id="ccea7-328">100</span></span> |<span data-ttu-id="ccea7-329">200</span><span class="sxs-lookup"><span data-stu-id="ccea7-329">200</span></span> |<span data-ttu-id="ccea7-330">400</span><span class="sxs-lookup"><span data-stu-id="ccea7-330">400</span></span> |<span data-ttu-id="ccea7-331">800</span><span class="sxs-lookup"><span data-stu-id="ccea7-331">800</span></span> |<span data-ttu-id="ccea7-332">1,600</span><span class="sxs-lookup"><span data-stu-id="ccea7-332">1,600</span></span> |<span data-ttu-id="ccea7-333">1,600</span><span class="sxs-lookup"><span data-stu-id="ccea7-333">1,600</span></span> |<span data-ttu-id="ccea7-334">1,600</span><span class="sxs-lookup"><span data-stu-id="ccea7-334">1,600</span></span> |<span data-ttu-id="ccea7-335">1,600</span><span class="sxs-lookup"><span data-stu-id="ccea7-335">1,600</span></span> |
| <span data-ttu-id="ccea7-336">DW1000</span><span class="sxs-lookup"><span data-stu-id="ccea7-336">DW1000</span></span> |<span data-ttu-id="ccea7-337">100</span><span class="sxs-lookup"><span data-stu-id="ccea7-337">100</span></span> |<span data-ttu-id="ccea7-338">200</span><span class="sxs-lookup"><span data-stu-id="ccea7-338">200</span></span> |<span data-ttu-id="ccea7-339">400</span><span class="sxs-lookup"><span data-stu-id="ccea7-339">400</span></span> |<span data-ttu-id="ccea7-340">800</span><span class="sxs-lookup"><span data-stu-id="ccea7-340">800</span></span> |<span data-ttu-id="ccea7-341">1,600</span><span class="sxs-lookup"><span data-stu-id="ccea7-341">1,600</span></span> |<span data-ttu-id="ccea7-342">3,200</span><span class="sxs-lookup"><span data-stu-id="ccea7-342">3,200</span></span> |<span data-ttu-id="ccea7-343">3,200</span><span class="sxs-lookup"><span data-stu-id="ccea7-343">3,200</span></span> |<span data-ttu-id="ccea7-344">3,200</span><span class="sxs-lookup"><span data-stu-id="ccea7-344">3,200</span></span> |
| <span data-ttu-id="ccea7-345">DW1200</span><span class="sxs-lookup"><span data-stu-id="ccea7-345">DW1200</span></span> |<span data-ttu-id="ccea7-346">100</span><span class="sxs-lookup"><span data-stu-id="ccea7-346">100</span></span> |<span data-ttu-id="ccea7-347">200</span><span class="sxs-lookup"><span data-stu-id="ccea7-347">200</span></span> |<span data-ttu-id="ccea7-348">400</span><span class="sxs-lookup"><span data-stu-id="ccea7-348">400</span></span> |<span data-ttu-id="ccea7-349">800</span><span class="sxs-lookup"><span data-stu-id="ccea7-349">800</span></span> |<span data-ttu-id="ccea7-350">1,600</span><span class="sxs-lookup"><span data-stu-id="ccea7-350">1,600</span></span> |<span data-ttu-id="ccea7-351">3,200</span><span class="sxs-lookup"><span data-stu-id="ccea7-351">3,200</span></span> |<span data-ttu-id="ccea7-352">3,200</span><span class="sxs-lookup"><span data-stu-id="ccea7-352">3,200</span></span> |<span data-ttu-id="ccea7-353">3,200</span><span class="sxs-lookup"><span data-stu-id="ccea7-353">3,200</span></span> |
| <span data-ttu-id="ccea7-354">DW1500</span><span class="sxs-lookup"><span data-stu-id="ccea7-354">DW1500</span></span> |<span data-ttu-id="ccea7-355">100</span><span class="sxs-lookup"><span data-stu-id="ccea7-355">100</span></span> |<span data-ttu-id="ccea7-356">200</span><span class="sxs-lookup"><span data-stu-id="ccea7-356">200</span></span> |<span data-ttu-id="ccea7-357">400</span><span class="sxs-lookup"><span data-stu-id="ccea7-357">400</span></span> |<span data-ttu-id="ccea7-358">800</span><span class="sxs-lookup"><span data-stu-id="ccea7-358">800</span></span> |<span data-ttu-id="ccea7-359">1,600</span><span class="sxs-lookup"><span data-stu-id="ccea7-359">1,600</span></span> |<span data-ttu-id="ccea7-360">3,200</span><span class="sxs-lookup"><span data-stu-id="ccea7-360">3,200</span></span> |<span data-ttu-id="ccea7-361">3,200</span><span class="sxs-lookup"><span data-stu-id="ccea7-361">3,200</span></span> |<span data-ttu-id="ccea7-362">3,200</span><span class="sxs-lookup"><span data-stu-id="ccea7-362">3,200</span></span> |
| <span data-ttu-id="ccea7-363">DW2000</span><span class="sxs-lookup"><span data-stu-id="ccea7-363">DW2000</span></span> |<span data-ttu-id="ccea7-364">100</span><span class="sxs-lookup"><span data-stu-id="ccea7-364">100</span></span> |<span data-ttu-id="ccea7-365">200</span><span class="sxs-lookup"><span data-stu-id="ccea7-365">200</span></span> |<span data-ttu-id="ccea7-366">400</span><span class="sxs-lookup"><span data-stu-id="ccea7-366">400</span></span> |<span data-ttu-id="ccea7-367">800</span><span class="sxs-lookup"><span data-stu-id="ccea7-367">800</span></span> |<span data-ttu-id="ccea7-368">1,600</span><span class="sxs-lookup"><span data-stu-id="ccea7-368">1,600</span></span> |<span data-ttu-id="ccea7-369">3,200</span><span class="sxs-lookup"><span data-stu-id="ccea7-369">3,200</span></span> |<span data-ttu-id="ccea7-370">6,400</span><span class="sxs-lookup"><span data-stu-id="ccea7-370">6,400</span></span> |<span data-ttu-id="ccea7-371">6,400</span><span class="sxs-lookup"><span data-stu-id="ccea7-371">6,400</span></span> |
| <span data-ttu-id="ccea7-372">DW3000</span><span class="sxs-lookup"><span data-stu-id="ccea7-372">DW3000</span></span> |<span data-ttu-id="ccea7-373">100</span><span class="sxs-lookup"><span data-stu-id="ccea7-373">100</span></span> |<span data-ttu-id="ccea7-374">200</span><span class="sxs-lookup"><span data-stu-id="ccea7-374">200</span></span> |<span data-ttu-id="ccea7-375">400</span><span class="sxs-lookup"><span data-stu-id="ccea7-375">400</span></span> |<span data-ttu-id="ccea7-376">800</span><span class="sxs-lookup"><span data-stu-id="ccea7-376">800</span></span> |<span data-ttu-id="ccea7-377">1,600</span><span class="sxs-lookup"><span data-stu-id="ccea7-377">1,600</span></span> |<span data-ttu-id="ccea7-378">3,200</span><span class="sxs-lookup"><span data-stu-id="ccea7-378">3,200</span></span> |<span data-ttu-id="ccea7-379">6,400</span><span class="sxs-lookup"><span data-stu-id="ccea7-379">6,400</span></span> |<span data-ttu-id="ccea7-380">6,400</span><span class="sxs-lookup"><span data-stu-id="ccea7-380">6,400</span></span> |
| <span data-ttu-id="ccea7-381">DW6000</span><span class="sxs-lookup"><span data-stu-id="ccea7-381">DW6000</span></span> |<span data-ttu-id="ccea7-382">100</span><span class="sxs-lookup"><span data-stu-id="ccea7-382">100</span></span> |<span data-ttu-id="ccea7-383">200</span><span class="sxs-lookup"><span data-stu-id="ccea7-383">200</span></span> |<span data-ttu-id="ccea7-384">400</span><span class="sxs-lookup"><span data-stu-id="ccea7-384">400</span></span> |<span data-ttu-id="ccea7-385">800</span><span class="sxs-lookup"><span data-stu-id="ccea7-385">800</span></span> |<span data-ttu-id="ccea7-386">1,600</span><span class="sxs-lookup"><span data-stu-id="ccea7-386">1,600</span></span> |<span data-ttu-id="ccea7-387">3,200</span><span class="sxs-lookup"><span data-stu-id="ccea7-387">3,200</span></span> |<span data-ttu-id="ccea7-388">6,400</span><span class="sxs-lookup"><span data-stu-id="ccea7-388">6,400</span></span> |<span data-ttu-id="ccea7-389">12,800</span><span class="sxs-lookup"><span data-stu-id="ccea7-389">12,800</span></span> |

<span data-ttu-id="ccea7-390">Z hello předcházející tabulce, můžete uvidíte, že dotaz spuštěný na DW2000 v hello **xlargerc** Třída prostředků by měla mít přístup too6, 400 MB paměti v každém z hello 60 distribuované databáze.</span><span class="sxs-lookup"><span data-stu-id="ccea7-390">From hello preceding table, you can see that a query running on a DW2000 in hello **xlargerc** resource class would have access too6,400 MB of memory within each of hello 60 distributed databases.</span></span>  <span data-ttu-id="ccea7-391">V SQL Data Warehouse jsou 60 distribuce.</span><span class="sxs-lookup"><span data-stu-id="ccea7-391">In SQL Data Warehouse, there are 60 distributions.</span></span> <span data-ttu-id="ccea7-392">Proto by měl toocalculate hello celkové paměti přidělení pro dotaz v třídě daný prostředek, hello nad hodnotu vynásobeny 60.</span><span class="sxs-lookup"><span data-stu-id="ccea7-392">Therefore, toocalculate hello total memory allocation for a query in a given resource class, hello above values should be multiplied by 60.</span></span>

### <a name="memory-allocations-system-wide-gb"></a><span data-ttu-id="ccea7-393">Paměť přidělení systémové (GB)</span><span class="sxs-lookup"><span data-stu-id="ccea7-393">Memory allocations system-wide (GB)</span></span>
| <span data-ttu-id="ccea7-394">DWU</span><span class="sxs-lookup"><span data-stu-id="ccea7-394">DWU</span></span> | <span data-ttu-id="ccea7-395">smallrc</span><span class="sxs-lookup"><span data-stu-id="ccea7-395">smallrc</span></span> | <span data-ttu-id="ccea7-396">mediumrc</span><span class="sxs-lookup"><span data-stu-id="ccea7-396">mediumrc</span></span> | <span data-ttu-id="ccea7-397">largerc</span><span class="sxs-lookup"><span data-stu-id="ccea7-397">largerc</span></span> | <span data-ttu-id="ccea7-398">xlargerc</span><span class="sxs-lookup"><span data-stu-id="ccea7-398">xlargerc</span></span> |
|:--- |:---:|:---:|:---:|:---:|
| <span data-ttu-id="ccea7-399">OD DW100</span><span class="sxs-lookup"><span data-stu-id="ccea7-399">DW100</span></span> |<span data-ttu-id="ccea7-400">6</span><span class="sxs-lookup"><span data-stu-id="ccea7-400">6</span></span> |<span data-ttu-id="ccea7-401">6</span><span class="sxs-lookup"><span data-stu-id="ccea7-401">6</span></span> |<span data-ttu-id="ccea7-402">12</span><span class="sxs-lookup"><span data-stu-id="ccea7-402">12</span></span> |<span data-ttu-id="ccea7-403">23</span><span class="sxs-lookup"><span data-stu-id="ccea7-403">23</span></span> |
| <span data-ttu-id="ccea7-404">DW200</span><span class="sxs-lookup"><span data-stu-id="ccea7-404">DW200</span></span> |<span data-ttu-id="ccea7-405">6</span><span class="sxs-lookup"><span data-stu-id="ccea7-405">6</span></span> |<span data-ttu-id="ccea7-406">12</span><span class="sxs-lookup"><span data-stu-id="ccea7-406">12</span></span> |<span data-ttu-id="ccea7-407">23</span><span class="sxs-lookup"><span data-stu-id="ccea7-407">23</span></span> |<span data-ttu-id="ccea7-408">47</span><span class="sxs-lookup"><span data-stu-id="ccea7-408">47</span></span> |
| <span data-ttu-id="ccea7-409">DW300</span><span class="sxs-lookup"><span data-stu-id="ccea7-409">DW300</span></span> |<span data-ttu-id="ccea7-410">6</span><span class="sxs-lookup"><span data-stu-id="ccea7-410">6</span></span> |<span data-ttu-id="ccea7-411">12</span><span class="sxs-lookup"><span data-stu-id="ccea7-411">12</span></span> |<span data-ttu-id="ccea7-412">23</span><span class="sxs-lookup"><span data-stu-id="ccea7-412">23</span></span> |<span data-ttu-id="ccea7-413">47</span><span class="sxs-lookup"><span data-stu-id="ccea7-413">47</span></span> |
| <span data-ttu-id="ccea7-414">DW400</span><span class="sxs-lookup"><span data-stu-id="ccea7-414">DW400</span></span> |<span data-ttu-id="ccea7-415">6</span><span class="sxs-lookup"><span data-stu-id="ccea7-415">6</span></span> |<span data-ttu-id="ccea7-416">23</span><span class="sxs-lookup"><span data-stu-id="ccea7-416">23</span></span> |<span data-ttu-id="ccea7-417">47</span><span class="sxs-lookup"><span data-stu-id="ccea7-417">47</span></span> |<span data-ttu-id="ccea7-418">94</span><span class="sxs-lookup"><span data-stu-id="ccea7-418">94</span></span> |
| <span data-ttu-id="ccea7-419">DW500</span><span class="sxs-lookup"><span data-stu-id="ccea7-419">DW500</span></span> |<span data-ttu-id="ccea7-420">6</span><span class="sxs-lookup"><span data-stu-id="ccea7-420">6</span></span> |<span data-ttu-id="ccea7-421">23</span><span class="sxs-lookup"><span data-stu-id="ccea7-421">23</span></span> |<span data-ttu-id="ccea7-422">47</span><span class="sxs-lookup"><span data-stu-id="ccea7-422">47</span></span> |<span data-ttu-id="ccea7-423">94</span><span class="sxs-lookup"><span data-stu-id="ccea7-423">94</span></span> |
| <span data-ttu-id="ccea7-424">DW600</span><span class="sxs-lookup"><span data-stu-id="ccea7-424">DW600</span></span> |<span data-ttu-id="ccea7-425">6</span><span class="sxs-lookup"><span data-stu-id="ccea7-425">6</span></span> |<span data-ttu-id="ccea7-426">23</span><span class="sxs-lookup"><span data-stu-id="ccea7-426">23</span></span> |<span data-ttu-id="ccea7-427">47</span><span class="sxs-lookup"><span data-stu-id="ccea7-427">47</span></span> |<span data-ttu-id="ccea7-428">94</span><span class="sxs-lookup"><span data-stu-id="ccea7-428">94</span></span> |
| <span data-ttu-id="ccea7-429">DW1000</span><span class="sxs-lookup"><span data-stu-id="ccea7-429">DW1000</span></span> |<span data-ttu-id="ccea7-430">6</span><span class="sxs-lookup"><span data-stu-id="ccea7-430">6</span></span> |<span data-ttu-id="ccea7-431">47</span><span class="sxs-lookup"><span data-stu-id="ccea7-431">47</span></span> |<span data-ttu-id="ccea7-432">94</span><span class="sxs-lookup"><span data-stu-id="ccea7-432">94</span></span> |<span data-ttu-id="ccea7-433">188</span><span class="sxs-lookup"><span data-stu-id="ccea7-433">188</span></span> |
| <span data-ttu-id="ccea7-434">DW1200</span><span class="sxs-lookup"><span data-stu-id="ccea7-434">DW1200</span></span> |<span data-ttu-id="ccea7-435">6</span><span class="sxs-lookup"><span data-stu-id="ccea7-435">6</span></span> |<span data-ttu-id="ccea7-436">47</span><span class="sxs-lookup"><span data-stu-id="ccea7-436">47</span></span> |<span data-ttu-id="ccea7-437">94</span><span class="sxs-lookup"><span data-stu-id="ccea7-437">94</span></span> |<span data-ttu-id="ccea7-438">188</span><span class="sxs-lookup"><span data-stu-id="ccea7-438">188</span></span> |
| <span data-ttu-id="ccea7-439">DW1500</span><span class="sxs-lookup"><span data-stu-id="ccea7-439">DW1500</span></span> |<span data-ttu-id="ccea7-440">6</span><span class="sxs-lookup"><span data-stu-id="ccea7-440">6</span></span> |<span data-ttu-id="ccea7-441">47</span><span class="sxs-lookup"><span data-stu-id="ccea7-441">47</span></span> |<span data-ttu-id="ccea7-442">94</span><span class="sxs-lookup"><span data-stu-id="ccea7-442">94</span></span> |<span data-ttu-id="ccea7-443">188</span><span class="sxs-lookup"><span data-stu-id="ccea7-443">188</span></span> |
| <span data-ttu-id="ccea7-444">DW2000</span><span class="sxs-lookup"><span data-stu-id="ccea7-444">DW2000</span></span> |<span data-ttu-id="ccea7-445">6</span><span class="sxs-lookup"><span data-stu-id="ccea7-445">6</span></span> |<span data-ttu-id="ccea7-446">94</span><span class="sxs-lookup"><span data-stu-id="ccea7-446">94</span></span> |<span data-ttu-id="ccea7-447">188</span><span class="sxs-lookup"><span data-stu-id="ccea7-447">188</span></span> |<span data-ttu-id="ccea7-448">375</span><span class="sxs-lookup"><span data-stu-id="ccea7-448">375</span></span> |
| <span data-ttu-id="ccea7-449">DW3000</span><span class="sxs-lookup"><span data-stu-id="ccea7-449">DW3000</span></span> |<span data-ttu-id="ccea7-450">6</span><span class="sxs-lookup"><span data-stu-id="ccea7-450">6</span></span> |<span data-ttu-id="ccea7-451">94</span><span class="sxs-lookup"><span data-stu-id="ccea7-451">94</span></span> |<span data-ttu-id="ccea7-452">188</span><span class="sxs-lookup"><span data-stu-id="ccea7-452">188</span></span> |<span data-ttu-id="ccea7-453">375</span><span class="sxs-lookup"><span data-stu-id="ccea7-453">375</span></span> |
| <span data-ttu-id="ccea7-454">DW6000</span><span class="sxs-lookup"><span data-stu-id="ccea7-454">DW6000</span></span> |<span data-ttu-id="ccea7-455">6</span><span class="sxs-lookup"><span data-stu-id="ccea7-455">6</span></span> |<span data-ttu-id="ccea7-456">188</span><span class="sxs-lookup"><span data-stu-id="ccea7-456">188</span></span> |<span data-ttu-id="ccea7-457">375</span><span class="sxs-lookup"><span data-stu-id="ccea7-457">375</span></span> |<span data-ttu-id="ccea7-458">750</span><span class="sxs-lookup"><span data-stu-id="ccea7-458">750</span></span> |

<span data-ttu-id="ccea7-459">Z této tabulky přidělených systémové paměti, uvidíte, že dotaz spuštěný na DW2000 ve třídě prostředků xlargerc hello je přidělen celkem 375 GB paměti (6 400 MB * 60 distribuce / 1 024 tooconvert tooGB) přes hello celého SQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="ccea7-459">From this table of system-wide memory allocations, you can see that a query running on a DW2000 in hello xlargerc resource class is allocated a total of 375 GB of memory (6,400 MB * 60 distributions / 1,024 tooconvert tooGB) over hello entirety of your SQL Data Warehouse.</span></span>

<span data-ttu-id="ccea7-460">Hello stejný výpočet platí toostatic třídy prostředků.</span><span class="sxs-lookup"><span data-stu-id="ccea7-460">hello same calculation applies toostatic resource classes.</span></span>
 
## <a name="concurrency-slot-consumption"></a><span data-ttu-id="ccea7-461">Spotřeba slotu souběžnosti</span><span class="sxs-lookup"><span data-stu-id="ccea7-461">Concurrency slot consumption</span></span>  
<span data-ttu-id="ccea7-462">SQL Data Warehouse uděluje více paměti tooqueries spuštěné v vyšší třídy prostředků.</span><span class="sxs-lookup"><span data-stu-id="ccea7-462">SQL Data Warehouse grants more memory tooqueries running in higher resource classes.</span></span> <span data-ttu-id="ccea7-463">Velikost paměti je pevné prostředků.</span><span class="sxs-lookup"><span data-stu-id="ccea7-463">Memory is a fixed resource.</span></span>  <span data-ttu-id="ccea7-464">Proto hello více paměti přidělené na dotaz, hello, které můžete provést méně souběžných dotazů.</span><span class="sxs-lookup"><span data-stu-id="ccea7-464">Therefore, hello more memory allocated per query, hello fewer concurrent queries can execute.</span></span> <span data-ttu-id="ccea7-465">Hello následující tabulka opětovně uvádí, že všechny hello předchozí konceptů v rámci jednoho zobrazení, která zobrazuje počet hello sloty souběžnosti podle DWU a hello sloty spotřebovávají každá třída prostředků.</span><span class="sxs-lookup"><span data-stu-id="ccea7-465">hello following table reiterates all of hello previous concepts in a single view that shows hello number of concurrency slots available by DWU and hello slots consumed by each resource class.</span></span>  

### <a name="allocation-and-consumption-of-concurrency-slots-for-dynamic-resource-classes"></a><span data-ttu-id="ccea7-466">Přidělení a využití souběžnosti přihrádky pro dynamické prostředků třídy</span><span class="sxs-lookup"><span data-stu-id="ccea7-466">Allocation and consumption of concurrency slots for dynamic resource classes</span></span>  
| <span data-ttu-id="ccea7-467">DWU</span><span class="sxs-lookup"><span data-stu-id="ccea7-467">DWU</span></span> | <span data-ttu-id="ccea7-468">Maximální počet souběžných dotazů</span><span class="sxs-lookup"><span data-stu-id="ccea7-468">Maximum concurrent queries</span></span> | <span data-ttu-id="ccea7-469">Přidělené sloty souběžnosti</span><span class="sxs-lookup"><span data-stu-id="ccea7-469">Concurrency slots allocated</span></span> | <span data-ttu-id="ccea7-470">Sloty používané smallrc</span><span class="sxs-lookup"><span data-stu-id="ccea7-470">Slots used by smallrc</span></span> | <span data-ttu-id="ccea7-471">Sloty používané mediumrc</span><span class="sxs-lookup"><span data-stu-id="ccea7-471">Slots used by mediumrc</span></span> | <span data-ttu-id="ccea7-472">Sloty používané largerc</span><span class="sxs-lookup"><span data-stu-id="ccea7-472">Slots used by largerc</span></span> | <span data-ttu-id="ccea7-473">Sloty používané xlargerc</span><span class="sxs-lookup"><span data-stu-id="ccea7-473">Slots used by xlargerc</span></span> |
|:--- |:---:|:---:|:---:|:---:|:---:|:---:|
| <span data-ttu-id="ccea7-474">OD DW100</span><span class="sxs-lookup"><span data-stu-id="ccea7-474">DW100</span></span> |<span data-ttu-id="ccea7-475">4</span><span class="sxs-lookup"><span data-stu-id="ccea7-475">4</span></span> |<span data-ttu-id="ccea7-476">4</span><span class="sxs-lookup"><span data-stu-id="ccea7-476">4</span></span> |<span data-ttu-id="ccea7-477">1</span><span class="sxs-lookup"><span data-stu-id="ccea7-477">1</span></span> |<span data-ttu-id="ccea7-478">1</span><span class="sxs-lookup"><span data-stu-id="ccea7-478">1</span></span> |<span data-ttu-id="ccea7-479">2</span><span class="sxs-lookup"><span data-stu-id="ccea7-479">2</span></span> |<span data-ttu-id="ccea7-480">4</span><span class="sxs-lookup"><span data-stu-id="ccea7-480">4</span></span> |
| <span data-ttu-id="ccea7-481">DW200</span><span class="sxs-lookup"><span data-stu-id="ccea7-481">DW200</span></span> |<span data-ttu-id="ccea7-482">8</span><span class="sxs-lookup"><span data-stu-id="ccea7-482">8</span></span> |<span data-ttu-id="ccea7-483">8</span><span class="sxs-lookup"><span data-stu-id="ccea7-483">8</span></span> |<span data-ttu-id="ccea7-484">1</span><span class="sxs-lookup"><span data-stu-id="ccea7-484">1</span></span> |<span data-ttu-id="ccea7-485">2</span><span class="sxs-lookup"><span data-stu-id="ccea7-485">2</span></span> |<span data-ttu-id="ccea7-486">4</span><span class="sxs-lookup"><span data-stu-id="ccea7-486">4</span></span> |<span data-ttu-id="ccea7-487">8</span><span class="sxs-lookup"><span data-stu-id="ccea7-487">8</span></span> |
| <span data-ttu-id="ccea7-488">DW300</span><span class="sxs-lookup"><span data-stu-id="ccea7-488">DW300</span></span> |<span data-ttu-id="ccea7-489">12</span><span class="sxs-lookup"><span data-stu-id="ccea7-489">12</span></span> |<span data-ttu-id="ccea7-490">12</span><span class="sxs-lookup"><span data-stu-id="ccea7-490">12</span></span> |<span data-ttu-id="ccea7-491">1</span><span class="sxs-lookup"><span data-stu-id="ccea7-491">1</span></span> |<span data-ttu-id="ccea7-492">2</span><span class="sxs-lookup"><span data-stu-id="ccea7-492">2</span></span> |<span data-ttu-id="ccea7-493">4</span><span class="sxs-lookup"><span data-stu-id="ccea7-493">4</span></span> |<span data-ttu-id="ccea7-494">8</span><span class="sxs-lookup"><span data-stu-id="ccea7-494">8</span></span> |
| <span data-ttu-id="ccea7-495">DW400</span><span class="sxs-lookup"><span data-stu-id="ccea7-495">DW400</span></span> |<span data-ttu-id="ccea7-496">16</span><span class="sxs-lookup"><span data-stu-id="ccea7-496">16</span></span> |<span data-ttu-id="ccea7-497">16</span><span class="sxs-lookup"><span data-stu-id="ccea7-497">16</span></span> |<span data-ttu-id="ccea7-498">1</span><span class="sxs-lookup"><span data-stu-id="ccea7-498">1</span></span> |<span data-ttu-id="ccea7-499">4</span><span class="sxs-lookup"><span data-stu-id="ccea7-499">4</span></span> |<span data-ttu-id="ccea7-500">8</span><span class="sxs-lookup"><span data-stu-id="ccea7-500">8</span></span> |<span data-ttu-id="ccea7-501">16</span><span class="sxs-lookup"><span data-stu-id="ccea7-501">16</span></span> |
| <span data-ttu-id="ccea7-502">DW500</span><span class="sxs-lookup"><span data-stu-id="ccea7-502">DW500</span></span> |<span data-ttu-id="ccea7-503">20</span><span class="sxs-lookup"><span data-stu-id="ccea7-503">20</span></span> |<span data-ttu-id="ccea7-504">20</span><span class="sxs-lookup"><span data-stu-id="ccea7-504">20</span></span> |<span data-ttu-id="ccea7-505">1</span><span class="sxs-lookup"><span data-stu-id="ccea7-505">1</span></span> |<span data-ttu-id="ccea7-506">4</span><span class="sxs-lookup"><span data-stu-id="ccea7-506">4</span></span> |<span data-ttu-id="ccea7-507">8</span><span class="sxs-lookup"><span data-stu-id="ccea7-507">8</span></span> |<span data-ttu-id="ccea7-508">16</span><span class="sxs-lookup"><span data-stu-id="ccea7-508">16</span></span> |
| <span data-ttu-id="ccea7-509">DW600</span><span class="sxs-lookup"><span data-stu-id="ccea7-509">DW600</span></span> |<span data-ttu-id="ccea7-510">24</span><span class="sxs-lookup"><span data-stu-id="ccea7-510">24</span></span> |<span data-ttu-id="ccea7-511">24</span><span class="sxs-lookup"><span data-stu-id="ccea7-511">24</span></span> |<span data-ttu-id="ccea7-512">1</span><span class="sxs-lookup"><span data-stu-id="ccea7-512">1</span></span> |<span data-ttu-id="ccea7-513">4</span><span class="sxs-lookup"><span data-stu-id="ccea7-513">4</span></span> |<span data-ttu-id="ccea7-514">8</span><span class="sxs-lookup"><span data-stu-id="ccea7-514">8</span></span> |<span data-ttu-id="ccea7-515">16</span><span class="sxs-lookup"><span data-stu-id="ccea7-515">16</span></span> |
| <span data-ttu-id="ccea7-516">DW1000</span><span class="sxs-lookup"><span data-stu-id="ccea7-516">DW1000</span></span> |<span data-ttu-id="ccea7-517">32</span><span class="sxs-lookup"><span data-stu-id="ccea7-517">32</span></span> |<span data-ttu-id="ccea7-518">40</span><span class="sxs-lookup"><span data-stu-id="ccea7-518">40</span></span> |<span data-ttu-id="ccea7-519">1</span><span class="sxs-lookup"><span data-stu-id="ccea7-519">1</span></span> |<span data-ttu-id="ccea7-520">8</span><span class="sxs-lookup"><span data-stu-id="ccea7-520">8</span></span> |<span data-ttu-id="ccea7-521">16</span><span class="sxs-lookup"><span data-stu-id="ccea7-521">16</span></span> |<span data-ttu-id="ccea7-522">32</span><span class="sxs-lookup"><span data-stu-id="ccea7-522">32</span></span> |
| <span data-ttu-id="ccea7-523">DW1200</span><span class="sxs-lookup"><span data-stu-id="ccea7-523">DW1200</span></span> |<span data-ttu-id="ccea7-524">32</span><span class="sxs-lookup"><span data-stu-id="ccea7-524">32</span></span> |<span data-ttu-id="ccea7-525">48</span><span class="sxs-lookup"><span data-stu-id="ccea7-525">48</span></span> |<span data-ttu-id="ccea7-526">1</span><span class="sxs-lookup"><span data-stu-id="ccea7-526">1</span></span> |<span data-ttu-id="ccea7-527">8</span><span class="sxs-lookup"><span data-stu-id="ccea7-527">8</span></span> |<span data-ttu-id="ccea7-528">16</span><span class="sxs-lookup"><span data-stu-id="ccea7-528">16</span></span> |<span data-ttu-id="ccea7-529">32</span><span class="sxs-lookup"><span data-stu-id="ccea7-529">32</span></span> |
| <span data-ttu-id="ccea7-530">DW1500</span><span class="sxs-lookup"><span data-stu-id="ccea7-530">DW1500</span></span> |<span data-ttu-id="ccea7-531">32</span><span class="sxs-lookup"><span data-stu-id="ccea7-531">32</span></span> |<span data-ttu-id="ccea7-532">60</span><span class="sxs-lookup"><span data-stu-id="ccea7-532">60</span></span> |<span data-ttu-id="ccea7-533">1</span><span class="sxs-lookup"><span data-stu-id="ccea7-533">1</span></span> |<span data-ttu-id="ccea7-534">8</span><span class="sxs-lookup"><span data-stu-id="ccea7-534">8</span></span> |<span data-ttu-id="ccea7-535">16</span><span class="sxs-lookup"><span data-stu-id="ccea7-535">16</span></span> |<span data-ttu-id="ccea7-536">32</span><span class="sxs-lookup"><span data-stu-id="ccea7-536">32</span></span> |
| <span data-ttu-id="ccea7-537">DW2000</span><span class="sxs-lookup"><span data-stu-id="ccea7-537">DW2000</span></span> |<span data-ttu-id="ccea7-538">32</span><span class="sxs-lookup"><span data-stu-id="ccea7-538">32</span></span> |<span data-ttu-id="ccea7-539">80</span><span class="sxs-lookup"><span data-stu-id="ccea7-539">80</span></span> |<span data-ttu-id="ccea7-540">1</span><span class="sxs-lookup"><span data-stu-id="ccea7-540">1</span></span> |<span data-ttu-id="ccea7-541">16</span><span class="sxs-lookup"><span data-stu-id="ccea7-541">16</span></span> |<span data-ttu-id="ccea7-542">32</span><span class="sxs-lookup"><span data-stu-id="ccea7-542">32</span></span> |<span data-ttu-id="ccea7-543">64</span><span class="sxs-lookup"><span data-stu-id="ccea7-543">64</span></span> |
| <span data-ttu-id="ccea7-544">DW3000</span><span class="sxs-lookup"><span data-stu-id="ccea7-544">DW3000</span></span> |<span data-ttu-id="ccea7-545">32</span><span class="sxs-lookup"><span data-stu-id="ccea7-545">32</span></span> |<span data-ttu-id="ccea7-546">120</span><span class="sxs-lookup"><span data-stu-id="ccea7-546">120</span></span> |<span data-ttu-id="ccea7-547">1</span><span class="sxs-lookup"><span data-stu-id="ccea7-547">1</span></span> |<span data-ttu-id="ccea7-548">16</span><span class="sxs-lookup"><span data-stu-id="ccea7-548">16</span></span> |<span data-ttu-id="ccea7-549">32</span><span class="sxs-lookup"><span data-stu-id="ccea7-549">32</span></span> |<span data-ttu-id="ccea7-550">64</span><span class="sxs-lookup"><span data-stu-id="ccea7-550">64</span></span> |
| <span data-ttu-id="ccea7-551">DW6000</span><span class="sxs-lookup"><span data-stu-id="ccea7-551">DW6000</span></span> |<span data-ttu-id="ccea7-552">32</span><span class="sxs-lookup"><span data-stu-id="ccea7-552">32</span></span> |<span data-ttu-id="ccea7-553">240</span><span class="sxs-lookup"><span data-stu-id="ccea7-553">240</span></span> |<span data-ttu-id="ccea7-554">1</span><span class="sxs-lookup"><span data-stu-id="ccea7-554">1</span></span> |<span data-ttu-id="ccea7-555">32</span><span class="sxs-lookup"><span data-stu-id="ccea7-555">32</span></span> |<span data-ttu-id="ccea7-556">64</span><span class="sxs-lookup"><span data-stu-id="ccea7-556">64</span></span> |<span data-ttu-id="ccea7-557">128</span><span class="sxs-lookup"><span data-stu-id="ccea7-557">128</span></span> |

### <a name="allocation-and-consumption-of-concurrency-slots-for-static-resource-classes"></a><span data-ttu-id="ccea7-558">Přidělení a využití souběžnosti přihrádky pro statické prostředků třídy</span><span class="sxs-lookup"><span data-stu-id="ccea7-558">Allocation and consumption of concurrency slots for static resource classes</span></span>  
| <span data-ttu-id="ccea7-559">DWU</span><span class="sxs-lookup"><span data-stu-id="ccea7-559">DWU</span></span> | <span data-ttu-id="ccea7-560">Maximální počet souběžných dotazů</span><span class="sxs-lookup"><span data-stu-id="ccea7-560">Maximum concurrent queries</span></span> | <span data-ttu-id="ccea7-561">Přidělené sloty souběžnosti</span><span class="sxs-lookup"><span data-stu-id="ccea7-561">Concurrency slots allocated</span></span> |<span data-ttu-id="ccea7-562">staticrc10</span><span class="sxs-lookup"><span data-stu-id="ccea7-562">staticrc10</span></span> | <span data-ttu-id="ccea7-563">staticrc20</span><span class="sxs-lookup"><span data-stu-id="ccea7-563">staticrc20</span></span> | <span data-ttu-id="ccea7-564">staticrc30</span><span class="sxs-lookup"><span data-stu-id="ccea7-564">staticrc30</span></span> | <span data-ttu-id="ccea7-565">staticrc40</span><span class="sxs-lookup"><span data-stu-id="ccea7-565">staticrc40</span></span> | <span data-ttu-id="ccea7-566">staticrc50</span><span class="sxs-lookup"><span data-stu-id="ccea7-566">staticrc50</span></span> | <span data-ttu-id="ccea7-567">staticrc60</span><span class="sxs-lookup"><span data-stu-id="ccea7-567">staticrc60</span></span> | <span data-ttu-id="ccea7-568">staticrc70</span><span class="sxs-lookup"><span data-stu-id="ccea7-568">staticrc70</span></span> | <span data-ttu-id="ccea7-569">staticrc80</span><span class="sxs-lookup"><span data-stu-id="ccea7-569">staticrc80</span></span> |
|:--- |:---:|:---:|:---:|:---:|:---:|:---:|:---:|:---:|:---:|:---:|
| <span data-ttu-id="ccea7-570">OD DW100</span><span class="sxs-lookup"><span data-stu-id="ccea7-570">DW100</span></span> |<span data-ttu-id="ccea7-571">4</span><span class="sxs-lookup"><span data-stu-id="ccea7-571">4</span></span> |<span data-ttu-id="ccea7-572">4</span><span class="sxs-lookup"><span data-stu-id="ccea7-572">4</span></span> |<span data-ttu-id="ccea7-573">1</span><span class="sxs-lookup"><span data-stu-id="ccea7-573">1</span></span> |<span data-ttu-id="ccea7-574">2</span><span class="sxs-lookup"><span data-stu-id="ccea7-574">2</span></span> |<span data-ttu-id="ccea7-575">4</span><span class="sxs-lookup"><span data-stu-id="ccea7-575">4</span></span> |<span data-ttu-id="ccea7-576">4</span><span class="sxs-lookup"><span data-stu-id="ccea7-576">4</span></span> |<span data-ttu-id="ccea7-577">4</span><span class="sxs-lookup"><span data-stu-id="ccea7-577">4</span></span> |<span data-ttu-id="ccea7-578">4</span><span class="sxs-lookup"><span data-stu-id="ccea7-578">4</span></span> |<span data-ttu-id="ccea7-579">4</span><span class="sxs-lookup"><span data-stu-id="ccea7-579">4</span></span> |<span data-ttu-id="ccea7-580">4</span><span class="sxs-lookup"><span data-stu-id="ccea7-580">4</span></span> |
| <span data-ttu-id="ccea7-581">DW200</span><span class="sxs-lookup"><span data-stu-id="ccea7-581">DW200</span></span> |<span data-ttu-id="ccea7-582">8</span><span class="sxs-lookup"><span data-stu-id="ccea7-582">8</span></span> |<span data-ttu-id="ccea7-583">8</span><span class="sxs-lookup"><span data-stu-id="ccea7-583">8</span></span> |<span data-ttu-id="ccea7-584">1</span><span class="sxs-lookup"><span data-stu-id="ccea7-584">1</span></span> |<span data-ttu-id="ccea7-585">2</span><span class="sxs-lookup"><span data-stu-id="ccea7-585">2</span></span> |<span data-ttu-id="ccea7-586">4</span><span class="sxs-lookup"><span data-stu-id="ccea7-586">4</span></span> |<span data-ttu-id="ccea7-587">8</span><span class="sxs-lookup"><span data-stu-id="ccea7-587">8</span></span> |<span data-ttu-id="ccea7-588">8</span><span class="sxs-lookup"><span data-stu-id="ccea7-588">8</span></span> |<span data-ttu-id="ccea7-589">8</span><span class="sxs-lookup"><span data-stu-id="ccea7-589">8</span></span> |<span data-ttu-id="ccea7-590">8</span><span class="sxs-lookup"><span data-stu-id="ccea7-590">8</span></span> |<span data-ttu-id="ccea7-591">8</span><span class="sxs-lookup"><span data-stu-id="ccea7-591">8</span></span> |
| <span data-ttu-id="ccea7-592">DW300</span><span class="sxs-lookup"><span data-stu-id="ccea7-592">DW300</span></span> |<span data-ttu-id="ccea7-593">12</span><span class="sxs-lookup"><span data-stu-id="ccea7-593">12</span></span> |<span data-ttu-id="ccea7-594">12</span><span class="sxs-lookup"><span data-stu-id="ccea7-594">12</span></span> |<span data-ttu-id="ccea7-595">1</span><span class="sxs-lookup"><span data-stu-id="ccea7-595">1</span></span> |<span data-ttu-id="ccea7-596">2</span><span class="sxs-lookup"><span data-stu-id="ccea7-596">2</span></span> |<span data-ttu-id="ccea7-597">4</span><span class="sxs-lookup"><span data-stu-id="ccea7-597">4</span></span> |<span data-ttu-id="ccea7-598">8</span><span class="sxs-lookup"><span data-stu-id="ccea7-598">8</span></span> |<span data-ttu-id="ccea7-599">8</span><span class="sxs-lookup"><span data-stu-id="ccea7-599">8</span></span> |<span data-ttu-id="ccea7-600">8</span><span class="sxs-lookup"><span data-stu-id="ccea7-600">8</span></span> |<span data-ttu-id="ccea7-601">8</span><span class="sxs-lookup"><span data-stu-id="ccea7-601">8</span></span> |<span data-ttu-id="ccea7-602">8</span><span class="sxs-lookup"><span data-stu-id="ccea7-602">8</span></span> |
| <span data-ttu-id="ccea7-603">DW400</span><span class="sxs-lookup"><span data-stu-id="ccea7-603">DW400</span></span> |<span data-ttu-id="ccea7-604">16</span><span class="sxs-lookup"><span data-stu-id="ccea7-604">16</span></span> |<span data-ttu-id="ccea7-605">16</span><span class="sxs-lookup"><span data-stu-id="ccea7-605">16</span></span> |<span data-ttu-id="ccea7-606">1</span><span class="sxs-lookup"><span data-stu-id="ccea7-606">1</span></span> |<span data-ttu-id="ccea7-607">2</span><span class="sxs-lookup"><span data-stu-id="ccea7-607">2</span></span> |<span data-ttu-id="ccea7-608">4</span><span class="sxs-lookup"><span data-stu-id="ccea7-608">4</span></span> |<span data-ttu-id="ccea7-609">8</span><span class="sxs-lookup"><span data-stu-id="ccea7-609">8</span></span> |<span data-ttu-id="ccea7-610">16</span><span class="sxs-lookup"><span data-stu-id="ccea7-610">16</span></span> |<span data-ttu-id="ccea7-611">16</span><span class="sxs-lookup"><span data-stu-id="ccea7-611">16</span></span> |<span data-ttu-id="ccea7-612">16</span><span class="sxs-lookup"><span data-stu-id="ccea7-612">16</span></span> |<span data-ttu-id="ccea7-613">16</span><span class="sxs-lookup"><span data-stu-id="ccea7-613">16</span></span> |
| <span data-ttu-id="ccea7-614">DW500</span><span class="sxs-lookup"><span data-stu-id="ccea7-614">DW500</span></span> | <span data-ttu-id="ccea7-615">20</span><span class="sxs-lookup"><span data-stu-id="ccea7-615">20</span></span>| <span data-ttu-id="ccea7-616">20</span><span class="sxs-lookup"><span data-stu-id="ccea7-616">20</span></span>| <span data-ttu-id="ccea7-617">1</span><span class="sxs-lookup"><span data-stu-id="ccea7-617">1</span></span>| <span data-ttu-id="ccea7-618">2</span><span class="sxs-lookup"><span data-stu-id="ccea7-618">2</span></span>| <span data-ttu-id="ccea7-619">4</span><span class="sxs-lookup"><span data-stu-id="ccea7-619">4</span></span>| <span data-ttu-id="ccea7-620">8</span><span class="sxs-lookup"><span data-stu-id="ccea7-620">8</span></span>| <span data-ttu-id="ccea7-621">16</span><span class="sxs-lookup"><span data-stu-id="ccea7-621">16</span></span>| <span data-ttu-id="ccea7-622">16</span><span class="sxs-lookup"><span data-stu-id="ccea7-622">16</span></span>| <span data-ttu-id="ccea7-623">16</span><span class="sxs-lookup"><span data-stu-id="ccea7-623">16</span></span>| <span data-ttu-id="ccea7-624">16</span><span class="sxs-lookup"><span data-stu-id="ccea7-624">16</span></span>|
| <span data-ttu-id="ccea7-625">DW600</span><span class="sxs-lookup"><span data-stu-id="ccea7-625">DW600</span></span> | <span data-ttu-id="ccea7-626">24</span><span class="sxs-lookup"><span data-stu-id="ccea7-626">24</span></span>| <span data-ttu-id="ccea7-627">24</span><span class="sxs-lookup"><span data-stu-id="ccea7-627">24</span></span>| <span data-ttu-id="ccea7-628">1</span><span class="sxs-lookup"><span data-stu-id="ccea7-628">1</span></span>| <span data-ttu-id="ccea7-629">2</span><span class="sxs-lookup"><span data-stu-id="ccea7-629">2</span></span>| <span data-ttu-id="ccea7-630">4</span><span class="sxs-lookup"><span data-stu-id="ccea7-630">4</span></span>| <span data-ttu-id="ccea7-631">8</span><span class="sxs-lookup"><span data-stu-id="ccea7-631">8</span></span>| <span data-ttu-id="ccea7-632">16</span><span class="sxs-lookup"><span data-stu-id="ccea7-632">16</span></span>| <span data-ttu-id="ccea7-633">16</span><span class="sxs-lookup"><span data-stu-id="ccea7-633">16</span></span>| <span data-ttu-id="ccea7-634">16</span><span class="sxs-lookup"><span data-stu-id="ccea7-634">16</span></span>| <span data-ttu-id="ccea7-635">16</span><span class="sxs-lookup"><span data-stu-id="ccea7-635">16</span></span>|
| <span data-ttu-id="ccea7-636">DW1000</span><span class="sxs-lookup"><span data-stu-id="ccea7-636">DW1000</span></span> | <span data-ttu-id="ccea7-637">32</span><span class="sxs-lookup"><span data-stu-id="ccea7-637">32</span></span>| <span data-ttu-id="ccea7-638">40</span><span class="sxs-lookup"><span data-stu-id="ccea7-638">40</span></span>| <span data-ttu-id="ccea7-639">1</span><span class="sxs-lookup"><span data-stu-id="ccea7-639">1</span></span>| <span data-ttu-id="ccea7-640">2</span><span class="sxs-lookup"><span data-stu-id="ccea7-640">2</span></span>| <span data-ttu-id="ccea7-641">4</span><span class="sxs-lookup"><span data-stu-id="ccea7-641">4</span></span>| <span data-ttu-id="ccea7-642">8</span><span class="sxs-lookup"><span data-stu-id="ccea7-642">8</span></span>| <span data-ttu-id="ccea7-643">16</span><span class="sxs-lookup"><span data-stu-id="ccea7-643">16</span></span>| <span data-ttu-id="ccea7-644">32</span><span class="sxs-lookup"><span data-stu-id="ccea7-644">32</span></span>| <span data-ttu-id="ccea7-645">32</span><span class="sxs-lookup"><span data-stu-id="ccea7-645">32</span></span>| <span data-ttu-id="ccea7-646">32</span><span class="sxs-lookup"><span data-stu-id="ccea7-646">32</span></span>|
| <span data-ttu-id="ccea7-647">DW1200</span><span class="sxs-lookup"><span data-stu-id="ccea7-647">DW1200</span></span> | <span data-ttu-id="ccea7-648">32</span><span class="sxs-lookup"><span data-stu-id="ccea7-648">32</span></span>| <span data-ttu-id="ccea7-649">48</span><span class="sxs-lookup"><span data-stu-id="ccea7-649">48</span></span>| <span data-ttu-id="ccea7-650">1</span><span class="sxs-lookup"><span data-stu-id="ccea7-650">1</span></span>| <span data-ttu-id="ccea7-651">2</span><span class="sxs-lookup"><span data-stu-id="ccea7-651">2</span></span>| <span data-ttu-id="ccea7-652">4</span><span class="sxs-lookup"><span data-stu-id="ccea7-652">4</span></span>| <span data-ttu-id="ccea7-653">8</span><span class="sxs-lookup"><span data-stu-id="ccea7-653">8</span></span>| <span data-ttu-id="ccea7-654">16</span><span class="sxs-lookup"><span data-stu-id="ccea7-654">16</span></span>| <span data-ttu-id="ccea7-655">32</span><span class="sxs-lookup"><span data-stu-id="ccea7-655">32</span></span>| <span data-ttu-id="ccea7-656">32</span><span class="sxs-lookup"><span data-stu-id="ccea7-656">32</span></span>| <span data-ttu-id="ccea7-657">32</span><span class="sxs-lookup"><span data-stu-id="ccea7-657">32</span></span>|
| <span data-ttu-id="ccea7-658">DW1500</span><span class="sxs-lookup"><span data-stu-id="ccea7-658">DW1500</span></span> | <span data-ttu-id="ccea7-659">32</span><span class="sxs-lookup"><span data-stu-id="ccea7-659">32</span></span>| <span data-ttu-id="ccea7-660">60</span><span class="sxs-lookup"><span data-stu-id="ccea7-660">60</span></span>| <span data-ttu-id="ccea7-661">1</span><span class="sxs-lookup"><span data-stu-id="ccea7-661">1</span></span>| <span data-ttu-id="ccea7-662">2</span><span class="sxs-lookup"><span data-stu-id="ccea7-662">2</span></span>| <span data-ttu-id="ccea7-663">4</span><span class="sxs-lookup"><span data-stu-id="ccea7-663">4</span></span>| <span data-ttu-id="ccea7-664">8</span><span class="sxs-lookup"><span data-stu-id="ccea7-664">8</span></span>| <span data-ttu-id="ccea7-665">16</span><span class="sxs-lookup"><span data-stu-id="ccea7-665">16</span></span>| <span data-ttu-id="ccea7-666">32</span><span class="sxs-lookup"><span data-stu-id="ccea7-666">32</span></span>| <span data-ttu-id="ccea7-667">32</span><span class="sxs-lookup"><span data-stu-id="ccea7-667">32</span></span>| <span data-ttu-id="ccea7-668">32</span><span class="sxs-lookup"><span data-stu-id="ccea7-668">32</span></span>|
| <span data-ttu-id="ccea7-669">DW2000</span><span class="sxs-lookup"><span data-stu-id="ccea7-669">DW2000</span></span> | <span data-ttu-id="ccea7-670">32</span><span class="sxs-lookup"><span data-stu-id="ccea7-670">32</span></span>| <span data-ttu-id="ccea7-671">80</span><span class="sxs-lookup"><span data-stu-id="ccea7-671">80</span></span>| <span data-ttu-id="ccea7-672">1</span><span class="sxs-lookup"><span data-stu-id="ccea7-672">1</span></span>| <span data-ttu-id="ccea7-673">2</span><span class="sxs-lookup"><span data-stu-id="ccea7-673">2</span></span>| <span data-ttu-id="ccea7-674">4</span><span class="sxs-lookup"><span data-stu-id="ccea7-674">4</span></span>| <span data-ttu-id="ccea7-675">8</span><span class="sxs-lookup"><span data-stu-id="ccea7-675">8</span></span>| <span data-ttu-id="ccea7-676">16</span><span class="sxs-lookup"><span data-stu-id="ccea7-676">16</span></span>| <span data-ttu-id="ccea7-677">32</span><span class="sxs-lookup"><span data-stu-id="ccea7-677">32</span></span>| <span data-ttu-id="ccea7-678">64</span><span class="sxs-lookup"><span data-stu-id="ccea7-678">64</span></span>| <span data-ttu-id="ccea7-679">64</span><span class="sxs-lookup"><span data-stu-id="ccea7-679">64</span></span>|
| <span data-ttu-id="ccea7-680">DW3000</span><span class="sxs-lookup"><span data-stu-id="ccea7-680">DW3000</span></span> | <span data-ttu-id="ccea7-681">32</span><span class="sxs-lookup"><span data-stu-id="ccea7-681">32</span></span>| <span data-ttu-id="ccea7-682">120</span><span class="sxs-lookup"><span data-stu-id="ccea7-682">120</span></span>| <span data-ttu-id="ccea7-683">1</span><span class="sxs-lookup"><span data-stu-id="ccea7-683">1</span></span>| <span data-ttu-id="ccea7-684">2</span><span class="sxs-lookup"><span data-stu-id="ccea7-684">2</span></span>| <span data-ttu-id="ccea7-685">4</span><span class="sxs-lookup"><span data-stu-id="ccea7-685">4</span></span>| <span data-ttu-id="ccea7-686">8</span><span class="sxs-lookup"><span data-stu-id="ccea7-686">8</span></span>| <span data-ttu-id="ccea7-687">16</span><span class="sxs-lookup"><span data-stu-id="ccea7-687">16</span></span>| <span data-ttu-id="ccea7-688">32</span><span class="sxs-lookup"><span data-stu-id="ccea7-688">32</span></span>| <span data-ttu-id="ccea7-689">64</span><span class="sxs-lookup"><span data-stu-id="ccea7-689">64</span></span>| <span data-ttu-id="ccea7-690">64</span><span class="sxs-lookup"><span data-stu-id="ccea7-690">64</span></span>|
| <span data-ttu-id="ccea7-691">DW6000</span><span class="sxs-lookup"><span data-stu-id="ccea7-691">DW6000</span></span> | <span data-ttu-id="ccea7-692">32</span><span class="sxs-lookup"><span data-stu-id="ccea7-692">32</span></span>| <span data-ttu-id="ccea7-693">240</span><span class="sxs-lookup"><span data-stu-id="ccea7-693">240</span></span>| <span data-ttu-id="ccea7-694">1</span><span class="sxs-lookup"><span data-stu-id="ccea7-694">1</span></span>| <span data-ttu-id="ccea7-695">2</span><span class="sxs-lookup"><span data-stu-id="ccea7-695">2</span></span>| <span data-ttu-id="ccea7-696">4</span><span class="sxs-lookup"><span data-stu-id="ccea7-696">4</span></span>| <span data-ttu-id="ccea7-697">8</span><span class="sxs-lookup"><span data-stu-id="ccea7-697">8</span></span>| <span data-ttu-id="ccea7-698">16</span><span class="sxs-lookup"><span data-stu-id="ccea7-698">16</span></span>| <span data-ttu-id="ccea7-699">32</span><span class="sxs-lookup"><span data-stu-id="ccea7-699">32</span></span>| <span data-ttu-id="ccea7-700">64</span><span class="sxs-lookup"><span data-stu-id="ccea7-700">64</span></span>| <span data-ttu-id="ccea7-701">128</span><span class="sxs-lookup"><span data-stu-id="ccea7-701">128</span></span>|

<span data-ttu-id="ccea7-702">Z těchto tabulek zobrazí se systémem SQL Data Warehouse jako DW1000 přiděluje maximálně 32 souběžných dotazů a celkem 40 sloty souběžnosti.</span><span class="sxs-lookup"><span data-stu-id="ccea7-702">From these tables, you can see that SQL Data Warehouse running as DW1000 allocates a maximum of 32 concurrent queries and a total of 40 concurrency slots.</span></span> <span data-ttu-id="ccea7-703">Pokud všichni uživatelé používají v smallrc, 32 souběžných dotazů by být povolena, protože každý dotaz by používat 1 slot souběžnosti.</span><span class="sxs-lookup"><span data-stu-id="ccea7-703">If all users are running in smallrc, 32 concurrent queries would be allowed because each query would consume 1 concurrency slot.</span></span> <span data-ttu-id="ccea7-704">Pokud všechny uživatele DW1000 byly spuštěny v mediumrc, by se každý dotaz přidělit 800 MB za distribuci pro přidělení celkové paměti 47 GB na jeden dotaz a souběžnosti by uživatelé omezené too5 (40 sloty souběžnosti 8 přihrádek na mediumrc uživatele /).</span><span class="sxs-lookup"><span data-stu-id="ccea7-704">If all users on a DW1000 were running in mediumrc, each query would be allocated 800 MB per distribution for a total memory allocation of 47 GB per query, and concurrency would be limited too5 users (40 concurrency slots / 8 slots per mediumrc user).</span></span>

## <a name="selecting-proper-resource-class"></a><span data-ttu-id="ccea7-705">Výběr třídy odpovídající prostředek</span><span class="sxs-lookup"><span data-stu-id="ccea7-705">Selecting proper resource class</span></span>  
<span data-ttu-id="ccea7-706">Doporučeným postupem je třída prostředků toopermanently přiřazovat uživatele tooa nemusíte měnit jejich třídy prostředků.</span><span class="sxs-lookup"><span data-stu-id="ccea7-706">A good practice is toopermanently assign users tooa resource class rather than changing their resource classes.</span></span> <span data-ttu-id="ccea7-707">Můžete například vytvořit tabulky columnstore tooclustered zatížení kvalitnější indexy při přidělení více paměti.</span><span class="sxs-lookup"><span data-stu-id="ccea7-707">For example, loads tooclustered columnstore tables create higher-quality indexes when allocated more memory.</span></span> <span data-ttu-id="ccea7-708">tooensure, objemy paměti toohigher přístup, vytvořte uživatele speciálně pro načítání dat a trvale přiřadit tento uživatel tooa vyšší Třída prostředků.</span><span class="sxs-lookup"><span data-stu-id="ccea7-708">tooensure that loads have access toohigher memory, create a user specifically for loading data and permanently assign this user tooa higher resource class.</span></span>
<span data-ttu-id="ccea7-709">Existuje několik osvědčených postupů toofollow sem.</span><span class="sxs-lookup"><span data-stu-id="ccea7-709">There are a couple of best practices toofollow here.</span></span> <span data-ttu-id="ccea7-710">Jak je uvedeno nahoře, SQL DW podporuje dva druhy třída typy prostředků: prostředků statické třídy a třídy dynamického prostředku.</span><span class="sxs-lookup"><span data-stu-id="ccea7-710">As mentioned above, SQL DW supports two kinds of resource class types: static resource classes and dynamic resource classes.</span></span>
### <a name="loading-best-practices"></a><span data-ttu-id="ccea7-711">Načítání osvědčené postupy</span><span class="sxs-lookup"><span data-stu-id="ccea7-711">Loading best practices</span></span>
1.  <span data-ttu-id="ccea7-712">Pokud hello očekávání zatížení s regulární množství dat, třída statické prostředků je vhodné použít.</span><span class="sxs-lookup"><span data-stu-id="ccea7-712">If hello expectations are loads with regular amount of data, a static resource class is a good choice.</span></span> <span data-ttu-id="ccea7-713">Později při rozšiřování prostředků tooget další výpočetní výkon, hello datového skladu bude mít toorun více souběžných dotazuje out-of-the-box, jako uživatel zatížení hello nespotřebovává více paměti.</span><span class="sxs-lookup"><span data-stu-id="ccea7-713">Later, when scaling up tooget more computational power, hello data warehouse will be able toorun more concurrent queries out-of-the-box, as hello load user does not consume more memory.</span></span>
2.  <span data-ttu-id="ccea7-714">Pokud hello očekávání větší zatížení v některých případech, třída dynamické prostředků je vhodné použít.</span><span class="sxs-lookup"><span data-stu-id="ccea7-714">If hello expectations are bigger loads in some occasions, a dynamic resource class is a good choice.</span></span> <span data-ttu-id="ccea7-715">Později, při rozšiřování prostředků tooget další výpočetní výkon, hello zatížení uživatel dostane další paměť out-of-the-box, proto hello zatížení tooperform umožňuje rychlejší.</span><span class="sxs-lookup"><span data-stu-id="ccea7-715">Later, when scaling up tooget more computational power, hello load user will get more memory out-of-the-box, hence allowing hello load tooperform faster.</span></span>

<span data-ttu-id="ccea7-716">Hello objemy paměti vyžadované tooprocess efektivně závisí na povaze hello hello tabulka načíst a hello objemu zpracovaných dat..</span><span class="sxs-lookup"><span data-stu-id="ccea7-716">hello memory needed tooprocess loads efficiently depends on hello nature of hello table loaded and hello amount of data processed.</span></span> <span data-ttu-id="ccea7-717">Načítání dat do tabulek KÚS vyžaduje například, že některé paměti toolet KÚS rowgroups dosáhnout optimality.</span><span class="sxs-lookup"><span data-stu-id="ccea7-717">For instance, loading data into CCI tables requires some memory toolet CCI rowgroups reach optimality.</span></span> <span data-ttu-id="ccea7-718">Další podrobnosti najdete v tématu indexy Columnstore hello - data načítání pokyny.</span><span class="sxs-lookup"><span data-stu-id="ccea7-718">For more details, see hello Columnstore indexes - data loading guidance.</span></span>

<span data-ttu-id="ccea7-719">Jako osvědčený postup doporučujeme vám toouse alespoň 200MB paměti pro zatížení.</span><span class="sxs-lookup"><span data-stu-id="ccea7-719">As a best practice, we advise you toouse at least 200MB of memory for loads.</span></span>

### <a name="querying-best-practices"></a><span data-ttu-id="ccea7-720">Dotazování osvědčené postupy</span><span class="sxs-lookup"><span data-stu-id="ccea7-720">Querying best practices</span></span>
<span data-ttu-id="ccea7-721">Dotazy mají různé požadavky v závislosti na jejich složitost.</span><span class="sxs-lookup"><span data-stu-id="ccea7-721">Queries have different requirements depending on their complexity.</span></span> <span data-ttu-id="ccea7-722">Zvýšení paměti na jeden dotaz nebo zvýšení hello souběžnosti jsou obě platné způsoby tooaugment celkovou propustnost v závislosti na potřebách hello dotazu.</span><span class="sxs-lookup"><span data-stu-id="ccea7-722">Increasing memory per query or increasing hello concurrency are both valid ways tooaugment overall throughput depending on hello query needs.</span></span>
1.  <span data-ttu-id="ccea7-723">Pokud hello očekávání regulární a složité dotazy (například toogenerate denní nebo týdenní sestavy) a není nutné tootake výhod souběžnosti, třída dynamické prostředků je vhodné použít.</span><span class="sxs-lookup"><span data-stu-id="ccea7-723">If hello expectations are regular, complex queries (for instance, toogenerate daily and weekly reports) and do not need tootake advantage of concurrency, a dynamic resource class is a good choice.</span></span> <span data-ttu-id="ccea7-724">Pokud má systém hello tooprocess další data, vertikálním navýšení kapacity hello datového skladu se proto automaticky pro další paměť toohello uživatele spuštění dotazu hello.</span><span class="sxs-lookup"><span data-stu-id="ccea7-724">If hello system has more data tooprocess, scaling up hello data warehouse will therefore automatically provide more memory toohello user running hello query.</span></span>
2.  <span data-ttu-id="ccea7-725">Pokud hello očekávání vzory proměnnou nebo křivka souběžnosti (pro instanci Pokud hello databáze je dotazován prostřednictvím webového uživatelského rozhraní široce dostupné), třída statické prostředků je vhodné použít.</span><span class="sxs-lookup"><span data-stu-id="ccea7-725">If hello expectations are variable or diurnal concurrency patterns (for instance if hello database is queried through a web UI broadly accessible), a static resource class is a good choice.</span></span> <span data-ttu-id="ccea7-726">Později, při rozšiřování prostředků toodata skladu, hello uživatel přidružený ke třídě hello statické prostředků bude automaticky se možné toorun více souběžných dotazů.</span><span class="sxs-lookup"><span data-stu-id="ccea7-726">Later, when scaling up toodata warehouse, hello user associated with hello static resource class will automatically be able toorun more concurrent queries.</span></span>

<span data-ttu-id="ccea7-727">Výběr správné paměti grant podle potřeby hello tohoto dotazu je netriviální, protože závisí na mnoha faktorech, například hello množství dat získaných, hello povahu hello schémata tabulek a různé spojení, výběru a predikáty skupiny.</span><span class="sxs-lookup"><span data-stu-id="ccea7-727">Selecting proper memory grant depending on hello need of your query is non-trivial, as it depends on many factors, such as hello amount of data queried, hello nature of hello table schemas, and various join, selection, and group predicates.</span></span> <span data-ttu-id="ccea7-728">Z hlediska obecné přidělení více paměti umožní dotazy toocomplete rychlejší, ale by snížení hello celkové souběžnosti.</span><span class="sxs-lookup"><span data-stu-id="ccea7-728">From a general standpoint, allocating more memory will allow queries toocomplete faster, but would reduce hello overall concurrency.</span></span> <span data-ttu-id="ccea7-729">Pokud souběžnosti není problém, přepsání přidělování paměti není poškodit.</span><span class="sxs-lookup"><span data-stu-id="ccea7-729">If concurrency is not an issue, over-allocating memory does not harm.</span></span> <span data-ttu-id="ccea7-730">toofine tune propustnost, při různých typů prostředků třídy mohou být vyžadovány.</span><span class="sxs-lookup"><span data-stu-id="ccea7-730">toofine-tune throughput, trying various flavors of resource classes may be required.</span></span>

<span data-ttu-id="ccea7-731">Můžete použít následující hello uložené procedury toofigure out souběžnosti a paměť grant za Třída prostředků na daný objekt SLO a hello nejbližší nejlepší prostředků třídu pro operace náročné na prostředky KÚS paměti na bez oddílů tabulky KÚS na třídy daného prostředku:</span><span class="sxs-lookup"><span data-stu-id="ccea7-731">You can use hello following stored procedure toofigure out concurrency and memory grant per resource class at a given SLO and hello closest best resource class for memory intensive CCI operations on non-partitioned CCI table at a given resource class:</span></span>

#### <a name="description"></a><span data-ttu-id="ccea7-732">Popis:</span><span class="sxs-lookup"><span data-stu-id="ccea7-732">Description:</span></span>  
<span data-ttu-id="ccea7-733">Tady je hello účel tuto uloženou proceduru:</span><span class="sxs-lookup"><span data-stu-id="ccea7-733">Here's hello purpose of this stored procedure:</span></span>  
1. <span data-ttu-id="ccea7-734">Obrázek uživatele toohelp out souběžnosti a paměť grant za Třída prostředků v daném objektu SLO.</span><span class="sxs-lookup"><span data-stu-id="ccea7-734">toohelp user figure out concurrency and memory grant per resource class at a given SLO.</span></span> <span data-ttu-id="ccea7-735">Uživatel potřebuje tooprovide hodnotu NULL pro schématu a název tabulky pro tento jak je znázorněno v následujícím příkladu hello.</span><span class="sxs-lookup"><span data-stu-id="ccea7-735">User needs tooprovide NULL for both schema and tablename for this as shown in hello example below.</span></span>  
2. <span data-ttu-id="ccea7-736">toohelp uživatele zjistěte hello nejbližší nejlepší Třída prostředků pro hello paměti intensed KÚS operací (zatížení, kopie tabulku znovu sestavit index, atd.) na jiný dělenou tabulku KÚS na třídu daného prostředku.</span><span class="sxs-lookup"><span data-stu-id="ccea7-736">toohelp user figure out hello closest best resource class for hello memory intensed CCI operations (load, copy table, rebuild index, etc.) on non partitioned CCI table at a given resource class.</span></span> <span data-ttu-id="ccea7-737">Hello uložená procedura používá pro tento toofind schématu tabulky se grant hello požadovanou paměť.</span><span class="sxs-lookup"><span data-stu-id="ccea7-737">hello stored proc uses table schema toofind out hello required memory grant for this.</span></span>

#### <a name="dependencies--restrictions"></a><span data-ttu-id="ccea7-738">Závislosti & omezení:</span><span class="sxs-lookup"><span data-stu-id="ccea7-738">Dependencies & Restrictions:</span></span>
- <span data-ttu-id="ccea7-739">Tato uložená procedura není požadavek na paměť navrženou toocalculate rozdělena na oddíly KÚS tabulky.</span><span class="sxs-lookup"><span data-stu-id="ccea7-739">This stored proc is not designed toocalculate memory requirement for partitioned-cci table.</span></span>    
- <span data-ttu-id="ccea7-740">Tato uložená procedura neberou požadavek na paměť v úvahu hello vyberte součást funkce CTAS/INSERT-výběr a předpokládá se toobe jednoduché vyberte.</span><span class="sxs-lookup"><span data-stu-id="ccea7-740">This stored proc doesn't take memory requirement into account for hello SELECT part of CTAS/INSERT-SELECT and assumes it toobe a simple SELECT.</span></span>
- <span data-ttu-id="ccea7-741">Tato uložená procedura používá dočasnou tabulku, takže toto lze použít v relaci hello kde byl vytvořen tento uložené procedury.</span><span class="sxs-lookup"><span data-stu-id="ccea7-741">This stored proc uses a temp table so this can be used in hello session where this stored proc was created.</span></span>    
- <span data-ttu-id="ccea7-742">Tato uložená procedura závisí na aktuální nabídky hello (např. konfiguraci hardwaru, konfigurace DMS) a pokud se některé této změny pak tato uložená procedura nebude fungovat správně.</span><span class="sxs-lookup"><span data-stu-id="ccea7-742">This stored proc depends on hello current offerings (e.g. hardware configuration, DMS config) and if any of that changes then this stored proc would not work correctly.</span></span>  
- <span data-ttu-id="ccea7-743">Tato uložená procedura závisí na existující nabízený souběžnosti limit a pokud se změní, pak tato uložená procedura nebude fungovat správně.</span><span class="sxs-lookup"><span data-stu-id="ccea7-743">This stored proc depends on existing offered concurrency limit and if that changes then this stored proc would not work correctly.</span></span>  
- <span data-ttu-id="ccea7-744">Tato uložená procedura závisí na stávající nabídky Třída prostředků a který změní-li se pak to uložené procedura zamýšlenému není pracovní správně.</span><span class="sxs-lookup"><span data-stu-id="ccea7-744">This stored proc depends on existing resource class offerings and if that changes then this stored proc wuold not work correctly.</span></span>  

>  [!NOTE]  
>  <span data-ttu-id="ccea7-745">Pokud se nezobrazují výstupu po spuštění uložené procedury s parametry zadané, pak může být dvěma případy.</span><span class="sxs-lookup"><span data-stu-id="ccea7-745">If you are not getting output after executing stored procedure with parameters provided then there could be two cases.</span></span> <br /><span data-ttu-id="ccea7-746">1. Buď parametr datového skladu obsahuje neplatnou hodnotu SLO</span><span class="sxs-lookup"><span data-stu-id="ccea7-746">1. Either DW Parameter contains invalid SLO value</span></span> <br /><span data-ttu-id="ccea7-747">2. NEBO pokud byl poskytnut název tabulky neexistují žádné odpovídající Třída prostředků pro operaci KÚS.</span><span class="sxs-lookup"><span data-stu-id="ccea7-747">2. OR there are no matching resource class for CCI operation if table name was provided.</span></span> <br /><span data-ttu-id="ccea7-748">Například na od DW100, nejvyšší přidělení paměti k dispozici je 400MB a pokud je schéma tabulky široké dostatek toocross hello požadavek 400MB.</span><span class="sxs-lookup"><span data-stu-id="ccea7-748">For example, at DW100, highest memory grant available is 400MB and if table schema is wide enough toocross hello requirement of 400MB.</span></span>
      
#### <a name="usage-example"></a><span data-ttu-id="ccea7-749">Příklad použití:</span><span class="sxs-lookup"><span data-stu-id="ccea7-749">Usage example:</span></span>
<span data-ttu-id="ccea7-750">Syntaxe:</span><span class="sxs-lookup"><span data-stu-id="ccea7-750">Syntax:</span></span>  
`EXEC dbo.prc_workload_management_by_DWU @DWU VARCHAR(7), @SCHEMA_NAME VARCHAR(128), @TABLE_NAME VARCHAR(128)`  
1. <span data-ttu-id="ccea7-751">@DWU:Buď zadejte parametr tooextract hodnotu NULL hello aktuální DWU z hello databázi datového skladu nebo zadejte jakýkoli podporovaný DWU hello tvar 'od DW100.</span><span class="sxs-lookup"><span data-stu-id="ccea7-751">@DWU: Either provide a NULL parameter tooextract hello current DWU from hello DW DB or provide any supported DWU in hello form of 'DW100'</span></span>
2. <span data-ttu-id="ccea7-752">@SCHEMA_NAME:Zadejte název schématu tabulky hello</span><span class="sxs-lookup"><span data-stu-id="ccea7-752">@SCHEMA_NAME: Provide a schema name of hello table</span></span>
3. <span data-ttu-id="ccea7-753">@TABLE_NAME:Zadejte název tabulky zájmu hello</span><span class="sxs-lookup"><span data-stu-id="ccea7-753">@TABLE_NAME: Provide a table name of hello interest</span></span>

<span data-ttu-id="ccea7-754">Příklady provádění této uložené procedury:</span><span class="sxs-lookup"><span data-stu-id="ccea7-754">Examples executing this stored proc:</span></span>  
```sql  
EXEC dbo.prc_workload_management_by_DWU 'DW2000', 'dbo', 'Table1';  
EXEC dbo.prc_workload_management_by_DWU NULL, 'dbo', 'Table1';  
EXEC dbo.prc_workload_management_by_DWU 'DW6000', NULL, NULL;  
EXEC dbo.prc_workload_management_by_DWU NULL, NULL, NULL;  
```

<span data-ttu-id="ccea7-755">By bylo možné vytvořit tabulky1 použít v hello výše příklady, jak je uvedeno níže:</span><span class="sxs-lookup"><span data-stu-id="ccea7-755">Table1 used in hello above examples could be created as below:</span></span>  
`CREATE TABLE Table1 (a int, b varchar(50), c decimal (18,10), d char(10), e varbinary(15), f float, g datetime, h date);`

#### <a name="heres-hello-stored-procedure-definition"></a><span data-ttu-id="ccea7-756">Zde je definice hello uložené procedury:</span><span class="sxs-lookup"><span data-stu-id="ccea7-756">Here's hello stored procedure definition:</span></span>
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
-- Selecting proper DWU for hello current DB if not specified.
SET @DWU = (
  SELECT 'DW'+CAST(COUNT(*)*100 AS VARCHAR(10))
  FROM sys.dm_pdw_nodes
  WHERE type = 'COMPUTE')
END

DECLARE @DWU_NUM INT
SET @DWU_NUM = CAST (SUBSTRING(@DWU, 3, LEN(@DWU)-2) AS INT)

-- Raise error if either schema name or table name is supplied but not both them supplied
--IF ((@SCHEMA_NAME IS NOT NULL AND @TABLE_NAME IS NULL) OR (@TABLE_NAME IS NULL AND @SCHEMA_NAME IS NOT NULL))
--     RAISEERROR('User need toosupply either both Schema Name and Table Name or none of them')
       
-- Dropping temp table if exists.
IF OBJECT_ID('tempdb..#ref') IS NOT NULL
BEGIN
  DROP TABLE #ref;
END

-- Creating ref. temptable (CTAS) toohold mapping info.
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
-- Creating workload mapping tootheir corresponding slot consumption and default memory grant.
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

## <a name="query-importance"></a><span data-ttu-id="ccea7-757">Význam dotazu</span><span class="sxs-lookup"><span data-stu-id="ccea7-757">Query importance</span></span>
<span data-ttu-id="ccea7-758">SQL Data Warehouse implementuje třídy prostředků pomocí skupiny úloh.</span><span class="sxs-lookup"><span data-stu-id="ccea7-758">SQL Data Warehouse implements resource classes by using workload groups.</span></span> <span data-ttu-id="ccea7-759">Existují celkem osm zatížení skupin, které řídí chování hello třídy prostředků hello napříč hello různých velikostí DWU.</span><span class="sxs-lookup"><span data-stu-id="ccea7-759">There are a total of eight workload groups that control hello behavior of hello resource classes across hello various DWU sizes.</span></span> <span data-ttu-id="ccea7-760">Pro všechny DWU SQL Data Warehouse používá jenom čtyři hello osm skupiny úloh.</span><span class="sxs-lookup"><span data-stu-id="ccea7-760">For any DWU, SQL Data Warehouse uses only four of hello eight workload groups.</span></span> <span data-ttu-id="ccea7-761">To dává smysl, protože každé skupiny úlohy je přiřazen tooone čtyři třídy prostředků: smallrc, mediumrc, largerc, nebo xlargerc.</span><span class="sxs-lookup"><span data-stu-id="ccea7-761">This makes sense because each workload group is assigned tooone of four resource classes: smallrc, mediumrc, largerc, or xlargerc.</span></span> <span data-ttu-id="ccea7-762">Hello význam pochopení skupiny úloh hello je, že některé z těchto skupin úloh jsou nastaveny toohigher *důležitosti*.</span><span class="sxs-lookup"><span data-stu-id="ccea7-762">hello importance of understanding hello workload groups is that some of these workload groups are set toohigher *importance*.</span></span> <span data-ttu-id="ccea7-763">Důležitost se používá pro procesoru plánování.</span><span class="sxs-lookup"><span data-stu-id="ccea7-763">Importance is used for CPU scheduling.</span></span> <span data-ttu-id="ccea7-764">Dotazy spustit s vysokou důležitostí získají třikrát více cyklů procesoru než ty, které se střední důležitostí.</span><span class="sxs-lookup"><span data-stu-id="ccea7-764">Queries run with high importance will get three times more CPU cycles than those with medium importance.</span></span> <span data-ttu-id="ccea7-765">Proto souběžnosti slotu mapování taky určit Priorita procesoru.</span><span class="sxs-lookup"><span data-stu-id="ccea7-765">Therefore, concurrency slot mappings also determine CPU priority.</span></span> <span data-ttu-id="ccea7-766">Pokud je dotaz spotřebovává 16 nebo víc sloty, běží jako vysokou důležitostí.</span><span class="sxs-lookup"><span data-stu-id="ccea7-766">When a query consumes 16 or more slots, it runs as high importance.</span></span>

<span data-ttu-id="ccea7-767">Hello následující tabulka uvádí hello důležitosti mapování pro každou skupinu úloh.</span><span class="sxs-lookup"><span data-stu-id="ccea7-767">hello following table shows hello importance mappings for each workload group.</span></span>

### <a name="workload-group-mappings-tooconcurrency-slots-and-importance"></a><span data-ttu-id="ccea7-768">Zatížení skupiny mapování tooconcurrency sloty a důležitostí</span><span class="sxs-lookup"><span data-stu-id="ccea7-768">Workload group mappings tooconcurrency slots and importance</span></span>
| <span data-ttu-id="ccea7-769">Skupiny úloh</span><span class="sxs-lookup"><span data-stu-id="ccea7-769">Workload groups</span></span> | <span data-ttu-id="ccea7-770">Mapování slotu souběžnosti</span><span class="sxs-lookup"><span data-stu-id="ccea7-770">Concurrency slot mapping</span></span> | <span data-ttu-id="ccea7-771">MB / distribuce</span><span class="sxs-lookup"><span data-stu-id="ccea7-771">MB / Distribution</span></span> | <span data-ttu-id="ccea7-772">Mapování důležitostí</span><span class="sxs-lookup"><span data-stu-id="ccea7-772">Importance mapping</span></span> |
|:--- |:---:|:---:|:--- |
| <span data-ttu-id="ccea7-773">SloDWGroupC00</span><span class="sxs-lookup"><span data-stu-id="ccea7-773">SloDWGroupC00</span></span> |<span data-ttu-id="ccea7-774">1</span><span class="sxs-lookup"><span data-stu-id="ccea7-774">1</span></span> |<span data-ttu-id="ccea7-775">100</span><span class="sxs-lookup"><span data-stu-id="ccea7-775">100</span></span> |<span data-ttu-id="ccea7-776">Střednědobé používání</span><span class="sxs-lookup"><span data-stu-id="ccea7-776">Medium</span></span> |
| <span data-ttu-id="ccea7-777">SloDWGroupC01</span><span class="sxs-lookup"><span data-stu-id="ccea7-777">SloDWGroupC01</span></span> |<span data-ttu-id="ccea7-778">2</span><span class="sxs-lookup"><span data-stu-id="ccea7-778">2</span></span> |<span data-ttu-id="ccea7-779">200</span><span class="sxs-lookup"><span data-stu-id="ccea7-779">200</span></span> |<span data-ttu-id="ccea7-780">Střednědobé používání</span><span class="sxs-lookup"><span data-stu-id="ccea7-780">Medium</span></span> |
| <span data-ttu-id="ccea7-781">SloDWGroupC02</span><span class="sxs-lookup"><span data-stu-id="ccea7-781">SloDWGroupC02</span></span> |<span data-ttu-id="ccea7-782">4</span><span class="sxs-lookup"><span data-stu-id="ccea7-782">4</span></span> |<span data-ttu-id="ccea7-783">400</span><span class="sxs-lookup"><span data-stu-id="ccea7-783">400</span></span> |<span data-ttu-id="ccea7-784">Střednědobé používání</span><span class="sxs-lookup"><span data-stu-id="ccea7-784">Medium</span></span> |
| <span data-ttu-id="ccea7-785">SloDWGroupC03</span><span class="sxs-lookup"><span data-stu-id="ccea7-785">SloDWGroupC03</span></span> |<span data-ttu-id="ccea7-786">8</span><span class="sxs-lookup"><span data-stu-id="ccea7-786">8</span></span> |<span data-ttu-id="ccea7-787">800</span><span class="sxs-lookup"><span data-stu-id="ccea7-787">800</span></span> |<span data-ttu-id="ccea7-788">Střednědobé používání</span><span class="sxs-lookup"><span data-stu-id="ccea7-788">Medium</span></span> |
| <span data-ttu-id="ccea7-789">SloDWGroupC04</span><span class="sxs-lookup"><span data-stu-id="ccea7-789">SloDWGroupC04</span></span> |<span data-ttu-id="ccea7-790">16</span><span class="sxs-lookup"><span data-stu-id="ccea7-790">16</span></span> |<span data-ttu-id="ccea7-791">1,600</span><span class="sxs-lookup"><span data-stu-id="ccea7-791">1,600</span></span> |<span data-ttu-id="ccea7-792">Vysoký</span><span class="sxs-lookup"><span data-stu-id="ccea7-792">High</span></span> |
| <span data-ttu-id="ccea7-793">SloDWGroupC05</span><span class="sxs-lookup"><span data-stu-id="ccea7-793">SloDWGroupC05</span></span> |<span data-ttu-id="ccea7-794">32</span><span class="sxs-lookup"><span data-stu-id="ccea7-794">32</span></span> |<span data-ttu-id="ccea7-795">3,200</span><span class="sxs-lookup"><span data-stu-id="ccea7-795">3,200</span></span> |<span data-ttu-id="ccea7-796">Vysoký</span><span class="sxs-lookup"><span data-stu-id="ccea7-796">High</span></span> |
| <span data-ttu-id="ccea7-797">SloDWGroupC06</span><span class="sxs-lookup"><span data-stu-id="ccea7-797">SloDWGroupC06</span></span> |<span data-ttu-id="ccea7-798">64</span><span class="sxs-lookup"><span data-stu-id="ccea7-798">64</span></span> |<span data-ttu-id="ccea7-799">6,400</span><span class="sxs-lookup"><span data-stu-id="ccea7-799">6,400</span></span> |<span data-ttu-id="ccea7-800">Vysoký</span><span class="sxs-lookup"><span data-stu-id="ccea7-800">High</span></span> |
| <span data-ttu-id="ccea7-801">SloDWGroupC07</span><span class="sxs-lookup"><span data-stu-id="ccea7-801">SloDWGroupC07</span></span> |<span data-ttu-id="ccea7-802">128</span><span class="sxs-lookup"><span data-stu-id="ccea7-802">128</span></span> |<span data-ttu-id="ccea7-803">12,800</span><span class="sxs-lookup"><span data-stu-id="ccea7-803">12,800</span></span> |<span data-ttu-id="ccea7-804">Vysoký</span><span class="sxs-lookup"><span data-stu-id="ccea7-804">High</span></span> |

<span data-ttu-id="ccea7-805">Z hello **přidělení a využití souběžnosti přihrádky** grafu, zjistíte, že DW500 používá 1, 4, 8 nebo 16 souběžnosti sloty pro smallrc, mediumrc, largerc a xlargerc, v uvedeném pořadí.</span><span class="sxs-lookup"><span data-stu-id="ccea7-805">From hello **Allocation and consumption of concurrency slots** chart, you can see that a DW500 uses 1, 4, 8 or 16 concurrency slots for smallrc, mediumrc, largerc, and xlargerc, respectively.</span></span> <span data-ttu-id="ccea7-806">Tyto hodnoty lze vyhledat v hello předcházející důležitosti hello toofind grafu pro každou třídu prostředků.</span><span class="sxs-lookup"><span data-stu-id="ccea7-806">You can look those values up in hello preceding chart toofind hello importance for each resource class.</span></span>

### <a name="dw500-mapping-of-resource-classes-tooimportance"></a><span data-ttu-id="ccea7-807">Mapování DW500 tooimportance třídy prostředků</span><span class="sxs-lookup"><span data-stu-id="ccea7-807">DW500 mapping of resource classes tooimportance</span></span>
| <span data-ttu-id="ccea7-808">Třída prostředků</span><span class="sxs-lookup"><span data-stu-id="ccea7-808">Resource class</span></span> | <span data-ttu-id="ccea7-809">Skupina úlohy</span><span class="sxs-lookup"><span data-stu-id="ccea7-809">Workload group</span></span> | <span data-ttu-id="ccea7-810">Použít sloty souběžnosti</span><span class="sxs-lookup"><span data-stu-id="ccea7-810">Concurrency slots used</span></span> | <span data-ttu-id="ccea7-811">MB / distribuce</span><span class="sxs-lookup"><span data-stu-id="ccea7-811">MB / Distribution</span></span> | <span data-ttu-id="ccea7-812">Význam</span><span class="sxs-lookup"><span data-stu-id="ccea7-812">Importance</span></span> |
|:--- |:--- |:---:|:---:|:--- |
| <span data-ttu-id="ccea7-813">smallrc</span><span class="sxs-lookup"><span data-stu-id="ccea7-813">smallrc</span></span> |<span data-ttu-id="ccea7-814">SloDWGroupC00</span><span class="sxs-lookup"><span data-stu-id="ccea7-814">SloDWGroupC00</span></span> |<span data-ttu-id="ccea7-815">1</span><span class="sxs-lookup"><span data-stu-id="ccea7-815">1</span></span> |<span data-ttu-id="ccea7-816">100</span><span class="sxs-lookup"><span data-stu-id="ccea7-816">100</span></span> |<span data-ttu-id="ccea7-817">Střednědobé používání</span><span class="sxs-lookup"><span data-stu-id="ccea7-817">Medium</span></span> |
| <span data-ttu-id="ccea7-818">mediumrc</span><span class="sxs-lookup"><span data-stu-id="ccea7-818">mediumrc</span></span> |<span data-ttu-id="ccea7-819">SloDWGroupC02</span><span class="sxs-lookup"><span data-stu-id="ccea7-819">SloDWGroupC02</span></span> |<span data-ttu-id="ccea7-820">4</span><span class="sxs-lookup"><span data-stu-id="ccea7-820">4</span></span> |<span data-ttu-id="ccea7-821">400</span><span class="sxs-lookup"><span data-stu-id="ccea7-821">400</span></span> |<span data-ttu-id="ccea7-822">Střednědobé používání</span><span class="sxs-lookup"><span data-stu-id="ccea7-822">Medium</span></span> |
| <span data-ttu-id="ccea7-823">largerc</span><span class="sxs-lookup"><span data-stu-id="ccea7-823">largerc</span></span> |<span data-ttu-id="ccea7-824">SloDWGroupC03</span><span class="sxs-lookup"><span data-stu-id="ccea7-824">SloDWGroupC03</span></span> |<span data-ttu-id="ccea7-825">8</span><span class="sxs-lookup"><span data-stu-id="ccea7-825">8</span></span> |<span data-ttu-id="ccea7-826">800</span><span class="sxs-lookup"><span data-stu-id="ccea7-826">800</span></span> |<span data-ttu-id="ccea7-827">Střednědobé používání</span><span class="sxs-lookup"><span data-stu-id="ccea7-827">Medium</span></span> |
| <span data-ttu-id="ccea7-828">xlargerc</span><span class="sxs-lookup"><span data-stu-id="ccea7-828">xlargerc</span></span> |<span data-ttu-id="ccea7-829">SloDWGroupC04</span><span class="sxs-lookup"><span data-stu-id="ccea7-829">SloDWGroupC04</span></span> |<span data-ttu-id="ccea7-830">16</span><span class="sxs-lookup"><span data-stu-id="ccea7-830">16</span></span> |<span data-ttu-id="ccea7-831">1,600</span><span class="sxs-lookup"><span data-stu-id="ccea7-831">1,600</span></span> |<span data-ttu-id="ccea7-832">Vysoký</span><span class="sxs-lookup"><span data-stu-id="ccea7-832">High</span></span> |
| <span data-ttu-id="ccea7-833">staticrc10</span><span class="sxs-lookup"><span data-stu-id="ccea7-833">staticrc10</span></span> |<span data-ttu-id="ccea7-834">SloDWGroupC00</span><span class="sxs-lookup"><span data-stu-id="ccea7-834">SloDWGroupC00</span></span> |<span data-ttu-id="ccea7-835">1</span><span class="sxs-lookup"><span data-stu-id="ccea7-835">1</span></span> |<span data-ttu-id="ccea7-836">100</span><span class="sxs-lookup"><span data-stu-id="ccea7-836">100</span></span> |<span data-ttu-id="ccea7-837">Střednědobé používání</span><span class="sxs-lookup"><span data-stu-id="ccea7-837">Medium</span></span> |
| <span data-ttu-id="ccea7-838">staticrc20</span><span class="sxs-lookup"><span data-stu-id="ccea7-838">staticrc20</span></span> |<span data-ttu-id="ccea7-839">SloDWGroupC01</span><span class="sxs-lookup"><span data-stu-id="ccea7-839">SloDWGroupC01</span></span> |<span data-ttu-id="ccea7-840">2</span><span class="sxs-lookup"><span data-stu-id="ccea7-840">2</span></span> |<span data-ttu-id="ccea7-841">200</span><span class="sxs-lookup"><span data-stu-id="ccea7-841">200</span></span> |<span data-ttu-id="ccea7-842">Střednědobé používání</span><span class="sxs-lookup"><span data-stu-id="ccea7-842">Medium</span></span> |
| <span data-ttu-id="ccea7-843">staticrc30</span><span class="sxs-lookup"><span data-stu-id="ccea7-843">staticrc30</span></span> |<span data-ttu-id="ccea7-844">SloDWGroupC02</span><span class="sxs-lookup"><span data-stu-id="ccea7-844">SloDWGroupC02</span></span> |<span data-ttu-id="ccea7-845">4</span><span class="sxs-lookup"><span data-stu-id="ccea7-845">4</span></span> |<span data-ttu-id="ccea7-846">400</span><span class="sxs-lookup"><span data-stu-id="ccea7-846">400</span></span> |<span data-ttu-id="ccea7-847">Střednědobé používání</span><span class="sxs-lookup"><span data-stu-id="ccea7-847">Medium</span></span> |
| <span data-ttu-id="ccea7-848">staticrc40</span><span class="sxs-lookup"><span data-stu-id="ccea7-848">staticrc40</span></span> |<span data-ttu-id="ccea7-849">SloDWGroupC03</span><span class="sxs-lookup"><span data-stu-id="ccea7-849">SloDWGroupC03</span></span> |<span data-ttu-id="ccea7-850">8</span><span class="sxs-lookup"><span data-stu-id="ccea7-850">8</span></span> |<span data-ttu-id="ccea7-851">800</span><span class="sxs-lookup"><span data-stu-id="ccea7-851">800</span></span> |<span data-ttu-id="ccea7-852">Střednědobé používání</span><span class="sxs-lookup"><span data-stu-id="ccea7-852">Medium</span></span> |
| <span data-ttu-id="ccea7-853">staticrc50</span><span class="sxs-lookup"><span data-stu-id="ccea7-853">staticrc50</span></span> |<span data-ttu-id="ccea7-854">SloDWGroupC03</span><span class="sxs-lookup"><span data-stu-id="ccea7-854">SloDWGroupC03</span></span> |<span data-ttu-id="ccea7-855">16</span><span class="sxs-lookup"><span data-stu-id="ccea7-855">16</span></span> |<span data-ttu-id="ccea7-856">1,600</span><span class="sxs-lookup"><span data-stu-id="ccea7-856">1,600</span></span> |<span data-ttu-id="ccea7-857">Vysoký</span><span class="sxs-lookup"><span data-stu-id="ccea7-857">High</span></span> |
| <span data-ttu-id="ccea7-858">staticrc60</span><span class="sxs-lookup"><span data-stu-id="ccea7-858">staticrc60</span></span> |<span data-ttu-id="ccea7-859">SloDWGroupC03</span><span class="sxs-lookup"><span data-stu-id="ccea7-859">SloDWGroupC03</span></span> |<span data-ttu-id="ccea7-860">16</span><span class="sxs-lookup"><span data-stu-id="ccea7-860">16</span></span> |<span data-ttu-id="ccea7-861">1,600</span><span class="sxs-lookup"><span data-stu-id="ccea7-861">1,600</span></span> |<span data-ttu-id="ccea7-862">Vysoký</span><span class="sxs-lookup"><span data-stu-id="ccea7-862">High</span></span> |
| <span data-ttu-id="ccea7-863">staticrc70</span><span class="sxs-lookup"><span data-stu-id="ccea7-863">staticrc70</span></span> |<span data-ttu-id="ccea7-864">SloDWGroupC03</span><span class="sxs-lookup"><span data-stu-id="ccea7-864">SloDWGroupC03</span></span> |<span data-ttu-id="ccea7-865">16</span><span class="sxs-lookup"><span data-stu-id="ccea7-865">16</span></span> |<span data-ttu-id="ccea7-866">1,600</span><span class="sxs-lookup"><span data-stu-id="ccea7-866">1,600</span></span> |<span data-ttu-id="ccea7-867">Vysoký</span><span class="sxs-lookup"><span data-stu-id="ccea7-867">High</span></span> |
| <span data-ttu-id="ccea7-868">staticrc80</span><span class="sxs-lookup"><span data-stu-id="ccea7-868">staticrc80</span></span> |<span data-ttu-id="ccea7-869">SloDWGroupC03</span><span class="sxs-lookup"><span data-stu-id="ccea7-869">SloDWGroupC03</span></span> |<span data-ttu-id="ccea7-870">16</span><span class="sxs-lookup"><span data-stu-id="ccea7-870">16</span></span> |<span data-ttu-id="ccea7-871">1,600</span><span class="sxs-lookup"><span data-stu-id="ccea7-871">1,600</span></span> |<span data-ttu-id="ccea7-872">Vysoký</span><span class="sxs-lookup"><span data-stu-id="ccea7-872">High</span></span> |

<span data-ttu-id="ccea7-873">Můžete použít následující toolook dotazu DMV v hello rozdíly v přidělování prostředků paměti podrobně z perspektivy hello správce zdrojů hello nebo tooanalyze aktivní a historického využití skupiny úloh hello při řešení potíží s hello.</span><span class="sxs-lookup"><span data-stu-id="ccea7-873">You can use hello following DMV query toolook at hello differences in memory resource allocation in detail from hello perspective of hello resource governor, or tooanalyze active and historic usage of hello workload groups when troubleshooting.</span></span>

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

## <a name="queries-that-honor-concurrency-limits"></a><span data-ttu-id="ccea7-874">Dotazy, které respektovat omezení souběžnosti</span><span class="sxs-lookup"><span data-stu-id="ccea7-874">Queries that honor concurrency limits</span></span>
<span data-ttu-id="ccea7-875">Třídy prostředků se řídí většina dotazů.</span><span class="sxs-lookup"><span data-stu-id="ccea7-875">Most queries are governed by resource classes.</span></span> <span data-ttu-id="ccea7-876">Tyto dotazy se musí vejít do hello souběžných dotazů a prahové hodnoty slotu souběžnosti.</span><span class="sxs-lookup"><span data-stu-id="ccea7-876">These queries must fit inside both hello concurrent query and concurrency slot thresholds.</span></span> <span data-ttu-id="ccea7-877">Uživatel, nelze zvolit tooexclude dotaz z modelu slotu hello souběžnosti.</span><span class="sxs-lookup"><span data-stu-id="ccea7-877">A user cannot choose tooexclude a query from hello concurrency slot model.</span></span>

<span data-ttu-id="ccea7-878">tooreiterate, hello následující příkazy dodržet prostředků třídy:</span><span class="sxs-lookup"><span data-stu-id="ccea7-878">tooreiterate, hello following statements honor resource classes:</span></span>

* <span data-ttu-id="ccea7-879">PŘÍKAZ INSERT SELECT</span><span class="sxs-lookup"><span data-stu-id="ccea7-879">INSERT-SELECT</span></span>
* <span data-ttu-id="ccea7-880">AKTUALIZACE</span><span class="sxs-lookup"><span data-stu-id="ccea7-880">UPDATE</span></span>
* <span data-ttu-id="ccea7-881">ODSTRANIT</span><span class="sxs-lookup"><span data-stu-id="ccea7-881">DELETE</span></span>
* <span data-ttu-id="ccea7-882">Vyberte (při dotazování tabulky uživatelů)</span><span class="sxs-lookup"><span data-stu-id="ccea7-882">SELECT (when querying user tables)</span></span>
* <span data-ttu-id="ccea7-883">PŘÍKAZ ALTER INDEX OPĚTOVNÉ SESTAVENÍ</span><span class="sxs-lookup"><span data-stu-id="ccea7-883">ALTER INDEX REBUILD</span></span>
* <span data-ttu-id="ccea7-884">PŘÍKAZ ALTER INDEX REORGANIZE</span><span class="sxs-lookup"><span data-stu-id="ccea7-884">ALTER INDEX REORGANIZE</span></span>
* <span data-ttu-id="ccea7-885">PŘÍKAZ ALTER TABLE OPĚTOVNÉ SESTAVENÍ</span><span class="sxs-lookup"><span data-stu-id="ccea7-885">ALTER TABLE REBUILD</span></span>
* <span data-ttu-id="ccea7-886">VYTVOŘENÍ INDEXU</span><span class="sxs-lookup"><span data-stu-id="ccea7-886">CREATE INDEX</span></span>
* <span data-ttu-id="ccea7-887">VYTVOŘIT CLUSTEROVANÝ INDEX COLUMNSTORE</span><span class="sxs-lookup"><span data-stu-id="ccea7-887">CREATE CLUSTERED COLUMNSTORE INDEX</span></span>
* <span data-ttu-id="ccea7-888">VYTVOŘENÍ TABLE AS SELECT (FUNKCE CTAS)</span><span class="sxs-lookup"><span data-stu-id="ccea7-888">CREATE TABLE AS SELECT (CTAS)</span></span>
* <span data-ttu-id="ccea7-889">Načítání dat</span><span class="sxs-lookup"><span data-stu-id="ccea7-889">Data loading</span></span>
* <span data-ttu-id="ccea7-890">Operace přesunu dat prováděné hello služby přesun dat (DMS)</span><span class="sxs-lookup"><span data-stu-id="ccea7-890">Data movement operations conducted by hello Data Movement Service (DMS)</span></span>

## <a name="query-exceptions-tooconcurrency-limits"></a><span data-ttu-id="ccea7-891">Omezení tooconcurrency výjimky dotazu</span><span class="sxs-lookup"><span data-stu-id="ccea7-891">Query exceptions tooconcurrency limits</span></span>
<span data-ttu-id="ccea7-892">Některé dotazy nerespektují hello prostředků třída toowhich hello uživatel je přiřazen.</span><span class="sxs-lookup"><span data-stu-id="ccea7-892">Some queries do not honor hello resource class toowhich hello user is assigned.</span></span> <span data-ttu-id="ccea7-893">Tyto limity souběžnosti toohello výjimky jsou vytvářeny po hello paměťové prostředky potřebné pro konkrétní příkaz nízká, často vzhledem k tomu, že příkaz hello je operace metadat.</span><span class="sxs-lookup"><span data-stu-id="ccea7-893">These exceptions toohello concurrency limits are made when hello memory resources needed for a particular command are low, often because hello command is a metadata operation.</span></span> <span data-ttu-id="ccea7-894">cílem Hello tyto výjimky je tooavoid větší přidělení paměti pro dotazy, které je nikdy nebudete potřebovat.</span><span class="sxs-lookup"><span data-stu-id="ccea7-894">hello goal of these exceptions is tooavoid larger memory allocations for queries that will never need them.</span></span> <span data-ttu-id="ccea7-895">V těchto případech přiřadit výchozí hello třída malé prostředků (smallrc) je vždycky použijí bez ohledu na Třída prostředků se skutečné hello toohello uživatele.</span><span class="sxs-lookup"><span data-stu-id="ccea7-895">In these cases, hello default small resource class (smallrc) is always used regardless of hello actual resource class assigned toohello user.</span></span> <span data-ttu-id="ccea7-896">Například `CREATE LOGIN` bude vždy spuštěn v smallrc.</span><span class="sxs-lookup"><span data-stu-id="ccea7-896">For example, `CREATE LOGIN` will always run in smallrc.</span></span> <span data-ttu-id="ccea7-897">Hello prostředků, které vyžaduje toofulfill této operace jsou velmi nízkou, takže neprovede smysl tooinclude hello dotazu v modelu slotu souběžnosti hello.</span><span class="sxs-lookup"><span data-stu-id="ccea7-897">hello resources required toofulfill this operation are very low, so it does not make sense tooinclude hello query in hello concurrency slot model.</span></span>  <span data-ttu-id="ccea7-898">Tyto dotazy nejsou také omezena hello 32 uživatele souběžnosti limit, neomezený počet tyto dotazy spustit až toohello omezení relace 1 024 relací.</span><span class="sxs-lookup"><span data-stu-id="ccea7-898">These queries are also not limited by hello 32 user concurrency limit, an unlimited number of these queries can run up toohello session limit of 1,024 sessions.</span></span>

<span data-ttu-id="ccea7-899">Následující příkazy Hello nerespektují třídy prostředků:</span><span class="sxs-lookup"><span data-stu-id="ccea7-899">hello following statements do not honor resource classes:</span></span>

* <span data-ttu-id="ccea7-900">Vytvořit nebo VYŘADIT tabulku</span><span class="sxs-lookup"><span data-stu-id="ccea7-900">CREATE or DROP TABLE</span></span>
* <span data-ttu-id="ccea7-901">PŘÍKAZ ALTER TABLE... PŘEPÍNAČE, rozdělení nebo sloučit oddíl</span><span class="sxs-lookup"><span data-stu-id="ccea7-901">ALTER TABLE ... SWITCH, SPLIT, or MERGE PARTITION</span></span>
* <span data-ttu-id="ccea7-902">PŘÍKAZ ALTER INDEX ZAKÁZAT</span><span class="sxs-lookup"><span data-stu-id="ccea7-902">ALTER INDEX DISABLE</span></span>
* <span data-ttu-id="ccea7-903">DROP INDEX</span><span class="sxs-lookup"><span data-stu-id="ccea7-903">DROP INDEX</span></span>
* <span data-ttu-id="ccea7-904">Vytvoření, aktualizace a použít příkaz DROP STATISTICS</span><span class="sxs-lookup"><span data-stu-id="ccea7-904">CREATE, UPDATE, or DROP STATISTICS</span></span>
* <span data-ttu-id="ccea7-905">ZKRÁCENÍ TABULKY</span><span class="sxs-lookup"><span data-stu-id="ccea7-905">TRUNCATE TABLE</span></span>
* <span data-ttu-id="ccea7-906">PŘÍKAZ ALTER AUTORIZACE</span><span class="sxs-lookup"><span data-stu-id="ccea7-906">ALTER AUTHORIZATION</span></span>
* <span data-ttu-id="ccea7-907">VYTVOŘIT PŘIHLÁŠENÍ</span><span class="sxs-lookup"><span data-stu-id="ccea7-907">CREATE LOGIN</span></span>
* <span data-ttu-id="ccea7-908">CREATE, ALTER nebo DROP USER</span><span class="sxs-lookup"><span data-stu-id="ccea7-908">CREATE, ALTER or DROP USER</span></span>
* <span data-ttu-id="ccea7-909">CREATE, ALTER nebo VYŘADIT postup</span><span class="sxs-lookup"><span data-stu-id="ccea7-909">CREATE, ALTER or DROP PROCEDURE</span></span>
* <span data-ttu-id="ccea7-910">Vytvořit nebo VYŘADIT zobrazení</span><span class="sxs-lookup"><span data-stu-id="ccea7-910">CREATE or DROP VIEW</span></span>
* <span data-ttu-id="ccea7-911">VLOŽENÍ HODNOTY</span><span class="sxs-lookup"><span data-stu-id="ccea7-911">INSERT VALUES</span></span>
* <span data-ttu-id="ccea7-912">Vyberte z systémová zobrazení a zobrazení dynamické správy</span><span class="sxs-lookup"><span data-stu-id="ccea7-912">SELECT from system views and DMVs</span></span>
* <span data-ttu-id="ccea7-913">VYSVĚTLUJÍ</span><span class="sxs-lookup"><span data-stu-id="ccea7-913">EXPLAIN</span></span>
* <span data-ttu-id="ccea7-914">PŘÍKAZ DBCC</span><span class="sxs-lookup"><span data-stu-id="ccea7-914">DBCC</span></span>

<!--
Removed as these two are not confirmed / supported under SQLDW
- CREATE REMOTE TABLE AS SELECT
- CREATE EXTERNAL TABLE AS SELECT
- REDISTRIBUTE
-->

##  <span data-ttu-id="ccea7-915"><a name="changing-user-resource-class-example"></a>Změnit v příkladu třída prostředků uživatele</span><span class="sxs-lookup"><span data-stu-id="ccea7-915"><a name="changing-user-resource-class-example"></a> Change a user resource class example</span></span>
1. <span data-ttu-id="ccea7-916">**Vytvořit přihlášení:** otevřete připojení tooyour **hlavní** databáze na serveru SQL hello hostování vaší databázi SQL Data Warehouse a spusťte následující příkazy hello.</span><span class="sxs-lookup"><span data-stu-id="ccea7-916">**Create login:** Open a connection tooyour **master** database on hello SQL server hosting your SQL Data Warehouse database and execute hello following commands.</span></span>
   
    ```sql
    CREATE LOGIN newperson WITH PASSWORD = 'mypassword';
    CREATE USER newperson for LOGIN newperson;
    ```
   
   > [!NOTE]
   > <span data-ttu-id="ccea7-917">Je vhodné toocreate uživatele v hlavní databázi hello pro uživatele Azure SQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="ccea7-917">It is a good idea toocreate a user in hello master database for Azure SQL Data Warehouse users.</span></span> <span data-ttu-id="ccea7-918">Vytváření uživatele v předloze umožňuje uživatele toologin, pomocí nástroje, například aplikace SSMS bez zadání názvu databáze.</span><span class="sxs-lookup"><span data-stu-id="ccea7-918">Creating a user in master allows a user toologin using tools like SSMS without specifying a database name.</span></span>  <span data-ttu-id="ccea7-919">Umožňuje také jejich toouse hello object explorer tooview všechny databáze na serveru SQL server.</span><span class="sxs-lookup"><span data-stu-id="ccea7-919">It also allows them toouse hello object explorer tooview all databases on a SQL server.</span></span>  <span data-ttu-id="ccea7-920">Další informace o vytváření a správě uživatelů najdete v tématu [zabezpečení databáze v SQL Data Warehouse][Secure a database in SQL Data Warehouse].</span><span class="sxs-lookup"><span data-stu-id="ccea7-920">For more details about creating and managing users, see [Secure a database in SQL Data Warehouse][Secure a database in SQL Data Warehouse].</span></span>
   > 
   > 
2. <span data-ttu-id="ccea7-921">**Vytvoření SQL Data Warehouse uživatele:** otevřete připojení toohello **SQL Data Warehouse** databáze a spusťte následující příkaz hello.</span><span class="sxs-lookup"><span data-stu-id="ccea7-921">**Create SQL Data Warehouse user:** Open a connection toohello **SQL Data Warehouse** database and execute hello following command.</span></span>
   
    ```sql
    CREATE USER newperson FOR LOGIN newperson;
    ```
3. <span data-ttu-id="ccea7-922">**Udělení oprávnění:** hello následující příklad uděluje `CONTROL` na hello **SQL Data Warehouse** databáze.</span><span class="sxs-lookup"><span data-stu-id="ccea7-922">**Grant permissions:** hello following example grants `CONTROL` on hello **SQL Data Warehouse** database.</span></span> <span data-ttu-id="ccea7-923">`CONTROL`v hello je hello úroveň databáze ekvivalentní role db_owner v systému SQL Server.</span><span class="sxs-lookup"><span data-stu-id="ccea7-923">`CONTROL` at hello database level is hello equivalent of db_owner in SQL Server.</span></span>
   
    ```sql
    GRANT CONTROL ON DATABASE::MySQLDW toonewperson;
    ```
4. <span data-ttu-id="ccea7-924">**Zvýšit Třída prostředků:** použití hello následující dotaz tooadd roli uživatele tooa vyšší úlohy správy.</span><span class="sxs-lookup"><span data-stu-id="ccea7-924">**Increase resource class:** Use hello following query tooadd a user tooa higher workload management role.</span></span>
   
    ```sql
    EXEC sp_addrolemember 'largerc', 'newperson'
    ```
5. <span data-ttu-id="ccea7-925">**Třída prostředků snížit:** použití hello následující dotaz tooremove uživatele z role úlohy správy.</span><span class="sxs-lookup"><span data-stu-id="ccea7-925">**Decrease resource class:** Use hello following query tooremove a user from a workload management role.</span></span>
   
    ```sql
    EXEC sp_droprolemember 'largerc', 'newperson';
    ```
   
   > [!NOTE]
   > <span data-ttu-id="ccea7-926">Není možné tooremove uživatele z smallrc.</span><span class="sxs-lookup"><span data-stu-id="ccea7-926">It is not possible tooremove a user from smallrc.</span></span>
   > 
   > 

## <a name="queued-query-detection-and-other-dmvs"></a><span data-ttu-id="ccea7-927">Detekce ve frontě dotazů a jiné zobrazení dynamické správy</span><span class="sxs-lookup"><span data-stu-id="ccea7-927">Queued query detection and other DMVs</span></span>
<span data-ttu-id="ccea7-928">Můžete použít hello `sys.dm_pdw_exec_requests` DMV tooidentify dotazy, které jsou čekajících ve frontě souběžnosti.</span><span class="sxs-lookup"><span data-stu-id="ccea7-928">You can use hello `sys.dm_pdw_exec_requests` DMV tooidentify queries that are waiting in a concurrency queue.</span></span> <span data-ttu-id="ccea7-929">Dotazuje čekání slot souběžnosti bude mít stav **pozastaveno**.</span><span class="sxs-lookup"><span data-stu-id="ccea7-929">Queries waiting for a concurrency slot will have a status of **suspended**.</span></span>

```sql
SELECT      r.[request_id]                 AS Request_ID
        ,r.[status]                 AS Request_Status
        ,r.[submit_time]             AS Request_SubmitTime
        ,r.[start_time]                 AS Request_StartTime
        ,DATEDIFF(ms,[submit_time],[start_time]) AS Request_InitiateDuration_ms
        ,r.resource_class                         AS Request_resource_class
FROM    sys.dm_pdw_exec_requests r;
```

<span data-ttu-id="ccea7-930">Role správy úloh lze zobrazit pomocí `sys.database_principals`.</span><span class="sxs-lookup"><span data-stu-id="ccea7-930">Workload management roles can be viewed with `sys.database_principals`.</span></span>

```sql
SELECT  ro.[name]           AS [db_role_name]
FROM    sys.database_principals ro
WHERE   ro.[type_desc]      = 'DATABASE_ROLE'
AND     ro.[is_fixed_role]  = 0;
```

<span data-ttu-id="ccea7-931">Hello následující dotaz uvádí role, které každý uživatel je přiřazen k.</span><span class="sxs-lookup"><span data-stu-id="ccea7-931">hello following query shows which role each user is assigned to.</span></span>

```sql
SELECT     r.name AS role_principal_name
        ,m.name AS member_principal_name
FROM    sys.database_role_members rm
JOIN    sys.database_principals AS r            ON rm.role_principal_id        = r.principal_id
JOIN    sys.database_principals AS m            ON rm.member_principal_id    = m.principal_id
WHERE    r.name IN ('mediumrc','largerc', 'xlargerc');
```

<span data-ttu-id="ccea7-932">SQL Data Warehouse má následující hello počkejte typy:</span><span class="sxs-lookup"><span data-stu-id="ccea7-932">SQL Data Warehouse has hello following wait types:</span></span>

* <span data-ttu-id="ccea7-933">**LocalQueriesConcurrencyResourceType**: dotazy, které se nacházejí mimo hello souběžnosti slotu framework.</span><span class="sxs-lookup"><span data-stu-id="ccea7-933">**LocalQueriesConcurrencyResourceType**: Queries that sit outside of hello concurrency slot framework.</span></span> <span data-ttu-id="ccea7-934">DMV dotazy a systému funkce, jako `SELECT @@VERSION` jsou příklady místní dotazů.</span><span class="sxs-lookup"><span data-stu-id="ccea7-934">DMV queries and system functions such as `SELECT @@VERSION` are examples of local queries.</span></span>
* <span data-ttu-id="ccea7-935">**UserConcurrencyResourceType**: dotazy, které se nacházejí v hello souběžnosti slotu framework.</span><span class="sxs-lookup"><span data-stu-id="ccea7-935">**UserConcurrencyResourceType**: Queries that sit inside hello concurrency slot framework.</span></span> <span data-ttu-id="ccea7-936">Dotazy pro koncového uživatele tabulky představují příklady, které byste použili tento typ prostředku.</span><span class="sxs-lookup"><span data-stu-id="ccea7-936">Queries against end-user tables represent examples that would use this resource type.</span></span>
* <span data-ttu-id="ccea7-937">**DmsConcurrencyResourceType**: počká vyplývající z operace přesunu dat.</span><span class="sxs-lookup"><span data-stu-id="ccea7-937">**DmsConcurrencyResourceType**: Waits resulting from data movement operations.</span></span>
* <span data-ttu-id="ccea7-938">**BackupConcurrencyResourceType**: Tento čekání označuje, že se záloha databáze.</span><span class="sxs-lookup"><span data-stu-id="ccea7-938">**BackupConcurrencyResourceType**: This wait indicates that a database is being backed up.</span></span> <span data-ttu-id="ccea7-939">Hello maximální hodnota pro tento typ prostředku je 1.</span><span class="sxs-lookup"><span data-stu-id="ccea7-939">hello maximum value for this resource type is 1.</span></span> <span data-ttu-id="ccea7-940">Pokud požadovaly více záloh v hello současně, hello jiné fronty.</span><span class="sxs-lookup"><span data-stu-id="ccea7-940">If multiple backups have been requested at hello same time, hello others will queue.</span></span>

<span data-ttu-id="ccea7-941">Hello `sys.dm_pdw_waits` DMV lze použít toosee prostředky, ke kterým žádost čeká.</span><span class="sxs-lookup"><span data-stu-id="ccea7-941">hello `sys.dm_pdw_waits` DMV can be used toosee which resources a request is waiting for.</span></span>

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

<span data-ttu-id="ccea7-942">Hello `sys.dm_pdw_resource_waits` DMV zobrazuje pouze hello prostředků čeká spotřebovávají daný dotaz.</span><span class="sxs-lookup"><span data-stu-id="ccea7-942">hello `sys.dm_pdw_resource_waits` DMV shows only hello resource waits consumed by a given query.</span></span> <span data-ttu-id="ccea7-943">Doba čekání prostředků pouze opatření hello času čekáním na prostředky toobe zadat, jak názvem na rozdíl od toosignal čekací dobu, což je čas hello je potřebná pro hello základní SQL servery tooschedule hello dotaz na hello procesoru.</span><span class="sxs-lookup"><span data-stu-id="ccea7-943">Resource wait time only measures hello time waiting for resources toobe provided, as opposed toosignal wait time, which is hello time it takes for hello underlying SQL servers tooschedule hello query onto hello CPU.</span></span>

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

<span data-ttu-id="ccea7-944">Hello `sys.dm_pdw_wait_stats` DMV lze použít pro analýzu Historický trend čeká.</span><span class="sxs-lookup"><span data-stu-id="ccea7-944">hello `sys.dm_pdw_wait_stats` DMV can be used for historic trend analysis of waits.</span></span>

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

## <a name="next-steps"></a><span data-ttu-id="ccea7-945">Další kroky</span><span class="sxs-lookup"><span data-stu-id="ccea7-945">Next steps</span></span>
<span data-ttu-id="ccea7-946">Další informace o správě uživatelů a zabezpečení najdete v tématu [zabezpečení databáze v SQL Data Warehouse][Secure a database in SQL Data Warehouse].</span><span class="sxs-lookup"><span data-stu-id="ccea7-946">For more information about managing database users and security, see [Secure a database in SQL Data Warehouse][Secure a database in SQL Data Warehouse].</span></span> <span data-ttu-id="ccea7-947">Další informace o tom, jak větší třídy prostředků můžete zlepšení kvality indexu columnstore clusteru, najdete v části [znovu sestavit indexy tooimprove segment kvality].</span><span class="sxs-lookup"><span data-stu-id="ccea7-947">For more information about how larger resource classes can improve clustered columnstore index quality, see [Rebuilding indexes tooimprove segment quality].</span></span>

<!--Image references-->

<!--Article references-->
[Secure a database in SQL Data Warehouse]: ./sql-data-warehouse-overview-manage-security.md
[znovu sestavit indexy tooimprove segment kvality]: ./sql-data-warehouse-tables-index.md#rebuilding-indexes-to-improve-segment-quality
[Secure a database in SQL Data Warehouse]: ./sql-data-warehouse-overview-manage-security.md

<!--MSDN references-->
[Managing Databases and Logins in Azure SQL Database]:https://msdn.microsoft.com/library/azure/ee336235.aspx

<!--Other Web references-->
