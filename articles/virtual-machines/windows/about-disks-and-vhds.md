---
title: "aaaAbout disky a virtuální pevné disky pro virtuální počítače Windows Microsoft Azure | Microsoft Docs"
description: "Další informace o hello základy disky a virtuální počítače virtuální pevné disky pro Windows v Azure."
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
ms.openlocfilehash: 1b0d6bf05237bb3d1497b2c60f79a0159b730108
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="about-disks-and-vhds-for-azure-windows-vms"></a><span data-ttu-id="7f219-103">O disky a virtuální pevné disky pro virtuální počítače Windows Azure</span><span class="sxs-lookup"><span data-stu-id="7f219-103">About disks and VHDs for Azure Windows VMs</span></span>
<span data-ttu-id="7f219-104">Stejně jako všechny ostatní počítače virtuální počítače v Azure používat disky jako místní toostore operačního systému, aplikace a data.</span><span class="sxs-lookup"><span data-stu-id="7f219-104">Just like any other computer, virtual machines in Azure use disks as a place toostore an operating system, applications, and data.</span></span> <span data-ttu-id="7f219-105">Všechny virtuální počítače Azure mít aspoň dva disky – disk operačního systému Windows a dočasný disk.</span><span class="sxs-lookup"><span data-stu-id="7f219-105">All Azure virtual machines have at least two disks – a Windows operating system disk and a temporary disk.</span></span> <span data-ttu-id="7f219-106">vytváření Hello disku operačního systému z bitové kopie a disku operačního systému hello i hello image jsou virtuální pevné disky (VHD) uložené v účtu úložiště Azure.</span><span class="sxs-lookup"><span data-stu-id="7f219-106">hello operating system disk is created from an image, and both hello operating system disk and hello image are virtual hard disks (VHDs) stored in an Azure storage account.</span></span> <span data-ttu-id="7f219-107">Virtuální počítače také může mít jeden nebo více datových disků, které jsou také uloženy jako virtuální pevné disky.</span><span class="sxs-lookup"><span data-stu-id="7f219-107">Virtual machines also can have one or more data disks, that are also stored as VHDs.</span></span> 

<span data-ttu-id="7f219-108">V tomto článku jsme bude v souvislosti se používá jinou hello hello disků a potom popisují hello různým typům disků můžete vytvořit a použít.</span><span class="sxs-lookup"><span data-stu-id="7f219-108">In this article, we will talk about hello different uses for hello disks, and then discuss hello different types of disks you can create and use.</span></span> <span data-ttu-id="7f219-109">Tento článek je také k dispozici pro [virtuální počítače s Linuxem](about-disks-and-vhds.md).</span><span class="sxs-lookup"><span data-stu-id="7f219-109">This article is also available for [Linux virtual machines](about-disks-and-vhds.md).</span></span>

[!INCLUDE [learn-about-deployment-models](../../../includes/learn-about-deployment-models-both-include.md)]

## <a name="disks-used-by-vms"></a><span data-ttu-id="7f219-110">Disky, které jsou používány virtuálními počítači</span><span class="sxs-lookup"><span data-stu-id="7f219-110">Disks used by VMs</span></span>

<span data-ttu-id="7f219-111">Podívejme se na použití hello disky podle hello virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="7f219-111">Let's take a look at how hello disks are used by hello VMs.</span></span>

### <a name="operating-system-disk"></a><span data-ttu-id="7f219-112">Disk operačním systému</span><span class="sxs-lookup"><span data-stu-id="7f219-112">Operating system disk</span></span>
<span data-ttu-id="7f219-113">Každý virtuální počítač má jeden disk připojené operačního systému.</span><span class="sxs-lookup"><span data-stu-id="7f219-113">Every virtual machine has one attached operating system disk.</span></span> <span data-ttu-id="7f219-114">Má registrován jako jednotky SATA a označené jako jednotky C: hello ve výchozím nastavení.</span><span class="sxs-lookup"><span data-stu-id="7f219-114">It's registered as a SATA drive and labeled as hello C: drive by default.</span></span> <span data-ttu-id="7f219-115">Tento disk má maximální kapacita 2 048 gigabajtů (GB).</span><span class="sxs-lookup"><span data-stu-id="7f219-115">This disk has a maximum capacity of 2048 gigabytes (GB).</span></span> 

### <a name="temporary-disk"></a><span data-ttu-id="7f219-116">Dočasné disku</span><span class="sxs-lookup"><span data-stu-id="7f219-116">Temporary disk</span></span>
<span data-ttu-id="7f219-117">Každý virtuální počítač obsahuje dočasné disk.</span><span class="sxs-lookup"><span data-stu-id="7f219-117">Each VM contains a temporary disk.</span></span> <span data-ttu-id="7f219-118">Hello dočasným diskovým poskytuje krátkodobé úložiště pro aplikace a procesy a je určený tooonly úložiště dat jako jsou soubory stránky nebo odkládacího souboru.</span><span class="sxs-lookup"><span data-stu-id="7f219-118">hello temporary disk provides short-term storage for applications and processes and is intended tooonly store data such as page or swap files.</span></span> <span data-ttu-id="7f219-119">Data na dočasné disku hello mohou být ztraceny při [údržby](manage-availability.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json#understand-vm-reboots---maintenance-vs-downtime) nebo když jste [znovu nasadit virtuální počítač](redeploy-to-new-node.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="7f219-119">Data on hello temporary disk may be lost during a [maintenance event](manage-availability.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json#understand-vm-reboots---maintenance-vs-downtime) or when you [redeploy a VM](redeploy-to-new-node.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span> <span data-ttu-id="7f219-120">Během restartu standardní hello virtuální počítač by měl zachovat hello data na dočasné jednotce hello.</span><span class="sxs-lookup"><span data-stu-id="7f219-120">During a standard reboot of hello VM, hello data on hello temporary drive should persist.</span></span>

<span data-ttu-id="7f219-121">dočasným diskovým Hello je označený jako hello D: jednotky ve výchozím nastavení a používá se pro ukládání pagefile.sys.</span><span class="sxs-lookup"><span data-stu-id="7f219-121">hello temporary disk is labeled as hello D: drive by default and it used for storing pagefile.sys.</span></span> <span data-ttu-id="7f219-122">tooremap tento disk tooa jiné písmeno jednotky, najdete v části [změnit písmeno jednotky hello dočasné disku Windows hello](change-drive-letter.md).</span><span class="sxs-lookup"><span data-stu-id="7f219-122">tooremap this disk tooa different drive letter, see [Change hello drive letter of hello Windows temporary disk](change-drive-letter.md).</span></span> <span data-ttu-id="7f219-123">velikost Hello hello dočasné disku se liší, na základě velikosti hello hello virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="7f219-123">hello size of hello temporary disk varies, based on hello size of hello virtual machine.</span></span> <span data-ttu-id="7f219-124">Další informace najdete v tématu [virtuální počítače s velikostí pro Windows](sizes.md).</span><span class="sxs-lookup"><span data-stu-id="7f219-124">For more information, see [Sizes for Windows virtual machines](sizes.md).</span></span>

<span data-ttu-id="7f219-125">Další informace o tom, jak Azure používá dočasným diskovým hello najdete v tématu [pochopení hello jednotku ve virtuálních počítačích Microsoft Azure](https://blogs.msdn.microsoft.com/mast/2013/12/06/understanding-the-temporary-drive-on-windows-azure-virtual-machines/)</span><span class="sxs-lookup"><span data-stu-id="7f219-125">For more information on how Azure uses hello temporary disk, see [Understanding hello temporary drive on Microsoft Azure Virtual Machines](https://blogs.msdn.microsoft.com/mast/2013/12/06/understanding-the-temporary-drive-on-windows-azure-virtual-machines/)</span></span>


### <a name="data-disk"></a><span data-ttu-id="7f219-126">Datový disk</span><span class="sxs-lookup"><span data-stu-id="7f219-126">Data disk</span></span>
<span data-ttu-id="7f219-127">Datový disk je virtuálního pevného disku, který se data aplikací toostore připojené tooa virtuálního počítače nebo jiná data, je nutné tookeep.</span><span class="sxs-lookup"><span data-stu-id="7f219-127">A data disk is a VHD that's attached tooa virtual machine toostore application data, or other data you need tookeep.</span></span> <span data-ttu-id="7f219-128">Datové disky jsou zaregistrované jako disky SCSI a jsou označeny písmenem, který zvolíte.</span><span class="sxs-lookup"><span data-stu-id="7f219-128">Data disks are registered as SCSI drives and are labeled with a letter that you choose.</span></span> <span data-ttu-id="7f219-129">Každý datový disk má maximální kapacitu 4095 GB.</span><span class="sxs-lookup"><span data-stu-id="7f219-129">Each data disk has a maximum capacity of 4095 GB.</span></span> <span data-ttu-id="7f219-130">Hello velikost hello virtuálního počítače určuje, kolik datových disků můžete připojit tooit a hello typ úložiště, můžete použít toohost hello disky.</span><span class="sxs-lookup"><span data-stu-id="7f219-130">hello size of hello virtual machine determines how many data disks you can attach tooit and hello type of storage you can use toohost hello disks.</span></span>

> [!NOTE]
> <span data-ttu-id="7f219-131">Další informace o virtuální počítače kapacity najdete v tématu [virtuální počítače s velikostí pro Windows](sizes.md).</span><span class="sxs-lookup"><span data-stu-id="7f219-131">For more information about virtual machines capacities, see [Sizes for Windows virtual machines](sizes.md).</span></span>
> 

<span data-ttu-id="7f219-132">Azure vytvoří disk operačního systému, když vytvoříte virtuální počítač z bitové kopie.</span><span class="sxs-lookup"><span data-stu-id="7f219-132">Azure creates an operating system disk when you create a virtual machine from an image.</span></span> <span data-ttu-id="7f219-133">Pokud používáte image, která obsahuje datové disky, Azure vytvoří také hello datových disků vytváří hello virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="7f219-133">If you use an image that includes data disks, Azure also creates hello data disks when it creates hello virtual machine.</span></span> <span data-ttu-id="7f219-134">Jinak datových disků přidat po vytvoření virtuálního počítače hello.</span><span class="sxs-lookup"><span data-stu-id="7f219-134">Otherwise, you add data disks after you create hello virtual machine.</span></span>

<span data-ttu-id="7f219-135">Datové disky tooa virtuálního počítače kdykoli, můžete přidat pomocí **připojení** hello disku toohello virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="7f219-135">You can add data disks tooa virtual machine at any time, by **attaching** hello disk toohello virtual machine.</span></span> <span data-ttu-id="7f219-136">Můžete vytvořit virtuální pevný disk, který jste nahráli nebo zkopírovat tooyour účet úložiště, nebo jeden této Azure vytvoří automaticky.</span><span class="sxs-lookup"><span data-stu-id="7f219-136">You can use a VHD that you've uploaded or copied tooyour storage account, or one that Azure creates for you.</span></span> <span data-ttu-id="7f219-137">Připojením datového disku přidruží souboru virtuálního pevného disku hello hello virtuálních počítačů umístěním zapůjčení na hello virtuálního pevného disku, nelze jej odstranit z úložiště při je stále připojen.</span><span class="sxs-lookup"><span data-stu-id="7f219-137">Attaching a data disk associates hello VHD file with hello VM by placing a 'lease' on hello VHD so it can't be deleted from storage while it's still attached.</span></span>


[!INCLUDE [storage-about-vhds-and-disks-windows-and-linux](../../../includes/storage-about-vhds-and-disks-windows-and-linux.md)]

## <a name="one-last-recommendation-use-trim-with-unmanaged-standard-disks"></a><span data-ttu-id="7f219-138">Jedním z poslední doporučení: použití TRIM s nespravované standardní disky</span><span class="sxs-lookup"><span data-stu-id="7f219-138">One last recommendation: Use TRIM with unmanaged standard disks</span></span> 

<span data-ttu-id="7f219-139">Pokud používáte nespravované standardní disky (HDD), měli byste povolit uvolnění dočasné paměti.</span><span class="sxs-lookup"><span data-stu-id="7f219-139">If you use unmanaged standard disks (HDD), you should enable TRIM.</span></span> <span data-ttu-id="7f219-140">TRIM zahodí nepoužívané bloky na disku hello tak fakturuje se pouze pro úložiště, které skutečně používáte.</span><span class="sxs-lookup"><span data-stu-id="7f219-140">TRIM discards unused blocks on hello disk so you are only billed for storage that you are actually using.</span></span> <span data-ttu-id="7f219-141">To můžete uložit na náklady, pokud chcete vytvořit velkých souborů a pak odstraňte je.</span><span class="sxs-lookup"><span data-stu-id="7f219-141">This can save on costs if you create large files and then delete them.</span></span> 

<span data-ttu-id="7f219-142">Můžete spustit toto OŘÍZNUTÍ nastavení příkaz toocheck hello.</span><span class="sxs-lookup"><span data-stu-id="7f219-142">You can run this command toocheck hello TRIM setting.</span></span> <span data-ttu-id="7f219-143">Otevřete příkazový řádek na virtuální počítač s Windows a zadejte:</span><span class="sxs-lookup"><span data-stu-id="7f219-143">Open a command prompt on your Windows VM and type:</span></span>


```
fsutil behavior query DisableDeleteNotify
```

<span data-ttu-id="7f219-144">Pokud příkaz hello vrátí hodnotu 0, TRIM povolený správně.</span><span class="sxs-lookup"><span data-stu-id="7f219-144">If hello command returns 0, TRIM is enabled correctly.</span></span> <span data-ttu-id="7f219-145">Pokud vrátí hodnotu 1, spusťte následující příkaz tooenable TRIM hello:</span><span class="sxs-lookup"><span data-stu-id="7f219-145">If it returns 1, run hello following command tooenable TRIM:</span></span>

```
fsutil behavior set DisableDeleteNotify 0
```

> [!NOTE]
> <span data-ttu-id="7f219-146">Poznámka: Podpora uvolnění dočasné paměti spustí s Windows serverem 2012 nebo Windows 8 a vyšší, najdete v tématu [nové rozhraní API umožňuje aplikacím toosend "TRIM a zrušit mapování" pomocné parametry toostorage média](https://msdn.microsoft.com/windows/compatibility/new-api-allows-apps-to-send-trim-and-unmap-hints).</span><span class="sxs-lookup"><span data-stu-id="7f219-146">Note: Trim support starts with Windows Server 2012 / Windows 8 and above, see see [New API allows apps toosend "TRIM and Unmap" hints toostorage media](https://msdn.microsoft.com/windows/compatibility/new-api-allows-apps-to-send-trim-and-unmap-hints).</span></span>
> 

<!-- Might want toomatch next-steps from overview of managed disks -->
## <a name="next-steps"></a><span data-ttu-id="7f219-147">Další kroky</span><span class="sxs-lookup"><span data-stu-id="7f219-147">Next steps</span></span>
* <span data-ttu-id="7f219-148">[Připojit disk](attach-disk-portal.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) tooadd dodatečné úložiště pro virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="7f219-148">[Attach a disk](attach-disk-portal.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) tooadd additional storage for your VM.</span></span>
* <span data-ttu-id="7f219-149">[Změnit písmeno jednotky hello dočasné disku Windows hello](change-drive-letter.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json) tak vaše aplikace může použít hello D: disku pro data.</span><span class="sxs-lookup"><span data-stu-id="7f219-149">[Change hello drive letter of hello Windows temporary disk](change-drive-letter.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json) so your application can use hello D: drive for data.</span></span>

