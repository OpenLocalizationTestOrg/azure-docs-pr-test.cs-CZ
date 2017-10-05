---
title: "Nasazení prostředků pomocí rozhraní API REST a šablony | Microsoft Docs"
description: "Nasazení prostředky do Azure pomocí Azure Resource Manageru a REST API Resource Manageru. Prostředky jsou definovány v šabloně Resource Manageru."
services: azure-resource-manager
documentationcenter: na
author: tfitzmac
manager: timlt
editor: tysonn
ms.assetid: 1d8fbd4c-78b0-425b-ba76-f2b7fd260b45
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/10/2017
ms.author: tomfitz
ms.openlocfilehash: 46856a25fb57bb2c5a3c1aeae13c11655e1a58a5
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="deploy-resources-with-resource-manager-templates-and-resource-manager-rest-api"></a><span data-ttu-id="68fa8-104">Nasazení prostředků pomocí šablon Resource Manageru a jeho rozhraní REST API</span><span class="sxs-lookup"><span data-stu-id="68fa8-104">Deploy resources with Resource Manager templates and Resource Manager REST API</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="68fa8-105">PowerShell</span><span class="sxs-lookup"><span data-stu-id="68fa8-105">PowerShell</span></span>](resource-group-template-deploy.md)
> * [<span data-ttu-id="68fa8-106">Azure CLI</span><span class="sxs-lookup"><span data-stu-id="68fa8-106">Azure CLI</span></span>](resource-group-template-deploy-cli.md)
> * [<span data-ttu-id="68fa8-107">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="68fa8-107">Portal</span></span>](resource-group-template-deploy-portal.md)
> * [<span data-ttu-id="68fa8-108">REST API</span><span class="sxs-lookup"><span data-stu-id="68fa8-108">REST API</span></span>](resource-group-template-deploy-rest.md)
> 
> 

<span data-ttu-id="68fa8-109">Tento článek vysvětluje, jak používat rozhraní REST API služby Správce prostředků pomocí šablony Resource Manageru k nasazení vašich prostředků Azure.</span><span class="sxs-lookup"><span data-stu-id="68fa8-109">This article explains how to use the Resource Manager REST API with Resource Manager templates to deploy your resources to Azure.</span></span>  

> [!TIP]
> <span data-ttu-id="68fa8-110">Pomoc s laděním chybu během nasazení najdete v tématu:</span><span class="sxs-lookup"><span data-stu-id="68fa8-110">For help with debugging an error during deployment, see:</span></span>
> 
> * <span data-ttu-id="68fa8-111">[Zobrazit operace nasazení](resource-manager-deployment-operations.md) Další informace o získání informací, která pomáhá odstranění vaší chyby</span><span class="sxs-lookup"><span data-stu-id="68fa8-111">[View deployment operations](resource-manager-deployment-operations.md) to learn about getting information that helps you troubleshoot your error</span></span>
> * <span data-ttu-id="68fa8-112">[Odstraňování běžných chyb při nasazování prostředků do Azure pomocí Azure Resource Manageru](resource-manager-common-deployment-errors.md) Další informace o řešení běžných chyb při nasazení</span><span class="sxs-lookup"><span data-stu-id="68fa8-112">[Troubleshoot common errors when deploying resources to Azure with Azure Resource Manager](resource-manager-common-deployment-errors.md) to learn how to resolve common deployment errors</span></span>
> 
> 

<span data-ttu-id="68fa8-113">Šablony může být místní soubor nebo externí soubor, který je k dispozici prostřednictvím identifikátoru URI.</span><span class="sxs-lookup"><span data-stu-id="68fa8-113">Your template can be either a local file or an external file that is available through a URI.</span></span> <span data-ttu-id="68fa8-114">Pokud vaše šablona se nachází v účtu úložiště, můžete omezit přístup k šabloně a během nasazení zadat token sdílený přístupový podpis (SAS).</span><span class="sxs-lookup"><span data-stu-id="68fa8-114">When your template resides in a storage account, you can restrict access to the template and provide a shared access signature (SAS) token during deployment.</span></span>

[!INCLUDE [resource-manager-deployments](../../includes/resource-manager-deployments.md)]

## <a name="deploy-with-the-rest-api"></a><span data-ttu-id="68fa8-115">Nasazení pomocí rozhraní REST API</span><span class="sxs-lookup"><span data-stu-id="68fa8-115">Deploy with the REST API</span></span>
1. <span data-ttu-id="68fa8-116">Nastavit [společné parametry a hlavičky](https://docs.microsoft.com/rest/api/index), včetně ověřování tokenů.</span><span class="sxs-lookup"><span data-stu-id="68fa8-116">Set [common parameters and headers](https://docs.microsoft.com/rest/api/index), including authentication tokens.</span></span>
2. <span data-ttu-id="68fa8-117">Pokud nemáte existující skupinu prostředků, vytvořte skupinu prostředků.</span><span class="sxs-lookup"><span data-stu-id="68fa8-117">If you do not have an existing resource group, create a resource group.</span></span> <span data-ttu-id="68fa8-118">Zadejte svoje ID předplatného, název nové skupiny prostředků a umístění, které potřebujete pro vaše řešení.</span><span class="sxs-lookup"><span data-stu-id="68fa8-118">Provide your subscription ID, the name of the new resource group, and location that you need for your solution.</span></span> <span data-ttu-id="68fa8-119">Další informace najdete v tématu [vytvořte skupinu prostředků](https://docs.microsoft.com/rest/api/resources/resourcegroups#ResourceGroups_CreateOrUpdate).</span><span class="sxs-lookup"><span data-stu-id="68fa8-119">For more information, see [Create a resource group](https://docs.microsoft.com/rest/api/resources/resourcegroups#ResourceGroups_CreateOrUpdate).</span></span>
   
        PUT https://management.azure.com/subscriptions/<YourSubscriptionId>/resourcegroups/<YourResourceGroupName>?api-version=2015-01-01
          <common headers>
          {
            "location": "West US",
            "tags": {
               "tagname1": "tagvalue1"
            }
          }
3. <span data-ttu-id="68fa8-120">Ověření nasazení před provedením spuštěním [ověření nasazení šablony](https://docs.microsoft.com/rest/api/resources/deployments#Deployments_Validate) operaci.</span><span class="sxs-lookup"><span data-stu-id="68fa8-120">Validate your deployment before executing it by running the [Validate a template deployment](https://docs.microsoft.com/rest/api/resources/deployments#Deployments_Validate) operation.</span></span> <span data-ttu-id="68fa8-121">Při testování nasazení, zadejte parametry přesně stejně jako při provádění nasazení (zobrazené v dalším kroku).</span><span class="sxs-lookup"><span data-stu-id="68fa8-121">When testing the deployment, provide parameters exactly as you would when executing the deployment (shown in the next step).</span></span>
4. <span data-ttu-id="68fa8-122">Vytvořte nasazení.</span><span class="sxs-lookup"><span data-stu-id="68fa8-122">Create a deployment.</span></span> <span data-ttu-id="68fa8-123">Zadejte svoje ID předplatného, název skupiny prostředků, název nasazení a odkaz na šablonu.</span><span class="sxs-lookup"><span data-stu-id="68fa8-123">Provide your subscription ID, the name of the resource group, the name of the deployment, and a link to your template.</span></span> <span data-ttu-id="68fa8-124">Informace o souboru šablony najdete v tématu [soubor parametrů](#parameter-file).</span><span class="sxs-lookup"><span data-stu-id="68fa8-124">For information about the template file, see [Parameter file](#parameter-file).</span></span> <span data-ttu-id="68fa8-125">Další informace o rozhraní REST API pro vytvoření skupiny prostředků najdete v tématu [vytvořit šablonu nasazení](https://docs.microsoft.com/rest/api/resources/deployments#Deployments_CreateOrUpdate).</span><span class="sxs-lookup"><span data-stu-id="68fa8-125">For more information about the REST API to create a resource group, see [Create a template deployment](https://docs.microsoft.com/rest/api/resources/deployments#Deployments_CreateOrUpdate).</span></span> <span data-ttu-id="68fa8-126">Upozornění **režimu** je nastaven na **přírůstkové**.</span><span class="sxs-lookup"><span data-stu-id="68fa8-126">Notice the **mode** is set to **Incremental**.</span></span> <span data-ttu-id="68fa8-127">Chcete-li spustit kompletní nasazení, nastavte **režimu** k **Complete**.</span><span class="sxs-lookup"><span data-stu-id="68fa8-127">To run a complete deployment, set **mode** to **Complete**.</span></span> <span data-ttu-id="68fa8-128">Buďte opatrní při používání dokončení režimu jako nechtěně může odstranit prostředky, které nejsou ve vaší šabloně.</span><span class="sxs-lookup"><span data-stu-id="68fa8-128">Be careful when using the complete mode as you can inadvertently delete resources that are not in your template.</span></span>
   
        PUT https://management.azure.com/subscriptions/<YourSubscriptionId>/resourcegroups/<YourResourceGroupName>/providers/Microsoft.Resources/deployments/<YourDeploymentName>?api-version=2015-01-01
          <common headers>
          {
            "properties": {
              "templateLink": {
                "uri": "http://mystorageaccount.blob.core.windows.net/templates/template.json",
                "contentVersion": "1.0.0.0"
              },
              "mode": "Incremental",
              "parametersLink": {
                "uri": "http://mystorageaccount.blob.core.windows.net/templates/parameters.json",
                "contentVersion": "1.0.0.0"
              }
            }
          }
   
      <span data-ttu-id="68fa8-129">Pokud chcete protokolovat obsah odpovědi, obsah žádosti nebo obojí, zahrnout **debugSetting** v požadavku.</span><span class="sxs-lookup"><span data-stu-id="68fa8-129">If you want to log response content, request content, or both, include **debugSetting** in the request.</span></span>
   
        "debugSetting": {
          "detailLevel": "requestContent, responseContent"
        }
   
      <span data-ttu-id="68fa8-130">Můžete nastavit účtu úložiště používat token sdílený přístupový podpis (SAS).</span><span class="sxs-lookup"><span data-stu-id="68fa8-130">You can set up your storage account to use a shared access signature (SAS) token.</span></span> <span data-ttu-id="68fa8-131">Další informace najdete v tématu [delegování přístupu k pomocí sdíleného přístupového podpisu](https://docs.microsoft.com/rest/api/storageservices/delegating-access-with-a-shared-access-signature).</span><span class="sxs-lookup"><span data-stu-id="68fa8-131">For more information, see [Delegating Access with a Shared Access Signature](https://docs.microsoft.com/rest/api/storageservices/delegating-access-with-a-shared-access-signature).</span></span>
5. <span data-ttu-id="68fa8-132">Získáte stav nasazení šablony.</span><span class="sxs-lookup"><span data-stu-id="68fa8-132">Get the status of the template deployment.</span></span> <span data-ttu-id="68fa8-133">Další informace najdete v tématu [získat informace o nasazení šablony](https://docs.microsoft.com/rest/api/resources/deployments#Deployments_Get).</span><span class="sxs-lookup"><span data-stu-id="68fa8-133">For more information, see [Get information about a template deployment](https://docs.microsoft.com/rest/api/resources/deployments#Deployments_Get).</span></span>
   
          GET https://management.azure.com/subscriptions/<YourSubscriptionId>/resourcegroups/<YourResourceGroupName>/providers/Microsoft.Resources/deployments/<YourDeploymentName>?api-version=2015-01-01
           <common headers>

## <a name="parameter-file"></a><span data-ttu-id="68fa8-134">Soubor parametrů</span><span class="sxs-lookup"><span data-stu-id="68fa8-134">Parameter file</span></span>

[!INCLUDE [resource-manager-parameter-file](../../includes/resource-manager-parameter-file.md)]

## <a name="next-steps"></a><span data-ttu-id="68fa8-135">Další kroky</span><span class="sxs-lookup"><span data-stu-id="68fa8-135">Next steps</span></span>
* <span data-ttu-id="68fa8-136">Další informace o zpracování asynchronní operace REST najdete v tématu [sledovat asynchronní operace Azure](resource-manager-async-operations.md).</span><span class="sxs-lookup"><span data-stu-id="68fa8-136">To learn about handling asynchronous REST operations, see [Track asynchronous Azure operations](resource-manager-async-operations.md).</span></span>
* <span data-ttu-id="68fa8-137">Příklad nasazení prostředků prostřednictvím klientské knihovny .NET, naleznete v části [nasazení prostředků s použitím knihovny .NET a šablonu](../virtual-machines/windows/csharp-template.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="68fa8-137">For an example of deploying resources through the .NET client library, see [Deploy resources using .NET libraries and a template](../virtual-machines/windows/csharp-template.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span>
* <span data-ttu-id="68fa8-138">Chcete-li definovat parametry v šabloně, přečtěte si téma [vytváření šablon](resource-group-authoring-templates.md#parameters).</span><span class="sxs-lookup"><span data-stu-id="68fa8-138">To define parameters in template, see [Authoring templates](resource-group-authoring-templates.md#parameters).</span></span>
* <span data-ttu-id="68fa8-139">Pokyny pro nasazení řešení do různých prostředí najdete v článku věnovaném [testovacím a vývojovým prostředím v Microsoft Azure](solution-dev-test-environments.md).</span><span class="sxs-lookup"><span data-stu-id="68fa8-139">For guidance on deploying your solution to different environments, see [Development and test environments in Microsoft Azure](solution-dev-test-environments.md).</span></span>
* <span data-ttu-id="68fa8-140">Pokyny k tomu, jak můžou podniky používat Resource Manager k efektivní správě předplatných, najdete v části [Základní kostra Azure Enterprise – zásady správného řízení pro předplatná](resource-manager-subscription-governance.md).</span><span class="sxs-lookup"><span data-stu-id="68fa8-140">For guidance on how enterprises can use Resource Manager to effectively manage subscriptions, see [Azure enterprise scaffold - prescriptive subscription governance](resource-manager-subscription-governance.md).</span></span>

