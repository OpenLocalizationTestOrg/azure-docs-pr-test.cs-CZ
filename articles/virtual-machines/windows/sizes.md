---
title: "velikosti aaaWindows virtuálních počítačů v Azure | Microsoft Docs"
description: "Obsahuje seznam různých velikostech hello k dispozici pro virtuální počítače s Windows v Azure."
services: virtual-machines-windows
documentationcenter: 
author: jonbeck7
manager: timlt
editor: 
tags: azure-resource-manager,azure-service-management
ms.assetid: aabf0d30-04eb-4d34-b44a-69f8bfb84f22
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure-services
ms.date: 07/28/2017
ms.author: jonbeck
ms.openlocfilehash: 80bd8241b134031c41b56224e841c7557c4845cf
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="sizes-for-windows-virtual-machines-in-azure"></a><span data-ttu-id="35ce3-103">Velikosti pro virtuální počítače s Windows v Azure</span><span class="sxs-lookup"><span data-stu-id="35ce3-103">Sizes for Windows virtual machines in Azure</span></span>

<span data-ttu-id="35ce3-104">Tento článek popisuje hello dostupných velikostí a možnosti pro hello virtuální počítače Azure můžete toorun vaše aplikace pro Windows a úlohy.</span><span class="sxs-lookup"><span data-stu-id="35ce3-104">This article describes hello available sizes and options for hello Azure virtual machines you can use toorun your Windows apps and workloads.</span></span> <span data-ttu-id="35ce3-105">Nabízí taky toobe aspekty nasazení vědět, když plánujete toouse tyto prostředky.</span><span class="sxs-lookup"><span data-stu-id="35ce3-105">It also provides deployment considerations toobe aware of when you're planning toouse these resources.</span></span>  <span data-ttu-id="35ce3-106">Tento článek je také k dispozici pro [virtuální počítače s Linuxem](../linux/sizes.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="35ce3-106">This article is also available for [Linux virtual machines](../linux/sizes.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span>


| <span data-ttu-id="35ce3-107">Typ</span><span class="sxs-lookup"><span data-stu-id="35ce3-107">Type</span></span>                     | <span data-ttu-id="35ce3-108">Velikosti</span><span class="sxs-lookup"><span data-stu-id="35ce3-108">Sizes</span></span>           |    <span data-ttu-id="35ce3-109">Popis</span><span class="sxs-lookup"><span data-stu-id="35ce3-109">Description</span></span>       |
|--------------------------|-------------------|------------------------------------------------------------------------------------------------------------------------------------|
| [<span data-ttu-id="35ce3-110">Obecné účely</span><span class="sxs-lookup"><span data-stu-id="35ce3-110">General purpose</span></span>](sizes-general.md)          | <span data-ttu-id="35ce3-111">Dsv3, Dv3, DSv2, Dv2, DS, D, Av2, A0 7</span><span class="sxs-lookup"><span data-stu-id="35ce3-111">Dsv3, Dv3, DSv2, Dv2, DS, D, Av2, A0-7</span></span> | <span data-ttu-id="35ce3-112">Vyvážený poměr procesorů k paměti.</span><span class="sxs-lookup"><span data-stu-id="35ce3-112">Balanced CPU-to-memory ratio.</span></span> <span data-ttu-id="35ce3-113">Ideální pro testování a vývoj, malé toomedium databáze a nízkou toomedium provoz webové servery.</span><span class="sxs-lookup"><span data-stu-id="35ce3-113">Ideal for testing and development, small toomedium databases, and low toomedium traffic web servers.</span></span> |
| [<span data-ttu-id="35ce3-114">Optimalizované z hlediska výpočetních služeb</span><span class="sxs-lookup"><span data-stu-id="35ce3-114">Compute optimized</span></span>](sizes-compute.md)        | <span data-ttu-id="35ce3-115">Služby FS, F</span><span class="sxs-lookup"><span data-stu-id="35ce3-115">Fs, F</span></span>             | <span data-ttu-id="35ce3-116">Vysoký poměr procesorů k paměti.</span><span class="sxs-lookup"><span data-stu-id="35ce3-116">High CPU-to-memory ratio.</span></span> <span data-ttu-id="35ce3-117">Vhodné pro webové servery se středním provozem, síťová zařízení, dávkové procesy a aplikační servery.</span><span class="sxs-lookup"><span data-stu-id="35ce3-117">Good for medium traffic web servers, network appliances, batch processes, and application servers.</span></span>        |
| [<span data-ttu-id="35ce3-118">Optimalizované z hlediska paměti</span><span class="sxs-lookup"><span data-stu-id="35ce3-118">Memory optimized</span></span>](../virtual-machines-windows-sizes-memory.md)         | <span data-ttu-id="35ce3-119">Esv3, Ev3, M, GS, G, DSv2, DS, Dv2, D</span><span class="sxs-lookup"><span data-stu-id="35ce3-119">Esv3, Ev3, M, GS, G, DSv2, DS, Dv2, D</span></span>   | <span data-ttu-id="35ce3-120">Vysoký poměr paměť procesoru.</span><span class="sxs-lookup"><span data-stu-id="35ce3-120">High memory-to-CPU ratio.</span></span> <span data-ttu-id="35ce3-121">Výborně hodí pro servery relační databáze, střední toolarge mezipaměti a analýzy v paměti.</span><span class="sxs-lookup"><span data-stu-id="35ce3-121">Great for relational database servers, medium toolarge caches, and in-memory analytics.</span></span>                 |
| [<span data-ttu-id="35ce3-122">Optimalizované z hlediska úložiště</span><span class="sxs-lookup"><span data-stu-id="35ce3-122">Storage optimized</span></span>](../virtual-machines-windows-sizes-storage.md)        | <span data-ttu-id="35ce3-123">Ls</span><span class="sxs-lookup"><span data-stu-id="35ce3-123">Ls</span></span>                | <span data-ttu-id="35ce3-124">Vysoká propustnost disku a V/V.</span><span class="sxs-lookup"><span data-stu-id="35ce3-124">High disk throughput and IO.</span></span> <span data-ttu-id="35ce3-125">Ideální pro databáze NoSQL, SQL a velké objemy dat.</span><span class="sxs-lookup"><span data-stu-id="35ce3-125">Ideal for Big Data, SQL, and NoSQL databases.</span></span>                                                         |
| [<span data-ttu-id="35ce3-126">GPU</span><span class="sxs-lookup"><span data-stu-id="35ce3-126">GPU</span></span>](sizes-gpu.md)            | <span data-ttu-id="35ce3-127">VS, NC</span><span class="sxs-lookup"><span data-stu-id="35ce3-127">NV, NC</span></span>            | <span data-ttu-id="35ce3-128">Specializované virtuální počítače cílené pro velkou grafické vykreslování a úpravy videa.</span><span class="sxs-lookup"><span data-stu-id="35ce3-128">Specialized virtual machines targeted for heavy graphic rendering and video editing.</span></span> <span data-ttu-id="35ce3-129">K dispozici jeden nebo více grafickými procesory.</span><span class="sxs-lookup"><span data-stu-id="35ce3-129">Available with single or multiple GPUs.</span></span>       |
| [<span data-ttu-id="35ce3-130">Vysokovýkonné výpočetní prostředí</span><span class="sxs-lookup"><span data-stu-id="35ce3-130">High performance compute</span></span>](sizes-hpc.md) | <span data-ttu-id="35ce3-131">H, A8 11</span><span class="sxs-lookup"><span data-stu-id="35ce3-131">H, A8-11</span></span>          | <span data-ttu-id="35ce3-132">Naše nejrychlejší a procesorově nejvýkonnější virtuální počítače s volitelnými síťovými rozhraními s vysokou propustností (RDMA).</span><span class="sxs-lookup"><span data-stu-id="35ce3-132">Our fastest and most powerful CPU virtual machines with optional high-throughput network interfaces (RDMA).</span></span> 

<br> 

- <span data-ttu-id="35ce3-133">Informace o cenách z hello různé velikosti najdete v tématu [ceny služeb Virtual Machines](https://azure.microsoft.com/pricing/details/virtual-machines/#Windows).</span><span class="sxs-lookup"><span data-stu-id="35ce3-133">For information about pricing of hello various sizes, see [Virtual Machines Pricing](https://azure.microsoft.com/pricing/details/virtual-machines/#Windows).</span></span> 
- <span data-ttu-id="35ce3-134">Obecná omezení toosee na virtuálních počítačích Azure, najdete v části [předplatného Azure a omezení služby, kvóty a omezení](../../azure-subscription-service-limits.md).</span><span class="sxs-lookup"><span data-stu-id="35ce3-134">toosee general limits on Azure VMs, see [Azure subscription and service limits, quotas, and constraints](../../azure-subscription-service-limits.md).</span></span>
- <span data-ttu-id="35ce3-135">Náklady na úložiště se počítá samostatně na základě použitých stránek v účtu úložiště hello.</span><span class="sxs-lookup"><span data-stu-id="35ce3-135">Storage costs are calculated separately based on used pages in hello storage account.</span></span> <span data-ttu-id="35ce3-136">Podrobnosti najdete [Azure Storage – ceny](https://azure.microsoft.com/pricing/details/storage/).</span><span class="sxs-lookup"><span data-stu-id="35ce3-136">For details, [Azure Storage Pricing](https://azure.microsoft.com/pricing/details/storage/).</span></span>
- <span data-ttu-id="35ce3-137">Další informace o [Azure výpočetní jednotky (ACU)](acu.md) můžete porovnat výpočetní výkon v Azure SKU.</span><span class="sxs-lookup"><span data-stu-id="35ce3-137">Learn more about how [Azure compute units (ACU)](acu.md) can help you compare compute performance across Azure SKUs.</span></span>



## <a name="rest-api"></a><span data-ttu-id="35ce3-138">Rozhraní REST API</span><span class="sxs-lookup"><span data-stu-id="35ce3-138">Rest API</span></span>

<span data-ttu-id="35ce3-139">Informace o používání tooquery hello REST API pro velikosti virtuálních počítačů najdete v tématu hello následující:</span><span class="sxs-lookup"><span data-stu-id="35ce3-139">For information on using hello REST API tooquery for VM sizes, see hello following:</span></span>

- [<span data-ttu-id="35ce3-140">Seznam dostupných velikostí virtuálních počítačů pro změnu velikosti</span><span class="sxs-lookup"><span data-stu-id="35ce3-140">List available virtual machine sizes for resizing</span></span>](https://docs.microsoft.com/rest/api/compute/virtualmachines/virtualmachines-list-sizes-for-resizing)
- [<span data-ttu-id="35ce3-141">Seznam dostupných velikostí virtuálních počítačů pro předplatné</span><span class="sxs-lookup"><span data-stu-id="35ce3-141">List available virtual machine sizes for a subscription</span></span>](https://docs.microsoft.com/rest/api/compute/virtualmachines/virtualmachines-list-sizes-region)
- [<span data-ttu-id="35ce3-142">Seznam dostupných velikostí virtuálních počítačů v nastavení dostupnosti</span><span class="sxs-lookup"><span data-stu-id="35ce3-142">List available virtual machine sizes in an availability set</span></span>](
https://docs.microsoft.com/rest/api/compute/virtualmachines/virtualmachines-list-sizes-availability-set)

## <a name="acu"></a><span data-ttu-id="35ce3-143">ACU</span><span class="sxs-lookup"><span data-stu-id="35ce3-143">ACU</span></span>

<span data-ttu-id="35ce3-144">Další informace o [Azure výpočetní jednotky (ACU)](acu.md) můžete porovnat výpočetní výkon v Azure SKU.</span><span class="sxs-lookup"><span data-stu-id="35ce3-144">Learn more about how [Azure compute units (ACU)](acu.md) can help you compare compute performance across Azure SKUs.</span></span>

## <a name="next-steps"></a><span data-ttu-id="35ce3-145">Další kroky</span><span class="sxs-lookup"><span data-stu-id="35ce3-145">Next steps</span></span>

<span data-ttu-id="35ce3-146">Další informace o hello různé velikosti virtuálních počítačů, které jsou k dispozici:</span><span class="sxs-lookup"><span data-stu-id="35ce3-146">Learn more about hello different VM sizes that are available:</span></span>
- [<span data-ttu-id="35ce3-147">Obecné účely</span><span class="sxs-lookup"><span data-stu-id="35ce3-147">General purpose</span></span>](sizes-general.md)
- [<span data-ttu-id="35ce3-148">Optimalizované z hlediska výpočetních služeb</span><span class="sxs-lookup"><span data-stu-id="35ce3-148">Compute optimized</span></span>](sizes-compute.md)
- [<span data-ttu-id="35ce3-149">Optimalizované z hlediska paměti</span><span class="sxs-lookup"><span data-stu-id="35ce3-149">Memory optimized</span></span>](../virtual-machines-windows-sizes-memory.md)
- [<span data-ttu-id="35ce3-150">Optimalizované z hlediska úložiště</span><span class="sxs-lookup"><span data-stu-id="35ce3-150">Storage optimized</span></span>](../virtual-machines-windows-sizes-storage.md)
- [<span data-ttu-id="35ce3-151">Optimalizované z hlediska GPU</span><span class="sxs-lookup"><span data-stu-id="35ce3-151">GPU optimized</span></span>](sizes-gpu.md)
- [<span data-ttu-id="35ce3-152">Vysokovýkonné výpočetní prostředí</span><span class="sxs-lookup"><span data-stu-id="35ce3-152">High performance compute</span></span>](sizes-hpc.md)



