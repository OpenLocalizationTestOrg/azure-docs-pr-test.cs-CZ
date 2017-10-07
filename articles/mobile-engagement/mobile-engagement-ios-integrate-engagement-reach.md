---
title: aaaAzure integraci sady Reach SDK Mobile Engagement iOS | Microsoft Docs
description: "Nejnovější aktualizace a postupy pro iOS SDK pro Azure Mobile Engagement"
services: mobile-engagement
documentationcenter: mobile
author: piyushjo
manager: erikre
editor: 
ms.assetid: 1f5f5857-867c-40c5-9d76-675a343a0296
ms.service: mobile-engagement
ms.workload: mobile
ms.tgt_pltfrm: mobile-ios
ms.devlang: objective-c
ms.topic: article
ms.date: 12/13/2016
ms.author: piyushjo
ms.openlocfilehash: 40c9bfbdb475ab0b97bdbc9cea798a59cb8a71ac
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="how-toointegrate-engagement-reach-on-ios"></a>Jak tooIntegrate Engagement Reach v systému iOS
Je třeba postupovat podle hello integrace postup popsaný v hello [jak tooIntegrate Engagement iOS dokumentu](mobile-engagement-ios-integrate-engagement.md) před těchto pokynů.

Tato dokumentace vyžaduje XCode 8. Pokud ve skutečnosti závisí na XCode 7 pak můžete použít hello [iOS Engagement SDK v3.2.4](https://aka.ms/r6oouh). Je známého problému na tato předchozí verze při spuštění v zařízení s iOS 10: systémová oznámení nejsou reagovali. toofix to budete mít tooimplement hello zastaralá rozhraní API `application:didReceiveRemoteNotification:` ve vaší aplikaci delegovat následujícím způsobem:

    - (void)application:(UIApplication*)application
    didReceiveRemoteNotification:(NSDictionary*)userInfo
    {
        [[EngagementAgent shared] applicationDidReceiveRemoteNotification:userInfo fetchCompletionHandler:nil];
    }

> [!IMPORTANT]
> **Toto řešení nedoporučujeme** jako toto chování můžete změnit v jakéhokoli upgradu verze iOS nadcházející (i dílčí), protože tato rozhraní API pro iOS je zastaralý. TooXCode 8 musí přepnout co nejdříve.
>
>

### <a name="enable-your-app-tooreceive-silent-push-notifications"></a>Povolit vaší aplikace tooreceive bezobslužných nabízených oznámení
[!INCLUDE [mobile-engagement-ios-silent-push](../../includes/mobile-engagement-ios-silent-push.md)]

## <a name="integration-steps"></a>Kroky integrace
### <a name="embed-hello-engagement-reach-sdk-into-your-ios-project"></a>Vložení hello Engagement Reach SDK do projektu iOS
* Přidejte hello Reach sdk do projektu Xcode. V prostředí Xcode přejděte příliš**projektu \> přidat tooproject** a zvolte hello `EngagementReach` složky.

### <a name="modify-your-application-delegate"></a>Úprava delegáta aplikace
* Hello horní části souboru implementace importujte modul Engagement Reach hello:

      [...]
      #import "AEReachModule.h"
* Uvnitř metody `applicationDidFinishLaunching:` nebo `application:didFinishLaunchingWithOptions:`, vytvořte modul kampaně reach a předejte jej tooyour existujícího inicializačního řádku Engagement:

      - (BOOL)application:(UIApplication *)application didFinishLaunchingWithOptions:(NSDictionary *)launchOptions {
        AEReachModule* reach = [AEReachModule moduleWithNotificationIcon:[UIImage imageNamed:@"icon.png"]];
        [EngagementAgent init:@"Endpoint={YOUR_APP_COLLECTION.DOMAIN};SdkKey={YOUR_SDK_KEY};AppId={YOUR_APPID}" modules:reach, nil];
        [...]

        return YES;
      }
* Upravit **'icon.png'** řetězec s názvem hello image chcete použít jako ikona oznámení.
* Pokud chcete, aby možnost hello toouse *aktualizace oznámení "BADGE" hodnota* v kampaně Reach nebo pokud chcete, aby toouse nativního nabízení \<SaaS/Reach API nebo kampaň formátu/nativní nabízená\> kampaní, je nutné nechat modul Reach hello spravovat Hello oznámení "BADGE" ikonu samotné (bude automaticky vymaže oznámení "BADGE" hello aplikace a také obnovit hello hodnotě uložené Engagement pokaždé, když je aplikace hello spuštěn nebo foregrounded). To se provádí přidáním hello po inicializaci modulu Reach následující řádek:

      [reach setAutoBadgeEnabled:YES];
* Pokud chcete toohandle Reach datová oznámení, je nutné nechat delegáta aplikace odpovídat toohello `AEReachDataPushDelegate` protokolu. Přidejte následující řádek po inicializaci modulu Reach hello:

      [reach setDataPushDelegate:self];
* Pak můžete implementovat hello metody `onDataPushStringReceived:` a `onDataPushBase64ReceivedWithDecodedBody:andEncodedBody:` v delegáta aplikace:

      -(BOOL)didReceiveStringDataPushWithCategory:(NSString*)category body:(NSString*)body
      {
         NSLog(@"String data push message with category <%@> received: %@", category, body);
         return YES;
      }

      -(BOOL)didReceiveBase64DataPushWithCategory:(NSString*)category decodedBody:(NSData *)decodedBody encodedBody:(NSString *)encodedBody
      {
         NSLog(@"Base64 data push message with category <%@> received: %@", category, encodedBody);
         // Do something useful with decodedBody like updating an image view
         return YES;
      }

### <a name="category"></a>Kategorie
Hello kategorie parametr je volitelný při vytváření kampaň nabízených Data a umožňuje že vám toofilter data nabízených oznámení. To je užitečné, pokud chcete různé typy toopush z `Base64` dat a chcete tooidentify jejich typů před analýzou je.

**Aplikace je nyní připraven tooreceive a zobrazení dosáhnout obsah!**

## <a name="how-tooreceive-announcements-and-polls-at-any-time"></a>Jak tooreceive oznámení a hlasování kdykoli
Zapojení oznámení můžete odesílat Reach tooyour koncoví uživatelé kdykoli pomocí hello Apple Push Notification Service.

tooenable tuto funkci, budete mít tooprepare aplikace pro Apple nabízená oznámení a úprava delegáta aplikace.

### <a name="prepare-your-application-for-apple-push-notifications"></a>Příprava aplikace pro nabízená oznámení Apple
Postupujte podle průvodce hello: [jak tooPrepare aplikace pro nabízená oznámení Apple](https://developer.apple.com/library/ios/documentation/IDEs/Conceptual/AppDistributionGuide/AddingCapabilities/AddingCapabilities.html#//apple_ref/doc/uid/TP40012582-CH26-SW6)

### <a name="add-hello-necessary-client-code"></a>Přidejte kód klienta hello
*Aplikace v tomto okamžiku měli registrované nabízeného certifikátu Apple hello Engagement front-endu.*

Pokud už není potřeba, musíte tooregister aplikace tooreceive nabízených oznámení.

* Import hello `User Notification` framework:

        #import <UserNotifications/UserNotifications.h>
* Přidejte následující řádek při spuštění aplikace hello (obvykle ve `application:didFinishLaunchingWithOptions:`):

        if (NSFoundationVersionNumber >= NSFoundationVersionNumber_iOS_8_0)
        {
            if (NSFoundationVersionNumber > NSFoundationVersionNumber_iOS_9_x_Max)
            {
                [UNUserNotificationCenter.currentNotificationCenter requestAuthorizationWithOptions:(UNAuthorizationOptionBadge | UNAuthorizationOptionSound | UNAuthorizationOptionAlert) completionHandler:^(BOOL granted, NSError * _Nullable error) {}];
            }else
            {
                [application registerUserNotificationSettings:[UIUserNotificationSettings settingsForTypes:(UIUserNotificationTypeBadge | UIUserNotificationTypeSound | UIUserNotificationTypeAlert)   categories:nil]];
            }
            [application registerForRemoteNotifications];
        }
        else
        {
            [application registerForRemoteNotificationTypes:(UIRemoteNotificationTypeBadge | UIRemoteNotificationTypeSound | UIRemoteNotificationTypeAlert)];
        }

Pak musíte tooprovide tooEngagement hello zařízení token vrácený Apple servery. To se provádí v hello metodu s názvem `application:didRegisterForRemoteNotificationsWithDeviceToken:` v delegáta aplikace:

    - (void)application:(UIApplication*)application didRegisterForRemoteNotificationsWithDeviceToken:(NSData*)deviceToken
    {
        [[EngagementAgent shared] registerDeviceToken:deviceToken];
    }

Nakonec máte tooinform hello Engagement SDK, když aplikace přijímá vzdáleného oznámení. toodo, které volat hello metoda `applicationDidReceiveRemoteNotification:fetchCompletionHandler:` v delegáta aplikace:

    - (void)application:(UIApplication*)application didReceiveRemoteNotification:(NSDictionary*)userInfo fetchCompletionHandler:(void (^)(UIBackgroundFetchResult result))handler
    {
        [[EngagementAgent shared] applicationDidReceiveRemoteNotification:userInfo fetchCompletionHandler:handler];
    }

> [!IMPORTANT]
> Ve výchozím nastavení řídí Engagement Reach hello completionHandler. Pokud chcete, aby toomanually reakce toohello `handler` blokovat ve vašem kódu, můžete předat hodnotu nil pro hello `handler` argument a řízení dokončení hello blokovat sami. V tématu hello `UIBackgroundFetchResult` typu seznam možných hodnot.
>
>

### <a name="full-example"></a>Úplný příklad
Zde je úplná ukázka integrace:

    #pragma mark -
    #pragma mark Application lifecycle

    - (BOOL)application:(UIApplication*)application didFinishLaunchingWithOptions:(NSDictionary*)launchOptions
    {
      /* Reach module */
      AEReachModule* reach = [AEReachModule moduleWithNotificationIcon:[UIImage imageNamed:@"icon.png"]];
      [reach setAutoBadgeEnabled:YES];

      /* Engagement initialization */
      [EngagementAgent init:@"Endpoint={YOUR_APP_COLLECTION.DOMAIN};SdkKey={YOUR_SDK_KEY};AppId={YOUR_APPID}" modules:reach, nil];
      [[EngagementAgent shared] setPushDelegate:self];

      /* Views */
      [window addSubview:[tabBarController view]];
      [window makeKeyAndVisible];

      [application registerForRemoteNotificationTypes:UIRemoteNotificationTypeAlert|UIRemoteNotificationTypeBadge|UIRemoteNotificationTypeSound];
      return YES;
    }

    - (void)application:(UIApplication*)application didRegisterForRemoteNotificationsWithDeviceToken:(NSData*)deviceToken
    {
      [[EngagementAgent shared] registerDeviceToken:deviceToken];
    }

    - (void)application:(UIApplication *)application didReceiveRemoteNotification:(NSDictionary *)userInfo fetchCompletionHandler:(void (^)(UIBackgroundFetchResult result))handler
    {
        [[EngagementAgent shared] applicationDidReceiveRemoteNotification:userInfo fetchCompletionHandler:handler];
    }

### <a name="resolve-unusernotificationcenter-delegate-conflicts"></a>Řešení konfliktů UNUserNotificationCenter delegáta

*Pokud se implementuje ani aplikace, nebo jeden z vašich knihovnách třetích stran `UNUserNotificationCenterDelegate` pak můžete tuto část přeskočit.*

A `UNUserNotificationCenter` delegát používá hello SDK toomonitor hello životní cyklus Engagement oznámení na zařízení se systémem iOS, 10 nebo vyšší. Hello SDK má svou vlastní implementace hello `UNUserNotificationCenterDelegate` protokolu, ale může být jen jedna `UNUserNotificationCenter` delegovat na aplikaci. Přidat další delegáta toohello `UNUserNotificationCenter` budou v konfliktu s hello Engagement jeden objekt. Pokud hello SDK zjistí delegáta nebo jakékoli jiné třetí strany, pak jej nebude používat vlastní toogive implementaci vám tooresolve prvního hello je v konfliktu. Budete mít tooadd hello Engagement logiku tooyour vlastní delegáta v pořadí tooresolve hello konflikty.

Existují dva způsoby tooachieve to.

Návrh 1, jednoduše tak, že předávání delegát volání toohello SDK:

    #import <UIKit/UIKit.h>
    #import "EngagementAgent.h"
    #import <UserNotifications/UserNotifications.h>


    @interface MyAppDelegate : NSObject <UIApplicationDelegate, UNUserNotificationCenterDelegate>
    @end

    @implementation MyAppDelegate

    - (void)userNotificationCenter:(UNUserNotificationCenter *)center willPresentNotification:(UNNotification *)notification withCompletionHandler:(void (^)(UNNotificationPresentationOptions options))completionHandler
    {
      // Your own logic.

      [[EngagementAgent shared] userNotificationCenterWillPresentNotification:notification withCompletionHandler:completionHandler]
    }

    - (void)userNotificationCenter:(UNUserNotificationCenter *)center didReceiveNotificationResponse:(UNNotificationResponse *)response withCompletionHandler:(void(^)())completionHandler
    {
      // Your own logic.

      [[EngagementAgent shared] userNotificationCenterDidReceiveNotificationResponse:response withCompletionHandler:completionHandler]
    }
    @end

Návrh 2, která dědí z hello nebo `AEUserNotificationHandler` – třída

    #import "AEUserNotificationHandler.h"
    #import "EngagementAgent.h"

    @interface CustomUserNotificationHandler :AEUserNotificationHandler
    @end

    @implementation CustomUserNotificationHandler

    - (void)userNotificationCenter:(UNUserNotificationCenter *)center willPresentNotification:(UNNotification *)notification withCompletionHandler:(void (^)(UNNotificationPresentationOptions options))completionHandler
    {
      // Your own logic.

      [super userNotificationCenter:center willPresentNotification:notification withCompletionHandler:completionHandler];
    }

    - (void)userNotificationCenter:(UNUserNotificationCenter *)center didReceiveNotificationResponse: UNNotificationResponse *)response withCompletionHandler:(void(^)())completionHandler
    {
      // Your own logic.

      [super userNotificationCenter:center didReceiveNotificationResponse:response withCompletionHandler:completionHandler];
    }

    @end

> [!NOTE]
> Můžete určit, zda oznámení pochází z Engagement nebo není předáním jeho `userInfo` slovník toohello agenta `isEngagementPushPayload:` třídy metoda.

Ujistěte se, že hello `UNUserNotificationCenter` objektu delegáta nastavena delegáta tooyour v rámci buď hello `application:willFinishLaunchingWithOptions:` nebo hello `application:didFinishLaunchingWithOptions:` metoda delegáta aplikace.
Například pokud jste implementovali hello výše návrh 1:

      - (BOOL)application:(UIApplication *)application didFinishLaunchingWithOptions:(NSDictionary *)launchOptions {
        // Any other code

        [UNUserNotificationCenter currentNotificationCenter].delegate = self;
        return YES;
      }

## <a name="how-toocustomize-campaigns"></a>Jak toocustomize kampaně
### <a name="notifications"></a>Oznámení
Existují dva typy oznámení: oznámení systému a v aplikaci.

Systémová oznámení jsou zpracovávány iOS a nelze upravovat.

Oznámení v aplikaci probíhají zobrazení, která se dynamicky přidá toohello aktuální okno aplikace. Tomu se říká překrytí oznámení. Překryvy oznámení jsou výborně hodí pro rychlé integraci, protože se nevyžaduje, aby vám toomodify všechna zobrazení v aplikaci.

#### <a name="layout"></a>Rozložení
Vzhled hello toomodify oznámení v aplikaci, můžete jednoduše upravit soubor hello `AENotificationView.xib` tooyour potřebuje, dokud Udržovat hodnoty značky hello a typy dílčích hello existující zobrazení.

Ve výchozím nastavení se zobrazí oznámení v aplikaci v hello dolní části obrazovky hello. Pokud dáváte přednost toodisplay je hello horní části obrazovky, upravit hello poskytuje `AENotificationView.xib` a změňte hello `AutoSizing` vlastnost hello hlavního zobrazení, může být udržována na hello začátku jeho superview.

#### <a name="categories"></a>Kategorie
Při úpravě hello zadat rozložení upravíte vzhled hello všechna oznámení. Kategorie povolit, že jste toodefine, které se různé cílové hledá oznámení (pravděpodobně chování). Kategorie lze při vytváření kampaně Reach. Mějte na paměti, že kategorie vám také umožní přizpůsobit oznámení a hlasování, který je popsán dále v tomto dokumentu.

tooregister kategorie obslužnou rutinu pro oznámení, musíte tooadd inicializaci volání po dosažení hello modulu.

    AEReachModule* reach = [AEReachModule moduleWithNotificationIcon:[UIImage imageNamed:@"icon.png"]];
    [reach registerNotifier:myNotifier forCategory:@"my_category"];
    ...

`myNotifier`musí být instancí objektu, který vyhovuje protokolu toohello `AENotifier`.

Hello protokol metody můžete implementovat podle sami nebo můžete zvolit existující třídy tooreimplement hello `AEDefaultNotifier` kterém již provádí většinu práce hello.

Například pokud chcete zobrazit oznámení hello tooredefine pro určitou kategorii, proveďte tento příklad:

    #import "AEDefaultNotifier.h"
    #import "AENotificationView.h"
    @interface MyNotifier : AEDefaultNotifier
    @end

    @implementation MyNotifier

    -(NSString*)nibNameForCategory:(NSString*)category
    {
      return "MyNotificationView";
    }

    @end

Tento jednoduchý příklad kategorie Předpokládejme, že máte soubor s názvem `MyNotificationView.xib` ve vaší sady hlavní aplikace. Pokud metoda hello není možné toofind odpovídající `.xib`, hello oznámení se nezobrazí a Engagement na výstup zprávu, v konzole hello.

soubor nib Hello poskytuje by měly dodržovat hello následující pravidla:

* Měl by obsahovat pouze jedno zobrazení.
* Dílčích zobrazení by měla být ve stejné typy jako hello těch, které jsou uvnitř hello poskytuje nib soubor s názvem hello`AENotificationView.xib`
* Dílčích zobrazení musí mít stejné značky jako hello těch, které jsou uvnitř hello poskytuje nib soubor s názvem hello`AENotificationView.xib`

> [!TIP]
> Jednoduše zkopírujete soubor nib hello zadaný s názvem `AENotificationView.xib`a mohli začít pracovat z ní. Ale buďte opatrní, hello zobrazení uvnitř tento soubor nib je přidruženou třídu toohello `AENotificationView`. Tato třída předefinovat hello metoda `layoutSubViews` toomove a změňte velikost jeho dílčích zobrazení podle toocontext. Může být vhodné tooreplace její `UIView` nebo vlastní zobrazení třída.
>
>

Pokud potřebujete podrobnější přizpůsobení oznámení (Pokud chcete například tooload zobrazení přímo z kódu hello), se doporučuje tootake, podívejte se na hello zadaný zdroj kódu a třída dokumentaci `Protocol ReferencesDefaultNotifier` a `AENotifier`.

Všimněte si, že můžete použít hello téhož oznamovatele pro více kategorií.

Můžete také Předefinovaná hello výchozí oznamovatel takto:

    AEReachModule* reach = [AEReachModule moduleWithNotificationIcon:[UIImage imageNamed:@"icon.png"]];
    [reach registerNotifier:myNotifier forCategory:kAEReachDefaultCategory];

##### <a name="notification-handling"></a>Zpracování oznámení
Při použití hello výchozí kategorie, se nazývají některé metody životní cyklus na hello `AEReachContent` objektu tooreport statistiky a aktualizace hello kampaň stavu:

* Když se zobrazí hello oznámení v aplikaci, hello `displayNotification` metoda je volána (který sestavy statistik) podle `AEReachModule` Pokud `handleNotification:` vrátí `YES`.
* Pokud se zavře hello oznámení, hello `exitNotification` metoda je volána, statistiky se použije v hlášení a další kampaně lze nyní zpracovat.
* Po kliknutí na oznámení hello `actionNotification` je volána, statistiky se použije v hlášení a hello přidružené akce se provádí.

Pokud vaši implementaci `AENotifier` přeskočení hello výchozí chování, máte tyto metody životního cyklu pro toocall samotnými. Hello následující příklady ilustrují některých případech, kde přeskočí hello výchozí chování:

* Nerozšíříte `AEDefaultNotifier`, například implementována kategorie zpracování od začátku.
* Můžete overrode `prepareNotificationView:forContent:`, že toomap alespoň `onNotificationActioned` nebo `onNotificationExited` tooone U.I ovládacích prvků.

> [!WARNING]
> Pokud `handleNotification:` vyvolá výjimku, hello obsahu se odstraní a `drop` je volána, to je uvedený v statistiky a další kampaně lze nyní zpracovat.
>
>

#### <a name="include-notification-as-part-of-an-existing-view"></a>Zahrnout oznámení jako součást stávající zobrazení
Překryvy se výborně hodí pro rychlé integrace, ale může být někdy není vhodné nebo může mít nežádoucí vedlejší účinky.

Pokud nejste s hello překrytí systému v některé z vaší zobrazení, můžete jej přizpůsobit pro tato zobrazení.

Můžete rozhodnout, tooinclude naše rozložení oznámení ve vaší stávající zobrazení. toodo, se tedy dvě styly implementace:

1. Přidání zobrazení hello oznámení pomocí rozhraní tvůrce

   * Otevřete *rozhraní tvůrce*
   * Umístěte 320 x 60 (nebo 768 x 60, pokud jste na zařízení iPad) `UIView` místo tooappear oznámení hello
   * Nastavte hodnotu hello značky pro toto zobrazení příliš: **36822491**
2. Přidání zobrazení oznámení hello prostřednictvím kódu programu. Stačí přidáte hello následující kód, pokud byl inicializován zobrazení:

       UIView* notificationView = [[UIView alloc] initWithFrame:CGRectMake(0, 0, 320, 60)]; //Replace x and y coordinate values tooyour needs.
       notificationView.tag = NOTIFICATION_AREA_VIEW_TAG;
       [self.view addSubview:notificationView];

`NOTIFICATION_AREA_VIEW_TAG`Makro lze nalézt v `AEDefaultNotifier.h`.

> [!NOTE]
> Hello výchozí oznamovatel automaticky rozpozná tohoto oznámení rozložení hello je zahrnuta v tomto zobrazení a nebude pro něj přidat překrytí.
>
>

### <a name="announcements-and-polls"></a>Oznámení a hlasování
#### <a name="layouts"></a>Rozložení
Můžete upravit soubory hello `AEDefaultAnnouncementView.xib` a `AEDefaultPollView.xib` tak dlouho, dokud ponechat hodnoty značky hello a typy dílčích hello existující zobrazení.

#### <a name="categories"></a>Kategorie
##### <a name="alternate-layouts"></a>Alternativní rozložení
Jako oznámení může být hello kampaň kategorie používané toohave alternativní rozložení pro oznámení a hlasování.

toocreate kategorii pro oznámení, musíte rozšířit **AEAnnouncementViewController** a zaregistrovat ji, jakmile byla inicializována modul reach hello:

    AEReachModule* reach = [AEReachModule moduleWithNotificationIcon:[UIImage imageNamed:@"icon.png"]];
    [reach registerAnnouncementController:[MyCustomAnnouncementViewController class] forCategory:@"my_category"];

> [!NOTE]
> Pokaždé, když se uživatel kliknutím na oznámení pro oznámení s hello kategorie "Moje\_kategorie", řadiči registrované zobrazení (v takovém případě `MyCustomAnnouncementViewController`) bude inicializována pomocí volání metody hello `initWithAnnouncement:` a bude hello zobrazení Přidání toohello aktuální okno aplikace.
>
>

V implementaci hello `AEAnnouncementViewController` třída bude mít vlastnost hello tooread `announcement` tooinitialize vaše dílčích zobrazení. Vezměte v úvahu hello níže uvedeném příkladu, kde dva popisky jsou inicializována pomocí `title` a `body` vlastnosti hello `AEReachAnnouncement` třídy:

    -(void)loadView
    {
        [super loadView];

        UILabel* titleLabel = [[UILabel alloc] initWithFrame:CGRectMake(10, 20, 300, 60)];
        titleLabel.font = [UIFont systemFontOfSize:32.0];
        titleLabel.text = self.announcement.title;

        UILabel* bodyLabel = [[UILabel alloc] initWithFrame:CGRectMake(10, 20, 300, 60)];
        bodyLabel.font = [UIFont systemFontOfSize:24.0];
        bodyLabel.text = self.announcement.body;

        [self.view addSubview:titleLabel];
        [self.view addSubview:bodyLabel];
    }

Pokud nechcete, aby tooload zobrazení sami podle, ale chcete jenom tooreuse hello výchozí oznámení zobrazení rozložení, můžete jednoduše provést řadiči vlastní zobrazení rozšiřuje hello Zadaná třída `AEDefaultAnnouncementViewController`. V takovém případě duplicitní hello nib soubor `AEDefaultAnnouncementView.xib` a přejmenujte ji, mohou být načteny ve vašem řadiči vlastní zobrazení (pro řadič s názvem `CustomAnnouncementViewController`, by měly volat souboru nib `CustomAnnouncementView.xib`).

kategorie výchozí hello tooreplace oznámení, jednoduše zaregistrovat řadiči vlastní zobrazení pro kategorii hello definované v `kAEReachDefaultCategory`:

    [reach registerAnnouncementController:[MyCustomAnnouncementViewController class] forCategory:kAEReachDefaultCategory];

Hlasování může být vlastní hello stejným způsobem jako:

    AEReachModule* reach = [AEReachModule moduleWithNotificationIcon:[UIImage imageNamed:@"icon.png"]];
    [reach registerPollController:[MyCustomPollViewController class] forCategory:@"my_category"];

Tento čas, hello poskytuje `MyCustomPollViewController` musíte rozšířit `AEPollViewController`. Nebo můžete zvolit tooextend z řadiče výchozí hello: `AEDefaultPollViewController`.

> [!IMPORTANT]
> Nezapomeňte toocall buď `action` (`submitAnswers:` pro vlastní dotazování řadiče zobrazení) nebo `exit` metoda před řadiče zobrazení hello se zavře. Jinak statistiky nebude odeslána (tj. žádné analýzy kampaň hello) a další je nejdůležitější další kampaně nebudete upozorněni, až po restartování procesu aplikace hello.
>
>

##### <a name="implementation-example"></a>Příklad implementace
V této implementaci hello vlastní oznámení zobrazení je načten z externí xib souboru.

Jako přizpůsobení pokročilé oznámení se doporučuje toolook v hello zdrojový kód standardní implementace hello.

`CustomAnnouncementViewController.h`

    //Interface
    @interface CustomAnnouncementViewController : AEAnnouncementViewController {
      UILabel* titleLabel;
      UITextView* descTextView;
      UIWebView* htmlWebView;
      UIButton* okButton;
      UIButton* cancelButton;
    }

    @property (nonatomic, retain) IBOutlet UILabel* titleLabel;
    @property (nonatomic, retain) IBOutlet UITextView* descTextView;
    @property (nonatomic, retain) IBOutlet UIWebView* htmlWebView;
    @property (nonatomic, retain) IBOutlet UIButton* okButton;
    @property (nonatomic, retain) IBOutlet UIButton* cancelButton;

    -(IBAction)okButtonClicked:(id)sender;
    -(IBAction)cancelButtonClicked:(id)sender;

`CustomAnnouncementViewController.m`

    //Implementation
    @implementation CustomAnnouncementViewController
    @synthesize titleLabel;
    @synthesize descTextView;
    @synthesize htmlWebView;
    @synthesize okButton;
    @synthesize cancelButton;

    -(id)initWithAnnouncement:(AEReachAnnouncement*)anAnnouncement
    {
      self = [super initWithNibName:@"CustomAnnouncementViewController" bundle:nil];
      if (self != nil) {
        self.announcement = anAnnouncement;
      }
      return self;
    }

    - (void) dealloc
    {
      [titleLabel release];
      [descTextView release];
      [htmlWebView release];
      [okButton release];
      [cancelButton release];
      [super dealloc];
    }

    - (void)viewDidLoad {
      [super viewDidLoad];

      /* Init announcement title */
      titleLabel.text = self.announcement.title;

      /* Init announcement body */
      if(self.announcement.type == AEAnnouncementTypeHtml)
      {
        titleLabel.hidden = YES;
        htmlWebView.hidden = NO;
        [htmlWebView loadHTMLString:self.announcement.body baseURL:[NSURL URLWithString:@"http://localhost/"]];
      }
      else
      {
        titleLabel.hidden = NO;
        htmlWebView.hidden = YES;
        descTextView.text = self.announcement.body;
      }

      /* Set action button label */
      if([self.announcement.actionLabel length] > 0)
        [okButton setTitle:self.announcement.actionLabel forState:UIControlStateNormal];

      /* Set exit button label */
      if([self.announcement.exitLabel length] > 0)
        [cancelButton setTitle:self.announcement.exitLabel forState:UIControlStateNormal];
    }

    #pragma mark Actions

    -(IBAction)okButtonClicked:(id)sender
    {
        [self action];
    }

    -(IBAction)cancelButtonClicked:(id)sender
    {
        [self exit];
    }

    @end
