---
title: "obor názvů sběrnice aaaCreate pomocí šablony Azure Resource Manager | Microsoft Docs"
description: "Použít toocreate šablony Azure Resource Manager oboru názvů Service Bus"
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
ms.openlocfilehash: fddf370affe761a734991ae9b60c1e5825e54ef7
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-service-bus-namespace-using-an-azure-resource-manager-template"></a><span data-ttu-id="745fa-103">Vytvoření oboru názvů Service Bus pomocí šablony Azure Resource Manager</span><span class="sxs-lookup"><span data-stu-id="745fa-103">Create a Service Bus namespace using an Azure Resource Manager template</span></span>

<span data-ttu-id="745fa-104">Tento článek popisuje, jak toouse šablonu Azure Resource Manager, který vytvořil oboru názvů Service Bus typu **zasílání zpráv** s Standard a základní SKU.</span><span class="sxs-lookup"><span data-stu-id="745fa-104">This article describes how toouse an Azure Resource Manager template that creates a Service Bus namespace of type **Messaging** with a Standard/Basic SKU.</span></span> <span data-ttu-id="745fa-105">Hello článku také definuje hello parametry, které jsou určené pro provádění hello hello nasazení.</span><span class="sxs-lookup"><span data-stu-id="745fa-105">hello article also defines hello parameters that are specified for hello execution of hello deployment.</span></span> <span data-ttu-id="745fa-106">Můžete tuto šablonu použít pro vlastní nasazení, nebo si ji přizpůsobit toomeet vašim požadavkům.</span><span class="sxs-lookup"><span data-stu-id="745fa-106">You can use this template for your own deployments, or customize it toomeet your requirements.</span></span>

<span data-ttu-id="745fa-107">Další informace o vytváření šablon najdete v tématu [šablon pro tvorbu Azure Resource Manageru][Authoring Azure Resource Manager templates].</span><span class="sxs-lookup"><span data-stu-id="745fa-107">For more information about creating templates, please see [Authoring Azure Resource Manager templates][Authoring Azure Resource Manager templates].</span></span>

<span data-ttu-id="745fa-108">Hello úplnou šablonu, najdete v části hello [šablony oboru názvů Service Bus] [ Service Bus namespace template] na Githubu.</span><span class="sxs-lookup"><span data-stu-id="745fa-108">For hello complete template, see hello [Service Bus namespace template][Service Bus namespace template] on GitHub.</span></span>

> [!NOTE]
> <span data-ttu-id="745fa-109">Hello následující šablony Azure Resource Manager jsou k dispozici ke stažení a nasazení.</span><span class="sxs-lookup"><span data-stu-id="745fa-109">hello following Azure Resource Manager templates are available for download and deployment.</span></span> 
> 
> * [<span data-ttu-id="745fa-110">Vytvoření oboru názvů Service Bus pomocí fronty</span><span class="sxs-lookup"><span data-stu-id="745fa-110">Create a Service Bus namespace with queue</span></span>](service-bus-resource-manager-namespace-queue.md)
> * [<span data-ttu-id="745fa-111">Vytvoření oboru názvů Service Bus s téma a odběr</span><span class="sxs-lookup"><span data-stu-id="745fa-111">Create a Service Bus namespace with topic and subscription</span></span>](service-bus-resource-manager-namespace-topic.md)
> * [<span data-ttu-id="745fa-112">Vytvoření oboru názvů Service Bus pomocí fronty a autorizační pravidla</span><span class="sxs-lookup"><span data-stu-id="745fa-112">Create a Service Bus namespace with queue and authorization rule</span></span>](service-bus-resource-manager-namespace-auth-rule.md)
> * [<span data-ttu-id="745fa-113">Vytvoření oboru názvů Service Bus pomocí tématu, předplatné a pravidla</span><span class="sxs-lookup"><span data-stu-id="745fa-113">Create a Service Bus namespace with topic, subscription, and rule</span></span>](service-bus-resource-manager-namespace-topic-with-rule.md)
> 
> <span data-ttu-id="745fa-114">toocheck pro hello nejnovější šablony, navštivte hello [šablon Azure rychlý Start] [ Azure Quickstart Templates] galerie a vyhledejte Service Bus.</span><span class="sxs-lookup"><span data-stu-id="745fa-114">toocheck for hello latest templates, visit hello [Azure Quickstart Templates][Azure Quickstart Templates] gallery and search for Service Bus.</span></span>
> 
> 

## <a name="what-will-you-deploy"></a><span data-ttu-id="745fa-115">Co budete nasazovat?</span><span class="sxs-lookup"><span data-stu-id="745fa-115">What will you deploy?</span></span>
<span data-ttu-id="745fa-116">Pomocí této šablony, kterou nasadíte v oboru názvů Service Bus s [Basic, Standard nebo Premium](https://azure.microsoft.com/pricing/details/service-bus/) SKU.</span><span class="sxs-lookup"><span data-stu-id="745fa-116">With this template, you will deploy a Service Bus namespace with a [Basic, Standard, or Premium](https://azure.microsoft.com/pricing/details/service-bus/) SKU.</span></span>

<span data-ttu-id="745fa-117">toorun hello nasazení automaticky, klikněte na následující tlačítko hello:</span><span class="sxs-lookup"><span data-stu-id="745fa-117">toorun hello deployment automatically, click hello following button:</span></span>

<span data-ttu-id="745fa-118">[![Nasazení tooAzure](./media/service-bus-resource-manager-namespace/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F101-servicebus-create-namespace%2Fazuredeploy.json)</span><span class="sxs-lookup"><span data-stu-id="745fa-118">[![Deploy tooAzure](./media/service-bus-resource-manager-namespace/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F101-servicebus-create-namespace%2Fazuredeploy.json)</span></span>

## <a name="parameters"></a><span data-ttu-id="745fa-119">Parametry</span><span class="sxs-lookup"><span data-stu-id="745fa-119">Parameters</span></span>
<span data-ttu-id="745fa-120">S Azure Resource Manager, můžete definovat parametry pro hodnoty chcete toospecify při nasazení šablony hello.</span><span class="sxs-lookup"><span data-stu-id="745fa-120">With Azure Resource Manager, you define parameters for values you want toospecify when hello template is deployed.</span></span> <span data-ttu-id="745fa-121">Šablona Hello zahrnuje části s názvem `Parameters` obsahující všechny hodnoty parametru hello.</span><span class="sxs-lookup"><span data-stu-id="745fa-121">hello template includes a section called `Parameters` that contains all of hello parameter values.</span></span> <span data-ttu-id="745fa-122">Měli byste parametr pro ty hodnoty, které se liší podle hello prostředí, které nasazujete nebo na základě hello projektu, které nasazujete.</span><span class="sxs-lookup"><span data-stu-id="745fa-122">You should define a parameter for those values that will vary based on hello project you are deploying or based on hello environment you are deploying to.</span></span> <span data-ttu-id="745fa-123">Parametry nedefinuje pro hodnoty, které vždy zůstanou hello stejné.</span><span class="sxs-lookup"><span data-stu-id="745fa-123">Do not define parameters for values that will always stay hello same.</span></span> <span data-ttu-id="745fa-124">Každá hodnota parametru se používá v hello šablony toodefine hello prostředky, které jsou nasazeny.</span><span class="sxs-lookup"><span data-stu-id="745fa-124">Each parameter value is used in hello template toodefine hello resources that are deployed.</span></span>

<span data-ttu-id="745fa-125">Tato šablona určuje hello následující parametry.</span><span class="sxs-lookup"><span data-stu-id="745fa-125">This template defines hello following parameters.</span></span>

### <a name="servicebusnamespacename"></a><span data-ttu-id="745fa-126">serviceBusNamespaceName</span><span class="sxs-lookup"><span data-stu-id="745fa-126">serviceBusNamespaceName</span></span>
<span data-ttu-id="745fa-127">Název Hello toocreate oboru názvů Service Bus hello.</span><span class="sxs-lookup"><span data-stu-id="745fa-127">hello name of hello Service Bus namespace toocreate.</span></span>

```json
"serviceBusNamespaceName": {
"type": "string",
"metadata": { 
    "description": "Name of hello Service Bus namespace" 
    }
}
```

### <a name="servicebussku"></a><span data-ttu-id="745fa-128">serviceBusSKU</span><span class="sxs-lookup"><span data-stu-id="745fa-128">serviceBusSKU</span></span>
<span data-ttu-id="745fa-129">Název Hello hello Service Bus [SKU](https://azure.microsoft.com/pricing/details/service-bus/) toocreate.</span><span class="sxs-lookup"><span data-stu-id="745fa-129">hello name of hello Service Bus [SKU](https://azure.microsoft.com/pricing/details/service-bus/) toocreate.</span></span>

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
        "description": "hello messaging tier for service Bus namespace" 
    } 

```

<span data-ttu-id="745fa-130">Šablona Hello definuje hello hodnoty, které jsou povoleny pro tento parametr (Basic, Standard nebo Premium) a přiřadí výchozí hodnotu (Standard), pokud není zadaná žádná hodnota.</span><span class="sxs-lookup"><span data-stu-id="745fa-130">hello template defines hello values that are permitted for this parameter (Basic, Standard, or Premium) and assigns a default value (Standard) if no value is specified.</span></span>

<span data-ttu-id="745fa-131">Další informace o cenách služby Service Bus najdete v tématu [Service Bus ceny a fakturace][Service Bus pricing and billing].</span><span class="sxs-lookup"><span data-stu-id="745fa-131">For more information about Service Bus pricing, see [Service Bus pricing and billing][Service Bus pricing and billing].</span></span>

### <a name="servicebusapiversion"></a><span data-ttu-id="745fa-132">serviceBusApiVersion</span><span class="sxs-lookup"><span data-stu-id="745fa-132">serviceBusApiVersion</span></span>
<span data-ttu-id="745fa-133">verze rozhraní API služby Service Bus Hello hello šablony.</span><span class="sxs-lookup"><span data-stu-id="745fa-133">hello Service Bus API version of hello template.</span></span>

```json
"serviceBusApiVersion": { 
       "type": "string", 
       "defaultValue": "2015-08-01", 
       "metadata": { 
           "description": "Service Bus ApiVersion used by hello template" 
       } 
```

## <a name="resources-toodeploy"></a><span data-ttu-id="745fa-134">Toodeploy prostředky</span><span class="sxs-lookup"><span data-stu-id="745fa-134">Resources toodeploy</span></span>
### <a name="service-bus-namespace"></a><span data-ttu-id="745fa-135">Obor názvů Service Bus</span><span class="sxs-lookup"><span data-stu-id="745fa-135">Service Bus namespace</span></span>
<span data-ttu-id="745fa-136">Vytvoří standardní oboru názvů Service Bus typu **zasílání zpráv**.</span><span class="sxs-lookup"><span data-stu-id="745fa-136">Creates a standard Service Bus namespace of type **Messaging**.</span></span>

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

## <a name="commands-toorun-deployment"></a><span data-ttu-id="745fa-137">Příkazy toorun nasazení</span><span class="sxs-lookup"><span data-stu-id="745fa-137">Commands toorun deployment</span></span>
[!INCLUDE [app-service-deploy-commands](../../includes/app-service-deploy-commands.md)]

### <a name="powershell"></a><span data-ttu-id="745fa-138">PowerShell</span><span class="sxs-lookup"><span data-stu-id="745fa-138">PowerShell</span></span>
```powershell
New-AzureRmResourceGroupDeployment -ResourceGroupName <resource-group-name> -TemplateFile https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/101-servicebus-create-namespace/azuredeploy.json
```

### <a name="azure-cli"></a><span data-ttu-id="745fa-139">Azure CLI</span><span class="sxs-lookup"><span data-stu-id="745fa-139">Azure CLI</span></span>
```azurecli
azure config mode arm

azure group deployment create <my-resource-group> <my-deployment-name> --template-uri https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/101-servicebus-create-namespace/azuredeploy.json
```

## <a name="next-steps"></a><span data-ttu-id="745fa-140">Další kroky</span><span class="sxs-lookup"><span data-stu-id="745fa-140">Next steps</span></span>
<span data-ttu-id="745fa-141">Nyní, po vytvoření a nasazení prostředků pomocí Azure Resource Manager, se naučíte, jak toomanage tyto prostředky načtením těchto článcích:</span><span class="sxs-lookup"><span data-stu-id="745fa-141">Now that you've created and deployed resources using Azure Resource Manager, learn how toomanage these resources by reading these articles:</span></span>

* [<span data-ttu-id="745fa-142">Správa služby Service Bus pomocí prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="745fa-142">Manage Service Bus with PowerShell</span></span>](service-bus-manage-with-ps.md)
* [<span data-ttu-id="745fa-143">Správa prostředků služby Service Bus pomocí hello Service Bus Explorer</span><span class="sxs-lookup"><span data-stu-id="745fa-143">Manage Service Bus resources with hello Service Bus Explorer</span></span>](https://github.com/paolosalvatori/ServiceBusExplorer/releases)

[Authoring Azure Resource Manager templates]: ../azure-resource-manager/resource-group-authoring-templates.md
[Service Bus namespace template]: https://github.com/Azure/azure-quickstart-templates/blob/master/101-servicebus-create-namespace/
[Azure Quickstart Templates]: https://azure.microsoft.com/documentation/templates/?term=service+bus
[Service Bus pricing and billing]: service-bus-pricing-billing.md
[Using Azure PowerShell with Azure Resource Manager]: ../azure-resource-manager/powershell-azure-resource-manager.md
[Using hello Azure CLI for Mac, Linux, and Windows with Azure Resource Management]: ../azure-resource-manager/xplat-cli-azure-resource-manager.md
