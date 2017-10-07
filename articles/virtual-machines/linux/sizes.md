---
title: "velikosti aaaLinux virtuálních počítačů v Azure | Microsoft Docs"
description: "Obsahuje seznam různých velikostech hello k dispozici pro virtuální počítače s Linuxem v Azure."
services: virtual-machines-linux
documentationcenter: 
author: jonbeck7
manager: timlt
editor: 
tags: azure-resource-manager,azure-service-management
ms.assetid: da681171-f045-4c80-a5a9-d8bd47964673
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 07/28/2017
ms.author: jonbeck
ms.openlocfilehash: 56cbe0a0d7d34def07636edba74c4c699e336012
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="sizes-for-linux-virtual-machines-in-azure"></a><span data-ttu-id="dac59-103">Velikosti pro virtuální počítače s Linuxem v Azure</span><span class="sxs-lookup"><span data-stu-id="dac59-103">Sizes for Linux virtual machines in Azure</span></span>
<span data-ttu-id="dac59-104">Tento článek popisuje hello dostupných velikostí a možnosti pro hello virtuální počítače Azure můžete toorun vaše Linux aplikace a úlohy.</span><span class="sxs-lookup"><span data-stu-id="dac59-104">This article describes hello available sizes and options for hello Azure virtual machines you can use toorun your Linux apps and workloads.</span></span> <span data-ttu-id="dac59-105">Nabízí taky toobe aspekty nasazení vědět, když plánujete toouse tyto prostředky.</span><span class="sxs-lookup"><span data-stu-id="dac59-105">It also provides deployment considerations toobe aware of when you're planning toouse these resources.</span></span> <span data-ttu-id="dac59-106">Tento článek je také k dispozici pro [virtuální počítače s Windows](../windows/sizes.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="dac59-106">This article is also available for [Windows virtual machines](../windows/sizes.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span>


| <span data-ttu-id="dac59-107">Typ</span><span class="sxs-lookup"><span data-stu-id="dac59-107">Type</span></span>                     | <span data-ttu-id="dac59-108">Velikosti</span><span class="sxs-lookup"><span data-stu-id="dac59-108">Sizes</span></span>           |    <span data-ttu-id="dac59-109">Popis</span><span class="sxs-lookup"><span data-stu-id="dac59-109">Description</span></span>       |
|--------------------------|-------------------|------------------------------------------------------------------------------------------------------------------------------------|
| [<span data-ttu-id="dac59-110">Obecné účely</span><span class="sxs-lookup"><span data-stu-id="dac59-110">General purpose</span></span>](sizes-general.md)          | <span data-ttu-id="dac59-111">Dsv3, Dv3, DSv2, Dv2, DS, D, Av2, A0-7</span><span class="sxs-lookup"><span data-stu-id="dac59-111">Dsv3, Dv3, DSv2, Dv2, DS, D, Av2, A0-7,</span></span>  | <span data-ttu-id="dac59-112">Vyvážený poměr procesorů k paměti.</span><span class="sxs-lookup"><span data-stu-id="dac59-112">Balanced CPU-to-memory ratio.</span></span> <span data-ttu-id="dac59-113">Ideální pro testování a vývoj, malé toomedium databáze a nízkou toomedium provoz webové servery.</span><span class="sxs-lookup"><span data-stu-id="dac59-113">Ideal for testing and development, small toomedium databases, and low toomedium traffic web servers.</span></span> |
| [<span data-ttu-id="dac59-114">Optimalizované z hlediska výpočetních služeb</span><span class="sxs-lookup"><span data-stu-id="dac59-114">Compute optimized</span></span>](sizes-compute.md)        | <span data-ttu-id="dac59-115">Služby FS, F</span><span class="sxs-lookup"><span data-stu-id="dac59-115">Fs, F</span></span>             | <span data-ttu-id="dac59-116">Vysoký poměr procesorů k paměti.</span><span class="sxs-lookup"><span data-stu-id="dac59-116">High CPU-to-memory ratio.</span></span> <span data-ttu-id="dac59-117">Je vhodný pro střední provoz webových serverů, síťových zařízení, lázně procesy a aplikační servery.</span><span class="sxs-lookup"><span data-stu-id="dac59-117">Good for medium traffic web servers, network appliances, bath processes, and application servers.</span></span>        |
| [<span data-ttu-id="dac59-118">Optimalizované z hlediska paměti</span><span class="sxs-lookup"><span data-stu-id="dac59-118">Memory optimized</span></span>](sizes-memory.md)         | <span data-ttu-id="dac59-119">Esv3, Ev3, M, GS, G, DSv2, DS, Dv2, D</span><span class="sxs-lookup"><span data-stu-id="dac59-119">Esv3, Ev3, M, GS, G, DSv2, DS, Dv2, D</span></span>   | <span data-ttu-id="dac59-120">Vysoký poměr paměť procesoru.</span><span class="sxs-lookup"><span data-stu-id="dac59-120">High memory-to-CPU ratio.</span></span> <span data-ttu-id="dac59-121">Výborně hodí pro servery relační databáze, střední toolarge mezipaměti a analýzy v paměti.</span><span class="sxs-lookup"><span data-stu-id="dac59-121">Great for relational database servers, medium toolarge caches, and in-memory analytics.</span></span>                 |
| [<span data-ttu-id="dac59-122">Optimalizované z hlediska úložiště</span><span class="sxs-lookup"><span data-stu-id="dac59-122">Storage optimized</span></span>](sizes-storage.md)        | <span data-ttu-id="dac59-123">Ls</span><span class="sxs-lookup"><span data-stu-id="dac59-123">Ls</span></span>                | <span data-ttu-id="dac59-124">Vysoká propustnost disku a V/V.</span><span class="sxs-lookup"><span data-stu-id="dac59-124">High disk throughput and IO.</span></span> <span data-ttu-id="dac59-125">Ideální pro databáze NoSQL, SQL a velké objemy dat.</span><span class="sxs-lookup"><span data-stu-id="dac59-125">Ideal for Big Data, SQL, and NoSQL databases.</span></span>                                                         |
| [<span data-ttu-id="dac59-126">GPU</span><span class="sxs-lookup"><span data-stu-id="dac59-126">GPU</span></span>](sizes-gpu.md)            | <span data-ttu-id="dac59-127">VS, NC</span><span class="sxs-lookup"><span data-stu-id="dac59-127">NV, NC</span></span>            | <span data-ttu-id="dac59-128">Specializované virtuální počítače cílené pro velkou grafické vykreslování a úpravy videa.</span><span class="sxs-lookup"><span data-stu-id="dac59-128">Specialized virtual machines targeted for heavy graphic rendering and video editing.</span></span> <span data-ttu-id="dac59-129">K dispozici jeden nebo více grafickými procesory.</span><span class="sxs-lookup"><span data-stu-id="dac59-129">Available with single or multiple GPUs.</span></span>       |
| [<span data-ttu-id="dac59-130">Vysokovýkonné výpočetní prostředí</span><span class="sxs-lookup"><span data-stu-id="dac59-130">High performance compute</span></span>](sizes-hpc.md) | <span data-ttu-id="dac59-131">H, A8 11</span><span class="sxs-lookup"><span data-stu-id="dac59-131">H, A8-11</span></span>          | <span data-ttu-id="dac59-132">Naše nejrychlejší a procesorově nejvýkonnější virtuální počítače s volitelnými síťovými rozhraními s vysokou propustností (RDMA).</span><span class="sxs-lookup"><span data-stu-id="dac59-132">Our fastest and most powerful CPU virtual machines with optional high-throughput network interfaces (RDMA).</span></span> 

<br>

- <span data-ttu-id="dac59-133">Informace o cenách z hello různé velikosti najdete v tématu [ceny služeb Virtual Machines](https://azure.microsoft.com/pricing/details/virtual-machines/#Linux).</span><span class="sxs-lookup"><span data-stu-id="dac59-133">For information about pricing of hello various sizes, see [Virtual Machines Pricing](https://azure.microsoft.com/pricing/details/virtual-machines/#Linux).</span></span> 
- <span data-ttu-id="dac59-134">Dostupnost velikostí virtuálních počítačů v oblastech Azure, najdete v části [produkty podle oblasti](https://azure.microsoft.com/regions/services/).</span><span class="sxs-lookup"><span data-stu-id="dac59-134">For availability of VM sizes in Azure regions, see [Products available by region](https://azure.microsoft.com/regions/services/).</span></span>
- <span data-ttu-id="dac59-135">Obecná omezení toosee na virtuálních počítačích Azure, najdete v části [předplatného Azure a omezení služby, kvóty a omezení](../../azure-subscription-service-limits.md).</span><span class="sxs-lookup"><span data-stu-id="dac59-135">toosee general limits on Azure VMs, see [Azure subscription and service limits, quotas, and constraints](../../azure-subscription-service-limits.md).</span></span>
- <span data-ttu-id="dac59-136">Další informace o [Azure výpočetní jednotky (ACU)](../windows/acu.md) můžete porovnat výpočetní výkon v Azure SKU.</span><span class="sxs-lookup"><span data-stu-id="dac59-136">Learn more about how [Azure compute units (ACU)](../windows/acu.md) can help you compare compute performance across Azure SKUs.</span></span>


## <a name="rest-api"></a><span data-ttu-id="dac59-137">Rozhraní REST API</span><span class="sxs-lookup"><span data-stu-id="dac59-137">Rest API</span></span>

<span data-ttu-id="dac59-138">Informace o používání tooquery hello REST API pro velikosti virtuálních počítačů najdete v tématu hello následující:</span><span class="sxs-lookup"><span data-stu-id="dac59-138">For information on using hello REST API tooquery for VM sizes, see hello following:</span></span>

- [<span data-ttu-id="dac59-139">Seznam dostupných velikostí virtuálních počítačů pro změnu velikosti</span><span class="sxs-lookup"><span data-stu-id="dac59-139">List available virtual machine sizes for resizing</span></span>](https://docs.microsoft.com/rest/api/compute/virtualmachines/virtualmachines-list-sizes-for-resizing)
- [<span data-ttu-id="dac59-140">Seznam dostupných velikostí virtuálních počítačů pro předplatné</span><span class="sxs-lookup"><span data-stu-id="dac59-140">List available virtual machine sizes for a subscription</span></span>](https://docs.microsoft.com/rest/api/compute/virtualmachines/virtualmachines-list-sizes-region)
- [<span data-ttu-id="dac59-141">Seznam dostupných velikostí virtuálních počítačů v nastavení dostupnosti</span><span class="sxs-lookup"><span data-stu-id="dac59-141">List available virtual machine sizes in an availability set</span></span>](
https://docs.microsoft.com/rest/api/compute/virtualmachines/virtualmachines-list-sizes-availability-set)

## <a name="acu"></a><span data-ttu-id="dac59-142">ACU</span><span class="sxs-lookup"><span data-stu-id="dac59-142">ACU</span></span>

<span data-ttu-id="dac59-143">Další informace o [Azure výpočetní jednotky (ACU)](acu.md) můžete porovnat výpočetní výkon v Azure SKU.</span><span class="sxs-lookup"><span data-stu-id="dac59-143">Learn more about how [Azure compute units (ACU)](acu.md) can help you compare compute performance across Azure SKUs.</span></span>

## <a name="next-steps"></a><span data-ttu-id="dac59-144">Další kroky</span><span class="sxs-lookup"><span data-stu-id="dac59-144">Next steps</span></span>

<span data-ttu-id="dac59-145">Další informace o hello různé velikosti virtuálních počítačů, které jsou k dispozici:</span><span class="sxs-lookup"><span data-stu-id="dac59-145">Learn more about hello different VM sizes that are available:</span></span>
- [<span data-ttu-id="dac59-146">Obecné účely</span><span class="sxs-lookup"><span data-stu-id="dac59-146">General purpose</span></span>](sizes-general.md)
- [<span data-ttu-id="dac59-147">Optimalizované z hlediska výpočetních služeb</span><span class="sxs-lookup"><span data-stu-id="dac59-147">Compute optimized</span></span>](sizes-compute.md)
- [<span data-ttu-id="dac59-148">Optimalizované z hlediska paměti</span><span class="sxs-lookup"><span data-stu-id="dac59-148">Memory optimized</span></span>](sizes-memory.md)
- [<span data-ttu-id="dac59-149">Optimalizované z hlediska úložiště</span><span class="sxs-lookup"><span data-stu-id="dac59-149">Storage optimized</span></span>](sizes-storage.md)
- [<span data-ttu-id="dac59-150">GPU</span><span class="sxs-lookup"><span data-stu-id="dac59-150">GPU</span></span>](sizes-gpu.md)
- [<span data-ttu-id="dac59-151">Vysokovýkonné výpočetní prostředí</span><span class="sxs-lookup"><span data-stu-id="dac59-151">High performance compute</span></span>](sizes-hpc.md)



