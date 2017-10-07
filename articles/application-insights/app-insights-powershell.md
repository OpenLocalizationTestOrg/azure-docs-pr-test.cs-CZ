---
title: "aaaAutomate Azure Application Insights v prostředí PowerShell | Microsoft Docs"
description: "Automatizovat vytváření prostředků, výstrahy a dostupnost testů v prostředí PowerShell pomocí šablony Azure Resource Manager."
services: application-insights
documentationcenter: 
author: CFreemanwa
manager: carmonm
ms.assetid: 9f73b87f-be63-4847-88c8-368543acad8b
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 04/02/2017
ms.author: bwren
ms.openlocfilehash: ebd336eafba58a690a0e8ffbd1c74f7e93dbb682
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
#  <a name="create-application-insights-resources-using-powershell"></a>Vytváření prostředků Application Insights v prostředí PowerShell
Tento článek ukazuje, jak tooautomate hello vytváření a aktualizace [Application Insights](app-insights-overview.md) prostředky automaticky pomocí nástroje Správa prostředků Azure. Může například uděláte jako součást procesu sestavení. Společně s hello základní prostředek Application Insights, můžete vytvořit [testy dostupnosti webu](app-insights-monitor-web-app-availability.md), nastavte [výstrahy](app-insights-alerts.md), nastavte hello [ceny schéma](app-insights-pricing.md)a vytvořte další Azure prostředky.

Hello klíče toocreating tyto prostředky se šablony JSON pro [Azure Resource Manager](../azure-resource-manager/powershell-azure-resource-manager.md). Stručně řečeno, postup hello je: stažení hello JSON definice prostředků existující; Parametrizace určité hodnoty, například názvy; a spusťte šablonu hello vždy, když chcete toocreate nový prostředek. Můžete balíček několik prostředků společně, toocreate, které je vše v jednom přejděte – například monitorování aplikace s testy dostupnosti, výstrahy a úložiště pro nepřetržitou export. Existují některé odlišnosti toosome z hello parameterizations, které vám objasníme sem.

## <a name="one-time-setup"></a>Jednorázové instalace
Pokud jste nepoužili prostředí PowerShell s předplatným Azure před:

Instalace modulu Azure Powershell hello na hello počítači, kde se má toorun hello skripty:

1. Nainstalujte [instalačního programu webové platformy (verze 5 nebo novější)](http://www.microsoft.com/web/downloads/platform.aspx).
2. Použijte tooinstall Microsoft Azure Powershell.

## <a name="create-an-azure-resource-manager-template"></a>Vytvořit šablonu Azure Resource Manager
Vytvořte nový soubor .json – umožňuje volání `template1.json` v tomto příkladu. Zkopírujte do ní tento obsah:

```JSON
    {
        "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
        "contentVersion": "1.0.0.0",
        "parameters": {
            "appName": {
                "type": "string",
                "metadata": {
                    "description": "Enter hello application name."
                }
            },
            "appType": {
                "type": "string",
                "defaultValue": "web",
                "allowedValues": [
                    "web",
                    "java",
                    "HockeyAppBridge",
                    "other"
                ],
                "metadata": {
                    "description": "Enter hello application type."
                }
            },
            "appLocation": {
                "type": "string",
                "defaultValue": "East US",
                "allowedValues": [
                    "South Central US",
                    "West Europe",
                    "East US",
                    "North Europe"
                ],
                "metadata": {
                    "description": "Enter hello application location."
                }
            },
            "priceCode": {
                "type": "int",
                "defaultValue": 1,
                "allowedValues": [
                    1,
                    2
                ],
                "metadata": {
                    "description": "1 = Basic, 2 = Enterprise"
                }
            },
            "dailyQuota": {
                "type": "int",
                "defaultValue": 100,
                "minValue": 1,
                "metadata": {
                    "description": "Enter daily quota in GB."
                }
            },
            "dailyQuotaResetTime": {
                "type": "int",
                "defaultValue": 24,
                "metadata": {
                    "description": "Enter daily quota reset hour in UTC (0 too23). Values outside hello range will get a random reset hour."
                }
            },
            "warningThreshold": {
                "type": "int",
                "defaultValue": 90,
                "minValue": 1,
                "maxValue": 100,
                "metadata": {
                    "description": "Enter hello % value of daily quota after which warning mail toobe sent. "
                }
            }
        },
        "variables": {
            "priceArray": [
                "Basic",
                "Application Insights Enterprise"
            ],
            "pricePlan": "[take(variables('priceArray'),parameters('priceCode'))]",
            "billingplan": "[concat(parameters('appName'),'/', variables('pricePlan')[0])]"
        },
        "resources": [
            {
                "type": "microsoft.insights/components",
                "kind": "[parameters('appType')]",
                "name": "[parameters('appName')]",
                "apiVersion": "2014-04-01",
                "location": "[parameters('appLocation')]",
                "tags": {},
                "properties": {
                    "ApplicationId": "[parameters('appName')]"
                },
                "dependsOn": []
            },
            {
                "name": "[variables('billingplan')]",
                "type": "microsoft.insights/components/CurrentBillingFeatures",
                "location": "[parameters('appLocation')]",
                "apiVersion": "2015-05-01",
                "dependsOn": [
                    "[resourceId('microsoft.insights/components', parameters('appName'))]"
                ],
                "properties": {
                    "CurrentBillingFeatures": "[variables('pricePlan')]",
                    "DataVolumeCap": {
                        "Cap": "[parameters('dailyQuota')]",
                        "WarningThreshold": "[parameters('warningThreshold')]",
                        "ResetTime": "[parameters('dailyQuotaResetTime')]"
                    }
                }
            }
        ]
    }
```



## <a name="create-application-insights-resources"></a>Vytvořit prostředky Application Insights
1. V prostředí PowerShell Přihlaste se tooAzure:
   
    `Login-AzureRmAccount`
2. Spusťte příkaz takto:
   
    ```PS
   
        New-AzureRmResourceGroupDeployment -ResourceGroupName Fabrikam `
               -TemplateFile .\template1.json `
               -appName myNewApp

    ``` 
   
   * `-ResourceGroupName`je skupina hello, kam chcete toocreate hello nové prostředky.
   * `-TemplateFile`musí nastat před hello vlastní parametry.
   * `-appName`Název Hello toocreate prostředků hello.

Můžete přidat další parametry – jejich popisy najdete v části Parametry hello hello šablony.

## <a name="tooget-hello-instrumentation-key"></a>klíč instrumentace tooget hello
Po vytvoření prostředek aplikace, budete muset klíč instrumentace hello: 

```PS
    $resource = Find-AzureRmResource -ResourceNameEquals "<YOUR APP NAME>" -ResourceType "Microsoft.Insights/components"
    $details = Get-AzureRmResource -ResourceId $resource.ResourceId
    $ikey = $details.Properties.InstrumentationKey
```


<a id="price"></a>
## <a name="set-hello-price-plan"></a>Sada hello cena plánu

Můžete nastavit hello [cena plán](app-insights-pricing.md).

toocreate prostředek aplikace hello Enterprise cena plán, pomocí šablony hello výše:

```PS
        New-AzureRmResourceGroupDeployment -ResourceGroupName Fabrikam `
               -TemplateFile .\template1.json `
               -priceCode 2 `
               -appName myNewApp
```

|priceCode|Plánování|
|---|---|
|1|Basic|
|2|Enterprise|

* Pokud chcete pouze toouse hello výchozí základní ceny plán, můžete vynechat hello CurrentBillingFeatures prostředků z šablony hello.
* Pokud chcete plán cena hello toochange po vytvoření hello součásti prostředků, můžete šablonu, která vynechá hello "microsoft.insights/components" prostředků. Navíc vynechejte hello `dependsOn` uzlu z hello fakturace prostředků. 

tooverify hello aktualizovaná cena plánu, podívejte se na hello "Funkce + ceny" okna v prohlížeči hello. **Aktualizujte zobrazení prohlížeče hello** toomake, že vidíte nejnovější stav hello.



## <a name="add-a-metric-alert"></a>Přidání metriky oznámení

tooset si metriky výstraha v hello stejný čas jako prostředek vaší aplikace, kód sloučení takto do souboru šablony hello:

```JSON
{
    parameters: { ... // existing parameters ...
            "responseTime": {
              "type": "int",
              "defaultValue": 3,
              "minValue": 1,
              "metadata": {
                "description": "Enter response time threshold in seconds."
              }
    },
    variables: { ... // existing variables ...
      // Alert names must be unique within resource group.
      "responseAlertName": "[concat('ResponseTime-', toLower(parameters('appName')))]"
    }, 
    resources: { ... // existing resources ...
     {
      //
      // Metric alert on response time
      //
      "name": "[variables('responseAlertName')]",
      "type": "Microsoft.Insights/alertrules",
      "apiVersion": "2014-04-01",
      "location": "[parameters('appLocation')]",
      // Ensure this resource is created after hello app resource:
      "dependsOn": [
        "[resourceId('Microsoft.Insights/components', parameters('appName'))]"
      ],
      "tags": {
        "[concat('hidden-link:', resourceId('Microsoft.Insights/components', parameters('appName')))]": "Resource"
      },
      "properties": {
        "name": "[variables('responseAlertName')]",
        "description": "response time alert",
        "isEnabled": true,
        "condition": {
          "$type": "Microsoft.WindowsAzure.Management.Monitoring.Alerts.Models.ThresholdRuleCondition, Microsoft.WindowsAzure.Management.Mon.Client",
          "odata.type": "Microsoft.Azure.Management.Insights.Models.ThresholdRuleCondition",
          "dataSource": {
            "$type": "Microsoft.WindowsAzure.Management.Monitoring.Alerts.Models.RuleMetricDataSource, Microsoft.WindowsAzure.Management.Mon.Client",
            "odata.type": "Microsoft.Azure.Management.Insights.Models.RuleMetricDataSource",
            "resourceUri": "[resourceId('microsoft.insights/components', parameters('appName'))]",
            "metricName": "request.duration"
          },
          "threshold": "[parameters('responseTime')]", //seconds
          "windowSize": "PT15M" // Take action if changed state for 15 minutes
        },
        "actions": [
          {
            "$type": "Microsoft.WindowsAzure.Management.Monitoring.Alerts.Models.RuleEmailAction, Microsoft.WindowsAzure.Management.Mon.Client",
            "odata.type": "Microsoft.Azure.Management.Insights.Models.RuleEmailAction",
            "sendToServiceOwners": true,
            "customEmails": []
          }
        ]
      }
    }
}
```

Při vyvolání hello šablony, můžete přidat tento parametr:

    `-responseTime 2`

Samozřejmě můžete parametrizovat další pole. 

toofind názvy typů hello a podrobnosti o konfiguraci jiných výstrahy pravidel ručně vytvořit pravidlo a zkontrolujte ji v [Azure Resource Manager](https://resources.azure.com/). 


## <a name="add-an-availability-test"></a>Přidat test dostupnosti

V tomto příkladu je pro testování ping (tootest jediné stránce).  

**Existují dvě části** v testu dostupnosti: test hello, sám a hello výstraha, která vás informuje o selhání.

Slučte následující kód do souboru hello šablony, který vytvoří aplikace hello hello.

```JSON
{
    parameters: { ... // existing parameters here ...
      "pingURL": { "type": "string" },
      "pingText": { "type": "string" , defaultValue: ""}
    },
    variables: { ... // existing variables here ...
      "pingTestName":"[concat('PingTest-', toLower(parameters('appName')))]",
      "pingAlertRuleName": "[concat('PingAlert-', toLower(parameters('appName')), '-', subscription().subscriptionId)]"
    },
    resources: { ... // existing resources here ...
    { //
      // Availability test: part 1 configures hello test
      //
      "name": "[variables('pingTestName')]",
      "type": "Microsoft.Insights/webtests",
      "apiVersion": "2014-04-01",
      "location": "[parameters('appLocation')]",
      // Ensure this is created after hello app resource:
      "dependsOn": [
        "[resourceId('Microsoft.Insights/components', parameters('appName'))]"
      ],
      "tags": {
        "[concat('hidden-link:', resourceId('Microsoft.Insights/components', parameters('appName')))]": "Resource"
      },
      "properties": {
        "Name": "[variables('pingTestName')]",
        "Description": "Basic ping test",
        "Enabled": true,
        "Frequency": 900, // 15 minutes
        "Timeout": 120, // 2 minutes
        "Kind": "ping", // single URL test
        "RetryEnabled": true,
        "Locations": [
          {
            "Id": "us-va-ash-azr"
          },
          {
            "Id": "emea-nl-ams-azr"
          },
          {
            "Id": "apac-jp-kaw-edge"
          }
        ],
        "Configuration": {
          "WebTest": "[concat('<WebTest   Name=\"', variables('pingTestName'), '\"   Enabled=\"True\"         CssProjectStructure=\"\"    CssIteration=\"\"  Timeout=\"120\"  WorkItemIds=\"\"         xmlns=\"http://microsoft.com/schemas/VisualStudio/TeamTest/2010\"         Description=\"\"  CredentialUserName=\"\"  CredentialPassword=\"\"         PreAuthenticate=\"True\"  Proxy=\"default\"  StopOnError=\"False\"         RecordedResultFile=\"\"  ResultsLocale=\"\">  <Items>  <Request Method=\"GET\"    Version=\"1.1\"  Url=\"', parameters('Url'),   '\" ThinkTime=\"0\"  Timeout=\"300\" ParseDependentRequests=\"True\"         FollowRedirects=\"True\" RecordResult=\"True\" Cache=\"False\"         ResponseTimeGoal=\"0\"  Encoding=\"utf-8\"  ExpectedHttpStatusCode=\"200\"         ExpectedResponseUrl=\"\" ReportingName=\"\" IgnoreHttpStatusCode=\"False\" />        </Items>  <ValidationRules> <ValidationRule  Classname=\"Microsoft.VisualStudio.TestTools.WebTesting.Rules.ValidationRuleFindText, Microsoft.VisualStudio.QualityTools.WebTestFramework, Version=10.0.0.0, Culture=neutral, PublicKeyToken=b03f5f7f11d50a3a\" DisplayName=\"Find Text\"         Description=\"Verifies hello existence of hello specified text in hello response.\"         Level=\"High\"  ExectuionOrder=\"BeforeDependents\">  <RuleParameters>        <RuleParameter Name=\"FindText\" Value=\"',   parameters('pingText'), '\" />  <RuleParameter Name=\"IgnoreCase\" Value=\"False\" />  <RuleParameter Name=\"UseRegularExpression\" Value=\"False\" />  <RuleParameter Name=\"PassIfTextFound\" Value=\"True\" />  </RuleParameters> </ValidationRule>  </ValidationRules>  </WebTest>')]"
        },
        "SyntheticMonitorId": "[variables('pingTestName')]"
      }
    },

    {
      //
      // Availability test: part 2, hello alert rule
      //
      "name": "[variables('pingAlertRuleName')]",
      "type": "Microsoft.Insights/alertrules",
      "apiVersion": "2014-04-01",
      "location": "[parameters('appLocation')]", 
      "dependsOn": [
        "[resourceId('Microsoft.Insights/webtests', variables('pingTestName'))]"
      ],
      "tags": {
        "[concat('hidden-link:', resourceId('Microsoft.Insights/components', parameters('appName')))]": "Resource",
        "[concat('hidden-link:', resourceId('Microsoft.Insights/webtests', variables('pingTestName')))]": "Resource"
      },
      "properties": {
        "name": "[variables('pingAlertRuleName')]",
        "description": "alert for web test",
        "isEnabled": true,
        "condition": {
          "$type": "Microsoft.WindowsAzure.Management.Monitoring.Alerts.Models.LocationThresholdRuleCondition, Microsoft.WindowsAzure.Management.Mon.Client",
          "odata.type": "Microsoft.Azure.Management.Insights.Models.LocationThresholdRuleCondition",
          "dataSource": {
            "$type": "Microsoft.WindowsAzure.Management.Monitoring.Alerts.Models.RuleMetricDataSource, Microsoft.WindowsAzure.Management.Mon.Client",
            "odata.type": "Microsoft.Azure.Management.Insights.Models.RuleMetricDataSource",
            "resourceUri": "[resourceId('microsoft.insights/webtests', variables('pingTestName'))]",
            "metricName": "GSMT_AvRaW"
          },
          "windowSize": "PT15M", // Take action if changed state for 15 minutes
          "failedLocationCount": 2
        },
        "actions": [
          {
            "$type": "Microsoft.WindowsAzure.Management.Monitoring.Alerts.Models.RuleEmailAction, Microsoft.WindowsAzure.Management.Mon.Client",
            "odata.type": "Microsoft.Azure.Management.Insights.Models.RuleEmailAction",
            "sendToServiceOwners": true,
            "customEmails": []
          }
        ]
      }
    }
}
```

toodiscover hello kódy pro jiné umístění testu nebo tooautomate hello vytvoření složitější webové testy, vytvořte příklad ručně a pak Parametrizace hello kód z [Azure Resource Manager](https://resources.azure.com/).

## <a name="add-more-resources"></a>Přidat další prostředky

Vytvoření hello tooautomate jiný prostředek libovolného typu, například vytvořit ručně a pak zkopírujte a Parametrizace jeho kód z [Azure Resource Manager](https://resources.azure.com/). 

1. Otevřete [Azure Resource Manager](https://resources.azure.com/). Přejděte dolů prostřednictvím `subscriptions/resourceGroups/<your resource group>/providers/Microsoft.Insights/components`, tooyour prostředků aplikace. 
   
    ![Navigace v Průzkumníku prostředků Azure.](./media/app-insights-powershell/01.png)
   
    *Součásti* jsou základní prostředky Application Insights pro zobrazení aplikace hello. Existují samostatné prostředky hello přidružená pravidla výstrah a testy dostupnosti webu.
2. Kopírování hello JSON hello součásti na příslušné místo hello v `template1.json`.
3. Odstraňte tyto vlastnosti:
   
   * `id`
   * `InstrumentationKey`
   * `CreationDate`
   * `TenantId`
4. Otevřete hello webtests a alertrules části a zkopírujte hello JSON pro jednotlivé položky do šablony. (Není zkopírovat z uzlů webtests nebo alertrules hello: přejděte do položky hello pod nimi.)
   
    Každý test webu má přidružené pravidlo výstrahy, takže máte toocopy z nich.
   
    Můžete použít také výstrahy o metrikách. [Metriky názvy](app-insights-powershell-alerts.md#metric-names).
5. Vložte tento řádek v každého prostředku:
   
    `"apiVersion": "2015-05-01",`

### <a name="parameterize-hello-template"></a>Parametrizace hello šablony
Nyní máte tooreplace hello konkrétní názvy s parametry. příliš[Parametrizace šablonu](../azure-resource-manager/resource-group-authoring-templates.md), můžete psát pomocí výrazy [sadu pomocných funkcí](../azure-resource-manager/resource-group-template-functions.md). 

Nelze Parametrizace jenom část řetězce, takže použijte `concat()` toobuild řetězce.

Zde jsou příklady nahrazení hello, měli byste toomake. Existuje několik výskyty každé nahrazování. Ostatní bude pravděpodobně nutné ve vaší šabloně. Tyto příklady použití hello parametry a proměnné, které jsme definovali v horní části hello hello šablony.

| Najít | Nahraďte |
| --- | --- |
| `"hidden-link:/subscriptions/.../components/MyAppName"` |`"[concat('hidden-link:',`<br/>` resourceId('microsoft.insights/components',` <br/> ` parameters('appName')))]"` |
| `"/subscriptions/.../alertrules/myAlertName-myAppName-subsId",` |`"[resourceId('Microsoft.Insights/alertrules', variables('alertRuleName'))]",` |
| `"/subscriptions/.../webtests/myTestName-myAppName",` |`"[resourceId('Microsoft.Insights/webtests', parameters('webTestName'))]",` |
| `"myWebTest-myAppName"` |`"[variables(testName)]"'` |
| `"myTestName-myAppName-subsId"` |`"[variables('alertRuleName')]"` |
| `"myAppName"` |`"[parameters('appName')]"` |
| `"myappname"`(malá písmena) |`"[toLower(parameters('appName'))]"` |
| `"<WebTest Name=\"myWebTest\" ...`<br/>` Url=\"http://fabrikam.com/home\" ...>"` |`[concat('<WebTest Name=\"',` <br/> `parameters('webTestName'),` <br/> `'\" ... Url=\"', parameters('Url'),` <br/> `'\"...>')]"`<br/>Odstranit Guid a ID. |

### <a name="set-dependencies-between-hello-resources"></a>Nastavte závislosti mezi prostředky hello
Azure měli nastavit hello prostředky v striktní pořadí. toomake se, že jeden instalační program dokončí před hello vedle, přidejte závislosti řádky:

* V rámci dostupnosti hello testovací prostředků:
  
    `"dependsOn": ["[resourceId('Microsoft.Insights/components', parameters('appName'))]"],`
* V hello výstrahy prostředku pro test dostupnosti:
  
    `"dependsOn": ["[resourceId('Microsoft.Insights/webtests', variables('testName'))]"],`



## <a name="next-steps"></a>Další kroky
Další články automatizace:

* [Vytvořte prostředek Application Insights](app-insights-powershell-script-create-resource.md) -rychlý způsob bez použití šablony.
* [Nastavení výstrah](app-insights-powershell-alerts.md)
* [Tvorba webových testů](https://azure.microsoft.com/blog/creating-a-web-test-alert-programmatically-with-application-insights/)
* [Odesílání Azure Diagnostics tooApplication statistiky](app-insights-powershell-azure-diagnostics.md)
* [Nasazení tooAzure z Githubu](http://blogs.msdn.com/b/webdev/archive/2015/09/16/deploy-to-azure-from-github-with-application-insights.aspx)
* [Vytvořit poznámky k verzi](https://github.com/Microsoft/ApplicationInsights-Home/blob/master/API/CreateReleaseAnnotation.ps1)

