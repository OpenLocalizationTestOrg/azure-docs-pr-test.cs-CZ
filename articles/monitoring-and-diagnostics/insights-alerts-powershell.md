---
title: "aaaCreate výstrahy pro služby Azure – prostředí PowerShell | Microsoft Docs"
description: "Při splnění zadané podmínky hello aktivaci e-mailů, oznámení, adresy URL weby volání (webhooky) nebo automatizace."
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
ms.openlocfilehash: 80d3a3f194fc6a5a09a81d04206ea7a1640bddb0
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-metric-alerts-in-azure-monitor-for-azure-services---powershell"></a><span data-ttu-id="37a54-103">Vytvoření metriky výstrah v monitorování Azure pro služby Azure – prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="37a54-103">Create metric alerts in Azure Monitor for Azure services - PowerShell</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="37a54-104">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="37a54-104">Portal</span></span>](insights-alerts-portal.md)
> * [<span data-ttu-id="37a54-105">PowerShell</span><span class="sxs-lookup"><span data-stu-id="37a54-105">PowerShell</span></span>](insights-alerts-powershell.md)
> * [<span data-ttu-id="37a54-106">Rozhraní příkazového řádku</span><span class="sxs-lookup"><span data-stu-id="37a54-106">CLI</span></span>](insights-alerts-command-line-interface.md)
>
>

## <a name="overview"></a><span data-ttu-id="37a54-107">Přehled</span><span class="sxs-lookup"><span data-stu-id="37a54-107">Overview</span></span>
<span data-ttu-id="37a54-108">Tento článek ukazuje, jak tooset si Azure metrika výstrah pomocí prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="37a54-108">This article shows you how tooset up Azure metric alerts using PowerShell.</span></span>  

<span data-ttu-id="37a54-109">Můžete zobrazit upozornění na základě monitorování metriky pro nebo událostí na služeb Azure.</span><span class="sxs-lookup"><span data-stu-id="37a54-109">You can receive an alert based on monitoring metrics for, or events on, your Azure services.</span></span>

* <span data-ttu-id="37a54-110">**Metriky hodnoty** – hello aktivační události výstrahy, když hodnota hello zadaný metriky překračuje prahovou hodnotu přiřadíte v obou směrech.</span><span class="sxs-lookup"><span data-stu-id="37a54-110">**Metric values** - hello alert triggers when hello value of a specified metric crosses a threshold you assign in either direction.</span></span> <span data-ttu-id="37a54-111">To znamená, aktivuje obě při první je splněna podmínka hello a pak později, pokud podmínky již probíhá není splněna.</span><span class="sxs-lookup"><span data-stu-id="37a54-111">That is, it triggers both when hello condition is first met and then afterwards when that condition is no longer being met.</span></span>    
* <span data-ttu-id="37a54-112">**Události protokolu aktivit** -výstrahu můžete aktivovat pro *každých* události nebo pouze tehdy, když dojde k určité události.</span><span class="sxs-lookup"><span data-stu-id="37a54-112">**Activity log events** - An alert can trigger on *every* event, or, only when a certain events occurs.</span></span> <span data-ttu-id="37a54-113">Další informace o protokolu upozornění toolearn [, klikněte sem](monitoring-activity-log-alerts.md)</span><span class="sxs-lookup"><span data-stu-id="37a54-113">toolearn more about activity log alerts [click here](monitoring-activity-log-alerts.md)</span></span>

<span data-ttu-id="37a54-114">Můžete nakonfigurovat následující hello metriky toodo výstrahy, když ji spustí:</span><span class="sxs-lookup"><span data-stu-id="37a54-114">You can configure a metric alert toodo hello following when it triggers:</span></span>

* <span data-ttu-id="37a54-115">odesílat oznámení e-mailu, toohello Správce služeb a spolusprávci</span><span class="sxs-lookup"><span data-stu-id="37a54-115">send email notifications toohello service administrator and co-administrators</span></span>
* <span data-ttu-id="37a54-116">odesílání e-mailů tooadditional e-mailů, které zadáte.</span><span class="sxs-lookup"><span data-stu-id="37a54-116">send email tooadditional emails that you specify.</span></span>
* <span data-ttu-id="37a54-117">Volat webhook, jehož</span><span class="sxs-lookup"><span data-stu-id="37a54-117">call a webhook</span></span>
* <span data-ttu-id="37a54-118">Spusťte provádění runbook služby Azure (pouze z hello portál Azure)</span><span class="sxs-lookup"><span data-stu-id="37a54-118">start execution of an Azure runbook (only from hello Azure portal)</span></span>

<span data-ttu-id="37a54-119">Můžete nakonfigurovat a získat informace o použití pravidla výstrah</span><span class="sxs-lookup"><span data-stu-id="37a54-119">You can configure and get information about alert rules using</span></span>

* [<span data-ttu-id="37a54-120">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="37a54-120">Azure portal</span></span>](insights-alerts-portal.md)
* [<span data-ttu-id="37a54-121">PowerShell</span><span class="sxs-lookup"><span data-stu-id="37a54-121">PowerShell</span></span>](insights-alerts-powershell.md)
* [<span data-ttu-id="37a54-122">rozhraní příkazového řádku (CLI)</span><span class="sxs-lookup"><span data-stu-id="37a54-122">command-line interface (CLI)</span></span>](insights-alerts-command-line-interface.md)
* [<span data-ttu-id="37a54-123">Rozhraní API REST Azure monitorování</span><span class="sxs-lookup"><span data-stu-id="37a54-123">Azure Monitor REST API</span></span>](https://msdn.microsoft.com/library/azure/dn931945.aspx)

<span data-ttu-id="37a54-124">Další informace najdete vždy zadáte ```Get-Help``` a pak hello příkaz prostředí PowerShell, který chcete zobrazit nápovědu.</span><span class="sxs-lookup"><span data-stu-id="37a54-124">For additional information, you can always type ```Get-Help``` and then hello PowerShell command you want help on.</span></span>

## <a name="create-alert-rules-in-powershell"></a><span data-ttu-id="37a54-125">Vytvořit pravidla výstrah v prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="37a54-125">Create alert rules in PowerShell</span></span>
1. <span data-ttu-id="37a54-126">Přihlaste se tooAzure.</span><span class="sxs-lookup"><span data-stu-id="37a54-126">Log in tooAzure.</span></span>   

    ```PowerShell
    Login-AzureRmAccount

    ```
2. <span data-ttu-id="37a54-127">Získání seznamu předplatných hello máte k dispozici.</span><span class="sxs-lookup"><span data-stu-id="37a54-127">Get a list of hello subscriptions you have available.</span></span> <span data-ttu-id="37a54-128">Ověřte, že pracujete s hello správné předplatné.</span><span class="sxs-lookup"><span data-stu-id="37a54-128">Verify that you are working with hello right subscription.</span></span> <span data-ttu-id="37a54-129">Pokud ne, nastavte ji toohello doprava o jednu pomocí výstup hello `Get-AzureRmSubscription`.</span><span class="sxs-lookup"><span data-stu-id="37a54-129">If not, set it toohello right one using hello output from `Get-AzureRmSubscription`.</span></span>

    ```PowerShell
    Get-AzureRmSubscription
    Get-AzureRmContext
    Set-AzureRmContext -SubscriptionId <subscriptionid>
    ```
3. <span data-ttu-id="37a54-130">toolist existující pravidla na skupinu prostředků, hello použijte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="37a54-130">toolist existing rules on a resource group, use hello following command:</span></span>

   ```PowerShell
   Get-AzureRmAlertRule -ResourceGroup <myresourcegroup> -DetailedOutput
   ```
4. <span data-ttu-id="37a54-131">toocreate pravidlo, budete potřebovat toohave několik důležitých informací nejdřív.</span><span class="sxs-lookup"><span data-stu-id="37a54-131">toocreate a rule, you need toohave several important pieces of information first.</span></span>

  * <span data-ttu-id="37a54-132">Hello **ID prostředku** pro prostředek hello chcete tooset výstrahu pro</span><span class="sxs-lookup"><span data-stu-id="37a54-132">hello **Resource ID** for hello resource you want tooset an alert for</span></span>
  * <span data-ttu-id="37a54-133">Hello **definice metrik** k dispozici pro tento prostředek</span><span class="sxs-lookup"><span data-stu-id="37a54-133">hello **metric definitions** available for that resource</span></span>

     <span data-ttu-id="37a54-134">Jedním ze způsobů tooget hello ID prostředku je toouse hello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="37a54-134">One way tooget hello Resource ID is toouse hello Azure portal.</span></span> <span data-ttu-id="37a54-135">Za předpokladu, že již není vytvořen hello prostředků, vyberte ho v portálu hello.</span><span class="sxs-lookup"><span data-stu-id="37a54-135">Assuming hello resource is already created, select it in hello portal.</span></span> <span data-ttu-id="37a54-136">V dalším okně hello vyberte *vlastnosti* pod hello *nastavení* části.</span><span class="sxs-lookup"><span data-stu-id="37a54-136">Then in hello next blade, select *Properties* under hello *Settings* section.</span></span> <span data-ttu-id="37a54-137">**ID prostředku** je pole v dalším okně hello.</span><span class="sxs-lookup"><span data-stu-id="37a54-137">**RESOURCE ID** is a field in hello next blade.</span></span> <span data-ttu-id="37a54-138">Další možností je toouse hello [Průzkumníka prostředků Azure](https://resources.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="37a54-138">Another way is toouse hello [Azure Resource Explorer](https://resources.azure.com/).</span></span>

     <span data-ttu-id="37a54-139">Je například ID prostředku pro webovou aplikaci</span><span class="sxs-lookup"><span data-stu-id="37a54-139">An example Resource ID for a web app is</span></span>

     ```
     /subscriptions/dededede-7aa0-407d-a6fb-eb20c8bd1192/resourceGroups/myresourcegroupname/providers/Microsoft.Web/sites/mywebsitename
     ```

     <span data-ttu-id="37a54-140">Můžete použít `Get-AzureRmMetricDefinition` tooview hello seznam všechny definice metrik pro konkrétní zdroje.</span><span class="sxs-lookup"><span data-stu-id="37a54-140">You can use `Get-AzureRmMetricDefinition` tooview hello list of all metric definitions for a specific resource.</span></span>

     ```PowerShell
     Get-AzureRmMetricDefinition -ResourceId <resource_id>
     ```

     <span data-ttu-id="37a54-141">Hello následující příklad vygeneruje tabulku s hello metrika název a hello jednotky pro tuto metriku.</span><span class="sxs-lookup"><span data-stu-id="37a54-141">hello following example generates a table with hello metric Name and hello Unit for that metric.</span></span>

     ```PowerShell
     Get-AzureRmMetricDefinition -ResourceId <resource_id> | Format-Table -Property Name,Unit

     ```
     <span data-ttu-id="37a54-142">Úplný seznam dostupných možností pro Get-AzureRmMetricDefinition je k dispozici spuštěním Get-MetricDefinitions.</span><span class="sxs-lookup"><span data-stu-id="37a54-142">A full list of available options for Get-AzureRmMetricDefinition is available by running Get-MetricDefinitions.</span></span>
5. <span data-ttu-id="37a54-143">Následující příklad sady si výstrahy na prostředku webu Hello.</span><span class="sxs-lookup"><span data-stu-id="37a54-143">hello following example sets up an alert on a web site resource.</span></span> <span data-ttu-id="37a54-144">Hello výstrahy aktivuje vždy, když obdrží veškerou komunikaci konzistentně 5 minut a opakujte při přijetí žádný provoz 5 minut.</span><span class="sxs-lookup"><span data-stu-id="37a54-144">hello alert triggers whenever it consistently receives any traffic for 5 minutes and again when it receives no traffic for 5 minutes.</span></span>

    ```PowerShell
    Add-AzureRmMetricAlertRule -Name myMetricRuleWithWebhookAndEmail -Location "East US" -ResourceGroup myresourcegroup -TargetResourceId /subscriptions/dededede-7aa0-407d-a6fb-eb20c8bd1192/resourceGroups/myresourcegroupname/providers/Microsoft.Web/sites/mywebsitename -MetricName "BytesReceived" -Operator GreaterThan -Threshold 2 -WindowSize 00:05:00 -TimeAggregationOperator Total -Description "alert on any website activity"

    ```
6. <span data-ttu-id="37a54-145">toocreate webhooku nebo odesílat e-maily, když se aktivuje výstraha, nejprve vytvořit hello e-mailu nebo webhooky.</span><span class="sxs-lookup"><span data-stu-id="37a54-145">toocreate webhook or send email when an alert triggers, first create hello email and/or webhooks.</span></span> <span data-ttu-id="37a54-146">Hned vytvořit pravidlo hello později s hello - akce značky a jak je znázorněno v následující ukázka hello.</span><span class="sxs-lookup"><span data-stu-id="37a54-146">Then immediately create hello rule afterwards with hello -Actions tag and as shown in hello following example.</span></span> <span data-ttu-id="37a54-147">Nelze přidružit webhooku nebo e-mailů s již vytvořili pravidla pomocí prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="37a54-147">You cannot associate webhook or emails with already created rules via PowerShell.</span></span>

    ```PowerShell
    $actionEmail = New-AzureRmAlertRuleEmail -CustomEmail myname@company.com
    $actionWebhook = New-AzureRmAlertRuleWebhook -ServiceUri https://www.contoso.com?token=mytoken

    Add-AzureRmMetricAlertRule -Name myMetricRuleWithWebhookAndEmail -Location "East US" -ResourceGroup myresourcegroup -TargetResourceId /subscriptions/dededede-7aa0-407d-a6fb-eb20c8bd1192/resourceGroups/myresourcegroupname/providers/Microsoft.Web/sites/mywebsitename -MetricName "BytesReceived" -Operator GreaterThan -Threshold 2 -WindowSize 00:05:00 -TimeAggregationOperator Total -Actions $actionEmail, $actionWebhook -Description "alert on any website activity"
    ```

7. <span data-ttu-id="37a54-148">tooverify, že vaše upozornění byla vytvořena správně prohlížením hello jednotlivých pravidel.</span><span class="sxs-lookup"><span data-stu-id="37a54-148">tooverify that your alerts have been created properly by looking at hello individual rules.</span></span>

    ```PowerShell
    Get-AzureRmAlertRule -Name myMetricRuleWithWebhookAndEmail -ResourceGroup myresourcegroup -DetailedOutput

    Get-AzureRmAlertRule -Name myLogAlertRule -ResourceGroup myresourcegroup -DetailedOutput
    ```
8. <span data-ttu-id="37a54-149">Oznámení odstraňte.</span><span class="sxs-lookup"><span data-stu-id="37a54-149">Delete your alerts.</span></span> <span data-ttu-id="37a54-150">Tyto příkazy odstranit pravidla hello vytvořili dříve v tomto článku.</span><span class="sxs-lookup"><span data-stu-id="37a54-150">These commands delete hello rules created previously in this article.</span></span>

    ```PowerShell
    Remove-AzureRmAlertRule -ResourceGroup myresourcegroup -Name myrule
    Remove-AzureRmAlertRule -ResourceGroup myresourcegroup -Name myMetricRuleWithWebhookAndEmail
    Remove-AzureRmAlertRule -ResourceGroup myresourcegroup -Name myLogAlertRule
    ```

## <a name="next-steps"></a><span data-ttu-id="37a54-151">Další kroky</span><span class="sxs-lookup"><span data-stu-id="37a54-151">Next steps</span></span>
* <span data-ttu-id="37a54-152">[Získat přehled o Azure monitorování](monitoring-overview.md) včetně hello typy informací, můžete sledovat a shromažďovat.</span><span class="sxs-lookup"><span data-stu-id="37a54-152">[Get an overview of Azure monitoring](monitoring-overview.md) including hello types of information you can collect and monitor.</span></span>
* <span data-ttu-id="37a54-153">Další informace o [konfigurace webhooky ve výstrahách](insights-webhooks-alerts.md).</span><span class="sxs-lookup"><span data-stu-id="37a54-153">Learn more about [configuring webhooks in alerts](insights-webhooks-alerts.md).</span></span>
* <span data-ttu-id="37a54-154">Další informace o [konfigurace výstrah pro aktivitu protokolu události](monitoring-activity-log-alerts.md).</span><span class="sxs-lookup"><span data-stu-id="37a54-154">Learn more about [configuring alerts on Activity log events](monitoring-activity-log-alerts.md).</span></span>
* <span data-ttu-id="37a54-155">Další informace o [sad Azure Automation Runbook](../automation/automation-starting-a-runbook.md).</span><span class="sxs-lookup"><span data-stu-id="37a54-155">Learn more about [Azure Automation Runbooks](../automation/automation-starting-a-runbook.md).</span></span>
* <span data-ttu-id="37a54-156">Získat [přehled shromažďování diagnostických protokolů](monitoring-overview-of-diagnostic-logs.md) toocollect podrobné vysoká frekvence metriky pro vaši službu.</span><span class="sxs-lookup"><span data-stu-id="37a54-156">Get an [overview of collecting diagnostic logs](monitoring-overview-of-diagnostic-logs.md) toocollect detailed high-frequency metrics on your service.</span></span>
* <span data-ttu-id="37a54-157">Získat [přehled metriky kolekce](insights-how-to-customize-monitoring.md) toomake, že je služba dostupná a dobře reagovaly.</span><span class="sxs-lookup"><span data-stu-id="37a54-157">Get an [overview of metrics collection](insights-how-to-customize-monitoring.md) toomake sure your service is available and responsive.</span></span>
