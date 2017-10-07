---
title: aaaHow tooUse modul plug-in pro Apache Cordova pro Azure Mobile Apps
description: Jak tooUse modul plug-in pro Apache Cordova pro Azure Mobile Apps
services: app-service\mobile
documentationcenter: javascript
author: ggailey777
manager: syntaxc4
editor: 
ms.assetid: a56a1ce4-de0c-4f3c-8763-66252c52aa59
ms.service: app-service-mobile
ms.workload: mobile
ms.tgt_pltfrm: mobile-html
ms.devlang: javascript
ms.topic: article
ms.date: 10/30/2016
ms.author: glenga
ms.openlocfilehash: d3e0639e6478c409132af25304a2fb0f28401e98
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="how-toouse-apache-cordova-client-library-for-azure-mobile-apps"></a>Jak Klientská knihovna pro Apache Cordova toouse pro Azure Mobile Apps
[!INCLUDE [app-service-mobile-selector-client-library](../../includes/app-service-mobile-selector-client-library.md)]

Tato příručka je určena tooperform běžné scénáře s využitím hello nejnovější [modul plug-in pro Apache Cordova pro Azure Mobile Apps]. Pokud jste nový tooAzure Mobile Apps, nejprve dokončit [Azure Mobile Apps rychlý Start] toocreate back-end, vytvořit tabulku a stáhněte si předem připravené projekt Apache Cordova. V této příručce se zaměříme na straně klienta hello modulu plug-in Apache Cordova.

## <a name="supported-platforms"></a>Podporované platformy
Tato sada SDK podporuje v6.0.0 Apache Cordova a později na iOS, Android a Windows zařízení.  Podpora platformy Hello vypadá takto:

* Rozhraní API systému Android 19 – 24 (KitKat prostřednictvím cukrovinkách typu nugát).
* iOS verze 8.0 a novější.
* Windows Phone 8.1.
* Univerzální platformy Windows.

## <a name="Setup"></a>Instalační program a požadavky
Tato příručka předpokládá, že jste vytvořili back-end s tabulkou. Tato příručka předpokládá, že tabulka hello má hello stejného schématu jako hello tabulky v těchto kurzech. Tato příručka také předpokládá, že jste přidali kód tooyour Apache Cordovu hello.  Pokud jste tak dosud neučinili, můžete na příkazovém řádku hello přidat hello Apache Cordova modulu plug-in tooyour projektu:

```
cordova plugin add cordova-plugin-ms-azure-mobile-apps
```

Další informace o vytváření [vaší první aplikace Apache Cordova], najdete v jejich dokumentaci.

## <a name="ionic"></a>Nastavení aplikace iontových v2

tooproperly konfigurace projektu iontových v2, nejprve vytvořit základní aplikaci a přidání modulu plug-in Cordova hello:

```
ionic start projectName --v2
cd projectName
ionic plugin add cordova-plugin-ms-azure-mobile-apps
```

Přidejte následující řádky příliš hello`app.component.ts` toocreate hello klientského objektu:

```
declare var WindowsAzure: any;
var client = new WindowsAzure.MobileServiceClient("https://yoursite.azurewebsites.net");
```

Teď můžete sestavit a spustit projekt hello v prohlížeči hello:

```
ionic platform add browser
ionic run browser
```

modul plug-in Azure Mobile Apps Cordova Hello podporuje obě iontových v1 a v2 aplikace.  Jenom aplikace hello iontových v2 vyžadovat další deklarace pro hello `WindowsAzure` objektu.

[!INCLUDE [app-service-mobile-html-js-library.md](../../includes/app-service-mobile-html-js-library.md)]

## <a name="auth"></a>Postupy: ověřování uživatelů
Azure App Service podporuje ověřování a autorizaci uživatelů aplikace pomocí různých zprostředkovatelů externí identity: Facebook, Google, Microsoft Account a Twitter. Oprávnění můžete nastavit na tabulky toorestrict přístup pro určité operace tooonly ověřeného uživatele. Můžete taky hello identity ověřené uživatele tooimplement autorizačních pravidel ve skriptech serveru. Další informace najdete v tématu hello [Začínáme s ověřováním] kurzu.

Při použití ověřování v aplikaci Apache Cordova, musí být k dispozici následující moduly plug-in Cordova hello:

* [cordova-plugin-device]
* [cordova-plugin-inappbrowser]

Jsou podporovány dva ověřování toky: serveru a klienta tok.  Vývojový server Hello poskytuje hello nejjednodušší ověřování, jako je závislé na hello zprostředkovatele webového ověření rozhraní. Hello tok klienta umožňuje hlubší integrace s funkcemi konkrétní zařízení, jako-jednotné přihlášení jako přitom spoléhá na konkrétní zařízení specifické pro poskytovatele sady SDK.

[!INCLUDE [app-service-mobile-html-js-auth-library.md](../../includes/app-service-mobile-html-js-auth-library.md)]

### <a name="configure-external-redirect-urls"></a>Postupy: Konfigurace služby Mobile App pro adresy URL pro externí přesměrování.
Několik typů aplikací Apache Cordova pomocí funkce toohandle zpětné smyčky, kterou toků OAuth uživatelského rozhraní.  Vzhledem k tomu, že služba ověřování hello zná pouze toky OAuth uživatelského rozhraní na místním hostiteli dojít k problémům, jak tooutilize služby ve výchozím nastavení.  Příklady problematické toky OAuth uživatelského rozhraní:

* Hello Ripple emulatoru.
* Živé opětovného načtení s Ionic.
* Spuštění mobilního back-endu hello místně
* Spuštění hello mobilní back-end v různých App Service Azure než jeden poskytnete ověřování hello.

Postupujte podle těchto pokynů tooadd konfiguraci toohello místní nastavení:

1. Přihlaste se toohello [portálu Azure]
2. Vyberte **všechny prostředky** nebo **App Services** pak klikněte na název hello mobilní aplikace.
3. Klikněte na tlačítko **nástroje**
4. Klikněte na tlačítko **Průzkumníka prostředků** v nabídce dodržovat hello klikněte **přejděte**.  Otevře se nové okno nebo kartu.
5. Rozbalte hello **konfigurace**, **authsettings** uzly pro svůj web v levém navigačním panelu hello.
6. Klikněte na tlačítko **upravit**
7. Vyhledejte element "allowedExternalRedirectUrls" hello.  Může být nastaven toonull nebo pole hodnot.  Změna toohello hodnota hello následující hodnotu:

         "allowedExternalRedirectUrls": [
             "http://localhost:3000",
             "https://localhost:3000"
         ],

    Adresy URL hello nahraďte hello adresy URL služby.  Mezi příklady patří http://localhost ": 3000" (pro služby ukázka Node.js hello) nebo "http://localhost:4400" (pro službu Ripple hello).  Ale tyto adresy URL jsou příklady - vaší situaci, včetně hello služeb uvedených v příkladech hello se můžou lišit.
8. Klikněte na tlačítko hello **pro čtení a zápis** tlačítko v pravém horním rohu hello úvodní obrazovka.
9. Klikněte na zelenou hello **PUT** tlačítko.

Hello nastavení se ukládají v tomto okamžiku.  Nezavírejte okno prohlížeče hello, dokud nebudou dokončeny hello nastavení ukládání.
Také přidáte tyto zpětné smyčky adresy URL toohello nastavení CORS pro App Service:

1. Přihlaste se toohello [portálu Azure]
2. Vyberte **všechny prostředky** nebo **App Services** pak klikněte na název hello mobilní aplikace.
3. automaticky se otevře okno nastavení Hello.  Pokud není, klikněte na tlačítko **všechna nastavení**.
4. Klikněte na tlačítko **CORS** v nabídce hello rozhraní API.
5. Zadejte adresu URL hello, který chcete tooadd v poli hello a stiskněte klávesu Enter.
6. Zadejte další adresy URL, podle potřeby.
7. Klikněte na tlačítko **Uložit** toosave hello nastavení.

Jak dlouho trvá přibližně 10 až 15 sekund pro hello nové nastavení tootake vliv.

## <a name="register-for-push"></a>Postupy: zaregistrovat pro nabízená oznámení
Nainstalujte hello [phonegap-plugin nabízené] toohandle nabízená oznámení.  Tento modul plug-in lze snadno přidat pomocí `cordova plugin add` příkazu na příkazovém řádku hello nebo prostřednictvím hello Git modulu plug-in instalační program v sadě Visual Studio.  Následující kód do vaší aplikace Apache Cordova registruje zařízení pro nabízená oznámení:

```
var pushOptions = {
    android: {
        senderId: '<from-gcm-console>'
    },
    ios: {
        alert: true,
        badge: true,
        sound: true
    },
    windows: {
    }
};
pushHandler = PushNotification.init(pushOptions);

pushHandler.on('registration', function (data) {
    registrationId = data.registrationId;
    // For cross-platform, you can use hello device plugin toodetermine hello device
    // Best is toouse device.platform
    var name = 'gcm'; // For android - default
    if (device.platform.toLowerCase() === 'ios')
        name = 'apns';
    if (device.platform.toLowerCase().substring(0, 3) === 'win')
        name = 'wns';
    client.push.register(name, registrationId);
});

pushHandler.on('notification', function (data) {
    // data is an object and is whatever is sent by hello PNS - check hello format
    // for your particular PNS
});

pushHandler.on('error', function (error) {
    // Handle errors
});
```

Použijte hello SDK centra oznámení toosend nabízená oznámení ze serveru hello.  Nikdy odeslat nabízená oznámení přímo z klientů. To proto může být použité tootrigger útoku DoS proti centra oznámení nebo hello systém PNS.  Hello systém PNS může zakázat provozu v důsledku takové útoky.

## <a name="more-information"></a>Další informace

Můžete najít podrobnosti podrobné rozhraní API v našem [dokumentaci k rozhraní API](http://azure.github.io/azure-mobile-apps-js-client/).

<!-- URLs. -->
[portálu Azure]: https://portal.azure.com
[Azure Mobile Apps rychlý Start]: app-service-mobile-cordova-get-started.md
[Začínáme s ověřováním]: app-service-mobile-cordova-get-started-users.md
[Add authentication tooyour app]: app-service-mobile-cordova-get-started-users.md

[modul plug-in pro Apache Cordova pro Azure Mobile Apps]: https://www.npmjs.com/package/cordova-plugin-ms-azure-mobile-apps
[vaší první aplikace Apache Cordova]: http://cordova.apache.org/#getstarted
[phonegap-facebook-plugin]: https://github.com/wizcorp/phonegap-facebook-plugin
[phonegap-plugin nabízené]: https://www.npmjs.com/package/phonegap-plugin-push
[cordova-plugin-device]: https://www.npmjs.com/package/cordova-plugin-device
[cordova-plugin-inappbrowser]: https://www.npmjs.com/package/cordova-plugin-inappbrowser
[Query object documentation]: https://msdn.microsoft.com/en-us/library/azure/jj613353.aspx
