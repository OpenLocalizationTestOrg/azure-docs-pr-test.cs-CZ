---
title: "Nasazení šablony Azure s tokenu SAS a prostředí PowerShell | Microsoft Docs"
description: "Pomocí Azure Resource Manageru a prostředí Azure PowerShell k nasazení prostředků do Azure ze šablony, která je chráněná pomocí tokenu SAS."
services: azure-resource-manager
documentationcenter: na
author: tfitzmac
manager: timlt
editor: tysonn
ms.assetid: 
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 04/19/2017
ms.author: tomfitz
ms.openlocfilehash: 1e3cea027b599e2b1af1ced0fdf14e2cc8a0db82
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="deploy-private-resource-manager-template-with-sas-token-and-azure-powershell"></a><span data-ttu-id="95773-103">Nasazení privátní šablony Resource Manageru pomocí tokenu SAS a prostředí Azure PowerShell</span><span class="sxs-lookup"><span data-stu-id="95773-103">Deploy private Resource Manager template with SAS token and Azure PowerShell</span></span>

<span data-ttu-id="95773-104">Pokud vaše šablona se nachází v účtu úložiště, můžete omezit přístup k šabloně a během nasazení zadat token sdílený přístupový podpis (SAS).</span><span class="sxs-lookup"><span data-stu-id="95773-104">When your template resides in a storage account, you can restrict access to the template and provide a shared access signature (SAS) token during deployment.</span></span> <span data-ttu-id="95773-105">Toto téma vysvětluje, jak pomocí prostředí Azure PowerShell s Resource Manager šablony během nasazení zadat SAS token.</span><span class="sxs-lookup"><span data-stu-id="95773-105">This topic explains how to use Azure PowerShell with Resource Manager templates to provide a SAS token during deployment.</span></span> 

## <a name="add-private-template-to-storage-account"></a><span data-ttu-id="95773-106">Přidání šablony privátní účet úložiště.</span><span class="sxs-lookup"><span data-stu-id="95773-106">Add private template to storage account</span></span>

<span data-ttu-id="95773-107">Šablony můžete přidat k účtu úložiště a odkaz na jejich během nasazení s tokenem SAS.</span><span class="sxs-lookup"><span data-stu-id="95773-107">You can add your templates to a storage account and link to them during deployment with a SAS token.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="95773-108">Pomocí následujících kroků, je přístupné pouze majiteli účtu objekt blob obsahující šablony.</span><span class="sxs-lookup"><span data-stu-id="95773-108">By following the steps below, the blob containing the template is accessible to only the account owner.</span></span> <span data-ttu-id="95773-109">Objekt blob je přístupný pro každý, kdo má tento identifikátor URI, ale při vytváření tokenu SAS pro tento objekt blob.</span><span class="sxs-lookup"><span data-stu-id="95773-109">However, when you create a SAS token for the blob, the blob is accessible to anyone with that URI.</span></span> <span data-ttu-id="95773-110">Pokud jiný uživatel zabrání identifikátor URI, tento uživatel má přístup k šabloně.</span><span class="sxs-lookup"><span data-stu-id="95773-110">If another user intercepts the URI, that user is able to access the template.</span></span> <span data-ttu-id="95773-111">Použití SAS token je vhodný způsob omezení přístupu ke šablony, ale nesmí obsahovat citlivá data, jako jsou hesla přímo v šabloně.</span><span class="sxs-lookup"><span data-stu-id="95773-111">Using a SAS token is a good way of limiting access to your templates, but you should not include sensitive data like passwords directly in the template.</span></span>
> 
> 

<span data-ttu-id="95773-112">Následující příklad nastaví kontejner privátní úložiště účet a odesílá šablonu:</span><span class="sxs-lookup"><span data-stu-id="95773-112">The following example sets up a private storage account container and uploads a template:</span></span>
   
```powershell
# create a storage account for templates
New-AzureRmResourceGroup -Name ManageGroup -Location "South Central US"
New-AzureRmStorageAccount -ResourceGroupName ManageGroup -Name {your-unique-name} -Type Standard_LRS -Location "West US"
Set-AzureRmCurrentStorageAccount -ResourceGroupName ManageGroup -Name {your-unique-name}

# create a container and upload template
New-AzureStorageContainer -Name templates -Permission Off
Set-AzureStorageBlobContent -Container templates -File c:\MyTemplates\storage.json
```

## <a name="provide-sas-token-during-deployment"></a><span data-ttu-id="95773-113">Během nasazení zadat tokenu SAS</span><span class="sxs-lookup"><span data-stu-id="95773-113">Provide SAS token during deployment</span></span>
<span data-ttu-id="95773-114">Pokud chcete nasadit šablonu privátní v účtu úložiště, vygenerování tokenu SAS a její zahrnutí do identifikátor URI pro šablonu.</span><span class="sxs-lookup"><span data-stu-id="95773-114">To deploy a private template in a storage account, generate a SAS token and include it in the URI for the template.</span></span> <span data-ttu-id="95773-115">Nastavte čas vypršení platnosti vyhradit dostatek času k dokončení nasazení.</span><span class="sxs-lookup"><span data-stu-id="95773-115">Set the expiry time to allow enough time to complete the deployment.</span></span>
   
```powershell
Set-AzureRmCurrentStorageAccount -ResourceGroupName ManageGroup -Name {your-unique-name}

# get the URI with the SAS token
$templateuri = New-AzureStorageBlobSASToken -Container templates -Blob storage.json -Permission r `
  -ExpiryTime (Get-Date).AddHours(2.0) -FullUri

# provide URI with SAS token during deployment
New-AzureRmResourceGroup -Name ExampleGroup -Location "South Central US"
New-AzureRmResourceGroupDeployment -ResourceGroupName ExampleGroup -TemplateUri $templateuri
```

<span data-ttu-id="95773-116">Příklad použití tokenu SAS s propojených šablon naleznete v části [použití propojených šablon s Azure Resource Manager](resource-group-linked-templates.md).</span><span class="sxs-lookup"><span data-stu-id="95773-116">For an example of using a SAS token with linked templates, see [Using linked templates with Azure Resource Manager](resource-group-linked-templates.md).</span></span>


## <a name="next-steps"></a><span data-ttu-id="95773-117">Další kroky</span><span class="sxs-lookup"><span data-stu-id="95773-117">Next steps</span></span>
* <span data-ttu-id="95773-118">Úvod do nasazení šablony, najdete v části [nasazení prostředků pomocí šablony Resource Manageru a prostředí Azure PowerShell](resource-group-template-deploy.md).</span><span class="sxs-lookup"><span data-stu-id="95773-118">For an introduction to deploying templates, see [Deploy resources with Resource Manager templates and Azure PowerShell](resource-group-template-deploy.md).</span></span>
* <span data-ttu-id="95773-119">Pro dokončení ukázkový skript, který nasadí šablonu, najdete v části [skript šablony nasazení Resource Manager](resource-manager-samples-powershell-deploy.md)</span><span class="sxs-lookup"><span data-stu-id="95773-119">For a complete sample script that deploys a template, see [Deploy Resource Manager template script](resource-manager-samples-powershell-deploy.md)</span></span>
* <span data-ttu-id="95773-120">Chcete-li definovat parametry v šabloně, přečtěte si téma [vytváření šablon](resource-group-authoring-templates.md#parameters).</span><span class="sxs-lookup"><span data-stu-id="95773-120">To define parameters in template, see [Authoring templates](resource-group-authoring-templates.md#parameters).</span></span>
* <span data-ttu-id="95773-121">Pokyny k tomu, jak můžou podniky používat Resource Manager k efektivní správě předplatných, najdete v části [Základní kostra Azure Enterprise – zásady správného řízení pro předplatná](resource-manager-subscription-governance.md).</span><span class="sxs-lookup"><span data-stu-id="95773-121">For guidance on how enterprises can use Resource Manager to effectively manage subscriptions, see [Azure enterprise scaffold - prescriptive subscription governance](resource-manager-subscription-governance.md).</span></span>

