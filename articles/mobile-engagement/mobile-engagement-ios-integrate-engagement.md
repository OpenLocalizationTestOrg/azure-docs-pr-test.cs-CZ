---
title: Azure Mobile Engagement iOS SDK integrace | Microsoft Docs
description: "Nejnovější aktualizace a postupy pro iOS SDK pro Azure Mobile Engagement"
services: mobile-engagement
documentationcenter: mobile
author: piyushjo
manager: erikre
editor: 
ms.assetid: 947ea44b-00c1-450f-9a3b-74437954dc56
ms.service: mobile-engagement
ms.workload: mobile
ms.tgt_pltfrm: mobile-ios
ms.devlang: objective-c
ms.topic: article
ms.date: 07/17/2017
ms.author: piyushjo
ms.openlocfilehash: 01fdbb43c21ac6932e8462f4a6507fc63e50542d
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="how-to-integrate-engagement-on-ios"></a><span data-ttu-id="0b551-103">Jak integrovat Engagement v systému iOS</span><span class="sxs-lookup"><span data-stu-id="0b551-103">How to Integrate Engagement on iOS</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="0b551-104">Univerzální pro Windows</span><span class="sxs-lookup"><span data-stu-id="0b551-104">Windows Universal</span></span>](mobile-engagement-windows-store-integrate-engagement.md)
> * [<span data-ttu-id="0b551-105">Windows Phone Silverlight</span><span class="sxs-lookup"><span data-stu-id="0b551-105">Windows Phone Silverlight</span></span>](mobile-engagement-windows-phone-integrate-engagement.md)
> * [<span data-ttu-id="0b551-106">iOS</span><span class="sxs-lookup"><span data-stu-id="0b551-106">iOS</span></span>](mobile-engagement-ios-integrate-engagement.md)
> * [<span data-ttu-id="0b551-107">Android</span><span class="sxs-lookup"><span data-stu-id="0b551-107">Android</span></span>](mobile-engagement-android-integrate-engagement.md)
>
>

<span data-ttu-id="0b551-108">Tento postup popisuje nejjednodušší způsob, jak aktivovat Engagement analýzy a monitorování funkcí v aplikace iOS.</span><span class="sxs-lookup"><span data-stu-id="0b551-108">This procedure describes the simplest way to activate Engagement's Analytics and Monitoring functions in your iOS application.</span></span>

<span data-ttu-id="0b551-109">Sady Engagement SDK vyžaduje systému IOS 7 + a Xcode 8 +: cíl nasazení vaší aplikace musí být alespoň iOS 7.</span><span class="sxs-lookup"><span data-stu-id="0b551-109">The Engagement SDK requires iOS7+ and Xcode 8+: the deployment target of your application must be at least iOS 7.</span></span>

> [!NOTE]
> <span data-ttu-id="0b551-110">Pokud jste ve skutečnosti závisí na XCode 7 pak můžete použít [iOS Engagement SDK v3.2.4](https://aka.ms/r6oouh).</span><span class="sxs-lookup"><span data-stu-id="0b551-110">If you really depend on XCode 7 then you may use the [iOS Engagement SDK v3.2.4](https://aka.ms/r6oouh).</span></span> <span data-ttu-id="0b551-111">Je známého problému při spuštění v iOS 10 zařízení najdete na modul Reach této předchozí verze [integrační modul reach](mobile-engagement-ios-integrate-engagement-reach.md) další podrobnosti.</span><span class="sxs-lookup"><span data-stu-id="0b551-111">There is a known bug on the Reach module of this previous version while running on iOS 10 devices see [the reach module integration](mobile-engagement-ios-integrate-engagement-reach.md) for more details.</span></span> <span data-ttu-id="0b551-112">Pokud se rozhodnete použít SDK v3.2.4 a z nich přeskočit, `UserNotifications.framework` importovat v dalším kroku.</span><span class="sxs-lookup"><span data-stu-id="0b551-112">If you choose to use the SDK v3.2.4 then just skip the `UserNotifications.framework` import in the next step.</span></span>
>
>

<span data-ttu-id="0b551-113">Následující kroky jsou dost aktivovat sestavy protokolů, které jsou potřebné k výpočtu všechny statistické údaje týkající se uživatelů, relací, aktivity, dojde k chybě a Technicals.</span><span class="sxs-lookup"><span data-stu-id="0b551-113">The following steps are enough to activate the report of logs needed to compute all statistics regarding Users, Sessions, Activities, Crashes and Technicals.</span></span> <span data-ttu-id="0b551-114">Sestava protokolů, které jsou potřebné k výpočtu další statistiky, jako jsou události a chyby úlohy je třeba provést ručně pomocí rozhraní API Engagement (najdete v části [jak používat rozšířené Mobile Engagement označování rozhraní API v aplikaci s iOS](mobile-engagement-ios-use-engagement-api.md) vzhledem k tomu, že se tyto statistiky závislé aplikace.</span><span class="sxs-lookup"><span data-stu-id="0b551-114">The report of logs needed to compute other statistics like Events, Errors and Jobs must be done manually using the Engagement API  (see [How to use the advanced Mobile Engagement tagging API in your iOS app](mobile-engagement-ios-use-engagement-api.md) since these statistics are application dependent.</span></span>

## <a name="embed-the-engagement-sdk-into-your-ios-project"></a><span data-ttu-id="0b551-115">Vložení sady Engagement SDK do projektu iOS</span><span class="sxs-lookup"><span data-stu-id="0b551-115">Embed the Engagement SDK into your iOS project</span></span>
* <span data-ttu-id="0b551-116">Sada SDK iOS stáhnout z [zde](http://aka.ms/qk2rnj).</span><span class="sxs-lookup"><span data-stu-id="0b551-116">Download the iOS SDK from [here](http://aka.ms/qk2rnj).</span></span>
* <span data-ttu-id="0b551-117">Přidejte sady Engagement SDK do projektu iOS: v Xcode, klikněte pravým tlačítkem na projekt a vyberte **"Přidat soubory do..."** a zvolte `EngagementSDK` složky.</span><span class="sxs-lookup"><span data-stu-id="0b551-117">Add the Engagement SDK to your iOS project: in Xcode, right click on your project and select **"Add files to ..."** and choose the `EngagementSDK` folder.</span></span>
* <span data-ttu-id="0b551-118">Engagement vyžaduje další rozhraní pro práci: v prohlížeči projektu otevřete podokno váš projekt a vyberte správný cíl.</span><span class="sxs-lookup"><span data-stu-id="0b551-118">Engagement requires additional frameworks to work: in the project explorer, open your project pane and select the correct target.</span></span> <span data-ttu-id="0b551-119">Potom otevřete **"Fáze sestavení"** kartě a v **"Odkaz binárních souborů a knihoven"** nabídky, přidejte tyto architektury:</span><span class="sxs-lookup"><span data-stu-id="0b551-119">Then, open the **"Build phases"** tab and in the **"Link Binary With Libraries"** menu, add these frameworks:</span></span>

  * <span data-ttu-id="0b551-120">`UserNotifications.framework`– nastavte odkaz jako`Optional`</span><span class="sxs-lookup"><span data-stu-id="0b551-120">`UserNotifications.framework` - set the link as `Optional`</span></span>
  * <span data-ttu-id="0b551-121">`AdSupport.framework`– nastavte odkaz jako`Optional`</span><span class="sxs-lookup"><span data-stu-id="0b551-121">`AdSupport.framework` - set the link as `Optional`</span></span>
  * `SystemConfiguration.framework`
  * `CoreTelephony.framework`
  * `CFNetwork.framework`
  * `CoreLocation.framework`
  * `libxml2.dylib`

> [!NOTE]
> <span data-ttu-id="0b551-122">Rozhraní framework AdSupport lze odebrat.</span><span class="sxs-lookup"><span data-stu-id="0b551-122">The AdSupport framework can be removed.</span></span> <span data-ttu-id="0b551-123">Zapojení musí toto rozhraní ke shromažďování IDFA.</span><span class="sxs-lookup"><span data-stu-id="0b551-123">Engagement needs this framework to collect the IDFA.</span></span> <span data-ttu-id="0b551-124">Nicméně je možné zakázat shromažďování IDFA \<ios sdk engagement idfa\> pro dosažení souladu s novou zásadu Apple týkající se mu ID této.</span><span class="sxs-lookup"><span data-stu-id="0b551-124">However, IDFA collection can be disabled \<ios-sdk-engagement-idfa\> to comply with the new Apple policy regarding this ID.</span></span>
>
>

## <a name="initialize-the-engagement-sdk"></a><span data-ttu-id="0b551-125">Inicializace Engagement SDK</span><span class="sxs-lookup"><span data-stu-id="0b551-125">Initialize the Engagement SDK</span></span>
<span data-ttu-id="0b551-126">Je třeba úprava delegáta aplikace:</span><span class="sxs-lookup"><span data-stu-id="0b551-126">You need to modify your Application Delegate:</span></span>

* <span data-ttu-id="0b551-127">V horní části souboru implementace importujte agenta Engagement:</span><span class="sxs-lookup"><span data-stu-id="0b551-127">At the top of your implementation file, import the Engagement agent:</span></span>

      [...]
      #import "EngagementAgent.h"
* <span data-ttu-id="0b551-128">Inicializace Engagement uvnitř metody '**applicationDidFinishLaunching:**"nebo"**aplikace: didFinishLaunchingWithOptions:**':</span><span class="sxs-lookup"><span data-stu-id="0b551-128">Initialize Engagement inside the method '**applicationDidFinishLaunching:**' or '**application:didFinishLaunchingWithOptions:**':</span></span>

      - (BOOL)application:(UIApplication *)application didFinishLaunchingWithOptions:(NSDictionary *)launchOptions
      {
        [...]
        [EngagementAgent init:@"Endpoint={YOUR_APP_COLLECTION.DOMAIN};SdkKey={YOUR_SDK_KEY};AppId={YOUR_APPID}"];
        [...]
      }

## <a name="basic-reporting"></a><span data-ttu-id="0b551-129">Vytváření základních sestav</span><span class="sxs-lookup"><span data-stu-id="0b551-129">Basic reporting</span></span>
### <a name="recommended-method-overload-your-uiviewcontroller-classes"></a><span data-ttu-id="0b551-130">Doporučená metoda: přetížení vaší `UIViewController` třídy</span><span class="sxs-lookup"><span data-stu-id="0b551-130">Recommended method: overload your `UIViewController` classes</span></span>
<span data-ttu-id="0b551-131">Chcete-li aktivovat sestavu všechny protokoly, které vyžadují zapojení k výpočtu uživatelů, relací, aktivity, dojde k chybě a technické statistiky, můžete jednoduše provést všechny vaše `UIViewController` dílčí třídy dědí `EngagementViewController` třídy (stejného pravidla pro `UITableViewController`  -\> `EngagementTableViewController`).</span><span class="sxs-lookup"><span data-stu-id="0b551-131">In order to activate the report of all the logs required by Engagement to compute Users, Sessions, Activities, Crashes and Technical statistics, you can simply make all your `UIViewController` sub-classes inherit from the `EngagementViewController` classes (same rule for `UITableViewController` -\> `EngagementTableViewController`).</span></span>

<span data-ttu-id="0b551-132">**Bez Engagement:**</span><span class="sxs-lookup"><span data-stu-id="0b551-132">**Without Engagement :**</span></span>

    #import <UIKit/UIKit.h>

    @interface Tab1ViewController : UIViewController<UITextFieldDelegate> {
      UITextField* myTextField1;
      UITextField* myTextField2;
    }

    @property (nonatomic, retain) IBOutlet UITextField* myTextField1;
    @property (nonatomic, retain) IBOutlet UITextField* myTextField2;

<span data-ttu-id="0b551-133">**S Engagement:**</span><span class="sxs-lookup"><span data-stu-id="0b551-133">**With Engagement :**</span></span>

    #import <UIKit/UIKit.h>
    #import "EngagementViewController.h"

    @interface Tab1ViewController : EngagementViewController<UITextFieldDelegate> {
      UITextField* myTextField1;
      UITextField* myTextField2;
    }

    @property (nonatomic, retain) IBOutlet UITextField* myTextField1;
    @property (nonatomic, retain) IBOutlet UITextField* myTextField2;

### <a name="alternate-method-call-startactivity-manually"></a><span data-ttu-id="0b551-134">Alternativní metoda: volání `startActivity()` ručně</span><span class="sxs-lookup"><span data-stu-id="0b551-134">Alternate method: call `startActivity()` manually</span></span>
<span data-ttu-id="0b551-135">Pokud nemůžete nebo nechcete přetížení vaše `UIViewController` třídy, můžete místo toho spustit vaše aktivity voláním `EngagementAgent`na metody přímo.</span><span class="sxs-lookup"><span data-stu-id="0b551-135">If you cannot or do not want to overload your `UIViewController` classes, you can instead start your activities by calling `EngagementAgent`'s methods directly.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="0b551-136">IOS SDK automaticky zavolá `endActivity()` metoda při ukončení aplikace.</span><span class="sxs-lookup"><span data-stu-id="0b551-136">The iOS SDK automatically calls the `endActivity()` method when the application is closed.</span></span> <span data-ttu-id="0b551-137">Z toho důvodu je *vysoce* doporučuje volat `startActivity` metoda vždy, když aktivita uživatele změnit a *nikdy* volání `endActivity` metoda, protože voláním této metody vynutí aktuální relace ukončit.</span><span class="sxs-lookup"><span data-stu-id="0b551-137">Thus, it is *HIGHLY* recommended to call the `startActivity` method whenever the activity of the user change, and to *NEVER* call the `endActivity` method, since calling this method forces the current session to be ended.</span></span>
>
>

## <a name="location-reporting"></a><span data-ttu-id="0b551-138">Rozšířené ohlašování polohy</span><span class="sxs-lookup"><span data-stu-id="0b551-138">Location reporting</span></span>
<span data-ttu-id="0b551-139">Podmínky služby Apple neumožňují aplikacím používat zjišťování za účelem statistiky pouze umístění.</span><span class="sxs-lookup"><span data-stu-id="0b551-139">Apple terms of service do not allow applications to use location tracking for statistics purpose only.</span></span> <span data-ttu-id="0b551-140">Proto se doporučuje povolit umístění sestavy pouze v případě, že vaše aplikace používat také umístění sledování z jiného důvodu.</span><span class="sxs-lookup"><span data-stu-id="0b551-140">Thus, it is recommended to enable location reports only if your application also use the location tracking for another reason.</span></span>

<span data-ttu-id="0b551-141">Od verze iOS 8, je nutné zadat popis jak vaše aplikace používá polohy nastavením řetězec pro klíč [NSLocationWhenInUseUsageDescription] nebo [NSLocationAlwaysUsageDescription]v souboru Info.plist vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="0b551-141">Starting with iOS 8, you must provide a description for how your app uses location services by setting a string for the key [NSLocationWhenInUseUsageDescription] or [NSLocationAlwaysUsageDescription] in your app's Info.plist file.</span></span> <span data-ttu-id="0b551-142">Pokud chcete umístění sestavy na pozadí se zapojení, přidejte klíč NSLocationAlwaysUsageDescription.</span><span class="sxs-lookup"><span data-stu-id="0b551-142">If you want to report location in the background with Engagement, add the key NSLocationAlwaysUsageDescription.</span></span> <span data-ttu-id="0b551-143">Ve všech ostatních případech přidáte klíč NSLocationWhenInUseUsageDescription.</span><span class="sxs-lookup"><span data-stu-id="0b551-143">In all other cases, add the key NSLocationWhenInUseUsageDescription.</span></span> <span data-ttu-id="0b551-144">Všimněte si, že je potřeba NSLocationAlwaysAndWhenInUseUsageDescription a NSLocationWhenInUseUsageDescription umístění pozadí sestavy v systému iOS 11.</span><span class="sxs-lookup"><span data-stu-id="0b551-144">Note that you need both NSLocationAlwaysAndWhenInUseUsageDescription and NSLocationWhenInUseUsageDescription to report background location on iOS 11.</span></span>

### <a name="lazy-area-location-reporting"></a><span data-ttu-id="0b551-145">Opožděné hlášení umístění oblasti</span><span class="sxs-lookup"><span data-stu-id="0b551-145">Lazy area location reporting</span></span>
<span data-ttu-id="0b551-146">Opožděné hlášení umístění oblasti umožňuje podat zprávu zemi, oblast a polohu přidružené k zařízení.</span><span class="sxs-lookup"><span data-stu-id="0b551-146">Lazy area location reporting allows to report the country, region and locality associated to devices.</span></span> <span data-ttu-id="0b551-147">Tento typ hlášení umístění používá jenom síťová umístění (na základě ID buňky nebo Wi-Fi).</span><span class="sxs-lookup"><span data-stu-id="0b551-147">This type of location reporting only uses network locations (based on Cell ID or WIFI).</span></span> <span data-ttu-id="0b551-148">Oblasti zařízení se hlásí nejvíce jednou za relace.</span><span class="sxs-lookup"><span data-stu-id="0b551-148">The device area is reported at most once per session.</span></span> <span data-ttu-id="0b551-149">GPS se nikdy nepoužívá, a proto tento typ umístění sestavy má jen v několika (není k vyslovení ne) dopad na baterii.</span><span class="sxs-lookup"><span data-stu-id="0b551-149">The GPS is never used, and thus this type of location report has very few (not to say no) impact on the battery.</span></span>

<span data-ttu-id="0b551-150">Hlášené oblasti se používají k výpočtu geografické statistické údaje o uživatelích, relacích, události a chyby.</span><span class="sxs-lookup"><span data-stu-id="0b551-150">Reported areas are used to compute geographic statistics about users, sessions, events and errors.</span></span> <span data-ttu-id="0b551-151">Může být také používány jako kritérium v kampaně Reach.</span><span class="sxs-lookup"><span data-stu-id="0b551-151">They can also be used as criterion in Reach campaigns.</span></span> <span data-ttu-id="0b551-152">Oblasti poslední známé ohlásil pro zařízení se dá načíst Poděkování [rozhraní API pro zařízení].</span><span class="sxs-lookup"><span data-stu-id="0b551-152">The last known area reported for a device can be retrieved thanks to the [Device API].</span></span>

<span data-ttu-id="0b551-153">Pokud chcete povolit opožděné hlášení umístění oblasti, přidejte následující řádek po inicializaci agenta Engagement:</span><span class="sxs-lookup"><span data-stu-id="0b551-153">To enable lazy area location reporting, add the following line after initializing the Engagement agent:</span></span>

    - (BOOL)application:(UIApplication *)application didFinishLaunchingWithOptions:(NSDictionary *)launchOptions
    {
      [...]
      [[EngagementAgent shared] setLazyAreaLocationReport:YES];
      [...]
    }

### <a name="real-time-location-reporting"></a><span data-ttu-id="0b551-154">Hlášení polohy reálném čase.</span><span class="sxs-lookup"><span data-stu-id="0b551-154">Real time location reporting</span></span>
<span data-ttu-id="0b551-155">Hlášení polohy reálném čase umožňuje podat zprávu pro zeměpisnou šířku a zeměpisnou délku přidružené k zařízení.</span><span class="sxs-lookup"><span data-stu-id="0b551-155">Real time location reporting allows to report the latitude and longitude associated to devices.</span></span> <span data-ttu-id="0b551-156">Ve výchozím nastavení tento typ hlášení umístění používá jenom síťová umístění (na základě ID buňky nebo Wi-Fi) a generování sestav je aktivní pouze když je aplikace spuštěná v popředí (tj. během relace).</span><span class="sxs-lookup"><span data-stu-id="0b551-156">By default, this type of location reporting only uses network locations (based on Cell ID or WIFI), and the reporting is only active when the application runs in foreground (i.e. during a session).</span></span>

<span data-ttu-id="0b551-157">Umístění v reálném čase, jsou *není* slouží k výpočtu statistiky.</span><span class="sxs-lookup"><span data-stu-id="0b551-157">Real time locations are *NOT* used to compute statistics.</span></span> <span data-ttu-id="0b551-158">Jejich jediným účelem je umožnit použití geografického vymezení reálném čase \<monitorování geografických Reach-cílové skupiny zón\> kritérium v kampaně Reach.</span><span class="sxs-lookup"><span data-stu-id="0b551-158">Their only purpose is to allow the use of real time geo-fencing \<Reach-Audience-geofencing\> criterion in Reach campaigns.</span></span>

<span data-ttu-id="0b551-159">Pokud chcete povolit generování sestav umístění reálném čase, přidejte následující řádek po inicializaci agenta Engagement:</span><span class="sxs-lookup"><span data-stu-id="0b551-159">To enable real time location reporting, add the following line after initializing the Engagement agent:</span></span>

    [[EngagementAgent shared] setRealtimeLocationReport:YES];

#### <a name="gps-based-reporting"></a><span data-ttu-id="0b551-160">Na základě GPS při vytváření sestav</span><span class="sxs-lookup"><span data-stu-id="0b551-160">GPS based reporting</span></span>
<span data-ttu-id="0b551-161">Ve výchozím nastavení hlášení polohy reálném čase pouze používá síťové umístění.</span><span class="sxs-lookup"><span data-stu-id="0b551-161">By default, real time location reporting only uses network based locations.</span></span> <span data-ttu-id="0b551-162">Chcete-li povolit použití GPS na základě umístění (což je mnohem přesnější), přidejte:</span><span class="sxs-lookup"><span data-stu-id="0b551-162">To enable the use of GPS based locations (which are far more precise), add:</span></span>

    [[EngagementAgent shared] setFineRealtimeLocationReport:YES];

#### <a name="background-reporting"></a><span data-ttu-id="0b551-163">Vytváření sestav pozadí</span><span class="sxs-lookup"><span data-stu-id="0b551-163">Background reporting</span></span>
<span data-ttu-id="0b551-164">Ve výchozím nastavení hlášení polohy reálném čase je aktivní pouze když je aplikace spuštěná v popředí (tj. během relace).</span><span class="sxs-lookup"><span data-stu-id="0b551-164">By default, real time location reporting is only active when the application runs in foreground (i.e. during a session).</span></span> <span data-ttu-id="0b551-165">Chcete-li povolit generování sestav také v pozadí, přidejte:</span><span class="sxs-lookup"><span data-stu-id="0b551-165">To enable the reporting also in background, add:</span></span>

    [[EngagementAgent shared] setBackgroundRealtimeLocationReport:YES withLaunchOptions:launchOptions];

> [!NOTE]
> <span data-ttu-id="0b551-166">Při spuštění aplikace v pozadí, pouze sítě na základě umístění nahlásí, i když je povolená GPS.</span><span class="sxs-lookup"><span data-stu-id="0b551-166">When the application runs in background, only network based locations are reported, even if you enabled the GPS.</span></span>
>
>

<span data-ttu-id="0b551-167">Implementace této funkce bude volat [startMonitoringSignificantLocationChanges] když vaše aplikace přejde do na pozadí.</span><span class="sxs-lookup"><span data-stu-id="0b551-167">Implementation of this function will call [startMonitoringSignificantLocationChanges] when your application goes into the background.</span></span> <span data-ttu-id="0b551-168">Uvědomte si ji budou spustit aplikaci do na pozadí, automaticky Pokud dorazí novou událost umístění.</span><span class="sxs-lookup"><span data-stu-id="0b551-168">Be aware that it will automatically relaunch your application into the background if a new location event arrives.</span></span>

## <a name="advanced-reporting"></a><span data-ttu-id="0b551-169">Rozšířená tvorba sestav</span><span class="sxs-lookup"><span data-stu-id="0b551-169">Advanced reporting</span></span>
<span data-ttu-id="0b551-170">Případně, pokud chcete k hlášení konkrétních událostí aplikace, chyb a úlohy, budete muset použít rozhraní API Engagement prostřednictvím metody `EngagementAgent` třídy.</span><span class="sxs-lookup"><span data-stu-id="0b551-170">Optionally, if you want to report application specific events, errors and jobs, you need to use the Engagement API through the methods of the `EngagementAgent` class.</span></span> <span data-ttu-id="0b551-171">Objekt této třídy může načíst volání `[EngagementAgent shared]` statickou metodu.</span><span class="sxs-lookup"><span data-stu-id="0b551-171">An object of this class can be retrieved by calling the `[EngagementAgent shared]` static method.</span></span>

<span data-ttu-id="0b551-172">Rozhraní API Engagement umožňuje používat všechny rozšířené možnosti Engagement a podrobně pokynů používat rozhraní API Engagement v systému iOS (i jako v technické dokumentaci `EngagementAgent` třídy).</span><span class="sxs-lookup"><span data-stu-id="0b551-172">The Engagement API allows to use all of Engagement's advanced capabilities and is detailed in the How to Use the Engagement API on iOS (as well as in the technical documentation of the `EngagementAgent` class).</span></span>

## <a name="disable-idfa-collection"></a><span data-ttu-id="0b551-173">Zakázání shromažďování IDFA</span><span class="sxs-lookup"><span data-stu-id="0b551-173">Disable IDFA collection</span></span>
<span data-ttu-id="0b551-174">Ve výchozím nastavení, bude používat Engagement [IDFA] k jednoznačné identifikaci uživatele.</span><span class="sxs-lookup"><span data-stu-id="0b551-174">By default, Engagement will use the [IDFA] to uniquely identify a user.</span></span> <span data-ttu-id="0b551-175">Ale pokud nepoužíváte inzerování jinde v aplikaci, vám mohou být odmítnuty zkontrolujte procesem obchodu s aplikacemi.</span><span class="sxs-lookup"><span data-stu-id="0b551-175">But if you’re not using advertising elsewhere in the app, you might be rejected by the App Store review process.</span></span> <span data-ttu-id="0b551-176">Shromažďování IDFA lze zakázat přidáním makro preprocesoru `ENGAGEMENT_DISABLE_IDFA` v souboru pch (nebo `Build Settings` aplikace).</span><span class="sxs-lookup"><span data-stu-id="0b551-176">IDFA collection can be disabled by adding the preprocessor macro `ENGAGEMENT_DISABLE_IDFA` in your pch file (or in the `Build Settings` of your application).</span></span> <span data-ttu-id="0b551-177">To zajistí, že neexistuje žádné odkazy na `ASIdentifierManager`, `advertisingIdentifier` nebo `isAdvertisingTrackingEnabled` v sestavení aplikace.</span><span class="sxs-lookup"><span data-stu-id="0b551-177">This will ensure that there is no references to `ASIdentifierManager`, `advertisingIdentifier` or `isAdvertisingTrackingEnabled` in the application build.</span></span>

<span data-ttu-id="0b551-178">Integrace ve **prefix.pch** souboru:</span><span class="sxs-lookup"><span data-stu-id="0b551-178">Integration in the **prefix.pch** file:</span></span>

    #define ENGAGEMENT_DISABLE_IDFA
    ...

<span data-ttu-id="0b551-179">Můžete ověřit, že shromažďování IDFA je správně zakázán v aplikaci kontrolou protokolů testování zapojení.</span><span class="sxs-lookup"><span data-stu-id="0b551-179">You can verify that the IDFA collection is properly disabled in your application by checking the Engagement test logs.</span></span> <span data-ttu-id="0b551-180">V tématu integrační testovací\<ios-sdk-engagement-test-idfa\> Další informace naleznete v dokumentaci.</span><span class="sxs-lookup"><span data-stu-id="0b551-180">See the Integration Test\<ios-sdk-engagement-test-idfa\> documentation for further information.</span></span>

## <a name="disable-log-reporting"></a><span data-ttu-id="0b551-181">Zakázání zasílání zpráv protokolu</span><span class="sxs-lookup"><span data-stu-id="0b551-181">Disable log reporting</span></span>
### <a name="using-a-method-call"></a><span data-ttu-id="0b551-182">Pomocí volání metody</span><span class="sxs-lookup"><span data-stu-id="0b551-182">Using a method call</span></span>
<span data-ttu-id="0b551-183">Pokud chcete Engagement zastavit odesílání protokolů, můžete volat:</span><span class="sxs-lookup"><span data-stu-id="0b551-183">If you want Engagement to stop sending logs, you can call:</span></span>

    [[EngagementAgent shared] setEnabled:NO];

<span data-ttu-id="0b551-184">Toto volání je trvalé: používá `NSUserDefaults` uložit informace o.</span><span class="sxs-lookup"><span data-stu-id="0b551-184">This call is persistent: it uses `NSUserDefaults` to store the information.</span></span>

<span data-ttu-id="0b551-185">Můžete povolit protokol reporting znovu voláním stejnou funkci s `YES`.</span><span class="sxs-lookup"><span data-stu-id="0b551-185">You can enable log reporting again by calling the same function with `YES`.</span></span>

### <a name="integration-in-your-settings-bundle"></a><span data-ttu-id="0b551-186">Integrace ve vaší sady nastavení</span><span class="sxs-lookup"><span data-stu-id="0b551-186">Integration in your settings bundle</span></span>
<span data-ttu-id="0b551-187">Namísto volání této funkce, můžete také integrovat toto nastavení přímo do existující `Settings.bundle` souboru.</span><span class="sxs-lookup"><span data-stu-id="0b551-187">Instead of calling this function, you can also integrate this setting directly in your existing `Settings.bundle` file.</span></span> <span data-ttu-id="0b551-188">Řetězec `engagement_agent_enabled` jako musí být použita identifikátor předvoleb a musí být přidružený k přepínači přepnutí (`PSToggleSwitchSpecifier`).</span><span class="sxs-lookup"><span data-stu-id="0b551-188">The string `engagement_agent_enabled` must be used as a the preference identifier and it must be associated to a toggle switch(`PSToggleSwitchSpecifier`).</span></span>

<span data-ttu-id="0b551-189">Následující příklad `Settings.bundle` ukazuje, jak implementovat ho:</span><span class="sxs-lookup"><span data-stu-id="0b551-189">The following example of `Settings.bundle` shows how to implement it:</span></span>

    <dict>
        <key>PreferenceSpecifiers</key>
        <array>
            <dict>
                <key>DefaultValue</key>
                <true/>
                <key>Key</key>
                <string>engagement_agent_enabled</string>
                <key>Title</key>
                <string>Log reporting enabled</string>
                <key>Type</key>
                <string>PSToggleSwitchSpecifier</string>
            </dict>
        </array>
        <key>StringsTable</key>
        <string>Root</string>
    </dict>

<!-- URLs. -->
<span data-ttu-id="0b551-190">[rozhraní API pro zařízení]: http://go.microsoft.com/?linkid=9876094</span><span class="sxs-lookup"><span data-stu-id="0b551-190">[Device API]: http://go.microsoft.com/?linkid=9876094</span></span>
<span data-ttu-id="0b551-191">[NSLocationWhenInUseUsageDescription]:https://developer.apple.com/library/prerelease/ios/documentation/General/Reference/InfoPlistKeyReference/Articles/CocoaKeys.html#//apple_ref/doc/uid/TP40009251-SW26</span><span class="sxs-lookup"><span data-stu-id="0b551-191">[NSLocationWhenInUseUsageDescription]:https://developer.apple.com/library/prerelease/ios/documentation/General/Reference/InfoPlistKeyReference/Articles/CocoaKeys.html#//apple_ref/doc/uid/TP40009251-SW26</span></span>
<span data-ttu-id="0b551-192">[NSLocationAlwaysUsageDescription]:https://developer.apple.com/library/prerelease/ios/documentation/General/Reference/InfoPlistKeyReference/Articles/CocoaKeys.html#//apple_ref/doc/uid/TP40009251-SW18</span><span class="sxs-lookup"><span data-stu-id="0b551-192">[NSLocationAlwaysUsageDescription]:https://developer.apple.com/library/prerelease/ios/documentation/General/Reference/InfoPlistKeyReference/Articles/CocoaKeys.html#//apple_ref/doc/uid/TP40009251-SW18</span></span>
<span data-ttu-id="0b551-193">[startMonitoringSignificantLocationChanges]:http://developer.apple.com/library/IOs/#documentation/CoreLocation/Reference/CLLocationManager_Class/CLLocationManager/CLLocationManager.html#//apple_ref/occ/instm/CLLocationManager/startMonitoringSignificantLocationChanges</span><span class="sxs-lookup"><span data-stu-id="0b551-193">[startMonitoringSignificantLocationChanges]:http://developer.apple.com/library/IOs/#documentation/CoreLocation/Reference/CLLocationManager_Class/CLLocationManager/CLLocationManager.html#//apple_ref/occ/instm/CLLocationManager/startMonitoringSignificantLocationChanges</span></span>
<span data-ttu-id="0b551-194">[IDFA]:https://developer.apple.com/library/ios/documentation/AdSupport/Reference/ASIdentifierManager_Ref/ASIdentifierManager.html#//apple_ref/occ/instp/ASIdentifierManager/advertisingIdentifier</span><span class="sxs-lookup"><span data-stu-id="0b551-194">[IDFA]:https://developer.apple.com/library/ios/documentation/AdSupport/Reference/ASIdentifierManager_Ref/ASIdentifierManager.html#//apple_ref/occ/instp/ASIdentifierManager/advertisingIdentifier</span></span>
