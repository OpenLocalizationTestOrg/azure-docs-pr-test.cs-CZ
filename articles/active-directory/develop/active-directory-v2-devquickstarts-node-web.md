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
# <a name="add-sign-in-tooa-nodejs-web-app"></a>Přidání webové aplikace Node.js tooa přihlášení

> [!NOTE]
> Ne všechny funkce a scénáře Azure Active Directory fungovat s koncovým bodem v2.0 hello. toodetermine zda byste měli používat koncového bodu v2.0 hello nebo koncový bod hello verze 1.0, přečtěte si informace o [v2.0 omezení](active-directory-v2-limitations.md).
> 

V tomto kurzu používáme Passport toodo hello následující úlohy:

* Ve webové aplikaci Přihlaste se pomocí služby Azure Active Directory (Azure AD) hello uživatele a hello koncového bodu v2.0.
* Zobrazení informací o uživateli hello.
* Přihlašovací hello uživatele mimo aplikaci hello.

**Passport** je ověřovací middleware pro Node.js. Flexibilní a modulární, můžete do jakéhokoli nenápadně vyřadit Passport využívající Express nebo restify webové aplikace. Komplexní sada strategií Passport, podporují ověřování pomocí uživatelského jména a hesla, Facebook, Twitter a další možnosti. Vyvinuli jsme strategii pro Azure AD. V tomto článku jsme ukážeme, jak tooinstall hello modul a poté přidejte hello Azure AD `passport-azure-ad` modulu plug-in.

## <a name="download"></a>Ke stažení
Hello kód v tomto kurzu se udržuje [na Githubu](https://github.com/AzureADQuickStarts/AppModelv2-WebApp-OpenIDConnect-nodejs). toofollow hello kurzu můžete [stáhnout kostru aplikace hello jako soubor ZIP](https://github.com/AzureADQuickStarts/AppModelv2-WebApp-OpenIDConnect-nodejs/archive/skeleton.zip) nebo hello kostru klonovat:

```git clone --branch skeleton https://github.com/AzureADQuickStarts/AppModelv2-WebApp-OpenIDConnect-nodejs.git```

Také můžete získat aplikace hello dokončit na konci hello tohoto kurzu.

## <a name="1-register-an-app"></a>1: registrace aplikace
Vytvoření nové aplikace v [apps.dev.microsoft.com](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList), nebo postupujte podle [tyto podrobné kroky](active-directory-v2-app-registration.md) tooregister aplikace. Ověřte, že je:

* Kopírování hello **Id aplikace** přiřazené tooyour aplikace. Budete ho potřebovat pro účely tohoto kurzu.
* Přidat hello **webové** platformu pro vaši aplikaci.
* Kopírování hello **identifikátor URI pro přesměrování** z portálu hello. Musíte použít hello výchozí hodnota URI `urn:ietf:wg:oauth:2.0:oob`.

## <a name="2-add-prerequisities-tooyour-directory"></a>2: Přidat prerequisities tooyour adresář
Na příkazovém řádku změňte adresáře toogo tooyour kořenové složky, pokud si nejste již existuje. Spusťte následující příkazy hello:

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

Kromě toho používáme `passport-azure-ad` v hello kostře rychlého startu hello:

* `npm install passport-azure-ad`

To nainstaluje knihovny hello který `passport-azure-ad` používá.

## <a name="3-set-up-your-app-toouse-hello-passport-node-js-strategy"></a>3: nastavení strategie passport uzlu js hello toouse aplikace
Nastavte hello Express middleware toouse hello ověřovacího protokolu OpenID Connect. Použití požadavků na přihlášení a odhlášení tooissue Passport, spravovat relace hello uživatele a získat informace o uživateli hello, mimo jiné.

1.  V kořenovém hello hello projektu otevřete hello souboru Config.js. V hello `exports.creds` zadejte hodnoty konfigurace vaší aplikace.
  
  * `clientID`: hello **Id aplikace** který je přiřazený tooyour aplikace v hello portálu Azure.
  * `returnURL`: hello **identifikátor URI pro přesměrování** kterou jste zadali v portálu hello.
  * `clientSecret`: hello tajný klíč, který jste vygenerovali hello portálu.

2.  V kořenovém hello hello projektu otevřete soubor App.js hello. tooinvoke hello OIDCStrategy stratey, který se dodává s `passport-azure-ad`, přidejte následující volání hello:

  ```JavaScript
  var OIDCStrategy = require('passport-azure-ad').OIDCStrategy;

  // Add some logging
  var log = bunyan.createLogger({
      name: 'Microsoft OIDC Example Web Application'
  });
  ```

3.  toohandle vaší žádostí o přihlášení, použijte hello strategie, kterou jste právě přidanou:

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

Passport používá podobný Princip pro všechny svoje strategie (Twitteru, Facebooku a tak dále). Vzor toohello řídí všichni tvůrci strategií. Předat hello strategie `function()` token, který používá a `done` jako parametry. strategie Hello se vrátí po dělá svou práci. Uložení hello uživatele a dočasné ukládání hello token, takže není nutné tooask pro něj znovu.

  > [!IMPORTANT]
  > Hello předchozí kód přijme všechny uživatele, který může ověřit tooyour serveru. To se označuje jako Automatická registrace. Na provozním serveru nebude chcete toolet každý, kdo v bez toho, aby předtím prošli registračním procesem, který zvolíte. Je to obvykle hello vzor, který můžete vidět u uživatelských aplikací. aplikace Hello může povolit tooregister službou Facebook, ale pak musíte tooenter Další informace. Pokud v tomto kurzu nebyly pomocí příkazového řádku programu, může extrahuje hello e-mailu z hello tokenu objekt, který je vrácen. Potom může zobrazit dotaz hello uživatele tooenter Další informace. Protože se jedná o testovací server, můžete přidat uživatele hello přímo toohello databázi v paměti.
  > 
  > 

4.  Přidejte metody hello použít tookeep sledování uživatelů, kteří se přihlásili, jak vyžaduje Passport. To zahrnuje serializaci a deserializaci informací o uživateli hello:

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

5.  Přidejte hello kód, který načte hello modulu Express. Použijte výchozí /views hello a /routes vzor, který Express poskytuje:

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

6.  Přidat hello POST tras, že můžete předat hello skutečné žádostí o přihlášení toohello `passport-azure-ad` modul:

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

## <a name="4-use-passport-tooissue-sign-in-and-sign-out-requests-tooazure-ad"></a>4: použití Passport tooissue přihlášení a odhlášení požadavky tooAzure AD
Aplikace je teď nastavený toocommunicate s hello koncového bodu v2.0 pomocí ověřovacího protokolu OpenID Connect hello. Hello `passport-azure-ad` strategie postará všechny podrobnosti hello věnujte zpráv ověřování, ověřování tokenů z Azure AD a údržbě hello uživatelské relace. Všechny, který se nechá toodo je toogive uživatelům způsob toosign v a přihlaste se na více systémů a toogather Další informace o hello uživatele, který je přihlášený.

1.  Přidat hello **výchozí**, **přihlášení**, **účet**, a **odhlášení** souboru App.js tooyour metody:

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

  Zde jsou hello podrobnosti:
    
    * Hello `/` trasa přesměruje toohello index.ejs zobrazení. Pak předá hello uživatele v požadavku hello (pokud existuje).
    * Hello `/account` směrovat nejprve *zajistí, že jsou ověří* (můžete implementovat, hello následující kód). Pak předá hello uživatele v požadavku hello. Toto je, abyste získali další informace o uživateli hello.
    * Hello `/login` směrování volání vaše `azuread-openidconnect` authenticator z `passport-azuread`. Pokud není úspěšné, je přesměrován zpět uživatele hello příliš`/login`.
    * Hello `/logout` trasy volá hello logout.ejs zobrazení (a směrování). Zruší soubory cookie a pak vrátí hello back tooindex.ejs uživatele.

2.  Přidat hello **EnsureAuthenticated** metoda, která jste použili výše v `/account`:

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

3.  V App.js vytvořte hello serveru:

  ```JavaScript

  app.listen(3000);

  ```


## <a name="5-create-hello-views-and-routes-in-express-that-you-show-your-user-on-hello-website"></a>5: vytvoření hello zobrazení a trasy v Express uživatelům zobrazí na webu hello
Přidejte hello trasy a zobrazení, které se zobrazí informace o toohello uživatele. Hello trasy a zobrazení, zpracovávají také dříve hello `/logout` a `/login` tras, které jste vytvořili.

1. V kořenovém adresáři hello, vytvořte hello `/routes/index.js` trasy.

  ```JavaScript

  /*
  * GET home page.
  */

  exports.index = function(req, res){
    res.render('index', { title: 'Express' });
  };
  ```

2.  V kořenovém adresáři hello, vytvořte hello `/routes/user.js` trasy.

  ```JavaScript

  /*
  * GET users listing.
  */

  exports.list = function(req, res){
    res.send("respond with a resource");
  };
  ```

  `/routes/index.js`a `/routes/user.js` jsou jednoduché trasy, které předávají hello požadavek tooyour zobrazení, včetně hello uživatele, pokud je k dispozici.

3.  V kořenovém adresáři hello, vytvořte hello `/views/index.ejs` zobrazení. Tato stránka volá vaše **přihlášení** a **odhlášení** metody. Můžete také použít hello `/views/index.ejs` zobrazit informace o účtu toocapture. Můžete použít hello podmíněného `if (!user)` jako uživatel hello předávány prostřednictvím v žádosti o hello. Je důkaz, že máte přihlášení uživatele.

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

4.  V kořenovém adresáři hello, vytvořte hello `/views/account.ejs` zobrazení. Hello `/views/account.ejs` zobrazení můžete další informace o tooview, `passport-azuread` převádí na žádost uživatele hello.

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

5.  Přidáte rozložení. V kořenovém adresáři hello, vytvořte hello `/views/layout.ejs` zobrazení.

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

6.  toobuild a spuštění aplikace, spustit `node app.js`. Pak přejděte příliš`http://localhost:3000`.

7.  Přihlaste se pomocí osobního účtu Microsoft nebo pracovní nebo školní účet. Upozorňujeme, že identita hello uživatele v seznamu /account hello. 

Nyní máte webovou aplikaci, která je zabezpečená službou pomocí standardních protokolů. Uživatelé ve vaší aplikaci, můžete ověřovat pomocí svoje osobní i pracovní nebo školní účty.

## <a name="next-steps"></a>Další kroky
Pro srovnání je hello dokončit ukázka (bez vašich hodnot nastavení) k dispozici jako [soubor .zip](https://github.com/AzureADQuickStarts/AppModelv2-WebApp-OpenIDConnect-nodejs/archive/complete.zip). Vám může ho také klonovat z Githubu:

```git clone --branch complete https://github.com/AzureADQuickStarts/AppModelv2-WebApp-OpenIDConnect-nodejs.git```

Potom můžete přesunout na toomore advanced témata. Můžete chtít tootry:

[Zabezpečit webové rozhraní API Node.js pomocí koncového bodu v2.0 hello](active-directory-v2-devquickstarts-node-api.md)

Zde jsou některé další zdroje informací:

* [Příručka vývojáře v2.0 služby Azure AD](active-directory-appmodel-v2-overview.md)
* [Značka "azure-active-directory" přetečení zásobníku](http://stackoverflow.com/questions/tagged/azure-active-directory)

### <a name="get-security-updates-for-our-products"></a>Získejte bezpečnostní aktualizace našich produktů
Doporučujeme toosign až toobe upozorněni, když bezpečnostních incidentech. Na hello [technické oznámení zabezpečení Microsoft](https://technet.microsoft.com/security/dd252948) stránky, přihlášení k odběru tooSecurity zpravodaje výstrahy.

