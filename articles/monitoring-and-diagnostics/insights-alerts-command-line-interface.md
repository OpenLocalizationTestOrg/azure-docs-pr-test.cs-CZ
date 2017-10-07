---
title: "aaaCreate výstrahy pro služby Azure - napříč platformami rozhraní příkazového řádku | Microsoft Docs"
description: "Při splnění zadané podmínky hello aktivaci e-mailů, oznámení, adresy URL weby volání (webhooky) nebo automatizace."
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
ms.openlocfilehash: e53701e5377a415038a69fbd32f1e5fc5fe99be9
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-metric-alerts-in-azure-monitor-for-azure-services---cross-platform-cli"></a><span data-ttu-id="b6225-103">Vytvoření metriky výstrah v monitorování Azure pro služby Azure - napříč platformami rozhraní příkazového řádku</span><span class="sxs-lookup"><span data-stu-id="b6225-103">Create metric alerts in Azure Monitor for Azure services - Cross-platform CLI</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="b6225-104">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="b6225-104">Portal</span></span>](insights-alerts-portal.md)
> * [<span data-ttu-id="b6225-105">PowerShell</span><span class="sxs-lookup"><span data-stu-id="b6225-105">PowerShell</span></span>](insights-alerts-powershell.md)
> * [<span data-ttu-id="b6225-106">Rozhraní příkazového řádku</span><span class="sxs-lookup"><span data-stu-id="b6225-106">CLI</span></span>](insights-alerts-command-line-interface.md)
>
>

## <a name="overview"></a><span data-ttu-id="b6225-107">Přehled</span><span class="sxs-lookup"><span data-stu-id="b6225-107">Overview</span></span>
<span data-ttu-id="b6225-108">Tento článek ukazuje, jak tooset si Azure metriky výstrah pomocí hello napříč platformami rozhraní příkazového řádku (CLI).</span><span class="sxs-lookup"><span data-stu-id="b6225-108">This article shows you how tooset up Azure metric alerts using hello cross-platform Command Line Interface (CLI).</span></span>

> [!NOTE]
> <span data-ttu-id="b6225-109">Azure monitorování je hello nový název pro co byla volána "Statistika Azure" až 25 září 2016.</span><span class="sxs-lookup"><span data-stu-id="b6225-109">Azure Monitor is hello new name for what was called "Azure Insights" until Sept 25th, 2016.</span></span> <span data-ttu-id="b6225-110">Ale hello obory názvů a proto níže uvedené příkazy hello stále obsahovat hello "insights".</span><span class="sxs-lookup"><span data-stu-id="b6225-110">However, hello namespaces and thus hello commands below still contain hello "insights".</span></span>
>
>

<span data-ttu-id="b6225-111">Můžete zobrazit upozornění na základě monitorování metriky pro nebo událostí na služeb Azure.</span><span class="sxs-lookup"><span data-stu-id="b6225-111">You can receive an alert based on monitoring metrics for, or events on, your Azure services.</span></span>

* <span data-ttu-id="b6225-112">**Metriky hodnoty** – hello aktivační události výstrahy, když hodnota hello zadaný metriky překračuje prahovou hodnotu přiřadíte v obou směrech.</span><span class="sxs-lookup"><span data-stu-id="b6225-112">**Metric values** - hello alert triggers when hello value of a specified metric crosses a threshold you assign in either direction.</span></span> <span data-ttu-id="b6225-113">To znamená, aktivuje obě při první je splněna podmínka hello a pak později, pokud podmínky již probíhá není splněna.</span><span class="sxs-lookup"><span data-stu-id="b6225-113">That is, it triggers both when hello condition is first met and then afterwards when that condition is no longer being met.</span></span>    
* <span data-ttu-id="b6225-114">**Události protokolu aktivit** -výstrahu můžete aktivovat pro *každých* události nebo pouze tehdy, když dojde k určité události.</span><span class="sxs-lookup"><span data-stu-id="b6225-114">**Activity log events** - An alert can trigger on *every* event, or, only when a certain events occurs.</span></span> <span data-ttu-id="b6225-115">Další informace o protokolu upozornění toolearn [, klikněte sem](monitoring-activity-log-alerts.md)</span><span class="sxs-lookup"><span data-stu-id="b6225-115">toolearn more about activity log alerts [click here](monitoring-activity-log-alerts.md)</span></span>

<span data-ttu-id="b6225-116">Můžete nakonfigurovat následující hello metriky toodo výstrahy, když ji spustí:</span><span class="sxs-lookup"><span data-stu-id="b6225-116">You can configure a metric alert toodo hello following when it triggers:</span></span>

* <span data-ttu-id="b6225-117">odesílat oznámení e-mailu, toohello Správce služeb a spolusprávci</span><span class="sxs-lookup"><span data-stu-id="b6225-117">send email notifications toohello service administrator and co-administrators</span></span>
* <span data-ttu-id="b6225-118">odesílání e-mailů tooadditional e-mailů, které zadáte.</span><span class="sxs-lookup"><span data-stu-id="b6225-118">send email tooadditional emails that you specify.</span></span>
* <span data-ttu-id="b6225-119">Volat webhook, jehož</span><span class="sxs-lookup"><span data-stu-id="b6225-119">call a webhook</span></span>
* <span data-ttu-id="b6225-120">Spusťte provádění runbook služby Azure (pouze z portálu Azure v tuto chvíli hello)</span><span class="sxs-lookup"><span data-stu-id="b6225-120">start execution of an Azure runbook (only from hello Azure portal at this time)</span></span>

<span data-ttu-id="b6225-121">Můžete nakonfigurovat a získat informace o použití metriky pravidla výstrah</span><span class="sxs-lookup"><span data-stu-id="b6225-121">You can configure and get information about metric alert rules using</span></span>

* [<span data-ttu-id="b6225-122">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="b6225-122">Azure portal</span></span>](insights-alerts-portal.md)
* [<span data-ttu-id="b6225-123">PowerShell</span><span class="sxs-lookup"><span data-stu-id="b6225-123">PowerShell</span></span>](insights-alerts-powershell.md)
* [<span data-ttu-id="b6225-124">rozhraní příkazového řádku (CLI)</span><span class="sxs-lookup"><span data-stu-id="b6225-124">command-line interface (CLI)</span></span>](insights-alerts-command-line-interface.md)
* [<span data-ttu-id="b6225-125">Rozhraní API REST Azure monitorování</span><span class="sxs-lookup"><span data-stu-id="b6225-125">Azure Monitor REST API</span></span>](https://msdn.microsoft.com/library/azure/dn931945.aspx)

<span data-ttu-id="b6225-126">Vždy dostanete nápovědy pro příkazy zadáním příkazu a - pomoci na konci hello.</span><span class="sxs-lookup"><span data-stu-id="b6225-126">You can always receive help for commands by typing a command and putting -help at hello end.</span></span> <span data-ttu-id="b6225-127">Například:</span><span class="sxs-lookup"><span data-stu-id="b6225-127">For example:</span></span>

    ```console
    azure insights alerts -help
    azure insights alerts actions email create -help
    ```

## <a name="create-alert-rules-using-hello-cli"></a><span data-ttu-id="b6225-128">Vytvořit pravidla výstrah pomocí hello rozhraní příkazového řádku</span><span class="sxs-lookup"><span data-stu-id="b6225-128">Create alert rules using hello CLI</span></span>
1. <span data-ttu-id="b6225-129">Proveďte hello požadavky a tooAzure přihlášení.</span><span class="sxs-lookup"><span data-stu-id="b6225-129">Perform hello Prerequisites and login tooAzure.</span></span> <span data-ttu-id="b6225-130">V tématu [ukázky rozhraní příkazového řádku Azure monitorování](insights-cli-samples.md).</span><span class="sxs-lookup"><span data-stu-id="b6225-130">See [Azure Monitor CLI samples](insights-cli-samples.md).</span></span> <span data-ttu-id="b6225-131">Stručně řečeno nainstalujte hello rozhraní příkazového řádku a spuštění těchto příkazů.</span><span class="sxs-lookup"><span data-stu-id="b6225-131">In short, install hello CLI and run these commands.</span></span> <span data-ttu-id="b6225-132">Jejich získat jste přihlášeni, jaké předplatné a připravit se příkazy Azure monitorování toorun zobrazit.</span><span class="sxs-lookup"><span data-stu-id="b6225-132">They get you logged in, show what subscription you are using, and prepare you toorun Azure Monitor commands.</span></span>

    ```console
    azure login
    azure account show
    azure config mode arm

    ```

2. <span data-ttu-id="b6225-133">toolist existující pravidla na skupinu prostředků, použijte následující formulář hello **statistiky azure výstrahy pravidlo seznamu** *[možnosti] &lt;resourceGroup&gt;*</span><span class="sxs-lookup"><span data-stu-id="b6225-133">toolist existing rules on a resource group, use hello following form **azure insights alerts rule list** *[options] &lt;resourceGroup&gt;*</span></span>

   ```console
   azure insights alerts rule list myresourcegroupname

   ```
3. <span data-ttu-id="b6225-134">toocreate pravidlo, budete potřebovat toohave několik důležitých informací nejdřív.</span><span class="sxs-lookup"><span data-stu-id="b6225-134">toocreate a rule, you need toohave several important pieces of information first.</span></span>
  * <span data-ttu-id="b6225-135">Hello **ID prostředku** pro prostředek hello chcete tooset výstrahu pro</span><span class="sxs-lookup"><span data-stu-id="b6225-135">hello **Resource ID** for hello resource you want tooset an alert for</span></span>
  * <span data-ttu-id="b6225-136">Hello **definice metrik** k dispozici pro tento prostředek</span><span class="sxs-lookup"><span data-stu-id="b6225-136">hello **metric definitions** available for that resource</span></span>

     <span data-ttu-id="b6225-137">Jedním ze způsobů tooget hello ID prostředku je toouse hello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="b6225-137">One way tooget hello Resource ID is toouse hello Azure portal.</span></span> <span data-ttu-id="b6225-138">Za předpokladu, že již není vytvořen hello prostředků, vyberte ho v portálu hello.</span><span class="sxs-lookup"><span data-stu-id="b6225-138">Assuming hello resource is already created, select it in hello portal.</span></span> <span data-ttu-id="b6225-139">V dalším okně hello vyberte *vlastnosti* pod hello *nastavení* části.</span><span class="sxs-lookup"><span data-stu-id="b6225-139">Then in hello next blade, select *Properties* under hello *Settings* section.</span></span> <span data-ttu-id="b6225-140">Hello *ID prostředku* je pole v dalším okně hello.</span><span class="sxs-lookup"><span data-stu-id="b6225-140">hello *RESOURCE ID* is a field in hello next blade.</span></span> <span data-ttu-id="b6225-141">Další možností je toouse hello [Průzkumníka prostředků Azure](https://resources.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="b6225-141">Another way is toouse hello [Azure Resource Explorer](https://resources.azure.com/).</span></span>

     <span data-ttu-id="b6225-142">Je třeba id prostředků pro webovou aplikaci</span><span class="sxs-lookup"><span data-stu-id="b6225-142">An example resource id for a web app is</span></span>

     ```console
     /subscriptions/dededede-7aa0-407d-a6fb-eb20c8bd1192/resourceGroups/myresourcegroupname/providers/Microsoft.Web/sites/mywebsitename
     ```

     <span data-ttu-id="b6225-143">tooget seznam hello k dostupným metrikám a jednotky pro tyto metriky pro hello předchozí příklad prostředků, hello použijte následující příkaz rozhraní příkazového řádku:</span><span class="sxs-lookup"><span data-stu-id="b6225-143">tooget a list of hello available metrics and units for those metrics for hello previous resource example, use hello following CLI command:</span></span>  

     ```console
     azure insights metrics list /subscriptions/dededede-7aa0-407d-a6fb-eb20c8bd1192/resourceGroups/myresourcegroupname/providers/Microsoft.Web/sites/mywebsitename PT1M
     ```

     <span data-ttu-id="b6225-144">*PT1M* je hello členitost hello k dispozici měření (1-minutách).</span><span class="sxs-lookup"><span data-stu-id="b6225-144">*PT1M* is hello granularity of hello available measurement (1-minute intervals).</span></span> <span data-ttu-id="b6225-145">Pomocí různých členitostí nabízí různé možnosti metriky.</span><span class="sxs-lookup"><span data-stu-id="b6225-145">Using different granularities gives you different metric options.</span></span>
4. <span data-ttu-id="b6225-146">toocreate základě metrika pravidlo výstrahy, použijte příkaz hello následující formulář:</span><span class="sxs-lookup"><span data-stu-id="b6225-146">toocreate a metric-based alert rule, use a command of hello following form:</span></span>

    <span data-ttu-id="b6225-147">**metriky sadu pravidel výstrahy statistiky Azure** *[možnosti] &lt;ruleName&gt; &lt;umístění&gt; &lt;resourceGroup&gt; &lt;velikost_okna &gt; &lt;operátor&gt; &lt;prahová hodnota&gt; &lt;targetResourceId&gt; &lt;metricName&gt; &lt; timeAggregationOperator&gt;*</span><span class="sxs-lookup"><span data-stu-id="b6225-147">**azure insights alerts rule metric set** *[options] &lt;ruleName&gt; &lt;location&gt; &lt;resourceGroup&gt; &lt;windowSize&gt; &lt;operator&gt; &lt;threshold&gt; &lt;targetResourceId&gt; &lt;metricName&gt; &lt;timeAggregationOperator&gt;*</span></span>

    <span data-ttu-id="b6225-148">Následující příklad sady si výstrahy na prostředku webu Hello.</span><span class="sxs-lookup"><span data-stu-id="b6225-148">hello following example sets up an alert on a web site resource.</span></span> <span data-ttu-id="b6225-149">Hello výstrahy aktivuje vždy, když obdrží veškerou komunikaci konzistentně 5 minut a opakujte při přijetí žádný provoz 5 minut.</span><span class="sxs-lookup"><span data-stu-id="b6225-149">hello alert triggers whenever it consistently receives any traffic for 5 minutes and again when it receives no traffic for 5 minutes.</span></span>

    ```console
    azure insights alerts rule metric set myrule eastus myreasourcegroup PT5M GreaterThan 2 /subscriptions/dededede-7aa0-407d-a6fb-eb20c8bd1192/resourceGroups/myresourcegroupname/providers/Microsoft.Web/sites/mywebsitename BytesReceived Total

    ```
5. <span data-ttu-id="b6225-150">toocreate webhooku nebo odesílání e-mailu při aktivaci se metriky výstrahy, se nejprve vytvořit hello e-mailu nebo webhooky.</span><span class="sxs-lookup"><span data-stu-id="b6225-150">toocreate webhook or send email when a metric alert fires, first create hello email and/or webhooks.</span></span> <span data-ttu-id="b6225-151">Vytvořit pravidlo hello okamžitě později.</span><span class="sxs-lookup"><span data-stu-id="b6225-151">Then create hello rule immediately afterwards.</span></span> <span data-ttu-id="b6225-152">Nelze přidružit webhooku nebo e-mailů s již vytvořili pravidla pomocí hello rozhraní příkazového řádku.</span><span class="sxs-lookup"><span data-stu-id="b6225-152">You cannot associate webhook or emails with already created rules using hello CLI.</span></span>

    ```console
    azure insights alerts actions email create --customEmails myemail@contoso.com

    azure insights alerts actions webhook create https://www.contoso.com

    azure insights alerts rule metric set myrulewithwebhookandemail eastus myreasourcegroup PT5M GreaterThan 2 /subscriptions/dededede-7aa0-407d-a6fb-eb20c8bd1192/resourceGroups/myresourcegroupname/providers/Microsoft.Web/sites/mywebsitename BytesReceived Total
    ```

6. <span data-ttu-id="b6225-153">Můžete ověřit, zda byly upozornění správně vytvořeny prohlížením jednotlivých pravidel.</span><span class="sxs-lookup"><span data-stu-id="b6225-153">You can verify that your alerts have been created properly by looking at an individual rule.</span></span>

    ```console
    azure insights alerts rule list myresourcegroup --ruleName myrule
    ```
7. <span data-ttu-id="b6225-154">pravidla toodelete příkazem hello formuláře:</span><span class="sxs-lookup"><span data-stu-id="b6225-154">toodelete rules, use a command of hello form:</span></span>

    <span data-ttu-id="b6225-155">**Odstranit pravidlo výstrahy statistiky** [možnosti] &lt;resourceGroup&gt; &lt;ruleName&gt;</span><span class="sxs-lookup"><span data-stu-id="b6225-155">**insights alerts rule delete** [options] &lt;resourceGroup&gt; &lt;ruleName&gt;</span></span>

    <span data-ttu-id="b6225-156">Tyto příkazy odstranit pravidla hello dříve vytvořené v tomto článku.</span><span class="sxs-lookup"><span data-stu-id="b6225-156">These commands delete hello rules previously created in this article.</span></span>

    ```console
    azure insights alerts rule delete myresourcegroup myrule
    azure insights alerts rule delete myresourcegroup myrulewithwebhookandemail
    azure insights alerts rule delete myresourcegroup myActivityLogRule
    ```

## <a name="next-steps"></a><span data-ttu-id="b6225-157">Další kroky</span><span class="sxs-lookup"><span data-stu-id="b6225-157">Next steps</span></span>
* <span data-ttu-id="b6225-158">[Získat přehled o Azure monitorování](monitoring-overview.md) včetně hello typy informací, můžete sledovat a shromažďovat.</span><span class="sxs-lookup"><span data-stu-id="b6225-158">[Get an overview of Azure monitoring](monitoring-overview.md) including hello types of information you can collect and monitor.</span></span>
* <span data-ttu-id="b6225-159">Další informace o [konfigurace webhooky ve výstrahách](insights-webhooks-alerts.md).</span><span class="sxs-lookup"><span data-stu-id="b6225-159">Learn more about [configuring webhooks in alerts](insights-webhooks-alerts.md).</span></span>
* <span data-ttu-id="b6225-160">Další informace o [konfigurace výstrah pro aktivitu protokolu události](monitoring-activity-log-alerts.md).</span><span class="sxs-lookup"><span data-stu-id="b6225-160">Learn more about [configuring alerts on Activity log events](monitoring-activity-log-alerts.md).</span></span>
* <span data-ttu-id="b6225-161">Další informace o [sad Azure Automation Runbook](../automation/automation-starting-a-runbook.md).</span><span class="sxs-lookup"><span data-stu-id="b6225-161">Learn more about [Azure Automation Runbooks](../automation/automation-starting-a-runbook.md).</span></span>
* <span data-ttu-id="b6225-162">Získat [přehled shromažďování diagnostických protokolů](monitoring-overview-of-diagnostic-logs.md) toocollect podrobné vysoká frekvence metriky pro vaši službu.</span><span class="sxs-lookup"><span data-stu-id="b6225-162">Get an [overview of collecting diagnostic logs](monitoring-overview-of-diagnostic-logs.md) toocollect detailed high-frequency metrics on your service.</span></span>
* <span data-ttu-id="b6225-163">Získat [přehled metriky kolekce](insights-how-to-customize-monitoring.md) toomake, že je služba dostupná a dobře reagovaly.</span><span class="sxs-lookup"><span data-stu-id="b6225-163">Get an [overview of metrics collection](insights-how-to-customize-monitoring.md) toomake sure your service is available and responsive.</span></span>
