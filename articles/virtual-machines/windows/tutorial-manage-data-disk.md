---
title: "aaaManage Azure disky s hello prostředí Azure PowerShell | Microsoft Docs"
description: "Kurz – Správa Azure disků s hello prostředí Azure PowerShell"
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
ms.openlocfilehash: 2f61ad18bc94bab527d7ae593da603c6073adc89
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="manage-azure-disks-with-powershell"></a><span data-ttu-id="20ec6-103">Správa Azure disky pomocí prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="20ec6-103">Manage Azure disks with PowerShell</span></span>

<span data-ttu-id="20ec6-104">Virtuální počítače Azure pomocí disků toostore hello virtuální počítače operačního systému, aplikace a data.</span><span class="sxs-lookup"><span data-stu-id="20ec6-104">Azure virtual machines use disks toostore hello VMs operating system, applications, and data.</span></span> <span data-ttu-id="20ec6-105">Při vytváření virtuálního počítače je důležité toochoose velikost disku a příslušné toohello očekává úlohy konfigurace.</span><span class="sxs-lookup"><span data-stu-id="20ec6-105">When creating a VM it is important toochoose a disk size and configuration appropriate toohello expected workload.</span></span> <span data-ttu-id="20ec6-106">Tento kurz se zaměřuje na nasazení a správě disky virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="20ec6-106">This tutorial covers deploying and managing VM disks.</span></span> <span data-ttu-id="20ec6-107">Informace o:</span><span class="sxs-lookup"><span data-stu-id="20ec6-107">You learn about:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="20ec6-108">Operační systém a dočasný disky</span><span class="sxs-lookup"><span data-stu-id="20ec6-108">OS disks and temporary disks</span></span>
> * <span data-ttu-id="20ec6-109">Datové disky</span><span class="sxs-lookup"><span data-stu-id="20ec6-109">Data disks</span></span>
> * <span data-ttu-id="20ec6-110">Standard a Premium disky</span><span class="sxs-lookup"><span data-stu-id="20ec6-110">Standard and Premium disks</span></span>
> * <span data-ttu-id="20ec6-111">Výkon disku</span><span class="sxs-lookup"><span data-stu-id="20ec6-111">Disk performance</span></span>
> * <span data-ttu-id="20ec6-112">Připojení a příprava datových disků</span><span class="sxs-lookup"><span data-stu-id="20ec6-112">Attaching and preparing data disks</span></span>

<span data-ttu-id="20ec6-113">Tento kurz vyžaduje hello prostředí Azure PowerShell verze modulu 3,6 nebo novější.</span><span class="sxs-lookup"><span data-stu-id="20ec6-113">This tutorial requires hello Azure PowerShell module version 3.6 or later.</span></span> <span data-ttu-id="20ec6-114">Spustit ` Get-Module -ListAvailable AzureRM` toofind hello verze.</span><span class="sxs-lookup"><span data-stu-id="20ec6-114">Run ` Get-Module -ListAvailable AzureRM` toofind hello version.</span></span> <span data-ttu-id="20ec6-115">Pokud potřebujete tooupgrade, přečtěte si [modul nainstalovat Azure PowerShell](/powershell/azure/install-azurerm-ps).</span><span class="sxs-lookup"><span data-stu-id="20ec6-115">If you need tooupgrade, see [Install Azure PowerShell module](/powershell/azure/install-azurerm-ps).</span></span>

## <a name="default-azure-disks"></a><span data-ttu-id="20ec6-116">Výchozí disky systému Azure</span><span class="sxs-lookup"><span data-stu-id="20ec6-116">Default Azure disks</span></span>

<span data-ttu-id="20ec6-117">Když je vytvořen virtuální počítač Azure, jsou dva disky automaticky připojené toohello virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="20ec6-117">When an Azure virtual machine is created, two disks are automatically attached toohello virtual machine.</span></span> 

<span data-ttu-id="20ec6-118">**Disk s operačním systémem** – disků operačního systému může mít velikost až too1 terabajt a hello hostitelů virtuálních počítačů operačního systému.</span><span class="sxs-lookup"><span data-stu-id="20ec6-118">**Operating system disk** - Operating system disks can be sized up too1 terabyte, and hosts hello VMs operating system.</span></span>  <span data-ttu-id="20ec6-119">disk Hello operačního systému je přiřazeno písmeno jednotky *c:* ve výchozím nastavení.</span><span class="sxs-lookup"><span data-stu-id="20ec6-119">hello OS disk is assigned a drive letter of *c:* by default.</span></span> <span data-ttu-id="20ec6-120">ukládání do mezipaměti konfigurace disku operačního systému hello disku Hello je optimalizovaná pro výkon operačního systému.</span><span class="sxs-lookup"><span data-stu-id="20ec6-120">hello disk caching configuration of hello OS disk is optimized for OS performance.</span></span> <span data-ttu-id="20ec6-121">Hello operačního systému disku **neměli** hostitelem aplikace nebo data.</span><span class="sxs-lookup"><span data-stu-id="20ec6-121">hello OS disk **should not** host applications or data.</span></span> <span data-ttu-id="20ec6-122">Pro aplikace a data použijte datový disk, který je podrobně popsán později v tomto článku.</span><span class="sxs-lookup"><span data-stu-id="20ec6-122">For applications and data, use a data disk, which is detailed later in this article.</span></span>

<span data-ttu-id="20ec6-123">**Dočasným diskovým** -dočasné disky používají jednotkou SSD, který je umístěný na hello stejného Azure hostitele jako hello virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="20ec6-123">**Temporary disk** - Temporary disks use a solid-state drive that is located on hello same Azure host as hello VM.</span></span> <span data-ttu-id="20ec6-124">Dočasné disky jsou vysoce původce a mohou být použity pro operací, jako je dočasná data zpracování.</span><span class="sxs-lookup"><span data-stu-id="20ec6-124">Temp disks are highly performant and may be used for operations such as temporary data processing.</span></span> <span data-ttu-id="20ec6-125">Pokud hello virtuální počítač přesunutý tooa nového hostitele, je odebrat všechna data uložená na dočasném disku.</span><span class="sxs-lookup"><span data-stu-id="20ec6-125">However, if hello VM is moved tooa new host, any data stored on a temporary disk is removed.</span></span> <span data-ttu-id="20ec6-126">Hello velikost dočasné disku hello je určen podle hello velikost virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="20ec6-126">hello size of hello temporary disk is determined by hello VM size.</span></span> <span data-ttu-id="20ec6-127">Dočasné disky přiřazené písmeno jednotky *d:* ve výchozím nastavení.</span><span class="sxs-lookup"><span data-stu-id="20ec6-127">Temporary disks are assigned a drive letter of *d:* by default.</span></span>

### <a name="temporary-disk-sizes"></a><span data-ttu-id="20ec6-128">Velikosti dočasné disků</span><span class="sxs-lookup"><span data-stu-id="20ec6-128">Temporary disk sizes</span></span>

| <span data-ttu-id="20ec6-129">Typ</span><span class="sxs-lookup"><span data-stu-id="20ec6-129">Type</span></span> | <span data-ttu-id="20ec6-130">Velikost virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="20ec6-130">VM Size</span></span> | <span data-ttu-id="20ec6-131">Maximální velikost dočasného disk (GB)</span><span class="sxs-lookup"><span data-stu-id="20ec6-131">Max temp disk size (GB)</span></span> |
|----|----|----|
| [<span data-ttu-id="20ec6-132">Obecné účely</span><span class="sxs-lookup"><span data-stu-id="20ec6-132">General purpose</span></span>](sizes-general.md) | <span data-ttu-id="20ec6-133">A a řady D</span><span class="sxs-lookup"><span data-stu-id="20ec6-133">A and D series</span></span> | <span data-ttu-id="20ec6-134">800</span><span class="sxs-lookup"><span data-stu-id="20ec6-134">800</span></span> |
| [<span data-ttu-id="20ec6-135">Optimalizované z hlediska výpočetních služeb</span><span class="sxs-lookup"><span data-stu-id="20ec6-135">Compute optimized</span></span>](sizes-compute.md) | <span data-ttu-id="20ec6-136">Řady F</span><span class="sxs-lookup"><span data-stu-id="20ec6-136">F series</span></span> | <span data-ttu-id="20ec6-137">800</span><span class="sxs-lookup"><span data-stu-id="20ec6-137">800</span></span> |
| [<span data-ttu-id="20ec6-138">Optimalizované z hlediska paměti</span><span class="sxs-lookup"><span data-stu-id="20ec6-138">Memory optimized</span></span>](../virtual-machines-windows-sizes-memory.md) | <span data-ttu-id="20ec6-139">Série D a G</span><span class="sxs-lookup"><span data-stu-id="20ec6-139">D and G series</span></span> | <span data-ttu-id="20ec6-140">6144</span><span class="sxs-lookup"><span data-stu-id="20ec6-140">6144</span></span> |
| [<span data-ttu-id="20ec6-141">Optimalizované z hlediska úložiště</span><span class="sxs-lookup"><span data-stu-id="20ec6-141">Storage optimized</span></span>](../virtual-machines-windows-sizes-storage.md) | <span data-ttu-id="20ec6-142">Řada L</span><span class="sxs-lookup"><span data-stu-id="20ec6-142">L series</span></span> | <span data-ttu-id="20ec6-143">5630</span><span class="sxs-lookup"><span data-stu-id="20ec6-143">5630</span></span> |
| [<span data-ttu-id="20ec6-144">GPU</span><span class="sxs-lookup"><span data-stu-id="20ec6-144">GPU</span></span>](sizes-gpu.md) | <span data-ttu-id="20ec6-145">N řady</span><span class="sxs-lookup"><span data-stu-id="20ec6-145">N series</span></span> | <span data-ttu-id="20ec6-146">1440</span><span class="sxs-lookup"><span data-stu-id="20ec6-146">1440</span></span> |
| [<span data-ttu-id="20ec6-147">Vysoký výkon</span><span class="sxs-lookup"><span data-stu-id="20ec6-147">High performance</span></span>](sizes-hpc.md) | <span data-ttu-id="20ec6-148">A a řady H</span><span class="sxs-lookup"><span data-stu-id="20ec6-148">A and H series</span></span> | <span data-ttu-id="20ec6-149">2000</span><span class="sxs-lookup"><span data-stu-id="20ec6-149">2000</span></span> |

## <a name="azure-data-disks"></a><span data-ttu-id="20ec6-150">Azure datových disků</span><span class="sxs-lookup"><span data-stu-id="20ec6-150">Azure data disks</span></span>

<span data-ttu-id="20ec6-151">Pro instalaci aplikací a ukládání dat přidáním dalších datových disků.</span><span class="sxs-lookup"><span data-stu-id="20ec6-151">Additional data disks can be added for installing applications and storing data.</span></span> <span data-ttu-id="20ec6-152">Datové disky by měl použít v každé situaci, kde je žádoucí, odolné a dobře reagovaly datové úložiště.</span><span class="sxs-lookup"><span data-stu-id="20ec6-152">Data disks should be used in any situation where durable and responsive data storage is desired.</span></span> <span data-ttu-id="20ec6-153">Každý datový disk má maximální kapacita 1 terabajt.</span><span class="sxs-lookup"><span data-stu-id="20ec6-153">Each data disk has a maximum capacity of 1 terabyte.</span></span> <span data-ttu-id="20ec6-154">velikost Hello hello virtuálního počítače určuje, kolik datových disků může být připojené tooa virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="20ec6-154">hello size of hello virtual machine determines how many data disks can be attached tooa VM.</span></span> <span data-ttu-id="20ec6-155">Pro každý základní virtuální počítač se dá připojit dvěma datovými disky.</span><span class="sxs-lookup"><span data-stu-id="20ec6-155">For each VM core, two data disks can be attached.</span></span> 

### <a name="max-data-disks-per-vm"></a><span data-ttu-id="20ec6-156">Maximální počet datových disků na virtuální počítač</span><span class="sxs-lookup"><span data-stu-id="20ec6-156">Max data disks per VM</span></span>

| <span data-ttu-id="20ec6-157">Typ</span><span class="sxs-lookup"><span data-stu-id="20ec6-157">Type</span></span> | <span data-ttu-id="20ec6-158">Velikost virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="20ec6-158">VM Size</span></span> | <span data-ttu-id="20ec6-159">Maximální počet datových disků na virtuální počítač</span><span class="sxs-lookup"><span data-stu-id="20ec6-159">Max data disks per VM</span></span> |
|----|----|----|
| [<span data-ttu-id="20ec6-160">Obecné účely</span><span class="sxs-lookup"><span data-stu-id="20ec6-160">General purpose</span></span>](sizes-general.md) | <span data-ttu-id="20ec6-161">A a řady D</span><span class="sxs-lookup"><span data-stu-id="20ec6-161">A and D series</span></span> | <span data-ttu-id="20ec6-162">32</span><span class="sxs-lookup"><span data-stu-id="20ec6-162">32</span></span> |
| [<span data-ttu-id="20ec6-163">Optimalizované z hlediska výpočetních služeb</span><span class="sxs-lookup"><span data-stu-id="20ec6-163">Compute optimized</span></span>](sizes-compute.md) | <span data-ttu-id="20ec6-164">Řady F</span><span class="sxs-lookup"><span data-stu-id="20ec6-164">F series</span></span> | <span data-ttu-id="20ec6-165">32</span><span class="sxs-lookup"><span data-stu-id="20ec6-165">32</span></span> |
| [<span data-ttu-id="20ec6-166">Optimalizované z hlediska paměti</span><span class="sxs-lookup"><span data-stu-id="20ec6-166">Memory optimized</span></span>](../virtual-machines-windows-sizes-memory.md) | <span data-ttu-id="20ec6-167">Série D a G</span><span class="sxs-lookup"><span data-stu-id="20ec6-167">D and G series</span></span> | <span data-ttu-id="20ec6-168">64</span><span class="sxs-lookup"><span data-stu-id="20ec6-168">64</span></span> |
| [<span data-ttu-id="20ec6-169">Optimalizované z hlediska úložiště</span><span class="sxs-lookup"><span data-stu-id="20ec6-169">Storage optimized</span></span>](../virtual-machines-windows-sizes-storage.md) | <span data-ttu-id="20ec6-170">Řada L</span><span class="sxs-lookup"><span data-stu-id="20ec6-170">L series</span></span> | <span data-ttu-id="20ec6-171">64</span><span class="sxs-lookup"><span data-stu-id="20ec6-171">64</span></span> |
| [<span data-ttu-id="20ec6-172">GPU</span><span class="sxs-lookup"><span data-stu-id="20ec6-172">GPU</span></span>](sizes-gpu.md) | <span data-ttu-id="20ec6-173">N řady</span><span class="sxs-lookup"><span data-stu-id="20ec6-173">N series</span></span> | <span data-ttu-id="20ec6-174">48</span><span class="sxs-lookup"><span data-stu-id="20ec6-174">48</span></span> |
| [<span data-ttu-id="20ec6-175">Vysoký výkon</span><span class="sxs-lookup"><span data-stu-id="20ec6-175">High performance</span></span>](sizes-hpc.md) | <span data-ttu-id="20ec6-176">A a řady H</span><span class="sxs-lookup"><span data-stu-id="20ec6-176">A and H series</span></span> | <span data-ttu-id="20ec6-177">32</span><span class="sxs-lookup"><span data-stu-id="20ec6-177">32</span></span> |

## <a name="vm-disk-types"></a><span data-ttu-id="20ec6-178">Typy disků virtuálních počítačů</span><span class="sxs-lookup"><span data-stu-id="20ec6-178">VM disk types</span></span>

<span data-ttu-id="20ec6-179">Azure nabízí dva typy disku.</span><span class="sxs-lookup"><span data-stu-id="20ec6-179">Azure provides two types of disk.</span></span>

### <a name="standard-disk"></a><span data-ttu-id="20ec6-180">Disků na úrovni Standard</span><span class="sxs-lookup"><span data-stu-id="20ec6-180">Standard disk</span></span>

<span data-ttu-id="20ec6-181">Služba Storage úrovně Standard je založená na jednotkách HDD a poskytuje nákladově efektivní úložiště se zachováním výkonu.</span><span class="sxs-lookup"><span data-stu-id="20ec6-181">Standard Storage is backed by HDDs, and delivers cost-effective storage while still being performant.</span></span> <span data-ttu-id="20ec6-182">Standardní disky jsou ideální pro finančně efektivní vývoj a testování pracovního vytížení.</span><span class="sxs-lookup"><span data-stu-id="20ec6-182">Standard disks are ideal for a cost effective dev and test workload.</span></span>

### <a name="premium-disk"></a><span data-ttu-id="20ec6-183">Premium disku</span><span class="sxs-lookup"><span data-stu-id="20ec6-183">Premium disk</span></span>

<span data-ttu-id="20ec6-184">Pro prémiové disky jsou zajišťované založená na SSD vysoce výkonné, nízkou latencí disku.</span><span class="sxs-lookup"><span data-stu-id="20ec6-184">Premium disks are backed by SSD-based high-performance, low-latency disk.</span></span> <span data-ttu-id="20ec6-185">Ideální pro virtuální počítače se systémem produkční zatížení.</span><span class="sxs-lookup"><span data-stu-id="20ec6-185">Perfect for VMs running production workload.</span></span> <span data-ttu-id="20ec6-186">Premium Storage podporuje DS-series, DSv2-series, GS-series a virtuálních počítačů služby FS-series.</span><span class="sxs-lookup"><span data-stu-id="20ec6-186">Premium Storage supports DS-series, DSv2-series, GS-series, and FS-series VMs.</span></span> <span data-ttu-id="20ec6-187">Prémiové disky se musí uvést ve třech typech (P10 P20, P30), velikost hello hello disku určuje typ disku hello.</span><span class="sxs-lookup"><span data-stu-id="20ec6-187">Premium disks come in three types (P10, P20, P30), hello size of hello disk determines hello disk type.</span></span> <span data-ttu-id="20ec6-188">Když vyberete, hodnota hello velikosti disku zaokrouhlit toohello další typ.</span><span class="sxs-lookup"><span data-stu-id="20ec6-188">When selecting, a disk size hello value is rounded up toohello next type.</span></span> <span data-ttu-id="20ec6-189">Například pokud hello velikost je menší než 128 GB typ disku hello bude P10, mezi 129 a 512 P20 a více než 512 P30.</span><span class="sxs-lookup"><span data-stu-id="20ec6-189">For example, if hello size is below 128 GB hello disk type will be P10, between 129 and 512 P20, and over 512 P30.</span></span> 

### <a name="premium-disk-performance"></a><span data-ttu-id="20ec6-190">Výkon disku Premium</span><span class="sxs-lookup"><span data-stu-id="20ec6-190">Premium disk performance</span></span>

|<span data-ttu-id="20ec6-191">Typ disku úložiště Premium</span><span class="sxs-lookup"><span data-stu-id="20ec6-191">Premium storage disk type</span></span> | <span data-ttu-id="20ec6-192">P10</span><span class="sxs-lookup"><span data-stu-id="20ec6-192">P10</span></span> | <span data-ttu-id="20ec6-193">P20</span><span class="sxs-lookup"><span data-stu-id="20ec6-193">P20</span></span> | <span data-ttu-id="20ec6-194">P30</span><span class="sxs-lookup"><span data-stu-id="20ec6-194">P30</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="20ec6-195">Velikost disku (round nahoru)</span><span class="sxs-lookup"><span data-stu-id="20ec6-195">Disk size (round up)</span></span> | <span data-ttu-id="20ec6-196">128 GB</span><span class="sxs-lookup"><span data-stu-id="20ec6-196">128 GB</span></span> | <span data-ttu-id="20ec6-197">512 GB</span><span class="sxs-lookup"><span data-stu-id="20ec6-197">512 GB</span></span> | <span data-ttu-id="20ec6-198">1024 GB (1 TB)</span><span class="sxs-lookup"><span data-stu-id="20ec6-198">1,024 GB (1 TB)</span></span> |
| <span data-ttu-id="20ec6-199">Vstupně-výstupní operace za sekundu / disk</span><span class="sxs-lookup"><span data-stu-id="20ec6-199">IOPS per disk</span></span> | <span data-ttu-id="20ec6-200">500</span><span class="sxs-lookup"><span data-stu-id="20ec6-200">500</span></span> | <span data-ttu-id="20ec6-201">2,300</span><span class="sxs-lookup"><span data-stu-id="20ec6-201">2,300</span></span> | <span data-ttu-id="20ec6-202">5,000</span><span class="sxs-lookup"><span data-stu-id="20ec6-202">5,000</span></span> |
<span data-ttu-id="20ec6-203">Propustnost / disk</span><span class="sxs-lookup"><span data-stu-id="20ec6-203">Throughput per disk</span></span> | <span data-ttu-id="20ec6-204">100 MB/s</span><span class="sxs-lookup"><span data-stu-id="20ec6-204">100 MB/s</span></span> | <span data-ttu-id="20ec6-205">150 MB/s</span><span class="sxs-lookup"><span data-stu-id="20ec6-205">150 MB/s</span></span> | <span data-ttu-id="20ec6-206">200 MB/s</span><span class="sxs-lookup"><span data-stu-id="20ec6-206">200 MB/s</span></span> |

<span data-ttu-id="20ec6-207">Při hello výše tabulky identifikuje maximální IOPS na disku, vyšší úroveň výkonu můžete dosáhnout prokládání více datových disků.</span><span class="sxs-lookup"><span data-stu-id="20ec6-207">While hello above table identifies max IOPS per disk, a higher level of performance can be achieved by striping multiple data disks.</span></span> <span data-ttu-id="20ec6-208">Například 64 dat, který může být disky připojené tooStandard_GS5 virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="20ec6-208">For instance, 64 data disks can be attached tooStandard_GS5 VM.</span></span> <span data-ttu-id="20ec6-209">Pokud každý z těchto disků jsou dimenzované jako P30, se dá dosáhnout maximálně 80 000 IOPS.</span><span class="sxs-lookup"><span data-stu-id="20ec6-209">If each of these disks are sized as a P30, a maximum of 80,000 IOPS can be achieved.</span></span> <span data-ttu-id="20ec6-210">Podrobné informace o maximální IOPS na virtuálních počítačů najdete v tématu [velikosti virtuálního počítače s Linuxem](./sizes.md).</span><span class="sxs-lookup"><span data-stu-id="20ec6-210">For detailed information on max IOPS per VM, see [Linux VM sizes](./sizes.md).</span></span>

## <a name="create-and-attach-disks"></a><span data-ttu-id="20ec6-211">Vytvořte a připojte disky</span><span class="sxs-lookup"><span data-stu-id="20ec6-211">Create and attach disks</span></span>

<span data-ttu-id="20ec6-212">Příklad hello toocomplete v tomto kurzu, musí mít existující virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="20ec6-212">toocomplete hello example in this tutorial, you must have an existing virtual machine.</span></span> <span data-ttu-id="20ec6-213">V případě potřeby to [ukázka skriptu](../scripts/virtual-machines-windows-powershell-sample-create-vm.md) můžete vytvořit za vás.</span><span class="sxs-lookup"><span data-stu-id="20ec6-213">If needed, this [script sample](../scripts/virtual-machines-windows-powershell-sample-create-vm.md) can create one for you.</span></span> <span data-ttu-id="20ec6-214">Při absolvování hello kurzu, nahraďte hello skupinu prostředků a virtuálních počítačů názvy, kde je potřeba.</span><span class="sxs-lookup"><span data-stu-id="20ec6-214">When working through hello tutorial, replace hello resource group and VM names where needed.</span></span>

<span data-ttu-id="20ec6-215">Vytvoření hello počáteční konfigurace s [New-AzureRmDiskConfig](/powershell/module/azurerm.compute/new-azurermdiskconfig).</span><span class="sxs-lookup"><span data-stu-id="20ec6-215">Create hello initial configuration with [New-AzureRmDiskConfig](/powershell/module/azurerm.compute/new-azurermdiskconfig).</span></span> <span data-ttu-id="20ec6-216">Následující ukázka Hello nakonfiguruje disk, který je 128 GB velikost.</span><span class="sxs-lookup"><span data-stu-id="20ec6-216">hello following example configures a disk that is 128 gigabytes in size.</span></span>

```powershell
$diskConfig = New-AzureRmDiskConfig -Location EastUS -CreateOption Empty -DiskSizeGB 128
```

<span data-ttu-id="20ec6-217">Vytvoření hello datový disk s hello [New-AzureRmDisk](/powershell/module/azurerm.compute/new-azurermdisk) příkaz.</span><span class="sxs-lookup"><span data-stu-id="20ec6-217">Create hello data disk with hello [New-AzureRmDisk](/powershell/module/azurerm.compute/new-azurermdisk) command.</span></span>

```powershell
$dataDisk = New-AzureRmDisk -ResourceGroupName myResourceGroup -DiskName myDataDisk -Disk $diskConfig
```

<span data-ttu-id="20ec6-218">Get hello virtuálního počítače, které chcete tooadd hello datového disku toowith hello [Get-AzureRmVM](/powershell/module/azurerm.compute/get-azurermvm) příkaz.</span><span class="sxs-lookup"><span data-stu-id="20ec6-218">Get hello virtual machine that you want tooadd hello data disk toowith hello [Get-AzureRmVM](/powershell/module/azurerm.compute/get-azurermvm) command.</span></span>

```powershell
$vm = Get-AzureRmVM -ResourceGroupName myResourceGroup -Name myVM
```

<span data-ttu-id="20ec6-219">Přidat hello data toohello virtuální počítač konfigurace disku s hello [přidat AzureRmVMDataDisk](/powershell/module/azurerm.compute/add-azurermvmdatadisk) příkaz.</span><span class="sxs-lookup"><span data-stu-id="20ec6-219">Add hello data disk toohello virtual machine configuration with hello [Add-AzureRmVMDataDisk](/powershell/module/azurerm.compute/add-azurermvmdatadisk) command.</span></span>

```powershell
$vm = Add-AzureRmVMDataDisk -VM $vm -Name myDataDisk -CreateOption Attach -ManagedDiskId $dataDisk.Id -Lun 1
```

<span data-ttu-id="20ec6-220">Aktualizovat hello virtuální počítač s hello [aktualizace-AzureRmVM](/powershell/module/azurerm.compute/add-azurermvmdatadisk) příkaz.</span><span class="sxs-lookup"><span data-stu-id="20ec6-220">Update hello virtual machine with hello [Update-AzureRmVM](/powershell/module/azurerm.compute/add-azurermvmdatadisk) command.</span></span>

```powershell
Update-AzureRmVM -ResourceGroupName myResourceGroup -VM $vm
```

## <a name="prepare-data-disks"></a><span data-ttu-id="20ec6-221">Příprava datových disků</span><span class="sxs-lookup"><span data-stu-id="20ec6-221">Prepare data disks</span></span>

<span data-ttu-id="20ec6-222">Jakmile disk virtuálního počítače připojené toohello, už hello operačního systému musí toobe nakonfigurované toouse hello disku.</span><span class="sxs-lookup"><span data-stu-id="20ec6-222">Once a disk has been attached toohello virtual machine, hello operating system needs toobe configured toouse hello disk.</span></span> <span data-ttu-id="20ec6-223">Hello následující příklad ukazuje, jak nakonfigurovat toomanually hello první disk přidat toohello virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="20ec6-223">hello following example shows how toomanually configure hello first disk added toohello VM.</span></span> <span data-ttu-id="20ec6-224">Tento proces je také možné automatizovat pomocí hello [rozšíření vlastních skriptů](./tutorial-automate-vm-deployment.md).</span><span class="sxs-lookup"><span data-stu-id="20ec6-224">This process can also be automated using hello [custom script extension](./tutorial-automate-vm-deployment.md).</span></span>

### <a name="manual-configuration"></a><span data-ttu-id="20ec6-225">Ruční konfigurace</span><span class="sxs-lookup"><span data-stu-id="20ec6-225">Manual configuration</span></span>

<span data-ttu-id="20ec6-226">Vytvořte připojení ke vzdálené ploše s hello virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="20ec6-226">Create an RDP connection with hello virtual machine.</span></span> <span data-ttu-id="20ec6-227">Otevřete prostředí PowerShell a spusťte tento skript.</span><span class="sxs-lookup"><span data-stu-id="20ec6-227">Open up PowerShell and run this script.</span></span>

```powershell
Get-Disk | Where partitionstyle -eq 'raw' | `
Initialize-Disk -PartitionStyle MBR -PassThru | `
New-Partition -AssignDriveLetter -UseMaximumSize | `
Format-Volume -FileSystem NTFS -NewFileSystemLabel "myDataDisk" -Confirm:$false
```

## <a name="next-steps"></a><span data-ttu-id="20ec6-228">Další kroky</span><span class="sxs-lookup"><span data-stu-id="20ec6-228">Next steps</span></span>

<span data-ttu-id="20ec6-229">V tomto kurzu jste se dozvěděli o tématech disky virtuálních počítačů, jako:</span><span class="sxs-lookup"><span data-stu-id="20ec6-229">In this tutorial, you learned about VM disks topics such as:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="20ec6-230">Operační systém a dočasný disky</span><span class="sxs-lookup"><span data-stu-id="20ec6-230">OS disks and temporary disks</span></span>
> * <span data-ttu-id="20ec6-231">Datové disky</span><span class="sxs-lookup"><span data-stu-id="20ec6-231">Data disks</span></span>
> * <span data-ttu-id="20ec6-232">Standard a Premium disky</span><span class="sxs-lookup"><span data-stu-id="20ec6-232">Standard and Premium disks</span></span>
> * <span data-ttu-id="20ec6-233">Výkon disku</span><span class="sxs-lookup"><span data-stu-id="20ec6-233">Disk performance</span></span>
> * <span data-ttu-id="20ec6-234">Připojení a příprava datových disků</span><span class="sxs-lookup"><span data-stu-id="20ec6-234">Attaching and preparing data disks</span></span>

<span data-ttu-id="20ec6-235">Posunutí další kurz toolearn toohello o automatizaci konfigurace virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="20ec6-235">Advance toohello next tutorial toolearn about automating VM configuration.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="20ec6-236">Automatizace konfigurace virtuálních počítačů</span><span class="sxs-lookup"><span data-stu-id="20ec6-236">Automate VM configuration</span></span>](./tutorial-automate-vm-deployment.md)
