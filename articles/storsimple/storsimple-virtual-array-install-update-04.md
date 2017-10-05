---
title: "Instalaci aktualizací do pole virtuální zařízení StorSimple | Microsoft Docs"
description: "Popisuje způsob použití pole virtuální zařízení StorSimple webového uživatelského rozhraní na aktualizace pomocí metody Azure portal a opravy hotfix"
services: storsimple
documentationcenter: NA
author: alkohli
manager: timlt
editor: 
ms.assetid: 
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 02/07/2017
ms.author: alkohli
ms.openlocfilehash: 3fb246b1515e7a637e6cff6499bf324c3f80dd45
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="install-update-04-on-your-storsimple-virtual-array"></a><span data-ttu-id="c0fa5-103">Nainstalujte aktualizace 0.4 na pole virtuální zařízení StorSimple</span><span class="sxs-lookup"><span data-stu-id="c0fa5-103">Install Update 0.4 on your StorSimple Virtual Array</span></span>

## <a name="overview"></a><span data-ttu-id="c0fa5-104">Přehled</span><span class="sxs-lookup"><span data-stu-id="c0fa5-104">Overview</span></span>

<span data-ttu-id="c0fa5-105">Tento článek popisuje kroky potřebné k instalaci aktualizace 0.4 pole virtuální zařízení StorSimple prostřednictvím místního webového uživatelského rozhraní a prostřednictvím portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="c0fa5-105">This article describes the steps required to install Update 0.4 on your StorSimple Virtual Array via the local web UI and via the Azure portal.</span></span> <span data-ttu-id="c0fa5-106">Budete muset použít aktualizace softwaru nebo opravy hotfix, které k zachování aktualizovaného stavu pole virtuální zařízení StorSimple.</span><span class="sxs-lookup"><span data-stu-id="c0fa5-106">You need to apply software updates or hotfixes to keep your StorSimple Virtual Array up-to-date.</span></span> 

<span data-ttu-id="c0fa5-107">Mějte na paměti, který instaluje aktualizaci nebo opravu hotfix restartuje vaše zařízení.</span><span class="sxs-lookup"><span data-stu-id="c0fa5-107">Keep in mind that installing an update or hotfix restarts your device.</span></span> <span data-ttu-id="c0fa5-108">Vzhledem k tomu, že pole virtuální zařízení StorSimple je jeden uzel zařízení, dojde k narušení všechny vstupně-výstupních operací v průběhu a zařízení dojde k výpadku.</span><span class="sxs-lookup"><span data-stu-id="c0fa5-108">Given that the StorSimple Virtual Array is a single node device, any I/O in progress is disrupted and your device experiences downtime.</span></span> 

<span data-ttu-id="c0fa5-109">Než použijete aktualizaci, doporučujeme vám věnovat svazky nebo sdílené složky do offline režimu na hostiteli první a pak zařízení.</span><span class="sxs-lookup"><span data-stu-id="c0fa5-109">Before you apply an update, we recommend that you take the volumes or shares offline on the host first and then the device.</span></span> <span data-ttu-id="c0fa5-110">Tím se minimalizují možnost poškození dat.</span><span class="sxs-lookup"><span data-stu-id="c0fa5-110">This minimizes any possibility of data corruption.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="c0fa5-111">Pokud používáte Update 0.1 nebo verze GA softwaru, musíte použít metodu opravu hotfix prostřednictvím místního webového uživatelského rozhraní pro instalaci aktualizace 0.3.</span><span class="sxs-lookup"><span data-stu-id="c0fa5-111">If you are running Update 0.1 or GA software versions, you must use the hotfix method via the local web UI to install update 0.3.</span></span> <span data-ttu-id="c0fa5-112">Pokud používáte verzi Update 0,2 nebo novější, doporučujeme, aby instalujete aktualizace prostřednictvím portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="c0fa5-112">If you are running Update 0.2 or later, we recommend that you install the updates via the Azure portal.</span></span>
 

## <a name="use-the-local-web-ui"></a><span data-ttu-id="c0fa5-113">Použití místního webového uživatelského rozhraní</span><span class="sxs-lookup"><span data-stu-id="c0fa5-113">Use the local web UI</span></span>

<span data-ttu-id="c0fa5-114">Existují dva kroky při použití místního webového uživatelského rozhraní:</span><span class="sxs-lookup"><span data-stu-id="c0fa5-114">There are two steps when using the local web UI:</span></span>

* <span data-ttu-id="c0fa5-115">Stáhnout aktualizace nebo opravy hotfix</span><span class="sxs-lookup"><span data-stu-id="c0fa5-115">Download the update or the hotfix</span></span>
* <span data-ttu-id="c0fa5-116">Instalovat aktualizace nebo opravy hotfix</span><span class="sxs-lookup"><span data-stu-id="c0fa5-116">Install the update or the hotfix</span></span>

### <a name="download-the-update-or-the-hotfix"></a><span data-ttu-id="c0fa5-117">Stáhnout aktualizace nebo opravy hotfix</span><span class="sxs-lookup"><span data-stu-id="c0fa5-117">Download the update or the hotfix</span></span>

<span data-ttu-id="c0fa5-118">Provedením následujících kroků si stáhněte aktualizace softwaru z Katalogu služby Microsoft Update.</span><span class="sxs-lookup"><span data-stu-id="c0fa5-118">Perform the following steps to download the software update from the Microsoft Update Catalog.</span></span>

#### <a name="to-download-the-update-or-the-hotfix"></a><span data-ttu-id="c0fa5-119">Chcete-li stáhnout aktualizace nebo opravy hotfix</span><span class="sxs-lookup"><span data-stu-id="c0fa5-119">To download the update or the hotfix</span></span>

1. <span data-ttu-id="c0fa5-120">Spusťte Internet Explorer a přejděte na [http://catalog.update.microsoft.com](http://catalog.update.microsoft.com).</span><span class="sxs-lookup"><span data-stu-id="c0fa5-120">Start Internet Explorer and navigate to [http://catalog.update.microsoft.com](http://catalog.update.microsoft.com).</span></span>

2. <span data-ttu-id="c0fa5-121">Pokud na tomto počítači používáte Katalog služby Microsoft Update poprvé, po zobrazení výzvy k instalaci doplňku Katalog služby Microsoft Update klikněte na **Nainstalovat**.</span><span class="sxs-lookup"><span data-stu-id="c0fa5-121">If this is your first time using the Microsoft Update Catalog on this computer, click **Install** when prompted to install the Microsoft Update Catalog add-on.</span></span>

3. <span data-ttu-id="c0fa5-122">Do vyhledávacího pole katalogu služby Microsoft Update zadejte číslo znalostní báze Knowledge Base (KB) opravy hotfix, které chcete stáhnout.</span><span class="sxs-lookup"><span data-stu-id="c0fa5-122">In the search box of the Microsoft Update Catalog, enter the Knowledge Base (KB) number of the hotfix you want to download.</span></span> <span data-ttu-id="c0fa5-123">Zadejte **3216577** pro aktualizace 0.4 a pak klikněte na tlačítko **vyhledávání**.</span><span class="sxs-lookup"><span data-stu-id="c0fa5-123">Enter **3216577** for Update 0.4, and then click **Search**.</span></span>
   
    <span data-ttu-id="c0fa5-124">Zobrazí se seznam oprav hotfix, například **aktualizace pole virtuálního zařízení StorSimple 0.4**.</span><span class="sxs-lookup"><span data-stu-id="c0fa5-124">The hotfix listing appears, for example, **StorSimple Virtual Array Update 0.4**.</span></span>
   
    ![Prohledávání katalogu](./media/storsimple-virtual-array-install-update-04/download1.png)

4. <span data-ttu-id="c0fa5-126">Klikněte na tlačítko **Přidat**.</span><span class="sxs-lookup"><span data-stu-id="c0fa5-126">Click **Add**.</span></span> <span data-ttu-id="c0fa5-127">Aktualizace se přidá do košíku.</span><span class="sxs-lookup"><span data-stu-id="c0fa5-127">The update is added to the basket.</span></span>

5. <span data-ttu-id="c0fa5-128">Klikněte na **Zobrazit košík**.</span><span class="sxs-lookup"><span data-stu-id="c0fa5-128">Click **View Basket**.</span></span>

6. <span data-ttu-id="c0fa5-129">Klikněte na **Stáhnout**.</span><span class="sxs-lookup"><span data-stu-id="c0fa5-129">Click **Download**.</span></span> <span data-ttu-id="c0fa5-130">Zadejte místní umístění, do kterého chcete aktualizace stáhnout, nebo do něj přejděte pomocí tlačítka **Procházet**.</span><span class="sxs-lookup"><span data-stu-id="c0fa5-130">Specify or **Browse** to a local location where you want the downloads to appear.</span></span> <span data-ttu-id="c0fa5-131">Aktualizace se stáhnou do zadaného umístění do podsložky se stejným názvem, jako má aktualizace.</span><span class="sxs-lookup"><span data-stu-id="c0fa5-131">The updates are downloaded to the specified location and placed in a subfolder with the same name as the update.</span></span> <span data-ttu-id="c0fa5-132">Složku je také možné zkopírovat do sdílené síťové složky dostupné ze zařízení.</span><span class="sxs-lookup"><span data-stu-id="c0fa5-132">The folder can also be copied to a network share that is reachable from the device.</span></span>

7. <span data-ttu-id="c0fa5-133">Otevřete složku, zkopírovaný, měli byste vidět soubor balíčku samostatné aktualizace Microsoft `WindowsTH-KB3011067-x64`.</span><span class="sxs-lookup"><span data-stu-id="c0fa5-133">Open the copied folder, you should see a Microsoft Update Standalone Package file `WindowsTH-KB3011067-x64`.</span></span> <span data-ttu-id="c0fa5-134">Tento soubor se používá k instalaci, aktualizaci nebo opravu hotfix.</span><span class="sxs-lookup"><span data-stu-id="c0fa5-134">This file is used to install the update or hotfix.</span></span>

### <a name="install-the-update-or-the-hotfix"></a><span data-ttu-id="c0fa5-135">Instalovat aktualizace nebo opravy hotfix</span><span class="sxs-lookup"><span data-stu-id="c0fa5-135">Install the update or the hotfix</span></span>

<span data-ttu-id="c0fa5-136">Před instalací aktualizace nebo opravy hotfix ověřte, že aktualizaci nebo opravu hotfix stažené buď místně na vašem hostiteli nebo přístupné přes síťové sdílené složky.</span><span class="sxs-lookup"><span data-stu-id="c0fa5-136">Prior to the update or hotfix installation, make sure that you have the update or the hotfix downloaded either locally on your host or accessible via a network share.</span></span> 

<span data-ttu-id="c0fa5-137">Tuto metodu použijte k instalaci aktualizací do zařízení se systémem GA nebo aktualizovat 0,1 verze softwaru.</span><span class="sxs-lookup"><span data-stu-id="c0fa5-137">Use this method to install updates on a device running GA or Update 0.1 software versions.</span></span> <span data-ttu-id="c0fa5-138">Tento postup trvá méně než 2 minut.</span><span class="sxs-lookup"><span data-stu-id="c0fa5-138">This procedure takes less than 2 minutes to complete.</span></span> <span data-ttu-id="c0fa5-139">Proveďte následující kroky k instalaci, aktualizaci nebo opravu hotfix.</span><span class="sxs-lookup"><span data-stu-id="c0fa5-139">Perform the following steps to install the update or hotfix.</span></span>

#### <a name="to-install-the-update-or-the-hotfix"></a><span data-ttu-id="c0fa5-140">K instalaci aktualizace nebo opravy hotfix</span><span class="sxs-lookup"><span data-stu-id="c0fa5-140">To install the update or the hotfix</span></span>

1. <span data-ttu-id="c0fa5-141">V místní webového uživatelského rozhraní, přejděte do **údržby** > **aktualizace softwaru**.</span><span class="sxs-lookup"><span data-stu-id="c0fa5-141">In the local web UI, go to **Maintenance** > **Software Update**.</span></span>
   
    ![aktualizace zařízení](./media/storsimple-virtual-array-install-update/update1m.png)

2. <span data-ttu-id="c0fa5-143">V **cesta k souboru aktualizace**, zadejte název souboru pro aktualizaci nebo opravu hotfix.</span><span class="sxs-lookup"><span data-stu-id="c0fa5-143">In **Update file path**, enter the file name for the update or the hotfix.</span></span> <span data-ttu-id="c0fa5-144">Můžete také procházet k instalačnímu souboru aktualizaci nebo opravu hotfix Pokud umístěn ve sdílené síťové složce.</span><span class="sxs-lookup"><span data-stu-id="c0fa5-144">You can also browse to the update or hotfix installation file if placed on a network share.</span></span> <span data-ttu-id="c0fa5-145">Klikněte na tlačítko **Použít**.</span><span class="sxs-lookup"><span data-stu-id="c0fa5-145">Click **Apply**.</span></span>
   
    ![aktualizace zařízení](./media/storsimple-virtual-array-install-update/update2m.png)

3. <span data-ttu-id="c0fa5-147">Zobrazí se upozornění.</span><span class="sxs-lookup"><span data-stu-id="c0fa5-147">A warning is displayed.</span></span> <span data-ttu-id="c0fa5-148">Zadaný to je jeden uzel zařízení, když tuto aktualizaci nenainstalujete, restartování zařízení a dojde k výpadku.</span><span class="sxs-lookup"><span data-stu-id="c0fa5-148">Given this is a single node device, after the update is applied, the device restarts and there is downtime.</span></span> <span data-ttu-id="c0fa5-149">Klikněte na ikonu zaškrtnutí.</span><span class="sxs-lookup"><span data-stu-id="c0fa5-149">Click the check icon.</span></span>
   
   ![aktualizace zařízení](./media/storsimple-virtual-array-install-update/update3m.png)

4. <span data-ttu-id="c0fa5-151">Aktualizace spustí.</span><span class="sxs-lookup"><span data-stu-id="c0fa5-151">The update starts.</span></span> <span data-ttu-id="c0fa5-152">Po úspěšné aktualizaci zařízení restartuje.</span><span class="sxs-lookup"><span data-stu-id="c0fa5-152">After the device is successfully updated, it restarts.</span></span> <span data-ttu-id="c0fa5-153">Místní uživatelské rozhraní není dostupný v této hodnotě duration.</span><span class="sxs-lookup"><span data-stu-id="c0fa5-153">The local UI is not accessible in this duration.</span></span>
   
    ![aktualizace zařízení](./media/storsimple-virtual-array-install-update/update5m.png)

5. <span data-ttu-id="c0fa5-155">Po dokončení restartování se otevře **přihlášení** stránky.</span><span class="sxs-lookup"><span data-stu-id="c0fa5-155">After the restart is complete, you are taken to the **Sign in** page.</span></span> <span data-ttu-id="c0fa5-156">Chcete-li ověřit, že zařízení software byl aktualizován, v místní webového uživatelského rozhraní, přejděte na **údržby** > **aktualizace softwaru**.</span><span class="sxs-lookup"><span data-stu-id="c0fa5-156">To verify that the device software has updated, in the local web UI, go to **Maintenance** > **Software Update**.</span></span> <span data-ttu-id="c0fa5-157">Zobrazit software verze musí být **10.0.0.0.0.10289.0** pro aktualizaci 0.4.</span><span class="sxs-lookup"><span data-stu-id="c0fa5-157">The displayed software version should be **10.0.0.0.0.10289.0** for Update 0.4.</span></span>
   
   > [!NOTE]
   > <span data-ttu-id="c0fa5-158">Verze softwaru jsme sestavu mírně odlišné způsobem místního webového uživatelského rozhraní a portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="c0fa5-158">We report the software versions in a slightly different way in the local web UI and the Azure portal.</span></span> <span data-ttu-id="c0fa5-159">Například místní webového uživatelského rozhraní sestavy **10.0.0.0.0.10289** a Azure portálu sestavy **10.0.10289.0** pro stejnou verzi.</span><span class="sxs-lookup"><span data-stu-id="c0fa5-159">For example, the local web UI reports **10.0.0.0.0.10289** and the Azure portal reports **10.0.10289.0** for the same version.</span></span>
   
    ![aktualizace zařízení](./media/storsimple-virtual-array-install-update/update6m.png)

## <a name="use-the-azure-portal"></a><span data-ttu-id="c0fa5-161">Použití webu Azure Portal</span><span class="sxs-lookup"><span data-stu-id="c0fa5-161">Use the Azure portal</span></span>

<span data-ttu-id="c0fa5-162">Pokud aktualizace 0,2 a novější, doporučujeme instalovat aktualizace prostřednictvím portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="c0fa5-162">If running Update 0.2 and later, we recommend that you install updates through the Azure portal.</span></span> <span data-ttu-id="c0fa5-163">Portál postup vyžaduje, aby uživatel ke kontrole, stáhněte a nainstalujte aktualizace.</span><span class="sxs-lookup"><span data-stu-id="c0fa5-163">The portal procedure requires the user to scan, download, and then install the updates.</span></span> <span data-ttu-id="c0fa5-164">Tento postup trvá asi 7 minut.</span><span class="sxs-lookup"><span data-stu-id="c0fa5-164">This procedure takes around 7 minutes to complete.</span></span> <span data-ttu-id="c0fa5-165">Proveďte následující kroky k instalaci, aktualizaci nebo opravu hotfix.</span><span class="sxs-lookup"><span data-stu-id="c0fa5-165">Perform the following steps to install the update or hotfix.</span></span>

[!INCLUDE [storsimple-virtual-array-install-update-via-portal](../../includes/storsimple-virtual-array-install-update-via-portal-04.md)]

<span data-ttu-id="c0fa5-166">Po dokončení instalace (jako je označené podle stavu úlohy na 100 %), přejděte do služby StorSimple Manager zařízení.</span><span class="sxs-lookup"><span data-stu-id="c0fa5-166">After the installation is complete (as indicated by job status at 100 %), go to your StorSimple Device Manager service.</span></span> <span data-ttu-id="c0fa5-167">Vyberte **zařízení** a poté vyberte a klikněte na zařízení, které chcete aktualizovat ze seznamu zařízení připojená k této službě.</span><span class="sxs-lookup"><span data-stu-id="c0fa5-167">Select **Devices** and then select and click the device you want to update from the list of devices connected to this service.</span></span> <span data-ttu-id="c0fa5-168">V **nastavení** okno, přejděte na **spravovat** a vyberte **aktualizace zařízení**.</span><span class="sxs-lookup"><span data-stu-id="c0fa5-168">In the **Settings** blade, go to **Manage** section and select **Device updates**.</span></span> <span data-ttu-id="c0fa5-169">Zobrazit software verze musí být **10.0.10289.0**.</span><span class="sxs-lookup"><span data-stu-id="c0fa5-169">The displayed software version should be **10.0.10289.0**.</span></span>


## <a name="next-steps"></a><span data-ttu-id="c0fa5-170">Další kroky</span><span class="sxs-lookup"><span data-stu-id="c0fa5-170">Next steps</span></span>

<span data-ttu-id="c0fa5-171">Další informace o [Správa pole virtuální zařízení StorSimple](storsimple-ova-web-ui-admin.md).</span><span class="sxs-lookup"><span data-stu-id="c0fa5-171">Learn more about [administering your StorSimple Virtual Array](storsimple-ova-web-ui-admin.md).</span></span>

