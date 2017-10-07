---
title: "aaaManage katalog záloh StorSimple | Microsoft Docs"
description: "Vysvětluje, jak toouse hello toolist stránka služby zálohování katalogu Správce zařízení StorSimple, vyberte a odstranit zálohovací sklady."
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
ms.openlocfilehash: e464609e74409a06a198790719abd82ed03c03d9
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="use-hello-storsimple-device-manager-service-toomanage-your-backup-catalog"></a><span data-ttu-id="8fcd2-103">Použít toomanage služby StorSimple Manager zařízení hello katalog záloh</span><span class="sxs-lookup"><span data-stu-id="8fcd2-103">Use hello StorSimple Device Manager service toomanage your backup catalog</span></span>
## <a name="overview"></a><span data-ttu-id="8fcd2-104">Přehled</span><span class="sxs-lookup"><span data-stu-id="8fcd2-104">Overview</span></span>
<span data-ttu-id="8fcd2-105">Hello služby StorSimple Manager zařízení **zálohování katalogu** zobrazuje všechny zálohovací sklady hello, které vytvářejí, když jsou provedeny ruční nebo plánované zálohy.</span><span class="sxs-lookup"><span data-stu-id="8fcd2-105">hello StorSimple Device Manager service **Backup Catalog** blade displays all hello backup sets that are created when manual or scheduled backups are taken.</span></span> <span data-ttu-id="8fcd2-106">Můžete použít tuto stránku toolist všechny hello zálohy pro zásady zálohování nebo svazek, vyberte nebo odstranit zálohy, nebo použít zálohování toorestore nebo klonování svazku.</span><span class="sxs-lookup"><span data-stu-id="8fcd2-106">You can use this page toolist all hello backups for a backup policy or a volume, select or delete backups, or use a backup toorestore or clone a volume.</span></span>

<span data-ttu-id="8fcd2-107">Tento kurz popisuje, jak toolist, vyberte a delete zálohovacího skladu.</span><span class="sxs-lookup"><span data-stu-id="8fcd2-107">This tutorial explains how toolist, select, and delete a backup set.</span></span> <span data-ttu-id="8fcd2-108">toolearn jak toorestore zařízení ze zálohy, přejděte příliš[obnovit vaše zařízení ze zálohovacího skladu](storsimple-8000-restore-from-backup-set-u2.md).</span><span class="sxs-lookup"><span data-stu-id="8fcd2-108">toolearn how toorestore your device from backup, go too[Restore your device from a backup set](storsimple-8000-restore-from-backup-set-u2.md).</span></span> <span data-ttu-id="8fcd2-109">jak tooclone svazku, přejděte příliš toolearn[klonovat svazek StorSimple](storsimple-8000-clone-volume-u2.md).</span><span class="sxs-lookup"><span data-stu-id="8fcd2-109">toolearn how tooclone a volume, go too[Clone a StorSimple volume](storsimple-8000-clone-volume-u2.md).</span></span>

![Zálohování katalogu](./media/storsimple-8000-manage-backup-catalog/bucatalog.png) 

<span data-ttu-id="8fcd2-111">Hello **zálohování katalogu** okno obsahuje dotaz toonarrow výběr zálohovacího skladu.</span><span class="sxs-lookup"><span data-stu-id="8fcd2-111">hello **Backup Catalog** blade provides a query toonarrow your backup set selection.</span></span> <span data-ttu-id="8fcd2-112">Můžete filtrovat hello zálohovací sklady, které jsou načteny podle hello následující parametry:</span><span class="sxs-lookup"><span data-stu-id="8fcd2-112">You can filter hello backup sets that are retrieved, based on hello following parameters:</span></span>

* <span data-ttu-id="8fcd2-113">**Zařízení** – hello zařízení, na které hello zálohovací sklad vytvořen.</span><span class="sxs-lookup"><span data-stu-id="8fcd2-113">**Device** – hello device on which hello backup set was created.</span></span>
* <span data-ttu-id="8fcd2-114">**Zásady zálohování nebo svazku** – hello zásady zálohování nebo svazku přidruženém k tohoto zálohovacího skladu.</span><span class="sxs-lookup"><span data-stu-id="8fcd2-114">**Backup policy or Volume** – hello backup policy or volume associated with this backup set.</span></span>
* <span data-ttu-id="8fcd2-115">**Od a do** – hello rozsah data a času v okamžiku vytvoření zálohovacího skladu hello.</span><span class="sxs-lookup"><span data-stu-id="8fcd2-115">**From and To** – hello date and time range when hello backup set was created.</span></span>

<span data-ttu-id="8fcd2-116">Hello filtrované zálohovací sklady jsou pak v tabulce podle hello následující atributy:</span><span class="sxs-lookup"><span data-stu-id="8fcd2-116">hello filtered backup sets are then tabulated based on hello following attributes:</span></span>

* <span data-ttu-id="8fcd2-117">**Název** – hello název zásady zálohování hello nebo svazku přidruženém k hello zálohovacího skladu.</span><span class="sxs-lookup"><span data-stu-id="8fcd2-117">**Name** – hello name of hello backup policy or volume associated with hello backup set.</span></span>
* <span data-ttu-id="8fcd2-118">**Velikost** – hello skutečná velikost hello zálohovacího skladu.</span><span class="sxs-lookup"><span data-stu-id="8fcd2-118">**Size** – hello actual size of hello backup set.</span></span>
* <span data-ttu-id="8fcd2-119">**Vytvořit na** – hello datum a čas, kdy byla vytvořena hello zálohy.</span><span class="sxs-lookup"><span data-stu-id="8fcd2-119">**Created On** – hello date and time when hello backups were created.</span></span> 
* <span data-ttu-id="8fcd2-120">**Typ** – zálohovací sklady může být místní snímky nebo cloudových snímků.</span><span class="sxs-lookup"><span data-stu-id="8fcd2-120">**Type** – Backup sets can be local snapshots or cloud snapshots.</span></span> <span data-ttu-id="8fcd2-121">Místní snímek je zálohování všech dat uložených místně na zařízení hello svazku, zatímco cloudový snímek odkazuje toohello zálohování svazku dat umístěných v cloudu hello.</span><span class="sxs-lookup"><span data-stu-id="8fcd2-121">A local snapshot is a backup of all your volume data stored locally on hello device, whereas a cloud snapshot refers toohello backup of volume data residing in hello cloud.</span></span> <span data-ttu-id="8fcd2-122">Místní snímky poskytují rychlejší přístup, že jsou pro záleží na odolnosti dat zvolena cloudových snímků.</span><span class="sxs-lookup"><span data-stu-id="8fcd2-122">Local snapshots provide faster access, whereas cloud snapshots are chosen for data resiliency.</span></span>
* <span data-ttu-id="8fcd2-123">**Iniciováno** – hello zálohy lze inicializovat automaticky podle plánu nebo ručně uživatelem.</span><span class="sxs-lookup"><span data-stu-id="8fcd2-123">**Initiated By** – hello backups can be initiated automatically by a schedule or manually by a user.</span></span> <span data-ttu-id="8fcd2-124">Můžete použít zálohování tooschedule zásady zálohování.</span><span class="sxs-lookup"><span data-stu-id="8fcd2-124">You can use a backup policy tooschedule backups.</span></span> <span data-ttu-id="8fcd2-125">Alternativně můžete použít hello **provést zálohování** tootake možnost ručního zálohování.</span><span class="sxs-lookup"><span data-stu-id="8fcd2-125">Alternatively, you can use hello **Take backup** option tootake a manual backup.</span></span>

## <a name="list-backup-sets-for-a-backup-policy"></a><span data-ttu-id="8fcd2-126">Zálohování seznamu nastaví pro zásady zálohování</span><span class="sxs-lookup"><span data-stu-id="8fcd2-126">List backup sets for a backup policy</span></span>
<span data-ttu-id="8fcd2-127">Proveďte následující kroky toolist hello všechny hello zálohy pro zásady zálohování.</span><span class="sxs-lookup"><span data-stu-id="8fcd2-127">Complete hello following steps toolist all hello backups for a backup policy.</span></span>

#### <a name="toolist-backup-sets"></a><span data-ttu-id="8fcd2-128">zálohovací sklady toolist</span><span class="sxs-lookup"><span data-stu-id="8fcd2-128">toolist backup sets</span></span>
1. <span data-ttu-id="8fcd2-129">Přejděte služby StorSimple Manager zařízení tooyour a klikněte na tlačítko **katalog zálohování**.</span><span class="sxs-lookup"><span data-stu-id="8fcd2-129">Go tooyour StorSimple Device Manager service and click **Backup catalog**.</span></span>

2. <span data-ttu-id="8fcd2-130">Filtrovat hello výběr následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="8fcd2-130">Filter hello selections as follows:</span></span>
   
   1. <span data-ttu-id="8fcd2-131">Zadejte časové rozmezí hello.</span><span class="sxs-lookup"><span data-stu-id="8fcd2-131">Specify hello time range.</span></span>
   2. <span data-ttu-id="8fcd2-132">Vyberte příslušné zařízení hello.</span><span class="sxs-lookup"><span data-stu-id="8fcd2-132">Select hello appropriate device.</span></span>
   3. <span data-ttu-id="8fcd2-133">Filtrovat podle **zálohování zásad** tooview hello odpovídající hello zálohy.</span><span class="sxs-lookup"><span data-stu-id="8fcd2-133">Filter by **Backup policy** tooview hello corresponding hello backups.</span></span>
   3. <span data-ttu-id="8fcd2-134">Z rozevíracího seznamu hello zásady zálohování vyberte **všechny** tooview všechny hello zálohy na hello vybraná zařízení.</span><span class="sxs-lookup"><span data-stu-id="8fcd2-134">From hello backup policy dropdown list, choose **All** tooview all hello backups on hello selected device.</span></span>
   4. <span data-ttu-id="8fcd2-135">Klikněte na tlačítko **použít** tooexecute tento dotaz.</span><span class="sxs-lookup"><span data-stu-id="8fcd2-135">Click **Apply** tooexecute this query.</span></span>
      
      <span data-ttu-id="8fcd2-136">Hello zálohy přidružené hello vybrané zásady zálohování by se zobrazit v seznamu hello zálohovací sklady.</span><span class="sxs-lookup"><span data-stu-id="8fcd2-136">hello backups associated with hello selected backup policy should appear in hello list of backup sets.</span></span>

      ![Přejděte toobackup katalogu](./media/storsimple-8000-manage-backup-catalog/bucatalog1.png)

## <a name="select-a-backup-set"></a><span data-ttu-id="8fcd2-138">Vyberte zálohovací sklad</span><span class="sxs-lookup"><span data-stu-id="8fcd2-138">Select a backup set</span></span>
<span data-ttu-id="8fcd2-139">Proveďte následující kroky tooselect zálohu nastavení pro zásadu svazek nebo zálohování hello.</span><span class="sxs-lookup"><span data-stu-id="8fcd2-139">Complete hello following steps tooselect a backup set for a volume or backup policy.</span></span>

#### <a name="tooselect-a-backup-set"></a><span data-ttu-id="8fcd2-140">tooselect zálohovacího skladu</span><span class="sxs-lookup"><span data-stu-id="8fcd2-140">tooselect a backup set</span></span>
1. <span data-ttu-id="8fcd2-141">Přejděte služby StorSimple Manager zařízení tooyour a klikněte na tlačítko **katalog zálohování**.</span><span class="sxs-lookup"><span data-stu-id="8fcd2-141">Go tooyour StorSimple Device Manager service and click **Backup catalog**.</span></span>
2. <span data-ttu-id="8fcd2-142">Filtrovat hello výběr následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="8fcd2-142">Filter hello selections as follows:</span></span>
   
   1. <span data-ttu-id="8fcd2-143">Zadejte časové rozmezí hello.</span><span class="sxs-lookup"><span data-stu-id="8fcd2-143">Specify hello time range.</span></span> 
   2. <span data-ttu-id="8fcd2-144">Vyberte příslušné zařízení hello.</span><span class="sxs-lookup"><span data-stu-id="8fcd2-144">Select hello appropriate device.</span></span> 
   3. <span data-ttu-id="8fcd2-145">Filtrovat podle zásad svazku nebo zálohu pro zálohování hello chcete tooselect.</span><span class="sxs-lookup"><span data-stu-id="8fcd2-145">Filter by volume or backup policy for hello backup that you wish tooselect.</span></span>
   4. <span data-ttu-id="8fcd2-146">Klikněte na tlačítko **použít** tooexecute tento dotaz.</span><span class="sxs-lookup"><span data-stu-id="8fcd2-146">Click **Apply** tooexecute this query.</span></span>
      
      <span data-ttu-id="8fcd2-147">Hello zálohování přidružená k zásadě hello vybraný svazek nebo zálohování by se zobrazit v seznamu hello zálohovací sklady.</span><span class="sxs-lookup"><span data-stu-id="8fcd2-147">hello backups associated with hello selected volume or backup policy should appear in hello list of backup sets.</span></span>

      ![Přejděte toobackup katalogu](./media/storsimple-8000-manage-backup-catalog/bucatalog1.png)

3. <span data-ttu-id="8fcd2-149">Vyberte a rozbalte zálohovacího skladu.</span><span class="sxs-lookup"><span data-stu-id="8fcd2-149">Select and expand a backup set.</span></span> <span data-ttu-id="8fcd2-150">Nyní můžete vidět hello zálohovací sklady členěné podle hello svazky, které obsahuje.</span><span class="sxs-lookup"><span data-stu-id="8fcd2-150">You can now see hello backup sets broken down by hello volumes that it contains.</span></span> <span data-ttu-id="8fcd2-151">Hello **obnovení** a **odstranit** možnosti jsou dostupné prostřednictvím hello kontextovou nabídku (klikněte pravým tlačítkem) hello zálohovacího skladu.</span><span class="sxs-lookup"><span data-stu-id="8fcd2-151">hello **Restore** and **Delete** options are available via hello context menu (right-click) for hello backup set.</span></span> <span data-ttu-id="8fcd2-152">Můžete provést některý z těchto akcí na hello zálohovacího skladu, který jste vybrali.</span><span class="sxs-lookup"><span data-stu-id="8fcd2-152">You can perform either of these actions on hello backup set that you selected.</span></span>

    ![Přejděte toobackup katalogu](./media/storsimple-8000-manage-backup-catalog/bucatalog2.png)

## <a name="delete-a-backup-set"></a><span data-ttu-id="8fcd2-154">Odstranit zálohovacího skladu</span><span class="sxs-lookup"><span data-stu-id="8fcd2-154">Delete a backup set</span></span>
<span data-ttu-id="8fcd2-155">Odstraňte zálohy, když už nechcete tooretain hello data s ním spojená.</span><span class="sxs-lookup"><span data-stu-id="8fcd2-155">Delete a backup when you no longer wish tooretain hello data associated with it.</span></span> <span data-ttu-id="8fcd2-156">Proveďte následující kroky toodelete zálohovacího skladu hello.</span><span class="sxs-lookup"><span data-stu-id="8fcd2-156">Perform hello following steps toodelete a backup set.</span></span>

#### <a name="toodelete-a-backup-set"></a><span data-ttu-id="8fcd2-157">toodelete zálohovacího skladu</span><span class="sxs-lookup"><span data-stu-id="8fcd2-157">toodelete a backup set</span></span>
 <span data-ttu-id="8fcd2-158">Přejděte služby StorSimple Manager zařízení tooyour a klikněte na tlačítko **katalog zálohování**.</span><span class="sxs-lookup"><span data-stu-id="8fcd2-158">Go tooyour StorSimple Device Manager service and click **Backup catalog**.</span></span>
2. <span data-ttu-id="8fcd2-159">Filtrovat hello výběr následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="8fcd2-159">Filter hello selections as follows:</span></span>
   
   1. <span data-ttu-id="8fcd2-160">Zadejte časové rozmezí hello.</span><span class="sxs-lookup"><span data-stu-id="8fcd2-160">Specify hello time range.</span></span> 
   2. <span data-ttu-id="8fcd2-161">Vyberte příslušné zařízení hello.</span><span class="sxs-lookup"><span data-stu-id="8fcd2-161">Select hello appropriate device.</span></span> 
   3. <span data-ttu-id="8fcd2-162">Filtrovat podle zásad svazku nebo zálohu pro zálohování hello chcete tooselect.</span><span class="sxs-lookup"><span data-stu-id="8fcd2-162">Filter by volume or backup policy for hello backup that you wish tooselect.</span></span>
   4. <span data-ttu-id="8fcd2-163">Klikněte na tlačítko **použít** tooexecute tento dotaz.</span><span class="sxs-lookup"><span data-stu-id="8fcd2-163">Click **Apply** tooexecute this query.</span></span>
      
      <span data-ttu-id="8fcd2-164">Hello zálohování přidružená k zásadě hello vybraný svazek nebo zálohování by se zobrazit v seznamu hello zálohovací sklady.</span><span class="sxs-lookup"><span data-stu-id="8fcd2-164">hello backups associated with hello selected volume or backup policy should appear in hello list of backup sets.</span></span>

      ![Přejděte toobackup katalogu](./media/storsimple-8000-manage-backup-catalog/bucatalog1.png)

3. <span data-ttu-id="8fcd2-166">Vyberte a rozbalte zálohovacího skladu.</span><span class="sxs-lookup"><span data-stu-id="8fcd2-166">Select and expand a backup set.</span></span> <span data-ttu-id="8fcd2-167">Nyní můžete vidět hello zálohovací sklady členěné podle hello svazky, které obsahuje.</span><span class="sxs-lookup"><span data-stu-id="8fcd2-167">You can now see hello backup sets broken down by hello volumes that it contains.</span></span> <span data-ttu-id="8fcd2-168">Hello **obnovení** a **odstranit** možnosti jsou dostupné prostřednictvím hello kontextovou nabídku (klikněte pravým tlačítkem) hello zálohovacího skladu.</span><span class="sxs-lookup"><span data-stu-id="8fcd2-168">hello **Restore** and **Delete** options are available via hello context menu (right-click) for hello backup set.</span></span> <span data-ttu-id="8fcd2-169">Klikněte pravým tlačítkem na vybrané hello zálohovacího skladu a hello místní nabídce vyberte **odstranit**.</span><span class="sxs-lookup"><span data-stu-id="8fcd2-169">Right-click hello selected backup set and from hello context menu, select **Delete**.</span></span>

    ![Přejděte toobackup katalogu](./media/storsimple-8000-manage-backup-catalog/bucatalog3.png)

4. <span data-ttu-id="8fcd2-171">Po zobrazení výzvy k potvrzení, zkontrolujte hello zobrazí informace a klikněte na tlačítko **odstranit**.</span><span class="sxs-lookup"><span data-stu-id="8fcd2-171">When prompted for confirmation, review hello displayed information and click **Delete**.</span></span> <span data-ttu-id="8fcd2-172">vybranou zálohu Hello je trvale odstraněn.</span><span class="sxs-lookup"><span data-stu-id="8fcd2-172">hello selected backup is deleted permanently.</span></span>

    ![Přejděte toobackup katalogu](./media/storsimple-8000-manage-backup-catalog/bucatalog4.png)  

5. <span data-ttu-id="8fcd2-174">Při odstranění hello právě probíhá a při úspěšně dokončil, budete upozorněni.</span><span class="sxs-lookup"><span data-stu-id="8fcd2-174">You will be notified when hello deletion is in progress and when it has successfully finished.</span></span> <span data-ttu-id="8fcd2-175">Po odstranění hello probíhá, aktualizujte hello dotazů na této stránce.</span><span class="sxs-lookup"><span data-stu-id="8fcd2-175">After hello deletion is done, refresh hello query on this page.</span></span> <span data-ttu-id="8fcd2-176">zálohovací sklad Hello odstranit se nebude zobrazovat v hello seznamu sad záloh.</span><span class="sxs-lookup"><span data-stu-id="8fcd2-176">hello deleted backup set will no longer appear in hello list of backup sets.</span></span>

    ![Přejděte toobackup katalogu](./media/storsimple-8000-manage-backup-catalog/bucatalog7.png)

## <a name="next-steps"></a><span data-ttu-id="8fcd2-178">Další kroky</span><span class="sxs-lookup"><span data-stu-id="8fcd2-178">Next steps</span></span>
* <span data-ttu-id="8fcd2-179">Zjistěte, jak příliš[použití hello zálohování katalogu toorestore zařízení ze zálohovacího skladu](storsimple-8000-restore-from-backup-set-u2.md).</span><span class="sxs-lookup"><span data-stu-id="8fcd2-179">Learn how too[use hello backup catalog toorestore your device from a backup set](storsimple-8000-restore-from-backup-set-u2.md).</span></span>
* <span data-ttu-id="8fcd2-180">Zjistěte, jak příliš[použití hello tooadminister service Manager zařízení StorSimple zařízení StorSimple](storsimple-8000-manager-service-administration.md).</span><span class="sxs-lookup"><span data-stu-id="8fcd2-180">Learn how too[use hello StorSimple Device Manager service tooadminister your StorSimple device](storsimple-8000-manager-service-administration.md).</span></span>

