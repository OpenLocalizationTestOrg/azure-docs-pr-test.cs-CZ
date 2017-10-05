---
title: "Odpovědi na výstrahy v OMS Log Analytics | Microsoft Docs"
description: "Výstrahy v Log Analytics identifikovat důležité informace ve svém úložišti OMS a můžete proaktivně upozorňují na problémy nebo vyvolání akce se pokusit o opravte je.  Tento článek popisuje, jak vytvořit pravidlo výstrahy a podrobnosti o různé akce, které jejich zajištění může trvat."
services: log-analytics
documentationcenter: 
author: bwren
manager: jwhit
editor: tysonn
ms.assetid: 6cfd2a46-b6a2-4f79-a67b-08ce488f9a91
ms.service: log-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/28/2017
ms.author: bwren
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: b8731e1fe48b7d809b113eb5273e3962542b8f34
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="add-actions-to-alert-rules-in-log-analytics"></a><span data-ttu-id="c5e65-104">Přidání akce do pravidla výstrah v analýzy protokolů</span><span class="sxs-lookup"><span data-stu-id="c5e65-104">Add actions to alert rules in Log Analytics</span></span>
<span data-ttu-id="c5e65-105">Když [v analýzy protokolů se vytvoří výstraha](log-analytics-alerts.md), máte možnost [konfigurace pravidlo výstrahy](log-analytics-alerts.md) provést několik akcí.</span><span class="sxs-lookup"><span data-stu-id="c5e65-105">When an [alert is created in Log Analytics](log-analytics-alerts.md), you have the option of [configuring the alert rule](log-analytics-alerts.md) to perform one or more actions.</span></span>  <span data-ttu-id="c5e65-106">Tento článek popisuje různé akce, které jsou k dispozici a podrobnosti o konfiguraci jednotlivých typů.</span><span class="sxs-lookup"><span data-stu-id="c5e65-106">This article describes the different actions that are available and details on configuring each kind.</span></span>

| <span data-ttu-id="c5e65-107">Akce</span><span class="sxs-lookup"><span data-stu-id="c5e65-107">Action</span></span> | <span data-ttu-id="c5e65-108">Popis</span><span class="sxs-lookup"><span data-stu-id="c5e65-108">Description</span></span> |
|:--|:--|
| [<span data-ttu-id="c5e65-109">E-mailu</span><span class="sxs-lookup"><span data-stu-id="c5e65-109">Email</span></span>](#email-actions) | <span data-ttu-id="c5e65-110">Jeden nebo více příjemcům odeslat e-mail s detaily výstrahy.</span><span class="sxs-lookup"><span data-stu-id="c5e65-110">Send an e-mail with the details of the alert to one or more recipients.</span></span> |
| [<span data-ttu-id="c5e65-111">Webhooku</span><span class="sxs-lookup"><span data-stu-id="c5e65-111">Webhook</span></span>](#webhook-actions) | <span data-ttu-id="c5e65-112">Vyvolání externího procesu prostřednictvím jedné žádosti HTTP POST.</span><span class="sxs-lookup"><span data-stu-id="c5e65-112">Invoke an external process through a single HTTP POST request.</span></span> |
| [<span data-ttu-id="c5e65-113">Sady Runbook</span><span class="sxs-lookup"><span data-stu-id="c5e65-113">Runbook</span></span>](#runbook-actions) | <span data-ttu-id="c5e65-114">Spuštění sady runbook ve službě Azure Automation.</span><span class="sxs-lookup"><span data-stu-id="c5e65-114">Start a runbook in Azure Automation.</span></span> |


## <a name="email-actions"></a><span data-ttu-id="c5e65-115">Akce e-mailu</span><span class="sxs-lookup"><span data-stu-id="c5e65-115">Email actions</span></span>
<span data-ttu-id="c5e65-116">E-mailu akce odeslání e-mailu s detaily výstrahy jednoho nebo více příjemců.</span><span class="sxs-lookup"><span data-stu-id="c5e65-116">Email actions send an e-mail with the details of the alert to one or more recipients.</span></span>  <span data-ttu-id="c5e65-117">Můžete zadat předmět e-mailu, ale jeho obsah je standardní formát vytvořená pomocí analýzy protokolů.</span><span class="sxs-lookup"><span data-stu-id="c5e65-117">You can specify the subject of the mail, but it's content is a standard format constructed by Log Analytics.</span></span>  <span data-ttu-id="c5e65-118">Obsahuje souhrnné informace, jako je název výstrahy kromě podrobností až deset záznamů protokolu nalezené.</span><span class="sxs-lookup"><span data-stu-id="c5e65-118">It includes summary information such as the name of the alert in addition to details of up to ten records returned by the log search.</span></span>  <span data-ttu-id="c5e65-119">Zahrnuje také odkaz k hledání protokolů v analýzy protokolů, který vrátí celou sadu záznamů z tohoto dotazu.</span><span class="sxs-lookup"><span data-stu-id="c5e65-119">It also includes a link to a log search in Log Analytics that will return the entire set of records from that query.</span></span>   <span data-ttu-id="c5e65-120">Odesílatelem e-mailu je *týmu Microsoft Operations Management Suite &lt; noreply@oms.microsoft.com &gt;* .</span><span class="sxs-lookup"><span data-stu-id="c5e65-120">The sender of the mail is *Microsoft Operations Management Suite Team &lt;noreply@oms.microsoft.com&gt;*.</span></span> 

<span data-ttu-id="c5e65-121">E-mailu akce vyžadují vlastnosti v následující tabulce.</span><span class="sxs-lookup"><span data-stu-id="c5e65-121">Email actions require the properties in the following table.</span></span>

| <span data-ttu-id="c5e65-122">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="c5e65-122">Property</span></span> | <span data-ttu-id="c5e65-123">Popis</span><span class="sxs-lookup"><span data-stu-id="c5e65-123">Description</span></span> |
|:--- |:--- |
| <span data-ttu-id="c5e65-124">Předmět</span><span class="sxs-lookup"><span data-stu-id="c5e65-124">Subject</span></span> |<span data-ttu-id="c5e65-125">Subjektu v e-mailu.</span><span class="sxs-lookup"><span data-stu-id="c5e65-125">Subject in the email.</span></span>  <span data-ttu-id="c5e65-126">Tělo e-mailu se nedá změnit.</span><span class="sxs-lookup"><span data-stu-id="c5e65-126">You cannot modify the body of the mail.</span></span> |
| <span data-ttu-id="c5e65-127">Příjemce</span><span class="sxs-lookup"><span data-stu-id="c5e65-127">Recipients</span></span> |<span data-ttu-id="c5e65-128">Adresy všech příjemců e-mailu.</span><span class="sxs-lookup"><span data-stu-id="c5e65-128">Addresses of all e-mail recipients.</span></span>  <span data-ttu-id="c5e65-129">Pokud zadáte víc než jednou adresou, jednotlivé adresy oddělujte středníkem (;).</span><span class="sxs-lookup"><span data-stu-id="c5e65-129">If you specify more than one address, then separate the addresses with a semicolon (;).</span></span> |


## <a name="webhook-actions"></a><span data-ttu-id="c5e65-130">Akce Webhooku</span><span class="sxs-lookup"><span data-stu-id="c5e65-130">Webhook actions</span></span>

<span data-ttu-id="c5e65-131">Akce Webhooku umožňují vyvolání externího procesu prostřednictvím jedné žádosti HTTP POST.</span><span class="sxs-lookup"><span data-stu-id="c5e65-131">Webhook actions allow you to invoke an external process through a single HTTP POST request.</span></span>  <span data-ttu-id="c5e65-132">Služba volané by měla podporovat webhooky a určit, jak se bude používat žádné datové části obdrží.</span><span class="sxs-lookup"><span data-stu-id="c5e65-132">The service being called should support webhooks and determine how it will use any payload it receives.</span></span>  <span data-ttu-id="c5e65-133">Může také zavolat REST API, která nepodporuje konkrétně webhooků, pokud je požadavek ve formátu, který funguje s technologií rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="c5e65-133">You could also call a REST API that doesn't specifically support webhooks as long as the request is in a format that the API understands.</span></span>  <span data-ttu-id="c5e65-134">Příklady použití webhook, jehož v reakci na výstrahy jsou odesílání zprávy v [Slack](http://slack.com) nebo vytváření incidentu v [PagerDuty](http://pagerduty.com/).</span><span class="sxs-lookup"><span data-stu-id="c5e65-134">Examples of using a webhook in response to an alert are sending a message in [Slack](http://slack.com) or creating an incident in [PagerDuty](http://pagerduty.com/).</span></span>  <span data-ttu-id="c5e65-135">Kompletní a podrobný postup vytvoření pravidla výstrahy s webhooku pro volání Slack je k dispozici na [Webhooky ve výstrahách analýzy protokolů](log-analytics-alerts-webhooks.md).</span><span class="sxs-lookup"><span data-stu-id="c5e65-135">A complete walkthrough of creating an alert rule with a webhook to call Slack is available at [Webhooks in Log Analytics alerts](log-analytics-alerts-webhooks.md).</span></span>

<span data-ttu-id="c5e65-136">Akce Webhooku vyžadují vlastnosti v následující tabulce.</span><span class="sxs-lookup"><span data-stu-id="c5e65-136">Webhook actions require the properties in the following table.</span></span>

| <span data-ttu-id="c5e65-137">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="c5e65-137">Property</span></span> | <span data-ttu-id="c5e65-138">Popis</span><span class="sxs-lookup"><span data-stu-id="c5e65-138">Description</span></span> |
|:--- |:--- |
| <span data-ttu-id="c5e65-139">Adresa URL Webhooku</span><span class="sxs-lookup"><span data-stu-id="c5e65-139">Webhook URL</span></span> |<span data-ttu-id="c5e65-140">Adresa URL webhooku.</span><span class="sxs-lookup"><span data-stu-id="c5e65-140">The URL of the webhook.</span></span> |
| <span data-ttu-id="c5e65-141">Vlastní datovou část JSON</span><span class="sxs-lookup"><span data-stu-id="c5e65-141">Custom JSON payload</span></span> |<span data-ttu-id="c5e65-142">Vlastní datovou část odeslat spolu s webhooku.</span><span class="sxs-lookup"><span data-stu-id="c5e65-142">Custom payload to send with the webhook.</span></span>  <span data-ttu-id="c5e65-143">Níže naleznete podrobnosti.</span><span class="sxs-lookup"><span data-stu-id="c5e65-143">See below for details.</span></span> |


<span data-ttu-id="c5e65-144">Webhooky zahrnují adresu URL a datovou část ve formátu JSON, který se data odesílají externí služby.</span><span class="sxs-lookup"><span data-stu-id="c5e65-144">Webhooks include a URL and a payload formatted in JSON that is the data sent to the external service.</span></span>  <span data-ttu-id="c5e65-145">Ve výchozím nastavení budou datové části zahrnují hodnoty v následující tabulce.</span><span class="sxs-lookup"><span data-stu-id="c5e65-145">By default, the payload will include the values in the following table.</span></span>  <span data-ttu-id="c5e65-146">Můžete nahradit vlastní jeden vlastní tato datová část.</span><span class="sxs-lookup"><span data-stu-id="c5e65-146">You can choose to replace this payload with a custom one of your own.</span></span>  <span data-ttu-id="c5e65-147">V takovém případě můžete proměnné v tabulce pro jednotlivé parametry mají být zahrnuty jejich hodnota vaše vlastní datovou část.</span><span class="sxs-lookup"><span data-stu-id="c5e65-147">In that case you can use the variables in the table for each of the parameters to include their value in your custom payload.</span></span>

| <span data-ttu-id="c5e65-148">Parametr</span><span class="sxs-lookup"><span data-stu-id="c5e65-148">Parameter</span></span> | <span data-ttu-id="c5e65-149">Proměnná</span><span class="sxs-lookup"><span data-stu-id="c5e65-149">Variable</span></span> | <span data-ttu-id="c5e65-150">Popis</span><span class="sxs-lookup"><span data-stu-id="c5e65-150">Description</span></span> |
|:--- |:--- |:--- |
| <span data-ttu-id="c5e65-151">AlertRuleName</span><span class="sxs-lookup"><span data-stu-id="c5e65-151">AlertRuleName</span></span> |<span data-ttu-id="c5e65-152">#alertrulename</span><span class="sxs-lookup"><span data-stu-id="c5e65-152">#alertrulename</span></span> |<span data-ttu-id="c5e65-153">Název pravidla výstrahy.</span><span class="sxs-lookup"><span data-stu-id="c5e65-153">Name of the alert rule.</span></span> |
| <span data-ttu-id="c5e65-154">AlertThresholdOperator</span><span class="sxs-lookup"><span data-stu-id="c5e65-154">AlertThresholdOperator</span></span> |<span data-ttu-id="c5e65-155">#thresholdoperator</span><span class="sxs-lookup"><span data-stu-id="c5e65-155">#thresholdoperator</span></span> |<span data-ttu-id="c5e65-156">Operátor prahová hodnota pro pravidlo výstrahy.</span><span class="sxs-lookup"><span data-stu-id="c5e65-156">Threshold operator for the alert rule.</span></span>  <span data-ttu-id="c5e65-157">*Větší než* nebo *menší než*.</span><span class="sxs-lookup"><span data-stu-id="c5e65-157">*Greater than* or *Less than*.</span></span> |
| <span data-ttu-id="c5e65-158">AlertThresholdValue</span><span class="sxs-lookup"><span data-stu-id="c5e65-158">AlertThresholdValue</span></span> |<span data-ttu-id="c5e65-159">#thresholdvalue</span><span class="sxs-lookup"><span data-stu-id="c5e65-159">#thresholdvalue</span></span> |<span data-ttu-id="c5e65-160">Prahová hodnota pro pravidlo výstrahy.</span><span class="sxs-lookup"><span data-stu-id="c5e65-160">Threshold value for the alert rule.</span></span> |
| <span data-ttu-id="c5e65-161">LinkToSearchResults</span><span class="sxs-lookup"><span data-stu-id="c5e65-161">LinkToSearchResults</span></span> |<span data-ttu-id="c5e65-162">#linktosearchresults</span><span class="sxs-lookup"><span data-stu-id="c5e65-162">#linktosearchresults</span></span> |<span data-ttu-id="c5e65-163">Propojit vyhledávání protokolu analýzy protokolů, který vrátí záznamy v dotazu, který vytvořili výstrahu.</span><span class="sxs-lookup"><span data-stu-id="c5e65-163">Link to Log Analytics log search that returns the records from the query that created the alert.</span></span> |
| <span data-ttu-id="c5e65-164">Element resultcount element</span><span class="sxs-lookup"><span data-stu-id="c5e65-164">ResultCount</span></span> |<span data-ttu-id="c5e65-165">#searchresultcount</span><span class="sxs-lookup"><span data-stu-id="c5e65-165">#searchresultcount</span></span> |<span data-ttu-id="c5e65-166">Počet záznamů ve výsledcích hledání.</span><span class="sxs-lookup"><span data-stu-id="c5e65-166">Number of records in the search results.</span></span> |
| <span data-ttu-id="c5e65-167">SearchIntervalEndtimeUtc</span><span class="sxs-lookup"><span data-stu-id="c5e65-167">SearchIntervalEndtimeUtc</span></span> |<span data-ttu-id="c5e65-168">#searchintervalendtimeutc</span><span class="sxs-lookup"><span data-stu-id="c5e65-168">#searchintervalendtimeutc</span></span> |<span data-ttu-id="c5e65-169">Koncový čas pro dotaz ve formátu UTC.</span><span class="sxs-lookup"><span data-stu-id="c5e65-169">End time for the query in UTC format.</span></span> |
| <span data-ttu-id="c5e65-170">SearchIntervalInSeconds</span><span class="sxs-lookup"><span data-stu-id="c5e65-170">SearchIntervalInSeconds</span></span> |<span data-ttu-id="c5e65-171">#searchinterval</span><span class="sxs-lookup"><span data-stu-id="c5e65-171">#searchinterval</span></span> |<span data-ttu-id="c5e65-172">Časový interval pro pravidlo výstrahy.</span><span class="sxs-lookup"><span data-stu-id="c5e65-172">Time window for the alert rule.</span></span> |
| <span data-ttu-id="c5e65-173">SearchIntervalStartTimeUtc</span><span class="sxs-lookup"><span data-stu-id="c5e65-173">SearchIntervalStartTimeUtc</span></span> |<span data-ttu-id="c5e65-174">#searchintervalstarttimeutc</span><span class="sxs-lookup"><span data-stu-id="c5e65-174">#searchintervalstarttimeutc</span></span> |<span data-ttu-id="c5e65-175">Počáteční čas pro dotaz ve formátu UTC.</span><span class="sxs-lookup"><span data-stu-id="c5e65-175">Start time for the query in UTC format.</span></span> |
| <span data-ttu-id="c5e65-176">SearchQuery</span><span class="sxs-lookup"><span data-stu-id="c5e65-176">SearchQuery</span></span> |<span data-ttu-id="c5e65-177">#searchquery</span><span class="sxs-lookup"><span data-stu-id="c5e65-177">#searchquery</span></span> |<span data-ttu-id="c5e65-178">Vyhledávací dotaz protokolu používá pravidlo výstrahy.</span><span class="sxs-lookup"><span data-stu-id="c5e65-178">Log search query used by the alert rule.</span></span> |
| <span data-ttu-id="c5e65-179">SearchResults</span><span class="sxs-lookup"><span data-stu-id="c5e65-179">SearchResults</span></span> |<span data-ttu-id="c5e65-180">Níže najdete</span><span class="sxs-lookup"><span data-stu-id="c5e65-180">See below</span></span> |<span data-ttu-id="c5e65-181">Záznamů vrácených dotazem ve formátu JSON.</span><span class="sxs-lookup"><span data-stu-id="c5e65-181">Records returned by the query in JSON format.</span></span>  <span data-ttu-id="c5e65-182">Omezeno na první 5 000 záznamů.</span><span class="sxs-lookup"><span data-stu-id="c5e65-182">Limited to the first 5,000 records.</span></span> |
| <span data-ttu-id="c5e65-183">ID pracovního prostoru</span><span class="sxs-lookup"><span data-stu-id="c5e65-183">WorkspaceID</span></span> |<span data-ttu-id="c5e65-184">#workspaceid</span><span class="sxs-lookup"><span data-stu-id="c5e65-184">#workspaceid</span></span> |<span data-ttu-id="c5e65-185">ID pracovního prostoru OMS.</span><span class="sxs-lookup"><span data-stu-id="c5e65-185">ID of your OMS workspace.</span></span> |

<span data-ttu-id="c5e65-186">Například může určit následující vlastní datovou část, která obsahuje jeden parametr s názvem *text*.</span><span class="sxs-lookup"><span data-stu-id="c5e65-186">For example, you might specify the following custom payload that includes a single parameter called *text*.</span></span>  <span data-ttu-id="c5e65-187">Služby, který volá tento webhook by byla očekávána tento parametr.</span><span class="sxs-lookup"><span data-stu-id="c5e65-187">The service that this webhook calls would be expecting this parameter.</span></span>

    {
        "text":"#alertrulename fired with #searchresultcount over threshold of #thresholdvalue."
    }

<span data-ttu-id="c5e65-188">Tato datová část příkladu by odkazující na něco jako následující odeslání do webhooku.</span><span class="sxs-lookup"><span data-stu-id="c5e65-188">This example payload would resolve to something like the following when sent to the webhook.</span></span>

    {
        "text":"My Alert Rule fired with 18 records over threshold of 10 ."
    }

<span data-ttu-id="c5e65-189">Zahrnout vlastní datovou část výsledky hledání, přidejte následující řádek jako vlastnost nejvyšší úrovně v datové části json.</span><span class="sxs-lookup"><span data-stu-id="c5e65-189">To include search results in a custom payload, add the following line as a top level property in the json payload.</span></span>  

    "IncludeSearchResults":true

<span data-ttu-id="c5e65-190">Například pokud chcete vytvořit vlastní datovou část, která obsahuje jenom název výstrahy a výsledky hledání, můžete použít následující.</span><span class="sxs-lookup"><span data-stu-id="c5e65-190">For example, to create a custom payload that includes just the alert name and the search results, you could use the following.</span></span> 

    {
       "alertname":"#alertrulename",
       "IncludeSearchResults":true
    }


<span data-ttu-id="c5e65-191">Si můžete projít kompletní příklad vytvoření pravidla výstrahy s webhooku zahájíte externí služba v [vytvoří akci, výstrah webhooku v OMS analýzy protokolů k odeslání zprávy na Slack](log-analytics-alerts-webhooks.md).</span><span class="sxs-lookup"><span data-stu-id="c5e65-191">You can walk through a complete example of creating an alert rule with a webhook to start an external service at [Create an alert webhook action in OMS Log Analytics to send message to Slack](log-analytics-alerts-webhooks.md).</span></span>

## <a name="runbook-actions"></a><span data-ttu-id="c5e65-192">Akce sady Runbook</span><span class="sxs-lookup"><span data-stu-id="c5e65-192">Runbook actions</span></span>
<span data-ttu-id="c5e65-193">Runbook akce spuštění sady runbook ve službě Azure Automation.</span><span class="sxs-lookup"><span data-stu-id="c5e65-193">Runbook actions start a runbook in Azure Automation.</span></span>  <span data-ttu-id="c5e65-194">Chcete-li použít tento typ akce, musíte mít [řešení služby Automation](log-analytics-add-solutions.md) nainstalován a nakonfigurován v vaším pracovním prostorem OMS.</span><span class="sxs-lookup"><span data-stu-id="c5e65-194">In order to use this type of action, you must have the [Automation solution](log-analytics-add-solutions.md) installed and configured in your OMS workspace.</span></span>  <span data-ttu-id="c5e65-195">Můžete vybrat ze sady runbook do účtu automation, který jste nakonfigurovali v řešení služby Automation.</span><span class="sxs-lookup"><span data-stu-id="c5e65-195">You can select from the runbooks in the automation account that you configured in the Automation solution.</span></span>

<span data-ttu-id="c5e65-196">Akce Runbook vyžadují vlastnosti v následující tabulce.</span><span class="sxs-lookup"><span data-stu-id="c5e65-196">Runbook actions require the properties in the following table.</span></span>

| <span data-ttu-id="c5e65-197">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="c5e65-197">Property</span></span> | <span data-ttu-id="c5e65-198">Popis</span><span class="sxs-lookup"><span data-stu-id="c5e65-198">Description</span></span> |
|:--- |:---|
| <span data-ttu-id="c5e65-199">Runbook</span><span class="sxs-lookup"><span data-stu-id="c5e65-199">Runbook</span></span> | <span data-ttu-id="c5e65-200">Sady Runbook, který chcete spustit v případě, že se vytvoří výstraha.</span><span class="sxs-lookup"><span data-stu-id="c5e65-200">Runbook that you want to start when an alert is created.</span></span> |
| <span data-ttu-id="c5e65-201">Spusťte na</span><span class="sxs-lookup"><span data-stu-id="c5e65-201">Run on</span></span> | <span data-ttu-id="c5e65-202">Zadejte **Azure** ke spuštění runbooku v cloudu.</span><span class="sxs-lookup"><span data-stu-id="c5e65-202">Specify **Azure** to run the runbook in the cloud.</span></span>  <span data-ttu-id="c5e65-203">Zadejte **hybridní pracovní proces** ke spuštění sady runbook na agenta s [hybridní pracovní proces Runbooku](../automation/automation-hybrid-runbook-worker.md ) nainstalována.</span><span class="sxs-lookup"><span data-stu-id="c5e65-203">Specify **Hybrid worker** to run the runbook on an agent with [Hybrid Runbook Worker](../automation/automation-hybrid-runbook-worker.md ) installed.</span></span>  |

<span data-ttu-id="c5e65-204">Akce Runbook spustit pomocí sady runbook [webhooku](../automation/automation-webhooks.md).</span><span class="sxs-lookup"><span data-stu-id="c5e65-204">Runbook actions start the runbook using a [webhook](../automation/automation-webhooks.md).</span></span>  <span data-ttu-id="c5e65-205">Když vytvoříte pravidlo výstrahy, automaticky vytvoří nové webhooku pro sadu runbook s názvem **OMS výstraha nápravy** následuje identifikátor GUID.</span><span class="sxs-lookup"><span data-stu-id="c5e65-205">When you create the alert rule, it will automatically create a new webhook for the runbook with the name **OMS Alert Remediation** followed by a GUID.</span></span>  

<span data-ttu-id="c5e65-206">Nelze přímo naplnit žádné parametry sady runbook, ale [parametru $WebhookData](../automation/automation-webhooks.md) bude obsahovat podrobnosti o výstraze, včetně výsledky hledání protokolů, která ji vytvořila.</span><span class="sxs-lookup"><span data-stu-id="c5e65-206">You cannot directly populate any parameters of the runbook, but the [$WebhookData parameter](../automation/automation-webhooks.md) will include the details of the alert, including the results of the log search that created it.</span></span>  <span data-ttu-id="c5e65-207">Sada runbook bude muset definovat **$WebhookData** jako parametr pro ho pro přístup k vlastnostem výstrahy.</span><span class="sxs-lookup"><span data-stu-id="c5e65-207">The runbook will need to define **$WebhookData** as a parameter for it to access the properties of the alert.</span></span>  <span data-ttu-id="c5e65-208">Data výstrah je k dispozici ve formátu json v jednom vlastnost s názvem **SearchResults** v **RequestBody** vlastnost **$WebhookData**.</span><span class="sxs-lookup"><span data-stu-id="c5e65-208">The alert data is available in json format in a single property called **SearchResults** in the **RequestBody** property of **$WebhookData**.</span></span>  <span data-ttu-id="c5e65-209">To bude mít s vlastnostmi v následující tabulce.</span><span class="sxs-lookup"><span data-stu-id="c5e65-209">This will have with the properties in the following table.</span></span>

| <span data-ttu-id="c5e65-210">Node</span><span class="sxs-lookup"><span data-stu-id="c5e65-210">Node</span></span> | <span data-ttu-id="c5e65-211">Popis</span><span class="sxs-lookup"><span data-stu-id="c5e65-211">Description</span></span> |
|:--- |:--- |
| <span data-ttu-id="c5e65-212">id</span><span class="sxs-lookup"><span data-stu-id="c5e65-212">id</span></span> |<span data-ttu-id="c5e65-213">Cesta a identifikátor GUID hledání.</span><span class="sxs-lookup"><span data-stu-id="c5e65-213">Path and GUID of the search.</span></span> |
| <span data-ttu-id="c5e65-214">__metadata</span><span class="sxs-lookup"><span data-stu-id="c5e65-214">__metadata</span></span> |<span data-ttu-id="c5e65-215">Informace o výstraze, včetně počet záznamů a stav výsledky hledání.</span><span class="sxs-lookup"><span data-stu-id="c5e65-215">Information about the alert including the number of records and status of the search results.</span></span> |
| <span data-ttu-id="c5e65-216">hodnota</span><span class="sxs-lookup"><span data-stu-id="c5e65-216">value</span></span> |<span data-ttu-id="c5e65-217">Samostatný záznam pro každý záznam ve výsledcích hledání.</span><span class="sxs-lookup"><span data-stu-id="c5e65-217">Separate entry for each record in the search results.</span></span>  <span data-ttu-id="c5e65-218">Podrobnosti o položka bude odpovídat vlastnosti a hodnoty záznamu.</span><span class="sxs-lookup"><span data-stu-id="c5e65-218">The details of the entry will match the properties and values of the record.</span></span> |

<span data-ttu-id="c5e65-219">Například následující runbook by extrahovat záznamy nalezené protokolu a přiřaďte jiné vlastnosti na základě typu každý záznam.</span><span class="sxs-lookup"><span data-stu-id="c5e65-219">For example, the following runbook would extract the records returned by the log search  and assign different properties based on the type of each record.</span></span>  <span data-ttu-id="c5e65-220">Všimněte si, že sada runbook spustí převedením **RequestBody** z formátu json, které se možné pracovat s jako objekt v prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="c5e65-220">Note that the runbook starts by converting **RequestBody** from json so that it can be worked with as an object in PowerShell.</span></span>

    param ( 
        [object]$WebhookData
    )

    $RequestBody = ConvertFrom-JSON -InputObject $WebhookData.RequestBody
    $Records     = $RequestBody.SearchResults.value

    foreach ($Record in $Records)
    {
        $Computer = $Record.Computer

        if ($Record.Type -eq 'Event')
        {
            $EventNo    = $Record.EventID
            $EventLevel = $Record.EventLevelName
            $EventData  = $Record.EventData
        }

        if ($Record.Type -eq 'Perf')
        {
            $Object    = $Record.ObjectName
            $Counter   = $Record.CounterName
            $Instance  = $Record.InstanceName
            $Value     = $Record.CounterValue
        }
    }


## <a name="next-steps"></a><span data-ttu-id="c5e65-221">Další kroky</span><span class="sxs-lookup"><span data-stu-id="c5e65-221">Next steps</span></span>
- <span data-ttu-id="c5e65-222">Dokončete průvodce pro [konfigurace webook](log-analytics-alerts-webhooks.md) s pravidlo výstrahy.</span><span class="sxs-lookup"><span data-stu-id="c5e65-222">Complete a walkthrough for [configuring a webook](log-analytics-alerts-webhooks.md) with an alert rule.</span></span>  
- <span data-ttu-id="c5e65-223">Další informace o zápisu [sady runbook ve službě Azure Automation](https://azure.microsoft.com/documentation/services/automation) k nápravě problémů identifikovaný výstrahy.</span><span class="sxs-lookup"><span data-stu-id="c5e65-223">Learn how to write [runbooks in Azure Automation](https://azure.microsoft.com/documentation/services/automation) to remediate problems identified by alerts.</span></span>