---
title: "Obnovit svazek StorSimple ze zálohy | Microsoft Docs"
description: "Vysvětluje, jak používat stránky zálohování katalogu služby StorSimple Manager obnovit svazek StorSimple ze zálohovacího skladu."
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
ms.openlocfilehash: 12484338f5b4d489604d70a657ef0992b6267297
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="restore-a-storsimple-volume-from-a-backup-set"></a><span data-ttu-id="1b653-103">Obnovit svazek StorSimple ze zálohovacího skladu</span><span class="sxs-lookup"><span data-stu-id="1b653-103">Restore a StorSimple volume from a backup set</span></span>
[!INCLUDE [storsimple-version-selector-restore-from-backup](../../includes/storsimple-version-selector-restore-from-backup.md)]

## <a name="overview"></a><span data-ttu-id="1b653-104">Přehled</span><span class="sxs-lookup"><span data-stu-id="1b653-104">Overview</span></span>
<span data-ttu-id="1b653-105">**Zálohování katalogu** stránka zobrazuje všechny zálohovací sklady, které vytvářejí, když jsou provedeny ruční nebo automatické zálohy.</span><span class="sxs-lookup"><span data-stu-id="1b653-105">The **Backup Catalog** page displays all the backup sets that are created when manual or automated backups are taken.</span></span> <span data-ttu-id="1b653-106">Můžete tato stránka slouží k zobrazení seznamu všech zálohy pro zásady zálohování nebo svazek, vyberte nebo odstranit zálohy, nebo použít zálohu k obnovení nebo klonování svazku.</span><span class="sxs-lookup"><span data-stu-id="1b653-106">You can use this page to list all the backups for a backup policy or a volume, select or delete backups, or use a backup to restore or clone a volume.</span></span>

 ![Zálohování stránky katalogu](./media/storsimple-restore-from-backup-set/HCS_BackupCatalog.png)

<span data-ttu-id="1b653-108">Tento kurz vysvětluje, jak používat **zálohování katalogu** stránku obnovit svazek v zařízení ze zálohovacího skladu.</span><span class="sxs-lookup"><span data-stu-id="1b653-108">This tutorial explains how to use the **Backup Catalog** page to restore a volume on your device from a backup set.</span></span>

## <a name="how-to-use-the-backup-catalog"></a><span data-ttu-id="1b653-109">Použití zálohování katalogu</span><span class="sxs-lookup"><span data-stu-id="1b653-109">How to use the backup catalog</span></span>
<span data-ttu-id="1b653-110">**Zálohování katalogu** stránka obsahuje dotaz, který umožňuje zúžit zálohování nastavte výběr.</span><span class="sxs-lookup"><span data-stu-id="1b653-110">The **Backup Catalog** page provides a query that helps you to narrow your backup set selection.</span></span> <span data-ttu-id="1b653-111">Můžete filtrovat zálohovací sklady, které jsou načteny na základě následujících parametrů:</span><span class="sxs-lookup"><span data-stu-id="1b653-111">You can filter the backup sets that are retrieved based on the following parameters:</span></span>

* <span data-ttu-id="1b653-112">**Zařízení** – zařízení, v němž byla vytvořena zálohovacího skladu.</span><span class="sxs-lookup"><span data-stu-id="1b653-112">**Device** – The device on which the backup set was created.</span></span>
* <span data-ttu-id="1b653-113">**Zásady zálohování** nebo **svazku** – zásady zálohování nebo svazku přidruženém k tohoto zálohovacího skladu.</span><span class="sxs-lookup"><span data-stu-id="1b653-113">**Backup policy** or **volume** – The backup policy or volume associated with this backup set.</span></span>
* <span data-ttu-id="1b653-114">**Z** a **k** – rozsah data a času v okamžiku vytvoření zálohovacího skladu.</span><span class="sxs-lookup"><span data-stu-id="1b653-114">**From** and **To** – The date and time range when the backup set was created.</span></span>

<span data-ttu-id="1b653-115">Filtrované zálohovací sklady jsou pak poskytovalo na základě následujících atributů:</span><span class="sxs-lookup"><span data-stu-id="1b653-115">The filtered backup sets are then tabulated based on the following attributes:</span></span>

* <span data-ttu-id="1b653-116">**Název** – název zásady zálohování nebo svazku přidruženém k zálohovacího skladu.</span><span class="sxs-lookup"><span data-stu-id="1b653-116">**Name** – The name of the backup policy or volume associated with the backup set.</span></span>
* <span data-ttu-id="1b653-117">**Velikost** – skutečná velikost zálohovacího skladu.</span><span class="sxs-lookup"><span data-stu-id="1b653-117">**Size** – The actual size of the backup set.</span></span>
* <span data-ttu-id="1b653-118">**Vytvořit v** – datum a čas, kdy byly vytvořeny zálohy.</span><span class="sxs-lookup"><span data-stu-id="1b653-118">**Created on** – The date and time when the backups were created.</span></span> 
* <span data-ttu-id="1b653-119">**Typ** – zálohovací sklady může být místní snímky nebo cloudových snímků.</span><span class="sxs-lookup"><span data-stu-id="1b653-119">**Type** – Backup sets can be local snapshots or cloud snapshots.</span></span> <span data-ttu-id="1b653-120">Místní snímek je zálohování všech dat uložených místně na zařízení, svazku, zatímco cloudový snímek odkazuje na zálohování svazku dat umístěných v cloudu.</span><span class="sxs-lookup"><span data-stu-id="1b653-120">A local snapshot is a backup of all your volume data stored locally on the device, whereas a cloud snapshot refers to the backup of volume data residing in the cloud.</span></span> <span data-ttu-id="1b653-121">Místní snímky poskytují rychlejší přístup, že jsou pro záleží na odolnosti dat zvolena cloudových snímků.</span><span class="sxs-lookup"><span data-stu-id="1b653-121">Local snapshots provide faster access, whereas cloud snapshots are chosen for data resiliency.</span></span>
* <span data-ttu-id="1b653-122">**Zahájit** – zálohy lze inicializovat automaticky podle plánu nebo ručně uživatelem.</span><span class="sxs-lookup"><span data-stu-id="1b653-122">**Initiated by** – The backups can be initiated automatically according to a schedule or manually by a user.</span></span> <span data-ttu-id="1b653-123">(Můžete použít zásady zálohování naplánovat zálohování.</span><span class="sxs-lookup"><span data-stu-id="1b653-123">(You can use a backup policy to schedule backups.</span></span> <span data-ttu-id="1b653-124">Alternativně můžete použít **provést zálohování** možnost provést zálohu interaktivní.)</span><span class="sxs-lookup"><span data-stu-id="1b653-124">Alternatively, you can use the **Take backup** option to take an interactive backup.)</span></span>

## <a name="how-to-restore-your-storsimple-volume-from-a-backup"></a><span data-ttu-id="1b653-125">Jak obnovit svazek StorSimple ze zálohy</span><span class="sxs-lookup"><span data-stu-id="1b653-125">How to restore your StorSimple volume from a backup</span></span>
<span data-ttu-id="1b653-126">Můžete použít **zálohování katalogu** stránku obnovit svazek StorSimple ze konkrétní zálohy.</span><span class="sxs-lookup"><span data-stu-id="1b653-126">You can use the **Backup Catalog** page to restore your StorSimple volume from a specific backup.</span></span> 

> [!WARNING]
> <span data-ttu-id="1b653-127">Obnovení ze zálohy nahradí existující svazky ze zálohy.</span><span class="sxs-lookup"><span data-stu-id="1b653-127">Restoring from a backup will replace the existing volumes from the backup.</span></span> <span data-ttu-id="1b653-128">To může způsobit ztrátu všechna data, která byla zapsána po vytvoření zálohy.</span><span class="sxs-lookup"><span data-stu-id="1b653-128">This may cause the loss of any data that was written after the backup was taken.</span></span>
> 
> 

<span data-ttu-id="1b653-129">Než zahájíte obnovení ve svazku, ujistěte se, že je svazek offline.</span><span class="sxs-lookup"><span data-stu-id="1b653-129">Before you initiate a restore on a volume, ensure that the volume is offline.</span></span> <span data-ttu-id="1b653-130">Budete muset provést offline svazek na hostiteli první a pak zařízení.</span><span class="sxs-lookup"><span data-stu-id="1b653-130">You will need to take the volume offline on the host first and then the device.</span></span> <span data-ttu-id="1b653-131">Postupujte podle kroků v [do offline režimu svazku](storsimple-manage-volumes.md#take-a-volume-offline).</span><span class="sxs-lookup"><span data-stu-id="1b653-131">Follow the steps in [Take a volume offline](storsimple-manage-volumes.md#take-a-volume-offline).</span></span> <span data-ttu-id="1b653-132">Proveďte následující kroky k obnovení svazku ze zálohovacího skladu.</span><span class="sxs-lookup"><span data-stu-id="1b653-132">Perform the following steps to restore a volume from a backup set.</span></span>

### <a name="to-restore-from-a-backup-set"></a><span data-ttu-id="1b653-133">Obnovení ze zálohovacího skladu</span><span class="sxs-lookup"><span data-stu-id="1b653-133">To restore from a backup set</span></span>
1. <span data-ttu-id="1b653-134">Na stránce služby StorSimple Manager, klepněte **katalog zálohování** kartě.</span><span class="sxs-lookup"><span data-stu-id="1b653-134">On the StorSimple Manager service page, click the **Backup catalog** tab.</span></span>
   
    ![Zálohování katalogu](./media/storsimple-restore-from-backup-set/HCS_Restore.png)
2. <span data-ttu-id="1b653-136">Vyberte zálohovací sklad následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="1b653-136">Select a backup set as follows:</span></span>
   
   1. <span data-ttu-id="1b653-137">Vyberte příslušné zařízení.</span><span class="sxs-lookup"><span data-stu-id="1b653-137">Select the appropriate device.</span></span>
   2. <span data-ttu-id="1b653-138">V rozevíracím seznamu vyberte svazek nebo zálohování zásady pro zálohu, kterou chcete vybrat.</span><span class="sxs-lookup"><span data-stu-id="1b653-138">In the drop-down list, choose the volume or backup policy for the backup that you wish to select.</span></span>
   3. <span data-ttu-id="1b653-139">Zadejte časový rozsah.</span><span class="sxs-lookup"><span data-stu-id="1b653-139">Specify the time range.</span></span>
   4. <span data-ttu-id="1b653-140">Klikněte na ikonu zaškrtnutí</span><span class="sxs-lookup"><span data-stu-id="1b653-140">Click the check icon</span></span> ![ikona zaškrtnutí](./media/storsimple-restore-from-backup-set/HCS_CheckIcon.png) <span data-ttu-id="1b653-142">k provedení tohoto dotazu.</span><span class="sxs-lookup"><span data-stu-id="1b653-142">to execute this query.</span></span>
      
      <span data-ttu-id="1b653-143">Zálohování přidružené k vybranému svazku nebo zásady zálohování by se měla objevit v seznamu sad záloh.</span><span class="sxs-lookup"><span data-stu-id="1b653-143">The backups associated with the selected volume or backup policy should appear in the list of backup sets.</span></span>
3. <span data-ttu-id="1b653-144">Rozbalte zálohovací sklad zobrazíte přidružené svazky.</span><span class="sxs-lookup"><span data-stu-id="1b653-144">Expand the backup set to view the associated volumes.</span></span> <span data-ttu-id="1b653-145">Tyto svazky musí být převedeno do režimu offline v hostiteli a zařízení, než bude možné obnovit.</span><span class="sxs-lookup"><span data-stu-id="1b653-145">These volumes must be taken offline on the host and device before you can restore them.</span></span> <span data-ttu-id="1b653-146">Postupujte podle kroků v [do offline režimu svazku](storsimple-manage-volumes.md#take-a-volume-offline).</span><span class="sxs-lookup"><span data-stu-id="1b653-146">Follow the steps in [Take a volume offline](storsimple-manage-volumes.md#take-a-volume-offline).</span></span>
   
   > [!IMPORTANT]
   > <span data-ttu-id="1b653-147">Ujistěte se, že jste provedli svazky do offline režimu na hostiteli nejprve, před provedením svazky do režimu offline v zařízení.</span><span class="sxs-lookup"><span data-stu-id="1b653-147">Make sure that you have taken the volumes offline on the host first, before you take the volumes offline on the device.</span></span> <span data-ttu-id="1b653-148">Pokud neprovedete offline svazky na hostiteli, může potenciálně vést k poškození dat.</span><span class="sxs-lookup"><span data-stu-id="1b653-148">If you do not take the volumes offline on the host, it could potentially lead to data corruption.</span></span>
   > 
   > 
4. <span data-ttu-id="1b653-149">Vyberte zálohovacího skladu.</span><span class="sxs-lookup"><span data-stu-id="1b653-149">Select a backup set.</span></span> <span data-ttu-id="1b653-150">Klikněte na tlačítko **obnovení** v dolní části stránky.</span><span class="sxs-lookup"><span data-stu-id="1b653-150">Click **Restore** at the bottom of the page.</span></span>
5. <span data-ttu-id="1b653-151">Zobrazí se výzva k potvrzení.</span><span class="sxs-lookup"><span data-stu-id="1b653-151">You will be prompted for confirmation.</span></span> 
   
    ![Stránka potvrzení](./media/storsimple-restore-from-backup-set/HCS_ConfirmRestore.png)
6. <span data-ttu-id="1b653-153">Zkontrolovat informace o obnovení a klikněte na ikonu zaškrtnutí ![ikona zaškrtnutí](./media/storsimple-restore-from-backup-set/HCS_CheckIcon.png).</span><span class="sxs-lookup"><span data-stu-id="1b653-153">Review the restore information and click the check icon ![check icon](./media/storsimple-restore-from-backup-set/HCS_CheckIcon.png).</span></span> <span data-ttu-id="1b653-154">Tím zahájíte obnovení úlohu, která si můžete zobrazit přístup **úlohy** stránky.</span><span class="sxs-lookup"><span data-stu-id="1b653-154">This will initiate a restore job that you can view by accessing the **Jobs** page.</span></span> 
7. <span data-ttu-id="1b653-155">Po dokončení obnovení můžete ověřit, že obsah svazků jsou nahrazovány svazků ze zálohy.</span><span class="sxs-lookup"><span data-stu-id="1b653-155">After the restore is complete, you can verify that the contents of your volumes are replaced by volumes from the backup.</span></span>

<span data-ttu-id="1b653-156">![Dostupné video](./media/storsimple-restore-from-backup-set/Video_icon.png) **Dostupné video**</span><span class="sxs-lookup"><span data-stu-id="1b653-156">![Video available](./media/storsimple-restore-from-backup-set/Video_icon.png) **Video available**</span></span>

<span data-ttu-id="1b653-157">Pokud chcete přehrát video, které ukazuje, jak můžete použít klonu a funkce v zařízení StorSimple obnovit odstraněné soubory obnovit, klikněte na tlačítko [zde](https://azure.microsoft.com/documentation/videos/storsimple-recover-deleted-files-with-storsimple/).</span><span class="sxs-lookup"><span data-stu-id="1b653-157">To watch a video that demonstrates how you can use the clone and restore features in StorSimple to recover deleted files, click [here](https://azure.microsoft.com/documentation/videos/storsimple-recover-deleted-files-with-storsimple/).</span></span>

## <a name="next-steps"></a><span data-ttu-id="1b653-158">Další kroky</span><span class="sxs-lookup"><span data-stu-id="1b653-158">Next steps</span></span>
* <span data-ttu-id="1b653-159">Zjistěte, jak [svazky spravovat zařízení StorSimple](storsimple-manage-volumes.md).</span><span class="sxs-lookup"><span data-stu-id="1b653-159">Learn how to [Manage StorSimple volumes](storsimple-manage-volumes.md).</span></span>
* <span data-ttu-id="1b653-160">Zjistěte, jak [použít službu StorSimple Manager ke správě zařízení StorSimple](storsimple-manager-service-administration.md).</span><span class="sxs-lookup"><span data-stu-id="1b653-160">Learn how to [use the StorSimple Manager service to administer your StorSimple device](storsimple-manager-service-administration.md).</span></span>

