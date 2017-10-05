---
title: "Obnovit svazek StorSimple ze zálohy | Microsoft Docs"
description: "Vysvětluje, jak používat stránky zálohování katalogu služby StorSimple Manager obnovit svazek StorSimple ze zálohovacího skladu."
services: storsimple
documentationcenter: NA
author: alkohli
manager: timlt
editor: 
ms.assetid: 6f289c39-96c7-4d57-b68a-4bc2e99aef9d
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 03/22/2017
ms.author: alkohli
ms.openlocfilehash: 99b76e3bc2939c65654cbf606fda6f8a45e0c44b
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="restore-a-storsimple-volume-from-a-backup-set-update-2"></a><span data-ttu-id="a47a5-103">Obnovit svazek StorSimple ze zálohovacího skladu (Update 2)</span><span class="sxs-lookup"><span data-stu-id="a47a5-103">Restore a StorSimple volume from a backup set (Update 2)</span></span>
[!INCLUDE [storsimple-version-selector-restore-from-backup](../../includes/storsimple-version-selector-restore-from-backup.md)]

## <a name="overview"></a><span data-ttu-id="a47a5-104">Přehled</span><span class="sxs-lookup"><span data-stu-id="a47a5-104">Overview</span></span>
<span data-ttu-id="a47a5-105">**Zálohování katalogu** stránka zobrazuje všechny zálohovací sklady, které vytvářejí, když jsou provedeny ruční nebo automatické zálohy.</span><span class="sxs-lookup"><span data-stu-id="a47a5-105">The **Backup Catalog** page displays all the backup sets that are created when manual or automated backups are taken.</span></span> <span data-ttu-id="a47a5-106">Pomocí této stránky lze zobrazit seznam a spravovat zálohy, obnovení ze zálohovacího skladu nebo klonovat svazku.</span><span class="sxs-lookup"><span data-stu-id="a47a5-106">Use this page to list and manage backups, restore from a backup set, or clone a volume.</span></span>

 ![Zálohování stránky katalogu](./media/storsimple-restore-from-backup-set-u2/restore.png)

<span data-ttu-id="a47a5-108">Tento kurz vysvětluje, jak používat **zálohování katalogu** stránku k obnovení vašeho zařízení ze zálohovacího skladu.</span><span class="sxs-lookup"><span data-stu-id="a47a5-108">This tutorial explains how to use the **Backup Catalog** page to restore your device from a backup set.</span></span>

<span data-ttu-id="a47a5-109">Můžete obnovit svazek z místní nebo cloudové zálohování.</span><span class="sxs-lookup"><span data-stu-id="a47a5-109">You can restore a volume from a local or cloud backup.</span></span> <span data-ttu-id="a47a5-110">V obou případech operace obnovení připojí svazek online okamžitě, když data se stáhne na pozadí.</span><span class="sxs-lookup"><span data-stu-id="a47a5-110">In either case, the restore operation brings the volume online immediately while data is downloaded in the background.</span></span> 

## <a name="before-you-restore"></a><span data-ttu-id="a47a5-111">Před obnovením</span><span class="sxs-lookup"><span data-stu-id="a47a5-111">Before you restore</span></span>
<span data-ttu-id="a47a5-112">Před spuštěním operace obnovení, byste měli vědět o těchto aspektů:</span><span class="sxs-lookup"><span data-stu-id="a47a5-112">Before you initiate a restore operation, you should be aware of the following caveats:</span></span>

* <span data-ttu-id="a47a5-113">**Do offline režimu svazku** – trvat svazek offline na hostiteli a zařízení, před spuštěním operace obnovení.</span><span class="sxs-lookup"><span data-stu-id="a47a5-113">**Take the volume offline** – Take the volume offline on both the host and the device before you initiate the restore operation.</span></span> <span data-ttu-id="a47a5-114">Přestože operace obnovení automaticky připojí svazek online na zařízení, je nutné ručně přenést zařízení online na hostiteli.</span><span class="sxs-lookup"><span data-stu-id="a47a5-114">Although the restore operation automatically brings the volume online on the device, you must manually bring the device online on the host.</span></span> <span data-ttu-id="a47a5-115">Převedete online svazek na hostiteli, pokud je svazek online na zařízení.</span><span class="sxs-lookup"><span data-stu-id="a47a5-115">You can bring the volume online on the host when the volume is online on the device.</span></span> <span data-ttu-id="a47a5-116">(Není nutné čekat, až po dokončení operace obnovení.) Postupy, přejděte na [do offline režimu svazku](storsimple-manage-volumes-u2.md#take-a-volume-offline).</span><span class="sxs-lookup"><span data-stu-id="a47a5-116">(You do not need to wait until the restore operation is finished.) For procedures, go to [Take a volume offline](storsimple-manage-volumes-u2.md#take-a-volume-offline).</span></span>
* <span data-ttu-id="a47a5-117">**Typ svazku po obnovení** – odstraněné svazky jsou obnoveny podle typu ve snímku.</span><span class="sxs-lookup"><span data-stu-id="a47a5-117">**Volume type after restore** – Deleted volumes are restored based on the type in the snapshot.</span></span> <span data-ttu-id="a47a5-118">Svazky, které byly místně vázaný se obnoví jako místně vázaných svazků a svazky, které byly zřízeny vrstvené se obnoví jako vrstvené svazky.</span><span class="sxs-lookup"><span data-stu-id="a47a5-118">Volumes that were locally pinned are restored as locally pinned volumes and volumes that were tiered are restored as tiered volumes.</span></span>
  
    <span data-ttu-id="a47a5-119">Pro existující svazky přepíše aktuální typ použití svazku typ, který je uložený ve snímku.</span><span class="sxs-lookup"><span data-stu-id="a47a5-119">For existing volumes, the current usage type of the volume overrides the type that is stored in the snapshot.</span></span> <span data-ttu-id="a47a5-120">Například pokud obnovujete svazek ze snímku, která se provede, když byla vrstvené typ svazku a typ svazku je nyní místně vázaný (z důvodu operaci převodu), pak svazek je obnoven jako místně vázaný svazek.</span><span class="sxs-lookup"><span data-stu-id="a47a5-120">For example, if you restore a volume from a snapshot that was taken when the volume type was tiered and that volume type is now locally pinned (due to a conversion operation), then the volume is restored as a locally pinned volume.</span></span> <span data-ttu-id="a47a5-121">Podobně pokud existující místně vázaný svazek je rozšířena a následně obnovit ze starší snímek pořízený při menší svazek, svazek obnovené zachová aktuální rozbalenou velikost.</span><span class="sxs-lookup"><span data-stu-id="a47a5-121">Similarly, if an existing locally pinned volume is expanded and subsequently restored from an older snapshot taken when the volume was smaller, the restored volume retains the current expanded size.</span></span>
  
    <span data-ttu-id="a47a5-122">Nelze převést na svazek z vrstvený svazek místně vázaný svazek nebo _naopak_ při obnovení svazku.</span><span class="sxs-lookup"><span data-stu-id="a47a5-122">You cannot convert a volume from a tiered volume to a locally pinned volume or _vice-versa_ while the volume is being restored.</span></span> <span data-ttu-id="a47a5-123">Počkejte na dokončení operace obnovení, a pak můžete převést svazek k jinému typu.</span><span class="sxs-lookup"><span data-stu-id="a47a5-123">Wait until the restore operation is finished, and then you can convert the volume to another type.</span></span> <span data-ttu-id="a47a5-124">Informace o převodu svazku, přejděte na [změnit typ svazku](storsimple-manage-volumes-u2.md#change-the-volume-type).</span><span class="sxs-lookup"><span data-stu-id="a47a5-124">For information about converting a volume, go to [Change the volume type](storsimple-manage-volumes-u2.md#change-the-volume-type).</span></span> 
* <span data-ttu-id="a47a5-125">**Velikost svazku se odrazí v obnovené svazku** – Toto je důležitý faktor, pokud obnovujete místně vázaný svazek, který byl odstraněn, (protože místně vázaných svazků jsou plně zřízený).</span><span class="sxs-lookup"><span data-stu-id="a47a5-125">**The volume size is reflected in the restored volume** – This is an important consideration if you are restoring a locally pinned volume that has been deleted (because locally pinned volumes are fully provisioned).</span></span> <span data-ttu-id="a47a5-126">Ujistěte se, že máte dostatek místa, před dalším pokusem o obnovení místně vázaný svazek, který byl dříve odstraněn.</span><span class="sxs-lookup"><span data-stu-id="a47a5-126">Make sure that you have sufficient space before you attempt to restore a locally pinned volume that was previously deleted.</span></span> 
* <span data-ttu-id="a47a5-127">**Nelze rozbalit svazku, zatímco je obnovena** – počkejte na dokončení operace obnovení před dalším pokusem o rozšířit svazek.</span><span class="sxs-lookup"><span data-stu-id="a47a5-127">**You cannot expand a volume while it is being restored** – Wait until the restore operation is finished before you attempt to expand the volume.</span></span> <span data-ttu-id="a47a5-128">Informace o rozšiřování svazek, přejděte na [upravit svazek](storsimple-manage-volumes-u2.md#modify-a-volume).</span><span class="sxs-lookup"><span data-stu-id="a47a5-128">For information about expanding a volume, go to [Modify a volume](storsimple-manage-volumes-u2.md#modify-a-volume).</span></span>
* <span data-ttu-id="a47a5-129">**Můžete provést zálohu, když obnovujete svazek místní** – postupy naleznete na [použít službu StorSimple Manager ke správě zásady zálohování](storsimple-manage-backup-policies.md).</span><span class="sxs-lookup"><span data-stu-id="a47a5-129">**You can perform a backup while you are restoring a local volume** – For procedures go to [Use the StorSimple Manager service to manage backup policies](storsimple-manage-backup-policies.md).</span></span>
* <span data-ttu-id="a47a5-130">**Můžete zrušit operaci obnovení** – Pokud zrušíte úlohy obnovení, pak svazek je vrácena zpět do stavu, který byl předtím, než jste spustili obnovení.</span><span class="sxs-lookup"><span data-stu-id="a47a5-130">**You can cancel a restore operation** – If you cancel the restore job, then the volume is rolled back to the state that it was in before you initiated the restore.</span></span> <span data-ttu-id="a47a5-131">Postupy, přejděte na [zrušení úlohy](storsimple-manage-jobs-u2.md#cancel-a-job).</span><span class="sxs-lookup"><span data-stu-id="a47a5-131">For procedures, go to [Cancel a job](storsimple-manage-jobs-u2.md#cancel-a-job).</span></span>

## <a name="how-does-restore-work"></a><span data-ttu-id="a47a5-132">Jak obnovit práci</span><span class="sxs-lookup"><span data-stu-id="a47a5-132">How does restore work</span></span>
<span data-ttu-id="a47a5-133">Pro zařízení se systémem Update 4 nebo novější se implementuje na základě heatmap obnovení.</span><span class="sxs-lookup"><span data-stu-id="a47a5-133">For devices running Update 4 or later, a heatmap-based restore is implemented.</span></span> <span data-ttu-id="a47a5-134">Jako hostitele požadavky na přístup k datům přístup zařízení, tyto požadavky jsou sledovány a k vytvoření heatmap.</span><span class="sxs-lookup"><span data-stu-id="a47a5-134">As the host requests to access data reach the device, these requests are tracked and a heatmap is created.</span></span> <span data-ttu-id="a47a5-135">Rychlost vysokou požadavků výsledkem bloky dat s vyšší heat, zatímco překládá nižší rychlost požadavků na bloky dat s nižší heat.</span><span class="sxs-lookup"><span data-stu-id="a47a5-135">High request rate results in data chunks with higher heat whereas lower request rate translates to chunks with lower heat.</span></span> <span data-ttu-id="a47a5-136">Je nutné získat přístup dat alespoň dvakrát na označit jako _aktivní_.</span><span class="sxs-lookup"><span data-stu-id="a47a5-136">You must access the data atleast twice to be marked as _hot_.</span></span> <span data-ttu-id="a47a5-137">Soubor, který se mění je také označena jako _aktivní_.</span><span class="sxs-lookup"><span data-stu-id="a47a5-137">A file that is modified is also marked as _hot_.</span></span> <span data-ttu-id="a47a5-138">Jakmile zahájíte obnovení, nastane proaktivní dosazováním dat podle heatmap.</span><span class="sxs-lookup"><span data-stu-id="a47a5-138">Once you initiate the restore, then proactive hydration of data occurs based on the heatmap.</span></span> <span data-ttu-id="a47a5-139">Verze starší než aktualizace 4, data byla stažena během obnovení založená na přístupu jen.</span><span class="sxs-lookup"><span data-stu-id="a47a5-139">For versions earlier than Update 4, the data was downloaded during restore based on access only.</span></span> 

<span data-ttu-id="a47a5-140">Na základě Heatmap sledování je povoleno pouze pro vrstvené svazky a místně vázaný svazky nejsou podporované.</span><span class="sxs-lookup"><span data-stu-id="a47a5-140">Heatmap-based tracking is enabled only for tiered volumes and locally pinned volumes are not supported.</span></span> <span data-ttu-id="a47a5-141">Obnovení na základě Heatmap není podporováno také při klonování svazku na jiné zařízení.</span><span class="sxs-lookup"><span data-stu-id="a47a5-141">Heatmap-based restore is also not supported when cloning a volume to another device.</span></span> <span data-ttu-id="a47a5-142">Pokud dojde obnovení na místě a místní snímek pro svazek, který se má obnovit v zařízení existuje, pak jsme není rehydrataci při spotřebě (protože data je již k dispozici místně).</span><span class="sxs-lookup"><span data-stu-id="a47a5-142">If there is an in-place restore and a local snapshot for the volume to be restored exists on the device, then we do not rehydrate (as data is already available locally).</span></span> <span data-ttu-id="a47a5-143">Ve výchozím nastavení Pokud obnovujete, rehydrataci úlohy se spouští které proaktivně rehydrataci při spotřebě dat podle heatmap.</span><span class="sxs-lookup"><span data-stu-id="a47a5-143">By default, when you restore, the rehydration jobs are initiated that proactively rehydrate data based on the heatmap.</span></span> <span data-ttu-id="a47a5-144">V aktualizaci 4 lze použít rutiny prostředí Windows PowerShell pro dotazování spuštěné úlohy rehydrataci, zrušení úlohy rehydrataci a získat stav úlohy rehydrataci.</span><span class="sxs-lookup"><span data-stu-id="a47a5-144">In Update 4, Windows PowerShell cmdlets can be used to query running rehydration jobs, cancel a rehydration job, and get the status of the rehydration job.</span></span>

* <span data-ttu-id="a47a5-145">`Get-HcsRehydrationJob`– Tato rutina načte stav úlohy rehydrataci.</span><span class="sxs-lookup"><span data-stu-id="a47a5-145">`Get-HcsRehydrationJob` - This cmdlet gets the status of the rehydration job.</span></span> <span data-ttu-id="a47a5-146">Aktivuje úlohu jeden rehydrataci se pro jeden svazek.</span><span class="sxs-lookup"><span data-stu-id="a47a5-146">A single rehydration job is triggered for one volume.</span></span>
* <span data-ttu-id="a47a5-147">`Set-HcsRehydrationJob`– Tato rutina umožňuje pozastavit, zastavit, obnovte úlohu rehydrataci, když probíhá rehydrataci.</span><span class="sxs-lookup"><span data-stu-id="a47a5-147">`Set-HcsRehydrationJob` - This cmdlet allows you to pause, stop, resume the rehydration job, when the rehydration is in progress.</span></span> 

<span data-ttu-id="a47a5-148">Další informace o rutinách rehydrataci, přejděte na [odkazu na rutiny Windows Powershellu pro StorSimple](https://technet.microsoft.com/library/dn688168.aspx).</span><span class="sxs-lookup"><span data-stu-id="a47a5-148">For more information on rehydration cmdlets, go to [Windows PowerShell cmdlet reference for StorSimple](https://technet.microsoft.com/library/dn688168.aspx).</span></span>

<span data-ttu-id="a47a5-149">S automatickou rehdyration obvykle vyšší výkon přechodný pro čtení se očekává.</span><span class="sxs-lookup"><span data-stu-id="a47a5-149">With automatic rehdyration, typically higher transient read performance is expected.</span></span> <span data-ttu-id="a47a5-150">Skutečné magniutde vylepšení závisí na různých faktorech, jako je například vzor přístupu, mísení dat a typu dat.</span><span class="sxs-lookup"><span data-stu-id="a47a5-150">The actual magniutde of improvements depends on various factors such as access pattern, data churn, and data type.</span></span> <span data-ttu-id="a47a5-151">Pokud chcete zrušit úlohu rehydrataci, můžete použít rutinu prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="a47a5-151">To cancel a rehydration job, you can use the PowerShell cmdlet.</span></span> <span data-ttu-id="a47a5-152">Pokud chcete trvale zakázat rehydrataci úloh pro všechny budoucí obnovení, obraťte se na Microsoft Support.</span><span class="sxs-lookup"><span data-stu-id="a47a5-152">If you wish to permanently disable rehydration jobs for all the future restores, contact Microsoft Support.</span></span>

## <a name="how-to-use-the-backup-catalog"></a><span data-ttu-id="a47a5-153">Použití zálohování katalogu</span><span class="sxs-lookup"><span data-stu-id="a47a5-153">How to use the backup catalog</span></span>
<span data-ttu-id="a47a5-154">**Zálohování katalogu** stránka obsahuje dotaz, který umožňuje zúžit zálohování nastavte výběr.</span><span class="sxs-lookup"><span data-stu-id="a47a5-154">The **Backup Catalog** page provides a query that helps you to narrow your backup set selection.</span></span> <span data-ttu-id="a47a5-155">Můžete filtrovat zálohovací sklady, které jsou načteny na základě následujících parametrů:</span><span class="sxs-lookup"><span data-stu-id="a47a5-155">You can filter the backup sets that are retrieved based on the following parameters:</span></span>

* <span data-ttu-id="a47a5-156">**Zařízení** – zařízení, v němž byla vytvořena zálohovacího skladu.</span><span class="sxs-lookup"><span data-stu-id="a47a5-156">**Device** – The device on which the backup set was created.</span></span>
* <span data-ttu-id="a47a5-157">**Zásady zálohování** nebo **svazku** – zásady zálohování nebo svazku přidruženém k tohoto zálohovacího skladu.</span><span class="sxs-lookup"><span data-stu-id="a47a5-157">**Backup policy** or **volume** – The backup policy or volume associated with this backup set.</span></span>
* <span data-ttu-id="a47a5-158">**Z** a **k** – rozsah data a času v okamžiku vytvoření zálohovacího skladu.</span><span class="sxs-lookup"><span data-stu-id="a47a5-158">**From** and **To** – The date and time range when the backup set was created.</span></span>

<span data-ttu-id="a47a5-159">Filtrované zálohovací sklady jsou pak poskytovalo na základě následujících atributů:</span><span class="sxs-lookup"><span data-stu-id="a47a5-159">The filtered backup sets are then tabulated based on the following attributes:</span></span>

* <span data-ttu-id="a47a5-160">**Název** – název zásady zálohování nebo svazku přidruženém k zálohovacího skladu.</span><span class="sxs-lookup"><span data-stu-id="a47a5-160">**Name** – The name of the backup policy or volume associated with the backup set.</span></span>
* <span data-ttu-id="a47a5-161">**Velikost** – skutečná velikost zálohovacího skladu.</span><span class="sxs-lookup"><span data-stu-id="a47a5-161">**Size** – The actual size of the backup set.</span></span>
* <span data-ttu-id="a47a5-162">**Vytvořit v** – datum a čas, kdy byly vytvořeny zálohy.</span><span class="sxs-lookup"><span data-stu-id="a47a5-162">**Created on** – The date and time when the backups were created.</span></span> 
* <span data-ttu-id="a47a5-163">**Typ** – zálohovací sklady může být místní snímky nebo cloudových snímků.</span><span class="sxs-lookup"><span data-stu-id="a47a5-163">**Type** – Backup sets can be local snapshots or cloud snapshots.</span></span> <span data-ttu-id="a47a5-164">Místní snímek je zálohování všech dat svazku uložených místně na zařízení.</span><span class="sxs-lookup"><span data-stu-id="a47a5-164">A local snapshot is a backup of all your volume data stored locally on the device.</span></span> <span data-ttu-id="a47a5-165">Cloudový snímek odkazuje na zálohování svazku dat umístěných v cloudu.</span><span class="sxs-lookup"><span data-stu-id="a47a5-165">A cloud snapshot refers to the backup of volume data residing in the cloud.</span></span> <span data-ttu-id="a47a5-166">Místní snímky poskytují rychlejší přístup, že jsou pro záleží na odolnosti dat zvolena cloudových snímků.</span><span class="sxs-lookup"><span data-stu-id="a47a5-166">Local snapshots provide faster access, whereas cloud snapshots are chosen for data resiliency.</span></span>
* <span data-ttu-id="a47a5-167">**Zahájit** – zálohy lze inicializovat automaticky podle plánu nebo ručně vy.</span><span class="sxs-lookup"><span data-stu-id="a47a5-167">**Initiated by** – The backups can be initiated automatically according to a schedule or manually by you.</span></span> <span data-ttu-id="a47a5-168">(Můžete použít zásady zálohování naplánovat zálohování.</span><span class="sxs-lookup"><span data-stu-id="a47a5-168">(You can use a backup policy to schedule backups.</span></span> <span data-ttu-id="a47a5-169">Alternativně můžete použít **provést zálohování** možnost provést zálohu interaktivní.)</span><span class="sxs-lookup"><span data-stu-id="a47a5-169">Alternatively, you can use the **Take backup** option to take an interactive backup.)</span></span>

## <a name="how-to-restore-your-storsimple-volume-from-a-backup"></a><span data-ttu-id="a47a5-170">Jak obnovit svazek StorSimple ze zálohy</span><span class="sxs-lookup"><span data-stu-id="a47a5-170">How to restore your StorSimple volume from a backup</span></span>
<span data-ttu-id="a47a5-171">Můžete použít **zálohování katalogu** stránku obnovit svazek StorSimple ze konkrétní zálohy.</span><span class="sxs-lookup"><span data-stu-id="a47a5-171">You can use the **Backup Catalog** page to restore your StorSimple volume from a specific backup.</span></span> <span data-ttu-id="a47a5-172">Mějte na paměti, ale, že obnovení svazku vrátí svazku stavu, ve kterém byl v době vytvoření zálohy.</span><span class="sxs-lookup"><span data-stu-id="a47a5-172">Keep in mind, however, that restoring a volume reverts the volume to the state it was in when the backup was taken.</span></span> <span data-ttu-id="a47a5-173">Žádná data, která byla přidána po ztrátě operace zálohování.</span><span class="sxs-lookup"><span data-stu-id="a47a5-173">Any data that was added after the backup operation is lost.</span></span>

> [!WARNING]
> <span data-ttu-id="a47a5-174">Obnovení ze zálohy nahradí existující svazky ze zálohy.</span><span class="sxs-lookup"><span data-stu-id="a47a5-174">Restoring from a backup replaces the existing volumes from the backup.</span></span> <span data-ttu-id="a47a5-175">To může způsobit ztrátu všechna data, která byla zapsána po vytvoření zálohy.</span><span class="sxs-lookup"><span data-stu-id="a47a5-175">This may cause the loss of any data that was written after the backup was taken.</span></span>
> 
> 

### <a name="to-restore-your-volume"></a><span data-ttu-id="a47a5-176">K obnovení svazku</span><span class="sxs-lookup"><span data-stu-id="a47a5-176">To restore your volume</span></span>
1. <span data-ttu-id="a47a5-177">Na stránce služby StorSimple Manager, klepněte **katalog zálohování** kartě.</span><span class="sxs-lookup"><span data-stu-id="a47a5-177">On the StorSimple Manager service page, click the **Backup catalog** tab.</span></span>
   
    ![Zálohování katalogu](./media/storsimple-restore-from-backup-set-u2/restore.png)
2. <span data-ttu-id="a47a5-179">Vyberte zálohovací sklad následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="a47a5-179">Select a backup set as follows:</span></span>
   
   1. <span data-ttu-id="a47a5-180">Vyberte příslušné zařízení.</span><span class="sxs-lookup"><span data-stu-id="a47a5-180">Select the appropriate device.</span></span>
   2. <span data-ttu-id="a47a5-181">V rozevíracím seznamu vyberte svazek nebo zálohování zásady pro zálohu, kterou chcete vybrat.</span><span class="sxs-lookup"><span data-stu-id="a47a5-181">In the drop-down list, choose the volume or backup policy for the backup that you wish to select.</span></span>
   3. <span data-ttu-id="a47a5-182">Zadejte časový rozsah.</span><span class="sxs-lookup"><span data-stu-id="a47a5-182">Specify the time range.</span></span>
   4. <span data-ttu-id="a47a5-183">Klikněte na ikonu zaškrtnutí</span><span class="sxs-lookup"><span data-stu-id="a47a5-183">Click the check icon</span></span> ![ikona zaškrtnutí](./media/storsimple-restore-from-backup-set-u2/HCS_CheckIcon.png) <span data-ttu-id="a47a5-185">k provedení tohoto dotazu.</span><span class="sxs-lookup"><span data-stu-id="a47a5-185">to execute this query.</span></span>
      
      <span data-ttu-id="a47a5-186">Zálohování přidružené k vybranému svazku nebo zásady zálohování by se měla objevit v seznamu sad záloh.</span><span class="sxs-lookup"><span data-stu-id="a47a5-186">The backups associated with the selected volume or backup policy should appear in the list of backup sets.</span></span>
3. <span data-ttu-id="a47a5-187">Rozbalte zálohovací sklad zobrazíte přidružené svazky.</span><span class="sxs-lookup"><span data-stu-id="a47a5-187">Expand the backup set to view the associated volumes.</span></span> <span data-ttu-id="a47a5-188">Tyto svazky musí být převedeno do režimu offline v hostiteli a zařízení, než bude možné obnovit.</span><span class="sxs-lookup"><span data-stu-id="a47a5-188">These volumes must be taken offline on the host and device before you can restore them.</span></span> <span data-ttu-id="a47a5-189">Přístup ke svazkům na **kontejnery svazků** stránky a pak postupujte podle kroků v [do offline režimu svazek](storsimple-manage-volumes-u2.md#take-a-volume-offline) je uvedení do režimu offline.</span><span class="sxs-lookup"><span data-stu-id="a47a5-189">Access the volumes on the **Volume Containers** page, and then follow the steps in [Take a volume offline](storsimple-manage-volumes-u2.md#take-a-volume-offline) to take them offline.</span></span>
   
   > [!IMPORTANT]
   > <span data-ttu-id="a47a5-190">Ujistěte se, že jste provedli svazky do offline režimu na hostiteli nejprve, před provedením svazky do režimu offline v zařízení.</span><span class="sxs-lookup"><span data-stu-id="a47a5-190">Make sure that you have taken the volumes offline on the host first, before you take the volumes offline on the device.</span></span> <span data-ttu-id="a47a5-191">Pokud neprovedete offline svazky na hostiteli, může potenciálně vést k poškození dat.</span><span class="sxs-lookup"><span data-stu-id="a47a5-191">If you do not take the volumes offline on the host, it could potentially lead to data corruption.</span></span>
   > 
   > 
4. <span data-ttu-id="a47a5-192">Přejděte zpět **zálohování katalogu** a vyberte zálohovacího skladu.</span><span class="sxs-lookup"><span data-stu-id="a47a5-192">Navigate back to the **Backup Catalog** tab and select a backup set.</span></span>
5. <span data-ttu-id="a47a5-193">Klikněte na tlačítko **obnovení** v dolní části stránky.</span><span class="sxs-lookup"><span data-stu-id="a47a5-193">Click **Restore** at the bottom of the page.</span></span>
6. <span data-ttu-id="a47a5-194">Zobrazí se výzva k potvrzení.</span><span class="sxs-lookup"><span data-stu-id="a47a5-194">You are prompted for confirmation.</span></span> <span data-ttu-id="a47a5-195">Zkontrolovat informace o obnovení a pak zaškrtněte políčko potvrzení.</span><span class="sxs-lookup"><span data-stu-id="a47a5-195">Review the restore information, and then select the confirmation check box.</span></span>
   
    ![Stránka potvrzení](./media/storsimple-restore-from-backup-set-u2/ConfirmRestore.png)
7. <span data-ttu-id="a47a5-197">Klikněte na ikonu zaškrtnutí ![ikona zaškrtnutí](./media/storsimple-restore-from-backup-set-u2/HCS_CheckIcon.png).</span><span class="sxs-lookup"><span data-stu-id="a47a5-197">Click the check icon ![check icon](./media/storsimple-restore-from-backup-set-u2/HCS_CheckIcon.png).</span></span> <span data-ttu-id="a47a5-198">Spustí úlohu obnovení.</span><span class="sxs-lookup"><span data-stu-id="a47a5-198">A restore job begins.</span></span> <span data-ttu-id="a47a5-199">Můžete zobrazit úlohy přístup **úlohy** stránky.</span><span class="sxs-lookup"><span data-stu-id="a47a5-199">You can view the job by accessing the **Jobs** page.</span></span> 
8. <span data-ttu-id="a47a5-200">Po dokončení obnovení můžete ověřit, že obsah svazků jsou nahrazovány svazků ze zálohy.</span><span class="sxs-lookup"><span data-stu-id="a47a5-200">After the restore is complete, you can verify that the contents of your volumes are replaced by volumes from the backup.</span></span>

<span data-ttu-id="a47a5-201">![Dostupné video](./media/storsimple-restore-from-backup-set-u2/Video_icon.png) **Dostupné video**</span><span class="sxs-lookup"><span data-stu-id="a47a5-201">![Video available](./media/storsimple-restore-from-backup-set-u2/Video_icon.png) **Video available**</span></span>

<span data-ttu-id="a47a5-202">Pokud chcete přehrát video, které ukazuje, jak můžete použít klonu a funkce v zařízení StorSimple obnovit odstraněné soubory obnovit, klikněte na tlačítko [zde](https://azure.microsoft.com/documentation/videos/storsimple-recover-deleted-files-with-storsimple/).</span><span class="sxs-lookup"><span data-stu-id="a47a5-202">To watch a video that demonstrates how you can use the clone and restore features in StorSimple to recover deleted files, click [here](https://azure.microsoft.com/documentation/videos/storsimple-recover-deleted-files-with-storsimple/).</span></span>

## <a name="if-the-restore-fails"></a><span data-ttu-id="a47a5-203">Pokud se nezdaří obnovení</span><span class="sxs-lookup"><span data-stu-id="a47a5-203">If the restore fails</span></span>
<span data-ttu-id="a47a5-204">Obdržíte výstrahu, pokud z nějakého důvodu selže operace obnovení.</span><span class="sxs-lookup"><span data-stu-id="a47a5-204">You receive an alert if the restore operation fails for any reason.</span></span> <span data-ttu-id="a47a5-205">Pokud k tomu dojde, aktualizujte seznamu zálohování a ověřte, zda zálohování je stále platný.</span><span class="sxs-lookup"><span data-stu-id="a47a5-205">If this occurs, refresh the backup list to verify that the backup is still valid.</span></span> <span data-ttu-id="a47a5-206">Pokud záloha není platná a zda obnovujete z cloudu, pak problémy s připojením k může být příčinou problému.</span><span class="sxs-lookup"><span data-stu-id="a47a5-206">If the backup is valid and you are restoring from the cloud, then connectivity issues might be causing the problem.</span></span> 

<span data-ttu-id="a47a5-207">Chcete-li dokončit operaci obnovení, provést offline svazek na hostiteli a opakujte operaci obnovení.</span><span class="sxs-lookup"><span data-stu-id="a47a5-207">To complete the restore operation, take the volume offline on the host and retry the restore operation.</span></span> <span data-ttu-id="a47a5-208">Veškeré úpravy data na svazku, které byly provedeny během procesu obnovení budou ztraceny.</span><span class="sxs-lookup"><span data-stu-id="a47a5-208">Any modifications to the volume data that were performed during the restore process are lost.</span></span>

## <a name="next-steps"></a><span data-ttu-id="a47a5-209">Další kroky</span><span class="sxs-lookup"><span data-stu-id="a47a5-209">Next steps</span></span>
* <span data-ttu-id="a47a5-210">Zjistěte, jak [svazky spravovat zařízení StorSimple](storsimple-manage-volumes-u2.md).</span><span class="sxs-lookup"><span data-stu-id="a47a5-210">Learn how to [Manage StorSimple volumes](storsimple-manage-volumes-u2.md).</span></span>
* <span data-ttu-id="a47a5-211">Zjistěte, jak [použít službu StorSimple Manager ke správě zařízení StorSimple](storsimple-manager-service-administration.md).</span><span class="sxs-lookup"><span data-stu-id="a47a5-211">Learn how to [use the StorSimple Manager service to administer your StorSimple device](storsimple-manager-service-administration.md).</span></span>
