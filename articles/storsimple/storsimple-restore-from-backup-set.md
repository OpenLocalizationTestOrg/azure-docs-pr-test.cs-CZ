---
title: "aaaRestore svazek StorSimple ze zálohy | Microsoft Docs"
description: "Vysvětluje, jak toouse hello toorestore stránky zálohování katalogu služby StorSimple Manager svazek StorSimple ze zálohovacího skladu."
services: storsimple
documentationcenter: NA
author: alkohli
manager: carmonm
editor: 
ms.assetid: b979782e-3184-4465-ad5f-e4516a5885d2
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 08/17/2016
ms.author: alkohli
ms.openlocfilehash: e0efa74b14603be41af0cfc5400de3c39ab8824e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="restore-a-storsimple-volume-from-a-backup-set"></a><span data-ttu-id="5ecff-103">Obnovit svazek StorSimple ze zálohovacího skladu</span><span class="sxs-lookup"><span data-stu-id="5ecff-103">Restore a StorSimple volume from a backup set</span></span>
[!INCLUDE [storsimple-version-selector-restore-from-backup](../../includes/storsimple-version-selector-restore-from-backup.md)]

## <a name="overview"></a><span data-ttu-id="5ecff-104">Přehled</span><span class="sxs-lookup"><span data-stu-id="5ecff-104">Overview</span></span>
<span data-ttu-id="5ecff-105">Hello **zálohování katalogu** stránka zobrazuje všechny zálohovací sklady hello, které vytvářejí, když jsou provedeny ruční nebo automatické zálohy.</span><span class="sxs-lookup"><span data-stu-id="5ecff-105">hello **Backup Catalog** page displays all hello backup sets that are created when manual or automated backups are taken.</span></span> <span data-ttu-id="5ecff-106">Můžete použít tuto stránku toolist všechny hello zálohy pro zásady zálohování nebo svazek, vyberte nebo odstranit zálohy, nebo použít zálohování toorestore nebo klonování svazku.</span><span class="sxs-lookup"><span data-stu-id="5ecff-106">You can use this page toolist all hello backups for a backup policy or a volume, select or delete backups, or use a backup toorestore or clone a volume.</span></span>

 ![Zálohování stránky katalogu](./media/storsimple-restore-from-backup-set/HCS_BackupCatalog.png)

<span data-ttu-id="5ecff-108">Tento kurz vysvětluje, jak toouse hello **zálohování katalogu** stránky toorestore svazek v zařízení ze zálohovacího skladu.</span><span class="sxs-lookup"><span data-stu-id="5ecff-108">This tutorial explains how toouse hello **Backup Catalog** page toorestore a volume on your device from a backup set.</span></span>

## <a name="how-toouse-hello-backup-catalog"></a><span data-ttu-id="5ecff-109">Jak toouse hello zálohování katalogu</span><span class="sxs-lookup"><span data-stu-id="5ecff-109">How toouse hello backup catalog</span></span>
<span data-ttu-id="5ecff-110">Hello **zálohování katalogu** stránka obsahuje dotaz, který vám pomůže toonarrow výběr zálohovacího skladu.</span><span class="sxs-lookup"><span data-stu-id="5ecff-110">hello **Backup Catalog** page provides a query that helps you toonarrow your backup set selection.</span></span> <span data-ttu-id="5ecff-111">Můžete filtrovat hello zálohování sad, které jsou načteny podle hello následující parametry:</span><span class="sxs-lookup"><span data-stu-id="5ecff-111">You can filter hello backup sets that are retrieved based on hello following parameters:</span></span>

* <span data-ttu-id="5ecff-112">**Zařízení** – hello zařízení, na které hello zálohovací sklad vytvořen.</span><span class="sxs-lookup"><span data-stu-id="5ecff-112">**Device** – hello device on which hello backup set was created.</span></span>
* <span data-ttu-id="5ecff-113">**Zásady zálohování** nebo **svazku** – hello zásady zálohování nebo svazku přidruženém k tohoto zálohovacího skladu.</span><span class="sxs-lookup"><span data-stu-id="5ecff-113">**Backup policy** or **volume** – hello backup policy or volume associated with this backup set.</span></span>
* <span data-ttu-id="5ecff-114">**Z** a **k** – hello rozsah data a času v okamžiku vytvoření zálohovacího skladu hello.</span><span class="sxs-lookup"><span data-stu-id="5ecff-114">**From** and **To** – hello date and time range when hello backup set was created.</span></span>

<span data-ttu-id="5ecff-115">Hello filtrované zálohovací sklady jsou pak v tabulce podle hello následující atributy:</span><span class="sxs-lookup"><span data-stu-id="5ecff-115">hello filtered backup sets are then tabulated based on hello following attributes:</span></span>

* <span data-ttu-id="5ecff-116">**Název** – hello název zásady zálohování hello nebo svazku přidruženém k hello zálohovacího skladu.</span><span class="sxs-lookup"><span data-stu-id="5ecff-116">**Name** – hello name of hello backup policy or volume associated with hello backup set.</span></span>
* <span data-ttu-id="5ecff-117">**Velikost** – hello skutečná velikost hello zálohovacího skladu.</span><span class="sxs-lookup"><span data-stu-id="5ecff-117">**Size** – hello actual size of hello backup set.</span></span>
* <span data-ttu-id="5ecff-118">**Vytvořit v** – hello datum a čas, kdy byla vytvořena hello zálohy.</span><span class="sxs-lookup"><span data-stu-id="5ecff-118">**Created on** – hello date and time when hello backups were created.</span></span> 
* <span data-ttu-id="5ecff-119">**Typ** – zálohovací sklady může být místní snímky nebo cloudových snímků.</span><span class="sxs-lookup"><span data-stu-id="5ecff-119">**Type** – Backup sets can be local snapshots or cloud snapshots.</span></span> <span data-ttu-id="5ecff-120">Místní snímek je zálohování všech dat uložených místně na zařízení hello svazku, zatímco cloudový snímek odkazuje toohello zálohování svazku dat umístěných v cloudu hello.</span><span class="sxs-lookup"><span data-stu-id="5ecff-120">A local snapshot is a backup of all your volume data stored locally on hello device, whereas a cloud snapshot refers toohello backup of volume data residing in hello cloud.</span></span> <span data-ttu-id="5ecff-121">Místní snímky poskytují rychlejší přístup, že jsou pro záleží na odolnosti dat zvolena cloudových snímků.</span><span class="sxs-lookup"><span data-stu-id="5ecff-121">Local snapshots provide faster access, whereas cloud snapshots are chosen for data resiliency.</span></span>
* <span data-ttu-id="5ecff-122">**Zahájit** – hello zálohy lze inicializovat automaticky podle plánu tooa nebo ručně uživatelem.</span><span class="sxs-lookup"><span data-stu-id="5ecff-122">**Initiated by** – hello backups can be initiated automatically according tooa schedule or manually by a user.</span></span> <span data-ttu-id="5ecff-123">(Můžete použít zálohování tooschedule zásady zálohování.</span><span class="sxs-lookup"><span data-stu-id="5ecff-123">(You can use a backup policy tooschedule backups.</span></span> <span data-ttu-id="5ecff-124">Alternativně můžete použít hello **provést zálohování** možnost tootake interaktivní zálohování.)</span><span class="sxs-lookup"><span data-stu-id="5ecff-124">Alternatively, you can use hello **Take backup** option tootake an interactive backup.)</span></span>

## <a name="how-toorestore-your-storsimple-volume-from-a-backup"></a><span data-ttu-id="5ecff-125">Jak toorestore svazek StorSimple ze zálohy</span><span class="sxs-lookup"><span data-stu-id="5ecff-125">How toorestore your StorSimple volume from a backup</span></span>
<span data-ttu-id="5ecff-126">Můžete použít hello **zálohování katalogu** stránka toorestore svazek StorSimple ze konkrétní zálohy.</span><span class="sxs-lookup"><span data-stu-id="5ecff-126">You can use hello **Backup Catalog** page toorestore your StorSimple volume from a specific backup.</span></span> 

> [!WARNING]
> <span data-ttu-id="5ecff-127">Obnovení ze zálohy nahradí existující svazky hello ze zálohy hello.</span><span class="sxs-lookup"><span data-stu-id="5ecff-127">Restoring from a backup will replace hello existing volumes from hello backup.</span></span> <span data-ttu-id="5ecff-128">To může způsobit ztrátu hello všechna data, která byla zapsána po hello zálohy.</span><span class="sxs-lookup"><span data-stu-id="5ecff-128">This may cause hello loss of any data that was written after hello backup was taken.</span></span>
> 
> 

<span data-ttu-id="5ecff-129">Než zahájíte obnovení ve svazku, zajistěte, aby byl hello svazek offline.</span><span class="sxs-lookup"><span data-stu-id="5ecff-129">Before you initiate a restore on a volume, ensure that hello volume is offline.</span></span> <span data-ttu-id="5ecff-130">Budete potřebovat tootake hello svazku do offline režimu na hostiteli hello nejprve a pak hello zařízení.</span><span class="sxs-lookup"><span data-stu-id="5ecff-130">You will need tootake hello volume offline on hello host first and then hello device.</span></span> <span data-ttu-id="5ecff-131">Postupujte podle kroků hello v [do offline režimu svazku](storsimple-manage-volumes.md#take-a-volume-offline).</span><span class="sxs-lookup"><span data-stu-id="5ecff-131">Follow hello steps in [Take a volume offline](storsimple-manage-volumes.md#take-a-volume-offline).</span></span> <span data-ttu-id="5ecff-132">Proveďte následující kroky toorestore svazek ze zálohovacího skladu hello.</span><span class="sxs-lookup"><span data-stu-id="5ecff-132">Perform hello following steps toorestore a volume from a backup set.</span></span>

### <a name="toorestore-from-a-backup-set"></a><span data-ttu-id="5ecff-133">toorestore ze zálohovacího skladu</span><span class="sxs-lookup"><span data-stu-id="5ecff-133">toorestore from a backup set</span></span>
1. <span data-ttu-id="5ecff-134">Na stránce služby StorSimple Manager hello, klikněte na hello **katalog zálohování** kartě.</span><span class="sxs-lookup"><span data-stu-id="5ecff-134">On hello StorSimple Manager service page, click hello **Backup catalog** tab.</span></span>
   
    ![Zálohování katalogu](./media/storsimple-restore-from-backup-set/HCS_Restore.png)
2. <span data-ttu-id="5ecff-136">Vyberte zálohovací sklad následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="5ecff-136">Select a backup set as follows:</span></span>
   
   1. <span data-ttu-id="5ecff-137">Vyberte příslušné zařízení hello.</span><span class="sxs-lookup"><span data-stu-id="5ecff-137">Select hello appropriate device.</span></span>
   2. <span data-ttu-id="5ecff-138">V rozevíracím seznamu hello výběru hello svazek nebo zálohování zásady zálohování hello chcete tooselect.</span><span class="sxs-lookup"><span data-stu-id="5ecff-138">In hello drop-down list, choose hello volume or backup policy for hello backup that you wish tooselect.</span></span>
   3. <span data-ttu-id="5ecff-139">Zadejte časové rozmezí hello.</span><span class="sxs-lookup"><span data-stu-id="5ecff-139">Specify hello time range.</span></span>
   4. <span data-ttu-id="5ecff-140">Klikněte na ikonu zaškrtnutí hello</span><span class="sxs-lookup"><span data-stu-id="5ecff-140">Click hello check icon</span></span> ![ikona zaškrtnutí](./media/storsimple-restore-from-backup-set/HCS_CheckIcon.png) <span data-ttu-id="5ecff-142">tooexecute tento dotaz.</span><span class="sxs-lookup"><span data-stu-id="5ecff-142">tooexecute this query.</span></span>
      
      <span data-ttu-id="5ecff-143">Hello zálohování přidružená k zásadě hello vybraný svazek nebo zálohování by se zobrazit v seznamu hello zálohovací sklady.</span><span class="sxs-lookup"><span data-stu-id="5ecff-143">hello backups associated with hello selected volume or backup policy should appear in hello list of backup sets.</span></span>
3. <span data-ttu-id="5ecff-144">Rozbalte hello zálohovacího skladu tooview hello přidružené svazky.</span><span class="sxs-lookup"><span data-stu-id="5ecff-144">Expand hello backup set tooview hello associated volumes.</span></span> <span data-ttu-id="5ecff-145">Tyto svazky musí být převedeno do režimu offline v hostiteli hello a zařízení, než bude možné obnovit.</span><span class="sxs-lookup"><span data-stu-id="5ecff-145">These volumes must be taken offline on hello host and device before you can restore them.</span></span> <span data-ttu-id="5ecff-146">Postupujte podle kroků hello v [do offline režimu svazku](storsimple-manage-volumes.md#take-a-volume-offline).</span><span class="sxs-lookup"><span data-stu-id="5ecff-146">Follow hello steps in [Take a volume offline](storsimple-manage-volumes.md#take-a-volume-offline).</span></span>
   
   > [!IMPORTANT]
   > <span data-ttu-id="5ecff-147">Ujistěte se, že jste provedli hello svazky do offline režimu na hostiteli hello nejprve, před provedením hello svazky do režimu offline v zařízení hello.</span><span class="sxs-lookup"><span data-stu-id="5ecff-147">Make sure that you have taken hello volumes offline on hello host first, before you take hello volumes offline on hello device.</span></span> <span data-ttu-id="5ecff-148">Pokud neprovedete offline hello svazky na hostiteli hello, může potenciálně vést k poškození toodata.</span><span class="sxs-lookup"><span data-stu-id="5ecff-148">If you do not take hello volumes offline on hello host, it could potentially lead toodata corruption.</span></span>
   > 
   > 
4. <span data-ttu-id="5ecff-149">Vyberte zálohovacího skladu.</span><span class="sxs-lookup"><span data-stu-id="5ecff-149">Select a backup set.</span></span> <span data-ttu-id="5ecff-150">Klikněte na tlačítko **obnovení** v hello dolní části stránky hello.</span><span class="sxs-lookup"><span data-stu-id="5ecff-150">Click **Restore** at hello bottom of hello page.</span></span>
5. <span data-ttu-id="5ecff-151">Zobrazí se výzva k potvrzení.</span><span class="sxs-lookup"><span data-stu-id="5ecff-151">You will be prompted for confirmation.</span></span> 
   
    ![Stránka potvrzení](./media/storsimple-restore-from-backup-set/HCS_ConfirmRestore.png)
6. <span data-ttu-id="5ecff-153">Zkontrolujte informace hello obnovení a klikněte na ikonu zaškrtnutí hello ![ikona zaškrtnutí](./media/storsimple-restore-from-backup-set/HCS_CheckIcon.png).</span><span class="sxs-lookup"><span data-stu-id="5ecff-153">Review hello restore information and click hello check icon ![check icon](./media/storsimple-restore-from-backup-set/HCS_CheckIcon.png).</span></span> <span data-ttu-id="5ecff-154">Tím zahájíte obnovení úlohu, která můžete zobrazit pomocí přístupu k hello **úlohy** stránky.</span><span class="sxs-lookup"><span data-stu-id="5ecff-154">This will initiate a restore job that you can view by accessing hello **Jobs** page.</span></span> 
7. <span data-ttu-id="5ecff-155">Po dokončení obnovení hello můžete ověřit, že hello obsah svazků jsou nahrazovány svazků ze zálohy hello.</span><span class="sxs-lookup"><span data-stu-id="5ecff-155">After hello restore is complete, you can verify that hello contents of your volumes are replaced by volumes from hello backup.</span></span>

<span data-ttu-id="5ecff-156">![Dostupné video](./media/storsimple-restore-from-backup-set/Video_icon.png)**Dostupné video**</span><span class="sxs-lookup"><span data-stu-id="5ecff-156">![Video available](./media/storsimple-restore-from-backup-set/Video_icon.png) **Video available**</span></span>

<span data-ttu-id="5ecff-157">Klikněte na tlačítko toowatch video, které ukazuje, jak můžete použít klonu hello a obnovení funkcí v zařízení StorSimple toorecover odstranit soubory, [zde](https://azure.microsoft.com/documentation/videos/storsimple-recover-deleted-files-with-storsimple/).</span><span class="sxs-lookup"><span data-stu-id="5ecff-157">toowatch a video that demonstrates how you can use hello clone and restore features in StorSimple toorecover deleted files, click [here](https://azure.microsoft.com/documentation/videos/storsimple-recover-deleted-files-with-storsimple/).</span></span>

## <a name="next-steps"></a><span data-ttu-id="5ecff-158">Další kroky</span><span class="sxs-lookup"><span data-stu-id="5ecff-158">Next steps</span></span>
* <span data-ttu-id="5ecff-159">Zjistěte, jak příliš[svazky spravovat zařízení StorSimple](storsimple-manage-volumes.md).</span><span class="sxs-lookup"><span data-stu-id="5ecff-159">Learn how too[Manage StorSimple volumes](storsimple-manage-volumes.md).</span></span>
* <span data-ttu-id="5ecff-160">Zjistěte, jak příliš[použití hello tooadminister služby StorSimple Manager zařízení StorSimple](storsimple-manager-service-administration.md).</span><span class="sxs-lookup"><span data-stu-id="5ecff-160">Learn how too[use hello StorSimple Manager service tooadminister your StorSimple device](storsimple-manager-service-administration.md).</span></span>

