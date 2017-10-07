---
title: "aaaGet Začínáme s Azure Mobile Engagementem pro Cordovu/Phonegap"
description: "Zjistěte, jak toouse Azure Mobile Engagement s analytickými funkcemi a nabízenými oznámeními pro aplikace Cordova/Phonegap."
services: mobile-engagement
documentationcenter: Mobile
author: piyushjo
manager: erikre
editor: 
ms.assetid: 54fe9113-e239-4ed7-9fd1-a502d7ac7f47
ms.service: mobile-engagement
ms.workload: mobile
ms.tgt_pltfrm: mobile-phonegap
ms.devlang: js
ms.topic: hero-article
ms.date: 08/19/2016
ms.author: piyushjo
ms.openlocfilehash: e67dabbdf7886802bb058f38964e558d5ae6854c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-azure-mobile-engagement-for-cordovaphonegap"></a>Začínáme s Azure Mobile Engagementem pro Cordovu/Phonegap
[!INCLUDE [Hero tutorial switcher](../../includes/mobile-engagement-hero-tutorial-switcher.md)]

Toto téma ukazuje, jak toounderstand Azure Mobile Engagement toouse aplikace využití a odesílání nabízených oznámení toosegmented uživatele pro mobilní aplikace vyvinuté pomocí Cordovy.

V tomto kurzu si pomocí Macu vytvoříme prázdnou aplikaci Cordova a pak do ní integrujeme sadu SDK Mobile Engagement. Ta shromažďuje základní analytická data a přijímá nabízená oznámení přes APNS ( Apple Push Notification System) pro iOS nebo GCM (Google Cloud Messaging) pro Android. Nasadíme tuto tooan zařízení iOS nebo Android pro testování. 

> [!NOTE]
> toocomplete tento kurz, musíte mít aktivní účet Azure. Pokud účet nemáte, můžete si během několika minut vytvořit bezplatný zkušební účet. Podrobnosti najdete v článku [Bezplatná zkušební verze Azure](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A0E0E5C02&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdocumentation%2Farticles%2Fmobile-engagement-cordova-get-started).
> 
> 

Tento kurz vyžaduje hello následující:

* XCode, který můžete nainstalovat z Mac App Storu (pro nasazení tooiOS)
* [Android SDK a emulátor](http://developer.android.com/sdk/installing/index.html) (pro nasazení tooAndroid)
* Certifikát nabízených oznámení (.p12), který můžete získat ve vývojářském centru Apple, pro APNS
* Číslo GCM projektu, které můžete získat z Vývojářské konzole Google, pro GCM
* [Plugin Mobile Engagement pro Cordovu](https://www.npmjs.com/package/cordova-plugin-ms-azure-mobile-engagement)

> [!NOTE]
> Můžete najít hello zdrojového kódu a hello – soubor ReadMe pro cordovu hello na [Githubu](https://github.com/Azure/azure-mobile-engagement-cordova)
> 
> 

## <a id="setup-azme"></a>Nastavení Mobile Engagementu pro aplikaci Cordova
[!INCLUDE [Create Mobile Engagement App in Portal](../../includes/mobile-engagement-create-app-in-portal-new.md)]

## <a id="connecting-app"></a>Připojení vaší aplikace toohello Mobile Engagement back-end
Tento kurzu si představíme "základní integraci", což je minimální hello nastavte požadované toocollect dat a odesílání nabízených oznámení. 

Pomocí integrace hello toodemonstrate Cordova vytvoříme základní aplikaci:

### <a name="create-a-new-cordova-project"></a>Vytvoření nového projektu Cordova
1. Spusťte *Terminálové* okno na vaše Mac počítače a typ hello následující, která se vytvoří nový projekt Cordova z hello výchozí šablony. Ujistěte se, že publikování hello profilu můžete nakonec použití toodeploy vaše aplikace iOS používá "com.mycompany.myapp" jako hello ID aplikace. 
   
        $ cordova create azme-cordova com.mycompany.myapp
        $ cd azme-cordova
2. Spuštění projektu pro hello následující tooconfigure **iOS** a spustíte ho v simulátoru iOS hello:
   
        $ cordova platform add ios 
        $ cordova run ios
3. Spuštění projektu pro hello následující tooconfigure **Android** a spustíte ho v emulátoru Android hello. Ujistěte se, že nastavení emulátoru sady SDK pro Android nastaven cíl jako rozhraní Google API (Google Inc.) s hello CPU / ABI jako Google APIs ARM.  
   
        $ cordova platform add android
        $ cordova run android
4. Přidejte plugin Cordova Console hello. 

    ```
    $ cordova plugin add cordova-plugin-console
    ``` 

### <a name="connect-your-app-toomobile-engagement-backend"></a>Připojit vaše tooMobile Engagement back-end aplikace
1. Nainstalujte plugin Azure Mobile Engagement pro Cordovu hello při současném poskytování hello modulu plug-in hello tooconfigure hodnoty proměnné:
   
        cordova plugin add cordova-plugin-ms-azure-mobile-engagement    
             --variable AZME_IOS_CONNECTION_STRING=<iOS Connection String> 
            --variable AZME_IOS_REACH_ICON=... (icon name WITH extension) 
            --variable AZME_ANDROID_CONNECTION_STRING=<Android Connection String> 
            --variable AZME_ANDROID_REACH_ICON=... (icon name WITHOUT extension)       
            --variable AZME_ANDROID_GOOGLE_PROJECT_NUMBER=... (From your Google Cloud console for sending push notifications) 
            --variable AZME_ACTION_URL =... (URL scheme which triggers hello app for deep linking)
            --variable AZME_ENABLE_NATIVE_LOG=true|false
            --variable AZME_ENABLE_PLUGIN_LOG=true|false

*Ikona Reach pro Android* : musí být název hello hello prostředku bez žádné přípony ani předpony (např:. mynotificationicon), a hello soubor ikony musí být zkopírován do vašeho projektu android (platformy/android/res/drawable)

*Ikona Reach pro iOS* : musí být název hello hello prostředku včetně přípony (např: mojeikonanotifikace.PNG), a hello soubor ikony musí být přidán do projektu iOS s XCode (přes hello nabídky přidat soubory)

## <a id="monitor"></a>Povolení sledování v reálném čase
1. V projektu Cordova hello - upravit **www/js/index.js** volání hello tooadd tooMobile Engagement toodeclare nová aktivita jednou hello *deviceReady* události.
   
         onDeviceReady: function() {
                Engagement.startActivity("myPage",{});
            }
2. Spuštění aplikace hello:
   
   * **Pro iOS**
     
       V `Terminal` okno spusťte aplikaci v nové instanci simulátoru spuštěním hello následující:
     
           cordova run ios
   * **Pro Android**
     
       V `Terminal` okno spusťte aplikaci v nové instanci emulátoru spuštěním hello následující:
     
           cordova run android
3. Zobrazí se následující hello v protokolech konzoly hello:
   
        [Engagement] Agent: Session started
        [Engagement] Agent: Activity 'myPage' started
        [Engagement] Connection: Established
        [Engagement] Connection: Sent: appInfo
        [Engagement] Connection: Sent: startSession
        [Engagement] Connection: Sent: activity name='myPage'

## <a id="monitor"></a>Připojení aplikace se sledováním v reálném čase
[!INCLUDE [Connect app with real-time monitoring](../../includes/mobile-engagement-connect-app-with-monitor.md)]

## <a id="integrate-push"></a>Povolení nabízených oznámení a zasílání zpráv v aplikaci
Mobile Engagement vám umožní toointeract s uživateli přes nabízená oznámení a zasílání zpráv v kontextu hello kampaní v aplikaci. Tento modul je hello portálu Mobile Engagement nazývá REACH.
Hello následujících sekcích nastavíte tooreceive vaše aplikace je.

### <a name="configure-push-credentials-for-mobile-engagement"></a>Konfigurace přihlašovacích údajů nabízených oznámení pro Mobile Engagement
tooallow Mobile Engagement toosend nabízená oznámení vaším jménem, je nutné toogrant, že přístup tooyour certifikátu IOS(Apple) nebo klíči rozhraní API serveru GCM. 

1. Přejděte tooyour portál Mobile Engagement. Zkontrolujte, zda pracujete v aplikaci hello používáte pro tento projekt a potom klikněte na hello **Engage** tlačítko dole v hello:
   
    ![][1]
2. Přejdete na stránku hello nastavení na portálu Engagement. Zde klikněte na hello **nativní oznámení** části:
   
    ![][2]
3. Konfigurace certifikátu iOS / klíče rozhraní API serveru GCM
   
    **[iOS]**
   
    a. Vyberte svůj soubor .p12, nahrajte ho a zadejte heslo:
   
    ![][3]
   
    **[Android]**
   
    a. Klikněte na ikonu pro úpravu hello před **klíč rozhraní API** v části Nastavení GCM hello v automaticky otevřeném okně hello, který se zobrazí, vložte hello klíč serveru GCM a klikněte na tlačítko **OK**. 
   
    ![][4]

### <a name="enable-push-notifications-in-hello-cordova-app"></a>Povolení nabízených oznámení v aplikaci Cordova hello
Upravit **www/js/index.js** tooadd hello volání tooMobile Engagement toorequest nabízená oznámení a deklaruje obslužnou rutinu:

     onDeviceReady: function() {
           Engagement.initializeReach(  
                 // on OpenUrl  
                 function(_url) {   
                 alert(_url);   
                 });  
            Engagement.startActivity("myPage",{});  
        }

### <a name="run-hello-app"></a>Spuštění aplikace hello
**[iOS]**

1. Jsme použije XCode toobuild a nasazení aplikace hello na hello zařízení tootest nabízená oznámení, protože iOS povoluje nabízená oznámení tooan skutečné zařízení. Přejděte toohello umístění, kde se má vytvořit projekt Cordova a přejděte příliš**...\platforms\ios** umístění. Otevřete soubor nativní .xcodeproj hello v XCode. 
2. Vytváření a nasazování hello Cordova aplikace toohello zařízení s iOS pomocí hello účet, který má hello obsahující hello certifikát, že jste právě nahráli portál Mobile Engagement toohello a hello Id aplikace, který by odpovídal hello jeden, který jste zadali při vytváření profilu pro zřizování aplikace Cordova Hello. Je možné rezervovat hello *identifikátor svazku* v vaše **prostředky\*-info.plist** souboru v XCode toomatch ho nahoru. 
3. Uvidíte, že hello standardní místní nabídka iOS na zařízení s informacemi o tom, že aplikace hello požadavky oprávnění toosend oznámení. Udělení oprávnění hello. 

**[Android]**

Jednoduše můžete hello emulátoru toorun hello Android aplikaci jako oznámení GCM jsou podporovány v emulátoru Android hello. 

    cordova run android

## <a id="send"></a>Poslat tooyour aplikaci oznámení
Teď vytvoříme jednoduchou kampaň nabízených oznámení, který bude odesílat nabízených tooyour aplikaci spuštěnou na zařízení hello:

1. Přejděte toohello **dosáhnout** na portálu Mobile Engagement
2. Klikněte na tlačítko **nové oznámení** toocreate kampaň nabízených
   
    ![][6]
3. Zadejte vstupy toocreate kampaň **[Android]**
   
   * Zadejte **Název** kampaně. 
   * Vyberte hello **typ doručení** jako *systémové oznámení* *jednoduché*
   * Vyberte hello **čas doručení** jako *"Kdykoliv"*
   * Zadejte **název** oznámení, který bude hello první řádek v nabízené hello.
   * Zadejte **zpráva** oznámení, který bude sloužit jako text zprávy hello. 
     
     ![][11]
4. Zadejte vstupy toocreate kampaň **[iOS]**
   
   * Zadejte **Název** kampaně. 
   * Vyberte hello **čas doručení** jako *"pouze mimo aplikaci"*
   * Zadejte **název** oznámení, který bude hello první řádek v nabízené hello.
   * Zadejte **zpráva** oznámení, který bude sloužit jako text zprávy hello. 
     
     ![][12]
5. Posuňte se dolů a v hello části obsahu vyberte **pouze oznámení**
   
    ![][8]
6. [Nepovinné] Můžete také zadat adresu URL akce. Ujistěte se, že používá schéma URL zadat při konfiguraci hello plugin **AZME\_PŘESMĚROVÁNÍ\_URL** proměnná například *myapp://test*.  
7. Dokončení nastavení hello nejzákladnější kampaně možné. Nyní znovu přejděte dolů a klikněte na tlačítko hello **vytvořit** tlačítko toosave kampaně.
8. Nakonec svou kampaň můžete **Aktivovat**
   
    ![][10]
9. Nyní byste měli na svém zařízení nebo emulátoru vidět nabízené oznámení z této kampaně. 

## <a id="next-steps"></a>Další kroky
[Přehled všech metod, které jsou k dispozici pro sadu Cordova Mobile Engagement SDK](https://github.com/Azure/azure-mobile-engagement-cordova)

<!-- Images. -->

[1]: ./media/mobile-engagement-cordova-get-started/engage-button.png
[2]: ./media/mobile-engagement-cordova-get-started/engagement-portal.png
[3]: ./media/mobile-engagement-cordova-get-started/native-push-settings.png
[4]: ./media/mobile-engagement-cordova-get-started/api-key.png
[6]: ./media/mobile-engagement-cordova-get-started/new-announcement.png
[8]: ./media/mobile-engagement-cordova-get-started/campaign-content.png
[10]: ./media/mobile-engagement-cordova-get-started/campaign-activate.png
[11]: ./media/mobile-engagement-cordova-get-started/campaign-first-params-android.png
[12]: ./media/mobile-engagement-cordova-get-started/campaign-first-params-ios.png

