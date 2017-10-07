---
title: "iOS aaaBridge webové zobrazení pomocí nativní Mobile Engagement iOS SDK"
description: "Popisuje, jak toocreate most mezi systémem Javascript a hello nativní Mobile Engagement iOS SDK webového zobrazení"
services: mobile-engagement
documentationcenter: mobile
author: piyushjo
manager: erikre
editor: 
ms.assetid: e1d6ff6f-cd67-4131-96eb-c3d6318de752
ms.service: mobile-engagement
ms.workload: mobile
ms.tgt_pltfrm: mobile-ios
ms.devlang: objective-c
ms.topic: article
ms.date: 08/19/2016
ms.author: piyushjo
ms.openlocfilehash: 089ed8484722cb5ba624e5dce0e670ab56de514d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="bridge-ios-webview-with-native-mobile-engagement-ios-sdk"></a>Most iOS webové zobrazení pomocí nativní Mobile Engagement iOS SDK
> [!div class="op_single_selector"]
> * [Most Android](mobile-engagement-bridge-webview-native-android.md)
> * [Most iOS](mobile-engagement-bridge-webview-native-ios.md)
> 
> 

Některé mobilní aplikace slouží jako hybridní aplikace, kde je hello aplikace vyvinuté pomocí nativní aplikace pro iOS jazyka Objective-C vývoj, ale některé nebo všechny hello obrazovky jsou vykreslovány v rámci iOS webové zobrazení. Můžete i nadále využívat Mobile Engagement iOS SDK v rámci těchto aplikací a tento kurz popisuje, jak toogo o to. 

Existují dva přístupy tooachieve to i když jsou obě nedokumentovanými:

* Nejdřív jednu je popsaný v tomto [odkaz](http://stackoverflow.com/questions/9826792/how-to-invoke-objective-c-method-from-javascript-and-send-back-data-to-javascrip) což zahrnuje registrace `UIWebViewDelegate` na vaše webové zobrazení a catch a okamžitě Storno umístění změnu provést v jazyce Javascript. 
* Druhý jeden je založena na tomto [WWDC 2013 relace](https://developer.apple.com/videos/play/wwdc2013/615), přístup, což je první čisticí než hello a který jsme bude následovat po této příručce. Všimněte si, že tento přístup funguje jenom v systému IOS 7 a vyšší. 

Pro hello iOS přemostění ukázkové, postupujte podle následujících kroků hello:

1. Je třeba nejprve všech, tooensure, který jste prošli naše [kurzu Začínáme](mobile-engagement-ios-get-started.md) toointegrate hello Mobile Engagement iOS SDK v hybridní aplikace. Volitelně můžete také povolit, testovací protokolování následujícím způsobem, aby mohli zobrazit hello SDK metody, jako je se spouští z webového zobrazení hello. 
   
        - (BOOL)application:(UIApplication *)application didFinishLaunchingWithOptions:(NSDictionary *)launchOptions {
           ....
             [EngagementAgent setTestLogEnabled:YES];
           ....
        }
2. Nyní se ujistěte, že hybridní aplikace má na obrazovce s webové zobrazení. Můžete jej přidat toohello `Main.storyboard` aplikace hello. 
3. Přidružte tento webové zobrazení s vaší **ViewController** kliknutím a přetažením webové zobrazení hello z hello scény řadiče zobrazení toohello `ViewController.h` upravit obrazovky, jeho umístění pod hello `@interface` řádku. 
4. Jakmile to uděláte, objeví se dialogové okno s výzvou k zadání názvu. Zadejte název hello jako **webové zobrazení**. Vaše `ViewController.h` soubor by měl vypadat jako následující hello:
   
        #import <UIKit/UIKit.h>
        #import "EngagementViewController.h"
   
        @interface ViewController : EngagementViewController
        @property (strong, nonatomic) IBOutlet UIWebView *webView;
   
        @end
5. Aktualizujeme hello `ViewController.m` , ale nejdřív vytvoříme hello most souboru, který vytvoří obálku přes některé běžně používané Mobile Engagement iOS SDK metody souboru později. Vytvořte nový soubor záhlaví s názvem **EngagementJsExports.h** , který používá hello `JSExport` mechanismus popsané v hello zmíněnými [relace](https://developer.apple.com/videos/play/wwdc2013/615) tooexpose hello nativní aplikace pro iOS metody. 
   
        #import <Foundation/Foundation.h>
        #import <JavaScriptCore/JavascriptCore.h>
   
        @protocol EngagementJsExports <JSExport>
   
        + (void) sendEngagementEvent:(NSString*) name :(NSString*)extras;
        + (void) startEngagementJob:(NSString*) name :(NSString*)extras;
        + (void) endEngagementJob:(NSString*) name;
        + (void) sendEngagementError:(NSString*) name :(NSString*)extras;
        + (void) sendEngagementAppInfo:(NSString*) appInfo;
   
        @end
   
        @interface EngagementJs : NSObject <EngagementJsExports>
   
        @end
6. Dále vytvoříme hello druhé části souboru most hello. Vytvořte soubor s názvem **EngagementJsExports.m** který bude obsahovat hello implementace vytváření hello skutečné obálky voláním hello Mobile Engagement iOS SDK metod. Všimněte si také, že jsme se analýza hello `extras` předávány z hello webové zobrazení javascript a umístění, do `NSMutableDictionary` objektu toobe byla dokončena s hello Engagement SDK metoda volání.  
   
        #import <UIKit/UIKit.h>
        #import "EngagementAgent.h"
        #import "EngagementJsExports.h"
   
        @implementation EngagementJs
   
        +(void) sendEngagementEvent:(NSString*)name :(NSString*)extras {
           NSMutableDictionary* extrasInput = [self ParseExtras:extras];
           [[EngagementAgent shared] sendEvent:name extras:extrasInput];
        }
   
        + (void) startEngagementJob:(NSString*) name :(NSString*)extras {
           NSMutableDictionary* extrasInput = [self ParseExtras:extras];
           [[EngagementAgent shared] startJob:name extras:extrasInput];
        }
   
        + (void) endEngagementJob:(NSString*) name {
           [[EngagementAgent shared] endJob:name];
        }
   
        + (void) sendEngagementError:(NSString*) name :(NSString*)extras {
           NSMutableDictionary* extrasInput = [self ParseExtras:extras];
           [[EngagementAgent shared] sendError:name extras:extrasInput];
        }
   
        + (void) sendEngagementAppInfo:(NSString*) appInfo {
           NSMutableDictionary* appInfoInput = [self ParseExtras:appInfo];
           [[EngagementAgent shared] sendAppInfo:appInfoInput];
        }
   
        + (NSMutableDictionary*) ParseExtras:(NSString*) input {
           NSData *data = [input dataUsingEncoding:NSUTF8StringEncoding];
           NSError* error = nil;
           NSMutableDictionary* extras = [NSJSONSerialization JSONObjectWithData:data options:0 error:&error];
   
           return extras;
        }
   
        @end
7. Nyní jsme vraťte toohello **ViewController.m** a aktualizujte pomocí hello následující kód: 
   
        #import <JavaScriptCore/JavaScriptCore.h>
        #import "ViewController.h"
        #import "EngagementJsExports.h"
   
        @interface ViewController ()
   
        @end
   
        @implementation ViewController
   
        - (void)viewDidLoad
        {
           self.webView.delegate = self;
           [super viewDidLoad];
           [self loadWebView];
        }
   
        - (void)loadWebView {
           NSBundle* mainBundle = [NSBundle mainBundle];
           NSURL* htmlPage = [mainBundle URLForResource:@"LocalPage" withExtension:@"html"];
   
           NSURLRequest* urlReq = [NSURLRequest requestWithURL:htmlPage];
           [self.webView loadRequest:urlReq];
        }
   
        - (void)webViewDidFinishLoad:(UIWebView*)wv
        {
           JSContext* context = [wv valueForKeyPath:@"documentView.webView.mainFrame.javaScriptContext"];
   
           context[@"EngagementJs"] = [EngagementJs class];
        }
   
        - (void)webView:(UIWebView*)wv didFailLoadWithError:(NSError*)error
        {
           NSLog(@"Error for WEBVIEW: %@", [error description]);
        }
   
        - (void)didReceiveMemoryWarning {
           [super didReceiveMemoryWarning];
           // Dispose of any resources that can be recreated.
        }
   
        @end
8. Poznámka: hello následující body o hello **ViewController.m** souboru:
   
   * V hello `loadWebView` metoda, jsme načítání do místního souboru HTML s názvem **LocalPage.html** jejíž kód jsme budou vedle zkontrolovat. 
   * V hello `webViewDidFinishLoad` metoda, jsme jsou metodou hello `JsContext` a přiřazení našeho obálkovou třídu s ním. To vám umožní volání metody SDK pomocí hello popisovač naše obálku **EngagementJs** z webového zobrazení hello. 
9. Vytvořte soubor s názvem **LocalPage.html** s hello následující kód:
   
        <!doctype html>
        <html>
           <head>
               <style type='text/css'>
                   html { font-family:Helvetica; color:#222; }
                   h1 { color:steelblue; font-size:22px; margin-top:16px; }
                   h2 { color:grey; font-size:14px; margin-top:18px; margin-bottom:0px; }
               </style>
   
               <script type="text/javascript">
   
               window.onerror = function(err)
               {
                   alert('window.onerror: ' + err);
               }
   
               function send(inputId)
               {
                   var input = document.getElementById(inputId);
                   if(input)
                   {
                       var value = input.value;
                       // Example of how extras info can be passed with hello Engagement logs
                       var extras = '{"CustomerId":"MS290011"}';
                   }
   
                   if(value && value.length > 0)
                   {
                       switch(inputId)
                       {
                           case "event":
                           EngagementJs.sendEngagementEvent(value, extras);
                           break;
   
                           case "job":
                           EngagementJs.startEngagementJob(value, extras);
                           window.setTimeout(
                           function(){
                               EngagementJs.endEngagementJob(value);
                           }, 10000 );
                           break;
   
                           case "error":
                           EngagementJs.sendEngagementError(value, extras);
                           break;
   
                           case "appInfo":
                           var appInfo = '{"customer_name":"' + value + '"}';
                           EngagementJs.sendEngagementAppInfo(appInfo);
                           break;
                       }
                   }
               }
               </script>
   
           </head>
           <body>
               <h1>Bridge Tester</h1>
   
               <div id='engagement'>
   
                   <br/>
                   <h2>Event</h2>
                   <input type="text" id="event" size="35">
                   <button onclick="send('event')">Send</button>
   
                   <br/>
                   <h2>Job</h2>
                   <input type="text" id="job" size="35">
                   <button onclick="send('job')">Send</button>
   
                   <br/>
                   <h2>Error</h2>
                   <input type="text" id="error" size="35">
                   <button onclick="send('error')">Send</button
   
                   <br/>
                   <h2>AppInfo</h2>
                   <input type="text" id="appInfo" size="35">
                   <button onclick="send('appInfo')">Send</button>
   
               </div>
           </body>
        </html>
10. Poznámka: hello následující body o soubor HTML hello výše:
    
    * Obsahuje sadu vstupních polí, kde je možné poskytnout toobe dat použít jako názvy pro události, úlohy, chyba, AppInfo. Když kliknete na další tooit hello tlačítko, Přišla žádost o toohello Javascript, který nakonec volání metody hello ze hello most souboru toopass toohello toto volání Mobile Engagement iOS SDK. 
    * Jsme jsou označování na některé události toohello statické doplňující informace, úlohy a i chyby toodemonstrate jak to lze provést. Tyto další informace o je odeslán jako řetězec JSON, který je-li hledat v hello `EngagementJsExports.m` souboru, rozdělování a předávání společně s odesílání událostí, úloh, chyb. 
    * Mobile Engagement úloha je spuštěna s názvem hello zadáte hello vstupní pole, spusťte pro 10 sekund a vypnout. 
    * Mobile Engagement appinfo nebo značka, se předá s 'jméno_zákazníka' jako hello statické klíč a hodnotu hello, kterou jste zadali v hello vstup jako hodnotu hello hello značky. 
11. Spuštění hello aplikace a můžete se zobrazí následující hello. Nyní zadejte nějaký název pro událost testování jako hello následující a klikněte na **odeslat** další tooit. 
    
     ![][1]
12. Nyní, pokud přejdete toohello **monitorování** karta aplikace a naleznete v části **události -> Podrobnosti o**, zobrazí se tato událost objeví spolu s hello statické aplikace informací, které jsme odesílání. 
    
    ![][2]

<!-- Images. -->
[1]: ./media/mobile-engagement-bridge-webview-native-ios/sending-event.png
[2]: ./media/mobile-engagement-bridge-webview-native-ios/event-output.png
