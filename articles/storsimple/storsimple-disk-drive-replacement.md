---
title: "aaaReplace diskovou jednotku na zařízení StorSimple | Microsoft Docs"
description: "Vysvětluje, jak tooreplace disk jednotka v primární skříni StorSimple nebo EBOD skříň."
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
ms.openlocfilehash: d2c78a6d951b0f00ac42e74a34cf1bc83952a3c8
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="replace-a-disk-drive-on-your-storsimple-device"></a><span data-ttu-id="3933b-103">Místo disku v zařízení StorSimple</span><span class="sxs-lookup"><span data-stu-id="3933b-103">Replace a disk drive on your StorSimple device</span></span>
## <a name="overview"></a><span data-ttu-id="3933b-104">Přehled</span><span class="sxs-lookup"><span data-stu-id="3933b-104">Overview</span></span>
<span data-ttu-id="3933b-105">V tomto kurzu vysvětluje, jak můžete odeberete a nahradíte nefunkční nebo selhání jednotky pevného disku na zařízení s Microsoft Azure StorSimple.</span><span class="sxs-lookup"><span data-stu-id="3933b-105">This tutorial explains how you can remove and replace a malfunctioning or failed hard disk drive on a Microsoft Azure StorSimple device.</span></span> <span data-ttu-id="3933b-106">tooreplace diskové jednotce, budete muset:</span><span class="sxs-lookup"><span data-stu-id="3933b-106">tooreplace a disk drive, you need to:</span></span>

* <span data-ttu-id="3933b-107">Musí se vypnout hello antitamper zámku</span><span class="sxs-lookup"><span data-stu-id="3933b-107">Disengage hello antitamper lock</span></span>
* <span data-ttu-id="3933b-108">Odebrat hello diskovou jednotku</span><span class="sxs-lookup"><span data-stu-id="3933b-108">Remove hello disk drive</span></span>
* <span data-ttu-id="3933b-109">Nainstalujte hello nahrazení diskovou jednotku</span><span class="sxs-lookup"><span data-stu-id="3933b-109">Install hello replacement disk drive</span></span>

> [!IMPORTANT]
> <span data-ttu-id="3933b-110">Před odebírání a nahrazování diskovou jednotku, zkontrolujte informace o zabezpečení hello v [StorSimple hardwarové součásti nahrazení](storsimple-hardware-component-replacement.md).</span><span class="sxs-lookup"><span data-stu-id="3933b-110">Before removing and replacing a disk drive, review hello safety information in [StorSimple hardware component replacement](storsimple-hardware-component-replacement.md).</span></span>
> 
> 

## <a name="disengage-hello-antitamper-lock"></a><span data-ttu-id="3933b-111">Musí se vypnout hello antitamper zámku</span><span class="sxs-lookup"><span data-stu-id="3933b-111">Disengage hello antitamper lock</span></span>
<span data-ttu-id="3933b-112">Tento postup vysvětluje, jak můžete hello antitamper zámky v zařízení StorSimple pověření nebo odpojí po nahrazení hello diskových jednotek.</span><span class="sxs-lookup"><span data-stu-id="3933b-112">This procedure explains how hello antitamper locks on your StorSimple device can be engaged or disengaged when you replace hello disk drives.</span></span> <span data-ttu-id="3933b-113">zámky antitamper Hello jsou umístěné v obslužných rutin nosného hello jednotky, a jsou přístupná prostřednictvím malou aperturou v části západky hello hello popisovač.</span><span class="sxs-lookup"><span data-stu-id="3933b-113">hello antitamper locks are fitted in hello drive carrier handles, and they are accessed through a small aperture in hello latch section of hello handle.</span></span> <span data-ttu-id="3933b-114">Disky se dodává s hello zámky sadu toohello uzamčení pozici.</span><span class="sxs-lookup"><span data-stu-id="3933b-114">Drives are supplied with hello locks set toohello locked position.</span></span>

#### <a name="toounlock-hello-antitamper-lock"></a><span data-ttu-id="3933b-115">toounlock hello antitamper zámku</span><span class="sxs-lookup"><span data-stu-id="3933b-115">toounlock hello antitamper lock</span></span>
1. <span data-ttu-id="3933b-116">Pečlivě vložte hello zámku klíč ("tamperproof" T10 šroubovák, které poskytuje Microsoft) do otvoru hello ve hello popisovač a do jeho soketu.</span><span class="sxs-lookup"><span data-stu-id="3933b-116">Carefully insert hello lock key (a "tamperproof" T10 screwdriver that Microsoft provided) into hello aperture in hello handle and into its socket.</span></span> 
   
   > [!NOTE]
   > <span data-ttu-id="3933b-117">Pokud je aktivovaná antitamper zámku hello, je zobrazen v hello otvoru hello red indikátoru.</span><span class="sxs-lookup"><span data-stu-id="3933b-117">If hello antitamper lock is activated, hello red indicator is visible in hello aperture.</span></span>
   > 
   > 
   
    ![Uzamčení diskovou jednotku](./media/storsimple-disk-drive-replacement/IC741056.png)
   
    <span data-ttu-id="3933b-119">**Obrázek 1** proti zfalšování zámku pověření</span><span class="sxs-lookup"><span data-stu-id="3933b-119">**Figure 1** Anti-tamper lock engaged</span></span>
   
   | <span data-ttu-id="3933b-120">Štítek</span><span class="sxs-lookup"><span data-stu-id="3933b-120">Label</span></span> | <span data-ttu-id="3933b-121">Popis</span><span class="sxs-lookup"><span data-stu-id="3933b-121">Description</span></span> |
   |:--- |:--- |
   | <span data-ttu-id="3933b-122">1</span><span class="sxs-lookup"><span data-stu-id="3933b-122">1</span></span> |<span data-ttu-id="3933b-123">Otvoru indikátoru</span><span class="sxs-lookup"><span data-stu-id="3933b-123">Indicator aperture</span></span> |
   | <span data-ttu-id="3933b-124">2</span><span class="sxs-lookup"><span data-stu-id="3933b-124">2</span></span> |<span data-ttu-id="3933b-125">Antitamper zámku</span><span class="sxs-lookup"><span data-stu-id="3933b-125">Antitamper lock</span></span> |
2. <span data-ttu-id="3933b-126">Otočte hello klíč v proti směru hodinových ručiček směru, dokud indikátor hello red není zobrazená v otvoru hello výše hello klíč.</span><span class="sxs-lookup"><span data-stu-id="3933b-126">Rotate hello key in an anticlockwise direction until hello red indicator is not visible in hello aperture above hello key.</span></span>
3. <span data-ttu-id="3933b-127">Klíč hello odeberte.</span><span class="sxs-lookup"><span data-stu-id="3933b-127">Remove hello key.</span></span>
   
    ![Odemknout diskovou jednotku](./media/storsimple-disk-drive-replacement/IC741057.png)
   
    <span data-ttu-id="3933b-129">**Obrázek 2** Odemknutý diskovou jednotku</span><span class="sxs-lookup"><span data-stu-id="3933b-129">**Figure 2** Unlocked disk drive</span></span>
4. <span data-ttu-id="3933b-130">Hello disku lze nyní odebrat.</span><span class="sxs-lookup"><span data-stu-id="3933b-130">hello disk drive can now be removed.</span></span>

<span data-ttu-id="3933b-131">Postupujte podle kroků hello v zpětné tooengage hello zámku.</span><span class="sxs-lookup"><span data-stu-id="3933b-131">Follow hello steps in reverse tooengage hello lock.</span></span>

## <a name="remove-hello-disk-drive"></a><span data-ttu-id="3933b-132">Odebrat hello diskovou jednotku</span><span class="sxs-lookup"><span data-stu-id="3933b-132">Remove hello disk drive</span></span>
<span data-ttu-id="3933b-133">Zařízení StorSimple podporuje konfiguraci RAID 10 jako úložiště prostorů.</span><span class="sxs-lookup"><span data-stu-id="3933b-133">Your StorSimple device supports a RAID 10-like storage spaces configuration.</span></span> <span data-ttu-id="3933b-134">To znamená, že může fungovat normálně u jeden selhání disk SSD jednotky SSD (Solid-State Drive), nebo jednotku pevného disku (HDD).</span><span class="sxs-lookup"><span data-stu-id="3933b-134">This implies that it can operate normally with one failed disk, solid-state drive (SSD), or hard disk drive (HDD).</span></span> 

> [!IMPORTANT]
> * <span data-ttu-id="3933b-135">Pokud má váš systém více než jednoho disku, který selhal, neodebírejte více než jeden SSD a HDD z hello systému v libovolném bodě v čase.</span><span class="sxs-lookup"><span data-stu-id="3933b-135">If your system has more than one failed disk, do not remove more than one SSD or HDD from hello system at any point in time.</span></span> <span data-ttu-id="3933b-136">Díky tomu může dojít ke ztrátě dat.</span><span class="sxs-lookup"><span data-stu-id="3933b-136">Doing so could result in loss of data.</span></span>
> * <span data-ttu-id="3933b-137">Zajistěte, aby umístíte náhradní SSD ve slotu, která dříve obsahovala SSD.</span><span class="sxs-lookup"><span data-stu-id="3933b-137">Make sure that you place a replacement SSD in a slot that previously contained an SSD.</span></span> <span data-ttu-id="3933b-138">Podobně umístěte náhradní pevný disk ve slotu, která dříve obsahovala pevný disk.</span><span class="sxs-lookup"><span data-stu-id="3933b-138">Similarly, place a replacement HDD in a slot that previously contained an HDD.</span></span>
> * <span data-ttu-id="3933b-139">V hello portál Azure classic, jsou sloty číslo v rozsahu 0 – 11.</span><span class="sxs-lookup"><span data-stu-id="3933b-139">In hello Azure classic portal, slots are numbered from 0 – 11.</span></span> <span data-ttu-id="3933b-140">Proto pokud hello portál ukazuje, že na pozici 2 selhání disku, na zařízení hello hledat hello poškozeném disku ve slotu třetí hello z hello nahoře vlevo.</span><span class="sxs-lookup"><span data-stu-id="3933b-140">Therefore, if hello portal shows that a disk in slot 2 has failed, on hello device, look for hello failed disk in hello third slot from hello top left.</span></span>
> 
> 

<span data-ttu-id="3933b-141">Jednotky můžete odebrat a nahradit, když je operační systém hello.</span><span class="sxs-lookup"><span data-stu-id="3933b-141">Drives can be removed and replaced while hello system is operating.</span></span>

#### <a name="tooremove-a-drive"></a><span data-ttu-id="3933b-142">tooremove na jednotku</span><span class="sxs-lookup"><span data-stu-id="3933b-142">tooremove a drive</span></span>
1. <span data-ttu-id="3933b-143">tooidentify hello selhání disku, v portálu Azure classic hello naleznete příliš**zařízení** > **údržby** > **stavu hardwaru**.</span><span class="sxs-lookup"><span data-stu-id="3933b-143">tooidentify hello failed disk, in hello Azure classic portal, go too**Devices** > **Maintenance** > **Hardware Status**.</span></span> <span data-ttu-id="3933b-144">Vzhledem k tomu, že disk může selhat v primární skříň hello nebo EBOD skříň (Pokud používáte 8600 model), podívejte se na stav hello hello disků v části **sdílené součásti** a v části **EBOD skříň sdílené součásti**.</span><span class="sxs-lookup"><span data-stu-id="3933b-144">Because a disk can fail in hello primary enclosure and/or in an EBOD enclosure (if you are using a 8600 model), look at hello status of hello disks under **Shared Components** and under **EBOD enclosure Shared Components**.</span></span> <span data-ttu-id="3933b-145">Poškozený disk v buď skříň zobrazí červený stav.</span><span class="sxs-lookup"><span data-stu-id="3933b-145">A failed disk in either enclosure will be shown with a red status.</span></span>
2. <span data-ttu-id="3933b-146">Vyhledejte hello jednotky v popředí hello hello primární skříň nebo hello EBOD skříň.</span><span class="sxs-lookup"><span data-stu-id="3933b-146">Locate hello drives in hello front of hello primary enclosure or hello EBOD enclosure.</span></span> 
3. <span data-ttu-id="3933b-147">Pokud je odemčený hello disku, pokračujte dalším krokem toohello.</span><span class="sxs-lookup"><span data-stu-id="3933b-147">If hello disk is unlocked, proceed toohello next step.</span></span> <span data-ttu-id="3933b-148">Pokud je hello disk, odemknout pomocí následujícího postupu hello v [musí vypnout antitamper zámku hello](#disengage-the-antitamper-lock).</span><span class="sxs-lookup"><span data-stu-id="3933b-148">If hello disk is locked, unlock it by following hello procedure in [Disengage hello antitamper lock](#disengage-the-antitamper-lock).</span></span>
4. <span data-ttu-id="3933b-149">Stiskněte klávesu hello černé opatřit na hello jednotky poskytovatel modulu a načítat hello jednotky poskytovatel popisovač odhlašování a rychle z hello přední hello skříní.</span><span class="sxs-lookup"><span data-stu-id="3933b-149">Press hello black latch on hello drive carrier module and pull hello drive carrier handle out and away from hello front of hello chassis.</span></span> 
   
    ![Uvolnění popisovač diskovou jednotku](./media/storsimple-disk-drive-replacement/IC741051.png)
   
    <span data-ttu-id="3933b-151">**Obrázek 3** uvolnění hello jednotky popisovač</span><span class="sxs-lookup"><span data-stu-id="3933b-151">**Figure 3** Releasing hello drive handle</span></span>
5. <span data-ttu-id="3933b-152">Pokud hello jednotky poskytovatel popisovač je plně rozšířen, posuňte hello jednotky poskytovatel mimo hello skříň.</span><span class="sxs-lookup"><span data-stu-id="3933b-152">When hello drive carrier handle is fully extended, slide hello drive carrier out of hello chassis.</span></span> 
   
    ![Klouzavé disku z disku](./media/storsimple-disk-drive-replacement/IC741052.png)
   
    <span data-ttu-id="3933b-154">**Obrázek 4** klouzavé hello disku mimo poskytovatel hello</span><span class="sxs-lookup"><span data-stu-id="3933b-154">**Figure 4** Sliding hello disk drive out of hello carrier</span></span>

## <a name="install-hello-replacement-disk-drive"></a><span data-ttu-id="3933b-155">Nainstalujte hello nahrazení diskovou jednotku</span><span class="sxs-lookup"><span data-stu-id="3933b-155">Install hello replacement disk drive</span></span>
<span data-ttu-id="3933b-156">Po jednotku se nezdařila v zařízení StorSimple a odeberete ji, postupujte podle tohoto postupu tooreplace její na nový disk.</span><span class="sxs-lookup"><span data-stu-id="3933b-156">After a drive has failed in your StorSimple device and you have removed it, follow this procedure tooreplace it with a new drive.</span></span>

#### <a name="tooinsert-a-drive"></a><span data-ttu-id="3933b-157">tooinsert na jednotku</span><span class="sxs-lookup"><span data-stu-id="3933b-157">tooinsert a drive</span></span>
1. <span data-ttu-id="3933b-158">Ujistěte se, že popisovač poskytovatel jednotky hello je plně rozšířené, jak ukazuje následující obrázek hello.</span><span class="sxs-lookup"><span data-stu-id="3933b-158">Ensure hello drive carrier handle is fully extended, as shown in hello following image.</span></span>
   
    ![Diskovou jednotku s popisovačem rozšířené](./media/storsimple-disk-drive-replacement/IC741044.png)
   
    <span data-ttu-id="3933b-160">**Obrázek 5** jednotku s popisovačem rozšířené</span><span class="sxs-lookup"><span data-stu-id="3933b-160">**Figure 5** Drive with handle extended</span></span>
2. <span data-ttu-id="3933b-161">Vysuňte hello jednotky poskytovatel všechny způsob hello do skříně hello.</span><span class="sxs-lookup"><span data-stu-id="3933b-161">Slide hello drive carrier all hello way into hello chassis.</span></span> 
   
    ![Klouzavé disku do poskytovatel diskovou jednotku](./media/storsimple-disk-drive-replacement/IC741045.png)
   
    <span data-ttu-id="3933b-163">**Obrázek 6** posuvné hello jednotky poskytovatel do skříně hello</span><span class="sxs-lookup"><span data-stu-id="3933b-163">**Figure 6**  Sliding hello drive carrier into hello chassis</span></span>
3. <span data-ttu-id="3933b-164">S hello jednotky poskytovatel vložené, Zavřít hello jednotky poskytovatel popisovačem při zpracování pokračuje toopush hello jednotky poskytovatel do hello skříň, dokud hello jednotky poskytovatel popisovač přichytí k uzamčení umístění.</span><span class="sxs-lookup"><span data-stu-id="3933b-164">With hello drive carrier inserted, close hello drive carrier handle while continuing toopush hello drive carrier into hello chassis, until hello drive carrier handle snaps into a locked position.</span></span>
4. <span data-ttu-id="3933b-165">Použití hello zámku klíč, který byl poskytnut Microsoft (tamperproof Torx šroubovák) toosecure hello poskytovatel popisovač do místní vypnutím hello zámku šroubovacím čtvrtletí zapnout po směru hodinových ručiček.</span><span class="sxs-lookup"><span data-stu-id="3933b-165">Use hello lock key that was provided by Microsoft (tamperproof Torx screwdriver) toosecure hello carrier handle into place by turning hello lock screw a quarter turn clockwise.</span></span>
5. <span data-ttu-id="3933b-166">Ověřte, zda hello nahrazení byla úspěšná a hello jednotka je provozní přístup k hello portál Azure classic a navigace příliš**údržby** > **stavu hardwaru**.</span><span class="sxs-lookup"><span data-stu-id="3933b-166">Verify that hello replacement was successful and hello drive is operational by accessing hello Azure classic portal and navigating too**Maintenance** > **Hardware Status**.</span></span> <span data-ttu-id="3933b-167">V části **sdílené součásti** nebo **EBOD skříň sdílené součásti**, musí být ve stavu jednotky hello zeleně, která udává, že je v pořádku.</span><span class="sxs-lookup"><span data-stu-id="3933b-167">Under **Shared Components** or **EBOD enclosure Shared Components**, hello drive status should be green, indicating that it is healthy.</span></span>
   
   > [!NOTE]
   > <span data-ttu-id="3933b-168">Ho může trvat několik hodin pro tooturn stav disku hello zelená po nahrazení hello.</span><span class="sxs-lookup"><span data-stu-id="3933b-168">It may take several hours for hello disk status tooturn green after hello replacement.</span></span>
   > 
   > 

## <a name="next-steps"></a><span data-ttu-id="3933b-169">Další kroky</span><span class="sxs-lookup"><span data-stu-id="3933b-169">Next steps</span></span>
<span data-ttu-id="3933b-170">Další informace o [StorSimple hardwarové součásti nahrazení](storsimple-hardware-component-replacement.md).</span><span class="sxs-lookup"><span data-stu-id="3933b-170">Learn more about [StorSimple hardware component replacement](storsimple-hardware-component-replacement.md).</span></span>

