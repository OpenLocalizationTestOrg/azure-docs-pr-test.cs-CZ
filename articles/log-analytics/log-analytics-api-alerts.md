---
title: "aaaUsing OMS Log Analytics výstrahy REST API"
description: "Hello Log Analytics výstrahy REST API můžete toocreate a Správa výstrah v analýzy protokolů, která je součástí služby Operations Management Suite (OMS).  Tento článek obsahuje podrobnosti o hello rozhraní API a několik příkladů pro provádění různých akcí."
services: log-analytics
documentationcenter: 
author: bwren
manager: jwhit
editor: tysonn
ms.assetid: 628ad256-7181-4a0d-9e68-4ed60c0f3f04
ms.service: log-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 05/12/2017
ms.author: bwren
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 418dc7eb71d6151c6380b8925f1f147a0e13b178
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-and-manage-alert-rules-in-log-analytics-with-rest-api"></a><span data-ttu-id="9c65f-104">Vytvářet a spravovat pravidla výstrah v analýzy protokolů pomocí rozhraní REST API</span><span class="sxs-lookup"><span data-stu-id="9c65f-104">Create and manage alert rules in Log Analytics with REST API</span></span>
<span data-ttu-id="9c65f-105">Hello Log Analytics výstrahy REST API můžete toocreate a Správa výstrah v Operations Management Suite (OMS).</span><span class="sxs-lookup"><span data-stu-id="9c65f-105">hello Log Analytics Alert REST API allows you toocreate and manage alerts in Operations Management Suite (OMS).</span></span>  <span data-ttu-id="9c65f-106">Tento článek obsahuje podrobnosti o hello rozhraní API a několik příkladů pro provádění různých akcí.</span><span class="sxs-lookup"><span data-stu-id="9c65f-106">This article provides details of hello API and several examples for performing different operations.</span></span>

<span data-ttu-id="9c65f-107">Hello Log Analytics vyhledávání REST API je dosáhl standardu RESTful a je přístupný prostřednictvím hello REST API služby Azure Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="9c65f-107">hello Log Analytics Search REST API is RESTful and can be accessed via hello Azure Resource Manager REST API.</span></span> <span data-ttu-id="9c65f-108">V tomto dokumentu najdete příklady kterých je přístup k hello rozhraní API z příkazového řádku pomocí prostředí PowerShell [ARMClient](https://github.com/projectkudu/ARMClient), nástroj pro příkazový řádek s otevřeným zdrojem, který zjednodušuje vyvolání hello rozhraní API Správce prostředků Azure.</span><span class="sxs-lookup"><span data-stu-id="9c65f-108">In this document you will find examples where hello API is accessed from a PowerShell command line using  [ARMClient](https://github.com/projectkudu/ARMClient), an open source command line tool that simplifies invoking hello Azure Resource Manager API.</span></span> <span data-ttu-id="9c65f-109">použití Hello ARMClient a prostředí PowerShell je jedním z mnoha možností tooaccess hello Log Analytics vyhledávání API.</span><span class="sxs-lookup"><span data-stu-id="9c65f-109">hello use of ARMClient and PowerShell is one of many options tooaccess hello Log Analytics Search API.</span></span> <span data-ttu-id="9c65f-110">Pomocí těchto nástrojů můžete využívat pracovní prostory hello RESTful API Azure Resource Manager toomake volání tooOMS a provádět příkazy vyhledávání v nich.</span><span class="sxs-lookup"><span data-stu-id="9c65f-110">With these tools you can utilize hello RESTful Azure Resource Manager API toomake calls tooOMS workspaces and perform search commands within them.</span></span> <span data-ttu-id="9c65f-111">Hello rozhraní API bude výstup tooyou výsledků vyhledávání ve formátu JSON, což vám výsledky hledání hello toouse mnoha různými způsoby prostřednictvím kódu programu.</span><span class="sxs-lookup"><span data-stu-id="9c65f-111">hello API will output search results tooyou in JSON format, allowing you toouse hello search results in many different ways programmatically.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="9c65f-112">Požadavky</span><span class="sxs-lookup"><span data-stu-id="9c65f-112">Prerequisites</span></span>
<span data-ttu-id="9c65f-113">V současné době mohou výstrahy vytvořeny pouze s uloženého hledání v analýzy protokolů.</span><span class="sxs-lookup"><span data-stu-id="9c65f-113">Currently, alerts can only be created with a saved search in Log Analytics.</span></span>  <span data-ttu-id="9c65f-114">Můžete se podívat toohello [rozhraní API REST vyhledávání protokolu](log-analytics-log-search-api.md) Další informace.</span><span class="sxs-lookup"><span data-stu-id="9c65f-114">You can refer toohello [Log Search REST API](log-analytics-log-search-api.md) for more information.</span></span>

## <a name="schedules"></a><span data-ttu-id="9c65f-115">Plány</span><span class="sxs-lookup"><span data-stu-id="9c65f-115">Schedules</span></span>
<span data-ttu-id="9c65f-116">Uložené hledání může mít jeden nebo více plánů.</span><span class="sxs-lookup"><span data-stu-id="9c65f-116">A saved search can have one or more schedules.</span></span> <span data-ttu-id="9c65f-117">Hello plán definuje jak často hello vyhledávání běží a je identifikován hello časový interval, přes které hello kritéria.</span><span class="sxs-lookup"><span data-stu-id="9c65f-117">hello schedule defines how often hello search is run and hello time interval over which hello criteria is identified.</span></span>
<span data-ttu-id="9c65f-118">Plány mít hello vlastnosti v hello následující tabulka.</span><span class="sxs-lookup"><span data-stu-id="9c65f-118">Schedules have hello properties in hello following table.</span></span>

| <span data-ttu-id="9c65f-119">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="9c65f-119">Property</span></span> | <span data-ttu-id="9c65f-120">Popis</span><span class="sxs-lookup"><span data-stu-id="9c65f-120">Description</span></span> |
|:--- |:--- |
| <span data-ttu-id="9c65f-121">Interval</span><span class="sxs-lookup"><span data-stu-id="9c65f-121">Interval</span></span> |<span data-ttu-id="9c65f-122">Jak často hello vyhledávání běží.</span><span class="sxs-lookup"><span data-stu-id="9c65f-122">How often hello search is run.</span></span> <span data-ttu-id="9c65f-123">Měří v minutách.</span><span class="sxs-lookup"><span data-stu-id="9c65f-123">Measured in minutes.</span></span> |
| <span data-ttu-id="9c65f-124">QueryTimeSpan</span><span class="sxs-lookup"><span data-stu-id="9c65f-124">QueryTimeSpan</span></span> |<span data-ttu-id="9c65f-125">Hello časový interval, přes které hello je vyhodnotit kritéria.</span><span class="sxs-lookup"><span data-stu-id="9c65f-125">hello time interval over which hello criteria is evaluated.</span></span> <span data-ttu-id="9c65f-126">Musí být rovna tooor větší než Interval.</span><span class="sxs-lookup"><span data-stu-id="9c65f-126">Must be equal tooor greater than Interval.</span></span> <span data-ttu-id="9c65f-127">Měří v minutách.</span><span class="sxs-lookup"><span data-stu-id="9c65f-127">Measured in minutes.</span></span> |
| <span data-ttu-id="9c65f-128">Verze</span><span class="sxs-lookup"><span data-stu-id="9c65f-128">Version</span></span> |<span data-ttu-id="9c65f-129">Hello používá verzi rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="9c65f-129">hello API version being used.</span></span>  <span data-ttu-id="9c65f-130">V současné době to musí být vždy nastavená too1.</span><span class="sxs-lookup"><span data-stu-id="9c65f-130">Currently, this should always be set too1.</span></span> |

<span data-ttu-id="9c65f-131">Představte si třeba dotazu událostí s Interval 15 minut a časový interval 30 minut.</span><span class="sxs-lookup"><span data-stu-id="9c65f-131">For example, consider an event query with an Interval of 15 minutes and a Timespan of 30 minutes.</span></span> <span data-ttu-id="9c65f-132">V takovém případě by být hello dotaz spustit každých 15 minut a výstraha by spustí, pokud hello kritéria dál tooresolve tootrue během určitého 30 minut.</span><span class="sxs-lookup"><span data-stu-id="9c65f-132">In this case, hello query would be run every 15 minutes, and an alert would be triggered if hello criteria continued tooresolve tootrue over a 30 minute span.</span></span>

### <a name="retrieving-schedules"></a><span data-ttu-id="9c65f-133">Načítání plány</span><span class="sxs-lookup"><span data-stu-id="9c65f-133">Retrieving schedules</span></span>
<span data-ttu-id="9c65f-134">Použití hello získat metoda tooretrieve všechny plány uložených hledání.</span><span class="sxs-lookup"><span data-stu-id="9c65f-134">Use hello Get method tooretrieve all schedules for a saved search.</span></span>

    armclient get /subscriptions/{Subscription ID}/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/{Search  ID}/schedules?api-version=2015-03-20

<span data-ttu-id="9c65f-135">Použití hello získat metoda s ID tooretrieve plán konkrétní plán uložených hledání.</span><span class="sxs-lookup"><span data-stu-id="9c65f-135">Use hello Get method with a schedule ID tooretrieve a particular schedule for a saved search.</span></span>

    armclient get /subscriptions/{Subscription ID}/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/{Subscription ID}/schedules/{Schedule ID}?api-version=2015-03-20

<span data-ttu-id="9c65f-136">Následuje ukázková odpověď pro plán.</span><span class="sxs-lookup"><span data-stu-id="9c65f-136">Following is a sample response for a schedule.</span></span>

```json
{
    "value": [{
        "id": "subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/MyWorkspace/savedSearches/0f0f4853-17f8-4ed1-9a03-8e888b0d16ec/schedules/a17b53ef-bd70-4ca4-9ead-83b00f2024a8",
        "etag": "W/\"datetime'2016-02-25T20%3A54%3A49.8074679Z'\"",
        "properties": {
            "Interval": 15,
            "QueryTimeSpan": 15
        }
    }]
}
```

### <a name="creating-a-schedule"></a><span data-ttu-id="9c65f-137">Vytvoření plánu</span><span class="sxs-lookup"><span data-stu-id="9c65f-137">Creating a schedule</span></span>
<span data-ttu-id="9c65f-138">Použijte metodu Put hello s plán jedinečné ID toocreate nový plán.</span><span class="sxs-lookup"><span data-stu-id="9c65f-138">Use hello Put method with a unique schedule ID toocreate a new schedule.</span></span>  <span data-ttu-id="9c65f-139">Všimněte si, že dvě plány nemůže mít hello stejným ID i v případě, že jsou přidruženy různé uložená hledání.</span><span class="sxs-lookup"><span data-stu-id="9c65f-139">Note that two schedules cannot have hello same ID even if they are associated with different saved searches.</span></span>  <span data-ttu-id="9c65f-140">Při vytváření plánu v konzole OMS hello, se vytvoří identifikátor GUID pro ID hello plán.</span><span class="sxs-lookup"><span data-stu-id="9c65f-140">When you create a schedule in hello OMS console, a GUID is created for hello schedule ID.</span></span>

> [!NOTE]
> <span data-ttu-id="9c65f-141">Název Hello všechna uložená hledání, plány a akce, které jsou vytvořené pomocí hello Log Analytics API musí být malými písmeny.</span><span class="sxs-lookup"><span data-stu-id="9c65f-141">hello name for all saved searches, schedules, and actions created with hello Log Analytics API must be in lowercase.</span></span>

    $scheduleJson = "{'properties': { 'Interval': 15, 'QueryTimeSpan':15, 'Active':'true' } }"
    armclient put /subscriptions/{Subscription ID}/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/{Search ID}/schedules/mynewschedule?api-version=2015-03-20 $scheduleJson

### <a name="editing-a-schedule"></a><span data-ttu-id="9c65f-142">Úprava plánu</span><span class="sxs-lookup"><span data-stu-id="9c65f-142">Editing a schedule</span></span>
<span data-ttu-id="9c65f-143">Použijte metodu Put hello s ID pro hello stejné uložit hledání toomodify, který naplánovat existující plán.</span><span class="sxs-lookup"><span data-stu-id="9c65f-143">Use hello Put method with an existing schedule ID for hello same saved search toomodify that schedule.</span></span>  <span data-ttu-id="9c65f-144">Hello textu hello požadavku musí obsahovat hello etag hello plánu.</span><span class="sxs-lookup"><span data-stu-id="9c65f-144">hello body of hello request must include hello etag of hello schedule.</span></span>

      $scheduleJson = "{'etag': 'W/\"datetime'2016-02-25T20%3A54%3A49.8074679Z'\""','properties': { 'Interval': 15, 'QueryTimeSpan':15, 'Active':'true' } }"
      armclient put /subscriptions/{Subscription ID}/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/{Search ID}/schedules/mynewschedule?api-version=2015-03-20 $scheduleJson


### <a name="deleting-schedules"></a><span data-ttu-id="9c65f-145">Odstraňování plány</span><span class="sxs-lookup"><span data-stu-id="9c65f-145">Deleting schedules</span></span>
<span data-ttu-id="9c65f-146">Použijte metodu Delete hello s toodelete plán ID plánu.</span><span class="sxs-lookup"><span data-stu-id="9c65f-146">Use hello Delete method with a schedule ID toodelete a schedule.</span></span>

    armclient delete /subscriptions/{Subscription ID}/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/{Subscription ID}/schedules/{Schedule ID}?api-version=2015-03-20


## <a name="actions"></a><span data-ttu-id="9c65f-147">Akce</span><span class="sxs-lookup"><span data-stu-id="9c65f-147">Actions</span></span>
<span data-ttu-id="9c65f-148">Plán může mít více akcí.</span><span class="sxs-lookup"><span data-stu-id="9c65f-148">A schedule can have multiple actions.</span></span> <span data-ttu-id="9c65f-149">Akce může definovat jeden nebo více tooperform procesy, jako je například odesílání e-mailu nebo spuštění sady runbook, nebo se může definovat prahové hodnoty, která určuje, kdy hello výsledky hledání kritériím některé.</span><span class="sxs-lookup"><span data-stu-id="9c65f-149">An action may define one or more processes tooperform such as sending a mail or starting a runbook, or it may define a threshold that determines when hello results of a search match some criteria.</span></span>  <span data-ttu-id="9c65f-150">Některé akce bude definovat i tak, aby procesy hello provede, když je dosaženo prahové hodnoty hello.</span><span class="sxs-lookup"><span data-stu-id="9c65f-150">Some actions will define both so that hello processes are performed when hello threshold is met.</span></span>

<span data-ttu-id="9c65f-151">Všechny akce mít hello vlastnosti v hello následující tabulka.</span><span class="sxs-lookup"><span data-stu-id="9c65f-151">All actions have hello properties in hello following table.</span></span>  <span data-ttu-id="9c65f-152">Různé typy výstrah mají různé další vlastnosti, které jsou popsány níže.</span><span class="sxs-lookup"><span data-stu-id="9c65f-152">Different types of alerts have different additional properties which are described below.</span></span>

| <span data-ttu-id="9c65f-153">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="9c65f-153">Property</span></span> | <span data-ttu-id="9c65f-154">Popis</span><span class="sxs-lookup"><span data-stu-id="9c65f-154">Description</span></span> |
|:--- |:--- |
| <span data-ttu-id="9c65f-155">Typ</span><span class="sxs-lookup"><span data-stu-id="9c65f-155">Type</span></span> |<span data-ttu-id="9c65f-156">Typ akce hello.</span><span class="sxs-lookup"><span data-stu-id="9c65f-156">Type of hello action.</span></span>  <span data-ttu-id="9c65f-157">Aktuálně hello možné hodnoty jsou výstrahy a Webhooku.</span><span class="sxs-lookup"><span data-stu-id="9c65f-157">Currently hello possible values are Alert and Webhook.</span></span> |
| <span data-ttu-id="9c65f-158">Name (Název)</span><span class="sxs-lookup"><span data-stu-id="9c65f-158">Name</span></span> |<span data-ttu-id="9c65f-159">Zobrazovaný název pro hello výstrahu.</span><span class="sxs-lookup"><span data-stu-id="9c65f-159">Display name for hello alert.</span></span> |
| <span data-ttu-id="9c65f-160">Verze</span><span class="sxs-lookup"><span data-stu-id="9c65f-160">Version</span></span> |<span data-ttu-id="9c65f-161">Hello používá verzi rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="9c65f-161">hello API version being used.</span></span>  <span data-ttu-id="9c65f-162">V současné době to musí být vždy nastavená too1.</span><span class="sxs-lookup"><span data-stu-id="9c65f-162">Currently, this should always be set too1.</span></span> |

### <a name="retrieving-actions"></a><span data-ttu-id="9c65f-163">Načítání akce</span><span class="sxs-lookup"><span data-stu-id="9c65f-163">Retrieving actions</span></span>
<span data-ttu-id="9c65f-164">Použití hello získat metoda tooretrieve všechny akce pro plán.</span><span class="sxs-lookup"><span data-stu-id="9c65f-164">Use hello Get method tooretrieve all actions for a schedule.</span></span>

    armclient get /subscriptions/{Subscription ID}/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/{Search  ID}/schedules/{Schedule ID}/actions?api-version=2015-03-20

<span data-ttu-id="9c65f-165">Použití hello získat metoda s hello akce ID tooretrieve určité akce pro plán.</span><span class="sxs-lookup"><span data-stu-id="9c65f-165">Use hello Get method with hello action ID tooretrieve a particular action for a schedule.</span></span>

    armclient get /subscriptions/{Subscription ID}/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/{Subscription ID}/schedules/{Schedule ID}/actions/{Action ID}?api-version=2015-03-20

### <a name="creating-or-editing-actions"></a><span data-ttu-id="9c65f-166">Vytvořením nebo úpravou akce</span><span class="sxs-lookup"><span data-stu-id="9c65f-166">Creating or editing actions</span></span>
<span data-ttu-id="9c65f-167">Použijte metodu Put hello s ID akce, které je jedinečné toohello plán toocreate novou akci.</span><span class="sxs-lookup"><span data-stu-id="9c65f-167">Use hello Put method with an action ID that is unique toohello schedule toocreate a new action.</span></span>  <span data-ttu-id="9c65f-168">Když vytvoříte akce v konzole OMS hello, je identifikátor GUID pro ID hello akce.</span><span class="sxs-lookup"><span data-stu-id="9c65f-168">When you create an action in hello OMS console, a GUID is for hello action ID.</span></span>

> [!NOTE]
> <span data-ttu-id="9c65f-169">Název Hello všechna uložená hledání, plány a akce, které jsou vytvořené pomocí hello Log Analytics API musí být malými písmeny.</span><span class="sxs-lookup"><span data-stu-id="9c65f-169">hello name for all saved searches, schedules, and actions created with hello Log Analytics API must be in lowercase.</span></span>

<span data-ttu-id="9c65f-170">Použijte metodu Put hello s existující akci ID pro hello stejné uložit hledání toomodify, který naplánovat.</span><span class="sxs-lookup"><span data-stu-id="9c65f-170">Use hello Put method with an existing action ID for hello same saved search toomodify that schedule.</span></span>  <span data-ttu-id="9c65f-171">Hello textu hello požadavku musí obsahovat hello etag hello plánu.</span><span class="sxs-lookup"><span data-stu-id="9c65f-171">hello body of hello request must include hello etag of hello schedule.</span></span>

<span data-ttu-id="9c65f-172">Hello formát požadavku pro vytvoření nové akce se liší podle typ akce, takže tyto příklady jsou uvedeny v následujících částech hello.</span><span class="sxs-lookup"><span data-stu-id="9c65f-172">hello request format for creating a new action varies by action type so these examples are provided in hello sections below.</span></span>

### <a name="deleting-actions"></a><span data-ttu-id="9c65f-173">Odstranění akcí</span><span class="sxs-lookup"><span data-stu-id="9c65f-173">Deleting actions</span></span>
<span data-ttu-id="9c65f-174">Použijte metodu Delete hello s hello akce ID toodelete akce.</span><span class="sxs-lookup"><span data-stu-id="9c65f-174">Use hello Delete method with hello action ID toodelete an action.</span></span>

    armclient delete /subscriptions/{Subscription ID}/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/{Subscription ID}/schedules/{Schedule ID}/Actions/{Action ID}?api-version=2015-03-20

### <a name="alert-actions"></a><span data-ttu-id="9c65f-175">Akce výstrah</span><span class="sxs-lookup"><span data-stu-id="9c65f-175">Alert Actions</span></span>
<span data-ttu-id="9c65f-176">Plán by měl mít pouze jeden výstrahy akce.</span><span class="sxs-lookup"><span data-stu-id="9c65f-176">A Schedule should have one and only one Alert action.</span></span>  <span data-ttu-id="9c65f-177">Akce výstrah minimálně jedna z částí hello v hello následující tabulka.</span><span class="sxs-lookup"><span data-stu-id="9c65f-177">Alert actions have one or more of hello sections in hello following table.</span></span>  <span data-ttu-id="9c65f-178">Každý je podrobně popsaná v další níže.</span><span class="sxs-lookup"><span data-stu-id="9c65f-178">Each is described in further detail below.</span></span>

| <span data-ttu-id="9c65f-179">Část</span><span class="sxs-lookup"><span data-stu-id="9c65f-179">Section</span></span> | <span data-ttu-id="9c65f-180">Popis</span><span class="sxs-lookup"><span data-stu-id="9c65f-180">Description</span></span> |
|:--- |:--- |
| <span data-ttu-id="9c65f-181">Prahová hodnota</span><span class="sxs-lookup"><span data-stu-id="9c65f-181">Threshold</span></span> |<span data-ttu-id="9c65f-182">Kritéria pro spuštění akce hello.</span><span class="sxs-lookup"><span data-stu-id="9c65f-182">Criteria for when hello action is run.</span></span> |
| <span data-ttu-id="9c65f-183">EmailNotification</span><span class="sxs-lookup"><span data-stu-id="9c65f-183">EmailNotification</span></span> |<span data-ttu-id="9c65f-184">Odesílat e-maily toomultiple příjemce.</span><span class="sxs-lookup"><span data-stu-id="9c65f-184">Send mail toomultiple recipients.</span></span> |
| <span data-ttu-id="9c65f-185">Nápravy</span><span class="sxs-lookup"><span data-stu-id="9c65f-185">Remediation</span></span> |<span data-ttu-id="9c65f-186">Spuštění sady runbook v Azure Automation tooattempt toocorrect identifikovat problém.</span><span class="sxs-lookup"><span data-stu-id="9c65f-186">Start a runbook in Azure Automation tooattempt toocorrect identified issue.</span></span> |

#### <a name="thresholds"></a><span data-ttu-id="9c65f-187">Prahové hodnoty</span><span class="sxs-lookup"><span data-stu-id="9c65f-187">Thresholds</span></span>
<span data-ttu-id="9c65f-188">Výstrahy akce by měl mít pouze jednu prahovou hodnotu.</span><span class="sxs-lookup"><span data-stu-id="9c65f-188">An Alert action should have one and only one threshold.</span></span>  <span data-ttu-id="9c65f-189">Pokud výsledky hello uloženého hledání neodpovídají hello prahovou hodnotu v akci spojené s toto hledání, jsou spuštěny žádné další procesy, které jsou v této akce.</span><span class="sxs-lookup"><span data-stu-id="9c65f-189">When hello results of a saved search match hello threshold in an action associated with that search, then any other processes in that action are run.</span></span>  <span data-ttu-id="9c65f-190">Akce může také obsahovat pouze prahovou hodnotu, aby se může použít s akcemi jiných typů, které neobsahují žádný prahové hodnoty.</span><span class="sxs-lookup"><span data-stu-id="9c65f-190">An action can also contain only a threshold so that it can be used with actions of other types that don’t contain thresholds.</span></span>

<span data-ttu-id="9c65f-191">Prahové hodnoty mají vlastnosti hello v hello následující tabulka.</span><span class="sxs-lookup"><span data-stu-id="9c65f-191">Thresholds have hello properties in hello following table.</span></span>

| <span data-ttu-id="9c65f-192">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="9c65f-192">Property</span></span> | <span data-ttu-id="9c65f-193">Popis</span><span class="sxs-lookup"><span data-stu-id="9c65f-193">Description</span></span> |
|:--- |:--- |
| <span data-ttu-id="9c65f-194">Operátor</span><span class="sxs-lookup"><span data-stu-id="9c65f-194">Operator</span></span> |<span data-ttu-id="9c65f-195">Operátor pro porovnání hello prahovou hodnotu.</span><span class="sxs-lookup"><span data-stu-id="9c65f-195">Operator for hello threshold comparison.</span></span> <br> <span data-ttu-id="9c65f-196">gt = větší než</span><span class="sxs-lookup"><span data-stu-id="9c65f-196">gt = Greater Than</span></span> <br> <span data-ttu-id="9c65f-197">lt = menší než</span><span class="sxs-lookup"><span data-stu-id="9c65f-197">lt = Less Than</span></span> |
| <span data-ttu-id="9c65f-198">Hodnota</span><span class="sxs-lookup"><span data-stu-id="9c65f-198">Value</span></span> |<span data-ttu-id="9c65f-199">Hodnota pro mezní hodnotu hello.</span><span class="sxs-lookup"><span data-stu-id="9c65f-199">Value for hello threshold.</span></span> |

<span data-ttu-id="9c65f-200">Představte si třeba dotazu událostí se v intervalu 15 minut, časový interval 30 minut a prahové hodnoty větší než 10.</span><span class="sxs-lookup"><span data-stu-id="9c65f-200">For example, consider an event query with an Interval of 15 minutes, a Timespan of 30 minutes, and a Threshold of greater than 10.</span></span> <span data-ttu-id="9c65f-201">V takovém případě by být hello dotaz spustit každých 15 minut a výstraha by spustí, pokud je vrácen 10 události, které byly vytvořeny během určitého 30 minut.</span><span class="sxs-lookup"><span data-stu-id="9c65f-201">In this case, hello query would be run every 15 minutes, and an alert would be triggered if it returned 10 events that were created over a 30 minute span.</span></span>

<span data-ttu-id="9c65f-202">Následuje ukázková odpověď pro akce s pouze prahovou hodnotu.</span><span class="sxs-lookup"><span data-stu-id="9c65f-202">Following is a sample response for an action with only a threshold.</span></span>  

    "etag": "W/\"datetime'2016-02-25T20%3A54%3A20.1302566Z'\"",
    "properties": {
        "Type": "Alert",
        "Name": "My threshold action",
        "Threshold": {
            "Operator": "gt",
            "Value": 10
        },
        "Version": 1
    }

<span data-ttu-id="9c65f-203">Použijte metodu Put hello s akce jedinečné ID toocreate novou akci prahové hodnoty pro plán.</span><span class="sxs-lookup"><span data-stu-id="9c65f-203">Use hello Put method with a unique action ID toocreate a new threshold action for a schedule.</span></span>  

    $thresholdJson = "{'properties': { 'Name': 'My Threshold', 'Version':'1', 'Type':'Alert', 'Threshold': { 'Operator': 'gt', 'Value': 10 } }"
    armclient put /subscriptions/{Subscription ID}/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/{Search ID}/schedules/{Schedule ID}/actions/mythreshold?api-version=2015-03-20 $thresholdJson

<span data-ttu-id="9c65f-204">Použijte metodu Put hello s existující toomodify ID akce akce prahové hodnoty pro plán.</span><span class="sxs-lookup"><span data-stu-id="9c65f-204">Use hello Put method with an existing action ID toomodify a threshold action for a schedule.</span></span>  <span data-ttu-id="9c65f-205">Hello textu hello požadavku musí obsahovat hello etag hello akce.</span><span class="sxs-lookup"><span data-stu-id="9c65f-205">hello body of hello request must include hello etag of hello action.</span></span>

    $thresholdJson = "{'etag': 'W/\"datetime'2016-02-25T20%3A54%3A20.1302566Z'\"','properties': { 'Name': 'My Threshold', 'Version':'1', 'Type':'Alert', 'Threshold': { 'Operator': 'gt', 'Value': 10 } }"
    armclient put /subscriptions/{Subscription ID}/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/{Search ID}/schedules/{Schedule ID}/actions/mythreshold?api-version=2015-03-20 $thresholdJson

#### <a name="email-notification"></a><span data-ttu-id="9c65f-206">E-mailových oznámení</span><span class="sxs-lookup"><span data-stu-id="9c65f-206">Email Notification</span></span>
<span data-ttu-id="9c65f-207">E-mailová oznámení odesílat e-mailu tooone nebo další příjemce.</span><span class="sxs-lookup"><span data-stu-id="9c65f-207">Email Notifications send mail tooone or more recipients.</span></span>  <span data-ttu-id="9c65f-208">Patří mezi ně hello vlastnosti v hello následující tabulka.</span><span class="sxs-lookup"><span data-stu-id="9c65f-208">They include hello properties in hello following table.</span></span>

| <span data-ttu-id="9c65f-209">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="9c65f-209">Property</span></span> | <span data-ttu-id="9c65f-210">Popis</span><span class="sxs-lookup"><span data-stu-id="9c65f-210">Description</span></span> |
|:--- |:--- |
| <span data-ttu-id="9c65f-211">Příjemce</span><span class="sxs-lookup"><span data-stu-id="9c65f-211">Recipients</span></span> |<span data-ttu-id="9c65f-212">Seznam adres e-mailu.</span><span class="sxs-lookup"><span data-stu-id="9c65f-212">List of mail addresses.</span></span> |
| <span data-ttu-id="9c65f-213">Předmět</span><span class="sxs-lookup"><span data-stu-id="9c65f-213">Subject</span></span> |<span data-ttu-id="9c65f-214">Hello předmět e-mailu hello.</span><span class="sxs-lookup"><span data-stu-id="9c65f-214">hello subject of hello mail.</span></span> |
| <span data-ttu-id="9c65f-215">Přílohy</span><span class="sxs-lookup"><span data-stu-id="9c65f-215">Attachment</span></span> |<span data-ttu-id="9c65f-216">Přílohy nejsou aktuálně podporovány, takže to bude mít vždy hodnotu "Žádný".</span><span class="sxs-lookup"><span data-stu-id="9c65f-216">Attachments are not currently supported, so this will always have a value of “None”.</span></span> |

<span data-ttu-id="9c65f-217">Následuje ukázková odpověď pro akci oznámení e-mailu s prahovou hodnotou.</span><span class="sxs-lookup"><span data-stu-id="9c65f-217">Following is a sample response for an email notification action with a threshold.</span></span>  

    "etag": "W/\"datetime'2016-02-25T20%3A54%3A20.1302566Z'\"",
    "properties": {
        "Type": "Alert",
        "Name": "My email action",
        "Threshold": {
            "Operator": "gt",
            "Value": 10
        },
        "EmailNotification": {
            "Recipients": [
                "recipient1@contoso.com",
                "recipient2@contoso.com"
            ],
            "Subject": "This is hello subject",
            "Attachment": "None"
        },
        "Version": 1
    }

<span data-ttu-id="9c65f-218">Použijte metodu Put hello s akce jedinečné ID toocreate novou akci e-mailu pro plán.</span><span class="sxs-lookup"><span data-stu-id="9c65f-218">Use hello Put method with a unique action ID toocreate a new e-mail action for a schedule.</span></span>  <span data-ttu-id="9c65f-219">Hello následující příklad vytvoří e-mailové oznámení s prahovou hodnotou, odešle hello e-mailu v případě, že výsledky hello hello uložené hledání překročit prahovou hodnotu hello.</span><span class="sxs-lookup"><span data-stu-id="9c65f-219">hello following example creates an email notification with a threshold so hello mail is sent when hello results of hello saved search exceed hello threshold.</span></span>

    $emailJson = "{'properties': { 'Name': 'MyEmailAction', 'Version':'1', 'Type':'Alert', 'Threshold': { 'Operator': 'gt', 'Value': 10 }, 'EmailNotification': {'Recipients': ['recipient1@contoso.com', 'recipient2@contoso.com'], 'Subject':'This is hello subject', 'Attachment':'None'} }"
    armclient put /subscriptions/{Subscription ID}/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/{Search ID}/schedules/{Schedule ID}/actions/myemailaction?api-version=2015-03-20 $emailJson

<span data-ttu-id="9c65f-220">Použijte metodu Put hello s existující toomodify ID akce akce e-mailu pro plán.</span><span class="sxs-lookup"><span data-stu-id="9c65f-220">Use hello Put method with an existing action ID toomodify an e-mail action for a schedule.</span></span>  <span data-ttu-id="9c65f-221">Hello textu hello požadavku musí obsahovat hello etag hello akce.</span><span class="sxs-lookup"><span data-stu-id="9c65f-221">hello body of hello request must include hello etag of hello action.</span></span>

    $emailJson = "{'etag': 'W/\"datetime'2016-02-25T20%3A54%3A20.1302566Z'\"','properties': { 'Name': 'MyEmailAction', 'Version':'1', 'Type':'Alert', 'Threshold': { 'Operator': 'gt', 'Value': 10 }, 'EmailNotification': {'Recipients': ['recipient1@contoso.com', 'recipient2@contoso.com'], 'Subject':'This is hello subject', 'Attachment':'None'} }"
    armclient put /subscriptions/{Subscription ID}/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/{Search ID}/schedules/{Schedule ID}/actions/myemailaction?api-version=2015-03-20 $emailJson

#### <a name="remediation-actions"></a><span data-ttu-id="9c65f-222">Akce nápravy</span><span class="sxs-lookup"><span data-stu-id="9c65f-222">Remediation actions</span></span>
<span data-ttu-id="9c65f-223">Nápravy spuštění sady runbook ve službě Azure Automation, která se pokusí problém hello toocorrect identifikovaný hello výstrahy.</span><span class="sxs-lookup"><span data-stu-id="9c65f-223">Remediations start a runbook in Azure Automation that attempts toocorrect hello problem identified by hello alert.</span></span>  <span data-ttu-id="9c65f-224">Musíte vytvořit webhooku pro sadu runbook hello používá v rámci nápravy akce a potom zadejte hello URI do hello WebhookUri vlastnost.</span><span class="sxs-lookup"><span data-stu-id="9c65f-224">You must create a webhook for hello runbook used in a remediation action and then specify hello URI in hello WebhookUri property.</span></span>  <span data-ttu-id="9c65f-225">Když vytvoříte tuto akci pomocí konzoly OMS hello, se automaticky vytvoří nové webhooku pro sadu runbook hello.</span><span class="sxs-lookup"><span data-stu-id="9c65f-225">When you create this action using hello OMS console, a new webhook is automatically created for hello runbook.</span></span>

<span data-ttu-id="9c65f-226">Nápravy zahrnout hello vlastnosti hello následující tabulka.</span><span class="sxs-lookup"><span data-stu-id="9c65f-226">Remediations include hello properties in hello following table.</span></span>

| <span data-ttu-id="9c65f-227">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="9c65f-227">Property</span></span> | <span data-ttu-id="9c65f-228">Popis</span><span class="sxs-lookup"><span data-stu-id="9c65f-228">Description</span></span> |
|:--- |:--- |
| <span data-ttu-id="9c65f-229">RunbookName</span><span class="sxs-lookup"><span data-stu-id="9c65f-229">RunbookName</span></span> |<span data-ttu-id="9c65f-230">Název sady runbook hello.</span><span class="sxs-lookup"><span data-stu-id="9c65f-230">Name of hello runbook.</span></span> <span data-ttu-id="9c65f-231">To se musí shodovat publikované sady runbook v účtu automation hello nakonfigurované v hello řešení služby Automation v pracovním prostoru OMS.</span><span class="sxs-lookup"><span data-stu-id="9c65f-231">This must match a published runbook in hello automation account configured in hello Automation Solution in your OMS workspace.</span></span> |
| <span data-ttu-id="9c65f-232">WebhookUri</span><span class="sxs-lookup"><span data-stu-id="9c65f-232">WebhookUri</span></span> |<span data-ttu-id="9c65f-233">Identifikátor URI služby webhooku hello.</span><span class="sxs-lookup"><span data-stu-id="9c65f-233">URI of hello webhook.</span></span> |
| <span data-ttu-id="9c65f-234">Vypršení platnosti</span><span class="sxs-lookup"><span data-stu-id="9c65f-234">Expiry</span></span> |<span data-ttu-id="9c65f-235">Datum vypršení platnosti Hello a čas webhooku hello.</span><span class="sxs-lookup"><span data-stu-id="9c65f-235">hello expiration date and time of hello webhook.</span></span>  <span data-ttu-id="9c65f-236">Pokud hello webhooku nemá vypršení platnosti, může to být žádné platné budoucí datum.</span><span class="sxs-lookup"><span data-stu-id="9c65f-236">If hello webhook doesn’t have an expiration, then this can be any valid future date.</span></span> |

<span data-ttu-id="9c65f-237">Následuje ukázková odpověď pro akci automatické nápravy s prahovou hodnotou.</span><span class="sxs-lookup"><span data-stu-id="9c65f-237">Following is a sample response for a remediation action with a threshold.</span></span>

    "etag": "W/\"datetime'2016-02-25T20%3A54%3A20.1302566Z'\"",
    "properties": {
        "Type": "Alert",
        "Name": "My remediation action",
        "Threshold": {
            "Operator": "gt",
            "Value": 10
        },
        "Remediation": {
            "RunbookName": "My-Runbook",
            "WebhookUri": "https://s1events.azure-automation.net/webhooks?token=4jCibOjO3w4W2Cfg%2b2NkjLYdafnusaG6i8tnP8h%2fNNg%3d",
            "Expiry": "2018-02-25T18:27:20"
            },
        "Version": 1
    }

<span data-ttu-id="9c65f-238">Použijte metodu Put hello s akce jedinečné ID toocreate novou akci automatické nápravy pro plán.</span><span class="sxs-lookup"><span data-stu-id="9c65f-238">Use hello Put method with a unique action ID toocreate a new remediation action for a schedule.</span></span>  <span data-ttu-id="9c65f-239">Hello následující příklad vytvoří nápravy s prahovou hodnotu, hello runbook je spuštěn, když hello výsledky hello uložené hledání překročit prahovou hodnotu hello.</span><span class="sxs-lookup"><span data-stu-id="9c65f-239">hello following example creates a remediation with a threshold so hello runbook is started when hello results of hello saved search exceed hello threshold.</span></span>

    $remediateJson = "{'properties': { 'Type':'Alert', 'Name': 'My Remediation Action', 'Version':'1', 'Threshold': { 'Operator': 'gt', 'Value': 10 }, 'Remediation': {'RunbookName': 'My-Runbook', 'WebhookUri':'https://s1events.azure-automation.net/webhooks?token=4jCibOjO3w4W2Cfg%2b2NkjLYdafnusaG6i8tnP8h%2fNNg%3d', 'Expiry':'2018-02-25T18:27:20Z'} }"
    armclient put /subscriptions/{Subscription ID}/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/{Search ID}/schedules/{Schedule ID}/actions/myremediationaction?api-version=2015-03-20 $remediateJson

<span data-ttu-id="9c65f-240">Použijte metodu Put hello s existující toomodify ID akce akci automatické nápravy pro plán.</span><span class="sxs-lookup"><span data-stu-id="9c65f-240">Use hello Put method with an existing action ID toomodify a remediation action for a schedule.</span></span>  <span data-ttu-id="9c65f-241">Hello textu hello požadavku musí obsahovat hello etag hello akce.</span><span class="sxs-lookup"><span data-stu-id="9c65f-241">hello body of hello request must include hello etag of hello action.</span></span>

    $remediateJson = "{'etag': 'W/\"datetime'2016-02-25T20%3A54%3A20.1302566Z'\"','properties': { 'Type':'Alert', 'Name': 'My Remediation Action', 'Version':'1', 'Threshold': { 'Operator': 'gt', 'Value': 10 }, 'Remediation': {'RunbookName': 'My-Runbook', 'WebhookUri':'https://s1events.azure-automation.net/webhooks?token=4jCibOjO3w4W2Cfg%2b2NkjLYdafnusaG6i8tnP8h%2fNNg%3d', 'Expiry':'2018-02-25T18:27:20Z'} }"
    armclient put /subscriptions/{Subscription ID}/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/{Search ID}/schedules/{Schedule ID}/actions/myremediationaction?api-version=2015-03-20 $remediateJson

#### <a name="example"></a><span data-ttu-id="9c65f-242">Příklad</span><span class="sxs-lookup"><span data-stu-id="9c65f-242">Example</span></span>
<span data-ttu-id="9c65f-243">Toto je kompletní příklad toocreate novou e-mailové výstrahy.</span><span class="sxs-lookup"><span data-stu-id="9c65f-243">Following is a complete example toocreate a new email alert.</span></span>  <span data-ttu-id="9c65f-244">Tím se vytvoří nový plán spolu s akce obsahující prahovou hodnotu a e-mailu.</span><span class="sxs-lookup"><span data-stu-id="9c65f-244">This creates a new schedule along with an action containing a threshold and email.</span></span>

    $subscriptionId = "3d56705e-5b26-5bcc-9368-dbc8d2fafbfc"
    $resourceGroup  = "MyResourceGroup"    
    $workspaceName    = "MyWorkspace"
    $searchId       = "MySearch"
    $scheduleId     = "MySchedule"
    $thresholdId    = "MyThreshold"
    $actionId       = "MyEmailAction"

    $scheduleJson = "{'properties': { 'Interval': 15, 'QueryTimeSpan':15, 'Active':'true' }"
    armclient put /subscriptions/$subscriptionId/resourceGroups/$resourceGroup/providers/Microsoft.OperationalInsights/workspaces/$workspaceName/savedSearches/$searchId/schedules/$scheduleId/?api-version=2015-03-20 $scheduleJson

    $emailJson = "{'properties': { 'Name': 'MyEmailAction', 'Version':'1', 'Severity':'Warning', 'Type':'Alert', 'Threshold': { 'Operator': 'gt', 'Value': 10 }, 'EmailNotification': {'Recipients': ['recipient1@contoso.com', 'recipient2@contoso.com'], 'Subject':'This is hello subject', 'Attachment':'None'} }"
    armclient put /subscriptions/$subscriptionId/resourceGroups/$resourceGroup/providers/Microsoft.OperationalInsights/workspaces/$workspaceName/savedSearches/$searchId/schedules/$scheduleId/actions/$actionId/?api-version=2015-03-20 $emailJson

### <a name="webhook-actions"></a><span data-ttu-id="9c65f-245">Akce Webhooku</span><span class="sxs-lookup"><span data-stu-id="9c65f-245">Webhook actions</span></span>
<span data-ttu-id="9c65f-246">Akce Webhooku spuštění procesu voláním adresu URL a volitelně poskytuje datové části toobe, odeslána.</span><span class="sxs-lookup"><span data-stu-id="9c65f-246">Webhook actions start a process by calling a URL and optionally providing a payload toobe sent.</span></span>  <span data-ttu-id="9c65f-247">Jsou podobné tooRemediation akce s výjimkou jsou určené pro webhooků, který může vyvolat procesy než Azure Automation runbook.</span><span class="sxs-lookup"><span data-stu-id="9c65f-247">They are similar tooRemediation actions except they are meant for webhooks that may invoke processes other than Azure Automation runbooks.</span></span>  <span data-ttu-id="9c65f-248">Obsahují taky hello další možnost poskytnout vzdáleného procesu toohello toobe doručovat datové části.</span><span class="sxs-lookup"><span data-stu-id="9c65f-248">They also provide hello additional option of providing a payload toobe delivered toohello remote process.</span></span>

<span data-ttu-id="9c65f-249">Akce Webhooku nemáte prahovou hodnotu, ale místo toho musí být přidaní tooa plán, který má výstrahy akce s prahovou hodnotou.</span><span class="sxs-lookup"><span data-stu-id="9c65f-249">Webhook actions do not have a threshold but instead should be added tooa schedule that has an Alert action with a threshold.</span></span>  <span data-ttu-id="9c65f-250">Můžete přidat více Webhooku akcí, které se všechny spustí při splnění hello prahovou hodnotu.</span><span class="sxs-lookup"><span data-stu-id="9c65f-250">You can add multiple Webhook actions that will all be run when hello threshold is met.</span></span>

<span data-ttu-id="9c65f-251">Akce Webhooku zahrnout hello vlastnosti hello následující tabulka.</span><span class="sxs-lookup"><span data-stu-id="9c65f-251">Webhook actions include hello properties in hello following table.</span></span>

| <span data-ttu-id="9c65f-252">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="9c65f-252">Property</span></span> | <span data-ttu-id="9c65f-253">Popis</span><span class="sxs-lookup"><span data-stu-id="9c65f-253">Description</span></span> |
|:--- |:--- |
| <span data-ttu-id="9c65f-254">WebhookUri</span><span class="sxs-lookup"><span data-stu-id="9c65f-254">WebhookUri</span></span> |<span data-ttu-id="9c65f-255">Hello předmět e-mailu hello.</span><span class="sxs-lookup"><span data-stu-id="9c65f-255">hello subject of hello mail.</span></span> |
| <span data-ttu-id="9c65f-256">CustomPayload</span><span class="sxs-lookup"><span data-stu-id="9c65f-256">CustomPayload</span></span> |<span data-ttu-id="9c65f-257">Webhooku toohello toobe odeslat vlastní datovou část.</span><span class="sxs-lookup"><span data-stu-id="9c65f-257">Custom payload toobe sent toohello webhook.</span></span>  <span data-ttu-id="9c65f-258">Formát Hello bude záviset na jaké hello webhooku očekává.</span><span class="sxs-lookup"><span data-stu-id="9c65f-258">hello format will depend on what hello webhook is expecting.</span></span> |

<span data-ttu-id="9c65f-259">Následuje ukázková odpověď pro akce webhooku a přidružené akce výstrah s prahovou hodnotou.</span><span class="sxs-lookup"><span data-stu-id="9c65f-259">Following is a sample response for webhook action and an associated alert action with a threshold.</span></span>

    {
        "__metadata": {},
        "value": [
            {
                "id": "subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/bwren/savedSearches/2d1b30fb-7f48-4de5-9614-79ee244b52de/schedules/b80f5621-7217-4007-b32d-165d14377093/Actions/72884702-acf9-4653-bb67-f42436b342b4",
                "etag": "W/\"datetime'2016-02-26T20%3A25%3A00.6862124Z'\"",
                "properties": {
                    "Type": "Webhook",
                    "Name": "My Webhook Action",
                    "WebhookUri": "https://oaaswebhookdf.cloudapp.net/webhooks?token=VfkYTIlpk%2fc%2bJBP",
                    "CustomPayload": "{\"fielld1\":\"value1\",\"field2\":\"value2\"}",
                    "Version": 1
                }
            },
            {
                "id": "subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/bwren/savedSearches/2d1b30fb-7f48-4de5-9614-79ee244b52de/schedules/b80f5621-7217-4007-b32d-165d14377093/Actions/90a27cf8-71b7-4df2-b04f-54ed01f1e4b6",
                "etag": "W/\"datetime'2016-02-26T20%3A25%3A00.565204Z'\"",
                "properties": {
                    "Type": "Alert",
                    "Name": "Threshold for my webhook action",
                    "Threshold": {
                        "Operator": "gt",
                        "Value": 10
                    },
                    "Version": 1
                }
            }
        ]
    }

#### <a name="create-or-edit-a-webhook-action"></a><span data-ttu-id="9c65f-260">Vytvořte nebo upravte akce webhooku</span><span class="sxs-lookup"><span data-stu-id="9c65f-260">Create or edit a webhook action</span></span>
<span data-ttu-id="9c65f-261">Použijte metodu Put hello s akce jedinečné ID toocreate Nová akce webhooku pro plán.</span><span class="sxs-lookup"><span data-stu-id="9c65f-261">Use hello Put method with a unique action ID toocreate a new webhook action for a schedule.</span></span>  <span data-ttu-id="9c65f-262">Hello následující příklad vytvoří akce Webhooku a výstrah akce s prahovou hodnotu, aby hello webhooku se aktivuje, když hello výsledky hello uložené hledání překročit prahovou hodnotu hello.</span><span class="sxs-lookup"><span data-stu-id="9c65f-262">hello following example creates a Webhook action and an Alert action with a threshold so that hello webhook will be triggered when hello results of hello saved search exceed hello threshold.</span></span>

    $thresholdAction = "{'properties': { 'Name': 'My Threshold', 'Version':'1', 'Type':'Alert', 'Threshold': { 'Operator': 'gt', 'Value': 10 } }"
    armclient put /subscriptions/{Subscription ID}/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/{Search ID}/schedules/{Schedule ID}/actions/mythreshold?api-version=2015-03-20 $thresholdAction

    $webhookAction = "{'properties': {'Type': 'Webhook', 'Name': 'My Webhook", 'WebhookUri': 'https://oaaswebhookdf.cloudapp.net/webhooks?token=VrkYTKlhk%2fc%2bKBP', 'CustomPayload': '{\"field1\":\"value1\",\"field2\":\"value2\"}', 'Version': 1 }"
    armclient put /subscriptions/{Subscription ID}/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/{Search ID}/schedules/{Schedule ID}/actions/mywebhookaction?api-version=2015-03-20 $webhookAction

<span data-ttu-id="9c65f-263">Použijte metodu Put hello s existující toomodify ID akce akce webhooku pro plán.</span><span class="sxs-lookup"><span data-stu-id="9c65f-263">Use hello Put method with an existing action ID toomodify a webhook action for a schedule.</span></span>  <span data-ttu-id="9c65f-264">Hello textu hello požadavku musí obsahovat hello etag hello akce.</span><span class="sxs-lookup"><span data-stu-id="9c65f-264">hello body of hello request must include hello etag of hello action.</span></span>

    $webhookAction = "{'etag': 'W/\"datetime'2016-02-26T20%3A25%3A00.6862124Z'\"','properties': {'Type': 'Webhook', 'Name': 'My Webhook", 'WebhookUri': 'https://oaaswebhookdf.cloudapp.net/webhooks?token=VrkYTKlhk%2fc%2bKBP', 'CustomPayload': '{\"field1\":\"value1\",\"field2\":\"value2\"}', 'Version': 1 }"
    armclient put /subscriptions/{Subscription ID}/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/{Search ID}/schedules/{Schedule ID}/actions/mywebhookaction?api-version=2015-03-20 $webhookAction

## <a name="next-steps"></a><span data-ttu-id="9c65f-265">Další kroky</span><span class="sxs-lookup"><span data-stu-id="9c65f-265">Next steps</span></span>
* <span data-ttu-id="9c65f-266">Použití hello [REST API tooperform protokolu hledání](log-analytics-log-search-api.md) v analýzy protokolů.</span><span class="sxs-lookup"><span data-stu-id="9c65f-266">Use hello [REST API tooperform log searches](log-analytics-log-search-api.md) in Log Analytics.</span></span>

