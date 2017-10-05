---
title: "Klonování svazku zařízení StorSimple | Microsoft Docs"
description: "Popisuje typy různých klonování a jejich použití a vysvětluje, jak můžete pomocí zálohovacího skladu klonování svazku."
services: storsimple
documentationcenter: NA
author: alkohli
manager: carmonm
editor: 
ms.assetid: b5d615f2-02a7-4222-9867-6c0385ce748c
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 08/17/2016
ms.author: alkohli
ms.openlocfilehash: 8f1936fac543f559a44ad0f9c35b30d1a92dce68
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="use-the-storsimple-manager-service-to-clone-a-volume"></a><span data-ttu-id="b9883-103">Použít službu StorSimple Manager klonování svazku</span><span class="sxs-lookup"><span data-stu-id="b9883-103">Use the StorSimple Manager service to clone a volume</span></span>
[!INCLUDE [storsimple-version-selector-clone-volume](../../includes/storsimple-version-selector-clone-volume.md)]

## <a name="overview"></a><span data-ttu-id="b9883-104">Přehled</span><span class="sxs-lookup"><span data-stu-id="b9883-104">Overview</span></span>
<span data-ttu-id="b9883-105">Služby StorSimple Manager **zálohování katalogu** stránka zobrazuje všechny zálohovací sklady, které vytvářejí, když jsou provedeny ruční nebo automatické zálohy.</span><span class="sxs-lookup"><span data-stu-id="b9883-105">The StorSimple Manager service **Backup Catalog** page displays all the backup sets that are created when manual or automated backups are taken.</span></span> <span data-ttu-id="b9883-106">Můžete tato stránka slouží k zobrazení seznamu všech zálohy pro zásady zálohování nebo svazek, vyberte nebo odstranit zálohy, nebo použít zálohu k obnovení nebo klonování svazku.</span><span class="sxs-lookup"><span data-stu-id="b9883-106">You can use this page to list all the backups for a backup policy or a volume, select or delete backups, or use a backup to restore or clone a volume.</span></span>

![Stránka zálohování katalogu](./media/storsimple-clone-volume/HCS_BackupCatalog.png)  

<span data-ttu-id="b9883-108">Tento kurz popisuje, jak můžete pomocí zálohovacího skladu klonování svazku.</span><span class="sxs-lookup"><span data-stu-id="b9883-108">This tutorial describes how you can use a backup set to clone an individual volume.</span></span> <span data-ttu-id="b9883-109">Také vysvětluje rozdíly mezi *přechodný* a *trvalé* provede klonování.</span><span class="sxs-lookup"><span data-stu-id="b9883-109">It also explains the difference between *transient* and *permanent* clones.</span></span> 

## <a name="create-a-clone-of-a-volume"></a><span data-ttu-id="b9883-110">Vytvořit klon svazku</span><span class="sxs-lookup"><span data-stu-id="b9883-110">Create a clone of a volume</span></span>
<span data-ttu-id="b9883-111">Můžete vytvořit klon na stejné zařízení, jiná zařízení nebo i virtuální počítač pomocí místní nebo cloudový snímek.</span><span class="sxs-lookup"><span data-stu-id="b9883-111">You can create a clone on the same device, another device, or even a virtual machine by using a local or a cloud snapshot.</span></span>

#### <a name="to-clone-a-volume"></a><span data-ttu-id="b9883-112">Klonování svazku</span><span class="sxs-lookup"><span data-stu-id="b9883-112">To clone a volume</span></span>
1. <span data-ttu-id="b9883-113">Na stránce služby StorSimple Manager, klepněte **katalog zálohování** a vyberte zálohovacího skladu.</span><span class="sxs-lookup"><span data-stu-id="b9883-113">On the StorSimple Manager service page, click the **Backup catalog** tab and select a backup set.</span></span>
2. <span data-ttu-id="b9883-114">Rozbalte zálohovací sklad zobrazíte přidružené svazky.</span><span class="sxs-lookup"><span data-stu-id="b9883-114">Expand the backup set to view the associated volumes.</span></span> <span data-ttu-id="b9883-115">Klikněte a vyberte svazek ze zálohovacího skladu.</span><span class="sxs-lookup"><span data-stu-id="b9883-115">Click and select a volume from the backup set.</span></span>
   
     ![Klonování svazku](./media/storsimple-clone-volume/HCS_Clone.png) 
3. <span data-ttu-id="b9883-117">Klikněte na tlačítko **klon** zahájíte klonování vybraného svazku.</span><span class="sxs-lookup"><span data-stu-id="b9883-117">Click **Clone** to begin cloning the selected volume.</span></span>
4. <span data-ttu-id="b9883-118">V Průvodci klonování svazku v části **zadejte název a umístění**:</span><span class="sxs-lookup"><span data-stu-id="b9883-118">In the Clone Volume wizard, under **Specify name and location**:</span></span>
   
   1. <span data-ttu-id="b9883-119">Určete cílové zařízení.</span><span class="sxs-lookup"><span data-stu-id="b9883-119">Identify a target device.</span></span> <span data-ttu-id="b9883-120">Toto je umístění, kde bude vytvořen klonu.</span><span class="sxs-lookup"><span data-stu-id="b9883-120">This is the location where the clone will be created.</span></span> <span data-ttu-id="b9883-121">Můžete vybrat stejné zařízení nebo zadejte jiné zařízení.</span><span class="sxs-lookup"><span data-stu-id="b9883-121">You can choose the same device or specify another device.</span></span> <span data-ttu-id="b9883-122">Pokud zvolíte svazku přidruženém k ostatní poskytovatele cloudových služeb (ne Azure), v rozevíracím seznamu cílové zařízení se zobrazí pouze fyzických zařízení.</span><span class="sxs-lookup"><span data-stu-id="b9883-122">If you choose a volume associated with other cloud service providers (not Azure), the drop-down list for the target device will only show physical devices.</span></span> <span data-ttu-id="b9883-123">Nelze klonovat svazku přidruženém k ostatní poskytovatele cloudových služeb na virtuální zařízení.</span><span class="sxs-lookup"><span data-stu-id="b9883-123">You cannot clone a volume associated with other cloud service providers on a virtual device.</span></span>
      
      > [!NOTE]
      > <span data-ttu-id="b9883-124">Ujistěte se, že kapacitu, vyžaduje se pro klonování je nižší než kapacity, které jsou k dispozici na cílovém zařízení.</span><span class="sxs-lookup"><span data-stu-id="b9883-124">Make sure that the capacity required for the clone is lower than the capacity available on the target device.</span></span>
      > 
      > 
   2. <span data-ttu-id="b9883-125">Zadejte název jedinečné svazku pro vaše klon.</span><span class="sxs-lookup"><span data-stu-id="b9883-125">Specify a unique volume name for your clone.</span></span> <span data-ttu-id="b9883-126">Název musí obsahovat 3 až 127 znaků.</span><span class="sxs-lookup"><span data-stu-id="b9883-126">The name must contain between 3 and 127 characters.</span></span>
   3. <span data-ttu-id="b9883-127">Kliknutím na ikonu šipky</span><span class="sxs-lookup"><span data-stu-id="b9883-127">Click the arrow icon</span></span> ![ikona šipky](./media/storsimple-clone-volume/HCS_ArrowIcon.png) <span data-ttu-id="b9883-129">aby bylo možné pokračovat na další stránku.</span><span class="sxs-lookup"><span data-stu-id="b9883-129">to proceed to the next page.</span></span>
5. <span data-ttu-id="b9883-130">V části **zadejte hostitelů, které můžete použít tento svazek**:</span><span class="sxs-lookup"><span data-stu-id="b9883-130">Under **Specify hosts that can use this volume**:</span></span>
   
   1. <span data-ttu-id="b9883-131">Zadejte záznam řízení přístupu (ACR) pro klonování.</span><span class="sxs-lookup"><span data-stu-id="b9883-131">Specify an access control record (ACR) for the clone.</span></span> <span data-ttu-id="b9883-132">Můžete přidat nové ACR nebo zvolte ze seznamu existující.</span><span class="sxs-lookup"><span data-stu-id="b9883-132">You can add a new ACR or choose from the existing list.</span></span>
   2. <span data-ttu-id="b9883-133">Klikněte na ikonu zaškrtnutí</span><span class="sxs-lookup"><span data-stu-id="b9883-133">Click the check icon</span></span> ![ikona zaškrtnutí](./media/storsimple-clone-volume/HCS_CheckIcon.png)<span data-ttu-id="b9883-135">k dokončení operace.</span><span class="sxs-lookup"><span data-stu-id="b9883-135">to complete the operation.</span></span>
6. <span data-ttu-id="b9883-136">Úloha klonování bude inicializován a budete informováni při klonování se úspěšně vytvořil.</span><span class="sxs-lookup"><span data-stu-id="b9883-136">A clone job will be initiated and you will be notified when the clone is successfully created.</span></span> <span data-ttu-id="b9883-137">Klikněte na tlačítko **zobrazit úlohy** na monitoru úlohu klonování **úlohy** stránky.</span><span class="sxs-lookup"><span data-stu-id="b9883-137">Click **View Job** to monitor the clone job on the **Jobs** page.</span></span>
7. <span data-ttu-id="b9883-138">Po dokončení úlohy klonu:</span><span class="sxs-lookup"><span data-stu-id="b9883-138">After the clone job is completed:</span></span>
   
   1. <span data-ttu-id="b9883-139">Přejděte na **zařízení** a vyberte **kontejnery svazků** kartě.</span><span class="sxs-lookup"><span data-stu-id="b9883-139">Go to the **Devices** page, and select the **Volume Containers** tab.</span></span> 
   2. <span data-ttu-id="b9883-140">Vyberte kontejner svazků, který je přidružen zdrojový svazek, který jste naklonovali.</span><span class="sxs-lookup"><span data-stu-id="b9883-140">Select the volume container that is associated with the source volume that you cloned.</span></span> <span data-ttu-id="b9883-141">V seznamu svazků měli byste vidět klonování, kterou jste právě vytvořili.</span><span class="sxs-lookup"><span data-stu-id="b9883-141">In the list of volumes, you should see the clone that was just created.</span></span>

> [!NOTE]
> <span data-ttu-id="b9883-142">Monitorování a výchozí zálohování jsou automaticky zakázán na klonovaný svazku.</span><span class="sxs-lookup"><span data-stu-id="b9883-142">Monitoring and default backup are automatically disabled on a cloned volume.</span></span>
> 
> 

<span data-ttu-id="b9883-143">Klon, který je vytvořen tímto způsobem je přechodný klonu.</span><span class="sxs-lookup"><span data-stu-id="b9883-143">A clone that is created this way is a transient clone.</span></span> <span data-ttu-id="b9883-144">Další informace o typech klonování najdete v tématu [přechodný oproti trvalé klony](#transient-vs-permanent-clones).</span><span class="sxs-lookup"><span data-stu-id="b9883-144">For more information about clone types, see [Transient vs. permanent clones](#transient-vs-permanent-clones).</span></span>

<span data-ttu-id="b9883-145">Tenhle klon. je teď regulární svazek a všechny operace, které je možné ve svazku, bude k dispozici pro klonování.</span><span class="sxs-lookup"><span data-stu-id="b9883-145">This clone is now a regular volume, and any operation that is possible on a volume will be available for the clone.</span></span> <span data-ttu-id="b9883-146">Musíte nakonfigurovat tento svazek pro všechny zálohy.</span><span class="sxs-lookup"><span data-stu-id="b9883-146">You will need to configure this volume for any backups.</span></span>

## <a name="transient-vs-permanent-clones"></a><span data-ttu-id="b9883-147">Pouze dočasné a trvalé klony</span><span class="sxs-lookup"><span data-stu-id="b9883-147">Transient vs. permanent clones</span></span>
<span data-ttu-id="b9883-148">Přechodný a trvalé klony vytvoří jenom v případě, že se klonování na jiném zařízení.</span><span class="sxs-lookup"><span data-stu-id="b9883-148">Transient and permanent clones are created only when you are cloning on to a different device.</span></span> <span data-ttu-id="b9883-149">Může klonovat svazek konkrétní ze zálohovacího skladu na jiném zařízení.</span><span class="sxs-lookup"><span data-stu-id="b9883-149">You can clone a specific volume from a backup set to a different device.</span></span> <span data-ttu-id="b9883-150">Klon vytvořené v tomto případě je *přechodný* klonování.</span><span class="sxs-lookup"><span data-stu-id="b9883-150">A clone created in this way is a *transient* clone.</span></span> <span data-ttu-id="b9883-151">Přechodný klonu bude mít odkazy na původního svazku a bude používat tento svazek čtení během zápisu místně.</span><span class="sxs-lookup"><span data-stu-id="b9883-151">The transient clone will have references to the original volume and will use that volume to read while writing locally.</span></span> 

<span data-ttu-id="b9883-152">Po provedení cloudový snímek přechodný klon, bude výsledný klonování *trvalé* klonování.</span><span class="sxs-lookup"><span data-stu-id="b9883-152">After you take a cloud snapshot of a transient clone, the resulting clone will be a *permanent* clone.</span></span> <span data-ttu-id="b9883-153">Trvalé klon je nezávislá a nemá všechny odkazy na původní svazek, který byl klonovat z.</span><span class="sxs-lookup"><span data-stu-id="b9883-153">The permanent clone is independent and doesn’t have any references to the original volume that it was cloned from.</span></span>  

## <a name="scenarios-for-transient-and-permanent-clones"></a><span data-ttu-id="b9883-154">Scénáře pro přechodný a trvalé klony</span><span class="sxs-lookup"><span data-stu-id="b9883-154">Scenarios for transient and permanent clones</span></span>
<span data-ttu-id="b9883-155">Následující části popisují příklad situacích, ve kterých může být použit klony přechodný a trvalé.</span><span class="sxs-lookup"><span data-stu-id="b9883-155">The following sections describe example situations in which transient and permanent clones can be used.</span></span>

### <a name="item-level-recovery-with-a-transient-clone"></a><span data-ttu-id="b9883-156">Obnovení na úrovni položek s přechodný klon</span><span class="sxs-lookup"><span data-stu-id="b9883-156">Item-level recovery with a transient clone</span></span>
<span data-ttu-id="b9883-157">Budete muset obnovit soubor s jedním. roku starý Microsoft PowerPoint.</span><span class="sxs-lookup"><span data-stu-id="b9883-157">You need to recover a one-year-old Microsoft PowerPoint presentation file.</span></span> <span data-ttu-id="b9883-158">Váš správce IT identifikuje konkrétní zálohování z toto časové období a pak filtry svazku.</span><span class="sxs-lookup"><span data-stu-id="b9883-158">Your IT administrator identifies the specific backup from that time frame, and then filters the volume.</span></span> <span data-ttu-id="b9883-159">Správce pak provede klonování svazku, vyhledá soubor, který hledáte a poskytuje vám.</span><span class="sxs-lookup"><span data-stu-id="b9883-159">The administrator then clones the volume, locates the file that you are looking for, and provides it to you.</span></span> <span data-ttu-id="b9883-160">V tomto scénáři se používá přechodný klonu.</span><span class="sxs-lookup"><span data-stu-id="b9883-160">In this scenario, a transient clone is used.</span></span> 

<span data-ttu-id="b9883-161">![Dostupné video](./media/storsimple-clone-volume/Video_icon.png) **Dostupné video**</span><span class="sxs-lookup"><span data-stu-id="b9883-161">![Video available](./media/storsimple-clone-volume/Video_icon.png) **Video available**</span></span>

<span data-ttu-id="b9883-162">Pokud chcete přehrát video, které ukazuje, jak můžete použít klonu a funkce v zařízení StorSimple obnovit odstraněné soubory obnovit, klikněte na tlačítko [zde](https://azure.microsoft.com/documentation/videos/storsimple-recover-deleted-files-with-storsimple/).</span><span class="sxs-lookup"><span data-stu-id="b9883-162">To watch a video that demonstrates how you can use the clone and restore features in StorSimple to recover deleted files, click [here](https://azure.microsoft.com/documentation/videos/storsimple-recover-deleted-files-with-storsimple/).</span></span>

### <a name="testing-in-the-production-environment-with-a-permanent-clone"></a><span data-ttu-id="b9883-163">Testování v produkčním prostředí s trvalé klon</span><span class="sxs-lookup"><span data-stu-id="b9883-163">Testing in the production environment with a permanent clone</span></span>
<span data-ttu-id="b9883-164">Je třeba ověřit chyby testování v produkčním prostředí.</span><span class="sxs-lookup"><span data-stu-id="b9883-164">You need to verify a testing bug in the production environment.</span></span> <span data-ttu-id="b9883-165">Při vytváření klon svazku v provozním prostředí pořízení snímku cloudu Tenhle klon.</span><span class="sxs-lookup"><span data-stu-id="b9883-165">You create a clone of the volume in the production environment by taking a cloud snapshot of this clone.</span></span> <span data-ttu-id="b9883-166">Klonovaný svazek je nyní nezávislé.</span><span class="sxs-lookup"><span data-stu-id="b9883-166">The cloned volume is now independent.</span></span> <span data-ttu-id="b9883-167">V tomto scénáři se používá trvalá klonu.</span><span class="sxs-lookup"><span data-stu-id="b9883-167">In this scenario, a permanent clone is used.</span></span>

## <a name="next-steps"></a><span data-ttu-id="b9883-168">Další kroky</span><span class="sxs-lookup"><span data-stu-id="b9883-168">Next steps</span></span>
* <span data-ttu-id="b9883-169">Zjistěte, jak [obnovit svazek StorSimple ze zálohovacího skladu](storsimple-restore-from-backup-set.md).</span><span class="sxs-lookup"><span data-stu-id="b9883-169">Learn how to [restore a StorSimple volume from a backup set](storsimple-restore-from-backup-set.md).</span></span>
* <span data-ttu-id="b9883-170">Zjistěte, jak [použít službu StorSimple Manager ke správě zařízení StorSimple](storsimple-manager-service-administration.md).</span><span class="sxs-lookup"><span data-stu-id="b9883-170">Learn how to [use the StorSimple Manager service to administer your StorSimple device](storsimple-manager-service-administration.md).</span></span>

