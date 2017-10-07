---
title: aaaAzure integrace sady Mobile Engagement Web SDK | Microsoft Docs
description: "Hello nejnovější aktualizace a postupy pro hello sada SDK webové služby Azure Mobile Engagement"
services: mobile-engagement
documentationcenter: mobile
author: piyushjo
manager: erikre
editor: 
ms.assetid: b5daa2a2-942b-489d-aa1d-568c3b25e56f
ms.service: mobile-engagement
ms.workload: mobile
ms.tgt_pltfrm: web
ms.devlang: js
ms.topic: article
ms.date: 02/29/2016
ms.author: piyushjo
ms.openlocfilehash: 99613b68b615bec4ddcfcc8e4e0133ce9d887bad
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="integrate-azure-mobile-engagement-in-a-web-application"></a>Integrovat Azure Mobile Engagement ve webové aplikaci
> [!div class="op_single_selector"]
> * [Univerzální pro Windows](mobile-engagement-windows-store-integrate-engagement.md)
> * [Windows Phone Silverlight](mobile-engagement-windows-phone-integrate-engagement.md)
> * [iOS](mobile-engagement-ios-integrate-engagement.md)
> * [Android](mobile-engagement-android-integrate-engagement.md)
> 
> 

Hello postupy v tomto článku popisují hello nejjednodušší způsob, jak tooactivate hello analýzy a monitorování funkcí v Azure Mobile Engagement ve webové aplikaci.

Postupujte podle hello kroky tooactivate hello protokolu sestavy, které jsou potřebné toocompute všechny statistické údaje o uživatelích, relacích, aktivity, dojde k chybě a technicals. Statistics závislé aplikace, jako jsou události a chyby, úlohy je nutné aktivovat sestavy protokolu ručně pomocí hello rozhraní API služby Azure Mobile Engagement. Další informace najdete další [jak toouse hello advanced označování rozhraní API ve webové aplikaci Mobile Engagement](mobile-engagement-web-use-engagement-api.md).

## <a name="introduction"></a>Úvod
[Stáhnout hello sada SDK webové služby Azure Mobile Engagement](http://aka.ms/P7b453).
Hello Mobile Engagement Web SDK je dodávána jako jeden soubor JavaScript, azure-engagement.js, které jste tooinclude v každé stránce webu či webové aplikace.

> [!IMPORTANT]
> Před spuštěním tohoto skriptu, musíte spustit skript nebo kód fragment kódu, který zápisu tooconfigure Mobile Engagementu pro vaši aplikaci.
> 
> 

## <a name="browser-compatibility"></a>Kompatibilita s prohlížeči
Hello Mobile Engagement Web SDK používá nativní JSON kódování a dekódování kromě požadavky AJAX toocross domény (přijímající na specifikaci W3C CORS hello). Je kompatibilní s hello následujících prohlížečů:

* Microsoft Edge 12 +
* Internet Explorer 10 +
* Firefox 3.5 +
* Chrome 4 +
* Safari 6 +
* Opera 12 +

## <a name="configure-mobile-engagement"></a>Konfigurace Mobile Engagement
Napsat skript, který vytvoří globální konfiguraci `azureEngagement` objekt jazyka JavaScript, stejně jako hello následující ukázka. Vzhledem k tomu, že váš web může mít násobky stránky, tento příklad předpokládá, že tento skript je zahrnuta v každé stránce. V tomto příkladu je název objektu JavaScript hello `azure-engagement-conf.js`.

    window.azureEngagement = {
      connectionString: 'Endpoint={appCollection}.{domain};AppId={appId};SdkKey={sdkKey}',
      appVersionName: '1.0.0',
      appVersionCode: 1
    };

Hello `connectionString` hodnotu pro vaše aplikace je zobrazena v hello portálu Azure.

> [!NOTE]
> `appVersionName`a `appVersionCode` jsou volitelné. Doporučujeme však, že jste je nakonfigurovat tak, aby analytics může zpracovat informace o verzi.
> 
> 

## <a name="include-mobile-engagement-scripts-in-your-pages"></a>Zahrnout Mobile Engagement skripty na svých stránkách
Přidání Mobile Engagement skripty tooyour stránky v jednom z následujících způsobů hello:

    <head>
      ...
      <script type="text/javascript" src="azure-engagement-conf.js"></script>
      <script type="text/javascript" src="azure-engagement.js"></script>
      ...
    </head>

Nebo to:

    <body>
      ...
      <script type="text/javascript" src="azure-engagement-conf.js"></script>
      <script type="text/javascript" src="azure-engagement.js"></script>
      ...
    </body>

## <a name="alias"></a>Alias
Po načtení hello Mobile Engagement Web SDK skript vytvoří hello **engagement** alias tooaccess hello rozhraní API sady SDK. Konfigurace sady SDK hello toodefine tento alias nelze použít. Tento alias se používá jako odkaz v této dokumentaci.

Všimněte si, že pokud alias výchozí hello je v konfliktu s jinou – globální proměnná ze stránky, můžete upravit ho v hello konfigurace takto před načtením hello Mobile Engagement Web SDK:

    window.azureEngagement = {
      connectionString: 'Endpoint={appCollection}.{domain};AppId={appId};SdkKey={sdkKey}',
      appVersionName: '1.0.0',
      appVersionCode: 1
      alias:'anotherAlias'
    };

## <a name="basic-reporting"></a>Vytváření základních sestav
Základní vytváření sestav v Mobile Engagementu popisuje statistické údaje, relace úrovni, třeba o uživatelích, relacích, aktivity a dojde k chybě.

### <a name="session-tracking"></a>Sledování relace
Mobile Engagement relace je rozdělené do posloupnost aktivit, identifikována názvem.

V klasického webu doporučujeme je deklarovat jiné aktivitě na každé stránce vašeho webu. Web nebo webovou aplikaci, ve které hello aktuální stránku nikdy změní můžete tootrack hello aktivity v menším měřítku, jako třeba v rámci stránky hello.

Buď způsobem, toostart nebo změňte hello aktuální aktivity uživatelů, volání hello `engagement.agent.startActivity` funkce. Například:

    <body onload="yourOnload()">

    <!-- -->

    yourOnload = function() {
      [...]
      engagement.agent.startActivity('welcome');
    };

Mobile Engagement server Hello automaticky ukončí otevřít relaci do tří minut po zavření stránky aplikace hello.

Alternativně můžete ukončit relaci ručně voláním `engagement.agent.endActivity`. Toto nastaví hello aktuální uživatel aktivity too'Idle. "  Hello relace bude ukončena 10 sekund později, pokud novou volání příliš`engagement.agent.startActivity` obnoví hello relace.

Prodlevu hello 10 do objektu hello globální zapojení, můžete nakonfigurovat následujícím způsobem:

    engagement.sessionTimeout = 2000; // 2 seconds
    // or
    engagement.sessionTimeout = 0; // end hello session as soon as endActivity is called

> [!NOTE]
> Nemůžete použít `engagement.agent.endActivity` v hello `onunload` zpětného volání vzhledem k tomu, že nemůžete provádět volání AJAX v této fázi.
> 
> 

## <a name="advanced-reporting"></a>Rozšířená tvorba sestav
Volitelně Pokud chcete události specifické pro aplikaci tooreport, chyb a úlohy, je nutné toouse hello Mobile Engagement API. Přístup k hello Mobile Engagement API prostřednictvím hello `engagement.agent` objektu.

Přístup ke všem hello rozšířené možnosti v Mobile Engagementu v hello Mobile Engagement API. Hello rozhraní API je podrobně popsaná v článku hello [jak toouse hello advanced označování rozhraní API ve webové aplikaci Mobile Engagement](mobile-engagement-web-use-engagement-api.md).

## <a name="customize-hello-urls-used-for-ajax-calls"></a>Přizpůsobení adresy URL hello použít pro volání AJAX
Adresy URL můžete přizpůsobit této hello, jaký používá SDK webové služby Mobile Engagement. Například tooredefine hello protokolu adresu URL (hello SDK koncový bod pro protokolování), můžete přepsat konfiguraci hello podobné výjimky:

    window.azureEngagement = {
      ...
      urls: {
        ...        
        getLoggerUrl: function() {
        return 'someProxy/log';
        }
      }
    };

Pokud adresa URL funkce vrátí řetězec, který začíná `/`, `//`, `http://`, nebo `https://`, výchozí schéma hello nepoužívá. Ve výchozím nastavení, hello `https://` schéma se používá pro tyto adresy URL. Pokud chcete toocustomize hello výchozí schéma, přepište hello konfigurace, například takto:

    window.azureEngagement = {
      ...
      urls: {
        ...         
        scheme: '//'
      }
    };
