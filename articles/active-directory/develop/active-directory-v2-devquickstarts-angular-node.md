---
title: "Služba Azure AD v2.0 NodeJS AngularJS jednostránkové aplikace Začínáme | Microsoft Docs"
description: "Jak sestavit úhlová JS jednostránkové aplikace s přihlašováním uživatelů s i osobní účty Microsoft a pracovní nebo školní účty."
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
ms.openlocfilehash: 0e90171afd9c4c782fbb18375ab2d147497ef442
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="add-sign-in-to-an-angularjs-single-page-app---nodejs"></a><span data-ttu-id="a3cc7-103">Přidejte přihlášení do AngularJS jednostránkové aplikace - NodeJS</span><span class="sxs-lookup"><span data-stu-id="a3cc7-103">Add sign-in to an AngularJS single page app - NodeJS</span></span>
<span data-ttu-id="a3cc7-104">V tomto článku přidáme přihlašují účty Microsoft používá technologii aplikace AngularJS pomocí koncového bodu v2.0 Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="a3cc7-104">In this article we'll add sign in with Microsoft powered accounts to an AngularJS app using the Azure Active Directory v2.0 endpoint.</span></span> <span data-ttu-id="a3cc7-105">koncový bod v2.0 umožňují provádět jeden integrace ve vaší aplikaci a ověřuje uživatele pomocí osobní i pracovní nebo školní účty.</span><span class="sxs-lookup"><span data-stu-id="a3cc7-105">the v2.0 endpoint enable you to perform a single integration in your app and authenticate users with both personal and work/school accounts.</span></span>

<span data-ttu-id="a3cc7-106">Tato ukázka je jednoduchou aplikaci seznamu úkolů jednu stránku, která ukládá úkoly do back-end REST API, napsané v NodeJS a zabezpečit pomocí nosných tokenů OAuth z Azure AD.</span><span class="sxs-lookup"><span data-stu-id="a3cc7-106">This sample is a simple To-Do List single page app that stores tasks in a backend REST API, written in NodeJS and secured using OAuth bearer tokens from Azure AD.</span></span>  <span data-ttu-id="a3cc7-107">AngularJS aplikace bude používat naše knihovna JavaScript ověřování s otevřeným zdrojem [adal.js](https://github.com/AzureAD/azure-activedirectory-library-for-js) ke zpracování celý proces přihlášení a získávat tokeny pro volání rozhraní REST API.</span><span class="sxs-lookup"><span data-stu-id="a3cc7-107">The AngularJS app will use our open source JavaScript authentication library [adal.js](https://github.com/AzureAD/azure-activedirectory-library-for-js) to handle the entire sign in process and acquire tokens for calling the REST API.</span></span>  <span data-ttu-id="a3cc7-108">Stejného vzoru lze použít k ověřování pro jiná rozhraní API REST, jako je třeba [Microsoft Graph](https://graph.microsoft.com) nebo rozhraní API Azure Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="a3cc7-108">The same pattern can be applied to authenticate to other REST APIs, like the [Microsoft Graph](https://graph.microsoft.com) or the Azure Resource Manager APIs.</span></span>

> [!NOTE]
> <span data-ttu-id="a3cc7-109">Ne všechny scénáře Azure Active Directory a funkce jsou podporovány koncového bodu v2.0.</span><span class="sxs-lookup"><span data-stu-id="a3cc7-109">Not all Azure Active Directory scenarios & features are supported by the v2.0 endpoint.</span></span>  <span data-ttu-id="a3cc7-110">Pokud chcete zjistit, pokud byste měli používat koncový bod v2.0, přečtěte si informace o [v2.0 omezení](active-directory-v2-limitations.md).</span><span class="sxs-lookup"><span data-stu-id="a3cc7-110">To determine if you should use the v2.0 endpoint, read about [v2.0 limitations](active-directory-v2-limitations.md).</span></span>
> 
> 

## <a name="download"></a><span data-ttu-id="a3cc7-111">Ke stažení</span><span class="sxs-lookup"><span data-stu-id="a3cc7-111">Download</span></span>
<span data-ttu-id="a3cc7-112">Abyste mohli začít, budete muset stáhnout a nainstalovat [node.js](https://nodejs.org).</span><span class="sxs-lookup"><span data-stu-id="a3cc7-112">To get started, you'll need to download & install [node.js](https://nodejs.org).</span></span>  <span data-ttu-id="a3cc7-113">Potom můžete klonování nebo [Stáhnout](https://github.com/AzureADQuickStarts/AppModelv2-SinglePageApp-AngularJS-NodeJS/archive/skeleton.zip) kostru aplikace:</span><span class="sxs-lookup"><span data-stu-id="a3cc7-113">Then you can clone or [download](https://github.com/AzureADQuickStarts/AppModelv2-SinglePageApp-AngularJS-NodeJS/archive/skeleton.zip) a skeleton app:</span></span>

```
git clone --branch skeleton https://github.com/AzureADQuickStarts/AppModelv2-SinglePageApp-AngularJS-NodeJS.git
```

<span data-ttu-id="a3cc7-114">Kostru aplikace zahrnuje všechny standardní kód pro jednoduchou aplikaci AngularJS, ale všechny součásti související s identity chybí.</span><span class="sxs-lookup"><span data-stu-id="a3cc7-114">The skeleton app includes all the boilerplate code for a simple AngularJS app, but is missing all of the identity-related pieces.</span></span>  <span data-ttu-id="a3cc7-115">Pokud nechcete, aby se podle nich zorientujete, můžete místo toho klonovat nebo [Stáhnout](https://github.com/AzureADQuickStarts/AppModelv2-SinglePageApp-AngularJS-NodeJS/archive/complete.zip) je hotová ukázka.</span><span class="sxs-lookup"><span data-stu-id="a3cc7-115">If you don't want to follow along, you can instead clone or [download](https://github.com/AzureADQuickStarts/AppModelv2-SinglePageApp-AngularJS-NodeJS/archive/complete.zip) the completed sample.</span></span>

```
git clone https://github.com/AzureADSamples/SinglePageApp-AngularJS-NodeJS.git
```

## <a name="register-an-app"></a><span data-ttu-id="a3cc7-116">Registrace aplikace</span><span class="sxs-lookup"><span data-stu-id="a3cc7-116">Register an app</span></span>
<span data-ttu-id="a3cc7-117">Nejprve vytvořte aplikaci [portálu pro registraci aplikace](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList), nebo postupujte podle těchto [podrobné kroky](active-directory-v2-app-registration.md).</span><span class="sxs-lookup"><span data-stu-id="a3cc7-117">First, create an app in the [App Registration Portal](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList), or follow these [detailed steps](active-directory-v2-app-registration.md).</span></span>  <span data-ttu-id="a3cc7-118">Zkontrolujte, že:</span><span class="sxs-lookup"><span data-stu-id="a3cc7-118">Make sure to:</span></span>

* <span data-ttu-id="a3cc7-119">Přidat **webové** platformu pro vaši aplikaci.</span><span class="sxs-lookup"><span data-stu-id="a3cc7-119">Add the **Web** platform for your app.</span></span>
* <span data-ttu-id="a3cc7-120">Zadejte správný **identifikátor URI pro přesměrování**.</span><span class="sxs-lookup"><span data-stu-id="a3cc7-120">Enter the correct **Redirect URI**.</span></span> <span data-ttu-id="a3cc7-121">Výchozí hodnota pro tato ukázka je `http://localhost:8080`.</span><span class="sxs-lookup"><span data-stu-id="a3cc7-121">The default for this sample is `http://localhost:8080`.</span></span>
* <span data-ttu-id="a3cc7-122">Ponechte **povolit implicitní tok** zaškrtávací políčko povoleno.</span><span class="sxs-lookup"><span data-stu-id="a3cc7-122">Leave the **Allow Implicit Flow** checkbox enabled.</span></span> 

<span data-ttu-id="a3cc7-123">Zkopírování **ID aplikace** přiřazené do vaší aplikace, budete ho potřebovat za chvíli.</span><span class="sxs-lookup"><span data-stu-id="a3cc7-123">Copy down the **Application ID** that is assigned to your app, you'll need it shortly.</span></span> 

## <a name="install-adaljs"></a><span data-ttu-id="a3cc7-124">Nainstalujte adal.js</span><span class="sxs-lookup"><span data-stu-id="a3cc7-124">Install adal.js</span></span>
<span data-ttu-id="a3cc7-125">Pokud chcete spustit, přejděte do projektu můžete stáhnout a nainstalovat adal.js.</span><span class="sxs-lookup"><span data-stu-id="a3cc7-125">To start, navigate to project you downloaded and install adal.js.</span></span>  <span data-ttu-id="a3cc7-126">Pokud máte [bower](http://bower.io/) nainstalovaná, právě spuštěním tohoto příkazu.</span><span class="sxs-lookup"><span data-stu-id="a3cc7-126">If you have [bower](http://bower.io/) installed, you can just run this command.</span></span>  <span data-ttu-id="a3cc7-127">Pro žádné neshody verze závislosti vyberte právě vyšší verze.</span><span class="sxs-lookup"><span data-stu-id="a3cc7-127">For any dependency version mismatches, just choose the higher version.</span></span>

```
bower install adal-angular#experimental
```

<span data-ttu-id="a3cc7-128">Alternativně můžete ručně stáhnout [adal.js](https://raw.githubusercontent.com/AzureAD/azure-activedirectory-library-for-js/experimental/dist/adal.min.js) a [adal angular.js](https://raw.githubusercontent.com/AzureAD/azure-activedirectory-library-for-js/experimental/dist/adal-angular.min.js).</span><span class="sxs-lookup"><span data-stu-id="a3cc7-128">Alternatively, you can manually download [adal.js](https://raw.githubusercontent.com/AzureAD/azure-activedirectory-library-for-js/experimental/dist/adal.min.js) and [adal-angular.js](https://raw.githubusercontent.com/AzureAD/azure-activedirectory-library-for-js/experimental/dist/adal-angular.min.js).</span></span>  <span data-ttu-id="a3cc7-129">Přidat oba soubory `app/lib/adal-angular-experimental/dist` adresáře.</span><span class="sxs-lookup"><span data-stu-id="a3cc7-129">Add both files to the `app/lib/adal-angular-experimental/dist` directory.</span></span>

<span data-ttu-id="a3cc7-130">Nyní otevřete projekt ve svém oblíbeném textovém editoru a načíst adal.js na konci obsahu stránky:</span><span class="sxs-lookup"><span data-stu-id="a3cc7-130">Now open the project in your favorite text editor, and load adal.js at the end of the page body:</span></span>

```html
<!--index.html-->

...

<script src="App/bower_components/dist/adal.min.js"></script>
<script src="App/bower_components/dist/adal-angular.min.js"></script>

...
```

## <a name="set-up-the-rest-api"></a><span data-ttu-id="a3cc7-131">Nastavení rozhraní REST API</span><span class="sxs-lookup"><span data-stu-id="a3cc7-131">Set up the REST API</span></span>
<span data-ttu-id="a3cc7-132">Když jste nastavujeme, umožní začít pracovat REST API back-end.</span><span class="sxs-lookup"><span data-stu-id="a3cc7-132">While we're setting things up, lets get the backend REST API working.</span></span>  <span data-ttu-id="a3cc7-133">V příkazovém řádku, nainstaluje všechny potřebné balíčky spuštěním (ujistěte se, zda pracujete v adresáři nejvyšší úrovně projektu):</span><span class="sxs-lookup"><span data-stu-id="a3cc7-133">In a command prompt, install all the necessary packages by running (make sure you're in the top-level directory of the project):</span></span>

```
npm install
```

<span data-ttu-id="a3cc7-134">Nyní otevřete `config.js` a nahraďte `audience` hodnotu:</span><span class="sxs-lookup"><span data-stu-id="a3cc7-134">Now open `config.js` and replace the `audience` value:</span></span>

```js
exports.creds = {

     // TODO: Replace this value with the Application ID from the registration portal
     audience: '<Your-application-id>',

     ...
}
```

<span data-ttu-id="a3cc7-135">Rozhraní REST API bude tato hodnota slouží k ověření tokeny, které obdrží z úhlová aplikace na požadavky AJAX.</span><span class="sxs-lookup"><span data-stu-id="a3cc7-135">The REST API will use this value to validate tokens it receives from the Angular app on AJAX requests.</span></span>  <span data-ttu-id="a3cc7-136">Všimněte si, že toto jednoduché rozhraní API REST ukládá data v paměti - takže každý čas stop pro server, budou ztraceny všechny dříve vytvořenou úlohy.</span><span class="sxs-lookup"><span data-stu-id="a3cc7-136">Note that this simple REST API stores data in-memory - so each time to stop the server, you will lose all previously created tasks.</span></span>

<span data-ttu-id="a3cc7-137">Je vždy, které vytvoříme zatěžovat hovoříte o tom, jak funguje rozhraní REST API.</span><span class="sxs-lookup"><span data-stu-id="a3cc7-137">That's all the time we're going to spend discussing how the REST API works.</span></span>  <span data-ttu-id="a3cc7-138">Nebojte se Vyvrtejte v kódu, ale pokud chcete získat další informace o zabezpečení webové rozhraní API s Azure AD, podívejte se na [v tomto článku](active-directory-v2-devquickstarts-node-api.md).</span><span class="sxs-lookup"><span data-stu-id="a3cc7-138">Feel free to poke around in the code, but if you want to learn more about securing web APIs with Azure AD, check out [this article](active-directory-v2-devquickstarts-node-api.md).</span></span> 

## <a name="sign-users-in"></a><span data-ttu-id="a3cc7-139">Přihlášení uživatelů</span><span class="sxs-lookup"><span data-stu-id="a3cc7-139">Sign users in</span></span>
<span data-ttu-id="a3cc7-140">Čas napsat kód, identity.</span><span class="sxs-lookup"><span data-stu-id="a3cc7-140">Time to write some identity code.</span></span>  <span data-ttu-id="a3cc7-141">Možná jste již si všimli, že tento adal.js obsahuje poskytovatele AngularJS, který hraje vhodně s úhlová směrování mechanismy.</span><span class="sxs-lookup"><span data-stu-id="a3cc7-141">You might have already noticed that adal.js contains an AngularJS provider, which plays nicely with Angular routing mechanisms.</span></span>  <span data-ttu-id="a3cc7-142">Začněte přidáním modulu adal k aplikaci:</span><span class="sxs-lookup"><span data-stu-id="a3cc7-142">Start by adding the adal module to the app:</span></span>

```js
// app/scripts/app.js

angular.module('todoApp', ['ngRoute','AdalAngular'])
.config(['$routeProvider','$httpProvider', 'adalAuthenticationServiceProvider',
 function ($routeProvider, $httpProvider, adalProvider) {

...
```

<span data-ttu-id="a3cc7-143">Nyní můžete inicializovat `adalProvider` s vaší aplikace ID:</span><span class="sxs-lookup"><span data-stu-id="a3cc7-143">You can now initialize the `adalProvider` with your Application ID:</span></span>

```js
// app/scripts/app.js

...

adalProvider.init({

        // Use this value for the public instance of Azure AD
        instance: 'https://login.microsoftonline.com/', 

        // The 'common' endpoint is used for multi-tenant applications like this one
        tenant: 'common',

        // Your application id from the registration portal
        clientId: '<Your-application-id>',

        // If you're using IE, uncommment this line - the default HTML5 sessionStorage does not work for localhost.
        //cacheLocation: 'localStorage',

    }, $httpProvider);
```

<span data-ttu-id="a3cc7-144">Skvělé, teď adal.js má všechny informace, které je nutné zabezpečit vaši aplikaci a přihlášení uživatelů.</span><span class="sxs-lookup"><span data-stu-id="a3cc7-144">Great, now adal.js has all the information it needs to secure your app and sign users in.</span></span>  <span data-ttu-id="a3cc7-145">Chcete-li vynutit přihlášení pro daný postup v aplikaci, jak dlouho trvá je jeden řádek kódu:</span><span class="sxs-lookup"><span data-stu-id="a3cc7-145">To force sign in for a particular route in the app, all it takes is one line of code:</span></span>

```js
// app/scripts/app.js

...

}).when("/TodoList", {
    controller: "todoListCtrl",
    templateUrl: "/static/views/TodoList.html",
    requireADLogin: true, // Ensures that the user must be logged in to access the route
})

...
```

<span data-ttu-id="a3cc7-146">Nyní když uživatel klikne na `TodoList` odkaz, adal.js automaticky přesměruje do služby Azure AD pro přihlášení v případě potřeby.</span><span class="sxs-lookup"><span data-stu-id="a3cc7-146">Now when a user clicks the `TodoList` link, adal.js will automatically redirect to Azure AD for sign-in if necessary.</span></span>  <span data-ttu-id="a3cc7-147">Vyvoláním adal.js v řadičích můžete odeslat také explicitně požadavků na přihlášení a odhlášení:</span><span class="sxs-lookup"><span data-stu-id="a3cc7-147">You can also explicitly send sign-in and sign-out requests by invoking adal.js in your controllers:</span></span>

```js
// app/scripts/homeCtrl.js

angular.module('todoApp')
// Load adal.js the same way for use in controllers and views   
.controller('homeCtrl', ['$scope', 'adalAuthenticationService','$location', function ($scope, adalService, $location) {
    $scope.login = function () {

        // Redirect the user to sign in
        adalService.login();

    };
    $scope.logout = function () {

        // Redirect the user to log out    
        adalService.logOut();

    };
...
```

## <a name="display-user-info"></a><span data-ttu-id="a3cc7-148">Zobrazit informace o uživateli</span><span class="sxs-lookup"><span data-stu-id="a3cc7-148">Display user info</span></span>
<span data-ttu-id="a3cc7-149">Teď, když se uživatel přihlásí, budete pravděpodobně potřebovat pro přístup k datům ověřování přihlášeného uživatele ve vaší aplikaci.</span><span class="sxs-lookup"><span data-stu-id="a3cc7-149">Now that the user is signed in, you'll probably need to access the signed-in user's authentication data in your application.</span></span>  <span data-ttu-id="a3cc7-150">Adal.js zpřístupní tyto informace můžete v `userInfo` objektu.</span><span class="sxs-lookup"><span data-stu-id="a3cc7-150">Adal.js exposes this information for you in the `userInfo` object.</span></span>  <span data-ttu-id="a3cc7-151">Chcete-li přístup k tomuto objektu v zobrazení, nejprve přidáte adal.js na kořenovém oboru odpovídající řadiče.</span><span class="sxs-lookup"><span data-stu-id="a3cc7-151">To access this object in a view, first add adal.js to the root scope of the corresponding controller:</span></span>

```js
// app/scripts/userDataCtrl.js

angular.module('todoApp')
// Load ADAL for use in view
.controller('userDataCtrl', ['$scope', 'adalAuthenticationService', function ($scope, adalService) {}]);
```

<span data-ttu-id="a3cc7-152">Pak můžete vyřešit přímo `userInfo` objekt ve svém zobrazení:</span><span class="sxs-lookup"><span data-stu-id="a3cc7-152">Then you can directly address the `userInfo` object in your view:</span></span> 

```html
<!--app/views/UserData.html-->

...

    <!--Get the user's profile information from the ADAL userInfo object-->
    <tr ng-repeat="(key, value) in userInfo.profile">
        <td>{{key}}</td>
        <td>{{value}}</td>
    </tr>
...
```

<span data-ttu-id="a3cc7-153">Můžete také `userInfo` objektem pro určení, pokud se uživatel je přihlášen nebo ne.</span><span class="sxs-lookup"><span data-stu-id="a3cc7-153">You can also use the `userInfo` object to determine if the user is signed in or not.</span></span>

```html
<!--index.html-->

...

    <!--Use the ADAL userInfo object to show the right login/logout button-->
    <ul class="nav navbar-nav navbar-right">
        <li><a class="btn btn-link" ng-show="userInfo.isAuthenticated" ng-click="logout()">Logout</a></li>
        <li><a class="btn btn-link" ng-hide="userInfo.isAuthenticated" ng-click="login()">Login</a></li>
    </ul>
...
```

## <a name="call-the-rest-api"></a><span data-ttu-id="a3cc7-154">Volání rozhraní REST API</span><span class="sxs-lookup"><span data-stu-id="a3cc7-154">Call the REST API</span></span>
<span data-ttu-id="a3cc7-155">Nakonec je čas získat některé tokeny a volání rozhraní REST API vytvářet, číst, aktualizovat a odstraňovat úkoly.</span><span class="sxs-lookup"><span data-stu-id="a3cc7-155">Finally, it's time to get some tokens and call the REST API to create, read, update, and delete tasks.</span></span>  <span data-ttu-id="a3cc7-156">A co s?</span><span class="sxs-lookup"><span data-stu-id="a3cc7-156">Well guess what?</span></span>  <span data-ttu-id="a3cc7-157">Není nutné provádět *co*.</span><span class="sxs-lookup"><span data-stu-id="a3cc7-157">You don't have to do *a thing*.</span></span>  <span data-ttu-id="a3cc7-158">Adal.js se automaticky postará o získávání, ukládání do mezipaměti a aktualizaci tokeny.</span><span class="sxs-lookup"><span data-stu-id="a3cc7-158">Adal.js will automatically take care of getting, caching, and refreshing tokens.</span></span>  <span data-ttu-id="a3cc7-159">Je také postará o tyto tokeny se připojuje k odchozí požadavky AJAX, které odesílají do rozhraní REST API.</span><span class="sxs-lookup"><span data-stu-id="a3cc7-159">It will also take care of attaching those tokens to outgoing AJAX requests that you send to the REST API.</span></span>  

<span data-ttu-id="a3cc7-160">Jak přesně to funguje?</span><span class="sxs-lookup"><span data-stu-id="a3cc7-160">How exactly does this work?</span></span> <span data-ttu-id="a3cc7-161">Je všechny díky magic z [AngularJS sběrače](https://docs.angularjs.org/api/ng/service/$http), což umožňuje adal.js k transformaci odchozí a příchozí zprávy http.</span><span class="sxs-lookup"><span data-stu-id="a3cc7-161">It's all thanks to the magic of [AngularJS interceptors](https://docs.angularjs.org/api/ng/service/$http), which allows adal.js to transform outgoing and incoming http messages.</span></span>  <span data-ttu-id="a3cc7-162">Kromě toho adal.js předpokládá, že všechny žádosti odeslat do stejné domény jako okna měli používat tokeny, které jsou určené pro stejné ID aplikace jako aplikace AngularJS.</span><span class="sxs-lookup"><span data-stu-id="a3cc7-162">Furthermore, adal.js assumes that any requests send to the same domain as the window should use tokens intended for the same Application ID as the AngularJS app.</span></span>  <span data-ttu-id="a3cc7-163">Z tohoto důvodu jsme použili stejné ID aplikace v úhlová aplikace a rozhraní REST API NodeJS.</span><span class="sxs-lookup"><span data-stu-id="a3cc7-163">This is why we used the same Application ID in both the Angular app and in the NodeJS REST API.</span></span>  <span data-ttu-id="a3cc7-164">Samozřejmě můžete toto chování potlačit a řekněte adal.js získat tokeny pro jiná rozhraní API REST v případě potřeby - ale pro tento scénář jednoduchého provede výchozí hodnoty.</span><span class="sxs-lookup"><span data-stu-id="a3cc7-164">Of course, you can override this behavior and tell adal.js to get tokens for other REST APIs if necessary - but for this simple scenario the defaults will do.</span></span>

<span data-ttu-id="a3cc7-165">Zde je fragment kódu, který ukazuje, jak snadné je k posílání požadavků s nosné tokeny z Azure AD:</span><span class="sxs-lookup"><span data-stu-id="a3cc7-165">Here's a snippet that shows how easy it is to send requests with bearer tokens from Azure AD:</span></span>

```js
// app/scripts/todoListSvc.js

...
return $http.get('/api/tasks');
...
```

<span data-ttu-id="a3cc7-166">Blahopřejeme!</span><span class="sxs-lookup"><span data-stu-id="a3cc7-166">Congratulations!</span></span>  <span data-ttu-id="a3cc7-167">Integrované jednostránkové aplikace Azure AD je nyní dokončen.</span><span class="sxs-lookup"><span data-stu-id="a3cc7-167">Your Azure AD integrated single page app is now complete.</span></span>  <span data-ttu-id="a3cc7-168">Pokračujte, proveďte předních.</span><span class="sxs-lookup"><span data-stu-id="a3cc7-168">Go ahead, take a bow.</span></span>  <span data-ttu-id="a3cc7-169">Ho můžete ověřovat uživatele, bezpečně volat jeho back-end pomocí OpenID Connect rozhraní REST API a získat základní informace o uživateli.</span><span class="sxs-lookup"><span data-stu-id="a3cc7-169">It can authenticate users, securely call its backend REST API using OpenID Connect, and get basic information about the user.</span></span>  <span data-ttu-id="a3cc7-170">Předinstalované podporuje každý uživatel s osobní Account Microsoft nebo pracovní nebo školní účet z Azure AD.</span><span class="sxs-lookup"><span data-stu-id="a3cc7-170">Out of the box, it supports any user with a personal Microsoft Account or a work/school account from Azure AD.</span></span>  <span data-ttu-id="a3cc7-171">Dejte aplikaci zkuste to spuštěním:</span><span class="sxs-lookup"><span data-stu-id="a3cc7-171">Give the app a try by running:</span></span>

```
node server.js
```

<span data-ttu-id="a3cc7-172">V prohlížeči přejděte na `http://localhost:8080`.</span><span class="sxs-lookup"><span data-stu-id="a3cc7-172">In a browser navigate to `http://localhost:8080`.</span></span>  <span data-ttu-id="a3cc7-173">Přihlaste se pomocí osobního účtu Microsoft nebo pracovní nebo školní účet.</span><span class="sxs-lookup"><span data-stu-id="a3cc7-173">Sign in using either a personal Microsoft account or a work/school account.</span></span>  <span data-ttu-id="a3cc7-174">Přidání úkolů do seznamu úkolů uživatele a odhlášení.</span><span class="sxs-lookup"><span data-stu-id="a3cc7-174">Add tasks to the user's to-do list, and sign out.</span></span>  <span data-ttu-id="a3cc7-175">Pokuste se přihlásit jiný typ účtu.</span><span class="sxs-lookup"><span data-stu-id="a3cc7-175">Try signing in with the other type of account.</span></span> <span data-ttu-id="a3cc7-176">Pokud potřebujete klienta Azure AD pro vytváření uživatelů pracovní nebo školní [zjistěte, jak získat tady](active-directory-howto-tenant.md) (je to zdarma).</span><span class="sxs-lookup"><span data-stu-id="a3cc7-176">If you need an Azure AD tenant to create work/school users, [learn how to get one here](active-directory-howto-tenant.md) (it's free).</span></span>

<span data-ttu-id="a3cc7-177">A pokračujte ve čtení o koncový bod v2.0, přejděte zpět naše [Příručka vývojáře v2.0](active-directory-appmodel-v2-overview.md).</span><span class="sxs-lookup"><span data-stu-id="a3cc7-177">To continue learning about the the v2.0 endpoint, head back to our [v2.0 developer guide](active-directory-appmodel-v2-overview.md).</span></span>  <span data-ttu-id="a3cc7-178">Další zdroje projděte si:</span><span class="sxs-lookup"><span data-stu-id="a3cc7-178">For additional resources, check out:</span></span>

* [<span data-ttu-id="a3cc7-179">Azure – ukázky z webu GitHub >></span><span class="sxs-lookup"><span data-stu-id="a3cc7-179">Azure-Samples on GitHub >></span></span>](https://github.com/Azure-Samples)
* [<span data-ttu-id="a3cc7-180">Azure AD na přetečení zásobníku >></span><span class="sxs-lookup"><span data-stu-id="a3cc7-180">Azure AD on Stack Overflow >></span></span>](http://stackoverflow.com/questions/tagged/azure-active-directory)
* <span data-ttu-id="a3cc7-181">Dokumentace k Azure AD na [Azure.com >>](https://azure.microsoft.com/documentation/services/active-directory/)</span><span class="sxs-lookup"><span data-stu-id="a3cc7-181">Azure AD documentation on [Azure.com >>](https://azure.microsoft.com/documentation/services/active-directory/)</span></span>

## <a name="get-security-updates-for-our-products"></a><span data-ttu-id="a3cc7-182">Získejte bezpečnostní aktualizace našich produktů</span><span class="sxs-lookup"><span data-stu-id="a3cc7-182">Get security updates for our products</span></span>
<span data-ttu-id="a3cc7-183">Doporučujeme vám získávat oznámení o bezpečnostních incidentech tak, že navštívíte [tuto stránku](https://technet.microsoft.com/security/dd252948) a přihlásíte se k odběru služby Security Advisory Alerts.</span><span class="sxs-lookup"><span data-stu-id="a3cc7-183">We encourage you to get notifications of when security incidents occur by visiting [this page](https://technet.microsoft.com/security/dd252948) and subscribing to Security Advisory Alerts.</span></span>

