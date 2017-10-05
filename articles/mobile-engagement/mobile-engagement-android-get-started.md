---
title: "Začínáme s Android Apps Azure Mobile Engagementem"
description: "Naučte se používat Azure Mobile Engagement s analýzou a nabízenými oznámeními pro aplikace pro Android."
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
ms.openlocfilehash: dc255a930bf71e6ef6d964bc5e3472a38ce4e467
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="get-started-with-azure-mobile-engagement-for-android-apps"></a><span data-ttu-id="96ef0-103">Začínáme s Azure Mobile Engagementem pro aplikace pro Android</span><span class="sxs-lookup"><span data-stu-id="96ef0-103">Get started with Azure Mobile Engagement for Android apps</span></span>
[!INCLUDE [Hero tutorial switcher](../../includes/mobile-engagement-hero-tutorial-switcher.md)]

<span data-ttu-id="96ef0-104">V tomto tématu si ukážeme, jak používat Azure Mobile Engagement, jak porozumět používání aplikací a odesílat nabízená oznámení segmentovaným uživatelům aplikace pro Android.</span><span class="sxs-lookup"><span data-stu-id="96ef0-104">This topic shows you how to use Azure Mobile Engagement to understand your app usage and how to send push notifications to segmented users of an Android application.</span></span>
<span data-ttu-id="96ef0-105">Tento kurz představuje scénář jednoduchého vysílání přes Mobile Engagement.</span><span class="sxs-lookup"><span data-stu-id="96ef0-105">This tutorial demonstrates the simple broadcast scenario using Mobile Engagement.</span></span> <span data-ttu-id="96ef0-106">V něm můžete vytvořit prázdnou aplikaci pro Android, která sbírá základní data a dostává nabízená oznámení pomocí služby GCM (Google Cloud Messaging).</span><span class="sxs-lookup"><span data-stu-id="96ef0-106">In it, you create a blank Android app that collects basic data and receives push notifications using Google Cloud Messaging (GCM).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="96ef0-107">Požadavky</span><span class="sxs-lookup"><span data-stu-id="96ef0-107">Prerequisites</span></span>
<span data-ttu-id="96ef0-108">K dokončení tohoto kurzu budete potřebovat [vývojářské nástroje pro Android](https://developer.android.com/sdk/index.html), které zahrnují integrované vývojové prostředí Android Studio a nejnovější platformu Android.</span><span class="sxs-lookup"><span data-stu-id="96ef0-108">Completing this tutorial requires the [Android Developer Tools](https://developer.android.com/sdk/index.html), which includes the Android Studio integrated development environment, and the latest Android platform.</span></span>

<span data-ttu-id="96ef0-109">Také budete potřebovat sadu [Mobile Engagement Android SDK](https://aka.ms/vq9mfn).</span><span class="sxs-lookup"><span data-stu-id="96ef0-109">It also requires the [Mobile Engagement Android SDK](https://aka.ms/vq9mfn).</span></span>

> [!IMPORTANT]
> <span data-ttu-id="96ef0-110">K dokončení tohoto kurzu potřebujete mít aktivní účet Azure.</span><span class="sxs-lookup"><span data-stu-id="96ef0-110">To complete this tutorial, you need an active Azure account.</span></span> <span data-ttu-id="96ef0-111">Pokud účet nemáte, můžete si během několika minut vytvořit bezplatný zkušební účet.</span><span class="sxs-lookup"><span data-stu-id="96ef0-111">If you don't have an account, you can create a free trial account in just a couple of minutes.</span></span> <span data-ttu-id="96ef0-112">Podrobnosti najdete v článku [Bezplatná zkušební verze Azure](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A0E0E5C02&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdocumentation%2Farticles%2Fmobile-engagement-android-get-started).</span><span class="sxs-lookup"><span data-stu-id="96ef0-112">For details, see [Azure Free Trial](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A0E0E5C02&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdocumentation%2Farticles%2Fmobile-engagement-android-get-started).</span></span>
>
>

## <a name="set-up-mobile-engagement-for-your-android-app"></a><span data-ttu-id="96ef0-113">Nastavení Mobile Engagementu pro vaši aplikaci pro Android</span><span class="sxs-lookup"><span data-stu-id="96ef0-113">Set up Mobile Engagement for your Android app</span></span>
[!INCLUDE [Create Mobile Engagement App in Portal](../../includes/mobile-engagement-create-app-in-portal-new.md)]

## <a name="connect-your-app-to-the-mobile-engagement-backend"></a><span data-ttu-id="96ef0-114">Připojení aplikace k back-endu Mobile Engagementu</span><span class="sxs-lookup"><span data-stu-id="96ef0-114">Connect your app to the Mobile Engagement backend</span></span>
<span data-ttu-id="96ef0-115">V tomto kurzu si představíme „základní integraci“, čili minimální sadu požadovanou pro shromažďování dat a odesílání nabízených oznámení.</span><span class="sxs-lookup"><span data-stu-id="96ef0-115">This tutorial presents a "basic integration", which is the minimal set required to collect data and send a push notification.</span></span> <span data-ttu-id="96ef0-116">Pomocí Android Studia vytvoříte základní aplikaci, na které si tuto integraci předvedeme.</span><span class="sxs-lookup"><span data-stu-id="96ef0-116">You create a basic app with Android Studio to demonstrate the integration.</span></span>

<span data-ttu-id="96ef0-117">Kompletní dokumentaci k integraci najdete v článku [Integrace sady Mobile Engagement Android SDK](mobile-engagement-android-sdk-overview.md).</span><span class="sxs-lookup"><span data-stu-id="96ef0-117">The complete integration documentation can be found in the [Mobile Engagement Android SDK integration](mobile-engagement-android-sdk-overview.md).</span></span>

### <a name="create-an-android-project"></a><span data-ttu-id="96ef0-118">Vytvoření projektu Android</span><span class="sxs-lookup"><span data-stu-id="96ef0-118">Create an Android project</span></span>
1. <span data-ttu-id="96ef0-119">Spusťte **Android Studio** a v místní nabídce vyberte **Start a new Android Studio project** (Začít nový projekt Android Studio).</span><span class="sxs-lookup"><span data-stu-id="96ef0-119">Start **Android Studio**, and in the pop-up, select **Start a new Android Studio project**.</span></span>

    ![][1]
2. <span data-ttu-id="96ef0-120">Zadejte název aplikace a doménu firmy.</span><span class="sxs-lookup"><span data-stu-id="96ef0-120">Provide an app name and company domain.</span></span> <span data-ttu-id="96ef0-121">Poznamenejte si, co vyplňujete, protože budete tyto údaje potřebovat později.</span><span class="sxs-lookup"><span data-stu-id="96ef0-121">Make a note of what you are filling, because you need it later.</span></span> <span data-ttu-id="96ef0-122">Klikněte na **Další**.</span><span class="sxs-lookup"><span data-stu-id="96ef0-122">Click **Next**.</span></span>

    ![][2]
3. <span data-ttu-id="96ef0-123">Zvolte cílový typ zařízení a úroveň API a klikněte na**Next**.</span><span class="sxs-lookup"><span data-stu-id="96ef0-123">Select the target form factor and API level, and click **Next**.</span></span>

   > [!NOTE]
   > <span data-ttu-id="96ef0-124">Mobile Engagement vyžaduje API minimálně úrovně 10 (Android 2.3.3).</span><span class="sxs-lookup"><span data-stu-id="96ef0-124">Mobile Engagement requires API level 10 minimum (Android 2.3.3).</span></span>
   >
   >

    ![][3]
4. <span data-ttu-id="96ef0-125">Zde vyberte **Blank Activity** (Prázdná aktivita), která je jedinou obrazovkou pro tuto aplikaci, a klikněte na **Next**.</span><span class="sxs-lookup"><span data-stu-id="96ef0-125">Select **Blank Activity** here, which is the only screen for this app and click **Next**.</span></span>

    ![][4]
5. <span data-ttu-id="96ef0-126">Na závěr ponechte výchozí nastavení a klikněte na**Finish** (Dokončit).</span><span class="sxs-lookup"><span data-stu-id="96ef0-126">Finally, leave the defaults as is and click **Finish**.</span></span>

    ![][5]

<span data-ttu-id="96ef0-127">Android Studio nyní vytvoří ukázkovou aplikaci, do které integrujeme Mobile Engagement.</span><span class="sxs-lookup"><span data-stu-id="96ef0-127">Android Studio now creates the demo app into which we integrate Mobile Engagement.</span></span>

### <a name="include-the-sdk-library-in-your-project"></a><span data-ttu-id="96ef0-128">Zahrnutí knihovny sady SDK do projektu</span><span class="sxs-lookup"><span data-stu-id="96ef0-128">Include the SDK library in your project</span></span>
1. <span data-ttu-id="96ef0-129">Stáhněte sadu [Mobile Engagement Android SDK](https://aka.ms/vq9mfn).</span><span class="sxs-lookup"><span data-stu-id="96ef0-129">Download the [Mobile Engagement Android SDK](https://aka.ms/vq9mfn).</span></span>
2. <span data-ttu-id="96ef0-130">Extrahujte soubor archivu do složky v počítači.</span><span class="sxs-lookup"><span data-stu-id="96ef0-130">Extract the archive file to a folder in your computer.</span></span>
3. <span data-ttu-id="96ef0-131">Identifikujte knihovnu .jar pro aktuální verzi této sady SDK a zkopírujte ji do schránky.</span><span class="sxs-lookup"><span data-stu-id="96ef0-131">Identify the .jar library for the current version of this SDK and copy it to the Clipboard.</span></span>

      ![][6]
4. <span data-ttu-id="96ef0-132">Přejděte do sekce **Project** (Projekt) (1) a vložte .sloubor jar do složky libs (2).</span><span class="sxs-lookup"><span data-stu-id="96ef0-132">Navigate to the **Project** section (1) and paste the .jar in the libs folder (2).</span></span>

      ![][7]
5. <span data-ttu-id="96ef0-133">Pro načtení knihovny synchronizujte projekt.</span><span class="sxs-lookup"><span data-stu-id="96ef0-133">To load the library, sync the project .</span></span>

      ![][8]

### <a name="connect-your-app-to-mobile-engagement-backend-with-the-connection-string"></a><span data-ttu-id="96ef0-134">Připojení aplikace k back-endu Mobile Engagementu pomocí připojovacího řetězce</span><span class="sxs-lookup"><span data-stu-id="96ef0-134">Connect your app to Mobile Engagement backend with the Connection String</span></span>
1. <span data-ttu-id="96ef0-135">Zkopírujte následující řádky kódu do vytváření aktivity (je třeba provést pouze na jednom místě aplikace, obvykle v hlavní aktivitě).</span><span class="sxs-lookup"><span data-stu-id="96ef0-135">Copy the following lines of code into the activity creation (must be done only in one place of your application, usually the main activity).</span></span> <span data-ttu-id="96ef0-136">V případě této ukázkové aplikace otevřete MainActivity pod složkou src -> main -> java a přidejte následující:</span><span class="sxs-lookup"><span data-stu-id="96ef0-136">For this sample app, open up the MainActivity under src -> main -> java folder and add the following:</span></span>

        EngagementConfiguration engagementConfiguration = new EngagementConfiguration();
        engagementConfiguration.setConnectionString("Endpoint={appCollection}.{domain};AppId={appId};SdkKey={sdkKey}");
        EngagementAgent.getInstance(this).init(engagementConfiguration);
2. <span data-ttu-id="96ef0-137">Vyřešte reference stisknutím Alt+Enter nebo přidáním následujících příkazů pro import:</span><span class="sxs-lookup"><span data-stu-id="96ef0-137">Resolve the references by pressing Alt + Enter or adding the following import statements:</span></span>

        import com.microsoft.azure.engagement.EngagementAgent;
        import com.microsoft.azure.engagement.EngagementConfiguration;
3. <span data-ttu-id="96ef0-138">Přejděte zpět na Azure Classic Portal na stránce **Connection Info** (Informace o připojení) vaší aplikace a zkopírujte obsah pole **Connection String** (Připojovací řetězec).</span><span class="sxs-lookup"><span data-stu-id="96ef0-138">Go back to the Azure Classic Portal in your app's **Connection Info** page and copy the **Connection String**.</span></span>

      ![][9]
4. <span data-ttu-id="96ef0-139">Vložte jej do parametru `setConnectionString` a nahraďte celý zobrazený řetězec následujícím kódem:</span><span class="sxs-lookup"><span data-stu-id="96ef0-139">Paste it into the `setConnectionString` parameter, replacing the entire string shown in the following code:</span></span>

        engagementConfiguration.setConnectionString("Endpoint=my-company-name.device.mobileengagement.windows.net;SdkKey=********************;AppId=*********");

### <a name="add-permissions-and-a-service-declaration"></a><span data-ttu-id="96ef0-140">Přidání oprávnění a deklarace služby</span><span class="sxs-lookup"><span data-stu-id="96ef0-140">Add permissions and a service declaration</span></span>
1. <span data-ttu-id="96ef0-141">Přidejte oprávnění do souboru Manifest.xml vašeho projektu těsně před značku `<application>` nebo za ni.</span><span class="sxs-lookup"><span data-stu-id="96ef0-141">Add these permissions to the Manifest.xml of your project immediately before or after the `<application>` tag:</span></span>

        <uses-permission android:name="android.permission.INTERNET"/>
        <uses-permission android:name="android.permission.ACCESS_NETWORK_STATE"/>
        <uses-permission android:name="android.permission.WRITE_EXTERNAL_STORAGE"/>
        <uses-permission android:name="android.permission.RECEIVE_BOOT_COMPLETED" />
        <uses-permission android:name="android.permission.VIBRATE" />
        <uses-permission android:name="android.permission.DOWNLOAD_WITHOUT_NOTIFICATION"/>
2. <span data-ttu-id="96ef0-142">Chcete-li deklarovat službu agenta, přidejte tento kód mezi značky `<application>` a `</application>`:</span><span class="sxs-lookup"><span data-stu-id="96ef0-142">To declare the agent service, add this code between the `<application>` and `</application>` tags:</span></span>

        <service
             android:name="com.microsoft.azure.engagement.service.EngagementService"
             android:exported="false"
             android:label="<Your application name>"
             android:process=":Engagement"/>
3. <span data-ttu-id="96ef0-143">Ve vloženém kódu nahraďte `"<Your application name>"` v popisku, který se zobrazí v nabídce **Nastavení**, kde můžete zobrazit služeb spuštěné v zařízení.</span><span class="sxs-lookup"><span data-stu-id="96ef0-143">In the code you pasted, replace `"<Your application name>"` in the label, which is displayed in the **Settings** menu where you can see services running on the device.</span></span> <span data-ttu-id="96ef0-144">Do tohoto popisku můžete například přidat slovo „Service“.</span><span class="sxs-lookup"><span data-stu-id="96ef0-144">You can add the word "Service" in that label for example.</span></span>

### <a name="send-a-screen-to-mobile-engagement"></a><span data-ttu-id="96ef0-145">Odeslání obrazovky do Mobile Engagementu</span><span class="sxs-lookup"><span data-stu-id="96ef0-145">Send a screen to Mobile Engagement</span></span>
<span data-ttu-id="96ef0-146">Pokud chcete začít odesílat data a zajistit, že uživatelé jsou aktivní, musíte odeslat alespoň jednu obrazovku (aktivitu) na back-end Mobile Engagementu.</span><span class="sxs-lookup"><span data-stu-id="96ef0-146">To start sending data and ensure that the users are active, you must send at least one screen (Activity) to the Mobile Engagement backend.</span></span>

<span data-ttu-id="96ef0-147">Přejděte na **MainActivity.java** a přidejte následující kód, kterým nahradíte základní třídu **MainActivity** třídou **EngagementActivity**:</span><span class="sxs-lookup"><span data-stu-id="96ef0-147">Go to **MainActivity.java** and add the following to replace the base class of **MainActivity** to **EngagementActivity**:</span></span>

    public class MainActivity extends EngagementActivity {

> [!NOTE]
> <span data-ttu-id="96ef0-148">Pokud vaše základní třída není *Activity*, podívejte se do [Rozšířených možností hlášení pro Android](mobile-engagement-android-advanced-reporting.md) na postupy, jak dědit z různých tříd.</span><span class="sxs-lookup"><span data-stu-id="96ef0-148">If your base class is not *Activity*, consult [Advanced Android Reporting](mobile-engagement-android-advanced-reporting.md) for how to inherit from different classes.</span></span>
>
>

<span data-ttu-id="96ef0-149">Okomentujte následující řádek pro tento jednoduchý vzorový scénář:</span><span class="sxs-lookup"><span data-stu-id="96ef0-149">Comment out the following line for this simple sample scenario:</span></span>

    // setSupportActionBar(toolbar);

<span data-ttu-id="96ef0-150">Pokud chcete zachovat `ActionBar` ve vaší aplikaci, zobrazte si část [Rozšířená sestava Android](mobile-engagement-android-advanced-reporting.md).</span><span class="sxs-lookup"><span data-stu-id="96ef0-150">If you want to keep the `ActionBar` in your app, see [Advanced Android Reporting](mobile-engagement-android-advanced-reporting.md).</span></span>

## <a name="connect-app-with-real-time-monitoring"></a><span data-ttu-id="96ef0-151">Připojení aplikace se sledováním v reálném čase</span><span class="sxs-lookup"><span data-stu-id="96ef0-151">Connect app with real-time monitoring</span></span>
[!INCLUDE [Connect app with real-time monitoring](../../includes/mobile-engagement-connect-app-with-monitor.md)]

## <a name="enable-push-notifications-and-in-app-messaging"></a><span data-ttu-id="96ef0-152">Povolení nabízených oznámení a zasílání zpráv v aplikaci</span><span class="sxs-lookup"><span data-stu-id="96ef0-152">Enable push notifications and in-app messaging</span></span>
<span data-ttu-id="96ef0-153">Během kampaně vám Mobile Engagement umožňuje interagovat a KOMUNIKOVAT s uživateli pomocí nabízených oznámení a zpráv v aplikaci.</span><span class="sxs-lookup"><span data-stu-id="96ef0-153">During a campaign, Mobile Engagement lets you interact with and REACH your users with push notifications and in-app messaging.</span></span> <span data-ttu-id="96ef0-154">Tento modul se na portálu Mobile Engagement nazývá REACH.</span><span class="sxs-lookup"><span data-stu-id="96ef0-154">This module is called REACH in the Mobile Engagement portal.</span></span>
<span data-ttu-id="96ef0-155">V následujících sekcích nastavíte aplikaci, aby tato nabízená oznámení a zprávy přijímala.</span><span class="sxs-lookup"><span data-stu-id="96ef0-155">The following section sets up your app to receive them.</span></span>

### <a name="copy-sdk-resources-in-your-project"></a><span data-ttu-id="96ef0-156">Kopírování prostředků sady SDK do projektu</span><span class="sxs-lookup"><span data-stu-id="96ef0-156">Copy SDK resources in your project</span></span>
1. <span data-ttu-id="96ef0-157">Přejděte zpět na obsah stažené sady SDK a zkopírujte složku **res**.</span><span class="sxs-lookup"><span data-stu-id="96ef0-157">Navigate back to your SDK download content and copy the **res** folder.</span></span>

    ![][10]
2. <span data-ttu-id="96ef0-158">Přejděte zpět na Android Studio, vyberte adresář **main** projektu a vložte do něj zkopírovanou složku res, abyste prostředky přidali do projektu.</span><span class="sxs-lookup"><span data-stu-id="96ef0-158">Go back to Android Studio, select the **main** directory of your project files, and then paste it to add the resources to your project.</span></span>

    ![][11]

[!INCLUDE [Enable Google Cloud Messaging](../../includes/mobile-engagement-enable-google-cloud-messaging.md)]

[!INCLUDE [Enable in-app messaging](../../includes/mobile-engagement-android-send-push.md)]

[!INCLUDE [Send notification from portal](../../includes/mobile-engagement-android-send-push-from-portal.md)]

## <a name="next-steps"></a><span data-ttu-id="96ef0-159">Další kroky</span><span class="sxs-lookup"><span data-stu-id="96ef0-159">Next steps</span></span>
<span data-ttu-id="96ef0-160">Podrobné informace o integraci sady SDK najdete v článku o integraci sady [Android SDK](mobile-engagement-android-sdk-overview.md).</span><span class="sxs-lookup"><span data-stu-id="96ef0-160">Go to [Android SDK](mobile-engagement-android-sdk-overview.md) to get detailed knowledge about the SDK integration.</span></span>

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
