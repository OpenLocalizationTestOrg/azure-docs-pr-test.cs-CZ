---
title: "aaaGet Začínáme s Azure Mobile Engagementem pro Xamarin.Android"
description: "Zjistěte, jak toouse Azure Mobile Engagement s analytickými funkcemi a nabízenými oznámeními pro aplikace pro Xamarin.Android."
services: mobile-engagement
documentationcenter: xamarin
author: piyushjo
manager: erikre
editor: 
ms.assetid: fb68cf98-08a2-41b5-8e59-757469de3fe7
ms.service: mobile-engagement
ms.workload: mobile
ms.tgt_pltfrm: mobile-xamarin-android
ms.devlang: dotnet
ms.topic: hero-article
ms.date: 06/16/2016
ms.author: piyushjo
ms.openlocfilehash: 9d584fea8e8153d511258cf9b6f87f31dac6aeca
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-azure-mobile-engagement-for-xamarinandroid-apps"></a><span data-ttu-id="299c1-103">Začínáme s Azure Mobile Engagementem pro aplikace pro Xamarin.Android</span><span class="sxs-lookup"><span data-stu-id="299c1-103">Get Started with Azure Mobile Engagement for Xamarin.Android Apps</span></span>
[!INCLUDE [Hero tutorial switcher](../../includes/mobile-engagement-hero-tutorial-switcher.md)]

<span data-ttu-id="299c1-104">Toto téma ukazuje, jak Azure Mobile Engagement toounderstand toouse používání aplikace a jak toosend nabízená oznámení uživatelům toosegmented aplikace pro Xamarin.Android.</span><span class="sxs-lookup"><span data-stu-id="299c1-104">This topic shows you how toouse Azure Mobile Engagement toounderstand your app usage and how toosend push notifications toosegmented users of a Xamarin.Android application.</span></span>
<span data-ttu-id="299c1-105">Tento kurz představuje scénář hello jednoduchého vysílání přes Mobile Engagement.</span><span class="sxs-lookup"><span data-stu-id="299c1-105">This tutorial demonstrates hello simple broadcast scenario using Mobile Engagement.</span></span> <span data-ttu-id="299c1-106">V něm můžete vytvořit prázdnou aplikaci pro Xamarin.Android, která sbírá základní data a dostává nabízená oznámení pomocí služby GCM (Google Cloud Messaging).</span><span class="sxs-lookup"><span data-stu-id="299c1-106">In it, you create a blank Xamarin.Android app that collects basic data and receives push notifications using Google Cloud Messaging (GCM).</span></span>

> [!NOTE]
> <span data-ttu-id="299c1-107">Služba Azure Mobile Engagement Hello vyřadí března 2018 a je aktuálně pouze k dispozici tooexisting zákazníků.</span><span class="sxs-lookup"><span data-stu-id="299c1-107">hello Azure Mobile Engagement service will be retired March 2018 and is currently only available tooexisting customers.</span></span> <span data-ttu-id="299c1-108">Další informace najdete v tématu [Mobile Engagement](https://azure.microsoft.com/en-us/services/mobile-engagement/).</span><span class="sxs-lookup"><span data-stu-id="299c1-108">For more information, see [Mobile Engagement](https://azure.microsoft.com/en-us/services/mobile-engagement/).</span></span>

<span data-ttu-id="299c1-109">Tento kurz vyžaduje hello následující:</span><span class="sxs-lookup"><span data-stu-id="299c1-109">This tutorial requires hello following:</span></span>

* <span data-ttu-id="299c1-110">[Xamarin Studio](http://xamarin.com/studio).</span><span class="sxs-lookup"><span data-stu-id="299c1-110">[Xamarin Studio](http://xamarin.com/studio).</span></span> <span data-ttu-id="299c1-111">Můžete také použít Visual Studio s Xamarinem, ale v tomto kurzu používáme Xamarin Studio.</span><span class="sxs-lookup"><span data-stu-id="299c1-111">You can also use Visual Studio with Xamarin but this tutorial uses Xamarin Studio.</span></span> <span data-ttu-id="299c1-112">Instalační pokyny najdete v tématu o [nastavení a instalaci pro Visual Studio a Xamarin](https://msdn.microsoft.com/library/mt613162.aspx).</span><span class="sxs-lookup"><span data-stu-id="299c1-112">For installation instructions, see [Setup and Install for Visual Studio and Xamarin](https://msdn.microsoft.com/library/mt613162.aspx).</span></span>
* [<span data-ttu-id="299c1-113">Mobile Engagement Xamarin SDK</span><span class="sxs-lookup"><span data-stu-id="299c1-113">Mobile Engagement Xamarin SDK</span></span>](https://www.nuget.org/packages/Microsoft.Azure.Engagement.Xamarin/)

> [!NOTE]
> <span data-ttu-id="299c1-114">toocomplete tento kurz, musíte mít aktivní účet Azure.</span><span class="sxs-lookup"><span data-stu-id="299c1-114">toocomplete this tutorial, you must have an active Azure account.</span></span> <span data-ttu-id="299c1-115">Pokud účet nemáte, můžete si během několika minut vytvořit bezplatný zkušební účet.</span><span class="sxs-lookup"><span data-stu-id="299c1-115">If you don't have an account, you can create a free trial account in just a couple of minutes.</span></span> <span data-ttu-id="299c1-116">Podrobnosti najdete v článku [Bezplatná zkušební verze Azure](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A0E0E5C02&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdocumentation%2Farticles%2Fmobile-engagement-xamarin-android-get-started).</span><span class="sxs-lookup"><span data-stu-id="299c1-116">For details, see [Azure Free Trial](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A0E0E5C02&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdocumentation%2Farticles%2Fmobile-engagement-xamarin-android-get-started).</span></span>
> 
> 

## <span data-ttu-id="299c1-117"><a id="setup-azme"></a>Nastavení Mobile Engagementu pro vaši aplikaci pro Android</span><span class="sxs-lookup"><span data-stu-id="299c1-117"><a id="setup-azme"></a>Setup Mobile Engagement for your Android app</span></span>
[!INCLUDE [Create Mobile Engagement App in Portal](../../includes/mobile-engagement-create-app-in-portal-new.md)]

## <span data-ttu-id="299c1-118"><a id="connecting-app"></a>Připojit vaše aplikace toohello Mobile Engagement back-end</span><span class="sxs-lookup"><span data-stu-id="299c1-118"><a id="connecting-app"></a>Connect your app toohello Mobile Engagement backend</span></span>
<span data-ttu-id="299c1-119">Tento kurzu si představíme "základní integraci", což je minimální hello nastavte požadované toocollect dat a odesílání nabízených oznámení.</span><span class="sxs-lookup"><span data-stu-id="299c1-119">This tutorial presents a "basic integration", which is hello minimal set required toocollect data and send a push notification.</span></span> 

<span data-ttu-id="299c1-120">Pomocí Xamarin Studio toodemonstrate hello integrace vytvoříme základní aplikaci.</span><span class="sxs-lookup"><span data-stu-id="299c1-120">We will create a basic app with Xamarin Studio toodemonstrate hello integration.</span></span>

### <a name="create-a-new-xamarinandroid-project"></a><span data-ttu-id="299c1-121">Vytvoření nového projektu Xamarin.Android</span><span class="sxs-lookup"><span data-stu-id="299c1-121">Create a new Xamarin.Android project</span></span>
1. <span data-ttu-id="299c1-122">Spusťte **Xamarin Studio** přejděte příliš**soubor** -> **nový** -> **řešení**</span><span class="sxs-lookup"><span data-stu-id="299c1-122">Launch **Xamarin Studio** Go too**File** -> **New** -> **Solution**</span></span> 
   
    ![][1]
2. <span data-ttu-id="299c1-123">Vyberte **aplikace pro Android** zkontrolujte hello vybraný jazyk je **C#** a klikněte na tlačítko **Další**.</span><span class="sxs-lookup"><span data-stu-id="299c1-123">Select **Android App** then make sure hello selected language is **C#** and click **Next**.</span></span>
   
    ![][2]
3. <span data-ttu-id="299c1-124">Vyplňte hello **název aplikace** a hello **identifikátor organizace**.</span><span class="sxs-lookup"><span data-stu-id="299c1-124">Fill in hello **App Name** and hello **Organization Identifier**.</span></span> <span data-ttu-id="299c1-125">Ujistěte se, že toocheckmark **služby Google Play** a pak klikněte na **Další**.</span><span class="sxs-lookup"><span data-stu-id="299c1-125">Make sure toocheckmark **Google Play Services** and then click **Next**.</span></span> 
   
    ![][3]
4. <span data-ttu-id="299c1-126">Aktualizace hello **název projektu**, **název řešení** a **umístění** dle potřeby **vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="299c1-126">Update hello **Project Name**, **Solution Name** and **Location** if required and click **Create**.</span></span>
   
    ![][4]

<span data-ttu-id="299c1-127">Xamarin Studio vytvoří aplikaci hello, do které budeme integrovat Mobile Engagement.</span><span class="sxs-lookup"><span data-stu-id="299c1-127">Xamarin Studio will create hello app in which we will integrate Mobile Engagement.</span></span> 

### <a name="connect-your-app-toomobile-engagement-backend"></a><span data-ttu-id="299c1-128">Připojit vaše tooMobile Engagement back-end aplikace</span><span class="sxs-lookup"><span data-stu-id="299c1-128">Connect your app tooMobile Engagement backend</span></span>
1. <span data-ttu-id="299c1-129">Klikněte pravým tlačítkem na hello **balíčky** složku v systému windows hello řešení a vyberte **přidat balíčky...**</span><span class="sxs-lookup"><span data-stu-id="299c1-129">Right click hello **Packages** folder in hello Solution windows and select **Add Packages...**</span></span>
   
    ![][5]
2. <span data-ttu-id="299c1-130">Vyhledejte hello **Microsoft Azure Mobile Engagement Xamarin SDK** a přidejte ji tooyour řešení.</span><span class="sxs-lookup"><span data-stu-id="299c1-130">Search for hello **Microsoft Azure Mobile Engagement Xamarin SDK** and add it tooyour solution.</span></span>  
   
    ![][6]
3. <span data-ttu-id="299c1-131">Otevřete **MainActivity.cs** a přidejte hello následující příkazy:</span><span class="sxs-lookup"><span data-stu-id="299c1-131">Open **MainActivity.cs** and add hello following using statements:</span></span>
   
        using Microsoft.Azure.Engagement;
        using Microsoft.Azure.Engagement.Activity;
4. <span data-ttu-id="299c1-132">V hello `OnCreate` metoda, přidejte následující tooinitialize hello připojení s back-end Mobile Engagementu hello.</span><span class="sxs-lookup"><span data-stu-id="299c1-132">In hello `OnCreate` method, add hello following tooinitialize hello connection with Mobile Engagement backend.</span></span> <span data-ttu-id="299c1-133">Ujistěte se, že tooadd vaše **ConnectionString**.</span><span class="sxs-lookup"><span data-stu-id="299c1-133">Make sure tooadd your **ConnectionString**.</span></span> 
   
        EngagementConfiguration engagementConfiguration = new EngagementConfiguration();
        engagementConfiguration.ConnectionString = "YourConnectionStringFromAzurePortal";
        EngagementAgent.Init(engagementConfiguration);

### <a name="add-permissions-and-a-service-declaration"></a><span data-ttu-id="299c1-134">Přidání oprávnění a deklarace služby</span><span class="sxs-lookup"><span data-stu-id="299c1-134">Add permissions and a service declaration</span></span>
1. <span data-ttu-id="299c1-135">Otevřete hello **Manifest.xml** soubor ve složce vlastnosti hello.</span><span class="sxs-lookup"><span data-stu-id="299c1-135">Open up hello **Manifest.xml** file under hello Properties folder.</span></span> <span data-ttu-id="299c1-136">Vyberte kartu zdroje, aby bylo přímo aktualizovat zdroj dat XML hello.</span><span class="sxs-lookup"><span data-stu-id="299c1-136">Select Source tab so that you directly update hello XML source.</span></span>
2. <span data-ttu-id="299c1-137">Přidat tyto toohello oprávnění souboru Manifest.xml (který najdete v části hello **vlastnosti** složky) vašeho projektu těsně před nebo po hello `<application>` značky:</span><span class="sxs-lookup"><span data-stu-id="299c1-137">Add these permissions toohello Manifest.xml (which can be found under hello **Properties** folder) of your project immediately before or after hello `<application>` tag:</span></span>
   
        <uses-permission android:name="android.permission.INTERNET"/>
        <uses-permission android:name="android.permission.ACCESS_NETWORK_STATE"/>
        <uses-permission android:name="android.permission.WRITE_EXTERNAL_STORAGE"/>
        <uses-permission android:name="android.permission.RECEIVE_BOOT_COMPLETED" />
        <uses-permission android:name="android.permission.VIBRATE" />
        <uses-permission android:name="android.permission.DOWNLOAD_WITHOUT_NOTIFICATION"/>
3. <span data-ttu-id="299c1-138">Přidejte následující hello mezi hello `<application>` a `</application>` značky toodeclare hello agenta služby:</span><span class="sxs-lookup"><span data-stu-id="299c1-138">Add hello following between hello `<application>` and `</application>` tags toodeclare hello agent service:</span></span>
   
        <service
             android:name="com.microsoft.azure.engagement.service.EngagementService"
             android:exported="false"
             android:label="<Your application name>"
             android:process=":Engagement"/>
4. <span data-ttu-id="299c1-139">V kódu hello jste vložili, nahraďte `"<Your application name>"` v popisku hello.</span><span class="sxs-lookup"><span data-stu-id="299c1-139">In hello code you just pasted, replace `"<Your application name>"` in hello label.</span></span> <span data-ttu-id="299c1-140">Zobrazí se v hello **nastavení** nabídky, kde uživatelé vidí služby spuštěné na zařízení hello.</span><span class="sxs-lookup"><span data-stu-id="299c1-140">This is displayed in hello **Settings** menu where users can see services running on hello device.</span></span> <span data-ttu-id="299c1-141">Do tohoto popisku můžete přidat hello slovo "Service" třeba.</span><span class="sxs-lookup"><span data-stu-id="299c1-141">You can add hello word "Service" in that label for example.</span></span>

### <a name="send-a-screen-toomobile-engagement"></a><span data-ttu-id="299c1-142">Odeslání obrazovky tooMobile zapojení</span><span class="sxs-lookup"><span data-stu-id="299c1-142">Send a screen tooMobile Engagement</span></span>
<span data-ttu-id="299c1-143">V pořadí toostart odesílat data a zajistit, že hello uživatelé jsou aktivní musíte odeslat alespoň jednu obrazovku toohello Mobile Engagement back-end.</span><span class="sxs-lookup"><span data-stu-id="299c1-143">In order toostart sending data and ensuring hello users are active, you must send at least one screen toohello Mobile Engagement backend.</span></span> <span data-ttu-id="299c1-144">To chcete provést – Ujistěte se, že hello `MainActivity` dědí z `EngagementActivity` místo `Activity`.</span><span class="sxs-lookup"><span data-stu-id="299c1-144">For doing this - ensure that hello `MainActivity` inherits from `EngagementActivity` instead of `Activity`.</span></span>

    public class MainActivity : EngagementActivity

<span data-ttu-id="299c1-145">Pokud `EngagementActivity` neumožňuje převzetí, musíte do `OnResume` a `OnPause` přidat metody `.StartActivity` a `.EndActivity`.</span><span class="sxs-lookup"><span data-stu-id="299c1-145">Alternatively, if you cannot inherit from `EngagementActivity` then you must add `.StartActivity` and `.EndActivity` methods in `OnResume` and `OnPause` respectively.</span></span>  

        protected override void OnResume()
            {
                EngagementAgent.StartActivity(EngagementAgentUtils.BuildEngagementActivityName(Java.Lang.Class.FromType(this.GetType())), null);
                base.OnResume();             
            }

            protected override void OnPause()
            {
                EngagementAgent.EndActivity();
                base.OnPause();            
            }

## <span data-ttu-id="299c1-146"><a id="monitor"></a>Připojení aplikace se sledováním v reálném čase</span><span class="sxs-lookup"><span data-stu-id="299c1-146"><a id="monitor"></a>Connect app with real-time monitoring</span></span>
[!INCLUDE [Connect app with real-time monitoring](../../includes/mobile-engagement-connect-app-with-monitor.md)]

## <span data-ttu-id="299c1-147"><a id="integrate-push"></a>Povolení nabízených oznámení a zasílání zpráv v aplikaci</span><span class="sxs-lookup"><span data-stu-id="299c1-147"><a id="integrate-push"></a>Enable push notifications and in-app messaging</span></span>
<span data-ttu-id="299c1-148">Mobile Engagement vám umožní toointeract s OSLOVIT uživatele a komunikovat s nabízená oznámení a zasílání zpráv v kontextu hello kampaní v aplikaci.</span><span class="sxs-lookup"><span data-stu-id="299c1-148">Mobile Engagement allows you toointeract with and REACH your users with push notifications and in-app messaging in hello context of campaigns.</span></span> <span data-ttu-id="299c1-149">Tento modul je hello portálu Mobile Engagement nazývá REACH.</span><span class="sxs-lookup"><span data-stu-id="299c1-149">This module is called REACH in hello Mobile Engagement portal.</span></span>
<span data-ttu-id="299c1-150">Následující části Hello nastaví tooreceive vaše aplikace je.</span><span class="sxs-lookup"><span data-stu-id="299c1-150">hello following sections sets up your app tooreceive them.</span></span>

[!INCLUDE [Enable Google Cloud Messaging](../../includes/mobile-engagement-enable-google-cloud-messaging.md)]

[!INCLUDE [Enable in-app messaging](../../includes/mobile-engagement-android-send-push.md)]

[!INCLUDE [Send notification from portal](../../includes/mobile-engagement-android-send-push-from-portal.md)]

<!-- Images -->
[1]: ./media/mobile-engagement-xamarin-android-get-started/1.png
[2]: ./media/mobile-engagement-xamarin-android-get-started/2.png
[3]: ./media/mobile-engagement-xamarin-android-get-started/3.png
[4]: ./media/mobile-engagement-xamarin-android-get-started/4.png
[5]: ./media/mobile-engagement-xamarin-android-get-started/5.png
[6]: ./media/mobile-engagement-xamarin-android-get-started/6.png
