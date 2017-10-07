---
title: "aaaManage zásady zálohování pro vaše zařízení StorSimple | Microsoft Docs"
description: "Vysvětluje, jak můžete použít toocreate služby StorSimple Manager hello a spravovat ručního zálohování, plánů zálohování a uchovávání záloh."
services: storsimple
documentationcenter: NA
author: SharS
manager: carmonm
editor: 
ms.assetid: d1834bc8-d520-4463-82ae-3b32fabd021c
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 05/10/2016
ms.author: v-sharos
ms.openlocfilehash: 710cbe54d14031b4de43e9da292ed169085d5af9
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="use-hello-storsimple-manager-service-toomanage-backup-policies"></a><span data-ttu-id="ed9af-103">Použijte zásady zálohování služby toomanage StorSimple Manager pro hello</span><span class="sxs-lookup"><span data-stu-id="ed9af-103">Use hello StorSimple Manager service toomanage backup policies</span></span>
[!INCLUDE [storsimple-version-selector-manage-backup-policies](../../includes/storsimple-version-selector-manage-backup-policies.md)]

## <a name="overview"></a><span data-ttu-id="ed9af-104">Přehled</span><span class="sxs-lookup"><span data-stu-id="ed9af-104">Overview</span></span>
<span data-ttu-id="ed9af-105">Tento kurz popisuje, jak toouse hello služby StorSimple Manager **zásady zálohování** stránka toocontrol procesy zálohování a uchovávání záloh pro formátujte svazky zařízení StorSimple.</span><span class="sxs-lookup"><span data-stu-id="ed9af-105">This tutorial explains how toouse hello StorSimple Manager service **Backup Policies** page toocontrol backup processes and backup retention for your StorSimple volumes.</span></span> <span data-ttu-id="ed9af-106">Také popisuje, jak toocomplete ručního zálohování.</span><span class="sxs-lookup"><span data-stu-id="ed9af-106">It also describes how toocomplete a manual backup.</span></span>

<span data-ttu-id="ed9af-107">Hello **zásady zálohování** stránka vám umožní zásady zálohování toomanage a plán místních a cloudových snímků.</span><span class="sxs-lookup"><span data-stu-id="ed9af-107">hello **Backup Policies** page allows you toomanage backup policies and schedule local and cloud snapshots.</span></span> <span data-ttu-id="ed9af-108">(Zásady zálohování jsou použité tooconfigure plánů zálohování a uchovávání záloh pro kolekci svazků). Zásady zálohování umožňují tootake snímek více svazků najednou.</span><span class="sxs-lookup"><span data-stu-id="ed9af-108">(Backup policies are used tooconfigure backup schedules and backup retention for a collection of volumes.) Backup policies enable you tootake a snapshot of multiple volumes simultaneously.</span></span> <span data-ttu-id="ed9af-109">To znamená, že hello zálohy vytvořené zásady zálohování bude kopie konzistentní při selhání.</span><span class="sxs-lookup"><span data-stu-id="ed9af-109">This means that hello backups created by a backup policy will be crash-consistent copies.</span></span> <span data-ttu-id="ed9af-110">Tato stránka zobrazuje hello zásady zálohování, jejich typy, hello přidružené svazky, hello počet záloh uchována a hello možnost tooenable tyto zásady.</span><span class="sxs-lookup"><span data-stu-id="ed9af-110">This page lists hello backup policies, their types, hello associated volumes, hello number of backups retained, and hello option tooenable these policies.</span></span>

<span data-ttu-id="ed9af-111">Hello **zásady zálohování** stránka vám také umožní toofilter hello existující zásady zálohování pro jednu nebo více hello následující pole:</span><span class="sxs-lookup"><span data-stu-id="ed9af-111">hello **Backup Policies** page also allows you toofilter hello existing backup policies by one or more of hello following fields:</span></span>

* <span data-ttu-id="ed9af-112">**Název zásady** – hello název spojený s hello zásad.</span><span class="sxs-lookup"><span data-stu-id="ed9af-112">**Policy name** – hello name associated with hello policy.</span></span> <span data-ttu-id="ed9af-113">Zahrnout Hello různé typy zásad:</span><span class="sxs-lookup"><span data-stu-id="ed9af-113">hello different types of policies include:</span></span>
  
  * <span data-ttu-id="ed9af-114">Naplánované zásady, které jsou explicitně vytvořené uživatelem hello.</span><span class="sxs-lookup"><span data-stu-id="ed9af-114">Scheduled policies, which are explicitly created by hello user.</span></span>
  * <span data-ttu-id="ed9af-115">Automatické zásady, které vytvářejí, když hello výchozí zálohování pro tuto možnost svazek byl povolen v době hello vytváření svazků.</span><span class="sxs-lookup"><span data-stu-id="ed9af-115">Automatic policies, which are created when hello default backup for this volume option was enabled at hello time of volume creation.</span></span> <span data-ttu-id="ed9af-116">Tyto zásady jsou pojmenované jako VolumeName_Default, kde název svazku odkazuje toohello název hello svazek StorSimple nakonfiguroval hello uživatel v hello portál Azure classic.</span><span class="sxs-lookup"><span data-stu-id="ed9af-116">These policies are named as VolumeName_Default where Volume name refers toohello name of hello StorSimple volume configured by hello user in hello Azure classic portal.</span></span> <span data-ttu-id="ed9af-117">Zásady automatické Hello za následek denní cloudových snímků od okamžiku zařízení 22:30.</span><span class="sxs-lookup"><span data-stu-id="ed9af-117">hello automatic policies result in daily cloud snapshots beginning at 22:30 device time.</span></span>
  * <span data-ttu-id="ed9af-118">Importovat zásady, které byly původně vytvořil v hello StorSimple Snapshot Manager.</span><span class="sxs-lookup"><span data-stu-id="ed9af-118">Imported policies, which were originally created in hello StorSimple Snapshot Manager.</span></span> <span data-ttu-id="ed9af-119">Tyto mají značku, která popisuje hello StorSimple Snapshot Manager hostitele, který hello zásady, které byly naimportovány z.</span><span class="sxs-lookup"><span data-stu-id="ed9af-119">These have a tag that describes hello StorSimple Snapshot Manager host that hello policies were imported from.</span></span>
* <span data-ttu-id="ed9af-120">**Svazky** – hello svazky, které jsou spojené s hello zásadami.</span><span class="sxs-lookup"><span data-stu-id="ed9af-120">**Volumes** – hello volumes associated with hello policy.</span></span> <span data-ttu-id="ed9af-121">Všechny svazky hello přidružené k zásadě zálohování jsou seskupeny dohromady při vytvoření zálohy.</span><span class="sxs-lookup"><span data-stu-id="ed9af-121">All hello volumes associated with a backup policy are grouped together when backups are created.</span></span>
* <span data-ttu-id="ed9af-122">**Poslední úspěšná zálohy** – hello datum a čas hello poslední úspěšné zálohy pořízené touto zásadou.</span><span class="sxs-lookup"><span data-stu-id="ed9af-122">**Last successful backup** – hello date and time of hello last successful backup that was taken with this policy.</span></span>
* <span data-ttu-id="ed9af-123">**Další zálohování** – hello datum a čas hello další naplánované zálohování, bude inicializován pomocí těchto zásad.</span><span class="sxs-lookup"><span data-stu-id="ed9af-123">**Next backup** – hello date and time of hello next scheduled backup that will be initiated by this policy.</span></span>
* <span data-ttu-id="ed9af-124">**Plány** – hello počet plány přidružené zásady zálohování hello.</span><span class="sxs-lookup"><span data-stu-id="ed9af-124">**Schedules** – hello number of schedules associated with hello backup policy.</span></span>

<span data-ttu-id="ed9af-125">jsou často používané Hello operace, které můžete provádět z této stránky:</span><span class="sxs-lookup"><span data-stu-id="ed9af-125">hello frequently used operations that you can perform from this page are:</span></span>

* <span data-ttu-id="ed9af-126">Přidání zásady zálohování</span><span class="sxs-lookup"><span data-stu-id="ed9af-126">Add a backup policy</span></span> 
* <span data-ttu-id="ed9af-127">Přidat nebo změnit plán</span><span class="sxs-lookup"><span data-stu-id="ed9af-127">Add or modify a schedule</span></span> 
* <span data-ttu-id="ed9af-128">Odstranit zásady zálohování</span><span class="sxs-lookup"><span data-stu-id="ed9af-128">Delete a backup policy</span></span> 
* <span data-ttu-id="ed9af-129">Proveďte ruční zálohy</span><span class="sxs-lookup"><span data-stu-id="ed9af-129">Take a manual backup</span></span> 
* <span data-ttu-id="ed9af-130">Vytvořit vlastní zásady zálohování s více svazků a plány</span><span class="sxs-lookup"><span data-stu-id="ed9af-130">Create a custom backup policy with multiple volumes and schedules</span></span> 

## <a name="add-a-backup-policy"></a><span data-ttu-id="ed9af-131">Přidání zásady zálohování</span><span class="sxs-lookup"><span data-stu-id="ed9af-131">Add a backup policy</span></span>
<span data-ttu-id="ed9af-132">Přidáte zásady zálohování tooautomatically plán zálohování.</span><span class="sxs-lookup"><span data-stu-id="ed9af-132">Add a backup policy tooautomatically schedule your backups.</span></span> <span data-ttu-id="ed9af-133">Proveďte následující kroky v hello Azure classic portálu tooadd zásady zálohování pro zařízení StorSimple hello.</span><span class="sxs-lookup"><span data-stu-id="ed9af-133">Perform hello following steps in hello Azure classic portal tooadd a backup policy for your StorSimple device.</span></span> <span data-ttu-id="ed9af-134">Po přidání hello zásady můžete definovat plán (viz [přidat nebo změnit plán](#add-or-modify-a-schedule)).</span><span class="sxs-lookup"><span data-stu-id="ed9af-134">After you add hello policy, you can define a schedule (see [Add or modify a schedule](#add-or-modify-a-schedule)).</span></span>

[!INCLUDE [storsimple-add-backup-policy](../../includes/storsimple-add-backup-policy.md)]

<span data-ttu-id="ed9af-135">![Dostupné video](./media/storsimple-manage-backup-policies/Video_icon.png)**Dostupné video**</span><span class="sxs-lookup"><span data-stu-id="ed9af-135">![Video available](./media/storsimple-manage-backup-policies/Video_icon.png) **Video available**</span></span>

<span data-ttu-id="ed9af-136">Klikněte na tlačítko toowatch video, které ukazuje, jak toocreate místní nebo cloudové zálohování zásad, [zde](https://azure.microsoft.com/documentation/videos/create-storsimple-backup-policies/).</span><span class="sxs-lookup"><span data-stu-id="ed9af-136">toowatch a video that demonstrates how toocreate a local or cloud backup policy, click [here](https://azure.microsoft.com/documentation/videos/create-storsimple-backup-policies/).</span></span>

## <a name="add-or-modify-a-schedule"></a><span data-ttu-id="ed9af-137">Přidat nebo změnit plán</span><span class="sxs-lookup"><span data-stu-id="ed9af-137">Add or modify a schedule</span></span>
<span data-ttu-id="ed9af-138">Můžete přidat nebo změnit plán, který je připojený tooan existující zásady zálohování v zařízení StorSimple.</span><span class="sxs-lookup"><span data-stu-id="ed9af-138">You can add or modify a schedule that is attached tooan existing backup policy on your StorSimple device.</span></span> <span data-ttu-id="ed9af-139">Proveďte následující kroky v hello Azure classic portálu tooadd hello nebo změna plánu.</span><span class="sxs-lookup"><span data-stu-id="ed9af-139">Perform hello following steps in hello Azure classic portal tooadd or modify a schedule.</span></span>

[!INCLUDE [storsimple-add-modify-backup-schedule](../../includes/storsimple-add-modify-backup-schedule.md)]

## <a name="delete-a-backup-policy"></a><span data-ttu-id="ed9af-140">Odstranit zásady zálohování</span><span class="sxs-lookup"><span data-stu-id="ed9af-140">Delete a backup policy</span></span>
<span data-ttu-id="ed9af-141">Proveďte následující kroky v hello Azure classic portálu toodelete zásady zálohování v zařízení StorSimple hello.</span><span class="sxs-lookup"><span data-stu-id="ed9af-141">Perform hello following steps in hello Azure classic portal toodelete a backup policy on your StorSimple device.</span></span>

[!INCLUDE [storsimple-delete-backup-policy](../../includes/storsimple-delete-backup-policy.md)]

## <a name="take-a-manual-backup"></a><span data-ttu-id="ed9af-142">Proveďte ruční zálohy</span><span class="sxs-lookup"><span data-stu-id="ed9af-142">Take a manual backup</span></span>
<span data-ttu-id="ed9af-143">Proveďte následující kroky hello Azure classic portálu toocreate na vyžádání (ručně) zálohování pro samostatný svazek hello.</span><span class="sxs-lookup"><span data-stu-id="ed9af-143">Perform hello following steps in hello Azure classic portal toocreate an on-demand (manual) backup for a single volume.</span></span>

[!INCLUDE [storsimple-create-manual-backup](../../includes/storsimple-create-manual-backup.md)]

## <a name="create-a-custom-backup-policy-with-multiple-volumes-and-schedules"></a><span data-ttu-id="ed9af-144">Vytvořit vlastní zásady zálohování s více svazků a plány</span><span class="sxs-lookup"><span data-stu-id="ed9af-144">Create a custom backup policy with multiple volumes and schedules</span></span>
<span data-ttu-id="ed9af-145">Proveďte následující kroky v hello Azure classic portálu toocreate vlastní zásady zálohování, který má více svazků a plány hello.</span><span class="sxs-lookup"><span data-stu-id="ed9af-145">Perform hello following steps in hello Azure classic portal toocreate a custom backup policy that has multiple volumes and schedules.</span></span>

[!INCLUDE [storsimple-create-custom-backup-policy](../../includes/storsimple-create-custom-backup-policy.md)]

## <a name="next-steps"></a><span data-ttu-id="ed9af-146">Další kroky</span><span class="sxs-lookup"><span data-stu-id="ed9af-146">Next steps</span></span>
<span data-ttu-id="ed9af-147">Další informace o [pomocí hello tooadminister služby StorSimple Manager zařízení StorSimple](storsimple-manager-service-administration.md).</span><span class="sxs-lookup"><span data-stu-id="ed9af-147">Learn more about [using hello StorSimple Manager service tooadminister your StorSimple device](storsimple-manager-service-administration.md).</span></span>

