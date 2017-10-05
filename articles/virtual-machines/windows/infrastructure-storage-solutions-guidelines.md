---
title: "Řešení úložiště pro virtuální počítače Windows v Azure | Microsoft Docs"
description: "Další informace o klíčových návrhu a implementace pokyny pro nasazení řešení úložiště ve službách infrastruktury Azure."
documentationcenter: 
services: virtual-machines-windows
author: iainfoulds
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 4ceb7d32-7a0d-4004-b701-c2bbcaff6b0b
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 06/26/2017
ms.author: iainfou
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: ba0d66c5af771ebcb3ca8e6742e15a5506d8fd61
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="azure-storage-infrastructure-guidelines-for-windows-vms"></a><span data-ttu-id="b292b-103">Pokyny infrastruktury úložiště Azure pro virtuální počítače Windows</span><span class="sxs-lookup"><span data-stu-id="b292b-103">Azure storage infrastructure guidelines for Windows VMs</span></span>

[!INCLUDE [virtual-machines-windows-infrastructure-guidelines-intro](../../../includes/virtual-machines-windows-infrastructure-guidelines-intro.md)]

<span data-ttu-id="b292b-104">Tento článek se zaměřuje na rozumět potřebám úložiště a aspekty návrhu pro dosažení optimálního virtuální počítač (VM) výkonu.</span><span class="sxs-lookup"><span data-stu-id="b292b-104">This article focuses on understanding storage needs and design considerations for achieving optimum virtual machine (VM) performance.</span></span>

## <a name="implementation-guidelines-for-storage"></a><span data-ttu-id="b292b-105">Postup implementace pro úložiště</span><span class="sxs-lookup"><span data-stu-id="b292b-105">Implementation guidelines for storage</span></span>
<span data-ttu-id="b292b-106">Rozhodnutí:</span><span class="sxs-lookup"><span data-stu-id="b292b-106">Decisions:</span></span>

* <span data-ttu-id="b292b-107">Chystáte se používat Azure disky spravované nebo nespravované disky?</span><span class="sxs-lookup"><span data-stu-id="b292b-107">Are you going to use Azure Managed Disks or unmanaged disks?</span></span>
* <span data-ttu-id="b292b-108">Je třeba použít Standard nebo Premium úložiště pro úlohy?</span><span class="sxs-lookup"><span data-stu-id="b292b-108">Do you need to use Standard or Premium storage for your workload?</span></span>
* <span data-ttu-id="b292b-109">Je potřeba prokládání disků k vytvoření disky větší než 4TB?</span><span class="sxs-lookup"><span data-stu-id="b292b-109">Do you need disk striping to create disks larger than 4TB?</span></span>
* <span data-ttu-id="b292b-110">Je potřeba prokládání disků k dosažení optimálního výkonu vstupně-výstupních operací pro vaše úlohy?</span><span class="sxs-lookup"><span data-stu-id="b292b-110">Do you need disk striping to achieve optimal I/O performance for your workload?</span></span>
* <span data-ttu-id="b292b-111">Jaká sada účtů úložiště potřebujete k hostování zatížení IT nebo infrastrukturu?</span><span class="sxs-lookup"><span data-stu-id="b292b-111">What set of storage accounts do you need to host your IT workload or infrastructure?</span></span>

<span data-ttu-id="b292b-112">Úlohy:</span><span class="sxs-lookup"><span data-stu-id="b292b-112">Tasks:</span></span>

* <span data-ttu-id="b292b-113">Zkontrolujte požadavky na vstupně-výstupní operace aplikací, nasazení a naplánovat odpovídající počet a typ úložiště účtů.</span><span class="sxs-lookup"><span data-stu-id="b292b-113">Review I/O demands of the applications you are deploying and plan the appropriate number and type of storage accounts.</span></span>
* <span data-ttu-id="b292b-114">Vytvořte sadu účtů úložiště pomocí zásady vytváření názvů.</span><span class="sxs-lookup"><span data-stu-id="b292b-114">Create the set of storage accounts using your naming convention.</span></span> <span data-ttu-id="b292b-115">Můžete použít Azure PowerShell nebo portálu.</span><span class="sxs-lookup"><span data-stu-id="b292b-115">You can use Azure PowerShell or the portal.</span></span>

## <a name="storage"></a><span data-ttu-id="b292b-116">Úložiště</span><span class="sxs-lookup"><span data-stu-id="b292b-116">Storage</span></span>
<span data-ttu-id="b292b-117">Azure Storage je klíčovou součástí nasazení a správa virtuálních počítačů (VM) a aplikací.</span><span class="sxs-lookup"><span data-stu-id="b292b-117">Azure Storage is a key part of deploying and managing virtual machines (VMs) and applications.</span></span> <span data-ttu-id="b292b-118">Úložiště Azure poskytuje služby pro ukládání dat souboru, nestrukturovaných dat a zprávy, který je taky součástí infrastruktury podpora virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="b292b-118">Azure Storage provides services for storing file data, unstructured data, and messages, and it is also part of the infrastructure supporting VMs.</span></span>

<span data-ttu-id="b292b-119">[Disky systému Azure spravované](../../storage/storage-managed-disks-overview.md) zpracovává úložiště pro vás na pozadí.</span><span class="sxs-lookup"><span data-stu-id="b292b-119">[Azure Managed Disks](../../storage/storage-managed-disks-overview.md) handles storage for you behind the scenes.</span></span> <span data-ttu-id="b292b-120">S nespravované disky vytvořte účty úložiště pro uložení disky (soubory VHD) pro virtuální počítače Azure.</span><span class="sxs-lookup"><span data-stu-id="b292b-120">With unmanaged disks, you create storage accounts to hold the disks (VHD files) for your Azure VMs.</span></span> <span data-ttu-id="b292b-121">Při rozšiřování prostředků, musíte se ujistit, že kterou jste vytvořili další účty úložiště, takže nepřekračují limit IOPS pro úložiště s žádným z vaše disky.</span><span class="sxs-lookup"><span data-stu-id="b292b-121">When scaling up, you must make sure you created additional storage accounts so you don’t exceed the IOPS limit for storage with any of your disks.</span></span> <span data-ttu-id="b292b-122">S spravované disky zpracování úložiště, můžete se už není omezené podle limity účtu úložiště (například 20 000 IOPS / účet).</span><span class="sxs-lookup"><span data-stu-id="b292b-122">With Managed Disks handling storage, you are no longer limited by the storage account limits (such as 20,000 IOPS / account).</span></span> <span data-ttu-id="b292b-123">Také již budete muset zkopírovat vlastních bitových kopií (soubory VHD) k několika účtům úložiště.</span><span class="sxs-lookup"><span data-stu-id="b292b-123">You also no longer have to copy your custom images (VHD files) to multiple storage accounts.</span></span> <span data-ttu-id="b292b-124">Můžete spravovat v centrálním umístění – jeden účet úložiště podle oblasti Azure – a pomocí nich vytvořte stovky virtuálních počítačů v předplatném.</span><span class="sxs-lookup"><span data-stu-id="b292b-124">You can manage them in a central location – one storage account per Azure region – and use them to create hundreds of VMs in a subscription.</span></span> <span data-ttu-id="b292b-125">Doporučujeme že používat spravované disky pro nová nasazení.</span><span class="sxs-lookup"><span data-stu-id="b292b-125">We recommend you use Managed Disks for new deployments.</span></span>

<span data-ttu-id="b292b-126">K dispozici pro podporu virtuálních počítačů existují dva typy účtů úložiště:</span><span class="sxs-lookup"><span data-stu-id="b292b-126">There are two types of storage accounts available for supporting VMs:</span></span>

* <span data-ttu-id="b292b-127">Udělte účty úložiště Standard storage přístup do úložiště objektů blob (používá se pro ukládání virtuálních počítačů Azure disky), úložiště table, úložiště queue a úložiště file.</span><span class="sxs-lookup"><span data-stu-id="b292b-127">Standard storage accounts give you access to blob storage (used for storing Azure VM disks), table storage, queue storage, and file storage.</span></span>
* <span data-ttu-id="b292b-128">[Storage úrovně Premium](../../storage/storage-premium-storage.md) účty poskytovat podporu disků vysoce výkonné, nízká latence pro zatížení s intenzivním vstupně-výstupních operací, jako je například SQL servery v clusteru služby AlwaysOn.</span><span class="sxs-lookup"><span data-stu-id="b292b-128">[Premium storage](../../storage/storage-premium-storage.md) accounts deliver high-performance, low-latency disk support for I/O intensive workloads, such as SQL Servers in an AlwaysOn cluster.</span></span> <span data-ttu-id="b292b-129">Storage úrovně Premium aktuálně podporuje jen disky virtuálního počítače Azure.</span><span class="sxs-lookup"><span data-stu-id="b292b-129">Premium storage currently supports Azure VM disks only.</span></span>

<span data-ttu-id="b292b-130">Azure vytvoří virtuální počítače s diskem operačního systému, dočasným diskovým a nula nebo více disků volitelnými daty.</span><span class="sxs-lookup"><span data-stu-id="b292b-130">Azure creates VMs with an operating system disk, a temporary disk, and zero or more optional data disks.</span></span> <span data-ttu-id="b292b-131">Disk operačního systému a datové disky jsou objekty BLOB stránky Azure, zatímco dočasným diskovým uložených místně na uzlu, kde je umístěn na počítač.</span><span class="sxs-lookup"><span data-stu-id="b292b-131">The operating system disk and data disks are Azure page blobs, whereas the temporary disk is stored locally on the node where the machine lives.</span></span> <span data-ttu-id="b292b-132">Vezměte v potaz při návrhu aplikace použít pouze tento dočasný disk pro dočasnou data jako virtuální počítač může být migrovat mezi hostiteli během údržby.</span><span class="sxs-lookup"><span data-stu-id="b292b-132">Take care when designing applications to only use this temporary disk for non-persistent data as the VM may be migrated between hosts during a maintenance event.</span></span> <span data-ttu-id="b292b-133">Všechna data uložená na dočasném disku bude ztracena.</span><span class="sxs-lookup"><span data-stu-id="b292b-133">Any data stored on the temporary disk would be lost.</span></span>

<span data-ttu-id="b292b-134">Odolnost a vysoká dostupnost zajišťuje základní prostředí Azure Storage k zajištění, že vaše data i nadále chráněn proti neplánované údržby nebo k selhání hardwaru.</span><span class="sxs-lookup"><span data-stu-id="b292b-134">Durability and high availability is provided by the underlying Azure Storage environment to ensure that your data remains protected against unplanned maintenance or hardware failures.</span></span> <span data-ttu-id="b292b-135">Při navrhování prostředí Azure Storage, můžete replikovat úložiště virtuálního počítače:</span><span class="sxs-lookup"><span data-stu-id="b292b-135">As you design your Azure Storage environment, you can choose to replicate VM storage:</span></span>

* <span data-ttu-id="b292b-136">místně v rámci dané datové centrum Azure</span><span class="sxs-lookup"><span data-stu-id="b292b-136">locally within a given Azure datacenter</span></span>
* <span data-ttu-id="b292b-137">v datových centrech Azure v rámci dané oblasti</span><span class="sxs-lookup"><span data-stu-id="b292b-137">across Azure datacenters within a given region</span></span>
* <span data-ttu-id="b292b-138">v datových centrech v různých oblastech Azure</span><span class="sxs-lookup"><span data-stu-id="b292b-138">across Azure datacenters across different regions</span></span>

<span data-ttu-id="b292b-139">Můžete si přečíst [Další informace o možnostech replikace pro zajištění vysoké dostupnosti](../../storage/storage-introduction.md#replication-for-durability-and-high-availability).</span><span class="sxs-lookup"><span data-stu-id="b292b-139">You can read [more about the replication options for high availability](../../storage/storage-introduction.md#replication-for-durability-and-high-availability).</span></span>

<span data-ttu-id="b292b-140">Disky operačního systému a datové disky mají maximální velikost 4 TB.</span><span class="sxs-lookup"><span data-stu-id="b292b-140">Operating system disks and data disks have a maximum size of 4TB.</span></span> <span data-ttu-id="b292b-141">Prostory úložiště v systému Windows Server 2012 nebo novějším můžete překročí toto omezení pomocí datových disků do virtuálního počítače prezentovat logické svazky větší než 4TB sdružování společně.</span><span class="sxs-lookup"><span data-stu-id="b292b-141">You can use Storage Spaces in Windows Server 2012 or later to surpass this limit by pooling together data disks to present logical volumes larger than 4TB to your VM.</span></span>

<span data-ttu-id="b292b-142">Existují některá omezení škálovatelnosti při návrhu nasazení Azure Storage – Další informace najdete v tématu [limitů předplatného a služby Microsoft Azure, kvóty a omezení](../../azure-subscription-service-limits.md#storage-limits).</span><span class="sxs-lookup"><span data-stu-id="b292b-142">There are some scalability limits when designing your Azure Storage deployments - for more information, see [Microsoft Azure subscription and service limits, quotas, and constraints](../../azure-subscription-service-limits.md#storage-limits).</span></span> <span data-ttu-id="b292b-143">Viz také [úložiště Azure škálovatelnosti a cílech výkonnosti](../../storage/storage-scalability-targets.md).</span><span class="sxs-lookup"><span data-stu-id="b292b-143">Also see [Azure storage scalability and performance targets](../../storage/storage-scalability-targets.md).</span></span>

<span data-ttu-id="b292b-144">Pro uložení aplikace můžete uložit nestrukturovaných dat, jako jsou dokumenty, obrázky, zálohování, konfigurační data, protokoly, atd.</span><span class="sxs-lookup"><span data-stu-id="b292b-144">For application storage, you can store unstructured object data such as documents, images, backups, configuration data, logs, etc.</span></span> <span data-ttu-id="b292b-145">Pomocí úložiště objektů blob.</span><span class="sxs-lookup"><span data-stu-id="b292b-145">using blob storage.</span></span> <span data-ttu-id="b292b-146">Namísto aplikace zápis do virtuální disk připojený k virtuálnímu počítači může aplikace zapisovat přímo do Azure blob storage.</span><span class="sxs-lookup"><span data-stu-id="b292b-146">Rather than your application writing to a virtual disk attached to the VM, the application can write directly to Azure blob storage.</span></span> <span data-ttu-id="b292b-147">Úložiště objektů BLOB také poskytuje možnost [za provozu a cool vrstvy úložiště](../../storage/storage-blob-storage-tiers.md) v závislosti na dostupnosti požadavky a náklady na omezení.</span><span class="sxs-lookup"><span data-stu-id="b292b-147">Blob storage also provides the option of [hot and cool storage tiers](../../storage/storage-blob-storage-tiers.md) depending on your availability needs and cost constraints.</span></span>

## <a name="striped-disks"></a><span data-ttu-id="b292b-148">Prokládané disky</span><span class="sxs-lookup"><span data-stu-id="b292b-148">Striped disks</span></span>
<span data-ttu-id="b292b-149">Kromě toho umožňuje vytvářet disky větší než 4TB v mnoha případech pomocí proložení pro datové disky zlepšuje výkon tím, že několik objektů blob pro zálohování úložiště pro jeden svazek.</span><span class="sxs-lookup"><span data-stu-id="b292b-149">Besides allowing you to create disks larger than 4TB, in many instances, using striping for data disks enhances performance by allowing multiple blobs to back the storage for a single volume.</span></span> <span data-ttu-id="b292b-150">S proložení, vstupně-výstupních operací nutná k zápisu a čtení dat z jednoho logického disku pokračuje paralelně.</span><span class="sxs-lookup"><span data-stu-id="b292b-150">With striping, the I/O required to write and read data from a single logical disk proceeds in parallel.</span></span>

<span data-ttu-id="b292b-151">Systému Azure vynucuje omezení pro počet datových disků a šířku pásma, které jsou k dispozici, v závislosti na velikosti virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="b292b-151">Azure imposes limits on the number of data disks and amount of bandwidth available, depending on the VM size.</span></span> <span data-ttu-id="b292b-152">Podrobnosti najdete v tématu [velikosti virtuálních počítačů](sizes.md).</span><span class="sxs-lookup"><span data-stu-id="b292b-152">For details, see [Sizes for virtual machines](sizes.md).</span></span>

<span data-ttu-id="b292b-153">Pokud používáte disk proložení pro Azure datových disků, vezměte v úvahu následující pokyny:</span><span class="sxs-lookup"><span data-stu-id="b292b-153">If you are using disk striping for Azure data disks, consider the following guidelines:</span></span>

* <span data-ttu-id="b292b-154">Připojení maximální datových disků pro velikost virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="b292b-154">Attach the maximum data disks allowed for the VM size.</span></span>
* <span data-ttu-id="b292b-155">Prostory úložiště.</span><span class="sxs-lookup"><span data-stu-id="b292b-155">Use Storage Spaces.</span></span>
* <span data-ttu-id="b292b-156">Nepoužívejte možnosti ukládání do mezipaměti Azure datový disk (ukládání do mezipaměti zásad = None).</span><span class="sxs-lookup"><span data-stu-id="b292b-156">Avoid using Azure data disk caching options (caching policy = None).</span></span>

<span data-ttu-id="b292b-157">Další informace najdete v tématu [prostory úložiště – návrh a výkon](http://social.technet.microsoft.com/wiki/contents/articles/15200.storage-spaces-designing-for-performance.aspx).</span><span class="sxs-lookup"><span data-stu-id="b292b-157">For more information, see [Storage spaces - designing for performance](http://social.technet.microsoft.com/wiki/contents/articles/15200.storage-spaces-designing-for-performance.aspx).</span></span>

## <a name="multiple-storage-accounts"></a><span data-ttu-id="b292b-158">Více účtů úložiště</span><span class="sxs-lookup"><span data-stu-id="b292b-158">Multiple storage accounts</span></span>
<span data-ttu-id="b292b-159">Tato část se nevztahuje na [Azure spravované disky](../../storage/storage-managed-disks-overview.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json), protože nelze vytvořit samostatné úložiště účtů.</span><span class="sxs-lookup"><span data-stu-id="b292b-159">This section does not apply to [Azure Managed Disks](../../storage/storage-managed-disks-overview.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json), as you do not create separate storage accounts.</span></span> 

<span data-ttu-id="b292b-160">Když navrhujete prostředí Azure Storage pro nespravovaná disky, můžete použít více účtů úložiště jako počet virtuálních počítačů můžete nasadit zvyšuje.</span><span class="sxs-lookup"><span data-stu-id="b292b-160">When designing your Azure Storage environment for unmanaged disks, you can use multiple storage accounts as the number of VMs you deploy increases.</span></span> <span data-ttu-id="b292b-161">Tento přístup pomáhá distribuovat na vstupy/výstupy mezi základní infrastrukturu Azure Storage tak, aby udržení optimálního výkonu pro virtuální počítače a aplikace.</span><span class="sxs-lookup"><span data-stu-id="b292b-161">This approach helps distribute out the I/O across the underlying Azure Storage infrastructure to maintain optimum performance for your VMs and applications.</span></span> <span data-ttu-id="b292b-162">Při navrhování aplikace, které nasazujete, zvažte požadavky na vstupně-výstupních operací, které má každý virtuální počítač a vyrovnávat na těchto virtuálních počítačů mezi různými účty úložiště Azure.</span><span class="sxs-lookup"><span data-stu-id="b292b-162">As you design the applications that you are deploying, consider the I/O requirements each VM has and balance out those VMs across Azure Storage accounts.</span></span> <span data-ttu-id="b292b-163">Pokuste se vyhnout, všechny vysoké vstupně-výstupních operací náročných na jen jeden nebo dva účty úložiště virtuálních počítačů v seskupení.</span><span class="sxs-lookup"><span data-stu-id="b292b-163">Try to avoid grouping all the high I/O demanding VMs in to just one or two storage accounts.</span></span>

<span data-ttu-id="b292b-164">Další informace o možnostech vstupně-výstupních operací různé možnosti Azure Storage a některé doporučujeme maximální možné limity, najdete v části [úložiště Azure škálovatelnosti a cílech výkonnosti](../../storage/storage-scalability-targets.md).</span><span class="sxs-lookup"><span data-stu-id="b292b-164">For more information about the I/O capabilities of the different Azure Storage options and some recommend maximums, see [Azure storage scalability and performance targets](../../storage/storage-scalability-targets.md).</span></span>

## <a name="next-steps"></a><span data-ttu-id="b292b-165">Další kroky</span><span class="sxs-lookup"><span data-stu-id="b292b-165">Next steps</span></span>
[!INCLUDE [virtual-machines-windows-infrastructure-guidelines-next-steps](../../../includes/virtual-machines-windows-infrastructure-guidelines-next-steps.md)]
