---
title: "Aktualizace aaaInstall v poli virtuální zařízení StorSimple | Microsoft Docs"
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
ms.date: 02/07/2017
ms.author: alkohli
ms.openlocfilehash: 6165b305fb0d404b404cf3a11dd0ade2f2f13b2f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="install-update-04-on-your-storsimple-virtual-array"></a><span data-ttu-id="4c3a3-103">Nainstalujte aktualizace 0.4 na pole virtuální zařízení StorSimple</span><span class="sxs-lookup"><span data-stu-id="4c3a3-103">Install Update 0.4 on your StorSimple Virtual Array</span></span>

## <a name="overview"></a><span data-ttu-id="4c3a3-104">Přehled</span><span class="sxs-lookup"><span data-stu-id="4c3a3-104">Overview</span></span>

<span data-ttu-id="4c3a3-105">Tento článek popisuje hello kroky požadované tooinstall aktualizace 0.4 na pole virtuální zařízení StorSimple prostřednictvím hello místního webového uživatelského rozhraní a prostřednictvím hello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="4c3a3-105">This article describes hello steps required tooinstall Update 0.4 on your StorSimple Virtual Array via hello local web UI and via hello Azure portal.</span></span> <span data-ttu-id="4c3a3-106">Je nutné tooapply softwarové aktualizace a opravy hotfix tookeep pole virtuální zařízení StorSimple aktuální.</span><span class="sxs-lookup"><span data-stu-id="4c3a3-106">You need tooapply software updates or hotfixes tookeep your StorSimple Virtual Array up-to-date.</span></span> 

<span data-ttu-id="4c3a3-107">Mějte na paměti, který instaluje aktualizaci nebo opravu hotfix restartuje vaše zařízení.</span><span class="sxs-lookup"><span data-stu-id="4c3a3-107">Keep in mind that installing an update or hotfix restarts your device.</span></span> <span data-ttu-id="4c3a3-108">Zadaná, hello pole virtuální zařízení StorSimple je jeden uzel zařízení, dojde k narušení všechny vstupně-výstupních operací v průběhu a zařízení dojde k výpadku.</span><span class="sxs-lookup"><span data-stu-id="4c3a3-108">Given that hello StorSimple Virtual Array is a single node device, any I/O in progress is disrupted and your device experiences downtime.</span></span> 

<span data-ttu-id="4c3a3-109">Před instalací aktualizace, doporučujeme je provést hello svazky nebo sdílené složky offline na hello nejprve hostitele a pak hello zařízení.</span><span class="sxs-lookup"><span data-stu-id="4c3a3-109">Before you apply an update, we recommend that you take hello volumes or shares offline on hello host first and then hello device.</span></span> <span data-ttu-id="4c3a3-110">Tím se minimalizují možnost poškození dat.</span><span class="sxs-lookup"><span data-stu-id="4c3a3-110">This minimizes any possibility of data corruption.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="4c3a3-111">Pokud používáte Update 0.1 nebo verze GA softwaru, musíte použít opravu hotfix metoda hello prostřednictvím hello místního webového uživatelského rozhraní tooinstall update 0.3.</span><span class="sxs-lookup"><span data-stu-id="4c3a3-111">If you are running Update 0.1 or GA software versions, you must use hello hotfix method via hello local web UI tooinstall update 0.3.</span></span> <span data-ttu-id="4c3a3-112">Pokud používáte verzi Update 0,2 nebo novější, doporučujeme, aby instalaci aktualizací hello prostřednictvím hello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="4c3a3-112">If you are running Update 0.2 or later, we recommend that you install hello updates via hello Azure portal.</span></span>
 

## <a name="use-hello-local-web-ui"></a><span data-ttu-id="4c3a3-113">Použít místní hello webového uživatelského rozhraní</span><span class="sxs-lookup"><span data-stu-id="4c3a3-113">Use hello local web UI</span></span>

<span data-ttu-id="4c3a3-114">Existují dva kroky při použití hello místního webového uživatelského rozhraní:</span><span class="sxs-lookup"><span data-stu-id="4c3a3-114">There are two steps when using hello local web UI:</span></span>

* <span data-ttu-id="4c3a3-115">Stáhnout hello aktualizaci nebo opravu hotfix hello</span><span class="sxs-lookup"><span data-stu-id="4c3a3-115">Download hello update or hello hotfix</span></span>
* <span data-ttu-id="4c3a3-116">Nainstalujte hello aktualizaci nebo opravu hotfix hello</span><span class="sxs-lookup"><span data-stu-id="4c3a3-116">Install hello update or hello hotfix</span></span>

### <a name="download-hello-update-or-hello-hotfix"></a><span data-ttu-id="4c3a3-117">Stáhnout hello aktualizaci nebo opravu hotfix hello</span><span class="sxs-lookup"><span data-stu-id="4c3a3-117">Download hello update or hello hotfix</span></span>

<span data-ttu-id="4c3a3-118">Proveďte následující kroky toodownload hello softwarové aktualizace z katalogu služby Microsoft Update hello hello.</span><span class="sxs-lookup"><span data-stu-id="4c3a3-118">Perform hello following steps toodownload hello software update from hello Microsoft Update Catalog.</span></span>

#### <a name="toodownload-hello-update-or-hello-hotfix"></a><span data-ttu-id="4c3a3-119">toodownload hello aktualizace nebo hello oprav hotfix</span><span class="sxs-lookup"><span data-stu-id="4c3a3-119">toodownload hello update or hello hotfix</span></span>

1. <span data-ttu-id="4c3a3-120">Spusťte Internet Explorer a přejděte příliš[http://catalog.update.microsoft.com](http://catalog.update.microsoft.com).</span><span class="sxs-lookup"><span data-stu-id="4c3a3-120">Start Internet Explorer and navigate too[http://catalog.update.microsoft.com](http://catalog.update.microsoft.com).</span></span>

2. <span data-ttu-id="4c3a3-121">Pokud používáte hello katalogu služby Microsoft Update na tomto počítači poprvé, klikněte na tlačítko **nainstalovat** při výzvami tooinstall hello rozšíření katalogu služby Microsoft Update.</span><span class="sxs-lookup"><span data-stu-id="4c3a3-121">If this is your first time using hello Microsoft Update Catalog on this computer, click **Install** when prompted tooinstall hello Microsoft Update Catalog add-on.</span></span>

3. <span data-ttu-id="4c3a3-122">Do vyhledávacího pole hello hello katalogu služby Microsoft Update, zadejte číslo znalostní báze Knowledge Base (KB) hello hello opravy hotfix, kterou chcete toodownload.</span><span class="sxs-lookup"><span data-stu-id="4c3a3-122">In hello search box of hello Microsoft Update Catalog, enter hello Knowledge Base (KB) number of hello hotfix you want toodownload.</span></span> <span data-ttu-id="4c3a3-123">Zadejte **3216577** pro aktualizace 0.4 a pak klikněte na tlačítko **vyhledávání**.</span><span class="sxs-lookup"><span data-stu-id="4c3a3-123">Enter **3216577** for Update 0.4, and then click **Search**.</span></span>
   
    <span data-ttu-id="4c3a3-124">Hello opravu hotfix seznamu se zobrazí, například **aktualizace pole virtuálního zařízení StorSimple 0.4**.</span><span class="sxs-lookup"><span data-stu-id="4c3a3-124">hello hotfix listing appears, for example, **StorSimple Virtual Array Update 0.4**.</span></span>
   
    ![Prohledávání katalogu](./media/storsimple-virtual-array-install-update-04/download1.png)

4. <span data-ttu-id="4c3a3-126">Klikněte na tlačítko **Přidat**.</span><span class="sxs-lookup"><span data-stu-id="4c3a3-126">Click **Add**.</span></span> <span data-ttu-id="4c3a3-127">aktualizace Hello je přidána toohello košík.</span><span class="sxs-lookup"><span data-stu-id="4c3a3-127">hello update is added toohello basket.</span></span>

5. <span data-ttu-id="4c3a3-128">Klikněte na **Zobrazit košík**.</span><span class="sxs-lookup"><span data-stu-id="4c3a3-128">Click **View Basket**.</span></span>

6. <span data-ttu-id="4c3a3-129">Klikněte na **Stáhnout**.</span><span class="sxs-lookup"><span data-stu-id="4c3a3-129">Click **Download**.</span></span> <span data-ttu-id="4c3a3-130">Zadejte nebo **Procházet** tooa místního umístění, kam má hello stáhne tooappear.</span><span class="sxs-lookup"><span data-stu-id="4c3a3-130">Specify or **Browse** tooa local location where you want hello downloads tooappear.</span></span> <span data-ttu-id="4c3a3-131">Hello aktualizace se stáhnou toohello zadané umístění a umístěny v podsložce s hello stejný název jako hello aktualizace.</span><span class="sxs-lookup"><span data-stu-id="4c3a3-131">hello updates are downloaded toohello specified location and placed in a subfolder with hello same name as hello update.</span></span> <span data-ttu-id="4c3a3-132">Hello složce může být také síťové sdílené složky zkopírovaný tooa, který je dosažitelný z hello zařízení.</span><span class="sxs-lookup"><span data-stu-id="4c3a3-132">hello folder can also be copied tooa network share that is reachable from hello device.</span></span>

7. <span data-ttu-id="4c3a3-133">Otevřete hello zkopírovali složku, měli byste vidět soubor balíčku samostatné aktualizace Microsoft `WindowsTH-KB3011067-x64`.</span><span class="sxs-lookup"><span data-stu-id="4c3a3-133">Open hello copied folder, you should see a Microsoft Update Standalone Package file `WindowsTH-KB3011067-x64`.</span></span> <span data-ttu-id="4c3a3-134">Tento soubor je použité tooinstall hello aktualizaci nebo opravu hotfix.</span><span class="sxs-lookup"><span data-stu-id="4c3a3-134">This file is used tooinstall hello update or hotfix.</span></span>

### <a name="install-hello-update-or-hello-hotfix"></a><span data-ttu-id="4c3a3-135">Nainstalujte hello aktualizaci nebo opravu hotfix hello</span><span class="sxs-lookup"><span data-stu-id="4c3a3-135">Install hello update or hello hotfix</span></span>

<span data-ttu-id="4c3a3-136">Předchozí toohello instalace aktualizace nebo opravy hotfix, ujistěte se, že máte hello aktualizace nebo opravy hotfix hello stáhli buď místně na vašem hostiteli nebo přístupné přes síťové sdílené složky.</span><span class="sxs-lookup"><span data-stu-id="4c3a3-136">Prior toohello update or hotfix installation, make sure that you have hello update or hello hotfix downloaded either locally on your host or accessible via a network share.</span></span> 

<span data-ttu-id="4c3a3-137">Tato metoda tooinstall aktualizace použít na zařízení se systémem GA nebo aktualizovat 0,1 verze softwaru.</span><span class="sxs-lookup"><span data-stu-id="4c3a3-137">Use this method tooinstall updates on a device running GA or Update 0.1 software versions.</span></span> <span data-ttu-id="4c3a3-138">Tento postup trvá méně než 2 minuty toocomplete.</span><span class="sxs-lookup"><span data-stu-id="4c3a3-138">This procedure takes less than 2 minutes toocomplete.</span></span> <span data-ttu-id="4c3a3-139">Proveďte následující hello kroky tooinstall hello aktualizaci nebo opravu hotfix.</span><span class="sxs-lookup"><span data-stu-id="4c3a3-139">Perform hello following steps tooinstall hello update or hotfix.</span></span>

#### <a name="tooinstall-hello-update-or-hello-hotfix"></a><span data-ttu-id="4c3a3-140">tooinstall hello aktualizace nebo hello oprav hotfix</span><span class="sxs-lookup"><span data-stu-id="4c3a3-140">tooinstall hello update or hello hotfix</span></span>

1. <span data-ttu-id="4c3a3-141">Hello místního webového uživatelského rozhraní, přejděte v příliš**údržby** > **aktualizace softwaru**.</span><span class="sxs-lookup"><span data-stu-id="4c3a3-141">In hello local web UI, go too**Maintenance** > **Software Update**.</span></span>
   
    ![aktualizace zařízení](./media/storsimple-virtual-array-install-update/update1m.png)

2. <span data-ttu-id="4c3a3-143">V **cesta k souboru aktualizace**, zadejte název souboru hello pro aktualizaci hello nebo hello opravu hotfix.</span><span class="sxs-lookup"><span data-stu-id="4c3a3-143">In **Update file path**, enter hello file name for hello update or hello hotfix.</span></span> <span data-ttu-id="4c3a3-144">Pokud je umístěn ve sdílené síťové složce, procházet toohello aktualizaci nebo opravu hotfix instalační soubor.</span><span class="sxs-lookup"><span data-stu-id="4c3a3-144">You can also browse toohello update or hotfix installation file if placed on a network share.</span></span> <span data-ttu-id="4c3a3-145">Klikněte na tlačítko **Použít**.</span><span class="sxs-lookup"><span data-stu-id="4c3a3-145">Click **Apply**.</span></span>
   
    ![aktualizace zařízení](./media/storsimple-virtual-array-install-update/update2m.png)

3. <span data-ttu-id="4c3a3-147">Zobrazí se upozornění.</span><span class="sxs-lookup"><span data-stu-id="4c3a3-147">A warning is displayed.</span></span> <span data-ttu-id="4c3a3-148">Zadaný to je jeden uzel zařízení, po instalaci aktualizace hello, hello zařízení restartuje a dojde k výpadku.</span><span class="sxs-lookup"><span data-stu-id="4c3a3-148">Given this is a single node device, after hello update is applied, hello device restarts and there is downtime.</span></span> <span data-ttu-id="4c3a3-149">Klikněte na ikonu zaškrtnutí hello.</span><span class="sxs-lookup"><span data-stu-id="4c3a3-149">Click hello check icon.</span></span>
   
   ![aktualizace zařízení](./media/storsimple-virtual-array-install-update/update3m.png)

4. <span data-ttu-id="4c3a3-151">Hello aktualizace spustí.</span><span class="sxs-lookup"><span data-stu-id="4c3a3-151">hello update starts.</span></span> <span data-ttu-id="4c3a3-152">Po úspěšné aktualizaci hello zařízení restartuje.</span><span class="sxs-lookup"><span data-stu-id="4c3a3-152">After hello device is successfully updated, it restarts.</span></span> <span data-ttu-id="4c3a3-153">Hello místního uživatelského rozhraní není dostupný v této hodnotě duration.</span><span class="sxs-lookup"><span data-stu-id="4c3a3-153">hello local UI is not accessible in this duration.</span></span>
   
    ![aktualizace zařízení](./media/storsimple-virtual-array-install-update/update5m.png)

5. <span data-ttu-id="4c3a3-155">Po dokončení restartování hello přejdete toohello **přihlášení** stránky.</span><span class="sxs-lookup"><span data-stu-id="4c3a3-155">After hello restart is complete, you are taken toohello **Sign in** page.</span></span> <span data-ttu-id="4c3a3-156">tooverify, která se aktualizovala hello zařízení software, hello místního webového uživatelského rozhraní, přejděte příliš**údržby** > **aktualizace softwaru**.</span><span class="sxs-lookup"><span data-stu-id="4c3a3-156">tooverify that hello device software has updated, in hello local web UI, go too**Maintenance** > **Software Update**.</span></span> <span data-ttu-id="4c3a3-157">verze softwaru Hello zobrazí musí být **10.0.0.0.0.10289.0** pro aktualizaci 0.4.</span><span class="sxs-lookup"><span data-stu-id="4c3a3-157">hello displayed software version should be **10.0.0.0.0.10289.0** for Update 0.4.</span></span>
   
   > [!NOTE]
   > <span data-ttu-id="4c3a3-158">Jsme sestavy verze softwaru hello mírně jiným způsobem v hello místního webového uživatelského rozhraní a hello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="4c3a3-158">We report hello software versions in a slightly different way in hello local web UI and hello Azure portal.</span></span> <span data-ttu-id="4c3a3-159">Například hello místní webové sestavy nástroje uživatelského rozhraní **10.0.0.0.0.10289** a hello Azure portálu sestavy **10.0.10289.0** pro hello stejnou verzi.</span><span class="sxs-lookup"><span data-stu-id="4c3a3-159">For example, hello local web UI reports **10.0.0.0.0.10289** and hello Azure portal reports **10.0.10289.0** for hello same version.</span></span>
   
    ![aktualizace zařízení](./media/storsimple-virtual-array-install-update/update6m.png)

## <a name="use-hello-azure-portal"></a><span data-ttu-id="4c3a3-161">Hello použití portálu Azure</span><span class="sxs-lookup"><span data-stu-id="4c3a3-161">Use hello Azure portal</span></span>

<span data-ttu-id="4c3a3-162">Pokud aktualizace 0,2 a novější, doporučujeme instalovat aktualizace prostřednictvím hello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="4c3a3-162">If running Update 0.2 and later, we recommend that you install updates through hello Azure portal.</span></span> <span data-ttu-id="4c3a3-163">Postup portálu Hello vyžaduje tooscan uživatele hello, stáhněte a nainstalujte aktualizace hello.</span><span class="sxs-lookup"><span data-stu-id="4c3a3-163">hello portal procedure requires hello user tooscan, download, and then install hello updates.</span></span> <span data-ttu-id="4c3a3-164">Tento postup trvá asi 7 minut toocomplete.</span><span class="sxs-lookup"><span data-stu-id="4c3a3-164">This procedure takes around 7 minutes toocomplete.</span></span> <span data-ttu-id="4c3a3-165">Proveďte následující hello kroky tooinstall hello aktualizaci nebo opravu hotfix.</span><span class="sxs-lookup"><span data-stu-id="4c3a3-165">Perform hello following steps tooinstall hello update or hotfix.</span></span>

[!INCLUDE [storsimple-virtual-array-install-update-via-portal](../../includes/storsimple-virtual-array-install-update-via-portal-04.md)]

<span data-ttu-id="4c3a3-166">Hello instalace po dokončení (jako je označené podle stavu úlohy na 100 %), přejděte tooyour služby StorSimple Manager zařízení.</span><span class="sxs-lookup"><span data-stu-id="4c3a3-166">After hello installation is complete (as indicated by job status at 100 %), go tooyour StorSimple Device Manager service.</span></span> <span data-ttu-id="4c3a3-167">Vyberte **zařízení** a pak vyberte a klikněte na zařízení hello chcete tooupdate hello seznamu zařízení připojených toothis služby.</span><span class="sxs-lookup"><span data-stu-id="4c3a3-167">Select **Devices** and then select and click hello device you want tooupdate from hello list of devices connected toothis service.</span></span> <span data-ttu-id="4c3a3-168">V hello **nastavení** okně přejděte příliš**spravovat** a vyberte **aktualizace zařízení**.</span><span class="sxs-lookup"><span data-stu-id="4c3a3-168">In hello **Settings** blade, go too**Manage** section and select **Device updates**.</span></span> <span data-ttu-id="4c3a3-169">verze softwaru Hello zobrazí musí být **10.0.10289.0**.</span><span class="sxs-lookup"><span data-stu-id="4c3a3-169">hello displayed software version should be **10.0.10289.0**.</span></span>


## <a name="next-steps"></a><span data-ttu-id="4c3a3-170">Další kroky</span><span class="sxs-lookup"><span data-stu-id="4c3a3-170">Next steps</span></span>

<span data-ttu-id="4c3a3-171">Další informace o [Správa pole virtuální zařízení StorSimple](storsimple-ova-web-ui-admin.md).</span><span class="sxs-lookup"><span data-stu-id="4c3a3-171">Learn more about [administering your StorSimple Virtual Array](storsimple-ova-web-ui-admin.md).</span></span>

