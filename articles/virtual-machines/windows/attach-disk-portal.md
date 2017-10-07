---
title: "aaaAttach nespravovaná data disku tooa virtuální počítač s Windows - Azure | Microsoft Docs"
description: "Jak tooattach nový nebo existující nespravovaná data disku tooa virtuální počítač s Windows v Azure pomocí portálu hello hello modelu nasazení Resource Manager."
services: virtual-machines-windows
documentationcenter: 
author: cynthn
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 3790fc59-7264-41df-b7a3-8d1226799885
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 05/09/2017
ms.author: cynthn
ms.openlocfilehash: 923a6663974143bf359970f9b0eb0d36cabcba9c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="how-tooattach-an-unmanaged-data-disk-tooa-windows-vm-in-hello-azure-portal"></a><span data-ttu-id="3a705-103">Jak tooattach nespravovaná data na disku tooa virtuální počítač s Windows v hello portálu Azure</span><span class="sxs-lookup"><span data-stu-id="3a705-103">How tooattach an unmanaged data disk tooa Windows VM in hello Azure portal</span></span>

<span data-ttu-id="3a705-104">Tento článek ukazuje, jak nespravované tooattach nové i stávající disky tooWindows virtuálním počítačům prostřednictvím hello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="3a705-104">This article shows you how tooattach both new and existing unmanaged disks tooWindows virtual machines through hello Azure portal.</span></span> <span data-ttu-id="3a705-105">Můžete také [připojit datový disk pomocí prostředí PowerShell](./attach-disk-ps.md).</span><span class="sxs-lookup"><span data-stu-id="3a705-105">You can also [attach a data disk using PowerShell](./attach-disk-ps.md).</span></span> <span data-ttu-id="3a705-106">Než to uděláte, projděte si tyto tipy:</span><span class="sxs-lookup"><span data-stu-id="3a705-106">Before you do this, review these tips:</span></span>

* <span data-ttu-id="3a705-107">velikost Hello hello virtuálního počítače určuje, kolik datových disků můžete připojit.</span><span class="sxs-lookup"><span data-stu-id="3a705-107">hello size of hello virtual machine controls how many data disks you can attach.</span></span> <span data-ttu-id="3a705-108">Podrobnosti najdete v tématu [velikosti virtuálních počítačů](sizes.md).</span><span class="sxs-lookup"><span data-stu-id="3a705-108">For details, see [Sizes for virtual machines](sizes.md).</span></span>
* <span data-ttu-id="3a705-109">toouse storage úrovně Premium, budete potřebovat DS-series nebo GS-series virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="3a705-109">toouse Premium storage, you need a DS-series or GS-series virtual machine.</span></span> <span data-ttu-id="3a705-110">Můžete použít disky z účty úložiště Premium a Standard s těchto virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="3a705-110">You can use disks from both Premium and Standard storage accounts with these virtual machines.</span></span> <span data-ttu-id="3a705-111">Storage úrovně Premium je k dispozici v určité oblasti.</span><span class="sxs-lookup"><span data-stu-id="3a705-111">Premium storage is available in certain regions.</span></span> <span data-ttu-id="3a705-112">Podrobnosti najdete v tématu [úložiště Premium: vysoce výkonné úložiště pro úlohy virtuálních počítačů Azure](../../storage/storage-premium-storage.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="3a705-112">For details, see [Premium Storage: High-Performance Storage for Azure Virtual Machine Workloads](../../storage/storage-premium-storage.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span>
* <span data-ttu-id="3a705-113">Pro nový disk, nepotřebujete toocreate ho první protože Azure ji vytvoří, když jeho připojení.</span><span class="sxs-lookup"><span data-stu-id="3a705-113">For a new disk, you don't need toocreate it first because Azure creates it when you attach it.</span></span>


<span data-ttu-id="3a705-114">Můžete také [připojit datový disk pomocí prostředí Powershell](attach-disk-ps.md).</span><span class="sxs-lookup"><span data-stu-id="3a705-114">You can also [attach a data disk using Powershell](attach-disk-ps.md).</span></span>


## <a name="find-hello-virtual-machine"></a><span data-ttu-id="3a705-115">Najít hello virtuální počítač</span><span class="sxs-lookup"><span data-stu-id="3a705-115">Find hello virtual machine</span></span>
1. <span data-ttu-id="3a705-116">Přihlaste se toohello [portál Azure](https://portal.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="3a705-116">Sign in toohello [Azure portal](https://portal.azure.com/).</span></span>
2. <span data-ttu-id="3a705-117">V nabídce hello hello vlevo, klikněte na **virtuální počítače**.</span><span class="sxs-lookup"><span data-stu-id="3a705-117">In hello menu on hello left, click **Virtual Machines**.</span></span>
3. <span data-ttu-id="3a705-118">Vyberte virtuální počítač hello hello seznamu.</span><span class="sxs-lookup"><span data-stu-id="3a705-118">Select hello virtual machine from hello list.</span></span>
4. <span data-ttu-id="3a705-119">V okně hello virtuální počítače, klikněte na tlačítko **disky**.</span><span class="sxs-lookup"><span data-stu-id="3a705-119">In hello Virtual machines blade, click **Disks**.</span></span>
   
<span data-ttu-id="3a705-120">Pokračujte podle pokynů pro připojení buď [nový disk](#option-1-attach-a-new-disk) nebo [stávající disk](#option-2-attach-an-existing-disk).</span><span class="sxs-lookup"><span data-stu-id="3a705-120">Continue by following instructions for attaching either a [new disk](#option-1-attach-a-new-disk) or an [existing disk](#option-2-attach-an-existing-disk).</span></span>

## <a name="option-1-attach-and-initialize-a-new-disk"></a><span data-ttu-id="3a705-121">Možnost 1: Připojte a inicializace nový disk</span><span class="sxs-lookup"><span data-stu-id="3a705-121">Option 1: Attach and initialize a new disk</span></span>
1. <span data-ttu-id="3a705-122">Na hello **disky** okně klikněte na tlačítko **+ přidat datový disk**.</span><span class="sxs-lookup"><span data-stu-id="3a705-122">On hello **Disks** blade, click **+ Add data disk**.</span></span>
2. <span data-ttu-id="3a705-123">V hello **spravovaných disků připojit** okno, zadejte název disku hello **název** a pak vyberte **nový (prázdný disk)** v **typ zdroje**.</span><span class="sxs-lookup"><span data-stu-id="3a705-123">In hello **Attach managed disk** blade, type a name for hello disk in **Name** and then select **New (empty disk)** in **Source type**.</span></span>
3. <span data-ttu-id="3a705-124">V části **kontejner úložiště**, klikněte na tlačítko hello **Procházet** tlačítko a toohello účet úložiště a kontejneru, kde chcete vytvořit hello nový virtuální pevný disk toobe uložené a pak klikněte na tlačítko Procházet **vyberte**.</span><span class="sxs-lookup"><span data-stu-id="3a705-124">Under **Storage container**, click hello **Browse** button and browse toohello storage account and container where you would like hello new VHD toobe stored and then click **Select**.</span></span> 
  
   ![Zkontrolujte nastavení disku](./media/attach-disk-portal/attach-empty-unmanaged.png)
   
3. <span data-ttu-id="3a705-126">Až skončíte s hello nastavení pro hello datový disk, klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="3a705-126">When you are done with hello settings for hello data disk, click **OK**.</span></span>
4. <span data-ttu-id="3a705-127">Zpět v hello **disky** okně klikněte na tlačítko **Uložit** tooadd hello disku toohello konfiguraci.</span><span class="sxs-lookup"><span data-stu-id="3a705-127">Back in hello **Disks** blade, click **Save** tooadd hello disk toohello VM configuration.</span></span>


### <a name="initialize-a-new-data-disk"></a><span data-ttu-id="3a705-128">Inicializace nový datový disk</span><span class="sxs-lookup"><span data-stu-id="3a705-128">Initialize a new data disk</span></span>

1. <span data-ttu-id="3a705-129">Připojte toohello virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="3a705-129">Connect toohello virtual machine.</span></span> <span data-ttu-id="3a705-130">Pokyny najdete v tématu [jak se tooconnect a protokol na tooan Azure virtuální počítač, systém Windows spuštěný](connect-logon.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="3a705-130">For instructions, see [How tooconnect and log on tooan Azure virtual machine running Windows](connect-logon.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span>
1. <span data-ttu-id="3a705-131">Klikněte na tlačítko hello **spustit** nabídky uvnitř hello virtuálních počítačů a typ **diskmgmt.msc** a počtu **Enter**.</span><span class="sxs-lookup"><span data-stu-id="3a705-131">Click hello **Start** menu inside hello VM and type **diskmgmt.msc** and hit **Enter**.</span></span> <span data-ttu-id="3a705-132">Otevře se modul snap-in Správa disků hello.</span><span class="sxs-lookup"><span data-stu-id="3a705-132">This starts hello Disk Management snap-in.</span></span>
2. <span data-ttu-id="3a705-133">Správa disků rozpozná, že máte nové, neinicializovaného disku a bude překryvné okno inicializovat Disk hello.</span><span class="sxs-lookup"><span data-stu-id="3a705-133">Disk Management recognizes that you have a new, uninitialized disk and hello Initialize Disk window will pop up.</span></span>
3. <span data-ttu-id="3a705-134">Musí být vybrána hello nový disk a klikněte na tlačítko **OK** tooinitialize ho.</span><span class="sxs-lookup"><span data-stu-id="3a705-134">Make sure hello new disk is selected and click **OK** tooinitialize it.</span></span>
4. <span data-ttu-id="3a705-135">Hello nový disk se teď zobrazí jako **nepřidělené**.</span><span class="sxs-lookup"><span data-stu-id="3a705-135">hello new disk now appears as **unallocated**.</span></span> <span data-ttu-id="3a705-136">Klikněte pravým tlačítkem na libovolné místo na disku hello a vyberte **nový jednoduchý svazek**.</span><span class="sxs-lookup"><span data-stu-id="3a705-136">Right-click anywhere on hello disk and select **New simple volume**.</span></span> <span data-ttu-id="3a705-137">Hello **Průvodci vytvořením jednoduchého svazku** spustí.</span><span class="sxs-lookup"><span data-stu-id="3a705-137">hello **New Simple Volume Wizard** starts.</span></span>
5. <span data-ttu-id="3a705-138">Projděte hello průvodce, udržuje všechny výchozí hodnoty hello, po dokončení vyberte **Dokončit**.</span><span class="sxs-lookup"><span data-stu-id="3a705-138">Go through hello wizard, keeping all of hello defaults, when you are done select **Finish**.</span></span>
6. <span data-ttu-id="3a705-139">Zavřete Správa disků.</span><span class="sxs-lookup"><span data-stu-id="3a705-139">Close Disk Management.</span></span>
7. <span data-ttu-id="3a705-140">Získat automaticky otevíraného okna je nutné tooformat hello nový disk, abyste mohli používat.</span><span class="sxs-lookup"><span data-stu-id="3a705-140">You get a pop-up that you need tooformat hello new disk before you can use it.</span></span> <span data-ttu-id="3a705-141">Klikněte na tlačítko **formát disku**.</span><span class="sxs-lookup"><span data-stu-id="3a705-141">Click **Format disk**.</span></span>
8. <span data-ttu-id="3a705-142">V hello **nový disk formát** dialogové okno, zkontrolujte hello nastavení a pak klikněte na tlačítko **spustit**.</span><span class="sxs-lookup"><span data-stu-id="3a705-142">In hello **Format new disk** dialog, check hello settings and then click **Start**.</span></span>
9. <span data-ttu-id="3a705-143">Zobrazí upozornění, že formátování hello disky vymaže všechna hello data, klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="3a705-143">You get a warning that formatting hello disks erases all of hello data, click **OK**.</span></span>
10. <span data-ttu-id="3a705-144">Po dokončení hello Formát klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="3a705-144">When hello format is complete, click **OK**.</span></span>


## <a name="option-2-attach-an-existing-disk"></a><span data-ttu-id="3a705-145">Možnost 2: Připojit stávající disk</span><span class="sxs-lookup"><span data-stu-id="3a705-145">Option 2: Attach an existing disk</span></span>
1. <span data-ttu-id="3a705-146">Na hello **disky** okně klikněte na tlačítko **+ přidat datový disk**.</span><span class="sxs-lookup"><span data-stu-id="3a705-146">On hello **Disks** blade, click **+ Add data disk**.</span></span>
2. <span data-ttu-id="3a705-147">Na hello **nespravované disk připojit** okno v **typ zdroje** vyberte **existující objekt blob**.</span><span class="sxs-lookup"><span data-stu-id="3a705-147">On hello **Attach unmanaged disk** blade, in **Source type** select **Existing blob**.</span></span>

    ![Zkontrolujte nastavení disku](./media/attach-disk-portal/attach-existing-unmanaged.png)

    3. <span data-ttu-id="3a705-149">Klikněte na tlačítko **Procházet** toonavigate toohello účtu úložiště a kontejneru, kde hello existující virtuální pevný disk nachází.</span><span class="sxs-lookup"><span data-stu-id="3a705-149">Click **Browse** toonavigate toohello storage account and container where hello existing VHD is located.</span></span> <span data-ttu-id="3a705-150">Klikněte na tlačítko hello virtuálního pevného disku a pak klikněte na tlačítko **vyberte**.</span><span class="sxs-lookup"><span data-stu-id="3a705-150">Click and hello VHD and then click **Select**.</span></span>
4. <span data-ttu-id="3a705-151">Klikněte na tlačítko **OK** v hello **nespravované disk připojit** okno.</span><span class="sxs-lookup"><span data-stu-id="3a705-151">Click **OK** in hello **Attach unmanaged disk** blade.</span></span>
5. <span data-ttu-id="3a705-152">V hello **disky** okně klikněte na tlačítko **Uložit** tooadd hello disku toohello konfigurace pro hello virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="3a705-152">In hello **Disks** blade, click **Save** tooadd hello disk toohello configuration for hello VM.</span></span>
   


## <a name="use-trim-with-standard-storage"></a><span data-ttu-id="3a705-153">Použít standardní úložiště uvolnění dočasné paměti</span><span class="sxs-lookup"><span data-stu-id="3a705-153">Use TRIM with standard storage</span></span>

<span data-ttu-id="3a705-154">Pokud chcete použít standardní úložiště (HDD), měli byste povolit uvolnění dočasné paměti.</span><span class="sxs-lookup"><span data-stu-id="3a705-154">If you use standard storage (HDD), you should enable TRIM.</span></span> <span data-ttu-id="3a705-155">TRIM zahodí nepoužívané bloky na disku hello tak fakturuje se pouze pro úložiště, které skutečně používáte.</span><span class="sxs-lookup"><span data-stu-id="3a705-155">TRIM discards unused blocks on hello disk so you are only billed for storage that you are actually using.</span></span> <span data-ttu-id="3a705-156">To můžete uložit na náklady, pokud chcete vytvořit velkých souborů a pak odstraňte je.</span><span class="sxs-lookup"><span data-stu-id="3a705-156">This can save on costs if you create large files and then delete them.</span></span> 

<span data-ttu-id="3a705-157">Můžete spustit toto OŘÍZNUTÍ nastavení příkaz toocheck hello.</span><span class="sxs-lookup"><span data-stu-id="3a705-157">You can run this command toocheck hello TRIM setting.</span></span> <span data-ttu-id="3a705-158">Otevřete příkazový řádek na virtuální počítač s Windows a zadejte:</span><span class="sxs-lookup"><span data-stu-id="3a705-158">Open a command prompt on your Windows VM and type:</span></span>

```
fsutil behavior query DisableDeleteNotify
```

<span data-ttu-id="3a705-159">Pokud příkaz hello vrátí hodnotu 0, TRIM povolený správně.</span><span class="sxs-lookup"><span data-stu-id="3a705-159">If hello command returns 0, TRIM is enabled correctly.</span></span> <span data-ttu-id="3a705-160">Pokud vrátí hodnotu 1, spusťte následující příkaz tooenable TRIM hello:</span><span class="sxs-lookup"><span data-stu-id="3a705-160">If it returns 1, run hello following command tooenable TRIM:</span></span>
```
fsutil behavior set DisableDeleteNotify 0
```

<span data-ttu-id="3a705-161">Po odstranění dat z disku, můžete zajistit, že hello TRIM operace vyprázdnění správně spuštěním defragmentační s uvolnění dočasné paměti:</span><span class="sxs-lookup"><span data-stu-id="3a705-161">After deleting data from your disk, you can ensure hello TRIM operations flush properly by running defrag with TRIM:</span></span>

```
defrag.exe <volume:> -l
```

<span data-ttu-id="3a705-162">Můžete také zajistit, že hello celý svazek je oříznut podle formátování svazku hello.</span><span class="sxs-lookup"><span data-stu-id="3a705-162">You can also ensure hello entire volume is trimmed by formatting hello volume.</span></span>


## <a name="next-steps"></a><span data-ttu-id="3a705-163">Další kroky</span><span class="sxs-lookup"><span data-stu-id="3a705-163">Next steps</span></span>
<span data-ttu-id="3a705-164">Pokud je aplikace potřebuje toouse hello D: jednotky toostore data, můžete [změnit písmeno jednotky hello dočasné disk systému Windows hello](change-drive-letter.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="3a705-164">If you application needs toouse hello D: drive toostore data, you can [change hello drive letter of hello Windows temporary disk](change-drive-letter.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json).</span></span>

