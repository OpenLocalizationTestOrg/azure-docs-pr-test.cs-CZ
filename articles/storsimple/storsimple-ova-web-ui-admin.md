---
title: "aaaStorSimple virtuální pole webového uživatelského rozhraní správy | Microsoft Docs"
description: "Popisuje, jak Správa základní zařízení tooperform úkoly hello pole virtuální zařízení StorSimple webového uživatelského rozhraní."
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
ms.openlocfilehash: 31a20a587c4302231f027fcf772a50df33b23407
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="use-hello-web-ui-tooadminister-your-storsimple-virtual-array"></a><span data-ttu-id="e76e8-103">Použití webového uživatelského rozhraní tooadminister hello pole virtuální zařízení StorSimple</span><span class="sxs-lookup"><span data-stu-id="e76e8-103">Use hello Web UI tooadminister your StorSimple Virtual Array</span></span>
![tok procesu instalace](./media/storsimple-ova-web-ui-admin/manage4.png)

## <a name="overview"></a><span data-ttu-id="e76e8-105">Přehled</span><span class="sxs-lookup"><span data-stu-id="e76e8-105">Overview</span></span>
<span data-ttu-id="e76e8-106">kurzy Hello v tomto článku se vztahují toohello Microsoft Azure StorSimple virtuální pole (také označované jako hello místní virtuální zařízení StorSimple) spuštěné. března 2016 obecné dostupnosti (GA) verze.</span><span class="sxs-lookup"><span data-stu-id="e76e8-106">hello tutorials in this article apply toohello Microsoft Azure StorSimple Virtual Array (also known as hello StorSimple on-premises virtual device) running March 2016 general availability (GA) release.</span></span> <span data-ttu-id="e76e8-107">Tento článek popisuje některé hello komplexní pracovní postupy a úlohy správy, které lze provést na hello pole virtuální zařízení StorSimple.</span><span class="sxs-lookup"><span data-stu-id="e76e8-107">This article describes some of hello complex workflows and management tasks that can be performed on hello StorSimple Virtual Array.</span></span> <span data-ttu-id="e76e8-108">Můžete spravovat hello poli virtuální zařízení StorSimple pomocí služby StorSimple Manager hello uživatelského rozhraní (označují tooas hello portál uživatelského rozhraní) a hello místního webového uživatelského rozhraní pro hello zařízení.</span><span class="sxs-lookup"><span data-stu-id="e76e8-108">You can manage hello StorSimple Virtual Array using hello StorSimple Manager service UI (referred tooas hello portal UI) and hello local web UI for hello device.</span></span> <span data-ttu-id="e76e8-109">Tento článek se zaměřuje na hello úlohy, že můžete provádět pomocí hello webového uživatelského rozhraní.</span><span class="sxs-lookup"><span data-stu-id="e76e8-109">This article focuses on hello tasks that you can perform using hello web UI.</span></span>

<span data-ttu-id="e76e8-110">Tento článek obsahuje hello následující kurzy:</span><span class="sxs-lookup"><span data-stu-id="e76e8-110">This article includes hello following tutorials:</span></span>

* <span data-ttu-id="e76e8-111">Získání šifrovacího klíče dat služby hello</span><span class="sxs-lookup"><span data-stu-id="e76e8-111">Get hello service data encryption key</span></span>
* <span data-ttu-id="e76e8-112">Řešení chyb instalace webového uživatelského rozhraní</span><span class="sxs-lookup"><span data-stu-id="e76e8-112">Troubleshoot web UI setup errors</span></span>
* <span data-ttu-id="e76e8-113">Generovat balíček protokolu</span><span class="sxs-lookup"><span data-stu-id="e76e8-113">Generate a log package</span></span>
* <span data-ttu-id="e76e8-114">Vypnutí nebo restartování zařízení</span><span class="sxs-lookup"><span data-stu-id="e76e8-114">Shut down or restart your device</span></span>

## <a name="get-hello-service-data-encryption-key"></a><span data-ttu-id="e76e8-115">Získání šifrovacího klíče dat služby hello</span><span class="sxs-lookup"><span data-stu-id="e76e8-115">Get hello service data encryption key</span></span>
<span data-ttu-id="e76e8-116">Šifrovací klíč dat služby se vygeneruje, když se zaregistrujete svoje první zařízení hello služby StorSimple Manager.</span><span class="sxs-lookup"><span data-stu-id="e76e8-116">A service data encryption key is generated when you register your first device with hello StorSimple Manager service.</span></span> <span data-ttu-id="e76e8-117">Tento klíč je pak potřeba s hello služby registrace klíče tooregister další zařízení s hello služby StorSimple Manager.</span><span class="sxs-lookup"><span data-stu-id="e76e8-117">This key is then required with hello service registration key tooregister additional devices with hello StorSimple Manager service.</span></span>

<span data-ttu-id="e76e8-118">Pokud jste k jejich chybnému umístění šifrovacího klíče dat služby a potřebovat tooretrieve, proveďte následující hello kroky v hello místního webového uživatelského rozhraní hello zařízení zaregistrovali služby.</span><span class="sxs-lookup"><span data-stu-id="e76e8-118">If you have misplaced your service data encryption key and need tooretrieve it, perform hello following steps in hello local web UI of hello device registered with your service.</span></span>

#### <a name="tooget-hello-service-data-encryption-key"></a><span data-ttu-id="e76e8-119">šifrovací klíč dat služby tooget hello</span><span class="sxs-lookup"><span data-stu-id="e76e8-119">tooget hello service data encryption key</span></span>
1. <span data-ttu-id="e76e8-120">Připojte toohello místního webového uživatelského rozhraní.</span><span class="sxs-lookup"><span data-stu-id="e76e8-120">Connect toohello local web UI.</span></span> <span data-ttu-id="e76e8-121">Přejděte příliš**konfigurace** > **nastavení cloudu**.</span><span class="sxs-lookup"><span data-stu-id="e76e8-121">Go too**Configuration** > **Cloud Settings**.</span></span>
2. <span data-ttu-id="e76e8-122">V dolní části hello hello stránky, klikněte na tlačítko **šifrovacího klíče dat služby Get**.</span><span class="sxs-lookup"><span data-stu-id="e76e8-122">At hello bottom of hello page, click **Get service data encryption key**.</span></span> <span data-ttu-id="e76e8-123">Zobrazí se klíč.</span><span class="sxs-lookup"><span data-stu-id="e76e8-123">A key will appear.</span></span> <span data-ttu-id="e76e8-124">Zkopírujte a uložte tento klíč.</span><span class="sxs-lookup"><span data-stu-id="e76e8-124">Copy and save this key.</span></span>
   
    ![získání šifrovacího klíče dat služby 1](./media/storsimple-ova-web-ui-admin/image27.png)

## <a name="troubleshoot-web-ui-setup-errors"></a><span data-ttu-id="e76e8-126">Řešení chyb instalace webového uživatelského rozhraní</span><span class="sxs-lookup"><span data-stu-id="e76e8-126">Troubleshoot web UI setup errors</span></span>
<span data-ttu-id="e76e8-127">V některých případech při konfiguraci zařízení hello hello místního webového uživatelského rozhraní, může dojít k chybám.</span><span class="sxs-lookup"><span data-stu-id="e76e8-127">In some instances when you configure hello device through hello local web UI, you might run into errors.</span></span> <span data-ttu-id="e76e8-128">toodiagnose a vyřešte tyto chyby, je možné spustit testy diagnostiky hello.</span><span class="sxs-lookup"><span data-stu-id="e76e8-128">toodiagnose and troubleshoot such errors, you can run hello diagnostics tests.</span></span>

#### <a name="toorun-hello-diagnostic-tests"></a><span data-ttu-id="e76e8-129">toorun hello diagnostických testů</span><span class="sxs-lookup"><span data-stu-id="e76e8-129">toorun hello diagnostic tests</span></span>
1. <span data-ttu-id="e76e8-130">Hello místního webového uživatelského rozhraní, přejděte v příliš**Poradce při potížích s** > **diagnostických testů**.</span><span class="sxs-lookup"><span data-stu-id="e76e8-130">In hello local web UI, go too**Troubleshooting** > **Diagnostic tests**.</span></span>
   
    ![spustit diagnostiku 1](./media/storsimple-ova-web-ui-admin/image29.png)
2. <span data-ttu-id="e76e8-132">V dolní části hello hello stránky, klikněte na tlačítko **spuštění diagnostických testů**.</span><span class="sxs-lookup"><span data-stu-id="e76e8-132">At hello bottom of hello page, click **Run Diagnostic Tests**.</span></span> <span data-ttu-id="e76e8-133">Tím zahájíte testy toodiagnose všech možných problémů s sítí, zařízení, webový proxy server, čas nebo nastavení cloudu.</span><span class="sxs-lookup"><span data-stu-id="e76e8-133">This will initiate tests toodiagnose any possible issues with your network, device, web proxy, time, or cloud settings.</span></span> <span data-ttu-id="e76e8-134">Budete informováni, že toto zařízení hello zpracovává testy.</span><span class="sxs-lookup"><span data-stu-id="e76e8-134">You will be notified that hello device is running tests.</span></span>
3. <span data-ttu-id="e76e8-135">Po dokončení testů hello, zobrazí se výsledky hello.</span><span class="sxs-lookup"><span data-stu-id="e76e8-135">After hello tests have completed, hello results will be displayed.</span></span> <span data-ttu-id="e76e8-136">Hello následující příklad ukazuje výsledky hello diagnostických testů.</span><span class="sxs-lookup"><span data-stu-id="e76e8-136">hello following example shows hello results of diagnostic tests.</span></span> <span data-ttu-id="e76e8-137">Upozorňujeme, že nebyly na toto zařízení nakonfigurované nastavení proxy serveru webové hello a proto nebyl spuštěn test proxy webu hello.</span><span class="sxs-lookup"><span data-stu-id="e76e8-137">Note that hello web proxy settings were not configured on this device, and therefore, hello web proxy test was not run.</span></span> <span data-ttu-id="e76e8-138">Všechny hello jiné testy pro nastavení sítě, DNS server a nastavení času byly úspěšné.</span><span class="sxs-lookup"><span data-stu-id="e76e8-138">All hello other tests for network settings, DNS server, and time settings were successful.</span></span>
   
    ![spustit diagnostiku 2](./media/storsimple-ova-web-ui-admin/image30.png)

## <a name="generate-a-log-package"></a><span data-ttu-id="e76e8-140">Generovat balíček protokolu</span><span class="sxs-lookup"><span data-stu-id="e76e8-140">Generate a log package</span></span>
<span data-ttu-id="e76e8-141">Balíček protokol se skládá z všechny relevantní protokoly hello, které mohou pomoci Microsoft Support s potíží jakékoli zařízení.</span><span class="sxs-lookup"><span data-stu-id="e76e8-141">A log package is comprised of all hello relevant logs that can assist Microsoft Support with troubleshooting any device issues.</span></span> <span data-ttu-id="e76e8-142">V této verzi můžete generovat balíček protokolu pomocí hello místního webového uživatelského rozhraní.</span><span class="sxs-lookup"><span data-stu-id="e76e8-142">In this release, a log package can be generated via hello local web UI.</span></span>

#### <a name="toogenerate-hello-log-package"></a><span data-ttu-id="e76e8-143">toogenerate hello protokolu balíčku</span><span class="sxs-lookup"><span data-stu-id="e76e8-143">toogenerate hello log package</span></span>
1. <span data-ttu-id="e76e8-144">Hello místního webového uživatelského rozhraní, přejděte v příliš**Poradce při potížích s** > **protokoly systému**.</span><span class="sxs-lookup"><span data-stu-id="e76e8-144">In hello local web UI, go too**Troubleshooting** > **System logs**.</span></span>
   
    ![Generovat balíček protokolu 1](./media/storsimple-ova-web-ui-admin/image31.png)
2. <span data-ttu-id="e76e8-146">V dolní části hello hello stránky, klikněte na tlačítko **vytvořit balíček protokolu**.</span><span class="sxs-lookup"><span data-stu-id="e76e8-146">At hello bottom of hello page, click **Create log package**.</span></span> <span data-ttu-id="e76e8-147">Bude vytvořen balíček protokolů systému hello.</span><span class="sxs-lookup"><span data-stu-id="e76e8-147">A package of hello system logs will be created.</span></span> <span data-ttu-id="e76e8-148">Bude to trvat několik minut.</span><span class="sxs-lookup"><span data-stu-id="e76e8-148">This will take a couple of minutes.</span></span>
   
    ![Generovat balíček protokolu 2](./media/storsimple-ova-web-ui-admin/image32.png)
   
    <span data-ttu-id="e76e8-150">Po hello balíček je úspěšně vytvořen a hello stránka bude aktualizována tooindicate hello čas a datum vytvoření balíčku hello, budete upozorněni.</span><span class="sxs-lookup"><span data-stu-id="e76e8-150">You will be notified after hello package is successfully created, and hello page will be updated tooindicate hello time and date when hello package was created.</span></span>
   
    ![Generovat balíček protokolu 3](./media/storsimple-ova-web-ui-admin/image33.png)
3. <span data-ttu-id="e76e8-152">Klikněte na tlačítko **balíček ke stažení protokolů**.</span><span class="sxs-lookup"><span data-stu-id="e76e8-152">Click **Download log package**.</span></span> <span data-ttu-id="e76e8-153">Komprimované balíčku budou staženy ve vašem systému.</span><span class="sxs-lookup"><span data-stu-id="e76e8-153">A zipped package will be downloaded on your system.</span></span>
   
    ![Generovat balíček protokolu 4](./media/storsimple-ova-web-ui-admin/image34.png)
4. <span data-ttu-id="e76e8-155">Můžete rozbalte balíček stažené protokolu hello a zobrazit hello systémových souborech protokolu.</span><span class="sxs-lookup"><span data-stu-id="e76e8-155">You can unzip hello downloaded log package and view hello system log files.</span></span>

## <a name="shut-down-and-restart-your-device"></a><span data-ttu-id="e76e8-156">Vypnutí a restartování zařízení</span><span class="sxs-lookup"><span data-stu-id="e76e8-156">Shut down and restart your device</span></span>
<span data-ttu-id="e76e8-157">Můžete vypnout nebo restartovat virtuální zařízení pomocí místní hello webového uživatelského rozhraní.</span><span class="sxs-lookup"><span data-stu-id="e76e8-157">You can shut down or restart your virtual device using hello local web UI.</span></span> <span data-ttu-id="e76e8-158">Doporučujeme, aby před restartovat, trvat hello svazky nebo sdílené složky do offline režimu na hostiteli hello a pak hello zařízení.</span><span class="sxs-lookup"><span data-stu-id="e76e8-158">We recommend that before you restart, take hello volumes or shares offline on hello host and then hello device.</span></span> <span data-ttu-id="e76e8-159">Tím se minimalizují možnost poškození dat.</span><span class="sxs-lookup"><span data-stu-id="e76e8-159">This will minimize any possibility of data corruption.</span></span> 

#### <a name="tooshut-down-your-virtual-device"></a><span data-ttu-id="e76e8-160">tooshut dolů virtuálního zařízení</span><span class="sxs-lookup"><span data-stu-id="e76e8-160">tooshut down your virtual device</span></span>
1. <span data-ttu-id="e76e8-161">Hello místního webového uživatelského rozhraní, přejděte v příliš**údržby** > **nastavení napájení**.</span><span class="sxs-lookup"><span data-stu-id="e76e8-161">In hello local web UI, go too**Maintenance** > **Power settings**.</span></span>
2. <span data-ttu-id="e76e8-162">V dolní části hello hello stránky, klikněte na tlačítko **vypnutí**.</span><span class="sxs-lookup"><span data-stu-id="e76e8-162">At hello bottom of hello page, click **Shutdown**.</span></span>
   
    ![vypnutí zařízení 1](./media/storsimple-ova-web-ui-admin/image36.png)
3. <span data-ttu-id="e76e8-164">Zobrazí se upozornění, s informacemi o tom, že vypnutí hello zařízení přeruší všechny vstupně-výstupní operace které byly v průběhu, což vede výpadku.</span><span class="sxs-lookup"><span data-stu-id="e76e8-164">A warning will appear stating that a shutdown of hello device will interrupt any IO that were in progress, resulting in a downtime.</span></span> <span data-ttu-id="e76e8-165">Klikněte na ikonu zaškrtnutí hello</span><span class="sxs-lookup"><span data-stu-id="e76e8-165">Click hello check icon</span></span> ![ikona zaškrtnutí](./media/storsimple-ova-web-ui-admin/image3.png)<span data-ttu-id="e76e8-167">.</span><span class="sxs-lookup"><span data-stu-id="e76e8-167">.</span></span>
   
    ![upozornění vypnutí zařízení](./media/storsimple-ova-web-ui-admin/image37.png)
   
    <span data-ttu-id="e76e8-169">Budete informováni, že tento vypnout hello byla inicializována.</span><span class="sxs-lookup"><span data-stu-id="e76e8-169">You will be notified that hello shutdown has been initiated.</span></span>
   
    ![vypnutí zařízení spuštění](./media/storsimple-ova-web-ui-admin/image38.png)
   
    <span data-ttu-id="e76e8-171">Hello zařízení bude nyní ukončena.</span><span class="sxs-lookup"><span data-stu-id="e76e8-171">hello device will now shut down.</span></span> <span data-ttu-id="e76e8-172">Pokud chcete toostart zařízení, budete potřebovat toodo, která prostřednictvím hello Správce technologie Hyper-V.</span><span class="sxs-lookup"><span data-stu-id="e76e8-172">If you want toostart your device, you will need toodo that through hello Hyper-V Manager.</span></span>

#### <a name="toorestart-your-virtual-device"></a><span data-ttu-id="e76e8-173">toorestart virtuální zařízení</span><span class="sxs-lookup"><span data-stu-id="e76e8-173">toorestart your virtual device</span></span>
1. <span data-ttu-id="e76e8-174">Hello místního webového uživatelského rozhraní, přejděte v příliš**údržby** > **nastavení napájení**.</span><span class="sxs-lookup"><span data-stu-id="e76e8-174">In hello local web UI, go too**Maintenance** > **Power settings**.</span></span>
2. <span data-ttu-id="e76e8-175">V dolní části hello hello stránky, klikněte na tlačítko **restartujte**.</span><span class="sxs-lookup"><span data-stu-id="e76e8-175">At hello bottom of hello page, click **Restart**.</span></span>
   
    ![restartování zařízení](./media/storsimple-ova-web-ui-admin/image36.png)
3. <span data-ttu-id="e76e8-177">Zobrazí se upozornění, oznamující, že toto restartování zařízení hello přeruší všechny IOs, které byly v průběhu, což vede výpadku.</span><span class="sxs-lookup"><span data-stu-id="e76e8-177">A warning will appear stating that restarting hello device will interrupt any IOs that were in progress, resulting in a downtime.</span></span> <span data-ttu-id="e76e8-178">Klikněte na ikonu zaškrtnutí hello</span><span class="sxs-lookup"><span data-stu-id="e76e8-178">Click hello check icon</span></span> ![ikona zaškrtnutí](./media/storsimple-ova-web-ui-admin/image3.png)<span data-ttu-id="e76e8-180">.</span><span class="sxs-lookup"><span data-stu-id="e76e8-180">.</span></span>
   
    ![upozornění na restartování](./media/storsimple-ova-web-ui-admin/image37.png)
   
    <span data-ttu-id="e76e8-182">Budete informováni, bylo zahájeno restartování hello.</span><span class="sxs-lookup"><span data-stu-id="e76e8-182">You will be notified that hello restart has been initiated.</span></span>
   
    ![restartování, inicializovat](./media/storsimple-ova-web-ui-admin/image39.png)
   
    <span data-ttu-id="e76e8-184">Když probíhá hello restartování, dojde ke ztrátě připojení toohello hello uživatelského rozhraní.</span><span class="sxs-lookup"><span data-stu-id="e76e8-184">While hello restart is in progress, you will lose hello connection toohello UI.</span></span> <span data-ttu-id="e76e8-185">Pravidelně aktualizujte hello uživatelského rozhraní můžete monitorovat hello restartování.</span><span class="sxs-lookup"><span data-stu-id="e76e8-185">You can monitor hello restart by refreshing hello UI periodically.</span></span> <span data-ttu-id="e76e8-186">Alternativně můžete monitorovat stav restartování zařízení hello prostřednictvím hello Správce technologie Hyper-V.</span><span class="sxs-lookup"><span data-stu-id="e76e8-186">Alternatively, you can monitor hello device restart status through hello Hyper-V Manager.</span></span>

## <a name="next-steps"></a><span data-ttu-id="e76e8-187">Další kroky</span><span class="sxs-lookup"><span data-stu-id="e76e8-187">Next steps</span></span>
<span data-ttu-id="e76e8-188">Zjistěte, jak příliš[použití hello toomanage služby StorSimple Manager zařízení](storsimple-virtual-array-manager-service-administration.md).</span><span class="sxs-lookup"><span data-stu-id="e76e8-188">Learn how too[use hello StorSimple Manager service toomanage your device](storsimple-virtual-array-manager-service-administration.md).</span></span>

