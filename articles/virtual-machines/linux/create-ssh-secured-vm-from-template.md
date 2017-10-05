---
title: "Vytvoření virtuálního počítače s Linuxem v Azure ze šablony | Microsoft Docs"
description: "Jak používat 2.0 rozhraní příkazového řádku Azure k vytvoření virtuálního počítače s Linuxem ze šablony Resource Manageru"
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
ms.openlocfilehash: 908a8a0c82b2d21fb25c9b33dbd714570d1ac272
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="how-to-create-a-linux-virtual-machine-with-azure-resource-manager-templates"></a><span data-ttu-id="cbb27-103">Jak vytvořit virtuální počítač s Linuxem pomocí šablony Azure Resource Manager</span><span class="sxs-lookup"><span data-stu-id="cbb27-103">How to create a Linux virtual machine with Azure Resource Manager templates</span></span>
<span data-ttu-id="cbb27-104">Tento článek ukazuje, jak rychle nasadit virtuální počítač s Linuxem (VM) pomocí šablony Azure Resource Manager a Azure CLI 2.0.</span><span class="sxs-lookup"><span data-stu-id="cbb27-104">This article shows you how to quickly deploy a Linux virtual machine (VM) with Azure Resource Manager templates and the Azure CLI 2.0.</span></span> <span data-ttu-id="cbb27-105">K provedení těchto kroků můžete také využít [Azure CLI 1.0](create-ssh-secured-vm-from-template-nodejs.md).</span><span class="sxs-lookup"><span data-stu-id="cbb27-105">You can also perform these steps with the [Azure CLI 1.0](create-ssh-secured-vm-from-template-nodejs.md).</span></span>


## <a name="templates-overview"></a><span data-ttu-id="cbb27-106">Přehled šablon</span><span class="sxs-lookup"><span data-stu-id="cbb27-106">Templates overview</span></span>
<span data-ttu-id="cbb27-107">Šablony Azure Resource Manageru jsou soubory JSON, které definují, infrastruktury a konfigurace řešení Azure.</span><span class="sxs-lookup"><span data-stu-id="cbb27-107">Azure Resource Manager templates are JSON files that define the infrastructure and configuration of your Azure solution.</span></span> <span data-ttu-id="cbb27-108">Pomocí šablony můžete řešení opakovaně nasadit v průběhu životního cyklu a mít přitom jistotu, že se prostředky nasadí konzistentně.</span><span class="sxs-lookup"><span data-stu-id="cbb27-108">By using a template, you can repeatedly deploy your solution throughout its lifecycle and have confidence your resources are deployed in a consistent state.</span></span> <span data-ttu-id="cbb27-109">Další informace o formátu šablony a jak vytvořit, najdete v části [vytvoření vaší první šablony Azure Resource Manager](../../azure-resource-manager/resource-manager-create-first-template.md).</span><span class="sxs-lookup"><span data-stu-id="cbb27-109">To learn more about the format of the template and how you construct it, see [Create your first Azure Resource Manager template](../../azure-resource-manager/resource-manager-create-first-template.md).</span></span> <span data-ttu-id="cbb27-110">Syntaxi JSON pro typy prostředků najdete v tématu [Definování prostředků v šablonách Azure Resource Manageru](/azure/templates/).</span><span class="sxs-lookup"><span data-stu-id="cbb27-110">To view the JSON syntax for resources types, see [Define resources in Azure Resource Manager templates](/azure/templates/).</span></span>


## <a name="create-resource-group"></a><span data-ttu-id="cbb27-111">Vytvoření skupiny prostředků</span><span class="sxs-lookup"><span data-stu-id="cbb27-111">Create resource group</span></span>
<span data-ttu-id="cbb27-112">Skupina prostředků Azure je logický kontejner, ve kterém se nasazují a spravují prostředky Azure.</span><span class="sxs-lookup"><span data-stu-id="cbb27-112">An Azure resource group is a logical container into which Azure resources are deployed and managed.</span></span> <span data-ttu-id="cbb27-113">Před virtuálního počítače je třeba vytvořit skupinu prostředků.</span><span class="sxs-lookup"><span data-stu-id="cbb27-113">A resource group must be created before a virtual machine.</span></span> <span data-ttu-id="cbb27-114">Následující příklad vytvoří skupinu prostředků s názvem *myResourceGroupVM* v *eastus* oblasti:</span><span class="sxs-lookup"><span data-stu-id="cbb27-114">The following example creates a resource group named *myResourceGroupVM* in the *eastus* region:</span></span>

```azurecli
az group create --name myResourceGroup --location eastus
```

## <a name="create-virtual-machine"></a><span data-ttu-id="cbb27-115">Vytvoření virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="cbb27-115">Create virtual machine</span></span>
<span data-ttu-id="cbb27-116">Následující příklad vytvoří virtuální počítač z [této šablony Azure Resource Manageru](https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-vm-sshkey/azuredeploy.json) s [vytvořit nasazení skupiny az](/cli/azure/group/deployment#create).</span><span class="sxs-lookup"><span data-stu-id="cbb27-116">The following example creates a VM from [this Azure Resource Manager template](https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-vm-sshkey/azuredeploy.json) with [az group deployment create](/cli/azure/group/deployment#create).</span></span> <span data-ttu-id="cbb27-117">Zadejte hodnotu vlastní veřejný klíč SSH, jako je například obsah *~/.ssh/id_rsa.pub*.</span><span class="sxs-lookup"><span data-stu-id="cbb27-117">Provide the value of your own SSH public key, such as the contents of *~/.ssh/id_rsa.pub*.</span></span> <span data-ttu-id="cbb27-118">Pokud potřebujete vytvořit dvojici klíčů SSH, přečtěte si [postup vytvoření a používání dvojici klíčů SSH pro virtuální počítače s Linuxem v Azure](mac-create-ssh-keys.md).</span><span class="sxs-lookup"><span data-stu-id="cbb27-118">If you need to create an SSH key pair, see [How to create and use an SSH key pair for Linux VMs in Azure](mac-create-ssh-keys.md).</span></span>

```azurecli
az group deployment create --resource-group myResourceGroup \
  --template-uri https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/101-vm-sshkey/azuredeploy.json \
  --parameters '{"sshKeyData": {"value": "ssh-rsa AAAAB3N{snip}B9eIgoZ"}}'
```

<span data-ttu-id="cbb27-119">V tomto příkladu jste zadali šablonu uložené v Githubu.</span><span class="sxs-lookup"><span data-stu-id="cbb27-119">In this example, you specified a template stored in GitHub.</span></span> <span data-ttu-id="cbb27-120">Můžete také stáhnout nebo vytvořit šablonu a zadejte místní cestu se stejným `--template-file` parametr.</span><span class="sxs-lookup"><span data-stu-id="cbb27-120">You can also download or create a template and specify the local path with the same `--template-file` parameter.</span></span>

<span data-ttu-id="cbb27-121">Chcete-li SSH k virtuálnímu počítači, získat veřejnou IP adresu s [az sítě veřejné ip zobrazit](/cli/azure/network/public-ip#show):</span><span class="sxs-lookup"><span data-stu-id="cbb27-121">To SSH to your VM, obtain the public IP address with [az network public-ip show](/cli/azure/network/public-ip#show):</span></span>

```azurecli
az network public-ip show \
    --resource-group myResourceGroup \
    --name sshPublicIP \
    --query [ipAddress] \
    --output tsv
```

<span data-ttu-id="cbb27-122">Pak můžete SSH pro virtuální počítač jako za normálních okolností:</span><span class="sxs-lookup"><span data-stu-id="cbb27-122">You can then SSH to your VM as normal:</span></span>

```bash
ssh azureuser@<ipAddress>
```

## <a name="next-steps"></a><span data-ttu-id="cbb27-123">Další kroky</span><span class="sxs-lookup"><span data-stu-id="cbb27-123">Next steps</span></span>
<span data-ttu-id="cbb27-124">V tomto příkladu jste vytvořili základní virtuální počítač s Linuxem.</span><span class="sxs-lookup"><span data-stu-id="cbb27-124">In this example, you created a basic Linux VM.</span></span> <span data-ttu-id="cbb27-125">Pro další šablony správce prostředků, které zahrnují aplikační architektury nebo vytvořit složitější prostředí, přejděte [galerii šablon Azure rychlý Start](https://azure.microsoft.com/documentation/templates/).</span><span class="sxs-lookup"><span data-stu-id="cbb27-125">For more Resource Manager templates that include application frameworks or create more complex environments, browse the [Azure quickstart templates gallery](https://azure.microsoft.com/documentation/templates/).</span></span>