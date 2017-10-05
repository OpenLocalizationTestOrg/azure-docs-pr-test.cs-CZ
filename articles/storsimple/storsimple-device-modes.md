---
title: "Změnit režim zařízení StorSimple | Microsoft Docs"
description: "Popisuje režimy zařízení StorSimple a vysvětluje, jak pomocí prostředí Windows PowerShell pro StorSimple ke změně režimu zařízení."
services: storsimple
documentationcenter: 
author: alkohli
manager: carmonm
editor: 
ms.assetid: e9d7d277-8a2f-45eb-9fef-355486e14cbc
ms.service: storsimple
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/17/2016
ms.author: alkohli
ms.openlocfilehash: 33c65bf2eecff3914f3227c76f7d638a4507e1f6
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="change-the-device-mode-on-your-storsimple-device"></a><span data-ttu-id="14617-103">Změnit režim zařízení na zařízení StorSimple</span><span class="sxs-lookup"><span data-stu-id="14617-103">Change the device mode on your StorSimple device</span></span>
<span data-ttu-id="14617-104">Tento článek obsahuje stručný popis v různých režimech, ve kterých může fungovat i zařízení StorSimple.</span><span class="sxs-lookup"><span data-stu-id="14617-104">This article provides a brief description of the various modes in which your StorSimple device can operate.</span></span> <span data-ttu-id="14617-105">Zařízení StorSimple, můžou fungovat v tři režimy: Normální, údržbu a obnovení.</span><span class="sxs-lookup"><span data-stu-id="14617-105">Your StorSimple device can function in three modes: normal, maintenance, and recovery.</span></span> 

<span data-ttu-id="14617-106">Po přečtení tohoto článku, budete vědět:</span><span class="sxs-lookup"><span data-stu-id="14617-106">After reading this article, you will know:</span></span>

* <span data-ttu-id="14617-107">Co režim pro zařízení StorSimple</span><span class="sxs-lookup"><span data-stu-id="14617-107">What the StorSimple device modes are</span></span>
* <span data-ttu-id="14617-108">Jak zjistit, který režim zařízení StorSimple je v</span><span class="sxs-lookup"><span data-stu-id="14617-108">How to figure out which mode the StorSimple device is in</span></span>
* <span data-ttu-id="14617-109">Postup změny mezi běžnou zátěží a režimu údržby a *naopak*</span><span class="sxs-lookup"><span data-stu-id="14617-109">How to change from normal to maintenance mode and *vice versa*</span></span>

<span data-ttu-id="14617-110">Výše uvedené úlohy správy lze provést pouze prostřednictvím rozhraní Windows PowerShell zařízení StorSimple.</span><span class="sxs-lookup"><span data-stu-id="14617-110">The above management tasks can only be performed via the Windows PowerShell interface of your StorSimple device.</span></span>

## <a name="about-storsimple-device-modes"></a><span data-ttu-id="14617-111">O režimech zařízení StorSimple</span><span class="sxs-lookup"><span data-stu-id="14617-111">About StorSimple device modes</span></span>
<span data-ttu-id="14617-112">Zařízení StorSimple můžou fungovat v režimu Normální, údržby nebo obnovení.</span><span class="sxs-lookup"><span data-stu-id="14617-112">Your StorSimple device can operate in normal, maintenance, or recovery mode.</span></span> <span data-ttu-id="14617-113">Každá z těchto režimů stručně je popsána níže.</span><span class="sxs-lookup"><span data-stu-id="14617-113">Each of these modes is briefly described below.</span></span>

### <a name="normal-mode"></a><span data-ttu-id="14617-114">Normálním režimu</span><span class="sxs-lookup"><span data-stu-id="14617-114">Normal mode</span></span>
<span data-ttu-id="14617-115">To je definován jako normální provozní režim pro zařízení StorSimple plně nakonfigurované.</span><span class="sxs-lookup"><span data-stu-id="14617-115">This is defined as the normal operational mode for a fully configured StorSimple device.</span></span> <span data-ttu-id="14617-116">Ve výchozím nastavení musí být zařízení v normálním režimu.</span><span class="sxs-lookup"><span data-stu-id="14617-116">By default, your device should be in normal mode.</span></span>

### <a name="maintenance-mode"></a><span data-ttu-id="14617-117">Režim údržby</span><span class="sxs-lookup"><span data-stu-id="14617-117">Maintenance mode</span></span>
<span data-ttu-id="14617-118">V některých případech zařízení StorSimple muset být umístěn do režimu údržby.</span><span class="sxs-lookup"><span data-stu-id="14617-118">Sometimes the StorSimple device may need to be placed into maintenance mode.</span></span> <span data-ttu-id="14617-119">Tento režim umožňuje provést údržbu na zařízení a instalovat aktualizace rušivým zásahům, například související s firmwarem disku.</span><span class="sxs-lookup"><span data-stu-id="14617-119">This mode allows you to perform maintenance on the device and install disruptive updates, such as those related to disk firmware.</span></span>

<span data-ttu-id="14617-120">Systém lze uvést do režimu údržby jenom prostřednictvím Windows Powershellu pro StorSimple.</span><span class="sxs-lookup"><span data-stu-id="14617-120">You can put the system into maintenance mode only via the Windows PowerShell for StorSimple.</span></span> <span data-ttu-id="14617-121">V tomto režimu jsou pozastavena všechny vstupně-výstupní požadavky.</span><span class="sxs-lookup"><span data-stu-id="14617-121">All I/O requests are paused in this mode.</span></span> <span data-ttu-id="14617-122">Také se zastaví službám, jako je paměť s náhodným přístupem stálé (paměti NVRAM) nebo službu clusteringu.</span><span class="sxs-lookup"><span data-stu-id="14617-122">Services such as non-volatile random access memory (NVRAM) or the clustering service are also stopped.</span></span> <span data-ttu-id="14617-123">Oběma řadičům se restartují, když zadáte nebo ukončit tento režim.</span><span class="sxs-lookup"><span data-stu-id="14617-123">Both the controllers are restarted when you enter or exit this mode.</span></span> <span data-ttu-id="14617-124">Při ukončení režimu údržby se všechny služby bude pokračovat a musí být v pořádku.</span><span class="sxs-lookup"><span data-stu-id="14617-124">When you exit the maintenance mode, all the services will resume and should be healthy.</span></span> <span data-ttu-id="14617-125">Může to trvat několik minut.</span><span class="sxs-lookup"><span data-stu-id="14617-125">This may take a few minutes.</span></span>

> [!NOTE]
> <span data-ttu-id="14617-126">**Režim údržby je podporována pouze na zařízení správně funguje. Není podporována u zařízení, ve které jedna nebo obě řadičů nefungují.**
> </span><span class="sxs-lookup"><span data-stu-id="14617-126">**Maintenance mode is only supported on a properly functioning device. It is not supported on a device in which one or both of the controllers are not functioning.**
</span></span></br>
> 
> 

### <a name="recovery-mode"></a><span data-ttu-id="14617-127">Obnovení režimu</span><span class="sxs-lookup"><span data-stu-id="14617-127">Recovery mode</span></span>
<span data-ttu-id="14617-128">Obnovení režimu lze popsat jako "Bezpečný režim pro Windows s podporou sítě".</span><span class="sxs-lookup"><span data-stu-id="14617-128">Recovery mode can be described as "Windows Safe Mode with network support".</span></span> <span data-ttu-id="14617-129">Obnovení režimu zapojí týmem Microsoft Support a umožňuje jim umožníte provádět diagnostiky v systému.</span><span class="sxs-lookup"><span data-stu-id="14617-129">Recovery mode engages the Microsoft Support team and allows them to perform diagnostics on the system.</span></span> <span data-ttu-id="14617-130">Primární cílem režimu obnovení je načíst protokoly systému.</span><span class="sxs-lookup"><span data-stu-id="14617-130">The primary goal of recovery mode is to retrieve the system logs.</span></span>

<span data-ttu-id="14617-131">Pokud váš systém přejde do režimu obnovení, měli byste požádat Microsoft Support pro další kroky.</span><span class="sxs-lookup"><span data-stu-id="14617-131">If your system goes into recovery mode, you should contact Microsoft Support for next steps.</span></span> <span data-ttu-id="14617-132">Další informace, přejděte na [obraťte se na podporu společnosti Microsoft](storsimple-contact-microsoft-support.md).</span><span class="sxs-lookup"><span data-stu-id="14617-132">For more information, go to [Contact Microsoft Support](storsimple-contact-microsoft-support.md).</span></span>

> [!NOTE]
> <span data-ttu-id="14617-133">**Zařízení nelze umístit v režimu obnovení. Pokud je ve špatném stavu zařízení, pokusí se režimu obnovení získat zařízení do stavu, ve kterém můžete zkontrolovat Microsoft Support pracovníky ho.**</span><span class="sxs-lookup"><span data-stu-id="14617-133">**You cannot place the device in recovery mode. If the device is in a bad state, recovery mode tries to get the device into a state in which Microsoft Support personnel can examine it.**</span></span>
> 
> 

## <a name="determine-storsimple-device-mode"></a><span data-ttu-id="14617-134">Určení režimu zařízení StorSimple</span><span class="sxs-lookup"><span data-stu-id="14617-134">Determine StorSimple device mode</span></span>
#### <a name="to-determine-the-current-device-mode"></a><span data-ttu-id="14617-135">Chcete-li zjistit aktuální režim zařízení</span><span class="sxs-lookup"><span data-stu-id="14617-135">To determine the current device mode</span></span>
1. <span data-ttu-id="14617-136">Přihlaste se k konzole sériového portu zařízení podle pokynů v [použití klienta PuTTY k připojení ke konzole sériového portu zařízení](storsimple-deployment-walkthrough.md#use-putty-to-connect-to-the-device-serial-console).</span><span class="sxs-lookup"><span data-stu-id="14617-136">Log on to the device serial console by following the steps in [Use PuTTY to connect to the device serial console](storsimple-deployment-walkthrough.md#use-putty-to-connect-to-the-device-serial-console).</span></span>
2. <span data-ttu-id="14617-137">Podívejte se na zpráva hlavičky v nabídce konzoly sériového portu zařízení.</span><span class="sxs-lookup"><span data-stu-id="14617-137">Look at the banner message in the serial console menu of the device.</span></span> <span data-ttu-id="14617-138">Tato zpráva explicitně znamená, zda je zařízení v režimu údržby nebo obnovení.</span><span class="sxs-lookup"><span data-stu-id="14617-138">This message explicitly indicates whether the device is in maintenance or recovery mode.</span></span> <span data-ttu-id="14617-139">Pokud zpráva neobsahuje žádné konkrétní informace týkající se režimu systému, zařízení je v normálním režimu.</span><span class="sxs-lookup"><span data-stu-id="14617-139">If the message does not contain any specific information pertaining to the system mode, the device is in normal mode.</span></span>

## <a name="change-the-storsimple-device-mode"></a><span data-ttu-id="14617-140">Změnit režim zařízení StorSimple</span><span class="sxs-lookup"><span data-stu-id="14617-140">Change the StorSimple device mode</span></span>
<span data-ttu-id="14617-141">Zařízení StorSimple můžete umístit do režimu údržby (od normální režim) k provedení údržby nebo instalace aktualizací režimu údržby.</span><span class="sxs-lookup"><span data-stu-id="14617-141">You can place the StorSimple device into maintenance mode (from normal mode) to perform maintenance or install maintenance mode updates.</span></span> <span data-ttu-id="14617-142">Proveďte následující postupy k zadání nebo ukončení režimu údržby.</span><span class="sxs-lookup"><span data-stu-id="14617-142">Perform the following procedures to enter or exit maintenance mode.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="14617-143">Před přechodem do režimu údržby, ověřte, zda jsou oba řadiče zařízení v pořádku přímým přístupem **stavu hardwaru** na **údržby** na portálu Azure classic.</span><span class="sxs-lookup"><span data-stu-id="14617-143">Before entering maintenance mode, verify that both device controllers are healthy by accessing the **Hardware Status** on the **Maintenance** page in the Azure classic portal.</span></span> <span data-ttu-id="14617-144">Pokud jeden nebo oba řadiče nejsou v pořádku, požádejte o další kroky Microsoft Support.</span><span class="sxs-lookup"><span data-stu-id="14617-144">If one or both the controllers are not healthy, contact Microsoft Support for the next steps.</span></span> <span data-ttu-id="14617-145">Další informace, přejděte na [obraťte se na podporu společnosti Microsoft](storsimple-contact-microsoft-support.md).</span><span class="sxs-lookup"><span data-stu-id="14617-145">For more information, go to [Contact Microsoft Support](storsimple-contact-microsoft-support.md).</span></span>
> 
> 

#### <a name="to-enter-maintenance-mode"></a><span data-ttu-id="14617-146">Přejít do režimu údržby</span><span class="sxs-lookup"><span data-stu-id="14617-146">To enter maintenance mode</span></span>
1. <span data-ttu-id="14617-147">Přihlaste se k konzole sériového portu zařízení podle pokynů v [použití klienta PuTTY k připojení ke konzole sériového portu zařízení](storsimple-deployment-walkthrough.md#use-putty-to-connect-to-the-device-serial-console).</span><span class="sxs-lookup"><span data-stu-id="14617-147">Log on to the device serial console by following the steps in [Use PuTTY to connect to the device serial console](storsimple-deployment-walkthrough.md#use-putty-to-connect-to-the-device-serial-console).</span></span>
2. <span data-ttu-id="14617-148">V nabídce konzoly sériového portu, zvolte možnost 1, **přihlásit úplný přístup**.</span><span class="sxs-lookup"><span data-stu-id="14617-148">In the serial console menu, choose option 1, **Log in with full access**.</span></span> <span data-ttu-id="14617-149">Pokud budete vyzváni, zadejte **hesla správce zařízení**.</span><span class="sxs-lookup"><span data-stu-id="14617-149">When prompted, provide the **device administrator password**.</span></span> <span data-ttu-id="14617-150">Výchozí heslo je: `Password1`.</span><span class="sxs-lookup"><span data-stu-id="14617-150">The default password is: `Password1`.</span></span>
3. <span data-ttu-id="14617-151">Na příkazovém řádku zadejte</span><span class="sxs-lookup"><span data-stu-id="14617-151">At the command prompt, type</span></span> 
   
    `Enter-HcsMaintenanceMode`
4. <span data-ttu-id="14617-152">Zobrazí se upozornění oznamující, že bude přerušit všechny vstupně-výstupní požadavky a severu připojení k portálu Azure classic režimu údržby a zobrazí se výzva k potvrzení.</span><span class="sxs-lookup"><span data-stu-id="14617-152">You will see a warning message telling you that maintenance mode will disrupt all I/O requests and sever the connection to the Azure classic portal, and you will be prompted for confirmation.</span></span> <span data-ttu-id="14617-153">Typ **Y** vstoupit do režimu údržby.</span><span class="sxs-lookup"><span data-stu-id="14617-153">Type **Y** to enter maintenance mode.</span></span>
5. <span data-ttu-id="14617-154">Oba řadiče se restartuje.</span><span class="sxs-lookup"><span data-stu-id="14617-154">Both controllers will restart.</span></span> <span data-ttu-id="14617-155">Po dokončení restartování se zobrazí další zpráva označující, že zařízení je v režimu údržby.</span><span class="sxs-lookup"><span data-stu-id="14617-155">When the restart is complete, another message will appear indicating that the device is in maintenance mode.</span></span>

#### <a name="to-exit-maintenance-mode"></a><span data-ttu-id="14617-156">Chcete-li ukončit režim údržby</span><span class="sxs-lookup"><span data-stu-id="14617-156">To exit maintenance mode</span></span>
1. <span data-ttu-id="14617-157">Přihlaste se ke konzole sériového portu zařízení.</span><span class="sxs-lookup"><span data-stu-id="14617-157">Log on to the device serial console.</span></span> <span data-ttu-id="14617-158">Ověřte ze zpráva hlavičky, která vaše zařízení je v režimu údržby.</span><span class="sxs-lookup"><span data-stu-id="14617-158">Verify from the banner message that your device is in maintenance mode.</span></span>
2. <span data-ttu-id="14617-159">Na příkazovém řádku zadejte:</span><span class="sxs-lookup"><span data-stu-id="14617-159">At the command prompt, type:</span></span>
   
    `Exit-HcsMaintenanceMode`
3. <span data-ttu-id="14617-160">Zobrazí se zpráva s upozorněním a potvrzovací zpráva.</span><span class="sxs-lookup"><span data-stu-id="14617-160">A warning message and a confirmation message will appear.</span></span> <span data-ttu-id="14617-161">Typ **Y** pro ukončení režimu údržby.</span><span class="sxs-lookup"><span data-stu-id="14617-161">Type **Y** to exit maintenance mode.</span></span>
4. <span data-ttu-id="14617-162">Oba řadiče se restartuje.</span><span class="sxs-lookup"><span data-stu-id="14617-162">Both controllers will restart.</span></span> <span data-ttu-id="14617-163">Po dokončení restartování se zobrazí další zpráva označující, že zařízení je v normálním režimu.</span><span class="sxs-lookup"><span data-stu-id="14617-163">When the restart is complete, another message will appear indicating that the device is in normal mode.</span></span>

## <a name="next-steps"></a><span data-ttu-id="14617-164">Další kroky</span><span class="sxs-lookup"><span data-stu-id="14617-164">Next steps</span></span>
<span data-ttu-id="14617-165">Zjistěte, jak [režim aktualizace normální a údržby](storsimple-update-device.md) zařízení StorSimple.</span><span class="sxs-lookup"><span data-stu-id="14617-165">Learn how to [apply normal and maintenance mode updates](storsimple-update-device.md) on your StorSimple device.</span></span>

