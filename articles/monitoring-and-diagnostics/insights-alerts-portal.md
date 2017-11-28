---
title: "aaaCreate výstrahy pro služby Azure - portálu Azure | Microsoft Docs"
description: "Při splnění zadané podmínky hello aktivaci e-mailů, oznámení, adresy URL weby volání (webhooky) nebo automatizace."
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
ms.openlocfilehash: 78d862d25255cda9fdfe347329e908a471c39846
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-metric-alerts-in-azure-monitor-for-azure-services---azure-portal"></a><span data-ttu-id="a3bf7-103">Vytvoření metriky výstrah v monitorování Azure pro služby Azure – portál Azure</span><span class="sxs-lookup"><span data-stu-id="a3bf7-103">Create metric alerts in Azure Monitor for Azure services - Azure portal</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="a3bf7-104">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="a3bf7-104">Portal</span></span>](insights-alerts-portal.md)
> * [<span data-ttu-id="a3bf7-105">PowerShell</span><span class="sxs-lookup"><span data-stu-id="a3bf7-105">PowerShell</span></span>](insights-alerts-powershell.md)
> * [<span data-ttu-id="a3bf7-106">Rozhraní příkazového řádku</span><span class="sxs-lookup"><span data-stu-id="a3bf7-106">CLI</span></span>](insights-alerts-command-line-interface.md)
>
>

## <a name="overview"></a><span data-ttu-id="a3bf7-107">Přehled</span><span class="sxs-lookup"><span data-stu-id="a3bf7-107">Overview</span></span>
<span data-ttu-id="a3bf7-108">Tento článek ukazuje, jak hello tooset si Azure metriky výstrah pomocí portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="a3bf7-108">This article shows you how tooset up Azure metric alerts using hello Azure portal.</span></span>   

<span data-ttu-id="a3bf7-109">Můžete zobrazit upozornění na základě monitorování metriky pro nebo událostí na služeb Azure.</span><span class="sxs-lookup"><span data-stu-id="a3bf7-109">You can receive an alert based on monitoring metrics for, or events on, your Azure services.</span></span>

* <span data-ttu-id="a3bf7-110">**Metriky hodnoty** – hello aktivační události výstrahy, když hodnota hello zadaný metriky překračuje prahovou hodnotu přiřadíte v obou směrech.</span><span class="sxs-lookup"><span data-stu-id="a3bf7-110">**Metric values** - hello alert triggers when hello value of a specified metric crosses a threshold you assign in either direction.</span></span> <span data-ttu-id="a3bf7-111">To znamená, aktivuje obě při první je splněna podmínka hello a pak později, pokud podmínky již probíhá není splněna.</span><span class="sxs-lookup"><span data-stu-id="a3bf7-111">That is, it triggers both when hello condition is first met and then afterwards when that condition is no longer being met.</span></span>    
* <span data-ttu-id="a3bf7-112">**Události protokolu aktivit** -výstrahu můžete aktivovat pro *každých* události nebo pouze tehdy, když dojde k určité události.</span><span class="sxs-lookup"><span data-stu-id="a3bf7-112">**Activity log events** - An alert can trigger on *every* event, or, only when a certain events occurs.</span></span> <span data-ttu-id="a3bf7-113">Další informace o protokolu upozornění toolearn [, klikněte sem](monitoring-activity-log-alerts.md)</span><span class="sxs-lookup"><span data-stu-id="a3bf7-113">toolearn more about activity log alerts [click here](monitoring-activity-log-alerts.md)</span></span>

<span data-ttu-id="a3bf7-114">Můžete nakonfigurovat následující hello metriky toodo výstrahy, když ji spustí:</span><span class="sxs-lookup"><span data-stu-id="a3bf7-114">You can configure a metric alert toodo hello following when it triggers:</span></span>

* <span data-ttu-id="a3bf7-115">odesílat oznámení e-mailu, toohello Správce služeb a spolusprávci</span><span class="sxs-lookup"><span data-stu-id="a3bf7-115">send email notifications toohello service administrator and co-administrators</span></span>
* <span data-ttu-id="a3bf7-116">odesílání e-mailů tooadditional e-mailů, které zadáte.</span><span class="sxs-lookup"><span data-stu-id="a3bf7-116">send email tooadditional emails that you specify.</span></span>
* <span data-ttu-id="a3bf7-117">Volat webhook, jehož</span><span class="sxs-lookup"><span data-stu-id="a3bf7-117">call a webhook</span></span>
* <span data-ttu-id="a3bf7-118">Spusťte provádění runbook služby Azure (pouze z hello portál Azure)</span><span class="sxs-lookup"><span data-stu-id="a3bf7-118">start execution of an Azure runbook (only from hello Azure portal)</span></span>

<span data-ttu-id="a3bf7-119">Můžete nakonfigurovat a získat informace o použití metriky pravidla výstrah</span><span class="sxs-lookup"><span data-stu-id="a3bf7-119">You can configure and get information about metric alert rules using</span></span>

* [<span data-ttu-id="a3bf7-120">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="a3bf7-120">Azure portal</span></span>](insights-alerts-portal.md)
* [<span data-ttu-id="a3bf7-121">PowerShell</span><span class="sxs-lookup"><span data-stu-id="a3bf7-121">PowerShell</span></span>](insights-alerts-powershell.md)
* [<span data-ttu-id="a3bf7-122">rozhraní příkazového řádku (CLI)</span><span class="sxs-lookup"><span data-stu-id="a3bf7-122">command-line interface (CLI)</span></span>](insights-alerts-command-line-interface.md)
* [<span data-ttu-id="a3bf7-123">Rozhraní API REST Azure monitorování</span><span class="sxs-lookup"><span data-stu-id="a3bf7-123">Azure Monitor REST API</span></span>](https://msdn.microsoft.com/library/azure/dn931945.aspx)

## <a name="create-an-alert-rule-on-a-metric-with-hello-azure-portal"></a><span data-ttu-id="a3bf7-124">Vytvořit pravidlo výstrahy na metrika s hello portálu Azure</span><span class="sxs-lookup"><span data-stu-id="a3bf7-124">Create an alert rule on a metric with hello Azure portal</span></span>
1. <span data-ttu-id="a3bf7-125">V hello [portál](https://portal.azure.com/), vyhledejte hello prostředků, které vás zajímají monitorování a vyberte ho.</span><span class="sxs-lookup"><span data-stu-id="a3bf7-125">In hello [portal](https://portal.azure.com/), locate hello resource you are interested in monitoring and select it.</span></span>

2. <span data-ttu-id="a3bf7-126">Vyberte **výstrahy** nebo **výstrah pravidla** pod hello části monitorování.</span><span class="sxs-lookup"><span data-stu-id="a3bf7-126">Select **Alerts** or **Alert rules** under hello MONITORING section.</span></span> <span data-ttu-id="a3bf7-127">Ikona a textu Hello se mohou mírně lišit pro různé prostředky.</span><span class="sxs-lookup"><span data-stu-id="a3bf7-127">hello text and icon may vary slightly for different resources.</span></span>  

    ![Monitorování](./media/insights-alerts-portal/AlertRulesButton.png)

3. <span data-ttu-id="a3bf7-129">Vyberte hello **přidat upozornění** příkazů a vyplňte pole hello.</span><span class="sxs-lookup"><span data-stu-id="a3bf7-129">Select hello **Add alert** command and fill in hello fields.</span></span>

    ![Přidání oznámení](./media/insights-alerts-portal/AddAlertOnlyParamsPage.png)

4. <span data-ttu-id="a3bf7-131">**Název** upozornění pravidlo a vyberte **popis**, který také zobrazuje v oznámení e-mailů.</span><span class="sxs-lookup"><span data-stu-id="a3bf7-131">**Name** your alert rule, and choose a **Description**, which also shows in notification emails.</span></span>

5. <span data-ttu-id="a3bf7-132">Vyberte hello **metrika** toomonitor a pak vyberte **podmínku** a **prahová hodnota** hodnotu metriky hello.</span><span class="sxs-lookup"><span data-stu-id="a3bf7-132">Select hello **Metric** you want toomonitor, then choose a **Condition** and **Threshold** value for hello metric.</span></span> <span data-ttu-id="a3bf7-133">Také si zvolili hello **období** času, který hello metrika pravidlo je nutné splnit před hello výstrahy aktivační události.</span><span class="sxs-lookup"><span data-stu-id="a3bf7-133">Also chose hello **Period** of time that hello metric rule must be satisfied before hello alert triggers.</span></span> <span data-ttu-id="a3bf7-134">Tak například pokud používáte období hello "PT5M" a upozornění hledá procesoru vyšší než 80 %, hello výstraha se aktivuje při hello procesoru bylo konzistentně výše 80 % 5 minut.</span><span class="sxs-lookup"><span data-stu-id="a3bf7-134">So for example, if you use hello period "PT5M" and your alert looks for CPU above 80%, hello alert triggers when hello CPU has been consistently above 80% for 5 minutes.</span></span> <span data-ttu-id="a3bf7-135">Jakmile dojde k první aktivační událost hello, se znovu spustí, když hello procesoru zůstává nižší než 80 % 5 minut.</span><span class="sxs-lookup"><span data-stu-id="a3bf7-135">Once hello first trigger occurs, it again triggers when hello CPU stays below 80% for 5 minutes.</span></span> <span data-ttu-id="a3bf7-136">Hello procesoru měření dochází každých 1 minuta.</span><span class="sxs-lookup"><span data-stu-id="a3bf7-136">hello CPU measurement occurs every 1 minute.</span></span>   

6. <span data-ttu-id="a3bf7-137">Zkontrolujte **e-mailu vlastníky...**  Pokud chcete, aby správci a spolusprávci toobe e-mailem při hello výstrahy aktivuje.</span><span class="sxs-lookup"><span data-stu-id="a3bf7-137">Check **Email owners...** if you want administrators and co-administrators toobe emailed when hello alert fires.</span></span>

7. <span data-ttu-id="a3bf7-138">Pokud chcete další e-mailů tooreceive oznámení, když hello výstrahy je aktivována, je přidat v hello **email(s) další správce** pole.</span><span class="sxs-lookup"><span data-stu-id="a3bf7-138">If you want additional emails tooreceive a notification when hello alert fires, add them in hello **Additional Administrator email(s)** field.</span></span> <span data-ttu-id="a3bf7-139">Oddělte více e-mailů středníky -  *email@contoso.com;email2@contoso.com*</span><span class="sxs-lookup"><span data-stu-id="a3bf7-139">Separate multiple emails with semi-colons - *email@contoso.com;email2@contoso.com*</span></span>

8. <span data-ttu-id="a3bf7-140">Umístit do platný identifikátor URI v hello **Webhooku** pole Pokud se má volat při hello výstrahy aktivuje.</span><span class="sxs-lookup"><span data-stu-id="a3bf7-140">Put in a valid URI in hello **Webhook** field if you want it called when hello alert fires.</span></span>

9. <span data-ttu-id="a3bf7-141">Pokud používáte Azure Automation, můžete vybrat Runbook toobe, spustit, když se aktivuje výstraha hello.</span><span class="sxs-lookup"><span data-stu-id="a3bf7-141">If you use Azure Automation, you can select a Runbook toobe run when hello alert fires.</span></span>

10. <span data-ttu-id="a3bf7-142">Vyberte **OK** při done toocreate hello výstrahy.</span><span class="sxs-lookup"><span data-stu-id="a3bf7-142">Select **OK** when done toocreate hello alert.</span></span>   

<span data-ttu-id="a3bf7-143">Během několika minut hello výstraha je aktivní a jak se popisuje výš se aktivuje.</span><span class="sxs-lookup"><span data-stu-id="a3bf7-143">Within a few minutes, hello alert is active and triggers as previously described.</span></span>

## <a name="managing-your-alerts"></a><span data-ttu-id="a3bf7-144">Správa upozornění</span><span class="sxs-lookup"><span data-stu-id="a3bf7-144">Managing your alerts</span></span>
<span data-ttu-id="a3bf7-145">Po vytvoření výstrahy, můžete ji vybrat a:</span><span class="sxs-lookup"><span data-stu-id="a3bf7-145">Once you have created an alert, you can select it and:</span></span>

* <span data-ttu-id="a3bf7-146">Zobrazte graf zobrazující hello metriky prahovou hodnotu a hello skutečnými hodnotami z hello předchozí den.</span><span class="sxs-lookup"><span data-stu-id="a3bf7-146">View a graph showing hello metric threshold and hello actual values from hello previous day.</span></span>
* <span data-ttu-id="a3bf7-147">Upravit nebo odstranit.</span><span class="sxs-lookup"><span data-stu-id="a3bf7-147">Edit or delete it.</span></span>
* <span data-ttu-id="a3bf7-148">**Zakázat** nebo **povolit** ho chcete-li zastavit tootemporarily nebo obnovit příjem oznámení pro výstrahy.</span><span class="sxs-lookup"><span data-stu-id="a3bf7-148">**Disable** or **Enable** it if you want tootemporarily stop or resume receiving notifications for that alert.</span></span>

## <a name="next-steps"></a><span data-ttu-id="a3bf7-149">Další kroky</span><span class="sxs-lookup"><span data-stu-id="a3bf7-149">Next steps</span></span>
* <span data-ttu-id="a3bf7-150">[Získat přehled o Azure monitorování](monitoring-overview.md) včetně hello typy informací, můžete sledovat a shromažďovat.</span><span class="sxs-lookup"><span data-stu-id="a3bf7-150">[Get an overview of Azure monitoring](monitoring-overview.md) including hello types of information you can collect and monitor.</span></span>
* <span data-ttu-id="a3bf7-151">Další informace o [konfigurace webhooky ve výstrahách](insights-webhooks-alerts.md).</span><span class="sxs-lookup"><span data-stu-id="a3bf7-151">Learn more about [configuring webhooks in alerts](insights-webhooks-alerts.md).</span></span>
* <span data-ttu-id="a3bf7-152">Další informace o [konfigurace výstrah pro aktivitu protokolu události](monitoring-activity-log-alerts.md).</span><span class="sxs-lookup"><span data-stu-id="a3bf7-152">Learn more about [configuring alerts on Activity log events](monitoring-activity-log-alerts.md).</span></span>
* <span data-ttu-id="a3bf7-153">Další informace o [sad Azure Automation Runbook](../automation/automation-starting-a-runbook.md).</span><span class="sxs-lookup"><span data-stu-id="a3bf7-153">Learn more about [Azure Automation Runbooks](../automation/automation-starting-a-runbook.md).</span></span>
* <span data-ttu-id="a3bf7-154">Získat [přehled diagnostické protokoly](monitoring-overview-of-diagnostic-logs.md) a shromažďovat podrobné metriky vysoká frekvence vaší služby.</span><span class="sxs-lookup"><span data-stu-id="a3bf7-154">Get an [overview of diagnostic logs](monitoring-overview-of-diagnostic-logs.md) and collect detailed high-frequency metrics on your service.</span></span>
* <span data-ttu-id="a3bf7-155">Získat [přehled metriky kolekce](insights-how-to-customize-monitoring.md) toomake, že je služba dostupná a dobře reagovaly.</span><span class="sxs-lookup"><span data-stu-id="a3bf7-155">Get an [overview of metrics collection](insights-how-to-customize-monitoring.md) toomake sure your service is available and responsive.</span></span>
