---
title: "Spravovat katalog záloh StorSimple | Microsoft Docs"
description: "Vysvětluje, jak používat stránky zálohování katalogu služby StorSimple Manager k seznamu, vyberte a odstraňte zálohovací sklady pro svazek."
services: storsimple
documentationcenter: NA
author: SharS
manager: carmonm
editor: 
ms.assetid: ad81bee9-fe43-40b3-a384-b15fb274ecd9
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 04/28/2016
ms.author: v-sharos
ms.openlocfilehash: 5ee9855e1428c7a2d871d9c215d302c5c3b7101a
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="use-the-storsimple-manager-service-to-manage-your-backup-catalog"></a><span data-ttu-id="f0782-103">Použít službu StorSimple Manager ke správě katalog záloh</span><span class="sxs-lookup"><span data-stu-id="f0782-103">Use the StorSimple Manager service to manage your backup catalog</span></span>
## <a name="overview"></a><span data-ttu-id="f0782-104">Přehled</span><span class="sxs-lookup"><span data-stu-id="f0782-104">Overview</span></span>
<span data-ttu-id="f0782-105">Služby StorSimple Manager **zálohování katalogu** stránka zobrazuje všechny zálohovací sklady, které vytvářejí, když jsou provedeny ruční nebo plánované zálohy.</span><span class="sxs-lookup"><span data-stu-id="f0782-105">The StorSimple Manager service **Backup Catalog** page displays all the backup sets that are created when manual or scheduled backups are taken.</span></span> <span data-ttu-id="f0782-106">Můžete tato stránka slouží k zobrazení seznamu všech zálohy pro zásady zálohování nebo svazek, vyberte nebo odstranit zálohy, nebo použít zálohu k obnovení nebo klonování svazku.</span><span class="sxs-lookup"><span data-stu-id="f0782-106">You can use this page to list all the backups for a backup policy or a volume, select or delete backups, or use a backup to restore or clone a volume.</span></span>

<span data-ttu-id="f0782-107">Tento kurz vysvětluje, jak zobrazit seznam, vyberte a odstraňte zálohovacího skladu.</span><span class="sxs-lookup"><span data-stu-id="f0782-107">This tutorial explains how to list, select, and delete a backup set.</span></span> <span data-ttu-id="f0782-108">Chcete-li se dozvědět, jak obnovit ze zálohy zařízení, přejděte na [obnovit vaše zařízení ze zálohovacího skladu](storsimple-restore-from-backup-set.md).</span><span class="sxs-lookup"><span data-stu-id="f0782-108">To learn how to restore your device from backup, go to [Restore your device from a backup set](storsimple-restore-from-backup-set.md).</span></span> <span data-ttu-id="f0782-109">Chcete-li se dozvědět, jak vytvořit kopii svazku, přejděte na [klonovat svazek StorSimple](storsimple-clone-volume.md).</span><span class="sxs-lookup"><span data-stu-id="f0782-109">To learn how to clone a volume, go to [Clone a StorSimple volume](storsimple-clone-volume.md).</span></span>

![Zálohování katalogu](./media/storsimple-manage-backup-catalog/backupcatalog.png) 

<span data-ttu-id="f0782-111">**Zálohování katalogu** stránka obsahuje dotaz, chcete-li zúžit zálohování sadu výběr.</span><span class="sxs-lookup"><span data-stu-id="f0782-111">The **Backup Catalog** page provides a query to narrow your backup set selection.</span></span> <span data-ttu-id="f0782-112">Můžete filtrovat zálohovací sklady, které jsou načteny, na základě následujících parametrů:</span><span class="sxs-lookup"><span data-stu-id="f0782-112">You can filter the backup sets that are retrieved, based on the following parameters:</span></span>

* <span data-ttu-id="f0782-113">**Zařízení** – zařízení, v němž byla vytvořena zálohovacího skladu.</span><span class="sxs-lookup"><span data-stu-id="f0782-113">**Device** – The device on which the backup set was created.</span></span>
* <span data-ttu-id="f0782-114">**Zálohování zásady nebo svazek** – zásady zálohování nebo svazku přidruženém k tohoto zálohovacího skladu.</span><span class="sxs-lookup"><span data-stu-id="f0782-114">**Backup Policy or Volume** – The backup policy or volume associated with this backup set.</span></span>
* <span data-ttu-id="f0782-115">**Od a do** – rozsah data a času v okamžiku vytvoření zálohovacího skladu.</span><span class="sxs-lookup"><span data-stu-id="f0782-115">**From and To** – The date and time range when the backup set was created.</span></span>

<span data-ttu-id="f0782-116">Filtrované zálohovací sklady jsou pak poskytovalo na základě následujících atributů:</span><span class="sxs-lookup"><span data-stu-id="f0782-116">The filtered backup sets are then tabulated based on the following attributes:</span></span>

* <span data-ttu-id="f0782-117">**Název** – název zásady zálohování nebo svazku přidruženém k zálohovacího skladu.</span><span class="sxs-lookup"><span data-stu-id="f0782-117">**Name** – The name of the backup policy or volume associated with the backup set.</span></span>
* <span data-ttu-id="f0782-118">**Velikost** – skutečná velikost zálohovacího skladu.</span><span class="sxs-lookup"><span data-stu-id="f0782-118">**Size** – The actual size of the backup set.</span></span>
* <span data-ttu-id="f0782-119">**Vytvořit na** – datum a čas, kdy byly vytvořeny zálohy.</span><span class="sxs-lookup"><span data-stu-id="f0782-119">**Created On** – The date and time when the backups were created.</span></span> 
* <span data-ttu-id="f0782-120">**Typ** – zálohovací sklady může být místní snímky nebo cloudových snímků.</span><span class="sxs-lookup"><span data-stu-id="f0782-120">**Type** – Backup sets can be local snapshots or cloud snapshots.</span></span> <span data-ttu-id="f0782-121">Místní snímek je zálohování všech dat uložených místně na zařízení, svazku, zatímco cloudový snímek odkazuje na zálohování svazku dat umístěných v cloudu.</span><span class="sxs-lookup"><span data-stu-id="f0782-121">A local snapshot is a backup of all your volume data stored locally on the device, whereas a cloud snapshot refers to the backup of volume data residing in the cloud.</span></span> <span data-ttu-id="f0782-122">Místní snímky poskytují rychlejší přístup, že jsou pro záleží na odolnosti dat zvolena cloudových snímků.</span><span class="sxs-lookup"><span data-stu-id="f0782-122">Local snapshots provide faster access, whereas cloud snapshots are chosen for data resiliency.</span></span>
* <span data-ttu-id="f0782-123">**Iniciováno** – zálohy lze inicializovat automaticky podle plánu nebo ručně uživatelem.</span><span class="sxs-lookup"><span data-stu-id="f0782-123">**Initiated By** – The backups can be initiated automatically by a schedule or manually by a user.</span></span> <span data-ttu-id="f0782-124">Zásady zálohování můžete naplánovat zálohování.</span><span class="sxs-lookup"><span data-stu-id="f0782-124">You can use a backup policy to schedule backups.</span></span> <span data-ttu-id="f0782-125">Alternativně můžete použít **provést zálohování** možnost provedení ruční zálohy.</span><span class="sxs-lookup"><span data-stu-id="f0782-125">Alternatively, you can use the **Take backup** option to take a manual backup.</span></span>

## <a name="list-backup-sets-for-a-volume"></a><span data-ttu-id="f0782-126">Seznam zálohovací sklady pro svazek clusteru</span><span class="sxs-lookup"><span data-stu-id="f0782-126">List backup sets for a volume</span></span>
<span data-ttu-id="f0782-127">Pomocí následujících kroků na seznamu všechny zálohy svazku.</span><span class="sxs-lookup"><span data-stu-id="f0782-127">Complete the following steps to list all the backups for a volume.</span></span>

#### <a name="to-list-backup-sets"></a><span data-ttu-id="f0782-128">Do seznamu zálohovací sklady</span><span class="sxs-lookup"><span data-stu-id="f0782-128">To list backup sets</span></span>
1. <span data-ttu-id="f0782-129">Na stránce služby StorSimple Manager, klepněte **katalog zálohování** kartě.</span><span class="sxs-lookup"><span data-stu-id="f0782-129">On the StorSimple Manager service page, click the **Backup catalog** tab.</span></span>
2. <span data-ttu-id="f0782-130">Výběr filtru následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="f0782-130">Filter the selections as follows:</span></span>
   
   1. <span data-ttu-id="f0782-131">Vyberte příslušné zařízení.</span><span class="sxs-lookup"><span data-stu-id="f0782-131">Select the appropriate device.</span></span>
   2. <span data-ttu-id="f0782-132">V rozevíracím seznamu vyberte svazek k zobrazení odpovídající zálohy.</span><span class="sxs-lookup"><span data-stu-id="f0782-132">In the drop-down list, choose a volume to view the corresponding the backups.</span></span>
   3. <span data-ttu-id="f0782-133">Zadejte časový rozsah.</span><span class="sxs-lookup"><span data-stu-id="f0782-133">Specify the time range.</span></span>
   4. <span data-ttu-id="f0782-134">Klikněte na ikonu zaškrtnutí</span><span class="sxs-lookup"><span data-stu-id="f0782-134">Click the check icon</span></span> ![Ikona zaškrtnutí](./media/storsimple-manage-backup-catalog/HCS_CheckIcon.png) <span data-ttu-id="f0782-136">k provedení tohoto dotazu.</span><span class="sxs-lookup"><span data-stu-id="f0782-136">to execute this query.</span></span>
      
      <span data-ttu-id="f0782-137">Zálohování přidružené k vybranému svazku by se zobrazit v seznamu sad záloh.</span><span class="sxs-lookup"><span data-stu-id="f0782-137">The backups associated with the selected volume should appear in the list of backup sets.</span></span>

## <a name="select-a-backup-set"></a><span data-ttu-id="f0782-138">Vyberte zálohovací sklad</span><span class="sxs-lookup"><span data-stu-id="f0782-138">Select a backup set</span></span>
<span data-ttu-id="f0782-139">Pomocí následujících kroků vyberte zálohovacího skladu pro svazek nebo zásady zálohování.</span><span class="sxs-lookup"><span data-stu-id="f0782-139">Complete the following steps to select a backup set for a volume or backup policy.</span></span>

#### <a name="to-select-a-backup-set"></a><span data-ttu-id="f0782-140">Chcete-li vybrat zálohovacího skladu</span><span class="sxs-lookup"><span data-stu-id="f0782-140">To select a backup set</span></span>
1. <span data-ttu-id="f0782-141">Na stránce služby StorSimple Manager, klepněte **katalog zálohování** kartě.</span><span class="sxs-lookup"><span data-stu-id="f0782-141">On the StorSimple Manager service page, click the **Backup catalog** tab.</span></span>
2. <span data-ttu-id="f0782-142">Výběr filtru následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="f0782-142">Filter the selections as follows:</span></span>
   
   1. <span data-ttu-id="f0782-143">Vyberte příslušné zařízení.</span><span class="sxs-lookup"><span data-stu-id="f0782-143">Select the appropriate device.</span></span>
   2. <span data-ttu-id="f0782-144">V rozevíracím seznamu vyberte svazek nebo zálohování zásady pro zálohu, kterou chcete vybrat.</span><span class="sxs-lookup"><span data-stu-id="f0782-144">In the drop-down list, choose the volume or backup policy for the backup that you wish to select.</span></span>
   3. <span data-ttu-id="f0782-145">Zadejte časový rozsah.</span><span class="sxs-lookup"><span data-stu-id="f0782-145">Specify the time range.</span></span>
   4. <span data-ttu-id="f0782-146">Klikněte na ikonu zaškrtnutí</span><span class="sxs-lookup"><span data-stu-id="f0782-146">Click the check icon</span></span> ![Ikona zaškrtnutí](./media/storsimple-manage-backup-catalog/HCS_CheckIcon.png) <span data-ttu-id="f0782-148">k provedení tohoto dotazu.</span><span class="sxs-lookup"><span data-stu-id="f0782-148">to execute this query.</span></span>
      
      <span data-ttu-id="f0782-149">Zálohování přidružené k vybranému svazku nebo zásady zálohování by se měla objevit v seznamu sad záloh.</span><span class="sxs-lookup"><span data-stu-id="f0782-149">The backups associated with the selected volume or backup policy should appear in the list of backup sets.</span></span>
3. <span data-ttu-id="f0782-150">Vyberte a rozbalte zálohovacího skladu.</span><span class="sxs-lookup"><span data-stu-id="f0782-150">Select and expand a backup set.</span></span> <span data-ttu-id="f0782-151">**Obnovení** a **odstranit** možnosti se zobrazí v dolní části stránky.</span><span class="sxs-lookup"><span data-stu-id="f0782-151">The **Restore** and **Delete** options are displayed at the bottom of the page.</span></span> <span data-ttu-id="f0782-152">Můžete provést některé z těchto akcí na zálohovací sklad, který jste vybrali.</span><span class="sxs-lookup"><span data-stu-id="f0782-152">You can perform either of these actions on the backup set that you selected.</span></span>

## <a name="delete-a-backup-set"></a><span data-ttu-id="f0782-153">Odstranit zálohovacího skladu</span><span class="sxs-lookup"><span data-stu-id="f0782-153">Delete a backup set</span></span>
<span data-ttu-id="f0782-154">Odstraňte zálohy, když už chcete zachovat data s ním spojená.</span><span class="sxs-lookup"><span data-stu-id="f0782-154">Delete a backup when you no longer wish to retain the data associated with it.</span></span> <span data-ttu-id="f0782-155">Proveďte následující kroky a odstraňte zálohovacího skladu.</span><span class="sxs-lookup"><span data-stu-id="f0782-155">Perform the following steps to delete a backup set.</span></span>

#### <a name="to-delete-a-backup-set"></a><span data-ttu-id="f0782-156">Chcete-li odstranit zálohovacího skladu</span><span class="sxs-lookup"><span data-stu-id="f0782-156">To delete a backup set</span></span>
1. <span data-ttu-id="f0782-157">Na stránce služby StorSimple Manager, klepněte **katalog zálohování karta**.</span><span class="sxs-lookup"><span data-stu-id="f0782-157">On the StorSimple Manager service page, click the **Backup Catalog tab**.</span></span>
2. <span data-ttu-id="f0782-158">Výběr filtru následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="f0782-158">Filter the selections as follows:</span></span>
   
   1. <span data-ttu-id="f0782-159">Vyberte příslušné zařízení.</span><span class="sxs-lookup"><span data-stu-id="f0782-159">Select the appropriate device.</span></span>
   2. <span data-ttu-id="f0782-160">V rozevíracím seznamu vyberte svazek nebo zálohování zásady pro zálohu, kterou chcete vybrat.</span><span class="sxs-lookup"><span data-stu-id="f0782-160">In the drop-down list, choose the volume or backup policy for the backup that you wish to select.</span></span>
   3. <span data-ttu-id="f0782-161">Zadejte časový rozsah.</span><span class="sxs-lookup"><span data-stu-id="f0782-161">Specify the time range.</span></span>
   4. <span data-ttu-id="f0782-162">Klikněte na ikonu zaškrtnutí</span><span class="sxs-lookup"><span data-stu-id="f0782-162">Click the check icon</span></span> ![Ikona zaškrtnutí](./media/storsimple-manage-backup-catalog/HCS_CheckIcon.png) <span data-ttu-id="f0782-164">k provedení tohoto dotazu.</span><span class="sxs-lookup"><span data-stu-id="f0782-164">to execute this query.</span></span>
      
      <span data-ttu-id="f0782-165">Zálohování přidružené k vybranému svazku nebo zásady zálohování by se měla objevit v seznamu sad záloh.</span><span class="sxs-lookup"><span data-stu-id="f0782-165">The backups associated with the selected volume or backup policy should appear in the list of backup sets.</span></span>
3. <span data-ttu-id="f0782-166">Vyberte a rozbalte zálohovacího skladu.</span><span class="sxs-lookup"><span data-stu-id="f0782-166">Select and expand a backup set.</span></span> <span data-ttu-id="f0782-167">**Obnovení** a **odstranit** možnosti se zobrazí v dolní části stránky.</span><span class="sxs-lookup"><span data-stu-id="f0782-167">The **Restore** and **Delete** options are displayed at the bottom of the page.</span></span> <span data-ttu-id="f0782-168">Klikněte na **Odstranit**.</span><span class="sxs-lookup"><span data-stu-id="f0782-168">Click **Delete**.</span></span>
4. <span data-ttu-id="f0782-169">Při odstranění je v průběhu a kdy má úspěšně dokončena, budete upozorněni.</span><span class="sxs-lookup"><span data-stu-id="f0782-169">You will be notified when the deletion is in progress and when it has successfully finished.</span></span> <span data-ttu-id="f0782-170">Po odstranění probíhá, aktualizujte dotaz na této stránce.</span><span class="sxs-lookup"><span data-stu-id="f0782-170">After the deletion is done, refresh the query on this page.</span></span> <span data-ttu-id="f0782-171">Odstraněné zálohovací sklad se nebude zobrazovat v seznamu sad záloh.</span><span class="sxs-lookup"><span data-stu-id="f0782-171">The deleted backup set will no longer appear in the list of backup sets.</span></span>

## <a name="next-steps"></a><span data-ttu-id="f0782-172">Další kroky</span><span class="sxs-lookup"><span data-stu-id="f0782-172">Next steps</span></span>
* <span data-ttu-id="f0782-173">Zjistěte, jak [zálohování katalog používat k obnovení vašeho zařízení ze zálohovacího skladu](storsimple-restore-from-backup-set.md).</span><span class="sxs-lookup"><span data-stu-id="f0782-173">Learn how to [use the backup catalog to restore your device from a backup set](storsimple-restore-from-backup-set.md).</span></span>
* <span data-ttu-id="f0782-174">Zjistěte, jak [použít službu StorSimple Manager ke správě zařízení StorSimple](storsimple-manager-service-administration.md).</span><span class="sxs-lookup"><span data-stu-id="f0782-174">Learn how to [use the StorSimple Manager service to administer your StorSimple device](storsimple-manager-service-administration.md).</span></span>

