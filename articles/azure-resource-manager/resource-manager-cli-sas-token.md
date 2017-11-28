---
title: "aaaDeploy šablony Azure s tokenu SAS a rozhraní příkazového řádku Azure | Microsoft Docs"
description: "Azure Resource Manageru a rozhraní příkazového řádku Azure toodeploy tooAzure prostředky ze šablony, která je chráněný pomocí tokenu SAS."
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
ms.openlocfilehash: 59c64616d6e1f5e456d88a72854d0ed99e1bdc0d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="deploy-private-resource-manager-template-with-sas-token-and-azure-cli"></a><span data-ttu-id="cc2a6-103">Nasazení privátní šablony Resource Manageru pomocí tokenu SAS a rozhraní příkazového řádku Azure</span><span class="sxs-lookup"><span data-stu-id="cc2a6-103">Deploy private Resource Manager template with SAS token and Azure CLI</span></span>

<span data-ttu-id="cc2a6-104">Pokud vaše šablona se nachází v účtu úložiště, můžete omezit přístup toohello šablony a během nasazení zadat token sdílený přístupový podpis (SAS).</span><span class="sxs-lookup"><span data-stu-id="cc2a6-104">When your template resides in a storage account, you can restrict access toohello template and provide a shared access signature (SAS) token during deployment.</span></span> <span data-ttu-id="cc2a6-105">Toto téma vysvětluje, jak toouse prostředí Azure PowerShell s Resource Manager šablony tooprovide token SAS během nasazení.</span><span class="sxs-lookup"><span data-stu-id="cc2a6-105">This topic explains how toouse Azure PowerShell with Resource Manager templates tooprovide a SAS token during deployment.</span></span> 

## <a name="add-private-template-toostorage-account"></a><span data-ttu-id="cc2a6-106">Přidat účet toostorage privátní šablony</span><span class="sxs-lookup"><span data-stu-id="cc2a6-106">Add private template toostorage account</span></span>

<span data-ttu-id="cc2a6-107">Můžete přidat váš účet úložiště tooa šablony a propojit toothem během nasazení s tokenem SAS.</span><span class="sxs-lookup"><span data-stu-id="cc2a6-107">You can add your templates tooa storage account and link toothem during deployment with a SAS token.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="cc2a6-108">Podle následujících kroků hello hello blob obsahující hello šablony je přístupný tooonly vlastníka účtu hello.</span><span class="sxs-lookup"><span data-stu-id="cc2a6-108">By following hello steps below, hello blob containing hello template is accessible tooonly hello account owner.</span></span> <span data-ttu-id="cc2a6-109">Při vytváření tokenu SAS pro objekt blob hello je hello blob však přístupné tooanyone s Tento identifikátor URI.</span><span class="sxs-lookup"><span data-stu-id="cc2a6-109">However, when you create a SAS token for hello blob, hello blob is accessible tooanyone with that URI.</span></span> <span data-ttu-id="cc2a6-110">Pokud jiný uživatel zabrání hello identifikátor URI, je tento uživatel moct tooaccess hello šablony.</span><span class="sxs-lookup"><span data-stu-id="cc2a6-110">If another user intercepts hello URI, that user is able tooaccess hello template.</span></span> <span data-ttu-id="cc2a6-111">Použití SAS token je vhodný způsob omezení přístupu tooyour šablony, ale nesmí obsahovat citlivá data, jako jsou hesla přímo v šabloně hello.</span><span class="sxs-lookup"><span data-stu-id="cc2a6-111">Using a SAS token is a good way of limiting access tooyour templates, but you should not include sensitive data like passwords directly in hello template.</span></span>
> 
> 

<span data-ttu-id="cc2a6-112">Hello následující příklad nastaví kontejner privátní úložiště účet a odesílá šablonu:</span><span class="sxs-lookup"><span data-stu-id="cc2a6-112">hello following example sets up a private storage account container and uploads a template:</span></span>
   
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

### <a name="provide-sas-token-during-deployment"></a><span data-ttu-id="cc2a6-113">Během nasazení zadat tokenu SAS</span><span class="sxs-lookup"><span data-stu-id="cc2a6-113">Provide SAS token during deployment</span></span>
<span data-ttu-id="cc2a6-114">toodeploy šablonu privátní v účtu úložiště, vygenerování tokenu SAS a její zahrnutí do hello identifikátor URI pro šablonu hello.</span><span class="sxs-lookup"><span data-stu-id="cc2a6-114">toodeploy a private template in a storage account, generate a SAS token and include it in hello URI for hello template.</span></span> <span data-ttu-id="cc2a6-115">Nastavte tooallow čas vypršení platnosti hello dostatek času toocomplete hello nasazení.</span><span class="sxs-lookup"><span data-stu-id="cc2a6-115">Set hello expiry time tooallow enough time toocomplete hello deployment.</span></span>
   
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

<span data-ttu-id="cc2a6-116">Příklad použití tokenu SAS s propojených šablon naleznete v části [použití propojených šablon s Azure Resource Manager](resource-group-linked-templates.md).</span><span class="sxs-lookup"><span data-stu-id="cc2a6-116">For an example of using a SAS token with linked templates, see [Using linked templates with Azure Resource Manager](resource-group-linked-templates.md).</span></span>

## <a name="next-steps"></a><span data-ttu-id="cc2a6-117">Další kroky</span><span class="sxs-lookup"><span data-stu-id="cc2a6-117">Next steps</span></span>
* <span data-ttu-id="cc2a6-118">Úvod toodeploying šablony, najdete v části [nasazení prostředků pomocí šablony Resource Manageru a prostředí Azure PowerShell](resource-group-template-deploy-cli.md).</span><span class="sxs-lookup"><span data-stu-id="cc2a6-118">For an introduction toodeploying templates, see [Deploy resources with Resource Manager templates and Azure PowerShell](resource-group-template-deploy-cli.md).</span></span>
* <span data-ttu-id="cc2a6-119">Pro dokončení ukázkový skript, který nasadí šablonu, najdete v části [skript šablony nasazení Resource Manager](resource-manager-samples-cli-deploy.md)</span><span class="sxs-lookup"><span data-stu-id="cc2a6-119">For a complete sample script that deploys a template, see [Deploy Resource Manager template script](resource-manager-samples-cli-deploy.md)</span></span>
* <span data-ttu-id="cc2a6-120">toodefine parametry v šabloně, najdete v části [vytváření šablon](resource-group-authoring-templates.md#parameters).</span><span class="sxs-lookup"><span data-stu-id="cc2a6-120">toodefine parameters in template, see [Authoring templates](resource-group-authoring-templates.md#parameters).</span></span>
* <span data-ttu-id="cc2a6-121">Pokyny k použití Resource Manager tooeffectively podniky můžou spravovat předplatná najdete v tématu [Azure enterprise vygenerované uživatelské rozhraní – zásady správného řízení doporučený předplatné](resource-manager-subscription-governance.md).</span><span class="sxs-lookup"><span data-stu-id="cc2a6-121">For guidance on how enterprises can use Resource Manager tooeffectively manage subscriptions, see [Azure enterprise scaffold - prescriptive subscription governance](resource-manager-subscription-governance.md).</span></span>
