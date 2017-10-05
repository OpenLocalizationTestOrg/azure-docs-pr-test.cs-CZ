---
title: "Nahraďte PCM na vašem zařízení řady StorSimple 8000 | Microsoft Docs"
description: "Vysvětluje, jak odeberete a nahradíte energii a chlazení modulu (PCM) v zařízení StorSimple"
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
ms.openlocfilehash: 7d181e6e434c998573dbea4b541cfacf7a28ee66
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="replace-a-power-and-cooling-module-on-your-storsimple-device"></a><span data-ttu-id="5d53e-103">Nahrazení energii a chlazení modulu zařízení StorSimple</span><span class="sxs-lookup"><span data-stu-id="5d53e-103">Replace a Power and Cooling Module on your StorSimple device</span></span>
## <a name="overview"></a><span data-ttu-id="5d53e-104">Přehled</span><span class="sxs-lookup"><span data-stu-id="5d53e-104">Overview</span></span>
<span data-ttu-id="5d53e-105">Zdroj napájení a chladicí ventilátory, které jsou řízena pomocí primárním serverem a skříně EBOD se skládá energii a chlazení modulu (PCM) v zařízení s Microsoft Azure StorSimple.</span><span class="sxs-lookup"><span data-stu-id="5d53e-105">The Power and Cooling Module (PCM) in your Microsoft Azure StorSimple device consists of a power supply and cooling fans that are controlled through the primary and EBOD enclosures.</span></span> <span data-ttu-id="5d53e-106">Existuje jenom jeden model PCM, který je certifikované pro každou skříň.</span><span class="sxs-lookup"><span data-stu-id="5d53e-106">There is only one model of PCM that is certified for each enclosure.</span></span> <span data-ttu-id="5d53e-107">Primární skříň je certifikované pro 764 W PCM a skříň EBOD je certifikované pro 580 W PCM.</span><span class="sxs-lookup"><span data-stu-id="5d53e-107">The primary enclosure is certified for a 764 W PCM and the EBOD enclosure is certified for a 580 W PCM.</span></span> <span data-ttu-id="5d53e-108">I když jsou různé PCMs skříni primární a EBOD skříň, postup nahrazení je stejný.</span><span class="sxs-lookup"><span data-stu-id="5d53e-108">Although the PCMs for the primary enclosure and the EBOD enclosure are different, the replacement procedure is identical.</span></span>

<span data-ttu-id="5d53e-109">Tento kurz vysvětluje postup:</span><span class="sxs-lookup"><span data-stu-id="5d53e-109">This tutorial explains how to:</span></span>

* <span data-ttu-id="5d53e-110">Odebrat PCM</span><span class="sxs-lookup"><span data-stu-id="5d53e-110">Remove a PCM</span></span>
* <span data-ttu-id="5d53e-111">Nainstalovat náhradní PCM</span><span class="sxs-lookup"><span data-stu-id="5d53e-111">Install a replacement PCM</span></span>

> [!IMPORTANT]
> <span data-ttu-id="5d53e-112">Před odebráním a nahraďte PCM, informace zabezpečení v [StorSimple hardwarové součásti nahrazení](storsimple-8000-hardware-component-replacement.md).</span><span class="sxs-lookup"><span data-stu-id="5d53e-112">Before removing and replacing a PCM, review the safety information in [StorSimple hardware component replacement](storsimple-8000-hardware-component-replacement.md).</span></span>


## <a name="before-you-replace-a-pcm"></a><span data-ttu-id="5d53e-113">Před nahrazením PCM</span><span class="sxs-lookup"><span data-stu-id="5d53e-113">Before you replace a PCM</span></span>
<span data-ttu-id="5d53e-114">Mějte na paměti následující důležité skutečnosti před nahrazením vaší PCM:</span><span class="sxs-lookup"><span data-stu-id="5d53e-114">Be aware of the following important issues before you replace your PCM:</span></span>

* <span data-ttu-id="5d53e-115">Pokud napájení PCM selže, nechte vadný modul nainstalovaný, ale odebrat napájecí kabel.</span><span class="sxs-lookup"><span data-stu-id="5d53e-115">If the power supply of the PCM fails, leave the faulty module installed, but remove the power cord.</span></span> <span data-ttu-id="5d53e-116">Ventilátor budou nadále přijímat power od skříně a pokračovat v poskytování správné chlazení.</span><span class="sxs-lookup"><span data-stu-id="5d53e-116">The fan will continue to receive power from the enclosure and continue to provide proper cooling.</span></span> <span data-ttu-id="5d53e-117">V případě selhání ventilátoru PCM je nutné vyměnit okamžitě.</span><span class="sxs-lookup"><span data-stu-id="5d53e-117">If the fan fails, the PCM needs to be replaced immediately.</span></span>
* <span data-ttu-id="5d53e-118">Před odebráním PCM, odpojte napájení PCM vypnutím hlavní přepínač (pokud existuje) nebo fyzicky odebráním napájecí kabel.</span><span class="sxs-lookup"><span data-stu-id="5d53e-118">Before removing the PCM, disconnect the power from the PCM by turning off the main switch (where present) or by physically removing the power cord.</span></span> <span data-ttu-id="5d53e-119">To poskytuje upozornění do vašeho systému hrozí vypnutí napájení.</span><span class="sxs-lookup"><span data-stu-id="5d53e-119">This provides a warning to your system that a power shutdown is imminent.</span></span>
* <span data-ttu-id="5d53e-120">Ujistěte se, že jiné PCM je funkční pro operace trvalá systému před výměnou vadný PCM.</span><span class="sxs-lookup"><span data-stu-id="5d53e-120">Make sure that the other PCM is functional for continued system operation before replacing the faulty PCM.</span></span> <span data-ttu-id="5d53e-121">Vadný PCM se musí nahradit odpovídajícími plně funkční PCM co nejdříve.</span><span class="sxs-lookup"><span data-stu-id="5d53e-121">A faulty PCM must be replaced by a fully operational PCM as soon as possible.</span></span>
* <span data-ttu-id="5d53e-122">Nahrazení modulu PCM trvá pouze několik minut, ale jeho musí být dokončeny v rámci 10 minut odebrání selhání PCM, aby nedocházelo k přehřívání.</span><span class="sxs-lookup"><span data-stu-id="5d53e-122">PCM module replacement takes only few minutes to complete, but it must be completed within 10 minutes of removing the failed PCM to prevent overheating.</span></span>
* <span data-ttu-id="5d53e-123">Všimněte si, že nahrazení 764 W PCM moduly dodaný z objektu pro vytváření neobsahují modul zálohování baterie.</span><span class="sxs-lookup"><span data-stu-id="5d53e-123">Note that the replacement 764 W PCM modules shipped from the factory do not contain the backup battery module.</span></span> <span data-ttu-id="5d53e-124">Musíte se k odebrání baterie vaší vadný PCM a vložte ji do modulu nahrazení před provedením nahrazení.</span><span class="sxs-lookup"><span data-stu-id="5d53e-124">You will need to remove the battery from your faulty PCM and then insert it into the replacement module prior to performing the replacement.</span></span> <span data-ttu-id="5d53e-125">Další informace najdete v tématu Jak [vyjměte a vložte modul zálohování baterie](storsimple-8000-battery-replacement.md).</span><span class="sxs-lookup"><span data-stu-id="5d53e-125">For more information, see how to [remove and insert a backup battery module](storsimple-8000-battery-replacement.md).</span></span>

## <a name="remove-a-pcm"></a><span data-ttu-id="5d53e-126">Odebrat PCM</span><span class="sxs-lookup"><span data-stu-id="5d53e-126">Remove a PCM</span></span>
<span data-ttu-id="5d53e-127">Až budete připraveni k odebrání zařízení s Microsoft Azure StorSimple energii a chlazení modulu (PCM), postupujte podle těchto pokynů.</span><span class="sxs-lookup"><span data-stu-id="5d53e-127">Follow these instructions when you are ready to remove a Power and Cooling Module (PCM) from your Microsoft Azure StorSimple device.</span></span>

> [!NOTE]
> <span data-ttu-id="5d53e-128">Před odebráním vaší PCM, ověřte, zda máte správné nahrazení (764 W pro primární skříň) nebo 580 W pro EBOD skříň.</span><span class="sxs-lookup"><span data-stu-id="5d53e-128">Before you remove your PCM, verify that you have a correct replacement (764 W for the primary enclosure or 580 W for the EBOD enclosure).</span></span>

#### <a name="to-remove-a-pcm"></a><span data-ttu-id="5d53e-129">Chcete-li odebrat PCM</span><span class="sxs-lookup"><span data-stu-id="5d53e-129">To remove a PCM</span></span>
1. <span data-ttu-id="5d53e-130">Na portálu Azure classic klikněte na tlačítko **Nastavení > Sledování > Stav hardwaru**.</span><span class="sxs-lookup"><span data-stu-id="5d53e-130">In the Azure classic portal, click **Settings > Monitor > Hardware health**.</span></span> <span data-ttu-id="5d53e-131">Zkontrolujte stav komponenty PCM pod **sdílené součásti** Chcete-li zjistit, jaké PCM se nezdařilo:</span><span class="sxs-lookup"><span data-stu-id="5d53e-131">Check the status of the PCM components under **Shared components** to identify which PCM has failed:</span></span>
   
   * <span data-ttu-id="5d53e-132">Pokud se ke zdroji napájení v PCM 0 nezdařila, stav **zdroj napájení v PCM 0** červená.</span><span class="sxs-lookup"><span data-stu-id="5d53e-132">If a power supply in PCM 0 has failed, the status of **Power Supply in PCM 0** will be red.</span></span>
   * <span data-ttu-id="5d53e-133">Pokud se ke zdroji napájení v PCM 1 nezdařila, stav **zdroj napájení v PCM 1** červená.</span><span class="sxs-lookup"><span data-stu-id="5d53e-133">If a power supply in PCM 1 has failed, the status of **Power Supply in PCM 1** will be red.</span></span>
   * <span data-ttu-id="5d53e-134">Pokud se nezdařila ventilátor v PCM 1, stav buď **chlazení 0 pro PCM 0** nebo **chlazení 1 pro PCM 0** červená.</span><span class="sxs-lookup"><span data-stu-id="5d53e-134">If the fan in PCM 1 has failed, the status of either **Cooling 0 for PCM 0** or **Cooling 1 for PCM 0** will be red.</span></span>
2. <span data-ttu-id="5d53e-135">Vyhledání chybných PCM na zadní straně primární skříň.</span><span class="sxs-lookup"><span data-stu-id="5d53e-135">Locate the failed PCM on the back of the primary enclosure.</span></span> <span data-ttu-id="5d53e-136">Pokud používáte 8600 model, určete primární skříň podle identifikační číslo jednotky systému zobrazený na přední panel DIODU zobrazení.</span><span class="sxs-lookup"><span data-stu-id="5d53e-136">If you are running an 8600 model, identify the primary enclosure by looking at the System Unit Identification Number shown on the front panel LED display.</span></span> <span data-ttu-id="5d53e-137">Výchozí hodnota je ID jednotky zobrazené na primární skříň **00**, zatímco výchozí hodnota je ID jednotky zobrazené na skříni EBOD **01**.</span><span class="sxs-lookup"><span data-stu-id="5d53e-137">The default Unit ID displayed on the primary enclosure is **00**, whereas the default Unit ID displayed on the EBOD enclosure is **01**.</span></span> <span data-ttu-id="5d53e-138">Následující obrázek a tabulka popisují přední panel DIODU zobrazení.</span><span class="sxs-lookup"><span data-stu-id="5d53e-138">The following diagram and table explain the front panel of the LED display.</span></span>
   
    ![ID systému na přední panel OPS](./media/storsimple-power-cooling-module-replacement/IC740991.png)
   
     <span data-ttu-id="5d53e-140">**Obrázek 1** přední panel zařízení</span><span class="sxs-lookup"><span data-stu-id="5d53e-140">**Figure 1** Front panel of the device</span></span>  
   
   | <span data-ttu-id="5d53e-141">Štítek</span><span class="sxs-lookup"><span data-stu-id="5d53e-141">Label</span></span> | <span data-ttu-id="5d53e-142">Popis</span><span class="sxs-lookup"><span data-stu-id="5d53e-142">Description</span></span> |
   |:--- |:--- |
   | <span data-ttu-id="5d53e-143">1</span><span class="sxs-lookup"><span data-stu-id="5d53e-143">1</span></span> |<span data-ttu-id="5d53e-144">Tlačítko pro ztlumení</span><span class="sxs-lookup"><span data-stu-id="5d53e-144">Mute button</span></span> |
   | <span data-ttu-id="5d53e-145">2</span><span class="sxs-lookup"><span data-stu-id="5d53e-145">2</span></span> |<span data-ttu-id="5d53e-146">Napájení systému</span><span class="sxs-lookup"><span data-stu-id="5d53e-146">System power</span></span> |
   | <span data-ttu-id="5d53e-147">3</span><span class="sxs-lookup"><span data-stu-id="5d53e-147">3</span></span> |<span data-ttu-id="5d53e-148">Selhání modulu</span><span class="sxs-lookup"><span data-stu-id="5d53e-148">Module fault</span></span> |
   | <span data-ttu-id="5d53e-149">4</span><span class="sxs-lookup"><span data-stu-id="5d53e-149">4</span></span> |<span data-ttu-id="5d53e-150">Logické chyby</span><span class="sxs-lookup"><span data-stu-id="5d53e-150">Logical fault</span></span> |
   | <span data-ttu-id="5d53e-151">5</span><span class="sxs-lookup"><span data-stu-id="5d53e-151">5</span></span> |<span data-ttu-id="5d53e-152">Zobrazení ID jednotky</span><span class="sxs-lookup"><span data-stu-id="5d53e-152">Unit ID display</span></span> |
3. <span data-ttu-id="5d53e-153">Monitorování indikátoru LED zadní primární skříň lze také identifikovat vadný PCM.</span><span class="sxs-lookup"><span data-stu-id="5d53e-153">The monitoring indicator LEDs in the back of the primary enclosure can also be used to identify the faulty PCM.</span></span> <span data-ttu-id="5d53e-154">Viz následující obrázek a tabulka pochopit, jak vyhledávat vadný PCM pomocí indikátory LED.</span><span class="sxs-lookup"><span data-stu-id="5d53e-154">See the following diagram and table to understand how to use the LEDs to locate the faulty PCM.</span></span> <span data-ttu-id="5d53e-155">Například pokud DIODU odpovídající k **ventilátor nezdaří** je lit, ventilátoru se nezdařilo.</span><span class="sxs-lookup"><span data-stu-id="5d53e-155">For example, if the LED corresponding to the **Fan Fail** is lit, the fan has failed.</span></span> <span data-ttu-id="5d53e-156">Podobně pokud DIODU odpovídající k **AC selhání** je lit, napájení se nezdařilo.</span><span class="sxs-lookup"><span data-stu-id="5d53e-156">Likewise, if the LED corresponding to **AC Fail** is lit, the power supply has failed.</span></span> 
   
    ![Propojovací rozhraní zařízení PCM monitorování ukazatele LED](./media/storsimple-power-cooling-module-replacement/IC740992.png)
   
     <span data-ttu-id="5d53e-158">**Obrázek 2** zpět of PCM s indikátor LED</span><span class="sxs-lookup"><span data-stu-id="5d53e-158">**Figure 2** Back of PCM with indicator LEDs</span></span>
   
   | <span data-ttu-id="5d53e-159">Štítek</span><span class="sxs-lookup"><span data-stu-id="5d53e-159">Label</span></span> | <span data-ttu-id="5d53e-160">Popis</span><span class="sxs-lookup"><span data-stu-id="5d53e-160">Description</span></span> |
   |:--- |:--- |
   | <span data-ttu-id="5d53e-161">1</span><span class="sxs-lookup"><span data-stu-id="5d53e-161">1</span></span> |<span data-ttu-id="5d53e-162">Výpadku napájení ze sítě</span><span class="sxs-lookup"><span data-stu-id="5d53e-162">AC power failure</span></span> |
   | <span data-ttu-id="5d53e-163">2</span><span class="sxs-lookup"><span data-stu-id="5d53e-163">2</span></span> |<span data-ttu-id="5d53e-164">Ventilátor selhání</span><span class="sxs-lookup"><span data-stu-id="5d53e-164">Fan failure</span></span> |
   | <span data-ttu-id="5d53e-165">3</span><span class="sxs-lookup"><span data-stu-id="5d53e-165">3</span></span> |<span data-ttu-id="5d53e-166">Selhání stav baterie.</span><span class="sxs-lookup"><span data-stu-id="5d53e-166">Battery fault</span></span> |
   | <span data-ttu-id="5d53e-167">4</span><span class="sxs-lookup"><span data-stu-id="5d53e-167">4</span></span> |<span data-ttu-id="5d53e-168">PCM OK</span><span class="sxs-lookup"><span data-stu-id="5d53e-168">PCM OK</span></span> |
   | <span data-ttu-id="5d53e-169">5</span><span class="sxs-lookup"><span data-stu-id="5d53e-169">5</span></span> |<span data-ttu-id="5d53e-170">Řadič domény výpadku proudu</span><span class="sxs-lookup"><span data-stu-id="5d53e-170">DC power failure</span></span> |
   | <span data-ttu-id="5d53e-171">6</span><span class="sxs-lookup"><span data-stu-id="5d53e-171">6</span></span> |<span data-ttu-id="5d53e-172">Dobrý stav baterie</span><span class="sxs-lookup"><span data-stu-id="5d53e-172">Battery healthy</span></span> |
4. <span data-ttu-id="5d53e-173">Naleznete na následující obrázek pozadí zařízení StorSimple najít PCM modulu se nezdařilo.</span><span class="sxs-lookup"><span data-stu-id="5d53e-173">Refer to the following diagram of the back of the StorSimple device to locate the failed PCM module.</span></span> <span data-ttu-id="5d53e-174">PCM 0 je na levé straně a PCM 1 je na pravé straně.</span><span class="sxs-lookup"><span data-stu-id="5d53e-174">PCM 0 is on the left and PCM 1 is on the right.</span></span> <span data-ttu-id="5d53e-175">Následující tabulka vysvětluje moduly.</span><span class="sxs-lookup"><span data-stu-id="5d53e-175">The table that follows explains the modules.</span></span>
   
     ![Propojovací rozhraní systému modulů skříň primární zařízení](./media/storsimple-power-cooling-module-replacement/IC740994.png)
   
     <span data-ttu-id="5d53e-177">**Obrázek 3** zadní zařízení s moduly plug-in</span><span class="sxs-lookup"><span data-stu-id="5d53e-177">**Figure 3** Back of device with plug-in modules</span></span> 
   
   | <span data-ttu-id="5d53e-178">Štítek</span><span class="sxs-lookup"><span data-stu-id="5d53e-178">Label</span></span> | <span data-ttu-id="5d53e-179">Popis</span><span class="sxs-lookup"><span data-stu-id="5d53e-179">Description</span></span> |
   |:--- |:--- |
   | <span data-ttu-id="5d53e-180">1</span><span class="sxs-lookup"><span data-stu-id="5d53e-180">1</span></span> |<span data-ttu-id="5d53e-181">PCM 0</span><span class="sxs-lookup"><span data-stu-id="5d53e-181">PCM 0</span></span> |
   | <span data-ttu-id="5d53e-182">2</span><span class="sxs-lookup"><span data-stu-id="5d53e-182">2</span></span> |<span data-ttu-id="5d53e-183">PCM 1</span><span class="sxs-lookup"><span data-stu-id="5d53e-183">PCM 1</span></span> |
   | <span data-ttu-id="5d53e-184">3</span><span class="sxs-lookup"><span data-stu-id="5d53e-184">3</span></span> |<span data-ttu-id="5d53e-185">Řadič 0</span><span class="sxs-lookup"><span data-stu-id="5d53e-185">Controller 0</span></span> |
   | <span data-ttu-id="5d53e-186">4</span><span class="sxs-lookup"><span data-stu-id="5d53e-186">4</span></span> |<span data-ttu-id="5d53e-187">Řadič 1</span><span class="sxs-lookup"><span data-stu-id="5d53e-187">Controller 1</span></span> |
5. <span data-ttu-id="5d53e-188">Vadný PCM vypnout a odpojit napájecí kabel napájení.</span><span class="sxs-lookup"><span data-stu-id="5d53e-188">Turn off the faulty PCM and disconnect the power supply cord.</span></span> <span data-ttu-id="5d53e-189">Nyní můžete odebrat PCM.</span><span class="sxs-lookup"><span data-stu-id="5d53e-189">You can now remove the PCM.</span></span>
6. <span data-ttu-id="5d53e-190">Pochopit zámek a na straně popisovač PCM mezi Flash a ukazováčkem a vměstnat je dohromady a otevřít popisovač.</span><span class="sxs-lookup"><span data-stu-id="5d53e-190">Grasp the latch and the side of the PCM handle between your thumb and forefinger, and squeeze them together to open the handle.</span></span>
   
    ![Otevírání PCM popisovač](./media/storsimple-power-cooling-module-replacement/IC740995.png)
   
    <span data-ttu-id="5d53e-192">**Obrázek 4** otevírání popisovač PCM</span><span class="sxs-lookup"><span data-stu-id="5d53e-192">**Figure 4** Opening the PCM handle</span></span>
7. <span data-ttu-id="5d53e-193">Uchopitelný popisovač a odeberte PCM.</span><span class="sxs-lookup"><span data-stu-id="5d53e-193">Grip the handle and remove the PCM.</span></span>
   
    ![Odebrání zařízení PCM](./media/storsimple-power-cooling-module-replacement/IC740996.png)
   
    <span data-ttu-id="5d53e-195">**Obrázek 5** odebrání PCM</span><span class="sxs-lookup"><span data-stu-id="5d53e-195">**Figure 5** Removing the PCM</span></span>

## <a name="install-a-replacement-pcm"></a><span data-ttu-id="5d53e-196">Nainstalovat náhradní PCM</span><span class="sxs-lookup"><span data-stu-id="5d53e-196">Install a replacement PCM</span></span>
<span data-ttu-id="5d53e-197">Postupujte podle těchto pokynů k instalaci PCM v zařízení StorSimple.</span><span class="sxs-lookup"><span data-stu-id="5d53e-197">Follow these instructions to install a PCM in your StorSimple device.</span></span> <span data-ttu-id="5d53e-198">Ujistěte se, že jste vložili modul zálohování baterie před instalací nahrazení PCM (týká se pouze 764 W PCMs).</span><span class="sxs-lookup"><span data-stu-id="5d53e-198">Ensure that you have inserted the backup battery module prior to installing the replacement PCM (applies to 764 W PCMs only).</span></span> <span data-ttu-id="5d53e-199">Další informace najdete v tématu Jak [vyjměte a vložte modul zálohování baterie](storsimple-8000-battery-replacement.md).</span><span class="sxs-lookup"><span data-stu-id="5d53e-199">For more information, see how to [remove and insert a backup battery module](storsimple-8000-battery-replacement.md).</span></span>

#### <a name="to-install-a-pcm"></a><span data-ttu-id="5d53e-200">Chcete-li nainstalovat PCM</span><span class="sxs-lookup"><span data-stu-id="5d53e-200">To install a PCM</span></span>
1. <span data-ttu-id="5d53e-201">Ověřte, zda máte správné náhradou PCM tento skříň.</span><span class="sxs-lookup"><span data-stu-id="5d53e-201">Verify that you have the correct replacement PCM for this enclosure.</span></span> <span data-ttu-id="5d53e-202">Primární skříň musí 764 W PCM a skříň EBOD musí 580 W PCM.</span><span class="sxs-lookup"><span data-stu-id="5d53e-202">The primary enclosure needs a 764 W PCM and the EBOD enclosure needs a 580 W PCM.</span></span> <span data-ttu-id="5d53e-203">Byste neměli používat 580 PCM W ve skříni primární nebo 764 PCM W ve skříni EBOD.</span><span class="sxs-lookup"><span data-stu-id="5d53e-203">You should not attempt to use the 580 W PCM in the Primary enclosure, or the 764 W PCM in the EBOD enclosure.</span></span> <span data-ttu-id="5d53e-204">Následující obrázek ukazuje, kde k identifikaci informací na štítek, který je opatřit PCM.</span><span class="sxs-lookup"><span data-stu-id="5d53e-204">The following image shows where to identify this information on the label that is affixed to the PCM.</span></span>
   
    ![Popisek PCM zařízení](./media/storsimple-power-cooling-module-replacement/IC740973.png)
   
    <span data-ttu-id="5d53e-206">**Obrázek 6** PCM popisek</span><span class="sxs-lookup"><span data-stu-id="5d53e-206">**Figure 6** PCM label</span></span>
2. <span data-ttu-id="5d53e-207">Zkontrolujte, zda škody skříň, věnujte zvláštní pozornost konektory.</span><span class="sxs-lookup"><span data-stu-id="5d53e-207">Check for damage to the enclosure, paying particular attention to the connectors.</span></span> 
   
   > [!NOTE]
   > <span data-ttu-id="5d53e-208">**Pokud jsou všechny kódy PIN konektor ohnuty není nainstalovaný modul.**</span><span class="sxs-lookup"><span data-stu-id="5d53e-208">**Do not install the module if any connector pins are bent.**</span></span>
   > 
   > 
3. <span data-ttu-id="5d53e-209">S popisovačem PCM v otevřené poloze posuňte modul do skříni.</span><span class="sxs-lookup"><span data-stu-id="5d53e-209">With the PCM handle in the open position, slide the module into the enclosure.</span></span>
   
    ![Instalace zařízení PCM](./media/storsimple-power-cooling-module-replacement/IC740975.png)
   
    <span data-ttu-id="5d53e-211">**Obrázek 7** instalaci PCM</span><span class="sxs-lookup"><span data-stu-id="5d53e-211">**Figure 7** Installing the PCM</span></span>
4. <span data-ttu-id="5d53e-212">Ruční zavření PCM popisovač.</span><span class="sxs-lookup"><span data-stu-id="5d53e-212">Manually close the PCM handle.</span></span> <span data-ttu-id="5d53e-213">A klikněte měli vědět, jak zapojí západky popisovač.</span><span class="sxs-lookup"><span data-stu-id="5d53e-213">You should hear a click as the handle latch engages.</span></span>
   
   > [!NOTE]
   > <span data-ttu-id="5d53e-214">Aby se zajistilo, že konektor PIN mít pověření, můžete můžete jemně tug na popisovač bez uvolnění zámek.</span><span class="sxs-lookup"><span data-stu-id="5d53e-214">To ensure that the connector pins have engaged, you can gently tug on the handle without releasing the latch.</span></span> <span data-ttu-id="5d53e-215">Pokud PCM snímky, znamená to, že zámek se zavřel před konektory pověření.</span><span class="sxs-lookup"><span data-stu-id="5d53e-215">If the PCM slides out, it implies that the latch was closed before the connectors engaged.</span></span>
   
5. <span data-ttu-id="5d53e-216">Připojte kabely power ke zdroji napájení a PCM.</span><span class="sxs-lookup"><span data-stu-id="5d53e-216">Connect the power cables to the power source and to the PCM.</span></span>
6. <span data-ttu-id="5d53e-217">Zabezpečte kmen balících pomoc.</span><span class="sxs-lookup"><span data-stu-id="5d53e-217">Secure the strain relief bales.</span></span>
7. <span data-ttu-id="5d53e-218">Zapněte PCM.</span><span class="sxs-lookup"><span data-stu-id="5d53e-218">Turn on the PCM.</span></span>
8. <span data-ttu-id="5d53e-219">Ověřit, pokud chcete nahrazení byla úspěšná: služby StorSimple Manager zařízení na portálu Azure, přejděte na zařízení a pak na **Nastavení > Sledování > Stav hardwaru**.</span><span class="sxs-lookup"><span data-stu-id="5d53e-219">Verify that the replacement was successful: in the Azure portal of your StorSimple Device Manager service, navigate to your device and then to **Settings > Monitor > Hardware health**.</span></span> <span data-ttu-id="5d53e-220">V části **sdílené součásti**, by měly být stav PCM.</span><span class="sxs-lookup"><span data-stu-id="5d53e-220">Under the **Shared components**, the status of the PCM should be green.</span></span>
   
   > [!NOTE]
   > <span data-ttu-id="5d53e-221">Ho může trvat několik minut nahrazení PCM úplně inicializovat.</span><span class="sxs-lookup"><span data-stu-id="5d53e-221">It may take a few minutes for the replacement PCM to completely initialize.</span></span>

## <a name="next-steps"></a><span data-ttu-id="5d53e-222">Další kroky</span><span class="sxs-lookup"><span data-stu-id="5d53e-222">Next steps</span></span>
<span data-ttu-id="5d53e-223">Další informace o [StorSimple hardwarové součásti nahrazení](storsimple-8000-hardware-component-replacement.md).</span><span class="sxs-lookup"><span data-stu-id="5d53e-223">Learn more about [StorSimple hardware component replacement](storsimple-8000-hardware-component-replacement.md).</span></span>

