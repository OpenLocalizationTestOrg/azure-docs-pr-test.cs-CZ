---
title: "aaaAzure AD v2.0 NodeJS AngularJS jednostránkové aplikace Začínáme | Microsoft Docs"
description: "Jak toobuild úhlová JS jednostránkové aplikace s přihlašováním uživatelů pomocí obou Microsoft osobní účty a pracovní nebo školní účty."
services: active-directory
documentationcenter: 
author: navyasric
manager: mbaldwin
editor: 
ms.assetid: d286aa33-8a94-452f-beb7-ddc6c6daa5c8
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: javascript
ms.topic: article
ms.date: 01/23/2017
ms.author: nacanuma
ms.custom: aaddev
ms.openlocfilehash: 1ab450caf08ab05fba140b94b1b8de652e99cbc1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="add-sign-in-tooan-angularjs-single-page-app---nodejs"></a>Přidat přihlašovací tooan AngularJS jednostránkové aplikace – NodeJS
V tomto článku přidáme Přihlaste se pomocí používá technologii Microsoft účty tooan AngularJS aplikace pomocí koncového bodu v2.0 hello Azure Active Directory. koncový bod v2.0 Hello umožňují tooperform jeden integrace ve vaší aplikaci a ověřuje uživatele pomocí osobní i pracovní nebo školní účty.

Tato ukázka je jednoduchou aplikaci seznamu úkolů jednu stránku, která ukládá úkoly do back-end REST API, napsané v NodeJS a zabezpečit pomocí nosných tokenů OAuth z Azure AD.  Hello AngularJS aplikace bude používat naše knihovna JavaScript ověřování s otevřeným zdrojem [adal.js](https://github.com/AzureAD/azure-activedirectory-library-for-js) toohandle hello celý v procesu přihlašování a získávat tokeny pro hello volání rozhraní REST API.  Hello stejného vzoru může být použité tooauthenticate tooother rozhraní REST API, jako je hello [Microsoft Graph](https://graph.microsoft.com) nebo hello rozhraní API Správce Azure Resource Manager.

> [!NOTE]
> Ne všechny scénáře Azure Active Directory a funkce jsou podporovány koncového bodu v2.0 hello.  toodetermine Pokud byste měli používat koncového bodu v2.0 hello, přečtěte si informace o [v2.0 omezení](active-directory-v2-limitations.md).
> 
> 

## <a name="download"></a>Ke stažení
tooget spuštění, budete potřebovat toodownload & instalace [node.js](https://nodejs.org).  Potom můžete klonování nebo [Stáhnout](https://github.com/AzureADQuickStarts/AppModelv2-SinglePageApp-AngularJS-NodeJS/archive/skeleton.zip) kostru aplikace:

```
git clone --branch skeleton https://github.com/AzureADQuickStarts/AppModelv2-SinglePageApp-AngularJS-NodeJS.git
```

kostru aplikace Hello zahrnuje všechny hello často používaný kód pro jednoduchou aplikaci AngularJS, ale chybí všechny součásti související s identity hello.  Pokud nechcete, aby toofollow společně, můžete místo toho klonovat nebo [Stáhnout](https://github.com/AzureADQuickStarts/AppModelv2-SinglePageApp-AngularJS-NodeJS/archive/complete.zip) ukázka hello byla dokončena.

```
git clone https://github.com/AzureADSamples/SinglePageApp-AngularJS-NodeJS.git
```

## <a name="register-an-app"></a>Registrace aplikace
Nejprve vytvořte aplikaci v hello [portálu pro registraci aplikace](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList), nebo postupujte podle těchto [podrobné kroky](active-directory-v2-app-registration.md).  Zkontrolujte, že:

* Přidat hello **webové** platformu pro vaši aplikaci.
* Zadejte správný hello **identifikátor URI pro přesměrování**. Výchozí hodnota Hello Tato ukázka je `http://localhost:8080`.
* Nechte hello **povolit implicitní tok** zaškrtávací políčko povoleno. 

Poznamenejte hello **ID aplikace** přiřazené tooyour aplikace, budete ho potřebovat za chvíli. 

## <a name="install-adaljs"></a>Nainstalujte adal.js
toostart, přejděte tooproject, které jste stáhli a nainstalujte adal.js.  Pokud máte [bower](http://bower.io/) nainstalovaná, právě spuštěním tohoto příkazu.  Pro žádné neshody verze závislosti vyberte právě hello vyšší verze.

```
bower install adal-angular#experimental
```

Alternativně můžete ručně stáhnout [adal.js](https://raw.githubusercontent.com/AzureAD/azure-activedirectory-library-for-js/experimental/dist/adal.min.js) a [adal angular.js](https://raw.githubusercontent.com/AzureAD/azure-activedirectory-library-for-js/experimental/dist/adal-angular.min.js).  Přidat oba soubory toohello `app/lib/adal-angular-experimental/dist` adresáře.

Nyní otevřete projekt hello ve svém oblíbeném textovém editoru a načíst adal.js na konci hello obsahu stránce hello:

```html
<!--index.html-->

...

<script src="App/bower_components/dist/adal.min.js"></script>
<script src="App/bower_components/dist/adal-angular.min.js"></script>

...
```

## <a name="set-up-hello-rest-api"></a>Nastavit hello REST API
Když jste nastavujeme, umožňuje get hello back-end REST API práci.  V příkazovém řádku, nainstalujte všechny potřebné balíčky hello spuštěním (ujistěte se, můžete začít adresář nejvyšší úrovně hello projektu hello):

```
npm install
```

Nyní otevřete `config.js` a nahraďte hello `audience` hodnotu:

```js
exports.creds = {

     // TODO: Replace this value with hello Application ID from hello registration portal
     audience: '<Your-application-id>',

     ...
}
```

Hello REST API použije tento hodnota toovalidate tokeny, které obdrží z hello úhlová aplikace na požadavky AJAX.  Všimněte si, že toto jednoduché rozhraní API REST ukládá data v paměti - takže každý server hello toostop čas dojde ke ztrátě všech vytvořených úkolů.

To je všechno hello čas vytvoříme toospend hovoříte o tom, jak funguje hello REST API.  Myslíte, že volné toopoke v hello kódu, ale pokud chcete, aby toolearn více informací o zabezpečení webového rozhraní API s Azure AD, podívejte se na [v tomto článku](active-directory-v2-devquickstarts-node-api.md). 

## <a name="sign-users-in"></a>Přihlášení uživatelů
Čas toowrite nějaký kód identity.  Možná jste již si všimli, že tento adal.js obsahuje poskytovatele AngularJS, který hraje vhodně s úhlová směrování mechanismy.  Začněte přidáním hello adal modulu toohello aplikace:

```js
// app/scripts/app.js

angular.module('todoApp', ['ngRoute','AdalAngular'])
.config(['$routeProvider','$httpProvider', 'adalAuthenticationServiceProvider',
 function ($routeProvider, $httpProvider, adalProvider) {

...
```

Nyní můžete inicializovat hello `adalProvider` s vaší aplikace ID:

```js
// app/scripts/app.js

...

adalProvider.init({

        // Use this value for hello public instance of Azure AD
        instance: 'https://login.microsoftonline.com/', 

        // hello 'common' endpoint is used for multi-tenant applications like this one
        tenant: 'common',

        // Your application id from hello registration portal
        clientId: '<Your-application-id>',

        // If you're using IE, uncommment this line - hello default HTML5 sessionStorage does not work for localhost.
        //cacheLocation: 'localStorage',

    }, $httpProvider);
```

Skvělé, teď adal.js má všechny informace o hello potřebuje toosecure uživatelům přihlášení a aplikace.  tooforce přihlášení pro daný postup v aplikaci hello, vyžaduje se jeden řádek kódu:

```js
// app/scripts/app.js

...

}).when("/TodoList", {
    controller: "todoListCtrl",
    templateUrl: "/static/views/TodoList.html",
    requireADLogin: true, // Ensures that hello user must be logged in tooaccess hello route
})

...
```

Teď, když uživatel klikne hello `TodoList` odkaz, adal.js se automaticky přesměruje tooAzure AD pro přihlášení v případě potřeby.  Vyvoláním adal.js v řadičích můžete odeslat také explicitně požadavků na přihlášení a odhlášení:

```js
// app/scripts/homeCtrl.js

angular.module('todoApp')
// Load adal.js hello same way for use in controllers and views   
.controller('homeCtrl', ['$scope', 'adalAuthenticationService','$location', function ($scope, adalService, $location) {
    $scope.login = function () {

        // Redirect hello user toosign in
        adalService.login();

    };
    $scope.logout = function () {

        // Redirect hello user toolog out    
        adalService.logOut();

    };
...
```

## <a name="display-user-info"></a>Zobrazit informace o uživateli
Teď, když hello uživatel se přihlásí, pravděpodobně budete potřebovat data ověřování tooaccess hello přihlášeného uživatele ve vaší aplikaci.  Adal.js zpřístupní tyto informace můžete v hello `userInfo` objektu.  tooaccess tento objekt v zobrazení, musíte nejdřív přidat adal.js toohello kořenovém oboru odpovídající řadiče hello:

```js
// app/scripts/userDataCtrl.js

angular.module('todoApp')
// Load ADAL for use in view
.controller('userDataCtrl', ['$scope', 'adalAuthenticationService', function ($scope, adalService) {}]);
```

Potom můžete přímo vyřešit hello `userInfo` objekt ve svém zobrazení: 

```html
<!--app/views/UserData.html-->

...

    <!--Get hello user's profile information from hello ADAL userInfo object-->
    <tr ng-repeat="(key, value) in userInfo.profile">
        <td>{{key}}</td>
        <td>{{value}}</td>
    </tr>
...
```

Můžete taky hello `userInfo` objektu toodetermine Pokud hello uživatel je přihlášený nebo ne.

```html
<!--index.html-->

...

    <!--Use hello ADAL userInfo object tooshow hello right login/logout button-->
    <ul class="nav navbar-nav navbar-right">
        <li><a class="btn btn-link" ng-show="userInfo.isAuthenticated" ng-click="logout()">Logout</a></li>
        <li><a class="btn btn-link" ng-hide="userInfo.isAuthenticated" ng-click="login()">Login</a></li>
    </ul>
...
```

## <a name="call-hello-rest-api"></a>Hello volání rozhraní REST API
Nakonec je čas tooget některé tokeny a volání hello toocreate REST API, číst, aktualizovat a odstraňovat úkoly.  A co s?  Nemáte toodo *co*.  Adal.js se automaticky postará o získávání, ukládání do mezipaměti a aktualizaci tokeny.  Je také postará o připojení těchto tokenů toooutgoing AJAX požadavky, které odesíláte toohello REST API.  

Jak přesně to funguje? Je všechny magic toohello Děkujeme z [AngularJS sběrače](https://docs.angularjs.org/api/ng/service/$http), což umožňuje adal.js tootransform odchozí a příchozí zprávy http.  Kromě toho adal.js předpokládá, že všechny žádosti odeslat toohello stejné doméně jako hello okna měli používat tokeny, které jsou určené pro hello stejným ID aplikace, jako hello aplikace AngularJS.  Z tohoto důvodu jsme použili hello stejným ID aplikace v obou úhlová aplikace hello a hello NodeJS REST API.  Samozřejmě můžete toto chování potlačit a řekněte adal.js tooget tokeny pro jiná rozhraní API REST v případě potřeby - ale pro tento scénář jednoduchého hello provede výchozí hodnoty.

Zde je fragment kódu, který ukazuje, jak je snadné toosend požadavků s nosné tokeny z Azure AD:

```js
// app/scripts/todoListSvc.js

...
return $http.get('/api/tasks');
...
```

Blahopřejeme!  Integrované jednostránkové aplikace Azure AD je nyní dokončen.  Pokračujte, proveďte předních.  Ho můžete ověřovat uživatele, bezpečně volat jeho back-end pomocí OpenID Connect rozhraní REST API a získat základní informace o uživateli hello.  Předinstalované hello podporuje každý uživatel s osobní Account Microsoft nebo pracovní nebo školní účet z Azure AD.  Poskytněte hello aplikaci zkuste to spuštěním:

```
node server.js
```

V prohlížeči přejděte příliš`http://localhost:8080`.  Přihlaste se pomocí osobního účtu Microsoft nebo pracovní nebo školní účet.  Přidejte seznam úkolů uživatele toohello úlohy a odhlaste.  Zkuste podepisování s hello jiný druh účtu. Pokud potřebujete pracovní nebo školní uživatele toocreate klienta Azure AD [zjistěte, jak jeden zde tooget](active-directory-howto-tenant.md) (je to zdarma).

získávání informací o hello toocontinue hello koncového bodu v2.0, přejděte zpět tooour [Příručka vývojáře v2.0](active-directory-appmodel-v2-overview.md).  Další zdroje projděte si:

* [Azure – ukázky z webu GitHub >>](https://github.com/Azure-Samples)
* [Azure AD na přetečení zásobníku >>](http://stackoverflow.com/questions/tagged/azure-active-directory)
* Dokumentace k Azure AD na [Azure.com >>](https://azure.microsoft.com/documentation/services/active-directory/)

## <a name="get-security-updates-for-our-products"></a>Získejte bezpečnostní aktualizace našich produktů
Doporučujeme vám tooget oznámení o bezpečnostních incidentech navštivte stránky [tuto stránku](https://technet.microsoft.com/security/dd252948) a přihlášení k odběru tooSecurity Advisory Alerts.

