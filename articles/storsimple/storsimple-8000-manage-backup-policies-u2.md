---
title: "zásady zálohování řady StorSimple 8000 aaaManage | Microsoft Docs"
description: "Vysvětluje, jak můžete použít toocreate služby StorSimple Manager zařízení hello a spravovat ručního zálohování, plánů zálohování a uchovávání záloh na zařízení řady StorSimple 8000."
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
ms.openlocfilehash: 7c56365abb6ba69d02008829ca6ae703d4632705
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="use-hello-storsimple-device-manager-service-in-azure-portal-toomanage-backup-policies"></a><span data-ttu-id="2bfe4-103">Použít službu StorSimple Manager zařízení hello ve službě Azure portálu toomanage zásady zálohování</span><span class="sxs-lookup"><span data-stu-id="2bfe4-103">Use hello StorSimple Device Manager service in Azure portal toomanage backup policies</span></span>


## <a name="overview"></a><span data-ttu-id="2bfe4-104">Přehled</span><span class="sxs-lookup"><span data-stu-id="2bfe4-104">Overview</span></span>

<span data-ttu-id="2bfe4-105">Tento kurz popisuje, jak toouse hello služby StorSimple Manager zařízení **zálohování zásad** okno toocontrol zálohování procesy a uchovávání záloh pro formátujte svazky zařízení StorSimple.</span><span class="sxs-lookup"><span data-stu-id="2bfe4-105">This tutorial explains how toouse hello StorSimple Device Manager service **Backup policy** blade toocontrol backup processes and backup retention for your StorSimple volumes.</span></span> <span data-ttu-id="2bfe4-106">Také popisuje, jak toocomplete ručního zálohování.</span><span class="sxs-lookup"><span data-stu-id="2bfe4-106">It also describes how toocomplete a manual backup.</span></span>

<span data-ttu-id="2bfe4-107">Při zálohování svazku, můžete toocreate místní snímek nebo cloudový snímek.</span><span class="sxs-lookup"><span data-stu-id="2bfe4-107">When you back up a volume, you can choose toocreate a local snapshot or a cloud snapshot.</span></span> <span data-ttu-id="2bfe4-108">Pokud zálohujete místně vázaný svazek, doporučujeme vám, že zadáváte cloudový snímek.</span><span class="sxs-lookup"><span data-stu-id="2bfe4-108">If you are backing up a locally pinned volume, we recommend that you specify a cloud snapshot.</span></span> <span data-ttu-id="2bfe4-109">Pořízení velký počet místní snímky místně vázaný svazek kombinaci s datovou sadou, která obsahuje mnoho změn způsobí v situaci, ve kterém můžete rychle spustit nedostatek místa na místní.</span><span class="sxs-lookup"><span data-stu-id="2bfe4-109">Taking a large number of local snapshots of a locally pinned volume coupled with a data set that has a lot of churn will result in a situation in which you could rapidly run out of local space.</span></span> <span data-ttu-id="2bfe4-110">Pokud si zvolíte tootake místní snímky, doporučujeme trvat méně denní tooback snímky hello nejnovější stav, uchovávány denně a pak odstraňte je.</span><span class="sxs-lookup"><span data-stu-id="2bfe4-110">If you choose tootake local snapshots, we recommend that you take fewer daily snapshots tooback up hello most recent state, retain them for a day, and then delete them.</span></span>

<span data-ttu-id="2bfe4-111">Pokud pořídíte snímek cloudu místně vázaný svazek, je zkopírovat pouze hello změnit dat toohello v cloudu, kde je s odstraněním duplicitních dat a komprimované.</span><span class="sxs-lookup"><span data-stu-id="2bfe4-111">When you take a cloud snapshot of a locally pinned volume, you copy only hello changed data toohello cloud, where it is deduplicated and compressed.</span></span>

## <a name="hello-backup-policy-blade"></a><span data-ttu-id="2bfe4-112">okno zásady zálohování Hello</span><span class="sxs-lookup"><span data-stu-id="2bfe4-112">hello Backup policy blade</span></span>

<span data-ttu-id="2bfe4-113">Hello **zálohování zásad** okno pro zařízení StorSimple můžete zásady zálohování toomanage a plán místních a cloudových snímků.</span><span class="sxs-lookup"><span data-stu-id="2bfe4-113">hello **Backup policy** blade for your StorSimple device allows you toomanage backup policies and schedule local and cloud snapshots.</span></span> <span data-ttu-id="2bfe4-114">Zásady zálohování jsou použité tooconfigure plánů zálohování a uchovávání záloh pro kolekci svazků.</span><span class="sxs-lookup"><span data-stu-id="2bfe4-114">Backup policies are used tooconfigure backup schedules and backup retention for a collection of volumes.</span></span> <span data-ttu-id="2bfe4-115">Zásady zálohování umožňují tootake snímek více svazků najednou.</span><span class="sxs-lookup"><span data-stu-id="2bfe4-115">Backup policies enable you tootake a snapshot of multiple volumes simultaneously.</span></span> <span data-ttu-id="2bfe4-116">To znamená, že hello zálohy vytvořené zásady zálohování bude kopie konzistentní při selhání.</span><span class="sxs-lookup"><span data-stu-id="2bfe4-116">This means that hello backups created by a backup policy will be crash-consistent copies.</span></span>

<span data-ttu-id="2bfe4-117">výpis tabulkové zásady zálohování Hello také umožňuje vám toofilter hello existující zásady zálohování pro jeden nebo více hello následující pole:</span><span class="sxs-lookup"><span data-stu-id="2bfe4-117">hello backup policies tabular listing also allows you toofilter hello existing backup policies by one or more of hello following fields:</span></span>

* <span data-ttu-id="2bfe4-118">**Název zásady** – hello název spojený s hello zásad.</span><span class="sxs-lookup"><span data-stu-id="2bfe4-118">**Policy name** – hello name associated with hello policy.</span></span> <span data-ttu-id="2bfe4-119">Zahrnout Hello různé typy zásad:</span><span class="sxs-lookup"><span data-stu-id="2bfe4-119">hello different types of policies include:</span></span>

  * <span data-ttu-id="2bfe4-120">Naplánované zásady, které jsou explicitně vytvořené uživatelem hello.</span><span class="sxs-lookup"><span data-stu-id="2bfe4-120">Scheduled policies, which are explicitly created by hello user.</span></span>
  * <span data-ttu-id="2bfe4-121">Importovat zásady, které byly původně vytvořil v hello StorSimple Snapshot Manager.</span><span class="sxs-lookup"><span data-stu-id="2bfe4-121">Imported policies, which were originally created in hello StorSimple Snapshot Manager.</span></span> <span data-ttu-id="2bfe4-122">Tyto mají značku, která popisuje hello StorSimple Snapshot Manager hostitele, který hello zásady, které byly naimportovány z.</span><span class="sxs-lookup"><span data-stu-id="2bfe4-122">These have a tag that describes hello StorSimple Snapshot Manager host that hello policies were imported from.</span></span>

  > [!NOTE]
  > <span data-ttu-id="2bfe4-123">Automatická nebo výchozí zásady zálohování již není povoleno v době hello vytváření svazků.</span><span class="sxs-lookup"><span data-stu-id="2bfe4-123">Automatic or default backup policies are no longer enabled at hello time of volume creation.</span></span>

* <span data-ttu-id="2bfe4-124">**Poslední úspěšná zálohy** – hello datum a čas hello poslední úspěšné zálohy pořízené touto zásadou.</span><span class="sxs-lookup"><span data-stu-id="2bfe4-124">**Last successful backup** – hello date and time of hello last successful backup that was taken with this policy.</span></span>

* <span data-ttu-id="2bfe4-125">**Další zálohování** – hello datum a čas hello další naplánované zálohování, bude inicializován pomocí těchto zásad.</span><span class="sxs-lookup"><span data-stu-id="2bfe4-125">**Next backup** – hello date and time of hello next scheduled backup that will be initiated by this policy.</span></span>

* <span data-ttu-id="2bfe4-126">**Svazky** – hello svazky, které jsou spojené s hello zásadami.</span><span class="sxs-lookup"><span data-stu-id="2bfe4-126">**Volumes** – hello volumes associated with hello policy.</span></span> <span data-ttu-id="2bfe4-127">Všechny svazky hello přidružené k zásadě zálohování jsou seskupeny dohromady při vytvoření zálohy.</span><span class="sxs-lookup"><span data-stu-id="2bfe4-127">All hello volumes associated with a backup policy are grouped together when backups are created.</span></span>

* <span data-ttu-id="2bfe4-128">**Plány** – hello počet plány přidružené zásady zálohování hello.</span><span class="sxs-lookup"><span data-stu-id="2bfe4-128">**Schedules** – hello number of schedules associated with hello backup policy.</span></span>

<span data-ttu-id="2bfe4-129">jsou často používané Hello operace, které můžete provést pro zásady zálohování:</span><span class="sxs-lookup"><span data-stu-id="2bfe4-129">hello frequently used operations that you can perform for backup policies are:</span></span>

* <span data-ttu-id="2bfe4-130">Přidání zásady zálohování</span><span class="sxs-lookup"><span data-stu-id="2bfe4-130">Add a backup policy</span></span>
* <span data-ttu-id="2bfe4-131">Přidat nebo změnit plán</span><span class="sxs-lookup"><span data-stu-id="2bfe4-131">Add or modify a schedule</span></span>
* <span data-ttu-id="2bfe4-132">Přidat nebo odebrat svazku</span><span class="sxs-lookup"><span data-stu-id="2bfe4-132">Add or remove a volume</span></span>
* <span data-ttu-id="2bfe4-133">Odstranit zásady zálohování</span><span class="sxs-lookup"><span data-stu-id="2bfe4-133">Delete a backup policy</span></span>
* <span data-ttu-id="2bfe4-134">Proveďte ruční zálohy</span><span class="sxs-lookup"><span data-stu-id="2bfe4-134">Take a manual backup</span></span>

## <a name="add-a-backup-policy"></a><span data-ttu-id="2bfe4-135">Přidání zásady zálohování</span><span class="sxs-lookup"><span data-stu-id="2bfe4-135">Add a backup policy</span></span>

<span data-ttu-id="2bfe4-136">Přidáte zásady zálohování tooautomatically plán zálohování.</span><span class="sxs-lookup"><span data-stu-id="2bfe4-136">Add a backup policy tooautomatically schedule your backups.</span></span> <span data-ttu-id="2bfe4-137">Při prvním vytvoření svazku, neexistuje žádný výchozí zásady zálohování přidružené k svazku.</span><span class="sxs-lookup"><span data-stu-id="2bfe4-137">When you first create a volume, there is no default backup policy associated with your volume.</span></span> <span data-ttu-id="2bfe4-138">Potřebujete tooadd a přiřadit zásady zálohování dat tooprotect svazku.</span><span class="sxs-lookup"><span data-stu-id="2bfe4-138">You need tooadd and assign a backup policy tooprotect volume data.</span></span>

<span data-ttu-id="2bfe4-139">Proveďte následující kroky v hello Azure portálu tooadd zásady zálohování pro zařízení StorSimple hello.</span><span class="sxs-lookup"><span data-stu-id="2bfe4-139">Perform hello following steps in hello Azure portal tooadd a backup policy for your StorSimple device.</span></span> <span data-ttu-id="2bfe4-140">Po přidání hello zásady můžete definovat plán (viz [přidat nebo změnit plán](#add-or-modify-a-schedule)).</span><span class="sxs-lookup"><span data-stu-id="2bfe4-140">After you add hello policy, you can define a schedule (see [Add or modify a schedule](#add-or-modify-a-schedule)).</span></span>

[!INCLUDE [storsimple-8000-add-backup-policy-u2](../../includes/storsimple-8000-add-backup-policy-u2.md)]

## <a name="add-or-modify-a-schedule"></a><span data-ttu-id="2bfe4-141">Přidat nebo změnit plán</span><span class="sxs-lookup"><span data-stu-id="2bfe4-141">Add or modify a schedule</span></span>

<span data-ttu-id="2bfe4-142">Můžete přidat nebo změnit plán, který je připojený tooan existující zásady zálohování v zařízení StorSimple.</span><span class="sxs-lookup"><span data-stu-id="2bfe4-142">You can add or modify a schedule that is attached tooan existing backup policy on your StorSimple device.</span></span> <span data-ttu-id="2bfe4-143">Proveďte následující kroky v hello Azure portálu tooadd hello nebo změna plánu.</span><span class="sxs-lookup"><span data-stu-id="2bfe4-143">Perform hello following steps in hello Azure portal tooadd or modify a schedule.</span></span>

[!INCLUDE [storsimple-8000-add-modify-backup-schedule](../../includes/storsimple-8000-add-modify-backup-schedule-u2.md)]


## <a name="add-or-remove-a-volume"></a><span data-ttu-id="2bfe4-144">Přidat nebo odebrat svazku</span><span class="sxs-lookup"><span data-stu-id="2bfe4-144">Add or remove a volume</span></span>

<span data-ttu-id="2bfe4-145">Můžete přidat nebo odebrat svazek přiřazené zásady zálohování tooa zařízení StorSimple.</span><span class="sxs-lookup"><span data-stu-id="2bfe4-145">You can add or remove a volume assigned tooa backup policy on your StorSimple device.</span></span> <span data-ttu-id="2bfe4-146">Proveďte následující kroky v hello Azure portálu tooadd hello nebo odeberte svazek.</span><span class="sxs-lookup"><span data-stu-id="2bfe4-146">Perform hello following steps in hello Azure portal tooadd or remove a volume.</span></span>

[!INCLUDE [storsimple-8000-add-volume-backup-policy-u2](../../includes/storsimple-8000-add-remove-volume-backup-policy-u2.md)]


## <a name="delete-a-backup-policy"></a><span data-ttu-id="2bfe4-147">Odstranit zásady zálohování</span><span class="sxs-lookup"><span data-stu-id="2bfe4-147">Delete a backup policy</span></span>

<span data-ttu-id="2bfe4-148">Proveďte následující kroky v hello Azure portálu toodelete zásady zálohování v zařízení StorSimple hello.</span><span class="sxs-lookup"><span data-stu-id="2bfe4-148">Perform hello following steps in hello Azure portal toodelete a backup policy on your StorSimple device.</span></span>

[!INCLUDE [storsimple-8000-delete-backup-policy](../../includes/storsimple-8000-delete-backup-policy.md)]

## <a name="take-a-manual-backup"></a><span data-ttu-id="2bfe4-149">Proveďte ruční zálohy</span><span class="sxs-lookup"><span data-stu-id="2bfe4-149">Take a manual backup</span></span>

<span data-ttu-id="2bfe4-150">Proveďte následující kroky hello Azure portálu toocreate na vyžádání (ručně) zálohování pro samostatný svazek hello.</span><span class="sxs-lookup"><span data-stu-id="2bfe4-150">Perform hello following steps in hello Azure portal toocreate an on-demand (manual) backup for a single volume.</span></span>

[!INCLUDE [storsimple-8000-create-manual-backup](../../includes/storsimple-8000-create-manual-backup.md)]

## <a name="next-steps"></a><span data-ttu-id="2bfe4-151">Další kroky</span><span class="sxs-lookup"><span data-stu-id="2bfe4-151">Next steps</span></span>

<span data-ttu-id="2bfe4-152">Další informace o [pomocí hello tooadminister service Manager zařízení StorSimple zařízení StorSimple](storsimple-8000-manager-service-administration.md).</span><span class="sxs-lookup"><span data-stu-id="2bfe4-152">Learn more about [using hello StorSimple Device Manager service tooadminister your StorSimple device](storsimple-8000-manager-service-administration.md).</span></span>

