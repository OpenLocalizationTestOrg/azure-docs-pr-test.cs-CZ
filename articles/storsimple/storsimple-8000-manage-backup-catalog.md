---
title: "Spravovat katalog záloh StorSimple | Microsoft Docs"
description: "Vysvětluje, jak pomocí Správce zařízení StorSimple na stránce Katalog zálohování seznamu, vyberte a odstraňte zálohovací sklady."
services: storsimple
documentationcenter: NA
author: alkohli
manager: timlt
editor: 
ms.assetid: 
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 06/29/2017
ms.author: alkohli
ms.openlocfilehash: ac2577c6cd350d6d437d55e61ec73d954cb24893
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="use-the-storsimple-device-manager-service-to-manage-your-backup-catalog"></a><span data-ttu-id="c0a2f-103">Použít službu StorSimple Manager zařízení ke správě katalog záloh</span><span class="sxs-lookup"><span data-stu-id="c0a2f-103">Use the StorSimple Device Manager service to manage your backup catalog</span></span>
## <a name="overview"></a><span data-ttu-id="c0a2f-104">Přehled</span><span class="sxs-lookup"><span data-stu-id="c0a2f-104">Overview</span></span>
<span data-ttu-id="c0a2f-105">Služby StorSimple Manager zařízení **zálohování katalogu** zobrazuje všechny zálohovací sklady, které vytvářejí, když jsou provedeny ruční nebo plánované zálohy.</span><span class="sxs-lookup"><span data-stu-id="c0a2f-105">The StorSimple Device Manager service **Backup Catalog** blade displays all the backup sets that are created when manual or scheduled backups are taken.</span></span> <span data-ttu-id="c0a2f-106">Můžete tato stránka slouží k zobrazení seznamu všech zálohy pro zásady zálohování nebo svazek, vyberte nebo odstranit zálohy, nebo použít zálohu k obnovení nebo klonování svazku.</span><span class="sxs-lookup"><span data-stu-id="c0a2f-106">You can use this page to list all the backups for a backup policy or a volume, select or delete backups, or use a backup to restore or clone a volume.</span></span>

<span data-ttu-id="c0a2f-107">Tento kurz vysvětluje, jak zobrazit seznam, vyberte a odstraňte zálohovacího skladu.</span><span class="sxs-lookup"><span data-stu-id="c0a2f-107">This tutorial explains how to list, select, and delete a backup set.</span></span> <span data-ttu-id="c0a2f-108">Chcete-li se dozvědět, jak obnovit ze zálohy zařízení, přejděte na [obnovit vaše zařízení ze zálohovacího skladu](storsimple-8000-restore-from-backup-set-u2.md).</span><span class="sxs-lookup"><span data-stu-id="c0a2f-108">To learn how to restore your device from backup, go to [Restore your device from a backup set](storsimple-8000-restore-from-backup-set-u2.md).</span></span> <span data-ttu-id="c0a2f-109">Chcete-li se dozvědět, jak vytvořit kopii svazku, přejděte na [klonovat svazek StorSimple](storsimple-8000-clone-volume-u2.md).</span><span class="sxs-lookup"><span data-stu-id="c0a2f-109">To learn how to clone a volume, go to [Clone a StorSimple volume](storsimple-8000-clone-volume-u2.md).</span></span>

![Zálohování katalogu](./media/storsimple-8000-manage-backup-catalog/bucatalog.png) 

<span data-ttu-id="c0a2f-111">**Zálohování katalogu** okno obsahuje dotaz, chcete-li zúžit zálohování sadu výběr.</span><span class="sxs-lookup"><span data-stu-id="c0a2f-111">The **Backup Catalog** blade provides a query to narrow your backup set selection.</span></span> <span data-ttu-id="c0a2f-112">Můžete filtrovat zálohovací sklady, které jsou načteny, na základě následujících parametrů:</span><span class="sxs-lookup"><span data-stu-id="c0a2f-112">You can filter the backup sets that are retrieved, based on the following parameters:</span></span>

* <span data-ttu-id="c0a2f-113">**Zařízení** – zařízení, v němž byla vytvořena zálohovacího skladu.</span><span class="sxs-lookup"><span data-stu-id="c0a2f-113">**Device** – The device on which the backup set was created.</span></span>
* <span data-ttu-id="c0a2f-114">**Zásady zálohování nebo svazku** – zásady zálohování nebo svazku přidruženém k tohoto zálohovacího skladu.</span><span class="sxs-lookup"><span data-stu-id="c0a2f-114">**Backup policy or Volume** – The backup policy or volume associated with this backup set.</span></span>
* <span data-ttu-id="c0a2f-115">**Od a do** – rozsah data a času v okamžiku vytvoření zálohovacího skladu.</span><span class="sxs-lookup"><span data-stu-id="c0a2f-115">**From and To** – The date and time range when the backup set was created.</span></span>

<span data-ttu-id="c0a2f-116">Filtrované zálohovací sklady jsou pak poskytovalo na základě následujících atributů:</span><span class="sxs-lookup"><span data-stu-id="c0a2f-116">The filtered backup sets are then tabulated based on the following attributes:</span></span>

* <span data-ttu-id="c0a2f-117">**Název** – název zásady zálohování nebo svazku přidruženém k zálohovacího skladu.</span><span class="sxs-lookup"><span data-stu-id="c0a2f-117">**Name** – The name of the backup policy or volume associated with the backup set.</span></span>
* <span data-ttu-id="c0a2f-118">**Velikost** – skutečná velikost zálohovacího skladu.</span><span class="sxs-lookup"><span data-stu-id="c0a2f-118">**Size** – The actual size of the backup set.</span></span>
* <span data-ttu-id="c0a2f-119">**Vytvořit na** – datum a čas, kdy byly vytvořeny zálohy.</span><span class="sxs-lookup"><span data-stu-id="c0a2f-119">**Created On** – The date and time when the backups were created.</span></span> 
* <span data-ttu-id="c0a2f-120">**Typ** – zálohovací sklady může být místní snímky nebo cloudových snímků.</span><span class="sxs-lookup"><span data-stu-id="c0a2f-120">**Type** – Backup sets can be local snapshots or cloud snapshots.</span></span> <span data-ttu-id="c0a2f-121">Místní snímek je zálohování všech dat uložených místně na zařízení, svazku, zatímco cloudový snímek odkazuje na zálohování svazku dat umístěných v cloudu.</span><span class="sxs-lookup"><span data-stu-id="c0a2f-121">A local snapshot is a backup of all your volume data stored locally on the device, whereas a cloud snapshot refers to the backup of volume data residing in the cloud.</span></span> <span data-ttu-id="c0a2f-122">Místní snímky poskytují rychlejší přístup, že jsou pro záleží na odolnosti dat zvolena cloudových snímků.</span><span class="sxs-lookup"><span data-stu-id="c0a2f-122">Local snapshots provide faster access, whereas cloud snapshots are chosen for data resiliency.</span></span>
* <span data-ttu-id="c0a2f-123">**Iniciováno** – zálohy lze inicializovat automaticky podle plánu nebo ručně uživatelem.</span><span class="sxs-lookup"><span data-stu-id="c0a2f-123">**Initiated By** – The backups can be initiated automatically by a schedule or manually by a user.</span></span> <span data-ttu-id="c0a2f-124">Zásady zálohování můžete naplánovat zálohování.</span><span class="sxs-lookup"><span data-stu-id="c0a2f-124">You can use a backup policy to schedule backups.</span></span> <span data-ttu-id="c0a2f-125">Alternativně můžete použít **provést zálohování** možnost provedení ruční zálohy.</span><span class="sxs-lookup"><span data-stu-id="c0a2f-125">Alternatively, you can use the **Take backup** option to take a manual backup.</span></span>

## <a name="list-backup-sets-for-a-backup-policy"></a><span data-ttu-id="c0a2f-126">Zálohování seznamu nastaví pro zásady zálohování</span><span class="sxs-lookup"><span data-stu-id="c0a2f-126">List backup sets for a backup policy</span></span>
<span data-ttu-id="c0a2f-127">Pomocí následujících kroků na seznam všech záloh pro zásady zálohování.</span><span class="sxs-lookup"><span data-stu-id="c0a2f-127">Complete the following steps to list all the backups for a backup policy.</span></span>

#### <a name="to-list-backup-sets"></a><span data-ttu-id="c0a2f-128">Do seznamu zálohovací sklady</span><span class="sxs-lookup"><span data-stu-id="c0a2f-128">To list backup sets</span></span>
1. <span data-ttu-id="c0a2f-129">Přejděte k vaší Správce zařízení StorSimple služby a klikněte na tlačítko **katalog zálohování**.</span><span class="sxs-lookup"><span data-stu-id="c0a2f-129">Go to your StorSimple Device Manager service and click **Backup catalog**.</span></span>

2. <span data-ttu-id="c0a2f-130">Výběr filtru následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="c0a2f-130">Filter the selections as follows:</span></span>
   
   1. <span data-ttu-id="c0a2f-131">Zadejte časový rozsah.</span><span class="sxs-lookup"><span data-stu-id="c0a2f-131">Specify the time range.</span></span>
   2. <span data-ttu-id="c0a2f-132">Vyberte příslušné zařízení.</span><span class="sxs-lookup"><span data-stu-id="c0a2f-132">Select the appropriate device.</span></span>
   3. <span data-ttu-id="c0a2f-133">Filtrovat podle **zálohování zásad** k zobrazení odpovídající zálohy.</span><span class="sxs-lookup"><span data-stu-id="c0a2f-133">Filter by **Backup policy** to view the corresponding the backups.</span></span>
   3. <span data-ttu-id="c0a2f-134">Z rozevíracího seznamu zásady zálohování vyberte **všechny** zobrazíte všechny zálohy na vybraná zařízení.</span><span class="sxs-lookup"><span data-stu-id="c0a2f-134">From the backup policy dropdown list, choose **All** to view all the backups on the selected device.</span></span>
   4. <span data-ttu-id="c0a2f-135">Klikněte na tlačítko **použít** ke spuštění tohoto dotazu.</span><span class="sxs-lookup"><span data-stu-id="c0a2f-135">Click **Apply** to execute this query.</span></span>
      
      <span data-ttu-id="c0a2f-136">Zálohování přidružených k vybrané zásadě zálohování by se zobrazit v seznamu sad záloh.</span><span class="sxs-lookup"><span data-stu-id="c0a2f-136">The backups associated with the selected backup policy should appear in the list of backup sets.</span></span>

      ![Přejděte do katalogu zálohování](./media/storsimple-8000-manage-backup-catalog/bucatalog1.png)

## <a name="select-a-backup-set"></a><span data-ttu-id="c0a2f-138">Vyberte zálohovací sklad</span><span class="sxs-lookup"><span data-stu-id="c0a2f-138">Select a backup set</span></span>
<span data-ttu-id="c0a2f-139">Pomocí následujících kroků vyberte zálohovacího skladu pro svazek nebo zásady zálohování.</span><span class="sxs-lookup"><span data-stu-id="c0a2f-139">Complete the following steps to select a backup set for a volume or backup policy.</span></span>

#### <a name="to-select-a-backup-set"></a><span data-ttu-id="c0a2f-140">Chcete-li vybrat zálohovacího skladu</span><span class="sxs-lookup"><span data-stu-id="c0a2f-140">To select a backup set</span></span>
1. <span data-ttu-id="c0a2f-141">Přejděte k vaší Správce zařízení StorSimple služby a klikněte na tlačítko **katalog zálohování**.</span><span class="sxs-lookup"><span data-stu-id="c0a2f-141">Go to your StorSimple Device Manager service and click **Backup catalog**.</span></span>
2. <span data-ttu-id="c0a2f-142">Výběr filtru následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="c0a2f-142">Filter the selections as follows:</span></span>
   
   1. <span data-ttu-id="c0a2f-143">Zadejte časový rozsah.</span><span class="sxs-lookup"><span data-stu-id="c0a2f-143">Specify the time range.</span></span> 
   2. <span data-ttu-id="c0a2f-144">Vyberte příslušné zařízení.</span><span class="sxs-lookup"><span data-stu-id="c0a2f-144">Select the appropriate device.</span></span> 
   3. <span data-ttu-id="c0a2f-145">Filtrovat podle zásad svazku nebo zálohu pro zálohování, který chcete vybrat.</span><span class="sxs-lookup"><span data-stu-id="c0a2f-145">Filter by volume or backup policy for the backup that you wish to select.</span></span>
   4. <span data-ttu-id="c0a2f-146">Klikněte na tlačítko **použít** ke spuštění tohoto dotazu.</span><span class="sxs-lookup"><span data-stu-id="c0a2f-146">Click **Apply** to execute this query.</span></span>
      
      <span data-ttu-id="c0a2f-147">Zálohování přidružené k vybranému svazku nebo zásady zálohování by se měla objevit v seznamu sad záloh.</span><span class="sxs-lookup"><span data-stu-id="c0a2f-147">The backups associated with the selected volume or backup policy should appear in the list of backup sets.</span></span>

      ![Přejděte do katalogu zálohování](./media/storsimple-8000-manage-backup-catalog/bucatalog1.png)

3. <span data-ttu-id="c0a2f-149">Vyberte a rozbalte zálohovacího skladu.</span><span class="sxs-lookup"><span data-stu-id="c0a2f-149">Select and expand a backup set.</span></span> <span data-ttu-id="c0a2f-150">Nyní můžete vidět zálohovací sklady členěné podle svazků, které obsahuje.</span><span class="sxs-lookup"><span data-stu-id="c0a2f-150">You can now see the backup sets broken down by the volumes that it contains.</span></span> <span data-ttu-id="c0a2f-151">**Obnovení** a **odstranit** možnosti jsou dostupné prostřednictvím kontextovou nabídku (klikněte pravým tlačítkem) zálohovacího skladu.</span><span class="sxs-lookup"><span data-stu-id="c0a2f-151">The **Restore** and **Delete** options are available via the context menu (right-click) for the backup set.</span></span> <span data-ttu-id="c0a2f-152">Můžete provést některé z těchto akcí na zálohovací sklad, který jste vybrali.</span><span class="sxs-lookup"><span data-stu-id="c0a2f-152">You can perform either of these actions on the backup set that you selected.</span></span>

    ![Přejděte do katalogu zálohování](./media/storsimple-8000-manage-backup-catalog/bucatalog2.png)

## <a name="delete-a-backup-set"></a><span data-ttu-id="c0a2f-154">Odstranit zálohovacího skladu</span><span class="sxs-lookup"><span data-stu-id="c0a2f-154">Delete a backup set</span></span>
<span data-ttu-id="c0a2f-155">Odstraňte zálohy, když už chcete zachovat data s ním spojená.</span><span class="sxs-lookup"><span data-stu-id="c0a2f-155">Delete a backup when you no longer wish to retain the data associated with it.</span></span> <span data-ttu-id="c0a2f-156">Proveďte následující kroky a odstraňte zálohovacího skladu.</span><span class="sxs-lookup"><span data-stu-id="c0a2f-156">Perform the following steps to delete a backup set.</span></span>

#### <a name="to-delete-a-backup-set"></a><span data-ttu-id="c0a2f-157">Chcete-li odstranit zálohovacího skladu</span><span class="sxs-lookup"><span data-stu-id="c0a2f-157">To delete a backup set</span></span>
 <span data-ttu-id="c0a2f-158">Přejděte k vaší Správce zařízení StorSimple služby a klikněte na tlačítko **katalog zálohování**.</span><span class="sxs-lookup"><span data-stu-id="c0a2f-158">Go to your StorSimple Device Manager service and click **Backup catalog**.</span></span>
2. <span data-ttu-id="c0a2f-159">Výběr filtru následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="c0a2f-159">Filter the selections as follows:</span></span>
   
   1. <span data-ttu-id="c0a2f-160">Zadejte časový rozsah.</span><span class="sxs-lookup"><span data-stu-id="c0a2f-160">Specify the time range.</span></span> 
   2. <span data-ttu-id="c0a2f-161">Vyberte příslušné zařízení.</span><span class="sxs-lookup"><span data-stu-id="c0a2f-161">Select the appropriate device.</span></span> 
   3. <span data-ttu-id="c0a2f-162">Filtrovat podle zásad svazku nebo zálohu pro zálohování, který chcete vybrat.</span><span class="sxs-lookup"><span data-stu-id="c0a2f-162">Filter by volume or backup policy for the backup that you wish to select.</span></span>
   4. <span data-ttu-id="c0a2f-163">Klikněte na tlačítko **použít** ke spuštění tohoto dotazu.</span><span class="sxs-lookup"><span data-stu-id="c0a2f-163">Click **Apply** to execute this query.</span></span>
      
      <span data-ttu-id="c0a2f-164">Zálohování přidružené k vybranému svazku nebo zásady zálohování by se měla objevit v seznamu sad záloh.</span><span class="sxs-lookup"><span data-stu-id="c0a2f-164">The backups associated with the selected volume or backup policy should appear in the list of backup sets.</span></span>

      ![Přejděte do katalogu zálohování](./media/storsimple-8000-manage-backup-catalog/bucatalog1.png)

3. <span data-ttu-id="c0a2f-166">Vyberte a rozbalte zálohovacího skladu.</span><span class="sxs-lookup"><span data-stu-id="c0a2f-166">Select and expand a backup set.</span></span> <span data-ttu-id="c0a2f-167">Nyní můžete vidět zálohovací sklady členěné podle svazků, které obsahuje.</span><span class="sxs-lookup"><span data-stu-id="c0a2f-167">You can now see the backup sets broken down by the volumes that it contains.</span></span> <span data-ttu-id="c0a2f-168">**Obnovení** a **odstranit** možnosti jsou dostupné prostřednictvím kontextovou nabídku (klikněte pravým tlačítkem) zálohovacího skladu.</span><span class="sxs-lookup"><span data-stu-id="c0a2f-168">The **Restore** and **Delete** options are available via the context menu (right-click) for the backup set.</span></span> <span data-ttu-id="c0a2f-169">Klikněte pravým tlačítkem na vybrané zálohovacího skladu a v místní nabídce vyberte **odstranit**.</span><span class="sxs-lookup"><span data-stu-id="c0a2f-169">Right-click the selected backup set and from the context menu, select **Delete**.</span></span>

    ![Přejděte do katalogu zálohování](./media/storsimple-8000-manage-backup-catalog/bucatalog3.png)

4. <span data-ttu-id="c0a2f-171">Po zobrazení výzvy k potvrzení, zkontrolujte zobrazené informace a klikněte na **odstranit**.</span><span class="sxs-lookup"><span data-stu-id="c0a2f-171">When prompted for confirmation, review the displayed information and click **Delete**.</span></span> <span data-ttu-id="c0a2f-172">Vybranou zálohu se trvale odstraní.</span><span class="sxs-lookup"><span data-stu-id="c0a2f-172">The selected backup is deleted permanently.</span></span>

    ![Přejděte do katalogu zálohování](./media/storsimple-8000-manage-backup-catalog/bucatalog4.png)  

5. <span data-ttu-id="c0a2f-174">Při odstranění je v průběhu a kdy má úspěšně dokončena, budete upozorněni.</span><span class="sxs-lookup"><span data-stu-id="c0a2f-174">You will be notified when the deletion is in progress and when it has successfully finished.</span></span> <span data-ttu-id="c0a2f-175">Po odstranění probíhá, aktualizujte dotaz na této stránce.</span><span class="sxs-lookup"><span data-stu-id="c0a2f-175">After the deletion is done, refresh the query on this page.</span></span> <span data-ttu-id="c0a2f-176">Odstraněné zálohovací sklad se nebude zobrazovat v seznamu sad záloh.</span><span class="sxs-lookup"><span data-stu-id="c0a2f-176">The deleted backup set will no longer appear in the list of backup sets.</span></span>

    ![Přejděte do katalogu zálohování](./media/storsimple-8000-manage-backup-catalog/bucatalog7.png)

## <a name="next-steps"></a><span data-ttu-id="c0a2f-178">Další kroky</span><span class="sxs-lookup"><span data-stu-id="c0a2f-178">Next steps</span></span>
* <span data-ttu-id="c0a2f-179">Zjistěte, jak [zálohování katalog používat k obnovení vašeho zařízení ze zálohovacího skladu](storsimple-8000-restore-from-backup-set-u2.md).</span><span class="sxs-lookup"><span data-stu-id="c0a2f-179">Learn how to [use the backup catalog to restore your device from a backup set](storsimple-8000-restore-from-backup-set-u2.md).</span></span>
* <span data-ttu-id="c0a2f-180">Zjistěte, jak [použít službu StorSimple Manager zařízení ke správě zařízení StorSimple](storsimple-8000-manager-service-administration.md).</span><span class="sxs-lookup"><span data-stu-id="c0a2f-180">Learn how to [use the StorSimple Device Manager service to administer your StorSimple device](storsimple-8000-manager-service-administration.md).</span></span>

