---
title: "Umístění vytváření sestav pro Azure Mobile Engagement Android SDK"
description: "Popisuje, jak nakonfigurovat umístění vytváření sestav pro Azure Mobile Engagement Android SDK"
services: mobile-engagement
documentationcenter: mobile
author: piyushjo
manager: erikre
editor: 
ms.assetid: 6cab5ed1-b767-46ac-9f0b-48a4e249d88c
ms.service: mobile-engagement
ms.workload: mobile
ms.tgt_pltfrm: mobile-android
ms.devlang: Java
ms.topic: article
ms.date: 08/12/2016
ms.author: piyushjo;ricksal
ms.openlocfilehash: 777d5719cce505b55dfb61c91dcac7e713b077a9
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="location-reporting-for-azure-mobile-engagement-android-sdk"></a><span data-ttu-id="6d910-103">Umístění vytváření sestav pro Azure Mobile Engagement Android SDK</span><span class="sxs-lookup"><span data-stu-id="6d910-103">Location Reporting for Azure Mobile Engagement Android SDK</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="6d910-104">Android</span><span class="sxs-lookup"><span data-stu-id="6d910-104">Android</span></span>](mobile-engagement-android-integrate-engagement.md)
> 
> 

<span data-ttu-id="6d910-105">Toto téma popisuje postup tvorby sestav pro aplikace Android umístění.</span><span class="sxs-lookup"><span data-stu-id="6d910-105">This topic describes how to do location reporting for your Android application.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="6d910-106">Požadavky</span><span class="sxs-lookup"><span data-stu-id="6d910-106">Prerequisites</span></span>
[!INCLUDE [Prereqs](../../includes/mobile-engagement-android-prereqs.md)]

## <a name="location-reporting"></a><span data-ttu-id="6d910-107">Rozšířené ohlašování polohy</span><span class="sxs-lookup"><span data-stu-id="6d910-107">Location reporting</span></span>
<span data-ttu-id="6d910-108">Pokud chcete umístění třeba ohlásit, je nutné přidat pár řádků konfigurace (mezi `<application>` a `</application>` značek).</span><span class="sxs-lookup"><span data-stu-id="6d910-108">If you want locations to be reported, you need to add a few lines of configuration (between the `<application>` and `</application>` tags).</span></span>

### <a name="lazy-area-location-reporting"></a><span data-ttu-id="6d910-109">Opožděné hlášení umístění oblasti</span><span class="sxs-lookup"><span data-stu-id="6d910-109">Lazy area location reporting</span></span>
<span data-ttu-id="6d910-110">Opožděné hlášení umístění oblasti umožňuje vytváření sestav zemi, oblast a polohu přidružené k zařízení.</span><span class="sxs-lookup"><span data-stu-id="6d910-110">Lazy area location reporting enables reporting the country, region, and locality associated with devices.</span></span> <span data-ttu-id="6d910-111">Tento typ hlášení umístění používá jenom síťová umístění (na základě ID buňky nebo Wi-Fi).</span><span class="sxs-lookup"><span data-stu-id="6d910-111">This type of location reporting only uses network locations (based on Cell ID or WIFI).</span></span> <span data-ttu-id="6d910-112">Oblasti zařízení se hlásí nejvíce jednou za relace.</span><span class="sxs-lookup"><span data-stu-id="6d910-112">The device area is reported at most once per session.</span></span> <span data-ttu-id="6d910-113">GPS se nikdy nepoužívá, a proto tento typ umístění sestavy má nízkou dopad na baterii.</span><span class="sxs-lookup"><span data-stu-id="6d910-113">The GPS is never used, and thus this type of location report has low impact on the battery.</span></span>

<span data-ttu-id="6d910-114">Hlášené oblasti se používají k výpočtu geografické statistické údaje o uživatelích, relacích, události a chyby.</span><span class="sxs-lookup"><span data-stu-id="6d910-114">Reported areas are used to compute geographic statistics about users, sessions, events, and errors.</span></span> <span data-ttu-id="6d910-115">Může být také používány jako kritérium v kampaně Reach.</span><span class="sxs-lookup"><span data-stu-id="6d910-115">They can also be used as criterion in Reach campaigns.</span></span>

<span data-ttu-id="6d910-116">Umístění opožděné oblasti vytváření sestav pomocí nástroje Konfigurace výše v tomto postupu povolíte:</span><span class="sxs-lookup"><span data-stu-id="6d910-116">You enable lazy area location reporting by using the configuration previously mentioned in this procedure:</span></span>

    EngagementConfiguration engagementConfiguration = new EngagementConfiguration();
    engagementConfiguration.setConnectionString("Endpoint={appCollection}.{domain};AppId={appId};SdkKey={sdkKey}");
    engagementConfiguration.setLazyAreaLocationReport(true);
    EngagementAgent.getInstance(this).init(engagementConfiguration);

<span data-ttu-id="6d910-117">Budete taky muset zadat umístění oprávnění.</span><span class="sxs-lookup"><span data-stu-id="6d910-117">You also need to specify a location permission.</span></span> <span data-ttu-id="6d910-118">Tento kód používá ``COARSE`` oprávnění:</span><span class="sxs-lookup"><span data-stu-id="6d910-118">This code uses ``COARSE`` permission:</span></span>

    <uses-permission android:name="android.permission.ACCESS_COARSE_LOCATION"/>

<span data-ttu-id="6d910-119">Pokud vaše aplikace vyžaduje, můžete použít ``ACCESS_FINE_LOCATION`` místo.</span><span class="sxs-lookup"><span data-stu-id="6d910-119">If your app requires it, you can use ``ACCESS_FINE_LOCATION`` instead.</span></span>

### <a name="real-time-location-reporting"></a><span data-ttu-id="6d910-120">Hlášení polohy v reálném čase</span><span class="sxs-lookup"><span data-stu-id="6d910-120">Real-time location reporting</span></span>
<span data-ttu-id="6d910-121">Hlášení polohy v reálném čase umožňuje vytváření sestav pro zeměpisnou šířku a zeměpisnou délku přidružené k zařízení.</span><span class="sxs-lookup"><span data-stu-id="6d910-121">Real-time location reporting enables reporting the latitude and longitude associated with devices.</span></span> <span data-ttu-id="6d910-122">Ve výchozím nastavení používá tento typ hlášení polohy jenom umístění v síti, na základě ID buňky nebo Wi-Fi.</span><span class="sxs-lookup"><span data-stu-id="6d910-122">By default, this type of location reporting only uses network locations, based on Cell ID or WIFI.</span></span> <span data-ttu-id="6d910-123">Generování sestav je aktivní pouze v případě, že je aplikace spuštěná v popředí (například během relace).</span><span class="sxs-lookup"><span data-stu-id="6d910-123">The reporting is only active when the application runs in foreground (for example, during a session).</span></span>

<span data-ttu-id="6d910-124">V reálném čase umístění jsou *není* slouží k výpočtu statistiky.</span><span class="sxs-lookup"><span data-stu-id="6d910-124">Real-time locations are *NOT* used to compute statistics.</span></span> <span data-ttu-id="6d910-125">Jejich jediným účelem je umožnit použití v reálném čase geografického vymezení \<monitorování geografických Reach-cílové skupiny zón\> kritérium v kampaně Reach.</span><span class="sxs-lookup"><span data-stu-id="6d910-125">Their only purpose is to allow the use of real-time geo-fencing \<Reach-Audience-geofencing\> criterion in Reach campaigns.</span></span>

<span data-ttu-id="6d910-126">Povolit v reálném čase umístění vytváření sestav, přidejte řádek kódu kde nastavit připojovací řetězec Engagement v aktivitě Spouštěče.</span><span class="sxs-lookup"><span data-stu-id="6d910-126">To enable real-time location reporting, add a line of code to where you set the Engagement connection string in the launcher activity.</span></span> <span data-ttu-id="6d910-127">Výsledek vypadá takto:</span><span class="sxs-lookup"><span data-stu-id="6d910-127">The result looks like the following:</span></span>

    EngagementConfiguration engagementConfiguration = new EngagementConfiguration();
    engagementConfiguration.setConnectionString("Endpoint={appCollection}.{domain};AppId={appId};SdkKey={sdkKey}");
    engagementConfiguration.setRealtimeLocationReport(true);
    EngagementAgent.getInstance(this).init(engagementConfiguration);

        You also need to specify a location permission. This code uses ``COARSE`` permission:

            <uses-permission android:name="android.permission.ACCESS_COARSE_LOCATION"/>

        If your app requires it, you can use ``ACCESS_FINE_LOCATION`` instead.

#### <a name="gps-based-reporting"></a><span data-ttu-id="6d910-128">Na základě GPS při vytváření sestav</span><span class="sxs-lookup"><span data-stu-id="6d910-128">GPS based reporting</span></span>
<span data-ttu-id="6d910-129">Ve výchozím nastavení hlášení polohy v reálném čase pouze používá síťové umístění.</span><span class="sxs-lookup"><span data-stu-id="6d910-129">By default, real-time location reporting only uses network-based locations.</span></span> <span data-ttu-id="6d910-130">Pokud chcete povolit používání na základě GPS umístění, které jsou mnohem přesnější, použijte objekt konfigurace:</span><span class="sxs-lookup"><span data-stu-id="6d910-130">To enable the use of GPS-based locations, which are far more precise, use the configuration object:</span></span>

    EngagementConfiguration engagementConfiguration = new EngagementConfiguration();
    engagementConfiguration.setConnectionString("Endpoint={appCollection}.{domain};AppId={appId};SdkKey={sdkKey}");
    engagementConfiguration.setRealtimeLocationReport(true);
    engagementConfiguration.setFineRealtimeLocationReport(true);
    EngagementAgent.getInstance(this).init(engagementConfiguration);

<span data-ttu-id="6d910-131">Musíte taky Pokud chybí, přidejte následující oprávnění:</span><span class="sxs-lookup"><span data-stu-id="6d910-131">You also need to add the following permission if missing:</span></span>

    <uses-permission android:name="android.permission.ACCESS_FINE_LOCATION"/>

#### <a name="background-reporting"></a><span data-ttu-id="6d910-132">Vytváření sestav pozadí</span><span class="sxs-lookup"><span data-stu-id="6d910-132">Background reporting</span></span>
<span data-ttu-id="6d910-133">Ve výchozím nastavení hlášení polohy v reálném čase je aktivní pouze když je aplikace spuštěná v popředí (například během relace).</span><span class="sxs-lookup"><span data-stu-id="6d910-133">By default, real-time location reporting is only active when the application runs in foreground (for example, during a session).</span></span> <span data-ttu-id="6d910-134">Pokud chcete povolit generování sestav také v pozadí, použijte tento objekt konfigurace:</span><span class="sxs-lookup"><span data-stu-id="6d910-134">To enable the reporting also in background, use this configuration object:</span></span>

    EngagementConfiguration engagementConfiguration = new EngagementConfiguration();
    engagementConfiguration.setConnectionString("Endpoint={appCollection}.{domain};AppId={appId};SdkKey={sdkKey}");
    engagementConfiguration.setRealtimeLocationReport(true);
    engagementConfiguration.setBackgroundRealtimeLocationReport(true);
    EngagementAgent.getInstance(this).init(engagementConfiguration);

> [!NOTE]
> <span data-ttu-id="6d910-135">Když je aplikace spuštěná v pozadí, nahlásí pouze na základě síťové umístění, i když je povolená GPS.</span><span class="sxs-lookup"><span data-stu-id="6d910-135">When the application runs in background, only network-based locations are reported, even if you enabled the GPS.</span></span>
> 
> 

<span data-ttu-id="6d910-136">Pokud uživatel restartuje jejich zařízení, je zastavena sestavu umístění pozadí.</span><span class="sxs-lookup"><span data-stu-id="6d910-136">If the user reboots their device, the background location report is stopped.</span></span> <span data-ttu-id="6d910-137">Chcete-li restartovat automaticky při spuštění, přidejte tento kód.</span><span class="sxs-lookup"><span data-stu-id="6d910-137">To make it automatically restart at boot time, add this code.</span></span>

    <receiver android:name="com.microsoft.azure.engagement.EngagementLocationBootReceiver"
           android:exported="false">
        <intent-filter>
            <action android:name="android.intent.action.BOOT_COMPLETED" />
        </intent-filter>
    </receiver>

<span data-ttu-id="6d910-138">Musíte taky Pokud chybí, přidejte následující oprávnění:</span><span class="sxs-lookup"><span data-stu-id="6d910-138">You also need to add the following permission if missing:</span></span>

    <uses-permission android:name="android.permission.RECEIVE_BOOT_COMPLETED" />

## <a name="android-m-permissions"></a><span data-ttu-id="6d910-139">Android M oprávnění</span><span class="sxs-lookup"><span data-stu-id="6d910-139">Android M permissions</span></span>
<span data-ttu-id="6d910-140">Od verze Android M, některá oprávnění jsou spravovány v době běhu a vyžadují schválení uživatele.</span><span class="sxs-lookup"><span data-stu-id="6d910-140">Starting with Android M, some permissions are managed at runtime and need user approval.</span></span>

<span data-ttu-id="6d910-141">Pokud cílíte na úrovni rozhraní API systému Android 23, oprávnění runtime jsou ve výchozím nastavení vypnuté pro nové instalace aplikace.</span><span class="sxs-lookup"><span data-stu-id="6d910-141">If you target Android API level 23, the runtime permissions are turned off by default for new app installations.</span></span> <span data-ttu-id="6d910-142">Jinak jsou zapnuty ve výchozím nastavení.</span><span class="sxs-lookup"><span data-stu-id="6d910-142">Otherwise they are turned on by default.</span></span>

<span data-ttu-id="6d910-143">Vám může povolit nebo zakázat tato oprávnění z nabídky nastavení zařízení.</span><span class="sxs-lookup"><span data-stu-id="6d910-143">You can enable/disable those permissions from the device settings menu.</span></span> <span data-ttu-id="6d910-144">Vypnutí oprávnění z nabídky systém ukončuje procesy na pozadí aplikace, kterou je chování systému a nemá žádný vliv na schopnost přijímat nabízená v pozadí.</span><span class="sxs-lookup"><span data-stu-id="6d910-144">Turning off permissions from the system menu kills the background processes of the application, which is a system behavior, and has no impact on ability to receive push in background.</span></span>

<span data-ttu-id="6d910-145">V rámci umístění Mobile Engagement vytváření sestav oprávnění, která vyžadují schválení za běhu jsou:</span><span class="sxs-lookup"><span data-stu-id="6d910-145">In the context of Mobile Engagement location reporting, the permissions that require approval at runtime are:</span></span>

* `ACCESS_COARSE_LOCATION`
* `ACCESS_FINE_LOCATION`

<span data-ttu-id="6d910-146">Požádat o oprávnění z uživatele s využitím systému standardní dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="6d910-146">Request permissions from the user using a standard system dialog.</span></span> <span data-ttu-id="6d910-147">Pokud uživatel schválí, řekněte ``EngagementAgent`` má provést tato změna v úvahu při v reálném čase.</span><span class="sxs-lookup"><span data-stu-id="6d910-147">If the user approves, tell ``EngagementAgent`` to take that change into account in real-time.</span></span> <span data-ttu-id="6d910-148">V opačném případě změna zpracování při příštím uživatel spustí aplikaci.</span><span class="sxs-lookup"><span data-stu-id="6d910-148">Otherwise the change is processed the next time the user launches the application.</span></span>

<span data-ttu-id="6d910-149">Zde je ukázka kódu pro použití v aktivitě aplikace žádostí o oprávnění a předání výsledek, pokud je kladné k ``EngagementAgent``:</span><span class="sxs-lookup"><span data-stu-id="6d910-149">Here is a code sample to use in an activity of your application to request permissions and forward the result if positive to ``EngagementAgent``:</span></span>

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
         * Request location permission, but this doesn't explain why it is needed to the user.
         * The standard Android documentation explains with more details how to display a rationale activity to explain the user why the permission is needed in your application.
         * Putting COARSE vs FINE has no impact here, they are part of the same group for runtime permission management.
         */
        if (checkSelfPermission(android.Manifest.permission.ACCESS_FINE_LOCATION) != PackageManager.PERMISSION_GRANTED)
          requestPermissions(new String[] { android.Manifest.permission.ACCESS_FINE_LOCATION }, 0);

      }
    }

    @Override
    public void onRequestPermissionsResult(int requestCode, String[] permissions, int[] grantResults)
    {
      /* Only a positive location permission update requires engagement agent refresh, hence the request code matching from above function */
      if (requestCode == 0 && grantResults[0] == PackageManager.PERMISSION_GRANTED)
        getEngagementAgent().refreshPermissions();
    }
