---
title: "doporučení výkonu aaaApply – Azure SQL Database | Microsoft Docs"
description: "Můžete použít hello Azure portálu toofind výkonu doporučení, které můžete optimalizovat výkon databáze SQL Azure nebo toocorrect některé problém uvedený v vaše úlohy."
services: sql-database
documentationcenter: 
author: stevestein
manager: jhubbard
editor: monicar
ms.assetid: cda8a646-0584-4368-b28a-85cdd9b54fcd
ms.service: sql-database
ms.custom: monitor & tune
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: data-management
ms.date: 07/05/2017
ms.author: sstein
ms.openlocfilehash: 0b2234840fc3254d235db9e7d18f5bc912851c6d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="find-and-apply-performance-recommendations"></a><span data-ttu-id="1405b-103">Najít a použít doporučení výkonu</span><span class="sxs-lookup"><span data-stu-id="1405b-103">Find and apply performance recommendations</span></span>

<span data-ttu-id="1405b-104">Můžete použít hello Azure portálu toofind výkonu doporučení, které můžete optimalizovat výkon databáze SQL Azure nebo toocorrect některé problém uvedený v vaše úlohy.</span><span class="sxs-lookup"><span data-stu-id="1405b-104">You can use hello Azure portal toofind performance recommendations that can optimize performance of your Azure SQL Database or toocorrect some issue identified in your workload.</span></span> <span data-ttu-id="1405b-105">**Výkon doporučení** stránka na portálu Azure vám umožňuje toofind hello nejvyšší doporučení podle jejich potenciální vliv.</span><span class="sxs-lookup"><span data-stu-id="1405b-105">**Performance recommendation** page in Azure portal enables you toofind hello top recommendations based on their potential impact.</span></span> 

## <a name="viewing-recommendations"></a><span data-ttu-id="1405b-106">Zobrazení doporučení</span><span class="sxs-lookup"><span data-stu-id="1405b-106">Viewing recommendations</span></span>

<span data-ttu-id="1405b-107">tooview a použít výkonu doporučení, třeba hello správné [řízení přístupu na základě role](../active-directory/role-based-access-control-what-is.md) oprávnění v Azure.</span><span class="sxs-lookup"><span data-stu-id="1405b-107">tooview and apply performance recommendations, you need hello correct [role-based access control](../active-directory/role-based-access-control-what-is.md) permissions in Azure.</span></span> <span data-ttu-id="1405b-108">**Čtečka**, **Přispěvatel databází SQL** oprávnění jsou požadované tooview doporučení a **vlastníka**, **Přispěvatel databází SQL** jsou požadována oprávnění tooexecute všechny akce; Vytvořit nebo vyřadit indexy a zrušit vytváření indexu.</span><span class="sxs-lookup"><span data-stu-id="1405b-108">**Reader**, **SQL DB Contributor** permissions are required tooview recommendations, and **Owner**, **SQL DB Contributor** permissions are required tooexecute any actions; create or drop indexes and cancel index creation.</span></span>

<span data-ttu-id="1405b-109">Použijte následující postup toofind výkonu doporučení na portálu Azure hello:</span><span class="sxs-lookup"><span data-stu-id="1405b-109">Use hello following steps toofind performance recommendations on Azure portal:</span></span>

1. <span data-ttu-id="1405b-110">Přihlaste se toohello [portál Azure](https://portal.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="1405b-110">Sign in toohello [Azure portal](https://portal.azure.com/).</span></span>
2. <span data-ttu-id="1405b-111">Přejděte příliš**další služby** > **databází SQL**a vyberte svou databázi.</span><span class="sxs-lookup"><span data-stu-id="1405b-111">Go too**More services** > **SQL databases**, and select your database.</span></span>
3. <span data-ttu-id="1405b-112">Přejděte příliš**výkonu doporučení** tooview dostupná doporučení pro vybranou databázi hello.</span><span class="sxs-lookup"><span data-stu-id="1405b-112">Navigate too**Performance recommendation** tooview available recommendations for hello selected database.</span></span>

<span data-ttu-id="1405b-113">V tabulce hello podobné se toohello, té, kterou vidíte na následující obrázek hello výkonu doporučení jsou shonw:</span><span class="sxs-lookup"><span data-stu-id="1405b-113">Performance recommendations are shonw in hello table similar toohello one shown on hello following figure:</span></span>

![Doporučení](./media/sql-database-advisor-portal/recommendations.png)

<span data-ttu-id="1405b-115">Doporučení jsou seřazené podle jejich potenciální dopad na výkon do hello následujících kategorií:</span><span class="sxs-lookup"><span data-stu-id="1405b-115">Recommendations are sorted by their potential impact on performance into hello following categories:</span></span>

| <span data-ttu-id="1405b-116">Dopad</span><span class="sxs-lookup"><span data-stu-id="1405b-116">Impact</span></span> | <span data-ttu-id="1405b-117">Popis</span><span class="sxs-lookup"><span data-stu-id="1405b-117">Description</span></span> |
|:--- |:--- |
| <span data-ttu-id="1405b-118">Vysoký</span><span class="sxs-lookup"><span data-stu-id="1405b-118">High</span></span> |<span data-ttu-id="1405b-119">Vysoký dopad na doporučení by měl poskytovat hello nejvýraznější dopad na výkon.</span><span class="sxs-lookup"><span data-stu-id="1405b-119">High impact recommendations should provide hello most significant performance impact.</span></span> |
| <span data-ttu-id="1405b-120">Střednědobé používání</span><span class="sxs-lookup"><span data-stu-id="1405b-120">Medium</span></span> |<span data-ttu-id="1405b-121">Střední dopad doporučení by měla zvýšit výkon, ale není podstatně.</span><span class="sxs-lookup"><span data-stu-id="1405b-121">Medium impact recommendations should improve performance, but not substantially.</span></span> |
| <span data-ttu-id="1405b-122">Nízký</span><span class="sxs-lookup"><span data-stu-id="1405b-122">Low</span></span> |<span data-ttu-id="1405b-123">Nízký dopad doporučení by měl poskytovat lepší výkon než bez, ale nemusí být výrazné vylepšení.</span><span class="sxs-lookup"><span data-stu-id="1405b-123">Low impact recommendations should provide better performance than without, but improvements might not be significant.</span></span> |


> [!NOTE]
> <span data-ttu-id="1405b-124">Databáze SQL Azure vyžaduje toomonitor aktivity alespoň za den v pořadí tooidentify několik doporučení.</span><span class="sxs-lookup"><span data-stu-id="1405b-124">Azure SQL Database needs toomonitor activities at least for a day in order tooidentify some recommendations.</span></span> <span data-ttu-id="1405b-125">Hello Azure SQL Database můžete snadněji optimalizovat konzistentní dotazu v případě vzorů, než je to možné na náhodných problematického shluky aktivity.</span><span class="sxs-lookup"><span data-stu-id="1405b-125">hello Azure SQL Database can more easily optimize for consistent query patterns than it can for random spotty bursts of activity.</span></span> <span data-ttu-id="1405b-126">Pokud doporučení nejsou k dispozici corrently, hello **výkonu doporučení** stránka obsahuje zprávu s vysvětlením, proč.</span><span class="sxs-lookup"><span data-stu-id="1405b-126">If recommendations are not corrently available, hello **Performance recommendation** page provides a message explaining why.</span></span>
> 

<span data-ttu-id="1405b-127">Můžete také zobrazit stav hello hello historických operací.</span><span class="sxs-lookup"><span data-stu-id="1405b-127">You can also view hello status of hello historical operations.</span></span> <span data-ttu-id="1405b-128">Vyberte stav nebo doporučení toosee další podrobnosti.</span><span class="sxs-lookup"><span data-stu-id="1405b-128">Select a recommendation or status toosee  more details.</span></span>

<span data-ttu-id="1405b-129">Tady je příklad "Vytvořit index" doporučení v hello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="1405b-129">Here is an example of "Create index" recommendation in hello Azure portal.</span></span>

![Vytvoření indexu](./media/sql-database-advisor-portal/sql-database-performance-recommendation.png)

## <a name="applying-recommendations"></a><span data-ttu-id="1405b-131">Uplatňovat doporučení</span><span class="sxs-lookup"><span data-stu-id="1405b-131">Applying recommendations</span></span>
<span data-ttu-id="1405b-132">Databáze SQL Azure vám dává plnou kontrolu nad jak doporučení jsou povolit pomocí některé z hello následující tři možnosti:</span><span class="sxs-lookup"><span data-stu-id="1405b-132">Azure SQL Database gives you full control over how recommendations are enabled using any of hello following three options:</span></span> 

* <span data-ttu-id="1405b-133">Používat jednotlivé doporučení jeden v daném okamžiku.</span><span class="sxs-lookup"><span data-stu-id="1405b-133">Apply individual recommendations one at a time.</span></span>
* <span data-ttu-id="1405b-134">Povolit hello automatické ladění tooautomatically platí doporučení.</span><span class="sxs-lookup"><span data-stu-id="1405b-134">Enable hello Automatic tuning tooautomatically apply recommendations.</span></span>
* <span data-ttu-id="1405b-135">tooimplement doporučení ručně, spusťte hello doporučená skriptu T-SQL pro vaši databázi.</span><span class="sxs-lookup"><span data-stu-id="1405b-135">tooimplement a recommendation manually, run hello recommended T-SQL script against your database.</span></span>

<span data-ttu-id="1405b-136">Vyberte všechny doporučení tooview jeho podrobnosti a pak klikněte na **zobrazit skript** tooreview hello přesný podrobné informace o tom, jak je vytvoření hello doporučení.</span><span class="sxs-lookup"><span data-stu-id="1405b-136">Select any recommendation tooview its details and then click **View script** tooreview hello exact details of how hello recommendation is created.</span></span>

<span data-ttu-id="1405b-137">Hello databáze zůstane online, zatímco hello doporučení platí – pomocí výkonu doporučení nebo automatické ladění nikdy převede databázi do režimu offline.</span><span class="sxs-lookup"><span data-stu-id="1405b-137">hello database remains online while hello recommendation is applied -- using performance recommendation or automatic tuning never takes a database offline.</span></span>

### <a name="apply-an-individual-recommendation"></a><span data-ttu-id="1405b-138">Jednotlivá doporučení použít</span><span class="sxs-lookup"><span data-stu-id="1405b-138">Apply an individual recommendation</span></span>
<span data-ttu-id="1405b-139">Můžete zkontrolovat a přijmout doporučení jeden najednou.</span><span class="sxs-lookup"><span data-stu-id="1405b-139">You can review and accept recommendations one at a time.</span></span>

1. <span data-ttu-id="1405b-140">Na hello **doporučení** okně vyberte doporučení.</span><span class="sxs-lookup"><span data-stu-id="1405b-140">On hello **Recommendations** blade, select a recommendation.</span></span>
2. <span data-ttu-id="1405b-141">Na hello **podrobnosti** okně klikněte na tlačítko **použít** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="1405b-141">On hello **Details** blade click **Apply** button.</span></span>
   
    ![použít doporučení](./media/sql-database-advisor-portal/apply.png)

<span data-ttu-id="1405b-143">Vybrané doporučení se použijí v databázi hello.</span><span class="sxs-lookup"><span data-stu-id="1405b-143">Selected recommendation will be applied on hello database.</span></span>

### <a name="removing-recommendations-from-hello-list"></a><span data-ttu-id="1405b-144">Odebrání doporučení ze seznamu hello</span><span class="sxs-lookup"><span data-stu-id="1405b-144">Removing recommendations from hello list</span></span>
<span data-ttu-id="1405b-145">Pokud seznam doporučení obsahuje položky, které chcete tooremove hello seznamu, můžete zrušit hello doporučení:</span><span class="sxs-lookup"><span data-stu-id="1405b-145">If your list of recommendations contains items that you want tooremove from hello list, you can discard hello recommendation:</span></span>

1. <span data-ttu-id="1405b-146">Vyberte v seznamu hello doporučení **doporučení** tooopen hello podrobnosti.</span><span class="sxs-lookup"><span data-stu-id="1405b-146">Select a recommendation in hello list of **Recommendations** tooopen hello details.</span></span>
2. <span data-ttu-id="1405b-147">Klikněte na tlačítko **zahodit** na hello **podrobnosti** okno.</span><span class="sxs-lookup"><span data-stu-id="1405b-147">Click **Discard** on hello **Details** blade.</span></span>

<span data-ttu-id="1405b-148">V případě potřeby můžete přidat vyřazené položky zpět toohello **doporučení** seznamu:</span><span class="sxs-lookup"><span data-stu-id="1405b-148">If desired, you can add discarded items back toohello **Recommendations** list:</span></span>

1. <span data-ttu-id="1405b-149">Na hello **doporučení** okně klikněte na tlačítko **zobrazení zahozena**.</span><span class="sxs-lookup"><span data-stu-id="1405b-149">On hello **Recommendations** blade click **View discarded**.</span></span>
2. <span data-ttu-id="1405b-150">Vyberte položku zrušených ze seznamu tooview hello její podrobnosti.</span><span class="sxs-lookup"><span data-stu-id="1405b-150">Select a discarded item from hello list tooview its details.</span></span>
3. <span data-ttu-id="1405b-151">Volitelně klikněte na **vrátit zpět zahodit** tooadd hello index back toohello hlavní seznam **doporučení**.</span><span class="sxs-lookup"><span data-stu-id="1405b-151">Optionally, click **Undo Discard** tooadd hello index back toohello main list of **Recommendations**.</span></span>


### <a name="enable-automatic-tuning"></a><span data-ttu-id="1405b-152">Povolení automatického ladění</span><span class="sxs-lookup"><span data-stu-id="1405b-152">Enable automatic tuning</span></span>
<span data-ttu-id="1405b-153">Hello Azure SQL Database tooimplement doporučení můžete nastavit automaticky.</span><span class="sxs-lookup"><span data-stu-id="1405b-153">You can set hello Azure SQL Database tooimplement recommendations automatically.</span></span> <span data-ttu-id="1405b-154">Dostupná doporučení budou automaticky použity.</span><span class="sxs-lookup"><span data-stu-id="1405b-154">As recommendations become available they will automatically be applied.</span></span> <span data-ttu-id="1405b-155">Jako s všechna doporučení spravuje hello služby pokud vlivu na výkon hello je záporný hello doporučení budou vráceny.</span><span class="sxs-lookup"><span data-stu-id="1405b-155">As with all recommendations managed by hello service if hello performance impact is negative hello recommendation will be reverted.</span></span>

1. <span data-ttu-id="1405b-156">Na hello **doporučení** okně klikněte na tlačítko **automatické**:</span><span class="sxs-lookup"><span data-stu-id="1405b-156">On hello **Recommendations** blade, click **Automate**:</span></span>
   
    ![Nastavení služby Advisor](./media/sql-database-advisor-portal/settings.png)
2. <span data-ttu-id="1405b-158">Vyberte tooautomate akce:</span><span class="sxs-lookup"><span data-stu-id="1405b-158">Select actions tooautomate:</span></span>
   
    ![Doporučené indexy](./media/sql-database-advisor-portal/automation.png)

### <a name="manually-run-hello-recommended-t-sql-script"></a><span data-ttu-id="1405b-160">Ručně spustit hello doporučená skriptu T-SQL</span><span class="sxs-lookup"><span data-stu-id="1405b-160">Manually run hello recommended T-SQL script</span></span>
<span data-ttu-id="1405b-161">Vyberte všechny doporučení a pak klikněte na tlačítko **zobrazit skript**.</span><span class="sxs-lookup"><span data-stu-id="1405b-161">Select any recommendation and then click **View script**.</span></span> <span data-ttu-id="1405b-162">Spusťte tento skript pro vaši databázi toomanually použít hello doporučení.</span><span class="sxs-lookup"><span data-stu-id="1405b-162">Run this script against your database toomanually apply hello recommendation.</span></span>

<span data-ttu-id="1405b-163">*Indexy, které jsou spouštěny ručně nejsou sledovat a ověřit pro vlivu na výkon službou hello* , doporučuje se po vytvoření tooverify sledovat tyto indexy se poskytují zvýšení výkonu a upravit nebo odstranit je nezbytné.</span><span class="sxs-lookup"><span data-stu-id="1405b-163">*Indexes that are manually executed are not monitored and validated for performance impact by hello service* so it is suggested that you monitor these indexes after creation tooverify they provide performance gains and adjust or delete them if necessary.</span></span> <span data-ttu-id="1405b-164">Podrobnosti o vytváření indexů najdete v tématu [CREATE INDEX (Transact-SQL)](https://msdn.microsoft.com/library/ms188783.aspx).</span><span class="sxs-lookup"><span data-stu-id="1405b-164">For details about creating indexes, see [CREATE INDEX (Transact-SQL)](https://msdn.microsoft.com/library/ms188783.aspx).</span></span>

### <a name="canceling-recommendations"></a><span data-ttu-id="1405b-165">Zrušení doporučení</span><span class="sxs-lookup"><span data-stu-id="1405b-165">Canceling recommendations</span></span>
<span data-ttu-id="1405b-166">Doporučení, které jsou v **čekající**, **ověření**, nebo **úspěch** stav se dají zrušit.</span><span class="sxs-lookup"><span data-stu-id="1405b-166">Recommendations that are in a **Pending**, **Verifying**, or **Success** status can be canceled.</span></span> <span data-ttu-id="1405b-167">Doporučení se stavem **zpracování** nelze zrušit.</span><span class="sxs-lookup"><span data-stu-id="1405b-167">Recommendations with a status of **Executing** cannot be canceled.</span></span>

1. <span data-ttu-id="1405b-168">Vyberte doporučení v hello **ladění historie** oblasti tooopen hello **podrobnosti doporučení** okno.</span><span class="sxs-lookup"><span data-stu-id="1405b-168">Select a recommendation in hello **Tuning History** area tooopen hello **recommendations details** blade.</span></span>
2. <span data-ttu-id="1405b-169">Klikněte na tlačítko **zrušit** tooabort hello proces použití hello doporučení.</span><span class="sxs-lookup"><span data-stu-id="1405b-169">Click **Cancel** tooabort hello process of applying hello recommendation.</span></span>

## <a name="monitoring-operations"></a><span data-ttu-id="1405b-170">Monitorování operací</span><span class="sxs-lookup"><span data-stu-id="1405b-170">Monitoring operations</span></span>
<span data-ttu-id="1405b-171">Použití doporučeným nemusí dojít okamžitě.</span><span class="sxs-lookup"><span data-stu-id="1405b-171">Applying a recommendation might not happen instantaneously.</span></span> <span data-ttu-id="1405b-172">portál Hello poskytuje podrobnosti týkající se stavu hello doporučení.</span><span class="sxs-lookup"><span data-stu-id="1405b-172">hello portal provides details regarding hello status of recommendation.</span></span> <span data-ttu-id="1405b-173">možné stavy, které může být indexu se Hello následující:</span><span class="sxs-lookup"><span data-stu-id="1405b-173">hello following are possible states that an index can be in:</span></span>

| <span data-ttu-id="1405b-174">Status</span><span class="sxs-lookup"><span data-stu-id="1405b-174">Status</span></span> | <span data-ttu-id="1405b-175">Popis</span><span class="sxs-lookup"><span data-stu-id="1405b-175">Description</span></span> |
|:--- |:--- |
| <span data-ttu-id="1405b-176">Čekající na vyřízení</span><span class="sxs-lookup"><span data-stu-id="1405b-176">Pending</span></span> |<span data-ttu-id="1405b-177">Platí doporučení, příkaz byl přijat a je naplánováno spuštění.</span><span class="sxs-lookup"><span data-stu-id="1405b-177">Apply recommendation command has been received and is scheduled for execution.</span></span> |
| <span data-ttu-id="1405b-178">Provádění</span><span class="sxs-lookup"><span data-stu-id="1405b-178">Executing</span></span> |<span data-ttu-id="1405b-179">Hello doporučení se právě používá.</span><span class="sxs-lookup"><span data-stu-id="1405b-179">hello recommendation is being applied.</span></span> |
| <span data-ttu-id="1405b-180">Ověření</span><span class="sxs-lookup"><span data-stu-id="1405b-180">Verifying</span></span> |<span data-ttu-id="1405b-181">Bylo úspěšně aplikováno doporučení a hello služby je měření hello výhody.</span><span class="sxs-lookup"><span data-stu-id="1405b-181">Recommendation was successfully applied and hello service is measuring hello benefits.</span></span> |
| <span data-ttu-id="1405b-182">Úspěch</span><span class="sxs-lookup"><span data-stu-id="1405b-182">Success</span></span> |<span data-ttu-id="1405b-183">Bylo úspěšně aplikováno doporučení a měří výhody.</span><span class="sxs-lookup"><span data-stu-id="1405b-183">Recommendation was successfully applied and benefits have been measured.</span></span> |
| <span data-ttu-id="1405b-184">Chyba</span><span class="sxs-lookup"><span data-stu-id="1405b-184">Error</span></span> |<span data-ttu-id="1405b-185">Došlo k chybě během procesu hello použití hello doporučení.</span><span class="sxs-lookup"><span data-stu-id="1405b-185">An error occurred during hello process of applying hello recommendation.</span></span> <span data-ttu-id="1405b-186">Může to být dočasný problém, nebo může být tabulku toohello změnu schématu a skriptu hello již není platný.</span><span class="sxs-lookup"><span data-stu-id="1405b-186">This can be a transient issue, or possibly a schema change toohello table and hello script is no longer valid.</span></span> |
| <span data-ttu-id="1405b-187">Návrat</span><span class="sxs-lookup"><span data-stu-id="1405b-187">Reverting</span></span> |<span data-ttu-id="1405b-188">doporučení Hello bylo použito, ale je považována za nenáročných a automaticky zrušeny.</span><span class="sxs-lookup"><span data-stu-id="1405b-188">hello recommendation was applied, but has been deemed non-performant and is being automatically reverted.</span></span> |
| <span data-ttu-id="1405b-189">Vrátit zpět</span><span class="sxs-lookup"><span data-stu-id="1405b-189">Reverted</span></span> |<span data-ttu-id="1405b-190">Hello doporučení se vrátila.</span><span class="sxs-lookup"><span data-stu-id="1405b-190">hello recommendation was reverted.</span></span> |

<span data-ttu-id="1405b-191">Kliknutím doporučení v procesu ze seznamu toosee hello další podrobnosti:</span><span class="sxs-lookup"><span data-stu-id="1405b-191">Click an in-process recommendation from hello list toosee more details:</span></span>

![Doporučené indexy](./media/sql-database-advisor-portal/operations.png)

### <a name="reverting-a-recommendation"></a><span data-ttu-id="1405b-193">Při vrácení doporučení</span><span class="sxs-lookup"><span data-stu-id="1405b-193">Reverting a recommendation</span></span>
<span data-ttu-id="1405b-194">Pokud jste použili hello výkonu doporučení tooapply hello doporučení (což znamená, že nebyl spuštěn ručně skriptu hello T-SQL) se automaticky vrátí ho Pokud najde hello výkonu dopad toobe záporné.</span><span class="sxs-lookup"><span data-stu-id="1405b-194">If you used hello performance recommendations tooapply hello recommendation (meaning you did not manually run hello T-SQL script) it will automatically revert it if it finds hello performance impact toobe negative.</span></span> <span data-ttu-id="1405b-195">Pokud z nějakého důvodu chcete jednoduše toorevert doporučení můžete provést následující hello:</span><span class="sxs-lookup"><span data-stu-id="1405b-195">If for any reason you simply want toorevert a recommendation you can do hello following:</span></span>

1. <span data-ttu-id="1405b-196">Vyberte úspěšně použité doporučení v hello **ladění historie** oblasti.</span><span class="sxs-lookup"><span data-stu-id="1405b-196">Select a successfully applied recommendation in hello **Tuning history** area.</span></span>
2. <span data-ttu-id="1405b-197">Klikněte na tlačítko **vrátit** na hello **podrobnosti o doporučení** okno.</span><span class="sxs-lookup"><span data-stu-id="1405b-197">Click **Revert** on hello **recommendation details** blade.</span></span>

![Doporučené indexy](./media/sql-database-advisor-portal/details.png)

## <a name="monitoring-performance-impact-of-index-recommendations"></a><span data-ttu-id="1405b-199">Monitorování vlivu na výkon doporučení indexu</span><span class="sxs-lookup"><span data-stu-id="1405b-199">Monitoring performance impact of index recommendations</span></span>
<span data-ttu-id="1405b-200">Po doporučení jsou úspěšně implementováno (v současné době operace indexu a Parametrizace pouze dotazy doporučení) můžete kliknout na **přehled dotazu** na tooopen okno Podrobnosti doporučení hello [dotazu Přehled o výkonu](sql-database-query-performance.md) a zobrazit hello vlivu na výkon nejčastějších dotazů.</span><span class="sxs-lookup"><span data-stu-id="1405b-200">After recommendations are successfully implemented (currently, index operations and parameterize queries recommendations only) you can click **Query Insights** on hello recommendation details blade tooopen [Query Performance Insights](sql-database-query-performance.md) and see hello performance impact of your top queries.</span></span>

![Dopad na výkon monitorování](./media/sql-database-advisor-portal/query-insights.png)

## <a name="summary"></a><span data-ttu-id="1405b-202">Souhrn</span><span class="sxs-lookup"><span data-stu-id="1405b-202">Summary</span></span>
<span data-ttu-id="1405b-203">Databáze SQL Azure poskytuje doporučení pro zlepšení výkonu databáze SQL.</span><span class="sxs-lookup"><span data-stu-id="1405b-203">Azure SQL Database provides recommendations for improving SQL database performance.</span></span> <span data-ttu-id="1405b-204">Tím, že poskytuje skriptů T-SQL, a také jednotlivé a plně automaticky, získat užitečné pomoc při optimalizaci vaši databázi a nakonec zlepšení výkonu dotazů.</span><span class="sxs-lookup"><span data-stu-id="1405b-204">By providing T-SQL scripts, as well as individual and fully-automatic, you get a helpful assistance in optimizing your database and ultimately improving query performance.</span></span>

## <a name="next-steps"></a><span data-ttu-id="1405b-205">Další kroky</span><span class="sxs-lookup"><span data-stu-id="1405b-205">Next steps</span></span>
<span data-ttu-id="1405b-206">Sledovat vaše doporučení a pokračovat tooapply je toorefine výkonu.</span><span class="sxs-lookup"><span data-stu-id="1405b-206">Monitor your recommendations and continue tooapply them toorefine performance.</span></span> <span data-ttu-id="1405b-207">Databázové úlohy jsou dynamické a průběžně změnu.</span><span class="sxs-lookup"><span data-stu-id="1405b-207">Database workloads are dynamic and change continuously.</span></span> <span data-ttu-id="1405b-208">Databáze SQL Azure bude i nadále toomonitor a poskytovat doporučení, které může zlepšit výkon vaší databáze.</span><span class="sxs-lookup"><span data-stu-id="1405b-208">Azure SQL Database will continue toomonitor and provide recommendations that can potentially improve your database's performance.</span></span> 

* <span data-ttu-id="1405b-209">V tématu [automatické ladění](sql-database-automatic-tuning.md) hello toolearn Další informace o automatické ladění ve službě Azure SQL Database.</span><span class="sxs-lookup"><span data-stu-id="1405b-209">See [Automatic tuning](sql-database-automatic-tuning.md) toolearn more about hello automatic tuning in Azure SQL Database.</span></span>
* <span data-ttu-id="1405b-210">V tématu [výkonu doporučení](sql-database-advisor.md) přehled o výkonu doporučení Azure SQL Database.</span><span class="sxs-lookup"><span data-stu-id="1405b-210">See [Performance recommendations](sql-database-advisor.md) for an overview of Azure SQL Database performance recommendations.</span></span>
* <span data-ttu-id="1405b-211">V tématu [Query Performance Insight](sql-database-query-performance.md) toolearn o zobrazení hello vlivu na výkon nejčastějších dotazů.</span><span class="sxs-lookup"><span data-stu-id="1405b-211">See [Query Performance Insights](sql-database-query-performance.md) toolearn about viewing hello performance impact of your top queries.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="1405b-212">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="1405b-212">Additional resources</span></span>
* [<span data-ttu-id="1405b-213">Úložiště dotazů</span><span class="sxs-lookup"><span data-stu-id="1405b-213">Query Store</span></span>](https://msdn.microsoft.com/library/dn817826.aspx)
* [<span data-ttu-id="1405b-214">VYTVOŘENÍ INDEXU</span><span class="sxs-lookup"><span data-stu-id="1405b-214">CREATE INDEX</span></span>](https://msdn.microsoft.com/library/ms188783.aspx)
* [<span data-ttu-id="1405b-215">Řízení přístupu na základě rolí</span><span class="sxs-lookup"><span data-stu-id="1405b-215">Role-based access control</span></span>](../active-directory/role-based-access-control-what-is.md)

