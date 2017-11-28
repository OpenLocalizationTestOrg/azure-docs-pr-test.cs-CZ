---
title: "Instalaci aktualizací v poli virtuální zařízení StorSimple | Microsoft Docs"
description: "Popisuje způsob použití pole virtuální zařízení StorSimple webového uživatelského rozhraní na aktualizace pomocí metody portál a opravy hotfix"
services: storsimple
documentationcenter: NA
author: alkohli
manager: carmonm
editor: 
ms.assetid: 7f1b1e7d-04d0-4ca2-9dbb-77077ff19bb9
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 09/07/2016
ms.author: alkohli
ms.openlocfilehash: bccb0d49c1959a690d513961c32d946763385a87
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="install-updates-on-your-storsimple-virtual-array"></a><span data-ttu-id="25154-103">Instalaci aktualizací do pole virtuální zařízení StorSimple</span><span class="sxs-lookup"><span data-stu-id="25154-103">Install Updates on your StorSimple Virtual Array</span></span>
## <a name="overview"></a><span data-ttu-id="25154-104">Přehled</span><span class="sxs-lookup"><span data-stu-id="25154-104">Overview</span></span>
<span data-ttu-id="25154-105">Tento článek popisuje kroky potřebné k instalaci aktualizací do pole virtuální zařízení StorSimple prostřednictvím místního webového uživatelského rozhraní a prostřednictvím portálu Azure classic.</span><span class="sxs-lookup"><span data-stu-id="25154-105">This article describes the steps required to install updates on your StorSimple Virtual Array via the local web UI and via the Azure classic portal.</span></span> <span data-ttu-id="25154-106">Budete muset použít aktualizace softwaru nebo opravy hotfix, které k zachování aktualizovaného stavu pole virtuální zařízení StorSimple.</span><span class="sxs-lookup"><span data-stu-id="25154-106">You need to apply software updates or hotfixes to keep your StorSimple Virtual Array up-to-date.</span></span> 

<span data-ttu-id="25154-107">Mějte na paměti, který instaluje aktualizaci nebo opravu hotfix restartuje vaše zařízení.</span><span class="sxs-lookup"><span data-stu-id="25154-107">Keep in mind that installing an update or hotfix restarts your device.</span></span> <span data-ttu-id="25154-108">Vzhledem k tomu, že pole virtuální zařízení StorSimple je jeden uzel zařízení, dojde k narušení všechny vstupně-výstupních operací v průběhu a zařízení dojde k výpadku.</span><span class="sxs-lookup"><span data-stu-id="25154-108">Given that the StorSimple Virtual Array is a single node device, any I/O in progress is disrupted and your device experiences downtime.</span></span> 

<span data-ttu-id="25154-109">Než použijete aktualizaci, doporučujeme vám věnovat svazky nebo sdílené složky do offline režimu na hostiteli první a pak zařízení.</span><span class="sxs-lookup"><span data-stu-id="25154-109">Before you apply an update, we recommend that you take the volumes or shares offline on the host first and then the device.</span></span> <span data-ttu-id="25154-110">Tím se minimalizují možnost poškození dat.</span><span class="sxs-lookup"><span data-stu-id="25154-110">This minimizes any possibility of data corruption.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="25154-111">Pokud používáte Update 0.1 nebo verze GA softwaru, musíte použít metodu opravu hotfix prostřednictvím místního webového uživatelského rozhraní pro instalaci aktualizace 0.3.</span><span class="sxs-lookup"><span data-stu-id="25154-111">If you are running Update 0.1 or GA software versions, you must use the hotfix method via the local web UI to install update 0.3.</span></span> <span data-ttu-id="25154-112">Pokud používáte aktualizace 0,2, doporučujeme vám, že instalujete aktualizace prostřednictvím portálu Azure classic.</span><span class="sxs-lookup"><span data-stu-id="25154-112">If you are running Update 0.2, we recommend that you install the updates via the Azure classic portal.</span></span>
> 
> 

## <a name="use-the-local-web-ui"></a><span data-ttu-id="25154-113">Použití místního webového uživatelského rozhraní</span><span class="sxs-lookup"><span data-stu-id="25154-113">Use the local web UI</span></span>
<span data-ttu-id="25154-114">Existují dva kroky při použití místního webového uživatelského rozhraní:</span><span class="sxs-lookup"><span data-stu-id="25154-114">There are two steps when using the local web UI:</span></span>

* <span data-ttu-id="25154-115">Stáhnout aktualizace nebo opravy hotfix</span><span class="sxs-lookup"><span data-stu-id="25154-115">Download the update or the hotfix</span></span>
* <span data-ttu-id="25154-116">Instalovat aktualizace nebo opravy hotfix</span><span class="sxs-lookup"><span data-stu-id="25154-116">Install the update or the hotfix</span></span>

### <a name="download-the-update-or-the-hotfix"></a><span data-ttu-id="25154-117">Stáhnout aktualizace nebo opravy hotfix</span><span class="sxs-lookup"><span data-stu-id="25154-117">Download the update or the hotfix</span></span>
<span data-ttu-id="25154-118">Provedením následujících kroků si stáhněte aktualizace softwaru z Katalogu služby Microsoft Update.</span><span class="sxs-lookup"><span data-stu-id="25154-118">Perform the following steps to download the software update from the Microsoft Update Catalog.</span></span>

#### <a name="to-download-the-update-or-the-hotfix"></a><span data-ttu-id="25154-119">Chcete-li stáhnout aktualizace nebo opravy hotfix</span><span class="sxs-lookup"><span data-stu-id="25154-119">To download the update or the hotfix</span></span>
1. <span data-ttu-id="25154-120">Spusťte Internet Explorer a přejděte na [http://catalog.update.microsoft.com](http://catalog.update.microsoft.com).</span><span class="sxs-lookup"><span data-stu-id="25154-120">Start Internet Explorer and navigate to [http://catalog.update.microsoft.com](http://catalog.update.microsoft.com).</span></span>
2. <span data-ttu-id="25154-121">Pokud na tomto počítači používáte Katalog služby Microsoft Update poprvé, po zobrazení výzvy k instalaci doplňku Katalog služby Microsoft Update klikněte na **Nainstalovat**.</span><span class="sxs-lookup"><span data-stu-id="25154-121">If this is your first time using the Microsoft Update Catalog on this computer, click **Install** when prompted to install the Microsoft Update Catalog add-on.</span></span>
3. <span data-ttu-id="25154-122">Do vyhledávacího pole katalogu služby Microsoft Update zadejte číslo znalostní báze Knowledge Base (KB) opravy hotfix, které chcete stáhnout.</span><span class="sxs-lookup"><span data-stu-id="25154-122">In the search box of the Microsoft Update Catalog, enter the Knowledge Base (KB) number of the hotfix you want to download.</span></span> <span data-ttu-id="25154-123">Zadejte **3182061** pro aktualizace 0.3 a pak klikněte na tlačítko **vyhledávání**.</span><span class="sxs-lookup"><span data-stu-id="25154-123">Enter **3182061** for Update 0.3, and then click **Search**.</span></span>
   
    <span data-ttu-id="25154-124">Zobrazí se seznam oprav hotfix, například **StorSimple virtuální pole Update 0.3**.</span><span class="sxs-lookup"><span data-stu-id="25154-124">The hotfix listing appears, for example, **StorSimple Virtual Array Update 0.3**.</span></span>
   
    ![Prohledávání katalogu](./media/storsimple-ova-install-update-01/download1.png)
4. <span data-ttu-id="25154-126">Klikněte na tlačítko **Přidat**.</span><span class="sxs-lookup"><span data-stu-id="25154-126">Click **Add**.</span></span> <span data-ttu-id="25154-127">Aktualizace se přidá do košíku.</span><span class="sxs-lookup"><span data-stu-id="25154-127">The update is added to the basket.</span></span>
5. <span data-ttu-id="25154-128">Klikněte na **Zobrazit košík**.</span><span class="sxs-lookup"><span data-stu-id="25154-128">Click **View Basket**.</span></span>
6. <span data-ttu-id="25154-129">Klikněte na **Stáhnout**.</span><span class="sxs-lookup"><span data-stu-id="25154-129">Click **Download**.</span></span> <span data-ttu-id="25154-130">Zadejte místní umístění, do kterého chcete aktualizace stáhnout, nebo do něj přejděte pomocí tlačítka **Procházet**.</span><span class="sxs-lookup"><span data-stu-id="25154-130">Specify or **Browse** to a local location where you want the downloads to appear.</span></span> <span data-ttu-id="25154-131">Aktualizace se stáhnou do zadaného umístění do podsložky se stejným názvem, jako má aktualizace.</span><span class="sxs-lookup"><span data-stu-id="25154-131">The updates are downloaded to the specified location and placed in a subfolder with the same name as the update.</span></span> <span data-ttu-id="25154-132">Složku je také možné zkopírovat do sdílené síťové složky dostupné ze zařízení.</span><span class="sxs-lookup"><span data-stu-id="25154-132">The folder can also be copied to a network share that is reachable from the device.</span></span>
7. <span data-ttu-id="25154-133">Otevřete složku, zkopírovaný, měli byste vidět soubor balíčku samostatné aktualizace Microsoft `WindowsTH-KB3011067-x64`.</span><span class="sxs-lookup"><span data-stu-id="25154-133">Open the copied folder, you should see a Microsoft Update Standalone Package file `WindowsTH-KB3011067-x64`.</span></span> <span data-ttu-id="25154-134">Tento soubor se používá k instalaci, aktualizaci nebo opravu hotfix.</span><span class="sxs-lookup"><span data-stu-id="25154-134">This file is used to install the update or hotfix.</span></span>

### <a name="install-the-update-or-the-hotfix"></a><span data-ttu-id="25154-135">Instalovat aktualizace nebo opravy hotfix</span><span class="sxs-lookup"><span data-stu-id="25154-135">Install the update or the hotfix</span></span>
<span data-ttu-id="25154-136">Před instalací aktualizace nebo opravy hotfix ověřte, že aktualizaci nebo opravu hotfix stažené buď místně na vašem hostiteli nebo přístupné přes síťové sdílené složky.</span><span class="sxs-lookup"><span data-stu-id="25154-136">Prior to the update or hotfix installation, make sure that you have the update or the hotfix downloaded either locally on your host or accessible via a network share.</span></span> 

<span data-ttu-id="25154-137">Tuto metodu použijte k instalaci aktualizací do zařízení se systémem GA nebo aktualizovat 0,1 verze softwaru.</span><span class="sxs-lookup"><span data-stu-id="25154-137">Use this method to install updates on a device running GA or Update 0.1 software versions.</span></span> <span data-ttu-id="25154-138">Tento postup trvá méně než 2 minut.</span><span class="sxs-lookup"><span data-stu-id="25154-138">This procedure takes less than 2 minutes to complete.</span></span> <span data-ttu-id="25154-139">Proveďte následující kroky k instalaci, aktualizaci nebo opravu hotfix.</span><span class="sxs-lookup"><span data-stu-id="25154-139">Perform the following steps to install the update or hotfix.</span></span>

#### <a name="to-install-the-update-or-the-hotfix"></a><span data-ttu-id="25154-140">K instalaci aktualizace nebo opravy hotfix</span><span class="sxs-lookup"><span data-stu-id="25154-140">To install the update or the hotfix</span></span>
1. <span data-ttu-id="25154-141">V místní webového uživatelského rozhraní, přejděte do **údržby** > **aktualizace softwaru**.</span><span class="sxs-lookup"><span data-stu-id="25154-141">In the local web UI, go to **Maintenance** > **Software Update**.</span></span>
   
    ![aktualizace zařízení](./media/storsimple-ova-install-update-01/update1m.png)
2. <span data-ttu-id="25154-143">V **cesta k souboru aktualizace**, zadejte název souboru pro aktualizaci nebo opravu hotfix.</span><span class="sxs-lookup"><span data-stu-id="25154-143">In **Update file path**, enter the file name for the update or the hotfix.</span></span> <span data-ttu-id="25154-144">Můžete také procházet k instalačnímu souboru aktualizaci nebo opravu hotfix Pokud umístěn ve sdílené síťové složce.</span><span class="sxs-lookup"><span data-stu-id="25154-144">You can also browse to the update or hotfix installation file if placed on a network share.</span></span> <span data-ttu-id="25154-145">Klikněte na tlačítko **Použít**.</span><span class="sxs-lookup"><span data-stu-id="25154-145">Click **Apply**.</span></span>
   
    ![aktualizace zařízení](./media/storsimple-ova-install-update-01/update2m.png)
3. <span data-ttu-id="25154-147">Zobrazí se upozornění.</span><span class="sxs-lookup"><span data-stu-id="25154-147">A warning is displayed.</span></span> <span data-ttu-id="25154-148">Zadaný to je jeden uzel zařízení, když tuto aktualizaci nenainstalujete, restartování zařízení a dojde k výpadku.</span><span class="sxs-lookup"><span data-stu-id="25154-148">Given this is a single node device, after the update is applied, the device restarts and there is downtime.</span></span> <span data-ttu-id="25154-149">Klikněte na ikonu zaškrtnutí.</span><span class="sxs-lookup"><span data-stu-id="25154-149">Click the check icon.</span></span>
   
   ![aktualizace zařízení](./media/storsimple-ova-install-update-01/update3m.png)
4. <span data-ttu-id="25154-151">Aktualizace spustí.</span><span class="sxs-lookup"><span data-stu-id="25154-151">The update starts.</span></span> <span data-ttu-id="25154-152">Po úspěšné aktualizaci zařízení restartuje.</span><span class="sxs-lookup"><span data-stu-id="25154-152">After the device is successfully updated, it restarts.</span></span> <span data-ttu-id="25154-153">Místní uživatelské rozhraní není dostupný v této hodnotě duration.</span><span class="sxs-lookup"><span data-stu-id="25154-153">The local UI is not accessible in this duration.</span></span>
   
    ![aktualizace zařízení](./media/storsimple-ova-install-update-01/update5m.png)
5. <span data-ttu-id="25154-155">Po dokončení restartování se otevře **přihlášení** stránky.</span><span class="sxs-lookup"><span data-stu-id="25154-155">After the restart is complete, you are taken to the **Sign in** page.</span></span> <span data-ttu-id="25154-156">Chcete-li ověřit, že zařízení software byl aktualizován, v místní webového uživatelského rozhraní, přejděte na **údržby** > **aktualizace softwaru**.</span><span class="sxs-lookup"><span data-stu-id="25154-156">To verify that the device software has updated, in the local web UI, go to **Maintenance** > **Software Update**.</span></span> <span data-ttu-id="25154-157">Zobrazit software verze musí být **10.0.0.0.0.10288.0** pro Update 0.3.</span><span class="sxs-lookup"><span data-stu-id="25154-157">The displayed software version should be **10.0.0.0.0.10288.0** for Update 0.3.</span></span>
   
   > [!NOTE]
   > <span data-ttu-id="25154-158">Verze softwaru jsme sestavu mírně odlišné způsobem místního webového uživatelského rozhraní a portálu Azure classic.</span><span class="sxs-lookup"><span data-stu-id="25154-158">We report the software versions in a slightly different way in the local web UI and the Azure classic portal.</span></span> <span data-ttu-id="25154-159">Například místní webového uživatelského rozhraní sestavy **10.0.0.0.0.10288** a sestavy portálu Azure classic **10.0.10288.0** pro stejnou verzi.</span><span class="sxs-lookup"><span data-stu-id="25154-159">For example, the local web UI reports **10.0.0.0.0.10288** and the Azure classic portal reports **10.0.10288.0** for the same version.</span></span> 
   > 
   > 
   
    ![aktualizace zařízení](./media/storsimple-ova-install-update-01/update6m.png)

## <a name="use-the-azure-classic-portal"></a><span data-ttu-id="25154-161">Použití portálu Azure classic</span><span class="sxs-lookup"><span data-stu-id="25154-161">Use the Azure classic portal</span></span>
<span data-ttu-id="25154-162">Pokud aktualizace 0,2, doporučujeme instalovat aktualizace prostřednictvím portálu Azure classic.</span><span class="sxs-lookup"><span data-stu-id="25154-162">If running Update 0.2, we recommend that you install updates through the Azure classic portal.</span></span> <span data-ttu-id="25154-163">Portál postup vyžaduje, aby uživatel ke kontrole, stáhněte a nainstalujte aktualizace.</span><span class="sxs-lookup"><span data-stu-id="25154-163">The portal procedure requires the user to scan, download, and then install the updates.</span></span> <span data-ttu-id="25154-164">Tento postup trvá asi 7 minut.</span><span class="sxs-lookup"><span data-stu-id="25154-164">This procedure takes around 7 minutes to complete.</span></span> <span data-ttu-id="25154-165">Proveďte následující kroky k instalaci, aktualizaci nebo opravu hotfix.</span><span class="sxs-lookup"><span data-stu-id="25154-165">Perform the following steps to install the update or hotfix.</span></span>

[!INCLUDE [storsimple-ova-install-update-via-portal](../../includes/storsimple-ova-install-update-via-portal.md)]

<span data-ttu-id="25154-166">Po dokončení instalace (jako je označené podle stavu úlohy na 100 %), přejděte na **zařízení > údržby > aktualizací softwaru**.</span><span class="sxs-lookup"><span data-stu-id="25154-166">After the installation is complete (as indicated by job status at 100 %), go to **Devices > Maintenance > Software Updates**.</span></span> <span data-ttu-id="25154-167">Verze zobrazených softwaru by měl být 10.0.10288.0.</span><span class="sxs-lookup"><span data-stu-id="25154-167">The displayed software version should be 10.0.10288.0.</span></span>

![aktualizace zařízení](./media/storsimple-ova-install-update-01/azupdate12m.png)

## <a name="next-steps"></a><span data-ttu-id="25154-169">Další kroky</span><span class="sxs-lookup"><span data-stu-id="25154-169">Next steps</span></span>
<span data-ttu-id="25154-170">Další informace o [Správa pole virtuální zařízení StorSimple](storsimple-ova-web-ui-admin.md).</span><span class="sxs-lookup"><span data-stu-id="25154-170">Learn more about [administering your StorSimple Virtual Array](storsimple-ova-web-ui-admin.md).</span></span>

