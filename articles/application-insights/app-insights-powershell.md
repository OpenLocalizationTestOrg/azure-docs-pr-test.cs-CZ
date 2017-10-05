---
title: "Automatizace Azure Application Insights v prostředí PowerShell | Microsoft Docs"
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
ms.openlocfilehash: 88dbb9515300f847789bc889911cdeff5f5bdb53
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
#  <a name="create-application-insights-resources-using-powershell"></a><span data-ttu-id="40649-103">Vytváření prostředků Application Insights v prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="40649-103">Create Application Insights resources using PowerShell</span></span>
<span data-ttu-id="40649-104">Tento článek ukazuje, jak automatizovat vytváření a aktualizace [Application Insights](app-insights-overview.md) prostředky automaticky pomocí nástroje Správa prostředků Azure.</span><span class="sxs-lookup"><span data-stu-id="40649-104">This article shows you how to automate the creation and update of [Application Insights](app-insights-overview.md) resources automatically by using Azure Resource Management.</span></span> <span data-ttu-id="40649-105">Může například uděláte jako součást procesu sestavení.</span><span class="sxs-lookup"><span data-stu-id="40649-105">You might, for example, do so as part of a build process.</span></span> <span data-ttu-id="40649-106">Společně s základní prostředku Application Insights, můžete vytvořit [testy dostupnosti webu](app-insights-monitor-web-app-availability.md), nastavte [výstrahy](app-insights-alerts.md), nastavte [ceny schéma](app-insights-pricing.md)a vytvořte další prostředky Azure .</span><span class="sxs-lookup"><span data-stu-id="40649-106">Along with the basic Application Insights resource, you can create [availability web tests](app-insights-monitor-web-app-availability.md), set up [alerts](app-insights-alerts.md), set the [pricing scheme](app-insights-pricing.md), and create other Azure resources.</span></span>

<span data-ttu-id="40649-107">Klíčem k vytváření těchto prostředků je šablony JSON pro [Azure Resource Manager](../azure-resource-manager/powershell-azure-resource-manager.md).</span><span class="sxs-lookup"><span data-stu-id="40649-107">The key to creating these resources is JSON templates for [Azure Resource Manager](../azure-resource-manager/powershell-azure-resource-manager.md).</span></span> <span data-ttu-id="40649-108">Stručně řečeno, je postup: stažení JSON definice prostředků existující; Parametrizace určité hodnoty, například názvy; a spusťte šablonu vždy, když chcete vytvořit nový prostředek.</span><span class="sxs-lookup"><span data-stu-id="40649-108">In a nutshell, the procedure is: download the JSON definitions of existing resources; parameterize certain values such as names; and then run the template whenever you want to create a new resource.</span></span> <span data-ttu-id="40649-109">Několik prostředků můžete balíček dohromady, k jejich vytvoření vše v jednom přejděte – například monitorování aplikace s testy dostupnosti, výstrahy a úložiště pro nepřetržitou export.</span><span class="sxs-lookup"><span data-stu-id="40649-109">You can package several resources together, to create them all in one go - for example, an app monitor with availability tests, alerts, and storage for continuous export.</span></span> <span data-ttu-id="40649-110">Existují některé odlišnosti k některým z parameterizations, které vám objasníme sem.</span><span class="sxs-lookup"><span data-stu-id="40649-110">There are some subtleties to some of the parameterizations, which we'll explain here.</span></span>

## <a name="one-time-setup"></a><span data-ttu-id="40649-111">Jednorázové instalace</span><span class="sxs-lookup"><span data-stu-id="40649-111">One-time setup</span></span>
<span data-ttu-id="40649-112">Pokud jste nepoužili prostředí PowerShell s předplatným Azure před:</span><span class="sxs-lookup"><span data-stu-id="40649-112">If you haven't used PowerShell with your Azure subscription before:</span></span>

<span data-ttu-id="40649-113">Instalace modulu Azure Powershell na počítači, kde chcete spustit skripty:</span><span class="sxs-lookup"><span data-stu-id="40649-113">Install the Azure Powershell module on the machine where you want to run the scripts:</span></span>

1. <span data-ttu-id="40649-114">Nainstalujte [instalačního programu webové platformy (verze 5 nebo novější)](http://www.microsoft.com/web/downloads/platform.aspx).</span><span class="sxs-lookup"><span data-stu-id="40649-114">Install [Microsoft Web Platform Installer (v5 or higher)](http://www.microsoft.com/web/downloads/platform.aspx).</span></span>
2. <span data-ttu-id="40649-115">Jeho použití k instalaci aplikace Microsoft Azure Powershell.</span><span class="sxs-lookup"><span data-stu-id="40649-115">Use it to install Microsoft Azure Powershell.</span></span>

## <a name="create-an-azure-resource-manager-template"></a><span data-ttu-id="40649-116">Vytvořit šablonu Azure Resource Manager</span><span class="sxs-lookup"><span data-stu-id="40649-116">Create an Azure Resource Manager template</span></span>
<span data-ttu-id="40649-117">Vytvořte nový soubor .json – umožňuje volání `template1.json` v tomto příkladu.</span><span class="sxs-lookup"><span data-stu-id="40649-117">Create a new .json file - let's call it `template1.json` in this example.</span></span> <span data-ttu-id="40649-118">Zkopírujte do ní tento obsah:</span><span class="sxs-lookup"><span data-stu-id="40649-118">Copy this content into it:</span></span>

```JSON
    {
        "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
        "contentVersion": "1.0.0.0",
        "parameters": {
            "appName": {
                "type": "string",
                "metadata": {
                    "description": "Enter the application name."
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
                    "description": "Enter the application type."
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
                    "description": "Enter the application location."
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
                    "description": "Enter daily quota reset hour in UTC (0 to 23). Values outside the range will get a random reset hour."
                }
            },
            "warningThreshold": {
                "type": "int",
                "defaultValue": 90,
                "minValue": 1,
                "maxValue": 100,
                "metadata": {
                    "description": "Enter the % value of daily quota after which warning mail to be sent. "
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



## <a name="create-application-insights-resources"></a><span data-ttu-id="40649-119">Vytvořit prostředky Application Insights</span><span class="sxs-lookup"><span data-stu-id="40649-119">Create Application Insights resources</span></span>
1. <span data-ttu-id="40649-120">V prostředí PowerShell Přihlaste se k Azure:</span><span class="sxs-lookup"><span data-stu-id="40649-120">In PowerShell, sign in to Azure:</span></span>
   
    `Login-AzureRmAccount`
2. <span data-ttu-id="40649-121">Spusťte příkaz takto:</span><span class="sxs-lookup"><span data-stu-id="40649-121">Run a command like this:</span></span>
   
    ```PS
   
        New-AzureRmResourceGroupDeployment -ResourceGroupName Fabrikam `
               -TemplateFile .\template1.json `
               -appName myNewApp

    ``` 
   
   * <span data-ttu-id="40649-122">`-ResourceGroupName`je skupina, kde chcete vytvořit nové prostředky.</span><span class="sxs-lookup"><span data-stu-id="40649-122">`-ResourceGroupName` is the group where you want to create the new resources.</span></span>
   * <span data-ttu-id="40649-123">`-TemplateFile`musí nastat před vlastní parametry.</span><span class="sxs-lookup"><span data-stu-id="40649-123">`-TemplateFile` must occur before the custom parameters.</span></span>
   * <span data-ttu-id="40649-124">`-appName`Název prostředek pro vytvoření.</span><span class="sxs-lookup"><span data-stu-id="40649-124">`-appName` The name of the resource to create.</span></span>

<span data-ttu-id="40649-125">Můžete přidat další parametry – jejich popisy najdete v sekci parametrů šablony.</span><span class="sxs-lookup"><span data-stu-id="40649-125">You can add other parameters - you'll find their descriptions in the parameters section of the template.</span></span>

## <a name="to-get-the-instrumentation-key"></a><span data-ttu-id="40649-126">Získat klíč instrumentace</span><span class="sxs-lookup"><span data-stu-id="40649-126">To get the instrumentation key</span></span>
<span data-ttu-id="40649-127">Po vytvoření prostředek aplikace, budete muset klíč instrumentace:</span><span class="sxs-lookup"><span data-stu-id="40649-127">After creating an application resource, you'll want the instrumentation key:</span></span> 

```PS
    $resource = Find-AzureRmResource -ResourceNameEquals "<YOUR APP NAME>" -ResourceType "Microsoft.Insights/components"
    $details = Get-AzureRmResource -ResourceId $resource.ResourceId
    $ikey = $details.Properties.InstrumentationKey
```


<a id="price"></a>
## <a name="set-the-price-plan"></a><span data-ttu-id="40649-128">Nastavte plán cena</span><span class="sxs-lookup"><span data-stu-id="40649-128">Set the price plan</span></span>

<span data-ttu-id="40649-129">Můžete nastavit [cena plán](app-insights-pricing.md).</span><span class="sxs-lookup"><span data-stu-id="40649-129">You can set the [price plan](app-insights-pricing.md).</span></span>

<span data-ttu-id="40649-130">Chcete-li vytvořit prostředek aplikace s plánem cena Enterprise, pomocí výše uvedené šablony:</span><span class="sxs-lookup"><span data-stu-id="40649-130">To create an app resource with the Enterprise price plan, using the template above:</span></span>

```PS
        New-AzureRmResourceGroupDeployment -ResourceGroupName Fabrikam `
               -TemplateFile .\template1.json `
               -priceCode 2 `
               -appName myNewApp
```

|<span data-ttu-id="40649-131">priceCode</span><span class="sxs-lookup"><span data-stu-id="40649-131">priceCode</span></span>|<span data-ttu-id="40649-132">Plánování</span><span class="sxs-lookup"><span data-stu-id="40649-132">plan</span></span>|
|---|---|
|<span data-ttu-id="40649-133">1</span><span class="sxs-lookup"><span data-stu-id="40649-133">1</span></span>|<span data-ttu-id="40649-134">Basic</span><span class="sxs-lookup"><span data-stu-id="40649-134">Basic</span></span>|
|<span data-ttu-id="40649-135">2</span><span class="sxs-lookup"><span data-stu-id="40649-135">2</span></span>|<span data-ttu-id="40649-136">Enterprise</span><span class="sxs-lookup"><span data-stu-id="40649-136">Enterprise</span></span>|

* <span data-ttu-id="40649-137">Pokud chcete použít výchozí plán základní ceny, můžete vynechat CurrentBillingFeatures prostředků ze šablony.</span><span class="sxs-lookup"><span data-stu-id="40649-137">If you only want to use the default Basic price plan, you can omit the CurrentBillingFeatures resource from the template.</span></span>
* <span data-ttu-id="40649-138">Pokud chcete změnit plán cena po vytvoření součásti prostředků, můžete šablonu, která vynechá prostředků "microsoft.insights/components".</span><span class="sxs-lookup"><span data-stu-id="40649-138">If you want to change the price plan after the component resource has been created, you can use a template that omits the "microsoft.insights/components" resource.</span></span> <span data-ttu-id="40649-139">Navíc vynechejte `dependsOn` uzel z fakturace prostředku.</span><span class="sxs-lookup"><span data-stu-id="40649-139">Also, omit the `dependsOn` node from the billing resource.</span></span> 

<span data-ttu-id="40649-140">Chcete-li ověřit, aktualizovaná cena plánu, podívejte se "Funkce + ceny" okna v prohlížeči.</span><span class="sxs-lookup"><span data-stu-id="40649-140">To verify the updated price plan, look at the "Features+pricing" blade in the browser.</span></span> <span data-ttu-id="40649-141">**Aktualizujte zobrazení prohlížeče** a ujistěte se, vidíte nejnovější stav.</span><span class="sxs-lookup"><span data-stu-id="40649-141">**Refresh the browser view** to make sure you see the latest state.</span></span>



## <a name="add-a-metric-alert"></a><span data-ttu-id="40649-142">Přidání metriky oznámení</span><span class="sxs-lookup"><span data-stu-id="40649-142">Add a metric alert</span></span>

<span data-ttu-id="40649-143">Pokud chcete nastavit upozornění na metriky ve stejnou dobu jako prostředek vaší aplikace, sloučení do souboru šablony kódu takto:</span><span class="sxs-lookup"><span data-stu-id="40649-143">To set up a metric alert at the same time as your app resource, merge code like this into the template file:</span></span>

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
      // Ensure this resource is created after the app resource:
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

<span data-ttu-id="40649-144">Při vyvolání šablony, můžete přidat tento parametr:</span><span class="sxs-lookup"><span data-stu-id="40649-144">When you invoke the template, you can optionally add this parameter:</span></span>

    `-responseTime 2`

<span data-ttu-id="40649-145">Samozřejmě můžete parametrizovat další pole.</span><span class="sxs-lookup"><span data-stu-id="40649-145">You can of course parameterize other fields.</span></span> 

<span data-ttu-id="40649-146">Pokud chcete zjistit názvy typů a podrobnosti o konfiguraci pravidel dalších výstrah, ručně vytvořit pravidlo a poté zkontrolovat v [Azure Resource Manager](https://resources.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="40649-146">To find out the type names and configuration details of other alert rules, create a rule manually and then inspect it in [Azure Resource Manager](https://resources.azure.com/).</span></span> 


## <a name="add-an-availability-test"></a><span data-ttu-id="40649-147">Přidat test dostupnosti</span><span class="sxs-lookup"><span data-stu-id="40649-147">Add an availability test</span></span>

<span data-ttu-id="40649-148">V tomto příkladu je pro test příkazem ping (k testování jediné stránce).</span><span class="sxs-lookup"><span data-stu-id="40649-148">This example is for a ping test (to test a single page).</span></span>  

<span data-ttu-id="40649-149">**Existují dvě části** v testu dostupnosti: sama a příslušnou výstrahu, která vás informuje o selhání.</span><span class="sxs-lookup"><span data-stu-id="40649-149">**There are two parts** in an availability test: the test itself, and the alert that notifies you of failures.</span></span>

<span data-ttu-id="40649-150">Slučte následující kód do souboru šablony, který vytvoří aplikaci.</span><span class="sxs-lookup"><span data-stu-id="40649-150">Merge the following code into the template file that creates the app.</span></span>

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
      // Availability test: part 1 configures the test
      //
      "name": "[variables('pingTestName')]",
      "type": "Microsoft.Insights/webtests",
      "apiVersion": "2014-04-01",
      "location": "[parameters('appLocation')]",
      // Ensure this is created after the app resource:
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
          "WebTest": "[concat('<WebTest   Name=\"', variables('pingTestName'), '\"   Enabled=\"True\"         CssProjectStructure=\"\"    CssIteration=\"\"  Timeout=\"120\"  WorkItemIds=\"\"         xmlns=\"http://microsoft.com/schemas/VisualStudio/TeamTest/2010\"         Description=\"\"  CredentialUserName=\"\"  CredentialPassword=\"\"         PreAuthenticate=\"True\"  Proxy=\"default\"  StopOnError=\"False\"         RecordedResultFile=\"\"  ResultsLocale=\"\">  <Items>  <Request Method=\"GET\"    Version=\"1.1\"  Url=\"', parameters('Url'),   '\" ThinkTime=\"0\"  Timeout=\"300\" ParseDependentRequests=\"True\"         FollowRedirects=\"True\" RecordResult=\"True\" Cache=\"False\"         ResponseTimeGoal=\"0\"  Encoding=\"utf-8\"  ExpectedHttpStatusCode=\"200\"         ExpectedResponseUrl=\"\" ReportingName=\"\" IgnoreHttpStatusCode=\"False\" />        </Items>  <ValidationRules> <ValidationRule  Classname=\"Microsoft.VisualStudio.TestTools.WebTesting.Rules.ValidationRuleFindText, Microsoft.VisualStudio.QualityTools.WebTestFramework, Version=10.0.0.0, Culture=neutral, PublicKeyToken=b03f5f7f11d50a3a\" DisplayName=\"Find Text\"         Description=\"Verifies the existence of the specified text in the response.\"         Level=\"High\"  ExectuionOrder=\"BeforeDependents\">  <RuleParameters>        <RuleParameter Name=\"FindText\" Value=\"',   parameters('pingText'), '\" />  <RuleParameter Name=\"IgnoreCase\" Value=\"False\" />  <RuleParameter Name=\"UseRegularExpression\" Value=\"False\" />  <RuleParameter Name=\"PassIfTextFound\" Value=\"True\" />  </RuleParameters> </ValidationRule>  </ValidationRules>  </WebTest>')]"
        },
        "SyntheticMonitorId": "[variables('pingTestName')]"
      }
    },

    {
      //
      // Availability test: part 2, the alert rule
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

<span data-ttu-id="40649-151">Ke zjištění kódy pro jiné umístění testu, nebo k automatizaci vytváření složitějších webové testy, vytvořte příklad ručně a pak Parametrizace kód z [Azure Resource Manager](https://resources.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="40649-151">To discover the codes for other test locations, or to automate the creation of more complex web tests, create an example manually and then parameterize the code from [Azure Resource Manager](https://resources.azure.com/).</span></span>

## <a name="add-more-resources"></a><span data-ttu-id="40649-152">Přidat další prostředky</span><span class="sxs-lookup"><span data-stu-id="40649-152">Add more resources</span></span>

<span data-ttu-id="40649-153">K automatizaci vytváření jiný prostředek libovolného typu, například vytvořit ručně a pak zkopírujte a Parametrizace jeho kód z [Azure Resource Manager](https://resources.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="40649-153">To automate the creation of any other resource of any kind, create an example manually, and then copy and parameterize its code from [Azure Resource Manager](https://resources.azure.com/).</span></span> 

1. <span data-ttu-id="40649-154">Otevřete [Azure Resource Manager](https://resources.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="40649-154">Open [Azure Resource Manager](https://resources.azure.com/).</span></span> <span data-ttu-id="40649-155">Přejděte dolů prostřednictvím `subscriptions/resourceGroups/<your resource group>/providers/Microsoft.Insights/components`, na prostředek vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="40649-155">Navigate down through `subscriptions/resourceGroups/<your resource group>/providers/Microsoft.Insights/components`, to your application resource.</span></span> 
   
    ![Navigace v Průzkumníku prostředků Azure.](./media/app-insights-powershell/01.png)
   
    <span data-ttu-id="40649-157">*Součásti* jsou základní prostředky Application Insights pro zobrazení aplikace.</span><span class="sxs-lookup"><span data-stu-id="40649-157">*Components* are the basic Application Insights resources for displaying applications.</span></span> <span data-ttu-id="40649-158">Existují samostatné prostředky pro přidružená pravidla výstrah a testy dostupnosti webu.</span><span class="sxs-lookup"><span data-stu-id="40649-158">There are separate resources for the associated alert rules and availability web tests.</span></span>
2. <span data-ttu-id="40649-159">Zkopírujte JSON komponenty do odpovídajícího místa v `template1.json`.</span><span class="sxs-lookup"><span data-stu-id="40649-159">Copy the JSON of the component into the appropriate place in `template1.json`.</span></span>
3. <span data-ttu-id="40649-160">Odstraňte tyto vlastnosti:</span><span class="sxs-lookup"><span data-stu-id="40649-160">Delete these properties:</span></span>
   
   * `id`
   * `InstrumentationKey`
   * `CreationDate`
   * `TenantId`
4. <span data-ttu-id="40649-161">Otevřete webtests a alertrules části a zkopírujte JSON pro jednotlivé položky do šablony.</span><span class="sxs-lookup"><span data-stu-id="40649-161">Open the webtests and alertrules sections and copy the JSON for individual items into your template.</span></span> <span data-ttu-id="40649-162">(Není zkopírovat z uzlů webtests nebo alertrules: přejděte do položky pod nimi.)</span><span class="sxs-lookup"><span data-stu-id="40649-162">(Don't copy from the webtests or alertrules nodes: go into the items under them.)</span></span>
   
    <span data-ttu-id="40649-163">Každý test webu má přidružené pravidlo výstrahy, takže musíte zkopírovat oba dva.</span><span class="sxs-lookup"><span data-stu-id="40649-163">Each web test has an associated alert rule, so you have to copy both of them.</span></span>
   
    <span data-ttu-id="40649-164">Můžete použít také výstrahy o metrikách.</span><span class="sxs-lookup"><span data-stu-id="40649-164">You can also include alerts on metrics.</span></span> <span data-ttu-id="40649-165">[Metriky názvy](app-insights-powershell-alerts.md#metric-names).</span><span class="sxs-lookup"><span data-stu-id="40649-165">[Metric names](app-insights-powershell-alerts.md#metric-names).</span></span>
5. <span data-ttu-id="40649-166">Vložte tento řádek v každého prostředku:</span><span class="sxs-lookup"><span data-stu-id="40649-166">Insert this line in each resource:</span></span>
   
    `"apiVersion": "2015-05-01",`

### <a name="parameterize-the-template"></a><span data-ttu-id="40649-167">Parametrizace šablony</span><span class="sxs-lookup"><span data-stu-id="40649-167">Parameterize the template</span></span>
<span data-ttu-id="40649-168">Nyní máte nahraďte konkrétní názvy s parametry.</span><span class="sxs-lookup"><span data-stu-id="40649-168">Now you have to replace the specific names with parameters.</span></span> <span data-ttu-id="40649-169">K [Parametrizace šablonu](../azure-resource-manager/resource-group-authoring-templates.md), můžete psát pomocí výrazy [sadu pomocných funkcí](../azure-resource-manager/resource-group-template-functions.md).</span><span class="sxs-lookup"><span data-stu-id="40649-169">To [parameterize a template](../azure-resource-manager/resource-group-authoring-templates.md), you write expressions using a [set of helper functions](../azure-resource-manager/resource-group-template-functions.md).</span></span> 

<span data-ttu-id="40649-170">Nelze Parametrizace jenom část řetězce, takže použijte `concat()` k sestavení řetězce.</span><span class="sxs-lookup"><span data-stu-id="40649-170">You can't parameterize just part of a string, so use `concat()` to build strings.</span></span>

<span data-ttu-id="40649-171">Zde jsou příklady nahrazení, který budete chtít provést.</span><span class="sxs-lookup"><span data-stu-id="40649-171">Here are examples of the substitutions you'll want to make.</span></span> <span data-ttu-id="40649-172">Existuje několik výskyty každé nahrazování.</span><span class="sxs-lookup"><span data-stu-id="40649-172">There are several occurrences of each substitution.</span></span> <span data-ttu-id="40649-173">Ostatní bude pravděpodobně nutné ve vaší šabloně.</span><span class="sxs-lookup"><span data-stu-id="40649-173">You might need others in your template.</span></span> <span data-ttu-id="40649-174">Tyto příklady použít parametry a proměnné, které jsme definovali v horní části šablony.</span><span class="sxs-lookup"><span data-stu-id="40649-174">These examples use the parameters and variables we defined at the top of the template.</span></span>

| <span data-ttu-id="40649-175">Najít</span><span class="sxs-lookup"><span data-stu-id="40649-175">find</span></span> | <span data-ttu-id="40649-176">Nahraďte</span><span class="sxs-lookup"><span data-stu-id="40649-176">replace with</span></span> |
| --- | --- |
| `"hidden-link:/subscriptions/.../components/MyAppName"` |`"[concat('hidden-link:',`<br/>` resourceId('microsoft.insights/components',` <br/> ` parameters('appName')))]"` |
| `"/subscriptions/.../alertrules/myAlertName-myAppName-subsId",` |`"[resourceId('Microsoft.Insights/alertrules', variables('alertRuleName'))]",` |
| `"/subscriptions/.../webtests/myTestName-myAppName",` |`"[resourceId('Microsoft.Insights/webtests', parameters('webTestName'))]",` |
| `"myWebTest-myAppName"` |`"[variables(testName)]"'` |
| `"myTestName-myAppName-subsId"` |`"[variables('alertRuleName')]"` |
| `"myAppName"` |`"[parameters('appName')]"` |
| <span data-ttu-id="40649-177">`"myappname"`(malá písmena)</span><span class="sxs-lookup"><span data-stu-id="40649-177">`"myappname"` (lower case)</span></span> |`"[toLower(parameters('appName'))]"` |
| `"<WebTest Name=\"myWebTest\" ...`<br/>` Url=\"http://fabrikam.com/home\" ...>"` |`[concat('<WebTest Name=\"',` <br/> `parameters('webTestName'),` <br/> `'\" ... Url=\"', parameters('Url'),` <br/> `'\"...>')]"`<br/><span data-ttu-id="40649-178">Odstranit Guid a ID.</span><span class="sxs-lookup"><span data-stu-id="40649-178">Delete Guid and Id.</span></span> |

### <a name="set-dependencies-between-the-resources"></a><span data-ttu-id="40649-179">Nastavte závislosti mezi prostředky</span><span class="sxs-lookup"><span data-stu-id="40649-179">Set dependencies between the resources</span></span>
<span data-ttu-id="40649-180">Azure měli nastavit prostředky v striktní pořadí.</span><span class="sxs-lookup"><span data-stu-id="40649-180">Azure should set up the resources in strict order.</span></span> <span data-ttu-id="40649-181">Pokud chcete mít jistotu, že jeden instalační program dokončí před zahájením dalších, přidejte závislosti řádky:</span><span class="sxs-lookup"><span data-stu-id="40649-181">To make sure one setup completes before the next begins, add dependency lines:</span></span>

* <span data-ttu-id="40649-182">V testu prostředku dostupnosti:</span><span class="sxs-lookup"><span data-stu-id="40649-182">In the availability test resource:</span></span>
  
    `"dependsOn": ["[resourceId('Microsoft.Insights/components', parameters('appName'))]"],`
* <span data-ttu-id="40649-183">U výstrah prostředku pro test dostupnosti:</span><span class="sxs-lookup"><span data-stu-id="40649-183">In the alert resource for an availability test:</span></span>
  
    `"dependsOn": ["[resourceId('Microsoft.Insights/webtests', variables('testName'))]"],`



## <a name="next-steps"></a><span data-ttu-id="40649-184">Další kroky</span><span class="sxs-lookup"><span data-stu-id="40649-184">Next steps</span></span>
<span data-ttu-id="40649-185">Další články automatizace:</span><span class="sxs-lookup"><span data-stu-id="40649-185">Other automation articles:</span></span>

* <span data-ttu-id="40649-186">[Vytvořte prostředek Application Insights](app-insights-powershell-script-create-resource.md) -rychlý způsob bez použití šablony.</span><span class="sxs-lookup"><span data-stu-id="40649-186">[Create an Application Insights resource](app-insights-powershell-script-create-resource.md) - quick method without using a template.</span></span>
* [<span data-ttu-id="40649-187">Nastavení výstrah</span><span class="sxs-lookup"><span data-stu-id="40649-187">Set up Alerts</span></span>](app-insights-powershell-alerts.md)
* [<span data-ttu-id="40649-188">Tvorba webových testů</span><span class="sxs-lookup"><span data-stu-id="40649-188">Create web tests</span></span>](https://azure.microsoft.com/blog/creating-a-web-test-alert-programmatically-with-application-insights/)
* [<span data-ttu-id="40649-189">Odesílání Diagnostiky Azure do Application Insights</span><span class="sxs-lookup"><span data-stu-id="40649-189">Send Azure Diagnostics to Application Insights</span></span>](app-insights-powershell-azure-diagnostics.md)
* [<span data-ttu-id="40649-190">Nasazení do Azure z Githubu</span><span class="sxs-lookup"><span data-stu-id="40649-190">Deploy to Azure from GitHub</span></span>](http://blogs.msdn.com/b/webdev/archive/2015/09/16/deploy-to-azure-from-github-with-application-insights.aspx)
* [<span data-ttu-id="40649-191">Vytvořit poznámky k verzi</span><span class="sxs-lookup"><span data-stu-id="40649-191">Create release annotations</span></span>](https://github.com/Microsoft/ApplicationInsights-Home/blob/master/API/CreateReleaseAnnotation.ps1)

