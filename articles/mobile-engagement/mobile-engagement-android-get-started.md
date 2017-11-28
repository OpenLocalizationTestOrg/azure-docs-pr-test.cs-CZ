---
title: "aaaGet začít s Androidem aplikace Azure Mobile Engagement"
description: "Zjistěte, jak toouse Azure Mobile Engagement s analytickými funkcemi a nabízenými oznámeními pro aplikace pro Android."
services: mobile-engagement
documentationcenter: android
author: piyushjo
manager: erikre
editor: 
ms.assetid: 3c286c6d-cfef-4e3e-9b2c-715429fe82db
ms.service: mobile-engagement
ms.workload: mobile
ms.tgt_pltfrm: mobile-android
ms.devlang: Java
ms.topic: hero-article
ms.date: 08/10/2016
ms.author: piyushjo;ricksal
ms.openlocfilehash: e8c92607691104750cdf1c4f7639a041d8a7bcd5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-azure-mobile-engagement-for-android-apps"></a><span data-ttu-id="62897-103">Začínáme s Azure Mobile Engagementem pro aplikace pro Android</span><span class="sxs-lookup"><span data-stu-id="62897-103">Get started with Azure Mobile Engagement for Android apps</span></span>
[!INCLUDE [Hero tutorial switcher](../../includes/mobile-engagement-hero-tutorial-switcher.md)]

<span data-ttu-id="62897-104">Toto téma ukazuje, jak Azure Mobile Engagement toounderstand toouse používání aplikace a jak toosend nabízená oznámení uživatelům toosegmented Android aplikace.</span><span class="sxs-lookup"><span data-stu-id="62897-104">This topic shows you how toouse Azure Mobile Engagement toounderstand your app usage and how toosend push notifications toosegmented users of an Android application.</span></span>
<span data-ttu-id="62897-105">Tento kurz představuje scénář hello jednoduchého vysílání přes Mobile Engagement.</span><span class="sxs-lookup"><span data-stu-id="62897-105">This tutorial demonstrates hello simple broadcast scenario using Mobile Engagement.</span></span> <span data-ttu-id="62897-106">V něm můžete vytvořit prázdnou aplikaci pro Android, která sbírá základní data a dostává nabízená oznámení pomocí služby GCM (Google Cloud Messaging).</span><span class="sxs-lookup"><span data-stu-id="62897-106">In it, you create a blank Android app that collects basic data and receives push notifications using Google Cloud Messaging (GCM).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="62897-107">Požadavky</span><span class="sxs-lookup"><span data-stu-id="62897-107">Prerequisites</span></span>
<span data-ttu-id="62897-108">Dokončení tohoto kurzu vyžaduje hello [Android Developer Tools](https://developer.android.com/sdk/index.html), které zahrnují integrované vývojové prostředí Android Studio hello a nejnovější platformu Android hello.</span><span class="sxs-lookup"><span data-stu-id="62897-108">Completing this tutorial requires hello [Android Developer Tools](https://developer.android.com/sdk/index.html), which includes hello Android Studio integrated development environment, and hello latest Android platform.</span></span>

<span data-ttu-id="62897-109">Také budete potřebovat hello [Mobile Engagement Android SDK](https://aka.ms/vq9mfn).</span><span class="sxs-lookup"><span data-stu-id="62897-109">It also requires hello [Mobile Engagement Android SDK](https://aka.ms/vq9mfn).</span></span>

> [!IMPORTANT]
> <span data-ttu-id="62897-110">toocomplete tohoto kurzu potřebujete aktivní účet Azure.</span><span class="sxs-lookup"><span data-stu-id="62897-110">toocomplete this tutorial, you need an active Azure account.</span></span> <span data-ttu-id="62897-111">Pokud účet nemáte, můžete si během několika minut vytvořit bezplatný zkušební účet.</span><span class="sxs-lookup"><span data-stu-id="62897-111">If you don't have an account, you can create a free trial account in just a couple of minutes.</span></span> <span data-ttu-id="62897-112">Podrobnosti najdete v článku [Bezplatná zkušební verze Azure](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A0E0E5C02&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdocumentation%2Farticles%2Fmobile-engagement-android-get-started).</span><span class="sxs-lookup"><span data-stu-id="62897-112">For details, see [Azure Free Trial](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A0E0E5C02&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdocumentation%2Farticles%2Fmobile-engagement-android-get-started).</span></span>
>
>

## <a name="set-up-mobile-engagement-for-your-android-app"></a><span data-ttu-id="62897-113">Nastavení Mobile Engagementu pro vaši aplikaci pro Android</span><span class="sxs-lookup"><span data-stu-id="62897-113">Set up Mobile Engagement for your Android app</span></span>
[!INCLUDE [Create Mobile Engagement App in Portal](../../includes/mobile-engagement-create-app-in-portal-new.md)]

## <a name="connect-your-app-toohello-mobile-engagement-backend"></a><span data-ttu-id="62897-114">Připojit vaše aplikace toohello Mobile Engagement back-end</span><span class="sxs-lookup"><span data-stu-id="62897-114">Connect your app toohello Mobile Engagement backend</span></span>
<span data-ttu-id="62897-115">Tento kurzu si představíme "základní integraci", což je minimální hello nastavte požadované toocollect dat a odesílání nabízených oznámení.</span><span class="sxs-lookup"><span data-stu-id="62897-115">This tutorial presents a "basic integration", which is hello minimal set required toocollect data and send a push notification.</span></span> <span data-ttu-id="62897-116">Vytvoříte základní aplikaci s integrací hello toodemonstrate Android Studio.</span><span class="sxs-lookup"><span data-stu-id="62897-116">You create a basic app with Android Studio toodemonstrate hello integration.</span></span>

<span data-ttu-id="62897-117">Hello kompletní dokumentaci k integraci najdete v hello [integrace sady Mobile Engagement Android SDK](mobile-engagement-android-sdk-overview.md).</span><span class="sxs-lookup"><span data-stu-id="62897-117">hello complete integration documentation can be found in hello [Mobile Engagement Android SDK integration](mobile-engagement-android-sdk-overview.md).</span></span>

### <a name="create-an-android-project"></a><span data-ttu-id="62897-118">Vytvoření projektu Android</span><span class="sxs-lookup"><span data-stu-id="62897-118">Create an Android project</span></span>
1. <span data-ttu-id="62897-119">Spustit **Android Studio**a v místní nabídce hello vyberte **spuštění nového projektu Android Studio**.</span><span class="sxs-lookup"><span data-stu-id="62897-119">Start **Android Studio**, and in hello pop-up, select **Start a new Android Studio project**.</span></span>

    ![][1]
2. <span data-ttu-id="62897-120">Zadejte název aplikace a doménu firmy.</span><span class="sxs-lookup"><span data-stu-id="62897-120">Provide an app name and company domain.</span></span> <span data-ttu-id="62897-121">Poznamenejte si, co vyplňujete, protože budete tyto údaje potřebovat později.</span><span class="sxs-lookup"><span data-stu-id="62897-121">Make a note of what you are filling, because you need it later.</span></span> <span data-ttu-id="62897-122">Klikněte na **Další**.</span><span class="sxs-lookup"><span data-stu-id="62897-122">Click **Next**.</span></span>

    ![][2]
3. <span data-ttu-id="62897-123">Vyberte cíl formuláře hello a úroveň API a klikněte na **Další**.</span><span class="sxs-lookup"><span data-stu-id="62897-123">Select hello target form factor and API level, and click **Next**.</span></span>

   > [!NOTE]
   > <span data-ttu-id="62897-124">Mobile Engagement vyžaduje API minimálně úrovně 10 (Android 2.3.3).</span><span class="sxs-lookup"><span data-stu-id="62897-124">Mobile Engagement requires API level 10 minimum (Android 2.3.3).</span></span>
   >
   >

    ![][3]
4. <span data-ttu-id="62897-125">Vyberte **prázdné aktivity** tady, což je hello jedinou obrazovkou pro tuto aplikaci a klikněte na tlačítko **Další**.</span><span class="sxs-lookup"><span data-stu-id="62897-125">Select **Blank Activity** here, which is hello only screen for this app and click **Next**.</span></span>

    ![][4]
5. <span data-ttu-id="62897-126">Závěr ponechte hello výchozí nastavení a klikněte na tlačítko **Dokončit**.</span><span class="sxs-lookup"><span data-stu-id="62897-126">Finally, leave hello defaults as is and click **Finish**.</span></span>

    ![][5]

<span data-ttu-id="62897-127">Android Studio nyní vytvoří hello ukázkovou aplikaci, do které budeme integrovat Mobile Engagement.</span><span class="sxs-lookup"><span data-stu-id="62897-127">Android Studio now creates hello demo app into which we integrate Mobile Engagement.</span></span>

### <a name="include-hello-sdk-library-in-your-project"></a><span data-ttu-id="62897-128">Zahrnout hello knihovně SDK do projektu</span><span class="sxs-lookup"><span data-stu-id="62897-128">Include hello SDK library in your project</span></span>
1. <span data-ttu-id="62897-129">Stáhnout hello [Mobile Engagement Android SDK](https://aka.ms/vq9mfn).</span><span class="sxs-lookup"><span data-stu-id="62897-129">Download hello [Mobile Engagement Android SDK](https://aka.ms/vq9mfn).</span></span>
2. <span data-ttu-id="62897-130">Extrahujte hello archivu souboru tooa složky v počítači.</span><span class="sxs-lookup"><span data-stu-id="62897-130">Extract hello archive file tooa folder in your computer.</span></span>
3. <span data-ttu-id="62897-131">Identifikujte knihovnu .jar hello pro hello aktuální verzi této sady SDK a zkopírujte ho toohello schránky.</span><span class="sxs-lookup"><span data-stu-id="62897-131">Identify hello .jar library for hello current version of this SDK and copy it toohello Clipboard.</span></span>

      ![][6]
4. <span data-ttu-id="62897-132">Přejděte toohello **projektu** část (1) a vložte hello .jar hello složky libs (2).</span><span class="sxs-lookup"><span data-stu-id="62897-132">Navigate toohello **Project** section (1) and paste hello .jar in hello libs folder (2).</span></span>

      ![][7]
5. <span data-ttu-id="62897-133">tooload hello knihovně, synchronizace hello projektu.</span><span class="sxs-lookup"><span data-stu-id="62897-133">tooload hello library, sync hello project .</span></span>

      ![][8]

### <a name="connect-your-app-toomobile-engagement-backend-with-hello-connection-string"></a><span data-ttu-id="62897-134">Připojit vaše tooMobile Engagement back-end aplikace s hello připojovací řetězec</span><span class="sxs-lookup"><span data-stu-id="62897-134">Connect your app tooMobile Engagement backend with hello Connection String</span></span>
1. <span data-ttu-id="62897-135">Zkopírujte následující řádky kódu do vytváření aktivity hello (je třeba provést pouze na jednom místě aplikace, obvykle hello hlavní aktivitě) hello.</span><span class="sxs-lookup"><span data-stu-id="62897-135">Copy hello following lines of code into hello activity creation (must be done only in one place of your application, usually hello main activity).</span></span> <span data-ttu-id="62897-136">Této ukázkové aplikace, otevřete si hello MainActivity pod src -> main -> java složky a přidat následující hello:</span><span class="sxs-lookup"><span data-stu-id="62897-136">For this sample app, open up hello MainActivity under src -> main -> java folder and add hello following:</span></span>

        EngagementConfiguration engagementConfiguration = new EngagementConfiguration();
        engagementConfiguration.setConnectionString("Endpoint={appCollection}.{domain};AppId={appId};SdkKey={sdkKey}");
        EngagementAgent.getInstance(this).init(engagementConfiguration);
2. <span data-ttu-id="62897-137">Vyřešte odkazy na hello stisknutím Alt + Enter nebo přidáním hello následující příkazy pro import:</span><span class="sxs-lookup"><span data-stu-id="62897-137">Resolve hello references by pressing Alt + Enter or adding hello following import statements:</span></span>

        import com.microsoft.azure.engagement.EngagementAgent;
        import com.microsoft.azure.engagement.EngagementConfiguration;
3. <span data-ttu-id="62897-138">Přejděte zpět toohello portálu Azure Classic ve vaší aplikaci **informace o připojení** stránky a zkopírujte hello **připojovací řetězec**.</span><span class="sxs-lookup"><span data-stu-id="62897-138">Go back toohello Azure Classic Portal in your app's **Connection Info** page and copy hello **Connection String**.</span></span>

      ![][9]
4. <span data-ttu-id="62897-139">Vložte jej do hello `setConnectionString` parametr, nahraďte hello celý řetězec ukazuje hello následující kód:</span><span class="sxs-lookup"><span data-stu-id="62897-139">Paste it into hello `setConnectionString` parameter, replacing hello entire string shown in hello following code:</span></span>

        engagementConfiguration.setConnectionString("Endpoint=my-company-name.device.mobileengagement.windows.net;SdkKey=********************;AppId=*********");

### <a name="add-permissions-and-a-service-declaration"></a><span data-ttu-id="62897-140">Přidání oprávnění a deklarace služby</span><span class="sxs-lookup"><span data-stu-id="62897-140">Add permissions and a service declaration</span></span>
1. <span data-ttu-id="62897-141">Přidat tyto toohello oprávnění souboru Manifest.xml vašeho projektu těsně před nebo po hello `<application>` značky:</span><span class="sxs-lookup"><span data-stu-id="62897-141">Add these permissions toohello Manifest.xml of your project immediately before or after hello `<application>` tag:</span></span>

        <uses-permission android:name="android.permission.INTERNET"/>
        <uses-permission android:name="android.permission.ACCESS_NETWORK_STATE"/>
        <uses-permission android:name="android.permission.WRITE_EXTERNAL_STORAGE"/>
        <uses-permission android:name="android.permission.RECEIVE_BOOT_COMPLETED" />
        <uses-permission android:name="android.permission.VIBRATE" />
        <uses-permission android:name="android.permission.DOWNLOAD_WITHOUT_NOTIFICATION"/>
2. <span data-ttu-id="62897-142">toodeclare hello Služba agenta, přidejte tento kód mezi hello `<application>` a `</application>` značky:</span><span class="sxs-lookup"><span data-stu-id="62897-142">toodeclare hello agent service, add this code between hello `<application>` and `</application>` tags:</span></span>

        <service
             android:name="com.microsoft.azure.engagement.service.EngagementService"
             android:exported="false"
             android:label="<Your application name>"
             android:process=":Engagement"/>
3. <span data-ttu-id="62897-143">V kódu hello jste vložili, nahraďte `"<Your application name>"` v hello popisek, který se zobrazí v hello **nastavení** nabídky, kde uvidíte služby spuštěné na zařízení hello.</span><span class="sxs-lookup"><span data-stu-id="62897-143">In hello code you pasted, replace `"<Your application name>"` in hello label, which is displayed in hello **Settings** menu where you can see services running on hello device.</span></span> <span data-ttu-id="62897-144">Do tohoto popisku můžete přidat hello slovo "Service" třeba.</span><span class="sxs-lookup"><span data-stu-id="62897-144">You can add hello word "Service" in that label for example.</span></span>

### <a name="send-a-screen-toomobile-engagement"></a><span data-ttu-id="62897-145">Odeslání obrazovky tooMobile zapojení</span><span class="sxs-lookup"><span data-stu-id="62897-145">Send a screen tooMobile Engagement</span></span>
<span data-ttu-id="62897-146">odeslání dat toostart a zkontrolujte, zda text hello uživatelé jsou aktivní, musíte odeslat alespoň jednu obrazovku (aktivitu) toohello Mobile Engagement back-end.</span><span class="sxs-lookup"><span data-stu-id="62897-146">toostart sending data and ensure that hello users are active, you must send at least one screen (Activity) toohello Mobile Engagement backend.</span></span>

<span data-ttu-id="62897-147">Přejděte příliš**MainActivity.java** a přidejte následující základní třídu hello tooreplace hello **MainActivity** příliš**EngagementActivity**:</span><span class="sxs-lookup"><span data-stu-id="62897-147">Go too**MainActivity.java** and add hello following tooreplace hello base class of **MainActivity** too**EngagementActivity**:</span></span>

    public class MainActivity extends EngagementActivity {

> [!NOTE]
> <span data-ttu-id="62897-148">Pokud vaše základní třída není *aktivity*, poraďte se [Advanced hlášení pro Android](mobile-engagement-android-advanced-reporting.md) jak tooinherit z různých tříd.</span><span class="sxs-lookup"><span data-stu-id="62897-148">If your base class is not *Activity*, consult [Advanced Android Reporting](mobile-engagement-android-advanced-reporting.md) for how tooinherit from different classes.</span></span>
>
>

<span data-ttu-id="62897-149">Komentář hello následující řádek tohoto jednoduchého ukázkového scénáře:</span><span class="sxs-lookup"><span data-stu-id="62897-149">Comment out hello following line for this simple sample scenario:</span></span>

    // setSupportActionBar(toolbar);

<span data-ttu-id="62897-150">Pokud chcete, aby tookeep hello `ActionBar` ve vaší aplikaci, najdete v části [Advanced hlášení pro Android](mobile-engagement-android-advanced-reporting.md).</span><span class="sxs-lookup"><span data-stu-id="62897-150">If you want tookeep hello `ActionBar` in your app, see [Advanced Android Reporting](mobile-engagement-android-advanced-reporting.md).</span></span>

## <a name="connect-app-with-real-time-monitoring"></a><span data-ttu-id="62897-151">Připojení aplikace se sledováním v reálném čase</span><span class="sxs-lookup"><span data-stu-id="62897-151">Connect app with real-time monitoring</span></span>
[!INCLUDE [Connect app with real-time monitoring](../../includes/mobile-engagement-connect-app-with-monitor.md)]

## <a name="enable-push-notifications-and-in-app-messaging"></a><span data-ttu-id="62897-152">Povolení nabízených oznámení a zasílání zpráv v aplikaci</span><span class="sxs-lookup"><span data-stu-id="62897-152">Enable push notifications and in-app messaging</span></span>
<span data-ttu-id="62897-153">Během kampaně vám Mobile Engagement umožňuje interagovat a KOMUNIKOVAT s uživateli pomocí nabízených oznámení a zpráv v aplikaci.</span><span class="sxs-lookup"><span data-stu-id="62897-153">During a campaign, Mobile Engagement lets you interact with and REACH your users with push notifications and in-app messaging.</span></span> <span data-ttu-id="62897-154">Tento modul je hello portálu Mobile Engagement nazývá REACH.</span><span class="sxs-lookup"><span data-stu-id="62897-154">This module is called REACH in hello Mobile Engagement portal.</span></span>
<span data-ttu-id="62897-155">Následující části Hello nastaví tooreceive vaše aplikace je.</span><span class="sxs-lookup"><span data-stu-id="62897-155">hello following section sets up your app tooreceive them.</span></span>

### <a name="copy-sdk-resources-in-your-project"></a><span data-ttu-id="62897-156">Kopírování prostředků sady SDK do projektu</span><span class="sxs-lookup"><span data-stu-id="62897-156">Copy SDK resources in your project</span></span>
1. <span data-ttu-id="62897-157">Přejděte zpět tooyour SDK stáhnout obsah a zkopírujte hello **res** složky.</span><span class="sxs-lookup"><span data-stu-id="62897-157">Navigate back tooyour SDK download content and copy hello **res** folder.</span></span>

    ![][10]
2. <span data-ttu-id="62897-158">Přejděte zpět tooAndroid Studio, vyberte hello **hlavní** adresář souborů projektu a pak ji vložit tooadd hello prostředky tooyour projektu.</span><span class="sxs-lookup"><span data-stu-id="62897-158">Go back tooAndroid Studio, select hello **main** directory of your project files, and then paste it tooadd hello resources tooyour project.</span></span>

    ![][11]

[!INCLUDE [Enable Google Cloud Messaging](../../includes/mobile-engagement-enable-google-cloud-messaging.md)]

[!INCLUDE [Enable in-app messaging](../../includes/mobile-engagement-android-send-push.md)]

[!INCLUDE [Send notification from portal](../../includes/mobile-engagement-android-send-push-from-portal.md)]

## <a name="next-steps"></a><span data-ttu-id="62897-159">Další kroky</span><span class="sxs-lookup"><span data-stu-id="62897-159">Next steps</span></span>
<span data-ttu-id="62897-160">Přejděte příliš[sady SDK pro Android](mobile-engagement-android-sdk-overview.md) tooget podrobné informace o hello integraci sady SDK.</span><span class="sxs-lookup"><span data-stu-id="62897-160">Go too[Android SDK](mobile-engagement-android-sdk-overview.md) tooget detailed knowledge about hello SDK integration.</span></span>

<!-- Images. -->
[1]: ./media/mobile-engagement-android-get-started/android-studio-new-project.png
[2]: ./media/mobile-engagement-android-get-started/android-studio-project-props.png
[3]: ./media/mobile-engagement-android-get-started/android-studio-project-props2.png
[4]: ./media/mobile-engagement-android-get-started/android-studio-add-activity.png
[5]: ./media/mobile-engagement-android-get-started/android-studio-activity-name.png
[6]: ./media/mobile-engagement-android-get-started/sdk-content.png
[7]: ./media/mobile-engagement-android-get-started/paste-jar.png
[8]: ./media/mobile-engagement-android-get-started/sync-project.png
[9]: ./media/mobile-engagement-android-get-started/app-connection-info-page.png
[10]: ./media/mobile-engagement-android-get-started/copy-resources.png
[11]: ./media/mobile-engagement-android-get-started/paste-resources.png
