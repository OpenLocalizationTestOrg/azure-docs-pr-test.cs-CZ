---
title: "aaaGet Začínáme s Azure Mobile Engagementem pro nasazení Unity iOS"
description: "Zjistěte, jak toouse Azure Mobile Engagement s analytickými funkcemi a nabízenými oznámeními pro aplikace Unity nasazované tooiOS zařízení."
services: mobile-engagement
documentationcenter: unity
author: piyushjo
manager: erikre
editor: 
ms.assetid: 7ddfbac3-8d13-4ebe-b061-c865f357297f
ms.service: mobile-engagement
ms.workload: mobile
ms.tgt_pltfrm: mobile-unity-ios
ms.devlang: dotnet
ms.topic: hero-article
ms.date: 08/19/2016
ms.author: piyushjo
ms.openlocfilehash: f4247b0a9240cbe2bf1a6618388919d3554c07fb
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-azure-mobile-engagement-for-unity-ios-deployment"></a><span data-ttu-id="7a281-103">Začínáme s Azure Mobile Engagementem pro nasazení aplikací Unity v systému iOS</span><span class="sxs-lookup"><span data-stu-id="7a281-103">Get Started with Azure Mobile Engagement for Unity iOS deployment</span></span>
[!INCLUDE [Hero tutorial switcher](../../includes/mobile-engagement-hero-tutorial-switcher.md)]

<span data-ttu-id="7a281-104">Toto téma ukazuje, jak Azure Mobile Engagement toounderstand toouse používání aplikace a jak toosend nabízená oznámení toosegmented uživatelům aplikace Unity při nasazení tooan zařízení iOS.</span><span class="sxs-lookup"><span data-stu-id="7a281-104">This topic shows you how toouse Azure Mobile Engagement toounderstand your app usage and how toosend push notifications toosegmented users of a Unity application when deploying tooan iOS device.</span></span>
<span data-ttu-id="7a281-105">Tento kurz používá hello classic vrátit Unity míč kurzu jako výchozí bod hello.</span><span class="sxs-lookup"><span data-stu-id="7a281-105">This tutorial uses hello classic Unity Roll a Ball tutorial as hello starting point.</span></span> <span data-ttu-id="7a281-106">Postupujte podle kroků hello v tomto [kurzu](mobile-engagement-unity-roll-a-ball.md) před pokračováním hello integraci Mobile Engagementu Představujeme v kurzu hello níže.</span><span class="sxs-lookup"><span data-stu-id="7a281-106">You should follow hello steps in this [tutorial](mobile-engagement-unity-roll-a-ball.md) before proceeding with hello Mobile Engagement integration we showcase in hello tutorial below.</span></span> 

<span data-ttu-id="7a281-107">Tento kurz vyžaduje hello následující:</span><span class="sxs-lookup"><span data-stu-id="7a281-107">This tutorial requires hello following:</span></span>

* [<span data-ttu-id="7a281-108">Unity Editor</span><span class="sxs-lookup"><span data-stu-id="7a281-108">Unity Editor</span></span>](http://unity3d.com/get-unity)
* [<span data-ttu-id="7a281-109">Mobile Engagement Unity SDK</span><span class="sxs-lookup"><span data-stu-id="7a281-109">Mobile Engagement Unity SDK</span></span>](https://aka.ms/azmeunitysdk)
* <span data-ttu-id="7a281-110">XCode Editor</span><span class="sxs-lookup"><span data-stu-id="7a281-110">XCode Editor</span></span>

> [!NOTE]
> <span data-ttu-id="7a281-111">toocomplete tento kurz, musíte mít aktivní účet Azure.</span><span class="sxs-lookup"><span data-stu-id="7a281-111">toocomplete this tutorial, you must have an active Azure account.</span></span> <span data-ttu-id="7a281-112">Pokud účet nemáte, můžete si během několika minut vytvořit bezplatný zkušební účet.</span><span class="sxs-lookup"><span data-stu-id="7a281-112">If you don't have an account, you can create a free trial account in just a couple of minutes.</span></span> <span data-ttu-id="7a281-113">Podrobnosti najdete v článku [Bezplatná zkušební verze Azure](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A0E0E5C02&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdocumentation%2Farticles%2Fmobile-engagement-unity-ios-get-started).</span><span class="sxs-lookup"><span data-stu-id="7a281-113">For details, see [Azure Free Trial](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A0E0E5C02&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdocumentation%2Farticles%2Fmobile-engagement-unity-ios-get-started).</span></span>
> 
> 

## <span data-ttu-id="7a281-114"><a id="setup-azme"></a>Nastavení Mobile Engagementu pro vaši aplikaci pro iOS</span><span class="sxs-lookup"><span data-stu-id="7a281-114"><a id="setup-azme"></a>Setup Mobile Engagement for your iOS app</span></span>
[!INCLUDE [Create Mobile Engagement App in Portal](../../includes/mobile-engagement-create-app-in-portal-new.md)]

## <span data-ttu-id="7a281-115"><a id="connecting-app"></a>Připojit vaše aplikace toohello Mobile Engagement back-end</span><span class="sxs-lookup"><span data-stu-id="7a281-115"><a id="connecting-app"></a>Connect your app toohello Mobile Engagement backend</span></span>
### <a name="import-hello-unity-package"></a><span data-ttu-id="7a281-116">Import balíčku Unity hello</span><span class="sxs-lookup"><span data-stu-id="7a281-116">Import hello Unity package</span></span>
1. <span data-ttu-id="7a281-117">Stáhnout hello [balíček Mobile Engagement Unity](https://aka.ms/azmeunitysdk) a uložte ho tooyour místního počítače.</span><span class="sxs-lookup"><span data-stu-id="7a281-117">Download hello [Mobile Engagement Unity package](https://aka.ms/azmeunitysdk) and save it tooyour local machine.</span></span> 
2. <span data-ttu-id="7a281-118">Přejděte příliš**prostředků -> Import balíčku -> vlastní balíček** a vyberte hello balíčku, které jste si stáhli v hello výše krok.</span><span class="sxs-lookup"><span data-stu-id="7a281-118">Go too**Assets -> Import Package -> Custom Package** and select hello package you downloaded in hello above step.</span></span> 
   
    ![][70] 
3. <span data-ttu-id="7a281-119">Zkontrolujte, zda jsou vybrány všechny soubory, a klikněte na tlačítko **Import**.</span><span class="sxs-lookup"><span data-stu-id="7a281-119">Make sure all files are selected and click **Import** button.</span></span> 
   
    ![][71] 
4. <span data-ttu-id="7a281-120">Po úspěšném importu, zobrazí se soubory SDK hello importovat ve vašem projektu.</span><span class="sxs-lookup"><span data-stu-id="7a281-120">Once Import is successful, you will see hello imported SDK files in your project.</span></span>  
   
    ![][72] 

### <a name="update-hello-engagementconfiguration"></a><span data-ttu-id="7a281-121">Hello aktualizace EngagementConfiguration</span><span class="sxs-lookup"><span data-stu-id="7a281-121">Update hello EngagementConfiguration</span></span>
1. <span data-ttu-id="7a281-122">Otevřete hello **EngagementConfiguration** soubor skriptu hello SDK složku a aktualizace hello **IOS\_připojení\_řetězec** s jste dříve získali hello připojovací řetězec z hello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="7a281-122">Open up hello **EngagementConfiguration** script file from hello SDK folder and update hello **IOS\_CONNECTION\_STRING** with hello connection string you obtained earlier from hello Azure portal.</span></span>  
   
    ![][73]
2. <span data-ttu-id="7a281-123">Uložte soubor hello.</span><span class="sxs-lookup"><span data-stu-id="7a281-123">Save hello file.</span></span> 

### <a name="configure-hello-app-for-basic-tracking"></a><span data-ttu-id="7a281-124">Konfigurace aplikace hello pro základní sledování</span><span class="sxs-lookup"><span data-stu-id="7a281-124">Configure hello app for basic tracking</span></span>
1. <span data-ttu-id="7a281-125">Otevřete hello **PlayerController** přidružen skript toohello objektu Player pro úpravy.</span><span class="sxs-lookup"><span data-stu-id="7a281-125">Open up hello **PlayerController** script attached toohello Player object for editing.</span></span> 
2. <span data-ttu-id="7a281-126">Přidat hello následující příkaz using:</span><span class="sxs-lookup"><span data-stu-id="7a281-126">Add hello following using statement:</span></span>
   
        using Microsoft.Azure.Engagement.Unity;
3. <span data-ttu-id="7a281-127">Přidejte následující toohello hello `Start()` – metoda</span><span class="sxs-lookup"><span data-stu-id="7a281-127">Add hello following toohello `Start()` method</span></span>
   
        EngagementAgent.Initialize();
        EngagementAgent.StartActivity("Home");

### <a name="deploy-and-run-hello-app"></a><span data-ttu-id="7a281-128">Nasazení a spuštění aplikace hello</span><span class="sxs-lookup"><span data-stu-id="7a281-128">Deploy and run hello app</span></span>
1. <span data-ttu-id="7a281-129">Připojte počítač tooyour zařízení s iOS.</span><span class="sxs-lookup"><span data-stu-id="7a281-129">Connect an iOS device tooyour machine.</span></span> 
2. <span data-ttu-id="7a281-130">Klikněte na položky **File (Soubor) -> Build Settings (Nastavení sestavení)**</span><span class="sxs-lookup"><span data-stu-id="7a281-130">Open up **File -> Build Settings**</span></span> 
   
    ![][40]
3. <span data-ttu-id="7a281-131">Vyberte **iOS** a potom klikněte na **Switch Platform** (Přepnout platformu)</span><span class="sxs-lookup"><span data-stu-id="7a281-131">Select **iOS** and then click on **Switch Platform**</span></span>
   
    ![][41]
   
    ![][42]
4. <span data-ttu-id="7a281-132">Klikněte na **Player settings** (Nastavení hráče) a zadejte platný identifikátor svazku.</span><span class="sxs-lookup"><span data-stu-id="7a281-132">Click on **Player settings** and provide a valid Bundle Identifier.</span></span> 
   
    ![][53]
5. <span data-ttu-id="7a281-133">Nakonec klikněte na **Build And Run** (Sestavit a spustit).</span><span class="sxs-lookup"><span data-stu-id="7a281-133">Finally click on **Build And Run**</span></span>
   
    ![][54]
6. <span data-ttu-id="7a281-134">Může být výzva tooprovide balíček iOS hello toostore název složky.</span><span class="sxs-lookup"><span data-stu-id="7a281-134">You may be asked tooprovide a folder name toostore hello iOS package.</span></span> 
   
    ![][43]
7. <span data-ttu-id="7a281-135">Pokud všechno půjde dobře, projekt hello se zkompiluje a jste měli ho otevřít v aplikaci XCode.</span><span class="sxs-lookup"><span data-stu-id="7a281-135">If everything goes fine, then hello project will be compiled and you should open it up on your XCode application.</span></span> 
8. <span data-ttu-id="7a281-136">Ujistěte se, že hello **identifikátor svazku** správné v projektu hello.</span><span class="sxs-lookup"><span data-stu-id="7a281-136">Make sure that hello **Bundle identifier** is correct in hello project.</span></span>  
   
    ![][75]
9. <span data-ttu-id="7a281-137">Teď spusťte hello aplikaci v XCode, tak, aby balíček hello je nasazené tooyour připojených zařízení a v telefonu by měla zobrazit vaše hra Unity!</span><span class="sxs-lookup"><span data-stu-id="7a281-137">Now run hello app in XCode so that hello package is deployed tooyour connected device and you should see your Unity game on your phone!</span></span> 

## <span data-ttu-id="7a281-138"><a id="monitor"></a>Připojení aplikace se sledováním v reálném čase</span><span class="sxs-lookup"><span data-stu-id="7a281-138"><a id="monitor"></a>Connect app with real-time monitoring</span></span>
[!INCLUDE [Connect app with real-time monitoring](../../includes/mobile-engagement-connect-app-with-monitor.md)]

## <span data-ttu-id="7a281-139"><a id="integrate-push"></a>Povolení nabízených oznámení a zasílání zpráv v aplikaci</span><span class="sxs-lookup"><span data-stu-id="7a281-139"><a id="integrate-push"></a>Enable push notifications and in-app messaging</span></span>
<span data-ttu-id="7a281-140">Mobile Engagement umožňuje toointeract s uživateli a REACH s nabízená oznámení a zasílání zpráv v kontextu hello kampaní v aplikaci.</span><span class="sxs-lookup"><span data-stu-id="7a281-140">Mobile Engagement allows you toointeract with your users and REACH with push notifications and in-app messaging in hello context of campaigns.</span></span> <span data-ttu-id="7a281-141">Tento modul je hello portálu Mobile Engagement nazývá REACH.</span><span class="sxs-lookup"><span data-stu-id="7a281-141">This module is called REACH in hello Mobile Engagement portal.</span></span>
<span data-ttu-id="7a281-142">Nemáte toodo žádné další konfigurace v aplikaci tooreceive oznámení a je již nastavena pro ni.</span><span class="sxs-lookup"><span data-stu-id="7a281-142">You don't have toodo any additional configuration in your app tooreceive notifications and it is already setup for it.</span></span>

[!INCLUDE [mobile-engagement-ios-send-push-push](../../includes/mobile-engagement-ios-send-push.md)]

<!-- Images. -->
[40]: ./media/mobile-engagement-unity-ios-get-started/40.png
[41]: ./media/mobile-engagement-unity-ios-get-started/41.png
[42]: ./media/mobile-engagement-unity-ios-get-started/42.png
[43]: ./media/mobile-engagement-unity-ios-get-started/43.png
[53]: ./media/mobile-engagement-unity-ios-get-started/53.png
[54]: ./media/mobile-engagement-unity-ios-get-started/54.png
[70]: ./media/mobile-engagement-unity-ios-get-started/70.png
[71]: ./media/mobile-engagement-unity-ios-get-started/71.png
[72]: ./media/mobile-engagement-unity-ios-get-started/72.png
[73]: ./media/mobile-engagement-unity-ios-get-started/73.png
[74]: ./media/mobile-engagement-unity-ios-get-started/74.png
[75]: ./media/mobile-engagement-unity-ios-get-started/75.png
