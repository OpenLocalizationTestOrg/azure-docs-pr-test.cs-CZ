---
title: "aaaAutomate nasazení prostředků pro funkce aplikace v Azure Functions | Microsoft Docs"
description: "Zjistěte, jak toobuild šablonu Azure Resource Manager, která nasadí aplikaci funkce."
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
ms.openlocfilehash: b0df0d4ef9fe93213f7b1cb1d1e6b4e14f8b3a30
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="automate-resource-deployment-for-your-function-app-in-azure-functions"></a><span data-ttu-id="68ae9-104">Automatizovat nasazení prostředků pro funkce aplikace v Azure Functions</span><span class="sxs-lookup"><span data-stu-id="68ae9-104">Automate resource deployment for your function app in Azure Functions</span></span>

<span data-ttu-id="68ae9-105">Můžete použít toodeploy šablony Azure Resource Manager aplikaci funkce.</span><span class="sxs-lookup"><span data-stu-id="68ae9-105">You can use an Azure Resource Manager template toodeploy a function app.</span></span> <span data-ttu-id="68ae9-106">Tento článek popisuje hello požadované prostředky a parametry, jak to udělat.</span><span class="sxs-lookup"><span data-stu-id="68ae9-106">This article outlines hello required resources and parameters for doing so.</span></span> <span data-ttu-id="68ae9-107">Budete potřebovat další prostředky toodeploy, v závislosti na hello [triggerů a vazeb](functions-triggers-bindings.md) ve vaší aplikaci funkce.</span><span class="sxs-lookup"><span data-stu-id="68ae9-107">You might need toodeploy additional resources, depending on hello [triggers and bindings](functions-triggers-bindings.md) in your function app.</span></span>

<span data-ttu-id="68ae9-108">Další informace o vytváření šablon najdete v tématu [šablon pro tvorbu Azure Resource Manageru](../azure-resource-manager/resource-group-authoring-templates.md).</span><span class="sxs-lookup"><span data-stu-id="68ae9-108">For more information about creating templates, see [Authoring Azure Resource Manager templates](../azure-resource-manager/resource-group-authoring-templates.md).</span></span>

<span data-ttu-id="68ae9-109">Ukázka šablony najdete v části:</span><span class="sxs-lookup"><span data-stu-id="68ae9-109">For sample templates, see:</span></span>
- <span data-ttu-id="68ae9-110">[funkce aplikace v plánu spotřeby]</span><span class="sxs-lookup"><span data-stu-id="68ae9-110">[Function app on Consumption plan]</span></span>
- <span data-ttu-id="68ae9-111">[funkce aplikace na plán služby Azure App Service]</span><span class="sxs-lookup"><span data-stu-id="68ae9-111">[Function app on Azure App Service plan]</span></span>

## <a name="required-resources"></a><span data-ttu-id="68ae9-112">Požadované prostředky</span><span class="sxs-lookup"><span data-stu-id="68ae9-112">Required resources</span></span>

<span data-ttu-id="68ae9-113">Funkce aplikace vyžaduje tyto prostředky:</span><span class="sxs-lookup"><span data-stu-id="68ae9-113">A function app requires these resources:</span></span>

* <span data-ttu-id="68ae9-114">[Azure Storage](../storage/index.md) účtu</span><span class="sxs-lookup"><span data-stu-id="68ae9-114">An [Azure Storage](../storage/index.md) account</span></span>
* <span data-ttu-id="68ae9-115">Plán hostování (spotřeba plánu nebo plán služby App Service)</span><span class="sxs-lookup"><span data-stu-id="68ae9-115">A hosting plan (Consumption plan or App Service plan)</span></span>
* <span data-ttu-id="68ae9-116">Aplikace – funkce</span><span class="sxs-lookup"><span data-stu-id="68ae9-116">A function app</span></span> 

### <a name="storage-account"></a><span data-ttu-id="68ae9-117">Účet úložiště</span><span class="sxs-lookup"><span data-stu-id="68ae9-117">Storage account</span></span>

<span data-ttu-id="68ae9-118">Účet úložiště Azure je vyžadována pro funkce aplikace.</span><span class="sxs-lookup"><span data-stu-id="68ae9-118">An Azure storage account is required for a function app.</span></span> <span data-ttu-id="68ae9-119">Potřebujete účet obecné účely, která podporuje objekty BLOB, tabulky, fronty a soubory.</span><span class="sxs-lookup"><span data-stu-id="68ae9-119">You need a general purpose account that supports blobs, tables, queues, and files.</span></span> <span data-ttu-id="68ae9-120">Další informace najdete v tématu [požadavky na účet úložiště Azure Functions](functions-create-function-app-portal.md#storage-account-requirements).</span><span class="sxs-lookup"><span data-stu-id="68ae9-120">For more information, see [Azure Functions storage account requirements](functions-create-function-app-portal.md#storage-account-requirements).</span></span>

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

<span data-ttu-id="68ae9-121">Kromě toho hello vlastnosti `AzureWebJobsStorage` a `AzureWebJobsDashboard` musí být zadány jako nastavení aplikace v konfiguraci lokality hello.</span><span class="sxs-lookup"><span data-stu-id="68ae9-121">In addition, hello properties `AzureWebJobsStorage` and `AzureWebJobsDashboard` must be specified as app settings in hello site configuration.</span></span> <span data-ttu-id="68ae9-122">modulu runtime Azure Functions Hello používá hello `AzureWebJobsStorage` připojovací řetězec toocreate vnitřní fronty.</span><span class="sxs-lookup"><span data-stu-id="68ae9-122">hello Azure Functions runtime uses hello `AzureWebJobsStorage` connection string toocreate internal queues.</span></span> <span data-ttu-id="68ae9-123">Hello připojovací řetězec `AzureWebJobsDashboard` je použité toolog tooAzure tabulky úložiště a power hello **monitorování** kartě hello portálu.</span><span class="sxs-lookup"><span data-stu-id="68ae9-123">hello connection string `AzureWebJobsDashboard` is used toolog tooAzure Table storage and power hello **Monitor** tab in hello portal.</span></span>

<span data-ttu-id="68ae9-124">Tyto vlastnosti jsou určené v hello `appSettings` kolekce v hello `siteConfig` objektu:</span><span class="sxs-lookup"><span data-stu-id="68ae9-124">These properties are specified in hello `appSettings` collection in hello `siteConfig` object:</span></span>

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

### <a name="hosting-plan"></a><span data-ttu-id="68ae9-125">Hostování plán</span><span class="sxs-lookup"><span data-stu-id="68ae9-125">Hosting plan</span></span>

<span data-ttu-id="68ae9-126">definice Hello hello hostování plán se liší v závislosti na tom, zda používáte plánu spotřeby nebo služby App Service.</span><span class="sxs-lookup"><span data-stu-id="68ae9-126">hello definition of hello hosting plan varies, depending on whether you use a Consumption or App Service plan.</span></span> <span data-ttu-id="68ae9-127">V tématu [nasazení funkce aplikace v plánu spotřeby hello](#consumption) a [nasazení funkce aplikace na hello plán služby App Service](#app-service-plan).</span><span class="sxs-lookup"><span data-stu-id="68ae9-127">See [Deploy a function app on hello Consumption plan](#consumption) and [Deploy a function app on hello App Service plan](#app-service-plan).</span></span>

### <a name="function-app"></a><span data-ttu-id="68ae9-128">Funkce aplikace</span><span class="sxs-lookup"><span data-stu-id="68ae9-128">Function app</span></span>

<span data-ttu-id="68ae9-129">prostředek aplikace Hello funkce se definuje pomocí prostředek typu **Microsoft.Web/Site** a typ **functionapp**:</span><span class="sxs-lookup"><span data-stu-id="68ae9-129">hello function app resource is defined by using a resource of type **Microsoft.Web/Site** and kind **functionapp**:</span></span>

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

## <a name="deploy-a-function-app-on-hello-consumption-plan"></a><span data-ttu-id="68ae9-130">Nasazení funkce aplikace v plánu spotřeby hello</span><span class="sxs-lookup"><span data-stu-id="68ae9-130">Deploy a function app on hello Consumption plan</span></span>

<span data-ttu-id="68ae9-131">Funkce aplikace můžete spustit ve dvou různých režimech: hello plánu spotřeby a hello plán služby App Service.</span><span class="sxs-lookup"><span data-stu-id="68ae9-131">You can run a function app in two different modes: hello Consumption plan and hello App Service plan.</span></span> <span data-ttu-id="68ae9-132">plánu spotřeby Hello automaticky přiděluje výpočetní výkon, pokud váš kód běží, horizontálně navýší kapacitu jako nezbytné toohandle zatížení a pak škáluje, pokud kód není spuštěna.</span><span class="sxs-lookup"><span data-stu-id="68ae9-132">hello Consumption plan automatically allocates compute power when your code is running, scales out as necessary toohandle load, and then scales down when code is not running.</span></span> <span data-ttu-id="68ae9-133">Ano nemáte toopay nečinnosti virtuálních počítačů a nemáte dostatečnou kapacitu tooreserve předem.</span><span class="sxs-lookup"><span data-stu-id="68ae9-133">So, you don't have toopay for idle VMs, and you don't have tooreserve capacity in advance.</span></span> <span data-ttu-id="68ae9-134">toolearn Další informace o hostování plány, najdete v části [využívání funkce Azure a služby App Service plány](functions-scale.md).</span><span class="sxs-lookup"><span data-stu-id="68ae9-134">toolearn more about hosting plans, see [Azure Functions Consumption and App Service plans](functions-scale.md).</span></span>

<span data-ttu-id="68ae9-135">Vzorové šablony Azure Resource Manager, najdete v části [funkce aplikace v plánu spotřeby].</span><span class="sxs-lookup"><span data-stu-id="68ae9-135">For a sample Azure Resource Manager template, see [Function app on Consumption plan].</span></span>

### <a name="create-a-consumption-plan"></a><span data-ttu-id="68ae9-136">Vytvoření plánu spotřeby</span><span class="sxs-lookup"><span data-stu-id="68ae9-136">Create a Consumption plan</span></span>

<span data-ttu-id="68ae9-137">Plán spotřeba je speciální typ prostředku "serverovou farmu".</span><span class="sxs-lookup"><span data-stu-id="68ae9-137">A Consumption plan is a special type of "serverfarm" resource.</span></span> <span data-ttu-id="68ae9-138">Můžete zadat pomocí hello `Dynamic` hodnotu hello `computeMode` a `sku` vlastnosti:</span><span class="sxs-lookup"><span data-stu-id="68ae9-138">You specify it by using hello `Dynamic` value for hello `computeMode` and `sku` properties:</span></span>

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

### <a name="create-a-function-app"></a><span data-ttu-id="68ae9-139">Vytvoření Function App</span><span class="sxs-lookup"><span data-stu-id="68ae9-139">Create a function app</span></span>

<span data-ttu-id="68ae9-140">Kromě toho plán spotřeby vyžaduje další dvě nastavení v konfiguraci lokality hello: `WEBSITE_CONTENTAZUREFILECONNECTIONSTRING` a `WEBSITE_CONTENTSHARE`.</span><span class="sxs-lookup"><span data-stu-id="68ae9-140">In addition, a Consumption plan requires two additional settings in hello site configuration: `WEBSITE_CONTENTAZUREFILECONNECTIONSTRING` and `WEBSITE_CONTENTSHARE`.</span></span> <span data-ttu-id="68ae9-141">Tyto vlastnosti nakonfigurovat hello úložiště účet a cestu k souboru kde jsou uloženy kód aplikace hello funkce a konfigurace.</span><span class="sxs-lookup"><span data-stu-id="68ae9-141">These properties configure hello storage account and file path where hello function app code and configuration are stored.</span></span>

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

## <a name="deploy-a-function-app-on-hello-app-service-plan"></a><span data-ttu-id="68ae9-142">Nasazení aplikace funkce na hello plán služby App Service</span><span class="sxs-lookup"><span data-stu-id="68ae9-142">Deploy a function app on hello App Service plan</span></span>

<span data-ttu-id="68ae9-143">V hello plán služby App Service které běží vaše aplikace funkce na vyhrazených virtuálních počítačích na Basic, Standard a Premium SKU, podobně jako tooweb aplikace.</span><span class="sxs-lookup"><span data-stu-id="68ae9-143">In hello App Service plan, your function app runs on dedicated VMs on Basic, Standard, and Premium SKUs, similar tooweb apps.</span></span> <span data-ttu-id="68ae9-144">Podrobnosti o tom, jak funguje hello plán služby App Service najdete v tématu hello [podrobný přehled plánů služby Azure App Service](../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md).</span><span class="sxs-lookup"><span data-stu-id="68ae9-144">For details about how hello App Service plan works, see hello [Azure App Service plans in-depth overview](../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md).</span></span> 

<span data-ttu-id="68ae9-145">Vzorové šablony Azure Resource Manager, najdete v části [funkce aplikace na plán služby Azure App Service].</span><span class="sxs-lookup"><span data-stu-id="68ae9-145">For a sample Azure Resource Manager template, see [Function app on Azure App Service plan].</span></span>

### <a name="create-an-app-service-plan"></a><span data-ttu-id="68ae9-146">Vytvoření plánu služby App Service</span><span class="sxs-lookup"><span data-stu-id="68ae9-146">Create an App Service plan</span></span>

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

### <a name="create-a-function-app"></a><span data-ttu-id="68ae9-147">Vytvoření Function App</span><span class="sxs-lookup"><span data-stu-id="68ae9-147">Create a function app</span></span> 

<span data-ttu-id="68ae9-148">Poté, co jste vybrali volbu změny měřítka, vytvořte aplikaci funkce.</span><span class="sxs-lookup"><span data-stu-id="68ae9-148">After you've selected a scaling option, create a function app.</span></span> <span data-ttu-id="68ae9-149">aplikace Hello je hello kontejner, který obsahuje všechny funkce.</span><span class="sxs-lookup"><span data-stu-id="68ae9-149">hello app is hello container that holds all your functions.</span></span>

<span data-ttu-id="68ae9-150">Funkce aplikace má mnoho podřízené prostředky, které můžete použít ve vašem nasazení, včetně nastavení aplikace a možnosti řízení zdroje.</span><span class="sxs-lookup"><span data-stu-id="68ae9-150">A function app has many child resources that you can use in your deployment, including app settings and source control options.</span></span> <span data-ttu-id="68ae9-151">Můžete také vybrat tooremove hello **sourcecontrols** podřízených prostředků a použijte jiné [možnost nasazení](functions-continuous-deployment.md) místo.</span><span class="sxs-lookup"><span data-stu-id="68ae9-151">You also might choose tooremove hello **sourcecontrols** child resource, and use a different [deployment option](functions-continuous-deployment.md) instead.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="68ae9-152">toosuccessfully nasazení vaší aplikace pomocí Azure Resource Manager, je důležité toounderstand, jak jsou prostředky nasazené v Azure.</span><span class="sxs-lookup"><span data-stu-id="68ae9-152">toosuccessfully deploy your application by using Azure Resource Manager, it's important toounderstand how resources are deployed in Azure.</span></span> <span data-ttu-id="68ae9-153">V následujícím příkladu hello, jsou použita nejvyšší úrovně konfigurace pomocí **siteConfig**.</span><span class="sxs-lookup"><span data-stu-id="68ae9-153">In hello following example, top-level configurations are applied by using **siteConfig**.</span></span> <span data-ttu-id="68ae9-154">Je důležité tooset tyto konfigurace horní úrovni, protože jejich předání informací o tom modul runtime a nasazení toohello funkce informace.</span><span class="sxs-lookup"><span data-stu-id="68ae9-154">It's important tooset these configurations at a top level, because they convey information toohello Functions runtime and deployment engine.</span></span> <span data-ttu-id="68ae9-155">Informace na nejvyšší úrovni je vyžadována před hello podřízené **sourcecontrols nebo webové** prostředků se použije.</span><span class="sxs-lookup"><span data-stu-id="68ae9-155">Top-level information is required before hello child **sourcecontrols/web** resource is applied.</span></span> <span data-ttu-id="68ae9-156">I když je možné tooconfigure tato nastavení v hello podřízenou **config/appSettings** prostředku, v některých případech aplikace funkce musí být nasazený *před* **config/appSettings**  platí.</span><span class="sxs-lookup"><span data-stu-id="68ae9-156">Although it's possible tooconfigure these settings in hello child-level **config/appSettings** resource, in some cases your function app must be deployed *before* **config/appSettings** is applied.</span></span> <span data-ttu-id="68ae9-157">Například při použití funkce s [Logic Apps](../logic-apps/index.md), funkcí jsou závislost jiný prostředek.</span><span class="sxs-lookup"><span data-stu-id="68ae9-157">For example, when you are using functions with [Logic Apps](../logic-apps/index.md), your functions are a dependency of another resource.</span></span>

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
> <span data-ttu-id="68ae9-158">Tato šablona používá hello [projektu](https://github.com/projectkudu/kudu/wiki/Customizing-deployments#using-app-settings-instead-of-a-deployment-file) hodnotu nastavení aplikace, která nastaví hello základního adresáře, ve které hello modul nasazení funkce (Kudu) hledá nasadit kódu.</span><span class="sxs-lookup"><span data-stu-id="68ae9-158">This template uses hello [Project](https://github.com/projectkudu/kudu/wiki/Customizing-deployments#using-app-settings-instead-of-a-deployment-file) app settings value, which sets hello base directory in which hello Functions deployment engine (Kudu) looks for deployable code.</span></span> <span data-ttu-id="68ae9-159">V našem úložišti naše funkce jsou v podsložce hello **src** složky.</span><span class="sxs-lookup"><span data-stu-id="68ae9-159">In our repository, our functions are in a subfolder of hello **src** folder.</span></span> <span data-ttu-id="68ae9-160">Ano, v předchozím příkladu hello, nastaví hodnotu nastavení aplikace hello příliš`src`.</span><span class="sxs-lookup"><span data-stu-id="68ae9-160">So, in hello preceding example, we set hello app settings value too`src`.</span></span> <span data-ttu-id="68ae9-161">Pokud jsou funkcí v hello kořenového úložiště nebo pokud nejsou nasazení od správy zdrojového kódu, můžete odebrat tuto hodnotu nastavení aplikace.</span><span class="sxs-lookup"><span data-stu-id="68ae9-161">If your functions are in hello root of your repository, or if you are not deploying from source control, you can remove this app settings value.</span></span>

## <a name="deploy-your-template"></a><span data-ttu-id="68ae9-162">Nasazení šablony</span><span class="sxs-lookup"><span data-stu-id="68ae9-162">Deploy your template</span></span>

<span data-ttu-id="68ae9-163">Můžete použít některou z následujících způsobů toodeploy hello šablony:</span><span class="sxs-lookup"><span data-stu-id="68ae9-163">You can use any of hello following ways toodeploy your template:</span></span>

* [<span data-ttu-id="68ae9-164">PowerShell</span><span class="sxs-lookup"><span data-stu-id="68ae9-164">PowerShell</span></span>](../azure-resource-manager/resource-group-template-deploy.md)
* [<span data-ttu-id="68ae9-165">Azure CLI</span><span class="sxs-lookup"><span data-stu-id="68ae9-165">Azure CLI</span></span>](../azure-resource-manager/resource-group-template-deploy-cli.md)
* [<span data-ttu-id="68ae9-166">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="68ae9-166">Azure portal</span></span>](../azure-resource-manager/resource-group-template-deploy-portal.md)
* [<span data-ttu-id="68ae9-167">REST API</span><span class="sxs-lookup"><span data-stu-id="68ae9-167">REST API</span></span>](../azure-resource-manager/resource-group-template-deploy-rest.md)

### <a name="deploy-tooazure-button"></a><span data-ttu-id="68ae9-168">TooAzure tlačítko nasadit</span><span class="sxs-lookup"><span data-stu-id="68ae9-168">Deploy tooAzure button</span></span>

<span data-ttu-id="68ae9-169">Nahraďte ```<url-encoded-path-to-azuredeploy-json>``` s [kódovaná adresou URL](https://www.bing.com/search?q=url+encode) verzi hello nezpracovaná cesta vaší `azuredeploy.json` souboru na Githubu.</span><span class="sxs-lookup"><span data-stu-id="68ae9-169">Replace ```<url-encoded-path-to-azuredeploy-json>``` with a [URL-encoded](https://www.bing.com/search?q=url+encode) version of hello raw path of your `azuredeploy.json` file in GitHub.</span></span>

<span data-ttu-id="68ae9-170">Tady je příklad, který používá markdownu:</span><span class="sxs-lookup"><span data-stu-id="68ae9-170">Here is an example that uses markdown:</span></span>

```markdown
[![Deploy tooAzure](http://azuredeploy.net/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/<url-encoded-path-to-azuredeploy-json>)
```

<span data-ttu-id="68ae9-171">Tady je příklad, který používá HTML:</span><span class="sxs-lookup"><span data-stu-id="68ae9-171">Here is an example that uses HTML:</span></span>

```html
<a href="https://portal.azure.com/#create/Microsoft.Template/uri/<url-encoded-path-to-azuredeploy-json>" target="_blank"><img src="http://azuredeploy.net/deploybutton.png"></a>
```

## <a name="next-steps"></a><span data-ttu-id="68ae9-172">Další kroky</span><span class="sxs-lookup"><span data-stu-id="68ae9-172">Next steps</span></span>

<span data-ttu-id="68ae9-173">Další informace o toodevelop a konfigurace Azure Functions.</span><span class="sxs-lookup"><span data-stu-id="68ae9-173">Learn more about how toodevelop and configure Azure Functions.</span></span>

* [<span data-ttu-id="68ae9-174">Referenční informace pro vývojáře Azure Functions</span><span class="sxs-lookup"><span data-stu-id="68ae9-174">Azure Functions developer reference</span></span>](functions-reference.md)
* [<span data-ttu-id="68ae9-175">Jak tooconfigure Azure fungovat nastavení aplikace</span><span class="sxs-lookup"><span data-stu-id="68ae9-175">How tooconfigure Azure function app settings</span></span>](functions-how-to-use-azure-function-app-settings.md)
* [<span data-ttu-id="68ae9-176">Vytvoření první funkce Azure</span><span class="sxs-lookup"><span data-stu-id="68ae9-176">Create your first Azure function</span></span>](functions-create-first-azure-function.md)

<!-- LINKS -->

[funkce aplikace v plánu spotřeby]: https://github.com/Azure/azure-quickstart-templates/blob/master/101-function-app-create-dynamic/azuredeploy.json
[funkce aplikace na plán služby Azure App Service]: https://github.com/Azure/azure-quickstart-templates/blob/master/101-function-app-create-dedicated/azuredeploy.json
