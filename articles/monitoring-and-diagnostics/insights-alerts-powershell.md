---
title: "Vytvořte výstrahy pro služby Azure – prostředí PowerShell | Microsoft Docs"
description: "Aktivační událost e-mailů, oznámení, weby adresy URL (webhooky), nebo volat automatizace při splnění zadané podmínky."
author: rboucher
manager: carmonm
editor: 
services: monitoring-and-diagnostics
documentationcenter: monitoring-and-diagnostics
ms.assetid: d26ab15b-7b7e-42a9-81c8-3ce9ead5d252
ms.service: monitoring-and-diagnostics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/20/2016
ms.author: robb
ms.openlocfilehash: 50127242cdf156771d0610e58cf2fc41281adae7
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="create-metric-alerts-in-azure-monitor-for-azure-services---powershell"></a><span data-ttu-id="6a477-103">Vytvoření metriky výstrah v monitorování Azure pro služby Azure – prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="6a477-103">Create metric alerts in Azure Monitor for Azure services - PowerShell</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="6a477-104">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="6a477-104">Portal</span></span>](insights-alerts-portal.md)
> * [<span data-ttu-id="6a477-105">PowerShell</span><span class="sxs-lookup"><span data-stu-id="6a477-105">PowerShell</span></span>](insights-alerts-powershell.md)
> * [<span data-ttu-id="6a477-106">Rozhraní příkazového řádku</span><span class="sxs-lookup"><span data-stu-id="6a477-106">CLI</span></span>](insights-alerts-command-line-interface.md)
>
>

## <a name="overview"></a><span data-ttu-id="6a477-107">Přehled</span><span class="sxs-lookup"><span data-stu-id="6a477-107">Overview</span></span>
<span data-ttu-id="6a477-108">Tento článek ukazuje, jak nastavit Azure metriky výstrah pomocí prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="6a477-108">This article shows you how to set up Azure metric alerts using PowerShell.</span></span>  

<span data-ttu-id="6a477-109">Můžete zobrazit upozornění na základě monitorování metriky pro nebo událostí na služeb Azure.</span><span class="sxs-lookup"><span data-stu-id="6a477-109">You can receive an alert based on monitoring metrics for, or events on, your Azure services.</span></span>

* <span data-ttu-id="6a477-110">**Metriky hodnoty** -výstrahy aktivuje, když hodnota zadané metriky překračuje prahovou hodnotu přiřadíte v obou směrech.</span><span class="sxs-lookup"><span data-stu-id="6a477-110">**Metric values** - The alert triggers when the value of a specified metric crosses a threshold you assign in either direction.</span></span> <span data-ttu-id="6a477-111">To znamená, aktivuje obě při nejprve je splněna podmínka, a pak později, pokud podmínka je už plněny.</span><span class="sxs-lookup"><span data-stu-id="6a477-111">That is, it triggers both when the condition is first met and then afterwards when that condition is no longer being met.</span></span>    
* <span data-ttu-id="6a477-112">**Události protokolu aktivit** -výstrahu můžete aktivovat pro *každých* události nebo pouze tehdy, když dojde k určité události.</span><span class="sxs-lookup"><span data-stu-id="6a477-112">**Activity log events** - An alert can trigger on *every* event, or, only when a certain events occurs.</span></span> <span data-ttu-id="6a477-113">Další informace o výstrahách aktivity protokolu [, klikněte sem](monitoring-activity-log-alerts.md)</span><span class="sxs-lookup"><span data-stu-id="6a477-113">To learn more about activity log alerts [click here](monitoring-activity-log-alerts.md)</span></span>

<span data-ttu-id="6a477-114">Můžete nakonfigurovat metriky výstrahu při aktivaci, proveďte následující:</span><span class="sxs-lookup"><span data-stu-id="6a477-114">You can configure a metric alert to do the following when it triggers:</span></span>

* <span data-ttu-id="6a477-115">odesílat oznámení e-mailu Správce služeb a spolusprávci</span><span class="sxs-lookup"><span data-stu-id="6a477-115">send email notifications to the service administrator and co-administrators</span></span>
* <span data-ttu-id="6a477-116">Odeslat e-mail na další e-mailů, které zadáte.</span><span class="sxs-lookup"><span data-stu-id="6a477-116">send email to additional emails that you specify.</span></span>
* <span data-ttu-id="6a477-117">Volat webhook, jehož</span><span class="sxs-lookup"><span data-stu-id="6a477-117">call a webhook</span></span>
* <span data-ttu-id="6a477-118">Spusťte provádění runbook služby Azure (pouze z portálu Azure)</span><span class="sxs-lookup"><span data-stu-id="6a477-118">start execution of an Azure runbook (only from the Azure portal)</span></span>

<span data-ttu-id="6a477-119">Můžete nakonfigurovat a získat informace o použití pravidla výstrah</span><span class="sxs-lookup"><span data-stu-id="6a477-119">You can configure and get information about alert rules using</span></span>

* [<span data-ttu-id="6a477-120">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="6a477-120">Azure portal</span></span>](insights-alerts-portal.md)
* [<span data-ttu-id="6a477-121">PowerShell</span><span class="sxs-lookup"><span data-stu-id="6a477-121">PowerShell</span></span>](insights-alerts-powershell.md)
* [<span data-ttu-id="6a477-122">rozhraní příkazového řádku (CLI)</span><span class="sxs-lookup"><span data-stu-id="6a477-122">command-line interface (CLI)</span></span>](insights-alerts-command-line-interface.md)
* [<span data-ttu-id="6a477-123">Rozhraní API REST Azure monitorování</span><span class="sxs-lookup"><span data-stu-id="6a477-123">Azure Monitor REST API</span></span>](https://msdn.microsoft.com/library/azure/dn931945.aspx)

<span data-ttu-id="6a477-124">Další informace najdete vždy zadáte ```Get-Help``` a potom příkaz prostředí PowerShell chcete zobrazit nápovědu.</span><span class="sxs-lookup"><span data-stu-id="6a477-124">For additional information, you can always type ```Get-Help``` and then the PowerShell command you want help on.</span></span>

## <a name="create-alert-rules-in-powershell"></a><span data-ttu-id="6a477-125">Vytvořit pravidla výstrah v prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="6a477-125">Create alert rules in PowerShell</span></span>
1. <span data-ttu-id="6a477-126">Přihlaste se k Azure.</span><span class="sxs-lookup"><span data-stu-id="6a477-126">Log in to Azure.</span></span>   

    ```PowerShell
    Login-AzureRmAccount

    ```
2. <span data-ttu-id="6a477-127">Získání seznamu odběry, které máte k dispozici.</span><span class="sxs-lookup"><span data-stu-id="6a477-127">Get a list of the subscriptions you have available.</span></span> <span data-ttu-id="6a477-128">Ověřte, že pracujete s správné předplatné.</span><span class="sxs-lookup"><span data-stu-id="6a477-128">Verify that you are working with the right subscription.</span></span> <span data-ttu-id="6a477-129">Pokud ne, nastavte ji na ten správný pomocí výstup `Get-AzureRmSubscription`.</span><span class="sxs-lookup"><span data-stu-id="6a477-129">If not, set it to the right one using the output from `Get-AzureRmSubscription`.</span></span>

    ```PowerShell
    Get-AzureRmSubscription
    Get-AzureRmContext
    Set-AzureRmContext -SubscriptionId <subscriptionid>
    ```
3. <span data-ttu-id="6a477-130">K zobrazení seznamu existujících pravidel ve skupině prostředků, použijte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="6a477-130">To list existing rules on a resource group, use the following command:</span></span>

   ```PowerShell
   Get-AzureRmAlertRule -ResourceGroup <myresourcegroup> -DetailedOutput
   ```
4. <span data-ttu-id="6a477-131">K vytvoření pravidla, musíte nejprve mít několik důležitých informací.</span><span class="sxs-lookup"><span data-stu-id="6a477-131">To create a rule, you need to have several important pieces of information first.</span></span>

  * <span data-ttu-id="6a477-132">**ID prostředku** pro prostředek, kterou chcete nastavit upozornění pro</span><span class="sxs-lookup"><span data-stu-id="6a477-132">The **Resource ID** for the resource you want to set an alert for</span></span>
  * <span data-ttu-id="6a477-133">**Definice metrik** k dispozici pro tento prostředek</span><span class="sxs-lookup"><span data-stu-id="6a477-133">The **metric definitions** available for that resource</span></span>

     <span data-ttu-id="6a477-134">Jeden způsob, jak získat ID prostředku je použití portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="6a477-134">One way to get the Resource ID is to use the Azure portal.</span></span> <span data-ttu-id="6a477-135">Za předpokladu, že byl již vytvořený, vyberte ho v portálu.</span><span class="sxs-lookup"><span data-stu-id="6a477-135">Assuming the resource is already created, select it in the portal.</span></span> <span data-ttu-id="6a477-136">V okně Další vyberte *vlastnosti* pod *nastavení* části.</span><span class="sxs-lookup"><span data-stu-id="6a477-136">Then in the next blade, select *Properties* under the *Settings* section.</span></span> <span data-ttu-id="6a477-137">**ID prostředku** je pole v okně Další.</span><span class="sxs-lookup"><span data-stu-id="6a477-137">**RESOURCE ID** is a field in the next blade.</span></span> <span data-ttu-id="6a477-138">Dalším způsobem je použít [Průzkumníka prostředků Azure](https://resources.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="6a477-138">Another way is to use the [Azure Resource Explorer](https://resources.azure.com/).</span></span>

     <span data-ttu-id="6a477-139">Je například ID prostředku pro webovou aplikaci</span><span class="sxs-lookup"><span data-stu-id="6a477-139">An example Resource ID for a web app is</span></span>

     ```
     /subscriptions/dededede-7aa0-407d-a6fb-eb20c8bd1192/resourceGroups/myresourcegroupname/providers/Microsoft.Web/sites/mywebsitename
     ```

     <span data-ttu-id="6a477-140">Můžete použít `Get-AzureRmMetricDefinition` zobrazíte seznam všechny definice metrik pro konkrétní zdroje.</span><span class="sxs-lookup"><span data-stu-id="6a477-140">You can use `Get-AzureRmMetricDefinition` to view the list of all metric definitions for a specific resource.</span></span>

     ```PowerShell
     Get-AzureRmMetricDefinition -ResourceId <resource_id>
     ```

     <span data-ttu-id="6a477-141">Následující příklad vytvoří tabulku s metrika název a jednotky pro tuto metriku.</span><span class="sxs-lookup"><span data-stu-id="6a477-141">The following example generates a table with the metric Name and the Unit for that metric.</span></span>

     ```PowerShell
     Get-AzureRmMetricDefinition -ResourceId <resource_id> | Format-Table -Property Name,Unit

     ```
     <span data-ttu-id="6a477-142">Úplný seznam dostupných možností pro Get-AzureRmMetricDefinition je k dispozici spuštěním Get-MetricDefinitions.</span><span class="sxs-lookup"><span data-stu-id="6a477-142">A full list of available options for Get-AzureRmMetricDefinition is available by running Get-MetricDefinitions.</span></span>
5. <span data-ttu-id="6a477-143">Následující příklad nastaví výstrahu na webový server prostředku.</span><span class="sxs-lookup"><span data-stu-id="6a477-143">The following example sets up an alert on a web site resource.</span></span> <span data-ttu-id="6a477-144">Výstrahy aktivačních událostí, kdykoliv konzistentně obdrží veškerou komunikaci pro 5 minut a opakujte při přijetí žádný provoz 5 minut.</span><span class="sxs-lookup"><span data-stu-id="6a477-144">The alert triggers whenever it consistently receives any traffic for 5 minutes and again when it receives no traffic for 5 minutes.</span></span>

    ```PowerShell
    Add-AzureRmMetricAlertRule -Name myMetricRuleWithWebhookAndEmail -Location "East US" -ResourceGroup myresourcegroup -TargetResourceId /subscriptions/dededede-7aa0-407d-a6fb-eb20c8bd1192/resourceGroups/myresourcegroupname/providers/Microsoft.Web/sites/mywebsitename -MetricName "BytesReceived" -Operator GreaterThan -Threshold 2 -WindowSize 00:05:00 -TimeAggregationOperator Total -Description "alert on any website activity"

    ```
6. <span data-ttu-id="6a477-145">K vytvoření webhooku nebo odeslat e-mail, když se aktivuje výstraha, je nutné nejprve vytvořte e-mailu nebo webhooky.</span><span class="sxs-lookup"><span data-stu-id="6a477-145">To create webhook or send email when an alert triggers, first create the email and/or webhooks.</span></span> <span data-ttu-id="6a477-146">Potom hned vytvořte pravidlo později se značkou - akce a jak je znázorněno v následujícím příkladu.</span><span class="sxs-lookup"><span data-stu-id="6a477-146">Then immediately create the rule afterwards with the -Actions tag and as shown in the following example.</span></span> <span data-ttu-id="6a477-147">Nelze přidružit webhooku nebo e-mailů s již vytvořili pravidla pomocí prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="6a477-147">You cannot associate webhook or emails with already created rules via PowerShell.</span></span>

    ```PowerShell
    $actionEmail = New-AzureRmAlertRuleEmail -CustomEmail myname@company.com
    $actionWebhook = New-AzureRmAlertRuleWebhook -ServiceUri https://www.contoso.com?token=mytoken

    Add-AzureRmMetricAlertRule -Name myMetricRuleWithWebhookAndEmail -Location "East US" -ResourceGroup myresourcegroup -TargetResourceId /subscriptions/dededede-7aa0-407d-a6fb-eb20c8bd1192/resourceGroups/myresourcegroupname/providers/Microsoft.Web/sites/mywebsitename -MetricName "BytesReceived" -Operator GreaterThan -Threshold 2 -WindowSize 00:05:00 -TimeAggregationOperator Total -Actions $actionEmail, $actionWebhook -Description "alert on any website activity"
    ```

7. <span data-ttu-id="6a477-148">Chcete-li ověřit, že upozornění nebyly vytvořeny správně pohledem na jednotlivá pravidla.</span><span class="sxs-lookup"><span data-stu-id="6a477-148">To verify that your alerts have been created properly by looking at the individual rules.</span></span>

    ```PowerShell
    Get-AzureRmAlertRule -Name myMetricRuleWithWebhookAndEmail -ResourceGroup myresourcegroup -DetailedOutput

    Get-AzureRmAlertRule -Name myLogAlertRule -ResourceGroup myresourcegroup -DetailedOutput
    ```
8. <span data-ttu-id="6a477-149">Oznámení odstraňte.</span><span class="sxs-lookup"><span data-stu-id="6a477-149">Delete your alerts.</span></span> <span data-ttu-id="6a477-150">Tyto příkazy odstranit pravidla vytvořená dříve v tomto článku.</span><span class="sxs-lookup"><span data-stu-id="6a477-150">These commands delete the rules created previously in this article.</span></span>

    ```PowerShell
    Remove-AzureRmAlertRule -ResourceGroup myresourcegroup -Name myrule
    Remove-AzureRmAlertRule -ResourceGroup myresourcegroup -Name myMetricRuleWithWebhookAndEmail
    Remove-AzureRmAlertRule -ResourceGroup myresourcegroup -Name myLogAlertRule
    ```

## <a name="next-steps"></a><span data-ttu-id="6a477-151">Další kroky</span><span class="sxs-lookup"><span data-stu-id="6a477-151">Next steps</span></span>
* <span data-ttu-id="6a477-152">[Získat přehled o Azure monitorování](monitoring-overview.md) včetně typy informací, můžete sledovat a shromažďovat.</span><span class="sxs-lookup"><span data-stu-id="6a477-152">[Get an overview of Azure monitoring](monitoring-overview.md) including the types of information you can collect and monitor.</span></span>
* <span data-ttu-id="6a477-153">Další informace o [konfigurace webhooky ve výstrahách](insights-webhooks-alerts.md).</span><span class="sxs-lookup"><span data-stu-id="6a477-153">Learn more about [configuring webhooks in alerts](insights-webhooks-alerts.md).</span></span>
* <span data-ttu-id="6a477-154">Další informace o [konfigurace výstrah pro aktivitu protokolu události](monitoring-activity-log-alerts.md).</span><span class="sxs-lookup"><span data-stu-id="6a477-154">Learn more about [configuring alerts on Activity log events](monitoring-activity-log-alerts.md).</span></span>
* <span data-ttu-id="6a477-155">Další informace o [sad Azure Automation Runbook](../automation/automation-starting-a-runbook.md).</span><span class="sxs-lookup"><span data-stu-id="6a477-155">Learn more about [Azure Automation Runbooks](../automation/automation-starting-a-runbook.md).</span></span>
* <span data-ttu-id="6a477-156">Získat [přehled shromažďování diagnostických protokolů](monitoring-overview-of-diagnostic-logs.md) ke shromažďování metrik podrobné vysoká frekvence vaší služby.</span><span class="sxs-lookup"><span data-stu-id="6a477-156">Get an [overview of collecting diagnostic logs](monitoring-overview-of-diagnostic-logs.md) to collect detailed high-frequency metrics on your service.</span></span>
* <span data-ttu-id="6a477-157">Získat [přehled metriky kolekce](insights-how-to-customize-monitoring.md) zkontrolovat služby je k dispozici a dobře reagovaly.</span><span class="sxs-lookup"><span data-stu-id="6a477-157">Get an [overview of metrics collection](insights-how-to-customize-monitoring.md) to make sure your service is available and responsive.</span></span>
