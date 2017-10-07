---
title: "aaaInstall aktualizace 0,5 v poli virtuální zařízení StorSimple | Microsoft Docs"
description: "Popisuje, jak toouse hello pole virtuální zařízení StorSimple webového uživatelského rozhraní tooapply aktualizací pomocí hello Azure portal a opravy hotfix – metoda"
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
ms.date: 05/10/2017
ms.author: alkohli
ms.openlocfilehash: c38daa85daa0086e67cf0206d76cb19d9c8b21b3
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="install-update-05-on-your-storsimple-virtual-array"></a><span data-ttu-id="be467-103">Nainstalujte aktualizace 0,5 na pole virtuální zařízení StorSimple</span><span class="sxs-lookup"><span data-stu-id="be467-103">Install Update 0.5 on your StorSimple Virtual Array</span></span>

## <a name="overview"></a><span data-ttu-id="be467-104">Přehled</span><span class="sxs-lookup"><span data-stu-id="be467-104">Overview</span></span>

<span data-ttu-id="be467-105">Tento článek popisuje hello kroky požadované tooinstall aktualizace 0,5 na pole virtuální zařízení StorSimple prostřednictvím hello místního webového uživatelského rozhraní a prostřednictvím hello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="be467-105">This article describes hello steps required tooinstall Update 0.5 on your StorSimple Virtual Array via hello local web UI and via hello Azure portal.</span></span> <span data-ttu-id="be467-106">Je nutné tooapply softwarové aktualizace a opravy hotfix tookeep pole virtuální zařízení StorSimple aktuální.</span><span class="sxs-lookup"><span data-stu-id="be467-106">You need tooapply software updates or hotfixes tookeep your StorSimple Virtual Array up-to-date.</span></span>

<span data-ttu-id="be467-107">Před instalací aktualizace, doporučujeme je provést hello svazky nebo sdílené složky offline na hello nejprve hostitele a pak hello zařízení.</span><span class="sxs-lookup"><span data-stu-id="be467-107">Before you apply an update, we recommend that you take hello volumes or shares offline on hello host first and then hello device.</span></span> <span data-ttu-id="be467-108">Tím se minimalizují možnost poškození dat.</span><span class="sxs-lookup"><span data-stu-id="be467-108">This minimizes any possibility of data corruption.</span></span> <span data-ttu-id="be467-109">Jakmile hello svazky nebo sdílené složky jsou offline, můžete také provést ruční zálohování hello zařízení.</span><span class="sxs-lookup"><span data-stu-id="be467-109">After hello volumes or shares are offline, you should also take a manual backup of hello device.</span></span>

> [!IMPORTANT]
> - <span data-ttu-id="be467-110">Aktualizace 0,5 odpovídá příliš**10.0.10290.0** verze softwaru ve vašem zařízení.</span><span class="sxs-lookup"><span data-stu-id="be467-110">Update 0.5 corresponds too**10.0.10290.0** software version on your device.</span></span> <span data-ttu-id="be467-111">Informace na to, co je nového v této aktualizaci najdete příliš[poznámky k verzi pro aktualizaci 0,5](storsimple-virtual-array-update-05-release-notes.md).</span><span class="sxs-lookup"><span data-stu-id="be467-111">For information on what is new in this update, go too[Release notes for Update 0.5](storsimple-virtual-array-update-05-release-notes.md).</span></span>
>
> - <span data-ttu-id="be467-112">Pokud používáte verzi Update 0,2 nebo novější, doporučujeme, aby instalaci aktualizací hello prostřednictvím hello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="be467-112">If you are running Update 0.2 or later, we recommend that you install hello updates via hello Azure portal.</span></span> <span data-ttu-id="be467-113">Pokud používáte Update 0.1 nebo verze GA softwaru, musíte použít opravu hotfix metody hello přes hello místního webového uživatelského rozhraní tooinstall aktualizace 0,5.</span><span class="sxs-lookup"><span data-stu-id="be467-113">If you are running Update 0.1 or GA software versions, you must use hello hotfix method via hello local web UI tooinstall Update 0.5.</span></span>
>
> - <span data-ttu-id="be467-114">Mějte na paměti, který instaluje aktualizaci nebo opravu hotfix restartuje vaše zařízení.</span><span class="sxs-lookup"><span data-stu-id="be467-114">Keep in mind that installing an update or hotfix restarts your device.</span></span> <span data-ttu-id="be467-115">Zadaná, hello pole virtuální zařízení StorSimple je jeden uzel zařízení, dojde k narušení všechny vstupně-výstupních operací v průběhu a zařízení dojde k výpadku.</span><span class="sxs-lookup"><span data-stu-id="be467-115">Given that hello StorSimple Virtual Array is a single node device, any I/O in progress is disrupted and your device experiences downtime.</span></span>

## <a name="use-hello-azure-portal"></a><span data-ttu-id="be467-116">Hello použití portálu Azure</span><span class="sxs-lookup"><span data-stu-id="be467-116">Use hello Azure portal</span></span>

<span data-ttu-id="be467-117">Pokud aktualizace 0,2 a novější, doporučujeme instalovat aktualizace prostřednictvím hello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="be467-117">If running Update 0.2 and later, we recommend that you install updates through hello Azure portal.</span></span> <span data-ttu-id="be467-118">Postup portálu Hello vyžaduje tooscan uživatele hello, stáhněte a nainstalujte aktualizace hello.</span><span class="sxs-lookup"><span data-stu-id="be467-118">hello portal procedure requires hello user tooscan, download, and then install hello updates.</span></span> <span data-ttu-id="be467-119">Tento postup trvá asi 7 minut toocomplete.</span><span class="sxs-lookup"><span data-stu-id="be467-119">This procedure takes around 7 minutes toocomplete.</span></span> <span data-ttu-id="be467-120">Proveďte následující hello kroky tooinstall hello aktualizaci nebo opravu hotfix.</span><span class="sxs-lookup"><span data-stu-id="be467-120">Perform hello following steps tooinstall hello update or hotfix.</span></span>

[!INCLUDE [storsimple-virtual-array-install-update-via-portal](../../includes/storsimple-virtual-array-install-update-via-portal-04.md)]

<span data-ttu-id="be467-121">Hello instalace po dokončení, přejděte tooyour služby StorSimple Manager zařízení.</span><span class="sxs-lookup"><span data-stu-id="be467-121">After hello installation is complete, go tooyour StorSimple Device Manager service.</span></span> <span data-ttu-id="be467-122">Vyberte **zařízení** a pak vyberte a klikněte na zařízení hello k aktualizaci.</span><span class="sxs-lookup"><span data-stu-id="be467-122">Select **Devices** and then select and click hello device you just updated.</span></span> <span data-ttu-id="be467-123">Přejděte příliš**Nastavení > Správa > aktualizace zařízení**.</span><span class="sxs-lookup"><span data-stu-id="be467-123">Go too**Settings > Manage > Device Updates**.</span></span> <span data-ttu-id="be467-124">verze softwaru Hello zobrazí musí být **10.0.10290.0**.</span><span class="sxs-lookup"><span data-stu-id="be467-124">hello displayed software version should be **10.0.10290.0**.</span></span>

## <a name="use-hello-local-web-ui"></a><span data-ttu-id="be467-125">Použít místní hello webového uživatelského rozhraní</span><span class="sxs-lookup"><span data-stu-id="be467-125">Use hello local web UI</span></span>

<span data-ttu-id="be467-126">Existují dva kroky při použití hello místního webového uživatelského rozhraní:</span><span class="sxs-lookup"><span data-stu-id="be467-126">There are two steps when using hello local web UI:</span></span>

* <span data-ttu-id="be467-127">Stáhnout hello aktualizaci nebo opravu hotfix hello</span><span class="sxs-lookup"><span data-stu-id="be467-127">Download hello update or hello hotfix</span></span>
* <span data-ttu-id="be467-128">Nainstalujte hello aktualizaci nebo opravu hotfix hello</span><span class="sxs-lookup"><span data-stu-id="be467-128">Install hello update or hello hotfix</span></span>

### <a name="download-hello-update-or-hello-hotfix"></a><span data-ttu-id="be467-129">Stáhnout hello aktualizaci nebo opravu hotfix hello</span><span class="sxs-lookup"><span data-stu-id="be467-129">Download hello update or hello hotfix</span></span>

<span data-ttu-id="be467-130">Proveďte následující kroky toodownload hello softwarové aktualizace z katalogu služby Microsoft Update hello hello.</span><span class="sxs-lookup"><span data-stu-id="be467-130">Perform hello following steps toodownload hello software update from hello Microsoft Update Catalog.</span></span>

#### <a name="toodownload-hello-update-or-hello-hotfix"></a><span data-ttu-id="be467-131">toodownload hello aktualizace nebo hello oprav hotfix</span><span class="sxs-lookup"><span data-stu-id="be467-131">toodownload hello update or hello hotfix</span></span>

1. <span data-ttu-id="be467-132">Spusťte Internet Explorer a přejděte příliš[http://catalog.update.microsoft.com](http://catalog.update.microsoft.com).</span><span class="sxs-lookup"><span data-stu-id="be467-132">Start Internet Explorer and navigate too[http://catalog.update.microsoft.com](http://catalog.update.microsoft.com).</span></span>

2. <span data-ttu-id="be467-133">Pokud používáte hello katalogu služby Microsoft Update na tomto počítači poprvé, klikněte na tlačítko **nainstalovat** při výzvami tooinstall hello rozšíření katalogu služby Microsoft Update.</span><span class="sxs-lookup"><span data-stu-id="be467-133">If this is your first time using hello Microsoft Update Catalog on this computer, click **Install** when prompted tooinstall hello Microsoft Update Catalog add-on.</span></span>

3. <span data-ttu-id="be467-134">Do vyhledávacího pole hello hello katalogu služby Microsoft Update, zadejte číslo znalostní báze Knowledge Base (KB) hello hello opravy hotfix, kterou chcete toodownload.</span><span class="sxs-lookup"><span data-stu-id="be467-134">In hello search box of hello Microsoft Update Catalog, enter hello Knowledge Base (KB) number of hello hotfix you want toodownload.</span></span> <span data-ttu-id="be467-135">Zadejte **4021576** pro aktualizace 0,5 a pak klikněte na tlačítko **vyhledávání**.</span><span class="sxs-lookup"><span data-stu-id="be467-135">Enter **4021576** for Update 0.5, and then click **Search**.</span></span>
   
    <span data-ttu-id="be467-136">Hello opravu hotfix seznamu se zobrazí, například **aktualizace pole virtuálního zařízení StorSimple 0,5**.</span><span class="sxs-lookup"><span data-stu-id="be467-136">hello hotfix listing appears, for example, **StorSimple Virtual Array Update 0.5**.</span></span>
   
    ![Prohledávání katalogu](./media/storsimple-virtual-array-install-update-05/download1.png)

4. <span data-ttu-id="be467-138">Klikněte na **Stáhnout**.</span><span class="sxs-lookup"><span data-stu-id="be467-138">Click **Download**.</span></span> 

5. <span data-ttu-id="be467-139">Měli byste vidět dva soubory toodownload *.msu* a *.cab* souboru.</span><span class="sxs-lookup"><span data-stu-id="be467-139">You should see two files toodownload, a *.msu* and a *.cab* file.</span></span> <span data-ttu-id="be467-140">Stáhněte si každý z těchto souborů tooa složky.</span><span class="sxs-lookup"><span data-stu-id="be467-140">Download each of those files tooa folder.</span></span> <span data-ttu-id="be467-141">Hello složce může být také síťové sdílené složky zkopírovaný tooa, který je dosažitelný z hello zařízení.</span><span class="sxs-lookup"><span data-stu-id="be467-141">hello folder can also be copied tooa network share that is reachable from hello device.</span></span>

6. <span data-ttu-id="be467-142">Otevřete složku hello, kde jsou umístěny soubory hello.</span><span class="sxs-lookup"><span data-stu-id="be467-142">Open hello folder where hello files are located.</span></span>
    <span data-ttu-id="be467-143">![Soubory v balíčku hello](./media/storsimple-virtual-array-install-update-05/update05folder.png)</span><span class="sxs-lookup"><span data-stu-id="be467-143">![Files in hello package](./media/storsimple-virtual-array-install-update-05/update05folder.png)</span></span>

    <span data-ttu-id="be467-144">Zobrazí:</span><span class="sxs-lookup"><span data-stu-id="be467-144">You see:</span></span>
    -  <span data-ttu-id="be467-145">Soubor balíčku samostatné aktualizace Microsoft `WindowsTH-KB3011067-x64`.</span><span class="sxs-lookup"><span data-stu-id="be467-145">A Microsoft Update Standalone Package file `WindowsTH-KB3011067-x64`.</span></span> <span data-ttu-id="be467-146">Tento soubor je software, zařízení používaných tooupdate hello.</span><span class="sxs-lookup"><span data-stu-id="be467-146">This file is used tooupdate hello device software.</span></span>
    - <span data-ttu-id="be467-147">Soubor balíčku agenta monitorování Genevy `GenevaMonitoringAgentPackageInstaller`.</span><span class="sxs-lookup"><span data-stu-id="be467-147">A Geneva Monitoring Agent Package file `GenevaMonitoringAgentPackageInstaller`.</span></span> <span data-ttu-id="be467-148">Tento soubor je použité tooupdate hello monitorovací a diagnostické služby (MDS) agenta.</span><span class="sxs-lookup"><span data-stu-id="be467-148">This file is used tooupdate hello Monitoring and Diagnostics service (MDS) agent.</span></span> <span data-ttu-id="be467-149">Poklikejte na soubor cab hello.</span><span class="sxs-lookup"><span data-stu-id="be467-149">Double-click hello cab file.</span></span> <span data-ttu-id="be467-150">Zobrazí se .msi.</span><span class="sxs-lookup"><span data-stu-id="be467-150">A .msi is displayed.</span></span> <span data-ttu-id="be467-151">Vyberte hello souboru, klikněte pravým tlačítkem a potom **extrahovat** hello souboru.</span><span class="sxs-lookup"><span data-stu-id="be467-151">Select hello file, right-click, and then **Extract** hello file.</span></span> <span data-ttu-id="be467-152">Budete používat hello _.msi_ souboru tooupdate hello agenta.</span><span class="sxs-lookup"><span data-stu-id="be467-152">You will use hello _.msi_ file tooupdate hello agent.</span></span>

        ![Extrahujte soubor aktualizace agenta služby MDS](./media/storsimple-virtual-array-install-update-05/extract-geneva-monitoring-agent-installer.png)
        
    

### <a name="install-hello-update-or-hello-hotfix"></a><span data-ttu-id="be467-154">Nainstalujte hello aktualizaci nebo opravu hotfix hello</span><span class="sxs-lookup"><span data-stu-id="be467-154">Install hello update or hello hotfix</span></span>

<span data-ttu-id="be467-155">Předchozí toohello instalace aktualizace nebo opravy hotfix, ujistěte se, že máte hello aktualizace nebo opravy hotfix hello stáhli buď místně na vašem hostiteli nebo přístupné přes síťové sdílené složky.</span><span class="sxs-lookup"><span data-stu-id="be467-155">Prior toohello update or hotfix installation, make sure that you have hello update or hello hotfix downloaded either locally on your host or accessible via a network share.</span></span>

<span data-ttu-id="be467-156">Tato metoda tooinstall aktualizace použít na zařízení se systémem GA nebo aktualizovat 0,1 verze softwaru.</span><span class="sxs-lookup"><span data-stu-id="be467-156">Use this method tooinstall updates on a device running GA or Update 0.1 software versions.</span></span> <span data-ttu-id="be467-157">Tento postup trvá méně než 2 minuty toocomplete.</span><span class="sxs-lookup"><span data-stu-id="be467-157">This procedure takes less than 2 minutes toocomplete.</span></span> <span data-ttu-id="be467-158">Proveďte následující hello kroky tooinstall hello aktualizaci nebo opravu hotfix.</span><span class="sxs-lookup"><span data-stu-id="be467-158">Perform hello following steps tooinstall hello update or hotfix.</span></span>

#### <a name="tooinstall-hello-update-or-hello-hotfix"></a><span data-ttu-id="be467-159">tooinstall hello aktualizace nebo hello oprav hotfix</span><span class="sxs-lookup"><span data-stu-id="be467-159">tooinstall hello update or hello hotfix</span></span>

1. <span data-ttu-id="be467-160">Hello místního webového uživatelského rozhraní, přejděte v příliš**údržby** > **aktualizace softwaru**.</span><span class="sxs-lookup"><span data-stu-id="be467-160">In hello local web UI, go too**Maintenance** > **Software Update**.</span></span>
   
    ![aktualizace zařízení](./media/storsimple-virtual-array-install-update-05/update1m.png)

2. <span data-ttu-id="be467-162">V **cesta k souboru aktualizace**, zadejte název souboru hello pro aktualizaci hello nebo hello opravu hotfix.</span><span class="sxs-lookup"><span data-stu-id="be467-162">In **Update file path**, enter hello file name for hello update or hello hotfix.</span></span> <span data-ttu-id="be467-163">Pokud je umístěn ve sdílené síťové složce, procházet toohello aktualizaci nebo opravu hotfix instalační soubor.</span><span class="sxs-lookup"><span data-stu-id="be467-163">You can also browse toohello update or hotfix installation file if placed on a network share.</span></span> <span data-ttu-id="be467-164">Klikněte na tlačítko **Použít**.</span><span class="sxs-lookup"><span data-stu-id="be467-164">Click **Apply**.</span></span>
   
    ![aktualizace zařízení](./media/storsimple-virtual-array-install-update-05/update2m.png)

3. <span data-ttu-id="be467-166">Zobrazí se upozornění.</span><span class="sxs-lookup"><span data-stu-id="be467-166">A warning is displayed.</span></span> <span data-ttu-id="be467-167">Zadaný to je jeden uzel zařízení, po instalaci aktualizace hello, hello zařízení restartuje a dojde k výpadku.</span><span class="sxs-lookup"><span data-stu-id="be467-167">Given this is a single node device, after hello update is applied, hello device restarts and there is downtime.</span></span> <span data-ttu-id="be467-168">Klikněte na ikonu zaškrtnutí hello.</span><span class="sxs-lookup"><span data-stu-id="be467-168">Click hello check icon.</span></span>
   
   ![aktualizace zařízení](./media/storsimple-virtual-array-install-update-05/update3m.png)

4. <span data-ttu-id="be467-170">Hello aktualizace spustí.</span><span class="sxs-lookup"><span data-stu-id="be467-170">hello update starts.</span></span> <span data-ttu-id="be467-171">Po úspěšné aktualizaci hello zařízení restartuje.</span><span class="sxs-lookup"><span data-stu-id="be467-171">After hello device is successfully updated, it restarts.</span></span> <span data-ttu-id="be467-172">Hello místního uživatelského rozhraní není dostupný v této hodnotě duration.</span><span class="sxs-lookup"><span data-stu-id="be467-172">hello local UI is not accessible in this duration.</span></span>
   
    ![aktualizace zařízení](./media/storsimple-virtual-array-install-update-05/update5m.png)

5. <span data-ttu-id="be467-174">Po dokončení restartování hello přejdete toohello **přihlášení** stránky.</span><span class="sxs-lookup"><span data-stu-id="be467-174">After hello restart is complete, you are taken toohello **Sign in** page.</span></span> <span data-ttu-id="be467-175">tooverify, která se aktualizovala hello zařízení software, hello místního webového uživatelského rozhraní, přejděte příliš**údržby** > **aktualizace softwaru**.</span><span class="sxs-lookup"><span data-stu-id="be467-175">tooverify that hello device software has updated, in hello local web UI, go too**Maintenance** > **Software Update**.</span></span> <span data-ttu-id="be467-176">verze softwaru Hello zobrazí musí být **10.0.0.0.0.10290.0** pro aktualizaci 0,5.</span><span class="sxs-lookup"><span data-stu-id="be467-176">hello displayed software version should be **10.0.0.0.0.10290.0** for Update 0.5.</span></span>
   
   > [!NOTE]
   > <span data-ttu-id="be467-177">Jsme sestavy verze softwaru hello mírně jiným způsobem v hello místního webového uživatelského rozhraní a hello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="be467-177">We report hello software versions in a slightly different way in hello local web UI and hello Azure portal.</span></span> <span data-ttu-id="be467-178">Například hello místní webové sestavy nástroje uživatelského rozhraní **10.0.0.0.0.10290** a hello Azure portálu sestavy **10.0.10290.0** pro hello stejnou verzi.</span><span class="sxs-lookup"><span data-stu-id="be467-178">For example, hello local web UI reports **10.0.0.0.0.10290** and hello Azure portal reports **10.0.10290.0** for hello same version.</span></span>
   
    ![aktualizace zařízení](./media/storsimple-virtual-array-install-update-05/update6m.png)

6. <span data-ttu-id="be467-180">dalším krokem Hello je tooupdate hello MDS agenta.</span><span class="sxs-lookup"><span data-stu-id="be467-180">hello next step is tooupdate hello MDS agent.</span></span> <span data-ttu-id="be467-181">V hello **aktualizace softwaru** stránky, přejděte toohello **cesta k souboru aktualizace** a procházet toohello `GenevaMonitoringAgentPackageInstaller.msi` souboru.</span><span class="sxs-lookup"><span data-stu-id="be467-181">In hello **Software Update** page, go toohello **Update file path** and browse toohello `GenevaMonitoringAgentPackageInstaller.msi` file.</span></span> <span data-ttu-id="be467-182">Opakujte kroky 2 – 4.</span><span class="sxs-lookup"><span data-stu-id="be467-182">Repeat steps 2-4.</span></span> <span data-ttu-id="be467-183">Po restartování hello virtuální pole, přihlaste se k hello místního webového uživatelského rozhraní.</span><span class="sxs-lookup"><span data-stu-id="be467-183">After hello virtual array restarts, sign into hello local web UI.</span></span>

<span data-ttu-id="be467-184">Nyní po dokončení aktualizace Hello.</span><span class="sxs-lookup"><span data-stu-id="be467-184">hello update is now complete.</span></span>

## <a name="next-steps"></a><span data-ttu-id="be467-185">Další kroky</span><span class="sxs-lookup"><span data-stu-id="be467-185">Next steps</span></span>

<span data-ttu-id="be467-186">Další informace o [Správa pole virtuální zařízení StorSimple](storsimple-ova-web-ui-admin.md).</span><span class="sxs-lookup"><span data-stu-id="be467-186">Learn more about [administering your StorSimple Virtual Array](storsimple-ova-web-ui-admin.md).</span></span>

