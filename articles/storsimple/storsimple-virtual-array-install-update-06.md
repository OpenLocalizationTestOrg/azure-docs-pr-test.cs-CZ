---
title: "aaaInstall aktualizace 0,6 v poli virtuální zařízení StorSimple | Microsoft Docs"
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
ms.date: 05/18/2017
ms.author: alkohli
ms.openlocfilehash: 2ccd1b5fc1957c35ebec35aa947d331b3ff05464
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="install-update-06-on-your-storsimple-virtual-array"></a><span data-ttu-id="bccd5-103">Nainstalujte aktualizace 0,6 na pole virtuální zařízení StorSimple</span><span class="sxs-lookup"><span data-stu-id="bccd5-103">Install Update 0.6 on your StorSimple Virtual Array</span></span>

## <a name="overview"></a><span data-ttu-id="bccd5-104">Přehled</span><span class="sxs-lookup"><span data-stu-id="bccd5-104">Overview</span></span>

<span data-ttu-id="bccd5-105">Tento článek popisuje hello kroky požadované tooinstall aktualizace 0,6 na pole virtuální zařízení StorSimple prostřednictvím hello místního webového uživatelského rozhraní a prostřednictvím hello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="bccd5-105">This article describes hello steps required tooinstall Update 0.6 on your StorSimple Virtual Array via hello local web UI and via hello Azure portal.</span></span> <span data-ttu-id="bccd5-106">Můžete použít hello softwarové aktualizace a opravy hotfix tookeep aktuální virtuální pole StorSimple.</span><span class="sxs-lookup"><span data-stu-id="bccd5-106">You apply hello software updates or hotfixes tookeep your StorSimple Virtual Array up-to-date.</span></span>

<span data-ttu-id="bccd5-107">Před instalací aktualizace, doporučujeme je provést hello svazky nebo sdílené složky offline na hello nejprve hostitele a pak hello zařízení.</span><span class="sxs-lookup"><span data-stu-id="bccd5-107">Before you apply an update, we recommend that you take hello volumes or shares offline on hello host first and then hello device.</span></span> <span data-ttu-id="bccd5-108">Tím se minimalizují možnost poškození dat.</span><span class="sxs-lookup"><span data-stu-id="bccd5-108">This minimizes any possibility of data corruption.</span></span> <span data-ttu-id="bccd5-109">Jakmile hello svazky nebo sdílené složky jsou offline, můžete také provést ruční zálohování hello zařízení.</span><span class="sxs-lookup"><span data-stu-id="bccd5-109">After hello volumes or shares are offline, you should also take a manual backup of hello device.</span></span>

> [!IMPORTANT]
> - <span data-ttu-id="bccd5-110">Aktualizace 0,6 odpovídá příliš**10.0.10293.0** verze softwaru ve vašem zařízení.</span><span class="sxs-lookup"><span data-stu-id="bccd5-110">Update 0.6 corresponds too**10.0.10293.0** software version on your device.</span></span> <span data-ttu-id="bccd5-111">Informace na to, co je nového v této aktualizaci najdete příliš[poznámky k verzi pro aktualizaci 0,6](storsimple-virtual-array-update-06-release-notes.md).</span><span class="sxs-lookup"><span data-stu-id="bccd5-111">For information on what is new in this update, go too[Release notes for Update 0.6](storsimple-virtual-array-update-06-release-notes.md).</span></span>
>
> - <span data-ttu-id="bccd5-112">Pokud používáte verzi Update 0,2 nebo novější, doporučujeme, aby instalaci aktualizací hello prostřednictvím hello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="bccd5-112">If you are running Update 0.2 or later, we recommend that you install hello updates via hello Azure portal.</span></span> <span data-ttu-id="bccd5-113">Pokud používáte Update 0.1 nebo verze GA softwaru, musíte použít opravu hotfix metody hello přes hello místního webového uživatelského rozhraní tooinstall aktualizace 0,6.</span><span class="sxs-lookup"><span data-stu-id="bccd5-113">If you are running Update 0.1 or GA software versions, you must use hello hotfix method via hello local web UI tooinstall Update 0.6.</span></span>
>
> - <span data-ttu-id="bccd5-114">Mějte na paměti, který instaluje aktualizaci nebo opravu hotfix restartuje vaše zařízení.</span><span class="sxs-lookup"><span data-stu-id="bccd5-114">Keep in mind that installing an update or hotfix restarts your device.</span></span> <span data-ttu-id="bccd5-115">Zadaná, hello pole virtuální zařízení StorSimple je jeden uzel zařízení, dojde k narušení všechny vstupně-výstupních operací v průběhu a zařízení dojde k výpadku.</span><span class="sxs-lookup"><span data-stu-id="bccd5-115">Given that hello StorSimple Virtual Array is a single node device, any I/O in progress is disrupted and your device experiences downtime.</span></span>

## <a name="use-hello-azure-portal"></a><span data-ttu-id="bccd5-116">Hello použití portálu Azure</span><span class="sxs-lookup"><span data-stu-id="bccd5-116">Use hello Azure portal</span></span>

<span data-ttu-id="bccd5-117">Pokud aktualizace 0,2 a novější, doporučujeme instalovat aktualizace prostřednictvím hello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="bccd5-117">If running Update 0.2 and later, we recommend that you install updates through hello Azure portal.</span></span> <span data-ttu-id="bccd5-118">Postup portálu Hello vyžaduje tooscan uživatele hello, stáhněte a nainstalujte aktualizace hello.</span><span class="sxs-lookup"><span data-stu-id="bccd5-118">hello portal procedure requires hello user tooscan, download, and then install hello updates.</span></span> <span data-ttu-id="bccd5-119">Tento postup trvá asi 7 minut toocomplete.</span><span class="sxs-lookup"><span data-stu-id="bccd5-119">This procedure takes around 7 minutes toocomplete.</span></span> <span data-ttu-id="bccd5-120">Proveďte následující hello kroky tooinstall hello aktualizaci nebo opravu hotfix.</span><span class="sxs-lookup"><span data-stu-id="bccd5-120">Perform hello following steps tooinstall hello update or hotfix.</span></span>

[!INCLUDE [storsimple-virtual-array-install-update-via-portal](../../includes/storsimple-virtual-array-install-update-via-portal-04.md)]

<span data-ttu-id="bccd5-121">Hello instalace po dokončení, přejděte tooyour služby StorSimple Manager zařízení.</span><span class="sxs-lookup"><span data-stu-id="bccd5-121">After hello installation is complete, go tooyour StorSimple Device Manager service.</span></span> <span data-ttu-id="bccd5-122">Vyberte **zařízení** a pak vyberte a klikněte na zařízení hello k aktualizaci.</span><span class="sxs-lookup"><span data-stu-id="bccd5-122">Select **Devices** and then select and click hello device you just updated.</span></span> <span data-ttu-id="bccd5-123">Přejděte příliš**Nastavení > Správa > aktualizace zařízení**.</span><span class="sxs-lookup"><span data-stu-id="bccd5-123">Go too**Settings > Manage > Device Updates**.</span></span> <span data-ttu-id="bccd5-124">verze softwaru Hello zobrazí musí být **10.0.10293.0**.</span><span class="sxs-lookup"><span data-stu-id="bccd5-124">hello displayed software version should be **10.0.10293.0**.</span></span>

## <a name="use-hello-local-web-ui"></a><span data-ttu-id="bccd5-125">Použít místní hello webového uživatelského rozhraní</span><span class="sxs-lookup"><span data-stu-id="bccd5-125">Use hello local web UI</span></span>

<span data-ttu-id="bccd5-126">Existují dva kroky při použití hello místního webového uživatelského rozhraní:</span><span class="sxs-lookup"><span data-stu-id="bccd5-126">There are two steps when using hello local web UI:</span></span>

* <span data-ttu-id="bccd5-127">Stáhnout hello aktualizaci nebo opravu hotfix hello</span><span class="sxs-lookup"><span data-stu-id="bccd5-127">Download hello update or hello hotfix</span></span>
* <span data-ttu-id="bccd5-128">Nainstalujte hello aktualizaci nebo opravu hotfix hello</span><span class="sxs-lookup"><span data-stu-id="bccd5-128">Install hello update or hello hotfix</span></span>

### <a name="download-hello-update-or-hello-hotfix"></a><span data-ttu-id="bccd5-129">Stáhnout hello aktualizaci nebo opravu hotfix hello</span><span class="sxs-lookup"><span data-stu-id="bccd5-129">Download hello update or hello hotfix</span></span>

<span data-ttu-id="bccd5-130">Proveďte následující kroky toodownload hello softwarové aktualizace z katalogu služby Microsoft Update hello hello.</span><span class="sxs-lookup"><span data-stu-id="bccd5-130">Perform hello following steps toodownload hello software update from hello Microsoft Update Catalog.</span></span>

#### <a name="toodownload-hello-update-or-hello-hotfix"></a><span data-ttu-id="bccd5-131">toodownload hello aktualizace nebo hello oprav hotfix</span><span class="sxs-lookup"><span data-stu-id="bccd5-131">toodownload hello update or hello hotfix</span></span>

1. <span data-ttu-id="bccd5-132">Spusťte Internet Explorer a přejděte příliš[http://catalog.update.microsoft.com](http://catalog.update.microsoft.com).</span><span class="sxs-lookup"><span data-stu-id="bccd5-132">Start Internet Explorer and navigate too[http://catalog.update.microsoft.com](http://catalog.update.microsoft.com).</span></span>

2. <span data-ttu-id="bccd5-133">Pokud používáte hello katalogu služby Microsoft Update pro hello poprvé v tomto počítači, klikněte na tlačítko **nainstalovat** při výzvami tooinstall hello rozšíření katalogu služby Microsoft Update.</span><span class="sxs-lookup"><span data-stu-id="bccd5-133">If you are using hello Microsoft Update Catalog for hello first time on this computer, click **Install** when prompted tooinstall hello Microsoft Update Catalog add-on.</span></span>

3. <span data-ttu-id="bccd5-134">Do vyhledávacího pole hello hello katalogu služby Microsoft Update, zadejte číslo znalostní báze Knowledge Base (KB) hello hello opravy hotfix, kterou chcete toodownload.</span><span class="sxs-lookup"><span data-stu-id="bccd5-134">In hello search box of hello Microsoft Update Catalog, enter hello Knowledge Base (KB) number of hello hotfix you want toodownload.</span></span> <span data-ttu-id="bccd5-135">Zadejte **4023268** pro aktualizace 0,6 a pak klikněte na tlačítko **vyhledávání**.</span><span class="sxs-lookup"><span data-stu-id="bccd5-135">Enter **4023268** for Update 0.6, and then click **Search**.</span></span>
   
    <span data-ttu-id="bccd5-136">Hello opravu hotfix seznamu se zobrazí, například **aktualizace pole virtuálního zařízení StorSimple 0,6**.</span><span class="sxs-lookup"><span data-stu-id="bccd5-136">hello hotfix listing appears, for example, **StorSimple Virtual Array Update 0.6**.</span></span>
   
    ![Prohledávání katalogu](./media/storsimple-virtual-array-install-update-06/download1.png)

4. <span data-ttu-id="bccd5-138">Klikněte na **Stáhnout**.</span><span class="sxs-lookup"><span data-stu-id="bccd5-138">Click **Download**.</span></span>

5. <span data-ttu-id="bccd5-139">Měli byste vidět toodownload pět souborů.</span><span class="sxs-lookup"><span data-stu-id="bccd5-139">You should see five files toodownload.</span></span> <span data-ttu-id="bccd5-140">Stáhněte si každý z těchto souborů tooa složky.</span><span class="sxs-lookup"><span data-stu-id="bccd5-140">Download each of those files tooa folder.</span></span> <span data-ttu-id="bccd5-141">Hello složce může být také síťové sdílené složky zkopírovaný tooa, který je dosažitelný z hello zařízení.</span><span class="sxs-lookup"><span data-stu-id="bccd5-141">hello folder can also be copied tooa network share that is reachable from hello device.</span></span>

6. <span data-ttu-id="bccd5-142">Otevřete složku hello, kde jsou umístěny soubory hello.</span><span class="sxs-lookup"><span data-stu-id="bccd5-142">Open hello folder where hello files are located.</span></span>
    <span data-ttu-id="bccd5-143">![Soubory v balíčku hello](./media/storsimple-virtual-array-install-update-06/update06folder.png)</span><span class="sxs-lookup"><span data-stu-id="bccd5-143">![Files in hello package](./media/storsimple-virtual-array-install-update-06/update06folder.png)</span></span>

    <span data-ttu-id="bccd5-144">Zobrazí:</span><span class="sxs-lookup"><span data-stu-id="bccd5-144">You see:</span></span>
    -  <span data-ttu-id="bccd5-145">Soubor balíčku samostatné aktualizace Microsoft `WindowsTH-KB3011067-x64`.</span><span class="sxs-lookup"><span data-stu-id="bccd5-145">A Microsoft Update Standalone Package file `WindowsTH-KB3011067-x64`.</span></span> <span data-ttu-id="bccd5-146">Tento soubor je software, zařízení používaných tooupdate hello.</span><span class="sxs-lookup"><span data-stu-id="bccd5-146">This file is used tooupdate hello device software.</span></span>
    - <span data-ttu-id="bccd5-147">Soubor balíčku agenta monitorování Genevy `GenevaMonitoringAgentPackageInstaller`.</span><span class="sxs-lookup"><span data-stu-id="bccd5-147">A Geneva Monitoring Agent Package file `GenevaMonitoringAgentPackageInstaller`.</span></span> <span data-ttu-id="bccd5-148">Tento soubor je použité tooupdate hello monitorovací a diagnostické služby (MDS) agenta.</span><span class="sxs-lookup"><span data-stu-id="bccd5-148">This file is used tooupdate hello Monitoring and Diagnostics service (MDS) agent.</span></span> <span data-ttu-id="bccd5-149">Poklikejte na soubor cab hello.</span><span class="sxs-lookup"><span data-stu-id="bccd5-149">Double-click hello cab file.</span></span> <span data-ttu-id="bccd5-150">A _.msi_ se zobrazí.</span><span class="sxs-lookup"><span data-stu-id="bccd5-150">A _.msi_ is displayed.</span></span> <span data-ttu-id="bccd5-151">Vyberte hello souboru, klikněte pravým tlačítkem a potom **extrahovat** hello souboru.</span><span class="sxs-lookup"><span data-stu-id="bccd5-151">Select hello file, right-click, and then **Extract** hello file.</span></span> <span data-ttu-id="bccd5-152">Použít hello _.msi_ souboru tooupdate hello agenta.</span><span class="sxs-lookup"><span data-stu-id="bccd5-152">You use hello _.msi_ file tooupdate hello agent.</span></span>

        ![Extrahujte soubor aktualizace agenta služby MDS](./media/storsimple-virtual-array-install-update-06/extract-geneva-monitoring-agent-installer.png)

        > [!IMPORTANT]
        > <span data-ttu-id="bccd5-154">Pokud používáte StorSimple Update 0,5 (0.0.10293.0) nemusí tooupdate hello MDS agenta.</span><span class="sxs-lookup"><span data-stu-id="bccd5-154">You do not need tooupdate hello MDS agent if you are running StorSimple Update 0.5 (0.0.10293.0).</span></span>

    - <span data-ttu-id="bccd5-155">Tři soubory, které obsahují důležité aktualizace zabezpečení systému Windows, `windows8.1-kb4012213-x64`,`windows8.1-kb3205400-x64`, a `windows8.1-kb4019213-x64`.</span><span class="sxs-lookup"><span data-stu-id="bccd5-155">Three files that contain critical Windows security updates, `windows8.1-kb4012213-x64`,`windows8.1-kb3205400-x64`, and `windows8.1-kb4019213-x64`.</span></span>


### <a name="install-hello-update-or-hello-hotfix"></a><span data-ttu-id="bccd5-156">Nainstalujte hello aktualizaci nebo opravu hotfix hello</span><span class="sxs-lookup"><span data-stu-id="bccd5-156">Install hello update or hello hotfix</span></span>

<span data-ttu-id="bccd5-157">Předchozí toohello instalace aktualizace nebo opravy hotfix, ujistěte se, že máte hello aktualizace nebo opravy hotfix hello stáhli buď místně na vašem hostiteli nebo přístupné přes síťové sdílené složky.</span><span class="sxs-lookup"><span data-stu-id="bccd5-157">Prior toohello update or hotfix installation, make sure that you have hello update or hello hotfix downloaded either locally on your host or accessible via a network share.</span></span>

<span data-ttu-id="bccd5-158">Tato metoda tooinstall aktualizace použít na zařízení se systémem GA nebo aktualizovat 0,1 verze softwaru.</span><span class="sxs-lookup"><span data-stu-id="bccd5-158">Use this method tooinstall updates on a device running GA or Update 0.1 software versions.</span></span> <span data-ttu-id="bccd5-159">Tento postup trvá přibližně 12 minut toocomplete.</span><span class="sxs-lookup"><span data-stu-id="bccd5-159">This procedure takes approximately 12 minutes toocomplete.</span></span> <span data-ttu-id="bccd5-160">Proveďte následující hello kroky tooinstall hello aktualizaci nebo opravu hotfix.</span><span class="sxs-lookup"><span data-stu-id="bccd5-160">Perform hello following steps tooinstall hello update or hotfix.</span></span>

#### <a name="tooinstall-hello-update-or-hello-hotfix"></a><span data-ttu-id="bccd5-161">tooinstall hello aktualizace nebo hello oprav hotfix</span><span class="sxs-lookup"><span data-stu-id="bccd5-161">tooinstall hello update or hello hotfix</span></span>

1. <span data-ttu-id="bccd5-162">Hello místního webového uživatelského rozhraní, přejděte v příliš**údržby** > **aktualizace softwaru**.</span><span class="sxs-lookup"><span data-stu-id="bccd5-162">In hello local web UI, go too**Maintenance** > **Software Update**.</span></span> <span data-ttu-id="bccd5-163">Poznamenejte si hello verze softwaru, který používáte.</span><span class="sxs-lookup"><span data-stu-id="bccd5-163">Make a note of hello software version that you are running.</span></span> <span data-ttu-id="bccd5-164">Pokud používáte **10.0.10290.0**, není nutné tooupdate hello MDS agenta v kroku 6.</span><span class="sxs-lookup"><span data-stu-id="bccd5-164">If you are running **10.0.10290.0**, you do not need tooupdate hello MDS agent in step 6.</span></span>
   
    ![aktualizace zařízení](./media/storsimple-virtual-array-install-update-05/update1m.png)

2. <span data-ttu-id="bccd5-166">V **cesta k souboru aktualizace**, zadejte název souboru hello pro aktualizaci hello nebo hello opravu hotfix.</span><span class="sxs-lookup"><span data-stu-id="bccd5-166">In **Update file path**, enter hello file name for hello update or hello hotfix.</span></span> <span data-ttu-id="bccd5-167">Pokud je umístěn ve sdílené síťové složce, procházet toohello aktualizaci nebo opravu hotfix instalační soubor.</span><span class="sxs-lookup"><span data-stu-id="bccd5-167">You can also browse toohello update or hotfix installation file if placed on a network share.</span></span> <span data-ttu-id="bccd5-168">Klikněte na tlačítko **Použít**.</span><span class="sxs-lookup"><span data-stu-id="bccd5-168">Click **Apply**.</span></span>
   
    ![aktualizace zařízení](./media/storsimple-virtual-array-install-update-05/update2m.png)

3. <span data-ttu-id="bccd5-170">Zobrazí se upozornění.</span><span class="sxs-lookup"><span data-stu-id="bccd5-170">A warning is displayed.</span></span> <span data-ttu-id="bccd5-171">Zadaný hello virtuální pole je jeden uzel zařízení, po instalaci aktualizace hello, hello zařízení restartuje a dojde k výpadku.</span><span class="sxs-lookup"><span data-stu-id="bccd5-171">Given hello virtual array is a single node device, after hello update is applied, hello device restarts and there is downtime.</span></span> <span data-ttu-id="bccd5-172">Klikněte na ikonu zaškrtnutí hello.</span><span class="sxs-lookup"><span data-stu-id="bccd5-172">Click hello check icon.</span></span>
   
   ![aktualizace zařízení](./media/storsimple-virtual-array-install-update-05/update3m.png)

4. <span data-ttu-id="bccd5-174">Hello aktualizace spustí.</span><span class="sxs-lookup"><span data-stu-id="bccd5-174">hello update starts.</span></span> <span data-ttu-id="bccd5-175">Po úspěšné aktualizaci hello zařízení restartuje.</span><span class="sxs-lookup"><span data-stu-id="bccd5-175">After hello device is successfully updated, it restarts.</span></span> <span data-ttu-id="bccd5-176">Hello místního uživatelského rozhraní není dostupný v této hodnotě duration.</span><span class="sxs-lookup"><span data-stu-id="bccd5-176">hello local UI is not accessible in this duration.</span></span>
   
    ![aktualizace zařízení](./media/storsimple-virtual-array-install-update-05/update5m.png)

5. <span data-ttu-id="bccd5-178">Po dokončení restartování hello přejdete toohello **přihlášení** stránky.</span><span class="sxs-lookup"><span data-stu-id="bccd5-178">After hello restart is complete, you are taken toohello **Sign in** page.</span></span> <span data-ttu-id="bccd5-179">tooverify, která se aktualizovala hello zařízení software, hello místního webového uživatelského rozhraní, přejděte příliš**údržby** > **aktualizace softwaru**.</span><span class="sxs-lookup"><span data-stu-id="bccd5-179">tooverify that hello device software has updated, in hello local web UI, go too**Maintenance** > **Software Update**.</span></span> <span data-ttu-id="bccd5-180">verze softwaru Hello zobrazí musí být **10.0.0.0.0.10293** pro aktualizaci 0,6.</span><span class="sxs-lookup"><span data-stu-id="bccd5-180">hello displayed software version should be **10.0.0.0.0.10293** for Update 0.6.</span></span>
   
   > [!NOTE]
   > <span data-ttu-id="bccd5-181">Jsme sestavy verze softwaru hello mírně jiným způsobem v hello místního webového uživatelského rozhraní a hello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="bccd5-181">We report hello software versions in a slightly different way in hello local web UI and hello Azure portal.</span></span> <span data-ttu-id="bccd5-182">Například hello místní webové sestavy nástroje uživatelského rozhraní **10.0.0.0.0.10293** a hello Azure portálu sestavy **10.0.10293.0** pro hello stejnou verzi.</span><span class="sxs-lookup"><span data-stu-id="bccd5-182">For example, hello local web UI reports **10.0.0.0.0.10293** and hello Azure portal reports **10.0.10293.0** for hello same version.</span></span>
   
    ![aktualizace zařízení](./media/storsimple-virtual-array-install-update-06/update6m.png)

6. <span data-ttu-id="bccd5-184">Tento krok přeskočte, pokud byly spuštěny aktualizace pole virtuálního zařízení StorSimple 0,5 (**10.0.10290.0**) před instalací této aktualizace.</span><span class="sxs-lookup"><span data-stu-id="bccd5-184">Skip this step if you were running StorSimple Virtual Array Update 0.5 (**10.0.10290.0**) before you applied this update.</span></span> <span data-ttu-id="bccd5-185">Učinit poznámku o verzi softwaru hello v kroku 1, než zahájíte tooupdate.</span><span class="sxs-lookup"><span data-stu-id="bccd5-185">You made a note of hello software version in step 1 before you began tooupdate.</span></span> <span data-ttu-id="bccd5-186">Pokud byly spuštěny aktualizace 0,5, vaše MDS agent je již aktuální.</span><span class="sxs-lookup"><span data-stu-id="bccd5-186">If you were running Update 0.5, your MDS agent is already up-to-date .</span></span>

    <span data-ttu-id="bccd5-187">Pokud používáte předchozí tooUpdate software verze 0,5, dalším krokem hello vám je tooupdate hello MDS agenta.</span><span class="sxs-lookup"><span data-stu-id="bccd5-187">If you are running a software version prior tooUpdate 0.5, hello next step for you is tooupdate hello MDS agent.</span></span> <span data-ttu-id="bccd5-188">V hello **aktualizace softwaru** stránky, přejděte toohello **cesta k souboru aktualizace** a procházet toohello `GenevaMonitoringAgentPackageInstaller.msi` souboru.</span><span class="sxs-lookup"><span data-stu-id="bccd5-188">In hello **Software Update** page, go toohello **Update file path** and browse toohello `GenevaMonitoringAgentPackageInstaller.msi` file.</span></span> <span data-ttu-id="bccd5-189">Opakujte kroky 2 – 4.</span><span class="sxs-lookup"><span data-stu-id="bccd5-189">Repeat steps 2-4.</span></span> <span data-ttu-id="bccd5-190">Po restartování hello virtuální pole, přihlaste se k hello místního webového uživatelského rozhraní.</span><span class="sxs-lookup"><span data-stu-id="bccd5-190">After hello virtual array restarts, sign into hello local web UI.</span></span>

7. <span data-ttu-id="bccd5-191">Opakujte krok opravy zabezpečení systému Windows hello tooinstall 2 – 4 pomocí souborů `windows8.1-kb4012213-x64`,`windows8.1-kb3205400-x64`, a `windows8.1-kb4019213-x64`.</span><span class="sxs-lookup"><span data-stu-id="bccd5-191">Repeat step 2-4 tooinstall hello Windows security fixes using files `windows8.1-kb4012213-x64`,`windows8.1-kb3205400-x64`, and `windows8.1-kb4019213-x64`.</span></span> <span data-ttu-id="bccd5-192">pole virtuálním Hello restartuje po každé instalaci a je třeba toosign do hello místního webového uživatelského rozhraní.</span><span class="sxs-lookup"><span data-stu-id="bccd5-192">hello virtual array restarts after each install and you need toosign into hello local web UI.</span></span>

## <a name="next-steps"></a><span data-ttu-id="bccd5-193">Další kroky</span><span class="sxs-lookup"><span data-stu-id="bccd5-193">Next steps</span></span>

<span data-ttu-id="bccd5-194">Další informace o [Správa pole virtuální zařízení StorSimple](storsimple-ova-web-ui-admin.md).</span><span class="sxs-lookup"><span data-stu-id="bccd5-194">Learn more about [administering your StorSimple Virtual Array](storsimple-ova-web-ui-admin.md).</span></span>

