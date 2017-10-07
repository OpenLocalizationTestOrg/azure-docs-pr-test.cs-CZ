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
#  <a name="create-application-insights-resources-using-powershell"></a><span data-ttu-id="b86c3-103">Vytváření prostředků Application Insights v prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="b86c3-103">Create Application Insights resources using PowerShell</span></span>
<span data-ttu-id="b86c3-104">Tento článek ukazuje, jak tooautomate hello vytváření a aktualizace [Application Insights](app-insights-overview.md) prostředky automaticky pomocí nástroje Správa prostředků Azure.</span><span class="sxs-lookup"><span data-stu-id="b86c3-104">This article shows you how tooautomate hello creation and update of [Application Insights](app-insights-overview.md) resources automatically by using Azure Resource Management.</span></span> <span data-ttu-id="b86c3-105">Může například uděláte jako součást procesu sestavení.</span><span class="sxs-lookup"><span data-stu-id="b86c3-105">You might, for example, do so as part of a build process.</span></span> <span data-ttu-id="b86c3-106">Společně s hello základní prostředek Application Insights, můžete vytvořit [testy dostupnosti webu](app-insights-monitor-web-app-availability.md), nastavte [výstrahy](app-insights-alerts.md), nastavte hello [ceny schéma](app-insights-pricing.md)a vytvořte další Azure prostředky.</span><span class="sxs-lookup"><span data-stu-id="b86c3-106">Along with hello basic Application Insights resource, you can create [availability web tests](app-insights-monitor-web-app-availability.md), set up [alerts](app-insights-alerts.md), set hello [pricing scheme](app-insights-pricing.md), and create other Azure resources.</span></span>

<span data-ttu-id="b86c3-107">Hello klíče toocreating tyto prostředky se šablony JSON pro [Azure Resource Manager](../azure-resource-manager/powershell-azure-resource-manager.md).</span><span class="sxs-lookup"><span data-stu-id="b86c3-107">hello key toocreating these resources is JSON templates for [Azure Resource Manager](../azure-resource-manager/powershell-azure-resource-manager.md).</span></span> <span data-ttu-id="b86c3-108">Stručně řečeno, postup hello je: stažení hello JSON definice prostředků existující; Parametrizace určité hodnoty, například názvy; a spusťte šablonu hello vždy, když chcete toocreate nový prostředek.</span><span class="sxs-lookup"><span data-stu-id="b86c3-108">In a nutshell, hello procedure is: download hello JSON definitions of existing resources; parameterize certain values such as names; and then run hello template whenever you want toocreate a new resource.</span></span> <span data-ttu-id="b86c3-109">Můžete balíček několik prostředků společně, toocreate, které je vše v jednom přejděte – například monitorování aplikace s testy dostupnosti, výstrahy a úložiště pro nepřetržitou export.</span><span class="sxs-lookup"><span data-stu-id="b86c3-109">You can package several resources together, toocreate them all in one go - for example, an app monitor with availability tests, alerts, and storage for continuous export.</span></span> <span data-ttu-id="b86c3-110">Existují některé odlišnosti toosome z hello parameterizations, které vám objasníme sem.</span><span class="sxs-lookup"><span data-stu-id="b86c3-110">There are some subtleties toosome of hello parameterizations, which we'll explain here.</span></span>

## <a name="one-time-setup"></a><span data-ttu-id="b86c3-111">Jednorázové instalace</span><span class="sxs-lookup"><span data-stu-id="b86c3-111">One-time setup</span></span>
<span data-ttu-id="b86c3-112">Pokud jste nepoužili prostředí PowerShell s předplatným Azure před:</span><span class="sxs-lookup"><span data-stu-id="b86c3-112">If you haven't used PowerShell with your Azure subscription before:</span></span>

<span data-ttu-id="b86c3-113">Instalace modulu Azure Powershell hello na hello počítači, kde se má toorun hello skripty:</span><span class="sxs-lookup"><span data-stu-id="b86c3-113">Install hello Azure Powershell module on hello machine where you want toorun hello scripts:</span></span>

1. <span data-ttu-id="b86c3-114">Nainstalujte [instalačního programu webové platformy (verze 5 nebo novější)](http://www.microsoft.com/web/downloads/platform.aspx).</span><span class="sxs-lookup"><span data-stu-id="b86c3-114">Install [Microsoft Web Platform Installer (v5 or higher)](http://www.microsoft.com/web/downloads/platform.aspx).</span></span>
2. <span data-ttu-id="b86c3-115">Použijte tooinstall Microsoft Azure Powershell.</span><span class="sxs-lookup"><span data-stu-id="b86c3-115">Use it tooinstall Microsoft Azure Powershell.</span></span>

## <a name="create-an-azure-resource-manager-template"></a><span data-ttu-id="b86c3-116">Vytvořit šablonu Azure Resource Manager</span><span class="sxs-lookup"><span data-stu-id="b86c3-116">Create an Azure Resource Manager template</span></span>
<span data-ttu-id="b86c3-117">Vytvořte nový soubor .json – umožňuje volání `template1.json` v tomto příkladu.</span><span class="sxs-lookup"><span data-stu-id="b86c3-117">Create a new .json file - let's call it `template1.json` in this example.</span></span> <span data-ttu-id="b86c3-118">Zkopírujte do ní tento obsah:</span><span class="sxs-lookup"><span data-stu-id="b86c3-118">Copy this content into it:</span></span>

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



## <a name="create-application-insights-resources"></a><span data-ttu-id="b86c3-119">Vytvořit prostředky Application Insights</span><span class="sxs-lookup"><span data-stu-id="b86c3-119">Create Application Insights resources</span></span>
1. <span data-ttu-id="b86c3-120">V prostředí PowerShell Přihlaste se tooAzure:</span><span class="sxs-lookup"><span data-stu-id="b86c3-120">In PowerShell, sign in tooAzure:</span></span>
   
    `Login-AzureRmAccount`
2. <span data-ttu-id="b86c3-121">Spusťte příkaz takto:</span><span class="sxs-lookup"><span data-stu-id="b86c3-121">Run a command like this:</span></span>
   
    ```PS
   
        New-AzureRmResourceGroupDeployment -ResourceGroupName Fabrikam `
               -TemplateFile .\template1.json `
               -appName myNewApp

    ``` 
   
   * <span data-ttu-id="b86c3-122">`-ResourceGroupName`je skupina hello, kam chcete toocreate hello nové prostředky.</span><span class="sxs-lookup"><span data-stu-id="b86c3-122">`-ResourceGroupName` is hello group where you want toocreate hello new resources.</span></span>
   * <span data-ttu-id="b86c3-123">`-TemplateFile`musí nastat před hello vlastní parametry.</span><span class="sxs-lookup"><span data-stu-id="b86c3-123">`-TemplateFile` must occur before hello custom parameters.</span></span>
   * <span data-ttu-id="b86c3-124">`-appName`Název Hello toocreate prostředků hello.</span><span class="sxs-lookup"><span data-stu-id="b86c3-124">`-appName` hello name of hello resource toocreate.</span></span>

<span data-ttu-id="b86c3-125">Můžete přidat další parametry – jejich popisy najdete v části Parametry hello hello šablony.</span><span class="sxs-lookup"><span data-stu-id="b86c3-125">You can add other parameters - you'll find their descriptions in hello parameters section of hello template.</span></span>

## <a name="tooget-hello-instrumentation-key"></a><span data-ttu-id="b86c3-126">klíč instrumentace tooget hello</span><span class="sxs-lookup"><span data-stu-id="b86c3-126">tooget hello instrumentation key</span></span>
<span data-ttu-id="b86c3-127">Po vytvoření prostředek aplikace, budete muset klíč instrumentace hello:</span><span class="sxs-lookup"><span data-stu-id="b86c3-127">After creating an application resource, you'll want hello instrumentation key:</span></span> 

```PS
    $resource = Find-AzureRmResource -ResourceNameEquals "<YOUR APP NAME>" -ResourceType "Microsoft.Insights/components"
    $details = Get-AzureRmResource -ResourceId $resource.ResourceId
    $ikey = $details.Properties.InstrumentationKey
```


<a id="price"></a>
## <a name="set-hello-price-plan"></a><span data-ttu-id="b86c3-128">Sada hello cena plánu</span><span class="sxs-lookup"><span data-stu-id="b86c3-128">Set hello price plan</span></span>

<span data-ttu-id="b86c3-129">Můžete nastavit hello [cena plán](app-insights-pricing.md).</span><span class="sxs-lookup"><span data-stu-id="b86c3-129">You can set hello [price plan](app-insights-pricing.md).</span></span>

<span data-ttu-id="b86c3-130">toocreate prostředek aplikace hello Enterprise cena plán, pomocí šablony hello výše:</span><span class="sxs-lookup"><span data-stu-id="b86c3-130">toocreate an app resource with hello Enterprise price plan, using hello template above:</span></span>

```PS
        New-AzureRmResourceGroupDeployment -ResourceGroupName Fabrikam `
               -TemplateFile .\template1.json `
               -priceCode 2 `
               -appName myNewApp
```

|<span data-ttu-id="b86c3-131">priceCode</span><span class="sxs-lookup"><span data-stu-id="b86c3-131">priceCode</span></span>|<span data-ttu-id="b86c3-132">Plánování</span><span class="sxs-lookup"><span data-stu-id="b86c3-132">plan</span></span>|
|---|---|
|<span data-ttu-id="b86c3-133">1</span><span class="sxs-lookup"><span data-stu-id="b86c3-133">1</span></span>|<span data-ttu-id="b86c3-134">Basic</span><span class="sxs-lookup"><span data-stu-id="b86c3-134">Basic</span></span>|
|<span data-ttu-id="b86c3-135">2</span><span class="sxs-lookup"><span data-stu-id="b86c3-135">2</span></span>|<span data-ttu-id="b86c3-136">Enterprise</span><span class="sxs-lookup"><span data-stu-id="b86c3-136">Enterprise</span></span>|

* <span data-ttu-id="b86c3-137">Pokud chcete pouze toouse hello výchozí základní ceny plán, můžete vynechat hello CurrentBillingFeatures prostředků z šablony hello.</span><span class="sxs-lookup"><span data-stu-id="b86c3-137">If you only want toouse hello default Basic price plan, you can omit hello CurrentBillingFeatures resource from hello template.</span></span>
* <span data-ttu-id="b86c3-138">Pokud chcete plán cena hello toochange po vytvoření hello součásti prostředků, můžete šablonu, která vynechá hello "microsoft.insights/components" prostředků.</span><span class="sxs-lookup"><span data-stu-id="b86c3-138">If you want toochange hello price plan after hello component resource has been created, you can use a template that omits hello "microsoft.insights/components" resource.</span></span> <span data-ttu-id="b86c3-139">Navíc vynechejte hello `dependsOn` uzlu z hello fakturace prostředků.</span><span class="sxs-lookup"><span data-stu-id="b86c3-139">Also, omit hello `dependsOn` node from hello billing resource.</span></span> 

<span data-ttu-id="b86c3-140">tooverify hello aktualizovaná cena plánu, podívejte se na hello "Funkce + ceny" okna v prohlížeči hello.</span><span class="sxs-lookup"><span data-stu-id="b86c3-140">tooverify hello updated price plan, look at hello "Features+pricing" blade in hello browser.</span></span> <span data-ttu-id="b86c3-141">**Aktualizujte zobrazení prohlížeče hello** toomake, že vidíte nejnovější stav hello.</span><span class="sxs-lookup"><span data-stu-id="b86c3-141">**Refresh hello browser view** toomake sure you see hello latest state.</span></span>



## <a name="add-a-metric-alert"></a><span data-ttu-id="b86c3-142">Přidání metriky oznámení</span><span class="sxs-lookup"><span data-stu-id="b86c3-142">Add a metric alert</span></span>

<span data-ttu-id="b86c3-143">tooset si metriky výstraha v hello stejný čas jako prostředek vaší aplikace, kód sloučení takto do souboru šablony hello:</span><span class="sxs-lookup"><span data-stu-id="b86c3-143">tooset up a metric alert at hello same time as your app resource, merge code like this into hello template file:</span></span>

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

<span data-ttu-id="b86c3-144">Při vyvolání hello šablony, můžete přidat tento parametr:</span><span class="sxs-lookup"><span data-stu-id="b86c3-144">When you invoke hello template, you can optionally add this parameter:</span></span>

    `-responseTime 2`

<span data-ttu-id="b86c3-145">Samozřejmě můžete parametrizovat další pole.</span><span class="sxs-lookup"><span data-stu-id="b86c3-145">You can of course parameterize other fields.</span></span> 

<span data-ttu-id="b86c3-146">toofind názvy typů hello a podrobnosti o konfiguraci jiných výstrahy pravidel ručně vytvořit pravidlo a zkontrolujte ji v [Azure Resource Manager](https://resources.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="b86c3-146">toofind out hello type names and configuration details of other alert rules, create a rule manually and then inspect it in [Azure Resource Manager](https://resources.azure.com/).</span></span> 


## <a name="add-an-availability-test"></a><span data-ttu-id="b86c3-147">Přidat test dostupnosti</span><span class="sxs-lookup"><span data-stu-id="b86c3-147">Add an availability test</span></span>

<span data-ttu-id="b86c3-148">V tomto příkladu je pro testování ping (tootest jediné stránce).</span><span class="sxs-lookup"><span data-stu-id="b86c3-148">This example is for a ping test (tootest a single page).</span></span>  

<span data-ttu-id="b86c3-149">**Existují dvě části** v testu dostupnosti: test hello, sám a hello výstraha, která vás informuje o selhání.</span><span class="sxs-lookup"><span data-stu-id="b86c3-149">**There are two parts** in an availability test: hello test itself, and hello alert that notifies you of failures.</span></span>

<span data-ttu-id="b86c3-150">Slučte následující kód do souboru hello šablony, který vytvoří aplikace hello hello.</span><span class="sxs-lookup"><span data-stu-id="b86c3-150">Merge hello following code into hello template file that creates hello app.</span></span>

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

<span data-ttu-id="b86c3-151">toodiscover hello kódy pro jiné umístění testu nebo tooautomate hello vytvoření složitější webové testy, vytvořte příklad ručně a pak Parametrizace hello kód z [Azure Resource Manager](https://resources.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="b86c3-151">toodiscover hello codes for other test locations, or tooautomate hello creation of more complex web tests, create an example manually and then parameterize hello code from [Azure Resource Manager](https://resources.azure.com/).</span></span>

## <a name="add-more-resources"></a><span data-ttu-id="b86c3-152">Přidat další prostředky</span><span class="sxs-lookup"><span data-stu-id="b86c3-152">Add more resources</span></span>

<span data-ttu-id="b86c3-153">Vytvoření hello tooautomate jiný prostředek libovolného typu, například vytvořit ručně a pak zkopírujte a Parametrizace jeho kód z [Azure Resource Manager](https://resources.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="b86c3-153">tooautomate hello creation of any other resource of any kind, create an example manually, and then copy and parameterize its code from [Azure Resource Manager](https://resources.azure.com/).</span></span> 

1. <span data-ttu-id="b86c3-154">Otevřete [Azure Resource Manager](https://resources.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="b86c3-154">Open [Azure Resource Manager](https://resources.azure.com/).</span></span> <span data-ttu-id="b86c3-155">Přejděte dolů prostřednictvím `subscriptions/resourceGroups/<your resource group>/providers/Microsoft.Insights/components`, tooyour prostředků aplikace.</span><span class="sxs-lookup"><span data-stu-id="b86c3-155">Navigate down through `subscriptions/resourceGroups/<your resource group>/providers/Microsoft.Insights/components`, tooyour application resource.</span></span> 
   
    ![Navigace v Průzkumníku prostředků Azure.](./media/app-insights-powershell/01.png)
   
    <span data-ttu-id="b86c3-157">*Součásti* jsou základní prostředky Application Insights pro zobrazení aplikace hello.</span><span class="sxs-lookup"><span data-stu-id="b86c3-157">*Components* are hello basic Application Insights resources for displaying applications.</span></span> <span data-ttu-id="b86c3-158">Existují samostatné prostředky hello přidružená pravidla výstrah a testy dostupnosti webu.</span><span class="sxs-lookup"><span data-stu-id="b86c3-158">There are separate resources for hello associated alert rules and availability web tests.</span></span>
2. <span data-ttu-id="b86c3-159">Kopírování hello JSON hello součásti na příslušné místo hello v `template1.json`.</span><span class="sxs-lookup"><span data-stu-id="b86c3-159">Copy hello JSON of hello component into hello appropriate place in `template1.json`.</span></span>
3. <span data-ttu-id="b86c3-160">Odstraňte tyto vlastnosti:</span><span class="sxs-lookup"><span data-stu-id="b86c3-160">Delete these properties:</span></span>
   
   * `id`
   * `InstrumentationKey`
   * `CreationDate`
   * `TenantId`
4. <span data-ttu-id="b86c3-161">Otevřete hello webtests a alertrules části a zkopírujte hello JSON pro jednotlivé položky do šablony.</span><span class="sxs-lookup"><span data-stu-id="b86c3-161">Open hello webtests and alertrules sections and copy hello JSON for individual items into your template.</span></span> <span data-ttu-id="b86c3-162">(Není zkopírovat z uzlů webtests nebo alertrules hello: přejděte do položky hello pod nimi.)</span><span class="sxs-lookup"><span data-stu-id="b86c3-162">(Don't copy from hello webtests or alertrules nodes: go into hello items under them.)</span></span>
   
    <span data-ttu-id="b86c3-163">Každý test webu má přidružené pravidlo výstrahy, takže máte toocopy z nich.</span><span class="sxs-lookup"><span data-stu-id="b86c3-163">Each web test has an associated alert rule, so you have toocopy both of them.</span></span>
   
    <span data-ttu-id="b86c3-164">Můžete použít také výstrahy o metrikách.</span><span class="sxs-lookup"><span data-stu-id="b86c3-164">You can also include alerts on metrics.</span></span> <span data-ttu-id="b86c3-165">[Metriky názvy](app-insights-powershell-alerts.md#metric-names).</span><span class="sxs-lookup"><span data-stu-id="b86c3-165">[Metric names](app-insights-powershell-alerts.md#metric-names).</span></span>
5. <span data-ttu-id="b86c3-166">Vložte tento řádek v každého prostředku:</span><span class="sxs-lookup"><span data-stu-id="b86c3-166">Insert this line in each resource:</span></span>
   
    `"apiVersion": "2015-05-01",`

### <a name="parameterize-hello-template"></a><span data-ttu-id="b86c3-167">Parametrizace hello šablony</span><span class="sxs-lookup"><span data-stu-id="b86c3-167">Parameterize hello template</span></span>
<span data-ttu-id="b86c3-168">Nyní máte tooreplace hello konkrétní názvy s parametry.</span><span class="sxs-lookup"><span data-stu-id="b86c3-168">Now you have tooreplace hello specific names with parameters.</span></span> <span data-ttu-id="b86c3-169">příliš[Parametrizace šablonu](../azure-resource-manager/resource-group-authoring-templates.md), můžete psát pomocí výrazy [sadu pomocných funkcí](../azure-resource-manager/resource-group-template-functions.md).</span><span class="sxs-lookup"><span data-stu-id="b86c3-169">too[parameterize a template](../azure-resource-manager/resource-group-authoring-templates.md), you write expressions using a [set of helper functions](../azure-resource-manager/resource-group-template-functions.md).</span></span> 

<span data-ttu-id="b86c3-170">Nelze Parametrizace jenom část řetězce, takže použijte `concat()` toobuild řetězce.</span><span class="sxs-lookup"><span data-stu-id="b86c3-170">You can't parameterize just part of a string, so use `concat()` toobuild strings.</span></span>

<span data-ttu-id="b86c3-171">Zde jsou příklady nahrazení hello, měli byste toomake.</span><span class="sxs-lookup"><span data-stu-id="b86c3-171">Here are examples of hello substitutions you'll want toomake.</span></span> <span data-ttu-id="b86c3-172">Existuje několik výskyty každé nahrazování.</span><span class="sxs-lookup"><span data-stu-id="b86c3-172">There are several occurrences of each substitution.</span></span> <span data-ttu-id="b86c3-173">Ostatní bude pravděpodobně nutné ve vaší šabloně.</span><span class="sxs-lookup"><span data-stu-id="b86c3-173">You might need others in your template.</span></span> <span data-ttu-id="b86c3-174">Tyto příklady použití hello parametry a proměnné, které jsme definovali v horní části hello hello šablony.</span><span class="sxs-lookup"><span data-stu-id="b86c3-174">These examples use hello parameters and variables we defined at hello top of hello template.</span></span>

| <span data-ttu-id="b86c3-175">Najít</span><span class="sxs-lookup"><span data-stu-id="b86c3-175">find</span></span> | <span data-ttu-id="b86c3-176">Nahraďte</span><span class="sxs-lookup"><span data-stu-id="b86c3-176">replace with</span></span> |
| --- | --- |
| `"hidden-link:/subscriptions/.../components/MyAppName"` |`"[concat('hidden-link:',`<br/>` resourceId('microsoft.insights/components',` <br/> ` parameters('appName')))]"` |
| `"/subscriptions/.../alertrules/myAlertName-myAppName-subsId",` |`"[resourceId('Microsoft.Insights/alertrules', variables('alertRuleName'))]",` |
| `"/subscriptions/.../webtests/myTestName-myAppName",` |`"[resourceId('Microsoft.Insights/webtests', parameters('webTestName'))]",` |
| `"myWebTest-myAppName"` |`"[variables(testName)]"'` |
| `"myTestName-myAppName-subsId"` |`"[variables('alertRuleName')]"` |
| `"myAppName"` |`"[parameters('appName')]"` |
| <span data-ttu-id="b86c3-177">`"myappname"`(malá písmena)</span><span class="sxs-lookup"><span data-stu-id="b86c3-177">`"myappname"` (lower case)</span></span> |`"[toLower(parameters('appName'))]"` |
| `"<WebTest Name=\"myWebTest\" ...`<br/>` Url=\"http://fabrikam.com/home\" ...>"` |`[concat('<WebTest Name=\"',` <br/> `parameters('webTestName'),` <br/> `'\" ... Url=\"', parameters('Url'),` <br/> `'\"...>')]"`<br/><span data-ttu-id="b86c3-178">Odstranit Guid a ID.</span><span class="sxs-lookup"><span data-stu-id="b86c3-178">Delete Guid and Id.</span></span> |

### <a name="set-dependencies-between-hello-resources"></a><span data-ttu-id="b86c3-179">Nastavte závislosti mezi prostředky hello</span><span class="sxs-lookup"><span data-stu-id="b86c3-179">Set dependencies between hello resources</span></span>
<span data-ttu-id="b86c3-180">Azure měli nastavit hello prostředky v striktní pořadí.</span><span class="sxs-lookup"><span data-stu-id="b86c3-180">Azure should set up hello resources in strict order.</span></span> <span data-ttu-id="b86c3-181">toomake se, že jeden instalační program dokončí před hello vedle, přidejte závislosti řádky:</span><span class="sxs-lookup"><span data-stu-id="b86c3-181">toomake sure one setup completes before hello next begins, add dependency lines:</span></span>

* <span data-ttu-id="b86c3-182">V rámci dostupnosti hello testovací prostředků:</span><span class="sxs-lookup"><span data-stu-id="b86c3-182">In hello availability test resource:</span></span>
  
    `"dependsOn": ["[resourceId('Microsoft.Insights/components', parameters('appName'))]"],`
* <span data-ttu-id="b86c3-183">V hello výstrahy prostředku pro test dostupnosti:</span><span class="sxs-lookup"><span data-stu-id="b86c3-183">In hello alert resource for an availability test:</span></span>
  
    `"dependsOn": ["[resourceId('Microsoft.Insights/webtests', variables('testName'))]"],`



## <a name="next-steps"></a><span data-ttu-id="b86c3-184">Další kroky</span><span class="sxs-lookup"><span data-stu-id="b86c3-184">Next steps</span></span>
<span data-ttu-id="b86c3-185">Další články automatizace:</span><span class="sxs-lookup"><span data-stu-id="b86c3-185">Other automation articles:</span></span>

* <span data-ttu-id="b86c3-186">[Vytvořte prostředek Application Insights](app-insights-powershell-script-create-resource.md) -rychlý způsob bez použití šablony.</span><span class="sxs-lookup"><span data-stu-id="b86c3-186">[Create an Application Insights resource](app-insights-powershell-script-create-resource.md) - quick method without using a template.</span></span>
* [<span data-ttu-id="b86c3-187">Nastavení výstrah</span><span class="sxs-lookup"><span data-stu-id="b86c3-187">Set up Alerts</span></span>](app-insights-powershell-alerts.md)
* [<span data-ttu-id="b86c3-188">Tvorba webových testů</span><span class="sxs-lookup"><span data-stu-id="b86c3-188">Create web tests</span></span>](https://azure.microsoft.com/blog/creating-a-web-test-alert-programmatically-with-application-insights/)
* [<span data-ttu-id="b86c3-189">Odesílání Azure Diagnostics tooApplication statistiky</span><span class="sxs-lookup"><span data-stu-id="b86c3-189">Send Azure Diagnostics tooApplication Insights</span></span>](app-insights-powershell-azure-diagnostics.md)
* [<span data-ttu-id="b86c3-190">Nasazení tooAzure z Githubu</span><span class="sxs-lookup"><span data-stu-id="b86c3-190">Deploy tooAzure from GitHub</span></span>](http://blogs.msdn.com/b/webdev/archive/2015/09/16/deploy-to-azure-from-github-with-application-insights.aspx)
* [<span data-ttu-id="b86c3-191">Vytvořit poznámky k verzi</span><span class="sxs-lookup"><span data-stu-id="b86c3-191">Create release annotations</span></span>](https://github.com/Microsoft/ApplicationInsights-Home/blob/master/API/CreateReleaseAnnotation.ps1)

