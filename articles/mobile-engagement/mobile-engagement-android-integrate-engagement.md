---
title: aaaAzure integraci sady Android SDK Mobile Engagement
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
ms.openlocfilehash: 4f79936ea0fa6102023dec2b4682032a4a81fa9e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="how-toointegrate-engagement-on-android"></a><span data-ttu-id="a1ebd-103">Jak tooIntegrate Engagement v systému Android</span><span class="sxs-lookup"><span data-stu-id="a1ebd-103">How tooIntegrate Engagement on Android</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="a1ebd-104">Univerzální pro Windows</span><span class="sxs-lookup"><span data-stu-id="a1ebd-104">Windows Universal</span></span>](mobile-engagement-windows-store-integrate-engagement.md)
> * [<span data-ttu-id="a1ebd-105">Windows Phone Silverlight</span><span class="sxs-lookup"><span data-stu-id="a1ebd-105">Windows Phone Silverlight</span></span>](mobile-engagement-windows-phone-integrate-engagement.md)
> * [<span data-ttu-id="a1ebd-106">iOS</span><span class="sxs-lookup"><span data-stu-id="a1ebd-106">iOS</span></span>](mobile-engagement-ios-integrate-engagement.md)
> * [<span data-ttu-id="a1ebd-107">Android</span><span class="sxs-lookup"><span data-stu-id="a1ebd-107">Android</span></span>](mobile-engagement-android-integrate-engagement.md)
> 
> 

<span data-ttu-id="a1ebd-108">Tento postup popisuje hello nejjednodušší způsob, jak tooactivate Engagement analýzy a monitorování funkce ve vaší aplikace Android.</span><span class="sxs-lookup"><span data-stu-id="a1ebd-108">This procedure describes hello simplest way tooactivate Engagement's Analytics and Monitoring functions in your Android application.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="a1ebd-109">Vaše minimální úroveň rozhraní API systému Android SDK musí být 10 nebo vyšší (Android 2.3.3 nebo vyšší).</span><span class="sxs-lookup"><span data-stu-id="a1ebd-109">Your minimum Android SDK API level must be 10 or higher (Android 2.3.3 or higher).</span></span>
> 
> 

<span data-ttu-id="a1ebd-110">Hello následující kroky jsou že dostatek tooactivates hello protokolů je nutná sestava toocompute všechny statistické údaje týkající se uživatelů, relací, aktivity, dojde k chybě a Technicals.</span><span class="sxs-lookup"><span data-stu-id="a1ebd-110">hello following steps are enough tooactivates hello report of logs needed toocompute all statistics regarding Users, Sessions, Activities, Crashes and Technicals.</span></span> <span data-ttu-id="a1ebd-111">Hello protokolů je nutná sestava toocompute další statistiky jako události a chyby úlohy je třeba provést ručně pomocí rozhraní API Engagement hello (viz [jak toouse hello advanced Mobile Engagement označování rozhraní API v systémem Android](mobile-engagement-android-use-engagement-api.md) od těchto statistiky jsou závislé aplikace.</span><span class="sxs-lookup"><span data-stu-id="a1ebd-111">hello report of logs needed toocompute other statistics like Events, Errors and Jobs must be done manually using hello Engagement API (see [How toouse hello advanced Mobile Engagement tagging API in your Android](mobile-engagement-android-use-engagement-api.md) since these statistics are application dependent.</span></span>

## <a name="embed-hello-engagement-sdk-and-service-into-your-android-project"></a><span data-ttu-id="a1ebd-112">Vložení hello Engagement SDK a služby do vašeho projektu Android</span><span class="sxs-lookup"><span data-stu-id="a1ebd-112">Embed hello Engagement SDK and service into your Android project</span></span>
<span data-ttu-id="a1ebd-113">Stažení hello sady SDK pro Android z [sem](https://aka.ms/vq9mfn) získat `mobile-engagement-VERSION.jar` a umístí je do hello `libs` složky vašeho projektu Android (vytvořit složky libs hello, pokud neexistuje ještě).</span><span class="sxs-lookup"><span data-stu-id="a1ebd-113">Download hello Android SDK from [here](https://aka.ms/vq9mfn) Get `mobile-engagement-VERSION.jar` and put them into hello `libs` folder of your Android project (create hello libs folder if it does not exist yet).</span></span>

> [!IMPORTANT]
> <span data-ttu-id="a1ebd-114">Pokud vytvoříte balíčku aplikace s ProGuard, musíte tookeep některé třídy.</span><span class="sxs-lookup"><span data-stu-id="a1ebd-114">If you build your application package with ProGuard, you need tookeep some classes.</span></span> <span data-ttu-id="a1ebd-115">Můžete použít následující fragment kódu konfigurace hello:</span><span class="sxs-lookup"><span data-stu-id="a1ebd-115">You can use hello following configuration snippet:</span></span>
> 
> <span data-ttu-id="a1ebd-116">-Udržovat veřejná třída * rozšiřuje android.os.IInterface-zachovat {com.microsoft.azure.engagement.reach.activity.EngagementWebAnnouncementActivity$EngagementReachContentJS – třída</span><span class="sxs-lookup"><span data-stu-id="a1ebd-116">-keep public class * extends android.os.IInterface -keep class com.microsoft.azure.engagement.reach.activity.EngagementWebAnnouncementActivity$EngagementReachContentJS {</span></span>
> 
> <span data-ttu-id="a1ebd-117"><methods>; }</span><span class="sxs-lookup"><span data-stu-id="a1ebd-117"><methods>; }</span></span>
> 
> 

<span data-ttu-id="a1ebd-118">Zadejte připojovací řetězec Engagement volání hello následující metodu v aktivitě Spouštěče hello:</span><span class="sxs-lookup"><span data-stu-id="a1ebd-118">Specify your Engagement connection string by calling hello following method in hello launcher activity:</span></span>

            EngagementConfiguration engagementConfiguration = new EngagementConfiguration();
            engagementConfiguration.setConnectionString("Endpoint={appCollection}.{domain};AppId={appId};SdkKey={sdkKey}");
            EngagementAgent.getInstance(this).init(engagementConfiguration);

<span data-ttu-id="a1ebd-119">Hello připojovací řetězec pro vaši aplikaci se zobrazí na portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="a1ebd-119">hello connection string for your application is displayed on Azure Portal.</span></span>

* <span data-ttu-id="a1ebd-120">Pokud chybí, přidejte následující Android oprávnění hello (před hello `<application>` značka):</span><span class="sxs-lookup"><span data-stu-id="a1ebd-120">If missing, add hello following Android permissions (before hello `<application>` tag):</span></span>
  
          <uses-permission android:name="android.permission.INTERNET"/>
          <uses-permission android:name="android.permission.ACCESS_NETWORK_STATE"/>
* <span data-ttu-id="a1ebd-121">Přidejte následující části hello (mezi hello `<application>` a `</application>` značky):</span><span class="sxs-lookup"><span data-stu-id="a1ebd-121">Add hello following section (between hello `<application>` and `</application>` tags):</span></span>
  
          <service
            android:name="com.microsoft.azure.engagement.service.EngagementService"
            android:exported="false"
            android:label="<Your application name>Service"
            android:process=":Engagement"/>
* <span data-ttu-id="a1ebd-122">Změna `<Your application name>` hello název vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="a1ebd-122">Change `<Your application name>` by hello name of your application.</span></span>

> [!TIP]
> <span data-ttu-id="a1ebd-123">Hello `android:label` atribut vám umožní toochoose hello název hello zapojení služby jak ho uvidí koncoví uživatelé toohello v obrazovce "Spuštění služby" hello svůj telefon.</span><span class="sxs-lookup"><span data-stu-id="a1ebd-123">hello `android:label` attribute allows you toochoose hello name of hello Engagement service as it will appear toohello end-users in hello "Running services" screen of their phone.</span></span> <span data-ttu-id="a1ebd-124">Je doporučeno tooset tento atribut příliš`"<Your application name>Service"` (například `"AcmeFunGameService"`).</span><span class="sxs-lookup"><span data-stu-id="a1ebd-124">It is recommended tooset this attribute too`"<Your application name>Service"` (e.g. `"AcmeFunGameService"`).</span></span>
> 
> 

<span data-ttu-id="a1ebd-125">Zadání hello `android:process` atribut zajistí, že hello Engagement služba se spustí ve svém vlastním procesu (spuštěnými Engagement na stejný proces vaše aplikace bude vaše vlákna main/uživatelského rozhraní potenciálně pomaleji hello).</span><span class="sxs-lookup"><span data-stu-id="a1ebd-125">Specifying hello `android:process` attribute ensures that hello Engagement service will run in its own process (running Engagement in hello same process as your application will make your main/UI thread potentially less responsive).</span></span>

> [!NOTE]
> <span data-ttu-id="a1ebd-126">Kód, umístěte do `Application.onCreate()` a dalších zpětná volání aplikace bude spuštěna pro procesy všechny aplikace, včetně služby zapojení hello.</span><span class="sxs-lookup"><span data-stu-id="a1ebd-126">Any code you place in `Application.onCreate()` and other application callbacks will be run for all your application's processes, including hello Engagement service.</span></span> <span data-ttu-id="a1ebd-127">Může mít nežádoucí vedlejší efekty (jako přiřazení nepotřebné paměti a vláken v procesu hello zapojení, duplicitní všesměrového vysílání příjemci nebo služby).</span><span class="sxs-lookup"><span data-stu-id="a1ebd-127">It may have unwanted side effects (like unneeded memory allocations and threads in hello Engagement's process, duplicate broadcast receivers or services).</span></span>
> 
> 

<span data-ttu-id="a1ebd-128">Pokud přepíšete `Application.onCreate()`, je doporučené tooadd hello následující fragment kódu na začátku hello vaší `Application.onCreate()` funkce:</span><span class="sxs-lookup"><span data-stu-id="a1ebd-128">If you override `Application.onCreate()`, it's recommended tooadd hello following code snippet at hello beginning of your `Application.onCreate()` function:</span></span>

             public void onCreate()
             {
               if (EngagementAgentUtils.isInDedicatedEngagementProcess(this))
                 return;

               ... Your code...
             }

<span data-ttu-id="a1ebd-129">Můžete provést hello samé pro `Application.onTerminate()`, `Application.onLowMemory()` a `Application.onConfigurationChanged(...)`.</span><span class="sxs-lookup"><span data-stu-id="a1ebd-129">You can do hello same thing for `Application.onTerminate()`, `Application.onLowMemory()` and `Application.onConfigurationChanged(...)`.</span></span>

<span data-ttu-id="a1ebd-130">Můžete také rozšířit `EngagementApplication` místo rozšíření `Application`: hello zpětného volání `Application.onCreate()` hello procesu kontroly a volání `Application.onApplicationProcessCreate()` jen pokud aktuální proces hello není hello jedné hostování hello zapojení služby, hello stejná pravidla platí pro Hello jiných zpětných volání.</span><span class="sxs-lookup"><span data-stu-id="a1ebd-130">You can also extend `EngagementApplication` instead of extending `Application`: hello callback `Application.onCreate()` does hello process check and calls `Application.onApplicationProcessCreate()` only if hello current process is not hello one hosting hello Engagement service, hello same rules apply for hello other callbacks.</span></span>

## <a name="basic-reporting"></a><span data-ttu-id="a1ebd-131">Vytváření základních sestav</span><span class="sxs-lookup"><span data-stu-id="a1ebd-131">Basic reporting</span></span>
### <a name="recommended-method-overload-your-activity-classes"></a><span data-ttu-id="a1ebd-132">Doporučená metoda: přetížení vaší `Activity` třídy</span><span class="sxs-lookup"><span data-stu-id="a1ebd-132">Recommended method: overload your `Activity` classes</span></span>
<span data-ttu-id="a1ebd-133">V sestavě hello tooactivate pořadí všechny protokoly hello vyžadují zapojení toocompute uživatelů, relací, aktivity, dojde k chybě a technické statistiky máte právě toomake všechny vaše `*Activity` dílčí třídy dědí hello odpovídající `Engagement*Activity` třídy (například pokud vaše aktivita starší verze rozšiřuje `ListActivity`, zkontrolujte ji rozšiřuje `EngagementListActivity`).</span><span class="sxs-lookup"><span data-stu-id="a1ebd-133">In order tooactivate hello report of all hello logs required by Engagement toocompute Users, Sessions, Activities, Crashes and Technical statistics, you just have toomake all your `*Activity` sub-classes inherit from hello corresponding `Engagement*Activity` classes (e.g. if your legacy activity extends `ListActivity`, make it extends `EngagementListActivity`).</span></span>

<span data-ttu-id="a1ebd-134">**Bez Engagement:**</span><span class="sxs-lookup"><span data-stu-id="a1ebd-134">**Without Engagement :**</span></span>

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

<span data-ttu-id="a1ebd-135">**S Engagement:**</span><span class="sxs-lookup"><span data-stu-id="a1ebd-135">**With Engagement :**</span></span>

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
> <span data-ttu-id="a1ebd-136">Při použití `EngagementListActivity` nebo `EngagementExpandableListActivity`, ujistěte se, že žádném volání, příliš`requestWindowFeature(...);` je provedena před hello volání příliš`super.onCreate(...);`, jinak dojde k chybě.</span><span class="sxs-lookup"><span data-stu-id="a1ebd-136">When using `EngagementListActivity` or `EngagementExpandableListActivity`, make sure any call too`requestWindowFeature(...);` is made before hello call too`super.onCreate(...);`, otherwise a crash will occur.</span></span>
> 
> 

<span data-ttu-id="a1ebd-137">Tyto třídy můžete najít v hello `src` složku a můžete je zkopírovat do projektu.</span><span class="sxs-lookup"><span data-stu-id="a1ebd-137">You can find these classes in hello `src` folder, and can copy them into your project.</span></span> <span data-ttu-id="a1ebd-138">Hello třídy jsou také v hello **JavaDoc**.</span><span class="sxs-lookup"><span data-stu-id="a1ebd-138">hello classes are also in hello **JavaDoc**.</span></span>

### <a name="alternate-method-call-startactivity-and-endactivity-manually"></a><span data-ttu-id="a1ebd-139">Alternativní metoda: volání `startActivity()` a `endActivity()` ručně</span><span class="sxs-lookup"><span data-stu-id="a1ebd-139">Alternate method: call `startActivity()` and `endActivity()` manually</span></span>
<span data-ttu-id="a1ebd-140">Pokud nemůžete nebo nechcete, aby toooverload vaše `Activity` třídy, můžete místo toho začínat a končit vaše aktivity voláním `EngagementAgent`na metody přímo.</span><span class="sxs-lookup"><span data-stu-id="a1ebd-140">If you cannot or do not want toooverload your `Activity` classes, you can instead start and end your activities by calling `EngagementAgent`'s methods directly.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="a1ebd-141">Hello Android SDK nikdy nevolá hello `endActivity()` metoda, i když ukončení aplikace hello (v systému Android, jsou ve skutečnosti nikdy ukončit aplikace).</span><span class="sxs-lookup"><span data-stu-id="a1ebd-141">hello Android SDK never calls hello `endActivity()` method, even when hello application is closed (on Android, applications are actually never closed).</span></span> <span data-ttu-id="a1ebd-142">Z toho důvodu je *vysoce* doporučená toocall hello `startActivity()` metoda v hello `onResume` zpětného volání z *všechny* vaše aktivity a hello `endActivity()` metoda v hello `onPause()` zpětné volání z *všechny* vašich aktivit.</span><span class="sxs-lookup"><span data-stu-id="a1ebd-142">Thus, it is *HIGHLY* recommended toocall hello `startActivity()` method in hello `onResume` callback of *ALL* your activities, and hello `endActivity()` method in hello `onPause()` callback of *ALL* your activities.</span></span> <span data-ttu-id="a1ebd-143">Toto je hello jediný způsob, jak toobe zda relací nebudou úniku.</span><span class="sxs-lookup"><span data-stu-id="a1ebd-143">This is hello only way toobe sure that sessions will not be leaked.</span></span> <span data-ttu-id="a1ebd-144">Pokud relace došlo k úniku, hello zapojení služby se nikdy odpojit od hello Engagement back-end (protože hello služby zůstane připojen tak dlouho, dokud relace čeká na vyřízení).</span><span class="sxs-lookup"><span data-stu-id="a1ebd-144">If a session is leaked, hello Engagement service will never disconnect from hello Engagement backend (since hello service stays connected as long as a session is pending).</span></span>
> 
> 

<span data-ttu-id="a1ebd-145">Zde naleznete příklad:</span><span class="sxs-lookup"><span data-stu-id="a1ebd-145">Here is an example:</span></span>

            public class MyActivity extends Some3rdPartyActivity
            {
              @Override
              protected void onResume()
              {
                super.onResume();
                String activityNameOnEngagement = EngagementAgentUtils.buildEngagementActivityName(getClass()); // Uses short class name and removes "Activity" at hello end.
                EngagementAgent.getInstance(this).startActivity(this, activityNameOnEngagement, null);
              }

              @Override
              protected void onPause()
              {
                super.onPause();
                EngagementAgent.getInstance(this).endActivity();
              }
            }

<span data-ttu-id="a1ebd-146">Tento příklad velmi podobné toohello `EngagementActivity` třídy a jeho variant, jejichž zdrojový kód je k dispozici v hello `src` složky.</span><span class="sxs-lookup"><span data-stu-id="a1ebd-146">This example very similiar toohello `EngagementActivity` class and its variants, whose source code is provided in hello `src` folder.</span></span>

## <a name="test"></a><span data-ttu-id="a1ebd-147">Test</span><span class="sxs-lookup"><span data-stu-id="a1ebd-147">Test</span></span>
<span data-ttu-id="a1ebd-148">Nyní Ověřte prosím svoji integraci spuštěním mobilní aplikaci v emulátoru nebo zařízení a ověření, že registruje relaci na kartě monitorování hello.</span><span class="sxs-lookup"><span data-stu-id="a1ebd-148">Now please verify your integration by running your mobile app in an emulator or device and verifying that it registers a session on hello Monitor tab.</span></span>

<span data-ttu-id="a1ebd-149">Další části Hello jsou volitelné.</span><span class="sxs-lookup"><span data-stu-id="a1ebd-149">hello next sections are optional.</span></span>

## <a name="location-reporting"></a><span data-ttu-id="a1ebd-150">Rozšířené ohlašování polohy</span><span class="sxs-lookup"><span data-stu-id="a1ebd-150">Location reporting</span></span>
<span data-ttu-id="a1ebd-151">Pokud chcete, aby oznámil toobe umístění, je třeba tooadd pár řádků konfigurace (mezi hello `<application>` a `</application>` značek).</span><span class="sxs-lookup"><span data-stu-id="a1ebd-151">If you want locations toobe reported, you need tooadd a few lines of configuration (between hello `<application>` and `</application>` tags).</span></span>

### <a name="lazy-area-location-reporting"></a><span data-ttu-id="a1ebd-152">Opožděné hlášení umístění oblasti</span><span class="sxs-lookup"><span data-stu-id="a1ebd-152">Lazy area location reporting</span></span>
<span data-ttu-id="a1ebd-153">Opožděné hlášení umístění oblasti umožňuje tooreport hello zemi, oblast a polohu přidružené toodevices.</span><span class="sxs-lookup"><span data-stu-id="a1ebd-153">Lazy area location reporting allows tooreport hello country, region and locality associated toodevices.</span></span> <span data-ttu-id="a1ebd-154">Tento typ hlášení umístění používá jenom síťová umístění (na základě ID buňky nebo Wi-Fi).</span><span class="sxs-lookup"><span data-stu-id="a1ebd-154">This type of location reporting only uses network locations (based on Cell ID or WIFI).</span></span> <span data-ttu-id="a1ebd-155">oblasti Hello zařízení se hlásí nejvíce jednou za relace.</span><span class="sxs-lookup"><span data-stu-id="a1ebd-155">hello device area is reported at most once per session.</span></span> <span data-ttu-id="a1ebd-156">Hello GPS se nikdy nepoužívá, a proto tento typ umístění sestavy má jen v několika (toosay není žádná) dopad na baterie hello.</span><span class="sxs-lookup"><span data-stu-id="a1ebd-156">hello GPS is never used, and thus this type of location report has very few (not toosay no) impact on hello battery.</span></span>

<span data-ttu-id="a1ebd-157">Hlášené oblasti jsou použité toocompute geografické Statistika týkající se uživatelů, relací, události a chyby.</span><span class="sxs-lookup"><span data-stu-id="a1ebd-157">Reported areas are used toocompute geographic statistics about users, sessions, events and errors.</span></span> <span data-ttu-id="a1ebd-158">Může být také používány jako kritérium v kampaně Reach.</span><span class="sxs-lookup"><span data-stu-id="a1ebd-158">They can also be used as criterion in Reach campaigns.</span></span>

<span data-ttu-id="a1ebd-159">vytváření sestav, můžete provést pomocí nástroje Konfigurace hello výše v tomto postupu umístění opožděné oblasti tooenable:</span><span class="sxs-lookup"><span data-stu-id="a1ebd-159">tooenable lazy area location reporting, you can do it by using hello configuration previously mentioned in this procedure :</span></span>

    EngagementConfiguration engagementConfiguration = new EngagementConfiguration();
    engagementConfiguration.setConnectionString("Endpoint={appCollection}.{domain};AppId={appId};SdkKey={sdkKey}");
    engagementConfiguration.setLazyAreaLocationReport(true);
    EngagementAgent.getInstance(this).init(engagementConfiguration);

<span data-ttu-id="a1ebd-160">Musíte taky tooadd hello, pokud chybí následující oprávnění:</span><span class="sxs-lookup"><span data-stu-id="a1ebd-160">You also need tooadd hello following permission if missing:</span></span>

            <uses-permission android:name="android.permission.ACCESS_COARSE_LOCATION"/>

<span data-ttu-id="a1ebd-161">Nebo můžete používat ``ACCESS_FINE_LOCATION`` pokud ji už používáte ve vaší aplikaci.</span><span class="sxs-lookup"><span data-stu-id="a1ebd-161">Or you can keep using ``ACCESS_FINE_LOCATION`` if you already use it in your application.</span></span>

### <a name="real-time-location-reporting"></a><span data-ttu-id="a1ebd-162">Hlášení polohy reálném čase.</span><span class="sxs-lookup"><span data-stu-id="a1ebd-162">Real time location reporting</span></span>
<span data-ttu-id="a1ebd-163">Hlášení polohy reálném čase umožňuje tooreport hello šířky a délky, přidružené toodevices.</span><span class="sxs-lookup"><span data-stu-id="a1ebd-163">Real time location reporting allows tooreport hello latitude and longitude associated toodevices.</span></span> <span data-ttu-id="a1ebd-164">Ve výchozím nastavení tento typ hlášení umístění používá jenom síťová umístění (na základě ID buňky nebo Wi-Fi) a vytváření sestav hello je aktivní pouze při spuštění aplikace hello v popředí (tj. během relace).</span><span class="sxs-lookup"><span data-stu-id="a1ebd-164">By default, this type of location reporting only uses network locations (based on Cell ID or WIFI), and hello reporting is only active when hello application runs in foreground (i.e. during a session).</span></span>

<span data-ttu-id="a1ebd-165">Umístění v reálném čase, jsou *není* používá toocompute statistiky.</span><span class="sxs-lookup"><span data-stu-id="a1ebd-165">Real time locations are *NOT* used toocompute statistics.</span></span> <span data-ttu-id="a1ebd-166">Jejich jediným účelem je použití hello tooallow geografického vymezení reálném čase \<monitorování geografických Reach-cílové skupiny zón\> kritérium v kampaně Reach.</span><span class="sxs-lookup"><span data-stu-id="a1ebd-166">Their only purpose is tooallow hello use of real time geo-fencing \<Reach-Audience-geofencing\> criterion in Reach campaigns.</span></span>

<span data-ttu-id="a1ebd-167">vytváření sestav, můžete provést pomocí nástroje Konfigurace hello výše v tomto postupu umístění reálném čase tooenable:</span><span class="sxs-lookup"><span data-stu-id="a1ebd-167">tooenable real time location reporting, you can do it by using hello configuration previously mentioned in this procedure :</span></span>

    EngagementConfiguration engagementConfiguration = new EngagementConfiguration();
    engagementConfiguration.setConnectionString("Endpoint={appCollection}.{domain};AppId={appId};SdkKey={sdkKey}");
    engagementConfiguration.setRealtimeLocationReport(true);
    EngagementAgent.getInstance(this).init(engagementConfiguration);

<span data-ttu-id="a1ebd-168">Musíte taky tooadd hello, pokud chybí následující oprávnění:</span><span class="sxs-lookup"><span data-stu-id="a1ebd-168">You also need tooadd hello following permission if missing:</span></span>

            <uses-permission android:name="android.permission.ACCESS_COARSE_LOCATION"/>

<span data-ttu-id="a1ebd-169">Nebo můžete používat ``ACCESS_FINE_LOCATION`` pokud ji už používáte ve vaší aplikaci.</span><span class="sxs-lookup"><span data-stu-id="a1ebd-169">Or you can keep using ``ACCESS_FINE_LOCATION`` if you already use it in your application.</span></span>

#### <a name="gps-based-reporting"></a><span data-ttu-id="a1ebd-170">Na základě GPS při vytváření sestav</span><span class="sxs-lookup"><span data-stu-id="a1ebd-170">GPS based reporting</span></span>
<span data-ttu-id="a1ebd-171">Ve výchozím nastavení hlášení polohy reálném čase pouze používá síťové umístění.</span><span class="sxs-lookup"><span data-stu-id="a1ebd-171">By default, real time location reporting only uses network based locations.</span></span> <span data-ttu-id="a1ebd-172">použití hello tooenable GPS na základě umístění (což je mnohem přesnější), použijte objekt konfigurace hello:</span><span class="sxs-lookup"><span data-stu-id="a1ebd-172">tooenable hello use of GPS based locations (which are far more precise), use hello configuration object:</span></span>

    EngagementConfiguration engagementConfiguration = new EngagementConfiguration();
    engagementConfiguration.setConnectionString("Endpoint={appCollection}.{domain};AppId={appId};SdkKey={sdkKey}");
    engagementConfiguration.setRealtimeLocationReport(true);
    engagementConfiguration.setFineRealtimeLocationReport(true);
    EngagementAgent.getInstance(this).init(engagementConfiguration);

<span data-ttu-id="a1ebd-173">Musíte taky tooadd hello, pokud chybí následující oprávnění:</span><span class="sxs-lookup"><span data-stu-id="a1ebd-173">You also need tooadd hello following permission if missing:</span></span>

            <uses-permission android:name="android.permission.ACCESS_FINE_LOCATION"/>

#### <a name="background-reporting"></a><span data-ttu-id="a1ebd-174">Vytváření sestav pozadí</span><span class="sxs-lookup"><span data-stu-id="a1ebd-174">Background reporting</span></span>
<span data-ttu-id="a1ebd-175">Ve výchozím nastavení hlášení polohy reálném čase je aktivní pouze při spuštění aplikace hello v popředí (tj. během relace).</span><span class="sxs-lookup"><span data-stu-id="a1ebd-175">By default, real time location reporting is only active when hello application runs in foreground (i.e. during a session).</span></span> <span data-ttu-id="a1ebd-176">vytváření sestav hello tooenable také v pozadí, použijte objekt konfigurace hello:</span><span class="sxs-lookup"><span data-stu-id="a1ebd-176">tooenable hello reporting also in background, use hello configuration object:</span></span>

    EngagementConfiguration engagementConfiguration = new EngagementConfiguration();
    engagementConfiguration.setConnectionString("Endpoint={appCollection}.{domain};AppId={appId};SdkKey={sdkKey}");
    engagementConfiguration.setRealtimeLocationReport(true);
    engagementConfiguration.setBackgroundRealtimeLocationReport(true);
    EngagementAgent.getInstance(this).init(engagementConfiguration);

> [!NOTE]
> <span data-ttu-id="a1ebd-177">Při spuštění aplikace hello v pozadí, jsou hlášeny pouze síťové umístění, i když jste povolili hello GPS.</span><span class="sxs-lookup"><span data-stu-id="a1ebd-177">When hello application runs in background, only network based locations are reported, even if you enabled hello GPS.</span></span>
> 
> 

<span data-ttu-id="a1ebd-178">Sestava umístění pozadí Hello se zastaví, pokud uživatel hello restartuje jeho zařízení, můžete přidat tento toomake automaticky restartuje při spuštění:</span><span class="sxs-lookup"><span data-stu-id="a1ebd-178">hello background location report will be stopped if hello user reboots its device, you can add this toomake it automatically restart at boot time:</span></span>

            <receiver android:name="com.microsoft.azure.engagement.EngagementLocationBootReceiver"
               android:exported="false">
               <intent-filter>
                  <action android:name="android.intent.action.BOOT_COMPLETED" />
               </intent-filter>
            </receiver>

<span data-ttu-id="a1ebd-179">Musíte taky tooadd hello, pokud chybí následující oprávnění:</span><span class="sxs-lookup"><span data-stu-id="a1ebd-179">You also need tooadd hello following permission if missing:</span></span>

            <uses-permission android:name="android.permission.RECEIVE_BOOT_COMPLETED" />

### <a name="android-m-permissions"></a><span data-ttu-id="a1ebd-180">Android M oprávnění</span><span class="sxs-lookup"><span data-stu-id="a1ebd-180">Android M permissions</span></span>
<span data-ttu-id="a1ebd-181">Od verze Android M, některá oprávnění jsou spravovány v době běhu a vyžaduje schválení uživatelem.</span><span class="sxs-lookup"><span data-stu-id="a1ebd-181">Starting with Android M, some permissions are managed at runtime and needs user approval.</span></span>

<span data-ttu-id="a1ebd-182">Pokud cílíte na úrovni rozhraní API systému Android 23, bude Hello runtime oprávnění vypnuta ve výchozím nastavení pro nové instalace aplikace.</span><span class="sxs-lookup"><span data-stu-id="a1ebd-182">hello runtime permissions will be turned off by default for new app installations if you target Android API level 23.</span></span> <span data-ttu-id="a1ebd-183">V opačném případě se bude se ve výchozím nastavení zapnuta.</span><span class="sxs-lookup"><span data-stu-id="a1ebd-183">Otherwise it will be turned on by default.</span></span>

<span data-ttu-id="a1ebd-184">Hello uživatele můžete povolit nebo zakázat tato oprávnění z nabídky nastavení zařízení hello.</span><span class="sxs-lookup"><span data-stu-id="a1ebd-184">hello user can enable/disable those permissions from hello device settings menu.</span></span> <span data-ttu-id="a1ebd-185">Vypnutí oprávnění z nabídky systému ukončí procesy na pozadí aplikace hello, to je chování systému a nemá žádný vliv na schopnost tooreceive nabízení v pozadí.</span><span class="sxs-lookup"><span data-stu-id="a1ebd-185">Turning off permissions from system menu kills background processes of hello application, this is a system behavior and has no impact on ability tooreceive push in background.</span></span>

<span data-ttu-id="a1ebd-186">V kontextu hello Mobile engagementu hello oprávnění, které vyžadují schválení za běhu jsou:</span><span class="sxs-lookup"><span data-stu-id="a1ebd-186">In hello context of Mobile Engagement, hello permissions that require approval at runtime are:</span></span>

* `ACCESS_COARSE_LOCATION`
* `ACCESS_FINE_LOCATION`
* <span data-ttu-id="a1ebd-187">`WRITE_EXTERNAL_STORAGE`(jenom při cílení na úrovni rozhraní API systému Android 23 pro tato)</span><span class="sxs-lookup"><span data-stu-id="a1ebd-187">`WRITE_EXTERNAL_STORAGE` (only when targeting Android API level 23 for this one)</span></span>

<span data-ttu-id="a1ebd-188">Hello externího úložiště se používá pouze pro funkce velký obrázek Reach.</span><span class="sxs-lookup"><span data-stu-id="a1ebd-188">hello external storage is used only for Reach big picture feature.</span></span> <span data-ttu-id="a1ebd-189">Pokud zjistíte, tato oprávnění toobe rušivý požádat uživatele, můžete jej odebrat Pokud se používá pouze pro Mobile Engagement, ale hello náklady na zakázání funkce velký obrázek.</span><span class="sxs-lookup"><span data-stu-id="a1ebd-189">If you find asking users this permission toobe disruptive, you can remove it if you used it only for Mobile Engagement but at hello cost of disabling big picture feature.</span></span>

<span data-ttu-id="a1ebd-190">Pro funkce hello umístění měli byste požádat o oprávnění toouser pomocí dialogu standardní systému.</span><span class="sxs-lookup"><span data-stu-id="a1ebd-190">For hello location features, you should request permissions toouser using a standard system dialog.</span></span> <span data-ttu-id="a1ebd-191">Pokud uživatel hello schválí, je třeba tootell ``EngagementAgent`` tootake, který změnit v úvahu v reálném čase (jinak změnu hello bude zpracován hello další čas hello uživatel spustí hello aplikaci).</span><span class="sxs-lookup"><span data-stu-id="a1ebd-191">If hello user approves, you need tootell ``EngagementAgent`` tootake that change into account in real time (otherwise hello change will be processed hello next time hello user launches hello application).</span></span>

<span data-ttu-id="a1ebd-192">Tady je toouse ukázkový kód v aktivitě toorequest oprávnění aplikací a k dopředného hello, pokud kladné příliš``EngagementAgent``:</span><span class="sxs-lookup"><span data-stu-id="a1ebd-192">Here is a code sample toouse in an activity of your application toorequest permissions and forward hello result if positive too``EngagementAgent``:</span></span>

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
         * Request location permission, but this won't explain why it is needed toohello user.
         * hello standard Android documentation explains with more details how toodisplay a rationale activity tooexplain hello user why hello permission is needed in your application.
         * Putting COARSE vs FINE has no impact here, they are part of hello same group for runtime permission management.
         */
        if (checkSelfPermission(android.Manifest.permission.ACCESS_FINE_LOCATION) != PackageManager.PERMISSION_GRANTED)
          requestPermissions(new String[] { android.Manifest.permission.ACCESS_FINE_LOCATION }, 0);

        /* Only if you want tookeep features using external storage */
        if (checkSelfPermission(android.Manifest.permission.WRITE_EXTERNAL_STORAGE) != PackageManager.PERMISSION_GRANTED)
          requestPermissions(new String[] { android.Manifest.permission.WRITE_EXTERNAL_STORAGE }, 1);
      }
    }

    @Override
    public void onRequestPermissionsResult(int requestCode, String[] permissions, int[] grantResults)
    {
      /* Only a positive location permission update requires engagement agent refresh, hence hello request code matching from above function */
      if (requestCode == 0 && grantResults[0] == PackageManager.PERMISSION_GRANTED)
        getEngagementAgent().refreshPermissions();
    }

## <a name="advanced-reporting"></a><span data-ttu-id="a1ebd-193">Rozšířená tvorba sestav</span><span class="sxs-lookup"><span data-stu-id="a1ebd-193">Advanced reporting</span></span>
<span data-ttu-id="a1ebd-194">Případně, pokud chcete tooreport aplikace konkrétní události, chyb a úlohy, je nutné toouse hello Engagement rozhraní API prostřednictvím metody hello hello `EngagementAgent` třídy.</span><span class="sxs-lookup"><span data-stu-id="a1ebd-194">Optionally, if you want tooreport application specific events, errors and jobs, you need toouse hello Engagement API through hello methods of hello `EngagementAgent` class.</span></span> <span data-ttu-id="a1ebd-195">Tato třída objektu může být retreived pomocí volání hello `EngagementAgent.getInstance()` statickou metodu.</span><span class="sxs-lookup"><span data-stu-id="a1ebd-195">An object of this class can be retreived by calling hello `EngagementAgent.getInstance()` static method.</span></span>

<span data-ttu-id="a1ebd-196">Hello Engagement rozhraní API umožňuje toouse, všechny rozšířené možnosti Engagement a jak je popsané v hello tooUse rozhraní API Engagement v systému Android (jako v technické dokumentaci hello hello i `EngagementAgent` třídy).</span><span class="sxs-lookup"><span data-stu-id="a1ebd-196">hello Engagement API allows toouse all of Engagement's advanced capabilities and is detailed in hello How tooUse the Engagement API on Android (as well as in hello technical documentation of hello `EngagementAgent` class).</span></span>

## <a name="advanced-configuration-in-androidmanifestxml"></a><span data-ttu-id="a1ebd-197">Pokročilá konfigurace (v AndroidManifest.xml)</span><span class="sxs-lookup"><span data-stu-id="a1ebd-197">Advanced configuration (in AndroidManifest.xml)</span></span>
### <a name="wake-locks"></a><span data-ttu-id="a1ebd-198">Zámky probuzení</span><span class="sxs-lookup"><span data-stu-id="a1ebd-198">Wake locks</span></span>
<span data-ttu-id="a1ebd-199">Pokud chcete, aby toobe jistotu, že statistiky se odesílají v reálném čase, při použití Wi-Fi nebo úvodní obrazovka je vypnutý, přidejte hello následující volitelné oprávnění:</span><span class="sxs-lookup"><span data-stu-id="a1ebd-199">If you want toobe sure that statistics are sent in real time when using Wifi or when hello screen is off, add hello following optional permission:</span></span>

            <uses-permission android:name="android.permission.WAKE_LOCK"/>

### <a name="crash-report"></a><span data-ttu-id="a1ebd-200">Hlášení o selhání</span><span class="sxs-lookup"><span data-stu-id="a1ebd-200">Crash report</span></span>
<span data-ttu-id="a1ebd-201">Pokud chcete toodisable hlášení selhání, přidejte tuto (mezi hello `<application>` a `</application>` značky):</span><span class="sxs-lookup"><span data-stu-id="a1ebd-201">If you want toodisable crash reports, add this (between hello `<application>` and `</application>` tags):</span></span>

            <meta-data android:name="engagement:reportCrash" android:value="false"/>

### <a name="burst-threshold"></a><span data-ttu-id="a1ebd-202">Prahová hodnota shluku</span><span class="sxs-lookup"><span data-stu-id="a1ebd-202">Burst threshold</span></span>
<span data-ttu-id="a1ebd-203">Ve výchozím nastavení sestavy služby zapojení hello protokolů v reálném čase.</span><span class="sxs-lookup"><span data-stu-id="a1ebd-203">By default, hello Engagement service reports logs in real time.</span></span> <span data-ttu-id="a1ebd-204">Pokud vaše aplikace hlásí protokoly velmi často, je lepší toobuffer hello protokoly a tooreport je všechny najednou na pravidelné základní časové (tomu se říká hello "shluků režim").</span><span class="sxs-lookup"><span data-stu-id="a1ebd-204">If your application reports logs very frequently, it is better toobuffer hello logs and tooreport them all at once on a regular time base (this is called hello "burst mode").</span></span> <span data-ttu-id="a1ebd-205">toodo Ano, přidejte tuto (mezi hello `<application>` a `</application>` značky):</span><span class="sxs-lookup"><span data-stu-id="a1ebd-205">toodo so, add this (between hello `<application>` and `</application>` tags):</span></span>

            <meta-data android:name="engagement:burstThreshold" android:value="{interval between too bursts (in milliseconds)}"/>

<span data-ttu-id="a1ebd-206">Hello shluků režimu mírně zvýšit hello baterie životnosti ale má vliv na hello monitorování Engagement: všechny doba trvání relace a úlohy se prahová hodnota shluků zaokrouhlené toohello (tedy relací a úlohy kratší, než je prahová hodnota shluků hello nemusí být zobrazeny).</span><span class="sxs-lookup"><span data-stu-id="a1ebd-206">hello burst mode slightly increase hello battery life but has an impact on hello Engagement Monitor: all sessions and jobs duration will be rounded toohello burst threshold (thus, sessions and jobs shorter than hello burst threshold may not be visible).</span></span> <span data-ttu-id="a1ebd-207">Je doporučeno toouse práh shluků než 30000 (30s).</span><span class="sxs-lookup"><span data-stu-id="a1ebd-207">It is recommended toouse a burst threshold no longer than 30000 (30s).</span></span>

### <a name="session-timeout"></a><span data-ttu-id="a1ebd-208">Časový limit relace</span><span class="sxs-lookup"><span data-stu-id="a1ebd-208">Session timeout</span></span>
<span data-ttu-id="a1ebd-209">Ve výchozím nastavení relace je zakončeno 10s po skončení hello jeho poslední aktivita (který obvykle probíhá ve stisknutím hello domů nebo zálohování klíče, nastavení hello phone nečinnosti nebo přechod do jiné aplikace).</span><span class="sxs-lookup"><span data-stu-id="a1ebd-209">By default, a session is ended 10s after hello end of its last activity (which usually occurs by pressing hello Home or Back key, by setting hello phone idle or by jumping into another application).</span></span> <span data-ttu-id="a1ebd-210">Toto je tooavoid rozdělení relace každý uživatel hello čas ukončení a velmi rychle vrátit toohello aplikace (který může dojít při mu vyzvednutí bitovou kopii, zkontrolujte, zda oznámení atd.).</span><span class="sxs-lookup"><span data-stu-id="a1ebd-210">This is tooavoid a session split each time hello user exit and return toohello application very quickly (which can happen when he pick up a image, check a notification, etc.).</span></span> <span data-ttu-id="a1ebd-211">Můžete chtít toomodify tento parametr.</span><span class="sxs-lookup"><span data-stu-id="a1ebd-211">You may want toomodify this parameter.</span></span> <span data-ttu-id="a1ebd-212">toodo Ano, přidejte tuto (mezi hello `<application>` a `</application>` značky):</span><span class="sxs-lookup"><span data-stu-id="a1ebd-212">toodo so, add this (between hello `<application>` and `</application>` tags):</span></span>

            <meta-data android:name="engagement:sessionTimeout" android:value="{session timeout (in milliseconds)}"/>

## <a name="disable-log-reporting"></a><span data-ttu-id="a1ebd-213">Zakázání zasílání zpráv protokolu</span><span class="sxs-lookup"><span data-stu-id="a1ebd-213">Disable log reporting</span></span>
### <a name="using-a-method-call"></a><span data-ttu-id="a1ebd-214">Pomocí volání metody</span><span class="sxs-lookup"><span data-stu-id="a1ebd-214">Using a method call</span></span>
<span data-ttu-id="a1ebd-215">Pokud chcete Engagement toostop odesílání protokolů, můžete volat:</span><span class="sxs-lookup"><span data-stu-id="a1ebd-215">If you want Engagement toostop sending logs, you can call:</span></span>

            EngagementAgent.getInstance(context).setEnabled(false);

<span data-ttu-id="a1ebd-216">Toto volání je trvalé: používá soubor sdílet předvolby.</span><span class="sxs-lookup"><span data-stu-id="a1ebd-216">This call is persistent: it uses a shared preferences file.</span></span>

<span data-ttu-id="a1ebd-217">Pokud je při volání této funkce aktivní zapojení, může trvat 1 minutu po dobu toostop služby hello.</span><span class="sxs-lookup"><span data-stu-id="a1ebd-217">If Engagement is active when you call this function, it may take 1 minute for hello service toostop.</span></span> <span data-ttu-id="a1ebd-218">Ale nespustí hello služby všechny hello příštím spuštění aplikace hello.</span><span class="sxs-lookup"><span data-stu-id="a1ebd-218">However it won't launch hello service at all hello next time you launch hello application.</span></span>

<span data-ttu-id="a1ebd-219">Můžete povolit protokol reporting znovu voláním hello stejné fungovat s `true`.</span><span class="sxs-lookup"><span data-stu-id="a1ebd-219">You can enable log reporting again by calling hello same function with `true`.</span></span>

### <a name="integration-in-your-own-preferenceactivity"></a><span data-ttu-id="a1ebd-220">Integrace ve vašem vlastním`PreferenceActivity`</span><span class="sxs-lookup"><span data-stu-id="a1ebd-220">Integration in your own `PreferenceActivity`</span></span>
<span data-ttu-id="a1ebd-221">Namísto volání této funkce, můžete také integrovat toto nastavení přímo do existující `PreferenceActivity`.</span><span class="sxs-lookup"><span data-stu-id="a1ebd-221">Instead of calling this function, you can also integrate this setting directly in your existing `PreferenceActivity`.</span></span>

<span data-ttu-id="a1ebd-222">Zapojení toouse vaše předvolby soubor (s požadovanou režim hello) můžete nakonfigurovat v hello `AndroidManifest.xml` soubor s `application meta-data`:</span><span class="sxs-lookup"><span data-stu-id="a1ebd-222">You can configure Engagement toouse your preferences file (with hello desired mode) in hello `AndroidManifest.xml` file with `application meta-data`:</span></span>

* <span data-ttu-id="a1ebd-223">Hello `engagement:agent:settings:name` klíč je použité toodefine hello název souboru sdílet předvolby hello.</span><span class="sxs-lookup"><span data-stu-id="a1ebd-223">hello `engagement:agent:settings:name` key is used toodefine hello name of hello shared preferences file.</span></span>
* <span data-ttu-id="a1ebd-224">Hello `engagement:agent:settings:mode` klíč je použité toodefine hello režim souboru hello sdílet předvolby, měli byste použít hello stejný režim jako v vaší `PreferenceActivity`.</span><span class="sxs-lookup"><span data-stu-id="a1ebd-224">hello `engagement:agent:settings:mode` key is used toodefine hello mode of hello shared preferences file, you should use hello same mode as in your `PreferenceActivity`.</span></span> <span data-ttu-id="a1ebd-225">Hello režimu, musí být předán jako číslo: Pokud používáte kombinaci konstantní příznaky ve vašem kódu, zkontrolujte hello celkové hodnoty.</span><span class="sxs-lookup"><span data-stu-id="a1ebd-225">hello mode must be passed as a number: if you are using a combination of constant flags in your code, check hello total value.</span></span>

<span data-ttu-id="a1ebd-226">Zapojení vždy použít hello `engagement:key` boolean klíč v souboru předvoleb hello ke správě tohoto nastavení.</span><span class="sxs-lookup"><span data-stu-id="a1ebd-226">Engagement always use hello `engagement:key` boolean key within hello preferences file for managing this setting.</span></span>

<span data-ttu-id="a1ebd-227">Následující příklad Hello `AndroidManifest.xml` ukazuje hello výchozí hodnoty:</span><span class="sxs-lookup"><span data-stu-id="a1ebd-227">hello following example of `AndroidManifest.xml` shows hello default values:</span></span>

            <application>
                [...]
                <meta-data
                  android:name="engagement:agent:settings:name"
                  android:value="engagement.agent" />
                <meta-data
                  android:name="engagement:agent:settings:mode"
                  android:value="0" />

<span data-ttu-id="a1ebd-228">Potom můžete přidat `CheckBoxPreference` v vaše předvolby rozložení jako hello jeden následující:</span><span class="sxs-lookup"><span data-stu-id="a1ebd-228">Then you can add a `CheckBoxPreference` in your preference layout like hello following one:</span></span>

            <CheckBoxPreference
              android:key="engagement:enabled"
              android:defaultValue="true"
              android:title="Use Engagement"
              android:summaryOn="Engagement is enabled."
              android:summaryOff="Engagement is disabled." />

<!-- URLs. -->
[Device API]: http://go.microsoft.com/?linkid=9876094
