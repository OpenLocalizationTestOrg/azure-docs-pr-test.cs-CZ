---
title: "Ukázka výstrahy akce Webhooku v OMS Log Analytics | Microsoft Docs"
description: "Jednu z akcí, můžete spustit v reakci na výstrahy analýzy protokolů je * webhooku *, které umožňuje vyvolání externího procesu prostřednictvím jedné žádosti HTTP. Tento článek vás provede příklad vytvoření webhooku akce výstrahou analýzy protokolů pomocí Slack."
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
ms.openlocfilehash: 55b66132f7ec5c26c0a7cac1ec0a5c403dbd1082
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="create-an-alert-webhook-action-in-oms-log-analytics-to-send-message-to-slack"></a><span data-ttu-id="52aa7-104">Vytvoří akci, výstrah webhooku v OMS analýzy protokolů k odeslání zprávy na Slack</span><span class="sxs-lookup"><span data-stu-id="52aa7-104">Create an alert webhook action in OMS Log Analytics to send message to Slack</span></span>
<span data-ttu-id="52aa7-105">Jednu z akcí můžete spustit v reakci na [analýzy protokolů výstraha](log-analytics-alerts.md) je *webhooku*, která umožňuje vyvolání externího procesu prostřednictvím jedné žádosti HTTP.</span><span class="sxs-lookup"><span data-stu-id="52aa7-105">One of the actions you can run in response to a [Log Analytics alert](log-analytics-alerts.md) is a *webhook*, which allows you to invoke an external process through a single HTTP request.</span></span>  <span data-ttu-id="52aa7-106">Další informace o podrobnosti o výstrahy a pomocí webhooků v [výstrahy v analýzy protokolů](log-analytics-alerts.md)</span><span class="sxs-lookup"><span data-stu-id="52aa7-106">You can read about details of alerts and webhooks in [Alerts in Log Analytics](log-analytics-alerts.md)</span></span>

<span data-ttu-id="52aa7-107">V tomto článku se budeme zabývat příklad vytvoření webhooku akce výstrahou analýzy protokolů pomocí Slack, což je službou zasílání zpráv.</span><span class="sxs-lookup"><span data-stu-id="52aa7-107">In this article, we’ll walk through an example of creating a webhook action in a Log Analytics alert using Slack which is a messaging service.</span></span>

> [!NOTE]
> <span data-ttu-id="52aa7-108">Musíte mít účet Slack k dokončení této ukázce.</span><span class="sxs-lookup"><span data-stu-id="52aa7-108">You must have a Slack account to complete this sample.</span></span>  <span data-ttu-id="52aa7-109">Můžete si zaregistrovat bezplatný účet v [slack.com](http://slack.com).</span><span class="sxs-lookup"><span data-stu-id="52aa7-109">You can sign up for a free account at [slack.com](http://slack.com).</span></span>
> 
> 

## <a name="step-1---enable-webhooks-in-slack"></a><span data-ttu-id="52aa7-110">Krok 1 – povolit webhooky v Slack</span><span class="sxs-lookup"><span data-stu-id="52aa7-110">Step 1 - Enable webhooks in Slack</span></span>
1. <span data-ttu-id="52aa7-111">Přihlaste se k systému Slack v [slack.com](http://slack.com).</span><span class="sxs-lookup"><span data-stu-id="52aa7-111">Sign in to Slack at [slack.com](http://slack.com).</span></span>
2. <span data-ttu-id="52aa7-112">Vyberte kanál v **kanály** část v levém podokně.</span><span class="sxs-lookup"><span data-stu-id="52aa7-112">Select a channel in the **Channels** section in the left pane.</span></span>  <span data-ttu-id="52aa7-113">Toto je kanál, který bude zaslána zpráva.</span><span class="sxs-lookup"><span data-stu-id="52aa7-113">This is the channel that the message will be sent to.</span></span>  <span data-ttu-id="52aa7-114">Můžete vybrat jednu z výchozích kanálů, jako **Obecné** nebo **náhodných**.</span><span class="sxs-lookup"><span data-stu-id="52aa7-114">You can select one of the default channels such as **general** or **random**.</span></span>  <span data-ttu-id="52aa7-115">V případě produkční by pravděpodobně vytvoříte speciální kanál, jako **criticalservicealerts**.</span><span class="sxs-lookup"><span data-stu-id="52aa7-115">In a production scenario, you would most likely create a special channel such as **criticalservicealerts**.</span></span> <br>
   
   ![Kanálů slack](media/log-analytics-alerts-webhooks/oms-webhooks01.png)
3. <span data-ttu-id="52aa7-117">Klikněte na tlačítko **přidání aplikace nebo vlastní integrace** otevřete adresář aplikace.</span><span class="sxs-lookup"><span data-stu-id="52aa7-117">Click **Add an app or custom integration** to open the App Directory.</span></span>
4. <span data-ttu-id="52aa7-118">Typ *webhooky* do vyhledávacího pole a pak vyberte **příchozí Webhooky**.</span><span class="sxs-lookup"><span data-stu-id="52aa7-118">Type *webhooks* into the search box and then select **Incoming WebHooks**.</span></span> <br>
   
   ![Kanálů slack](media/log-analytics-alerts-webhooks/oms-webhooks02.png)
5. <span data-ttu-id="52aa7-120">Klikněte na tlačítko **nainstalovat** vedle vašeho jména týmu.</span><span class="sxs-lookup"><span data-stu-id="52aa7-120">Click **Install** next to your team name.</span></span>
6. <span data-ttu-id="52aa7-121">Klikněte na tlačítko **Přidat konfiguraci**.</span><span class="sxs-lookup"><span data-stu-id="52aa7-121">Click **Add Configuration**.</span></span>
7. <span data-ttu-id="52aa7-122">Vyberte kanálu, který budete používat pro tento příklad, a pak klikněte na **přidat příchozí Webhooky integrace**.</span><span class="sxs-lookup"><span data-stu-id="52aa7-122">Select the the channel that you're going to use for this example, and then click **Add Incoming WebHooks integration**.</span></span>  
8. <span data-ttu-id="52aa7-123">Kopírování **adresa URL Webhooku**.</span><span class="sxs-lookup"><span data-stu-id="52aa7-123">Copy the **Webhook URL**.</span></span>  <span data-ttu-id="52aa7-124">Budete vkládat to do konfigurace výstrahy.</span><span class="sxs-lookup"><span data-stu-id="52aa7-124">You'll be pasting this into the Alert configuration.</span></span> <br>
   
    ![Kanálů slack](media/log-analytics-alerts-webhooks/oms-webhooks05.png)

## <a name="step-2---create-alert-rule-in-log-analytics"></a><span data-ttu-id="52aa7-126">Krok 2 – vytvořte pravidlo výstrahy v analýzy protokolů</span><span class="sxs-lookup"><span data-stu-id="52aa7-126">Step 2 - Create alert rule in Log Analytics</span></span>
1. <span data-ttu-id="52aa7-127">[Vytvořit pravidlo výstrahy](log-analytics-alerts.md) s následujícím nastavením.</span><span class="sxs-lookup"><span data-stu-id="52aa7-127">[Create an alert rule](log-analytics-alerts.md) with the following settings.</span></span>
   * <span data-ttu-id="52aa7-128">Dotaz:```    Type=Event EventLevelName=error ```</span><span class="sxs-lookup"><span data-stu-id="52aa7-128">Query: ```    Type=Event EventLevelName=error ```</span></span>
   * <span data-ttu-id="52aa7-129">Kontrolovat tuto výstrahu každých: 5 minut</span><span class="sxs-lookup"><span data-stu-id="52aa7-129">Check for this alert every: 5 minutes</span></span>
   * <span data-ttu-id="52aa7-130">Počet výsledků je: větší než 10</span><span class="sxs-lookup"><span data-stu-id="52aa7-130">The number of results is: greater than 10</span></span>
   * <span data-ttu-id="52aa7-131">Přes tento časový interval: 60 minut</span><span class="sxs-lookup"><span data-stu-id="52aa7-131">Over this time window: 60 minutes</span></span>
   * <span data-ttu-id="52aa7-132">Vyberte **Ano** pro **Webhooku** a **ne** pro další akce.</span><span class="sxs-lookup"><span data-stu-id="52aa7-132">Select **Yes** for **Webhook** and **No** for the other actions.</span></span>
2. <span data-ttu-id="52aa7-133">Vložte adresu URL Slack do **adresa URL Webhooku** pole.</span><span class="sxs-lookup"><span data-stu-id="52aa7-133">Paste the Slack URL into the **Webhook URL** field.</span></span>
3. <span data-ttu-id="52aa7-134">Vyberte možnost **zahrnout vlastní datovou část JSON**.</span><span class="sxs-lookup"><span data-stu-id="52aa7-134">Select the option to **include a custom JSON payload**.</span></span>
4. <span data-ttu-id="52aa7-135">Zvětralého očekává datové části ve formátu JSON s parametrem s názvem *text*.</span><span class="sxs-lookup"><span data-stu-id="52aa7-135">Slack expects a payload formatted in JSON with a parameter named *text*.</span></span>  <span data-ttu-id="52aa7-136">Toto je text, který se zobrazí ve zprávě, kterou vytvoří.</span><span class="sxs-lookup"><span data-stu-id="52aa7-136">This is the text that it will display in the message it creates.</span></span>  <span data-ttu-id="52aa7-137">Můžete použít jeden nebo více parametrů výstrah pomocí  *#*  symbolů, jako v následujícím příkladu.</span><span class="sxs-lookup"><span data-stu-id="52aa7-137">You can use one or more of the alert parameters using the *#* symbol such as in the following example.</span></span>
   
    ```
    {
    "text":"#alertrulename fired with #searchresultcount records which exceeds the over threshold of #thresholdvalue ."
    }
    ```
   
    ![Příklad datové části JSON](media/log-analytics-alerts-webhooks/oms-webhooks07.png)
5. <span data-ttu-id="52aa7-139">Klikněte na tlačítko **Uložit** uložte pravidlo výstrahy.</span><span class="sxs-lookup"><span data-stu-id="52aa7-139">Click **Save** to save the alert rule.</span></span>
6. <span data-ttu-id="52aa7-140">Počkejte, než dostatek času pro výstrahu určenou k vytvořit a potom zkontrolujte Slack pro zprávu, která bude podobný následujícímu.</span><span class="sxs-lookup"><span data-stu-id="52aa7-140">Wait sufficient time for an alert to be created and then check Slack for a message which will be similar to the following.</span></span>
   
   ![Příklad webhooku v Slack](media/log-analytics-alerts-webhooks/oms-webhooks08.png)

### <a name="advanced-webhook-payload-for-slack"></a><span data-ttu-id="52aa7-142">Rozšířené datové části webhooku pro Slack</span><span class="sxs-lookup"><span data-stu-id="52aa7-142">Advanced webhook payload for Slack</span></span>
<span data-ttu-id="52aa7-143">Můžete upravit hojně příchozí zprávy s Slack.</span><span class="sxs-lookup"><span data-stu-id="52aa7-143">You can extensively customize inbound messages with Slack.</span></span> <span data-ttu-id="52aa7-144">Další informace najdete v tématu [příchozí Webhooky](https://api.slack.com/incoming-webhooks) na webu Slack.</span><span class="sxs-lookup"><span data-stu-id="52aa7-144">For more information, see [Incoming Webhooks](https://api.slack.com/incoming-webhooks) on the Slack website.</span></span> <span data-ttu-id="52aa7-145">Toto je složitější datové části pro vytvoření bohaté zprávy s formátování:</span><span class="sxs-lookup"><span data-stu-id="52aa7-145">Following is a more complex payload to create a rich message with formatting:</span></span>

    {
        "attachments": [
            {
                "title":"OMS Alerts Custom Payload",
                "fields": [
                    {
                        "title": "Alert Rule Name",
                        "value": "#alertrulename"},
                    {
                        "title": "Link To SearchResults",
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


<span data-ttu-id="52aa7-146">To by vygeneroval zprávu v systému Slack podobný následujícímu.</span><span class="sxs-lookup"><span data-stu-id="52aa7-146">This would generate a message in Slack similar to the following.</span></span>

![Příklad zprávy v systému Slack](media/log-analytics-alerts-webhooks/oms-webhooks09.png)

## <a name="summary"></a><span data-ttu-id="52aa7-148">Souhrn</span><span class="sxs-lookup"><span data-stu-id="52aa7-148">Summary</span></span>
<span data-ttu-id="52aa7-149">S pravidlo výstrahy na místě budete mít zprávy odeslané k systému Slack pokaždé, když je splněna kritéria.</span><span class="sxs-lookup"><span data-stu-id="52aa7-149">With this alert rule in place, you would have a message sent to Slack every time the criteria is met.</span></span>  

<span data-ttu-id="52aa7-150">Toto je pouze jeden z příkladů akci, která můžete vytvořit v reakci na výstrahy.</span><span class="sxs-lookup"><span data-stu-id="52aa7-150">This is only one example of an action that you can create in response to an alert.</span></span>  <span data-ttu-id="52aa7-151">Můžete vytvořit webhooku akce, která volá jinou službu externí, runbook akci pro spuštění sady runbook ve službě Azure Automation nebo e-mailu akci, kterou chcete odeslat e-mail s sobě nebo ostatní příjemci.</span><span class="sxs-lookup"><span data-stu-id="52aa7-151">You could create a webhook action that calls another external service, a runbook action to start a runbook in Azure Automation, or an email action to send a mail to yourself or other recipients.</span></span>   

## <a name="next-steps"></a><span data-ttu-id="52aa7-152">Další kroky</span><span class="sxs-lookup"><span data-stu-id="52aa7-152">Next Steps</span></span>
* <span data-ttu-id="52aa7-153">Další informace o dalších [výstrahy akce v analýzy protokolů](log-analytics-alerts-actions.md) včetně dalších akcí.</span><span class="sxs-lookup"><span data-stu-id="52aa7-153">Learn about other [alert actions in Log Analytics](log-analytics-alerts-actions.md) including other actions.</span></span>


