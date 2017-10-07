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
# <a name="how-toointegrate-engagement-on-ios"></a>Jak tooIntegrate Engagement v systému iOS
> [!div class="op_single_selector"]
> * [Univerzální pro Windows](mobile-engagement-windows-store-integrate-engagement.md)
> * [Windows Phone Silverlight](mobile-engagement-windows-phone-integrate-engagement.md)
> * [iOS](mobile-engagement-ios-integrate-engagement.md)
> * [Android](mobile-engagement-android-integrate-engagement.md)
>
>

Tento postup popisuje hello nejjednodušší způsob, jak tooactivate Engagement analýzy a monitorování funkcí v aplikace iOS.

Hello Engagement SDK vyžaduje systému IOS 7 + a Xcode 8 +: hello cíl nasazení vaší aplikace musí být alespoň iOS 7.

> [!NOTE]
> Pokud ve skutečnosti závisí na XCode 7 pak můžete použít hello [iOS Engagement SDK v3.2.4](https://aka.ms/r6oouh). Je známého problému při spuštění v iOS 10 zařízení najdete na modul Reach hello tato předchozí verze [hello reach modulu integrace](mobile-engagement-ios-integrate-engagement-reach.md) další podrobnosti. Pokud zvolte toouse hello SDK v3.2.4 a z nich přeskočit, hello `UserNotifications.framework` importovat v dalším kroku hello.
>
>

Hello následující kroky jsou že dostatek tooactivate hello protokolů je nutná sestava toocompute všechny statistické údaje týkající se uživatelů, relací, aktivity, dojde k chybě a Technicals. Hello protokolů je nutná sestava toocompute další statistiky jako události a chyby úlohy je třeba provést ručně pomocí hello Engagement rozhraní API (v tématu [jak toouse hello advanced Mobile Engagement označování rozhraní API v aplikaci s iOS](mobile-engagement-ios-use-engagement-api.md) od těchto statistiky jsou závislé aplikace.

## <a name="embed-hello-engagement-sdk-into-your-ios-project"></a>Vložení hello Engagement SDK do projektu iOS
* Stahování aplikací z iOS SDK hello z [zde](http://aka.ms/qk2rnj).
* Přidat hello Engagement SDK tooyour iOS projektu: v Xcode, klikněte pravým tlačítkem na projekt a vyberte **"Přidat soubory příliš..."** a zvolte hello `EngagementSDK` složky.
* Engagement vyžaduje další rozhraní toowork: v prohlížeči projektu hello, otevřete podokno váš projekt a vyberte hello správné cílové. Potom otevřete hello **"Fáze sestavení"** kartě a v hello **"Odkaz binárních souborů a knihoven"** nabídky, přidejte tyto architektury:

  * `UserNotifications.framework`-sadu hello připojit jako`Optional`
  * `AdSupport.framework`-sadu hello připojit jako`Optional`
  * `SystemConfiguration.framework`
  * `CoreTelephony.framework`
  * `CFNetwork.framework`
  * `CoreLocation.framework`
  * `libxml2.dylib`

> [!NOTE]
> Hello AdSupport framework se může odebrat. Zapojení musí tento framework toocollect hello IDFA. Nicméně je možné zakázat shromažďování IDFA \<ios sdk engagement idfa\> toocomply pomocí nové zásady Apple hello týkající se mu ID této.
>
>

## <a name="initialize-hello-engagement-sdk"></a>Inicializace hello Engagement SDK
Je třeba toomodify delegáta aplikace:

* Hello horní části souboru implementace importujte hello Engagement agenta:

      [...]
      #import "EngagementAgent.h"
* Inicializace Engagement uvnitř metody hello '**applicationDidFinishLaunching:**"nebo"**aplikace: didFinishLaunchingWithOptions:**':

      - (BOOL)application:(UIApplication *)application didFinishLaunchingWithOptions:(NSDictionary *)launchOptions
      {
        [...]
        [EngagementAgent init:@"Endpoint={YOUR_APP_COLLECTION.DOMAIN};SdkKey={YOUR_SDK_KEY};AppId={YOUR_APPID}"];
        [...]
      }

## <a name="basic-reporting"></a>Vytváření základních sestav
### <a name="recommended-method-overload-your-uiviewcontroller-classes"></a>Doporučená metoda: přetížení vaší `UIViewController` třídy
V pořadí tooactivate hello sestavu všechny protokoly hello vyžadují zapojení toocompute uživatelů, relací, aktivity, dojde k chybě a technické statistiky, můžete jednoduše provést všechny vaše `UIViewController` dílčí třídy dědí hello `EngagementViewController` třídy (stejného pravidla pro `UITableViewController`  - \> `EngagementTableViewController`).

**Bez Engagement:**

    #import <UIKit/UIKit.h>

    @interface Tab1ViewController : UIViewController<UITextFieldDelegate> {
      UITextField* myTextField1;
      UITextField* myTextField2;
    }

    @property (nonatomic, retain) IBOutlet UITextField* myTextField1;
    @property (nonatomic, retain) IBOutlet UITextField* myTextField2;

**S Engagement:**

    #import <UIKit/UIKit.h>
    #import "EngagementViewController.h"

    @interface Tab1ViewController : EngagementViewController<UITextFieldDelegate> {
      UITextField* myTextField1;
      UITextField* myTextField2;
    }

    @property (nonatomic, retain) IBOutlet UITextField* myTextField1;
    @property (nonatomic, retain) IBOutlet UITextField* myTextField2;

### <a name="alternate-method-call-startactivity-manually"></a>Alternativní metoda: volání `startActivity()` ručně
Pokud nemůžete nebo nechcete, aby toooverload vaše `UIViewController` třídy, můžete místo toho spustit vaše aktivity voláním `EngagementAgent`na metody přímo.

> [!IMPORTANT]
> Sada SDK iOS Hello automaticky volá hello `endActivity()` metoda při zavření aplikace hello. Z toho důvodu je *vysoce* doporučená toocall hello `startActivity` metoda vždy, když aktivita hello hello uživatele změnit a příliš*nikdy* volání hello `endActivity` metoda od volání, vynutí tato metoda toobe Hello aktuální relace skončila.
>
>

## <a name="location-reporting"></a>Rozšířené ohlašování polohy
Podmínky služby Apple neumožňují toouse umístění sledování pro účely statistiky pouze aplikace. Proto se doporučuje tooenable umístění sestavy pouze v případě, že vaše aplikace používat také hello umístění sledování z jiného důvodu.

Od verze iOS 8, je nutné zadat popis jak vaše aplikace používá polohy nastavením řetězec pro klíč hello [NSLocationWhenInUseUsageDescription] nebo [NSLocationAlwaysUsageDescription]v souboru Info.plist vaší aplikace. Pokud chcete tooreport umístění pozadí hello se zapojení, přidejte klíč hello NSLocationAlwaysUsageDescription. Ve všech ostatních případech přidáte klíč hello NSLocationWhenInUseUsageDescription. Všimněte si, že je potřeba NSLocationAlwaysAndWhenInUseUsageDescription a NSLocationWhenInUseUsageDescription umístění pozadí tooreport na iOS 11.

### <a name="lazy-area-location-reporting"></a>Opožděné hlášení umístění oblasti
Opožděné hlášení umístění oblasti umožňuje tooreport hello zemi, oblast a polohu přidružené toodevices. Tento typ hlášení umístění používá jenom síťová umístění (na základě ID buňky nebo Wi-Fi). oblasti Hello zařízení se hlásí nejvíce jednou za relace. Hello GPS se nikdy nepoužívá, a proto tento typ umístění sestavy má jen v několika (toosay není žádná) dopad na baterie hello.

Hlášené oblasti jsou použité toocompute geografické Statistika týkající se uživatelů, relací, události a chyby. Může být také používány jako kritérium v kampaně Reach. Hello poslední známá oblasti pro zařízení může být načtena Děkujeme toohello [rozhraní API pro zařízení].

umístění tooenable opožděné oblasti vytváření sestav, přidejte následující řádek po inicializaci agenta Engagement hello hello:

    - (BOOL)application:(UIApplication *)application didFinishLaunchingWithOptions:(NSDictionary *)launchOptions
    {
      [...]
      [[EngagementAgent shared] setLazyAreaLocationReport:YES];
      [...]
    }

### <a name="real-time-location-reporting"></a>Hlášení polohy reálném čase.
Hlášení polohy reálném čase umožňuje tooreport hello šířky a délky, přidružené toodevices. Ve výchozím nastavení tento typ hlášení umístění používá jenom síťová umístění (na základě ID buňky nebo Wi-Fi) a vytváření sestav hello je aktivní pouze při spuštění aplikace hello v popředí (tj. během relace).

Umístění v reálném čase, jsou *není* používá toocompute statistiky. Jejich jediným účelem je použití hello tooallow geografického vymezení reálném čase \<monitorování geografických Reach-cílové skupiny zón\> kritérium v kampaně Reach.

umístění reálném čase tooenable vytváření sestav, přidejte následující řádek po inicializaci agenta Engagement hello hello:

    [[EngagementAgent shared] setRealtimeLocationReport:YES];

#### <a name="gps-based-reporting"></a>Na základě GPS při vytváření sestav
Ve výchozím nastavení hlášení polohy reálném čase pouze používá síťové umístění. použití hello tooenable GPS na základě umístění (což je mnohem přesnější), přidejte:

    [[EngagementAgent shared] setFineRealtimeLocationReport:YES];

#### <a name="background-reporting"></a>Vytváření sestav pozadí
Ve výchozím nastavení hlášení polohy reálném čase je aktivní pouze při spuštění aplikace hello v popředí (tj. během relace). hello tooenable také vytváření sestav v pozadí, přidejte:

    [[EngagementAgent shared] setBackgroundRealtimeLocationReport:YES withLaunchOptions:launchOptions];

> [!NOTE]
> Při spuštění aplikace hello v pozadí, jsou hlášeny pouze síťové umístění, i když jste povolili hello GPS.
>
>

Implementace této funkce bude volat [startMonitoringSignificantLocationChanges] když vaše aplikace přejde do pozadí hello. Uvědomte si ji budou spustit aplikaci do hello pozadí, automaticky Pokud dorazí novou událost umístění.

## <a name="advanced-reporting"></a>Rozšířená tvorba sestav
Případně, pokud chcete tooreport aplikace konkrétní události, chyb a úlohy, je nutné toouse hello Engagement rozhraní API prostřednictvím metody hello hello `EngagementAgent` třídy. Objekt této třídy může načíst volání hello `[EngagementAgent shared]` statickou metodu.

Hello Engagement rozhraní API umožňuje toouse, všechny rozšířené možnosti Engagement a jak je popsané v hello tooUse rozhraní API Engagement v systému iOS (jako v technické dokumentaci hello hello i `EngagementAgent` třídy).

## <a name="disable-idfa-collection"></a>Zakázání shromažďování IDFA
Ve výchozím nastavení, použije Engagement hello [IDFA] toouniquely identifikace uživatele. Ale pokud nepoužíváte inzerování jinde v aplikaci hello, vám může být odmítnut hello proces kontroly obchodu s aplikacemi. Shromažďování IDFA lze zakázat přidáním makro preprocesoru hello `ENGAGEMENT_DISABLE_IDFA` v souboru pch (nebo v hello `Build Settings` aplikace). To zajistí, že neexistuje žádné odkazy příliš`ASIdentifierManager`, `advertisingIdentifier` nebo `isAdvertisingTrackingEnabled` v sestavení aplikace hello.

Integrace ve hello **prefix.pch** souboru:

    #define ENGAGEMENT_DISABLE_IDFA
    ...

Můžete ověřit, že shromažďování IDFA hello je správně zakázán v aplikaci kontrolou protokolů testování Engagement hello. Najdete v části hello testování integrace\<ios-sdk-engagement-test-idfa\> Další informace naleznete v dokumentaci.

## <a name="disable-log-reporting"></a>Zakázání zasílání zpráv protokolu
### <a name="using-a-method-call"></a>Pomocí volání metody
Pokud chcete Engagement toostop odesílání protokolů, můžete volat:

    [[EngagementAgent shared] setEnabled:NO];

Toto volání je trvalé: používá `NSUserDefaults` toostore hello informace.

Můžete povolit protokol reporting znovu voláním hello stejné fungovat s `YES`.

### <a name="integration-in-your-settings-bundle"></a>Integrace ve vaší sady nastavení
Namísto volání této funkce, můžete také integrovat toto nastavení přímo do existující `Settings.bundle` souboru. Hello řetězec `engagement_agent_enabled` musí být použita jako identifikátor předvoleb hello a musí být přidružené tooa přepnutí přepínače (`PSToggleSwitchSpecifier`).

Následující příklad Hello `Settings.bundle` ukazuje, jak tooimplement ho:

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
