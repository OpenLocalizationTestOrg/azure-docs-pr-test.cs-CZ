---
title: "aaaCreate virtuálního počítače s Linuxem v Azure ze šablony | Microsoft Docs"
description: "Jak toouse hello Azure CLI 2.0 toocreate virtuálního počítače s Linuxem ze šablony Resource Manageru"
services: virtual-machines-linux
documentationcenter: 
author: iainfoulds
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 721b8378-9e47-411e-842c-ec3276d3256a
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: azurecli
ms.topic: article
ms.date: 05/12/2017
ms.author: iainfou
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: f4b8c4103cccbae13c679ff2a2cac928a0e8e809
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="how-toocreate-a-linux-virtual-machine-with-azure-resource-manager-templates"></a><span data-ttu-id="6925f-103">Jak toocreate virtuální počítač s Linuxem pomocí šablony Azure Resource Manager</span><span class="sxs-lookup"><span data-stu-id="6925f-103">How toocreate a Linux virtual machine with Azure Resource Manager templates</span></span>
<span data-ttu-id="6925f-104">Tento článek ukazuje, jak tooquickly nasazení Linux virtuálního počítače (VM) pomocí šablony Azure Resource Manager a hello 2.0 rozhraní příkazového řádku Azure.</span><span class="sxs-lookup"><span data-stu-id="6925f-104">This article shows you how tooquickly deploy a Linux virtual machine (VM) with Azure Resource Manager templates and hello Azure CLI 2.0.</span></span> <span data-ttu-id="6925f-105">Můžete také provést tyto kroky hello [Azure CLI 1.0](create-ssh-secured-vm-from-template-nodejs.md).</span><span class="sxs-lookup"><span data-stu-id="6925f-105">You can also perform these steps with hello [Azure CLI 1.0](create-ssh-secured-vm-from-template-nodejs.md).</span></span>


## <a name="templates-overview"></a><span data-ttu-id="6925f-106">Přehled šablon</span><span class="sxs-lookup"><span data-stu-id="6925f-106">Templates overview</span></span>
<span data-ttu-id="6925f-107">Šablony Azure Resource Manageru jsou soubory JSON, které definují hello infrastruktury a konfigurace řešení Azure.</span><span class="sxs-lookup"><span data-stu-id="6925f-107">Azure Resource Manager templates are JSON files that define hello infrastructure and configuration of your Azure solution.</span></span> <span data-ttu-id="6925f-108">Pomocí šablony můžete řešení opakovaně nasadit v průběhu životního cyklu a mít přitom jistotu, že se prostředky nasadí konzistentně.</span><span class="sxs-lookup"><span data-stu-id="6925f-108">By using a template, you can repeatedly deploy your solution throughout its lifecycle and have confidence your resources are deployed in a consistent state.</span></span> <span data-ttu-id="6925f-109">toolearn Další informace o formátu hello hello šablony a jak vytvořit, najdete v části [vytvoření vaší první šablony Azure Resource Manager](../../azure-resource-manager/resource-manager-create-first-template.md).</span><span class="sxs-lookup"><span data-stu-id="6925f-109">toolearn more about hello format of hello template and how you construct it, see [Create your first Azure Resource Manager template](../../azure-resource-manager/resource-manager-create-first-template.md).</span></span> <span data-ttu-id="6925f-110">tooview hello syntaxe JSON pro typy prostředků, najdete v části [definování zdrojů v šablonách Azure Resource Manager](/azure/templates/).</span><span class="sxs-lookup"><span data-stu-id="6925f-110">tooview hello JSON syntax for resources types, see [Define resources in Azure Resource Manager templates](/azure/templates/).</span></span>


## <a name="create-resource-group"></a><span data-ttu-id="6925f-111">Vytvoření skupiny prostředků</span><span class="sxs-lookup"><span data-stu-id="6925f-111">Create resource group</span></span>
<span data-ttu-id="6925f-112">Skupina prostředků Azure je logický kontejner, ve kterém se nasazují a spravují prostředky Azure.</span><span class="sxs-lookup"><span data-stu-id="6925f-112">An Azure resource group is a logical container into which Azure resources are deployed and managed.</span></span> <span data-ttu-id="6925f-113">Před virtuálního počítače je třeba vytvořit skupinu prostředků.</span><span class="sxs-lookup"><span data-stu-id="6925f-113">A resource group must be created before a virtual machine.</span></span> <span data-ttu-id="6925f-114">Hello následující příklad vytvoří skupinu prostředků s názvem *myResourceGroupVM* v hello *eastus* oblasti:</span><span class="sxs-lookup"><span data-stu-id="6925f-114">hello following example creates a resource group named *myResourceGroupVM* in hello *eastus* region:</span></span>

```azurecli
az group create --name myResourceGroup --location eastus
```

## <a name="create-virtual-machine"></a><span data-ttu-id="6925f-115">Vytvoření virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="6925f-115">Create virtual machine</span></span>
<span data-ttu-id="6925f-116">Hello následující příklad vytvoří virtuální počítač z [této šablony Azure Resource Manageru](https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-vm-sshkey/azuredeploy.json) s [vytvořit nasazení skupiny az](/cli/azure/group/deployment#create).</span><span class="sxs-lookup"><span data-stu-id="6925f-116">hello following example creates a VM from [this Azure Resource Manager template](https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-vm-sshkey/azuredeploy.json) with [az group deployment create](/cli/azure/group/deployment#create).</span></span> <span data-ttu-id="6925f-117">Zadejte hodnotu hello vlastní veřejného klíče SSH, jako je například hello obsah *~/.ssh/id_rsa.pub*.</span><span class="sxs-lookup"><span data-stu-id="6925f-117">Provide hello value of your own SSH public key, such as hello contents of *~/.ssh/id_rsa.pub*.</span></span> <span data-ttu-id="6925f-118">Pokud potřebujete toocreate dvojici klíčů SSH, přečtěte si [jak toocreate a použití SSH klíče dvojice pro virtuální počítače s Linuxem v Azure](mac-create-ssh-keys.md).</span><span class="sxs-lookup"><span data-stu-id="6925f-118">If you need toocreate an SSH key pair, see [How toocreate and use an SSH key pair for Linux VMs in Azure](mac-create-ssh-keys.md).</span></span>

```azurecli
az group deployment create --resource-group myResourceGroup \
  --template-uri https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/101-vm-sshkey/azuredeploy.json \
  --parameters '{"sshKeyData": {"value": "ssh-rsa AAAAB3N{snip}B9eIgoZ"}}'
```

<span data-ttu-id="6925f-119">V tomto příkladu jste zadali šablonu uložené v Githubu.</span><span class="sxs-lookup"><span data-stu-id="6925f-119">In this example, you specified a template stored in GitHub.</span></span> <span data-ttu-id="6925f-120">Můžete také stáhnout nebo vytvořit šablonu a zadejte místní cestu hello s hello stejné `--template-file` parametr.</span><span class="sxs-lookup"><span data-stu-id="6925f-120">You can also download or create a template and specify hello local path with hello same `--template-file` parameter.</span></span>

<span data-ttu-id="6925f-121">tooSSH tooyour virtuálních počítačů, získat hello veřejnou IP adresu s [az sítě veřejné ip zobrazit](/cli/azure/network/public-ip#show):</span><span class="sxs-lookup"><span data-stu-id="6925f-121">tooSSH tooyour VM, obtain hello public IP address with [az network public-ip show](/cli/azure/network/public-ip#show):</span></span>

```azurecli
az network public-ip show \
    --resource-group myResourceGroup \
    --name sshPublicIP \
    --query [ipAddress] \
    --output tsv
```

<span data-ttu-id="6925f-122">Pak můžete SSH tooyour virtuálního počítače jako za normálních okolností:</span><span class="sxs-lookup"><span data-stu-id="6925f-122">You can then SSH tooyour VM as normal:</span></span>

```bash
ssh azureuser@<ipAddress>
```

## <a name="next-steps"></a><span data-ttu-id="6925f-123">Další kroky</span><span class="sxs-lookup"><span data-stu-id="6925f-123">Next steps</span></span>
<span data-ttu-id="6925f-124">V tomto příkladu jste vytvořili základní virtuální počítač s Linuxem.</span><span class="sxs-lookup"><span data-stu-id="6925f-124">In this example, you created a basic Linux VM.</span></span> <span data-ttu-id="6925f-125">Pro další šablony správce prostředků, které zahrnují aplikační architektury nebo vytvořit složitější prostředí, procházet hello [galerii šablon Azure rychlý Start](https://azure.microsoft.com/documentation/templates/).</span><span class="sxs-lookup"><span data-stu-id="6925f-125">For more Resource Manager templates that include application frameworks or create more complex environments, browse hello [Azure quickstart templates gallery](https://azure.microsoft.com/documentation/templates/).</span></span>
