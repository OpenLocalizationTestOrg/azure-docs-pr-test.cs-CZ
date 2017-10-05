---
title: "Velikost virtuálního počítače s Windows v Azure | Microsoft Docs"
description: "Obsahuje seznam různých velikostí, které jsou k dispozici pro virtuální počítače s Windows v Azure."
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
ms.openlocfilehash: b3a674137ed3dd47188d4af0bc845104eabc885e
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="sizes-for-windows-virtual-machines-in-azure"></a><span data-ttu-id="2b890-103">Velikosti pro virtuální počítače s Windows v Azure</span><span class="sxs-lookup"><span data-stu-id="2b890-103">Sizes for Windows virtual machines in Azure</span></span>

<span data-ttu-id="2b890-104">Tento článek popisuje dostupné velikosti a možnosti pro virtuální počítače Azure, které můžete použít ke spuštění aplikace pro Windows a úlohy.</span><span class="sxs-lookup"><span data-stu-id="2b890-104">This article describes the available sizes and options for the Azure virtual machines you can use to run your Windows apps and workloads.</span></span> <span data-ttu-id="2b890-105">Je také důležité informace o nasazení znát při plánování použití těchto prostředků.</span><span class="sxs-lookup"><span data-stu-id="2b890-105">It also provides deployment considerations to be aware of when you're planning to use these resources.</span></span>  <span data-ttu-id="2b890-106">Tento článek je také k dispozici pro [virtuální počítače s Linuxem](../linux/sizes.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="2b890-106">This article is also available for [Linux virtual machines](../linux/sizes.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span>


| <span data-ttu-id="2b890-107">Typ</span><span class="sxs-lookup"><span data-stu-id="2b890-107">Type</span></span>                     | <span data-ttu-id="2b890-108">Velikosti</span><span class="sxs-lookup"><span data-stu-id="2b890-108">Sizes</span></span>           |    <span data-ttu-id="2b890-109">Popis</span><span class="sxs-lookup"><span data-stu-id="2b890-109">Description</span></span>       |
|--------------------------|-------------------|------------------------------------------------------------------------------------------------------------------------------------|
| [<span data-ttu-id="2b890-110">Obecné účely</span><span class="sxs-lookup"><span data-stu-id="2b890-110">General purpose</span></span>](sizes-general.md)          | <span data-ttu-id="2b890-111">Dsv3, Dv3, DSv2, Dv2, DS, D, Av2, A0 7</span><span class="sxs-lookup"><span data-stu-id="2b890-111">Dsv3, Dv3, DSv2, Dv2, DS, D, Av2, A0-7</span></span> | <span data-ttu-id="2b890-112">Vyvážený poměr procesorů k paměti.</span><span class="sxs-lookup"><span data-stu-id="2b890-112">Balanced CPU-to-memory ratio.</span></span> <span data-ttu-id="2b890-113">Ideální pro testování a vývoj, malé a střední databáze a webové servery s nízkým a středním provozem.</span><span class="sxs-lookup"><span data-stu-id="2b890-113">Ideal for testing and development, small to medium databases, and low to medium traffic web servers.</span></span> |
| [<span data-ttu-id="2b890-114">Optimalizované z hlediska výpočetních služeb</span><span class="sxs-lookup"><span data-stu-id="2b890-114">Compute optimized</span></span>](sizes-compute.md)        | <span data-ttu-id="2b890-115">Služby FS, F</span><span class="sxs-lookup"><span data-stu-id="2b890-115">Fs, F</span></span>             | <span data-ttu-id="2b890-116">Vysoký poměr procesorů k paměti.</span><span class="sxs-lookup"><span data-stu-id="2b890-116">High CPU-to-memory ratio.</span></span> <span data-ttu-id="2b890-117">Vhodné pro webové servery se středním provozem, síťová zařízení, dávkové procesy a aplikační servery.</span><span class="sxs-lookup"><span data-stu-id="2b890-117">Good for medium traffic web servers, network appliances, batch processes, and application servers.</span></span>        |
| [<span data-ttu-id="2b890-118">Optimalizované z hlediska paměti</span><span class="sxs-lookup"><span data-stu-id="2b890-118">Memory optimized</span></span>](../virtual-machines-windows-sizes-memory.md)         | <span data-ttu-id="2b890-119">Esv3, Ev3, M, GS, G, DSv2, DS, Dv2, D</span><span class="sxs-lookup"><span data-stu-id="2b890-119">Esv3, Ev3, M, GS, G, DSv2, DS, Dv2, D</span></span>   | <span data-ttu-id="2b890-120">Vysoký poměr paměť procesoru.</span><span class="sxs-lookup"><span data-stu-id="2b890-120">High memory-to-CPU ratio.</span></span> <span data-ttu-id="2b890-121">Velmi vhodné pro servery relačních databází, střední a velké mezipaměti a analýzu v paměti.</span><span class="sxs-lookup"><span data-stu-id="2b890-121">Great for relational database servers, medium to large caches, and in-memory analytics.</span></span>                 |
| [<span data-ttu-id="2b890-122">Optimalizované z hlediska úložiště</span><span class="sxs-lookup"><span data-stu-id="2b890-122">Storage optimized</span></span>](../virtual-machines-windows-sizes-storage.md)        | <span data-ttu-id="2b890-123">Ls</span><span class="sxs-lookup"><span data-stu-id="2b890-123">Ls</span></span>                | <span data-ttu-id="2b890-124">Vysoká propustnost disku a V/V.</span><span class="sxs-lookup"><span data-stu-id="2b890-124">High disk throughput and IO.</span></span> <span data-ttu-id="2b890-125">Ideální pro databáze NoSQL, SQL a velké objemy dat.</span><span class="sxs-lookup"><span data-stu-id="2b890-125">Ideal for Big Data, SQL, and NoSQL databases.</span></span>                                                         |
| [<span data-ttu-id="2b890-126">GPU</span><span class="sxs-lookup"><span data-stu-id="2b890-126">GPU</span></span>](sizes-gpu.md)            | <span data-ttu-id="2b890-127">VS, NC</span><span class="sxs-lookup"><span data-stu-id="2b890-127">NV, NC</span></span>            | <span data-ttu-id="2b890-128">Specializované virtuální počítače cílené pro velkou grafické vykreslování a úpravy videa.</span><span class="sxs-lookup"><span data-stu-id="2b890-128">Specialized virtual machines targeted for heavy graphic rendering and video editing.</span></span> <span data-ttu-id="2b890-129">K dispozici jeden nebo více grafickými procesory.</span><span class="sxs-lookup"><span data-stu-id="2b890-129">Available with single or multiple GPUs.</span></span>       |
| [<span data-ttu-id="2b890-130">Vysokovýkonné výpočetní prostředí</span><span class="sxs-lookup"><span data-stu-id="2b890-130">High performance compute</span></span>](sizes-hpc.md) | <span data-ttu-id="2b890-131">H, A8 11</span><span class="sxs-lookup"><span data-stu-id="2b890-131">H, A8-11</span></span>          | <span data-ttu-id="2b890-132">Naše nejrychlejší a procesorově nejvýkonnější virtuální počítače s volitelnými síťovými rozhraními s vysokou propustností (RDMA).</span><span class="sxs-lookup"><span data-stu-id="2b890-132">Our fastest and most powerful CPU virtual machines with optional high-throughput network interfaces (RDMA).</span></span> 

<br> 

- <span data-ttu-id="2b890-133">Informace o cenách různých velikostí najdete v tématu [ceny služeb Virtual Machines](https://azure.microsoft.com/pricing/details/virtual-machines/#Windows).</span><span class="sxs-lookup"><span data-stu-id="2b890-133">For information about pricing of the various sizes, see [Virtual Machines Pricing](https://azure.microsoft.com/pricing/details/virtual-machines/#Windows).</span></span> 
- <span data-ttu-id="2b890-134">Obecná omezení na virtuálních počítačích Azure najdete v tématu [předplatného Azure a omezení služby, kvóty a omezení](../../azure-subscription-service-limits.md).</span><span class="sxs-lookup"><span data-stu-id="2b890-134">To see general limits on Azure VMs, see [Azure subscription and service limits, quotas, and constraints](../../azure-subscription-service-limits.md).</span></span>
- <span data-ttu-id="2b890-135">Náklady na úložiště se počítají samostatně na základě využitých stránek v účtu úložiště.</span><span class="sxs-lookup"><span data-stu-id="2b890-135">Storage costs are calculated separately based on used pages in the storage account.</span></span> <span data-ttu-id="2b890-136">Podrobnosti najdete [Azure Storage – ceny](https://azure.microsoft.com/pricing/details/storage/).</span><span class="sxs-lookup"><span data-stu-id="2b890-136">For details, [Azure Storage Pricing](https://azure.microsoft.com/pricing/details/storage/).</span></span>
- <span data-ttu-id="2b890-137">Další informace o [Azure výpočetní jednotky (ACU)](acu.md) můžete porovnat výpočetní výkon v Azure SKU.</span><span class="sxs-lookup"><span data-stu-id="2b890-137">Learn more about how [Azure compute units (ACU)](acu.md) can help you compare compute performance across Azure SKUs.</span></span>



## <a name="rest-api"></a><span data-ttu-id="2b890-138">Rozhraní REST API</span><span class="sxs-lookup"><span data-stu-id="2b890-138">Rest API</span></span>

<span data-ttu-id="2b890-139">Informace o používání rozhraní API REST k dotazu pro velikosti virtuálních počítačů naleznete v následujících tématech:</span><span class="sxs-lookup"><span data-stu-id="2b890-139">For information on using the REST API to query for VM sizes, see the following:</span></span>

- [<span data-ttu-id="2b890-140">Seznam dostupných velikostí virtuálních počítačů pro změnu velikosti</span><span class="sxs-lookup"><span data-stu-id="2b890-140">List available virtual machine sizes for resizing</span></span>](https://docs.microsoft.com/rest/api/compute/virtualmachines/virtualmachines-list-sizes-for-resizing)
- [<span data-ttu-id="2b890-141">Seznam dostupných velikostí virtuálních počítačů pro předplatné</span><span class="sxs-lookup"><span data-stu-id="2b890-141">List available virtual machine sizes for a subscription</span></span>](https://docs.microsoft.com/rest/api/compute/virtualmachines/virtualmachines-list-sizes-region)
- [<span data-ttu-id="2b890-142">Seznam dostupných velikostí virtuálních počítačů v nastavení dostupnosti</span><span class="sxs-lookup"><span data-stu-id="2b890-142">List available virtual machine sizes in an availability set</span></span>](
https://docs.microsoft.com/rest/api/compute/virtualmachines/virtualmachines-list-sizes-availability-set)

## <a name="acu"></a><span data-ttu-id="2b890-143">ACU</span><span class="sxs-lookup"><span data-stu-id="2b890-143">ACU</span></span>

<span data-ttu-id="2b890-144">Další informace o [Azure výpočetní jednotky (ACU)](acu.md) můžete porovnat výpočetní výkon v Azure SKU.</span><span class="sxs-lookup"><span data-stu-id="2b890-144">Learn more about how [Azure compute units (ACU)](acu.md) can help you compare compute performance across Azure SKUs.</span></span>

## <a name="next-steps"></a><span data-ttu-id="2b890-145">Další kroky</span><span class="sxs-lookup"><span data-stu-id="2b890-145">Next steps</span></span>

<span data-ttu-id="2b890-146">Další informace o různých velikosti virtuálních počítačů, které jsou k dispozici:</span><span class="sxs-lookup"><span data-stu-id="2b890-146">Learn more about the different VM sizes that are available:</span></span>
- [<span data-ttu-id="2b890-147">Obecné účely</span><span class="sxs-lookup"><span data-stu-id="2b890-147">General purpose</span></span>](sizes-general.md)
- [<span data-ttu-id="2b890-148">Optimalizované z hlediska výpočetních služeb</span><span class="sxs-lookup"><span data-stu-id="2b890-148">Compute optimized</span></span>](sizes-compute.md)
- [<span data-ttu-id="2b890-149">Optimalizované z hlediska paměti</span><span class="sxs-lookup"><span data-stu-id="2b890-149">Memory optimized</span></span>](../virtual-machines-windows-sizes-memory.md)
- [<span data-ttu-id="2b890-150">Optimalizované z hlediska úložiště</span><span class="sxs-lookup"><span data-stu-id="2b890-150">Storage optimized</span></span>](../virtual-machines-windows-sizes-storage.md)
- [<span data-ttu-id="2b890-151">Optimalizované z hlediska GPU</span><span class="sxs-lookup"><span data-stu-id="2b890-151">GPU optimized</span></span>](sizes-gpu.md)
- [<span data-ttu-id="2b890-152">Vysokovýkonné výpočetní prostředí</span><span class="sxs-lookup"><span data-stu-id="2b890-152">High performance compute</span></span>](sizes-hpc.md)



