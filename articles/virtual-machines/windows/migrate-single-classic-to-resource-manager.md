---
title: "Migrace klasické virtuální počítač do virtuálních počítačů spravovaných disků ARM | Microsoft Docs"
description: "Migrujte jeden virtuální počítač Azure z modelu nasazení classic k spravovaná diskům v modelu nasazení Resource Manager."
services: virtual-machines-windows
documentationcenter: 
author: cynthn
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 06/15/2017
ms.author: cynthn
ms.openlocfilehash: 82389834d85981c0ed71bdcc891fbfdbe1377654
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="manually-migrate-a-classic-vm-to-a-new-arm-managed-disk-vm-from-the-vhd"></a><span data-ttu-id="5565a-103">Ruční migraci Classic virtuálního počítače do nového ARM spravované disku virtuálního počítače z virtuálního pevného disku</span><span class="sxs-lookup"><span data-stu-id="5565a-103">Manually migrate a Classic VM to a new ARM Managed Disk VM from the VHD</span></span> 


<span data-ttu-id="5565a-104">V této části vám umožňuje migrovat existující virtuální počítače Azure z modelu nasazení classic do [spravované disky](managed-disks-overview.md) v modelu nasazení Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="5565a-104">This section helps you to migrate your existing Azure VMs from the classic deployment model to [Managed Disks](managed-disks-overview.md) in the Resource Manager deployment model.</span></span>


## <a name="plan-for-the-migration-to-managed-disks"></a><span data-ttu-id="5565a-105">Plánování migrace na spravované disky</span><span class="sxs-lookup"><span data-stu-id="5565a-105">Plan for the migration to Managed Disks</span></span>

<span data-ttu-id="5565a-106">V této části můžete vytvořit nejlepší rozhodnutí o typech virtuálního počítače a disku.</span><span class="sxs-lookup"><span data-stu-id="5565a-106">This section helps you to make the best decision on VM and disk types.</span></span>


### <a name="location"></a><span data-ttu-id="5565a-107">Umístění</span><span class="sxs-lookup"><span data-stu-id="5565a-107">Location</span></span>

<span data-ttu-id="5565a-108">Vyberte umístění, kde Azure spravované disky jsou dostupné.</span><span class="sxs-lookup"><span data-stu-id="5565a-108">Pick a location where Azure Managed Disks are available.</span></span> <span data-ttu-id="5565a-109">Při migraci disků spravovaných Premium také zajistíte, že úložiště Premium je k dispozici v oblasti, kde máte v úmyslu migrovat.</span><span class="sxs-lookup"><span data-stu-id="5565a-109">If you are migrating to Premium Managed Disks, also ensure that Premium storage is available in the region where you are planning to migrate to.</span></span> <span data-ttu-id="5565a-110">V tématu [služeb Azure byRegion](https://azure.microsoft.com/regions/#services) aktuální informace o dostupných umístění.</span><span class="sxs-lookup"><span data-stu-id="5565a-110">See [Azure Services byRegion](https://azure.microsoft.com/regions/#services) for up-to-date information on available locations.</span></span>

### <a name="vm-sizes"></a><span data-ttu-id="5565a-111">Velikost virtuálních počítačů</span><span class="sxs-lookup"><span data-stu-id="5565a-111">VM sizes</span></span>

<span data-ttu-id="5565a-112">Pokud provádíte migraci na disky spravované Premium, budete muset aktualizovat velikost virtuálního počítače do Storage úrovně Premium podporující velikost dostupné v oblasti, kde je umístěn virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="5565a-112">If you are migrating to Premium Managed Disks, you have to update the size of the VM to Premium Storage capable size available in the region where VM is located.</span></span> <span data-ttu-id="5565a-113">Zkontrolujte velikosti virtuálních počítačů, které jsou podporující Storage úrovně Premium.</span><span class="sxs-lookup"><span data-stu-id="5565a-113">Review the VM sizes that are Premium Storage capable.</span></span> <span data-ttu-id="5565a-114">Specifikace velikosti virtuálního počítače Azure, jsou uvedeny v [velikosti virtuálních počítačů](sizes.md).</span><span class="sxs-lookup"><span data-stu-id="5565a-114">The Azure VM size specifications are listed in [Sizes for virtual machines](sizes.md).</span></span>
<span data-ttu-id="5565a-115">Zkontrolujte vlastnosti výkonu virtuálních počítačů, které pracovat Storage úrovně Premium a vyberete nejvhodnější velikost virtuálního počítače, který nejlépe vyhovuje vaší zatížení.</span><span class="sxs-lookup"><span data-stu-id="5565a-115">Review the performance characteristics of virtual machines that work with Premium Storage and choose the most appropriate VM size that best suits your workload.</span></span> <span data-ttu-id="5565a-116">Ujistěte se, že je dostatečnou šířku pásma dostupné na vašem virtuálním počítači k řízení provozu disku.</span><span class="sxs-lookup"><span data-stu-id="5565a-116">Make sure that there is sufficient bandwidth available on your VM to drive the disk traffic.</span></span>

### <a name="disk-sizes"></a><span data-ttu-id="5565a-117">Velikost disků</span><span class="sxs-lookup"><span data-stu-id="5565a-117">Disk sizes</span></span>

<span data-ttu-id="5565a-118">**Premium spravované disky**</span><span class="sxs-lookup"><span data-stu-id="5565a-118">**Premium Managed Disks**</span></span>

<span data-ttu-id="5565a-119">Existují sedm typy disků spravované premium, které lze použít s vaší virtuálních počítačů a každá z nich má konkrétní IOPs a propustnost omezení.</span><span class="sxs-lookup"><span data-stu-id="5565a-119">There are seven types of premium Managed disks that can be used with your VM and each has specific IOPs and throughput limits.</span></span> <span data-ttu-id="5565a-120">Vezměte v úvahu tyto limity při výběru typu Premium disku pro virtuální počítač na základě potřeb vaší aplikace z hlediska kapacity, výkon, škálovatelnost a načte ve špičce.</span><span class="sxs-lookup"><span data-stu-id="5565a-120">Consider these limits when choosing the Premium disk type for your VM based on the needs of your application in terms of capacity, performance, scalability, and peak loads.</span></span>

| <span data-ttu-id="5565a-121">Disky typu Premium</span><span class="sxs-lookup"><span data-stu-id="5565a-121">Premium Disks Type</span></span>  | <span data-ttu-id="5565a-122">P4</span><span class="sxs-lookup"><span data-stu-id="5565a-122">P4</span></span>    | <span data-ttu-id="5565a-123">P6</span><span class="sxs-lookup"><span data-stu-id="5565a-123">P6</span></span>    | <span data-ttu-id="5565a-124">P10</span><span class="sxs-lookup"><span data-stu-id="5565a-124">P10</span></span>   | <span data-ttu-id="5565a-125">P20</span><span class="sxs-lookup"><span data-stu-id="5565a-125">P20</span></span>   | <span data-ttu-id="5565a-126">P30</span><span class="sxs-lookup"><span data-stu-id="5565a-126">P30</span></span>   | <span data-ttu-id="5565a-127">P40</span><span class="sxs-lookup"><span data-stu-id="5565a-127">P40</span></span>   | <span data-ttu-id="5565a-128">P50</span><span class="sxs-lookup"><span data-stu-id="5565a-128">P50</span></span>   | 
|---------------------|-------|-------|-------|-------|-------|-------|-------|
| <span data-ttu-id="5565a-129">Velikost disku</span><span class="sxs-lookup"><span data-stu-id="5565a-129">Disk size</span></span>           | <span data-ttu-id="5565a-130">128 GB</span><span class="sxs-lookup"><span data-stu-id="5565a-130">128 GB</span></span>| <span data-ttu-id="5565a-131">512 GB</span><span class="sxs-lookup"><span data-stu-id="5565a-131">512 GB</span></span>| <span data-ttu-id="5565a-132">128 GB</span><span class="sxs-lookup"><span data-stu-id="5565a-132">128 GB</span></span>| <span data-ttu-id="5565a-133">512 GB</span><span class="sxs-lookup"><span data-stu-id="5565a-133">512 GB</span></span>            | <span data-ttu-id="5565a-134">1024 GB (1 TB)</span><span class="sxs-lookup"><span data-stu-id="5565a-134">1024 GB (1 TB)</span></span>    | <span data-ttu-id="5565a-135">2 048 GB (2 TB)</span><span class="sxs-lookup"><span data-stu-id="5565a-135">2048 GB (2 TB)</span></span>    | <span data-ttu-id="5565a-136">4095 GB (4 TB)</span><span class="sxs-lookup"><span data-stu-id="5565a-136">4095 GB (4 TB)</span></span>    | 
| <span data-ttu-id="5565a-137">Vstupně-výstupní operace za sekundu / disk</span><span class="sxs-lookup"><span data-stu-id="5565a-137">IOPS per disk</span></span>       | <span data-ttu-id="5565a-138">120</span><span class="sxs-lookup"><span data-stu-id="5565a-138">120</span></span>   | <span data-ttu-id="5565a-139">240</span><span class="sxs-lookup"><span data-stu-id="5565a-139">240</span></span>   | <span data-ttu-id="5565a-140">500</span><span class="sxs-lookup"><span data-stu-id="5565a-140">500</span></span>   | <span data-ttu-id="5565a-141">2300</span><span class="sxs-lookup"><span data-stu-id="5565a-141">2300</span></span>              | <span data-ttu-id="5565a-142">5000</span><span class="sxs-lookup"><span data-stu-id="5565a-142">5000</span></span>              | <span data-ttu-id="5565a-143">7500</span><span class="sxs-lookup"><span data-stu-id="5565a-143">7500</span></span>              | <span data-ttu-id="5565a-144">7500</span><span class="sxs-lookup"><span data-stu-id="5565a-144">7500</span></span>              | 
| <span data-ttu-id="5565a-145">Propustnost / disk</span><span class="sxs-lookup"><span data-stu-id="5565a-145">Throughput per disk</span></span> | <span data-ttu-id="5565a-146">25 MB za sekundu</span><span class="sxs-lookup"><span data-stu-id="5565a-146">25 MB per second</span></span>  | <span data-ttu-id="5565a-147">50 MB za sekundu</span><span class="sxs-lookup"><span data-stu-id="5565a-147">50 MB per second</span></span>  | <span data-ttu-id="5565a-148">100 MB za sekundu</span><span class="sxs-lookup"><span data-stu-id="5565a-148">100 MB per second</span></span> | <span data-ttu-id="5565a-149">150 MB za sekundu</span><span class="sxs-lookup"><span data-stu-id="5565a-149">150 MB per second</span></span> | <span data-ttu-id="5565a-150">200 MB za sekundu</span><span class="sxs-lookup"><span data-stu-id="5565a-150">200 MB per second</span></span> | <span data-ttu-id="5565a-151">250 MB za sekundu</span><span class="sxs-lookup"><span data-stu-id="5565a-151">250 MB per second</span></span> | <span data-ttu-id="5565a-152">250 MB za sekundu</span><span class="sxs-lookup"><span data-stu-id="5565a-152">250 MB per second</span></span> | 

<span data-ttu-id="5565a-153">**Standardní disky spravované**</span><span class="sxs-lookup"><span data-stu-id="5565a-153">**Standard Managed Disks**</span></span>

<span data-ttu-id="5565a-154">Existují sedm typy spravované standardní disky, které lze použít s virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="5565a-154">There are seven types of Standard Managed disks that can be used with your VM.</span></span> <span data-ttu-id="5565a-155">Každý z nich mají různé kapacity ale mít stejné IOPS a omezení propustnosti.</span><span class="sxs-lookup"><span data-stu-id="5565a-155">Each of them have different capacity but have same IOPS and throughput limits.</span></span> <span data-ttu-id="5565a-156">Vyberte typ standardní spravovaných disků na základě potřeb kapacitu vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="5565a-156">Choose the type of Standard Managed disks based on the capacity needs of your application.</span></span>

| <span data-ttu-id="5565a-157">Disk typu Standard</span><span class="sxs-lookup"><span data-stu-id="5565a-157">Standard Disk Type</span></span>  | <span data-ttu-id="5565a-158">S4</span><span class="sxs-lookup"><span data-stu-id="5565a-158">S4</span></span>               | <span data-ttu-id="5565a-159">S6</span><span class="sxs-lookup"><span data-stu-id="5565a-159">S6</span></span>               | <span data-ttu-id="5565a-160">S10</span><span class="sxs-lookup"><span data-stu-id="5565a-160">S10</span></span>              | <span data-ttu-id="5565a-161">S20</span><span class="sxs-lookup"><span data-stu-id="5565a-161">S20</span></span>              | <span data-ttu-id="5565a-162">S30</span><span class="sxs-lookup"><span data-stu-id="5565a-162">S30</span></span>              | <span data-ttu-id="5565a-163">S40</span><span class="sxs-lookup"><span data-stu-id="5565a-163">S40</span></span>              | <span data-ttu-id="5565a-164">S50</span><span class="sxs-lookup"><span data-stu-id="5565a-164">S50</span></span>              | 
|---------------------|---------------------|---------------------|------------------|------------------|------------------|------------------|------------------| 
| <span data-ttu-id="5565a-165">Velikost disku</span><span class="sxs-lookup"><span data-stu-id="5565a-165">Disk size</span></span>           | <span data-ttu-id="5565a-166">30 GB</span><span class="sxs-lookup"><span data-stu-id="5565a-166">30 GB</span></span>            | <span data-ttu-id="5565a-167">64 GB</span><span class="sxs-lookup"><span data-stu-id="5565a-167">64 GB</span></span>            | <span data-ttu-id="5565a-168">128 GB</span><span class="sxs-lookup"><span data-stu-id="5565a-168">128 GB</span></span>           | <span data-ttu-id="5565a-169">512 GB</span><span class="sxs-lookup"><span data-stu-id="5565a-169">512 GB</span></span>           | <span data-ttu-id="5565a-170">1024 GB (1 TB)</span><span class="sxs-lookup"><span data-stu-id="5565a-170">1024 GB (1 TB)</span></span>   | <span data-ttu-id="5565a-171">2 048 GB (2TB)</span><span class="sxs-lookup"><span data-stu-id="5565a-171">2048 GB (2TB)</span></span>    | <span data-ttu-id="5565a-172">4095 GB (4 TB)</span><span class="sxs-lookup"><span data-stu-id="5565a-172">4095 GB (4 TB)</span></span>   | 
| <span data-ttu-id="5565a-173">Vstupně-výstupní operace za sekundu / disk</span><span class="sxs-lookup"><span data-stu-id="5565a-173">IOPS per disk</span></span>       | <span data-ttu-id="5565a-174">500</span><span class="sxs-lookup"><span data-stu-id="5565a-174">500</span></span>              | <span data-ttu-id="5565a-175">500</span><span class="sxs-lookup"><span data-stu-id="5565a-175">500</span></span>              | <span data-ttu-id="5565a-176">500</span><span class="sxs-lookup"><span data-stu-id="5565a-176">500</span></span>              | <span data-ttu-id="5565a-177">500</span><span class="sxs-lookup"><span data-stu-id="5565a-177">500</span></span>              | <span data-ttu-id="5565a-178">500</span><span class="sxs-lookup"><span data-stu-id="5565a-178">500</span></span>              | <span data-ttu-id="5565a-179">500</span><span class="sxs-lookup"><span data-stu-id="5565a-179">500</span></span>             | <span data-ttu-id="5565a-180">500</span><span class="sxs-lookup"><span data-stu-id="5565a-180">500</span></span>              | 
| <span data-ttu-id="5565a-181">Propustnost / disk</span><span class="sxs-lookup"><span data-stu-id="5565a-181">Throughput per disk</span></span> | <span data-ttu-id="5565a-182">60 MB za sekundu</span><span class="sxs-lookup"><span data-stu-id="5565a-182">60 MB per second</span></span> | <span data-ttu-id="5565a-183">60 MB za sekundu</span><span class="sxs-lookup"><span data-stu-id="5565a-183">60 MB per second</span></span> | <span data-ttu-id="5565a-184">60 MB za sekundu</span><span class="sxs-lookup"><span data-stu-id="5565a-184">60 MB per second</span></span> | <span data-ttu-id="5565a-185">60 MB za sekundu</span><span class="sxs-lookup"><span data-stu-id="5565a-185">60 MB per second</span></span> | <span data-ttu-id="5565a-186">60 MB za sekundu</span><span class="sxs-lookup"><span data-stu-id="5565a-186">60 MB per second</span></span> | <span data-ttu-id="5565a-187">60 MB za sekundu</span><span class="sxs-lookup"><span data-stu-id="5565a-187">60 MB per second</span></span> | <span data-ttu-id="5565a-188">60 MB za sekundu</span><span class="sxs-lookup"><span data-stu-id="5565a-188">60 MB per second</span></span> | 


### <a name="disk-caching-policy"></a><span data-ttu-id="5565a-189">Zásady ukládání do mezipaměti na disku</span><span class="sxs-lookup"><span data-stu-id="5565a-189">Disk caching policy</span></span> 

<span data-ttu-id="5565a-190">**Premium spravované disky**</span><span class="sxs-lookup"><span data-stu-id="5565a-190">**Premium Managed Disks**</span></span>

<span data-ttu-id="5565a-191">Ve výchozím nastavení, je disk ukládání do mezipaměti zásad *jen pro čtení* pro všechny Premium datových disků, a *pro čtení a zápis* pro disk operačního systému Premium připojen k virtuálnímu počítači.</span><span class="sxs-lookup"><span data-stu-id="5565a-191">By default, disk caching policy is *Read-Only* for all the Premium data disks, and *Read-Write* for the Premium operating system disk attached to the VM.</span></span> <span data-ttu-id="5565a-192">Toto nastavení konfigurace se doporučuje pro dosažení optimálního výkonu pro vaše aplikace IOs.</span><span class="sxs-lookup"><span data-stu-id="5565a-192">This configuration setting is recommended to achieve the optimal performance for your application’s IOs.</span></span> <span data-ttu-id="5565a-193">Těžký zápisu nebo pouze pro zápis datové disky (například soubory protokolu serveru SQL Server) zakažte ukládání do mezipaměti disku, takže můžete dosáhnout lepší výkon aplikace.</span><span class="sxs-lookup"><span data-stu-id="5565a-193">For write-heavy or write-only data disks (such as SQL Server log files), disable disk caching so that you can achieve better application performance.</span></span>

### <a name="pricing"></a><span data-ttu-id="5565a-194">Ceny</span><span class="sxs-lookup"><span data-stu-id="5565a-194">Pricing</span></span>

<span data-ttu-id="5565a-195">Zkontrolujte [ceny pro spravované disky](https://azure.microsoft.com/en-us/pricing/details/managed-disks/).</span><span class="sxs-lookup"><span data-stu-id="5565a-195">Review the [pricing for Managed Disks](https://azure.microsoft.com/en-us/pricing/details/managed-disks/).</span></span> <span data-ttu-id="5565a-196">Ceny disků spravované Premium je stejný jako nespravované prémiové disky.</span><span class="sxs-lookup"><span data-stu-id="5565a-196">Pricing of Premium Managed Disks is same as the Premium Unmanaged Disks.</span></span> <span data-ttu-id="5565a-197">Ale ceny pro standardní disky spravované se liší od standardní nespravované disky.</span><span class="sxs-lookup"><span data-stu-id="5565a-197">But pricing for Standard Managed Disks is different than Standard Unmanaged Disks.</span></span>


## <a name="checklist"></a><span data-ttu-id="5565a-198">Kontrolní seznam</span><span class="sxs-lookup"><span data-stu-id="5565a-198">Checklist</span></span>

1.  <span data-ttu-id="5565a-199">Pokud provádíte migraci na prémiové disky spravovat, zkontrolujte, zda že je k dispozici v oblast, kterou provádíte migraci do.</span><span class="sxs-lookup"><span data-stu-id="5565a-199">If you are migrating to Premium Managed Disks, make sure it is available in the region you are migrating to.</span></span>

2.  <span data-ttu-id="5565a-200">Rozhodněte, nové řadu virtuálních počítačů, kterou budete používat.</span><span class="sxs-lookup"><span data-stu-id="5565a-200">Decide the new VM series you will be using.</span></span> <span data-ttu-id="5565a-201">Pokud provádíte migraci na prémiové disky spravované by mělo být schopný úložiště Premium.</span><span class="sxs-lookup"><span data-stu-id="5565a-201">It should be a Premium Storage capable if you are migrating to Premium Managed Disks.</span></span>

3.  <span data-ttu-id="5565a-202">Rozhodněte, přesný velikost virtuálního počítače, které budete používat, které jsou k dispozici v oblast, kterou provádíte migraci do.</span><span class="sxs-lookup"><span data-stu-id="5565a-202">Decide the exact VM size you will use which are available in the region you are migrating to.</span></span> <span data-ttu-id="5565a-203">Velikost virtuálního počítače musí být dostatečně velký pro podporu většímu počtu datových disků, které máte.</span><span class="sxs-lookup"><span data-stu-id="5565a-203">VM size needs to be large enough to support the number of data disks you have.</span></span> <span data-ttu-id="5565a-204">Například pokud máte čtyři datové disky, virtuální počítač musí mít dva nebo víc jader.</span><span class="sxs-lookup"><span data-stu-id="5565a-204">For example, if you have four data disks, the VM must have two or more cores.</span></span> <span data-ttu-id="5565a-205">Zvažte také výpočetní výkon, paměti a šířky pásma sítě musí.</span><span class="sxs-lookup"><span data-stu-id="5565a-205">Also, consider processing power, memory and network bandwidth needs.</span></span>

4.  <span data-ttu-id="5565a-206">Máte aktuální podrobnosti o virtuálních počítačů, které jsou užitečné, včetně seznamu disků a odpovídající BLOB VHD.</span><span class="sxs-lookup"><span data-stu-id="5565a-206">Have the current VM details handy, including the list of disks and corresponding VHD blobs.</span></span>

<span data-ttu-id="5565a-207">Připravte aplikaci výpadek.</span><span class="sxs-lookup"><span data-stu-id="5565a-207">Prepare your application for downtime.</span></span> <span data-ttu-id="5565a-208">K provedení čisté migrace, budete muset zastavit veškeré zpracování v aktuálním systému.</span><span class="sxs-lookup"><span data-stu-id="5565a-208">To do a clean migration, you have to stop all the processing in the current system.</span></span> <span data-ttu-id="5565a-209">Pak můžete získat do konzistentního stavu, které můžete migrovat na nové platformě.</span><span class="sxs-lookup"><span data-stu-id="5565a-209">Only then you can get it to consistent state which you can migrate to the new platform.</span></span> <span data-ttu-id="5565a-210">Doba trvání výpadku závisí na množství dat na discích k migraci.</span><span class="sxs-lookup"><span data-stu-id="5565a-210">Downtime duration depends on the amount of data in the disks to migrate.</span></span>


## <a name="migrate-the-vm"></a><span data-ttu-id="5565a-211">Migraci virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="5565a-211">Migrate the VM</span></span>

<span data-ttu-id="5565a-212">Připravte aplikaci výpadek.</span><span class="sxs-lookup"><span data-stu-id="5565a-212">Prepare your application for downtime.</span></span> <span data-ttu-id="5565a-213">K provedení čisté migrace, budete muset zastavit veškeré zpracování v aktuálním systému.</span><span class="sxs-lookup"><span data-stu-id="5565a-213">To do a clean migration, you have to stop all the processing in the current system.</span></span> <span data-ttu-id="5565a-214">Pak můžete získat do konzistentního stavu, které můžete migrovat na nové platformě.</span><span class="sxs-lookup"><span data-stu-id="5565a-214">Only then you can get it to consistent state which you can migrate to the new platform.</span></span> <span data-ttu-id="5565a-215">Doba trvání výpadku závisí množství dat na discích k migraci.</span><span class="sxs-lookup"><span data-stu-id="5565a-215">Downtime duration depends the amount of data in the disks to migrate.</span></span>


1.  <span data-ttu-id="5565a-216">Nastavte nejprve, běžné parametry:</span><span class="sxs-lookup"><span data-stu-id="5565a-216">First, set the common parameters:</span></span>

    ```powershell
    $resourceGroupName = 'yourResourceGroupName'
    
    $location = 'your location' 
    
    $virtualNetworkName = 'yourExistingVirtualNetworkName'
    
    $virtualMachineName = 'yourVMName'
    
    $virtualMachineSize = 'Standard_DS3'
    
    $adminUserName = "youradminusername"
    
    $adminPassword = "yourpassword" | ConvertTo-SecureString -AsPlainText -Force
    
    $imageName = 'yourImageName'
    
    $osVhdUri = 'https://storageaccount.blob.core.windows.net/vhdcontainer/osdisk.vhd'
    
    $dataVhdUri = 'https://storageaccount.blob.core.windows.net/vhdcontainer/datadisk1.vhd'
    
    $dataDiskName = 'dataDisk1'
    ```

2.  <span data-ttu-id="5565a-217">Vytvoření spravovaného disku operačního systému pomocí virtuálního pevného disku z klasického virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="5565a-217">Create a managed OS disk using the VHD from the classic VM.</span></span>

    <span data-ttu-id="5565a-218">Zkontrolujte, zda jste zadali úplný identifikátor URI virtuálního pevného disku operačního systému na parametr $osVhdUri.</span><span class="sxs-lookup"><span data-stu-id="5565a-218">Ensure that you have provided the complete URI of the OS VHD to the $osVhdUri parameter.</span></span> <span data-ttu-id="5565a-219">Zadejte také **- AccountType** jako **PremiumLRS** nebo **StandardLRS** na základě typu disků (Standard nebo Premium) při migraci do.</span><span class="sxs-lookup"><span data-stu-id="5565a-219">Also, enter **-AccountType** as **PremiumLRS** or **StandardLRS** based on type of disks (Premium or Standard) you are migrating to.</span></span>

    ```powershell
    $osDisk = New-AzureRmDisk -DiskName $osDiskName -Disk (New-AzureRmDiskConfig '
    -AccountType PremiumLRS -Location $location -CreateOption Import -SourceUri $osVhdUri) '
    -ResourceGroupName $resourceGroupName
    ```

3.  <span data-ttu-id="5565a-220">Připojte disk operačního systému na nový virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="5565a-220">Attach the OS disk to the new VM.</span></span>

    ```powershell
    $VirtualMachine = New-AzureRmVMConfig -VMName $virtualMachineName -VMSize $virtualMachineSize
    $VirtualMachine = Set-AzureRmVMOSDisk -VM $VirtualMachine -ManagedDiskId $osDisk.Id '
    -StorageAccountType PremiumLRS -DiskSizeInGB 128 -CreateOption Attach -Windows
    ```

4.  <span data-ttu-id="5565a-221">Vytvoření disku spravovaná data z datového souboru virtuálního pevného disku a přidat jej do nového virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="5565a-221">Create a managed data disk from the data VHD file and add it to the new VM.</span></span>

    ```powershell
    $dataDisk1 = New-AzureRmDisk -DiskName $dataDiskName -Disk (New-AzureRmDiskConfig '
    -AccountType PremiumLRS -Location $location -CreationDataCreateOption Import '
    -SourceUri $dataVhdUri ) -ResourceGroupName $resourceGroupName
    
    $VirtualMachine = Add-AzureRmVMDataDisk -VM $VirtualMachine -Name $dataDiskName '
    -CreateOption Attach -ManagedDiskId $dataDisk1.Id -Lun 1
    ```

5.  <span data-ttu-id="5565a-222">Vytvoření nového virtuálního počítače pomocí nastavení veřejné IP adresy, virtuální sítě a síťový adaptér.</span><span class="sxs-lookup"><span data-stu-id="5565a-222">Create the new VM by setting public IP, Virtual Network and NIC.</span></span>

    ```powershell
    $publicIp = New-AzureRmPublicIpAddress -Name ($VirtualMachineName.ToLower()+'_ip') '
    -ResourceGroupName $resourceGroupName -Location $location -AllocationMethod Dynamic
    
    $vnet = Get-AzureRmVirtualNetwork -Name $virtualNetworkName -ResourceGroupName $resourceGroupName
    
    $nic = New-AzureRmNetworkInterface -Name ($VirtualMachineName.ToLower()+'_nic') '
    -ResourceGroupName $resourceGroupName -Location $location -SubnetId $vnet.Subnets[0].Id '
    -PublicIpAddressId $publicIp.Id
    
    $VirtualMachine = Add-AzureRmVMNetworkInterface -VM $VirtualMachine -Id $nic.Id
    
    New-AzureRmVM -VM $VirtualMachine -ResourceGroupName $resourceGroupName -Location $location
    ```

> [!NOTE]
><span data-ttu-id="5565a-223">Mohou existovat další kroky nezbytné pro podporu vaší aplikace, která je nesmí být zahrnuté v této příručce.</span><span class="sxs-lookup"><span data-stu-id="5565a-223">There may be additional steps necessary to support your application that is not be covered by this guide.</span></span>
>
>

## <a name="next-steps"></a><span data-ttu-id="5565a-224">Další kroky</span><span class="sxs-lookup"><span data-stu-id="5565a-224">Next steps</span></span>

- <span data-ttu-id="5565a-225">Připojte k virtuálnímu počítači.</span><span class="sxs-lookup"><span data-stu-id="5565a-225">Connect to the virtual machine.</span></span> <span data-ttu-id="5565a-226">Pokyny najdete v tématu [jak se připojit a přihlásit se na virtuálním počítači Azure s Windows](connect-logon.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="5565a-226">For instructions, see [How to connect and log on to an Azure virtual machine running Windows](connect-logon.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span>

