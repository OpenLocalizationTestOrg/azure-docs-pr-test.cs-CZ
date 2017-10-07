---
title: "ukázka výstrahy akce aaaWebhook v OMS Log Analytics | Microsoft Docs"
description: "Jednu z akcí hello můžete spustit v odpovědi tooa výstraha analýzy protokolů * webhooku *, což vám umožní tooinvoke externího procesu prostřednictvím jedné žádosti HTTP. Tento článek vás provede příklad vytvoření webhooku akce výstrahou analýzy protokolů pomocí Slack."
services: log-analytics
documentationcenter: 
author: bwren
manager: jwhit
editor: tysonn
ms.assetid: 13c39f0f-fd3c-472d-8324-ddf7538be45e
ms.service: log-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/27/2017
ms.author: bwren
ms.openlocfilehash: e60bdc4922347073d572c2e4719461b13e8e7d1c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-an-alert-webhook-action-in-oms-log-analytics-toosend-message-tooslack"></a><span data-ttu-id="8a22c-104">Vytvoří akci, výstrah webhooku v tooSlack zpráva toosend OMS analýzy protokolů</span><span class="sxs-lookup"><span data-stu-id="8a22c-104">Create an alert webhook action in OMS Log Analytics toosend message tooSlack</span></span>
<span data-ttu-id="8a22c-105">Jednu z akcí hello můžete spustit v odpovědi tooa [analýzy protokolů výstraha](log-analytics-alerts.md) je *webhooku*, což vám umožní tooinvoke externího procesu prostřednictvím jedné žádosti HTTP.</span><span class="sxs-lookup"><span data-stu-id="8a22c-105">One of hello actions you can run in response tooa [Log Analytics alert](log-analytics-alerts.md) is a *webhook*, which allows you tooinvoke an external process through a single HTTP request.</span></span>  <span data-ttu-id="8a22c-106">Další informace o podrobnosti o výstrahy a pomocí webhooků v [výstrahy v analýzy protokolů](log-analytics-alerts.md)</span><span class="sxs-lookup"><span data-stu-id="8a22c-106">You can read about details of alerts and webhooks in [Alerts in Log Analytics](log-analytics-alerts.md)</span></span>

<span data-ttu-id="8a22c-107">V tomto článku se budeme zabývat příklad vytvoření webhooku akce výstrahou analýzy protokolů pomocí Slack, což je službou zasílání zpráv.</span><span class="sxs-lookup"><span data-stu-id="8a22c-107">In this article, we’ll walk through an example of creating a webhook action in a Log Analytics alert using Slack which is a messaging service.</span></span>

> [!NOTE]
> <span data-ttu-id="8a22c-108">Toocomplete Slack účet musí mít tato ukázka.</span><span class="sxs-lookup"><span data-stu-id="8a22c-108">You must have a Slack account toocomplete this sample.</span></span>  <span data-ttu-id="8a22c-109">Můžete si zaregistrovat bezplatný účet v [slack.com](http://slack.com).</span><span class="sxs-lookup"><span data-stu-id="8a22c-109">You can sign up for a free account at [slack.com](http://slack.com).</span></span>
> 
> 

## <a name="step-1---enable-webhooks-in-slack"></a><span data-ttu-id="8a22c-110">Krok 1 – povolit webhooky v Slack</span><span class="sxs-lookup"><span data-stu-id="8a22c-110">Step 1 - Enable webhooks in Slack</span></span>
1. <span data-ttu-id="8a22c-111">Přihlaste se tooSlack na [slack.com](http://slack.com).</span><span class="sxs-lookup"><span data-stu-id="8a22c-111">Sign in tooSlack at [slack.com](http://slack.com).</span></span>
2. <span data-ttu-id="8a22c-112">Vyberte kanál v hello **kanály** část v levém podokně hello.</span><span class="sxs-lookup"><span data-stu-id="8a22c-112">Select a channel in hello **Channels** section in hello left pane.</span></span>  <span data-ttu-id="8a22c-113">Toto je kanál hello odeslána zpráva hello.</span><span class="sxs-lookup"><span data-stu-id="8a22c-113">This is hello channel that hello message will be sent to.</span></span>  <span data-ttu-id="8a22c-114">Můžete vybrat jednu z hello výchozí kanálů, jako **Obecné** nebo **náhodných**.</span><span class="sxs-lookup"><span data-stu-id="8a22c-114">You can select one of hello default channels such as **general** or **random**.</span></span>  <span data-ttu-id="8a22c-115">V případě produkční by pravděpodobně vytvoříte speciální kanál, jako **criticalservicealerts**.</span><span class="sxs-lookup"><span data-stu-id="8a22c-115">In a production scenario, you would most likely create a special channel such as **criticalservicealerts**.</span></span> <br>
   
   ![Kanálů slack](media/log-analytics-alerts-webhooks/oms-webhooks01.png)
3. <span data-ttu-id="8a22c-117">Klikněte na tlačítko **přidání aplikace nebo vlastní integrace** tooopen hello adresář aplikace.</span><span class="sxs-lookup"><span data-stu-id="8a22c-117">Click **Add an app or custom integration** tooopen hello App Directory.</span></span>
4. <span data-ttu-id="8a22c-118">Typ *webhooky* do vyhledávacího pole hello a potom vyberte **příchozí Webhooky**.</span><span class="sxs-lookup"><span data-stu-id="8a22c-118">Type *webhooks* into hello search box and then select **Incoming WebHooks**.</span></span> <br>
   
   ![Kanálů slack](media/log-analytics-alerts-webhooks/oms-webhooks02.png)
5. <span data-ttu-id="8a22c-120">Klikněte na tlačítko **nainstalovat** další tooyour název týmu.</span><span class="sxs-lookup"><span data-stu-id="8a22c-120">Click **Install** next tooyour team name.</span></span>
6. <span data-ttu-id="8a22c-121">Klikněte na tlačítko **Přidat konfiguraci**.</span><span class="sxs-lookup"><span data-stu-id="8a22c-121">Click **Add Configuration**.</span></span>
7. <span data-ttu-id="8a22c-122">Vyberte hello hello kanál kliknete toouse v tomto příkladu a potom klikněte na **přidat příchozí Webhooky integrace**.</span><span class="sxs-lookup"><span data-stu-id="8a22c-122">Select hello hello channel that you're going toouse for this example, and then click **Add Incoming WebHooks integration**.</span></span>  
8. <span data-ttu-id="8a22c-123">Kopírování hello **adresa URL Webhooku**.</span><span class="sxs-lookup"><span data-stu-id="8a22c-123">Copy hello **Webhook URL**.</span></span>  <span data-ttu-id="8a22c-124">Budete vkládat to do konfigurace výstrah hello.</span><span class="sxs-lookup"><span data-stu-id="8a22c-124">You'll be pasting this into hello Alert configuration.</span></span> <br>
   
    ![Kanálů slack](media/log-analytics-alerts-webhooks/oms-webhooks05.png)

## <a name="step-2---create-alert-rule-in-log-analytics"></a><span data-ttu-id="8a22c-126">Krok 2 – vytvořte pravidlo výstrahy v analýzy protokolů</span><span class="sxs-lookup"><span data-stu-id="8a22c-126">Step 2 - Create alert rule in Log Analytics</span></span>
1. <span data-ttu-id="8a22c-127">[Vytvořit pravidlo výstrahy](log-analytics-alerts.md) s hello následující nastavení.</span><span class="sxs-lookup"><span data-stu-id="8a22c-127">[Create an alert rule](log-analytics-alerts.md) with hello following settings.</span></span>
   * <span data-ttu-id="8a22c-128">Dotaz:```    Type=Event EventLevelName=error ```</span><span class="sxs-lookup"><span data-stu-id="8a22c-128">Query: ```    Type=Event EventLevelName=error ```</span></span>
   * <span data-ttu-id="8a22c-129">Kontrolovat tuto výstrahu každých: 5 minut</span><span class="sxs-lookup"><span data-stu-id="8a22c-129">Check for this alert every: 5 minutes</span></span>
   * <span data-ttu-id="8a22c-130">je Hello počet výsledků: větší než 10</span><span class="sxs-lookup"><span data-stu-id="8a22c-130">hello number of results is: greater than 10</span></span>
   * <span data-ttu-id="8a22c-131">Přes tento časový interval: 60 minut</span><span class="sxs-lookup"><span data-stu-id="8a22c-131">Over this time window: 60 minutes</span></span>
   * <span data-ttu-id="8a22c-132">Vyberte **Ano** pro **Webhooku** a **ne** pro hello dalších akcí.</span><span class="sxs-lookup"><span data-stu-id="8a22c-132">Select **Yes** for **Webhook** and **No** for hello other actions.</span></span>
2. <span data-ttu-id="8a22c-133">Vložení hello Slack adresu URL do hello **adresa URL Webhooku** pole.</span><span class="sxs-lookup"><span data-stu-id="8a22c-133">Paste hello Slack URL into hello **Webhook URL** field.</span></span>
3. <span data-ttu-id="8a22c-134">Vyberte možnost hello příliš**zahrnout vlastní datovou část JSON**.</span><span class="sxs-lookup"><span data-stu-id="8a22c-134">Select hello option too**include a custom JSON payload**.</span></span>
4. <span data-ttu-id="8a22c-135">Zvětralého očekává datové části ve formátu JSON s parametrem s názvem *text*.</span><span class="sxs-lookup"><span data-stu-id="8a22c-135">Slack expects a payload formatted in JSON with a parameter named *text*.</span></span>  <span data-ttu-id="8a22c-136">Toto je text hello, který se zobrazí v uvítací zprávu, kterou vytvoří.</span><span class="sxs-lookup"><span data-stu-id="8a22c-136">This is hello text that it will display in hello message it creates.</span></span>  <span data-ttu-id="8a22c-137">Můžete použít jeden nebo více výstrah parametrů hello pomocí hello  *#*  symbolů, stejně jako hello následující ukázka.</span><span class="sxs-lookup"><span data-stu-id="8a22c-137">You can use one or more of hello alert parameters using hello *#* symbol such as in hello following example.</span></span>
   
    ```
    {
    "text":"#alertrulename fired with #searchresultcount records which exceeds hello over threshold of #thresholdvalue ."
    }
    ```
   
    ![Příklad datové části JSON](media/log-analytics-alerts-webhooks/oms-webhooks07.png)
5. <span data-ttu-id="8a22c-139">Klikněte na tlačítko **Uložit** pravidlo výstrahy toosave hello.</span><span class="sxs-lookup"><span data-stu-id="8a22c-139">Click **Save** toosave hello alert rule.</span></span>
6. <span data-ttu-id="8a22c-140">Počkejte dostatek času výstrahy toobe vytvořen a zkontrolujte zprávu, která budou podobná následující toohello Slack.</span><span class="sxs-lookup"><span data-stu-id="8a22c-140">Wait sufficient time for an alert toobe created and then check Slack for a message which will be similar toohello following.</span></span>
   
   ![Příklad webhooku v Slack](media/log-analytics-alerts-webhooks/oms-webhooks08.png)

### <a name="advanced-webhook-payload-for-slack"></a><span data-ttu-id="8a22c-142">Rozšířené datové části webhooku pro Slack</span><span class="sxs-lookup"><span data-stu-id="8a22c-142">Advanced webhook payload for Slack</span></span>
<span data-ttu-id="8a22c-143">Můžete upravit hojně příchozí zprávy s Slack.</span><span class="sxs-lookup"><span data-stu-id="8a22c-143">You can extensively customize inbound messages with Slack.</span></span> <span data-ttu-id="8a22c-144">Další informace najdete v tématu [příchozí Webhooky](https://api.slack.com/incoming-webhooks) na webu Slack hello.</span><span class="sxs-lookup"><span data-stu-id="8a22c-144">For more information, see [Incoming Webhooks](https://api.slack.com/incoming-webhooks) on hello Slack website.</span></span> <span data-ttu-id="8a22c-145">Toto je složitější toocreate datové části bohaté zprávu s formátování:</span><span class="sxs-lookup"><span data-stu-id="8a22c-145">Following is a more complex payload toocreate a rich message with formatting:</span></span>

    {
        "attachments": [
            {
                "title":"OMS Alerts Custom Payload",
                "fields": [
                    {
                        "title": "Alert Rule Name",
                        "value": "#alertrulename"},
                    {
                        "title": "Link tooSearchResults",
                        "value": "<#linktosearchresults|OMS Search Results>"},
                    {
                        "title": "Search Interval",
                        "value": "#searchinterval"},
                    {
                        "title": "Threshold Operator",
                        "value": "#thresholdoperator"},
                    {
                        "title": "Threshold Value",
                        "value": "#thresholdvalue"}
                ],
                "color": "#F35A00"
            }
        ]
    }


<span data-ttu-id="8a22c-146">To by vygeneroval zprávu v Slack podobné následující toohello.</span><span class="sxs-lookup"><span data-stu-id="8a22c-146">This would generate a message in Slack similar toohello following.</span></span>

![Příklad zprávy v systému Slack](media/log-analytics-alerts-webhooks/oms-webhooks09.png)

## <a name="summary"></a><span data-ttu-id="8a22c-148">Souhrn</span><span class="sxs-lookup"><span data-stu-id="8a22c-148">Summary</span></span>
<span data-ttu-id="8a22c-149">S pravidlo výstrahy zavedené by měla mít tooSlack zpráva byla odeslána pokaždé, když je splněna kritéria hello.</span><span class="sxs-lookup"><span data-stu-id="8a22c-149">With this alert rule in place, you would have a message sent tooSlack every time hello criteria is met.</span></span>  

<span data-ttu-id="8a22c-150">Toto je pouze jeden z příkladů akci, která můžete vytvořit v odpovědi tooan výstrahy.</span><span class="sxs-lookup"><span data-stu-id="8a22c-150">This is only one example of an action that you can create in response tooan alert.</span></span>  <span data-ttu-id="8a22c-151">Můžete vytvořit webhooku akce, která volá jiné externí služby, akce toostart runbook sady runbook v Azure Automation, nebo e-mailu akce toosend tooyourself e-mailu nebo ostatní příjemci.</span><span class="sxs-lookup"><span data-stu-id="8a22c-151">You could create a webhook action that calls another external service, a runbook action toostart a runbook in Azure Automation, or an email action toosend a mail tooyourself or other recipients.</span></span>   

## <a name="next-steps"></a><span data-ttu-id="8a22c-152">Další kroky</span><span class="sxs-lookup"><span data-stu-id="8a22c-152">Next Steps</span></span>
* <span data-ttu-id="8a22c-153">Další informace o dalších [výstrahy akce v analýzy protokolů](log-analytics-alerts-actions.md) včetně dalších akcí.</span><span class="sxs-lookup"><span data-stu-id="8a22c-153">Learn about other [alert actions in Log Analytics](log-analytics-alerts-actions.md) including other actions.</span></span>


