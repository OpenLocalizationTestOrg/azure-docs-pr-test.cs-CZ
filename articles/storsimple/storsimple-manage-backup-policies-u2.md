---
title: "Spravovat zásady zálohování pro vaše zařízení StorSimple | Microsoft Docs"
description: "Vysvětluje, jak je možné použít službu StorSimple Manager k vytváření a správě ručního zálohování, plánů zálohování a uchovávání záloh."
services: storsimple
documentationcenter: NA
author: SharS
manager: carmonm
editor: 
ms.assetid: 4a2db707-bbfc-425c-bfeb-bc5c97781873
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 05/10/2016
ms.author: v-sharos
ms.openlocfilehash: 5448247428ab96887470c6b53f7a9b3dcd9238f0
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="use-the-storsimple-manager-service-to-manage-backup-policies-update-2"></a><span data-ttu-id="e35de-103">Použít službu StorSimple Manager ke správě zásady zálohování (Update 2)</span><span class="sxs-lookup"><span data-stu-id="e35de-103">Use the StorSimple Manager service to manage backup policies (Update 2)</span></span>
[!INCLUDE [storsimple-version-selector-manage-backup-policies](../../includes/storsimple-version-selector-manage-backup-policies.md)]

## <a name="overview"></a><span data-ttu-id="e35de-104">Přehled</span><span class="sxs-lookup"><span data-stu-id="e35de-104">Overview</span></span>
<span data-ttu-id="e35de-105">Tento kurz vysvětluje, jak používat službu StorSimple Manager **zásady zálohování** stránky k řízení procesů zálohování a uchovávání záloh pro formátujte svazky zařízení StorSimple.</span><span class="sxs-lookup"><span data-stu-id="e35de-105">This tutorial explains how to use the StorSimple Manager service **Backup Policies** page to control backup processes and backup retention for your StorSimple volumes.</span></span> <span data-ttu-id="e35de-106">Také popisuje, jak provést ruční zálohy.</span><span class="sxs-lookup"><span data-stu-id="e35de-106">It also describes how to complete a manual backup.</span></span>

<span data-ttu-id="e35de-107">Při zálohování svazku, můžete vytvořit místní snímek nebo cloudový snímek.</span><span class="sxs-lookup"><span data-stu-id="e35de-107">When you back up a volume, you can choose to create a local snapshot or a cloud snapshot.</span></span> <span data-ttu-id="e35de-108">Pokud zálohujete místně vázaný svazek, doporučujeme vám, že zadáváte cloudový snímek.</span><span class="sxs-lookup"><span data-stu-id="e35de-108">If you are backing up a locally pinned volume, we recommend that you specify a cloud snapshot.</span></span> <span data-ttu-id="e35de-109">Pořízení velký počet místní snímky místně vázaný svazek kombinaci s datovou sadou, která obsahuje mnoho změn způsobí v situaci, ve kterém můžete rychle spustit nedostatek místa na místní.</span><span class="sxs-lookup"><span data-stu-id="e35de-109">Taking a large number of local snapshots of a locally pinned volume coupled with a data set that has a lot of churn will result in a situation in which you could rapidly run out of local space.</span></span> <span data-ttu-id="e35de-110">Pokud se rozhodnete převést místní snímky, doporučujeme provést méně snímky denní zálohování nejnovější stav, je zachování za den a pak odstraňte je.</span><span class="sxs-lookup"><span data-stu-id="e35de-110">If you choose to take local snapshots, we recommend that you take fewer daily snapshots to back up the most recent state, retain them for a day, and then delete them.</span></span>

<span data-ttu-id="e35de-111">Pokud pořídíte snímek cloudu místně vázaný svazek, zkopírovat pouze změněné data do cloudu, kde je s odstraněním duplicitních dat a komprimované.</span><span class="sxs-lookup"><span data-stu-id="e35de-111">When you take a cloud snapshot of a locally pinned volume, you copy only the changed data to the cloud, where it is deduplicated and compressed.</span></span> 

## <a name="the-backup-policies-page"></a><span data-ttu-id="e35de-112">Stránka zásady zálohování</span><span class="sxs-lookup"><span data-stu-id="e35de-112">The Backup Policies page</span></span>
<span data-ttu-id="e35de-113">**Zásady zálohování** stránka umožňuje spravovat zásady zálohování a plánování místních a cloudových snímků.</span><span class="sxs-lookup"><span data-stu-id="e35de-113">The **Backup Policies** page allows you to manage backup policies and schedule local and cloud snapshots.</span></span> <span data-ttu-id="e35de-114">(Zásady zálohování se používají ke konfiguraci plánů zálohování a uchovávání záloh pro kolekci svazků). Zásady zálohování umožňují pořízení snímku více svazků najednou.</span><span class="sxs-lookup"><span data-stu-id="e35de-114">(Backup policies are used to configure backup schedules and backup retention for a collection of volumes.) Backup policies enable you to take a snapshot of multiple volumes simultaneously.</span></span> <span data-ttu-id="e35de-115">To znamená, že zálohy vytvořené zásady zálohování bude kopie konzistentní při selhání.</span><span class="sxs-lookup"><span data-stu-id="e35de-115">This means that the backups created by a backup policy will be crash-consistent copies.</span></span> <span data-ttu-id="e35de-116">**Zásady zálohování** stránce zobrazí zásady zálohování, jejich typy, přidružené svazky, počet záloh uchována a možnost povolit tyto zásady.</span><span class="sxs-lookup"><span data-stu-id="e35de-116">The **Backup Policies** page lists the backup policies, their types, the associated volumes, the number of backups retained, and the option to enable these policies.</span></span>

<span data-ttu-id="e35de-117">**Zásady zálohování** stránce můžete také filtrovat existující zásady zálohování jednu nebo více z následujících polí:</span><span class="sxs-lookup"><span data-stu-id="e35de-117">The **Backup Policies** page also allows you to filter the existing backup policies by one or more of the following fields:</span></span>

* <span data-ttu-id="e35de-118">**Název zásady** – název přidružených k zásadě.</span><span class="sxs-lookup"><span data-stu-id="e35de-118">**Policy name** – The name associated with the policy.</span></span> <span data-ttu-id="e35de-119">Různé typy zásady patří:</span><span class="sxs-lookup"><span data-stu-id="e35de-119">The different types of policies include:</span></span>
  
  * <span data-ttu-id="e35de-120">Naplánované zásady, které jsou explicitně vytvořený uživatelem.</span><span class="sxs-lookup"><span data-stu-id="e35de-120">Scheduled policies, which are explicitly created by the user.</span></span>
  * <span data-ttu-id="e35de-121">Automatické zásady, které vytvářejí, když výchozí zálohování pro tuto možnost svazek byl povolen v době vytváření svazků.</span><span class="sxs-lookup"><span data-stu-id="e35de-121">Automatic policies, which are created when the default backup for this volume option was enabled at the time of volume creation.</span></span> <span data-ttu-id="e35de-122">Tyto zásady jsou pojmenované jako *VolumeName*_výchozí kde *VolumeName* odkazuje na název svazku zařízení StorSimple nakonfigurovaná uživatelem na portálu Azure classic.</span><span class="sxs-lookup"><span data-stu-id="e35de-122">These policies are named as *VolumeName*_Default where *VolumeName* refers to the name of the StorSimple volume configured by the user in the Azure classic portal.</span></span> <span data-ttu-id="e35de-123">Výsledkem automatické zásad denní cloudových snímků od okamžiku zařízení 22:30.</span><span class="sxs-lookup"><span data-stu-id="e35de-123">The automatic policies result in daily cloud snapshots beginning at 22:30 device time.</span></span>
  * <span data-ttu-id="e35de-124">Importované zásady, které byly původně vytvořil v Snapshot Manager zařízení StorSimple.</span><span class="sxs-lookup"><span data-stu-id="e35de-124">Imported policies, which were originally created in the StorSimple Snapshot Manager.</span></span> <span data-ttu-id="e35de-125">Tyto mají značku, která popisuje StorSimple Snapshot Manager hostitele, který zásady, které byly naimportovány z.</span><span class="sxs-lookup"><span data-stu-id="e35de-125">These have a tag that describes the StorSimple Snapshot Manager host that the policies were imported from.</span></span>
* <span data-ttu-id="e35de-126">**Svazky** – svazky přidružených k zásadě.</span><span class="sxs-lookup"><span data-stu-id="e35de-126">**Volumes** – The volumes associated with the policy.</span></span> <span data-ttu-id="e35de-127">Všechny svazky, které jsou přidružené k zásadě zálohování jsou seskupeny dohromady při vytvoření zálohy.</span><span class="sxs-lookup"><span data-stu-id="e35de-127">All the volumes associated with a backup policy are grouped together when backups are created.</span></span>
* <span data-ttu-id="e35de-128">**Poslední úspěšná zálohy** – datum a čas poslední úspěšné zálohy pořízené touto zásadou.</span><span class="sxs-lookup"><span data-stu-id="e35de-128">**Last successful backup** – The date and time of the last successful backup that was taken with this policy.</span></span>
* <span data-ttu-id="e35de-129">**Další zálohování** – datum a čas další naplánované zálohování, které bude inicializován pomocí těchto zásad.</span><span class="sxs-lookup"><span data-stu-id="e35de-129">**Next backup** – The date and time of the next scheduled backup that will be initiated by this policy.</span></span>
* <span data-ttu-id="e35de-130">**Plány** – počet plány přidružené zásady zálohování.</span><span class="sxs-lookup"><span data-stu-id="e35de-130">**Schedules** – The number of schedules associated with the backup policy.</span></span>

<span data-ttu-id="e35de-131">Jsou často používaných operace, které můžete provádět z této stránky:</span><span class="sxs-lookup"><span data-stu-id="e35de-131">The frequently used operations that you can perform from this page are:</span></span>

* <span data-ttu-id="e35de-132">Přidání zásady zálohování</span><span class="sxs-lookup"><span data-stu-id="e35de-132">Add a backup policy</span></span> 
* <span data-ttu-id="e35de-133">Přidat nebo změnit plán</span><span class="sxs-lookup"><span data-stu-id="e35de-133">Add or modify a schedule</span></span> 
* <span data-ttu-id="e35de-134">Odstranit zásady zálohování</span><span class="sxs-lookup"><span data-stu-id="e35de-134">Delete a backup policy</span></span> 
* <span data-ttu-id="e35de-135">Proveďte ruční zálohy</span><span class="sxs-lookup"><span data-stu-id="e35de-135">Take a manual backup</span></span> 
* <span data-ttu-id="e35de-136">Vytvořit vlastní zásady zálohování s více svazků a plány</span><span class="sxs-lookup"><span data-stu-id="e35de-136">Create a custom backup policy with multiple volumes and schedules</span></span> 

## <a name="add-a-backup-policy"></a><span data-ttu-id="e35de-137">Přidání zásady zálohování</span><span class="sxs-lookup"><span data-stu-id="e35de-137">Add a backup policy</span></span>
<span data-ttu-id="e35de-138">Přidáte zásadu zálohování automaticky naplánovat zálohování.</span><span class="sxs-lookup"><span data-stu-id="e35de-138">Add a backup policy to automatically schedule your backups.</span></span> <span data-ttu-id="e35de-139">Proveďte následující kroky na portálu Azure classic přidat zásady zálohování pro zařízení StorSimple.</span><span class="sxs-lookup"><span data-stu-id="e35de-139">Perform the following steps in the Azure classic portal to add a backup policy for your StorSimple device.</span></span> <span data-ttu-id="e35de-140">Po přidání zásady můžete definovat plán (viz [přidat nebo změnit plán](#add-or-modify-a-schedule)).</span><span class="sxs-lookup"><span data-stu-id="e35de-140">After you add the policy, you can define a schedule (see [Add or modify a schedule](#add-or-modify-a-schedule)).</span></span>

[!INCLUDE [storsimple-add-backup-policy-u2](../../includes/storsimple-add-backup-policy-u2.md)]

<span data-ttu-id="e35de-141">![Dostupné video](./media/storsimple-manage-backup-policies-u2/Video_icon.png) **Dostupné video**</span><span class="sxs-lookup"><span data-stu-id="e35de-141">![Video available](./media/storsimple-manage-backup-policies-u2/Video_icon.png) **Video available**</span></span>

<span data-ttu-id="e35de-142">Pokud chcete přehrát video, které ukazuje, jak vytvořit místní nebo cloudové zásady zálohování, klikněte na tlačítko [zde](https://azure.microsoft.com/documentation/videos/create-storsimple-backup-policies/).</span><span class="sxs-lookup"><span data-stu-id="e35de-142">To watch a video that demonstrates how to create a local or cloud backup policy, click [here](https://azure.microsoft.com/documentation/videos/create-storsimple-backup-policies/).</span></span>

## <a name="add-or-modify-a-schedule"></a><span data-ttu-id="e35de-143">Přidat nebo změnit plán</span><span class="sxs-lookup"><span data-stu-id="e35de-143">Add or modify a schedule</span></span>
<span data-ttu-id="e35de-144">Můžete přidat nebo změnit plán, který je připojen k existující zásady zálohování v zařízení StorSimple.</span><span class="sxs-lookup"><span data-stu-id="e35de-144">You can add or modify a schedule that is attached to an existing backup policy on your StorSimple device.</span></span> <span data-ttu-id="e35de-145">Proveďte následující kroky na portálu Azure classic k přidání nebo úpravě plánu.</span><span class="sxs-lookup"><span data-stu-id="e35de-145">Perform the following steps in the Azure classic portal to add or modify a schedule.</span></span>

[!INCLUDE [storsimple-add-modify-backup-schedule](../../includes/storsimple-add-modify-backup-schedule-u2.md)]

## <a name="delete-a-backup-policy"></a><span data-ttu-id="e35de-146">Odstranit zásady zálohování</span><span class="sxs-lookup"><span data-stu-id="e35de-146">Delete a backup policy</span></span>
<span data-ttu-id="e35de-147">Proveďte následující kroky na portálu Azure classic odstranit zásady zálohování v zařízení StorSimple.</span><span class="sxs-lookup"><span data-stu-id="e35de-147">Perform the following steps in the Azure classic portal to delete a backup policy on your StorSimple device.</span></span>

[!INCLUDE [storsimple-delete-backup-policy](../../includes/storsimple-delete-backup-policy.md)]

## <a name="take-a-manual-backup"></a><span data-ttu-id="e35de-148">Proveďte ruční zálohy</span><span class="sxs-lookup"><span data-stu-id="e35de-148">Take a manual backup</span></span>
<span data-ttu-id="e35de-149">Proveďte následující kroky na portálu Azure classic vytvořit zálohu na vyžádání (ručně) pro jeden svazek.</span><span class="sxs-lookup"><span data-stu-id="e35de-149">Perform the following steps in the Azure classic portal to create an on-demand (manual) backup for a single volume.</span></span>

[!INCLUDE [storsimple-create-manual-backup](../../includes/storsimple-create-manual-backup.md)]

## <a name="create-a-custom-backup-policy-with-multiple-volumes-and-schedules"></a><span data-ttu-id="e35de-150">Vytvořit vlastní zásady zálohování s více svazků a plány</span><span class="sxs-lookup"><span data-stu-id="e35de-150">Create a custom backup policy with multiple volumes and schedules</span></span>
<span data-ttu-id="e35de-151">Proveďte následující kroky na portálu Azure classic k vytvoření vlastní zásady zálohování, který má více svazků a plány.</span><span class="sxs-lookup"><span data-stu-id="e35de-151">Perform the following steps in the Azure classic portal to create a custom backup policy that has multiple volumes and schedules.</span></span>

[!INCLUDE [storsimple-create-custom-backup-policy](../../includes/storsimple-create-custom-backup-policy-u2.md)]

## <a name="next-steps"></a><span data-ttu-id="e35de-152">Další kroky</span><span class="sxs-lookup"><span data-stu-id="e35de-152">Next steps</span></span>
<span data-ttu-id="e35de-153">Další informace o [pomocí služby StorSimple Manager ke správě zařízení StorSimple](storsimple-manager-service-administration.md).</span><span class="sxs-lookup"><span data-stu-id="e35de-153">Learn more about [using the StorSimple Manager service to administer your StorSimple device](storsimple-manager-service-administration.md).</span></span>

