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
# <a name="bridge-android-webview-with-native-mobile-engagement-android-sdk"></a>Android webové zobrazení most s nativní Mobile Engagement Android SDK
> [!div class="op_single_selector"]
> * [Most Android](mobile-engagement-bridge-webview-native-android.md)
> * [Most iOS](mobile-engagement-bridge-webview-native-ios.md)
> 
> 

Některé mobilní aplikace slouží jako hybridní aplikace, kde je hello aplikace vyvinuté pomocí nativní vývoj pro Android, ale některé nebo všechny hello obrazovky jsou vykreslovány v rámci Android webové zobrazení. Mobile Engagement Android SDK můžete i nadále využívat v rámci těchto aplikací a tento kurz popisuje, jak toogo o to. Ukázkový kód Hello níže podle hello Android dokumentaci [zde](https://developer.android.com/guide/webapps/webview.html#BindingJavaScript). Popisuje, jak by bylo možné tuto metodu zdokumentovaných tooimplement hello stejné pro Mobile Engagement Android SDK pro běžně používané metody tak, aby webové zobrazení z hybridní aplikace lze také zahájit události tootrack požadavků, úloh, chyb, app-info při jejich prostřednictvím potrubí Naše sady SDK pro Android. 

1. Je třeba nejprve všech, tooensure, který jste prošli naše [kurzu Začínáme](mobile-engagement-android-get-started.md) toointegrate hello Mobile Engagement Android SDK v hybridní aplikace. Jakmile to uděláte, vaše `OnCreate` metoda bude vypadat podobně jako následující hello.  
   
        @Override
        protected void onCreate(Bundle savedInstanceState) {
            super.onCreate(savedInstanceState);
            setContentView(R.layout.activity_main);
   
            EngagementConfiguration engagementConfiguration = new EngagementConfiguration();
            engagementConfiguration.setConnectionString("<Mobile Engagement Conn String>");
            EngagementAgent.getInstance(this).init(engagementConfiguration);
        }
2. Nyní se ujistěte, že hybridní aplikace má na obrazovce s webové zobrazení. Hello kód pro něj bude podobné následující toohello kde jsme jsou načítání místní soubor HTML **Sample.html** v hello webové zobrazení v hello `onCreate` metoda obrazovky. 
   
        private void SetWebView() {
            WebView myWebView = (WebView) findViewById(R.id.webview);
            myWebView.loadUrl("file:///android_asset/Sample.html");
        }
   
        protected void onCreate(Bundle savedInstanceState) {
            ...
            ...
            SetWebView();
        }
3. Teď vytvořte most soubor s názvem **WebAppInterface** běžně vytváří obálku přes některé použít metody Mobile Engagement Android SDK pomocí hello `@JavascriptInterface` postupuje podle hello [Android dokumentace ](https://developer.android.com/guide/webapps/webview.html#BindingJavaScript):
   
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
4. Jakmile jsme vytvořili hello výše most souboru, potřebujeme tooensure, že je spojen s naše webové zobrazení. Pro tento toohappen, je třeba tooedit vaše `SetWebview` metoda, takže to vypadá hello následující:
   
        private void SetWebView() {
            WebView myWebView = (WebView) findViewById(R.id.webview);
            myWebView.loadUrl("file:///android_asset/Sample.html");
            WebSettings webSettings = myWebView.getSettings();
            webSettings.setJavaScriptEnabled(true);
            myWebView.addJavascriptInterface(new WebAppInterface(this), "EngagementJs");
        }
5. V hello výše fragment kódu, volali jsme `addJavascriptInterface` tooassociate naše most s naše webové zobrazení třídy a také vytvořit popisovač názvem **EngagementJs** toocall hello metody ze souboru most hello. 
6. Nyní vytvoří hello následující soubor s názvem **Sample.html** ve projekt ve složce s názvem **prostředky** který je načten do hello webové zobrazení a kde hello metody bude volat ze souboru most hello.
   
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
7. Poznámka: hello následující body o soubor HTML hello výše:
   
   * Obsahuje sadu vstupních polí, kde je možné poskytnout toobe dat použít jako názvy pro události, úlohy, chyba, AppInfo. Když kliknete na další tooit hello tlačítko, Přišla žádost o toohello Javascript, který nakonec volání metody hello ze hello most souboru toopass toohello toto volání Mobile Engagement Android SDK. 
   * Jsme jsou označování na některé události toohello statické doplňující informace, úlohy a i chyby toodemonstrate jak to lze provést. Tyto další informace o je odeslán jako řetězec JSON, který je-li hledat v hello `WebAppInterface` souboru, analyzovat a umístí Android `Bundle` a je předána společně s odesílání událostí, úloh, chyb. 
   * Mobile Engagement úloha je spuštěna s názvem hello zadáte hello vstupní pole, spusťte pro 10 sekund a vypnout. 
   * Mobile Engagement appinfo nebo značka, se předá s 'jméno_zákazníka' jako hello statické klíč a hodnotu hello, kterou jste zadali v hello vstup jako hodnotu hello hello značky. 
8. Spuštění hello aplikace a můžete se zobrazí následující hello. Nyní zadejte nějaký název pro událost testování jako hello následující a klikněte na **odeslat** pod ním. 
   
    ![][1]
9. Nyní, pokud přejdete toohello **monitorování** karta aplikace a naleznete v části **události -> Podrobnosti o**, zobrazí se tato událost objeví spolu s hello statické aplikace informací, které jsme odesílání. 
   
   ![][2]

<!-- Images. -->
[1]: ./media/mobile-engagement-bridge-webview-native-android/sending-event.png
[2]: ./media/mobile-engagement-bridge-webview-native-android/event-output.png
