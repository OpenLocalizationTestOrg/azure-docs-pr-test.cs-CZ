---
title: "aaaGetting Začínáme s Azure AD přihlášení a odhlášení pomocí Node.js | Microsoft Docs"
description: "Zjistěte, jak toobuild Node.js Express MVC webová aplikace, který se integruje s Azure AD pro přihlášení."
services: active-directory
documentationcenter: nodejs
author: navyasric
manager: mbaldwin
editor: 
ms.assetid: 81deecec-dbe2-4e75-8bc0-cf3788645f99
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: javascript
ms.topic: article
ms.date: 01/07/2017
ms.author: nacanuma
ms.custom: aaddev
ms.openlocfilehash: 26481899c74741743b947bd891b65ff24ffc43c6
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="nodejs-web-app-sign-in-and-sign-out-with-azure-ad"></a><span data-ttu-id="d6dae-103">Webové aplikace Node.js přihlášení a odhlášení s Azure AD</span><span class="sxs-lookup"><span data-stu-id="d6dae-103">Node.js web app sign-in and sign-out with Azure AD</span></span>
<span data-ttu-id="d6dae-104">Tady používáme Passport:</span><span class="sxs-lookup"><span data-stu-id="d6dae-104">Here we use Passport to:</span></span>

* <span data-ttu-id="d6dae-105">Přihlašovací hello uživatele v aplikaci toohello službou Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="d6dae-105">Sign hello user in toohello app with Azure Active Directory (Azure AD).</span></span>
* <span data-ttu-id="d6dae-106">Zobrazení informací o uživateli hello.</span><span class="sxs-lookup"><span data-stu-id="d6dae-106">Display information about hello user.</span></span>
* <span data-ttu-id="d6dae-107">Přihlašovací hello uživatele mimo aplikaci hello.</span><span class="sxs-lookup"><span data-stu-id="d6dae-107">Sign hello user out of hello app.</span></span>

<span data-ttu-id="d6dae-108">Passport je ověřovací middleware pro Node.js.</span><span class="sxs-lookup"><span data-stu-id="d6dae-108">Passport is authentication middleware for Node.js.</span></span> <span data-ttu-id="d6dae-109">Flexibilní a modulární, Passport lze snadno vyřadit v tooany využívající Express nebo restify webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="d6dae-109">Flexible and modular, Passport can be unobtrusively dropped in tooany Express-based or restify web application.</span></span> <span data-ttu-id="d6dae-110">Komplexní sada strategií podporují ověřování pomocí uživatelského jména a hesla, Facebook, Twitter a další.</span><span class="sxs-lookup"><span data-stu-id="d6dae-110">A comprehensive set of strategies support authentication that uses a username and password, Facebook, Twitter, and more.</span></span> <span data-ttu-id="d6dae-111">Vyvinuli jsme strategii pro Microsoft Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="d6dae-111">We have developed a strategy for Microsoft Azure Active Directory.</span></span> <span data-ttu-id="d6dae-112">Jsme nainstalujete tento modul a poté přidejte hello Microsoft Azure Active Directory `passport-azure-ad` modulu plug-in.</span><span class="sxs-lookup"><span data-stu-id="d6dae-112">We install this module and then add hello Microsoft Azure Active Directory `passport-azure-ad` plug-in.</span></span>

<span data-ttu-id="d6dae-113">toodo hello tento, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="d6dae-113">toodo this, take hello following steps:</span></span>

1. <span data-ttu-id="d6dae-114">Zaregistrujte aplikaci.</span><span class="sxs-lookup"><span data-stu-id="d6dae-114">Register an app.</span></span>
2. <span data-ttu-id="d6dae-115">Nastavení vaší aplikace hello toouse `passport-azure-ad` strategie.</span><span class="sxs-lookup"><span data-stu-id="d6dae-115">Set up your app toouse hello `passport-azure-ad` strategy.</span></span>
3. <span data-ttu-id="d6dae-116">Použijte Passport tooissue-přihlášení a odhlášení požadavky tooAzure AD.</span><span class="sxs-lookup"><span data-stu-id="d6dae-116">Use Passport tooissue sign-in and sign-out requests tooAzure AD.</span></span>
4. <span data-ttu-id="d6dae-117">Tisknout data o uživateli hello.</span><span class="sxs-lookup"><span data-stu-id="d6dae-117">Print data about hello user.</span></span>

<span data-ttu-id="d6dae-118">Hello kód v tomto kurzu se udržuje [na Githubu](https://github.com/AzureADQuickStarts/WebApp-OpenIDConnect-NodeJS).</span><span class="sxs-lookup"><span data-stu-id="d6dae-118">hello code for this tutorial is maintained [on GitHub](https://github.com/AzureADQuickStarts/WebApp-OpenIDConnect-NodeJS).</span></span>  <span data-ttu-id="d6dae-119">toofollow společně, můžete [stáhnout kostru aplikace hello jako soubor ZIP](https://github.com/AzureADQuickStarts/WebApp-OpenIDConnect-NodeJS/archive/skeleton.zip) nebo hello kostru klonovat:</span><span class="sxs-lookup"><span data-stu-id="d6dae-119">toofollow along, you can [download hello app's skeleton as a .zip file](https://github.com/AzureADQuickStarts/WebApp-OpenIDConnect-NodeJS/archive/skeleton.zip) or clone hello skeleton:</span></span>

```git clone --branch skeleton https://github.com/AzureADQuickStarts/WebApp-OpenIDConnect-NodeJS.git```

<span data-ttu-id="d6dae-120">aplikace Hello dokončit, je k dispozici na konci hello také tohoto kurzu.</span><span class="sxs-lookup"><span data-stu-id="d6dae-120">hello completed application is provided at hello end of this tutorial as well.</span></span>

## <a name="step-1-register-an-app"></a><span data-ttu-id="d6dae-121">Krok 1: Registrace aplikace</span><span class="sxs-lookup"><span data-stu-id="d6dae-121">Step 1: Register an app</span></span>
1. <span data-ttu-id="d6dae-122">Přihlaste se toohello [portál Azure](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="d6dae-122">Sign in toohello [Azure portal](https://portal.azure.com).</span></span>

2. <span data-ttu-id="d6dae-123">V nabídce hello hello horní části stránky hello vyberte svůj účet.</span><span class="sxs-lookup"><span data-stu-id="d6dae-123">In hello menu at hello top of hello page, select your account.</span></span> <span data-ttu-id="d6dae-124">V části hello **Directory** vyberte místo, kam chcete tooregister klienta služby Active Directory hello vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="d6dae-124">Under hello **Directory** list, choose hello Active Directory tenant where you want tooregister your application.</span></span>

3. <span data-ttu-id="d6dae-125">Vyberte **více služeb** v hello nabídky na hello levé strany úvodní obrazovka a pak vyberte **Azure Active Directory**.</span><span class="sxs-lookup"><span data-stu-id="d6dae-125">Select **More Services** in hello menu on hello left side of hello screen, and then select **Azure Active Directory**.</span></span>

4. <span data-ttu-id="d6dae-126">Vyberte **registrace aplikace**a potom vyberte **přidat**.</span><span class="sxs-lookup"><span data-stu-id="d6dae-126">Select **App registrations**, and then select **Add**.</span></span>

5. <span data-ttu-id="d6dae-127">Postupujte podle pokynů toocreate hello **webové aplikace** nebo **WebAPI**.</span><span class="sxs-lookup"><span data-stu-id="d6dae-127">Follow hello prompts toocreate a **Web Application** and/or **WebAPI**.</span></span>
  * <span data-ttu-id="d6dae-128">Hello **název** z hello aplikace popisuje toousers vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="d6dae-128">hello **name** of hello application describes your application toousers.</span></span>

  * <span data-ttu-id="d6dae-129">Hello **přihlašovací adresa URL** je hello základní adresu URL aplikace.</span><span class="sxs-lookup"><span data-stu-id="d6dae-129">hello **Sign-On URL** is hello base URL of your app.</span></span>  <span data-ttu-id="d6dae-130">Hello kostru je výchozí hodnota je ' http://localhost: 3000/auth/openid nebo vrátit hodnotu.</span><span class="sxs-lookup"><span data-stu-id="d6dae-130">hello skeleton's default is `http://localhost:3000/auth/openid/return`\`.</span></span>

6. <span data-ttu-id="d6dae-131">Po registraci, Azure AD přiřadí vaší aplikace ID jedinečný aplikace.</span><span class="sxs-lookup"><span data-stu-id="d6dae-131">After you register, Azure AD assigns your app a unique application ID.</span></span> <span data-ttu-id="d6dae-132">Je třeba tuto hodnotu v hello následující oddíly, takže zkopírovat ze stránky aplikace hello.</span><span class="sxs-lookup"><span data-stu-id="d6dae-132">You need this value in hello following sections, so copy it from hello application page.</span></span>
7. <span data-ttu-id="d6dae-133">Z hello **nastavení** -> **vlastnosti** stránky pro aplikace, aktualizujte hello identifikátor ID URI aplikace.</span><span class="sxs-lookup"><span data-stu-id="d6dae-133">From hello **Settings** -> **Properties** page for your application, update hello App ID URI.</span></span> <span data-ttu-id="d6dae-134">Hello **identifikátor ID URI aplikace** je jedinečný identifikátor pro vaši aplikaci.</span><span class="sxs-lookup"><span data-stu-id="d6dae-134">hello **App ID URI** is a unique identifier for your application.</span></span> <span data-ttu-id="d6dae-135">konvence Hello je formát hello toouse `https://<tenant-domain>/<app-name>`, například: `https://contoso.onmicrosoft.com/my-first-aad-app`.</span><span class="sxs-lookup"><span data-stu-id="d6dae-135">hello convention is toouse hello format `https://<tenant-domain>/<app-name>`, for example: `https://contoso.onmicrosoft.com/my-first-aad-app`.</span></span>

## <a name="step-2-add-prerequisites-tooyour-directory"></a><span data-ttu-id="d6dae-136">Krok 2: Přidejte adresář tooyour požadavky</span><span class="sxs-lookup"><span data-stu-id="d6dae-136">Step 2: Add prerequisites tooyour directory</span></span>
1. <span data-ttu-id="d6dae-137">Z příkazového řádku hello, změňte adresáře tooyour kořenové složky, pokud si nejste již existuje, a pak spusťte hello následující příkazy:</span><span class="sxs-lookup"><span data-stu-id="d6dae-137">From hello command line, change directories tooyour root folder if you're not already there, and then run hello following commands:</span></span>

    * `npm install express`
    * `npm install ejs`
    * `npm install ejs-locals`
    * `npm install restify`
    * `npm install mongoose`
    * `npm install bunyan`
    * `npm install assert-plus`
    * `npm install passport`

2. <span data-ttu-id="d6dae-138">Kromě toho musíte `passport-azure-ad`:</span><span class="sxs-lookup"><span data-stu-id="d6dae-138">In addition, you need `passport-azure-ad`:</span></span>
    * `npm install passport-azure-ad`

<span data-ttu-id="d6dae-139">To nainstaluje knihovny hello který `passport-azure-ad` závisí na.</span><span class="sxs-lookup"><span data-stu-id="d6dae-139">This installs hello libraries that `passport-azure-ad` depends on.</span></span>

## <a name="step-3-set-up-your-app-toouse-hello-passport-node-js-strategy"></a><span data-ttu-id="d6dae-140">Krok 3: Nastavení strategie passport uzlu js hello toouse aplikace</span><span class="sxs-lookup"><span data-stu-id="d6dae-140">Step 3: Set up your app toouse hello passport-node-js strategy</span></span>
<span data-ttu-id="d6dae-141">Zde jsme nakonfigurovat Express toouse hello ověřovacího protokolu OpenID Connect.</span><span class="sxs-lookup"><span data-stu-id="d6dae-141">Here, we configure Express toouse hello OpenID Connect authentication protocol.</span></span>  <span data-ttu-id="d6dae-142">Passport je použité toodo různé položky, včetně požadavků na přihlášení a odhlášení problém, spravovat relace hello uživatele a získat informace o uživateli hello.</span><span class="sxs-lookup"><span data-stu-id="d6dae-142">Passport is used toodo various things, including issue sign-in and sign-out requests, manage hello user's session, and get information about hello user.</span></span>

1. <span data-ttu-id="d6dae-143">toobegin, otevřete hello `config.js` souboru v kořenovém hello hello projektu a potom zadejte hodnoty konfigurace vaší aplikace v hello `exports.creds` části.</span><span class="sxs-lookup"><span data-stu-id="d6dae-143">toobegin, open hello `config.js` file at hello root of hello project, and then enter your app's configuration values in hello `exports.creds` section.</span></span>

  * <span data-ttu-id="d6dae-144">Hello `clientID` je hello **Id aplikace** který je přiřazený tooyour aplikace v portálu pro registraci hello.</span><span class="sxs-lookup"><span data-stu-id="d6dae-144">hello `clientID` is hello **Application Id** that's assigned tooyour app in hello registration portal.</span></span>

  * <span data-ttu-id="d6dae-145">Hello `returnURL` je hello **identifikátor Uri pro přesměrování** kterou jste zadali v portálu hello.</span><span class="sxs-lookup"><span data-stu-id="d6dae-145">hello `returnURL` is hello **Redirect Uri** that you entered in hello portal.</span></span>

  * <span data-ttu-id="d6dae-146">Hello `clientSecret` je hello tajný klíč, který jste vygenerovali hello portálu.</span><span class="sxs-lookup"><span data-stu-id="d6dae-146">hello `clientSecret` is hello secret that you generated in hello portal.</span></span>

2. <span data-ttu-id="d6dae-147">Dále otevřete hello `app.js` soubor v kořenovém hello hello projektu.</span><span class="sxs-lookup"><span data-stu-id="d6dae-147">Next, open hello `app.js` file in hello root of hello project.</span></span> <span data-ttu-id="d6dae-148">Pak přidejte následující volání tooinvoke hello hello `OIDCStrategy` strategie, která se dodává s `passport-azure-ad`.</span><span class="sxs-lookup"><span data-stu-id="d6dae-148">Then add hello following call tooinvoke hello `OIDCStrategy` strategy that comes with `passport-azure-ad`.</span></span>

    ```JavaScript
    var OIDCStrategy = require('passport-azure-ad').OIDCStrategy;

    // add a logger

    var log = bunyan.createLogger({
    name: 'Microsoft OIDC Example Web Application'
    });
    ```

3. <span data-ttu-id="d6dae-149">Potom použijte hello strategii jsme právě přidanou toohandle naše žádostí o přihlášení.</span><span class="sxs-lookup"><span data-stu-id="d6dae-149">After that, use hello strategy we just referenced toohandle our sign-in requests.</span></span>

    ```JavaScript
    // Use hello OIDCStrategy within Passport. (Section 2)
    //
    //   Strategies in passport require a `validate` function that accepts
    //   credentials (in this case, an OpenID identifier), and invokes a callback
    //   with a user object.
    passport.use(new OIDCStrategy({
        callbackURL: config.creds.returnURL,
        realm: config.creds.realm,
        clientID: config.creds.clientID,
        clientSecret: config.creds.clientSecret,
        oidcIssuer: config.creds.issuer,
        identityMetadata: config.creds.identityMetadata,
        skipUserProfile: config.creds.skipUserProfile,
        responseType: config.creds.responseType,
        responseMode: config.creds.responseMode
    },
    function(iss, sub, profile, accessToken, refreshToken, done) {
        if (!profile.email) {
        return done(new Error("No email found"), null);
        }
        // asynchronous verification, for effect...
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
<span data-ttu-id="d6dae-150">Passport používá podobný Princip pro všechny svoje strategie (Twitteru, Facebooku a tak dále), které řídí všichni autoři strategií k.</span><span class="sxs-lookup"><span data-stu-id="d6dae-150">Passport uses a similar pattern for all its strategies (Twitter, Facebook, and so on) that all strategy writers adhere to.</span></span> <span data-ttu-id="d6dae-151">Prohlížení hello strategie, uvidíte, že jsme předat funkci, která má token a done jako parametry hello.</span><span class="sxs-lookup"><span data-stu-id="d6dae-151">Looking at hello strategy, you see that we pass it a function that has a token and a done as hello parameters.</span></span> <span data-ttu-id="d6dae-152">po jeho činnosti provede se dodává zpět toous strategie Hello.</span><span class="sxs-lookup"><span data-stu-id="d6dae-152">hello strategy comes back toous after it does its work.</span></span> <span data-ttu-id="d6dae-153">Potom chceme toostore hello uživatele a dočasné ukládání hello token, společnost Microsoft nepotřebuje tooask pro něj znovu.</span><span class="sxs-lookup"><span data-stu-id="d6dae-153">Then we want toostore hello user and stash hello token so we don't need tooask for it again.</span></span>

> [!IMPORTANT]
<span data-ttu-id="d6dae-154">Předchozí kód Hello trvá každý uživatel, který se stane tooauthenticate tooour serveru.</span><span class="sxs-lookup"><span data-stu-id="d6dae-154">hello previous code takes any user that happens tooauthenticate tooour server.</span></span> <span data-ttu-id="d6dae-155">To se označuje jako Automatická registrace.</span><span class="sxs-lookup"><span data-stu-id="d6dae-155">This is known as auto-registration.</span></span> <span data-ttu-id="d6dae-156">Doporučujeme vám, že nedošlo každý, kdo ověřovat tooa provozním serveru, aniž by bylo nejdříve je registrovat prostřednictvím procesu, který se rozhodnete.</span><span class="sxs-lookup"><span data-stu-id="d6dae-156">We    recommend that you don't let anyone authenticate tooa production server without first having them register via a process that you decide on.</span></span> <span data-ttu-id="d6dae-157">Je to obvykle hello vzor, který můžete vidět u uživatelských aplikací, které umožňují tooregister službou Facebook, ale poté vás požádají tooprovide Další informace.</span><span class="sxs-lookup"><span data-stu-id="d6dae-157">This is usually hello pattern you see in consumer apps, which allow you tooregister with Facebook but then ask you tooprovide additional information.</span></span> <span data-ttu-id="d6dae-158">Pokud to nebyly ukázkovou aplikaci, mohli bychom extrahovat hello uživatele e-mailovou adresu z hello tokenu objektu, který je vrácen a poté požádat uživatele toofill hello Další informace.</span><span class="sxs-lookup"><span data-stu-id="d6dae-158">If this weren't a sample application, we could have extracted hello user's email address from hello token object that is returned and then asked hello user toofill out additional information.</span></span> <span data-ttu-id="d6dae-159">Protože se jedná o testovací server, přidáme je toohello databáze v paměti.</span><span class="sxs-lookup"><span data-stu-id="d6dae-159">Because this is a test server, we add them toohello in-memory database.</span></span>


4. <span data-ttu-id="d6dae-160">V dalším kroku přidejme hello metody, které nám umožní tootrack hello přihlášeného uživatele jako vyžaduje Passport.</span><span class="sxs-lookup"><span data-stu-id="d6dae-160">Next, let's add hello methods that enable us tootrack hello signed-in users as required by Passport.</span></span> <span data-ttu-id="d6dae-161">Tyto metody zahrnují serializaci a deserializaci informací o uživateli hello.</span><span class="sxs-lookup"><span data-stu-id="d6dae-161">These methods include serializing and deserializing hello user's information.</span></span>

    ```JavaScript

            // Passport session setup. (Section 2)

            //   toosupport persistent sign-in sessions, Passport needs toobe able to
            //   serialize users into hello session and deserialize them out of hello session. Typically,
            //   this is done simply by storing hello user ID when serializing and finding
            //   hello user by ID when deserializing.
            passport.serializeUser(function(user, done) {
            done(null, user.email);
            });

            passport.deserializeUser(function(id, done) {
            findByEmail(id, function (err, user) {
                done(err, user);
            });
            });

            // array toohold signed-in users
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

5.  <span data-ttu-id="d6dae-162">V dalším kroku přidejme modulu Express hello kód tooload hello.</span><span class="sxs-lookup"><span data-stu-id="d6dae-162">Next, let's add hello code tooload hello Express engine.</span></span> <span data-ttu-id="d6dae-163">Tady používáme hello výchozí /views a poskytuje /routes vzor, který Express.</span><span class="sxs-lookup"><span data-stu-id="d6dae-163">Here we use hello default /views and /routes pattern that Express provides.</span></span>

    ```JavaScript

        // configure Express (section 2)

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

6. <span data-ttu-id="d6dae-164">Nakonec přidejme hello tras, že můžete předat hello skutečné žádostí o přihlášení toohello `passport-azure-ad` modul:</span><span class="sxs-lookup"><span data-stu-id="d6dae-164">Finally, let's add hello routes that hand off hello actual sign-in requests toohello `passport-azure-ad` engine:</span></span>


       ```JavaScript

        // Our Auth routes (section 3)

        // GET /auth/openid
        //   Use passport.authenticate() as route middleware tooauthenticate the
        //   request. hello first step in OpenID authentication involves redirecting
        //   hello user tootheir OpenID provider. After authenticating, hello OpenID
        //   provider redirects hello user back toothis application at
        //   /auth/openid/return.
        app.get('/auth/openid',
        passport.authenticate('azuread-openidconnect', { failureRedirect: '/login' }),
        function(req, res) {
            log.info('Authentication was called in hello Sample');
            res.redirect('/');
        });

            // GET /auth/openid/return
            //   Use passport.authenticate() as route middleware tooauthenticate the
            //   request. If authentication fails, hello user is redirected back toothe
            //   sign-in page. Otherwise, hello primary route function is called,
            //   which, in this example, redirects hello user toohello home page.
            app.get('/auth/openid/return',
              passport.authenticate('azuread-openidconnect', { failureRedirect: '/login' }),
              function(req, res) {
                log.info('We received a return from AzureAD.');
                res.redirect('/');
              });

            // POST /auth/openid/return
            //   Use passport.authenticate() as route middleware tooauthenticate the
            //   request. If authentication fails, hello user is redirected back toothe
            //   sign-in page. Otherwise, hello primary route function is called,
            //   which, in this example, redirects hello user toohello home page.
            app.post('/auth/openid/return',
              passport.authenticate('azuread-openidconnect', { failureRedirect: '/login' }),
              function(req, res) {
                log.info('We received a return from AzureAD.');
                res.redirect('/');
              });
       ```


## <a name="step-4-use-passport-tooissue-sign-in-and-sign-out-requests-tooazure-ad"></a><span data-ttu-id="d6dae-165">Krok 4: Použití Passport tooissue přihlášení a odhlášení požadavky tooAzure AD</span><span class="sxs-lookup"><span data-stu-id="d6dae-165">Step 4: Use Passport tooissue sign-in and sign-out requests tooAzure AD</span></span>
<span data-ttu-id="d6dae-166">Aplikace je nyní správně nakonfigurované toocommunicate s koncovým bodem hello pomocí ověřovacího protokolu OpenID Connect hello.</span><span class="sxs-lookup"><span data-stu-id="d6dae-166">Your app is now properly configured toocommunicate with hello endpoint by using hello OpenID Connect authentication protocol.</span></span>  <span data-ttu-id="d6dae-167">`passport-azure-ad`má postaráno všech podrobností o hello věnujte zpráv ověřování, ověřování tokenů z Azure AD a správa uživatelských relací.</span><span class="sxs-lookup"><span data-stu-id="d6dae-167">`passport-azure-ad` has taken care of all hello details of crafting authentication messages, validating tokens from Azure AD, and maintaining user sessions.</span></span> <span data-ttu-id="d6dae-168">Všechno, co zůstane je udělení uživatelům, odhlaste se a způsob toosign v a shromáždění Další informace o hello přihlášeného uživatele.</span><span class="sxs-lookup"><span data-stu-id="d6dae-168">All that remains is giving your users a way toosign in and sign out, and gathering additional information about hello signed-in users.</span></span>

1. <span data-ttu-id="d6dae-169">Nejprve přidejme hello výchozí, přihlášení, účet a odhlášení metody tooour `app.js` souboru:</span><span class="sxs-lookup"><span data-stu-id="d6dae-169">First, let's add hello default, sign-in, account, and sign-out methods tooour `app.js` file:</span></span>

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
            log.info('Login was called in hello Sample');
            res.redirect('/');
        });

        app.get('/logout', function(req, res){
          req.logout();
          res.redirect('/');
        });

    ```

2.  <span data-ttu-id="d6dae-170">Pojďme si podrobněji:</span><span class="sxs-lookup"><span data-stu-id="d6dae-170">Let's review these in detail:</span></span>

  * <span data-ttu-id="d6dae-171">Hello `/`trasa přesměruje toohello index.ejs zobrazení, předávání hello uživatele v požadavku hello (pokud existuje).</span><span class="sxs-lookup"><span data-stu-id="d6dae-171">hello `/`route redirects toohello index.ejs view, passing hello user in hello request (if it exists).</span></span>
  * <span data-ttu-id="d6dae-172">Hello `/account` směrovat nejprve *zajistí jsme se ověří* (jsme implementovat, v následující ukázka hello), a potom předává hello uživatele v požadavku hello tak, aby se nám můžete získat další informace o uživateli hello.</span><span class="sxs-lookup"><span data-stu-id="d6dae-172">hello `/account` route first *ensures we are authenticated* (we implement that in hello following example), and then passes hello user in hello request so that we can get additional information about hello user.</span></span>
  * <span data-ttu-id="d6dae-173">Hello `/login` trasy volá naše azuread openidconnect authenticator z `passport-azuread`.</span><span class="sxs-lookup"><span data-stu-id="d6dae-173">hello `/login` route calls our azuread-openidconnect authenticator from `passport-azuread`.</span></span> <span data-ttu-id="d6dae-174">Pokud není úspěšné, přesměruje hello back příliš nebo přihlášení uživatele.</span><span class="sxs-lookup"><span data-stu-id="d6dae-174">If that doesn't succeed, it redirects hello user back too/login.</span></span>
  * <span data-ttu-id="d6dae-175">Hello `/logout` trasy jednoduše volá hello logout.ejs (a trasy), které vymaže soubory cookie a pak vrátí hello back tooindex.ejs uživatele.</span><span class="sxs-lookup"><span data-stu-id="d6dae-175">hello `/logout` route simply calls hello logout.ejs (and route), which clears cookies and then returns hello user back tooindex.ejs.</span></span>

3. <span data-ttu-id="d6dae-176">Pro poslední část hello `app.js`, přidejme hello **EnsureAuthenticated** metoda, která se používá v `/account`, jako je uvedené výše.</span><span class="sxs-lookup"><span data-stu-id="d6dae-176">For hello last part of `app.js`, let's add hello **EnsureAuthenticated** method that is used in `/account`, as shown earlier.</span></span>

    ```JavaScript

        // Simple route middleware tooensure user is authenticated. (section 4)

        //   Use this route middleware on any resource that needs toobe protected. If
        //   hello request is authenticated (typically via a persistent sign-in session),
        //   hello request proceeds. Otherwise, hello user is redirected toothe
        //   sign-in page.
        function ensureAuthenticated(req, res, next) {
          if (req.isAuthenticated()) { return next(); }
          res.redirect('/login')
        }
    ```

4. <span data-ttu-id="d6dae-177">Nakonec vytvořte samotný server hello v `app.js`:</span><span class="sxs-lookup"><span data-stu-id="d6dae-177">Finally, let's create hello server itself in `app.js`:</span></span>

```JavaScript

        app.listen(3000);

```


## <a name="step-5-toodisplay-our-user-in-hello-website-create-hello-views-and-routes-in-express"></a><span data-ttu-id="d6dae-178">Krok 5: toodisplay naše uživatele hello webu, vytvoření hello zobrazení a trasy v Express</span><span class="sxs-lookup"><span data-stu-id="d6dae-178">Step 5: toodisplay our user in hello website, create hello views and routes in Express</span></span>
<span data-ttu-id="d6dae-179">Nyní `app.js` dokončení.</span><span class="sxs-lookup"><span data-stu-id="d6dae-179">Now `app.js` is complete.</span></span> <span data-ttu-id="d6dae-180">Můžeme jednoduše potřebovat tooadd hello trasy a zobrazení tyto informace zobrazit hello jsme získat toohello uživatele, stejně jako zpracovává hello `/logout` a `/login` tras, které jsme vytvořili.</span><span class="sxs-lookup"><span data-stu-id="d6dae-180">We simply need tooadd hello routes and views that show hello information we get toohello user, as well as handle hello `/logout` and `/login` routes that we  created.</span></span>

1. <span data-ttu-id="d6dae-181">Vytvoření hello `/routes/index.js` trasy pod kořenovým adresářem hello.</span><span class="sxs-lookup"><span data-stu-id="d6dae-181">Create hello `/routes/index.js` route under hello root directory.</span></span>

    ```JavaScript
                /*
                 * GET home page.
                 */

                exports.index = function(req, res){
                  res.render('index', { title: 'Express' });
                };
    ```

2. <span data-ttu-id="d6dae-182">Vytvoření hello `/routes/user.js` trasy pod kořenovým adresářem hello.</span><span class="sxs-lookup"><span data-stu-id="d6dae-182">Create hello `/routes/user.js` route under hello root directory.</span></span>

                ```JavaScript
                /*
                 * GET users listing.
                 */

                exports.list = function(req, res){
                  res.send("respond with a resource");
                };
                ```

 <span data-ttu-id="d6dae-183">Tyto předají hello požadavek tooour zobrazení, včetně uživatelských hello, pokud je k dispozici.</span><span class="sxs-lookup"><span data-stu-id="d6dae-183">These pass along hello request tooour views, including hello user if present.</span></span>

3. <span data-ttu-id="d6dae-184">Vytvoření hello `/views/index.ejs` zobrazení pod kořenovým adresářem hello.</span><span class="sxs-lookup"><span data-stu-id="d6dae-184">Create hello `/views/index.ejs` view under hello root directory.</span></span> <span data-ttu-id="d6dae-185">Toto je jednoduchá stránka, která volá metody, naše přihlášení a odhlášení a umožňuje nám informace o účtu toograb.</span><span class="sxs-lookup"><span data-stu-id="d6dae-185">This is a simple page that calls our login and logout methods and enables us toograb account information.</span></span> <span data-ttu-id="d6dae-186">Všimněte si, že můžeme použít hello podmíněného `if (!user)` jako uživatel hello předávány prostřednictvím v žádosti o hello je důkaz máme přihlášeného uživatele.</span><span class="sxs-lookup"><span data-stu-id="d6dae-186">Notice that we can use hello conditional `if (!user)` as hello user being passed through in hello request is evidence we have a signed-in user.</span></span>

    ```JavaScript
    <% if (!user) { %>
        <h2>Welcome! Please log in.</h2>
        <a href="/login">Log In</a>
    <% } else { %>
        <h2>Hello, <%= user.displayName %>.</h2>
        <a href="/account">Account Info</a></br>
        <a href="/logout">Log Out</a>
    <% } %>
    ```

4. <span data-ttu-id="d6dae-187">Vytvoření hello `/views/account.ejs` zobrazení pod kořenovým adresářem hello, takže jsme můžete zobrazit další informace, `passport-azuread` pozastavil v požadavku uživatele hello.</span><span class="sxs-lookup"><span data-stu-id="d6dae-187">Create hello `/views/account.ejs` view under hello root directory so that we can view additional information that `passport-azuread` has put in hello user request.</span></span>

    ```Javascript
    <% if (!user) { %>
        <h2>Welcome! Please log in.</h2>
        <a href="/login">Log In</a>
    <% } else { %>
    <p>displayName: <%= user.displayName %></p>
    <p>givenName: <%= user.name.givenName %></p>
    <p>familyName: <%= user.name.familyName %></p>
    <p>UPN: <%= user._json.upn %></p>
    <p>Profile ID: <%= user.id %></p>
  ##Next steps  <p>Full Claimes</p>
    <%- JSON.stringify(user) %>
    <p></p>
    <a href="/logout">Log Out</a>
    <% } %>
    ```

5. <span data-ttu-id="d6dae-188">Umožňuje, aby tento vzhled dobrý přidání rozložení.</span><span class="sxs-lookup"><span data-stu-id="d6dae-188">Let's make this look good by adding a layout.</span></span> <span data-ttu-id="d6dae-189">Vytvoření hello "/ zobrazení se views/layout.ejs pod hello kořenový adresář.</span><span class="sxs-lookup"><span data-stu-id="d6dae-189">Create hello '/views/layout.ejs' view under hello root directory.</span></span>

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
                <a href="/login">Log In</a>
                </p>
            <% } else { %>
                <p>
                <a href="/">Home</a> |
                <a href="/account">Account</a> |
                <a href="/logout">Log Out</a>
                </p>
            <% } %>
            <%- body %>
        </body>
    </html>
    ```

##<a name="next-steps"></a><span data-ttu-id="d6dae-190">Další kroky</span><span class="sxs-lookup"><span data-stu-id="d6dae-190">Next steps</span></span>
<span data-ttu-id="d6dae-191">Nakonec sestavte a spusťte aplikaci.</span><span class="sxs-lookup"><span data-stu-id="d6dae-191">Finally, build and run your app.</span></span> <span data-ttu-id="d6dae-192">Spustit `node app.js`a potom přejděte příliš`http://localhost:3000`.</span><span class="sxs-lookup"><span data-stu-id="d6dae-192">Run `node app.js`, and then go too`http://localhost:3000`.</span></span>

<span data-ttu-id="d6dae-193">Přihlaste se pomocí osobního účtu Microsoft nebo pracovní nebo školní účet a Všimněte si, jak je identita uživatele hello projeví v seznamu /account hello.</span><span class="sxs-lookup"><span data-stu-id="d6dae-193">Sign in with either a personal Microsoft account or a work or school account, and notice how hello user's identity is reflected in hello /account list.</span></span> <span data-ttu-id="d6dae-194">Nyní máte webovou aplikaci, která je zabezpečen pomocí standardních oborových protokolech, které může ověřit uživatele s svoje osobní, tak i pracovní nebo školní účty.</span><span class="sxs-lookup"><span data-stu-id="d6dae-194">You now have a web app that's secured with industry standard protocols that can authenticate users with both their personal and work/school accounts.</span></span>

<span data-ttu-id="d6dae-195">Pro referenci hello dokončit ukázka (bez vašich hodnot nastavení) [je k dispozici jako soubor ZIP](https://github.com/AzureADQuickStarts/WebApp-OpenIDConnect-NodeJS/archive/complete.zip).</span><span class="sxs-lookup"><span data-stu-id="d6dae-195">For reference, hello completed sample (without your configuration values) [is provided as a .zip file](https://github.com/AzureADQuickStarts/WebApp-OpenIDConnect-NodeJS/archive/complete.zip).</span></span> <span data-ttu-id="d6dae-196">Alternativně můžete klonovat z Githubu:</span><span class="sxs-lookup"><span data-stu-id="d6dae-196">Alternatively, you can clone it from GitHub:</span></span>

```git clone --branch complete https://github.com/AzureADQuickStarts/WebApp-OpenIDConnect-NodeJS.git```

<span data-ttu-id="d6dae-197">Nyní se můžete přesunout na pokročilejší témata.</span><span class="sxs-lookup"><span data-stu-id="d6dae-197">You can now move onto more advanced topics.</span></span> <span data-ttu-id="d6dae-198">Můžete chtít tootry:</span><span class="sxs-lookup"><span data-stu-id="d6dae-198">You might want tootry:</span></span>

[<span data-ttu-id="d6dae-199">Zabezpečení webového rozhraní API pomocí Azure AD</span><span class="sxs-lookup"><span data-stu-id="d6dae-199">Secure a Web API with Azure AD</span></span>](active-directory-devquickstarts-webapi-nodejs.md)

[!INCLUDE [active-directory-devquickstarts-additional-resources](../../../includes/active-directory-devquickstarts-additional-resources.md)]
