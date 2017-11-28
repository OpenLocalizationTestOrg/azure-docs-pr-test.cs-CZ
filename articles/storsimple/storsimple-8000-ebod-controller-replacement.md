---
title: "aaaReplace řadič StorSimple 8600 EBOD | Microsoft Docs"
description: "Vysvětluje, jak tooremove a nahraďte jeden nebo oba řadiče EBOD na zařízení StorSimple 8600."
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
ms.openlocfilehash: 8343ed6f48ae97fc9204452f85e1936bfb1d6919
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="replace-an-ebod-controller-on-your-storsimple-device"></a><span data-ttu-id="51feb-103">Nahraďte řadič EBOD zařízení StorSimple</span><span class="sxs-lookup"><span data-stu-id="51feb-103">Replace an EBOD controller on your StorSimple device</span></span>

## <a name="overview"></a><span data-ttu-id="51feb-104">Přehled</span><span class="sxs-lookup"><span data-stu-id="51feb-104">Overview</span></span>
<span data-ttu-id="51feb-105">Tento kurz vysvětluje, jak tooreplace modul vadný řadič EBOD na zařízení s Microsoft Azure StorSimple.</span><span class="sxs-lookup"><span data-stu-id="51feb-105">This tutorial explains how tooreplace a faulty EBOD controller module on your Microsoft Azure StorSimple device.</span></span> <span data-ttu-id="51feb-106">tooreplace modul EBOD řadiče, budete muset:</span><span class="sxs-lookup"><span data-stu-id="51feb-106">tooreplace an EBOD controller module, you need to:</span></span>

* <span data-ttu-id="51feb-107">Odebrání hello vadný EBOD řadiče</span><span class="sxs-lookup"><span data-stu-id="51feb-107">Remove hello faulty EBOD controller</span></span>
* <span data-ttu-id="51feb-108">Nainstalovat nový řadič EBOD</span><span class="sxs-lookup"><span data-stu-id="51feb-108">Install a new EBOD controller</span></span>

<span data-ttu-id="51feb-109">Vezměte v úvahu hello před zahájením následující informace:</span><span class="sxs-lookup"><span data-stu-id="51feb-109">Consider hello following information before you begin:</span></span>

* <span data-ttu-id="51feb-110">Prázdné EBOD moduly musí být vložený do všechny nepoužívané sloty.</span><span class="sxs-lookup"><span data-stu-id="51feb-110">Blank EBOD modules must be inserted into all unused slots.</span></span> <span data-ttu-id="51feb-111">Skříň Hello nebude cool správně, pokud slotu je ponechány otevřené.</span><span class="sxs-lookup"><span data-stu-id="51feb-111">hello enclosure will not cool properly if a slot is left open.</span></span>
* <span data-ttu-id="51feb-112">je řadič EBOD Hello za provozu a můžete ho odebrat nebo nahradit.</span><span class="sxs-lookup"><span data-stu-id="51feb-112">hello EBOD controller is hot-swappable and can be removed or replaced.</span></span> <span data-ttu-id="51feb-113">Neodebírejte selhání modulu, dokud nebudete mít nahrazení.</span><span class="sxs-lookup"><span data-stu-id="51feb-113">Do not remove a failed module until you have a replacement.</span></span> <span data-ttu-id="51feb-114">Když zahájíte proces nahrazení hello, musíte ji dokončíte během 10 minut.</span><span class="sxs-lookup"><span data-stu-id="51feb-114">When you initiate hello replacement process, you must finish it within 10 minutes.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="51feb-115">Před pokusem o tooremove nebo nahradit všechny součásti, StorSimple, ujistěte se, abyste si prošli hello [zabezpečení ikonu konvence](storsimple-safety.md#safety-icon-conventions) a dalších [bezpečnostní opatření](storsimple-safety.md).</span><span class="sxs-lookup"><span data-stu-id="51feb-115">Before attempting tooremove or replace any StorSimple component, make sure that you review hello [safety icon conventions](storsimple-safety.md#safety-icon-conventions) and other [safety precautions](storsimple-safety.md).</span></span>

## <a name="remove-an-ebod-controller"></a><span data-ttu-id="51feb-116">Odebrat řadič EBOD</span><span class="sxs-lookup"><span data-stu-id="51feb-116">Remove an EBOD controller</span></span>
<span data-ttu-id="51feb-117">Před nahrazení hello chyba modulu EBOD řadiče v zařízení StorSimple, ujistěte se, že hello jiné řadiče modul EBOD je aktivní a spuštěná.</span><span class="sxs-lookup"><span data-stu-id="51feb-117">Before replacing hello failed EBOD controller module in your StorSimple device, make sure that hello other EBOD controller module is active and running.</span></span> <span data-ttu-id="51feb-118">Hello následující postup a tabulka vysvětlují, jak tooremove hello EBOD řadiče modulu.</span><span class="sxs-lookup"><span data-stu-id="51feb-118">hello following procedure and table explain how tooremove hello EBOD controller module.</span></span>

#### <a name="tooremove-an-ebod-module"></a><span data-ttu-id="51feb-119">tooremove modul EBOD</span><span class="sxs-lookup"><span data-stu-id="51feb-119">tooremove an EBOD module</span></span>
1. <span data-ttu-id="51feb-120">Otevřete hello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="51feb-120">Open hello Azure portal.</span></span>
2. <span data-ttu-id="51feb-121">Přejděte tooyour zařízení a přejděte příliš**nastavení** > **stavu hardwaru**a ověřte stav hello hello DIODU hello active EBOD řadiče modul je zelená a hello DIODU pro hello se nezdařilo Modul řadiče EBOD je red.</span><span class="sxs-lookup"><span data-stu-id="51feb-121">Go tooyour device and navigate too**Settings** > **Hardware health**, and verify that hello status of hello LED for hello active EBOD controller module is green and hello LED for hello failed EBOD controller module is red.</span></span>
3. <span data-ttu-id="51feb-122">Vyhledejte modul řadiče EBOD hello se nezdařila v hello zadní hello zařízení.</span><span class="sxs-lookup"><span data-stu-id="51feb-122">Locate hello failed EBOD controller module at hello back of hello device.</span></span>
4. <span data-ttu-id="51feb-123">Odeberte hello kabely, které připojují hello EBOD řadiče modulu toohello řadiče před přepnutím hello EBOD modulu mimo hello systému.</span><span class="sxs-lookup"><span data-stu-id="51feb-123">Remove hello cables that connect hello EBOD controller module toohello controller before taking hello EBOD module out of hello system.</span></span>
5. <span data-ttu-id="51feb-124">Poznamenejte si hello přesný SAS portu hello EBOD řadiče modul, který není připojený toohello řadiče.</span><span class="sxs-lookup"><span data-stu-id="51feb-124">Make a note of hello exact SAS port of hello EBOD controller module that was connected toohello controller.</span></span> <span data-ttu-id="51feb-125">Po nahrazení hello EBOD modulu, bude se konfigurace toothis systému požadované toorestore hello.</span><span class="sxs-lookup"><span data-stu-id="51feb-125">You will be required toorestore hello system toothis configuration after you replace hello EBOD module.</span></span>
   
   > [!NOTE]
   > <span data-ttu-id="51feb-126">Obvykle to bude Port A, který je označený jako **hostitele v** v hello následující diagram.</span><span class="sxs-lookup"><span data-stu-id="51feb-126">Typically, this will be Port A, which is labeled as **Host in** in hello following diagram.</span></span>
   
    ![Propojovací rozhraní EBOD řadiče](./media/storsimple-ebod-controller-replacement/IC741049.png)
   
     <span data-ttu-id="51feb-128">**Obrázek 1** zpět of EBOD modulu</span><span class="sxs-lookup"><span data-stu-id="51feb-128">**Figure 1** Back of EBOD module</span></span>
   
   | <span data-ttu-id="51feb-129">Štítek</span><span class="sxs-lookup"><span data-stu-id="51feb-129">Label</span></span> | <span data-ttu-id="51feb-130">Popis</span><span class="sxs-lookup"><span data-stu-id="51feb-130">Description</span></span> |
   |:--- |:--- |
   | <span data-ttu-id="51feb-131">1</span><span class="sxs-lookup"><span data-stu-id="51feb-131">1</span></span> |<span data-ttu-id="51feb-132">Selhání DIODU</span><span class="sxs-lookup"><span data-stu-id="51feb-132">Fault LED</span></span> |
   | <span data-ttu-id="51feb-133">2</span><span class="sxs-lookup"><span data-stu-id="51feb-133">2</span></span> |<span data-ttu-id="51feb-134">Napájení DIODU</span><span class="sxs-lookup"><span data-stu-id="51feb-134">Power LED</span></span> |
   | <span data-ttu-id="51feb-135">3</span><span class="sxs-lookup"><span data-stu-id="51feb-135">3</span></span> |<span data-ttu-id="51feb-136">SAS konektory</span><span class="sxs-lookup"><span data-stu-id="51feb-136">SAS connectors</span></span> |
   | <span data-ttu-id="51feb-137">4</span><span class="sxs-lookup"><span data-stu-id="51feb-137">4</span></span> |<span data-ttu-id="51feb-138">LED SAS</span><span class="sxs-lookup"><span data-stu-id="51feb-138">SAS LEDs</span></span> |
   | <span data-ttu-id="51feb-139">5</span><span class="sxs-lookup"><span data-stu-id="51feb-139">5</span></span> |<span data-ttu-id="51feb-140">Sériové porty pouze pro objekt pro vytváření</span><span class="sxs-lookup"><span data-stu-id="51feb-140">Serial ports for factory use only</span></span> |
   | <span data-ttu-id="51feb-141">6</span><span class="sxs-lookup"><span data-stu-id="51feb-141">6</span></span> |<span data-ttu-id="51feb-142">Port (hostitel v)</span><span class="sxs-lookup"><span data-stu-id="51feb-142">Port A (Host in)</span></span> |
   | <span data-ttu-id="51feb-143">7</span><span class="sxs-lookup"><span data-stu-id="51feb-143">7</span></span> |<span data-ttu-id="51feb-144">Port B (hostitel out)</span><span class="sxs-lookup"><span data-stu-id="51feb-144">Port B (Host out)</span></span> |
   | <span data-ttu-id="51feb-145">8</span><span class="sxs-lookup"><span data-stu-id="51feb-145">8</span></span> |<span data-ttu-id="51feb-146">Port C (pouze pro objekt pro vytváření použití)</span><span class="sxs-lookup"><span data-stu-id="51feb-146">Port C (Factory use only)</span></span> |

## <a name="install-a-new-ebod-controller"></a><span data-ttu-id="51feb-147">Nainstalovat nový řadič EBOD</span><span class="sxs-lookup"><span data-stu-id="51feb-147">Install a new EBOD controller</span></span>
<span data-ttu-id="51feb-148">Hello následující postup a tabulka vysvětlují, jak tooinstall modul EBOD řadiče v zařízení StorSimple.</span><span class="sxs-lookup"><span data-stu-id="51feb-148">hello following procedure and table explain how tooinstall an EBOD controller module in your StorSimple device.</span></span>

#### <a name="tooinstall-an-ebod-controller"></a><span data-ttu-id="51feb-149">tooinstall řadič EBOD</span><span class="sxs-lookup"><span data-stu-id="51feb-149">tooinstall an EBOD controller</span></span>
1. <span data-ttu-id="51feb-150">Zkontrolujte zařízení EBOD hello škody, zejména toohello rozhraní konektoru.</span><span class="sxs-lookup"><span data-stu-id="51feb-150">Check hello EBOD device for damage, especially toohello interface connector.</span></span> <span data-ttu-id="51feb-151">Neinstalujte nový řadič EBOD hello, pokud jsou ohnuty všechny kódy PIN.</span><span class="sxs-lookup"><span data-stu-id="51feb-151">Do not install hello new EBOD controller if any pins are bent.</span></span>
2. <span data-ttu-id="51feb-152">S hello zámky v hello otevřete pozici, modul hello snímku hello skříň, dokud zámky hello zapojení.</span><span class="sxs-lookup"><span data-stu-id="51feb-152">With hello latches in hello open position, slide hello module into hello enclosure until hello latches engage.</span></span>
   
    ![Instalaci EBOD řadiče](./media/storsimple-ebod-controller-replacement/IC741050.png)
   
    <span data-ttu-id="51feb-154">**Obrázek 2** instalace hello EBOD řadiče modulu</span><span class="sxs-lookup"><span data-stu-id="51feb-154">**Figure 2**  Installing hello EBOD controller module</span></span>
3. <span data-ttu-id="51feb-155">Zavřít hello západky.</span><span class="sxs-lookup"><span data-stu-id="51feb-155">Close hello latch.</span></span> <span data-ttu-id="51feb-156">Klikněte na tlačítko by měla být slyšet jako zapojí západky hello.</span><span class="sxs-lookup"><span data-stu-id="51feb-156">You should hear a click as hello latch engages.</span></span>
   
    ![Uvolnění EBOD západky](./media/storsimple-ebod-controller-replacement/IC741047.png)
   
    <span data-ttu-id="51feb-158">**Obrázek 3** zavření hello EBOD modulu západky</span><span class="sxs-lookup"><span data-stu-id="51feb-158">**Figure 3**  Closing hello EBOD module latch</span></span>
4. <span data-ttu-id="51feb-159">Znovu připojte kabely hello.</span><span class="sxs-lookup"><span data-stu-id="51feb-159">Reconnect hello cables.</span></span> <span data-ttu-id="51feb-160">Použijte přesnou konfiguraci hello, který se nachází před hello nahrazení.</span><span class="sxs-lookup"><span data-stu-id="51feb-160">Use hello exact configuration that was present before hello replacement.</span></span> <span data-ttu-id="51feb-161">Viz následující diagram hello a tabulky Podrobnosti o tom, kabely tooconnect hello.</span><span class="sxs-lookup"><span data-stu-id="51feb-161">See hello following diagram and table for details about how tooconnect hello cables.</span></span>
   
    ![Zapojení kabeláže zařízení 4U pro napájení](./media/storsimple-ebod-controller-replacement/IC770723.png)
   
    <span data-ttu-id="51feb-163">**Obrázek 4**.</span><span class="sxs-lookup"><span data-stu-id="51feb-163">**Figure 4**.</span></span> <span data-ttu-id="51feb-164">Opětovné připojení kabely</span><span class="sxs-lookup"><span data-stu-id="51feb-164">Reconnecting cables</span></span>
   
   | <span data-ttu-id="51feb-165">Štítek</span><span class="sxs-lookup"><span data-stu-id="51feb-165">Label</span></span> | <span data-ttu-id="51feb-166">Popis</span><span class="sxs-lookup"><span data-stu-id="51feb-166">Description</span></span> |
   |:--- |:--- |
   | <span data-ttu-id="51feb-167">1</span><span class="sxs-lookup"><span data-stu-id="51feb-167">1</span></span> |<span data-ttu-id="51feb-168">Primární skříň</span><span class="sxs-lookup"><span data-stu-id="51feb-168">Primary enclosure</span></span> |
   | <span data-ttu-id="51feb-169">2</span><span class="sxs-lookup"><span data-stu-id="51feb-169">2</span></span> |<span data-ttu-id="51feb-170">PCM 0</span><span class="sxs-lookup"><span data-stu-id="51feb-170">PCM 0</span></span> |
   | <span data-ttu-id="51feb-171">3</span><span class="sxs-lookup"><span data-stu-id="51feb-171">3</span></span> |<span data-ttu-id="51feb-172">PCM 1</span><span class="sxs-lookup"><span data-stu-id="51feb-172">PCM 1</span></span> |
   | <span data-ttu-id="51feb-173">4</span><span class="sxs-lookup"><span data-stu-id="51feb-173">4</span></span> |<span data-ttu-id="51feb-174">Řadič 0</span><span class="sxs-lookup"><span data-stu-id="51feb-174">Controller 0</span></span> |
   | <span data-ttu-id="51feb-175">5</span><span class="sxs-lookup"><span data-stu-id="51feb-175">5</span></span> |<span data-ttu-id="51feb-176">Řadič 1</span><span class="sxs-lookup"><span data-stu-id="51feb-176">Controller 1</span></span> |
   | <span data-ttu-id="51feb-177">6</span><span class="sxs-lookup"><span data-stu-id="51feb-177">6</span></span> |<span data-ttu-id="51feb-178">EBOD řadič 0</span><span class="sxs-lookup"><span data-stu-id="51feb-178">EBOD controller 0</span></span> |
   | <span data-ttu-id="51feb-179">7</span><span class="sxs-lookup"><span data-stu-id="51feb-179">7</span></span> |<span data-ttu-id="51feb-180">EBOD řadiči 1</span><span class="sxs-lookup"><span data-stu-id="51feb-180">EBOD controller 1</span></span> |
   | <span data-ttu-id="51feb-181">8</span><span class="sxs-lookup"><span data-stu-id="51feb-181">8</span></span> |<span data-ttu-id="51feb-182">EBOD skříň</span><span class="sxs-lookup"><span data-stu-id="51feb-182">EBOD enclosure</span></span> |
   | <span data-ttu-id="51feb-183">9</span><span class="sxs-lookup"><span data-stu-id="51feb-183">9</span></span> |<span data-ttu-id="51feb-184">Jednotek pro distribuci napájení</span><span class="sxs-lookup"><span data-stu-id="51feb-184">Power Distribution Units</span></span> |

## <a name="next-steps"></a><span data-ttu-id="51feb-185">Další kroky</span><span class="sxs-lookup"><span data-stu-id="51feb-185">Next steps</span></span>
<span data-ttu-id="51feb-186">Další informace o [StorSimple hardwarové součásti nahrazení](storsimple-8000-hardware-component-replacement.md).</span><span class="sxs-lookup"><span data-stu-id="51feb-186">Learn more about [StorSimple hardware component replacement](storsimple-8000-hardware-component-replacement.md).</span></span>

