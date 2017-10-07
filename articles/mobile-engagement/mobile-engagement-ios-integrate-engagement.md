---
title: aaaAzure Mobile Engagement iOS SDK integrace | Microsoft Docs
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
ms.openlocfilehash: 66ce34efabede7d882caa8a91431a8df71e4fb59
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="how-toointegrate-engagement-on-ios"></a><span data-ttu-id="8185b-103">Jak tooIntegrate Engagement v systému iOS</span><span class="sxs-lookup"><span data-stu-id="8185b-103">How tooIntegrate Engagement on iOS</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="8185b-104">Univerzální pro Windows</span><span class="sxs-lookup"><span data-stu-id="8185b-104">Windows Universal</span></span>](mobile-engagement-windows-store-integrate-engagement.md)
> * [<span data-ttu-id="8185b-105">Windows Phone Silverlight</span><span class="sxs-lookup"><span data-stu-id="8185b-105">Windows Phone Silverlight</span></span>](mobile-engagement-windows-phone-integrate-engagement.md)
> * [<span data-ttu-id="8185b-106">iOS</span><span class="sxs-lookup"><span data-stu-id="8185b-106">iOS</span></span>](mobile-engagement-ios-integrate-engagement.md)
> * [<span data-ttu-id="8185b-107">Android</span><span class="sxs-lookup"><span data-stu-id="8185b-107">Android</span></span>](mobile-engagement-android-integrate-engagement.md)
>
>

<span data-ttu-id="8185b-108">Tento postup popisuje hello nejjednodušší způsob, jak tooactivate Engagement analýzy a monitorování funkcí v aplikace iOS.</span><span class="sxs-lookup"><span data-stu-id="8185b-108">This procedure describes hello simplest way tooactivate Engagement's Analytics and Monitoring functions in your iOS application.</span></span>

<span data-ttu-id="8185b-109">Hello Engagement SDK vyžaduje systému IOS 7 + a Xcode 8 +: hello cíl nasazení vaší aplikace musí být alespoň iOS 7.</span><span class="sxs-lookup"><span data-stu-id="8185b-109">hello Engagement SDK requires iOS7+ and Xcode 8+: hello deployment target of your application must be at least iOS 7.</span></span>

> [!NOTE]
> <span data-ttu-id="8185b-110">Pokud ve skutečnosti závisí na XCode 7 pak můžete použít hello [iOS Engagement SDK v3.2.4](https://aka.ms/r6oouh).</span><span class="sxs-lookup"><span data-stu-id="8185b-110">If you really depend on XCode 7 then you may use hello [iOS Engagement SDK v3.2.4](https://aka.ms/r6oouh).</span></span> <span data-ttu-id="8185b-111">Je známého problému při spuštění v iOS 10 zařízení najdete na modul Reach hello tato předchozí verze [hello reach modulu integrace](mobile-engagement-ios-integrate-engagement-reach.md) další podrobnosti.</span><span class="sxs-lookup"><span data-stu-id="8185b-111">There is a known bug on hello Reach module of this previous version while running on iOS 10 devices see [hello reach module integration](mobile-engagement-ios-integrate-engagement-reach.md) for more details.</span></span> <span data-ttu-id="8185b-112">Pokud zvolte toouse hello SDK v3.2.4 a z nich přeskočit, hello `UserNotifications.framework` importovat v dalším kroku hello.</span><span class="sxs-lookup"><span data-stu-id="8185b-112">If you choose toouse hello SDK v3.2.4 then just skip hello `UserNotifications.framework` import in hello next step.</span></span>
>
>

<span data-ttu-id="8185b-113">Hello následující kroky jsou že dostatek tooactivate hello protokolů je nutná sestava toocompute všechny statistické údaje týkající se uživatelů, relací, aktivity, dojde k chybě a Technicals.</span><span class="sxs-lookup"><span data-stu-id="8185b-113">hello following steps are enough tooactivate hello report of logs needed toocompute all statistics regarding Users, Sessions, Activities, Crashes and Technicals.</span></span> <span data-ttu-id="8185b-114">Hello protokolů je nutná sestava toocompute další statistiky jako události a chyby úlohy je třeba provést ručně pomocí hello Engagement rozhraní API (v tématu [jak toouse hello advanced Mobile Engagement označování rozhraní API v aplikaci s iOS](mobile-engagement-ios-use-engagement-api.md) od těchto statistiky jsou závislé aplikace.</span><span class="sxs-lookup"><span data-stu-id="8185b-114">hello report of logs needed toocompute other statistics like Events, Errors and Jobs must be done manually using hello Engagement API  (see [How toouse hello advanced Mobile Engagement tagging API in your iOS app](mobile-engagement-ios-use-engagement-api.md) since these statistics are application dependent.</span></span>

## <a name="embed-hello-engagement-sdk-into-your-ios-project"></a><span data-ttu-id="8185b-115">Vložení hello Engagement SDK do projektu iOS</span><span class="sxs-lookup"><span data-stu-id="8185b-115">Embed hello Engagement SDK into your iOS project</span></span>
* <span data-ttu-id="8185b-116">Stahování aplikací z iOS SDK hello z [zde](http://aka.ms/qk2rnj).</span><span class="sxs-lookup"><span data-stu-id="8185b-116">Download hello iOS SDK from [here](http://aka.ms/qk2rnj).</span></span>
* <span data-ttu-id="8185b-117">Přidat hello Engagement SDK tooyour iOS projektu: v Xcode, klikněte pravým tlačítkem na projekt a vyberte **"Přidat soubory příliš..."** a zvolte hello `EngagementSDK` složky.</span><span class="sxs-lookup"><span data-stu-id="8185b-117">Add hello Engagement SDK tooyour iOS project: in Xcode, right click on your project and select **"Add files too..."** and choose hello `EngagementSDK` folder.</span></span>
* <span data-ttu-id="8185b-118">Engagement vyžaduje další rozhraní toowork: v prohlížeči projektu hello, otevřete podokno váš projekt a vyberte hello správné cílové.</span><span class="sxs-lookup"><span data-stu-id="8185b-118">Engagement requires additional frameworks toowork: in hello project explorer, open your project pane and select hello correct target.</span></span> <span data-ttu-id="8185b-119">Potom otevřete hello **"Fáze sestavení"** kartě a v hello **"Odkaz binárních souborů a knihoven"** nabídky, přidejte tyto architektury:</span><span class="sxs-lookup"><span data-stu-id="8185b-119">Then, open hello **"Build phases"** tab and in hello **"Link Binary With Libraries"** menu, add these frameworks:</span></span>

  * <span data-ttu-id="8185b-120">`UserNotifications.framework`-sadu hello připojit jako`Optional`</span><span class="sxs-lookup"><span data-stu-id="8185b-120">`UserNotifications.framework` - set hello link as `Optional`</span></span>
  * <span data-ttu-id="8185b-121">`AdSupport.framework`-sadu hello připojit jako`Optional`</span><span class="sxs-lookup"><span data-stu-id="8185b-121">`AdSupport.framework` - set hello link as `Optional`</span></span>
  * `SystemConfiguration.framework`
  * `CoreTelephony.framework`
  * `CFNetwork.framework`
  * `CoreLocation.framework`
  * `libxml2.dylib`

> [!NOTE]
> <span data-ttu-id="8185b-122">Hello AdSupport framework se může odebrat.</span><span class="sxs-lookup"><span data-stu-id="8185b-122">hello AdSupport framework can be removed.</span></span> <span data-ttu-id="8185b-123">Zapojení musí tento framework toocollect hello IDFA.</span><span class="sxs-lookup"><span data-stu-id="8185b-123">Engagement needs this framework toocollect hello IDFA.</span></span> <span data-ttu-id="8185b-124">Nicméně je možné zakázat shromažďování IDFA \<ios sdk engagement idfa\> toocomply pomocí nové zásady Apple hello týkající se mu ID této.</span><span class="sxs-lookup"><span data-stu-id="8185b-124">However, IDFA collection can be disabled \<ios-sdk-engagement-idfa\> toocomply with hello new Apple policy regarding this ID.</span></span>
>
>

## <a name="initialize-hello-engagement-sdk"></a><span data-ttu-id="8185b-125">Inicializace hello Engagement SDK</span><span class="sxs-lookup"><span data-stu-id="8185b-125">Initialize hello Engagement SDK</span></span>
<span data-ttu-id="8185b-126">Je třeba toomodify delegáta aplikace:</span><span class="sxs-lookup"><span data-stu-id="8185b-126">You need toomodify your Application Delegate:</span></span>

* <span data-ttu-id="8185b-127">Hello horní části souboru implementace importujte hello Engagement agenta:</span><span class="sxs-lookup"><span data-stu-id="8185b-127">At hello top of your implementation file, import hello Engagement agent:</span></span>

      [...]
      #import "EngagementAgent.h"
* <span data-ttu-id="8185b-128">Inicializace Engagement uvnitř metody hello '**applicationDidFinishLaunching:**"nebo"**aplikace: didFinishLaunchingWithOptions:**':</span><span class="sxs-lookup"><span data-stu-id="8185b-128">Initialize Engagement inside hello method '**applicationDidFinishLaunching:**' or '**application:didFinishLaunchingWithOptions:**':</span></span>

      - (BOOL)application:(UIApplication *)application didFinishLaunchingWithOptions:(NSDictionary *)launchOptions
      {
        [...]
        [EngagementAgent init:@"Endpoint={YOUR_APP_COLLECTION.DOMAIN};SdkKey={YOUR_SDK_KEY};AppId={YOUR_APPID}"];
        [...]
      }

## <a name="basic-reporting"></a><span data-ttu-id="8185b-129">Vytváření základních sestav</span><span class="sxs-lookup"><span data-stu-id="8185b-129">Basic reporting</span></span>
### <a name="recommended-method-overload-your-uiviewcontroller-classes"></a><span data-ttu-id="8185b-130">Doporučená metoda: přetížení vaší `UIViewController` třídy</span><span class="sxs-lookup"><span data-stu-id="8185b-130">Recommended method: overload your `UIViewController` classes</span></span>
<span data-ttu-id="8185b-131">V pořadí tooactivate hello sestavu všechny protokoly hello vyžadují zapojení toocompute uživatelů, relací, aktivity, dojde k chybě a technické statistiky, můžete jednoduše provést všechny vaše `UIViewController` dílčí třídy dědí hello `EngagementViewController` třídy (stejného pravidla pro `UITableViewController`  - \> `EngagementTableViewController`).</span><span class="sxs-lookup"><span data-stu-id="8185b-131">In order tooactivate hello report of all hello logs required by Engagement toocompute Users, Sessions, Activities, Crashes and Technical statistics, you can simply make all your `UIViewController` sub-classes inherit from hello `EngagementViewController` classes (same rule for `UITableViewController` -\> `EngagementTableViewController`).</span></span>

<span data-ttu-id="8185b-132">**Bez Engagement:**</span><span class="sxs-lookup"><span data-stu-id="8185b-132">**Without Engagement :**</span></span>

    #import <UIKit/UIKit.h>

    @interface Tab1ViewController : UIViewController<UITextFieldDelegate> {
      UITextField* myTextField1;
      UITextField* myTextField2;
    }

    @property (nonatomic, retain) IBOutlet UITextField* myTextField1;
    @property (nonatomic, retain) IBOutlet UITextField* myTextField2;

<span data-ttu-id="8185b-133">**S Engagement:**</span><span class="sxs-lookup"><span data-stu-id="8185b-133">**With Engagement :**</span></span>

    #import <UIKit/UIKit.h>
    #import "EngagementViewController.h"

    @interface Tab1ViewController : EngagementViewController<UITextFieldDelegate> {
      UITextField* myTextField1;
      UITextField* myTextField2;
    }

    @property (nonatomic, retain) IBOutlet UITextField* myTextField1;
    @property (nonatomic, retain) IBOutlet UITextField* myTextField2;

### <a name="alternate-method-call-startactivity-manually"></a><span data-ttu-id="8185b-134">Alternativní metoda: volání `startActivity()` ručně</span><span class="sxs-lookup"><span data-stu-id="8185b-134">Alternate method: call `startActivity()` manually</span></span>
<span data-ttu-id="8185b-135">Pokud nemůžete nebo nechcete, aby toooverload vaše `UIViewController` třídy, můžete místo toho spustit vaše aktivity voláním `EngagementAgent`na metody přímo.</span><span class="sxs-lookup"><span data-stu-id="8185b-135">If you cannot or do not want toooverload your `UIViewController` classes, you can instead start your activities by calling `EngagementAgent`'s methods directly.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="8185b-136">Sada SDK iOS Hello automaticky volá hello `endActivity()` metoda při zavření aplikace hello.</span><span class="sxs-lookup"><span data-stu-id="8185b-136">hello iOS SDK automatically calls hello `endActivity()` method when hello application is closed.</span></span> <span data-ttu-id="8185b-137">Z toho důvodu je *vysoce* doporučená toocall hello `startActivity` metoda vždy, když aktivita hello hello uživatele změnit a příliš*nikdy* volání hello `endActivity` metoda od volání, vynutí tato metoda toobe Hello aktuální relace skončila.</span><span class="sxs-lookup"><span data-stu-id="8185b-137">Thus, it is *HIGHLY* recommended toocall hello `startActivity` method whenever hello activity of hello user change, and too*NEVER* call hello `endActivity` method, since calling this method forces hello current session toobe ended.</span></span>
>
>

## <a name="location-reporting"></a><span data-ttu-id="8185b-138">Rozšířené ohlašování polohy</span><span class="sxs-lookup"><span data-stu-id="8185b-138">Location reporting</span></span>
<span data-ttu-id="8185b-139">Podmínky služby Apple neumožňují toouse umístění sledování pro účely statistiky pouze aplikace.</span><span class="sxs-lookup"><span data-stu-id="8185b-139">Apple terms of service do not allow applications toouse location tracking for statistics purpose only.</span></span> <span data-ttu-id="8185b-140">Proto se doporučuje tooenable umístění sestavy pouze v případě, že vaše aplikace používat také hello umístění sledování z jiného důvodu.</span><span class="sxs-lookup"><span data-stu-id="8185b-140">Thus, it is recommended tooenable location reports only if your application also use hello location tracking for another reason.</span></span>

<span data-ttu-id="8185b-141">Od verze iOS 8, je nutné zadat popis jak vaše aplikace používá polohy nastavením řetězec pro klíč hello [NSLocationWhenInUseUsageDescription] nebo [NSLocationAlwaysUsageDescription]v souboru Info.plist vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="8185b-141">Starting with iOS 8, you must provide a description for how your app uses location services by setting a string for hello key [NSLocationWhenInUseUsageDescription] or [NSLocationAlwaysUsageDescription] in your app's Info.plist file.</span></span> <span data-ttu-id="8185b-142">Pokud chcete tooreport umístění pozadí hello se zapojení, přidejte klíč hello NSLocationAlwaysUsageDescription.</span><span class="sxs-lookup"><span data-stu-id="8185b-142">If you want tooreport location in hello background with Engagement, add hello key NSLocationAlwaysUsageDescription.</span></span> <span data-ttu-id="8185b-143">Ve všech ostatních případech přidáte klíč hello NSLocationWhenInUseUsageDescription.</span><span class="sxs-lookup"><span data-stu-id="8185b-143">In all other cases, add hello key NSLocationWhenInUseUsageDescription.</span></span> <span data-ttu-id="8185b-144">Všimněte si, že je potřeba NSLocationAlwaysAndWhenInUseUsageDescription a NSLocationWhenInUseUsageDescription umístění pozadí tooreport na iOS 11.</span><span class="sxs-lookup"><span data-stu-id="8185b-144">Note that you need both NSLocationAlwaysAndWhenInUseUsageDescription and NSLocationWhenInUseUsageDescription tooreport background location on iOS 11.</span></span>

### <a name="lazy-area-location-reporting"></a><span data-ttu-id="8185b-145">Opožděné hlášení umístění oblasti</span><span class="sxs-lookup"><span data-stu-id="8185b-145">Lazy area location reporting</span></span>
<span data-ttu-id="8185b-146">Opožděné hlášení umístění oblasti umožňuje tooreport hello zemi, oblast a polohu přidružené toodevices.</span><span class="sxs-lookup"><span data-stu-id="8185b-146">Lazy area location reporting allows tooreport hello country, region and locality associated toodevices.</span></span> <span data-ttu-id="8185b-147">Tento typ hlášení umístění používá jenom síťová umístění (na základě ID buňky nebo Wi-Fi).</span><span class="sxs-lookup"><span data-stu-id="8185b-147">This type of location reporting only uses network locations (based on Cell ID or WIFI).</span></span> <span data-ttu-id="8185b-148">oblasti Hello zařízení se hlásí nejvíce jednou za relace.</span><span class="sxs-lookup"><span data-stu-id="8185b-148">hello device area is reported at most once per session.</span></span> <span data-ttu-id="8185b-149">Hello GPS se nikdy nepoužívá, a proto tento typ umístění sestavy má jen v několika (toosay není žádná) dopad na baterie hello.</span><span class="sxs-lookup"><span data-stu-id="8185b-149">hello GPS is never used, and thus this type of location report has very few (not toosay no) impact on hello battery.</span></span>

<span data-ttu-id="8185b-150">Hlášené oblasti jsou použité toocompute geografické Statistika týkající se uživatelů, relací, události a chyby.</span><span class="sxs-lookup"><span data-stu-id="8185b-150">Reported areas are used toocompute geographic statistics about users, sessions, events and errors.</span></span> <span data-ttu-id="8185b-151">Může být také používány jako kritérium v kampaně Reach.</span><span class="sxs-lookup"><span data-stu-id="8185b-151">They can also be used as criterion in Reach campaigns.</span></span> <span data-ttu-id="8185b-152">Hello poslední známá oblasti pro zařízení může být načtena Děkujeme toohello [rozhraní API pro zařízení].</span><span class="sxs-lookup"><span data-stu-id="8185b-152">hello last known area reported for a device can be retrieved thanks toohello [Device API].</span></span>

<span data-ttu-id="8185b-153">umístění tooenable opožděné oblasti vytváření sestav, přidejte následující řádek po inicializaci agenta Engagement hello hello:</span><span class="sxs-lookup"><span data-stu-id="8185b-153">tooenable lazy area location reporting, add hello following line after initializing hello Engagement agent:</span></span>

    - (BOOL)application:(UIApplication *)application didFinishLaunchingWithOptions:(NSDictionary *)launchOptions
    {
      [...]
      [[EngagementAgent shared] setLazyAreaLocationReport:YES];
      [...]
    }

### <a name="real-time-location-reporting"></a><span data-ttu-id="8185b-154">Hlášení polohy reálném čase.</span><span class="sxs-lookup"><span data-stu-id="8185b-154">Real time location reporting</span></span>
<span data-ttu-id="8185b-155">Hlášení polohy reálném čase umožňuje tooreport hello šířky a délky, přidružené toodevices.</span><span class="sxs-lookup"><span data-stu-id="8185b-155">Real time location reporting allows tooreport hello latitude and longitude associated toodevices.</span></span> <span data-ttu-id="8185b-156">Ve výchozím nastavení tento typ hlášení umístění používá jenom síťová umístění (na základě ID buňky nebo Wi-Fi) a vytváření sestav hello je aktivní pouze při spuštění aplikace hello v popředí (tj. během relace).</span><span class="sxs-lookup"><span data-stu-id="8185b-156">By default, this type of location reporting only uses network locations (based on Cell ID or WIFI), and hello reporting is only active when hello application runs in foreground (i.e. during a session).</span></span>

<span data-ttu-id="8185b-157">Umístění v reálném čase, jsou *není* používá toocompute statistiky.</span><span class="sxs-lookup"><span data-stu-id="8185b-157">Real time locations are *NOT* used toocompute statistics.</span></span> <span data-ttu-id="8185b-158">Jejich jediným účelem je použití hello tooallow geografického vymezení reálném čase \<monitorování geografických Reach-cílové skupiny zón\> kritérium v kampaně Reach.</span><span class="sxs-lookup"><span data-stu-id="8185b-158">Their only purpose is tooallow hello use of real time geo-fencing \<Reach-Audience-geofencing\> criterion in Reach campaigns.</span></span>

<span data-ttu-id="8185b-159">umístění reálném čase tooenable vytváření sestav, přidejte následující řádek po inicializaci agenta Engagement hello hello:</span><span class="sxs-lookup"><span data-stu-id="8185b-159">tooenable real time location reporting, add hello following line after initializing hello Engagement agent:</span></span>

    [[EngagementAgent shared] setRealtimeLocationReport:YES];

#### <a name="gps-based-reporting"></a><span data-ttu-id="8185b-160">Na základě GPS při vytváření sestav</span><span class="sxs-lookup"><span data-stu-id="8185b-160">GPS based reporting</span></span>
<span data-ttu-id="8185b-161">Ve výchozím nastavení hlášení polohy reálném čase pouze používá síťové umístění.</span><span class="sxs-lookup"><span data-stu-id="8185b-161">By default, real time location reporting only uses network based locations.</span></span> <span data-ttu-id="8185b-162">použití hello tooenable GPS na základě umístění (což je mnohem přesnější), přidejte:</span><span class="sxs-lookup"><span data-stu-id="8185b-162">tooenable hello use of GPS based locations (which are far more precise), add:</span></span>

    [[EngagementAgent shared] setFineRealtimeLocationReport:YES];

#### <a name="background-reporting"></a><span data-ttu-id="8185b-163">Vytváření sestav pozadí</span><span class="sxs-lookup"><span data-stu-id="8185b-163">Background reporting</span></span>
<span data-ttu-id="8185b-164">Ve výchozím nastavení hlášení polohy reálném čase je aktivní pouze při spuštění aplikace hello v popředí (tj. během relace).</span><span class="sxs-lookup"><span data-stu-id="8185b-164">By default, real time location reporting is only active when hello application runs in foreground (i.e. during a session).</span></span> <span data-ttu-id="8185b-165">hello tooenable také vytváření sestav v pozadí, přidejte:</span><span class="sxs-lookup"><span data-stu-id="8185b-165">tooenable hello reporting also in background, add:</span></span>

    [[EngagementAgent shared] setBackgroundRealtimeLocationReport:YES withLaunchOptions:launchOptions];

> [!NOTE]
> <span data-ttu-id="8185b-166">Při spuštění aplikace hello v pozadí, jsou hlášeny pouze síťové umístění, i když jste povolili hello GPS.</span><span class="sxs-lookup"><span data-stu-id="8185b-166">When hello application runs in background, only network based locations are reported, even if you enabled hello GPS.</span></span>
>
>

<span data-ttu-id="8185b-167">Implementace této funkce bude volat [startMonitoringSignificantLocationChanges] když vaše aplikace přejde do pozadí hello.</span><span class="sxs-lookup"><span data-stu-id="8185b-167">Implementation of this function will call [startMonitoringSignificantLocationChanges] when your application goes into hello background.</span></span> <span data-ttu-id="8185b-168">Uvědomte si ji budou spustit aplikaci do hello pozadí, automaticky Pokud dorazí novou událost umístění.</span><span class="sxs-lookup"><span data-stu-id="8185b-168">Be aware that it will automatically relaunch your application into hello background if a new location event arrives.</span></span>

## <a name="advanced-reporting"></a><span data-ttu-id="8185b-169">Rozšířená tvorba sestav</span><span class="sxs-lookup"><span data-stu-id="8185b-169">Advanced reporting</span></span>
<span data-ttu-id="8185b-170">Případně, pokud chcete tooreport aplikace konkrétní události, chyb a úlohy, je nutné toouse hello Engagement rozhraní API prostřednictvím metody hello hello `EngagementAgent` třídy.</span><span class="sxs-lookup"><span data-stu-id="8185b-170">Optionally, if you want tooreport application specific events, errors and jobs, you need toouse hello Engagement API through hello methods of hello `EngagementAgent` class.</span></span> <span data-ttu-id="8185b-171">Objekt této třídy může načíst volání hello `[EngagementAgent shared]` statickou metodu.</span><span class="sxs-lookup"><span data-stu-id="8185b-171">An object of this class can be retrieved by calling hello `[EngagementAgent shared]` static method.</span></span>

<span data-ttu-id="8185b-172">Hello Engagement rozhraní API umožňuje toouse, všechny rozšířené možnosti Engagement a jak je popsané v hello tooUse rozhraní API Engagement v systému iOS (jako v technické dokumentaci hello hello i `EngagementAgent` třídy).</span><span class="sxs-lookup"><span data-stu-id="8185b-172">hello Engagement API allows toouse all of Engagement's advanced capabilities and is detailed in hello How tooUse the Engagement API on iOS (as well as in hello technical documentation of hello `EngagementAgent` class).</span></span>

## <a name="disable-idfa-collection"></a><span data-ttu-id="8185b-173">Zakázání shromažďování IDFA</span><span class="sxs-lookup"><span data-stu-id="8185b-173">Disable IDFA collection</span></span>
<span data-ttu-id="8185b-174">Ve výchozím nastavení, použije Engagement hello [IDFA] toouniquely identifikace uživatele.</span><span class="sxs-lookup"><span data-stu-id="8185b-174">By default, Engagement will use hello [IDFA] toouniquely identify a user.</span></span> <span data-ttu-id="8185b-175">Ale pokud nepoužíváte inzerování jinde v aplikaci hello, vám může být odmítnut hello proces kontroly obchodu s aplikacemi.</span><span class="sxs-lookup"><span data-stu-id="8185b-175">But if you’re not using advertising elsewhere in hello app, you might be rejected by hello App Store review process.</span></span> <span data-ttu-id="8185b-176">Shromažďování IDFA lze zakázat přidáním makro preprocesoru hello `ENGAGEMENT_DISABLE_IDFA` v souboru pch (nebo v hello `Build Settings` aplikace).</span><span class="sxs-lookup"><span data-stu-id="8185b-176">IDFA collection can be disabled by adding hello preprocessor macro `ENGAGEMENT_DISABLE_IDFA` in your pch file (or in hello `Build Settings` of your application).</span></span> <span data-ttu-id="8185b-177">To zajistí, že neexistuje žádné odkazy příliš`ASIdentifierManager`, `advertisingIdentifier` nebo `isAdvertisingTrackingEnabled` v sestavení aplikace hello.</span><span class="sxs-lookup"><span data-stu-id="8185b-177">This will ensure that there is no references too`ASIdentifierManager`, `advertisingIdentifier` or `isAdvertisingTrackingEnabled` in hello application build.</span></span>

<span data-ttu-id="8185b-178">Integrace ve hello **prefix.pch** souboru:</span><span class="sxs-lookup"><span data-stu-id="8185b-178">Integration in hello **prefix.pch** file:</span></span>

    #define ENGAGEMENT_DISABLE_IDFA
    ...

<span data-ttu-id="8185b-179">Můžete ověřit, že shromažďování IDFA hello je správně zakázán v aplikaci kontrolou protokolů testování Engagement hello.</span><span class="sxs-lookup"><span data-stu-id="8185b-179">You can verify that hello IDFA collection is properly disabled in your application by checking hello Engagement test logs.</span></span> <span data-ttu-id="8185b-180">Najdete v části hello testování integrace\<ios-sdk-engagement-test-idfa\> Další informace naleznete v dokumentaci.</span><span class="sxs-lookup"><span data-stu-id="8185b-180">See hello Integration Test\<ios-sdk-engagement-test-idfa\> documentation for further information.</span></span>

## <a name="disable-log-reporting"></a><span data-ttu-id="8185b-181">Zakázání zasílání zpráv protokolu</span><span class="sxs-lookup"><span data-stu-id="8185b-181">Disable log reporting</span></span>
### <a name="using-a-method-call"></a><span data-ttu-id="8185b-182">Pomocí volání metody</span><span class="sxs-lookup"><span data-stu-id="8185b-182">Using a method call</span></span>
<span data-ttu-id="8185b-183">Pokud chcete Engagement toostop odesílání protokolů, můžete volat:</span><span class="sxs-lookup"><span data-stu-id="8185b-183">If you want Engagement toostop sending logs, you can call:</span></span>

    [[EngagementAgent shared] setEnabled:NO];

<span data-ttu-id="8185b-184">Toto volání je trvalé: používá `NSUserDefaults` toostore hello informace.</span><span class="sxs-lookup"><span data-stu-id="8185b-184">This call is persistent: it uses `NSUserDefaults` toostore hello information.</span></span>

<span data-ttu-id="8185b-185">Můžete povolit protokol reporting znovu voláním hello stejné fungovat s `YES`.</span><span class="sxs-lookup"><span data-stu-id="8185b-185">You can enable log reporting again by calling hello same function with `YES`.</span></span>

### <a name="integration-in-your-settings-bundle"></a><span data-ttu-id="8185b-186">Integrace ve vaší sady nastavení</span><span class="sxs-lookup"><span data-stu-id="8185b-186">Integration in your settings bundle</span></span>
<span data-ttu-id="8185b-187">Namísto volání této funkce, můžete také integrovat toto nastavení přímo do existující `Settings.bundle` souboru.</span><span class="sxs-lookup"><span data-stu-id="8185b-187">Instead of calling this function, you can also integrate this setting directly in your existing `Settings.bundle` file.</span></span> <span data-ttu-id="8185b-188">Hello řetězec `engagement_agent_enabled` musí být použita jako identifikátor předvoleb hello a musí být přidružené tooa přepnutí přepínače (`PSToggleSwitchSpecifier`).</span><span class="sxs-lookup"><span data-stu-id="8185b-188">hello string `engagement_agent_enabled` must be used as a hello preference identifier and it must be associated tooa toggle switch(`PSToggleSwitchSpecifier`).</span></span>

<span data-ttu-id="8185b-189">Následující příklad Hello `Settings.bundle` ukazuje, jak tooimplement ho:</span><span class="sxs-lookup"><span data-stu-id="8185b-189">hello following example of `Settings.bundle` shows how tooimplement it:</span></span>

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
[rozhraní API pro zařízení]: http://go.microsoft.com/?linkid=9876094
[NSLocationWhenInUseUsageDescription]:https://developer.apple.com/library/prerelease/ios/documentation/General/Reference/InfoPlistKeyReference/Articles/CocoaKeys.html#//apple_ref/doc/uid/TP40009251-SW26
[NSLocationAlwaysUsageDescription]:https://developer.apple.com/library/prerelease/ios/documentation/General/Reference/InfoPlistKeyReference/Articles/CocoaKeys.html#//apple_ref/doc/uid/TP40009251-SW18
[startMonitoringSignificantLocationChanges]:http://developer.apple.com/library/IOs/#documentation/CoreLocation/Reference/CLLocationManager_Class/CLLocationManager/CLLocationManager.html#//apple_ref/occ/instm/CLLocationManager/startMonitoringSignificantLocationChanges
[IDFA]:https://developer.apple.com/library/ios/documentation/AdSupport/Reference/ASIdentifierManager_Ref/ASIdentifierManager.html#//apple_ref/occ/instp/ASIdentifierManager/advertisingIdentifier
