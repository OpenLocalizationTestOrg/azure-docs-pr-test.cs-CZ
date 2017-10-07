---
title: "ověřování aaaAdd na Apache Cordova s Mobile Apps | Microsoft Docs"
description: "Zjistěte, jak toouse Mobile Apps v Azure App Service tooauthenticate uživatele vaší aplikace Apache Cordova prostřednictvím řady různých zprostředkovatelů identity, včetně Google, Facebook, Twitter a společnosti Microsoft."
services: app-service\mobile
documentationcenter: javascript
author: ggailey777
manager: syntaxc4
editor: 
ms.assetid: 10dd6dc9-ddf5-423d-8205-00ad74929f0d
ms.service: app-service-mobile
ms.workload: na
ms.tgt_pltfrm: mobile-html
ms.devlang: javascript
ms.topic: article
ms.date: 10/30/2016
ms.author: glenga
ms.openlocfilehash: 61a05c5ac67fd0f0bc4c9d6920954a9b464a0a8d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="add-authentication-tooyour-apache-cordova-app"></a>Přidat aplikaci Apache Cordova tooyour ověřování
[!INCLUDE [app-service-mobile-selector-get-started-users](../../includes/app-service-mobile-selector-get-started-users.md)]

## <a name="summary"></a>Souhrn
V tomto kurzu přidáte projekt rychlého startu todolist toohello ověřování na Apache Cordova pomocí zprostředkovatele identity podporované. V tomto kurzu vychází z hello [Začínáme s Mobile Apps] kurz, který je třeba nejprve provést.

## <a name="register"></a>Registrace aplikace pro ověřování a nakonfigurujte hello služby App Service
[!INCLUDE [app-service-mobile-register-authentication](../../includes/app-service-mobile-register-authentication.md)]

[Podívejte se na video zobrazující podobný postup.](https://channel9.msdn.com/series/Azure-connected-services-with-Cordova/Azure-connected-services-task-8-Azure-authentication)

## <a name="permissions"></a>Omezte oprávnění tooauthenticated
[!INCLUDE [app-service-mobile-restrict-permissions-dotnet-backend](../../includes/app-service-mobile-restrict-permissions-dotnet-backend.md)]

Nyní můžete ověřit, že back-end tooyour anonymní přístup je zakázané. V sadě Visual Studio:

* Otevřete hello projekt, který jste vytvořili, když jste dokončili kurz hello [Začínáme s Mobile Apps].
* Spusťte aplikaci v hello **emulátor Google Android**.
* Ověřte, zda neočekávaného selhání připojení je zobrazen po spuštění aplikace hello.

Potom aktualizujte uživatele tooauthenticate aplikace hello před vyžádáním prostředky z back-end mobilní aplikace hello.

## <a name="add-authentication"></a>Přidat aplikaci toohello ověřování
1. Otevřete projekt v **Visual Studio**, pak otevřete hello `www/index.html` soubor pro úpravy.
2. Vyhledejte hello `Content-Security-Policy` značka meta v části head hello.  Přidáte hostitele hello OAuth toohello seznamu povolených zdrojů.

   | Poskytovatel | Název poskytovatele sady SDK | OAuth hostitele |
   |:--- |:--- |:--- |
   | Azure Active Directory | aad | https://login.microsoftonline.com |
   | Facebook | Facebook | https://www.Facebook.com |
   | Google | Google | https://accounts.Google.com |
   | Microsoft | MicrosoftAccount | https://Login.live.com |
   | Twitter | Služby Twitter. | https://API.Twitter.com |

    Příklad obsah-Security-Policy (implementována pro Azure Active Directory) je následující:

        <meta http-equiv="Content-Security-Policy" content="default-src 'self'
            data: gap: https://login.microsoftonline.com https://yourapp.azurewebsites.net; style-src 'self'">

    Nahraďte `https://login.microsoftonline.com` s hostitelem OAuth hello z hello předcházející tabulce.  Další informace o značka meta hello zásady zabezpečení obsahu najdete v tématu hello [zásady zabezpečení obsahu dokumentaci].

    Někteří poskytovatelé ověřování nevyžadují změny zásad zabezpečení obsahu při použití na příslušné mobilních zařízeních.  Například žádné změny zásad zabezpečení obsahu jsou požadovaná při použití ověřování Google na zařízení s Androidem.

3. Otevřete hello `www/js/index.js` souboru pro úpravy, vyhledejte hello `onDeviceReady()` metoda, a v části Vytvoření klienta hello kód přidejte následující kód hello:

        // Login toohello service
        client.login('SDK_Provider_Name')
            .then(function () {

                // BEGINNING OF ORIGINAL CODE

                // Create a table reference
                todoItemTable = client.getTable('todoitem');

                // Refresh hello todoItems
                refreshDisplay();

                // Wire up hello UI Event Handler for hello Add Item
                $('#add-item').submit(addItemHandler);
                $('#refresh').on('click', refreshDisplay);

                // END OF ORIGINAL CODE

            }, handleError);

    Tento kód nahradí hello existující kód, který vytvoří odkaz na tabulku hello a aktualizuje hello uživatelského rozhraní.

    Metoda login() Hello začíná hello zprostředkovatele ověřování. Metoda login() Hello je asynchronní funkce vracející JavaScript Promise.  Hello zbytek inicializace hello je umístěn uvnitř hello promise odpovědi tak, aby nebude provedena, dokud se nedokončí Metoda login() hello.

4. V hello kód, který jste právě přidali, nahraďte `SDK_Provider_Name` s názvem hello zprostředkovatele přihlášení. Například pro Azure Active Directory, použijte `client.login('aad')`.
5. Spusťte projekt.  Po dokončení inicializace hello projektu aplikace zobrazuje hello OAuth přihlašovací stránku pro hello vybrali zprostředkovatele ověřování.

## <a name="next-steps"></a>Další kroky
* Další informace [o ověřování] službou Azure App Service.
* Pokračovat hello kurzu přidáním [nabízená oznámení] tooyour aplikace Apache Cordova.

Zjistěte, jak toouse hello sady SDK.

* [Apache Cordova SDK]
* [ASP.NET Server SDK]
* [Node.js Server SDK]

<!-- URLs. -->
[Začínáme s Mobile Apps]: app-service-mobile-cordova-get-started.md
[zásady zabezpečení obsahu dokumentaci]: https://cordova.apache.org/docs/en/latest/guide/appdev/whitelist/index.html
[nabízená oznámení]: app-service-mobile-cordova-get-started-push.md
[o ověřování]: app-service-mobile-auth.md
[Apache Cordova SDK]: app-service-mobile-cordova-how-to-use-client-library.md
[ASP.NET Server SDK]: app-service-mobile-dotnet-backend-how-to-use-server-sdk.md
[Node.js Server SDK]: app-service-mobile-node-backend-how-to-use-server-sdk.md
