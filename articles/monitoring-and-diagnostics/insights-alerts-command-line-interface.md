---
title: "Vytvořte výstrahy pro služby Azure - napříč platformami rozhraní příkazového řádku | Microsoft Docs"
description: "Aktivační událost e-mailů, oznámení, weby adresy URL (webhooky), nebo volat automatizace při splnění zadané podmínky."
author: rboucher
manager: carmonm
editor: 
services: monitoring-and-diagnostics
documentationcenter: monitoring-and-diagnostics
ms.assetid: 5c6a2d27-7dcc-4f89-8752-9bb31b05ff35
ms.service: monitoring-and-diagnostics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/24/2016
ms.author: robb
ms.openlocfilehash: 92246a8da73a244a1c9a924bed55711d71a20fd8
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="create-metric-alerts-in-azure-monitor-for-azure-services---cross-platform-cli"></a><span data-ttu-id="27606-103">Vytvoření metriky výstrah v monitorování Azure pro služby Azure - napříč platformami rozhraní příkazového řádku</span><span class="sxs-lookup"><span data-stu-id="27606-103">Create metric alerts in Azure Monitor for Azure services - Cross-platform CLI</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="27606-104">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="27606-104">Portal</span></span>](insights-alerts-portal.md)
> * [<span data-ttu-id="27606-105">PowerShell</span><span class="sxs-lookup"><span data-stu-id="27606-105">PowerShell</span></span>](insights-alerts-powershell.md)
> * [<span data-ttu-id="27606-106">Rozhraní příkazového řádku</span><span class="sxs-lookup"><span data-stu-id="27606-106">CLI</span></span>](insights-alerts-command-line-interface.md)
>
>

## <a name="overview"></a><span data-ttu-id="27606-107">Přehled</span><span class="sxs-lookup"><span data-stu-id="27606-107">Overview</span></span>
<span data-ttu-id="27606-108">Tento článek ukazuje, jak nastavit Azure metriky výstrah pomocí napříč platformami rozhraní příkazového řádku (CLI).</span><span class="sxs-lookup"><span data-stu-id="27606-108">This article shows you how to set up Azure metric alerts using the cross-platform Command Line Interface (CLI).</span></span>

> [!NOTE]
> <span data-ttu-id="27606-109">Azure monitorování je nový název pro co byla volána "Statistika Azure" až 25 září 2016.</span><span class="sxs-lookup"><span data-stu-id="27606-109">Azure Monitor is the new name for what was called "Azure Insights" until Sept 25th, 2016.</span></span> <span data-ttu-id="27606-110">Však obory názvů a proto níže uvedených příkazů stále obsahovat "insights".</span><span class="sxs-lookup"><span data-stu-id="27606-110">However, the namespaces and thus the commands below still contain the "insights".</span></span>
>
>

<span data-ttu-id="27606-111">Můžete zobrazit upozornění na základě monitorování metriky pro nebo událostí na služeb Azure.</span><span class="sxs-lookup"><span data-stu-id="27606-111">You can receive an alert based on monitoring metrics for, or events on, your Azure services.</span></span>

* <span data-ttu-id="27606-112">**Metriky hodnoty** -výstrahy aktivuje, když hodnota zadané metriky překračuje prahovou hodnotu přiřadíte v obou směrech.</span><span class="sxs-lookup"><span data-stu-id="27606-112">**Metric values** - The alert triggers when the value of a specified metric crosses a threshold you assign in either direction.</span></span> <span data-ttu-id="27606-113">To znamená, aktivuje obě při nejprve je splněna podmínka, a pak později, pokud podmínka je už plněny.</span><span class="sxs-lookup"><span data-stu-id="27606-113">That is, it triggers both when the condition is first met and then afterwards when that condition is no longer being met.</span></span>    
* <span data-ttu-id="27606-114">**Události protokolu aktivit** -výstrahu můžete aktivovat pro *každých* události nebo pouze tehdy, když dojde k určité události.</span><span class="sxs-lookup"><span data-stu-id="27606-114">**Activity log events** - An alert can trigger on *every* event, or, only when a certain events occurs.</span></span> <span data-ttu-id="27606-115">Další informace o výstrahách aktivity protokolu [, klikněte sem](monitoring-activity-log-alerts.md)</span><span class="sxs-lookup"><span data-stu-id="27606-115">To learn more about activity log alerts [click here](monitoring-activity-log-alerts.md)</span></span>

<span data-ttu-id="27606-116">Můžete nakonfigurovat metriky výstrahu při aktivaci, proveďte následující:</span><span class="sxs-lookup"><span data-stu-id="27606-116">You can configure a metric alert to do the following when it triggers:</span></span>

* <span data-ttu-id="27606-117">odesílat oznámení e-mailu Správce služeb a spolusprávci</span><span class="sxs-lookup"><span data-stu-id="27606-117">send email notifications to the service administrator and co-administrators</span></span>
* <span data-ttu-id="27606-118">Odeslat e-mail na další e-mailů, které zadáte.</span><span class="sxs-lookup"><span data-stu-id="27606-118">send email to additional emails that you specify.</span></span>
* <span data-ttu-id="27606-119">Volat webhook, jehož</span><span class="sxs-lookup"><span data-stu-id="27606-119">call a webhook</span></span>
* <span data-ttu-id="27606-120">Spusťte provádění runbook služby Azure (pouze z portálu Azure v tuto chvíli)</span><span class="sxs-lookup"><span data-stu-id="27606-120">start execution of an Azure runbook (only from the Azure portal at this time)</span></span>

<span data-ttu-id="27606-121">Můžete nakonfigurovat a získat informace o použití metriky pravidla výstrah</span><span class="sxs-lookup"><span data-stu-id="27606-121">You can configure and get information about metric alert rules using</span></span>

* [<span data-ttu-id="27606-122">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="27606-122">Azure portal</span></span>](insights-alerts-portal.md)
* [<span data-ttu-id="27606-123">PowerShell</span><span class="sxs-lookup"><span data-stu-id="27606-123">PowerShell</span></span>](insights-alerts-powershell.md)
* [<span data-ttu-id="27606-124">rozhraní příkazového řádku (CLI)</span><span class="sxs-lookup"><span data-stu-id="27606-124">command-line interface (CLI)</span></span>](insights-alerts-command-line-interface.md)
* [<span data-ttu-id="27606-125">Rozhraní API REST Azure monitorování</span><span class="sxs-lookup"><span data-stu-id="27606-125">Azure Monitor REST API</span></span>](https://msdn.microsoft.com/library/azure/dn931945.aspx)

<span data-ttu-id="27606-126">Vždy dostanete nápovědy pro příkazy zadáním příkazu a - pomoci na konci.</span><span class="sxs-lookup"><span data-stu-id="27606-126">You can always receive help for commands by typing a command and putting -help at the end.</span></span> <span data-ttu-id="27606-127">Například:</span><span class="sxs-lookup"><span data-stu-id="27606-127">For example:</span></span>

    ```console
    azure insights alerts -help
    azure insights alerts actions email create -help
    ```

## <a name="create-alert-rules-using-the-cli"></a><span data-ttu-id="27606-128">Vytvořit pravidla výstrah pomocí rozhraní příkazového řádku</span><span class="sxs-lookup"><span data-stu-id="27606-128">Create alert rules using the CLI</span></span>
1. <span data-ttu-id="27606-129">Proveďte požadované součásti a přihlášení k Azure.</span><span class="sxs-lookup"><span data-stu-id="27606-129">Perform the Prerequisites and login to Azure.</span></span> <span data-ttu-id="27606-130">V tématu [ukázky rozhraní příkazového řádku Azure monitorování](insights-cli-samples.md).</span><span class="sxs-lookup"><span data-stu-id="27606-130">See [Azure Monitor CLI samples](insights-cli-samples.md).</span></span> <span data-ttu-id="27606-131">Stručně řečeno nainstalujte rozhraní příkazového řádku a spuštění těchto příkazů.</span><span class="sxs-lookup"><span data-stu-id="27606-131">In short, install the CLI and run these commands.</span></span> <span data-ttu-id="27606-132">Budou si přihlášení, zobrazit, jaké předplatné a připravit se ke spuštění příkazů Azure monitorování.</span><span class="sxs-lookup"><span data-stu-id="27606-132">They get you logged in, show what subscription you are using, and prepare you to run Azure Monitor commands.</span></span>

    ```console
    azure login
    azure account show
    azure config mode arm

    ```

2. <span data-ttu-id="27606-133">K zobrazení seznamu existujících pravidel ve skupině prostředků, použijte následující formulář **statistiky azure výstrahy pravidlo seznamu** *[možnosti] &lt;resourceGroup&gt;*</span><span class="sxs-lookup"><span data-stu-id="27606-133">To list existing rules on a resource group, use the following form **azure insights alerts rule list** *[options] &lt;resourceGroup&gt;*</span></span>

   ```console
   azure insights alerts rule list myresourcegroupname

   ```
3. <span data-ttu-id="27606-134">K vytvoření pravidla, musíte nejprve mít několik důležitých informací.</span><span class="sxs-lookup"><span data-stu-id="27606-134">To create a rule, you need to have several important pieces of information first.</span></span>
  * <span data-ttu-id="27606-135">**ID prostředku** pro prostředek, kterou chcete nastavit upozornění pro</span><span class="sxs-lookup"><span data-stu-id="27606-135">The **Resource ID** for the resource you want to set an alert for</span></span>
  * <span data-ttu-id="27606-136">**Definice metrik** k dispozici pro tento prostředek</span><span class="sxs-lookup"><span data-stu-id="27606-136">The **metric definitions** available for that resource</span></span>

     <span data-ttu-id="27606-137">Jeden způsob, jak získat ID prostředku je použití portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="27606-137">One way to get the Resource ID is to use the Azure portal.</span></span> <span data-ttu-id="27606-138">Za předpokladu, že byl již vytvořený, vyberte ho v portálu.</span><span class="sxs-lookup"><span data-stu-id="27606-138">Assuming the resource is already created, select it in the portal.</span></span> <span data-ttu-id="27606-139">V okně Další vyberte *vlastnosti* pod *nastavení* části.</span><span class="sxs-lookup"><span data-stu-id="27606-139">Then in the next blade, select *Properties* under the *Settings* section.</span></span> <span data-ttu-id="27606-140">*ID prostředku* je pole v okně Další.</span><span class="sxs-lookup"><span data-stu-id="27606-140">The *RESOURCE ID* is a field in the next blade.</span></span> <span data-ttu-id="27606-141">Dalším způsobem je použít [Průzkumníka prostředků Azure](https://resources.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="27606-141">Another way is to use the [Azure Resource Explorer](https://resources.azure.com/).</span></span>

     <span data-ttu-id="27606-142">Je třeba id prostředků pro webovou aplikaci</span><span class="sxs-lookup"><span data-stu-id="27606-142">An example resource id for a web app is</span></span>

     ```console
     /subscriptions/dededede-7aa0-407d-a6fb-eb20c8bd1192/resourceGroups/myresourcegroupname/providers/Microsoft.Web/sites/mywebsitename
     ```

     <span data-ttu-id="27606-143">Pokud chcete získat seznam dostupné metriky a jednotky pro tyto metriky pro předchozí příklad prostředků, použijte následující příkaz rozhraní příkazového řádku:</span><span class="sxs-lookup"><span data-stu-id="27606-143">To get a list of the available metrics and units for those metrics for the previous resource example, use the following CLI command:</span></span>  

     ```console
     azure insights metrics list /subscriptions/dededede-7aa0-407d-a6fb-eb20c8bd1192/resourceGroups/myresourcegroupname/providers/Microsoft.Web/sites/mywebsitename PT1M
     ```

     <span data-ttu-id="27606-144">*PT1M* je stejná i členitost dostupné měření (1-minutách).</span><span class="sxs-lookup"><span data-stu-id="27606-144">*PT1M* is the granularity of the available measurement (1-minute intervals).</span></span> <span data-ttu-id="27606-145">Pomocí různých členitostí nabízí různé možnosti metriky.</span><span class="sxs-lookup"><span data-stu-id="27606-145">Using different granularities gives you different metric options.</span></span>
4. <span data-ttu-id="27606-146">Pokud chcete vytvořit pravidlo výstrahy založené na metrika, použijte příkaz v následujícím formátu:</span><span class="sxs-lookup"><span data-stu-id="27606-146">To create a metric-based alert rule, use a command of the following form:</span></span>

    <span data-ttu-id="27606-147">**metriky sadu pravidel výstrahy statistiky Azure** *[možnosti] &lt;ruleName&gt; &lt;umístění&gt; &lt;resourceGroup&gt; &lt;velikost_okna &gt; &lt;operátor&gt; &lt;prahová hodnota&gt; &lt;targetResourceId&gt; &lt;metricName&gt; &lt; timeAggregationOperator&gt;*</span><span class="sxs-lookup"><span data-stu-id="27606-147">**azure insights alerts rule metric set** *[options] &lt;ruleName&gt; &lt;location&gt; &lt;resourceGroup&gt; &lt;windowSize&gt; &lt;operator&gt; &lt;threshold&gt; &lt;targetResourceId&gt; &lt;metricName&gt; &lt;timeAggregationOperator&gt;*</span></span>

    <span data-ttu-id="27606-148">Následující příklad nastaví výstrahu na webový server prostředku.</span><span class="sxs-lookup"><span data-stu-id="27606-148">The following example sets up an alert on a web site resource.</span></span> <span data-ttu-id="27606-149">Výstrahy aktivačních událostí, kdykoliv konzistentně obdrží veškerou komunikaci pro 5 minut a opakujte při přijetí žádný provoz 5 minut.</span><span class="sxs-lookup"><span data-stu-id="27606-149">The alert triggers whenever it consistently receives any traffic for 5 minutes and again when it receives no traffic for 5 minutes.</span></span>

    ```console
    azure insights alerts rule metric set myrule eastus myreasourcegroup PT5M GreaterThan 2 /subscriptions/dededede-7aa0-407d-a6fb-eb20c8bd1192/resourceGroups/myresourcegroupname/providers/Microsoft.Web/sites/mywebsitename BytesReceived Total

    ```
5. <span data-ttu-id="27606-150">K vytvoření webhooku nebo odeslat e-mail, když se aktivuje upozornění na metriky, je nutné nejprve vytvořte e-mailu nebo webhooky.</span><span class="sxs-lookup"><span data-stu-id="27606-150">To create webhook or send email when a metric alert fires, first create the email and/or webhooks.</span></span> <span data-ttu-id="27606-151">Pravidlo okamžitě vytvořit později.</span><span class="sxs-lookup"><span data-stu-id="27606-151">Then create the rule immediately afterwards.</span></span> <span data-ttu-id="27606-152">Nelze přidružit webhooku nebo e-mailů s již vytvořili pravidla pomocí rozhraní příkazového řádku.</span><span class="sxs-lookup"><span data-stu-id="27606-152">You cannot associate webhook or emails with already created rules using the CLI.</span></span>

    ```console
    azure insights alerts actions email create --customEmails myemail@contoso.com

    azure insights alerts actions webhook create https://www.contoso.com

    azure insights alerts rule metric set myrulewithwebhookandemail eastus myreasourcegroup PT5M GreaterThan 2 /subscriptions/dededede-7aa0-407d-a6fb-eb20c8bd1192/resourceGroups/myresourcegroupname/providers/Microsoft.Web/sites/mywebsitename BytesReceived Total
    ```

6. <span data-ttu-id="27606-153">Můžete ověřit, zda byly upozornění správně vytvořeny prohlížením jednotlivých pravidel.</span><span class="sxs-lookup"><span data-stu-id="27606-153">You can verify that your alerts have been created properly by looking at an individual rule.</span></span>

    ```console
    azure insights alerts rule list myresourcegroup --ruleName myrule
    ```
7. <span data-ttu-id="27606-154">Pokud chcete odstranit pravidla, použijte příkaz ve tvaru:</span><span class="sxs-lookup"><span data-stu-id="27606-154">To delete rules, use a command of the form:</span></span>

    <span data-ttu-id="27606-155">**Odstranit pravidlo výstrahy statistiky** [možnosti] &lt;resourceGroup&gt; &lt;ruleName&gt;</span><span class="sxs-lookup"><span data-stu-id="27606-155">**insights alerts rule delete** [options] &lt;resourceGroup&gt; &lt;ruleName&gt;</span></span>

    <span data-ttu-id="27606-156">Tyto příkazy odstranit pravidla dříve vytvořená v tomto článku.</span><span class="sxs-lookup"><span data-stu-id="27606-156">These commands delete the rules previously created in this article.</span></span>

    ```console
    azure insights alerts rule delete myresourcegroup myrule
    azure insights alerts rule delete myresourcegroup myrulewithwebhookandemail
    azure insights alerts rule delete myresourcegroup myActivityLogRule
    ```

## <a name="next-steps"></a><span data-ttu-id="27606-157">Další kroky</span><span class="sxs-lookup"><span data-stu-id="27606-157">Next steps</span></span>
* <span data-ttu-id="27606-158">[Získat přehled o Azure monitorování](monitoring-overview.md) včetně typy informací, můžete sledovat a shromažďovat.</span><span class="sxs-lookup"><span data-stu-id="27606-158">[Get an overview of Azure monitoring](monitoring-overview.md) including the types of information you can collect and monitor.</span></span>
* <span data-ttu-id="27606-159">Další informace o [konfigurace webhooky ve výstrahách](insights-webhooks-alerts.md).</span><span class="sxs-lookup"><span data-stu-id="27606-159">Learn more about [configuring webhooks in alerts](insights-webhooks-alerts.md).</span></span>
* <span data-ttu-id="27606-160">Další informace o [konfigurace výstrah pro aktivitu protokolu události](monitoring-activity-log-alerts.md).</span><span class="sxs-lookup"><span data-stu-id="27606-160">Learn more about [configuring alerts on Activity log events](monitoring-activity-log-alerts.md).</span></span>
* <span data-ttu-id="27606-161">Další informace o [sad Azure Automation Runbook](../automation/automation-starting-a-runbook.md).</span><span class="sxs-lookup"><span data-stu-id="27606-161">Learn more about [Azure Automation Runbooks](../automation/automation-starting-a-runbook.md).</span></span>
* <span data-ttu-id="27606-162">Získat [přehled shromažďování diagnostických protokolů](monitoring-overview-of-diagnostic-logs.md) ke shromažďování metrik podrobné vysoká frekvence vaší služby.</span><span class="sxs-lookup"><span data-stu-id="27606-162">Get an [overview of collecting diagnostic logs](monitoring-overview-of-diagnostic-logs.md) to collect detailed high-frequency metrics on your service.</span></span>
* <span data-ttu-id="27606-163">Získat [přehled metriky kolekce](insights-how-to-customize-monitoring.md) zkontrolovat služby je k dispozici a dobře reagovaly.</span><span class="sxs-lookup"><span data-stu-id="27606-163">Get an [overview of metrics collection](insights-how-to-customize-monitoring.md) to make sure your service is available and responsive.</span></span>
