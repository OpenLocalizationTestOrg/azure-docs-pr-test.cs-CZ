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
# <a name="nodejs-web-app-sign-in-and-sign-out-with-azure-ad"></a>Webové aplikace Node.js přihlášení a odhlášení s Azure AD
Tady používáme Passport:

* Přihlašovací hello uživatele v aplikaci toohello službou Azure Active Directory (Azure AD).
* Zobrazení informací o uživateli hello.
* Přihlašovací hello uživatele mimo aplikaci hello.

Passport je ověřovací middleware pro Node.js. Flexibilní a modulární, Passport lze snadno vyřadit v tooany využívající Express nebo restify webové aplikace. Komplexní sada strategií podporují ověřování pomocí uživatelského jména a hesla, Facebook, Twitter a další. Vyvinuli jsme strategii pro Microsoft Azure Active Directory. Jsme nainstalujete tento modul a poté přidejte hello Microsoft Azure Active Directory `passport-azure-ad` modulu plug-in.

toodo hello tento, proveďte následující kroky:

1. Zaregistrujte aplikaci.
2. Nastavení vaší aplikace hello toouse `passport-azure-ad` strategie.
3. Použijte Passport tooissue-přihlášení a odhlášení požadavky tooAzure AD.
4. Tisknout data o uživateli hello.

Hello kód v tomto kurzu se udržuje [na Githubu](https://github.com/AzureADQuickStarts/WebApp-OpenIDConnect-NodeJS).  toofollow společně, můžete [stáhnout kostru aplikace hello jako soubor ZIP](https://github.com/AzureADQuickStarts/WebApp-OpenIDConnect-NodeJS/archive/skeleton.zip) nebo hello kostru klonovat:

```git clone --branch skeleton https://github.com/AzureADQuickStarts/WebApp-OpenIDConnect-NodeJS.git```

aplikace Hello dokončit, je k dispozici na konci hello také tohoto kurzu.

## <a name="step-1-register-an-app"></a>Krok 1: Registrace aplikace
1. Přihlaste se toohello [portál Azure](https://portal.azure.com).

2. V nabídce hello hello horní části stránky hello vyberte svůj účet. V části hello **Directory** vyberte místo, kam chcete tooregister klienta služby Active Directory hello vaší aplikace.

3. Vyberte **více služeb** v hello nabídky na hello levé strany úvodní obrazovka a pak vyberte **Azure Active Directory**.

4. Vyberte **registrace aplikace**a potom vyberte **přidat**.

5. Postupujte podle pokynů toocreate hello **webové aplikace** nebo **WebAPI**.
  * Hello **název** z hello aplikace popisuje toousers vaší aplikace.

  * Hello **přihlašovací adresa URL** je hello základní adresu URL aplikace.  Hello kostru je výchozí hodnota je ' http://localhost: 3000/auth/openid nebo vrátit hodnotu.

6. Po registraci, Azure AD přiřadí vaší aplikace ID jedinečný aplikace. Je třeba tuto hodnotu v hello následující oddíly, takže zkopírovat ze stránky aplikace hello.
7. Z hello **nastavení** -> **vlastnosti** stránky pro aplikace, aktualizujte hello identifikátor ID URI aplikace. Hello **identifikátor ID URI aplikace** je jedinečný identifikátor pro vaši aplikaci. konvence Hello je formát hello toouse `https://<tenant-domain>/<app-name>`, například: `https://contoso.onmicrosoft.com/my-first-aad-app`.

## <a name="step-2-add-prerequisites-tooyour-directory"></a>Krok 2: Přidejte adresář tooyour požadavky
1. Z příkazového řádku hello, změňte adresáře tooyour kořenové složky, pokud si nejste již existuje, a pak spusťte hello následující příkazy:

    * `npm install express`
    * `npm install ejs`
    * `npm install ejs-locals`
    * `npm install restify`
    * `npm install mongoose`
    * `npm install bunyan`
    * `npm install assert-plus`
    * `npm install passport`

2. Kromě toho musíte `passport-azure-ad`:
    * `npm install passport-azure-ad`

To nainstaluje knihovny hello který `passport-azure-ad` závisí na.

## <a name="step-3-set-up-your-app-toouse-hello-passport-node-js-strategy"></a>Krok 3: Nastavení strategie passport uzlu js hello toouse aplikace
Zde jsme nakonfigurovat Express toouse hello ověřovacího protokolu OpenID Connect.  Passport je použité toodo různé položky, včetně požadavků na přihlášení a odhlášení problém, spravovat relace hello uživatele a získat informace o uživateli hello.

1. toobegin, otevřete hello `config.js` souboru v kořenovém hello hello projektu a potom zadejte hodnoty konfigurace vaší aplikace v hello `exports.creds` části.

  * Hello `clientID` je hello **Id aplikace** který je přiřazený tooyour aplikace v portálu pro registraci hello.

  * Hello `returnURL` je hello **identifikátor Uri pro přesměrování** kterou jste zadali v portálu hello.

  * Hello `clientSecret` je hello tajný klíč, který jste vygenerovali hello portálu.

2. Dále otevřete hello `app.js` soubor v kořenovém hello hello projektu. Pak přidejte následující volání tooinvoke hello hello `OIDCStrategy` strategie, která se dodává s `passport-azure-ad`.

    ```JavaScript
    var OIDCStrategy = require('passport-azure-ad').OIDCStrategy;

    // add a logger

    var log = bunyan.createLogger({
    name: 'Microsoft OIDC Example Web Application'
    });
    ```

3. Potom použijte hello strategii jsme právě přidanou toohandle naše žádostí o přihlášení.

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
Passport používá podobný Princip pro všechny svoje strategie (Twitteru, Facebooku a tak dále), které řídí všichni autoři strategií k. Prohlížení hello strategie, uvidíte, že jsme předat funkci, která má token a done jako parametry hello. po jeho činnosti provede se dodává zpět toous strategie Hello. Potom chceme toostore hello uživatele a dočasné ukládání hello token, společnost Microsoft nepotřebuje tooask pro něj znovu.

> [!IMPORTANT]
Předchozí kód Hello trvá každý uživatel, který se stane tooauthenticate tooour serveru. To se označuje jako Automatická registrace. Doporučujeme vám, že nedošlo každý, kdo ověřovat tooa provozním serveru, aniž by bylo nejdříve je registrovat prostřednictvím procesu, který se rozhodnete. Je to obvykle hello vzor, který můžete vidět u uživatelských aplikací, které umožňují tooregister službou Facebook, ale poté vás požádají tooprovide Další informace. Pokud to nebyly ukázkovou aplikaci, mohli bychom extrahovat hello uživatele e-mailovou adresu z hello tokenu objektu, který je vrácen a poté požádat uživatele toofill hello Další informace. Protože se jedná o testovací server, přidáme je toohello databáze v paměti.


4. V dalším kroku přidejme hello metody, které nám umožní tootrack hello přihlášeného uživatele jako vyžaduje Passport. Tyto metody zahrnují serializaci a deserializaci informací o uživateli hello.

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

5.  V dalším kroku přidejme modulu Express hello kód tooload hello. Tady používáme hello výchozí /views a poskytuje /routes vzor, který Express.

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

6. Nakonec přidejme hello tras, že můžete předat hello skutečné žádostí o přihlášení toohello `passport-azure-ad` modul:


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


## <a name="step-4-use-passport-tooissue-sign-in-and-sign-out-requests-tooazure-ad"></a>Krok 4: Použití Passport tooissue přihlášení a odhlášení požadavky tooAzure AD
Aplikace je nyní správně nakonfigurované toocommunicate s koncovým bodem hello pomocí ověřovacího protokolu OpenID Connect hello.  `passport-azure-ad`má postaráno všech podrobností o hello věnujte zpráv ověřování, ověřování tokenů z Azure AD a správa uživatelských relací. Všechno, co zůstane je udělení uživatelům, odhlaste se a způsob toosign v a shromáždění Další informace o hello přihlášeného uživatele.

1. Nejprve přidejme hello výchozí, přihlášení, účet a odhlášení metody tooour `app.js` souboru:

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

2.  Pojďme si podrobněji:

  * Hello `/`trasa přesměruje toohello index.ejs zobrazení, předávání hello uživatele v požadavku hello (pokud existuje).
  * Hello `/account` směrovat nejprve *zajistí jsme se ověří* (jsme implementovat, v následující ukázka hello), a potom předává hello uživatele v požadavku hello tak, aby se nám můžete získat další informace o uživateli hello.
  * Hello `/login` trasy volá naše azuread openidconnect authenticator z `passport-azuread`. Pokud není úspěšné, přesměruje hello back příliš nebo přihlášení uživatele.
  * Hello `/logout` trasy jednoduše volá hello logout.ejs (a trasy), které vymaže soubory cookie a pak vrátí hello back tooindex.ejs uživatele.

3. Pro poslední část hello `app.js`, přidejme hello **EnsureAuthenticated** metoda, která se používá v `/account`, jako je uvedené výše.

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

4. Nakonec vytvořte samotný server hello v `app.js`:

```JavaScript

        app.listen(3000);

```


## <a name="step-5-toodisplay-our-user-in-hello-website-create-hello-views-and-routes-in-express"></a>Krok 5: toodisplay naše uživatele hello webu, vytvoření hello zobrazení a trasy v Express
Nyní `app.js` dokončení. Můžeme jednoduše potřebovat tooadd hello trasy a zobrazení tyto informace zobrazit hello jsme získat toohello uživatele, stejně jako zpracovává hello `/logout` a `/login` tras, které jsme vytvořili.

1. Vytvoření hello `/routes/index.js` trasy pod kořenovým adresářem hello.

    ```JavaScript
                /*
                 * GET home page.
                 */

                exports.index = function(req, res){
                  res.render('index', { title: 'Express' });
                };
    ```

2. Vytvoření hello `/routes/user.js` trasy pod kořenovým adresářem hello.

                ```JavaScript
                /*
                 * GET users listing.
                 */

                exports.list = function(req, res){
                  res.send("respond with a resource");
                };
                ```

 Tyto předají hello požadavek tooour zobrazení, včetně uživatelských hello, pokud je k dispozici.

3. Vytvoření hello `/views/index.ejs` zobrazení pod kořenovým adresářem hello. Toto je jednoduchá stránka, která volá metody, naše přihlášení a odhlášení a umožňuje nám informace o účtu toograb. Všimněte si, že můžeme použít hello podmíněného `if (!user)` jako uživatel hello předávány prostřednictvím v žádosti o hello je důkaz máme přihlášeného uživatele.

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

4. Vytvoření hello `/views/account.ejs` zobrazení pod kořenovým adresářem hello, takže jsme můžete zobrazit další informace, `passport-azuread` pozastavil v požadavku uživatele hello.

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

5. Umožňuje, aby tento vzhled dobrý přidání rozložení. Vytvoření hello "/ zobrazení se views/layout.ejs pod hello kořenový adresář.

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

##<a name="next-steps"></a>Další kroky
Nakonec sestavte a spusťte aplikaci. Spustit `node app.js`a potom přejděte příliš`http://localhost:3000`.

Přihlaste se pomocí osobního účtu Microsoft nebo pracovní nebo školní účet a Všimněte si, jak je identita uživatele hello projeví v seznamu /account hello. Nyní máte webovou aplikaci, která je zabezpečen pomocí standardních oborových protokolech, které může ověřit uživatele s svoje osobní, tak i pracovní nebo školní účty.

Pro referenci hello dokončit ukázka (bez vašich hodnot nastavení) [je k dispozici jako soubor ZIP](https://github.com/AzureADQuickStarts/WebApp-OpenIDConnect-NodeJS/archive/complete.zip). Alternativně můžete klonovat z Githubu:

```git clone --branch complete https://github.com/AzureADQuickStarts/WebApp-OpenIDConnect-NodeJS.git```

Nyní se můžete přesunout na pokročilejší témata. Můžete chtít tootry:

[Zabezpečení webového rozhraní API pomocí Azure AD](active-directory-devquickstarts-webapi-nodejs.md)

[!INCLUDE [active-directory-devquickstarts-additional-resources](../../../includes/active-directory-devquickstarts-additional-resources.md)]
