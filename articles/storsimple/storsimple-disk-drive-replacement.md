---
title: "Nahraďte diskovou jednotku na zařízení StorSimple | Microsoft Docs"
description: "Vysvětluje, jak nahradit diskovou jednotku na skříni primární zařízení StorSimple nebo EBOD skříň."
services: storsimple
documentationcenter: 
author: alkohli
manager: carmonm
editor: 
ms.assetid: 98890d36-b613-40fd-994e-330dd907a8a1
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 08/17/2016
ms.author: alkohli
ms.openlocfilehash: 0659ab9d304dbfcce72e8c3c79edad68e70b9630
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="replace-a-disk-drive-on-your-storsimple-device"></a><span data-ttu-id="5d39c-103">Místo disku v zařízení StorSimple</span><span class="sxs-lookup"><span data-stu-id="5d39c-103">Replace a disk drive on your StorSimple device</span></span>
## <a name="overview"></a><span data-ttu-id="5d39c-104">Přehled</span><span class="sxs-lookup"><span data-stu-id="5d39c-104">Overview</span></span>
<span data-ttu-id="5d39c-105">V tomto kurzu vysvětluje, jak můžete odeberete a nahradíte nefunkční nebo selhání jednotky pevného disku na zařízení s Microsoft Azure StorSimple.</span><span class="sxs-lookup"><span data-stu-id="5d39c-105">This tutorial explains how you can remove and replace a malfunctioning or failed hard disk drive on a Microsoft Azure StorSimple device.</span></span> <span data-ttu-id="5d39c-106">Chcete-li nahradit diskovou jednotku, je potřeba:</span><span class="sxs-lookup"><span data-stu-id="5d39c-106">To replace a disk drive, you need to:</span></span>

* <span data-ttu-id="5d39c-107">Musí se vypnout antitamper zámek</span><span class="sxs-lookup"><span data-stu-id="5d39c-107">Disengage the antitamper lock</span></span>
* <span data-ttu-id="5d39c-108">Odebrat z disku</span><span class="sxs-lookup"><span data-stu-id="5d39c-108">Remove the disk drive</span></span>
* <span data-ttu-id="5d39c-109">Nainstalujte na disk nahrazení</span><span class="sxs-lookup"><span data-stu-id="5d39c-109">Install the replacement disk drive</span></span>

> [!IMPORTANT]
> <span data-ttu-id="5d39c-110">Před odebírání a nahrazování diskovou jednotku, zkontrolujte informace zabezpečení v [StorSimple hardwarové součásti nahrazení](storsimple-hardware-component-replacement.md).</span><span class="sxs-lookup"><span data-stu-id="5d39c-110">Before removing and replacing a disk drive, review the safety information in [StorSimple hardware component replacement](storsimple-hardware-component-replacement.md).</span></span>
> 
> 

## <a name="disengage-the-antitamper-lock"></a><span data-ttu-id="5d39c-111">Musí se vypnout antitamper zámek</span><span class="sxs-lookup"><span data-stu-id="5d39c-111">Disengage the antitamper lock</span></span>
<span data-ttu-id="5d39c-112">Tento postup vysvětluje, jak můžete antitamper zámky v zařízení StorSimple pověření nebo odpojí po nahrazení diskové jednotky.</span><span class="sxs-lookup"><span data-stu-id="5d39c-112">This procedure explains how the antitamper locks on your StorSimple device can be engaged or disengaged when you replace the disk drives.</span></span> <span data-ttu-id="5d39c-113">Antitamper zámky jsou umístěné v obslužných rutin nosného jednotky, a jsou přístupná prostřednictvím malou aperturou v části západky popisovač.</span><span class="sxs-lookup"><span data-stu-id="5d39c-113">The antitamper locks are fitted in the drive carrier handles, and they are accessed through a small aperture in the latch section of the handle.</span></span> <span data-ttu-id="5d39c-114">Disky se dodává s zámky nastavena na poloze.</span><span class="sxs-lookup"><span data-stu-id="5d39c-114">Drives are supplied with the locks set to the locked position.</span></span>

#### <a name="to-unlock-the-antitamper-lock"></a><span data-ttu-id="5d39c-115">K odemknutí antitamper zámek</span><span class="sxs-lookup"><span data-stu-id="5d39c-115">To unlock the antitamper lock</span></span>
1. <span data-ttu-id="5d39c-116">Pečlivě vložení klávesy lock ("tamperproof" T10 šroubovák, které poskytuje Microsoft) do otvoru ve popisovač a do jeho soketu.</span><span class="sxs-lookup"><span data-stu-id="5d39c-116">Carefully insert the lock key (a "tamperproof" T10 screwdriver that Microsoft provided) into the aperture in the handle and into its socket.</span></span> 
   
   > [!NOTE]
   > <span data-ttu-id="5d39c-117">Pokud je aktivovaná antitamper zámek, je zobrazen v otvoru red indikátoru.</span><span class="sxs-lookup"><span data-stu-id="5d39c-117">If the antitamper lock is activated, the red indicator is visible in the aperture.</span></span>
   > 
   > 
   
    ![Uzamčení diskovou jednotku](./media/storsimple-disk-drive-replacement/IC741056.png)
   
    <span data-ttu-id="5d39c-119">**Obrázek 1** proti zfalšování zámku pověření</span><span class="sxs-lookup"><span data-stu-id="5d39c-119">**Figure 1** Anti-tamper lock engaged</span></span>
   
   | <span data-ttu-id="5d39c-120">Štítek</span><span class="sxs-lookup"><span data-stu-id="5d39c-120">Label</span></span> | <span data-ttu-id="5d39c-121">Popis</span><span class="sxs-lookup"><span data-stu-id="5d39c-121">Description</span></span> |
   |:--- |:--- |
   | <span data-ttu-id="5d39c-122">1</span><span class="sxs-lookup"><span data-stu-id="5d39c-122">1</span></span> |<span data-ttu-id="5d39c-123">Otvoru indikátoru</span><span class="sxs-lookup"><span data-stu-id="5d39c-123">Indicator aperture</span></span> |
   | <span data-ttu-id="5d39c-124">2</span><span class="sxs-lookup"><span data-stu-id="5d39c-124">2</span></span> |<span data-ttu-id="5d39c-125">Antitamper zámku</span><span class="sxs-lookup"><span data-stu-id="5d39c-125">Antitamper lock</span></span> |
2. <span data-ttu-id="5d39c-126">Otočte klíče v proti směru hodinových ručiček směr, dokud red indikátoru není zobrazená v otvoru nad klíčem.</span><span class="sxs-lookup"><span data-stu-id="5d39c-126">Rotate the key in an anticlockwise direction until the red indicator is not visible in the aperture above the key.</span></span>
3. <span data-ttu-id="5d39c-127">Klíč odeberte.</span><span class="sxs-lookup"><span data-stu-id="5d39c-127">Remove the key.</span></span>
   
    ![Odemknout diskovou jednotku](./media/storsimple-disk-drive-replacement/IC741057.png)
   
    <span data-ttu-id="5d39c-129">**Obrázek 2** Odemknutý diskovou jednotku</span><span class="sxs-lookup"><span data-stu-id="5d39c-129">**Figure 2** Unlocked disk drive</span></span>
4. <span data-ttu-id="5d39c-130">Nyní nelze odebrat z disku.</span><span class="sxs-lookup"><span data-stu-id="5d39c-130">The disk drive can now be removed.</span></span>

<span data-ttu-id="5d39c-131">Postupujte podle kroků v zpětného provozovat zámek.</span><span class="sxs-lookup"><span data-stu-id="5d39c-131">Follow the steps in reverse to engage the lock.</span></span>

## <a name="remove-the-disk-drive"></a><span data-ttu-id="5d39c-132">Odebrat z disku</span><span class="sxs-lookup"><span data-stu-id="5d39c-132">Remove the disk drive</span></span>
<span data-ttu-id="5d39c-133">Zařízení StorSimple podporuje konfiguraci RAID 10 jako úložiště prostorů.</span><span class="sxs-lookup"><span data-stu-id="5d39c-133">Your StorSimple device supports a RAID 10-like storage spaces configuration.</span></span> <span data-ttu-id="5d39c-134">To znamená, že může fungovat normálně u jeden selhání disk SSD jednotky SSD (Solid-State Drive), nebo jednotku pevného disku (HDD).</span><span class="sxs-lookup"><span data-stu-id="5d39c-134">This implies that it can operate normally with one failed disk, solid-state drive (SSD), or hard disk drive (HDD).</span></span> 

> [!IMPORTANT]
> * <span data-ttu-id="5d39c-135">Pokud má váš systém více než jednoho disku, který selhal, neodebírejte víc než jeden SSD nebo HDD ze systému v libovolném bodě v čase.</span><span class="sxs-lookup"><span data-stu-id="5d39c-135">If your system has more than one failed disk, do not remove more than one SSD or HDD from the system at any point in time.</span></span> <span data-ttu-id="5d39c-136">Díky tomu může dojít ke ztrátě dat.</span><span class="sxs-lookup"><span data-stu-id="5d39c-136">Doing so could result in loss of data.</span></span>
> * <span data-ttu-id="5d39c-137">Zajistěte, aby umístíte náhradní SSD ve slotu, která dříve obsahovala SSD.</span><span class="sxs-lookup"><span data-stu-id="5d39c-137">Make sure that you place a replacement SSD in a slot that previously contained an SSD.</span></span> <span data-ttu-id="5d39c-138">Podobně umístěte náhradní pevný disk ve slotu, která dříve obsahovala pevný disk.</span><span class="sxs-lookup"><span data-stu-id="5d39c-138">Similarly, place a replacement HDD in a slot that previously contained an HDD.</span></span>
> * <span data-ttu-id="5d39c-139">Na portálu Azure classic jsou sloty číslo v rozsahu 0 – 11.</span><span class="sxs-lookup"><span data-stu-id="5d39c-139">In the Azure classic portal, slots are numbered from 0 – 11.</span></span> <span data-ttu-id="5d39c-140">Proto pokud portálu ukazuje, že na pozici 2 selhání disku, na zařízení, Hledat selhání disku ve třetí slotu z nahoře vlevo.</span><span class="sxs-lookup"><span data-stu-id="5d39c-140">Therefore, if the portal shows that a disk in slot 2 has failed, on the device, look for the failed disk in the third slot from the top left.</span></span>
> 
> 

<span data-ttu-id="5d39c-141">Jednotky můžete odebrat a nahradit, když je operační systém.</span><span class="sxs-lookup"><span data-stu-id="5d39c-141">Drives can be removed and replaced while the system is operating.</span></span>

#### <a name="to-remove-a-drive"></a><span data-ttu-id="5d39c-142">Chcete-li odebrat jednotku</span><span class="sxs-lookup"><span data-stu-id="5d39c-142">To remove a drive</span></span>
1. <span data-ttu-id="5d39c-143">Chcete-li identifikovat selhání disku, na portálu Azure classic, přejděte na **zařízení** > **údržby** > **stavu hardwaru**.</span><span class="sxs-lookup"><span data-stu-id="5d39c-143">To identify the failed disk, in the Azure classic portal, go to **Devices** > **Maintenance** > **Hardware Status**.</span></span> <span data-ttu-id="5d39c-144">Vzhledem k tomu, že disk může selhat v primární skříň nebo EBOD skříň (Pokud používáte 8600 model), podívejte se na stav disků v části **sdílené součásti** a v části **EBOD skříň sdílené součásti**.</span><span class="sxs-lookup"><span data-stu-id="5d39c-144">Because a disk can fail in the primary enclosure and/or in an EBOD enclosure (if you are using a 8600 model), look at the status of the disks under **Shared Components** and under **EBOD enclosure Shared Components**.</span></span> <span data-ttu-id="5d39c-145">Poškozený disk v buď skříň zobrazí červený stav.</span><span class="sxs-lookup"><span data-stu-id="5d39c-145">A failed disk in either enclosure will be shown with a red status.</span></span>
2. <span data-ttu-id="5d39c-146">Vyhledejte jednotky ve skříni primární nebo EBOD skříň.</span><span class="sxs-lookup"><span data-stu-id="5d39c-146">Locate the drives in the front of the primary enclosure or the EBOD enclosure.</span></span> 
3. <span data-ttu-id="5d39c-147">Pokud je disk odemknout, přejděte k dalšímu kroku.</span><span class="sxs-lookup"><span data-stu-id="5d39c-147">If the disk is unlocked, proceed to the next step.</span></span> <span data-ttu-id="5d39c-148">Pokud je na disku, odemknout pomocí postupu v [musí vypnout antitamper Zámek](#disengage-the-antitamper-lock).</span><span class="sxs-lookup"><span data-stu-id="5d39c-148">If the disk is locked, unlock it by following the procedure in [Disengage the antitamper lock](#disengage-the-antitamper-lock).</span></span>
4. <span data-ttu-id="5d39c-149">Stiskněte klávesu černým západky v modulu poskytovatel jednotky a načítat popisovač poskytovatel jednotky odhlašování a rychle z před skříň.</span><span class="sxs-lookup"><span data-stu-id="5d39c-149">Press the black latch on the drive carrier module and pull the drive carrier handle out and away from the front of the chassis.</span></span> 
   
    ![Uvolnění popisovač diskovou jednotku](./media/storsimple-disk-drive-replacement/IC741051.png)
   
    <span data-ttu-id="5d39c-151">**Obrázek 3** uvolnění popisovač jednotky</span><span class="sxs-lookup"><span data-stu-id="5d39c-151">**Figure 3** Releasing the drive handle</span></span>
5. <span data-ttu-id="5d39c-152">Když je poskytovatel popisovač jednotky plně rozšířeno, posuňte poskytovatel jednotky mimo skříň.</span><span class="sxs-lookup"><span data-stu-id="5d39c-152">When the drive carrier handle is fully extended, slide the drive carrier out of the chassis.</span></span> 
   
    ![Klouzavé disku z disku](./media/storsimple-disk-drive-replacement/IC741052.png)
   
    <span data-ttu-id="5d39c-154">**Obrázek 4** klouzavé diskové jednotce mimo zařadit</span><span class="sxs-lookup"><span data-stu-id="5d39c-154">**Figure 4** Sliding the disk drive out of the carrier</span></span>

## <a name="install-the-replacement-disk-drive"></a><span data-ttu-id="5d39c-155">Nainstalujte na disk nahrazení</span><span class="sxs-lookup"><span data-stu-id="5d39c-155">Install the replacement disk drive</span></span>
<span data-ttu-id="5d39c-156">Jakmile jednotku se nezdařila v zařízení StorSimple a jste ji odstranili, pomocí následujícího postupu nahraďte ji na nový disk.</span><span class="sxs-lookup"><span data-stu-id="5d39c-156">After a drive has failed in your StorSimple device and you have removed it, follow this procedure to replace it with a new drive.</span></span>

#### <a name="to-insert-a-drive"></a><span data-ttu-id="5d39c-157">Chcete-li vložit na jednotku</span><span class="sxs-lookup"><span data-stu-id="5d39c-157">To insert a drive</span></span>
1. <span data-ttu-id="5d39c-158">Ujistěte se, že poskytovatel popisovač jednotky je plně rozšířené, jak je znázorněno na následujícím obrázku.</span><span class="sxs-lookup"><span data-stu-id="5d39c-158">Ensure the drive carrier handle is fully extended, as shown in the following image.</span></span>
   
    ![Diskovou jednotku s popisovačem rozšířené](./media/storsimple-disk-drive-replacement/IC741044.png)
   
    <span data-ttu-id="5d39c-160">**Obrázek 5** jednotku s popisovačem rozšířené</span><span class="sxs-lookup"><span data-stu-id="5d39c-160">**Figure 5** Drive with handle extended</span></span>
2. <span data-ttu-id="5d39c-161">Vysuňte poskytovatel jednotky úplně do skříň.</span><span class="sxs-lookup"><span data-stu-id="5d39c-161">Slide the drive carrier all the way into the chassis.</span></span> 
   
    ![Klouzavé disku do poskytovatel diskovou jednotku](./media/storsimple-disk-drive-replacement/IC741045.png)
   
    <span data-ttu-id="5d39c-163">**Obrázek 6** klouzavé poskytovatel jednotku do skříň.</span><span class="sxs-lookup"><span data-stu-id="5d39c-163">**Figure 6**  Sliding the drive carrier into the chassis</span></span>
3. <span data-ttu-id="5d39c-164">S poskytovatel jednotky, vložit zavřete popisovač poskytovatel jednotky přitom dál tak, aby nabízel poskytovatel jednotku do skříň, dokud popisovač poskytovatel jednotky přichytí k uzamčení umístění.</span><span class="sxs-lookup"><span data-stu-id="5d39c-164">With the drive carrier inserted, close the drive carrier handle while continuing to push the drive carrier into the chassis, until the drive carrier handle snaps into a locked position.</span></span>
4. <span data-ttu-id="5d39c-165">Používá se zámek od společnosti Microsoft (tamperproof Torx šroubovák) zabezpečování popisovače poskytovatel do místní vypnutím šroubovacím Zámek čtvrtletí zapnout po směru hodinových ručiček.</span><span class="sxs-lookup"><span data-stu-id="5d39c-165">Use the lock key that was provided by Microsoft (tamperproof Torx screwdriver) to secure the carrier handle into place by turning the lock screw a quarter turn clockwise.</span></span>
5. <span data-ttu-id="5d39c-166">Ověřte, zda nahrazení byla úspěšná a je tak, že přístup k portálu Azure classic a přejdete na funkční jednotku **údržby** > **stavu hardwaru**.</span><span class="sxs-lookup"><span data-stu-id="5d39c-166">Verify that the replacement was successful and the drive is operational by accessing the Azure classic portal and navigating to **Maintenance** > **Hardware Status**.</span></span> <span data-ttu-id="5d39c-167">V části **sdílené součásti** nebo **EBOD skříň sdílené součásti**, stav jednotky by měla být zeleně, která udává, že je v pořádku.</span><span class="sxs-lookup"><span data-stu-id="5d39c-167">Under **Shared Components** or **EBOD enclosure Shared Components**, the drive status should be green, indicating that it is healthy.</span></span>
   
   > [!NOTE]
   > <span data-ttu-id="5d39c-168">Může trvat několik hodin stav disku, chcete-li zelený po nahrazení.</span><span class="sxs-lookup"><span data-stu-id="5d39c-168">It may take several hours for the disk status to turn green after the replacement.</span></span>
   > 
   > 

## <a name="next-steps"></a><span data-ttu-id="5d39c-169">Další kroky</span><span class="sxs-lookup"><span data-stu-id="5d39c-169">Next steps</span></span>
<span data-ttu-id="5d39c-170">Další informace o [StorSimple hardwarové součásti nahrazení](storsimple-hardware-component-replacement.md).</span><span class="sxs-lookup"><span data-stu-id="5d39c-170">Learn more about [StorSimple hardware component replacement](storsimple-hardware-component-replacement.md).</span></span>

