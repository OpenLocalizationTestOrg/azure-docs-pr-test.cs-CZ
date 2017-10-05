---
title: "Začínáme s Azure Mobile Engagementem pro univerzální aplikace pro Windows"
description: "Naučte se používat Azure Mobile Engagement s analytickými funkcemi a nabízenými oznámeními pro univerzální aplikace pro Windows."
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
ms.openlocfilehash: 40db7e4dd151ec391c754dc6d4145aeeb8058eca
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="get-started-with-azure-mobile-engagement-for-windows-universal-apps"></a><span data-ttu-id="78256-103">Začínáme s Azure Mobile Engagementem pro univerzální aplikace pro Windows</span><span class="sxs-lookup"><span data-stu-id="78256-103">Get started with Azure Mobile Engagement for Windows Universal Apps</span></span>
[!INCLUDE [Hero tutorial switcher](../../includes/mobile-engagement-hero-tutorial-switcher.md)]

<span data-ttu-id="78256-104">V tomto tématu si ukážeme, jak používat Azure Mobile Engagement, jak porozumět používání aplikace a jak odesílat nabízená oznámení segmentovaným uživatelům univerzální aplikace pro Windows.</span><span class="sxs-lookup"><span data-stu-id="78256-104">This topic shows you how to use Azure Mobile Engagement to understand your app usage and send push notifications to segmented users of a Windows Universal application.</span></span>
<span data-ttu-id="78256-105">Tento kurz představuje scénář jednoduchého vysílání přes Mobile Engagement.</span><span class="sxs-lookup"><span data-stu-id="78256-105">This tutorial demonstrates the simple broadcast scenario using Mobile Engagement.</span></span> <span data-ttu-id="78256-106">Vytvoříte prázdnou univerzální aplikaci pro Windows, která bude shromažďovat základní data o využití aplikace a přijímat nabízená oznámení pomocí služby oznamování systému Windows (WNS).</span><span class="sxs-lookup"><span data-stu-id="78256-106">You create a blank Windows Universal App that collects basic app usage data and receives push notifications using Windows Notification Service (WNS).</span></span>

> [!NOTE]
> <span data-ttu-id="78256-107">Službu Azure Mobile Engagement vyřadíme z provozu v březnu 2018. V současnosti je dostupná jenom pro stávající zákazníky.</span><span class="sxs-lookup"><span data-stu-id="78256-107">The Azure Mobile Engagement service will be retired March 2018 and is currently only available to existing customers.</span></span> <span data-ttu-id="78256-108">Další informace najdete v tématu [Mobile Engagement](https://azure.microsoft.com/en-us/services/mobile-engagement/).</span><span class="sxs-lookup"><span data-stu-id="78256-108">For more information, see [Mobile Engagement](https://azure.microsoft.com/en-us/services/mobile-engagement/).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="78256-109">Požadavky</span><span class="sxs-lookup"><span data-stu-id="78256-109">Prerequisites</span></span>
[!INCLUDE [Prereqs](../../includes/mobile-engagement-windows-store-prereqs.md)]

## <a name="set-up-mobile-engagement-for-your-windows-universal-app"></a><span data-ttu-id="78256-110">Nastavení Mobile Engagementu pro univerzální aplikaci pro Windows</span><span class="sxs-lookup"><span data-stu-id="78256-110">Set up Mobile Engagement for your Windows Universal app</span></span>
[!INCLUDE [Create Mobile Engagement App in Portal](../../includes/mobile-engagement-create-app-in-portal-new.md)]

## <span data-ttu-id="78256-111"><a id="connecting-app"></a>Připojení aplikace k back-endu Mobile Engagementu</span><span class="sxs-lookup"><span data-stu-id="78256-111"><a id="connecting-app"></a>Connect your app to the Mobile Engagement backend</span></span>
<span data-ttu-id="78256-112">V tomto kurzu si představíme „základní integraci“, čili minimální sadu, která je zapotřebí pro shromažďování dat a odesílání nabízených oznámení.</span><span class="sxs-lookup"><span data-stu-id="78256-112">This tutorial presents a "basic integration," which is the minimal set required to collect data and send a push notification.</span></span> <span data-ttu-id="78256-113">Kompletní dokumentaci k integraci najdete v článku [Integrace sady Mobile Engagement Windows Universal SDK](mobile-engagement-windows-store-sdk-overview.md)</span><span class="sxs-lookup"><span data-stu-id="78256-113">The complete integration documentation can be found in the [Mobile Engagement Windows Universal SDK integration](mobile-engagement-windows-store-sdk-overview.md).</span></span>

<span data-ttu-id="78256-114">Pomocí sady Visual Studio vytvoříte základní aplikaci, na které si tuto integraci předvedeme.</span><span class="sxs-lookup"><span data-stu-id="78256-114">You create a basic app with Visual Studio to demonstrate the integration.</span></span>

### <a name="create-a-windows-universal-app-project"></a><span data-ttu-id="78256-115">Vytvoření projektu univerzální aplikace pro Windows</span><span class="sxs-lookup"><span data-stu-id="78256-115">Create a Windows Universal App project</span></span>
<span data-ttu-id="78256-116">Následující postup předpokládá použití sady Visual Studio 2015, ačkoliv kroky se podobají předchozím verzím sady Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="78256-116">The following steps assume the use of Visual Studio 2015 though the steps are similar in earlier versions of Visual Studio.</span></span>

1. <span data-ttu-id="78256-117">Spusťte Visual Studio a na obrazovce **Domů** vyberte **Nový projekt**.</span><span class="sxs-lookup"><span data-stu-id="78256-117">Start Visual Studio, and in the **Home** screen, select **New Project**.</span></span>
2. <span data-ttu-id="78256-118">V místní nabídce vyberte **Windows** -> **Universal** -> **Prázdná aplikace (Universal Windows)**.</span><span class="sxs-lookup"><span data-stu-id="78256-118">In the pop-up, select **Windows** -> **Universal** -> **Blank App (Universal Windows)**.</span></span> <span data-ttu-id="78256-119">Vyplňte **Název** aplikace a **Název řešení** a potom klikněte na **OK**.</span><span class="sxs-lookup"><span data-stu-id="78256-119">Fill in the app **Name** and **Solution name**, and then click **OK**.</span></span>

    ![][1]

<span data-ttu-id="78256-120">Nyní jste vytvořili projekt univerzální aplikace pro Windows, do kterého budete dál integrovat sadu Azure Mobile Engagement SDK.</span><span class="sxs-lookup"><span data-stu-id="78256-120">You have now created a Windows Universal App project into which you next integrate the Azure Mobile Engagement SDK.</span></span>

### <a name="connect-your-app-to-mobile-engagement-backend"></a><span data-ttu-id="78256-121">Připojení aplikace k back-endu Mobile Engagementu</span><span class="sxs-lookup"><span data-stu-id="78256-121">Connect your app to Mobile Engagement backend</span></span>
1. <span data-ttu-id="78256-122">Nainstalujte balíček NuGet [MicrosoftAzure.MobileEngagement] do projektu.</span><span class="sxs-lookup"><span data-stu-id="78256-122">Install the [MicrosoftAzure.MobileEngagement] Nuget package in your project.</span></span> <span data-ttu-id="78256-123">Pokud cílíte na platformy Windows i Windows Phone, musíte to provést u obou projektů.</span><span class="sxs-lookup"><span data-stu-id="78256-123">If you are targeting both Windows and Windows Phone platforms, you need to do this for both projects.</span></span> <span data-ttu-id="78256-124">U systému Windows 8.x a Windows Phone 8.1 umístí jeden balíček NuGet do každého projektu správné binární soubory pro konkrétní platformu.</span><span class="sxs-lookup"><span data-stu-id="78256-124">For Windows 8.x and Windows Phone 8.1, the same Nuget package places the correct platform-specific binaries in each project.</span></span>
2. <span data-ttu-id="78256-125">Otevřete **Package.appxmanifest** a ujistěte se, zda je přidána následující možnost:</span><span class="sxs-lookup"><span data-stu-id="78256-125">Open **Package.appxmanifest** and make sure that the following capability is added there:</span></span>

        Internet (Client)

    ![][2]
3. <span data-ttu-id="78256-126">Nyní zkopírujte připojovací řetězec, který jste zkopírovali dříve pro aplikaci Mobile Engagementu, a vložte jej do souboru `Resources\EngagementConfiguration.xml` mezi značky `<connectionString>` a `</connectionString>`:</span><span class="sxs-lookup"><span data-stu-id="78256-126">Now copy the connection string that you copied earlier for your Mobile Engagement App and paste it in the `Resources\EngagementConfiguration.xml` file, between the `<connectionString>` and `</connectionString>` tags:</span></span>

    ![][3]

    > [!TIP]
    > <span data-ttu-id="78256-127">Pokud vaše aplikace cílí na platformy Windows i Windows Phone, měli byste stále vytvořit dvě aplikace Mobile Engagementu – jednu pro každou podporovanou platformu.</span><span class="sxs-lookup"><span data-stu-id="78256-127">If your App targets both Windows and Windows Phone platforms, you should still create two Mobile Engagement Applications - one for each supported platform.</span></span> <span data-ttu-id="78256-128">Dvě aplikace zajišťují, že můžete správně segmentovat publika a posílat vhodně cílená oznámení na každou platformu.</span><span class="sxs-lookup"><span data-stu-id="78256-128">Having two apps ensures that you can create correct segmentation of the audience and can send appropriately targeted notifications for each platform.</span></span>

    > [!IMPORTANT]
    > <span data-ttu-id="78256-129">NuGet automaticky nekopíruje prostředky sady SDK v aplikaci Windows 10 UWP.</span><span class="sxs-lookup"><span data-stu-id="78256-129">NuGet does not automatically copy the SDK resources in your Windows 10 UWP application.</span></span> <span data-ttu-id="78256-130">Je potřeba to provést ručně podle pokynů, které se zobrazí (readme.txt) při instalaci balíčku Nuget.</span><span class="sxs-lookup"><span data-stu-id="78256-130">You have to do it manually following the steps which show up (readme.txt) when the Nuget package is installed.</span></span>  

1. <span data-ttu-id="78256-131">V souboru `App.xaml.cs`:</span><span class="sxs-lookup"><span data-stu-id="78256-131">In the `App.xaml.cs` file:</span></span>

    <span data-ttu-id="78256-132">a.</span><span class="sxs-lookup"><span data-stu-id="78256-132">a.</span></span> <span data-ttu-id="78256-133">Přidejte příkaz `using`:</span><span class="sxs-lookup"><span data-stu-id="78256-133">Add the `using` statement:</span></span>

            using Microsoft.Azure.Engagement;

    <span data-ttu-id="78256-134">b.</span><span class="sxs-lookup"><span data-stu-id="78256-134">b.</span></span> <span data-ttu-id="78256-135">Přidejte metodu, která inicializuje Engagement:</span><span class="sxs-lookup"><span data-stu-id="78256-135">Add a method that initializes the Engagement:</span></span>

           private void InitEngagement(IActivatedEventArgs e)
           {
             EngagementAgent.Instance.Init(e);

             //... rest of the code
           }

    <span data-ttu-id="78256-136">c.</span><span class="sxs-lookup"><span data-stu-id="78256-136">c.</span></span> <span data-ttu-id="78256-137">Inicializujte sadu SDK v metodě **OnLaunched**:</span><span class="sxs-lookup"><span data-stu-id="78256-137">Initialize the SDK in the **OnLaunched** method:</span></span>

            protected override void OnLaunched(LaunchActivatedEventArgs e)
            {
              InitEngagement(e);

              //... rest of the code
            }

    <span data-ttu-id="78256-138">c.</span><span class="sxs-lookup"><span data-stu-id="78256-138">c.</span></span> <span data-ttu-id="78256-139">Do metody **OnActivated** vložte následující a metodu přidejte (pokud zatím není přítomná):</span><span class="sxs-lookup"><span data-stu-id="78256-139">Insert the following in the **OnActivated** method and add the method if it is not already present:</span></span>

            protected override void OnActivated(IActivatedEventArgs e)
            {
              InitEngagement(e);

              //... rest of the code
            }

## <span data-ttu-id="78256-140"><a id="monitor"></a>Povolení sledování v reálném čase</span><span class="sxs-lookup"><span data-stu-id="78256-140"><a id="monitor"></a>Enable real-time monitoring</span></span>
<span data-ttu-id="78256-141">Pokud chcete začít odesílat data a zajistit, že uživatelé jsou aktivní, musíte odeslat alespoň jednu obrazovku (aktivitu) na back-end Mobile Engagementu.</span><span class="sxs-lookup"><span data-stu-id="78256-141">To start sending data and ensuring that the users are active, you must send at least one screen (Activity) to the Mobile Engagement backend.</span></span>

1. <span data-ttu-id="78256-142">V **MainPage.xaml.cs** přidejte následující příkaz `using`:</span><span class="sxs-lookup"><span data-stu-id="78256-142">In the **MainPage.xaml.cs**, add the following `using` statement:</span></span>

    <span data-ttu-id="78256-143">použití příkazu Microsoft.Azure.Engagement.Overlay;</span><span class="sxs-lookup"><span data-stu-id="78256-143">using Microsoft.Azure.Engagement.Overlay;</span></span>
2. <span data-ttu-id="78256-144">Změňte základní třídu **MainPage** z **Page** na **EngagementPageOverlay**:</span><span class="sxs-lookup"><span data-stu-id="78256-144">Change the base class of **MainPage** from **Page** to **EngagementPageOverlay**:</span></span>

        class MainPage : EngagementPageOverlay
3. <span data-ttu-id="78256-145">V souboru `MainPage.xaml`:</span><span class="sxs-lookup"><span data-stu-id="78256-145">In the `MainPage.xaml` file:</span></span>

    <span data-ttu-id="78256-146">a.</span><span class="sxs-lookup"><span data-stu-id="78256-146">a.</span></span> <span data-ttu-id="78256-147">Přidejte do deklarací oborů názvů:</span><span class="sxs-lookup"><span data-stu-id="78256-147">Add to your namespaces declarations:</span></span>

        xmlns:engagement="using:Microsoft.Azure.Engagement.Overlay"

    <span data-ttu-id="78256-148">b.</span><span class="sxs-lookup"><span data-stu-id="78256-148">b.</span></span> <span data-ttu-id="78256-149">Nahraďte **Page** v názvu značky XML textem **engagement:EngagementPageOverlay**</span><span class="sxs-lookup"><span data-stu-id="78256-149">Replace the **Page** in the XML tag name with **engagement:EngagementPageOverlay**</span></span>

> [!IMPORTANT]
> <span data-ttu-id="78256-150">Pokud stránka přepíše metodu `OnNavigatedTo`, nezapomeňte volat `base.OnNavigatedTo(e)`.</span><span class="sxs-lookup"><span data-stu-id="78256-150">If your page overrides the `OnNavigatedTo` method, be sure to call `base.OnNavigatedTo(e)`.</span></span> <span data-ttu-id="78256-151">Jinak aktivita nebude hlášena (`EngagementPage` volá `StartActivity` uvnitř své metody `OnNavigatedTo`).</span><span class="sxs-lookup"><span data-stu-id="78256-151">Otherwise, the activity is not reported `EngagementPage` calls `StartActivity` inside its `OnNavigatedTo` method).</span></span> <span data-ttu-id="78256-152">To je obzvláště důležité v projektu Windows Phone, kde má výchozí šablona metodu `OnNavigatedTo`.</span><span class="sxs-lookup"><span data-stu-id="78256-152">This is especially important in a Windows Phone project where the default template has an `OnNavigatedTo` method.</span></span>
>
> <span data-ttu-id="78256-153">Pro **univerzální aplikace pro Windows 10** použijte místo metody nahoře doporučenou metodu v části „Doporučení metoda: přetížení tříd Page“ tématu [Rozšířená tvorba sestav pomocí sady Windows Universal Apps Engagement SDK](mobile-engagement-windows-store-advanced-reporting.md).</span><span class="sxs-lookup"><span data-stu-id="78256-153">For **Windows 10 Universal apps**, use the method recommended in the "Recommended method: overload your Page classes" section of [Advanced Reporting with the Windows Universal Apps Engagement SDK](mobile-engagement-windows-store-advanced-reporting.md), rather than the one mentioned above.</span></span>

## <span data-ttu-id="78256-154"><a id="monitor"></a>Připojení aplikace se sledováním v reálném čase</span><span class="sxs-lookup"><span data-stu-id="78256-154"><a id="monitor"></a>Connect app with real-time monitoring</span></span>
[!INCLUDE [Connect app with real-time monitoring](../../includes/mobile-engagement-connect-app-with-monitor.md)]

## <span data-ttu-id="78256-155"><a id="integrate-push"></a>Povolení nabízených oznámení a zasílání zpráv v aplikaci</span><span class="sxs-lookup"><span data-stu-id="78256-155"><a id="integrate-push"></a>Enable push notifications and in-app messaging</span></span>
<span data-ttu-id="78256-156">Mobile Engagement vám umožňuje v rámci kampaní oslovit uživatele a komunikovat s nimi prostřednictvím nabízených oznámení a zpráv v aplikacích.</span><span class="sxs-lookup"><span data-stu-id="78256-156">Mobile Engagement allows you to interact and reach your users with push notifications and in-app messaging in the context of campaigns.</span></span> <span data-ttu-id="78256-157">Tento modul se na portálu Mobile Engagement nazývá REACH.</span><span class="sxs-lookup"><span data-stu-id="78256-157">This module is called REACH in the Mobile Engagement portal.</span></span>
<span data-ttu-id="78256-158">V následujících sekcích nastavíte aplikaci, aby tato nabízená oznámení a zprávy přijímala.</span><span class="sxs-lookup"><span data-stu-id="78256-158">The following sections set up your app to receive them.</span></span>

### <a name="enable-your-app-to-receive-wns-push-notifications"></a><span data-ttu-id="78256-159">Povolení přijímání nabízených oznámení WNS v aplikaci</span><span class="sxs-lookup"><span data-stu-id="78256-159">Enable your app to receive WNS Push Notifications</span></span>
1. <span data-ttu-id="78256-160">V souboru `Package.appxmanifest` na kartě **Aplikace** v části **Oznámení** nastavte **Podporující oznamovací zprávy:** na **Ano**</span><span class="sxs-lookup"><span data-stu-id="78256-160">In the `Package.appxmanifest` file, in the **Application** tab, under **Notifications**, set **Toast capable:** to **Yes**</span></span>

    ![][5]

### <a name="initialize-the-reach-sdk"></a><span data-ttu-id="78256-161">Inicializace sady REACH SDK</span><span class="sxs-lookup"><span data-stu-id="78256-161">Initialize the REACH SDK</span></span>
<span data-ttu-id="78256-162">V `App.xaml.cs` volejte **EngagementReach.Instance.Init(e);** ve funkci **InitEngagement** hned po inicializaci agenta:</span><span class="sxs-lookup"><span data-stu-id="78256-162">In `App.xaml.cs`, call **EngagementReach.Instance.Init(e);** in the **InitEngagement** function right after the agent initialization:</span></span>

        private void InitEngagement(IActivatedEventArgs e)
        {
           EngagementAgent.Instance.Init(e);
           EngagementReach.Instance.Init(e);
        }

<span data-ttu-id="78256-163">Jste připravení odeslat oznámení.</span><span class="sxs-lookup"><span data-stu-id="78256-163">You're ready to send a toast.</span></span> <span data-ttu-id="78256-164">Dál ověříme, jestli jste základní integraci provedli správně.</span><span class="sxs-lookup"><span data-stu-id="78256-164">Next we verify that you have correctly carried out this basic integration.</span></span>

### <a name="grant-access-to-mobile-engagement-to-send-notifications"></a><span data-ttu-id="78256-165">Udělení přístupu k Mobile Engagementu za účelem odesílání oznámení</span><span class="sxs-lookup"><span data-stu-id="78256-165">Grant access to Mobile Engagement to send notifications</span></span>
1. <span data-ttu-id="78256-166">Otevřete [Centrum vývojářů pro Windows Store] ve webovém prohlížeči, přihlaste se a v případě potřeby si vytvořte účet.</span><span class="sxs-lookup"><span data-stu-id="78256-166">Open [Windows Store Dev Center] in your web browser, login, and create an account if necessary.</span></span>
2. <span data-ttu-id="78256-167">Klikněte na **Řídicí panel** v pravém horním rohu a pak klikněte na **Vytvořit novou aplikaci** z nabídky na levém panelu.</span><span class="sxs-lookup"><span data-stu-id="78256-167">Click **Dashboard** at the top right corner and then click **Create a new app** from the left panel menu.</span></span>

    ![][9]
3. <span data-ttu-id="78256-168">Vytvoření aplikace rezervací názvu.</span><span class="sxs-lookup"><span data-stu-id="78256-168">Create your app by reserving its name.</span></span>

    ![][10]
4. <span data-ttu-id="78256-169">Po vytvoření aplikace přejděte do části **Služby -> Nabízená oznámení** v nabídce vlevo.</span><span class="sxs-lookup"><span data-stu-id="78256-169">Once the app has been created, navigate to **Services -> Push notifications** from the left menu.</span></span>

    ![][11]
5. <span data-ttu-id="78256-170">V části Nabízená oznámení klikněte na odkaz **web služeb Live Services**.</span><span class="sxs-lookup"><span data-stu-id="78256-170">In the Push notifications section, click the **Live Services site** link.</span></span>

    ![][12]
6. <span data-ttu-id="78256-171">Budete přesměrováni do části Přihlašovací údaje oznámení.</span><span class="sxs-lookup"><span data-stu-id="78256-171">You navigate to the Push credentials section.</span></span> <span data-ttu-id="78256-172">Zkontrolujte, zda jste v části **Nastavení aplikace**, a poté zkopírujte **SID balíčku** a **Tajný klíč klienta**</span><span class="sxs-lookup"><span data-stu-id="78256-172">Make sure you are in the **App Settings** section and then copy your **Package SID** and **Client secret**</span></span>

    ![][13]
7. <span data-ttu-id="78256-173">Přejděte do **Nastavení** v portálu Mobile Engagement a klikněte na část **Nativní oznámení** na levé straně.</span><span class="sxs-lookup"><span data-stu-id="78256-173">Navigate to the **Settings** of your Mobile Engagement portal, and click the **Native Push** section on the left.</span></span> <span data-ttu-id="78256-174">Klikněte na tlačítko **Upravit** a zadejte **Identifikátor zabezpečení balíčku (SID)** a **Tajný klíč**, jak je zobrazeno:</span><span class="sxs-lookup"><span data-stu-id="78256-174">Then, click the **Edit** button to enter your **Package security identifier (SID)** and your **Secret Key** as shown:</span></span>

    ![][6]
8. <span data-ttu-id="78256-175">Nakonec nezapomeňte přiřadit aplikaci Visual Studio k této vytvořené aplikaci v App Storu.</span><span class="sxs-lookup"><span data-stu-id="78256-175">Finally make sure that you have associated your Visual Studio app with this created app in the App store.</span></span> <span data-ttu-id="78256-176">V sadě Visual Studio klikněte na **Propojit aplikaci se Storem**.</span><span class="sxs-lookup"><span data-stu-id="78256-176">Click **Associate App with Store** in Visual Studio.</span></span>

    ![][7]

## <span data-ttu-id="78256-177"><a id="send"></a>Odeslání oznámení do vaší aplikace</span><span class="sxs-lookup"><span data-stu-id="78256-177"><a id="send"></a>Send a notification to your app</span></span>
[!INCLUDE [Create Windows Push campaign](../../includes/mobile-engagement-windows-push-campaign.md)]

<span data-ttu-id="78256-178">Pokud aplikace běží, zobrazí se oznámení v aplikaci.</span><span class="sxs-lookup"><span data-stu-id="78256-178">If the app is running, you see an in-app notification.</span></span> <span data-ttu-id="78256-179">Pokud je aplikace je zavřená, zobrazí se informační zpráva.</span><span class="sxs-lookup"><span data-stu-id="78256-179">otherwise if the app is closed, you see a toast notification.</span></span>
<span data-ttu-id="78256-180">Pokud se zobrazuje oznámení v aplikaci, ale ne oznámení s informační zprávou a aplikace je spuštěna v režimu ladění v sadě Visual Studio, zkontrolujte v části **Události životního cyklu -> Pozastavit**, že je aplikace pozastavená.</span><span class="sxs-lookup"><span data-stu-id="78256-180">If you see an in-app notification but not a toast notification, and you are running the app in debug mode in Visual Studio, then try **Lifecycle events -> Suspend** in the toolbar to ensure that the app is suspended.</span></span> <span data-ttu-id="78256-181">Po kliknutí na tlačítko Domů při ladění aplikace v sadě Visual Studio nemusí vždy dojít k jejímu pozastavení, a protože se oznámení zobrazuje v aplikaci, nezobrazí se jako informační zpráva.</span><span class="sxs-lookup"><span data-stu-id="78256-181">If you clicked the Home button while debugging the application in Visual Studio, then it doesn't always get suspended and while you see the notification as in-app, it doesn't show up as a toast notification.</span></span>  

![][8]

<!-- URLs. -->
[Mobile Engagement Windows Universal SDK documentation]: ../mobile-engagement-windows-store-integrate-engagement/
<span data-ttu-id="78256-182">[MicrosoftAzure.MobileEngagement]: http://go.microsoft.com/?linkid=9864592</span><span class="sxs-lookup"><span data-stu-id="78256-182">[MicrosoftAzure.MobileEngagement]: http://go.microsoft.com/?linkid=9864592</span></span>
<span data-ttu-id="78256-183">[Centrum vývojářů pro Windows Store]: https://dev.windows.com</span><span class="sxs-lookup"><span data-stu-id="78256-183">[Windows Store Dev Center]: https://dev.windows.com</span></span>
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
