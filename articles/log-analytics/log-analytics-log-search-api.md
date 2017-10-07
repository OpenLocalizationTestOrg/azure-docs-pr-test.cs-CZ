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
# <a name="log-analytics-log-search-rest-api"></a>Hledání protokolu analýzy protokolů REST API
Tato příručka obsahuje základní kurz, včetně příkladů, jak lze použít hello Log Analytics vyhledávání REST API. Analýzy protokolů je součástí hello Operations Management Suite (OMS).

> [!NOTE]
> Pokud pracovní prostor byl upgradovaný toohello [nové analýzy protokolů dotazu jazyka](log-analytics-log-search-upgrade.md), pak by měly pokračovat toouse hello starší verze dotazovací jazyk s hledání protokolů hello rozhraní API, jak je popsáno v tomto článku.  Nové rozhraní API, budou vydané pro upgradovaný pracovních prostorů a dokumentaci hello budou aktualizovány v daném čase. 

> [!NOTE]
> Analýzy protokolů volala dřív Operational Insights, proto je hello název používaný v hello poskytovatele prostředků.
>
>

## <a name="overview-of-hello-log-search-rest-api"></a>Přehled hello protokolu vyhledávání REST API
Hello Log Analytics vyhledávání REST API je dosáhl standardu RESTful a je přístupný prostřednictvím hello rozhraní API Správce prostředků Azure. Tento článek obsahuje příklady přístup k API hello prostřednictvím [ARMClient](https://github.com/projectkudu/ARMClient), nástroje příkazového řádku s otevřeným zdrojem, který zjednodušuje vyvolání hello rozhraní API Správce prostředků Azure. použití Hello ARMClient je jedním z mnoha možností tooaccess hello Log Analytics vyhledávání API. Další možností je modul Azure PowerShell hello toouse pro OperationalInsights, který obsahuje rutiny pro přístup k vyhledávání. Pomocí těchto nástrojů můžete využívat pracovní prostory hello rozhraní API služby Azure Resource Manager toomake volání tooOMS a provádět příkazy vyhledávání v nich. Hello API výstupy výsledky hledání ve formátu JSON, což vám výsledky hledání hello toouse mnoha různými způsoby prostřednictvím kódu programu.

Hello Azure Resource Manager je možné prostřednictvím [knihovna pro .NET](https://msdn.microsoft.com/library/azure/dn910477.aspx) a hello [REST API](https://msdn.microsoft.com/library/azure/mt163658.aspx). toolearn víc, přečtěte si hello propojené webové stránky.

> [!NOTE]
> Pokud například použijete příkaz agregace `|measure count()` nebo `distinct`, každé volání toosearch může vrátit maximálně 500 000 záznamů. Hledání, které neobsahují příkaz agregace vrátit maximálně 5 000 záznamů.
>
>

## <a name="basic-log-analytics-search-rest-api-tutorial"></a>Základní kurz Log Analytics vyhledávání REST API
### <a name="toouse-armclient"></a>toouse ARMClient
1. Nainstalujte [Chocolatey](https://chocolatey.org/), což je otevřete Správce balíčků zdroje pro systém Windows. Otevřete okno příkazového řádku jako správce a spusťte následující příkaz hello:

    ```
    @powershell -NoProfile -ExecutionPolicy unrestricted -Command "iex ((new-object net.webclient).DownloadString('https://chocolatey.org/install.ps1'))" && SET PATH=%PATH%;%ALLUSERSPROFILE%\chocolatey\bin
    ```
2. Instalace ARMClient spuštěním hello následující příkaz:

    ```
    choco install armclient
    ```

### <a name="tooperform-a-search-using-armclient"></a>tooperform a vyhledávání pomocí ARMClient
1. Přihlaste se pomocí účtu Microsoft nebo pracovní nebo školní účet:

    ```
    armclient login
    ```

    Úspěšné přihlášení uvádí všechny odběry svázané toohello zadaný účet:

    ```
    PS C:\Users\SampleUserName> armclient login
    Welcome YourEmail@ORG.com (Tenant: zzzzzzzz-zzzz-zzzz-zzzz-zzzzzzzzzzzz)
    User: YourEmail@ORG.com, Tenant: zzzzzzzz-zzzz-zzzz-zzzz-zzzzzzzzzzzz (Example org)
    There are 3 subscriptions
    Subscription xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx (Example Name 1)
    Subscription xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx (Example Name 2)
    Subscription xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx (Example Name 3)
    ```
2. Získáte pracovní prostory hello Operations Management Suite:

    ```
    armclient get /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces?api-version=2015-03-20
    ```

    Úspěšné volání Get by výstup, že všechny pracovní prostory svázané toohello předplatného:

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
3. Vytvořte vaše proměnná vyhledávání:

    ```
    $mySearch = "{ 'top':150, 'query':'Error'}";
    ```
4. Vyhledávání pomocí vaší novou proměnnou vyhledávání:

    ```
    armclient post /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{WORKSPACE NAME}/search?api-version=2015-03-20 $mySearch
    ```

## <a name="log-analytics-search-rest-api-reference-examples"></a>Příklady referenční dokumentace rozhraní API REST Analytics vyhledávání protokolu
Hello následující příklady ukazují, jak je možné používat hello rozhraní API pro hledání.

### <a name="search---actionread"></a>Hledání - akce/čtení
**Ukázka adresa Url:**

```
    /subscriptions/{SubscriptionId}/resourceGroups/{ResourceGroupId}/providers/Microsoft.OperationalInsights/workspaces/{WorkspaceName}/search?api-version=2015-03-20
```

**Žádost:**

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
Hello následující tabulka popisuje hello vlastnosti, které jsou k dispozici.

| **Vlastnost** | **Popis** |
| --- | --- |
| Horní |Hello maximální počet výsledků tooreturn. |
| Zvýrazněte |Obsahuje předběžné a post parametry, obvykle použité pro zvýraznění odpovídající pole |
| Před |Předpony hello zadaný řetězec tooyour shodná pole. |
| POST |Připojí zadaný řetězec tooyour shodná pole hello. |
| query |vyhledávací dotaz Hello používá toocollect a vrátí výsledky. |
| start |Hello začátek časového okna hello chcete výsledky toobe nalezených v. |
| End |Hello konec časového okna hello chcete výsledky toobe nalezených v. |

**Odpověď:**

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

### <a name="searchid---actionread"></a>Hledání / {ID} - akce/čtení
**Žádost o obsah hello uložené výsledky hledání:**

```
    armclient post /subscriptions/{SubId}/resourceGroups/{ResourceGroupId}/providers/Microsoft.OperationalInsights/workspaces/{WorkspaceName}/search/{SearchId}?api-version=2015-03-20
```

> [!NOTE]
> Pokud hello vyhledávání vrací se stavem 'Čekající', dotazování hello aktualizovat výsledky můžete to provést prostřednictvím tohoto rozhraní API. Po 6 min hello výsledky hledání hello budou odstraněna z mezipaměti hello a zmizel HTTP bude vrácen. Pokud požadavek počáteční hledání hello vrátí 'úspěšné, stav okamžitě, výsledky hello nebyly přidány toohello mezipaměti způsobuje toto rozhraní API tooreturn zmizel HTTP, pokud dotaz. obsah Hello výsledek HTTP 200 jsou hello stejný formát jako požadavku počáteční hledání hello právě s aktualizovanými hodnotami.
>
>

### <a name="saved-searches"></a>Uložená hledání
**Žádosti o seznam uložených hledání:**

```
    armclient post /subscriptions/{SubId}/resourceGroups/{ResourceGroupId}/providers/Microsoft.OperationalInsights/workspaces/{WorkspaceName}/savedSearches?api-version=2015-03-20
```

Podporované metody: GET, PUT odstranit

Podporované metody kolekce: získání

Hello následující tabulka popisuje hello vlastnosti, které jsou k dispozici.

| Vlastnost | Popis |
| --- | --- |
| ID |Jedinečný identifikátor Hello. |
| Značka Etag |**Vyžaduje se pro opravu**. Server aktualizaci na každý zápis. Hodnota musí být rovna toohello aktuální uložené hodnoty nebo ' *' tooupdate. 409 vrátil hodnoty starý nebo neplatná. |
| Properties.Query |**Požadované**. Hello vyhledávací dotaz. |
| properties.displayName |**Požadované**. uživatelem definované zobrazovaný název Hello hello dotazu. |
| Properties.category |**Požadované**. Hello uživatelem definované kategorie hello dotazu. |

> [!NOTE]
> Hello rozhraní API pro vyhledávání analýzy protokolů aktuálně vrací hodnotu vytvořené uživatelem uložená hledání při dotazování pro uložená hledání v pracovním prostoru. Hello API nevrací uložená hledání poskytované řešení.
>
>

### <a name="create-saved-searches"></a>Vytvoření uložených hledání
**Žádost:**

```
    $savedSearchParametersJson = "{'properties': { 'Category': 'myCategory', 'DisplayName':'myDisplayName', 'Query':'* | measure Count() by Source', 'Version':'1'  }"
    armclient put /subscriptions/{Subscription ID}/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{Workspace ID}/savedSearches/thisismyid?api-version=2015-03-20 $savedSearchParametersJson
```

> [!NOTE]
> Název Hello všechna uložená hledání, plány a akce, které jsou vytvořené pomocí hello Log Analytics API musí být malými písmeny.

### <a name="delete-saved-searches"></a>Odstraňte uložená hledání
**Žádost:**

```
    armclient delete /subscriptions/{Subscription ID}/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{Workspace ID}/savedSearches/thisIsMyId?api-version=2015-03-20
```

### <a name="update-saved-searches"></a>Aktualizace uložených hledání
 **Žádost:**

```
    $savedSearchParametersJson = "{'etag': 'W/`"datetime\'2015-04-16T23%3A35%3A35.3182423Z\'`"', 'properties': { 'Category': 'myCategory', 'DisplayName':'myDisplayName', 'Query':'* | measure Count() by Source', 'Version':'1'  }"
    armclient put /subscriptions/{Subscription ID}/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{Workspace ID}/savedSearches/thisIsMyId?api-version=2015-03-20 $savedSearchParametersJson
```

### <a name="metadata---json-only"></a>Metadata – pouze JSON
Zde je způsob toosee hello pole pro všechny typy protokolu pro hello data shromážděná v pracovním prostoru. Například pokud chcete, že víte, že pokud hello typ události obsahuje pole s názvem počítače, pak tento dotaz je jedním ze způsobů toocheck.

**Žádost o pole:**

```
    armclient get /subscriptions/{SubId}/resourceGroups/{ResourceGroupId}/providers/Microsoft.OperationalInsights/workspaces/{WorkspaceName}/schema?api-version=2015-03-20
```

**Odpověď:**

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

Hello následující tabulka popisuje hello vlastnosti, které jsou k dispozici.

| **Vlastnost** | **Popis** |
| --- | --- |
| jméno |Název pole. |
| displayName |Hello zobrazovaný název pole hello. |
| type |Typ hodnoty pole hello Hello. |
| facetable (kategorizovatelné) |Kombinace aktuální 'indexované', ' uložené ' a 'omezující vlastnost' vlastnosti. |
| Zobrazení |Aktuální vlastnost 'display'. Hodnota TRUE, pokud pole je viditelné ve vyhledávání. |
| ownerType |Typy snížené tooonly patřící tooonboarded IP. |

## <a name="optional-parameters"></a>Volitelné parametry
Následující informace Hello popisuje volitelné parametry, které jsou k dispozici.

### <a name="highlighting"></a>Zvýraznění
Hello "Highlight" parametr je volitelný parametr můžete použít toorequest hello vyhledávání subsystému obsahují sadu značek v odpovědi.

Tyto značky upozornění hello zahájení a ukončení zvýrazněný text, který odpovídá hello podmínky uvedené v vyhledávací dotaz.
Může zadat hello počáteční a koncovou značek, které jsou používány hledaný termín hello zvýrazněná toowrap.

**Příklad vyhledávací dotaz.**

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

**Ukázkové výsledky:**

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

Všimněte si, že hello předchozí výsledek obsahuje chybovou zprávu, která má předponu a připojí.

## <a name="computer-groups"></a>Skupiny počítačů
Skupiny počítačů jsou speciální uložená hledání, které vrátí sadu počítačů.  Můžete vytvořit skupinu počítačů v jiných dotazy toolimit hello výsledky toohello počítačů ve skupině hello.  Skupina počítačů je implementovaný jako uložené hledání se skupiny značku s hodnotou počítače.

Následuje ukázková odpověď pro skupinu počítačů.

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

### <a name="retrieving-computer-groups"></a>Načítání skupin počítačů
ID tooretrieve skupinu počítačů použijte hello metodu Get s hello skupiny.

```
armclient get /subscriptions/{Subscription ID}/resourceGroups/{Resource Group Name}/providers/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/{Group ID}`?api-version=2015-03-20
```

### <a name="creating-or-updating-a-computer-group"></a>Vytváření nebo aktualizaci skupinu počítačů
toocreate skupinu počítačů použijte hello metodu Put s ID jedinečný uloženého hledání. Pokud chcete použít existující skupinu ID počítače, než je upravit. Když vytvoříte skupinu počítačů v portálu Analýza protokolů hello, hello ID je vytvořeno ze skupiny hello a název.

Hello dotaz byl použit pro definice skupiny hello musí vrátit sadu počítačů pro hello skupiny toofunction správně.  Doporučuje se, že vám stát, že váš dotaz s `| Distinct Computer` tooensure hello správná data jsou vrácena.

definice Hello hello uložené hledání musí obsahovat značku s hodnotou počítače pro toobe vyhledávání hello jsou klasifikovány jako skupinu počítačů s názvem skupiny.

```
    $etag=Get-Date -Format yyyy-MM-ddThh:mm:ss.msZ
    $groupName="My Computer Group"
    $groupQuery = "Computer=srv* | Distinct Computer"
    $groupCategory="My Computer Groups"
    $groupID = "My Computer Groups | My Computer Group"

    $groupJson = "{'etag': 'W/`"datetime\'" + $etag + "\'`"', 'properties': { 'Category': '" + $groupCategory + "', 'DisplayName':'"  + $groupName + "', 'Query':'" + $groupQuery + "', 'Tags': [{'Name': 'Group', 'Value': 'Computer'}], 'Version':'1'  }"

    armclient put /subscriptions/{Subscription ID}/resourceGroups/{Resource Group Name}/providers/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/$groupId`?api-version=2015-03-20 $groupJson
```

### <a name="deleting-computer-groups"></a>Odstranění skupiny počítačů
ID toodelete skupinu počítačů použijte hello metodu Delete s hello skupiny.

```
armclient delete /subscriptions/{Subscription ID}/resourceGroups/{Resource Group Name}/providers/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/$groupId`?api-version=2015-03-20
```


## <a name="next-steps"></a>Další kroky
* Další informace o [protokolu hledání](log-analytics-log-searches.md) toobuild dotazy pomocí vlastních polí pro kritéria.
