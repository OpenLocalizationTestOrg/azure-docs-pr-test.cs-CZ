---
title: "Klonování svazku řady StorSimple 8000 | Microsoft Docs"
description: "Popisuje typy různých klonování a jejich použití a vysvětluje, jak můžete pomocí zálohovacího skladu klonování svazku."
services: storsimple
documentationcenter: NA
author: alkohli
manager: carmonm
editor: 
ms.assetid: 070ac53e-7388-4c48-b8a5-8ed7f9108b2c
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 07/26/2017
ms.author: alkohli
ms.openlocfilehash: 2b627250df62bbe3299869ef3812947afbb8f29f
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="use-the-storsimple-manager-service-to-clone-a-volume-update-2"></a><span data-ttu-id="22d35-103">Použít službu StorSimple Manager klonovat svazek (Update 2)</span><span class="sxs-lookup"><span data-stu-id="22d35-103">Use the StorSimple Manager service to clone a volume (Update 2)</span></span>
[!INCLUDE [storsimple-version-selector-clone-volume](../../includes/storsimple-version-selector-clone-volume.md)]

## <a name="overview"></a><span data-ttu-id="22d35-104">Přehled</span><span class="sxs-lookup"><span data-stu-id="22d35-104">Overview</span></span>
<span data-ttu-id="22d35-105">Služby StorSimple Manager **zálohování katalogu** stránka zobrazuje všechny zálohovací sklady, které vytvářejí, když jsou provedeny ruční nebo automatické zálohy.</span><span class="sxs-lookup"><span data-stu-id="22d35-105">The StorSimple Manager service **Backup Catalog** page displays all the backup sets that are created when manual or automated backups are taken.</span></span> <span data-ttu-id="22d35-106">Můžete tato stránka slouží k zobrazení seznamu všech zálohy pro zásady zálohování nebo svazek, vyberte nebo odstranit zálohy, nebo použít zálohu k obnovení nebo klonování svazku.</span><span class="sxs-lookup"><span data-stu-id="22d35-106">You can use this page to list all the backups for a backup policy or a volume, select or delete backups, or use a backup to restore or clone a volume.</span></span>

![Stránka zálohování katalogu](./media/storsimple-clone-volume-u2/backupCatalog.png)  

<span data-ttu-id="22d35-108">Tento kurz popisuje, jak můžete pomocí zálohovacího skladu klonování svazku.</span><span class="sxs-lookup"><span data-stu-id="22d35-108">This tutorial describes how you can use a backup set to clone an individual volume.</span></span> <span data-ttu-id="22d35-109">Také vysvětluje rozdíly mezi *přechodný* a *trvalé* provede klonování.</span><span class="sxs-lookup"><span data-stu-id="22d35-109">It also explains the difference between *transient* and *permanent* clones.</span></span>

> [!NOTE]
> <span data-ttu-id="22d35-110">Místně vázaný svazek, budou se klonovat jako vrstvený svazek.</span><span class="sxs-lookup"><span data-stu-id="22d35-110">A locally pinned volume will be cloned as a tiered volume.</span></span> <span data-ttu-id="22d35-111">Pokud potřebujete klonovaný být místně vázaný svazek, můžete klonu převést místně vázaný svazek po úspěšném dokončení operace klonování.</span><span class="sxs-lookup"><span data-stu-id="22d35-111">If you need the cloned volume to be locally pinned, you can convert the clone to a locally pinned volume after the clone operation is successfully completed.</span></span> <span data-ttu-id="22d35-112">Informace o převodu vrstvený svazek na místně vázaný svazek, přejděte na [změnit typ svazku](storsimple-manage-volumes-u2.md#change-the-volume-type).</span><span class="sxs-lookup"><span data-stu-id="22d35-112">For information about converting a tiered volume to a locally pinned volume, go to [Change the volume type](storsimple-manage-volumes-u2.md#change-the-volume-type).</span></span>
> 
> <span data-ttu-id="22d35-113">Pokud se pokusíte převést svazek klonovaný ze zřízeny vrstvené místně vázaný okamžitě po klonování (Pokud je stále přechodný klon), převod selže s se následující chybová zpráva:</span><span class="sxs-lookup"><span data-stu-id="22d35-113">If you try to convert a cloned volume from tiered to locally pinned immediately after cloning (when it is still a transient clone), the conversion will fail with the following error message:</span></span>
> 
> `Unable to modify the usage type for volume {0}. This can happen if the volume being modified is a transient clone and hasn’t been made permanent. Take a cloud snapshot of this volume and then retry the modify operation.` 
> 
> <span data-ttu-id="22d35-114">Tato chyba byl přijat pouze v případě, že se klonování na jiném zařízení.</span><span class="sxs-lookup"><span data-stu-id="22d35-114">This error is received only if you are cloning on to a different device.</span></span> <span data-ttu-id="22d35-115">Pokud převedete přechodný klonu nejdřív na trvalé klon místně vázaný svazek můžete úspěšně převést.</span><span class="sxs-lookup"><span data-stu-id="22d35-115">You can successfully convert the volume to locally pinned if you first convert the transient clone to a permanent clone.</span></span> <span data-ttu-id="22d35-116">Převést přechodný klonu trvalé klon, pořízení snímku cloudu ho.</span><span class="sxs-lookup"><span data-stu-id="22d35-116">To convert the transient clone to a permanent clone, take a cloud snapshot of it.</span></span>
> 
> 

## <a name="create-a-clone-of-a-volume"></a><span data-ttu-id="22d35-117">Vytvořit klon svazku</span><span class="sxs-lookup"><span data-stu-id="22d35-117">Create a clone of a volume</span></span>
<span data-ttu-id="22d35-118">Můžete vytvořit klon na stejné zařízení, jiná zařízení nebo i virtuální počítač pomocí místní nebo cloudový snímek.</span><span class="sxs-lookup"><span data-stu-id="22d35-118">You can create a clone on the same device, another device, or even a virtual machine by using a local or cloud snapshot.</span></span>

#### <a name="to-clone-a-volume"></a><span data-ttu-id="22d35-119">Klonování svazku</span><span class="sxs-lookup"><span data-stu-id="22d35-119">To clone a volume</span></span>
1. <span data-ttu-id="22d35-120">Na stránce služby StorSimple Manager, klepněte **katalog zálohování** a vyberte zálohovacího skladu.</span><span class="sxs-lookup"><span data-stu-id="22d35-120">On the StorSimple Manager service page, click the **Backup catalog** tab and select a backup set.</span></span>
2. <span data-ttu-id="22d35-121">Rozbalte zálohovací sklad zobrazíte přidružené svazky.</span><span class="sxs-lookup"><span data-stu-id="22d35-121">Expand the backup set to view the associated volumes.</span></span> <span data-ttu-id="22d35-122">Klikněte a vyberte svazek ze zálohovacího skladu.</span><span class="sxs-lookup"><span data-stu-id="22d35-122">Click and select a volume from the backup set.</span></span>
   
     ![Klonování svazku](./media/storsimple-clone-volume-u2/CloneVol.png) 
3. <span data-ttu-id="22d35-124">Klikněte na tlačítko **klon** zahájíte klonování vybraného svazku.</span><span class="sxs-lookup"><span data-stu-id="22d35-124">Click **Clone** to begin cloning the selected volume.</span></span>
4. <span data-ttu-id="22d35-125">V Průvodci klonování svazku v části **zadejte název a umístění**:</span><span class="sxs-lookup"><span data-stu-id="22d35-125">In the Clone Volume wizard, under **Specify name and location**:</span></span>
   
   1. <span data-ttu-id="22d35-126">Určete cílové zařízení.</span><span class="sxs-lookup"><span data-stu-id="22d35-126">Identify a target device.</span></span> <span data-ttu-id="22d35-127">Toto je umístění, kde bude vytvořen klonu.</span><span class="sxs-lookup"><span data-stu-id="22d35-127">This is the location where the clone will be created.</span></span> <span data-ttu-id="22d35-128">Můžete vybrat stejné zařízení nebo zadejte jiné zařízení.</span><span class="sxs-lookup"><span data-stu-id="22d35-128">You can choose the same device or specify another device.</span></span> <span data-ttu-id="22d35-129">Pokud zvolíte svazku přidruženém k ostatní poskytovatele cloudových služeb (ne Azure), v rozevíracím seznamu cílové zařízení se zobrazí pouze fyzických zařízení.</span><span class="sxs-lookup"><span data-stu-id="22d35-129">If you choose a volume associated with other cloud service providers (not Azure), the drop-down list for the target device will only show physical devices.</span></span> <span data-ttu-id="22d35-130">Nelze klonovat svazku přidruženém k ostatní poskytovatele cloudových služeb na virtuální zařízení.</span><span class="sxs-lookup"><span data-stu-id="22d35-130">You cannot clone a volume associated with other cloud service providers on a virtual device.</span></span>
      
      > [!NOTE]
      > <span data-ttu-id="22d35-131">Ujistěte se, že kapacitu, vyžaduje se pro klonování je nižší než kapacity, které jsou k dispozici na cílovém zařízení.</span><span class="sxs-lookup"><span data-stu-id="22d35-131">Make sure that the capacity required for the clone is lower than the capacity available on the target device.</span></span>
      > 
      > 
   2. <span data-ttu-id="22d35-132">Zadejte název jedinečné svazku pro vaše klon.</span><span class="sxs-lookup"><span data-stu-id="22d35-132">Specify a unique volume name for your clone.</span></span> <span data-ttu-id="22d35-133">Název musí obsahovat 3 až 127 znaků.</span><span class="sxs-lookup"><span data-stu-id="22d35-133">The name must contain between 3 and 127 characters.</span></span> 
      
      > [!NOTE]
      > <span data-ttu-id="22d35-134">**Klonování svazku jako** pole bude **nastavování** i v případě, že se klonování místně vázaný svazek.</span><span class="sxs-lookup"><span data-stu-id="22d35-134">The **Clone Volume As** field will be **Tiered** even if you are cloning a locally pinned volume.</span></span> <span data-ttu-id="22d35-135">Nelze změnit toto nastavení; ale pokud budete potřebovat klonovaný svazku, který má být místně vázaný i, můžete převést klonu pro místně vázaný svazek po úspěšném vytvoření klonu.</span><span class="sxs-lookup"><span data-stu-id="22d35-135">You cannot change this setting; however, if you need the cloned volume to be locally pinned as well, you can convert the clone to a locally pinned volume after you successfully create the clone.</span></span> <span data-ttu-id="22d35-136">Informace o převodu vrstvený svazek na místně vázaný svazek, přejděte na [změnit typ svazku](storsimple-manage-volumes-u2.md#change-the-volume-type).</span><span class="sxs-lookup"><span data-stu-id="22d35-136">For information about converting a tiered volume to a locally pinned volume, go to [Change the volume type](storsimple-manage-volumes-u2.md#change-the-volume-type).</span></span>
      > 
      > 
      
        ![Průvodce klon 1](./media/storsimple-clone-volume-u2/clone1.png) 
   3. <span data-ttu-id="22d35-138">Kliknutím na ikonu šipky</span><span class="sxs-lookup"><span data-stu-id="22d35-138">Click the arrow icon</span></span> ![ikona šipky](./media/storsimple-clone-volume-u2/HCS_ArrowIcon.png) <span data-ttu-id="22d35-140">aby bylo možné pokračovat na další stránku.</span><span class="sxs-lookup"><span data-stu-id="22d35-140">to proceed to the next page.</span></span>
5. <span data-ttu-id="22d35-141">V části **zadejte hostitelů, které můžete použít tento svazek**:</span><span class="sxs-lookup"><span data-stu-id="22d35-141">Under **Specify hosts that can use this volume**:</span></span>
   
   1. <span data-ttu-id="22d35-142">Zadejte záznam řízení přístupu (ACR) pro klonování.</span><span class="sxs-lookup"><span data-stu-id="22d35-142">Specify an access control record (ACR) for the clone.</span></span> <span data-ttu-id="22d35-143">Můžete přidat nové ACR nebo zvolte ze seznamu existující.</span><span class="sxs-lookup"><span data-stu-id="22d35-143">You can add a new ACR or choose from the existing list.</span></span>
      
        ![Průvodce klon 2](./media/storsimple-clone-volume-u2/clone2.png) 
   2. <span data-ttu-id="22d35-145">Klikněte na ikonu zaškrtnutí</span><span class="sxs-lookup"><span data-stu-id="22d35-145">Click the check icon</span></span> ![ikona zaškrtnutí](./media/storsimple-clone-volume-u2/HCS_CheckIcon.png)<span data-ttu-id="22d35-147">k dokončení operace.</span><span class="sxs-lookup"><span data-stu-id="22d35-147">to complete the operation.</span></span>
6. <span data-ttu-id="22d35-148">Úloha klonování bude inicializován a budete informováni při klonování se úspěšně vytvořil.</span><span class="sxs-lookup"><span data-stu-id="22d35-148">A clone job will be initiated and you will be notified when the clone is successfully created.</span></span> <span data-ttu-id="22d35-149">Klikněte na tlačítko **zobrazit úlohy** na monitoru úlohu klonování **úlohy** stránky.</span><span class="sxs-lookup"><span data-stu-id="22d35-149">Click **View Job** to monitor the clone job on the **Jobs** page.</span></span> <span data-ttu-id="22d35-150">Po dokončení úlohy klonování, zobrazí se následující zpráva:</span><span class="sxs-lookup"><span data-stu-id="22d35-150">You will see the following message when the clone job is finished:</span></span>
   
    ![Zpráva klonování](./media/storsimple-clone-volume-u2/CloneMsg.png) 
7. <span data-ttu-id="22d35-152">Po dokončení úlohy klonu:</span><span class="sxs-lookup"><span data-stu-id="22d35-152">After the clone job is completed:</span></span>
   
   1. <span data-ttu-id="22d35-153">Přejděte na **zařízení** a vyberte **kontejnery svazků** kartě.</span><span class="sxs-lookup"><span data-stu-id="22d35-153">Go to the **Devices** page, and select the **Volume Containers** tab.</span></span> 
   2. <span data-ttu-id="22d35-154">Vyberte kontejner svazků, který je přidružen zdrojový svazek, který jste naklonovali.</span><span class="sxs-lookup"><span data-stu-id="22d35-154">Select the volume container that is associated with the source volume that you cloned.</span></span> <span data-ttu-id="22d35-155">V seznamu svazků měli byste vidět klonování, kterou jste právě vytvořili.</span><span class="sxs-lookup"><span data-stu-id="22d35-155">In the list of volumes, you should see the clone that was just created.</span></span>

> [!NOTE]
> <span data-ttu-id="22d35-156">Monitorování a výchozí zálohování jsou automaticky zakázán na klonovaný svazku.</span><span class="sxs-lookup"><span data-stu-id="22d35-156">Monitoring and default backup are automatically disabled on a cloned volume.</span></span>
> 
> 

<span data-ttu-id="22d35-157">Klon, který je vytvořen tímto způsobem je přechodný klonu.</span><span class="sxs-lookup"><span data-stu-id="22d35-157">A clone that is created this way is a transient clone.</span></span> <span data-ttu-id="22d35-158">Další informace o typech klonování najdete v tématu [přechodný oproti trvalé klony](#transient-vs-permanent-clones).</span><span class="sxs-lookup"><span data-stu-id="22d35-158">For more information about clone types, see [Transient vs. permanent clones](#transient-vs-permanent-clones).</span></span>

<span data-ttu-id="22d35-159">Tenhle klon. je teď regulární svazek a všechny operace, které je možné ve svazku, bude k dispozici pro klonování.</span><span class="sxs-lookup"><span data-stu-id="22d35-159">This clone is now a regular volume, and any operation that is possible on a volume will be available for the clone.</span></span> <span data-ttu-id="22d35-160">Musíte nakonfigurovat tento svazek pro všechny zálohy.</span><span class="sxs-lookup"><span data-stu-id="22d35-160">You will need to configure this volume for any backups.</span></span>

## <a name="transient-vs-permanent-clones"></a><span data-ttu-id="22d35-161">Pouze dočasné a trvalé klony</span><span class="sxs-lookup"><span data-stu-id="22d35-161">Transient vs. permanent clones</span></span>
<span data-ttu-id="22d35-162">Přechodný klony vytvoří jenom v případě, že se klonování na jiném zařízení.</span><span class="sxs-lookup"><span data-stu-id="22d35-162">Transient clones are created only when you are cloning to a different device.</span></span> <span data-ttu-id="22d35-163">Může klonovat svazek konkrétní ze zálohovacího skladu na jiném zařízení spravovat pomocí Správce zařízení StorSimple.</span><span class="sxs-lookup"><span data-stu-id="22d35-163">You can clone a specific volume from a backup set to a different device managed by the StorSimple Manager.</span></span> <span data-ttu-id="22d35-164">Přechodný klonu bude mít odkazů na data v původním svazku a bude používat tato data ke čtení a zápisu místně na cílovém zařízení.</span><span class="sxs-lookup"><span data-stu-id="22d35-164">The transient clone will have references to the data in the original volume and will use that data to read and write locally on the target device.</span></span> 

<span data-ttu-id="22d35-165">Po provedení cloudový snímek přechodný klon, bude výsledný klonování *trvalé* klonování.</span><span class="sxs-lookup"><span data-stu-id="22d35-165">After you take a cloud snapshot of a transient clone, the resulting clone will be a *permanent* clone.</span></span> <span data-ttu-id="22d35-166">Během tohoto procesu se vytvoří kopie dat v cloudu a čas zkopírovat tato data je určena velikostí dat a Azure latenci (Toto je kopie Azure do Azure).</span><span class="sxs-lookup"><span data-stu-id="22d35-166">During this process, a copy of the data is created on the cloud and the time to copy this data is determined by the size of the data and the Azure latencies (this is an Azure-to-Azure copy).</span></span> <span data-ttu-id="22d35-167">Tento proces může trvat dnů, týdnů.</span><span class="sxs-lookup"><span data-stu-id="22d35-167">This process can take days to weeks.</span></span> <span data-ttu-id="22d35-168">Přechodný klon tímto způsobem se změní na trvalé klonování a nemá všechny odkazy na původní data svazku, která byla naklonována ze.</span><span class="sxs-lookup"><span data-stu-id="22d35-168">The transient clone becomes a permanent clone this way and doesn’t have any references to the original volume data that it was cloned from.</span></span> 

## <a name="scenarios-for-transient-and-permanent-clones"></a><span data-ttu-id="22d35-169">Scénáře pro přechodný a trvalé klony</span><span class="sxs-lookup"><span data-stu-id="22d35-169">Scenarios for transient and permanent clones</span></span>
<span data-ttu-id="22d35-170">Následující části popisují příklad situacích, ve kterých může být použit klony přechodný a trvalé.</span><span class="sxs-lookup"><span data-stu-id="22d35-170">The following sections describe example situations in which transient and permanent clones can be used.</span></span>

### <a name="item-level-recovery-with-a-transient-clone"></a><span data-ttu-id="22d35-171">Obnovení na úrovni položek s přechodný klon</span><span class="sxs-lookup"><span data-stu-id="22d35-171">Item-level recovery with a transient clone</span></span>
<span data-ttu-id="22d35-172">Budete muset obnovit soubor s jedním. roku starý Microsoft PowerPoint.</span><span class="sxs-lookup"><span data-stu-id="22d35-172">You need to recover a one-year-old Microsoft PowerPoint presentation file.</span></span> <span data-ttu-id="22d35-173">Váš správce IT identifikuje konkrétní zálohování z toto časové období a pak filtry svazku.</span><span class="sxs-lookup"><span data-stu-id="22d35-173">Your IT administrator identifies the specific backup from that time frame, and then filters the volume.</span></span> <span data-ttu-id="22d35-174">Správce pak provede klonování svazku, vyhledá soubor, který hledáte a poskytuje vám.</span><span class="sxs-lookup"><span data-stu-id="22d35-174">The administrator then clones the volume, locates the file that you are looking for, and provides it to you.</span></span> <span data-ttu-id="22d35-175">V tomto scénáři se používá přechodný klonu.</span><span class="sxs-lookup"><span data-stu-id="22d35-175">In this scenario, a transient clone is used.</span></span> 

<span data-ttu-id="22d35-176">![Dostupné video](./media/storsimple-clone-volume-u2/Video_icon.png) **Dostupné video**</span><span class="sxs-lookup"><span data-stu-id="22d35-176">![Video available](./media/storsimple-clone-volume-u2/Video_icon.png) **Video available**</span></span>

<span data-ttu-id="22d35-177">Pokud chcete přehrát video, které ukazuje, jak můžete použít klonu a funkce v zařízení StorSimple obnovit odstraněné soubory obnovit, klikněte na tlačítko [zde](https://azure.microsoft.com/documentation/videos/storsimple-recover-deleted-files-with-storsimple/).</span><span class="sxs-lookup"><span data-stu-id="22d35-177">To watch a video that demonstrates how you can use the clone and restore features in StorSimple to recover deleted files, click [here](https://azure.microsoft.com/documentation/videos/storsimple-recover-deleted-files-with-storsimple/).</span></span>

### <a name="testing-in-the-production-environment-with-a-permanent-clone"></a><span data-ttu-id="22d35-178">Testování v produkčním prostředí s trvalé klon</span><span class="sxs-lookup"><span data-stu-id="22d35-178">Testing in the production environment with a permanent clone</span></span>
<span data-ttu-id="22d35-179">Je třeba ověřit chyby testování v produkčním prostředí.</span><span class="sxs-lookup"><span data-stu-id="22d35-179">You need to verify a testing bug in the production environment.</span></span> <span data-ttu-id="22d35-180">Vytvořit klon svazku v provozním prostředí a pak proveďte cloudový snímek Tenhle klon. k vytvoření klonovaného svazek nezávislé.</span><span class="sxs-lookup"><span data-stu-id="22d35-180">You create a clone of the volume in the production environment and then take a cloud snapshot of this clone to create an independent cloned volume.</span></span> <span data-ttu-id="22d35-181">V tomto scénáři se používá trvalá klonu.</span><span class="sxs-lookup"><span data-stu-id="22d35-181">In this scenario, a permanent clone is used.</span></span>  

## <a name="next-steps"></a><span data-ttu-id="22d35-182">Další kroky</span><span class="sxs-lookup"><span data-stu-id="22d35-182">Next steps</span></span>
* <span data-ttu-id="22d35-183">Zjistěte, jak [obnovit svazek StorSimple ze zálohovacího skladu](storsimple-restore-from-backup-set-u2.md).</span><span class="sxs-lookup"><span data-stu-id="22d35-183">Learn how to [restore a StorSimple volume from a backup set](storsimple-restore-from-backup-set-u2.md).</span></span>
* <span data-ttu-id="22d35-184">Zjistěte, jak [použít službu StorSimple Manager ke správě zařízení StorSimple](storsimple-manager-service-administration.md).</span><span class="sxs-lookup"><span data-stu-id="22d35-184">Learn how to [use the StorSimple Manager service to administer your StorSimple device](storsimple-manager-service-administration.md).</span></span>

