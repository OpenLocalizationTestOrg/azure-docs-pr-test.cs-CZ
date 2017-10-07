---
title: "aaaReplace PCM na vašem zařízení řady StorSimple 8000 | Microsoft Docs"
description: "Vysvětluje, jak tooremove a nahradit text hello energii a chlazení modulu (PCM) v zařízení StorSimple"
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
ms.date: 06/02/2017
ms.author: alkohli
ms.openlocfilehash: 474fd09787c5361a81efda4de74356027ac60f47
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="replace-a-power-and-cooling-module-on-your-storsimple-device"></a><span data-ttu-id="4c5b7-103">Nahrazení energii a chlazení modulu zařízení StorSimple</span><span class="sxs-lookup"><span data-stu-id="4c5b7-103">Replace a Power and Cooling Module on your StorSimple device</span></span>
## <a name="overview"></a><span data-ttu-id="4c5b7-104">Přehled</span><span class="sxs-lookup"><span data-stu-id="4c5b7-104">Overview</span></span>
<span data-ttu-id="4c5b7-105">zdroj napájení a chladicí ventilátory, které jsou řízena pomocí hello primární a skříně EBOD se skládá Hello napájení a chlazení modulu (PCM) v zařízení s Microsoft Azure StorSimple.</span><span class="sxs-lookup"><span data-stu-id="4c5b7-105">hello Power and Cooling Module (PCM) in your Microsoft Azure StorSimple device consists of a power supply and cooling fans that are controlled through hello primary and EBOD enclosures.</span></span> <span data-ttu-id="4c5b7-106">Existuje jenom jeden model PCM, který je certifikované pro každou skříň.</span><span class="sxs-lookup"><span data-stu-id="4c5b7-106">There is only one model of PCM that is certified for each enclosure.</span></span> <span data-ttu-id="4c5b7-107">primární skříň Hello je certifikované pro 764 W PCM a skříň EBOD hello je certifikované pro 580 W PCM.</span><span class="sxs-lookup"><span data-stu-id="4c5b7-107">hello primary enclosure is certified for a 764 W PCM and hello EBOD enclosure is certified for a 580 W PCM.</span></span> <span data-ttu-id="4c5b7-108">I když jsou různé hello PCMs pro primární skříň hello a hello EBOD skříň, hello nahrazení postup se shoduje.</span><span class="sxs-lookup"><span data-stu-id="4c5b7-108">Although hello PCMs for hello primary enclosure and hello EBOD enclosure are different, hello replacement procedure is identical.</span></span>

<span data-ttu-id="4c5b7-109">Tento kurz vysvětluje postup:</span><span class="sxs-lookup"><span data-stu-id="4c5b7-109">This tutorial explains how to:</span></span>

* <span data-ttu-id="4c5b7-110">Odebrat PCM</span><span class="sxs-lookup"><span data-stu-id="4c5b7-110">Remove a PCM</span></span>
* <span data-ttu-id="4c5b7-111">Nainstalovat náhradní PCM</span><span class="sxs-lookup"><span data-stu-id="4c5b7-111">Install a replacement PCM</span></span>

> [!IMPORTANT]
> <span data-ttu-id="4c5b7-112">Před odebráním a nahraďte PCM, zkontrolujte informace zabezpečení hello v [StorSimple hardwarové součásti nahrazení](storsimple-8000-hardware-component-replacement.md).</span><span class="sxs-lookup"><span data-stu-id="4c5b7-112">Before removing and replacing a PCM, review hello safety information in [StorSimple hardware component replacement](storsimple-8000-hardware-component-replacement.md).</span></span>


## <a name="before-you-replace-a-pcm"></a><span data-ttu-id="4c5b7-113">Před nahrazením PCM</span><span class="sxs-lookup"><span data-stu-id="4c5b7-113">Before you replace a PCM</span></span>
<span data-ttu-id="4c5b7-114">Mějte na paměti následující důležité skutečnosti, před nahrazením vaší PCM Dobrý den:</span><span class="sxs-lookup"><span data-stu-id="4c5b7-114">Be aware of hello following important issues before you replace your PCM:</span></span>

* <span data-ttu-id="4c5b7-115">Pokud hello zdroj napájení z hello PCM selže, nechte hello vadný modul nainstalován, ale odebrat hello napájecí kabel.</span><span class="sxs-lookup"><span data-stu-id="4c5b7-115">If hello power supply of hello PCM fails, leave hello faulty module installed, but remove hello power cord.</span></span> <span data-ttu-id="4c5b7-116">ventilátor Hello bude pokračovat tooreceive napájení z hello skříň a bude pokračovat tooprovide správné chlazení.</span><span class="sxs-lookup"><span data-stu-id="4c5b7-116">hello fan will continue tooreceive power from hello enclosure and continue tooprovide proper cooling.</span></span> <span data-ttu-id="4c5b7-117">V případě selhání hello ventilátor hello PCM musí toobe nahradit okamžitě.</span><span class="sxs-lookup"><span data-stu-id="4c5b7-117">If hello fan fails, hello PCM needs toobe replaced immediately.</span></span>
* <span data-ttu-id="4c5b7-118">Před odebráním hello PCM, odpojte hello power od hello PCM vypnutím hlavní přepínač hello (pokud existuje) nebo fyzicky odebráním hello napájecí kabel.</span><span class="sxs-lookup"><span data-stu-id="4c5b7-118">Before removing hello PCM, disconnect hello power from hello PCM by turning off hello main switch (where present) or by physically removing hello power cord.</span></span> <span data-ttu-id="4c5b7-119">To poskytuje systém tooyour upozornění hrozí vypnutí napájení.</span><span class="sxs-lookup"><span data-stu-id="4c5b7-119">This provides a warning tooyour system that a power shutdown is imminent.</span></span>
* <span data-ttu-id="4c5b7-120">Ujistěte se, že tento hello jiných PCM je funkční pro pokračování operace systému před nahrazení hello vadný PCM.</span><span class="sxs-lookup"><span data-stu-id="4c5b7-120">Make sure that hello other PCM is functional for continued system operation before replacing hello faulty PCM.</span></span> <span data-ttu-id="4c5b7-121">Vadný PCM se musí nahradit odpovídajícími plně funkční PCM co nejdříve.</span><span class="sxs-lookup"><span data-stu-id="4c5b7-121">A faulty PCM must be replaced by a fully operational PCM as soon as possible.</span></span>
* <span data-ttu-id="4c5b7-122">Nahrazení modulu PCM trvá pouze několik minut toocomplete, ale jeho musí být dokončeny v rámci 10 minut odebrání přehřívání tooprevent PCM hello se nezdařilo.</span><span class="sxs-lookup"><span data-stu-id="4c5b7-122">PCM module replacement takes only few minutes toocomplete, but it must be completed within 10 minutes of removing hello failed PCM tooprevent overheating.</span></span>
* <span data-ttu-id="4c5b7-123">Všimněte si, že hello nahrazení 764 W PCM moduly dodaný z objektu pro vytváření hello neobsahují hello zálohování baterie modulu.</span><span class="sxs-lookup"><span data-stu-id="4c5b7-123">Note that hello replacement 764 W PCM modules shipped from hello factory do not contain hello backup battery module.</span></span> <span data-ttu-id="4c5b7-124">Bude potřebovat tooremove hello baterie z vaší vadný PCM a vložte ji do hello nahrazení modulu předchozí tooperforming hello nahrazení.</span><span class="sxs-lookup"><span data-stu-id="4c5b7-124">You will need tooremove hello battery from your faulty PCM and then insert it into hello replacement module prior tooperforming hello replacement.</span></span> <span data-ttu-id="4c5b7-125">Další informace najdete v tématu Jak příliš[vyjměte a vložte modul zálohování baterie](storsimple-8000-battery-replacement.md).</span><span class="sxs-lookup"><span data-stu-id="4c5b7-125">For more information, see how too[remove and insert a backup battery module](storsimple-8000-battery-replacement.md).</span></span>

## <a name="remove-a-pcm"></a><span data-ttu-id="4c5b7-126">Odebrat PCM</span><span class="sxs-lookup"><span data-stu-id="4c5b7-126">Remove a PCM</span></span>
<span data-ttu-id="4c5b7-127">Postupujte podle těchto pokynů, když jsou připravené tooremove napájení a chlazení modulu (PCM) ze zařízení s Microsoft Azure StorSimple.</span><span class="sxs-lookup"><span data-stu-id="4c5b7-127">Follow these instructions when you are ready tooremove a Power and Cooling Module (PCM) from your Microsoft Azure StorSimple device.</span></span>

> [!NOTE]
> <span data-ttu-id="4c5b7-128">Před odebráním vaší PCM, ověřte, zda máte správné nahrazení (764 W pro primární skříň hello) nebo 580 W pro hello EBOD skříň.</span><span class="sxs-lookup"><span data-stu-id="4c5b7-128">Before you remove your PCM, verify that you have a correct replacement (764 W for hello primary enclosure or 580 W for hello EBOD enclosure).</span></span>

#### <a name="tooremove-a-pcm"></a><span data-ttu-id="4c5b7-129">tooremove PCM</span><span class="sxs-lookup"><span data-stu-id="4c5b7-129">tooremove a PCM</span></span>
1. <span data-ttu-id="4c5b7-130">V hello portál Azure classic, klikněte na **Nastavení > Sledování > Stav hardwaru**.</span><span class="sxs-lookup"><span data-stu-id="4c5b7-130">In hello Azure classic portal, click **Settings > Monitor > Hardware health**.</span></span> <span data-ttu-id="4c5b7-131">Zkontrolujte stav hello součástí hello PCM pod **sdílené součásti** tooidentify, který PCM se nezdařilo:</span><span class="sxs-lookup"><span data-stu-id="4c5b7-131">Check hello status of hello PCM components under **Shared components** tooidentify which PCM has failed:</span></span>
   
   * <span data-ttu-id="4c5b7-132">Pokud se nezdařila napájení v PCM 0, hello stav **zdroj napájení v PCM 0** červená.</span><span class="sxs-lookup"><span data-stu-id="4c5b7-132">If a power supply in PCM 0 has failed, hello status of **Power Supply in PCM 0** will be red.</span></span>
   * <span data-ttu-id="4c5b7-133">Pokud napájení v PCM 1 se nezdařila, hello stav **zdroj napájení v PCM 1** červená.</span><span class="sxs-lookup"><span data-stu-id="4c5b7-133">If a power supply in PCM 1 has failed, hello status of **Power Supply in PCM 1** will be red.</span></span>
   * <span data-ttu-id="4c5b7-134">Pokud hello ventilátor v PCM 1 se nezdařila, hello stav buď **chlazení 0 pro PCM 0** nebo **chlazení 1 pro PCM 0** červená.</span><span class="sxs-lookup"><span data-stu-id="4c5b7-134">If hello fan in PCM 1 has failed, hello status of either **Cooling 0 for PCM 0** or **Cooling 1 for PCM 0** will be red.</span></span>
2. <span data-ttu-id="4c5b7-135">Vyhledejte hello selhání PCM na hello zpět z primární skříň hello.</span><span class="sxs-lookup"><span data-stu-id="4c5b7-135">Locate hello failed PCM on hello back of hello primary enclosure.</span></span> <span data-ttu-id="4c5b7-136">Pokud používáte 8600 model, určete podle hello systémové jednotky identifikační číslo zobrazené v zobrazení předního panelu DIODU hello primární skříň hello.</span><span class="sxs-lookup"><span data-stu-id="4c5b7-136">If you are running an 8600 model, identify hello primary enclosure by looking at hello System Unit Identification Number shown on hello front panel LED display.</span></span> <span data-ttu-id="4c5b7-137">Hello výchozí ID jednotky zobrazené na primární skříň hello je **00**, kdežto hello výchozí ID jednotky zobrazené na hello EBOD skříň **01**.</span><span class="sxs-lookup"><span data-stu-id="4c5b7-137">hello default Unit ID displayed on hello primary enclosure is **00**, whereas hello default Unit ID displayed on hello EBOD enclosure is **01**.</span></span> <span data-ttu-id="4c5b7-138">Hello následující obrázek a tabulka popisují hello přední panel hello DIODU zobrazení.</span><span class="sxs-lookup"><span data-stu-id="4c5b7-138">hello following diagram and table explain hello front panel of hello LED display.</span></span>
   
    ![ID systému na přední panel OPS](./media/storsimple-power-cooling-module-replacement/IC740991.png)
   
     <span data-ttu-id="4c5b7-140">**Obrázek 1** přední panel hello zařízení</span><span class="sxs-lookup"><span data-stu-id="4c5b7-140">**Figure 1** Front panel of hello device</span></span>  
   
   | <span data-ttu-id="4c5b7-141">Štítek</span><span class="sxs-lookup"><span data-stu-id="4c5b7-141">Label</span></span> | <span data-ttu-id="4c5b7-142">Popis</span><span class="sxs-lookup"><span data-stu-id="4c5b7-142">Description</span></span> |
   |:--- |:--- |
   | <span data-ttu-id="4c5b7-143">1</span><span class="sxs-lookup"><span data-stu-id="4c5b7-143">1</span></span> |<span data-ttu-id="4c5b7-144">Tlačítko pro ztlumení</span><span class="sxs-lookup"><span data-stu-id="4c5b7-144">Mute button</span></span> |
   | <span data-ttu-id="4c5b7-145">2</span><span class="sxs-lookup"><span data-stu-id="4c5b7-145">2</span></span> |<span data-ttu-id="4c5b7-146">Napájení systému</span><span class="sxs-lookup"><span data-stu-id="4c5b7-146">System power</span></span> |
   | <span data-ttu-id="4c5b7-147">3</span><span class="sxs-lookup"><span data-stu-id="4c5b7-147">3</span></span> |<span data-ttu-id="4c5b7-148">Selhání modulu</span><span class="sxs-lookup"><span data-stu-id="4c5b7-148">Module fault</span></span> |
   | <span data-ttu-id="4c5b7-149">4</span><span class="sxs-lookup"><span data-stu-id="4c5b7-149">4</span></span> |<span data-ttu-id="4c5b7-150">Logické chyby</span><span class="sxs-lookup"><span data-stu-id="4c5b7-150">Logical fault</span></span> |
   | <span data-ttu-id="4c5b7-151">5</span><span class="sxs-lookup"><span data-stu-id="4c5b7-151">5</span></span> |<span data-ttu-id="4c5b7-152">Zobrazení ID jednotky</span><span class="sxs-lookup"><span data-stu-id="4c5b7-152">Unit ID display</span></span> |
3. <span data-ttu-id="4c5b7-153">Hello monitorování kláves v hello zadní primární skříň hello lze také použít tooidentify hello PCM vadný.</span><span class="sxs-lookup"><span data-stu-id="4c5b7-153">hello monitoring indicator LEDs in hello back of hello primary enclosure can also be used tooidentify hello faulty PCM.</span></span> <span data-ttu-id="4c5b7-154">V tématu hello následující diagram a jak tabulky toounderstand toouse hello LED toolocate hello PCM vadný.</span><span class="sxs-lookup"><span data-stu-id="4c5b7-154">See hello following diagram and table toounderstand how toouse hello LEDs toolocate hello faulty PCM.</span></span> <span data-ttu-id="4c5b7-155">Například pokud hello VEDLA odpovídající toohello **ventilátor nezdaří** je lit, ventilátor hello se nezdařilo.</span><span class="sxs-lookup"><span data-stu-id="4c5b7-155">For example, if hello LED corresponding toohello **Fan Fail** is lit, hello fan has failed.</span></span> <span data-ttu-id="4c5b7-156">Podobně pokud hello zobrazí průvodce odpovídající příliš**AC selhání** je lit, napájení hello se nezdařilo.</span><span class="sxs-lookup"><span data-stu-id="4c5b7-156">Likewise, if hello LED corresponding too**AC Fail** is lit, hello power supply has failed.</span></span> 
   
    ![Propojovací rozhraní zařízení PCM monitorování ukazatele LED](./media/storsimple-power-cooling-module-replacement/IC740992.png)
   
     <span data-ttu-id="4c5b7-158">**Obrázek 2** zpět of PCM s indikátor LED</span><span class="sxs-lookup"><span data-stu-id="4c5b7-158">**Figure 2** Back of PCM with indicator LEDs</span></span>
   
   | <span data-ttu-id="4c5b7-159">Štítek</span><span class="sxs-lookup"><span data-stu-id="4c5b7-159">Label</span></span> | <span data-ttu-id="4c5b7-160">Popis</span><span class="sxs-lookup"><span data-stu-id="4c5b7-160">Description</span></span> |
   |:--- |:--- |
   | <span data-ttu-id="4c5b7-161">1</span><span class="sxs-lookup"><span data-stu-id="4c5b7-161">1</span></span> |<span data-ttu-id="4c5b7-162">Výpadku napájení ze sítě</span><span class="sxs-lookup"><span data-stu-id="4c5b7-162">AC power failure</span></span> |
   | <span data-ttu-id="4c5b7-163">2</span><span class="sxs-lookup"><span data-stu-id="4c5b7-163">2</span></span> |<span data-ttu-id="4c5b7-164">Ventilátor selhání</span><span class="sxs-lookup"><span data-stu-id="4c5b7-164">Fan failure</span></span> |
   | <span data-ttu-id="4c5b7-165">3</span><span class="sxs-lookup"><span data-stu-id="4c5b7-165">3</span></span> |<span data-ttu-id="4c5b7-166">Selhání stav baterie.</span><span class="sxs-lookup"><span data-stu-id="4c5b7-166">Battery fault</span></span> |
   | <span data-ttu-id="4c5b7-167">4</span><span class="sxs-lookup"><span data-stu-id="4c5b7-167">4</span></span> |<span data-ttu-id="4c5b7-168">PCM OK</span><span class="sxs-lookup"><span data-stu-id="4c5b7-168">PCM OK</span></span> |
   | <span data-ttu-id="4c5b7-169">5</span><span class="sxs-lookup"><span data-stu-id="4c5b7-169">5</span></span> |<span data-ttu-id="4c5b7-170">Řadič domény výpadku proudu</span><span class="sxs-lookup"><span data-stu-id="4c5b7-170">DC power failure</span></span> |
   | <span data-ttu-id="4c5b7-171">6</span><span class="sxs-lookup"><span data-stu-id="4c5b7-171">6</span></span> |<span data-ttu-id="4c5b7-172">Dobrý stav baterie</span><span class="sxs-lookup"><span data-stu-id="4c5b7-172">Battery healthy</span></span> |
4. <span data-ttu-id="4c5b7-173">Následující diagram hello zadní hello StorSimple zařízení toolocate hello se nezdařilo PCM modulu toohello naleznete v nápovědě.</span><span class="sxs-lookup"><span data-stu-id="4c5b7-173">Refer toohello following diagram of hello back of hello StorSimple device toolocate hello failed PCM module.</span></span> <span data-ttu-id="4c5b7-174">PCM 0 je na levé straně hello a PCM 1 je na správné hello.</span><span class="sxs-lookup"><span data-stu-id="4c5b7-174">PCM 0 is on hello left and PCM 1 is on hello right.</span></span> <span data-ttu-id="4c5b7-175">Hello tabulky, který následuje dále vysvětluje hello moduly.</span><span class="sxs-lookup"><span data-stu-id="4c5b7-175">hello table that follows explains hello modules.</span></span>
   
     ![Propojovací rozhraní systému modulů skříň primární zařízení](./media/storsimple-power-cooling-module-replacement/IC740994.png)
   
     <span data-ttu-id="4c5b7-177">**Obrázek 3** zadní zařízení s moduly plug-in</span><span class="sxs-lookup"><span data-stu-id="4c5b7-177">**Figure 3** Back of device with plug-in modules</span></span> 
   
   | <span data-ttu-id="4c5b7-178">Štítek</span><span class="sxs-lookup"><span data-stu-id="4c5b7-178">Label</span></span> | <span data-ttu-id="4c5b7-179">Popis</span><span class="sxs-lookup"><span data-stu-id="4c5b7-179">Description</span></span> |
   |:--- |:--- |
   | <span data-ttu-id="4c5b7-180">1</span><span class="sxs-lookup"><span data-stu-id="4c5b7-180">1</span></span> |<span data-ttu-id="4c5b7-181">PCM 0</span><span class="sxs-lookup"><span data-stu-id="4c5b7-181">PCM 0</span></span> |
   | <span data-ttu-id="4c5b7-182">2</span><span class="sxs-lookup"><span data-stu-id="4c5b7-182">2</span></span> |<span data-ttu-id="4c5b7-183">PCM 1</span><span class="sxs-lookup"><span data-stu-id="4c5b7-183">PCM 1</span></span> |
   | <span data-ttu-id="4c5b7-184">3</span><span class="sxs-lookup"><span data-stu-id="4c5b7-184">3</span></span> |<span data-ttu-id="4c5b7-185">Řadič 0</span><span class="sxs-lookup"><span data-stu-id="4c5b7-185">Controller 0</span></span> |
   | <span data-ttu-id="4c5b7-186">4</span><span class="sxs-lookup"><span data-stu-id="4c5b7-186">4</span></span> |<span data-ttu-id="4c5b7-187">Řadič 1</span><span class="sxs-lookup"><span data-stu-id="4c5b7-187">Controller 1</span></span> |
5. <span data-ttu-id="4c5b7-188">Zapnout hello vadný PCM a odpojit kabel napájení power hello.</span><span class="sxs-lookup"><span data-stu-id="4c5b7-188">Turn off hello faulty PCM and disconnect hello power supply cord.</span></span> <span data-ttu-id="4c5b7-189">Nyní můžete odebrat hello PCM.</span><span class="sxs-lookup"><span data-stu-id="4c5b7-189">You can now remove hello PCM.</span></span>
6. <span data-ttu-id="4c5b7-190">Pochopit hello západky a hello straně hello PCM zpracování mezi Flash a ukazováčkem a vměstnat je společně tooopen hello popisovač.</span><span class="sxs-lookup"><span data-stu-id="4c5b7-190">Grasp hello latch and hello side of hello PCM handle between your thumb and forefinger, and squeeze them together tooopen hello handle.</span></span>
   
    ![Otevírání PCM popisovač](./media/storsimple-power-cooling-module-replacement/IC740995.png)
   
    <span data-ttu-id="4c5b7-192">**Obrázek 4** otevírání hello PCM zpracování</span><span class="sxs-lookup"><span data-stu-id="4c5b7-192">**Figure 4** Opening hello PCM handle</span></span>
7. <span data-ttu-id="4c5b7-193">Hello úchytu zpracování a odeberte hello PCM.</span><span class="sxs-lookup"><span data-stu-id="4c5b7-193">Grip hello handle and remove hello PCM.</span></span>
   
    ![Odebrání zařízení PCM](./media/storsimple-power-cooling-module-replacement/IC740996.png)
   
    <span data-ttu-id="4c5b7-195">**Obrázek 5** odebrání hello PCM</span><span class="sxs-lookup"><span data-stu-id="4c5b7-195">**Figure 5** Removing hello PCM</span></span>

## <a name="install-a-replacement-pcm"></a><span data-ttu-id="4c5b7-196">Nainstalovat náhradní PCM</span><span class="sxs-lookup"><span data-stu-id="4c5b7-196">Install a replacement PCM</span></span>
<span data-ttu-id="4c5b7-197">Postupujte podle těchto pokynů tooinstall PCM v zařízení StorSimple.</span><span class="sxs-lookup"><span data-stu-id="4c5b7-197">Follow these instructions tooinstall a PCM in your StorSimple device.</span></span> <span data-ttu-id="4c5b7-198">Ujistěte se, že jste vložili hello zálohování baterie modulu předchozí tooinstalling hello nahrazení PCM (platí too764 pouze W PCMs).</span><span class="sxs-lookup"><span data-stu-id="4c5b7-198">Ensure that you have inserted hello backup battery module prior tooinstalling hello replacement PCM (applies too764 W PCMs only).</span></span> <span data-ttu-id="4c5b7-199">Další informace najdete v tématu Jak příliš[vyjměte a vložte modul zálohování baterie](storsimple-8000-battery-replacement.md).</span><span class="sxs-lookup"><span data-stu-id="4c5b7-199">For more information, see how too[remove and insert a backup battery module](storsimple-8000-battery-replacement.md).</span></span>

#### <a name="tooinstall-a-pcm"></a><span data-ttu-id="4c5b7-200">tooinstall PCM</span><span class="sxs-lookup"><span data-stu-id="4c5b7-200">tooinstall a PCM</span></span>
1. <span data-ttu-id="4c5b7-201">Ověřte, zda máte správné nahrazení hello PCM pro tento skříň.</span><span class="sxs-lookup"><span data-stu-id="4c5b7-201">Verify that you have hello correct replacement PCM for this enclosure.</span></span> <span data-ttu-id="4c5b7-202">primární skříň Hello musí 764 W PCM a hello EBOD skříň musí 580 W PCM.</span><span class="sxs-lookup"><span data-stu-id="4c5b7-202">hello primary enclosure needs a 764 W PCM and hello EBOD enclosure needs a 580 W PCM.</span></span> <span data-ttu-id="4c5b7-203">Byste neměli toouse hello 580 PCM W v primární skříň hello nebo hello 764 PCM W v hello EBOD skříň.</span><span class="sxs-lookup"><span data-stu-id="4c5b7-203">You should not attempt toouse hello 580 W PCM in hello Primary enclosure, or hello 764 W PCM in hello EBOD enclosure.</span></span> <span data-ttu-id="4c5b7-204">Následující obrázek ukazuje, kdy tooidentify tyto informace na hello popisku, který je připojeno toohello PCM Hello.</span><span class="sxs-lookup"><span data-stu-id="4c5b7-204">hello following image shows where tooidentify this information on hello label that is affixed toohello PCM.</span></span>
   
    ![Popisek PCM zařízení](./media/storsimple-power-cooling-module-replacement/IC740973.png)
   
    <span data-ttu-id="4c5b7-206">**Obrázek 6** PCM popisek</span><span class="sxs-lookup"><span data-stu-id="4c5b7-206">**Figure 6** PCM label</span></span>
2. <span data-ttu-id="4c5b7-207">Zkontrolujte, zda škody toohello skříň, věnujte zvláštní pozornost toohello konektory.</span><span class="sxs-lookup"><span data-stu-id="4c5b7-207">Check for damage toohello enclosure, paying particular attention toohello connectors.</span></span> 
   
   > [!NOTE]
   > <span data-ttu-id="4c5b7-208">**Pokud jsou všechny kódy PIN konektor ohnuty není nainstalovaný modul hello.**</span><span class="sxs-lookup"><span data-stu-id="4c5b7-208">**Do not install hello module if any connector pins are bent.**</span></span>
   > 
   > 
3. <span data-ttu-id="4c5b7-209">S hello PCM zpracování v hello otevřete pozici, modul hello snímku hello skříň.</span><span class="sxs-lookup"><span data-stu-id="4c5b7-209">With hello PCM handle in hello open position, slide hello module into hello enclosure.</span></span>
   
    ![Instalace zařízení PCM](./media/storsimple-power-cooling-module-replacement/IC740975.png)
   
    <span data-ttu-id="4c5b7-211">**Obrázek 7** instalace hello PCM</span><span class="sxs-lookup"><span data-stu-id="4c5b7-211">**Figure 7** Installing hello PCM</span></span>
4. <span data-ttu-id="4c5b7-212">Ruční zavření hello PCM popisovač.</span><span class="sxs-lookup"><span data-stu-id="4c5b7-212">Manually close hello PCM handle.</span></span> <span data-ttu-id="4c5b7-213">Klikněte na by měla být slyšet jako zapojí západky popisovač hello.</span><span class="sxs-lookup"><span data-stu-id="4c5b7-213">You should hear a click as hello handle latch engages.</span></span>
   
   > [!NOTE]
   > <span data-ttu-id="4c5b7-214">tooensure, který konektor mít zapojení kódy PIN, můžete se na popisovač hello bez uvolnění západky hello jemně tug hello.</span><span class="sxs-lookup"><span data-stu-id="4c5b7-214">tooensure that hello connector pins have engaged, you can gently tug on hello handle without releasing hello latch.</span></span> <span data-ttu-id="4c5b7-215">Pokud hello PCM snímky, znamená to, že tuto západku hello se zavřel před hello konektory pověření.</span><span class="sxs-lookup"><span data-stu-id="4c5b7-215">If hello PCM slides out, it implies that hello latch was closed before hello connectors engaged.</span></span>
   
5. <span data-ttu-id="4c5b7-216">Připojte zdroj energie hello power kabely toohello a toohello PCM.</span><span class="sxs-lookup"><span data-stu-id="4c5b7-216">Connect hello power cables toohello power source and toohello PCM.</span></span>
6. <span data-ttu-id="4c5b7-217">Zabezpečte hello kmen balících pomoc.</span><span class="sxs-lookup"><span data-stu-id="4c5b7-217">Secure hello strain relief bales.</span></span>
7. <span data-ttu-id="4c5b7-218">Zapněte hello PCM.</span><span class="sxs-lookup"><span data-stu-id="4c5b7-218">Turn on hello PCM.</span></span>
8. <span data-ttu-id="4c5b7-219">Ověřte, že hello nahrazení byla úspěšná: v hello portál Azure služby StorSimple Manager zařízení, přejděte tooyour zařízení a potom příliš**Nastavení > Sledování > Stav hardwaru**.</span><span class="sxs-lookup"><span data-stu-id="4c5b7-219">Verify that hello replacement was successful: in hello Azure portal of your StorSimple Device Manager service, navigate tooyour device and then too**Settings > Monitor > Hardware health**.</span></span> <span data-ttu-id="4c5b7-220">V části hello **sdílené součásti**, by měly být hello stav hello PCM.</span><span class="sxs-lookup"><span data-stu-id="4c5b7-220">Under hello **Shared components**, hello status of hello PCM should be green.</span></span>
   
   > [!NOTE]
   > <span data-ttu-id="4c5b7-221">To může trvat několik minut, než hello nahrazení PCM toocompletely inicializovat.</span><span class="sxs-lookup"><span data-stu-id="4c5b7-221">It may take a few minutes for hello replacement PCM toocompletely initialize.</span></span>

## <a name="next-steps"></a><span data-ttu-id="4c5b7-222">Další kroky</span><span class="sxs-lookup"><span data-stu-id="4c5b7-222">Next steps</span></span>
<span data-ttu-id="4c5b7-223">Další informace o [StorSimple hardwarové součásti nahrazení](storsimple-8000-hardware-component-replacement.md).</span><span class="sxs-lookup"><span data-stu-id="4c5b7-223">Learn more about [StorSimple hardware component replacement](storsimple-8000-hardware-component-replacement.md).</span></span>

