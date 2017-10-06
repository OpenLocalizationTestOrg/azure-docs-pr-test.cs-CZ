---
title: "aaaReplace baterie na zařízení Microsoft Azure StorSimple | Microsoft Docs"
description: "Popisuje, jak nahradit tooremove a udržovat hello modulu zálohování baterie zařízení StorSimple."
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
ms.openlocfilehash: 542774a5f451ec7ad2bd442f88598df318d8b285
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="replace-hello-backup-battery-module-on-your-storsimple-device"></a><span data-ttu-id="2539d-103">Nahraďte hello modulu zálohování baterie zařízení StorSimple</span><span class="sxs-lookup"><span data-stu-id="2539d-103">Replace hello backup battery module on your StorSimple device</span></span>
## <a name="overview"></a><span data-ttu-id="2539d-104">Přehled</span><span class="sxs-lookup"><span data-stu-id="2539d-104">Overview</span></span>
<span data-ttu-id="2539d-105">primární skříň Hello napájení a chlazení modulu (PCM) na zařízení s Microsoft Azure StorSimple má balík další stav baterie.</span><span class="sxs-lookup"><span data-stu-id="2539d-105">hello primary enclosure Power and Cooling Module (PCM) on your Microsoft Azure StorSimple device has an additional battery pack.</span></span> <span data-ttu-id="2539d-106">Tento balíček poskytuje power tak, aby hello zařízení StorSimple můžete uložit data Pokud dojde ke ztrátě AC power toohello primární skříň.</span><span class="sxs-lookup"><span data-stu-id="2539d-106">This pack provides power so that hello StorSimple device can save data if there is loss of AC power toohello primary enclosure.</span></span> <span data-ttu-id="2539d-107">Tato sada pack baterie je hello odkazované tooas *zálohování baterie modulu*.</span><span class="sxs-lookup"><span data-stu-id="2539d-107">This battery pack is referred tooas hello *backup battery module*.</span></span> <span data-ttu-id="2539d-108">modul zálohování baterie Hello existuje pouze pro primární skříň hello v zařízení StorSimple (hello EBOD skříň neobsahuje modul zálohování baterie).</span><span class="sxs-lookup"><span data-stu-id="2539d-108">hello backup battery module exists only for hello primary enclosure in your StorSimple device (hello EBOD enclosure does not contain a backup battery module).</span></span> 

<span data-ttu-id="2539d-109">Tento kurz vysvětluje postup:</span><span class="sxs-lookup"><span data-stu-id="2539d-109">This tutorial explains how to:</span></span>

* <span data-ttu-id="2539d-110">Modul zálohování baterie hello odebrat</span><span class="sxs-lookup"><span data-stu-id="2539d-110">Remove hello backup battery module</span></span> 
* <span data-ttu-id="2539d-111">Nainstalujte nový modul zálohování baterie</span><span class="sxs-lookup"><span data-stu-id="2539d-111">Install a new backup battery module</span></span>
* <span data-ttu-id="2539d-112">Udržovat hello zálohování baterie modulu</span><span class="sxs-lookup"><span data-stu-id="2539d-112">Maintain hello backup battery module</span></span>

> [!IMPORTANT]
> <span data-ttu-id="2539d-113">Před odebírání a nahrazování modul zálohování baterie zkontrolovat informace o zabezpečení hello v hello [Úvod tooStorSimple hardwarové součásti nahrazení](storsimple-hardware-component-replacement.md).</span><span class="sxs-lookup"><span data-stu-id="2539d-113">Before removing and replacing a backup battery module, review hello safety information in hello [Introduction tooStorSimple hardware component replacement](storsimple-hardware-component-replacement.md).</span></span>
> 
> 

## <a name="remove-hello-backup-battery-module"></a><span data-ttu-id="2539d-114">Modul zálohování baterie hello odebrat</span><span class="sxs-lookup"><span data-stu-id="2539d-114">Remove hello backup battery module</span></span>
<span data-ttu-id="2539d-115">modul Hello zálohování baterie pro zařízení StorSimple je periferní Výměnná jednotka.</span><span class="sxs-lookup"><span data-stu-id="2539d-115">hello backup battery module for your StorSimple device is a field-replaceable unit.</span></span> <span data-ttu-id="2539d-116">Před instalací v hello PCM, modul baterie hello by měly být uložené v původním balení.</span><span class="sxs-lookup"><span data-stu-id="2539d-116">Before it is installed in hello PCM, hello battery module should be stored in its original packaging.</span></span> <span data-ttu-id="2539d-117">Proveďte následující kroky tooremove hello zálohování baterie hello.</span><span class="sxs-lookup"><span data-stu-id="2539d-117">Perform hello following steps tooremove hello backup battery.</span></span>

#### <a name="tooremove-hello-backup-battery-module"></a><span data-ttu-id="2539d-118">modul zálohování baterie tooremove hello</span><span class="sxs-lookup"><span data-stu-id="2539d-118">tooremove hello backup battery module</span></span>
1. <span data-ttu-id="2539d-119">V hello portál Azure classic, přejděte příliš**zařízení** > **údržby** > **stavu hardwaru**.</span><span class="sxs-lookup"><span data-stu-id="2539d-119">In hello Azure classic portal, go too**Devices** > **Maintenance** > **Hardware Status**.</span></span> <span data-ttu-id="2539d-120">V části **sdílené součásti**, podívejte se na stav baterie hello hello.</span><span class="sxs-lookup"><span data-stu-id="2539d-120">Under **Shared Components**, look at hello status of hello battery.</span></span>
2. <span data-ttu-id="2539d-121">Identifikujte hello PCM, ve které hello baterie selhal.</span><span class="sxs-lookup"><span data-stu-id="2539d-121">Identify hello PCM in which hello battery has failed.</span></span> <span data-ttu-id="2539d-122">Obrázek 1 zobrazuje hello zadní hello zařízení StorSimple.</span><span class="sxs-lookup"><span data-stu-id="2539d-122">Figure 1 shows hello back of hello StorSimple device.</span></span>
   
    ![Propojovací rozhraní systému modulů skříň primární zařízení](./media/storsimple-battery-replacement/IC740994.png)
   
    <span data-ttu-id="2539d-124">**Obrázek 1** zadní zobrazující modulů PCM a řadiče primární zařízení</span><span class="sxs-lookup"><span data-stu-id="2539d-124">**Figure 1** Back of primary device showing PCM and controller modules</span></span>
   
   | <span data-ttu-id="2539d-125">Štítek</span><span class="sxs-lookup"><span data-stu-id="2539d-125">Label</span></span> | <span data-ttu-id="2539d-126">Popis</span><span class="sxs-lookup"><span data-stu-id="2539d-126">Description</span></span> |
   |:--- |:--- |
   | <span data-ttu-id="2539d-127">1</span><span class="sxs-lookup"><span data-stu-id="2539d-127">1</span></span> |<span data-ttu-id="2539d-128">PCM 0</span><span class="sxs-lookup"><span data-stu-id="2539d-128">PCM 0</span></span> |
   | <span data-ttu-id="2539d-129">2</span><span class="sxs-lookup"><span data-stu-id="2539d-129">2</span></span> |<span data-ttu-id="2539d-130">PCM 1</span><span class="sxs-lookup"><span data-stu-id="2539d-130">PCM 1</span></span> |
   | <span data-ttu-id="2539d-131">3</span><span class="sxs-lookup"><span data-stu-id="2539d-131">3</span></span> |<span data-ttu-id="2539d-132">Řadič 0</span><span class="sxs-lookup"><span data-stu-id="2539d-132">Controller 0</span></span> |
   | <span data-ttu-id="2539d-133">4</span><span class="sxs-lookup"><span data-stu-id="2539d-133">4</span></span> |<span data-ttu-id="2539d-134">Řadič 1</span><span class="sxs-lookup"><span data-stu-id="2539d-134">Controller 1</span></span> |
   
    <span data-ttu-id="2539d-135">Jak je znázorněno číslem 3 v hello obrázek 2, hello monitorování indikátor VEDLA na PCM 0, která odpovídá příliš**baterie selhání** by měl být lit.</span><span class="sxs-lookup"><span data-stu-id="2539d-135">As shown by number 3 in hello Figure 2, hello monitoring indicator LED on PCM 0 that corresponds too**Battery Fault** should be lit.</span></span>
   
    ![Propojovací rozhraní systému zařízení PCM monitorování kláves](./media/storsimple-battery-replacement/IC740992.png)
   
    <span data-ttu-id="2539d-137">**Obrázek 2** zpět of PCM zobrazující hello monitorování kláves</span><span class="sxs-lookup"><span data-stu-id="2539d-137">**Figure 2** Back of PCM showing hello monitoring indicator LEDs</span></span>
   
   | <span data-ttu-id="2539d-138">Štítek</span><span class="sxs-lookup"><span data-stu-id="2539d-138">Label</span></span> | <span data-ttu-id="2539d-139">Popis</span><span class="sxs-lookup"><span data-stu-id="2539d-139">Description</span></span> |
   |:--- |:--- |
   | <span data-ttu-id="2539d-140">1</span><span class="sxs-lookup"><span data-stu-id="2539d-140">1</span></span> |<span data-ttu-id="2539d-141">Výpadku napájení ze sítě</span><span class="sxs-lookup"><span data-stu-id="2539d-141">AC power failure</span></span> |
   | <span data-ttu-id="2539d-142">2</span><span class="sxs-lookup"><span data-stu-id="2539d-142">2</span></span> |<span data-ttu-id="2539d-143">Ventilátor selhání</span><span class="sxs-lookup"><span data-stu-id="2539d-143">Fan failure</span></span> |
   | <span data-ttu-id="2539d-144">3</span><span class="sxs-lookup"><span data-stu-id="2539d-144">3</span></span> |<span data-ttu-id="2539d-145">Selhání stav baterie.</span><span class="sxs-lookup"><span data-stu-id="2539d-145">Battery fault</span></span> |
   | <span data-ttu-id="2539d-146">4</span><span class="sxs-lookup"><span data-stu-id="2539d-146">4</span></span> |<span data-ttu-id="2539d-147">PCM OK</span><span class="sxs-lookup"><span data-stu-id="2539d-147">PCM OK</span></span> |
   | <span data-ttu-id="2539d-148">5</span><span class="sxs-lookup"><span data-stu-id="2539d-148">5</span></span> |<span data-ttu-id="2539d-149">Řadič domény výpadku proudu</span><span class="sxs-lookup"><span data-stu-id="2539d-149">DC power failure</span></span> |
   | <span data-ttu-id="2539d-150">6</span><span class="sxs-lookup"><span data-stu-id="2539d-150">6</span></span> |<span data-ttu-id="2539d-151">Dobrý stav baterie</span><span class="sxs-lookup"><span data-stu-id="2539d-151">Battery healthy</span></span> |
3. <span data-ttu-id="2539d-152">tooremove hello PCM s selhání baterie, postupujte podle kroků hello v [odebrat PCM](storsimple-power-cooling-module-replacement.md#remove-a-pcm).</span><span class="sxs-lookup"><span data-stu-id="2539d-152">tooremove hello PCM with a failed battery, follow hello steps in [Remove a PCM](storsimple-power-cooling-module-replacement.md#remove-a-pcm).</span></span>
4. <span data-ttu-id="2539d-153">S hello PCM odebrány, navýšení a otáčení hello baterie modul zpracování směrem nahoru, jak je uvedeno v hello následující obrázek a vyžádat si tooremove hello baterie.</span><span class="sxs-lookup"><span data-stu-id="2539d-153">With hello PCM removed, lift and rotate hello battery module handle upward as indicated in hello following figure, and pull it up tooremove hello battery.</span></span>
   
    ![Odebráním PCM stav baterie.](./media/storsimple-battery-replacement/IC741019.png)
   
    <span data-ttu-id="2539d-155">**Obrázek 3** odebráním hello PCM hello baterie</span><span class="sxs-lookup"><span data-stu-id="2539d-155">**Figure 3** Removing hello battery from hello PCM</span></span>
5. <span data-ttu-id="2539d-156">Umístěte hello modulu hello periferní výměnná Jednotka balení.</span><span class="sxs-lookup"><span data-stu-id="2539d-156">Place hello module in hello field-replaceable unit packaging.</span></span>
6. <span data-ttu-id="2539d-157">Vrátí hello vadný jednotky tooMicrosoft správnou údržbu a zpracování.</span><span class="sxs-lookup"><span data-stu-id="2539d-157">Return hello defective unit tooMicrosoft for proper servicing and handling.</span></span>

## <a name="install-a-new-backup-battery-module"></a><span data-ttu-id="2539d-158">Nainstalujte nový modul zálohování baterie</span><span class="sxs-lookup"><span data-stu-id="2539d-158">Install a new backup battery module</span></span>
<span data-ttu-id="2539d-159">Proveďte následující kroky tooinstall hello nahrazení baterie modulu v hello PCM v primární skříň hello zařízení StorSimple hello.</span><span class="sxs-lookup"><span data-stu-id="2539d-159">Perform hello following steps tooinstall hello replacement battery module in hello PCM in hello primary enclosure of your StorSimple device.</span></span>

#### <a name="tooinstall-hello-battery-module"></a><span data-ttu-id="2539d-160">tooinstall hello baterie modulu</span><span class="sxs-lookup"><span data-stu-id="2539d-160">tooinstall hello battery module</span></span>
1. <span data-ttu-id="2539d-161">Umístěte hello zálohování baterie modulu hello správnou orientaci v hello PCM.</span><span class="sxs-lookup"><span data-stu-id="2539d-161">Place hello backup battery module in hello proper orientation in hello PCM.</span></span>
2. <span data-ttu-id="2539d-162">Stiskněte klávesu dolů hello baterie modul zpracování všech hello způsob tooseat hello konektor.</span><span class="sxs-lookup"><span data-stu-id="2539d-162">Press down hello battery module handle all hello way tooseat hello connector.</span></span>
3. <span data-ttu-id="2539d-163">Nahraďte text hello PCM v primární skříň hello podle následujících pokynů hello v [nahrazení energii a chlazení modulu zařízení StorSimple](storsimple-power-cooling-module-replacement.md).</span><span class="sxs-lookup"><span data-stu-id="2539d-163">Replace hello PCM in hello primary enclosure by following hello guidelines in [Replace a Power and Cooling Module on your StorSimple device](storsimple-power-cooling-module-replacement.md).</span></span>
4. <span data-ttu-id="2539d-164">Po dokončení hello nahrazení přejděte příliš**zařízení** > **údržby** > **stavu hardwaru** v hello portál Azure classic.</span><span class="sxs-lookup"><span data-stu-id="2539d-164">After hello replacement is complete, go too**Devices** > **Maintenance** > **Hardware Status** in hello Azure classic portal.</span></span> <span data-ttu-id="2539d-165">Zkontrolujte stav hello toomake baterie hello se, zda text hello instalace byla úspěšná.</span><span class="sxs-lookup"><span data-stu-id="2539d-165">Verify hello status of hello battery toomake sure that hello installation was successful.</span></span> <span data-ttu-id="2539d-166">Zelený stav označuje, že baterie hello je v pořádku.</span><span class="sxs-lookup"><span data-stu-id="2539d-166">A green status indicates that hello battery is healthy.</span></span>

## <a name="maintain-hello-backup-battery-module"></a><span data-ttu-id="2539d-167">Udržovat hello zálohování baterie modulu</span><span class="sxs-lookup"><span data-stu-id="2539d-167">Maintain hello backup battery module</span></span>
<span data-ttu-id="2539d-168">V zařízení StorSimple modul zálohování baterie hello poskytuje řadič toohello spotřeby během ztrátě power.</span><span class="sxs-lookup"><span data-stu-id="2539d-168">In your StorSimple device, hello backup battery module provides power toohello controller during a power loss event.</span></span> <span data-ttu-id="2539d-169">To umožňuje hello StorSimple zařízení toosave důležitá data předchozí tooshutting dolů řízené způsobem.</span><span class="sxs-lookup"><span data-stu-id="2539d-169">It allows hello StorSimple device toosave critical data prior tooshutting down in a controlled manner.</span></span> <span data-ttu-id="2539d-170">S dvě plně účtovat baterie v hello PCMs hello systému může zpracovávat dvě po sobě jdoucích ztrátu události.</span><span class="sxs-lookup"><span data-stu-id="2539d-170">With two fully charged batteries in hello PCMs, hello system can handle two consecutive loss events.</span></span>

<span data-ttu-id="2539d-171">V hello portál Azure classic, hello **stavu hardwaru** na hello **údržby** stránky určuje, zda pracuje správně hello baterie nebo se blíží hello end životnosti.</span><span class="sxs-lookup"><span data-stu-id="2539d-171">In hello Azure classic portal, hello **Hardware Status** on hello **Maintenance** page indicates whether hello battery is malfunctioning or hello end-of-life is approaching.</span></span> <span data-ttu-id="2539d-172">je indikován stav baterie Hello **baterie v PCM 0** nebo **baterie v PCM 1** pod **sdílené součásti**.</span><span class="sxs-lookup"><span data-stu-id="2539d-172">hello battery status is indicated by **Battery in PCM 0** or **Battery in PCM 1** under **Shared Components**.</span></span> <span data-ttu-id="2539d-173">Tato stránka se zobrazí **SNÍŽENÝ** stavu pro ukončenou životností blíží, a **se nezdařilo** pro koncové životnosti dostupný.</span><span class="sxs-lookup"><span data-stu-id="2539d-173">This page will show a **DEGRADED** state for end-of-life approaching, and **FAILED** for end-of-life reached.</span></span> 

> [!NOTE]
> <span data-ttu-id="2539d-174">může hlásit Hello baterie **se nezdařilo** když ho jednoduše toobe účtovat potřebuje.</span><span class="sxs-lookup"><span data-stu-id="2539d-174">hello battery can report **FAILED** when it simply needs toobe charged.</span></span>
> 
> 

<span data-ttu-id="2539d-175">Pokud hello **SNÍŽENÝ** stav se zobrazí, doporučujeme hello následující postup:</span><span class="sxs-lookup"><span data-stu-id="2539d-175">If hello **DEGRADED** state appears, we recommend hello following course of action:</span></span>

* <span data-ttu-id="2539d-176">Hello systému pravděpodobně došlo k poslední výpadku napájení nebo hello baterie může probíhat periodické údržby.</span><span class="sxs-lookup"><span data-stu-id="2539d-176">hello system may have experienced a recent power loss or hello batteries may be undergoing periodic maintenance.</span></span> <span data-ttu-id="2539d-177">Sledujte hello systému 12 hodin, než budete pokračovat.</span><span class="sxs-lookup"><span data-stu-id="2539d-177">Observe hello system for 12 hours before proceeding.</span></span>
  
  * <span data-ttu-id="2539d-178">Pokud stav hello je stále **SNÍŽENÝ** po 12 hodinách napájení tooAC nepřetržité připojení s hello řadiče a PCMs spuštěna, pak hello stav baterie musí toobe nahradit.</span><span class="sxs-lookup"><span data-stu-id="2539d-178">If hello state is still **DEGRADED** after 12 hours of continuous connection tooAC power with hello controllers and PCMs running, then hello battery needs toobe replaced.</span></span> <span data-ttu-id="2539d-179">Prosím [kontaktovat Microsoft Support](storsimple-contact-microsoft-support.md) pro modul zálohování baterie nahrazení.</span><span class="sxs-lookup"><span data-stu-id="2539d-179">Please [contact Microsoft Support](storsimple-contact-microsoft-support.md) for a replacement backup battery module.</span></span>
  * <span data-ttu-id="2539d-180">Pokud hello stav se změní na OK po 12 hodinách, baterie hello je funkční a potřeba jenom údržby poplatků.</span><span class="sxs-lookup"><span data-stu-id="2539d-180">If hello state becomes OK after 12 hours, hello battery is operational, and it only needed a maintenance charge.</span></span>
* <span data-ttu-id="2539d-181">Pokud nedošlo k přidružené ke ztrátě napájení ze sítě a hello PCM je zapnutý a připojený tooAC power, baterie hello musí toobe nahradit.</span><span class="sxs-lookup"><span data-stu-id="2539d-181">If there has not been an associated loss of AC power and hello PCM is turned on and connected tooAC power, hello battery needs toobe replaced.</span></span> <span data-ttu-id="2539d-182">[Kontaktujte Microsoft Support](storsimple-contact-microsoft-support.md) tooorder modul zálohování baterie nahrazení.</span><span class="sxs-lookup"><span data-stu-id="2539d-182">[Contact Microsoft Support](storsimple-contact-microsoft-support.md) tooorder a replacement backup battery module.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="2539d-183">Uvolnění hello se nezdařila, baterie podle toonational a místní předpisy.</span><span class="sxs-lookup"><span data-stu-id="2539d-183">Dispose of hello failed battery according toonational and regional regulations.</span></span> 
> 
> 

## <a name="next-steps"></a><span data-ttu-id="2539d-184">Další kroky</span><span class="sxs-lookup"><span data-stu-id="2539d-184">Next steps</span></span>
<span data-ttu-id="2539d-185">Další informace o [StorSimple hardwarové součásti nahrazení](storsimple-hardware-component-replacement.md).</span><span class="sxs-lookup"><span data-stu-id="2539d-185">Learn more about [StorSimple hardware component replacement](storsimple-hardware-component-replacement.md).</span></span>

