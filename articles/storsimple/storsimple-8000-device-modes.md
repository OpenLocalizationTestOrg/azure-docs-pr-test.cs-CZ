---
title: "režim zařízení StorSimple aaaChange | Microsoft Docs"
description: "Popisuje režimy zařízení StorSimple hello a vysvětluje, jak toouse Windows Powershellu pro StorSimple toochange hello režim zařízení."
services: storsimple
documentationcenter: 
author: alkohli
manager: timlt
editor: 
ms.assetid: 
ms.service: storsimple
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/29/2017
ms.author: alkohli
ms.openlocfilehash: 058ca6cc38954bce3679cc21b39d341b10cb4dfb
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="change-hello-device-mode-on-your-storsimple-device"></a><span data-ttu-id="5a2e5-103">Změnit režim hello zařízení na zařízení StorSimple</span><span class="sxs-lookup"><span data-stu-id="5a2e5-103">Change hello device mode on your StorSimple device</span></span>

<span data-ttu-id="5a2e5-104">Tento článek obsahuje stručný popis hello různé režimy, ve kterých může fungovat i zařízení StorSimple.</span><span class="sxs-lookup"><span data-stu-id="5a2e5-104">This article provides a brief description of hello various modes in which your StorSimple device can operate.</span></span> <span data-ttu-id="5a2e5-105">Zařízení StorSimple, můžou fungovat v tři režimy: Normální, údržbu a obnovení.</span><span class="sxs-lookup"><span data-stu-id="5a2e5-105">Your StorSimple device can function in three modes: normal, maintenance, and recovery.</span></span>

<span data-ttu-id="5a2e5-106">Po přečtení tohoto článku, budete vědět:</span><span class="sxs-lookup"><span data-stu-id="5a2e5-106">After reading this article, you will know:</span></span>

* <span data-ttu-id="5a2e5-107">Jaké jsou režimy zařízení StorSimple hello</span><span class="sxs-lookup"><span data-stu-id="5a2e5-107">What hello StorSimple device modes are</span></span>
* <span data-ttu-id="5a2e5-108">Jak toofigure, na kterém režimu hello zařízení StorSimple je v</span><span class="sxs-lookup"><span data-stu-id="5a2e5-108">How toofigure out which mode hello StorSimple device is in</span></span>
* <span data-ttu-id="5a2e5-109">Jak toochange z režimu Normální toomaintenance a *naopak*</span><span class="sxs-lookup"><span data-stu-id="5a2e5-109">How toochange from normal toomaintenance mode and *vice versa*</span></span>

<span data-ttu-id="5a2e5-110">Hello výše úlohy správy lze provést pouze prostřednictvím rozhraní Windows PowerShell hello zařízení StorSimple.</span><span class="sxs-lookup"><span data-stu-id="5a2e5-110">hello above management tasks can only be performed via hello Windows PowerShell interface of your StorSimple device.</span></span>

## <a name="about-storsimple-device-modes"></a><span data-ttu-id="5a2e5-111">O režimech zařízení StorSimple</span><span class="sxs-lookup"><span data-stu-id="5a2e5-111">About StorSimple device modes</span></span>

<span data-ttu-id="5a2e5-112">Zařízení StorSimple můžou fungovat v režimu Normální, údržby nebo obnovení.</span><span class="sxs-lookup"><span data-stu-id="5a2e5-112">Your StorSimple device can operate in normal, maintenance, or recovery mode.</span></span> <span data-ttu-id="5a2e5-113">Každá z těchto režimů stručně je popsána níže.</span><span class="sxs-lookup"><span data-stu-id="5a2e5-113">Each of these modes is briefly described below.</span></span>

### <a name="normal-mode"></a><span data-ttu-id="5a2e5-114">Normálním režimu</span><span class="sxs-lookup"><span data-stu-id="5a2e5-114">Normal mode</span></span>

<span data-ttu-id="5a2e5-115">To je definován jako hello normální provozní režim pro zařízení StorSimple plně nakonfigurované.</span><span class="sxs-lookup"><span data-stu-id="5a2e5-115">This is defined as hello normal operational mode for a fully configured StorSimple device.</span></span> <span data-ttu-id="5a2e5-116">Ve výchozím nastavení musí být zařízení v normálním režimu.</span><span class="sxs-lookup"><span data-stu-id="5a2e5-116">By default, your device should be in normal mode.</span></span>

### <a name="maintenance-mode"></a><span data-ttu-id="5a2e5-117">Režim údržby</span><span class="sxs-lookup"><span data-stu-id="5a2e5-117">Maintenance mode</span></span>

<span data-ttu-id="5a2e5-118">Někdy hello zařízení StorSimple může být nutné toobe umístěn do režimu údržby.</span><span class="sxs-lookup"><span data-stu-id="5a2e5-118">Sometimes hello StorSimple device may need toobe placed into maintenance mode.</span></span> <span data-ttu-id="5a2e5-119">Tento režim na zařízení hello vám umožní tooperform údržby a instalovat rušivý aktualizace, například těch, které související toodisk firmwaru.</span><span class="sxs-lookup"><span data-stu-id="5a2e5-119">This mode allows you tooperform maintenance on hello device and install disruptive updates, such as those related toodisk firmware.</span></span>

<span data-ttu-id="5a2e5-120">Hello systému můžete uvést do režimu údržby pouze prostřednictvím hello Windows Powershellu pro StorSimple.</span><span class="sxs-lookup"><span data-stu-id="5a2e5-120">You can put hello system into maintenance mode only via hello Windows PowerShell for StorSimple.</span></span> <span data-ttu-id="5a2e5-121">V tomto režimu jsou pozastavena všechny vstupně-výstupní požadavky.</span><span class="sxs-lookup"><span data-stu-id="5a2e5-121">All I/O requests are paused in this mode.</span></span> <span data-ttu-id="5a2e5-122">Také se zastaví službám, jako je paměť s náhodným přístupem stálé (paměti NVRAM) nebo hello Clusterová služba.</span><span class="sxs-lookup"><span data-stu-id="5a2e5-122">Services such as non-volatile random access memory (NVRAM) or hello clustering service are also stopped.</span></span> <span data-ttu-id="5a2e5-123">Oba řadiče hello se restartují, když zadáte nebo ukončit tento režim.</span><span class="sxs-lookup"><span data-stu-id="5a2e5-123">Both hello controllers are restarted when you enter or exit this mode.</span></span> <span data-ttu-id="5a2e5-124">Při ukončení režimu údržby hello se všechny služby hello bude pokračovat a musí být v pořádku.</span><span class="sxs-lookup"><span data-stu-id="5a2e5-124">When you exit hello maintenance mode, all hello services will resume and should be healthy.</span></span> <span data-ttu-id="5a2e5-125">Může to trvat několik minut.</span><span class="sxs-lookup"><span data-stu-id="5a2e5-125">This may take a few minutes.</span></span>

> [!NOTE]
> <span data-ttu-id="5a2e5-126">**Režim údržby je podporována pouze na zařízení správně funguje. Není podporována u zařízení, ve které jedna nebo obě hello řadičů nefungují.**</span><span class="sxs-lookup"><span data-stu-id="5a2e5-126">**Maintenance mode is only supported on a properly functioning device. It is not supported on a device in which one or both of hello controllers are not functioning.**</span></span>


### <a name="recovery-mode"></a><span data-ttu-id="5a2e5-127">Obnovení režimu</span><span class="sxs-lookup"><span data-stu-id="5a2e5-127">Recovery mode</span></span>

<span data-ttu-id="5a2e5-128">Obnovení režimu lze popsat jako "Bezpečný režim pro Windows s podporou sítě".</span><span class="sxs-lookup"><span data-stu-id="5a2e5-128">Recovery mode can be described as "Windows Safe Mode with network support".</span></span> <span data-ttu-id="5a2e5-129">Režimu obnovení, zapojí týmu Microsoft Support hello a umožňuje jim tooperform diagnostiky systému hello.</span><span class="sxs-lookup"><span data-stu-id="5a2e5-129">Recovery mode engages hello Microsoft Support team and allows them tooperform diagnostics on hello system.</span></span> <span data-ttu-id="5a2e5-130">primární cílem Hello režimu obnovení je tooretrieve hello systémové protokoly.</span><span class="sxs-lookup"><span data-stu-id="5a2e5-130">hello primary goal of recovery mode is tooretrieve hello system logs.</span></span>

<span data-ttu-id="5a2e5-131">Pokud váš systém přejde do režimu obnovení, měli byste požádat Microsoft Support pro další kroky.</span><span class="sxs-lookup"><span data-stu-id="5a2e5-131">If your system goes into recovery mode, you should contact Microsoft Support for next steps.</span></span> <span data-ttu-id="5a2e5-132">Další informace, přejděte příliš[obraťte se na podporu společnosti Microsoft](storsimple-8000-contact-microsoft-support.md).</span><span class="sxs-lookup"><span data-stu-id="5a2e5-132">For more information, go too[Contact Microsoft Support](storsimple-8000-contact-microsoft-support.md).</span></span>

> [!NOTE]
> <span data-ttu-id="5a2e5-133">**Nelze umístit hello zařízení v režimu obnovení. Pokud zařízení hello je ve špatném stavu, režimu obnovení pokusí tooget hello zařízení ve stavu, ve kterém můžete zkontrolovat Microsoft Support pracovníky ho.**</span><span class="sxs-lookup"><span data-stu-id="5a2e5-133">**You cannot place hello device in recovery mode. If hello device is in a bad state, recovery mode tries tooget hello device into a state in which Microsoft Support personnel can examine it.**</span></span>

## <a name="determine-storsimple-device-mode"></a><span data-ttu-id="5a2e5-134">Určení režimu zařízení StorSimple</span><span class="sxs-lookup"><span data-stu-id="5a2e5-134">Determine StorSimple device mode</span></span>

#### <a name="toodetermine-hello-current-device-mode"></a><span data-ttu-id="5a2e5-135">aktuální režim zařízení toodetermine hello</span><span class="sxs-lookup"><span data-stu-id="5a2e5-135">toodetermine hello current device mode</span></span>

1. <span data-ttu-id="5a2e5-136">Přihlaste se pomocí následujících kroků hello v konzole sériového portu zařízení toohello [konzoly sériového portu toohello zařízení tooconnect pro použití klienta PuTTY](storsimple-8000-deployment-walkthrough-u2.md#use-putty-to-connect-to-the-device-serial-console).</span><span class="sxs-lookup"><span data-stu-id="5a2e5-136">Log on toohello device serial console by following hello steps in [Use PuTTY tooconnect toohello device serial console](storsimple-8000-deployment-walkthrough-u2.md#use-putty-to-connect-to-the-device-serial-console).</span></span>
2. <span data-ttu-id="5a2e5-137">Podívejte se na uvítací zprávu banner v nabídce konzoly sériového portu hello hello zařízení.</span><span class="sxs-lookup"><span data-stu-id="5a2e5-137">Look at hello banner message in hello serial console menu of hello device.</span></span> <span data-ttu-id="5a2e5-138">Tato zpráva explicitně určuje, zda text hello zařízení je v režimu údržby nebo obnovení.</span><span class="sxs-lookup"><span data-stu-id="5a2e5-138">This message explicitly indicates whether hello device is in maintenance or recovery mode.</span></span> <span data-ttu-id="5a2e5-139">Pokud zpráva hello neobsahuje žádné konkrétní informace týkající se režimu toohello systému, hello zařízení je v normálním režimu.</span><span class="sxs-lookup"><span data-stu-id="5a2e5-139">If hello message does not contain any specific information pertaining toohello system mode, hello device is in normal mode.</span></span>

## <a name="change-hello-storsimple-device-mode"></a><span data-ttu-id="5a2e5-140">Změnit režim zařízení StorSimple hello</span><span class="sxs-lookup"><span data-stu-id="5a2e5-140">Change hello StorSimple device mode</span></span>

<span data-ttu-id="5a2e5-141">Můžete umístit zařízení StorSimple hello do údržby režimu (od normální režim) tooperform údržby nebo nainstalovat aktualizace režimu údržby.</span><span class="sxs-lookup"><span data-stu-id="5a2e5-141">You can place hello StorSimple device into maintenance mode (from normal mode) tooperform maintenance or install maintenance mode updates.</span></span> <span data-ttu-id="5a2e5-142">Proveďte následující postupy tooenter nebo ukončení režimu údržby hello.</span><span class="sxs-lookup"><span data-stu-id="5a2e5-142">Perform hello following procedures tooenter or exit maintenance mode.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="5a2e5-143">Před přechodem do režimu údržby, ověřte, zda jsou oba řadiče zařízení v pořádku díky přístupu k hello **nastavení zařízení > stavu hardwaru** pro zařízení v hello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="5a2e5-143">Before entering maintenance mode, verify that both device controllers are healthy by accessing hello **Device settings > Hardware health** for your device in hello Azure portal.</span></span> <span data-ttu-id="5a2e5-144">Pokud jeden nebo oba řadiče hello nejsou v pořádku, požádejte o další kroky hello Microsoft Support.</span><span class="sxs-lookup"><span data-stu-id="5a2e5-144">If one or both hello controllers are not healthy, contact Microsoft Support for hello next steps.</span></span> <span data-ttu-id="5a2e5-145">Další informace, přejděte příliš[obraťte se na podporu společnosti Microsoft](storsimple-8000-contact-microsoft-support.md).</span><span class="sxs-lookup"><span data-stu-id="5a2e5-145">For more information, go too[Contact Microsoft Support](storsimple-8000-contact-microsoft-support.md).</span></span>
 

#### <a name="tooenter-maintenance-mode"></a><span data-ttu-id="5a2e5-146">tooenter režimu údržby</span><span class="sxs-lookup"><span data-stu-id="5a2e5-146">tooenter maintenance mode</span></span>

1. <span data-ttu-id="5a2e5-147">Přihlaste se pomocí následujících kroků hello v konzole sériového portu zařízení toohello [konzoly sériového portu toohello zařízení tooconnect pro použití klienta PuTTY](storsimple-8000-deployment-walkthrough-u2.md#use-putty-to-connect-to-the-device-serial-console).</span><span class="sxs-lookup"><span data-stu-id="5a2e5-147">Log on toohello device serial console by following hello steps in [Use PuTTY tooconnect toohello device serial console](storsimple-8000-deployment-walkthrough-u2.md#use-putty-to-connect-to-the-device-serial-console).</span></span>
2. <span data-ttu-id="5a2e5-148">V nabídce konzoly sériového portu hello, zvolte možnost 1, **přihlásit úplný přístup**.</span><span class="sxs-lookup"><span data-stu-id="5a2e5-148">In hello serial console menu, choose option 1, **Log in with full access**.</span></span> <span data-ttu-id="5a2e5-149">Po zobrazení výzvy zadejte hello **hesla správce zařízení**.</span><span class="sxs-lookup"><span data-stu-id="5a2e5-149">When prompted, provide hello **device administrator password**.</span></span> <span data-ttu-id="5a2e5-150">výchozí heslo Hello je: `Password1`.</span><span class="sxs-lookup"><span data-stu-id="5a2e5-150">hello default password is: `Password1`.</span></span>
3. <span data-ttu-id="5a2e5-151">Hello příkazového řádku zadejte</span><span class="sxs-lookup"><span data-stu-id="5a2e5-151">At hello command prompt, type</span></span> 
   
    `Enter-HcsMaintenanceMode`
4. <span data-ttu-id="5a2e5-152">Zobrazí se upozornění oznamující, že režimu údržby se přerušit všechny vstupně-výstupní požadavky a severu hello připojení toohello portál Azure a zobrazí se výzva k potvrzení.</span><span class="sxs-lookup"><span data-stu-id="5a2e5-152">You will see a warning message telling you that maintenance mode will disrupt all I/O requests and sever hello connection toohello Azure portal, and you will be prompted for confirmation.</span></span> <span data-ttu-id="5a2e5-153">Typ **Y** tooenter režimu údržby.</span><span class="sxs-lookup"><span data-stu-id="5a2e5-153">Type **Y** tooenter maintenance mode.</span></span>
5. <span data-ttu-id="5a2e5-154">Oba řadiče se restartuje.</span><span class="sxs-lookup"><span data-stu-id="5a2e5-154">Both controllers will restart.</span></span> <span data-ttu-id="5a2e5-155">Po dokončení restartování hello hello konzoly sériového portu banner označí, že toto zařízení hello je v režimu údržby.</span><span class="sxs-lookup"><span data-stu-id="5a2e5-155">When hello restart is complete, hello serial console banner will indicate that hello device is in maintenance mode.</span></span> <span data-ttu-id="5a2e5-156">Ukázkový výstup najdete níž.</span><span class="sxs-lookup"><span data-stu-id="5a2e5-156">A sample output is shown below.</span></span>

```
    ---------------------------------------------------------------
    Microsoft Azure StorSimple Appliance Model 8100
    Name: 8100-SHX0991003G44MT
    Software Version: 6.3.9600.17820
    Copyright (C) 2014 Microsoft Corporation. All rights reserved.
    You are connected tooController0 - Passive
    ---------------------------------------------------------------

    Controller0>Enter-HcsMaintenanceMode
    Checking device state...

    In maintenance mode, your device will not service IOs and will be disconnected from hello Microsoft Azure StorSimple Manager service. Entering maintenance mode will end hello current session and reboot both controllers, which takes a few minutes toocomplete. Are you sure you want tooenter maintenance mode?
    [Y] Yes [N] No (Default is "Y"): Y

    <BOTH CONTROLLERS RESTART>

    -----------------------MAINTENANCE MODE------------------------
    Microsoft Azure StorSimple Appliance Model 8100
    Name: 8100-SHX0991003G44MT
    Software Version: 6.3.9600.17820
    Copyright (C) 2014 Microsoft Corporation. All rights reserved.
    You are connected tooController0 - Passive
    ---------------------------------------------------------------

    Serial Console Menu
    [1] Log in with full access
    [2] Log into peer controller with full access
    [3] Connect with limited access
    [4] Change language
    Please enter your choice>

```

#### <a name="tooexit-maintenance-mode"></a><span data-ttu-id="5a2e5-157">tooexit režimu údržby</span><span class="sxs-lookup"><span data-stu-id="5a2e5-157">tooexit maintenance mode</span></span>

1. <span data-ttu-id="5a2e5-158">Přihlaste se toohello konzoly sériového portu zařízení.</span><span class="sxs-lookup"><span data-stu-id="5a2e5-158">Log on toohello device serial console.</span></span> <span data-ttu-id="5a2e5-159">Ověřte ze hello zpráva hlavičky, která vaše zařízení je v režimu údržby.</span><span class="sxs-lookup"><span data-stu-id="5a2e5-159">Verify from hello banner message that your device is in maintenance mode.</span></span>
2. <span data-ttu-id="5a2e5-160">Hello příkazového řádku zadejte:</span><span class="sxs-lookup"><span data-stu-id="5a2e5-160">At hello command prompt, type:</span></span>
   
    `Exit-HcsMaintenanceMode`
3. <span data-ttu-id="5a2e5-161">Zobrazí se zpráva s upozorněním a potvrzovací zpráva.</span><span class="sxs-lookup"><span data-stu-id="5a2e5-161">A warning message and a confirmation message will appear.</span></span> <span data-ttu-id="5a2e5-162">Typ **Y** tooexit režimu údržby.</span><span class="sxs-lookup"><span data-stu-id="5a2e5-162">Type **Y** tooexit maintenance mode.</span></span>
4. <span data-ttu-id="5a2e5-163">Oba řadiče se restartuje.</span><span class="sxs-lookup"><span data-stu-id="5a2e5-163">Both controllers will restart.</span></span> <span data-ttu-id="5a2e5-164">Po dokončení restartování hello hello konzoly sériového portu banner značí, že toto zařízení hello je v normálním režimu.</span><span class="sxs-lookup"><span data-stu-id="5a2e5-164">When hello restart is complete, hello serial console banner indicates that hello device is in normal mode.</span></span> <span data-ttu-id="5a2e5-165">Ukázkový výstup najdete níž.</span><span class="sxs-lookup"><span data-stu-id="5a2e5-165">A sample output is shown below.</span></span>

```
    -----------------------MAINTENANCE MODE------------------------
    Microsoft Azure StorSimple Appliance Model 8100
    Name: 8100-SHX0991003G44MT
    Software Version: 6.3.9600.17820
    Copyright (C) 2014 Microsoft Corporation. All rights reserved.
    You are connected tooController0
    ---------------------------------------------------------------

    Controller0>Exit-HcsMaintenanceMode
    Checking device state...

    Before exiting maintenance mode, ensure that any updates that are required on both controllers have been applied. Failure tooinstall on each controller could result in data corruption. Exiting maintenance mode will end hello current session and reboot both controllers, which takes a few minutes toocomplete. Are you sure you want tooexit maintenance mode?
    [Y] Yes [N] No (Default is "Y"): Y

    <BOTH CONTROLLERS RESTART>

    ---------------------------------------------------------------
    Microsoft Azure StorSimple Appliance Model 8100
    Name: 8100-SHX0991003G44MT
    Software Version: 6.3.9600.17820
    Copyright (C) 2014 Microsoft Corporation. All rights reserved.
    You are connected tooController0 - Active
    ---------------------------------------------------------------

    Serial Console Menu
    [1] Log in with full access
    [2] Log into peer controller with full access
    [3] Connect with limited access
    [4] Change language
    Please enter your choice>
```

## <a name="next-steps"></a><span data-ttu-id="5a2e5-166">Další kroky</span><span class="sxs-lookup"><span data-stu-id="5a2e5-166">Next steps</span></span>

<span data-ttu-id="5a2e5-167">Zjistěte, jak příliš[režim aktualizace normální a údržby](storsimple-update-device.md) zařízení StorSimple.</span><span class="sxs-lookup"><span data-stu-id="5a2e5-167">Learn how too[apply normal and maintenance mode updates](storsimple-update-device.md) on your StorSimple device.</span></span>

