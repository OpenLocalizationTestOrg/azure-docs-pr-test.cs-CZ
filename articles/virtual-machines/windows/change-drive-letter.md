---
title: "Ujistěte se, hello D: disku virtuálního počítače datový disk | Microsoft Docs"
description: "Popisuje, jak písmena jednotky toochange pro virtuální počítač s Windows tak, aby hello D: jednotky můžete použít jako datový disk."
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
ms.openlocfilehash: 43939da1a890ac4049266487951e3889aa0ed9d7
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="use-hello-d-drive-as-a-data-drive-on-a-windows-vm"></a><span data-ttu-id="b57a7-103">Použití disků D: hello jako datový disk na virtuální počítač s Windows</span><span class="sxs-lookup"><span data-stu-id="b57a7-103">Use hello D: drive as a data drive on a Windows VM</span></span>
<span data-ttu-id="b57a7-104">Pokud aplikace potřebuje toouse hello D jednotky toostore dat, postupujte podle těchto pokynů toouse jiné písmeno jednotky pro dočasné disk hello.</span><span class="sxs-lookup"><span data-stu-id="b57a7-104">If your application needs toouse hello D drive toostore data, follow these instructions toouse a different drive letter for hello temporary disk.</span></span> <span data-ttu-id="b57a7-105">Nikdy nepoužívejte hello dočasným diskovým toostore dat, je nutné, aby tookeep.</span><span class="sxs-lookup"><span data-stu-id="b57a7-105">Never use hello temporary disk toostore data that you need tookeep.</span></span>

<span data-ttu-id="b57a7-106">Při změně velikosti nebo **zastavení (Deallocate)** virtuální počítač, to může aktivovat umístění nového hypervisoru tooa hello virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="b57a7-106">If you resize or **Stop (Deallocate)** a virtual machine, this may trigger placement of hello virtual machine tooa new hypervisor.</span></span> <span data-ttu-id="b57a7-107">Události plánované i neplánované údržby, může se spustit toto umístění.</span><span class="sxs-lookup"><span data-stu-id="b57a7-107">A planned or unplanned maintenance event may also trigger this placement.</span></span> <span data-ttu-id="b57a7-108">V tomto scénáři bude dočasným diskovým hello přeřadit toohello první dostupné písmeno jednotky.</span><span class="sxs-lookup"><span data-stu-id="b57a7-108">In this scenario, hello temporary disk will be reassigned toohello first available drive letter.</span></span> <span data-ttu-id="b57a7-109">Pokud máte aplikaci, která vyžaduje konkrétně hello D: jednotky, je třeba toofollow tyto kroky tootemporarily přesunutí hello pagefile.sys připojit nový datový disk a přiřaďte ho hello písmeno D a potom přesunutí hello pagefile.sys zpět toohello dočasné jednotce.</span><span class="sxs-lookup"><span data-stu-id="b57a7-109">If you have an application that specifically requires hello D: drive, you need toofollow these steps tootemporarily move hello pagefile.sys, attach a new data disk and assign it hello letter D and then move hello pagefile.sys back toohello temporary drive.</span></span> <span data-ttu-id="b57a7-110">Po dokončení Azure nebude zpět hello D: Pokud hello virtuální počítač přesune tooa různých hypervisoru.</span><span class="sxs-lookup"><span data-stu-id="b57a7-110">Once complete, Azure will not take back hello D: if hello VM moves tooa different hypervisor.</span></span>

<span data-ttu-id="b57a7-111">Další informace o tom, jak Azure používá dočasným diskovým hello najdete v tématu [pochopení hello jednotku ve virtuálních počítačích Microsoft Azure](https://blogs.msdn.microsoft.com/mast/2013/12/06/understanding-the-temporary-drive-on-windows-azure-virtual-machines/)</span><span class="sxs-lookup"><span data-stu-id="b57a7-111">For more information about how Azure uses hello temporary disk, see [Understanding hello temporary drive on Microsoft Azure Virtual Machines](https://blogs.msdn.microsoft.com/mast/2013/12/06/understanding-the-temporary-drive-on-windows-azure-virtual-machines/)</span></span>

## <a name="attach-hello-data-disk"></a><span data-ttu-id="b57a7-112">Připojit datový disk hello</span><span class="sxs-lookup"><span data-stu-id="b57a7-112">Attach hello data disk</span></span>
<span data-ttu-id="b57a7-113">Nejdřív musíte tooattach hello datového disku toohello virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="b57a7-113">First, you'll need tooattach hello data disk toohello virtual machine.</span></span> <span data-ttu-id="b57a7-114">toodo tento pomocí hello portálu, najdete v tématu [jak tooattach spravovaná data na disku v hello portál Azure](attach-managed-disk-portal.md).</span><span class="sxs-lookup"><span data-stu-id="b57a7-114">toodo this using hello portal, see [How tooattach a managed data disk in hello Azure portal](attach-managed-disk-portal.md).</span></span>

## <a name="temporarily-move-pagefilesys-tooc-drive"></a><span data-ttu-id="b57a7-115">Dočasně přesunout pagefile.sys tooC jednotky</span><span class="sxs-lookup"><span data-stu-id="b57a7-115">Temporarily move pagefile.sys tooC drive</span></span>
1. <span data-ttu-id="b57a7-116">Připojte toohello virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="b57a7-116">Connect toohello virtual machine.</span></span> 
2. <span data-ttu-id="b57a7-117">Klikněte pravým tlačítkem na hello **spustit** nabídku a vyberte **systému**.</span><span class="sxs-lookup"><span data-stu-id="b57a7-117">Right-click hello **Start** menu and select **System**.</span></span>
3. <span data-ttu-id="b57a7-118">V levé nabídce hello vyberte **Upřesnit nastavení systému**.</span><span class="sxs-lookup"><span data-stu-id="b57a7-118">In hello left-hand menu, select **Advanced system settings**.</span></span>
4. <span data-ttu-id="b57a7-119">V hello **výkonu** vyberte **nastavení**.</span><span class="sxs-lookup"><span data-stu-id="b57a7-119">In hello **Performance** section, select **Settings**.</span></span>
5. <span data-ttu-id="b57a7-120">Vyberte hello **Upřesnit** kartě.</span><span class="sxs-lookup"><span data-stu-id="b57a7-120">Select hello **Advanced** tab.</span></span>
6. <span data-ttu-id="b57a7-121">V hello **virtuální paměti** vyberte **změnu**.</span><span class="sxs-lookup"><span data-stu-id="b57a7-121">In hello **Virtual memory** section, select **Change**.</span></span>
7. <span data-ttu-id="b57a7-122">Vyberte hello **C** jednotky a pak klikněte na **velikost určí systém** a pak klikněte na **nastavit**.</span><span class="sxs-lookup"><span data-stu-id="b57a7-122">Select hello **C** drive and then click **System managed size** and then click **Set**.</span></span>
8. <span data-ttu-id="b57a7-123">Vyberte hello **D** jednotky a pak klikněte na **stránkovací soubor** a pak klikněte na **nastavit**.</span><span class="sxs-lookup"><span data-stu-id="b57a7-123">Select hello **D** drive and then click **No paging file** and then click **Set**.</span></span>
9. <span data-ttu-id="b57a7-124">Kliknutím na tlačítko použít.</span><span class="sxs-lookup"><span data-stu-id="b57a7-124">Click Apply.</span></span> <span data-ttu-id="b57a7-125">Zobrazí se upozornění, že tento počítač hello je toobe restartovat, aby se mohla ovlivnit tootake změny hello.</span><span class="sxs-lookup"><span data-stu-id="b57a7-125">You will get a warning that hello computer needs toobe restarted for hello changes tootake affect.</span></span>
10. <span data-ttu-id="b57a7-126">Restartujte hello virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="b57a7-126">Restart hello virtual machine.</span></span>

## <a name="change-hello-drive-letters"></a><span data-ttu-id="b57a7-127">Změnit písmena jednotek hello</span><span class="sxs-lookup"><span data-stu-id="b57a7-127">Change hello drive letters</span></span>
1. <span data-ttu-id="b57a7-128">Jednou hello restartování virtuálního počítače, přihlaste se zpět toohello virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="b57a7-128">Once hello VM restarts, log back on toohello VM.</span></span>
2. <span data-ttu-id="b57a7-129">Klikněte na tlačítko hello **spustit** nabídce a typ **diskmgmt.msc** a stiskněte Enter.</span><span class="sxs-lookup"><span data-stu-id="b57a7-129">Click hello **Start** menu and type **diskmgmt.msc** and hit Enter.</span></span> <span data-ttu-id="b57a7-130">Správa disků se spustí.</span><span class="sxs-lookup"><span data-stu-id="b57a7-130">Disk Management will start.</span></span>
3. <span data-ttu-id="b57a7-131">Klikněte pravým tlačítkem na **D**hello dočasné úložiště jednotky a vyberte **změnit písmeno jednotky a cesty**.</span><span class="sxs-lookup"><span data-stu-id="b57a7-131">Right-click on **D**, hello Temporary Storage drive, and select **Change Drive Letter and Paths**.</span></span>
4. <span data-ttu-id="b57a7-132">V části písmeno jednotky, vyberte na nový disk **T** a pak klikněte na **OK**.</span><span class="sxs-lookup"><span data-stu-id="b57a7-132">Under Drive letter, select a new drive such as **T** and then click **OK**.</span></span> 
5. <span data-ttu-id="b57a7-133">Klikněte pravým tlačítkem na hello datový disk a vyberte **změnit písmeno jednotky a cesty**.</span><span class="sxs-lookup"><span data-stu-id="b57a7-133">Right-click on hello data disk, and select **Change Drive Letter and Paths**.</span></span>
6. <span data-ttu-id="b57a7-134">V části písmeno jednotky, vyberte jednotky **D** a pak klikněte na **OK**.</span><span class="sxs-lookup"><span data-stu-id="b57a7-134">Under Drive letter, select drive **D** and then click **OK**.</span></span> 

## <a name="move-pagefilesys-back-toohello-temporary-storage-drive"></a><span data-ttu-id="b57a7-135">Přesunout pagefile.sys back toohello dočasné úložiště disku</span><span class="sxs-lookup"><span data-stu-id="b57a7-135">Move pagefile.sys back toohello temporary storage drive</span></span>
1. <span data-ttu-id="b57a7-136">Klikněte pravým tlačítkem na hello **spustit** nabídku a vyberte **systému**</span><span class="sxs-lookup"><span data-stu-id="b57a7-136">Right-click hello **Start** menu and select **System**</span></span>
2. <span data-ttu-id="b57a7-137">V levé nabídce hello vyberte **Upřesnit nastavení systému**.</span><span class="sxs-lookup"><span data-stu-id="b57a7-137">In hello left-hand menu, select **Advanced system settings**.</span></span>
3. <span data-ttu-id="b57a7-138">V hello **výkonu** vyberte **nastavení**.</span><span class="sxs-lookup"><span data-stu-id="b57a7-138">In hello **Performance** section, select **Settings**.</span></span>
4. <span data-ttu-id="b57a7-139">Vyberte hello **Upřesnit** kartě.</span><span class="sxs-lookup"><span data-stu-id="b57a7-139">Select hello **Advanced** tab.</span></span>
5. <span data-ttu-id="b57a7-140">V hello **virtuální paměti** vyberte **změnu**.</span><span class="sxs-lookup"><span data-stu-id="b57a7-140">In hello **Virtual memory** section, select **Change**.</span></span>
6. <span data-ttu-id="b57a7-141">Vyberte jednotku hello OS **C** a klikněte na tlačítko **stránkovací soubor** a pak klikněte na **nastavit**.</span><span class="sxs-lookup"><span data-stu-id="b57a7-141">Select hello OS drive **C** and click **No paging file** and then click **Set**.</span></span>
7. <span data-ttu-id="b57a7-142">Vyberte jednotku dočasné úložiště hello **T** a pak klikněte na **velikost určí systém** a pak klikněte na **nastavit**.</span><span class="sxs-lookup"><span data-stu-id="b57a7-142">Select hello temporary storage drive **T** and then click **System managed size** and then click **Set**.</span></span>
8. <span data-ttu-id="b57a7-143">Klikněte na tlačítko **Použít**.</span><span class="sxs-lookup"><span data-stu-id="b57a7-143">Click **Apply**.</span></span> <span data-ttu-id="b57a7-144">Zobrazí se upozornění, že tento počítač hello je toobe restartovat, aby se mohla ovlivnit tootake změny hello.</span><span class="sxs-lookup"><span data-stu-id="b57a7-144">You will get a warning that hello computer needs toobe restarted for hello changes tootake affect.</span></span>
9. <span data-ttu-id="b57a7-145">Restartujte hello virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="b57a7-145">Restart hello virtual machine.</span></span>

## <a name="next-steps"></a><span data-ttu-id="b57a7-146">Další kroky</span><span class="sxs-lookup"><span data-stu-id="b57a7-146">Next steps</span></span>
* <span data-ttu-id="b57a7-147">Můžete zvýšit hello úložiště k dispozici tooyour virtuálního počítače pomocí [další datový disk se připojuje](attach-managed-disk-portal.md).</span><span class="sxs-lookup"><span data-stu-id="b57a7-147">You can increase hello storage available tooyour virtual machine by [attaching a additional data disk](attach-managed-disk-portal.md).</span></span>

