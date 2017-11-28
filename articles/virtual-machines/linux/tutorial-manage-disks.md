---
title: "aaaManage Azure disky s hello rozhraní příkazového řádku Azure | Microsoft Docs"
description: "Kurz – Správa Azure disků s hello rozhraní příkazového řádku Azure"
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
ms.openlocfilehash: ad29cfbec5f9738f47705b19d0450c9a2f555492
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="manage-azure-disks-with-hello-azure-cli"></a><span data-ttu-id="3af64-103">Správa Azure disků s hello rozhraní příkazového řádku Azure</span><span class="sxs-lookup"><span data-stu-id="3af64-103">Manage Azure disks with hello Azure CLI</span></span>

<span data-ttu-id="3af64-104">Virtuální počítače Azure pomocí disků toostore hello virtuální počítače operačního systému, aplikace a data.</span><span class="sxs-lookup"><span data-stu-id="3af64-104">Azure virtual machines use disks toostore hello VMs operating system, applications, and data.</span></span> <span data-ttu-id="3af64-105">Při vytváření virtuálního počítače je důležité toochoose velikost disku a příslušné toohello očekává úlohy konfigurace.</span><span class="sxs-lookup"><span data-stu-id="3af64-105">When creating a VM it is important toochoose a disk size and configuration appropriate toohello expected workload.</span></span> <span data-ttu-id="3af64-106">Tento kurz se zaměřuje na nasazení a správě disky virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="3af64-106">This tutorial covers deploying and managing VM disks.</span></span> <span data-ttu-id="3af64-107">Informace o:</span><span class="sxs-lookup"><span data-stu-id="3af64-107">You learn about:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="3af64-108">Operační systém a dočasný disky</span><span class="sxs-lookup"><span data-stu-id="3af64-108">OS disks and temporary disks</span></span>
> * <span data-ttu-id="3af64-109">Datové disky</span><span class="sxs-lookup"><span data-stu-id="3af64-109">Data disks</span></span>
> * <span data-ttu-id="3af64-110">Standard a Premium disky</span><span class="sxs-lookup"><span data-stu-id="3af64-110">Standard and Premium disks</span></span>
> * <span data-ttu-id="3af64-111">Výkon disku</span><span class="sxs-lookup"><span data-stu-id="3af64-111">Disk performance</span></span>
> * <span data-ttu-id="3af64-112">Připojení a příprava datových disků</span><span class="sxs-lookup"><span data-stu-id="3af64-112">Attaching and preparing data disks</span></span>
> * <span data-ttu-id="3af64-113">Změna velikosti disků</span><span class="sxs-lookup"><span data-stu-id="3af64-113">Resizing disks</span></span>
> * <span data-ttu-id="3af64-114">Snímky disku</span><span class="sxs-lookup"><span data-stu-id="3af64-114">Disk snapshots</span></span>


[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

<span data-ttu-id="3af64-115">Pokud zvolíte tooinstall a místně pomocí hello rozhraní příkazového řádku, tento kurz vyžaduje, že používáte verzi rozhraní příkazového řádku Azure hello verze 2.0.4 nebo novější.</span><span class="sxs-lookup"><span data-stu-id="3af64-115">If you choose tooinstall and use hello CLI locally, this tutorial requires that you are running hello Azure CLI version 2.0.4 or later.</span></span> <span data-ttu-id="3af64-116">Spustit `az --version` toofind hello verze.</span><span class="sxs-lookup"><span data-stu-id="3af64-116">Run `az --version` toofind hello version.</span></span> <span data-ttu-id="3af64-117">Pokud potřebujete tooinstall nebo aktualizace, přečtěte si [nainstalovat Azure CLI 2.0]( /cli/azure/install-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="3af64-117">If you need tooinstall or upgrade, see [Install Azure CLI 2.0]( /cli/azure/install-azure-cli).</span></span> 

## <a name="default-azure-disks"></a><span data-ttu-id="3af64-118">Výchozí disky systému Azure</span><span class="sxs-lookup"><span data-stu-id="3af64-118">Default Azure disks</span></span>

<span data-ttu-id="3af64-119">Když je vytvořen virtuální počítač Azure, jsou dva disky automaticky připojené toohello virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="3af64-119">When an Azure virtual machine is created, two disks are automatically attached toohello virtual machine.</span></span> 

<span data-ttu-id="3af64-120">**Disk s operačním systémem** – disků operačního systému může mít velikost až too1 terabajt a hello hostitelů virtuálních počítačů operačního systému.</span><span class="sxs-lookup"><span data-stu-id="3af64-120">**Operating system disk** - Operating system disks can be sized up too1 terabyte, and hosts hello VMs operating system.</span></span> <span data-ttu-id="3af64-121">Hello OS disk označený */dev/sda* ve výchozím nastavení.</span><span class="sxs-lookup"><span data-stu-id="3af64-121">hello OS disk is labeled */dev/sda* by default.</span></span> <span data-ttu-id="3af64-122">ukládání do mezipaměti konfigurace disku operačního systému hello disku Hello je optimalizovaná pro výkon operačního systému.</span><span class="sxs-lookup"><span data-stu-id="3af64-122">hello disk caching configuration of hello OS disk is optimized for OS performance.</span></span> <span data-ttu-id="3af64-123">Z důvodu této konfiguraci hello disk s operačním systémem **neměli** hostitelem aplikace nebo data.</span><span class="sxs-lookup"><span data-stu-id="3af64-123">Because of this configuration, hello OS disk **should not** host applications or data.</span></span> <span data-ttu-id="3af64-124">Pro aplikace a data použijte datových disků, které jsou podrobně popsané dál v tomto článku.</span><span class="sxs-lookup"><span data-stu-id="3af64-124">For applications and data, use data disks, which are detailed later in this article.</span></span> 

<span data-ttu-id="3af64-125">**Dočasným diskovým** -dočasné disky používají jednotkou SSD, který je umístěný na hello stejného Azure hostitele jako hello virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="3af64-125">**Temporary disk** - Temporary disks use a solid-state drive that is located on hello same Azure host as hello VM.</span></span> <span data-ttu-id="3af64-126">Dočasné disky jsou vysoce původce a mohou být použity pro operací, jako je dočasná data zpracování.</span><span class="sxs-lookup"><span data-stu-id="3af64-126">Temp disks are highly performant and may be used for operations such as temporary data processing.</span></span> <span data-ttu-id="3af64-127">Pokud hello virtuální počítač přesunutý tooa nového hostitele, je odebrat všechna data uložená na dočasném disku.</span><span class="sxs-lookup"><span data-stu-id="3af64-127">However, if hello VM is moved tooa new host, any data stored on a temporary disk is removed.</span></span> <span data-ttu-id="3af64-128">Hello velikost dočasné disku hello je určen podle hello velikost virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="3af64-128">hello size of hello temporary disk is determined by hello VM size.</span></span> <span data-ttu-id="3af64-129">Dočasné disky jsou označeny */dev/sdb* a mít přípojný bod systému */mnt*.</span><span class="sxs-lookup"><span data-stu-id="3af64-129">Temporary disks are labeled */dev/sdb* and have a mountpoint of */mnt*.</span></span>

### <a name="temporary-disk-sizes"></a><span data-ttu-id="3af64-130">Velikosti dočasné disků</span><span class="sxs-lookup"><span data-stu-id="3af64-130">Temporary disk sizes</span></span>

| <span data-ttu-id="3af64-131">Typ</span><span class="sxs-lookup"><span data-stu-id="3af64-131">Type</span></span> | <span data-ttu-id="3af64-132">Velikost virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="3af64-132">VM Size</span></span> | <span data-ttu-id="3af64-133">Maximální velikost dočasného disk (GB)</span><span class="sxs-lookup"><span data-stu-id="3af64-133">Max temp disk size (GB)</span></span> |
|----|----|----|
| [<span data-ttu-id="3af64-134">Obecné účely</span><span class="sxs-lookup"><span data-stu-id="3af64-134">General purpose</span></span>](sizes-general.md) | <span data-ttu-id="3af64-135">A a řady D</span><span class="sxs-lookup"><span data-stu-id="3af64-135">A and D series</span></span> | <span data-ttu-id="3af64-136">800</span><span class="sxs-lookup"><span data-stu-id="3af64-136">800</span></span> |
| [<span data-ttu-id="3af64-137">Optimalizované z hlediska výpočetních služeb</span><span class="sxs-lookup"><span data-stu-id="3af64-137">Compute optimized</span></span>](sizes-compute.md) | <span data-ttu-id="3af64-138">Řady F</span><span class="sxs-lookup"><span data-stu-id="3af64-138">F series</span></span> | <span data-ttu-id="3af64-139">800</span><span class="sxs-lookup"><span data-stu-id="3af64-139">800</span></span> |
| [<span data-ttu-id="3af64-140">Optimalizované z hlediska paměti</span><span class="sxs-lookup"><span data-stu-id="3af64-140">Memory optimized</span></span>](../virtual-machines-windows-sizes-memory.md) | <span data-ttu-id="3af64-141">Série D a G</span><span class="sxs-lookup"><span data-stu-id="3af64-141">D and G series</span></span> | <span data-ttu-id="3af64-142">6144</span><span class="sxs-lookup"><span data-stu-id="3af64-142">6144</span></span> |
| [<span data-ttu-id="3af64-143">Optimalizované z hlediska úložiště</span><span class="sxs-lookup"><span data-stu-id="3af64-143">Storage optimized</span></span>](../virtual-machines-windows-sizes-storage.md) | <span data-ttu-id="3af64-144">Řada L</span><span class="sxs-lookup"><span data-stu-id="3af64-144">L series</span></span> | <span data-ttu-id="3af64-145">5630</span><span class="sxs-lookup"><span data-stu-id="3af64-145">5630</span></span> |
| [<span data-ttu-id="3af64-146">GPU</span><span class="sxs-lookup"><span data-stu-id="3af64-146">GPU</span></span>](sizes-gpu.md) | <span data-ttu-id="3af64-147">N řady</span><span class="sxs-lookup"><span data-stu-id="3af64-147">N series</span></span> | <span data-ttu-id="3af64-148">1440</span><span class="sxs-lookup"><span data-stu-id="3af64-148">1440</span></span> |
| [<span data-ttu-id="3af64-149">Vysoký výkon</span><span class="sxs-lookup"><span data-stu-id="3af64-149">High performance</span></span>](sizes-hpc.md) | <span data-ttu-id="3af64-150">A a řady H</span><span class="sxs-lookup"><span data-stu-id="3af64-150">A and H series</span></span> | <span data-ttu-id="3af64-151">2000</span><span class="sxs-lookup"><span data-stu-id="3af64-151">2000</span></span> |

## <a name="azure-data-disks"></a><span data-ttu-id="3af64-152">Azure datových disků</span><span class="sxs-lookup"><span data-stu-id="3af64-152">Azure data disks</span></span>

<span data-ttu-id="3af64-153">Pro instalaci aplikací a ukládání dat přidáním dalších datových disků.</span><span class="sxs-lookup"><span data-stu-id="3af64-153">Additional data disks can be added for installing applications and storing data.</span></span> <span data-ttu-id="3af64-154">Datové disky by měl použít v každé situaci, kde je žádoucí, odolné a dobře reagovaly datové úložiště.</span><span class="sxs-lookup"><span data-stu-id="3af64-154">Data disks should be used in any situation where durable and responsive data storage is desired.</span></span> <span data-ttu-id="3af64-155">Každý datový disk má maximální kapacita 1 terabajt.</span><span class="sxs-lookup"><span data-stu-id="3af64-155">Each data disk has a maximum capacity of 1 terabyte.</span></span> <span data-ttu-id="3af64-156">velikost Hello hello virtuálního počítače určuje, kolik datových disků může být připojené tooa virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="3af64-156">hello size of hello virtual machine determines how many data disks can be attached tooa VM.</span></span> <span data-ttu-id="3af64-157">Pro každý základní virtuální počítač se dá připojit dvěma datovými disky.</span><span class="sxs-lookup"><span data-stu-id="3af64-157">For each VM core, two data disks can be attached.</span></span> 

### <a name="max-data-disks-per-vm"></a><span data-ttu-id="3af64-158">Maximální počet datových disků na virtuální počítač</span><span class="sxs-lookup"><span data-stu-id="3af64-158">Max data disks per VM</span></span>

| <span data-ttu-id="3af64-159">Typ</span><span class="sxs-lookup"><span data-stu-id="3af64-159">Type</span></span> | <span data-ttu-id="3af64-160">Velikost virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="3af64-160">VM Size</span></span> | <span data-ttu-id="3af64-161">Maximální počet datových disků na virtuální počítač</span><span class="sxs-lookup"><span data-stu-id="3af64-161">Max data disks per VM</span></span> |
|----|----|----|
| [<span data-ttu-id="3af64-162">Obecné účely</span><span class="sxs-lookup"><span data-stu-id="3af64-162">General purpose</span></span>](sizes-general.md) | <span data-ttu-id="3af64-163">A a řady D</span><span class="sxs-lookup"><span data-stu-id="3af64-163">A and D series</span></span> | <span data-ttu-id="3af64-164">32</span><span class="sxs-lookup"><span data-stu-id="3af64-164">32</span></span> |
| [<span data-ttu-id="3af64-165">Optimalizované z hlediska výpočetních služeb</span><span class="sxs-lookup"><span data-stu-id="3af64-165">Compute optimized</span></span>](sizes-compute.md) | <span data-ttu-id="3af64-166">Řady F</span><span class="sxs-lookup"><span data-stu-id="3af64-166">F series</span></span> | <span data-ttu-id="3af64-167">32</span><span class="sxs-lookup"><span data-stu-id="3af64-167">32</span></span> |
| [<span data-ttu-id="3af64-168">Optimalizované z hlediska paměti</span><span class="sxs-lookup"><span data-stu-id="3af64-168">Memory optimized</span></span>](../virtual-machines-windows-sizes-memory.md) | <span data-ttu-id="3af64-169">Série D a G</span><span class="sxs-lookup"><span data-stu-id="3af64-169">D and G series</span></span> | <span data-ttu-id="3af64-170">64</span><span class="sxs-lookup"><span data-stu-id="3af64-170">64</span></span> |
| [<span data-ttu-id="3af64-171">Optimalizované z hlediska úložiště</span><span class="sxs-lookup"><span data-stu-id="3af64-171">Storage optimized</span></span>](../virtual-machines-windows-sizes-storage.md) | <span data-ttu-id="3af64-172">Řada L</span><span class="sxs-lookup"><span data-stu-id="3af64-172">L series</span></span> | <span data-ttu-id="3af64-173">64</span><span class="sxs-lookup"><span data-stu-id="3af64-173">64</span></span> |
| [<span data-ttu-id="3af64-174">GPU</span><span class="sxs-lookup"><span data-stu-id="3af64-174">GPU</span></span>](sizes-gpu.md) | <span data-ttu-id="3af64-175">N řady</span><span class="sxs-lookup"><span data-stu-id="3af64-175">N series</span></span> | <span data-ttu-id="3af64-176">48</span><span class="sxs-lookup"><span data-stu-id="3af64-176">48</span></span> |
| [<span data-ttu-id="3af64-177">Vysoký výkon</span><span class="sxs-lookup"><span data-stu-id="3af64-177">High performance</span></span>](sizes-hpc.md) | <span data-ttu-id="3af64-178">A a řady H</span><span class="sxs-lookup"><span data-stu-id="3af64-178">A and H series</span></span> | <span data-ttu-id="3af64-179">32</span><span class="sxs-lookup"><span data-stu-id="3af64-179">32</span></span> |

## <a name="vm-disk-types"></a><span data-ttu-id="3af64-180">Typy disků virtuálních počítačů</span><span class="sxs-lookup"><span data-stu-id="3af64-180">VM disk types</span></span>

<span data-ttu-id="3af64-181">Azure nabízí dva typy disku.</span><span class="sxs-lookup"><span data-stu-id="3af64-181">Azure provides two types of disk.</span></span>

### <a name="standard-disk"></a><span data-ttu-id="3af64-182">Disků na úrovni Standard</span><span class="sxs-lookup"><span data-stu-id="3af64-182">Standard disk</span></span>

<span data-ttu-id="3af64-183">Služba Storage úrovně Standard je založená na jednotkách HDD a poskytuje nákladově efektivní úložiště se zachováním výkonu.</span><span class="sxs-lookup"><span data-stu-id="3af64-183">Standard Storage is backed by HDDs, and delivers cost-effective storage while still being performant.</span></span> <span data-ttu-id="3af64-184">Standardní disky jsou ideální pro finančně efektivní vývoj a testování pracovního vytížení.</span><span class="sxs-lookup"><span data-stu-id="3af64-184">Standard disks are ideal for a cost effective dev and test workload.</span></span>

### <a name="premium-disk"></a><span data-ttu-id="3af64-185">Premium disku</span><span class="sxs-lookup"><span data-stu-id="3af64-185">Premium disk</span></span>

<span data-ttu-id="3af64-186">Pro prémiové disky jsou zajišťované založená na SSD vysoce výkonné, nízkou latencí disku.</span><span class="sxs-lookup"><span data-stu-id="3af64-186">Premium disks are backed by SSD-based high-performance, low-latency disk.</span></span> <span data-ttu-id="3af64-187">Ideální pro virtuální počítače se systémem produkční zatížení.</span><span class="sxs-lookup"><span data-stu-id="3af64-187">Perfect for VMs running production workload.</span></span> <span data-ttu-id="3af64-188">Premium Storage podporuje DS-series, DSv2-series, GS-series a virtuálních počítačů služby FS-series.</span><span class="sxs-lookup"><span data-stu-id="3af64-188">Premium Storage supports DS-series, DSv2-series, GS-series, and FS-series VMs.</span></span> <span data-ttu-id="3af64-189">Prémiové disky se musí uvést ve třech typech (P10 P20, P30), velikost hello hello disku určuje typ disku hello.</span><span class="sxs-lookup"><span data-stu-id="3af64-189">Premium disks come in three types (P10, P20, P30), hello size of hello disk determines hello disk type.</span></span> <span data-ttu-id="3af64-190">Když vyberete, hodnota hello velikosti disku zaokrouhlit toohello další typ.</span><span class="sxs-lookup"><span data-stu-id="3af64-190">When selecting, a disk size hello value is rounded up toohello next type.</span></span> <span data-ttu-id="3af64-191">Například pokud hello velikost disku je menší než 128 GB, typ disku hello je P10.</span><span class="sxs-lookup"><span data-stu-id="3af64-191">For example, if hello disk size is less than 128 GB, hello disk type is P10.</span></span> <span data-ttu-id="3af64-192">Pokud je velikost disku hello 129 GB až 512 GB, velikost hello je P20.</span><span class="sxs-lookup"><span data-stu-id="3af64-192">If hello disk size is between 129 GB and 512 GB, hello size is a P20.</span></span> <span data-ttu-id="3af64-193">Nic více než 512 GB, velikost hello je P30.</span><span class="sxs-lookup"><span data-stu-id="3af64-193">Anything over 512 GB, hello size is a P30.</span></span>

### <a name="premium-disk-performance"></a><span data-ttu-id="3af64-194">Výkon disku Premium</span><span class="sxs-lookup"><span data-stu-id="3af64-194">Premium disk performance</span></span>

|<span data-ttu-id="3af64-195">Typ disku úložiště Premium</span><span class="sxs-lookup"><span data-stu-id="3af64-195">Premium storage disk type</span></span> | <span data-ttu-id="3af64-196">P10</span><span class="sxs-lookup"><span data-stu-id="3af64-196">P10</span></span> | <span data-ttu-id="3af64-197">P20</span><span class="sxs-lookup"><span data-stu-id="3af64-197">P20</span></span> | <span data-ttu-id="3af64-198">P30</span><span class="sxs-lookup"><span data-stu-id="3af64-198">P30</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="3af64-199">Velikost disku (round nahoru)</span><span class="sxs-lookup"><span data-stu-id="3af64-199">Disk size (round up)</span></span> | <span data-ttu-id="3af64-200">128 GB</span><span class="sxs-lookup"><span data-stu-id="3af64-200">128 GB</span></span> | <span data-ttu-id="3af64-201">512 GB</span><span class="sxs-lookup"><span data-stu-id="3af64-201">512 GB</span></span> | <span data-ttu-id="3af64-202">1024 GB (1 TB)</span><span class="sxs-lookup"><span data-stu-id="3af64-202">1,024 GB (1 TB)</span></span> |
| <span data-ttu-id="3af64-203">Maximum vstupně-výstupních operací za sekundu (IOPS) na disk</span><span class="sxs-lookup"><span data-stu-id="3af64-203">Max IOPS per disk</span></span> | <span data-ttu-id="3af64-204">500</span><span class="sxs-lookup"><span data-stu-id="3af64-204">500</span></span> | <span data-ttu-id="3af64-205">2,300</span><span class="sxs-lookup"><span data-stu-id="3af64-205">2,300</span></span> | <span data-ttu-id="3af64-206">5,000</span><span class="sxs-lookup"><span data-stu-id="3af64-206">5,000</span></span> |
<span data-ttu-id="3af64-207">Propustnost / disk</span><span class="sxs-lookup"><span data-stu-id="3af64-207">Throughput per disk</span></span> | <span data-ttu-id="3af64-208">100 MB/s</span><span class="sxs-lookup"><span data-stu-id="3af64-208">100 MB/s</span></span> | <span data-ttu-id="3af64-209">150 MB/s</span><span class="sxs-lookup"><span data-stu-id="3af64-209">150 MB/s</span></span> | <span data-ttu-id="3af64-210">200 MB/s</span><span class="sxs-lookup"><span data-stu-id="3af64-210">200 MB/s</span></span> |

<span data-ttu-id="3af64-211">Při hello výše tabulky identifikuje maximální IOPS na disku, vyšší úroveň výkonu můžete dosáhnout prokládání více datových disků.</span><span class="sxs-lookup"><span data-stu-id="3af64-211">While hello above table identifies max IOPS per disk, a higher level of performance can be achieved by striping multiple data disks.</span></span> <span data-ttu-id="3af64-212">Pro instanci virtuálního počítače Standard_GS5 můžete dosáhnout maximálně 80 000 IOPS.</span><span class="sxs-lookup"><span data-stu-id="3af64-212">For instance, a Standard_GS5 VM can achieve a maximum of 80,000 IOPS.</span></span> <span data-ttu-id="3af64-213">Podrobné informace o maximální IOPS na virtuálních počítačů najdete v tématu [velikosti virtuálního počítače s Linuxem](sizes.md).</span><span class="sxs-lookup"><span data-stu-id="3af64-213">For detailed information on max IOPS per VM, see [Linux VM sizes](sizes.md).</span></span>

## <a name="create-and-attach-disks"></a><span data-ttu-id="3af64-214">Vytvořte a připojte disky</span><span class="sxs-lookup"><span data-stu-id="3af64-214">Create and attach disks</span></span>

<span data-ttu-id="3af64-215">Datové disky můžete vytvořit a připojit v okamžiku vytvoření virtuálního počítače nebo tooan existující virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="3af64-215">Data disks can be created and attached at VM creation time or tooan existing VM.</span></span>

### <a name="attach-disk-at-vm-creation"></a><span data-ttu-id="3af64-216">Připojit disk při vytváření virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="3af64-216">Attach disk at VM creation</span></span>

<span data-ttu-id="3af64-217">Vytvořte skupinu prostředků s hello [vytvořit skupinu az](https://docs.microsoft.com/cli/azure/group#create) příkaz.</span><span class="sxs-lookup"><span data-stu-id="3af64-217">Create a resource group with hello [az group create](https://docs.microsoft.com/cli/azure/group#create) command.</span></span> 

```azurecli-interactive 
az group create --name myResourceGroupDisk --location eastus
```

<span data-ttu-id="3af64-218">Vytvoření virtuálního počítače pomocí hello [vytvořit virtuální počítač az]( /cli/azure/vm#create) příkaz.</span><span class="sxs-lookup"><span data-stu-id="3af64-218">Create a VM using hello [az vm create]( /cli/azure/vm#create) command.</span></span> <span data-ttu-id="3af64-219">Hello `--datadisk-sizes-gb` argument je použité toospecify, že další disk by měl vytvořit a připojit toohello virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="3af64-219">hello `--datadisk-sizes-gb` argument is used toospecify that an additional disk should be created and attached toohello virtual machine.</span></span> <span data-ttu-id="3af64-220">toocreate a připojit více než jeden disk, použijte mezerami oddělený seznam hodnot velikosti disku.</span><span class="sxs-lookup"><span data-stu-id="3af64-220">toocreate and attach more than one disk, use a space-delimited list of disk size values.</span></span> <span data-ttu-id="3af64-221">V následujícím příkladu hello je virtuální počítač vytvořený s dvěma datovými disky, oba 128 GB.</span><span class="sxs-lookup"><span data-stu-id="3af64-221">In hello following example, a VM is created with two data disks, both 128 GB.</span></span> <span data-ttu-id="3af64-222">Protože hello velikosti disků jsou 128 GB, jsou obě tyto disky nakonfigurované jako P10s, které poskytují maximální 500 IOPS na disku.</span><span class="sxs-lookup"><span data-stu-id="3af64-222">Because hello disk sizes are 128 GB, these disks are both configured as P10s, which provide maximum 500 IOPS per disk.</span></span>

```azurecli-interactive 
az vm create \
  --resource-group myResourceGroupDisk \
  --name myVM \
  --image UbuntuLTS \
  --size Standard_DS2_v2 \
  --data-disk-sizes-gb 128 128 \
  --generate-ssh-keys
```

### <a name="attach-disk-tooexisting-vm"></a><span data-ttu-id="3af64-223">Připojte tooexisting disku virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="3af64-223">Attach disk tooexisting VM</span></span>

<span data-ttu-id="3af64-224">toocreate a připojit nový disk tooan existující virtuální počítač, použijte hello [připojit disk virtuálního počítače az](/cli/azure/vm/disk#attach) příkaz.</span><span class="sxs-lookup"><span data-stu-id="3af64-224">toocreate and attach a new disk tooan existing virtual machine, use hello [az vm disk attach](/cli/azure/vm/disk#attach) command.</span></span> <span data-ttu-id="3af64-225">Hello následující příklad vytvoří disk premium, 128 GB, velikost a připojí jej toohello, kterou virtuální počítač vytvořen v posledním kroku hello.</span><span class="sxs-lookup"><span data-stu-id="3af64-225">hello following example creates a premium disk, 128 gigabytes in size, and attaches it toohello VM created in hello last step.</span></span>

```azurecli-interactive 
az vm disk attach --vm-name myVM --resource-group myResourceGroupDisk --disk myDataDisk --size-gb 128 --sku Premium_LRS --new 
```

## <a name="prepare-data-disks"></a><span data-ttu-id="3af64-226">Příprava datových disků</span><span class="sxs-lookup"><span data-stu-id="3af64-226">Prepare data disks</span></span>

<span data-ttu-id="3af64-227">Jakmile disk virtuálního počítače připojené toohello, už hello operačního systému musí toobe nakonfigurované toouse hello disku.</span><span class="sxs-lookup"><span data-stu-id="3af64-227">Once a disk has been attached toohello virtual machine, hello operating system needs toobe configured toouse hello disk.</span></span> <span data-ttu-id="3af64-228">Hello následující příklad ukazuje, jak toomanually konfigurace disku.</span><span class="sxs-lookup"><span data-stu-id="3af64-228">hello following example shows how toomanually configure a disk.</span></span> <span data-ttu-id="3af64-229">Tento proces je také možné automatizovat pomocí inicializací cloudu, kterému se věnujeme v [novější kurzu](./tutorial-automate-vm-deployment.md).</span><span class="sxs-lookup"><span data-stu-id="3af64-229">This process can also be automated using cloud-init, which is covered in a [later tutorial](./tutorial-automate-vm-deployment.md).</span></span>

### <a name="manual-configuration"></a><span data-ttu-id="3af64-230">Ruční konfigurace</span><span class="sxs-lookup"><span data-stu-id="3af64-230">Manual configuration</span></span>

<span data-ttu-id="3af64-231">Vytvoření připojení SSH s hello virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="3af64-231">Create an SSH connection with hello virtual machine.</span></span> <span data-ttu-id="3af64-232">Nahraďte hello příklad IP adresu s veřejnou IP adresu hello hello virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="3af64-232">Replace hello example IP address with hello public IP of hello virtual machine.</span></span>

```azurecli-interactive 
ssh 52.174.34.95
```

<span data-ttu-id="3af64-233">Rozdělit disk na hello s `fdisk`.</span><span class="sxs-lookup"><span data-stu-id="3af64-233">Partition hello disk with `fdisk`.</span></span>

```bash
(echo n; echo p; echo 1; echo ; echo ; echo w) | sudo fdisk /dev/sdc
```

<span data-ttu-id="3af64-234">Zapsat systému souborů toohello oddílu pomocí hello `mkfs` příkaz.</span><span class="sxs-lookup"><span data-stu-id="3af64-234">Write a file system toohello partition by using hello `mkfs` command.</span></span>

```bash
sudo mkfs -t ext4 /dev/sdc1
```

<span data-ttu-id="3af64-235">Připojte nový disk hello tak, aby se v hello operačního systému.</span><span class="sxs-lookup"><span data-stu-id="3af64-235">Mount hello new disk so that it is accessible in hello operating system.</span></span>

```bash
sudo mkdir /datadrive && sudo mount /dev/sdc1 /datadrive
```

<span data-ttu-id="3af64-236">Hello disku se dá dostat teď hello *datadrive* přípojný bod, který lze ověřit spuštěním hello `df -h` příkaz.</span><span class="sxs-lookup"><span data-stu-id="3af64-236">hello disk can now be accessed through hello *datadrive* mountpoint, which can be verified by running hello `df -h` command.</span></span> 

```bash
df -h
```

<span data-ttu-id="3af64-237">výstup Hello zobrazuje hello připojit na nový disk */datadrive*.</span><span class="sxs-lookup"><span data-stu-id="3af64-237">hello output shows hello new drive mounted on */datadrive*.</span></span>

```bash
Filesystem      Size  Used Avail Use% Mounted on
/dev/sda1        30G  1.4G   28G   5% /
/dev/sdb1       6.8G   16M  6.4G   1% /mnt
/dev/sdc1        50G   52M   47G   1% /datadrive
```

<span data-ttu-id="3af64-238">tooensure, který hello jednotky je znovu připojeny po restartování systému, musí být přidané toohello */etc/fstab* souboru.</span><span class="sxs-lookup"><span data-stu-id="3af64-238">tooensure that hello drive is remounted after a reboot, it must be added toohello */etc/fstab* file.</span></span> <span data-ttu-id="3af64-239">toodo tedy získat hello UUID disku hello s hello `blkid` nástroj.</span><span class="sxs-lookup"><span data-stu-id="3af64-239">toodo so, get hello UUID of hello disk with hello `blkid` utility.</span></span>

```bash
sudo -i blkid
```

<span data-ttu-id="3af64-240">výstup Hello zobrazí hello UUID disku hello `/dev/sdc1` v tomto případě.</span><span class="sxs-lookup"><span data-stu-id="3af64-240">hello output displays hello UUID of hello drive, `/dev/sdc1` in this case.</span></span>

```bash
/dev/sdc1: UUID="33333333-3b3b-3c3c-3d3d-3e3e3e3e3e3e" TYPE="ext4"
```

<span data-ttu-id="3af64-241">Přidání řádku podobné toohello následující toohello */etc/fstab* souboru.</span><span class="sxs-lookup"><span data-stu-id="3af64-241">Add a line similar toohello following toohello */etc/fstab* file.</span></span> <span data-ttu-id="3af64-242">Také Upozorňujeme, že zápis překážek můžete zakázat pomocí *bariéry = 0*, tato konfigurace může zlepšit výkon disku.</span><span class="sxs-lookup"><span data-stu-id="3af64-242">Also note that write barriers can be disabled using *barrier=0*, this configuration can improve disk performance.</span></span> 

```bash
UUID=33333333-3b3b-3c3c-3d3d-3e3e3e3e3e3e   /datadrive  ext4    defaults,nofail,barrier=0   1  2
```

<span data-ttu-id="3af64-243">Teď, když hello disku byl nakonfigurován, zavřete relace SSH hello.</span><span class="sxs-lookup"><span data-stu-id="3af64-243">Now that hello disk has been configured, close hello SSH session.</span></span>

```bash
exit
```

## <a name="resize-vm-disk"></a><span data-ttu-id="3af64-244">Změna velikosti disku virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="3af64-244">Resize VM disk</span></span>

<span data-ttu-id="3af64-245">Po nasazený virtuální počítač, disk operačního systému hello nebo jakýchkoli připojených datových disků je možné zvýšit velikost.</span><span class="sxs-lookup"><span data-stu-id="3af64-245">Once a VM has been deployed, hello operating system disk or any attached data disks can be increased in size.</span></span> <span data-ttu-id="3af64-246">Zvýšení hello velikost disku je v případě nutnosti další úložný prostor nebo vyšší úroveň výkonu (P10, P20, P30).</span><span class="sxs-lookup"><span data-stu-id="3af64-246">Increasing hello size of a disk is beneficial when needing more storage space or a higher level of performance (P10, P20, P30).</span></span> <span data-ttu-id="3af64-247">Všimněte si, že se disky není možné snížit velikost.</span><span class="sxs-lookup"><span data-stu-id="3af64-247">Note, disks cannot be decreased in size.</span></span>

<span data-ttu-id="3af64-248">Před zvýšením velikosti disku, hello Id nebo název hello disku je potřeba.</span><span class="sxs-lookup"><span data-stu-id="3af64-248">Before increasing disk size, hello Id or name of hello disk is needed.</span></span> <span data-ttu-id="3af64-249">Použití hello [seznam disků az](/cli/azure/vm/disk#list) příkaz tooreturn všechny disky ve skupině prostředků.</span><span class="sxs-lookup"><span data-stu-id="3af64-249">Use hello [az disk list](/cli/azure/vm/disk#list) command tooreturn all disks in a resource group.</span></span> <span data-ttu-id="3af64-250">Poznamenejte si název hello disku, které chcete tooresize.</span><span class="sxs-lookup"><span data-stu-id="3af64-250">Take note of hello disk name that you would like tooresize.</span></span>

```azurecli-interactive 
az disk list -g myResourceGroupDisk --query '[*].{Name:name,Gb:diskSizeGb,Tier:accountType}' --output table
```

<span data-ttu-id="3af64-251">také musí být deallocated Hello virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="3af64-251">hello VM must also be deallocated.</span></span> <span data-ttu-id="3af64-252">Použití hello [az OM deallocate]( /cli/azure/vm#deallocate) příkaz toostop a navrátit hello virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="3af64-252">Use hello [az vm deallocate]( /cli/azure/vm#deallocate) command toostop and deallocate hello VM.</span></span>

```azurecli-interactive 
az vm deallocate --resource-group myResourceGroupDisk --name myVM
```

<span data-ttu-id="3af64-253">Použití hello [aktualizace disku az](/cli/azure/vm/disk#update) příkaz tooresize hello disku.</span><span class="sxs-lookup"><span data-stu-id="3af64-253">Use hello [az disk update](/cli/azure/vm/disk#update) command tooresize hello disk.</span></span> <span data-ttu-id="3af64-254">Tento příklad změní velikost disku s názvem *myDataDisk* too1 terabajt.</span><span class="sxs-lookup"><span data-stu-id="3af64-254">This example resizes a disk named *myDataDisk* too1 terabyte.</span></span>

```azurecli-interactive 
az disk update --name myDataDisk --resource-group myResourceGroupDisk --size-gb 1023
```

<span data-ttu-id="3af64-255">Po dokončení operace změny velikosti hello spusťte hello virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="3af64-255">Once hello resize operation has completed, start hello VM.</span></span>

```azurecli-interactive 
az vm start --resource-group myResourceGroupDisk --name myVM
```

<span data-ttu-id="3af64-256">Pokud jste ke změně velikosti hello operačního systému disku, je automaticky rozšířena hello oddílu.</span><span class="sxs-lookup"><span data-stu-id="3af64-256">If you’ve resized hello operating system disk, hello partition is automatically be expanded.</span></span> <span data-ttu-id="3af64-257">Pokud jste změnili datový disk, musí všechny aktuální oddíly toobe rozšířit hello virtuální počítače operačního systému.</span><span class="sxs-lookup"><span data-stu-id="3af64-257">If you have resized a data disk, any current partitions need toobe expanded in hello VMs operating system.</span></span>

## <a name="snapshot-azure-disks"></a><span data-ttu-id="3af64-258">Snímek disky systému Azure</span><span class="sxs-lookup"><span data-stu-id="3af64-258">Snapshot Azure disks</span></span>

<span data-ttu-id="3af64-259">Pořízení snímku disku vytvoří čtení pouze, v okamžiku kopie disku hello.</span><span class="sxs-lookup"><span data-stu-id="3af64-259">Taking a disk snapshot creates a read only, point-in-time copy of hello disk.</span></span> <span data-ttu-id="3af64-260">Azure snímky virtuálních počítačů jsou užitečné pro rychle ukládají hello stav virtuálního počítače, před prováděním změn konfigurace.</span><span class="sxs-lookup"><span data-stu-id="3af64-260">Azure VM snapshots are useful for quickly saving hello state of a VM before making configuration changes.</span></span> <span data-ttu-id="3af64-261">V případě hello hello změny konfigurace prokázat toobe nežádoucí, stav virtuálního počítače může být obnovovány pomocí snímků hello.</span><span class="sxs-lookup"><span data-stu-id="3af64-261">In hello event hello configuration changes prove toobe undesired, VM state can be restored using hello snapshot.</span></span> <span data-ttu-id="3af64-262">Virtuální počítač má více než jeden disk, je snímek prováděné každého disku nezávisle na hello ostatní.</span><span class="sxs-lookup"><span data-stu-id="3af64-262">When a VM has more than one disk, a snapshot is taken of each disk independently of hello others.</span></span> <span data-ttu-id="3af64-263">Za vyjádření zálohování konzistentní s aplikací, vezměte v úvahu zastavení hello virtuálních počítačů před přepnutím snímky disku.</span><span class="sxs-lookup"><span data-stu-id="3af64-263">For taking application consistent backups, consider stopping hello VM before taking disk snapshots.</span></span> <span data-ttu-id="3af64-264">Můžete taky použít hello [služby Azure Backup](/azure/backup/), které umožňuje vám tooperform automatizované zálohování při hello je spuštěný virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="3af64-264">Alternatively, use hello [Azure Backup service](/azure/backup/), which enables you tooperform automated backups while hello VM is running.</span></span>

### <a name="create-snapshot"></a><span data-ttu-id="3af64-265">Vytvořit snímek</span><span class="sxs-lookup"><span data-stu-id="3af64-265">Create snapshot</span></span>

<span data-ttu-id="3af64-266">Před vytvořením snímku disku virtuálního počítače, hello Id nebo názvem hello disku je potřeba.</span><span class="sxs-lookup"><span data-stu-id="3af64-266">Before creating a virtual machine disk snapshot, hello Id or name of hello disk is needed.</span></span> <span data-ttu-id="3af64-267">Použití hello [az virtuálních počítačů zobrazit](https://docs.microsoft.com/en-us/cli/azure/vm#show) id disku hello tooreturn příkaz. V tomto příkladu je id disku hello uložené v proměnné, aby se může použít později.</span><span class="sxs-lookup"><span data-stu-id="3af64-267">Use hello [az vm show](https://docs.microsoft.com/en-us/cli/azure/vm#show) command tooreturn hello disk id. In this example, hello disk id is stored in a variable so that it can be used in a later step.</span></span>

```azurecli-interactive 
osdiskid=$(az vm show -g myResourceGroupDisk -n myVM --query "storageProfile.osDisk.managedDisk.id" -o tsv)
```

<span data-ttu-id="3af64-268">Teď, když máte hello id hello disku virtuálního počítače, hello následující příkaz vytvoří snímek hello disku.</span><span class="sxs-lookup"><span data-stu-id="3af64-268">Now that you have hello id of hello virtual machine disk, hello following command creates a snapshot of hello disk.</span></span>

```azurcli
az snapshot create -g myResourceGroupDisk --source "$osdiskid" --name osDisk-backup
```

### <a name="create-disk-from-snapshot"></a><span data-ttu-id="3af64-269">Vytvoření disku ze snímku</span><span class="sxs-lookup"><span data-stu-id="3af64-269">Create disk from snapshot</span></span>

<span data-ttu-id="3af64-270">Tento snímek pak může být převedena na disk, který lze použít toorecreate hello virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="3af64-270">This snapshot can then be converted into a disk, which can be used toorecreate hello virtual machine.</span></span>

```azurecli-interactive 
az disk create --resource-group myResourceGroupDisk --name mySnapshotDisk --source osDisk-backup
```

### <a name="restore-virtual-machine-from-snapshot"></a><span data-ttu-id="3af64-271">Virtuální počítač obnovit ze snímku</span><span class="sxs-lookup"><span data-stu-id="3af64-271">Restore virtual machine from snapshot</span></span>

<span data-ttu-id="3af64-272">obnovení virtuálního počítače toodemonstrate, odstraňte hello existující virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="3af64-272">toodemonstrate virtual machine recovery, delete hello existing virtual machine.</span></span> 

```azurecli-interactive 
az vm delete --resource-group myResourceGroupDisk --name myVM
```

<span data-ttu-id="3af64-273">Vytvoření nového virtuálního počítače z disku hello snímku.</span><span class="sxs-lookup"><span data-stu-id="3af64-273">Create a new virtual machine from hello snapshot disk.</span></span>

```azurecli-interactive 
az vm create --resource-group myResourceGroupDisk --name myVM --attach-os-disk mySnapshotDisk --os-type linux
```

### <a name="reattach-data-disk"></a><span data-ttu-id="3af64-274">Připojte datový disk</span><span class="sxs-lookup"><span data-stu-id="3af64-274">Reattach data disk</span></span>

<span data-ttu-id="3af64-275">Všechny disky dat potřebovat toobe znovu připojit toohello virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="3af64-275">All data disks need toobe reattached toohello virtual machine.</span></span>

<span data-ttu-id="3af64-276">Nejprve najít název disku hello dat pomocí hello [seznam disků az](https://docs.microsoft.com/cli/azure/disk#list) příkaz.</span><span class="sxs-lookup"><span data-stu-id="3af64-276">First find hello data disk name using hello [az disk list](https://docs.microsoft.com/cli/azure/disk#list) command.</span></span> <span data-ttu-id="3af64-277">Tento příklad místech hello název disku hello do proměnné s názvem *datadisk*, která je použita v dalším kroku hello.</span><span class="sxs-lookup"><span data-stu-id="3af64-277">This example places hello name of hello disk in a variable named *datadisk*, which is used in hello next step.</span></span>

```azurecli-interactive 
datadisk=$(az disk list -g myResourceGroupDisk --query "[?contains(name,'myVM')].[name]" -o tsv)
```

<span data-ttu-id="3af64-278">Použití hello [připojit disk virtuálního počítače az](https://docs.microsoft.com/cli/azure/vm/disk#attach) příkaz tooattach hello disku.</span><span class="sxs-lookup"><span data-stu-id="3af64-278">Use hello [az vm disk attach](https://docs.microsoft.com/cli/azure/vm/disk#attach) command tooattach hello disk.</span></span>

```azurecli-interactive 
az vm disk attach –g myResourceGroupDisk –-vm-name myVM –-disk $datadisk
```

## <a name="next-steps"></a><span data-ttu-id="3af64-279">Další kroky</span><span class="sxs-lookup"><span data-stu-id="3af64-279">Next steps</span></span>

<span data-ttu-id="3af64-280">V tomto kurzu jste se dozvěděli o tématech disky virtuálních počítačů, jako:</span><span class="sxs-lookup"><span data-stu-id="3af64-280">In this tutorial, you learned about VM disks topics such as:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="3af64-281">Operační systém a dočasný disky</span><span class="sxs-lookup"><span data-stu-id="3af64-281">OS disks and temporary disks</span></span>
> * <span data-ttu-id="3af64-282">Datové disky</span><span class="sxs-lookup"><span data-stu-id="3af64-282">Data disks</span></span>
> * <span data-ttu-id="3af64-283">Standard a Premium disky</span><span class="sxs-lookup"><span data-stu-id="3af64-283">Standard and Premium disks</span></span>
> * <span data-ttu-id="3af64-284">Výkon disku</span><span class="sxs-lookup"><span data-stu-id="3af64-284">Disk performance</span></span>
> * <span data-ttu-id="3af64-285">Připojení a příprava datových disků</span><span class="sxs-lookup"><span data-stu-id="3af64-285">Attaching and preparing data disks</span></span>
> * <span data-ttu-id="3af64-286">Změna velikosti disků</span><span class="sxs-lookup"><span data-stu-id="3af64-286">Resizing disks</span></span>
> * <span data-ttu-id="3af64-287">Snímky disku</span><span class="sxs-lookup"><span data-stu-id="3af64-287">Disk snapshots</span></span>

<span data-ttu-id="3af64-288">Posunutí další kurz toolearn toohello o automatizaci konfigurace virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="3af64-288">Advance toohello next tutorial toolearn about automating VM configuration.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="3af64-289">Automatizace konfigurace virtuálních počítačů</span><span class="sxs-lookup"><span data-stu-id="3af64-289">Automate VM configuration</span></span>](./tutorial-automate-vm-deployment.md)
