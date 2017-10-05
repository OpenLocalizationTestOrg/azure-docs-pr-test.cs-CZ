---
title: "Azure CLI skriptu ukázkové – vytvoření virtuálního počítače Windows serveru 2016 se službou IIS pomocí DSC | Microsoft Docs"
description: "Azure CLI skriptu ukázkové – vytvoření virtuálního počítače Windows serveru 2016 se službou IIS pomocí DSC"
services: virtual-machines-windows
documentationcenter: virtual-machines
author: rickstercdn
manager: timlt
editor: tysonn
tags: 
ms.assetid: 
ms.service: virtual-machines-Windows
ms.devlang: azurecli
ms.topic: sample
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 02/23/2017
ms.author: rclaus
ms.custom: mvc
ms.openlocfilehash: 1f605c0260fcaea29851d76c90ba0b287bec5ae7
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="create-a-vm-with-iis-using-dsc"></a><span data-ttu-id="6abf0-103">Vytvoření virtuálního počítače se službou IIS pomocí DSC</span><span class="sxs-lookup"><span data-stu-id="6abf0-103">Create a VM with IIS using DSC</span></span>

<span data-ttu-id="6abf0-104">Tento skript vytvoří virtuální počítač a rozšíření vlastních skriptů DSC virtuální počítač Azure používá k instalaci a konfiguraci služby IIS.</span><span class="sxs-lookup"><span data-stu-id="6abf0-104">This script creates a virtual machine, and uses the Azure Virtual Machine DSC custom script extension to install and configure IIS.</span></span> 

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a><span data-ttu-id="6abf0-105">Ukázkový skript</span><span class="sxs-lookup"><span data-stu-id="6abf0-105">Sample script</span></span>

<span data-ttu-id="6abf0-106">[!code-azurecli-interactive[hlavní](../../../cli_scripts/virtual-machine/create-windows-iis-using-dsc/create-windows-iis-using-dsc.sh "rychlé vytvoření virtuálního počítače")]</span><span class="sxs-lookup"><span data-stu-id="6abf0-106">[!code-azurecli-interactive[main](../../../cli_scripts/virtual-machine/create-windows-iis-using-dsc/create-windows-iis-using-dsc.sh "Quick Create VM")]</span></span>

## <a name="clean-up-deployment"></a><span data-ttu-id="6abf0-107">Vyčištění nasazení</span><span class="sxs-lookup"><span data-stu-id="6abf0-107">Clean up deployment</span></span> 

<span data-ttu-id="6abf0-108">Spusťte následující příkaz pro odebrání skupiny prostředků, virtuální počítač a všechny související prostředky.</span><span class="sxs-lookup"><span data-stu-id="6abf0-108">Run the following command to remove the resource group, VM, and all related resources.</span></span>

```azurecli-interactive 
az group delete --name myResourceGroup --yes
```

## <a name="script-explanation"></a><span data-ttu-id="6abf0-109">Vysvětlení skriptu</span><span class="sxs-lookup"><span data-stu-id="6abf0-109">Script explanation</span></span>

<span data-ttu-id="6abf0-110">Tento skript používá následující příkazy k vytvoření skupiny prostředků, virtuální počítač a všechny související prostředky.</span><span class="sxs-lookup"><span data-stu-id="6abf0-110">This script uses the following commands to create a resource group, virtual machine, and all related resources.</span></span> <span data-ttu-id="6abf0-111">Každý příkaz v tabulce odkazy na dokumentaci konkrétní příkaz.</span><span class="sxs-lookup"><span data-stu-id="6abf0-111">Each command in the table links to command specific documentation.</span></span>

| <span data-ttu-id="6abf0-112">Příkaz</span><span class="sxs-lookup"><span data-stu-id="6abf0-112">Command</span></span> | <span data-ttu-id="6abf0-113">Poznámky</span><span class="sxs-lookup"><span data-stu-id="6abf0-113">Notes</span></span> |
|---|---|
| [<span data-ttu-id="6abf0-114">Vytvoření skupiny az</span><span class="sxs-lookup"><span data-stu-id="6abf0-114">az group create</span></span>](https://docs.microsoft.com/cli/azure/group#create) | <span data-ttu-id="6abf0-115">Vytvoří skupinu prostředků, ve kterém jsou uložené všechny prostředky.</span><span class="sxs-lookup"><span data-stu-id="6abf0-115">Creates a resource group in which all resources are stored.</span></span> |
| [<span data-ttu-id="6abf0-116">Vytvoření virtuálního počítače az</span><span class="sxs-lookup"><span data-stu-id="6abf0-116">az vm create</span></span>](https://docs.microsoft.com/cli/azure/vm#create) | <span data-ttu-id="6abf0-117">Vytvoří virtuální počítač a připojí jej k síťové karty, virtuální sítě, podsítě a NSG.</span><span class="sxs-lookup"><span data-stu-id="6abf0-117">Creates the virtual machine and connects it to the network card, virtual network, subnet, and NSG.</span></span> <span data-ttu-id="6abf0-118">Tento příkaz také Určuje bitovou kopii virtuálního počítače, který se má použít a pověření pro správu.</span><span class="sxs-lookup"><span data-stu-id="6abf0-118">This command also specifies the virtual machine image to be used, and administrative credentials.</span></span>  |
| [<span data-ttu-id="6abf0-119">nastavení rozšíření virtuálního az</span><span class="sxs-lookup"><span data-stu-id="6abf0-119">az vm extension set</span></span>](https://docs.microsoft.com/cli/azure/vm#create) | <span data-ttu-id="6abf0-120">Přidejte do virtuálního počítače, které vyvolá skript k instalaci IIS rozšíření vlastních skriptů.</span><span class="sxs-lookup"><span data-stu-id="6abf0-120">Add the Custom Script Extension to the virtual machine which invokes a script to install IIS.</span></span> |
| [<span data-ttu-id="6abf0-121">virtuální počítač az open-port</span><span class="sxs-lookup"><span data-stu-id="6abf0-121">az vm open-port</span></span>](https://docs.microsoft.com/cli/azure/vm#open-port) | <span data-ttu-id="6abf0-122">Vytvoří pravidlo skupiny zabezpečení sítě chcete povolit příchozí přenosy.</span><span class="sxs-lookup"><span data-stu-id="6abf0-122">Creates a network security group rule to allow inbound traffic.</span></span> <span data-ttu-id="6abf0-123">V této ukázce je otevřen port 80 pro přenosy protokolu HTTP.</span><span class="sxs-lookup"><span data-stu-id="6abf0-123">In this sample, port 80 is opened for HTTP traffic.</span></span> |
| [<span data-ttu-id="6abf0-124">Odstranění skupiny az</span><span class="sxs-lookup"><span data-stu-id="6abf0-124">az group delete</span></span>](https://docs.microsoft.com/cli/azure/vm/extension#set) | <span data-ttu-id="6abf0-125">Odstraní skupinu prostředků, včetně všech vnořených prostředků.</span><span class="sxs-lookup"><span data-stu-id="6abf0-125">Deletes a resource group including all nested resources.</span></span> |

## <a name="next-steps"></a><span data-ttu-id="6abf0-126">Další kroky</span><span class="sxs-lookup"><span data-stu-id="6abf0-126">Next steps</span></span>

<span data-ttu-id="6abf0-127">Další informace o rozhraní příkazového řádku Azure najdete v tématu [dokumentaci k rozhraní příkazového řádku Azure](https://docs.microsoft.com/cli/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="6abf0-127">For more information on the Azure CLI, see [Azure CLI documentation](https://docs.microsoft.com/cli/azure/overview).</span></span>

<span data-ttu-id="6abf0-128">Ukázky skriptu rozhraní příkazového řádku další virtuální počítač nachází v [virtuálního počítače Windows Azure dokumentaci](../windows/cli-samples.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="6abf0-128">Additional virtual machine CLI script samples can be found in the [Azure Windows VM documentation](../windows/cli-samples.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span>
