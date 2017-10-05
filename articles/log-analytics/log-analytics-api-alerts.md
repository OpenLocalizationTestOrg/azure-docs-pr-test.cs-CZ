---
title: "Pomocí OMS Log Analytics výstrahy REST API"
description: "Log Analytics výstrahy REST API umožňuje vytvářet a spravovat výstrahy v analýzy protokolů, která je součástí služby Operations Management Suite (OMS).  Tento článek obsahuje podrobné informace o rozhraní API a několik příkladů pro provádění různých akcí."
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
ms.openlocfilehash: 5ce72ffef4394bf3bbe39fa420c4fcaa965ae35c
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="create-and-manage-alert-rules-in-log-analytics-with-rest-api"></a><span data-ttu-id="ff85d-104">Vytvářet a spravovat pravidla výstrah v analýzy protokolů pomocí rozhraní REST API</span><span class="sxs-lookup"><span data-stu-id="ff85d-104">Create and manage alert rules in Log Analytics with REST API</span></span>
<span data-ttu-id="ff85d-105">Log Analytics výstrahy REST API umožňuje vytvářet a spravovat výstrahy v Operations Management Suite (OMS).</span><span class="sxs-lookup"><span data-stu-id="ff85d-105">The Log Analytics Alert REST API allows you to create and manage alerts in Operations Management Suite (OMS).</span></span>  <span data-ttu-id="ff85d-106">Tento článek obsahuje podrobné informace o rozhraní API a několik příkladů pro provádění různých akcí.</span><span class="sxs-lookup"><span data-stu-id="ff85d-106">This article provides details of the API and several examples for performing different operations.</span></span>

<span data-ttu-id="ff85d-107">Rozhraní REST API Log Analytics Search je dosáhl standardu RESTful a je přístupný prostřednictvím rozhraní REST API Azure Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="ff85d-107">The Log Analytics Search REST API is RESTful and can be accessed via the Azure Resource Manager REST API.</span></span> <span data-ttu-id="ff85d-108">V tomto dokumentu najdete příklady kterých je přístup k rozhraní API z příkazového řádku pomocí prostředí PowerShell [ARMClient](https://github.com/projectkudu/ARMClient), nástroj pro příkazový řádek s otevřeným zdrojem, který zjednodušuje volání rozhraní API služby Azure Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="ff85d-108">In this document you will find examples where the API is accessed from a PowerShell command line using  [ARMClient](https://github.com/projectkudu/ARMClient), an open source command line tool that simplifies invoking the Azure Resource Manager API.</span></span> <span data-ttu-id="ff85d-109">Použití ARMClient a prostředí PowerShell je jedním z mnoha možností pro přístup k rozhraní API pro vyhledávání Analytics protokolu.</span><span class="sxs-lookup"><span data-stu-id="ff85d-109">The use of ARMClient and PowerShell is one of many options to access the Log Analytics Search API.</span></span> <span data-ttu-id="ff85d-110">Pomocí těchto nástrojů můžete využít rozhraní RESTful API Správce prostředků Azure provádět volání do OMS pracovních prostorů a provádět příkazy vyhledávání v nich.</span><span class="sxs-lookup"><span data-stu-id="ff85d-110">With these tools you can utilize the RESTful Azure Resource Manager API to make calls to OMS workspaces and perform search commands within them.</span></span> <span data-ttu-id="ff85d-111">Rozhraní API výstup výsledků vyhledávání do je ve formátu JSON, budete moci použít výsledky hledání v mnoha různými způsoby prostřednictvím kódu programu.</span><span class="sxs-lookup"><span data-stu-id="ff85d-111">The API will output search results to you in JSON format, allowing you to use the search results in many different ways programmatically.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="ff85d-112">Požadavky</span><span class="sxs-lookup"><span data-stu-id="ff85d-112">Prerequisites</span></span>
<span data-ttu-id="ff85d-113">V současné době mohou výstrahy vytvořeny pouze s uloženého hledání v analýzy protokolů.</span><span class="sxs-lookup"><span data-stu-id="ff85d-113">Currently, alerts can only be created with a saved search in Log Analytics.</span></span>  <span data-ttu-id="ff85d-114">Můžete se podívat do [rozhraní API REST vyhledávání protokolu](log-analytics-log-search-api.md) Další informace.</span><span class="sxs-lookup"><span data-stu-id="ff85d-114">You can refer to the [Log Search REST API](log-analytics-log-search-api.md) for more information.</span></span>

## <a name="schedules"></a><span data-ttu-id="ff85d-115">Plány</span><span class="sxs-lookup"><span data-stu-id="ff85d-115">Schedules</span></span>
<span data-ttu-id="ff85d-116">Uložené hledání může mít jeden nebo více plánů.</span><span class="sxs-lookup"><span data-stu-id="ff85d-116">A saved search can have one or more schedules.</span></span> <span data-ttu-id="ff85d-117">Plán definuje, jak často hledání je spustit a časový interval, za které se identifikuje kritéria.</span><span class="sxs-lookup"><span data-stu-id="ff85d-117">The schedule defines how often the search is run and the time interval over which the criteria is identified.</span></span>
<span data-ttu-id="ff85d-118">Plány mít vlastnosti v následující tabulce.</span><span class="sxs-lookup"><span data-stu-id="ff85d-118">Schedules have the properties in the following table.</span></span>

| <span data-ttu-id="ff85d-119">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="ff85d-119">Property</span></span> | <span data-ttu-id="ff85d-120">Popis</span><span class="sxs-lookup"><span data-stu-id="ff85d-120">Description</span></span> |
|:--- |:--- |
| <span data-ttu-id="ff85d-121">Interval</span><span class="sxs-lookup"><span data-stu-id="ff85d-121">Interval</span></span> |<span data-ttu-id="ff85d-122">Jak často je spustit hledání.</span><span class="sxs-lookup"><span data-stu-id="ff85d-122">How often the search is run.</span></span> <span data-ttu-id="ff85d-123">Měří v minutách.</span><span class="sxs-lookup"><span data-stu-id="ff85d-123">Measured in minutes.</span></span> |
| <span data-ttu-id="ff85d-124">QueryTimeSpan</span><span class="sxs-lookup"><span data-stu-id="ff85d-124">QueryTimeSpan</span></span> |<span data-ttu-id="ff85d-125">Časový interval, za které se vyhodnotí kritéria.</span><span class="sxs-lookup"><span data-stu-id="ff85d-125">The time interval over which the criteria is evaluated.</span></span> <span data-ttu-id="ff85d-126">Musí být rovna nebo větší než Interval.</span><span class="sxs-lookup"><span data-stu-id="ff85d-126">Must be equal to or greater than Interval.</span></span> <span data-ttu-id="ff85d-127">Měří v minutách.</span><span class="sxs-lookup"><span data-stu-id="ff85d-127">Measured in minutes.</span></span> |
| <span data-ttu-id="ff85d-128">Verze</span><span class="sxs-lookup"><span data-stu-id="ff85d-128">Version</span></span> |<span data-ttu-id="ff85d-129">Verze rozhraní API používá.</span><span class="sxs-lookup"><span data-stu-id="ff85d-129">The API version being used.</span></span>  <span data-ttu-id="ff85d-130">V současné době to musí být vždy nastavená na hodnotu 1.</span><span class="sxs-lookup"><span data-stu-id="ff85d-130">Currently, this should always be set to 1.</span></span> |

<span data-ttu-id="ff85d-131">Představte si třeba dotazu událostí s Interval 15 minut a časový interval 30 minut.</span><span class="sxs-lookup"><span data-stu-id="ff85d-131">For example, consider an event query with an Interval of 15 minutes and a Timespan of 30 minutes.</span></span> <span data-ttu-id="ff85d-132">V takovém případě by se spustí dotaz každých 15 minut a výstraha by spustí, pokud kritéria dál odkazující na hodnotu true přes rozpětí 30 minut.</span><span class="sxs-lookup"><span data-stu-id="ff85d-132">In this case, the query would be run every 15 minutes, and an alert would be triggered if the criteria continued to resolve to true over a 30 minute span.</span></span>

### <a name="retrieving-schedules"></a><span data-ttu-id="ff85d-133">Načítání plány</span><span class="sxs-lookup"><span data-stu-id="ff85d-133">Retrieving schedules</span></span>
<span data-ttu-id="ff85d-134">Umožňuje načíst všechny plány uložených hledání metodu Get.</span><span class="sxs-lookup"><span data-stu-id="ff85d-134">Use the Get method to retrieve all schedules for a saved search.</span></span>

    armclient get /subscriptions/{Subscription ID}/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/{Search  ID}/schedules?api-version=2015-03-20

<span data-ttu-id="ff85d-135">Pomocí této metody Get s ID plánu načíst konkrétní plán uložených hledání.</span><span class="sxs-lookup"><span data-stu-id="ff85d-135">Use the Get method with a schedule ID to retrieve a particular schedule for a saved search.</span></span>

    armclient get /subscriptions/{Subscription ID}/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/{Subscription ID}/schedules/{Schedule ID}?api-version=2015-03-20

<span data-ttu-id="ff85d-136">Následuje ukázková odpověď pro plán.</span><span class="sxs-lookup"><span data-stu-id="ff85d-136">Following is a sample response for a schedule.</span></span>

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

### <a name="creating-a-schedule"></a><span data-ttu-id="ff85d-137">Vytvoření plánu</span><span class="sxs-lookup"><span data-stu-id="ff85d-137">Creating a schedule</span></span>
<span data-ttu-id="ff85d-138">K vytvoření nového plánu použijte metodu Put s ID plánu jedinečný.</span><span class="sxs-lookup"><span data-stu-id="ff85d-138">Use the Put method with a unique schedule ID to create a new schedule.</span></span>  <span data-ttu-id="ff85d-139">Všimněte si, že dvě plány nemůže mít stejné ID i v případě, že jsou spojeny s jinou uložená hledání.</span><span class="sxs-lookup"><span data-stu-id="ff85d-139">Note that two schedules cannot have the same ID even if they are associated with different saved searches.</span></span>  <span data-ttu-id="ff85d-140">Při vytváření plánu v konzole OMS, se vytvoří identifikátor GUID pro ID plánu.</span><span class="sxs-lookup"><span data-stu-id="ff85d-140">When you create a schedule in the OMS console, a GUID is created for the schedule ID.</span></span>

> [!NOTE]
> <span data-ttu-id="ff85d-141">Název pro všechny uložená hledání, plány a akce, které jsou vytvořené pomocí rozhraní API Log Analytics musí být malými písmeny.</span><span class="sxs-lookup"><span data-stu-id="ff85d-141">The name for all saved searches, schedules, and actions created with the Log Analytics API must be in lowercase.</span></span>

    $scheduleJson = "{'properties': { 'Interval': 15, 'QueryTimeSpan':15, 'Active':'true' } }"
    armclient put /subscriptions/{Subscription ID}/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/{Search ID}/schedules/mynewschedule?api-version=2015-03-20 $scheduleJson

### <a name="editing-a-schedule"></a><span data-ttu-id="ff85d-142">Úprava plánu</span><span class="sxs-lookup"><span data-stu-id="ff85d-142">Editing a schedule</span></span>
<span data-ttu-id="ff85d-143">K úpravě tento plán, použijte metodu Put s existující ID plánu pro stejné uloženého hledání.</span><span class="sxs-lookup"><span data-stu-id="ff85d-143">Use the Put method with an existing schedule ID for the same saved search to modify that schedule.</span></span>  <span data-ttu-id="ff85d-144">Text žádosti musí obsahovat značku etag plánu.</span><span class="sxs-lookup"><span data-stu-id="ff85d-144">The body of the request must include the etag of the schedule.</span></span>

      $scheduleJson = "{'etag': 'W/\"datetime'2016-02-25T20%3A54%3A49.8074679Z'\""','properties': { 'Interval': 15, 'QueryTimeSpan':15, 'Active':'true' } }"
      armclient put /subscriptions/{Subscription ID}/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/{Search ID}/schedules/mynewschedule?api-version=2015-03-20 $scheduleJson


### <a name="deleting-schedules"></a><span data-ttu-id="ff85d-145">Odstraňování plány</span><span class="sxs-lookup"><span data-stu-id="ff85d-145">Deleting schedules</span></span>
<span data-ttu-id="ff85d-146">Chcete-li odstranit plán pomocí ID plánu metodu Delete.</span><span class="sxs-lookup"><span data-stu-id="ff85d-146">Use the Delete method with a schedule ID to delete a schedule.</span></span>

    armclient delete /subscriptions/{Subscription ID}/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/{Subscription ID}/schedules/{Schedule ID}?api-version=2015-03-20


## <a name="actions"></a><span data-ttu-id="ff85d-147">Akce</span><span class="sxs-lookup"><span data-stu-id="ff85d-147">Actions</span></span>
<span data-ttu-id="ff85d-148">Plán může mít více akcí.</span><span class="sxs-lookup"><span data-stu-id="ff85d-148">A schedule can have multiple actions.</span></span> <span data-ttu-id="ff85d-149">Akce může definovat jeden nebo více procesy provést například odesílání e-mailu nebo spuštění sady runbook, nebo se může definovat prahové hodnoty, která určuje, kdy výsledky hledání kritériím některé.</span><span class="sxs-lookup"><span data-stu-id="ff85d-149">An action may define one or more processes to perform such as sending a mail or starting a runbook, or it may define a threshold that determines when the results of a search match some criteria.</span></span>  <span data-ttu-id="ff85d-150">Některé akce obě bude definovat, aby procesy provede, když je splněna prahovou hodnotu.</span><span class="sxs-lookup"><span data-stu-id="ff85d-150">Some actions will define both so that the processes are performed when the threshold is met.</span></span>

<span data-ttu-id="ff85d-151">Všechny akce mít vlastnosti v následující tabulce.</span><span class="sxs-lookup"><span data-stu-id="ff85d-151">All actions have the properties in the following table.</span></span>  <span data-ttu-id="ff85d-152">Různé typy výstrah mají různé další vlastnosti, které jsou popsány níže.</span><span class="sxs-lookup"><span data-stu-id="ff85d-152">Different types of alerts have different additional properties which are described below.</span></span>

| <span data-ttu-id="ff85d-153">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="ff85d-153">Property</span></span> | <span data-ttu-id="ff85d-154">Popis</span><span class="sxs-lookup"><span data-stu-id="ff85d-154">Description</span></span> |
|:--- |:--- |
| <span data-ttu-id="ff85d-155">Typ</span><span class="sxs-lookup"><span data-stu-id="ff85d-155">Type</span></span> |<span data-ttu-id="ff85d-156">Typ akce.</span><span class="sxs-lookup"><span data-stu-id="ff85d-156">Type of the action.</span></span>  <span data-ttu-id="ff85d-157">Možné hodnoty jsou aktuálně upozornění a Webhooku.</span><span class="sxs-lookup"><span data-stu-id="ff85d-157">Currently the possible values are Alert and Webhook.</span></span> |
| <span data-ttu-id="ff85d-158">Name (Název)</span><span class="sxs-lookup"><span data-stu-id="ff85d-158">Name</span></span> |<span data-ttu-id="ff85d-159">Zobrazovaný název výstrahy.</span><span class="sxs-lookup"><span data-stu-id="ff85d-159">Display name for the alert.</span></span> |
| <span data-ttu-id="ff85d-160">Verze</span><span class="sxs-lookup"><span data-stu-id="ff85d-160">Version</span></span> |<span data-ttu-id="ff85d-161">Verze rozhraní API používá.</span><span class="sxs-lookup"><span data-stu-id="ff85d-161">The API version being used.</span></span>  <span data-ttu-id="ff85d-162">V současné době to musí být vždy nastavená na hodnotu 1.</span><span class="sxs-lookup"><span data-stu-id="ff85d-162">Currently, this should always be set to 1.</span></span> |

### <a name="retrieving-actions"></a><span data-ttu-id="ff85d-163">Načítání akce</span><span class="sxs-lookup"><span data-stu-id="ff85d-163">Retrieving actions</span></span>
<span data-ttu-id="ff85d-164">Umožňuje načíst všechny akce pro plán metodu Get.</span><span class="sxs-lookup"><span data-stu-id="ff85d-164">Use the Get method to retrieve all actions for a schedule.</span></span>

    armclient get /subscriptions/{Subscription ID}/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/{Search  ID}/schedules/{Schedule ID}/actions?api-version=2015-03-20

<span data-ttu-id="ff85d-165">Pomocí metody Get s ID akce pro načtení určité akce pro plán.</span><span class="sxs-lookup"><span data-stu-id="ff85d-165">Use the Get method with the action ID to retrieve a particular action for a schedule.</span></span>

    armclient get /subscriptions/{Subscription ID}/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/{Subscription ID}/schedules/{Schedule ID}/actions/{Action ID}?api-version=2015-03-20

### <a name="creating-or-editing-actions"></a><span data-ttu-id="ff85d-166">Vytvořením nebo úpravou akce</span><span class="sxs-lookup"><span data-stu-id="ff85d-166">Creating or editing actions</span></span>
<span data-ttu-id="ff85d-167">Použijte metodu Put s ID akce, které jsou jedinečné pro plán pro vytvoření nové akce.</span><span class="sxs-lookup"><span data-stu-id="ff85d-167">Use the Put method with an action ID that is unique to the schedule to create a new action.</span></span>  <span data-ttu-id="ff85d-168">Když vytvoříte v konzole OMS akce, je identifikátor GUID pro ID akce.</span><span class="sxs-lookup"><span data-stu-id="ff85d-168">When you create an action in the OMS console, a GUID is for the action ID.</span></span>

> [!NOTE]
> <span data-ttu-id="ff85d-169">Název pro všechny uložená hledání, plány a akce, které jsou vytvořené pomocí rozhraní API Log Analytics musí být malými písmeny.</span><span class="sxs-lookup"><span data-stu-id="ff85d-169">The name for all saved searches, schedules, and actions created with the Log Analytics API must be in lowercase.</span></span>

<span data-ttu-id="ff85d-170">K úpravě tento plán, použijte metodu Put s existující ID akce pro stejné uloženého hledání.</span><span class="sxs-lookup"><span data-stu-id="ff85d-170">Use the Put method with an existing action ID for the same saved search to modify that schedule.</span></span>  <span data-ttu-id="ff85d-171">Text žádosti musí obsahovat značku etag plánu.</span><span class="sxs-lookup"><span data-stu-id="ff85d-171">The body of the request must include the etag of the schedule.</span></span>

<span data-ttu-id="ff85d-172">Formát požadavku pro vytvoření nové akce se liší podle typu akce, takže tyto příklady jsou uvedeny v následujících částech.</span><span class="sxs-lookup"><span data-stu-id="ff85d-172">The request format for creating a new action varies by action type so these examples are provided in the sections below.</span></span>

### <a name="deleting-actions"></a><span data-ttu-id="ff85d-173">Odstranění akcí</span><span class="sxs-lookup"><span data-stu-id="ff85d-173">Deleting actions</span></span>
<span data-ttu-id="ff85d-174">Použijte metodu Delete s ID akce k odstranění akce.</span><span class="sxs-lookup"><span data-stu-id="ff85d-174">Use the Delete method with the action ID to delete an action.</span></span>

    armclient delete /subscriptions/{Subscription ID}/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/{Subscription ID}/schedules/{Schedule ID}/Actions/{Action ID}?api-version=2015-03-20

### <a name="alert-actions"></a><span data-ttu-id="ff85d-175">Akce výstrah</span><span class="sxs-lookup"><span data-stu-id="ff85d-175">Alert Actions</span></span>
<span data-ttu-id="ff85d-176">Plán by měl mít pouze jeden výstrahy akce.</span><span class="sxs-lookup"><span data-stu-id="ff85d-176">A Schedule should have one and only one Alert action.</span></span>  <span data-ttu-id="ff85d-177">Jeden nebo více oddílů v následující tabulce, které se mají výstrahy akce.</span><span class="sxs-lookup"><span data-stu-id="ff85d-177">Alert actions have one or more of the sections in the following table.</span></span>  <span data-ttu-id="ff85d-178">Každý je podrobně popsaná v další níže.</span><span class="sxs-lookup"><span data-stu-id="ff85d-178">Each is described in further detail below.</span></span>

| <span data-ttu-id="ff85d-179">Část</span><span class="sxs-lookup"><span data-stu-id="ff85d-179">Section</span></span> | <span data-ttu-id="ff85d-180">Popis</span><span class="sxs-lookup"><span data-stu-id="ff85d-180">Description</span></span> |
|:--- |:--- |
| <span data-ttu-id="ff85d-181">Prahová hodnota</span><span class="sxs-lookup"><span data-stu-id="ff85d-181">Threshold</span></span> |<span data-ttu-id="ff85d-182">Kritéria pro spuštění akce.</span><span class="sxs-lookup"><span data-stu-id="ff85d-182">Criteria for when the action is run.</span></span> |
| <span data-ttu-id="ff85d-183">EmailNotification</span><span class="sxs-lookup"><span data-stu-id="ff85d-183">EmailNotification</span></span> |<span data-ttu-id="ff85d-184">Odesílat e-maily několika příjemcům.</span><span class="sxs-lookup"><span data-stu-id="ff85d-184">Send mail to multiple recipients.</span></span> |
| <span data-ttu-id="ff85d-185">Nápravy</span><span class="sxs-lookup"><span data-stu-id="ff85d-185">Remediation</span></span> |<span data-ttu-id="ff85d-186">Spuštění sady runbook ve službě Azure Automation se pokuste odstranit zjištěný problém.</span><span class="sxs-lookup"><span data-stu-id="ff85d-186">Start a runbook in Azure Automation to attempt to correct identified issue.</span></span> |

#### <a name="thresholds"></a><span data-ttu-id="ff85d-187">Prahové hodnoty</span><span class="sxs-lookup"><span data-stu-id="ff85d-187">Thresholds</span></span>
<span data-ttu-id="ff85d-188">Výstrahy akce by měl mít pouze jednu prahovou hodnotu.</span><span class="sxs-lookup"><span data-stu-id="ff85d-188">An Alert action should have one and only one threshold.</span></span>  <span data-ttu-id="ff85d-189">Pokud výsledky uloženého hledání neodpovídají prahovou hodnotu v akci spojené s toto hledání, jsou spuštěny žádné další procesy, které jsou v této akce.</span><span class="sxs-lookup"><span data-stu-id="ff85d-189">When the results of a saved search match the threshold in an action associated with that search, then any other processes in that action are run.</span></span>  <span data-ttu-id="ff85d-190">Akce může také obsahovat pouze prahovou hodnotu, aby se může použít s akcemi jiných typů, které neobsahují žádný prahové hodnoty.</span><span class="sxs-lookup"><span data-stu-id="ff85d-190">An action can also contain only a threshold so that it can be used with actions of other types that don’t contain thresholds.</span></span>

<span data-ttu-id="ff85d-191">Prahové hodnoty mít vlastnosti v následující tabulce.</span><span class="sxs-lookup"><span data-stu-id="ff85d-191">Thresholds have the properties in the following table.</span></span>

| <span data-ttu-id="ff85d-192">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="ff85d-192">Property</span></span> | <span data-ttu-id="ff85d-193">Popis</span><span class="sxs-lookup"><span data-stu-id="ff85d-193">Description</span></span> |
|:--- |:--- |
| <span data-ttu-id="ff85d-194">Operátor</span><span class="sxs-lookup"><span data-stu-id="ff85d-194">Operator</span></span> |<span data-ttu-id="ff85d-195">Operátor pro porovnání prahovou hodnotu.</span><span class="sxs-lookup"><span data-stu-id="ff85d-195">Operator for the threshold comparison.</span></span> <br> <span data-ttu-id="ff85d-196">gt = větší než</span><span class="sxs-lookup"><span data-stu-id="ff85d-196">gt = Greater Than</span></span> <br> <span data-ttu-id="ff85d-197">lt = menší než</span><span class="sxs-lookup"><span data-stu-id="ff85d-197">lt = Less Than</span></span> |
| <span data-ttu-id="ff85d-198">Hodnota</span><span class="sxs-lookup"><span data-stu-id="ff85d-198">Value</span></span> |<span data-ttu-id="ff85d-199">Hodnota pro mezní hodnotu.</span><span class="sxs-lookup"><span data-stu-id="ff85d-199">Value for the threshold.</span></span> |

<span data-ttu-id="ff85d-200">Představte si třeba dotazu událostí se v intervalu 15 minut, časový interval 30 minut a prahové hodnoty větší než 10.</span><span class="sxs-lookup"><span data-stu-id="ff85d-200">For example, consider an event query with an Interval of 15 minutes, a Timespan of 30 minutes, and a Threshold of greater than 10.</span></span> <span data-ttu-id="ff85d-201">V takovém případě by se spustí dotaz každých 15 minut a výstraha by spustí, pokud je vrácen 10 události, které byly vytvořeny během určitého 30 minut.</span><span class="sxs-lookup"><span data-stu-id="ff85d-201">In this case, the query would be run every 15 minutes, and an alert would be triggered if it returned 10 events that were created over a 30 minute span.</span></span>

<span data-ttu-id="ff85d-202">Následuje ukázková odpověď pro akce s pouze prahovou hodnotu.</span><span class="sxs-lookup"><span data-stu-id="ff85d-202">Following is a sample response for an action with only a threshold.</span></span>  

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

<span data-ttu-id="ff85d-203">Chcete-li vytvořit novou akci prahovou hodnotu pro harmonogram pomocí akce jedinečné ID metodu Put.</span><span class="sxs-lookup"><span data-stu-id="ff85d-203">Use the Put method with a unique action ID to create a new threshold action for a schedule.</span></span>  

    $thresholdJson = "{'properties': { 'Name': 'My Threshold', 'Version':'1', 'Type':'Alert', 'Threshold': { 'Operator': 'gt', 'Value': 10 } }"
    armclient put /subscriptions/{Subscription ID}/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/{Search ID}/schedules/{Schedule ID}/actions/mythreshold?api-version=2015-03-20 $thresholdJson

<span data-ttu-id="ff85d-204">K úpravě akce prahové hodnoty pro plán, použijte metodu Put s existující ID akce.</span><span class="sxs-lookup"><span data-stu-id="ff85d-204">Use the Put method with an existing action ID to modify a threshold action for a schedule.</span></span>  <span data-ttu-id="ff85d-205">Text žádosti musí obsahovat značku etag akce.</span><span class="sxs-lookup"><span data-stu-id="ff85d-205">The body of the request must include the etag of the action.</span></span>

    $thresholdJson = "{'etag': 'W/\"datetime'2016-02-25T20%3A54%3A20.1302566Z'\"','properties': { 'Name': 'My Threshold', 'Version':'1', 'Type':'Alert', 'Threshold': { 'Operator': 'gt', 'Value': 10 } }"
    armclient put /subscriptions/{Subscription ID}/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/{Search ID}/schedules/{Schedule ID}/actions/mythreshold?api-version=2015-03-20 $thresholdJson

#### <a name="email-notification"></a><span data-ttu-id="ff85d-206">E-mailových oznámení</span><span class="sxs-lookup"><span data-stu-id="ff85d-206">Email Notification</span></span>
<span data-ttu-id="ff85d-207">E-mailová oznámení odesílat e-mailu na jeden nebo více příjemců.</span><span class="sxs-lookup"><span data-stu-id="ff85d-207">Email Notifications send mail to one or more recipients.</span></span>  <span data-ttu-id="ff85d-208">Patří mezi ně vlastnosti v následující tabulce.</span><span class="sxs-lookup"><span data-stu-id="ff85d-208">They include the properties in the following table.</span></span>

| <span data-ttu-id="ff85d-209">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="ff85d-209">Property</span></span> | <span data-ttu-id="ff85d-210">Popis</span><span class="sxs-lookup"><span data-stu-id="ff85d-210">Description</span></span> |
|:--- |:--- |
| <span data-ttu-id="ff85d-211">Příjemce</span><span class="sxs-lookup"><span data-stu-id="ff85d-211">Recipients</span></span> |<span data-ttu-id="ff85d-212">Seznam adres e-mailu.</span><span class="sxs-lookup"><span data-stu-id="ff85d-212">List of mail addresses.</span></span> |
| <span data-ttu-id="ff85d-213">Předmět</span><span class="sxs-lookup"><span data-stu-id="ff85d-213">Subject</span></span> |<span data-ttu-id="ff85d-214">Předmět e-mailu.</span><span class="sxs-lookup"><span data-stu-id="ff85d-214">The subject of the mail.</span></span> |
| <span data-ttu-id="ff85d-215">Přílohy</span><span class="sxs-lookup"><span data-stu-id="ff85d-215">Attachment</span></span> |<span data-ttu-id="ff85d-216">Přílohy nejsou aktuálně podporovány, takže to bude mít vždy hodnotu "Žádný".</span><span class="sxs-lookup"><span data-stu-id="ff85d-216">Attachments are not currently supported, so this will always have a value of “None”.</span></span> |

<span data-ttu-id="ff85d-217">Následuje ukázková odpověď pro akci oznámení e-mailu s prahovou hodnotou.</span><span class="sxs-lookup"><span data-stu-id="ff85d-217">Following is a sample response for an email notification action with a threshold.</span></span>  

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
            "Subject": "This is the subject",
            "Attachment": "None"
        },
        "Version": 1
    }

<span data-ttu-id="ff85d-218">Chcete-li vytvořit novou e-mailu akci pro plán pomocí akce jedinečné ID metodu Put.</span><span class="sxs-lookup"><span data-stu-id="ff85d-218">Use the Put method with a unique action ID to create a new e-mail action for a schedule.</span></span>  <span data-ttu-id="ff85d-219">Následující příklad vytvoří e-mailové oznámení s prahovou hodnotou, takže odešle e-mailu v případě, že výsledky uloženého hledání překročit prahovou hodnotu.</span><span class="sxs-lookup"><span data-stu-id="ff85d-219">The following example creates an email notification with a threshold so the mail is sent when the results of the saved search exceed the threshold.</span></span>

    $emailJson = "{'properties': { 'Name': 'MyEmailAction', 'Version':'1', 'Type':'Alert', 'Threshold': { 'Operator': 'gt', 'Value': 10 }, 'EmailNotification': {'Recipients': ['recipient1@contoso.com', 'recipient2@contoso.com'], 'Subject':'This is the subject', 'Attachment':'None'} }"
    armclient put /subscriptions/{Subscription ID}/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/{Search ID}/schedules/{Schedule ID}/actions/myemailaction?api-version=2015-03-20 $emailJson

<span data-ttu-id="ff85d-220">K úpravě akce e-mailu pro plán, použijte metodu Put s existující ID akce.</span><span class="sxs-lookup"><span data-stu-id="ff85d-220">Use the Put method with an existing action ID to modify an e-mail action for a schedule.</span></span>  <span data-ttu-id="ff85d-221">Text žádosti musí obsahovat značku etag akce.</span><span class="sxs-lookup"><span data-stu-id="ff85d-221">The body of the request must include the etag of the action.</span></span>

    $emailJson = "{'etag': 'W/\"datetime'2016-02-25T20%3A54%3A20.1302566Z'\"','properties': { 'Name': 'MyEmailAction', 'Version':'1', 'Type':'Alert', 'Threshold': { 'Operator': 'gt', 'Value': 10 }, 'EmailNotification': {'Recipients': ['recipient1@contoso.com', 'recipient2@contoso.com'], 'Subject':'This is the subject', 'Attachment':'None'} }"
    armclient put /subscriptions/{Subscription ID}/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/{Search ID}/schedules/{Schedule ID}/actions/myemailaction?api-version=2015-03-20 $emailJson

#### <a name="remediation-actions"></a><span data-ttu-id="ff85d-222">Akce nápravy</span><span class="sxs-lookup"><span data-stu-id="ff85d-222">Remediation actions</span></span>
<span data-ttu-id="ff85d-223">Nápravy spuštění sady runbook ve službě Azure Automation, který se pokouší odstranit problém identifikovaný výstrahy.</span><span class="sxs-lookup"><span data-stu-id="ff85d-223">Remediations start a runbook in Azure Automation that attempts to correct the problem identified by the alert.</span></span>  <span data-ttu-id="ff85d-224">Musíte vytvořit webhooku pro sadu runbook použít v akci automatické nápravy a pak zadejte identifikátor URI ve vlastnosti WebhookUri.</span><span class="sxs-lookup"><span data-stu-id="ff85d-224">You must create a webhook for the runbook used in a remediation action and then specify the URI in the WebhookUri property.</span></span>  <span data-ttu-id="ff85d-225">Když vytvoříte tuto akci pomocí konzole OMS, se automaticky vytvoří nové webhooku pro sadu runbook.</span><span class="sxs-lookup"><span data-stu-id="ff85d-225">When you create this action using the OMS console, a new webhook is automatically created for the runbook.</span></span>

<span data-ttu-id="ff85d-226">Nápravami, které zahrnují vlastnosti v následující tabulce.</span><span class="sxs-lookup"><span data-stu-id="ff85d-226">Remediations include the properties in the following table.</span></span>

| <span data-ttu-id="ff85d-227">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="ff85d-227">Property</span></span> | <span data-ttu-id="ff85d-228">Popis</span><span class="sxs-lookup"><span data-stu-id="ff85d-228">Description</span></span> |
|:--- |:--- |
| <span data-ttu-id="ff85d-229">RunbookName</span><span class="sxs-lookup"><span data-stu-id="ff85d-229">RunbookName</span></span> |<span data-ttu-id="ff85d-230">Název sady runbook.</span><span class="sxs-lookup"><span data-stu-id="ff85d-230">Name of the runbook.</span></span> <span data-ttu-id="ff85d-231">To se musí shodovat publikované sady runbook v účtu automation konfigurované v řešení služby Automation v pracovním prostoru OMS.</span><span class="sxs-lookup"><span data-stu-id="ff85d-231">This must match a published runbook in the automation account configured in the Automation Solution in your OMS workspace.</span></span> |
| <span data-ttu-id="ff85d-232">WebhookUri</span><span class="sxs-lookup"><span data-stu-id="ff85d-232">WebhookUri</span></span> |<span data-ttu-id="ff85d-233">Identifikátor URI služby webhooku.</span><span class="sxs-lookup"><span data-stu-id="ff85d-233">URI of the webhook.</span></span> |
| <span data-ttu-id="ff85d-234">Vypršení platnosti</span><span class="sxs-lookup"><span data-stu-id="ff85d-234">Expiry</span></span> |<span data-ttu-id="ff85d-235">Datum vypršení platnosti a čas webhooku.</span><span class="sxs-lookup"><span data-stu-id="ff85d-235">The expiration date and time of the webhook.</span></span>  <span data-ttu-id="ff85d-236">Pokud webhooku nemá vypršení platnosti, může to být žádné platné budoucí datum.</span><span class="sxs-lookup"><span data-stu-id="ff85d-236">If the webhook doesn’t have an expiration, then this can be any valid future date.</span></span> |

<span data-ttu-id="ff85d-237">Následuje ukázková odpověď pro akci automatické nápravy s prahovou hodnotou.</span><span class="sxs-lookup"><span data-stu-id="ff85d-237">Following is a sample response for a remediation action with a threshold.</span></span>

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

<span data-ttu-id="ff85d-238">Chcete-li vytvořit novou akci nápravy pro plán pomocí akce jedinečné ID metodu Put.</span><span class="sxs-lookup"><span data-stu-id="ff85d-238">Use the Put method with a unique action ID to create a new remediation action for a schedule.</span></span>  <span data-ttu-id="ff85d-239">Následující příklad vytvoří nápravy s prahovou hodnotou, takže spuštění runbooku při výsledky uloženého hledání překročit prahovou hodnotu.</span><span class="sxs-lookup"><span data-stu-id="ff85d-239">The following example creates a remediation with a threshold so the runbook is started when the results of the saved search exceed the threshold.</span></span>

    $remediateJson = "{'properties': { 'Type':'Alert', 'Name': 'My Remediation Action', 'Version':'1', 'Threshold': { 'Operator': 'gt', 'Value': 10 }, 'Remediation': {'RunbookName': 'My-Runbook', 'WebhookUri':'https://s1events.azure-automation.net/webhooks?token=4jCibOjO3w4W2Cfg%2b2NkjLYdafnusaG6i8tnP8h%2fNNg%3d', 'Expiry':'2018-02-25T18:27:20Z'} }"
    armclient put /subscriptions/{Subscription ID}/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/{Search ID}/schedules/{Schedule ID}/actions/myremediationaction?api-version=2015-03-20 $remediateJson

<span data-ttu-id="ff85d-240">K úpravě akci automatické nápravy pro plán, použijte metodu Put s existující ID akce.</span><span class="sxs-lookup"><span data-stu-id="ff85d-240">Use the Put method with an existing action ID to modify a remediation action for a schedule.</span></span>  <span data-ttu-id="ff85d-241">Text žádosti musí obsahovat značku etag akce.</span><span class="sxs-lookup"><span data-stu-id="ff85d-241">The body of the request must include the etag of the action.</span></span>

    $remediateJson = "{'etag': 'W/\"datetime'2016-02-25T20%3A54%3A20.1302566Z'\"','properties': { 'Type':'Alert', 'Name': 'My Remediation Action', 'Version':'1', 'Threshold': { 'Operator': 'gt', 'Value': 10 }, 'Remediation': {'RunbookName': 'My-Runbook', 'WebhookUri':'https://s1events.azure-automation.net/webhooks?token=4jCibOjO3w4W2Cfg%2b2NkjLYdafnusaG6i8tnP8h%2fNNg%3d', 'Expiry':'2018-02-25T18:27:20Z'} }"
    armclient put /subscriptions/{Subscription ID}/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/{Search ID}/schedules/{Schedule ID}/actions/myremediationaction?api-version=2015-03-20 $remediateJson

#### <a name="example"></a><span data-ttu-id="ff85d-242">Příklad</span><span class="sxs-lookup"><span data-stu-id="ff85d-242">Example</span></span>
<span data-ttu-id="ff85d-243">Toto je kompletní příklad k vytvoření nové e-mailové výstrahy.</span><span class="sxs-lookup"><span data-stu-id="ff85d-243">Following is a complete example to create a new email alert.</span></span>  <span data-ttu-id="ff85d-244">Tím se vytvoří nový plán spolu s akce obsahující prahovou hodnotu a e-mailu.</span><span class="sxs-lookup"><span data-stu-id="ff85d-244">This creates a new schedule along with an action containing a threshold and email.</span></span>

    $subscriptionId = "3d56705e-5b26-5bcc-9368-dbc8d2fafbfc"
    $resourceGroup  = "MyResourceGroup"    
    $workspaceName    = "MyWorkspace"
    $searchId       = "MySearch"
    $scheduleId     = "MySchedule"
    $thresholdId    = "MyThreshold"
    $actionId       = "MyEmailAction"

    $scheduleJson = "{'properties': { 'Interval': 15, 'QueryTimeSpan':15, 'Active':'true' }"
    armclient put /subscriptions/$subscriptionId/resourceGroups/$resourceGroup/providers/Microsoft.OperationalInsights/workspaces/$workspaceName/savedSearches/$searchId/schedules/$scheduleId/?api-version=2015-03-20 $scheduleJson

    $emailJson = "{'properties': { 'Name': 'MyEmailAction', 'Version':'1', 'Severity':'Warning', 'Type':'Alert', 'Threshold': { 'Operator': 'gt', 'Value': 10 }, 'EmailNotification': {'Recipients': ['recipient1@contoso.com', 'recipient2@contoso.com'], 'Subject':'This is the subject', 'Attachment':'None'} }"
    armclient put /subscriptions/$subscriptionId/resourceGroups/$resourceGroup/providers/Microsoft.OperationalInsights/workspaces/$workspaceName/savedSearches/$searchId/schedules/$scheduleId/actions/$actionId/?api-version=2015-03-20 $emailJson

### <a name="webhook-actions"></a><span data-ttu-id="ff85d-245">Akce Webhooku</span><span class="sxs-lookup"><span data-stu-id="ff85d-245">Webhook actions</span></span>
<span data-ttu-id="ff85d-246">Akce Webhooku spuštění procesu voláním adresu URL a volitelně poskytuje datové části k odeslání.</span><span class="sxs-lookup"><span data-stu-id="ff85d-246">Webhook actions start a process by calling a URL and optionally providing a payload to be sent.</span></span>  <span data-ttu-id="ff85d-247">Jsou podobná nápravné akce s výjimkou jsou určené pro webhooků, který může vyvolat procesy než Azure Automation runbook.</span><span class="sxs-lookup"><span data-stu-id="ff85d-247">They are similar to Remediation actions except they are meant for webhooks that may invoke processes other than Azure Automation runbooks.</span></span>  <span data-ttu-id="ff85d-248">Obsahují taky další možnost poskytnout datové části který bude doručen do vzdálený proces.</span><span class="sxs-lookup"><span data-stu-id="ff85d-248">They also provide the additional option of providing a payload to be delivered to the remote process.</span></span>

<span data-ttu-id="ff85d-249">Akce Webhooku nemáte prahovou hodnotu, ale místo toho musí být přidaní do plánu, který má výstrahy akce s prahovou hodnotou.</span><span class="sxs-lookup"><span data-stu-id="ff85d-249">Webhook actions do not have a threshold but instead should be added to a schedule that has an Alert action with a threshold.</span></span>  <span data-ttu-id="ff85d-250">Můžete přidat více Webhooku akcí, které se všechny spustí při splnění prahovou hodnotu.</span><span class="sxs-lookup"><span data-stu-id="ff85d-250">You can add multiple Webhook actions that will all be run when the threshold is met.</span></span>

<span data-ttu-id="ff85d-251">Akce Webhooku zahrnují vlastnosti v následující tabulce.</span><span class="sxs-lookup"><span data-stu-id="ff85d-251">Webhook actions include the properties in the following table.</span></span>

| <span data-ttu-id="ff85d-252">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="ff85d-252">Property</span></span> | <span data-ttu-id="ff85d-253">Popis</span><span class="sxs-lookup"><span data-stu-id="ff85d-253">Description</span></span> |
|:--- |:--- |
| <span data-ttu-id="ff85d-254">WebhookUri</span><span class="sxs-lookup"><span data-stu-id="ff85d-254">WebhookUri</span></span> |<span data-ttu-id="ff85d-255">Předmět e-mailu.</span><span class="sxs-lookup"><span data-stu-id="ff85d-255">The subject of the mail.</span></span> |
| <span data-ttu-id="ff85d-256">CustomPayload</span><span class="sxs-lookup"><span data-stu-id="ff85d-256">CustomPayload</span></span> |<span data-ttu-id="ff85d-257">Vlastní datovou část k odeslání do webhooku.</span><span class="sxs-lookup"><span data-stu-id="ff85d-257">Custom payload to be sent to the webhook.</span></span>  <span data-ttu-id="ff85d-258">Formát bude záviset na co webhooku očekává.</span><span class="sxs-lookup"><span data-stu-id="ff85d-258">The format will depend on what the webhook is expecting.</span></span> |

<span data-ttu-id="ff85d-259">Následuje ukázková odpověď pro akce webhooku a přidružené akce výstrah s prahovou hodnotou.</span><span class="sxs-lookup"><span data-stu-id="ff85d-259">Following is a sample response for webhook action and an associated alert action with a threshold.</span></span>

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

#### <a name="create-or-edit-a-webhook-action"></a><span data-ttu-id="ff85d-260">Vytvořte nebo upravte akce webhooku</span><span class="sxs-lookup"><span data-stu-id="ff85d-260">Create or edit a webhook action</span></span>
<span data-ttu-id="ff85d-261">K vytvoření nové akce webhooku pro plán použijte metodu Put s ID jedinečný akce.</span><span class="sxs-lookup"><span data-stu-id="ff85d-261">Use the Put method with a unique action ID to create a new webhook action for a schedule.</span></span>  <span data-ttu-id="ff85d-262">Následující příklad vytvoří akce Webhooku a výstrah akce s prahovou hodnotu, aby webhooku se aktivuje, když výsledky uloženého hledání překročit prahovou hodnotu.</span><span class="sxs-lookup"><span data-stu-id="ff85d-262">The following example creates a Webhook action and an Alert action with a threshold so that the webhook will be triggered when the results of the saved search exceed the threshold.</span></span>

    $thresholdAction = "{'properties': { 'Name': 'My Threshold', 'Version':'1', 'Type':'Alert', 'Threshold': { 'Operator': 'gt', 'Value': 10 } }"
    armclient put /subscriptions/{Subscription ID}/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/{Search ID}/schedules/{Schedule ID}/actions/mythreshold?api-version=2015-03-20 $thresholdAction

    $webhookAction = "{'properties': {'Type': 'Webhook', 'Name': 'My Webhook", 'WebhookUri': 'https://oaaswebhookdf.cloudapp.net/webhooks?token=VrkYTKlhk%2fc%2bKBP', 'CustomPayload': '{\"field1\":\"value1\",\"field2\":\"value2\"}', 'Version': 1 }"
    armclient put /subscriptions/{Subscription ID}/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/{Search ID}/schedules/{Schedule ID}/actions/mywebhookaction?api-version=2015-03-20 $webhookAction

<span data-ttu-id="ff85d-263">K úpravě akce webhooku pro plán, použijte metodu Put s existující ID akce.</span><span class="sxs-lookup"><span data-stu-id="ff85d-263">Use the Put method with an existing action ID to modify a webhook action for a schedule.</span></span>  <span data-ttu-id="ff85d-264">Text žádosti musí obsahovat značku etag akce.</span><span class="sxs-lookup"><span data-stu-id="ff85d-264">The body of the request must include the etag of the action.</span></span>

    $webhookAction = "{'etag': 'W/\"datetime'2016-02-26T20%3A25%3A00.6862124Z'\"','properties': {'Type': 'Webhook', 'Name': 'My Webhook", 'WebhookUri': 'https://oaaswebhookdf.cloudapp.net/webhooks?token=VrkYTKlhk%2fc%2bKBP', 'CustomPayload': '{\"field1\":\"value1\",\"field2\":\"value2\"}', 'Version': 1 }"
    armclient put /subscriptions/{Subscription ID}/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/{Search ID}/schedules/{Schedule ID}/actions/mywebhookaction?api-version=2015-03-20 $webhookAction

## <a name="next-steps"></a><span data-ttu-id="ff85d-265">Další kroky</span><span class="sxs-lookup"><span data-stu-id="ff85d-265">Next steps</span></span>
* <span data-ttu-id="ff85d-266">Použití [rozhraní API REST k vyhledávání protokolu](log-analytics-log-search-api.md) v analýzy protokolů.</span><span class="sxs-lookup"><span data-stu-id="ff85d-266">Use the [REST API to perform log searches](log-analytics-log-search-api.md) in Log Analytics.</span></span>

