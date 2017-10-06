---
title: aaaResponses tooalerts v OMS Log Analytics | Microsoft Docs
description: "Výstrahy v analýzy protokolů zjišťovat důležité informace ve svém úložišti OMS a zda lze proaktivně upozorňují na problémy nebo vyvolání akce tooattempt toocorrect je.  Tento článek popisuje, jak toocreate pravidlo výstrahy a podrobnosti hello různé akce jejich trvat."
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
ms.openlocfilehash: d24bb726a96e7143985f111c0599dc4e7898b4f0
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="add-actions-tooalert-rules-in-log-analytics"></a><span data-ttu-id="23705-104">Přidání pravidla tooalert akce v analýzy protokolů</span><span class="sxs-lookup"><span data-stu-id="23705-104">Add actions tooalert rules in Log Analytics</span></span>
<span data-ttu-id="23705-105">Když [výstraha je vytvořen v analýzy protokolů](log-analytics-alerts.md), máte možnost hello [konfiguraci pravidlo výstrahy hello](log-analytics-alerts.md) tooperform jednu nebo více akcí.</span><span class="sxs-lookup"><span data-stu-id="23705-105">When an [alert is created in Log Analytics](log-analytics-alerts.md), you have hello option of [configuring hello alert rule](log-analytics-alerts.md) tooperform one or more actions.</span></span>  <span data-ttu-id="23705-106">Tento článek popisuje hello různé akce, které jsou k dispozici a podrobnosti o konfiguraci jednotlivých typů.</span><span class="sxs-lookup"><span data-stu-id="23705-106">This article describes hello different actions that are available and details on configuring each kind.</span></span>

| <span data-ttu-id="23705-107">Akce</span><span class="sxs-lookup"><span data-stu-id="23705-107">Action</span></span> | <span data-ttu-id="23705-108">Popis</span><span class="sxs-lookup"><span data-stu-id="23705-108">Description</span></span> |
|:--|:--|
| [<span data-ttu-id="23705-109">E-mailu</span><span class="sxs-lookup"><span data-stu-id="23705-109">Email</span></span>](#email-actions) | <span data-ttu-id="23705-110">Odeslat e-mail s hello podrobností výstrahy tooone hello nebo další příjemce.</span><span class="sxs-lookup"><span data-stu-id="23705-110">Send an e-mail with hello details of hello alert tooone or more recipients.</span></span> |
| [<span data-ttu-id="23705-111">Webhooku</span><span class="sxs-lookup"><span data-stu-id="23705-111">Webhook</span></span>](#webhook-actions) | <span data-ttu-id="23705-112">Vyvolání externího procesu prostřednictvím jedné žádosti HTTP POST.</span><span class="sxs-lookup"><span data-stu-id="23705-112">Invoke an external process through a single HTTP POST request.</span></span> |
| [<span data-ttu-id="23705-113">Sady Runbook</span><span class="sxs-lookup"><span data-stu-id="23705-113">Runbook</span></span>](#runbook-actions) | <span data-ttu-id="23705-114">Spuštění sady runbook ve službě Azure Automation.</span><span class="sxs-lookup"><span data-stu-id="23705-114">Start a runbook in Azure Automation.</span></span> |


## <a name="email-actions"></a><span data-ttu-id="23705-115">Akce e-mailu</span><span class="sxs-lookup"><span data-stu-id="23705-115">Email actions</span></span>
<span data-ttu-id="23705-116">E-mailu akce Odeslat e-mail s hello podrobností výstrahy tooone hello nebo další příjemce.</span><span class="sxs-lookup"><span data-stu-id="23705-116">Email actions send an e-mail with hello details of hello alert tooone or more recipients.</span></span>  <span data-ttu-id="23705-117">Můžete zadat hello předmět e-mailu hello, ale jeho obsah je standardní formát vytvořená pomocí analýzy protokolů.</span><span class="sxs-lookup"><span data-stu-id="23705-117">You can specify hello subject of hello mail, but it's content is a standard format constructed by Log Analytics.</span></span>  <span data-ttu-id="23705-118">V přidání toodetails z až tooten záznamů vrácených hledání protokolů hello obsahuje souhrnné informace, jako je název hello hello výstrahy.</span><span class="sxs-lookup"><span data-stu-id="23705-118">It includes summary information such as hello name of hello alert in addition toodetails of up tooten records returned by hello log search.</span></span>  <span data-ttu-id="23705-119">Zahrnuje také odkaz tooa protokolu vyhledávání v analýzy protokolů, který vrátí hello celou sadu záznamů z tohoto dotazu.</span><span class="sxs-lookup"><span data-stu-id="23705-119">It also includes a link tooa log search in Log Analytics that will return hello entire set of records from that query.</span></span>   <span data-ttu-id="23705-120">Odesílatel Hello hello pošty je *týmu Microsoft Operations Management Suite &lt; noreply@oms.microsoft.com &gt;* .</span><span class="sxs-lookup"><span data-stu-id="23705-120">hello sender of hello mail is *Microsoft Operations Management Suite Team &lt;noreply@oms.microsoft.com&gt;*.</span></span> 

<span data-ttu-id="23705-121">E-mailu akce vyžadují hello vlastnosti v hello následující tabulka.</span><span class="sxs-lookup"><span data-stu-id="23705-121">Email actions require hello properties in hello following table.</span></span>

| <span data-ttu-id="23705-122">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="23705-122">Property</span></span> | <span data-ttu-id="23705-123">Popis</span><span class="sxs-lookup"><span data-stu-id="23705-123">Description</span></span> |
|:--- |:--- |
| <span data-ttu-id="23705-124">Předmět</span><span class="sxs-lookup"><span data-stu-id="23705-124">Subject</span></span> |<span data-ttu-id="23705-125">Subjektu v e-mailu hello.</span><span class="sxs-lookup"><span data-stu-id="23705-125">Subject in hello email.</span></span>  <span data-ttu-id="23705-126">Nelze upravit hello textu hello pošty.</span><span class="sxs-lookup"><span data-stu-id="23705-126">You cannot modify hello body of hello mail.</span></span> |
| <span data-ttu-id="23705-127">Příjemce</span><span class="sxs-lookup"><span data-stu-id="23705-127">Recipients</span></span> |<span data-ttu-id="23705-128">Adresy všech příjemců e-mailu.</span><span class="sxs-lookup"><span data-stu-id="23705-128">Addresses of all e-mail recipients.</span></span>  <span data-ttu-id="23705-129">Pokud zadáte víc než jednou adresou pak samostatné hello adresy oddělte středníkem (;).</span><span class="sxs-lookup"><span data-stu-id="23705-129">If you specify more than one address, then separate hello addresses with a semicolon (;).</span></span> |


## <a name="webhook-actions"></a><span data-ttu-id="23705-130">Akce Webhooku</span><span class="sxs-lookup"><span data-stu-id="23705-130">Webhook actions</span></span>

<span data-ttu-id="23705-131">Akce Webhooku povolit tooinvoke externího procesu prostřednictvím jedné žádosti HTTP POST.</span><span class="sxs-lookup"><span data-stu-id="23705-131">Webhook actions allow you tooinvoke an external process through a single HTTP POST request.</span></span>  <span data-ttu-id="23705-132">Služba Hello volané by měla podporovat webhooky a určit, jak se bude používat žádné datové části obdrží.</span><span class="sxs-lookup"><span data-stu-id="23705-132">hello service being called should support webhooks and determine how it will use any payload it receives.</span></span>  <span data-ttu-id="23705-133">Může také zavolat REST API, která nepodporuje konkrétně webhooků, dokud hello požadavku je ve formátu, že hello rozumí rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="23705-133">You could also call a REST API that doesn't specifically support webhooks as long as hello request is in a format that hello API understands.</span></span>  <span data-ttu-id="23705-134">Příklady použití webhook, jehož v odpovědi tooan výstrahy jsou odesílání zprávy v [Slack](http://slack.com) nebo vytváření incidentu v [PagerDuty](http://pagerduty.com/).</span><span class="sxs-lookup"><span data-stu-id="23705-134">Examples of using a webhook in response tooan alert are sending a message in [Slack](http://slack.com) or creating an incident in [PagerDuty](http://pagerduty.com/).</span></span>  <span data-ttu-id="23705-135">Kompletní a podrobný postup vytvoření pravidla výstrahy s webhooku toocall Slack je k dispozici na [Webhooky ve výstrahách analýzy protokolů](log-analytics-alerts-webhooks.md).</span><span class="sxs-lookup"><span data-stu-id="23705-135">A complete walkthrough of creating an alert rule with a webhook toocall Slack is available at [Webhooks in Log Analytics alerts](log-analytics-alerts-webhooks.md).</span></span>

<span data-ttu-id="23705-136">Akce Webhooku vyžadují hello vlastnosti v hello následující tabulka.</span><span class="sxs-lookup"><span data-stu-id="23705-136">Webhook actions require hello properties in hello following table.</span></span>

| <span data-ttu-id="23705-137">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="23705-137">Property</span></span> | <span data-ttu-id="23705-138">Popis</span><span class="sxs-lookup"><span data-stu-id="23705-138">Description</span></span> |
|:--- |:--- |
| <span data-ttu-id="23705-139">Adresa URL Webhooku</span><span class="sxs-lookup"><span data-stu-id="23705-139">Webhook URL</span></span> |<span data-ttu-id="23705-140">Hello adresa URL webhooku hello.</span><span class="sxs-lookup"><span data-stu-id="23705-140">hello URL of hello webhook.</span></span> |
| <span data-ttu-id="23705-141">Vlastní datovou část JSON</span><span class="sxs-lookup"><span data-stu-id="23705-141">Custom JSON payload</span></span> |<span data-ttu-id="23705-142">Vlastní datovou část toosend s webhooku hello.</span><span class="sxs-lookup"><span data-stu-id="23705-142">Custom payload toosend with hello webhook.</span></span>  <span data-ttu-id="23705-143">Níže naleznete podrobnosti.</span><span class="sxs-lookup"><span data-stu-id="23705-143">See below for details.</span></span> |


<span data-ttu-id="23705-144">Webhooky přidat adresu URL a datovou část ve formátu JSON, který je hello data odeslána toohello externí služby.</span><span class="sxs-lookup"><span data-stu-id="23705-144">Webhooks include a URL and a payload formatted in JSON that is hello data sent toohello external service.</span></span>  <span data-ttu-id="23705-145">Ve výchozím nastavení datová část hello obsahovat hello hodnoty v hello následující tabulka.</span><span class="sxs-lookup"><span data-stu-id="23705-145">By default, hello payload will include hello values in hello following table.</span></span>  <span data-ttu-id="23705-146">Můžete tooreplace tato datová část s vlastní jeden vlastní.</span><span class="sxs-lookup"><span data-stu-id="23705-146">You can choose tooreplace this payload with a custom one of your own.</span></span>  <span data-ttu-id="23705-147">V takovém případě můžete použít hello proměnné v tabulce hello pro jednotlivé parametry tooinclude hello jejich hodnotu ve vaší vlastní datovou část.</span><span class="sxs-lookup"><span data-stu-id="23705-147">In that case you can use hello variables in hello table for each of hello parameters tooinclude their value in your custom payload.</span></span>

| <span data-ttu-id="23705-148">Parametr</span><span class="sxs-lookup"><span data-stu-id="23705-148">Parameter</span></span> | <span data-ttu-id="23705-149">Proměnná</span><span class="sxs-lookup"><span data-stu-id="23705-149">Variable</span></span> | <span data-ttu-id="23705-150">Popis</span><span class="sxs-lookup"><span data-stu-id="23705-150">Description</span></span> |
|:--- |:--- |:--- |
| <span data-ttu-id="23705-151">AlertRuleName</span><span class="sxs-lookup"><span data-stu-id="23705-151">AlertRuleName</span></span> |<span data-ttu-id="23705-152">#alertrulename</span><span class="sxs-lookup"><span data-stu-id="23705-152">#alertrulename</span></span> |<span data-ttu-id="23705-153">Název pravidla výstrahy hello.</span><span class="sxs-lookup"><span data-stu-id="23705-153">Name of hello alert rule.</span></span> |
| <span data-ttu-id="23705-154">AlertThresholdOperator</span><span class="sxs-lookup"><span data-stu-id="23705-154">AlertThresholdOperator</span></span> |<span data-ttu-id="23705-155">#thresholdoperator</span><span class="sxs-lookup"><span data-stu-id="23705-155">#thresholdoperator</span></span> |<span data-ttu-id="23705-156">Operátor prahová hodnota pro pravidlo výstrahy hello.</span><span class="sxs-lookup"><span data-stu-id="23705-156">Threshold operator for hello alert rule.</span></span>  <span data-ttu-id="23705-157">*Větší než* nebo *menší než*.</span><span class="sxs-lookup"><span data-stu-id="23705-157">*Greater than* or *Less than*.</span></span> |
| <span data-ttu-id="23705-158">AlertThresholdValue</span><span class="sxs-lookup"><span data-stu-id="23705-158">AlertThresholdValue</span></span> |<span data-ttu-id="23705-159">#thresholdvalue</span><span class="sxs-lookup"><span data-stu-id="23705-159">#thresholdvalue</span></span> |<span data-ttu-id="23705-160">Prahová hodnota pro pravidlo výstrahy hello.</span><span class="sxs-lookup"><span data-stu-id="23705-160">Threshold value for hello alert rule.</span></span> |
| <span data-ttu-id="23705-161">LinkToSearchResults</span><span class="sxs-lookup"><span data-stu-id="23705-161">LinkToSearchResults</span></span> |<span data-ttu-id="23705-162">#linktosearchresults</span><span class="sxs-lookup"><span data-stu-id="23705-162">#linktosearchresults</span></span> |<span data-ttu-id="23705-163">Odkaz tooLog Analytics protokolu vyhledávání, který vrátí hello záznamů z hello dotazu, který vytvořili výstrahu hello.</span><span class="sxs-lookup"><span data-stu-id="23705-163">Link tooLog Analytics log search that returns hello records from hello query that created hello alert.</span></span> |
| <span data-ttu-id="23705-164">Element resultcount element</span><span class="sxs-lookup"><span data-stu-id="23705-164">ResultCount</span></span> |<span data-ttu-id="23705-165">#searchresultcount</span><span class="sxs-lookup"><span data-stu-id="23705-165">#searchresultcount</span></span> |<span data-ttu-id="23705-166">Počet záznamů ve výsledcích hledání hello.</span><span class="sxs-lookup"><span data-stu-id="23705-166">Number of records in hello search results.</span></span> |
| <span data-ttu-id="23705-167">SearchIntervalEndtimeUtc</span><span class="sxs-lookup"><span data-stu-id="23705-167">SearchIntervalEndtimeUtc</span></span> |<span data-ttu-id="23705-168">#searchintervalendtimeutc</span><span class="sxs-lookup"><span data-stu-id="23705-168">#searchintervalendtimeutc</span></span> |<span data-ttu-id="23705-169">Koncový čas pro dotaz hello ve formátu UTC.</span><span class="sxs-lookup"><span data-stu-id="23705-169">End time for hello query in UTC format.</span></span> |
| <span data-ttu-id="23705-170">SearchIntervalInSeconds</span><span class="sxs-lookup"><span data-stu-id="23705-170">SearchIntervalInSeconds</span></span> |<span data-ttu-id="23705-171">#searchinterval</span><span class="sxs-lookup"><span data-stu-id="23705-171">#searchinterval</span></span> |<span data-ttu-id="23705-172">Časový interval pro pravidlo výstrahy hello.</span><span class="sxs-lookup"><span data-stu-id="23705-172">Time window for hello alert rule.</span></span> |
| <span data-ttu-id="23705-173">SearchIntervalStartTimeUtc</span><span class="sxs-lookup"><span data-stu-id="23705-173">SearchIntervalStartTimeUtc</span></span> |<span data-ttu-id="23705-174">#searchintervalstarttimeutc</span><span class="sxs-lookup"><span data-stu-id="23705-174">#searchintervalstarttimeutc</span></span> |<span data-ttu-id="23705-175">Počáteční čas pro hello dotazu ve formátu UTC.</span><span class="sxs-lookup"><span data-stu-id="23705-175">Start time for hello query in UTC format.</span></span> |
| <span data-ttu-id="23705-176">SearchQuery</span><span class="sxs-lookup"><span data-stu-id="23705-176">SearchQuery</span></span> |<span data-ttu-id="23705-177">#searchquery</span><span class="sxs-lookup"><span data-stu-id="23705-177">#searchquery</span></span> |<span data-ttu-id="23705-178">Vyhledávací dotaz protokolu používá pravidlo výstrahy hello.</span><span class="sxs-lookup"><span data-stu-id="23705-178">Log search query used by hello alert rule.</span></span> |
| <span data-ttu-id="23705-179">SearchResults</span><span class="sxs-lookup"><span data-stu-id="23705-179">SearchResults</span></span> |<span data-ttu-id="23705-180">Níže najdete</span><span class="sxs-lookup"><span data-stu-id="23705-180">See below</span></span> |<span data-ttu-id="23705-181">Záznamů vrácených dotazem hello ve formátu JSON.</span><span class="sxs-lookup"><span data-stu-id="23705-181">Records returned by hello query in JSON format.</span></span>  <span data-ttu-id="23705-182">Omezené toohello první 5 000 záznamů.</span><span class="sxs-lookup"><span data-stu-id="23705-182">Limited toohello first 5,000 records.</span></span> |
| <span data-ttu-id="23705-183">ID pracovního prostoru</span><span class="sxs-lookup"><span data-stu-id="23705-183">WorkspaceID</span></span> |<span data-ttu-id="23705-184">#workspaceid</span><span class="sxs-lookup"><span data-stu-id="23705-184">#workspaceid</span></span> |<span data-ttu-id="23705-185">ID pracovního prostoru OMS.</span><span class="sxs-lookup"><span data-stu-id="23705-185">ID of your OMS workspace.</span></span> |

<span data-ttu-id="23705-186">Například mohla uvádět hello následující vlastní datovou část, která obsahuje jeden parametr s názvem *text*.</span><span class="sxs-lookup"><span data-stu-id="23705-186">For example, you might specify hello following custom payload that includes a single parameter called *text*.</span></span>  <span data-ttu-id="23705-187">Hello služby, který volá tento webhook by byla očekávána tento parametr.</span><span class="sxs-lookup"><span data-stu-id="23705-187">hello service that this webhook calls would be expecting this parameter.</span></span>

    {
        "text":"#alertrulename fired with #searchresultcount over threshold of #thresholdvalue."
    }

<span data-ttu-id="23705-188">Tato datová část příkladu by vyřešit toosomething jako hello následující při odesílání toohello webhooku.</span><span class="sxs-lookup"><span data-stu-id="23705-188">This example payload would resolve toosomething like hello following when sent toohello webhook.</span></span>

    {
        "text":"My Alert Rule fired with 18 records over threshold of 10 ."
    }

<span data-ttu-id="23705-189">tooinclude výsledků vyhledávání ve vlastní datovou část, přidejte následující řádek jako vlastnost nejvyšší úrovně v datové části json hello hello.</span><span class="sxs-lookup"><span data-stu-id="23705-189">tooinclude search results in a custom payload, add hello following line as a top level property in hello json payload.</span></span>  

    "IncludeSearchResults":true

<span data-ttu-id="23705-190">Například toocreate vlastní datové části, která obsahuje jenom hello název výstrahy a výsledky hledání hello, můžete použít následující hello.</span><span class="sxs-lookup"><span data-stu-id="23705-190">For example, toocreate a custom payload that includes just hello alert name and hello search results, you could use hello following.</span></span> 

    {
       "alertname":"#alertrulename",
       "IncludeSearchResults":true
    }


<span data-ttu-id="23705-191">Si můžete projít kompletní příklad vytvoření pravidla výstrahy s webhook toostart služby externí v [vytvoří akci, výstrah webhooku v OMS Log Analytics toosend zpráva tooSlack](log-analytics-alerts-webhooks.md).</span><span class="sxs-lookup"><span data-stu-id="23705-191">You can walk through a complete example of creating an alert rule with a webhook toostart an external service at [Create an alert webhook action in OMS Log Analytics toosend message tooSlack](log-analytics-alerts-webhooks.md).</span></span>

## <a name="runbook-actions"></a><span data-ttu-id="23705-192">Akce sady Runbook</span><span class="sxs-lookup"><span data-stu-id="23705-192">Runbook actions</span></span>
<span data-ttu-id="23705-193">Runbook akce spuštění sady runbook ve službě Azure Automation.</span><span class="sxs-lookup"><span data-stu-id="23705-193">Runbook actions start a runbook in Azure Automation.</span></span>  <span data-ttu-id="23705-194">V pořadí toouse tento typ akce, musí mít hello [řešení služby Automation](log-analytics-add-solutions.md) nainstalován a nakonfigurován v vaším pracovním prostorem OMS.</span><span class="sxs-lookup"><span data-stu-id="23705-194">In order toouse this type of action, you must have hello [Automation solution](log-analytics-add-solutions.md) installed and configured in your OMS workspace.</span></span>  <span data-ttu-id="23705-195">Můžete vybrat ze sady runbook hello v hello účet automation, který jste nakonfigurovali v hello řešení služby Automation.</span><span class="sxs-lookup"><span data-stu-id="23705-195">You can select from hello runbooks in hello automation account that you configured in hello Automation solution.</span></span>

<span data-ttu-id="23705-196">Akce Runbook vyžadují hello vlastnosti v hello následující tabulka.</span><span class="sxs-lookup"><span data-stu-id="23705-196">Runbook actions require hello properties in hello following table.</span></span>

| <span data-ttu-id="23705-197">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="23705-197">Property</span></span> | <span data-ttu-id="23705-198">Popis</span><span class="sxs-lookup"><span data-stu-id="23705-198">Description</span></span> |
|:--- |:---|
| <span data-ttu-id="23705-199">Runbook</span><span class="sxs-lookup"><span data-stu-id="23705-199">Runbook</span></span> | <span data-ttu-id="23705-200">Sada Runbook má toostart, když se vytvoří výstraha.</span><span class="sxs-lookup"><span data-stu-id="23705-200">Runbook that you want toostart when an alert is created.</span></span> |
| <span data-ttu-id="23705-201">Spusťte na</span><span class="sxs-lookup"><span data-stu-id="23705-201">Run on</span></span> | <span data-ttu-id="23705-202">Zadejte **Azure** toorun hello runbooku v cloudu hello.</span><span class="sxs-lookup"><span data-stu-id="23705-202">Specify **Azure** toorun hello runbook in hello cloud.</span></span>  <span data-ttu-id="23705-203">Zadejte **hybridní pracovní proces** toorun hello runbook na agenta s [hybridní pracovní proces Runbooku](../automation/automation-hybrid-runbook-worker.md ) nainstalována.</span><span class="sxs-lookup"><span data-stu-id="23705-203">Specify **Hybrid worker** toorun hello runbook on an agent with [Hybrid Runbook Worker](../automation/automation-hybrid-runbook-worker.md ) installed.</span></span>  |

<span data-ttu-id="23705-204">Akce Runbook spustit hello runbooku pomocí [webhooku](../automation/automation-webhooks.md).</span><span class="sxs-lookup"><span data-stu-id="23705-204">Runbook actions start hello runbook using a [webhook](../automation/automation-webhooks.md).</span></span>  <span data-ttu-id="23705-205">Když vytvoříte pravidlo výstrahy hello, automaticky vytvoří nové webhooku hello sady runbook s názvem hello **OMS výstraha nápravy** následuje identifikátor GUID.</span><span class="sxs-lookup"><span data-stu-id="23705-205">When you create hello alert rule, it will automatically create a new webhook for hello runbook with hello name **OMS Alert Remediation** followed by a GUID.</span></span>  

<span data-ttu-id="23705-206">Nemůžete přímo naplnit žádné parametry hello sady runbook, ale hello [parametru $WebhookData](../automation/automation-webhooks.md) zahrne podrobnosti hello hello výstrahy, včetně hello výsledky hledání hello protokolů, která ji vytvořila.</span><span class="sxs-lookup"><span data-stu-id="23705-206">You cannot directly populate any parameters of hello runbook, but hello [$WebhookData parameter](../automation/automation-webhooks.md) will include hello details of hello alert, including hello results of hello log search that created it.</span></span>  <span data-ttu-id="23705-207">Hello runbook bude nutné toodefine **$WebhookData** jako parametr pro něj tooaccess hello vlastnosti hello výstrahy.</span><span class="sxs-lookup"><span data-stu-id="23705-207">hello runbook will need toodefine **$WebhookData** as a parameter for it tooaccess hello properties of hello alert.</span></span>  <span data-ttu-id="23705-208">Hello dat výstrah je k dispozici ve formátu json v jednom vlastnost s názvem **SearchResults** v hello **RequestBody** vlastnost **$WebhookData**.</span><span class="sxs-lookup"><span data-stu-id="23705-208">hello alert data is available in json format in a single property called **SearchResults** in hello **RequestBody** property of **$WebhookData**.</span></span>  <span data-ttu-id="23705-209">To bude mít s vlastnostmi hello v hello následující tabulka.</span><span class="sxs-lookup"><span data-stu-id="23705-209">This will have with hello properties in hello following table.</span></span>

| <span data-ttu-id="23705-210">Node</span><span class="sxs-lookup"><span data-stu-id="23705-210">Node</span></span> | <span data-ttu-id="23705-211">Popis</span><span class="sxs-lookup"><span data-stu-id="23705-211">Description</span></span> |
|:--- |:--- |
| <span data-ttu-id="23705-212">id</span><span class="sxs-lookup"><span data-stu-id="23705-212">id</span></span> |<span data-ttu-id="23705-213">Cesta a identifikátor GUID hello hledání.</span><span class="sxs-lookup"><span data-stu-id="23705-213">Path and GUID of hello search.</span></span> |
| <span data-ttu-id="23705-214">__metadata</span><span class="sxs-lookup"><span data-stu-id="23705-214">__metadata</span></span> |<span data-ttu-id="23705-215">Informace o hello výstrahy včetně hello počet záznamů a stav hello výsledky hledání.</span><span class="sxs-lookup"><span data-stu-id="23705-215">Information about hello alert including hello number of records and status of hello search results.</span></span> |
| <span data-ttu-id="23705-216">hodnota</span><span class="sxs-lookup"><span data-stu-id="23705-216">value</span></span> |<span data-ttu-id="23705-217">Samostatný záznam pro každý záznam ve výsledcích hledání hello.</span><span class="sxs-lookup"><span data-stu-id="23705-217">Separate entry for each record in hello search results.</span></span>  <span data-ttu-id="23705-218">Podrobnosti o Hello hello položky bude odpovídat hello vlastnosti a hodnoty hello záznamu.</span><span class="sxs-lookup"><span data-stu-id="23705-218">hello details of hello entry will match hello properties and values of hello record.</span></span> |

<span data-ttu-id="23705-219">Například hello následující runbook by extrahovat hello záznamů vrácených hledání hello protokolů a přiřaďte jiné vlastnosti na základě typu hello každý záznam.</span><span class="sxs-lookup"><span data-stu-id="23705-219">For example, hello following runbook would extract hello records returned by hello log search  and assign different properties based on hello type of each record.</span></span>  <span data-ttu-id="23705-220">Všimněte si dané sady runbook hello začne převádění **RequestBody** z formátu json, které se možné pracovat s jako objekt v prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="23705-220">Note that hello runbook starts by converting **RequestBody** from json so that it can be worked with as an object in PowerShell.</span></span>

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


## <a name="next-steps"></a><span data-ttu-id="23705-221">Další kroky</span><span class="sxs-lookup"><span data-stu-id="23705-221">Next steps</span></span>
- <span data-ttu-id="23705-222">Dokončete průvodce pro [konfigurace webook](log-analytics-alerts-webhooks.md) s pravidlo výstrahy.</span><span class="sxs-lookup"><span data-stu-id="23705-222">Complete a walkthrough for [configuring a webook](log-analytics-alerts-webhooks.md) with an alert rule.</span></span>  
- <span data-ttu-id="23705-223">Zjistěte, jak toowrite [sady runbook ve službě Azure Automation](https://azure.microsoft.com/documentation/services/automation) tooremediate problémy identifikovat pomocí výstrah.</span><span class="sxs-lookup"><span data-stu-id="23705-223">Learn how toowrite [runbooks in Azure Automation](https://azure.microsoft.com/documentation/services/automation) tooremediate problems identified by alerts.</span></span>
