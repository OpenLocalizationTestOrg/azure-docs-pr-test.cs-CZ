---
title: "Analýzy protokolů protokolu vyhledávání REST API | Microsoft Docs"
description: "Tato příručka obsahuje základní kurz popisující, jak můžete použít vyhledávání analýzy protokolů REST API v Operations Management Suite (OMS) a poskytuje příklady, které ukazují, jak používat příkazy."
services: log-analytics
documentationcenter: 
author: bwren
manager: carmonm
editor: 
ms.assetid: b4e9ebe8-80f0-418e-a855-de7954668df7
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/23/2017
ms.author: bwren
ms.openlocfilehash: 78afb2f065dde4a3e7a3ab787c939b3c52b72cc6
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="log-analytics-log-search-rest-api"></a><span data-ttu-id="210a0-103">Hledání protokolu analýzy protokolů REST API</span><span class="sxs-lookup"><span data-stu-id="210a0-103">Log Analytics log search REST API</span></span>
<span data-ttu-id="210a0-104">Tato příručka obsahuje základní kurz, včetně příkladů, jak můžete pomocí rozhraní REST API Log Analytics Search.</span><span class="sxs-lookup"><span data-stu-id="210a0-104">This guide provides a basic tutorial, including examples, of how you can use the Log Analytics Search REST API.</span></span> <span data-ttu-id="210a0-105">Analýzy protokolů je součástí Operations Management Suite (OMS).</span><span class="sxs-lookup"><span data-stu-id="210a0-105">Log Analytics is part of the Operations Management Suite (OMS).</span></span>

> [!NOTE]
> <span data-ttu-id="210a0-106">Pokud pracovní prostor byl upgradován na verzi [nové analýzy protokolů dotazu jazyka](log-analytics-log-search-upgrade.md), pak by měly být nadále používat starší verze dotazovací jazyk s hledání protokolů rozhraní API, jak je popsáno v tomto článku.</span><span class="sxs-lookup"><span data-stu-id="210a0-106">If your workspace has been upgraded to the [new Log Analytics query language](log-analytics-log-search-upgrade.md), then you should continue to use the legacy query language with the log search API as described in this article.</span></span>  <span data-ttu-id="210a0-107">Nové rozhraní API, budou vydané pro upgradovaný pracovní prostory, a v dokumentaci budou aktualizovány v daném čase.</span><span class="sxs-lookup"><span data-stu-id="210a0-107">A new API will be released for upgraded workspaces, and the documentation will be updated at that time.</span></span> 

> [!NOTE]
> <span data-ttu-id="210a0-108">Analýzy protokolů volala dřív Operational Insights, proto je název používaný v poskytovatele prostředků.</span><span class="sxs-lookup"><span data-stu-id="210a0-108">Log Analytics was previously called Operational Insights, which is why it is the name used in the resource provider.</span></span>
>
>

## <a name="overview-of-the-log-search-rest-api"></a><span data-ttu-id="210a0-109">Přehled protokolu hledání rozhraní REST API</span><span class="sxs-lookup"><span data-stu-id="210a0-109">Overview of the Log Search REST API</span></span>
<span data-ttu-id="210a0-110">Rozhraní REST API Log Analytics Search je dosáhl standardu RESTful a je přístupný prostřednictvím rozhraní API služby Azure Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="210a0-110">The Log Analytics Search REST API is RESTful and can be accessed via the Azure Resource Manager API.</span></span> <span data-ttu-id="210a0-111">Tento článek obsahuje příklady přístup k rozhraní API prostřednictvím [ARMClient](https://github.com/projectkudu/ARMClient), nástroje příkazového řádku s otevřeným zdrojem, který zjednodušuje volání rozhraní API služby Azure Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="210a0-111">This article provides examples of accessing the API through [ARMClient](https://github.com/projectkudu/ARMClient), an open source command-line tool that simplifies invoking the Azure Resource Manager API.</span></span> <span data-ttu-id="210a0-112">Použití ARMClient je jedním z mnoha možností pro přístup k rozhraní API pro vyhledávání Analytics protokolu.</span><span class="sxs-lookup"><span data-stu-id="210a0-112">The use of ARMClient is one of many options to access the Log Analytics Search API.</span></span> <span data-ttu-id="210a0-113">Další možností je použít modul Azure PowerShell pro OperationalInsights, který obsahuje rutiny pro přístup k vyhledávání.</span><span class="sxs-lookup"><span data-stu-id="210a0-113">Another option is to use the Azure PowerShell module for OperationalInsights, which includes cmdlets for accessing search.</span></span> <span data-ttu-id="210a0-114">Pomocí těchto nástrojů můžete využít rozhraní API služby Azure Resource Manager provádět volání do OMS pracovních prostorů a provádět příkazy vyhledávání v nich.</span><span class="sxs-lookup"><span data-stu-id="210a0-114">With these tools, you can utilize the Azure Resource Manager API to make calls to OMS workspaces and perform search commands within them.</span></span> <span data-ttu-id="210a0-115">Rozhraní API výstupy výsledků vyhledávání ve formátu JSON, budete moci použít výsledky hledání v mnoha různými způsoby prostřednictvím kódu programu.</span><span class="sxs-lookup"><span data-stu-id="210a0-115">The API outputs search results in JSON format, allowing you to use the search results in many different ways programmatically.</span></span>

<span data-ttu-id="210a0-116">Azure Resource Manager je možné prostřednictvím [knihovna pro .NET](https://msdn.microsoft.com/library/azure/dn910477.aspx) a [REST API](https://msdn.microsoft.com/library/azure/mt163658.aspx).</span><span class="sxs-lookup"><span data-stu-id="210a0-116">The Azure Resource Manager can be used via a [Library for .NET](https://msdn.microsoft.com/library/azure/dn910477.aspx) and the [REST API](https://msdn.microsoft.com/library/azure/mt163658.aspx).</span></span> <span data-ttu-id="210a0-117">Další informace, zkontrolujte propojené webové stránky.</span><span class="sxs-lookup"><span data-stu-id="210a0-117">To learn more, review the linked web pages.</span></span>

> [!NOTE]
> <span data-ttu-id="210a0-118">Pokud například použijete příkaz agregace `|measure count()` nebo `distinct`, každé volání hledání může vrátit maximálně 500 000 záznamů.</span><span class="sxs-lookup"><span data-stu-id="210a0-118">If you use an aggregation command such as `|measure count()` or `distinct`, each call to search can return upto 500,000 records.</span></span> <span data-ttu-id="210a0-119">Hledání, které neobsahují příkaz agregace vrátit maximálně 5 000 záznamů.</span><span class="sxs-lookup"><span data-stu-id="210a0-119">Searches that do not include an aggregation command return upto 5,000 records.</span></span>
>
>

## <a name="basic-log-analytics-search-rest-api-tutorial"></a><span data-ttu-id="210a0-120">Základní kurz Log Analytics vyhledávání REST API</span><span class="sxs-lookup"><span data-stu-id="210a0-120">Basic Log Analytics Search REST API tutorial</span></span>
### <a name="to-use-armclient"></a><span data-ttu-id="210a0-121">Chcete-li použít ARMClient</span><span class="sxs-lookup"><span data-stu-id="210a0-121">To use ARMClient</span></span>
1. <span data-ttu-id="210a0-122">Nainstalujte [Chocolatey](https://chocolatey.org/), což je otevřete Správce balíčků zdroje pro systém Windows.</span><span class="sxs-lookup"><span data-stu-id="210a0-122">Install [Chocolatey](https://chocolatey.org/), which is an Open Source Package Manager for Windows.</span></span> <span data-ttu-id="210a0-123">Otevřete okno příkazového řádku jako správce a spusťte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="210a0-123">Open a command prompt window as administrator and then run the following command:</span></span>

    ```
    @powershell -NoProfile -ExecutionPolicy unrestricted -Command "iex ((new-object net.webclient).DownloadString('https://chocolatey.org/install.ps1'))" && SET PATH=%PATH%;%ALLUSERSPROFILE%\chocolatey\bin
    ```
2. <span data-ttu-id="210a0-124">Instalace ARMClient spuštěním následujícího příkazu:</span><span class="sxs-lookup"><span data-stu-id="210a0-124">Install ARMClient by running the following command:</span></span>

    ```
    choco install armclient
    ```

### <a name="to-perform-a-search-using-armclient"></a><span data-ttu-id="210a0-125">K provedení vyhledávání pomocí ARMClient</span><span class="sxs-lookup"><span data-stu-id="210a0-125">To perform a search using ARMClient</span></span>
1. <span data-ttu-id="210a0-126">Přihlaste se pomocí účtu Microsoft nebo pracovní nebo školní účet:</span><span class="sxs-lookup"><span data-stu-id="210a0-126">Log in using your Microsoft account or your work or school account:</span></span>

    ```
    armclient login
    ```

    <span data-ttu-id="210a0-127">Úspěšné přihlášení zobrazí všechny odběry vázaný na daný účet:</span><span class="sxs-lookup"><span data-stu-id="210a0-127">A successful login lists all subscriptions tied to the given account:</span></span>

    ```
    PS C:\Users\SampleUserName> armclient login
    Welcome YourEmail@ORG.com (Tenant: zzzzzzzz-zzzz-zzzz-zzzz-zzzzzzzzzzzz)
    User: YourEmail@ORG.com, Tenant: zzzzzzzz-zzzz-zzzz-zzzz-zzzzzzzzzzzz (Example org)
    There are 3 subscriptions
    Subscription xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx (Example Name 1)
    Subscription xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx (Example Name 2)
    Subscription xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx (Example Name 3)
    ```
2. <span data-ttu-id="210a0-128">Získáte pracovní prostory služby Operations Management Suite:</span><span class="sxs-lookup"><span data-stu-id="210a0-128">Get the Operations Management Suite workspaces:</span></span>

    ```
    armclient get /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces?api-version=2015-03-20
    ```

    <span data-ttu-id="210a0-129">Úspěšné volání Get by výstup všechny pracovní prostory, které jsou vázané na předplatné:</span><span class="sxs-lookup"><span data-stu-id="210a0-129">A successful Get call would output all workspaces tied to the subscription:</span></span>

    ```
    {
    "value": [
    {
      "properties": {
    "source": "External",
    "customerId": "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
    "portalUrl": "https://eus.login.mms.microsoft.com/SignIn.aspx?returnUrl=https%3a%2f%2feus.mms.microsoft.com%2fMain.aspx%3fcid%xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
      },
      "id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourcegroups/oi-default-east-us/providers/microsoft.operationalinsights/workspaces/{WORKSPACE NAME}",
      "name": "{WORKSPACE NAME}",
      "type": "Microsoft.OperationalInsights/workspaces",
      "location": "East US"
       ]
    }
    ```
3. <span data-ttu-id="210a0-130">Vytvořte vaše proměnná vyhledávání:</span><span class="sxs-lookup"><span data-stu-id="210a0-130">Create your search variable:</span></span>

    ```
    $mySearch = "{ 'top':150, 'query':'Error'}";
    ```
4. <span data-ttu-id="210a0-131">Vyhledávání pomocí vaší novou proměnnou vyhledávání:</span><span class="sxs-lookup"><span data-stu-id="210a0-131">Search using your new search variable:</span></span>

    ```
    armclient post /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{WORKSPACE NAME}/search?api-version=2015-03-20 $mySearch
    ```

## <a name="log-analytics-search-rest-api-reference-examples"></a><span data-ttu-id="210a0-132">Příklady referenční dokumentace rozhraní API REST Analytics vyhledávání protokolu</span><span class="sxs-lookup"><span data-stu-id="210a0-132">Log Analytics Search REST API reference examples</span></span>
<span data-ttu-id="210a0-133">Následující příklady ukazují, jak můžete použít rozhraní API služby Search.</span><span class="sxs-lookup"><span data-stu-id="210a0-133">The following examples show you how you can use the Search API.</span></span>

### <a name="search---actionread"></a><span data-ttu-id="210a0-134">Hledání - akce/čtení</span><span class="sxs-lookup"><span data-stu-id="210a0-134">Search - Action/Read</span></span>
<span data-ttu-id="210a0-135">**Ukázka adresa Url:**</span><span class="sxs-lookup"><span data-stu-id="210a0-135">**Sample Url:**</span></span>

```
    /subscriptions/{SubscriptionId}/resourceGroups/{ResourceGroupId}/providers/Microsoft.OperationalInsights/workspaces/{WorkspaceName}/search?api-version=2015-03-20
```

<span data-ttu-id="210a0-136">**Žádost:**</span><span class="sxs-lookup"><span data-stu-id="210a0-136">**Request:**</span></span>

```
    $savedSearchParametersJson =
    {
      "top":150,
      "highlight":{
        "pre":"{[hl]}",
        "post":"{[/hl]}"
      },
      "query":"*",
      "start":"2015-02-04T21:03:29.231Z",
      "end":"2015-02-11T21:03:29.231Z"
    }
    armclient post /subscriptions/{Subscription ID}/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{Workspace ID}/search?api-version=2015-03-20 $searchParametersJson
```
<span data-ttu-id="210a0-137">Následující tabulka popisuje vlastnosti, které jsou k dispozici.</span><span class="sxs-lookup"><span data-stu-id="210a0-137">The following table describes the properties that are available.</span></span>

| <span data-ttu-id="210a0-138">**Vlastnost**</span><span class="sxs-lookup"><span data-stu-id="210a0-138">**Property**</span></span> | <span data-ttu-id="210a0-139">**Popis**</span><span class="sxs-lookup"><span data-stu-id="210a0-139">**Description**</span></span> |
| --- | --- |
| <span data-ttu-id="210a0-140">Horní</span><span class="sxs-lookup"><span data-stu-id="210a0-140">top</span></span> |<span data-ttu-id="210a0-141">Maximální počet výsledků vrátit.</span><span class="sxs-lookup"><span data-stu-id="210a0-141">The maximum number of results to return.</span></span> |
| <span data-ttu-id="210a0-142">Zvýrazněte</span><span class="sxs-lookup"><span data-stu-id="210a0-142">highlight</span></span> |<span data-ttu-id="210a0-143">Obsahuje předběžné a post parametry, obvykle použité pro zvýraznění odpovídající pole</span><span class="sxs-lookup"><span data-stu-id="210a0-143">Contains pre and post parameters, used usually for highlighting matching fields</span></span> |
| <span data-ttu-id="210a0-144">Před</span><span class="sxs-lookup"><span data-stu-id="210a0-144">pre</span></span> |<span data-ttu-id="210a0-145">Předpony daný řetězec, který má vaše odpovídající pole.</span><span class="sxs-lookup"><span data-stu-id="210a0-145">Prefixes the given string to your matched fields.</span></span> |
| <span data-ttu-id="210a0-146">POST</span><span class="sxs-lookup"><span data-stu-id="210a0-146">post</span></span> |<span data-ttu-id="210a0-147">Daný řetězec připojí k vaší odpovídající pole.</span><span class="sxs-lookup"><span data-stu-id="210a0-147">Appends the given string to your matched fields.</span></span> |
| <span data-ttu-id="210a0-148">query</span><span class="sxs-lookup"><span data-stu-id="210a0-148">query</span></span> |<span data-ttu-id="210a0-149">Vyhledávací dotaz, použít ke shromažďování a vrátí výsledky.</span><span class="sxs-lookup"><span data-stu-id="210a0-149">The search query used to collect and return results.</span></span> |
| <span data-ttu-id="210a0-150">start</span><span class="sxs-lookup"><span data-stu-id="210a0-150">start</span></span> |<span data-ttu-id="210a0-151">Začátek časový interval, který chcete výsledky, která se má najít z.</span><span class="sxs-lookup"><span data-stu-id="210a0-151">The beginning of the time window you want results to be found from.</span></span> |
| <span data-ttu-id="210a0-152">End</span><span class="sxs-lookup"><span data-stu-id="210a0-152">end</span></span> |<span data-ttu-id="210a0-153">Konec časový interval, který chcete výsledky, která se má najít z.</span><span class="sxs-lookup"><span data-stu-id="210a0-153">The end of the time window you want results to be found from.</span></span> |

<span data-ttu-id="210a0-154">**Odpověď:**</span><span class="sxs-lookup"><span data-stu-id="210a0-154">**Response:**</span></span>

```
    {
      "id" : "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
      "__metadata" : {
        "resultType" : "raw",
        "total" : 1455,
        "top" : 150,
        "StartTime" : "2015-02-11T21:09:07.0345815Z",
        "Status" : "Successful",
        "LastUpdated" : "2015-02-11T21:09:07.331463Z",
        "CoreResponses" : [],
        "sort" : [{
          "name" : "TimeGenerated",
          "order" : "desc"
        }],
        "requestTime" : 450
      },
      "value": [{
        "SourceSystem" : "OpsManager",
        "TimeGenerated" : "2015-02-07T14:07:33Z",
        "Source" : "SideBySide",
        "EventLog" : "Application",
        "Computer" : "BAMBAM",
        "EventCategory" : 0,
        "EventLevel" : 1,
        "EventLevelName" : "Error",
        "UserName" : "N/A",
        "EventID" : 78,
        "MG" : "00000000-0000-0000-0000-000000000001",
        "TimeCollected" : "2015-02-07T14:10:04.69Z",
        "ManagementGroupName" : "AOI-5bf9a37f-e841-462b-80d2-1d19cd97dc40",
        "id" : "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
        "Type" : "Event",
        "__metadata" : {
          "Type" : "Event",
          "TimeGenerated" : "2015-02-07T14:07:33Z",
          "highlighting" : {
          "EventLevelName" : ["{[hl]}Error{[/hl]}"]
        }
      }]
    ],
            "start" : "2015-02-04T21:03:29.231Z",
            "end" : "2015-02-11T21:03:29.231Z"
          }
        }
      }]
    }
```

### <a name="searchid---actionread"></a><span data-ttu-id="210a0-155">Hledání / {ID} - akce/čtení</span><span class="sxs-lookup"><span data-stu-id="210a0-155">Search/{ID} - Action/Read</span></span>
<span data-ttu-id="210a0-156">**Požadavek na obsah uložené výsledky hledání:**</span><span class="sxs-lookup"><span data-stu-id="210a0-156">**Request the contents of a Saved Search:**</span></span>

```
    armclient post /subscriptions/{SubId}/resourceGroups/{ResourceGroupId}/providers/Microsoft.OperationalInsights/workspaces/{WorkspaceName}/search/{SearchId}?api-version=2015-03-20
```

> [!NOTE]
> <span data-ttu-id="210a0-157">Pokud hledání vrátí se stavem "Čekající na vyřízení", pak dotazování aktualizované výsledky lze provést prostřednictvím tohoto rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="210a0-157">If the search returns with a ‘Pending’ status, then polling the updated results can be done through this API.</span></span> <span data-ttu-id="210a0-158">Po 6 minimum výsledek hledání budou odstraněna z mezipaměti a zmizel HTTP bude vrácen.</span><span class="sxs-lookup"><span data-stu-id="210a0-158">After 6 min, the result of the search will be dropped from the cache and HTTP Gone will be returned.</span></span> <span data-ttu-id="210a0-159">Pokud požadavek počáteční vyhledávání vrátí, úspěšné, stav okamžitě, výsledky nejsou přidány do mezipaměti způsobuje toto rozhraní API vrátit zmizel HTTP, pokud dotaz.</span><span class="sxs-lookup"><span data-stu-id="210a0-159">If the initial search request returns a ‘Successful’ status immediately, the results are not added to the cache causing this API to return HTTP Gone if queried.</span></span> <span data-ttu-id="210a0-160">Obsah výsledek HTTP 200 jsou ve stejném formátu jako počáteční vyhledávání požadavek právě s aktualizovanými hodnotami.</span><span class="sxs-lookup"><span data-stu-id="210a0-160">The contents of an HTTP 200 result are in the same format as the initial search request just with updated values.</span></span>
>
>

### <a name="saved-searches"></a><span data-ttu-id="210a0-161">Uložená hledání</span><span class="sxs-lookup"><span data-stu-id="210a0-161">Saved searches</span></span>
<span data-ttu-id="210a0-162">**Žádosti o seznam uložených hledání:**</span><span class="sxs-lookup"><span data-stu-id="210a0-162">**Request List of Saved Searches:**</span></span>

```
    armclient post /subscriptions/{SubId}/resourceGroups/{ResourceGroupId}/providers/Microsoft.OperationalInsights/workspaces/{WorkspaceName}/savedSearches?api-version=2015-03-20
```

<span data-ttu-id="210a0-163">Podporované metody: GET, PUT odstranit</span><span class="sxs-lookup"><span data-stu-id="210a0-163">Supported methods: GET PUT DELETE</span></span>

<span data-ttu-id="210a0-164">Podporované metody kolekce: získání</span><span class="sxs-lookup"><span data-stu-id="210a0-164">Supported collection methods: GET</span></span>

<span data-ttu-id="210a0-165">Následující tabulka popisuje vlastnosti, které jsou k dispozici.</span><span class="sxs-lookup"><span data-stu-id="210a0-165">The following table describes the properties that are available.</span></span>

| <span data-ttu-id="210a0-166">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="210a0-166">Property</span></span> | <span data-ttu-id="210a0-167">Popis</span><span class="sxs-lookup"><span data-stu-id="210a0-167">Description</span></span> |
| --- | --- |
| <span data-ttu-id="210a0-168">ID</span><span class="sxs-lookup"><span data-stu-id="210a0-168">Id</span></span> |<span data-ttu-id="210a0-169">Jedinečný identifikátor.</span><span class="sxs-lookup"><span data-stu-id="210a0-169">The unique identifier.</span></span> |
| <span data-ttu-id="210a0-170">Značka Etag</span><span class="sxs-lookup"><span data-stu-id="210a0-170">Etag</span></span> |<span data-ttu-id="210a0-171">**Vyžaduje se pro opravu**.</span><span class="sxs-lookup"><span data-stu-id="210a0-171">**Required for Patch**.</span></span> <span data-ttu-id="210a0-172">Server aktualizaci na každý zápis.</span><span class="sxs-lookup"><span data-stu-id="210a0-172">Updated by server on each write.</span></span> <span data-ttu-id="210a0-173">Hodnota musí být stejná jako aktuální hodnota uložené nebo ' *' aktualizovat.</span><span class="sxs-lookup"><span data-stu-id="210a0-173">Value must be equal to the current stored value or ‘*’ to update.</span></span> <span data-ttu-id="210a0-174">409 vrátil hodnoty starý nebo neplatná.</span><span class="sxs-lookup"><span data-stu-id="210a0-174">409 returned for old/invalid values.</span></span> |
| <span data-ttu-id="210a0-175">Properties.Query</span><span class="sxs-lookup"><span data-stu-id="210a0-175">properties.query</span></span> |<span data-ttu-id="210a0-176">**Požadované**.</span><span class="sxs-lookup"><span data-stu-id="210a0-176">**Required**.</span></span> <span data-ttu-id="210a0-177">Vyhledávací dotaz.</span><span class="sxs-lookup"><span data-stu-id="210a0-177">The search query.</span></span> |
| <span data-ttu-id="210a0-178">properties.displayName</span><span class="sxs-lookup"><span data-stu-id="210a0-178">properties.displayName</span></span> |<span data-ttu-id="210a0-179">**Požadované**.</span><span class="sxs-lookup"><span data-stu-id="210a0-179">**Required**.</span></span> <span data-ttu-id="210a0-180">Uživatelem definované zobrazovaný název dotazu.</span><span class="sxs-lookup"><span data-stu-id="210a0-180">The user-defined display name of the query.</span></span> |
| <span data-ttu-id="210a0-181">Properties.category</span><span class="sxs-lookup"><span data-stu-id="210a0-181">properties.category</span></span> |<span data-ttu-id="210a0-182">**Požadované**.</span><span class="sxs-lookup"><span data-stu-id="210a0-182">**Required**.</span></span> <span data-ttu-id="210a0-183">Uživatelem definované kategorie dotaz.</span><span class="sxs-lookup"><span data-stu-id="210a0-183">The user-defined category of the query.</span></span> |

> [!NOTE]
> <span data-ttu-id="210a0-184">Rozhraní API pro vyhledávání analýzy protokolů aktuálně vrací hodnotu vytvořené uživatelem uložená hledání při dotazování pro uložená hledání v pracovním prostoru.</span><span class="sxs-lookup"><span data-stu-id="210a0-184">The Log Analytics search API currently returns user-created saved searches when polled for saved searches in a workspace.</span></span> <span data-ttu-id="210a0-185">Rozhraní API nevrací uložená hledání poskytované řešení.</span><span class="sxs-lookup"><span data-stu-id="210a0-185">The API does not return saved searches provided by solutions.</span></span>
>
>

### <a name="create-saved-searches"></a><span data-ttu-id="210a0-186">Vytvoření uložených hledání</span><span class="sxs-lookup"><span data-stu-id="210a0-186">Create saved searches</span></span>
<span data-ttu-id="210a0-187">**Žádost:**</span><span class="sxs-lookup"><span data-stu-id="210a0-187">**Request:**</span></span>

```
    $savedSearchParametersJson = "{'properties': { 'Category': 'myCategory', 'DisplayName':'myDisplayName', 'Query':'* | measure Count() by Source', 'Version':'1'  }"
    armclient put /subscriptions/{Subscription ID}/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{Workspace ID}/savedSearches/thisismyid?api-version=2015-03-20 $savedSearchParametersJson
```

> [!NOTE]
> <span data-ttu-id="210a0-188">Název pro všechny uložená hledání, plány a akce, které jsou vytvořené pomocí rozhraní API Log Analytics musí být malými písmeny.</span><span class="sxs-lookup"><span data-stu-id="210a0-188">The name for all saved searches, schedules, and actions created with the Log Analytics API must be in lowercase.</span></span>

### <a name="delete-saved-searches"></a><span data-ttu-id="210a0-189">Odstraňte uložená hledání</span><span class="sxs-lookup"><span data-stu-id="210a0-189">Delete saved searches</span></span>
<span data-ttu-id="210a0-190">**Žádost:**</span><span class="sxs-lookup"><span data-stu-id="210a0-190">**Request:**</span></span>

```
    armclient delete /subscriptions/{Subscription ID}/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{Workspace ID}/savedSearches/thisIsMyId?api-version=2015-03-20
```

### <a name="update-saved-searches"></a><span data-ttu-id="210a0-191">Aktualizace uložených hledání</span><span class="sxs-lookup"><span data-stu-id="210a0-191">Update saved searches</span></span>
 <span data-ttu-id="210a0-192">**Žádost:**</span><span class="sxs-lookup"><span data-stu-id="210a0-192">**Request:**</span></span>

```
    $savedSearchParametersJson = "{'etag': 'W/`"datetime\'2015-04-16T23%3A35%3A35.3182423Z\'`"', 'properties': { 'Category': 'myCategory', 'DisplayName':'myDisplayName', 'Query':'* | measure Count() by Source', 'Version':'1'  }"
    armclient put /subscriptions/{Subscription ID}/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{Workspace ID}/savedSearches/thisIsMyId?api-version=2015-03-20 $savedSearchParametersJson
```

### <a name="metadata---json-only"></a><span data-ttu-id="210a0-193">Metadata – pouze JSON</span><span class="sxs-lookup"><span data-stu-id="210a0-193">Metadata - JSON only</span></span>
<span data-ttu-id="210a0-194">Tady je způsob, jak je uvedeno v poli pro všechny typy protokolu pro data shromážděná v pracovním prostoru.</span><span class="sxs-lookup"><span data-stu-id="210a0-194">Here’s a way to see the fields for all log types for the data collected in your workspace.</span></span> <span data-ttu-id="210a0-195">Například pokud chcete, že znáte, pokud má tento typ události pole s názvem počítače, pak tento dotaz je jeden způsob kontroly.</span><span class="sxs-lookup"><span data-stu-id="210a0-195">For example, if you want you know if the Event type has a field named Computer, then this query is one way to check.</span></span>

<span data-ttu-id="210a0-196">**Žádost o pole:**</span><span class="sxs-lookup"><span data-stu-id="210a0-196">**Request for Fields:**</span></span>

```
    armclient get /subscriptions/{SubId}/resourceGroups/{ResourceGroupId}/providers/Microsoft.OperationalInsights/workspaces/{WorkspaceName}/schema?api-version=2015-03-20
```

<span data-ttu-id="210a0-197">**Odpověď:**</span><span class="sxs-lookup"><span data-stu-id="210a0-197">**Response:**</span></span>

```
    {
      "__metadata" : {
        "schema" : {
          "name" : "Example Name",
          "version" : 2
        },
        "resultType" : "schema",
        "requestTime" : 35
      },
      "value" : [{
          "name" : "MG",
          "displayName" : "MG",
          "type" : "Guid",
          "facetable" : true,
          "display" : false,
          "ownerType" : ["PerfHourly", "ProtectionStatus", "Capacity_SMBUtilizationByHost", "Capacity_ArrayUtilization", "Capacity_SMBShareUtilization", "SQLAssessmentRecommendation", "Event", "ConfigurationChange", "ConfigurationAlert", "ADAssessmentRecommendation", "ConfigurationObject", "ConfigurationObjectProperty"]
        }, {
          "name" : "ManagementGroupName",
          "displayName" : "ManagementGroupName",
          "type" : "String",
          "facetable" : true,
          "display" : true,
          "ownerType" : ["PerfHourly", "ProtectionStatus", "Event", "ConfigurationChange", "ConfigurationAlert", "W3CIISLog", "AlertHistory", "Recommendation", "Alert", "SecurityEvent", "UpdateAgent", "RequiredUpdate", "ConfigurationObject", "ConfigurationObjectProperty"]
        }
      ]
    }
```

<span data-ttu-id="210a0-198">Následující tabulka popisuje vlastnosti, které jsou k dispozici.</span><span class="sxs-lookup"><span data-stu-id="210a0-198">The following table describes the properties that are available.</span></span>

| <span data-ttu-id="210a0-199">**Vlastnost**</span><span class="sxs-lookup"><span data-stu-id="210a0-199">**Property**</span></span> | <span data-ttu-id="210a0-200">**Popis**</span><span class="sxs-lookup"><span data-stu-id="210a0-200">**Description**</span></span> |
| --- | --- |
| <span data-ttu-id="210a0-201">jméno</span><span class="sxs-lookup"><span data-stu-id="210a0-201">name</span></span> |<span data-ttu-id="210a0-202">Název pole.</span><span class="sxs-lookup"><span data-stu-id="210a0-202">Field name.</span></span> |
| <span data-ttu-id="210a0-203">displayName</span><span class="sxs-lookup"><span data-stu-id="210a0-203">displayName</span></span> |<span data-ttu-id="210a0-204">Zobrazovaný název pole.</span><span class="sxs-lookup"><span data-stu-id="210a0-204">The display name of the field.</span></span> |
| <span data-ttu-id="210a0-205">type</span><span class="sxs-lookup"><span data-stu-id="210a0-205">type</span></span> |<span data-ttu-id="210a0-206">Typ v poli hodnota.</span><span class="sxs-lookup"><span data-stu-id="210a0-206">The Type of the field value.</span></span> |
| <span data-ttu-id="210a0-207">facetable (kategorizovatelné)</span><span class="sxs-lookup"><span data-stu-id="210a0-207">facetable</span></span> |<span data-ttu-id="210a0-208">Kombinace aktuální 'indexované', ' uložené ' a 'omezující vlastnost' vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="210a0-208">Combination of current ‘indexed’, ‘stored ‘and ‘facet’ properties.</span></span> |
| <span data-ttu-id="210a0-209">Zobrazení</span><span class="sxs-lookup"><span data-stu-id="210a0-209">display</span></span> |<span data-ttu-id="210a0-210">Aktuální vlastnost 'display'.</span><span class="sxs-lookup"><span data-stu-id="210a0-210">Current ‘display’ property.</span></span> <span data-ttu-id="210a0-211">Hodnota TRUE, pokud pole je viditelné ve vyhledávání.</span><span class="sxs-lookup"><span data-stu-id="210a0-211">True if field is visible in search.</span></span> |
| <span data-ttu-id="210a0-212">ownerType</span><span class="sxs-lookup"><span data-stu-id="210a0-212">ownerType</span></span> |<span data-ttu-id="210a0-213">Snížit na pouze typy, které patří do zařazený nemá IP.</span><span class="sxs-lookup"><span data-stu-id="210a0-213">Reduced to only Types belonging to onboarded IP’s.</span></span> |

## <a name="optional-parameters"></a><span data-ttu-id="210a0-214">Volitelné parametry</span><span class="sxs-lookup"><span data-stu-id="210a0-214">Optional parameters</span></span>
<span data-ttu-id="210a0-215">Následující informace popisují volitelné parametry, které jsou k dispozici.</span><span class="sxs-lookup"><span data-stu-id="210a0-215">The following information describes optional parameters available.</span></span>

### <a name="highlighting"></a><span data-ttu-id="210a0-216">Zvýraznění</span><span class="sxs-lookup"><span data-stu-id="210a0-216">Highlighting</span></span>
<span data-ttu-id="210a0-217">"Zvýraznění" parametr je volitelný parametr, který můžete použít k vyžádání subsystém hledání obsahují sadu značek v odpovědi.</span><span class="sxs-lookup"><span data-stu-id="210a0-217">The “Highlight” parameter is an optional parameter you may use to request the search subsystem include a set of markers in its response.</span></span>

<span data-ttu-id="210a0-218">Tyto značky upozornění na zahájení a ukončení zvýrazněný text, který odpovídá podmínky uvedené v vyhledávací dotaz.</span><span class="sxs-lookup"><span data-stu-id="210a0-218">These markers indicate the start and end highlighted text that matches the terms provided in your search query.</span></span>
<span data-ttu-id="210a0-219">Můžete určit počáteční a koncové značky, které jsou používány vyhledávání obtékat zvýrazněná termín.</span><span class="sxs-lookup"><span data-stu-id="210a0-219">You may specify the start and end markers that are used by search to wrap the highlighted term.</span></span>

<span data-ttu-id="210a0-220">**Příklad vyhledávací dotaz.**</span><span class="sxs-lookup"><span data-stu-id="210a0-220">**Example search query**</span></span>

```
    $savedSearchParametersJson =
    {
      "top":150,
      "highlight":{
        "pre":"{[hl]}",
        "post":"{[/hl]}"
      },
      "query":"*",
      "start":"2015-02-04T21:03:29.231Z",
      "end":"2015-02-11T21:03:29.231Z"
    }
    armclient post /subscriptions/{Subscription ID}/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{Workspace ID}/search?api-version=2015-03-20 $searchParametersJson
```

<span data-ttu-id="210a0-221">**Ukázkové výsledky:**</span><span class="sxs-lookup"><span data-stu-id="210a0-221">**Sample result:**</span></span>

```
    {
        "TimeGenerated":
        "2015-05-18T23:55:59Z", "Source":
        "EventLog": "Application",
        "Computer": "smokedturkey.net",
        "EventCategory": 0,
        "EventLevel":1,
        "EventLevelName":
        "Error"
        "Manager ", "__metadata":
        {
            "Type": "Event",
            "TimeGenerated": "2015-05-18T23:55:59Z",
            "highlighting": {
                "EventLevelName":
                ["{[hl]}Error{[/hl]}"]
            }
        }
    }
```

<span data-ttu-id="210a0-222">Všimněte si, že předchozí výsledek obsahuje chybovou zprávu, která má předponu a připojí.</span><span class="sxs-lookup"><span data-stu-id="210a0-222">Notice that the preceding result contains an error message that has been prefixed and appended.</span></span>

## <a name="computer-groups"></a><span data-ttu-id="210a0-223">Skupiny počítačů</span><span class="sxs-lookup"><span data-stu-id="210a0-223">Computer groups</span></span>
<span data-ttu-id="210a0-224">Skupiny počítačů jsou speciální uložená hledání, které vrátí sadu počítačů.</span><span class="sxs-lookup"><span data-stu-id="210a0-224">Computer groups are special saved searches that return a set of computers.</span></span>  <span data-ttu-id="210a0-225">Skupina počítačů můžete použít v jiné dotazy omezit výsledky do počítače ve skupině.</span><span class="sxs-lookup"><span data-stu-id="210a0-225">You can use a computer group in other queries to limit the results to the computers in the group.</span></span>  <span data-ttu-id="210a0-226">Skupina počítačů je implementovaný jako uložené hledání se skupiny značku s hodnotou počítače.</span><span class="sxs-lookup"><span data-stu-id="210a0-226">A computer group is implemented as a saved search with a Group tag with a value of Computer.</span></span>

<span data-ttu-id="210a0-227">Následuje ukázková odpověď pro skupinu počítačů.</span><span class="sxs-lookup"><span data-stu-id="210a0-227">Following is a sample response for a computer group.</span></span>

```
    "etag": "W/\"datetime'2016-04-01T13%3A38%3A04.7763203Z'\"",
    "properties": {
        "Category": "My Computer Groups",
        "DisplayName": "My Computer Group",
        "Query": "srv* | Distinct Computer",
        "Tags": [{
            "Name": "Group",
            "Value": "Computer"
          }],
    "Version": 1
    }
```

### <a name="retrieving-computer-groups"></a><span data-ttu-id="210a0-228">Načítání skupin počítačů</span><span class="sxs-lookup"><span data-stu-id="210a0-228">Retrieving computer groups</span></span>
<span data-ttu-id="210a0-229">Chcete-li načíst skupinu počítačů, použijte metodu Get s ID skupiny.</span><span class="sxs-lookup"><span data-stu-id="210a0-229">To retrieve a computer group, use the Get method with the group ID.</span></span>

```
armclient get /subscriptions/{Subscription ID}/resourceGroups/{Resource Group Name}/providers/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/{Group ID}`?api-version=2015-03-20
```

### <a name="creating-or-updating-a-computer-group"></a><span data-ttu-id="210a0-230">Vytváření nebo aktualizaci skupinu počítačů</span><span class="sxs-lookup"><span data-stu-id="210a0-230">Creating or updating a computer group</span></span>
<span data-ttu-id="210a0-231">Chcete-li vytvořit skupinu počítačů, použijte metodu Put s ID jedinečný uloženého hledání.</span><span class="sxs-lookup"><span data-stu-id="210a0-231">To create a computer group, use the Put method with a unique saved search ID.</span></span> <span data-ttu-id="210a0-232">Pokud chcete použít existující skupinu ID počítače, než je upravit.</span><span class="sxs-lookup"><span data-stu-id="210a0-232">If you use an existing computer group ID, then that one is modified.</span></span> <span data-ttu-id="210a0-233">Když vytvoříte skupinu počítačů na portálu analýzy protokolů, ID je vytvořen ze skupiny a název.</span><span class="sxs-lookup"><span data-stu-id="210a0-233">When you create a computer group in the Log Analytics portal, then the ID is created from the group and name.</span></span>

<span data-ttu-id="210a0-234">Dotaz, použít pro definice skupiny musí vrátit sadu počítačů pro skupinu ke správnému fungování.</span><span class="sxs-lookup"><span data-stu-id="210a0-234">The query used for the group definition must return a set of computers for the group to function properly.</span></span>  <span data-ttu-id="210a0-235">Doporučuje se, že vám stát, že váš dotaz s `| Distinct Computer` zajistit správné data jsou vrácena.</span><span class="sxs-lookup"><span data-stu-id="210a0-235">It's recommended that you end your query with `| Distinct Computer` to ensure the correct data is returned.</span></span>

<span data-ttu-id="210a0-236">Definice uloženého hledání musí obsahovat značku s hodnotou počítače pro hledání být označeny jako skupinu počítačů s názvem skupiny.</span><span class="sxs-lookup"><span data-stu-id="210a0-236">The definition of the saved search must include a tag named Group with a value of Computer for the search to be classified as a computer group.</span></span>

```
    $etag=Get-Date -Format yyyy-MM-ddThh:mm:ss.msZ
    $groupName="My Computer Group"
    $groupQuery = "Computer=srv* | Distinct Computer"
    $groupCategory="My Computer Groups"
    $groupID = "My Computer Groups | My Computer Group"

    $groupJson = "{'etag': 'W/`"datetime\'" + $etag + "\'`"', 'properties': { 'Category': '" + $groupCategory + "', 'DisplayName':'"  + $groupName + "', 'Query':'" + $groupQuery + "', 'Tags': [{'Name': 'Group', 'Value': 'Computer'}], 'Version':'1'  }"

    armclient put /subscriptions/{Subscription ID}/resourceGroups/{Resource Group Name}/providers/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/$groupId`?api-version=2015-03-20 $groupJson
```

### <a name="deleting-computer-groups"></a><span data-ttu-id="210a0-237">Odstranění skupiny počítačů</span><span class="sxs-lookup"><span data-stu-id="210a0-237">Deleting computer groups</span></span>
<span data-ttu-id="210a0-238">Chcete-li odstranit skupinu počítačů, použijte metodu Delete s ID skupiny.</span><span class="sxs-lookup"><span data-stu-id="210a0-238">To delete a computer group, use the Delete method with the group ID.</span></span>

```
armclient delete /subscriptions/{Subscription ID}/resourceGroups/{Resource Group Name}/providers/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/$groupId`?api-version=2015-03-20
```


## <a name="next-steps"></a><span data-ttu-id="210a0-239">Další kroky</span><span class="sxs-lookup"><span data-stu-id="210a0-239">Next steps</span></span>
* <span data-ttu-id="210a0-240">Další informace o [protokolu hledání](log-analytics-log-searches.md) k vytvoření dotazů pomocí vlastních polí pro kritéria.</span><span class="sxs-lookup"><span data-stu-id="210a0-240">Learn about [log searches](log-analytics-log-searches.md) to build queries using custom fields for criteria.</span></span>
