---
title: "aaaManage zásady zálohování pro vaše zařízení StorSimple | Microsoft Docs"
description: "Vysvětluje, jak můžete použít toocreate služby StorSimple Manager hello a spravovat ručního zálohování, plánů zálohování a uchovávání záloh."
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
ms.openlocfilehash: 7b01f29a8d8a096d9890c8406557021317b9baff
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="use-hello-storsimple-manager-service-toomanage-backup-policies-update-2"></a><span data-ttu-id="2fd02-103">Použít hello StorSimple Manager service toomanage zásady zálohování (Update 2)</span><span class="sxs-lookup"><span data-stu-id="2fd02-103">Use hello StorSimple Manager service toomanage backup policies (Update 2)</span></span>
[!INCLUDE [storsimple-version-selector-manage-backup-policies](../../includes/storsimple-version-selector-manage-backup-policies.md)]

## <a name="overview"></a><span data-ttu-id="2fd02-104">Přehled</span><span class="sxs-lookup"><span data-stu-id="2fd02-104">Overview</span></span>
<span data-ttu-id="2fd02-105">Tento kurz popisuje, jak toouse hello služby StorSimple Manager **zásady zálohování** stránka toocontrol procesy zálohování a uchovávání záloh pro formátujte svazky zařízení StorSimple.</span><span class="sxs-lookup"><span data-stu-id="2fd02-105">This tutorial explains how toouse hello StorSimple Manager service **Backup Policies** page toocontrol backup processes and backup retention for your StorSimple volumes.</span></span> <span data-ttu-id="2fd02-106">Také popisuje, jak toocomplete ručního zálohování.</span><span class="sxs-lookup"><span data-stu-id="2fd02-106">It also describes how toocomplete a manual backup.</span></span>

<span data-ttu-id="2fd02-107">Při zálohování svazku, můžete toocreate místní snímek nebo cloudový snímek.</span><span class="sxs-lookup"><span data-stu-id="2fd02-107">When you back up a volume, you can choose toocreate a local snapshot or a cloud snapshot.</span></span> <span data-ttu-id="2fd02-108">Pokud zálohujete místně vázaný svazek, doporučujeme vám, že zadáváte cloudový snímek.</span><span class="sxs-lookup"><span data-stu-id="2fd02-108">If you are backing up a locally pinned volume, we recommend that you specify a cloud snapshot.</span></span> <span data-ttu-id="2fd02-109">Pořízení velký počet místní snímky místně vázaný svazek kombinaci s datovou sadou, která obsahuje mnoho změn způsobí v situaci, ve kterém můžete rychle spustit nedostatek místa na místní.</span><span class="sxs-lookup"><span data-stu-id="2fd02-109">Taking a large number of local snapshots of a locally pinned volume coupled with a data set that has a lot of churn will result in a situation in which you could rapidly run out of local space.</span></span> <span data-ttu-id="2fd02-110">Pokud si zvolíte tootake místní snímky, doporučujeme trvat méně denní tooback snímky hello nejnovější stav, uchovávány denně a pak odstraňte je.</span><span class="sxs-lookup"><span data-stu-id="2fd02-110">If you choose tootake local snapshots, we recommend that you take fewer daily snapshots tooback up hello most recent state, retain them for a day, and then delete them.</span></span>

<span data-ttu-id="2fd02-111">Pokud pořídíte snímek cloudu místně vázaný svazek, je zkopírovat pouze hello změnit dat toohello v cloudu, kde je s odstraněním duplicitních dat a komprimované.</span><span class="sxs-lookup"><span data-stu-id="2fd02-111">When you take a cloud snapshot of a locally pinned volume, you copy only hello changed data toohello cloud, where it is deduplicated and compressed.</span></span> 

## <a name="hello-backup-policies-page"></a><span data-ttu-id="2fd02-112">Zásady zálohování stránku Hello</span><span class="sxs-lookup"><span data-stu-id="2fd02-112">hello Backup Policies page</span></span>
<span data-ttu-id="2fd02-113">Hello **zásady zálohování** stránka vám umožní zásady zálohování toomanage a plán místních a cloudových snímků.</span><span class="sxs-lookup"><span data-stu-id="2fd02-113">hello **Backup Policies** page allows you toomanage backup policies and schedule local and cloud snapshots.</span></span> <span data-ttu-id="2fd02-114">(Zásady zálohování jsou použité tooconfigure plánů zálohování a uchovávání záloh pro kolekci svazků). Zásady zálohování umožňují tootake snímek více svazků najednou.</span><span class="sxs-lookup"><span data-stu-id="2fd02-114">(Backup policies are used tooconfigure backup schedules and backup retention for a collection of volumes.) Backup policies enable you tootake a snapshot of multiple volumes simultaneously.</span></span> <span data-ttu-id="2fd02-115">To znamená, že hello zálohy vytvořené zásady zálohování bude kopie konzistentní při selhání.</span><span class="sxs-lookup"><span data-stu-id="2fd02-115">This means that hello backups created by a backup policy will be crash-consistent copies.</span></span> <span data-ttu-id="2fd02-116">Hello **zásady zálohování** zobrazuje stránku hello zásady zálohování, jejich typy, hello přidružené svazky, hello počet záloh uchována a hello možnost tooenable tyto zásady.</span><span class="sxs-lookup"><span data-stu-id="2fd02-116">hello **Backup Policies** page lists hello backup policies, their types, hello associated volumes, hello number of backups retained, and hello option tooenable these policies.</span></span>

<span data-ttu-id="2fd02-117">Hello **zásady zálohování** stránka vám také umožní toofilter hello existující zásady zálohování pro jednu nebo více hello následující pole:</span><span class="sxs-lookup"><span data-stu-id="2fd02-117">hello **Backup Policies** page also allows you toofilter hello existing backup policies by one or more of hello following fields:</span></span>

* <span data-ttu-id="2fd02-118">**Název zásady** – hello název spojený s hello zásad.</span><span class="sxs-lookup"><span data-stu-id="2fd02-118">**Policy name** – hello name associated with hello policy.</span></span> <span data-ttu-id="2fd02-119">Zahrnout Hello různé typy zásad:</span><span class="sxs-lookup"><span data-stu-id="2fd02-119">hello different types of policies include:</span></span>
  
  * <span data-ttu-id="2fd02-120">Naplánované zásady, které jsou explicitně vytvořené uživatelem hello.</span><span class="sxs-lookup"><span data-stu-id="2fd02-120">Scheduled policies, which are explicitly created by hello user.</span></span>
  * <span data-ttu-id="2fd02-121">Automatické zásady, které vytvářejí, když hello výchozí zálohování pro tuto možnost svazek byl povolen v době hello vytváření svazků.</span><span class="sxs-lookup"><span data-stu-id="2fd02-121">Automatic policies, which are created when hello default backup for this volume option was enabled at hello time of volume creation.</span></span> <span data-ttu-id="2fd02-122">Tyto zásady jsou pojmenované jako *VolumeName*_výchozí kde *VolumeName* odkazuje toohello název hello svazek StorSimple nakonfiguroval hello uživatel v hello portál Azure classic.</span><span class="sxs-lookup"><span data-stu-id="2fd02-122">These policies are named as *VolumeName*_Default where *VolumeName* refers toohello name of hello StorSimple volume configured by hello user in hello Azure classic portal.</span></span> <span data-ttu-id="2fd02-123">Zásady automatické Hello za následek denní cloudových snímků od okamžiku zařízení 22:30.</span><span class="sxs-lookup"><span data-stu-id="2fd02-123">hello automatic policies result in daily cloud snapshots beginning at 22:30 device time.</span></span>
  * <span data-ttu-id="2fd02-124">Importovat zásady, které byly původně vytvořil v hello StorSimple Snapshot Manager.</span><span class="sxs-lookup"><span data-stu-id="2fd02-124">Imported policies, which were originally created in hello StorSimple Snapshot Manager.</span></span> <span data-ttu-id="2fd02-125">Tyto mají značku, která popisuje hello StorSimple Snapshot Manager hostitele, který hello zásady, které byly naimportovány z.</span><span class="sxs-lookup"><span data-stu-id="2fd02-125">These have a tag that describes hello StorSimple Snapshot Manager host that hello policies were imported from.</span></span>
* <span data-ttu-id="2fd02-126">**Svazky** – hello svazky, které jsou spojené s hello zásadami.</span><span class="sxs-lookup"><span data-stu-id="2fd02-126">**Volumes** – hello volumes associated with hello policy.</span></span> <span data-ttu-id="2fd02-127">Všechny svazky hello přidružené k zásadě zálohování jsou seskupeny dohromady při vytvoření zálohy.</span><span class="sxs-lookup"><span data-stu-id="2fd02-127">All hello volumes associated with a backup policy are grouped together when backups are created.</span></span>
* <span data-ttu-id="2fd02-128">**Poslední úspěšná zálohy** – hello datum a čas hello poslední úspěšné zálohy pořízené touto zásadou.</span><span class="sxs-lookup"><span data-stu-id="2fd02-128">**Last successful backup** – hello date and time of hello last successful backup that was taken with this policy.</span></span>
* <span data-ttu-id="2fd02-129">**Další zálohování** – hello datum a čas hello další naplánované zálohování, bude inicializován pomocí těchto zásad.</span><span class="sxs-lookup"><span data-stu-id="2fd02-129">**Next backup** – hello date and time of hello next scheduled backup that will be initiated by this policy.</span></span>
* <span data-ttu-id="2fd02-130">**Plány** – hello počet plány přidružené zásady zálohování hello.</span><span class="sxs-lookup"><span data-stu-id="2fd02-130">**Schedules** – hello number of schedules associated with hello backup policy.</span></span>

<span data-ttu-id="2fd02-131">jsou často používané Hello operace, které můžete provádět z této stránky:</span><span class="sxs-lookup"><span data-stu-id="2fd02-131">hello frequently used operations that you can perform from this page are:</span></span>

* <span data-ttu-id="2fd02-132">Přidání zásady zálohování</span><span class="sxs-lookup"><span data-stu-id="2fd02-132">Add a backup policy</span></span> 
* <span data-ttu-id="2fd02-133">Přidat nebo změnit plán</span><span class="sxs-lookup"><span data-stu-id="2fd02-133">Add or modify a schedule</span></span> 
* <span data-ttu-id="2fd02-134">Odstranit zásady zálohování</span><span class="sxs-lookup"><span data-stu-id="2fd02-134">Delete a backup policy</span></span> 
* <span data-ttu-id="2fd02-135">Proveďte ruční zálohy</span><span class="sxs-lookup"><span data-stu-id="2fd02-135">Take a manual backup</span></span> 
* <span data-ttu-id="2fd02-136">Vytvořit vlastní zásady zálohování s více svazků a plány</span><span class="sxs-lookup"><span data-stu-id="2fd02-136">Create a custom backup policy with multiple volumes and schedules</span></span> 

## <a name="add-a-backup-policy"></a><span data-ttu-id="2fd02-137">Přidání zásady zálohování</span><span class="sxs-lookup"><span data-stu-id="2fd02-137">Add a backup policy</span></span>
<span data-ttu-id="2fd02-138">Přidáte zásady zálohování tooautomatically plán zálohování.</span><span class="sxs-lookup"><span data-stu-id="2fd02-138">Add a backup policy tooautomatically schedule your backups.</span></span> <span data-ttu-id="2fd02-139">Proveďte následující kroky v hello Azure classic portálu tooadd zásady zálohování pro zařízení StorSimple hello.</span><span class="sxs-lookup"><span data-stu-id="2fd02-139">Perform hello following steps in hello Azure classic portal tooadd a backup policy for your StorSimple device.</span></span> <span data-ttu-id="2fd02-140">Po přidání hello zásady můžete definovat plán (viz [přidat nebo změnit plán](#add-or-modify-a-schedule)).</span><span class="sxs-lookup"><span data-stu-id="2fd02-140">After you add hello policy, you can define a schedule (see [Add or modify a schedule](#add-or-modify-a-schedule)).</span></span>

[!INCLUDE [storsimple-add-backup-policy-u2](../../includes/storsimple-add-backup-policy-u2.md)]

<span data-ttu-id="2fd02-141">![Dostupné video](./media/storsimple-manage-backup-policies-u2/Video_icon.png)**Dostupné video**</span><span class="sxs-lookup"><span data-stu-id="2fd02-141">![Video available](./media/storsimple-manage-backup-policies-u2/Video_icon.png) **Video available**</span></span>

<span data-ttu-id="2fd02-142">Klikněte na tlačítko toowatch video, které ukazuje, jak toocreate místní nebo cloudové zálohování zásad, [zde](https://azure.microsoft.com/documentation/videos/create-storsimple-backup-policies/).</span><span class="sxs-lookup"><span data-stu-id="2fd02-142">toowatch a video that demonstrates how toocreate a local or cloud backup policy, click [here](https://azure.microsoft.com/documentation/videos/create-storsimple-backup-policies/).</span></span>

## <a name="add-or-modify-a-schedule"></a><span data-ttu-id="2fd02-143">Přidat nebo změnit plán</span><span class="sxs-lookup"><span data-stu-id="2fd02-143">Add or modify a schedule</span></span>
<span data-ttu-id="2fd02-144">Můžete přidat nebo změnit plán, který je připojený tooan existující zásady zálohování v zařízení StorSimple.</span><span class="sxs-lookup"><span data-stu-id="2fd02-144">You can add or modify a schedule that is attached tooan existing backup policy on your StorSimple device.</span></span> <span data-ttu-id="2fd02-145">Proveďte následující kroky v hello Azure classic portálu tooadd hello nebo změna plánu.</span><span class="sxs-lookup"><span data-stu-id="2fd02-145">Perform hello following steps in hello Azure classic portal tooadd or modify a schedule.</span></span>

[!INCLUDE [storsimple-add-modify-backup-schedule](../../includes/storsimple-add-modify-backup-schedule-u2.md)]

## <a name="delete-a-backup-policy"></a><span data-ttu-id="2fd02-146">Odstranit zásady zálohování</span><span class="sxs-lookup"><span data-stu-id="2fd02-146">Delete a backup policy</span></span>
<span data-ttu-id="2fd02-147">Proveďte následující kroky v hello Azure classic portálu toodelete zásady zálohování v zařízení StorSimple hello.</span><span class="sxs-lookup"><span data-stu-id="2fd02-147">Perform hello following steps in hello Azure classic portal toodelete a backup policy on your StorSimple device.</span></span>

[!INCLUDE [storsimple-delete-backup-policy](../../includes/storsimple-delete-backup-policy.md)]

## <a name="take-a-manual-backup"></a><span data-ttu-id="2fd02-148">Proveďte ruční zálohy</span><span class="sxs-lookup"><span data-stu-id="2fd02-148">Take a manual backup</span></span>
<span data-ttu-id="2fd02-149">Proveďte následující kroky hello Azure classic portálu toocreate na vyžádání (ručně) zálohování pro samostatný svazek hello.</span><span class="sxs-lookup"><span data-stu-id="2fd02-149">Perform hello following steps in hello Azure classic portal toocreate an on-demand (manual) backup for a single volume.</span></span>

[!INCLUDE [storsimple-create-manual-backup](../../includes/storsimple-create-manual-backup.md)]

## <a name="create-a-custom-backup-policy-with-multiple-volumes-and-schedules"></a><span data-ttu-id="2fd02-150">Vytvořit vlastní zásady zálohování s více svazků a plány</span><span class="sxs-lookup"><span data-stu-id="2fd02-150">Create a custom backup policy with multiple volumes and schedules</span></span>
<span data-ttu-id="2fd02-151">Proveďte následující kroky v hello Azure classic portálu toocreate vlastní zásady zálohování, který má více svazků a plány hello.</span><span class="sxs-lookup"><span data-stu-id="2fd02-151">Perform hello following steps in hello Azure classic portal toocreate a custom backup policy that has multiple volumes and schedules.</span></span>

[!INCLUDE [storsimple-create-custom-backup-policy](../../includes/storsimple-create-custom-backup-policy-u2.md)]

## <a name="next-steps"></a><span data-ttu-id="2fd02-152">Další kroky</span><span class="sxs-lookup"><span data-stu-id="2fd02-152">Next steps</span></span>
<span data-ttu-id="2fd02-153">Další informace o [pomocí hello tooadminister služby StorSimple Manager zařízení StorSimple](storsimple-manager-service-administration.md).</span><span class="sxs-lookup"><span data-stu-id="2fd02-153">Learn more about [using hello StorSimple Manager service tooadminister your StorSimple device](storsimple-manager-service-administration.md).</span></span>

