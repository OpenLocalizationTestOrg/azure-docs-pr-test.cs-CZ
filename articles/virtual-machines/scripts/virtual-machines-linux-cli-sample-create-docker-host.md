---
title: "aaaAzure ukázka skriptu rozhraní příkazového řádku - vytvořit hostitele Docker | Microsoft Docs"
description: "Azure CLI skriptu ukázkové – vytvořit hostitele Docker"
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
ms.date: 03/15/2017
ms.author: nepeters
ms.custom: mvc
ms.openlocfilehash: 7df2561c428ff5d3ab0a0d53c8e046781996be77
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-vm-with-docker"></a><span data-ttu-id="df8ad-103">Vytvoření virtuálního počítače s Docker</span><span class="sxs-lookup"><span data-stu-id="df8ad-103">Create a VM with Docker</span></span>

<span data-ttu-id="df8ad-104">Tento skript vytvoří virtuální počítač s Docker povolené a spustí kontejner Docker systémem NGINX.</span><span class="sxs-lookup"><span data-stu-id="df8ad-104">This script creates a virtual machine with Docker enabled and starts a Docker container running NGINX.</span></span> <span data-ttu-id="df8ad-105">Po spuštění skriptu hello, můžete přístup hello NGINX webovému serveru přes hello plně kvalifikovaný název domény hello virtuální počítač Azure.</span><span class="sxs-lookup"><span data-stu-id="df8ad-105">After running hello script, you can access hello NGINX web server through hello FQDN of hello Azure virtual machine.</span></span> 

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a><span data-ttu-id="df8ad-106">Ukázkový skript</span><span class="sxs-lookup"><span data-stu-id="df8ad-106">Sample script</span></span>

[!code-azurecli-interactive[main](../../../cli_scripts/virtual-machine/create-docker-host/create-docker-host.sh "Docker Host")]

## <a name="clean-up-deployment"></a><span data-ttu-id="df8ad-107">Vyčištění nasazení</span><span class="sxs-lookup"><span data-stu-id="df8ad-107">Clean up deployment</span></span> 

<span data-ttu-id="df8ad-108">Spusťte následující příkaz tooremove hello prostředků skupiny virtuálních počítačů a všechny související prostředky hello.</span><span class="sxs-lookup"><span data-stu-id="df8ad-108">Run hello following command tooremove hello resource group, VM, and all related resources.</span></span>

```azurecli-interactive 
az group delete --name myResourceGroup
```

## <a name="script-explanation"></a><span data-ttu-id="df8ad-109">Vysvětlení skriptu</span><span class="sxs-lookup"><span data-stu-id="df8ad-109">Script explanation</span></span>

<span data-ttu-id="df8ad-110">Tento skript používá hello následující příkazy toocreate hello nasazení.</span><span class="sxs-lookup"><span data-stu-id="df8ad-110">This script uses hello following commands toocreate hello deployment.</span></span> <span data-ttu-id="df8ad-111">Každou položku v tabulce hello propojí toocommand konkrétní dokumentaci.</span><span class="sxs-lookup"><span data-stu-id="df8ad-111">Each item in hello table links toocommand specific documentation.</span></span>

| <span data-ttu-id="df8ad-112">Příkaz</span><span class="sxs-lookup"><span data-stu-id="df8ad-112">Command</span></span> | <span data-ttu-id="df8ad-113">Poznámky</span><span class="sxs-lookup"><span data-stu-id="df8ad-113">Notes</span></span> |
|---|---|
| [<span data-ttu-id="df8ad-114">Vytvoření skupiny az</span><span class="sxs-lookup"><span data-stu-id="df8ad-114">az group create</span></span>](https://docs.microsoft.com/cli/azure/group#create) | <span data-ttu-id="df8ad-115">Vytvoří skupinu prostředků, ve kterém jsou uložené všechny prostředky.</span><span class="sxs-lookup"><span data-stu-id="df8ad-115">Creates a resource group in which all resources are stored.</span></span> |
| [<span data-ttu-id="df8ad-116">Vytvoření virtuálního počítače az</span><span class="sxs-lookup"><span data-stu-id="df8ad-116">az vm create</span></span>](https://docs.microsoft.com/cli/azure/vm#create) | <span data-ttu-id="df8ad-117">Vytvoří hello virtuální počítač a připojí ho toohello síťové karty, virtuální sítě, podsítě a skupinu zabezpečení sítě.</span><span class="sxs-lookup"><span data-stu-id="df8ad-117">Creates hello virtual machine and connects it toohello network card, virtual network, subnet, and network security group.</span></span> <span data-ttu-id="df8ad-118">Tento příkaz také určuje toobe bitové kopie virtuálního počítače hello používá a pověření pro správu.</span><span class="sxs-lookup"><span data-stu-id="df8ad-118">This command also specifies hello virtual machine image toobe used, and administrative credentials.</span></span>  |
| [<span data-ttu-id="df8ad-119">virtuální počítač az open-port</span><span class="sxs-lookup"><span data-stu-id="df8ad-119">az vm open-port</span></span>](https://docs.microsoft.com/cli/azure/vm#open-port) | <span data-ttu-id="df8ad-120">Vytvoří tooallow pravidla skupiny zabezpečení sítě příchozí přenosy.</span><span class="sxs-lookup"><span data-stu-id="df8ad-120">Creates a network security group rule tooallow inbound traffic.</span></span> <span data-ttu-id="df8ad-121">V této ukázce je otevřen port 80 pro přenosy protokolu HTTP.</span><span class="sxs-lookup"><span data-stu-id="df8ad-121">In this sample, port 80 is opened for HTTP traffic.</span></span> |
| [<span data-ttu-id="df8ad-122">sada rozšíření virtuálního počítače Azure</span><span class="sxs-lookup"><span data-stu-id="df8ad-122">azure vm extension set</span></span>](https://docs.microsoft.com/cli/azure/vm/extension#set) | <span data-ttu-id="df8ad-123">Přidá a běží virtuálním počítači rozšíření tooa virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="df8ad-123">Adds and runs a virtual machine extension tooa VM.</span></span> <span data-ttu-id="df8ad-124">V této ukázce je hello rozšíření virtuálního počítače Docker použité tooconfigure Docker hostitele.</span><span class="sxs-lookup"><span data-stu-id="df8ad-124">In this sample, hello Docker VM extension is used tooconfigure a Docker host.</span></span>|
| [<span data-ttu-id="df8ad-125">Odstranění skupiny az</span><span class="sxs-lookup"><span data-stu-id="df8ad-125">az group delete</span></span>](https://docs.microsoft.com/cli/azure/vm/extension#set) | <span data-ttu-id="df8ad-126">Odstraní skupinu prostředků, včetně všech vnořených prostředků.</span><span class="sxs-lookup"><span data-stu-id="df8ad-126">Deletes a resource group, including all nested resources.</span></span> |

## <a name="next-steps"></a><span data-ttu-id="df8ad-127">Další kroky</span><span class="sxs-lookup"><span data-stu-id="df8ad-127">Next steps</span></span>

<span data-ttu-id="df8ad-128">Další informace o hello rozhraní příkazového řádku Azure najdete v tématu [dokumentaci k rozhraní příkazového řádku Azure](https://docs.microsoft.com/cli/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="df8ad-128">For more information on hello Azure CLI, see [Azure CLI documentation](https://docs.microsoft.com/cli/azure/overview).</span></span>

<span data-ttu-id="df8ad-129">Ukázky skriptu rozhraní příkazového řádku další virtuální počítač nachází v hello [virtuální počítač Azure s Linuxem dokumentaci](../linux/cli-samples.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="df8ad-129">Additional virtual machine CLI script samples can be found in hello [Azure Linux VM documentation](../linux/cli-samples.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span>
