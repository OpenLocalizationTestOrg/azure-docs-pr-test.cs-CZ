---
title: "aaaDeploy prostředků pomocí rozhraní API REST a šablony | Microsoft Docs"
description: "Pomocí Azure Resource Manageru a REST API Resource Manageru toodeploy tooAzure prostředky. Hello prostředky jsou definovány v šabloně Resource Manager."
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
ms.openlocfilehash: 347ad3bdb604429e7291297d448688204af69b46
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="deploy-resources-with-resource-manager-templates-and-resource-manager-rest-api"></a><span data-ttu-id="e1f74-104">Nasazení prostředků pomocí šablon Resource Manageru a jeho rozhraní REST API</span><span class="sxs-lookup"><span data-stu-id="e1f74-104">Deploy resources with Resource Manager templates and Resource Manager REST API</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="e1f74-105">PowerShell</span><span class="sxs-lookup"><span data-stu-id="e1f74-105">PowerShell</span></span>](resource-group-template-deploy.md)
> * [<span data-ttu-id="e1f74-106">Azure CLI</span><span class="sxs-lookup"><span data-stu-id="e1f74-106">Azure CLI</span></span>](resource-group-template-deploy-cli.md)
> * [<span data-ttu-id="e1f74-107">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="e1f74-107">Portal</span></span>](resource-group-template-deploy-portal.md)
> * [<span data-ttu-id="e1f74-108">REST API</span><span class="sxs-lookup"><span data-stu-id="e1f74-108">REST API</span></span>](resource-group-template-deploy-rest.md)
> 
> 

<span data-ttu-id="e1f74-109">Tento článek vysvětluje, jak toouse hello REST API Resource Manager pomocí Správce prostředků šablony toodeploy tooAzure vaše prostředky.</span><span class="sxs-lookup"><span data-stu-id="e1f74-109">This article explains how toouse hello Resource Manager REST API with Resource Manager templates toodeploy your resources tooAzure.</span></span>  

> [!TIP]
> <span data-ttu-id="e1f74-110">Pomoc s laděním chybu během nasazení najdete v tématu:</span><span class="sxs-lookup"><span data-stu-id="e1f74-110">For help with debugging an error during deployment, see:</span></span>
> 
> * <span data-ttu-id="e1f74-111">[Zobrazit operace nasazení](resource-manager-deployment-operations.md) toolearn o získání informací, která vám pomůže vyřešit vaše chybu</span><span class="sxs-lookup"><span data-stu-id="e1f74-111">[View deployment operations](resource-manager-deployment-operations.md) toolearn about getting information that helps you troubleshoot your error</span></span>
> * <span data-ttu-id="e1f74-112">[Odstraňování běžných chyb při nasazování tooAzure prostředků pomocí Azure Resource Manageru](resource-manager-common-deployment-errors.md) toolearn jak tooresolve běžné chyby nasazení</span><span class="sxs-lookup"><span data-stu-id="e1f74-112">[Troubleshoot common errors when deploying resources tooAzure with Azure Resource Manager](resource-manager-common-deployment-errors.md) toolearn how tooresolve common deployment errors</span></span>
> 
> 

<span data-ttu-id="e1f74-113">Šablony může být místní soubor nebo externí soubor, který je k dispozici prostřednictvím identifikátoru URI.</span><span class="sxs-lookup"><span data-stu-id="e1f74-113">Your template can be either a local file or an external file that is available through a URI.</span></span> <span data-ttu-id="e1f74-114">Pokud vaše šablona se nachází v účtu úložiště, můžete omezit přístup toohello šablony a během nasazení zadat token sdílený přístupový podpis (SAS).</span><span class="sxs-lookup"><span data-stu-id="e1f74-114">When your template resides in a storage account, you can restrict access toohello template and provide a shared access signature (SAS) token during deployment.</span></span>

[!INCLUDE [resource-manager-deployments](../../includes/resource-manager-deployments.md)]

## <a name="deploy-with-hello-rest-api"></a><span data-ttu-id="e1f74-115">Nasazení s hello REST API</span><span class="sxs-lookup"><span data-stu-id="e1f74-115">Deploy with hello REST API</span></span>
1. <span data-ttu-id="e1f74-116">Nastavit [společné parametry a hlavičky](https://docs.microsoft.com/rest/api/index), včetně ověřování tokenů.</span><span class="sxs-lookup"><span data-stu-id="e1f74-116">Set [common parameters and headers](https://docs.microsoft.com/rest/api/index), including authentication tokens.</span></span>
2. <span data-ttu-id="e1f74-117">Pokud nemáte existující skupinu prostředků, vytvořte skupinu prostředků.</span><span class="sxs-lookup"><span data-stu-id="e1f74-117">If you do not have an existing resource group, create a resource group.</span></span> <span data-ttu-id="e1f74-118">Zadejte ID předplatného, název hello hello novou skupinu prostředků a umístění, které potřebujete pro vaše řešení.</span><span class="sxs-lookup"><span data-stu-id="e1f74-118">Provide your subscription ID, hello name of hello new resource group, and location that you need for your solution.</span></span> <span data-ttu-id="e1f74-119">Další informace najdete v tématu [vytvořte skupinu prostředků](https://docs.microsoft.com/rest/api/resources/resourcegroups#ResourceGroups_CreateOrUpdate).</span><span class="sxs-lookup"><span data-stu-id="e1f74-119">For more information, see [Create a resource group](https://docs.microsoft.com/rest/api/resources/resourcegroups#ResourceGroups_CreateOrUpdate).</span></span>
   
        PUT https://management.azure.com/subscriptions/<YourSubscriptionId>/resourcegroups/<YourResourceGroupName>?api-version=2015-01-01
          <common headers>
          {
            "location": "West US",
            "tags": {
               "tagname1": "tagvalue1"
            }
          }
3. <span data-ttu-id="e1f74-120">Ověření nasazení před provedením spuštěním hello [ověření nasazení šablony](https://docs.microsoft.com/rest/api/resources/deployments#Deployments_Validate) operaci.</span><span class="sxs-lookup"><span data-stu-id="e1f74-120">Validate your deployment before executing it by running hello [Validate a template deployment](https://docs.microsoft.com/rest/api/resources/deployments#Deployments_Validate) operation.</span></span> <span data-ttu-id="e1f74-121">Při testování hello nasazení, zadejte parametry přesně stejně jako při provádění nasazení hello (zobrazené v dalším kroku hello).</span><span class="sxs-lookup"><span data-stu-id="e1f74-121">When testing hello deployment, provide parameters exactly as you would when executing hello deployment (shown in hello next step).</span></span>
4. <span data-ttu-id="e1f74-122">Vytvořte nasazení.</span><span class="sxs-lookup"><span data-stu-id="e1f74-122">Create a deployment.</span></span> <span data-ttu-id="e1f74-123">Zadejte svoje ID předplatného, hello název skupiny prostředků hello, název hello hello nasazení a šablonu tooyour odkaz.</span><span class="sxs-lookup"><span data-stu-id="e1f74-123">Provide your subscription ID, hello name of hello resource group, hello name of hello deployment, and a link tooyour template.</span></span> <span data-ttu-id="e1f74-124">Informace o souboru šablony hello najdete v tématu [soubor parametrů](#parameter-file).</span><span class="sxs-lookup"><span data-stu-id="e1f74-124">For information about hello template file, see [Parameter file](#parameter-file).</span></span> <span data-ttu-id="e1f74-125">Další informace o rozhraní REST API toocreate hello skupinu prostředků najdete v tématu [vytvořit šablonu nasazení](https://docs.microsoft.com/rest/api/resources/deployments#Deployments_CreateOrUpdate).</span><span class="sxs-lookup"><span data-stu-id="e1f74-125">For more information about hello REST API toocreate a resource group, see [Create a template deployment](https://docs.microsoft.com/rest/api/resources/deployments#Deployments_CreateOrUpdate).</span></span> <span data-ttu-id="e1f74-126">Všimněte si hello **režimu** je nastaven příliš**přírůstkové**.</span><span class="sxs-lookup"><span data-stu-id="e1f74-126">Notice hello **mode** is set too**Incremental**.</span></span> <span data-ttu-id="e1f74-127">toorun dokončení nasazení, nastavte **režimu** příliš**Complete**.</span><span class="sxs-lookup"><span data-stu-id="e1f74-127">toorun a complete deployment, set **mode** too**Complete**.</span></span> <span data-ttu-id="e1f74-128">Dávejte pozor při použití režimu dokončení hello jako nechtěně může odstranit prostředky, které nejsou ve vaší šabloně.</span><span class="sxs-lookup"><span data-stu-id="e1f74-128">Be careful when using hello complete mode as you can inadvertently delete resources that are not in your template.</span></span>
   
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
   
      <span data-ttu-id="e1f74-129">Pokud chcete obsah odpovědi toolog, obsah žádosti nebo obojí, zahrnují **debugSetting** v žádosti o hello.</span><span class="sxs-lookup"><span data-stu-id="e1f74-129">If you want toolog response content, request content, or both, include **debugSetting** in hello request.</span></span>
   
        "debugSetting": {
          "detailLevel": "requestContent, responseContent"
        }
   
      <span data-ttu-id="e1f74-130">Můžete nastavit vaše toouse účet úložiště token sdílený přístupový podpis (SAS).</span><span class="sxs-lookup"><span data-stu-id="e1f74-130">You can set up your storage account toouse a shared access signature (SAS) token.</span></span> <span data-ttu-id="e1f74-131">Další informace najdete v tématu [delegování přístupu k pomocí sdíleného přístupového podpisu](https://docs.microsoft.com/rest/api/storageservices/delegating-access-with-a-shared-access-signature).</span><span class="sxs-lookup"><span data-stu-id="e1f74-131">For more information, see [Delegating Access with a Shared Access Signature](https://docs.microsoft.com/rest/api/storageservices/delegating-access-with-a-shared-access-signature).</span></span>
5. <span data-ttu-id="e1f74-132">Získáte stav hello hello šablonu nasazení.</span><span class="sxs-lookup"><span data-stu-id="e1f74-132">Get hello status of hello template deployment.</span></span> <span data-ttu-id="e1f74-133">Další informace najdete v tématu [získat informace o nasazení šablony](https://docs.microsoft.com/rest/api/resources/deployments#Deployments_Get).</span><span class="sxs-lookup"><span data-stu-id="e1f74-133">For more information, see [Get information about a template deployment](https://docs.microsoft.com/rest/api/resources/deployments#Deployments_Get).</span></span>
   
          GET https://management.azure.com/subscriptions/<YourSubscriptionId>/resourcegroups/<YourResourceGroupName>/providers/Microsoft.Resources/deployments/<YourDeploymentName>?api-version=2015-01-01
           <common headers>

## <a name="parameter-file"></a><span data-ttu-id="e1f74-134">Soubor parametrů</span><span class="sxs-lookup"><span data-stu-id="e1f74-134">Parameter file</span></span>

[!INCLUDE [resource-manager-parameter-file](../../includes/resource-manager-parameter-file.md)]

## <a name="next-steps"></a><span data-ttu-id="e1f74-135">Další kroky</span><span class="sxs-lookup"><span data-stu-id="e1f74-135">Next steps</span></span>
* <span data-ttu-id="e1f74-136">toolearn o zpracování asynchronní operace REST, najdete v části [sledovat asynchronní operace Azure](resource-manager-async-operations.md).</span><span class="sxs-lookup"><span data-stu-id="e1f74-136">toolearn about handling asynchronous REST operations, see [Track asynchronous Azure operations](resource-manager-async-operations.md).</span></span>
* <span data-ttu-id="e1f74-137">Příklad nasazení prostředků prostřednictvím klientské knihovny hello .NET, naleznete v části [nasazení prostředků s použitím knihovny .NET a šablonu](../virtual-machines/windows/csharp-template.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="e1f74-137">For an example of deploying resources through hello .NET client library, see [Deploy resources using .NET libraries and a template](../virtual-machines/windows/csharp-template.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span>
* <span data-ttu-id="e1f74-138">toodefine parametry v šabloně, najdete v části [vytváření šablon](resource-group-authoring-templates.md#parameters).</span><span class="sxs-lookup"><span data-stu-id="e1f74-138">toodefine parameters in template, see [Authoring templates](resource-group-authoring-templates.md#parameters).</span></span>
* <span data-ttu-id="e1f74-139">Pokyny k nasazení vašeho prostředí toodifferent řešení najdete v tématu [vývojová a testovací prostředí v Microsoft Azure](solution-dev-test-environments.md).</span><span class="sxs-lookup"><span data-stu-id="e1f74-139">For guidance on deploying your solution toodifferent environments, see [Development and test environments in Microsoft Azure](solution-dev-test-environments.md).</span></span>
* <span data-ttu-id="e1f74-140">Pokyny k použití Resource Manager tooeffectively podniky můžou spravovat předplatná najdete v tématu [Azure enterprise vygenerované uživatelské rozhraní – zásady správného řízení doporučený předplatné](resource-manager-subscription-governance.md).</span><span class="sxs-lookup"><span data-stu-id="e1f74-140">For guidance on how enterprises can use Resource Manager tooeffectively manage subscriptions, see [Azure enterprise scaffold - prescriptive subscription governance](resource-manager-subscription-governance.md).</span></span>

