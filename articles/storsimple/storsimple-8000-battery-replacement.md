---
title: "aaaReplace baterie na zařízení řady Microsoft Azure StorSimple 8000 | Microsoft Docs"
description: "Popisuje, jak nahradit tooremove a udržovat hello modulu zálohování baterie zařízení StorSimple."
services: storsimple
documentationcenter: 
author: alkohli
manager: timlt
editor: 
ms.assetid: 
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 06/05/2017
ms.author: alkohli
ms.openlocfilehash: 5ac767807e6c3fd817d8d522629db2aceaac9bdf
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="replace-hello-backup-battery-module-on-your-storsimple-device"></a><span data-ttu-id="07ff7-103">Nahraďte hello modulu zálohování baterie zařízení StorSimple</span><span class="sxs-lookup"><span data-stu-id="07ff7-103">Replace hello backup battery module on your StorSimple device</span></span>

## <a name="overview"></a><span data-ttu-id="07ff7-104">Přehled</span><span class="sxs-lookup"><span data-stu-id="07ff7-104">Overview</span></span>
<span data-ttu-id="07ff7-105">primární skříň Hello napájení a chlazení modulu (PCM) na zařízení s Microsoft Azure StorSimple má balík další stav baterie.</span><span class="sxs-lookup"><span data-stu-id="07ff7-105">hello primary enclosure Power and Cooling Module (PCM) on your Microsoft Azure StorSimple device has an additional battery pack.</span></span> <span data-ttu-id="07ff7-106">Tento balíček poskytuje power tak, aby hello zařízení StorSimple můžete uložit data Pokud dojde ke ztrátě AC power toohello primární skříň.</span><span class="sxs-lookup"><span data-stu-id="07ff7-106">This pack provides power so that hello StorSimple device can save data if there is loss of AC power toohello primary enclosure.</span></span> <span data-ttu-id="07ff7-107">Tato sada pack baterie je hello odkazované tooas *zálohování baterie modulu*.</span><span class="sxs-lookup"><span data-stu-id="07ff7-107">This battery pack is referred tooas hello *backup battery module*.</span></span> <span data-ttu-id="07ff7-108">modul zálohování baterie Hello existuje pouze pro primární skříň hello v zařízení StorSimple (hello EBOD skříň neobsahuje modul zálohování baterie).</span><span class="sxs-lookup"><span data-stu-id="07ff7-108">hello backup battery module exists only for hello primary enclosure in your StorSimple device (hello EBOD enclosure does not contain a backup battery module).</span></span>

<span data-ttu-id="07ff7-109">Tento kurz vysvětluje postup:</span><span class="sxs-lookup"><span data-stu-id="07ff7-109">This tutorial explains how to:</span></span>

* <span data-ttu-id="07ff7-110">Modul zálohování baterie hello odebrat</span><span class="sxs-lookup"><span data-stu-id="07ff7-110">Remove hello backup battery module</span></span>
* <span data-ttu-id="07ff7-111">Nainstalujte nový modul zálohování baterie</span><span class="sxs-lookup"><span data-stu-id="07ff7-111">Install a new backup battery module</span></span>
* <span data-ttu-id="07ff7-112">Udržovat hello zálohování baterie modulu</span><span class="sxs-lookup"><span data-stu-id="07ff7-112">Maintain hello backup battery module</span></span>

> [!IMPORTANT]
> <span data-ttu-id="07ff7-113">Před odebírání a nahrazování modul zálohování baterie zkontrolovat informace o zabezpečení hello v hello [Úvod tooStorSimple hardwarové součásti nahrazení](storsimple-8000-hardware-component-replacement.md).</span><span class="sxs-lookup"><span data-stu-id="07ff7-113">Before removing and replacing a backup battery module, review hello safety information in hello [Introduction tooStorSimple hardware component replacement](storsimple-8000-hardware-component-replacement.md).</span></span>


## <a name="remove-hello-backup-battery-module"></a><span data-ttu-id="07ff7-114">Modul zálohování baterie hello odebrat</span><span class="sxs-lookup"><span data-stu-id="07ff7-114">Remove hello backup battery module</span></span>
<span data-ttu-id="07ff7-115">modul Hello zálohování baterie pro zařízení StorSimple je periferní Výměnná jednotka.</span><span class="sxs-lookup"><span data-stu-id="07ff7-115">hello backup battery module for your StorSimple device is a field-replaceable unit.</span></span> <span data-ttu-id="07ff7-116">Před instalací v hello PCM, modul baterie hello by měly být uložené v původním balení.</span><span class="sxs-lookup"><span data-stu-id="07ff7-116">Before it is installed in hello PCM, hello battery module should be stored in its original packaging.</span></span> <span data-ttu-id="07ff7-117">Proveďte následující kroky tooremove hello zálohování baterie hello.</span><span class="sxs-lookup"><span data-stu-id="07ff7-117">Perform hello following steps tooremove hello backup battery.</span></span>

#### <a name="tooremove-hello-backup-battery-module"></a><span data-ttu-id="07ff7-118">modul zálohování baterie tooremove hello</span><span class="sxs-lookup"><span data-stu-id="07ff7-118">tooremove hello backup battery module</span></span>
1. <span data-ttu-id="07ff7-119">V hello portálu Azure přejděte okno služby StorSimple Manager zařízení tooyour.</span><span class="sxs-lookup"><span data-stu-id="07ff7-119">In hello Azure portal, go tooyour StorSimple Device Manager service blade.</span></span> <span data-ttu-id="07ff7-120">Přejděte příliš**zařízení** a potom vyberte zařízení ze seznamu hello zařízení.</span><span class="sxs-lookup"><span data-stu-id="07ff7-120">Go too**Devices** and then select your device from hello list of devices.</span></span> <span data-ttu-id="07ff7-121">Přejděte příliš**monitorování** > **stavu hardwaru**.</span><span class="sxs-lookup"><span data-stu-id="07ff7-121">Navigate too**Monitor** > **Hardware health**.</span></span> <span data-ttu-id="07ff7-122">V části **sdílené součásti**, podívejte se na stav baterie hello hello.</span><span class="sxs-lookup"><span data-stu-id="07ff7-122">Under **Shared Components**, look at hello status of hello battery.</span></span>
2. <span data-ttu-id="07ff7-123">Identifikujte hello PCM, ve které hello baterie selhal.</span><span class="sxs-lookup"><span data-stu-id="07ff7-123">Identify hello PCM in which hello battery has failed.</span></span> <span data-ttu-id="07ff7-124">Obrázek 1 zobrazuje hello zadní hello zařízení StorSimple.</span><span class="sxs-lookup"><span data-stu-id="07ff7-124">Figure 1 shows hello back of hello StorSimple device.</span></span>
   
    ![Propojovací rozhraní systému modulů skříň primární zařízení](./media/storsimple-battery-replacement/IC740994.png)
   
    <span data-ttu-id="07ff7-126">**Obrázek 1** zadní zobrazující modulů PCM a řadiče primární zařízení</span><span class="sxs-lookup"><span data-stu-id="07ff7-126">**Figure 1** Back of primary device showing PCM and controller modules</span></span>
   
   | <span data-ttu-id="07ff7-127">Štítek</span><span class="sxs-lookup"><span data-stu-id="07ff7-127">Label</span></span> | <span data-ttu-id="07ff7-128">Popis</span><span class="sxs-lookup"><span data-stu-id="07ff7-128">Description</span></span> |
   |:--- |:--- |
   | <span data-ttu-id="07ff7-129">1</span><span class="sxs-lookup"><span data-stu-id="07ff7-129">1</span></span> |<span data-ttu-id="07ff7-130">PCM 0</span><span class="sxs-lookup"><span data-stu-id="07ff7-130">PCM 0</span></span> |
   | <span data-ttu-id="07ff7-131">2</span><span class="sxs-lookup"><span data-stu-id="07ff7-131">2</span></span> |<span data-ttu-id="07ff7-132">PCM 1</span><span class="sxs-lookup"><span data-stu-id="07ff7-132">PCM 1</span></span> |
   | <span data-ttu-id="07ff7-133">3</span><span class="sxs-lookup"><span data-stu-id="07ff7-133">3</span></span> |<span data-ttu-id="07ff7-134">Řadič 0</span><span class="sxs-lookup"><span data-stu-id="07ff7-134">Controller 0</span></span> |
   | <span data-ttu-id="07ff7-135">4</span><span class="sxs-lookup"><span data-stu-id="07ff7-135">4</span></span> |<span data-ttu-id="07ff7-136">Řadič 1</span><span class="sxs-lookup"><span data-stu-id="07ff7-136">Controller 1</span></span> |
   
    <span data-ttu-id="07ff7-137">Jak je znázorněno číslem 3 v hello obrázek 2, hello monitorování indikátor VEDLA na PCM 0, která odpovídá příliš**baterie selhání** by měl být lit.</span><span class="sxs-lookup"><span data-stu-id="07ff7-137">As shown by number 3 in hello Figure 2, hello monitoring indicator LED on PCM 0 that corresponds too**Battery Fault** should be lit.</span></span>
   
    ![Propojovací rozhraní systému zařízení PCM monitorování kláves](./media/storsimple-battery-replacement/IC740992.png)
   
    <span data-ttu-id="07ff7-139">**Obrázek 2** zpět of PCM zobrazující hello monitorování kláves</span><span class="sxs-lookup"><span data-stu-id="07ff7-139">**Figure 2** Back of PCM showing hello monitoring indicator LEDs</span></span>
   
   | <span data-ttu-id="07ff7-140">Štítek</span><span class="sxs-lookup"><span data-stu-id="07ff7-140">Label</span></span> | <span data-ttu-id="07ff7-141">Popis</span><span class="sxs-lookup"><span data-stu-id="07ff7-141">Description</span></span> |
   |:--- |:--- |
   | <span data-ttu-id="07ff7-142">1</span><span class="sxs-lookup"><span data-stu-id="07ff7-142">1</span></span> |<span data-ttu-id="07ff7-143">Výpadku napájení ze sítě</span><span class="sxs-lookup"><span data-stu-id="07ff7-143">AC power failure</span></span> |
   | <span data-ttu-id="07ff7-144">2</span><span class="sxs-lookup"><span data-stu-id="07ff7-144">2</span></span> |<span data-ttu-id="07ff7-145">Ventilátor selhání</span><span class="sxs-lookup"><span data-stu-id="07ff7-145">Fan failure</span></span> |
   | <span data-ttu-id="07ff7-146">3</span><span class="sxs-lookup"><span data-stu-id="07ff7-146">3</span></span> |<span data-ttu-id="07ff7-147">Selhání stav baterie.</span><span class="sxs-lookup"><span data-stu-id="07ff7-147">Battery fault</span></span> |
   | <span data-ttu-id="07ff7-148">4</span><span class="sxs-lookup"><span data-stu-id="07ff7-148">4</span></span> |<span data-ttu-id="07ff7-149">PCM OK</span><span class="sxs-lookup"><span data-stu-id="07ff7-149">PCM OK</span></span> |
   | <span data-ttu-id="07ff7-150">5</span><span class="sxs-lookup"><span data-stu-id="07ff7-150">5</span></span> |<span data-ttu-id="07ff7-151">Řadič domény výpadku proudu</span><span class="sxs-lookup"><span data-stu-id="07ff7-151">DC power failure</span></span> |
   | <span data-ttu-id="07ff7-152">6</span><span class="sxs-lookup"><span data-stu-id="07ff7-152">6</span></span> |<span data-ttu-id="07ff7-153">Dobrý stav baterie</span><span class="sxs-lookup"><span data-stu-id="07ff7-153">Battery healthy</span></span> |
3. <span data-ttu-id="07ff7-154">tooremove hello PCM s selhání baterie, postupujte podle kroků hello v [odebrat PCM](storsimple-power-cooling-module-replacement.md#remove-a-pcm).</span><span class="sxs-lookup"><span data-stu-id="07ff7-154">tooremove hello PCM with a failed battery, follow hello steps in [Remove a PCM](storsimple-power-cooling-module-replacement.md#remove-a-pcm).</span></span>
4. <span data-ttu-id="07ff7-155">S hello PCM odebrány, navýšení a otáčení hello baterie modul zpracování směrem nahoru, jak je uvedeno v hello následující obrázek a vyžádat si tooremove hello baterie.</span><span class="sxs-lookup"><span data-stu-id="07ff7-155">With hello PCM removed, lift and rotate hello battery module handle upward as indicated in hello following figure, and pull it up tooremove hello battery.</span></span>
   
    ![Odebráním PCM stav baterie.](./media/storsimple-battery-replacement/IC741019.png)
   
    <span data-ttu-id="07ff7-157">**Obrázek 3** odebráním hello PCM hello baterie</span><span class="sxs-lookup"><span data-stu-id="07ff7-157">**Figure 3** Removing hello battery from hello PCM</span></span>
5. <span data-ttu-id="07ff7-158">Umístěte hello modulu hello periferní výměnná Jednotka balení.</span><span class="sxs-lookup"><span data-stu-id="07ff7-158">Place hello module in hello field-replaceable unit packaging.</span></span>
6. <span data-ttu-id="07ff7-159">Vrátí hello vadný jednotky tooMicrosoft správnou údržbu a zpracování.</span><span class="sxs-lookup"><span data-stu-id="07ff7-159">Return hello defective unit tooMicrosoft for proper servicing and handling.</span></span>

## <a name="install-a-new-backup-battery-module"></a><span data-ttu-id="07ff7-160">Nainstalujte nový modul zálohování baterie</span><span class="sxs-lookup"><span data-stu-id="07ff7-160">Install a new backup battery module</span></span>
<span data-ttu-id="07ff7-161">Proveďte následující kroky tooinstall hello nahrazení baterie modulu v hello PCM v primární skříň hello zařízení StorSimple hello.</span><span class="sxs-lookup"><span data-stu-id="07ff7-161">Perform hello following steps tooinstall hello replacement battery module in hello PCM in hello primary enclosure of your StorSimple device.</span></span>

#### <a name="tooinstall-hello-battery-module"></a><span data-ttu-id="07ff7-162">tooinstall hello baterie modulu</span><span class="sxs-lookup"><span data-stu-id="07ff7-162">tooinstall hello battery module</span></span>
1. <span data-ttu-id="07ff7-163">Umístěte hello zálohování baterie modulu hello správnou orientaci v hello PCM.</span><span class="sxs-lookup"><span data-stu-id="07ff7-163">Place hello backup battery module in hello proper orientation in hello PCM.</span></span>
2. <span data-ttu-id="07ff7-164">Stiskněte klávesu dolů hello baterie modul zpracování všech hello způsob tooseat hello konektor.</span><span class="sxs-lookup"><span data-stu-id="07ff7-164">Press down hello battery module handle all hello way tooseat hello connector.</span></span>
3. <span data-ttu-id="07ff7-165">Nahraďte text hello PCM v primární skříň hello podle následujících pokynů hello v [nahrazení energii a chlazení modulu zařízení StorSimple](storsimple-power-cooling-module-replacement.md).</span><span class="sxs-lookup"><span data-stu-id="07ff7-165">Replace hello PCM in hello primary enclosure by following hello guidelines in [Replace a Power and Cooling Module on your StorSimple device](storsimple-power-cooling-module-replacement.md).</span></span>
4. <span data-ttu-id="07ff7-166">Po dokončení hello nahrazení přejděte tooyour zařízení a potom přejděte příliš**monitorování** > **stavu hardwaru** v hello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="07ff7-166">After hello replacement is complete, go tooyour device and then go too**Monitor** > **Hardware health** in hello Azure portal.</span></span> <span data-ttu-id="07ff7-167">Zkontrolujte stav hello toomake baterie hello se, zda text hello instalace byla úspěšná.</span><span class="sxs-lookup"><span data-stu-id="07ff7-167">Verify hello status of hello battery toomake sure that hello installation was successful.</span></span> <span data-ttu-id="07ff7-168">Zelený stav označuje, že baterie hello je v pořádku.</span><span class="sxs-lookup"><span data-stu-id="07ff7-168">A green status indicates that hello battery is healthy.</span></span>

## <a name="maintain-hello-backup-battery-module"></a><span data-ttu-id="07ff7-169">Udržovat hello zálohování baterie modulu</span><span class="sxs-lookup"><span data-stu-id="07ff7-169">Maintain hello backup battery module</span></span>
<span data-ttu-id="07ff7-170">V zařízení StorSimple modul zálohování baterie hello poskytuje řadič toohello spotřeby během ztrátě power.</span><span class="sxs-lookup"><span data-stu-id="07ff7-170">In your StorSimple device, hello backup battery module provides power toohello controller during a power loss event.</span></span> <span data-ttu-id="07ff7-171">To umožňuje hello StorSimple zařízení toosave důležitá data předchozí tooshutting dolů řízené způsobem.</span><span class="sxs-lookup"><span data-stu-id="07ff7-171">It allows hello StorSimple device toosave critical data prior tooshutting down in a controlled manner.</span></span> <span data-ttu-id="07ff7-172">S dvě plně účtovat baterie v hello PCMs hello systému může zpracovávat dvě po sobě jdoucích ztrátu události.</span><span class="sxs-lookup"><span data-stu-id="07ff7-172">With two fully charged batteries in hello PCMs, hello system can handle two consecutive loss events.</span></span>

<span data-ttu-id="07ff7-173">V hello portálu Azure, hello **stavu hardwaru** pod hello **monitorování** okno určuje, zda pracuje správně hello baterie nebo se blíží hello end životnosti.</span><span class="sxs-lookup"><span data-stu-id="07ff7-173">In hello Azure portal, hello **Hardware health** under hello **Monitor** blade indicates whether hello battery is malfunctioning or hello end-of-life is approaching.</span></span> <span data-ttu-id="07ff7-174">je indikován stav baterie Hello **baterie v PCM 0** nebo **baterie v PCM 1** pod **sdílené součásti**.</span><span class="sxs-lookup"><span data-stu-id="07ff7-174">hello battery status is indicated by **Battery in PCM 0** or **Battery in PCM 1** under **Shared Components**.</span></span> <span data-ttu-id="07ff7-175">Toto okno se zobrazí **SNÍŽENÝ** stavu pro ukončenou životností blíží, a **se nezdařilo** pro koncové životnosti dostupný.</span><span class="sxs-lookup"><span data-stu-id="07ff7-175">This blade will show a **DEGRADED** state for end-of-life approaching, and **FAILED** for end-of-life reached.</span></span>

> [!NOTE]
> <span data-ttu-id="07ff7-176">může hlásit Hello baterie **se nezdařilo** když ho jednoduše toobe účtovat potřebuje.</span><span class="sxs-lookup"><span data-stu-id="07ff7-176">hello battery can report **FAILED** when it simply needs toobe charged.</span></span>


<span data-ttu-id="07ff7-177">Pokud hello **SNÍŽENÝ** stav se zobrazí, doporučujeme hello následující postup:</span><span class="sxs-lookup"><span data-stu-id="07ff7-177">If hello **DEGRADED** state appears, we recommend hello following course of action:</span></span>

* <span data-ttu-id="07ff7-178">Hello systému pravděpodobně došlo k poslední výpadku napájení nebo hello baterie může probíhat periodické údržby.</span><span class="sxs-lookup"><span data-stu-id="07ff7-178">hello system may have experienced a recent power loss or hello batteries may be undergoing periodic maintenance.</span></span> <span data-ttu-id="07ff7-179">Sledujte hello systému 12 hodin, než budete pokračovat.</span><span class="sxs-lookup"><span data-stu-id="07ff7-179">Observe hello system for 12 hours before proceeding.</span></span>
  
  * <span data-ttu-id="07ff7-180">Pokud stav hello je stále **SNÍŽENÝ** po 12 hodinách napájení tooAC nepřetržité připojení s hello řadiče a PCMs spuštěna, pak hello stav baterie musí toobe nahradit.</span><span class="sxs-lookup"><span data-stu-id="07ff7-180">If hello state is still **DEGRADED** after 12 hours of continuous connection tooAC power with hello controllers and PCMs running, then hello battery needs toobe replaced.</span></span> <span data-ttu-id="07ff7-181">Prosím [kontaktovat Microsoft Support](storsimple-8000-contact-microsoft-support.md) pro modul zálohování baterie nahrazení.</span><span class="sxs-lookup"><span data-stu-id="07ff7-181">Please [contact Microsoft Support](storsimple-8000-contact-microsoft-support.md) for a replacement backup battery module.</span></span>
  * <span data-ttu-id="07ff7-182">Pokud hello stav se změní na OK po 12 hodinách, baterie hello je funkční a potřeba jenom údržby poplatků.</span><span class="sxs-lookup"><span data-stu-id="07ff7-182">If hello state becomes OK after 12 hours, hello battery is operational, and it only needed a maintenance charge.</span></span>
* <span data-ttu-id="07ff7-183">Pokud nedošlo k přidružené ke ztrátě napájení ze sítě a hello PCM je zapnutý a připojený tooAC power, baterie hello musí toobe nahradit.</span><span class="sxs-lookup"><span data-stu-id="07ff7-183">If there has not been an associated loss of AC power and hello PCM is turned on and connected tooAC power, hello battery needs toobe replaced.</span></span> <span data-ttu-id="07ff7-184">[Kontaktujte Microsoft Support](storsimple-8000-contact-microsoft-support.md) tooorder modul zálohování baterie nahrazení.</span><span class="sxs-lookup"><span data-stu-id="07ff7-184">[Contact Microsoft Support](storsimple-8000-contact-microsoft-support.md) tooorder a replacement backup battery module.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="07ff7-185">Uvolnění hello se nezdařila, baterie podle toonational a místní předpisy.</span><span class="sxs-lookup"><span data-stu-id="07ff7-185">Dispose of hello failed battery according toonational and regional regulations.</span></span>

## <a name="next-steps"></a><span data-ttu-id="07ff7-186">Další kroky</span><span class="sxs-lookup"><span data-stu-id="07ff7-186">Next steps</span></span>
<span data-ttu-id="07ff7-187">Další informace o [StorSimple hardwarové součásti nahrazení](storsimple-8000-hardware-component-replacement.md).</span><span class="sxs-lookup"><span data-stu-id="07ff7-187">Learn more about [StorSimple hardware component replacement](storsimple-8000-hardware-component-replacement.md).</span></span>

