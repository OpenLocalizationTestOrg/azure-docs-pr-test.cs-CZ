---
title: "Azure Active Directory v2.0 Node.js webové aplikace přihlášení | Microsoft Docs"
description: "Naučte se vytvářet webové aplikace Node.js, který se přihlásí uživatel pomocí účtu Microsoft osobní i pracovní nebo školní účet."
services: active-directory
documentationcenter: nodejs
author: navyasric
manager: mbaldwin
editor: 
ms.assetid: 1b889e72-f5c3-464a-af57-79abf5e2e147
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: javascript
ms.topic: article
ms.date: 05/13/2017
ms.author: nacanuma
ms.custom: aaddev
ms.openlocfilehash: 6d49c742f72440e22830915c90de009d9188db2a
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="add-sign-in-to-a-nodejs-web-app"></a><span data-ttu-id="2e318-103">Přidání přihlašování do webové aplikace Node.js</span><span class="sxs-lookup"><span data-stu-id="2e318-103">Add sign-in to a Node.js web app</span></span>

> [!NOTE]
> <span data-ttu-id="2e318-104">Ne všechny funkce a scénáře Azure Active Directory fungovat s koncovým bodem v2.0.</span><span class="sxs-lookup"><span data-stu-id="2e318-104">Not all Azure Active Directory scenarios and features work with the v2.0 endpoint.</span></span> <span data-ttu-id="2e318-105">Pokud chcete zjistit, zda by měl používat koncového bodu v2.0 nebo koncový bod verze 1.0, přečtěte si informace o [v2.0 omezení](active-directory-v2-limitations.md).</span><span class="sxs-lookup"><span data-stu-id="2e318-105">To determine whether you should use the v2.0 endpoint or the v1.0 endpoint, read about [v2.0 limitations](active-directory-v2-limitations.md).</span></span>
> 

<span data-ttu-id="2e318-106">V tomto kurzu používáme Passport provádění následujících úloh:</span><span class="sxs-lookup"><span data-stu-id="2e318-106">In this tutorial, we use Passport to do the following tasks:</span></span>

* <span data-ttu-id="2e318-107">Ve webové aplikaci přihlaste uživatele pomocí služby Azure Active Directory (Azure AD) a koncový bod v2.0.</span><span class="sxs-lookup"><span data-stu-id="2e318-107">In a web app, sign in the user by using Azure Active Directory (Azure AD) and the v2.0 endpoint.</span></span>
* <span data-ttu-id="2e318-108">Zobrazí informace o uživateli.</span><span class="sxs-lookup"><span data-stu-id="2e318-108">Display information about the user.</span></span>
* <span data-ttu-id="2e318-109">Přihlášení uživatele mimo aplikaci.</span><span class="sxs-lookup"><span data-stu-id="2e318-109">Sign the user out of the app.</span></span>

<span data-ttu-id="2e318-110">**Passport** je ověřovací middleware pro Node.js.</span><span class="sxs-lookup"><span data-stu-id="2e318-110">**Passport** is authentication middleware for Node.js.</span></span> <span data-ttu-id="2e318-111">Flexibilní a modulární, můžete do jakéhokoli nenápadně vyřadit Passport využívající Express nebo restify webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="2e318-111">Flexible and modular, Passport can be unobtrusively dropped into any Express-based or restify web application.</span></span> <span data-ttu-id="2e318-112">Komplexní sada strategií Passport, podporují ověřování pomocí uživatelského jména a hesla, Facebook, Twitter a další možnosti.</span><span class="sxs-lookup"><span data-stu-id="2e318-112">In Passport, a comprehensive set of strategies support authentication by using a username and password, Facebook, Twitter, or other options.</span></span> <span data-ttu-id="2e318-113">Vyvinuli jsme strategii pro Azure AD.</span><span class="sxs-lookup"><span data-stu-id="2e318-113">We have developed a strategy for Azure AD.</span></span> <span data-ttu-id="2e318-114">V tomto článku jsme ukazují, jak nainstalovat modul a poté přidejte Azure AD `passport-azure-ad` modulu plug-in.</span><span class="sxs-lookup"><span data-stu-id="2e318-114">In this article, we show you how to install the module, and then add the Azure AD `passport-azure-ad` plug-in.</span></span>

## <a name="download"></a><span data-ttu-id="2e318-115">Ke stažení</span><span class="sxs-lookup"><span data-stu-id="2e318-115">Download</span></span>
<span data-ttu-id="2e318-116">Kód k tomuto kurzu je udržovaný [na GitHubu](https://github.com/AzureADQuickStarts/AppModelv2-WebApp-OpenIDConnect-nodejs).</span><span class="sxs-lookup"><span data-stu-id="2e318-116">The code for this tutorial is maintained [on GitHub](https://github.com/AzureADQuickStarts/AppModelv2-WebApp-OpenIDConnect-nodejs).</span></span> <span data-ttu-id="2e318-117">Chcete-li postupovat v kurzu, můžete [stáhnout kostru aplikace jako soubor ZIP](https://github.com/AzureADQuickStarts/AppModelv2-WebApp-OpenIDConnect-nodejs/archive/skeleton.zip) nebo tuto kostru klonovat:</span><span class="sxs-lookup"><span data-stu-id="2e318-117">To follow the tutorial, you can [download the app's skeleton as a .zip file](https://github.com/AzureADQuickStarts/AppModelv2-WebApp-OpenIDConnect-nodejs/archive/skeleton.zip) or clone the skeleton:</span></span>

```git clone --branch skeleton https://github.com/AzureADQuickStarts/AppModelv2-WebApp-OpenIDConnect-nodejs.git```

<span data-ttu-id="2e318-118">Také můžete získat hotová aplikace na konci tohoto kurzu.</span><span class="sxs-lookup"><span data-stu-id="2e318-118">You also can get the completed application at the end of this tutorial.</span></span>

## <a name="1-register-an-app"></a><span data-ttu-id="2e318-119">1: registrace aplikace</span><span class="sxs-lookup"><span data-stu-id="2e318-119">1: Register an app</span></span>
<span data-ttu-id="2e318-120">Vytvoření nové aplikace v [apps.dev.microsoft.com](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList), nebo postupujte podle [tyto podrobné kroky](active-directory-v2-app-registration.md) k registraci aplikace.</span><span class="sxs-lookup"><span data-stu-id="2e318-120">Create a new app at [apps.dev.microsoft.com](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList), or follow [these detailed steps](active-directory-v2-app-registration.md) to register an app.</span></span> <span data-ttu-id="2e318-121">Ověřte, že je:</span><span class="sxs-lookup"><span data-stu-id="2e318-121">Make sure you:</span></span>

* <span data-ttu-id="2e318-122">Kopírování **Id aplikace** přiřazené vaší aplikaci.</span><span class="sxs-lookup"><span data-stu-id="2e318-122">Copy the **Application Id** assigned to your app.</span></span> <span data-ttu-id="2e318-123">Budete ho potřebovat pro účely tohoto kurzu.</span><span class="sxs-lookup"><span data-stu-id="2e318-123">You need it for this tutorial.</span></span>
* <span data-ttu-id="2e318-124">Přidat **webové** platformu pro vaši aplikaci.</span><span class="sxs-lookup"><span data-stu-id="2e318-124">Add the **Web** platform for your app.</span></span>
* <span data-ttu-id="2e318-125">Kopírování **identifikátor URI pro přesměrování** z portálu.</span><span class="sxs-lookup"><span data-stu-id="2e318-125">Copy the **Redirect URI** from the portal.</span></span> <span data-ttu-id="2e318-126">Musíte použít výchozí hodnotu identifikátoru URI `urn:ietf:wg:oauth:2.0:oob`.</span><span class="sxs-lookup"><span data-stu-id="2e318-126">You must use the default URI value of `urn:ietf:wg:oauth:2.0:oob`.</span></span>

## <a name="2-add-prerequisities-to-your-directory"></a><span data-ttu-id="2e318-127">2: Přidání prerequisities do adresáře</span><span class="sxs-lookup"><span data-stu-id="2e318-127">2: Add prerequisities to your directory</span></span>
<span data-ttu-id="2e318-128">Na příkazovém řádku změňte adresáře na přejděte do kořenové složky, pokud si nejste již existuje.</span><span class="sxs-lookup"><span data-stu-id="2e318-128">At a command prompt, change directories to go to your root folder, if you are not already there.</span></span> <span data-ttu-id="2e318-129">Spusťte následující příkazy:</span><span class="sxs-lookup"><span data-stu-id="2e318-129">Run the following commands:</span></span>

* `npm install express`
* `npm install ejs`
* `npm install ejs-locals`
* `npm install restify`
* `npm install mongoose`
* `npm install bunyan`
* `npm install assert-plus`
* `npm install passport`
* `npm install webfinger`
* `npm install body-parser`
* `npm install express-session`
* `npm install cookie-parser`

<span data-ttu-id="2e318-130">Kromě toho používáme `passport-azure-ad` v kostře rychlého startu:</span><span class="sxs-lookup"><span data-stu-id="2e318-130">In addition, we use `passport-azure-ad` in the skeleton of the quickstart:</span></span>

* `npm install passport-azure-ad`

<span data-ttu-id="2e318-131">Tím se nainstaluje do knihoven, `passport-azure-ad` používá.</span><span class="sxs-lookup"><span data-stu-id="2e318-131">This installs the libraries that `passport-azure-ad` uses.</span></span>

## <a name="3-set-up-your-app-to-use-the-passport-node-js-strategy"></a><span data-ttu-id="2e318-132">3: nastavení aplikace pro použití strategie passport uzlu js</span><span class="sxs-lookup"><span data-stu-id="2e318-132">3: Set up your app to use the passport-node-js strategy</span></span>
<span data-ttu-id="2e318-133">Nastavte Express middleware pro použití ověřovacího protokolu OpenID Connect.</span><span class="sxs-lookup"><span data-stu-id="2e318-133">Set up the Express middleware to use the OpenID Connect authentication protocol.</span></span> <span data-ttu-id="2e318-134">Používání služby Passport pro zasílání požadavků na přihlášení a odhlášení, správu relace uživatele a získat informace o uživateli, mimo jiné.</span><span class="sxs-lookup"><span data-stu-id="2e318-134">You use Passport to issue sign-in and sign-out requests, manage the user's session, and get information about the user, among other things.</span></span>

1.  <span data-ttu-id="2e318-135">V kořenovém adresáři projektu otevřete souboru Config.js.</span><span class="sxs-lookup"><span data-stu-id="2e318-135">In the root of the project, open the Config.js file.</span></span> <span data-ttu-id="2e318-136">V `exports.creds` zadejte hodnoty konfigurace vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="2e318-136">In the `exports.creds` section, enter your app's configuration values.</span></span>
  
  * <span data-ttu-id="2e318-137">`clientID`: **Id aplikace** přiřazené k aplikaci na portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="2e318-137">`clientID`: The **Application Id** that's assigned to your app in the Azure portal.</span></span>
  * <span data-ttu-id="2e318-138">`returnURL`: **Identifikátor URI pro přesměrování** kterou jste zadali v portálu.</span><span class="sxs-lookup"><span data-stu-id="2e318-138">`returnURL`: The **Redirect URI** that you entered in the portal.</span></span>
  * <span data-ttu-id="2e318-139">`clientSecret`: Tajný klíč, který jste vygenerovali na portálu.</span><span class="sxs-lookup"><span data-stu-id="2e318-139">`clientSecret`: The secret that you generated in the portal.</span></span>

2.  <span data-ttu-id="2e318-140">V kořenovém adresáři projektu otevřete soubor App.js.</span><span class="sxs-lookup"><span data-stu-id="2e318-140">In the root of the project, open the App.js file.</span></span> <span data-ttu-id="2e318-141">K vyvolání stratey OIDCStrategy, který se dodává s `passport-azure-ad`, přidejte následující volání:</span><span class="sxs-lookup"><span data-stu-id="2e318-141">To invoke the OIDCStrategy stratey, which comes with `passport-azure-ad`, add the following call:</span></span>

  ```JavaScript
  var OIDCStrategy = require('passport-azure-ad').OIDCStrategy;

  // Add some logging
  var log = bunyan.createLogger({
      name: 'Microsoft OIDC Example Web Application'
  });
  ```

3.  <span data-ttu-id="2e318-142">Pro zpracování vaší žádosti o přihlášení, použijte strategie, kterou jste právě odkazuje:</span><span class="sxs-lookup"><span data-stu-id="2e318-142">To handle your sign-in requests, use the strategy you just referenced:</span></span>

  ```JavaScript
  // Use the OIDCStrategy within Passport (section 2)
  //
  //   Strategies in Passport require a `validate` function. The function accepts
  //   credentials (in this case, an OpenID identifier), and invokes a callback
  //   with a user object.
  passport.use( new OIDCStrategy({
      callbackURL: config.creds.returnURL,
      realm: config.creds.realm,
      clientID: config.creds.clientID,
      clientSecret: config.creds.clientSecret,
      oidcIssuer: config.creds.issuer,
      identityMetadata: config.creds.identityMetadata,
      responseType: config.creds.responseType,
      responseMode: config.creds.responseMode,
      skipUserProfile: config.creds.skipUserProfile
      scope: config.creds.scope
    },
    function(iss, sub, profile, accessToken, refreshToken, done) {
      log.info('Example: Email address we received was: ', profile.email);
      // Asynchronous verification, for effect...
      process.nextTick(function () {
        findByEmail(profile.email, function(err, user) {
          if (err) {
            return done(err);
          }
          if (!user) {
            // "Auto-registration"
            users.push(profile);
            return done(null, profile);
          }
          return done(null, user);
        });
      });
    }
  ));
  ```

<span data-ttu-id="2e318-143">Passport používá podobný Princip pro všechny svoje strategie (Twitteru, Facebooku a tak dále).</span><span class="sxs-lookup"><span data-stu-id="2e318-143">Passport uses a similar pattern for all its strategies (Twitter, Facebook, and so on).</span></span> <span data-ttu-id="2e318-144">Řídí všichni autoři strategií se vzorem.</span><span class="sxs-lookup"><span data-stu-id="2e318-144">All strategy writers adhere to the pattern.</span></span> <span data-ttu-id="2e318-145">Předat strategie `function()` token, který používá a `done` jako parametry.</span><span class="sxs-lookup"><span data-stu-id="2e318-145">Pass the strategy a `function()` that uses a token and `done` as parameters.</span></span> <span data-ttu-id="2e318-146">Strategie se vrátí po dělá svou práci.</span><span class="sxs-lookup"><span data-stu-id="2e318-146">The strategy is returned after it does all its work.</span></span> <span data-ttu-id="2e318-147">Uložení uživatele a skrytí tokenu, takže je nebudete muset požadovat znovu.</span><span class="sxs-lookup"><span data-stu-id="2e318-147">Store the user and stash the token so you don’t need to ask for it again.</span></span>

  > [!IMPORTANT]
  > <span data-ttu-id="2e318-148">Předchozí kód přijme všechny uživatele, který může ověřit na váš server.</span><span class="sxs-lookup"><span data-stu-id="2e318-148">The preceding code takes any user that can authenticate to your server.</span></span> <span data-ttu-id="2e318-149">To se označuje jako Automatická registrace.</span><span class="sxs-lookup"><span data-stu-id="2e318-149">This is known as auto-registration.</span></span> <span data-ttu-id="2e318-150">Na provozním serveru byste neměli chtít vpouštět každý, kdo bez toho, aby předtím prošli registračním procesem, který zvolíte.</span><span class="sxs-lookup"><span data-stu-id="2e318-150">On a production server, you wouldn’t want to let anyone in without first having them go through a registration process that you choose.</span></span> <span data-ttu-id="2e318-151">To je obvykle vzor, který můžete vidět u uživatelských aplikací.</span><span class="sxs-lookup"><span data-stu-id="2e318-151">This is usually the pattern that you see in consumer apps.</span></span> <span data-ttu-id="2e318-152">Aplikace může umožňují registraci pomocí Facebooku, ale následně požádá, můžete k zadání dalších informací.</span><span class="sxs-lookup"><span data-stu-id="2e318-152">The app might allow you to register with Facebook, but then it asks you to enter additional information.</span></span> <span data-ttu-id="2e318-153">Pokud v tomto kurzu nebyly pomocí příkazového řádku programu, může extrahovat e-mail z vráceného objektu tokenu.</span><span class="sxs-lookup"><span data-stu-id="2e318-153">If you weren't using a command-line program for this tutorial, you could extract the email from the token object that is returned.</span></span> <span data-ttu-id="2e318-154">Potom může požádat uživatele k zadání dalších informací.</span><span class="sxs-lookup"><span data-stu-id="2e318-154">Then, you might ask the user to enter additional information.</span></span> <span data-ttu-id="2e318-155">Protože se jedná o testovací server, je třeba přidat uživatele přímo k databázi v paměti.</span><span class="sxs-lookup"><span data-stu-id="2e318-155">Because this is a test server, you add the user directly to the in-memory database.</span></span>
  > 
  > 

4.  <span data-ttu-id="2e318-156">Přidejte metody, které můžete použít ke sledování uživatelů, kteří se přihlásili, jak vyžaduje Passport.</span><span class="sxs-lookup"><span data-stu-id="2e318-156">Add the methods that you use to keep track of users who are signed in, as required by Passport.</span></span> <span data-ttu-id="2e318-157">To zahrnuje serializaci a deserializaci informací o uživateli:</span><span class="sxs-lookup"><span data-stu-id="2e318-157">This includes serializing and deserializing the user's information:</span></span>

  ```JavaScript

  // Passport session setup (section 2)

  //   To support persistent login sessions, Passport needs to be able to
  //   serialize users into, and deserialize users out of, the session. Typically,
  //   this is as simple as storing the user ID when serializing, and finding
  //   the user by ID when deserializing.
  passport.serializeUser(function(user, done) {
    done(null, user.email);
  });

  passport.deserializeUser(function(id, done) {
    findByEmail(id, function (err, user) {
      done(err, user);
    });
  });

  // Array to hold signed-in users
  var users = [];

  var findByEmail = function(email, fn) {
    for (var i = 0, len = users.length; i < len; i++) {
      var user = users[i];
      log.info('we are using user: ', user);
      if (user.email === email) {
        return fn(null, user);
      }
    }
    return fn(null, null);
  };
  ```

5.  <span data-ttu-id="2e318-158">Přidejte kód, který načte modulu Express.</span><span class="sxs-lookup"><span data-stu-id="2e318-158">Add the code that loads the Express engine.</span></span> <span data-ttu-id="2e318-159">Použijte výchozí /views a /routes vzor, který Express poskytuje:</span><span class="sxs-lookup"><span data-stu-id="2e318-159">You use the default /views and /routes pattern that Express provides:</span></span>

  ```JavaScript

  // Set up Express (section 2)

  var app = express();

  app.configure(function() {
    app.set('views', __dirname + '/views');
    app.set('view engine', 'ejs');
    app.use(express.logger());
    app.use(express.methodOverride());
    app.use(cookieParser());
    app.use(expressSession({ secret: 'keyboard cat', resave: true, saveUninitialized: false }));
    app.use(bodyParser.urlencoded({ extended : true }));
    // Initialize Passport!  Also use passport.session() middleware, to support
    // persistent login sessions (recommended).
    app.use(passport.initialize());
    app.use(passport.session());
    app.use(app.router);
    app.use(express.static(__dirname + '/../../public'));
  });

  ```

6.  <span data-ttu-id="2e318-160">Přidání tras této můžete předat skutečné přihlášení žádosti, které chcete v příspěvku `passport-azure-ad` modul:</span><span class="sxs-lookup"><span data-stu-id="2e318-160">Add the POST routes that hand off the actual sign-in requests to the `passport-azure-ad` engine:</span></span>

  ```JavaScript

  // Auth routes (section 3)

  // GET /auth/openid
  //   Use passport.authenticate() as route middleware to authenticate the
  //   request. The first step in OpenID authentication involves redirecting
  //   the user to the user's OpenID provider. After authenticating, the OpenID
  //   provider redirects the user back to this application at
  //   /auth/openid/return.

  app.get('/auth/openid',
    passport.authenticate('azuread-openidconnect', { failureRedirect: '/login' }),
    function(req, res) {
      log.info('Authentication was called in the sample');
      res.redirect('/');
    });

  // GET /auth/openid/return
  //   Use passport.authenticate() as route middleware to authenticate the
  //   request. If authentication fails, the user is redirected back to the
  //   sign-in page. Otherwise, the primary route function is called.
  //   In this example, it redirects the user to the home page.
  app.get('/auth/openid/return',
    passport.authenticate('azuread-openidconnect', { failureRedirect: '/login' }),
    function(req, res) {

      res.redirect('/');
    });

  // POST /auth/openid/return
  //   Use passport.authenticate() as route middleware to authenticate the
  //   request. If authentication fails, the user is redirected back to the
  //   sign-in page. Otherwise, the primary route function is called. 
  //   In this example, it redirects the user to the home page.

  app.post('/auth/openid/return',
    passport.authenticate('azuread-openidconnect', { failureRedirect: '/login' }),
    function(req, res) {

      res.redirect('/');
    });
  ```

## <a name="4-use-passport-to-issue-sign-in-and-sign-out-requests-to-azure-ad"></a><span data-ttu-id="2e318-161">4: použití služby Passport pro zasílání požadavků na přihlášení a odhlášení do Azure AD</span><span class="sxs-lookup"><span data-stu-id="2e318-161">4: Use Passport to issue sign-in and sign-out requests to Azure AD</span></span>
<span data-ttu-id="2e318-162">Aplikace je teď nastavený pro komunikaci s koncovým bodem v2.0 pomocí ověřovacího protokolu OpenID Connect.</span><span class="sxs-lookup"><span data-stu-id="2e318-162">Your app is now set up to communicate with the v2.0 endpoint by using the OpenID Connect authentication protocol.</span></span> <span data-ttu-id="2e318-163">`passport-azure-ad` Strategie postará všechny podrobnosti o věnujte zpráv ověřování, ověřování tokenů z Azure AD a údržbě uživatelské relace.</span><span class="sxs-lookup"><span data-stu-id="2e318-163">The `passport-azure-ad` strategy takes care of all the details of crafting authentication messages, validating tokens from Azure AD, and maintaining the user session.</span></span> <span data-ttu-id="2e318-164">Je již zbývá chcete umožnit uživatelům způsob přihlášení a odhlášení a získat další informace o uživateli, který je přihlášený.</span><span class="sxs-lookup"><span data-stu-id="2e318-164">All that is left to do is to give your users a way to sign in and sign out, and to gather more information about the user who is signed in.</span></span>

1.  <span data-ttu-id="2e318-165">Přidat **výchozí**, **přihlášení**, **účet**, a **odhlášení** metody do souboru App.js:</span><span class="sxs-lookup"><span data-stu-id="2e318-165">Add the **default**, **login**, **account**, and **logout** methods to your App.js file:</span></span>

  ```JavaScript

  //Routes (section 4)

  app.get('/', function(req, res){
    res.render('index', { user: req.user });
  });

  app.get('/account', ensureAuthenticated, function(req, res){
    res.render('account', { user: req.user });
  });

  app.get('/login',
    passport.authenticate('azuread-openidconnect', { failureRedirect: '/login' }),
    function(req, res) {
      log.info('Login was called in the sample');
      res.redirect('/');
  });

  app.get('/logout', function(req, res){
    req.logout();
    res.redirect('/');
  });

  ```

  <span data-ttu-id="2e318-166">Zde jsou uvedeny podrobnosti:</span><span class="sxs-lookup"><span data-stu-id="2e318-166">Here are the details:</span></span>
    
    * <span data-ttu-id="2e318-167">`/` Trasa přesměruje na zobrazení index.ejs.</span><span class="sxs-lookup"><span data-stu-id="2e318-167">The `/` route redirects to the index.ejs view.</span></span> <span data-ttu-id="2e318-168">Pak předá uživatele v požadavku (pokud existuje).</span><span class="sxs-lookup"><span data-stu-id="2e318-168">It passes the user in the request (if it exists).</span></span>
    * <span data-ttu-id="2e318-169">`/account` Směrovat nejprve *zajistí, že jsou ověří* (implementujete, v následujícím kódu).</span><span class="sxs-lookup"><span data-stu-id="2e318-169">The `/account` route first *ensures that you are authenticated* (you implement that in the following code).</span></span> <span data-ttu-id="2e318-170">Poté předá uživatele v požadavku.</span><span class="sxs-lookup"><span data-stu-id="2e318-170">Then, it passes the user in the request.</span></span> <span data-ttu-id="2e318-171">Toto je, abyste získali další informace o uživateli.</span><span class="sxs-lookup"><span data-stu-id="2e318-171">This is so you can get more information about the user.</span></span>
    * <span data-ttu-id="2e318-172">`/login` Směrování volání vaše `azuread-openidconnect` authenticator z `passport-azuread`.</span><span class="sxs-lookup"><span data-stu-id="2e318-172">The `/login` route calls your `azuread-openidconnect` authenticator from `passport-azuread`.</span></span> <span data-ttu-id="2e318-173">Pokud není úspěšné, ho přesměruje uživatele zpět na `/login`.</span><span class="sxs-lookup"><span data-stu-id="2e318-173">If that doesn't succeed, it redirects the user back to `/login`.</span></span>
    * <span data-ttu-id="2e318-174">`/logout` Trasy volá logout.ejs zobrazení (a směrování).</span><span class="sxs-lookup"><span data-stu-id="2e318-174">The `/logout` route calls the logout.ejs view (and route).</span></span> <span data-ttu-id="2e318-175">Vymaže soubory cookie a poté vrátí uživatele zpět na index.ejs.</span><span class="sxs-lookup"><span data-stu-id="2e318-175">This clears cookies, and then returns the user back to index.ejs.</span></span>

2.  <span data-ttu-id="2e318-176">Přidat **EnsureAuthenticated** metoda, která jste použili výše v `/account`:</span><span class="sxs-lookup"><span data-stu-id="2e318-176">Add the **EnsureAuthenticated** method that you used earlier in `/account`:</span></span>

  ```JavaScript

  // Route middleware to ensure the user is authenticated (section 4)

  //   Use this route middleware on any resource that needs to be protected. If
  //   the request is authenticated (typically via a persistent login session),
  //   the request proceeds. Otherwise, the user is redirected to the
  //   sign-in page.
  function ensureAuthenticated(req, res, next) {
    if (req.isAuthenticated()) { return next(); }
    res.redirect('/login')
  }

  ```

3.  <span data-ttu-id="2e318-177">V App.js vytvořte serveru:</span><span class="sxs-lookup"><span data-stu-id="2e318-177">In App.js, create the server:</span></span>

  ```JavaScript

  app.listen(3000);

  ```


## <a name="5-create-the-views-and-routes-in-express-that-you-show-your-user-on-the-website"></a><span data-ttu-id="2e318-178">5: vytvoření zobrazení a trasy v Express uživatelům zobrazí na webu</span><span class="sxs-lookup"><span data-stu-id="2e318-178">5: Create the views and routes in Express that you show your user on the website</span></span>
<span data-ttu-id="2e318-179">Přidáte trasy a zobrazení, které se zobrazí informace o uživateli.</span><span class="sxs-lookup"><span data-stu-id="2e318-179">Add the routes and views that show information to the user.</span></span> <span data-ttu-id="2e318-180">Trasy a zobrazení, zpracovávají také dříve `/logout` a `/login` tras, které jste vytvořili.</span><span class="sxs-lookup"><span data-stu-id="2e318-180">The routes and views also handle the `/logout` and `/login` routes that you created.</span></span>

1. <span data-ttu-id="2e318-181">V kořenovém adresáři vytvořte `/routes/index.js` trasy.</span><span class="sxs-lookup"><span data-stu-id="2e318-181">In the root directory, create the `/routes/index.js` route.</span></span>

  ```JavaScript

  /*
  * GET home page.
  */

  exports.index = function(req, res){
    res.render('index', { title: 'Express' });
  };
  ```

2.  <span data-ttu-id="2e318-182">V kořenovém adresáři vytvořte `/routes/user.js` trasy.</span><span class="sxs-lookup"><span data-stu-id="2e318-182">In the root directory, create the `/routes/user.js` route.</span></span>

  ```JavaScript

  /*
  * GET users listing.
  */

  exports.list = function(req, res){
    res.send("respond with a resource");
  };
  ```

  <span data-ttu-id="2e318-183">`/routes/index.js`a `/routes/user.js` jsou jednoduché trasy, které předávají žádosti zobrazením, včetně uživatele, pokud je k dispozici.</span><span class="sxs-lookup"><span data-stu-id="2e318-183">`/routes/index.js` and `/routes/user.js` are simple routes that pass along the request to your views, including the user, if present.</span></span>

3.  <span data-ttu-id="2e318-184">V kořenovém adresáři vytvořte `/views/index.ejs` zobrazení.</span><span class="sxs-lookup"><span data-stu-id="2e318-184">In the root directory, create the `/views/index.ejs` view.</span></span> <span data-ttu-id="2e318-185">Tato stránka volá vaše **přihlášení** a **odhlášení** metody.</span><span class="sxs-lookup"><span data-stu-id="2e318-185">This page calls your **login** and **logout** methods.</span></span> <span data-ttu-id="2e318-186">Můžete také použít `/views/index.ejs` zobrazení k zaznamenání informací o účtu.</span><span class="sxs-lookup"><span data-stu-id="2e318-186">You also use the `/views/index.ejs` view to capture account information.</span></span> <span data-ttu-id="2e318-187">Můžete využít podmínku `if (!user)` jako uživatel předávány prostřednictvím v požadavku.</span><span class="sxs-lookup"><span data-stu-id="2e318-187">You can use the conditional `if (!user)` as the user being passed through in the request.</span></span> <span data-ttu-id="2e318-188">Je důkaz, že máte přihlášení uživatele.</span><span class="sxs-lookup"><span data-stu-id="2e318-188">It is evidence that you have a user signed in.</span></span>

  ```JavaScript
  <% if (!user) { %>
      <h2>Welcome! Please sign in.</h2>
      <a href="/login">Sign in</a>
  <% } else { %>
      <h2>Hello, <%= user.displayName %>.</h2>
      <a href="/account">Account info</a></br>
      <a href="/logout">Sign out</a>
  <% } %>
  ```

4.  <span data-ttu-id="2e318-189">V kořenovém adresáři vytvořte `/views/account.ejs` zobrazení.</span><span class="sxs-lookup"><span data-stu-id="2e318-189">In the root directory, create the `/views/account.ejs` view.</span></span> <span data-ttu-id="2e318-190">`/views/account.ejs` Zobrazení umožňuje zobrazit další informace, `passport-azuread` převádí na žádost uživatele.</span><span class="sxs-lookup"><span data-stu-id="2e318-190">The `/views/account.ejs` view allows you to view additional information that `passport-azuread` puts in the user request.</span></span>

  ```Javascript
  <% if (!user) { %>
      <h2>Welcome! Please sign in.</h2>
      <a href="/login">Sign in</a>
  <% } else { %>
  <p>displayName: <%= user.displayName %></p>
  <p>givenName: <%= user.name.givenName %></p>
  <p>familyName: <%= user.name.familyName %></p>
  <p>UPN: <%= user._json.upn %></p>
  <p>Profile ID: <%= user.id %></p>
  <p>Full Claimes</p>
  <%- JSON.stringify(user) %>
  <p></p>
  <a href="/logout">Sign out</a>
  <% } %>
  ```

5.  <span data-ttu-id="2e318-191">Přidáte rozložení.</span><span class="sxs-lookup"><span data-stu-id="2e318-191">Add a layout.</span></span> <span data-ttu-id="2e318-192">V kořenovém adresáři vytvořte `/views/layout.ejs` zobrazení.</span><span class="sxs-lookup"><span data-stu-id="2e318-192">In the root directory, create the `/views/layout.ejs` view.</span></span>

  ```HTML

  <!DOCTYPE html>
  <html>
      <head>
          <title>Passport-OpenID Example</title>
      </head>
      <body>
          <% if (!user) { %>
              <p>
              <a href="/">Home</a> |
              <a href="/login">Sign in</a>
              </p>
          <% } else { %>
              <p>
              <a href="/">Home</a> |
              <a href="/account">Account</a> |
              <a href="/logout">Sign out</a>
              </p>
          <% } %>
          <%- body %>
      </body>
  </html>
  ```

6.  <span data-ttu-id="2e318-193">Sestavení a spuštění aplikace, spustit `node app.js`.</span><span class="sxs-lookup"><span data-stu-id="2e318-193">To build and run your app, run `node app.js`.</span></span> <span data-ttu-id="2e318-194">Potom přejděte na `http://localhost:3000`.</span><span class="sxs-lookup"><span data-stu-id="2e318-194">Then, go to `http://localhost:3000`.</span></span>

7.  <span data-ttu-id="2e318-195">Přihlaste se pomocí osobního účtu Microsoft nebo pracovní nebo školní účet.</span><span class="sxs-lookup"><span data-stu-id="2e318-195">Sign in with either a personal Microsoft account or a work or school account.</span></span> <span data-ttu-id="2e318-196">Upozorňujeme, že je v seznamu /account identitu uživatele.</span><span class="sxs-lookup"><span data-stu-id="2e318-196">Note that the user's identity is reflected in the /account list.</span></span> 

<span data-ttu-id="2e318-197">Nyní máte webovou aplikaci, která je zabezpečená službou pomocí standardních protokolů.</span><span class="sxs-lookup"><span data-stu-id="2e318-197">You now have a web app that is secured by using industry standard protocols.</span></span> <span data-ttu-id="2e318-198">Uživatelé ve vaší aplikaci, můžete ověřovat pomocí svoje osobní i pracovní nebo školní účty.</span><span class="sxs-lookup"><span data-stu-id="2e318-198">You can authenticate users in your app by using their personal and work or school accounts.</span></span>

## <a name="next-steps"></a><span data-ttu-id="2e318-199">Další kroky</span><span class="sxs-lookup"><span data-stu-id="2e318-199">Next steps</span></span>
<span data-ttu-id="2e318-200">Pro srovnání je hotová ukázka (bez vašich hodnot nastavení) k dispozici jako [soubor .zip](https://github.com/AzureADQuickStarts/AppModelv2-WebApp-OpenIDConnect-nodejs/archive/complete.zip).</span><span class="sxs-lookup"><span data-stu-id="2e318-200">For reference, the completed sample (without your configuration values) is provided as [a .zip file](https://github.com/AzureADQuickStarts/AppModelv2-WebApp-OpenIDConnect-nodejs/archive/complete.zip).</span></span> <span data-ttu-id="2e318-201">Vám může ho také klonovat z Githubu:</span><span class="sxs-lookup"><span data-stu-id="2e318-201">You also can clone it from GitHub:</span></span>

```git clone --branch complete https://github.com/AzureADQuickStarts/AppModelv2-WebApp-OpenIDConnect-nodejs.git```

<span data-ttu-id="2e318-202">Potom můžete přesunout pokročilejší témata.</span><span class="sxs-lookup"><span data-stu-id="2e318-202">Next, you can move on to more advanced topics.</span></span> <span data-ttu-id="2e318-203">Můžete se pokusit:</span><span class="sxs-lookup"><span data-stu-id="2e318-203">You might want to try:</span></span>

[<span data-ttu-id="2e318-204">Zabezpečit webové rozhraní API Node.js pomocí koncového bodu v2.0</span><span class="sxs-lookup"><span data-stu-id="2e318-204">Secure a Node.js web API by using the v2.0 endpoint</span></span>](active-directory-v2-devquickstarts-node-api.md)

<span data-ttu-id="2e318-205">Zde jsou některé další zdroje informací:</span><span class="sxs-lookup"><span data-stu-id="2e318-205">Here are some additional resources:</span></span>

* [<span data-ttu-id="2e318-206">Příručka vývojáře v2.0 služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="2e318-206">Azure AD v2.0 developer guide</span></span>](active-directory-appmodel-v2-overview.md)
* [<span data-ttu-id="2e318-207">Značka "azure-active-directory" přetečení zásobníku</span><span class="sxs-lookup"><span data-stu-id="2e318-207">Stack Overflow "azure-active-directory" tag</span></span>](http://stackoverflow.com/questions/tagged/azure-active-directory)

### <a name="get-security-updates-for-our-products"></a><span data-ttu-id="2e318-208">Získejte bezpečnostní aktualizace našich produktů</span><span class="sxs-lookup"><span data-stu-id="2e318-208">Get security updates for our products</span></span>
<span data-ttu-id="2e318-209">Doporučujeme registrace oznámení o bezpečnostních incidentech.</span><span class="sxs-lookup"><span data-stu-id="2e318-209">We encourage you to sign up to be notified when security incidents occur.</span></span> <span data-ttu-id="2e318-210">Na [technické oznámení zabezpečení Microsoft](https://technet.microsoft.com/security/dd252948) stránky, odběr výstrah zpravodaje zabezpečení.</span><span class="sxs-lookup"><span data-stu-id="2e318-210">On the [Microsoft Technical Security Notifications](https://technet.microsoft.com/security/dd252948) page, subscribe to Security Advisories Alerts.</span></span>

