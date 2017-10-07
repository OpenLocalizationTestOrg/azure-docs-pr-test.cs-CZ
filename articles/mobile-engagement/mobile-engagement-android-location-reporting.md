---
title: "aaaLocation vytváření sestav pro Azure Mobile Engagement Android SDK"
description: "Popisuje, jak umístění tooconfigure vytváření sestav pro Azure Mobile Engagement Android SDK"
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
ms.openlocfilehash: c2cb097df2a77bee2d56ffe9509dc116548db408
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="location-reporting-for-azure-mobile-engagement-android-sdk"></a><span data-ttu-id="5b816-103">Umístění vytváření sestav pro Azure Mobile Engagement Android SDK</span><span class="sxs-lookup"><span data-stu-id="5b816-103">Location Reporting for Azure Mobile Engagement Android SDK</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="5b816-104">Android</span><span class="sxs-lookup"><span data-stu-id="5b816-104">Android</span></span>](mobile-engagement-android-integrate-engagement.md)
> 
> 

<span data-ttu-id="5b816-105">Toto téma popisuje, jak umístění toodo vytváření sestav pro aplikace pro Android.</span><span class="sxs-lookup"><span data-stu-id="5b816-105">This topic describes how toodo location reporting for your Android application.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="5b816-106">Požadavky</span><span class="sxs-lookup"><span data-stu-id="5b816-106">Prerequisites</span></span>
[!INCLUDE [Prereqs](../../includes/mobile-engagement-android-prereqs.md)]

## <a name="location-reporting"></a><span data-ttu-id="5b816-107">Rozšířené ohlašování polohy</span><span class="sxs-lookup"><span data-stu-id="5b816-107">Location reporting</span></span>
<span data-ttu-id="5b816-108">Pokud chcete, aby oznámil toobe umístění, je třeba tooadd pár řádků konfigurace (mezi hello `<application>` a `</application>` značek).</span><span class="sxs-lookup"><span data-stu-id="5b816-108">If you want locations toobe reported, you need tooadd a few lines of configuration (between hello `<application>` and `</application>` tags).</span></span>

### <a name="lazy-area-location-reporting"></a><span data-ttu-id="5b816-109">Opožděné hlášení umístění oblasti</span><span class="sxs-lookup"><span data-stu-id="5b816-109">Lazy area location reporting</span></span>
<span data-ttu-id="5b816-110">Opožděné hlášení umístění oblasti umožňuje vytváření sestav hello zemi, oblast a polohu přidružené k zařízení.</span><span class="sxs-lookup"><span data-stu-id="5b816-110">Lazy area location reporting enables reporting hello country, region, and locality associated with devices.</span></span> <span data-ttu-id="5b816-111">Tento typ hlášení umístění používá jenom síťová umístění (na základě ID buňky nebo Wi-Fi).</span><span class="sxs-lookup"><span data-stu-id="5b816-111">This type of location reporting only uses network locations (based on Cell ID or WIFI).</span></span> <span data-ttu-id="5b816-112">oblasti Hello zařízení se hlásí nejvíce jednou za relace.</span><span class="sxs-lookup"><span data-stu-id="5b816-112">hello device area is reported at most once per session.</span></span> <span data-ttu-id="5b816-113">Hello GPS se nikdy nepoužívá, a proto tento typ umístění sestavy má nízkou dopad na baterie hello.</span><span class="sxs-lookup"><span data-stu-id="5b816-113">hello GPS is never used, and thus this type of location report has low impact on hello battery.</span></span>

<span data-ttu-id="5b816-114">Hlášené oblasti jsou použité toocompute geografické Statistika týkající se uživatelů, relací, události a chyby.</span><span class="sxs-lookup"><span data-stu-id="5b816-114">Reported areas are used toocompute geographic statistics about users, sessions, events, and errors.</span></span> <span data-ttu-id="5b816-115">Může být také používány jako kritérium v kampaně Reach.</span><span class="sxs-lookup"><span data-stu-id="5b816-115">They can also be used as criterion in Reach campaigns.</span></span>

<span data-ttu-id="5b816-116">Umístění opožděné oblasti vytváření sestav pomocí nástroje Konfigurace hello výše v tomto postupu povolíte:</span><span class="sxs-lookup"><span data-stu-id="5b816-116">You enable lazy area location reporting by using hello configuration previously mentioned in this procedure:</span></span>

    EngagementConfiguration engagementConfiguration = new EngagementConfiguration();
    engagementConfiguration.setConnectionString("Endpoint={appCollection}.{domain};AppId={appId};SdkKey={sdkKey}");
    engagementConfiguration.setLazyAreaLocationReport(true);
    EngagementAgent.getInstance(this).init(engagementConfiguration);

<span data-ttu-id="5b816-117">Budete také potřebovat toospecify oprávnění umístění.</span><span class="sxs-lookup"><span data-stu-id="5b816-117">You also need toospecify a location permission.</span></span> <span data-ttu-id="5b816-118">Tento kód používá ``COARSE`` oprávnění:</span><span class="sxs-lookup"><span data-stu-id="5b816-118">This code uses ``COARSE`` permission:</span></span>

    <uses-permission android:name="android.permission.ACCESS_COARSE_LOCATION"/>

<span data-ttu-id="5b816-119">Pokud vaše aplikace vyžaduje, můžete použít ``ACCESS_FINE_LOCATION`` místo.</span><span class="sxs-lookup"><span data-stu-id="5b816-119">If your app requires it, you can use ``ACCESS_FINE_LOCATION`` instead.</span></span>

### <a name="real-time-location-reporting"></a><span data-ttu-id="5b816-120">Hlášení polohy v reálném čase</span><span class="sxs-lookup"><span data-stu-id="5b816-120">Real-time location reporting</span></span>
<span data-ttu-id="5b816-121">Hlášení polohy v reálném čase umožňuje vytváření sestav hello šířky a délky, které jsou přidružené k zařízení.</span><span class="sxs-lookup"><span data-stu-id="5b816-121">Real-time location reporting enables reporting hello latitude and longitude associated with devices.</span></span> <span data-ttu-id="5b816-122">Ve výchozím nastavení používá tento typ hlášení polohy jenom umístění v síti, na základě ID buňky nebo Wi-Fi.</span><span class="sxs-lookup"><span data-stu-id="5b816-122">By default, this type of location reporting only uses network locations, based on Cell ID or WIFI.</span></span> <span data-ttu-id="5b816-123">Hello reporting je aktivní jenom v případě hello aplikace spuštěná v popředí (například během relace).</span><span class="sxs-lookup"><span data-stu-id="5b816-123">hello reporting is only active when hello application runs in foreground (for example, during a session).</span></span>

<span data-ttu-id="5b816-124">V reálném čase umístění jsou *není* používá toocompute statistiky.</span><span class="sxs-lookup"><span data-stu-id="5b816-124">Real-time locations are *NOT* used toocompute statistics.</span></span> <span data-ttu-id="5b816-125">Jejich jediným účelem je použití hello tooallow v reálném čase geografického vymezení \<monitorování geografických Reach-cílové skupiny zón\> kritérium v kampaně Reach.</span><span class="sxs-lookup"><span data-stu-id="5b816-125">Their only purpose is tooallow hello use of real-time geo-fencing \<Reach-Audience-geofencing\> criterion in Reach campaigns.</span></span>

<span data-ttu-id="5b816-126">umístění v reálném čase tooenable vytváření sestav, přidejte řádek z kódu toowhere nastavíte hello Engagement připojovací řetězec v aktivitě Spouštěče hello.</span><span class="sxs-lookup"><span data-stu-id="5b816-126">tooenable real-time location reporting, add a line of code toowhere you set hello Engagement connection string in hello launcher activity.</span></span> <span data-ttu-id="5b816-127">výsledek Hello vypadá hello následující:</span><span class="sxs-lookup"><span data-stu-id="5b816-127">hello result looks like hello following:</span></span>

    EngagementConfiguration engagementConfiguration = new EngagementConfiguration();
    engagementConfiguration.setConnectionString("Endpoint={appCollection}.{domain};AppId={appId};SdkKey={sdkKey}");
    engagementConfiguration.setRealtimeLocationReport(true);
    EngagementAgent.getInstance(this).init(engagementConfiguration);

        You also need toospecify a location permission. This code uses ``COARSE`` permission:

            <uses-permission android:name="android.permission.ACCESS_COARSE_LOCATION"/>

        If your app requires it, you can use ``ACCESS_FINE_LOCATION`` instead.

#### <a name="gps-based-reporting"></a><span data-ttu-id="5b816-128">Na základě GPS při vytváření sestav</span><span class="sxs-lookup"><span data-stu-id="5b816-128">GPS based reporting</span></span>
<span data-ttu-id="5b816-129">Ve výchozím nastavení hlášení polohy v reálném čase pouze používá síťové umístění.</span><span class="sxs-lookup"><span data-stu-id="5b816-129">By default, real-time location reporting only uses network-based locations.</span></span> <span data-ttu-id="5b816-130">použití hello tooenable na základě GPS umístění, které jsou mnohem přesnější, použijte objekt konfigurace hello:</span><span class="sxs-lookup"><span data-stu-id="5b816-130">tooenable hello use of GPS-based locations, which are far more precise, use hello configuration object:</span></span>

    EngagementConfiguration engagementConfiguration = new EngagementConfiguration();
    engagementConfiguration.setConnectionString("Endpoint={appCollection}.{domain};AppId={appId};SdkKey={sdkKey}");
    engagementConfiguration.setRealtimeLocationReport(true);
    engagementConfiguration.setFineRealtimeLocationReport(true);
    EngagementAgent.getInstance(this).init(engagementConfiguration);

<span data-ttu-id="5b816-131">Musíte taky tooadd hello, pokud chybí následující oprávnění:</span><span class="sxs-lookup"><span data-stu-id="5b816-131">You also need tooadd hello following permission if missing:</span></span>

    <uses-permission android:name="android.permission.ACCESS_FINE_LOCATION"/>

#### <a name="background-reporting"></a><span data-ttu-id="5b816-132">Vytváření sestav pozadí</span><span class="sxs-lookup"><span data-stu-id="5b816-132">Background reporting</span></span>
<span data-ttu-id="5b816-133">Ve výchozím nastavení hlášení polohy v reálném čase je aktivní pouze při spuštění aplikace hello v popředí (například během relace).</span><span class="sxs-lookup"><span data-stu-id="5b816-133">By default, real-time location reporting is only active when hello application runs in foreground (for example, during a session).</span></span> <span data-ttu-id="5b816-134">hello tooenable také vytváření sestav v pozadí, použijte tento objekt konfigurace:</span><span class="sxs-lookup"><span data-stu-id="5b816-134">tooenable hello reporting also in background, use this configuration object:</span></span>

    EngagementConfiguration engagementConfiguration = new EngagementConfiguration();
    engagementConfiguration.setConnectionString("Endpoint={appCollection}.{domain};AppId={appId};SdkKey={sdkKey}");
    engagementConfiguration.setRealtimeLocationReport(true);
    engagementConfiguration.setBackgroundRealtimeLocationReport(true);
    EngagementAgent.getInstance(this).init(engagementConfiguration);

> [!NOTE]
> <span data-ttu-id="5b816-135">Při spuštění aplikace hello v pozadí, jsou hlášeny pouze na základě síťové umístění, i když jste povolili hello GPS.</span><span class="sxs-lookup"><span data-stu-id="5b816-135">When hello application runs in background, only network-based locations are reported, even if you enabled hello GPS.</span></span>
> 
> 

<span data-ttu-id="5b816-136">Pokud uživatel hello restartuje jejich zařízení, je zastavena hello pozadí umístění sestavy.</span><span class="sxs-lookup"><span data-stu-id="5b816-136">If hello user reboots their device, hello background location report is stopped.</span></span> <span data-ttu-id="5b816-137">toomake automaticky restartuje při spuštění, přidejte tento kód.</span><span class="sxs-lookup"><span data-stu-id="5b816-137">toomake it automatically restart at boot time, add this code.</span></span>

    <receiver android:name="com.microsoft.azure.engagement.EngagementLocationBootReceiver"
           android:exported="false">
        <intent-filter>
            <action android:name="android.intent.action.BOOT_COMPLETED" />
        </intent-filter>
    </receiver>

<span data-ttu-id="5b816-138">Musíte taky tooadd hello, pokud chybí následující oprávnění:</span><span class="sxs-lookup"><span data-stu-id="5b816-138">You also need tooadd hello following permission if missing:</span></span>

    <uses-permission android:name="android.permission.RECEIVE_BOOT_COMPLETED" />

## <a name="android-m-permissions"></a><span data-ttu-id="5b816-139">Android M oprávnění</span><span class="sxs-lookup"><span data-stu-id="5b816-139">Android M permissions</span></span>
<span data-ttu-id="5b816-140">Od verze Android M, některá oprávnění jsou spravovány v době běhu a vyžadují schválení uživatele.</span><span class="sxs-lookup"><span data-stu-id="5b816-140">Starting with Android M, some permissions are managed at runtime and need user approval.</span></span>

<span data-ttu-id="5b816-141">Pokud cílíte na úrovni rozhraní API systému Android 23, oprávnění hello runtime jsou ve výchozím nastavení vypnuté pro nové instalace aplikace.</span><span class="sxs-lookup"><span data-stu-id="5b816-141">If you target Android API level 23, hello runtime permissions are turned off by default for new app installations.</span></span> <span data-ttu-id="5b816-142">Jinak jsou zapnuty ve výchozím nastavení.</span><span class="sxs-lookup"><span data-stu-id="5b816-142">Otherwise they are turned on by default.</span></span>

<span data-ttu-id="5b816-143">Vám může povolit nebo zakázat tato oprávnění z nabídky nastavení zařízení hello.</span><span class="sxs-lookup"><span data-stu-id="5b816-143">You can enable/disable those permissions from hello device settings menu.</span></span> <span data-ttu-id="5b816-144">Vypnutí oprávnění z nabídky systému hello ukončí procesy na pozadí hello hello aplikace, která je chování systému a nemá žádný vliv na schopnost tooreceive nabízení v pozadí.</span><span class="sxs-lookup"><span data-stu-id="5b816-144">Turning off permissions from hello system menu kills hello background processes of hello application, which is a system behavior, and has no impact on ability tooreceive push in background.</span></span>

<span data-ttu-id="5b816-145">V kontextu hello Mobile Engagement umístění reporting hello oprávnění, které vyžadují schválení za běhu jsou:</span><span class="sxs-lookup"><span data-stu-id="5b816-145">In hello context of Mobile Engagement location reporting, hello permissions that require approval at runtime are:</span></span>

* `ACCESS_COARSE_LOCATION`
* `ACCESS_FINE_LOCATION`

<span data-ttu-id="5b816-146">Požádat o oprávnění z hello uživatele pomocí dialogu standardní systému.</span><span class="sxs-lookup"><span data-stu-id="5b816-146">Request permissions from hello user using a standard system dialog.</span></span> <span data-ttu-id="5b816-147">Pokud uživatel hello schválí, řekněte ``EngagementAgent`` tootake, který změnit v úvahu v reálném čase.</span><span class="sxs-lookup"><span data-stu-id="5b816-147">If hello user approves, tell ``EngagementAgent`` tootake that change into account in real-time.</span></span> <span data-ttu-id="5b816-148">V opačném případě změnu hello je zpracovaná hello další čas hello uživatel spustí hello aplikace.</span><span class="sxs-lookup"><span data-stu-id="5b816-148">Otherwise hello change is processed hello next time hello user launches hello application.</span></span>

<span data-ttu-id="5b816-149">Tady je toouse ukázkový kód v aktivitě toorequest oprávnění aplikací a k dopředného hello, pokud kladné příliš``EngagementAgent``:</span><span class="sxs-lookup"><span data-stu-id="5b816-149">Here is a code sample toouse in an activity of your application toorequest permissions and forward hello result if positive too``EngagementAgent``:</span></span>

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
         * Request location permission, but this doesn't explain why it is needed toohello user.
         * hello standard Android documentation explains with more details how toodisplay a rationale activity tooexplain hello user why hello permission is needed in your application.
         * Putting COARSE vs FINE has no impact here, they are part of hello same group for runtime permission management.
         */
        if (checkSelfPermission(android.Manifest.permission.ACCESS_FINE_LOCATION) != PackageManager.PERMISSION_GRANTED)
          requestPermissions(new String[] { android.Manifest.permission.ACCESS_FINE_LOCATION }, 0);

      }
    }

    @Override
    public void onRequestPermissionsResult(int requestCode, String[] permissions, int[] grantResults)
    {
      /* Only a positive location permission update requires engagement agent refresh, hence hello request code matching from above function */
      if (requestCode == 0 && grantResults[0] == PackageManager.PERMISSION_GRANTED)
        getEngagementAgent().refreshPermissions();
    }
