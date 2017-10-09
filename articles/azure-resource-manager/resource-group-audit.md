---
title: "aaaView aktivita Azure protokoly toomonitor prostředky | Microsoft Docs"
description: "Použití hello aktivity protokoly tooreview uživatele akcí a chyby. Zobrazuje portálu prostředí Azure PowerShell, rozhraní příkazového řádku Azure a REST."
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
ms.openlocfilehash: 8430ed2a9c1dfe5f13423a55d358e590b0facb22
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="view-activity-logs-tooaudit-actions-on-resources"></a><span data-ttu-id="75e3f-104">Zobrazit aktivitu protokoluje akce tooaudit na prostředky</span><span class="sxs-lookup"><span data-stu-id="75e3f-104">View activity logs tooaudit actions on resources</span></span>
<span data-ttu-id="75e3f-105">Prostřednictvím protokolů činnosti můžete určit:</span><span class="sxs-lookup"><span data-stu-id="75e3f-105">Through activity logs, you can determine:</span></span>

* <span data-ttu-id="75e3f-106">jaké operace provedené na hello prostředky ve vašem předplatném</span><span class="sxs-lookup"><span data-stu-id="75e3f-106">what operations were taken on hello resources in your subscription</span></span>
* <span data-ttu-id="75e3f-107">Kdo inicioval hello operace (i když operations iniciovaná back-end službu nevrátí uživatele jako volající hello)</span><span class="sxs-lookup"><span data-stu-id="75e3f-107">who initiated hello operation (although operations initiated by a backend service do not return a user as hello caller)</span></span>
* <span data-ttu-id="75e3f-108">Pokud došlo k operaci hello</span><span class="sxs-lookup"><span data-stu-id="75e3f-108">when hello operation occurred</span></span>
* <span data-ttu-id="75e3f-109">Hello stav operace hello</span><span class="sxs-lookup"><span data-stu-id="75e3f-109">hello status of hello operation</span></span>
* <span data-ttu-id="75e3f-110">Hello hodnotách jiných vlastností, které vám můžou pomoct zkoumání operaci hello</span><span class="sxs-lookup"><span data-stu-id="75e3f-110">hello values of other properties that might help you research hello operation</span></span>

[!INCLUDE [resource-manager-audit-limitations](../../includes/resource-manager-audit-limitations.md)]

<span data-ttu-id="75e3f-111">Mohou načítat informace z protokolů činnosti hello prostřednictvím portálu hello prostředí PowerShell, rozhraní příkazového řádku Azure, rozhraní API pro přehledy REST, nebo [knihovny .NET Insights](https://www.nuget.org/packages/Microsoft.Azure.Insights/).</span><span class="sxs-lookup"><span data-stu-id="75e3f-111">You can retrieve information from hello activity logs through hello portal, PowerShell, Azure CLI, Insights REST API, or [Insights .NET Library](https://www.nuget.org/packages/Microsoft.Azure.Insights/).</span></span>

## <a name="portal"></a><span data-ttu-id="75e3f-112">Portál</span><span class="sxs-lookup"><span data-stu-id="75e3f-112">Portal</span></span>
1. <span data-ttu-id="75e3f-113">Vyberte protokoly aktivity hello tooview prostřednictvím portálu hello **monitorování**.</span><span class="sxs-lookup"><span data-stu-id="75e3f-113">tooview hello activity logs through hello portal, select **Monitor**.</span></span>
   
    ![Vyberte protokoly aktivity](./media/resource-group-audit/select-monitor.png)

   <span data-ttu-id="75e3f-115">Nebo tooautomatically filtru hello protokol aktivit pro konkrétní prostředek nebo skupina prostředků, vyberte **protokol aktivit** z tohoto okna prostředků.</span><span class="sxs-lookup"><span data-stu-id="75e3f-115">Or, tooautomatically filter hello activity log for a particular resource or resource group, select **Activity log** from that resource blade.</span></span> <span data-ttu-id="75e3f-116">Všimněte si, že protokol aktivit hello se automaticky filtruje hello vybraný prostředek.</span><span class="sxs-lookup"><span data-stu-id="75e3f-116">Notice that hello activity log is automatically filtered by hello selected resource.</span></span>
   
    ![Filtrovat podle prostředků](./media/resource-group-audit/filtered-by-resource.png)
2. <span data-ttu-id="75e3f-118">V hello **protokol aktivit** okně se zobrazí souhrn posledních operací.</span><span class="sxs-lookup"><span data-stu-id="75e3f-118">In hello **Activity Log** blade, you see a summary of recent operations.</span></span>
   
    ![Zobrazit akce](./media/resource-group-audit/audit-summary.png)
3. <span data-ttu-id="75e3f-120">toorestrict hello počet operací se zobrazí, vyberte jiné podmínky.</span><span class="sxs-lookup"><span data-stu-id="75e3f-120">toorestrict hello number of operations displayed, select different conditions.</span></span> <span data-ttu-id="75e3f-121">Například hello následující obrázek ukazuje hello **časový interval** a **událostí iniciovaná** pole změněna tooview hello akce provedené konkrétní uživatele nebo aplikace pro hello poslední měsíc.</span><span class="sxs-lookup"><span data-stu-id="75e3f-121">For example, hello following image shows hello **Timespan** and **Event initiated by** fields changed tooview hello actions taken by a particular user or application for hello past month.</span></span> <span data-ttu-id="75e3f-122">Vyberte **použít** tooview hello výsledky dotazu.</span><span class="sxs-lookup"><span data-stu-id="75e3f-122">Select **Apply** tooview hello results of your query.</span></span>
   
    ![Nastavení možností filtru](./media/resource-group-audit/set-filter.png)

4. <span data-ttu-id="75e3f-124">Pokud později potřebujete toorun hello dotazu, vyberte **Uložit** a pojmenujte hello dotazu.</span><span class="sxs-lookup"><span data-stu-id="75e3f-124">If you need toorun hello query again later, select **Save** and give hello query a name.</span></span>
   
    ![uložení dotazu](./media/resource-group-audit/save-query.png)
5. <span data-ttu-id="75e3f-126">tooquickly spuštění dotazu, můžete vybrat jeden z předdefinovaných hello dotazů, například selhání nasazení.</span><span class="sxs-lookup"><span data-stu-id="75e3f-126">tooquickly run a query, you can select one of hello built-in queries, such as failed deployments.</span></span>

    ![Vybrat dotaz](./media/resource-group-audit/select-quick-query.png)

   <span data-ttu-id="75e3f-128">Vybraný dotaz Hello automaticky nastaví hello požadované hodnoty filtru.</span><span class="sxs-lookup"><span data-stu-id="75e3f-128">hello selected query automatically sets hello required filter values.</span></span>

    ![Zobrazit chyby nasazení](./media/resource-group-audit/view-failed-deployment.png)   

6. <span data-ttu-id="75e3f-130">Vyberte jednu z hello operations toosee souhrn události hello.</span><span class="sxs-lookup"><span data-stu-id="75e3f-130">Select one of hello operations toosee a summary of hello event.</span></span>

    ![Operace zobrazení](./media/resource-group-audit/view-operation.png)  

## <a name="powershell"></a><span data-ttu-id="75e3f-132">PowerShell</span><span class="sxs-lookup"><span data-stu-id="75e3f-132">PowerShell</span></span>
1. <span data-ttu-id="75e3f-133">položky protokolu tooretrieve, spusťte hello **Get-AzureRmLog** příkaz.</span><span class="sxs-lookup"><span data-stu-id="75e3f-133">tooretrieve log entries, run hello **Get-AzureRmLog** command.</span></span> <span data-ttu-id="75e3f-134">Je zadat další parametry toofilter hello seznamu položek.</span><span class="sxs-lookup"><span data-stu-id="75e3f-134">You provide additional parameters toofilter hello list of entries.</span></span> <span data-ttu-id="75e3f-135">Pokud nezadáte počáteční a koncový čas, vrátí se záznamy pro hello poslední hodinu.</span><span class="sxs-lookup"><span data-stu-id="75e3f-135">If you do not specify a start and end time, entries for hello last hour are returned.</span></span> <span data-ttu-id="75e3f-136">Například spusťte tooretrieve hello operace pro skupinu prostředků během poslední hodiny hello:</span><span class="sxs-lookup"><span data-stu-id="75e3f-136">For example, tooretrieve hello operations for a resource group during hello past hour run:</span></span>

  ```powershell
  Get-AzureRmLog -ResourceGroup ExampleGroup
  ```
   
    <span data-ttu-id="75e3f-137">Hello následující příklad ukazuje, jak toouse hello aktivity protokolu tooresearch operace prováděné během zadané doby.</span><span class="sxs-lookup"><span data-stu-id="75e3f-137">hello following example shows how toouse hello activity log tooresearch operations taken during a specified time.</span></span> <span data-ttu-id="75e3f-138">Hello počátečním a koncovým datem nejsou zadány ve formátu data.</span><span class="sxs-lookup"><span data-stu-id="75e3f-138">hello start and end dates are specified in a date format.</span></span>

  ```powershell
  Get-AzureRmLog -ResourceGroup ExampleGroup -StartTime 2015-08-28T06:00 -EndTime 2015-09-10T06:00
  ```

    <span data-ttu-id="75e3f-139">Nebo můžete použít funkce toospecify hello datum rozsah dat, jako je například hello posledních 14 dní.</span><span class="sxs-lookup"><span data-stu-id="75e3f-139">Or, you can use date functions toospecify hello date range, such as hello last 14 days.</span></span>
   
  ```powershell 
  Get-AzureRmLog -ResourceGroup ExampleGroup -StartTime (Get-Date).AddDays(-14)
  ```

2. <span data-ttu-id="75e3f-140">V závislosti na hello čas zahájení, který zadáte může vrátit předchozí příkazy hello dlouhý seznam operací pro skupinu prostředků hello.</span><span class="sxs-lookup"><span data-stu-id="75e3f-140">Depending on hello start time you specify, hello previous commands can return a long list of operations for hello resource group.</span></span> <span data-ttu-id="75e3f-141">Můžete filtrovat výsledky hello co hledáte tím, že poskytuje kritéria vyhledávání.</span><span class="sxs-lookup"><span data-stu-id="75e3f-141">You can filter hello results for what you are looking for by providing search criteria.</span></span> <span data-ttu-id="75e3f-142">Například pokud se pokoušíte tooresearch jak webové aplikace byla zastavena, může spustit hello následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="75e3f-142">For example, if you are trying tooresearch how a web app was stopped, you could run hello following command:</span></span>

  ```powershell
  Get-AzureRmLog -ResourceGroup ExampleGroup -StartTime (Get-Date).AddDays(-14) | Where-Object OperationName -eq Microsoft.Web/sites/stop/action
  ```

    <span data-ttu-id="75e3f-143">V tomto příkladu zobrazující se provádí akce zastavení someone@contoso.com.</span><span class="sxs-lookup"><span data-stu-id="75e3f-143">Which for this example shows that a stop action was performed by someone@contoso.com.</span></span> 

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

3. <span data-ttu-id="75e3f-144">Můžete vyhledat hello akce prováděné určitého uživatele, i pro skupinu prostředků, která již existuje.</span><span class="sxs-lookup"><span data-stu-id="75e3f-144">You can look up hello actions taken by a particular user, even for a resource group that no longer exists.</span></span>

  ```powershell 
  Get-AzureRmLog -ResourceGroup deletedgroup -StartTime (Get-Date).AddDays(-14) -Caller someone@contoso.com
  ```

4. <span data-ttu-id="75e3f-145">Můžete filtrovat pro operace se nezdařila.</span><span class="sxs-lookup"><span data-stu-id="75e3f-145">You can filter for failed operations.</span></span>

  ```powershell
  Get-AzureRmLog -ResourceGroup ExampleGroup -Status Failed
  ```

5. <span data-ttu-id="75e3f-146">Můžete se zaměřit na jednu chybu prohlížením hello stavovou zprávu pro tuto položku.</span><span class="sxs-lookup"><span data-stu-id="75e3f-146">You can focus on one error by looking at hello status message for that entry.</span></span>
   
        ((Get-AzureRmLog -Status Failed -ResourceGroup ExampleGroup -DetailedOutput).Properties[1].Content["statusMessage"] | ConvertFrom-Json).error
   
    <span data-ttu-id="75e3f-147">Která vrací:</span><span class="sxs-lookup"><span data-stu-id="75e3f-147">Which returns:</span></span>
   
        code           message                                                                        
        ----           -------                                                                        
        DnsRecordInUse DNS record dns.westus.cloudapp.azure.com is already used by another public IP. 


## <a name="azure-cli"></a><span data-ttu-id="75e3f-148">Azure CLI</span><span class="sxs-lookup"><span data-stu-id="75e3f-148">Azure CLI</span></span>
* <span data-ttu-id="75e3f-149">tooretrieve položky protokolu, spusťte hello **zobrazit skupiny azure protokolu** příkaz.</span><span class="sxs-lookup"><span data-stu-id="75e3f-149">tooretrieve log entries, you run hello **azure group log show** command.</span></span>

  ```azurecli
  azure group log show ExampleGroup --json
  ```


## <a name="rest-api"></a><span data-ttu-id="75e3f-150">REST API</span><span class="sxs-lookup"><span data-stu-id="75e3f-150">REST API</span></span>
<span data-ttu-id="75e3f-151">Hello operace REST pro práci s hello protokolu aktivit jsou součástí hello [rozhraní REST API pro přehledy](https://msdn.microsoft.com/library/azure/dn931943.aspx).</span><span class="sxs-lookup"><span data-stu-id="75e3f-151">hello REST operations for working with hello activity log are part of hello [Insights REST API](https://msdn.microsoft.com/library/azure/dn931943.aspx).</span></span> <span data-ttu-id="75e3f-152">tooretrieve aktivity protokolu události, viz [seznam událostí správy hello v předplatném](https://msdn.microsoft.com/library/azure/dn931934.aspx).</span><span class="sxs-lookup"><span data-stu-id="75e3f-152">tooretrieve activity log events, see [List hello management events in a subscription](https://msdn.microsoft.com/library/azure/dn931934.aspx).</span></span>

## <a name="next-steps"></a><span data-ttu-id="75e3f-153">Další kroky</span><span class="sxs-lookup"><span data-stu-id="75e3f-153">Next steps</span></span>
* <span data-ttu-id="75e3f-154">Azure protokoly aktivity lze použít s Power BI toogain lepší přehled o hello akce v rámci vašeho předplatného.</span><span class="sxs-lookup"><span data-stu-id="75e3f-154">Azure Activity logs can be used with Power BI toogain greater insights about hello actions in your subscription.</span></span> <span data-ttu-id="75e3f-155">V tématu [zobrazení a analýza protokolů Azure aktivity v Power BI a další](https://azure.microsoft.com/blog/analyze-azure-audit-logs-in-powerbi-more/).</span><span class="sxs-lookup"><span data-stu-id="75e3f-155">See [View and analyze Azure Activity Logs in Power BI and more](https://azure.microsoft.com/blog/analyze-azure-audit-logs-in-powerbi-more/).</span></span>
* <span data-ttu-id="75e3f-156">toolearn o nastavení zásad zabezpečení, najdete v části [řízení přístupu na základě Role v Azure](../active-directory/role-based-access-control-configure.md).</span><span class="sxs-lookup"><span data-stu-id="75e3f-156">toolearn about setting security policies, see [Azure Role-based Access Control](../active-directory/role-based-access-control-configure.md).</span></span>
* <span data-ttu-id="75e3f-157">toolearn o hello příkazy pro zobrazení operace nasazení, najdete v části [zobrazit operace nasazení](resource-manager-deployment-operations.md).</span><span class="sxs-lookup"><span data-stu-id="75e3f-157">toolearn about hello commands for viewing deployment operations, see [View deployment operations](resource-manager-deployment-operations.md).</span></span>
* <span data-ttu-id="75e3f-158">jak zjistit, tooprevent odstranění prostředku pro všechny uživatele, toolearn [zamknutí prostředků pomocí Azure Resource Manageru](resource-group-lock-resources.md).</span><span class="sxs-lookup"><span data-stu-id="75e3f-158">toolearn how tooprevent deletions on a resource for all users, see [Lock resources with Azure Resource Manager](resource-group-lock-resources.md).</span></span>

