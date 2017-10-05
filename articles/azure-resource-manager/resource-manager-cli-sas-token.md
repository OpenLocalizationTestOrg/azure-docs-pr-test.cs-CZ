---
title: "Nasazení šablony Azure s tokenu SAS a rozhraní příkazového řádku Azure | Microsoft Docs"
description: "Pomocí Azure Resource Manager a rozhraní příkazového řádku Azure můžete nasadit prostředky do Azure ze šablony, která je chráněná pomocí tokenu SAS."
services: azure-resource-manager
documentationcenter: na
author: tfitzmac
manager: timlt
editor: tysonn
ms.assetid: 
ms.service: azure-resource-manager
ms.devlang: azurecli
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 05/31/2017
ms.author: tomfitz
ms.openlocfilehash: 22387aadd8f53a65efb76a29a9403c46a2c25954
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="deploy-private-resource-manager-template-with-sas-token-and-azure-cli"></a><span data-ttu-id="ef774-103">Nasazení privátní šablony Resource Manageru pomocí tokenu SAS a rozhraní příkazového řádku Azure</span><span class="sxs-lookup"><span data-stu-id="ef774-103">Deploy private Resource Manager template with SAS token and Azure CLI</span></span>

<span data-ttu-id="ef774-104">Pokud vaše šablona se nachází v účtu úložiště, můžete omezit přístup k šabloně a během nasazení zadat token sdílený přístupový podpis (SAS).</span><span class="sxs-lookup"><span data-stu-id="ef774-104">When your template resides in a storage account, you can restrict access to the template and provide a shared access signature (SAS) token during deployment.</span></span> <span data-ttu-id="ef774-105">Toto téma vysvětluje, jak pomocí prostředí Azure PowerShell s Resource Manager šablony během nasazení zadat SAS token.</span><span class="sxs-lookup"><span data-stu-id="ef774-105">This topic explains how to use Azure PowerShell with Resource Manager templates to provide a SAS token during deployment.</span></span> 

## <a name="add-private-template-to-storage-account"></a><span data-ttu-id="ef774-106">Přidání šablony privátní účet úložiště.</span><span class="sxs-lookup"><span data-stu-id="ef774-106">Add private template to storage account</span></span>

<span data-ttu-id="ef774-107">Šablony můžete přidat k účtu úložiště a odkaz na jejich během nasazení s tokenem SAS.</span><span class="sxs-lookup"><span data-stu-id="ef774-107">You can add your templates to a storage account and link to them during deployment with a SAS token.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="ef774-108">Pomocí následujících kroků, je přístupné pouze majiteli účtu objekt blob obsahující šablony.</span><span class="sxs-lookup"><span data-stu-id="ef774-108">By following the steps below, the blob containing the template is accessible to only the account owner.</span></span> <span data-ttu-id="ef774-109">Objekt blob je přístupný pro každý, kdo má tento identifikátor URI, ale při vytváření tokenu SAS pro tento objekt blob.</span><span class="sxs-lookup"><span data-stu-id="ef774-109">However, when you create a SAS token for the blob, the blob is accessible to anyone with that URI.</span></span> <span data-ttu-id="ef774-110">Pokud jiný uživatel zabrání identifikátor URI, tento uživatel má přístup k šabloně.</span><span class="sxs-lookup"><span data-stu-id="ef774-110">If another user intercepts the URI, that user is able to access the template.</span></span> <span data-ttu-id="ef774-111">Použití SAS token je vhodný způsob omezení přístupu ke šablony, ale nesmí obsahovat citlivá data, jako jsou hesla přímo v šabloně.</span><span class="sxs-lookup"><span data-stu-id="ef774-111">Using a SAS token is a good way of limiting access to your templates, but you should not include sensitive data like passwords directly in the template.</span></span>
> 
> 

<span data-ttu-id="ef774-112">Následující příklad nastaví kontejner privátní úložiště účet a odesílá šablonu:</span><span class="sxs-lookup"><span data-stu-id="ef774-112">The following example sets up a private storage account container and uploads a template:</span></span>
   
```azurecli
az group create --name "ManageGroup" --location "South Central US"
az storage account create \
    --resource-group ManageGroup \
    --location "South Central US" \
    --sku Standard_LRS \
    --kind Storage \
    --name {your-unique-name}
connection=$(az storage account show-connection-string \
    --resource-group ManageGroup \
    --name {your-unique-name} \
    --query connectionString)
az storage container create \
    --name templates \
    --public-access Off \
    --connection-string $connection
az storage blob upload \
    --container-name templates \
    --file vmlinux.json \
    --name vmlinux.json \
    --connection-string $connection
```

### <a name="provide-sas-token-during-deployment"></a><span data-ttu-id="ef774-113">Během nasazení zadat tokenu SAS</span><span class="sxs-lookup"><span data-stu-id="ef774-113">Provide SAS token during deployment</span></span>
<span data-ttu-id="ef774-114">Pokud chcete nasadit šablonu privátní v účtu úložiště, vygenerování tokenu SAS a její zahrnutí do identifikátor URI pro šablonu.</span><span class="sxs-lookup"><span data-stu-id="ef774-114">To deploy a private template in a storage account, generate a SAS token and include it in the URI for the template.</span></span> <span data-ttu-id="ef774-115">Nastavte čas vypršení platnosti vyhradit dostatek času k dokončení nasazení.</span><span class="sxs-lookup"><span data-stu-id="ef774-115">Set the expiry time to allow enough time to complete the deployment.</span></span>
   
```azurecli
expiretime=$(date -u -d '30 minutes' +%Y-%m-%dT%H:%MZ)
connection=$(az storage account show-connection-string \
    --resource-group ManageGroup \
    --name {your-unique-name} \
    --query connectionString)
token=$(az storage blob generate-sas \
    --container-name templates \
    --name vmlinux.json \
    --expiry $expiretime \
    --permissions r \
    --output tsv \
    --connection-string $connection)
url=$(az storage blob url \
    --container-name templates \
    --name vmlinux.json \
    --output tsv \
    --connection-string $connection)
az group deployment create --resource-group ExampleGroup --template-uri $url?$token
```

<span data-ttu-id="ef774-116">Příklad použití tokenu SAS s propojených šablon naleznete v části [použití propojených šablon s Azure Resource Manager](resource-group-linked-templates.md).</span><span class="sxs-lookup"><span data-stu-id="ef774-116">For an example of using a SAS token with linked templates, see [Using linked templates with Azure Resource Manager](resource-group-linked-templates.md).</span></span>

## <a name="next-steps"></a><span data-ttu-id="ef774-117">Další kroky</span><span class="sxs-lookup"><span data-stu-id="ef774-117">Next steps</span></span>
* <span data-ttu-id="ef774-118">Úvod do nasazení šablony, najdete v části [nasazení prostředků pomocí šablony Resource Manageru a prostředí Azure PowerShell](resource-group-template-deploy-cli.md).</span><span class="sxs-lookup"><span data-stu-id="ef774-118">For an introduction to deploying templates, see [Deploy resources with Resource Manager templates and Azure PowerShell](resource-group-template-deploy-cli.md).</span></span>
* <span data-ttu-id="ef774-119">Pro dokončení ukázkový skript, který nasadí šablonu, najdete v části [skript šablony nasazení Resource Manager](resource-manager-samples-cli-deploy.md)</span><span class="sxs-lookup"><span data-stu-id="ef774-119">For a complete sample script that deploys a template, see [Deploy Resource Manager template script](resource-manager-samples-cli-deploy.md)</span></span>
* <span data-ttu-id="ef774-120">Chcete-li definovat parametry v šabloně, přečtěte si téma [vytváření šablon](resource-group-authoring-templates.md#parameters).</span><span class="sxs-lookup"><span data-stu-id="ef774-120">To define parameters in template, see [Authoring templates](resource-group-authoring-templates.md#parameters).</span></span>
* <span data-ttu-id="ef774-121">Pokyny k tomu, jak můžou podniky používat Resource Manager k efektivní správě předplatných, najdete v části [Základní kostra Azure Enterprise – zásady správného řízení pro předplatná](resource-manager-subscription-governance.md).</span><span class="sxs-lookup"><span data-stu-id="ef774-121">For guidance on how enterprises can use Resource Manager to effectively manage subscriptions, see [Azure enterprise scaffold - prescriptive subscription governance](resource-manager-subscription-governance.md).</span></span>
