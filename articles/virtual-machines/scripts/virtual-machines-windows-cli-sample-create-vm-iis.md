---
title: "Ukázka skriptu rozhraní příkazového řádku Azure – instalace služby IIS | Microsoft Docs"
description: "Ukázka skriptu rozhraní příkazového řádku Azure – instalace služby IIS"
services: virtual-machines-Windows
documentationcenter: virtual-machines
author: neilpeterson
manager: timlt
editor: tysonn
tags: 
ms.assetid: 
ms.service: virtual-machines-Windows
ms.devlang: azurecli
ms.topic: sample
ms.tgt_pltfrm: vm-Windows
ms.workload: infrastructure
ms.date: 02/28/2017
ms.author: nepeters
ms.openlocfilehash: 426418c01b23845372443af5b8f4e826fb321f7d
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="quick-create-a-virtual-machine-with-the-azure-cli"></a><span data-ttu-id="f6165-103">Rychlé vytvoření virtuálního počítače pomocí rozhraní příkazového řádku Azure</span><span class="sxs-lookup"><span data-stu-id="f6165-103">Quick Create a virtual machine with the Azure CLI</span></span>

<span data-ttu-id="f6165-104">Tento skript vytvoří virtuální počítač Azure systémem Windows Server 2016 a používá k instalaci IIS rozšíření vlastních skriptů virtuálního počítače Azure.</span><span class="sxs-lookup"><span data-stu-id="f6165-104">This script creates an Azure Virtual Machine running Windows Server 2016, and uses the Azure Virtual Machine Custom Script Extension to install IIS.</span></span> <span data-ttu-id="f6165-105">Po spuštění skriptu, můžete přejít na výchozí web služby IIS na veřejnou IP adresu virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="f6165-105">After running the script, you can access the default IIS website on the public IP address of the virtual machine.</span></span>

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a><span data-ttu-id="f6165-106">Ukázkový skript</span><span class="sxs-lookup"><span data-stu-id="f6165-106">Sample script</span></span>

<span data-ttu-id="f6165-107">[!code-azurecli-interactive[hlavní](../../../cli_scripts/virtual-machine/create-vm-windows-iis/create-vm-windows-iis.sh "rychlé vytvoření virtuálního počítače")]</span><span class="sxs-lookup"><span data-stu-id="f6165-107">[!code-azurecli-interactive[main](../../../cli_scripts/virtual-machine/create-vm-windows-iis/create-vm-windows-iis.sh "Quick Create VM")]</span></span>

## <a name="clean-up-deployment"></a><span data-ttu-id="f6165-108">Vyčištění nasazení</span><span class="sxs-lookup"><span data-stu-id="f6165-108">Clean up deployment</span></span> 

<span data-ttu-id="f6165-109">Spusťte následující příkaz pro odebrání skupiny prostředků, virtuální počítač a všechny související prostředky.</span><span class="sxs-lookup"><span data-stu-id="f6165-109">Run the following command to remove the resource group, VM, and all related resources.</span></span>

```azurecli-interactive 
az group delete --name myResourceGroup --yes
```

## <a name="script-explanation"></a><span data-ttu-id="f6165-110">Vysvětlení skriptu</span><span class="sxs-lookup"><span data-stu-id="f6165-110">Script explanation</span></span>

<span data-ttu-id="f6165-111">Tento skript používá následující příkazy k vytvoření skupiny prostředků, virtuální počítač a všechny související prostředky.</span><span class="sxs-lookup"><span data-stu-id="f6165-111">This script uses the following commands to create a resource group, virtual machine, and all related resources.</span></span> <span data-ttu-id="f6165-112">Každý příkaz v tabulce odkazy na dokumentaci konkrétní příkaz.</span><span class="sxs-lookup"><span data-stu-id="f6165-112">Each command in the table links to command specific documentation.</span></span>

| <span data-ttu-id="f6165-113">Příkaz</span><span class="sxs-lookup"><span data-stu-id="f6165-113">Command</span></span> | <span data-ttu-id="f6165-114">Poznámky</span><span class="sxs-lookup"><span data-stu-id="f6165-114">Notes</span></span> |
|---|---|
| [<span data-ttu-id="f6165-115">Vytvoření skupiny az</span><span class="sxs-lookup"><span data-stu-id="f6165-115">az group create</span></span>](https://docs.microsoft.com/cli/azure/group#create) | <span data-ttu-id="f6165-116">Vytvoří skupinu prostředků, ve kterém jsou uložené všechny prostředky.</span><span class="sxs-lookup"><span data-stu-id="f6165-116">Creates a resource group in which all resources are stored.</span></span> |
| [<span data-ttu-id="f6165-117">Vytvoření virtuálního počítače az</span><span class="sxs-lookup"><span data-stu-id="f6165-117">az vm create</span></span>](https://docs.microsoft.com/cli/azure/vm#create) | <span data-ttu-id="f6165-118">Vytvoří virtuální počítač a připojí jej k síťové karty, virtuální sítě, podsítě a skupinu zabezpečení sítě.</span><span class="sxs-lookup"><span data-stu-id="f6165-118">Creates the virtual machine and connects it to the network card, virtual network, subnet, and network security group.</span></span> <span data-ttu-id="f6165-119">Tento příkaz také určuje image virtuálního počítače jako přihlašovací údaje použité a správu.</span><span class="sxs-lookup"><span data-stu-id="f6165-119">This command also specifies the virtual machine image to be used and administrative credentials.</span></span>  |
| [<span data-ttu-id="f6165-120">virtuální počítač az open-port</span><span class="sxs-lookup"><span data-stu-id="f6165-120">az vm open-port</span></span>](https://docs.microsoft.com/cli/azure/network/nsg/rule#create) | <span data-ttu-id="f6165-121">Vytvoří pravidlo skupiny zabezpečení sítě chcete povolit příchozí přenosy.</span><span class="sxs-lookup"><span data-stu-id="f6165-121">Creates a network security group rule to allow inbound traffic.</span></span> <span data-ttu-id="f6165-122">V této ukázce je otevřen port 80 pro přenosy protokolu HTTP.</span><span class="sxs-lookup"><span data-stu-id="f6165-122">In this sample, port 80 is opened for HTTP traffic.</span></span> |
| [<span data-ttu-id="f6165-123">sada rozšíření virtuálního počítače Azure</span><span class="sxs-lookup"><span data-stu-id="f6165-123">azure vm extension set</span></span>](https://docs.microsoft.com/cli/azure/vm/extension#set) | <span data-ttu-id="f6165-124">Přidá a spustí rozšíření virtuálního počítače k virtuálnímu počítači.</span><span class="sxs-lookup"><span data-stu-id="f6165-124">Adds and runs a virtual machine extension to a VM.</span></span> <span data-ttu-id="f6165-125">V této ukázce se používá rozšíření vlastních skriptů instalace služby IIS.</span><span class="sxs-lookup"><span data-stu-id="f6165-125">In this sample, the custom script extension is used to install IIS.</span></span>|
| [<span data-ttu-id="f6165-126">Odstranění skupiny az</span><span class="sxs-lookup"><span data-stu-id="f6165-126">az group delete</span></span>](https://docs.microsoft.com/cli/azure/vm/extension#set) | <span data-ttu-id="f6165-127">Odstraní skupinu prostředků, včetně všech vnořených prostředků.</span><span class="sxs-lookup"><span data-stu-id="f6165-127">Deletes a resource group including all nested resources.</span></span> |

## <a name="next-steps"></a><span data-ttu-id="f6165-128">Další kroky</span><span class="sxs-lookup"><span data-stu-id="f6165-128">Next steps</span></span>

<span data-ttu-id="f6165-129">Další informace o rozhraní příkazového řádku Azure najdete v tématu [dokumentaci k rozhraní příkazového řádku Azure](https://docs.microsoft.com/cli/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="f6165-129">For more information on the Azure CLI, see [Azure CLI documentation](https://docs.microsoft.com/cli/azure/overview).</span></span>

<span data-ttu-id="f6165-130">Ukázky skriptu rozhraní příkazového řádku další virtuální počítač nachází v [virtuálního počítače Windows Azure dokumentaci](../windows/cli-samples.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="f6165-130">Additional virtual machine CLI script samples can be found in the [Azure Windows VM documentation](../windows/cli-samples.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span>
