---
title: "aaaAttach tooa disku classic virtuálního počítače Azure | Microsoft Docs"
description: "Připojit datový disk tooa virtuálního počítače s Windows vytvořené pomocí modelu nasazení classic hello a provést jeho inicializaci."
services: virtual-machines-windows, storage
documentationcenter: 
author: cynthn
manager: timlt
editor: tysonn
tags: azure-service-management
ms.assetid: be4e3e74-05bc-4527-969f-84f10a1d66a7
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 02/21/2017
ms.author: cynthn
ms.openlocfilehash: bfe1b0fa066277d28d3862a18da4b1023cb4452d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="attach-a-data-disk-tooa-windows-virtual-machine-created-with-hello-classic-deployment-model"></a><span data-ttu-id="4743d-103">Připojit datový disk tooa virtuálního počítače s Windows vytvořené pomocí modelu nasazení classic hello</span><span class="sxs-lookup"><span data-stu-id="4743d-103">Attach a data disk tooa Windows virtual machine created with hello classic deployment model</span></span>
<!--
Refernce article:
    If you want toouse hello new portal, see [How tooattach a data disk tooa Windows VM in hello Azure portal](../../virtual-machines-windows-attach-disk-portal.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).
-->

<span data-ttu-id="4743d-104">Tento článek ukazuje, jak tooattach nových nebo stávajících disky vytvořené pomocí hello klasického nasazení modelu tooa virtuálního počítače Windows hello pomocí portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="4743d-104">This article shows you how tooattach new and existing disks created with hello Classic deployment model tooa Windows virtual machine using hello Azure portal.</span></span>

<span data-ttu-id="4743d-105">Můžete také [připojit tooa disku data virtuálního počítače s Linuxem v hello portál Azure](../../linux/attach-disk-portal.md).</span><span class="sxs-lookup"><span data-stu-id="4743d-105">You can also [attach a data disk tooa Linux VM in hello Azure portal](../../linux/attach-disk-portal.md).</span></span>

<span data-ttu-id="4743d-106">Než připojíte disk, projděte si tyto tipy:</span><span class="sxs-lookup"><span data-stu-id="4743d-106">Before you attach a disk, review these tips:</span></span>

* <span data-ttu-id="4743d-107">velikost Hello hello virtuálního počítače určuje, kolik datových disků můžete připojit.</span><span class="sxs-lookup"><span data-stu-id="4743d-107">hello size of hello virtual machine controls how many data disks you can attach.</span></span> <span data-ttu-id="4743d-108">Podrobnosti najdete v tématu [velikosti virtuálních počítačů](../../virtual-machines-windows-sizes.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="4743d-108">For details, see [Sizes for virtual machines](../../virtual-machines-windows-sizes.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span>

* <span data-ttu-id="4743d-109">toouse storage úrovně Premium, budete potřebovat DS-series nebo GS-series virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="4743d-109">toouse Premium storage, you need a DS-series or GS-series virtual machine.</span></span> <span data-ttu-id="4743d-110">Můžete použít disky z účty úložiště Premium a Standard s těchto virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="4743d-110">You can use disks from both Premium and Standard storage accounts with these virtual machines.</span></span> <span data-ttu-id="4743d-111">Storage úrovně Premium je k dispozici v určité oblasti.</span><span class="sxs-lookup"><span data-stu-id="4743d-111">Premium storage is available in certain regions.</span></span> <span data-ttu-id="4743d-112">Podrobnosti najdete v tématu [úložiště Premium: vysoce výkonné úložiště pro úlohy virtuálních počítačů Azure](../../../storage/common/storage-premium-storage.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="4743d-112">For details, see [Premium Storage: High-Performance Storage for Azure Virtual Machine Workloads](../../../storage/common/storage-premium-storage.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span>

* <span data-ttu-id="4743d-113">Pro nový disk, nepotřebujete toocreate ho první protože Azure ji vytvoří, když jeho připojení.</span><span class="sxs-lookup"><span data-stu-id="4743d-113">For a new disk, you don't need toocreate it first because Azure creates it when you attach it.</span></span>

<span data-ttu-id="4743d-114">Můžete také [připojit datový disk pomocí prostředí Powershell](../../virtual-machines-windows-attach-disk-ps.md).</span><span class="sxs-lookup"><span data-stu-id="4743d-114">You can also [attach a data disk using Powershell](../../virtual-machines-windows-attach-disk-ps.md).</span></span>

> [!IMPORTANT]
> <span data-ttu-id="4743d-115">Azure má dva různé modely nasazení pro vytváření a práci s prostředky: [Resource Manager a klasický](../../../resource-manager-deployment-model.md).</span><span class="sxs-lookup"><span data-stu-id="4743d-115">Azure has two different deployment models for creating and working with resources: [Resource Manager and Classic](../../../resource-manager-deployment-model.md).</span></span>

## <a name="find-hello-virtual-machine"></a><span data-ttu-id="4743d-116">Najít hello virtuální počítač</span><span class="sxs-lookup"><span data-stu-id="4743d-116">Find hello virtual machine</span></span>
1. <span data-ttu-id="4743d-117">Přihlaste se toohello [portál Azure](https://portal.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="4743d-117">Sign in toohello [Azure portal](https://portal.azure.com/).</span></span>
2. <span data-ttu-id="4743d-118">Virtuální počítač vyberte hello z prostředku hello uvedené na řídicím panelu hello.</span><span class="sxs-lookup"><span data-stu-id="4743d-118">Select hello virtual machine from hello resource listed on hello dashboard.</span></span>
3. <span data-ttu-id="4743d-119">V levém podokně hello pod **nastavení**, klikněte na tlačítko **disky**.</span><span class="sxs-lookup"><span data-stu-id="4743d-119">In hello left pane under **Settings**, click **Disks**.</span></span>

    ![Otevřete nastavení disku](./media/attach-disk/virtualmachinedisks.png)

<span data-ttu-id="4743d-121">Pokračujte podle pokynů pro připojení buď [nový disk](#option-1-attach-a-new-disk) nebo [stávající disk](#option-2-attach-an-existing-disk).</span><span class="sxs-lookup"><span data-stu-id="4743d-121">Continue by following instructions for attaching either a [new disk](#option-1-attach-a-new-disk) or an [existing disk](#option-2-attach-an-existing-disk).</span></span>

## <a name="option-1-attach-and-initialize-a-new-disk"></a><span data-ttu-id="4743d-122">Možnost 1: Připojte a inicializace nový disk</span><span class="sxs-lookup"><span data-stu-id="4743d-122">Option 1: Attach and initialize a new disk</span></span>

1. <span data-ttu-id="4743d-123">Na hello **disky** okně klikněte na tlačítko **připojit nový**.</span><span class="sxs-lookup"><span data-stu-id="4743d-123">On hello **Disks** blade, click **Attach new**.</span></span>
2. <span data-ttu-id="4743d-124">Zkontrolujte hello výchozí nastavení, aktualizovat podle potřeby a pak klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="4743d-124">Review hello default settings, update as necessary, and then click **OK**.</span></span>

   ![Zkontrolujte nastavení disku](./media/attach-disk/attach-new.png)

3. <span data-ttu-id="4743d-126">Jakmile Azure vytvoří hello disk a připojí jej toohello virtuálního počítače, hello nového disku je uvedena v nastavení disku hello virtuálního počítače v části **datových disků**.</span><span class="sxs-lookup"><span data-stu-id="4743d-126">After Azure creates hello disk and attaches it toohello virtual machine, hello new disk is listed in hello virtual machine's disk settings under **Data Disks**.</span></span>

### <a name="initialize-a-new-data-disk"></a><span data-ttu-id="4743d-127">Inicializace nový datový disk</span><span class="sxs-lookup"><span data-stu-id="4743d-127">Initialize a new data disk</span></span>

1. <span data-ttu-id="4743d-128">Připojte toohello virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="4743d-128">Connect toohello virtual machine.</span></span> <span data-ttu-id="4743d-129">Pokyny najdete v tématu [jak se tooconnect a protokol na tooan Azure virtuální počítač, systém Windows spuštěný](../../virtual-machines-windows-connect-logon.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="4743d-129">For instructions, see [How tooconnect and log on tooan Azure virtual machine running Windows](../../virtual-machines-windows-connect-logon.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span>
2. <span data-ttu-id="4743d-130">Po přihlášení toohello virtuální počítač, otevřete **správce serveru**.</span><span class="sxs-lookup"><span data-stu-id="4743d-130">After you log on toohello virtual machine, open **Server Manager**.</span></span> <span data-ttu-id="4743d-131">V levém podokně hello vyberte **Souborová služba a služba úložiště**.</span><span class="sxs-lookup"><span data-stu-id="4743d-131">In hello left pane, select **File and Storage Services**.</span></span>

    ![Otevřete správce serveru](../media/attach-disk-portal/fileandstorageservices.png)

3. <span data-ttu-id="4743d-133">Vyberte **disky**.</span><span class="sxs-lookup"><span data-stu-id="4743d-133">Select **Disks**.</span></span>
4. <span data-ttu-id="4743d-134">Hello **disky** části jsou uvedené disky hello.</span><span class="sxs-lookup"><span data-stu-id="4743d-134">hello **Disks** section lists hello disks.</span></span> <span data-ttu-id="4743d-135">Virtuální počítač má nejčastěji disk 0, disk 1 a 2 disku.</span><span class="sxs-lookup"><span data-stu-id="4743d-135">Most often, a virtual machine has disk 0, disk 1, and disk 2.</span></span> <span data-ttu-id="4743d-136">Disk 0 je hello operačního systému, disk 1 je hello dočasné a disk 2 je, aby byl datový disk hello nově připojen toohello virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="4743d-136">Disk 0 is hello operating system disk, disk 1 is hello temporary disk, and disk 2 is hello data disk newly attached toohello virtual machine.</span></span> <span data-ttu-id="4743d-137">Hello datového disku seznamy hello oddílu jako **neznámé**.</span><span class="sxs-lookup"><span data-stu-id="4743d-137">hello data disk lists hello Partition as **Unknown**.</span></span>

 <span data-ttu-id="4743d-138">Klikněte pravým tlačítkem na hello disk a vyberte **inicializovat**.</span><span class="sxs-lookup"><span data-stu-id="4743d-138">Right-click hello disk and select **Initialize**.</span></span>

5. <span data-ttu-id="4743d-139">Zobrazení upozornění, že všechna data vymažou se při hello disk inicializován.</span><span class="sxs-lookup"><span data-stu-id="4743d-139">You're notified that all data will be erased when hello disk is initialized.</span></span> <span data-ttu-id="4743d-140">Klikněte na tlačítko **Ano** tooacknowledge hello upozornění a inicializovat hello disku.</span><span class="sxs-lookup"><span data-stu-id="4743d-140">Click **Yes** tooacknowledge hello warning and initialize hello disk.</span></span> <span data-ttu-id="4743d-141">Po dokončení hello oddíl bude uvedený jako **GPT**.</span><span class="sxs-lookup"><span data-stu-id="4743d-141">Once complete, hello partition will be listed as **GPT**.</span></span> <span data-ttu-id="4743d-142">Klikněte pravým tlačítkem na hello disk znovu a vyberte **nový svazek**.</span><span class="sxs-lookup"><span data-stu-id="4743d-142">Right-click hello disk again and select **New Volume**.</span></span>

6. <span data-ttu-id="4743d-143">Dokončete Průvodce hello hello výchozí hodnoty.</span><span class="sxs-lookup"><span data-stu-id="4743d-143">Complete hello wizard using hello default values.</span></span> <span data-ttu-id="4743d-144">Po dokončení Průvodce hello hello **svazky** části je uveden seznam hello nový svazek.</span><span class="sxs-lookup"><span data-stu-id="4743d-144">When hello wizard is done, hello **Volumes** section lists hello new volume.</span></span> <span data-ttu-id="4743d-145">Hello disk je teď online a připravená toostore data.</span><span class="sxs-lookup"><span data-stu-id="4743d-145">hello disk is now online and ready toostore data.</span></span>

    ![Svazek úspěšně inicializován.](./media/attach-disk/newdiskafterinitialization.png)

## <a name="option-2-attach-an-existing-disk"></a><span data-ttu-id="4743d-147">Možnost 2: Připojit stávající disk</span><span class="sxs-lookup"><span data-stu-id="4743d-147">Option 2: Attach an existing disk</span></span>
1. <span data-ttu-id="4743d-148">Na hello **disky** okně klikněte na tlačítko **připojit existující**.</span><span class="sxs-lookup"><span data-stu-id="4743d-148">On hello **Disks** blade, click **Attach existing**.</span></span>
2. <span data-ttu-id="4743d-149">V části **připojit stávající disk**, klikněte na tlačítko **umístění**.</span><span class="sxs-lookup"><span data-stu-id="4743d-149">Under **Attach existing disk**, click **Location**.</span></span>

   ![Připojit stávající disk](./media/attach-disk/attachexistingdisksettings.png)
3. <span data-ttu-id="4743d-151">V části **účty úložiště**, vyberte účet hello a kontejner, který obsahuje soubor VHD hello.</span><span class="sxs-lookup"><span data-stu-id="4743d-151">Under **Storage accounts**, select hello account and container that holds hello .vhd file.</span></span>

   ![Vyhledejte umístění virtuálního pevného disku](./media/attach-disk/existdiskstorageaccountandcontainer.png)

4. <span data-ttu-id="4743d-153">Vyberte soubor VHD hello.</span><span class="sxs-lookup"><span data-stu-id="4743d-153">Select hello .vhd file.</span></span>
5. <span data-ttu-id="4743d-154">V části **připojit stávající disk**, právě vybraný soubor hello je uveden v části **souboru virtuálního pevného disku**.</span><span class="sxs-lookup"><span data-stu-id="4743d-154">Under **Attach existing disk**, hello file you just selected is listed under **VHD File**.</span></span> <span data-ttu-id="4743d-155">Klikněte na **OK**.</span><span class="sxs-lookup"><span data-stu-id="4743d-155">Click **OK**.</span></span>
6. <span data-ttu-id="4743d-156">Po Azure připojí hello disku toohello virtuálního počítače, je uvedena v nastavení disku hello virtuálního počítače v části **datových disků**.</span><span class="sxs-lookup"><span data-stu-id="4743d-156">After Azure attaches hello disk toohello virtual machine, it's listed in hello virtual machine's disk settings under **Data Disks**.</span></span>

## <a name="use-trim-with-standard-storage"></a><span data-ttu-id="4743d-157">Použít standardní úložiště uvolnění dočasné paměti</span><span class="sxs-lookup"><span data-stu-id="4743d-157">Use TRIM with standard storage</span></span>

<span data-ttu-id="4743d-158">Pokud chcete použít standardní úložiště (HDD), měli byste povolit uvolnění dočasné paměti.</span><span class="sxs-lookup"><span data-stu-id="4743d-158">If you use standard storage (HDD), you should enable TRIM.</span></span> <span data-ttu-id="4743d-159">TRIM zahodí nepoužívané bloky na disku hello tak fakturuje se pouze pro úložiště, které skutečně používáte.</span><span class="sxs-lookup"><span data-stu-id="4743d-159">TRIM discards unused blocks on hello disk so you are only billed for storage that you are actually using.</span></span> <span data-ttu-id="4743d-160">Pomocí uvolnění dočasné paměti může významně snížit náklady, včetně nepoužívané bloky, které jsou výsledkem odstranění velkých souborů.</span><span class="sxs-lookup"><span data-stu-id="4743d-160">Using TRIM can save costs, including unused blocks that result from deleting large files.</span></span>

<span data-ttu-id="4743d-161">Můžete spustit toto OŘÍZNUTÍ nastavení příkaz toocheck hello.</span><span class="sxs-lookup"><span data-stu-id="4743d-161">You can run this command toocheck hello TRIM setting.</span></span> <span data-ttu-id="4743d-162">Otevřete příkazový řádek na virtuální počítač s Windows a zadejte:</span><span class="sxs-lookup"><span data-stu-id="4743d-162">Open a command prompt on your Windows VM and type:</span></span>

```
fsutil behavior query DisableDeleteNotify
```

<span data-ttu-id="4743d-163">Pokud příkaz hello vrátí hodnotu 0, TRIM povolený správně.</span><span class="sxs-lookup"><span data-stu-id="4743d-163">If hello command returns 0, TRIM is enabled correctly.</span></span> <span data-ttu-id="4743d-164">Pokud vrátí hodnotu 1, spusťte následující příkaz tooenable TRIM hello:</span><span class="sxs-lookup"><span data-stu-id="4743d-164">If it returns 1, run hello following command tooenable TRIM:</span></span>
```
fsutil behavior set DisableDeleteNotify 0
```

## <a name="next-steps"></a><span data-ttu-id="4743d-165">Další kroky</span><span class="sxs-lookup"><span data-stu-id="4743d-165">Next steps</span></span>
<span data-ttu-id="4743d-166">Pokud aplikace potřebuje toouse hello D: jednotky toostore data, můžete [změnit písmeno jednotky hello dočasné disk systému Windows hello](../../virtual-machines-windows-change-drive-letter.md).</span><span class="sxs-lookup"><span data-stu-id="4743d-166">If your application needs toouse hello D: drive toostore data, you can [change hello drive letter of hello Windows temporary disk](../../virtual-machines-windows-change-drive-letter.md).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="4743d-167">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="4743d-167">Additional resources</span></span>
[<span data-ttu-id="4743d-168">O disky a virtuální pevné disky pro virtuální počítače</span><span class="sxs-lookup"><span data-stu-id="4743d-168">About disks and VHDs for virtual machines</span></span>](../../virtual-machines-linux-about-disks-vhds.md)
