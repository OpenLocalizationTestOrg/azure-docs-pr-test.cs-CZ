---
title: "Most iOS webové zobrazení pomocí nativní Mobile Engagement iOS SDK"
description: "Popisuje postup vytvoření most mezi systémem Javascript a nativní Mobile Engagement iOS SDK webového zobrazení"
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
ms.openlocfilehash: 35f7bdbeb480122513ae2a0b04a6d8cfd426802a
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="bridge-ios-webview-with-native-mobile-engagement-ios-sdk"></a><span data-ttu-id="cf71e-103">Most iOS webové zobrazení pomocí nativní Mobile Engagement iOS SDK</span><span class="sxs-lookup"><span data-stu-id="cf71e-103">Bridge iOS WebView with native Mobile Engagement iOS SDK</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="cf71e-104">Most Android</span><span class="sxs-lookup"><span data-stu-id="cf71e-104">Android Bridge</span></span>](mobile-engagement-bridge-webview-native-android.md)
> * [<span data-ttu-id="cf71e-105">Most iOS</span><span class="sxs-lookup"><span data-stu-id="cf71e-105">iOS Bridge</span></span>](mobile-engagement-bridge-webview-native-ios.md)
> 
> 

<span data-ttu-id="cf71e-106">Některé mobilní aplikace slouží jako hybridní aplikace, kde je aplikace vyvinuté pomocí nativní aplikace pro iOS jazyka Objective-C vývoj, ale některé nebo všechny obrazovky jsou vykreslovány v rámci iOS webové zobrazení.</span><span class="sxs-lookup"><span data-stu-id="cf71e-106">Some mobile apps are designed as a hybrid app where the app itself is developed using native iOS Objective-C development but some or even all of the screens are rendered within an iOS WebView.</span></span> <span data-ttu-id="cf71e-107">Můžete i nadále využívat Mobile Engagement iOS SDK v rámci těchto aplikací a tento kurz popisuje, jak chcete-li přejít k provedení tohoto.</span><span class="sxs-lookup"><span data-stu-id="cf71e-107">You can still consume Mobile Engagement iOS SDK within such apps and this tutorial describes how to go about doing this.</span></span> 

<span data-ttu-id="cf71e-108">Jak toho docílit, když jsou obě nedokumentovanými dvěma způsoby:</span><span class="sxs-lookup"><span data-stu-id="cf71e-108">There are two approaches to achieve this though both are undocumented:</span></span>

* <span data-ttu-id="cf71e-109">Nejdřív jednu je popsaný v tomto [odkaz](http://stackoverflow.com/questions/9826792/how-to-invoke-objective-c-method-from-javascript-and-send-back-data-to-javascrip) což zahrnuje registrace `UIWebViewDelegate` na vaše webové zobrazení a catch a okamžitě Storno umístění změnu provést v jazyce Javascript.</span><span class="sxs-lookup"><span data-stu-id="cf71e-109">First one is described on this [link](http://stackoverflow.com/questions/9826792/how-to-invoke-objective-c-method-from-javascript-and-send-back-data-to-javascrip) which involves registering a `UIWebViewDelegate` on your web view and catch-and-immediatly-cancel a location change done in Javascript.</span></span> 
* <span data-ttu-id="cf71e-110">Druhý jeden je založena na tomto [WWDC 2013 relace](https://developer.apple.com/videos/play/wwdc2013/615), přístup, což je čisticí než první a který jsme bude následovat po této příručce.</span><span class="sxs-lookup"><span data-stu-id="cf71e-110">Second one is based on this [WWDC 2013 session](https://developer.apple.com/videos/play/wwdc2013/615), an approach which is cleaner than the first and which we will follow for this guide.</span></span> <span data-ttu-id="cf71e-111">Všimněte si, že tento přístup funguje jenom v systému IOS 7 a vyšší.</span><span class="sxs-lookup"><span data-stu-id="cf71e-111">Note that this approach only works on iOS7 and above.</span></span> 

<span data-ttu-id="cf71e-112">Pro iOS přemostění ukázkové, postupujte podle následujících kroků:</span><span class="sxs-lookup"><span data-stu-id="cf71e-112">Follow the steps below for the iOS bridge sample:</span></span>

1. <span data-ttu-id="cf71e-113">První řadě je potřeba zajistit, že jste prošli naše [kurzu Začínáme](mobile-engagement-ios-get-started.md) integrovat Mobile Engagement iOS SDK v hybridní aplikace.</span><span class="sxs-lookup"><span data-stu-id="cf71e-113">First of all, you need to ensure that you have gone through our [Getting Started tutorial](mobile-engagement-ios-get-started.md) to integrate the Mobile Engagement iOS SDK in your hybrid app.</span></span> <span data-ttu-id="cf71e-114">Volitelně můžete také povolit, testovací protokolování následujícím způsobem, aby mohli zobrazit metody SDK, jako je se spouští z webového zobrazení.</span><span class="sxs-lookup"><span data-stu-id="cf71e-114">Optionally, you can also enable test logging as follows so that you can see the SDK methods as we trigger them from the webview.</span></span> 
   
        - (BOOL)application:(UIApplication *)application didFinishLaunchingWithOptions:(NSDictionary *)launchOptions {
           ....
             [EngagementAgent setTestLogEnabled:YES];
           ....
        }
2. <span data-ttu-id="cf71e-115">Nyní se ujistěte, že hybridní aplikace má na obrazovce s webové zobrazení.</span><span class="sxs-lookup"><span data-stu-id="cf71e-115">Now make sure that your hybrid app has a screen with a webview on it.</span></span> <span data-ttu-id="cf71e-116">Můžete je přidat do `Main.storyboard` aplikace.</span><span class="sxs-lookup"><span data-stu-id="cf71e-116">You can add it to the `Main.storyboard` of the app.</span></span> 
3. <span data-ttu-id="cf71e-117">Přidružte tento webové zobrazení s vaší **ViewController** kliknutím a přetažením webové zobrazení z scény řadiče zobrazení k `ViewController.h` upravit obrazovky, jeho umístění právě níže `@interface` řádku.</span><span class="sxs-lookup"><span data-stu-id="cf71e-117">Associate this webview with your **ViewController** by clicking and dragging the webview from the View Controller Scene to the `ViewController.h` edit screen, placing it just below the `@interface` line.</span></span> 
4. <span data-ttu-id="cf71e-118">Jakmile to uděláte, objeví se dialogové okno s výzvou k zadání názvu.</span><span class="sxs-lookup"><span data-stu-id="cf71e-118">Once you do this, a dialog box will pop up asking for a name.</span></span> <span data-ttu-id="cf71e-119">Zadejte název ve tvaru **webové zobrazení**.</span><span class="sxs-lookup"><span data-stu-id="cf71e-119">Provide the name as **webView**.</span></span> <span data-ttu-id="cf71e-120">Vaše `ViewController.h` soubor by měl vypadat takto:</span><span class="sxs-lookup"><span data-stu-id="cf71e-120">Your `ViewController.h` file should look like the following:</span></span>
   
        #import <UIKit/UIKit.h>
        #import "EngagementViewController.h"
   
        @interface ViewController : EngagementViewController
        @property (strong, nonatomic) IBOutlet UIWebView *webView;
   
        @end
5. <span data-ttu-id="cf71e-121">Aktualizujeme `ViewController.m` , ale nejdřív vytvoříme soubor mostu, který vytvoří obálku přes některé běžně používané Mobile Engagement iOS SDK metody souboru později.</span><span class="sxs-lookup"><span data-stu-id="cf71e-121">We will update the `ViewController.m` file later but first we will create the bridge file which creates a wrapper over some commonly used Mobile Engagement iOS SDK methods.</span></span> <span data-ttu-id="cf71e-122">Vytvořte nový soubor záhlaví s názvem **EngagementJsExports.h** které používá `JSExport` mechanismus popsané ve zmíněném [relace](https://developer.apple.com/videos/play/wwdc2013/615) vystavit metody nativní aplikace pro iOS.</span><span class="sxs-lookup"><span data-stu-id="cf71e-122">Create a new header file called **EngagementJsExports.h** which uses the `JSExport` mechanism described in the aforementioned [session](https://developer.apple.com/videos/play/wwdc2013/615) to expose the native iOS methods.</span></span> 
   
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
6. <span data-ttu-id="cf71e-123">Dále vytvoříme druhou část souboru, most.</span><span class="sxs-lookup"><span data-stu-id="cf71e-123">Next we will create the second part of the bridge file.</span></span> <span data-ttu-id="cf71e-124">Vytvořte soubor s názvem **EngagementJsExports.m** který bude obsahovat implementace vytváření obálek skutečné voláním Mobile Engagement iOS SDK metod.</span><span class="sxs-lookup"><span data-stu-id="cf71e-124">Create a file called **EngagementJsExports.m** which will contain the implementation creating the actual wrappers by calling the Mobile Engagement iOS SDK methods.</span></span> <span data-ttu-id="cf71e-125">Také Upozorňujeme, že jsme analýza `extras` předávány z webového zobrazení javascript a umístění, do `NSMutableDictionary` objekt, který má být předána s volání metod Engagement SDK.</span><span class="sxs-lookup"><span data-stu-id="cf71e-125">Also note that we are parsing the `extras` being passed from the webview javascript and putting that into an `NSMutableDictionary` object to be passed with the Engagement SDK method calls.</span></span>  
   
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
7. <span data-ttu-id="cf71e-126">Nyní jsme se vraťte k **ViewController.m** a aktualizovat ji následujícím kódem:</span><span class="sxs-lookup"><span data-stu-id="cf71e-126">Now we come back to the **ViewController.m** and update it with the following code:</span></span> 
   
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
8. <span data-ttu-id="cf71e-127">Poznámka: následující body o **ViewController.m** souboru:</span><span class="sxs-lookup"><span data-stu-id="cf71e-127">Note the following points about the **ViewController.m** file:</span></span>
   
   * <span data-ttu-id="cf71e-128">V `loadWebView` metoda, jsme načítání do místního souboru HTML s názvem **LocalPage.html** jejíž kód jsme budou vedle zkontrolovat.</span><span class="sxs-lookup"><span data-stu-id="cf71e-128">In the `loadWebView` method, we are loading a local HTML file called **LocalPage.html** whose code we will review next.</span></span> 
   * <span data-ttu-id="cf71e-129">V `webViewDidFinishLoad` metoda, jsme metodou `JsContext` a přiřazení našeho obálkovou třídu s ním.</span><span class="sxs-lookup"><span data-stu-id="cf71e-129">In the `webViewDidFinishLoad` method, we are grabbing the `JsContext` and associating our wrapper class with it.</span></span> <span data-ttu-id="cf71e-130">To vám umožní volání metody SDK použitím popisovače, naše obálku **EngagementJs** z webové zobrazení.</span><span class="sxs-lookup"><span data-stu-id="cf71e-130">This will allow calling our wrapper SDK methods using the handle **EngagementJs** from the webView.</span></span> 
9. <span data-ttu-id="cf71e-131">Vytvořte soubor s názvem **LocalPage.html** následujícím kódem:</span><span class="sxs-lookup"><span data-stu-id="cf71e-131">Create a file called **LocalPage.html** with the following code:</span></span>
   
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
                       // Example of how extras info can be passed with the Engagement logs
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
10. <span data-ttu-id="cf71e-132">Vezměte na vědomí následující body týkající se souboru HTML výše:</span><span class="sxs-lookup"><span data-stu-id="cf71e-132">Note the following points about the HTML file above:</span></span>
    
    * <span data-ttu-id="cf71e-133">Obsahuje sadu vstupních polí, kde můžete zadat data, která má být použit jako názvy pro události, úlohy, chyba, AppInfo.</span><span class="sxs-lookup"><span data-stu-id="cf71e-133">It contains a set of input boxes where you can provide data to be used as names for your Event, Job, Error, AppInfo.</span></span> <span data-ttu-id="cf71e-134">Když kliknete na tlačítko vedle sebe, Přišla žádost o jazyka JavaScript, který nakonec volá metody ze souboru most předat volání Mobile Engagement iOS SDK.</span><span class="sxs-lookup"><span data-stu-id="cf71e-134">When you click on the button next to it, a call is made to the Javascript which eventually calls the methods from the bridge file to pass this call to the Mobile Engagement iOS SDK.</span></span> 
    * <span data-ttu-id="cf71e-135">Jsme jsou označování na některé statické doplňující informace o události, úlohy a k předvedení toho, jak to lze provést i chyby.</span><span class="sxs-lookup"><span data-stu-id="cf71e-135">We are tagging on some static extra info to the events, jobs and even errors to demonstrate how this could be done.</span></span> <span data-ttu-id="cf71e-136">Tyto doplňující informace o je odeslán jako řetězec JSON, který je-li hledat v `EngagementJsExports.m` souboru, rozdělování a předávání společně s odesílání událostí, úloh, chyb.</span><span class="sxs-lookup"><span data-stu-id="cf71e-136">This extra info is sent as a JSON string which, if you look in the `EngagementJsExports.m` file, is parsed and passed along with sending Events, Jobs, Errors.</span></span> 
    * <span data-ttu-id="cf71e-137">Mobile Engagement úloha je spuštěna s názvem spustit 10 sekund a zadáte v dialogovém okně vstupní vypnout.</span><span class="sxs-lookup"><span data-stu-id="cf71e-137">A Mobile Engagement Job is kicked off with the name you specify in the input box, run for 10 seconds and shut down.</span></span> 
    * <span data-ttu-id="cf71e-138">Mobile Engagement appinfo nebo značka, předá s 'jméno_zákazníka' jako statické klíč a hodnotu, kterou jste zadali v vstup jako hodnotu pro značku.</span><span class="sxs-lookup"><span data-stu-id="cf71e-138">A Mobile Engagement appinfo or tag is passed with 'customer_name' as the static key and the value that you entered in the input as the value for the tag.</span></span> 
11. <span data-ttu-id="cf71e-139">Spusťte aplikaci a zobrazí se následující.</span><span class="sxs-lookup"><span data-stu-id="cf71e-139">Run the app and you will see the following.</span></span> <span data-ttu-id="cf71e-140">Nyní zadejte nějaký název pro událost testování takto a klikněte na **odeslat** vedle sebe.</span><span class="sxs-lookup"><span data-stu-id="cf71e-140">Now provide some name for a test event like the following and click **Send** next to it.</span></span> 
    
     ![][1]
12. <span data-ttu-id="cf71e-141">Nyní, pokud přejdete do **monitorování** karta aplikace a naleznete v části **události -> Podrobnosti o**, zobrazí se tato událost objeví spolu s statické aplikace informace, které jsme odesílání.</span><span class="sxs-lookup"><span data-stu-id="cf71e-141">Now if you go to the **Monitor** tab of your app and look under **Events -> Details**, you will see this event show up along with the static app-info that we are sending.</span></span> 
    
    ![][2]

<!-- Images. -->
[1]: ./media/mobile-engagement-bridge-webview-native-ios/sending-event.png
[2]: ./media/mobile-engagement-bridge-webview-native-ios/event-output.png
