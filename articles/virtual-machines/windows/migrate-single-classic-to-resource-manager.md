---
title: "aaaMigrate tooan Classic virtuálních počítačů ARM spravované disku virtuálního počítače | Microsoft Docs"
description: "Migrovat jeden virtuální počítač Azure z nasazení classic hello modelu tooManaged disky v modelu nasazení Resource Manager hello."
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
ms.openlocfilehash: d8c4b9431f5dd8a071fcbc2ee36581a33f76ba62
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="manually-migrate-a-classic-vm-tooa-new-arm-managed-disk-vm-from-hello-vhd"></a><span data-ttu-id="9ec60-103">Ruční migraci Classic virtuálních počítačů tooa nového ARM spravované disku virtuálního počítače z hello virtuálního pevného disku</span><span class="sxs-lookup"><span data-stu-id="9ec60-103">Manually migrate a Classic VM tooa new ARM Managed Disk VM from hello VHD</span></span> 


<span data-ttu-id="9ec60-104">Tato část pomůže vám toomigrate existující virtuální počítače Azure z modelu nasazení classic hello příliš[spravované disky](managed-disks-overview.md) v modelu nasazení Resource Manager hello.</span><span class="sxs-lookup"><span data-stu-id="9ec60-104">This section helps you toomigrate your existing Azure VMs from hello classic deployment model too[Managed Disks](managed-disks-overview.md) in hello Resource Manager deployment model.</span></span>


## <a name="plan-for-hello-migration-toomanaged-disks"></a><span data-ttu-id="9ec60-105">Plánování migrace hello tooManaged disky</span><span class="sxs-lookup"><span data-stu-id="9ec60-105">Plan for hello migration tooManaged Disks</span></span>

<span data-ttu-id="9ec60-106">Tato část vám pomůže toomake hello nejlepší rozhodnutí o typy virtuálního počítače a disku.</span><span class="sxs-lookup"><span data-stu-id="9ec60-106">This section helps you toomake hello best decision on VM and disk types.</span></span>


### <a name="location"></a><span data-ttu-id="9ec60-107">Umístění</span><span class="sxs-lookup"><span data-stu-id="9ec60-107">Location</span></span>

<span data-ttu-id="9ec60-108">Vyberte umístění, kde Azure spravované disky jsou dostupné.</span><span class="sxs-lookup"><span data-stu-id="9ec60-108">Pick a location where Azure Managed Disks are available.</span></span> <span data-ttu-id="9ec60-109">Při migraci disků spravovaných tooPremium, ujistěte se také, že úložiště Premium je k dispozici v hello oblasti, kde jsou plánování toomigrate k.</span><span class="sxs-lookup"><span data-stu-id="9ec60-109">If you are migrating tooPremium Managed Disks, also ensure that Premium storage is available in hello region where you are planning toomigrate to.</span></span> <span data-ttu-id="9ec60-110">V tématu [služeb Azure byRegion](https://azure.microsoft.com/regions/#services) aktuální informace o dostupných umístění.</span><span class="sxs-lookup"><span data-stu-id="9ec60-110">See [Azure Services byRegion](https://azure.microsoft.com/regions/#services) for up-to-date information on available locations.</span></span>

### <a name="vm-sizes"></a><span data-ttu-id="9ec60-111">Velikost virtuálních počítačů</span><span class="sxs-lookup"><span data-stu-id="9ec60-111">VM sizes</span></span>

<span data-ttu-id="9ec60-112">Při migraci disků spravovaných tooPremium, máte tooupdate hello velikost virtuálního počítače tooPremium hello podporující velikost úložiště k dispozici v hello oblasti, kde je umístěn virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="9ec60-112">If you are migrating tooPremium Managed Disks, you have tooupdate hello size of hello VM tooPremium Storage capable size available in hello region where VM is located.</span></span> <span data-ttu-id="9ec60-113">Zkontrolujte hello velikosti virtuálních počítačů, které jsou podporující Storage úrovně Premium.</span><span class="sxs-lookup"><span data-stu-id="9ec60-113">Review hello VM sizes that are Premium Storage capable.</span></span> <span data-ttu-id="9ec60-114">Specifikace velikosti virtuálního počítače Azure Hello jsou uvedeny v [velikosti virtuálních počítačů](sizes.md).</span><span class="sxs-lookup"><span data-stu-id="9ec60-114">hello Azure VM size specifications are listed in [Sizes for virtual machines](sizes.md).</span></span>
<span data-ttu-id="9ec60-115">Zkontrolujte hello výkonové charakteristiky virtuálních počítačů, které pracovat s Storage úrovně Premium a zvolte hello nejvhodnější velikost virtuálního počítače, který nejlépe vyhovuje vaší zatížení.</span><span class="sxs-lookup"><span data-stu-id="9ec60-115">Review hello performance characteristics of virtual machines that work with Premium Storage and choose hello most appropriate VM size that best suits your workload.</span></span> <span data-ttu-id="9ec60-116">Ujistěte se, zda je dostatečnou šířku pásma k dispozici na disku provozu hello toodrive virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="9ec60-116">Make sure that there is sufficient bandwidth available on your VM toodrive hello disk traffic.</span></span>

### <a name="disk-sizes"></a><span data-ttu-id="9ec60-117">Velikost disků</span><span class="sxs-lookup"><span data-stu-id="9ec60-117">Disk sizes</span></span>

<span data-ttu-id="9ec60-118">**Premium spravované disky**</span><span class="sxs-lookup"><span data-stu-id="9ec60-118">**Premium Managed Disks**</span></span>

<span data-ttu-id="9ec60-119">Existují sedm typy disků spravované premium, které lze použít s vaší virtuálních počítačů a každá z nich má konkrétní IOPs a propustnost omezení.</span><span class="sxs-lookup"><span data-stu-id="9ec60-119">There are seven types of premium Managed disks that can be used with your VM and each has specific IOPs and throughput limits.</span></span> <span data-ttu-id="9ec60-120">Při výběru hello Premium typ disku pro virtuální počítač podle potřeb hello vaší aplikace z hlediska kapacity, výkon, škálovatelnost a načte ve špičce, zvažte tyto limity.</span><span class="sxs-lookup"><span data-stu-id="9ec60-120">Consider these limits when choosing hello Premium disk type for your VM based on hello needs of your application in terms of capacity, performance, scalability, and peak loads.</span></span>

| <span data-ttu-id="9ec60-121">Disky typu Premium</span><span class="sxs-lookup"><span data-stu-id="9ec60-121">Premium Disks Type</span></span>  | <span data-ttu-id="9ec60-122">P4</span><span class="sxs-lookup"><span data-stu-id="9ec60-122">P4</span></span>    | <span data-ttu-id="9ec60-123">P6</span><span class="sxs-lookup"><span data-stu-id="9ec60-123">P6</span></span>    | <span data-ttu-id="9ec60-124">P10</span><span class="sxs-lookup"><span data-stu-id="9ec60-124">P10</span></span>   | <span data-ttu-id="9ec60-125">P20</span><span class="sxs-lookup"><span data-stu-id="9ec60-125">P20</span></span>   | <span data-ttu-id="9ec60-126">P30</span><span class="sxs-lookup"><span data-stu-id="9ec60-126">P30</span></span>   | <span data-ttu-id="9ec60-127">P40</span><span class="sxs-lookup"><span data-stu-id="9ec60-127">P40</span></span>   | <span data-ttu-id="9ec60-128">P50</span><span class="sxs-lookup"><span data-stu-id="9ec60-128">P50</span></span>   | 
|---------------------|-------|-------|-------|-------|-------|-------|-------|
| <span data-ttu-id="9ec60-129">Velikost disku</span><span class="sxs-lookup"><span data-stu-id="9ec60-129">Disk size</span></span>           | <span data-ttu-id="9ec60-130">128 GB</span><span class="sxs-lookup"><span data-stu-id="9ec60-130">128 GB</span></span>| <span data-ttu-id="9ec60-131">512 GB</span><span class="sxs-lookup"><span data-stu-id="9ec60-131">512 GB</span></span>| <span data-ttu-id="9ec60-132">128 GB</span><span class="sxs-lookup"><span data-stu-id="9ec60-132">128 GB</span></span>| <span data-ttu-id="9ec60-133">512 GB</span><span class="sxs-lookup"><span data-stu-id="9ec60-133">512 GB</span></span>            | <span data-ttu-id="9ec60-134">1024 GB (1 TB)</span><span class="sxs-lookup"><span data-stu-id="9ec60-134">1024 GB (1 TB)</span></span>    | <span data-ttu-id="9ec60-135">2 048 GB (2 TB)</span><span class="sxs-lookup"><span data-stu-id="9ec60-135">2048 GB (2 TB)</span></span>    | <span data-ttu-id="9ec60-136">4095 GB (4 TB)</span><span class="sxs-lookup"><span data-stu-id="9ec60-136">4095 GB (4 TB)</span></span>    | 
| <span data-ttu-id="9ec60-137">Vstupně-výstupní operace za sekundu / disk</span><span class="sxs-lookup"><span data-stu-id="9ec60-137">IOPS per disk</span></span>       | <span data-ttu-id="9ec60-138">120</span><span class="sxs-lookup"><span data-stu-id="9ec60-138">120</span></span>   | <span data-ttu-id="9ec60-139">240</span><span class="sxs-lookup"><span data-stu-id="9ec60-139">240</span></span>   | <span data-ttu-id="9ec60-140">500</span><span class="sxs-lookup"><span data-stu-id="9ec60-140">500</span></span>   | <span data-ttu-id="9ec60-141">2300</span><span class="sxs-lookup"><span data-stu-id="9ec60-141">2300</span></span>              | <span data-ttu-id="9ec60-142">5000</span><span class="sxs-lookup"><span data-stu-id="9ec60-142">5000</span></span>              | <span data-ttu-id="9ec60-143">7500</span><span class="sxs-lookup"><span data-stu-id="9ec60-143">7500</span></span>              | <span data-ttu-id="9ec60-144">7500</span><span class="sxs-lookup"><span data-stu-id="9ec60-144">7500</span></span>              | 
| <span data-ttu-id="9ec60-145">Propustnost / disk</span><span class="sxs-lookup"><span data-stu-id="9ec60-145">Throughput per disk</span></span> | <span data-ttu-id="9ec60-146">25 MB za sekundu</span><span class="sxs-lookup"><span data-stu-id="9ec60-146">25 MB per second</span></span>  | <span data-ttu-id="9ec60-147">50 MB za sekundu</span><span class="sxs-lookup"><span data-stu-id="9ec60-147">50 MB per second</span></span>  | <span data-ttu-id="9ec60-148">100 MB za sekundu</span><span class="sxs-lookup"><span data-stu-id="9ec60-148">100 MB per second</span></span> | <span data-ttu-id="9ec60-149">150 MB za sekundu</span><span class="sxs-lookup"><span data-stu-id="9ec60-149">150 MB per second</span></span> | <span data-ttu-id="9ec60-150">200 MB za sekundu</span><span class="sxs-lookup"><span data-stu-id="9ec60-150">200 MB per second</span></span> | <span data-ttu-id="9ec60-151">250 MB za sekundu</span><span class="sxs-lookup"><span data-stu-id="9ec60-151">250 MB per second</span></span> | <span data-ttu-id="9ec60-152">250 MB za sekundu</span><span class="sxs-lookup"><span data-stu-id="9ec60-152">250 MB per second</span></span> | 

<span data-ttu-id="9ec60-153">**Standardní disky spravované**</span><span class="sxs-lookup"><span data-stu-id="9ec60-153">**Standard Managed Disks**</span></span>

<span data-ttu-id="9ec60-154">Existují sedm typy spravované standardní disky, které lze použít s virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="9ec60-154">There are seven types of Standard Managed disks that can be used with your VM.</span></span> <span data-ttu-id="9ec60-155">Každý z nich mají různé kapacity ale mít stejné IOPS a omezení propustnosti.</span><span class="sxs-lookup"><span data-stu-id="9ec60-155">Each of them have different capacity but have same IOPS and throughput limits.</span></span> <span data-ttu-id="9ec60-156">Vyberte typ hello standardní spravovaných disků na základě potřeb kapacity hello vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="9ec60-156">Choose hello type of Standard Managed disks based on hello capacity needs of your application.</span></span>

| <span data-ttu-id="9ec60-157">Disk typu Standard</span><span class="sxs-lookup"><span data-stu-id="9ec60-157">Standard Disk Type</span></span>  | <span data-ttu-id="9ec60-158">S4</span><span class="sxs-lookup"><span data-stu-id="9ec60-158">S4</span></span>               | <span data-ttu-id="9ec60-159">S6</span><span class="sxs-lookup"><span data-stu-id="9ec60-159">S6</span></span>               | <span data-ttu-id="9ec60-160">S10</span><span class="sxs-lookup"><span data-stu-id="9ec60-160">S10</span></span>              | <span data-ttu-id="9ec60-161">S20</span><span class="sxs-lookup"><span data-stu-id="9ec60-161">S20</span></span>              | <span data-ttu-id="9ec60-162">S30</span><span class="sxs-lookup"><span data-stu-id="9ec60-162">S30</span></span>              | <span data-ttu-id="9ec60-163">S40</span><span class="sxs-lookup"><span data-stu-id="9ec60-163">S40</span></span>              | <span data-ttu-id="9ec60-164">S50</span><span class="sxs-lookup"><span data-stu-id="9ec60-164">S50</span></span>              | 
|---------------------|---------------------|---------------------|------------------|------------------|------------------|------------------|------------------| 
| <span data-ttu-id="9ec60-165">Velikost disku</span><span class="sxs-lookup"><span data-stu-id="9ec60-165">Disk size</span></span>           | <span data-ttu-id="9ec60-166">30 GB</span><span class="sxs-lookup"><span data-stu-id="9ec60-166">30 GB</span></span>            | <span data-ttu-id="9ec60-167">64 GB</span><span class="sxs-lookup"><span data-stu-id="9ec60-167">64 GB</span></span>            | <span data-ttu-id="9ec60-168">128 GB</span><span class="sxs-lookup"><span data-stu-id="9ec60-168">128 GB</span></span>           | <span data-ttu-id="9ec60-169">512 GB</span><span class="sxs-lookup"><span data-stu-id="9ec60-169">512 GB</span></span>           | <span data-ttu-id="9ec60-170">1024 GB (1 TB)</span><span class="sxs-lookup"><span data-stu-id="9ec60-170">1024 GB (1 TB)</span></span>   | <span data-ttu-id="9ec60-171">2 048 GB (2TB)</span><span class="sxs-lookup"><span data-stu-id="9ec60-171">2048 GB (2TB)</span></span>    | <span data-ttu-id="9ec60-172">4095 GB (4 TB)</span><span class="sxs-lookup"><span data-stu-id="9ec60-172">4095 GB (4 TB)</span></span>   | 
| <span data-ttu-id="9ec60-173">Vstupně-výstupní operace za sekundu / disk</span><span class="sxs-lookup"><span data-stu-id="9ec60-173">IOPS per disk</span></span>       | <span data-ttu-id="9ec60-174">500</span><span class="sxs-lookup"><span data-stu-id="9ec60-174">500</span></span>              | <span data-ttu-id="9ec60-175">500</span><span class="sxs-lookup"><span data-stu-id="9ec60-175">500</span></span>              | <span data-ttu-id="9ec60-176">500</span><span class="sxs-lookup"><span data-stu-id="9ec60-176">500</span></span>              | <span data-ttu-id="9ec60-177">500</span><span class="sxs-lookup"><span data-stu-id="9ec60-177">500</span></span>              | <span data-ttu-id="9ec60-178">500</span><span class="sxs-lookup"><span data-stu-id="9ec60-178">500</span></span>              | <span data-ttu-id="9ec60-179">500</span><span class="sxs-lookup"><span data-stu-id="9ec60-179">500</span></span>             | <span data-ttu-id="9ec60-180">500</span><span class="sxs-lookup"><span data-stu-id="9ec60-180">500</span></span>              | 
| <span data-ttu-id="9ec60-181">Propustnost / disk</span><span class="sxs-lookup"><span data-stu-id="9ec60-181">Throughput per disk</span></span> | <span data-ttu-id="9ec60-182">60 MB za sekundu</span><span class="sxs-lookup"><span data-stu-id="9ec60-182">60 MB per second</span></span> | <span data-ttu-id="9ec60-183">60 MB za sekundu</span><span class="sxs-lookup"><span data-stu-id="9ec60-183">60 MB per second</span></span> | <span data-ttu-id="9ec60-184">60 MB za sekundu</span><span class="sxs-lookup"><span data-stu-id="9ec60-184">60 MB per second</span></span> | <span data-ttu-id="9ec60-185">60 MB za sekundu</span><span class="sxs-lookup"><span data-stu-id="9ec60-185">60 MB per second</span></span> | <span data-ttu-id="9ec60-186">60 MB za sekundu</span><span class="sxs-lookup"><span data-stu-id="9ec60-186">60 MB per second</span></span> | <span data-ttu-id="9ec60-187">60 MB za sekundu</span><span class="sxs-lookup"><span data-stu-id="9ec60-187">60 MB per second</span></span> | <span data-ttu-id="9ec60-188">60 MB za sekundu</span><span class="sxs-lookup"><span data-stu-id="9ec60-188">60 MB per second</span></span> | 


### <a name="disk-caching-policy"></a><span data-ttu-id="9ec60-189">Zásady ukládání do mezipaměti na disku</span><span class="sxs-lookup"><span data-stu-id="9ec60-189">Disk caching policy</span></span> 

<span data-ttu-id="9ec60-190">**Premium spravované disky**</span><span class="sxs-lookup"><span data-stu-id="9ec60-190">**Premium Managed Disks**</span></span>

<span data-ttu-id="9ec60-191">Ve výchozím nastavení, je disk ukládání do mezipaměti zásad *jen pro čtení* pro všechny hello Premium datových disků, a *pro čtení a zápis* pro disk operačního systému Premium hello připojené toohello virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="9ec60-191">By default, disk caching policy is *Read-Only* for all hello Premium data disks, and *Read-Write* for hello Premium operating system disk attached toohello VM.</span></span> <span data-ttu-id="9ec60-192">Toto nastavení konfigurace se doporučuje tooachieve hello optimální výkon pro aplikace IOs.</span><span class="sxs-lookup"><span data-stu-id="9ec60-192">This configuration setting is recommended tooachieve hello optimal performance for your application’s IOs.</span></span> <span data-ttu-id="9ec60-193">Těžký zápisu nebo pouze pro zápis datové disky (například soubory protokolu serveru SQL Server) zakažte ukládání do mezipaměti disku, takže můžete dosáhnout lepší výkon aplikace.</span><span class="sxs-lookup"><span data-stu-id="9ec60-193">For write-heavy or write-only data disks (such as SQL Server log files), disable disk caching so that you can achieve better application performance.</span></span>

### <a name="pricing"></a><span data-ttu-id="9ec60-194">Ceny</span><span class="sxs-lookup"><span data-stu-id="9ec60-194">Pricing</span></span>

<span data-ttu-id="9ec60-195">Zkontrolujte hello [ceny pro spravované disky](https://azure.microsoft.com/en-us/pricing/details/managed-disks/).</span><span class="sxs-lookup"><span data-stu-id="9ec60-195">Review hello [pricing for Managed Disks](https://azure.microsoft.com/en-us/pricing/details/managed-disks/).</span></span> <span data-ttu-id="9ec60-196">Ceny disků spravované Premium je stejný jako hello nespravované prémiové disky.</span><span class="sxs-lookup"><span data-stu-id="9ec60-196">Pricing of Premium Managed Disks is same as hello Premium Unmanaged Disks.</span></span> <span data-ttu-id="9ec60-197">Ale ceny pro standardní disky spravované se liší od standardní nespravované disky.</span><span class="sxs-lookup"><span data-stu-id="9ec60-197">But pricing for Standard Managed Disks is different than Standard Unmanaged Disks.</span></span>


## <a name="checklist"></a><span data-ttu-id="9ec60-198">Kontrolní seznam</span><span class="sxs-lookup"><span data-stu-id="9ec60-198">Checklist</span></span>

1.  <span data-ttu-id="9ec60-199">Při migraci disků spravovaných tooPremium, zkontrolujte, zda že je k dispozici v hello oblasti, které provádíte migraci do.</span><span class="sxs-lookup"><span data-stu-id="9ec60-199">If you are migrating tooPremium Managed Disks, make sure it is available in hello region you are migrating to.</span></span>

2.  <span data-ttu-id="9ec60-200">Rozhodněte, hello nový virtuální počítač řad, které budete používat.</span><span class="sxs-lookup"><span data-stu-id="9ec60-200">Decide hello new VM series you will be using.</span></span> <span data-ttu-id="9ec60-201">Při migraci disků spravovaných tooPremium by mělo být schopný úložiště Premium.</span><span class="sxs-lookup"><span data-stu-id="9ec60-201">It should be a Premium Storage capable if you are migrating tooPremium Managed Disks.</span></span>

3.  <span data-ttu-id="9ec60-202">Rozhodněte, hello přesnou velikost virtuálního počítače, které budete používat, které jsou k dispozici v hello oblasti, které provádíte migraci do.</span><span class="sxs-lookup"><span data-stu-id="9ec60-202">Decide hello exact VM size you will use which are available in hello region you are migrating to.</span></span> <span data-ttu-id="9ec60-203">Velikost virtuálního počítače musí toobe dostatečně velké na to toosupport hello počet datových disků, které máte.</span><span class="sxs-lookup"><span data-stu-id="9ec60-203">VM size needs toobe large enough toosupport hello number of data disks you have.</span></span> <span data-ttu-id="9ec60-204">Například pokud máte čtyři datových disků, hello virtuálního počítače musí mít dva nebo víc jader.</span><span class="sxs-lookup"><span data-stu-id="9ec60-204">For example, if you have four data disks, hello VM must have two or more cores.</span></span> <span data-ttu-id="9ec60-205">Zvažte také výpočetní výkon, paměti a šířky pásma sítě musí.</span><span class="sxs-lookup"><span data-stu-id="9ec60-205">Also, consider processing power, memory and network bandwidth needs.</span></span>

4.  <span data-ttu-id="9ec60-206">Máte hello aktuální virtuální počítač podrobnosti o užitečný, včetně hello seznam disků a odpovídající BLOB VHD.</span><span class="sxs-lookup"><span data-stu-id="9ec60-206">Have hello current VM details handy, including hello list of disks and corresponding VHD blobs.</span></span>

<span data-ttu-id="9ec60-207">Připravte aplikaci výpadek.</span><span class="sxs-lookup"><span data-stu-id="9ec60-207">Prepare your application for downtime.</span></span> <span data-ttu-id="9ec60-208">toodo čisté migrace, máte toostop veškeré zpracování hello v aktuálním systému hello.</span><span class="sxs-lookup"><span data-stu-id="9ec60-208">toodo a clean migration, you have toostop all hello processing in hello current system.</span></span> <span data-ttu-id="9ec60-209">Pak můžete ho získat tooconsistent stavu, který je možné migrovat toohello nová platforma.</span><span class="sxs-lookup"><span data-stu-id="9ec60-209">Only then you can get it tooconsistent state which you can migrate toohello new platform.</span></span> <span data-ttu-id="9ec60-210">Doba trvání výpadku závisí na množství dat v hello disky toomigrate hello.</span><span class="sxs-lookup"><span data-stu-id="9ec60-210">Downtime duration depends on hello amount of data in hello disks toomigrate.</span></span>


## <a name="migrate-hello-vm"></a><span data-ttu-id="9ec60-211">Migrace hello virtuálních počítačů</span><span class="sxs-lookup"><span data-stu-id="9ec60-211">Migrate hello VM</span></span>

<span data-ttu-id="9ec60-212">Připravte aplikaci výpadek.</span><span class="sxs-lookup"><span data-stu-id="9ec60-212">Prepare your application for downtime.</span></span> <span data-ttu-id="9ec60-213">toodo čisté migrace, máte toostop veškeré zpracování hello v aktuálním systému hello.</span><span class="sxs-lookup"><span data-stu-id="9ec60-213">toodo a clean migration, you have toostop all hello processing in hello current system.</span></span> <span data-ttu-id="9ec60-214">Pak můžete ho získat tooconsistent stavu, který je možné migrovat toohello nová platforma.</span><span class="sxs-lookup"><span data-stu-id="9ec60-214">Only then you can get it tooconsistent state which you can migrate toohello new platform.</span></span> <span data-ttu-id="9ec60-215">Doba trvání výpadku závisí hello množství dat v toomigrate disky hello.</span><span class="sxs-lookup"><span data-stu-id="9ec60-215">Downtime duration depends hello amount of data in hello disks toomigrate.</span></span>


1.  <span data-ttu-id="9ec60-216">Nastavte nejprve, hello společné parametry:</span><span class="sxs-lookup"><span data-stu-id="9ec60-216">First, set hello common parameters:</span></span>

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

2.  <span data-ttu-id="9ec60-217">Vytvoření spravovaného disku operačního systému pomocí hello virtuálního pevného disku z hello classic virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="9ec60-217">Create a managed OS disk using hello VHD from hello classic VM.</span></span>

    <span data-ttu-id="9ec60-218">Ujistěte se, že máte zadaný identifikátor URI virtuálního pevného disku operačního systému toohello $osVhdUri parametr hello dokončení hello.</span><span class="sxs-lookup"><span data-stu-id="9ec60-218">Ensure that you have provided hello complete URI of hello OS VHD toohello $osVhdUri parameter.</span></span> <span data-ttu-id="9ec60-219">Zadejte také **- AccountType** jako **PremiumLRS** nebo **StandardLRS** na základě typu disků (Standard nebo Premium) při migraci do.</span><span class="sxs-lookup"><span data-stu-id="9ec60-219">Also, enter **-AccountType** as **PremiumLRS** or **StandardLRS** based on type of disks (Premium or Standard) you are migrating to.</span></span>

    ```powershell
    $osDisk = New-AzureRmDisk -DiskName $osDiskName -Disk (New-AzureRmDiskConfig '
    -AccountType PremiumLRS -Location $location -CreateOption Import -SourceUri $osVhdUri) '
    -ResourceGroupName $resourceGroupName
    ```

3.  <span data-ttu-id="9ec60-220">Připojte hello operačního systému disku toohello nového virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="9ec60-220">Attach hello OS disk toohello new VM.</span></span>

    ```powershell
    $VirtualMachine = New-AzureRmVMConfig -VMName $virtualMachineName -VMSize $virtualMachineSize
    $VirtualMachine = Set-AzureRmVMOSDisk -VM $VirtualMachine -ManagedDiskId $osDisk.Id '
    -StorageAccountType PremiumLRS -DiskSizeInGB 128 -CreateOption Attach -Windows
    ```

4.  <span data-ttu-id="9ec60-221">Vytvoření disku spravovaných dat ze souboru VHD hello dat a přidejte ji toohello nového virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="9ec60-221">Create a managed data disk from hello data VHD file and add it toohello new VM.</span></span>

    ```powershell
    $dataDisk1 = New-AzureRmDisk -DiskName $dataDiskName -Disk (New-AzureRmDiskConfig '
    -AccountType PremiumLRS -Location $location -CreationDataCreateOption Import '
    -SourceUri $dataVhdUri ) -ResourceGroupName $resourceGroupName
    
    $VirtualMachine = Add-AzureRmVMDataDisk -VM $VirtualMachine -Name $dataDiskName '
    -CreateOption Attach -ManagedDiskId $dataDisk1.Id -Lun 1
    ```

5.  <span data-ttu-id="9ec60-222">Vytvoření nového virtuálního počítače hello nastavením veřejné IP adresy, virtuální sítě a síťový adaptér.</span><span class="sxs-lookup"><span data-stu-id="9ec60-222">Create hello new VM by setting public IP, Virtual Network and NIC.</span></span>

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
><span data-ttu-id="9ec60-223">Mohou existovat další kroky potřebné toosupport aplikace, který je nesmí být zahrnuté v této příručce.</span><span class="sxs-lookup"><span data-stu-id="9ec60-223">There may be additional steps necessary toosupport your application that is not be covered by this guide.</span></span>
>
>

## <a name="next-steps"></a><span data-ttu-id="9ec60-224">Další kroky</span><span class="sxs-lookup"><span data-stu-id="9ec60-224">Next steps</span></span>

- <span data-ttu-id="9ec60-225">Připojte toohello virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="9ec60-225">Connect toohello virtual machine.</span></span> <span data-ttu-id="9ec60-226">Pokyny najdete v tématu [jak se tooconnect a protokol na tooan Azure virtuální počítač, systém Windows spuštěný](connect-logon.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="9ec60-226">For instructions, see [How tooconnect and log on tooan Azure virtual machine running Windows](connect-logon.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span>

