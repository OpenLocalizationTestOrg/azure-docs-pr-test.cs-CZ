---
title: Konfigurace aaaAdvanced pro Azure Mobile Engagement Android SDK
description: "Popisuje hello rozšířených možnostech konfigurace, včetně hello Android Manifest s Azure Mobile Engagement Android SDK"
services: mobile-engagement
documentationcenter: mobile
author: piyushjo
manager: erikre
editor: 
ms.assetid: 37d2c09a-86fa-473d-8987-c7e35a0eb3e8
ms.service: mobile-engagement
ms.workload: mobile
ms.tgt_pltfrm: mobile-android
ms.devlang: Java
ms.topic: article
ms.date: 10/04/2016
ms.author: piyushjo;ricksal
ms.openlocfilehash: 757abf362021fd018f444cae6305524623e77062
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="advanced-configuration-for-azure-mobile-engagement-android-sdk"></a>Upřesňující konfigurace pro Azure Mobile Engagement Android SDK
> [!div class="op_single_selector"]
> * [Univerzální platforma Windows](mobile-engagement-windows-store-advanced-configuration.md)
> * [Windows Phone Silverlight](mobile-engagement-windows-phone-integrate-engagement.md)
> * [iOS](mobile-engagement-ios-integrate-engagement.md)
> * [Android](mobile-engagement-android-advanced-configuration.md)
>
>

Tento postup popisuje, jak tooconfigure různé možnosti konfigurace aplikací pro Azure Mobile Engagement Android.

## <a name="prerequisites"></a>Požadavky
[!INCLUDE [Prereqs](../../includes/mobile-engagement-android-prereqs.md)]

## <a name="permission-requirements"></a>Požadavky na oprávnění
Některé možnosti vyžadují specifické oprávnění, které jsou zde uvedeny pro referenční a v řádku v hello příslušné funkce. Přidat tyto oprávnění toohello AndroidManifest.xml vašeho projektu těsně před nebo po hello `<application>` značky.

Kód oprávnění Hello musí toolook jako hello následující, kde vyplňte příslušná oprávnění hello z hello tabulky, který následuje.

    <uses-permission android:name="android.permission.[specific permission]"/>


| Oprávnění | Při použití |
| --- | --- |
| INTERNET |Povinná hodnota. Pro základní vytváření sestav |
| ACCESS_NETWORK_STATE |Povinná hodnota. Pro základní vytváření sestav |
| RECEIVE_BOOT_COMPLETED |Povinná hodnota. tooshow do center oznámení hello po restartování zařízení |
| WAKE_LOCK |Nedoporučuje. Umožňuje shromažďování dat při použití Wi-Fi nebo při vypnuté obrazovky |
| ZAVIBROVAT |Volitelné. Umožňuje vibracím při přijetí oznámení |
| DOWNLOAD_WITHOUT_NOTIFICATION |Volitelné. Umožňuje oznámení Android velký obrázek |
| WRITE_EXTERNAL_STORAGE |Volitelné. Umožňuje oznámení Android velký obrázek |
| ACCESS_COARSE_LOCATION |Volitelné. Umožňuje hlášení polohy v reálném čase |
| ACCESS_FINE_LOCATION |Volitelné. Umožňuje vytváření sestav na základě GPS umístění |

Od verze Android M [některá oprávnění jsou spravovány v době běhu](mobile-engagement-android-location-reporting.md#android-m-permissions).

Pokud už používáte ``ACCESS_FINE_LOCATION``, pak nepotřebujete tooalso použít ``ACCESS_COARSE_LOCATION``.

## <a name="android-manifest-configuration-options"></a>Možnosti konfigurace pro Android manifestu
### <a name="crash-report"></a>Hlášení o selhání
toodisable hlášení selhání, přidejte tento kód mezi hello `<application>` a `</application>` značky:

    <meta-data android:name="engagement:reportCrash" android:value="false"/>

### <a name="burst-threshold"></a>Prahová hodnota shluku
Ve výchozím nastavení sestavy služby zapojení hello protokolů v reálném čase. Pokud se vaše protokoly sestavy aplikací často liší, je lepší toobuffer hello protokoly a tooreport je všechny najednou na pravidelné základní časové (nazývané "burst režim"). toodo Ano, přidat tento kód mezi hello `<application>` a `</application>` značky:

    <meta-data android:name="engagement:burstThreshold" android:value="{interval between too bursts (in milliseconds)}"/>

Shluků režimu mírně zvyšuje hello z baterie, ale má vliv na hello monitorování Engagement: všechny dobu trvání relace a úlohy jsou prahová hodnota shluků zaokrouhlené toohello (tedy relací a úlohy kratší, než je prahová hodnota shluků hello nemusí být zobrazeny). Vaše shluků prahová hodnota musí být delší než 30000 (30s).

### <a name="session-timeout"></a>Časový limit relace
 Aktivitu můžete ukončit pomocí klávesy hello **Domů** nebo **zpět** klíče, nastavení hello phone nečinnosti nebo přechod do jiné aplikace. Ve výchozím nastavení je relace skončila deseti sekund po skončení hello jeho poslední aktivita. Tím je zabráněno rozdělení relace každý uživatel hello čas ukončení a rychle, vrátí toohello aplikace, které může dojít při hello uživatele převezme bitovou kopii, zkontroluje, oznámení atd. Můžete chtít toomodify tento parametr. toodo Ano, přidat tento kód mezi hello `<application>` a `</application>` značky:

    <meta-data android:name="engagement:sessionTimeout" android:value="{session timeout (in milliseconds)}"/>

## <a name="disable-log-reporting"></a>Zakázání zasílání zpráv protokolu
### <a name="using-a-method-call"></a>Pomocí volání metody
Pokud chcete Engagement toostop odesílání protokolů, můžete volat:

    EngagementAgent.getInstance(context).setEnabled(false);

Toto volání je trvalé: používá soubor sdílet předvolby.

Pokud je při volání této funkce aktivní zapojení, může trvat jednu minutu po dobu toostop služby hello. Ale nespustí hello služby všechny hello příštím spuštění aplikace hello.

Můžete povolit protokol reporting znovu voláním hello stejné fungovat s `true`.

### <a name="integration-in-your-own-preferenceactivity"></a>Integrace ve vašem vlastním`PreferenceActivity`
Namísto volání této funkce, můžete také integrovat toto nastavení přímo do existující `PreferenceActivity`.

Zapojení toouse vaše předvolby soubor (s požadovanou režim hello) můžete nakonfigurovat v hello `AndroidManifest.xml` soubor s `application meta-data`:

* Hello `engagement:agent:settings:name` klíč je použité toodefine hello název souboru sdílet předvolby hello.
* Hello `engagement:agent:settings:mode` klíč je použité toodefine hello režim souboru sdílet předvolby hello. Použití hello stejné jako v režimu vaše `PreferenceActivity`. Hello režimu, musí být předán jako číslo: Pokud používáte kombinaci konstantní příznaky ve vašem kódu, zkontrolujte hello celkové hodnoty.

Zapojení vždy používá hello `engagement:key` boolean klíč v souboru předvoleb hello ke správě tohoto nastavení.

Následující příklad Hello `AndroidManifest.xml` ukazuje hello výchozí hodnoty:

    <application>
        [...]
        <meta-data
          android:name="engagement:agent:settings:name"
          android:value="engagement.agent" />
        <meta-data
          android:name="engagement:agent:settings:mode"
          android:value="0" />

Potom můžete přidat `CheckBoxPreference` v vaše předvolby rozložení jako hello jeden následující:

    <CheckBoxPreference
      android:key="engagement:enabled"
      android:defaultValue="true"
      android:title="Use Engagement"
      android:summaryOn="Engagement is enabled."
      android:summaryOff="Engagement is disabled." />
