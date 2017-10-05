---
title: "Spravovat výpočetní výkon v Azure SQL Data Warehouse (přehled) | Microsoft Docs"
description: "Výkon škálování možnosti v Azure SQL Data Warehouse. Horizontální navýšení kapacity úpravou Dwu nebo pozastavení a obnovení výpočetní prostředky, abyste ušetřili náklady."
services: sql-data-warehouse
documentationcenter: NA
author: hirokib
manager: johnmac
editor: 
ms.assetid: e13a82b0-abfe-429f-ac3c-f2b6789a70c6
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: manage
ms.date: 03/22/2017
ms.author: elbutter
ms.openlocfilehash: abe22f542a79714f6e894870872ee6b76ffe7633
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="manage-compute-power-in-azure-sql-data-warehouse-overview"></a><span data-ttu-id="09696-104">Spravovat výpočetní výkon v Azure SQL Data Warehouse (přehled)</span><span class="sxs-lookup"><span data-stu-id="09696-104">Manage compute power in Azure SQL Data Warehouse (Overview)</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="09696-105">Přehled</span><span class="sxs-lookup"><span data-stu-id="09696-105">Overview</span></span>](sql-data-warehouse-manage-compute-overview.md)
> * [<span data-ttu-id="09696-106">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="09696-106">Portal</span></span>](sql-data-warehouse-manage-compute-portal.md)
> * [<span data-ttu-id="09696-107">PowerShell</span><span class="sxs-lookup"><span data-stu-id="09696-107">PowerShell</span></span>](sql-data-warehouse-manage-compute-powershell.md)
> * [<span data-ttu-id="09696-108">REST</span><span class="sxs-lookup"><span data-stu-id="09696-108">REST</span></span>](sql-data-warehouse-manage-compute-rest-api.md)
> * [<span data-ttu-id="09696-109">TSQL</span><span class="sxs-lookup"><span data-stu-id="09696-109">TSQL</span></span>](sql-data-warehouse-manage-compute-tsql.md)
>
>

<span data-ttu-id="09696-110">Architektura služby SQL Data Warehouse odděluje úložiště a výpočty, což nezávislé škálování.</span><span class="sxs-lookup"><span data-stu-id="09696-110">The architecture of SQL Data Warehouse separates storage and compute, allowing each to scale independently.</span></span> <span data-ttu-id="09696-111">V důsledku toho je možné rozšířit výpočetní splňovat požadavky na výkon nezávislé na množství dat.</span><span class="sxs-lookup"><span data-stu-id="09696-111">As a result, compute can be scaled to meet performance demands independent of the amount of data.</span></span> <span data-ttu-id="09696-112">Přirozený důsledek této architektury je, že [fakturace] [ billed] pro výpočetní prostředí a úložiště jsou oddělené.</span><span class="sxs-lookup"><span data-stu-id="09696-112">A natural consequence of this architecture is that [billing][billed] for compute and storage is separate.</span></span> 

<span data-ttu-id="09696-113">Tento přehled popisuje jak škálovat funguje s SQL Data Warehouse a jak využívat pozastavení, obnovení a škálování možnosti služby SQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="09696-113">This overview describes how scale out works with SQL Data Warehouse and how to utilize the pause, resume, and scale capabilities of SQL Data Warehouse.</span></span> <span data-ttu-id="09696-114">Obrátit [datového skladu (Dwu) jednotky] [ data warehouse units (DWUs)] stránky se dozvíte, jak se vztahují Dwu a výkonu.</span><span class="sxs-lookup"><span data-stu-id="09696-114">Consult the [data warehouse units (DWUs)][data warehouse units (DWUs)] page to learn how DWUs and performance are related.</span></span> 

## <a name="how-compute-management-operations-work-in-sql-data-warehouse"></a><span data-ttu-id="09696-115">Jak výpočetní operace správy pracovat v SQL Data Warehouse</span><span class="sxs-lookup"><span data-stu-id="09696-115">How compute management operations work in SQL Data Warehouse</span></span>
<span data-ttu-id="09696-116">Architektura pro SQL Data Warehouse se skládá z řídicí uzel, výpočetní uzly a vrstvy úložiště rozloženy 60 distribuce.</span><span class="sxs-lookup"><span data-stu-id="09696-116">The architecture for SQL Data Warehouse consists of a control node, compute nodes, and the storage layer spread across 60 distributions.</span></span> 

<span data-ttu-id="09696-117">Během normálního aktivní relace v SQL Data Warehouse váš systém hlavního uzlu spravuje metadata a obsahuje Optimalizátor distribuovaných dotazů.</span><span class="sxs-lookup"><span data-stu-id="09696-117">During a normal active session in SQL Data Warehouse, your system's head node manages the metadata and contains the distributed query optimizer.</span></span> <span data-ttu-id="09696-118">Pod tímto uzlem head jsou výpočetní uzly a vrstvě úložiště.</span><span class="sxs-lookup"><span data-stu-id="09696-118">Beneath this head node are your compute nodes and your storage layer.</span></span> <span data-ttu-id="09696-119">Pro 400 DWU má váš systém jeden hlavní uzel, čtyři výpočetní uzly a vrstvy úložiště, který se skládá z 60 distribuce.</span><span class="sxs-lookup"><span data-stu-id="09696-119">For a DWU 400, your system has one head node, four compute nodes, and the storage layer, consisting of 60 distributions.</span></span> 

<span data-ttu-id="09696-120">Když podstoupit škálování nebo pozastavit operace, systém nejprve ukončí všechny příchozí dotazy a vrátí zpět transakce zajistit konzistentní stav.</span><span class="sxs-lookup"><span data-stu-id="09696-120">When you undergo a scale or pause operation, the system first kills all incoming queries and then rolls back transactions to ensure a consistent state.</span></span> <span data-ttu-id="09696-121">Pro operace škálování škálování dojde pouze po dokončení této transakční vrácení zpět.</span><span class="sxs-lookup"><span data-stu-id="09696-121">For scale operations, scaling will only occur once this transactional rollback has completed.</span></span> <span data-ttu-id="09696-122">U operace škálování zřizuje systému nadbytečné potřeby počet výpočetních uzlů a poté zahájí opětovné výpočetní uzly do vrstvy úložiště.</span><span class="sxs-lookup"><span data-stu-id="09696-122">For a scale-up operation, the system provisions the extra desired number of compute nodes, and then begins reattaching the compute nodes to the storage layer.</span></span> <span data-ttu-id="09696-123">Pro vertikální snížení kapacity operaci jsou vydávány nepotřebné uzly a zbývající výpočetní uzly sami připojte k odpovídající počet distribuce.</span><span class="sxs-lookup"><span data-stu-id="09696-123">For a scale-down operation, the unneeded nodes are released and the remaining compute nodes reattach themselves to the appropriate number of distributions.</span></span> <span data-ttu-id="09696-124">Operace pozastavení všechny výpočetní uzly jsou vydávány a systém se má provést různých operací s metadaty ponechat vaší poslední systém stabilní.</span><span class="sxs-lookup"><span data-stu-id="09696-124">For a pause operation, all compute nodes are released and your system will undergo a variety of metadata operations to leave your final system in a stable state.</span></span>

| <span data-ttu-id="09696-125">DWU</span><span class="sxs-lookup"><span data-stu-id="09696-125">DWU</span></span>  | <span data-ttu-id="09696-126">\#výpočetních uzlů</span><span class="sxs-lookup"><span data-stu-id="09696-126">\#of compute nodes</span></span> | <span data-ttu-id="09696-127">\#distribucí na uzel</span><span class="sxs-lookup"><span data-stu-id="09696-127">\# of distributions per node</span></span> |
| ---- | ------------------ | ---------------------------- |
| <span data-ttu-id="09696-128">100</span><span class="sxs-lookup"><span data-stu-id="09696-128">100</span></span>  | <span data-ttu-id="09696-129">1</span><span class="sxs-lookup"><span data-stu-id="09696-129">1</span></span>                  | <span data-ttu-id="09696-130">60</span><span class="sxs-lookup"><span data-stu-id="09696-130">60</span></span>                           |
| <span data-ttu-id="09696-131">200</span><span class="sxs-lookup"><span data-stu-id="09696-131">200</span></span>  | <span data-ttu-id="09696-132">2</span><span class="sxs-lookup"><span data-stu-id="09696-132">2</span></span>                  | <span data-ttu-id="09696-133">30</span><span class="sxs-lookup"><span data-stu-id="09696-133">30</span></span>                           |
| <span data-ttu-id="09696-134">300</span><span class="sxs-lookup"><span data-stu-id="09696-134">300</span></span>  | <span data-ttu-id="09696-135">3</span><span class="sxs-lookup"><span data-stu-id="09696-135">3</span></span>                  | <span data-ttu-id="09696-136">20</span><span class="sxs-lookup"><span data-stu-id="09696-136">20</span></span>                           |
| <span data-ttu-id="09696-137">400</span><span class="sxs-lookup"><span data-stu-id="09696-137">400</span></span>  | <span data-ttu-id="09696-138">4</span><span class="sxs-lookup"><span data-stu-id="09696-138">4</span></span>                  | <span data-ttu-id="09696-139">15</span><span class="sxs-lookup"><span data-stu-id="09696-139">15</span></span>                           |
| <span data-ttu-id="09696-140">500</span><span class="sxs-lookup"><span data-stu-id="09696-140">500</span></span>  | <span data-ttu-id="09696-141">5</span><span class="sxs-lookup"><span data-stu-id="09696-141">5</span></span>                  | <span data-ttu-id="09696-142">12</span><span class="sxs-lookup"><span data-stu-id="09696-142">12</span></span>                           |
| <span data-ttu-id="09696-143">600</span><span class="sxs-lookup"><span data-stu-id="09696-143">600</span></span>  | <span data-ttu-id="09696-144">6</span><span class="sxs-lookup"><span data-stu-id="09696-144">6</span></span>                  | <span data-ttu-id="09696-145">10</span><span class="sxs-lookup"><span data-stu-id="09696-145">10</span></span>                           |
| <span data-ttu-id="09696-146">1000</span><span class="sxs-lookup"><span data-stu-id="09696-146">1000</span></span> | <span data-ttu-id="09696-147">10</span><span class="sxs-lookup"><span data-stu-id="09696-147">10</span></span>                 | <span data-ttu-id="09696-148">6</span><span class="sxs-lookup"><span data-stu-id="09696-148">6</span></span>                            |
| <span data-ttu-id="09696-149">1200</span><span class="sxs-lookup"><span data-stu-id="09696-149">1200</span></span> | <span data-ttu-id="09696-150">12</span><span class="sxs-lookup"><span data-stu-id="09696-150">12</span></span>                 | <span data-ttu-id="09696-151">5</span><span class="sxs-lookup"><span data-stu-id="09696-151">5</span></span>                            |
| <span data-ttu-id="09696-152">1 500</span><span class="sxs-lookup"><span data-stu-id="09696-152">1500</span></span> | <span data-ttu-id="09696-153">15</span><span class="sxs-lookup"><span data-stu-id="09696-153">15</span></span>                 | <span data-ttu-id="09696-154">4</span><span class="sxs-lookup"><span data-stu-id="09696-154">4</span></span>                            |
| <span data-ttu-id="09696-155">2000</span><span class="sxs-lookup"><span data-stu-id="09696-155">2000</span></span> | <span data-ttu-id="09696-156">20</span><span class="sxs-lookup"><span data-stu-id="09696-156">20</span></span>                 | <span data-ttu-id="09696-157">3</span><span class="sxs-lookup"><span data-stu-id="09696-157">3</span></span>                            |
| <span data-ttu-id="09696-158">3000</span><span class="sxs-lookup"><span data-stu-id="09696-158">3000</span></span> | <span data-ttu-id="09696-159">30</span><span class="sxs-lookup"><span data-stu-id="09696-159">30</span></span>                 | <span data-ttu-id="09696-160">2</span><span class="sxs-lookup"><span data-stu-id="09696-160">2</span></span>                            |
| <span data-ttu-id="09696-161">6000</span><span class="sxs-lookup"><span data-stu-id="09696-161">6000</span></span> | <span data-ttu-id="09696-162">60</span><span class="sxs-lookup"><span data-stu-id="09696-162">60</span></span>                 | <span data-ttu-id="09696-163">1</span><span class="sxs-lookup"><span data-stu-id="09696-163">1</span></span>                            |

<span data-ttu-id="09696-164">Tři primární funkce pro správu výpočetních jsou:</span><span class="sxs-lookup"><span data-stu-id="09696-164">The three primary functions for managing compute are:</span></span>

1. <span data-ttu-id="09696-165">Pozastavení</span><span class="sxs-lookup"><span data-stu-id="09696-165">Pause</span></span>
2. <span data-ttu-id="09696-166">Obnovit</span><span class="sxs-lookup"><span data-stu-id="09696-166">Resume</span></span>
3. <span data-ttu-id="09696-167">Měřítko</span><span class="sxs-lookup"><span data-stu-id="09696-167">Scale</span></span>

<span data-ttu-id="09696-168">Všechny tyto operace může trvat několik minut na dokončení.</span><span class="sxs-lookup"><span data-stu-id="09696-168">Each of these operations may take several minutes to complete.</span></span> <span data-ttu-id="09696-169">Pokud si nejste škálování nebo pozastavení nebo obnovení automaticky, můžete implementovat logiku zajistit, že některé operace dokončeny před pokračováním další akci.</span><span class="sxs-lookup"><span data-stu-id="09696-169">If you are scaling/pausing/resuming automatically, you may want to implement logic to ensure that certain operations have been completed before proceeding with another action.</span></span> 

<span data-ttu-id="09696-170">Kontroluje se stav databáze prostřednictvím různých koncových bodů vám umožní správně implementovat automatizace těchto operací.</span><span class="sxs-lookup"><span data-stu-id="09696-170">Checking the database state through various endpoints will allow you to correctly implement automation of such operations.</span></span> <span data-ttu-id="09696-171">Na portálu oznámení po dokončení operace a databáze bude poskytovat, aktuální stav, ale neumožňuje programový kontroly stavu.</span><span class="sxs-lookup"><span data-stu-id="09696-171">The portal will provide notification upon completion of an operation and the databases current state but does not allow for programmatic checking of state.</span></span> 

>  [!NOTE]
>
>  <span data-ttu-id="09696-172">Výpočetní funkce správy neexistuje mezi všechny koncové body.</span><span class="sxs-lookup"><span data-stu-id="09696-172">Compute management functionality does not exist across all endpoints.</span></span>
>
>  

|              | <span data-ttu-id="09696-173">Pozastavení nebo obnovení</span><span class="sxs-lookup"><span data-stu-id="09696-173">Pause/Resume</span></span> | <span data-ttu-id="09696-174">Měřítko</span><span class="sxs-lookup"><span data-stu-id="09696-174">Scale</span></span> | <span data-ttu-id="09696-175">Zkontrolujte stav databáze</span><span class="sxs-lookup"><span data-stu-id="09696-175">Check database state</span></span> |
| ------------ | ------------ | ----- | -------------------- |
| <span data-ttu-id="09696-176">portál Azure</span><span class="sxs-lookup"><span data-stu-id="09696-176">Azure portal</span></span> | <span data-ttu-id="09696-177">Ano</span><span class="sxs-lookup"><span data-stu-id="09696-177">Yes</span></span>          | <span data-ttu-id="09696-178">Ano</span><span class="sxs-lookup"><span data-stu-id="09696-178">Yes</span></span>   | <span data-ttu-id="09696-179">**Ne**</span><span class="sxs-lookup"><span data-stu-id="09696-179">**No**</span></span>               |
| <span data-ttu-id="09696-180">PowerShell</span><span class="sxs-lookup"><span data-stu-id="09696-180">PowerShell</span></span>   | <span data-ttu-id="09696-181">Ano</span><span class="sxs-lookup"><span data-stu-id="09696-181">Yes</span></span>          | <span data-ttu-id="09696-182">Ano</span><span class="sxs-lookup"><span data-stu-id="09696-182">Yes</span></span>   | <span data-ttu-id="09696-183">Ano</span><span class="sxs-lookup"><span data-stu-id="09696-183">Yes</span></span>                  |
| <span data-ttu-id="09696-184">REST API</span><span class="sxs-lookup"><span data-stu-id="09696-184">REST API</span></span>     | <span data-ttu-id="09696-185">Ano</span><span class="sxs-lookup"><span data-stu-id="09696-185">Yes</span></span>          | <span data-ttu-id="09696-186">Ano</span><span class="sxs-lookup"><span data-stu-id="09696-186">Yes</span></span>   | <span data-ttu-id="09696-187">Ano</span><span class="sxs-lookup"><span data-stu-id="09696-187">Yes</span></span>                  |
| <span data-ttu-id="09696-188">T-SQL</span><span class="sxs-lookup"><span data-stu-id="09696-188">T-SQL</span></span>        | <span data-ttu-id="09696-189">**Ne**</span><span class="sxs-lookup"><span data-stu-id="09696-189">**No**</span></span>       | <span data-ttu-id="09696-190">Ano</span><span class="sxs-lookup"><span data-stu-id="09696-190">Yes</span></span>   | <span data-ttu-id="09696-191">Ano</span><span class="sxs-lookup"><span data-stu-id="09696-191">Yes</span></span>                  |



<a name="scale-compute-bk"></a>

## <a name="scale-compute"></a><span data-ttu-id="09696-192">Škálování výpočetní kapacity</span><span class="sxs-lookup"><span data-stu-id="09696-192">Scale compute</span></span>

<span data-ttu-id="09696-193">Výkon v SQL Data Warehouse se měří v [datového skladu (Dwu) jednotky] [ data warehouse units (DWUs)] tedy abstraktní měr výpočetní prostředky, jako je například procesoru, paměti a vstupně-výstupní šířky pásma.</span><span class="sxs-lookup"><span data-stu-id="09696-193">Performance in SQL Data Warehouse is measured in [data warehouse units (DWUs)][data warehouse units (DWUs)] which is an abstracted measure of compute resources such as CPU, memory, and I/O bandwidth.</span></span> <span data-ttu-id="09696-194">Uživatel, který chce škálovat výkon jejich systému lze provést různé způsoby, například prostřednictvím portálu, T-SQL a rozhraní REST API.</span><span class="sxs-lookup"><span data-stu-id="09696-194">A user who wishes to scale their system's performance can do so through various means, such as through the portal, T-SQL, and REST APIs.</span></span> 

### <a name="how-do-i-scale-compute"></a><span data-ttu-id="09696-195">Jak škálovat výpočetní?</span><span class="sxs-lookup"><span data-stu-id="09696-195">How do I scale compute?</span></span>
<span data-ttu-id="09696-196">Výpočetní power spravovaná SQL Data Warehouse změnou nastavení DWU.</span><span class="sxs-lookup"><span data-stu-id="09696-196">Compute power is managed for you SQL Data Warehouse by changing the DWU setting.</span></span> <span data-ttu-id="09696-197">Zvýšení výkonu [lineárně] [ linearly] jako přidáte další DWU pro určité operace.</span><span class="sxs-lookup"><span data-stu-id="09696-197">Performance increases [linearly][linearly] as you add more DWU for certain operations.</span></span>  <span data-ttu-id="09696-198">Nabízíme DWU nabídky, které zajišťují, že výkon změní výrazně když je systém škálovat nahoru nebo dolů.</span><span class="sxs-lookup"><span data-stu-id="09696-198">We offer DWU offerings that ensure that your performance will change noticeably when you scale your system up or down.</span></span> 

<span data-ttu-id="09696-199">Chcete-li upravit Dwu, můžete použít některou z těchto jednotlivých metod.</span><span class="sxs-lookup"><span data-stu-id="09696-199">To adjust DWUs, you can use any of these individual methods.</span></span>

* <span data-ttu-id="09696-200">[Škálování výpočetního výkonu pomocí portálu Azure][Scale compute power with Azure portal]</span><span class="sxs-lookup"><span data-stu-id="09696-200">[Scale compute power with Azure portal][Scale compute power with Azure portal]</span></span>
* <span data-ttu-id="09696-201">[Škálování výpočetního výkonu pomocí prostředí PowerShell][Scale compute power with PowerShell]</span><span class="sxs-lookup"><span data-stu-id="09696-201">[Scale compute power with PowerShell][Scale compute power with PowerShell]</span></span>
* <span data-ttu-id="09696-202">[Škálování výpočetní výkon pomocí rozhraní REST API][Scale compute power with REST APIs]</span><span class="sxs-lookup"><span data-stu-id="09696-202">[Scale compute power with REST APIs][Scale compute power with REST APIs]</span></span>
* <span data-ttu-id="09696-203">[Škálování výpočetního výkonu pomocí TSQL][Scale compute power with TSQL]</span><span class="sxs-lookup"><span data-stu-id="09696-203">[Scale compute power with TSQL][Scale compute power with TSQL]</span></span>

### <a name="how-many-dwus-should-i-use"></a><span data-ttu-id="09696-204">Kolik Dwu mám použít?</span><span class="sxs-lookup"><span data-stu-id="09696-204">How many DWUs should I use?</span></span>

<span data-ttu-id="09696-205">Když chcete spolehlivě zjistit, jaká hodnota DWU je pro vás optimální, zkuste po načtení dat vertikálně navýšit a snížit kapacitu a spustit pár dotazů.</span><span class="sxs-lookup"><span data-stu-id="09696-205">To understand what your ideal DWU value is, try scaling up and down, and running a few queries after loading your data.</span></span> <span data-ttu-id="09696-206">Škálování je rychlé, můžete zkusit různé úrovně výkonu za hodinu nebo méně.</span><span class="sxs-lookup"><span data-stu-id="09696-206">Since scaling is quick, you can try various performance levels in an hour or less.</span></span> 

> [!Note] 
> <span data-ttu-id="09696-207">SQL Data Warehouse je navržen pro zpracování velkých objemů dat.</span><span class="sxs-lookup"><span data-stu-id="09696-207">SQL Data Warehouse is designed to process large amounts of data.</span></span> <span data-ttu-id="09696-208">Zobrazíte možnosti true pro škálování, zejména u větší Dwu, budete chtít použít velké datové sady, který se blíží nebo je vyšší než 1 TB.</span><span class="sxs-lookup"><span data-stu-id="09696-208">To see its true capabilities for scaling, especially at larger DWUs, you want to use a large data set which approaches or exceeds 1 TB.</span></span>

<span data-ttu-id="09696-209">Doporučení pro hledání nejlepší DWU pro úlohy:</span><span class="sxs-lookup"><span data-stu-id="09696-209">Recommendations for finding the best DWU for your workload:</span></span>

1. <span data-ttu-id="09696-210">Pro datový sklad v vývoj Začněte výběrem menší úroveň výkonu DWU.</span><span class="sxs-lookup"><span data-stu-id="09696-210">For a data warehouse in development, begin by selecting a smaller DWU performance level.</span></span>  <span data-ttu-id="09696-211">Vhodná výchozí hodnota je DW400 nebo DW200.</span><span class="sxs-lookup"><span data-stu-id="09696-211">A good starting point is DW400 or DW200.</span></span>
2. <span data-ttu-id="09696-212">Monitorujte výkon aplikací, sledování počet Dwu vybrané ve srovnání s, které můžete sledovat výkon.</span><span class="sxs-lookup"><span data-stu-id="09696-212">Monitor your application performance, observing the number of DWUs selected compared to the performance you observe.</span></span>
3. <span data-ttu-id="09696-213">Určete, jak daleko vyšší nebo nižší výkon by měla být pro vás k dosažení optimálního výkonu úrovně pro vaše požadavky pomocí za předpokladu, že lineární stupnice.</span><span class="sxs-lookup"><span data-stu-id="09696-213">Determine how much faster or slower performance should be for you to reach the optimum performance level for your requirements by assuming linear scale.</span></span>
4. <span data-ttu-id="09696-214">Zvyšte nebo snižte počet Dwu v poměru k jak mnohem rychlejší nebo něco pomalejší chcete úlohu provést.</span><span class="sxs-lookup"><span data-stu-id="09696-214">Increase or decrease the number of DWUs in proportion to how much faster or slower you want your workload to perform.</span></span> 
5. <span data-ttu-id="09696-215">Pokračujte v provádění úprav, dokud se nedostanete na úroveň optimálního výkonu pro vaše podnikové požadavky.</span><span class="sxs-lookup"><span data-stu-id="09696-215">Continue making adjustments until you reach an optimum performance level for your business requirements.</span></span>

> [!NOTE]
>
> <span data-ttu-id="09696-216">Výkon dotazů pouze hodnota se zvyšuje s další paralelizace Pokud práci můžete rozdělit mezi výpočetní uzly.</span><span class="sxs-lookup"><span data-stu-id="09696-216">Query performance only increases with more parallelization if the work can be split between compute nodes.</span></span> <span data-ttu-id="09696-217">Pokud zjistíte, že škálování není změna výkon, podrobnosti naleznete v našem ladění články zkontrolujte, zda je vaše data nerovnoměrně distribuované nebo pokud jsou představení velké množství přesun dat výkonu.</span><span class="sxs-lookup"><span data-stu-id="09696-217">If you find that scaling is not changing your performance, please check out our performance tuning articles to check whether your data is unevenly distributed or if you are introducing a large amount of data movement.</span></span> 

### <a name="when-should-i-scale-dwus"></a><span data-ttu-id="09696-218">Pokud by měl škálování Dwu?</span><span class="sxs-lookup"><span data-stu-id="09696-218">When should I scale DWUs?</span></span>
<span data-ttu-id="09696-219">Škálování Dwu mění důležité následující scénáře:</span><span class="sxs-lookup"><span data-stu-id="09696-219">Scaling DWUs alters the following important scenarios:</span></span>

1. <span data-ttu-id="09696-220">Změna lineárně výkon systému pro prohledávání, agregace a funkce CTAS příkazy</span><span class="sxs-lookup"><span data-stu-id="09696-220">Linearly changing performance of the system for scans, aggregations, and CTAS statements</span></span>
2. <span data-ttu-id="09696-221">Při načítání pomocí funkce PolyBase zvýšit počet čtení a zápis</span><span class="sxs-lookup"><span data-stu-id="09696-221">Increasing the number of readers and writers when loading with PolyBase</span></span>
3. <span data-ttu-id="09696-222">Maximální počet souběžných dotazů a sloty souběžnosti</span><span class="sxs-lookup"><span data-stu-id="09696-222">Maximum number of concurrent queries and concurrency slots</span></span>

<span data-ttu-id="09696-223">Doporučení pro kdy škálovat Dwu:</span><span class="sxs-lookup"><span data-stu-id="09696-223">Recommendations for when to scale DWUs:</span></span>

1. <span data-ttu-id="09696-224">Před provedením operace načítání nebo transformace dat těžká, škálovat Dwu tak, aby vaše data jsou k dispozici rychleji.</span><span class="sxs-lookup"><span data-stu-id="09696-224">Before you perform a heavy data loading or transformation operation, scale up DWUs so that your data is available more quickly.</span></span>
2. <span data-ttu-id="09696-225">Během pracovní dobu ve špičce škálovat, aby dokázala pojmout větší počty souběžných dotazů.</span><span class="sxs-lookup"><span data-stu-id="09696-225">During peak business hours, scale to accommodate larger numbers of concurrent queries.</span></span> 

<a name="pause-compute-bk"></a>

## <a name="pause-compute"></a><span data-ttu-id="09696-226">Pozastavit výpočetní</span><span class="sxs-lookup"><span data-stu-id="09696-226">Pause compute</span></span>
[!INCLUDE [SQL Data Warehouse pause description](../../includes/sql-data-warehouse-pause-description.md)]

<span data-ttu-id="09696-227">Pokud chcete pozastavit databázi, použijte některou z těchto jednotlivých metod.</span><span class="sxs-lookup"><span data-stu-id="09696-227">To pause a database, use any of these individual methods.</span></span>

* <span data-ttu-id="09696-228">[Pozastavit výpočetní pomocí portálu Azure][Pause compute with Azure portal]</span><span class="sxs-lookup"><span data-stu-id="09696-228">[Pause compute with Azure portal][Pause compute with Azure portal]</span></span>
* <span data-ttu-id="09696-229">[Pozastavit výpočetní pomocí prostředí PowerShell][Pause compute with PowerShell]</span><span class="sxs-lookup"><span data-stu-id="09696-229">[Pause compute with PowerShell][Pause compute with PowerShell]</span></span>
* <span data-ttu-id="09696-230">[Pozastavit výpočetní pomocí rozhraní REST API][Pause compute with REST APIs]</span><span class="sxs-lookup"><span data-stu-id="09696-230">[Pause compute with REST APIs][Pause compute with REST APIs]</span></span>

<a name="resume-compute-bk"></a>

## <a name="resume-compute"></a><span data-ttu-id="09696-231">Obnovit výpočetní</span><span class="sxs-lookup"><span data-stu-id="09696-231">Resume compute</span></span>
[!INCLUDE [SQL Data Warehouse resume description](../../includes/sql-data-warehouse-resume-description.md)]

<span data-ttu-id="09696-232">Chcete-li obnovit databázi, použijte některou z těchto jednotlivých metod.</span><span class="sxs-lookup"><span data-stu-id="09696-232">To resume a database, use any of these individual methods.</span></span>

* <span data-ttu-id="09696-233">[Výpočetní obnovit pomocí portálu Azure][Resume compute with Azure portal]</span><span class="sxs-lookup"><span data-stu-id="09696-233">[Resume compute with Azure portal][Resume compute with Azure portal]</span></span>
* <span data-ttu-id="09696-234">[Výpočetní obnovení pomocí prostředí PowerShell][Resume compute with PowerShell]</span><span class="sxs-lookup"><span data-stu-id="09696-234">[Resume compute with PowerShell][Resume compute with PowerShell]</span></span>
* <span data-ttu-id="09696-235">[Obnovit výpočetní pomocí rozhraní REST API][Resume compute with REST APIs]</span><span class="sxs-lookup"><span data-stu-id="09696-235">[Resume compute with REST APIs][Resume compute with REST APIs]</span></span>

<a name="check-compute-bk"></a>

## <a name="check-database-state"></a><span data-ttu-id="09696-236">Zkontrolujte stav databáze</span><span class="sxs-lookup"><span data-stu-id="09696-236">Check database state</span></span> 

<span data-ttu-id="09696-237">Chcete-li obnovit databázi, použijte některou z těchto jednotlivých metod.</span><span class="sxs-lookup"><span data-stu-id="09696-237">To resume a database, use any of these individual methods.</span></span>

- <span data-ttu-id="09696-238">[Zkontrolujte stav databáze s T-SQL][Check database state with T-SQL]</span><span class="sxs-lookup"><span data-stu-id="09696-238">[Check database state with T-SQL][Check database state with T-SQL]</span></span>
- <span data-ttu-id="09696-239">[Zkontrolujte stav databáze pomocí prostředí PowerShell][Check database state with PowerShell]</span><span class="sxs-lookup"><span data-stu-id="09696-239">[Check database state with PowerShell][Check database state with PowerShell]</span></span>
- <span data-ttu-id="09696-240">[Zkontrolujte stav databáze pomocí rozhraní REST API][Check database state with REST APIs]</span><span class="sxs-lookup"><span data-stu-id="09696-240">[Check database state with REST APIs][Check database state with REST APIs]</span></span>

## <a name="permissions"></a><span data-ttu-id="09696-241">Oprávnění</span><span class="sxs-lookup"><span data-stu-id="09696-241">Permissions</span></span>

<span data-ttu-id="09696-242">Škálování databáze vyžaduje oprávnění popsaná v [ALTER DATABASE][ALTER DATABASE].</span><span class="sxs-lookup"><span data-stu-id="09696-242">Scaling the database requires the permissions described in [ALTER DATABASE][ALTER DATABASE].</span></span>  <span data-ttu-id="09696-243">Pozastavení a obnovení vyžadují [Přispěvatel databází SQL] [ SQL DB Contributor] oprávnění, konkrétně Microsoft.Sql/servers/databases/action.</span><span class="sxs-lookup"><span data-stu-id="09696-243">Pause and Resume require the [SQL DB Contributor][SQL DB Contributor] permission, specifically Microsoft.Sql/servers/databases/action.</span></span>

<a name="next-steps-bk"></a>

## <a name="next-steps"></a><span data-ttu-id="09696-244">Další kroky</span><span class="sxs-lookup"><span data-stu-id="09696-244">Next steps</span></span>
<span data-ttu-id="09696-245">Najdete v následujících článcích, které vám pomohou pochopit některé pojmy další klíče výkonu:</span><span class="sxs-lookup"><span data-stu-id="09696-245">Refer to the following articles to help you understand some additional key performance concepts:</span></span>

* <span data-ttu-id="09696-246">[Správa úloh a souběžnost][Workload and concurrency management]</span><span class="sxs-lookup"><span data-stu-id="09696-246">[Workload and concurrency management][Workload and concurrency management]</span></span>
* <span data-ttu-id="09696-247">[Přehled návrhu tabulky][Table design overview]</span><span class="sxs-lookup"><span data-stu-id="09696-247">[Table design overview][Table design overview]</span></span>
* <span data-ttu-id="09696-248">[Distribuce tabulky][Table distribution]</span><span class="sxs-lookup"><span data-stu-id="09696-248">[Table distribution][Table distribution]</span></span>
* <span data-ttu-id="09696-249">[Tabulka indexování][Table indexing]</span><span class="sxs-lookup"><span data-stu-id="09696-249">[Table indexing][Table indexing]</span></span>
* <span data-ttu-id="09696-250">[Vytváření oddílů tabulky][Table partitioning]</span><span class="sxs-lookup"><span data-stu-id="09696-250">[Table partitioning][Table partitioning]</span></span>
* <span data-ttu-id="09696-251">[Statistiky tabulky][Table statistics]</span><span class="sxs-lookup"><span data-stu-id="09696-251">[Table statistics][Table statistics]</span></span>
* <span data-ttu-id="09696-252">[Doporučené postupy][Best practices]</span><span class="sxs-lookup"><span data-stu-id="09696-252">[Best practices][Best practices]</span></span>

<!--Image reference-->

<!--Article references-->
[data warehouse units (DWUs)]: ./sql-data-warehouse-overview-what-is.md#predictable-and-scalable-performance-with-data-warehouse-units
[billed]: https://azure.microsoft.com/en-us/pricing/details/sql-data-warehouse/
[linearly]: ./sql-data-warehouse-overview-what-is.md#predictable-and-scalable-performance-with-data-warehouse-units
[Scale compute power with Azure portal]: ./sql-data-warehouse-manage-compute-portal.md#scale-compute-power
[Scale compute power with PowerShell]: ./sql-data-warehouse-manage-compute-powershell.md#scale-compute-bk
[Scale compute power with REST APIs]: ./sql-data-warehouse-manage-compute-rest-api.md#scale-compute-bk
[Scale compute power with TSQL]: ./sql-data-warehouse-manage-compute-tsql.md#scale-compute-bk

[capacity limits]: ./sql-data-warehouse-service-capacity-limits.md

[Pause compute with Azure portal]:  ./sql-data-warehouse-manage-compute-portal.md#pause-compute-bk
[Pause compute with PowerShell]: ./sql-data-warehouse-manage-compute-powershell.md#pause-compute-bk
[Pause compute with REST APIs]: ./sql-data-warehouse-manage-compute-rest-api.md#pause-compute-bk

[Resume compute with Azure portal]:  ./sql-data-warehouse-manage-compute-portal.md#resume-compute-bk
[Resume compute with PowerShell]: ./sql-data-warehouse-manage-compute-powershell.md#resume-compute-bk
[Resume compute with REST APIs]: ./sql-data-warehouse-manage-compute-rest-api.md#resume-compute-bk

[Check database state with T-SQL]: ./sql-data-warehouse-manage-compute-tsql.md#check-database-state-and-operation-progress
[Check database state with PowerShell]: ./sql-data-warehouse-manage-compute-powershell.md#check-database-state
[Check database state with REST APIs]: ./sql-data-warehouse-manage-compute-rest-api.md#check-database-state

[Workload and concurrency management]: ./sql-data-warehouse-develop-concurrency.md
[Table design overview]: ./sql-data-warehouse-tables-overview.md
[Table distribution]: ./sql-data-warehouse-tables-distribute.md
[Table indexing]: ./sql-data-warehouse-tables-index.md
[Table partitioning]: ./sql-data-warehouse-tables-partition.md
[Table statistics]: ./sql-data-warehouse-tables-statistics.md
[Best practices]: ./sql-data-warehouse-best-practices.md
[development overview]: ./sql-data-warehouse-overview-develop.md

[SQL DB Contributor]: ../active-directory/role-based-access-built-in-roles.md#sql-db-contributor

<!--MSDN references-->
[ALTER DATABASE]: https://msdn.microsoft.com/library/mt204042.aspx

<!--Other Web references-->
[Azure portal]: http://portal.azure.com/
