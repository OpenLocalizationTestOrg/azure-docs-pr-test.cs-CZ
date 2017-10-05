---
title: "Spravovat zásady zálohování pro vaše zařízení StorSimple | Microsoft Docs"
description: "Vysvětluje, jak je možné použít službu StorSimple Manager k vytváření a správě ručního zálohování, plánů zálohování a uchovávání záloh."
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
ms.openlocfilehash: c1e9d5d0450bab5d371aafb40fd7c5920d39dfdb
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="use-the-storsimple-manager-service-to-manage-backup-policies"></a><span data-ttu-id="47a58-103">Použít službu StorSimple Manager ke správě zásady zálohování</span><span class="sxs-lookup"><span data-stu-id="47a58-103">Use the StorSimple Manager service to manage backup policies</span></span>
[!INCLUDE [storsimple-version-selector-manage-backup-policies](../../includes/storsimple-version-selector-manage-backup-policies.md)]

## <a name="overview"></a><span data-ttu-id="47a58-104">Přehled</span><span class="sxs-lookup"><span data-stu-id="47a58-104">Overview</span></span>
<span data-ttu-id="47a58-105">Tento kurz vysvětluje, jak používat službu StorSimple Manager **zásady zálohování** stránky k řízení procesů zálohování a uchovávání záloh pro formátujte svazky zařízení StorSimple.</span><span class="sxs-lookup"><span data-stu-id="47a58-105">This tutorial explains how to use the StorSimple Manager service **Backup Policies** page to control backup processes and backup retention for your StorSimple volumes.</span></span> <span data-ttu-id="47a58-106">Také popisuje, jak provést ruční zálohy.</span><span class="sxs-lookup"><span data-stu-id="47a58-106">It also describes how to complete a manual backup.</span></span>

<span data-ttu-id="47a58-107">**Zásady zálohování** stránka umožňuje spravovat zásady zálohování a plánování místních a cloudových snímků.</span><span class="sxs-lookup"><span data-stu-id="47a58-107">The **Backup Policies** page allows you to manage backup policies and schedule local and cloud snapshots.</span></span> <span data-ttu-id="47a58-108">(Zásady zálohování se používají ke konfiguraci plánů zálohování a uchovávání záloh pro kolekci svazků). Zásady zálohování umožňují pořízení snímku více svazků najednou.</span><span class="sxs-lookup"><span data-stu-id="47a58-108">(Backup policies are used to configure backup schedules and backup retention for a collection of volumes.) Backup policies enable you to take a snapshot of multiple volumes simultaneously.</span></span> <span data-ttu-id="47a58-109">To znamená, že zálohy vytvořené zásady zálohování bude kopie konzistentní při selhání.</span><span class="sxs-lookup"><span data-stu-id="47a58-109">This means that the backups created by a backup policy will be crash-consistent copies.</span></span> <span data-ttu-id="47a58-110">Tato stránka obsahuje seznam zásady zálohování, jejich typy, přidružené svazky, počet záloh uchována a možnost povolit tyto zásady.</span><span class="sxs-lookup"><span data-stu-id="47a58-110">This page lists the backup policies, their types, the associated volumes, the number of backups retained, and the option to enable these policies.</span></span>

<span data-ttu-id="47a58-111">**Zásady zálohování** stránce můžete také filtrovat existující zásady zálohování jednu nebo více z následujících polí:</span><span class="sxs-lookup"><span data-stu-id="47a58-111">The **Backup Policies** page also allows you to filter the existing backup policies by one or more of the following fields:</span></span>

* <span data-ttu-id="47a58-112">**Název zásady** – název přidružených k zásadě.</span><span class="sxs-lookup"><span data-stu-id="47a58-112">**Policy name** – The name associated with the policy.</span></span> <span data-ttu-id="47a58-113">Různé typy zásady patří:</span><span class="sxs-lookup"><span data-stu-id="47a58-113">The different types of policies include:</span></span>
  
  * <span data-ttu-id="47a58-114">Naplánované zásady, které jsou explicitně vytvořený uživatelem.</span><span class="sxs-lookup"><span data-stu-id="47a58-114">Scheduled policies, which are explicitly created by the user.</span></span>
  * <span data-ttu-id="47a58-115">Automatické zásady, které vytvářejí, když výchozí zálohování pro tuto možnost svazek byl povolen v době vytváření svazků.</span><span class="sxs-lookup"><span data-stu-id="47a58-115">Automatic policies, which are created when the default backup for this volume option was enabled at the time of volume creation.</span></span> <span data-ttu-id="47a58-116">Tyto zásady jsou pojmenované jako VolumeName_Default, kde název svazku odkazuje na název svazku zařízení StorSimple nakonfigurovaná uživatelem na portálu Azure classic.</span><span class="sxs-lookup"><span data-stu-id="47a58-116">These policies are named as VolumeName_Default where Volume name refers to the name of the StorSimple volume configured by the user in the Azure classic portal.</span></span> <span data-ttu-id="47a58-117">Výsledkem automatické zásad denní cloudových snímků od okamžiku zařízení 22:30.</span><span class="sxs-lookup"><span data-stu-id="47a58-117">The automatic policies result in daily cloud snapshots beginning at 22:30 device time.</span></span>
  * <span data-ttu-id="47a58-118">Importované zásady, které byly původně vytvořil v Snapshot Manager zařízení StorSimple.</span><span class="sxs-lookup"><span data-stu-id="47a58-118">Imported policies, which were originally created in the StorSimple Snapshot Manager.</span></span> <span data-ttu-id="47a58-119">Tyto mají značku, která popisuje StorSimple Snapshot Manager hostitele, který zásady, které byly naimportovány z.</span><span class="sxs-lookup"><span data-stu-id="47a58-119">These have a tag that describes the StorSimple Snapshot Manager host that the policies were imported from.</span></span>
* <span data-ttu-id="47a58-120">**Svazky** – svazky přidružených k zásadě.</span><span class="sxs-lookup"><span data-stu-id="47a58-120">**Volumes** – The volumes associated with the policy.</span></span> <span data-ttu-id="47a58-121">Všechny svazky, které jsou přidružené k zásadě zálohování jsou seskupeny dohromady při vytvoření zálohy.</span><span class="sxs-lookup"><span data-stu-id="47a58-121">All the volumes associated with a backup policy are grouped together when backups are created.</span></span>
* <span data-ttu-id="47a58-122">**Poslední úspěšná zálohy** – datum a čas poslední úspěšné zálohy pořízené touto zásadou.</span><span class="sxs-lookup"><span data-stu-id="47a58-122">**Last successful backup** – The date and time of the last successful backup that was taken with this policy.</span></span>
* <span data-ttu-id="47a58-123">**Další zálohování** – datum a čas další naplánované zálohování, které bude inicializován pomocí těchto zásad.</span><span class="sxs-lookup"><span data-stu-id="47a58-123">**Next backup** – The date and time of the next scheduled backup that will be initiated by this policy.</span></span>
* <span data-ttu-id="47a58-124">**Plány** – počet plány přidružené zásady zálohování.</span><span class="sxs-lookup"><span data-stu-id="47a58-124">**Schedules** – The number of schedules associated with the backup policy.</span></span>

<span data-ttu-id="47a58-125">Jsou často používaných operace, které můžete provádět z této stránky:</span><span class="sxs-lookup"><span data-stu-id="47a58-125">The frequently used operations that you can perform from this page are:</span></span>

* <span data-ttu-id="47a58-126">Přidání zásady zálohování</span><span class="sxs-lookup"><span data-stu-id="47a58-126">Add a backup policy</span></span> 
* <span data-ttu-id="47a58-127">Přidat nebo změnit plán</span><span class="sxs-lookup"><span data-stu-id="47a58-127">Add or modify a schedule</span></span> 
* <span data-ttu-id="47a58-128">Odstranit zásady zálohování</span><span class="sxs-lookup"><span data-stu-id="47a58-128">Delete a backup policy</span></span> 
* <span data-ttu-id="47a58-129">Proveďte ruční zálohy</span><span class="sxs-lookup"><span data-stu-id="47a58-129">Take a manual backup</span></span> 
* <span data-ttu-id="47a58-130">Vytvořit vlastní zásady zálohování s více svazků a plány</span><span class="sxs-lookup"><span data-stu-id="47a58-130">Create a custom backup policy with multiple volumes and schedules</span></span> 

## <a name="add-a-backup-policy"></a><span data-ttu-id="47a58-131">Přidání zásady zálohování</span><span class="sxs-lookup"><span data-stu-id="47a58-131">Add a backup policy</span></span>
<span data-ttu-id="47a58-132">Přidáte zásadu zálohování automaticky naplánovat zálohování.</span><span class="sxs-lookup"><span data-stu-id="47a58-132">Add a backup policy to automatically schedule your backups.</span></span> <span data-ttu-id="47a58-133">Proveďte následující kroky na portálu Azure classic přidat zásady zálohování pro zařízení StorSimple.</span><span class="sxs-lookup"><span data-stu-id="47a58-133">Perform the following steps in the Azure classic portal to add a backup policy for your StorSimple device.</span></span> <span data-ttu-id="47a58-134">Po přidání zásady můžete definovat plán (viz [přidat nebo změnit plán](#add-or-modify-a-schedule)).</span><span class="sxs-lookup"><span data-stu-id="47a58-134">After you add the policy, you can define a schedule (see [Add or modify a schedule](#add-or-modify-a-schedule)).</span></span>

[!INCLUDE [storsimple-add-backup-policy](../../includes/storsimple-add-backup-policy.md)]

<span data-ttu-id="47a58-135">![Dostupné video](./media/storsimple-manage-backup-policies/Video_icon.png) **Dostupné video**</span><span class="sxs-lookup"><span data-stu-id="47a58-135">![Video available](./media/storsimple-manage-backup-policies/Video_icon.png) **Video available**</span></span>

<span data-ttu-id="47a58-136">Pokud chcete přehrát video, které ukazuje, jak vytvořit místní nebo cloudové zásady zálohování, klikněte na tlačítko [zde](https://azure.microsoft.com/documentation/videos/create-storsimple-backup-policies/).</span><span class="sxs-lookup"><span data-stu-id="47a58-136">To watch a video that demonstrates how to create a local or cloud backup policy, click [here](https://azure.microsoft.com/documentation/videos/create-storsimple-backup-policies/).</span></span>

## <a name="add-or-modify-a-schedule"></a><span data-ttu-id="47a58-137">Přidat nebo změnit plán</span><span class="sxs-lookup"><span data-stu-id="47a58-137">Add or modify a schedule</span></span>
<span data-ttu-id="47a58-138">Můžete přidat nebo změnit plán, který je připojen k existující zásady zálohování v zařízení StorSimple.</span><span class="sxs-lookup"><span data-stu-id="47a58-138">You can add or modify a schedule that is attached to an existing backup policy on your StorSimple device.</span></span> <span data-ttu-id="47a58-139">Proveďte následující kroky na portálu Azure classic k přidání nebo úpravě plánu.</span><span class="sxs-lookup"><span data-stu-id="47a58-139">Perform the following steps in the Azure classic portal to add or modify a schedule.</span></span>

[!INCLUDE [storsimple-add-modify-backup-schedule](../../includes/storsimple-add-modify-backup-schedule.md)]

## <a name="delete-a-backup-policy"></a><span data-ttu-id="47a58-140">Odstranit zásady zálohování</span><span class="sxs-lookup"><span data-stu-id="47a58-140">Delete a backup policy</span></span>
<span data-ttu-id="47a58-141">Proveďte následující kroky na portálu Azure classic odstranit zásady zálohování v zařízení StorSimple.</span><span class="sxs-lookup"><span data-stu-id="47a58-141">Perform the following steps in the Azure classic portal to delete a backup policy on your StorSimple device.</span></span>

[!INCLUDE [storsimple-delete-backup-policy](../../includes/storsimple-delete-backup-policy.md)]

## <a name="take-a-manual-backup"></a><span data-ttu-id="47a58-142">Proveďte ruční zálohy</span><span class="sxs-lookup"><span data-stu-id="47a58-142">Take a manual backup</span></span>
<span data-ttu-id="47a58-143">Proveďte následující kroky na portálu Azure classic vytvořit zálohu na vyžádání (ručně) pro jeden svazek.</span><span class="sxs-lookup"><span data-stu-id="47a58-143">Perform the following steps in the Azure classic portal to create an on-demand (manual) backup for a single volume.</span></span>

[!INCLUDE [storsimple-create-manual-backup](../../includes/storsimple-create-manual-backup.md)]

## <a name="create-a-custom-backup-policy-with-multiple-volumes-and-schedules"></a><span data-ttu-id="47a58-144">Vytvořit vlastní zásady zálohování s více svazků a plány</span><span class="sxs-lookup"><span data-stu-id="47a58-144">Create a custom backup policy with multiple volumes and schedules</span></span>
<span data-ttu-id="47a58-145">Proveďte následující kroky na portálu Azure classic k vytvoření vlastní zásady zálohování, který má více svazků a plány.</span><span class="sxs-lookup"><span data-stu-id="47a58-145">Perform the following steps in the Azure classic portal to create a custom backup policy that has multiple volumes and schedules.</span></span>

[!INCLUDE [storsimple-create-custom-backup-policy](../../includes/storsimple-create-custom-backup-policy.md)]

## <a name="next-steps"></a><span data-ttu-id="47a58-146">Další kroky</span><span class="sxs-lookup"><span data-stu-id="47a58-146">Next steps</span></span>
<span data-ttu-id="47a58-147">Další informace o [pomocí služby StorSimple Manager ke správě zařízení StorSimple](storsimple-manager-service-administration.md).</span><span class="sxs-lookup"><span data-stu-id="47a58-147">Learn more about [using the StorSimple Manager service to administer your StorSimple device](storsimple-manager-service-administration.md).</span></span>

