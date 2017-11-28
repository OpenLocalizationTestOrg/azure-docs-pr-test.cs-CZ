---
title: "aaaUsing spravované disky s Azure sady škálování virtuálního počítače | Microsoft Docs"
description: "Zjistěte, proč a jak spravovat toouse disky s sady škálování virtuálního počítače"
services: virtual-machine-scale-sets
documentationcenter: 
author: gatneil
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 76ac7fd7-2e05-4762-88ca-3b499e87906e
ms.service: virtual-machine-scale-sets
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 6/01/2017
ms.author: negat
ms.openlocfilehash: 0e2a21e9f8b114ae1c8b81e1e6124621366f5643
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-vm-scale-sets-and-managed-disks"></a><span data-ttu-id="379ed-103">Škálovací sady virtuálních počítačů Azure a spravované disky</span><span class="sxs-lookup"><span data-stu-id="379ed-103">Azure VM scale sets and managed disks</span></span>

<span data-ttu-id="379ed-104">[Škálovací sady virtuálních počítačů](/azure/virtual-machine-scale-sets/) Azure podporují virtuální počítače se spravovanými disky.</span><span class="sxs-lookup"><span data-stu-id="379ed-104">Azure [virtual machine scale sets](/azure/virtual-machine-scale-sets/) supports virtual machines with managed disks.</span></span> <span data-ttu-id="379ed-105">Použití spravovaných disků se škálovacími sadami má několik výhod:</span><span class="sxs-lookup"><span data-stu-id="379ed-105">Using managed disks with scale sets has several benefits, including:</span></span>

* <span data-ttu-id="379ed-106">Již nepotřebujete toopre – vytvoření a Správa disků hello OS toostore účty úložiště pro virtuální počítače sady škálování hello.</span><span class="sxs-lookup"><span data-stu-id="379ed-106">You no longer need toopre-create and manage storage accounts toostore hello OS disks for hello scale set VMs.</span></span>

* <span data-ttu-id="379ed-107">Spravované datové disky toohello škálovací sadu můžete připojit.</span><span class="sxs-lookup"><span data-stu-id="379ed-107">You can attach managed data disks toohello scale set.</span></span>

* <span data-ttu-id="379ed-108">Při použití spravovaného disku může mít škálovací sada kapacitu až 1 000 virtuálních počítačů, pokud je založená na imagi platformy, nebo 100 virtuálních počítačů, pokud je založená na vlastní imagi.</span><span class="sxs-lookup"><span data-stu-id="379ed-108">With managed disk, a scale set can have capacity as high as 1,000 VMs if based on a platform image or 100 VMs if based on a custom image.</span></span>

## <a name="get-started"></a><span data-ttu-id="379ed-109">Začínáme</span><span class="sxs-lookup"><span data-stu-id="379ed-109">Get started</span></span>

<span data-ttu-id="379ed-110">Jednoduchý způsob tooget začít s sady škálování spravovaného disku je toodeploy jeden z hello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="379ed-110">A simple way tooget started with managed disk scale sets is toodeploy one from hello Azure portal.</span></span> <span data-ttu-id="379ed-111">Další informace najdete v [tomto článku](./virtual-machine-scale-sets-portal-create.md).</span><span class="sxs-lookup"><span data-stu-id="379ed-111">For more information, see [this article](./virtual-machine-scale-sets-portal-create.md).</span></span> <span data-ttu-id="379ed-112">Jiné jednoduchý způsob tooget spuštění je toouse [Azure CLI 2.0](https://docs.microsoft.com/cli/azure/install-az-cli2) toodeploy sady škálování virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="379ed-112">Another simple way tooget started is toouse [Azure CLI 2.0](https://docs.microsoft.com/cli/azure/install-az-cli2) toodeploy a scale set.</span></span> <span data-ttu-id="379ed-113">Hello následující příklad ukazuje, jak toocreate Ubuntu na základě škálování s 10 virtuálních počítačů, každý s 50 GB a 100 GB datový disk:</span><span class="sxs-lookup"><span data-stu-id="379ed-113">hello following example shows how toocreate an Ubuntu based scale set with 10 VMs, each with a 50-GB and 100-GB data disk:</span></span>

```azurecli
az group create -l southcentralus -n dsktest
az vmss create -g dsktest -n dskvmss --image ubuntults --instance-count 10 --data-disk-sizes-gb 50 100
```

<span data-ttu-id="379ed-114">Alternativně může hledat v hello [úložiště GitHub šablony Azure rychlý Start](https://github.com/Azure/azure-quickstart-templates) pro složky, které obsahují `vmss` toosee předem vytvořené Příklady šablon, které nasadit sady škálování.</span><span class="sxs-lookup"><span data-stu-id="379ed-114">Alternatively, you could look in hello [Azure Quickstart Templates GitHub repo](https://github.com/Azure/azure-quickstart-templates) for folders that contain `vmss` toosee pre-built examples of templates that deploy scale sets.</span></span> <span data-ttu-id="379ed-115">tootell šablony, které už používáte spravovaných disků, naleznete v části příliš[tento seznam](https://github.com/Azure/azure-quickstart-templates/blob/master/managed-disk-support-list.md).</span><span class="sxs-lookup"><span data-stu-id="379ed-115">tootell which templates are already using managed disks, you can refer too[this list](https://github.com/Azure/azure-quickstart-templates/blob/master/managed-disk-support-list.md).</span></span>

## <a name="next-steps"></a><span data-ttu-id="379ed-116">Další kroky</span><span class="sxs-lookup"><span data-stu-id="379ed-116">Next steps</span></span>

<span data-ttu-id="379ed-117">Další obecné informace o spravovaných discích najdete v [tomto článku](../virtual-machines/windows/managed-disks-overview.md).</span><span class="sxs-lookup"><span data-stu-id="379ed-117">For more information on managed disks in general, see [this article](../virtual-machines/windows/managed-disks-overview.md).</span></span>

<span data-ttu-id="379ed-118">toosee způsob tooconvert nastaví škálování tooprovision šablony Resource Manageru pomocí správy disků, najdete v části [v tomto článku](./virtual-machine-scale-sets-convert-template-to-md.md).</span><span class="sxs-lookup"><span data-stu-id="379ed-118">toosee how tooconvert a Resource Manager template tooprovision scale sets with managed disks, see [this article](./virtual-machine-scale-sets-convert-template-to-md.md).</span></span> <span data-ttu-id="379ed-119">Hello stejné šablony Resource Manageru toohello změny použít také toohello rozhraní REST API Azure.</span><span class="sxs-lookup"><span data-stu-id="379ed-119">hello same modifications toohello Resource Manager templates apply toohello Azure REST API as well.</span></span>

<span data-ttu-id="379ed-120">toolearn Další informace o spravovaných datových disků pomocí sad škálování, najdete v části [v tomto článku](./virtual-machine-scale-sets-attached-disks.md).</span><span class="sxs-lookup"><span data-stu-id="379ed-120">toolearn more about using managed data disks with scale sets, see [this article](./virtual-machine-scale-sets-attached-disks.md).</span></span>

<span data-ttu-id="379ed-121">Práce se sadami velkém měřítku, toobegin odkazovat příliš[v tomto článku](./virtual-machine-scale-sets-placement-groups.md).</span><span class="sxs-lookup"><span data-stu-id="379ed-121">toobegin working with large scale sets, refer too[this article](./virtual-machine-scale-sets-placement-groups.md).</span></span>


