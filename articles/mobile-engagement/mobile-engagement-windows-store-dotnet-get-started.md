---
title: "aaaGet začít s Windows Universal aplikace Azure Mobile Engagement"
description: "Zjistěte, jak toouse Azure Mobile Engagement s analytickými funkcemi a nabízenými oznámeními pro univerzální aplikace Windows."
services: mobile-engagement
documentationcenter: windows
author: piyushjo
manager: erikre
editor: 
ms.assetid: 48103867-7f64-4646-b019-42bd797d38e2
ms.service: mobile-engagement
ms.workload: mobile
ms.tgt_pltfrm: mobile-windows-store
ms.devlang: dotnet
ms.topic: hero-article
ms.date: 08/12/2016
ms.author: piyushjo;ricksal
ms.openlocfilehash: 8224a6d3789cfe4784bbc9472005f9eddb94a8b8
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-azure-mobile-engagement-for-windows-universal-apps"></a><span data-ttu-id="432d7-103">Začínáme s Azure Mobile Engagementem pro univerzální aplikace pro Windows</span><span class="sxs-lookup"><span data-stu-id="432d7-103">Get started with Azure Mobile Engagement for Windows Universal Apps</span></span>
[!INCLUDE [Hero tutorial switcher](../../includes/mobile-engagement-hero-tutorial-switcher.md)]

<span data-ttu-id="432d7-104">Toto téma ukazuje, jak Azure Mobile Engagement toounderstand toouse využití aplikací a odesílat nabízená oznámení uživatelům toosegmented univerzální aplikace pro Windows.</span><span class="sxs-lookup"><span data-stu-id="432d7-104">This topic shows you how toouse Azure Mobile Engagement toounderstand your app usage and send push notifications toosegmented users of a Windows Universal application.</span></span>
<span data-ttu-id="432d7-105">Tento kurz představuje scénář hello jednoduchého vysílání přes Mobile Engagement.</span><span class="sxs-lookup"><span data-stu-id="432d7-105">This tutorial demonstrates hello simple broadcast scenario using Mobile Engagement.</span></span> <span data-ttu-id="432d7-106">Vytvoříte prázdnou univerzální aplikaci pro Windows, která bude shromažďovat základní data o využití aplikace a přijímat nabízená oznámení pomocí služby oznamování systému Windows (WNS).</span><span class="sxs-lookup"><span data-stu-id="432d7-106">You create a blank Windows Universal App that collects basic app usage data and receives push notifications using Windows Notification Service (WNS).</span></span>

> [!NOTE]
> <span data-ttu-id="432d7-107">Služba Azure Mobile Engagement Hello vyřadí března 2018 a je aktuálně pouze k dispozici tooexisting zákazníků.</span><span class="sxs-lookup"><span data-stu-id="432d7-107">hello Azure Mobile Engagement service will be retired March 2018 and is currently only available tooexisting customers.</span></span> <span data-ttu-id="432d7-108">Další informace najdete v tématu [Mobile Engagement](https://azure.microsoft.com/en-us/services/mobile-engagement/).</span><span class="sxs-lookup"><span data-stu-id="432d7-108">For more information, see [Mobile Engagement](https://azure.microsoft.com/en-us/services/mobile-engagement/).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="432d7-109">Požadavky</span><span class="sxs-lookup"><span data-stu-id="432d7-109">Prerequisites</span></span>
[!INCLUDE [Prereqs](../../includes/mobile-engagement-windows-store-prereqs.md)]

## <a name="set-up-mobile-engagement-for-your-windows-universal-app"></a><span data-ttu-id="432d7-110">Nastavení Mobile Engagementu pro univerzální aplikaci pro Windows</span><span class="sxs-lookup"><span data-stu-id="432d7-110">Set up Mobile Engagement for your Windows Universal app</span></span>
[!INCLUDE [Create Mobile Engagement App in Portal](../../includes/mobile-engagement-create-app-in-portal-new.md)]

## <span data-ttu-id="432d7-111"><a id="connecting-app"></a>Připojit vaše aplikace toohello Mobile Engagement back-end</span><span class="sxs-lookup"><span data-stu-id="432d7-111"><a id="connecting-app"></a>Connect your app toohello Mobile Engagement backend</span></span>
<span data-ttu-id="432d7-112">V tomto kurzu uvede "základní integraci", což je minimální hello nastavte požadované toocollect dat a odesílání nabízených oznámení.</span><span class="sxs-lookup"><span data-stu-id="432d7-112">This tutorial presents a "basic integration," which is hello minimal set required toocollect data and send a push notification.</span></span> <span data-ttu-id="432d7-113">Hello kompletní dokumentaci k integraci najdete v hello [integrace sady Mobile Engagement Windows Universal SDK](mobile-engagement-windows-store-sdk-overview.md).</span><span class="sxs-lookup"><span data-stu-id="432d7-113">hello complete integration documentation can be found in hello [Mobile Engagement Windows Universal SDK integration](mobile-engagement-windows-store-sdk-overview.md).</span></span>

<span data-ttu-id="432d7-114">Vytvoříte základní aplikaci s integrace hello toodemonstrate sady Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="432d7-114">You create a basic app with Visual Studio toodemonstrate hello integration.</span></span>

### <a name="create-a-windows-universal-app-project"></a><span data-ttu-id="432d7-115">Vytvoření projektu univerzální aplikace pro Windows</span><span class="sxs-lookup"><span data-stu-id="432d7-115">Create a Windows Universal App project</span></span>
<span data-ttu-id="432d7-116">Hello následující postup předpokládá použití hello sady Visual Studio 2015 když hello postup je podobný jako ve starších verzích sady Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="432d7-116">hello following steps assume hello use of Visual Studio 2015 though hello steps are similar in earlier versions of Visual Studio.</span></span>

1. <span data-ttu-id="432d7-117">Spuštění sady Visual Studio a v hello **Domů** obrazovku, vyberte **nový projekt**.</span><span class="sxs-lookup"><span data-stu-id="432d7-117">Start Visual Studio, and in hello **Home** screen, select **New Project**.</span></span>
2. <span data-ttu-id="432d7-118">V místní nabídce hello vyberte **Windows** -> **Universal** -> **prázdná aplikace (univerzální pro Windows)**.</span><span class="sxs-lookup"><span data-stu-id="432d7-118">In hello pop-up, select **Windows** -> **Universal** -> **Blank App (Universal Windows)**.</span></span> <span data-ttu-id="432d7-119">Vyplňte aplikace hello **název** a **název řešení**a potom klikněte na **OK**.</span><span class="sxs-lookup"><span data-stu-id="432d7-119">Fill in hello app **Name** and **Solution name**, and then click **OK**.</span></span>

    ![][1]

<span data-ttu-id="432d7-120">Nyní jste vytvořili projekt univerzální aplikace pro Windows, do které vedle integrovat hello Azure Mobile Engagement SDK.</span><span class="sxs-lookup"><span data-stu-id="432d7-120">You have now created a Windows Universal App project into which you next integrate hello Azure Mobile Engagement SDK.</span></span>

### <a name="connect-your-app-toomobile-engagement-backend"></a><span data-ttu-id="432d7-121">Připojit vaše tooMobile Engagement back-end aplikace</span><span class="sxs-lookup"><span data-stu-id="432d7-121">Connect your app tooMobile Engagement backend</span></span>
1. <span data-ttu-id="432d7-122">Nainstalujte hello [MicrosoftAzure.MobileEngagement] balíček Nuget do projektu.</span><span class="sxs-lookup"><span data-stu-id="432d7-122">Install hello [MicrosoftAzure.MobileEngagement] Nuget package in your project.</span></span> <span data-ttu-id="432d7-123">Pokud cílíte na platformy Windows i Windows Phone, musíte toodo to pro oba projekty.</span><span class="sxs-lookup"><span data-stu-id="432d7-123">If you are targeting both Windows and Windows Phone platforms, you need toodo this for both projects.</span></span> <span data-ttu-id="432d7-124">Pro Windows 8.x a Windows Phone 8.1, hello stejné Nuget balíček místech hello správné specifické pro platformu binárních souborů do každého projektu.</span><span class="sxs-lookup"><span data-stu-id="432d7-124">For Windows 8.x and Windows Phone 8.1, hello same Nuget package places hello correct platform-specific binaries in each project.</span></span>
2. <span data-ttu-id="432d7-125">Otevřete **Package.appxmanifest** a zajistěte, aby je přidána této hello následující funkce:</span><span class="sxs-lookup"><span data-stu-id="432d7-125">Open **Package.appxmanifest** and make sure that hello following capability is added there:</span></span>

        Internet (Client)

    ![][2]
3. <span data-ttu-id="432d7-126">Nyní hello připojovací řetězec, který jste zkopírovali dříve pro aplikaci Mobile Engagementu zkopírujte a vložte ji do hello `Resources\EngagementConfiguration.xml` souboru mezi hello `<connectionString>` a `</connectionString>` značky:</span><span class="sxs-lookup"><span data-stu-id="432d7-126">Now copy hello connection string that you copied earlier for your Mobile Engagement App and paste it in hello `Resources\EngagementConfiguration.xml` file, between hello `<connectionString>` and `</connectionString>` tags:</span></span>

    ![][3]

    > [!TIP]
    > <span data-ttu-id="432d7-127">Pokud vaše aplikace cílí na platformy Windows i Windows Phone, měli byste stále vytvořit dvě aplikace Mobile Engagementu – jednu pro každou podporovanou platformu.</span><span class="sxs-lookup"><span data-stu-id="432d7-127">If your App targets both Windows and Windows Phone platforms, you should still create two Mobile Engagement Applications - one for each supported platform.</span></span> <span data-ttu-id="432d7-128">Má dvě aplikace zajišťuje, že jste správně segmentovat cílovou skupinu hello můžete vytvořit a můžete posílat vhodně cílená oznámení pro každou platformu.</span><span class="sxs-lookup"><span data-stu-id="432d7-128">Having two apps ensures that you can create correct segmentation of hello audience and can send appropriately targeted notifications for each platform.</span></span>

    > [!IMPORTANT]
    > <span data-ttu-id="432d7-129">NuGet automaticky nekopíruje hello prostředků sady SDK v aplikaci Windows 10 UWP.</span><span class="sxs-lookup"><span data-stu-id="432d7-129">NuGet does not automatically copy hello SDK resources in your Windows 10 UWP application.</span></span> <span data-ttu-id="432d7-130">Jej budete mít toodo ručně následující hello kroky, které ukazují nahoru (readme.txt) při instalaci balíčku Nuget hello.</span><span class="sxs-lookup"><span data-stu-id="432d7-130">You have toodo it manually following hello steps which show up (readme.txt) when hello Nuget package is installed.</span></span>  

1. <span data-ttu-id="432d7-131">V hello `App.xaml.cs` souboru:</span><span class="sxs-lookup"><span data-stu-id="432d7-131">In hello `App.xaml.cs` file:</span></span>

    <span data-ttu-id="432d7-132">a.</span><span class="sxs-lookup"><span data-stu-id="432d7-132">a.</span></span> <span data-ttu-id="432d7-133">Přidat hello `using` příkaz:</span><span class="sxs-lookup"><span data-stu-id="432d7-133">Add hello `using` statement:</span></span>

            using Microsoft.Azure.Engagement;

    <span data-ttu-id="432d7-134">b.</span><span class="sxs-lookup"><span data-stu-id="432d7-134">b.</span></span> <span data-ttu-id="432d7-135">Přidejte metodu, která inicializuje hello Engagement:</span><span class="sxs-lookup"><span data-stu-id="432d7-135">Add a method that initializes hello Engagement:</span></span>

           private void InitEngagement(IActivatedEventArgs e)
           {
             EngagementAgent.Instance.Init(e);

             //... rest of hello code
           }

    <span data-ttu-id="432d7-136">c.</span><span class="sxs-lookup"><span data-stu-id="432d7-136">c.</span></span> <span data-ttu-id="432d7-137">Inicializace hello SDK v hello **OnLaunched** metoda:</span><span class="sxs-lookup"><span data-stu-id="432d7-137">Initialize hello SDK in hello **OnLaunched** method:</span></span>

            protected override void OnLaunched(LaunchActivatedEventArgs e)
            {
              InitEngagement(e);

              //... rest of hello code
            }

    <span data-ttu-id="432d7-138">c.</span><span class="sxs-lookup"><span data-stu-id="432d7-138">c.</span></span> <span data-ttu-id="432d7-139">Vložte následující hello hello **OnActivated** metoda a přidejte metodu hello, pokud již není přítomna:</span><span class="sxs-lookup"><span data-stu-id="432d7-139">Insert hello following in hello **OnActivated** method and add hello method if it is not already present:</span></span>

            protected override void OnActivated(IActivatedEventArgs e)
            {
              InitEngagement(e);

              //... rest of hello code
            }

## <span data-ttu-id="432d7-140"><a id="monitor"></a>Povolení sledování v reálném čase</span><span class="sxs-lookup"><span data-stu-id="432d7-140"><a id="monitor"></a>Enable real-time monitoring</span></span>
<span data-ttu-id="432d7-141">toostart odesílat data a zajistit, že hello uživatelé jsou aktivní, musíte odeslat alespoň jednu obrazovku (aktivitu) toohello Mobile Engagement back-end.</span><span class="sxs-lookup"><span data-stu-id="432d7-141">toostart sending data and ensuring that hello users are active, you must send at least one screen (Activity) toohello Mobile Engagement backend.</span></span>

1. <span data-ttu-id="432d7-142">V hello **MainPage.xaml.cs**, přidejte následující hello `using` příkaz:</span><span class="sxs-lookup"><span data-stu-id="432d7-142">In hello **MainPage.xaml.cs**, add hello following `using` statement:</span></span>

    <span data-ttu-id="432d7-143">použití příkazu Microsoft.Azure.Engagement.Overlay;</span><span class="sxs-lookup"><span data-stu-id="432d7-143">using Microsoft.Azure.Engagement.Overlay;</span></span>
2. <span data-ttu-id="432d7-144">Změňte základní třídu hello **MainPage** z **stránky** příliš**EngagementPageOverlay**:</span><span class="sxs-lookup"><span data-stu-id="432d7-144">Change hello base class of **MainPage** from **Page** too**EngagementPageOverlay**:</span></span>

        class MainPage : EngagementPageOverlay
3. <span data-ttu-id="432d7-145">V hello `MainPage.xaml` souboru:</span><span class="sxs-lookup"><span data-stu-id="432d7-145">In hello `MainPage.xaml` file:</span></span>

    <span data-ttu-id="432d7-146">a.</span><span class="sxs-lookup"><span data-stu-id="432d7-146">a.</span></span> <span data-ttu-id="432d7-147">Přidejte tooyour deklarací oborů názvů:</span><span class="sxs-lookup"><span data-stu-id="432d7-147">Add tooyour namespaces declarations:</span></span>

        xmlns:engagement="using:Microsoft.Azure.Engagement.Overlay"

    <span data-ttu-id="432d7-148">b.</span><span class="sxs-lookup"><span data-stu-id="432d7-148">b.</span></span> <span data-ttu-id="432d7-149">Nahraďte hello **stránky** v názvu značky XML hello s **engagement: EngagementPageOverlay**</span><span class="sxs-lookup"><span data-stu-id="432d7-149">Replace hello **Page** in hello XML tag name with **engagement:EngagementPageOverlay**</span></span>

> [!IMPORTANT]
> <span data-ttu-id="432d7-150">Pokud stránka přepíše hello `OnNavigatedTo` metoda, že toocall být `base.OnNavigatedTo(e)`.</span><span class="sxs-lookup"><span data-stu-id="432d7-150">If your page overrides hello `OnNavigatedTo` method, be sure toocall `base.OnNavigatedTo(e)`.</span></span> <span data-ttu-id="432d7-151">Jinak, není hlášena hello aktivity `EngagementPage` volání `StartActivity` uvnitř jeho `OnNavigatedTo` metoda).</span><span class="sxs-lookup"><span data-stu-id="432d7-151">Otherwise, hello activity is not reported `EngagementPage` calls `StartActivity` inside its `OnNavigatedTo` method).</span></span> <span data-ttu-id="432d7-152">To je obzvláště důležité v projektu Windows Phone, kde má výchozí šablona hello `OnNavigatedTo` metoda.</span><span class="sxs-lookup"><span data-stu-id="432d7-152">This is especially important in a Windows Phone project where hello default template has an `OnNavigatedTo` method.</span></span>
>
> <span data-ttu-id="432d7-153">Pro **10 univerzálních aplikací pro Windows**, použijte metodu hello doporučení v hello "doporučená metoda: přetížení třídy stránky" části [pokročilé vytváření sestav s hello Windows Universal SDK aplikace Engagement](mobile-engagement-windows-store-advanced-reporting.md) , místo hello jeden uvedených výše.</span><span class="sxs-lookup"><span data-stu-id="432d7-153">For **Windows 10 Universal apps**, use hello method recommended in hello "Recommended method: overload your Page classes" section of [Advanced Reporting with hello Windows Universal Apps Engagement SDK](mobile-engagement-windows-store-advanced-reporting.md), rather than hello one mentioned above.</span></span>

## <span data-ttu-id="432d7-154"><a id="monitor"></a>Připojení aplikace se sledováním v reálném čase</span><span class="sxs-lookup"><span data-stu-id="432d7-154"><a id="monitor"></a>Connect app with real-time monitoring</span></span>
[!INCLUDE [Connect app with real-time monitoring](../../includes/mobile-engagement-connect-app-with-monitor.md)]

## <span data-ttu-id="432d7-155"><a id="integrate-push"></a>Povolení nabízených oznámení a zasílání zpráv v aplikaci</span><span class="sxs-lookup"><span data-stu-id="432d7-155"><a id="integrate-push"></a>Enable push notifications and in-app messaging</span></span>
<span data-ttu-id="432d7-156">Mobile Engagement vám umožní toointeract oslovit uživatele a komunikovat s nabízená oznámení a zasílání zpráv v kontextu hello kampaní v aplikaci.</span><span class="sxs-lookup"><span data-stu-id="432d7-156">Mobile Engagement allows you toointeract and reach your users with push notifications and in-app messaging in hello context of campaigns.</span></span> <span data-ttu-id="432d7-157">Tento modul je hello portálu Mobile Engagement nazývá REACH.</span><span class="sxs-lookup"><span data-stu-id="432d7-157">This module is called REACH in hello Mobile Engagement portal.</span></span>
<span data-ttu-id="432d7-158">Hello následujících sekcích nastavíte tooreceive vaše aplikace je.</span><span class="sxs-lookup"><span data-stu-id="432d7-158">hello following sections set up your app tooreceive them.</span></span>

### <a name="enable-your-app-tooreceive-wns-push-notifications"></a><span data-ttu-id="432d7-159">Povolit tooreceive nabízených oznámení WNS vaší aplikace</span><span class="sxs-lookup"><span data-stu-id="432d7-159">Enable your app tooreceive WNS Push Notifications</span></span>
1. <span data-ttu-id="432d7-160">V hello `Package.appxmanifest` souboru ve hello **aplikace** v části **oznámení**, nastavte **podporující oznamovací zprávy:** příliš**Ano**</span><span class="sxs-lookup"><span data-stu-id="432d7-160">In hello `Package.appxmanifest` file, in hello **Application** tab, under **Notifications**, set **Toast capable:** too**Yes**</span></span>

    ![][5]

### <a name="initialize-hello-reach-sdk"></a><span data-ttu-id="432d7-161">Inicializace hello REACH SDK</span><span class="sxs-lookup"><span data-stu-id="432d7-161">Initialize hello REACH SDK</span></span>
<span data-ttu-id="432d7-162">V `App.xaml.cs`, volání **EngagementReach.Instance.Init(e);** v hello **InitEngagement** funkce hned po inicializaci agenta hello:</span><span class="sxs-lookup"><span data-stu-id="432d7-162">In `App.xaml.cs`, call **EngagementReach.Instance.Init(e);** in hello **InitEngagement** function right after hello agent initialization:</span></span>

        private void InitEngagement(IActivatedEventArgs e)
        {
           EngagementAgent.Instance.Init(e);
           EngagementReach.Instance.Init(e);
        }

<span data-ttu-id="432d7-163">Jste připravené toosend oznámení.</span><span class="sxs-lookup"><span data-stu-id="432d7-163">You're ready toosend a toast.</span></span> <span data-ttu-id="432d7-164">Dál ověříme, jestli jste základní integraci provedli správně.</span><span class="sxs-lookup"><span data-stu-id="432d7-164">Next we verify that you have correctly carried out this basic integration.</span></span>

### <a name="grant-access-toomobile-engagement-toosend-notifications"></a><span data-ttu-id="432d7-165">Udělení přístupu tooMobile Engagement toosend oznámení</span><span class="sxs-lookup"><span data-stu-id="432d7-165">Grant access tooMobile Engagement toosend notifications</span></span>
1. <span data-ttu-id="432d7-166">Otevřete [Centrum vývojářů pro Windows Store] ve webovém prohlížeči, přihlaste se a v případě potřeby si vytvořte účet.</span><span class="sxs-lookup"><span data-stu-id="432d7-166">Open [Windows Store Dev Center] in your web browser, login, and create an account if necessary.</span></span>
2. <span data-ttu-id="432d7-167">Klikněte na tlačítko **řídicí panel** v hello pravém horním rohu a pak klikněte na **vytvořit novou aplikaci** z nabídky na levém panelu hello.</span><span class="sxs-lookup"><span data-stu-id="432d7-167">Click **Dashboard** at hello top right corner and then click **Create a new app** from hello left panel menu.</span></span>

    ![][9]
3. <span data-ttu-id="432d7-168">Vytvoření aplikace rezervací názvu.</span><span class="sxs-lookup"><span data-stu-id="432d7-168">Create your app by reserving its name.</span></span>

    ![][10]
4. <span data-ttu-id="432d7-169">Po vytvoření aplikace hello přejděte příliš**služby -> nabízená oznámení** hello levé nabídce.</span><span class="sxs-lookup"><span data-stu-id="432d7-169">Once hello app has been created, navigate too**Services -> Push notifications** from hello left menu.</span></span>

    ![][11]
5. <span data-ttu-id="432d7-170">V hello nabízená oznámení oddíl, klikněte na tlačítko hello **Web služeb Live Services** odkaz.</span><span class="sxs-lookup"><span data-stu-id="432d7-170">In hello Push notifications section, click hello **Live Services site** link.</span></span>

    ![][12]
6. <span data-ttu-id="432d7-171">Přejdete toohello nabízené přihlašovací údaje části.</span><span class="sxs-lookup"><span data-stu-id="432d7-171">You navigate toohello Push credentials section.</span></span> <span data-ttu-id="432d7-172">Zkontrolujte, zda jsou v hello **nastavení aplikace** a poté zkopírujte vaše **SID balíčku** a **tajný klíč klienta**</span><span class="sxs-lookup"><span data-stu-id="432d7-172">Make sure you are in hello **App Settings** section and then copy your **Package SID** and **Client secret**</span></span>

    ![][13]
7. <span data-ttu-id="432d7-173">Přejděte toohello **nastavení** z portálu Mobile Engagement a klikněte na tlačítko hello **nativní oznámení** části na levé straně hello.</span><span class="sxs-lookup"><span data-stu-id="432d7-173">Navigate toohello **Settings** of your Mobile Engagement portal, and click hello **Native Push** section on hello left.</span></span> <span data-ttu-id="432d7-174">Potom klikněte na hello **upravit** tooenter tlačítko vaše **identifikátor zabezpečení (SID) balíčku** a **tajný klíč** znázorněné:</span><span class="sxs-lookup"><span data-stu-id="432d7-174">Then, click hello **Edit** button tooenter your **Package security identifier (SID)** and your **Secret Key** as shown:</span></span>

    ![][6]
8. <span data-ttu-id="432d7-175">Nakonec nezapomeňte přiřadit aplikaci Visual Studio k této vytvořené aplikaci v obchodě s aplikacemi hello.</span><span class="sxs-lookup"><span data-stu-id="432d7-175">Finally make sure that you have associated your Visual Studio app with this created app in hello App store.</span></span> <span data-ttu-id="432d7-176">V sadě Visual Studio klikněte na **Propojit aplikaci se Storem**.</span><span class="sxs-lookup"><span data-stu-id="432d7-176">Click **Associate App with Store** in Visual Studio.</span></span>

    ![][7]

## <span data-ttu-id="432d7-177"><a id="send"></a>Poslat tooyour aplikaci oznámení</span><span class="sxs-lookup"><span data-stu-id="432d7-177"><a id="send"></a>Send a notification tooyour app</span></span>
[!INCLUDE [Create Windows Push campaign](../../includes/mobile-engagement-windows-push-campaign.md)]

<span data-ttu-id="432d7-178">Pokud hello aplikace běží, zobrazí se oznámení v aplikaci.</span><span class="sxs-lookup"><span data-stu-id="432d7-178">If hello app is running, you see an in-app notification.</span></span> <span data-ttu-id="432d7-179">Pokud aplikace hello je zavřená, v opačném případě zobrazí oznámení s informační zprávou.</span><span class="sxs-lookup"><span data-stu-id="432d7-179">otherwise if hello app is closed, you see a toast notification.</span></span>
<span data-ttu-id="432d7-180">Pokud se zobrazí oznámení v aplikaci, ale ne oznámení s informační zprávou a hello aplikace běží v režimu ladění v sadě Visual Studio, opakujte **události životního cyklu -> pozastavit** v panelu nástrojů tooensure hello aplikaci hello pozastaveno.</span><span class="sxs-lookup"><span data-stu-id="432d7-180">If you see an in-app notification but not a toast notification, and you are running hello app in debug mode in Visual Studio, then try **Lifecycle events -> Suspend** in hello toolbar tooensure that hello app is suspended.</span></span> <span data-ttu-id="432d7-181">Pokud jste klikli hello tlačítko Domů při ladění aplikace hello v sadě Visual Studio, poté ji není vždy dojít k jejímu pozastavení a při hello oznámení se zobrazí v aplikaci, nezobrazí jako oznámení s informační zprávou.</span><span class="sxs-lookup"><span data-stu-id="432d7-181">If you clicked hello Home button while debugging hello application in Visual Studio, then it doesn't always get suspended and while you see hello notification as in-app, it doesn't show up as a toast notification.</span></span>  

![][8]

<!-- URLs. -->
[Mobile Engagement Windows Universal SDK documentation]: ../mobile-engagement-windows-store-integrate-engagement/
[MicrosoftAzure.MobileEngagement]: http://go.microsoft.com/?linkid=9864592
[Centrum vývojářů pro Windows Store]: https://dev.windows.com
[Windows Universal Apps - Overlay integration]: ../mobile-engagement-windows-store-integrate-engagement-reach/#overlay-integration

<!-- Images. -->
[1]: ./media/mobile-engagement-windows-store-dotnet-get-started/universal-app-creation.png
[2]: ./media/mobile-engagement-windows-store-dotnet-get-started/manifest-capabilities.png
[3]: ./media/mobile-engagement-windows-store-dotnet-get-started/add-connection-info.png
[5]: ./media/mobile-engagement-windows-store-dotnet-get-started/manifest-toast.png
[6]: ./media/mobile-engagement-windows-store-dotnet-get-started/enter-credentials.png
[7]: ./media/mobile-engagement-windows-store-dotnet-get-started/associate-app-store.png
[8]: ./media/mobile-engagement-windows-store-dotnet-get-started/vs-suspend.png
[9]: ./media/mobile-engagement-windows-store-dotnet-get-started/dashboard_create_app.png
[10]: ./media/mobile-engagement-windows-store-dotnet-get-started/dashboard_app_name.png
[11]: ./media/mobile-engagement-windows-store-dotnet-get-started/dashboard_services_push.png
[12]: ./media/mobile-engagement-windows-store-dotnet-get-started/dashboard_services_push_1.png
[13]: ./media/mobile-engagement-windows-store-dotnet-get-started/dashboard_services_push_creds.png
