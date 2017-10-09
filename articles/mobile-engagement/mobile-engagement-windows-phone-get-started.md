---
title: "aaaGet začít s Azure Mobile Engagement pro Windows Phone aplikace Silverlight"
description: "Zjistěte, jak toouse Azure Mobile Engagement s analytickými funkcemi a nabízenými oznámeními pro aplikace pro Windows Phone Silverlight."
services: mobile-engagement
documentationcenter: windows
author: piyushjo
manager: erikre
editor: 
ms.assetid: aa34692f-87f7-47c6-a20c-a1972750bc25
ms.service: mobile-engagement
ms.workload: mobile
ms.tgt_pltfrm: mobile-windows-phone
ms.devlang: dotnet
ms.topic: hero-article
ms.date: 08/19/2016
ms.author: piyushjo
ms.openlocfilehash: b39a838ab03217b2dc845cbf59d7bf8b094dac1f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-azure-mobile-engagement-for-windows-phone-silverlight-apps"></a><span data-ttu-id="a1bfa-103">Začínáme s Azure Mobile Engagementem pro aplikace Silverlight pro Windows Phone</span><span class="sxs-lookup"><span data-stu-id="a1bfa-103">Get started with Azure Mobile Engagement for Windows Phone Silverlight apps</span></span>
[!INCLUDE [Hero tutorial switcher](../../includes/mobile-engagement-hero-tutorial-switcher.md)]

<span data-ttu-id="a1bfa-104">Toto téma ukazuje, jak Azure Mobile Engagement toounderstand toouse využití aplikací a odesílat nabízená oznámení uživatelům toosegmented aplikace pro Windows Phone Silverlight.</span><span class="sxs-lookup"><span data-stu-id="a1bfa-104">This topic shows you how toouse Azure Mobile Engagement toounderstand your app usage and send push notifications toosegmented users of a Windows Phone Silverlight application.</span></span>
<span data-ttu-id="a1bfa-105">Tento kurz představuje scénář hello jednoduchého vysílání přes Mobile Engagement.</span><span class="sxs-lookup"><span data-stu-id="a1bfa-105">This tutorial demonstrates hello simple broadcast scenario using Mobile Engagement.</span></span> <span data-ttu-id="a1bfa-106">Vytvoříte prázdnou aplikaci Silverlight pro Windows Phone, která bude shromažďovat základní data a přijímat nabízená oznámení pomocí Microsoft Služby nabízených oznámení (MPNS).</span><span class="sxs-lookup"><span data-stu-id="a1bfa-106">In it, you create a blank Windows Phone Silverlight app that collects basic data and receives push notifications using Microsoft Push Notification Service (MPNS).</span></span>

> [!NOTE]
> <span data-ttu-id="a1bfa-107">Služba Azure Mobile Engagement Hello vyřadí března 2018 a je aktuálně pouze k dispozici tooexisting zákazníků.</span><span class="sxs-lookup"><span data-stu-id="a1bfa-107">hello Azure Mobile Engagement service will be retired March 2018 and is currently only available tooexisting customers.</span></span> <span data-ttu-id="a1bfa-108">Další informace najdete v tématu [Mobile Engagement](https://azure.microsoft.com/en-us/services/mobile-engagement/).</span><span class="sxs-lookup"><span data-stu-id="a1bfa-108">For more information, see [Mobile Engagement](https://azure.microsoft.com/en-us/services/mobile-engagement/).</span></span>

> [!NOTE]
> <span data-ttu-id="a1bfa-109">Projekty pro Windows Phone 8.1 a starší verze nejsou podporovány v sadě Visual Studio 2017.</span><span class="sxs-lookup"><span data-stu-id="a1bfa-109">Windows Phone 8.1 and prior version projects are not supported in Visual Studio 2017.</span></span>  <span data-ttu-id="a1bfa-110">Další informace najdete v tématu [Cílení na platformy a kompatibilita v sadě Visual Studio 2017](https://www.visualstudio.com/en-us/productinfo/vs2017-compatibility-vs).</span><span class="sxs-lookup"><span data-stu-id="a1bfa-110">For more information, see [Visual Studio 2017 Platform Targeting and Compatibility](https://www.visualstudio.com/en-us/productinfo/vs2017-compatibility-vs).</span></span>

> [!NOTE]
> <span data-ttu-id="a1bfa-111">Pokud cílíte na Windows Phone 8.1 (bez Silverlight), přečtěte si téma toohello [kurz pro univerzální aplikace Windows](mobile-engagement-windows-store-dotnet-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="a1bfa-111">If you are targeting Windows Phone 8.1 (non-Silverlight), refer toohello [Windows Universal tutorial](mobile-engagement-windows-store-dotnet-get-started.md).</span></span>
> 
> 

<span data-ttu-id="a1bfa-112">Tento kurz vyžaduje hello následující:</span><span class="sxs-lookup"><span data-stu-id="a1bfa-112">This tutorial requires hello following:</span></span>

* <span data-ttu-id="a1bfa-113">Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="a1bfa-113">Visual Studio 2013</span></span>
* <span data-ttu-id="a1bfa-114">Balíček NuGet [MicrosoftAzure.MobileEngagement]</span><span class="sxs-lookup"><span data-stu-id="a1bfa-114">[MicrosoftAzure.MobileEngagement] Nuget package</span></span>

> [!NOTE]
> <span data-ttu-id="a1bfa-115">toocomplete tento kurz, musíte mít aktivní účet Azure.</span><span class="sxs-lookup"><span data-stu-id="a1bfa-115">toocomplete this tutorial, you must have an active Azure account.</span></span> <span data-ttu-id="a1bfa-116">Pokud účet nemáte, můžete si během několika minut vytvořit bezplatný zkušební účet.</span><span class="sxs-lookup"><span data-stu-id="a1bfa-116">If you don't have an account, you can create a free trial account in just a couple of minutes.</span></span> <span data-ttu-id="a1bfa-117">Podrobnosti najdete v článku [Bezplatná zkušební verze Azure](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A0E0E5C02&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdocumentation%2Farticles%2Fmobile-engagement-windows-phone-get-started).</span><span class="sxs-lookup"><span data-stu-id="a1bfa-117">For details, see [Azure Free Trial](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A0E0E5C02&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdocumentation%2Farticles%2Fmobile-engagement-windows-phone-get-started).</span></span>
> 
> 

## <span data-ttu-id="a1bfa-118"><a id="setup-azme"></a>Nastavení Mobile Engagementu pro aplikaci pro Windows Phone</span><span class="sxs-lookup"><span data-stu-id="a1bfa-118"><a id="setup-azme"></a>Setup Mobile Engagement for your Windows Phone app</span></span>
[!INCLUDE [Create Mobile Engagement App in Portal](../../includes/mobile-engagement-create-app-in-portal-new.md)]

## <span data-ttu-id="a1bfa-119"><a id="connecting-app"></a>Připojit vaše aplikace toohello Mobile Engagement back-end</span><span class="sxs-lookup"><span data-stu-id="a1bfa-119"><a id="connecting-app"></a>Connect your app toohello Mobile Engagement backend</span></span>
<span data-ttu-id="a1bfa-120">Tento kurzu si představíme "základní integraci", což je minimální hello nastavte požadované toocollect dat a odesílání nabízených oznámení.</span><span class="sxs-lookup"><span data-stu-id="a1bfa-120">This tutorial presents a "basic integration", which is hello minimal set required toocollect data and send a push notification.</span></span> <span data-ttu-id="a1bfa-121">Hello kompletní dokumentaci k integraci najdete v hello [integrace sady Mobile Engagement Windows Phone SDK](mobile-engagement-windows-phone-sdk-overview.md)</span><span class="sxs-lookup"><span data-stu-id="a1bfa-121">hello complete integration documentation can be found in hello [Mobile Engagement Windows Phone SDK integration](mobile-engagement-windows-phone-sdk-overview.md)</span></span>

<span data-ttu-id="a1bfa-122">Pomocí integrace sady Visual Studio toodemonstrate hello vytvoříme základní aplikaci.</span><span class="sxs-lookup"><span data-stu-id="a1bfa-122">We will create a basic app with Visual Studio toodemonstrate hello integration.</span></span>

### <a name="create-a-new-windows-phone-silverlight-project"></a><span data-ttu-id="a1bfa-123">Vytvoření nového projektu Windows Phone Silverlight</span><span class="sxs-lookup"><span data-stu-id="a1bfa-123">Create a new Windows Phone Silverlight project</span></span>
<span data-ttu-id="a1bfa-124">Hello následující postup předpokládá použití hello sady Visual Studio 2015 když hello postup je podobný jako ve starších verzích sady Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="a1bfa-124">hello following steps assume hello use of Visual Studio 2015 though hello steps are similar in earlier versions of Visual Studio.</span></span> 

1. <span data-ttu-id="a1bfa-125">Spuštění sady Visual Studio a v hello **Domů** obrazovku, vyberte **nový projekt**.</span><span class="sxs-lookup"><span data-stu-id="a1bfa-125">Start Visual Studio, and in hello **Home** screen, select **New Project**.</span></span>
2. <span data-ttu-id="a1bfa-126">V místní nabídce hello vyberte **Windows 8** -> **Windows Phone** -> **prázdná aplikace (Windows Phone Silverlight)**.</span><span class="sxs-lookup"><span data-stu-id="a1bfa-126">In hello pop-up, select **Windows 8** -> **Windows Phone** -> **Blank App (Windows Phone Silverlight)**.</span></span> <span data-ttu-id="a1bfa-127">Vyplňte aplikace hello **název** a **název řešení**a potom klikněte na **OK**.</span><span class="sxs-lookup"><span data-stu-id="a1bfa-127">Fill in hello app **Name** and **Solution name**, and then click **OK**.</span></span>
   
    ![][1]
3. <span data-ttu-id="a1bfa-128">Buď můžete tootarget **Windows Phone 8.0** nebo **Windows Phone 8.1**.</span><span class="sxs-lookup"><span data-stu-id="a1bfa-128">You can choose tootarget either **Windows Phone 8.0** or **Windows Phone 8.1**.</span></span>

<span data-ttu-id="a1bfa-129">Nyní jste vytvořili novou aplikaci Silverlight pro Windows Phone, do které budeme integrovat hello Azure Mobile Engagement SDK.</span><span class="sxs-lookup"><span data-stu-id="a1bfa-129">You have now created a new Windows Phone Silverlight app into which we will integrate hello Azure Mobile Engagement SDK.</span></span>

### <a name="connect-your-app-toohello-mobile-engagement-backend"></a><span data-ttu-id="a1bfa-130">Připojit vaše aplikace toohello Mobile Engagement back-end</span><span class="sxs-lookup"><span data-stu-id="a1bfa-130">Connect your app toohello Mobile Engagement backend</span></span>
1. <span data-ttu-id="a1bfa-131">Nainstalujte hello [MicrosoftAzure.MobileEngagement] balíček nuget do projektu.</span><span class="sxs-lookup"><span data-stu-id="a1bfa-131">Install hello [MicrosoftAzure.MobileEngagement] nuget package in your project.</span></span>
2. <span data-ttu-id="a1bfa-132">Otevřete `WMAppManifest.xml` (ve složce vlastnosti hello) a zajistěte, aby jsou deklarovány následující hello (přidejte je, pokud nejsou) v hello `<Capabilities />` značky:</span><span class="sxs-lookup"><span data-stu-id="a1bfa-132">Open `WMAppManifest.xml` (under hello Properties folder) and make sure hello following are declared (add them if they are not) in hello `<Capabilities />` tag:</span></span>
   
        <Capability Name="ID_CAP_NETWORKING" />
        <Capability Name="ID_CAP_IDENTITY_DEVICE" />
   
    ![][2]
3. <span data-ttu-id="a1bfa-133">Nyní vložte hello připojovací řetězec, který jste zkopírovali dříve pro aplikaci Mobile Engagement a vložte ji do hello `Resources\EngagementConfiguration.xml` souboru mezi hello `<connectionString>` a `</connectionString>` značky:</span><span class="sxs-lookup"><span data-stu-id="a1bfa-133">Now paste hello connection string that you copied earlier for your Mobile Engagement app and paste it in hello `Resources\EngagementConfiguration.xml` file, between hello `<connectionString>` and `</connectionString>` tags:</span></span>
   
    ![][3]
4. <span data-ttu-id="a1bfa-134">V hello `App.xaml.cs` souboru:</span><span class="sxs-lookup"><span data-stu-id="a1bfa-134">In hello `App.xaml.cs` file:</span></span>
   
    <span data-ttu-id="a1bfa-135">a.</span><span class="sxs-lookup"><span data-stu-id="a1bfa-135">a.</span></span> <span data-ttu-id="a1bfa-136">Přidat hello `using` příkaz:</span><span class="sxs-lookup"><span data-stu-id="a1bfa-136">Add hello `using` statement:</span></span>
   
            using Microsoft.Azure.Engagement;
   
    <span data-ttu-id="a1bfa-137">b.</span><span class="sxs-lookup"><span data-stu-id="a1bfa-137">b.</span></span> <span data-ttu-id="a1bfa-138">Inicializace hello SDK v hello `Application_Launching` metoda:</span><span class="sxs-lookup"><span data-stu-id="a1bfa-138">Initialize hello SDK in hello `Application_Launching` method:</span></span>
   
            private void Application_Launching(object sender, LaunchingEventArgs e)
            {
              EngagementAgent.Instance.Init();
            }
   
    <span data-ttu-id="a1bfa-139">c.</span><span class="sxs-lookup"><span data-stu-id="a1bfa-139">c.</span></span> <span data-ttu-id="a1bfa-140">Vložte následující hello hello `Application_Activated`:</span><span class="sxs-lookup"><span data-stu-id="a1bfa-140">Insert hello following in hello `Application_Activated`:</span></span>
   
            private void Application_Activated(object sender, ActivatedEventArgs e)
            {
               EngagementAgent.Instance.OnActivated(e);
            }

## <span data-ttu-id="a1bfa-141"><a id="monitor"></a>Povolení sledování v reálném čase</span><span class="sxs-lookup"><span data-stu-id="a1bfa-141"><a id="monitor"></a>Enable real-time monitoring</span></span>
<span data-ttu-id="a1bfa-142">V pořadí toostart odesílat data a zajistit, že hello uživatelé jsou aktivní musíte odeslat alespoň jednu obrazovku (aktivitu) toohello Mobile Engagement back-end.</span><span class="sxs-lookup"><span data-stu-id="a1bfa-142">In order toostart sending data and ensuring that hello users are active, you must send at least one screen (Activity) toohello Mobile Engagement backend.</span></span>

1. <span data-ttu-id="a1bfa-143">Hello MainPage.xaml.cs přidejte hello `using` příkaz:</span><span class="sxs-lookup"><span data-stu-id="a1bfa-143">In hello MainPage.xaml.cs, add hello `using` statement:</span></span>
   
        using Microsoft.Azure.Engagement;
2. <span data-ttu-id="a1bfa-144">Nahraďte základní třídu hello **MainPage**, která je před **PhoneApplicationPage**, s **EngagementPage**.</span><span class="sxs-lookup"><span data-stu-id="a1bfa-144">Replace hello base class of **MainPage**, which is before **PhoneApplicationPage**, with **EngagementPage**.</span></span>
   
        class MainPage : EngagementPage 
3. <span data-ttu-id="a1bfa-145">V souboru `MainPage.xml`:</span><span class="sxs-lookup"><span data-stu-id="a1bfa-145">In your `MainPage.xml` file:</span></span>
   
    <span data-ttu-id="a1bfa-146">a.</span><span class="sxs-lookup"><span data-stu-id="a1bfa-146">a.</span></span> <span data-ttu-id="a1bfa-147">Přidejte tooyour deklarací oborů názvů:</span><span class="sxs-lookup"><span data-stu-id="a1bfa-147">Add tooyour namespaces declarations:</span></span>
   
            xmlns:engagement="clr-namespace:Microsoft.Azure.Engagement;assembly=Microsoft.Azure.Engagement.EngagementAgent.WP"
   
    <span data-ttu-id="a1bfa-148">b.</span><span class="sxs-lookup"><span data-stu-id="a1bfa-148">b.</span></span> <span data-ttu-id="a1bfa-149">Nahraďte `phone:PhoneApplicationPage` v názvu značky XML hello s `engagement:EngagementPage`.</span><span class="sxs-lookup"><span data-stu-id="a1bfa-149">Replace `phone:PhoneApplicationPage` in hello XML tag name with `engagement:EngagementPage`.</span></span>

## <span data-ttu-id="a1bfa-150"><a id="monitor"></a>Připojení aplikace se sledováním v reálném čase</span><span class="sxs-lookup"><span data-stu-id="a1bfa-150"><a id="monitor"></a>Connect app with real-time monitoring</span></span>
[!INCLUDE [Connect app with real-time monitoring](../../includes/mobile-engagement-connect-app-with-monitor.md)]

## <span data-ttu-id="a1bfa-151"><a id="integrate-push"></a>Povolení nabízených oznámení a zasílání zpráv v aplikaci</span><span class="sxs-lookup"><span data-stu-id="a1bfa-151"><a id="integrate-push"></a>Enable push notifications and in-app messaging</span></span>
<span data-ttu-id="a1bfa-152">Mobile Engagement vám umožní toointeract oslovit uživatele a pomocí nabízených oznámení a zasílání zpráv v aplikaci v kontextu hello kampaní.</span><span class="sxs-lookup"><span data-stu-id="a1bfa-152">Mobile Engagement allows you toointeract and reach your users with Push Notifications and in-app Messaging in hello context of campaigns.</span></span> <span data-ttu-id="a1bfa-153">Tento modul je hello portálu Mobile Engagement nazývá REACH.</span><span class="sxs-lookup"><span data-stu-id="a1bfa-153">This module is called REACH in hello Mobile Engagement portal.</span></span>
<span data-ttu-id="a1bfa-154">Hello následujících sekcích nastavíte tooreceive vaše aplikace je.</span><span class="sxs-lookup"><span data-stu-id="a1bfa-154">hello following sections set up your app tooreceive them.</span></span>

### <a name="enable-your-app-tooreceive-mpns-push-notifications"></a><span data-ttu-id="a1bfa-155">Povolit tooreceive nabízených oznámení MPNS vaší aplikace</span><span class="sxs-lookup"><span data-stu-id="a1bfa-155">Enable your app tooreceive MPNS Push Notifications</span></span>
<span data-ttu-id="a1bfa-156">Přidat nové možnosti tooyour `WMAppManifest.xml` souboru:</span><span class="sxs-lookup"><span data-stu-id="a1bfa-156">Add new Capabilities tooyour `WMAppManifest.xml` file:</span></span>

        ID_CAP_PUSH_NOTIFICATION
        ID_CAP_WEBBROWSERCOMPONENT

   ![][5]

### <a name="initialize-hello-reach-sdk"></a><span data-ttu-id="a1bfa-157">Inicializace hello REACH SDK</span><span class="sxs-lookup"><span data-stu-id="a1bfa-157">Initialize hello REACH SDK</span></span>
1. <span data-ttu-id="a1bfa-158">V `App.xaml.cs`, volání `EngagementReach.Instance.Init();` v hello **Application_Launching** funkce hned po inicializaci agenta hello:</span><span class="sxs-lookup"><span data-stu-id="a1bfa-158">In `App.xaml.cs`, call `EngagementReach.Instance.Init();` in hello **Application_Launching** function, right after hello agent initialization:</span></span>
   
        private void Application_Launching(object sender, LaunchingEventArgs e)
        {
           EngagementAgent.Instance.Init();
           EngagementReach.Instance.Init();
        }
2. <span data-ttu-id="a1bfa-159">V `App.xaml.cs`, volání `EngagementReach.Instance.OnActivated(e);` v hello **Application_Activated** funkce hned po inicializaci agenta hello:</span><span class="sxs-lookup"><span data-stu-id="a1bfa-159">In `App.xaml.cs`, call `EngagementReach.Instance.OnActivated(e);` in hello **Application_Activated** function, right after hello agent initialization:</span></span>
   
        private void Application_Activated(object sender, ActivatedEventArgs e)
        {
           EngagementAgent.Instance.OnActivated(e);
           EngagementReach.Instance.OnActivated(e);
        }

<span data-ttu-id="a1bfa-160">Nyní je vše připraveno.</span><span class="sxs-lookup"><span data-stu-id="a1bfa-160">You're all set.</span></span> <span data-ttu-id="a1bfa-161">A teď ověříme, zda jste základní integraci provedli správně.</span><span class="sxs-lookup"><span data-stu-id="a1bfa-161">Now we will verify that you have correctly cried out this basic integration.</span></span>

## <span data-ttu-id="a1bfa-162"><a id="send"></a>Poslat tooyour aplikaci oznámení</span><span class="sxs-lookup"><span data-stu-id="a1bfa-162"><a id="send"></a>Send a notification tooyour app</span></span>
[!INCLUDE [Create Windows Push campaign](../../includes/mobile-engagement-windows-push-campaign.md)]

<span data-ttu-id="a1bfa-163">Pokud je aplikace hello otevřete jinak jako oznámení s informační zprávou jako následující hello, měli byste vidět oznámení na vašem zařízení, které se zobrazí jako oznámení v aplikaci:</span><span class="sxs-lookup"><span data-stu-id="a1bfa-163">You should now see a notification on your device which will show up as an in-app notification if hello app is open otherwise as a toast notification like hello following:</span></span> 

![][6]

<!-- URLs. -->
[MicrosoftAzure.MobileEngagement]: http://go.microsoft.com/?linkid=9874664
[Mobile Engagement Windows Phone SDK documentation]: ../mobile-engagement-windows-phone-integrate-engagement/

<!-- Images. -->
[1]: ./media/mobile-engagement-windows-phone-get-started/project-properties.png
[2]: ./media/mobile-engagement-windows-phone-get-started/wmappmanifest-capabilities.png
[3]: ./media/mobile-engagement-windows-phone-get-started/add-connection-string.png
[5]: ./media/mobile-engagement-windows-phone-get-started/reach-capabilities.png
[6]: ./media/mobile-engagement-windows-phone-get-started/push-screenshot.png
