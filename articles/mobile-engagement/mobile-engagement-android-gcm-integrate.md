---
title: aaaAzure integraci sady Android SDK Mobile Engagement
description: "Nejnovější aktualizace a postupy pro Android SDK pro Azure Mobile Engagement"
services: mobile-engagement
documentationcenter: mobile
author: piyushjo
manager: erikre
editor: 
ms.assetid: d72b5014-a22b-4a7f-a470-d2b8145b5b86
ms.service: mobile-engagement
ms.workload: mobile
ms.tgt_pltfrm: mobile-android
ms.devlang: Java
ms.topic: article
ms.date: 10/10/2016
ms.author: piyushjo
ms.openlocfilehash: e81230cbc99a209f2909cc163c4e566df67dc828
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="how-toointegrate-gcm-with-mobile-engagement"></a>Jak tooIntegrate GCM s Mobile Engagement
> [!IMPORTANT]
> Postupujte podle hello integrace postup popsaný v hello jak tooIntegrate Engagement v systému Android dokumentu před těchto pokynů.
> 
> Tento dokument je užitečný jenom v případě, že jste již integrované hello dosáhnout modulu a plán toopush Google Play zařízení. kampaně Reach toointegrate ve vaší aplikaci, přečtěte si nejprve jak tooIntegrate Engagement Reach v systému Android.
> 
> 

## <a name="introduction"></a>Úvod
Integrace služby GCM umožňuje vaší aplikace toobe poslat.

Datové části GCM nabídnutých toohello SDK vždy obsahovat hello `azme` klíče v hello datový objekt. Proto pokud používáte GCM pro jiný účel ve vaší aplikaci, můžete filtrovat podle klíči nabízených oznámení.

> [!IMPORTANT]
> Jenom zařízení se systémem Android 2.2 nebo vyšší, s Google Play nainstalovaná a má Google pozadí připojení povoleno mohou poslat pomocí služby GCM; Můžete však integrovat tento kód bezpečně na nepodporované zařízení (používá právě záměry).
> 
> 

## <a name="create-a-google-cloud-messaging-project-with-api-key"></a>Vytvoření projektu služby GCM (Google Cloud Messaging) s klíčem rozhraní API
[!INCLUDE [mobile-engagement-enable-Google-cloud-messaging](../../includes/mobile-engagement-enable-google-cloud-messaging.md)]

## <a name="sdk-integration"></a>Integrace sady SDK
### <a name="managing-device-registrations"></a>Správa registrace zařízení
Každé zařízení musí odeslat příkaz toohello registrace servery Googlu, jinak nelze získat přístup.

Zařízení můžete také zrušit registraci oznámení GCM (hello zařízení se automaticky registrace, když se odinstaluje aplikace hello).

Pokud nepoužijete [Google Play SDK] nebo jste již Neodesílat hello registrace záměr sami, můžete provést Engagement registrovat zařízení hello automaticky za vás.

tooenable, přidejte následující tooyour hello `AndroidManifest.xml` souboru uvnitř hello `<application/>` značky:

            <!-- If only 1 sender, don't forget hello \n, otherwise it will be parsed as a negative number... -->
            <meta-data android:name="engagement:gcm:sender" android:value="<Your Google Project Number>\n" />

### <a name="communicate-registration-id-toohello-engagement-push-service-and-receive-notifications"></a>Komunikaci registrace id toohello Engagement nabízené služby a přijímat oznámení
V pořadí toocommunicate hello id registrace toohello zařízení hello Engagement nabízené služby a jeho oznámení dostávat, přidat následující tooyour hello `AndroidManifest.xml` souboru uvnitř hello `<application/>` značky (i v případě registrace zařízení spravujete sami):

            <receiver android:name="com.microsoft.azure.engagement.gcm.EngagementGCMEnabler"
              android:exported="false">
              <intent-filter>
                <action android:name="com.microsoft.azure.engagement.intent.action.APPID_GOT" />
              </intent-filter>
            </receiver>

            <receiver android:name="com.microsoft.azure.engagement.gcm.EngagementGCMReceiver" android:permission="com.google.android.c2dm.permission.SEND">
              <intent-filter>
                <action android:name="com.google.android.c2dm.intent.REGISTRATION" />
                <action android:name="com.google.android.c2dm.intent.RECEIVE" />
                <category android:name="<your_package_name>" />
              </intent-filter>
            </receiver>

Ujistěte se, máte následující oprávnění v hello vaše `AndroidManifest.xml` (po hello `</application>` značka).

            <uses-permission android:name="com.google.android.c2dm.permission.RECEIVE" />
            <uses-permission android:name="<your_package_name>.permission.C2D_MESSAGE" />
            <permission android:name="<your_package_name>.permission.C2D_MESSAGE" android:protectionLevel="signature" />

## <a name="grant-mobile-engagement-access-tooyour-gcm-api-key"></a>Mobile Engagement udělení přístupu tooyour klíč rozhraní API GCM
Postupujte podle [Tato příručka](mobile-engagement-android-get-started.md#grant-mobile-engagement-access-to-your-gcm-api-key) toogrant Mobile Engagement přístup tooyour klíč rozhraní API GCM.

[Google Play SDK]:https://developers.google.com/cloud-messaging/android/start
