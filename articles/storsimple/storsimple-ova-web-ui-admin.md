---
title: "Pole virtuální zařízení StorSimple webového uživatelského rozhraní správy | Microsoft Docs"
description: "Popisuje, jak provádět úlohy správy základní zařízení StorSimple virtuální pole webového uživatelského rozhraní."
services: storsimple
documentationcenter: NA
author: alkohli
manager: carmonm
editor: 
ms.assetid: ea65b4c7-a478-43e6-83df-1d9ea62916a6
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 12/1/2016
ms.author: alkohli
ms.openlocfilehash: 989e7b697f9b527df549fb32be18edd1d3c8d224
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="use-the-web-ui-to-administer-your-storsimple-virtual-array"></a><span data-ttu-id="d5239-103">Pomocí webového uživatelského rozhraní pro správu pole virtuální zařízení StorSimple</span><span class="sxs-lookup"><span data-stu-id="d5239-103">Use the Web UI to administer your StorSimple Virtual Array</span></span>
![tok procesu instalace](./media/storsimple-ova-web-ui-admin/manage4.png)

## <a name="overview"></a><span data-ttu-id="d5239-105">Přehled</span><span class="sxs-lookup"><span data-stu-id="d5239-105">Overview</span></span>
<span data-ttu-id="d5239-106">Kurzy v tomto článku se vztahují na Microsoft Azure StorSimple virtuální pole (také označované jako místní virtuální zařízení StorSimple) verzí obecné dostupnosti (GA). března 2016.</span><span class="sxs-lookup"><span data-stu-id="d5239-106">The tutorials in this article apply to the Microsoft Azure StorSimple Virtual Array (also known as the StorSimple on-premises virtual device) running March 2016 general availability (GA) release.</span></span> <span data-ttu-id="d5239-107">Tento článek popisuje některé komplexní pracovní postupy a úlohy správy, které lze provést v poli virtuální zařízení StorSimple.</span><span class="sxs-lookup"><span data-stu-id="d5239-107">This article describes some of the complex workflows and management tasks that can be performed on the StorSimple Virtual Array.</span></span> <span data-ttu-id="d5239-108">Pole virtuální zařízení StorSimple pomocí StorSimple Manager můžete spravovat služby uživatelského rozhraní (označované jako portál uživatelského rozhraní) a místní webového uživatelského rozhraní pro zařízení.</span><span class="sxs-lookup"><span data-stu-id="d5239-108">You can manage the StorSimple Virtual Array using the StorSimple Manager service UI (referred to as the portal UI) and the local web UI for the device.</span></span> <span data-ttu-id="d5239-109">Tento článek se zaměřuje na úlohách, že můžete provádět pomocí webového uživatelského rozhraní.</span><span class="sxs-lookup"><span data-stu-id="d5239-109">This article focuses on the tasks that you can perform using the web UI.</span></span>

<span data-ttu-id="d5239-110">Tento článek obsahuje následující kurzy:</span><span class="sxs-lookup"><span data-stu-id="d5239-110">This article includes the following tutorials:</span></span>

* <span data-ttu-id="d5239-111">Získání šifrovacího klíče dat služby</span><span class="sxs-lookup"><span data-stu-id="d5239-111">Get the service data encryption key</span></span>
* <span data-ttu-id="d5239-112">Řešení chyb instalace webového uživatelského rozhraní</span><span class="sxs-lookup"><span data-stu-id="d5239-112">Troubleshoot web UI setup errors</span></span>
* <span data-ttu-id="d5239-113">Generovat balíček protokolu</span><span class="sxs-lookup"><span data-stu-id="d5239-113">Generate a log package</span></span>
* <span data-ttu-id="d5239-114">Vypnutí nebo restartování zařízení</span><span class="sxs-lookup"><span data-stu-id="d5239-114">Shut down or restart your device</span></span>

## <a name="get-the-service-data-encryption-key"></a><span data-ttu-id="d5239-115">Získání šifrovacího klíče dat služby</span><span class="sxs-lookup"><span data-stu-id="d5239-115">Get the service data encryption key</span></span>
<span data-ttu-id="d5239-116">Šifrovací klíč dat služby se vygeneruje, když zaregistrujete svoje první zařízení pomocí služby StorSimple Manager.</span><span class="sxs-lookup"><span data-stu-id="d5239-116">A service data encryption key is generated when you register your first device with the StorSimple Manager service.</span></span> <span data-ttu-id="d5239-117">Tento klíč je pak potřeba s registrační klíč služby k registraci dalších zařízení pomocí služby StorSimple Manager.</span><span class="sxs-lookup"><span data-stu-id="d5239-117">This key is then required with the service registration key to register additional devices with the StorSimple Manager service.</span></span>

<span data-ttu-id="d5239-118">Pokud budete mít k jejich chybnému umístění šifrovacího klíče dat služby a nutnost jejich načtení, proveďte následující kroky v místní webového uživatelského rozhraní zařízení zaregistrovali služby.</span><span class="sxs-lookup"><span data-stu-id="d5239-118">If you have misplaced your service data encryption key and need to retrieve it, perform the following steps in the local web UI of the device registered with your service.</span></span>

#### <a name="to-get-the-service-data-encryption-key"></a><span data-ttu-id="d5239-119">Chcete-li získat šifrovacího klíče dat služby</span><span class="sxs-lookup"><span data-stu-id="d5239-119">To get the service data encryption key</span></span>
1. <span data-ttu-id="d5239-120">Připojte k místní webového uživatelského rozhraní.</span><span class="sxs-lookup"><span data-stu-id="d5239-120">Connect to the local web UI.</span></span> <span data-ttu-id="d5239-121">Přejděte na **konfigurace** > **nastavení cloudu**.</span><span class="sxs-lookup"><span data-stu-id="d5239-121">Go to **Configuration** > **Cloud Settings**.</span></span>
2. <span data-ttu-id="d5239-122">V dolní části stránky klikněte na tlačítko **šifrovacího klíče dat služby Get**.</span><span class="sxs-lookup"><span data-stu-id="d5239-122">At the bottom of the page, click **Get service data encryption key**.</span></span> <span data-ttu-id="d5239-123">Zobrazí se klíč.</span><span class="sxs-lookup"><span data-stu-id="d5239-123">A key will appear.</span></span> <span data-ttu-id="d5239-124">Zkopírujte a uložte tento klíč.</span><span class="sxs-lookup"><span data-stu-id="d5239-124">Copy and save this key.</span></span>
   
    ![získání šifrovacího klíče dat služby 1](./media/storsimple-ova-web-ui-admin/image27.png)

## <a name="troubleshoot-web-ui-setup-errors"></a><span data-ttu-id="d5239-126">Řešení chyb instalace webového uživatelského rozhraní</span><span class="sxs-lookup"><span data-stu-id="d5239-126">Troubleshoot web UI setup errors</span></span>
<span data-ttu-id="d5239-127">V některých případech při konfiguraci zařízení prostřednictvím místního webového uživatelského rozhraní, může dojít k chybám.</span><span class="sxs-lookup"><span data-stu-id="d5239-127">In some instances when you configure the device through the local web UI, you might run into errors.</span></span> <span data-ttu-id="d5239-128">Při diagnostice a řešení těchto chyb, můžete spustit testy diagnostiky.</span><span class="sxs-lookup"><span data-stu-id="d5239-128">To diagnose and troubleshoot such errors, you can run the diagnostics tests.</span></span>

#### <a name="to-run-the-diagnostic-tests"></a><span data-ttu-id="d5239-129">Spustit diagnostické testy</span><span class="sxs-lookup"><span data-stu-id="d5239-129">To run the diagnostic tests</span></span>
1. <span data-ttu-id="d5239-130">V místní webového uživatelského rozhraní, přejděte do **Poradce při potížích s** > **diagnostických testů**.</span><span class="sxs-lookup"><span data-stu-id="d5239-130">In the local web UI, go to **Troubleshooting** > **Diagnostic tests**.</span></span>
   
    ![spustit diagnostiku 1](./media/storsimple-ova-web-ui-admin/image29.png)
2. <span data-ttu-id="d5239-132">V dolní části stránky klikněte na tlačítko **spuštění diagnostických testů**.</span><span class="sxs-lookup"><span data-stu-id="d5239-132">At the bottom of the page, click **Run Diagnostic Tests**.</span></span> <span data-ttu-id="d5239-133">Tím zahájíte testy při diagnostice všech možných problémů s vaší sítě, zařízení, webový proxy server, čas nebo nastavení cloudu.</span><span class="sxs-lookup"><span data-stu-id="d5239-133">This will initiate tests to diagnose any possible issues with your network, device, web proxy, time, or cloud settings.</span></span> <span data-ttu-id="d5239-134">Budete informováni, že je na zařízení spuštěný testy.</span><span class="sxs-lookup"><span data-stu-id="d5239-134">You will be notified that the device is running tests.</span></span>
3. <span data-ttu-id="d5239-135">Po dokončení testů, se zobrazí výsledky.</span><span class="sxs-lookup"><span data-stu-id="d5239-135">After the tests have completed, the results will be displayed.</span></span> <span data-ttu-id="d5239-136">Následující příklad ukazuje výsledky diagnostických testů.</span><span class="sxs-lookup"><span data-stu-id="d5239-136">The following example shows the results of diagnostic tests.</span></span> <span data-ttu-id="d5239-137">Upozorňujeme, že nastavení webového proxy serveru nebyly nakonfigurovány na tomto zařízení a proto nebyl spuštěn test webového proxy serveru.</span><span class="sxs-lookup"><span data-stu-id="d5239-137">Note that the web proxy settings were not configured on this device, and therefore, the web proxy test was not run.</span></span> <span data-ttu-id="d5239-138">Všechny ostatní testy pro nastavení sítě, DNS server a nastavení času byly úspěšné.</span><span class="sxs-lookup"><span data-stu-id="d5239-138">All the other tests for network settings, DNS server, and time settings were successful.</span></span>
   
    ![spustit diagnostiku 2](./media/storsimple-ova-web-ui-admin/image30.png)

## <a name="generate-a-log-package"></a><span data-ttu-id="d5239-140">Generovat balíček protokolu</span><span class="sxs-lookup"><span data-stu-id="d5239-140">Generate a log package</span></span>
<span data-ttu-id="d5239-141">Balíček protokol se skládá z příslušné protokoly, které vám mohou pomoci Microsoft Support s potíží jakékoli zařízení.</span><span class="sxs-lookup"><span data-stu-id="d5239-141">A log package is comprised of all the relevant logs that can assist Microsoft Support with troubleshooting any device issues.</span></span> <span data-ttu-id="d5239-142">V této verzi můžete generovat balíček protokolu prostřednictvím místního webového uživatelského rozhraní.</span><span class="sxs-lookup"><span data-stu-id="d5239-142">In this release, a log package can be generated via the local web UI.</span></span>

#### <a name="to-generate-the-log-package"></a><span data-ttu-id="d5239-143">Pro vygenerování balíčku protokolu</span><span class="sxs-lookup"><span data-stu-id="d5239-143">To generate the log package</span></span>
1. <span data-ttu-id="d5239-144">V místní webového uživatelského rozhraní, přejděte do **Poradce při potížích s** > **protokoly systému**.</span><span class="sxs-lookup"><span data-stu-id="d5239-144">In the local web UI, go to **Troubleshooting** > **System logs**.</span></span>
   
    ![Generovat balíček protokolu 1](./media/storsimple-ova-web-ui-admin/image31.png)
2. <span data-ttu-id="d5239-146">V dolní části stránky klikněte na tlačítko **vytvořit balíček protokolu**.</span><span class="sxs-lookup"><span data-stu-id="d5239-146">At the bottom of the page, click **Create log package**.</span></span> <span data-ttu-id="d5239-147">Bude vytvořen balíček protokolů systému.</span><span class="sxs-lookup"><span data-stu-id="d5239-147">A package of the system logs will be created.</span></span> <span data-ttu-id="d5239-148">Bude to trvat několik minut.</span><span class="sxs-lookup"><span data-stu-id="d5239-148">This will take a couple of minutes.</span></span>
   
    ![Generovat balíček protokolu 2](./media/storsimple-ova-web-ui-admin/image32.png)
   
    <span data-ttu-id="d5239-150">Po úspěšném vytvoření balíčku se a stránky se aktualizují na označuje čas a datum vytvoření balíčku, budete upozorněni.</span><span class="sxs-lookup"><span data-stu-id="d5239-150">You will be notified after the package is successfully created, and the page will be updated to indicate the time and date when the package was created.</span></span>
   
    ![Generovat balíček protokolu 3](./media/storsimple-ova-web-ui-admin/image33.png)
3. <span data-ttu-id="d5239-152">Klikněte na tlačítko **balíček ke stažení protokolů**.</span><span class="sxs-lookup"><span data-stu-id="d5239-152">Click **Download log package**.</span></span> <span data-ttu-id="d5239-153">Komprimované balíčku budou staženy ve vašem systému.</span><span class="sxs-lookup"><span data-stu-id="d5239-153">A zipped package will be downloaded on your system.</span></span>
   
    ![Generovat balíček protokolu 4](./media/storsimple-ova-web-ui-admin/image34.png)
4. <span data-ttu-id="d5239-155">Můžete rozbalte balíček stažené protokolu a zobrazit soubory protokolů systému.</span><span class="sxs-lookup"><span data-stu-id="d5239-155">You can unzip the downloaded log package and view the system log files.</span></span>

## <a name="shut-down-and-restart-your-device"></a><span data-ttu-id="d5239-156">Vypnutí a restartování zařízení</span><span class="sxs-lookup"><span data-stu-id="d5239-156">Shut down and restart your device</span></span>
<span data-ttu-id="d5239-157">Můžete vypnout nebo restartovat virtuální zařízení pomocí místní webového uživatelského rozhraní.</span><span class="sxs-lookup"><span data-stu-id="d5239-157">You can shut down or restart your virtual device using the local web UI.</span></span> <span data-ttu-id="d5239-158">Jsme doporučujeme před restartováním, trvat svazky nebo sdílené složky do offline režimu na hostiteli a poté zařízení.</span><span class="sxs-lookup"><span data-stu-id="d5239-158">We recommend that before you restart, take the volumes or shares offline on the host and then the device.</span></span> <span data-ttu-id="d5239-159">Tím se minimalizují možnost poškození dat.</span><span class="sxs-lookup"><span data-stu-id="d5239-159">This will minimize any possibility of data corruption.</span></span> 

#### <a name="to-shut-down-your-virtual-device"></a><span data-ttu-id="d5239-160">Vypnutí virtuálního zařízení</span><span class="sxs-lookup"><span data-stu-id="d5239-160">To shut down your virtual device</span></span>
1. <span data-ttu-id="d5239-161">V místní webového uživatelského rozhraní, přejděte do **údržby** > **nastavení napájení**.</span><span class="sxs-lookup"><span data-stu-id="d5239-161">In the local web UI, go to **Maintenance** > **Power settings**.</span></span>
2. <span data-ttu-id="d5239-162">V dolní části stránky klikněte na tlačítko **vypnutí**.</span><span class="sxs-lookup"><span data-stu-id="d5239-162">At the bottom of the page, click **Shutdown**.</span></span>
   
    ![vypnutí zařízení 1](./media/storsimple-ova-web-ui-admin/image36.png)
3. <span data-ttu-id="d5239-164">Zobrazí se upozornění, s informacemi o tom, že vypnutí zařízení přeruší všechny vstupně-výstupní operace které byly v průběhu, což vede výpadku.</span><span class="sxs-lookup"><span data-stu-id="d5239-164">A warning will appear stating that a shutdown of the device will interrupt any IO that were in progress, resulting in a downtime.</span></span> <span data-ttu-id="d5239-165">Klikněte na ikonu zaškrtnutí</span><span class="sxs-lookup"><span data-stu-id="d5239-165">Click the check icon</span></span> ![ikona zaškrtnutí](./media/storsimple-ova-web-ui-admin/image3.png)<span data-ttu-id="d5239-167">.</span><span class="sxs-lookup"><span data-stu-id="d5239-167">.</span></span>
   
    ![upozornění vypnutí zařízení](./media/storsimple-ova-web-ui-admin/image37.png)
   
    <span data-ttu-id="d5239-169">Budete informováni, že byla inicializována ukončení.</span><span class="sxs-lookup"><span data-stu-id="d5239-169">You will be notified that the shutdown has been initiated.</span></span>
   
    ![vypnutí zařízení spuštění](./media/storsimple-ova-web-ui-admin/image38.png)
   
    <span data-ttu-id="d5239-171">Zařízení bude nyní ukončena.</span><span class="sxs-lookup"><span data-stu-id="d5239-171">The device will now shut down.</span></span> <span data-ttu-id="d5239-172">Pokud chcete spustit zařízení, musíte to provést pomocí Správce technologie Hyper-V.</span><span class="sxs-lookup"><span data-stu-id="d5239-172">If you want to start your device, you will need to do that through the Hyper-V Manager.</span></span>

#### <a name="to-restart-your-virtual-device"></a><span data-ttu-id="d5239-173">Restartování virtuálního zařízení</span><span class="sxs-lookup"><span data-stu-id="d5239-173">To restart your virtual device</span></span>
1. <span data-ttu-id="d5239-174">V místní webového uživatelského rozhraní, přejděte do **údržby** > **nastavení napájení**.</span><span class="sxs-lookup"><span data-stu-id="d5239-174">In the local web UI, go to **Maintenance** > **Power settings**.</span></span>
2. <span data-ttu-id="d5239-175">V dolní části stránky klikněte na tlačítko **restartujte**.</span><span class="sxs-lookup"><span data-stu-id="d5239-175">At the bottom of the page, click **Restart**.</span></span>
   
    ![restartování zařízení](./media/storsimple-ova-web-ui-admin/image36.png)
3. <span data-ttu-id="d5239-177">Zobrazí se upozornění, s informacemi o tom, že restartování zařízení přeruší všechny IOs, které byly v průběhu, což vede výpadku.</span><span class="sxs-lookup"><span data-stu-id="d5239-177">A warning will appear stating that restarting the device will interrupt any IOs that were in progress, resulting in a downtime.</span></span> <span data-ttu-id="d5239-178">Klikněte na ikonu zaškrtnutí</span><span class="sxs-lookup"><span data-stu-id="d5239-178">Click the check icon</span></span> ![ikona zaškrtnutí](./media/storsimple-ova-web-ui-admin/image3.png)<span data-ttu-id="d5239-180">.</span><span class="sxs-lookup"><span data-stu-id="d5239-180">.</span></span>
   
    ![upozornění na restartování](./media/storsimple-ova-web-ui-admin/image37.png)
   
    <span data-ttu-id="d5239-182">Budete informováni, zda bylo zahájeno restartování.</span><span class="sxs-lookup"><span data-stu-id="d5239-182">You will be notified that the restart has been initiated.</span></span>
   
    ![restartování, inicializovat](./media/storsimple-ova-web-ui-admin/image39.png)
   
    <span data-ttu-id="d5239-184">Když probíhá restartování, dojde ke ztrátě připojení k rozhraní.</span><span class="sxs-lookup"><span data-stu-id="d5239-184">While the restart is in progress, you will lose the connection to the UI.</span></span> <span data-ttu-id="d5239-185">Pravidelně aktualizujte uživatelské rozhraní můžete sledovat na restartování.</span><span class="sxs-lookup"><span data-stu-id="d5239-185">You can monitor the restart by refreshing the UI periodically.</span></span> <span data-ttu-id="d5239-186">Alternativně můžete sledovat stav restartování zařízení pomocí Správce technologie Hyper-V.</span><span class="sxs-lookup"><span data-stu-id="d5239-186">Alternatively, you can monitor the device restart status through the Hyper-V Manager.</span></span>

## <a name="next-steps"></a><span data-ttu-id="d5239-187">Další kroky</span><span class="sxs-lookup"><span data-stu-id="d5239-187">Next steps</span></span>
<span data-ttu-id="d5239-188">Zjistěte, jak [používat službu StorSimple Manager ke správě vašich zařízení](storsimple-virtual-array-manager-service-administration.md).</span><span class="sxs-lookup"><span data-stu-id="d5239-188">Learn how to [use the StorSimple Manager service to manage your device](storsimple-virtual-array-manager-service-administration.md).</span></span>

