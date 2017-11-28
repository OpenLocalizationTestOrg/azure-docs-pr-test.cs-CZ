---
title: "virtuální počítače tooAzure aaaMigrating Premium Storage | Microsoft Docs"
description: "Migrujte existující virtuální počítače tooAzure Storage úrovně Premium. Premium Storage nabízí podporu vysoce výkonné, nízkou latencí disku pro I náročnými úlohy běžící na virtuálních počítačích Azure."
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
ms.openlocfilehash: 19aaf2b7594e570f5a964baa00958a7a8eaae97b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="migrating-tooazure-premium-storage-unmanaged-disks"></a><span data-ttu-id="3d3b4-104">Migrace tooAzure Storage úrovně Premium (nespravované disků)</span><span class="sxs-lookup"><span data-stu-id="3d3b4-104">Migrating tooAzure Premium Storage (Unmanaged Disks)</span></span>

> [!NOTE]
> <span data-ttu-id="3d3b4-105">Tento článek popisuje, jak toomigrate virtuální počítač, který používá tooa nespravované standardní disky virtuálního počítače, který používá nespravované prémiové disky.</span><span class="sxs-lookup"><span data-stu-id="3d3b4-105">This article discusses how toomigrate a VM that uses unmanaged standard disks tooa VM that uses unmanaged premium disks.</span></span> <span data-ttu-id="3d3b4-106">Doporučujeme použít Azure spravované disky pro nové virtuální počítače a převést vaše předchozí toomanaged disky nespravované disky.</span><span class="sxs-lookup"><span data-stu-id="3d3b4-106">We recommend that you use Azure Managed Disks for new VMs, and that you convert your previous unmanaged disks toomanaged disks.</span></span> <span data-ttu-id="3d3b4-107">Disky popisovač hello základní úložiště účty spravované, tak nemusíte.</span><span class="sxs-lookup"><span data-stu-id="3d3b4-107">Managed Disks handle hello underlying storage accounts so you don't have to.</span></span> <span data-ttu-id="3d3b4-108">Další informace najdete v tématu naše [přehled disky spravované](../../virtual-machines/windows/managed-disks-overview.md).</span><span class="sxs-lookup"><span data-stu-id="3d3b4-108">For more information, please see our [Managed Disks Overview](../../virtual-machines/windows/managed-disks-overview.md).</span></span>
>

<span data-ttu-id="3d3b4-109">Azure Premium Storage nabízí podporu vysoce výkonné, nízkou latencí disku pro virtuální počítače spuštěné I náročnými úlohy.</span><span class="sxs-lookup"><span data-stu-id="3d3b4-109">Azure Premium Storage delivers high-performance, low-latency disk support for virtual machines running I/O-intensive workloads.</span></span> <span data-ttu-id="3d3b4-110">Pomocí migrace vaší aplikace virtuálních počítačů disky tooAzure Premium Storage můžete využít výhod hello rychlosti a výkonu z těchto disků.</span><span class="sxs-lookup"><span data-stu-id="3d3b4-110">You can take advantage of hello speed and performance of these disks by migrating your application's VM disks tooAzure Premium Storage.</span></span>

<span data-ttu-id="3d3b4-111">Hello účel tohoto průvodce je, že noví uživatelé toohelp Azure Premium Storage lepší Příprava toomake plynulý přechod z jejich aktuálního tooPremium systému úložiště.</span><span class="sxs-lookup"><span data-stu-id="3d3b4-111">hello purpose of this guide is toohelp new users of Azure Premium Storage better prepare toomake a smooth transition from their current system tooPremium Storage.</span></span> <span data-ttu-id="3d3b4-112">Hello Průvodce řeší tři klíčové komponenty hello v tomto procesu:</span><span class="sxs-lookup"><span data-stu-id="3d3b4-112">hello guide addresses three of hello key components in this process:</span></span>

* [<span data-ttu-id="3d3b4-113">Plánování migrace tooPremium hello úložiště</span><span class="sxs-lookup"><span data-stu-id="3d3b4-113">Plan for hello Migration tooPremium Storage</span></span>](#plan-the-migration-to-premium-storage)
* [<span data-ttu-id="3d3b4-114">Příprava a tooPremium kopírování virtuálních pevných disků (VHD) úložiště</span><span class="sxs-lookup"><span data-stu-id="3d3b4-114">Prepare and Copy Virtual Hard Disks (VHDs) tooPremium Storage</span></span>](#prepare-and-copy-virtual-hard-disks-VHDs-to-premium-storage)
* [<span data-ttu-id="3d3b4-115">Vytvoření virtuálního počítače Azure pomocí služby Storage úrovně Premium</span><span class="sxs-lookup"><span data-stu-id="3d3b4-115">Create Azure Virtual Machine using Premium Storage</span></span>](#create-azure-virtual-machine-using-premium-storage)

<span data-ttu-id="3d3b4-116">Můžete migrovat virtuální počítače z jiných platforem tooAzure Storage úrovně Premium nebo migrovat existující virtuální počítače Azure z úložiště Standard Storage tooPremium úložiště.</span><span class="sxs-lookup"><span data-stu-id="3d3b4-116">You can either migrate VMs from other platforms tooAzure Premium Storage or migrate existing Azure VMs from Standard Storage tooPremium Storage.</span></span> <span data-ttu-id="3d3b4-117">Tento průvodce popisuje kroky pro oba dva scénáře.</span><span class="sxs-lookup"><span data-stu-id="3d3b4-117">This guide covers steps for both two scenarios.</span></span> <span data-ttu-id="3d3b4-118">Postupujte podle kroků hello zadaný v příslušné části hello v závislosti na vašem scénáři.</span><span class="sxs-lookup"><span data-stu-id="3d3b4-118">Follow hello steps specified in hello relevant section depending on your scenario.</span></span>

> [!NOTE]
> <span data-ttu-id="3d3b4-119">Najdete přehled funkcí a cen. úložiště Premium Storage v úložiště Premium: [vysoce výkonné úložiště pro úlohy virtuálních počítačů Azure](storage-premium-storage.md).</span><span class="sxs-lookup"><span data-stu-id="3d3b4-119">You can find a feature overview and pricing of Premium Storage in Premium Storage: [High-Performance Storage for Azure Virtual Machine Workloads](storage-premium-storage.md).</span></span> <span data-ttu-id="3d3b4-120">Doporučujeme migraci všech disku virtuálního počítače vyžadující vysokou IOPS tooAzure Storage úrovně Premium hello nejlepšího výkonu pro vaši aplikaci.</span><span class="sxs-lookup"><span data-stu-id="3d3b4-120">We recommend migrating any virtual machine disk requiring high IOPS tooAzure Premium Storage for hello best performance for your application.</span></span> <span data-ttu-id="3d3b4-121">Pokud disk nevyžadují vysokou IOPS, můžete omezit náklady na údržbu ve standardní úložiště, která ukládá data disku virtuálního počítače na jednotkách pevných disků (HDD) namísto SSD.</span><span class="sxs-lookup"><span data-stu-id="3d3b4-121">If your disk does not require high IOPS, you can limit costs by maintaining it in Standard Storage, which stores virtual machine disk data on Hard Disk Drives (HDDs) instead of SSDs.</span></span>
>

<span data-ttu-id="3d3b4-122">Dokončení procesu migrace hello v celé jeho šíři může vyžadovat další akce před i po hello kroků uvedených v této příručce.</span><span class="sxs-lookup"><span data-stu-id="3d3b4-122">Completing hello migration process in its entirety may require additional actions both before and after hello steps provided in this guide.</span></span> <span data-ttu-id="3d3b4-123">Mezi příklady patří konfiguraci virtuální sítě nebo koncových bodů nebo provádění změn kódu v rámci aplikace hello, sám sebe, což může vyžadovat výpadky v aplikaci.</span><span class="sxs-lookup"><span data-stu-id="3d3b4-123">Examples include configuring virtual networks or endpoints or making code changes within hello application itself which may require some downtime in your application.</span></span> <span data-ttu-id="3d3b4-124">Tyto akce jsou jedinečné tooeach aplikace a měli byste pokračovat společně s hello kroků uvedených v této příručce toomake hello úplné přechod tooPremium úložiště jako bezproblémové nejblíže.</span><span class="sxs-lookup"><span data-stu-id="3d3b4-124">These actions are unique tooeach application, and you should complete them along with hello steps provided in this guide toomake hello full transition tooPremium Storage as seamless as possible.</span></span>

## <span data-ttu-id="3d3b4-125"><a name="plan-the-migration-to-premium-storage"></a>Plánování migrace tooPremium hello úložiště</span><span class="sxs-lookup"><span data-stu-id="3d3b4-125"><a name="plan-the-migration-to-premium-storage"></a>Plan for hello migration tooPremium Storage</span></span>
<span data-ttu-id="3d3b4-126">Tato část zajistí jsou připravené toofollow hello postupy migrace popsané v tomto článku a vám pomůže toomake hello nejlepší rozhodnutí o typy virtuálního počítače a disku.</span><span class="sxs-lookup"><span data-stu-id="3d3b4-126">This section ensures that you are ready toofollow hello migration steps in this article, and helps you toomake hello best decision on VM and Disk types.</span></span>

### <a name="prerequisites"></a><span data-ttu-id="3d3b4-127">Požadavky</span><span class="sxs-lookup"><span data-stu-id="3d3b4-127">Prerequisites</span></span>
* <span data-ttu-id="3d3b4-128">Budete potřebovat předplatné Azure.</span><span class="sxs-lookup"><span data-stu-id="3d3b4-128">You will need an Azure subscription.</span></span> <span data-ttu-id="3d3b4-129">Pokud nemáte, můžete vytvořit jeden měsíc [bezplatnou zkušební verzi](https://azure.microsoft.com/pricing/free-trial/) předplatné nebo navštivte [Azure – ceny](https://azure.microsoft.com/pricing/) další možnosti.</span><span class="sxs-lookup"><span data-stu-id="3d3b4-129">If you don't have one, you can create a one-month [free trial](https://azure.microsoft.com/pricing/free-trial/) subscription or visit [Azure Pricing](https://azure.microsoft.com/pricing/) for more options.</span></span>
* <span data-ttu-id="3d3b4-130">tooexecute rutiny prostředí PowerShell, je nutné modul Microsoft Azure PowerShell hello.</span><span class="sxs-lookup"><span data-stu-id="3d3b4-130">tooexecute PowerShell cmdlets, you will need hello Microsoft Azure PowerShell module.</span></span> <span data-ttu-id="3d3b4-131">V tématu [jak tooinstall a konfigurace prostředí Azure PowerShell](/powershell/azure/overview) hello nainstalovat bod a pokyny k instalaci.</span><span class="sxs-lookup"><span data-stu-id="3d3b4-131">See [How tooinstall and configure Azure PowerShell](/powershell/azure/overview) for hello install point and installation instructions.</span></span>
* <span data-ttu-id="3d3b4-132">Pokud máte v plánu toouse Azure virtuálních počítačů spuštěných na Storage úrovně Premium, je potřeba toouse hello podporující virtuálních počítačů služby Storage úrovně Premium.</span><span class="sxs-lookup"><span data-stu-id="3d3b4-132">When you plan toouse Azure VMs running on Premium Storage, you need toouse hello Premium Storage capable VMs.</span></span> <span data-ttu-id="3d3b4-133">Standard a Premium Storage disků můžete použít s podporou virtuálních počítačů služby Storage úrovně Premium.</span><span class="sxs-lookup"><span data-stu-id="3d3b4-133">You can use both Standard and Premium Storage disks with Premium Storage capable VMs.</span></span> <span data-ttu-id="3d3b4-134">Disky úložiště Premium je k dispozici s více typy virtuálních počítačů v budoucí hello.</span><span class="sxs-lookup"><span data-stu-id="3d3b4-134">Premium storage disks will be available with more VM types in hello future.</span></span> <span data-ttu-id="3d3b4-135">Další informace o všech dostupných typů disku virtuálního počítače Azure a velikosti najdete v tématu [velikosti virtuálních počítačů](../../virtual-machines/windows/sizes.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) a [velikosti pro cloudové služby](../../cloud-services/cloud-services-sizes-specs.md).</span><span class="sxs-lookup"><span data-stu-id="3d3b4-135">For more information on all available Azure VM disk types and sizes, see [Sizes for virtual machines](../../virtual-machines/windows/sizes.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) and [Sizes for Cloud Services](../../cloud-services/cloud-services-sizes-specs.md).</span></span>

### <a name="considerations"></a><span data-ttu-id="3d3b4-136">Požadavky</span><span class="sxs-lookup"><span data-stu-id="3d3b4-136">Considerations</span></span>
<span data-ttu-id="3d3b4-137">Virtuální počítač Azure podporuje připojení několik disků úložiště Premium tak, aby vaše aplikace může mít až too256 TB místa na virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="3d3b4-137">An Azure VM supports attaching several Premium Storage disks so that your applications can have up too256 TB of storage per VM.</span></span> <span data-ttu-id="3d3b4-138">Storage úrovně Premium můžete aplikace dosáhnout 80 000 IOPS (vstupně výstupních operací za sekundu) na virtuální počítač a 2000 MB za druhé propustnost disku na virtuální počítač s velmi nízkou latenci pro operace čtení.</span><span class="sxs-lookup"><span data-stu-id="3d3b4-138">With Premium Storage, your applications can achieve 80,000 IOPS (input/output operations per second) per VM and 2000 MB per second disk throughput per VM with extremely low latencies for read operations.</span></span> <span data-ttu-id="3d3b4-139">Máte možností v různých virtuálních počítačů a disků.</span><span class="sxs-lookup"><span data-stu-id="3d3b4-139">You have options in a variety of VMs and Disks.</span></span> <span data-ttu-id="3d3b4-140">Tato část se toohelp toofind možnost, která nejlépe vyhovuje vaší zatížení.</span><span class="sxs-lookup"><span data-stu-id="3d3b4-140">This section is toohelp you toofind an option that best suits your workload.</span></span>

#### <a name="vm-sizes"></a><span data-ttu-id="3d3b4-141">Velikost virtuálních počítačů</span><span class="sxs-lookup"><span data-stu-id="3d3b4-141">VM sizes</span></span>
<span data-ttu-id="3d3b4-142">Specifikace velikosti virtuálního počítače Azure Hello jsou uvedeny v [velikosti virtuálních počítačů](../../virtual-machines/windows/sizes.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="3d3b4-142">hello Azure VM size specifications are listed in [Sizes for virtual machines](../../virtual-machines/windows/sizes.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span> <span data-ttu-id="3d3b4-143">Zkontrolujte hello výkonové charakteristiky virtuálních počítačů, které pracovat s Storage úrovně Premium a zvolte hello nejvhodnější velikost virtuálního počítače, který nejlépe vyhovuje vaší zatížení.</span><span class="sxs-lookup"><span data-stu-id="3d3b4-143">Review hello performance characteristics of virtual machines that work with Premium Storage and choose hello most appropriate VM size that best suits your workload.</span></span> <span data-ttu-id="3d3b4-144">Ujistěte se, zda je dostatečnou šířku pásma k dispozici na disku provozu hello toodrive virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="3d3b4-144">Make sure that there is sufficient bandwidth available on your VM toodrive hello disk traffic.</span></span>

#### <a name="disk-sizes"></a><span data-ttu-id="3d3b4-145">Velikost disků</span><span class="sxs-lookup"><span data-stu-id="3d3b4-145">Disk sizes</span></span>
<span data-ttu-id="3d3b4-146">Existuje pět typů disků, které lze použít s vaší virtuálních počítačů a každá z nich má konkrétní IOPs a propustnost omezení.</span><span class="sxs-lookup"><span data-stu-id="3d3b4-146">There are five types of disks that can be used with your VM and each has specific IOPs and throughput limits.</span></span> <span data-ttu-id="3d3b4-147">Vzít v úvahu tyto limity, když pro svého virtuálního počítače zvolit typ disku hello podle potřeb hello vaší aplikace z hlediska kapacity, výkon, škálovatelnost a načte ve špičce.</span><span class="sxs-lookup"><span data-stu-id="3d3b4-147">Take into consideration these limits when choosing hello disk type for your VM based on hello needs of your application in terms of capacity, performance, scalability, and peak loads.</span></span>

| <span data-ttu-id="3d3b4-148">Disky typu Premium</span><span class="sxs-lookup"><span data-stu-id="3d3b4-148">Premium Disks Type</span></span>  | <span data-ttu-id="3d3b4-149">P10</span><span class="sxs-lookup"><span data-stu-id="3d3b4-149">P10</span></span>   | <span data-ttu-id="3d3b4-150">P20</span><span class="sxs-lookup"><span data-stu-id="3d3b4-150">P20</span></span>   | <span data-ttu-id="3d3b4-151">P30</span><span class="sxs-lookup"><span data-stu-id="3d3b4-151">P30</span></span>            | <span data-ttu-id="3d3b4-152">P40</span><span class="sxs-lookup"><span data-stu-id="3d3b4-152">P40</span></span>            | <span data-ttu-id="3d3b4-153">P50</span><span class="sxs-lookup"><span data-stu-id="3d3b4-153">P50</span></span>            | 
|:-------------------:|:-----:|:-----:|:--------------:|:--------------:|:--------------:|
| <span data-ttu-id="3d3b4-154">Velikost disku</span><span class="sxs-lookup"><span data-stu-id="3d3b4-154">Disk size</span></span>           | <span data-ttu-id="3d3b4-155">128 GB</span><span class="sxs-lookup"><span data-stu-id="3d3b4-155">128 GB</span></span>| <span data-ttu-id="3d3b4-156">512 GB</span><span class="sxs-lookup"><span data-stu-id="3d3b4-156">512 GB</span></span>| <span data-ttu-id="3d3b4-157">1024 GB (1 TB)</span><span class="sxs-lookup"><span data-stu-id="3d3b4-157">1024 GB (1 TB)</span></span> | <span data-ttu-id="3d3b4-158">2 048 GB (2 TB)</span><span class="sxs-lookup"><span data-stu-id="3d3b4-158">2048 GB (2 TB)</span></span> | <span data-ttu-id="3d3b4-159">4095 GB (4 TB)</span><span class="sxs-lookup"><span data-stu-id="3d3b4-159">4095 GB (4 TB)</span></span> | 
| <span data-ttu-id="3d3b4-160">Vstupně-výstupní operace za sekundu / disk</span><span class="sxs-lookup"><span data-stu-id="3d3b4-160">IOPS per disk</span></span>       | <span data-ttu-id="3d3b4-161">500</span><span class="sxs-lookup"><span data-stu-id="3d3b4-161">500</span></span>   | <span data-ttu-id="3d3b4-162">2300</span><span class="sxs-lookup"><span data-stu-id="3d3b4-162">2300</span></span>  | <span data-ttu-id="3d3b4-163">5000</span><span class="sxs-lookup"><span data-stu-id="3d3b4-163">5000</span></span>           | <span data-ttu-id="3d3b4-164">7500</span><span class="sxs-lookup"><span data-stu-id="3d3b4-164">7500</span></span>           | <span data-ttu-id="3d3b4-165">7500</span><span class="sxs-lookup"><span data-stu-id="3d3b4-165">7500</span></span>           | 
| <span data-ttu-id="3d3b4-166">Propustnost / disk</span><span class="sxs-lookup"><span data-stu-id="3d3b4-166">Throughput per disk</span></span> | <span data-ttu-id="3d3b4-167">100 MB za sekundu</span><span class="sxs-lookup"><span data-stu-id="3d3b4-167">100 MB per second</span></span> | <span data-ttu-id="3d3b4-168">150 MB za sekundu</span><span class="sxs-lookup"><span data-stu-id="3d3b4-168">150 MB per second</span></span> | <span data-ttu-id="3d3b4-169">200 MB za sekundu</span><span class="sxs-lookup"><span data-stu-id="3d3b4-169">200 MB per second</span></span> | <span data-ttu-id="3d3b4-170">250 MB za sekundu</span><span class="sxs-lookup"><span data-stu-id="3d3b4-170">250 MB per second</span></span> | <span data-ttu-id="3d3b4-171">250 MB za sekundu</span><span class="sxs-lookup"><span data-stu-id="3d3b4-171">250 MB per second</span></span> |

<span data-ttu-id="3d3b4-172">V závislosti na velikosti pracovní zátěže určí, jestli dalších datových disků jsou nezbytné pro virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="3d3b4-172">Depending on your workload, determine if additional data disks are necessary for your VM.</span></span> <span data-ttu-id="3d3b4-173">Můžete připojit několik trvalých datových disků tooyour virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="3d3b4-173">You can attach several persistent data disks tooyour VM.</span></span> <span data-ttu-id="3d3b4-174">V případě potřeby můžete rozkládají napříč hello disky tooincrease hello kapacitu a výkon hello svazku.</span><span class="sxs-lookup"><span data-stu-id="3d3b4-174">If needed, you can stripe across hello disks tooincrease hello capacity and performance of hello volume.</span></span> <span data-ttu-id="3d3b4-175">(Zjistíte novinky prokládání disků [zde](storage-premium-storage-performance.md#disk-striping).) Pokud rozkládají Storage úrovně Premium datových disků pomocí [prostory úložiště][4], byste měli nakonfigurovat s jedním sloupcem pro každý disk, který se používá.</span><span class="sxs-lookup"><span data-stu-id="3d3b4-175">(See what is Disk Striping [here](storage-premium-storage-performance.md#disk-striping).) If you stripe Premium Storage data disks using [Storage Spaces][4], you should configure it with one column for each disk that is used.</span></span> <span data-ttu-id="3d3b4-176">V opačném hello celkový výkon hello rozdělená svazku může být nižší, než se očekávalo kvůli toouneven distribuce přenosů mezi disky hello.</span><span class="sxs-lookup"><span data-stu-id="3d3b4-176">Otherwise, hello overall performance of hello striped volume may be lower than expected due toouneven distribution of traffic across hello disks.</span></span> <span data-ttu-id="3d3b4-177">Pro virtuální počítače s Linuxem můžete použít hello *mdadm* tooachieve nástroj hello stejné.</span><span class="sxs-lookup"><span data-stu-id="3d3b4-177">For Linux VMs you can use hello *mdadm* utility tooachieve hello same.</span></span> <span data-ttu-id="3d3b4-178">Najdete v článku [konfigurace RAID softwaru v systému Linux](../../virtual-machines/linux/configure-raid.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) podrobnosti.</span><span class="sxs-lookup"><span data-stu-id="3d3b4-178">See article [Configure Software RAID on Linux](../../virtual-machines/linux/configure-raid.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) for details.</span></span>

#### <a name="storage-account-scalability-targets"></a><span data-ttu-id="3d3b4-179">Cíle škálovatelnosti účtu Storage</span><span class="sxs-lookup"><span data-stu-id="3d3b4-179">Storage account scalability targets</span></span>
<span data-ttu-id="3d3b4-180">Prémiové účty úložiště mají hello následující cíle škálovatelnosti v přidání toohello [a cíle výkonnosti služby Azure Storage Scalability](storage-scalability-targets.md).</span><span class="sxs-lookup"><span data-stu-id="3d3b4-180">Premium Storage accounts have hello following scalability targets in addition toohello [Azure Storage Scalability and Performance Targets](storage-scalability-targets.md).</span></span> <span data-ttu-id="3d3b4-181">Pokud vaše požadavky aplikací přesahují hello cíle škálovatelnosti účtu jedno úložiště, sestavení vaší aplikace toouse více účtů úložiště a oddílu data mezi různými tyto účty úložiště.</span><span class="sxs-lookup"><span data-stu-id="3d3b4-181">If your application requirements exceed hello scalability targets of a single storage account, build your application toouse multiple storage accounts, and partition your data across those storage accounts.</span></span>

| <span data-ttu-id="3d3b4-182">Celkový počet účet kapacity</span><span class="sxs-lookup"><span data-stu-id="3d3b4-182">Total Account Capacity</span></span> | <span data-ttu-id="3d3b4-183">Celková šířka pásma pro účet místně redundantní úložiště</span><span class="sxs-lookup"><span data-stu-id="3d3b4-183">Total Bandwidth for a Locally Redundant Storage Account</span></span> |
|:--- |:--- |
| <span data-ttu-id="3d3b4-184">Kapacita disku: 35TB</span><span class="sxs-lookup"><span data-stu-id="3d3b4-184">Disk capacity: 35TB</span></span><br /><span data-ttu-id="3d3b4-185">Pořízení snímku kapacity: 10 TB</span><span class="sxs-lookup"><span data-stu-id="3d3b4-185">Snapshot capacity: 10 TB</span></span> |<span data-ttu-id="3d3b4-186">Až too50 gigabity za sekundu pro příchozí a odchozí</span><span class="sxs-lookup"><span data-stu-id="3d3b4-186">Up too50 gigabits per second for Inbound + Outbound</span></span> |

<span data-ttu-id="3d3b4-187">Pro hello Další informace o specifikacích Premium Storage, podívejte se na [škálovatelnost a cíle výkonnosti při použití služby Premium Storage](storage-premium-storage.md#scalability-and-performance-targets).</span><span class="sxs-lookup"><span data-stu-id="3d3b4-187">For hello more information on Premium Storage specifications, check out [Scalability and Performance Targets when using Premium Storage](storage-premium-storage.md#scalability-and-performance-targets).</span></span>

#### <a name="disk-caching-policy"></a><span data-ttu-id="3d3b4-188">Zásady ukládání do mezipaměti na disku</span><span class="sxs-lookup"><span data-stu-id="3d3b4-188">Disk caching policy</span></span>
<span data-ttu-id="3d3b4-189">Ve výchozím nastavení, je disk ukládání do mezipaměti zásad *jen pro čtení* pro všechny hello Premium datových disků, a *pro čtení a zápis* pro disk operačního systému Premium hello připojené toohello virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="3d3b4-189">By default, disk caching policy is *Read-Only* for all hello Premium data disks, and *Read-Write* for hello Premium operating system disk attached toohello VM.</span></span> <span data-ttu-id="3d3b4-190">Toto nastavení konfigurace se doporučuje tooachieve hello optimální výkon pro aplikace IOs.</span><span class="sxs-lookup"><span data-stu-id="3d3b4-190">This configuration setting is recommended tooachieve hello optimal performance for your application's IOs.</span></span> <span data-ttu-id="3d3b4-191">Těžký zápisu nebo pouze pro zápis datové disky (například soubory protokolu serveru SQL Server) zakažte ukládání do mezipaměti disku, takže můžete dosáhnout lepší výkon aplikace.</span><span class="sxs-lookup"><span data-stu-id="3d3b4-191">For write-heavy or write-only data disks (such as SQL Server log files), disable disk caching so that you can achieve better application performance.</span></span> <span data-ttu-id="3d3b4-192">nastavení mezipaměti Hello pro existující datové disky můžete aktualizovat pomocí [portálu Azure](https://portal.azure.com) nebo hello *- HostCaching* parametr hello *Set-AzureDataDisk* rutiny.</span><span class="sxs-lookup"><span data-stu-id="3d3b4-192">hello cache settings for existing data disks can be updated using [Azure Portal](https://portal.azure.com) or hello *-HostCaching* parameter of hello *Set-AzureDataDisk* cmdlet.</span></span>

#### <a name="location"></a><span data-ttu-id="3d3b4-193">Umístění</span><span class="sxs-lookup"><span data-stu-id="3d3b4-193">Location</span></span>
<span data-ttu-id="3d3b4-194">Vyberte umístění, kde je k dispozici Azure Premium Storage.</span><span class="sxs-lookup"><span data-stu-id="3d3b4-194">Pick a location where Azure Premium Storage is available.</span></span> <span data-ttu-id="3d3b4-195">V tématu [služby Azure podle oblasti](https://azure.microsoft.com/regions/#services) aktuální informace o dostupných umístění.</span><span class="sxs-lookup"><span data-stu-id="3d3b4-195">See [Azure Services by Region](https://azure.microsoft.com/regions/#services) for up-to-date information on available locations.</span></span> <span data-ttu-id="3d3b4-196">Virtuální počítače umístěné v hello stejné oblasti jako účet úložiště ukládá pro hello virtuálních počítačů získáte mnohem lepší výkon než pokud jsou v oblastech text hello disky hello.</span><span class="sxs-lookup"><span data-stu-id="3d3b4-196">VMs located in hello same region as hello Storage account that stores hello disks for hello VM will give much better performance than if they are in separate regions.</span></span>

#### <a name="other-azure-vm-configuration-settings"></a><span data-ttu-id="3d3b4-197">Další nastavení konfigurace virtuálního počítače Azure</span><span class="sxs-lookup"><span data-stu-id="3d3b4-197">Other Azure VM configuration settings</span></span>
<span data-ttu-id="3d3b4-198">Při vytváření virtuálního počítače Azure, budete vyzváni tooconfigure určitá nastavení virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="3d3b4-198">When creating an Azure VM, you will be asked tooconfigure certain VM settings.</span></span> <span data-ttu-id="3d3b4-199">Pamatujte si, že se stanoví několik nastavení pro hello životnost hello virtuálních počítačů, zatímco můžete upravit nebo jiné přidat později.</span><span class="sxs-lookup"><span data-stu-id="3d3b4-199">Remember, few settings are fixed for hello lifetime of hello VM, while you can modify or add others later.</span></span> <span data-ttu-id="3d3b4-200">Zkontrolujte nastavení konfigurace virtuálního počítače Azure a přesvědčte se, že jsou správně nakonfigurována toomatch požadavků na zatížení.</span><span class="sxs-lookup"><span data-stu-id="3d3b4-200">Review these Azure VM configuration settings and make sure that these are configured appropriately toomatch your workload requirements.</span></span>

### <a name="optimization"></a><span data-ttu-id="3d3b4-201">Optimalizace</span><span class="sxs-lookup"><span data-stu-id="3d3b4-201">Optimization</span></span>
<span data-ttu-id="3d3b4-202">[Azure Premium Storage: Návrh vysoce výkonné](storage-premium-storage-performance.md) poskytuje pokyny pro vytváření vysoce výkonné aplikace pomocí Azure Premium Storage.</span><span class="sxs-lookup"><span data-stu-id="3d3b4-202">[Azure Premium Storage: Design for High Performance](storage-premium-storage-performance.md) provides guidelines for building high-performance applications using Azure Premium Storage.</span></span> <span data-ttu-id="3d3b4-203">Můžete provést hello pokyny v kombinaci s výkonu osvědčených postupů použít tootechnologies používá vaše aplikace.</span><span class="sxs-lookup"><span data-stu-id="3d3b4-203">You can follow hello guidelines combined with performance best practices applicable tootechnologies used by your application.</span></span>

## <span data-ttu-id="3d3b4-204"><a name="prepare-and-copy-virtual-hard-disks-VHDs-to-premium-storage"></a>Příprava a zkopírovat virtuální pevné disky (VHD) tooPremium úložiště</span><span class="sxs-lookup"><span data-stu-id="3d3b4-204"><a name="prepare-and-copy-virtual-hard-disks-VHDs-to-premium-storage"></a>Prepare and copy virtual hard disks (VHDs) tooPremium Storage</span></span>
<span data-ttu-id="3d3b4-205">Hello následující část obsahuje pokyny pro přípravu virtuální pevné disky z virtuálního počítače a kopírování virtuálních pevných disků tooAzure úložiště.</span><span class="sxs-lookup"><span data-stu-id="3d3b4-205">hello following section provides guidelines for preparing VHDs from your VM and copying VHDs tooAzure Storage.</span></span>

* [<span data-ttu-id="3d3b4-206">Scénář 1: "I mě migrace tooAzure existující virtuální počítače Azure Premium Storage."</span><span class="sxs-lookup"><span data-stu-id="3d3b4-206">Scenario 1: "I am migrating existing Azure VMs tooAzure Premium Storage."</span></span>](#scenario1)
* [<span data-ttu-id="3d3b4-207">Scénář 2: "I mě migrace virtuálních počítačů z jiných platforem tooAzure Storage úrovně Premium."</span><span class="sxs-lookup"><span data-stu-id="3d3b4-207">Scenario 2: "I am migrating VMs from other platforms tooAzure Premium Storage."</span></span>](#scenario2)

### <a name="prerequisites"></a><span data-ttu-id="3d3b4-208">Požadavky</span><span class="sxs-lookup"><span data-stu-id="3d3b4-208">Prerequisites</span></span>
<span data-ttu-id="3d3b4-209">tooprepare hello virtuální pevné disky pro migraci, budete potřebovat:</span><span class="sxs-lookup"><span data-stu-id="3d3b4-209">tooprepare hello VHDs for migration, you'll need:</span></span>

* <span data-ttu-id="3d3b4-210">Předplatné Azure, účet úložiště a kontejneru v tomto úložišti účtu toowhich můžete zkopírovat svůj disk VHD.</span><span class="sxs-lookup"><span data-stu-id="3d3b4-210">An Azure subscription, a storage account, and a container in that storage account toowhich you can copy your VHD.</span></span> <span data-ttu-id="3d3b4-211">Všimněte si, že hello cílový účet úložiště může být účet Standard nebo Premium Storage podle potřeby.</span><span class="sxs-lookup"><span data-stu-id="3d3b4-211">Note that hello destination storage account can be a Standard or Premium Storage account depending on your requirement.</span></span>
* <span data-ttu-id="3d3b4-212">Nástroj toogeneralize text hello virtuálního pevného disku, pokud máte v plánu toocreate více instancí virtuálního počítače z něj.</span><span class="sxs-lookup"><span data-stu-id="3d3b4-212">A tool toogeneralize hello VHD if you plan toocreate multiple VM instances from it.</span></span> <span data-ttu-id="3d3b4-213">Například nástroj sysprep pro systém Windows nebo virt.krychle sysprep pro Ubuntu.</span><span class="sxs-lookup"><span data-stu-id="3d3b4-213">For example, sysprep for Windows or virt-sysprep for Ubuntu.</span></span>
* <span data-ttu-id="3d3b4-214">Nástroj tooupload hello virtuálního pevného disku soubor toohello účet úložiště.</span><span class="sxs-lookup"><span data-stu-id="3d3b4-214">A tool tooupload hello VHD file toohello Storage account.</span></span> <span data-ttu-id="3d3b4-215">V tématu [přenos dat pomocí příkazového řádku Azcopy hello](storage-use-azcopy.md) nebo použijte [Azure storage Exploreru](http://blogs.msdn.com/b/windowsazurestorage/archive/2014/03/11/windows-azure-storage-explorers-2014.aspx).</span><span class="sxs-lookup"><span data-stu-id="3d3b4-215">See [Transfer data with hello AzCopy Command-Line Utility](storage-use-azcopy.md) or use an [Azure storage explorer](http://blogs.msdn.com/b/windowsazurestorage/archive/2014/03/11/windows-azure-storage-explorers-2014.aspx).</span></span> <span data-ttu-id="3d3b4-216">Tato příručka popisuje kopírování svůj disk VHD pomocí nástroje AzCopy hello.</span><span class="sxs-lookup"><span data-stu-id="3d3b4-216">This guide describes copying your VHD using hello AzCopy tool.</span></span>

> [!NOTE]
> <span data-ttu-id="3d3b4-217">Pokud zvolíte možnost synchronní kopie s AzCopy, optimální výkon, zkopírujte svůj disk VHD spuštěním jednoho z těchto nástrojů z virtuálního počítače Azure, který je v hello stejné oblasti jako hello cílový účet úložiště.</span><span class="sxs-lookup"><span data-stu-id="3d3b4-217">If you choose synchronous copy option with AzCopy, for optimal performance, copy your VHD by running one of these tools from an Azure VM that is in hello same region as hello destination storage account.</span></span> <span data-ttu-id="3d3b4-218">Pokud kopírujete virtuální pevný disk z virtuálního počítače Azure v jiné oblasti, může být pomalejší výkon.</span><span class="sxs-lookup"><span data-stu-id="3d3b4-218">If you are copying a VHD from an Azure VM in a different region, your performance may be slower.</span></span>
>
> <span data-ttu-id="3d3b4-219">Kopírování velkého množství dat přes omezenou šířkou pásma, zvažte [pomocí hello Azure Import/Export služby tootransfer data tooBlob úložiště](../storage-import-export-service.md); to vám umožní tootransfer data přesouvání pevným diskem disky tooan Azure Datacenter.</span><span class="sxs-lookup"><span data-stu-id="3d3b4-219">For copying a large amount of data over limited bandwidth, consider [using hello Azure Import/Export service tootransfer data tooBlob Storage](../storage-import-export-service.md); this allows you tootransfer your data by shipping hard disk drives tooan Azure datacenter.</span></span> <span data-ttu-id="3d3b4-220">Můžete použít Azure Import/Export služby toocopy data tooa standardní účet úložiště pouze hello.</span><span class="sxs-lookup"><span data-stu-id="3d3b4-220">You can use hello Azure Import/Export service toocopy data tooa standard storage account only.</span></span> <span data-ttu-id="3d3b4-221">Jakmile hello data ve vašem účtu standardní úložiště, můžete použít buď hello [kopírovat objekt Blob API](https://msdn.microsoft.com/library/azure/dd894037.aspx) nebo AzCopy tootransfer hello data tooyour prémiový účet úložiště.</span><span class="sxs-lookup"><span data-stu-id="3d3b4-221">Once hello data is in your standard storage account, you can use either hello [Copy Blob API](https://msdn.microsoft.com/library/azure/dd894037.aspx) or AzCopy tootransfer hello data tooyour premium storage account.</span></span>
>
> <span data-ttu-id="3d3b4-222">Všimněte si, že Microsoft Azure podporuje pouze soubory virtuálního pevného disku pevné velikosti.</span><span class="sxs-lookup"><span data-stu-id="3d3b4-222">Note that Microsoft Azure only supports fixed size VHD files.</span></span> <span data-ttu-id="3d3b4-223">Soubory VHDX nebo dynamické virtuální pevné disky nejsou podporovány.</span><span class="sxs-lookup"><span data-stu-id="3d3b4-223">VHDX files or dynamic VHDs are not supported.</span></span> <span data-ttu-id="3d3b4-224">Pokud máte dynamický virtuální pevný disk, můžete ji převést toofixed velikost pomocí hello [Convert-VHD](http://technet.microsoft.com/library/hh848454.aspx) rutiny.</span><span class="sxs-lookup"><span data-stu-id="3d3b4-224">If you have a dynamic VHD, you can convert it toofixed size using hello [Convert-VHD](http://technet.microsoft.com/library/hh848454.aspx) cmdlet.</span></span>
>
>

### <span data-ttu-id="3d3b4-225"><a name="scenario1"></a>Scénář 1: "I mě migrace tooAzure existující virtuální počítače Azure Premium Storage."</span><span class="sxs-lookup"><span data-stu-id="3d3b4-225"><a name="scenario1"></a>Scenario 1: "I am migrating existing Azure VMs tooAzure Premium Storage."</span></span>
<span data-ttu-id="3d3b4-226">Pokud migrujete existující virtuální počítače Azure, hello zastavení virtuálního počítače, virtuální pevné disky na hello typ virtuálního pevného disku, které chcete připravit a zkopírujte hello virtuálního pevného disku s AzCopy nebo prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="3d3b4-226">If you are migrating existing Azure VMs, stop hello VM, prepare VHDs per hello type of VHD you want, and then copy hello VHD with AzCopy or PowerShell.</span></span>

<span data-ttu-id="3d3b4-227">Hello virtuálních počítačů musí toobe úplně dolů toomigrate do čistého stavu.</span><span class="sxs-lookup"><span data-stu-id="3d3b4-227">hello VM needs toobe completely down toomigrate a clean state.</span></span> <span data-ttu-id="3d3b4-228">Budou existovat výpadky až po dokončení migrace hello.</span><span class="sxs-lookup"><span data-stu-id="3d3b4-228">There will be a downtime until hello migration completes.</span></span>

#### <a name="step-1-prepare-vhds-for-migration"></a><span data-ttu-id="3d3b4-229">Krok 1.</span><span class="sxs-lookup"><span data-stu-id="3d3b4-229">Step 1.</span></span> <span data-ttu-id="3d3b4-230">Příprava na migraci virtuálních pevných disků</span><span class="sxs-lookup"><span data-stu-id="3d3b4-230">Prepare VHDs for migration</span></span>
<span data-ttu-id="3d3b4-231">Pokud provádíte migraci virtuálních počítačů Azure tooPremium existující úložiště, může být svůj disk VHD:</span><span class="sxs-lookup"><span data-stu-id="3d3b4-231">If you are migrating existing Azure VMs tooPremium Storage, your VHD may be:</span></span>

* <span data-ttu-id="3d3b4-232">Bitovou kopii operačního systému</span><span class="sxs-lookup"><span data-stu-id="3d3b4-232">A generalized operating system image</span></span>
* <span data-ttu-id="3d3b4-233">Disk jedinečné operačního systému</span><span class="sxs-lookup"><span data-stu-id="3d3b4-233">A unique operating system disk</span></span>
* <span data-ttu-id="3d3b4-234">Datový disk</span><span class="sxs-lookup"><span data-stu-id="3d3b4-234">A data disk</span></span>

<span data-ttu-id="3d3b4-235">Níže budeme provede tyto 3 scénáře pro přípravu svůj disk VHD.</span><span class="sxs-lookup"><span data-stu-id="3d3b4-235">Below we walk through these 3 scenarios for preparing your VHD.</span></span>

##### <a name="use-a-generalized-operating-system-vhd-toocreate-multiple-vm-instances"></a><span data-ttu-id="3d3b4-236">Používání zobecněný virtuální pevný disk operačního systému toocreate více instancí virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="3d3b4-236">Use a generalized Operating System VHD toocreate multiple VM instances</span></span>
<span data-ttu-id="3d3b4-237">Pokud nahráváte virtuálního pevného disku, které budou použité toocreate více obecné instancí virtuálního počítače Azure, musíte nejprve generalize virtuální pevný disk pomocí nástroje sysprep.</span><span class="sxs-lookup"><span data-stu-id="3d3b4-237">If you are uploading a VHD that will be used toocreate multiple generic Azure VM instances, you must first generalize VHD using a sysprep utility.</span></span> <span data-ttu-id="3d3b4-238">To platí tooa virtuální pevný disk, který je místně nebo v cloudu hello.</span><span class="sxs-lookup"><span data-stu-id="3d3b4-238">This applies tooa VHD that is on-premises or in hello cloud.</span></span> <span data-ttu-id="3d3b4-239">Nástroj Sysprep odebere všechny informace specifické pro počítač hello virtuálního pevného disku.</span><span class="sxs-lookup"><span data-stu-id="3d3b4-239">Sysprep removes any machine-specific information from hello VHD.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="3d3b4-240">Pořízení snímku nebo zálohování virtuálního počítače před jeho generalizací.</span><span class="sxs-lookup"><span data-stu-id="3d3b4-240">Take a snapshot or backup your VM before generalizing it.</span></span> <span data-ttu-id="3d3b4-241">Spuštěný nástroj sysprep zastaví a navrátit hello instance virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="3d3b4-241">Running sysprep will stop and deallocate hello VM instance.</span></span> <span data-ttu-id="3d3b4-242">Postupujte podle kroků toosysprep virtuálního pevného disku operačního systému Windows.</span><span class="sxs-lookup"><span data-stu-id="3d3b4-242">Follow steps below toosysprep a Windows OS VHD.</span></span> <span data-ttu-id="3d3b4-243">Všimněte si, že spustíte příkaz Sysprep hello bude vyžadovat tooshut dolů hello virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="3d3b4-243">Note that running hello Sysprep command will require you tooshut down hello virtual machine.</span></span> <span data-ttu-id="3d3b4-244">Další informace o nástroji Sysprep najdete v tématu [přehled nástroje Sysprep](http://technet.microsoft.com/library/hh825209.aspx) nebo [technické informace o nástroji Sysprep](http://technet.microsoft.com/library/cc766049.aspx).</span><span class="sxs-lookup"><span data-stu-id="3d3b4-244">For more information about Sysprep, see [Sysprep Overview](http://technet.microsoft.com/library/hh825209.aspx) or [Sysprep Technical Reference](http://technet.microsoft.com/library/cc766049.aspx).</span></span>
>
>

1. <span data-ttu-id="3d3b4-245">Otevřete okno příkazového řádku jako správce.</span><span class="sxs-lookup"><span data-stu-id="3d3b4-245">Open a Command Prompt window as an administrator.</span></span>
2. <span data-ttu-id="3d3b4-246">Zadejte následující příkaz tooopen Sysprep hello:</span><span class="sxs-lookup"><span data-stu-id="3d3b4-246">Enter hello following command tooopen Sysprep:</span></span>

    ```
    %windir%\system32\sysprep\sysprep.exe
    ```

3. <span data-ttu-id="3d3b4-247">V hello nástroj pro přípravu systému, vyberte možnost zadejte systému Out-of-Box prostředí (při prvním zapnutí), vyberte hello generalizace zaškrtávací políčko, vyberte **vypnutí**a potom klikněte na **OK**, jak ukazuje následující obrázek hello.</span><span class="sxs-lookup"><span data-stu-id="3d3b4-247">In hello System Preparation Tool, select Enter System Out-of-Box Experience (OOBE), select hello Generalize check box, select **Shutdown**, and then click **OK**, as shown in hello image below.</span></span> <span data-ttu-id="3d3b4-248">Nástroj Sysprep se generalize hello operačního systému a vypnout hello systém.</span><span class="sxs-lookup"><span data-stu-id="3d3b4-248">Sysprep will generalize hello operating system and shut down hello system.</span></span>

    ![][1]

<span data-ttu-id="3d3b4-249">U virtuálního počítače s Ubuntu hello tooachieve virt.krychle sysprep pomocí stejné.</span><span class="sxs-lookup"><span data-stu-id="3d3b4-249">For an Ubuntu VM, use virt-sysprep tooachieve hello same.</span></span> <span data-ttu-id="3d3b4-250">V tématu [virt.krychle sysprep](http://manpages.ubuntu.com/manpages/precise/man1/virt-sysprep.1.html) další podrobnosti.</span><span class="sxs-lookup"><span data-stu-id="3d3b4-250">See [virt-sysprep](http://manpages.ubuntu.com/manpages/precise/man1/virt-sysprep.1.html) for more details.</span></span> <span data-ttu-id="3d3b4-251">Viz také některé softwaru hello open source [Linux serveru zřizování softwaru](http://www.cyberciti.biz/tips/server-provisioning-software.html) pro jiné operační systémy Linux.</span><span class="sxs-lookup"><span data-stu-id="3d3b4-251">See also some of hello open source [Linux Server Provisioning software](http://www.cyberciti.biz/tips/server-provisioning-software.html) for other Linux operating systems.</span></span>

##### <a name="use-a-unique-operating-system-vhd-toocreate-a-single-vm-instance"></a><span data-ttu-id="3d3b4-252">Použijte jedinečný virtuální pevný disk operačního systému toocreate jedna instance virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="3d3b4-252">Use a unique Operating System VHD toocreate a single VM instance</span></span>
<span data-ttu-id="3d3b4-253">Pokud máte aplikace běžící v hello virtuálních počítačů, které vyžaduje hello počítače konkrétní data, není generalize hello virtuálního pevného disku.</span><span class="sxs-lookup"><span data-stu-id="3d3b4-253">If you have an application running on hello VM which requires hello machine specific data, do not generalize hello VHD.</span></span> <span data-ttu-id="3d3b4-254">Zobecněný virtuální pevný disk se dá použít toocreate jedinečné instance virtuálního počítače Azure.</span><span class="sxs-lookup"><span data-stu-id="3d3b4-254">A non-generalized VHD can be used toocreate a unique Azure VM instance.</span></span> <span data-ttu-id="3d3b4-255">Například pokud máte řadiče domény na svůj disk VHD, provádění sysprep znamená, že neúčinná jako řadič domény.</span><span class="sxs-lookup"><span data-stu-id="3d3b4-255">For example, if you have Domain Controller on your VHD, executing sysprep will make it ineffective as a Domain Controller.</span></span> <span data-ttu-id="3d3b4-256">Zkontrolujte hello aplikací běžících na váš počítač a hello vlivu spuštěním programu sysprep v nich před generalizací hello virtuálního pevného disku.</span><span class="sxs-lookup"><span data-stu-id="3d3b4-256">Review hello applications running on your VM and hello impact of running sysprep on them before generalizing hello VHD.</span></span>

##### <a name="register-data-disk-vhd"></a><span data-ttu-id="3d3b4-257">Registrovat datový disk VHD</span><span class="sxs-lookup"><span data-stu-id="3d3b4-257">Register data disk VHD</span></span>
<span data-ttu-id="3d3b4-258">Pokud máte datové disky v Azure toobe migrovat, musí zkontrolujte, zda text hello virtuálních počítačů, které pomocí těchto dat, které disky jsou vypnout.</span><span class="sxs-lookup"><span data-stu-id="3d3b4-258">If you have data disks in Azure toobe migrated, you must make sure hello VMs that use these data disks are shut down.</span></span>

<span data-ttu-id="3d3b4-259">Postupujte podle kroků hello popsaných níže toocopy virtuálního pevného disku tooAzure Storage úrovně Premium a zaregistrovat ji jako zřízené datový disk.</span><span class="sxs-lookup"><span data-stu-id="3d3b4-259">Follow hello steps described below toocopy VHD tooAzure Premium Storage and register it as a provisioned data disk.</span></span>

#### <a name="step-2-create-hello-destination-for-your-vhd"></a><span data-ttu-id="3d3b4-260">Krok 2.</span><span class="sxs-lookup"><span data-stu-id="3d3b4-260">Step 2.</span></span> <span data-ttu-id="3d3b4-261">Vytvoření hello cíl pro svůj disk VHD</span><span class="sxs-lookup"><span data-stu-id="3d3b4-261">Create hello destination for your VHD</span></span>
<span data-ttu-id="3d3b4-262">Vytvořte účet úložiště pro údržbu virtuální pevné disky.</span><span class="sxs-lookup"><span data-stu-id="3d3b4-262">Create a storage account for maintaining your VHDs.</span></span> <span data-ttu-id="3d3b4-263">Vezměte v úvahu následující body při plánování kde hello toostore virtuální pevné disky:</span><span class="sxs-lookup"><span data-stu-id="3d3b4-263">Consider hello following points when planning where toostore your VHDs:</span></span>

* <span data-ttu-id="3d3b4-264">cíl Hello prémiový účet úložiště.</span><span class="sxs-lookup"><span data-stu-id="3d3b4-264">hello target Premium storage account.</span></span>
* <span data-ttu-id="3d3b4-265">Hello umístění účtu úložiště musí být stejná jako úložiště Premium podporuje virtuální počítače Azure v konečné fázi hello vytvoříte.</span><span class="sxs-lookup"><span data-stu-id="3d3b4-265">hello storage account location must be same as Premium Storage capable Azure VMs you will create in hello final stage.</span></span> <span data-ttu-id="3d3b4-266">Může zkopírovat tooa nový účet úložiště, nebo plán toouse hello stejný účet úložiště podle vašich potřeb.</span><span class="sxs-lookup"><span data-stu-id="3d3b4-266">You could copy tooa new storage account, or plan toouse hello same storage account based on your needs.</span></span>
* <span data-ttu-id="3d3b4-267">Zkopírujte a uložte klíč účtu úložiště hello hello cílový účet úložiště pro další fáze hello.</span><span class="sxs-lookup"><span data-stu-id="3d3b4-267">Copy and save hello storage account key of hello destination storage account for hello next stage.</span></span>

<span data-ttu-id="3d3b4-268">Pro datové disky, můžete zvolit tookeep některé datové disky v standardní účet úložiště (například disky, které obsahují chladič úložiště), ale důrazně doporučujeme přesouvání všech dat pro produkční zatížení toouse premium storage.</span><span class="sxs-lookup"><span data-stu-id="3d3b4-268">For data disks, you can choose tookeep some data disks in a standard storage account (for example, disks that have cooler storage), but we strongly recommend you moving all data for production workload toouse premium storage.</span></span>

#### <span data-ttu-id="3d3b4-269"><a name="copy-vhd-with-azcopy-or-powershell"></a>Krok 3.</span><span class="sxs-lookup"><span data-stu-id="3d3b4-269"><a name="copy-vhd-with-azcopy-or-powershell"></a>Step 3.</span></span> <span data-ttu-id="3d3b4-270">Zkopírujte virtuální pevný disk s AzCopy nebo prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="3d3b4-270">Copy VHD with AzCopy or PowerShell</span></span>
<span data-ttu-id="3d3b4-271">Budete potřebovat toofind cesta kontejneru a úložiště účet klíče tooprocess jednu z těchto dvou možností.</span><span class="sxs-lookup"><span data-stu-id="3d3b4-271">You will need toofind your container path and storage account key tooprocess either of these two options.</span></span> <span data-ttu-id="3d3b4-272">Klíč účtu úložiště a cesta kontejneru lze nalézt v **portálu Azure** > **úložiště**.</span><span class="sxs-lookup"><span data-stu-id="3d3b4-272">Container path and storage account key can be found in **Azure Portal** > **Storage**.</span></span> <span data-ttu-id="3d3b4-273">jako "https://myaccount.blob.core.windows.net/mycontainer/" bude mít adresu URL kontejneru Hello.</span><span class="sxs-lookup"><span data-stu-id="3d3b4-273">hello container URL will be like "https://myaccount.blob.core.windows.net/mycontainer/".</span></span>

##### <a name="option-1-copy-a-vhd-with-azcopy-asynchronous-copy"></a><span data-ttu-id="3d3b4-274">Možnost 1: Zkopírujte virtuální pevný disk s AzCopy (asynchronní kopie)</span><span class="sxs-lookup"><span data-stu-id="3d3b4-274">Option 1: Copy a VHD with AzCopy (Asynchronous copy)</span></span>
<span data-ttu-id="3d3b4-275">Pomocí AzCopy, můžete snadno odeslat hello VHD přes hello Internetu.</span><span class="sxs-lookup"><span data-stu-id="3d3b4-275">Using AzCopy, you can easily upload hello VHD over hello Internet.</span></span> <span data-ttu-id="3d3b4-276">V závislosti na velikosti hello hello virtuálních pevných disků může to trvat čas.</span><span class="sxs-lookup"><span data-stu-id="3d3b4-276">Depending on hello size of hello VHDs, this may take time.</span></span> <span data-ttu-id="3d3b4-277">Při použití této možnosti mějte na paměti limity vstupní/výstupní účtu úložiště toocheck hello.</span><span class="sxs-lookup"><span data-stu-id="3d3b4-277">Remember toocheck hello storage account ingress/egress limits when using this option.</span></span> <span data-ttu-id="3d3b4-278">V tématu [a cíle výkonnosti služby Azure Storage Scalability](storage-scalability-targets.md) podrobnosti.</span><span class="sxs-lookup"><span data-stu-id="3d3b4-278">See [Azure Storage Scalability and Performance Targets](storage-scalability-targets.md) for details.</span></span>

1. <span data-ttu-id="3d3b4-279">Stáhněte a nainstalujte AzCopy odsud: [nejnovější verzi AzCopy](http://aka.ms/downloadazcopy)</span><span class="sxs-lookup"><span data-stu-id="3d3b4-279">Download and install AzCopy from here: [Latest version of AzCopy](http://aka.ms/downloadazcopy)</span></span>
2. <span data-ttu-id="3d3b4-280">Otevřete prostředí Azure PowerShell a přejděte toohello složku, kde je nainstalován nástroj AzCopy.</span><span class="sxs-lookup"><span data-stu-id="3d3b4-280">Open Azure PowerShell and go toohello folder where AzCopy is installed.</span></span>
3. <span data-ttu-id="3d3b4-281">Použití hello následující příkaz toocopy hello souboru virtuálního pevného disku z "Zdroje" příliš "Cílové".</span><span class="sxs-lookup"><span data-stu-id="3d3b4-281">Use hello following command toocopy hello VHD file from "Source" too"Destination".</span></span>

    ```azcopy
    AzCopy /Source: <source> /SourceKey: <source-account-key> /Dest: <destination> /DestKey: <dest-account-key> /BlobType:page /Pattern: <file-name>
    ```

    <span data-ttu-id="3d3b4-282">Příklad:</span><span class="sxs-lookup"><span data-stu-id="3d3b4-282">Example:</span></span>

    ```azcopy
    AzCopy /Source:https://sourceaccount.blob.core.windows.net/mycontainer1 /SourceKey:key1 /Dest:https://destaccount.blob.core.windows.net/mycontainer2 /DestKey:key2 /Pattern:abc.vhd
    ```

    <span data-ttu-id="3d3b4-283">Zde je uveden popis hello parametry použité v hello AzCopy příkaz:</span><span class="sxs-lookup"><span data-stu-id="3d3b4-283">Here are descriptions of hello parameters used in hello AzCopy command:</span></span>

   * <span data-ttu-id="3d3b4-284">**/ Zdroj:  *&lt;zdroj&gt;:***  umístění složky hello nebo URL kontejneru úložiště, která obsahuje hello virtuálního pevného disku.</span><span class="sxs-lookup"><span data-stu-id="3d3b4-284">**/Source: *&lt;source&gt;:*** Location of hello folder or storage container URL that contains hello VHD.</span></span>
   * <span data-ttu-id="3d3b4-285">**/ SourceKey:  *&lt;klíč účtu zdroj&gt;:***  klíč účtu úložiště účtu úložiště hello zdroje.</span><span class="sxs-lookup"><span data-stu-id="3d3b4-285">**/SourceKey: *&lt;source-account-key&gt;:*** Storage account key of hello source storage account.</span></span>
   * <span data-ttu-id="3d3b4-286">**/ Cíle:  *&lt;cílové&gt;:***  toocopy URL kontejneru úložiště hello virtuálního pevného disku na.</span><span class="sxs-lookup"><span data-stu-id="3d3b4-286">**/Dest: *&lt;destination&gt;:*** Storage container URL toocopy hello VHD to.</span></span>
   * <span data-ttu-id="3d3b4-287">**/ DestKey:  *&lt;klíč účtu cíle&gt;:***  klíč účtu úložiště hello cílový účet úložiště.</span><span class="sxs-lookup"><span data-stu-id="3d3b4-287">**/DestKey: *&lt;dest-account-key&gt;:*** Storage account key of hello destination storage account.</span></span>
   * <span data-ttu-id="3d3b4-288">**/ Vzor:  *&lt;název souboru&gt;:***  zadejte název souboru hello toocopy hello virtuálního pevného disku.</span><span class="sxs-lookup"><span data-stu-id="3d3b4-288">**/Pattern: *&lt;file-name&gt;:*** Specify hello file name of hello VHD toocopy.</span></span>

<span data-ttu-id="3d3b4-289">Podrobnosti o použití nástroje AzCopy nástroje najdete v tématu [přenos dat pomocí příkazového řádku Azcopy hello](storage-use-azcopy.md).</span><span class="sxs-lookup"><span data-stu-id="3d3b4-289">For details on using AzCopy tool, see [Transfer data with hello AzCopy Command-Line Utility](storage-use-azcopy.md).</span></span>

##### <a name="option-2-copy-a-vhd-with-powershell-synchronized-copy"></a><span data-ttu-id="3d3b4-290">Možnost 2: Zkopírujte virtuální pevný disk pomocí prostředí PowerShell (Synchronized kopie)</span><span class="sxs-lookup"><span data-stu-id="3d3b4-290">Option 2: Copy a VHD with PowerShell (Synchronized copy)</span></span>
<span data-ttu-id="3d3b4-291">Můžete také zkopírovat soubor virtuálního pevného disku hello pomocí rutiny prostředí PowerShell hello AzureStorageBlobCopy spuštění.</span><span class="sxs-lookup"><span data-stu-id="3d3b4-291">You can also copy hello VHD file using hello PowerShell cmdlet Start-AzureStorageBlobCopy.</span></span> <span data-ttu-id="3d3b4-292">Použijte následující příkaz v prostředí Azure PowerShell toocopy virtuálního pevného disku hello.</span><span class="sxs-lookup"><span data-stu-id="3d3b4-292">Use hello following command on Azure PowerShell toocopy VHD.</span></span> <span data-ttu-id="3d3b4-293">Nahraďte hodnoty hello v <> odpovídající hodnoty z vašeho zdrojového a cílového účtu úložiště.</span><span class="sxs-lookup"><span data-stu-id="3d3b4-293">Replace hello values in <> with corresponding values from your source and destination storage account.</span></span> <span data-ttu-id="3d3b4-294">Tento příkaz, musíte mít volat virtuální pevné disky ve vašem účtu úložiště cílový kontejner toouse.</span><span class="sxs-lookup"><span data-stu-id="3d3b4-294">toouse this command, you must have a container called vhds in your destination storage account.</span></span> <span data-ttu-id="3d3b4-295">Pokud hello kontejner neexistuje, vytvořte ji před spuštěním příkazu hello.</span><span class="sxs-lookup"><span data-stu-id="3d3b4-295">If hello container doesn't exist, create one before running hello command.</span></span>

```powershell
$sourceBlobUri = <source-vhd-uri>

$sourceContext = New-AzureStorageContext  –StorageAccountName <source-account> -StorageAccountKey <source-account-key>

$destinationContext = New-AzureStorageContext  –StorageAccountName <dest-account> -StorageAccountKey <dest-account-key>

Start-AzureStorageBlobCopy -srcUri $sourceBlobUri -SrcContext $sourceContext -DestContainer <dest-container> -DestBlob <dest-disk-name> -DestContext $destinationContext
```

<span data-ttu-id="3d3b4-296">Příklad:</span><span class="sxs-lookup"><span data-stu-id="3d3b4-296">Example:</span></span>

```powershell
C:\PS> $sourceBlobUri = "https://sourceaccount.blob.core.windows.net/vhds/myvhd.vhd"

C:\PS> $sourceContext = New-AzureStorageContext  –StorageAccountName "sourceaccount" -StorageAccountKey "J4zUI9T5b8gvHohkiRg"

C:\PS> $destinationContext = New-AzureStorageContext  –StorageAccountName "destaccount" -StorageAccountKey "XZTmqSGKUYFSh7zB5"

C:\PS> Start-AzureStorageBlobCopy -srcUri $sourceBlobUri -SrcContext $sourceContext -DestContainer "vhds" -DestBlob "myvhd.vhd" -DestContext $destinationContext
```

### <span data-ttu-id="3d3b4-297"><a name="scenario2"></a>Scénář 2: "I mě migrace virtuálních počítačů z jiných platforem tooAzure Storage úrovně Premium."</span><span class="sxs-lookup"><span data-stu-id="3d3b4-297"><a name="scenario2"></a>Scenario 2: "I am migrating VMs from other platforms tooAzure Premium Storage."</span></span>
<span data-ttu-id="3d3b4-298">Pokud migrujete virtuální pevný disk z úložiště v Azure Cloud tooAzure, musíte napřed exportovat hello virtuálního pevného disku tooa místního adresáře.</span><span class="sxs-lookup"><span data-stu-id="3d3b4-298">If you are migrating VHD from non-Azure Cloud Storage tooAzure, you must first export hello VHD tooa local directory.</span></span> <span data-ttu-id="3d3b4-299">Cesta kompletní zdrojový hello hello místnímu adresáři, kde je uložený užitečný virtuálního pevného disku a potom použitím AzCopy tooupload ho tooAzure úložiště.</span><span class="sxs-lookup"><span data-stu-id="3d3b4-299">Have hello complete source path of hello local directory where VHD is stored handy, and then using AzCopy tooupload it tooAzure Storage.</span></span>

#### <a name="step-1-export-vhd-tooa-local-directory"></a><span data-ttu-id="3d3b4-300">Krok 1.</span><span class="sxs-lookup"><span data-stu-id="3d3b4-300">Step 1.</span></span> <span data-ttu-id="3d3b4-301">Export virtuálního pevného disku tooa místního adresáře</span><span class="sxs-lookup"><span data-stu-id="3d3b4-301">Export VHD tooa local directory</span></span>
##### <a name="copy-a-vhd-from-aws"></a><span data-ttu-id="3d3b4-302">Zkopírujte virtuální pevný disk z AWS</span><span class="sxs-lookup"><span data-stu-id="3d3b4-302">Copy a VHD from AWS</span></span>
1. <span data-ttu-id="3d3b4-303">Pokud používáte AWS, exportujte hello EC2 instance tooa virtuálního pevného disku v sady Amazon S3.</span><span class="sxs-lookup"><span data-stu-id="3d3b4-303">If you are using AWS, export hello EC2 instance tooa VHD in an Amazon S3 bucket.</span></span> <span data-ttu-id="3d3b4-304">Postupujte podle kroků hello popsaných v hello Amazon dokumentaci k rozhraní příkazového řádku (CLI) nástroj pro export instancí EC2 Amazon tooinstall hello Amazon EC2 a spusťte soubor VHD tooa hello EC2 hello vytvořit instanci export úkolů příkaz tooexport instance.</span><span class="sxs-lookup"><span data-stu-id="3d3b4-304">Follow hello steps described in hello Amazon documentation for Exporting Amazon EC2 Instances tooinstall hello Amazon EC2 command-line interface (CLI) tool and run hello create-instance-export-task command tooexport hello EC2 instance tooa VHD file.</span></span> <span data-ttu-id="3d3b4-305">Být jisti toouse **virtuálního pevného disku** pro hello DISK &#95; IMAGE &#95; Formát proměnné při spuštění hello **vytvořit instanci export úkolů** příkaz.</span><span class="sxs-lookup"><span data-stu-id="3d3b4-305">Be sure toouse **VHD** for hello DISK&#95;IMAGE&#95;FORMAT variable when running hello **create-instance-export-task** command.</span></span> <span data-ttu-id="3d3b4-306">Hello exportovaný soubor virtuálního pevného disku se uloží do sady hello Amazon S3, které určíte během tohoto procesu.</span><span class="sxs-lookup"><span data-stu-id="3d3b4-306">hello exported VHD file is saved in hello Amazon S3 bucket you designate during that process.</span></span>

    ```
    aws ec2 create-instance-export-task --instance-id ID --target-environment TARGET_ENVIRONMENT \
      --export-to-s3-task DiskImageFormat=DISK_IMAGE_FORMAT,ContainerFormat=ova,S3Bucket=BUCKET,S3Prefix=PREFIX
    ```

2. <span data-ttu-id="3d3b4-307">Stáhněte si soubor virtuálního pevného disku hello ze sady S3 hello.</span><span class="sxs-lookup"><span data-stu-id="3d3b4-307">Download hello VHD file from hello S3 bucket.</span></span> <span data-ttu-id="3d3b4-308">Vyberte hello souboru virtuálního pevného disku, pak **akce** > **Stáhnout**.</span><span class="sxs-lookup"><span data-stu-id="3d3b4-308">Select hello VHD file, then **Actions** > **Download**.</span></span>

    ![][3]

##### <a name="copy-a-vhd-from-other-non-azure-cloud"></a><span data-ttu-id="3d3b4-309">Zkopírujte virtuální pevný disk z jiné mimo Azure cloud</span><span class="sxs-lookup"><span data-stu-id="3d3b4-309">Copy a VHD from other non-Azure cloud</span></span>
<span data-ttu-id="3d3b4-310">Pokud migrujete virtuální pevný disk z úložiště v Azure Cloud tooAzure, musíte napřed exportovat hello virtuálního pevného disku tooa místního adresáře.</span><span class="sxs-lookup"><span data-stu-id="3d3b4-310">If you are migrating VHD from non-Azure Cloud Storage tooAzure, you must first export hello VHD tooa local directory.</span></span> <span data-ttu-id="3d3b4-311">Zkopírujte hello dokončení zdrojová cesta hello místnímu adresáři, kde je uložený virtuální pevný disk.</span><span class="sxs-lookup"><span data-stu-id="3d3b4-311">Copy hello complete source path of hello local directory where VHD is stored.</span></span>

##### <a name="copy-a-vhd-from-on-premises"></a><span data-ttu-id="3d3b4-312">Zkopírujte virtuální pevný disk z místní</span><span class="sxs-lookup"><span data-stu-id="3d3b4-312">Copy a VHD from on-premises</span></span>
<span data-ttu-id="3d3b4-313">Pokud migrujete virtuální pevný disk z místního prostředí, budete potřebovat hello kompletní zdrojový cestu, kde je uložený virtuální pevný disk.</span><span class="sxs-lookup"><span data-stu-id="3d3b4-313">If you are migrating VHD from an on-premises environment, you will need hello complete source path where VHD is stored.</span></span> <span data-ttu-id="3d3b4-314">Hello zdrojová cesta může být server umístění nebo sdílené složky.</span><span class="sxs-lookup"><span data-stu-id="3d3b4-314">hello source path could be a server location or file share.</span></span>

#### <a name="step-2-create-hello-destination-for-your-vhd"></a><span data-ttu-id="3d3b4-315">Krok 2.</span><span class="sxs-lookup"><span data-stu-id="3d3b4-315">Step 2.</span></span> <span data-ttu-id="3d3b4-316">Vytvoření hello cíl pro svůj disk VHD</span><span class="sxs-lookup"><span data-stu-id="3d3b4-316">Create hello destination for your VHD</span></span>
<span data-ttu-id="3d3b4-317">Vytvořte účet úložiště pro údržbu virtuální pevné disky.</span><span class="sxs-lookup"><span data-stu-id="3d3b4-317">Create a storage account for maintaining your VHDs.</span></span> <span data-ttu-id="3d3b4-318">Vezměte v úvahu následující body při plánování kde hello toostore virtuální pevné disky:</span><span class="sxs-lookup"><span data-stu-id="3d3b4-318">Consider hello following points when planning where toostore your VHDs:</span></span>

* <span data-ttu-id="3d3b4-319">Hello cílový účet úložiště může být úložiště standard nebo premium, v závislosti na požadavcích vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="3d3b4-319">hello target storage account could be standard or premium storage depending on your application requirement.</span></span>
* <span data-ttu-id="3d3b4-320">Hello oblast účtu úložiště musí být stejná jako úložiště Premium podporuje virtuální počítače Azure v konečné fázi hello vytvoříte.</span><span class="sxs-lookup"><span data-stu-id="3d3b4-320">hello storage account region must be same as Premium Storage capable Azure VMs you will create in hello final stage.</span></span> <span data-ttu-id="3d3b4-321">Může zkopírovat tooa nový účet úložiště, nebo plán toouse hello stejný účet úložiště podle vašich potřeb.</span><span class="sxs-lookup"><span data-stu-id="3d3b4-321">You could copy tooa new storage account, or plan toouse hello same storage account based on your needs.</span></span>
* <span data-ttu-id="3d3b4-322">Zkopírujte a uložte klíč účtu úložiště hello hello cílový účet úložiště pro další fáze hello.</span><span class="sxs-lookup"><span data-stu-id="3d3b4-322">Copy and save hello storage account key of hello destination storage account for hello next stage.</span></span>

<span data-ttu-id="3d3b4-323">Důrazně doporučujeme přesouvání všech dat pro produkční zatížení toouse premium storage.</span><span class="sxs-lookup"><span data-stu-id="3d3b4-323">We strongly recommend you moving all data for production workload toouse premium storage.</span></span>

#### <a name="step-3-upload-hello-vhd-tooazure-storage"></a><span data-ttu-id="3d3b4-324">Krok 3.</span><span class="sxs-lookup"><span data-stu-id="3d3b4-324">Step 3.</span></span> <span data-ttu-id="3d3b4-325">Nahrání virtuálního pevného disku tooAzure hello úložiště</span><span class="sxs-lookup"><span data-stu-id="3d3b4-325">Upload hello VHD tooAzure Storage</span></span>
<span data-ttu-id="3d3b4-326">Nyní když máte v místních adresářových hello svůj disk VHD, můžete použít AzCopy nebo AzurePowerShell tooupload hello VHD soubor tooAzure úložiště.</span><span class="sxs-lookup"><span data-stu-id="3d3b4-326">Now that you have your VHD in hello local directory, you can use AzCopy or AzurePowerShell tooupload hello .vhd file tooAzure Storage.</span></span> <span data-ttu-id="3d3b4-327">Obě možnosti jsou k dispozici zde:</span><span class="sxs-lookup"><span data-stu-id="3d3b4-327">Both options are provided here:</span></span>

##### <a name="option-1-using-azure-powershell-add-azurevhd-tooupload-hello-vhd-file"></a><span data-ttu-id="3d3b4-328">Možnost 1: Použití Azure PowerShell Add-AzureVhd tooupload hello VHD souboru</span><span class="sxs-lookup"><span data-stu-id="3d3b4-328">Option 1: Using Azure PowerShell Add-AzureVhd tooupload hello .vhd file</span></span>

```powershell
Add-AzureVhd [-Destination] <Uri> [-LocalFilePath] <FileInfo>
```

<span data-ttu-id="3d3b4-329">Příklad <Uri> může být ***"https://storagesample.blob.core.windows.net/mycontainer/blob1.vhd"***.</span><span class="sxs-lookup"><span data-stu-id="3d3b4-329">An example <Uri> might be ***"https://storagesample.blob.core.windows.net/mycontainer/blob1.vhd"***.</span></span> <span data-ttu-id="3d3b4-330">Příklad <FileInfo> může být ***"C:\path\to\upload.vhd"***.</span><span class="sxs-lookup"><span data-stu-id="3d3b4-330">An example <FileInfo> might be ***"C:\path\to\upload.vhd"***.</span></span>

##### <a name="option-2-using-azcopy-tooupload-hello-vhd-file"></a><span data-ttu-id="3d3b4-331">Možnost 2: Pomocí souboru VHD hello tooupload AzCopy</span><span class="sxs-lookup"><span data-stu-id="3d3b4-331">Option 2: Using AzCopy tooupload hello .vhd file</span></span>
<span data-ttu-id="3d3b4-332">Pomocí AzCopy, můžete snadno odeslat hello VHD přes hello Internetu.</span><span class="sxs-lookup"><span data-stu-id="3d3b4-332">Using AzCopy, you can easily upload hello VHD over hello Internet.</span></span> <span data-ttu-id="3d3b4-333">V závislosti na velikosti hello hello virtuálních pevných disků může to trvat čas.</span><span class="sxs-lookup"><span data-stu-id="3d3b4-333">Depending on hello size of hello VHDs, this may take time.</span></span> <span data-ttu-id="3d3b4-334">Při použití této možnosti mějte na paměti limity vstupní/výstupní účtu úložiště toocheck hello.</span><span class="sxs-lookup"><span data-stu-id="3d3b4-334">Remember toocheck hello storage account ingress/egress limits when using this option.</span></span> <span data-ttu-id="3d3b4-335">V tématu [a cíle výkonnosti služby Azure Storage Scalability](storage-scalability-targets.md) podrobnosti.</span><span class="sxs-lookup"><span data-stu-id="3d3b4-335">See [Azure Storage Scalability and Performance Targets](storage-scalability-targets.md) for details.</span></span>

1. <span data-ttu-id="3d3b4-336">Stáhněte a nainstalujte AzCopy odsud: [nejnovější verzi AzCopy](http://aka.ms/downloadazcopy)</span><span class="sxs-lookup"><span data-stu-id="3d3b4-336">Download and install AzCopy from here: [Latest version of AzCopy](http://aka.ms/downloadazcopy)</span></span>
2. <span data-ttu-id="3d3b4-337">Otevřete prostředí Azure PowerShell a přejděte toohello složku, kde je nainstalován nástroj AzCopy.</span><span class="sxs-lookup"><span data-stu-id="3d3b4-337">Open Azure PowerShell and go toohello folder where AzCopy is installed.</span></span>
3. <span data-ttu-id="3d3b4-338">Použití hello následující příkaz toocopy hello souboru virtuálního pevného disku z "Zdroje" příliš "Cílové".</span><span class="sxs-lookup"><span data-stu-id="3d3b4-338">Use hello following command toocopy hello VHD file from "Source" too"Destination".</span></span>

    ```azcopy
    AzCopy /Source: <source> /SourceKey: <source-account-key> /Dest: <destination> /DestKey: <dest-account-key> /BlobType:page /Pattern: <file-name>
    ```

    <span data-ttu-id="3d3b4-339">Příklad:</span><span class="sxs-lookup"><span data-stu-id="3d3b4-339">Example:</span></span>

    ```azcopy
    AzCopy /Source:https://sourceaccount.blob.core.windows.net/mycontainer1 /SourceKey:key1 /Dest:https://destaccount.blob.core.windows.net/mycontainer2 /DestKey:key2 /BlobType:page /Pattern:abc.vhd
    ```

    <span data-ttu-id="3d3b4-340">Zde je uveden popis hello parametry použité v hello AzCopy příkaz:</span><span class="sxs-lookup"><span data-stu-id="3d3b4-340">Here are descriptions of hello parameters used in hello AzCopy command:</span></span>

   * <span data-ttu-id="3d3b4-341">**/ Zdroj:  *&lt;zdroj&gt;:***  umístění složky hello nebo URL kontejneru úložiště, která obsahuje hello virtuálního pevného disku.</span><span class="sxs-lookup"><span data-stu-id="3d3b4-341">**/Source: *&lt;source&gt;:*** Location of hello folder or storage container URL that contains hello VHD.</span></span>
   * <span data-ttu-id="3d3b4-342">**/ SourceKey:  *&lt;klíč účtu zdroj&gt;:***  klíč účtu úložiště účtu úložiště hello zdroje.</span><span class="sxs-lookup"><span data-stu-id="3d3b4-342">**/SourceKey: *&lt;source-account-key&gt;:*** Storage account key of hello source storage account.</span></span>
   * <span data-ttu-id="3d3b4-343">**/ Cíle:  *&lt;cílové&gt;:***  toocopy URL kontejneru úložiště hello virtuálního pevného disku na.</span><span class="sxs-lookup"><span data-stu-id="3d3b4-343">**/Dest: *&lt;destination&gt;:*** Storage container URL toocopy hello VHD to.</span></span>
   * <span data-ttu-id="3d3b4-344">**/ DestKey:  *&lt;klíč účtu cíle&gt;:***  klíč účtu úložiště hello cílový účet úložiště.</span><span class="sxs-lookup"><span data-stu-id="3d3b4-344">**/DestKey: *&lt;dest-account-key&gt;:*** Storage account key of hello destination storage account.</span></span>
   * <span data-ttu-id="3d3b4-345">**/ BlobType: stránka:** Určuje, že cíl hello je objekt blob stránky.</span><span class="sxs-lookup"><span data-stu-id="3d3b4-345">**/BlobType: page:** Specifies that hello destination is a page blob.</span></span>
   * <span data-ttu-id="3d3b4-346">**/ Vzor:  *&lt;název souboru&gt;:***  zadejte název souboru hello toocopy hello virtuálního pevného disku.</span><span class="sxs-lookup"><span data-stu-id="3d3b4-346">**/Pattern: *&lt;file-name&gt;:*** Specify hello file name of hello VHD toocopy.</span></span>

<span data-ttu-id="3d3b4-347">Podrobnosti o použití nástroje AzCopy nástroje najdete v tématu [přenos dat pomocí příkazového řádku Azcopy hello](storage-use-azcopy.md).</span><span class="sxs-lookup"><span data-stu-id="3d3b4-347">For details on using AzCopy tool, see [Transfer data with hello AzCopy Command-Line Utility](storage-use-azcopy.md).</span></span>

##### <a name="other-options-for-uploading-a-vhd"></a><span data-ttu-id="3d3b4-348">Další možnosti pro odesílání virtuálního pevného disku</span><span class="sxs-lookup"><span data-stu-id="3d3b4-348">Other options for uploading a VHD</span></span>
<span data-ttu-id="3d3b4-349">Můžete také nahrát účtu úložiště tooyour virtuální pevný disk pomocí jedné z hello následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="3d3b4-349">You can also upload a VHD tooyour storage account using one of hello following means:</span></span>

* [<span data-ttu-id="3d3b4-350">Objekt Blob služby Azure Storage kopie rozhraní API</span><span class="sxs-lookup"><span data-stu-id="3d3b4-350">Azure Storage Copy Blob API</span></span>](https://msdn.microsoft.com/library/azure/dd894037.aspx)
* [<span data-ttu-id="3d3b4-351">Objektů BLOB Azure Storage Explorer odesílání</span><span class="sxs-lookup"><span data-stu-id="3d3b4-351">Azure Storage Explorer Uploading Blobs</span></span>](https://azurestorageexplorer.codeplex.com/)
* [<span data-ttu-id="3d3b4-352">Referenční dokumentace rozhraní API úložiště importu/exportu služby REST</span><span class="sxs-lookup"><span data-stu-id="3d3b4-352">Storage Import/Export Service REST API Reference</span></span>](https://msdn.microsoft.com/library/dn529096.aspx)

> [!NOTE]
> <span data-ttu-id="3d3b4-353">Doporučujeme používat službu Import/Export, pokud je předpokládaná doba odesílání doba je delší než 7 dní.</span><span class="sxs-lookup"><span data-stu-id="3d3b4-353">We recommend using Import/Export Service if estimated uploading time is longer than 7 days.</span></span> <span data-ttu-id="3d3b4-354">Můžete použít [DataTransferSpeedCalculator](https://github.com/Azure-Samples/storage-dotnet-import-export-job-management/blob/master/DataTransferSpeedCalculator.html) čas hello tooestimate z jednotky velikost a přenos dat.</span><span class="sxs-lookup"><span data-stu-id="3d3b4-354">You can use [DataTransferSpeedCalculator](https://github.com/Azure-Samples/storage-dotnet-import-export-job-management/blob/master/DataTransferSpeedCalculator.html) tooestimate hello time from data size and transfer unit.</span></span>
>
> <span data-ttu-id="3d3b4-355">Import a Export lze použít toocopy tooa standardní účet úložiště.</span><span class="sxs-lookup"><span data-stu-id="3d3b4-355">Import/Export can be used toocopy tooa standard storage account.</span></span> <span data-ttu-id="3d3b4-356">Budete potřebovat toocopy z účtu úložiště toopremium standardní úložiště pomocí nástroje, například AzCopy.</span><span class="sxs-lookup"><span data-stu-id="3d3b4-356">You will need toocopy from standard storage toopremium storage account using a tool like AzCopy.</span></span>
>
>

## <span data-ttu-id="3d3b4-357"><a name="create-azure-virtual-machine-using-premium-storage"></a>Vytvořit virtuální počítače Azure pomocí služby Storage úrovně Premium</span><span class="sxs-lookup"><span data-stu-id="3d3b4-357"><a name="create-azure-virtual-machine-using-premium-storage"></a>Create Azure VMs using Premium Storage</span></span>
<span data-ttu-id="3d3b4-358">Po hello virtuálního pevného disku je nahrané nebo zkopírovaný toohello potřeby účet úložiště, postupujte podle pokynů hello v této části tooregister hello virtuálního pevného disku jako bitovou kopii operačního systému, nebo disk operačního systému v závislosti na vašem scénáři a pak vytvořte instanci virtuálního počítače z něj.</span><span class="sxs-lookup"><span data-stu-id="3d3b4-358">After hello VHD is uploaded or copied toohello desired storage account, follow hello instructions in this section tooregister hello VHD as an OS image, or OS disk depending on your scenario and then create a VM instance from it.</span></span> <span data-ttu-id="3d3b4-359">Hello datový disk VHD může být připojené toohello virtuálního počítače, po jejím vytvoření.</span><span class="sxs-lookup"><span data-stu-id="3d3b4-359">hello data disk VHD can be attached toohello VM once it is created.</span></span>
<span data-ttu-id="3d3b4-360">Ukázkový skript migrace je k dispozici na konci hello v této části.</span><span class="sxs-lookup"><span data-stu-id="3d3b4-360">A sample migration script is provided at hello end of this section.</span></span> <span data-ttu-id="3d3b4-361">Tento jednoduchý skript neodpovídá všechny scénáře.</span><span class="sxs-lookup"><span data-stu-id="3d3b4-361">This simple script does not match all scenarios.</span></span> <span data-ttu-id="3d3b4-362">Může být nutné tooupdate hello skriptu toomatch s konkrétní scénář.</span><span class="sxs-lookup"><span data-stu-id="3d3b4-362">You may need tooupdate hello script toomatch with your specific scenario.</span></span> <span data-ttu-id="3d3b4-363">Níže najdete toosee, pokud se tento skript vztahuje tooyour scénáři [A ukázkový migrace skript](#a-sample-migration-script).</span><span class="sxs-lookup"><span data-stu-id="3d3b4-363">toosee if this script applies tooyour scenario, see below [A Sample Migration Script](#a-sample-migration-script).</span></span>

### <a name="checklist"></a><span data-ttu-id="3d3b4-364">Kontrolní seznam</span><span class="sxs-lookup"><span data-stu-id="3d3b4-364">Checklist</span></span>
1. <span data-ttu-id="3d3b4-365">Počkat, až všechny disky VHD hello kopírování je dokončena.</span><span class="sxs-lookup"><span data-stu-id="3d3b4-365">Wait until all hello VHD disks copying is complete.</span></span>
2. <span data-ttu-id="3d3b4-366">Ujistěte se, že úložiště Premium je k dispozici v hello oblasti, které provádíte migraci do.</span><span class="sxs-lookup"><span data-stu-id="3d3b4-366">Make sure Premium Storage is available in hello region you are migrating to.</span></span>
3. <span data-ttu-id="3d3b4-367">Rozhodněte, hello nový virtuální počítač řad, které budete používat.</span><span class="sxs-lookup"><span data-stu-id="3d3b4-367">Decide hello new VM series you will be using.</span></span> <span data-ttu-id="3d3b4-368">Měla by být schopný úložiště Premium a hello velikost by měla být v závislosti na dostupnosti hello v oblasti hello a podle potřeb.</span><span class="sxs-lookup"><span data-stu-id="3d3b4-368">It should be a Premium Storage capable, and hello size should be depending on hello availability in hello region and based on your needs.</span></span>
4. <span data-ttu-id="3d3b4-369">Rozhodněte, hello přesnou velikost virtuálního počítače, které budete používat.</span><span class="sxs-lookup"><span data-stu-id="3d3b4-369">Decide hello exact VM size you will use.</span></span> <span data-ttu-id="3d3b4-370">Velikost virtuálního počítače musí toobe dostatečně velké na to toosupport hello počet datových disků, které máte.</span><span class="sxs-lookup"><span data-stu-id="3d3b4-370">VM size needs toobe large enough toosupport hello number of data disks you have.</span></span> <span data-ttu-id="3d3b4-371">Například</span><span class="sxs-lookup"><span data-stu-id="3d3b4-371">E.g.</span></span> <span data-ttu-id="3d3b4-372">Pokud máte 4 datových disků, musí mít hello virtuálních počítačů 2 nebo více jader.</span><span class="sxs-lookup"><span data-stu-id="3d3b4-372">if you have 4 data disks, hello VM must have 2 or more cores.</span></span> <span data-ttu-id="3d3b4-373">Zvažte také výpočetní výkon, paměti a šířky pásma sítě musí.</span><span class="sxs-lookup"><span data-stu-id="3d3b4-373">Also, consider processing power, memory and network bandwidth needs.</span></span>
5. <span data-ttu-id="3d3b4-374">Vytvořte účet úložiště Premium hello cílová oblast.</span><span class="sxs-lookup"><span data-stu-id="3d3b4-374">Create a Premium Storage account in hello target region.</span></span> <span data-ttu-id="3d3b4-375">Toto je účet hello budete používat pro hello nového virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="3d3b4-375">This is hello account you will use for hello new VM.</span></span>
6. <span data-ttu-id="3d3b4-376">Máte hello aktuální virtuální počítač podrobnosti o užitečný, včetně hello seznam disků a odpovídající BLOB VHD.</span><span class="sxs-lookup"><span data-stu-id="3d3b4-376">Have hello current VM details handy, including hello list of disks and corresponding VHD blobs.</span></span>

<span data-ttu-id="3d3b4-377">Připravte aplikaci výpadek.</span><span class="sxs-lookup"><span data-stu-id="3d3b4-377">Prepare your application for downtime.</span></span> <span data-ttu-id="3d3b4-378">toodo čisté migrace, máte toostop veškeré zpracování hello v aktuálním systému hello.</span><span class="sxs-lookup"><span data-stu-id="3d3b4-378">toodo a clean migration, you have toostop all hello processing in hello current system.</span></span> <span data-ttu-id="3d3b4-379">Pak můžete ho získat tooconsistent stavu, který je možné migrovat toohello nová platforma.</span><span class="sxs-lookup"><span data-stu-id="3d3b4-379">Only then you can get it tooconsistent state which you can migrate toohello new platform.</span></span> <span data-ttu-id="3d3b4-380">Doba trvání výpadku bude záviset na hello množství dat v toomigrate disky hello.</span><span class="sxs-lookup"><span data-stu-id="3d3b4-380">Downtime duration will depend on hello amount of data in hello disks toomigrate.</span></span>

> [!NOTE]
> <span data-ttu-id="3d3b4-381">Pokud vytváříte virtuální počítač Azure Resource Manager z specializované disku VHD, naleznete příliš[této šablony](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vm-specialized-vhd) pro nasazení Resource Manager virtuálního počítače pomocí stávající disk.</span><span class="sxs-lookup"><span data-stu-id="3d3b4-381">If you are creating an Azure Resource Manager VM from a specialized VHD Disk, please refer too[this template](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vm-specialized-vhd) for deploying Resource Manager VM using existing disk.</span></span>
>
>

### <a name="register-your-vhd"></a><span data-ttu-id="3d3b4-382">Zaregistrovat svůj disk VHD</span><span class="sxs-lookup"><span data-stu-id="3d3b4-382">Register your VHD</span></span>
<span data-ttu-id="3d3b4-383">toocreate virtuálního počítače z virtuálního pevného disku operačního systému nebo tooattach tooa disku data nového virtuálního počítače, nejprve je nutné zaregistrovat je.</span><span class="sxs-lookup"><span data-stu-id="3d3b4-383">toocreate a VM from OS VHD or tooattach a data disk tooa new VM, you must first register them.</span></span> <span data-ttu-id="3d3b4-384">Postupujte podle kroků v závislosti na scénáři svůj disk VHD.</span><span class="sxs-lookup"><span data-stu-id="3d3b4-384">Follow steps below depending on your VHD's scenario.</span></span>

#### <a name="generalized-operating-system-vhd-toocreate-multiple-azure-vm-instances"></a><span data-ttu-id="3d3b4-385">Zobecněný virtuální pevný disk operačního systému toocreate více instancí virtuálního počítače Azure</span><span class="sxs-lookup"><span data-stu-id="3d3b4-385">Generalized Operating System VHD toocreate multiple Azure VM instances</span></span>
<span data-ttu-id="3d3b4-386">Po odeslání účet úložiště toohello virtuální pevný disk zobecněný bitovou kopii operačního systému ji jako zaregistrovat **Image virtuálního počítače Azure** tak, aby z ní můžete vytvořit jeden nebo více instancí virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="3d3b4-386">After generalized OS image VHD is uploaded toohello storage account, register it as an **Azure VM Image** so that you can create one or more VM instances from it.</span></span> <span data-ttu-id="3d3b4-387">Použijte svůj disk VHD hello následující tooregister rutiny prostředí PowerShell jako image operačního systému virtuálního počítače Azure.</span><span class="sxs-lookup"><span data-stu-id="3d3b4-387">Use hello following PowerShell cmdlets tooregister your VHD as an Azure VM OS image.</span></span> <span data-ttu-id="3d3b4-388">Zadejte adresu URL dokončení kontejneru hello kde virtuálního pevného disku byla zkopírována do.</span><span class="sxs-lookup"><span data-stu-id="3d3b4-388">Provide hello complete container URL where VHD was copied to.</span></span>

```powershell
Add-AzureVMImage -ImageName "OSImageName" -MediaLocation "https://storageaccount.blob.core.windows.net/vhdcontainer/osimage.vhd" -OS Windows
```

<span data-ttu-id="3d3b4-389">Zkopírujte a uložte hello název tuto novou bitovou kopii virtuálního počítače Azure.</span><span class="sxs-lookup"><span data-stu-id="3d3b4-389">Copy and save hello name of this new Azure VM Image.</span></span> <span data-ttu-id="3d3b4-390">V předchozím příkladu hello, je *OSImageName*.</span><span class="sxs-lookup"><span data-stu-id="3d3b4-390">In hello example above, it is *OSImageName*.</span></span>

#### <a name="unique-operating-system-vhd-toocreate-a-single-azure-vm-instance"></a><span data-ttu-id="3d3b4-391">Jedinečný virtuální pevný disk operačního systému toocreate jedna instance virtuálního počítače Azure</span><span class="sxs-lookup"><span data-stu-id="3d3b4-391">Unique Operating System VHD toocreate a single Azure VM instance</span></span>
<span data-ttu-id="3d3b4-392">Po hello jedinečný virtuální pevný disk operačního systému je úložiště nahrané toohello účet, zaregistrujte si ho jako **Disk s operačním systémem Azure** tak, aby instance virtuálního počítače můžete vytvořit z něj.</span><span class="sxs-lookup"><span data-stu-id="3d3b4-392">After hello unique OS VHD is uploaded toohello storage account, register it as an **Azure OS Disk** so that you can create a VM instance from it.</span></span> <span data-ttu-id="3d3b4-393">Použijte tyto tooregister rutiny prostředí PowerShell svůj disk VHD jako Disk operačního systému Azure.</span><span class="sxs-lookup"><span data-stu-id="3d3b4-393">Use these PowerShell cmdlets tooregister your VHD as an Azure OS Disk.</span></span> <span data-ttu-id="3d3b4-394">Zadejte adresu URL dokončení kontejneru hello kde virtuálního pevného disku byla zkopírována do.</span><span class="sxs-lookup"><span data-stu-id="3d3b4-394">Provide hello complete container URL where VHD was copied to.</span></span>

```powershell
Add-AzureDisk -DiskName "OSDisk" -MediaLocation "https://storageaccount.blob.core.windows.net/vhdcontainer/osdisk.vhd" -Label "My OS Disk" -OS "Windows"
```

<span data-ttu-id="3d3b4-395">Zkopírujte a uložte hello název tento nový Disk operačního systému Azure.</span><span class="sxs-lookup"><span data-stu-id="3d3b4-395">Copy and save hello name of this new Azure OS Disk.</span></span> <span data-ttu-id="3d3b4-396">V předchozím příkladu hello, je *OSDisk*.</span><span class="sxs-lookup"><span data-stu-id="3d3b4-396">In hello example above, it is *OSDisk*.</span></span>

#### <a name="data-disk-vhd-toobe-attached-toonew-azure-vm-instances"></a><span data-ttu-id="3d3b4-397">Připojit datový disk VHD toobe toonew instance virtuálního počítače Azure</span><span class="sxs-lookup"><span data-stu-id="3d3b4-397">Data disk VHD toobe attached toonew Azure VM instance(s)</span></span>
<span data-ttu-id="3d3b4-398">Po hello datový disk VHD je odeslání toostorage účet, zaregistrovat ji jako datový Disk Azure tak, aby bylo možné ho připojené tooyour nové DS Series, DSv2 series nebo GS virtuálního počítače Azure řady instance.</span><span class="sxs-lookup"><span data-stu-id="3d3b4-398">After hello data disk VHD is uploaded toostorage account, register it as an Azure Data Disk so that it can be attached tooyour new DS Series, DSv2 series or GS Series Azure VM instance.</span></span>

<span data-ttu-id="3d3b4-399">Použijte tyto tooregister rutiny prostředí PowerShell svůj disk VHD jako datový Disk Azure.</span><span class="sxs-lookup"><span data-stu-id="3d3b4-399">Use these PowerShell cmdlets tooregister your VHD as an Azure Data Disk.</span></span> <span data-ttu-id="3d3b4-400">Zadejte adresu URL dokončení kontejneru hello kde virtuálního pevného disku byla zkopírována do.</span><span class="sxs-lookup"><span data-stu-id="3d3b4-400">Provide hello complete container URL where VHD was copied to.</span></span>

```powershell
Add-AzureDisk -DiskName "DataDisk" -MediaLocation "https://storageaccount.blob.core.windows.net/vhdcontainer/datadisk.vhd" -Label "My Data Disk"
```

<span data-ttu-id="3d3b4-401">Zkopírujte a uložte hello název tento nový datový Disk Azure.</span><span class="sxs-lookup"><span data-stu-id="3d3b4-401">Copy and save hello name of this new Azure Data Disk.</span></span> <span data-ttu-id="3d3b4-402">V předchozím příkladu hello, je *DataDisk*.</span><span class="sxs-lookup"><span data-stu-id="3d3b4-402">In hello example above, it is *DataDisk*.</span></span>

### <a name="create-a-premium-storage-capable-vm"></a><span data-ttu-id="3d3b4-403">Vytvoření virtuálního počítače s podporou služby Storage úrovně Premium</span><span class="sxs-lookup"><span data-stu-id="3d3b4-403">Create a Premium Storage capable VM</span></span>
<span data-ttu-id="3d3b4-404">Jednou hello bitové kopie operačního systému nebo disk operačního systému jsou zaregistrovány, vytvořte nový DS-series, DSv2-series nebo GS-series virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="3d3b4-404">Once hello OS image or OS disk are registered, create a new DS-series, DSv2-series or GS-series VM.</span></span> <span data-ttu-id="3d3b4-405">Budete používat hello bitovou kopii operačního systému nebo název disku operačního systému, který je zaregistrovaný.</span><span class="sxs-lookup"><span data-stu-id="3d3b4-405">You will be using hello operating system image or operating system disk name that you registered.</span></span> <span data-ttu-id="3d3b4-406">Vyberte typ hello virtuálního počítače z vrstvy úložiště Premium hello.</span><span class="sxs-lookup"><span data-stu-id="3d3b4-406">Select hello VM type from hello Premium Storage tier.</span></span> <span data-ttu-id="3d3b4-407">V následujícím příkladu používáme hello *Standard_DS2* velikost virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="3d3b4-407">In example below, we are using hello *Standard_DS2* VM size.</span></span>

> [!NOTE]
> <span data-ttu-id="3d3b4-408">Aktualizace se, že odpovídá kapacitu a požadavky na výkon a velikosti disku Azure hello toomake velikost disku hello.</span><span class="sxs-lookup"><span data-stu-id="3d3b4-408">Update hello disk size toomake sure it matches your capacity and performance requirements and hello available Azure disk sizes.</span></span>
>
>

<span data-ttu-id="3d3b4-409">Rutiny Powershellu krok za krokem postupujte podle hello níže toocreate hello nového virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="3d3b4-409">Follow hello step by step PowerShell cmdlets below toocreate hello new VM.</span></span> <span data-ttu-id="3d3b4-410">Nastavte nejprve, hello společné parametry:</span><span class="sxs-lookup"><span data-stu-id="3d3b4-410">First, set hello common parameters:</span></span>

```powershell
$serviceName = "yourVM"
$location = "location-name" (e.g., West US)
$vmSize ="Standard_DS2"
$adminUser = "youradmin"
$adminPassword = "yourpassword"
$vmName ="yourVM"
$vmSize = "Standard_DS2"
```

<span data-ttu-id="3d3b4-411">Nejprve vytvořte Cloudová služba, ve kterém bude hostitelem nové virtuální počítače.</span><span class="sxs-lookup"><span data-stu-id="3d3b4-411">First, create a cloud service in which you will be hosting your new VMs.</span></span>

```powershell
New-AzureService -ServiceName $serviceName -Location $location
```

<span data-ttu-id="3d3b4-412">Dále v závislosti na scénáři, vytvořte instanci virtuálního počítače Azure hello z hello bitovou kopii operačního systému nebo Disk operačního systému, který je zaregistrovaný.</span><span class="sxs-lookup"><span data-stu-id="3d3b4-412">Next, depending on your scenario, create hello Azure VM instance from either hello OS Image or OS Disk that you registered.</span></span>

#### <a name="generalized-operating-system-vhd-toocreate-multiple-azure-vm-instances"></a><span data-ttu-id="3d3b4-413">Zobecněný virtuální pevný disk operačního systému toocreate více instancí virtuálního počítače Azure</span><span class="sxs-lookup"><span data-stu-id="3d3b4-413">Generalized Operating System VHD toocreate multiple Azure VM instances</span></span>
<span data-ttu-id="3d3b4-414">Vytvořte jeden nebo více nový virtuální počítač Azure řady DS instancí pomocí hello hello **bitovou kopii operačního systému služby Azure** který je zaregistrovaný.</span><span class="sxs-lookup"><span data-stu-id="3d3b4-414">Create hello one or more new DS Series Azure VM instances using hello **Azure OS Image** that you registered.</span></span> <span data-ttu-id="3d3b4-415">Při vytváření nového virtuálního počítače, jak je uvedeno níže, zadejte název této bitové kopie operačního systému v konfiguraci virtuálního počítače hello.</span><span class="sxs-lookup"><span data-stu-id="3d3b4-415">Specify this OS Image name in hello VM configuration when creating new VM as shown below.</span></span>

```powershell
$OSImage = Get-AzureVMImage –ImageName "OSImageName"

$vm = New-AzureVMConfig -Name $vmName –InstanceSize $vmSize -ImageName $OSImage.ImageName

Add-AzureProvisioningConfig -Windows –AdminUserName $adminUser -Password $adminPassword –VM $vm

New-AzureVM -ServiceName $serviceName -VM $vm
```

#### <a name="unique-operating-system-vhd-toocreate-a-single-azure-vm-instance"></a><span data-ttu-id="3d3b4-416">Jedinečný virtuální pevný disk operačního systému toocreate jedna instance virtuálního počítače Azure</span><span class="sxs-lookup"><span data-stu-id="3d3b4-416">Unique Operating System VHD toocreate a single Azure VM instance</span></span>
<span data-ttu-id="3d3b4-417">Vytvoření nové služby DS řady virtuálního počítače Azure instance pomocí hello **Disk s operačním systémem Azure** který je zaregistrovaný.</span><span class="sxs-lookup"><span data-stu-id="3d3b4-417">Create a new DS series Azure VM instance using hello **Azure OS Disk** that you registered.</span></span> <span data-ttu-id="3d3b4-418">Při vytváření nového virtuálního počítače hello, jak je uvedeno níže, zadejte v konfiguraci virtuálního počítače hello tento název disku operačního systému.</span><span class="sxs-lookup"><span data-stu-id="3d3b4-418">Specify this OS Disk name in hello VM configuration when creating hello new VM as shown below.</span></span>

```powershell
$OSDisk = Get-AzureDisk –DiskName "OSDisk"

$vm = New-AzureVMConfig -Name $vmName -InstanceSize $vmSize -DiskName $OSDisk.DiskName

New-AzureVM -ServiceName $serviceName –VM $vm
```

<span data-ttu-id="3d3b4-419">Zadejte další virtuální počítač Azure informace, například cloudové služby, oblast, účet úložiště, skupinu dostupnosti a zásady ukládání do mezipaměti.</span><span class="sxs-lookup"><span data-stu-id="3d3b4-419">Specify other Azure VM information, such as a cloud service, region, storage account, availability set, and caching policy.</span></span> <span data-ttu-id="3d3b4-420">Poznámka: hello instance virtuálního počítače musí být umístěn společně s přidružené operační systém nebo datové disky, takže hello vybrané cloudové služby, oblast a účet úložiště musí být v hello stejné umístění jako hello základní virtuální pevné disky těchto disků.</span><span class="sxs-lookup"><span data-stu-id="3d3b4-420">Note that hello VM instance must be co-located with associated operating system or data disks, so hello selected cloud service, region and storage account must all be in hello same location as hello underlying VHDs of those disks.</span></span>

### <a name="attach-data-disk"></a><span data-ttu-id="3d3b4-421">Připojení datového disku</span><span class="sxs-lookup"><span data-stu-id="3d3b4-421">Attach data disk</span></span>
<span data-ttu-id="3d3b4-422">Nakonec, pokud jste si zaregistrovali dat disku VHD, připojte je toohello nové úložiště Premium podporující virtuálního počítače Azure.</span><span class="sxs-lookup"><span data-stu-id="3d3b4-422">Lastly, if you have registered data disk VHDs, attach them toohello new Premium Storage capable Azure VM.</span></span>

<span data-ttu-id="3d3b4-423">Použijte následující prostředí PowerShell rutinu tooattach datového disku toohello nového virtuálního počítače a zadejte hello zásady ukládání do mezipaměti.</span><span class="sxs-lookup"><span data-stu-id="3d3b4-423">Use following PowerShell cmdlet tooattach data disk toohello new VM and specify hello caching policy.</span></span> <span data-ttu-id="3d3b4-424">V příkladu níže hello zásady ukládání do mezipaměti je nastaven příliš*jen pro čtení*.</span><span class="sxs-lookup"><span data-stu-id="3d3b4-424">In example below hello caching policy is set too*ReadOnly*.</span></span>

```powershell
$vm = Get-AzureVM -ServiceName $serviceName -Name $vmName

Add-AzureDataDisk -ImportFrom -DiskName "DataDisk" -LUN 0 –HostCaching ReadOnly –VM $vm

Update-AzureVM  -VM $vm
```

> [!NOTE]
> <span data-ttu-id="3d3b4-425">Mohou existovat další kroky potřebné toosupport aplikace, který je nesmí být zahrnuté v této příručce.</span><span class="sxs-lookup"><span data-stu-id="3d3b4-425">There may be additional steps necessary toosupport your application that is not be covered by this guide.</span></span>
>
>

### <a name="checking-and-plan-backup"></a><span data-ttu-id="3d3b4-426">Kontrola a plánování zálohování</span><span class="sxs-lookup"><span data-stu-id="3d3b4-426">Checking and plan backup</span></span>
<span data-ttu-id="3d3b4-427">Jednou hello nový virtuální počítač je zapnutý a běží, pomocí hello stejné id a heslo pro přístup k jako hello původní virtuální počítač a ověřte, zda vše funguje podle očekávání.</span><span class="sxs-lookup"><span data-stu-id="3d3b4-427">Once hello new VM is up and running, access it using hello same login id and password is as hello original VM, and verify that everything is working as expected.</span></span> <span data-ttu-id="3d3b4-428">Všechna nastavení hello, včetně hello prokládané svazky, budou existovat v hello nového virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="3d3b4-428">All hello settings, including hello striped volumes, would be present in hello new VM.</span></span>

<span data-ttu-id="3d3b4-429">posledním krokem Hello je tooplan plán zálohování a údržby pro nový virtuální počítač podle potřeb hello aplikace hello.</span><span class="sxs-lookup"><span data-stu-id="3d3b4-429">hello last step is tooplan backup and maintenance schedule for hello new VM based on hello application's needs.</span></span>

### <span data-ttu-id="3d3b4-430"><a name="a-sample-migration-script"></a>Ukázkový skript migrace</span><span class="sxs-lookup"><span data-stu-id="3d3b4-430"><a name="a-sample-migration-script"></a>A sample migration script</span></span>
<span data-ttu-id="3d3b4-431">Pokud máte více toomigrate virtuální počítače, bude užitečné automatizace prostřednictvím skriptů prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="3d3b4-431">If you have multiple VMs toomigrate, automation through PowerShell scripts will be helpful.</span></span> <span data-ttu-id="3d3b4-432">Následuje ukázkový skript, který zautomatizuje hello migrace virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="3d3b4-432">Following is a sample script that automates hello migration of a VM.</span></span> <span data-ttu-id="3d3b4-433">Poznámka: níže uvedený skript se pouze příklad a neexistují několik předpokladům o hello aktuální disky virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="3d3b4-433">Note that below script is only an example, and there are few assumptions made about hello current VM disks.</span></span> <span data-ttu-id="3d3b4-434">Může být nutné tooupdate hello skriptu toomatch s konkrétní scénář.</span><span class="sxs-lookup"><span data-stu-id="3d3b4-434">You may need tooupdate hello script toomatch with your specific scenario.</span></span>

<span data-ttu-id="3d3b4-435">předpoklady Hello jsou:</span><span class="sxs-lookup"><span data-stu-id="3d3b4-435">hello assumptions are:</span></span>

* <span data-ttu-id="3d3b4-436">Vytváříte klasické virtuální počítače Azure.</span><span class="sxs-lookup"><span data-stu-id="3d3b4-436">You are creating classic Azure VMs.</span></span>
* <span data-ttu-id="3d3b4-437">Vaše zdrojové disky operačního systému a datové disky zdroje jsou ve stejném účtu úložiště a kontejneru.</span><span class="sxs-lookup"><span data-stu-id="3d3b4-437">Your source OS Disks and source Data Disks are in same storage account and same container.</span></span> <span data-ttu-id="3d3b4-438">Pokud vaše disky operačního systému a datové disky nejsou v hello stejné umístění, můžete použít AzCopy nebo Azure PowerShell toocopy disků VHD přes účty úložiště a kontejnery.</span><span class="sxs-lookup"><span data-stu-id="3d3b4-438">If your OS Disks and Data Disks are not in hello same place, you can use AzCopy or Azure PowerShell toocopy VHDs over storage accounts and containers.</span></span> <span data-ttu-id="3d3b4-439">Najdete v předchozím kroku toohello: [kopie virtuálního pevného disku s AzCopy nebo prostředí PowerShell](#copy-vhd-with-azcopy-or-powershell).</span><span class="sxs-lookup"><span data-stu-id="3d3b4-439">Refer toohello previous step: [Copy VHD with AzCopy or PowerShell](#copy-vhd-with-azcopy-or-powershell).</span></span> <span data-ttu-id="3d3b4-440">Úpravy tento skript toomeet váš scénář je další možnost, ale doporučujeme použít AzCopy nebo prostředí PowerShell, protože je jednodušší a rychlejší.</span><span class="sxs-lookup"><span data-stu-id="3d3b4-440">Editing this script toomeet your scenario is another choice, but we recommend using AzCopy or PowerShell since it is easier and faster.</span></span>

<span data-ttu-id="3d3b4-441">skriptu pro automatizaci Hello najdete níže.</span><span class="sxs-lookup"><span data-stu-id="3d3b4-441">hello automation script is provided below.</span></span> <span data-ttu-id="3d3b4-442">Nahradí text s informacemi a aktualizujte hello skriptu toomatch s konkrétní scénář.</span><span class="sxs-lookup"><span data-stu-id="3d3b4-442">Replace text with your information and update hello script toomatch with your specific scenario.</span></span>

> [!NOTE]
> <span data-ttu-id="3d3b4-443">Pomocí existujícího skriptu, který hello nezachovává hello síťovou konfiguraci vašeho zdrojového virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="3d3b4-443">Using hello existing script does not preserve hello network configuration of your source VM.</span></span> <span data-ttu-id="3d3b4-444">Nastavení sítě hello toore-config budete potřebovat na migrovaných virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="3d3b4-444">You will need toore-config hello networking settings on your migrated VMs.</span></span>
>
>

```
    <#
    .Synopsis
    This script is provided as an EXAMPLE tooshow how toomigrate a VM from a standard storage account tooa premium storage account. You can customize it according tooyour specific requirements.

    .Description
    hello script will copy hello vhds (page blobs) of hello source VM toohello new storage account.
    And then it will create a new VM from these copied vhds based on hello inputs that you specified for hello new VM.
    You can modify hello script toosatisfy your specific requirement, but please be aware of hello items specified
    in hello Terms of Use section.

    .Terms of Use
    Copyright © 2015 Microsoft Corporation.  All rights reserved.

    THIS CODE AND ANY ASSOCIATED INFORMATION ARE PROVIDED "AS IS" WITHOUT WARRANTY OF ANY KIND,
    EITHER EXPRESSED OR IMPLIED, INCLUDING BUT NOT LIMITED toohello IMPLIED WARRANTIES OF MERCHANTABILITY
    AND/OR FITNESS FOR A PARTICULAR PURPOSE. hello ENTIRE RISK OF USE, INABILITY tooUSE, OR
    RESULTS FROM hello USE OF THIS CODE REMAINS WITH hello USER.

    .Example (Save this script as Migrate-AzureVM.ps1)

    .\Migrate-AzureVM.ps1 -SourceServiceName CurrentServiceName -SourceVMName CurrentVMName –DestStorageAccount newpremiumstorageaccount -DestServiceName NewServiceName -DestVMName NewDSVMName -DestVMSize "Standard_DS2" –Location "Southeast Asia"

    .Link
    toofind more information about how tooset up Azure PowerShell, refer toohello following links.
    http://azure.microsoft.com/documentation/articles/powershell-install-configure/
    http://azure.microsoft.com/documentation/articles/storage-powershell-guide-full/
    http://azure.microsoft.com/blog/2014/10/22/migrate-azure-virtual-machines-between-storage-accounts/

    #>

    Param(
    # hello cloud service name of hello VM.
    [Parameter(Mandatory = $true)]
    [string] $SourceServiceName,

    # hello VM name toocopy.
    [Parameter(Mandatory = $true)]
    [String] $SourceVMName,

    # hello destination storage account name.
    [Parameter(Mandatory = $true)]
    [String] $DestStorageAccount,

    # hello destination cloud service name
    [Parameter(Mandatory = $true)]
    [String] $DestServiceName,

    # hello destination vm name
    [Parameter(Mandatory = $true)]
    [String] $DestVMName,

    # hello destination vm size
    [Parameter(Mandatory = $true)]
    [String] $DestVMSize,

    # hello location of destination VM.
    [Parameter(Mandatory = $true)]
    [string] $Location,

    # whether or not toocopy hello os disk, hello default is only copy data disks
    [Parameter(Mandatory = $false)]
    [Bool] $DataDiskOnly = $true,

    # how frequently tooreport hello copy status in sceconds
    [Parameter(Mandatory = $false)]
    [Int32] $CopyStatusReportInterval = 15,

    # hello name suffix tooadd toonew created disks tooavoid conflict with source disk names
    [Parameter(Mandatory = $false)]
    [String]$DiskNameSuffix = "-prem"

    ) #end param

    #######################################################################
    #  Verify Azure PowerShell module and version
    #######################################################################

    #import hello Azure PowerShell module
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


    #Check hello Azure PowerShell module version
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
        Write-Host "[ERROR] - There is no valid Azure subscription found in PowerShell. Please refer toothis article http://azure.microsoft.com/documentation/articles/powershell-install-configure/ tooconnect an Azure subscription. Exiting." -ForegroundColor Red
        Exit
    }


    #######################################################################
    #  Check if hello VM is shut down
    #  Stopping hello VM is a required step so that hello file system is consistent when you do hello copy operation.
    #  Azure does not support live migration at this time..
    #######################################################################

    if (($sourceVM = Get-AzureVM –ServiceName $SourceServiceName –Name $SourceVMName) -eq $null)
    {
        Write-Host "[ERROR] - hello source VM doesn't exist in hello current subscription. Exiting." -ForegroundColor Red
        Exit
    }

    # check if VM is shut down
    if ( $sourceVM.Status -notmatch "Stopped" )
    {
        Write-Host "[Warning] - Stopping hello VM is a required step so that hello file system is consistent when you do hello copy operation. Azure does not support live migration at this time. If you'd like toocreate a VM from a generalized image, sys-prep hello Virtual Machine before stopping it." -ForegroundColor Yellow
        $ContinueAnswer = Read-Host "`n`tDo you wish toostop $SourceVMName now? Input 'N' if you want tooshut down hello VM manually and come back later.(Y/N)"
        If ($ContinueAnswer -ne "Y") { Write-Host "`n Exiting." -ForegroundColor Red;Exit }
        $sourceVM | Stop-AzureVM

        # wait until hello VM is shut down
        $VMStatus = (Get-AzureVM –ServiceName $SourceServiceName –Name $vmName).Status
        while ($VMStatus -notmatch "Stopped")
        {
            Write-Host "`n[Status] - Waiting VM $vmName tooshut down" -ForegroundColor Green
            Sleep -Seconds 5
            $VMStatus = (Get-AzureVM –ServiceName $SourceServiceName –Name $vmName).Status
        }
    }

    # exporting hello sourve vm tooa configuration file, you can restore hello original VM by importing this config file
    # see more information for Import-AzureVM
    $workingDir = (Get-Location).Path
    $vmConfigurationPath = $env:HOMEPATH + "\VM-" + $SourceVMName + ".xml"
    Write-Host "`n[WORKITEM] - Exporting VM configuration too$vmConfigurationPath" -ForegroundColor Yellow
    $exportRe = $sourceVM | Export-AzureVM -Path $vmConfigurationPath


    #######################################################################
    #  Copy hello vhds of hello source vm
    #  You can choose toocopy all disks including os and data disks by specifying the
    #  parameter -DataDiskOnly toobe $false. hello default is toocopy only data disk vhds
    #  and hello new VM will boot from hello original os disk.
    #######################################################################

    $sourceOSDisk = $sourceVM.VM.OSVirtualHardDisk
    $sourceDataDisks = $sourceVM.VM.DataVirtualHardDisks

    # Get source storage account information, not considering hello data disks and os disks are in different accounts
    $sourceStorageAccountName = $sourceOSDisk.MediaLink.Host -split "\." | select -First 1
    $sourceStorageKey = (Get-AzureStorageKey -StorageAccountName $sourceStorageAccountName).Primary
    $sourceContext = New-AzureStorageContext –StorageAccountName $sourceStorageAccountName -StorageAccountKey $sourceStorageKey

    # Create destination context
    $destStorageKey = (Get-AzureStorageKey -StorageAccountName $DestStorageAccount).Primary
    $destContext = New-AzureStorageContext –StorageAccountName $DestStorageAccount -StorageAccountKey $destStorageKey

    # Create a container of vhds if it doesn't exist
    if ((Get-AzureStorageContainer -Context $destContext -Name vhds -ErrorAction SilentlyContinue) -eq $null)
    {
        Write-Host "`n[WORKITEM] - Creating a container vhds in hello destination storage account." -ForegroundColor Yellow
        New-AzureStorageContainer -Context $destContext -Name vhds
    }


    $allDisksToCopy = $sourceDataDisks
    # check if need toocopy os disk
    $sourceOSVHD = $sourceOSDisk.MediaLink.Segments[2]
    if ($DataDiskOnly)
    {
        # copy data disks only, this option requires deleting hello source VM so that dest VM can boot
        # from hello same vhd blob.
        $ContinueAnswer = Read-Host "`n`t[Warning] You chose toocopy data disks only. Moving VM requires removing hello original VM (hello disks and backing vhd files will NOT be deleted) so that hello new VM can boot from hello same vhd. This is an irreversible action. Do you wish tooproceed right now? (Y/N)"
        If ($ContinueAnswer -ne "Y") { Write-Host "`n Exiting." -ForegroundColor Red;Exit }
        $destOSVHD = Get-AzureStorageBlob -Blob $sourceOSVHD -Container vhds -Context $sourceContext
        Write-Host "`n[WORKITEM] - Removing hello original VM (hello vhd files are NOT deleted)." -ForegroundColor Yellow
        Remove-AzureVM -Name $SourceVMName -ServiceName $SourceServiceName

        Write-Host "`n[WORKITEM] - Waiting utill hello OS disk is released by source VM. This may take up tooseveral minutes."
        $diskAttachedTo = (Get-AzureDisk -DiskName $sourceOSDisk.DiskName).AttachedTo
        while ($diskAttachedTo -ne $null)
        {
            Start-Sleep -Seconds 10
            $diskAttachedTo = (Get-AzureDisk -DiskName $sourceOSDisk.DiskName).AttachedTo
        }

    }
    else
    {
        # copy hello os disk vhd
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
        # update hello media link toopoint toohello target blob link
        $disk.MediaLink = $targetBlob.ICloudBlob.Uri.AbsoluteUri
    }

    # Wait until all vhd files are copied.
    $diskComplete = @()
    do
    {
        Write-Host "`n[WORKITEM] - Waiting for all disk copy toocomplete. Checking status every $CopyStatusReportInterval seconds." -ForegroundColor Yellow
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
    #  hello new VM can be created from hello copied disks or hello original os disk.
    #  You can ddd your own logic here toosatisfy your specific requirements of hello vm.
    #######################################################################

    # Create a VM from hello existing os disk
    if ($DataDiskOnly)
    {
        $vm = New-AzureVMConfig -Name $DestVMName -InstanceSize $DestVMSize -DiskName $sourceOSDisk.DiskName
    }
    else
    {
        $newOSDisk = Add-AzureDisk -OS $sourceOSDisk.OS -DiskName ($sourceOSDisk.DiskName + $DiskNameSuffix) -MediaLocation $destOSVHD.ICloudBlob.Uri.AbsoluteUri
        $vm = New-AzureVMConfig -Name $DestVMName -InstanceSize $DestVMSize -DiskName $newOSDisk.DiskName
    }
    # Attached hello copied data disks toohello new VM
    foreach ($dataDisk in $sourceDataDisks)
    {
        # add -DiskLabel $dataDisk.DiskLabel if there are labels for disks of hello source vm
        $diskLabel = "drive" + $dataDisk.Lun
        $vm | Add-AzureDataDisk -ImportFrom -DiskLabel $diskLabel -LUN $dataDisk.Lun -MediaLocation $dataDisk.MediaLink
    }

    # Edit this if you want tooadd more custimization toohello new VM
    # $vm | Add-AzureEndpoint -Protocol tcp -LocalPort 443 -PublicPort 443 -Name 'HTTPs'
    # $vm | Set-AzureSubnet "PubSubnet","PrivSubnet"

    New-AzureVM -ServiceName $DestServiceName -VMs $vm -Location $Location
```

#### <span data-ttu-id="3d3b4-445"><a name="optimization"></a>Optimalizace</span><span class="sxs-lookup"><span data-stu-id="3d3b4-445"><a name="optimization"></a>Optimization</span></span>
<span data-ttu-id="3d3b4-446">Aktuální konfigurace virtuálních počítačů lze přizpůsobit konkrétně toowork dobře u standardní disky.</span><span class="sxs-lookup"><span data-stu-id="3d3b4-446">Your current VM configuration may be customized specifically toowork well with Standard disks.</span></span> <span data-ttu-id="3d3b4-447">Například tooincrease pomocí mnoho disků v prokládaný svazek výkonu hello.</span><span class="sxs-lookup"><span data-stu-id="3d3b4-447">For instance, tooincrease hello performance by using many disks in a striped volume.</span></span> <span data-ttu-id="3d3b4-448">Místo použití 4 disky samostatně na Storage úrovně Premium, například může být schopný toooptimize hello náklady tak, že jeden disk.</span><span class="sxs-lookup"><span data-stu-id="3d3b4-448">For example, instead of using 4 disks separately on Premium Storage, you may be able toooptimize hello cost by having a single disk.</span></span> <span data-ttu-id="3d3b4-449">Optimalizace jako tento nutné toobe zpracovává případ od případu a vyžadují vlastní kroky po migraci hello.</span><span class="sxs-lookup"><span data-stu-id="3d3b4-449">Optimizations like this need toobe handled on a case by case basis and require custom steps after hello migration.</span></span> <span data-ttu-id="3d3b4-450">Všimněte si také, že tento proces nemusí fungovat i pro databáze a aplikace, které závisí na rozložení disku hello definované v instalačním programu hello.</span><span class="sxs-lookup"><span data-stu-id="3d3b4-450">Also, note that this process may not well work for databases and applications that depend on hello disk layout defined in hello setup.</span></span>

##### <a name="preparation"></a><span data-ttu-id="3d3b4-451">Příprava</span><span class="sxs-lookup"><span data-stu-id="3d3b4-451">Preparation</span></span>
1. <span data-ttu-id="3d3b4-452">Dokončení hello jednoduché migrace, jak je popsáno v hello dříve části.</span><span class="sxs-lookup"><span data-stu-id="3d3b4-452">Complete hello Simple Migration as described in hello earlier section.</span></span> <span data-ttu-id="3d3b4-453">Optimalizace se provede na hello nový virtuální počítač po migraci hello.</span><span class="sxs-lookup"><span data-stu-id="3d3b4-453">Optimizations will be performed on hello new VM after hello migration.</span></span>
2. <span data-ttu-id="3d3b4-454">Zadejte nové velikosti disku hello potřebného pro konfiguraci hello optimalizované.</span><span class="sxs-lookup"><span data-stu-id="3d3b4-454">Define hello new disk sizes needed for hello optimized configuration.</span></span>
3. <span data-ttu-id="3d3b4-455">Určuje mapování hello aktuální disky nebo svazky toohello nové specifikace disku.</span><span class="sxs-lookup"><span data-stu-id="3d3b4-455">Determine mapping of hello current disks/volumes toohello new disk specifications.</span></span>

##### <a name="execution-steps"></a><span data-ttu-id="3d3b4-456">Provedení kroků</span><span class="sxs-lookup"><span data-stu-id="3d3b4-456">Execution steps</span></span>
1. <span data-ttu-id="3d3b4-457">Vytvořte nové disky hello se správnými velikostmi hello na hello virtuálního počítače služby Storage úrovně Premium.</span><span class="sxs-lookup"><span data-stu-id="3d3b4-457">Create hello new disks with hello right sizes on hello Premium Storage VM.</span></span>
2. <span data-ttu-id="3d3b4-458">Přihlášení toohello virtuálních počítačů a zkopírujte hello data z hello aktuální svazek toohello nový disk, který mapuje toothat svazku.</span><span class="sxs-lookup"><span data-stu-id="3d3b4-458">Login toohello VM and copy hello data from hello current volume toohello new disk that maps toothat volume.</span></span> <span data-ttu-id="3d3b4-459">To lze proveďte pro všechny svazky aktuální hello, které je třeba toomap tooa nový disk.</span><span class="sxs-lookup"><span data-stu-id="3d3b4-459">Do this for all hello current volumes that need toomap tooa new disk.</span></span>
3. <span data-ttu-id="3d3b4-460">V dalším kroku změnit hello aplikace nastavení tooswitch toohello nové disky a odpojit hello starých svazků.</span><span class="sxs-lookup"><span data-stu-id="3d3b4-460">Next, change hello application settings tooswitch toohello new disks, and detach hello old volumes.</span></span>

<span data-ttu-id="3d3b4-461">Ladění aplikace hello pro lepší výkon disku, naleznete příliš[optimalizace výkonu aplikace](storage-premium-storage-performance.md#optimizing-application-performance).</span><span class="sxs-lookup"><span data-stu-id="3d3b4-461">For tuning hello application for better disk performance, please refer too[Optimizing Application Performance](storage-premium-storage-performance.md#optimizing-application-performance).</span></span>

### <a name="application-migrations"></a><span data-ttu-id="3d3b4-462">Migrace aplikací</span><span class="sxs-lookup"><span data-stu-id="3d3b4-462">Application migrations</span></span>
<span data-ttu-id="3d3b4-463">Databáze a jiné komplexní aplikace může vyžadovat zvláštní postup podle definice poskytovatele aplikace hello hello migrace.</span><span class="sxs-lookup"><span data-stu-id="3d3b4-463">Databases and other complex applications may require special steps as defined by hello application provider for hello migration.</span></span> <span data-ttu-id="3d3b4-464">Naleznete v dokumentaci toorespective aplikace.</span><span class="sxs-lookup"><span data-stu-id="3d3b4-464">Please refer toorespective application documentation.</span></span> <span data-ttu-id="3d3b4-465">Například</span><span class="sxs-lookup"><span data-stu-id="3d3b4-465">E.g.</span></span> <span data-ttu-id="3d3b4-466">Obvykle můžete migrovat databází pomocí zálohování a obnovení.</span><span class="sxs-lookup"><span data-stu-id="3d3b4-466">typically databases can be migrated through backup and restore.</span></span>

## <a name="next-steps"></a><span data-ttu-id="3d3b4-467">Další kroky</span><span class="sxs-lookup"><span data-stu-id="3d3b4-467">Next steps</span></span>
<span data-ttu-id="3d3b4-468">V tématu hello následující prostředků u konkrétních scénářů pro migraci virtuálních počítačů:</span><span class="sxs-lookup"><span data-stu-id="3d3b4-468">See hello following resources for specific scenarios for migrating virtual machines:</span></span>

* [<span data-ttu-id="3d3b4-469">Migrovat virtuální počítače, které jsou mezi účty úložiště Azure</span><span class="sxs-lookup"><span data-stu-id="3d3b4-469">Migrate Azure Virtual Machines between Storage Accounts</span></span>](https://azure.microsoft.com/blog/2014/10/22/migrate-azure-virtual-machines-between-storage-accounts/)
* [<span data-ttu-id="3d3b4-470">Vytvoření a nahrání virtuálního pevného disku serveru Windows tooAzure.</span><span class="sxs-lookup"><span data-stu-id="3d3b4-470">Create and upload a Windows Server VHD tooAzure.</span></span>](../../virtual-machines/windows/classic/createupload-vhd.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json)
* [<span data-ttu-id="3d3b4-471">Vytvoření a nahrání virtuálního pevného disku tohoto hello obsahuje operační systém Linux</span><span class="sxs-lookup"><span data-stu-id="3d3b4-471">Creating and Uploading a Virtual Hard Disk that Contains hello Linux Operating System</span></span>](../../virtual-machines/linux/classic/create-upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json)
* [<span data-ttu-id="3d3b4-472">Migrace virtuálních počítačů z Amazon AWS tooMicrosoft Azure</span><span class="sxs-lookup"><span data-stu-id="3d3b4-472">Migrating Virtual Machines from Amazon AWS tooMicrosoft Azure</span></span>](http://channel9.msdn.com/Series/Migrating-Virtual-Machines-from-Amazon-AWS-to-Microsoft-Azure)

<span data-ttu-id="3d3b4-473">Viz také hello následující prostředky toolearn Další informace o Azure Storage a virtuálních počítačích Azure:</span><span class="sxs-lookup"><span data-stu-id="3d3b4-473">Also, see hello following resources toolearn more about Azure Storage and Azure Virtual Machines:</span></span>

* [<span data-ttu-id="3d3b4-474">Azure Storage</span><span class="sxs-lookup"><span data-stu-id="3d3b4-474">Azure Storage</span></span>](https://azure.microsoft.com/documentation/services/storage/)
* [<span data-ttu-id="3d3b4-475">Virtuální počítače Azure</span><span class="sxs-lookup"><span data-stu-id="3d3b4-475">Azure Virtual Machines</span></span>](https://azure.microsoft.com/documentation/services/virtual-machines/)
* [<span data-ttu-id="3d3b4-476">Storage úrovně Premium: Vysoce výkonné úložiště pro úlohy virtuálních počítačů Azure</span><span class="sxs-lookup"><span data-stu-id="3d3b4-476">Premium Storage: High-Performance Storage for Azure Virtual Machine Workloads</span></span>](storage-premium-storage.md)

[1]:./media/storage-migration-to-premium-storage/migration-to-premium-storage-1.png
[2]:./media/storage-migration-to-premium-storage/migration-to-premium-storage-1.png
[3]:./media/storage-migration-to-premium-storage/migration-to-premium-storage-3.png
[4]: http://technet.microsoft.com/library/hh831739.aspx
