---
title: "Připojit spravované datový disk k virtuálnímu počítači Windows - Azure | Microsoft Docs"
description: "Jak připojit nový spravovaný datový disk pro virtuální počítač s Windows v portálu Azure pomocí modelu nasazení Resource Manager."
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
ms.date: 05/09/2017
ms.author: cynthn
ms.openlocfilehash: f0cf88a06c5470ef173b22e7213419a6c8760723
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="how-to-attach-a-managed-data-disk-to-a-windows-vm-in-the-azure-portal"></a><span data-ttu-id="986c7-103">Tom, jak připojit spravované datový disk k virtuálnímu počítači Windows na portálu Azure</span><span class="sxs-lookup"><span data-stu-id="986c7-103">How to attach a managed data disk to a Windows VM in the Azure portal</span></span>

<span data-ttu-id="986c7-104">Tento článek ukazuje, jak připojit nový spravovaný datový disk pro virtuální počítače s Windows pomocí portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="986c7-104">This article shows you how to attach a new managed data disk to Windows virtual machines through the Azure portal.</span></span> <span data-ttu-id="986c7-105">Než to uděláte, projděte si tyto tipy:</span><span class="sxs-lookup"><span data-stu-id="986c7-105">Before you do this, review these tips:</span></span>

* <span data-ttu-id="986c7-106">Velikost virtuálního počítače určuje, kolik datových disků můžete připojit.</span><span class="sxs-lookup"><span data-stu-id="986c7-106">The size of the virtual machine controls how many data disks you can attach.</span></span> <span data-ttu-id="986c7-107">Podrobnosti najdete v tématu [velikosti virtuálních počítačů](sizes.md).</span><span class="sxs-lookup"><span data-stu-id="986c7-107">For details, see [Sizes for virtual machines](sizes.md).</span></span>
* <span data-ttu-id="986c7-108">Pro nový disk nemusíte nejdřív ji vytvořit, protože Azure ji vytvoří, když jeho připojení.</span><span class="sxs-lookup"><span data-stu-id="986c7-108">For a new disk, you don't need to create it first because Azure creates it when you attach it.</span></span>

<span data-ttu-id="986c7-109">Můžete také [připojit datový disk pomocí prostředí Powershell](attach-disk-ps.md).</span><span class="sxs-lookup"><span data-stu-id="986c7-109">You can also [attach a data disk using Powershell](attach-disk-ps.md).</span></span>



## <a name="add-a-data-disk"></a><span data-ttu-id="986c7-110">Přidat datový disk</span><span class="sxs-lookup"><span data-stu-id="986c7-110">Add a data disk</span></span>
1. <span data-ttu-id="986c7-111">V nabídce na levé straně klikněte na tlačítko **virtuální počítače**.</span><span class="sxs-lookup"><span data-stu-id="986c7-111">In the menu on the left, click **Virtual Machines**.</span></span>
2. <span data-ttu-id="986c7-112">Ze seznamu vyberte virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="986c7-112">Select the virtual machine from the list.</span></span>
3. <span data-ttu-id="986c7-113">V okně virtuálního počítače klikněte na **disky**.</span><span class="sxs-lookup"><span data-stu-id="986c7-113">On the virtual machine blade, click **Disks**.</span></span>
   4. <span data-ttu-id="986c7-114">Na **disky** okně klikněte na tlačítko **+ přidat datový disk**.</span><span class="sxs-lookup"><span data-stu-id="986c7-114">On the **Disks** blade, click **+ Add data disk**.</span></span>
5. <span data-ttu-id="986c7-115">V rozevírací nabídky pro nový disk, vyberte **vytvořit prázdný**.</span><span class="sxs-lookup"><span data-stu-id="986c7-115">In the drop-down for the new disk, select **Create empty**.</span></span>
6. <span data-ttu-id="986c7-116">V **spravovaných disků na vytvořit** okno, zadejte název pro disk a upravte nastavení podle potřeby.</span><span class="sxs-lookup"><span data-stu-id="986c7-116">In the **Create managed disk** blade, type in a name for the disk and adjust the other settings as necessary.</span></span> <span data-ttu-id="986c7-117">Až budete hotovi, klikněte na tlačítko **vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="986c7-117">When you are done, click **Create**.</span></span>
7. <span data-ttu-id="986c7-118">V **disky** okně klikněte na tlačítko Uložit uložte novou konfiguraci disku pro virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="986c7-118">In the **Disks** blade, click save to save the new disk configuration for the VM.</span></span>
6. <span data-ttu-id="986c7-119">Jakmile Azure vytvoří disk a připojí jej k virtuálnímu počítači, na nový disk, je uvedena ve nastavení disku virtuálního počítače v části **datových disků**.</span><span class="sxs-lookup"><span data-stu-id="986c7-119">After Azure creates the disk and attaches it to the virtual machine, the new disk is listed in the virtual machine's disk settings under **Data Disks**.</span></span>


## <a name="initialize-a-new-data-disk"></a><span data-ttu-id="986c7-120">Inicializace nový datový disk</span><span class="sxs-lookup"><span data-stu-id="986c7-120">Initialize a new data disk</span></span>

1. <span data-ttu-id="986c7-121">Připojte k virtuálnímu počítači.</span><span class="sxs-lookup"><span data-stu-id="986c7-121">Connect to the VM.</span></span>
1. <span data-ttu-id="986c7-122">Klikněte na nabídku start uvnitř virtuálního počítače a typ **diskmgmt.msc** a počtu **Enter**.</span><span class="sxs-lookup"><span data-stu-id="986c7-122">Click the start menu inside the VM and type **diskmgmt.msc** and hit **Enter**.</span></span> <span data-ttu-id="986c7-123">Tím se spustí modul snap-in Správa disků.</span><span class="sxs-lookup"><span data-stu-id="986c7-123">This will start the Disk Management snap-in.</span></span>
2. <span data-ttu-id="986c7-124">Správa disků rozpozná, že máte nový, zrušení inicializovaného disku a bude překryvné okno inicializovat Disk.</span><span class="sxs-lookup"><span data-stu-id="986c7-124">Disk Management will recognize that you have a new, un-initialized disk and the Initialize Disk window will pop up.</span></span>
3. <span data-ttu-id="986c7-125">Zkontrolujte, že je vybraný nový disk a klikněte na tlačítko **OK** k chybě při inicializaci ho.</span><span class="sxs-lookup"><span data-stu-id="986c7-125">Make sure the new disk is selected and click **OK** to initialize it.</span></span>
4. <span data-ttu-id="986c7-126">Nový disk se nyní zobrazí jako **nepřidělené**.</span><span class="sxs-lookup"><span data-stu-id="986c7-126">The new disk will now appear as **unallocated**.</span></span> <span data-ttu-id="986c7-127">Klepněte pravým tlačítkem myši na disk a vyberte **nový jednoduchý svazek**.</span><span class="sxs-lookup"><span data-stu-id="986c7-127">Right-click anywhere on the disk and select **New simple volume**.</span></span> <span data-ttu-id="986c7-128">**Průvodci vytvořením jednoduchého svazku** spustí.</span><span class="sxs-lookup"><span data-stu-id="986c7-128">The **New Simple Volume Wizard** will start.</span></span>
5. <span data-ttu-id="986c7-129">Postupujte dle pokynů průvodce, udržuje všechny výchozí hodnoty, po dokončení vyberte **Dokončit**.</span><span class="sxs-lookup"><span data-stu-id="986c7-129">Go through the wizard, keeping all of the defaults, when you are done select **Finish**.</span></span>
6. <span data-ttu-id="986c7-130">Zavřete Správa disků.</span><span class="sxs-lookup"><span data-stu-id="986c7-130">Close Disk Management.</span></span>
7. <span data-ttu-id="986c7-131">Zobrazí se automaticky otevírané okno, které potřebujete k formátování nový disk, abyste mohli používat.</span><span class="sxs-lookup"><span data-stu-id="986c7-131">You will get a pop-up that you need to format the new disk before you can use it.</span></span> <span data-ttu-id="986c7-132">Klikněte na tlačítko **formát disku**.</span><span class="sxs-lookup"><span data-stu-id="986c7-132">Click **Format disk**.</span></span>
8. <span data-ttu-id="986c7-133">V **nový disk formát** dialogové okno, zkontrolujte nastavení a pak klikněte na tlačítko **spustit**.</span><span class="sxs-lookup"><span data-stu-id="986c7-133">In the **Format new disk** dialog, check the settings and then click **Start**.</span></span>
9. <span data-ttu-id="986c7-134">Zobrazí se upozornění, které se formátování disky bude vymazat všechna data, klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="986c7-134">You will get a warning that formatting the disks will erase all of the data, click **OK**.</span></span>
10. <span data-ttu-id="986c7-135">Po dokončení Formát klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="986c7-135">When the format is complete, click **OK**.</span></span>

## <a name="use-trim-with-standard-storage"></a><span data-ttu-id="986c7-136">Použít standardní úložiště uvolnění dočasné paměti</span><span class="sxs-lookup"><span data-stu-id="986c7-136">Use TRIM with standard storage</span></span>

<span data-ttu-id="986c7-137">Pokud chcete použít standardní úložiště (HDD), měli byste povolit uvolnění dočasné paměti.</span><span class="sxs-lookup"><span data-stu-id="986c7-137">If you use standard storage (HDD), you should enable TRIM.</span></span> <span data-ttu-id="986c7-138">TRIM zahodí nepoužívané bloky na disku, můžete se účtují pouze pro úložiště, které skutečně používáte.</span><span class="sxs-lookup"><span data-stu-id="986c7-138">TRIM discards unused blocks on the disk so you are only billed for storage that you are actually using.</span></span> <span data-ttu-id="986c7-139">To můžete uložit na náklady, pokud chcete vytvořit velkých souborů a pak odstraňte je.</span><span class="sxs-lookup"><span data-stu-id="986c7-139">This can save on costs if you create large files and then delete them.</span></span> 

<span data-ttu-id="986c7-140">Můžete spustit tento příkaz a zkontrolujte nastavení uvolnění dočasné paměti.</span><span class="sxs-lookup"><span data-stu-id="986c7-140">You can run this command to check the TRIM setting.</span></span> <span data-ttu-id="986c7-141">Otevřete příkazový řádek na virtuální počítač s Windows a zadejte:</span><span class="sxs-lookup"><span data-stu-id="986c7-141">Open a command prompt on your Windows VM and type:</span></span>

```
fsutil behavior query DisableDeleteNotify
```

<span data-ttu-id="986c7-142">Pokud příkaz vrátí hodnotu 0, TRIM správně povolené.</span><span class="sxs-lookup"><span data-stu-id="986c7-142">If the command returns 0, TRIM is enabled correctly.</span></span> <span data-ttu-id="986c7-143">Pokud vrátí hodnotu 1, spusťte následující příkaz k povolení uvolnění dočasné paměti:</span><span class="sxs-lookup"><span data-stu-id="986c7-143">If it returns 1, run the following command to enable TRIM:</span></span>
```
fsutil behavior set DisableDeleteNotify 0
```

<span data-ttu-id="986c7-144">Po odstranění dat z disku, můžete zajistit TRIM operace vyprázdnění správně spuštěním defragmentační s uvolnění dočasné paměti:</span><span class="sxs-lookup"><span data-stu-id="986c7-144">After deleting data from your disk, you can ensure the TRIM operations flush properly by running defrag with TRIM:</span></span>

```
defrag.exe <volume:> -l
```

<span data-ttu-id="986c7-145">Můžete také zajistit, že celý svazek je oříznut podle formátování svazku.</span><span class="sxs-lookup"><span data-stu-id="986c7-145">You can also ensure the entire volume is trimmed by formatting the volume.</span></span>

## <a name="next-steps"></a><span data-ttu-id="986c7-146">Další kroky</span><span class="sxs-lookup"><span data-stu-id="986c7-146">Next steps</span></span>
<span data-ttu-id="986c7-147">Pokud jste aplikaci potřebuje používat D: disk k uložení dat, můžete [změnit písmeno jednotky dočasné disk systému Windows](change-drive-letter.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="986c7-147">If you application needs to use the D: drive to store data, you can [change the drive letter of the Windows temporary disk](change-drive-letter.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json).</span></span>
