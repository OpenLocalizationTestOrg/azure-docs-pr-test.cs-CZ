---
title: "Migrace virtuálních počítačů do Azure Premium Storage | Microsoft Docs"
description: "Migrujte existující virtuální počítače na Azure Premium Storage. Premium Storage nabízí podporu vysoce výkonné, nízkou latencí disku pro I náročnými úlohy běžící na virtuálních počítačích Azure."
services: storage
documentationcenter: na
author: yuemlu
manager: tadb
editor: tysonn
ms.assetid: 272250b3-fd4e-41d2-8e34-fd8cc341ec87
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/27/2017
ms.author: yuemlu
ms.openlocfilehash: 645b37fff2dd6a12114719bac66a937cf8e8ffaa
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="migrating-to-azure-premium-storage-unmanaged-disks"></a><span data-ttu-id="e07a2-104">Migrace na Azure Premium Storage (nespravované disků)</span><span class="sxs-lookup"><span data-stu-id="e07a2-104">Migrating to Azure Premium Storage (Unmanaged Disks)</span></span>

> [!NOTE]
> <span data-ttu-id="e07a2-105">Tento článek popisuje, jak migrovat virtuální počítač, který používá nespravované standardní disky pro virtuální počítač, který používá nespravované prémiové disky.</span><span class="sxs-lookup"><span data-stu-id="e07a2-105">This article discusses how to migrate a VM that uses unmanaged standard disks to a VM that uses unmanaged premium disks.</span></span> <span data-ttu-id="e07a2-106">Doporučujeme použít Azure spravované disky pro nové virtuální počítače, a převést vaše předchozí disky nespravované na spravované disky.</span><span class="sxs-lookup"><span data-stu-id="e07a2-106">We recommend that you use Azure Managed Disks for new VMs, and that you convert your previous unmanaged disks to managed disks.</span></span> <span data-ttu-id="e07a2-107">Spravované disky popisovač základní účty úložiště, nemusíte.</span><span class="sxs-lookup"><span data-stu-id="e07a2-107">Managed Disks handle the underlying storage accounts so you don't have to.</span></span> <span data-ttu-id="e07a2-108">Další informace najdete v tématu naše [přehled disky spravované](storage-managed-disks-overview.md).</span><span class="sxs-lookup"><span data-stu-id="e07a2-108">For more information, please see our [Managed Disks Overview](storage-managed-disks-overview.md).</span></span>
>

<span data-ttu-id="e07a2-109">Azure Premium Storage nabízí podporu vysoce výkonné, nízkou latencí disku pro virtuální počítače spuštěné I náročnými úlohy.</span><span class="sxs-lookup"><span data-stu-id="e07a2-109">Azure Premium Storage delivers high-performance, low-latency disk support for virtual machines running I/O-intensive workloads.</span></span> <span data-ttu-id="e07a2-110">Pomocí migrace vaší aplikace disky virtuálních počítačů do Azure Premium Storage můžete využít výhod rychlosti a výkonu z těchto disků.</span><span class="sxs-lookup"><span data-stu-id="e07a2-110">You can take advantage of the speed and performance of these disks by migrating your application's VM disks to Azure Premium Storage.</span></span>

<span data-ttu-id="e07a2-111">Účel tohoto průvodce je pomoct noví uživatelé Azure Premium Storage lepší Příprava na s hladkým přechodem z jejich aktuálního systému do úložiště úrovně Premium.</span><span class="sxs-lookup"><span data-stu-id="e07a2-111">The purpose of this guide is to help new users of Azure Premium Storage better prepare to make a smooth transition from their current system to Premium Storage.</span></span> <span data-ttu-id="e07a2-112">Průvodce řeší tři klíčové komponenty v tomto procesu:</span><span class="sxs-lookup"><span data-stu-id="e07a2-112">The guide addresses three of the key components in this process:</span></span>

* [<span data-ttu-id="e07a2-113">Plánování migrace na Storage úrovně Premium</span><span class="sxs-lookup"><span data-stu-id="e07a2-113">Plan for the Migration to Premium Storage</span></span>](#plan-the-migration-to-premium-storage)
* [<span data-ttu-id="e07a2-114">Příprava a zkopírovat virtuální pevné disky (VHD) do úložiště úrovně Premium</span><span class="sxs-lookup"><span data-stu-id="e07a2-114">Prepare and Copy Virtual Hard Disks (VHDs) to Premium Storage</span></span>](#prepare-and-copy-virtual-hard-disks-VHDs-to-premium-storage)
* [<span data-ttu-id="e07a2-115">Vytvoření virtuálního počítače Azure pomocí služby Storage úrovně Premium</span><span class="sxs-lookup"><span data-stu-id="e07a2-115">Create Azure Virtual Machine using Premium Storage</span></span>](#create-azure-virtual-machine-using-premium-storage)

<span data-ttu-id="e07a2-116">Můžete migrovat virtuální počítače z jiné platformy Azure Premium Storage nebo migrovat existující virtuální počítače Azure ze standardního úložiště do úložiště úrovně Premium.</span><span class="sxs-lookup"><span data-stu-id="e07a2-116">You can either migrate VMs from other platforms to Azure Premium Storage or migrate existing Azure VMs from Standard Storage to Premium Storage.</span></span> <span data-ttu-id="e07a2-117">Tento průvodce popisuje kroky pro oba dva scénáře.</span><span class="sxs-lookup"><span data-stu-id="e07a2-117">This guide covers steps for both two scenarios.</span></span> <span data-ttu-id="e07a2-118">Postupujte podle kroků v příslušné části v závislosti na vašem scénáři zadaný.</span><span class="sxs-lookup"><span data-stu-id="e07a2-118">Follow the steps specified in the relevant section depending on your scenario.</span></span>

> [!NOTE]
> <span data-ttu-id="e07a2-119">Najdete přehled funkcí a cen. úložiště Premium Storage v úložiště Premium: [vysoce výkonné úložiště pro úlohy virtuálních počítačů Azure](storage-premium-storage.md).</span><span class="sxs-lookup"><span data-stu-id="e07a2-119">You can find a feature overview and pricing of Premium Storage in Premium Storage: [High-Performance Storage for Azure Virtual Machine Workloads](storage-premium-storage.md).</span></span> <span data-ttu-id="e07a2-120">Doporučujeme migraci všech disku virtuálního počítače vyžadující vysokou IOPS na Azure Premium Storage k dosažení nejlepšího výkonu pro vaši aplikaci.</span><span class="sxs-lookup"><span data-stu-id="e07a2-120">We recommend migrating any virtual machine disk requiring high IOPS to Azure Premium Storage for the best performance for your application.</span></span> <span data-ttu-id="e07a2-121">Pokud disk nevyžadují vysokou IOPS, můžete omezit náklady na údržbu ve standardní úložiště, která ukládá data disku virtuálního počítače na jednotkách pevných disků (HDD) namísto SSD.</span><span class="sxs-lookup"><span data-stu-id="e07a2-121">If your disk does not require high IOPS, you can limit costs by maintaining it in Standard Storage, which stores virtual machine disk data on Hard Disk Drives (HDDs) instead of SSDs.</span></span>
>

<span data-ttu-id="e07a2-122">Dokončení procesu migrace v celé jeho šíři může vyžadovat další akce před i po podle kroků uvedených v této příručce.</span><span class="sxs-lookup"><span data-stu-id="e07a2-122">Completing the migration process in its entirety may require additional actions both before and after the steps provided in this guide.</span></span> <span data-ttu-id="e07a2-123">Mezi příklady patří konfiguraci virtuální sítě nebo koncových bodů nebo provádění změn kódu v aplikaci sám sebe, což může vyžadovat výpadky v aplikaci.</span><span class="sxs-lookup"><span data-stu-id="e07a2-123">Examples include configuring virtual networks or endpoints or making code changes within the application itself which may require some downtime in your application.</span></span> <span data-ttu-id="e07a2-124">Tyto akce jsou jedinečné pro každou aplikaci a měli byste pokračovat společně s podle kroků uvedených v této příručce na přechod úplné do úložiště úrovně Premium jako bezproblémové nejblíže.</span><span class="sxs-lookup"><span data-stu-id="e07a2-124">These actions are unique to each application, and you should complete them along with the steps provided in this guide to make the full transition to Premium Storage as seamless as possible.</span></span>

## <span data-ttu-id="e07a2-125"><a name="plan-the-migration-to-premium-storage"></a>Plánování migrace na Storage úrovně Premium</span><span class="sxs-lookup"><span data-stu-id="e07a2-125"><a name="plan-the-migration-to-premium-storage"></a>Plan for the migration to Premium Storage</span></span>
<span data-ttu-id="e07a2-126">Tato část zajistí připravení postupujte podle kroků migrace v tomto článku a může být nejlepší rozhodnutí o typech virtuálního počítače a disku.</span><span class="sxs-lookup"><span data-stu-id="e07a2-126">This section ensures that you are ready to follow the migration steps in this article, and helps you to make the best decision on VM and Disk types.</span></span>

### <a name="prerequisites"></a><span data-ttu-id="e07a2-127">Požadavky</span><span class="sxs-lookup"><span data-stu-id="e07a2-127">Prerequisites</span></span>
* <span data-ttu-id="e07a2-128">Budete potřebovat předplatné Azure.</span><span class="sxs-lookup"><span data-stu-id="e07a2-128">You will need an Azure subscription.</span></span> <span data-ttu-id="e07a2-129">Pokud nemáte, můžete vytvořit jeden měsíc [bezplatnou zkušební verzi](https://azure.microsoft.com/pricing/free-trial/) předplatné nebo navštivte [Azure – ceny](https://azure.microsoft.com/pricing/) další možnosti.</span><span class="sxs-lookup"><span data-stu-id="e07a2-129">If you don't have one, you can create a one-month [free trial](https://azure.microsoft.com/pricing/free-trial/) subscription or visit [Azure Pricing](https://azure.microsoft.com/pricing/) for more options.</span></span>
* <span data-ttu-id="e07a2-130">Spuštění rutiny prostředí PowerShell, je nutné modul Microsoft Azure PowerShell.</span><span class="sxs-lookup"><span data-stu-id="e07a2-130">To execute PowerShell cmdlets, you will need the Microsoft Azure PowerShell module.</span></span> <span data-ttu-id="e07a2-131">Bod instalace a pokyny pro instalaci jsou popsané v tématu [Jak nainstalovat a nakonfigurovat Azure PowerShell](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="e07a2-131">See [How to install and configure Azure PowerShell](/powershell/azure/overview) for the install point and installation instructions.</span></span>
* <span data-ttu-id="e07a2-132">Když budete chtít použít Azure virtuálních počítačů spuštěných na Storage úrovně Premium, budete muset použít podporující virtuálních počítačů služby Storage úrovně Premium.</span><span class="sxs-lookup"><span data-stu-id="e07a2-132">When you plan to use Azure VMs running on Premium Storage, you need to use the Premium Storage capable VMs.</span></span> <span data-ttu-id="e07a2-133">Standard a Premium Storage disků můžete použít s podporou virtuálních počítačů služby Storage úrovně Premium.</span><span class="sxs-lookup"><span data-stu-id="e07a2-133">You can use both Standard and Premium Storage disks with Premium Storage capable VMs.</span></span> <span data-ttu-id="e07a2-134">V budoucnu bude k dispozici s více typy virtuálních počítačů disky úložiště Premium.</span><span class="sxs-lookup"><span data-stu-id="e07a2-134">Premium storage disks will be available with more VM types in the future.</span></span> <span data-ttu-id="e07a2-135">Další informace o všech dostupných typů disku virtuálního počítače Azure a velikosti najdete v tématu [velikosti virtuálních počítačů](../virtual-machines/windows/sizes.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) a [velikosti pro cloudové služby](../cloud-services/cloud-services-sizes-specs.md).</span><span class="sxs-lookup"><span data-stu-id="e07a2-135">For more information on all available Azure VM disk types and sizes, see [Sizes for virtual machines](../virtual-machines/windows/sizes.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) and [Sizes for Cloud Services](../cloud-services/cloud-services-sizes-specs.md).</span></span>

### <a name="considerations"></a><span data-ttu-id="e07a2-136">Požadavky</span><span class="sxs-lookup"><span data-stu-id="e07a2-136">Considerations</span></span>
<span data-ttu-id="e07a2-137">Virtuální počítač Azure podporuje připojení několik disků úložiště Premium tak, aby vaše aplikace může mít až 256 TB úložiště na virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="e07a2-137">An Azure VM supports attaching several Premium Storage disks so that your applications can have up to 256 TB of storage per VM.</span></span> <span data-ttu-id="e07a2-138">Storage úrovně Premium můžete aplikace dosáhnout 80 000 IOPS (vstupně výstupních operací za sekundu) na virtuální počítač a 2000 MB za druhé propustnost disku na virtuální počítač s velmi nízkou latenci pro operace čtení.</span><span class="sxs-lookup"><span data-stu-id="e07a2-138">With Premium Storage, your applications can achieve 80,000 IOPS (input/output operations per second) per VM and 2000 MB per second disk throughput per VM with extremely low latencies for read operations.</span></span> <span data-ttu-id="e07a2-139">Máte možností v různých virtuálních počítačů a disků.</span><span class="sxs-lookup"><span data-stu-id="e07a2-139">You have options in a variety of VMs and Disks.</span></span> <span data-ttu-id="e07a2-140">Tato část se můžete k vyhledání možnost, která nejlépe vyhovuje vaší zatížení.</span><span class="sxs-lookup"><span data-stu-id="e07a2-140">This section is to help you to find an option that best suits your workload.</span></span>

#### <a name="vm-sizes"></a><span data-ttu-id="e07a2-141">Velikost virtuálních počítačů</span><span class="sxs-lookup"><span data-stu-id="e07a2-141">VM sizes</span></span>
<span data-ttu-id="e07a2-142">Specifikace velikosti virtuálního počítače Azure, jsou uvedeny v [velikosti virtuálních počítačů](../virtual-machines/windows/sizes.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="e07a2-142">The Azure VM size specifications are listed in [Sizes for virtual machines](../virtual-machines/windows/sizes.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span> <span data-ttu-id="e07a2-143">Zkontrolujte vlastnosti výkonu virtuálních počítačů, které pracovat Storage úrovně Premium a vyberete nejvhodnější velikost virtuálního počítače, který nejlépe vyhovuje vaší zatížení.</span><span class="sxs-lookup"><span data-stu-id="e07a2-143">Review the performance characteristics of virtual machines that work with Premium Storage and choose the most appropriate VM size that best suits your workload.</span></span> <span data-ttu-id="e07a2-144">Ujistěte se, že je dostatečnou šířku pásma dostupné na vašem virtuálním počítači k řízení provozu disku.</span><span class="sxs-lookup"><span data-stu-id="e07a2-144">Make sure that there is sufficient bandwidth available on your VM to drive the disk traffic.</span></span>

#### <a name="disk-sizes"></a><span data-ttu-id="e07a2-145">Velikost disků</span><span class="sxs-lookup"><span data-stu-id="e07a2-145">Disk sizes</span></span>
<span data-ttu-id="e07a2-146">Existuje pět typů disků, které lze použít s vaší virtuálních počítačů a každá z nich má konkrétní IOPs a propustnost omezení.</span><span class="sxs-lookup"><span data-stu-id="e07a2-146">There are five types of disks that can be used with your VM and each has specific IOPs and throughput limits.</span></span> <span data-ttu-id="e07a2-147">Vzít v úvahu tyto limity, když vyberete typ disku pro virtuální počítač podle potřeb vaší aplikace z hlediska kapacity, výkon, škálovatelnost a načte ve špičce.</span><span class="sxs-lookup"><span data-stu-id="e07a2-147">Take into consideration these limits when choosing the disk type for your VM based on the needs of your application in terms of capacity, performance, scalability, and peak loads.</span></span>

| <span data-ttu-id="e07a2-148">Disky typu Premium</span><span class="sxs-lookup"><span data-stu-id="e07a2-148">Premium Disks Type</span></span>  | <span data-ttu-id="e07a2-149">P10</span><span class="sxs-lookup"><span data-stu-id="e07a2-149">P10</span></span>   | <span data-ttu-id="e07a2-150">P20</span><span class="sxs-lookup"><span data-stu-id="e07a2-150">P20</span></span>   | <span data-ttu-id="e07a2-151">P30</span><span class="sxs-lookup"><span data-stu-id="e07a2-151">P30</span></span>            | <span data-ttu-id="e07a2-152">P40</span><span class="sxs-lookup"><span data-stu-id="e07a2-152">P40</span></span>            | <span data-ttu-id="e07a2-153">P50</span><span class="sxs-lookup"><span data-stu-id="e07a2-153">P50</span></span>            | 
|:-------------------:|:-----:|:-----:|:--------------:|:--------------:|:--------------:|
| <span data-ttu-id="e07a2-154">Velikost disku</span><span class="sxs-lookup"><span data-stu-id="e07a2-154">Disk size</span></span>           | <span data-ttu-id="e07a2-155">128 GB</span><span class="sxs-lookup"><span data-stu-id="e07a2-155">128 GB</span></span>| <span data-ttu-id="e07a2-156">512 GB</span><span class="sxs-lookup"><span data-stu-id="e07a2-156">512 GB</span></span>| <span data-ttu-id="e07a2-157">1024 GB (1 TB)</span><span class="sxs-lookup"><span data-stu-id="e07a2-157">1024 GB (1 TB)</span></span> | <span data-ttu-id="e07a2-158">2 048 GB (2 TB)</span><span class="sxs-lookup"><span data-stu-id="e07a2-158">2048 GB (2 TB)</span></span> | <span data-ttu-id="e07a2-159">4095 GB (4 TB)</span><span class="sxs-lookup"><span data-stu-id="e07a2-159">4095 GB (4 TB)</span></span> | 
| <span data-ttu-id="e07a2-160">Vstupně-výstupní operace za sekundu / disk</span><span class="sxs-lookup"><span data-stu-id="e07a2-160">IOPS per disk</span></span>       | <span data-ttu-id="e07a2-161">500</span><span class="sxs-lookup"><span data-stu-id="e07a2-161">500</span></span>   | <span data-ttu-id="e07a2-162">2300</span><span class="sxs-lookup"><span data-stu-id="e07a2-162">2300</span></span>  | <span data-ttu-id="e07a2-163">5000</span><span class="sxs-lookup"><span data-stu-id="e07a2-163">5000</span></span>           | <span data-ttu-id="e07a2-164">7500</span><span class="sxs-lookup"><span data-stu-id="e07a2-164">7500</span></span>           | <span data-ttu-id="e07a2-165">7500</span><span class="sxs-lookup"><span data-stu-id="e07a2-165">7500</span></span>           | 
| <span data-ttu-id="e07a2-166">Propustnost / disk</span><span class="sxs-lookup"><span data-stu-id="e07a2-166">Throughput per disk</span></span> | <span data-ttu-id="e07a2-167">100 MB za sekundu</span><span class="sxs-lookup"><span data-stu-id="e07a2-167">100 MB per second</span></span> | <span data-ttu-id="e07a2-168">150 MB za sekundu</span><span class="sxs-lookup"><span data-stu-id="e07a2-168">150 MB per second</span></span> | <span data-ttu-id="e07a2-169">200 MB za sekundu</span><span class="sxs-lookup"><span data-stu-id="e07a2-169">200 MB per second</span></span> | <span data-ttu-id="e07a2-170">250 MB za sekundu</span><span class="sxs-lookup"><span data-stu-id="e07a2-170">250 MB per second</span></span> | <span data-ttu-id="e07a2-171">250 MB za sekundu</span><span class="sxs-lookup"><span data-stu-id="e07a2-171">250 MB per second</span></span> |

<span data-ttu-id="e07a2-172">V závislosti na velikosti pracovní zátěže určí, jestli dalších datových disků jsou nezbytné pro virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="e07a2-172">Depending on your workload, determine if additional data disks are necessary for your VM.</span></span> <span data-ttu-id="e07a2-173">Několik trvalých datových disků můžete připojit k virtuálnímu počítači.</span><span class="sxs-lookup"><span data-stu-id="e07a2-173">You can attach several persistent data disks to your VM.</span></span> <span data-ttu-id="e07a2-174">V případě potřeby můžete rozkládají mezi disky tím zvýšit kapacitu a výkon svazku.</span><span class="sxs-lookup"><span data-stu-id="e07a2-174">If needed, you can stripe across the disks to increase the capacity and performance of the volume.</span></span> <span data-ttu-id="e07a2-175">(Zjistíte novinky prokládání disků [zde](storage-premium-storage-performance.md#disk-striping).) Pokud rozkládají Storage úrovně Premium datových disků pomocí [prostory úložiště][4], byste měli nakonfigurovat s jedním sloupcem pro každý disk, který se používá.</span><span class="sxs-lookup"><span data-stu-id="e07a2-175">(See what is Disk Striping [here](storage-premium-storage-performance.md#disk-striping).) If you stripe Premium Storage data disks using [Storage Spaces][4], you should configure it with one column for each disk that is used.</span></span> <span data-ttu-id="e07a2-176">Celkový výkon prokládané svazku, jinak hodnota může být nižší než očekávaný kvůli nevyrovnaná distribuce přenosů mezi disky.</span><span class="sxs-lookup"><span data-stu-id="e07a2-176">Otherwise, the overall performance of the striped volume may be lower than expected due to uneven distribution of traffic across the disks.</span></span> <span data-ttu-id="e07a2-177">Pro virtuální počítače s Linuxem můžete použít *mdadm* nástroj k dosažení stejné.</span><span class="sxs-lookup"><span data-stu-id="e07a2-177">For Linux VMs you can use the *mdadm* utility to achieve the same.</span></span> <span data-ttu-id="e07a2-178">Najdete v článku [konfigurace RAID softwaru v systému Linux](../virtual-machines/linux/configure-raid.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) podrobnosti.</span><span class="sxs-lookup"><span data-stu-id="e07a2-178">See article [Configure Software RAID on Linux](../virtual-machines/linux/configure-raid.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) for details.</span></span>

#### <a name="storage-account-scalability-targets"></a><span data-ttu-id="e07a2-179">Cíle škálovatelnosti účtu Storage</span><span class="sxs-lookup"><span data-stu-id="e07a2-179">Storage account scalability targets</span></span>
<span data-ttu-id="e07a2-180">Prémiové účty úložiště mají následující cíle škálovatelnosti kromě [a cíle výkonnosti služby Azure Storage Scalability](storage-scalability-targets.md).</span><span class="sxs-lookup"><span data-stu-id="e07a2-180">Premium Storage accounts have the following scalability targets in addition to the [Azure Storage Scalability and Performance Targets](storage-scalability-targets.md).</span></span> <span data-ttu-id="e07a2-181">Pokud vaše požadavky aplikací přesahují cíle škálovatelnosti účtu jedno úložiště, sestavení aplikace pomocí více účtů úložiště a oddílu data mezi různými tyto účty úložiště.</span><span class="sxs-lookup"><span data-stu-id="e07a2-181">If your application requirements exceed the scalability targets of a single storage account, build your application to use multiple storage accounts, and partition your data across those storage accounts.</span></span>

| <span data-ttu-id="e07a2-182">Celkový počet účet kapacity</span><span class="sxs-lookup"><span data-stu-id="e07a2-182">Total Account Capacity</span></span> | <span data-ttu-id="e07a2-183">Celková šířka pásma pro účet místně redundantní úložiště</span><span class="sxs-lookup"><span data-stu-id="e07a2-183">Total Bandwidth for a Locally Redundant Storage Account</span></span> |
|:--- |:--- |
| <span data-ttu-id="e07a2-184">Kapacita disku: 35TB</span><span class="sxs-lookup"><span data-stu-id="e07a2-184">Disk capacity: 35TB</span></span><br /><span data-ttu-id="e07a2-185">Pořízení snímku kapacity: 10 TB</span><span class="sxs-lookup"><span data-stu-id="e07a2-185">Snapshot capacity: 10 TB</span></span> |<span data-ttu-id="e07a2-186">Až 50 gigabity za sekundu pro příchozí a odchozí</span><span class="sxs-lookup"><span data-stu-id="e07a2-186">Up to 50 gigabits per second for Inbound + Outbound</span></span> |

<span data-ttu-id="e07a2-187">Další informace o specifikacích Premium Storage, podívejte se na [škálovatelnost a cíle výkonnosti při použití služby Premium Storage](storage-premium-storage.md#scalability-and-performance-targets).</span><span class="sxs-lookup"><span data-stu-id="e07a2-187">For the more information on Premium Storage specifications, check out [Scalability and Performance Targets when using Premium Storage](storage-premium-storage.md#scalability-and-performance-targets).</span></span>

#### <a name="disk-caching-policy"></a><span data-ttu-id="e07a2-188">Zásady ukládání do mezipaměti na disku</span><span class="sxs-lookup"><span data-stu-id="e07a2-188">Disk caching policy</span></span>
<span data-ttu-id="e07a2-189">Ve výchozím nastavení, je disk ukládání do mezipaměti zásad *jen pro čtení* pro všechny Premium datových disků, a *pro čtení a zápis* pro disk operačního systému Premium připojen k virtuálnímu počítači.</span><span class="sxs-lookup"><span data-stu-id="e07a2-189">By default, disk caching policy is *Read-Only* for all the Premium data disks, and *Read-Write* for the Premium operating system disk attached to the VM.</span></span> <span data-ttu-id="e07a2-190">Toto nastavení konfigurace se doporučuje pro dosažení optimálního výkonu pro vaše aplikace IOs.</span><span class="sxs-lookup"><span data-stu-id="e07a2-190">This configuration setting is recommended to achieve the optimal performance for your application's IOs.</span></span> <span data-ttu-id="e07a2-191">Těžký zápisu nebo pouze pro zápis datové disky (například soubory protokolu serveru SQL Server) zakažte ukládání do mezipaměti disku, takže můžete dosáhnout lepší výkon aplikace.</span><span class="sxs-lookup"><span data-stu-id="e07a2-191">For write-heavy or write-only data disks (such as SQL Server log files), disable disk caching so that you can achieve better application performance.</span></span> <span data-ttu-id="e07a2-192">Nastavení mezipaměti pro existující datových disků můžete aktualizovat pomocí [portálu Azure](https://portal.azure.com) nebo *- HostCaching* parametr *Set-AzureDataDisk* rutiny.</span><span class="sxs-lookup"><span data-stu-id="e07a2-192">The cache settings for existing data disks can be updated using [Azure Portal](https://portal.azure.com) or the *-HostCaching* parameter of the *Set-AzureDataDisk* cmdlet.</span></span>

#### <a name="location"></a><span data-ttu-id="e07a2-193">Umístění</span><span class="sxs-lookup"><span data-stu-id="e07a2-193">Location</span></span>
<span data-ttu-id="e07a2-194">Vyberte umístění, kde je k dispozici Azure Premium Storage.</span><span class="sxs-lookup"><span data-stu-id="e07a2-194">Pick a location where Azure Premium Storage is available.</span></span> <span data-ttu-id="e07a2-195">V tématu [služby Azure podle oblasti](https://azure.microsoft.com/regions/#services) aktuální informace o dostupných umístění.</span><span class="sxs-lookup"><span data-stu-id="e07a2-195">See [Azure Services by Region](https://azure.microsoft.com/regions/#services) for up-to-date information on available locations.</span></span> <span data-ttu-id="e07a2-196">Virtuální počítače umístěné ve stejné oblasti jako účet úložiště, že disky pro virtuální počítač bude Story mnohem lepší výkon než pokud jsou v oblastech.</span><span class="sxs-lookup"><span data-stu-id="e07a2-196">VMs located in the same region as the Storage account that stores the disks for the VM will give much better performance than if they are in separate regions.</span></span>

#### <a name="other-azure-vm-configuration-settings"></a><span data-ttu-id="e07a2-197">Další nastavení konfigurace virtuálního počítače Azure</span><span class="sxs-lookup"><span data-stu-id="e07a2-197">Other Azure VM configuration settings</span></span>
<span data-ttu-id="e07a2-198">Při vytváření virtuálního počítače Azure, zobrazí se výzva k nakonfigurovat některá nastavení virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="e07a2-198">When creating an Azure VM, you will be asked to configure certain VM settings.</span></span> <span data-ttu-id="e07a2-199">Pamatujte si, že se několik nastavení opravě po dobu jeho existence virtuálního počítače, zatímco můžete upravit nebo jiné přidat později.</span><span class="sxs-lookup"><span data-stu-id="e07a2-199">Remember, few settings are fixed for the lifetime of the VM, while you can modify or add others later.</span></span> <span data-ttu-id="e07a2-200">Zkontrolujte nastavení konfigurace virtuálního počítače Azure a ujistěte se, zda tyto jsou správně nakonfigurována tak, aby odpovídaly vašim požadavkům zatížení.</span><span class="sxs-lookup"><span data-stu-id="e07a2-200">Review these Azure VM configuration settings and make sure that these are configured appropriately to match your workload requirements.</span></span>

### <a name="optimization"></a><span data-ttu-id="e07a2-201">Optimalizace</span><span class="sxs-lookup"><span data-stu-id="e07a2-201">Optimization</span></span>
<span data-ttu-id="e07a2-202">[Azure Premium Storage: Návrh vysoce výkonné](storage-premium-storage-performance.md) poskytuje pokyny pro vytváření vysoce výkonné aplikace pomocí Azure Premium Storage.</span><span class="sxs-lookup"><span data-stu-id="e07a2-202">[Azure Premium Storage: Design for High Performance](storage-premium-storage-performance.md) provides guidelines for building high-performance applications using Azure Premium Storage.</span></span> <span data-ttu-id="e07a2-203">Můžete postupujte podle pokynů v kombinaci s osvědčené postupy z hlediska výkonu pro technologie, které používá vaše aplikace.</span><span class="sxs-lookup"><span data-stu-id="e07a2-203">You can follow the guidelines combined with performance best practices applicable to technologies used by your application.</span></span>

## <span data-ttu-id="e07a2-204"><a name="prepare-and-copy-virtual-hard-disks-VHDs-to-premium-storage"></a>Příprava a zkopírovat virtuální pevné disky (VHD) do úložiště úrovně Premium</span><span class="sxs-lookup"><span data-stu-id="e07a2-204"><a name="prepare-and-copy-virtual-hard-disks-VHDs-to-premium-storage"></a>Prepare and copy virtual hard disks (VHDs) to Premium Storage</span></span>
<span data-ttu-id="e07a2-205">Následující část obsahuje pokyny pro přípravu virtuální pevné disky z virtuálního počítače a kopírování virtuálních pevných disků do úložiště Azure.</span><span class="sxs-lookup"><span data-stu-id="e07a2-205">The following section provides guidelines for preparing VHDs from your VM and copying VHDs to Azure Storage.</span></span>

* [<span data-ttu-id="e07a2-206">Scénář 1: "I se migruje existující virtuální počítače Azure do Azure Storage úrovně Premium."</span><span class="sxs-lookup"><span data-stu-id="e07a2-206">Scenario 1: "I am migrating existing Azure VMs to Azure Premium Storage."</span></span>](#scenario1)
* [<span data-ttu-id="e07a2-207">Scénář 2: "I mě migrace virtuálních počítačů z jiných platformách, na Azure Premium Storage."</span><span class="sxs-lookup"><span data-stu-id="e07a2-207">Scenario 2: "I am migrating VMs from other platforms to Azure Premium Storage."</span></span>](#scenario2)

### <a name="prerequisites"></a><span data-ttu-id="e07a2-208">Požadavky</span><span class="sxs-lookup"><span data-stu-id="e07a2-208">Prerequisites</span></span>
<span data-ttu-id="e07a2-209">Příprava na migraci virtuální pevné disky, budete potřebovat:</span><span class="sxs-lookup"><span data-stu-id="e07a2-209">To prepare the VHDs for migration, you'll need:</span></span>

* <span data-ttu-id="e07a2-210">Předplatné Azure, účet úložiště a kontejneru v daném účtu úložiště, do které můžete zkopírovat svůj disk VHD.</span><span class="sxs-lookup"><span data-stu-id="e07a2-210">An Azure subscription, a storage account, and a container in that storage account to which you can copy your VHD.</span></span> <span data-ttu-id="e07a2-211">Všimněte si, že cílový účet úložiště může být účet Standard nebo Premium Storage podle potřeby.</span><span class="sxs-lookup"><span data-stu-id="e07a2-211">Note that the destination storage account can be a Standard or Premium Storage account depending on your requirement.</span></span>
* <span data-ttu-id="e07a2-212">Nástroj pro zobecnění virtuální pevný disk, pokud budete chtít vytvořit více instancí virtuálního počítače z něj.</span><span class="sxs-lookup"><span data-stu-id="e07a2-212">A tool to generalize the VHD if you plan to create multiple VM instances from it.</span></span> <span data-ttu-id="e07a2-213">Například nástroj sysprep pro systém Windows nebo virt.krychle sysprep pro Ubuntu.</span><span class="sxs-lookup"><span data-stu-id="e07a2-213">For example, sysprep for Windows or virt-sysprep for Ubuntu.</span></span>
* <span data-ttu-id="e07a2-214">Nástroj pro nahrání souboru virtuálního pevného disku do účtu úložiště.</span><span class="sxs-lookup"><span data-stu-id="e07a2-214">A tool to upload the VHD file to the Storage account.</span></span> <span data-ttu-id="e07a2-215">V tématu [přenos dat pomocí nástroje příkazového řádku Azcopy](storage-use-azcopy.md) nebo použijte [Azure storage Exploreru](http://blogs.msdn.com/b/windowsazurestorage/archive/2014/03/11/windows-azure-storage-explorers-2014.aspx).</span><span class="sxs-lookup"><span data-stu-id="e07a2-215">See [Transfer data with the AzCopy Command-Line Utility](storage-use-azcopy.md) or use an [Azure storage explorer](http://blogs.msdn.com/b/windowsazurestorage/archive/2014/03/11/windows-azure-storage-explorers-2014.aspx).</span></span> <span data-ttu-id="e07a2-216">Tato příručka popisuje kopírování svůj disk VHD pomocí nástroje AzCopy.</span><span class="sxs-lookup"><span data-stu-id="e07a2-216">This guide describes copying your VHD using the AzCopy tool.</span></span>

> [!NOTE]
> <span data-ttu-id="e07a2-217">Pokud zvolíte možnost synchronní kopie s AzCopy, optimální výkon, zkopírujte svůj disk VHD spuštěním jednoho z těchto nástrojů z virtuálního počítače Azure, který je ve stejné oblasti jako cílový účet úložiště.</span><span class="sxs-lookup"><span data-stu-id="e07a2-217">If you choose synchronous copy option with AzCopy, for optimal performance, copy your VHD by running one of these tools from an Azure VM that is in the same region as the destination storage account.</span></span> <span data-ttu-id="e07a2-218">Pokud kopírujete virtuální pevný disk z virtuálního počítače Azure v jiné oblasti, může být pomalejší výkon.</span><span class="sxs-lookup"><span data-stu-id="e07a2-218">If you are copying a VHD from an Azure VM in a different region, your performance may be slower.</span></span>
>
> <span data-ttu-id="e07a2-219">Kopírování velkého množství dat přes omezenou šířkou pásma, zvažte [k přenosu dat do Blob Storage pomocí služby Azure Import/Export](storage-import-export-service.md); to umožňuje přenos dat pomocí přesouvání pevných disků o datovém centru Azure.</span><span class="sxs-lookup"><span data-stu-id="e07a2-219">For copying a large amount of data over limited bandwidth, consider [using the Azure Import/Export service to transfer data to Blob Storage](storage-import-export-service.md); this allows you to transfer your data by shipping hard disk drives to an Azure datacenter.</span></span> <span data-ttu-id="e07a2-220">Služba Azure Import/Export můžete použít ke zkopírování dat na účet úložiště standard storage.</span><span class="sxs-lookup"><span data-stu-id="e07a2-220">You can use the Azure Import/Export service to copy data to a standard storage account only.</span></span> <span data-ttu-id="e07a2-221">Jakmile jsou data ve vašem účtu standardní úložiště, můžete použít buď [kopírovat objekt Blob API](https://msdn.microsoft.com/library/azure/dd894037.aspx) nebo AzCopy k přenosu dat do účtu úložiště premium.</span><span class="sxs-lookup"><span data-stu-id="e07a2-221">Once the data is in your standard storage account, you can use either the [Copy Blob API](https://msdn.microsoft.com/library/azure/dd894037.aspx) or AzCopy to transfer the data to your premium storage account.</span></span>
>
> <span data-ttu-id="e07a2-222">Všimněte si, že Microsoft Azure podporuje pouze soubory virtuálního pevného disku pevné velikosti.</span><span class="sxs-lookup"><span data-stu-id="e07a2-222">Note that Microsoft Azure only supports fixed size VHD files.</span></span> <span data-ttu-id="e07a2-223">Soubory VHDX nebo dynamické virtuální pevné disky nejsou podporovány.</span><span class="sxs-lookup"><span data-stu-id="e07a2-223">VHDX files or dynamic VHDs are not supported.</span></span> <span data-ttu-id="e07a2-224">Pokud máte dynamický virtuální pevný disk, můžete ji převést na pevné velikosti pomocí [Convert-VHD](http://technet.microsoft.com/library/hh848454.aspx) rutiny.</span><span class="sxs-lookup"><span data-stu-id="e07a2-224">If you have a dynamic VHD, you can convert it to fixed size using the [Convert-VHD](http://technet.microsoft.com/library/hh848454.aspx) cmdlet.</span></span>
>
>

### <span data-ttu-id="e07a2-225"><a name="scenario1"></a>Scénář 1: "I se migruje existující virtuální počítače Azure do Azure Storage úrovně Premium."</span><span class="sxs-lookup"><span data-stu-id="e07a2-225"><a name="scenario1"></a>Scenario 1: "I am migrating existing Azure VMs to Azure Premium Storage."</span></span>
<span data-ttu-id="e07a2-226">Pokud migrujete existující virtuální počítače Azure, zastavte virtuální počítač, virtuální pevné disky podle typu virtuálního pevného disku, které chcete připravit a poté zkopírujte virtuální pevný disk s AzCopy nebo prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="e07a2-226">If you are migrating existing Azure VMs, stop the VM, prepare VHDs per the type of VHD you want, and then copy the VHD with AzCopy or PowerShell.</span></span>

<span data-ttu-id="e07a2-227">Virtuální počítač musí být úplně dolů k migraci do čistého stavu.</span><span class="sxs-lookup"><span data-stu-id="e07a2-227">The VM needs to be completely down to migrate a clean state.</span></span> <span data-ttu-id="e07a2-228">Nedojde výpadku až po dokončení migrace.</span><span class="sxs-lookup"><span data-stu-id="e07a2-228">There will be a downtime until the migration completes.</span></span>

#### <a name="step-1-prepare-vhds-for-migration"></a><span data-ttu-id="e07a2-229">Krok 1.</span><span class="sxs-lookup"><span data-stu-id="e07a2-229">Step 1.</span></span> <span data-ttu-id="e07a2-230">Příprava na migraci virtuálních pevných disků</span><span class="sxs-lookup"><span data-stu-id="e07a2-230">Prepare VHDs for migration</span></span>
<span data-ttu-id="e07a2-231">Pokud migrujete existující virtuální počítače Azure do úložiště úrovně Premium, může být svůj disk VHD:</span><span class="sxs-lookup"><span data-stu-id="e07a2-231">If you are migrating existing Azure VMs to Premium Storage, your VHD may be:</span></span>

* <span data-ttu-id="e07a2-232">Bitovou kopii operačního systému</span><span class="sxs-lookup"><span data-stu-id="e07a2-232">A generalized operating system image</span></span>
* <span data-ttu-id="e07a2-233">Disk jedinečné operačního systému</span><span class="sxs-lookup"><span data-stu-id="e07a2-233">A unique operating system disk</span></span>
* <span data-ttu-id="e07a2-234">Datový disk</span><span class="sxs-lookup"><span data-stu-id="e07a2-234">A data disk</span></span>

<span data-ttu-id="e07a2-235">Níže budeme provede tyto 3 scénáře pro přípravu svůj disk VHD.</span><span class="sxs-lookup"><span data-stu-id="e07a2-235">Below we walk through these 3 scenarios for preparing your VHD.</span></span>

##### <a name="use-a-generalized-operating-system-vhd-to-create-multiple-vm-instances"></a><span data-ttu-id="e07a2-236">Pomocí zobecněný virtuální pevný disk operačního systému můžete vytvořit více instancí virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="e07a2-236">Use a generalized Operating System VHD to create multiple VM instances</span></span>
<span data-ttu-id="e07a2-237">Pokud nahráváte virtuálního pevného disku, který se použije k vytvoření více obecné instancí virtuálního počítače Azure, musíte nejprve generalize virtuální pevný disk pomocí nástroje sysprep.</span><span class="sxs-lookup"><span data-stu-id="e07a2-237">If you are uploading a VHD that will be used to create multiple generic Azure VM instances, you must first generalize VHD using a sysprep utility.</span></span> <span data-ttu-id="e07a2-238">To se vztahuje na virtuální pevný disk, který je místně nebo v cloudu.</span><span class="sxs-lookup"><span data-stu-id="e07a2-238">This applies to a VHD that is on-premises or in the cloud.</span></span> <span data-ttu-id="e07a2-239">Nástroj Sysprep odebere všechny informace specifické pro počítač z virtuálního pevného disku.</span><span class="sxs-lookup"><span data-stu-id="e07a2-239">Sysprep removes any machine-specific information from the VHD.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="e07a2-240">Pořízení snímku nebo zálohování virtuálního počítače před jeho generalizací.</span><span class="sxs-lookup"><span data-stu-id="e07a2-240">Take a snapshot or backup your VM before generalizing it.</span></span> <span data-ttu-id="e07a2-241">Spuštěný nástroj sysprep zastaví a navrátit instance virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="e07a2-241">Running sysprep will stop and deallocate the VM instance.</span></span> <span data-ttu-id="e07a2-242">Postupujte podle kroků pro nástroj sysprep virtuálního pevného disku operačního systému Windows.</span><span class="sxs-lookup"><span data-stu-id="e07a2-242">Follow steps below to sysprep a Windows OS VHD.</span></span> <span data-ttu-id="e07a2-243">Všimněte si, že spustíte příkaz Sysprep bude vyžadovat, abyste vypnout virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="e07a2-243">Note that running the Sysprep command will require you to shut down the virtual machine.</span></span> <span data-ttu-id="e07a2-244">Další informace o nástroji Sysprep najdete v tématu [přehled nástroje Sysprep](http://technet.microsoft.com/library/hh825209.aspx) nebo [technické informace o nástroji Sysprep](http://technet.microsoft.com/library/cc766049.aspx).</span><span class="sxs-lookup"><span data-stu-id="e07a2-244">For more information about Sysprep, see [Sysprep Overview](http://technet.microsoft.com/library/hh825209.aspx) or [Sysprep Technical Reference](http://technet.microsoft.com/library/cc766049.aspx).</span></span>
>
>

1. <span data-ttu-id="e07a2-245">Otevřete okno příkazového řádku jako správce.</span><span class="sxs-lookup"><span data-stu-id="e07a2-245">Open a Command Prompt window as an administrator.</span></span>
2. <span data-ttu-id="e07a2-246">Zadejte následující příkaz, chcete-li spustit nástroj Sysprep:</span><span class="sxs-lookup"><span data-stu-id="e07a2-246">Enter the following command to open Sysprep:</span></span>

    ```
    %windir%\system32\sysprep\sysprep.exe
    ```

3. <span data-ttu-id="e07a2-247">V nástroji pro přípravu systému vyberte prostředí systému zadejte Out-of-Box (při prvním zapnutí), zaškrtněte políčko generalizace, vyberte **vypnutí**a potom klikněte na **OK**, jak je znázorněno na obrázku níže.</span><span class="sxs-lookup"><span data-stu-id="e07a2-247">In the System Preparation Tool, select Enter System Out-of-Box Experience (OOBE), select the Generalize check box, select **Shutdown**, and then click **OK**, as shown in the image below.</span></span> <span data-ttu-id="e07a2-248">Nástroj Sysprep odstraní zobecní operační systém a vypnutí systému.</span><span class="sxs-lookup"><span data-stu-id="e07a2-248">Sysprep will generalize the operating system and shut down the system.</span></span>

    ![][1]

<span data-ttu-id="e07a2-249">U virtuálního počítače s Ubuntu použijte virt.krychle sysprep k dosažení stejné.</span><span class="sxs-lookup"><span data-stu-id="e07a2-249">For an Ubuntu VM, use virt-sysprep to achieve the same.</span></span> <span data-ttu-id="e07a2-250">V tématu [virt.krychle sysprep](http://manpages.ubuntu.com/manpages/precise/man1/virt-sysprep.1.html) další podrobnosti.</span><span class="sxs-lookup"><span data-stu-id="e07a2-250">See [virt-sysprep](http://manpages.ubuntu.com/manpages/precise/man1/virt-sysprep.1.html) for more details.</span></span> <span data-ttu-id="e07a2-251">Viz také některé open source [Linux serveru zřizování softwaru](http://www.cyberciti.biz/tips/server-provisioning-software.html) pro jiné operační systémy Linux.</span><span class="sxs-lookup"><span data-stu-id="e07a2-251">See also some of the open source [Linux Server Provisioning software](http://www.cyberciti.biz/tips/server-provisioning-software.html) for other Linux operating systems.</span></span>

##### <a name="use-a-unique-operating-system-vhd-to-create-a-single-vm-instance"></a><span data-ttu-id="e07a2-252">K vytvoření jedné instance virtuálního počítače použijte jedinečný virtuální pevný disk operačního systému</span><span class="sxs-lookup"><span data-stu-id="e07a2-252">Use a unique Operating System VHD to create a single VM instance</span></span>
<span data-ttu-id="e07a2-253">Pokud máte aplikace běžící na virtuálním počítači, který vyžaduje konkrétní data počítače, není generalize virtuální pevný disk.</span><span class="sxs-lookup"><span data-stu-id="e07a2-253">If you have an application running on the VM which requires the machine specific data, do not generalize the VHD.</span></span> <span data-ttu-id="e07a2-254">Zobecněný virtuální pevný disk lze použít k vytvoření jedinečné instance virtuálního počítače Azure.</span><span class="sxs-lookup"><span data-stu-id="e07a2-254">A non-generalized VHD can be used to create a unique Azure VM instance.</span></span> <span data-ttu-id="e07a2-255">Například pokud máte řadiče domény na svůj disk VHD, provádění sysprep znamená, že neúčinná jako řadič domény.</span><span class="sxs-lookup"><span data-stu-id="e07a2-255">For example, if you have Domain Controller on your VHD, executing sysprep will make it ineffective as a Domain Controller.</span></span> <span data-ttu-id="e07a2-256">Zkontrolujte aplikací běžících na virtuální počítač a dopad spuštění nástroje sysprep v jejich před generalizací virtuální pevný disk.</span><span class="sxs-lookup"><span data-stu-id="e07a2-256">Review the applications running on your VM and the impact of running sysprep on them before generalizing the VHD.</span></span>

##### <a name="register-data-disk-vhd"></a><span data-ttu-id="e07a2-257">Registrovat datový disk VHD</span><span class="sxs-lookup"><span data-stu-id="e07a2-257">Register data disk VHD</span></span>
<span data-ttu-id="e07a2-258">Pokud máte v Azure a migrovat datových disků, musí se zkontrolujte, zda jsou virtuální počítače, které používají tyto datové disky vypnout.</span><span class="sxs-lookup"><span data-stu-id="e07a2-258">If you have data disks in Azure to be migrated, you must make sure the VMs that use these data disks are shut down.</span></span>

<span data-ttu-id="e07a2-259">Postupujte podle kroků popsaných níže zkopírujte virtuální pevný disk do Azure Premium Storage a zaregistrovat ho jako zřízené datový disk.</span><span class="sxs-lookup"><span data-stu-id="e07a2-259">Follow the steps described below to copy VHD to Azure Premium Storage and register it as a provisioned data disk.</span></span>

#### <a name="step-2-create-the-destination-for-your-vhd"></a><span data-ttu-id="e07a2-260">Krok 2.</span><span class="sxs-lookup"><span data-stu-id="e07a2-260">Step 2.</span></span> <span data-ttu-id="e07a2-261">Vytvořit cíl pro svůj disk VHD</span><span class="sxs-lookup"><span data-stu-id="e07a2-261">Create the destination for your VHD</span></span>
<span data-ttu-id="e07a2-262">Vytvořte účet úložiště pro údržbu virtuální pevné disky.</span><span class="sxs-lookup"><span data-stu-id="e07a2-262">Create a storage account for maintaining your VHDs.</span></span> <span data-ttu-id="e07a2-263">Při plánování kam se mají ukládat virtuální pevné disky, zvažte následující body:</span><span class="sxs-lookup"><span data-stu-id="e07a2-263">Consider the following points when planning where to store your VHDs:</span></span>

* <span data-ttu-id="e07a2-264">Cílový účet úložiště Premium.</span><span class="sxs-lookup"><span data-stu-id="e07a2-264">The target Premium storage account.</span></span>
* <span data-ttu-id="e07a2-265">Umístění účtu úložiště musí být stejná jako úložiště Premium podporuje virtuální počítače Azure se vytvoří v konečné fázi.</span><span class="sxs-lookup"><span data-stu-id="e07a2-265">The storage account location must be same as Premium Storage capable Azure VMs you will create in the final stage.</span></span> <span data-ttu-id="e07a2-266">Může zkopírovat nového účtu úložiště, nebo se chystáte použít stejný účet úložiště na základě potřeb.</span><span class="sxs-lookup"><span data-stu-id="e07a2-266">You could copy to a new storage account, or plan to use the same storage account based on your needs.</span></span>
* <span data-ttu-id="e07a2-267">Zkopírujte a uložte klíč účtu úložiště cílový účet úložiště pro další fáze.</span><span class="sxs-lookup"><span data-stu-id="e07a2-267">Copy and save the storage account key of the destination storage account for the next stage.</span></span>

<span data-ttu-id="e07a2-268">Pro datové disky můžete si nechat některé datové disky v standardní účet úložiště (například disky, které obsahují chladič úložiště), ale důrazně doporučujeme přesouvání všech dat pro produkční pracovního vytížení k používání služby storage úrovně premium.</span><span class="sxs-lookup"><span data-stu-id="e07a2-268">For data disks, you can choose to keep some data disks in a standard storage account (for example, disks that have cooler storage), but we strongly recommend you moving all data for production workload to use premium storage.</span></span>

#### <span data-ttu-id="e07a2-269"><a name="copy-vhd-with-azcopy-or-powershell"></a>Krok 3.</span><span class="sxs-lookup"><span data-stu-id="e07a2-269"><a name="copy-vhd-with-azcopy-or-powershell"></a>Step 3.</span></span> <span data-ttu-id="e07a2-270">Zkopírujte virtuální pevný disk s AzCopy nebo prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="e07a2-270">Copy VHD with AzCopy or PowerShell</span></span>
<span data-ttu-id="e07a2-271">Musíte se najít klíč kontejneru úložiště a cesta účtu ke zpracování některá z těchto dvou možností.</span><span class="sxs-lookup"><span data-stu-id="e07a2-271">You will need to find your container path and storage account key to process either of these two options.</span></span> <span data-ttu-id="e07a2-272">Klíč účtu úložiště a cesta kontejneru lze nalézt v **portálu Azure** > **úložiště**.</span><span class="sxs-lookup"><span data-stu-id="e07a2-272">Container path and storage account key can be found in **Azure Portal** > **Storage**.</span></span> <span data-ttu-id="e07a2-273">Jako "https://myaccount.blob.core.windows.net/mycontainer/" bude mít adresu URL kontejneru.</span><span class="sxs-lookup"><span data-stu-id="e07a2-273">The container URL will be like "https://myaccount.blob.core.windows.net/mycontainer/".</span></span>

##### <a name="option-1-copy-a-vhd-with-azcopy-asynchronous-copy"></a><span data-ttu-id="e07a2-274">Možnost 1: Zkopírujte virtuální pevný disk s AzCopy (asynchronní kopie)</span><span class="sxs-lookup"><span data-stu-id="e07a2-274">Option 1: Copy a VHD with AzCopy (Asynchronous copy)</span></span>
<span data-ttu-id="e07a2-275">Pomocí AzCopy, můžete snadno nahrávat VHD přes Internet.</span><span class="sxs-lookup"><span data-stu-id="e07a2-275">Using AzCopy, you can easily upload the VHD over the Internet.</span></span> <span data-ttu-id="e07a2-276">V závislosti na velikosti virtuálních pevných disků může to trvat čas.</span><span class="sxs-lookup"><span data-stu-id="e07a2-276">Depending on the size of the VHDs, this may take time.</span></span> <span data-ttu-id="e07a2-277">Mějte na paměti, zkontrolujte vstupní/výstupní limity účtu úložiště při použití této možnosti.</span><span class="sxs-lookup"><span data-stu-id="e07a2-277">Remember to check the storage account ingress/egress limits when using this option.</span></span> <span data-ttu-id="e07a2-278">V tématu [a cíle výkonnosti služby Azure Storage Scalability](storage-scalability-targets.md) podrobnosti.</span><span class="sxs-lookup"><span data-stu-id="e07a2-278">See [Azure Storage Scalability and Performance Targets](storage-scalability-targets.md) for details.</span></span>

1. <span data-ttu-id="e07a2-279">Stáhněte a nainstalujte AzCopy odsud: [nejnovější verzi AzCopy](http://aka.ms/downloadazcopy)</span><span class="sxs-lookup"><span data-stu-id="e07a2-279">Download and install AzCopy from here: [Latest version of AzCopy](http://aka.ms/downloadazcopy)</span></span>
2. <span data-ttu-id="e07a2-280">Otevřete prostředí Azure PowerShell a přejděte do složky, kde je nainstalován nástroj AzCopy.</span><span class="sxs-lookup"><span data-stu-id="e07a2-280">Open Azure PowerShell and go to the folder where AzCopy is installed.</span></span>
3. <span data-ttu-id="e07a2-281">Pomocí následujícího příkazu zkopírujte soubor virtuálního pevného disku z "Zdroje" do "Cíle".</span><span class="sxs-lookup"><span data-stu-id="e07a2-281">Use the following command to copy the VHD file from "Source" to "Destination".</span></span>

    ```azcopy
    AzCopy /Source: <source> /SourceKey: <source-account-key> /Dest: <destination> /DestKey: <dest-account-key> /BlobType:page /Pattern: <file-name>
    ```

    <span data-ttu-id="e07a2-282">Příklad:</span><span class="sxs-lookup"><span data-stu-id="e07a2-282">Example:</span></span>

    ```azcopy
    AzCopy /Source:https://sourceaccount.blob.core.windows.net/mycontainer1 /SourceKey:key1 /Dest:https://destaccount.blob.core.windows.net/mycontainer2 /DestKey:key2 /Pattern:abc.vhd
    ```

    <span data-ttu-id="e07a2-283">Zde je uveden popis použité v příkazu AzCopy parametry:</span><span class="sxs-lookup"><span data-stu-id="e07a2-283">Here are descriptions of the parameters used in the AzCopy command:</span></span>

   * <span data-ttu-id="e07a2-284">**/ Zdroj:  *&lt;zdroj&gt;:***  umístění složky nebo URL adresa kontejneru úložiště, který obsahuje virtuální pevný disk.</span><span class="sxs-lookup"><span data-stu-id="e07a2-284">**/Source: *&lt;source&gt;:*** Location of the folder or storage container URL that contains the VHD.</span></span>
   * <span data-ttu-id="e07a2-285">**/ SourceKey:  *&lt;klíč účtu zdroj&gt;:***  klíč účtu úložiště zdrojového účtu úložiště.</span><span class="sxs-lookup"><span data-stu-id="e07a2-285">**/SourceKey: *&lt;source-account-key&gt;:*** Storage account key of the source storage account.</span></span>
   * <span data-ttu-id="e07a2-286">**/ Cíle:  *&lt;cílové&gt;:***  úložiště URL adresa kontejneru VHD, který chcete zkopírovat.</span><span class="sxs-lookup"><span data-stu-id="e07a2-286">**/Dest: *&lt;destination&gt;:*** Storage container URL to copy the VHD to.</span></span>
   * <span data-ttu-id="e07a2-287">**/ DestKey:  *&lt;klíč účtu cíle&gt;:***  klíč účtu úložiště cílový účet úložiště.</span><span class="sxs-lookup"><span data-stu-id="e07a2-287">**/DestKey: *&lt;dest-account-key&gt;:*** Storage account key of the destination storage account.</span></span>
   * <span data-ttu-id="e07a2-288">**/ Vzor:  *&lt;název souboru&gt;:***  zadejte název souboru virtuálního pevného disku ke kopírování.</span><span class="sxs-lookup"><span data-stu-id="e07a2-288">**/Pattern: *&lt;file-name&gt;:*** Specify the file name of the VHD to copy.</span></span>

<span data-ttu-id="e07a2-289">Podrobnosti o použití nástroje AzCopy nástroje najdete v tématu [přenos dat pomocí nástroje příkazového řádku Azcopy](storage-use-azcopy.md).</span><span class="sxs-lookup"><span data-stu-id="e07a2-289">For details on using AzCopy tool, see [Transfer data with the AzCopy Command-Line Utility](storage-use-azcopy.md).</span></span>

##### <a name="option-2-copy-a-vhd-with-powershell-synchronized-copy"></a><span data-ttu-id="e07a2-290">Možnost 2: Zkopírujte virtuální pevný disk pomocí prostředí PowerShell (Synchronized kopie)</span><span class="sxs-lookup"><span data-stu-id="e07a2-290">Option 2: Copy a VHD with PowerShell (Synchronized copy)</span></span>
<span data-ttu-id="e07a2-291">Můžete také zkopírovat soubor virtuálního pevného disku pomocí rutiny prostředí PowerShell Start AzureStorageBlobCopy.</span><span class="sxs-lookup"><span data-stu-id="e07a2-291">You can also copy the VHD file using the PowerShell cmdlet Start-AzureStorageBlobCopy.</span></span> <span data-ttu-id="e07a2-292">Pomocí následujícího příkazu v prostředí Azure PowerShell zkopírujte virtuální pevný disk.</span><span class="sxs-lookup"><span data-stu-id="e07a2-292">Use the following command on Azure PowerShell to copy VHD.</span></span> <span data-ttu-id="e07a2-293">Nahraďte hodnoty v <> s odpovídajícími hodnotami z vašeho zdrojového a cílového účtu úložiště.</span><span class="sxs-lookup"><span data-stu-id="e07a2-293">Replace the values in <> with corresponding values from your source and destination storage account.</span></span> <span data-ttu-id="e07a2-294">Chcete-li použít tento příkaz, musíte mít volat virtuální pevné disky ve vašem účtu úložiště cílový kontejner.</span><span class="sxs-lookup"><span data-stu-id="e07a2-294">To use this command, you must have a container called vhds in your destination storage account.</span></span> <span data-ttu-id="e07a2-295">Pokud kontejner neexistuje, vytvořte ji před spuštěním příkazu.</span><span class="sxs-lookup"><span data-stu-id="e07a2-295">If the container doesn't exist, create one before running the command.</span></span>

```powershell
$sourceBlobUri = <source-vhd-uri>

$sourceContext = New-AzureStorageContext  –StorageAccountName <source-account> -StorageAccountKey <source-account-key>

$destinationContext = New-AzureStorageContext  –StorageAccountName <dest-account> -StorageAccountKey <dest-account-key>

Start-AzureStorageBlobCopy -srcUri $sourceBlobUri -SrcContext $sourceContext -DestContainer <dest-container> -DestBlob <dest-disk-name> -DestContext $destinationContext
```

<span data-ttu-id="e07a2-296">Příklad:</span><span class="sxs-lookup"><span data-stu-id="e07a2-296">Example:</span></span>

```powershell
C:\PS> $sourceBlobUri = "https://sourceaccount.blob.core.windows.net/vhds/myvhd.vhd"

C:\PS> $sourceContext = New-AzureStorageContext  –StorageAccountName "sourceaccount" -StorageAccountKey "J4zUI9T5b8gvHohkiRg"

C:\PS> $destinationContext = New-AzureStorageContext  –StorageAccountName "destaccount" -StorageAccountKey "XZTmqSGKUYFSh7zB5"

C:\PS> Start-AzureStorageBlobCopy -srcUri $sourceBlobUri -SrcContext $sourceContext -DestContainer "vhds" -DestBlob "myvhd.vhd" -DestContext $destinationContext
```

### <span data-ttu-id="e07a2-297"><a name="scenario2"></a>Scénář 2: "I mě migrace virtuálních počítačů z jiných platformách, na Azure Premium Storage."</span><span class="sxs-lookup"><span data-stu-id="e07a2-297"><a name="scenario2"></a>Scenario 2: "I am migrating VMs from other platforms to Azure Premium Storage."</span></span>
<span data-ttu-id="e07a2-298">Pokud migrujete virtuální pevný disk z jiných - cloudové úložiště Azure do Azure, musíte napřed exportovat virtuální pevný disk do místního adresáře.</span><span class="sxs-lookup"><span data-stu-id="e07a2-298">If you are migrating VHD from non-Azure Cloud Storage to Azure, you must first export the VHD to a local directory.</span></span> <span data-ttu-id="e07a2-299">Úplný zdrojový cesty místnímu adresáři, kde je uložený užitečný virtuálního pevného disku a potom pomocí AzCopy nahrajte ho do úložiště Azure.</span><span class="sxs-lookup"><span data-stu-id="e07a2-299">Have the complete source path of the local directory where VHD is stored handy, and then using AzCopy to upload it to Azure Storage.</span></span>

#### <a name="step-1-export-vhd-to-a-local-directory"></a><span data-ttu-id="e07a2-300">Krok 1.</span><span class="sxs-lookup"><span data-stu-id="e07a2-300">Step 1.</span></span> <span data-ttu-id="e07a2-301">Export virtuálního pevného disku do místního adresáře</span><span class="sxs-lookup"><span data-stu-id="e07a2-301">Export VHD to a local directory</span></span>
##### <a name="copy-a-vhd-from-aws"></a><span data-ttu-id="e07a2-302">Zkopírujte virtuální pevný disk z AWS</span><span class="sxs-lookup"><span data-stu-id="e07a2-302">Copy a VHD from AWS</span></span>
1. <span data-ttu-id="e07a2-303">Pokud používáte AWS, vyexportovat do disku VHD v sady Amazon S3 EC2 instance.</span><span class="sxs-lookup"><span data-stu-id="e07a2-303">If you are using AWS, export the EC2 instance to a VHD in an Amazon S3 bucket.</span></span> <span data-ttu-id="e07a2-304">Postupujte podle kroků popsaných v dokumentaci k Amazon pro export Amazon EC2 instance, které chcete nainstalovat nástroj Amazon EC2 rozhraní příkazového řádku (CLI) a spusťte příkaz Vytvořit instanci export úkolů EC2 instance exportovat do souboru VHD.</span><span class="sxs-lookup"><span data-stu-id="e07a2-304">Follow the steps described in the Amazon documentation for Exporting Amazon EC2 Instances to install the Amazon EC2 command-line interface (CLI) tool and run the create-instance-export-task command to export the EC2 instance to a VHD file.</span></span> <span data-ttu-id="e07a2-305">Nezapomeňte použít **virtuálního pevného disku** pro DISK &#95; IMAGE &#95; Formát proměnné při spuštění **vytvořit instanci export úkolů** příkaz.</span><span class="sxs-lookup"><span data-stu-id="e07a2-305">Be sure to use **VHD** for the DISK&#95;IMAGE&#95;FORMAT variable when running the **create-instance-export-task** command.</span></span> <span data-ttu-id="e07a2-306">Exportovaný soubor virtuálního pevného disku je uložen v Amazon S3 sady, které určíte během tohoto procesu.</span><span class="sxs-lookup"><span data-stu-id="e07a2-306">The exported VHD file is saved in the Amazon S3 bucket you designate during that process.</span></span>

    ```
    aws ec2 create-instance-export-task --instance-id ID --target-environment TARGET_ENVIRONMENT \
      --export-to-s3-task DiskImageFormat=DISK_IMAGE_FORMAT,ContainerFormat=ova,S3Bucket=BUCKET,S3Prefix=PREFIX
    ```

2. <span data-ttu-id="e07a2-307">Soubor VHD stáhněte ze sady S3.</span><span class="sxs-lookup"><span data-stu-id="e07a2-307">Download the VHD file from the S3 bucket.</span></span> <span data-ttu-id="e07a2-308">Pak vyberte příslušný soubor VHD **akce** > **Stáhnout**.</span><span class="sxs-lookup"><span data-stu-id="e07a2-308">Select the VHD file, then **Actions** > **Download**.</span></span>

    ![][3]

##### <a name="copy-a-vhd-from-other-non-azure-cloud"></a><span data-ttu-id="e07a2-309">Zkopírujte virtuální pevný disk z jiné mimo Azure cloud</span><span class="sxs-lookup"><span data-stu-id="e07a2-309">Copy a VHD from other non-Azure cloud</span></span>
<span data-ttu-id="e07a2-310">Pokud migrujete virtuální pevný disk z jiných - cloudové úložiště Azure do Azure, musíte napřed exportovat virtuální pevný disk do místního adresáře.</span><span class="sxs-lookup"><span data-stu-id="e07a2-310">If you are migrating VHD from non-Azure Cloud Storage to Azure, you must first export the VHD to a local directory.</span></span> <span data-ttu-id="e07a2-311">Zkopírujte dokončení zdrojovou cestu místnímu adresáři, kde je uložený virtuální pevný disk.</span><span class="sxs-lookup"><span data-stu-id="e07a2-311">Copy the complete source path of the local directory where VHD is stored.</span></span>

##### <a name="copy-a-vhd-from-on-premises"></a><span data-ttu-id="e07a2-312">Zkopírujte virtuální pevný disk z místní</span><span class="sxs-lookup"><span data-stu-id="e07a2-312">Copy a VHD from on-premises</span></span>
<span data-ttu-id="e07a2-313">Pokud migrujete virtuální pevný disk z místního prostředí, budete potřebovat úplný zdrojový cestu, kde je uložený virtuální pevný disk.</span><span class="sxs-lookup"><span data-stu-id="e07a2-313">If you are migrating VHD from an on-premises environment, you will need the complete source path where VHD is stored.</span></span> <span data-ttu-id="e07a2-314">Zdrojová cesta může být server umístění nebo sdílené složky.</span><span class="sxs-lookup"><span data-stu-id="e07a2-314">The source path could be a server location or file share.</span></span>

#### <a name="step-2-create-the-destination-for-your-vhd"></a><span data-ttu-id="e07a2-315">Krok 2.</span><span class="sxs-lookup"><span data-stu-id="e07a2-315">Step 2.</span></span> <span data-ttu-id="e07a2-316">Vytvořit cíl pro svůj disk VHD</span><span class="sxs-lookup"><span data-stu-id="e07a2-316">Create the destination for your VHD</span></span>
<span data-ttu-id="e07a2-317">Vytvořte účet úložiště pro údržbu virtuální pevné disky.</span><span class="sxs-lookup"><span data-stu-id="e07a2-317">Create a storage account for maintaining your VHDs.</span></span> <span data-ttu-id="e07a2-318">Při plánování kam se mají ukládat virtuální pevné disky, zvažte následující body:</span><span class="sxs-lookup"><span data-stu-id="e07a2-318">Consider the following points when planning where to store your VHDs:</span></span>

* <span data-ttu-id="e07a2-319">Cílový účet úložiště může být úložiště standard nebo premium, v závislosti na požadavcích vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="e07a2-319">The target storage account could be standard or premium storage depending on your application requirement.</span></span>
* <span data-ttu-id="e07a2-320">Oblast účtu úložiště musí být stejná jako úložiště Premium podporuje virtuální počítače Azure se vytvoří v konečné fázi.</span><span class="sxs-lookup"><span data-stu-id="e07a2-320">The storage account region must be same as Premium Storage capable Azure VMs you will create in the final stage.</span></span> <span data-ttu-id="e07a2-321">Může zkopírovat nového účtu úložiště, nebo se chystáte použít stejný účet úložiště na základě potřeb.</span><span class="sxs-lookup"><span data-stu-id="e07a2-321">You could copy to a new storage account, or plan to use the same storage account based on your needs.</span></span>
* <span data-ttu-id="e07a2-322">Zkopírujte a uložte klíč účtu úložiště cílový účet úložiště pro další fáze.</span><span class="sxs-lookup"><span data-stu-id="e07a2-322">Copy and save the storage account key of the destination storage account for the next stage.</span></span>

<span data-ttu-id="e07a2-323">Důrazně doporučujeme přesouvání všech dat pro produkční pracovního vytížení k používání služby storage úrovně premium.</span><span class="sxs-lookup"><span data-stu-id="e07a2-323">We strongly recommend you moving all data for production workload to use premium storage.</span></span>

#### <a name="step-3-upload-the-vhd-to-azure-storage"></a><span data-ttu-id="e07a2-324">Krok 3.</span><span class="sxs-lookup"><span data-stu-id="e07a2-324">Step 3.</span></span> <span data-ttu-id="e07a2-325">Nahrání virtuálního pevného disku do úložiště Azure</span><span class="sxs-lookup"><span data-stu-id="e07a2-325">Upload the VHD to Azure Storage</span></span>
<span data-ttu-id="e07a2-326">Nyní když máte vaši virtuálního pevného disku v místním adresáři, můžete AzCopy nebo AzurePowerShell nahrát soubor VHD do služby Azure Storage.</span><span class="sxs-lookup"><span data-stu-id="e07a2-326">Now that you have your VHD in the local directory, you can use AzCopy or AzurePowerShell to upload the .vhd file to Azure Storage.</span></span> <span data-ttu-id="e07a2-327">Obě možnosti jsou k dispozici zde:</span><span class="sxs-lookup"><span data-stu-id="e07a2-327">Both options are provided here:</span></span>

##### <a name="option-1-using-azure-powershell-add-azurevhd-to-upload-the-vhd-file"></a><span data-ttu-id="e07a2-328">Možnost 1: Použití Azure PowerShell Add-AzureVhd nahrát soubor VHD</span><span class="sxs-lookup"><span data-stu-id="e07a2-328">Option 1: Using Azure PowerShell Add-AzureVhd to upload the .vhd file</span></span>

```powershell
Add-AzureVhd [-Destination] <Uri> [-LocalFilePath] <FileInfo>
```

<span data-ttu-id="e07a2-329">Příklad <Uri> může být ***"https://storagesample.blob.core.windows.net/mycontainer/blob1.vhd"***.</span><span class="sxs-lookup"><span data-stu-id="e07a2-329">An example <Uri> might be ***"https://storagesample.blob.core.windows.net/mycontainer/blob1.vhd"***.</span></span> <span data-ttu-id="e07a2-330">Příklad <FileInfo> může být ***"C:\path\to\upload.vhd"***.</span><span class="sxs-lookup"><span data-stu-id="e07a2-330">An example <FileInfo> might be ***"C:\path\to\upload.vhd"***.</span></span>

##### <a name="option-2-using-azcopy-to-upload-the-vhd-file"></a><span data-ttu-id="e07a2-331">Možnost 2: Nahrání souboru VHD pomocí nástroje AzCopy</span><span class="sxs-lookup"><span data-stu-id="e07a2-331">Option 2: Using AzCopy to upload the .vhd file</span></span>
<span data-ttu-id="e07a2-332">Pomocí AzCopy, můžete snadno nahrávat VHD přes Internet.</span><span class="sxs-lookup"><span data-stu-id="e07a2-332">Using AzCopy, you can easily upload the VHD over the Internet.</span></span> <span data-ttu-id="e07a2-333">V závislosti na velikosti virtuálních pevných disků může to trvat čas.</span><span class="sxs-lookup"><span data-stu-id="e07a2-333">Depending on the size of the VHDs, this may take time.</span></span> <span data-ttu-id="e07a2-334">Mějte na paměti, zkontrolujte vstupní/výstupní limity účtu úložiště při použití této možnosti.</span><span class="sxs-lookup"><span data-stu-id="e07a2-334">Remember to check the storage account ingress/egress limits when using this option.</span></span> <span data-ttu-id="e07a2-335">V tématu [a cíle výkonnosti služby Azure Storage Scalability](storage-scalability-targets.md) podrobnosti.</span><span class="sxs-lookup"><span data-stu-id="e07a2-335">See [Azure Storage Scalability and Performance Targets](storage-scalability-targets.md) for details.</span></span>

1. <span data-ttu-id="e07a2-336">Stáhněte a nainstalujte AzCopy odsud: [nejnovější verzi AzCopy](http://aka.ms/downloadazcopy)</span><span class="sxs-lookup"><span data-stu-id="e07a2-336">Download and install AzCopy from here: [Latest version of AzCopy](http://aka.ms/downloadazcopy)</span></span>
2. <span data-ttu-id="e07a2-337">Otevřete prostředí Azure PowerShell a přejděte do složky, kde je nainstalován nástroj AzCopy.</span><span class="sxs-lookup"><span data-stu-id="e07a2-337">Open Azure PowerShell and go to the folder where AzCopy is installed.</span></span>
3. <span data-ttu-id="e07a2-338">Pomocí následujícího příkazu zkopírujte soubor virtuálního pevného disku z "Zdroje" do "Cíle".</span><span class="sxs-lookup"><span data-stu-id="e07a2-338">Use the following command to copy the VHD file from "Source" to "Destination".</span></span>

    ```azcopy
    AzCopy /Source: <source> /SourceKey: <source-account-key> /Dest: <destination> /DestKey: <dest-account-key> /BlobType:page /Pattern: <file-name>
    ```

    <span data-ttu-id="e07a2-339">Příklad:</span><span class="sxs-lookup"><span data-stu-id="e07a2-339">Example:</span></span>

    ```azcopy
    AzCopy /Source:https://sourceaccount.blob.core.windows.net/mycontainer1 /SourceKey:key1 /Dest:https://destaccount.blob.core.windows.net/mycontainer2 /DestKey:key2 /BlobType:page /Pattern:abc.vhd
    ```

    <span data-ttu-id="e07a2-340">Zde je uveden popis použité v příkazu AzCopy parametry:</span><span class="sxs-lookup"><span data-stu-id="e07a2-340">Here are descriptions of the parameters used in the AzCopy command:</span></span>

   * <span data-ttu-id="e07a2-341">**/ Zdroj:  *&lt;zdroj&gt;:***  umístění složky nebo URL adresa kontejneru úložiště, který obsahuje virtuální pevný disk.</span><span class="sxs-lookup"><span data-stu-id="e07a2-341">**/Source: *&lt;source&gt;:*** Location of the folder or storage container URL that contains the VHD.</span></span>
   * <span data-ttu-id="e07a2-342">**/ SourceKey:  *&lt;klíč účtu zdroj&gt;:***  klíč účtu úložiště zdrojového účtu úložiště.</span><span class="sxs-lookup"><span data-stu-id="e07a2-342">**/SourceKey: *&lt;source-account-key&gt;:*** Storage account key of the source storage account.</span></span>
   * <span data-ttu-id="e07a2-343">**/ Cíle:  *&lt;cílové&gt;:***  úložiště URL adresa kontejneru VHD, který chcete zkopírovat.</span><span class="sxs-lookup"><span data-stu-id="e07a2-343">**/Dest: *&lt;destination&gt;:*** Storage container URL to copy the VHD to.</span></span>
   * <span data-ttu-id="e07a2-344">**/ DestKey:  *&lt;klíč účtu cíle&gt;:***  klíč účtu úložiště cílový účet úložiště.</span><span class="sxs-lookup"><span data-stu-id="e07a2-344">**/DestKey: *&lt;dest-account-key&gt;:*** Storage account key of the destination storage account.</span></span>
   * <span data-ttu-id="e07a2-345">**/ BlobType: stránka:** Určuje, že cíl je objekt blob stránky.</span><span class="sxs-lookup"><span data-stu-id="e07a2-345">**/BlobType: page:** Specifies that the destination is a page blob.</span></span>
   * <span data-ttu-id="e07a2-346">**/ Vzor:  *&lt;název souboru&gt;:***  zadejte název souboru virtuálního pevného disku ke kopírování.</span><span class="sxs-lookup"><span data-stu-id="e07a2-346">**/Pattern: *&lt;file-name&gt;:*** Specify the file name of the VHD to copy.</span></span>

<span data-ttu-id="e07a2-347">Podrobnosti o použití nástroje AzCopy nástroje najdete v tématu [přenos dat pomocí nástroje příkazového řádku Azcopy](storage-use-azcopy.md).</span><span class="sxs-lookup"><span data-stu-id="e07a2-347">For details on using AzCopy tool, see [Transfer data with the AzCopy Command-Line Utility](storage-use-azcopy.md).</span></span>

##### <a name="other-options-for-uploading-a-vhd"></a><span data-ttu-id="e07a2-348">Další možnosti pro odesílání virtuálního pevného disku</span><span class="sxs-lookup"><span data-stu-id="e07a2-348">Other options for uploading a VHD</span></span>
<span data-ttu-id="e07a2-349">Můžete také nahrát virtuální pevný disk na váš účet úložiště pomocí jedné z těchto způsobů:</span><span class="sxs-lookup"><span data-stu-id="e07a2-349">You can also upload a VHD to your storage account using one of the following means:</span></span>

* [<span data-ttu-id="e07a2-350">Objekt Blob služby Azure Storage kopie rozhraní API</span><span class="sxs-lookup"><span data-stu-id="e07a2-350">Azure Storage Copy Blob API</span></span>](https://msdn.microsoft.com/library/azure/dd894037.aspx)
* [<span data-ttu-id="e07a2-351">Objektů BLOB Azure Storage Explorer odesílání</span><span class="sxs-lookup"><span data-stu-id="e07a2-351">Azure Storage Explorer Uploading Blobs</span></span>](https://azurestorageexplorer.codeplex.com/)
* [<span data-ttu-id="e07a2-352">Referenční dokumentace rozhraní API úložiště importu/exportu služby REST</span><span class="sxs-lookup"><span data-stu-id="e07a2-352">Storage Import/Export Service REST API Reference</span></span>](https://msdn.microsoft.com/library/dn529096.aspx)

> [!NOTE]
> <span data-ttu-id="e07a2-353">Doporučujeme používat službu Import/Export, pokud je předpokládaná doba odesílání doba je delší než 7 dní.</span><span class="sxs-lookup"><span data-stu-id="e07a2-353">We recommend using Import/Export Service if estimated uploading time is longer than 7 days.</span></span> <span data-ttu-id="e07a2-354">Můžete použít [DataTransferSpeedCalculator](https://github.com/Azure-Samples/storage-dotnet-import-export-job-management/blob/master/DataTransferSpeedCalculator.html) k zjištění přibližné hodnoty času z jednotky velikost a přenášet data.</span><span class="sxs-lookup"><span data-stu-id="e07a2-354">You can use [DataTransferSpeedCalculator](https://github.com/Azure-Samples/storage-dotnet-import-export-job-management/blob/master/DataTransferSpeedCalculator.html) to estimate the time from data size and transfer unit.</span></span>
>
> <span data-ttu-id="e07a2-355">Import a Export lze použít pro kopírování na standardní účet úložiště.</span><span class="sxs-lookup"><span data-stu-id="e07a2-355">Import/Export can be used to copy to a standard storage account.</span></span> <span data-ttu-id="e07a2-356">Musíte zkopírovat z úložiště standard storage do prémiový účet úložiště pomocí nástroje, například AzCopy.</span><span class="sxs-lookup"><span data-stu-id="e07a2-356">You will need to copy from standard storage to premium storage account using a tool like AzCopy.</span></span>
>
>

## <span data-ttu-id="e07a2-357"><a name="create-azure-virtual-machine-using-premium-storage"></a>Vytvořit virtuální počítače Azure pomocí služby Storage úrovně Premium</span><span class="sxs-lookup"><span data-stu-id="e07a2-357"><a name="create-azure-virtual-machine-using-premium-storage"></a>Create Azure VMs using Premium Storage</span></span>
<span data-ttu-id="e07a2-358">Po virtuálního pevného disku se nahrál nebo zkopírovat do požadovaný účet úložiště, postupujte podle pokynů v této části k registraci virtuálního pevného disku jako bitovou kopii operačního systému, nebo disk operačního systému v závislosti na vašem scénáři a pak vytvořte instanci virtuálního počítače z něj.</span><span class="sxs-lookup"><span data-stu-id="e07a2-358">After the VHD is uploaded or copied to the desired storage account, follow the instructions in this section to register the VHD as an OS image, or OS disk depending on your scenario and then create a VM instance from it.</span></span> <span data-ttu-id="e07a2-359">Datový disk VHD lze připojit k virtuálnímu počítači po jeho vytvoření.</span><span class="sxs-lookup"><span data-stu-id="e07a2-359">The data disk VHD can be attached to the VM once it is created.</span></span>
<span data-ttu-id="e07a2-360">Ukázkový skript migrace je k dispozici na konci této části.</span><span class="sxs-lookup"><span data-stu-id="e07a2-360">A sample migration script is provided at the end of this section.</span></span> <span data-ttu-id="e07a2-361">Tento jednoduchý skript neodpovídá všechny scénáře.</span><span class="sxs-lookup"><span data-stu-id="e07a2-361">This simple script does not match all scenarios.</span></span> <span data-ttu-id="e07a2-362">Musíte aktualizovat skript tak, aby odpovídaly s konkrétní scénář.</span><span class="sxs-lookup"><span data-stu-id="e07a2-362">You may need to update the script to match with your specific scenario.</span></span> <span data-ttu-id="e07a2-363">Pokud chcete zobrazit, pokud se tento skript platí pro váš scénář, najdete níže [A ukázkový migrace skript](#a-sample-migration-script).</span><span class="sxs-lookup"><span data-stu-id="e07a2-363">To see if this script applies to your scenario, see below [A Sample Migration Script](#a-sample-migration-script).</span></span>

### <a name="checklist"></a><span data-ttu-id="e07a2-364">Kontrolní seznam</span><span class="sxs-lookup"><span data-stu-id="e07a2-364">Checklist</span></span>
1. <span data-ttu-id="e07a2-365">Počkat, až všechny disky VHD kopírování je dokončena.</span><span class="sxs-lookup"><span data-stu-id="e07a2-365">Wait until all the VHD disks copying is complete.</span></span>
2. <span data-ttu-id="e07a2-366">Ujistěte se, že úložiště Premium je k dispozici v oblast, kterou provádíte migraci do.</span><span class="sxs-lookup"><span data-stu-id="e07a2-366">Make sure Premium Storage is available in the region you are migrating to.</span></span>
3. <span data-ttu-id="e07a2-367">Rozhodněte, nové řadu virtuálních počítačů, kterou budete používat.</span><span class="sxs-lookup"><span data-stu-id="e07a2-367">Decide the new VM series you will be using.</span></span> <span data-ttu-id="e07a2-368">Měla by být schopný úložiště Premium a velikost by měla být v závislosti na dostupnosti v oblasti a na základě potřeb.</span><span class="sxs-lookup"><span data-stu-id="e07a2-368">It should be a Premium Storage capable, and the size should be depending on the availability in the region and based on your needs.</span></span>
4. <span data-ttu-id="e07a2-369">Rozhodněte, přesný velikost virtuálního počítače, které chcete použít.</span><span class="sxs-lookup"><span data-stu-id="e07a2-369">Decide the exact VM size you will use.</span></span> <span data-ttu-id="e07a2-370">Velikost virtuálního počítače musí být dostatečně velký pro podporu většímu počtu datových disků, které máte.</span><span class="sxs-lookup"><span data-stu-id="e07a2-370">VM size needs to be large enough to support the number of data disks you have.</span></span> <span data-ttu-id="e07a2-371">Například</span><span class="sxs-lookup"><span data-stu-id="e07a2-371">E.g.</span></span> <span data-ttu-id="e07a2-372">Pokud máte 4 datových disků, musí mít virtuální počítač 2 nebo více jader.</span><span class="sxs-lookup"><span data-stu-id="e07a2-372">if you have 4 data disks, the VM must have 2 or more cores.</span></span> <span data-ttu-id="e07a2-373">Zvažte také výpočetní výkon, paměti a šířky pásma sítě musí.</span><span class="sxs-lookup"><span data-stu-id="e07a2-373">Also, consider processing power, memory and network bandwidth needs.</span></span>
5. <span data-ttu-id="e07a2-374">Vytvořte účet úložiště Premium cílová oblast.</span><span class="sxs-lookup"><span data-stu-id="e07a2-374">Create a Premium Storage account in the target region.</span></span> <span data-ttu-id="e07a2-375">Toto je účet, který budete používat pro nový virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="e07a2-375">This is the account you will use for the new VM.</span></span>
6. <span data-ttu-id="e07a2-376">Máte aktuální podrobnosti o virtuálních počítačů, které jsou užitečné, včetně seznamu disků a odpovídající BLOB VHD.</span><span class="sxs-lookup"><span data-stu-id="e07a2-376">Have the current VM details handy, including the list of disks and corresponding VHD blobs.</span></span>

<span data-ttu-id="e07a2-377">Připravte aplikaci výpadek.</span><span class="sxs-lookup"><span data-stu-id="e07a2-377">Prepare your application for downtime.</span></span> <span data-ttu-id="e07a2-378">K provedení čisté migrace, budete muset zastavit veškeré zpracování v aktuálním systému.</span><span class="sxs-lookup"><span data-stu-id="e07a2-378">To do a clean migration, you have to stop all the processing in the current system.</span></span> <span data-ttu-id="e07a2-379">Pak můžete získat do konzistentního stavu, které můžete migrovat na nové platformě.</span><span class="sxs-lookup"><span data-stu-id="e07a2-379">Only then you can get it to consistent state which you can migrate to the new platform.</span></span> <span data-ttu-id="e07a2-380">Doba trvání výpadku závisí na množství dat na discích k migraci.</span><span class="sxs-lookup"><span data-stu-id="e07a2-380">Downtime duration will depend on the amount of data in the disks to migrate.</span></span>

> [!NOTE]
> <span data-ttu-id="e07a2-381">Pokud vytváříte virtuální počítač Azure Resource Manager z specializované disku VHD, získáte informace [této šablony](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vm-specialized-vhd) pro nasazení Resource Manager virtuálního počítače pomocí stávající disk.</span><span class="sxs-lookup"><span data-stu-id="e07a2-381">If you are creating an Azure Resource Manager VM from a specialized VHD Disk, please refer to [this template](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vm-specialized-vhd) for deploying Resource Manager VM using existing disk.</span></span>
>
>

### <a name="register-your-vhd"></a><span data-ttu-id="e07a2-382">Zaregistrovat svůj disk VHD</span><span class="sxs-lookup"><span data-stu-id="e07a2-382">Register your VHD</span></span>
<span data-ttu-id="e07a2-383">Vytvoření virtuálního počítače z virtuálního pevného disku operačního systému nebo na nový virtuální počítač připojit datový disk, je nutné je zaregistrovat.</span><span class="sxs-lookup"><span data-stu-id="e07a2-383">To create a VM from OS VHD or to attach a data disk to a new VM, you must first register them.</span></span> <span data-ttu-id="e07a2-384">Postupujte podle kroků v závislosti na scénáři svůj disk VHD.</span><span class="sxs-lookup"><span data-stu-id="e07a2-384">Follow steps below depending on your VHD's scenario.</span></span>

#### <a name="generalized-operating-system-vhd-to-create-multiple-azure-vm-instances"></a><span data-ttu-id="e07a2-385">Zobecněný virtuální pevný disk operačního systému vytvořit více instancí virtuálního počítače Azure</span><span class="sxs-lookup"><span data-stu-id="e07a2-385">Generalized Operating System VHD to create multiple Azure VM instances</span></span>
<span data-ttu-id="e07a2-386">Po odeslání image OS zobecněný virtuální pevný disk k účtu úložiště Zaregistrujte ho jako **Image virtuálního počítače Azure** tak, aby z ní můžete vytvořit jeden nebo více instancí virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="e07a2-386">After generalized OS image VHD is uploaded to the storage account, register it as an **Azure VM Image** so that you can create one or more VM instances from it.</span></span> <span data-ttu-id="e07a2-387">Pomocí následující rutiny prostředí PowerShell zaregistrovat svůj disk VHD jako image operačního systému virtuálního počítače Azure.</span><span class="sxs-lookup"><span data-stu-id="e07a2-387">Use the following PowerShell cmdlets to register your VHD as an Azure VM OS image.</span></span> <span data-ttu-id="e07a2-388">Zadejte adresu URL dokončení kontejneru, kde virtuálního pevného disku byla zkopírována do.</span><span class="sxs-lookup"><span data-stu-id="e07a2-388">Provide the complete container URL where VHD was copied to.</span></span>

```powershell
Add-AzureVMImage -ImageName "OSImageName" -MediaLocation "https://storageaccount.blob.core.windows.net/vhdcontainer/osimage.vhd" -OS Windows
```

<span data-ttu-id="e07a2-389">Zkopírujte a uložte název tuto novou bitovou kopii virtuálního počítače Azure.</span><span class="sxs-lookup"><span data-stu-id="e07a2-389">Copy and save the name of this new Azure VM Image.</span></span> <span data-ttu-id="e07a2-390">V předchozím příkladu je *OSImageName*.</span><span class="sxs-lookup"><span data-stu-id="e07a2-390">In the example above, it is *OSImageName*.</span></span>

#### <a name="unique-operating-system-vhd-to-create-a-single-azure-vm-instance"></a><span data-ttu-id="e07a2-391">Jedinečný operačního systému virtuálního pevného disku pro vytvoření jedné instance virtuálního počítače Azure</span><span class="sxs-lookup"><span data-stu-id="e07a2-391">Unique Operating System VHD to create a single Azure VM instance</span></span>
<span data-ttu-id="e07a2-392">Po odeslání jedinečný virtuální pevný disk operačního systému k účtu úložiště Zaregistrujte ho jako **Disk s operačním systémem Azure** tak, aby instance virtuálního počítače můžete vytvořit z něj.</span><span class="sxs-lookup"><span data-stu-id="e07a2-392">After the unique OS VHD is uploaded to the storage account, register it as an **Azure OS Disk** so that you can create a VM instance from it.</span></span> <span data-ttu-id="e07a2-393">Použijte tyto rutiny prostředí PowerShell k registraci svůj disk VHD jako Disk operačního systému Azure.</span><span class="sxs-lookup"><span data-stu-id="e07a2-393">Use these PowerShell cmdlets to register your VHD as an Azure OS Disk.</span></span> <span data-ttu-id="e07a2-394">Zadejte adresu URL dokončení kontejneru, kde virtuálního pevného disku byla zkopírována do.</span><span class="sxs-lookup"><span data-stu-id="e07a2-394">Provide the complete container URL where VHD was copied to.</span></span>

```powershell
Add-AzureDisk -DiskName "OSDisk" -MediaLocation "https://storageaccount.blob.core.windows.net/vhdcontainer/osdisk.vhd" -Label "My OS Disk" -OS "Windows"
```

<span data-ttu-id="e07a2-395">Zkopírujte a uložte název tento nový Disk operačního systému Azure.</span><span class="sxs-lookup"><span data-stu-id="e07a2-395">Copy and save the name of this new Azure OS Disk.</span></span> <span data-ttu-id="e07a2-396">V předchozím příkladu je *OSDisk*.</span><span class="sxs-lookup"><span data-stu-id="e07a2-396">In the example above, it is *OSDisk*.</span></span>

#### <a name="data-disk-vhd-to-be-attached-to-new-azure-vm-instances"></a><span data-ttu-id="e07a2-397">Data na disku VHD, který chcete připojit k nové instance virtuálního počítače Azure</span><span class="sxs-lookup"><span data-stu-id="e07a2-397">Data disk VHD to be attached to new Azure VM instance(s)</span></span>
<span data-ttu-id="e07a2-398">Po odeslání datový disk VHD k účtu úložiště zaregistrujte jako datový Disk Azure tak, aby se lze připojit k vaší nové DS Series, DSv2 series nebo GS virtuálního počítače Azure řady instance.</span><span class="sxs-lookup"><span data-stu-id="e07a2-398">After the data disk VHD is uploaded to storage account, register it as an Azure Data Disk so that it can be attached to your new DS Series, DSv2 series or GS Series Azure VM instance.</span></span>

<span data-ttu-id="e07a2-399">Použijte tyto rutiny prostředí PowerShell k registraci vaší virtuální pevný disk jako datový Disk Azure.</span><span class="sxs-lookup"><span data-stu-id="e07a2-399">Use these PowerShell cmdlets to register your VHD as an Azure Data Disk.</span></span> <span data-ttu-id="e07a2-400">Zadejte adresu URL dokončení kontejneru, kde virtuálního pevného disku byla zkopírována do.</span><span class="sxs-lookup"><span data-stu-id="e07a2-400">Provide the complete container URL where VHD was copied to.</span></span>

```powershell
Add-AzureDisk -DiskName "DataDisk" -MediaLocation "https://storageaccount.blob.core.windows.net/vhdcontainer/datadisk.vhd" -Label "My Data Disk"
```

<span data-ttu-id="e07a2-401">Zkopírujte a uložte název tento nový datový Disk Azure.</span><span class="sxs-lookup"><span data-stu-id="e07a2-401">Copy and save the name of this new Azure Data Disk.</span></span> <span data-ttu-id="e07a2-402">V předchozím příkladu je *DataDisk*.</span><span class="sxs-lookup"><span data-stu-id="e07a2-402">In the example above, it is *DataDisk*.</span></span>

### <a name="create-a-premium-storage-capable-vm"></a><span data-ttu-id="e07a2-403">Vytvoření virtuálního počítače s podporou služby Storage úrovně Premium</span><span class="sxs-lookup"><span data-stu-id="e07a2-403">Create a Premium Storage capable VM</span></span>
<span data-ttu-id="e07a2-404">Jednou image operačního systému nebo disk operačního systému jsou zaregistrovány, vytvořte nový DS-series, DSv2-series nebo GS-series virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="e07a2-404">Once the OS image or OS disk are registered, create a new DS-series, DSv2-series or GS-series VM.</span></span> <span data-ttu-id="e07a2-405">Budete používat image operačního systému nebo název disku operačního systému, který je zaregistrovaný.</span><span class="sxs-lookup"><span data-stu-id="e07a2-405">You will be using the operating system image or operating system disk name that you registered.</span></span> <span data-ttu-id="e07a2-406">Vyberte typ virtuálního počítače z vrstvy úložiště Premium.</span><span class="sxs-lookup"><span data-stu-id="e07a2-406">Select the VM type from the Premium Storage tier.</span></span> <span data-ttu-id="e07a2-407">V následujícím příkladu se používá *Standard_DS2* velikost virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="e07a2-407">In example below, we are using the *Standard_DS2* VM size.</span></span>

> [!NOTE]
> <span data-ttu-id="e07a2-408">Velikost disku a ujistěte se, že odpovídá požadavkům na výkon a velikosti disku Azure a vaše kapacita aktualizujte.</span><span class="sxs-lookup"><span data-stu-id="e07a2-408">Update the disk size to make sure it matches your capacity and performance requirements and the available Azure disk sizes.</span></span>
>
>

<span data-ttu-id="e07a2-409">Použijte rutiny Powershellu krok za krokem níže k vytvoření nového virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="e07a2-409">Follow the step by step PowerShell cmdlets below to create the new VM.</span></span> <span data-ttu-id="e07a2-410">Nastavte nejprve, běžné parametry:</span><span class="sxs-lookup"><span data-stu-id="e07a2-410">First, set the common parameters:</span></span>

```powershell
$serviceName = "yourVM"
$location = "location-name" (e.g., West US)
$vmSize ="Standard_DS2"
$adminUser = "youradmin"
$adminPassword = "yourpassword"
$vmName ="yourVM"
$vmSize = "Standard_DS2"
```

<span data-ttu-id="e07a2-411">Nejprve vytvořte Cloudová služba, ve kterém bude hostitelem nové virtuální počítače.</span><span class="sxs-lookup"><span data-stu-id="e07a2-411">First, create a cloud service in which you will be hosting your new VMs.</span></span>

```powershell
New-AzureService -ServiceName $serviceName -Location $location
```

<span data-ttu-id="e07a2-412">Dále vytvořte instanci virtuálního počítače Azure z bitové kopie operačního systému nebo Disk operačního systému, který je zaregistrovaný v závislosti na vašem scénáři.</span><span class="sxs-lookup"><span data-stu-id="e07a2-412">Next, depending on your scenario, create the Azure VM instance from either the OS Image or OS Disk that you registered.</span></span>

#### <a name="generalized-operating-system-vhd-to-create-multiple-azure-vm-instances"></a><span data-ttu-id="e07a2-413">Zobecněný virtuální pevný disk operačního systému vytvořit více instancí virtuálního počítače Azure</span><span class="sxs-lookup"><span data-stu-id="e07a2-413">Generalized Operating System VHD to create multiple Azure VM instances</span></span>
<span data-ttu-id="e07a2-414">Vytvořte jeden nebo více nový virtuální počítač Azure řady DS instance pomocí **bitovou kopii operačního systému služby Azure** který je zaregistrovaný.</span><span class="sxs-lookup"><span data-stu-id="e07a2-414">Create the one or more new DS Series Azure VM instances using the **Azure OS Image** that you registered.</span></span> <span data-ttu-id="e07a2-415">Při vytváření nového virtuálního počítače, jak je uvedeno níže, zadejte název této bitové kopie operačního systému v konfiguraci virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="e07a2-415">Specify this OS Image name in the VM configuration when creating new VM as shown below.</span></span>

```powershell
$OSImage = Get-AzureVMImage –ImageName "OSImageName"

$vm = New-AzureVMConfig -Name $vmName –InstanceSize $vmSize -ImageName $OSImage.ImageName

Add-AzureProvisioningConfig -Windows –AdminUserName $adminUser -Password $adminPassword –VM $vm

New-AzureVM -ServiceName $serviceName -VM $vm
```

#### <a name="unique-operating-system-vhd-to-create-a-single-azure-vm-instance"></a><span data-ttu-id="e07a2-416">Jedinečný operačního systému virtuálního pevného disku pro vytvoření jedné instance virtuálního počítače Azure</span><span class="sxs-lookup"><span data-stu-id="e07a2-416">Unique Operating System VHD to create a single Azure VM instance</span></span>
<span data-ttu-id="e07a2-417">Vytvoření nové služby DS řady virtuálního počítače Azure instance pomocí **Disk s operačním systémem Azure** který je zaregistrovaný.</span><span class="sxs-lookup"><span data-stu-id="e07a2-417">Create a new DS series Azure VM instance using the **Azure OS Disk** that you registered.</span></span> <span data-ttu-id="e07a2-418">Při vytváření nového virtuálního počítače, jak je uvedeno níže, zadejte tento název disku operačního systému v konfiguraci virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="e07a2-418">Specify this OS Disk name in the VM configuration when creating the new VM as shown below.</span></span>

```powershell
$OSDisk = Get-AzureDisk –DiskName "OSDisk"

$vm = New-AzureVMConfig -Name $vmName -InstanceSize $vmSize -DiskName $OSDisk.DiskName

New-AzureVM -ServiceName $serviceName –VM $vm
```

<span data-ttu-id="e07a2-419">Zadejte další virtuální počítač Azure informace, například cloudové služby, oblast, účet úložiště, skupinu dostupnosti a zásady ukládání do mezipaměti.</span><span class="sxs-lookup"><span data-stu-id="e07a2-419">Specify other Azure VM information, such as a cloud service, region, storage account, availability set, and caching policy.</span></span> <span data-ttu-id="e07a2-420">Všimněte si, že instance virtuálního počítače musí být umístěné společně se službou přidružené operační systém nebo datové disky, takže účet vybranou cloudovou službu, oblast a úložiště musí být ve stejném umístění jako základní virtuální pevné disky, a těchto disků.</span><span class="sxs-lookup"><span data-stu-id="e07a2-420">Note that the VM instance must be co-located with associated operating system or data disks, so the selected cloud service, region and storage account must all be in the same location as the underlying VHDs of those disks.</span></span>

### <a name="attach-data-disk"></a><span data-ttu-id="e07a2-421">Připojení datového disku</span><span class="sxs-lookup"><span data-stu-id="e07a2-421">Attach data disk</span></span>
<span data-ttu-id="e07a2-422">Nakonec pokud jste si zaregistrovali datový disk VHD, připojte je k nové úložiště Premium podporující virtuálního počítače Azure.</span><span class="sxs-lookup"><span data-stu-id="e07a2-422">Lastly, if you have registered data disk VHDs, attach them to the new Premium Storage capable Azure VM.</span></span>

<span data-ttu-id="e07a2-423">Pomocí následující rutiny prostředí PowerShell připojit datový disk do nového virtuálního počítače a zadejte zásady ukládání do mezipaměti.</span><span class="sxs-lookup"><span data-stu-id="e07a2-423">Use following PowerShell cmdlet to attach data disk to the new VM and specify the caching policy.</span></span> <span data-ttu-id="e07a2-424">V následujícím příkladu je ukládání do mezipaměti zásada nastavená *jen pro čtení*.</span><span class="sxs-lookup"><span data-stu-id="e07a2-424">In example below the caching policy is set to *ReadOnly*.</span></span>

```powershell
$vm = Get-AzureVM -ServiceName $serviceName -Name $vmName

Add-AzureDataDisk -ImportFrom -DiskName "DataDisk" -LUN 0 –HostCaching ReadOnly –VM $vm

Update-AzureVM  -VM $vm
```

> [!NOTE]
> <span data-ttu-id="e07a2-425">Mohou existovat další kroky nezbytné pro podporu vaší aplikace, která je nesmí být zahrnuté v této příručce.</span><span class="sxs-lookup"><span data-stu-id="e07a2-425">There may be additional steps necessary to support your application that is not be covered by this guide.</span></span>
>
>

### <a name="checking-and-plan-backup"></a><span data-ttu-id="e07a2-426">Kontrola a plánování zálohování</span><span class="sxs-lookup"><span data-stu-id="e07a2-426">Checking and plan backup</span></span>
<span data-ttu-id="e07a2-427">Jakmile nový virtuální počítač je v provozu, přístup pomocí stejné přihlašovacího id a heslo je jako původní virtuální počítač a ověřte, že vše funguje podle očekávání.</span><span class="sxs-lookup"><span data-stu-id="e07a2-427">Once the new VM is up and running, access it using the same login id and password is as the original VM, and verify that everything is working as expected.</span></span> <span data-ttu-id="e07a2-428">Nový virtuální počítač bude obsahovat všechna nastavení, včetně prokládané svazky.</span><span class="sxs-lookup"><span data-stu-id="e07a2-428">All the settings, including the striped volumes, would be present in the new VM.</span></span>

<span data-ttu-id="e07a2-429">Posledním krokem je plánování zálohování a plán údržby pro nový virtuální počítač podle potřeb aplikace.</span><span class="sxs-lookup"><span data-stu-id="e07a2-429">The last step is to plan backup and maintenance schedule for the new VM based on the application's needs.</span></span>

### <span data-ttu-id="e07a2-430"><a name="a-sample-migration-script"></a>Ukázkový skript migrace</span><span class="sxs-lookup"><span data-stu-id="e07a2-430"><a name="a-sample-migration-script"></a>A sample migration script</span></span>
<span data-ttu-id="e07a2-431">Pokud máte víc virtuálních počítačů k migraci, bude užitečné automatizace prostřednictvím skriptů prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="e07a2-431">If you have multiple VMs to migrate, automation through PowerShell scripts will be helpful.</span></span> <span data-ttu-id="e07a2-432">Následuje ukázkový skript, který zautomatizuje migrace virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="e07a2-432">Following is a sample script that automates the migration of a VM.</span></span> <span data-ttu-id="e07a2-433">Poznámka: níže uvedený skript se pouze příklad a neexistují několik předpokladům o aktuální disky virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="e07a2-433">Note that below script is only an example, and there are few assumptions made about the current VM disks.</span></span> <span data-ttu-id="e07a2-434">Musíte aktualizovat skript tak, aby odpovídaly s konkrétní scénář.</span><span class="sxs-lookup"><span data-stu-id="e07a2-434">You may need to update the script to match with your specific scenario.</span></span>

<span data-ttu-id="e07a2-435">Předpoklady jsou:</span><span class="sxs-lookup"><span data-stu-id="e07a2-435">The assumptions are:</span></span>

* <span data-ttu-id="e07a2-436">Vytváříte klasické virtuální počítače Azure.</span><span class="sxs-lookup"><span data-stu-id="e07a2-436">You are creating classic Azure VMs.</span></span>
* <span data-ttu-id="e07a2-437">Vaše zdrojové disky operačního systému a datové disky zdroje jsou ve stejném účtu úložiště a kontejneru.</span><span class="sxs-lookup"><span data-stu-id="e07a2-437">Your source OS Disks and source Data Disks are in same storage account and same container.</span></span> <span data-ttu-id="e07a2-438">Pokud vaše disky operačního systému a datové disky nejsou na stejném místě, můžete použít ke kopírování disků VHD přes účty úložiště a kontejnery AzCopy nebo Azure PowerShell.</span><span class="sxs-lookup"><span data-stu-id="e07a2-438">If your OS Disks and Data Disks are not in the same place, you can use AzCopy or Azure PowerShell to copy VHDs over storage accounts and containers.</span></span> <span data-ttu-id="e07a2-439">Najdete v předchozím kroku: [kopie virtuálního pevného disku s AzCopy nebo prostředí PowerShell](#copy-vhd-with-azcopy-or-powershell).</span><span class="sxs-lookup"><span data-stu-id="e07a2-439">Refer to the previous step: [Copy VHD with AzCopy or PowerShell](#copy-vhd-with-azcopy-or-powershell).</span></span> <span data-ttu-id="e07a2-440">Úpravy tento skript ke splnění váš scénář je další možnost, ale doporučujeme použít AzCopy nebo prostředí PowerShell, protože je jednodušší a rychlejší.</span><span class="sxs-lookup"><span data-stu-id="e07a2-440">Editing this script to meet your scenario is another choice, but we recommend using AzCopy or PowerShell since it is easier and faster.</span></span>

<span data-ttu-id="e07a2-441">Skriptu pro automatizaci najdete níže.</span><span class="sxs-lookup"><span data-stu-id="e07a2-441">The automation script is provided below.</span></span> <span data-ttu-id="e07a2-442">Nahradí text s informacemi a skript tak, aby odpovídaly s konkrétní scénář aktualizovat.</span><span class="sxs-lookup"><span data-stu-id="e07a2-442">Replace text with your information and update the script to match with your specific scenario.</span></span>

> [!NOTE]
> <span data-ttu-id="e07a2-443">Pomocí stávající skript nezachovává síťovou konfiguraci vašeho zdrojového virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="e07a2-443">Using the existing script does not preserve the network configuration of your source VM.</span></span> <span data-ttu-id="e07a2-444">Musíte opětovná konfigurace nastavení sítě na migrovaných virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="e07a2-444">You will need to re-config the networking settings on your migrated VMs.</span></span>
>
>

```
    <#
    .Synopsis
    This script is provided as an EXAMPLE to show how to migrate a VM from a standard storage account to a premium storage account. You can customize it according to your specific requirements.

    .Description
    The script will copy the vhds (page blobs) of the source VM to the new storage account.
    And then it will create a new VM from these copied vhds based on the inputs that you specified for the new VM.
    You can modify the script to satisfy your specific requirement, but please be aware of the items specified
    in the Terms of Use section.

    .Terms of Use
    Copyright © 2015 Microsoft Corporation.  All rights reserved.

    THIS CODE AND ANY ASSOCIATED INFORMATION ARE PROVIDED "AS IS" WITHOUT WARRANTY OF ANY KIND,
    EITHER EXPRESSED OR IMPLIED, INCLUDING BUT NOT LIMITED TO THE IMPLIED WARRANTIES OF MERCHANTABILITY
    AND/OR FITNESS FOR A PARTICULAR PURPOSE. THE ENTIRE RISK OF USE, INABILITY TO USE, OR
    RESULTS FROM THE USE OF THIS CODE REMAINS WITH THE USER.

    .Example (Save this script as Migrate-AzureVM.ps1)

    .\Migrate-AzureVM.ps1 -SourceServiceName CurrentServiceName -SourceVMName CurrentVMName –DestStorageAccount newpremiumstorageaccount -DestServiceName NewServiceName -DestVMName NewDSVMName -DestVMSize "Standard_DS2" –Location "Southeast Asia"

    .Link
    To find more information about how to set up Azure PowerShell, refer to the following links.
    http://azure.microsoft.com/documentation/articles/powershell-install-configure/
    http://azure.microsoft.com/documentation/articles/storage-powershell-guide-full/
    http://azure.microsoft.com/blog/2014/10/22/migrate-azure-virtual-machines-between-storage-accounts/

    #>

    Param(
    # the cloud service name of the VM.
    [Parameter(Mandatory = $true)]
    [string] $SourceServiceName,

    # The VM name to copy.
    [Parameter(Mandatory = $true)]
    [String] $SourceVMName,

    # The destination storage account name.
    [Parameter(Mandatory = $true)]
    [String] $DestStorageAccount,

    # The destination cloud service name
    [Parameter(Mandatory = $true)]
    [String] $DestServiceName,

    # the destination vm name
    [Parameter(Mandatory = $true)]
    [String] $DestVMName,

    # the destination vm size
    [Parameter(Mandatory = $true)]
    [String] $DestVMSize,

    # the location of destination VM.
    [Parameter(Mandatory = $true)]
    [string] $Location,

    # whether or not to copy the os disk, the default is only copy data disks
    [Parameter(Mandatory = $false)]
    [Bool] $DataDiskOnly = $true,

    # how frequently to report the copy status in sceconds
    [Parameter(Mandatory = $false)]
    [Int32] $CopyStatusReportInterval = 15,

    # the name suffix to add to new created disks to avoid conflict with source disk names
    [Parameter(Mandatory = $false)]
    [String]$DiskNameSuffix = "-prem"

    ) #end param

    #######################################################################
    #  Verify Azure PowerShell module and version
    #######################################################################

    #import the Azure PowerShell module
    Write-Host "`n[WORKITEM] - Importing Azure PowerShell module" -ForegroundColor Yellow
    $azureModule = Import-Module Azure -PassThru

    if ($azureModule -ne $null)
    {
        Write-Host "`tSuccess" -ForegroundColor Green
    }
    else
    {
        #show module not found interaction and bail out
        Write-Host "[ERROR] - PowerShell module not found. Exiting." -ForegroundColor Red
        Exit
    }


    #Check the Azure PowerShell module version
    Write-Host "`n[WORKITEM] - Checking Azure PowerShell module verion" -ForegroundColor Yellow
    If ($azureModule.Version -ge (New-Object System.Version -ArgumentList "0.8.14"))
    {
        Write-Host "`tSuccess" -ForegroundColor Green
    }
    Else
    {
        Write-Host "[ERROR] - Azure PowerShell module must be version 0.8.14 or higher. Exiting." -ForegroundColor Red
        Exit
    }

    #Check if there is an azure subscription set up in PowerShell
    Write-Host "`n[WORKITEM] - Checking Azure Subscription" -ForegroundColor Yellow
    $currentSubs = Get-AzureSubscription -Current
    if ($currentSubs -ne $null)
    {
        Write-Host "`tSuccess" -ForegroundColor Green
        Write-Host "`tYour current azure subscription in PowerShell is $($currentSubs.SubscriptionName)." -ForegroundColor Green
    }
    else
    {
        Write-Host "[ERROR] - There is no valid Azure subscription found in PowerShell. Please refer to this article http://azure.microsoft.com/documentation/articles/powershell-install-configure/ to connect an Azure subscription. Exiting." -ForegroundColor Red
        Exit
    }


    #######################################################################
    #  Check if the VM is shut down
    #  Stopping the VM is a required step so that the file system is consistent when you do the copy operation.
    #  Azure does not support live migration at this time..
    #######################################################################

    if (($sourceVM = Get-AzureVM –ServiceName $SourceServiceName –Name $SourceVMName) -eq $null)
    {
        Write-Host "[ERROR] - The source VM doesn't exist in the current subscription. Exiting." -ForegroundColor Red
        Exit
    }

    # check if VM is shut down
    if ( $sourceVM.Status -notmatch "Stopped" )
    {
        Write-Host "[Warning] - Stopping the VM is a required step so that the file system is consistent when you do the copy operation. Azure does not support live migration at this time. If you'd like to create a VM from a generalized image, sys-prep the Virtual Machine before stopping it." -ForegroundColor Yellow
        $ContinueAnswer = Read-Host "`n`tDo you wish to stop $SourceVMName now? Input 'N' if you want to shut down the VM manually and come back later.(Y/N)"
        If ($ContinueAnswer -ne "Y") { Write-Host "`n Exiting." -ForegroundColor Red;Exit }
        $sourceVM | Stop-AzureVM

        # wait until the VM is shut down
        $VMStatus = (Get-AzureVM –ServiceName $SourceServiceName –Name $vmName).Status
        while ($VMStatus -notmatch "Stopped")
        {
            Write-Host "`n[Status] - Waiting VM $vmName to shut down" -ForegroundColor Green
            Sleep -Seconds 5
            $VMStatus = (Get-AzureVM –ServiceName $SourceServiceName –Name $vmName).Status
        }
    }

    # exporting the sourve vm to a configuration file, you can restore the original VM by importing this config file
    # see more information for Import-AzureVM
    $workingDir = (Get-Location).Path
    $vmConfigurationPath = $env:HOMEPATH + "\VM-" + $SourceVMName + ".xml"
    Write-Host "`n[WORKITEM] - Exporting VM configuration to $vmConfigurationPath" -ForegroundColor Yellow
    $exportRe = $sourceVM | Export-AzureVM -Path $vmConfigurationPath


    #######################################################################
    #  Copy the vhds of the source vm
    #  You can choose to copy all disks including os and data disks by specifying the
    #  parameter -DataDiskOnly to be $false. The default is to copy only data disk vhds
    #  and the new VM will boot from the original os disk.
    #######################################################################

    $sourceOSDisk = $sourceVM.VM.OSVirtualHardDisk
    $sourceDataDisks = $sourceVM.VM.DataVirtualHardDisks

    # Get source storage account information, not considering the data disks and os disks are in different accounts
    $sourceStorageAccountName = $sourceOSDisk.MediaLink.Host -split "\." | select -First 1
    $sourceStorageKey = (Get-AzureStorageKey -StorageAccountName $sourceStorageAccountName).Primary
    $sourceContext = New-AzureStorageContext –StorageAccountName $sourceStorageAccountName -StorageAccountKey $sourceStorageKey

    # Create destination context
    $destStorageKey = (Get-AzureStorageKey -StorageAccountName $DestStorageAccount).Primary
    $destContext = New-AzureStorageContext –StorageAccountName $DestStorageAccount -StorageAccountKey $destStorageKey

    # Create a container of vhds if it doesn't exist
    if ((Get-AzureStorageContainer -Context $destContext -Name vhds -ErrorAction SilentlyContinue) -eq $null)
    {
        Write-Host "`n[WORKITEM] - Creating a container vhds in the destination storage account." -ForegroundColor Yellow
        New-AzureStorageContainer -Context $destContext -Name vhds
    }


    $allDisksToCopy = $sourceDataDisks
    # check if need to copy os disk
    $sourceOSVHD = $sourceOSDisk.MediaLink.Segments[2]
    if ($DataDiskOnly)
    {
        # copy data disks only, this option requires deleting the source VM so that dest VM can boot
        # from the same vhd blob.
        $ContinueAnswer = Read-Host "`n`t[Warning] You chose to copy data disks only. Moving VM requires removing the original VM (the disks and backing vhd files will NOT be deleted) so that the new VM can boot from the same vhd. This is an irreversible action. Do you wish to proceed right now? (Y/N)"
        If ($ContinueAnswer -ne "Y") { Write-Host "`n Exiting." -ForegroundColor Red;Exit }
        $destOSVHD = Get-AzureStorageBlob -Blob $sourceOSVHD -Container vhds -Context $sourceContext
        Write-Host "`n[WORKITEM] - Removing the original VM (the vhd files are NOT deleted)." -ForegroundColor Yellow
        Remove-AzureVM -Name $SourceVMName -ServiceName $SourceServiceName

        Write-Host "`n[WORKITEM] - Waiting utill the OS disk is released by source VM. This may take up to several minutes."
        $diskAttachedTo = (Get-AzureDisk -DiskName $sourceOSDisk.DiskName).AttachedTo
        while ($diskAttachedTo -ne $null)
        {
            Start-Sleep -Seconds 10
            $diskAttachedTo = (Get-AzureDisk -DiskName $sourceOSDisk.DiskName).AttachedTo
        }

    }
    else
    {
        # copy the os disk vhd
        Write-Host "`n[WORKITEM] - Starting copying os disk $($disk.DiskName) at $(get-date)." -ForegroundColor Yellow
        $allDisksToCopy += @($sourceOSDisk)
        $targetBlob = Start-AzureStorageBlobCopy -SrcContainer vhds -SrcBlob $sourceOSVHD -DestContainer vhds -DestBlob $sourceOSVHD -Context $sourceContext -DestContext $destContext -Force
        $destOSVHD = $targetBlob
    }


    # Copy all data disk vhds
    # Start all async copy requests in parallel.
    foreach($disk in $sourceDataDisks)
    {
        $blobName = $disk.MediaLink.Segments[2]
        # copy all data disks
        Write-Host "`n[WORKITEM] - Starting copying data disk $($disk.DiskName) at $(get-date)." -ForegroundColor Yellow
        $targetBlob = Start-AzureStorageBlobCopy -SrcContainer vhds -SrcBlob $blobName -DestContainer vhds -DestBlob $blobName -Context $sourceContext -DestContext $destContext -Force
        # update the media link to point to the target blob link
        $disk.MediaLink = $targetBlob.ICloudBlob.Uri.AbsoluteUri
    }

    # Wait until all vhd files are copied.
    $diskComplete = @()
    do
    {
        Write-Host "`n[WORKITEM] - Waiting for all disk copy to complete. Checking status every $CopyStatusReportInterval seconds." -ForegroundColor Yellow
        # check status every 30 seconds
        Sleep -Seconds $CopyStatusReportInterval
        foreach ( $disk in $allDisksToCopy)
        {
            if ($diskComplete -contains $disk)
            {
                Continue
            }
            $blobName = $disk.MediaLink.Segments[2]
            $copyState = Get-AzureStorageBlobCopyState -Blob $blobName -Container vhds -Context $destContext
            if ($copyState.Status -eq "Success")
            {
                Write-Host "`n[Status] - Success for disk copy $($disk.DiskName) at $($copyState.CompletionTime)" -ForegroundColor Green
                $diskComplete += $disk
            }
            else
            {
                if ($copyState.TotalBytes -gt 0)
                {
                    $percent = ($copyState.BytesCopied / $copyState.TotalBytes) * 100
                    Write-Host "`n[Status] - $('{0:N2}' -f $percent)% Complete for disk copy $($disk.DiskName)" -ForegroundColor Green
                }
            }
        }
    }
    while($diskComplete.Count -lt $allDisksToCopy.Count)

    #######################################################################
    #  Create a new vm
    #  the new VM can be created from the copied disks or the original os disk.
    #  You can ddd your own logic here to satisfy your specific requirements of the vm.
    #######################################################################

    # Create a VM from the existing os disk
    if ($DataDiskOnly)
    {
        $vm = New-AzureVMConfig -Name $DestVMName -InstanceSize $DestVMSize -DiskName $sourceOSDisk.DiskName
    }
    else
    {
        $newOSDisk = Add-AzureDisk -OS $sourceOSDisk.OS -DiskName ($sourceOSDisk.DiskName + $DiskNameSuffix) -MediaLocation $destOSVHD.ICloudBlob.Uri.AbsoluteUri
        $vm = New-AzureVMConfig -Name $DestVMName -InstanceSize $DestVMSize -DiskName $newOSDisk.DiskName
    }
    # Attached the copied data disks to the new VM
    foreach ($dataDisk in $sourceDataDisks)
    {
        # add -DiskLabel $dataDisk.DiskLabel if there are labels for disks of the source vm
        $diskLabel = "drive" + $dataDisk.Lun
        $vm | Add-AzureDataDisk -ImportFrom -DiskLabel $diskLabel -LUN $dataDisk.Lun -MediaLocation $dataDisk.MediaLink
    }

    # Edit this if you want to add more custimization to the new VM
    # $vm | Add-AzureEndpoint -Protocol tcp -LocalPort 443 -PublicPort 443 -Name 'HTTPs'
    # $vm | Set-AzureSubnet "PubSubnet","PrivSubnet"

    New-AzureVM -ServiceName $DestServiceName -VMs $vm -Location $Location
```

#### <span data-ttu-id="e07a2-445"><a name="optimization"></a>Optimalizace</span><span class="sxs-lookup"><span data-stu-id="e07a2-445"><a name="optimization"></a>Optimization</span></span>
<span data-ttu-id="e07a2-446">Aktuální konfigurace virtuálních počítačů lze přizpůsobit určený speciálně pro funkce fungují dobře u standardní disky.</span><span class="sxs-lookup"><span data-stu-id="e07a2-446">Your current VM configuration may be customized specifically to work well with Standard disks.</span></span> <span data-ttu-id="e07a2-447">Například pokud chcete zvýšit výkon pomocí mnoho disků v prokládaný svazek.</span><span class="sxs-lookup"><span data-stu-id="e07a2-447">For instance, to increase the performance by using many disks in a striped volume.</span></span> <span data-ttu-id="e07a2-448">Například místo použití 4 disky samostatně na Storage úrovně Premium, možná nebudete moct optimalizovat náklady tak, že jeden disk.</span><span class="sxs-lookup"><span data-stu-id="e07a2-448">For example, instead of using 4 disks separately on Premium Storage, you may be able to optimize the cost by having a single disk.</span></span> <span data-ttu-id="e07a2-449">Optimalizace jako tento potřeba zpracovávat případ od případu a vyžadují vlastní kroky po migraci.</span><span class="sxs-lookup"><span data-stu-id="e07a2-449">Optimizations like this need to be handled on a case by case basis and require custom steps after the migration.</span></span> <span data-ttu-id="e07a2-450">Všimněte si také, že tento proces nemusí fungovat i pro databáze a aplikace, které závisí na definované v nastavení rozložení disku.</span><span class="sxs-lookup"><span data-stu-id="e07a2-450">Also, note that this process may not well work for databases and applications that depend on the disk layout defined in the setup.</span></span>

##### <a name="preparation"></a><span data-ttu-id="e07a2-451">Příprava</span><span class="sxs-lookup"><span data-stu-id="e07a2-451">Preparation</span></span>
1. <span data-ttu-id="e07a2-452">Dokončení migrace jednoduchý, jak je popsáno v předchozí části.</span><span class="sxs-lookup"><span data-stu-id="e07a2-452">Complete the Simple Migration as described in the earlier section.</span></span> <span data-ttu-id="e07a2-453">Optimalizace se provede na nový virtuální počítač po dokončení migrace.</span><span class="sxs-lookup"><span data-stu-id="e07a2-453">Optimizations will be performed on the new VM after the migration.</span></span>
2. <span data-ttu-id="e07a2-454">Zadejte nové velikosti disků, které jsou potřebné pro konfiguraci optimalizované.</span><span class="sxs-lookup"><span data-stu-id="e07a2-454">Define the new disk sizes needed for the optimized configuration.</span></span>
3. <span data-ttu-id="e07a2-455">Určí, mapování na nový disk specifikace aktuální disky nebo svazky.</span><span class="sxs-lookup"><span data-stu-id="e07a2-455">Determine mapping of the current disks/volumes to the new disk specifications.</span></span>

##### <a name="execution-steps"></a><span data-ttu-id="e07a2-456">Provedení kroků</span><span class="sxs-lookup"><span data-stu-id="e07a2-456">Execution steps</span></span>
1. <span data-ttu-id="e07a2-457">Vytvořte nový disků se správnými velikostmi ve virtuálním počítači úložiště Premium.</span><span class="sxs-lookup"><span data-stu-id="e07a2-457">Create the new disks with the right sizes on the Premium Storage VM.</span></span>
2. <span data-ttu-id="e07a2-458">Přihlášení k virtuálních počítačů a zkopírujte data z aktuální svazek na nový disk, který se mapuje na tomto svazku.</span><span class="sxs-lookup"><span data-stu-id="e07a2-458">Login to the VM and copy the data from the current volume to the new disk that maps to that volume.</span></span> <span data-ttu-id="e07a2-459">To lze proveďte pro všechny aktuální svazky, které potřebují k mapování na nový disk.</span><span class="sxs-lookup"><span data-stu-id="e07a2-459">Do this for all the current volumes that need to map to a new disk.</span></span>
3. <span data-ttu-id="e07a2-460">V dalším kroku změnit nastavení aplikace, přejděte na nové disky a odpojit starých svazků.</span><span class="sxs-lookup"><span data-stu-id="e07a2-460">Next, change the application settings to switch to the new disks, and detach the old volumes.</span></span>

<span data-ttu-id="e07a2-461">Ladění aplikace pro lepší výkon disku, naleznete v [optimalizace výkonu aplikace](storage-premium-storage-performance.md#optimizing-application-performance).</span><span class="sxs-lookup"><span data-stu-id="e07a2-461">For tuning the application for better disk performance, please refer to [Optimizing Application Performance](storage-premium-storage-performance.md#optimizing-application-performance).</span></span>

### <a name="application-migrations"></a><span data-ttu-id="e07a2-462">Migrace aplikací</span><span class="sxs-lookup"><span data-stu-id="e07a2-462">Application migrations</span></span>
<span data-ttu-id="e07a2-463">Databáze a jiné komplexní aplikace může vyžadovat zvláštní postup podle definice poskytovatele aplikace pro migraci.</span><span class="sxs-lookup"><span data-stu-id="e07a2-463">Databases and other complex applications may require special steps as defined by the application provider for the migration.</span></span> <span data-ttu-id="e07a2-464">Naleznete v dokumentaci k příslušné aplikace.</span><span class="sxs-lookup"><span data-stu-id="e07a2-464">Please refer to respective application documentation.</span></span> <span data-ttu-id="e07a2-465">Například</span><span class="sxs-lookup"><span data-stu-id="e07a2-465">E.g.</span></span> <span data-ttu-id="e07a2-466">Obvykle můžete migrovat databází pomocí zálohování a obnovení.</span><span class="sxs-lookup"><span data-stu-id="e07a2-466">typically databases can be migrated through backup and restore.</span></span>

## <a name="next-steps"></a><span data-ttu-id="e07a2-467">Další kroky</span><span class="sxs-lookup"><span data-stu-id="e07a2-467">Next steps</span></span>
<span data-ttu-id="e07a2-468">Najdete v následujících materiálech u konkrétních scénářů pro migraci virtuálních počítačů:</span><span class="sxs-lookup"><span data-stu-id="e07a2-468">See the following resources for specific scenarios for migrating virtual machines:</span></span>

* [<span data-ttu-id="e07a2-469">Migrovat virtuální počítače, které jsou mezi účty úložiště Azure</span><span class="sxs-lookup"><span data-stu-id="e07a2-469">Migrate Azure Virtual Machines between Storage Accounts</span></span>](https://azure.microsoft.com/blog/2014/10/22/migrate-azure-virtual-machines-between-storage-accounts/)
* [<span data-ttu-id="e07a2-470">Vytvoření a nahrání virtuálního pevného disku serveru Windows do Azure.</span><span class="sxs-lookup"><span data-stu-id="e07a2-470">Create and upload a Windows Server VHD to Azure.</span></span>](../virtual-machines/windows/classic/createupload-vhd.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json)
* [<span data-ttu-id="e07a2-471">Vytváření a odesílání virtuální pevný Disk, který obsahuje operační systém Linux</span><span class="sxs-lookup"><span data-stu-id="e07a2-471">Creating and Uploading a Virtual Hard Disk that Contains the Linux Operating System</span></span>](../virtual-machines/linux/classic/create-upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json)
* [<span data-ttu-id="e07a2-472">Migrace virtuálních počítačů z Amazon AWS k Microsoft Azure</span><span class="sxs-lookup"><span data-stu-id="e07a2-472">Migrating Virtual Machines from Amazon AWS to Microsoft Azure</span></span>](http://channel9.msdn.com/Series/Migrating-Virtual-Machines-from-Amazon-AWS-to-Microsoft-Azure)

<span data-ttu-id="e07a2-473">Zkontrolujte také, další informace o Azure Storage a virtuální počítače Azure v následujících zdrojích:</span><span class="sxs-lookup"><span data-stu-id="e07a2-473">Also, see the following resources to learn more about Azure Storage and Azure Virtual Machines:</span></span>

* [<span data-ttu-id="e07a2-474">Azure Storage</span><span class="sxs-lookup"><span data-stu-id="e07a2-474">Azure Storage</span></span>](https://azure.microsoft.com/documentation/services/storage/)
* [<span data-ttu-id="e07a2-475">Virtuální počítače Azure</span><span class="sxs-lookup"><span data-stu-id="e07a2-475">Azure Virtual Machines</span></span>](https://azure.microsoft.com/documentation/services/virtual-machines/)
* [<span data-ttu-id="e07a2-476">Storage úrovně Premium: Vysoce výkonné úložiště pro úlohy virtuálních počítačů Azure</span><span class="sxs-lookup"><span data-stu-id="e07a2-476">Premium Storage: High-Performance Storage for Azure Virtual Machine Workloads</span></span>](storage-premium-storage.md)

[1]:./media/storage-migration-to-premium-storage/migration-to-premium-storage-1.png
[2]:./media/storage-migration-to-premium-storage/migration-to-premium-storage-1.png
[3]:./media/storage-migration-to-premium-storage/migration-to-premium-storage-3.png
[4]: http://technet.microsoft.com/library/hh831739.aspx
