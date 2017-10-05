---
title: "Spravovat disky systému Azure pomocí rozhraní příkazového řádku Azure | Microsoft Docs"
description: "Kurz – spravovat disky systému Azure pomocí Azure CLI"
services: virtual-machines-linux
documentationcenter: virtual-machines
author: neilpeterson
manager: timlt
editor: tysonn
tags: azure-service-management
ms.assetid: 
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 05/02/2017
ms.author: nepeters
ms.custom: mvc
ms.openlocfilehash: d77dd2b44dca8cee6fa2e93e79cda76c80ccfe1a
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="manage-azure-disks-with-the-azure-cli"></a><span data-ttu-id="fa0f5-103">Spravovat disky systému Azure pomocí Azure CLI</span><span class="sxs-lookup"><span data-stu-id="fa0f5-103">Manage Azure disks with the Azure CLI</span></span>

<span data-ttu-id="fa0f5-104">Virtuální počítače Azure pomocí disků ukládat virtuální počítače operačního systému, aplikace a data.</span><span class="sxs-lookup"><span data-stu-id="fa0f5-104">Azure virtual machines use disks to store the VMs operating system, applications, and data.</span></span> <span data-ttu-id="fa0f5-105">Při vytváření virtuálního počítače je důležité, abyste zvolili velikost disku a konfigurace, které jsou vhodné pro očekávané úlohy.</span><span class="sxs-lookup"><span data-stu-id="fa0f5-105">When creating a VM it is important to choose a disk size and configuration appropriate to the expected workload.</span></span> <span data-ttu-id="fa0f5-106">Tento kurz se zaměřuje na nasazení a správě disky virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="fa0f5-106">This tutorial covers deploying and managing VM disks.</span></span> <span data-ttu-id="fa0f5-107">Informace o:</span><span class="sxs-lookup"><span data-stu-id="fa0f5-107">You learn about:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="fa0f5-108">Operační systém a dočasný disky</span><span class="sxs-lookup"><span data-stu-id="fa0f5-108">OS disks and temporary disks</span></span>
> * <span data-ttu-id="fa0f5-109">Datové disky</span><span class="sxs-lookup"><span data-stu-id="fa0f5-109">Data disks</span></span>
> * <span data-ttu-id="fa0f5-110">Standard a Premium disky</span><span class="sxs-lookup"><span data-stu-id="fa0f5-110">Standard and Premium disks</span></span>
> * <span data-ttu-id="fa0f5-111">Výkon disku</span><span class="sxs-lookup"><span data-stu-id="fa0f5-111">Disk performance</span></span>
> * <span data-ttu-id="fa0f5-112">Připojení a příprava datových disků</span><span class="sxs-lookup"><span data-stu-id="fa0f5-112">Attaching and preparing data disks</span></span>
> * <span data-ttu-id="fa0f5-113">Změna velikosti disků</span><span class="sxs-lookup"><span data-stu-id="fa0f5-113">Resizing disks</span></span>
> * <span data-ttu-id="fa0f5-114">Snímky disku</span><span class="sxs-lookup"><span data-stu-id="fa0f5-114">Disk snapshots</span></span>


[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

<span data-ttu-id="fa0f5-115">Pokud si zvolíte instalaci a použití rozhraní příkazového řádku místně, tento kurz vyžaduje, že používáte Azure CLI verze verze 2.0.4 nebo novější.</span><span class="sxs-lookup"><span data-stu-id="fa0f5-115">If you choose to install and use the CLI locally, this tutorial requires that you are running the Azure CLI version 2.0.4 or later.</span></span> <span data-ttu-id="fa0f5-116">Verzi zjistíte spuštěním příkazu `az --version`.</span><span class="sxs-lookup"><span data-stu-id="fa0f5-116">Run `az --version` to find the version.</span></span> <span data-ttu-id="fa0f5-117">Pokud potřebujete instalaci nebo upgrade, přečtěte si téma [Instalace Azure CLI 2.0]( /cli/azure/install-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="fa0f5-117">If you need to install or upgrade, see [Install Azure CLI 2.0]( /cli/azure/install-azure-cli).</span></span> 

## <a name="default-azure-disks"></a><span data-ttu-id="fa0f5-118">Výchozí disky systému Azure</span><span class="sxs-lookup"><span data-stu-id="fa0f5-118">Default Azure disks</span></span>

<span data-ttu-id="fa0f5-119">Vytvoření virtuálního počítače Azure má dva disky jsou automaticky připojena k virtuálnímu počítači.</span><span class="sxs-lookup"><span data-stu-id="fa0f5-119">When an Azure virtual machine is created, two disks are automatically attached to the virtual machine.</span></span> 

<span data-ttu-id="fa0f5-120">**Disk s operačním systémem** -provoz systémových disků může mít velikost až 1 terabajt a hostuje virtuální počítače operačního systému.</span><span class="sxs-lookup"><span data-stu-id="fa0f5-120">**Operating system disk** - Operating system disks can be sized up to 1 terabyte, and hosts the VMs operating system.</span></span> <span data-ttu-id="fa0f5-121">Disk operačního systému je označeno */dev/sda* ve výchozím nastavení.</span><span class="sxs-lookup"><span data-stu-id="fa0f5-121">The OS disk is labeled */dev/sda* by default.</span></span> <span data-ttu-id="fa0f5-122">Disk ukládání do mezipaměti konfigurace disku operačního systému je optimalizovaná pro výkon operačního systému.</span><span class="sxs-lookup"><span data-stu-id="fa0f5-122">The disk caching configuration of the OS disk is optimized for OS performance.</span></span> <span data-ttu-id="fa0f5-123">Z důvodu tuto konfiguraci, disk operačního systému **neměli** hostitelem aplikace nebo data.</span><span class="sxs-lookup"><span data-stu-id="fa0f5-123">Because of this configuration, the OS disk **should not** host applications or data.</span></span> <span data-ttu-id="fa0f5-124">Pro aplikace a data použijte datových disků, které jsou podrobně popsané dál v tomto článku.</span><span class="sxs-lookup"><span data-stu-id="fa0f5-124">For applications and data, use data disks, which are detailed later in this article.</span></span> 

<span data-ttu-id="fa0f5-125">**Dočasným diskovým** -dočasné disky používají jednotkou SSD, který je umístěný na stejném hostiteli Azure jako virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="fa0f5-125">**Temporary disk** - Temporary disks use a solid-state drive that is located on the same Azure host as the VM.</span></span> <span data-ttu-id="fa0f5-126">Dočasné disky jsou vysoce původce a mohou být použity pro operací, jako je dočasná data zpracování.</span><span class="sxs-lookup"><span data-stu-id="fa0f5-126">Temp disks are highly performant and may be used for operations such as temporary data processing.</span></span> <span data-ttu-id="fa0f5-127">Pokud se virtuální počítač přesune do nového hostitele, je odebrat všechna data uložená na dočasném disku.</span><span class="sxs-lookup"><span data-stu-id="fa0f5-127">However, if the VM is moved to a new host, any data stored on a temporary disk is removed.</span></span> <span data-ttu-id="fa0f5-128">Velikost dočasné disku je určen podle velikosti virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="fa0f5-128">The size of the temporary disk is determined by the VM size.</span></span> <span data-ttu-id="fa0f5-129">Dočasné disky jsou označeny */dev/sdb* a mít přípojný bod systému */mnt*.</span><span class="sxs-lookup"><span data-stu-id="fa0f5-129">Temporary disks are labeled */dev/sdb* and have a mountpoint of */mnt*.</span></span>

### <a name="temporary-disk-sizes"></a><span data-ttu-id="fa0f5-130">Velikosti dočasné disků</span><span class="sxs-lookup"><span data-stu-id="fa0f5-130">Temporary disk sizes</span></span>

| <span data-ttu-id="fa0f5-131">Typ</span><span class="sxs-lookup"><span data-stu-id="fa0f5-131">Type</span></span> | <span data-ttu-id="fa0f5-132">Velikost virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="fa0f5-132">VM Size</span></span> | <span data-ttu-id="fa0f5-133">Maximální velikost dočasného disk (GB)</span><span class="sxs-lookup"><span data-stu-id="fa0f5-133">Max temp disk size (GB)</span></span> |
|----|----|----|
| [<span data-ttu-id="fa0f5-134">Obecné účely</span><span class="sxs-lookup"><span data-stu-id="fa0f5-134">General purpose</span></span>](sizes-general.md) | <span data-ttu-id="fa0f5-135">A a řady D</span><span class="sxs-lookup"><span data-stu-id="fa0f5-135">A and D series</span></span> | <span data-ttu-id="fa0f5-136">800</span><span class="sxs-lookup"><span data-stu-id="fa0f5-136">800</span></span> |
| [<span data-ttu-id="fa0f5-137">Optimalizované z hlediska výpočetních služeb</span><span class="sxs-lookup"><span data-stu-id="fa0f5-137">Compute optimized</span></span>](sizes-compute.md) | <span data-ttu-id="fa0f5-138">Řady F</span><span class="sxs-lookup"><span data-stu-id="fa0f5-138">F series</span></span> | <span data-ttu-id="fa0f5-139">800</span><span class="sxs-lookup"><span data-stu-id="fa0f5-139">800</span></span> |
| [<span data-ttu-id="fa0f5-140">Optimalizované z hlediska paměti</span><span class="sxs-lookup"><span data-stu-id="fa0f5-140">Memory optimized</span></span>](../virtual-machines-windows-sizes-memory.md) | <span data-ttu-id="fa0f5-141">Série D a G</span><span class="sxs-lookup"><span data-stu-id="fa0f5-141">D and G series</span></span> | <span data-ttu-id="fa0f5-142">6144</span><span class="sxs-lookup"><span data-stu-id="fa0f5-142">6144</span></span> |
| [<span data-ttu-id="fa0f5-143">Optimalizované z hlediska úložiště</span><span class="sxs-lookup"><span data-stu-id="fa0f5-143">Storage optimized</span></span>](../virtual-machines-windows-sizes-storage.md) | <span data-ttu-id="fa0f5-144">Řada L</span><span class="sxs-lookup"><span data-stu-id="fa0f5-144">L series</span></span> | <span data-ttu-id="fa0f5-145">5630</span><span class="sxs-lookup"><span data-stu-id="fa0f5-145">5630</span></span> |
| [<span data-ttu-id="fa0f5-146">GPU</span><span class="sxs-lookup"><span data-stu-id="fa0f5-146">GPU</span></span>](sizes-gpu.md) | <span data-ttu-id="fa0f5-147">N řady</span><span class="sxs-lookup"><span data-stu-id="fa0f5-147">N series</span></span> | <span data-ttu-id="fa0f5-148">1440</span><span class="sxs-lookup"><span data-stu-id="fa0f5-148">1440</span></span> |
| [<span data-ttu-id="fa0f5-149">Vysoký výkon</span><span class="sxs-lookup"><span data-stu-id="fa0f5-149">High performance</span></span>](sizes-hpc.md) | <span data-ttu-id="fa0f5-150">A a řady H</span><span class="sxs-lookup"><span data-stu-id="fa0f5-150">A and H series</span></span> | <span data-ttu-id="fa0f5-151">2000</span><span class="sxs-lookup"><span data-stu-id="fa0f5-151">2000</span></span> |

## <a name="azure-data-disks"></a><span data-ttu-id="fa0f5-152">Azure datových disků</span><span class="sxs-lookup"><span data-stu-id="fa0f5-152">Azure data disks</span></span>

<span data-ttu-id="fa0f5-153">Pro instalaci aplikací a ukládání dat přidáním dalších datových disků.</span><span class="sxs-lookup"><span data-stu-id="fa0f5-153">Additional data disks can be added for installing applications and storing data.</span></span> <span data-ttu-id="fa0f5-154">Datové disky by měl použít v každé situaci, kde je žádoucí, odolné a dobře reagovaly datové úložiště.</span><span class="sxs-lookup"><span data-stu-id="fa0f5-154">Data disks should be used in any situation where durable and responsive data storage is desired.</span></span> <span data-ttu-id="fa0f5-155">Každý datový disk má maximální kapacita 1 terabajt.</span><span class="sxs-lookup"><span data-stu-id="fa0f5-155">Each data disk has a maximum capacity of 1 terabyte.</span></span> <span data-ttu-id="fa0f5-156">Velikost virtuálního počítače určuje, kolik datových disků lze připojit k virtuálnímu počítači.</span><span class="sxs-lookup"><span data-stu-id="fa0f5-156">The size of the virtual machine determines how many data disks can be attached to a VM.</span></span> <span data-ttu-id="fa0f5-157">Pro každý základní virtuální počítač se dá připojit dvěma datovými disky.</span><span class="sxs-lookup"><span data-stu-id="fa0f5-157">For each VM core, two data disks can be attached.</span></span> 

### <a name="max-data-disks-per-vm"></a><span data-ttu-id="fa0f5-158">Maximální počet datových disků na virtuální počítač</span><span class="sxs-lookup"><span data-stu-id="fa0f5-158">Max data disks per VM</span></span>

| <span data-ttu-id="fa0f5-159">Typ</span><span class="sxs-lookup"><span data-stu-id="fa0f5-159">Type</span></span> | <span data-ttu-id="fa0f5-160">Velikost virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="fa0f5-160">VM Size</span></span> | <span data-ttu-id="fa0f5-161">Maximální počet datových disků na virtuální počítač</span><span class="sxs-lookup"><span data-stu-id="fa0f5-161">Max data disks per VM</span></span> |
|----|----|----|
| [<span data-ttu-id="fa0f5-162">Obecné účely</span><span class="sxs-lookup"><span data-stu-id="fa0f5-162">General purpose</span></span>](sizes-general.md) | <span data-ttu-id="fa0f5-163">A a řady D</span><span class="sxs-lookup"><span data-stu-id="fa0f5-163">A and D series</span></span> | <span data-ttu-id="fa0f5-164">32</span><span class="sxs-lookup"><span data-stu-id="fa0f5-164">32</span></span> |
| [<span data-ttu-id="fa0f5-165">Optimalizované z hlediska výpočetních služeb</span><span class="sxs-lookup"><span data-stu-id="fa0f5-165">Compute optimized</span></span>](sizes-compute.md) | <span data-ttu-id="fa0f5-166">Řady F</span><span class="sxs-lookup"><span data-stu-id="fa0f5-166">F series</span></span> | <span data-ttu-id="fa0f5-167">32</span><span class="sxs-lookup"><span data-stu-id="fa0f5-167">32</span></span> |
| [<span data-ttu-id="fa0f5-168">Optimalizované z hlediska paměti</span><span class="sxs-lookup"><span data-stu-id="fa0f5-168">Memory optimized</span></span>](../virtual-machines-windows-sizes-memory.md) | <span data-ttu-id="fa0f5-169">Série D a G</span><span class="sxs-lookup"><span data-stu-id="fa0f5-169">D and G series</span></span> | <span data-ttu-id="fa0f5-170">64</span><span class="sxs-lookup"><span data-stu-id="fa0f5-170">64</span></span> |
| [<span data-ttu-id="fa0f5-171">Optimalizované z hlediska úložiště</span><span class="sxs-lookup"><span data-stu-id="fa0f5-171">Storage optimized</span></span>](../virtual-machines-windows-sizes-storage.md) | <span data-ttu-id="fa0f5-172">Řada L</span><span class="sxs-lookup"><span data-stu-id="fa0f5-172">L series</span></span> | <span data-ttu-id="fa0f5-173">64</span><span class="sxs-lookup"><span data-stu-id="fa0f5-173">64</span></span> |
| [<span data-ttu-id="fa0f5-174">GPU</span><span class="sxs-lookup"><span data-stu-id="fa0f5-174">GPU</span></span>](sizes-gpu.md) | <span data-ttu-id="fa0f5-175">N řady</span><span class="sxs-lookup"><span data-stu-id="fa0f5-175">N series</span></span> | <span data-ttu-id="fa0f5-176">48</span><span class="sxs-lookup"><span data-stu-id="fa0f5-176">48</span></span> |
| [<span data-ttu-id="fa0f5-177">Vysoký výkon</span><span class="sxs-lookup"><span data-stu-id="fa0f5-177">High performance</span></span>](sizes-hpc.md) | <span data-ttu-id="fa0f5-178">A a řady H</span><span class="sxs-lookup"><span data-stu-id="fa0f5-178">A and H series</span></span> | <span data-ttu-id="fa0f5-179">32</span><span class="sxs-lookup"><span data-stu-id="fa0f5-179">32</span></span> |

## <a name="vm-disk-types"></a><span data-ttu-id="fa0f5-180">Typy disků virtuálních počítačů</span><span class="sxs-lookup"><span data-stu-id="fa0f5-180">VM disk types</span></span>

<span data-ttu-id="fa0f5-181">Azure nabízí dva typy disku.</span><span class="sxs-lookup"><span data-stu-id="fa0f5-181">Azure provides two types of disk.</span></span>

### <a name="standard-disk"></a><span data-ttu-id="fa0f5-182">Disků na úrovni Standard</span><span class="sxs-lookup"><span data-stu-id="fa0f5-182">Standard disk</span></span>

<span data-ttu-id="fa0f5-183">Služba Storage úrovně Standard je založená na jednotkách HDD a poskytuje nákladově efektivní úložiště se zachováním výkonu.</span><span class="sxs-lookup"><span data-stu-id="fa0f5-183">Standard Storage is backed by HDDs, and delivers cost-effective storage while still being performant.</span></span> <span data-ttu-id="fa0f5-184">Standardní disky jsou ideální pro finančně efektivní vývoj a testování pracovního vytížení.</span><span class="sxs-lookup"><span data-stu-id="fa0f5-184">Standard disks are ideal for a cost effective dev and test workload.</span></span>

### <a name="premium-disk"></a><span data-ttu-id="fa0f5-185">Premium disku</span><span class="sxs-lookup"><span data-stu-id="fa0f5-185">Premium disk</span></span>

<span data-ttu-id="fa0f5-186">Pro prémiové disky jsou zajišťované založená na SSD vysoce výkonné, nízkou latencí disku.</span><span class="sxs-lookup"><span data-stu-id="fa0f5-186">Premium disks are backed by SSD-based high-performance, low-latency disk.</span></span> <span data-ttu-id="fa0f5-187">Ideální pro virtuální počítače se systémem produkční zatížení.</span><span class="sxs-lookup"><span data-stu-id="fa0f5-187">Perfect for VMs running production workload.</span></span> <span data-ttu-id="fa0f5-188">Premium Storage podporuje DS-series, DSv2-series, GS-series a virtuálních počítačů služby FS-series.</span><span class="sxs-lookup"><span data-stu-id="fa0f5-188">Premium Storage supports DS-series, DSv2-series, GS-series, and FS-series VMs.</span></span> <span data-ttu-id="fa0f5-189">Pro prémiové disky se musí uvést ve třech typech (P10 P20, P30), velikost disku určuje typ disku.</span><span class="sxs-lookup"><span data-stu-id="fa0f5-189">Premium disks come in three types (P10, P20, P30), the size of the disk determines the disk type.</span></span> <span data-ttu-id="fa0f5-190">Když vyberete, je velikost disku hodnota zaokrouhlený nahoru na další typ.</span><span class="sxs-lookup"><span data-stu-id="fa0f5-190">When selecting, a disk size the value is rounded up to the next type.</span></span> <span data-ttu-id="fa0f5-191">Například pokud je velikost disku je menší než 128 GB, typ disku je P10.</span><span class="sxs-lookup"><span data-stu-id="fa0f5-191">For example, if the disk size is less than 128 GB, the disk type is P10.</span></span> <span data-ttu-id="fa0f5-192">Pokud je velikost disku je 129 GB až 512 GB, velikost je P20.</span><span class="sxs-lookup"><span data-stu-id="fa0f5-192">If the disk size is between 129 GB and 512 GB, the size is a P20.</span></span> <span data-ttu-id="fa0f5-193">Nic více než 512 GB, velikost je P30.</span><span class="sxs-lookup"><span data-stu-id="fa0f5-193">Anything over 512 GB, the size is a P30.</span></span>

### <a name="premium-disk-performance"></a><span data-ttu-id="fa0f5-194">Výkon disku Premium</span><span class="sxs-lookup"><span data-stu-id="fa0f5-194">Premium disk performance</span></span>

|<span data-ttu-id="fa0f5-195">Typ disku úložiště Premium</span><span class="sxs-lookup"><span data-stu-id="fa0f5-195">Premium storage disk type</span></span> | <span data-ttu-id="fa0f5-196">P10</span><span class="sxs-lookup"><span data-stu-id="fa0f5-196">P10</span></span> | <span data-ttu-id="fa0f5-197">P20</span><span class="sxs-lookup"><span data-stu-id="fa0f5-197">P20</span></span> | <span data-ttu-id="fa0f5-198">P30</span><span class="sxs-lookup"><span data-stu-id="fa0f5-198">P30</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="fa0f5-199">Velikost disku (round nahoru)</span><span class="sxs-lookup"><span data-stu-id="fa0f5-199">Disk size (round up)</span></span> | <span data-ttu-id="fa0f5-200">128 GB</span><span class="sxs-lookup"><span data-stu-id="fa0f5-200">128 GB</span></span> | <span data-ttu-id="fa0f5-201">512 GB</span><span class="sxs-lookup"><span data-stu-id="fa0f5-201">512 GB</span></span> | <span data-ttu-id="fa0f5-202">1024 GB (1 TB)</span><span class="sxs-lookup"><span data-stu-id="fa0f5-202">1,024 GB (1 TB)</span></span> |
| <span data-ttu-id="fa0f5-203">Maximum vstupně-výstupních operací za sekundu (IOPS) na disk</span><span class="sxs-lookup"><span data-stu-id="fa0f5-203">Max IOPS per disk</span></span> | <span data-ttu-id="fa0f5-204">500</span><span class="sxs-lookup"><span data-stu-id="fa0f5-204">500</span></span> | <span data-ttu-id="fa0f5-205">2,300</span><span class="sxs-lookup"><span data-stu-id="fa0f5-205">2,300</span></span> | <span data-ttu-id="fa0f5-206">5,000</span><span class="sxs-lookup"><span data-stu-id="fa0f5-206">5,000</span></span> |
<span data-ttu-id="fa0f5-207">Propustnost / disk</span><span class="sxs-lookup"><span data-stu-id="fa0f5-207">Throughput per disk</span></span> | <span data-ttu-id="fa0f5-208">100 MB/s</span><span class="sxs-lookup"><span data-stu-id="fa0f5-208">100 MB/s</span></span> | <span data-ttu-id="fa0f5-209">150 MB/s</span><span class="sxs-lookup"><span data-stu-id="fa0f5-209">150 MB/s</span></span> | <span data-ttu-id="fa0f5-210">200 MB/s</span><span class="sxs-lookup"><span data-stu-id="fa0f5-210">200 MB/s</span></span> |

<span data-ttu-id="fa0f5-211">Při výše uvedené tabulce jsou uvedeny maximální IOPS na disku, vyšší úroveň výkonu můžete dosáhnout prokládání více datových disků.</span><span class="sxs-lookup"><span data-stu-id="fa0f5-211">While the above table identifies max IOPS per disk, a higher level of performance can be achieved by striping multiple data disks.</span></span> <span data-ttu-id="fa0f5-212">Pro instanci virtuálního počítače Standard_GS5 můžete dosáhnout maximálně 80 000 IOPS.</span><span class="sxs-lookup"><span data-stu-id="fa0f5-212">For instance, a Standard_GS5 VM can achieve a maximum of 80,000 IOPS.</span></span> <span data-ttu-id="fa0f5-213">Podrobné informace o maximální IOPS na virtuálních počítačů najdete v tématu [velikosti virtuálního počítače s Linuxem](sizes.md).</span><span class="sxs-lookup"><span data-stu-id="fa0f5-213">For detailed information on max IOPS per VM, see [Linux VM sizes](sizes.md).</span></span>

## <a name="create-and-attach-disks"></a><span data-ttu-id="fa0f5-214">Vytvořte a připojte disky</span><span class="sxs-lookup"><span data-stu-id="fa0f5-214">Create and attach disks</span></span>

<span data-ttu-id="fa0f5-215">Datové disky můžete vytvořit a v okamžiku vytvoření virtuálního počítače nebo na existující virtuální počítač připojen.</span><span class="sxs-lookup"><span data-stu-id="fa0f5-215">Data disks can be created and attached at VM creation time or to an existing VM.</span></span>

### <a name="attach-disk-at-vm-creation"></a><span data-ttu-id="fa0f5-216">Připojit disk při vytváření virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="fa0f5-216">Attach disk at VM creation</span></span>

<span data-ttu-id="fa0f5-217">Vytvořte skupinu prostředků pomocí příkazu [az group create](https://docs.microsoft.com/cli/azure/group#create).</span><span class="sxs-lookup"><span data-stu-id="fa0f5-217">Create a resource group with the [az group create](https://docs.microsoft.com/cli/azure/group#create) command.</span></span> 

```azurecli-interactive 
az group create --name myResourceGroupDisk --location eastus
```

<span data-ttu-id="fa0f5-218">Vytvoření virtuálního počítače pomocí [vytvořit virtuální počítač az]( /cli/azure/vm#create) příkaz.</span><span class="sxs-lookup"><span data-stu-id="fa0f5-218">Create a VM using the [az vm create]( /cli/azure/vm#create) command.</span></span> <span data-ttu-id="fa0f5-219">`--datadisk-sizes-gb` Argument slouží k určení, že další disk vytvořit a připojit k virtuálnímu počítači.</span><span class="sxs-lookup"><span data-stu-id="fa0f5-219">The `--datadisk-sizes-gb` argument is used to specify that an additional disk should be created and attached to the virtual machine.</span></span> <span data-ttu-id="fa0f5-220">Pokud chcete vytvořit a připojit více než jeden disk, použijte mezerami oddělený seznam hodnot velikosti disku.</span><span class="sxs-lookup"><span data-stu-id="fa0f5-220">To create and attach more than one disk, use a space-delimited list of disk size values.</span></span> <span data-ttu-id="fa0f5-221">V následujícím příkladu je virtuální počítač vytvořený s dvěma datovými disky, oba 128 GB.</span><span class="sxs-lookup"><span data-stu-id="fa0f5-221">In the following example, a VM is created with two data disks, both 128 GB.</span></span> <span data-ttu-id="fa0f5-222">Vzhledem k velikosti disků jsou 128 GB, jsou obě tyto disky nakonfigurované jako P10s, které poskytují maximální 500 IOPS na disku.</span><span class="sxs-lookup"><span data-stu-id="fa0f5-222">Because the disk sizes are 128 GB, these disks are both configured as P10s, which provide maximum 500 IOPS per disk.</span></span>

```azurecli-interactive 
az vm create \
  --resource-group myResourceGroupDisk \
  --name myVM \
  --image UbuntuLTS \
  --size Standard_DS2_v2 \
  --data-disk-sizes-gb 128 128 \
  --generate-ssh-keys
```

### <a name="attach-disk-to-existing-vm"></a><span data-ttu-id="fa0f5-223">Připojit disk k existující virtuální počítač</span><span class="sxs-lookup"><span data-stu-id="fa0f5-223">Attach disk to existing VM</span></span>

<span data-ttu-id="fa0f5-224">Chcete-li vytvořit a připojit nový disk k existující virtuální počítač, použijte [připojit disk virtuálního počítače az](/cli/azure/vm/disk#attach) příkaz.</span><span class="sxs-lookup"><span data-stu-id="fa0f5-224">To create and attach a new disk to an existing virtual machine, use the [az vm disk attach](/cli/azure/vm/disk#attach) command.</span></span> <span data-ttu-id="fa0f5-225">Následující příklad vytvoří disk premium, 128 GB, velikost a připojí k virtuální počítač vytvořený v předchozím kroku.</span><span class="sxs-lookup"><span data-stu-id="fa0f5-225">The following example creates a premium disk, 128 gigabytes in size, and attaches it to the VM created in the last step.</span></span>

```azurecli-interactive 
az vm disk attach --vm-name myVM --resource-group myResourceGroupDisk --disk myDataDisk --size-gb 128 --sku Premium_LRS --new 
```

## <a name="prepare-data-disks"></a><span data-ttu-id="fa0f5-226">Příprava datových disků</span><span class="sxs-lookup"><span data-stu-id="fa0f5-226">Prepare data disks</span></span>

<span data-ttu-id="fa0f5-227">Jakmile byla přiřazena disk k virtuálnímu počítači, musí se nakonfigurovat na použití disku operačního systému.</span><span class="sxs-lookup"><span data-stu-id="fa0f5-227">Once a disk has been attached to the virtual machine, the operating system needs to be configured to use the disk.</span></span> <span data-ttu-id="fa0f5-228">Následující příklad ukazuje, jak ručně nakonfigurovat disk.</span><span class="sxs-lookup"><span data-stu-id="fa0f5-228">The following example shows how to manually configure a disk.</span></span> <span data-ttu-id="fa0f5-229">Tento proces je také možné automatizovat pomocí inicializací cloudu, kterému se věnujeme v [novější kurzu](./tutorial-automate-vm-deployment.md).</span><span class="sxs-lookup"><span data-stu-id="fa0f5-229">This process can also be automated using cloud-init, which is covered in a [later tutorial](./tutorial-automate-vm-deployment.md).</span></span>

### <a name="manual-configuration"></a><span data-ttu-id="fa0f5-230">Ruční konfigurace</span><span class="sxs-lookup"><span data-stu-id="fa0f5-230">Manual configuration</span></span>

<span data-ttu-id="fa0f5-231">Vytvořte připojení SSH k virtuálnímu počítači.</span><span class="sxs-lookup"><span data-stu-id="fa0f5-231">Create an SSH connection with the virtual machine.</span></span> <span data-ttu-id="fa0f5-232">Nahraďte IP adresu příklad veřejné IP adresy virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="fa0f5-232">Replace the example IP address with the public IP of the virtual machine.</span></span>

```azurecli-interactive 
ssh 52.174.34.95
```

<span data-ttu-id="fa0f5-233">Rozdělit disk s `fdisk`.</span><span class="sxs-lookup"><span data-stu-id="fa0f5-233">Partition the disk with `fdisk`.</span></span>

```bash
(echo n; echo p; echo 1; echo ; echo ; echo w) | sudo fdisk /dev/sdc
```

<span data-ttu-id="fa0f5-234">Zapsat pomocí systému souborů k oddílu `mkfs` příkaz.</span><span class="sxs-lookup"><span data-stu-id="fa0f5-234">Write a file system to the partition by using the `mkfs` command.</span></span>

```bash
sudo mkfs -t ext4 /dev/sdc1
```

<span data-ttu-id="fa0f5-235">Připojte nový disk, aby byl přístupný v operačním systému.</span><span class="sxs-lookup"><span data-stu-id="fa0f5-235">Mount the new disk so that it is accessible in the operating system.</span></span>

```bash
sudo mkdir /datadrive && sudo mount /dev/sdc1 /datadrive
```

<span data-ttu-id="fa0f5-236">Disk je teď přístupná prostřednictvím *datadrive* přípojný bod, který lze ověřit spuštěním `df -h` příkaz.</span><span class="sxs-lookup"><span data-stu-id="fa0f5-236">The disk can now be accessed through the *datadrive* mountpoint, which can be verified by running the `df -h` command.</span></span> 

```bash
df -h
```

<span data-ttu-id="fa0f5-237">Výstup ukazuje nový disk připojit na */datadrive*.</span><span class="sxs-lookup"><span data-stu-id="fa0f5-237">The output shows the new drive mounted on */datadrive*.</span></span>

```bash
Filesystem      Size  Used Avail Use% Mounted on
/dev/sda1        30G  1.4G   28G   5% /
/dev/sdb1       6.8G   16M  6.4G   1% /mnt
/dev/sdc1        50G   52M   47G   1% /datadrive
```

<span data-ttu-id="fa0f5-238">Aby se zajistilo, že jednotka je znovu připojeny po restartování systému, je nutné přidat do */etc/fstab* souboru.</span><span class="sxs-lookup"><span data-stu-id="fa0f5-238">To ensure that the drive is remounted after a reboot, it must be added to the */etc/fstab* file.</span></span> <span data-ttu-id="fa0f5-239">Uděláte to tak získat identifikátor UUID disku spolu s `blkid` nástroj.</span><span class="sxs-lookup"><span data-stu-id="fa0f5-239">To do so, get the UUID of the disk with the `blkid` utility.</span></span>

```bash
sudo -i blkid
```

<span data-ttu-id="fa0f5-240">Výstup zobrazuje identifikátor UUID jednotky `/dev/sdc1` v tomto případě.</span><span class="sxs-lookup"><span data-stu-id="fa0f5-240">The output displays the UUID of the drive, `/dev/sdc1` in this case.</span></span>

```bash
/dev/sdc1: UUID="33333333-3b3b-3c3c-3d3d-3e3e3e3e3e3e" TYPE="ext4"
```

<span data-ttu-id="fa0f5-241">Přidá řádek podobný následujícímu k */etc/fstab* souboru.</span><span class="sxs-lookup"><span data-stu-id="fa0f5-241">Add a line similar to the following to the */etc/fstab* file.</span></span> <span data-ttu-id="fa0f5-242">Také Upozorňujeme, že zápis překážek můžete zakázat pomocí *bariéry = 0*, tato konfigurace může zlepšit výkon disku.</span><span class="sxs-lookup"><span data-stu-id="fa0f5-242">Also note that write barriers can be disabled using *barrier=0*, this configuration can improve disk performance.</span></span> 

```bash
UUID=33333333-3b3b-3c3c-3d3d-3e3e3e3e3e3e   /datadrive  ext4    defaults,nofail,barrier=0   1  2
```

<span data-ttu-id="fa0f5-243">Teď, když byl nakonfigurován na disku, zavřete relace SSH.</span><span class="sxs-lookup"><span data-stu-id="fa0f5-243">Now that the disk has been configured, close the SSH session.</span></span>

```bash
exit
```

## <a name="resize-vm-disk"></a><span data-ttu-id="fa0f5-244">Změna velikosti disku virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="fa0f5-244">Resize VM disk</span></span>

<span data-ttu-id="fa0f5-245">Jakmile nasazen virtuální počítač, disk operačního systému nebo jakýchkoli připojených datových disků je možné zvýšit velikost.</span><span class="sxs-lookup"><span data-stu-id="fa0f5-245">Once a VM has been deployed, the operating system disk or any attached data disks can be increased in size.</span></span> <span data-ttu-id="fa0f5-246">Zvětšení velikosti disku je v případě nutnosti další úložný prostor nebo vyšší úroveň výkonu (P10, P20, P30).</span><span class="sxs-lookup"><span data-stu-id="fa0f5-246">Increasing the size of a disk is beneficial when needing more storage space or a higher level of performance (P10, P20, P30).</span></span> <span data-ttu-id="fa0f5-247">Všimněte si, že se disky není možné snížit velikost.</span><span class="sxs-lookup"><span data-stu-id="fa0f5-247">Note, disks cannot be decreased in size.</span></span>

<span data-ttu-id="fa0f5-248">Před zvýšením velikosti disku, je potřeba Id nebo název disku.</span><span class="sxs-lookup"><span data-stu-id="fa0f5-248">Before increasing disk size, the Id or name of the disk is needed.</span></span> <span data-ttu-id="fa0f5-249">Použití [seznam disků az](/cli/azure/vm/disk#list) příkaz vrátí všechny disky ve skupině prostředků.</span><span class="sxs-lookup"><span data-stu-id="fa0f5-249">Use the [az disk list](/cli/azure/vm/disk#list) command to return all disks in a resource group.</span></span> <span data-ttu-id="fa0f5-250">Poznamenejte si název disku, který chcete přizpůsobit.</span><span class="sxs-lookup"><span data-stu-id="fa0f5-250">Take note of the disk name that you would like to resize.</span></span>

```azurecli-interactive 
az disk list -g myResourceGroupDisk --query '[*].{Name:name,Gb:diskSizeGb,Tier:accountType}' --output table
```

<span data-ttu-id="fa0f5-251">Virtuální počítač musí být také navrácena.</span><span class="sxs-lookup"><span data-stu-id="fa0f5-251">The VM must also be deallocated.</span></span> <span data-ttu-id="fa0f5-252">Použití [az OM deallocate]( /cli/azure/vm#deallocate) příkaz k zastavení a zrušit přidělení virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="fa0f5-252">Use the [az vm deallocate]( /cli/azure/vm#deallocate) command to stop and deallocate the VM.</span></span>

```azurecli-interactive 
az vm deallocate --resource-group myResourceGroupDisk --name myVM
```

<span data-ttu-id="fa0f5-253">Použití [aktualizace disku az](/cli/azure/vm/disk#update) příkaz ke změně velikosti disku.</span><span class="sxs-lookup"><span data-stu-id="fa0f5-253">Use the [az disk update](/cli/azure/vm/disk#update) command to resize the disk.</span></span> <span data-ttu-id="fa0f5-254">Tento příklad změní velikost disku s názvem *myDataDisk* na 1 terabajt.</span><span class="sxs-lookup"><span data-stu-id="fa0f5-254">This example resizes a disk named *myDataDisk* to 1 terabyte.</span></span>

```azurecli-interactive 
az disk update --name myDataDisk --resource-group myResourceGroupDisk --size-gb 1023
```

<span data-ttu-id="fa0f5-255">Po dokončení operace změny velikosti, spusťte virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="fa0f5-255">Once the resize operation has completed, start the VM.</span></span>

```azurecli-interactive 
az vm start --resource-group myResourceGroupDisk --name myVM
```

<span data-ttu-id="fa0f5-256">Pokud jste ke změně velikosti disku operačního systému, je automaticky rozšířena oddílu.</span><span class="sxs-lookup"><span data-stu-id="fa0f5-256">If you’ve resized the operating system disk, the partition is automatically be expanded.</span></span> <span data-ttu-id="fa0f5-257">Pokud jste změnili datový disk, je potřeba rozšířit v operačním systému virtuální počítače žádné aktuální oddíly.</span><span class="sxs-lookup"><span data-stu-id="fa0f5-257">If you have resized a data disk, any current partitions need to be expanded in the VMs operating system.</span></span>

## <a name="snapshot-azure-disks"></a><span data-ttu-id="fa0f5-258">Snímek disky systému Azure</span><span class="sxs-lookup"><span data-stu-id="fa0f5-258">Snapshot Azure disks</span></span>

<span data-ttu-id="fa0f5-259">Pořízení snímku disku vytvoří čtení pouze, v okamžiku kopie disku.</span><span class="sxs-lookup"><span data-stu-id="fa0f5-259">Taking a disk snapshot creates a read only, point-in-time copy of the disk.</span></span> <span data-ttu-id="fa0f5-260">Azure snímky virtuálních počítačů jsou užitečné pro rychle uložení stavu virtuálního počítače, před prováděním změn konfigurace.</span><span class="sxs-lookup"><span data-stu-id="fa0f5-260">Azure VM snapshots are useful for quickly saving the state of a VM before making configuration changes.</span></span> <span data-ttu-id="fa0f5-261">V případě, že změny konfigurace ukázat jako nežádoucí, může být stav virtuálního počítače obnovena pomocí snímku.</span><span class="sxs-lookup"><span data-stu-id="fa0f5-261">In the event the configuration changes prove to be undesired, VM state can be restored using the snapshot.</span></span> <span data-ttu-id="fa0f5-262">Virtuální počítač má více než jeden disk, je převzat snímek každého disku nezávisle na ostatních.</span><span class="sxs-lookup"><span data-stu-id="fa0f5-262">When a VM has more than one disk, a snapshot is taken of each disk independently of the others.</span></span> <span data-ttu-id="fa0f5-263">Za vyjádření zálohování konzistentní s aplikací, vezměte v úvahu před přepnutím snímky disku zastavení virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="fa0f5-263">For taking application consistent backups, consider stopping the VM before taking disk snapshots.</span></span> <span data-ttu-id="fa0f5-264">Můžete taky použít [služby Azure Backup](/azure/backup/), což umožňuje provádět automatizované zálohování spuštěného virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="fa0f5-264">Alternatively, use the [Azure Backup service](/azure/backup/), which enables you to perform automated backups while the VM is running.</span></span>

### <a name="create-snapshot"></a><span data-ttu-id="fa0f5-265">Vytvořit snímek</span><span class="sxs-lookup"><span data-stu-id="fa0f5-265">Create snapshot</span></span>

<span data-ttu-id="fa0f5-266">Před vytvořením snímku disku virtuálního počítače, je potřeba Id nebo název disku.</span><span class="sxs-lookup"><span data-stu-id="fa0f5-266">Before creating a virtual machine disk snapshot, the Id or name of the disk is needed.</span></span> <span data-ttu-id="fa0f5-267">Použití [az virtuálních počítačů zobrazit](https://docs.microsoft.com/en-us/cli/azure/vm#show) příkaz vrátí id disku.</span><span class="sxs-lookup"><span data-stu-id="fa0f5-267">Use the [az vm show](https://docs.microsoft.com/en-us/cli/azure/vm#show) command to return the disk id.</span></span> <span data-ttu-id="fa0f5-268">V tomto příkladu je id disku uložené v proměnné, aby se může použít později.</span><span class="sxs-lookup"><span data-stu-id="fa0f5-268">In this example, the disk id is stored in a variable so that it can be used in a later step.</span></span>

```azurecli-interactive 
osdiskid=$(az vm show -g myResourceGroupDisk -n myVM --query "storageProfile.osDisk.managedDisk.id" -o tsv)
```

<span data-ttu-id="fa0f5-269">Teď, když máte id disku virtuálního počítače, následující příkaz vytvoří snímek disku.</span><span class="sxs-lookup"><span data-stu-id="fa0f5-269">Now that you have the id of the virtual machine disk, the following command creates a snapshot of the disk.</span></span>

```azurcli
az snapshot create -g myResourceGroupDisk --source "$osdiskid" --name osDisk-backup
```

### <a name="create-disk-from-snapshot"></a><span data-ttu-id="fa0f5-270">Vytvoření disku ze snímku</span><span class="sxs-lookup"><span data-stu-id="fa0f5-270">Create disk from snapshot</span></span>

<span data-ttu-id="fa0f5-271">Tento snímek pak může být převedena na disk, který můžete použít k opětovnému vytvoření virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="fa0f5-271">This snapshot can then be converted into a disk, which can be used to recreate the virtual machine.</span></span>

```azurecli-interactive 
az disk create --resource-group myResourceGroupDisk --name mySnapshotDisk --source osDisk-backup
```

### <a name="restore-virtual-machine-from-snapshot"></a><span data-ttu-id="fa0f5-272">Virtuální počítač obnovit ze snímku</span><span class="sxs-lookup"><span data-stu-id="fa0f5-272">Restore virtual machine from snapshot</span></span>

<span data-ttu-id="fa0f5-273">K předvedení obnovení virtuálního počítače, odstraňte existující virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="fa0f5-273">To demonstrate virtual machine recovery, delete the existing virtual machine.</span></span> 

```azurecli-interactive 
az vm delete --resource-group myResourceGroupDisk --name myVM
```

<span data-ttu-id="fa0f5-274">Vytvoření nového virtuálního počítače z disku snímku.</span><span class="sxs-lookup"><span data-stu-id="fa0f5-274">Create a new virtual machine from the snapshot disk.</span></span>

```azurecli-interactive 
az vm create --resource-group myResourceGroupDisk --name myVM --attach-os-disk mySnapshotDisk --os-type linux
```

### <a name="reattach-data-disk"></a><span data-ttu-id="fa0f5-275">Připojte datový disk</span><span class="sxs-lookup"><span data-stu-id="fa0f5-275">Reattach data disk</span></span>

<span data-ttu-id="fa0f5-276">Musí být znovu připojit k virtuálnímu počítači všechny disky data.</span><span class="sxs-lookup"><span data-stu-id="fa0f5-276">All data disks need to be reattached to the virtual machine.</span></span>

<span data-ttu-id="fa0f5-277">Nejprve najít název disku dat pomocí [seznam disků az](https://docs.microsoft.com/cli/azure/disk#list) příkaz.</span><span class="sxs-lookup"><span data-stu-id="fa0f5-277">First find the data disk name using the [az disk list](https://docs.microsoft.com/cli/azure/disk#list) command.</span></span> <span data-ttu-id="fa0f5-278">Tento příklad umístí název disku do proměnné s názvem *datadisk*, která je použita v dalším kroku.</span><span class="sxs-lookup"><span data-stu-id="fa0f5-278">This example places the name of the disk in a variable named *datadisk*, which is used in the next step.</span></span>

```azurecli-interactive 
datadisk=$(az disk list -g myResourceGroupDisk --query "[?contains(name,'myVM')].[name]" -o tsv)
```

<span data-ttu-id="fa0f5-279">Použití [připojit disk virtuálního počítače az](https://docs.microsoft.com/cli/azure/vm/disk#attach) příkazu připojit disk.</span><span class="sxs-lookup"><span data-stu-id="fa0f5-279">Use the [az vm disk attach](https://docs.microsoft.com/cli/azure/vm/disk#attach) command to attach the disk.</span></span>

```azurecli-interactive 
az vm disk attach –g myResourceGroupDisk –-vm-name myVM –-disk $datadisk
```

## <a name="next-steps"></a><span data-ttu-id="fa0f5-280">Další kroky</span><span class="sxs-lookup"><span data-stu-id="fa0f5-280">Next steps</span></span>

<span data-ttu-id="fa0f5-281">V tomto kurzu jste se dozvěděli o tématech disky virtuálních počítačů, jako:</span><span class="sxs-lookup"><span data-stu-id="fa0f5-281">In this tutorial, you learned about VM disks topics such as:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="fa0f5-282">Operační systém a dočasný disky</span><span class="sxs-lookup"><span data-stu-id="fa0f5-282">OS disks and temporary disks</span></span>
> * <span data-ttu-id="fa0f5-283">Datové disky</span><span class="sxs-lookup"><span data-stu-id="fa0f5-283">Data disks</span></span>
> * <span data-ttu-id="fa0f5-284">Standard a Premium disky</span><span class="sxs-lookup"><span data-stu-id="fa0f5-284">Standard and Premium disks</span></span>
> * <span data-ttu-id="fa0f5-285">Výkon disku</span><span class="sxs-lookup"><span data-stu-id="fa0f5-285">Disk performance</span></span>
> * <span data-ttu-id="fa0f5-286">Připojení a příprava datových disků</span><span class="sxs-lookup"><span data-stu-id="fa0f5-286">Attaching and preparing data disks</span></span>
> * <span data-ttu-id="fa0f5-287">Změna velikosti disků</span><span class="sxs-lookup"><span data-stu-id="fa0f5-287">Resizing disks</span></span>
> * <span data-ttu-id="fa0f5-288">Snímky disku</span><span class="sxs-lookup"><span data-stu-id="fa0f5-288">Disk snapshots</span></span>

<span data-ttu-id="fa0f5-289">Přechodu na další informace o automatizaci konfigurace virtuálního počítače v dalším kurzu.</span><span class="sxs-lookup"><span data-stu-id="fa0f5-289">Advance to the next tutorial to learn about automating VM configuration.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="fa0f5-290">Automatizace konfigurace virtuálních počítačů</span><span class="sxs-lookup"><span data-stu-id="fa0f5-290">Automate VM configuration</span></span>](./tutorial-automate-vm-deployment.md)
