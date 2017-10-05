---
title: "Automatizovat nasazení prostředků pro funkce aplikace v Azure Functions | Microsoft Docs"
description: "Naučte se vytvářet šablonu Azure Resource Manager, která nasadí aplikaci funkce."
services: Functions
documtationcenter: na
author: lindydonna
manager: erikre
editor: 
tags: 
keywords: "Azure funkce, funkce, bez serveru architektura, infrastruktura jako kód, azure resource Manageru"
ms.assetid: d20743e3-aab6-442c-a836-9bcea09bfd32
ms.server: functions
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 05/25/2017
ms.author: glenga
ms.openlocfilehash: 15496e4ab2858b2aa319d53f1c438a259a3d5e49
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="automate-resource-deployment-for-your-function-app-in-azure-functions"></a><span data-ttu-id="68a4b-104">Automatizovat nasazení prostředků pro funkce aplikace v Azure Functions</span><span class="sxs-lookup"><span data-stu-id="68a4b-104">Automate resource deployment for your function app in Azure Functions</span></span>

<span data-ttu-id="68a4b-105">Šablonu Azure Resource Manager můžete použít k nasazení aplikace funkce.</span><span class="sxs-lookup"><span data-stu-id="68a4b-105">You can use an Azure Resource Manager template to deploy a function app.</span></span> <span data-ttu-id="68a4b-106">Tento článek popisuje požadované prostředky a parametry, jak to udělat.</span><span class="sxs-lookup"><span data-stu-id="68a4b-106">This article outlines the required resources and parameters for doing so.</span></span> <span data-ttu-id="68a4b-107">Možná budete muset nasadit další prostředky, v závislosti na [triggerů a vazeb](functions-triggers-bindings.md) ve vaší aplikaci funkce.</span><span class="sxs-lookup"><span data-stu-id="68a4b-107">You might need to deploy additional resources, depending on the [triggers and bindings](functions-triggers-bindings.md) in your function app.</span></span>

<span data-ttu-id="68a4b-108">Další informace o vytváření šablon najdete v tématu [šablon pro tvorbu Azure Resource Manageru](../azure-resource-manager/resource-group-authoring-templates.md).</span><span class="sxs-lookup"><span data-stu-id="68a4b-108">For more information about creating templates, see [Authoring Azure Resource Manager templates](../azure-resource-manager/resource-group-authoring-templates.md).</span></span>

<span data-ttu-id="68a4b-109">Ukázka šablony najdete v části:</span><span class="sxs-lookup"><span data-stu-id="68a4b-109">For sample templates, see:</span></span>
- <span data-ttu-id="68a4b-110">[funkce aplikace v plánu spotřeby]</span><span class="sxs-lookup"><span data-stu-id="68a4b-110">[Function app on Consumption plan]</span></span>
- <span data-ttu-id="68a4b-111">[funkce aplikace na plán služby Azure App Service]</span><span class="sxs-lookup"><span data-stu-id="68a4b-111">[Function app on Azure App Service plan]</span></span>

## <a name="required-resources"></a><span data-ttu-id="68a4b-112">Požadované prostředky</span><span class="sxs-lookup"><span data-stu-id="68a4b-112">Required resources</span></span>

<span data-ttu-id="68a4b-113">Funkce aplikace vyžaduje tyto prostředky:</span><span class="sxs-lookup"><span data-stu-id="68a4b-113">A function app requires these resources:</span></span>

* <span data-ttu-id="68a4b-114">[Azure Storage](../storage/index.md) účtu</span><span class="sxs-lookup"><span data-stu-id="68a4b-114">An [Azure Storage](../storage/index.md) account</span></span>
* <span data-ttu-id="68a4b-115">Plán hostování (spotřeba plánu nebo plán služby App Service)</span><span class="sxs-lookup"><span data-stu-id="68a4b-115">A hosting plan (Consumption plan or App Service plan)</span></span>
* <span data-ttu-id="68a4b-116">Aplikace – funkce</span><span class="sxs-lookup"><span data-stu-id="68a4b-116">A function app</span></span> 

### <a name="storage-account"></a><span data-ttu-id="68a4b-117">Účet úložiště</span><span class="sxs-lookup"><span data-stu-id="68a4b-117">Storage account</span></span>

<span data-ttu-id="68a4b-118">Účet úložiště Azure je vyžadována pro funkce aplikace.</span><span class="sxs-lookup"><span data-stu-id="68a4b-118">An Azure storage account is required for a function app.</span></span> <span data-ttu-id="68a4b-119">Potřebujete účet obecné účely, která podporuje objekty BLOB, tabulky, fronty a soubory.</span><span class="sxs-lookup"><span data-stu-id="68a4b-119">You need a general purpose account that supports blobs, tables, queues, and files.</span></span> <span data-ttu-id="68a4b-120">Další informace najdete v tématu [požadavky na účet úložiště Azure Functions](functions-create-function-app-portal.md#storage-account-requirements).</span><span class="sxs-lookup"><span data-stu-id="68a4b-120">For more information, see [Azure Functions storage account requirements](functions-create-function-app-portal.md#storage-account-requirements).</span></span>

```json
{
    "type": "Microsoft.Storage/storageAccounts",
    "name": "[variables('storageAccountName')]",
    "apiVersion": "2015-06-15",
    "location": "[resourceGroup().location]",
    "properties": {
        "accountType": "[parameters('storageAccountType')]"
    }
}
```

<span data-ttu-id="68a4b-121">Kromě toho vlastnosti `AzureWebJobsStorage` a `AzureWebJobsDashboard` musí být zadány jako nastavení aplikace v konfiguraci lokality.</span><span class="sxs-lookup"><span data-stu-id="68a4b-121">In addition, the properties `AzureWebJobsStorage` and `AzureWebJobsDashboard` must be specified as app settings in the site configuration.</span></span> <span data-ttu-id="68a4b-122">Modul runtime Azure Functions používá `AzureWebJobsStorage` připojovací řetězec k vytvoření interní front.</span><span class="sxs-lookup"><span data-stu-id="68a4b-122">The Azure Functions runtime uses the `AzureWebJobsStorage` connection string to create internal queues.</span></span> <span data-ttu-id="68a4b-123">Připojovací řetězec `AzureWebJobsDashboard` se používá k přihlášení k Azure Table storage a power **monitorování** na portálu.</span><span class="sxs-lookup"><span data-stu-id="68a4b-123">The connection string `AzureWebJobsDashboard` is used to log to Azure Table storage and power the **Monitor** tab in the portal.</span></span>

<span data-ttu-id="68a4b-124">Tyto vlastnosti jsou určené v `appSettings` kolekce `siteConfig` objektu:</span><span class="sxs-lookup"><span data-stu-id="68a4b-124">These properties are specified in the `appSettings` collection in the `siteConfig` object:</span></span>

```json
"appSettings": [
    {
        "name": "AzureWebJobsStorage",
        "value": "[concat('DefaultEndpointsProtocol=https;AccountName=', variables('storageAccountName'), ';AccountKey=', listKeys(variables('storageAccountid'),'2015-05-01-preview').key1)]"
    },
    {
        "name": "AzureWebJobsDashboard",
        "value": "[concat('DefaultEndpointsProtocol=https;AccountName=', variables('storageAccountName'), ';AccountKey=', listKeys(variables('storageAccountid'),'2015-05-01-preview').key1)]"
    }
```    

### <a name="hosting-plan"></a><span data-ttu-id="68a4b-125">Hostování plán</span><span class="sxs-lookup"><span data-stu-id="68a4b-125">Hosting plan</span></span>

<span data-ttu-id="68a4b-126">Definice hostingový plán se liší v závislosti na tom, zda používáte plánu spotřeby nebo služby App Service.</span><span class="sxs-lookup"><span data-stu-id="68a4b-126">The definition of the hosting plan varies, depending on whether you use a Consumption or App Service plan.</span></span> <span data-ttu-id="68a4b-127">V tématu [nasazení funkce aplikace plánu spotřeby](#consumption) a [nasazení funkce aplikace na plán služby App Service](#app-service-plan).</span><span class="sxs-lookup"><span data-stu-id="68a4b-127">See [Deploy a function app on the Consumption plan](#consumption) and [Deploy a function app on the App Service plan](#app-service-plan).</span></span>

### <a name="function-app"></a><span data-ttu-id="68a4b-128">Funkce aplikace</span><span class="sxs-lookup"><span data-stu-id="68a4b-128">Function app</span></span>

<span data-ttu-id="68a4b-129">Prostředek aplikace funkce se definuje pomocí prostředek typu **Microsoft.Web/Site** a typ **functionapp**:</span><span class="sxs-lookup"><span data-stu-id="68a4b-129">The function app resource is defined by using a resource of type **Microsoft.Web/Site** and kind **functionapp**:</span></span>

```json
{
    "apiVersion": "2015-08-01",
    "type": "Microsoft.Web/sites",
    "name": "[variables('functionAppName')]",
    "location": "[resourceGroup().location]",
    "kind": "functionapp",            
    "dependsOn": [
        "[resourceId('Microsoft.Web/serverfarms', variables('hostingPlanName'))]",
        "[resourceId('Microsoft.Storage/storageAccounts', variables('storageAccountName'))]"
    ]
```

<a name="consumption"></a>

## <a name="deploy-a-function-app-on-the-consumption-plan"></a><span data-ttu-id="68a4b-130">Nasazení aplikace funkce plánu spotřeby</span><span class="sxs-lookup"><span data-stu-id="68a4b-130">Deploy a function app on the Consumption plan</span></span>

<span data-ttu-id="68a4b-131">Funkce aplikace můžete spustit ve dvou různých režimech: plánování využívání a plán služby App Service.</span><span class="sxs-lookup"><span data-stu-id="68a4b-131">You can run a function app in two different modes: the Consumption plan and the App Service plan.</span></span> <span data-ttu-id="68a4b-132">Plánu spotřeby automaticky přiděluje výpočetní výkon, když kód běží, horizontálně navýší kapacitu podle potřeby pro zpracování zatížení a potom škáluje, pokud kód není spuštěna.</span><span class="sxs-lookup"><span data-stu-id="68a4b-132">The Consumption plan automatically allocates compute power when your code is running, scales out as necessary to handle load, and then scales down when code is not running.</span></span> <span data-ttu-id="68a4b-133">Ano nemusíte zaplatit nečinnosti virtuálních počítačů a nemusíte předem záložní kapacita.</span><span class="sxs-lookup"><span data-stu-id="68a4b-133">So, you don't have to pay for idle VMs, and you don't have to reserve capacity in advance.</span></span> <span data-ttu-id="68a4b-134">Další informace o hostování plány, najdete v části [využívání funkce Azure a služby App Service plány](functions-scale.md).</span><span class="sxs-lookup"><span data-stu-id="68a4b-134">To learn more about hosting plans, see [Azure Functions Consumption and App Service plans](functions-scale.md).</span></span>

<span data-ttu-id="68a4b-135">Vzorové šablony Azure Resource Manager, najdete v části [funkce aplikace v plánu spotřeby].</span><span class="sxs-lookup"><span data-stu-id="68a4b-135">For a sample Azure Resource Manager template, see [Function app on Consumption plan].</span></span>

### <a name="create-a-consumption-plan"></a><span data-ttu-id="68a4b-136">Vytvoření plánu spotřeby</span><span class="sxs-lookup"><span data-stu-id="68a4b-136">Create a Consumption plan</span></span>

<span data-ttu-id="68a4b-137">Plán spotřeba je speciální typ prostředku "serverovou farmu".</span><span class="sxs-lookup"><span data-stu-id="68a4b-137">A Consumption plan is a special type of "serverfarm" resource.</span></span> <span data-ttu-id="68a4b-138">Můžete zadat pomocí `Dynamic` hodnota `computeMode` a `sku` vlastnosti:</span><span class="sxs-lookup"><span data-stu-id="68a4b-138">You specify it by using the `Dynamic` value for the `computeMode` and `sku` properties:</span></span>

```json
{
    "type": "Microsoft.Web/serverfarms",
    "apiVersion": "2015-04-01",
    "name": "[variables('hostingPlanName')]",
    "location": "[resourceGroup().location]",
    "properties": {
        "name": "[variables('hostingPlanName')]",
        "computeMode": "Dynamic",
        "sku": "Dynamic"
    }
}
```

### <a name="create-a-function-app"></a><span data-ttu-id="68a4b-139">Vytvoření Function App</span><span class="sxs-lookup"><span data-stu-id="68a4b-139">Create a function app</span></span>

<span data-ttu-id="68a4b-140">Kromě toho plán spotřeby vyžaduje další dvě nastavení v konfiguraci lokality: `WEBSITE_CONTENTAZUREFILECONNECTIONSTRING` a `WEBSITE_CONTENTSHARE`.</span><span class="sxs-lookup"><span data-stu-id="68a4b-140">In addition, a Consumption plan requires two additional settings in the site configuration: `WEBSITE_CONTENTAZUREFILECONNECTIONSTRING` and `WEBSITE_CONTENTSHARE`.</span></span> <span data-ttu-id="68a4b-141">Tyto vlastnosti nakonfigurovat cesta k úložišti účtu a soubor kde jsou uložené funkce kódu aplikace a konfigurace.</span><span class="sxs-lookup"><span data-stu-id="68a4b-141">These properties configure the storage account and file path where the function app code and configuration are stored.</span></span>

```json
{
    "apiVersion": "2015-08-01",
    "type": "Microsoft.Web/sites",
    "name": "[variables('functionAppName')]",
    "location": "[resourceGroup().location]",
    "kind": "functionapp",            
    "dependsOn": [
        "[resourceId('Microsoft.Web/serverfarms', variables('hostingPlanName'))]",
        "[resourceId('Microsoft.Storage/storageAccounts', variables('storageAccountName'))]"
    ],
    "properties": {
        "serverFarmId": "[resourceId('Microsoft.Web/serverfarms', variables('hostingPlanName'))]",
        "siteConfig": {
            "appSettings": [
                {
                    "name": "AzureWebJobsDashboard",
                    "value": "[concat('DefaultEndpointsProtocol=https;AccountName=', variables('storageAccountName'), ';AccountKey=', listKeys(variables('storageAccountid'),'2015-05-01-preview').key1)]"
                },
                {
                    "name": "AzureWebJobsStorage",
                    "value": "[concat('DefaultEndpointsProtocol=https;AccountName=', variables('storageAccountName'), ';AccountKey=', listKeys(variables('storageAccountid'),'2015-05-01-preview').key1)]"
                },
                {
                    "name": "WEBSITE_CONTENTAZUREFILECONNECTIONSTRING",
                    "value": "[concat('DefaultEndpointsProtocol=https;AccountName=', variables('storageAccountName'), ';AccountKey=', listKeys(variables('storageAccountid'),'2015-05-01-preview').key1)]"
                },
                {
                    "name": "WEBSITE_CONTENTSHARE",
                    "value": "[toLower(variables('functionAppName'))]"
                },
                {
                    "name": "FUNCTIONS_EXTENSION_VERSION",
                    "value": "~1"
                }
            ]
        }
    }
}
```                    

<a name="app-service-plan"></a> 

## <a name="deploy-a-function-app-on-the-app-service-plan"></a><span data-ttu-id="68a4b-142">Nasazení aplikace funkce na plán služby App Service</span><span class="sxs-lookup"><span data-stu-id="68a4b-142">Deploy a function app on the App Service plan</span></span>

<span data-ttu-id="68a4b-143">V plánu služby App Service které běží vaše aplikace funkce na vyhrazených virtuálních počítačích na Basic, Standard a Premium SKU, podobně jako webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="68a4b-143">In the App Service plan, your function app runs on dedicated VMs on Basic, Standard, and Premium SKUs, similar to web apps.</span></span> <span data-ttu-id="68a4b-144">Podrobnosti o tom, jak funguje plán služby App Service najdete v tématu [podrobný přehled plánů služby Azure App Service](../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md).</span><span class="sxs-lookup"><span data-stu-id="68a4b-144">For details about how the App Service plan works, see the [Azure App Service plans in-depth overview](../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md).</span></span> 

<span data-ttu-id="68a4b-145">Vzorové šablony Azure Resource Manager, najdete v části [funkce aplikace na plán služby Azure App Service].</span><span class="sxs-lookup"><span data-stu-id="68a4b-145">For a sample Azure Resource Manager template, see [Function app on Azure App Service plan].</span></span>

### <a name="create-an-app-service-plan"></a><span data-ttu-id="68a4b-146">Vytvoření plánu služby App Service</span><span class="sxs-lookup"><span data-stu-id="68a4b-146">Create an App Service plan</span></span>

```json
{
    "type": "Microsoft.Web/serverfarms",
    "apiVersion": "2015-04-01",
    "name": "[variables('hostingPlanName')]",
    "location": "[resourceGroup().location]",
    "properties": {
        "name": "[variables('hostingPlanName')]",
        "sku": "[parameters('sku')]",
        "workerSize": "[parameters('workerSize')]",
        "hostingEnvironment": "",
        "numberOfWorkers": 1
    }
}
```

### <a name="create-a-function-app"></a><span data-ttu-id="68a4b-147">Vytvoření Function App</span><span class="sxs-lookup"><span data-stu-id="68a4b-147">Create a function app</span></span> 

<span data-ttu-id="68a4b-148">Poté, co jste vybrali volbu změny měřítka, vytvořte aplikaci funkce.</span><span class="sxs-lookup"><span data-stu-id="68a4b-148">After you've selected a scaling option, create a function app.</span></span> <span data-ttu-id="68a4b-149">Tato aplikace je kontejner, který obsahuje všechny funkce.</span><span class="sxs-lookup"><span data-stu-id="68a4b-149">The app is the container that holds all your functions.</span></span>

<span data-ttu-id="68a4b-150">Funkce aplikace má mnoho podřízené prostředky, které můžete použít ve vašem nasazení, včetně nastavení aplikace a možnosti řízení zdroje.</span><span class="sxs-lookup"><span data-stu-id="68a4b-150">A function app has many child resources that you can use in your deployment, including app settings and source control options.</span></span> <span data-ttu-id="68a4b-151">Můžete také zvolit odebrat **sourcecontrols** podřízených prostředků a použijte jiné [možnost nasazení](functions-continuous-deployment.md) místo.</span><span class="sxs-lookup"><span data-stu-id="68a4b-151">You also might choose to remove the **sourcecontrols** child resource, and use a different [deployment option](functions-continuous-deployment.md) instead.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="68a4b-152">K úspěšnému nasazení vaší aplikace pomocí Azure Resource Manager, je důležité pochopit, jak jsou prostředky nasazené v Azure.</span><span class="sxs-lookup"><span data-stu-id="68a4b-152">To successfully deploy your application by using Azure Resource Manager, it's important to understand how resources are deployed in Azure.</span></span> <span data-ttu-id="68a4b-153">V následujícím příkladu jsou použita nejvyšší úrovně konfigurace pomocí **siteConfig**.</span><span class="sxs-lookup"><span data-stu-id="68a4b-153">In the following example, top-level configurations are applied by using **siteConfig**.</span></span> <span data-ttu-id="68a4b-154">Je důležité nastavit tyto konfigurace na nejvyšší úrovni, protože jejich předání informací o tom informace k modulu runtime a nasazení funkce.</span><span class="sxs-lookup"><span data-stu-id="68a4b-154">It's important to set these configurations at a top level, because they convey information to the Functions runtime and deployment engine.</span></span> <span data-ttu-id="68a4b-155">Informace na nejvyšší úrovni je vyžadována před podřízená **sourcecontrols nebo webové** prostředků se použije.</span><span class="sxs-lookup"><span data-stu-id="68a4b-155">Top-level information is required before the child **sourcecontrols/web** resource is applied.</span></span> <span data-ttu-id="68a4b-156">I když je možné nakonfigurovat tato nastavení na podřízené úrovni **config/appSettings** prostředku, v některých případech aplikace funkce musí být nasazený *před* **config/appSettings** platí.</span><span class="sxs-lookup"><span data-stu-id="68a4b-156">Although it's possible to configure these settings in the child-level **config/appSettings** resource, in some cases your function app must be deployed *before* **config/appSettings** is applied.</span></span> <span data-ttu-id="68a4b-157">Například při použití funkce s [Logic Apps](../logic-apps/index.md), funkcí jsou závislost jiný prostředek.</span><span class="sxs-lookup"><span data-stu-id="68a4b-157">For example, when you are using functions with [Logic Apps](../logic-apps/index.md), your functions are a dependency of another resource.</span></span>

```json
{
  "apiVersion": "2015-08-01",
  "name": "[parameters('appName')]",
  "type": "Microsoft.Web/sites",
  "kind": "functionapp",
  "location": "[parameters('location')]",
  "dependsOn": [
    "[resourceId('Microsoft.Storage/storageAccounts', variables('storageAccountName'))]",
    "[resourceId('Microsoft.Web/serverfarms', parameters('appName'))]"
  ],
  "properties": {
     "serverFarmId": "[variables('appServicePlanName')]",
     "siteConfig": {
        "alwaysOn": true,
        "appSettings": [
            { "name": "FUNCTIONS_EXTENSION_VERSION", "value": "~1" },
            { "name": "Project", "value": "src" }
        ]
     }
  },
  "resources": [
     {
        "apiVersion": "2015-08-01",
        "name": "appsettings",
        "type": "config",
        "dependsOn": [
          "[resourceId('Microsoft.Web/Sites', parameters('appName'))]",
          "[resourceId('Microsoft.Web/Sites/sourcecontrols', parameters('appName'), 'web')]",
          "[resourceId('Microsoft.Storage/storageAccounts', variables('storageAccountName'))]"
        ],
        "properties": {
          "AzureWebJobsStorage": "[concat('DefaultEndpointsProtocol=https;AccountName=', variables('storageAccountName'), ';AccountKey=', listKeys(variables('storageAccountid'),'2015-05-01-preview').key1)]",
          "AzureWebJobsDashboard": "[concat('DefaultEndpointsProtocol=https;AccountName=', variables('storageAccountName'), ';AccountKey=', listKeys(variables('storageAccountid'),'2015-05-01-preview').key1)]"
        }
     },
     {
          "apiVersion": "2015-08-01",
          "name": "web",
          "type": "sourcecontrols",
          "dependsOn": [
            "[resourceId('Microsoft.Web/sites/', parameters('appName'))]"
          ],
          "properties": {
            "RepoUrl": "[parameters('sourceCodeRepositoryURL')]",
            "branch": "[parameters('sourceCodeBranch')]",
            "IsManualIntegration": "[parameters('sourceCodeManualIntegration')]"
          }
     }
  ]
}
```
> [!TIP]
> <span data-ttu-id="68a4b-158">Tato šablona používá [projektu](https://github.com/projectkudu/kudu/wiki/Customizing-deployments#using-app-settings-instead-of-a-deployment-file) hodnotu nastavení aplikace, která nastaví základního adresáře, ve které funkce modul pro nasazení (Kudu) hledá nasadit kódu.</span><span class="sxs-lookup"><span data-stu-id="68a4b-158">This template uses the [Project](https://github.com/projectkudu/kudu/wiki/Customizing-deployments#using-app-settings-instead-of-a-deployment-file) app settings value, which sets the base directory in which the Functions deployment engine (Kudu) looks for deployable code.</span></span> <span data-ttu-id="68a4b-159">V našem úložišti naše funkce jsou v podsložce **src** složky.</span><span class="sxs-lookup"><span data-stu-id="68a4b-159">In our repository, our functions are in a subfolder of the **src** folder.</span></span> <span data-ttu-id="68a4b-160">Ano, v předchozím příkladu jsme hodnotu aplikaci nastavení `src`.</span><span class="sxs-lookup"><span data-stu-id="68a4b-160">So, in the preceding example, we set the app settings value to `src`.</span></span> <span data-ttu-id="68a4b-161">Pokud jsou funkcí v kořenovém adresáři úložiště nebo pokud nejsou nasazení od správy zdrojového kódu, můžete odebrat tuto hodnotu nastavení aplikace.</span><span class="sxs-lookup"><span data-stu-id="68a4b-161">If your functions are in the root of your repository, or if you are not deploying from source control, you can remove this app settings value.</span></span>

## <a name="deploy-your-template"></a><span data-ttu-id="68a4b-162">Nasazení šablony</span><span class="sxs-lookup"><span data-stu-id="68a4b-162">Deploy your template</span></span>

<span data-ttu-id="68a4b-163">Můžete použít některou z následujících způsobů k nasazení vaší šablony:</span><span class="sxs-lookup"><span data-stu-id="68a4b-163">You can use any of the following ways to deploy your template:</span></span>

* [<span data-ttu-id="68a4b-164">PowerShell</span><span class="sxs-lookup"><span data-stu-id="68a4b-164">PowerShell</span></span>](../azure-resource-manager/resource-group-template-deploy.md)
* [<span data-ttu-id="68a4b-165">Azure CLI</span><span class="sxs-lookup"><span data-stu-id="68a4b-165">Azure CLI</span></span>](../azure-resource-manager/resource-group-template-deploy-cli.md)
* [<span data-ttu-id="68a4b-166">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="68a4b-166">Azure portal</span></span>](../azure-resource-manager/resource-group-template-deploy-portal.md)
* [<span data-ttu-id="68a4b-167">REST API</span><span class="sxs-lookup"><span data-stu-id="68a4b-167">REST API</span></span>](../azure-resource-manager/resource-group-template-deploy-rest.md)

### <a name="deploy-to-azure-button"></a><span data-ttu-id="68a4b-168">Nasazení do Azure tlačítko</span><span class="sxs-lookup"><span data-stu-id="68a4b-168">Deploy to Azure button</span></span>

<span data-ttu-id="68a4b-169">Nahraďte ```<url-encoded-path-to-azuredeploy-json>``` s [kódovaná adresou URL](https://www.bing.com/search?q=url+encode) verzi nezpracovaná cesta vaší `azuredeploy.json` souboru na Githubu.</span><span class="sxs-lookup"><span data-stu-id="68a4b-169">Replace ```<url-encoded-path-to-azuredeploy-json>``` with a [URL-encoded](https://www.bing.com/search?q=url+encode) version of the raw path of your `azuredeploy.json` file in GitHub.</span></span>

<span data-ttu-id="68a4b-170">Tady je příklad, který používá markdownu:</span><span class="sxs-lookup"><span data-stu-id="68a4b-170">Here is an example that uses markdown:</span></span>

```markdown
[![Deploy to Azure](http://azuredeploy.net/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/<url-encoded-path-to-azuredeploy-json>)
```

<span data-ttu-id="68a4b-171">Tady je příklad, který používá HTML:</span><span class="sxs-lookup"><span data-stu-id="68a4b-171">Here is an example that uses HTML:</span></span>

```html
<a href="https://portal.azure.com/#create/Microsoft.Template/uri/<url-encoded-path-to-azuredeploy-json>" target="_blank"><img src="http://azuredeploy.net/deploybutton.png"></a>
```

## <a name="next-steps"></a><span data-ttu-id="68a4b-172">Další kroky</span><span class="sxs-lookup"><span data-stu-id="68a4b-172">Next steps</span></span>

<span data-ttu-id="68a4b-173">Další informace o tom, jak vyvíjet a konfigurovat Azure Functions.</span><span class="sxs-lookup"><span data-stu-id="68a4b-173">Learn more about how to develop and configure Azure Functions.</span></span>

* [<span data-ttu-id="68a4b-174">Referenční informace pro vývojáře Azure Functions</span><span class="sxs-lookup"><span data-stu-id="68a4b-174">Azure Functions developer reference</span></span>](functions-reference.md)
* [<span data-ttu-id="68a4b-175">Postup konfigurace nastavení aplikace Azure – funkce</span><span class="sxs-lookup"><span data-stu-id="68a4b-175">How to configure Azure function app settings</span></span>](functions-how-to-use-azure-function-app-settings.md)
* [<span data-ttu-id="68a4b-176">Vytvoření první funkce Azure</span><span class="sxs-lookup"><span data-stu-id="68a4b-176">Create your first Azure function</span></span>](functions-create-first-azure-function.md)

<!-- LINKS -->

[funkce aplikace v plánu spotřeby]: https://github.com/Azure/azure-quickstart-templates/blob/master/101-function-app-create-dynamic/azuredeploy.json
[funkce aplikace na plán služby Azure App Service]: https://github.com/Azure/azure-quickstart-templates/blob/master/101-function-app-create-dedicated/azuredeploy.json