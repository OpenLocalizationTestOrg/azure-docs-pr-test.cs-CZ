---
title: "aaaManage výpočetní výkon v Azure SQL Data Warehouse (přehled) | Microsoft Docs"
description: "Výkon škálování možnosti v Azure SQL Data Warehouse. Horizontální navýšení kapacity úpravou Dwu nebo pozastavení a obnovení toosave náklady na výpočetní prostředky."
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
ms.openlocfilehash: 1ffbe8d694ac181eaeb6f585a2cee87a570ed7d5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="manage-compute-power-in-azure-sql-data-warehouse-overview"></a><span data-ttu-id="f5f2b-104">Spravovat výpočetní výkon v Azure SQL Data Warehouse (přehled)</span><span class="sxs-lookup"><span data-stu-id="f5f2b-104">Manage compute power in Azure SQL Data Warehouse (Overview)</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="f5f2b-105">Přehled</span><span class="sxs-lookup"><span data-stu-id="f5f2b-105">Overview</span></span>](sql-data-warehouse-manage-compute-overview.md)
> * [<span data-ttu-id="f5f2b-106">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="f5f2b-106">Portal</span></span>](sql-data-warehouse-manage-compute-portal.md)
> * [<span data-ttu-id="f5f2b-107">PowerShell</span><span class="sxs-lookup"><span data-stu-id="f5f2b-107">PowerShell</span></span>](sql-data-warehouse-manage-compute-powershell.md)
> * [<span data-ttu-id="f5f2b-108">REST</span><span class="sxs-lookup"><span data-stu-id="f5f2b-108">REST</span></span>](sql-data-warehouse-manage-compute-rest-api.md)
> * [<span data-ttu-id="f5f2b-109">TSQL</span><span class="sxs-lookup"><span data-stu-id="f5f2b-109">TSQL</span></span>](sql-data-warehouse-manage-compute-tsql.md)
>
>

<span data-ttu-id="f5f2b-110">Architektura Hello služby SQL Data Warehouse odděluje úložiště a výpočty, povolení jednotlivých tooscale nezávisle.</span><span class="sxs-lookup"><span data-stu-id="f5f2b-110">hello architecture of SQL Data Warehouse separates storage and compute, allowing each tooscale independently.</span></span> <span data-ttu-id="f5f2b-111">Výpočetní v důsledku toho může být nezávislá hello množství dat, požadavky na výkon škálovat toomeet.</span><span class="sxs-lookup"><span data-stu-id="f5f2b-111">As a result, compute can be scaled toomeet performance demands independent of hello amount of data.</span></span> <span data-ttu-id="f5f2b-112">Přirozený důsledek této architektury je, že [fakturace] [ billed] pro výpočetní prostředí a úložiště jsou oddělené.</span><span class="sxs-lookup"><span data-stu-id="f5f2b-112">A natural consequence of this architecture is that [billing][billed] for compute and storage is separate.</span></span> 

<span data-ttu-id="f5f2b-113">Tento přehled popisuje jak škálovat funguje s SQL Data Warehouse a jak tooutilize hello pozastavení, opětovné spuštění a možnosti škálování služby SQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="f5f2b-113">This overview describes how scale out works with SQL Data Warehouse and how tooutilize hello pause, resume, and scale capabilities of SQL Data Warehouse.</span></span> <span data-ttu-id="f5f2b-114">Poraďte se hello [datového skladu (Dwu) jednotky] [ data warehouse units (DWUs)] jak Dwu a výkonu související s toolearn stránky.</span><span class="sxs-lookup"><span data-stu-id="f5f2b-114">Consult hello [data warehouse units (DWUs)][data warehouse units (DWUs)] page toolearn how DWUs and performance are related.</span></span> 

## <a name="how-compute-management-operations-work-in-sql-data-warehouse"></a><span data-ttu-id="f5f2b-115">Jak výpočetní operace správy pracovat v SQL Data Warehouse</span><span class="sxs-lookup"><span data-stu-id="f5f2b-115">How compute management operations work in SQL Data Warehouse</span></span>
<span data-ttu-id="f5f2b-116">Architektura Hello pro SQL Data Warehouse se skládá z řídicí uzel, výpočetní uzly a vrstvy úložiště hello rozloženy 60 distribuce.</span><span class="sxs-lookup"><span data-stu-id="f5f2b-116">hello architecture for SQL Data Warehouse consists of a control node, compute nodes, and hello storage layer spread across 60 distributions.</span></span> 

<span data-ttu-id="f5f2b-117">Během normálního aktivní relace v SQL Data Warehouse váš systém hlavního uzlu spravuje hello metadata a obsahuje hello distribuované Optimalizátor dotazů.</span><span class="sxs-lookup"><span data-stu-id="f5f2b-117">During a normal active session in SQL Data Warehouse, your system's head node manages hello metadata and contains hello distributed query optimizer.</span></span> <span data-ttu-id="f5f2b-118">Pod tímto uzlem head jsou výpočetní uzly a vrstvě úložiště.</span><span class="sxs-lookup"><span data-stu-id="f5f2b-118">Beneath this head node are your compute nodes and your storage layer.</span></span> <span data-ttu-id="f5f2b-119">Pro 400 DWU má váš systém jeden hlavní uzel, čtyři výpočetní uzly a vrstvy úložiště hello, který se skládá z 60 distribuce.</span><span class="sxs-lookup"><span data-stu-id="f5f2b-119">For a DWU 400, your system has one head node, four compute nodes, and hello storage layer, consisting of 60 distributions.</span></span> 

<span data-ttu-id="f5f2b-120">Když podstoupit škálování nebo pozastavit operace, systém hello nejprve ukončí všechny příchozí dotazy a vrátí zpět transakce tooensure konzistentním stavu.</span><span class="sxs-lookup"><span data-stu-id="f5f2b-120">When you undergo a scale or pause operation, hello system first kills all incoming queries and then rolls back transactions tooensure a consistent state.</span></span> <span data-ttu-id="f5f2b-121">Pro operace škálování škálování dojde pouze po dokončení této transakční vrácení zpět.</span><span class="sxs-lookup"><span data-stu-id="f5f2b-121">For scale operations, scaling will only occur once this transactional rollback has completed.</span></span> <span data-ttu-id="f5f2b-122">U operace škálování hello systému zřizuje hello velmi požadovaný počet výpočetních uzlů a poté zahájí opětovné hello výpočetní uzly toohello úložiště vrstvy.</span><span class="sxs-lookup"><span data-stu-id="f5f2b-122">For a scale-up operation, hello system provisions hello extra desired number of compute nodes, and then begins reattaching hello compute nodes toohello storage layer.</span></span> <span data-ttu-id="f5f2b-123">Pro operaci vertikální snížení kapacity hello nepotřebné uzly jsou vydávány a hello zbývající výpočetní uzly připojte, sami toohello odpovídající počet distribuce.</span><span class="sxs-lookup"><span data-stu-id="f5f2b-123">For a scale-down operation, hello unneeded nodes are released and hello remaining compute nodes reattach themselves toohello appropriate number of distributions.</span></span> <span data-ttu-id="f5f2b-124">Operace pozastavení všechny výpočetní uzly jsou vydávány a váš systém určitým celou řadu metadata operations tooleave konečné systém stabilní.</span><span class="sxs-lookup"><span data-stu-id="f5f2b-124">For a pause operation, all compute nodes are released and your system will undergo a variety of metadata operations tooleave your final system in a stable state.</span></span>

| <span data-ttu-id="f5f2b-125">DWU</span><span class="sxs-lookup"><span data-stu-id="f5f2b-125">DWU</span></span>  | <span data-ttu-id="f5f2b-126">\#výpočetních uzlů</span><span class="sxs-lookup"><span data-stu-id="f5f2b-126">\#of compute nodes</span></span> | <span data-ttu-id="f5f2b-127">\#distribucí na uzel</span><span class="sxs-lookup"><span data-stu-id="f5f2b-127">\# of distributions per node</span></span> |
| ---- | ------------------ | ---------------------------- |
| <span data-ttu-id="f5f2b-128">100</span><span class="sxs-lookup"><span data-stu-id="f5f2b-128">100</span></span>  | <span data-ttu-id="f5f2b-129">1</span><span class="sxs-lookup"><span data-stu-id="f5f2b-129">1</span></span>                  | <span data-ttu-id="f5f2b-130">60</span><span class="sxs-lookup"><span data-stu-id="f5f2b-130">60</span></span>                           |
| <span data-ttu-id="f5f2b-131">200</span><span class="sxs-lookup"><span data-stu-id="f5f2b-131">200</span></span>  | <span data-ttu-id="f5f2b-132">2</span><span class="sxs-lookup"><span data-stu-id="f5f2b-132">2</span></span>                  | <span data-ttu-id="f5f2b-133">30</span><span class="sxs-lookup"><span data-stu-id="f5f2b-133">30</span></span>                           |
| <span data-ttu-id="f5f2b-134">300</span><span class="sxs-lookup"><span data-stu-id="f5f2b-134">300</span></span>  | <span data-ttu-id="f5f2b-135">3</span><span class="sxs-lookup"><span data-stu-id="f5f2b-135">3</span></span>                  | <span data-ttu-id="f5f2b-136">20</span><span class="sxs-lookup"><span data-stu-id="f5f2b-136">20</span></span>                           |
| <span data-ttu-id="f5f2b-137">400</span><span class="sxs-lookup"><span data-stu-id="f5f2b-137">400</span></span>  | <span data-ttu-id="f5f2b-138">4</span><span class="sxs-lookup"><span data-stu-id="f5f2b-138">4</span></span>                  | <span data-ttu-id="f5f2b-139">15</span><span class="sxs-lookup"><span data-stu-id="f5f2b-139">15</span></span>                           |
| <span data-ttu-id="f5f2b-140">500</span><span class="sxs-lookup"><span data-stu-id="f5f2b-140">500</span></span>  | <span data-ttu-id="f5f2b-141">5</span><span class="sxs-lookup"><span data-stu-id="f5f2b-141">5</span></span>                  | <span data-ttu-id="f5f2b-142">12</span><span class="sxs-lookup"><span data-stu-id="f5f2b-142">12</span></span>                           |
| <span data-ttu-id="f5f2b-143">600</span><span class="sxs-lookup"><span data-stu-id="f5f2b-143">600</span></span>  | <span data-ttu-id="f5f2b-144">6</span><span class="sxs-lookup"><span data-stu-id="f5f2b-144">6</span></span>                  | <span data-ttu-id="f5f2b-145">10</span><span class="sxs-lookup"><span data-stu-id="f5f2b-145">10</span></span>                           |
| <span data-ttu-id="f5f2b-146">1000</span><span class="sxs-lookup"><span data-stu-id="f5f2b-146">1000</span></span> | <span data-ttu-id="f5f2b-147">10</span><span class="sxs-lookup"><span data-stu-id="f5f2b-147">10</span></span>                 | <span data-ttu-id="f5f2b-148">6</span><span class="sxs-lookup"><span data-stu-id="f5f2b-148">6</span></span>                            |
| <span data-ttu-id="f5f2b-149">1200</span><span class="sxs-lookup"><span data-stu-id="f5f2b-149">1200</span></span> | <span data-ttu-id="f5f2b-150">12</span><span class="sxs-lookup"><span data-stu-id="f5f2b-150">12</span></span>                 | <span data-ttu-id="f5f2b-151">5</span><span class="sxs-lookup"><span data-stu-id="f5f2b-151">5</span></span>                            |
| <span data-ttu-id="f5f2b-152">1 500</span><span class="sxs-lookup"><span data-stu-id="f5f2b-152">1500</span></span> | <span data-ttu-id="f5f2b-153">15</span><span class="sxs-lookup"><span data-stu-id="f5f2b-153">15</span></span>                 | <span data-ttu-id="f5f2b-154">4</span><span class="sxs-lookup"><span data-stu-id="f5f2b-154">4</span></span>                            |
| <span data-ttu-id="f5f2b-155">2000</span><span class="sxs-lookup"><span data-stu-id="f5f2b-155">2000</span></span> | <span data-ttu-id="f5f2b-156">20</span><span class="sxs-lookup"><span data-stu-id="f5f2b-156">20</span></span>                 | <span data-ttu-id="f5f2b-157">3</span><span class="sxs-lookup"><span data-stu-id="f5f2b-157">3</span></span>                            |
| <span data-ttu-id="f5f2b-158">3000</span><span class="sxs-lookup"><span data-stu-id="f5f2b-158">3000</span></span> | <span data-ttu-id="f5f2b-159">30</span><span class="sxs-lookup"><span data-stu-id="f5f2b-159">30</span></span>                 | <span data-ttu-id="f5f2b-160">2</span><span class="sxs-lookup"><span data-stu-id="f5f2b-160">2</span></span>                            |
| <span data-ttu-id="f5f2b-161">6000</span><span class="sxs-lookup"><span data-stu-id="f5f2b-161">6000</span></span> | <span data-ttu-id="f5f2b-162">60</span><span class="sxs-lookup"><span data-stu-id="f5f2b-162">60</span></span>                 | <span data-ttu-id="f5f2b-163">1</span><span class="sxs-lookup"><span data-stu-id="f5f2b-163">1</span></span>                            |

<span data-ttu-id="f5f2b-164">jsou tři primární funkce pro správu výpočetních Hello:</span><span class="sxs-lookup"><span data-stu-id="f5f2b-164">hello three primary functions for managing compute are:</span></span>

1. <span data-ttu-id="f5f2b-165">Pozastavení</span><span class="sxs-lookup"><span data-stu-id="f5f2b-165">Pause</span></span>
2. <span data-ttu-id="f5f2b-166">Obnovit</span><span class="sxs-lookup"><span data-stu-id="f5f2b-166">Resume</span></span>
3. <span data-ttu-id="f5f2b-167">Měřítko</span><span class="sxs-lookup"><span data-stu-id="f5f2b-167">Scale</span></span>

<span data-ttu-id="f5f2b-168">Všechny tyto operace může trvat několik minut toocomplete.</span><span class="sxs-lookup"><span data-stu-id="f5f2b-168">Each of these operations may take several minutes toocomplete.</span></span> <span data-ttu-id="f5f2b-169">Pokud jste škálování nebo pozastavení nebo obnovení automaticky, můžete tooimplement logiku tooensure určitá operace byly dokončeny před pokračováním další akci.</span><span class="sxs-lookup"><span data-stu-id="f5f2b-169">If you are scaling/pausing/resuming automatically, you may want tooimplement logic tooensure that certain operations have been completed before proceeding with another action.</span></span> 

<span data-ttu-id="f5f2b-170">Kontroluje se stav databáze hello prostřednictvím různých koncových bodů vám umožní toocorrectly implementace automatizace těchto operací.</span><span class="sxs-lookup"><span data-stu-id="f5f2b-170">Checking hello database state through various endpoints will allow you toocorrectly implement automation of such operations.</span></span> <span data-ttu-id="f5f2b-171">portál Hello bude poskytovat oznámení po dokončení operaci a hello databáze aktuální stav, ale neumožňuje programový kontroly stavu.</span><span class="sxs-lookup"><span data-stu-id="f5f2b-171">hello portal will provide notification upon completion of an operation and hello databases current state but does not allow for programmatic checking of state.</span></span> 

>  [!NOTE]
>
>  <span data-ttu-id="f5f2b-172">Výpočetní funkce správy neexistuje mezi všechny koncové body.</span><span class="sxs-lookup"><span data-stu-id="f5f2b-172">Compute management functionality does not exist across all endpoints.</span></span>
>
>  

|              | <span data-ttu-id="f5f2b-173">Pozastavení nebo obnovení</span><span class="sxs-lookup"><span data-stu-id="f5f2b-173">Pause/Resume</span></span> | <span data-ttu-id="f5f2b-174">Měřítko</span><span class="sxs-lookup"><span data-stu-id="f5f2b-174">Scale</span></span> | <span data-ttu-id="f5f2b-175">Zkontrolujte stav databáze</span><span class="sxs-lookup"><span data-stu-id="f5f2b-175">Check database state</span></span> |
| ------------ | ------------ | ----- | -------------------- |
| <span data-ttu-id="f5f2b-176">portál Azure</span><span class="sxs-lookup"><span data-stu-id="f5f2b-176">Azure portal</span></span> | <span data-ttu-id="f5f2b-177">Ano</span><span class="sxs-lookup"><span data-stu-id="f5f2b-177">Yes</span></span>          | <span data-ttu-id="f5f2b-178">Ano</span><span class="sxs-lookup"><span data-stu-id="f5f2b-178">Yes</span></span>   | <span data-ttu-id="f5f2b-179">**Ne**</span><span class="sxs-lookup"><span data-stu-id="f5f2b-179">**No**</span></span>               |
| <span data-ttu-id="f5f2b-180">PowerShell</span><span class="sxs-lookup"><span data-stu-id="f5f2b-180">PowerShell</span></span>   | <span data-ttu-id="f5f2b-181">Ano</span><span class="sxs-lookup"><span data-stu-id="f5f2b-181">Yes</span></span>          | <span data-ttu-id="f5f2b-182">Ano</span><span class="sxs-lookup"><span data-stu-id="f5f2b-182">Yes</span></span>   | <span data-ttu-id="f5f2b-183">Ano</span><span class="sxs-lookup"><span data-stu-id="f5f2b-183">Yes</span></span>                  |
| <span data-ttu-id="f5f2b-184">REST API</span><span class="sxs-lookup"><span data-stu-id="f5f2b-184">REST API</span></span>     | <span data-ttu-id="f5f2b-185">Ano</span><span class="sxs-lookup"><span data-stu-id="f5f2b-185">Yes</span></span>          | <span data-ttu-id="f5f2b-186">Ano</span><span class="sxs-lookup"><span data-stu-id="f5f2b-186">Yes</span></span>   | <span data-ttu-id="f5f2b-187">Ano</span><span class="sxs-lookup"><span data-stu-id="f5f2b-187">Yes</span></span>                  |
| <span data-ttu-id="f5f2b-188">T-SQL</span><span class="sxs-lookup"><span data-stu-id="f5f2b-188">T-SQL</span></span>        | <span data-ttu-id="f5f2b-189">**Ne**</span><span class="sxs-lookup"><span data-stu-id="f5f2b-189">**No**</span></span>       | <span data-ttu-id="f5f2b-190">Ano</span><span class="sxs-lookup"><span data-stu-id="f5f2b-190">Yes</span></span>   | <span data-ttu-id="f5f2b-191">Ano</span><span class="sxs-lookup"><span data-stu-id="f5f2b-191">Yes</span></span>                  |



<a name="scale-compute-bk"></a>

## <a name="scale-compute"></a><span data-ttu-id="f5f2b-192">Škálování výpočetní kapacity</span><span class="sxs-lookup"><span data-stu-id="f5f2b-192">Scale compute</span></span>

<span data-ttu-id="f5f2b-193">Výkon v SQL Data Warehouse se měří v [datového skladu (Dwu) jednotky] [ data warehouse units (DWUs)] tedy abstraktní měr výpočetní prostředky, jako je například procesoru, paměti a vstupně-výstupní šířky pásma.</span><span class="sxs-lookup"><span data-stu-id="f5f2b-193">Performance in SQL Data Warehouse is measured in [data warehouse units (DWUs)][data warehouse units (DWUs)] which is an abstracted measure of compute resources such as CPU, memory, and I/O bandwidth.</span></span> <span data-ttu-id="f5f2b-194">Uživatel, který si přeje tooscale výkon jejich systému lze provést různé způsoby, například prostřednictvím portálu hello T-SQL a rozhraní REST API.</span><span class="sxs-lookup"><span data-stu-id="f5f2b-194">A user who wishes tooscale their system's performance can do so through various means, such as through hello portal, T-SQL, and REST APIs.</span></span> 

### <a name="how-do-i-scale-compute"></a><span data-ttu-id="f5f2b-195">Jak škálovat výpočetní?</span><span class="sxs-lookup"><span data-stu-id="f5f2b-195">How do I scale compute?</span></span>
<span data-ttu-id="f5f2b-196">Výpočetní výkon spravovaná SQL Data Warehouse změnou nastavení DWU hello.</span><span class="sxs-lookup"><span data-stu-id="f5f2b-196">Compute power is managed for you SQL Data Warehouse by changing hello DWU setting.</span></span> <span data-ttu-id="f5f2b-197">Zvýšení výkonu [lineárně] [ linearly] jako přidáte další DWU pro určité operace.</span><span class="sxs-lookup"><span data-stu-id="f5f2b-197">Performance increases [linearly][linearly] as you add more DWU for certain operations.</span></span>  <span data-ttu-id="f5f2b-198">Nabízíme DWU nabídky, které zajišťují, že výkon změní výrazně když je systém škálovat nahoru nebo dolů.</span><span class="sxs-lookup"><span data-stu-id="f5f2b-198">We offer DWU offerings that ensure that your performance will change noticeably when you scale your system up or down.</span></span> 

<span data-ttu-id="f5f2b-199">tooadjust Dwu, můžete použít některou z těchto jednotlivých metod.</span><span class="sxs-lookup"><span data-stu-id="f5f2b-199">tooadjust DWUs, you can use any of these individual methods.</span></span>

* <span data-ttu-id="f5f2b-200">[Škálování výpočetního výkonu pomocí portálu Azure][Scale compute power with Azure portal]</span><span class="sxs-lookup"><span data-stu-id="f5f2b-200">[Scale compute power with Azure portal][Scale compute power with Azure portal]</span></span>
* <span data-ttu-id="f5f2b-201">[Škálování výpočetního výkonu pomocí prostředí PowerShell][Scale compute power with PowerShell]</span><span class="sxs-lookup"><span data-stu-id="f5f2b-201">[Scale compute power with PowerShell][Scale compute power with PowerShell]</span></span>
* <span data-ttu-id="f5f2b-202">[Škálování výpočetní výkon pomocí rozhraní REST API][Scale compute power with REST APIs]</span><span class="sxs-lookup"><span data-stu-id="f5f2b-202">[Scale compute power with REST APIs][Scale compute power with REST APIs]</span></span>
* <span data-ttu-id="f5f2b-203">[Škálování výpočetního výkonu pomocí TSQL][Scale compute power with TSQL]</span><span class="sxs-lookup"><span data-stu-id="f5f2b-203">[Scale compute power with TSQL][Scale compute power with TSQL]</span></span>

### <a name="how-many-dwus-should-i-use"></a><span data-ttu-id="f5f2b-204">Kolik Dwu mám použít?</span><span class="sxs-lookup"><span data-stu-id="f5f2b-204">How many DWUs should I use?</span></span>

<span data-ttu-id="f5f2b-205">toounderstand jaké ideální hodnota DWU je, zkuste škálování nahoru a dolů a spustit pár dotazů po načtení dat.</span><span class="sxs-lookup"><span data-stu-id="f5f2b-205">toounderstand what your ideal DWU value is, try scaling up and down, and running a few queries after loading your data.</span></span> <span data-ttu-id="f5f2b-206">Škálování je rychlé, můžete zkusit různé úrovně výkonu za hodinu nebo méně.</span><span class="sxs-lookup"><span data-stu-id="f5f2b-206">Since scaling is quick, you can try various performance levels in an hour or less.</span></span> 

> [!Note] 
> <span data-ttu-id="f5f2b-207">Datový sklad SQL je navrženou tooprocess velkých objemů dat.</span><span class="sxs-lookup"><span data-stu-id="f5f2b-207">SQL Data Warehouse is designed tooprocess large amounts of data.</span></span> <span data-ttu-id="f5f2b-208">toosee možnosti true pro škálování, zejména u větší Dwu, chcete toouse velké datové sady, který se blíží nebo je vyšší než 1 TB.</span><span class="sxs-lookup"><span data-stu-id="f5f2b-208">toosee its true capabilities for scaling, especially at larger DWUs, you want toouse a large data set which approaches or exceeds 1 TB.</span></span>

<span data-ttu-id="f5f2b-209">Doporučení pro vyhledání hello nejlepší DWU pro úlohy:</span><span class="sxs-lookup"><span data-stu-id="f5f2b-209">Recommendations for finding hello best DWU for your workload:</span></span>

1. <span data-ttu-id="f5f2b-210">Pro datový sklad v vývoj Začněte výběrem menší úroveň výkonu DWU.</span><span class="sxs-lookup"><span data-stu-id="f5f2b-210">For a data warehouse in development, begin by selecting a smaller DWU performance level.</span></span>  <span data-ttu-id="f5f2b-211">Vhodná výchozí hodnota je DW400 nebo DW200.</span><span class="sxs-lookup"><span data-stu-id="f5f2b-211">A good starting point is DW400 or DW200.</span></span>
2. <span data-ttu-id="f5f2b-212">Monitorovat výkon aplikací, sledování hello počet Dwu vybrané porovnání výkonu toohello zjistíte.</span><span class="sxs-lookup"><span data-stu-id="f5f2b-212">Monitor your application performance, observing hello number of DWUs selected compared toohello performance you observe.</span></span>
3. <span data-ttu-id="f5f2b-213">Určují, jak daleko vyšší nebo nižší výkon by mělo být pro jste tooreach hello optimální úroveň výkonu pro vaše požadavky podle za předpokladu, že lineární stupnice.</span><span class="sxs-lookup"><span data-stu-id="f5f2b-213">Determine how much faster or slower performance should be for you tooreach hello optimum performance level for your requirements by assuming linear scale.</span></span>
4. <span data-ttu-id="f5f2b-214">Zvýšení nebo snížení počtu hello Dwu v poměru toohow mnohem vyšší nebo nižší, na které má vaše tooperform zatížení.</span><span class="sxs-lookup"><span data-stu-id="f5f2b-214">Increase or decrease hello number of DWUs in proportion toohow much faster or slower you want your workload tooperform.</span></span> 
5. <span data-ttu-id="f5f2b-215">Pokračujte v provádění úprav, dokud se nedostanete na úroveň optimálního výkonu pro vaše podnikové požadavky.</span><span class="sxs-lookup"><span data-stu-id="f5f2b-215">Continue making adjustments until you reach an optimum performance level for your business requirements.</span></span>

> [!NOTE]
>
> <span data-ttu-id="f5f2b-216">Výkon dotazů pouze hodnota se zvyšuje s další paralelizace Pokud hello pracovní můžete rozdělit mezi výpočetní uzly.</span><span class="sxs-lookup"><span data-stu-id="f5f2b-216">Query performance only increases with more parallelization if hello work can be split between compute nodes.</span></span> <span data-ttu-id="f5f2b-217">Pokud zjistíte, že škálování není změna výkon, podrobnosti naleznete v našem ladění toocheck články, zda je vaše data nerovnoměrně distribuované nebo pokud jsou představení velké množství přesun dat výkonu.</span><span class="sxs-lookup"><span data-stu-id="f5f2b-217">If you find that scaling is not changing your performance, please check out our performance tuning articles toocheck whether your data is unevenly distributed or if you are introducing a large amount of data movement.</span></span> 

### <a name="when-should-i-scale-dwus"></a><span data-ttu-id="f5f2b-218">Pokud by měl škálování Dwu?</span><span class="sxs-lookup"><span data-stu-id="f5f2b-218">When should I scale DWUs?</span></span>
<span data-ttu-id="f5f2b-219">Škálování Dwu mění hello následující důležité scénáře:</span><span class="sxs-lookup"><span data-stu-id="f5f2b-219">Scaling DWUs alters hello following important scenarios:</span></span>

1. <span data-ttu-id="f5f2b-220">Změna lineárně výkon systému hello kontrol, agregace a funkce CTAS příkazy</span><span class="sxs-lookup"><span data-stu-id="f5f2b-220">Linearly changing performance of hello system for scans, aggregations, and CTAS statements</span></span>
2. <span data-ttu-id="f5f2b-221">Při načítání pomocí funkce PolyBase zvýšit počet hello čtení a zápis</span><span class="sxs-lookup"><span data-stu-id="f5f2b-221">Increasing hello number of readers and writers when loading with PolyBase</span></span>
3. <span data-ttu-id="f5f2b-222">Maximální počet souběžných dotazů a sloty souběžnosti</span><span class="sxs-lookup"><span data-stu-id="f5f2b-222">Maximum number of concurrent queries and concurrency slots</span></span>

<span data-ttu-id="f5f2b-223">Doporučení pro případ tooscale Dwu:</span><span class="sxs-lookup"><span data-stu-id="f5f2b-223">Recommendations for when tooscale DWUs:</span></span>

1. <span data-ttu-id="f5f2b-224">Před provedením operace načítání nebo transformace dat těžká, škálovat Dwu tak, aby vaše data jsou k dispozici rychleji.</span><span class="sxs-lookup"><span data-stu-id="f5f2b-224">Before you perform a heavy data loading or transformation operation, scale up DWUs so that your data is available more quickly.</span></span>
2. <span data-ttu-id="f5f2b-225">Během pracovní dobu ve špičce škálovat tooaccommodate velký počet souběžných dotazů.</span><span class="sxs-lookup"><span data-stu-id="f5f2b-225">During peak business hours, scale tooaccommodate larger numbers of concurrent queries.</span></span> 

<a name="pause-compute-bk"></a>

## <a name="pause-compute"></a><span data-ttu-id="f5f2b-226">Pozastavit výpočetní</span><span class="sxs-lookup"><span data-stu-id="f5f2b-226">Pause compute</span></span>
[!INCLUDE [SQL Data Warehouse pause description](../../includes/sql-data-warehouse-pause-description.md)]

<span data-ttu-id="f5f2b-227">toopause databázi, použijte některou z těchto jednotlivých metod.</span><span class="sxs-lookup"><span data-stu-id="f5f2b-227">toopause a database, use any of these individual methods.</span></span>

* <span data-ttu-id="f5f2b-228">[Pozastavit výpočetní pomocí portálu Azure][Pause compute with Azure portal]</span><span class="sxs-lookup"><span data-stu-id="f5f2b-228">[Pause compute with Azure portal][Pause compute with Azure portal]</span></span>
* <span data-ttu-id="f5f2b-229">[Pozastavit výpočetní pomocí prostředí PowerShell][Pause compute with PowerShell]</span><span class="sxs-lookup"><span data-stu-id="f5f2b-229">[Pause compute with PowerShell][Pause compute with PowerShell]</span></span>
* <span data-ttu-id="f5f2b-230">[Pozastavit výpočetní pomocí rozhraní REST API][Pause compute with REST APIs]</span><span class="sxs-lookup"><span data-stu-id="f5f2b-230">[Pause compute with REST APIs][Pause compute with REST APIs]</span></span>

<a name="resume-compute-bk"></a>

## <a name="resume-compute"></a><span data-ttu-id="f5f2b-231">Obnovit výpočetní</span><span class="sxs-lookup"><span data-stu-id="f5f2b-231">Resume compute</span></span>
[!INCLUDE [SQL Data Warehouse resume description](../../includes/sql-data-warehouse-resume-description.md)]

<span data-ttu-id="f5f2b-232">tooresume databázi, použijte některou z těchto jednotlivých metod.</span><span class="sxs-lookup"><span data-stu-id="f5f2b-232">tooresume a database, use any of these individual methods.</span></span>

* <span data-ttu-id="f5f2b-233">[Výpočetní obnovit pomocí portálu Azure][Resume compute with Azure portal]</span><span class="sxs-lookup"><span data-stu-id="f5f2b-233">[Resume compute with Azure portal][Resume compute with Azure portal]</span></span>
* <span data-ttu-id="f5f2b-234">[Výpočetní obnovení pomocí prostředí PowerShell][Resume compute with PowerShell]</span><span class="sxs-lookup"><span data-stu-id="f5f2b-234">[Resume compute with PowerShell][Resume compute with PowerShell]</span></span>
* <span data-ttu-id="f5f2b-235">[Obnovit výpočetní pomocí rozhraní REST API][Resume compute with REST APIs]</span><span class="sxs-lookup"><span data-stu-id="f5f2b-235">[Resume compute with REST APIs][Resume compute with REST APIs]</span></span>

<a name="check-compute-bk"></a>

## <a name="check-database-state"></a><span data-ttu-id="f5f2b-236">Zkontrolujte stav databáze</span><span class="sxs-lookup"><span data-stu-id="f5f2b-236">Check database state</span></span> 

<span data-ttu-id="f5f2b-237">tooresume databázi, použijte některou z těchto jednotlivých metod.</span><span class="sxs-lookup"><span data-stu-id="f5f2b-237">tooresume a database, use any of these individual methods.</span></span>

- <span data-ttu-id="f5f2b-238">[Zkontrolujte stav databáze s T-SQL][Check database state with T-SQL]</span><span class="sxs-lookup"><span data-stu-id="f5f2b-238">[Check database state with T-SQL][Check database state with T-SQL]</span></span>
- <span data-ttu-id="f5f2b-239">[Zkontrolujte stav databáze pomocí prostředí PowerShell][Check database state with PowerShell]</span><span class="sxs-lookup"><span data-stu-id="f5f2b-239">[Check database state with PowerShell][Check database state with PowerShell]</span></span>
- <span data-ttu-id="f5f2b-240">[Zkontrolujte stav databáze pomocí rozhraní REST API][Check database state with REST APIs]</span><span class="sxs-lookup"><span data-stu-id="f5f2b-240">[Check database state with REST APIs][Check database state with REST APIs]</span></span>

## <a name="permissions"></a><span data-ttu-id="f5f2b-241">Oprávnění</span><span class="sxs-lookup"><span data-stu-id="f5f2b-241">Permissions</span></span>

<span data-ttu-id="f5f2b-242">Škálování hello databáze vyžaduje hello oprávnění popsaná v [ALTER DATABASE][ALTER DATABASE].</span><span class="sxs-lookup"><span data-stu-id="f5f2b-242">Scaling hello database requires hello permissions described in [ALTER DATABASE][ALTER DATABASE].</span></span>  <span data-ttu-id="f5f2b-243">Pozastavení a obnovení vyžadují hello [Přispěvatel databází SQL] [ SQL DB Contributor] oprávnění, konkrétně Microsoft.Sql/servers/databases/action.</span><span class="sxs-lookup"><span data-stu-id="f5f2b-243">Pause and Resume require hello [SQL DB Contributor][SQL DB Contributor] permission, specifically Microsoft.Sql/servers/databases/action.</span></span>

<a name="next-steps-bk"></a>

## <a name="next-steps"></a><span data-ttu-id="f5f2b-244">Další kroky</span><span class="sxs-lookup"><span data-stu-id="f5f2b-244">Next steps</span></span>
<span data-ttu-id="f5f2b-245">Odkažte toohello následující články toohelp základními pojmy některé další klíče výkonu:</span><span class="sxs-lookup"><span data-stu-id="f5f2b-245">Refer toohello following articles toohelp you understand some additional key performance concepts:</span></span>

* <span data-ttu-id="f5f2b-246">[Správa úloh a souběžnost][Workload and concurrency management]</span><span class="sxs-lookup"><span data-stu-id="f5f2b-246">[Workload and concurrency management][Workload and concurrency management]</span></span>
* <span data-ttu-id="f5f2b-247">[Přehled návrhu tabulky][Table design overview]</span><span class="sxs-lookup"><span data-stu-id="f5f2b-247">[Table design overview][Table design overview]</span></span>
* <span data-ttu-id="f5f2b-248">[Distribuce tabulky][Table distribution]</span><span class="sxs-lookup"><span data-stu-id="f5f2b-248">[Table distribution][Table distribution]</span></span>
* <span data-ttu-id="f5f2b-249">[Tabulka indexování][Table indexing]</span><span class="sxs-lookup"><span data-stu-id="f5f2b-249">[Table indexing][Table indexing]</span></span>
* <span data-ttu-id="f5f2b-250">[Vytváření oddílů tabulky][Table partitioning]</span><span class="sxs-lookup"><span data-stu-id="f5f2b-250">[Table partitioning][Table partitioning]</span></span>
* <span data-ttu-id="f5f2b-251">[Statistiky tabulky][Table statistics]</span><span class="sxs-lookup"><span data-stu-id="f5f2b-251">[Table statistics][Table statistics]</span></span>
* <span data-ttu-id="f5f2b-252">[Doporučené postupy][Best practices]</span><span class="sxs-lookup"><span data-stu-id="f5f2b-252">[Best practices][Best practices]</span></span>

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
