---
title: "Rozhraní příkazového řádku Azure ukázkový skript – vytvoření virtuálního počítače s Linuxem | Microsoft Docs"
description: "Rozhraní příkazového řádku Azure ukázkový skript – vytvoření virtuálního počítače s Linuxem"
services: virtual-machines-linux
documentationcenter: virtual-machines
author: neilpeterson
manager: timlt
editor: tysonn
tags: azure-service-management
ms.assetid: 
ms.service: virtual-machines-linux
ms.devlang: azurecli
ms.topic: sample
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 02/27/2017
ms.author: nepeters
ms.custom: mvc
ms.openlocfilehash: dc7e7276bcea32c59c67a42dedaffcedfd0cf907
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="create-a-fully-configured-virtual-machine"></a><span data-ttu-id="6cd56-103">Vytvoření kompletně nakonfigurovaný virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="6cd56-103">Create a fully configured virtual machine</span></span>

<span data-ttu-id="6cd56-104">Tento skript vytvoří virtuální počítač Azure s Ubuntu operačního systému.</span><span class="sxs-lookup"><span data-stu-id="6cd56-104">This script creates an Azure Virtual Machine with an Ubuntu operating system.</span></span> <span data-ttu-id="6cd56-105">Po spuštění skriptu, můžete k virtuálnímu počítači přes protokol SSH.</span><span class="sxs-lookup"><span data-stu-id="6cd56-105">After running the script, you can access the virtual machine over SSH.</span></span>

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a><span data-ttu-id="6cd56-106">Ukázkový skript</span><span class="sxs-lookup"><span data-stu-id="6cd56-106">Sample script</span></span>

<span data-ttu-id="6cd56-107">[!code-azurecli-interactive[hlavní](../../../cli_scripts/virtual-machine/create-vm-detailed/create-vm-detailed.sh "rychlé vytvoření virtuálního počítače")]</span><span class="sxs-lookup"><span data-stu-id="6cd56-107">[!code-azurecli-interactive[main](../../../cli_scripts/virtual-machine/create-vm-detailed/create-vm-detailed.sh "Quick Create VM")]</span></span>

## <a name="clean-up-deployment"></a><span data-ttu-id="6cd56-108">Vyčištění nasazení</span><span class="sxs-lookup"><span data-stu-id="6cd56-108">Clean up deployment</span></span> 

<span data-ttu-id="6cd56-109">Spusťte následující příkaz pro odebrání skupiny prostředků, virtuální počítač a všechny související prostředky.</span><span class="sxs-lookup"><span data-stu-id="6cd56-109">Run the following command to remove the resource group, VM, and all related resources.</span></span>

```azurecli-interactive 
az group delete --name myResourceGroup
```

## <a name="script-explanation"></a><span data-ttu-id="6cd56-110">Vysvětlení skriptu</span><span class="sxs-lookup"><span data-stu-id="6cd56-110">Script explanation</span></span>

<span data-ttu-id="6cd56-111">Tento skript používá následující příkazy k vytvoření skupiny prostředků, virtuální počítač a všechny související prostředky.</span><span class="sxs-lookup"><span data-stu-id="6cd56-111">This script uses the following commands to create a resource group, virtual machine, and all related resources.</span></span> <span data-ttu-id="6cd56-112">Každý příkaz v tabulce odkazy na dokumentaci konkrétní příkaz.</span><span class="sxs-lookup"><span data-stu-id="6cd56-112">Each command in the table links to command specific documentation.</span></span>

| <span data-ttu-id="6cd56-113">Příkaz</span><span class="sxs-lookup"><span data-stu-id="6cd56-113">Command</span></span> | <span data-ttu-id="6cd56-114">Poznámky</span><span class="sxs-lookup"><span data-stu-id="6cd56-114">Notes</span></span> |
|---|---|
| [<span data-ttu-id="6cd56-115">Vytvoření skupiny az</span><span class="sxs-lookup"><span data-stu-id="6cd56-115">az group create</span></span>](https://docs.microsoft.com/cli/azure/group#create) | <span data-ttu-id="6cd56-116">Vytvoří skupinu prostředků, ve kterém jsou uložené všechny prostředky.</span><span class="sxs-lookup"><span data-stu-id="6cd56-116">Creates a resource group in which all resources are stored.</span></span> |
| [<span data-ttu-id="6cd56-117">Vytvoření sítě vnet az</span><span class="sxs-lookup"><span data-stu-id="6cd56-117">az network vnet create</span></span>](https://docs.microsoft.com/cli/azure/network/vnet#create) | <span data-ttu-id="6cd56-118">Vytvoří virtuální síť Azure a podsíť.</span><span class="sxs-lookup"><span data-stu-id="6cd56-118">Creates an Azure virtual network and subnet.</span></span> |
| [<span data-ttu-id="6cd56-119">Vytvoření veřejné sítě az-ip</span><span class="sxs-lookup"><span data-stu-id="6cd56-119">az network public-ip create</span></span>](https://docs.microsoft.com/cli/azure/network/public-ip#create) | <span data-ttu-id="6cd56-120">Vytvoří veřejnou IP adresu se statickou IP adresu a přidružené název DNS.</span><span class="sxs-lookup"><span data-stu-id="6cd56-120">Creates a public IP address with a static IP address and an associated DNS name.</span></span> |
| [<span data-ttu-id="6cd56-121">Vytvoření az sítě nsg</span><span class="sxs-lookup"><span data-stu-id="6cd56-121">az network nsg create</span></span>](https://docs.microsoft.com/cli/azure/network/nsg#create) | <span data-ttu-id="6cd56-122">Vytvoří skupinu zabezpečení sítě (NSG), což je hranice zabezpečení mezi Internetu a virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="6cd56-122">Creates a network security group (NSG), which is a security boundary between the internet and the virtual machine.</span></span> |
| [<span data-ttu-id="6cd56-123">Vytvoření pravidla nsg az sítě</span><span class="sxs-lookup"><span data-stu-id="6cd56-123">az network nsg rule create</span></span>](https://docs.microsoft.com/cli/azure/network/nsg/rule#create) | <span data-ttu-id="6cd56-124">Vytvoří pravidlo NSG chcete povolit příchozí přenosy.</span><span class="sxs-lookup"><span data-stu-id="6cd56-124">Creates an NSG rule to allow inbound traffic.</span></span> <span data-ttu-id="6cd56-125">V této ukázce je otevřen port 22 pro SSH provoz.</span><span class="sxs-lookup"><span data-stu-id="6cd56-125">In this sample, port 22 is opened for SSH traffic.</span></span> |
| [<span data-ttu-id="6cd56-126">Vytvoření az síťových adaptérů sítě</span><span class="sxs-lookup"><span data-stu-id="6cd56-126">az network nic create</span></span>](https://docs.microsoft.com/cli/azure/network/nic#create) | <span data-ttu-id="6cd56-127">Vytvoří virtuální síťové karty a připojí jej k virtuální síti, podsíti a NSG.</span><span class="sxs-lookup"><span data-stu-id="6cd56-127">Creates a virtual network card and attaches it to the virtual network, subnet, and NSG.</span></span> |
| [<span data-ttu-id="6cd56-128">Vytvoření virtuálního počítače az</span><span class="sxs-lookup"><span data-stu-id="6cd56-128">az vm create</span></span>](https://docs.microsoft.com/cli/azure/vm#create) | <span data-ttu-id="6cd56-129">Vytvoří virtuální počítač a připojí jej k síťové karty, virtuální sítě, podsítě a NSG.</span><span class="sxs-lookup"><span data-stu-id="6cd56-129">Creates the virtual machine and connects it to the network card, virtual network, subnet, and NSG.</span></span> <span data-ttu-id="6cd56-130">Tento příkaz také Určuje bitovou kopii virtuálního počítače, který se má použít a pověření pro správu.</span><span class="sxs-lookup"><span data-stu-id="6cd56-130">This command also specifies the virtual machine image to be used, and administrative credentials.</span></span>  |
| [<span data-ttu-id="6cd56-131">Odstranění skupiny az</span><span class="sxs-lookup"><span data-stu-id="6cd56-131">az group delete</span></span>](https://docs.microsoft.com/cli/azure/vm/extension#set) | <span data-ttu-id="6cd56-132">Odstraní skupinu prostředků, včetně všech vnořených prostředků.</span><span class="sxs-lookup"><span data-stu-id="6cd56-132">Deletes a resource group including all nested resources.</span></span> |

## <a name="next-steps"></a><span data-ttu-id="6cd56-133">Další kroky</span><span class="sxs-lookup"><span data-stu-id="6cd56-133">Next steps</span></span>

<span data-ttu-id="6cd56-134">Další informace o rozhraní příkazového řádku Azure najdete v tématu [dokumentaci k rozhraní příkazového řádku Azure](https://docs.microsoft.com/cli/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="6cd56-134">For more information on the Azure CLI, see [Azure CLI documentation](https://docs.microsoft.com/cli/azure/overview).</span></span>

<span data-ttu-id="6cd56-135">Ukázky skriptu rozhraní příkazového řádku další virtuální počítač nachází v [virtuální počítač Azure s Linuxem dokumentaci](../linux/cli-samples.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="6cd56-135">Additional virtual machine CLI script samples can be found in the [Azure Linux VM documentation](../linux/cli-samples.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span>
