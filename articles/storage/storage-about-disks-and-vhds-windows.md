---
title: "O disky a virtuální pevné disky pro virtuální počítače Windows Microsoft Azure | Microsoft Docs"
description: "Další informace o základní informace o disky a virtuální počítače virtuální pevné disky pro Windows v Azure."
services: storage
documentationcenter: 
author: robinsh
manager: timlt
editor: tysonn
ms.assetid: 0142c64d-5e8c-4d62-aa6f-06d6261f485a
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/15/2017
ms.author: robinsh
ms.openlocfilehash: 34a4d8fa176484fbadb1b385d794cada5be607c8
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="about-disks-and-vhds-for-azure-windows-vms"></a><span data-ttu-id="2e47f-103">O disky a virtuální pevné disky pro virtuální počítače Windows Azure</span><span class="sxs-lookup"><span data-stu-id="2e47f-103">About disks and VHDs for Azure Windows VMs</span></span>
<span data-ttu-id="2e47f-104">Stejně jako všechny ostatní počítače virtuálních počítačů v Azure používat disky jako místo pro uložení operačního systému, aplikace a data.</span><span class="sxs-lookup"><span data-stu-id="2e47f-104">Just like any other computer, virtual machines in Azure use disks as a place to store an operating system, applications, and data.</span></span> <span data-ttu-id="2e47f-105">Všechny virtuální počítače Azure mít aspoň dva disky – disk operačního systému Windows a dočasný disk.</span><span class="sxs-lookup"><span data-stu-id="2e47f-105">All Azure virtual machines have at least two disks – a Windows operating system disk and a temporary disk.</span></span> <span data-ttu-id="2e47f-106">Vytváření disku operačního systému z bitové kopie a disku operačního systému a image jsou virtuální pevné disky (VHD) uložené v účtu úložiště Azure.</span><span class="sxs-lookup"><span data-stu-id="2e47f-106">The operating system disk is created from an image, and both the operating system disk and the image are virtual hard disks (VHDs) stored in an Azure storage account.</span></span> <span data-ttu-id="2e47f-107">Virtuální počítače také může mít jeden nebo více datových disků, které jsou také uloženy jako virtuální pevné disky.</span><span class="sxs-lookup"><span data-stu-id="2e47f-107">Virtual machines also can have one or more data disks, that are also stored as VHDs.</span></span> 

<span data-ttu-id="2e47f-108">V tomto článku jsme se v souvislosti se jiný používá pro disky a potom popisují různé typy disků můžete vytvořit a použít.</span><span class="sxs-lookup"><span data-stu-id="2e47f-108">In this article, we will talk about the different uses for the disks, and then discuss the different types of disks you can create and use.</span></span> <span data-ttu-id="2e47f-109">Tento článek je také k dispozici pro [virtuální počítače s Linuxem](storage-about-disks-and-vhds-linux.md).</span><span class="sxs-lookup"><span data-stu-id="2e47f-109">This article is also available for [Linux virtual machines](storage-about-disks-and-vhds-linux.md).</span></span>

[!INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-both-include.md)]

## <a name="disks-used-by-vms"></a><span data-ttu-id="2e47f-110">Disky, které jsou používány virtuálními počítači</span><span class="sxs-lookup"><span data-stu-id="2e47f-110">Disks used by VMs</span></span>

<span data-ttu-id="2e47f-111">Podívejme se na tom, jak jsou disky používány virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="2e47f-111">Let's take a look at how the disks are used by the VMs.</span></span>

### <a name="operating-system-disk"></a><span data-ttu-id="2e47f-112">Disk operačním systému</span><span class="sxs-lookup"><span data-stu-id="2e47f-112">Operating system disk</span></span>
<span data-ttu-id="2e47f-113">Každý virtuální počítač má jeden disk připojené operačního systému.</span><span class="sxs-lookup"><span data-stu-id="2e47f-113">Every virtual machine has one attached operating system disk.</span></span> <span data-ttu-id="2e47f-114">Má registrován jako jednotky SATA a označené jako jednotky C: ve výchozím nastavení.</span><span class="sxs-lookup"><span data-stu-id="2e47f-114">It's registered as a SATA drive and labeled as the C: drive by default.</span></span> <span data-ttu-id="2e47f-115">Tento disk má maximální kapacita 2 048 gigabajtů (GB).</span><span class="sxs-lookup"><span data-stu-id="2e47f-115">This disk has a maximum capacity of 2048 gigabytes (GB).</span></span> 

### <a name="temporary-disk"></a><span data-ttu-id="2e47f-116">Dočasné disku</span><span class="sxs-lookup"><span data-stu-id="2e47f-116">Temporary disk</span></span>
<span data-ttu-id="2e47f-117">Každý virtuální počítač obsahuje dočasné disk.</span><span class="sxs-lookup"><span data-stu-id="2e47f-117">Each VM contains a temporary disk.</span></span> <span data-ttu-id="2e47f-118">Dočasné disku poskytuje krátkodobé úložiště pro aplikace a procesy a slouží k uložení pouze data, jako jsou soubory stránky nebo odkládacího souboru.</span><span class="sxs-lookup"><span data-stu-id="2e47f-118">The temporary disk provides short-term storage for applications and processes and is intended to only store data such as page or swap files.</span></span> <span data-ttu-id="2e47f-119">Data na dočasné disku mohou být ztraceny při [údržby](../virtual-machines/windows/manage-availability.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json#understand-vm-reboots---maintenance-vs-downtime) nebo když jste [znovu nasadit virtuální počítač](../virtual-machines/windows/redeploy-to-new-node.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="2e47f-119">Data on the temporary disk may be lost during a [maintenance event](../virtual-machines/windows/manage-availability.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json#understand-vm-reboots---maintenance-vs-downtime) or when you [redeploy a VM](../virtual-machines/windows/redeploy-to-new-node.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span> <span data-ttu-id="2e47f-120">Během restartu standardní virtuální počítač by měl uchovat data na dočasné jednotce.</span><span class="sxs-lookup"><span data-stu-id="2e47f-120">During a standard reboot of the VM, the data on the temporary drive should persist.</span></span>

<span data-ttu-id="2e47f-121">Dočasné disku je označený jako jednotka D: výchozí a používá se pro ukládání pagefile.sys.</span><span class="sxs-lookup"><span data-stu-id="2e47f-121">The temporary disk is labeled as the D: drive by default and it used for storing pagefile.sys.</span></span> <span data-ttu-id="2e47f-122">Přemapování tento disk k jiné písmeno jednotky, najdete v části [změnit písmeno jednotky dočasné disk systému Windows](../virtual-machines/windows/change-drive-letter.md).</span><span class="sxs-lookup"><span data-stu-id="2e47f-122">To remap this disk to a different drive letter, see [Change the drive letter of the Windows temporary disk](../virtual-machines/windows/change-drive-letter.md).</span></span> <span data-ttu-id="2e47f-123">Velikost dočasné disku se liší, na základě velikosti virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="2e47f-123">The size of the temporary disk varies, based on the size of the virtual machine.</span></span> <span data-ttu-id="2e47f-124">Další informace najdete v tématu [virtuální počítače s velikostí pro Windows](../virtual-machines/windows/sizes.md).</span><span class="sxs-lookup"><span data-stu-id="2e47f-124">For more information, see [Sizes for Windows virtual machines](../virtual-machines/windows/sizes.md).</span></span>

<span data-ttu-id="2e47f-125">Další informace o tom, jak Azure používá dočasným diskovým najdete v tématu [pochopení dočasné jednotce ve virtuálních počítačích Microsoft Azure](https://blogs.msdn.microsoft.com/mast/2013/12/06/understanding-the-temporary-drive-on-windows-azure-virtual-machines/)</span><span class="sxs-lookup"><span data-stu-id="2e47f-125">For more information on how Azure uses the temporary disk, see [Understanding the temporary drive on Microsoft Azure Virtual Machines](https://blogs.msdn.microsoft.com/mast/2013/12/06/understanding-the-temporary-drive-on-windows-azure-virtual-machines/)</span></span>


### <a name="data-disk"></a><span data-ttu-id="2e47f-126">Datový disk</span><span class="sxs-lookup"><span data-stu-id="2e47f-126">Data disk</span></span>
<span data-ttu-id="2e47f-127">Datový disk je virtuální pevný disk, který je připojen k virtuálnímu počítači na ukládání dat aplikací, nebo data, která je třeba zachovat.</span><span class="sxs-lookup"><span data-stu-id="2e47f-127">A data disk is a VHD that's attached to a virtual machine to store application data, or other data you need to keep.</span></span> <span data-ttu-id="2e47f-128">Datové disky jsou zaregistrované jako disky SCSI a jsou označeny písmenem, který zvolíte.</span><span class="sxs-lookup"><span data-stu-id="2e47f-128">Data disks are registered as SCSI drives and are labeled with a letter that you choose.</span></span> <span data-ttu-id="2e47f-129">Každý datový disk má maximální kapacitu 4095 GB.</span><span class="sxs-lookup"><span data-stu-id="2e47f-129">Each data disk has a maximum capacity of 4095 GB.</span></span> <span data-ttu-id="2e47f-130">Velikost virtuálního počítače určuje, kolik datových disků můžete připojit k jeho a typ úložiště můžete použít k hostování disky.</span><span class="sxs-lookup"><span data-stu-id="2e47f-130">The size of the virtual machine determines how many data disks you can attach to it and the type of storage you can use to host the disks.</span></span>

> [!NOTE]
> <span data-ttu-id="2e47f-131">Další informace o virtuální počítače kapacity najdete v tématu [virtuální počítače s velikostí pro Windows](../virtual-machines/windows/sizes.md).</span><span class="sxs-lookup"><span data-stu-id="2e47f-131">For more information about virtual machines capacities, see [Sizes for Windows virtual machines](../virtual-machines/windows/sizes.md).</span></span>
> 

<span data-ttu-id="2e47f-132">Azure vytvoří disk operačního systému, když vytvoříte virtuální počítač z bitové kopie.</span><span class="sxs-lookup"><span data-stu-id="2e47f-132">Azure creates an operating system disk when you create a virtual machine from an image.</span></span> <span data-ttu-id="2e47f-133">Pokud používáte image, která obsahuje datové disky, Azure vytvoří datových disků také při vytváření virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="2e47f-133">If you use an image that includes data disks, Azure also creates the data disks when it creates the virtual machine.</span></span> <span data-ttu-id="2e47f-134">V opačném datových disků přidat po vytvoření virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="2e47f-134">Otherwise, you add data disks after you create the virtual machine.</span></span>

<span data-ttu-id="2e47f-135">Můžete přidat datových disků pro virtuální počítač v každém okamžiku nástrojem **připojení** disk k virtuálnímu počítači.</span><span class="sxs-lookup"><span data-stu-id="2e47f-135">You can add data disks to a virtual machine at any time, by **attaching** the disk to the virtual machine.</span></span> <span data-ttu-id="2e47f-136">Můžete vytvořit virtuální pevný disk, který jste nahráli nebo můžete zkopírovat do svého účtu úložiště nebo ten, který pro vás vytvoří Azure.</span><span class="sxs-lookup"><span data-stu-id="2e47f-136">You can use a VHD that you've uploaded or copied to your storage account, or one that Azure creates for you.</span></span> <span data-ttu-id="2e47f-137">Připojením datového disku přiřadí soubor virtuálního pevného disku s virtuálním Počítačem umístěním zapůjčení na virtuální pevný disk, nelze jej odstranit z úložiště při je stále připojen.</span><span class="sxs-lookup"><span data-stu-id="2e47f-137">Attaching a data disk associates the VHD file with the VM by placing a 'lease' on the VHD so it can't be deleted from storage while it's still attached.</span></span>


[!INCLUDE [storage-about-vhds-and-disks-windows-and-linux](../../includes/storage-about-vhds-and-disks-windows-and-linux.md)]

## <a name="one-last-recommendation-use-trim-with-unmanaged-standard-disks"></a><span data-ttu-id="2e47f-138">Jedním z poslední doporučení: použití TRIM s nespravované standardní disky</span><span class="sxs-lookup"><span data-stu-id="2e47f-138">One last recommendation: Use TRIM with unmanaged standard disks</span></span> 

<span data-ttu-id="2e47f-139">Pokud používáte nespravované standardní disky (HDD), měli byste povolit uvolnění dočasné paměti.</span><span class="sxs-lookup"><span data-stu-id="2e47f-139">If you use unmanaged standard disks (HDD), you should enable TRIM.</span></span> <span data-ttu-id="2e47f-140">TRIM zahodí nepoužívané bloky na disku, můžete se účtují pouze pro úložiště, které skutečně používáte.</span><span class="sxs-lookup"><span data-stu-id="2e47f-140">TRIM discards unused blocks on the disk so you are only billed for storage that you are actually using.</span></span> <span data-ttu-id="2e47f-141">To můžete uložit na náklady, pokud chcete vytvořit velkých souborů a pak odstraňte je.</span><span class="sxs-lookup"><span data-stu-id="2e47f-141">This can save on costs if you create large files and then delete them.</span></span> 

<span data-ttu-id="2e47f-142">Můžete spustit tento příkaz a zkontrolujte nastavení uvolnění dočasné paměti.</span><span class="sxs-lookup"><span data-stu-id="2e47f-142">You can run this command to check the TRIM setting.</span></span> <span data-ttu-id="2e47f-143">Otevřete příkazový řádek na virtuální počítač s Windows a zadejte:</span><span class="sxs-lookup"><span data-stu-id="2e47f-143">Open a command prompt on your Windows VM and type:</span></span>


```
fsutil behavior query DisableDeleteNotify
```

<span data-ttu-id="2e47f-144">Pokud příkaz vrátí hodnotu 0, TRIM správně povolené.</span><span class="sxs-lookup"><span data-stu-id="2e47f-144">If the command returns 0, TRIM is enabled correctly.</span></span> <span data-ttu-id="2e47f-145">Pokud vrátí hodnotu 1, spusťte následující příkaz k povolení uvolnění dočasné paměti:</span><span class="sxs-lookup"><span data-stu-id="2e47f-145">If it returns 1, run the following command to enable TRIM:</span></span>

```
fsutil behavior set DisableDeleteNotify 0
```

> [!NOTE]
> <span data-ttu-id="2e47f-146">Poznámka: Podpora uvolnění dočasné paměti spustí s Windows serverem 2012 nebo Windows 8 a vyšší, najdete v tématu [nové rozhraní API umožňuje aplikacím odeslat "TRIM a zrušit mapování" pomocné parametry úložná média,](https://msdn.microsoft.com/windows/compatibility/new-api-allows-apps-to-send-trim-and-unmap-hints).</span><span class="sxs-lookup"><span data-stu-id="2e47f-146">Note: Trim support starts with Windows Server 2012 / Windows 8 and above, see see [New API allows apps to send "TRIM and Unmap" hints to storage media](https://msdn.microsoft.com/windows/compatibility/new-api-allows-apps-to-send-trim-and-unmap-hints).</span></span>
> 

<!-- Might want to match next-steps from overview of managed disks -->
## <a name="next-steps"></a><span data-ttu-id="2e47f-147">Další kroky</span><span class="sxs-lookup"><span data-stu-id="2e47f-147">Next steps</span></span>
* <span data-ttu-id="2e47f-148">[Připojit disk](../virtual-machines/windows/attach-managed-disk-portal.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) přidat další úložiště pro virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="2e47f-148">[Attach a disk](../virtual-machines/windows/attach-managed-disk-portal.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) to add additional storage for your VM.</span></span>
* <span data-ttu-id="2e47f-149">[Změňte písmeno jednotky dočasné disk systému Windows](../virtual-machines/windows/change-drive-letter.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json) tak vaše aplikace můžete používat jednotku D: pro data.</span><span class="sxs-lookup"><span data-stu-id="2e47f-149">[Change the drive letter of the Windows temporary disk](../virtual-machines/windows/change-drive-letter.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json) so your application can use the D: drive for data.</span></span>

