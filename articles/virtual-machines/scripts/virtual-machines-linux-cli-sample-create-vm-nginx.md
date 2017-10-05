---
title: "Rozhraní příkazového řádku Azure Script ukázka – vytvoření virtuálního počítače s Linuxem pomocí NGINX | Microsoft Docs"
description: "Rozhraní příkazového řádku Azure Script ukázka – vytvoření virtuálního počítače s Linuxem pomocí NGINX"
services: virtual-machines-linux
documentationcenter: virtual-machines
author: neilpeterson
manager: timlt
editor: tysonn
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-linux
ms.devlang: azurecli
ms.topic: sample
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 02/27/2017
ms.author: nepeters
ms.custom: mvc
ms.openlocfilehash: 416624d9e378d09f4fb0593119dbc30adeb09f91
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="create-a-vm-with-nginx"></a><span data-ttu-id="49d1f-103">Vytvoření virtuálního počítače s NGINX</span><span class="sxs-lookup"><span data-stu-id="49d1f-103">Create a VM with NGINX</span></span>

<span data-ttu-id="49d1f-104">Tento skript vytvoří virtuální počítač Azure a používá k instalaci NGINX rozšíření vlastních skriptů virtuálního počítače Azure.</span><span class="sxs-lookup"><span data-stu-id="49d1f-104">This script creates an Azure Virtual Machine and uses the Azure Virtual Machine Custom Script Extension to install NGINX.</span></span> <span data-ttu-id="49d1f-105">Po spuštění skriptu, můžete přejít na web ukázku veřejnou IP adresu virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="49d1f-105">After running the script, you can access a demo website on the public IP address of the virtual machine.</span></span>

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a><span data-ttu-id="49d1f-106">Ukázkový skript</span><span class="sxs-lookup"><span data-stu-id="49d1f-106">Sample script</span></span>

<span data-ttu-id="49d1f-107">[!code-azurecli-interactive[hlavní](../../../cli_scripts/virtual-machine/create-vm-nginx/create-vm-nginx.sh "rychlé vytvoření virtuálního počítače")]</span><span class="sxs-lookup"><span data-stu-id="49d1f-107">[!code-azurecli-interactive[main](../../../cli_scripts/virtual-machine/create-vm-nginx/create-vm-nginx.sh "Quick Create VM")]</span></span>

## <a name="custom-script-extension"></a><span data-ttu-id="49d1f-108">Rozšíření vlastních skriptů</span><span class="sxs-lookup"><span data-stu-id="49d1f-108">Custom Script Extension</span></span>

<span data-ttu-id="49d1f-109">Rozšíření vlastních skriptů zkopíruje tento skript do virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="49d1f-109">The custom script extension copies this script onto the virtual machine.</span></span> <span data-ttu-id="49d1f-110">K instalaci a konfiguraci webového serveru se službou NGINX je pak spusťte skript.</span><span class="sxs-lookup"><span data-stu-id="49d1f-110">The script is then run to install and configure an NGINX web server.</span></span> 

```bash
#!/bin/bash

# update package source
apt-get -y update

# install NGINX
apt-get -y install nginx
```

## <a name="clean-up-deployment"></a><span data-ttu-id="49d1f-111">Vyčištění nasazení</span><span class="sxs-lookup"><span data-stu-id="49d1f-111">Clean up deployment</span></span> 

<span data-ttu-id="49d1f-112">Spusťte následující příkaz pro odebrání skupiny prostředků, virtuální počítač a všechny související prostředky.</span><span class="sxs-lookup"><span data-stu-id="49d1f-112">Run the following command to remove the resource group, VM, and all related resources.</span></span>

```azurecli-interactive 
az group delete --name myResourceGroup
```

## <a name="script-explanation"></a><span data-ttu-id="49d1f-113">Vysvětlení skriptu</span><span class="sxs-lookup"><span data-stu-id="49d1f-113">Script explanation</span></span>

<span data-ttu-id="49d1f-114">Tento skript používá následující příkazy k vytvoření skupiny prostředků, virtuální počítač a všechny související prostředky.</span><span class="sxs-lookup"><span data-stu-id="49d1f-114">This script uses the following commands to create a resource group, virtual machine, and all related resources.</span></span> <span data-ttu-id="49d1f-115">Každý příkaz v tabulce odkazy na dokumentaci konkrétní příkaz.</span><span class="sxs-lookup"><span data-stu-id="49d1f-115">Each command in the table links to command specific documentation.</span></span>

| <span data-ttu-id="49d1f-116">Příkaz</span><span class="sxs-lookup"><span data-stu-id="49d1f-116">Command</span></span> | <span data-ttu-id="49d1f-117">Poznámky</span><span class="sxs-lookup"><span data-stu-id="49d1f-117">Notes</span></span> |
|---|---|
| [<span data-ttu-id="49d1f-118">Vytvoření skupiny az</span><span class="sxs-lookup"><span data-stu-id="49d1f-118">az group create</span></span>](https://docs.microsoft.com/cli/azure/group#create) | <span data-ttu-id="49d1f-119">Vytvoří skupinu prostředků, ve kterém jsou uložené všechny prostředky.</span><span class="sxs-lookup"><span data-stu-id="49d1f-119">Creates a resource group in which all resources are stored.</span></span> |
| [<span data-ttu-id="49d1f-120">Vytvoření virtuálního počítače az</span><span class="sxs-lookup"><span data-stu-id="49d1f-120">az vm create</span></span>](https://docs.microsoft.com/cli/azure/vm#create) | <span data-ttu-id="49d1f-121">Vytvoří virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="49d1f-121">Creates the virtual machine.</span></span> <span data-ttu-id="49d1f-122">Tento příkaz také Určuje bitovou kopii virtuálního počítače, který se má použít a pověření pro správu.</span><span class="sxs-lookup"><span data-stu-id="49d1f-122">This command also specifies the virtual machine image to be used, and administrative credentials.</span></span>  |
| [<span data-ttu-id="49d1f-123">virtuální počítač az open-port</span><span class="sxs-lookup"><span data-stu-id="49d1f-123">az vm open-port</span></span>](https://docs.microsoft.com/cli/azure/network/nsg/rule#create) | <span data-ttu-id="49d1f-124">Vytvoří pravidlo skupiny zabezpečení sítě chcete povolit příchozí přenosy.</span><span class="sxs-lookup"><span data-stu-id="49d1f-124">Creates a network security group rule to allow inbound traffic.</span></span> <span data-ttu-id="49d1f-125">V této ukázce je otevřen port 80 pro přenosy protokolu HTTP.</span><span class="sxs-lookup"><span data-stu-id="49d1f-125">In this sample, port 80 is opened for HTTP traffic.</span></span> |
| [<span data-ttu-id="49d1f-126">sada rozšíření virtuálního počítače Azure</span><span class="sxs-lookup"><span data-stu-id="49d1f-126">azure vm extension set</span></span>](https://docs.microsoft.com/cli/azure/vm/extension#set) | <span data-ttu-id="49d1f-127">Přidá a spustí rozšíření virtuálního počítače k virtuálnímu počítači.</span><span class="sxs-lookup"><span data-stu-id="49d1f-127">Adds and runs a virtual machine extension to a VM.</span></span> <span data-ttu-id="49d1f-128">V této ukázce rozšíření vlastních skriptů je použít k instalaci NGINX.</span><span class="sxs-lookup"><span data-stu-id="49d1f-128">In this sample, the custom script extension is used to install NGINX.</span></span>|
| [<span data-ttu-id="49d1f-129">Odstranění skupiny az</span><span class="sxs-lookup"><span data-stu-id="49d1f-129">az group delete</span></span>](https://docs.microsoft.com/cli/azure/vm/extension#set) | <span data-ttu-id="49d1f-130">Odstraní skupinu prostředků, včetně všech vnořených prostředků.</span><span class="sxs-lookup"><span data-stu-id="49d1f-130">Deletes a resource group including all nested resources.</span></span> |

## <a name="next-steps"></a><span data-ttu-id="49d1f-131">Další kroky</span><span class="sxs-lookup"><span data-stu-id="49d1f-131">Next steps</span></span>

<span data-ttu-id="49d1f-132">Další informace o rozhraní příkazového řádku Azure najdete v tématu [dokumentaci k rozhraní příkazového řádku Azure](https://docs.microsoft.com/cli/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="49d1f-132">For more information on the Azure CLI, see [Azure CLI documentation](https://docs.microsoft.com/cli/azure/overview).</span></span>

<span data-ttu-id="49d1f-133">Ukázky skriptu rozhraní příkazového řádku další virtuální počítač nachází v [virtuální počítač Azure s Linuxem dokumentaci](../linux/cli-samples.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="49d1f-133">Additional virtual machine CLI script samples can be found in the [Azure Linux VM documentation](../linux/cli-samples.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span>
