---
title: "Zobrazit protokoly aktivita Azure k monitorování zdrojů | Microsoft Docs"
description: "Použijte protokoly aktivity zkontrolujte akcemi uživatelů a chyby. Zobrazuje portálu prostředí Azure PowerShell, rozhraní příkazového řádku Azure a REST."
services: azure-resource-manager
documentationcenter: 
author: tfitzmac
manager: timlt
editor: tysonn
ms.assetid: fcdb3125-13ce-4c3b-9087-f514c5e41e73
ms.service: azure-resource-manager
ms.workload: multiple
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/09/2017
ms.author: tomfitz
ms.openlocfilehash: 9f90bc80c146c6c2da04aacbc110f7d389c0baa2
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="view-activity-logs-to-audit-actions-on-resources"></a><span data-ttu-id="38bdd-104">Zobrazit protokoly aktivity akce u prostředků</span><span class="sxs-lookup"><span data-stu-id="38bdd-104">View activity logs to audit actions on resources</span></span>
<span data-ttu-id="38bdd-105">Prostřednictvím protokolů činnosti můžete určit:</span><span class="sxs-lookup"><span data-stu-id="38bdd-105">Through activity logs, you can determine:</span></span>

* <span data-ttu-id="38bdd-106">jaké operace provedené na prostředky v rámci vašeho předplatného</span><span class="sxs-lookup"><span data-stu-id="38bdd-106">what operations were taken on the resources in your subscription</span></span>
* <span data-ttu-id="38bdd-107">Kdo inicioval operaci (i když operations iniciovaná back-end službu nevrátí uživatele jako volající)</span><span class="sxs-lookup"><span data-stu-id="38bdd-107">who initiated the operation (although operations initiated by a backend service do not return a user as the caller)</span></span>
* <span data-ttu-id="38bdd-108">Při operaci došlo k chybě</span><span class="sxs-lookup"><span data-stu-id="38bdd-108">when the operation occurred</span></span>
* <span data-ttu-id="38bdd-109">Stav operace</span><span class="sxs-lookup"><span data-stu-id="38bdd-109">the status of the operation</span></span>
* <span data-ttu-id="38bdd-110">Hodnoty další vlastnosti, které vám můžou pomoct zkoumání operaci</span><span class="sxs-lookup"><span data-stu-id="38bdd-110">the values of other properties that might help you research the operation</span></span>

[!INCLUDE [resource-manager-audit-limitations](../../includes/resource-manager-audit-limitations.md)]

<span data-ttu-id="38bdd-111">Můžete načíst informace z protokolů aktivity prostřednictvím portálu, prostředí PowerShell, rozhraní příkazového řádku Azure, rozhraní API pro přehledy REST, nebo [knihovny .NET Insights](https://www.nuget.org/packages/Microsoft.Azure.Insights/).</span><span class="sxs-lookup"><span data-stu-id="38bdd-111">You can retrieve information from the activity logs through the portal, PowerShell, Azure CLI, Insights REST API, or [Insights .NET Library](https://www.nuget.org/packages/Microsoft.Azure.Insights/).</span></span>

## <a name="portal"></a><span data-ttu-id="38bdd-112">Portál</span><span class="sxs-lookup"><span data-stu-id="38bdd-112">Portal</span></span>
1. <span data-ttu-id="38bdd-113">Chcete-li zobrazit protokoly aktivity přes portál, vyberte **monitorování**.</span><span class="sxs-lookup"><span data-stu-id="38bdd-113">To view the activity logs through the portal, select **Monitor**.</span></span>
   
    ![Vyberte protokoly aktivity](./media/resource-group-audit/select-monitor.png)

   <span data-ttu-id="38bdd-115">Nebo pokud chcete automaticky filtrovat protokol aktivit pro konkrétní prostředek nebo skupina prostředků, vyberte **protokol aktivit** z tohoto okna prostředků.</span><span class="sxs-lookup"><span data-stu-id="38bdd-115">Or, to automatically filter the activity log for a particular resource or resource group, select **Activity log** from that resource blade.</span></span> <span data-ttu-id="38bdd-116">Všimněte si, že je protokol aktivit automaticky filtrovaná podle vybraného prostředku.</span><span class="sxs-lookup"><span data-stu-id="38bdd-116">Notice that the activity log is automatically filtered by the selected resource.</span></span>
   
    ![Filtrovat podle prostředků](./media/resource-group-audit/filtered-by-resource.png)
2. <span data-ttu-id="38bdd-118">V **protokol aktivit** okně se zobrazí souhrn posledních operací.</span><span class="sxs-lookup"><span data-stu-id="38bdd-118">In the **Activity Log** blade, you see a summary of recent operations.</span></span>
   
    ![Zobrazit akce](./media/resource-group-audit/audit-summary.png)
3. <span data-ttu-id="38bdd-120">Pokud chcete omezit počet operací zobrazí, vyberte jiné podmínky.</span><span class="sxs-lookup"><span data-stu-id="38bdd-120">To restrict the number of operations displayed, select different conditions.</span></span> <span data-ttu-id="38bdd-121">Například na následujícím obrázku **časový interval** a **událostí iniciovaná** pole změnit tak, aby zobrazení akce prováděné konkrétního uživatele nebo aplikace pro poslední měsíc.</span><span class="sxs-lookup"><span data-stu-id="38bdd-121">For example, the following image shows the **Timespan** and **Event initiated by** fields changed to view the actions taken by a particular user or application for the past month.</span></span> <span data-ttu-id="38bdd-122">Vyberte **použít** pro zobrazení výsledků dotazu.</span><span class="sxs-lookup"><span data-stu-id="38bdd-122">Select **Apply** to view the results of your query.</span></span>
   
    ![Nastavení možností filtru](./media/resource-group-audit/set-filter.png)

4. <span data-ttu-id="38bdd-124">Pokud potřebujete spusťte dotaz znovu později, vyberte **Uložit** a zadejte název dotazu.</span><span class="sxs-lookup"><span data-stu-id="38bdd-124">If you need to run the query again later, select **Save** and give the query a name.</span></span>
   
    ![uložení dotazu](./media/resource-group-audit/save-query.png)
5. <span data-ttu-id="38bdd-126">Chcete-li rychle spuštění dotazu, můžete vybrat jeden z předdefinovaných dotazů, například selhání nasazení.</span><span class="sxs-lookup"><span data-stu-id="38bdd-126">To quickly run a query, you can select one of the built-in queries, such as failed deployments.</span></span>

    ![Vybrat dotaz](./media/resource-group-audit/select-quick-query.png)

   <span data-ttu-id="38bdd-128">Vybraný dotaz automaticky nastaví hodnoty požadované filtru.</span><span class="sxs-lookup"><span data-stu-id="38bdd-128">The selected query automatically sets the required filter values.</span></span>

    ![Zobrazit chyby nasazení](./media/resource-group-audit/view-failed-deployment.png)   

6. <span data-ttu-id="38bdd-130">Vyberte jednu z operace se zobrazí souhrn události.</span><span class="sxs-lookup"><span data-stu-id="38bdd-130">Select one of the operations to see a summary of the event.</span></span>

    ![Operace zobrazení](./media/resource-group-audit/view-operation.png)  

## <a name="powershell"></a><span data-ttu-id="38bdd-132">PowerShell</span><span class="sxs-lookup"><span data-stu-id="38bdd-132">PowerShell</span></span>
1. <span data-ttu-id="38bdd-133">Chcete-li načíst položky protokolu, spusťte **Get-AzureRmLog** příkaz.</span><span class="sxs-lookup"><span data-stu-id="38bdd-133">To retrieve log entries, run the **Get-AzureRmLog** command.</span></span> <span data-ttu-id="38bdd-134">Můžete zadat další parametry pro filtrování seznamu položek.</span><span class="sxs-lookup"><span data-stu-id="38bdd-134">You provide additional parameters to filter the list of entries.</span></span> <span data-ttu-id="38bdd-135">Pokud nezadáte počáteční a koncový čas, vrátí se položky za poslední hodinu.</span><span class="sxs-lookup"><span data-stu-id="38bdd-135">If you do not specify a start and end time, entries for the last hour are returned.</span></span> <span data-ttu-id="38bdd-136">Chcete-li například získat operace pro skupinu prostředků během poslední hodiny spustit:</span><span class="sxs-lookup"><span data-stu-id="38bdd-136">For example, to retrieve the operations for a resource group during the past hour run:</span></span>

  ```powershell
  Get-AzureRmLog -ResourceGroup ExampleGroup
  ```
   
    <span data-ttu-id="38bdd-137">Následující příklad ukazuje, jak používat protokol aktivit k operacím research prováděné během zadané doby.</span><span class="sxs-lookup"><span data-stu-id="38bdd-137">The following example shows how to use the activity log to research operations taken during a specified time.</span></span> <span data-ttu-id="38bdd-138">Počáteční a koncové datum nejsou zadány ve formátu data.</span><span class="sxs-lookup"><span data-stu-id="38bdd-138">The start and end dates are specified in a date format.</span></span>

  ```powershell
  Get-AzureRmLog -ResourceGroup ExampleGroup -StartTime 2015-08-28T06:00 -EndTime 2015-09-10T06:00
  ```

    <span data-ttu-id="38bdd-139">Nebo můžete použít funkce datum zadat rozsah dat, jako je například posledních 14 dní.</span><span class="sxs-lookup"><span data-stu-id="38bdd-139">Or, you can use date functions to specify the date range, such as the last 14 days.</span></span>
   
  ```powershell 
  Get-AzureRmLog -ResourceGroup ExampleGroup -StartTime (Get-Date).AddDays(-14)
  ```

2. <span data-ttu-id="38bdd-140">V závislosti na čas spuštění, který zadáte může vrátit předchozí příkazy dlouhý seznam operací pro skupinu prostředků.</span><span class="sxs-lookup"><span data-stu-id="38bdd-140">Depending on the start time you specify, the previous commands can return a long list of operations for the resource group.</span></span> <span data-ttu-id="38bdd-141">Můžete filtrovat výsledky pro co hledáte tím, že poskytuje kritéria vyhledávání.</span><span class="sxs-lookup"><span data-stu-id="38bdd-141">You can filter the results for what you are looking for by providing search criteria.</span></span> <span data-ttu-id="38bdd-142">Například pokud chcete prozkoumat, jak byla zastavena webovou aplikaci, můžete spustit následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="38bdd-142">For example, if you are trying to research how a web app was stopped, you could run the following command:</span></span>

  ```powershell
  Get-AzureRmLog -ResourceGroup ExampleGroup -StartTime (Get-Date).AddDays(-14) | Where-Object OperationName -eq Microsoft.Web/sites/stop/action
  ```

    <span data-ttu-id="38bdd-143">V tomto příkladu zobrazující se provádí akce zastavení someone@contoso.com.</span><span class="sxs-lookup"><span data-stu-id="38bdd-143">Which for this example shows that a stop action was performed by someone@contoso.com.</span></span> 

  ```powershell 
  Authorization     :
  Scope     : /subscriptions/xxxxx/resourcegroups/ExampleGroup/providers/Microsoft.Web/sites/ExampleSite
  Action    : Microsoft.Web/sites/stop/action
  Role      : Subscription Admin
  Condition :
  Caller            : someone@contoso.com
  CorrelationId     : 84beae59-92aa-4662-a6fc-b6fecc0ff8da
  EventSource       : Administrative
  EventTimestamp    : 8/28/2015 4:08:18 PM
  OperationName     : Microsoft.Web/sites/stop/action
  ResourceGroupName : ExampleGroup
  ResourceId        : /subscriptions/xxxxx/resourcegroups/ExampleGroup/providers/Microsoft.Web/sites/ExampleSite
  Status            : Succeeded
  SubscriptionId    : xxxxx
  SubStatus         : OK
  ```

3. <span data-ttu-id="38bdd-144">Můžete vyhledat akce prováděné určitého uživatele, i pro skupinu prostředků, která již existuje.</span><span class="sxs-lookup"><span data-stu-id="38bdd-144">You can look up the actions taken by a particular user, even for a resource group that no longer exists.</span></span>

  ```powershell 
  Get-AzureRmLog -ResourceGroup deletedgroup -StartTime (Get-Date).AddDays(-14) -Caller someone@contoso.com
  ```

4. <span data-ttu-id="38bdd-145">Můžete filtrovat pro operace se nezdařila.</span><span class="sxs-lookup"><span data-stu-id="38bdd-145">You can filter for failed operations.</span></span>

  ```powershell
  Get-AzureRmLog -ResourceGroup ExampleGroup -Status Failed
  ```

5. <span data-ttu-id="38bdd-146">Můžete se zaměřit na jednu chybu pohledem na stavové zprávy pro tuto položku.</span><span class="sxs-lookup"><span data-stu-id="38bdd-146">You can focus on one error by looking at the status message for that entry.</span></span>
   
        ((Get-AzureRmLog -Status Failed -ResourceGroup ExampleGroup -DetailedOutput).Properties[1].Content["statusMessage"] | ConvertFrom-Json).error
   
    <span data-ttu-id="38bdd-147">Která vrací:</span><span class="sxs-lookup"><span data-stu-id="38bdd-147">Which returns:</span></span>
   
        code           message                                                                        
        ----           -------                                                                        
        DnsRecordInUse DNS record dns.westus.cloudapp.azure.com is already used by another public IP. 


## <a name="azure-cli"></a><span data-ttu-id="38bdd-148">Azure CLI</span><span class="sxs-lookup"><span data-stu-id="38bdd-148">Azure CLI</span></span>
* <span data-ttu-id="38bdd-149">Chcete-li načíst položky protokolu, spusťte **zobrazit skupiny azure protokolu** příkaz.</span><span class="sxs-lookup"><span data-stu-id="38bdd-149">To retrieve log entries, you run the **azure group log show** command.</span></span>

  ```azurecli
  azure group log show ExampleGroup --json
  ```


## <a name="rest-api"></a><span data-ttu-id="38bdd-150">REST API</span><span class="sxs-lookup"><span data-stu-id="38bdd-150">REST API</span></span>
<span data-ttu-id="38bdd-151">Operace REST pro práci s protokolu aktivit jsou součástí [rozhraní REST API pro přehledy](https://msdn.microsoft.com/library/azure/dn931943.aspx).</span><span class="sxs-lookup"><span data-stu-id="38bdd-151">The REST operations for working with the activity log are part of the [Insights REST API](https://msdn.microsoft.com/library/azure/dn931943.aspx).</span></span> <span data-ttu-id="38bdd-152">Načtení aktivity protokolu události, najdete v části [seznam událostí správy v předplatném](https://msdn.microsoft.com/library/azure/dn931934.aspx).</span><span class="sxs-lookup"><span data-stu-id="38bdd-152">To retrieve activity log events, see [List the management events in a subscription](https://msdn.microsoft.com/library/azure/dn931934.aspx).</span></span>

## <a name="next-steps"></a><span data-ttu-id="38bdd-153">Další kroky</span><span class="sxs-lookup"><span data-stu-id="38bdd-153">Next steps</span></span>
* <span data-ttu-id="38bdd-154">Azure protokoly aktivity s Power BI můžete použít k získání lepší přehled o akcích v rámci vašeho předplatného.</span><span class="sxs-lookup"><span data-stu-id="38bdd-154">Azure Activity logs can be used with Power BI to gain greater insights about the actions in your subscription.</span></span> <span data-ttu-id="38bdd-155">V tématu [zobrazení a analýza protokolů Azure aktivity v Power BI a další](https://azure.microsoft.com/blog/analyze-azure-audit-logs-in-powerbi-more/).</span><span class="sxs-lookup"><span data-stu-id="38bdd-155">See [View and analyze Azure Activity Logs in Power BI and more](https://azure.microsoft.com/blog/analyze-azure-audit-logs-in-powerbi-more/).</span></span>
* <span data-ttu-id="38bdd-156">Další informace o nastavení zásad zabezpečení najdete v tématu [řízení přístupu na základě Role v Azure](../active-directory/role-based-access-control-configure.md).</span><span class="sxs-lookup"><span data-stu-id="38bdd-156">To learn about setting security policies, see [Azure Role-based Access Control](../active-directory/role-based-access-control-configure.md).</span></span>
* <span data-ttu-id="38bdd-157">Další informace o příkazy pro zobrazení operace nasazení najdete v tématu [zobrazit operace nasazení](resource-manager-deployment-operations.md).</span><span class="sxs-lookup"><span data-stu-id="38bdd-157">To learn about the commands for viewing deployment operations, see [View deployment operations](resource-manager-deployment-operations.md).</span></span>
* <span data-ttu-id="38bdd-158">Informace o tom, aby se zabránilo odstranění prostředku pro všechny uživatele, najdete v části [zamknutí prostředků pomocí Azure Resource Manageru](resource-group-lock-resources.md).</span><span class="sxs-lookup"><span data-stu-id="38bdd-158">To learn how to prevent deletions on a resource for all users, see [Lock resources with Azure Resource Manager](resource-group-lock-resources.md).</span></span>

