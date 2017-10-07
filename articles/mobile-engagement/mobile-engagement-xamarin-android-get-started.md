---
title: "aaaGet Začínáme s Azure Mobile Engagementem pro Xamarin.Android"
description: "Zjistěte, jak toouse Azure Mobile Engagement s analytickými funkcemi a nabízenými oznámeními pro aplikace pro Xamarin.Android."
services: mobile-engagement
documentationcenter: xamarin
author: piyushjo
manager: erikre
editor: 
ms.assetid: fb68cf98-08a2-41b5-8e59-757469de3fe7
ms.service: mobile-engagement
ms.workload: mobile
ms.tgt_pltfrm: mobile-xamarin-android
ms.devlang: dotnet
ms.topic: hero-article
ms.date: 06/16/2016
ms.author: piyushjo
ms.openlocfilehash: 9d584fea8e8153d511258cf9b6f87f31dac6aeca
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-azure-mobile-engagement-for-xamarinandroid-apps"></a>Začínáme s Azure Mobile Engagementem pro aplikace pro Xamarin.Android
[!INCLUDE [Hero tutorial switcher](../../includes/mobile-engagement-hero-tutorial-switcher.md)]

Toto téma ukazuje, jak Azure Mobile Engagement toounderstand toouse používání aplikace a jak toosend nabízená oznámení uživatelům toosegmented aplikace pro Xamarin.Android.
Tento kurz představuje scénář hello jednoduchého vysílání přes Mobile Engagement. V něm můžete vytvořit prázdnou aplikaci pro Xamarin.Android, která sbírá základní data a dostává nabízená oznámení pomocí služby GCM (Google Cloud Messaging).

> [!NOTE]
> Služba Azure Mobile Engagement Hello vyřadí března 2018 a je aktuálně pouze k dispozici tooexisting zákazníků. Další informace najdete v tématu [Mobile Engagement](https://azure.microsoft.com/en-us/services/mobile-engagement/).

Tento kurz vyžaduje hello následující:

* [Xamarin Studio](http://xamarin.com/studio). Můžete také použít Visual Studio s Xamarinem, ale v tomto kurzu používáme Xamarin Studio. Instalační pokyny najdete v tématu o [nastavení a instalaci pro Visual Studio a Xamarin](https://msdn.microsoft.com/library/mt613162.aspx).
* [Mobile Engagement Xamarin SDK](https://www.nuget.org/packages/Microsoft.Azure.Engagement.Xamarin/)

> [!NOTE]
> toocomplete tento kurz, musíte mít aktivní účet Azure. Pokud účet nemáte, můžete si během několika minut vytvořit bezplatný zkušební účet. Podrobnosti najdete v článku [Bezplatná zkušební verze Azure](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A0E0E5C02&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdocumentation%2Farticles%2Fmobile-engagement-xamarin-android-get-started).
> 
> 

## <a id="setup-azme"></a>Nastavení Mobile Engagementu pro vaši aplikaci pro Android
[!INCLUDE [Create Mobile Engagement App in Portal](../../includes/mobile-engagement-create-app-in-portal-new.md)]

## <a id="connecting-app"></a>Připojit vaše aplikace toohello Mobile Engagement back-end
Tento kurzu si představíme "základní integraci", což je minimální hello nastavte požadované toocollect dat a odesílání nabízených oznámení. 

Pomocí Xamarin Studio toodemonstrate hello integrace vytvoříme základní aplikaci.

### <a name="create-a-new-xamarinandroid-project"></a>Vytvoření nového projektu Xamarin.Android
1. Spusťte **Xamarin Studio** přejděte příliš**soubor** -> **nový** -> **řešení** 
   
    ![][1]
2. Vyberte **aplikace pro Android** zkontrolujte hello vybraný jazyk je **C#** a klikněte na tlačítko **Další**.
   
    ![][2]
3. Vyplňte hello **název aplikace** a hello **identifikátor organizace**. Ujistěte se, že toocheckmark **služby Google Play** a pak klikněte na **Další**. 
   
    ![][3]
4. Aktualizace hello **název projektu**, **název řešení** a **umístění** dle potřeby **vytvořit**.
   
    ![][4]

Xamarin Studio vytvoří aplikaci hello, do které budeme integrovat Mobile Engagement. 

### <a name="connect-your-app-toomobile-engagement-backend"></a>Připojit vaše tooMobile Engagement back-end aplikace
1. Klikněte pravým tlačítkem na hello **balíčky** složku v systému windows hello řešení a vyberte **přidat balíčky...**
   
    ![][5]
2. Vyhledejte hello **Microsoft Azure Mobile Engagement Xamarin SDK** a přidejte ji tooyour řešení.  
   
    ![][6]
3. Otevřete **MainActivity.cs** a přidejte hello následující příkazy:
   
        using Microsoft.Azure.Engagement;
        using Microsoft.Azure.Engagement.Activity;
4. V hello `OnCreate` metoda, přidejte následující tooinitialize hello připojení s back-end Mobile Engagementu hello. Ujistěte se, že tooadd vaše **ConnectionString**. 
   
        EngagementConfiguration engagementConfiguration = new EngagementConfiguration();
        engagementConfiguration.ConnectionString = "YourConnectionStringFromAzurePortal";
        EngagementAgent.Init(engagementConfiguration);

### <a name="add-permissions-and-a-service-declaration"></a>Přidání oprávnění a deklarace služby
1. Otevřete hello **Manifest.xml** soubor ve složce vlastnosti hello. Vyberte kartu zdroje, aby bylo přímo aktualizovat zdroj dat XML hello.
2. Přidat tyto toohello oprávnění souboru Manifest.xml (který najdete v části hello **vlastnosti** složky) vašeho projektu těsně před nebo po hello `<application>` značky:
   
        <uses-permission android:name="android.permission.INTERNET"/>
        <uses-permission android:name="android.permission.ACCESS_NETWORK_STATE"/>
        <uses-permission android:name="android.permission.WRITE_EXTERNAL_STORAGE"/>
        <uses-permission android:name="android.permission.RECEIVE_BOOT_COMPLETED" />
        <uses-permission android:name="android.permission.VIBRATE" />
        <uses-permission android:name="android.permission.DOWNLOAD_WITHOUT_NOTIFICATION"/>
3. Přidejte následující hello mezi hello `<application>` a `</application>` značky toodeclare hello agenta služby:
   
        <service
             android:name="com.microsoft.azure.engagement.service.EngagementService"
             android:exported="false"
             android:label="<Your application name>"
             android:process=":Engagement"/>
4. V kódu hello jste vložili, nahraďte `"<Your application name>"` v popisku hello. Zobrazí se v hello **nastavení** nabídky, kde uživatelé vidí služby spuštěné na zařízení hello. Do tohoto popisku můžete přidat hello slovo "Service" třeba.

### <a name="send-a-screen-toomobile-engagement"></a>Odeslání obrazovky tooMobile zapojení
V pořadí toostart odesílat data a zajistit, že hello uživatelé jsou aktivní musíte odeslat alespoň jednu obrazovku toohello Mobile Engagement back-end. To chcete provést – Ujistěte se, že hello `MainActivity` dědí z `EngagementActivity` místo `Activity`.

    public class MainActivity : EngagementActivity

Pokud `EngagementActivity` neumožňuje převzetí, musíte do `OnResume` a `OnPause` přidat metody `.StartActivity` a `.EndActivity`.  

        protected override void OnResume()
            {
                EngagementAgent.StartActivity(EngagementAgentUtils.BuildEngagementActivityName(Java.Lang.Class.FromType(this.GetType())), null);
                base.OnResume();             
            }

            protected override void OnPause()
            {
                EngagementAgent.EndActivity();
                base.OnPause();            
            }

## <a id="monitor"></a>Připojení aplikace se sledováním v reálném čase
[!INCLUDE [Connect app with real-time monitoring](../../includes/mobile-engagement-connect-app-with-monitor.md)]

## <a id="integrate-push"></a>Povolení nabízených oznámení a zasílání zpráv v aplikaci
Mobile Engagement vám umožní toointeract s OSLOVIT uživatele a komunikovat s nabízená oznámení a zasílání zpráv v kontextu hello kampaní v aplikaci. Tento modul je hello portálu Mobile Engagement nazývá REACH.
Následující části Hello nastaví tooreceive vaše aplikace je.

[!INCLUDE [Enable Google Cloud Messaging](../../includes/mobile-engagement-enable-google-cloud-messaging.md)]

[!INCLUDE [Enable in-app messaging](../../includes/mobile-engagement-android-send-push.md)]

[!INCLUDE [Send notification from portal](../../includes/mobile-engagement-android-send-push-from-portal.md)]

<!-- Images -->
[1]: ./media/mobile-engagement-xamarin-android-get-started/1.png
[2]: ./media/mobile-engagement-xamarin-android-get-started/2.png
[3]: ./media/mobile-engagement-xamarin-android-get-started/3.png
[4]: ./media/mobile-engagement-xamarin-android-get-started/4.png
[5]: ./media/mobile-engagement-xamarin-android-get-started/5.png
[6]: ./media/mobile-engagement-xamarin-android-get-started/6.png
