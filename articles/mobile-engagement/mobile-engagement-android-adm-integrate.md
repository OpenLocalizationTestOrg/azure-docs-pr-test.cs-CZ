---
title: aaaAzure integraci sady Android SDK Mobile Engagement
description: "Nejnovější aktualizace a postupy pro Android SDK pro Azure Mobile Engagement"
services: mobile-engagement
documentationcenter: mobile
author: piyushjo
manager: erikre
editor: 
ms.assetid: a7d719ec-67b3-4be3-9d7f-0b61a57fe978
ms.service: mobile-engagement
ms.workload: mobile
ms.tgt_pltfrm: mobile-android
ms.devlang: Java
ms.topic: article
ms.date: 08/19/2016
ms.author: piyushjo
ms.openlocfilehash: c57132ff49cf8c335627a72b37f9b78529e84f48
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="how-toointegrate-adm-with-engagement"></a>Jak tooIntegrate ADM s zapojení
> [!IMPORTANT]
> Postupujte podle hello integrace postup popsaný v hello jak tooIntegrate Engagement v systému Android dokumentu před těchto pokynů.
> 
> Tento dokument je užitečný jenom v případě, že již integrovanou hello Reach modulu a plán toopush Amazon zařízení. kampaně Reach toointegrate ve vaší aplikaci, přečtěte si nejprve jak tooIntegrate Engagement Reach v systému Android.
> 
> 

## <a name="introduction"></a>Úvod
Integrace ADM umožňuje vaší aplikace toobe nabídnutých při cílení na zařízení se systémem Android Amazon.

Datové části ADM nabídnutých toohello SDK vždy obsahovat hello `azme` klíče v hello datový objekt. Proto pokud používáte ADM pro jiný účel ve vaší aplikaci, můžete filtrovat podle klíči nabízených oznámení.

> [!IMPORTANT]
> Pouze Amazon Kindle zařízení se systémem Android 4.0.3 nebo vyšší jsou podporovány Amazon Device Messaging; Můžete však integrovat tento kód bezpečně na jiné zařízení.
> 
> 

## <a name="sign-up-tooadm"></a>Zaregistrujte si tooADM
Pokud dosud neučinili, musíte povolit ADM na vašem účtu Amazon.

Postup Hello je podrobně popsán v: [ <https://developer.amazon.com/sdk/adm/credentials.html>].

Po dokončení postupu hello se zobrazí:

* OAuth přihlašovací údaje (ID klienta a tajný klíč klienta) pro zapojení toobe možné toopush zařízení.
* Klíč rozhraní API, která musí být integrovaná do aplikace.

## <a name="sdk-integration"></a>Integrace sady SDK
### <a name="managing-device-registrations"></a>Správa registrace zařízení
Každé zařízení musí odeslat příkaz toohello registrace ADM servery, v opačném případě nelze získat přístup.

Pokud už používáte hello [klientské knihovny ADM]a už máte [integrované ADM] můžete přímo přejít tooandroid-sdk-adm přijímat.

Pokud nebyly integrované ADM ještě Engagement má jednodušší způsob tooenable ho v aplikaci:

Upravit vaše `AndroidManifest.xml` souboru:

* Přidejte hello názvů Amazon, hello soubor by měl začínat takto:
  
      <?xml version="1.0" encoding="utf-8"?>
      <manifest xmlns:android="http://schemas.android.com/apk/res/android"
                xmlns:amazon="http://schemas.amazon.com/apk/res/android"
* Uvnitř hello `<application/>` značky, přidejte v této části:
  
      <amazon:enable-feature
         android:name="com.amazon.device.messaging"
         android:required="false"/>
  
      <meta-data android:name="engagement:adm:register" android:value="true" />
* Po přidání značka hello amazon, může mít chyby sestavení, pokud je váš cíl sestavení projektu menší než Android 2.1. Máte toouse **Android 2.1 +** sestavení cíl (nemusíte si dělat starosti, můžete mít dál `minSdkVersion` nastavit too4).
* Integrovat hello klíč rozhraní API ADM jako prostředek podle [tento postup].

Potom postupujte podle pokynů hello hello další části.

### <a name="communicate-registration-id-toohello-engagement-push-service-and-receive-notifications"></a>Komunikaci registrace id toohello Engagement nabízené služby a přijímat oznámení
V pořadí toocommunicate hello id registrace toohello zařízení hello Engagement nabízené služby a jeho oznámení dostávat, přidat následující tooyour hello `AndroidManifest.xml` souboru uvnitř hello `<application/>` značky (i když používáte ADM bez zapojení):

        <receiver android:name="com.microsoft.azure.engagement.adm.EngagementADMEnabler"
          android:exported="false">
          <intent-filter>
            <action android:name="com.microsoft.azure.engagement.intent.action.APPID_GOT"/>
          </intent-filter>
        </receiver>

         <receiver android:name="com.microsoft.azure.engagement.adm.EngagementADMReceiver"
           android:permission="com.amazon.device.messaging.permission.SEND">
          <intent-filter>
            <action android:name="com.amazon.device.messaging.intent.REGISTRATION"/>
            <action android:name="com.amazon.device.messaging.intent.RECEIVE"/>
            <category android:name="<your_package_name>"/>
          </intent-filter>
        </receiver>   

Ujistěte se, máte následující oprávnění v hello vaše `AndroidManifest.xml` (před hello `</application>` značka).

        <uses-permission android:name="android.permission.WAKE_LOCK"/>
        <uses-permission android:name="com.amazon.device.messaging.permission.RECEIVE"/>
        <uses-permission android:name="<your_package_name>.permission.RECEIVE_ADM_MESSAGE"/>
        <permission android:name="<your_package_name>.permission.RECEIVE_ADM_MESSAGE" android:protectionLevel="signature"/>

## <a name="grant-engagement-oauth-credentials"></a>Přihlašovací údaje OAuth grant zapojení
Odešlete vaše přihlašovací údaje OAuth (ID klienta a tajný klíč klienta) na portálu Engagement.

[< https://developer.amazon.com/sdk/adm/credentials.html>]:https://developer.amazon.com/sdk/adm/credentials.html
[klientské knihovny ADM]:https://developer.amazon.com/sdk/adm/setup.html
[integrované ADM]:https://developer.amazon.com/sdk/adm/integrating-app.html
[tento postup]:https://developer.amazon.com/sdk/adm/integrating-app.html#Asset
