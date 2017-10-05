---
title: "Začínáme s Azure AD, přihlašování a odhlašování pomocí Node.js | Microsoft Docs"
description: "Naučte se vytvářet webové aplikace Node.js Express MVC, která se integruje se službou Azure AD pro přihlášení."
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
ms.openlocfilehash: 13317b016f9ff3955f376b858645c42668b0de42
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="nodejs-web-app-sign-in-and-sign-out-with-azure-ad"></a><span data-ttu-id="afc87-103">Webové aplikace Node.js přihlášení a odhlášení s Azure AD</span><span class="sxs-lookup"><span data-stu-id="afc87-103">Node.js web app sign-in and sign-out with Azure AD</span></span>
<span data-ttu-id="afc87-104">Tady používáme Passport:</span><span class="sxs-lookup"><span data-stu-id="afc87-104">Here we use Passport to:</span></span>

* <span data-ttu-id="afc87-105">Uživatele přihlaste k aplikaci pomocí Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="afc87-105">Sign the user in to the app with Azure Active Directory (Azure AD).</span></span>
* <span data-ttu-id="afc87-106">Zobrazí informace o uživateli.</span><span class="sxs-lookup"><span data-stu-id="afc87-106">Display information about the user.</span></span>
* <span data-ttu-id="afc87-107">Přihlášení uživatele mimo aplikaci.</span><span class="sxs-lookup"><span data-stu-id="afc87-107">Sign the user out of the app.</span></span>

<span data-ttu-id="afc87-108">Passport je ověřovací middleware pro Node.js.</span><span class="sxs-lookup"><span data-stu-id="afc87-108">Passport is authentication middleware for Node.js.</span></span> <span data-ttu-id="afc87-109">Flexibilní a modulární, Passport lze snadno vyřadit k žádnému využívající Express nebo restify webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="afc87-109">Flexible and modular, Passport can be unobtrusively dropped in to any Express-based or restify web application.</span></span> <span data-ttu-id="afc87-110">Komplexní sada strategií podporují ověřování pomocí uživatelského jména a hesla, Facebook, Twitter a další.</span><span class="sxs-lookup"><span data-stu-id="afc87-110">A comprehensive set of strategies support authentication that uses a username and password, Facebook, Twitter, and more.</span></span> <span data-ttu-id="afc87-111">Vyvinuli jsme strategii pro Microsoft Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="afc87-111">We have developed a strategy for Microsoft Azure Active Directory.</span></span> <span data-ttu-id="afc87-112">Jsme nainstalujete tento modul a poté přidejte Microsoft Azure Active Directory `passport-azure-ad` modulu plug-in.</span><span class="sxs-lookup"><span data-stu-id="afc87-112">We install this module and then add the Microsoft Azure Active Directory `passport-azure-ad` plug-in.</span></span>

<span data-ttu-id="afc87-113">Chcete-li to provést, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="afc87-113">To do this, take the following steps:</span></span>

1. <span data-ttu-id="afc87-114">Zaregistrujte aplikaci.</span><span class="sxs-lookup"><span data-stu-id="afc87-114">Register an app.</span></span>
2. <span data-ttu-id="afc87-115">Nastavení aplikace pro použití `passport-azure-ad` strategie.</span><span class="sxs-lookup"><span data-stu-id="afc87-115">Set up your app to use the `passport-azure-ad` strategy.</span></span>
3. <span data-ttu-id="afc87-116">Použít Passport pro zasílání požadavků na přihlášení a odhlášení do Azure AD.</span><span class="sxs-lookup"><span data-stu-id="afc87-116">Use Passport to issue sign-in and sign-out requests to Azure AD.</span></span>
4. <span data-ttu-id="afc87-117">Tisknout data o uživateli.</span><span class="sxs-lookup"><span data-stu-id="afc87-117">Print data about the user.</span></span>

<span data-ttu-id="afc87-118">Kód k tomuto kurzu je udržovaný [na GitHubu](https://github.com/AzureADQuickStarts/WebApp-OpenIDConnect-NodeJS).</span><span class="sxs-lookup"><span data-stu-id="afc87-118">The code for this tutorial is maintained [on GitHub](https://github.com/AzureADQuickStarts/WebApp-OpenIDConnect-NodeJS).</span></span>  <span data-ttu-id="afc87-119">Chcete-li sledovat, můžete [stáhnout kostru aplikace jako soubor ZIP](https://github.com/AzureADQuickStarts/WebApp-OpenIDConnect-NodeJS/archive/skeleton.zip) nebo tuto kostru klonovat:</span><span class="sxs-lookup"><span data-stu-id="afc87-119">To follow along, you can [download the app's skeleton as a .zip file](https://github.com/AzureADQuickStarts/WebApp-OpenIDConnect-NodeJS/archive/skeleton.zip) or clone the skeleton:</span></span>

```git clone --branch skeleton https://github.com/AzureADQuickStarts/WebApp-OpenIDConnect-NodeJS.git```

<span data-ttu-id="afc87-120">Dokončená aplikace je k dispozici na konci tohoto kurzu také.</span><span class="sxs-lookup"><span data-stu-id="afc87-120">The completed application is provided at the end of this tutorial as well.</span></span>

## <a name="step-1-register-an-app"></a><span data-ttu-id="afc87-121">Krok 1: Registrace aplikace</span><span class="sxs-lookup"><span data-stu-id="afc87-121">Step 1: Register an app</span></span>
1. <span data-ttu-id="afc87-122">Přihlaste se k webu [Azure Portal](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="afc87-122">Sign in to the [Azure portal](https://portal.azure.com).</span></span>

2. <span data-ttu-id="afc87-123">V nabídce v horní části stránky vyberte svůj účet.</span><span class="sxs-lookup"><span data-stu-id="afc87-123">In the menu at the top of the page, select your account.</span></span> <span data-ttu-id="afc87-124">V části **Directory** vyberte klienta služby Active Directory, kde chcete registrace vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="afc87-124">Under the **Directory** list, choose the Active Directory tenant where you want to register your application.</span></span>

3. <span data-ttu-id="afc87-125">Vyberte **více služeb** v nabídce na levé straně obrazovky a pak vyberte **Azure Active Directory**.</span><span class="sxs-lookup"><span data-stu-id="afc87-125">Select **More Services** in the menu on the left side of the screen, and then select **Azure Active Directory**.</span></span>

4. <span data-ttu-id="afc87-126">Vyberte **registrace aplikace**a potom vyberte **přidat**.</span><span class="sxs-lookup"><span data-stu-id="afc87-126">Select **App registrations**, and then select **Add**.</span></span>

5. <span data-ttu-id="afc87-127">Postupujte podle výzev a vytvořte **webové aplikace** nebo **WebAPI**.</span><span class="sxs-lookup"><span data-stu-id="afc87-127">Follow the prompts to create a **Web Application** and/or **WebAPI**.</span></span>
  * <span data-ttu-id="afc87-128">**Název** aplikace popisuje vaší aplikace pro uživatele.</span><span class="sxs-lookup"><span data-stu-id="afc87-128">The **name** of the application describes your application to users.</span></span>

  * <span data-ttu-id="afc87-129">**Přihlašovací adresa URL** je základní adresu URL aplikace.</span><span class="sxs-lookup"><span data-stu-id="afc87-129">The **Sign-On URL** is the base URL of your app.</span></span>  <span data-ttu-id="afc87-130">Kostru je výchozí hodnota je ' http://localhost: 3000/auth/openid nebo vrátit hodnotu.</span><span class="sxs-lookup"><span data-stu-id="afc87-130">The skeleton's default is `http://localhost:3000/auth/openid/return`\`.</span></span>

6. <span data-ttu-id="afc87-131">Po registraci, Azure AD přiřadí vaší aplikace ID jedinečný aplikace.</span><span class="sxs-lookup"><span data-stu-id="afc87-131">After you register, Azure AD assigns your app a unique application ID.</span></span> <span data-ttu-id="afc87-132">Je třeba tuto hodnotu v následujících částech, zkopírujte jej ze stránky aplikace.</span><span class="sxs-lookup"><span data-stu-id="afc87-132">You need this value in the following sections, so copy it from the application page.</span></span>
7. <span data-ttu-id="afc87-133">Z **nastavení** -> **vlastnosti** stránky pro aplikace, aktualizujte identifikátor ID URI aplikace.</span><span class="sxs-lookup"><span data-stu-id="afc87-133">From the **Settings** -> **Properties** page for your application, update the App ID URI.</span></span> <span data-ttu-id="afc87-134">**Identifikátor ID URI aplikace** je jedinečný identifikátor pro vaši aplikaci.</span><span class="sxs-lookup"><span data-stu-id="afc87-134">The **App ID URI** is a unique identifier for your application.</span></span> <span data-ttu-id="afc87-135">Konvence, je použít formát `https://<tenant-domain>/<app-name>`, například: `https://contoso.onmicrosoft.com/my-first-aad-app`.</span><span class="sxs-lookup"><span data-stu-id="afc87-135">The convention is to use the format `https://<tenant-domain>/<app-name>`, for example: `https://contoso.onmicrosoft.com/my-first-aad-app`.</span></span>

## <a name="step-2-add-prerequisites-to-your-directory"></a><span data-ttu-id="afc87-136">Krok 2: Přidání požadovaných součástí do adresáře</span><span class="sxs-lookup"><span data-stu-id="afc87-136">Step 2: Add prerequisites to your directory</span></span>
1. <span data-ttu-id="afc87-137">Z příkazového řádku, změňte adresáře na kořenové složky a pokud si nejste již existuje, a poté spusťte následující příkazy:</span><span class="sxs-lookup"><span data-stu-id="afc87-137">From the command line, change directories to your root folder if you're not already there, and then run the following commands:</span></span>

    * `npm install express`
    * `npm install ejs`
    * `npm install ejs-locals`
    * `npm install restify`
    * `npm install mongoose`
    * `npm install bunyan`
    * `npm install assert-plus`
    * `npm install passport`

2. <span data-ttu-id="afc87-138">Kromě toho musíte `passport-azure-ad`:</span><span class="sxs-lookup"><span data-stu-id="afc87-138">In addition, you need `passport-azure-ad`:</span></span>
    * `npm install passport-azure-ad`

<span data-ttu-id="afc87-139">Tím se nainstaluje do knihoven, `passport-azure-ad` závisí na.</span><span class="sxs-lookup"><span data-stu-id="afc87-139">This installs the libraries that `passport-azure-ad` depends on.</span></span>

## <a name="step-3-set-up-your-app-to-use-the-passport-node-js-strategy"></a><span data-ttu-id="afc87-140">Krok 3: Nastavení aplikace k použití strategie passport uzlu js</span><span class="sxs-lookup"><span data-stu-id="afc87-140">Step 3: Set up your app to use the passport-node-js strategy</span></span>
<span data-ttu-id="afc87-141">Zde jsme nakonfigurovat Express pro použití ověřovacího protokolu OpenID Connect.</span><span class="sxs-lookup"><span data-stu-id="afc87-141">Here, we configure Express to use the OpenID Connect authentication protocol.</span></span>  <span data-ttu-id="afc87-142">Passport umožňuje provádět různé akce, včetně požadavků na přihlášení a odhlášení problém, spravovat relace uživatele a získat informace o uživateli.</span><span class="sxs-lookup"><span data-stu-id="afc87-142">Passport is used to do various things, including issue sign-in and sign-out requests, manage the user's session, and get information about the user.</span></span>

1. <span data-ttu-id="afc87-143">Chcete-li začít, otevřete `config.js` souboru v kořenovém adresáři projektu a potom zadejte hodnoty konfigurace vaší aplikace v `exports.creds` oddílu.</span><span class="sxs-lookup"><span data-stu-id="afc87-143">To begin, open the `config.js` file at the root of the project, and then enter your app's configuration values in the `exports.creds` section.</span></span>

  * <span data-ttu-id="afc87-144">`clientID` Je **Id aplikace** přiřazené vaší aplikaci v portálu pro registraci.</span><span class="sxs-lookup"><span data-stu-id="afc87-144">The `clientID` is the **Application Id** that's assigned to your app in the registration portal.</span></span>

  * <span data-ttu-id="afc87-145">`returnURL` Je **identifikátor Uri pro přesměrování** kterou jste zadali v portálu.</span><span class="sxs-lookup"><span data-stu-id="afc87-145">The `returnURL` is the **Redirect Uri** that you entered in the portal.</span></span>

  * <span data-ttu-id="afc87-146">`clientSecret` Je tajný klíč, který jste vygenerovali na portálu.</span><span class="sxs-lookup"><span data-stu-id="afc87-146">The `clientSecret` is the secret that you generated in the portal.</span></span>

2. <span data-ttu-id="afc87-147">Dále otevřete `app.js` soubor v kořenovém adresáři projektu.</span><span class="sxs-lookup"><span data-stu-id="afc87-147">Next, open the `app.js` file in the root of the project.</span></span> <span data-ttu-id="afc87-148">Pak přidejte následující volání pro vyvolání `OIDCStrategy` strategie, která se dodává s `passport-azure-ad`.</span><span class="sxs-lookup"><span data-stu-id="afc87-148">Then add the following call to invoke the `OIDCStrategy` strategy that comes with `passport-azure-ad`.</span></span>

    ```JavaScript
    var OIDCStrategy = require('passport-azure-ad').OIDCStrategy;

    // add a logger

    var log = bunyan.createLogger({
    name: 'Microsoft OIDC Example Web Application'
    });
    ```

3. <span data-ttu-id="afc87-149">Potom použijte strategii jsme právě přidanou pro zpracování naše žádostí o přihlášení.</span><span class="sxs-lookup"><span data-stu-id="afc87-149">After that, use the strategy we just referenced to handle our sign-in requests.</span></span>

    ```JavaScript
    // Use the OIDCStrategy within Passport. (Section 2)
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
<span data-ttu-id="afc87-150">Passport používá podobný Princip pro všechny svoje strategie (Twitteru, Facebooku a tak dále), které řídí všichni autoři strategií k.</span><span class="sxs-lookup"><span data-stu-id="afc87-150">Passport uses a similar pattern for all its strategies (Twitter, Facebook, and so on) that all strategy writers adhere to.</span></span> <span data-ttu-id="afc87-151">Prohlížení strategie, uvidíte, že jsme předáváme funkci, která má token a done jako parametry.</span><span class="sxs-lookup"><span data-stu-id="afc87-151">Looking at the strategy, you see that we pass it a function that has a token and a done as the parameters.</span></span> <span data-ttu-id="afc87-152">Strategie vrátí do us po jeho činnosti provede.</span><span class="sxs-lookup"><span data-stu-id="afc87-152">The strategy comes back to us after it does its work.</span></span> <span data-ttu-id="afc87-153">Potom chceme uložení uživatele a skrytí tokenu, takže jsme nemusíte požadovat znovu.</span><span class="sxs-lookup"><span data-stu-id="afc87-153">Then we want to store the user and stash the token so we don't need to ask for it again.</span></span>

> [!IMPORTANT]
<span data-ttu-id="afc87-154">Předchozí kód přijímá jakéhokoli uživatele, které dochází k ověřování na našem server.</span><span class="sxs-lookup"><span data-stu-id="afc87-154">The previous code takes any user that happens to authenticate to our server.</span></span> <span data-ttu-id="afc87-155">To se označuje jako Automatická registrace.</span><span class="sxs-lookup"><span data-stu-id="afc87-155">This is known as auto-registration.</span></span> <span data-ttu-id="afc87-156">Doporučujeme vám, že nedošlo každý, kdo ověření na provozním serveru, aniž by bylo nejdříve je registrovat prostřednictvím procesu, který se rozhodnete.</span><span class="sxs-lookup"><span data-stu-id="afc87-156">We    recommend that you don't let anyone authenticate to a production server without first having them register via a process that you decide on.</span></span> <span data-ttu-id="afc87-157">To je obvykle vzor, který můžete vidět u uživatelských aplikací, které umožňují registraci pomocí Facebooku, ale poté vás požádají o poskytují další informace.</span><span class="sxs-lookup"><span data-stu-id="afc87-157">This is usually the pattern you see in consumer apps, which allow you to register with Facebook but then ask you to provide additional information.</span></span> <span data-ttu-id="afc87-158">Pokud to nebyly ukázkovou aplikaci, mohli bychom extrahovat e-mailovou adresu uživatele z tokenu objektu, který se vrátí a poté požádat uživatele k vyplnění dodatečných informací.</span><span class="sxs-lookup"><span data-stu-id="afc87-158">If this weren't a sample application, we could have extracted the user's email address from the token object that is returned and then asked the user to fill out additional information.</span></span> <span data-ttu-id="afc87-159">Protože se jedná o testovací server, jsme je přidat k databázi v paměti.</span><span class="sxs-lookup"><span data-stu-id="afc87-159">Because this is a test server, we add them to the in-memory database.</span></span>


4. <span data-ttu-id="afc87-160">V dalším kroku přidejme metody, které umožní, abychom mohli sledovat přihlášených uživatelů podle požadavků Passport.</span><span class="sxs-lookup"><span data-stu-id="afc87-160">Next, let's add the methods that enable us to track the signed-in users as required by Passport.</span></span> <span data-ttu-id="afc87-161">Tyto metody zahrnují serializaci a deserializaci informací o uživateli.</span><span class="sxs-lookup"><span data-stu-id="afc87-161">These methods include serializing and deserializing the user's information.</span></span>

    ```JavaScript

            // Passport session setup. (Section 2)

            //   To support persistent sign-in sessions, Passport needs to be able to
            //   serialize users into the session and deserialize them out of the session. Typically,
            //   this is done simply by storing the user ID when serializing and finding
            //   the user by ID when deserializing.
            passport.serializeUser(function(user, done) {
            done(null, user.email);
            });

            passport.deserializeUser(function(id, done) {
            findByEmail(id, function (err, user) {
                done(err, user);
            });
            });

            // array to hold signed-in users
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

5.  <span data-ttu-id="afc87-162">Dále umožňuje přidat kód pro načtení modulu Express.</span><span class="sxs-lookup"><span data-stu-id="afc87-162">Next, let's add the code to load the Express engine.</span></span> <span data-ttu-id="afc87-163">Tady používáme výchozí /views a poskytuje /routes vzor, který Express.</span><span class="sxs-lookup"><span data-stu-id="afc87-163">Here we use the default /views and /routes pattern that Express provides.</span></span>

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
          // Initialize Passport!  Also use passport.session() middleware, to support
          // persistent login sessions (recommended).
          app.use(passport.initialize());
          app.use(passport.session());
          app.use(app.router);
          app.use(express.static(__dirname + '/../../public'));
        });

    ```

6. <span data-ttu-id="afc87-164">Nakonec přidejme tras, které přebírají skutečné přihlášení žádosti, které chcete `passport-azure-ad` modul:</span><span class="sxs-lookup"><span data-stu-id="afc87-164">Finally, let's add the routes that hand off the actual sign-in requests to the `passport-azure-ad` engine:</span></span>


       ```JavaScript

        // Our Auth routes (section 3)

        // GET /auth/openid
        //   Use passport.authenticate() as route middleware to authenticate the
        //   request. The first step in OpenID authentication involves redirecting
        //   the user to their OpenID provider. After authenticating, the OpenID
        //   provider redirects the user back to this application at
        //   /auth/openid/return.
        app.get('/auth/openid',
        passport.authenticate('azuread-openidconnect', { failureRedirect: '/login' }),
        function(req, res) {
            log.info('Authentication was called in the Sample');
            res.redirect('/');
        });

            // GET /auth/openid/return
            //   Use passport.authenticate() as route middleware to authenticate the
            //   request. If authentication fails, the user is redirected back to the
            //   sign-in page. Otherwise, the primary route function is called,
            //   which, in this example, redirects the user to the home page.
            app.get('/auth/openid/return',
              passport.authenticate('azuread-openidconnect', { failureRedirect: '/login' }),
              function(req, res) {
                log.info('We received a return from AzureAD.');
                res.redirect('/');
              });

            // POST /auth/openid/return
            //   Use passport.authenticate() as route middleware to authenticate the
            //   request. If authentication fails, the user is redirected back to the
            //   sign-in page. Otherwise, the primary route function is called,
            //   which, in this example, redirects the user to the home page.
            app.post('/auth/openid/return',
              passport.authenticate('azuread-openidconnect', { failureRedirect: '/login' }),
              function(req, res) {
                log.info('We received a return from AzureAD.');
                res.redirect('/');
              });
       ```


## <a name="step-4-use-passport-to-issue-sign-in-and-sign-out-requests-to-azure-ad"></a><span data-ttu-id="afc87-165">Krok 4: Použití služby Passport pro zasílání požadavků na přihlášení a odhlášení do Azure AD</span><span class="sxs-lookup"><span data-stu-id="afc87-165">Step 4: Use Passport to issue sign-in and sign-out requests to Azure AD</span></span>
<span data-ttu-id="afc87-166">Aplikace je nyní správně nakonfigurován pro komunikaci s koncovým bodem pomocí ověřovacího protokolu OpenID Connect.</span><span class="sxs-lookup"><span data-stu-id="afc87-166">Your app is now properly configured to communicate with the endpoint by using the OpenID Connect authentication protocol.</span></span>  <span data-ttu-id="afc87-167">`passport-azure-ad`má postaráno všechny podrobnosti o věnujte zpráv ověřování, ověřování tokenů z Azure AD a údržbě uživatelských relací.</span><span class="sxs-lookup"><span data-stu-id="afc87-167">`passport-azure-ad` has taken care of all the details of crafting authentication messages, validating tokens from Azure AD, and maintaining user sessions.</span></span> <span data-ttu-id="afc87-168">Všechny, které zůstává je udělení uživatelům způsob, jak přihlášení a odhlášení a shromažďování Další informace o přihlášených uživatelů.</span><span class="sxs-lookup"><span data-stu-id="afc87-168">All that remains is giving your users a way to sign in and sign out, and gathering additional information about the signed-in users.</span></span>

1. <span data-ttu-id="afc87-169">Nejprve přidejme výchozí, přihlášení, účet a odhlášení metody pro naše `app.js` souboru:</span><span class="sxs-lookup"><span data-stu-id="afc87-169">First, let's add the default, sign-in, account, and sign-out methods to our `app.js` file:</span></span>

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
            log.info('Login was called in the Sample');
            res.redirect('/');
        });

        app.get('/logout', function(req, res){
          req.logout();
          res.redirect('/');
        });

    ```

2.  <span data-ttu-id="afc87-170">Pojďme si podrobněji:</span><span class="sxs-lookup"><span data-stu-id="afc87-170">Let's review these in detail:</span></span>

  * <span data-ttu-id="afc87-171">`/`Trasa přesměruje na zobrazení index.ejs předávání uživatele v požadavku (pokud existuje).</span><span class="sxs-lookup"><span data-stu-id="afc87-171">The `/`route redirects to the index.ejs view, passing the user in the request (if it exists).</span></span>
  * <span data-ttu-id="afc87-172">`/account` Směrovat nejprve *zajistí jsme se ověří* (jsme implementovat, v následujícím příkladu) a poté předá uživatele v požadavku tak, aby se nám můžete získat další informace o uživateli.</span><span class="sxs-lookup"><span data-stu-id="afc87-172">The `/account` route first *ensures we are authenticated* (we implement that in the following example), and then passes the user in the request so that we can get additional information about the user.</span></span>
  * <span data-ttu-id="afc87-173">`/login` Trasy volá naše azuread openidconnect authenticator z `passport-azuread`.</span><span class="sxs-lookup"><span data-stu-id="afc87-173">The `/login` route calls our azuread-openidconnect authenticator from `passport-azuread`.</span></span> <span data-ttu-id="afc87-174">Pokud není úspěšné, přesměruje uživatele zpět na Publikace1.</span><span class="sxs-lookup"><span data-stu-id="afc87-174">If that doesn't succeed, it redirects the user back to /login.</span></span>
  * <span data-ttu-id="afc87-175">`/logout` Směrování jednoduše volání logout.ejs (a trasy), které vymaže soubory cookie a poté vrátí uživatele zpět na index.ejs.</span><span class="sxs-lookup"><span data-stu-id="afc87-175">The `/logout` route simply calls the logout.ejs (and route), which clears cookies and then returns the user back to index.ejs.</span></span>

3. <span data-ttu-id="afc87-176">Pro poslední část `app.js`, přidejme **EnsureAuthenticated** metoda, která se používá v `/account`, jako je uvedené výše.</span><span class="sxs-lookup"><span data-stu-id="afc87-176">For the last part of `app.js`, let's add the **EnsureAuthenticated** method that is used in `/account`, as shown earlier.</span></span>

    ```JavaScript

        // Simple route middleware to ensure user is authenticated. (section 4)

        //   Use this route middleware on any resource that needs to be protected. If
        //   the request is authenticated (typically via a persistent sign-in session),
        //   the request proceeds. Otherwise, the user is redirected to the
        //   sign-in page.
        function ensureAuthenticated(req, res, next) {
          if (req.isAuthenticated()) { return next(); }
          res.redirect('/login')
        }
    ```

4. <span data-ttu-id="afc87-177">Nakonec vytvořte samotný server v `app.js`:</span><span class="sxs-lookup"><span data-stu-id="afc87-177">Finally, let's create the server itself in `app.js`:</span></span>

```JavaScript

        app.listen(3000);

```


## <a name="step-5-to-display-our-user-in-the-website-create-the-views-and-routes-in-express"></a><span data-ttu-id="afc87-178">Krok 5: Pokud chcete zobrazit naše uživatele na webu, zobrazení a vytvořte tras ve Express</span><span class="sxs-lookup"><span data-stu-id="afc87-178">Step 5: To display our user in the website, create the views and routes in Express</span></span>
<span data-ttu-id="afc87-179">Nyní `app.js` dokončení.</span><span class="sxs-lookup"><span data-stu-id="afc87-179">Now `app.js` is complete.</span></span> <span data-ttu-id="afc87-180">Musíme jednoduše přidat trasy a zobrazení, které zobrazují informace jsme získat uživateli, stejně jako zpracování `/logout` a `/login` tras, které jsme vytvořili.</span><span class="sxs-lookup"><span data-stu-id="afc87-180">We simply need to add the routes and views that show the information we get to the user, as well as handle the `/logout` and `/login` routes that we  created.</span></span>

1. <span data-ttu-id="afc87-181">V kořenovém adresáři vytvořte trasu `/routes/index.js`.</span><span class="sxs-lookup"><span data-stu-id="afc87-181">Create the `/routes/index.js` route under the root directory.</span></span>

    ```JavaScript
                /*
                 * GET home page.
                 */

                exports.index = function(req, res){
                  res.render('index', { title: 'Express' });
                };
    ```

2. <span data-ttu-id="afc87-182">V kořenovém adresáři vytvořte trasu `/routes/user.js`.</span><span class="sxs-lookup"><span data-stu-id="afc87-182">Create the `/routes/user.js` route under the root directory.</span></span>

                ```JavaScript
                /*
                 * GET users listing.
                 */

                exports.list = function(req, res){
                  res.send("respond with a resource");
                };
                ```

 <span data-ttu-id="afc87-183">Tyto předají požadavek na našem zobrazení, včetně uživatele, pokud je k dispozici.</span><span class="sxs-lookup"><span data-stu-id="afc87-183">These pass along the request to our views, including the user if present.</span></span>

3. <span data-ttu-id="afc87-184">V kořenovém adresáři vytvořte zobrazení `/views/index.ejs`.</span><span class="sxs-lookup"><span data-stu-id="afc87-184">Create the `/views/index.ejs` view under the root directory.</span></span> <span data-ttu-id="afc87-185">Toto je jednoduchá stránka, která volá metody, naše přihlášení a odhlášení a umožňuje nám se získat informace o účtu.</span><span class="sxs-lookup"><span data-stu-id="afc87-185">This is a simple page that calls our login and logout methods and enables us to grab account information.</span></span> <span data-ttu-id="afc87-186">Všimněte si, že budeme moci použít podmínku `if (!user)` jako uživatel předávány prostřednictvím v požadavku je důkaz máme přihlášeného uživatele.</span><span class="sxs-lookup"><span data-stu-id="afc87-186">Notice that we can use the conditional `if (!user)` as the user being passed through in the request is evidence we have a signed-in user.</span></span>

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

4. <span data-ttu-id="afc87-187">Vytvořte `/views/account.ejs` zobrazení pod kořenovým adresářem, takže jsme můžete zobrazit další informace, `passport-azuread` pozastavil v požadavku uživatele.</span><span class="sxs-lookup"><span data-stu-id="afc87-187">Create the `/views/account.ejs` view under the root directory so that we can view additional information that `passport-azuread` has put in the user request.</span></span>

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

5. <span data-ttu-id="afc87-188">Umožňuje, aby tento vzhled dobrý přidání rozložení.</span><span class="sxs-lookup"><span data-stu-id="afc87-188">Let's make this look good by adding a layout.</span></span> <span data-ttu-id="afc87-189">Vytvořte ' / zobrazení se views/layout.ejs v kořenovém adresáři.</span><span class="sxs-lookup"><span data-stu-id="afc87-189">Create the '/views/layout.ejs' view under the root directory.</span></span>

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

##<a name="next-steps"></a><span data-ttu-id="afc87-190">Další kroky</span><span class="sxs-lookup"><span data-stu-id="afc87-190">Next steps</span></span>
<span data-ttu-id="afc87-191">Nakonec sestavte a spusťte aplikaci.</span><span class="sxs-lookup"><span data-stu-id="afc87-191">Finally, build and run your app.</span></span> <span data-ttu-id="afc87-192">Spustit `node app.js`a pak přejděte na `http://localhost:3000`.</span><span class="sxs-lookup"><span data-stu-id="afc87-192">Run `node app.js`, and then go to `http://localhost:3000`.</span></span>

<span data-ttu-id="afc87-193">Přihlaste se pomocí osobního účtu Microsoft nebo pracovní nebo školní účet a Všimněte si, jak se v seznamu /account projeví identitu uživatele.</span><span class="sxs-lookup"><span data-stu-id="afc87-193">Sign in with either a personal Microsoft account or a work or school account, and notice how the user's identity is reflected in the /account list.</span></span> <span data-ttu-id="afc87-194">Nyní máte webovou aplikaci, která je zabezpečen pomocí standardních oborových protokolech, které může ověřit uživatele s svoje osobní, tak i pracovní nebo školní účty.</span><span class="sxs-lookup"><span data-stu-id="afc87-194">You now have a web app that's secured with industry standard protocols that can authenticate users with both their personal and work/school accounts.</span></span>

<span data-ttu-id="afc87-195">Pro srovnání je hotová ukázka (bez vašich hodnot nastavení) [k dispozici jako soubor .zip](https://github.com/AzureADQuickStarts/WebApp-OpenIDConnect-NodeJS/archive/complete.zip).</span><span class="sxs-lookup"><span data-stu-id="afc87-195">For reference, the completed sample (without your configuration values) [is provided as a .zip file](https://github.com/AzureADQuickStarts/WebApp-OpenIDConnect-NodeJS/archive/complete.zip).</span></span> <span data-ttu-id="afc87-196">Alternativně můžete klonovat z Githubu:</span><span class="sxs-lookup"><span data-stu-id="afc87-196">Alternatively, you can clone it from GitHub:</span></span>

```git clone --branch complete https://github.com/AzureADQuickStarts/WebApp-OpenIDConnect-NodeJS.git```

<span data-ttu-id="afc87-197">Nyní se můžete přesunout na pokročilejší témata.</span><span class="sxs-lookup"><span data-stu-id="afc87-197">You can now move onto more advanced topics.</span></span> <span data-ttu-id="afc87-198">Můžete se pokusit:</span><span class="sxs-lookup"><span data-stu-id="afc87-198">You might want to try:</span></span>

[<span data-ttu-id="afc87-199">Zabezpečení webového rozhraní API pomocí Azure AD</span><span class="sxs-lookup"><span data-stu-id="afc87-199">Secure a Web API with Azure AD</span></span>](active-directory-devquickstarts-webapi-nodejs.md)

[!INCLUDE [active-directory-devquickstarts-additional-resources](../../../includes/active-directory-devquickstarts-additional-resources.md)]
