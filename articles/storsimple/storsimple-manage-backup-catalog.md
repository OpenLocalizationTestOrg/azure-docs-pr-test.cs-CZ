---
title: "aaaManage katalog záloh StorSimple | Microsoft Docs"
description: "Vysvětluje, jak toouse hello StorSimple Manager služby zálohování katalogu stránky toolist, vyberte a odstranit zálohovací sklady pro svazek."
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
ms.openlocfilehash: 14f565c174a10da2c9e2f934a533a5e493f77226
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="use-hello-storsimple-manager-service-toomanage-your-backup-catalog"></a><span data-ttu-id="8bbe7-103">Použít toomanage služby StorSimple Manager hello katalog záloh</span><span class="sxs-lookup"><span data-stu-id="8bbe7-103">Use hello StorSimple Manager service toomanage your backup catalog</span></span>
## <a name="overview"></a><span data-ttu-id="8bbe7-104">Přehled</span><span class="sxs-lookup"><span data-stu-id="8bbe7-104">Overview</span></span>
<span data-ttu-id="8bbe7-105">Hello služby StorSimple Manager **zálohování katalogu** stránka zobrazuje všechny zálohovací sklady hello, které vytvářejí, když jsou provedeny ruční nebo plánované zálohy.</span><span class="sxs-lookup"><span data-stu-id="8bbe7-105">hello StorSimple Manager service **Backup Catalog** page displays all hello backup sets that are created when manual or scheduled backups are taken.</span></span> <span data-ttu-id="8bbe7-106">Můžete použít tuto stránku toolist všechny hello zálohy pro zásady zálohování nebo svazek, vyberte nebo odstranit zálohy, nebo použít zálohování toorestore nebo klonování svazku.</span><span class="sxs-lookup"><span data-stu-id="8bbe7-106">You can use this page toolist all hello backups for a backup policy or a volume, select or delete backups, or use a backup toorestore or clone a volume.</span></span>

<span data-ttu-id="8bbe7-107">Tento kurz popisuje, jak toolist, vyberte a delete zálohovacího skladu.</span><span class="sxs-lookup"><span data-stu-id="8bbe7-107">This tutorial explains how toolist, select, and delete a backup set.</span></span> <span data-ttu-id="8bbe7-108">toolearn jak toorestore zařízení ze zálohy, přejděte příliš[obnovit vaše zařízení ze zálohovacího skladu](storsimple-restore-from-backup-set.md).</span><span class="sxs-lookup"><span data-stu-id="8bbe7-108">toolearn how toorestore your device from backup, go too[Restore your device from a backup set](storsimple-restore-from-backup-set.md).</span></span> <span data-ttu-id="8bbe7-109">jak tooclone svazku, přejděte příliš toolearn[klonovat svazek StorSimple](storsimple-clone-volume.md).</span><span class="sxs-lookup"><span data-stu-id="8bbe7-109">toolearn how tooclone a volume, go too[Clone a StorSimple volume](storsimple-clone-volume.md).</span></span>

![Zálohování katalogu](./media/storsimple-manage-backup-catalog/backupcatalog.png) 

<span data-ttu-id="8bbe7-111">Hello **zálohování katalogu** stránka obsahuje dotaz toonarrow výběr zálohovacího skladu.</span><span class="sxs-lookup"><span data-stu-id="8bbe7-111">hello **Backup Catalog** page provides a query toonarrow your backup set selection.</span></span> <span data-ttu-id="8bbe7-112">Můžete filtrovat hello zálohovací sklady, které jsou načteny podle hello následující parametry:</span><span class="sxs-lookup"><span data-stu-id="8bbe7-112">You can filter hello backup sets that are retrieved, based on hello following parameters:</span></span>

* <span data-ttu-id="8bbe7-113">**Zařízení** – hello zařízení, na které hello zálohovací sklad vytvořen.</span><span class="sxs-lookup"><span data-stu-id="8bbe7-113">**Device** – hello device on which hello backup set was created.</span></span>
* <span data-ttu-id="8bbe7-114">**Zálohování zásady nebo svazek** – hello zásady zálohování nebo svazku přidruženém k tohoto zálohovacího skladu.</span><span class="sxs-lookup"><span data-stu-id="8bbe7-114">**Backup Policy or Volume** – hello backup policy or volume associated with this backup set.</span></span>
* <span data-ttu-id="8bbe7-115">**Od a do** – hello rozsah data a času v okamžiku vytvoření zálohovacího skladu hello.</span><span class="sxs-lookup"><span data-stu-id="8bbe7-115">**From and To** – hello date and time range when hello backup set was created.</span></span>

<span data-ttu-id="8bbe7-116">Hello filtrované zálohovací sklady jsou pak v tabulce podle hello následující atributy:</span><span class="sxs-lookup"><span data-stu-id="8bbe7-116">hello filtered backup sets are then tabulated based on hello following attributes:</span></span>

* <span data-ttu-id="8bbe7-117">**Název** – hello název zásady zálohování hello nebo svazku přidruženém k hello zálohovacího skladu.</span><span class="sxs-lookup"><span data-stu-id="8bbe7-117">**Name** – hello name of hello backup policy or volume associated with hello backup set.</span></span>
* <span data-ttu-id="8bbe7-118">**Velikost** – hello skutečná velikost hello zálohovacího skladu.</span><span class="sxs-lookup"><span data-stu-id="8bbe7-118">**Size** – hello actual size of hello backup set.</span></span>
* <span data-ttu-id="8bbe7-119">**Vytvořit na** – hello datum a čas, kdy byla vytvořena hello zálohy.</span><span class="sxs-lookup"><span data-stu-id="8bbe7-119">**Created On** – hello date and time when hello backups were created.</span></span> 
* <span data-ttu-id="8bbe7-120">**Typ** – zálohovací sklady může být místní snímky nebo cloudových snímků.</span><span class="sxs-lookup"><span data-stu-id="8bbe7-120">**Type** – Backup sets can be local snapshots or cloud snapshots.</span></span> <span data-ttu-id="8bbe7-121">Místní snímek je zálohování všech dat uložených místně na zařízení hello svazku, zatímco cloudový snímek odkazuje toohello zálohování svazku dat umístěných v cloudu hello.</span><span class="sxs-lookup"><span data-stu-id="8bbe7-121">A local snapshot is a backup of all your volume data stored locally on hello device, whereas a cloud snapshot refers toohello backup of volume data residing in hello cloud.</span></span> <span data-ttu-id="8bbe7-122">Místní snímky poskytují rychlejší přístup, že jsou pro záleží na odolnosti dat zvolena cloudových snímků.</span><span class="sxs-lookup"><span data-stu-id="8bbe7-122">Local snapshots provide faster access, whereas cloud snapshots are chosen for data resiliency.</span></span>
* <span data-ttu-id="8bbe7-123">**Iniciováno** – hello zálohy lze inicializovat automaticky podle plánu nebo ručně uživatelem.</span><span class="sxs-lookup"><span data-stu-id="8bbe7-123">**Initiated By** – hello backups can be initiated automatically by a schedule or manually by a user.</span></span> <span data-ttu-id="8bbe7-124">Můžete použít zálohování tooschedule zásady zálohování.</span><span class="sxs-lookup"><span data-stu-id="8bbe7-124">You can use a backup policy tooschedule backups.</span></span> <span data-ttu-id="8bbe7-125">Alternativně můžete použít hello **provést zálohování** tootake možnost ručního zálohování.</span><span class="sxs-lookup"><span data-stu-id="8bbe7-125">Alternatively, you can use hello **Take backup** option tootake a manual backup.</span></span>

## <a name="list-backup-sets-for-a-volume"></a><span data-ttu-id="8bbe7-126">Seznam zálohovací sklady pro svazek clusteru</span><span class="sxs-lookup"><span data-stu-id="8bbe7-126">List backup sets for a volume</span></span>
<span data-ttu-id="8bbe7-127">Proveďte následující kroky toolist hello všechny hello zálohy pro svazek.</span><span class="sxs-lookup"><span data-stu-id="8bbe7-127">Complete hello following steps toolist all hello backups for a volume.</span></span>

#### <a name="toolist-backup-sets"></a><span data-ttu-id="8bbe7-128">zálohovací sklady toolist</span><span class="sxs-lookup"><span data-stu-id="8bbe7-128">toolist backup sets</span></span>
1. <span data-ttu-id="8bbe7-129">Na stránce služby StorSimple Manager hello, klikněte na hello **katalog zálohování** kartě.</span><span class="sxs-lookup"><span data-stu-id="8bbe7-129">On hello StorSimple Manager service page, click hello **Backup catalog** tab.</span></span>
2. <span data-ttu-id="8bbe7-130">Filtrovat hello výběr následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="8bbe7-130">Filter hello selections as follows:</span></span>
   
   1. <span data-ttu-id="8bbe7-131">Vyberte příslušné zařízení hello.</span><span class="sxs-lookup"><span data-stu-id="8bbe7-131">Select hello appropriate device.</span></span>
   2. <span data-ttu-id="8bbe7-132">V rozevíracím seznamu hello vyberte svazek tooview hello odpovídající hello zálohy.</span><span class="sxs-lookup"><span data-stu-id="8bbe7-132">In hello drop-down list, choose a volume tooview hello corresponding hello backups.</span></span>
   3. <span data-ttu-id="8bbe7-133">Zadejte časové rozmezí hello.</span><span class="sxs-lookup"><span data-stu-id="8bbe7-133">Specify hello time range.</span></span>
   4. <span data-ttu-id="8bbe7-134">Klikněte na ikonu zaškrtnutí hello</span><span class="sxs-lookup"><span data-stu-id="8bbe7-134">Click hello check icon</span></span> ![Ikona zaškrtnutí](./media/storsimple-manage-backup-catalog/HCS_CheckIcon.png) <span data-ttu-id="8bbe7-136">tooexecute tento dotaz.</span><span class="sxs-lookup"><span data-stu-id="8bbe7-136">tooexecute this query.</span></span>
      
      <span data-ttu-id="8bbe7-137">zálohování Hello přidružené hello vybraný svazek by se zobrazit v seznamu hello zálohovací sklady.</span><span class="sxs-lookup"><span data-stu-id="8bbe7-137">hello backups associated with hello selected volume should appear in hello list of backup sets.</span></span>

## <a name="select-a-backup-set"></a><span data-ttu-id="8bbe7-138">Vyberte zálohovací sklad</span><span class="sxs-lookup"><span data-stu-id="8bbe7-138">Select a backup set</span></span>
<span data-ttu-id="8bbe7-139">Proveďte následující kroky tooselect zálohu nastavení pro zásadu svazek nebo zálohování hello.</span><span class="sxs-lookup"><span data-stu-id="8bbe7-139">Complete hello following steps tooselect a backup set for a volume or backup policy.</span></span>

#### <a name="tooselect-a-backup-set"></a><span data-ttu-id="8bbe7-140">tooselect zálohovacího skladu</span><span class="sxs-lookup"><span data-stu-id="8bbe7-140">tooselect a backup set</span></span>
1. <span data-ttu-id="8bbe7-141">Na stránce služby StorSimple Manager hello, klikněte na hello **katalog zálohování** kartě.</span><span class="sxs-lookup"><span data-stu-id="8bbe7-141">On hello StorSimple Manager service page, click hello **Backup catalog** tab.</span></span>
2. <span data-ttu-id="8bbe7-142">Filtrovat hello výběr následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="8bbe7-142">Filter hello selections as follows:</span></span>
   
   1. <span data-ttu-id="8bbe7-143">Vyberte příslušné zařízení hello.</span><span class="sxs-lookup"><span data-stu-id="8bbe7-143">Select hello appropriate device.</span></span>
   2. <span data-ttu-id="8bbe7-144">V rozevíracím seznamu hello výběru hello svazek nebo zálohování zásady zálohování hello chcete tooselect.</span><span class="sxs-lookup"><span data-stu-id="8bbe7-144">In hello drop-down list, choose hello volume or backup policy for hello backup that you wish tooselect.</span></span>
   3. <span data-ttu-id="8bbe7-145">Zadejte časové rozmezí hello.</span><span class="sxs-lookup"><span data-stu-id="8bbe7-145">Specify hello time range.</span></span>
   4. <span data-ttu-id="8bbe7-146">Klikněte na ikonu zaškrtnutí hello</span><span class="sxs-lookup"><span data-stu-id="8bbe7-146">Click hello check icon</span></span> ![Ikona zaškrtnutí](./media/storsimple-manage-backup-catalog/HCS_CheckIcon.png) <span data-ttu-id="8bbe7-148">tooexecute tento dotaz.</span><span class="sxs-lookup"><span data-stu-id="8bbe7-148">tooexecute this query.</span></span>
      
      <span data-ttu-id="8bbe7-149">Hello zálohování přidružená k zásadě hello vybraný svazek nebo zálohování by se zobrazit v seznamu hello zálohovací sklady.</span><span class="sxs-lookup"><span data-stu-id="8bbe7-149">hello backups associated with hello selected volume or backup policy should appear in hello list of backup sets.</span></span>
3. <span data-ttu-id="8bbe7-150">Vyberte a rozbalte zálohovacího skladu.</span><span class="sxs-lookup"><span data-stu-id="8bbe7-150">Select and expand a backup set.</span></span> <span data-ttu-id="8bbe7-151">Hello **obnovení** a **odstranit** možnosti se zobrazí v dolní části hello hello stránky.</span><span class="sxs-lookup"><span data-stu-id="8bbe7-151">hello **Restore** and **Delete** options are displayed at hello bottom of hello page.</span></span> <span data-ttu-id="8bbe7-152">Můžete provést některý z těchto akcí na hello zálohovacího skladu, který jste vybrali.</span><span class="sxs-lookup"><span data-stu-id="8bbe7-152">You can perform either of these actions on hello backup set that you selected.</span></span>

## <a name="delete-a-backup-set"></a><span data-ttu-id="8bbe7-153">Odstranit zálohovacího skladu</span><span class="sxs-lookup"><span data-stu-id="8bbe7-153">Delete a backup set</span></span>
<span data-ttu-id="8bbe7-154">Odstraňte zálohy, když už nechcete tooretain hello data s ním spojená.</span><span class="sxs-lookup"><span data-stu-id="8bbe7-154">Delete a backup when you no longer wish tooretain hello data associated with it.</span></span> <span data-ttu-id="8bbe7-155">Proveďte následující kroky toodelete zálohovacího skladu hello.</span><span class="sxs-lookup"><span data-stu-id="8bbe7-155">Perform hello following steps toodelete a backup set.</span></span>

#### <a name="toodelete-a-backup-set"></a><span data-ttu-id="8bbe7-156">toodelete zálohovacího skladu</span><span class="sxs-lookup"><span data-stu-id="8bbe7-156">toodelete a backup set</span></span>
1. <span data-ttu-id="8bbe7-157">Na stránce služby StorSimple Manager hello, klikněte na hello **katalog zálohování karta**.</span><span class="sxs-lookup"><span data-stu-id="8bbe7-157">On hello StorSimple Manager service page, click hello **Backup Catalog tab**.</span></span>
2. <span data-ttu-id="8bbe7-158">Filtrovat hello výběr následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="8bbe7-158">Filter hello selections as follows:</span></span>
   
   1. <span data-ttu-id="8bbe7-159">Vyberte příslušné zařízení hello.</span><span class="sxs-lookup"><span data-stu-id="8bbe7-159">Select hello appropriate device.</span></span>
   2. <span data-ttu-id="8bbe7-160">V rozevíracím seznamu hello výběru hello svazek nebo zálohování zásady zálohování hello chcete tooselect.</span><span class="sxs-lookup"><span data-stu-id="8bbe7-160">In hello drop-down list, choose hello volume or backup policy for hello backup that you wish tooselect.</span></span>
   3. <span data-ttu-id="8bbe7-161">Zadejte časové rozmezí hello.</span><span class="sxs-lookup"><span data-stu-id="8bbe7-161">Specify hello time range.</span></span>
   4. <span data-ttu-id="8bbe7-162">Klikněte na ikonu zaškrtnutí hello</span><span class="sxs-lookup"><span data-stu-id="8bbe7-162">Click hello check icon</span></span> ![Ikona zaškrtnutí](./media/storsimple-manage-backup-catalog/HCS_CheckIcon.png) <span data-ttu-id="8bbe7-164">tooexecute tento dotaz.</span><span class="sxs-lookup"><span data-stu-id="8bbe7-164">tooexecute this query.</span></span>
      
      <span data-ttu-id="8bbe7-165">Hello zálohování přidružená k zásadě hello vybraný svazek nebo zálohování by se zobrazit v seznamu hello zálohovací sklady.</span><span class="sxs-lookup"><span data-stu-id="8bbe7-165">hello backups associated with hello selected volume or backup policy should appear in hello list of backup sets.</span></span>
3. <span data-ttu-id="8bbe7-166">Vyberte a rozbalte zálohovacího skladu.</span><span class="sxs-lookup"><span data-stu-id="8bbe7-166">Select and expand a backup set.</span></span> <span data-ttu-id="8bbe7-167">Hello **obnovení** a **odstranit** možnosti se zobrazí v dolní části hello hello stránky.</span><span class="sxs-lookup"><span data-stu-id="8bbe7-167">hello **Restore** and **Delete** options are displayed at hello bottom of hello page.</span></span> <span data-ttu-id="8bbe7-168">Klikněte na **Odstranit**.</span><span class="sxs-lookup"><span data-stu-id="8bbe7-168">Click **Delete**.</span></span>
4. <span data-ttu-id="8bbe7-169">Při odstranění hello právě probíhá a při úspěšně dokončil, budete upozorněni.</span><span class="sxs-lookup"><span data-stu-id="8bbe7-169">You will be notified when hello deletion is in progress and when it has successfully finished.</span></span> <span data-ttu-id="8bbe7-170">Po odstranění hello probíhá, aktualizujte hello dotazů na této stránce.</span><span class="sxs-lookup"><span data-stu-id="8bbe7-170">After hello deletion is done, refresh hello query on this page.</span></span> <span data-ttu-id="8bbe7-171">zálohovací sklad Hello odstranit se nebude zobrazovat v hello seznamu sad záloh.</span><span class="sxs-lookup"><span data-stu-id="8bbe7-171">hello deleted backup set will no longer appear in hello list of backup sets.</span></span>

## <a name="next-steps"></a><span data-ttu-id="8bbe7-172">Další kroky</span><span class="sxs-lookup"><span data-stu-id="8bbe7-172">Next steps</span></span>
* <span data-ttu-id="8bbe7-173">Zjistěte, jak příliš[použití hello zálohování katalogu toorestore zařízení ze zálohovacího skladu](storsimple-restore-from-backup-set.md).</span><span class="sxs-lookup"><span data-stu-id="8bbe7-173">Learn how too[use hello backup catalog toorestore your device from a backup set](storsimple-restore-from-backup-set.md).</span></span>
* <span data-ttu-id="8bbe7-174">Zjistěte, jak příliš[použití hello tooadminister služby StorSimple Manager zařízení StorSimple](storsimple-manager-service-administration.md).</span><span class="sxs-lookup"><span data-stu-id="8bbe7-174">Learn how too[use hello StorSimple Manager service tooadminister your StorSimple device](storsimple-manager-service-administration.md).</span></span>

