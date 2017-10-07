---
title: "aaaGet Začínáme s Azure Mobile Engagementem pro nasazení Unity Android"
description: "Zjistěte, jak toouse Azure Mobile Engagement s analytickými funkcemi a nabízenými oznámeními pro aplikace Unity nasazované tooiOS zařízení."
services: mobile-engagement
documentationcenter: unity
author: piyushjo
manager: erikre
editor: 
ms.assetid: d5f0ef79-be00-4cec-97a5-a0b2fdaa380e
ms.service: mobile-engagement
ms.workload: mobile
ms.tgt_pltfrm: mobile-unity-android
ms.devlang: dotnet
ms.topic: hero-article
ms.date: 08/19/2016
ms.author: piyushjo
ms.openlocfilehash: c4d34691daeb7544b11c2d6895b2474af0f902b4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-azure-mobile-engagement-for-unity-android-deployment"></a><span data-ttu-id="7fccb-103">Začínáme s Azure Mobile Engagementem pro nasazení Unity Android</span><span class="sxs-lookup"><span data-stu-id="7fccb-103">Get Started with Azure Mobile Engagement for Unity Android deployment</span></span>
[!INCLUDE [Hero tutorial switcher](../../includes/mobile-engagement-hero-tutorial-switcher.md)]

<span data-ttu-id="7fccb-104">Toto téma ukazuje, jak Azure Mobile Engagement toounderstand toouse používání aplikace a jak toosend nabízená oznámení toosegmented uživatelům aplikace Unity při nasazení tooan zařízení se systémem Android.</span><span class="sxs-lookup"><span data-stu-id="7fccb-104">This topic shows you how toouse Azure Mobile Engagement toounderstand your app usage and how toosend push notifications toosegmented users of a Unity application when deploying tooan Android device.</span></span>
<span data-ttu-id="7fccb-105">Tento kurz používá hello classic vrátit Unity míč kurzu jako výchozí bod hello.</span><span class="sxs-lookup"><span data-stu-id="7fccb-105">This tutorial uses hello classic Unity Roll a Ball tutorial as hello starting point.</span></span> <span data-ttu-id="7fccb-106">Postupujte podle kroků hello v tomto [kurzu](mobile-engagement-unity-roll-a-ball.md) před pokračováním hello integraci Mobile Engagementu Představujeme v kurzu hello níže.</span><span class="sxs-lookup"><span data-stu-id="7fccb-106">You should follow hello steps in this [tutorial](mobile-engagement-unity-roll-a-ball.md) before proceeding with hello Mobile Engagement integration we showcase in hello tutorial below.</span></span> 

<span data-ttu-id="7fccb-107">Tento kurz vyžaduje hello následující:</span><span class="sxs-lookup"><span data-stu-id="7fccb-107">This tutorial requires hello following:</span></span>

* [<span data-ttu-id="7fccb-108">Unity Editor</span><span class="sxs-lookup"><span data-stu-id="7fccb-108">Unity Editor</span></span>](http://unity3d.com/get-unity)
* [<span data-ttu-id="7fccb-109">Mobile Engagement Unity SDK</span><span class="sxs-lookup"><span data-stu-id="7fccb-109">Mobile Engagement Unity SDK</span></span>](https://aka.ms/azmeunitysdk)
* <span data-ttu-id="7fccb-110">Google Android SDK</span><span class="sxs-lookup"><span data-stu-id="7fccb-110">Google Android SDK</span></span>

> [!NOTE]
> <span data-ttu-id="7fccb-111">toocomplete tento kurz, musíte mít aktivní účet Azure.</span><span class="sxs-lookup"><span data-stu-id="7fccb-111">toocomplete this tutorial, you must have an active Azure account.</span></span> <span data-ttu-id="7fccb-112">Pokud účet nemáte, můžete si během několika minut vytvořit bezplatný zkušební účet.</span><span class="sxs-lookup"><span data-stu-id="7fccb-112">If you don't have an account, you can create a free trial account in just a couple of minutes.</span></span> <span data-ttu-id="7fccb-113">Podrobnosti najdete v článku [Bezplatná zkušební verze Azure](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A0E0E5C02&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdocumentation%2Farticles%2Fmobile-engagement-unity-android-get-started).</span><span class="sxs-lookup"><span data-stu-id="7fccb-113">For details, see [Azure Free Trial](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A0E0E5C02&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdocumentation%2Farticles%2Fmobile-engagement-unity-android-get-started).</span></span>
> 
> 

## <span data-ttu-id="7fccb-114"><a id="setup-azme"></a>Nastavení Mobile Engagementu pro vaši aplikaci pro Android</span><span class="sxs-lookup"><span data-stu-id="7fccb-114"><a id="setup-azme"></a>Setup Mobile Engagement for your Android app</span></span>
[!INCLUDE [Create Mobile Engagement App in Portal](../../includes/mobile-engagement-create-app-in-portal-new.md)]

## <span data-ttu-id="7fccb-115"><a id="connecting-app"></a>Připojit vaše aplikace toohello Mobile Engagement back-end</span><span class="sxs-lookup"><span data-stu-id="7fccb-115"><a id="connecting-app"></a>Connect your app toohello Mobile Engagement backend</span></span>
### <a name="import-hello-unity-package"></a><span data-ttu-id="7fccb-116">Import balíčku Unity hello</span><span class="sxs-lookup"><span data-stu-id="7fccb-116">Import hello Unity package</span></span>
1. <span data-ttu-id="7fccb-117">Stáhnout hello [balíček Mobile Engagement Unity](https://aka.ms/azmeunitysdk) a uložte ho tooyour místního počítače.</span><span class="sxs-lookup"><span data-stu-id="7fccb-117">Download hello [Mobile Engagement Unity package](https://aka.ms/azmeunitysdk) and save it tooyour local machine.</span></span> 
2. <span data-ttu-id="7fccb-118">Přejděte příliš**prostředků -> Import balíčku -> vlastní balíček** a vyberte hello balíčku, které jste si stáhli v hello výše krok.</span><span class="sxs-lookup"><span data-stu-id="7fccb-118">Go too**Assets -> Import Package -> Custom Package** and select hello package you downloaded in hello above step.</span></span> 
   
    ![][70] 
3. <span data-ttu-id="7fccb-119">Zkontrolujte, zda jsou vybrány všechny soubory, a klikněte na tlačítko **Import**.</span><span class="sxs-lookup"><span data-stu-id="7fccb-119">Make sure all files are selected and click **Import** button.</span></span> 
   
    ![][71] 
4. <span data-ttu-id="7fccb-120">Po úspěšném importu, zobrazí se soubory SDK hello importovat ve vašem projektu.</span><span class="sxs-lookup"><span data-stu-id="7fccb-120">Once Import is successful, you will see hello imported SDK files in your project.</span></span>  
   
    ![][72] 

### <a name="update-hello-engagementconfiguration"></a><span data-ttu-id="7fccb-121">Hello aktualizace EngagementConfiguration</span><span class="sxs-lookup"><span data-stu-id="7fccb-121">Update hello EngagementConfiguration</span></span>
1. <span data-ttu-id="7fccb-122">Otevřete hello **EngagementConfiguration** soubor skriptu hello SDK složku a aktualizace hello **ANDROID\_připojení\_řetězec** s jste získali hello připojovací řetězec dříve z hello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="7fccb-122">Open up hello **EngagementConfiguration** script file from hello SDK folder and update hello **ANDROID\_CONNECTION\_STRING** with hello connection string you obtained earlier from hello Azure portal.</span></span>  
   
    ![][73]
2. <span data-ttu-id="7fccb-123">Uložte soubor hello</span><span class="sxs-lookup"><span data-stu-id="7fccb-123">Save hello file</span></span> 
3. <span data-ttu-id="7fccb-124">Spusťte **File (Soubor) -> Engagement -> Generate Android Manifest (Generovat manifest Android)**.</span><span class="sxs-lookup"><span data-stu-id="7fccb-124">Execute **File -> Engagement -> Generate Android Manifest**.</span></span> <span data-ttu-id="7fccb-125">Toto je hello plugin přidaný sadou Mobile Engagement SDK hello a kliknutí na tlačítko se automaticky aktualizuje nastavení projektu.</span><span class="sxs-lookup"><span data-stu-id="7fccb-125">This is hello plugin added by hello Mobile Engagement SDK and clicking on it will automatically update your project settings.</span></span> 
   
    ![][74]

> [!IMPORTANT]
> <span data-ttu-id="7fccb-126">Ujistěte se, že tooexecute to při každé aktualizaci hello **EngagementConfiguration** souboru vaše změny se neprojeví v aplikaci hello.</span><span class="sxs-lookup"><span data-stu-id="7fccb-126">Make sure tooexecute this every time you update hello **EngagementConfiguration** file otherwise your changes will not be reflected in hello app.</span></span> 
> 
> 

### <a name="configure-hello-app-for-basic-tracking"></a><span data-ttu-id="7fccb-127">Konfigurace aplikace hello pro základní sledování</span><span class="sxs-lookup"><span data-stu-id="7fccb-127">Configure hello app for basic tracking</span></span>
1. <span data-ttu-id="7fccb-128">Otevřete hello **PlayerController** přidružen skript toohello objektu Player pro úpravy.</span><span class="sxs-lookup"><span data-stu-id="7fccb-128">Open up hello **PlayerController** script attached toohello Player object for editing.</span></span> 
2. <span data-ttu-id="7fccb-129">Přidat hello následující příkaz using:</span><span class="sxs-lookup"><span data-stu-id="7fccb-129">Add hello following using statement:</span></span>
   
        using Microsoft.Azure.Engagement.Unity;
3. <span data-ttu-id="7fccb-130">Přidejte následující toohello hello `Start()` – metoda</span><span class="sxs-lookup"><span data-stu-id="7fccb-130">Add hello following toohello `Start()` method</span></span>
   
        EngagementAgent.Initialize();
        EngagementAgent.StartActivity("Home");

### <a name="deploy-and-run-hello-app"></a><span data-ttu-id="7fccb-131">Nasazení a spuštění aplikace hello</span><span class="sxs-lookup"><span data-stu-id="7fccb-131">Deploy and run hello app</span></span>
<span data-ttu-id="7fccb-132">Ujistěte se, že máte nainstalovaný na počítači před pokusem o toodeploy toto zařízení tooyour aplikace Unity Android SDK.</span><span class="sxs-lookup"><span data-stu-id="7fccb-132">Make sure that you have Android SDK installed on your machine before attempting toodeploy this Unity app tooyour device.</span></span> 

1. <span data-ttu-id="7fccb-133">Připojte počítači tooyour zařízení se systémem Android.</span><span class="sxs-lookup"><span data-stu-id="7fccb-133">Connect an Android device tooyour machine.</span></span> 
2. <span data-ttu-id="7fccb-134">Klikněte na položky **File (Soubor) -> Build Settings (Nastavení sestavení)**</span><span class="sxs-lookup"><span data-stu-id="7fccb-134">Open up **File -> Build Settings**</span></span> 
   
    ![][40]
3. <span data-ttu-id="7fccb-135">Vyberte **Android** a potom klikněte na **Switch Platform** (Přepnout platformu).</span><span class="sxs-lookup"><span data-stu-id="7fccb-135">Select **Android** and then click on **Switch Platform**</span></span>
   
    ![][51]
   
    ![][52]
4. <span data-ttu-id="7fccb-136">Klikněte na **Player settings** (Nastavení hráče) a zadejte platný identifikátor svazku.</span><span class="sxs-lookup"><span data-stu-id="7fccb-136">Click on **Player settings** and provide a valid Bundle Identifier.</span></span> 
   
    ![][53]
5. <span data-ttu-id="7fccb-137">Nakonec klikněte na **Build And Run** (Sestavit a spustit).</span><span class="sxs-lookup"><span data-stu-id="7fccb-137">Finally click on **Build And Run**</span></span>
   
    ![][54]
6. <span data-ttu-id="7fccb-138">Může být výzva tooprovide složky název toostore hello Android balíček.</span><span class="sxs-lookup"><span data-stu-id="7fccb-138">You may be asked tooprovide a folder name toostore hello Android package.</span></span> 
7. <span data-ttu-id="7fccb-139">Pokud všechno půjde dobře, balíček hello bude nasazený tooyour připojené zařízení a které by měla zobrazit vaše hra Unity na váš telefon!</span><span class="sxs-lookup"><span data-stu-id="7fccb-139">If everything goes fine, then hello package will be deployed tooyour connected device and you should see your Unity game on your phone!</span></span> 

## <span data-ttu-id="7fccb-140"><a id="monitor"></a>Připojení aplikace se sledováním v reálném čase</span><span class="sxs-lookup"><span data-stu-id="7fccb-140"><a id="monitor"></a>Connect app with real-time monitoring</span></span>
[!INCLUDE [Connect app with real-time monitoring](../../includes/mobile-engagement-connect-app-with-monitor.md)]

## <span data-ttu-id="7fccb-141"><a id="integrate-push"></a>Povolení nabízených oznámení a zasílání zpráv v aplikaci</span><span class="sxs-lookup"><span data-stu-id="7fccb-141"><a id="integrate-push"></a>Enable push notifications and in-app messaging</span></span>
[!INCLUDE [Enable Google Cloud Messaging](../../includes/mobile-engagement-enable-google-cloud-messaging.md)]

### <a name="update-hello-engagementconfiguration"></a><span data-ttu-id="7fccb-142">Hello aktualizace EngagementConfiguration</span><span class="sxs-lookup"><span data-stu-id="7fccb-142">Update hello EngagementConfiguration</span></span>
1. <span data-ttu-id="7fccb-143">Otevřete hello **EngagementConfiguration** soubor skriptu hello SDK složku a aktualizace hello **ANDROID\_GOOGLE\_číslo** s hello **projektu Google Číslo** jste dříve získali z portálu Google Cloud Developer hello.</span><span class="sxs-lookup"><span data-stu-id="7fccb-143">Open up hello **EngagementConfiguration** script file from hello SDK folder and update hello **ANDROID\_GOOGLE\_NUMBER** with hello **Google Project Number** you obtained earlier from hello Google Cloud Developer portal.</span></span> <span data-ttu-id="7fccb-144">Toto je řetězec hodnotu, ujistěte se, že tooenclose ji do uvozovek.</span><span class="sxs-lookup"><span data-stu-id="7fccb-144">This is a string value so make sure tooenclose it in double quotes.</span></span> 
   
    ![][75]
2. <span data-ttu-id="7fccb-145">Uložte soubor hello.</span><span class="sxs-lookup"><span data-stu-id="7fccb-145">Save hello file.</span></span> 
3. <span data-ttu-id="7fccb-146">Spusťte **File (Soubor) -> Engagement -> Generate Android Manifest (Generovat manifest Android)**.</span><span class="sxs-lookup"><span data-stu-id="7fccb-146">Execute **File -> Engagement -> Generate Android Manifest**.</span></span> <span data-ttu-id="7fccb-147">Toto je hello plugin přidaný sadou Mobile Engagement SDK hello a kliknutí na tlačítko se automaticky aktualizuje nastavení projektu.</span><span class="sxs-lookup"><span data-stu-id="7fccb-147">This is hello plugin added by hello Mobile Engagement SDK and clicking on it will automatically update your project settings.</span></span> 
   
    ![][74]

### <a name="configure-hello-app-tooreceive-notifications"></a><span data-ttu-id="7fccb-148">Konfigurace oznámení tooreceive aplikace hello</span><span class="sxs-lookup"><span data-stu-id="7fccb-148">Configure hello app tooreceive notifications</span></span>
1. <span data-ttu-id="7fccb-149">Otevřete hello **PlayerController** přidružen skript toohello objektu Player pro úpravy.</span><span class="sxs-lookup"><span data-stu-id="7fccb-149">Open up hello **PlayerController** script attached toohello Player object for editing.</span></span> 
2. <span data-ttu-id="7fccb-150">Přidejte následující toohello hello `Start()` – metoda</span><span class="sxs-lookup"><span data-stu-id="7fccb-150">Add hello following toohello `Start()` method</span></span>
   
        EngagementReachAgent.Initialize();
3. <span data-ttu-id="7fccb-151">Teď, když hello aplikace se aktualizuje, nasazení a spuštění aplikace hello na zařízení podle níže uvedených pokynů hello.</span><span class="sxs-lookup"><span data-stu-id="7fccb-151">Now that hello app is updated, deploy and run hello app on a device per hello instructions provided below.</span></span> 

[!INCLUDE [Send notification from portal](../../includes/mobile-engagement-android-send-push-from-portal.md)]

<!-- Images -->
[40]: ./media/mobile-engagement-unity-android-get-started/40.png
[70]: ./media/mobile-engagement-unity-android-get-started/70.png
[71]: ./media/mobile-engagement-unity-android-get-started/71.png
[72]: ./media/mobile-engagement-unity-android-get-started/72.png
[73]: ./media/mobile-engagement-unity-android-get-started/73.png
[74]: ./media/mobile-engagement-unity-android-get-started/74.png
[75]: ./media/mobile-engagement-unity-android-get-started/75.png
[51]: ./media/mobile-engagement-unity-android-get-started/51.png
[52]: ./media/mobile-engagement-unity-android-get-started/52.png
[53]: ./media/mobile-engagement-unity-android-get-started/53.png
[54]: ./media/mobile-engagement-unity-android-get-started/54.png
