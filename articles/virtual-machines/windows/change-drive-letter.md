---
title: "Vytvořit jednotku D: datový disk virtuálního počítače | Microsoft Docs"
description: "Popisuje, jak změnit písmena jednotek pro virtuální počítač s Windows, tak, aby D: jednotku můžete používat jako datový disk."
services: virtual-machines-windows
documentationcenter: 
author: cynthn
manager: timlt
editor: 
tags: azure-resource-manager,azure-service-management
ms.assetid: 0867a931-0055-4e31-8403-9b38a3eeb904
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 05/31/2017
ms.author: cynthn
ms.openlocfilehash: 7667175c01be2421bfc3badd83b1d8aaeb29bfde
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="use-the-d-drive-as-a-data-drive-on-a-windows-vm"></a><span data-ttu-id="d9030-103">Jako datový disk pro virtuální počítač Windows jednotku D:</span><span class="sxs-lookup"><span data-stu-id="d9030-103">Use the D: drive as a data drive on a Windows VM</span></span>
<span data-ttu-id="d9030-104">Pokud aplikace potřebuje využívat diskovou jednotku d k ukládání dat, postupujte podle těchto pokynů můžete použít jiné písmeno jednotky pro dočasné disku.</span><span class="sxs-lookup"><span data-stu-id="d9030-104">If your application needs to use the D drive to store data, follow these instructions to use a different drive letter for the temporary disk.</span></span> <span data-ttu-id="d9030-105">Nikdy nepoužívejte dočasným diskovým k ukládání dat, který je třeba zachovat.</span><span class="sxs-lookup"><span data-stu-id="d9030-105">Never use the temporary disk to store data that you need to keep.</span></span>

<span data-ttu-id="d9030-106">Při změně velikosti nebo **zastavení (Deallocate)** virtuální počítač, to může aktivovat umístění virtuálního počítače do nového hypervisoru.</span><span class="sxs-lookup"><span data-stu-id="d9030-106">If you resize or **Stop (Deallocate)** a virtual machine, this may trigger placement of the virtual machine to a new hypervisor.</span></span> <span data-ttu-id="d9030-107">Události plánované i neplánované údržby, může se spustit toto umístění.</span><span class="sxs-lookup"><span data-stu-id="d9030-107">A planned or unplanned maintenance event may also trigger this placement.</span></span> <span data-ttu-id="d9030-108">V tomto scénáři bude přiřazen dočasným diskovým první dostupné písmeno jednotky.</span><span class="sxs-lookup"><span data-stu-id="d9030-108">In this scenario, the temporary disk will be reassigned to the first available drive letter.</span></span> <span data-ttu-id="d9030-109">Pokud máte aplikaci, která vyžaduje konkrétně jednotky D:, budete muset následujícím postupem dočasně přesunout pagefile.sys, připojit nový datový disk a přiřaďte ho písmeno D a poté přesuňte pagefile.sys zpět na dočasné jednotce.</span><span class="sxs-lookup"><span data-stu-id="d9030-109">If you have an application that specifically requires the D: drive, you need to follow these steps to temporarily move the pagefile.sys, attach a new data disk and assign it the letter D and then move the pagefile.sys back to the temporary drive.</span></span> <span data-ttu-id="d9030-110">Po dokončení Azure nebude zpět D: Pokud virtuální počítač přesune do různých hypervisoru.</span><span class="sxs-lookup"><span data-stu-id="d9030-110">Once complete, Azure will not take back the D: if the VM moves to a different hypervisor.</span></span>

<span data-ttu-id="d9030-111">Další informace o tom, jak Azure používá dočasným diskovým najdete v tématu [pochopení dočasné jednotce ve virtuálních počítačích Microsoft Azure](https://blogs.msdn.microsoft.com/mast/2013/12/06/understanding-the-temporary-drive-on-windows-azure-virtual-machines/)</span><span class="sxs-lookup"><span data-stu-id="d9030-111">For more information about how Azure uses the temporary disk, see [Understanding the temporary drive on Microsoft Azure Virtual Machines](https://blogs.msdn.microsoft.com/mast/2013/12/06/understanding-the-temporary-drive-on-windows-azure-virtual-machines/)</span></span>

## <a name="attach-the-data-disk"></a><span data-ttu-id="d9030-112">Připojit datový disk</span><span class="sxs-lookup"><span data-stu-id="d9030-112">Attach the data disk</span></span>
<span data-ttu-id="d9030-113">Nejprve budete muset připojit datový disk k virtuálnímu počítači.</span><span class="sxs-lookup"><span data-stu-id="d9030-113">First, you'll need to attach the data disk to the virtual machine.</span></span> <span data-ttu-id="d9030-114">To provedete pomocí portálu najdete v části [jak připojit spravované datový disk na portálu Azure](attach-managed-disk-portal.md).</span><span class="sxs-lookup"><span data-stu-id="d9030-114">To do this using the portal, see [How to attach a managed data disk in the Azure portal](attach-managed-disk-portal.md).</span></span>

## <a name="temporarily-move-pagefilesys-to-c-drive"></a><span data-ttu-id="d9030-115">Dočasně přesunout pagefile.sys na jednotce C</span><span class="sxs-lookup"><span data-stu-id="d9030-115">Temporarily move pagefile.sys to C drive</span></span>
1. <span data-ttu-id="d9030-116">Připojte k virtuálnímu počítači.</span><span class="sxs-lookup"><span data-stu-id="d9030-116">Connect to the virtual machine.</span></span> 
2. <span data-ttu-id="d9030-117">Klikněte pravým tlačítkem myši **spustit** nabídku a vyberte **systému**.</span><span class="sxs-lookup"><span data-stu-id="d9030-117">Right-click the **Start** menu and select **System**.</span></span>
3. <span data-ttu-id="d9030-118">V levé nabídce vyberte **Upřesnit nastavení systému**.</span><span class="sxs-lookup"><span data-stu-id="d9030-118">In the left-hand menu, select **Advanced system settings**.</span></span>
4. <span data-ttu-id="d9030-119">V **výkonu** vyberte **nastavení**.</span><span class="sxs-lookup"><span data-stu-id="d9030-119">In the **Performance** section, select **Settings**.</span></span>
5. <span data-ttu-id="d9030-120">Vyberte **Upřesnit** kartě.</span><span class="sxs-lookup"><span data-stu-id="d9030-120">Select the **Advanced** tab.</span></span>
6. <span data-ttu-id="d9030-121">V **virtuální paměti** vyberte **změnu**.</span><span class="sxs-lookup"><span data-stu-id="d9030-121">In the **Virtual memory** section, select **Change**.</span></span>
7. <span data-ttu-id="d9030-122">Vyberte **C** jednotky a pak klikněte na **velikost určí systém** a pak klikněte na **nastavit**.</span><span class="sxs-lookup"><span data-stu-id="d9030-122">Select the **C** drive and then click **System managed size** and then click **Set**.</span></span>
8. <span data-ttu-id="d9030-123">Vyberte **D** jednotky a pak klikněte na **stránkovací soubor** a pak klikněte na **nastavit**.</span><span class="sxs-lookup"><span data-stu-id="d9030-123">Select the **D** drive and then click **No paging file** and then click **Set**.</span></span>
9. <span data-ttu-id="d9030-124">Kliknutím na tlačítko použít.</span><span class="sxs-lookup"><span data-stu-id="d9030-124">Click Apply.</span></span> <span data-ttu-id="d9030-125">Zobrazí se upozornění, že počítač je třeba restartovat změny se projeví.</span><span class="sxs-lookup"><span data-stu-id="d9030-125">You will get a warning that the computer needs to be restarted for the changes to take affect.</span></span>
10. <span data-ttu-id="d9030-126">Restartujte virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="d9030-126">Restart the virtual machine.</span></span>

## <a name="change-the-drive-letters"></a><span data-ttu-id="d9030-127">Změna písmena jednotek</span><span class="sxs-lookup"><span data-stu-id="d9030-127">Change the drive letters</span></span>
1. <span data-ttu-id="d9030-128">Po restartování virtuálního počítače se přihlásíte do virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="d9030-128">Once the VM restarts, log back on to the VM.</span></span>
2. <span data-ttu-id="d9030-129">Klikněte **spustit** nabídce a typ **diskmgmt.msc** a stiskněte Enter.</span><span class="sxs-lookup"><span data-stu-id="d9030-129">Click the **Start** menu and type **diskmgmt.msc** and hit Enter.</span></span> <span data-ttu-id="d9030-130">Správa disků se spustí.</span><span class="sxs-lookup"><span data-stu-id="d9030-130">Disk Management will start.</span></span>
3. <span data-ttu-id="d9030-131">Klikněte pravým tlačítkem na **D**, dočasné úložiště jednotky a vyberte **změnit písmeno jednotky a cesty**.</span><span class="sxs-lookup"><span data-stu-id="d9030-131">Right-click on **D**, the Temporary Storage drive, and select **Change Drive Letter and Paths**.</span></span>
4. <span data-ttu-id="d9030-132">V části písmeno jednotky, vyberte na nový disk **T** a pak klikněte na **OK**.</span><span class="sxs-lookup"><span data-stu-id="d9030-132">Under Drive letter, select a new drive such as **T** and then click **OK**.</span></span> 
5. <span data-ttu-id="d9030-133">Klikněte pravým tlačítkem na datový disk a vyberte **změnit písmeno jednotky a cesty**.</span><span class="sxs-lookup"><span data-stu-id="d9030-133">Right-click on the data disk, and select **Change Drive Letter and Paths**.</span></span>
6. <span data-ttu-id="d9030-134">V části písmeno jednotky, vyberte jednotky **D** a pak klikněte na **OK**.</span><span class="sxs-lookup"><span data-stu-id="d9030-134">Under Drive letter, select drive **D** and then click **OK**.</span></span> 

## <a name="move-pagefilesys-back-to-the-temporary-storage-drive"></a><span data-ttu-id="d9030-135">Přesunout pagefile.sys zpět na jednotku dočasné úložiště</span><span class="sxs-lookup"><span data-stu-id="d9030-135">Move pagefile.sys back to the temporary storage drive</span></span>
1. <span data-ttu-id="d9030-136">Klikněte pravým tlačítkem myši **spustit** nabídku a vyberte **systému**</span><span class="sxs-lookup"><span data-stu-id="d9030-136">Right-click the **Start** menu and select **System**</span></span>
2. <span data-ttu-id="d9030-137">V levé nabídce vyberte **Upřesnit nastavení systému**.</span><span class="sxs-lookup"><span data-stu-id="d9030-137">In the left-hand menu, select **Advanced system settings**.</span></span>
3. <span data-ttu-id="d9030-138">V **výkonu** vyberte **nastavení**.</span><span class="sxs-lookup"><span data-stu-id="d9030-138">In the **Performance** section, select **Settings**.</span></span>
4. <span data-ttu-id="d9030-139">Vyberte **Upřesnit** kartě.</span><span class="sxs-lookup"><span data-stu-id="d9030-139">Select the **Advanced** tab.</span></span>
5. <span data-ttu-id="d9030-140">V **virtuální paměti** vyberte **změnu**.</span><span class="sxs-lookup"><span data-stu-id="d9030-140">In the **Virtual memory** section, select **Change**.</span></span>
6. <span data-ttu-id="d9030-141">Vyberte jednotku operačního systému **C** a klikněte na tlačítko **stránkovací soubor** a pak klikněte na **nastavit**.</span><span class="sxs-lookup"><span data-stu-id="d9030-141">Select the OS drive **C** and click **No paging file** and then click **Set**.</span></span>
7. <span data-ttu-id="d9030-142">Vyberte jednotku dočasné úložiště **T** a pak klikněte na **velikost určí systém** a pak klikněte na **nastavit**.</span><span class="sxs-lookup"><span data-stu-id="d9030-142">Select the temporary storage drive **T** and then click **System managed size** and then click **Set**.</span></span>
8. <span data-ttu-id="d9030-143">Klikněte na tlačítko **Použít**.</span><span class="sxs-lookup"><span data-stu-id="d9030-143">Click **Apply**.</span></span> <span data-ttu-id="d9030-144">Zobrazí se upozornění, že počítač je třeba restartovat změny se projeví.</span><span class="sxs-lookup"><span data-stu-id="d9030-144">You will get a warning that the computer needs to be restarted for the changes to take affect.</span></span>
9. <span data-ttu-id="d9030-145">Restartujte virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="d9030-145">Restart the virtual machine.</span></span>

## <a name="next-steps"></a><span data-ttu-id="d9030-146">Další kroky</span><span class="sxs-lookup"><span data-stu-id="d9030-146">Next steps</span></span>
* <span data-ttu-id="d9030-147">Můžete zvýšit úložiště k dispozici k virtuálnímu počítači pomocí [další datový disk se připojuje](attach-managed-disk-portal.md).</span><span class="sxs-lookup"><span data-stu-id="d9030-147">You can increase the storage available to your virtual machine by [attaching a additional data disk](attach-managed-disk-portal.md).</span></span>

