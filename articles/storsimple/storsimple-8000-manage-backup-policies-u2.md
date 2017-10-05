---
title: "Spravovat zásady zálohování řady StorSimple 8000 | Microsoft Docs"
description: "Vysvětluje, jak je možné použít službu StorSimple Manager zařízení k vytváření a správě ručního zálohování, plánů zálohování a uchovávání záloh na zařízení řady StorSimple 8000."
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
ms.date: 07/05/2017
ms.author: alkohli
ms.openlocfilehash: 569dbfdeb7dcd526cb5a54b487ea1bfb59b13cc6
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="use-the-storsimple-device-manager-service-in-azure-portal-to-manage-backup-policies"></a><span data-ttu-id="3446c-103">Použít službu StorSimple Manager zařízení na portálu Azure ke správě zásady zálohování</span><span class="sxs-lookup"><span data-stu-id="3446c-103">Use the StorSimple Device Manager service in Azure portal to manage backup policies</span></span>


## <a name="overview"></a><span data-ttu-id="3446c-104">Přehled</span><span class="sxs-lookup"><span data-stu-id="3446c-104">Overview</span></span>

<span data-ttu-id="3446c-105">Tento kurz vysvětluje, jak používat službu StorSimple Manager zařízení **zálohování zásad** okno k řízení procesů zálohování a uchovávání záloh pro formátujte svazky zařízení StorSimple.</span><span class="sxs-lookup"><span data-stu-id="3446c-105">This tutorial explains how to use the StorSimple Device Manager service **Backup policy** blade to control backup processes and backup retention for your StorSimple volumes.</span></span> <span data-ttu-id="3446c-106">Také popisuje, jak provést ruční zálohy.</span><span class="sxs-lookup"><span data-stu-id="3446c-106">It also describes how to complete a manual backup.</span></span>

<span data-ttu-id="3446c-107">Při zálohování svazku, můžete vytvořit místní snímek nebo cloudový snímek.</span><span class="sxs-lookup"><span data-stu-id="3446c-107">When you back up a volume, you can choose to create a local snapshot or a cloud snapshot.</span></span> <span data-ttu-id="3446c-108">Pokud zálohujete místně vázaný svazek, doporučujeme vám, že zadáváte cloudový snímek.</span><span class="sxs-lookup"><span data-stu-id="3446c-108">If you are backing up a locally pinned volume, we recommend that you specify a cloud snapshot.</span></span> <span data-ttu-id="3446c-109">Pořízení velký počet místní snímky místně vázaný svazek kombinaci s datovou sadou, která obsahuje mnoho změn způsobí v situaci, ve kterém můžete rychle spustit nedostatek místa na místní.</span><span class="sxs-lookup"><span data-stu-id="3446c-109">Taking a large number of local snapshots of a locally pinned volume coupled with a data set that has a lot of churn will result in a situation in which you could rapidly run out of local space.</span></span> <span data-ttu-id="3446c-110">Pokud se rozhodnete převést místní snímky, doporučujeme provést méně snímky denní zálohování nejnovější stav, je zachování za den a pak odstraňte je.</span><span class="sxs-lookup"><span data-stu-id="3446c-110">If you choose to take local snapshots, we recommend that you take fewer daily snapshots to back up the most recent state, retain them for a day, and then delete them.</span></span>

<span data-ttu-id="3446c-111">Pokud pořídíte snímek cloudu místně vázaný svazek, zkopírovat pouze změněné data do cloudu, kde je s odstraněním duplicitních dat a komprimované.</span><span class="sxs-lookup"><span data-stu-id="3446c-111">When you take a cloud snapshot of a locally pinned volume, you copy only the changed data to the cloud, where it is deduplicated and compressed.</span></span>

## <a name="the-backup-policy-blade"></a><span data-ttu-id="3446c-112">Okno zásady zálohování</span><span class="sxs-lookup"><span data-stu-id="3446c-112">The Backup policy blade</span></span>

<span data-ttu-id="3446c-113">**Zálohování zásad** okno pro zařízení StorSimple umožňuje spravovat zásady zálohování a plánování místních a cloudových snímků.</span><span class="sxs-lookup"><span data-stu-id="3446c-113">The **Backup policy** blade for your StorSimple device allows you to manage backup policies and schedule local and cloud snapshots.</span></span> <span data-ttu-id="3446c-114">Zásady zálohování se používají ke konfiguraci plánů zálohování a uchovávání záloh pro kolekci svazků.</span><span class="sxs-lookup"><span data-stu-id="3446c-114">Backup policies are used to configure backup schedules and backup retention for a collection of volumes.</span></span> <span data-ttu-id="3446c-115">Zásady zálohování umožňují pořízení snímku více svazků najednou.</span><span class="sxs-lookup"><span data-stu-id="3446c-115">Backup policies enable you to take a snapshot of multiple volumes simultaneously.</span></span> <span data-ttu-id="3446c-116">To znamená, že zálohy vytvořené zásady zálohování bude kopie konzistentní při selhání.</span><span class="sxs-lookup"><span data-stu-id="3446c-116">This means that the backups created by a backup policy will be crash-consistent copies.</span></span>

<span data-ttu-id="3446c-117">Výpis tabulkové zásady zálohování také vám umožní filtrovat existující zásady zálohování jednu nebo více z následujících polí:</span><span class="sxs-lookup"><span data-stu-id="3446c-117">The backup policies tabular listing also allows you to filter the existing backup policies by one or more of the following fields:</span></span>

* <span data-ttu-id="3446c-118">**Název zásady** – název přidružených k zásadě.</span><span class="sxs-lookup"><span data-stu-id="3446c-118">**Policy name** – The name associated with the policy.</span></span> <span data-ttu-id="3446c-119">Různé typy zásady patří:</span><span class="sxs-lookup"><span data-stu-id="3446c-119">The different types of policies include:</span></span>

  * <span data-ttu-id="3446c-120">Naplánované zásady, které jsou explicitně vytvořený uživatelem.</span><span class="sxs-lookup"><span data-stu-id="3446c-120">Scheduled policies, which are explicitly created by the user.</span></span>
  * <span data-ttu-id="3446c-121">Importované zásady, které byly původně vytvořil v Snapshot Manager zařízení StorSimple.</span><span class="sxs-lookup"><span data-stu-id="3446c-121">Imported policies, which were originally created in the StorSimple Snapshot Manager.</span></span> <span data-ttu-id="3446c-122">Tyto mají značku, která popisuje StorSimple Snapshot Manager hostitele, který zásady, které byly naimportovány z.</span><span class="sxs-lookup"><span data-stu-id="3446c-122">These have a tag that describes the StorSimple Snapshot Manager host that the policies were imported from.</span></span>

  > [!NOTE]
  > <span data-ttu-id="3446c-123">Automatická nebo výchozí zásady zálohování již není povoleno v době vytváření svazků.</span><span class="sxs-lookup"><span data-stu-id="3446c-123">Automatic or default backup policies are no longer enabled at the time of volume creation.</span></span>

* <span data-ttu-id="3446c-124">**Poslední úspěšná zálohy** – datum a čas poslední úspěšné zálohy pořízené touto zásadou.</span><span class="sxs-lookup"><span data-stu-id="3446c-124">**Last successful backup** – The date and time of the last successful backup that was taken with this policy.</span></span>

* <span data-ttu-id="3446c-125">**Další zálohování** – datum a čas další naplánované zálohování, které bude inicializován pomocí těchto zásad.</span><span class="sxs-lookup"><span data-stu-id="3446c-125">**Next backup** – The date and time of the next scheduled backup that will be initiated by this policy.</span></span>

* <span data-ttu-id="3446c-126">**Svazky** – svazky přidružených k zásadě.</span><span class="sxs-lookup"><span data-stu-id="3446c-126">**Volumes** – The volumes associated with the policy.</span></span> <span data-ttu-id="3446c-127">Všechny svazky, které jsou přidružené k zásadě zálohování jsou seskupeny dohromady při vytvoření zálohy.</span><span class="sxs-lookup"><span data-stu-id="3446c-127">All the volumes associated with a backup policy are grouped together when backups are created.</span></span>

* <span data-ttu-id="3446c-128">**Plány** – počet plány přidružené zásady zálohování.</span><span class="sxs-lookup"><span data-stu-id="3446c-128">**Schedules** – The number of schedules associated with the backup policy.</span></span>

<span data-ttu-id="3446c-129">Jsou často používaných operace, které můžete provést pro zásady zálohování:</span><span class="sxs-lookup"><span data-stu-id="3446c-129">The frequently used operations that you can perform for backup policies are:</span></span>

* <span data-ttu-id="3446c-130">Přidání zásady zálohování</span><span class="sxs-lookup"><span data-stu-id="3446c-130">Add a backup policy</span></span>
* <span data-ttu-id="3446c-131">Přidat nebo změnit plán</span><span class="sxs-lookup"><span data-stu-id="3446c-131">Add or modify a schedule</span></span>
* <span data-ttu-id="3446c-132">Přidat nebo odebrat svazku</span><span class="sxs-lookup"><span data-stu-id="3446c-132">Add or remove a volume</span></span>
* <span data-ttu-id="3446c-133">Odstranit zásady zálohování</span><span class="sxs-lookup"><span data-stu-id="3446c-133">Delete a backup policy</span></span>
* <span data-ttu-id="3446c-134">Proveďte ruční zálohy</span><span class="sxs-lookup"><span data-stu-id="3446c-134">Take a manual backup</span></span>

## <a name="add-a-backup-policy"></a><span data-ttu-id="3446c-135">Přidání zásady zálohování</span><span class="sxs-lookup"><span data-stu-id="3446c-135">Add a backup policy</span></span>

<span data-ttu-id="3446c-136">Přidáte zásadu zálohování automaticky naplánovat zálohování.</span><span class="sxs-lookup"><span data-stu-id="3446c-136">Add a backup policy to automatically schedule your backups.</span></span> <span data-ttu-id="3446c-137">Při prvním vytvoření svazku, neexistuje žádný výchozí zásady zálohování přidružené k svazku.</span><span class="sxs-lookup"><span data-stu-id="3446c-137">When you first create a volume, there is no default backup policy associated with your volume.</span></span> <span data-ttu-id="3446c-138">Budete muset přidat a přiřadit zásady zálohování pro ochranu dat svazku.</span><span class="sxs-lookup"><span data-stu-id="3446c-138">You need to add and assign a backup policy to protect volume data.</span></span>

<span data-ttu-id="3446c-139">Proveďte následující kroky na portálu Azure přidat zásady zálohování pro zařízení StorSimple.</span><span class="sxs-lookup"><span data-stu-id="3446c-139">Perform the following steps in the Azure portal to add a backup policy for your StorSimple device.</span></span> <span data-ttu-id="3446c-140">Po přidání zásady můžete definovat plán (viz [přidat nebo změnit plán](#add-or-modify-a-schedule)).</span><span class="sxs-lookup"><span data-stu-id="3446c-140">After you add the policy, you can define a schedule (see [Add or modify a schedule](#add-or-modify-a-schedule)).</span></span>

[!INCLUDE [storsimple-8000-add-backup-policy-u2](../../includes/storsimple-8000-add-backup-policy-u2.md)]

## <a name="add-or-modify-a-schedule"></a><span data-ttu-id="3446c-141">Přidat nebo změnit plán</span><span class="sxs-lookup"><span data-stu-id="3446c-141">Add or modify a schedule</span></span>

<span data-ttu-id="3446c-142">Můžete přidat nebo změnit plán, který je připojen k existující zásady zálohování v zařízení StorSimple.</span><span class="sxs-lookup"><span data-stu-id="3446c-142">You can add or modify a schedule that is attached to an existing backup policy on your StorSimple device.</span></span> <span data-ttu-id="3446c-143">Proveďte následující kroky na portálu Azure přidat nebo upravit plán.</span><span class="sxs-lookup"><span data-stu-id="3446c-143">Perform the following steps in the Azure portal to add or modify a schedule.</span></span>

[!INCLUDE [storsimple-8000-add-modify-backup-schedule](../../includes/storsimple-8000-add-modify-backup-schedule-u2.md)]


## <a name="add-or-remove-a-volume"></a><span data-ttu-id="3446c-144">Přidat nebo odebrat svazku</span><span class="sxs-lookup"><span data-stu-id="3446c-144">Add or remove a volume</span></span>

<span data-ttu-id="3446c-145">Můžete přidat nebo odebrat svazek přiřazené zásady zálohování v zařízení StorSimple.</span><span class="sxs-lookup"><span data-stu-id="3446c-145">You can add or remove a volume assigned to a backup policy on your StorSimple device.</span></span> <span data-ttu-id="3446c-146">Proveďte následující kroky na portálu Azure můžete přidat nebo odebrat svazek.</span><span class="sxs-lookup"><span data-stu-id="3446c-146">Perform the following steps in the Azure portal to add or remove a volume.</span></span>

[!INCLUDE [storsimple-8000-add-volume-backup-policy-u2](../../includes/storsimple-8000-add-remove-volume-backup-policy-u2.md)]


## <a name="delete-a-backup-policy"></a><span data-ttu-id="3446c-147">Odstranit zásady zálohování</span><span class="sxs-lookup"><span data-stu-id="3446c-147">Delete a backup policy</span></span>

<span data-ttu-id="3446c-148">Proveďte následující kroky na portálu Azure odstranit zásady zálohování v zařízení StorSimple.</span><span class="sxs-lookup"><span data-stu-id="3446c-148">Perform the following steps in the Azure portal to delete a backup policy on your StorSimple device.</span></span>

[!INCLUDE [storsimple-8000-delete-backup-policy](../../includes/storsimple-8000-delete-backup-policy.md)]

## <a name="take-a-manual-backup"></a><span data-ttu-id="3446c-149">Proveďte ruční zálohy</span><span class="sxs-lookup"><span data-stu-id="3446c-149">Take a manual backup</span></span>

<span data-ttu-id="3446c-150">Proveďte následující kroky na portálu Azure vytvořit zálohu na vyžádání (ručně) pro jeden svazek.</span><span class="sxs-lookup"><span data-stu-id="3446c-150">Perform the following steps in the Azure portal to create an on-demand (manual) backup for a single volume.</span></span>

[!INCLUDE [storsimple-8000-create-manual-backup](../../includes/storsimple-8000-create-manual-backup.md)]

## <a name="next-steps"></a><span data-ttu-id="3446c-151">Další kroky</span><span class="sxs-lookup"><span data-stu-id="3446c-151">Next steps</span></span>

<span data-ttu-id="3446c-152">Další informace o [pomocí služby StorSimple Manager zařízení ke správě zařízení StorSimple](storsimple-8000-manager-service-administration.md).</span><span class="sxs-lookup"><span data-stu-id="3446c-152">Learn more about [using the StorSimple Device Manager service to administer your StorSimple device](storsimple-8000-manager-service-administration.md).</span></span>

