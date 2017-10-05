---
title: "Připojit nespravované datový disk k virtuálnímu počítači Windows - Azure | Microsoft Docs"
description: "Jak připojit nový nebo existující nespravované datový disk pro virtuální počítač s Windows v portálu Azure pomocí modelu nasazení Resource Manager."
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
ms.openlocfilehash: c0886302c82641f8cc1a00d3972870d58ba8afb4
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="how-to-attach-an-unmanaged-data-disk-to-a-windows-vm-in-the-azure-portal"></a><span data-ttu-id="e0f5f-103">Tom, jak připojit nespravované datový disk k virtuálnímu počítači Windows na portálu Azure</span><span class="sxs-lookup"><span data-stu-id="e0f5f-103">How to attach an unmanaged data disk to a Windows VM in the Azure portal</span></span>

<span data-ttu-id="e0f5f-104">Tento článek ukazuje, jak připojit nové i stávající nespravované disky pro virtuální počítače s Windows pomocí portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="e0f5f-104">This article shows you how to attach both new and existing unmanaged disks to Windows virtual machines through the Azure portal.</span></span> <span data-ttu-id="e0f5f-105">Můžete také [připojit datový disk pomocí prostředí PowerShell](./attach-disk-ps.md).</span><span class="sxs-lookup"><span data-stu-id="e0f5f-105">You can also [attach a data disk using PowerShell](./attach-disk-ps.md).</span></span> <span data-ttu-id="e0f5f-106">Než to uděláte, projděte si tyto tipy:</span><span class="sxs-lookup"><span data-stu-id="e0f5f-106">Before you do this, review these tips:</span></span>

* <span data-ttu-id="e0f5f-107">Velikost virtuálního počítače určuje, kolik datových disků můžete připojit.</span><span class="sxs-lookup"><span data-stu-id="e0f5f-107">The size of the virtual machine controls how many data disks you can attach.</span></span> <span data-ttu-id="e0f5f-108">Podrobnosti najdete v tématu [velikosti virtuálních počítačů](sizes.md).</span><span class="sxs-lookup"><span data-stu-id="e0f5f-108">For details, see [Sizes for virtual machines](sizes.md).</span></span>
* <span data-ttu-id="e0f5f-109">Chcete-li používat úložiště úrovně Premium, je třeba DS-series nebo GS-series virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="e0f5f-109">To use Premium storage, you need a DS-series or GS-series virtual machine.</span></span> <span data-ttu-id="e0f5f-110">Můžete použít disky z účty úložiště Premium a Standard s těchto virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="e0f5f-110">You can use disks from both Premium and Standard storage accounts with these virtual machines.</span></span> <span data-ttu-id="e0f5f-111">Storage úrovně Premium je k dispozici v určité oblasti.</span><span class="sxs-lookup"><span data-stu-id="e0f5f-111">Premium storage is available in certain regions.</span></span> <span data-ttu-id="e0f5f-112">Podrobnosti najdete v tématu [úložiště Premium: vysoce výkonné úložiště pro úlohy virtuálních počítačů Azure](../../storage/storage-premium-storage.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="e0f5f-112">For details, see [Premium Storage: High-Performance Storage for Azure Virtual Machine Workloads](../../storage/storage-premium-storage.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span>
* <span data-ttu-id="e0f5f-113">Pro nový disk nemusíte nejdřív ji vytvořit, protože Azure ji vytvoří, když jeho připojení.</span><span class="sxs-lookup"><span data-stu-id="e0f5f-113">For a new disk, you don't need to create it first because Azure creates it when you attach it.</span></span>


<span data-ttu-id="e0f5f-114">Můžete také [připojit datový disk pomocí prostředí Powershell](attach-disk-ps.md).</span><span class="sxs-lookup"><span data-stu-id="e0f5f-114">You can also [attach a data disk using Powershell](attach-disk-ps.md).</span></span>


## <a name="find-the-virtual-machine"></a><span data-ttu-id="e0f5f-115">Najít virtuální počítač</span><span class="sxs-lookup"><span data-stu-id="e0f5f-115">Find the virtual machine</span></span>
1. <span data-ttu-id="e0f5f-116">Přihlaste se k webu [Azure Portal](https://portal.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="e0f5f-116">Sign in to the [Azure portal](https://portal.azure.com/).</span></span>
2. <span data-ttu-id="e0f5f-117">V nabídce na levé straně klikněte na tlačítko **virtuální počítače**.</span><span class="sxs-lookup"><span data-stu-id="e0f5f-117">In the menu on the left, click **Virtual Machines**.</span></span>
3. <span data-ttu-id="e0f5f-118">Ze seznamu vyberte virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="e0f5f-118">Select the virtual machine from the list.</span></span>
4. <span data-ttu-id="e0f5f-119">V okně virtuální počítače, klikněte na tlačítko **disky**.</span><span class="sxs-lookup"><span data-stu-id="e0f5f-119">In the Virtual machines blade, click **Disks**.</span></span>
   
<span data-ttu-id="e0f5f-120">Pokračujte podle pokynů pro připojení buď [nový disk](#option-1-attach-a-new-disk) nebo [stávající disk](#option-2-attach-an-existing-disk).</span><span class="sxs-lookup"><span data-stu-id="e0f5f-120">Continue by following instructions for attaching either a [new disk](#option-1-attach-a-new-disk) or an [existing disk](#option-2-attach-an-existing-disk).</span></span>

## <a name="option-1-attach-and-initialize-a-new-disk"></a><span data-ttu-id="e0f5f-121">Možnost 1: Připojte a inicializace nový disk</span><span class="sxs-lookup"><span data-stu-id="e0f5f-121">Option 1: Attach and initialize a new disk</span></span>
1. <span data-ttu-id="e0f5f-122">Na **disky** okně klikněte na tlačítko **+ přidat datový disk**.</span><span class="sxs-lookup"><span data-stu-id="e0f5f-122">On the **Disks** blade, click **+ Add data disk**.</span></span>
2. <span data-ttu-id="e0f5f-123">V **spravovaných disků připojit** okno, zadejte název disku **název** a pak vyberte **nový (prázdný disk)** v **typ zdroje**.</span><span class="sxs-lookup"><span data-stu-id="e0f5f-123">In the **Attach managed disk** blade, type a name for the disk in **Name** and then select **New (empty disk)** in **Source type**.</span></span>
3. <span data-ttu-id="e0f5f-124">V části **kontejner úložiště**, klikněte **Procházet** tlačítko a přejděte do účtu úložiště a kontejner, kam chcete uložit a potom klikněte na nový virtuální pevný disk **vyberte**.</span><span class="sxs-lookup"><span data-stu-id="e0f5f-124">Under **Storage container**, click the **Browse** button and browse to the storage account and container where you would like the new VHD to be stored and then click **Select**.</span></span> 
  
   ![Zkontrolujte nastavení disku](./media/attach-disk-portal/attach-empty-unmanaged.png)
   
3. <span data-ttu-id="e0f5f-126">Až skončíte s nastavením pro datový disk, klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="e0f5f-126">When you are done with the settings for the data disk, click **OK**.</span></span>
4. <span data-ttu-id="e0f5f-127">Zpět v **disky** okně klikněte na tlačítko **Uložit** disk přidejte do konfigurace virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="e0f5f-127">Back in the **Disks** blade, click **Save** to add the disk to the VM configuration.</span></span>


### <a name="initialize-a-new-data-disk"></a><span data-ttu-id="e0f5f-128">Inicializace nový datový disk</span><span class="sxs-lookup"><span data-stu-id="e0f5f-128">Initialize a new data disk</span></span>

1. <span data-ttu-id="e0f5f-129">Připojte k virtuálnímu počítači.</span><span class="sxs-lookup"><span data-stu-id="e0f5f-129">Connect to the virtual machine.</span></span> <span data-ttu-id="e0f5f-130">Pokyny najdete v tématu [jak se připojit a přihlásit se na virtuálním počítači Azure s Windows](connect-logon.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="e0f5f-130">For instructions, see [How to connect and log on to an Azure virtual machine running Windows](connect-logon.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span>
1. <span data-ttu-id="e0f5f-131">Klikněte na tlačítko **spustit** nabídky uvnitř virtuálního počítače a typ **diskmgmt.msc** a počtu **Enter**.</span><span class="sxs-lookup"><span data-stu-id="e0f5f-131">Click the **Start** menu inside the VM and type **diskmgmt.msc** and hit **Enter**.</span></span> <span data-ttu-id="e0f5f-132">Otevře se modul snap-in Správa disků.</span><span class="sxs-lookup"><span data-stu-id="e0f5f-132">This starts the Disk Management snap-in.</span></span>
2. <span data-ttu-id="e0f5f-133">Správa disků rozpozná, že máte nové, neinicializovaného disku a bude překryvné okno inicializovat Disk.</span><span class="sxs-lookup"><span data-stu-id="e0f5f-133">Disk Management recognizes that you have a new, uninitialized disk and the Initialize Disk window will pop up.</span></span>
3. <span data-ttu-id="e0f5f-134">Zkontrolujte, že je vybraný nový disk a klikněte na tlačítko **OK** k chybě při inicializaci ho.</span><span class="sxs-lookup"><span data-stu-id="e0f5f-134">Make sure the new disk is selected and click **OK** to initialize it.</span></span>
4. <span data-ttu-id="e0f5f-135">Nový disk se zobrazí jako **nepřidělené**.</span><span class="sxs-lookup"><span data-stu-id="e0f5f-135">The new disk now appears as **unallocated**.</span></span> <span data-ttu-id="e0f5f-136">Klepněte pravým tlačítkem myši na disk a vyberte **nový jednoduchý svazek**.</span><span class="sxs-lookup"><span data-stu-id="e0f5f-136">Right-click anywhere on the disk and select **New simple volume**.</span></span> <span data-ttu-id="e0f5f-137">**Průvodci vytvořením jednoduchého svazku** spustí.</span><span class="sxs-lookup"><span data-stu-id="e0f5f-137">The **New Simple Volume Wizard** starts.</span></span>
5. <span data-ttu-id="e0f5f-138">Postupujte dle pokynů průvodce, udržuje všechny výchozí hodnoty, po dokončení vyberte **Dokončit**.</span><span class="sxs-lookup"><span data-stu-id="e0f5f-138">Go through the wizard, keeping all of the defaults, when you are done select **Finish**.</span></span>
6. <span data-ttu-id="e0f5f-139">Zavřete Správa disků.</span><span class="sxs-lookup"><span data-stu-id="e0f5f-139">Close Disk Management.</span></span>
7. <span data-ttu-id="e0f5f-140">Zobrazí automaticky otevírané okno, které potřebujete k formátování nový disk, abyste mohli používat.</span><span class="sxs-lookup"><span data-stu-id="e0f5f-140">You get a pop-up that you need to format the new disk before you can use it.</span></span> <span data-ttu-id="e0f5f-141">Klikněte na tlačítko **formát disku**.</span><span class="sxs-lookup"><span data-stu-id="e0f5f-141">Click **Format disk**.</span></span>
8. <span data-ttu-id="e0f5f-142">V **nový disk formát** dialogové okno, zkontrolujte nastavení a pak klikněte na tlačítko **spustit**.</span><span class="sxs-lookup"><span data-stu-id="e0f5f-142">In the **Format new disk** dialog, check the settings and then click **Start**.</span></span>
9. <span data-ttu-id="e0f5f-143">Zobrazí upozornění, že formátování disky vymaže všechna data, klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="e0f5f-143">You get a warning that formatting the disks erases all of the data, click **OK**.</span></span>
10. <span data-ttu-id="e0f5f-144">Po dokončení Formát klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="e0f5f-144">When the format is complete, click **OK**.</span></span>


## <a name="option-2-attach-an-existing-disk"></a><span data-ttu-id="e0f5f-145">Možnost 2: Připojit stávající disk</span><span class="sxs-lookup"><span data-stu-id="e0f5f-145">Option 2: Attach an existing disk</span></span>
1. <span data-ttu-id="e0f5f-146">Na **disky** okně klikněte na tlačítko **+ přidat datový disk**.</span><span class="sxs-lookup"><span data-stu-id="e0f5f-146">On the **Disks** blade, click **+ Add data disk**.</span></span>
2. <span data-ttu-id="e0f5f-147">Na **nespravované disk připojit** okno v **typ zdroje** vyberte **existující objekt blob**.</span><span class="sxs-lookup"><span data-stu-id="e0f5f-147">On the **Attach unmanaged disk** blade, in **Source type** select **Existing blob**.</span></span>

    ![Zkontrolujte nastavení disku](./media/attach-disk-portal/attach-existing-unmanaged.png)

    3. <span data-ttu-id="e0f5f-149">Klikněte na tlačítko **Procházet** přejděte na účet úložiště a kontejneru, kde se nachází existující virtuální pevný disk.</span><span class="sxs-lookup"><span data-stu-id="e0f5f-149">Click **Browse** to navigate to the storage account and container where the existing VHD is located.</span></span> <span data-ttu-id="e0f5f-150">Klikněte na tlačítko a virtuální pevný disk a pak klikněte na tlačítko **vyberte**.</span><span class="sxs-lookup"><span data-stu-id="e0f5f-150">Click and the VHD and then click **Select**.</span></span>
4. <span data-ttu-id="e0f5f-151">Klikněte na tlačítko **OK** v **nespravované disk připojit** okno.</span><span class="sxs-lookup"><span data-stu-id="e0f5f-151">Click **OK** in the **Attach unmanaged disk** blade.</span></span>
5. <span data-ttu-id="e0f5f-152">V **disky** okně klikněte na tlačítko **Uložit** přidání disku do konfigurace pro virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="e0f5f-152">In the **Disks** blade, click **Save** to add the disk to the configuration for the VM.</span></span>
   


## <a name="use-trim-with-standard-storage"></a><span data-ttu-id="e0f5f-153">Použít standardní úložiště uvolnění dočasné paměti</span><span class="sxs-lookup"><span data-stu-id="e0f5f-153">Use TRIM with standard storage</span></span>

<span data-ttu-id="e0f5f-154">Pokud chcete použít standardní úložiště (HDD), měli byste povolit uvolnění dočasné paměti.</span><span class="sxs-lookup"><span data-stu-id="e0f5f-154">If you use standard storage (HDD), you should enable TRIM.</span></span> <span data-ttu-id="e0f5f-155">TRIM zahodí nepoužívané bloky na disku, můžete se účtují pouze pro úložiště, které skutečně používáte.</span><span class="sxs-lookup"><span data-stu-id="e0f5f-155">TRIM discards unused blocks on the disk so you are only billed for storage that you are actually using.</span></span> <span data-ttu-id="e0f5f-156">To můžete uložit na náklady, pokud chcete vytvořit velkých souborů a pak odstraňte je.</span><span class="sxs-lookup"><span data-stu-id="e0f5f-156">This can save on costs if you create large files and then delete them.</span></span> 

<span data-ttu-id="e0f5f-157">Můžete spustit tento příkaz a zkontrolujte nastavení uvolnění dočasné paměti.</span><span class="sxs-lookup"><span data-stu-id="e0f5f-157">You can run this command to check the TRIM setting.</span></span> <span data-ttu-id="e0f5f-158">Otevřete příkazový řádek na virtuální počítač s Windows a zadejte:</span><span class="sxs-lookup"><span data-stu-id="e0f5f-158">Open a command prompt on your Windows VM and type:</span></span>

```
fsutil behavior query DisableDeleteNotify
```

<span data-ttu-id="e0f5f-159">Pokud příkaz vrátí hodnotu 0, TRIM správně povolené.</span><span class="sxs-lookup"><span data-stu-id="e0f5f-159">If the command returns 0, TRIM is enabled correctly.</span></span> <span data-ttu-id="e0f5f-160">Pokud vrátí hodnotu 1, spusťte následující příkaz k povolení uvolnění dočasné paměti:</span><span class="sxs-lookup"><span data-stu-id="e0f5f-160">If it returns 1, run the following command to enable TRIM:</span></span>
```
fsutil behavior set DisableDeleteNotify 0
```

<span data-ttu-id="e0f5f-161">Po odstranění dat z disku, můžete zajistit TRIM operace vyprázdnění správně spuštěním defragmentační s uvolnění dočasné paměti:</span><span class="sxs-lookup"><span data-stu-id="e0f5f-161">After deleting data from your disk, you can ensure the TRIM operations flush properly by running defrag with TRIM:</span></span>

```
defrag.exe <volume:> -l
```

<span data-ttu-id="e0f5f-162">Můžete také zajistit, že celý svazek je oříznut podle formátování svazku.</span><span class="sxs-lookup"><span data-stu-id="e0f5f-162">You can also ensure the entire volume is trimmed by formatting the volume.</span></span>


## <a name="next-steps"></a><span data-ttu-id="e0f5f-163">Další kroky</span><span class="sxs-lookup"><span data-stu-id="e0f5f-163">Next steps</span></span>
<span data-ttu-id="e0f5f-164">Pokud jste aplikaci potřebuje používat D: disk k uložení dat, můžete [změnit písmeno jednotky dočasné disk systému Windows](change-drive-letter.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="e0f5f-164">If you application needs to use the D: drive to store data, you can [change the drive letter of the Windows temporary disk](change-drive-letter.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json).</span></span>

