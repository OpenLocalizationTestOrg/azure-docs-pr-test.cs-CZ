---
title: "kurz zálohování Azure StorSimple virtuální pole aaaMicrosoft | Microsoft Docs"
description: "Popisuje, jak sdílené složky tooback až pole virtuální zařízení StorSimple a svazky."
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
ms.openlocfilehash: 7a015fd594f8f56c48fab149a2736be9dec2c24b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="back-up-shares-or-volumes-on-your-storsimple-virtual-array"></a><span data-ttu-id="24ea6-103">Zálohování sdílené složky nebo svazky na pole virtuální zařízení StorSimple</span><span class="sxs-lookup"><span data-stu-id="24ea6-103">Back up shares or volumes on your StorSimple Virtual Array</span></span>

## <a name="overview"></a><span data-ttu-id="24ea6-104">Přehled</span><span class="sxs-lookup"><span data-stu-id="24ea6-104">Overview</span></span>

<span data-ttu-id="24ea6-105">Hello pole virtuální zařízení StorSimple je hybridní cloudové úložiště místní virtuální zařízení, které se dají konfigurovat jako souborový server nebo server se službou iSCSI.</span><span class="sxs-lookup"><span data-stu-id="24ea6-105">hello StorSimple Virtual Array is a hybrid cloud storage on-premises virtual device that can be configured as a file server or an iSCSI server.</span></span> <span data-ttu-id="24ea6-106">pole virtuálním Hello umožňuje hello uživatele toocreate plánované a ruční zálohování všech hello složek nebo svazků v zařízení hello.</span><span class="sxs-lookup"><span data-stu-id="24ea6-106">hello virtual array allows hello user toocreate scheduled and manual backups of all hello shares or volumes on hello device.</span></span> <span data-ttu-id="24ea6-107">Když nakonfigurovaný jako souborový server, také umožňuje obnovení na úrovni položek.</span><span class="sxs-lookup"><span data-stu-id="24ea6-107">When configured as a file server, it also allows item-level recovery.</span></span> <span data-ttu-id="24ea6-108">Tento kurz popisuje, jak naplánovat toocreate a ručního zálohování a proveďte obnovení na úrovni položek toorestore odstraněnému souboru na vaše virtuální pole.</span><span class="sxs-lookup"><span data-stu-id="24ea6-108">This tutorial describes how toocreate scheduled and manual backups and perform item-level recovery toorestore a deleted file on your virtual array.</span></span>

<span data-ttu-id="24ea6-109">V tomto kurzu platí toohello StorSimple virtuální pole pouze.</span><span class="sxs-lookup"><span data-stu-id="24ea6-109">This tutorial applies toohello StorSimple Virtual Arrays only.</span></span> <span data-ttu-id="24ea6-110">Informace o řady 8000 přejděte příliš[vytvoření zálohy pro řady 8000 zařízení](storsimple-manage-backup-policies-u2.md)</span><span class="sxs-lookup"><span data-stu-id="24ea6-110">For information on 8000 series, go too[Create a backup for 8000 series device](storsimple-manage-backup-policies-u2.md)</span></span>

## <a name="back-up-shares-and-volumes"></a><span data-ttu-id="24ea6-111">Zálohování sdílených složek a svazků</span><span class="sxs-lookup"><span data-stu-id="24ea6-111">Back up shares and volumes</span></span>

<span data-ttu-id="24ea6-112">Zálohování poskytuje ochranu v okamžiku, zvyšují možnost a minimalizovat dobu obnovení sdílené složky a svazky.</span><span class="sxs-lookup"><span data-stu-id="24ea6-112">Backups provide point-in-time protection, improve recoverability, and minimize restore times for shares and volumes.</span></span> <span data-ttu-id="24ea6-113">Můžete zálohovat na sdílené složky nebo svazku zařízení StorSimple dvěma způsoby: **naplánovaná** nebo **ruční**.</span><span class="sxs-lookup"><span data-stu-id="24ea6-113">You can back up a share or volume on your StorSimple device in two ways: **Scheduled** or **Manual**.</span></span> <span data-ttu-id="24ea6-114">Každá z metod hello je popsané v následující části hello.</span><span class="sxs-lookup"><span data-stu-id="24ea6-114">Each of hello methods is discussed in hello following sections.</span></span>

## <a name="change-hello-backup-start-time"></a><span data-ttu-id="24ea6-115">Změňte čas spuštění zálohování hello</span><span class="sxs-lookup"><span data-stu-id="24ea6-115">Change hello backup start time</span></span>

> [!NOTE]
> <span data-ttu-id="24ea6-116">V této verzi jsou naplánované zálohy vytvořené pomocí výchozí zásadu, která spouští každý den v určitou dobu a zálohuje všechny hello sdílené složky nebo svazky na hello zařízení.</span><span class="sxs-lookup"><span data-stu-id="24ea6-116">In this release, scheduled backups are created by a default policy that runs daily at a specified time and backs up all hello shares or volumes on hello device.</span></span> <span data-ttu-id="24ea6-117">Není možné toocreate vlastní zásady pro naplánovaných záloh v tuto chvíli.</span><span class="sxs-lookup"><span data-stu-id="24ea6-117">It is not possible toocreate custom policies for scheduled backups at this time.</span></span>


<span data-ttu-id="24ea6-118">Pole virtuální zařízení StorSimple má výchozí zásady zálohování, který začíná v zadané době den (22:30) a zálohuje všechny hello sdílené složky nebo svazky na zařízení hello jednou denně.</span><span class="sxs-lookup"><span data-stu-id="24ea6-118">Your StorSimple Virtual Array has a default backup policy that starts at a specified time of day (22:30) and backs up all hello shares or volumes on hello device once a day.</span></span> <span data-ttu-id="24ea6-119">Hello dobu, na které hello zálohování se spustí, ale hello četnost a hello uchování (který určuje hello počet záloh tooretain) nelze změnit, můžete změnit.</span><span class="sxs-lookup"><span data-stu-id="24ea6-119">You can change hello time at which hello backup starts, but hello frequency and hello retention (which specifies hello number of backups tooretain) cannot be changed.</span></span> <span data-ttu-id="24ea6-120">Během tyto zálohy je zálohován hello celý virtuální zařízení.</span><span class="sxs-lookup"><span data-stu-id="24ea6-120">During these backups, hello entire virtual device is backed up.</span></span> <span data-ttu-id="24ea6-121">To může potenciálně ovlivnit výkon hello hello zařízení a ovlivnit hello úlohy nasazené na zařízení hello.</span><span class="sxs-lookup"><span data-stu-id="24ea6-121">This could potentially impact hello performance of hello device and affect hello workloads deployed on hello device.</span></span> <span data-ttu-id="24ea6-122">Proto doporučujeme, abyste naplánovali tyto zálohy pro hodiny mimo špičku.</span><span class="sxs-lookup"><span data-stu-id="24ea6-122">Therefore, we recommend that you schedule these backups for off-peak hours.</span></span>

 <span data-ttu-id="24ea6-123">Počáteční čas toochange hello výchozí zálohování, proveďte následující kroky v hello hello [portál Azure](https://portal.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="24ea6-123">toochange hello default backup start time, perform hello following steps in hello [Azure portal](https://portal.azure.com/).</span></span>

#### <a name="toochange-hello-start-time-for-hello-default-backup-policy"></a><span data-ttu-id="24ea6-124">Počáteční čas pro zásady zálohování výchozí hello toochange hello</span><span class="sxs-lookup"><span data-stu-id="24ea6-124">toochange hello start time for hello default backup policy</span></span>

1. <span data-ttu-id="24ea6-125">Přejděte příliš**zařízení**.</span><span class="sxs-lookup"><span data-stu-id="24ea6-125">Go too**Devices**.</span></span> <span data-ttu-id="24ea6-126">Zobrazí se seznam Hello zařízení zaregistrovaná s služby StorSimple Manager zařízení.</span><span class="sxs-lookup"><span data-stu-id="24ea6-126">hello list of devices registered with your StorSimple Device Manager service will be displayed.</span></span> 
   
    ![přejděte toodevices](./media/storsimple-virtual-array-backup/changebuschedule1.png)

2. <span data-ttu-id="24ea6-128">Vyberte a klikněte na zařízení.</span><span class="sxs-lookup"><span data-stu-id="24ea6-128">Select and click your device.</span></span> <span data-ttu-id="24ea6-129">Hello **nastavení** zobrazí se okno.</span><span class="sxs-lookup"><span data-stu-id="24ea6-129">hello **Settings** blade will be displayed.</span></span> <span data-ttu-id="24ea6-130">Přejděte příliš**spravovat > zásady zálohování**.</span><span class="sxs-lookup"><span data-stu-id="24ea6-130">Go too**Manage > Backup policies**.</span></span>
   
    ![Vyberte příslušné zařízení.](./media/storsimple-virtual-array-backup/changebuschedule2.png)

3. <span data-ttu-id="24ea6-132">V hello **zásady zálohování** okně čas spuštění výchozí hello je 22:30.</span><span class="sxs-lookup"><span data-stu-id="24ea6-132">In hello **Backup policies** blade, hello default start time is 22:30.</span></span> <span data-ttu-id="24ea6-133">Hello nový počáteční čas pro denní plán hello můžete zadat v zařízení časovém pásmu.</span><span class="sxs-lookup"><span data-stu-id="24ea6-133">You can specify hello new start time for hello daily schedule in device time zone.</span></span>
   
    ![přejděte toobackup zásady](./media/storsimple-virtual-array-backup/changebuschedule5.png)

4. <span data-ttu-id="24ea6-135">Klikněte na **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="24ea6-135">Click **Save**.</span></span>

### <a name="take-a-manual-backup"></a><span data-ttu-id="24ea6-136">Proveďte ruční zálohy</span><span class="sxs-lookup"><span data-stu-id="24ea6-136">Take a manual backup</span></span>

<span data-ttu-id="24ea6-137">Kromě toho tooscheduled zálohování, můžete využít ručního zálohování (na vyžádání) dat zařízení kdykoli.</span><span class="sxs-lookup"><span data-stu-id="24ea6-137">In addition tooscheduled backups, you can take a manual (on-demand) backup of device data at any time.</span></span>

#### <a name="toocreate-a-manual-backup"></a><span data-ttu-id="24ea6-138">toocreate ruční zálohy</span><span class="sxs-lookup"><span data-stu-id="24ea6-138">toocreate a manual backup</span></span>

1. <span data-ttu-id="24ea6-139">Přejděte příliš**zařízení**.</span><span class="sxs-lookup"><span data-stu-id="24ea6-139">Go too**Devices**.</span></span> <span data-ttu-id="24ea6-140">Vyberte zařízení a klikněte pravým tlačítkem na **...**  v hello úplně vpravo v hello vybraný řádek.</span><span class="sxs-lookup"><span data-stu-id="24ea6-140">Select your device and right-click **...** at hello far right in hello selected row.</span></span> <span data-ttu-id="24ea6-141">Hello místní nabídce vyberte **provést zálohování**.</span><span class="sxs-lookup"><span data-stu-id="24ea6-141">From hello context menu, select **Take backup**.</span></span>
   
    ![přejděte tootake zálohování](./media/storsimple-virtual-array-backup/takebackup1m.png)

2. <span data-ttu-id="24ea6-143">V hello **provést zálohování** okně klikněte na tlačítko **provést zálohování**.</span><span class="sxs-lookup"><span data-stu-id="24ea6-143">In hello **Take backup** blade, click **Take backup**.</span></span> <span data-ttu-id="24ea6-144">To bude zálohovat všechny hello sdílené složky na souborovém serveru hello nebo všechny svazky hello na vašem serveru iSCSI.</span><span class="sxs-lookup"><span data-stu-id="24ea6-144">This will backup all hello shares on hello file server or all hello volumes on your iSCSI server.</span></span> 
   
    ![spuštění zálohování](./media/storsimple-virtual-array-backup/takebackup2m.png)
   
    <span data-ttu-id="24ea6-146">Spustí zálohu na vyžádání a uvidíte, že byla spuštěna úloha zálohování.</span><span class="sxs-lookup"><span data-stu-id="24ea6-146">An on-demand backup starts and you see that a backup job has started.</span></span>
   
    ![spuštění zálohování](./media/storsimple-virtual-array-backup/takebackup3m.png) 
   
    <span data-ttu-id="24ea6-148">Po hello úloha byla úspěšně dokončena, zobrazí se upozornění znovu.</span><span class="sxs-lookup"><span data-stu-id="24ea6-148">Once hello job has successfully completed, you are notified again.</span></span> <span data-ttu-id="24ea6-149">pak spustí proces zálohování Hello.</span><span class="sxs-lookup"><span data-stu-id="24ea6-149">hello backup process then starts.</span></span>
   
    ![Vytvořit úlohu zálohování](./media/storsimple-virtual-array-backup/takebackup4m.png)

3. <span data-ttu-id="24ea6-151">průběh hello tootrack hello zálohy a podívejte se na podrobnosti úlohy hello, klikněte na tlačítko hello oznámení.</span><span class="sxs-lookup"><span data-stu-id="24ea6-151">tootrack hello progress of hello backups and look at hello job details, click hello notification.</span></span> <span data-ttu-id="24ea6-152">Tím přejdete příliš **podrobnosti úlohy**.</span><span class="sxs-lookup"><span data-stu-id="24ea6-152">This takes you too **Job details**.</span></span>
   
     ![Podrobnosti úlohy zálohování.](./media/storsimple-virtual-array-backup/takebackup5m.png)

4. <span data-ttu-id="24ea6-154">Po dokončení zálohování hello přejděte příliš**správy > Katalog zálohování**.</span><span class="sxs-lookup"><span data-stu-id="24ea6-154">After hello backup is complete, go too**Management > Backup catalog**.</span></span> <span data-ttu-id="24ea6-155">Zobrazí se cloudový snímek všech sdílených složek hello (nebo svazky) ve vašem zařízení.</span><span class="sxs-lookup"><span data-stu-id="24ea6-155">You will see a cloud snapshot of all hello shares (or volumes) on your device.</span></span>
   
    ![Dokončené zálohování](./media/storsimple-virtual-array-backup/takebackup19m.png) 

## <a name="view-existing-backups"></a><span data-ttu-id="24ea6-157">Zobrazit existující zálohy</span><span class="sxs-lookup"><span data-stu-id="24ea6-157">View existing backups</span></span>
<span data-ttu-id="24ea6-158">tooview hello existující zálohy, proveďte hello proveďte kroky v hello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="24ea6-158">tooview hello existing backups, perform hello following steps in hello Azure portal.</span></span>

#### <a name="tooview-existing-backups"></a><span data-ttu-id="24ea6-159">tooview existující zálohy</span><span class="sxs-lookup"><span data-stu-id="24ea6-159">tooview existing backups</span></span>

1. <span data-ttu-id="24ea6-160">Přejděte příliš**zařízení** okno.</span><span class="sxs-lookup"><span data-stu-id="24ea6-160">Go too**Devices** blade.</span></span> <span data-ttu-id="24ea6-161">Vyberte a klikněte na zařízení.</span><span class="sxs-lookup"><span data-stu-id="24ea6-161">Select and click your device.</span></span> <span data-ttu-id="24ea6-162">V hello **nastavení** okně přejděte příliš**správy > Katalog zálohování**.</span><span class="sxs-lookup"><span data-stu-id="24ea6-162">In hello **Settings** blade, go too**Management > Backup Catalog**.</span></span>
   
    ![Přejděte toobackup katalogu](./media/storsimple-virtual-array-backup/viewbackups1.png)
2. <span data-ttu-id="24ea6-164">Zadejte hello následující kritéria toobe použitých pro filtrování:</span><span class="sxs-lookup"><span data-stu-id="24ea6-164">Specify hello following criteria toobe used for filtering:</span></span>
   
    - <span data-ttu-id="24ea6-165">**Čas rozsah** – může být **poslední 1 hodinu**, **za posledních 24 hodin**, **za posledních 7 dní**, **za posledních 30 dnů**, **za poslední rok**, a **vlastní datum**.</span><span class="sxs-lookup"><span data-stu-id="24ea6-165">**Time range** – can be **Past 1 hour**, **Past 24 hours**, **Past 7 days**, **Past 30 days**, **Past year**, and **Custom date**.</span></span>
    
    - <span data-ttu-id="24ea6-166">**Zařízení** – vyberte ze seznamu hello souborových serverů nebo serverů iSCSI zaregistrována služby StorSimple Manager zařízení.</span><span class="sxs-lookup"><span data-stu-id="24ea6-166">**Devices** – select from hello list of file servers or iSCSI servers registered with your StorSimple Device Manager service.</span></span>
   
    - <span data-ttu-id="24ea6-167">**Iniciované** – lze automaticky **naplánovaná** (podle zásady zálohování) nebo **ručně** iniciovaná (můžete).</span><span class="sxs-lookup"><span data-stu-id="24ea6-167">**Initiated** – can be automatically **Scheduled** (by a backup policy) or **Manually** initiated (by you).</span></span>
   
    ![Filtr zálohy](./media/storsimple-virtual-array-backup/viewbackups2.png)

3. <span data-ttu-id="24ea6-169">Klikněte na tlačítko **Použít**.</span><span class="sxs-lookup"><span data-stu-id="24ea6-169">Click **Apply**.</span></span> <span data-ttu-id="24ea6-170">Hello filtrovaný seznam zálohy se zobrazí v hello **katalog zálohování** okno.</span><span class="sxs-lookup"><span data-stu-id="24ea6-170">hello filtered list of backups is displayed in hello **Backup catalog** blade.</span></span> <span data-ttu-id="24ea6-171">Poznámka: pouze 100 zálohování elementy lze zobrazit v daném okamžiku.</span><span class="sxs-lookup"><span data-stu-id="24ea6-171">Note only 100 backup elements can be displayed at a given time.</span></span>
   
    ![Aktualizované zálohování katalogu](./media/storsimple-virtual-array-backup/viewbackups3.png)

## <a name="next-steps"></a><span data-ttu-id="24ea6-173">Další kroky</span><span class="sxs-lookup"><span data-stu-id="24ea6-173">Next steps</span></span>

<span data-ttu-id="24ea6-174">Další informace o [Správa pole virtuální zařízení StorSimple](storsimple-ova-web-ui-admin.md).</span><span class="sxs-lookup"><span data-stu-id="24ea6-174">Learn more about [administering your StorSimple Virtual Array](storsimple-ova-web-ui-admin.md).</span></span>

