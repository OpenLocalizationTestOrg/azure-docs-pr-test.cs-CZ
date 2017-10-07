---
title: aaaHow tooUse hello JavaScript SDK pro Azure Mobile Apps
description: Jak v tooUse pro Azure Mobile Apps
services: app-service\mobile
documentationcenter: javascript
author: ggailey777
manager: syntaxc4
editor: 
ms.assetid: 53b78965-caa3-4b22-bb67-5bd5c19d03c4
ms.service: app-service-mobile
ms.workload: mobile
ms.tgt_pltfrm: html
ms.devlang: javascript
ms.topic: article
ms.date: 10/30/2016
ms.author: glenga
ms.openlocfilehash: 3fcbb0c5bd6918a285bdafa1946ba0bd47bb21b0
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="how-toouse-hello-javascript-client-library-for-azure-mobile-apps"></a>Jak tooUse hello knihovny JavaScript klienta pro Azure Mobile Apps
[!INCLUDE [app-service-mobile-selector-client-library](../../includes/app-service-mobile-selector-client-library.md)]

Tato příručka je určena tooperform běžné scénáře s využitím hello nejnovější [JavaScript SDK pro Azure Mobile Apps]. Pokud jste nový tooAzure Mobile Apps, nejprve dokončit [Azure Mobile Apps rychlý Start] toocreate back-end a vytvořte tabulku. V této příručce se zaměříme na používání mobilního back-endu hello v HTML/JavaScript webových aplikací.

## <a name="supported-platforms"></a>Podporované platformy
Jsme omezit aktuální toohello podpora prohlížeče a poslední verze hello hlavní prohlížeče: Google Chrome, Microsoft Edge, Microsoft Internet Explorer a Mozilla Firefox.  Očekáváme, že toofunction SDK hello pomocí libovolného relativně moderní prohlížeče.

Hello balíček je distribuován jako univerzální modul JavaScript, tak, aby podporuje globals, AMD, a formátuje CommonJS.

## <a name="Setup"></a>Instalační program a požadavky
Tato příručka předpokládá, že jste vytvořili back-end s tabulkou. Tato příručka předpokládá, že tabulka hello má hello stejného schématu jako hello tabulky v těchto kurzech.

Instalace hello Azure Mobile Apps JavaScript SDK, můžete to udělat pomocí hello `npm` příkaz:

```
npm install azure-mobile-apps-client --save
```

Hello knihovně mohou sloužit také jako modul ES2015 v rámci CommonJS prostředí, například Browserify a Webpack a jako AMD knihovny.  Například:

```
# For ECMAScript 5.1 CommonJS
var WindowsAzure = require('azure-mobile-apps-client');
# For ES2015 modules
import * as WindowsAzure from 'azure-mobile-apps-client';
```

Můžete také předem připravené verzi hello SDK stáhnout přímo z našich CDN:

```html
<script src="https://zumo.blob.core.windows.net/sdk/azure-mobile-apps-client.min.js"></script>
```

[!INCLUDE [app-service-mobile-html-js-library](../../includes/app-service-mobile-html-js-library.md)]

## <a name="auth"></a>Postupy: ověřování uživatelů
Azure App Service podporuje ověřování a autorizaci uživatelů aplikace pomocí různých zprostředkovatelů externí identity: Facebook, Google, Microsoft Account a Twitter. Oprávnění můžete nastavit na tabulky toorestrict přístup pro určité operace tooonly ověřeného uživatele. Můžete taky hello identity ověřené uživatele tooimplement autorizačních pravidel ve skriptech serveru. Další informace najdete v tématu hello [Začínáme s ověřováním] kurzu.

Jsou podporovány dva ověřování toky: serveru a klienta tok.  Vývojový server Hello poskytuje hello nejjednodušší ověřování, jako je závislé na hello zprostředkovatele webového ověření rozhraní. Hello tok klienta umožňuje hlubší integrace s funkcemi konkrétní zařízení, jako-jednotné přihlášení jako přitom spoléhá na specifický pro zprostředkovatele sady SDK.

[!INCLUDE [app-service-mobile-html-js-auth-library](../../includes/app-service-mobile-html-js-auth-library.md)]

### <a name="configure-external-redirect-urls"></a>Postupy: Konfigurace služby Mobile App pro adresy URL pro externí přesměrování.
Několik typů aplikací JavaScript pomocí funkce toohandle zpětné smyčky, kterou toků OAuth uživatelského rozhraní.  Tyto funkce patří:

* Spuštění služby místně
* Načtěte za provozu pomocí hello iontových Framework
* Přesměrování tooApp služby pro ověřování.

Spuštěn místně může způsobit problémy, protože ve výchozím nastavení, aplikační služby ověřování je jenom nakonfigurován tooallow přístup z váš back-end mobilní aplikace. Použijte následující postup toochange hello ověřování tooenable nastavení služby App Service při místním spuštění hello server hello:

1. Přihlaste se toohello [portálu Azure]
2. Přejděte back-end mobilní aplikace tooyour.
3. Vyberte **Průzkumníka prostředků** v hello **nástroje pro vývoj** nabídky.
4. Klikněte na tlačítko **přejděte** tooopen hello Průzkumníka prostředků pro váš back-end mobilní aplikace v nové záložky nebo okno.
5. Rozbalte hello **konfigurace** > **authsettings** uzel pro vaši aplikaci.
6. Klikněte na tlačítko hello **upravit** tlačítko tooenable úpravy hello prostředku.
7. Najde hello **allowedExternalRedirectUrls** element, který by měl mít hodnotu null. Přidejte vaší adresy URL do pole:

         "allowedExternalRedirectUrls": [
             "http://localhost:3000",
             "https://localhost:3000"
         ],

    Nahraďte hello adresy URL v poli hello hello adresy URL služby, který v tomto příkladu je `http://localhost:3000` hello místní Node.js Ukázka služby. Můžete také použít `http://localhost:4400` pro hello Ripple služby nebo některých jiných adresu URL, v závislosti na konfiguraci aplikace.
8. V horní části hello hello stránky, klikněte na tlačítko **pro čtení a zápis**, pak klikněte na tlačítko **PUT** toosave vaše aktualizace.

Musíte taky tooadd hello stejné zpětné smyčky adresy URL toohello CORS povolených nastavení:

1. Přejděte zpět toohello [portálu Azure].
2. Přejděte back-end mobilní aplikace tooyour.
3. Klikněte na tlačítko **CORS** v hello **rozhraní API** nabídky.
4. Zadejte jednotlivé adresy URL v hello prázdný **povolené zdroje** textové pole.  Vytvoří se nové textové pole.
5. Klikněte na tlačítko **uložit**

Po aktualizaci hello back-end, bude možné toouse hello nové zpětné smyčky adresy URL ve vaší aplikaci.

<!-- URLs. -->
[Azure Mobile Apps rychlý Start]: app-service-mobile-cordova-get-started.md
[Začínáme s ověřováním]: app-service-mobile-cordova-get-started-users.md
[Add authentication tooyour app]: app-service-mobile-cordova-get-started-users.md

[portálu Azure]: https://portal.azure.com/
[JavaScript SDK pro Azure Mobile Apps]: https://www.npmjs.com/package/azure-mobile-apps-client
[Query object documentation]: https://msdn.microsoft.com/en-us/library/azure/jj613353.aspx
