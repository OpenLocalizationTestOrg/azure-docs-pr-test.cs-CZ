---
title: "aaaBridge Android webové zobrazení s nativní Mobile Engagement Android SDK"
description: "Popisuje, jak toocreate most mezi webové zobrazení spuštěn Javascript a hello nativní Mobile Engagement Android SDK"
services: mobile-engagement
documentationcenter: mobile
author: piyushjo
manager: erikre
editor: 
ms.assetid: cf272f3f-2b09-41b1-b190-944cdca8bba2
ms.service: mobile-engagement
ms.workload: mobile
ms.tgt_pltfrm: mobile-android
ms.devlang: Java
ms.topic: article
ms.date: 08/19/2016
ms.author: piyushjo
ms.openlocfilehash: a7a09bcc156490fe69ad29a67809745dcfc22da6
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="bridge-android-webview-with-native-mobile-engagement-android-sdk"></a><span data-ttu-id="3f02c-103">Android webové zobrazení most s nativní Mobile Engagement Android SDK</span><span class="sxs-lookup"><span data-stu-id="3f02c-103">Bridge Android WebView with native Mobile Engagement Android SDK</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="3f02c-104">Most Android</span><span class="sxs-lookup"><span data-stu-id="3f02c-104">Android Bridge</span></span>](mobile-engagement-bridge-webview-native-android.md)
> * [<span data-ttu-id="3f02c-105">Most iOS</span><span class="sxs-lookup"><span data-stu-id="3f02c-105">iOS Bridge</span></span>](mobile-engagement-bridge-webview-native-ios.md)
> 
> 

<span data-ttu-id="3f02c-106">Některé mobilní aplikace slouží jako hybridní aplikace, kde je hello aplikace vyvinuté pomocí nativní vývoj pro Android, ale některé nebo všechny hello obrazovky jsou vykreslovány v rámci Android webové zobrazení.</span><span class="sxs-lookup"><span data-stu-id="3f02c-106">Some mobile apps are designed as a hybrid app where hello app itself is developed using native Android development but some or even all of hello screens are rendered within an Android WebView.</span></span> <span data-ttu-id="3f02c-107">Mobile Engagement Android SDK můžete i nadále využívat v rámci těchto aplikací a tento kurz popisuje, jak toogo o to.</span><span class="sxs-lookup"><span data-stu-id="3f02c-107">You can still consume Mobile Engagement Android SDK within such apps and this tutorial describes how toogo about doing this.</span></span> <span data-ttu-id="3f02c-108">Ukázkový kód Hello níže podle hello Android dokumentaci [zde](https://developer.android.com/guide/webapps/webview.html#BindingJavaScript).</span><span class="sxs-lookup"><span data-stu-id="3f02c-108">hello sample code below is based on hello Android documentation [here](https://developer.android.com/guide/webapps/webview.html#BindingJavaScript).</span></span> <span data-ttu-id="3f02c-109">Popisuje, jak by bylo možné tuto metodu zdokumentovaných tooimplement hello stejné pro Mobile Engagement Android SDK pro běžně používané metody tak, aby webové zobrazení z hybridní aplikace lze také zahájit události tootrack požadavků, úloh, chyb, app-info při jejich prostřednictvím potrubí Naše sady SDK pro Android.</span><span class="sxs-lookup"><span data-stu-id="3f02c-109">It describes how this documented approach could be used tooimplement hello same for Mobile Engagement Android SDK's commonly used methods such that a Webview from a hybrid app can also initiate requests tootrack events, jobs, errors, app-info while piping them via our Android SDK.</span></span> 

1. <span data-ttu-id="3f02c-110">Je třeba nejprve všech, tooensure, který jste prošli naše [kurzu Začínáme](mobile-engagement-android-get-started.md) toointegrate hello Mobile Engagement Android SDK v hybridní aplikace.</span><span class="sxs-lookup"><span data-stu-id="3f02c-110">First of all, you need tooensure that you have gone through our [Getting Started tutorial](mobile-engagement-android-get-started.md) toointegrate hello Mobile Engagement Android SDK in your hybrid app.</span></span> <span data-ttu-id="3f02c-111">Jakmile to uděláte, vaše `OnCreate` metoda bude vypadat podobně jako následující hello.</span><span class="sxs-lookup"><span data-stu-id="3f02c-111">Once you do that, your `OnCreate` method will look like hello following.</span></span>  
   
        @Override
        protected void onCreate(Bundle savedInstanceState) {
            super.onCreate(savedInstanceState);
            setContentView(R.layout.activity_main);
   
            EngagementConfiguration engagementConfiguration = new EngagementConfiguration();
            engagementConfiguration.setConnectionString("<Mobile Engagement Conn String>");
            EngagementAgent.getInstance(this).init(engagementConfiguration);
        }
2. <span data-ttu-id="3f02c-112">Nyní se ujistěte, že hybridní aplikace má na obrazovce s webové zobrazení.</span><span class="sxs-lookup"><span data-stu-id="3f02c-112">Now make sure that your hybrid app has a screen with a WebView on it.</span></span> <span data-ttu-id="3f02c-113">Hello kód pro něj bude podobné následující toohello kde jsme jsou načítání místní soubor HTML **Sample.html** v hello webové zobrazení v hello `onCreate` metoda obrazovky.</span><span class="sxs-lookup"><span data-stu-id="3f02c-113">hello code for it will be similar toohello following where we are loading a local HTML file **Sample.html** in hello Webview in hello `onCreate` method of your screen.</span></span> 
   
        private void SetWebView() {
            WebView myWebView = (WebView) findViewById(R.id.webview);
            myWebView.loadUrl("file:///android_asset/Sample.html");
        }
   
        protected void onCreate(Bundle savedInstanceState) {
            ...
            ...
            SetWebView();
        }
3. <span data-ttu-id="3f02c-114">Teď vytvořte most soubor s názvem **WebAppInterface** běžně vytváří obálku přes některé použít metody Mobile Engagement Android SDK pomocí hello `@JavascriptInterface` postupuje podle hello [Android dokumentace ](https://developer.android.com/guide/webapps/webview.html#BindingJavaScript):</span><span class="sxs-lookup"><span data-stu-id="3f02c-114">Now create a bridge file called **WebAppInterface** which creates a wrapper over some commonly used Mobile Engagement Android SDK methods using hello `@JavascriptInterface` approach described in hello [Android documentation](https://developer.android.com/guide/webapps/webview.html#BindingJavaScript):</span></span>
   
        import android.content.Context;
        import android.os.Bundle;
        import android.util.Log;
        import android.webkit.JavascriptInterface;
   
        import com.microsoft.azure.engagement.EngagementAgent;
   
        import org.json.JSONArray;
        import org.json.JSONObject;
   
        public class WebAppInterface {
            Context mContext;
   
            /** Instantiate hello interface and set hello context */
            WebAppInterface(Context c) {
                mContext = c;
            }
   
            @JavascriptInterface
            public void sendEngagementEvent(String name, String extras ){
                EngagementAgent.getInstance(mContext).sendEvent(name, ParseExtras(extras));
            }
   
            @JavascriptInterface
            public void startEngagementJob(String name, String extras ){
                EngagementAgent.getInstance(mContext).startJob(name, ParseExtras(extras));
            }
   
            @JavascriptInterface
            public void endEngagementJob(String name){
                EngagementAgent.getInstance(mContext).endJob(name);
            }
   
            @JavascriptInterface
            public void sendEngagementError(String name, String extras ){
                EngagementAgent.getInstance(mContext).sendError(name, ParseExtras(extras));
            }
   
            @JavascriptInterface
            public void sendEngagementAppInfo(String appInfo){
                EngagementAgent.getInstance(mContext).sendAppInfo(ParseExtras(appInfo));
            }
   
            public Bundle ParseExtras(String input) {
                Bundle extras = new Bundle();
   
                try {
                    JSONObject jObject = new JSONObject(input);
                    extras.putString(jObject.names().getString(0),
                            jObject.get(jObject.names().getString(0)).toString());
                } catch (Exception e) {
                    e.printStackTrace();
                }
                return extras;
            }
        }  
4. <span data-ttu-id="3f02c-115">Jakmile jsme vytvořili hello výše most souboru, potřebujeme tooensure, že je spojen s naše webové zobrazení.</span><span class="sxs-lookup"><span data-stu-id="3f02c-115">Once we have created hello above bridge file, we need tooensure that it is associated with our Webview.</span></span> <span data-ttu-id="3f02c-116">Pro tento toohappen, je třeba tooedit vaše `SetWebview` metoda, takže to vypadá hello následující:</span><span class="sxs-lookup"><span data-stu-id="3f02c-116">For this toohappen, you need tooedit your `SetWebview` method so that it looks like hello following:</span></span>
   
        private void SetWebView() {
            WebView myWebView = (WebView) findViewById(R.id.webview);
            myWebView.loadUrl("file:///android_asset/Sample.html");
            WebSettings webSettings = myWebView.getSettings();
            webSettings.setJavaScriptEnabled(true);
            myWebView.addJavascriptInterface(new WebAppInterface(this), "EngagementJs");
        }
5. <span data-ttu-id="3f02c-117">V hello výše fragment kódu, volali jsme `addJavascriptInterface` tooassociate naše most s naše webové zobrazení třídy a také vytvořit popisovač názvem **EngagementJs** toocall hello metody ze souboru most hello.</span><span class="sxs-lookup"><span data-stu-id="3f02c-117">In hello above snippet, we called `addJavascriptInterface` tooassociate our bridge class with our Webview and also created a handle called **EngagementJs** toocall hello methods from hello bridge file.</span></span> 
6. <span data-ttu-id="3f02c-118">Nyní vytvoří hello následující soubor s názvem **Sample.html** ve projekt ve složce s názvem **prostředky** který je načten do hello webové zobrazení a kde hello metody bude volat ze souboru most hello.</span><span class="sxs-lookup"><span data-stu-id="3f02c-118">Now create hello following file called **Sample.html** in your project in a folder called **assets** which is loaded into hello Webview and where we will call hello methods from hello bridge file.</span></span>
   
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
                      log('window.onerror: ' + err);
                    }
   
                    send = function(inputId)
                    {
                        var input = document.getElementById(inputId);
                        if(input)
                        {
                            var value = input.value;
                            // Example of how extras info can be passed with hello Engagement logs
                            var extras = '{"CustomerId":"MS290011"}';
   
                            if(value && value.length > 0)
                            {
                                switch(inputId)
                                {
                                    case "event":
                                    EngagementJs.sendEngagementEvent(value, extras);
                                    break;
   
                                    case "job":
                                    EngagementJs.startEngagementJob(value, extras);
                                    window.setTimeout( function()
                                    {
                                      EngagementJs.endEngagementJob(value);
                                    }, 10000 );
                                    break;
   
                                    case "error":
                                    EngagementJs.sendEngagementError(value, extras);
                                    break;
   
                                    case "appInfo":
                                    EngagementJs.sendEngagementAppInfo({"customer_name":value});
                                    break;
                                }
                            }
                        }
                    }
                </script>
            </head>
            <body>
                <h1>Bridge Tester</h1>
                <div id='engagement'>
                    <h2>Event</h2>
                    <input type="text" id="event" size="35">
                    <button onclick="send('event')">Send</button>
   
                    <h2>Job</h2>
                    <input type="text" id="job" size="35">
                    <button onclick="send('job')">Send</button>
   
                    <h2>Error</h2>
                    <input type="text" id="error" size="35">
                    <button onclick="send('error')">Send</button>
   
                    <h2>AppInfo</h2>
                    <input type="text" id="appInfo" size="35">
                    <button onclick="send('appInfo')">Send</button>
                </div>
            </body>
        </html>
7. <span data-ttu-id="3f02c-119">Poznámka: hello následující body o soubor HTML hello výše:</span><span class="sxs-lookup"><span data-stu-id="3f02c-119">Note hello following points about hello HTML file above:</span></span>
   
   * <span data-ttu-id="3f02c-120">Obsahuje sadu vstupních polí, kde je možné poskytnout toobe dat použít jako názvy pro události, úlohy, chyba, AppInfo.</span><span class="sxs-lookup"><span data-stu-id="3f02c-120">It contains a set of input boxes where you can provide data toobe used as names for your Event, Job, Error, AppInfo.</span></span> <span data-ttu-id="3f02c-121">Když kliknete na další tooit hello tlačítko, Přišla žádost o toohello Javascript, který nakonec volání metody hello ze hello most souboru toopass toohello toto volání Mobile Engagement Android SDK.</span><span class="sxs-lookup"><span data-stu-id="3f02c-121">When you click on hello button next tooit, a call is made toohello Javascript which eventually calls hello methods from hello bridge file toopass this call toohello Mobile Engagement Android SDK.</span></span> 
   * <span data-ttu-id="3f02c-122">Jsme jsou označování na některé události toohello statické doplňující informace, úlohy a i chyby toodemonstrate jak to lze provést.</span><span class="sxs-lookup"><span data-stu-id="3f02c-122">We are tagging on some static extra info toohello events, jobs and even errors toodemonstrate how this could be done.</span></span> <span data-ttu-id="3f02c-123">Tyto další informace o je odeslán jako řetězec JSON, který je-li hledat v hello `WebAppInterface` souboru, analyzovat a umístí Android `Bundle` a je předána společně s odesílání událostí, úloh, chyb.</span><span class="sxs-lookup"><span data-stu-id="3f02c-123">This extra info is sent as a JSON string which, if you look in hello `WebAppInterface` file, is parsed and put in an Android `Bundle` and is passed along with sending Events, Jobs, Errors.</span></span> 
   * <span data-ttu-id="3f02c-124">Mobile Engagement úloha je spuštěna s názvem hello zadáte hello vstupní pole, spusťte pro 10 sekund a vypnout.</span><span class="sxs-lookup"><span data-stu-id="3f02c-124">A Mobile Engagement Job is kicked off with hello name you specify in hello input box, run for 10 seconds and shut down.</span></span> 
   * <span data-ttu-id="3f02c-125">Mobile Engagement appinfo nebo značka, se předá s 'jméno_zákazníka' jako hello statické klíč a hodnotu hello, kterou jste zadali v hello vstup jako hodnotu hello hello značky.</span><span class="sxs-lookup"><span data-stu-id="3f02c-125">A Mobile Engagement appinfo or tag is passed with 'customer_name' as hello static key and hello value that you entered in hello input as hello value for hello tag.</span></span> 
8. <span data-ttu-id="3f02c-126">Spuštění hello aplikace a můžete se zobrazí následující hello.</span><span class="sxs-lookup"><span data-stu-id="3f02c-126">Run hello app and you will see hello following.</span></span> <span data-ttu-id="3f02c-127">Nyní zadejte nějaký název pro událost testování jako hello následující a klikněte na **odeslat** pod ním.</span><span class="sxs-lookup"><span data-stu-id="3f02c-127">Now provide some name for a test event like hello following and click **Send** below it.</span></span> 
   
    ![][1]
9. <span data-ttu-id="3f02c-128">Nyní, pokud přejdete toohello **monitorování** karta aplikace a naleznete v části **události -> Podrobnosti o**, zobrazí se tato událost objeví spolu s hello statické aplikace informací, které jsme odesílání.</span><span class="sxs-lookup"><span data-stu-id="3f02c-128">Now if you go toohello **Monitor** tab of your app and look under **Events -> Details**, you will see this event show up along with hello static app-info that we are sending.</span></span> 
   
   ![][2]

<!-- Images. -->
[1]: ./media/mobile-engagement-bridge-webview-native-android/sending-event.png
[2]: ./media/mobile-engagement-bridge-webview-native-android/event-output.png
