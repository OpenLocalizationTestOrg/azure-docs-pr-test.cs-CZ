---
title: "Instalaci aktualizace 0,6 v poli virtuální zařízení StorSimple | Microsoft Docs"
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
ms.date: 05/18/2017
ms.author: alkohli
ms.openlocfilehash: 111976cd684561f5bc63b92f09a20470fe3036d7
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="install-update-06-on-your-storsimple-virtual-array"></a><span data-ttu-id="a3962-103">Nainstalujte aktualizace 0,6 na pole virtuální zařízení StorSimple</span><span class="sxs-lookup"><span data-stu-id="a3962-103">Install Update 0.6 on your StorSimple Virtual Array</span></span>

## <a name="overview"></a><span data-ttu-id="a3962-104">Přehled</span><span class="sxs-lookup"><span data-stu-id="a3962-104">Overview</span></span>

<span data-ttu-id="a3962-105">Tento článek popisuje kroky potřebné k instalaci aktualizace 0,6 pole virtuální zařízení StorSimple prostřednictvím místního webového uživatelského rozhraní a prostřednictvím portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="a3962-105">This article describes the steps required to install Update 0.6 on your StorSimple Virtual Array via the local web UI and via the Azure portal.</span></span> <span data-ttu-id="a3962-106">Můžete použít aktualizace softwaru nebo opravy hotfix, které k zachování aktualizovaného stavu pole virtuální zařízení StorSimple.</span><span class="sxs-lookup"><span data-stu-id="a3962-106">You apply the software updates or hotfixes to keep your StorSimple Virtual Array up-to-date.</span></span>

<span data-ttu-id="a3962-107">Než použijete aktualizaci, doporučujeme vám věnovat svazky nebo sdílené složky do offline režimu na hostiteli první a pak zařízení.</span><span class="sxs-lookup"><span data-stu-id="a3962-107">Before you apply an update, we recommend that you take the volumes or shares offline on the host first and then the device.</span></span> <span data-ttu-id="a3962-108">Tím se minimalizují možnost poškození dat.</span><span class="sxs-lookup"><span data-stu-id="a3962-108">This minimizes any possibility of data corruption.</span></span> <span data-ttu-id="a3962-109">Jakmile svazky nebo sdílené složky jsou offline, můžete také provést ruční zálohování zařízení.</span><span class="sxs-lookup"><span data-stu-id="a3962-109">After the volumes or shares are offline, you should also take a manual backup of the device.</span></span>

> [!IMPORTANT]
> - <span data-ttu-id="a3962-110">Aktualizace 0,6 odpovídá **10.0.10293.0** verze softwaru ve vašem zařízení.</span><span class="sxs-lookup"><span data-stu-id="a3962-110">Update 0.6 corresponds to **10.0.10293.0** software version on your device.</span></span> <span data-ttu-id="a3962-111">Informace na to, co je nového v této aktualizaci najdete na [poznámky k verzi pro aktualizaci 0,6](storsimple-virtual-array-update-06-release-notes.md).</span><span class="sxs-lookup"><span data-stu-id="a3962-111">For information on what is new in this update, go to [Release notes for Update 0.6](storsimple-virtual-array-update-06-release-notes.md).</span></span>
>
> - <span data-ttu-id="a3962-112">Pokud používáte verzi Update 0,2 nebo novější, doporučujeme, aby instalujete aktualizace prostřednictvím portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="a3962-112">If you are running Update 0.2 or later, we recommend that you install the updates via the Azure portal.</span></span> <span data-ttu-id="a3962-113">Pokud používáte Update 0.1 nebo verze GA softwaru, musíte použít metodu opravu hotfix prostřednictvím místního webového uživatelského rozhraní pro instalaci aktualizace 0,6.</span><span class="sxs-lookup"><span data-stu-id="a3962-113">If you are running Update 0.1 or GA software versions, you must use the hotfix method via the local web UI to install Update 0.6.</span></span>
>
> - <span data-ttu-id="a3962-114">Mějte na paměti, který instaluje aktualizaci nebo opravu hotfix restartuje vaše zařízení.</span><span class="sxs-lookup"><span data-stu-id="a3962-114">Keep in mind that installing an update or hotfix restarts your device.</span></span> <span data-ttu-id="a3962-115">Vzhledem k tomu, že pole virtuální zařízení StorSimple je jeden uzel zařízení, dojde k narušení všechny vstupně-výstupních operací v průběhu a zařízení dojde k výpadku.</span><span class="sxs-lookup"><span data-stu-id="a3962-115">Given that the StorSimple Virtual Array is a single node device, any I/O in progress is disrupted and your device experiences downtime.</span></span>

## <a name="use-the-azure-portal"></a><span data-ttu-id="a3962-116">Použití webu Azure Portal</span><span class="sxs-lookup"><span data-stu-id="a3962-116">Use the Azure portal</span></span>

<span data-ttu-id="a3962-117">Pokud aktualizace 0,2 a novější, doporučujeme instalovat aktualizace prostřednictvím portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="a3962-117">If running Update 0.2 and later, we recommend that you install updates through the Azure portal.</span></span> <span data-ttu-id="a3962-118">Portál postup vyžaduje, aby uživatel ke kontrole, stáhněte a nainstalujte aktualizace.</span><span class="sxs-lookup"><span data-stu-id="a3962-118">The portal procedure requires the user to scan, download, and then install the updates.</span></span> <span data-ttu-id="a3962-119">Tento postup trvá asi 7 minut.</span><span class="sxs-lookup"><span data-stu-id="a3962-119">This procedure takes around 7 minutes to complete.</span></span> <span data-ttu-id="a3962-120">Proveďte následující kroky k instalaci, aktualizaci nebo opravu hotfix.</span><span class="sxs-lookup"><span data-stu-id="a3962-120">Perform the following steps to install the update or hotfix.</span></span>

[!INCLUDE [storsimple-virtual-array-install-update-via-portal](../../includes/storsimple-virtual-array-install-update-via-portal-04.md)]

<span data-ttu-id="a3962-121">Po dokončení instalace, přejděte do služby StorSimple Manager zařízení.</span><span class="sxs-lookup"><span data-stu-id="a3962-121">After the installation is complete, go to your StorSimple Device Manager service.</span></span> <span data-ttu-id="a3962-122">Vyberte **zařízení** a poté vyberte a klikněte na zařízení k aktualizaci.</span><span class="sxs-lookup"><span data-stu-id="a3962-122">Select **Devices** and then select and click the device you just updated.</span></span> <span data-ttu-id="a3962-123">Přejděte na **Nastavení > Správa > aktualizace zařízení**.</span><span class="sxs-lookup"><span data-stu-id="a3962-123">Go to **Settings > Manage > Device Updates**.</span></span> <span data-ttu-id="a3962-124">Zobrazit software verze musí být **10.0.10293.0**.</span><span class="sxs-lookup"><span data-stu-id="a3962-124">The displayed software version should be **10.0.10293.0**.</span></span>

## <a name="use-the-local-web-ui"></a><span data-ttu-id="a3962-125">Použití místního webového uživatelského rozhraní</span><span class="sxs-lookup"><span data-stu-id="a3962-125">Use the local web UI</span></span>

<span data-ttu-id="a3962-126">Existují dva kroky při použití místního webového uživatelského rozhraní:</span><span class="sxs-lookup"><span data-stu-id="a3962-126">There are two steps when using the local web UI:</span></span>

* <span data-ttu-id="a3962-127">Stáhnout aktualizace nebo opravy hotfix</span><span class="sxs-lookup"><span data-stu-id="a3962-127">Download the update or the hotfix</span></span>
* <span data-ttu-id="a3962-128">Instalovat aktualizace nebo opravy hotfix</span><span class="sxs-lookup"><span data-stu-id="a3962-128">Install the update or the hotfix</span></span>

### <a name="download-the-update-or-the-hotfix"></a><span data-ttu-id="a3962-129">Stáhnout aktualizace nebo opravy hotfix</span><span class="sxs-lookup"><span data-stu-id="a3962-129">Download the update or the hotfix</span></span>

<span data-ttu-id="a3962-130">Provedením následujících kroků si stáhněte aktualizace softwaru z Katalogu služby Microsoft Update.</span><span class="sxs-lookup"><span data-stu-id="a3962-130">Perform the following steps to download the software update from the Microsoft Update Catalog.</span></span>

#### <a name="to-download-the-update-or-the-hotfix"></a><span data-ttu-id="a3962-131">Chcete-li stáhnout aktualizace nebo opravy hotfix</span><span class="sxs-lookup"><span data-stu-id="a3962-131">To download the update or the hotfix</span></span>

1. <span data-ttu-id="a3962-132">Spusťte Internet Explorer a přejděte na [http://catalog.update.microsoft.com](http://catalog.update.microsoft.com).</span><span class="sxs-lookup"><span data-stu-id="a3962-132">Start Internet Explorer and navigate to [http://catalog.update.microsoft.com](http://catalog.update.microsoft.com).</span></span>

2. <span data-ttu-id="a3962-133">Pokud používáte katalogu služby Microsoft Update na tomto počítači poprvé, klikněte na tlačítko **nainstalovat** po zobrazení výzvy k instalaci doplňku katalogu služby Microsoft Update.</span><span class="sxs-lookup"><span data-stu-id="a3962-133">If you are using the Microsoft Update Catalog for the first time on this computer, click **Install** when prompted to install the Microsoft Update Catalog add-on.</span></span>

3. <span data-ttu-id="a3962-134">Do vyhledávacího pole katalogu služby Microsoft Update zadejte číslo znalostní báze Knowledge Base (KB) opravy hotfix, které chcete stáhnout.</span><span class="sxs-lookup"><span data-stu-id="a3962-134">In the search box of the Microsoft Update Catalog, enter the Knowledge Base (KB) number of the hotfix you want to download.</span></span> <span data-ttu-id="a3962-135">Zadejte **4023268** pro aktualizace 0,6 a pak klikněte na tlačítko **vyhledávání**.</span><span class="sxs-lookup"><span data-stu-id="a3962-135">Enter **4023268** for Update 0.6, and then click **Search**.</span></span>
   
    <span data-ttu-id="a3962-136">Zobrazí se seznam oprav hotfix, například **aktualizace pole virtuálního zařízení StorSimple 0,6**.</span><span class="sxs-lookup"><span data-stu-id="a3962-136">The hotfix listing appears, for example, **StorSimple Virtual Array Update 0.6**.</span></span>
   
    ![Prohledávání katalogu](./media/storsimple-virtual-array-install-update-06/download1.png)

4. <span data-ttu-id="a3962-138">Klikněte na **Stáhnout**.</span><span class="sxs-lookup"><span data-stu-id="a3962-138">Click **Download**.</span></span>

5. <span data-ttu-id="a3962-139">Měli byste vidět pět souborů ke stažení.</span><span class="sxs-lookup"><span data-stu-id="a3962-139">You should see five files to download.</span></span> <span data-ttu-id="a3962-140">Stáhněte si každý z těchto souborů do složky.</span><span class="sxs-lookup"><span data-stu-id="a3962-140">Download each of those files to a folder.</span></span> <span data-ttu-id="a3962-141">Složku je také možné zkopírovat do sdílené síťové složky dostupné ze zařízení.</span><span class="sxs-lookup"><span data-stu-id="a3962-141">The folder can also be copied to a network share that is reachable from the device.</span></span>

6. <span data-ttu-id="a3962-142">Otevřete složku, kde jsou umístěny soubory.</span><span class="sxs-lookup"><span data-stu-id="a3962-142">Open the folder where the files are located.</span></span>
    <span data-ttu-id="a3962-143">![Soubory v balíčku](./media/storsimple-virtual-array-install-update-06/update06folder.png)</span><span class="sxs-lookup"><span data-stu-id="a3962-143">![Files in the package](./media/storsimple-virtual-array-install-update-06/update06folder.png)</span></span>

    <span data-ttu-id="a3962-144">Zobrazí:</span><span class="sxs-lookup"><span data-stu-id="a3962-144">You see:</span></span>
    -  <span data-ttu-id="a3962-145">Soubor balíčku samostatné aktualizace Microsoft `WindowsTH-KB3011067-x64`.</span><span class="sxs-lookup"><span data-stu-id="a3962-145">A Microsoft Update Standalone Package file `WindowsTH-KB3011067-x64`.</span></span> <span data-ttu-id="a3962-146">Tento soubor se používá k aktualizaci softwaru zařízení.</span><span class="sxs-lookup"><span data-stu-id="a3962-146">This file is used to update the device software.</span></span>
    - <span data-ttu-id="a3962-147">Soubor balíčku agenta monitorování Genevy `GenevaMonitoringAgentPackageInstaller`.</span><span class="sxs-lookup"><span data-stu-id="a3962-147">A Geneva Monitoring Agent Package file `GenevaMonitoringAgentPackageInstaller`.</span></span> <span data-ttu-id="a3962-148">Tento soubor se používá k aktualizaci agenta monitorovací a diagnostické služby (MDS).</span><span class="sxs-lookup"><span data-stu-id="a3962-148">This file is used to update the Monitoring and Diagnostics service (MDS) agent.</span></span> <span data-ttu-id="a3962-149">Poklikejte na soubor cab.</span><span class="sxs-lookup"><span data-stu-id="a3962-149">Double-click the cab file.</span></span> <span data-ttu-id="a3962-150">A _.msi_ se zobrazí.</span><span class="sxs-lookup"><span data-stu-id="a3962-150">A _.msi_ is displayed.</span></span> <span data-ttu-id="a3962-151">Vyberte soubor, klikněte pravým tlačítkem a potom **extrahovat** soubor.</span><span class="sxs-lookup"><span data-stu-id="a3962-151">Select the file, right-click, and then **Extract** the file.</span></span> <span data-ttu-id="a3962-152">Můžete použít _.msi_ soubor aktualizace agenta.</span><span class="sxs-lookup"><span data-stu-id="a3962-152">You use the _.msi_ file to update the agent.</span></span>

        ![Extrahujte soubor aktualizace agenta služby MDS](./media/storsimple-virtual-array-install-update-06/extract-geneva-monitoring-agent-installer.png)

        > [!IMPORTANT]
        > <span data-ttu-id="a3962-154">Není nutné aktualizovat agenta MDS, pokud používáte StorSimple Update 0,5 (0.0.10293.0).</span><span class="sxs-lookup"><span data-stu-id="a3962-154">You do not need to update the MDS agent if you are running StorSimple Update 0.5 (0.0.10293.0).</span></span>

    - <span data-ttu-id="a3962-155">Tři soubory, které obsahují důležité aktualizace zabezpečení systému Windows, `windows8.1-kb4012213-x64`,`windows8.1-kb3205400-x64`, a `windows8.1-kb4019213-x64`.</span><span class="sxs-lookup"><span data-stu-id="a3962-155">Three files that contain critical Windows security updates, `windows8.1-kb4012213-x64`,`windows8.1-kb3205400-x64`, and `windows8.1-kb4019213-x64`.</span></span>


### <a name="install-the-update-or-the-hotfix"></a><span data-ttu-id="a3962-156">Instalovat aktualizace nebo opravy hotfix</span><span class="sxs-lookup"><span data-stu-id="a3962-156">Install the update or the hotfix</span></span>

<span data-ttu-id="a3962-157">Před instalací aktualizace nebo opravy hotfix ověřte, že aktualizaci nebo opravu hotfix stažené buď místně na vašem hostiteli nebo přístupné přes síťové sdílené složky.</span><span class="sxs-lookup"><span data-stu-id="a3962-157">Prior to the update or hotfix installation, make sure that you have the update or the hotfix downloaded either locally on your host or accessible via a network share.</span></span>

<span data-ttu-id="a3962-158">Tuto metodu použijte k instalaci aktualizací do zařízení se systémem GA nebo aktualizovat 0,1 verze softwaru.</span><span class="sxs-lookup"><span data-stu-id="a3962-158">Use this method to install updates on a device running GA or Update 0.1 software versions.</span></span> <span data-ttu-id="a3962-159">Tento postup trvá přibližně 12 minut na dokončení.</span><span class="sxs-lookup"><span data-stu-id="a3962-159">This procedure takes approximately 12 minutes to complete.</span></span> <span data-ttu-id="a3962-160">Proveďte následující kroky k instalaci, aktualizaci nebo opravu hotfix.</span><span class="sxs-lookup"><span data-stu-id="a3962-160">Perform the following steps to install the update or hotfix.</span></span>

#### <a name="to-install-the-update-or-the-hotfix"></a><span data-ttu-id="a3962-161">K instalaci aktualizace nebo opravy hotfix</span><span class="sxs-lookup"><span data-stu-id="a3962-161">To install the update or the hotfix</span></span>

1. <span data-ttu-id="a3962-162">V místní webového uživatelského rozhraní, přejděte do **údržby** > **aktualizace softwaru**.</span><span class="sxs-lookup"><span data-stu-id="a3962-162">In the local web UI, go to **Maintenance** > **Software Update**.</span></span> <span data-ttu-id="a3962-163">Poznamenejte si verzi softwaru, kterou používáte.</span><span class="sxs-lookup"><span data-stu-id="a3962-163">Make a note of the software version that you are running.</span></span> <span data-ttu-id="a3962-164">Pokud používáte **10.0.10290.0**, není nutné aktualizovat agenta MDS v kroku 6.</span><span class="sxs-lookup"><span data-stu-id="a3962-164">If you are running **10.0.10290.0**, you do not need to update the MDS agent in step 6.</span></span>
   
    ![aktualizace zařízení](./media/storsimple-virtual-array-install-update-05/update1m.png)

2. <span data-ttu-id="a3962-166">V **cesta k souboru aktualizace**, zadejte název souboru pro aktualizaci nebo opravu hotfix.</span><span class="sxs-lookup"><span data-stu-id="a3962-166">In **Update file path**, enter the file name for the update or the hotfix.</span></span> <span data-ttu-id="a3962-167">Můžete také procházet k instalačnímu souboru aktualizaci nebo opravu hotfix Pokud umístěn ve sdílené síťové složce.</span><span class="sxs-lookup"><span data-stu-id="a3962-167">You can also browse to the update or hotfix installation file if placed on a network share.</span></span> <span data-ttu-id="a3962-168">Klikněte na tlačítko **Použít**.</span><span class="sxs-lookup"><span data-stu-id="a3962-168">Click **Apply**.</span></span>
   
    ![aktualizace zařízení](./media/storsimple-virtual-array-install-update-05/update2m.png)

3. <span data-ttu-id="a3962-170">Zobrazí se upozornění.</span><span class="sxs-lookup"><span data-stu-id="a3962-170">A warning is displayed.</span></span> <span data-ttu-id="a3962-171">Zadané virtuální pole je jeden uzel zařízení, po tuto aktualizaci nenainstalujete, restartování zařízení a dojde k výpadku.</span><span class="sxs-lookup"><span data-stu-id="a3962-171">Given the virtual array is a single node device, after the update is applied, the device restarts and there is downtime.</span></span> <span data-ttu-id="a3962-172">Klikněte na ikonu zaškrtnutí.</span><span class="sxs-lookup"><span data-stu-id="a3962-172">Click the check icon.</span></span>
   
   ![aktualizace zařízení](./media/storsimple-virtual-array-install-update-05/update3m.png)

4. <span data-ttu-id="a3962-174">Aktualizace spustí.</span><span class="sxs-lookup"><span data-stu-id="a3962-174">The update starts.</span></span> <span data-ttu-id="a3962-175">Po úspěšné aktualizaci zařízení restartuje.</span><span class="sxs-lookup"><span data-stu-id="a3962-175">After the device is successfully updated, it restarts.</span></span> <span data-ttu-id="a3962-176">Místní uživatelské rozhraní není dostupný v této hodnotě duration.</span><span class="sxs-lookup"><span data-stu-id="a3962-176">The local UI is not accessible in this duration.</span></span>
   
    ![aktualizace zařízení](./media/storsimple-virtual-array-install-update-05/update5m.png)

5. <span data-ttu-id="a3962-178">Po dokončení restartování se otevře **přihlášení** stránky.</span><span class="sxs-lookup"><span data-stu-id="a3962-178">After the restart is complete, you are taken to the **Sign in** page.</span></span> <span data-ttu-id="a3962-179">Chcete-li ověřit, že zařízení software byl aktualizován, v místní webového uživatelského rozhraní, přejděte na **údržby** > **aktualizace softwaru**.</span><span class="sxs-lookup"><span data-stu-id="a3962-179">To verify that the device software has updated, in the local web UI, go to **Maintenance** > **Software Update**.</span></span> <span data-ttu-id="a3962-180">Zobrazit software verze musí být **10.0.0.0.0.10293** pro aktualizaci 0,6.</span><span class="sxs-lookup"><span data-stu-id="a3962-180">The displayed software version should be **10.0.0.0.0.10293** for Update 0.6.</span></span>
   
   > [!NOTE]
   > <span data-ttu-id="a3962-181">Verze softwaru jsme sestavu mírně odlišné způsobem místního webového uživatelského rozhraní a portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="a3962-181">We report the software versions in a slightly different way in the local web UI and the Azure portal.</span></span> <span data-ttu-id="a3962-182">Například místní webového uživatelského rozhraní sestavy **10.0.0.0.0.10293** a Azure portálu sestavy **10.0.10293.0** pro stejnou verzi.</span><span class="sxs-lookup"><span data-stu-id="a3962-182">For example, the local web UI reports **10.0.0.0.0.10293** and the Azure portal reports **10.0.10293.0** for the same version.</span></span>
   
    ![aktualizace zařízení](./media/storsimple-virtual-array-install-update-06/update6m.png)

6. <span data-ttu-id="a3962-184">Tento krok přeskočte, pokud byly spuštěny aktualizace pole virtuálního zařízení StorSimple 0,5 (**10.0.10290.0**) před instalací této aktualizace.</span><span class="sxs-lookup"><span data-stu-id="a3962-184">Skip this step if you were running StorSimple Virtual Array Update 0.5 (**10.0.10290.0**) before you applied this update.</span></span> <span data-ttu-id="a3962-185">Před započetím aktualizace provedené v kroku 1 poznamenejte si verzi softwaru.</span><span class="sxs-lookup"><span data-stu-id="a3962-185">You made a note of the software version in step 1 before you began to update.</span></span> <span data-ttu-id="a3962-186">Pokud byly spuštěny aktualizace 0,5, vaše MDS agent je již aktuální.</span><span class="sxs-lookup"><span data-stu-id="a3962-186">If you were running Update 0.5, your MDS agent is already up-to-date .</span></span>

    <span data-ttu-id="a3962-187">Pokud používáte verzi softwaru před aktualizací 0,5, je dalším krokem pro vás aktualizace agenta služby MDS.</span><span class="sxs-lookup"><span data-stu-id="a3962-187">If you are running a software version prior to Update 0.5, the next step for you is to update the MDS agent.</span></span> <span data-ttu-id="a3962-188">V **aktualizace softwaru** stránky, přejděte na **cesta k souboru aktualizace** a přejděte do `GenevaMonitoringAgentPackageInstaller.msi` souboru.</span><span class="sxs-lookup"><span data-stu-id="a3962-188">In the **Software Update** page, go to the **Update file path** and browse to the `GenevaMonitoringAgentPackageInstaller.msi` file.</span></span> <span data-ttu-id="a3962-189">Opakujte kroky 2 – 4.</span><span class="sxs-lookup"><span data-stu-id="a3962-189">Repeat steps 2-4.</span></span> <span data-ttu-id="a3962-190">Po restartování virtuální pole, přihlaste se k místní webového uživatelského rozhraní.</span><span class="sxs-lookup"><span data-stu-id="a3962-190">After the virtual array restarts, sign into the local web UI.</span></span>

7. <span data-ttu-id="a3962-191">Opakujte krok 2 – 4 k instalaci zabezpečení systému Windows opravy pomocí souborů `windows8.1-kb4012213-x64`,`windows8.1-kb3205400-x64`, a `windows8.1-kb4019213-x64`.</span><span class="sxs-lookup"><span data-stu-id="a3962-191">Repeat step 2-4 to install the Windows security fixes using files `windows8.1-kb4012213-x64`,`windows8.1-kb3205400-x64`, and `windows8.1-kb4019213-x64`.</span></span> <span data-ttu-id="a3962-192">Pole virtuální restartuje po každé instalaci a je třeba se přihlásit do místního webového uživatelského rozhraní.</span><span class="sxs-lookup"><span data-stu-id="a3962-192">The virtual array restarts after each install and you need to sign into the local web UI.</span></span>

## <a name="next-steps"></a><span data-ttu-id="a3962-193">Další kroky</span><span class="sxs-lookup"><span data-stu-id="a3962-193">Next steps</span></span>

<span data-ttu-id="a3962-194">Další informace o [Správa pole virtuální zařízení StorSimple](storsimple-ova-web-ui-admin.md).</span><span class="sxs-lookup"><span data-stu-id="a3962-194">Learn more about [administering your StorSimple Virtual Array](storsimple-ova-web-ui-admin.md).</span></span>

