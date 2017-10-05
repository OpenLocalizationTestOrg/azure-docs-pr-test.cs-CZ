---
title: "Vytvoření oboru názvů Service Bus pomocí šablony Azure Resource Manager | Microsoft Docs"
description: "Vytvoření oboru názvů Service Bus pomocí šablony Azure Resource Manageru"
services: service-bus-messaging
documentationcenter: .net
author: sethmanheim
manager: timlt
editor: 
ms.assetid: dc0d6482-6344-4cef-8644-d4573639f5e4
ms.service: service-bus-messaging
ms.devlang: tbd
ms.topic: article
ms.tgt_pltfrm: dotnet
ms.workload: na
ms.date: 08/07/2017
ms.author: sethm;shvija
ms.openlocfilehash: 8fff390919a1807995646dab322b4cbe56dd0268
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="create-a-service-bus-namespace-using-an-azure-resource-manager-template"></a><span data-ttu-id="cf14b-103">Vytvoření oboru názvů Service Bus pomocí šablony Azure Resource Manager</span><span class="sxs-lookup"><span data-stu-id="cf14b-103">Create a Service Bus namespace using an Azure Resource Manager template</span></span>

<span data-ttu-id="cf14b-104">Tento článek popisuje, jak používat šablonu Azure Resource Manager, která vytvoří obor názvů sběrnice typu **zasílání zpráv** s Standard a základní SKU.</span><span class="sxs-lookup"><span data-stu-id="cf14b-104">This article describes how to use an Azure Resource Manager template that creates a Service Bus namespace of type **Messaging** with a Standard/Basic SKU.</span></span> <span data-ttu-id="cf14b-105">V článku také definuje parametry, které jsou určené pro spuštění nasazení.</span><span class="sxs-lookup"><span data-stu-id="cf14b-105">The article also defines the parameters that are specified for the execution of the deployment.</span></span> <span data-ttu-id="cf14b-106">Tuto šablonu můžete použít pro vlastní nasazení nebo ji upravit, aby splňovala vaše požadavky.</span><span class="sxs-lookup"><span data-stu-id="cf14b-106">You can use this template for your own deployments, or customize it to meet your requirements.</span></span>

<span data-ttu-id="cf14b-107">Další informace o vytváření šablon najdete v tématu [šablon pro tvorbu Azure Resource Manageru][Authoring Azure Resource Manager templates].</span><span class="sxs-lookup"><span data-stu-id="cf14b-107">For more information about creating templates, please see [Authoring Azure Resource Manager templates][Authoring Azure Resource Manager templates].</span></span>

<span data-ttu-id="cf14b-108">Úplnou šablonu, najdete v článku [šablony oboru názvů Service Bus] [ Service Bus namespace template] na Githubu.</span><span class="sxs-lookup"><span data-stu-id="cf14b-108">For the complete template, see the [Service Bus namespace template][Service Bus namespace template] on GitHub.</span></span>

> [!NOTE]
> <span data-ttu-id="cf14b-109">Následující šablony Azure Resource Manager jsou k dispozici ke stažení a nasazení.</span><span class="sxs-lookup"><span data-stu-id="cf14b-109">The following Azure Resource Manager templates are available for download and deployment.</span></span> 
> 
> * [<span data-ttu-id="cf14b-110">Vytvoření oboru názvů Service Bus pomocí fronty</span><span class="sxs-lookup"><span data-stu-id="cf14b-110">Create a Service Bus namespace with queue</span></span>](service-bus-resource-manager-namespace-queue.md)
> * [<span data-ttu-id="cf14b-111">Vytvoření oboru názvů Service Bus s téma a odběr</span><span class="sxs-lookup"><span data-stu-id="cf14b-111">Create a Service Bus namespace with topic and subscription</span></span>](service-bus-resource-manager-namespace-topic.md)
> * [<span data-ttu-id="cf14b-112">Vytvoření oboru názvů Service Bus pomocí fronty a autorizační pravidla</span><span class="sxs-lookup"><span data-stu-id="cf14b-112">Create a Service Bus namespace with queue and authorization rule</span></span>](service-bus-resource-manager-namespace-auth-rule.md)
> * [<span data-ttu-id="cf14b-113">Vytvoření oboru názvů Service Bus pomocí tématu, předplatné a pravidla</span><span class="sxs-lookup"><span data-stu-id="cf14b-113">Create a Service Bus namespace with topic, subscription, and rule</span></span>](service-bus-resource-manager-namespace-topic-with-rule.md)
> 
> <span data-ttu-id="cf14b-114">Vyhledat nejnovější šablony, najdete [šablon Azure rychlý Start] [ Azure Quickstart Templates] galerie a vyhledejte Service Bus.</span><span class="sxs-lookup"><span data-stu-id="cf14b-114">To check for the latest templates, visit the [Azure Quickstart Templates][Azure Quickstart Templates] gallery and search for Service Bus.</span></span>
> 
> 

## <a name="what-will-you-deploy"></a><span data-ttu-id="cf14b-115">Co budete nasazovat?</span><span class="sxs-lookup"><span data-stu-id="cf14b-115">What will you deploy?</span></span>
<span data-ttu-id="cf14b-116">Pomocí této šablony, kterou nasadíte v oboru názvů Service Bus s [Basic, Standard nebo Premium](https://azure.microsoft.com/pricing/details/service-bus/) SKU.</span><span class="sxs-lookup"><span data-stu-id="cf14b-116">With this template, you will deploy a Service Bus namespace with a [Basic, Standard, or Premium](https://azure.microsoft.com/pricing/details/service-bus/) SKU.</span></span>

<span data-ttu-id="cf14b-117">Pokud chcete nasazení spustit automaticky, klikněte na následující tlačítko:</span><span class="sxs-lookup"><span data-stu-id="cf14b-117">To run the deployment automatically, click the following button:</span></span>

<span data-ttu-id="cf14b-118">[![Nasazení do Azure](./media/service-bus-resource-manager-namespace/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F101-servicebus-create-namespace%2Fazuredeploy.json)</span><span class="sxs-lookup"><span data-stu-id="cf14b-118">[![Deploy to Azure](./media/service-bus-resource-manager-namespace/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F101-servicebus-create-namespace%2Fazuredeploy.json)</span></span>

## <a name="parameters"></a><span data-ttu-id="cf14b-119">Parametry</span><span class="sxs-lookup"><span data-stu-id="cf14b-119">Parameters</span></span>
<span data-ttu-id="cf14b-120">Pomocí Azure Resource Manageru definujete parametry pro hodnoty, které chcete zadat při nasazení šablony.</span><span class="sxs-lookup"><span data-stu-id="cf14b-120">With Azure Resource Manager, you define parameters for values you want to specify when the template is deployed.</span></span> <span data-ttu-id="cf14b-121">Šablona obsahuje oddíl s názvem `Parameters` obsahující všechny hodnoty parametru.</span><span class="sxs-lookup"><span data-stu-id="cf14b-121">The template includes a section called `Parameters` that contains all of the parameter values.</span></span> <span data-ttu-id="cf14b-122">Měli byste parametr pro ty hodnoty, které se liší podle prostředí, ve kterém provádíte nasazení nebo na základě projektu, které nasazujete.</span><span class="sxs-lookup"><span data-stu-id="cf14b-122">You should define a parameter for those values that will vary based on the project you are deploying or based on the environment you are deploying to.</span></span> <span data-ttu-id="cf14b-123">Nedefinují parametry pro hodnoty, které zůstanou vždy stejná.</span><span class="sxs-lookup"><span data-stu-id="cf14b-123">Do not define parameters for values that will always stay the same.</span></span> <span data-ttu-id="cf14b-124">Každá hodnota parametru se v šabloně použije k definování nasazovaných prostředků.</span><span class="sxs-lookup"><span data-stu-id="cf14b-124">Each parameter value is used in the template to define the resources that are deployed.</span></span>

<span data-ttu-id="cf14b-125">Tato šablona definuje následující parametry.</span><span class="sxs-lookup"><span data-stu-id="cf14b-125">This template defines the following parameters.</span></span>

### <a name="servicebusnamespacename"></a><span data-ttu-id="cf14b-126">serviceBusNamespaceName</span><span class="sxs-lookup"><span data-stu-id="cf14b-126">serviceBusNamespaceName</span></span>
<span data-ttu-id="cf14b-127">Název oboru názvů Service Bus k vytvoření.</span><span class="sxs-lookup"><span data-stu-id="cf14b-127">The name of the Service Bus namespace to create.</span></span>

```json
"serviceBusNamespaceName": {
"type": "string",
"metadata": { 
    "description": "Name of the Service Bus namespace" 
    }
}
```

### <a name="servicebussku"></a><span data-ttu-id="cf14b-128">serviceBusSKU</span><span class="sxs-lookup"><span data-stu-id="cf14b-128">serviceBusSKU</span></span>
<span data-ttu-id="cf14b-129">Název služby Service Bus [SKU](https://azure.microsoft.com/pricing/details/service-bus/) k vytvoření.</span><span class="sxs-lookup"><span data-stu-id="cf14b-129">The name of the Service Bus [SKU](https://azure.microsoft.com/pricing/details/service-bus/) to create.</span></span>

```json
"serviceBusSku": { 
    "type": "string", 
    "allowedValues": [ 
        "Basic", 
        "Standard",
        "Premium" 
    ], 
    "defaultValue": "Standard", 
    "metadata": { 
        "description": "The messaging tier for service Bus namespace" 
    } 

```

<span data-ttu-id="cf14b-130">Šablona definuje hodnoty, které jsou povoleny pro tento parametr (Basic, Standard nebo Premium) a přiřadí výchozí hodnotu (Standard), pokud není zadaná žádná hodnota.</span><span class="sxs-lookup"><span data-stu-id="cf14b-130">The template defines the values that are permitted for this parameter (Basic, Standard, or Premium) and assigns a default value (Standard) if no value is specified.</span></span>

<span data-ttu-id="cf14b-131">Další informace o cenách služby Service Bus najdete v tématu [Service Bus ceny a fakturace][Service Bus pricing and billing].</span><span class="sxs-lookup"><span data-stu-id="cf14b-131">For more information about Service Bus pricing, see [Service Bus pricing and billing][Service Bus pricing and billing].</span></span>

### <a name="servicebusapiversion"></a><span data-ttu-id="cf14b-132">serviceBusApiVersion</span><span class="sxs-lookup"><span data-stu-id="cf14b-132">serviceBusApiVersion</span></span>
<span data-ttu-id="cf14b-133">Verze rozhraní API služby Service Bus šablony.</span><span class="sxs-lookup"><span data-stu-id="cf14b-133">The Service Bus API version of the template.</span></span>

```json
"serviceBusApiVersion": { 
       "type": "string", 
       "defaultValue": "2015-08-01", 
       "metadata": { 
           "description": "Service Bus ApiVersion used by the template" 
       } 
```

## <a name="resources-to-deploy"></a><span data-ttu-id="cf14b-134">Prostředky k nasazení</span><span class="sxs-lookup"><span data-stu-id="cf14b-134">Resources to deploy</span></span>
### <a name="service-bus-namespace"></a><span data-ttu-id="cf14b-135">Obor názvů Service Bus</span><span class="sxs-lookup"><span data-stu-id="cf14b-135">Service Bus namespace</span></span>
<span data-ttu-id="cf14b-136">Vytvoří standardní oboru názvů Service Bus typu **zasílání zpráv**.</span><span class="sxs-lookup"><span data-stu-id="cf14b-136">Creates a standard Service Bus namespace of type **Messaging**.</span></span>

```json
"resources": [
    {
        "apiVersion": "[parameters('serviceBusApiVersion')]",
        "name": "[parameters('serviceBusNamespaceName')]",
        "type": "Microsoft.ServiceBus/Namespaces",
        "location": "[variables('location')]",
        "kind": "Messaging",
        "sku": {
            "name": "StandardSku",
            "tier": "Standard"
        },
        "properties": {
        }
    }
]
```

## <a name="commands-to-run-deployment"></a><span data-ttu-id="cf14b-137">Příkazy pro spuštění nasazení</span><span class="sxs-lookup"><span data-stu-id="cf14b-137">Commands to run deployment</span></span>
[!INCLUDE [app-service-deploy-commands](../../includes/app-service-deploy-commands.md)]

### <a name="powershell"></a><span data-ttu-id="cf14b-138">PowerShell</span><span class="sxs-lookup"><span data-stu-id="cf14b-138">PowerShell</span></span>
```powershell
New-AzureRmResourceGroupDeployment -ResourceGroupName <resource-group-name> -TemplateFile https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/101-servicebus-create-namespace/azuredeploy.json
```

### <a name="azure-cli"></a><span data-ttu-id="cf14b-139">Azure CLI</span><span class="sxs-lookup"><span data-stu-id="cf14b-139">Azure CLI</span></span>
```azurecli
azure config mode arm

azure group deployment create <my-resource-group> <my-deployment-name> --template-uri https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/101-servicebus-create-namespace/azuredeploy.json
```

## <a name="next-steps"></a><span data-ttu-id="cf14b-140">Další kroky</span><span class="sxs-lookup"><span data-stu-id="cf14b-140">Next steps</span></span>
<span data-ttu-id="cf14b-141">Teď, když jste vytvoření a nasazení prostředků pomocí Azure Resource Manager, zjistěte, jak tyto zdroje spravovat pomocí čtení těchto článcích:</span><span class="sxs-lookup"><span data-stu-id="cf14b-141">Now that you've created and deployed resources using Azure Resource Manager, learn how to manage these resources by reading these articles:</span></span>

* [<span data-ttu-id="cf14b-142">Správa služby Service Bus pomocí prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="cf14b-142">Manage Service Bus with PowerShell</span></span>](service-bus-manage-with-ps.md)
* [<span data-ttu-id="cf14b-143">Správa prostředků služby Service Bus pomocí Průzkumníka služby sběrnice</span><span class="sxs-lookup"><span data-stu-id="cf14b-143">Manage Service Bus resources with the Service Bus Explorer</span></span>](https://github.com/paolosalvatori/ServiceBusExplorer/releases)

[Authoring Azure Resource Manager templates]: ../azure-resource-manager/resource-group-authoring-templates.md
[Service Bus namespace template]: https://github.com/Azure/azure-quickstart-templates/blob/master/101-servicebus-create-namespace/
[Azure Quickstart Templates]: https://azure.microsoft.com/documentation/templates/?term=service+bus
[Service Bus pricing and billing]: service-bus-pricing-billing.md
[Using Azure PowerShell with Azure Resource Manager]: ../azure-resource-manager/powershell-azure-resource-manager.md
[Using the Azure CLI for Mac, Linux, and Windows with Azure Resource Management]: ../azure-resource-manager/xplat-cli-azure-resource-manager.md
