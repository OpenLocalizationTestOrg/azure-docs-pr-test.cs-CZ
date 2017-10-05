---
title: "Vytvořte výstrahy pro služby Azure - portálu Azure | Microsoft Docs"
description: "Aktivační událost e-mailů, oznámení, weby adresy URL (webhooky), nebo volat automatizace při splnění zadané podmínky."
author: rboucher
manager: carmonm
editor: 
services: monitoring-and-diagnostics
documentationcenter: monitoring-and-diagnostics
ms.assetid: f7457655-ced6-4102-a9dd-7ddf2265c0e2
ms.service: monitoring-and-diagnostics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/23/2016
ms.author: robb
ms.openlocfilehash: 745a9c016bd037f1051025a2c5a468c3935e4550
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="create-metric-alerts-in-azure-monitor-for-azure-services---azure-portal"></a><span data-ttu-id="e1a80-103">Vytvoření metriky výstrah v monitorování Azure pro služby Azure – portál Azure</span><span class="sxs-lookup"><span data-stu-id="e1a80-103">Create metric alerts in Azure Monitor for Azure services - Azure portal</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="e1a80-104">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="e1a80-104">Portal</span></span>](insights-alerts-portal.md)
> * [<span data-ttu-id="e1a80-105">PowerShell</span><span class="sxs-lookup"><span data-stu-id="e1a80-105">PowerShell</span></span>](insights-alerts-powershell.md)
> * [<span data-ttu-id="e1a80-106">Rozhraní příkazového řádku</span><span class="sxs-lookup"><span data-stu-id="e1a80-106">CLI</span></span>](insights-alerts-command-line-interface.md)
>
>

## <a name="overview"></a><span data-ttu-id="e1a80-107">Přehled</span><span class="sxs-lookup"><span data-stu-id="e1a80-107">Overview</span></span>
<span data-ttu-id="e1a80-108">Tento článek ukazuje, jak nastavit Azure metriky výstrah pomocí portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="e1a80-108">This article shows you how to set up Azure metric alerts using the Azure portal.</span></span>   

<span data-ttu-id="e1a80-109">Můžete zobrazit upozornění na základě monitorování metriky pro nebo událostí na služeb Azure.</span><span class="sxs-lookup"><span data-stu-id="e1a80-109">You can receive an alert based on monitoring metrics for, or events on, your Azure services.</span></span>

* <span data-ttu-id="e1a80-110">**Metriky hodnoty** -výstrahy aktivuje, když hodnota zadané metriky překračuje prahovou hodnotu přiřadíte v obou směrech.</span><span class="sxs-lookup"><span data-stu-id="e1a80-110">**Metric values** - The alert triggers when the value of a specified metric crosses a threshold you assign in either direction.</span></span> <span data-ttu-id="e1a80-111">To znamená, aktivuje obě při nejprve je splněna podmínka, a pak později, pokud podmínka je už plněny.</span><span class="sxs-lookup"><span data-stu-id="e1a80-111">That is, it triggers both when the condition is first met and then afterwards when that condition is no longer being met.</span></span>    
* <span data-ttu-id="e1a80-112">**Události protokolu aktivit** -výstrahu můžete aktivovat pro *každých* události nebo pouze tehdy, když dojde k určité události.</span><span class="sxs-lookup"><span data-stu-id="e1a80-112">**Activity log events** - An alert can trigger on *every* event, or, only when a certain events occurs.</span></span> <span data-ttu-id="e1a80-113">Další informace o výstrahách aktivity protokolu [, klikněte sem](monitoring-activity-log-alerts.md)</span><span class="sxs-lookup"><span data-stu-id="e1a80-113">To learn more about activity log alerts [click here](monitoring-activity-log-alerts.md)</span></span>

<span data-ttu-id="e1a80-114">Můžete nakonfigurovat metriky výstrahu při aktivaci, proveďte následující:</span><span class="sxs-lookup"><span data-stu-id="e1a80-114">You can configure a metric alert to do the following when it triggers:</span></span>

* <span data-ttu-id="e1a80-115">odesílat oznámení e-mailu Správce služeb a spolusprávci</span><span class="sxs-lookup"><span data-stu-id="e1a80-115">send email notifications to the service administrator and co-administrators</span></span>
* <span data-ttu-id="e1a80-116">Odeslat e-mail na další e-mailů, které zadáte.</span><span class="sxs-lookup"><span data-stu-id="e1a80-116">send email to additional emails that you specify.</span></span>
* <span data-ttu-id="e1a80-117">Volat webhook, jehož</span><span class="sxs-lookup"><span data-stu-id="e1a80-117">call a webhook</span></span>
* <span data-ttu-id="e1a80-118">Spusťte provádění runbook služby Azure (pouze z portálu Azure)</span><span class="sxs-lookup"><span data-stu-id="e1a80-118">start execution of an Azure runbook (only from the Azure portal)</span></span>

<span data-ttu-id="e1a80-119">Můžete nakonfigurovat a získat informace o použití metriky pravidla výstrah</span><span class="sxs-lookup"><span data-stu-id="e1a80-119">You can configure and get information about metric alert rules using</span></span>

* [<span data-ttu-id="e1a80-120">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="e1a80-120">Azure portal</span></span>](insights-alerts-portal.md)
* [<span data-ttu-id="e1a80-121">PowerShell</span><span class="sxs-lookup"><span data-stu-id="e1a80-121">PowerShell</span></span>](insights-alerts-powershell.md)
* [<span data-ttu-id="e1a80-122">rozhraní příkazového řádku (CLI)</span><span class="sxs-lookup"><span data-stu-id="e1a80-122">command-line interface (CLI)</span></span>](insights-alerts-command-line-interface.md)
* [<span data-ttu-id="e1a80-123">Rozhraní API REST Azure monitorování</span><span class="sxs-lookup"><span data-stu-id="e1a80-123">Azure Monitor REST API</span></span>](https://msdn.microsoft.com/library/azure/dn931945.aspx)

## <a name="create-an-alert-rule-on-a-metric-with-the-azure-portal"></a><span data-ttu-id="e1a80-124">Vytvořit pravidlo výstrahy na metrika pomocí portálu Azure</span><span class="sxs-lookup"><span data-stu-id="e1a80-124">Create an alert rule on a metric with the Azure portal</span></span>
1. <span data-ttu-id="e1a80-125">V [portál](https://portal.azure.com/), vyhledejte prostředků, které vás zajímají monitorování a vyberte ho.</span><span class="sxs-lookup"><span data-stu-id="e1a80-125">In the [portal](https://portal.azure.com/), locate the resource you are interested in monitoring and select it.</span></span>

2. <span data-ttu-id="e1a80-126">Vyberte **výstrahy** nebo **výstrah pravidla** části monitorování.</span><span class="sxs-lookup"><span data-stu-id="e1a80-126">Select **Alerts** or **Alert rules** under the MONITORING section.</span></span> <span data-ttu-id="e1a80-127">Text a ikona se mohou mírně lišit pro různé prostředky.</span><span class="sxs-lookup"><span data-stu-id="e1a80-127">The text and icon may vary slightly for different resources.</span></span>  

    ![Monitorování](./media/insights-alerts-portal/AlertRulesButton.png)

3. <span data-ttu-id="e1a80-129">Vyberte **přidat upozornění** příkazů a vyplňte příslušná pole.</span><span class="sxs-lookup"><span data-stu-id="e1a80-129">Select the **Add alert** command and fill in the fields.</span></span>

    ![Přidání oznámení](./media/insights-alerts-portal/AddAlertOnlyParamsPage.png)

4. <span data-ttu-id="e1a80-131">**Název** upozornění pravidlo a vyberte **popis**, který také zobrazuje v oznámení e-mailů.</span><span class="sxs-lookup"><span data-stu-id="e1a80-131">**Name** your alert rule, and choose a **Description**, which also shows in notification emails.</span></span>

5. <span data-ttu-id="e1a80-132">Vyberte **metrika** chcete monitorovat, a potom vyberte **podmínku** a **prahová hodnota** hodnoty pro metriku.</span><span class="sxs-lookup"><span data-stu-id="e1a80-132">Select the **Metric** you want to monitor, then choose a **Condition** and **Threshold** value for the metric.</span></span> <span data-ttu-id="e1a80-133">Také si zvolili **období** času, který metriky pravidlo je nutné splnit před výstrahy aktivačních událostí.</span><span class="sxs-lookup"><span data-stu-id="e1a80-133">Also chose the **Period** of time that the metric rule must be satisfied before the alert triggers.</span></span> <span data-ttu-id="e1a80-134">Tak například pokud používáte období "PT5M" a upozornění hledá procesoru vyšší než 80 %, výstrahy se aktivuje při procesoru bylo konzistentně výše 80 % 5 minut.</span><span class="sxs-lookup"><span data-stu-id="e1a80-134">So for example, if you use the period "PT5M" and your alert looks for CPU above 80%, the alert triggers when the CPU has been consistently above 80% for 5 minutes.</span></span> <span data-ttu-id="e1a80-135">Jakmile dojde k první aktivační událost, se znovu spustí, když procesoru zůstává nižší než 80 % 5 minut.</span><span class="sxs-lookup"><span data-stu-id="e1a80-135">Once the first trigger occurs, it again triggers when the CPU stays below 80% for 5 minutes.</span></span> <span data-ttu-id="e1a80-136">Procesor měření dochází každých 1 minuta.</span><span class="sxs-lookup"><span data-stu-id="e1a80-136">The CPU measurement occurs every 1 minute.</span></span>   

6. <span data-ttu-id="e1a80-137">Zkontrolujte **e-mailu vlastníky...**  Pokud chcete, aby správci a spolusprávci se má při aktivuje výstrahu.</span><span class="sxs-lookup"><span data-stu-id="e1a80-137">Check **Email owners...** if you want administrators and co-administrators to be emailed when the alert fires.</span></span>

7. <span data-ttu-id="e1a80-138">Pokud chcete další e-mailů příjmu oznámení, když se aktivuje výstraha, přidejte je do **email(s) další správce** pole.</span><span class="sxs-lookup"><span data-stu-id="e1a80-138">If you want additional emails to receive a notification when the alert fires, add them in the **Additional Administrator email(s)** field.</span></span> <span data-ttu-id="e1a80-139">Oddělte více e-mailů středníky -  *email@contoso.com;email2@contoso.com*</span><span class="sxs-lookup"><span data-stu-id="e1a80-139">Separate multiple emails with semi-colons - *email@contoso.com;email2@contoso.com*</span></span>

8. <span data-ttu-id="e1a80-140">Umístit do platný identifikátor URI, **Webhooku** pole, pokud chcete, aby je volána, když se aktivuje výstrahu.</span><span class="sxs-lookup"><span data-stu-id="e1a80-140">Put in a valid URI in the **Webhook** field if you want it called when the alert fires.</span></span>

9. <span data-ttu-id="e1a80-141">Pokud používáte Azure Automation, můžete vybrat Runbook a spustit, když se aktivuje výstrahu.</span><span class="sxs-lookup"><span data-stu-id="e1a80-141">If you use Azure Automation, you can select a Runbook to be run when the alert fires.</span></span>

10. <span data-ttu-id="e1a80-142">Vyberte **OK** po dokončení vytvoření výstrahy.</span><span class="sxs-lookup"><span data-stu-id="e1a80-142">Select **OK** when done to create the alert.</span></span>   

<span data-ttu-id="e1a80-143">Během několika minut výstraha je aktivní a jak se popisuje výš se aktivuje.</span><span class="sxs-lookup"><span data-stu-id="e1a80-143">Within a few minutes, the alert is active and triggers as previously described.</span></span>

## <a name="managing-your-alerts"></a><span data-ttu-id="e1a80-144">Správa upozornění</span><span class="sxs-lookup"><span data-stu-id="e1a80-144">Managing your alerts</span></span>
<span data-ttu-id="e1a80-145">Po vytvoření výstrahy, můžete ji vybrat a:</span><span class="sxs-lookup"><span data-stu-id="e1a80-145">Once you have created an alert, you can select it and:</span></span>

* <span data-ttu-id="e1a80-146">Zobrazte graf zobrazující prahovou hodnotu metriky a skutečnými hodnotami z předchozí den.</span><span class="sxs-lookup"><span data-stu-id="e1a80-146">View a graph showing the metric threshold and the actual values from the previous day.</span></span>
* <span data-ttu-id="e1a80-147">Upravit nebo odstranit.</span><span class="sxs-lookup"><span data-stu-id="e1a80-147">Edit or delete it.</span></span>
* <span data-ttu-id="e1a80-148">**Zakázat** nebo **povolit** ji, pokud chcete dočasně zastavit nebo obnovit příjem oznámení pro výstrahy.</span><span class="sxs-lookup"><span data-stu-id="e1a80-148">**Disable** or **Enable** it if you want to temporarily stop or resume receiving notifications for that alert.</span></span>

## <a name="next-steps"></a><span data-ttu-id="e1a80-149">Další kroky</span><span class="sxs-lookup"><span data-stu-id="e1a80-149">Next steps</span></span>
* <span data-ttu-id="e1a80-150">[Získat přehled o Azure monitorování](monitoring-overview.md) včetně typy informací, můžete sledovat a shromažďovat.</span><span class="sxs-lookup"><span data-stu-id="e1a80-150">[Get an overview of Azure monitoring](monitoring-overview.md) including the types of information you can collect and monitor.</span></span>
* <span data-ttu-id="e1a80-151">Další informace o [konfigurace webhooky ve výstrahách](insights-webhooks-alerts.md).</span><span class="sxs-lookup"><span data-stu-id="e1a80-151">Learn more about [configuring webhooks in alerts](insights-webhooks-alerts.md).</span></span>
* <span data-ttu-id="e1a80-152">Další informace o [konfigurace výstrah pro aktivitu protokolu události](monitoring-activity-log-alerts.md).</span><span class="sxs-lookup"><span data-stu-id="e1a80-152">Learn more about [configuring alerts on Activity log events](monitoring-activity-log-alerts.md).</span></span>
* <span data-ttu-id="e1a80-153">Další informace o [sad Azure Automation Runbook](../automation/automation-starting-a-runbook.md).</span><span class="sxs-lookup"><span data-stu-id="e1a80-153">Learn more about [Azure Automation Runbooks](../automation/automation-starting-a-runbook.md).</span></span>
* <span data-ttu-id="e1a80-154">Získat [přehled diagnostické protokoly](monitoring-overview-of-diagnostic-logs.md) a shromažďovat podrobné metriky vysoká frekvence vaší služby.</span><span class="sxs-lookup"><span data-stu-id="e1a80-154">Get an [overview of diagnostic logs](monitoring-overview-of-diagnostic-logs.md) and collect detailed high-frequency metrics on your service.</span></span>
* <span data-ttu-id="e1a80-155">Získat [přehled metriky kolekce](insights-how-to-customize-monitoring.md) zkontrolovat služby je k dispozici a dobře reagovaly.</span><span class="sxs-lookup"><span data-stu-id="e1a80-155">Get an [overview of metrics collection](insights-how-to-customize-monitoring.md) to make sure your service is available and responsive.</span></span>
