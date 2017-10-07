---
title: "aaaAzure AD AngularJS Začínáme | Microsoft Docs"
description: "Jak toobuild jednostránková aplikace AngularJS, se integruje se službou Azure AD pro přihlášení a zavolá rozhraní API Azure AD chráněné pomocí OAuth."
services: active-directory
documentationcenter: 
author: jmprieur
manager: mbaldwin
editor: 
ms.assetid: f2991054-8146-4718-a5f7-59b892230ad7
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: javascript
ms.topic: article
ms.date: 01/07/2017
ms.author: jmprieur
ms.custom: aaddev
ms.openlocfilehash: eca5e1c9662186dfae4f96ca3041f9350583cf79
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="help-secure-angularjs-single-page-apps-by-using-azure-ad"></a>Pomoc se zabezpečením AngularJS jednostránkové aplikace pomocí Azure AD

[!INCLUDE [active-directory-devguide](../../../includes/active-directory-devguide.md)]

Azure Active Directory (Azure AD) umožňuje jednoduchá a přímočará pro tooadd přihlášení, odhlášení a volání rozhraní API zabezpečené OAuth tooyour jednostránkové aplikace.  To umožňuje uživatelům tooauthenticate aplikace pomocí jejich účtů systému Windows Server Active Directory a využívat žádné webové rozhraní API, které Azure AD pomáhá chránit, jako je například hello rozhraní API Office 365 nebo hello rozhraní API služby Azure.

Pro aplikace JavaScript spuštěnému v prohlížeči, Azure AD poskytuje hello Active Directory Authentication Library (ADAL), nebo adal.js. jediný účel adal.js Hello je toomake ho snadno pro vaše aplikace tooget přístupových tokenů. toodemonstrate, jak lze snadno, je, zde jsme budete vytvářet AngularJS tooDo aplikaci seznamu který:

* Hello uživatel přihlásí v aplikaci toohello pomocí Azure AD jako zprostředkovatele identity hello.

* Zobrazí některé informace o uživateli hello.
* Volání bezpečně hello tooDo aplikace API seznamu pomocí nosných tokenů z Azure AD.
* Přihlásí hello uživatele mimo aplikaci hello.

toobuild hello kompletní, funkční aplikaci, musíte:

1. Registrovat aplikaci s Azure AD.
2. ADAL nainstalujte a nakonfigurujte hello jednostránkové aplikace.
3. Použití ADAL toohelp zabezpečené stránky v aplikaci jednostránkové hello.

spuštění, tooget [stáhnout kostru aplikace hello](https://github.com/AzureADQuickStarts/SinglePageApp-AngularJS-DotNet/archive/skeleton.zip) nebo [stažení ukázky hello Dokončit](https://github.com/AzureADQuickStarts/SinglePageApp-AngularJS-DotNet/archive/complete.zip). Musíte taky klient služby Azure AD, ve kterém můžete vytvořit uživatele a zaregistrovat aplikaci. Pokud ještě nemáte klienta, [zjistěte, jak tooget jeden](active-directory-howto-tenant.md).

## <a name="step-1-register-hello-directorysearcher-application"></a>Krok 1: Registrace aplikace DirectorySearcher hello
tooenable tooauthenticate uživatelů aplikace a get tokeny, musíte nejprve tooregister ji ve službě Azure AD klienta:

1. Přihlaste se toohello [portál Azure](https://portal.azure.com).
2. Pokud jste přihlášeni toomultiple adresáře, musíte tooensure prohlížíte hello správném adresáři. toodo tedy na horním panelu hello, klikněte na váš účet. V části hello **Directory** vyberte místo, kam chcete tooregister klienta hello Azure AD vaší aplikace.
3. Klikněte na tlačítko **více služeb** v levém podokně text hello a potom vyberte **Azure Active Directory**.
4. Klikněte na tlačítko **registrace aplikace**a potom vyberte **přidat**.
5. Postupujte podle pokynů hello a vytvořit novou webovou aplikaci nebo webové rozhraní API:
  * **Název** popisuje toousers vaší aplikace.
  * **Identifikátor Uri pro přesměrování** je hello umístění toowhich Azure AD vrátí tokeny. Hello výchozí umístění pro tato ukázka je `https://localhost:44326/`.
6. Po dokončení registrace přiřadí Azure AD aplikace jedinečné ID tooyour aplikace.  Tuto hodnotu budete potřebovat v dalších částech hello, takže zkopírujte jej z karty aplikace hello.
7. Adal.js používá toocommunicate implicitního toku OAuth hello s Azure AD. Implicitní tok hello musíte povolit pro aplikace:
  1. Klikněte na tlačítko aplikace hello a vyberte **Manifest** tooopen hello vložené manifestu editor.
  2. Vyhledejte hello `oauth2AllowImplicitFlow` vlastnost. Jeho hodnotu nastavte příliš`true`.
  3. Klikněte na tlačítko **Uložit** toosave hello manifestu.
8. Udělení oprávnění vašeho klienta pro vaši aplikaci. Přejděte příliš**nastavení** > **vlastnosti** > **požadovaných oprávnění**a klikněte na tlačítko hello **udělit oprávnění**tlačítko na horním panelu hello. Klikněte na tlačítko **Ano** tooconfirm.

## <a name="step-2-install-adal-and-configure-hello-single-page-app"></a>Krok 2: Instalace ADAL a konfigurace hello jednostránkové aplikace
Teď, když máte aplikaci ve službě Azure AD, můžete nainstalovat adal.js a zadejte kód, týkající se identity.

### <a name="configure-hello-javascript-client"></a>Konfigurace klienta JavaScript hello
Začněte tím, že přidání adal.js toohello TodoSPA projektu pomocí hello Konzola správce balíčků:
  1. Stáhněte si [adal.js](https://raw.githubusercontent.com/AzureAD/azure-activedirectory-library-for-js/master/lib/adal.js) a přidejte ji toohello `App/Scripts/` adresáři projektu.
  2. Stáhněte si [adal angular.js](https://raw.githubusercontent.com/AzureAD/azure-activedirectory-library-for-js/master/lib/adal-angular.js) a přidejte ji toohello `App/Scripts/` adresáři projektu.
  3. Načíst každý skript hello konce hello `</body>` v `index.html`:

    ```js
    ...
    <script src="App/Scripts/adal.js"></script>
    <script src="App/Scripts/adal-angular.js"></script>
    ...
    ```

### <a name="configure-hello-back-end-server"></a>Konfigurace back-end server hello
Hello back-end pro hello jednostránkové aplikace tooDo back endové rozhraní API seznamu tooaccept tokeny z prohlížeče hello, musí konfigurace informace o registraci aplikace hello. V projektu TodoSPA hello otevřete `web.config`. Nahraďte hodnoty hello hello elementů v hello `<appSettings>` hello části tooreflect hello hodnoty, které jste použili v portálu Azure. Váš kód bude odkazovat tyto hodnoty vždy, když ho využívá ADAL.
  * `ida:Tenant`je hello domény klienta služby Azure AD – například contoso.onmicrosoft.com.
  * `ida:Audience`je hello ID klienta aplikace, který jste zkopírovali z portálu hello.

## <a name="step-3-use-adal-toohelp-secure-pages-in-hello-single-page-app"></a>Krok 3: Použití ADAL toohelp zabezpečené stránky v hello jednostránkové aplikace
Adal.js se integruje s AngularJS trasy a zprostředkovatele protokolu HTTP, můžete pomoct zabezpečené jednotlivých zobrazení v jednostránkové aplikace.

1. V `App/Scripts/app.js`, převeďte v modulu adal.js hello:

    ```js
    angular.module('todoApp', ['ngRoute','AdalAngular'])
    .config(['$routeProvider','$httpProvider', 'adalAuthenticationServiceProvider',
     function ($routeProvider, $httpProvider, adalProvider) {
    ...
    ```
2. Inicializace `adalProvider` také pomocí hodnoty konfigurace hello registrace vaší aplikace v `App/Scripts/app.js`:

    ```js
    adalProvider.init(
      {
          instance: 'https://login.microsoftonline.com/',
          tenant: 'Enter your tenant name here e.g. contoso.onmicrosoft.com',
          clientId: 'Enter your client ID here e.g. e9a5a8b6-8af7-4719-9821-0deef255f68e',
          extraQueryParameter: 'nux=1',
          //cacheLocation: 'localStorage', // enable this for IE, as sessionStorage does not work for localhost.
      },
      $httpProvider
    );
    ```
3. Pomůže zabezpečené hello `TodoList` zobrazení v hello aplikace pomocí pouze jeden řádek kódu: `requireADLogin`.

    ```js
    ...
    }).when("/TodoList", {
            controller: "todoListCtrl",
            templateUrl: "/App/Views/TodoList.html",
            requireADLogin: true,
    ...
    ```

## <a name="summary"></a>Souhrn
Nyní máte zabezpečené aplikaci jednostránkové, která může přihlásit uživatele a vydat chráněný token nosiče požadavky tooits back endové rozhraní API. Když uživatel klikne hello **TodoList** odkaz, adal.js se automaticky přesměruje tooAzure AD pro přihlášení v případě potřeby. Kromě toho adal.js automaticky připojí přístupu k tokenu tooany, které požadavky Ajax odeslaný toohello aplikace back-end.  

Hello předchozí kroky jsou hello úplné minimální potřebné toobuild jednostránkové aplikace s použitím adal.js. Ale několik dalších funkcí jsou užitečné při jednostránkové aplikace:

* tooexplicitly zasílání požadavků na přihlášení a odhlášení, funkce můžete definovat v řadičích, které vyvolají adal.js.  V `App/Scripts/homeCtrl.js`:

    ```js
    ...
    $scope.login = function () {
        adalService.login();
    };
    $scope.logout = function () {
        adalService.logOut();
    };
    ...
    ```
* Můžete chtít toopresent informace o uživateli v uživatelském rozhraní aplikace hello. Hello ADAL služby již byla přidána toohello `userDataCtrl` řadič, takže přistupujete hello `userInfo` objekt v hello přidružené zobrazení, `App/Views/UserData.html`:

    ```js
    <p>{{userInfo.userName}}</p>
    <p>aud:{{userInfo.profile.aud}}</p>
    <p>iss:{{userInfo.profile.iss}}</p>
    ...
    ```

* Existuje mnoho scénářů, ve kterých budete muset tooknow Pokud hello uživatel je přihlášený nebo ne. Můžete taky hello `userInfo` objektu toogather tyto informace.  Například v `index.html`, můžete zobrazit buď hello **přihlášení** nebo **odhlášení** tlačítko na základě ověření stavu:

    ```js
    <li><a class="btn btn-link" ng-show="userInfo.isAuthenticated" ng-click="logout()">Logout</a></li>
    <li><a class="btn btn-link" ng-hide=" userInfo.isAuthenticated" ng-click="login()">Login</a></li>
    ```

Jednostránkové aplikace Azure integrované s Active Directory můžete ověřovat uživatele, bezpečně volat jeho back-end pomocí standardu OAuth 2.0 a získat základní informace o uživateli hello. Pokud jste to ještě neudělali, nyní je čas toopopulate hello vašeho klienta s některými uživateli. Spuštění tooDo seznamu jednostránkové aplikace a přihlaste se pomocí jeden z těchto uživatelů. Přidejte uživatele toohello úlohy na seznam úkolů, odhlásit se a přihlaste se zpět.

Adal.js umožňuje snadno tooincorporate běžné funkce identity do aplikace. Se stará o všechny pracovní dirty hello za vás: Správa mezipaměti, podpora protokolu OAuth, prezentuje hello uživatele přihlásit uživatelského rozhraní, aktualizace vypršení platnosti tokenů a další.

Pro srovnání je k dispozici v hello dokončit ukázka (bez vašich hodnot nastavení) [Githubu](https://github.com/AzureADQuickStarts/SinglePageApp-AngularJS-DotNet/archive/complete.zip).

## <a name="next-steps"></a>Další kroky
Nyní se můžete přesunout na tooadditional scénáře. Můžete chtít tootry: [volání CORS webového rozhraní API z jednostránkové aplikace](https://github.com/AzureAdSamples/SinglePageApp-WebAPI-AngularJS-DotNet).

[!INCLUDE [active-directory-devquickstarts-additional-resources](../../../includes/active-directory-devquickstarts-additional-resources.md)]
