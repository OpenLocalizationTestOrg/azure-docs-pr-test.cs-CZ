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
# <a name="automate-resource-deployment-for-your-function-app-in-azure-functions"></a>Automatizovat nasazení prostředků pro funkce aplikace v Azure Functions

Můžete použít toodeploy šablony Azure Resource Manager aplikaci funkce. Tento článek popisuje hello požadované prostředky a parametry, jak to udělat. Budete potřebovat další prostředky toodeploy, v závislosti na hello [triggerů a vazeb](functions-triggers-bindings.md) ve vaší aplikaci funkce.

Další informace o vytváření šablon najdete v tématu [šablon pro tvorbu Azure Resource Manageru](../azure-resource-manager/resource-group-authoring-templates.md).

Ukázka šablony najdete v části:
- [funkce aplikace v plánu spotřeby]
- [funkce aplikace na plán služby Azure App Service]

## <a name="required-resources"></a>Požadované prostředky

Funkce aplikace vyžaduje tyto prostředky:

* [Azure Storage](../storage/index.md) účtu
* Plán hostování (spotřeba plánu nebo plán služby App Service)
* Aplikace – funkce 

### <a name="storage-account"></a>Účet úložiště

Účet úložiště Azure je vyžadována pro funkce aplikace. Potřebujete účet obecné účely, která podporuje objekty BLOB, tabulky, fronty a soubory. Další informace najdete v tématu [požadavky na účet úložiště Azure Functions](functions-create-function-app-portal.md#storage-account-requirements).

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

Kromě toho hello vlastnosti `AzureWebJobsStorage` a `AzureWebJobsDashboard` musí být zadány jako nastavení aplikace v konfiguraci lokality hello. modulu runtime Azure Functions Hello používá hello `AzureWebJobsStorage` připojovací řetězec toocreate vnitřní fronty. Hello připojovací řetězec `AzureWebJobsDashboard` je použité toolog tooAzure tabulky úložiště a power hello **monitorování** kartě hello portálu.

Tyto vlastnosti jsou určené v hello `appSettings` kolekce v hello `siteConfig` objektu:

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

### <a name="hosting-plan"></a>Hostování plán

definice Hello hello hostování plán se liší v závislosti na tom, zda používáte plánu spotřeby nebo služby App Service. V tématu [nasazení funkce aplikace v plánu spotřeby hello](#consumption) a [nasazení funkce aplikace na hello plán služby App Service](#app-service-plan).

### <a name="function-app"></a>Funkce aplikace

prostředek aplikace Hello funkce se definuje pomocí prostředek typu **Microsoft.Web/Site** a typ **functionapp**:

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

## <a name="deploy-a-function-app-on-hello-consumption-plan"></a>Nasazení funkce aplikace v plánu spotřeby hello

Funkce aplikace můžete spustit ve dvou různých režimech: hello plánu spotřeby a hello plán služby App Service. plánu spotřeby Hello automaticky přiděluje výpočetní výkon, pokud váš kód běží, horizontálně navýší kapacitu jako nezbytné toohandle zatížení a pak škáluje, pokud kód není spuštěna. Ano nemáte toopay nečinnosti virtuálních počítačů a nemáte dostatečnou kapacitu tooreserve předem. toolearn Další informace o hostování plány, najdete v části [využívání funkce Azure a služby App Service plány](functions-scale.md).

Vzorové šablony Azure Resource Manager, najdete v části [funkce aplikace v plánu spotřeby].

### <a name="create-a-consumption-plan"></a>Vytvoření plánu spotřeby

Plán spotřeba je speciální typ prostředku "serverovou farmu". Můžete zadat pomocí hello `Dynamic` hodnotu hello `computeMode` a `sku` vlastnosti:

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

### <a name="create-a-function-app"></a>Vytvoření Function App

Kromě toho plán spotřeby vyžaduje další dvě nastavení v konfiguraci lokality hello: `WEBSITE_CONTENTAZUREFILECONNECTIONSTRING` a `WEBSITE_CONTENTSHARE`. Tyto vlastnosti nakonfigurovat hello úložiště účet a cestu k souboru kde jsou uloženy kód aplikace hello funkce a konfigurace.

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

## <a name="deploy-a-function-app-on-hello-app-service-plan"></a>Nasazení aplikace funkce na hello plán služby App Service

V hello plán služby App Service které běží vaše aplikace funkce na vyhrazených virtuálních počítačích na Basic, Standard a Premium SKU, podobně jako tooweb aplikace. Podrobnosti o tom, jak funguje hello plán služby App Service najdete v tématu hello [podrobný přehled plánů služby Azure App Service](../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md). 

Vzorové šablony Azure Resource Manager, najdete v části [funkce aplikace na plán služby Azure App Service].

### <a name="create-an-app-service-plan"></a>Vytvoření plánu služby App Service

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

### <a name="create-a-function-app"></a>Vytvoření Function App 

Poté, co jste vybrali volbu změny měřítka, vytvořte aplikaci funkce. aplikace Hello je hello kontejner, který obsahuje všechny funkce.

Funkce aplikace má mnoho podřízené prostředky, které můžete použít ve vašem nasazení, včetně nastavení aplikace a možnosti řízení zdroje. Můžete také vybrat tooremove hello **sourcecontrols** podřízených prostředků a použijte jiné [možnost nasazení](functions-continuous-deployment.md) místo.

> [!IMPORTANT]
> toosuccessfully nasazení vaší aplikace pomocí Azure Resource Manager, je důležité toounderstand, jak jsou prostředky nasazené v Azure. V následujícím příkladu hello, jsou použita nejvyšší úrovně konfigurace pomocí **siteConfig**. Je důležité tooset tyto konfigurace horní úrovni, protože jejich předání informací o tom modul runtime a nasazení toohello funkce informace. Informace na nejvyšší úrovni je vyžadována před hello podřízené **sourcecontrols nebo webové** prostředků se použije. I když je možné tooconfigure tato nastavení v hello podřízenou **config/appSettings** prostředku, v některých případech aplikace funkce musí být nasazený *před* **config/appSettings**  platí. Například při použití funkce s [Logic Apps](../logic-apps/index.md), funkcí jsou závislost jiný prostředek.

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
> Tato šablona používá hello [projektu](https://github.com/projectkudu/kudu/wiki/Customizing-deployments#using-app-settings-instead-of-a-deployment-file) hodnotu nastavení aplikace, která nastaví hello základního adresáře, ve které hello modul nasazení funkce (Kudu) hledá nasadit kódu. V našem úložišti naše funkce jsou v podsložce hello **src** složky. Ano, v předchozím příkladu hello, nastaví hodnotu nastavení aplikace hello příliš`src`. Pokud jsou funkcí v hello kořenového úložiště nebo pokud nejsou nasazení od správy zdrojového kódu, můžete odebrat tuto hodnotu nastavení aplikace.

## <a name="deploy-your-template"></a>Nasazení šablony

Můžete použít některou z následujících způsobů toodeploy hello šablony:

* [PowerShell](../azure-resource-manager/resource-group-template-deploy.md)
* [Azure CLI](../azure-resource-manager/resource-group-template-deploy-cli.md)
* [Azure Portal](../azure-resource-manager/resource-group-template-deploy-portal.md)
* [REST API](../azure-resource-manager/resource-group-template-deploy-rest.md)

### <a name="deploy-tooazure-button"></a>TooAzure tlačítko nasadit

Nahraďte ```<url-encoded-path-to-azuredeploy-json>``` s [kódovaná adresou URL](https://www.bing.com/search?q=url+encode) verzi hello nezpracovaná cesta vaší `azuredeploy.json` souboru na Githubu.

Tady je příklad, který používá markdownu:

```markdown
[![Deploy tooAzure](http://azuredeploy.net/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/<url-encoded-path-to-azuredeploy-json>)
```

Tady je příklad, který používá HTML:

```html
<a href="https://portal.azure.com/#create/Microsoft.Template/uri/<url-encoded-path-to-azuredeploy-json>" target="_blank"><img src="http://azuredeploy.net/deploybutton.png"></a>
```

## <a name="next-steps"></a>Další kroky

Další informace o toodevelop a konfigurace Azure Functions.

* [Referenční informace pro vývojáře Azure Functions](functions-reference.md)
* [Jak tooconfigure Azure fungovat nastavení aplikace](functions-how-to-use-azure-function-app-settings.md)
* [Vytvoření první funkce Azure](functions-create-first-azure-function.md)

<!-- LINKS -->

[funkce aplikace v plánu spotřeby]: https://github.com/Azure/azure-quickstart-templates/blob/master/101-function-app-create-dynamic/azuredeploy.json
[funkce aplikace na plán služby Azure App Service]: https://github.com/Azure/azure-quickstart-templates/blob/master/101-function-app-create-dedicated/azuredeploy.json
