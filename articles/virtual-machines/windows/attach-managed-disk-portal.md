---
title: "aaaAttach spravovaných dat disku tooa virtuální počítač s Windows - Azure | Microsoft Docs"
description: "Způsob správy dat disku tooa tooattach nový virtuální počítač s Windows v Azure pomocí portálu hello hello modelu nasazení Resource Manager."
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
ms.openlocfilehash: bacc0589ad2d93e4d3d055c8f837f8db27291ead
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="how-tooattach-a-managed-data-disk-tooa-windows-vm-in-hello-azure-portal"></a><span data-ttu-id="4298c-103">Jak tooattach spravovaná data na disku tooa virtuální počítač s Windows v hello portálu Azure</span><span class="sxs-lookup"><span data-stu-id="4298c-103">How tooattach a managed data disk tooa Windows VM in hello Azure portal</span></span>

<span data-ttu-id="4298c-104">Tento článek ukazuje, jak tooattach nová data spravovaného disku tooWindows virtuálním počítačům prostřednictvím hello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="4298c-104">This article shows you how tooattach a new managed data disk tooWindows virtual machines through hello Azure portal.</span></span> <span data-ttu-id="4298c-105">Než to uděláte, projděte si tyto tipy:</span><span class="sxs-lookup"><span data-stu-id="4298c-105">Before you do this, review these tips:</span></span>

* <span data-ttu-id="4298c-106">velikost Hello hello virtuálního počítače určuje, kolik datových disků můžete připojit.</span><span class="sxs-lookup"><span data-stu-id="4298c-106">hello size of hello virtual machine controls how many data disks you can attach.</span></span> <span data-ttu-id="4298c-107">Podrobnosti najdete v tématu [velikosti virtuálních počítačů](sizes.md).</span><span class="sxs-lookup"><span data-stu-id="4298c-107">For details, see [Sizes for virtual machines](sizes.md).</span></span>
* <span data-ttu-id="4298c-108">Pro nový disk, nepotřebujete toocreate ho první protože Azure ji vytvoří, když jeho připojení.</span><span class="sxs-lookup"><span data-stu-id="4298c-108">For a new disk, you don't need toocreate it first because Azure creates it when you attach it.</span></span>

<span data-ttu-id="4298c-109">Můžete také [připojit datový disk pomocí prostředí Powershell](attach-disk-ps.md).</span><span class="sxs-lookup"><span data-stu-id="4298c-109">You can also [attach a data disk using Powershell](attach-disk-ps.md).</span></span>



## <a name="add-a-data-disk"></a><span data-ttu-id="4298c-110">Přidat datový disk</span><span class="sxs-lookup"><span data-stu-id="4298c-110">Add a data disk</span></span>
1. <span data-ttu-id="4298c-111">V nabídce hello hello vlevo, klikněte na **virtuální počítače**.</span><span class="sxs-lookup"><span data-stu-id="4298c-111">In hello menu on hello left, click **Virtual Machines**.</span></span>
2. <span data-ttu-id="4298c-112">Vyberte virtuální počítač hello hello seznamu.</span><span class="sxs-lookup"><span data-stu-id="4298c-112">Select hello virtual machine from hello list.</span></span>
3. <span data-ttu-id="4298c-113">V okně hello virtuální počítač, klikněte na tlačítko **disky**.</span><span class="sxs-lookup"><span data-stu-id="4298c-113">On hello virtual machine blade, click **Disks**.</span></span>
   4. <span data-ttu-id="4298c-114">Na hello **disky** okně klikněte na tlačítko **+ přidat datový disk**.</span><span class="sxs-lookup"><span data-stu-id="4298c-114">On hello **Disks** blade, click **+ Add data disk**.</span></span>
5. <span data-ttu-id="4298c-115">V hello rozevírací seznam hello nový disk, vyberte **vytvořit prázdný**.</span><span class="sxs-lookup"><span data-stu-id="4298c-115">In hello drop-down for hello new disk, select **Create empty**.</span></span>
6. <span data-ttu-id="4298c-116">V hello **spravovaných disků na vytvořit** okno, zadejte název pro hello disk a upravte hello další nastavení podle potřeby.</span><span class="sxs-lookup"><span data-stu-id="4298c-116">In hello **Create managed disk** blade, type in a name for hello disk and adjust hello other settings as necessary.</span></span> <span data-ttu-id="4298c-117">Až budete hotovi, klikněte na tlačítko **vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="4298c-117">When you are done, click **Create**.</span></span>
7. <span data-ttu-id="4298c-118">V hello **disky** okně klikněte na tlačítko Uložit toosave hello nové konfigurace disku hello virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="4298c-118">In hello **Disks** blade, click save toosave hello new disk configuration for hello VM.</span></span>
6. <span data-ttu-id="4298c-119">Jakmile Azure vytvoří hello disk a připojí jej toohello virtuálního počítače, hello nového disku je uvedena v nastavení disku hello virtuálního počítače v části **datových disků**.</span><span class="sxs-lookup"><span data-stu-id="4298c-119">After Azure creates hello disk and attaches it toohello virtual machine, hello new disk is listed in hello virtual machine's disk settings under **Data Disks**.</span></span>


## <a name="initialize-a-new-data-disk"></a><span data-ttu-id="4298c-120">Inicializace nový datový disk</span><span class="sxs-lookup"><span data-stu-id="4298c-120">Initialize a new data disk</span></span>

1. <span data-ttu-id="4298c-121">Připojte toohello virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="4298c-121">Connect toohello VM.</span></span>
1. <span data-ttu-id="4298c-122">Klikněte na položku nabídky start hello uvnitř hello virtuálních počítačů a typ **diskmgmt.msc** a počtu **Enter**.</span><span class="sxs-lookup"><span data-stu-id="4298c-122">Click hello start menu inside hello VM and type **diskmgmt.msc** and hit **Enter**.</span></span> <span data-ttu-id="4298c-123">Tím se spustí modul snap-in Správa disků hello.</span><span class="sxs-lookup"><span data-stu-id="4298c-123">This will start hello Disk Management snap-in.</span></span>
2. <span data-ttu-id="4298c-124">Správa disků rozpozná, že máte nový, zrušení inicializovaného disku a bude překryvné okno inicializovat Disk hello.</span><span class="sxs-lookup"><span data-stu-id="4298c-124">Disk Management will recognize that you have a new, un-initialized disk and hello Initialize Disk window will pop up.</span></span>
3. <span data-ttu-id="4298c-125">Musí být vybrána hello nový disk a klikněte na tlačítko **OK** tooinitialize ho.</span><span class="sxs-lookup"><span data-stu-id="4298c-125">Make sure hello new disk is selected and click **OK** tooinitialize it.</span></span>
4. <span data-ttu-id="4298c-126">nový disk Hello se nyní zobrazí jako **nepřidělené**.</span><span class="sxs-lookup"><span data-stu-id="4298c-126">hello new disk will now appear as **unallocated**.</span></span> <span data-ttu-id="4298c-127">Klikněte pravým tlačítkem na libovolné místo na disku hello a vyberte **nový jednoduchý svazek**.</span><span class="sxs-lookup"><span data-stu-id="4298c-127">Right-click anywhere on hello disk and select **New simple volume**.</span></span> <span data-ttu-id="4298c-128">Hello **Průvodci vytvořením jednoduchého svazku** spustí.</span><span class="sxs-lookup"><span data-stu-id="4298c-128">hello **New Simple Volume Wizard** will start.</span></span>
5. <span data-ttu-id="4298c-129">Projděte hello průvodce, udržuje všechny výchozí hodnoty hello, po dokončení vyberte **Dokončit**.</span><span class="sxs-lookup"><span data-stu-id="4298c-129">Go through hello wizard, keeping all of hello defaults, when you are done select **Finish**.</span></span>
6. <span data-ttu-id="4298c-130">Zavřete Správa disků.</span><span class="sxs-lookup"><span data-stu-id="4298c-130">Close Disk Management.</span></span>
7. <span data-ttu-id="4298c-131">Zobrazí se automaticky otevíraného okna, je nutné tooformat hello nový disk, abyste mohli používat.</span><span class="sxs-lookup"><span data-stu-id="4298c-131">You will get a pop-up that you need tooformat hello new disk before you can use it.</span></span> <span data-ttu-id="4298c-132">Klikněte na tlačítko **formát disku**.</span><span class="sxs-lookup"><span data-stu-id="4298c-132">Click **Format disk**.</span></span>
8. <span data-ttu-id="4298c-133">V hello **nový disk formát** dialogové okno, zkontrolujte hello nastavení a pak klikněte na tlačítko **spustit**.</span><span class="sxs-lookup"><span data-stu-id="4298c-133">In hello **Format new disk** dialog, check hello settings and then click **Start**.</span></span>
9. <span data-ttu-id="4298c-134">Zobrazí se upozornění, že formátování hello disky vymaže všechna hello data, klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="4298c-134">You will get a warning that formatting hello disks will erase all of hello data, click **OK**.</span></span>
10. <span data-ttu-id="4298c-135">Po dokončení hello Formát klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="4298c-135">When hello format is complete, click **OK**.</span></span>

## <a name="use-trim-with-standard-storage"></a><span data-ttu-id="4298c-136">Použít standardní úložiště uvolnění dočasné paměti</span><span class="sxs-lookup"><span data-stu-id="4298c-136">Use TRIM with standard storage</span></span>

<span data-ttu-id="4298c-137">Pokud chcete použít standardní úložiště (HDD), měli byste povolit uvolnění dočasné paměti.</span><span class="sxs-lookup"><span data-stu-id="4298c-137">If you use standard storage (HDD), you should enable TRIM.</span></span> <span data-ttu-id="4298c-138">TRIM zahodí nepoužívané bloky na disku hello tak fakturuje se pouze pro úložiště, které skutečně používáte.</span><span class="sxs-lookup"><span data-stu-id="4298c-138">TRIM discards unused blocks on hello disk so you are only billed for storage that you are actually using.</span></span> <span data-ttu-id="4298c-139">To můžete uložit na náklady, pokud chcete vytvořit velkých souborů a pak odstraňte je.</span><span class="sxs-lookup"><span data-stu-id="4298c-139">This can save on costs if you create large files and then delete them.</span></span> 

<span data-ttu-id="4298c-140">Můžete spustit toto OŘÍZNUTÍ nastavení příkaz toocheck hello.</span><span class="sxs-lookup"><span data-stu-id="4298c-140">You can run this command toocheck hello TRIM setting.</span></span> <span data-ttu-id="4298c-141">Otevřete příkazový řádek na virtuální počítač s Windows a zadejte:</span><span class="sxs-lookup"><span data-stu-id="4298c-141">Open a command prompt on your Windows VM and type:</span></span>

```
fsutil behavior query DisableDeleteNotify
```

<span data-ttu-id="4298c-142">Pokud příkaz hello vrátí hodnotu 0, TRIM povolený správně.</span><span class="sxs-lookup"><span data-stu-id="4298c-142">If hello command returns 0, TRIM is enabled correctly.</span></span> <span data-ttu-id="4298c-143">Pokud vrátí hodnotu 1, spusťte následující příkaz tooenable TRIM hello:</span><span class="sxs-lookup"><span data-stu-id="4298c-143">If it returns 1, run hello following command tooenable TRIM:</span></span>
```
fsutil behavior set DisableDeleteNotify 0
```

<span data-ttu-id="4298c-144">Po odstranění dat z disku, můžete zajistit, že hello TRIM operace vyprázdnění správně spuštěním defragmentační s uvolnění dočasné paměti:</span><span class="sxs-lookup"><span data-stu-id="4298c-144">After deleting data from your disk, you can ensure hello TRIM operations flush properly by running defrag with TRIM:</span></span>

```
defrag.exe <volume:> -l
```

<span data-ttu-id="4298c-145">Můžete také zajistit, že hello celý svazek je oříznut podle formátování svazku hello.</span><span class="sxs-lookup"><span data-stu-id="4298c-145">You can also ensure hello entire volume is trimmed by formatting hello volume.</span></span>

## <a name="next-steps"></a><span data-ttu-id="4298c-146">Další kroky</span><span class="sxs-lookup"><span data-stu-id="4298c-146">Next steps</span></span>
<span data-ttu-id="4298c-147">Pokud je aplikace potřebuje toouse hello D: jednotky toostore data, můžete [změnit písmeno jednotky hello dočasné disk systému Windows hello](change-drive-letter.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="4298c-147">If you application needs toouse hello D: drive toostore data, you can [change hello drive letter of hello Windows temporary disk](change-drive-letter.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json).</span></span>
