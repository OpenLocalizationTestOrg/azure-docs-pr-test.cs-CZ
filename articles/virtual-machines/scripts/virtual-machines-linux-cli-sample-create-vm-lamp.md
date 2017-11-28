---
title: "aaaAzure ukázka skriptu rozhraní příkazového řádku - nasazení hello svítilna zásobníku v architektuře s vyrovnáváním zatížení virtuální Machin Škálovací sadě | Microsoft Docs"
description: "Použít vlastní skript rozšíření toodeploy hello svítilna zásobníku v zatížení = sad v Azure škálování vyrovnáváním virtuálního počítače."
services: virtual-machines-linux
documentationcenter: virtual-machines
author: allclark
manager: douge
editor: tysonn
tags: azure-service-management
ms.assetid: 
ms.service: virtual-machines-linux
ms.devlang: azurecli
ms.topic: sample
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 04/05/2017
ms.author: allclark
ms.custom: mvc
ms.openlocfilehash: d5278db809faaa0997a08b00a53387d754fce3d3
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="deploy-hello-lamp-stack-in-a-load-balanced-virtual-machine-scale-set"></a><span data-ttu-id="0e34a-103">Nasazení hello svítilna zásobníku v sadě škálování Vyrovnávání zatížení sítě virtuálních počítačů</span><span class="sxs-lookup"><span data-stu-id="0e34a-103">Deploy hello LAMP stack in a load-balanced virtual machine scale set</span></span>

<span data-ttu-id="0e34a-104">Tento příklad vytvoří škálovací sadu virtuálních počítačů a platí rozšíření, které běží vlastní skript toodeploy hello svítilna zásobníku na každém virtuálním počítači v sadě škálování hello.</span><span class="sxs-lookup"><span data-stu-id="0e34a-104">This example creates a virtual machine scale set and applies an extension that runs a custom script toodeploy hello LAMP stack on each virtual machine in hello scale set.</span></span>

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

## <a name="sample-script"></a><span data-ttu-id="0e34a-105">Ukázkový skript</span><span class="sxs-lookup"><span data-stu-id="0e34a-105">Sample script</span></span>

[!code-azurecli-interactive[main](../../../cli_scripts/virtual-machine/create-scaleset-php-ansible/build-stack.sh "Create virtual machine scale set with LAMP stack")]

## <a name="connect"></a><span data-ttu-id="0e34a-106">Připojení</span><span class="sxs-lookup"><span data-stu-id="0e34a-106">Connect</span></span>

<span data-ttu-id="0e34a-107">Použijte tento kód toosee, jak nastavit tooconnect tooyour virtuální počítače a vaše škálování.</span><span class="sxs-lookup"><span data-stu-id="0e34a-107">Use this code toosee how tooconnect tooyour VMs and your scale set.</span></span>

[!code-azurecli[main](../../../cli_scripts/virtual-machine/create-scaleset-php-ansible/how-to-access.sh "Access hello virtual machine scale set")]

## <a name="clean-up-deployment"></a><span data-ttu-id="0e34a-108">Vyčištění nasazení</span><span class="sxs-lookup"><span data-stu-id="0e34a-108">Clean up deployment</span></span> 

<span data-ttu-id="0e34a-109">Spusťte následující příkaz tooremove hello prostředků skupiny, sadě škálování hello a virtuální počítače a všechny související prostředky hello.</span><span class="sxs-lookup"><span data-stu-id="0e34a-109">Run hello following command tooremove hello resource group, hello scale set and VMs, and all related resources.</span></span>

```azurecli-interactive 
az group delete -n myResourceGroup
```

## <a name="script-explanation"></a><span data-ttu-id="0e34a-110">Vysvětlení skriptu</span><span class="sxs-lookup"><span data-stu-id="0e34a-110">Script explanation</span></span>

<span data-ttu-id="0e34a-111">Tento skript používá hello následující příkazy toocreate skupinu prostředků, virtuální počítač, skupinu dostupnosti, nástroj pro vyrovnávání zatížení a všechny související prostředky.</span><span class="sxs-lookup"><span data-stu-id="0e34a-111">This script uses hello following commands toocreate a resource group, virtual machine, availability set, load balancer, and all related resources.</span></span> <span data-ttu-id="0e34a-112">Každý příkaz v hello tabulky odkazů toocommand konkrétní dokumentaci.</span><span class="sxs-lookup"><span data-stu-id="0e34a-112">Each command in hello table links toocommand specific documentation.</span></span>

| <span data-ttu-id="0e34a-113">Příkaz</span><span class="sxs-lookup"><span data-stu-id="0e34a-113">Command</span></span> | <span data-ttu-id="0e34a-114">Poznámky</span><span class="sxs-lookup"><span data-stu-id="0e34a-114">Notes</span></span> |
|---|---|
| [<span data-ttu-id="0e34a-115">Vytvoření skupiny az</span><span class="sxs-lookup"><span data-stu-id="0e34a-115">az group create</span></span>](https://docs.microsoft.com/cli/azure/group#create) | <span data-ttu-id="0e34a-116">Vytvoří skupinu prostředků, ve kterém jsou uložené všechny prostředky.</span><span class="sxs-lookup"><span data-stu-id="0e34a-116">Creates a resource group in which all resources are stored.</span></span> |
| [<span data-ttu-id="0e34a-117">Vytvoření az vmss</span><span class="sxs-lookup"><span data-stu-id="0e34a-117">az vmss create</span></span>](https://docs.microsoft.com/cli/azure/vmss#create) | <span data-ttu-id="0e34a-118">Vytvoří škálovací sadu virtuálních počítačů</span><span class="sxs-lookup"><span data-stu-id="0e34a-118">Creates a virtual machine scale set</span></span> |
| [<span data-ttu-id="0e34a-119">Vytvořit pravidlo lb az sítě</span><span class="sxs-lookup"><span data-stu-id="0e34a-119">az network lb rule create</span></span>](https://docs.microsoft.com/cli/azure/network/lb/rule#create) | <span data-ttu-id="0e34a-120">Přidat koncový bod Vyrovnávání zatížení sítě</span><span class="sxs-lookup"><span data-stu-id="0e34a-120">Add a load-balanced endpoint</span></span> |
| [<span data-ttu-id="0e34a-121">sada rozšíření vmss az</span><span class="sxs-lookup"><span data-stu-id="0e34a-121">az vmss extension set</span></span>](https://docs.microsoft.com/cli/azure/vmss/extension#set) | <span data-ttu-id="0e34a-122">Vytváření rozšíření hello, který spouští hello vlastních skriptů při nasazení virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="0e34a-122">Create hello extension that runs hello custom script on deployment of a VM</span></span> |
| [<span data-ttu-id="0e34a-123">AZ vmss aktualizace instance</span><span class="sxs-lookup"><span data-stu-id="0e34a-123">az vmss update-instances</span></span>](https://docs.microsoft.com/cli/azure/vmss#update-instances) | <span data-ttu-id="0e34a-124">Spusťte skript vlastní hello hello instancí virtuálních počítačů, které byly nasazené, než bylo použito rozšíření hello toohello škálovací sadu.</span><span class="sxs-lookup"><span data-stu-id="0e34a-124">Run hello custom script on hello VM instances that were deployed before hello extension was applied toohello scale set.</span></span> |
| [<span data-ttu-id="0e34a-125">AZ vmss škálování</span><span class="sxs-lookup"><span data-stu-id="0e34a-125">az vmss scale</span></span>](https://docs.microsoft.com/cli/azure/vmss#scale) | <span data-ttu-id="0e34a-126">Škálování hello škálování nastavený přidáním více instancí virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="0e34a-126">Scale up hello scale set by adding more VM instances.</span></span> <span data-ttu-id="0e34a-127">vlastní skript Hello běží v nich při jejich nasazení.</span><span class="sxs-lookup"><span data-stu-id="0e34a-127">hello custom script is run on these when they are deployed.</span></span> |
| [<span data-ttu-id="0e34a-128">seznam veřejné ip az sítě</span><span class="sxs-lookup"><span data-stu-id="0e34a-128">az network public-ip list</span></span>](https://docs.microsoft.com/cli/azure/network/public-ip#list) | <span data-ttu-id="0e34a-129">Získáte IP adresy hello hello virtuální počítače vytvořené ukázka hello.</span><span class="sxs-lookup"><span data-stu-id="0e34a-129">Get hello IP addresses of hello VMs created by hello sample.</span></span> |
| [<span data-ttu-id="0e34a-130">Zobrazit lb az sítě</span><span class="sxs-lookup"><span data-stu-id="0e34a-130">az network lb show</span></span>](https://docs.microsoft.com/cli/azure/network/lb#show) | <span data-ttu-id="0e34a-131">Porty používané pro vyrovnávání zatížení hello získáte hello front-end a back-end.</span><span class="sxs-lookup"><span data-stu-id="0e34a-131">Get hello frontend and backend ports used by hello load balancer.</span></span> |

## <a name="next-steps"></a><span data-ttu-id="0e34a-132">Další kroky</span><span class="sxs-lookup"><span data-stu-id="0e34a-132">Next steps</span></span>

<span data-ttu-id="0e34a-133">Další informace o hello rozhraní příkazového řádku Azure najdete v tématu [dokumentaci k rozhraní příkazového řádku Azure](https://docs.microsoft.com/cli/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="0e34a-133">For more information on hello Azure CLI, see [Azure CLI documentation](https://docs.microsoft.com/cli/azure/overview).</span></span>

<span data-ttu-id="0e34a-134">Ukázky skriptu rozhraní příkazového řádku další virtuální počítač nachází v hello [virtuální počítač Azure s Linuxem dokumentaci](../linux/cli-samples.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="0e34a-134">Additional virtual machine CLI script samples can be found in hello [Azure Linux VM documentation](../linux/cli-samples.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span>
