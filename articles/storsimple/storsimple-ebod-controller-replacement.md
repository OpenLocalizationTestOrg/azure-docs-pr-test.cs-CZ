---
title: "Nahraďte řadič StorSimple EBOD | Microsoft Docs"
description: "Vysvětluje, jak odeberete a nahradíte jeden nebo oba řadiče EBOD na zařízení StorSimple 8600."
services: storsimple
documentationcenter: 
author: alkohli
manager: carmonm
editor: 
ms.assetid: 8cbfa507-1a56-4e24-99dd-7db9abd3b850
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 08/17/2016
ms.author: alkohli
ms.openlocfilehash: 23d819ddc3bbcbaf2847cdcc9191407ead0ff43d
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="replace-an-ebod-controller-on-your-storsimple-device"></a><span data-ttu-id="24457-103">Nahraďte řadič EBOD zařízení StorSimple</span><span class="sxs-lookup"><span data-stu-id="24457-103">Replace an EBOD controller on your StorSimple device</span></span>
## <a name="overview"></a><span data-ttu-id="24457-104">Přehled</span><span class="sxs-lookup"><span data-stu-id="24457-104">Overview</span></span>
<span data-ttu-id="24457-105">Tento kurz vysvětluje, jak nahradit vadný modul řadiče EBOD na zařízení s Microsoft Azure StorSimple.</span><span class="sxs-lookup"><span data-stu-id="24457-105">This tutorial explains how to replace a faulty EBOD controller module on your Microsoft Azure StorSimple device.</span></span> <span data-ttu-id="24457-106">Chcete-li nahradit modul EBOD řadiče, je potřeba:</span><span class="sxs-lookup"><span data-stu-id="24457-106">To replace an EBOD controller module, you need to:</span></span>

* <span data-ttu-id="24457-107">Odebrat vadný řadič EBOD</span><span class="sxs-lookup"><span data-stu-id="24457-107">Remove the faulty EBOD controller</span></span>
* <span data-ttu-id="24457-108">Nainstalovat nový řadič EBOD</span><span class="sxs-lookup"><span data-stu-id="24457-108">Install a new EBOD controller</span></span>

<span data-ttu-id="24457-109">Před zahájením vezměte v úvahu následující informace:</span><span class="sxs-lookup"><span data-stu-id="24457-109">Consider the following information before you begin:</span></span>

* <span data-ttu-id="24457-110">Prázdné EBOD moduly musí být vložený do všechny nepoužívané sloty.</span><span class="sxs-lookup"><span data-stu-id="24457-110">Blank EBOD modules must be inserted into all unused slots.</span></span> <span data-ttu-id="24457-111">Skříni nebude cool správně, pokud slotu je ponechány otevřené.</span><span class="sxs-lookup"><span data-stu-id="24457-111">The enclosure will not cool properly if a slot is left open.</span></span>
* <span data-ttu-id="24457-112">Řadič EBOD je za provozu a můžete ho odebrat nebo nahradit.</span><span class="sxs-lookup"><span data-stu-id="24457-112">The EBOD controller is hot-swappable and can be removed or replaced.</span></span> <span data-ttu-id="24457-113">Neodebírejte selhání modulu, dokud nebudete mít nahrazení.</span><span class="sxs-lookup"><span data-stu-id="24457-113">Do not remove a failed module until you have a replacement.</span></span> <span data-ttu-id="24457-114">Když zahájíte proces nahrazení, musí ji dokončíte během 10 minut.</span><span class="sxs-lookup"><span data-stu-id="24457-114">When you initiate the replacement process, you must finish it within 10 minutes.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="24457-115">Před pokusem o odeberte nebo nahraďte všechny součásti, StorSimple, ujistěte se, abyste si prošli [zabezpečení ikonu konvence](storsimple-safety.md#safety-icon-conventions) a dalších [bezpečnostní opatření](storsimple-safety.md).</span><span class="sxs-lookup"><span data-stu-id="24457-115">Before attempting to remove or replace any StorSimple component, make sure that you review the [safety icon conventions](storsimple-safety.md#safety-icon-conventions) and other [safety precautions](storsimple-safety.md).</span></span>
> 
> 

## <a name="remove-an-ebod-controller"></a><span data-ttu-id="24457-116">Odebrat řadič EBOD</span><span class="sxs-lookup"><span data-stu-id="24457-116">Remove an EBOD controller</span></span>
<span data-ttu-id="24457-117">Před výměnou selhání modulu EBOD řadiče v zařízení StorSimple, ujistěte se, zda jiné řadiče modul EBOD je aktivní a spuštěná.</span><span class="sxs-lookup"><span data-stu-id="24457-117">Before replacing the failed EBOD controller module in your StorSimple device, make sure that the other EBOD controller module is active and running.</span></span> <span data-ttu-id="24457-118">Následující postup a tabulka vysvětlují, jak odebrat modul EBOD řadiče.</span><span class="sxs-lookup"><span data-stu-id="24457-118">The following procedure and table explain how to remove the EBOD controller module.</span></span>

#### <a name="to-remove-an-ebod-module"></a><span data-ttu-id="24457-119">Chcete-li odebrat modul EBOD</span><span class="sxs-lookup"><span data-stu-id="24457-119">To remove an EBOD module</span></span>
1. <span data-ttu-id="24457-120">Otevřete portál Azure classic.</span><span class="sxs-lookup"><span data-stu-id="24457-120">Open the Azure classic portal.</span></span>
2. <span data-ttu-id="24457-121">Přejděte na **zařízení** > **údržby** > **stavu hardwaru**a ověřte, že stav DIODU active EBOD kontroleru modul je zobrazen zeleně a je red LED selhání modulu EBOD řadiče.</span><span class="sxs-lookup"><span data-stu-id="24457-121">Navigate to **Devices** > **Maintenance** > **Hardware Status**, and verify that the status of the LED for the active EBOD controller module is green and the LED for the failed EBOD controller module is red.</span></span>
3. <span data-ttu-id="24457-122">Vyhledejte modul selhání řadiče EBOD zadní straně zařízení.</span><span class="sxs-lookup"><span data-stu-id="24457-122">Locate the failed EBOD controller module at the back of the device.</span></span>
4. <span data-ttu-id="24457-123">Odeberte kabely, které modul řadiče EBOD připojení k řadiči před přepnutím modul EBOD mimo systém.</span><span class="sxs-lookup"><span data-stu-id="24457-123">Remove the cables that connect the EBOD controller module to the controller before taking the EBOD module out of the system.</span></span>
5. <span data-ttu-id="24457-124">Poznamenejte si přesný SAS portu EBOD řadiče modul, který byl připojený k řadiči.</span><span class="sxs-lookup"><span data-stu-id="24457-124">Make a note of the exact SAS port of the EBOD controller module that was connected to the controller.</span></span> <span data-ttu-id="24457-125">Budete muset po replace modul EBOD obnovení systému do této konfigurace.</span><span class="sxs-lookup"><span data-stu-id="24457-125">You will be required to restore the system to this configuration after you replace the EBOD module.</span></span> 
   
   > [!NOTE]
   > <span data-ttu-id="24457-126">Obvykle to bude Port A, který je označený jako **hostitele v** v následujícím diagramu.</span><span class="sxs-lookup"><span data-stu-id="24457-126">Typically, this will be Port A, which is labeled as **Host in** in the following diagram.</span></span>
   > 
   > 
   
    ![Propojovací rozhraní EBOD řadiče](./media/storsimple-ebod-controller-replacement/IC741049.png)
   
     <span data-ttu-id="24457-128">**Obrázek 1** zpět of EBOD modulu</span><span class="sxs-lookup"><span data-stu-id="24457-128">**Figure 1** Back of EBOD module</span></span>
   
   | <span data-ttu-id="24457-129">Štítek</span><span class="sxs-lookup"><span data-stu-id="24457-129">Label</span></span> | <span data-ttu-id="24457-130">Popis</span><span class="sxs-lookup"><span data-stu-id="24457-130">Description</span></span> |
   |:--- |:--- |
   | <span data-ttu-id="24457-131">1</span><span class="sxs-lookup"><span data-stu-id="24457-131">1</span></span> |<span data-ttu-id="24457-132">Selhání DIODU</span><span class="sxs-lookup"><span data-stu-id="24457-132">Fault LED</span></span> |
   | <span data-ttu-id="24457-133">2</span><span class="sxs-lookup"><span data-stu-id="24457-133">2</span></span> |<span data-ttu-id="24457-134">Napájení DIODU</span><span class="sxs-lookup"><span data-stu-id="24457-134">Power LED</span></span> |
   | <span data-ttu-id="24457-135">3</span><span class="sxs-lookup"><span data-stu-id="24457-135">3</span></span> |<span data-ttu-id="24457-136">SAS konektory</span><span class="sxs-lookup"><span data-stu-id="24457-136">SAS connectors</span></span> |
   | <span data-ttu-id="24457-137">4</span><span class="sxs-lookup"><span data-stu-id="24457-137">4</span></span> |<span data-ttu-id="24457-138">LED SAS</span><span class="sxs-lookup"><span data-stu-id="24457-138">SAS LEDs</span></span> |
   | <span data-ttu-id="24457-139">5</span><span class="sxs-lookup"><span data-stu-id="24457-139">5</span></span> |<span data-ttu-id="24457-140">Sériové porty pouze pro objekt pro vytváření</span><span class="sxs-lookup"><span data-stu-id="24457-140">Serial ports for factory use only</span></span> |
   | <span data-ttu-id="24457-141">6</span><span class="sxs-lookup"><span data-stu-id="24457-141">6</span></span> |<span data-ttu-id="24457-142">Port (hostitel v)</span><span class="sxs-lookup"><span data-stu-id="24457-142">Port A (Host in)</span></span> |
   | <span data-ttu-id="24457-143">7</span><span class="sxs-lookup"><span data-stu-id="24457-143">7</span></span> |<span data-ttu-id="24457-144">Port B (hostitel out)</span><span class="sxs-lookup"><span data-stu-id="24457-144">Port B (Host out)</span></span> |
   | <span data-ttu-id="24457-145">8</span><span class="sxs-lookup"><span data-stu-id="24457-145">8</span></span> |<span data-ttu-id="24457-146">Port C (pouze pro objekt pro vytváření použití)</span><span class="sxs-lookup"><span data-stu-id="24457-146">Port C (Factory use only)</span></span> |

## <a name="install-a-new-ebod-controller"></a><span data-ttu-id="24457-147">Nainstalovat nový řadič EBOD</span><span class="sxs-lookup"><span data-stu-id="24457-147">Install a new EBOD controller</span></span>
<span data-ttu-id="24457-148">Následující postup a tabulka vysvětlují, jak nainstalovat modul EBOD řadiče v zařízení StorSimple.</span><span class="sxs-lookup"><span data-stu-id="24457-148">The following procedure and table explain how to install an EBOD controller module in your StorSimple device.</span></span>

#### <a name="to-install-an-ebod-controller"></a><span data-ttu-id="24457-149">Abyste mohli nainstalovat řadič EBOD</span><span class="sxs-lookup"><span data-stu-id="24457-149">To install an EBOD controller</span></span>
1. <span data-ttu-id="24457-150">Zkontrolujte zařízení EBOD škody, zejména konektor rozhraní.</span><span class="sxs-lookup"><span data-stu-id="24457-150">Check the EBOD device for damage, especially to the interface connector.</span></span> <span data-ttu-id="24457-151">Neinstalujte nový řadič EBOD, pokud jsou ohnuty všechny kódy PIN.</span><span class="sxs-lookup"><span data-stu-id="24457-151">Do not install the new EBOD controller if any pins are bent.</span></span>
2. <span data-ttu-id="24457-152">S zámky v otevřené poloze posuňte modul do skříni, dokud západek zapojení.</span><span class="sxs-lookup"><span data-stu-id="24457-152">With the latches in the open position, slide the module into the enclosure until the latches engage.</span></span>
   
    ![Instalaci EBOD řadiče](./media/storsimple-ebod-controller-replacement/IC741050.png)
   
    <span data-ttu-id="24457-154">**Obrázek 2** instalaci modulu EBOD řadiče</span><span class="sxs-lookup"><span data-stu-id="24457-154">**Figure 2**  Installing the EBOD controller module</span></span>
3. <span data-ttu-id="24457-155">Zavřete zámek.</span><span class="sxs-lookup"><span data-stu-id="24457-155">Close the latch.</span></span> <span data-ttu-id="24457-156">Klikněte na měli vědět, jak zapojí zámek.</span><span class="sxs-lookup"><span data-stu-id="24457-156">You should hear a click as the latch engages.</span></span>
   
    ![Uvolnění EBOD západky](./media/storsimple-ebod-controller-replacement/IC741047.png)
   
    <span data-ttu-id="24457-158">**Obrázek 3** zavření západky modulu EBOD</span><span class="sxs-lookup"><span data-stu-id="24457-158">**Figure 3**  Closing the EBOD module latch</span></span>
4. <span data-ttu-id="24457-159">Znovu připojte kabely.</span><span class="sxs-lookup"><span data-stu-id="24457-159">Reconnect the cables.</span></span> <span data-ttu-id="24457-160">Použijte přesnou konfiguraci, která se nachází před nahrazení.</span><span class="sxs-lookup"><span data-stu-id="24457-160">Use the exact configuration that was present before the replacement.</span></span> <span data-ttu-id="24457-161">Viz následující diagram a tabulka Podrobnosti o tom, jak připojte kabely.</span><span class="sxs-lookup"><span data-stu-id="24457-161">See the following diagram and table for details about how to connect the cables.</span></span>
   
    ![Zapojení kabeláže zařízení 4U pro napájení](./media/storsimple-ebod-controller-replacement/IC770723.png)
   
    <span data-ttu-id="24457-163">**Obrázek 4**.</span><span class="sxs-lookup"><span data-stu-id="24457-163">**Figure 4**.</span></span> <span data-ttu-id="24457-164">Opětovné připojení kabely</span><span class="sxs-lookup"><span data-stu-id="24457-164">Reconnecting cables</span></span>
   
   | <span data-ttu-id="24457-165">Štítek</span><span class="sxs-lookup"><span data-stu-id="24457-165">Label</span></span> | <span data-ttu-id="24457-166">Popis</span><span class="sxs-lookup"><span data-stu-id="24457-166">Description</span></span> |
   |:--- |:--- |
   | <span data-ttu-id="24457-167">1</span><span class="sxs-lookup"><span data-stu-id="24457-167">1</span></span> |<span data-ttu-id="24457-168">Primární skříň</span><span class="sxs-lookup"><span data-stu-id="24457-168">Primary enclosure</span></span> |
   | <span data-ttu-id="24457-169">2</span><span class="sxs-lookup"><span data-stu-id="24457-169">2</span></span> |<span data-ttu-id="24457-170">PCM 0</span><span class="sxs-lookup"><span data-stu-id="24457-170">PCM 0</span></span> |
   | <span data-ttu-id="24457-171">3</span><span class="sxs-lookup"><span data-stu-id="24457-171">3</span></span> |<span data-ttu-id="24457-172">PCM 1</span><span class="sxs-lookup"><span data-stu-id="24457-172">PCM 1</span></span> |
   | <span data-ttu-id="24457-173">4</span><span class="sxs-lookup"><span data-stu-id="24457-173">4</span></span> |<span data-ttu-id="24457-174">Řadič 0</span><span class="sxs-lookup"><span data-stu-id="24457-174">Controller 0</span></span> |
   | <span data-ttu-id="24457-175">5</span><span class="sxs-lookup"><span data-stu-id="24457-175">5</span></span> |<span data-ttu-id="24457-176">Řadič 1</span><span class="sxs-lookup"><span data-stu-id="24457-176">Controller 1</span></span> |
   | <span data-ttu-id="24457-177">6</span><span class="sxs-lookup"><span data-stu-id="24457-177">6</span></span> |<span data-ttu-id="24457-178">EBOD řadič 0</span><span class="sxs-lookup"><span data-stu-id="24457-178">EBOD controller 0</span></span> |
   | <span data-ttu-id="24457-179">7</span><span class="sxs-lookup"><span data-stu-id="24457-179">7</span></span> |<span data-ttu-id="24457-180">EBOD řadiči 1</span><span class="sxs-lookup"><span data-stu-id="24457-180">EBOD controller 1</span></span> |
   | <span data-ttu-id="24457-181">8</span><span class="sxs-lookup"><span data-stu-id="24457-181">8</span></span> |<span data-ttu-id="24457-182">EBOD skříň</span><span class="sxs-lookup"><span data-stu-id="24457-182">EBOD enclosure</span></span> |
   | <span data-ttu-id="24457-183">9</span><span class="sxs-lookup"><span data-stu-id="24457-183">9</span></span> |<span data-ttu-id="24457-184">Jednotek pro distribuci napájení</span><span class="sxs-lookup"><span data-stu-id="24457-184">Power Distribution Units</span></span> |

## <a name="next-steps"></a><span data-ttu-id="24457-185">Další kroky</span><span class="sxs-lookup"><span data-stu-id="24457-185">Next steps</span></span>
<span data-ttu-id="24457-186">Další informace o [StorSimple hardwarové součásti nahrazení](storsimple-hardware-component-replacement.md).</span><span class="sxs-lookup"><span data-stu-id="24457-186">Learn more about [StorSimple hardware component replacement](storsimple-hardware-component-replacement.md).</span></span>

