---
title: "hledání aaaLog analýzy protokolů REST API | Microsoft Docs"
description: "Tato příručka poskytuje základní kurz popisující, jak je možné používat hello analýzy protokolů hledání rozhraní API REST v hello Operations Management Suite (OMS) a poskytuje příklady, které ukazují, jak toouse hello příkazy."
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
ms.openlocfilehash: dafe5eeb8cc11a339f2cbf78cec657e344d87cac
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="log-analytics-log-search-rest-api"></a><span data-ttu-id="360d2-103">Hledání protokolu analýzy protokolů REST API</span><span class="sxs-lookup"><span data-stu-id="360d2-103">Log Analytics log search REST API</span></span>
<span data-ttu-id="360d2-104">Tato příručka obsahuje základní kurz, včetně příkladů, jak lze použít hello Log Analytics vyhledávání REST API.</span><span class="sxs-lookup"><span data-stu-id="360d2-104">This guide provides a basic tutorial, including examples, of how you can use hello Log Analytics Search REST API.</span></span> <span data-ttu-id="360d2-105">Analýzy protokolů je součástí hello Operations Management Suite (OMS).</span><span class="sxs-lookup"><span data-stu-id="360d2-105">Log Analytics is part of hello Operations Management Suite (OMS).</span></span>

> [!NOTE]
> <span data-ttu-id="360d2-106">Pokud pracovní prostor byl upgradovaný toohello [nové analýzy protokolů dotazu jazyka](log-analytics-log-search-upgrade.md), pak by měly pokračovat toouse hello starší verze dotazovací jazyk s hledání protokolů hello rozhraní API, jak je popsáno v tomto článku.</span><span class="sxs-lookup"><span data-stu-id="360d2-106">If your workspace has been upgraded toohello [new Log Analytics query language](log-analytics-log-search-upgrade.md), then you should continue toouse hello legacy query language with hello log search API as described in this article.</span></span>  <span data-ttu-id="360d2-107">Nové rozhraní API, budou vydané pro upgradovaný pracovních prostorů a dokumentaci hello budou aktualizovány v daném čase.</span><span class="sxs-lookup"><span data-stu-id="360d2-107">A new API will be released for upgraded workspaces, and hello documentation will be updated at that time.</span></span> 

> [!NOTE]
> <span data-ttu-id="360d2-108">Analýzy protokolů volala dřív Operational Insights, proto je hello název používaný v hello poskytovatele prostředků.</span><span class="sxs-lookup"><span data-stu-id="360d2-108">Log Analytics was previously called Operational Insights, which is why it is hello name used in hello resource provider.</span></span>
>
>

## <a name="overview-of-hello-log-search-rest-api"></a><span data-ttu-id="360d2-109">Přehled hello protokolu vyhledávání REST API</span><span class="sxs-lookup"><span data-stu-id="360d2-109">Overview of hello Log Search REST API</span></span>
<span data-ttu-id="360d2-110">Hello Log Analytics vyhledávání REST API je dosáhl standardu RESTful a je přístupný prostřednictvím hello rozhraní API Správce prostředků Azure.</span><span class="sxs-lookup"><span data-stu-id="360d2-110">hello Log Analytics Search REST API is RESTful and can be accessed via hello Azure Resource Manager API.</span></span> <span data-ttu-id="360d2-111">Tento článek obsahuje příklady přístup k API hello prostřednictvím [ARMClient](https://github.com/projectkudu/ARMClient), nástroje příkazového řádku s otevřeným zdrojem, který zjednodušuje vyvolání hello rozhraní API Správce prostředků Azure.</span><span class="sxs-lookup"><span data-stu-id="360d2-111">This article provides examples of accessing hello API through [ARMClient](https://github.com/projectkudu/ARMClient), an open source command-line tool that simplifies invoking hello Azure Resource Manager API.</span></span> <span data-ttu-id="360d2-112">použití Hello ARMClient je jedním z mnoha možností tooaccess hello Log Analytics vyhledávání API.</span><span class="sxs-lookup"><span data-stu-id="360d2-112">hello use of ARMClient is one of many options tooaccess hello Log Analytics Search API.</span></span> <span data-ttu-id="360d2-113">Další možností je modul Azure PowerShell hello toouse pro OperationalInsights, který obsahuje rutiny pro přístup k vyhledávání.</span><span class="sxs-lookup"><span data-stu-id="360d2-113">Another option is toouse hello Azure PowerShell module for OperationalInsights, which includes cmdlets for accessing search.</span></span> <span data-ttu-id="360d2-114">Pomocí těchto nástrojů můžete využívat pracovní prostory hello rozhraní API služby Azure Resource Manager toomake volání tooOMS a provádět příkazy vyhledávání v nich.</span><span class="sxs-lookup"><span data-stu-id="360d2-114">With these tools, you can utilize hello Azure Resource Manager API toomake calls tooOMS workspaces and perform search commands within them.</span></span> <span data-ttu-id="360d2-115">Hello API výstupy výsledky hledání ve formátu JSON, což vám výsledky hledání hello toouse mnoha různými způsoby prostřednictvím kódu programu.</span><span class="sxs-lookup"><span data-stu-id="360d2-115">hello API outputs search results in JSON format, allowing you toouse hello search results in many different ways programmatically.</span></span>

<span data-ttu-id="360d2-116">Hello Azure Resource Manager je možné prostřednictvím [knihovna pro .NET](https://msdn.microsoft.com/library/azure/dn910477.aspx) a hello [REST API](https://msdn.microsoft.com/library/azure/mt163658.aspx).</span><span class="sxs-lookup"><span data-stu-id="360d2-116">hello Azure Resource Manager can be used via a [Library for .NET](https://msdn.microsoft.com/library/azure/dn910477.aspx) and hello [REST API](https://msdn.microsoft.com/library/azure/mt163658.aspx).</span></span> <span data-ttu-id="360d2-117">toolearn víc, přečtěte si hello propojené webové stránky.</span><span class="sxs-lookup"><span data-stu-id="360d2-117">toolearn more, review hello linked web pages.</span></span>

> [!NOTE]
> <span data-ttu-id="360d2-118">Pokud například použijete příkaz agregace `|measure count()` nebo `distinct`, každé volání toosearch může vrátit maximálně 500 000 záznamů.</span><span class="sxs-lookup"><span data-stu-id="360d2-118">If you use an aggregation command such as `|measure count()` or `distinct`, each call toosearch can return upto 500,000 records.</span></span> <span data-ttu-id="360d2-119">Hledání, které neobsahují příkaz agregace vrátit maximálně 5 000 záznamů.</span><span class="sxs-lookup"><span data-stu-id="360d2-119">Searches that do not include an aggregation command return upto 5,000 records.</span></span>
>
>

## <a name="basic-log-analytics-search-rest-api-tutorial"></a><span data-ttu-id="360d2-120">Základní kurz Log Analytics vyhledávání REST API</span><span class="sxs-lookup"><span data-stu-id="360d2-120">Basic Log Analytics Search REST API tutorial</span></span>
### <a name="toouse-armclient"></a><span data-ttu-id="360d2-121">toouse ARMClient</span><span class="sxs-lookup"><span data-stu-id="360d2-121">toouse ARMClient</span></span>
1. <span data-ttu-id="360d2-122">Nainstalujte [Chocolatey](https://chocolatey.org/), což je otevřete Správce balíčků zdroje pro systém Windows.</span><span class="sxs-lookup"><span data-stu-id="360d2-122">Install [Chocolatey](https://chocolatey.org/), which is an Open Source Package Manager for Windows.</span></span> <span data-ttu-id="360d2-123">Otevřete okno příkazového řádku jako správce a spusťte následující příkaz hello:</span><span class="sxs-lookup"><span data-stu-id="360d2-123">Open a command prompt window as administrator and then run hello following command:</span></span>

    ```
    @powershell -NoProfile -ExecutionPolicy unrestricted -Command "iex ((new-object net.webclient).DownloadString('https://chocolatey.org/install.ps1'))" && SET PATH=%PATH%;%ALLUSERSPROFILE%\chocolatey\bin
    ```
2. <span data-ttu-id="360d2-124">Instalace ARMClient spuštěním hello následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="360d2-124">Install ARMClient by running hello following command:</span></span>

    ```
    choco install armclient
    ```

### <a name="tooperform-a-search-using-armclient"></a><span data-ttu-id="360d2-125">tooperform a vyhledávání pomocí ARMClient</span><span class="sxs-lookup"><span data-stu-id="360d2-125">tooperform a search using ARMClient</span></span>
1. <span data-ttu-id="360d2-126">Přihlaste se pomocí účtu Microsoft nebo pracovní nebo školní účet:</span><span class="sxs-lookup"><span data-stu-id="360d2-126">Log in using your Microsoft account or your work or school account:</span></span>

    ```
    armclient login
    ```

    <span data-ttu-id="360d2-127">Úspěšné přihlášení uvádí všechny odběry svázané toohello zadaný účet:</span><span class="sxs-lookup"><span data-stu-id="360d2-127">A successful login lists all subscriptions tied toohello given account:</span></span>

    ```
    PS C:\Users\SampleUserName> armclient login
    Welcome YourEmail@ORG.com (Tenant: zzzzzzzz-zzzz-zzzz-zzzz-zzzzzzzzzzzz)
    User: YourEmail@ORG.com, Tenant: zzzzzzzz-zzzz-zzzz-zzzz-zzzzzzzzzzzz (Example org)
    There are 3 subscriptions
    Subscription xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx (Example Name 1)
    Subscription xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx (Example Name 2)
    Subscription xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx (Example Name 3)
    ```
2. <span data-ttu-id="360d2-128">Získáte pracovní prostory hello Operations Management Suite:</span><span class="sxs-lookup"><span data-stu-id="360d2-128">Get hello Operations Management Suite workspaces:</span></span>

    ```
    armclient get /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces?api-version=2015-03-20
    ```

    <span data-ttu-id="360d2-129">Úspěšné volání Get by výstup, že všechny pracovní prostory svázané toohello předplatného:</span><span class="sxs-lookup"><span data-stu-id="360d2-129">A successful Get call would output all workspaces tied toohello subscription:</span></span>

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
3. <span data-ttu-id="360d2-130">Vytvořte vaše proměnná vyhledávání:</span><span class="sxs-lookup"><span data-stu-id="360d2-130">Create your search variable:</span></span>

    ```
    $mySearch = "{ 'top':150, 'query':'Error'}";
    ```
4. <span data-ttu-id="360d2-131">Vyhledávání pomocí vaší novou proměnnou vyhledávání:</span><span class="sxs-lookup"><span data-stu-id="360d2-131">Search using your new search variable:</span></span>

    ```
    armclient post /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{WORKSPACE NAME}/search?api-version=2015-03-20 $mySearch
    ```

## <a name="log-analytics-search-rest-api-reference-examples"></a><span data-ttu-id="360d2-132">Příklady referenční dokumentace rozhraní API REST Analytics vyhledávání protokolu</span><span class="sxs-lookup"><span data-stu-id="360d2-132">Log Analytics Search REST API reference examples</span></span>
<span data-ttu-id="360d2-133">Hello následující příklady ukazují, jak je možné používat hello rozhraní API pro hledání.</span><span class="sxs-lookup"><span data-stu-id="360d2-133">hello following examples show you how you can use hello Search API.</span></span>

### <a name="search---actionread"></a><span data-ttu-id="360d2-134">Hledání - akce/čtení</span><span class="sxs-lookup"><span data-stu-id="360d2-134">Search - Action/Read</span></span>
<span data-ttu-id="360d2-135">**Ukázka adresa Url:**</span><span class="sxs-lookup"><span data-stu-id="360d2-135">**Sample Url:**</span></span>

```
    /subscriptions/{SubscriptionId}/resourceGroups/{ResourceGroupId}/providers/Microsoft.OperationalInsights/workspaces/{WorkspaceName}/search?api-version=2015-03-20
```

<span data-ttu-id="360d2-136">**Žádost:**</span><span class="sxs-lookup"><span data-stu-id="360d2-136">**Request:**</span></span>

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
<span data-ttu-id="360d2-137">Hello následující tabulka popisuje hello vlastnosti, které jsou k dispozici.</span><span class="sxs-lookup"><span data-stu-id="360d2-137">hello following table describes hello properties that are available.</span></span>

| <span data-ttu-id="360d2-138">**Vlastnost**</span><span class="sxs-lookup"><span data-stu-id="360d2-138">**Property**</span></span> | <span data-ttu-id="360d2-139">**Popis**</span><span class="sxs-lookup"><span data-stu-id="360d2-139">**Description**</span></span> |
| --- | --- |
| <span data-ttu-id="360d2-140">Horní</span><span class="sxs-lookup"><span data-stu-id="360d2-140">top</span></span> |<span data-ttu-id="360d2-141">Hello maximální počet výsledků tooreturn.</span><span class="sxs-lookup"><span data-stu-id="360d2-141">hello maximum number of results tooreturn.</span></span> |
| <span data-ttu-id="360d2-142">Zvýrazněte</span><span class="sxs-lookup"><span data-stu-id="360d2-142">highlight</span></span> |<span data-ttu-id="360d2-143">Obsahuje předběžné a post parametry, obvykle použité pro zvýraznění odpovídající pole</span><span class="sxs-lookup"><span data-stu-id="360d2-143">Contains pre and post parameters, used usually for highlighting matching fields</span></span> |
| <span data-ttu-id="360d2-144">Před</span><span class="sxs-lookup"><span data-stu-id="360d2-144">pre</span></span> |<span data-ttu-id="360d2-145">Předpony hello zadaný řetězec tooyour shodná pole.</span><span class="sxs-lookup"><span data-stu-id="360d2-145">Prefixes hello given string tooyour matched fields.</span></span> |
| <span data-ttu-id="360d2-146">POST</span><span class="sxs-lookup"><span data-stu-id="360d2-146">post</span></span> |<span data-ttu-id="360d2-147">Připojí zadaný řetězec tooyour shodná pole hello.</span><span class="sxs-lookup"><span data-stu-id="360d2-147">Appends hello given string tooyour matched fields.</span></span> |
| <span data-ttu-id="360d2-148">query</span><span class="sxs-lookup"><span data-stu-id="360d2-148">query</span></span> |<span data-ttu-id="360d2-149">vyhledávací dotaz Hello používá toocollect a vrátí výsledky.</span><span class="sxs-lookup"><span data-stu-id="360d2-149">hello search query used toocollect and return results.</span></span> |
| <span data-ttu-id="360d2-150">start</span><span class="sxs-lookup"><span data-stu-id="360d2-150">start</span></span> |<span data-ttu-id="360d2-151">Hello začátek časového okna hello chcete výsledky toobe nalezených v.</span><span class="sxs-lookup"><span data-stu-id="360d2-151">hello beginning of hello time window you want results toobe found from.</span></span> |
| <span data-ttu-id="360d2-152">End</span><span class="sxs-lookup"><span data-stu-id="360d2-152">end</span></span> |<span data-ttu-id="360d2-153">Hello konec časového okna hello chcete výsledky toobe nalezených v.</span><span class="sxs-lookup"><span data-stu-id="360d2-153">hello end of hello time window you want results toobe found from.</span></span> |

<span data-ttu-id="360d2-154">**Odpověď:**</span><span class="sxs-lookup"><span data-stu-id="360d2-154">**Response:**</span></span>

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

### <a name="searchid---actionread"></a><span data-ttu-id="360d2-155">Hledání / {ID} - akce/čtení</span><span class="sxs-lookup"><span data-stu-id="360d2-155">Search/{ID} - Action/Read</span></span>
<span data-ttu-id="360d2-156">**Žádost o obsah hello uložené výsledky hledání:**</span><span class="sxs-lookup"><span data-stu-id="360d2-156">**Request hello contents of a Saved Search:**</span></span>

```
    armclient post /subscriptions/{SubId}/resourceGroups/{ResourceGroupId}/providers/Microsoft.OperationalInsights/workspaces/{WorkspaceName}/search/{SearchId}?api-version=2015-03-20
```

> [!NOTE]
> <span data-ttu-id="360d2-157">Pokud hello vyhledávání vrací se stavem 'Čekající', dotazování hello aktualizovat výsledky můžete to provést prostřednictvím tohoto rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="360d2-157">If hello search returns with a ‘Pending’ status, then polling hello updated results can be done through this API.</span></span> <span data-ttu-id="360d2-158">Po 6 min hello výsledky hledání hello budou odstraněna z mezipaměti hello a zmizel HTTP bude vrácen.</span><span class="sxs-lookup"><span data-stu-id="360d2-158">After 6 min, hello result of hello search will be dropped from hello cache and HTTP Gone will be returned.</span></span> <span data-ttu-id="360d2-159">Pokud požadavek počáteční hledání hello vrátí 'úspěšné, stav okamžitě, výsledky hello nebyly přidány toohello mezipaměti způsobuje toto rozhraní API tooreturn zmizel HTTP, pokud dotaz.</span><span class="sxs-lookup"><span data-stu-id="360d2-159">If hello initial search request returns a ‘Successful’ status immediately, hello results are not added toohello cache causing this API tooreturn HTTP Gone if queried.</span></span> <span data-ttu-id="360d2-160">obsah Hello výsledek HTTP 200 jsou hello stejný formát jako požadavku počáteční hledání hello právě s aktualizovanými hodnotami.</span><span class="sxs-lookup"><span data-stu-id="360d2-160">hello contents of an HTTP 200 result are in hello same format as hello initial search request just with updated values.</span></span>
>
>

### <a name="saved-searches"></a><span data-ttu-id="360d2-161">Uložená hledání</span><span class="sxs-lookup"><span data-stu-id="360d2-161">Saved searches</span></span>
<span data-ttu-id="360d2-162">**Žádosti o seznam uložených hledání:**</span><span class="sxs-lookup"><span data-stu-id="360d2-162">**Request List of Saved Searches:**</span></span>

```
    armclient post /subscriptions/{SubId}/resourceGroups/{ResourceGroupId}/providers/Microsoft.OperationalInsights/workspaces/{WorkspaceName}/savedSearches?api-version=2015-03-20
```

<span data-ttu-id="360d2-163">Podporované metody: GET, PUT odstranit</span><span class="sxs-lookup"><span data-stu-id="360d2-163">Supported methods: GET PUT DELETE</span></span>

<span data-ttu-id="360d2-164">Podporované metody kolekce: získání</span><span class="sxs-lookup"><span data-stu-id="360d2-164">Supported collection methods: GET</span></span>

<span data-ttu-id="360d2-165">Hello následující tabulka popisuje hello vlastnosti, které jsou k dispozici.</span><span class="sxs-lookup"><span data-stu-id="360d2-165">hello following table describes hello properties that are available.</span></span>

| <span data-ttu-id="360d2-166">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="360d2-166">Property</span></span> | <span data-ttu-id="360d2-167">Popis</span><span class="sxs-lookup"><span data-stu-id="360d2-167">Description</span></span> |
| --- | --- |
| <span data-ttu-id="360d2-168">ID</span><span class="sxs-lookup"><span data-stu-id="360d2-168">Id</span></span> |<span data-ttu-id="360d2-169">Jedinečný identifikátor Hello.</span><span class="sxs-lookup"><span data-stu-id="360d2-169">hello unique identifier.</span></span> |
| <span data-ttu-id="360d2-170">Značka Etag</span><span class="sxs-lookup"><span data-stu-id="360d2-170">Etag</span></span> |<span data-ttu-id="360d2-171">**Vyžaduje se pro opravu**.</span><span class="sxs-lookup"><span data-stu-id="360d2-171">**Required for Patch**.</span></span> <span data-ttu-id="360d2-172">Server aktualizaci na každý zápis.</span><span class="sxs-lookup"><span data-stu-id="360d2-172">Updated by server on each write.</span></span> <span data-ttu-id="360d2-173">Hodnota musí být rovna toohello aktuální uložené hodnoty nebo ' *' tooupdate.</span><span class="sxs-lookup"><span data-stu-id="360d2-173">Value must be equal toohello current stored value or ‘*’ tooupdate.</span></span> <span data-ttu-id="360d2-174">409 vrátil hodnoty starý nebo neplatná.</span><span class="sxs-lookup"><span data-stu-id="360d2-174">409 returned for old/invalid values.</span></span> |
| <span data-ttu-id="360d2-175">Properties.Query</span><span class="sxs-lookup"><span data-stu-id="360d2-175">properties.query</span></span> |<span data-ttu-id="360d2-176">**Požadované**.</span><span class="sxs-lookup"><span data-stu-id="360d2-176">**Required**.</span></span> <span data-ttu-id="360d2-177">Hello vyhledávací dotaz.</span><span class="sxs-lookup"><span data-stu-id="360d2-177">hello search query.</span></span> |
| <span data-ttu-id="360d2-178">properties.displayName</span><span class="sxs-lookup"><span data-stu-id="360d2-178">properties.displayName</span></span> |<span data-ttu-id="360d2-179">**Požadované**.</span><span class="sxs-lookup"><span data-stu-id="360d2-179">**Required**.</span></span> <span data-ttu-id="360d2-180">uživatelem definované zobrazovaný název Hello hello dotazu.</span><span class="sxs-lookup"><span data-stu-id="360d2-180">hello user-defined display name of hello query.</span></span> |
| <span data-ttu-id="360d2-181">Properties.category</span><span class="sxs-lookup"><span data-stu-id="360d2-181">properties.category</span></span> |<span data-ttu-id="360d2-182">**Požadované**.</span><span class="sxs-lookup"><span data-stu-id="360d2-182">**Required**.</span></span> <span data-ttu-id="360d2-183">Hello uživatelem definované kategorie hello dotazu.</span><span class="sxs-lookup"><span data-stu-id="360d2-183">hello user-defined category of hello query.</span></span> |

> [!NOTE]
> <span data-ttu-id="360d2-184">Hello rozhraní API pro vyhledávání analýzy protokolů aktuálně vrací hodnotu vytvořené uživatelem uložená hledání při dotazování pro uložená hledání v pracovním prostoru.</span><span class="sxs-lookup"><span data-stu-id="360d2-184">hello Log Analytics search API currently returns user-created saved searches when polled for saved searches in a workspace.</span></span> <span data-ttu-id="360d2-185">Hello API nevrací uložená hledání poskytované řešení.</span><span class="sxs-lookup"><span data-stu-id="360d2-185">hello API does not return saved searches provided by solutions.</span></span>
>
>

### <a name="create-saved-searches"></a><span data-ttu-id="360d2-186">Vytvoření uložených hledání</span><span class="sxs-lookup"><span data-stu-id="360d2-186">Create saved searches</span></span>
<span data-ttu-id="360d2-187">**Žádost:**</span><span class="sxs-lookup"><span data-stu-id="360d2-187">**Request:**</span></span>

```
    $savedSearchParametersJson = "{'properties': { 'Category': 'myCategory', 'DisplayName':'myDisplayName', 'Query':'* | measure Count() by Source', 'Version':'1'  }"
    armclient put /subscriptions/{Subscription ID}/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{Workspace ID}/savedSearches/thisismyid?api-version=2015-03-20 $savedSearchParametersJson
```

> [!NOTE]
> <span data-ttu-id="360d2-188">Název Hello všechna uložená hledání, plány a akce, které jsou vytvořené pomocí hello Log Analytics API musí být malými písmeny.</span><span class="sxs-lookup"><span data-stu-id="360d2-188">hello name for all saved searches, schedules, and actions created with hello Log Analytics API must be in lowercase.</span></span>

### <a name="delete-saved-searches"></a><span data-ttu-id="360d2-189">Odstraňte uložená hledání</span><span class="sxs-lookup"><span data-stu-id="360d2-189">Delete saved searches</span></span>
<span data-ttu-id="360d2-190">**Žádost:**</span><span class="sxs-lookup"><span data-stu-id="360d2-190">**Request:**</span></span>

```
    armclient delete /subscriptions/{Subscription ID}/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{Workspace ID}/savedSearches/thisIsMyId?api-version=2015-03-20
```

### <a name="update-saved-searches"></a><span data-ttu-id="360d2-191">Aktualizace uložených hledání</span><span class="sxs-lookup"><span data-stu-id="360d2-191">Update saved searches</span></span>
 <span data-ttu-id="360d2-192">**Žádost:**</span><span class="sxs-lookup"><span data-stu-id="360d2-192">**Request:**</span></span>

```
    $savedSearchParametersJson = "{'etag': 'W/`"datetime\'2015-04-16T23%3A35%3A35.3182423Z\'`"', 'properties': { 'Category': 'myCategory', 'DisplayName':'myDisplayName', 'Query':'* | measure Count() by Source', 'Version':'1'  }"
    armclient put /subscriptions/{Subscription ID}/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{Workspace ID}/savedSearches/thisIsMyId?api-version=2015-03-20 $savedSearchParametersJson
```

### <a name="metadata---json-only"></a><span data-ttu-id="360d2-193">Metadata – pouze JSON</span><span class="sxs-lookup"><span data-stu-id="360d2-193">Metadata - JSON only</span></span>
<span data-ttu-id="360d2-194">Zde je způsob toosee hello pole pro všechny typy protokolu pro hello data shromážděná v pracovním prostoru.</span><span class="sxs-lookup"><span data-stu-id="360d2-194">Here’s a way toosee hello fields for all log types for hello data collected in your workspace.</span></span> <span data-ttu-id="360d2-195">Například pokud chcete, že víte, že pokud hello typ události obsahuje pole s názvem počítače, pak tento dotaz je jedním ze způsobů toocheck.</span><span class="sxs-lookup"><span data-stu-id="360d2-195">For example, if you want you know if hello Event type has a field named Computer, then this query is one way toocheck.</span></span>

<span data-ttu-id="360d2-196">**Žádost o pole:**</span><span class="sxs-lookup"><span data-stu-id="360d2-196">**Request for Fields:**</span></span>

```
    armclient get /subscriptions/{SubId}/resourceGroups/{ResourceGroupId}/providers/Microsoft.OperationalInsights/workspaces/{WorkspaceName}/schema?api-version=2015-03-20
```

<span data-ttu-id="360d2-197">**Odpověď:**</span><span class="sxs-lookup"><span data-stu-id="360d2-197">**Response:**</span></span>

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

<span data-ttu-id="360d2-198">Hello následující tabulka popisuje hello vlastnosti, které jsou k dispozici.</span><span class="sxs-lookup"><span data-stu-id="360d2-198">hello following table describes hello properties that are available.</span></span>

| <span data-ttu-id="360d2-199">**Vlastnost**</span><span class="sxs-lookup"><span data-stu-id="360d2-199">**Property**</span></span> | <span data-ttu-id="360d2-200">**Popis**</span><span class="sxs-lookup"><span data-stu-id="360d2-200">**Description**</span></span> |
| --- | --- |
| <span data-ttu-id="360d2-201">jméno</span><span class="sxs-lookup"><span data-stu-id="360d2-201">name</span></span> |<span data-ttu-id="360d2-202">Název pole.</span><span class="sxs-lookup"><span data-stu-id="360d2-202">Field name.</span></span> |
| <span data-ttu-id="360d2-203">displayName</span><span class="sxs-lookup"><span data-stu-id="360d2-203">displayName</span></span> |<span data-ttu-id="360d2-204">Hello zobrazovaný název pole hello.</span><span class="sxs-lookup"><span data-stu-id="360d2-204">hello display name of hello field.</span></span> |
| <span data-ttu-id="360d2-205">type</span><span class="sxs-lookup"><span data-stu-id="360d2-205">type</span></span> |<span data-ttu-id="360d2-206">Typ hodnoty pole hello Hello.</span><span class="sxs-lookup"><span data-stu-id="360d2-206">hello Type of hello field value.</span></span> |
| <span data-ttu-id="360d2-207">facetable (kategorizovatelné)</span><span class="sxs-lookup"><span data-stu-id="360d2-207">facetable</span></span> |<span data-ttu-id="360d2-208">Kombinace aktuální 'indexované', ' uložené ' a 'omezující vlastnost' vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="360d2-208">Combination of current ‘indexed’, ‘stored ‘and ‘facet’ properties.</span></span> |
| <span data-ttu-id="360d2-209">Zobrazení</span><span class="sxs-lookup"><span data-stu-id="360d2-209">display</span></span> |<span data-ttu-id="360d2-210">Aktuální vlastnost 'display'.</span><span class="sxs-lookup"><span data-stu-id="360d2-210">Current ‘display’ property.</span></span> <span data-ttu-id="360d2-211">Hodnota TRUE, pokud pole je viditelné ve vyhledávání.</span><span class="sxs-lookup"><span data-stu-id="360d2-211">True if field is visible in search.</span></span> |
| <span data-ttu-id="360d2-212">ownerType</span><span class="sxs-lookup"><span data-stu-id="360d2-212">ownerType</span></span> |<span data-ttu-id="360d2-213">Typy snížené tooonly patřící tooonboarded IP.</span><span class="sxs-lookup"><span data-stu-id="360d2-213">Reduced tooonly Types belonging tooonboarded IP’s.</span></span> |

## <a name="optional-parameters"></a><span data-ttu-id="360d2-214">Volitelné parametry</span><span class="sxs-lookup"><span data-stu-id="360d2-214">Optional parameters</span></span>
<span data-ttu-id="360d2-215">Následující informace Hello popisuje volitelné parametry, které jsou k dispozici.</span><span class="sxs-lookup"><span data-stu-id="360d2-215">hello following information describes optional parameters available.</span></span>

### <a name="highlighting"></a><span data-ttu-id="360d2-216">Zvýraznění</span><span class="sxs-lookup"><span data-stu-id="360d2-216">Highlighting</span></span>
<span data-ttu-id="360d2-217">Hello "Highlight" parametr je volitelný parametr můžete použít toorequest hello vyhledávání subsystému obsahují sadu značek v odpovědi.</span><span class="sxs-lookup"><span data-stu-id="360d2-217">hello “Highlight” parameter is an optional parameter you may use toorequest hello search subsystem include a set of markers in its response.</span></span>

<span data-ttu-id="360d2-218">Tyto značky upozornění hello zahájení a ukončení zvýrazněný text, který odpovídá hello podmínky uvedené v vyhledávací dotaz.</span><span class="sxs-lookup"><span data-stu-id="360d2-218">These markers indicate hello start and end highlighted text that matches hello terms provided in your search query.</span></span>
<span data-ttu-id="360d2-219">Může zadat hello počáteční a koncovou značek, které jsou používány hledaný termín hello zvýrazněná toowrap.</span><span class="sxs-lookup"><span data-stu-id="360d2-219">You may specify hello start and end markers that are used by search toowrap hello highlighted term.</span></span>

<span data-ttu-id="360d2-220">**Příklad vyhledávací dotaz.**</span><span class="sxs-lookup"><span data-stu-id="360d2-220">**Example search query**</span></span>

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

<span data-ttu-id="360d2-221">**Ukázkové výsledky:**</span><span class="sxs-lookup"><span data-stu-id="360d2-221">**Sample result:**</span></span>

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

<span data-ttu-id="360d2-222">Všimněte si, že hello předchozí výsledek obsahuje chybovou zprávu, která má předponu a připojí.</span><span class="sxs-lookup"><span data-stu-id="360d2-222">Notice that hello preceding result contains an error message that has been prefixed and appended.</span></span>

## <a name="computer-groups"></a><span data-ttu-id="360d2-223">Skupiny počítačů</span><span class="sxs-lookup"><span data-stu-id="360d2-223">Computer groups</span></span>
<span data-ttu-id="360d2-224">Skupiny počítačů jsou speciální uložená hledání, které vrátí sadu počítačů.</span><span class="sxs-lookup"><span data-stu-id="360d2-224">Computer groups are special saved searches that return a set of computers.</span></span>  <span data-ttu-id="360d2-225">Můžete vytvořit skupinu počítačů v jiných dotazy toolimit hello výsledky toohello počítačů ve skupině hello.</span><span class="sxs-lookup"><span data-stu-id="360d2-225">You can use a computer group in other queries toolimit hello results toohello computers in hello group.</span></span>  <span data-ttu-id="360d2-226">Skupina počítačů je implementovaný jako uložené hledání se skupiny značku s hodnotou počítače.</span><span class="sxs-lookup"><span data-stu-id="360d2-226">A computer group is implemented as a saved search with a Group tag with a value of Computer.</span></span>

<span data-ttu-id="360d2-227">Následuje ukázková odpověď pro skupinu počítačů.</span><span class="sxs-lookup"><span data-stu-id="360d2-227">Following is a sample response for a computer group.</span></span>

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

### <a name="retrieving-computer-groups"></a><span data-ttu-id="360d2-228">Načítání skupin počítačů</span><span class="sxs-lookup"><span data-stu-id="360d2-228">Retrieving computer groups</span></span>
<span data-ttu-id="360d2-229">ID tooretrieve skupinu počítačů použijte hello metodu Get s hello skupiny.</span><span class="sxs-lookup"><span data-stu-id="360d2-229">tooretrieve a computer group, use hello Get method with hello group ID.</span></span>

```
armclient get /subscriptions/{Subscription ID}/resourceGroups/{Resource Group Name}/providers/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/{Group ID}`?api-version=2015-03-20
```

### <a name="creating-or-updating-a-computer-group"></a><span data-ttu-id="360d2-230">Vytváření nebo aktualizaci skupinu počítačů</span><span class="sxs-lookup"><span data-stu-id="360d2-230">Creating or updating a computer group</span></span>
<span data-ttu-id="360d2-231">toocreate skupinu počítačů použijte hello metodu Put s ID jedinečný uloženého hledání.</span><span class="sxs-lookup"><span data-stu-id="360d2-231">toocreate a computer group, use hello Put method with a unique saved search ID.</span></span> <span data-ttu-id="360d2-232">Pokud chcete použít existující skupinu ID počítače, než je upravit.</span><span class="sxs-lookup"><span data-stu-id="360d2-232">If you use an existing computer group ID, then that one is modified.</span></span> <span data-ttu-id="360d2-233">Když vytvoříte skupinu počítačů v portálu Analýza protokolů hello, hello ID je vytvořeno ze skupiny hello a název.</span><span class="sxs-lookup"><span data-stu-id="360d2-233">When you create a computer group in hello Log Analytics portal, then hello ID is created from hello group and name.</span></span>

<span data-ttu-id="360d2-234">Hello dotaz byl použit pro definice skupiny hello musí vrátit sadu počítačů pro hello skupiny toofunction správně.</span><span class="sxs-lookup"><span data-stu-id="360d2-234">hello query used for hello group definition must return a set of computers for hello group toofunction properly.</span></span>  <span data-ttu-id="360d2-235">Doporučuje se, že vám stát, že váš dotaz s `| Distinct Computer` tooensure hello správná data jsou vrácena.</span><span class="sxs-lookup"><span data-stu-id="360d2-235">It's recommended that you end your query with `| Distinct Computer` tooensure hello correct data is returned.</span></span>

<span data-ttu-id="360d2-236">definice Hello hello uložené hledání musí obsahovat značku s hodnotou počítače pro toobe vyhledávání hello jsou klasifikovány jako skupinu počítačů s názvem skupiny.</span><span class="sxs-lookup"><span data-stu-id="360d2-236">hello definition of hello saved search must include a tag named Group with a value of Computer for hello search toobe classified as a computer group.</span></span>

```
    $etag=Get-Date -Format yyyy-MM-ddThh:mm:ss.msZ
    $groupName="My Computer Group"
    $groupQuery = "Computer=srv* | Distinct Computer"
    $groupCategory="My Computer Groups"
    $groupID = "My Computer Groups | My Computer Group"

    $groupJson = "{'etag': 'W/`"datetime\'" + $etag + "\'`"', 'properties': { 'Category': '" + $groupCategory + "', 'DisplayName':'"  + $groupName + "', 'Query':'" + $groupQuery + "', 'Tags': [{'Name': 'Group', 'Value': 'Computer'}], 'Version':'1'  }"

    armclient put /subscriptions/{Subscription ID}/resourceGroups/{Resource Group Name}/providers/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/$groupId`?api-version=2015-03-20 $groupJson
```

### <a name="deleting-computer-groups"></a><span data-ttu-id="360d2-237">Odstranění skupiny počítačů</span><span class="sxs-lookup"><span data-stu-id="360d2-237">Deleting computer groups</span></span>
<span data-ttu-id="360d2-238">ID toodelete skupinu počítačů použijte hello metodu Delete s hello skupiny.</span><span class="sxs-lookup"><span data-stu-id="360d2-238">toodelete a computer group, use hello Delete method with hello group ID.</span></span>

```
armclient delete /subscriptions/{Subscription ID}/resourceGroups/{Resource Group Name}/providers/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/$groupId`?api-version=2015-03-20
```


## <a name="next-steps"></a><span data-ttu-id="360d2-239">Další kroky</span><span class="sxs-lookup"><span data-stu-id="360d2-239">Next steps</span></span>
* <span data-ttu-id="360d2-240">Další informace o [protokolu hledání](log-analytics-log-searches.md) toobuild dotazy pomocí vlastních polí pro kritéria.</span><span class="sxs-lookup"><span data-stu-id="360d2-240">Learn about [log searches](log-analytics-log-searches.md) toobuild queries using custom fields for criteria.</span></span>
