---
title: "Android webové zobrazení most s nativní Mobile Engagement Android SDK"
description: "Popisuje postup vytvoření most mezi systémem Javascript a nativní Mobile Engagement Android SDK webového zobrazení"
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
ms.openlocfilehash: f4fc7b3c81747ec80974a99084eeb1acc311f11f
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="bridge-android-webview-with-native-mobile-engagement-android-sdk"></a><span data-ttu-id="6c957-103">Android webové zobrazení most s nativní Mobile Engagement Android SDK</span><span class="sxs-lookup"><span data-stu-id="6c957-103">Bridge Android WebView with native Mobile Engagement Android SDK</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="6c957-104">Most Android</span><span class="sxs-lookup"><span data-stu-id="6c957-104">Android Bridge</span></span>](mobile-engagement-bridge-webview-native-android.md)
> * [<span data-ttu-id="6c957-105">Most iOS</span><span class="sxs-lookup"><span data-stu-id="6c957-105">iOS Bridge</span></span>](mobile-engagement-bridge-webview-native-ios.md)
> 
> 

<span data-ttu-id="6c957-106">Některé mobilní aplikace slouží jako hybridní aplikace, kde je aplikace vyvinuté pomocí nativní vývoj pro Android, ale některé nebo všechny obrazovky jsou vykreslovány v rámci Android webové zobrazení.</span><span class="sxs-lookup"><span data-stu-id="6c957-106">Some mobile apps are designed as a hybrid app where the app itself is developed using native Android development but some or even all of the screens are rendered within an Android WebView.</span></span> <span data-ttu-id="6c957-107">Mobile Engagement Android SDK můžete i nadále využívat v rámci těchto aplikací a tento kurz popisuje, jak chcete-li přejít k provedení tohoto.</span><span class="sxs-lookup"><span data-stu-id="6c957-107">You can still consume Mobile Engagement Android SDK within such apps and this tutorial describes how to go about doing this.</span></span> <span data-ttu-id="6c957-108">Následující ukázkový kód podle Android dokumentaci [zde](https://developer.android.com/guide/webapps/webview.html#BindingJavaScript).</span><span class="sxs-lookup"><span data-stu-id="6c957-108">The sample code below is based on the Android documentation [here](https://developer.android.com/guide/webapps/webview.html#BindingJavaScript).</span></span> <span data-ttu-id="6c957-109">Popisuje, jak tento zdokumentovaných přístup může k implementaci pro běžně používané metody Mobile Engagement Android SDK na stejný tak, aby webové zobrazení z hybridní aplikace lze také zahájit žádosti o sledování událostí, úloh, chyb, app-info při jejich potrubí pomocí naší sady SDK pro Android.</span><span class="sxs-lookup"><span data-stu-id="6c957-109">It describes how this documented approach could be used to implement the same for Mobile Engagement Android SDK's commonly used methods such that a Webview from a hybrid app can also initiate requests to track events, jobs, errors, app-info while piping them via our Android SDK.</span></span> 

1. <span data-ttu-id="6c957-110">První řadě je potřeba zajistit, že jste prošli naše [kurzu Začínáme](mobile-engagement-android-get-started.md) integrovat Mobile Engagement Android SDK v hybridní aplikace.</span><span class="sxs-lookup"><span data-stu-id="6c957-110">First of all, you need to ensure that you have gone through our [Getting Started tutorial](mobile-engagement-android-get-started.md) to integrate the Mobile Engagement Android SDK in your hybrid app.</span></span> <span data-ttu-id="6c957-111">Jakmile to uděláte, vaše `OnCreate` metoda bude vypadat podobně jako následující.</span><span class="sxs-lookup"><span data-stu-id="6c957-111">Once you do that, your `OnCreate` method will look like the following.</span></span>  
   
        @Override
        protected void onCreate(Bundle savedInstanceState) {
            super.onCreate(savedInstanceState);
            setContentView(R.layout.activity_main);
   
            EngagementConfiguration engagementConfiguration = new EngagementConfiguration();
            engagementConfiguration.setConnectionString("<Mobile Engagement Conn String>");
            EngagementAgent.getInstance(this).init(engagementConfiguration);
        }
2. <span data-ttu-id="6c957-112">Nyní se ujistěte, že hybridní aplikace má na obrazovce s webové zobrazení.</span><span class="sxs-lookup"><span data-stu-id="6c957-112">Now make sure that your hybrid app has a screen with a WebView on it.</span></span> <span data-ttu-id="6c957-113">Kód pro něj bude podobný následujícímu kde jsme jsou načítání místní soubor HTML **Sample.html** ve webovém zobrazení v `onCreate` metoda obrazovky.</span><span class="sxs-lookup"><span data-stu-id="6c957-113">The code for it will be similar to the following where we are loading a local HTML file **Sample.html** in the Webview in the `onCreate` method of your screen.</span></span> 
   
        private void SetWebView() {
            WebView myWebView = (WebView) findViewById(R.id.webview);
            myWebView.loadUrl("file:///android_asset/Sample.html");
        }
   
        protected void onCreate(Bundle savedInstanceState) {
            ...
            ...
            SetWebView();
        }
3. <span data-ttu-id="6c957-114">Nyní vytvořte most soubor s názvem **WebAppInterface** běžně vytváří obálku přes některé použít metody Mobile Engagement Android SDK, pomocí `@JavascriptInterface` postup uvedený v [Android dokumentaci](https://developer.android.com/guide/webapps/webview.html#BindingJavaScript):</span><span class="sxs-lookup"><span data-stu-id="6c957-114">Now create a bridge file called **WebAppInterface** which creates a wrapper over some commonly used Mobile Engagement Android SDK methods using the `@JavascriptInterface` approach described in the [Android documentation](https://developer.android.com/guide/webapps/webview.html#BindingJavaScript):</span></span>
   
        import android.content.Context;
        import android.os.Bundle;
        import android.util.Log;
        import android.webkit.JavascriptInterface;
   
        import com.microsoft.azure.engagement.EngagementAgent;
   
        import org.json.JSONArray;
        import org.json.JSONObject;
   
        public class WebAppInterface {
            Context mContext;
   
            /** Instantiate the interface and set the context */
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
4. <span data-ttu-id="6c957-115">Jakmile jsme vytvořili výše uvedeného souboru most, potřebujeme zajistit, že je spojen s naše webové zobrazení.</span><span class="sxs-lookup"><span data-stu-id="6c957-115">Once we have created the above bridge file, we need to ensure that it is associated with our Webview.</span></span> <span data-ttu-id="6c957-116">K tomu dojít, budete muset upravit vaše `SetWebview` metoda, takže to vypadá takto:</span><span class="sxs-lookup"><span data-stu-id="6c957-116">For this to happen, you need to edit your `SetWebview` method so that it looks like the following:</span></span>
   
        private void SetWebView() {
            WebView myWebView = (WebView) findViewById(R.id.webview);
            myWebView.loadUrl("file:///android_asset/Sample.html");
            WebSettings webSettings = myWebView.getSettings();
            webSettings.setJavaScriptEnabled(true);
            myWebView.addJavascriptInterface(new WebAppInterface(this), "EngagementJs");
        }
5. <span data-ttu-id="6c957-117">Ve výše uvedeném fragmentu volali jsme `addJavascriptInterface` Naše třída most přidružit naše webové zobrazení a také vytvořit popisovač názvem **EngagementJs** volat metody ze souboru most.</span><span class="sxs-lookup"><span data-stu-id="6c957-117">In the above snippet, we called `addJavascriptInterface` to associate our bridge class with our Webview and also created a handle called **EngagementJs** to call the methods from the bridge file.</span></span> 
6. <span data-ttu-id="6c957-118">Nyní vytvoří následující soubor s názvem **Sample.html** ve projekt ve složce s názvem **prostředky** který je načten do webové zobrazení a kde metody bude volat ze souboru most.</span><span class="sxs-lookup"><span data-stu-id="6c957-118">Now create the following file called **Sample.html** in your project in a folder called **assets** which is loaded into the Webview and where we will call the methods from the bridge file.</span></span>
   
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
                            // Example of how extras info can be passed with the Engagement logs
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
7. <span data-ttu-id="6c957-119">Vezměte na vědomí následující body týkající se souboru HTML výše:</span><span class="sxs-lookup"><span data-stu-id="6c957-119">Note the following points about the HTML file above:</span></span>
   
   * <span data-ttu-id="6c957-120">Obsahuje sadu vstupních polí, kde můžete zadat data, která má být použit jako názvy pro události, úlohy, chyba, AppInfo.</span><span class="sxs-lookup"><span data-stu-id="6c957-120">It contains a set of input boxes where you can provide data to be used as names for your Event, Job, Error, AppInfo.</span></span> <span data-ttu-id="6c957-121">Když kliknete na tlačítko vedle sebe, Přišla žádost o jazyka JavaScript, který nakonec volá metody ze souboru most předat volání Mobile Engagement Android SDK.</span><span class="sxs-lookup"><span data-stu-id="6c957-121">When you click on the button next to it, a call is made to the Javascript which eventually calls the methods from the bridge file to pass this call to the Mobile Engagement Android SDK.</span></span> 
   * <span data-ttu-id="6c957-122">Jsme jsou označování na některé statické doplňující informace o události, úlohy a k předvedení toho, jak to lze provést i chyby.</span><span class="sxs-lookup"><span data-stu-id="6c957-122">We are tagging on some static extra info to the events, jobs and even errors to demonstrate how this could be done.</span></span> <span data-ttu-id="6c957-123">Tyto doplňující informace o je odeslán jako řetězec JSON, který je-li hledat v `WebAppInterface` souboru, analyzovat a umístí Android `Bundle` a je předána společně s odesílání událostí, úloh, chyb.</span><span class="sxs-lookup"><span data-stu-id="6c957-123">This extra info is sent as a JSON string which, if you look in the `WebAppInterface` file, is parsed and put in an Android `Bundle` and is passed along with sending Events, Jobs, Errors.</span></span> 
   * <span data-ttu-id="6c957-124">Mobile Engagement úloha je spuštěna s názvem spustit 10 sekund a zadáte v dialogovém okně vstupní vypnout.</span><span class="sxs-lookup"><span data-stu-id="6c957-124">A Mobile Engagement Job is kicked off with the name you specify in the input box, run for 10 seconds and shut down.</span></span> 
   * <span data-ttu-id="6c957-125">Mobile Engagement appinfo nebo značka, předá s 'jméno_zákazníka' jako statické klíč a hodnotu, kterou jste zadali v vstup jako hodnotu pro značku.</span><span class="sxs-lookup"><span data-stu-id="6c957-125">A Mobile Engagement appinfo or tag is passed with 'customer_name' as the static key and the value that you entered in the input as the value for the tag.</span></span> 
8. <span data-ttu-id="6c957-126">Spusťte aplikaci a zobrazí se následující.</span><span class="sxs-lookup"><span data-stu-id="6c957-126">Run the app and you will see the following.</span></span> <span data-ttu-id="6c957-127">Nyní zadejte nějaký název pro událost testování takto a klikněte na **odeslat** pod ním.</span><span class="sxs-lookup"><span data-stu-id="6c957-127">Now provide some name for a test event like the following and click **Send** below it.</span></span> 
   
    ![][1]
9. <span data-ttu-id="6c957-128">Nyní, pokud přejdete do **monitorování** karta aplikace a naleznete v části **události -> Podrobnosti o**, zobrazí se tato událost objeví spolu s statické aplikace informace, které jsme odesílání.</span><span class="sxs-lookup"><span data-stu-id="6c957-128">Now if you go to the **Monitor** tab of your app and look under **Events -> Details**, you will see this event show up along with the static app-info that we are sending.</span></span> 
   
   ![][2]

<!-- Images. -->
[1]: ./media/mobile-engagement-bridge-webview-native-android/sending-event.png
[2]: ./media/mobile-engagement-bridge-webview-native-android/event-output.png
