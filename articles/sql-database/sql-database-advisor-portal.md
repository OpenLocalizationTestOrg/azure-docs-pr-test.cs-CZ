---
title: "Použít výkonu doporučení – Azure SQL Database | Microsoft Docs"
description: "Portálu Azure můžete použít k vyhledání doporučení výkonu, které můžete optimalizovat výkon vaší databáze SQL Azure nebo opravte některé problém uvedený v vaše úlohy."
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
ms.openlocfilehash: 018afaa8b08bd001e55693390e80c8e2c4f33a30
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="find-and-apply-performance-recommendations"></a><span data-ttu-id="670fd-103">Najít a použít doporučení výkonu</span><span class="sxs-lookup"><span data-stu-id="670fd-103">Find and apply performance recommendations</span></span>

<span data-ttu-id="670fd-104">Portálu Azure můžete použít k vyhledání doporučení výkonu, které můžete optimalizovat výkon vaší databáze SQL Azure nebo opravte některé problém uvedený v vaše úlohy.</span><span class="sxs-lookup"><span data-stu-id="670fd-104">You can use the Azure portal to find performance recommendations that can optimize performance of your Azure SQL Database or to correct some issue identified in your workload.</span></span> <span data-ttu-id="670fd-105">**Výkon doporučení** stránka na portálu Azure umožňuje najít nejvyšší doporučení podle jejich potenciální vliv.</span><span class="sxs-lookup"><span data-stu-id="670fd-105">**Performance recommendation** page in Azure portal enables you to find the top recommendations based on their potential impact.</span></span> 

## <a name="viewing-recommendations"></a><span data-ttu-id="670fd-106">Zobrazení doporučení</span><span class="sxs-lookup"><span data-stu-id="670fd-106">Viewing recommendations</span></span>

<span data-ttu-id="670fd-107">Pokud chcete zobrazit a použít doporučení pro optimální výkon, je nutné správný [řízení přístupu na základě role](../active-directory/role-based-access-control-what-is.md) oprávnění v Azure.</span><span class="sxs-lookup"><span data-stu-id="670fd-107">To view and apply performance recommendations, you need the correct [role-based access control](../active-directory/role-based-access-control-what-is.md) permissions in Azure.</span></span> <span data-ttu-id="670fd-108">**Čtečka**, **Přispěvatel databází SQL** zobrazíte doporučení, jsou vyžadována oprávnění a **vlastníka**, **Přispěvatel databází SQL** se vyžadují oprávnění k provádět žádné akce; Vytvořit nebo vyřadit indexy a zrušit vytváření indexu.</span><span class="sxs-lookup"><span data-stu-id="670fd-108">**Reader**, **SQL DB Contributor** permissions are required to view recommendations, and **Owner**, **SQL DB Contributor** permissions are required to execute any actions; create or drop indexes and cancel index creation.</span></span>

<span data-ttu-id="670fd-109">Najít výkonu doporučení na portálu Azure pomocí následujících kroků:</span><span class="sxs-lookup"><span data-stu-id="670fd-109">Use the following steps to find performance recommendations on Azure portal:</span></span>

1. <span data-ttu-id="670fd-110">Přihlaste se k webu [Azure Portal](https://portal.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="670fd-110">Sign in to the [Azure portal](https://portal.azure.com/).</span></span>
2. <span data-ttu-id="670fd-111">Přejděte na **další služby** > **databází SQL**a vyberte svou databázi.</span><span class="sxs-lookup"><span data-stu-id="670fd-111">Go to **More services** > **SQL databases**, and select your database.</span></span>
3. <span data-ttu-id="670fd-112">Přejděte na **výkonu doporučení** zobrazíte doporučení k dispozici pro vybranou databázi.</span><span class="sxs-lookup"><span data-stu-id="670fd-112">Navigate to **Performance recommendation** to view available recommendations for the selected database.</span></span>

<span data-ttu-id="670fd-113">Výkon doporučení jsou shonw v tabulce podobně jako na následujícím obrázku:</span><span class="sxs-lookup"><span data-stu-id="670fd-113">Performance recommendations are shonw in the table similar to the one shown on the following figure:</span></span>

![Doporučení](./media/sql-database-advisor-portal/recommendations.png)

<span data-ttu-id="670fd-115">Doporučení jsou seřazené podle jejich potenciální dopad na výkon do následujících kategorií:</span><span class="sxs-lookup"><span data-stu-id="670fd-115">Recommendations are sorted by their potential impact on performance into the following categories:</span></span>

| <span data-ttu-id="670fd-116">Dopad</span><span class="sxs-lookup"><span data-stu-id="670fd-116">Impact</span></span> | <span data-ttu-id="670fd-117">Popis</span><span class="sxs-lookup"><span data-stu-id="670fd-117">Description</span></span> |
|:--- |:--- |
| <span data-ttu-id="670fd-118">Vysoký</span><span class="sxs-lookup"><span data-stu-id="670fd-118">High</span></span> |<span data-ttu-id="670fd-119">Vysoký dopad na doporučení by měl poskytovat nejvýraznější dopad na výkon.</span><span class="sxs-lookup"><span data-stu-id="670fd-119">High impact recommendations should provide the most significant performance impact.</span></span> |
| <span data-ttu-id="670fd-120">Střednědobé používání</span><span class="sxs-lookup"><span data-stu-id="670fd-120">Medium</span></span> |<span data-ttu-id="670fd-121">Střední dopad doporučení by měla zvýšit výkon, ale není podstatně.</span><span class="sxs-lookup"><span data-stu-id="670fd-121">Medium impact recommendations should improve performance, but not substantially.</span></span> |
| <span data-ttu-id="670fd-122">Nízký</span><span class="sxs-lookup"><span data-stu-id="670fd-122">Low</span></span> |<span data-ttu-id="670fd-123">Nízký dopad doporučení by měl poskytovat lepší výkon než bez, ale nemusí být výrazné vylepšení.</span><span class="sxs-lookup"><span data-stu-id="670fd-123">Low impact recommendations should provide better performance than without, but improvements might not be significant.</span></span> |


> [!NOTE]
> <span data-ttu-id="670fd-124">Databáze SQL Azure je potřeba sledovat aktivity alespoň za den za účelem zjištění několik doporučení.</span><span class="sxs-lookup"><span data-stu-id="670fd-124">Azure SQL Database needs to monitor activities at least for a day in order to identify some recommendations.</span></span> <span data-ttu-id="670fd-125">Databáze SQL Azure můžete snadněji optimalizovat konzistentní dotazu v případě vzorů, než je to možné na náhodných problematického shluky aktivity.</span><span class="sxs-lookup"><span data-stu-id="670fd-125">The Azure SQL Database can more easily optimize for consistent query patterns than it can for random spotty bursts of activity.</span></span> <span data-ttu-id="670fd-126">Pokud doporučení nejsou k dispozici, corrently **výkonu doporučení** stránka obsahuje zprávu s vysvětlením, proč.</span><span class="sxs-lookup"><span data-stu-id="670fd-126">If recommendations are not corrently available, the **Performance recommendation** page provides a message explaining why.</span></span>
> 

<span data-ttu-id="670fd-127">Můžete také zobrazit stav přehled činností.</span><span class="sxs-lookup"><span data-stu-id="670fd-127">You can also view the status of the historical operations.</span></span> <span data-ttu-id="670fd-128">Vyberte doporučení nebo stav zobrazíte další podrobnosti.</span><span class="sxs-lookup"><span data-stu-id="670fd-128">Select a recommendation or status to see  more details.</span></span>

<span data-ttu-id="670fd-129">Tady je příklad "Vytvořit index" doporučení na portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="670fd-129">Here is an example of "Create index" recommendation in the Azure portal.</span></span>

![Vytvoření indexu](./media/sql-database-advisor-portal/sql-database-performance-recommendation.png)

## <a name="applying-recommendations"></a><span data-ttu-id="670fd-131">Uplatňovat doporučení</span><span class="sxs-lookup"><span data-stu-id="670fd-131">Applying recommendations</span></span>
<span data-ttu-id="670fd-132">Databáze SQL Azure vám dává plnou kontrolu nad jak doporučení jsou povolit pomocí některé z následujících tří možností:</span><span class="sxs-lookup"><span data-stu-id="670fd-132">Azure SQL Database gives you full control over how recommendations are enabled using any of the following three options:</span></span> 

* <span data-ttu-id="670fd-133">Používat jednotlivé doporučení jeden v daném okamžiku.</span><span class="sxs-lookup"><span data-stu-id="670fd-133">Apply individual recommendations one at a time.</span></span>
* <span data-ttu-id="670fd-134">Povolte automatické ladění pro automatické použití doporučení.</span><span class="sxs-lookup"><span data-stu-id="670fd-134">Enable the Automatic tuning to automatically apply recommendations.</span></span>
* <span data-ttu-id="670fd-135">Pokud chcete doporučení implementovat ručně, spusťte doporučené skript T-SQL databáze.</span><span class="sxs-lookup"><span data-stu-id="670fd-135">To implement a recommendation manually, run the recommended T-SQL script against your database.</span></span>

<span data-ttu-id="670fd-136">Vyberte všechny doporučení zobrazit jeho podrobnosti a potom klikněte na **zobrazit skript** ke kontrole přesné údaje o tom, jak vytvořit doporučení.</span><span class="sxs-lookup"><span data-stu-id="670fd-136">Select any recommendation to view its details and then click **View script** to review the exact details of how the recommendation is created.</span></span>

<span data-ttu-id="670fd-137">Databáze zůstane online, zatímco doporučení platí – pomocí výkonu doporučení nebo automatické ladění nikdy převede do režimu offline databáze.</span><span class="sxs-lookup"><span data-stu-id="670fd-137">The database remains online while the recommendation is applied -- using performance recommendation or automatic tuning never takes a database offline.</span></span>

### <a name="apply-an-individual-recommendation"></a><span data-ttu-id="670fd-138">Jednotlivá doporučení použít</span><span class="sxs-lookup"><span data-stu-id="670fd-138">Apply an individual recommendation</span></span>
<span data-ttu-id="670fd-139">Můžete zkontrolovat a přijmout doporučení jeden najednou.</span><span class="sxs-lookup"><span data-stu-id="670fd-139">You can review and accept recommendations one at a time.</span></span>

1. <span data-ttu-id="670fd-140">Na **doporučení** okně vyberte doporučení.</span><span class="sxs-lookup"><span data-stu-id="670fd-140">On the **Recommendations** blade, select a recommendation.</span></span>
2. <span data-ttu-id="670fd-141">Na **podrobnosti** okně klikněte na tlačítko **použít** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="670fd-141">On the **Details** blade click **Apply** button.</span></span>
   
    ![použít doporučení](./media/sql-database-advisor-portal/apply.png)

<span data-ttu-id="670fd-143">Vybrané doporučení se použijí v databázi.</span><span class="sxs-lookup"><span data-stu-id="670fd-143">Selected recommendation will be applied on the database.</span></span>

### <a name="removing-recommendations-from-the-list"></a><span data-ttu-id="670fd-144">Odebrání doporučení ze seznamu</span><span class="sxs-lookup"><span data-stu-id="670fd-144">Removing recommendations from the list</span></span>
<span data-ttu-id="670fd-145">Pokud seznam doporučení obsahuje položky, které chcete odebrat ze seznamu, můžete zrušit doporučení:</span><span class="sxs-lookup"><span data-stu-id="670fd-145">If your list of recommendations contains items that you want to remove from the list, you can discard the recommendation:</span></span>

1. <span data-ttu-id="670fd-146">V seznamu vyberte doporučení **doporučení** otevřete podrobnosti.</span><span class="sxs-lookup"><span data-stu-id="670fd-146">Select a recommendation in the list of **Recommendations** to open the details.</span></span>
2. <span data-ttu-id="670fd-147">Klikněte na tlačítko **zahodit** na **podrobnosti** okno.</span><span class="sxs-lookup"><span data-stu-id="670fd-147">Click **Discard** on the **Details** blade.</span></span>

<span data-ttu-id="670fd-148">V případě potřeby můžete přidat vyřazené položky zpět do **doporučení** seznamu:</span><span class="sxs-lookup"><span data-stu-id="670fd-148">If desired, you can add discarded items back to the **Recommendations** list:</span></span>

1. <span data-ttu-id="670fd-149">Na **doporučení** okně klikněte na tlačítko **zobrazení zahozena**.</span><span class="sxs-lookup"><span data-stu-id="670fd-149">On the **Recommendations** blade click **View discarded**.</span></span>
2. <span data-ttu-id="670fd-150">Vyberte položku zrušených ze seznamu zobrazíte její podrobnosti.</span><span class="sxs-lookup"><span data-stu-id="670fd-150">Select a discarded item from the list to view its details.</span></span>
3. <span data-ttu-id="670fd-151">Volitelně klikněte na **vrátit zpět zahodit** přidat index zpět do hlavní seznam **doporučení**.</span><span class="sxs-lookup"><span data-stu-id="670fd-151">Optionally, click **Undo Discard** to add the index back to the main list of **Recommendations**.</span></span>


### <a name="enable-automatic-tuning"></a><span data-ttu-id="670fd-152">Povolení automatického ladění</span><span class="sxs-lookup"><span data-stu-id="670fd-152">Enable automatic tuning</span></span>
<span data-ttu-id="670fd-153">Můžete nastavit Azure SQL Database k implementaci doporučení automaticky.</span><span class="sxs-lookup"><span data-stu-id="670fd-153">You can set the Azure SQL Database to implement recommendations automatically.</span></span> <span data-ttu-id="670fd-154">Dostupná doporučení budou automaticky použity.</span><span class="sxs-lookup"><span data-stu-id="670fd-154">As recommendations become available they will automatically be applied.</span></span> <span data-ttu-id="670fd-155">Jako s všechna doporučení prostřednictvím služby spravovány, pokud je negativní dopad na výkon doporučení budou vráceny.</span><span class="sxs-lookup"><span data-stu-id="670fd-155">As with all recommendations managed by the service if the performance impact is negative the recommendation will be reverted.</span></span>

1. <span data-ttu-id="670fd-156">Na **doporučení** okně klikněte na tlačítko **automatické**:</span><span class="sxs-lookup"><span data-stu-id="670fd-156">On the **Recommendations** blade, click **Automate**:</span></span>
   
    ![Nastavení služby Advisor](./media/sql-database-advisor-portal/settings.png)
2. <span data-ttu-id="670fd-158">Vyberte akce automatizovat:</span><span class="sxs-lookup"><span data-stu-id="670fd-158">Select actions to automate:</span></span>
   
    ![Doporučené indexy](./media/sql-database-advisor-portal/automation.png)

### <a name="manually-run-the-recommended-t-sql-script"></a><span data-ttu-id="670fd-160">Ručně spustit doporučené skriptu T-SQL</span><span class="sxs-lookup"><span data-stu-id="670fd-160">Manually run the recommended T-SQL script</span></span>
<span data-ttu-id="670fd-161">Vyberte všechny doporučení a pak klikněte na tlačítko **zobrazit skript**.</span><span class="sxs-lookup"><span data-stu-id="670fd-161">Select any recommendation and then click **View script**.</span></span> <span data-ttu-id="670fd-162">Spusťte tento skript databázi doporučení použít ručně.</span><span class="sxs-lookup"><span data-stu-id="670fd-162">Run this script against your database to manually apply the recommendation.</span></span>

<span data-ttu-id="670fd-163">*Indexy, které jsou spouštěny ručně nejsou sledovat a ověřit pro vlivu na výkon službou* , doporučujeme sledovat tyto indexy po vytvoření ověřte se poskytují zvýšení výkonu a upravit nebo odstranit v případě potřeby.</span><span class="sxs-lookup"><span data-stu-id="670fd-163">*Indexes that are manually executed are not monitored and validated for performance impact by the service* so it is suggested that you monitor these indexes after creation to verify they provide performance gains and adjust or delete them if necessary.</span></span> <span data-ttu-id="670fd-164">Podrobnosti o vytváření indexů najdete v tématu [CREATE INDEX (Transact-SQL)](https://msdn.microsoft.com/library/ms188783.aspx).</span><span class="sxs-lookup"><span data-stu-id="670fd-164">For details about creating indexes, see [CREATE INDEX (Transact-SQL)](https://msdn.microsoft.com/library/ms188783.aspx).</span></span>

### <a name="canceling-recommendations"></a><span data-ttu-id="670fd-165">Zrušení doporučení</span><span class="sxs-lookup"><span data-stu-id="670fd-165">Canceling recommendations</span></span>
<span data-ttu-id="670fd-166">Doporučení, které jsou v **čekající**, **ověření**, nebo **úspěch** stav se dají zrušit.</span><span class="sxs-lookup"><span data-stu-id="670fd-166">Recommendations that are in a **Pending**, **Verifying**, or **Success** status can be canceled.</span></span> <span data-ttu-id="670fd-167">Doporučení se stavem **zpracování** nelze zrušit.</span><span class="sxs-lookup"><span data-stu-id="670fd-167">Recommendations with a status of **Executing** cannot be canceled.</span></span>

1. <span data-ttu-id="670fd-168">Vyberte doporučení v **ladění historie** oblasti otevřete **podrobnosti doporučení** okno.</span><span class="sxs-lookup"><span data-stu-id="670fd-168">Select a recommendation in the **Tuning History** area to open the **recommendations details** blade.</span></span>
2. <span data-ttu-id="670fd-169">Klikněte na tlačítko **zrušit** k přerušení proces použití doporučení.</span><span class="sxs-lookup"><span data-stu-id="670fd-169">Click **Cancel** to abort the process of applying the recommendation.</span></span>

## <a name="monitoring-operations"></a><span data-ttu-id="670fd-170">Monitorování operací</span><span class="sxs-lookup"><span data-stu-id="670fd-170">Monitoring operations</span></span>
<span data-ttu-id="670fd-171">Použití doporučeným nemusí dojít okamžitě.</span><span class="sxs-lookup"><span data-stu-id="670fd-171">Applying a recommendation might not happen instantaneously.</span></span> <span data-ttu-id="670fd-172">Portál obsahuje podrobné informace týkající se stavu doporučení.</span><span class="sxs-lookup"><span data-stu-id="670fd-172">The portal provides details regarding the status of recommendation.</span></span> <span data-ttu-id="670fd-173">Toto jsou možné stavy, které může být index:</span><span class="sxs-lookup"><span data-stu-id="670fd-173">The following are possible states that an index can be in:</span></span>

| <span data-ttu-id="670fd-174">Status</span><span class="sxs-lookup"><span data-stu-id="670fd-174">Status</span></span> | <span data-ttu-id="670fd-175">Popis</span><span class="sxs-lookup"><span data-stu-id="670fd-175">Description</span></span> |
|:--- |:--- |
| <span data-ttu-id="670fd-176">Čekající na vyřízení</span><span class="sxs-lookup"><span data-stu-id="670fd-176">Pending</span></span> |<span data-ttu-id="670fd-177">Platí doporučení, příkaz byl přijat a je naplánováno spuštění.</span><span class="sxs-lookup"><span data-stu-id="670fd-177">Apply recommendation command has been received and is scheduled for execution.</span></span> |
| <span data-ttu-id="670fd-178">Provádění</span><span class="sxs-lookup"><span data-stu-id="670fd-178">Executing</span></span> |<span data-ttu-id="670fd-179">Doporučuje se právě používá.</span><span class="sxs-lookup"><span data-stu-id="670fd-179">The recommendation is being applied.</span></span> |
| <span data-ttu-id="670fd-180">Ověření</span><span class="sxs-lookup"><span data-stu-id="670fd-180">Verifying</span></span> |<span data-ttu-id="670fd-181">Bylo úspěšně aplikováno doporučení a službu je měření výhody.</span><span class="sxs-lookup"><span data-stu-id="670fd-181">Recommendation was successfully applied and the service is measuring the benefits.</span></span> |
| <span data-ttu-id="670fd-182">Úspěch</span><span class="sxs-lookup"><span data-stu-id="670fd-182">Success</span></span> |<span data-ttu-id="670fd-183">Bylo úspěšně aplikováno doporučení a měří výhody.</span><span class="sxs-lookup"><span data-stu-id="670fd-183">Recommendation was successfully applied and benefits have been measured.</span></span> |
| <span data-ttu-id="670fd-184">Chyba</span><span class="sxs-lookup"><span data-stu-id="670fd-184">Error</span></span> |<span data-ttu-id="670fd-185">Při zavádění doporučení se stala chyba.</span><span class="sxs-lookup"><span data-stu-id="670fd-185">An error occurred during the process of applying the recommendation.</span></span> <span data-ttu-id="670fd-186">Může to být dočasný problém, nebo které by mohly mít schéma změnit do tabulky a skript již není platný.</span><span class="sxs-lookup"><span data-stu-id="670fd-186">This can be a transient issue, or possibly a schema change to the table and the script is no longer valid.</span></span> |
| <span data-ttu-id="670fd-187">Návrat</span><span class="sxs-lookup"><span data-stu-id="670fd-187">Reverting</span></span> |<span data-ttu-id="670fd-188">Doporučení bylo použito, ale je považována za nenáročných a automaticky zrušeny.</span><span class="sxs-lookup"><span data-stu-id="670fd-188">The recommendation was applied, but has been deemed non-performant and is being automatically reverted.</span></span> |
| <span data-ttu-id="670fd-189">Vrátit zpět</span><span class="sxs-lookup"><span data-stu-id="670fd-189">Reverted</span></span> |<span data-ttu-id="670fd-190">Doporučuje se vrátila.</span><span class="sxs-lookup"><span data-stu-id="670fd-190">The recommendation was reverted.</span></span> |

<span data-ttu-id="670fd-191">Kliknutím na doporučení v procesu v seznamu zobrazíte další podrobnosti:</span><span class="sxs-lookup"><span data-stu-id="670fd-191">Click an in-process recommendation from the list to see more details:</span></span>

![Doporučené indexy](./media/sql-database-advisor-portal/operations.png)

### <a name="reverting-a-recommendation"></a><span data-ttu-id="670fd-193">Při vrácení doporučení</span><span class="sxs-lookup"><span data-stu-id="670fd-193">Reverting a recommendation</span></span>
<span data-ttu-id="670fd-194">Pokud jste použili výkonu doporučení k doporučení použít (což znamená, že nebyl spuštěn ručně skriptu T-SQL) se automaticky vrátí ho Pokud najde negativní dopad na výkon.</span><span class="sxs-lookup"><span data-stu-id="670fd-194">If you used the performance recommendations to apply the recommendation (meaning you did not manually run the T-SQL script) it will automatically revert it if it finds the performance impact to be negative.</span></span> <span data-ttu-id="670fd-195">Pokud z nějakého důvodu, že chcete jednoduše obnovit doporučení můžete provést následující:</span><span class="sxs-lookup"><span data-stu-id="670fd-195">If for any reason you simply want to revert a recommendation you can do the following:</span></span>

1. <span data-ttu-id="670fd-196">Vyberte úspěšně použité doporučení v **ladění historie** oblasti.</span><span class="sxs-lookup"><span data-stu-id="670fd-196">Select a successfully applied recommendation in the **Tuning history** area.</span></span>
2. <span data-ttu-id="670fd-197">Klikněte na tlačítko **vrátit** na **podrobnosti o doporučení** okno.</span><span class="sxs-lookup"><span data-stu-id="670fd-197">Click **Revert** on the **recommendation details** blade.</span></span>

![Doporučené indexy](./media/sql-database-advisor-portal/details.png)

## <a name="monitoring-performance-impact-of-index-recommendations"></a><span data-ttu-id="670fd-199">Monitorování vlivu na výkon doporučení indexu</span><span class="sxs-lookup"><span data-stu-id="670fd-199">Monitoring performance impact of index recommendations</span></span>
<span data-ttu-id="670fd-200">Po doporučení jsou úspěšně implementováno (v současné době operace indexu a Parametrizace pouze dotazy doporučení) můžete kliknout na **přehled dotazu** v okně doporučení podrobnosti otevřete [dotazu Přehled o výkonu](sql-database-query-performance.md) a zobrazit dopad na výkon nejčastějších dotazů.</span><span class="sxs-lookup"><span data-stu-id="670fd-200">After recommendations are successfully implemented (currently, index operations and parameterize queries recommendations only) you can click **Query Insights** on the recommendation details blade to open [Query Performance Insights](sql-database-query-performance.md) and see the performance impact of your top queries.</span></span>

![Dopad na výkon monitorování](./media/sql-database-advisor-portal/query-insights.png)

## <a name="summary"></a><span data-ttu-id="670fd-202">Souhrn</span><span class="sxs-lookup"><span data-stu-id="670fd-202">Summary</span></span>
<span data-ttu-id="670fd-203">Databáze SQL Azure poskytuje doporučení pro zlepšení výkonu databáze SQL.</span><span class="sxs-lookup"><span data-stu-id="670fd-203">Azure SQL Database provides recommendations for improving SQL database performance.</span></span> <span data-ttu-id="670fd-204">Tím, že poskytuje skriptů T-SQL, a také jednotlivé a plně automaticky, získat užitečné pomoc při optimalizaci vaši databázi a nakonec zlepšení výkonu dotazů.</span><span class="sxs-lookup"><span data-stu-id="670fd-204">By providing T-SQL scripts, as well as individual and fully-automatic, you get a helpful assistance in optimizing your database and ultimately improving query performance.</span></span>

## <a name="next-steps"></a><span data-ttu-id="670fd-205">Další kroky</span><span class="sxs-lookup"><span data-stu-id="670fd-205">Next steps</span></span>
<span data-ttu-id="670fd-206">Sledovat vaše doporučení a pokračuje v používání jejich Upřesnit výkonu.</span><span class="sxs-lookup"><span data-stu-id="670fd-206">Monitor your recommendations and continue to apply them to refine performance.</span></span> <span data-ttu-id="670fd-207">Databázové úlohy jsou dynamické a průběžně změnu.</span><span class="sxs-lookup"><span data-stu-id="670fd-207">Database workloads are dynamic and change continuously.</span></span> <span data-ttu-id="670fd-208">Databáze SQL Azure bude sledovat a poskytovat doporučení, které může zlepšit výkon vaší databáze.</span><span class="sxs-lookup"><span data-stu-id="670fd-208">Azure SQL Database will continue to monitor and provide recommendations that can potentially improve your database's performance.</span></span> 

* <span data-ttu-id="670fd-209">V tématu [automatické ladění](sql-database-automatic-tuning.md) Další informace o automatické ladění ve službě Azure SQL Database.</span><span class="sxs-lookup"><span data-stu-id="670fd-209">See [Automatic tuning](sql-database-automatic-tuning.md) to learn more about the automatic tuning in Azure SQL Database.</span></span>
* <span data-ttu-id="670fd-210">V tématu [výkonu doporučení](sql-database-advisor.md) přehled o výkonu doporučení Azure SQL Database.</span><span class="sxs-lookup"><span data-stu-id="670fd-210">See [Performance recommendations](sql-database-advisor.md) for an overview of Azure SQL Database performance recommendations.</span></span>
* <span data-ttu-id="670fd-211">V tématu [Query Performance Insight](sql-database-query-performance.md) Další informace o zobrazení dopad na výkon nejčastějších dotazů.</span><span class="sxs-lookup"><span data-stu-id="670fd-211">See [Query Performance Insights](sql-database-query-performance.md) to learn about viewing the performance impact of your top queries.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="670fd-212">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="670fd-212">Additional resources</span></span>
* [<span data-ttu-id="670fd-213">Úložiště dotazů</span><span class="sxs-lookup"><span data-stu-id="670fd-213">Query Store</span></span>](https://msdn.microsoft.com/library/dn817826.aspx)
* [<span data-ttu-id="670fd-214">VYTVOŘENÍ INDEXU</span><span class="sxs-lookup"><span data-stu-id="670fd-214">CREATE INDEX</span></span>](https://msdn.microsoft.com/library/ms188783.aspx)
* [<span data-ttu-id="670fd-215">Řízení přístupu na základě rolí</span><span class="sxs-lookup"><span data-stu-id="670fd-215">Role-based access control</span></span>](../active-directory/role-based-access-control-what-is.md)

