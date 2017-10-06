---
title: "aaaGet začít s Androidem aplikace Azure Mobile Engagement"
description: "Zjistěte, jak toouse Azure Mobile Engagement s analytickými funkcemi a nabízenými oznámeními pro aplikace pro Android."
services: mobile-engagement
documentationcenter: android
author: piyushjo
manager: erikre
editor: 
ms.assetid: 3c286c6d-cfef-4e3e-9b2c-715429fe82db
ms.service: mobile-engagement
ms.workload: mobile
ms.tgt_pltfrm: mobile-android
ms.devlang: Java
ms.topic: hero-article
ms.date: 08/10/2016
ms.author: piyushjo;ricksal
ms.openlocfilehash: e8c92607691104750cdf1c4f7639a041d8a7bcd5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-azure-mobile-engagement-for-android-apps"></a>Začínáme s Azure Mobile Engagementem pro aplikace pro Android
[!INCLUDE [Hero tutorial switcher](../../includes/mobile-engagement-hero-tutorial-switcher.md)]

Toto téma ukazuje, jak Azure Mobile Engagement toounderstand toouse používání aplikace a jak toosend nabízená oznámení uživatelům toosegmented Android aplikace.
Tento kurz představuje scénář hello jednoduchého vysílání přes Mobile Engagement. V něm můžete vytvořit prázdnou aplikaci pro Android, která sbírá základní data a dostává nabízená oznámení pomocí služby GCM (Google Cloud Messaging).

## <a name="prerequisites"></a>Požadavky
Dokončení tohoto kurzu vyžaduje hello [Android Developer Tools](https://developer.android.com/sdk/index.html), které zahrnují integrované vývojové prostředí Android Studio hello a nejnovější platformu Android hello.

Také budete potřebovat hello [Mobile Engagement Android SDK](https://aka.ms/vq9mfn).

> [!IMPORTANT]
> toocomplete tohoto kurzu potřebujete aktivní účet Azure. Pokud účet nemáte, můžete si během několika minut vytvořit bezplatný zkušební účet. Podrobnosti najdete v článku [Bezplatná zkušební verze Azure](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A0E0E5C02&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdocumentation%2Farticles%2Fmobile-engagement-android-get-started).
>
>

## <a name="set-up-mobile-engagement-for-your-android-app"></a>Nastavení Mobile Engagementu pro vaši aplikaci pro Android
[!INCLUDE [Create Mobile Engagement App in Portal](../../includes/mobile-engagement-create-app-in-portal-new.md)]

## <a name="connect-your-app-toohello-mobile-engagement-backend"></a>Připojit vaše aplikace toohello Mobile Engagement back-end
Tento kurzu si představíme "základní integraci", což je minimální hello nastavte požadované toocollect dat a odesílání nabízených oznámení. Vytvoříte základní aplikaci s integrací hello toodemonstrate Android Studio.

Hello kompletní dokumentaci k integraci najdete v hello [integrace sady Mobile Engagement Android SDK](mobile-engagement-android-sdk-overview.md).

### <a name="create-an-android-project"></a>Vytvoření projektu Android
1. Spustit **Android Studio**a v místní nabídce hello vyberte **spuštění nového projektu Android Studio**.

    ![][1]
2. Zadejte název aplikace a doménu firmy. Poznamenejte si, co vyplňujete, protože budete tyto údaje potřebovat později. Klikněte na **Další**.

    ![][2]
3. Vyberte cíl formuláře hello a úroveň API a klikněte na **Další**.

   > [!NOTE]
   > Mobile Engagement vyžaduje API minimálně úrovně 10 (Android 2.3.3).
   >
   >

    ![][3]
4. Vyberte **prázdné aktivity** tady, což je hello jedinou obrazovkou pro tuto aplikaci a klikněte na tlačítko **Další**.

    ![][4]
5. Závěr ponechte hello výchozí nastavení a klikněte na tlačítko **Dokončit**.

    ![][5]

Android Studio nyní vytvoří hello ukázkovou aplikaci, do které budeme integrovat Mobile Engagement.

### <a name="include-hello-sdk-library-in-your-project"></a>Zahrnout hello knihovně SDK do projektu
1. Stáhnout hello [Mobile Engagement Android SDK](https://aka.ms/vq9mfn).
2. Extrahujte hello archivu souboru tooa složky v počítači.
3. Identifikujte knihovnu .jar hello pro hello aktuální verzi této sady SDK a zkopírujte ho toohello schránky.

      ![][6]
4. Přejděte toohello **projektu** část (1) a vložte hello .jar hello složky libs (2).

      ![][7]
5. tooload hello knihovně, synchronizace hello projektu.

      ![][8]

### <a name="connect-your-app-toomobile-engagement-backend-with-hello-connection-string"></a>Připojit vaše tooMobile Engagement back-end aplikace s hello připojovací řetězec
1. Zkopírujte následující řádky kódu do vytváření aktivity hello (je třeba provést pouze na jednom místě aplikace, obvykle hello hlavní aktivitě) hello. Této ukázkové aplikace, otevřete si hello MainActivity pod src -> main -> java složky a přidat následující hello:

        EngagementConfiguration engagementConfiguration = new EngagementConfiguration();
        engagementConfiguration.setConnectionString("Endpoint={appCollection}.{domain};AppId={appId};SdkKey={sdkKey}");
        EngagementAgent.getInstance(this).init(engagementConfiguration);
2. Vyřešte odkazy na hello stisknutím Alt + Enter nebo přidáním hello následující příkazy pro import:

        import com.microsoft.azure.engagement.EngagementAgent;
        import com.microsoft.azure.engagement.EngagementConfiguration;
3. Přejděte zpět toohello portálu Azure Classic ve vaší aplikaci **informace o připojení** stránky a zkopírujte hello **připojovací řetězec**.

      ![][9]
4. Vložte jej do hello `setConnectionString` parametr, nahraďte hello celý řetězec ukazuje hello následující kód:

        engagementConfiguration.setConnectionString("Endpoint=my-company-name.device.mobileengagement.windows.net;SdkKey=********************;AppId=*********");

### <a name="add-permissions-and-a-service-declaration"></a>Přidání oprávnění a deklarace služby
1. Přidat tyto toohello oprávnění souboru Manifest.xml vašeho projektu těsně před nebo po hello `<application>` značky:

        <uses-permission android:name="android.permission.INTERNET"/>
        <uses-permission android:name="android.permission.ACCESS_NETWORK_STATE"/>
        <uses-permission android:name="android.permission.WRITE_EXTERNAL_STORAGE"/>
        <uses-permission android:name="android.permission.RECEIVE_BOOT_COMPLETED" />
        <uses-permission android:name="android.permission.VIBRATE" />
        <uses-permission android:name="android.permission.DOWNLOAD_WITHOUT_NOTIFICATION"/>
2. toodeclare hello Služba agenta, přidejte tento kód mezi hello `<application>` a `</application>` značky:

        <service
             android:name="com.microsoft.azure.engagement.service.EngagementService"
             android:exported="false"
             android:label="<Your application name>"
             android:process=":Engagement"/>
3. V kódu hello jste vložili, nahraďte `"<Your application name>"` v hello popisek, který se zobrazí v hello **nastavení** nabídky, kde uvidíte služby spuštěné na zařízení hello. Do tohoto popisku můžete přidat hello slovo "Service" třeba.

### <a name="send-a-screen-toomobile-engagement"></a>Odeslání obrazovky tooMobile zapojení
odeslání dat toostart a zkontrolujte, zda text hello uživatelé jsou aktivní, musíte odeslat alespoň jednu obrazovku (aktivitu) toohello Mobile Engagement back-end.

Přejděte příliš**MainActivity.java** a přidejte následující základní třídu hello tooreplace hello **MainActivity** příliš**EngagementActivity**:

    public class MainActivity extends EngagementActivity {

> [!NOTE]
> Pokud vaše základní třída není *aktivity*, poraďte se [Advanced hlášení pro Android](mobile-engagement-android-advanced-reporting.md) jak tooinherit z různých tříd.
>
>

Komentář hello následující řádek tohoto jednoduchého ukázkového scénáře:

    // setSupportActionBar(toolbar);

Pokud chcete, aby tookeep hello `ActionBar` ve vaší aplikaci, najdete v části [Advanced hlášení pro Android](mobile-engagement-android-advanced-reporting.md).

## <a name="connect-app-with-real-time-monitoring"></a>Připojení aplikace se sledováním v reálném čase
[!INCLUDE [Connect app with real-time monitoring](../../includes/mobile-engagement-connect-app-with-monitor.md)]

## <a name="enable-push-notifications-and-in-app-messaging"></a>Povolení nabízených oznámení a zasílání zpráv v aplikaci
Během kampaně vám Mobile Engagement umožňuje interagovat a KOMUNIKOVAT s uživateli pomocí nabízených oznámení a zpráv v aplikaci. Tento modul je hello portálu Mobile Engagement nazývá REACH.
Následující části Hello nastaví tooreceive vaše aplikace je.

### <a name="copy-sdk-resources-in-your-project"></a>Kopírování prostředků sady SDK do projektu
1. Přejděte zpět tooyour SDK stáhnout obsah a zkopírujte hello **res** složky.

    ![][10]
2. Přejděte zpět tooAndroid Studio, vyberte hello **hlavní** adresář souborů projektu a pak ji vložit tooadd hello prostředky tooyour projektu.

    ![][11]

[!INCLUDE [Enable Google Cloud Messaging](../../includes/mobile-engagement-enable-google-cloud-messaging.md)]

[!INCLUDE [Enable in-app messaging](../../includes/mobile-engagement-android-send-push.md)]

[!INCLUDE [Send notification from portal](../../includes/mobile-engagement-android-send-push-from-portal.md)]

## <a name="next-steps"></a>Další kroky
Přejděte příliš[sady SDK pro Android](mobile-engagement-android-sdk-overview.md) tooget podrobné informace o hello integraci sady SDK.

<!-- Images. -->
[1]: ./media/mobile-engagement-android-get-started/android-studio-new-project.png
[2]: ./media/mobile-engagement-android-get-started/android-studio-project-props.png
[3]: ./media/mobile-engagement-android-get-started/android-studio-project-props2.png
[4]: ./media/mobile-engagement-android-get-started/android-studio-add-activity.png
[5]: ./media/mobile-engagement-android-get-started/android-studio-activity-name.png
[6]: ./media/mobile-engagement-android-get-started/sdk-content.png
[7]: ./media/mobile-engagement-android-get-started/paste-jar.png
[8]: ./media/mobile-engagement-android-get-started/sync-project.png
[9]: ./media/mobile-engagement-android-get-started/app-connection-info-page.png
[10]: ./media/mobile-engagement-android-get-started/copy-resources.png
[11]: ./media/mobile-engagement-android-get-started/paste-resources.png
