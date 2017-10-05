---
title: "Rozhraní příkazového řádku Azure Script ukázka – vytvoření virtuálního počítače s Linuxem pomocí WordPress | Microsoft Docs"
description: "Rozhraní příkazového řádku Azure Script ukázka – vytvoření virtuálního počítače s Linuxem pomocí WordPress"
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
ms.openlocfilehash: cc95a190b58cb208ac0b642fc9dc2253e993ca23
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="create-a-vm-with-wordpress"></a><span data-ttu-id="bcda5-103">Vytvoření virtuálního počítače s WordPress</span><span class="sxs-lookup"><span data-stu-id="bcda5-103">Create a VM with WordPress</span></span>

<span data-ttu-id="bcda5-104">Tento skript vytvoří virtuální počítač a pak používá k instalaci WordPress rozšíření vlastních skriptů virtuálním počítači Azure.</span><span class="sxs-lookup"><span data-stu-id="bcda5-104">This script creates a virtual machine, and then uses the Azure Virtual Machine custom script extension to install WordPress.</span></span> <span data-ttu-id="bcda5-105">Po spuštění skriptu, můžete přístup k webu WordPress konfigurace v `http://<public IP of VM>/wordpress`.</span><span class="sxs-lookup"><span data-stu-id="bcda5-105">After running the script, you can access the WordPress configuration site at  `http://<public IP of VM>/wordpress`.</span></span> 

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a><span data-ttu-id="bcda5-106">Ukázkový skript</span><span class="sxs-lookup"><span data-stu-id="bcda5-106">Sample script</span></span>

<span data-ttu-id="bcda5-107">[!code-azurecli-interactive[hlavní](../../../cli_scripts/virtual-machine/create-wordpress-mysql/create-wordpress-mysql.sh "rychlé vytvoření virtuálního počítače")]</span><span class="sxs-lookup"><span data-stu-id="bcda5-107">[!code-azurecli-interactive[main](../../../cli_scripts/virtual-machine/create-wordpress-mysql/create-wordpress-mysql.sh "Quick Create VM")]</span></span>

## <a name="clean-up-deployment"></a><span data-ttu-id="bcda5-108">Vyčištění nasazení</span><span class="sxs-lookup"><span data-stu-id="bcda5-108">Clean up deployment</span></span> 

<span data-ttu-id="bcda5-109">Spusťte následující příkaz pro odebrání skupiny prostředků, virtuální počítač a všechny související prostředky.</span><span class="sxs-lookup"><span data-stu-id="bcda5-109">Run the following command to remove the resource group, VM, and all related resources.</span></span>

```azurecli-interactive 
az group delete --name myResourceGroup
```

## <a name="script-explanation"></a><span data-ttu-id="bcda5-110">Vysvětlení skriptu</span><span class="sxs-lookup"><span data-stu-id="bcda5-110">Script explanation</span></span>

<span data-ttu-id="bcda5-111">Tento skript používá následující příkazy k vytvoření skupiny prostředků, virtuální počítač a všechny související prostředky.</span><span class="sxs-lookup"><span data-stu-id="bcda5-111">This script uses the following commands to create a resource group, virtual machine, and all related resources.</span></span> <span data-ttu-id="bcda5-112">Každý příkaz v tabulce odkazy na dokumentaci konkrétní příkaz.</span><span class="sxs-lookup"><span data-stu-id="bcda5-112">Each command in the table links to command specific documentation.</span></span>

| <span data-ttu-id="bcda5-113">Příkaz</span><span class="sxs-lookup"><span data-stu-id="bcda5-113">Command</span></span> | <span data-ttu-id="bcda5-114">Poznámky</span><span class="sxs-lookup"><span data-stu-id="bcda5-114">Notes</span></span> |
|---|---|
| [<span data-ttu-id="bcda5-115">Vytvoření skupiny az</span><span class="sxs-lookup"><span data-stu-id="bcda5-115">az group create</span></span>](https://docs.microsoft.com/cli/azure/group#create) | <span data-ttu-id="bcda5-116">Vytvoří skupinu prostředků, ve kterém jsou uložené všechny prostředky.</span><span class="sxs-lookup"><span data-stu-id="bcda5-116">Creates a resource group in which all resources are stored.</span></span> |
| [<span data-ttu-id="bcda5-117">Vytvoření virtuálního počítače az</span><span class="sxs-lookup"><span data-stu-id="bcda5-117">az vm create</span></span>](https://docs.microsoft.com/cli/azure/vm#create) | <span data-ttu-id="bcda5-118">Vytvoří virtuální počítač a připojí jej k síťové karty, virtuální sítě, podsítě a NSG.</span><span class="sxs-lookup"><span data-stu-id="bcda5-118">Creates the virtual machine and connects it to the network card, virtual network, subnet, and NSG.</span></span> <span data-ttu-id="bcda5-119">Tento příkaz také Určuje bitovou kopii virtuálního počítače, který se má použít a pověření pro správu.</span><span class="sxs-lookup"><span data-stu-id="bcda5-119">This command also specifies the virtual machine image to be used, and administrative credentials.</span></span>  |
| [<span data-ttu-id="bcda5-120">virtuální počítač az open-port</span><span class="sxs-lookup"><span data-stu-id="bcda5-120">az vm open-port</span></span>](https://docs.microsoft.com/cli/azure/vm#open-port) | <span data-ttu-id="bcda5-121">Vytvoří pravidlo skupiny zabezpečení sítě chcete povolit příchozí přenosy.</span><span class="sxs-lookup"><span data-stu-id="bcda5-121">Creates a network security group rule to allow inbound traffic.</span></span> <span data-ttu-id="bcda5-122">V této ukázce je otevřen port 80 pro přenosy protokolu HTTP.</span><span class="sxs-lookup"><span data-stu-id="bcda5-122">In this sample, port 80 is opened for HTTP traffic.</span></span> |
| [<span data-ttu-id="bcda5-123">nastavení rozšíření virtuálního az</span><span class="sxs-lookup"><span data-stu-id="bcda5-123">az vm extension set</span></span>](https://docs.microsoft.com/cli/azure/vm#create) | <span data-ttu-id="bcda5-124">Přidejte do virtuálního počítače, které vyvolá skriptu instalace aplikace WordPress rozšíření vlastních skriptů.</span><span class="sxs-lookup"><span data-stu-id="bcda5-124">Add the Custom Script Extension to the virtual machine, which invokes a script to install WordPress.</span></span> |
| [<span data-ttu-id="bcda5-125">Odstranění skupiny az</span><span class="sxs-lookup"><span data-stu-id="bcda5-125">az group delete</span></span>](https://docs.microsoft.com/cli/azure/vm/extension#set) | <span data-ttu-id="bcda5-126">Odstraní skupinu prostředků, včetně všech vnořených prostředků.</span><span class="sxs-lookup"><span data-stu-id="bcda5-126">Deletes a resource group including all nested resources.</span></span> |

## <a name="next-steps"></a><span data-ttu-id="bcda5-127">Další kroky</span><span class="sxs-lookup"><span data-stu-id="bcda5-127">Next steps</span></span>

<span data-ttu-id="bcda5-128">Další informace o rozhraní příkazového řádku Azure najdete v tématu [dokumentaci k rozhraní příkazového řádku Azure](https://docs.microsoft.com/cli/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="bcda5-128">For more information on the Azure CLI, see [Azure CLI documentation](https://docs.microsoft.com/cli/azure/overview).</span></span>

<span data-ttu-id="bcda5-129">Ukázky skriptu rozhraní příkazového řádku další virtuální počítač nachází v [virtuální počítač Azure s Linuxem dokumentaci](../linux/cli-samples.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="bcda5-129">Additional virtual machine CLI script samples can be found in the [Azure Linux VM documentation](../linux/cli-samples.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span>
