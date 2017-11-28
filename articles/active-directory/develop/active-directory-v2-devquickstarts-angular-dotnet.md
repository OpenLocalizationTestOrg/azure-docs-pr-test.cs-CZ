---
title: "aaaAzure AD v2.0 .NET AngularJS jednostránkové aplikace Začínáme | Microsoft Docs"
description: "Jak toobuild úhlová JS jednostránkové aplikace s přihlašováním uživatelů pomocí obou Microsoft osobní účty a pracovní nebo školní účty."
services: active-directory
documentationcenter: 
author: jmprieur
manager: mbaldwin
editor: 
ms.assetid: 6a341781-278f-461b-92ca-7572a06e6852
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: javascript
ms.topic: article
ms.date: 01/23/2017
ms.author: jmprieur
ms.custom: aaddev
ms.openlocfilehash: bd3fc8dce91eb0bedcbfed47a9b3ef52c5568c6a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="add-sign-in-tooan-angularjs-single-page-app---net"></a><span data-ttu-id="bcc02-103">Přidat přihlašovací tooan AngularJS jednostránkové aplikace – rozhraní .NET</span><span class="sxs-lookup"><span data-stu-id="bcc02-103">Add sign-in tooan AngularJS single page app - .NET</span></span>
<span data-ttu-id="bcc02-104">V tomto článku přidáme Přihlaste se pomocí používá technologii Microsoft účty tooan AngularJS aplikace pomocí koncového bodu v2.0 hello Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="bcc02-104">In this article we'll add sign in with Microsoft powered accounts tooan AngularJS app using hello Azure Active Directory v2.0 endpoint.</span></span>  <span data-ttu-id="bcc02-105">koncový bod v2.0 Hello vám umožní tooperform jeden integrace ve vaší aplikaci a ověřuje uživatele pomocí osobní i pracovní nebo školní účty.</span><span class="sxs-lookup"><span data-stu-id="bcc02-105">hello v2.0 endpoint enables you tooperform a single integration in your app and authenticate users with both personal and work/school accounts.</span></span>

<span data-ttu-id="bcc02-106">Tato ukázka je jednoduchou aplikaci seznamu úkolů jednu stránku, která ukládá úkoly do back-end REST API, napsané v rozhraní .NET 4.5 MVC framework hello a zabezpečit pomocí nosných tokenů OAuth z Azure AD.</span><span class="sxs-lookup"><span data-stu-id="bcc02-106">This sample is a simple To-Do List single page app that stores tasks in a backend REST API, written using hello .NET 4.5 MVC framework and secured using OAuth bearer tokens from Azure AD.</span></span>  <span data-ttu-id="bcc02-107">Hello AngularJS aplikace bude používat naše knihovna JavaScript ověřování s otevřeným zdrojem [adal.js](https://github.com/AzureAD/azure-activedirectory-library-for-js) toohandle hello celý v procesu přihlašování a získávat tokeny pro hello volání rozhraní REST API.</span><span class="sxs-lookup"><span data-stu-id="bcc02-107">hello AngularJS app will use our open source JavaScript authentication library [adal.js](https://github.com/AzureAD/azure-activedirectory-library-for-js) toohandle hello entire sign in process and acquire tokens for calling hello REST API.</span></span>  <span data-ttu-id="bcc02-108">Hello stejného vzoru může být použité tooauthenticate tooother rozhraní REST API, jako je hello [Microsoft Graph](https://graph.microsoft.com).</span><span class="sxs-lookup"><span data-stu-id="bcc02-108">hello same pattern can be applied tooauthenticate tooother REST APIs, like hello [Microsoft Graph](https://graph.microsoft.com).</span></span>

> [!NOTE]
> <span data-ttu-id="bcc02-109">Ne všechny scénáře Azure Active Directory a funkce jsou podporovány koncového bodu v2.0 hello.</span><span class="sxs-lookup"><span data-stu-id="bcc02-109">Not all Azure Active Directory scenarios & features are supported by hello v2.0 endpoint.</span></span>  <span data-ttu-id="bcc02-110">toodetermine Pokud byste měli používat koncového bodu v2.0 hello, přečtěte si informace o [v2.0 omezení](active-directory-v2-limitations.md).</span><span class="sxs-lookup"><span data-stu-id="bcc02-110">toodetermine if you should use hello v2.0 endpoint, read about [v2.0 limitations](active-directory-v2-limitations.md).</span></span>
> 
> 

## <a name="download"></a><span data-ttu-id="bcc02-111">Ke stažení</span><span class="sxs-lookup"><span data-stu-id="bcc02-111">Download</span></span>
<span data-ttu-id="bcc02-112">tooget spuštění, budete potřebovat toodownload & instalaci sady Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="bcc02-112">tooget started, you'll need toodownload & install Visual Studio.</span></span>  <span data-ttu-id="bcc02-113">Potom můžete klonování nebo [Stáhnout](https://github.com/AzureADQuickStarts/AppModelv2-SinglePageApp-AngularJS-DotNet/archive/skeleton.zip) kostru aplikace:</span><span class="sxs-lookup"><span data-stu-id="bcc02-113">Then you can clone or [download](https://github.com/AzureADQuickStarts/AppModelv2-SinglePageApp-AngularJS-DotNet/archive/skeleton.zip) a skeleton app:</span></span>

```
git clone --branch skeleton https://github.com/AzureADQuickStarts/AppModelv2-SinglePageApp-AngularJS-DotNet.git
```

<span data-ttu-id="bcc02-114">kostru aplikace Hello zahrnuje všechny hello často používaný kód pro jednoduchou aplikaci AngularJS, ale chybí všechny součásti související s identity hello.</span><span class="sxs-lookup"><span data-stu-id="bcc02-114">hello skeleton app includes all hello boilerplate code for a simple AngularJS app, but is missing all of hello identity-related pieces.</span></span>  <span data-ttu-id="bcc02-115">Pokud nechcete, aby toofollow společně, můžete místo toho klonovat nebo [Stáhnout](https://github.com/AzureADQuickStarts/AppModelv2-SinglePageApp-AngularJS-DotNet/archive/complete.zip) ukázka hello byla dokončena.</span><span class="sxs-lookup"><span data-stu-id="bcc02-115">If you don't want toofollow along, you can instead clone or [download](https://github.com/AzureADQuickStarts/AppModelv2-SinglePageApp-AngularJS-DotNet/archive/complete.zip) hello completed sample.</span></span>

```
git clone https://github.com/AzureADSamples/SinglePageApp-AngularJS-DotNet.git
```

## <a name="register-an-app"></a><span data-ttu-id="bcc02-116">Registrace aplikace</span><span class="sxs-lookup"><span data-stu-id="bcc02-116">Register an app</span></span>
<span data-ttu-id="bcc02-117">Nejprve vytvořte aplikaci v hello [portálu pro registraci aplikace](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList), nebo postupujte podle těchto [podrobné kroky](active-directory-v2-app-registration.md).</span><span class="sxs-lookup"><span data-stu-id="bcc02-117">First, create an app in hello [App Registration Portal](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList), or follow these [detailed steps](active-directory-v2-app-registration.md).</span></span>  <span data-ttu-id="bcc02-118">Zkontrolujte, že:</span><span class="sxs-lookup"><span data-stu-id="bcc02-118">Make sure to:</span></span>

* <span data-ttu-id="bcc02-119">Přidat hello **webové** platformu pro vaši aplikaci.</span><span class="sxs-lookup"><span data-stu-id="bcc02-119">Add hello **Web** platform for your app.</span></span>
* <span data-ttu-id="bcc02-120">Zadejte správný hello **identifikátor URI pro přesměrování**.</span><span class="sxs-lookup"><span data-stu-id="bcc02-120">Enter hello correct **Redirect URI**.</span></span> <span data-ttu-id="bcc02-121">Výchozí hodnota Hello Tato ukázka je `https://localhost:44326/`.</span><span class="sxs-lookup"><span data-stu-id="bcc02-121">hello default for this sample is `https://localhost:44326/`.</span></span>
* <span data-ttu-id="bcc02-122">Nechte hello **povolit implicitní tok** zaškrtávací políčko povoleno.</span><span class="sxs-lookup"><span data-stu-id="bcc02-122">Leave hello **Allow Implicit Flow** checkbox enabled.</span></span> 

<span data-ttu-id="bcc02-123">Poznamenejte hello **ID aplikace** přiřazené tooyour aplikace, budete ho potřebovat za chvíli.</span><span class="sxs-lookup"><span data-stu-id="bcc02-123">Copy down hello **Application ID** that is assigned tooyour app, you'll need it shortly.</span></span> 

## <a name="install-adaljs"></a><span data-ttu-id="bcc02-124">Nainstalujte adal.js</span><span class="sxs-lookup"><span data-stu-id="bcc02-124">Install adal.js</span></span>
<span data-ttu-id="bcc02-125">toostart, přejděte tooproject, které jste stáhli a nainstalujte adal.js.</span><span class="sxs-lookup"><span data-stu-id="bcc02-125">toostart, navigate tooproject you downloaded and install adal.js.</span></span>  <span data-ttu-id="bcc02-126">Pokud máte [bower](http://bower.io/) nainstalovaná, právě spuštěním tohoto příkazu.</span><span class="sxs-lookup"><span data-stu-id="bcc02-126">If you have [bower](http://bower.io/) installed, you can just run this command.</span></span>  <span data-ttu-id="bcc02-127">Pro žádné neshody verze závislosti vyberte právě hello vyšší verze.</span><span class="sxs-lookup"><span data-stu-id="bcc02-127">For any dependency version mismatches, just choose hello higher version.</span></span>

```
bower install adal-angular#experimental
```

<span data-ttu-id="bcc02-128">Alternativně můžete ručně stáhnout [adal.js](https://raw.githubusercontent.com/AzureAD/azure-activedirectory-library-for-js/experimental/dist/adal.min.js) a [adal angular.js](https://raw.githubusercontent.com/AzureAD/azure-activedirectory-library-for-js/experimental/dist/adal-angular.min.js).</span><span class="sxs-lookup"><span data-stu-id="bcc02-128">Alternatively, you can manually download [adal.js](https://raw.githubusercontent.com/AzureAD/azure-activedirectory-library-for-js/experimental/dist/adal.min.js) and [adal-angular.js](https://raw.githubusercontent.com/AzureAD/azure-activedirectory-library-for-js/experimental/dist/adal-angular.min.js).</span></span>  <span data-ttu-id="bcc02-129">Přidat oba soubory toohello `app/lib/adal-angular-experimental/dist` adresář hello `TodoSPA` projektu.</span><span class="sxs-lookup"><span data-stu-id="bcc02-129">Add both files toohello `app/lib/adal-angular-experimental/dist` directory of hello `TodoSPA` project.</span></span>

<span data-ttu-id="bcc02-130">Nyní otevřete hello projektu v sadě Visual Studio a načíst adal.js na konci hello textu hello hlavní stránky:</span><span class="sxs-lookup"><span data-stu-id="bcc02-130">Now open hello project in Visual Studio, and load adal.js at hello end of hello main page's body:</span></span>

```html
<!--index.html-->

...

<script src="App/bower_components/dist/adal.min.js"></script>
<script src="App/bower_components/dist/adal-angular.min.js"></script>

...
```

## <a name="set-up-hello-rest-api"></a><span data-ttu-id="bcc02-131">Nastavit hello REST API</span><span class="sxs-lookup"><span data-stu-id="bcc02-131">Set up hello REST API</span></span>
<span data-ttu-id="bcc02-132">Když jste nastavujeme, můžeme začít pracovat REST API back-end hello.</span><span class="sxs-lookup"><span data-stu-id="bcc02-132">While we're setting things up, let's get hello backend REST API working.</span></span>  <span data-ttu-id="bcc02-133">V kořenovém hello hello projektu, otevřete `web.config` a nahraďte hello `audience` hodnotu.</span><span class="sxs-lookup"><span data-stu-id="bcc02-133">In hello root of hello project, open `web.config` and replace hello `audience` value.</span></span>  <span data-ttu-id="bcc02-134">Hello REST API použije tento hodnota toovalidate tokeny, které obdrží z hello úhlová aplikace na požadavky AJAX.</span><span class="sxs-lookup"><span data-stu-id="bcc02-134">hello REST API will use this value toovalidate tokens it receives from hello Angular app on AJAX requests.</span></span>

```xml
<!--web.config-->

...

    <appSettings>
        <add key="ida:Audience" value="[Your-application-id]" />
    </appSettings>

...
```

<span data-ttu-id="bcc02-135">To je všechno hello čas vytvoříme toospend hovoříte o tom, jak funguje hello REST API.</span><span class="sxs-lookup"><span data-stu-id="bcc02-135">That's all hello time we're going toospend discussing how hello REST API works.</span></span>  <span data-ttu-id="bcc02-136">Myslíte, že volné toopoke v hello kódu, ale pokud chcete, aby toolearn více informací o zabezpečení webového rozhraní API s Azure AD, podívejte se na [v tomto článku](active-directory-v2-devquickstarts-dotnet-api.md).</span><span class="sxs-lookup"><span data-stu-id="bcc02-136">Feel free toopoke around in hello code, but if you want toolearn more about securing web APIs with Azure AD, check out [this article](active-directory-v2-devquickstarts-dotnet-api.md).</span></span> 

## <a name="sign-users-in"></a><span data-ttu-id="bcc02-137">Přihlášení uživatelů</span><span class="sxs-lookup"><span data-stu-id="bcc02-137">Sign users in</span></span>
<span data-ttu-id="bcc02-138">Čas toowrite nějaký kód identity.</span><span class="sxs-lookup"><span data-stu-id="bcc02-138">Time toowrite some identity code.</span></span>  <span data-ttu-id="bcc02-139">Možná jste již si všimli, že tento adal.js obsahuje poskytovatele AngularJS, který hraje vhodně s úhlová směrování mechanismy.</span><span class="sxs-lookup"><span data-stu-id="bcc02-139">You might have already noticed that adal.js contains an AngularJS provider, which plays nicely with Angular routing mechanisms.</span></span>  <span data-ttu-id="bcc02-140">Začněte přidáním hello adal modulu toohello aplikace:</span><span class="sxs-lookup"><span data-stu-id="bcc02-140">Start by adding hello adal module toohello app:</span></span>

```js
// app/scripts/app.js

angular.module('todoApp', ['ngRoute','AdalAngular'])
.config(['$routeProvider','$httpProvider', 'adalAuthenticationServiceProvider',
 function ($routeProvider, $httpProvider, adalProvider) {

...
```

<span data-ttu-id="bcc02-141">Nyní můžete inicializovat hello `adalProvider` s vaší aplikace ID:</span><span class="sxs-lookup"><span data-stu-id="bcc02-141">You can now initialize hello `adalProvider` with your Application ID:</span></span>

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

<span data-ttu-id="bcc02-142">Skvělé, teď adal.js má všechny informace o hello potřebuje toosecure uživatelům přihlášení a aplikace.</span><span class="sxs-lookup"><span data-stu-id="bcc02-142">Great, now adal.js has all hello information it needs toosecure your app and sign users in.</span></span>  <span data-ttu-id="bcc02-143">tooforce přihlášení pro daný postup v aplikaci hello, vyžaduje se jeden řádek kódu:</span><span class="sxs-lookup"><span data-stu-id="bcc02-143">tooforce sign in for a particular route in hello app, all it takes is one line of code:</span></span>

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

<span data-ttu-id="bcc02-144">Teď, když uživatel klikne hello `TodoList` odkaz, adal.js se automaticky přesměruje tooAzure AD pro přihlášení v případě potřeby.</span><span class="sxs-lookup"><span data-stu-id="bcc02-144">Now when a user clicks hello `TodoList` link, adal.js will automatically redirect tooAzure AD for sign-in if necessary.</span></span>  <span data-ttu-id="bcc02-145">Vyvoláním adal.js v řadičích můžete odeslat také explicitně požadavků na přihlášení a odhlášení:</span><span class="sxs-lookup"><span data-stu-id="bcc02-145">You can also explicitly send sign-in and sign-out requests by invoking adal.js in your controllers:</span></span>

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

## <a name="display-user-info"></a><span data-ttu-id="bcc02-146">Zobrazit informace o uživateli</span><span class="sxs-lookup"><span data-stu-id="bcc02-146">Display user info</span></span>
<span data-ttu-id="bcc02-147">Teď, když hello uživatel se přihlásí, pravděpodobně budete potřebovat data ověřování tooaccess hello přihlášeného uživatele ve vaší aplikaci.</span><span class="sxs-lookup"><span data-stu-id="bcc02-147">Now that hello user is signed in, you'll probably need tooaccess hello signed-in user's authentication data in your application.</span></span>  <span data-ttu-id="bcc02-148">Adal.js zpřístupní tyto informace můžete v hello `userInfo` objektu.</span><span class="sxs-lookup"><span data-stu-id="bcc02-148">Adal.js exposes this information for you in hello `userInfo` object.</span></span>  <span data-ttu-id="bcc02-149">tooaccess tento objekt v zobrazení, musíte nejdřív přidat adal.js toohello kořenovém oboru odpovídající řadiče hello:</span><span class="sxs-lookup"><span data-stu-id="bcc02-149">tooaccess this object in a view, first add adal.js toohello root scope of hello corresponding controller:</span></span>

```js
// app/scripts/userDataCtrl.js

angular.module('todoApp')
// Load ADAL for use in view
.controller('userDataCtrl', ['$scope', 'adalAuthenticationService', function ($scope, adalService) {}]);
```

<span data-ttu-id="bcc02-150">Potom můžete přímo vyřešit hello `userInfo` objekt ve svém zobrazení:</span><span class="sxs-lookup"><span data-stu-id="bcc02-150">Then you can directly address hello `userInfo` object in your view:</span></span> 

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

<span data-ttu-id="bcc02-151">Můžete taky hello `userInfo` objektu toodetermine Pokud hello uživatel je přihlášený nebo ne.</span><span class="sxs-lookup"><span data-stu-id="bcc02-151">You can also use hello `userInfo` object toodetermine if hello user is signed in or not.</span></span>

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

## <a name="call-hello-rest-api"></a><span data-ttu-id="bcc02-152">Hello volání rozhraní REST API</span><span class="sxs-lookup"><span data-stu-id="bcc02-152">Call hello REST API</span></span>
<span data-ttu-id="bcc02-153">Nakonec je čas tooget některé tokeny a volání hello toocreate REST API, číst, aktualizovat a odstraňovat úkoly.</span><span class="sxs-lookup"><span data-stu-id="bcc02-153">Finally, it's time tooget some tokens and call hello REST API toocreate, read, update, and delete tasks.</span></span>  <span data-ttu-id="bcc02-154">A co s?</span><span class="sxs-lookup"><span data-stu-id="bcc02-154">Well guess what?</span></span>  <span data-ttu-id="bcc02-155">Nemáte toodo *co*.</span><span class="sxs-lookup"><span data-stu-id="bcc02-155">You don't have toodo *a thing*.</span></span>  <span data-ttu-id="bcc02-156">Adal.js se automaticky postará o získávání, ukládání do mezipaměti a aktualizaci tokeny.</span><span class="sxs-lookup"><span data-stu-id="bcc02-156">Adal.js will automatically take care of getting, caching, and refreshing tokens.</span></span>  <span data-ttu-id="bcc02-157">Je také postará o připojení těchto tokenů toooutgoing AJAX požadavky, které odesíláte toohello REST API.</span><span class="sxs-lookup"><span data-stu-id="bcc02-157">It will also take care of attaching those tokens toooutgoing AJAX requests that you send toohello REST API.</span></span>  

<span data-ttu-id="bcc02-158">Jak přesně to funguje?</span><span class="sxs-lookup"><span data-stu-id="bcc02-158">How exactly does this work?</span></span> <span data-ttu-id="bcc02-159">Je všechny magic toohello Děkujeme z [AngularJS sběrače](https://docs.angularjs.org/api/ng/service/$http), což umožňuje adal.js tootransform odchozí a příchozí zprávy http.</span><span class="sxs-lookup"><span data-stu-id="bcc02-159">It's all thanks toohello magic of [AngularJS interceptors](https://docs.angularjs.org/api/ng/service/$http), which allows adal.js tootransform outgoing and incoming http messages.</span></span>  <span data-ttu-id="bcc02-160">Kromě toho adal.js předpokládá, že všechny žádosti odeslat toohello stejné doméně jako hello okna měli používat tokeny, které jsou určené pro hello stejným ID aplikace, jako hello aplikace AngularJS.</span><span class="sxs-lookup"><span data-stu-id="bcc02-160">Furthermore, adal.js assumes that any requests send toohello same domain as hello window should use tokens intended for hello same Application ID as hello AngularJS app.</span></span>  <span data-ttu-id="bcc02-161">Z tohoto důvodu jsme použili hello stejným ID aplikace v obou úhlová aplikace hello a hello NodeJS REST API.</span><span class="sxs-lookup"><span data-stu-id="bcc02-161">This is why we used hello same Application ID in both hello Angular app and in hello NodeJS REST API.</span></span>  <span data-ttu-id="bcc02-162">Samozřejmě můžete toto chování potlačit a řekněte adal.js tooget tokeny pro jiná rozhraní API REST v případě potřeby - ale pro tento scénář jednoduchého hello provede výchozí hodnoty.</span><span class="sxs-lookup"><span data-stu-id="bcc02-162">Of course, you can override this behavior and tell adal.js tooget tokens for other REST APIs if necessary - but for this simple scenario hello defaults will do.</span></span>

<span data-ttu-id="bcc02-163">Zde je fragment kódu, který ukazuje, jak je snadné toosend požadavků s nosné tokeny z Azure AD:</span><span class="sxs-lookup"><span data-stu-id="bcc02-163">Here's a snippet that shows how easy it is toosend requests with bearer tokens from Azure AD:</span></span>

```js
// app/scripts/todoListSvc.js

...
return $http.get('/api/tasks');
...
```

<span data-ttu-id="bcc02-164">Blahopřejeme!</span><span class="sxs-lookup"><span data-stu-id="bcc02-164">Congratulations!</span></span>  <span data-ttu-id="bcc02-165">Integrované jednostránkové aplikace Azure AD je nyní dokončen.</span><span class="sxs-lookup"><span data-stu-id="bcc02-165">Your Azure AD integrated single page app is now complete.</span></span>  <span data-ttu-id="bcc02-166">Pokračujte, proveďte předních.</span><span class="sxs-lookup"><span data-stu-id="bcc02-166">Go ahead, take a bow.</span></span>  <span data-ttu-id="bcc02-167">Ho můžete ověřovat uživatele, bezpečně volat jeho back-end pomocí OpenID Connect rozhraní REST API a získat základní informace o uživateli hello.</span><span class="sxs-lookup"><span data-stu-id="bcc02-167">It can authenticate users, securely call its backend REST API using OpenID Connect, and get basic information about hello user.</span></span>  <span data-ttu-id="bcc02-168">Předinstalované hello podporuje každý uživatel s osobní Account Microsoft nebo pracovní nebo školní účet z Azure AD.</span><span class="sxs-lookup"><span data-stu-id="bcc02-168">Out of hello box, it supports any user with a personal Microsoft Account or a work/school account from Azure AD.</span></span>  <span data-ttu-id="bcc02-169">Spuštění aplikace hello a v prohlížeči přejděte příliš`https://localhost:44326/`.</span><span class="sxs-lookup"><span data-stu-id="bcc02-169">Run hello app, and in a browser navigate too`https://localhost:44326/`.</span></span>  <span data-ttu-id="bcc02-170">Přihlaste se pomocí osobního účtu Microsoft nebo pracovní nebo školní účet.</span><span class="sxs-lookup"><span data-stu-id="bcc02-170">Sign in using either a personal Microsoft account or a work/school account.</span></span>  <span data-ttu-id="bcc02-171">Přidejte seznam úkolů uživatele toohello úlohy a odhlaste.  Zkuste podepisování s hello jiný druh účtu.</span><span class="sxs-lookup"><span data-stu-id="bcc02-171">Add tasks toohello user's to-do list, and sign out.  Try signing in with hello other type of account.</span></span> <span data-ttu-id="bcc02-172">Pokud potřebujete pracovní nebo školní uživatele toocreate klienta Azure AD [zjistěte, jak jeden zde tooget](active-directory-howto-tenant.md) (je to zdarma).</span><span class="sxs-lookup"><span data-stu-id="bcc02-172">If you need an Azure AD tenant toocreate work/school users, [learn how tooget one here](active-directory-howto-tenant.md) (it's free).</span></span>

<span data-ttu-id="bcc02-173">získávání informací o koncového bodu v2.0 hello, head back tooour toocontinue [Příručka vývojáře v2.0](active-directory-appmodel-v2-overview.md).</span><span class="sxs-lookup"><span data-stu-id="bcc02-173">toocontinue learning about hello v2.0 endpoint, head back tooour [v2.0 developer guide](active-directory-appmodel-v2-overview.md).</span></span>  <span data-ttu-id="bcc02-174">Další zdroje projděte si:</span><span class="sxs-lookup"><span data-stu-id="bcc02-174">For additional resources, check out:</span></span>

* [<span data-ttu-id="bcc02-175">Azure – ukázky z webu GitHub >></span><span class="sxs-lookup"><span data-stu-id="bcc02-175">Azure-Samples on GitHub >></span></span>](https://github.com/Azure-Samples)
* [<span data-ttu-id="bcc02-176">Azure AD na přetečení zásobníku >></span><span class="sxs-lookup"><span data-stu-id="bcc02-176">Azure AD on Stack Overflow >></span></span>](http://stackoverflow.com/questions/tagged/azure-active-directory)
* <span data-ttu-id="bcc02-177">Dokumentace k Azure AD na [Azure.com >>](https://azure.microsoft.com/documentation/services/active-directory/)</span><span class="sxs-lookup"><span data-stu-id="bcc02-177">Azure AD documentation on [Azure.com >>](https://azure.microsoft.com/documentation/services/active-directory/)</span></span>

## <a name="get-security-updates-for-our-products"></a><span data-ttu-id="bcc02-178">Získejte bezpečnostní aktualizace našich produktů</span><span class="sxs-lookup"><span data-stu-id="bcc02-178">Get security updates for our products</span></span>
<span data-ttu-id="bcc02-179">Doporučujeme vám tooget oznámení o bezpečnostních incidentech navštivte stránky [tuto stránku](https://technet.microsoft.com/security/dd252948) a přihlášení k odběru tooSecurity Advisory Alerts.</span><span class="sxs-lookup"><span data-stu-id="bcc02-179">We encourage you tooget notifications of when security incidents occur by visiting [this page](https://technet.microsoft.com/security/dd252948) and subscribing tooSecurity Advisory Alerts.</span></span>

