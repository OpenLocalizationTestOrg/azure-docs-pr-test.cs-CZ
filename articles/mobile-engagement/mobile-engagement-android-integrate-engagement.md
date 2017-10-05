---
title: Integraci sady Azure Mobile Engagement Android SDK
description: "Nejnovější aktualizace a postupy pro Android SDK pro Azure Mobile Engagement"
services: mobile-engagement
documentationcenter: mobile
author: piyushjo
manager: erikre
editor: 
ms.assetid: a5487793-1a12-4f6c-a1cf-587c5a671e6b
ms.service: mobile-engagement
ms.workload: mobile
ms.tgt_pltfrm: mobile-android
ms.devlang: Java
ms.topic: article
ms.date: 08/19/2016
ms.author: piyushjo
ms.openlocfilehash: 35bd92e52b7a02f58620a03156902f9f91be57ae
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="how-to-integrate-engagement-on-android"></a><span data-ttu-id="923e0-103">Postup při integraci Engagement v systému Android</span><span class="sxs-lookup"><span data-stu-id="923e0-103">How to Integrate Engagement on Android</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="923e0-104">Univerzální pro Windows</span><span class="sxs-lookup"><span data-stu-id="923e0-104">Windows Universal</span></span>](mobile-engagement-windows-store-integrate-engagement.md)
> * [<span data-ttu-id="923e0-105">Windows Phone Silverlight</span><span class="sxs-lookup"><span data-stu-id="923e0-105">Windows Phone Silverlight</span></span>](mobile-engagement-windows-phone-integrate-engagement.md)
> * [<span data-ttu-id="923e0-106">iOS</span><span class="sxs-lookup"><span data-stu-id="923e0-106">iOS</span></span>](mobile-engagement-ios-integrate-engagement.md)
> * [<span data-ttu-id="923e0-107">Android</span><span class="sxs-lookup"><span data-stu-id="923e0-107">Android</span></span>](mobile-engagement-android-integrate-engagement.md)
> 
> 

<span data-ttu-id="923e0-108">Tento postup popisuje nejjednodušší způsob, jak aktivovat Engagement analýzy a monitorování funkce ve vaší aplikace Android.</span><span class="sxs-lookup"><span data-stu-id="923e0-108">This procedure describes the simplest way to activate Engagement's Analytics and Monitoring functions in your Android application.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="923e0-109">Vaše minimální úroveň rozhraní API systému Android SDK musí být 10 nebo vyšší (Android 2.3.3 nebo vyšší).</span><span class="sxs-lookup"><span data-stu-id="923e0-109">Your minimum Android SDK API level must be 10 or higher (Android 2.3.3 or higher).</span></span>
> 
> 

<span data-ttu-id="923e0-110">Následující kroky jsou dost aktivuje sestavy protokolů, které jsou potřebné k výpočtu všechny statistické údaje týkající se uživatelů, relací, aktivity, dojde k chybě a Technicals.</span><span class="sxs-lookup"><span data-stu-id="923e0-110">The following steps are enough to activates the report of logs needed to compute all statistics regarding Users, Sessions, Activities, Crashes and Technicals.</span></span> <span data-ttu-id="923e0-111">Sestava protokolů, které jsou potřebné k výpočtu další statistiky, jako jsou události a chyby úlohy je třeba provést ručně pomocí rozhraní API Engagement (najdete v části [jak používat rozšířené Mobile Engagement označování rozhraní API v systémem Android](mobile-engagement-android-use-engagement-api.md) vzhledem k tomu, že tyto statistické údaje jsou závislé aplikace.</span><span class="sxs-lookup"><span data-stu-id="923e0-111">The report of logs needed to compute other statistics like Events, Errors and Jobs must be done manually using the Engagement API (see [How to use the advanced Mobile Engagement tagging API in your Android](mobile-engagement-android-use-engagement-api.md) since these statistics are application dependent.</span></span>

## <a name="embed-the-engagement-sdk-and-service-into-your-android-project"></a><span data-ttu-id="923e0-112">Vložení Engagement SDK a služby do vašeho projektu Android</span><span class="sxs-lookup"><span data-stu-id="923e0-112">Embed the Engagement SDK and service into your Android project</span></span>
<span data-ttu-id="923e0-113">Stažení sady SDK pro Android z [sem](https://aka.ms/vq9mfn) získat `mobile-engagement-VERSION.jar` a umístí je do `libs` složky vašeho projektu Android (vytvořit složku knihovny, pokud neexistuje ještě).</span><span class="sxs-lookup"><span data-stu-id="923e0-113">Download the Android SDK from [here](https://aka.ms/vq9mfn) Get `mobile-engagement-VERSION.jar` and put them into the `libs` folder of your Android project (create the libs folder if it does not exist yet).</span></span>

> [!IMPORTANT]
> <span data-ttu-id="923e0-114">Pokud vytvoříte balíčku aplikace s ProGuard, budete muset zachovat některé třídy.</span><span class="sxs-lookup"><span data-stu-id="923e0-114">If you build your application package with ProGuard, you need to keep some classes.</span></span> <span data-ttu-id="923e0-115">Můžete použít následující fragment kódu konfigurace:</span><span class="sxs-lookup"><span data-stu-id="923e0-115">You can use the following configuration snippet:</span></span>
> 
> <span data-ttu-id="923e0-116">-Udržovat veřejná třída * rozšiřuje android.os.IInterface-zachovat {com.microsoft.azure.engagement.reach.activity.EngagementWebAnnouncementActivity$EngagementReachContentJS – třída</span><span class="sxs-lookup"><span data-stu-id="923e0-116">-keep public class * extends android.os.IInterface -keep class com.microsoft.azure.engagement.reach.activity.EngagementWebAnnouncementActivity$EngagementReachContentJS {</span></span>
> 
> <span data-ttu-id="923e0-117"><methods>; }</span><span class="sxs-lookup"><span data-stu-id="923e0-117"><methods>; }</span></span>
> 
> 

<span data-ttu-id="923e0-118">Zadejte připojovací řetězec Engagement voláním následující metodu v rámci Spouštěče aktivity:</span><span class="sxs-lookup"><span data-stu-id="923e0-118">Specify your Engagement connection string by calling the following method in the launcher activity:</span></span>

            EngagementConfiguration engagementConfiguration = new EngagementConfiguration();
            engagementConfiguration.setConnectionString("Endpoint={appCollection}.{domain};AppId={appId};SdkKey={sdkKey}");
            EngagementAgent.getInstance(this).init(engagementConfiguration);

<span data-ttu-id="923e0-119">Připojovací řetězec pro vaši aplikaci se zobrazí na portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="923e0-119">The connection string for your application is displayed on Azure Portal.</span></span>

* <span data-ttu-id="923e0-120">Pokud chybí, přidejte následující oprávnění Android (před `<application>` značka):</span><span class="sxs-lookup"><span data-stu-id="923e0-120">If missing, add the following Android permissions (before the `<application>` tag):</span></span>
  
          <uses-permission android:name="android.permission.INTERNET"/>
          <uses-permission android:name="android.permission.ACCESS_NETWORK_STATE"/>
* <span data-ttu-id="923e0-121">Přidejte následující část (mezi `<application>` a `</application>` značky):</span><span class="sxs-lookup"><span data-stu-id="923e0-121">Add the following section (between the `<application>` and `</application>` tags):</span></span>
  
          <service
            android:name="com.microsoft.azure.engagement.service.EngagementService"
            android:exported="false"
            android:label="<Your application name>Service"
            android:process=":Engagement"/>
* <span data-ttu-id="923e0-122">Změna `<Your application name>` podle názvu aplikace.</span><span class="sxs-lookup"><span data-stu-id="923e0-122">Change `<Your application name>` by the name of your application.</span></span>

> [!TIP]
> <span data-ttu-id="923e0-123">`android:label` Atribut umožňuje zvolit název služby Engagement, jak ho uvidí koncoví uživatelé na obrazovce "Spuštění služby" svůj telefon.</span><span class="sxs-lookup"><span data-stu-id="923e0-123">The `android:label` attribute allows you to choose the name of the Engagement service as it will appear to the end-users in the "Running services" screen of their phone.</span></span> <span data-ttu-id="923e0-124">Doporučuje se nastavit tento atribut `"<Your application name>Service"` (například `"AcmeFunGameService"`).</span><span class="sxs-lookup"><span data-stu-id="923e0-124">It is recommended to set this attribute to `"<Your application name>Service"` (e.g. `"AcmeFunGameService"`).</span></span>
> 
> 

<span data-ttu-id="923e0-125">Určení `android:process` atribut zajistí, že Engagement služba bude spuštěna ve svém vlastním procesu (systémem Engagement v rámci jednoho procesu jako vaše aplikace bude vaše vlákna main/uživatelského rozhraní potenciálně pomaleji).</span><span class="sxs-lookup"><span data-stu-id="923e0-125">Specifying the `android:process` attribute ensures that the Engagement service will run in its own process (running Engagement in the same process as your application will make your main/UI thread potentially less responsive).</span></span>

> [!NOTE]
> <span data-ttu-id="923e0-126">Kód, umístěte do `Application.onCreate()` a dalších zpětná volání aplikace bude spuštěna pro procesy všechny aplikace, včetně služby zapojení.</span><span class="sxs-lookup"><span data-stu-id="923e0-126">Any code you place in `Application.onCreate()` and other application callbacks will be run for all your application's processes, including the Engagement service.</span></span> <span data-ttu-id="923e0-127">Může mít nežádoucí vedlejší efekty (jako přiřazení nepotřebné paměti a vláken v procesu zapojení, služby nebo duplicitní všesměrového vysílání příjemci).</span><span class="sxs-lookup"><span data-stu-id="923e0-127">It may have unwanted side effects (like unneeded memory allocations and threads in the Engagement's process, duplicate broadcast receivers or services).</span></span>
> 
> 

<span data-ttu-id="923e0-128">Pokud přepíšete `Application.onCreate()`, se doporučuje přidejte následující fragment kódu na začátku vaší `Application.onCreate()` funkce:</span><span class="sxs-lookup"><span data-stu-id="923e0-128">If you override `Application.onCreate()`, it's recommended to add the following code snippet at the beginning of your `Application.onCreate()` function:</span></span>

             public void onCreate()
             {
               if (EngagementAgentUtils.isInDedicatedEngagementProcess(this))
                 return;

               ... Your code...
             }

<span data-ttu-id="923e0-129">Může stejnou věc udělat `Application.onTerminate()`, `Application.onLowMemory()` a `Application.onConfigurationChanged(...)`.</span><span class="sxs-lookup"><span data-stu-id="923e0-129">You can do the same thing for `Application.onTerminate()`, `Application.onLowMemory()` and `Application.onConfigurationChanged(...)`.</span></span>

<span data-ttu-id="923e0-130">Můžete také rozšířit `EngagementApplication` místo rozšíření `Application`: zpětné volání `Application.onCreate()` nemá procesu kontroly a volání `Application.onApplicationProcessCreate()` jen pokud aktuální proces není ta, který je hostitelem služby zapojení, stejná pravidla platit pro ostatní zpětných volání.</span><span class="sxs-lookup"><span data-stu-id="923e0-130">You can also extend `EngagementApplication` instead of extending `Application`: the callback `Application.onCreate()` does the process check and calls `Application.onApplicationProcessCreate()` only if the current process is not the one hosting the Engagement service, the same rules apply for the other callbacks.</span></span>

## <a name="basic-reporting"></a><span data-ttu-id="923e0-131">Vytváření základních sestav</span><span class="sxs-lookup"><span data-stu-id="923e0-131">Basic reporting</span></span>
### <a name="recommended-method-overload-your-activity-classes"></a><span data-ttu-id="923e0-132">Doporučená metoda: přetížení vaší `Activity` třídy</span><span class="sxs-lookup"><span data-stu-id="923e0-132">Recommended method: overload your `Activity` classes</span></span>
<span data-ttu-id="923e0-133">Chcete-li aktivovat sestavu všechny protokoly, které vyžadují zapojení k výpočtu uživatelů, relací, aktivity, dojde k chybě a technické statistiky, stejně musíte provádět všechny vaše `*Activity` dílčí třídy dědí odpovídající `Engagement*Activity` třídy (například pokud vaše aktivita starší verze rozšiřuje `ListActivity`, zkontrolujte ji rozšiřuje `EngagementListActivity`).</span><span class="sxs-lookup"><span data-stu-id="923e0-133">In order to activate the report of all the logs required by Engagement to compute Users, Sessions, Activities, Crashes and Technical statistics, you just have to make all your `*Activity` sub-classes inherit from the corresponding `Engagement*Activity` classes (e.g. if your legacy activity extends `ListActivity`, make it extends `EngagementListActivity`).</span></span>

<span data-ttu-id="923e0-134">**Bez Engagement:**</span><span class="sxs-lookup"><span data-stu-id="923e0-134">**Without Engagement :**</span></span>

            package com.company.myapp;

            import android.app.Activity;
            import android.os.Bundle;

            public class MyApp extends Activity
            {
              @Override
              public void onCreate(Bundle savedInstanceState)
              {
                super.onCreate(savedInstanceState);
                setContentView(R.layout.main);
              }
            }

<span data-ttu-id="923e0-135">**S Engagement:**</span><span class="sxs-lookup"><span data-stu-id="923e0-135">**With Engagement :**</span></span>

            package com.company.myapp;

            import com.microsoft.azure.engagement.activity.EngagementActivity;
            import android.os.Bundle;

            public class MyApp extends EngagementActivity
            {
              @Override
              public void onCreate(Bundle savedInstanceState)
              {
                super.onCreate(savedInstanceState);
                setContentView(R.layout.main);
              }
            }

> [!IMPORTANT]
> <span data-ttu-id="923e0-136">Při použití `EngagementListActivity` nebo `EngagementExpandableListActivity`, ujistěte se, že všechny volat na `requestWindowFeature(...);` je provedena před volání `super.onCreate(...);`, jinak dojde k chybě.</span><span class="sxs-lookup"><span data-stu-id="923e0-136">When using `EngagementListActivity` or `EngagementExpandableListActivity`, make sure any call to `requestWindowFeature(...);` is made before the call to `super.onCreate(...);`, otherwise a crash will occur.</span></span>
> 
> 

<span data-ttu-id="923e0-137">Můžete najít v těchto tříd `src` složku a můžete je zkopírovat do projektu.</span><span class="sxs-lookup"><span data-stu-id="923e0-137">You can find these classes in the `src` folder, and can copy them into your project.</span></span> <span data-ttu-id="923e0-138">Třídy jsou i ve **JavaDoc**.</span><span class="sxs-lookup"><span data-stu-id="923e0-138">The classes are also in the **JavaDoc**.</span></span>

### <a name="alternate-method-call-startactivity-and-endactivity-manually"></a><span data-ttu-id="923e0-139">Alternativní metoda: volání `startActivity()` a `endActivity()` ručně</span><span class="sxs-lookup"><span data-stu-id="923e0-139">Alternate method: call `startActivity()` and `endActivity()` manually</span></span>
<span data-ttu-id="923e0-140">Pokud nemůžete nebo nechcete přetížení vaše `Activity` třídy, můžete místo toho začínat a končit vaše aktivity voláním `EngagementAgent`na metody přímo.</span><span class="sxs-lookup"><span data-stu-id="923e0-140">If you cannot or do not want to overload your `Activity` classes, you can instead start and end your activities by calling `EngagementAgent`'s methods directly.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="923e0-141">Nikdy nevolá SDK pro Android `endActivity()` metoda, i když ukončení aplikace (v systému Android, jsou ve skutečnosti nikdy ukončit aplikace).</span><span class="sxs-lookup"><span data-stu-id="923e0-141">The Android SDK never calls the `endActivity()` method, even when the application is closed (on Android, applications are actually never closed).</span></span> <span data-ttu-id="923e0-142">Z toho důvodu je *vysoce* doporučuje volat `startActivity()` metoda v `onResume` zpětného volání z *všechny* vaše aktivity a `endActivity()` metoda v `onPause()` zpětného volání z *všechny* vaše aktivity.</span><span class="sxs-lookup"><span data-stu-id="923e0-142">Thus, it is *HIGHLY* recommended to call the `startActivity()` method in the `onResume` callback of *ALL* your activities, and the `endActivity()` method in the `onPause()` callback of *ALL* your activities.</span></span> <span data-ttu-id="923e0-143">Toto je jediný způsob, jak Ujistěte se, nebude úniku relací.</span><span class="sxs-lookup"><span data-stu-id="923e0-143">This is the only way to be sure that sessions will not be leaked.</span></span> <span data-ttu-id="923e0-144">Pokud relace došlo k úniku, službu Engagement nikdy odpojí z back-end Engagement (protože službu zůstane připojen tak dlouho, dokud relace čeká na vyřízení).</span><span class="sxs-lookup"><span data-stu-id="923e0-144">If a session is leaked, the Engagement service will never disconnect from the Engagement backend (since the service stays connected as long as a session is pending).</span></span>
> 
> 

<span data-ttu-id="923e0-145">Zde naleznete příklad:</span><span class="sxs-lookup"><span data-stu-id="923e0-145">Here is an example:</span></span>

            public class MyActivity extends Some3rdPartyActivity
            {
              @Override
              protected void onResume()
              {
                super.onResume();
                String activityNameOnEngagement = EngagementAgentUtils.buildEngagementActivityName(getClass()); // Uses short class name and removes "Activity" at the end.
                EngagementAgent.getInstance(this).startActivity(this, activityNameOnEngagement, null);
              }

              @Override
              protected void onPause()
              {
                super.onPause();
                EngagementAgent.getInstance(this).endActivity();
              }
            }

<span data-ttu-id="923e0-146">Tento příklad velmi podobné `EngagementActivity` třídy a jeho variant, jejichž zdrojový kód je k dispozici v `src` složky.</span><span class="sxs-lookup"><span data-stu-id="923e0-146">This example very similiar to the `EngagementActivity` class and its variants, whose source code is provided in the `src` folder.</span></span>

## <a name="test"></a><span data-ttu-id="923e0-147">Test</span><span class="sxs-lookup"><span data-stu-id="923e0-147">Test</span></span>
<span data-ttu-id="923e0-148">Nyní Ověřte prosím svoji integraci spuštěním mobilní aplikaci v emulátoru nebo zařízení a ověření, že registruje relaci na kartě monitorování.</span><span class="sxs-lookup"><span data-stu-id="923e0-148">Now please verify your integration by running your mobile app in an emulator or device and verifying that it registers a session on the Monitor tab.</span></span>

<span data-ttu-id="923e0-149">V dalších oddílech jsou volitelné.</span><span class="sxs-lookup"><span data-stu-id="923e0-149">The next sections are optional.</span></span>

## <a name="location-reporting"></a><span data-ttu-id="923e0-150">Rozšířené ohlašování polohy</span><span class="sxs-lookup"><span data-stu-id="923e0-150">Location reporting</span></span>
<span data-ttu-id="923e0-151">Pokud chcete umístění třeba ohlásit, je nutné přidat pár řádků konfigurace (mezi `<application>` a `</application>` značek).</span><span class="sxs-lookup"><span data-stu-id="923e0-151">If you want locations to be reported, you need to add a few lines of configuration (between the `<application>` and `</application>` tags).</span></span>

### <a name="lazy-area-location-reporting"></a><span data-ttu-id="923e0-152">Opožděné hlášení umístění oblasti</span><span class="sxs-lookup"><span data-stu-id="923e0-152">Lazy area location reporting</span></span>
<span data-ttu-id="923e0-153">Opožděné hlášení umístění oblasti umožňuje podat zprávu zemi, oblast a polohu přidružené k zařízení.</span><span class="sxs-lookup"><span data-stu-id="923e0-153">Lazy area location reporting allows to report the country, region and locality associated to devices.</span></span> <span data-ttu-id="923e0-154">Tento typ hlášení umístění používá jenom síťová umístění (na základě ID buňky nebo Wi-Fi).</span><span class="sxs-lookup"><span data-stu-id="923e0-154">This type of location reporting only uses network locations (based on Cell ID or WIFI).</span></span> <span data-ttu-id="923e0-155">Oblasti zařízení se hlásí nejvíce jednou za relace.</span><span class="sxs-lookup"><span data-stu-id="923e0-155">The device area is reported at most once per session.</span></span> <span data-ttu-id="923e0-156">GPS se nikdy nepoužívá, a proto tento typ umístění sestavy má jen v několika (není k vyslovení ne) dopad na baterii.</span><span class="sxs-lookup"><span data-stu-id="923e0-156">The GPS is never used, and thus this type of location report has very few (not to say no) impact on the battery.</span></span>

<span data-ttu-id="923e0-157">Hlášené oblasti se používají k výpočtu geografické statistické údaje o uživatelích, relacích, události a chyby.</span><span class="sxs-lookup"><span data-stu-id="923e0-157">Reported areas are used to compute geographic statistics about users, sessions, events and errors.</span></span> <span data-ttu-id="923e0-158">Může být také používány jako kritérium v kampaně Reach.</span><span class="sxs-lookup"><span data-stu-id="923e0-158">They can also be used as criterion in Reach campaigns.</span></span>

<span data-ttu-id="923e0-159">Pokud chcete povolit opožděné hlášení umístění oblasti, můžete provést pomocí výše v tomto postupu konfigurace:</span><span class="sxs-lookup"><span data-stu-id="923e0-159">To enable lazy area location reporting, you can do it by using the configuration previously mentioned in this procedure :</span></span>

    EngagementConfiguration engagementConfiguration = new EngagementConfiguration();
    engagementConfiguration.setConnectionString("Endpoint={appCollection}.{domain};AppId={appId};SdkKey={sdkKey}");
    engagementConfiguration.setLazyAreaLocationReport(true);
    EngagementAgent.getInstance(this).init(engagementConfiguration);

<span data-ttu-id="923e0-160">Musíte taky Pokud chybí, přidejte následující oprávnění:</span><span class="sxs-lookup"><span data-stu-id="923e0-160">You also need to add the following permission if missing:</span></span>

            <uses-permission android:name="android.permission.ACCESS_COARSE_LOCATION"/>

<span data-ttu-id="923e0-161">Nebo můžete používat ``ACCESS_FINE_LOCATION`` pokud ji už používáte ve vaší aplikaci.</span><span class="sxs-lookup"><span data-stu-id="923e0-161">Or you can keep using ``ACCESS_FINE_LOCATION`` if you already use it in your application.</span></span>

### <a name="real-time-location-reporting"></a><span data-ttu-id="923e0-162">Hlášení polohy reálném čase.</span><span class="sxs-lookup"><span data-stu-id="923e0-162">Real time location reporting</span></span>
<span data-ttu-id="923e0-163">Hlášení polohy reálném čase umožňuje podat zprávu pro zeměpisnou šířku a zeměpisnou délku přidružené k zařízení.</span><span class="sxs-lookup"><span data-stu-id="923e0-163">Real time location reporting allows to report the latitude and longitude associated to devices.</span></span> <span data-ttu-id="923e0-164">Ve výchozím nastavení tento typ hlášení umístění používá jenom síťová umístění (na základě ID buňky nebo Wi-Fi) a generování sestav je aktivní pouze když je aplikace spuštěná v popředí (tj. během relace).</span><span class="sxs-lookup"><span data-stu-id="923e0-164">By default, this type of location reporting only uses network locations (based on Cell ID or WIFI), and the reporting is only active when the application runs in foreground (i.e. during a session).</span></span>

<span data-ttu-id="923e0-165">Umístění v reálném čase, jsou *není* slouží k výpočtu statistiky.</span><span class="sxs-lookup"><span data-stu-id="923e0-165">Real time locations are *NOT* used to compute statistics.</span></span> <span data-ttu-id="923e0-166">Jejich jediným účelem je umožnit použití geografického vymezení reálném čase \<monitorování geografických Reach-cílové skupiny zón\> kritérium v kampaně Reach.</span><span class="sxs-lookup"><span data-stu-id="923e0-166">Their only purpose is to allow the use of real time geo-fencing \<Reach-Audience-geofencing\> criterion in Reach campaigns.</span></span>

<span data-ttu-id="923e0-167">Pokud chcete povolit generování sestav umístění reálném čase, můžete provést pomocí výše v tomto postupu konfigurace:</span><span class="sxs-lookup"><span data-stu-id="923e0-167">To enable real time location reporting, you can do it by using the configuration previously mentioned in this procedure :</span></span>

    EngagementConfiguration engagementConfiguration = new EngagementConfiguration();
    engagementConfiguration.setConnectionString("Endpoint={appCollection}.{domain};AppId={appId};SdkKey={sdkKey}");
    engagementConfiguration.setRealtimeLocationReport(true);
    EngagementAgent.getInstance(this).init(engagementConfiguration);

<span data-ttu-id="923e0-168">Musíte taky Pokud chybí, přidejte následující oprávnění:</span><span class="sxs-lookup"><span data-stu-id="923e0-168">You also need to add the following permission if missing:</span></span>

            <uses-permission android:name="android.permission.ACCESS_COARSE_LOCATION"/>

<span data-ttu-id="923e0-169">Nebo můžete používat ``ACCESS_FINE_LOCATION`` pokud ji už používáte ve vaší aplikaci.</span><span class="sxs-lookup"><span data-stu-id="923e0-169">Or you can keep using ``ACCESS_FINE_LOCATION`` if you already use it in your application.</span></span>

#### <a name="gps-based-reporting"></a><span data-ttu-id="923e0-170">Na základě GPS při vytváření sestav</span><span class="sxs-lookup"><span data-stu-id="923e0-170">GPS based reporting</span></span>
<span data-ttu-id="923e0-171">Ve výchozím nastavení hlášení polohy reálném čase pouze používá síťové umístění.</span><span class="sxs-lookup"><span data-stu-id="923e0-171">By default, real time location reporting only uses network based locations.</span></span> <span data-ttu-id="923e0-172">Chcete-li povolit použití GPS na základě umístění (což je mnohem přesnější), použijte objekt konfigurace:</span><span class="sxs-lookup"><span data-stu-id="923e0-172">To enable the use of GPS based locations (which are far more precise), use the configuration object:</span></span>

    EngagementConfiguration engagementConfiguration = new EngagementConfiguration();
    engagementConfiguration.setConnectionString("Endpoint={appCollection}.{domain};AppId={appId};SdkKey={sdkKey}");
    engagementConfiguration.setRealtimeLocationReport(true);
    engagementConfiguration.setFineRealtimeLocationReport(true);
    EngagementAgent.getInstance(this).init(engagementConfiguration);

<span data-ttu-id="923e0-173">Musíte taky Pokud chybí, přidejte následující oprávnění:</span><span class="sxs-lookup"><span data-stu-id="923e0-173">You also need to add the following permission if missing:</span></span>

            <uses-permission android:name="android.permission.ACCESS_FINE_LOCATION"/>

#### <a name="background-reporting"></a><span data-ttu-id="923e0-174">Vytváření sestav pozadí</span><span class="sxs-lookup"><span data-stu-id="923e0-174">Background reporting</span></span>
<span data-ttu-id="923e0-175">Ve výchozím nastavení hlášení polohy reálném čase je aktivní pouze když je aplikace spuštěná v popředí (tj. během relace).</span><span class="sxs-lookup"><span data-stu-id="923e0-175">By default, real time location reporting is only active when the application runs in foreground (i.e. during a session).</span></span> <span data-ttu-id="923e0-176">Pokud chcete povolit také sestav v pozadí, použijte objekt konfigurace:</span><span class="sxs-lookup"><span data-stu-id="923e0-176">To enable the reporting also in background, use the configuration object:</span></span>

    EngagementConfiguration engagementConfiguration = new EngagementConfiguration();
    engagementConfiguration.setConnectionString("Endpoint={appCollection}.{domain};AppId={appId};SdkKey={sdkKey}");
    engagementConfiguration.setRealtimeLocationReport(true);
    engagementConfiguration.setBackgroundRealtimeLocationReport(true);
    EngagementAgent.getInstance(this).init(engagementConfiguration);

> [!NOTE]
> <span data-ttu-id="923e0-177">Při spuštění aplikace v pozadí, pouze sítě na základě umístění nahlásí, i když je povolená GPS.</span><span class="sxs-lookup"><span data-stu-id="923e0-177">When the application runs in background, only network based locations are reported, even if you enabled the GPS.</span></span>
> 
> 

<span data-ttu-id="923e0-178">Sestava umístění pozadí se zastaví, pokud uživatel restartuje jeho zařízení, můžete přidat to, aby se automaticky restartuje při spuštění:</span><span class="sxs-lookup"><span data-stu-id="923e0-178">The background location report will be stopped if the user reboots its device, you can add this to make it automatically restart at boot time:</span></span>

            <receiver android:name="com.microsoft.azure.engagement.EngagementLocationBootReceiver"
               android:exported="false">
               <intent-filter>
                  <action android:name="android.intent.action.BOOT_COMPLETED" />
               </intent-filter>
            </receiver>

<span data-ttu-id="923e0-179">Musíte taky Pokud chybí, přidejte následující oprávnění:</span><span class="sxs-lookup"><span data-stu-id="923e0-179">You also need to add the following permission if missing:</span></span>

            <uses-permission android:name="android.permission.RECEIVE_BOOT_COMPLETED" />

### <a name="android-m-permissions"></a><span data-ttu-id="923e0-180">Android M oprávnění</span><span class="sxs-lookup"><span data-stu-id="923e0-180">Android M permissions</span></span>
<span data-ttu-id="923e0-181">Od verze Android M, některá oprávnění jsou spravovány v době běhu a vyžaduje schválení uživatelem.</span><span class="sxs-lookup"><span data-stu-id="923e0-181">Starting with Android M, some permissions are managed at runtime and needs user approval.</span></span>

<span data-ttu-id="923e0-182">Pokud cílíte na úrovni rozhraní API systému Android 23, bude oprávnění runtime vypnuta ve výchozím nastavení pro nové instalace aplikace.</span><span class="sxs-lookup"><span data-stu-id="923e0-182">The runtime permissions will be turned off by default for new app installations if you target Android API level 23.</span></span> <span data-ttu-id="923e0-183">V opačném případě se bude se ve výchozím nastavení zapnuta.</span><span class="sxs-lookup"><span data-stu-id="923e0-183">Otherwise it will be turned on by default.</span></span>

<span data-ttu-id="923e0-184">Uživatele můžete povolit nebo zakázat tato oprávnění z nabídky nastavení zařízení.</span><span class="sxs-lookup"><span data-stu-id="923e0-184">The user can enable/disable those permissions from the device settings menu.</span></span> <span data-ttu-id="923e0-185">Vypnutí oprávnění z nabídky systému ukončí procesy na pozadí aplikace, to je chování systému a nemá žádný vliv na schopnost přijímat nabízená v pozadí.</span><span class="sxs-lookup"><span data-stu-id="923e0-185">Turning off permissions from system menu kills background processes of the application, this is a system behavior and has no impact on ability to receive push in background.</span></span>

<span data-ttu-id="923e0-186">V kontextu Mobile Engagement oprávnění, která vyžadují schválení za běhu jsou:</span><span class="sxs-lookup"><span data-stu-id="923e0-186">In the context of Mobile Engagement, the permissions that require approval at runtime are:</span></span>

* `ACCESS_COARSE_LOCATION`
* `ACCESS_FINE_LOCATION`
* <span data-ttu-id="923e0-187">`WRITE_EXTERNAL_STORAGE`(jenom při cílení na úrovni rozhraní API systému Android 23 pro tato)</span><span class="sxs-lookup"><span data-stu-id="923e0-187">`WRITE_EXTERNAL_STORAGE` (only when targeting Android API level 23 for this one)</span></span>

<span data-ttu-id="923e0-188">Externího úložiště se používá pouze pro funkce velký obrázek Reach.</span><span class="sxs-lookup"><span data-stu-id="923e0-188">The external storage is used only for Reach big picture feature.</span></span> <span data-ttu-id="923e0-189">Zjistíte-li toto oprávnění být rušivý požádat uživatele, můžete jej odebrat Pokud se používá pouze pro Mobile Engagement, ale za cenu zakázáním funkce velký obrázek.</span><span class="sxs-lookup"><span data-stu-id="923e0-189">If you find asking users this permission to be disruptive, you can remove it if you used it only for Mobile Engagement but at the cost of disabling big picture feature.</span></span>

<span data-ttu-id="923e0-190">Pro funkce, umístění měli byste požádat o oprávnění pro uživatele s využitím systému standardní dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="923e0-190">For the location features, you should request permissions to user using a standard system dialog.</span></span> <span data-ttu-id="923e0-191">Pokud uživatel schválí, budete muset říct ``EngagementAgent`` má provést tato změna v úvahu v reálném čase (jinak změny budou zpracovány při příštím uživatel spustí aplikaci).</span><span class="sxs-lookup"><span data-stu-id="923e0-191">If the user approves, you need to tell ``EngagementAgent`` to take that change into account in real time (otherwise the change will be processed the next time the user launches the application).</span></span>

<span data-ttu-id="923e0-192">Zde je ukázka kódu pro použití v aktivitě aplikace žádostí o oprávnění a předání výsledek, pokud je kladné k ``EngagementAgent``:</span><span class="sxs-lookup"><span data-stu-id="923e0-192">Here is a code sample to use in an activity of your application to request permissions and forward the result if positive to ``EngagementAgent``:</span></span>

    @Override
    public void onCreate(Bundle savedInstanceState)
    {
      /* Other code... */

      /* Request permissions */
      requestPermissions();
    }

    @TargetApi(Build.VERSION_CODES.M)
    private void requestPermissions()
    {
      /* Avoid crashing if not on Android M */
      if (Build.VERSION.SDK_INT >= Build.VERSION_CODES.M)
      {
        /*
         * Request location permission, but this won't explain why it is needed to the user.
         * The standard Android documentation explains with more details how to display a rationale activity to explain the user why the permission is needed in your application.
         * Putting COARSE vs FINE has no impact here, they are part of the same group for runtime permission management.
         */
        if (checkSelfPermission(android.Manifest.permission.ACCESS_FINE_LOCATION) != PackageManager.PERMISSION_GRANTED)
          requestPermissions(new String[] { android.Manifest.permission.ACCESS_FINE_LOCATION }, 0);

        /* Only if you want to keep features using external storage */
        if (checkSelfPermission(android.Manifest.permission.WRITE_EXTERNAL_STORAGE) != PackageManager.PERMISSION_GRANTED)
          requestPermissions(new String[] { android.Manifest.permission.WRITE_EXTERNAL_STORAGE }, 1);
      }
    }

    @Override
    public void onRequestPermissionsResult(int requestCode, String[] permissions, int[] grantResults)
    {
      /* Only a positive location permission update requires engagement agent refresh, hence the request code matching from above function */
      if (requestCode == 0 && grantResults[0] == PackageManager.PERMISSION_GRANTED)
        getEngagementAgent().refreshPermissions();
    }

## <a name="advanced-reporting"></a><span data-ttu-id="923e0-193">Rozšířená tvorba sestav</span><span class="sxs-lookup"><span data-stu-id="923e0-193">Advanced reporting</span></span>
<span data-ttu-id="923e0-194">Případně, pokud chcete k hlášení konkrétních událostí aplikace, chyb a úlohy, budete muset použít rozhraní API Engagement prostřednictvím metody `EngagementAgent` třídy.</span><span class="sxs-lookup"><span data-stu-id="923e0-194">Optionally, if you want to report application specific events, errors and jobs, you need to use the Engagement API through the methods of the `EngagementAgent` class.</span></span> <span data-ttu-id="923e0-195">Tato třída objektu může být retreived voláním `EngagementAgent.getInstance()` statickou metodu.</span><span class="sxs-lookup"><span data-stu-id="923e0-195">An object of this class can be retreived by calling the `EngagementAgent.getInstance()` static method.</span></span>

<span data-ttu-id="923e0-196">Rozhraní API Engagement umožňuje používat všechny rozšířené možnosti Engagement a podrobně pokynů používat rozhraní API Engagement v systému Android (i jako v technické dokumentaci `EngagementAgent` třídy).</span><span class="sxs-lookup"><span data-stu-id="923e0-196">The Engagement API allows to use all of Engagement's advanced capabilities and is detailed in the How to Use the Engagement API on Android (as well as in the technical documentation of the `EngagementAgent` class).</span></span>

## <a name="advanced-configuration-in-androidmanifestxml"></a><span data-ttu-id="923e0-197">Pokročilá konfigurace (v AndroidManifest.xml)</span><span class="sxs-lookup"><span data-stu-id="923e0-197">Advanced configuration (in AndroidManifest.xml)</span></span>
### <a name="wake-locks"></a><span data-ttu-id="923e0-198">Zámky probuzení</span><span class="sxs-lookup"><span data-stu-id="923e0-198">Wake locks</span></span>
<span data-ttu-id="923e0-199">Pokud chcete zajistit, že statistiky se odesílají v reálném čase, při použití Wi-Fi nebo obrazovky je vypnutý, přidejte následující volitelné oprávnění:</span><span class="sxs-lookup"><span data-stu-id="923e0-199">If you want to be sure that statistics are sent in real time when using Wifi or when the screen is off, add the following optional permission:</span></span>

            <uses-permission android:name="android.permission.WAKE_LOCK"/>

### <a name="crash-report"></a><span data-ttu-id="923e0-200">Hlášení o selhání</span><span class="sxs-lookup"><span data-stu-id="923e0-200">Crash report</span></span>
<span data-ttu-id="923e0-201">Pokud chcete zakázat hlášení selhání, přidejte tuto (mezi `<application>` a `</application>` značky):</span><span class="sxs-lookup"><span data-stu-id="923e0-201">If you want to disable crash reports, add this (between the `<application>` and `</application>` tags):</span></span>

            <meta-data android:name="engagement:reportCrash" android:value="false"/>

### <a name="burst-threshold"></a><span data-ttu-id="923e0-202">Prahová hodnota shluku</span><span class="sxs-lookup"><span data-stu-id="923e0-202">Burst threshold</span></span>
<span data-ttu-id="923e0-203">Ve výchozím nastavení sestavy služby zapojení protokolů v reálném čase.</span><span class="sxs-lookup"><span data-stu-id="923e0-203">By default, the Engagement service reports logs in real time.</span></span> <span data-ttu-id="923e0-204">Pokud vaše aplikace hlásí protokoly velmi často, je lepší ukládat do vyrovnávací paměti protokoly a sestavy je všechny najednou pravidelné základní časové (to se nazývá "shluků režim").</span><span class="sxs-lookup"><span data-stu-id="923e0-204">If your application reports logs very frequently, it is better to buffer the logs and to report them all at once on a regular time base (this is called the "burst mode").</span></span> <span data-ttu-id="923e0-205">Uděláte to tak, přidejte tuto (mezi `<application>` a `</application>` značky):</span><span class="sxs-lookup"><span data-stu-id="923e0-205">To do so, add this (between the `<application>` and `</application>` tags):</span></span>

            <meta-data android:name="engagement:burstThreshold" android:value="{interval between too bursts (in milliseconds)}"/>

<span data-ttu-id="923e0-206">Režim shluků mírně zvýšit výdrž baterie, ale má vliv na zapojení monitorování: všechny dobu trvání relace a úlohy zaokrouhlen shluků prahovou hodnotu (tedy relací a úlohy kratší, než je prahová hodnota shluků nemusí být zobrazeny).</span><span class="sxs-lookup"><span data-stu-id="923e0-206">The burst mode slightly increase the battery life but has an impact on the Engagement Monitor: all sessions and jobs duration will be rounded to the burst threshold (thus, sessions and jobs shorter than the burst threshold may not be visible).</span></span> <span data-ttu-id="923e0-207">Doporučujeme používat práh shluků než 30000 (30s).</span><span class="sxs-lookup"><span data-stu-id="923e0-207">It is recommended to use a burst threshold no longer than 30000 (30s).</span></span>

### <a name="session-timeout"></a><span data-ttu-id="923e0-208">Časový limit relace</span><span class="sxs-lookup"><span data-stu-id="923e0-208">Session timeout</span></span>
<span data-ttu-id="923e0-209">Ve výchozím nastavení relace je zakončeno 10s po uplynutí jeho poslední aktivita (který obvykle probíhá po stisknutí klávesy Home nebo zpět, nastavení telefonu nečinnosti nebo přechod do jiné aplikace).</span><span class="sxs-lookup"><span data-stu-id="923e0-209">By default, a session is ended 10s after the end of its last activity (which usually occurs by pressing the Home or Back key, by setting the phone idle or by jumping into another application).</span></span> <span data-ttu-id="923e0-210">Tím se vyhnete rozdělení relace každý čas ukončení uživatele a vrátit se do aplikace velmi rychle (který může dojít při mu vyzvednutí bitovou kopii, zkontrolujte, zda oznámení atd.).</span><span class="sxs-lookup"><span data-stu-id="923e0-210">This is to avoid a session split each time the user exit and return to the application very quickly (which can happen when he pick up a image, check a notification, etc.).</span></span> <span data-ttu-id="923e0-211">Můžete upravit tento parametr.</span><span class="sxs-lookup"><span data-stu-id="923e0-211">You may want to modify this parameter.</span></span> <span data-ttu-id="923e0-212">Uděláte to tak, přidejte tuto (mezi `<application>` a `</application>` značky):</span><span class="sxs-lookup"><span data-stu-id="923e0-212">To do so, add this (between the `<application>` and `</application>` tags):</span></span>

            <meta-data android:name="engagement:sessionTimeout" android:value="{session timeout (in milliseconds)}"/>

## <a name="disable-log-reporting"></a><span data-ttu-id="923e0-213">Zakázání zasílání zpráv protokolu</span><span class="sxs-lookup"><span data-stu-id="923e0-213">Disable log reporting</span></span>
### <a name="using-a-method-call"></a><span data-ttu-id="923e0-214">Pomocí volání metody</span><span class="sxs-lookup"><span data-stu-id="923e0-214">Using a method call</span></span>
<span data-ttu-id="923e0-215">Pokud chcete Engagement zastavit odesílání protokolů, můžete volat:</span><span class="sxs-lookup"><span data-stu-id="923e0-215">If you want Engagement to stop sending logs, you can call:</span></span>

            EngagementAgent.getInstance(context).setEnabled(false);

<span data-ttu-id="923e0-216">Toto volání je trvalé: používá soubor sdílet předvolby.</span><span class="sxs-lookup"><span data-stu-id="923e0-216">This call is persistent: it uses a shared preferences file.</span></span>

<span data-ttu-id="923e0-217">Pokud je při volání této funkce aktivní zapojení, může trvat 1 minuta pro zastavení služby.</span><span class="sxs-lookup"><span data-stu-id="923e0-217">If Engagement is active when you call this function, it may take 1 minute for the service to stop.</span></span> <span data-ttu-id="923e0-218">Ale se nespustí službu vůbec při příštím spuštění aplikace.</span><span class="sxs-lookup"><span data-stu-id="923e0-218">However it won't launch the service at all the next time you launch the application.</span></span>

<span data-ttu-id="923e0-219">Můžete povolit protokol reporting znovu voláním stejnou funkci s `true`.</span><span class="sxs-lookup"><span data-stu-id="923e0-219">You can enable log reporting again by calling the same function with `true`.</span></span>

### <a name="integration-in-your-own-preferenceactivity"></a><span data-ttu-id="923e0-220">Integrace ve vašem vlastním`PreferenceActivity`</span><span class="sxs-lookup"><span data-stu-id="923e0-220">Integration in your own `PreferenceActivity`</span></span>
<span data-ttu-id="923e0-221">Namísto volání této funkce, můžete také integrovat toto nastavení přímo do existující `PreferenceActivity`.</span><span class="sxs-lookup"><span data-stu-id="923e0-221">Instead of calling this function, you can also integrate this setting directly in your existing `PreferenceActivity`.</span></span>

<span data-ttu-id="923e0-222">Zapojení používat vaše předvolby soubor (s požadovanou režim) můžete nakonfigurovat `AndroidManifest.xml` soubor s `application meta-data`:</span><span class="sxs-lookup"><span data-stu-id="923e0-222">You can configure Engagement to use your preferences file (with the desired mode) in the `AndroidManifest.xml` file with `application meta-data`:</span></span>

* <span data-ttu-id="923e0-223">`engagement:agent:settings:name` Klíč se používá k definování názvu souboru sdílet předvolby.</span><span class="sxs-lookup"><span data-stu-id="923e0-223">The `engagement:agent:settings:name` key is used to define the name of the shared preferences file.</span></span>
* <span data-ttu-id="923e0-224">`engagement:agent:settings:mode` Klíč se používá k definování režim souboru sdílet předvolby, měli byste použít stejný režim jako ve vašem `PreferenceActivity`.</span><span class="sxs-lookup"><span data-stu-id="923e0-224">The `engagement:agent:settings:mode` key is used to define the mode of the shared preferences file, you should use the same mode as in your `PreferenceActivity`.</span></span> <span data-ttu-id="923e0-225">Režim musí být předán jako číslo: Pokud používáte kombinaci konstantní příznaky ve vašem kódu, zkontrolujte celkovou hodnotu.</span><span class="sxs-lookup"><span data-stu-id="923e0-225">The mode must be passed as a number: if you are using a combination of constant flags in your code, check the total value.</span></span>

<span data-ttu-id="923e0-226">Zapojení vždy použít `engagement:key` boolean klíč v souboru předvoleb pro správu tohoto nastavení.</span><span class="sxs-lookup"><span data-stu-id="923e0-226">Engagement always use the `engagement:key` boolean key within the preferences file for managing this setting.</span></span>

<span data-ttu-id="923e0-227">Následující příklad `AndroidManifest.xml` zobrazí výchozí hodnoty:</span><span class="sxs-lookup"><span data-stu-id="923e0-227">The following example of `AndroidManifest.xml` shows the default values:</span></span>

            <application>
                [...]
                <meta-data
                  android:name="engagement:agent:settings:name"
                  android:value="engagement.agent" />
                <meta-data
                  android:name="engagement:agent:settings:mode"
                  android:value="0" />

<span data-ttu-id="923e0-228">Potom můžete přidat `CheckBoxPreference` v vaše předvolby rozložení stejný, jako je následující:</span><span class="sxs-lookup"><span data-stu-id="923e0-228">Then you can add a `CheckBoxPreference` in your preference layout like the following one:</span></span>

            <CheckBoxPreference
              android:key="engagement:enabled"
              android:defaultValue="true"
              android:title="Use Engagement"
              android:summaryOn="Engagement is enabled."
              android:summaryOff="Engagement is disabled." />

<!-- URLs. -->
[Device API]: http://go.microsoft.com/?linkid=9876094
