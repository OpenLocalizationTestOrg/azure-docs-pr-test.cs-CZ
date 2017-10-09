---
title: "aaaAzure ukázka skriptu rozhraní příkazového řádku - vytvoření virtuálního počítače s Linuxem pomocí NGINX | Microsoft Docs"
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
ms.openlocfilehash: 9166ccfd4f2e6eea731a8dc6956575d9f8f85488
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-vm-with-nginx"></a><span data-ttu-id="e9db9-103">Vytvoření virtuálního počítače s NGINX</span><span class="sxs-lookup"><span data-stu-id="e9db9-103">Create a VM with NGINX</span></span>

<span data-ttu-id="e9db9-104">Tento skript vytvoří virtuální počítač Azure a používá hello rozšíření vlastních skriptů Azure virtuálního počítače tooinstall NGINX.</span><span class="sxs-lookup"><span data-stu-id="e9db9-104">This script creates an Azure Virtual Machine and uses hello Azure Virtual Machine Custom Script Extension tooinstall NGINX.</span></span> <span data-ttu-id="e9db9-105">Po spuštění skriptu hello, můžete přejít na web ukázku hello veřejnou IP adresu hello virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="e9db9-105">After running hello script, you can access a demo website on hello public IP address of hello virtual machine.</span></span>

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a><span data-ttu-id="e9db9-106">Ukázkový skript</span><span class="sxs-lookup"><span data-stu-id="e9db9-106">Sample script</span></span>

[!code-azurecli-interactive[main](../../../cli_scripts/virtual-machine/create-vm-nginx/create-vm-nginx.sh "Quick Create VM")]

## <a name="custom-script-extension"></a><span data-ttu-id="e9db9-107">Rozšíření vlastních skriptů</span><span class="sxs-lookup"><span data-stu-id="e9db9-107">Custom Script Extension</span></span>

<span data-ttu-id="e9db9-108">rozšíření vlastních skriptů Hello zkopíruje tento skript na hello virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="e9db9-108">hello custom script extension copies this script onto hello virtual machine.</span></span> <span data-ttu-id="e9db9-109">skript Hello je tooinstall spusťte a nakonfigurujte webového serveru se službou NGINX.</span><span class="sxs-lookup"><span data-stu-id="e9db9-109">hello script is then run tooinstall and configure an NGINX web server.</span></span> 

```bash
#!/bin/bash

# update package source
apt-get -y update

# install NGINX
apt-get -y install nginx
```

## <a name="clean-up-deployment"></a><span data-ttu-id="e9db9-110">Vyčištění nasazení</span><span class="sxs-lookup"><span data-stu-id="e9db9-110">Clean up deployment</span></span> 

<span data-ttu-id="e9db9-111">Spusťte následující příkaz tooremove hello prostředků skupiny virtuálních počítačů a všechny související prostředky hello.</span><span class="sxs-lookup"><span data-stu-id="e9db9-111">Run hello following command tooremove hello resource group, VM, and all related resources.</span></span>

```azurecli-interactive 
az group delete --name myResourceGroup
```

## <a name="script-explanation"></a><span data-ttu-id="e9db9-112">Vysvětlení skriptu</span><span class="sxs-lookup"><span data-stu-id="e9db9-112">Script explanation</span></span>

<span data-ttu-id="e9db9-113">Tento skript používá hello následující příkazy toocreate skupinu prostředků, virtuální počítač, a všechny související prostředky.</span><span class="sxs-lookup"><span data-stu-id="e9db9-113">This script uses hello following commands toocreate a resource group, virtual machine, and all related resources.</span></span> <span data-ttu-id="e9db9-114">Každý příkaz v hello tabulky odkazů toocommand konkrétní dokumentaci.</span><span class="sxs-lookup"><span data-stu-id="e9db9-114">Each command in hello table links toocommand specific documentation.</span></span>

| <span data-ttu-id="e9db9-115">Příkaz</span><span class="sxs-lookup"><span data-stu-id="e9db9-115">Command</span></span> | <span data-ttu-id="e9db9-116">Poznámky</span><span class="sxs-lookup"><span data-stu-id="e9db9-116">Notes</span></span> |
|---|---|
| [<span data-ttu-id="e9db9-117">Vytvoření skupiny az</span><span class="sxs-lookup"><span data-stu-id="e9db9-117">az group create</span></span>](https://docs.microsoft.com/cli/azure/group#create) | <span data-ttu-id="e9db9-118">Vytvoří skupinu prostředků, ve kterém jsou uložené všechny prostředky.</span><span class="sxs-lookup"><span data-stu-id="e9db9-118">Creates a resource group in which all resources are stored.</span></span> |
| [<span data-ttu-id="e9db9-119">Vytvoření virtuálního počítače az</span><span class="sxs-lookup"><span data-stu-id="e9db9-119">az vm create</span></span>](https://docs.microsoft.com/cli/azure/vm#create) | <span data-ttu-id="e9db9-120">Vytvoří hello virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="e9db9-120">Creates hello virtual machine.</span></span> <span data-ttu-id="e9db9-121">Tento příkaz také určuje toobe bitové kopie virtuálního počítače hello používá a pověření pro správu.</span><span class="sxs-lookup"><span data-stu-id="e9db9-121">This command also specifies hello virtual machine image toobe used, and administrative credentials.</span></span>  |
| [<span data-ttu-id="e9db9-122">virtuální počítač az open-port</span><span class="sxs-lookup"><span data-stu-id="e9db9-122">az vm open-port</span></span>](https://docs.microsoft.com/cli/azure/network/nsg/rule#create) | <span data-ttu-id="e9db9-123">Vytvoří tooallow pravidla skupiny zabezpečení sítě příchozí přenosy.</span><span class="sxs-lookup"><span data-stu-id="e9db9-123">Creates a network security group rule tooallow inbound traffic.</span></span> <span data-ttu-id="e9db9-124">V této ukázce je otevřen port 80 pro přenosy protokolu HTTP.</span><span class="sxs-lookup"><span data-stu-id="e9db9-124">In this sample, port 80 is opened for HTTP traffic.</span></span> |
| [<span data-ttu-id="e9db9-125">sada rozšíření virtuálního počítače Azure</span><span class="sxs-lookup"><span data-stu-id="e9db9-125">azure vm extension set</span></span>](https://docs.microsoft.com/cli/azure/vm/extension#set) | <span data-ttu-id="e9db9-126">Přidá a běží virtuálním počítači rozšíření tooa virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="e9db9-126">Adds and runs a virtual machine extension tooa VM.</span></span> <span data-ttu-id="e9db9-127">V této ukázce je rozšíření vlastních skriptů hello použité tooinstall NGINX.</span><span class="sxs-lookup"><span data-stu-id="e9db9-127">In this sample, hello custom script extension is used tooinstall NGINX.</span></span>|
| [<span data-ttu-id="e9db9-128">Odstranění skupiny az</span><span class="sxs-lookup"><span data-stu-id="e9db9-128">az group delete</span></span>](https://docs.microsoft.com/cli/azure/vm/extension#set) | <span data-ttu-id="e9db9-129">Odstraní skupinu prostředků, včetně všech vnořených prostředků.</span><span class="sxs-lookup"><span data-stu-id="e9db9-129">Deletes a resource group including all nested resources.</span></span> |

## <a name="next-steps"></a><span data-ttu-id="e9db9-130">Další kroky</span><span class="sxs-lookup"><span data-stu-id="e9db9-130">Next steps</span></span>

<span data-ttu-id="e9db9-131">Další informace o hello rozhraní příkazového řádku Azure najdete v tématu [dokumentaci k rozhraní příkazového řádku Azure](https://docs.microsoft.com/cli/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="e9db9-131">For more information on hello Azure CLI, see [Azure CLI documentation](https://docs.microsoft.com/cli/azure/overview).</span></span>

<span data-ttu-id="e9db9-132">Ukázky skriptu rozhraní příkazového řádku další virtuální počítač nachází v hello [virtuální počítač Azure s Linuxem dokumentaci](../linux/cli-samples.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="e9db9-132">Additional virtual machine CLI script samples can be found in hello [Azure Linux VM documentation](../linux/cli-samples.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span>
