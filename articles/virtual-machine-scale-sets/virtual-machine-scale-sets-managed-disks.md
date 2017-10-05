---
title: "Použití spravovaných disků se škálovacími sadami virtuálních počítačů Azure | Dokumentace Microsoftu"
description: "Naučte se, kdy a jak používat spravované disky se škálovacími sadami virtuálních počítačů."
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
ms.openlocfilehash: 3ab1d432a2f90db57b99f0e7d419d85e2958c308
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="azure-vm-scale-sets-and-managed-disks"></a><span data-ttu-id="f1346-103">Škálovací sady virtuálních počítačů Azure a spravované disky</span><span class="sxs-lookup"><span data-stu-id="f1346-103">Azure VM scale sets and managed disks</span></span>

<span data-ttu-id="f1346-104">[Škálovací sady virtuálních počítačů](/azure/virtual-machine-scale-sets/) Azure podporují virtuální počítače se spravovanými disky.</span><span class="sxs-lookup"><span data-stu-id="f1346-104">Azure [virtual machine scale sets](/azure/virtual-machine-scale-sets/) supports virtual machines with managed disks.</span></span> <span data-ttu-id="f1346-105">Použití spravovaných disků se škálovacími sadami má několik výhod:</span><span class="sxs-lookup"><span data-stu-id="f1346-105">Using managed disks with scale sets has several benefits, including:</span></span>

* <span data-ttu-id="f1346-106">Pro virtuální počítače škálovacích sad už není potřeba předem vytvářet a spravovat účty úložiště pro ukládání disků operačního systému.</span><span class="sxs-lookup"><span data-stu-id="f1346-106">You no longer need to pre-create and manage storage accounts to store the OS disks for the scale set VMs.</span></span>

* <span data-ttu-id="f1346-107">Spravované datové disky můžete připojit ke škálovací sadě.</span><span class="sxs-lookup"><span data-stu-id="f1346-107">You can attach managed data disks to the scale set.</span></span>

* <span data-ttu-id="f1346-108">Při použití spravovaného disku může mít škálovací sada kapacitu až 1 000 virtuálních počítačů, pokud je založená na imagi platformy, nebo 100 virtuálních počítačů, pokud je založená na vlastní imagi.</span><span class="sxs-lookup"><span data-stu-id="f1346-108">With managed disk, a scale set can have capacity as high as 1,000 VMs if based on a platform image or 100 VMs if based on a custom image.</span></span>

## <a name="get-started"></a><span data-ttu-id="f1346-109">Začínáme</span><span class="sxs-lookup"><span data-stu-id="f1346-109">Get started</span></span>

<span data-ttu-id="f1346-110">Jednoduchým způsobem, jak začít pracovat se škálovacími sadami spravovaného disku, je provést nasazení z webu Azure Portal.</span><span class="sxs-lookup"><span data-stu-id="f1346-110">A simple way to get started with managed disk scale sets is to deploy one from the Azure portal.</span></span> <span data-ttu-id="f1346-111">Další informace najdete v [tomto článku](./virtual-machine-scale-sets-portal-create.md).</span><span class="sxs-lookup"><span data-stu-id="f1346-111">For more information, see [this article](./virtual-machine-scale-sets-portal-create.md).</span></span> <span data-ttu-id="f1346-112">Dalším jednoduchým způsobem, jak začít, je nasadit škálovací sadu pomocí [Azure CLI 2.0](https://docs.microsoft.com/cli/azure/install-az-cli2).</span><span class="sxs-lookup"><span data-stu-id="f1346-112">Another simple way to get started is to use [Azure CLI 2.0](https://docs.microsoft.com/cli/azure/install-az-cli2) to deploy a scale set.</span></span> <span data-ttu-id="f1346-113">Následující příklad ukazuje, jak vytvořit škálovací sadu založenou na Ubuntu s 10 virtuálními počítači, z nichž každý má 50GB a 100GB datový disk:</span><span class="sxs-lookup"><span data-stu-id="f1346-113">The following example shows how to create an Ubuntu based scale set with 10 VMs, each with a 50-GB and 100-GB data disk:</span></span>

```azurecli
az group create -l southcentralus -n dsktest
az vmss create -g dsktest -n dskvmss --image ubuntults --instance-count 10 --data-disk-sizes-gb 50 100
```

<span data-ttu-id="f1346-114">Další možností je vyhledat v [úložišti šablon Azure Quickstart na GitHubu](https://github.com/Azure/azure-quickstart-templates) složky, které obsahují `vmss`, a podívat se na předem vytvořené příklady šablon, které nasazují škálovací sady.</span><span class="sxs-lookup"><span data-stu-id="f1346-114">Alternatively, you could look in the [Azure Quickstart Templates GitHub repo](https://github.com/Azure/azure-quickstart-templates) for folders that contain `vmss` to see pre-built examples of templates that deploy scale sets.</span></span> <span data-ttu-id="f1346-115">Ke zjištění, které ze šablon už používají spravované disky, můžete použít [tento seznam](https://github.com/Azure/azure-quickstart-templates/blob/master/managed-disk-support-list.md).</span><span class="sxs-lookup"><span data-stu-id="f1346-115">To tell which templates are already using managed disks, you can refer to [this list](https://github.com/Azure/azure-quickstart-templates/blob/master/managed-disk-support-list.md).</span></span>

## <a name="next-steps"></a><span data-ttu-id="f1346-116">Další kroky</span><span class="sxs-lookup"><span data-stu-id="f1346-116">Next steps</span></span>

<span data-ttu-id="f1346-117">Další obecné informace o spravovaných discích najdete v [tomto článku](../virtual-machines/windows/managed-disks-overview.md).</span><span class="sxs-lookup"><span data-stu-id="f1346-117">For more information on managed disks in general, see [this article](../virtual-machines/windows/managed-disks-overview.md).</span></span>

<span data-ttu-id="f1346-118">Postup převedení šablony Resource Manageru pro zřízení škálovací sady se spravovanými disky najdete v [tomto článku](./virtual-machine-scale-sets-convert-template-to-md.md).</span><span class="sxs-lookup"><span data-stu-id="f1346-118">To see how to convert a Resource Manager template to provision scale sets with managed disks, see [this article](./virtual-machine-scale-sets-convert-template-to-md.md).</span></span> <span data-ttu-id="f1346-119">Stejné úpravy šablon Resource Manageru platí i pro rozhraní Azure REST API.</span><span class="sxs-lookup"><span data-stu-id="f1346-119">The same modifications to the Resource Manager templates apply to the Azure REST API as well.</span></span>

<span data-ttu-id="f1346-120">Další informace o použití spravovaných datových disků se škálovacími sadami najdete v [tomto článku](./virtual-machine-scale-sets-attached-disks.md).</span><span class="sxs-lookup"><span data-stu-id="f1346-120">To learn more about using managed data disks with scale sets, see [this article](./virtual-machine-scale-sets-attached-disks.md).</span></span>

<span data-ttu-id="f1346-121">Pokud chcete začít pracovat s rozsáhlými škálovacími sadami, přečtěte si [tento článek](./virtual-machine-scale-sets-placement-groups.md).</span><span class="sxs-lookup"><span data-stu-id="f1346-121">To begin working with large scale sets, refer to [this article](./virtual-machine-scale-sets-placement-groups.md).</span></span>


