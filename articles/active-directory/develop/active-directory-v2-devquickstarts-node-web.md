---
title: "aaaAzure služby Active Directory v2.0 Node.js webové aplikace přihlášení | Microsoft Docs"
description: "Zjistěte, jak toobuild Node.js webová aplikace, který se přihlásí uživatel pomocí účtu Microsoft osobní i pracovní nebo školní účet."
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
ms.openlocfilehash: f8ce6e2b841c215cb14e82bcf444fe849634cc88
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="add-sign-in-tooa-nodejs-web-app"></a><span data-ttu-id="548ae-103">Přidání webové aplikace Node.js tooa přihlášení</span><span class="sxs-lookup"><span data-stu-id="548ae-103">Add sign-in tooa Node.js web app</span></span>

> [!NOTE]
> <span data-ttu-id="548ae-104">Ne všechny funkce a scénáře Azure Active Directory fungovat s koncovým bodem v2.0 hello.</span><span class="sxs-lookup"><span data-stu-id="548ae-104">Not all Azure Active Directory scenarios and features work with hello v2.0 endpoint.</span></span> <span data-ttu-id="548ae-105">toodetermine zda byste měli používat koncového bodu v2.0 hello nebo koncový bod hello verze 1.0, přečtěte si informace o [v2.0 omezení](active-directory-v2-limitations.md).</span><span class="sxs-lookup"><span data-stu-id="548ae-105">toodetermine whether you should use hello v2.0 endpoint or hello v1.0 endpoint, read about [v2.0 limitations](active-directory-v2-limitations.md).</span></span>
> 

<span data-ttu-id="548ae-106">V tomto kurzu používáme Passport toodo hello následující úlohy:</span><span class="sxs-lookup"><span data-stu-id="548ae-106">In this tutorial, we use Passport toodo hello following tasks:</span></span>

* <span data-ttu-id="548ae-107">Ve webové aplikaci Přihlaste se pomocí služby Azure Active Directory (Azure AD) hello uživatele a hello koncového bodu v2.0.</span><span class="sxs-lookup"><span data-stu-id="548ae-107">In a web app, sign in hello user by using Azure Active Directory (Azure AD) and hello v2.0 endpoint.</span></span>
* <span data-ttu-id="548ae-108">Zobrazení informací o uživateli hello.</span><span class="sxs-lookup"><span data-stu-id="548ae-108">Display information about hello user.</span></span>
* <span data-ttu-id="548ae-109">Přihlašovací hello uživatele mimo aplikaci hello.</span><span class="sxs-lookup"><span data-stu-id="548ae-109">Sign hello user out of hello app.</span></span>

<span data-ttu-id="548ae-110">**Passport** je ověřovací middleware pro Node.js.</span><span class="sxs-lookup"><span data-stu-id="548ae-110">**Passport** is authentication middleware for Node.js.</span></span> <span data-ttu-id="548ae-111">Flexibilní a modulární, můžete do jakéhokoli nenápadně vyřadit Passport využívající Express nebo restify webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="548ae-111">Flexible and modular, Passport can be unobtrusively dropped into any Express-based or restify web application.</span></span> <span data-ttu-id="548ae-112">Komplexní sada strategií Passport, podporují ověřování pomocí uživatelského jména a hesla, Facebook, Twitter a další možnosti.</span><span class="sxs-lookup"><span data-stu-id="548ae-112">In Passport, a comprehensive set of strategies support authentication by using a username and password, Facebook, Twitter, or other options.</span></span> <span data-ttu-id="548ae-113">Vyvinuli jsme strategii pro Azure AD.</span><span class="sxs-lookup"><span data-stu-id="548ae-113">We have developed a strategy for Azure AD.</span></span> <span data-ttu-id="548ae-114">V tomto článku jsme ukážeme, jak tooinstall hello modul a poté přidejte hello Azure AD `passport-azure-ad` modulu plug-in.</span><span class="sxs-lookup"><span data-stu-id="548ae-114">In this article, we show you how tooinstall hello module, and then add hello Azure AD `passport-azure-ad` plug-in.</span></span>

## <a name="download"></a><span data-ttu-id="548ae-115">Ke stažení</span><span class="sxs-lookup"><span data-stu-id="548ae-115">Download</span></span>
<span data-ttu-id="548ae-116">Hello kód v tomto kurzu se udržuje [na Githubu](https://github.com/AzureADQuickStarts/AppModelv2-WebApp-OpenIDConnect-nodejs).</span><span class="sxs-lookup"><span data-stu-id="548ae-116">hello code for this tutorial is maintained [on GitHub](https://github.com/AzureADQuickStarts/AppModelv2-WebApp-OpenIDConnect-nodejs).</span></span> <span data-ttu-id="548ae-117">toofollow hello kurzu můžete [stáhnout kostru aplikace hello jako soubor ZIP](https://github.com/AzureADQuickStarts/AppModelv2-WebApp-OpenIDConnect-nodejs/archive/skeleton.zip) nebo hello kostru klonovat:</span><span class="sxs-lookup"><span data-stu-id="548ae-117">toofollow hello tutorial, you can [download hello app's skeleton as a .zip file](https://github.com/AzureADQuickStarts/AppModelv2-WebApp-OpenIDConnect-nodejs/archive/skeleton.zip) or clone hello skeleton:</span></span>

```git clone --branch skeleton https://github.com/AzureADQuickStarts/AppModelv2-WebApp-OpenIDConnect-nodejs.git```

<span data-ttu-id="548ae-118">Také můžete získat aplikace hello dokončit na konci hello tohoto kurzu.</span><span class="sxs-lookup"><span data-stu-id="548ae-118">You also can get hello completed application at hello end of this tutorial.</span></span>

## <a name="1-register-an-app"></a><span data-ttu-id="548ae-119">1: registrace aplikace</span><span class="sxs-lookup"><span data-stu-id="548ae-119">1: Register an app</span></span>
<span data-ttu-id="548ae-120">Vytvoření nové aplikace v [apps.dev.microsoft.com](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList), nebo postupujte podle [tyto podrobné kroky](active-directory-v2-app-registration.md) tooregister aplikace.</span><span class="sxs-lookup"><span data-stu-id="548ae-120">Create a new app at [apps.dev.microsoft.com](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList), or follow [these detailed steps](active-directory-v2-app-registration.md) tooregister an app.</span></span> <span data-ttu-id="548ae-121">Ověřte, že je:</span><span class="sxs-lookup"><span data-stu-id="548ae-121">Make sure you:</span></span>

* <span data-ttu-id="548ae-122">Kopírování hello **Id aplikace** přiřazené tooyour aplikace.</span><span class="sxs-lookup"><span data-stu-id="548ae-122">Copy hello **Application Id** assigned tooyour app.</span></span> <span data-ttu-id="548ae-123">Budete ho potřebovat pro účely tohoto kurzu.</span><span class="sxs-lookup"><span data-stu-id="548ae-123">You need it for this tutorial.</span></span>
* <span data-ttu-id="548ae-124">Přidat hello **webové** platformu pro vaši aplikaci.</span><span class="sxs-lookup"><span data-stu-id="548ae-124">Add hello **Web** platform for your app.</span></span>
* <span data-ttu-id="548ae-125">Kopírování hello **identifikátor URI pro přesměrování** z portálu hello.</span><span class="sxs-lookup"><span data-stu-id="548ae-125">Copy hello **Redirect URI** from hello portal.</span></span> <span data-ttu-id="548ae-126">Musíte použít hello výchozí hodnota URI `urn:ietf:wg:oauth:2.0:oob`.</span><span class="sxs-lookup"><span data-stu-id="548ae-126">You must use hello default URI value of `urn:ietf:wg:oauth:2.0:oob`.</span></span>

## <a name="2-add-prerequisities-tooyour-directory"></a><span data-ttu-id="548ae-127">2: Přidat prerequisities tooyour adresář</span><span class="sxs-lookup"><span data-stu-id="548ae-127">2: Add prerequisities tooyour directory</span></span>
<span data-ttu-id="548ae-128">Na příkazovém řádku změňte adresáře toogo tooyour kořenové složky, pokud si nejste již existuje.</span><span class="sxs-lookup"><span data-stu-id="548ae-128">At a command prompt, change directories toogo tooyour root folder, if you are not already there.</span></span> <span data-ttu-id="548ae-129">Spusťte následující příkazy hello:</span><span class="sxs-lookup"><span data-stu-id="548ae-129">Run hello following commands:</span></span>

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

<span data-ttu-id="548ae-130">Kromě toho používáme `passport-azure-ad` v hello kostře rychlého startu hello:</span><span class="sxs-lookup"><span data-stu-id="548ae-130">In addition, we use `passport-azure-ad` in hello skeleton of hello quickstart:</span></span>

* `npm install passport-azure-ad`

<span data-ttu-id="548ae-131">To nainstaluje knihovny hello který `passport-azure-ad` používá.</span><span class="sxs-lookup"><span data-stu-id="548ae-131">This installs hello libraries that `passport-azure-ad` uses.</span></span>

## <a name="3-set-up-your-app-toouse-hello-passport-node-js-strategy"></a><span data-ttu-id="548ae-132">3: nastavení strategie passport uzlu js hello toouse aplikace</span><span class="sxs-lookup"><span data-stu-id="548ae-132">3: Set up your app toouse hello passport-node-js strategy</span></span>
<span data-ttu-id="548ae-133">Nastavte hello Express middleware toouse hello ověřovacího protokolu OpenID Connect.</span><span class="sxs-lookup"><span data-stu-id="548ae-133">Set up hello Express middleware toouse hello OpenID Connect authentication protocol.</span></span> <span data-ttu-id="548ae-134">Použití požadavků na přihlášení a odhlášení tooissue Passport, spravovat relace hello uživatele a získat informace o uživateli hello, mimo jiné.</span><span class="sxs-lookup"><span data-stu-id="548ae-134">You use Passport tooissue sign-in and sign-out requests, manage hello user's session, and get information about hello user, among other things.</span></span>

1.  <span data-ttu-id="548ae-135">V kořenovém hello hello projektu otevřete hello souboru Config.js.</span><span class="sxs-lookup"><span data-stu-id="548ae-135">In hello root of hello project, open hello Config.js file.</span></span> <span data-ttu-id="548ae-136">V hello `exports.creds` zadejte hodnoty konfigurace vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="548ae-136">In hello `exports.creds` section, enter your app's configuration values.</span></span>
  
  * <span data-ttu-id="548ae-137">`clientID`: hello **Id aplikace** který je přiřazený tooyour aplikace v hello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="548ae-137">`clientID`: hello **Application Id** that's assigned tooyour app in hello Azure portal.</span></span>
  * <span data-ttu-id="548ae-138">`returnURL`: hello **identifikátor URI pro přesměrování** kterou jste zadali v portálu hello.</span><span class="sxs-lookup"><span data-stu-id="548ae-138">`returnURL`: hello **Redirect URI** that you entered in hello portal.</span></span>
  * <span data-ttu-id="548ae-139">`clientSecret`: hello tajný klíč, který jste vygenerovali hello portálu.</span><span class="sxs-lookup"><span data-stu-id="548ae-139">`clientSecret`: hello secret that you generated in hello portal.</span></span>

2.  <span data-ttu-id="548ae-140">V kořenovém hello hello projektu otevřete soubor App.js hello.</span><span class="sxs-lookup"><span data-stu-id="548ae-140">In hello root of hello project, open hello App.js file.</span></span> <span data-ttu-id="548ae-141">tooinvoke hello OIDCStrategy stratey, který se dodává s `passport-azure-ad`, přidejte následující volání hello:</span><span class="sxs-lookup"><span data-stu-id="548ae-141">tooinvoke hello OIDCStrategy stratey, which comes with `passport-azure-ad`, add hello following call:</span></span>

  ```JavaScript
  var OIDCStrategy = require('passport-azure-ad').OIDCStrategy;

  // Add some logging
  var log = bunyan.createLogger({
      name: 'Microsoft OIDC Example Web Application'
  });
  ```

3.  <span data-ttu-id="548ae-142">toohandle vaší žádostí o přihlášení, použijte hello strategie, kterou jste právě přidanou:</span><span class="sxs-lookup"><span data-stu-id="548ae-142">toohandle your sign-in requests, use hello strategy you just referenced:</span></span>

  ```JavaScript
  // Use hello OIDCStrategy within Passport (section 2)
  //
  //   Strategies in Passport require a `validate` function. hello function accepts
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

<span data-ttu-id="548ae-143">Passport používá podobný Princip pro všechny svoje strategie (Twitteru, Facebooku a tak dále).</span><span class="sxs-lookup"><span data-stu-id="548ae-143">Passport uses a similar pattern for all its strategies (Twitter, Facebook, and so on).</span></span> <span data-ttu-id="548ae-144">Vzor toohello řídí všichni tvůrci strategií.</span><span class="sxs-lookup"><span data-stu-id="548ae-144">All strategy writers adhere toohello pattern.</span></span> <span data-ttu-id="548ae-145">Předat hello strategie `function()` token, který používá a `done` jako parametry.</span><span class="sxs-lookup"><span data-stu-id="548ae-145">Pass hello strategy a `function()` that uses a token and `done` as parameters.</span></span> <span data-ttu-id="548ae-146">strategie Hello se vrátí po dělá svou práci.</span><span class="sxs-lookup"><span data-stu-id="548ae-146">hello strategy is returned after it does all its work.</span></span> <span data-ttu-id="548ae-147">Uložení hello uživatele a dočasné ukládání hello token, takže není nutné tooask pro něj znovu.</span><span class="sxs-lookup"><span data-stu-id="548ae-147">Store hello user and stash hello token so you don’t need tooask for it again.</span></span>

  > [!IMPORTANT]
  > <span data-ttu-id="548ae-148">Hello předchozí kód přijme všechny uživatele, který může ověřit tooyour serveru.</span><span class="sxs-lookup"><span data-stu-id="548ae-148">hello preceding code takes any user that can authenticate tooyour server.</span></span> <span data-ttu-id="548ae-149">To se označuje jako Automatická registrace.</span><span class="sxs-lookup"><span data-stu-id="548ae-149">This is known as auto-registration.</span></span> <span data-ttu-id="548ae-150">Na provozním serveru nebude chcete toolet každý, kdo v bez toho, aby předtím prošli registračním procesem, který zvolíte.</span><span class="sxs-lookup"><span data-stu-id="548ae-150">On a production server, you wouldn’t want toolet anyone in without first having them go through a registration process that you choose.</span></span> <span data-ttu-id="548ae-151">Je to obvykle hello vzor, který můžete vidět u uživatelských aplikací.</span><span class="sxs-lookup"><span data-stu-id="548ae-151">This is usually hello pattern that you see in consumer apps.</span></span> <span data-ttu-id="548ae-152">aplikace Hello může povolit tooregister službou Facebook, ale pak musíte tooenter Další informace.</span><span class="sxs-lookup"><span data-stu-id="548ae-152">hello app might allow you tooregister with Facebook, but then it asks you tooenter additional information.</span></span> <span data-ttu-id="548ae-153">Pokud v tomto kurzu nebyly pomocí příkazového řádku programu, může extrahuje hello e-mailu z hello tokenu objekt, který je vrácen.</span><span class="sxs-lookup"><span data-stu-id="548ae-153">If you weren't using a command-line program for this tutorial, you could extract hello email from hello token object that is returned.</span></span> <span data-ttu-id="548ae-154">Potom může zobrazit dotaz hello uživatele tooenter Další informace.</span><span class="sxs-lookup"><span data-stu-id="548ae-154">Then, you might ask hello user tooenter additional information.</span></span> <span data-ttu-id="548ae-155">Protože se jedná o testovací server, můžete přidat uživatele hello přímo toohello databázi v paměti.</span><span class="sxs-lookup"><span data-stu-id="548ae-155">Because this is a test server, you add hello user directly toohello in-memory database.</span></span>
  > 
  > 

4.  <span data-ttu-id="548ae-156">Přidejte metody hello použít tookeep sledování uživatelů, kteří se přihlásili, jak vyžaduje Passport.</span><span class="sxs-lookup"><span data-stu-id="548ae-156">Add hello methods that you use tookeep track of users who are signed in, as required by Passport.</span></span> <span data-ttu-id="548ae-157">To zahrnuje serializaci a deserializaci informací o uživateli hello:</span><span class="sxs-lookup"><span data-stu-id="548ae-157">This includes serializing and deserializing hello user's information:</span></span>

  ```JavaScript

  // Passport session setup (section 2)

  //   toosupport persistent login sessions, Passport needs toobe able to
  //   serialize users into, and deserialize users out of, hello session. Typically,
  //   this is as simple as storing hello user ID when serializing, and finding
  //   hello user by ID when deserializing.
  passport.serializeUser(function(user, done) {
    done(null, user.email);
  });

  passport.deserializeUser(function(id, done) {
    findByEmail(id, function (err, user) {
      done(err, user);
    });
  });

  // Array toohold signed-in users
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

5.  <span data-ttu-id="548ae-158">Přidejte hello kód, který načte hello modulu Express.</span><span class="sxs-lookup"><span data-stu-id="548ae-158">Add hello code that loads hello Express engine.</span></span> <span data-ttu-id="548ae-159">Použijte výchozí /views hello a /routes vzor, který Express poskytuje:</span><span class="sxs-lookup"><span data-stu-id="548ae-159">You use hello default /views and /routes pattern that Express provides:</span></span>

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
    // Initialize Passport!  Also use passport.session() middleware, toosupport
    // persistent login sessions (recommended).
    app.use(passport.initialize());
    app.use(passport.session());
    app.use(app.router);
    app.use(express.static(__dirname + '/../../public'));
  });

  ```

6.  <span data-ttu-id="548ae-160">Přidat hello POST tras, že můžete předat hello skutečné žádostí o přihlášení toohello `passport-azure-ad` modul:</span><span class="sxs-lookup"><span data-stu-id="548ae-160">Add hello POST routes that hand off hello actual sign-in requests toohello `passport-azure-ad` engine:</span></span>

  ```JavaScript

  // Auth routes (section 3)

  // GET /auth/openid
  //   Use passport.authenticate() as route middleware tooauthenticate the
  //   request. hello first step in OpenID authentication involves redirecting
  //   hello user toohello user's OpenID provider. After authenticating, hello OpenID
  //   provider redirects hello user back toothis application at
  //   /auth/openid/return.

  app.get('/auth/openid',
    passport.authenticate('azuread-openidconnect', { failureRedirect: '/login' }),
    function(req, res) {
      log.info('Authentication was called in hello sample');
      res.redirect('/');
    });

  // GET /auth/openid/return
  //   Use passport.authenticate() as route middleware tooauthenticate the
  //   request. If authentication fails, hello user is redirected back toothe
  //   sign-in page. Otherwise, hello primary route function is called.
  //   In this example, it redirects hello user toohello home page.
  app.get('/auth/openid/return',
    passport.authenticate('azuread-openidconnect', { failureRedirect: '/login' }),
    function(req, res) {

      res.redirect('/');
    });

  // POST /auth/openid/return
  //   Use passport.authenticate() as route middleware tooauthenticate the
  //   request. If authentication fails, hello user is redirected back toothe
  //   sign-in page. Otherwise, hello primary route function is called. 
  //   In this example, it redirects hello user toohello home page.

  app.post('/auth/openid/return',
    passport.authenticate('azuread-openidconnect', { failureRedirect: '/login' }),
    function(req, res) {

      res.redirect('/');
    });
  ```

## <a name="4-use-passport-tooissue-sign-in-and-sign-out-requests-tooazure-ad"></a><span data-ttu-id="548ae-161">4: použití Passport tooissue přihlášení a odhlášení požadavky tooAzure AD</span><span class="sxs-lookup"><span data-stu-id="548ae-161">4: Use Passport tooissue sign-in and sign-out requests tooAzure AD</span></span>
<span data-ttu-id="548ae-162">Aplikace je teď nastavený toocommunicate s hello koncového bodu v2.0 pomocí ověřovacího protokolu OpenID Connect hello.</span><span class="sxs-lookup"><span data-stu-id="548ae-162">Your app is now set up toocommunicate with hello v2.0 endpoint by using hello OpenID Connect authentication protocol.</span></span> <span data-ttu-id="548ae-163">Hello `passport-azure-ad` strategie postará všechny podrobnosti hello věnujte zpráv ověřování, ověřování tokenů z Azure AD a údržbě hello uživatelské relace.</span><span class="sxs-lookup"><span data-stu-id="548ae-163">hello `passport-azure-ad` strategy takes care of all hello details of crafting authentication messages, validating tokens from Azure AD, and maintaining hello user session.</span></span> <span data-ttu-id="548ae-164">Všechny, který se nechá toodo je toogive uživatelům způsob toosign v a přihlaste se na více systémů a toogather Další informace o hello uživatele, který je přihlášený.</span><span class="sxs-lookup"><span data-stu-id="548ae-164">All that is left toodo is toogive your users a way toosign in and sign out, and toogather more information about hello user who is signed in.</span></span>

1.  <span data-ttu-id="548ae-165">Přidat hello **výchozí**, **přihlášení**, **účet**, a **odhlášení** souboru App.js tooyour metody:</span><span class="sxs-lookup"><span data-stu-id="548ae-165">Add hello **default**, **login**, **account**, and **logout** methods tooyour App.js file:</span></span>

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
      log.info('Login was called in hello sample');
      res.redirect('/');
  });

  app.get('/logout', function(req, res){
    req.logout();
    res.redirect('/');
  });

  ```

  <span data-ttu-id="548ae-166">Zde jsou hello podrobnosti:</span><span class="sxs-lookup"><span data-stu-id="548ae-166">Here are hello details:</span></span>
    
    * <span data-ttu-id="548ae-167">Hello `/` trasa přesměruje toohello index.ejs zobrazení.</span><span class="sxs-lookup"><span data-stu-id="548ae-167">hello `/` route redirects toohello index.ejs view.</span></span> <span data-ttu-id="548ae-168">Pak předá hello uživatele v požadavku hello (pokud existuje).</span><span class="sxs-lookup"><span data-stu-id="548ae-168">It passes hello user in hello request (if it exists).</span></span>
    * <span data-ttu-id="548ae-169">Hello `/account` směrovat nejprve *zajistí, že jsou ověří* (můžete implementovat, hello následující kód).</span><span class="sxs-lookup"><span data-stu-id="548ae-169">hello `/account` route first *ensures that you are authenticated* (you implement that in hello following code).</span></span> <span data-ttu-id="548ae-170">Pak předá hello uživatele v požadavku hello.</span><span class="sxs-lookup"><span data-stu-id="548ae-170">Then, it passes hello user in hello request.</span></span> <span data-ttu-id="548ae-171">Toto je, abyste získali další informace o uživateli hello.</span><span class="sxs-lookup"><span data-stu-id="548ae-171">This is so you can get more information about hello user.</span></span>
    * <span data-ttu-id="548ae-172">Hello `/login` směrování volání vaše `azuread-openidconnect` authenticator z `passport-azuread`.</span><span class="sxs-lookup"><span data-stu-id="548ae-172">hello `/login` route calls your `azuread-openidconnect` authenticator from `passport-azuread`.</span></span> <span data-ttu-id="548ae-173">Pokud není úspěšné, je přesměrován zpět uživatele hello příliš`/login`.</span><span class="sxs-lookup"><span data-stu-id="548ae-173">If that doesn't succeed, it redirects hello user back too`/login`.</span></span>
    * <span data-ttu-id="548ae-174">Hello `/logout` trasy volá hello logout.ejs zobrazení (a směrování).</span><span class="sxs-lookup"><span data-stu-id="548ae-174">hello `/logout` route calls hello logout.ejs view (and route).</span></span> <span data-ttu-id="548ae-175">Zruší soubory cookie a pak vrátí hello back tooindex.ejs uživatele.</span><span class="sxs-lookup"><span data-stu-id="548ae-175">This clears cookies, and then returns hello user back tooindex.ejs.</span></span>

2.  <span data-ttu-id="548ae-176">Přidat hello **EnsureAuthenticated** metoda, která jste použili výše v `/account`:</span><span class="sxs-lookup"><span data-stu-id="548ae-176">Add hello **EnsureAuthenticated** method that you used earlier in `/account`:</span></span>

  ```JavaScript

  // Route middleware tooensure hello user is authenticated (section 4)

  //   Use this route middleware on any resource that needs toobe protected. If
  //   hello request is authenticated (typically via a persistent login session),
  //   hello request proceeds. Otherwise, hello user is redirected toothe
  //   sign-in page.
  function ensureAuthenticated(req, res, next) {
    if (req.isAuthenticated()) { return next(); }
    res.redirect('/login')
  }

  ```

3.  <span data-ttu-id="548ae-177">V App.js vytvořte hello serveru:</span><span class="sxs-lookup"><span data-stu-id="548ae-177">In App.js, create hello server:</span></span>

  ```JavaScript

  app.listen(3000);

  ```


## <a name="5-create-hello-views-and-routes-in-express-that-you-show-your-user-on-hello-website"></a><span data-ttu-id="548ae-178">5: vytvoření hello zobrazení a trasy v Express uživatelům zobrazí na webu hello</span><span class="sxs-lookup"><span data-stu-id="548ae-178">5: Create hello views and routes in Express that you show your user on hello website</span></span>
<span data-ttu-id="548ae-179">Přidejte hello trasy a zobrazení, které se zobrazí informace o toohello uživatele.</span><span class="sxs-lookup"><span data-stu-id="548ae-179">Add hello routes and views that show information toohello user.</span></span> <span data-ttu-id="548ae-180">Hello trasy a zobrazení, zpracovávají také dříve hello `/logout` a `/login` tras, které jste vytvořili.</span><span class="sxs-lookup"><span data-stu-id="548ae-180">hello routes and views also handle hello `/logout` and `/login` routes that you created.</span></span>

1. <span data-ttu-id="548ae-181">V kořenovém adresáři hello, vytvořte hello `/routes/index.js` trasy.</span><span class="sxs-lookup"><span data-stu-id="548ae-181">In hello root directory, create hello `/routes/index.js` route.</span></span>

  ```JavaScript

  /*
  * GET home page.
  */

  exports.index = function(req, res){
    res.render('index', { title: 'Express' });
  };
  ```

2.  <span data-ttu-id="548ae-182">V kořenovém adresáři hello, vytvořte hello `/routes/user.js` trasy.</span><span class="sxs-lookup"><span data-stu-id="548ae-182">In hello root directory, create hello `/routes/user.js` route.</span></span>

  ```JavaScript

  /*
  * GET users listing.
  */

  exports.list = function(req, res){
    res.send("respond with a resource");
  };
  ```

  <span data-ttu-id="548ae-183">`/routes/index.js`a `/routes/user.js` jsou jednoduché trasy, které předávají hello požadavek tooyour zobrazení, včetně hello uživatele, pokud je k dispozici.</span><span class="sxs-lookup"><span data-stu-id="548ae-183">`/routes/index.js` and `/routes/user.js` are simple routes that pass along hello request tooyour views, including hello user, if present.</span></span>

3.  <span data-ttu-id="548ae-184">V kořenovém adresáři hello, vytvořte hello `/views/index.ejs` zobrazení.</span><span class="sxs-lookup"><span data-stu-id="548ae-184">In hello root directory, create hello `/views/index.ejs` view.</span></span> <span data-ttu-id="548ae-185">Tato stránka volá vaše **přihlášení** a **odhlášení** metody.</span><span class="sxs-lookup"><span data-stu-id="548ae-185">This page calls your **login** and **logout** methods.</span></span> <span data-ttu-id="548ae-186">Můžete také použít hello `/views/index.ejs` zobrazit informace o účtu toocapture.</span><span class="sxs-lookup"><span data-stu-id="548ae-186">You also use hello `/views/index.ejs` view toocapture account information.</span></span> <span data-ttu-id="548ae-187">Můžete použít hello podmíněného `if (!user)` jako uživatel hello předávány prostřednictvím v žádosti o hello.</span><span class="sxs-lookup"><span data-stu-id="548ae-187">You can use hello conditional `if (!user)` as hello user being passed through in hello request.</span></span> <span data-ttu-id="548ae-188">Je důkaz, že máte přihlášení uživatele.</span><span class="sxs-lookup"><span data-stu-id="548ae-188">It is evidence that you have a user signed in.</span></span>

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

4.  <span data-ttu-id="548ae-189">V kořenovém adresáři hello, vytvořte hello `/views/account.ejs` zobrazení.</span><span class="sxs-lookup"><span data-stu-id="548ae-189">In hello root directory, create hello `/views/account.ejs` view.</span></span> <span data-ttu-id="548ae-190">Hello `/views/account.ejs` zobrazení můžete další informace o tooview, `passport-azuread` převádí na žádost uživatele hello.</span><span class="sxs-lookup"><span data-stu-id="548ae-190">hello `/views/account.ejs` view allows you tooview additional information that `passport-azuread` puts in hello user request.</span></span>

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

5.  <span data-ttu-id="548ae-191">Přidáte rozložení.</span><span class="sxs-lookup"><span data-stu-id="548ae-191">Add a layout.</span></span> <span data-ttu-id="548ae-192">V kořenovém adresáři hello, vytvořte hello `/views/layout.ejs` zobrazení.</span><span class="sxs-lookup"><span data-stu-id="548ae-192">In hello root directory, create hello `/views/layout.ejs` view.</span></span>

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

6.  <span data-ttu-id="548ae-193">toobuild a spuštění aplikace, spustit `node app.js`.</span><span class="sxs-lookup"><span data-stu-id="548ae-193">toobuild and run your app, run `node app.js`.</span></span> <span data-ttu-id="548ae-194">Pak přejděte příliš`http://localhost:3000`.</span><span class="sxs-lookup"><span data-stu-id="548ae-194">Then, go too`http://localhost:3000`.</span></span>

7.  <span data-ttu-id="548ae-195">Přihlaste se pomocí osobního účtu Microsoft nebo pracovní nebo školní účet.</span><span class="sxs-lookup"><span data-stu-id="548ae-195">Sign in with either a personal Microsoft account or a work or school account.</span></span> <span data-ttu-id="548ae-196">Upozorňujeme, že identita hello uživatele v seznamu /account hello.</span><span class="sxs-lookup"><span data-stu-id="548ae-196">Note that hello user's identity is reflected in hello /account list.</span></span> 

<span data-ttu-id="548ae-197">Nyní máte webovou aplikaci, která je zabezpečená službou pomocí standardních protokolů.</span><span class="sxs-lookup"><span data-stu-id="548ae-197">You now have a web app that is secured by using industry standard protocols.</span></span> <span data-ttu-id="548ae-198">Uživatelé ve vaší aplikaci, můžete ověřovat pomocí svoje osobní i pracovní nebo školní účty.</span><span class="sxs-lookup"><span data-stu-id="548ae-198">You can authenticate users in your app by using their personal and work or school accounts.</span></span>

## <a name="next-steps"></a><span data-ttu-id="548ae-199">Další kroky</span><span class="sxs-lookup"><span data-stu-id="548ae-199">Next steps</span></span>
<span data-ttu-id="548ae-200">Pro srovnání je hello dokončit ukázka (bez vašich hodnot nastavení) k dispozici jako [soubor .zip](https://github.com/AzureADQuickStarts/AppModelv2-WebApp-OpenIDConnect-nodejs/archive/complete.zip).</span><span class="sxs-lookup"><span data-stu-id="548ae-200">For reference, hello completed sample (without your configuration values) is provided as [a .zip file](https://github.com/AzureADQuickStarts/AppModelv2-WebApp-OpenIDConnect-nodejs/archive/complete.zip).</span></span> <span data-ttu-id="548ae-201">Vám může ho také klonovat z Githubu:</span><span class="sxs-lookup"><span data-stu-id="548ae-201">You also can clone it from GitHub:</span></span>

```git clone --branch complete https://github.com/AzureADQuickStarts/AppModelv2-WebApp-OpenIDConnect-nodejs.git```

<span data-ttu-id="548ae-202">Potom můžete přesunout na toomore advanced témata.</span><span class="sxs-lookup"><span data-stu-id="548ae-202">Next, you can move on toomore advanced topics.</span></span> <span data-ttu-id="548ae-203">Můžete chtít tootry:</span><span class="sxs-lookup"><span data-stu-id="548ae-203">You might want tootry:</span></span>

[<span data-ttu-id="548ae-204">Zabezpečit webové rozhraní API Node.js pomocí koncového bodu v2.0 hello</span><span class="sxs-lookup"><span data-stu-id="548ae-204">Secure a Node.js web API by using hello v2.0 endpoint</span></span>](active-directory-v2-devquickstarts-node-api.md)

<span data-ttu-id="548ae-205">Zde jsou některé další zdroje informací:</span><span class="sxs-lookup"><span data-stu-id="548ae-205">Here are some additional resources:</span></span>

* [<span data-ttu-id="548ae-206">Příručka vývojáře v2.0 služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="548ae-206">Azure AD v2.0 developer guide</span></span>](active-directory-appmodel-v2-overview.md)
* [<span data-ttu-id="548ae-207">Značka "azure-active-directory" přetečení zásobníku</span><span class="sxs-lookup"><span data-stu-id="548ae-207">Stack Overflow "azure-active-directory" tag</span></span>](http://stackoverflow.com/questions/tagged/azure-active-directory)

### <a name="get-security-updates-for-our-products"></a><span data-ttu-id="548ae-208">Získejte bezpečnostní aktualizace našich produktů</span><span class="sxs-lookup"><span data-stu-id="548ae-208">Get security updates for our products</span></span>
<span data-ttu-id="548ae-209">Doporučujeme toosign až toobe upozorněni, když bezpečnostních incidentech.</span><span class="sxs-lookup"><span data-stu-id="548ae-209">We encourage you toosign up toobe notified when security incidents occur.</span></span> <span data-ttu-id="548ae-210">Na hello [technické oznámení zabezpečení Microsoft](https://technet.microsoft.com/security/dd252948) stránky, přihlášení k odběru tooSecurity zpravodaje výstrahy.</span><span class="sxs-lookup"><span data-stu-id="548ae-210">On hello [Microsoft Technical Security Notifications](https://technet.microsoft.com/security/dd252948) page, subscribe tooSecurity Advisories Alerts.</span></span>

