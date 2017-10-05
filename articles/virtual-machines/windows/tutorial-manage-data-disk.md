---
title: "Správa Azure disky pomocí prostředí Azure PowerShell | Microsoft Docs"
description: "Kurz – spravovat Azure disky pomocí prostředí Azure PowerShell"
services: virtual-machines-windows
documentationcenter: virtual-machines
author: neilpeterson
manager: timlt
editor: tysonn
tags: azure-service-management
ms.assetid: 
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 05/02/2017
ms.author: nepeters
ms.custom: mvc
ms.openlocfilehash: 6f1bc9361745adc211f22416a7ba8ac1b8dc614e
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="manage-azure-disks-with-powershell"></a><span data-ttu-id="4fc0e-103">Správa Azure disky pomocí prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="4fc0e-103">Manage Azure disks with PowerShell</span></span>

<span data-ttu-id="4fc0e-104">Virtuální počítače Azure pomocí disků ukládat virtuální počítače operačního systému, aplikace a data.</span><span class="sxs-lookup"><span data-stu-id="4fc0e-104">Azure virtual machines use disks to store the VMs operating system, applications, and data.</span></span> <span data-ttu-id="4fc0e-105">Při vytváření virtuálního počítače je důležité, abyste zvolili velikost disku a konfigurace, které jsou vhodné pro očekávané úlohy.</span><span class="sxs-lookup"><span data-stu-id="4fc0e-105">When creating a VM it is important to choose a disk size and configuration appropriate to the expected workload.</span></span> <span data-ttu-id="4fc0e-106">Tento kurz se zaměřuje na nasazení a správě disky virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="4fc0e-106">This tutorial covers deploying and managing VM disks.</span></span> <span data-ttu-id="4fc0e-107">Informace o:</span><span class="sxs-lookup"><span data-stu-id="4fc0e-107">You learn about:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="4fc0e-108">Operační systém a dočasný disky</span><span class="sxs-lookup"><span data-stu-id="4fc0e-108">OS disks and temporary disks</span></span>
> * <span data-ttu-id="4fc0e-109">Datové disky</span><span class="sxs-lookup"><span data-stu-id="4fc0e-109">Data disks</span></span>
> * <span data-ttu-id="4fc0e-110">Standard a Premium disky</span><span class="sxs-lookup"><span data-stu-id="4fc0e-110">Standard and Premium disks</span></span>
> * <span data-ttu-id="4fc0e-111">Výkon disku</span><span class="sxs-lookup"><span data-stu-id="4fc0e-111">Disk performance</span></span>
> * <span data-ttu-id="4fc0e-112">Připojení a příprava datových disků</span><span class="sxs-lookup"><span data-stu-id="4fc0e-112">Attaching and preparing data disks</span></span>

<span data-ttu-id="4fc0e-113">Tento kurz vyžaduje modul Azure PowerShell verze 3.6 nebo novější.</span><span class="sxs-lookup"><span data-stu-id="4fc0e-113">This tutorial requires the Azure PowerShell module version 3.6 or later.</span></span> <span data-ttu-id="4fc0e-114">Verzi zjistíte spuštěním příkazu ` Get-Module -ListAvailable AzureRM`.</span><span class="sxs-lookup"><span data-stu-id="4fc0e-114">Run ` Get-Module -ListAvailable AzureRM` to find the version.</span></span> <span data-ttu-id="4fc0e-115">Pokud je třeba upgradovat, přečtěte si téma [modul nainstalovat Azure PowerShell](/powershell/azure/install-azurerm-ps).</span><span class="sxs-lookup"><span data-stu-id="4fc0e-115">If you need to upgrade, see [Install Azure PowerShell module](/powershell/azure/install-azurerm-ps).</span></span>

## <a name="default-azure-disks"></a><span data-ttu-id="4fc0e-116">Výchozí disky systému Azure</span><span class="sxs-lookup"><span data-stu-id="4fc0e-116">Default Azure disks</span></span>

<span data-ttu-id="4fc0e-117">Vytvoření virtuálního počítače Azure má dva disky jsou automaticky připojena k virtuálnímu počítači.</span><span class="sxs-lookup"><span data-stu-id="4fc0e-117">When an Azure virtual machine is created, two disks are automatically attached to the virtual machine.</span></span> 

<span data-ttu-id="4fc0e-118">**Disk s operačním systémem** -provoz systémových disků může mít velikost až 1 terabajt a hostuje virtuální počítače operačního systému.</span><span class="sxs-lookup"><span data-stu-id="4fc0e-118">**Operating system disk** - Operating system disks can be sized up to 1 terabyte, and hosts the VMs operating system.</span></span>  <span data-ttu-id="4fc0e-119">Disk operačního systému je přiřazeno písmeno jednotky *c:* ve výchozím nastavení.</span><span class="sxs-lookup"><span data-stu-id="4fc0e-119">The OS disk is assigned a drive letter of *c:* by default.</span></span> <span data-ttu-id="4fc0e-120">Disk ukládání do mezipaměti konfigurace disku operačního systému je optimalizovaná pro výkon operačního systému.</span><span class="sxs-lookup"><span data-stu-id="4fc0e-120">The disk caching configuration of the OS disk is optimized for OS performance.</span></span> <span data-ttu-id="4fc0e-121">Disk operačního systému **neměli** hostitelem aplikace nebo data.</span><span class="sxs-lookup"><span data-stu-id="4fc0e-121">The OS disk **should not** host applications or data.</span></span> <span data-ttu-id="4fc0e-122">Pro aplikace a data použijte datový disk, který je podrobně popsán později v tomto článku.</span><span class="sxs-lookup"><span data-stu-id="4fc0e-122">For applications and data, use a data disk, which is detailed later in this article.</span></span>

<span data-ttu-id="4fc0e-123">**Dočasným diskovým** -dočasné disky používají jednotkou SSD, který je umístěný na stejném hostiteli Azure jako virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="4fc0e-123">**Temporary disk** - Temporary disks use a solid-state drive that is located on the same Azure host as the VM.</span></span> <span data-ttu-id="4fc0e-124">Dočasné disky jsou vysoce původce a mohou být použity pro operací, jako je dočasná data zpracování.</span><span class="sxs-lookup"><span data-stu-id="4fc0e-124">Temp disks are highly performant and may be used for operations such as temporary data processing.</span></span> <span data-ttu-id="4fc0e-125">Pokud se virtuální počítač přesune do nového hostitele, je odebrat všechna data uložená na dočasném disku.</span><span class="sxs-lookup"><span data-stu-id="4fc0e-125">However, if the VM is moved to a new host, any data stored on a temporary disk is removed.</span></span> <span data-ttu-id="4fc0e-126">Velikost dočasné disku je určen podle velikosti virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="4fc0e-126">The size of the temporary disk is determined by the VM size.</span></span> <span data-ttu-id="4fc0e-127">Dočasné disky přiřazené písmeno jednotky *d:* ve výchozím nastavení.</span><span class="sxs-lookup"><span data-stu-id="4fc0e-127">Temporary disks are assigned a drive letter of *d:* by default.</span></span>

### <a name="temporary-disk-sizes"></a><span data-ttu-id="4fc0e-128">Velikosti dočasné disků</span><span class="sxs-lookup"><span data-stu-id="4fc0e-128">Temporary disk sizes</span></span>

| <span data-ttu-id="4fc0e-129">Typ</span><span class="sxs-lookup"><span data-stu-id="4fc0e-129">Type</span></span> | <span data-ttu-id="4fc0e-130">Velikost virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="4fc0e-130">VM Size</span></span> | <span data-ttu-id="4fc0e-131">Maximální velikost dočasného disk (GB)</span><span class="sxs-lookup"><span data-stu-id="4fc0e-131">Max temp disk size (GB)</span></span> |
|----|----|----|
| [<span data-ttu-id="4fc0e-132">Obecné účely</span><span class="sxs-lookup"><span data-stu-id="4fc0e-132">General purpose</span></span>](sizes-general.md) | <span data-ttu-id="4fc0e-133">A a řady D</span><span class="sxs-lookup"><span data-stu-id="4fc0e-133">A and D series</span></span> | <span data-ttu-id="4fc0e-134">800</span><span class="sxs-lookup"><span data-stu-id="4fc0e-134">800</span></span> |
| [<span data-ttu-id="4fc0e-135">Optimalizované z hlediska výpočetních služeb</span><span class="sxs-lookup"><span data-stu-id="4fc0e-135">Compute optimized</span></span>](sizes-compute.md) | <span data-ttu-id="4fc0e-136">Řady F</span><span class="sxs-lookup"><span data-stu-id="4fc0e-136">F series</span></span> | <span data-ttu-id="4fc0e-137">800</span><span class="sxs-lookup"><span data-stu-id="4fc0e-137">800</span></span> |
| [<span data-ttu-id="4fc0e-138">Optimalizované z hlediska paměti</span><span class="sxs-lookup"><span data-stu-id="4fc0e-138">Memory optimized</span></span>](../virtual-machines-windows-sizes-memory.md) | <span data-ttu-id="4fc0e-139">Série D a G</span><span class="sxs-lookup"><span data-stu-id="4fc0e-139">D and G series</span></span> | <span data-ttu-id="4fc0e-140">6144</span><span class="sxs-lookup"><span data-stu-id="4fc0e-140">6144</span></span> |
| [<span data-ttu-id="4fc0e-141">Optimalizované z hlediska úložiště</span><span class="sxs-lookup"><span data-stu-id="4fc0e-141">Storage optimized</span></span>](../virtual-machines-windows-sizes-storage.md) | <span data-ttu-id="4fc0e-142">Řada L</span><span class="sxs-lookup"><span data-stu-id="4fc0e-142">L series</span></span> | <span data-ttu-id="4fc0e-143">5630</span><span class="sxs-lookup"><span data-stu-id="4fc0e-143">5630</span></span> |
| [<span data-ttu-id="4fc0e-144">GPU</span><span class="sxs-lookup"><span data-stu-id="4fc0e-144">GPU</span></span>](sizes-gpu.md) | <span data-ttu-id="4fc0e-145">N řady</span><span class="sxs-lookup"><span data-stu-id="4fc0e-145">N series</span></span> | <span data-ttu-id="4fc0e-146">1440</span><span class="sxs-lookup"><span data-stu-id="4fc0e-146">1440</span></span> |
| [<span data-ttu-id="4fc0e-147">Vysoký výkon</span><span class="sxs-lookup"><span data-stu-id="4fc0e-147">High performance</span></span>](sizes-hpc.md) | <span data-ttu-id="4fc0e-148">A a řady H</span><span class="sxs-lookup"><span data-stu-id="4fc0e-148">A and H series</span></span> | <span data-ttu-id="4fc0e-149">2000</span><span class="sxs-lookup"><span data-stu-id="4fc0e-149">2000</span></span> |

## <a name="azure-data-disks"></a><span data-ttu-id="4fc0e-150">Azure datových disků</span><span class="sxs-lookup"><span data-stu-id="4fc0e-150">Azure data disks</span></span>

<span data-ttu-id="4fc0e-151">Pro instalaci aplikací a ukládání dat přidáním dalších datových disků.</span><span class="sxs-lookup"><span data-stu-id="4fc0e-151">Additional data disks can be added for installing applications and storing data.</span></span> <span data-ttu-id="4fc0e-152">Datové disky by měl použít v každé situaci, kde je žádoucí, odolné a dobře reagovaly datové úložiště.</span><span class="sxs-lookup"><span data-stu-id="4fc0e-152">Data disks should be used in any situation where durable and responsive data storage is desired.</span></span> <span data-ttu-id="4fc0e-153">Každý datový disk má maximální kapacita 1 terabajt.</span><span class="sxs-lookup"><span data-stu-id="4fc0e-153">Each data disk has a maximum capacity of 1 terabyte.</span></span> <span data-ttu-id="4fc0e-154">Velikost virtuálního počítače určuje, kolik datových disků lze připojit k virtuálnímu počítači.</span><span class="sxs-lookup"><span data-stu-id="4fc0e-154">The size of the virtual machine determines how many data disks can be attached to a VM.</span></span> <span data-ttu-id="4fc0e-155">Pro každý základní virtuální počítač se dá připojit dvěma datovými disky.</span><span class="sxs-lookup"><span data-stu-id="4fc0e-155">For each VM core, two data disks can be attached.</span></span> 

### <a name="max-data-disks-per-vm"></a><span data-ttu-id="4fc0e-156">Maximální počet datových disků na virtuální počítač</span><span class="sxs-lookup"><span data-stu-id="4fc0e-156">Max data disks per VM</span></span>

| <span data-ttu-id="4fc0e-157">Typ</span><span class="sxs-lookup"><span data-stu-id="4fc0e-157">Type</span></span> | <span data-ttu-id="4fc0e-158">Velikost virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="4fc0e-158">VM Size</span></span> | <span data-ttu-id="4fc0e-159">Maximální počet datových disků na virtuální počítač</span><span class="sxs-lookup"><span data-stu-id="4fc0e-159">Max data disks per VM</span></span> |
|----|----|----|
| [<span data-ttu-id="4fc0e-160">Obecné účely</span><span class="sxs-lookup"><span data-stu-id="4fc0e-160">General purpose</span></span>](sizes-general.md) | <span data-ttu-id="4fc0e-161">A a řady D</span><span class="sxs-lookup"><span data-stu-id="4fc0e-161">A and D series</span></span> | <span data-ttu-id="4fc0e-162">32</span><span class="sxs-lookup"><span data-stu-id="4fc0e-162">32</span></span> |
| [<span data-ttu-id="4fc0e-163">Optimalizované z hlediska výpočetních služeb</span><span class="sxs-lookup"><span data-stu-id="4fc0e-163">Compute optimized</span></span>](sizes-compute.md) | <span data-ttu-id="4fc0e-164">Řady F</span><span class="sxs-lookup"><span data-stu-id="4fc0e-164">F series</span></span> | <span data-ttu-id="4fc0e-165">32</span><span class="sxs-lookup"><span data-stu-id="4fc0e-165">32</span></span> |
| [<span data-ttu-id="4fc0e-166">Optimalizované z hlediska paměti</span><span class="sxs-lookup"><span data-stu-id="4fc0e-166">Memory optimized</span></span>](../virtual-machines-windows-sizes-memory.md) | <span data-ttu-id="4fc0e-167">Série D a G</span><span class="sxs-lookup"><span data-stu-id="4fc0e-167">D and G series</span></span> | <span data-ttu-id="4fc0e-168">64</span><span class="sxs-lookup"><span data-stu-id="4fc0e-168">64</span></span> |
| [<span data-ttu-id="4fc0e-169">Optimalizované z hlediska úložiště</span><span class="sxs-lookup"><span data-stu-id="4fc0e-169">Storage optimized</span></span>](../virtual-machines-windows-sizes-storage.md) | <span data-ttu-id="4fc0e-170">Řada L</span><span class="sxs-lookup"><span data-stu-id="4fc0e-170">L series</span></span> | <span data-ttu-id="4fc0e-171">64</span><span class="sxs-lookup"><span data-stu-id="4fc0e-171">64</span></span> |
| [<span data-ttu-id="4fc0e-172">GPU</span><span class="sxs-lookup"><span data-stu-id="4fc0e-172">GPU</span></span>](sizes-gpu.md) | <span data-ttu-id="4fc0e-173">N řady</span><span class="sxs-lookup"><span data-stu-id="4fc0e-173">N series</span></span> | <span data-ttu-id="4fc0e-174">48</span><span class="sxs-lookup"><span data-stu-id="4fc0e-174">48</span></span> |
| [<span data-ttu-id="4fc0e-175">Vysoký výkon</span><span class="sxs-lookup"><span data-stu-id="4fc0e-175">High performance</span></span>](sizes-hpc.md) | <span data-ttu-id="4fc0e-176">A a řady H</span><span class="sxs-lookup"><span data-stu-id="4fc0e-176">A and H series</span></span> | <span data-ttu-id="4fc0e-177">32</span><span class="sxs-lookup"><span data-stu-id="4fc0e-177">32</span></span> |

## <a name="vm-disk-types"></a><span data-ttu-id="4fc0e-178">Typy disků virtuálních počítačů</span><span class="sxs-lookup"><span data-stu-id="4fc0e-178">VM disk types</span></span>

<span data-ttu-id="4fc0e-179">Azure nabízí dva typy disku.</span><span class="sxs-lookup"><span data-stu-id="4fc0e-179">Azure provides two types of disk.</span></span>

### <a name="standard-disk"></a><span data-ttu-id="4fc0e-180">Disků na úrovni Standard</span><span class="sxs-lookup"><span data-stu-id="4fc0e-180">Standard disk</span></span>

<span data-ttu-id="4fc0e-181">Služba Storage úrovně Standard je založená na jednotkách HDD a poskytuje nákladově efektivní úložiště se zachováním výkonu.</span><span class="sxs-lookup"><span data-stu-id="4fc0e-181">Standard Storage is backed by HDDs, and delivers cost-effective storage while still being performant.</span></span> <span data-ttu-id="4fc0e-182">Standardní disky jsou ideální pro finančně efektivní vývoj a testování pracovního vytížení.</span><span class="sxs-lookup"><span data-stu-id="4fc0e-182">Standard disks are ideal for a cost effective dev and test workload.</span></span>

### <a name="premium-disk"></a><span data-ttu-id="4fc0e-183">Premium disku</span><span class="sxs-lookup"><span data-stu-id="4fc0e-183">Premium disk</span></span>

<span data-ttu-id="4fc0e-184">Pro prémiové disky jsou zajišťované založená na SSD vysoce výkonné, nízkou latencí disku.</span><span class="sxs-lookup"><span data-stu-id="4fc0e-184">Premium disks are backed by SSD-based high-performance, low-latency disk.</span></span> <span data-ttu-id="4fc0e-185">Ideální pro virtuální počítače se systémem produkční zatížení.</span><span class="sxs-lookup"><span data-stu-id="4fc0e-185">Perfect for VMs running production workload.</span></span> <span data-ttu-id="4fc0e-186">Premium Storage podporuje DS-series, DSv2-series, GS-series a virtuálních počítačů služby FS-series.</span><span class="sxs-lookup"><span data-stu-id="4fc0e-186">Premium Storage supports DS-series, DSv2-series, GS-series, and FS-series VMs.</span></span> <span data-ttu-id="4fc0e-187">Pro prémiové disky se musí uvést ve třech typech (P10 P20, P30), velikost disku určuje typ disku.</span><span class="sxs-lookup"><span data-stu-id="4fc0e-187">Premium disks come in three types (P10, P20, P30), the size of the disk determines the disk type.</span></span> <span data-ttu-id="4fc0e-188">Když vyberete, je velikost disku hodnota zaokrouhlený nahoru na další typ.</span><span class="sxs-lookup"><span data-stu-id="4fc0e-188">When selecting, a disk size the value is rounded up to the next type.</span></span> <span data-ttu-id="4fc0e-189">Například pokud je velikost je menší než 128 GB typ disku bude P10, mezi 129 a 512 P20 a více než 512 P30.</span><span class="sxs-lookup"><span data-stu-id="4fc0e-189">For example, if the size is below 128 GB the disk type will be P10, between 129 and 512 P20, and over 512 P30.</span></span> 

### <a name="premium-disk-performance"></a><span data-ttu-id="4fc0e-190">Výkon disku Premium</span><span class="sxs-lookup"><span data-stu-id="4fc0e-190">Premium disk performance</span></span>

|<span data-ttu-id="4fc0e-191">Typ disku úložiště Premium</span><span class="sxs-lookup"><span data-stu-id="4fc0e-191">Premium storage disk type</span></span> | <span data-ttu-id="4fc0e-192">P10</span><span class="sxs-lookup"><span data-stu-id="4fc0e-192">P10</span></span> | <span data-ttu-id="4fc0e-193">P20</span><span class="sxs-lookup"><span data-stu-id="4fc0e-193">P20</span></span> | <span data-ttu-id="4fc0e-194">P30</span><span class="sxs-lookup"><span data-stu-id="4fc0e-194">P30</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="4fc0e-195">Velikost disku (round nahoru)</span><span class="sxs-lookup"><span data-stu-id="4fc0e-195">Disk size (round up)</span></span> | <span data-ttu-id="4fc0e-196">128 GB</span><span class="sxs-lookup"><span data-stu-id="4fc0e-196">128 GB</span></span> | <span data-ttu-id="4fc0e-197">512 GB</span><span class="sxs-lookup"><span data-stu-id="4fc0e-197">512 GB</span></span> | <span data-ttu-id="4fc0e-198">1024 GB (1 TB)</span><span class="sxs-lookup"><span data-stu-id="4fc0e-198">1,024 GB (1 TB)</span></span> |
| <span data-ttu-id="4fc0e-199">Vstupně-výstupní operace za sekundu / disk</span><span class="sxs-lookup"><span data-stu-id="4fc0e-199">IOPS per disk</span></span> | <span data-ttu-id="4fc0e-200">500</span><span class="sxs-lookup"><span data-stu-id="4fc0e-200">500</span></span> | <span data-ttu-id="4fc0e-201">2,300</span><span class="sxs-lookup"><span data-stu-id="4fc0e-201">2,300</span></span> | <span data-ttu-id="4fc0e-202">5,000</span><span class="sxs-lookup"><span data-stu-id="4fc0e-202">5,000</span></span> |
<span data-ttu-id="4fc0e-203">Propustnost / disk</span><span class="sxs-lookup"><span data-stu-id="4fc0e-203">Throughput per disk</span></span> | <span data-ttu-id="4fc0e-204">100 MB/s</span><span class="sxs-lookup"><span data-stu-id="4fc0e-204">100 MB/s</span></span> | <span data-ttu-id="4fc0e-205">150 MB/s</span><span class="sxs-lookup"><span data-stu-id="4fc0e-205">150 MB/s</span></span> | <span data-ttu-id="4fc0e-206">200 MB/s</span><span class="sxs-lookup"><span data-stu-id="4fc0e-206">200 MB/s</span></span> |

<span data-ttu-id="4fc0e-207">Při výše uvedené tabulce jsou uvedeny maximální IOPS na disku, vyšší úroveň výkonu můžete dosáhnout prokládání více datových disků.</span><span class="sxs-lookup"><span data-stu-id="4fc0e-207">While the above table identifies max IOPS per disk, a higher level of performance can be achieved by striping multiple data disks.</span></span> <span data-ttu-id="4fc0e-208">Například 64 datových disků se dá připojit k virtuálnímu počítači Standard_GS5.</span><span class="sxs-lookup"><span data-stu-id="4fc0e-208">For instance, 64 data disks can be attached to Standard_GS5 VM.</span></span> <span data-ttu-id="4fc0e-209">Pokud každý z těchto disků jsou dimenzované jako P30, se dá dosáhnout maximálně 80 000 IOPS.</span><span class="sxs-lookup"><span data-stu-id="4fc0e-209">If each of these disks are sized as a P30, a maximum of 80,000 IOPS can be achieved.</span></span> <span data-ttu-id="4fc0e-210">Podrobné informace o maximální IOPS na virtuálních počítačů najdete v tématu [velikosti virtuálního počítače s Linuxem](./sizes.md).</span><span class="sxs-lookup"><span data-stu-id="4fc0e-210">For detailed information on max IOPS per VM, see [Linux VM sizes](./sizes.md).</span></span>

## <a name="create-and-attach-disks"></a><span data-ttu-id="4fc0e-211">Vytvořte a připojte disky</span><span class="sxs-lookup"><span data-stu-id="4fc0e-211">Create and attach disks</span></span>

<span data-ttu-id="4fc0e-212">Pokud chcete provést v příkladu v tomto kurzu, musí mít existující virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="4fc0e-212">To complete the example in this tutorial, you must have an existing virtual machine.</span></span> <span data-ttu-id="4fc0e-213">V případě potřeby to [ukázka skriptu](../scripts/virtual-machines-windows-powershell-sample-create-vm.md) můžete vytvořit za vás.</span><span class="sxs-lookup"><span data-stu-id="4fc0e-213">If needed, this [script sample](../scripts/virtual-machines-windows-powershell-sample-create-vm.md) can create one for you.</span></span> <span data-ttu-id="4fc0e-214">Při absolvování kurzu, nahraďte skupinu prostředků a virtuálních počítačů názvy, kde je potřeba.</span><span class="sxs-lookup"><span data-stu-id="4fc0e-214">When working through the tutorial, replace the resource group and VM names where needed.</span></span>

<span data-ttu-id="4fc0e-215">Vytvoření počáteční konfigurace s [New-AzureRmDiskConfig](/powershell/module/azurerm.compute/new-azurermdiskconfig).</span><span class="sxs-lookup"><span data-stu-id="4fc0e-215">Create the initial configuration with [New-AzureRmDiskConfig](/powershell/module/azurerm.compute/new-azurermdiskconfig).</span></span> <span data-ttu-id="4fc0e-216">Následující příklad konfiguruje disk, který je 128 GB velikost.</span><span class="sxs-lookup"><span data-stu-id="4fc0e-216">The following example configures a disk that is 128 gigabytes in size.</span></span>

```powershell
$diskConfig = New-AzureRmDiskConfig -Location EastUS -CreateOption Empty -DiskSizeGB 128
```

<span data-ttu-id="4fc0e-217">Vytvořit datový disk se [New-AzureRmDisk](/powershell/module/azurerm.compute/new-azurermdisk) příkaz.</span><span class="sxs-lookup"><span data-stu-id="4fc0e-217">Create the data disk with the [New-AzureRmDisk](/powershell/module/azurerm.compute/new-azurermdisk) command.</span></span>

```powershell
$dataDisk = New-AzureRmDisk -ResourceGroupName myResourceGroup -DiskName myDataDisk -Disk $diskConfig
```

<span data-ttu-id="4fc0e-218">Získat virtuální počítač, který chcete přidat datový disk do s [Get-AzureRmVM](/powershell/module/azurerm.compute/get-azurermvm) příkaz.</span><span class="sxs-lookup"><span data-stu-id="4fc0e-218">Get the virtual machine that you want to add the data disk to with the [Get-AzureRmVM](/powershell/module/azurerm.compute/get-azurermvm) command.</span></span>

```powershell
$vm = Get-AzureRmVM -ResourceGroupName myResourceGroup -Name myVM
```

<span data-ttu-id="4fc0e-219">Přidat datový disk pro konfiguraci virtuálního počítače s [přidat AzureRmVMDataDisk](/powershell/module/azurerm.compute/add-azurermvmdatadisk) příkaz.</span><span class="sxs-lookup"><span data-stu-id="4fc0e-219">Add the data disk to the virtual machine configuration with the [Add-AzureRmVMDataDisk](/powershell/module/azurerm.compute/add-azurermvmdatadisk) command.</span></span>

```powershell
$vm = Add-AzureRmVMDataDisk -VM $vm -Name myDataDisk -CreateOption Attach -ManagedDiskId $dataDisk.Id -Lun 1
```

<span data-ttu-id="4fc0e-220">Aktualizovat virtuální počítač s [aktualizace-AzureRmVM](/powershell/module/azurerm.compute/add-azurermvmdatadisk) příkaz.</span><span class="sxs-lookup"><span data-stu-id="4fc0e-220">Update the virtual machine with the [Update-AzureRmVM](/powershell/module/azurerm.compute/add-azurermvmdatadisk) command.</span></span>

```powershell
Update-AzureRmVM -ResourceGroupName myResourceGroup -VM $vm
```

## <a name="prepare-data-disks"></a><span data-ttu-id="4fc0e-221">Příprava datových disků</span><span class="sxs-lookup"><span data-stu-id="4fc0e-221">Prepare data disks</span></span>

<span data-ttu-id="4fc0e-222">Jakmile byla přiřazena disk k virtuálnímu počítači, musí se nakonfigurovat na použití disku operačního systému.</span><span class="sxs-lookup"><span data-stu-id="4fc0e-222">Once a disk has been attached to the virtual machine, the operating system needs to be configured to use the disk.</span></span> <span data-ttu-id="4fc0e-223">Následující příklad ukazuje, jak ručně nakonfigurovat první disk přidat do virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="4fc0e-223">The following example shows how to manually configure the first disk added to the VM.</span></span> <span data-ttu-id="4fc0e-224">Tento proces je také možné automatizovat pomocí [rozšíření vlastních skriptů](./tutorial-automate-vm-deployment.md).</span><span class="sxs-lookup"><span data-stu-id="4fc0e-224">This process can also be automated using the [custom script extension](./tutorial-automate-vm-deployment.md).</span></span>

### <a name="manual-configuration"></a><span data-ttu-id="4fc0e-225">Ruční konfigurace</span><span class="sxs-lookup"><span data-stu-id="4fc0e-225">Manual configuration</span></span>

<span data-ttu-id="4fc0e-226">Vytvořte připojení ke vzdálené ploše virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="4fc0e-226">Create an RDP connection with the virtual machine.</span></span> <span data-ttu-id="4fc0e-227">Otevřete prostředí PowerShell a spusťte tento skript.</span><span class="sxs-lookup"><span data-stu-id="4fc0e-227">Open up PowerShell and run this script.</span></span>

```powershell
Get-Disk | Where partitionstyle -eq 'raw' | `
Initialize-Disk -PartitionStyle MBR -PassThru | `
New-Partition -AssignDriveLetter -UseMaximumSize | `
Format-Volume -FileSystem NTFS -NewFileSystemLabel "myDataDisk" -Confirm:$false
```

## <a name="next-steps"></a><span data-ttu-id="4fc0e-228">Další kroky</span><span class="sxs-lookup"><span data-stu-id="4fc0e-228">Next steps</span></span>

<span data-ttu-id="4fc0e-229">V tomto kurzu jste se dozvěděli o tématech disky virtuálních počítačů, jako:</span><span class="sxs-lookup"><span data-stu-id="4fc0e-229">In this tutorial, you learned about VM disks topics such as:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="4fc0e-230">Operační systém a dočasný disky</span><span class="sxs-lookup"><span data-stu-id="4fc0e-230">OS disks and temporary disks</span></span>
> * <span data-ttu-id="4fc0e-231">Datové disky</span><span class="sxs-lookup"><span data-stu-id="4fc0e-231">Data disks</span></span>
> * <span data-ttu-id="4fc0e-232">Standard a Premium disky</span><span class="sxs-lookup"><span data-stu-id="4fc0e-232">Standard and Premium disks</span></span>
> * <span data-ttu-id="4fc0e-233">Výkon disku</span><span class="sxs-lookup"><span data-stu-id="4fc0e-233">Disk performance</span></span>
> * <span data-ttu-id="4fc0e-234">Připojení a příprava datových disků</span><span class="sxs-lookup"><span data-stu-id="4fc0e-234">Attaching and preparing data disks</span></span>

<span data-ttu-id="4fc0e-235">Přechodu na další informace o automatizaci konfigurace virtuálního počítače v dalším kurzu.</span><span class="sxs-lookup"><span data-stu-id="4fc0e-235">Advance to the next tutorial to learn about automating VM configuration.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="4fc0e-236">Automatizace konfigurace virtuálních počítačů</span><span class="sxs-lookup"><span data-stu-id="4fc0e-236">Automate VM configuration</span></span>](./tutorial-automate-vm-deployment.md)
