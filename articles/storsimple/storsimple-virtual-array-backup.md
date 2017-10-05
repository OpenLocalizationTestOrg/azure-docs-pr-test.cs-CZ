---
title: "Kurz zálohování Microsoft Azure StorSimple virtuální pole | Microsoft Docs"
description: "Popisuje, jak vytvořit zálohu StorSimple virtuální pole složkami a svazky."
services: storsimple
documentationcenter: NA
author: alkohli
manager: timlt
editor: 
ms.assetid: e3cdcd9e-33b1-424d-82aa-b369d934067e
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 02/27/2017
ms.author: alkohli
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: c926f0c80ce56cac3106ad97ec3ec2e18a8e2cc6
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="back-up-shares-or-volumes-on-your-storsimple-virtual-array"></a><span data-ttu-id="4887d-103">Zálohování sdílené složky nebo svazky na pole virtuální zařízení StorSimple</span><span class="sxs-lookup"><span data-stu-id="4887d-103">Back up shares or volumes on your StorSimple Virtual Array</span></span>

## <a name="overview"></a><span data-ttu-id="4887d-104">Přehled</span><span class="sxs-lookup"><span data-stu-id="4887d-104">Overview</span></span>

<span data-ttu-id="4887d-105">Pole virtuální zařízení StorSimple je hybridní cloudové úložiště místní virtuální zařízení, které se dají konfigurovat jako souborový server nebo server se službou iSCSI.</span><span class="sxs-lookup"><span data-stu-id="4887d-105">The StorSimple Virtual Array is a hybrid cloud storage on-premises virtual device that can be configured as a file server or an iSCSI server.</span></span> <span data-ttu-id="4887d-106">Pole virtuální umožňuje uživatelům vytvářet plánované a ruční zálohy všech sdílených složek nebo svazků v zařízení.</span><span class="sxs-lookup"><span data-stu-id="4887d-106">The virtual array allows the user to create scheduled and manual backups of all the shares or volumes on the device.</span></span> <span data-ttu-id="4887d-107">Když nakonfigurovaný jako souborový server, také umožňuje obnovení na úrovni položek.</span><span class="sxs-lookup"><span data-stu-id="4887d-107">When configured as a file server, it also allows item-level recovery.</span></span> <span data-ttu-id="4887d-108">Tento kurz popisuje, jak vytvářet zálohy plánované a ruční a proveďte obnovení na úrovni položek pro obnovení odstraněného souboru na vaše virtuální pole.</span><span class="sxs-lookup"><span data-stu-id="4887d-108">This tutorial describes how to create scheduled and manual backups and perform item-level recovery to restore a deleted file on your virtual array.</span></span>

<span data-ttu-id="4887d-109">V tomto kurzu platí pro StorSimple virtuální pole pouze.</span><span class="sxs-lookup"><span data-stu-id="4887d-109">This tutorial applies to the StorSimple Virtual Arrays only.</span></span> <span data-ttu-id="4887d-110">Informace o řady 8000, přejděte na [vytvoření zálohy pro řady 8000 zařízení](storsimple-manage-backup-policies-u2.md)</span><span class="sxs-lookup"><span data-stu-id="4887d-110">For information on 8000 series, go to [Create a backup for 8000 series device](storsimple-manage-backup-policies-u2.md)</span></span>

## <a name="back-up-shares-and-volumes"></a><span data-ttu-id="4887d-111">Zálohování sdílených složek a svazků</span><span class="sxs-lookup"><span data-stu-id="4887d-111">Back up shares and volumes</span></span>

<span data-ttu-id="4887d-112">Zálohování poskytuje ochranu v okamžiku, zvyšují možnost a minimalizovat dobu obnovení sdílené složky a svazky.</span><span class="sxs-lookup"><span data-stu-id="4887d-112">Backups provide point-in-time protection, improve recoverability, and minimize restore times for shares and volumes.</span></span> <span data-ttu-id="4887d-113">Můžete zálohovat na sdílené složky nebo svazku zařízení StorSimple dvěma způsoby: **naplánovaná** nebo **ruční**.</span><span class="sxs-lookup"><span data-stu-id="4887d-113">You can back up a share or volume on your StorSimple device in two ways: **Scheduled** or **Manual**.</span></span> <span data-ttu-id="4887d-114">Všechny metody popsané v následujících částech.</span><span class="sxs-lookup"><span data-stu-id="4887d-114">Each of the methods is discussed in the following sections.</span></span>

## <a name="change-the-backup-start-time"></a><span data-ttu-id="4887d-115">Změňte čas spuštění zálohování</span><span class="sxs-lookup"><span data-stu-id="4887d-115">Change the backup start time</span></span>

> [!NOTE]
> <span data-ttu-id="4887d-116">V této verzi jsou naplánované zálohy vytvořené pomocí výchozí zásadu, která spouští každý den v určitou dobu a zálohuje všechny sdílené složky nebo svazky na zařízení.</span><span class="sxs-lookup"><span data-stu-id="4887d-116">In this release, scheduled backups are created by a default policy that runs daily at a specified time and backs up all the shares or volumes on the device.</span></span> <span data-ttu-id="4887d-117">Není možné vytvořit vlastní zásady pro naplánovaných záloh v tuto chvíli.</span><span class="sxs-lookup"><span data-stu-id="4887d-117">It is not possible to create custom policies for scheduled backups at this time.</span></span>


<span data-ttu-id="4887d-118">Pole virtuální zařízení StorSimple má výchozí zásady zálohování, který začíná v zadané době den (22:30) a zálohuje všechny sdílené složky nebo svazky na zařízení jednou denně.</span><span class="sxs-lookup"><span data-stu-id="4887d-118">Your StorSimple Virtual Array has a default backup policy that starts at a specified time of day (22:30) and backs up all the shares or volumes on the device once a day.</span></span> <span data-ttu-id="4887d-119">Můžete změnit čas, kdy zálohování se spustí, ale četnost a uchovávání dat (který určuje počet záloh chcete zachovat) nelze změnit.</span><span class="sxs-lookup"><span data-stu-id="4887d-119">You can change the time at which the backup starts, but the frequency and the retention (which specifies the number of backups to retain) cannot be changed.</span></span> <span data-ttu-id="4887d-120">Během tyto zálohy je zálohovat celé virtuální zařízení.</span><span class="sxs-lookup"><span data-stu-id="4887d-120">During these backups, the entire virtual device is backed up.</span></span> <span data-ttu-id="4887d-121">To může potenciálně ovlivnit výkon zařízení a ovlivnit úlohy nasazené na zařízení.</span><span class="sxs-lookup"><span data-stu-id="4887d-121">This could potentially impact the performance of the device and affect the workloads deployed on the device.</span></span> <span data-ttu-id="4887d-122">Proto doporučujeme, abyste naplánovali tyto zálohy pro hodiny mimo špičku.</span><span class="sxs-lookup"><span data-stu-id="4887d-122">Therefore, we recommend that you schedule these backups for off-peak hours.</span></span>

 <span data-ttu-id="4887d-123">Chcete-li změnit výchozí zálohování čas, proveďte následující kroky v [portál Azure](https://portal.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="4887d-123">To change the default backup start time, perform the following steps in the [Azure portal](https://portal.azure.com/).</span></span>

#### <a name="to-change-the-start-time-for-the-default-backup-policy"></a><span data-ttu-id="4887d-124">Chcete-li změnit čas zahájení pro výchozí zásady zálohování</span><span class="sxs-lookup"><span data-stu-id="4887d-124">To change the start time for the default backup policy</span></span>

1. <span data-ttu-id="4887d-125">Přejděte na **zařízení**.</span><span class="sxs-lookup"><span data-stu-id="4887d-125">Go to **Devices**.</span></span> <span data-ttu-id="4887d-126">Zobrazí seznam zařízení zaregistrovaná s služby StorSimple Manager zařízení.</span><span class="sxs-lookup"><span data-stu-id="4887d-126">The list of devices registered with your StorSimple Device Manager service will be displayed.</span></span> 
   
    ![přejděte na zařízení](./media/storsimple-virtual-array-backup/changebuschedule1.png)

2. <span data-ttu-id="4887d-128">Vyberte a klikněte na zařízení.</span><span class="sxs-lookup"><span data-stu-id="4887d-128">Select and click your device.</span></span> <span data-ttu-id="4887d-129">**Nastavení** zobrazí se okno.</span><span class="sxs-lookup"><span data-stu-id="4887d-129">The **Settings** blade will be displayed.</span></span> <span data-ttu-id="4887d-130">Přejděte na **spravovat > zásady zálohování**.</span><span class="sxs-lookup"><span data-stu-id="4887d-130">Go to **Manage > Backup policies**.</span></span>
   
    ![Vyberte příslušné zařízení.](./media/storsimple-virtual-array-backup/changebuschedule2.png)

3. <span data-ttu-id="4887d-132">V **zásady zálohování** okně výchozí čas spuštění je 22:30.</span><span class="sxs-lookup"><span data-stu-id="4887d-132">In the **Backup policies** blade, the default start time is 22:30.</span></span> <span data-ttu-id="4887d-133">Nové počáteční čas pro denní plán můžete zadat v zařízení časovém pásmu.</span><span class="sxs-lookup"><span data-stu-id="4887d-133">You can specify the new start time for the daily schedule in device time zone.</span></span>
   
    ![Přejděte do zásady zálohování](./media/storsimple-virtual-array-backup/changebuschedule5.png)

4. <span data-ttu-id="4887d-135">Klikněte na **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="4887d-135">Click **Save**.</span></span>

### <a name="take-a-manual-backup"></a><span data-ttu-id="4887d-136">Proveďte ruční zálohy</span><span class="sxs-lookup"><span data-stu-id="4887d-136">Take a manual backup</span></span>

<span data-ttu-id="4887d-137">Kromě plánovaných záloh můžete provést ruční (na vyžádání), které jsou zálohování dat zařízení kdykoli.</span><span class="sxs-lookup"><span data-stu-id="4887d-137">In addition to scheduled backups, you can take a manual (on-demand) backup of device data at any time.</span></span>

#### <a name="to-create-a-manual-backup"></a><span data-ttu-id="4887d-138">Ruční vytvoření zálohy</span><span class="sxs-lookup"><span data-stu-id="4887d-138">To create a manual backup</span></span>

1. <span data-ttu-id="4887d-139">Přejděte na **zařízení**.</span><span class="sxs-lookup"><span data-stu-id="4887d-139">Go to **Devices**.</span></span> <span data-ttu-id="4887d-140">Vyberte zařízení a klikněte pravým tlačítkem na **...**  na pravém v vybraný řádek.</span><span class="sxs-lookup"><span data-stu-id="4887d-140">Select your device and right-click **...** at the far right in the selected row.</span></span> <span data-ttu-id="4887d-141">V místní nabídce vyberte **provést zálohování**.</span><span class="sxs-lookup"><span data-stu-id="4887d-141">From the context menu, select **Take backup**.</span></span>
   
    ![Přejděte do provést zálohování](./media/storsimple-virtual-array-backup/takebackup1m.png)

2. <span data-ttu-id="4887d-143">V **provést zálohování** okně klikněte na tlačítko **provést zálohování**.</span><span class="sxs-lookup"><span data-stu-id="4887d-143">In the **Take backup** blade, click **Take backup**.</span></span> <span data-ttu-id="4887d-144">To bude zálohovat všechny sdílené složky na souborovém serveru nebo všechny svazky na vašem serveru iSCSI.</span><span class="sxs-lookup"><span data-stu-id="4887d-144">This will backup all the shares on the file server or all the volumes on your iSCSI server.</span></span> 
   
    ![spuštění zálohování](./media/storsimple-virtual-array-backup/takebackup2m.png)
   
    <span data-ttu-id="4887d-146">Spustí zálohu na vyžádání a uvidíte, že byla spuštěna úloha zálohování.</span><span class="sxs-lookup"><span data-stu-id="4887d-146">An on-demand backup starts and you see that a backup job has started.</span></span>
   
    ![spuštění zálohování](./media/storsimple-virtual-array-backup/takebackup3m.png) 
   
    <span data-ttu-id="4887d-148">Když úloha úspěšně dokončí, upozornění se zobrazí znovu.</span><span class="sxs-lookup"><span data-stu-id="4887d-148">Once the job has successfully completed, you are notified again.</span></span> <span data-ttu-id="4887d-149">Pak spustí proces zálohování.</span><span class="sxs-lookup"><span data-stu-id="4887d-149">The backup process then starts.</span></span>
   
    ![Vytvořit úlohu zálohování](./media/storsimple-virtual-array-backup/takebackup4m.png)

3. <span data-ttu-id="4887d-151">Chcete-li sledovat průběh zálohování a podívejte se na podrobnosti úlohy, kliknutím na oznámení.</span><span class="sxs-lookup"><span data-stu-id="4887d-151">To track the progress of the backups and look at the job details, click the notification.</span></span> <span data-ttu-id="4887d-152">Tím přejdete **podrobnosti úlohy**.</span><span class="sxs-lookup"><span data-stu-id="4887d-152">This takes you to  **Job details**.</span></span>
   
     ![Podrobnosti úlohy zálohování.](./media/storsimple-virtual-array-backup/takebackup5m.png)

4. <span data-ttu-id="4887d-154">Po dokončení zálohování přejděte na **správy > Katalog zálohování**.</span><span class="sxs-lookup"><span data-stu-id="4887d-154">After the backup is complete, go to **Management > Backup catalog**.</span></span> <span data-ttu-id="4887d-155">Zobrazí se cloudový snímek všechny sdílené složky (nebo svazky) ve vašem zařízení.</span><span class="sxs-lookup"><span data-stu-id="4887d-155">You will see a cloud snapshot of all the shares (or volumes) on your device.</span></span>
   
    ![Dokončené zálohování](./media/storsimple-virtual-array-backup/takebackup19m.png) 

## <a name="view-existing-backups"></a><span data-ttu-id="4887d-157">Zobrazit existující zálohy</span><span class="sxs-lookup"><span data-stu-id="4887d-157">View existing backups</span></span>
<span data-ttu-id="4887d-158">Chcete-li zobrazit existující zálohy, proveďte následující kroky na portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="4887d-158">To view the existing backups, perform the following steps in the Azure portal.</span></span>

#### <a name="to-view-existing-backups"></a><span data-ttu-id="4887d-159">Chcete-li zobrazit existující zálohy</span><span class="sxs-lookup"><span data-stu-id="4887d-159">To view existing backups</span></span>

1. <span data-ttu-id="4887d-160">Přejděte na **zařízení** okno.</span><span class="sxs-lookup"><span data-stu-id="4887d-160">Go to **Devices** blade.</span></span> <span data-ttu-id="4887d-161">Vyberte a klikněte na zařízení.</span><span class="sxs-lookup"><span data-stu-id="4887d-161">Select and click your device.</span></span> <span data-ttu-id="4887d-162">V **nastavení** okno, přejděte na **správy > Katalog zálohování**.</span><span class="sxs-lookup"><span data-stu-id="4887d-162">In the **Settings** blade, go to **Management > Backup Catalog**.</span></span>
   
    ![Přejděte do katalogu zálohy](./media/storsimple-virtual-array-backup/viewbackups1.png)
2. <span data-ttu-id="4887d-164">Zadejte následující kritéria, která má být použit pro filtrování:</span><span class="sxs-lookup"><span data-stu-id="4887d-164">Specify the following criteria to be used for filtering:</span></span>
   
    - <span data-ttu-id="4887d-165">**Čas rozsah** – může být **poslední 1 hodinu**, **za posledních 24 hodin**, **za posledních 7 dní**, **za posledních 30 dnů**, **za poslední rok**, a **vlastní datum**.</span><span class="sxs-lookup"><span data-stu-id="4887d-165">**Time range** – can be **Past 1 hour**, **Past 24 hours**, **Past 7 days**, **Past 30 days**, **Past year**, and **Custom date**.</span></span>
    
    - <span data-ttu-id="4887d-166">**Zařízení** – vyberte ze seznamu souborových serverů nebo serverů iSCSI zaregistrována služby StorSimple Manager zařízení.</span><span class="sxs-lookup"><span data-stu-id="4887d-166">**Devices** – select from the list of file servers or iSCSI servers registered with your StorSimple Device Manager service.</span></span>
   
    - <span data-ttu-id="4887d-167">**Iniciované** – lze automaticky **naplánovaná** (podle zásady zálohování) nebo **ručně** iniciovaná (můžete).</span><span class="sxs-lookup"><span data-stu-id="4887d-167">**Initiated** – can be automatically **Scheduled** (by a backup policy) or **Manually** initiated (by you).</span></span>
   
    ![Filtr zálohy](./media/storsimple-virtual-array-backup/viewbackups2.png)

3. <span data-ttu-id="4887d-169">Klikněte na tlačítko **Použít**.</span><span class="sxs-lookup"><span data-stu-id="4887d-169">Click **Apply**.</span></span> <span data-ttu-id="4887d-170">Filtrovaný seznam zálohy se zobrazí v **katalog zálohování** okno.</span><span class="sxs-lookup"><span data-stu-id="4887d-170">The filtered list of backups is displayed in the **Backup catalog** blade.</span></span> <span data-ttu-id="4887d-171">Poznámka: pouze 100 zálohování elementy lze zobrazit v daném okamžiku.</span><span class="sxs-lookup"><span data-stu-id="4887d-171">Note only 100 backup elements can be displayed at a given time.</span></span>
   
    ![Aktualizované zálohování katalogu](./media/storsimple-virtual-array-backup/viewbackups3.png)

## <a name="next-steps"></a><span data-ttu-id="4887d-173">Další kroky</span><span class="sxs-lookup"><span data-stu-id="4887d-173">Next steps</span></span>

<span data-ttu-id="4887d-174">Další informace o [Správa pole virtuální zařízení StorSimple](storsimple-ova-web-ui-admin.md).</span><span class="sxs-lookup"><span data-stu-id="4887d-174">Learn more about [administering your StorSimple Virtual Array](storsimple-ova-web-ui-admin.md).</span></span>

