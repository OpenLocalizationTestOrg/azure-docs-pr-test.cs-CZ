---
title: "Nahraďte baterie na zařízení Microsoft Azure StorSimple | Microsoft Docs"
description: "Popisuje, jak odebrat, nahraďte a udržovat modul zálohování baterie zařízení StorSimple."
services: storsimple
documentationcenter: 
author: alkohli
manager: carmonm
editor: 
ms.assetid: 3c8a6654-4826-4883-aad8-75f332347c53
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 08/17/2016
ms.author: alkohli
ms.openlocfilehash: f8b89b3f6851ec9ee0570f551b5407419fdba2d6
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="replace-the-backup-battery-module-on-your-storsimple-device"></a><span data-ttu-id="decb0-103">Nahraďte modul zálohování baterie zařízení StorSimple</span><span class="sxs-lookup"><span data-stu-id="decb0-103">Replace the backup battery module on your StorSimple device</span></span>
## <a name="overview"></a><span data-ttu-id="decb0-104">Přehled</span><span class="sxs-lookup"><span data-stu-id="decb0-104">Overview</span></span>
<span data-ttu-id="decb0-105">Primární skříň napájení a chlazení modulu (PCM) na zařízení s Microsoft Azure StorSimple má balík další stav baterie.</span><span class="sxs-lookup"><span data-stu-id="decb0-105">The primary enclosure Power and Cooling Module (PCM) on your Microsoft Azure StorSimple device has an additional battery pack.</span></span> <span data-ttu-id="decb0-106">Tento balíček poskytuje power tak, aby zařízení StorSimple můžete uložit data, pokud dojde ke ztrátě napájení ke skříni primární.</span><span class="sxs-lookup"><span data-stu-id="decb0-106">This pack provides power so that the StorSimple device can save data if there is loss of AC power to the primary enclosure.</span></span> <span data-ttu-id="decb0-107">Tato sada pack baterie se označuje jako *zálohování baterie modulu*.</span><span class="sxs-lookup"><span data-stu-id="decb0-107">This battery pack is referred to as the *backup battery module*.</span></span> <span data-ttu-id="decb0-108">Modul zálohování baterie existuje pouze pro primární skříň v zařízení StorSimple (skříni EBOD neobsahuje modul zálohování baterie).</span><span class="sxs-lookup"><span data-stu-id="decb0-108">The backup battery module exists only for the primary enclosure in your StorSimple device (the EBOD enclosure does not contain a backup battery module).</span></span> 

<span data-ttu-id="decb0-109">Tento kurz vysvětluje postup:</span><span class="sxs-lookup"><span data-stu-id="decb0-109">This tutorial explains how to:</span></span>

* <span data-ttu-id="decb0-110">Odebere modul zálohování baterie</span><span class="sxs-lookup"><span data-stu-id="decb0-110">Remove the backup battery module</span></span> 
* <span data-ttu-id="decb0-111">Nainstalujte nový modul zálohování baterie</span><span class="sxs-lookup"><span data-stu-id="decb0-111">Install a new backup battery module</span></span>
* <span data-ttu-id="decb0-112">Udržovat modul zálohování baterie</span><span class="sxs-lookup"><span data-stu-id="decb0-112">Maintain the backup battery module</span></span>

> [!IMPORTANT]
> <span data-ttu-id="decb0-113">Před odebírání a nahrazování modul zálohování baterie zkontrolovat informace o zabezpečení v [Úvod do StorSimple hardwarové součásti nahrazení](storsimple-hardware-component-replacement.md).</span><span class="sxs-lookup"><span data-stu-id="decb0-113">Before removing and replacing a backup battery module, review the safety information in the [Introduction to StorSimple hardware component replacement](storsimple-hardware-component-replacement.md).</span></span>
> 
> 

## <a name="remove-the-backup-battery-module"></a><span data-ttu-id="decb0-114">Odebere modul zálohování baterie</span><span class="sxs-lookup"><span data-stu-id="decb0-114">Remove the backup battery module</span></span>
<span data-ttu-id="decb0-115">Modul zálohování baterie pro zařízení StorSimple je periferní Výměnná jednotka.</span><span class="sxs-lookup"><span data-stu-id="decb0-115">The backup battery module for your StorSimple device is a field-replaceable unit.</span></span> <span data-ttu-id="decb0-116">Před instalací v PCM, modul baterie by měly být uložené v původním balení.</span><span class="sxs-lookup"><span data-stu-id="decb0-116">Before it is installed in the PCM, the battery module should be stored in its original packaging.</span></span> <span data-ttu-id="decb0-117">Proveďte následující kroky k odebrání zálohování baterie.</span><span class="sxs-lookup"><span data-stu-id="decb0-117">Perform the following steps to remove the backup battery.</span></span>

#### <a name="to-remove-the-backup-battery-module"></a><span data-ttu-id="decb0-118">Chcete-li odebrat modul zálohování baterie</span><span class="sxs-lookup"><span data-stu-id="decb0-118">To remove the backup battery module</span></span>
1. <span data-ttu-id="decb0-119">Na portálu Azure classic, přejděte na **zařízení** > **údržby** > **stavu hardwaru**.</span><span class="sxs-lookup"><span data-stu-id="decb0-119">In the Azure classic portal, go to **Devices** > **Maintenance** > **Hardware Status**.</span></span> <span data-ttu-id="decb0-120">V části **sdílené součásti**, podívejte se na stav baterie.</span><span class="sxs-lookup"><span data-stu-id="decb0-120">Under **Shared Components**, look at the status of the battery.</span></span>
2. <span data-ttu-id="decb0-121">Identifikujte PCM, ve kterém se nezdařilo baterie.</span><span class="sxs-lookup"><span data-stu-id="decb0-121">Identify the PCM in which the battery has failed.</span></span> <span data-ttu-id="decb0-122">Obrázek 1 zobrazuje zadní straně zařízení StorSimple.</span><span class="sxs-lookup"><span data-stu-id="decb0-122">Figure 1 shows the back of the StorSimple device.</span></span>
   
    ![Propojovací rozhraní systému modulů skříň primární zařízení](./media/storsimple-battery-replacement/IC740994.png)
   
    <span data-ttu-id="decb0-124">**Obrázek 1** zadní zobrazující modulů PCM a řadiče primární zařízení</span><span class="sxs-lookup"><span data-stu-id="decb0-124">**Figure 1** Back of primary device showing PCM and controller modules</span></span>
   
   | <span data-ttu-id="decb0-125">Štítek</span><span class="sxs-lookup"><span data-stu-id="decb0-125">Label</span></span> | <span data-ttu-id="decb0-126">Popis</span><span class="sxs-lookup"><span data-stu-id="decb0-126">Description</span></span> |
   |:--- |:--- |
   | <span data-ttu-id="decb0-127">1</span><span class="sxs-lookup"><span data-stu-id="decb0-127">1</span></span> |<span data-ttu-id="decb0-128">PCM 0</span><span class="sxs-lookup"><span data-stu-id="decb0-128">PCM 0</span></span> |
   | <span data-ttu-id="decb0-129">2</span><span class="sxs-lookup"><span data-stu-id="decb0-129">2</span></span> |<span data-ttu-id="decb0-130">PCM 1</span><span class="sxs-lookup"><span data-stu-id="decb0-130">PCM 1</span></span> |
   | <span data-ttu-id="decb0-131">3</span><span class="sxs-lookup"><span data-stu-id="decb0-131">3</span></span> |<span data-ttu-id="decb0-132">Řadič 0</span><span class="sxs-lookup"><span data-stu-id="decb0-132">Controller 0</span></span> |
   | <span data-ttu-id="decb0-133">4</span><span class="sxs-lookup"><span data-stu-id="decb0-133">4</span></span> |<span data-ttu-id="decb0-134">Řadič 1</span><span class="sxs-lookup"><span data-stu-id="decb0-134">Controller 1</span></span> |
   
    <span data-ttu-id="decb0-135">Jak je znázorněno číslem 3 na obrázku 2, monitorování indikátoru VEDLA na PCM 0, která odpovídá **baterie selhání** by měl být lit.</span><span class="sxs-lookup"><span data-stu-id="decb0-135">As shown by number 3 in the Figure 2, the monitoring indicator LED on PCM 0 that corresponds to **Battery Fault** should be lit.</span></span>
   
    ![Propojovací rozhraní systému zařízení PCM monitorování kláves](./media/storsimple-battery-replacement/IC740992.png)
   
    <span data-ttu-id="decb0-137">**Obrázek 2** zpět of PCM zobrazující monitorování indikátoru LED</span><span class="sxs-lookup"><span data-stu-id="decb0-137">**Figure 2** Back of PCM showing the monitoring indicator LEDs</span></span>
   
   | <span data-ttu-id="decb0-138">Štítek</span><span class="sxs-lookup"><span data-stu-id="decb0-138">Label</span></span> | <span data-ttu-id="decb0-139">Popis</span><span class="sxs-lookup"><span data-stu-id="decb0-139">Description</span></span> |
   |:--- |:--- |
   | <span data-ttu-id="decb0-140">1</span><span class="sxs-lookup"><span data-stu-id="decb0-140">1</span></span> |<span data-ttu-id="decb0-141">Výpadku napájení ze sítě</span><span class="sxs-lookup"><span data-stu-id="decb0-141">AC power failure</span></span> |
   | <span data-ttu-id="decb0-142">2</span><span class="sxs-lookup"><span data-stu-id="decb0-142">2</span></span> |<span data-ttu-id="decb0-143">Ventilátor selhání</span><span class="sxs-lookup"><span data-stu-id="decb0-143">Fan failure</span></span> |
   | <span data-ttu-id="decb0-144">3</span><span class="sxs-lookup"><span data-stu-id="decb0-144">3</span></span> |<span data-ttu-id="decb0-145">Selhání stav baterie.</span><span class="sxs-lookup"><span data-stu-id="decb0-145">Battery fault</span></span> |
   | <span data-ttu-id="decb0-146">4</span><span class="sxs-lookup"><span data-stu-id="decb0-146">4</span></span> |<span data-ttu-id="decb0-147">PCM OK</span><span class="sxs-lookup"><span data-stu-id="decb0-147">PCM OK</span></span> |
   | <span data-ttu-id="decb0-148">5</span><span class="sxs-lookup"><span data-stu-id="decb0-148">5</span></span> |<span data-ttu-id="decb0-149">Řadič domény výpadku proudu</span><span class="sxs-lookup"><span data-stu-id="decb0-149">DC power failure</span></span> |
   | <span data-ttu-id="decb0-150">6</span><span class="sxs-lookup"><span data-stu-id="decb0-150">6</span></span> |<span data-ttu-id="decb0-151">Dobrý stav baterie</span><span class="sxs-lookup"><span data-stu-id="decb0-151">Battery healthy</span></span> |
3. <span data-ttu-id="decb0-152">Chcete-li odebrat PCM s selhání baterie, postupujte podle kroků v [odebrat PCM](storsimple-power-cooling-module-replacement.md#remove-a-pcm).</span><span class="sxs-lookup"><span data-stu-id="decb0-152">To remove the PCM with a failed battery, follow the steps in [Remove a PCM](storsimple-power-cooling-module-replacement.md#remove-a-pcm).</span></span>
4. <span data-ttu-id="decb0-153">S PCM odebrána navýšení otočit popisovač modulu baterie směrem nahoru, jak je uvedeno v následující obrázek a načítat až odebrat baterie.</span><span class="sxs-lookup"><span data-stu-id="decb0-153">With the PCM removed, lift and rotate the battery module handle upward as indicated in the following figure, and pull it up to remove the battery.</span></span>
   
    ![Odebráním PCM stav baterie.](./media/storsimple-battery-replacement/IC741019.png)
   
    <span data-ttu-id="decb0-155">**Obrázek 3** odebráním PCM baterie</span><span class="sxs-lookup"><span data-stu-id="decb0-155">**Figure 3** Removing the battery from the PCM</span></span>
5. <span data-ttu-id="decb0-156">Umístěte modul periferní výměnná Jednotka balení.</span><span class="sxs-lookup"><span data-stu-id="decb0-156">Place the module in the field-replaceable unit packaging.</span></span>
6. <span data-ttu-id="decb0-157">Vrátí vadný jednotky společnosti Microsoft pro správné údržby a zpracování.</span><span class="sxs-lookup"><span data-stu-id="decb0-157">Return the defective unit to Microsoft for proper servicing and handling.</span></span>

## <a name="install-a-new-backup-battery-module"></a><span data-ttu-id="decb0-158">Nainstalujte nový modul zálohování baterie</span><span class="sxs-lookup"><span data-stu-id="decb0-158">Install a new backup battery module</span></span>
<span data-ttu-id="decb0-159">Proveďte následující kroky k instalaci modulu baterie nahrazení v PCM ve skříni primární zařízení StorSimple.</span><span class="sxs-lookup"><span data-stu-id="decb0-159">Perform the following steps to install the replacement battery module in the PCM in the primary enclosure of your StorSimple device.</span></span>

#### <a name="to-install-the-battery-module"></a><span data-ttu-id="decb0-160">Nainstalovat modul stav baterie.</span><span class="sxs-lookup"><span data-stu-id="decb0-160">To install the battery module</span></span>
1. <span data-ttu-id="decb0-161">Umístěte modul zálohování baterie správnou orientaci v PCM.</span><span class="sxs-lookup"><span data-stu-id="decb0-161">Place the backup battery module in the proper orientation in the PCM.</span></span>
2. <span data-ttu-id="decb0-162">Podržte popisovač modulu baterie úplně pro konektor.</span><span class="sxs-lookup"><span data-stu-id="decb0-162">Press down the battery module handle all the way to seat the connector.</span></span>
3. <span data-ttu-id="decb0-163">Nahraďte PCM ve skříni primární podle pokynů v [nahrazení energii a chlazení modulu zařízení StorSimple](storsimple-power-cooling-module-replacement.md).</span><span class="sxs-lookup"><span data-stu-id="decb0-163">Replace the PCM in the primary enclosure by following the guidelines in [Replace a Power and Cooling Module on your StorSimple device](storsimple-power-cooling-module-replacement.md).</span></span>
4. <span data-ttu-id="decb0-164">Po dokončení nahrazení přejít na **zařízení** > **údržby** > **stavu hardwaru** na portálu Azure classic.</span><span class="sxs-lookup"><span data-stu-id="decb0-164">After the replacement is complete, go to **Devices** > **Maintenance** > **Hardware Status** in the Azure classic portal.</span></span> <span data-ttu-id="decb0-165">Zkontrolujte stav baterie a ujistěte se, že instalace proběhla úspěšně.</span><span class="sxs-lookup"><span data-stu-id="decb0-165">Verify the status of the battery to make sure that the installation was successful.</span></span> <span data-ttu-id="decb0-166">Zelený stav označuje, že je v pořádku baterie.</span><span class="sxs-lookup"><span data-stu-id="decb0-166">A green status indicates that the battery is healthy.</span></span>

## <a name="maintain-the-backup-battery-module"></a><span data-ttu-id="decb0-167">Udržovat modul zálohování baterie</span><span class="sxs-lookup"><span data-stu-id="decb0-167">Maintain the backup battery module</span></span>
<span data-ttu-id="decb0-168">V zařízení StorSimple poskytuje modul zálohování baterie power řadiče při ztrátě událostí do power.</span><span class="sxs-lookup"><span data-stu-id="decb0-168">In your StorSimple device, the backup battery module provides power to the controller during a power loss event.</span></span> <span data-ttu-id="decb0-169">Umožňuje zařízení StorSimple pro uložení důležitých dat před vypíná řízené způsobem.</span><span class="sxs-lookup"><span data-stu-id="decb0-169">It allows the StorSimple device to save critical data prior to shutting down in a controlled manner.</span></span> <span data-ttu-id="decb0-170">S dvě plně účtovat baterie v PCMs systému může zpracovávat dvě po sobě jdoucích ztrátu události.</span><span class="sxs-lookup"><span data-stu-id="decb0-170">With two fully charged batteries in the PCMs, the system can handle two consecutive loss events.</span></span>

<span data-ttu-id="decb0-171">Na portálu Azure classic **stavu hardwaru** na **údržby** stránky určuje, zda pracuje správně baterie nebo se blíží koncoví-dobu životnosti.</span><span class="sxs-lookup"><span data-stu-id="decb0-171">In the Azure classic portal, the **Hardware Status** on the **Maintenance** page indicates whether the battery is malfunctioning or the end-of-life is approaching.</span></span> <span data-ttu-id="decb0-172">Je indikován stav baterie **baterie v PCM 0** nebo **baterie v PCM 1** pod **sdílené součásti**.</span><span class="sxs-lookup"><span data-stu-id="decb0-172">The battery status is indicated by **Battery in PCM 0** or **Battery in PCM 1** under **Shared Components**.</span></span> <span data-ttu-id="decb0-173">Tato stránka se zobrazí **SNÍŽENÝ** stavu pro ukončenou životností blíží, a **se nezdařilo** pro koncové životnosti dostupný.</span><span class="sxs-lookup"><span data-stu-id="decb0-173">This page will show a **DEGRADED** state for end-of-life approaching, and **FAILED** for end-of-life reached.</span></span> 

> [!NOTE]
> <span data-ttu-id="decb0-174">Může hlásit baterie **se nezdařilo** když je jednoduše potřeba nic nestrhne.</span><span class="sxs-lookup"><span data-stu-id="decb0-174">The battery can report **FAILED** when it simply needs to be charged.</span></span>
> 
> 

<span data-ttu-id="decb0-175">Pokud **SNÍŽENÝ** stav se zobrazí, doporučujeme během následující akce:</span><span class="sxs-lookup"><span data-stu-id="decb0-175">If the **DEGRADED** state appears, we recommend the following course of action:</span></span>

* <span data-ttu-id="decb0-176">Systém pravděpodobně došlo k poslední výpadku napájení nebo baterie může probíhat periodické údržby.</span><span class="sxs-lookup"><span data-stu-id="decb0-176">The system may have experienced a recent power loss or the batteries may be undergoing periodic maintenance.</span></span> <span data-ttu-id="decb0-177">Sledujte systému 12 hodin, než budete pokračovat.</span><span class="sxs-lookup"><span data-stu-id="decb0-177">Observe the system for 12 hours before proceeding.</span></span>
  
  * <span data-ttu-id="decb0-178">Pokud stav není stále **SNÍŽENÝ** po 12 hodinách nepřetržité připojení k AC spotřeby s řadiči a PCMs systémem, pak baterie je nutné vyměnit.</span><span class="sxs-lookup"><span data-stu-id="decb0-178">If the state is still **DEGRADED** after 12 hours of continuous connection to AC power with the controllers and PCMs running, then the battery needs to be replaced.</span></span> <span data-ttu-id="decb0-179">Prosím [kontaktovat Microsoft Support](storsimple-contact-microsoft-support.md) pro modul zálohování baterie nahrazení.</span><span class="sxs-lookup"><span data-stu-id="decb0-179">Please [contact Microsoft Support](storsimple-contact-microsoft-support.md) for a replacement backup battery module.</span></span>
  * <span data-ttu-id="decb0-180">Pokud stav se změní na OK po 12 hodinách, baterie funkční a potřeba jenom údržby poplatků.</span><span class="sxs-lookup"><span data-stu-id="decb0-180">If the state becomes OK after 12 hours, the battery is operational, and it only needed a maintenance charge.</span></span>
* <span data-ttu-id="decb0-181">Pokud nedošlo k přidružené ke ztrátě napájení a PCM je zapnutý a připojený k napájení ze sítě, je nutné vyměnit baterie.</span><span class="sxs-lookup"><span data-stu-id="decb0-181">If there has not been an associated loss of AC power and the PCM is turned on and connected to AC power, the battery needs to be replaced.</span></span> <span data-ttu-id="decb0-182">[Kontaktujte Microsoft Support](storsimple-contact-microsoft-support.md) pořadí modul zálohování baterie nahrazení.</span><span class="sxs-lookup"><span data-stu-id="decb0-182">[Contact Microsoft Support](storsimple-contact-microsoft-support.md) to order a replacement backup battery module.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="decb0-183">Odstranění se nezdařilo baterie podle national a místní předpisy.</span><span class="sxs-lookup"><span data-stu-id="decb0-183">Dispose of the failed battery according to national and regional regulations.</span></span> 
> 
> 

## <a name="next-steps"></a><span data-ttu-id="decb0-184">Další kroky</span><span class="sxs-lookup"><span data-stu-id="decb0-184">Next steps</span></span>
<span data-ttu-id="decb0-185">Další informace o [StorSimple hardwarové součásti nahrazení](storsimple-hardware-component-replacement.md).</span><span class="sxs-lookup"><span data-stu-id="decb0-185">Learn more about [StorSimple hardware component replacement](storsimple-hardware-component-replacement.md).</span></span>

