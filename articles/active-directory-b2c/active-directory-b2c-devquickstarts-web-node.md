---
title: "webové aplikace Node.js aaaAdd tooa přihlášení pro Azure B2C | Microsoft Docs"
description: "Jak toobuild webové aplikace Node.js, přihlašováním uživatelů pomocí klienta B2C."
services: active-directory-b2c
documentationcenter: 
author: dstrockis
manager: mbaldwin
editor: 
ms.assetid: db97f84a-1f24-447b-b6d2-0265c6896b27
ms.service: active-directory-b2c
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: javascript
ms.topic: hero-article
ms.date: 03/10/2017
ms.author: xerners
ms.openlocfilehash: b4c334b1f7a0669df2d0864140603dc55bbb5408
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-ad-b2c-add-sign-in-tooa-nodejs-web-app"></a>Azure AD B2C: Přidání webové aplikace Node.js tooa přihlášení

**Passport** je ověřovací middleware pro Node.js. Passport je velmi flexibilní a modulární a lze ho snadno nainstalovat v jakékoli webové aplikaci využívající Express nebo Restify. Komplexní sada strategií podporuje ověřování pomocí uživatelského jména a hesla, Facebooku, Twitteru a dalších.

Vyvinuli jsme strategii pro Azure Active Directory (Azure AD). Nainstalujete tento modul a poté přidejte hello Azure AD `passport-azure-ad` modulu plug-in.

toodo, budete muset:

1. Zaregistrovat aplikaci pomocí Azure AD.
2. Nastavení vaší aplikace hello toouse `passport-azure-ad` modulu plug-in.
3. Použijte Passport tooissue-přihlášení a odhlášení požadavky tooAzure AD.
4. Tisknout data o uživatelích.

Hello kód pro tento kurz [je udržovaný na Githubu](https://github.com/AzureADQuickStarts/B2C-WebApp-OpenIDConnect-NodeJS). toofollow společně, můžete [stáhnout kostru aplikace hello jako soubor ZIP](https://github.com/AzureADQuickStarts/B2C-WebApp-OpenIDConnect-NodeJS/archive/skeleton.zip). Můžete také hello kostru klonovat:

```git clone --branch skeleton https://github.com/AzureADQuickStarts/B2C-WebApp-OpenIDConnect-NodeJS.git```

aplikace Hello dokončit, je k dispozici na konci hello tohoto kurzu.

## <a name="get-an-azure-ad-b2c-directory"></a>Získání adresáře služby Azure AD B2C

Před použitím Azure AD B2C musíte vytvořit adresář, nebo klienta.  Adresář je kontejner pro všechny vaše uživatele, aplikace, skupiny a další. Pokud ho ještě nemáte, [vytvořte adresář B2C](active-directory-b2c-get-started.md) předtím, než budete pokračovat.

## <a name="create-an-application"></a>Vytvoření aplikace

Dále musíte toocreate aplikace ve svém adresáři B2C. Díky tomu získá informace o Azure AD, je nutné toocommunicate bezpečně s vaší aplikací. Obě hello klientské aplikace a webové rozhraní API budou odpovídat jedné **ID aplikace**, protože společně tvoří jednu logickou aplikaci. toocreate na aplikace, postupujte podle [tyto pokyny](active-directory-b2c-app-registration.md). Ujistěte se, že:

- Zahrnout **webové aplikace**/**webové rozhraní API** v aplikaci hello.
- Jste do pole **Adresa URL odpovědi** vyplnili `http://localhost:3000/auth/openid/return`. Je hello výchozí adresa URL pro tuto ukázku kódu.
- Vytvořte pro aplikaci **tajný klíč aplikace** a poznamenejte si ho. Budete ho potřebovat později. Všimněte si, že tato hodnota se musí toobe [uvozena v XML](https://www.w3.org/TR/2006/REC-xml11-20060816/#dt-escape) dříve, než ho použijete.
- Kopírování hello **ID aplikace** který je přiřazený tooyour aplikace. To také budete potřebovat později.

[!INCLUDE [active-directory-b2c-devquickstarts-v2-apps](../../includes/active-directory-b2c-devquickstarts-v2-apps.md)]

## <a name="create-your-policies"></a>Vytvořte svoje zásady

V Azure AD B2C je každé uživatelské rozhraní definováno [zásadou](active-directory-b2c-reference-policies.md). Tato aplikace obsahuje tři činnosti koncového uživatele: registrace, přihlášení a přihlášení pomocí Facebooku. Je třeba toocreate tuto zásadu každého typu, jak je popsáno v [článku o zásadách](active-directory-b2c-reference-policies.md#create-a-sign-up-policy). Když vytváříte tyto tři zásady, nezapomeňte:

- Zvolte hello **zobrazovaný název** a další atributy registrace ve svojí registrační zásadě.
- Zvolte hello **zobrazovaný název** a **ID objektu** deklarace identity aplikace v každé zásadě. Můžete zvolit i další deklarace identity.
- Kopírování hello **název** po jejím vytvoření každé zásady. Měl by mít předponu hello `b2c_1_`.  Tyto názvy zásad budete potřebovat později.

[!INCLUDE [active-directory-b2c-devquickstarts-policy](../../includes/active-directory-b2c-devquickstarts-policy.md)]

Po vytvoření tří zásad jste připravené toobuild vaší aplikace.

Všimněte si, že tento článek nepopisuje, jak zásady hello toouse jste právě vytvořili. toolearn o tom, jak fungují zásady v Azure AD B2C, začněte s hello [webové aplikace kurzem Začínáme .NET](active-directory-b2c-devquickstarts-web-dotnet.md).

## <a name="add-prerequisites-tooyour-directory"></a>Přidejte adresář tooyour požadavky

Z příkazového řádku hello změňte adresáře tooyour kořenové složky, pokud si nejste již existuje. Spusťte následující příkazy hello:

- `npm install express`
- `npm install ejs`
- `npm install ejs-locals`
- `npm install restify`
- `npm install mongoose`
- `npm install bunyan`
- `npm install assert-plus`
- `npm install passport`
- `npm install webfinger`
- `npm install body-parser`
- `npm install express-session`
- `npm install cookie-parser`

Kromě toho jsme použili `passport-azure-ad` pro náhled v kostře hello hello rychlý start naší.

- `npm install passport-azure-ad`

To nainstaluje knihovny hello který `passport-azure-ad` závisí na.

## <a name="set-up-your-app-toouse-hello-passport-nodejs-strategy"></a>Nastavení vaší aplikace toouse hello strategie Passport-Node.js
Nakonfigurujte hello Express middleware toouse hello ověřovacího protokolu OpenID Connect. Passport bude použité tooissue požadavků na přihlášení a odhlášení, správě uživatelských relací a získat informace o uživatelích, mimo jiné.

Otevřete hello `config.js` souboru v kořenovém hello hello projektu a zadejte hodnoty konfigurace vaší aplikace v hello `exports.creds` části.
- `clientID`: hello **ID aplikace** přiřazené tooyour aplikace v portálu pro registraci hello.
- `returnURL`: hello **identifikátor URI pro přesměrování** jste zadali v portálu hello.
- `tenantName`: název klienta hello vaší aplikace, například **contoso.onmicrosoft.com**.

[!INCLUDE [active-directory-b2c-devquickstarts-tenant-name](../../includes/active-directory-b2c-devquickstarts-tenant-name.md)]

Otevřete hello `app.js` soubor v kořenovém hello hello projektu. Přidejte následující volání tooinvoke hello hello `OIDCStrategy` strategie, která se dodává s `passport-azure-ad`.


```JavaScript
var OIDCStrategy = require('passport-azure-ad').OIDCStrategy;

// Add some logging
var log = bunyan.createLogger({
    name: 'Microsoft OIDC Example Web Application'
});
```

Použijte právě přidanou toohandle žádostí o přihlášení strategii hello.

```JavaScript
// Use hello OIDCStrategy in Passport (Section 2).
//
//   Strategies in Passport require a "validate" function that accepts
//   credentials (in this case, an OpenID identifier), and invokes a callback
//   by using a user object.
passport.use(new OIDCStrategy({
    callbackURL: config.creds.returnURL,
    realm: config.creds.realm,
    clientID: config.creds.clientID,
    oidcIssuer: config.creds.issuer,
    identityMetadata: config.creds.identityMetadata,
    skipUserProfile: config.creds.skipUserProfile,
    responseType: config.creds.responseType,
    responseMode: config.creds.responseMode,
    tenantName: config.creds.tenantName
  },
  function(iss, sub, profile, accessToken, refreshToken, done) {
    log.info('Example: Email address we received was: ', profile.email);
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
Passport používá podobný princip pro všechny strategie (včetně Twitteru a Facebooku). Vzor toothis řídí všichni tvůrci strategií. Když se podíváte na hello strategie, uvidíte předejte ji `function()` s token a `done` jako parametry hello. Hello strategie vrátí tooyou po dokončení veškeré práce. Uložení hello uživatele a skrytí tokenu hello tak, aby tooask pro ni není nutné znovu.

> [!IMPORTANT]
Hello předchozí kód přijme všechny uživatele, kterým ověří hello server. To je automatická registrace. Pokud používáte produkční servery, nechcete toolet mezi uživateli, pokud jste prošli registračním procesem, který jste nastavili. Tento princip je často k vidění u uživatelských aplikací. Ty umožňují tooregister pomocí Facebooku, ale poté vás vyzvou toofill Další informace. Naše aplikace nebyla pouze příklad, jsme pravděpodobně extrahování e-mailovou adresu z objektu tokenu hello, která je vrácena a pak požádejte uživatele toofill hello Další informace. Protože se jedná o testovací server, jednoduše přidáme uživatele databáze v paměti toohello.

Přidejte hello metody, které vám umožňují sledovat tookeep uživatelů, kteří mají přihlášení, jak vyžaduje Passport. To zahrnuje serializaci a deserializaci informací o uživateli:

```JavaScript

// Passport session setup. (Section 2)

//   toosupport persistent sign-in sessions, Passport needs toobe able to
//   serialize users into and deserialize users out of sessions. Typically,
//   this is as simple as storing hello user ID when Passport serializes a user
//   and finding hello user by ID when Passport deserializes that user.
passport.serializeUser(function(user, done) {
  done(null, user.email);
});

passport.deserializeUser(function(id, done) {
  findByEmail(id, function (err, user) {
    done(err, user);
  });
});

// Array toohold users who have signed in
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

Přidání modulu Express hello kód tooload hello. V následující hello, uvidíte, že používáme výchozí hello `/views` a `/routes` vzor, který poskytuje Express.

```JavaScript

// configure Express (Section 2)

var app = express();


app.configure(function() {
  app.set('views', __dirname + '/views');
  app.set('view engine', 'ejs');
  app.use(express.logger());
  app.use(express.methodOverride());
  app.use(cookieParser());
  app.use(expressSession({ secret: 'keyboard cat', resave: true, saveUninitialized: false }));
  app.use(bodyParser.urlencoded({ extended : true }));
  // Initialize Passport!  Also use passport.session() middleware toosupport
  // persistent sign-in sessions (recommended).
  app.use(passport.initialize());
  app.use(passport.session());
  app.use(app.router);
  app.use(express.static(__dirname + '/../../public'));
});

```

Přidat hello `POST` tras, které přebírají hello skutečné žádostí o přihlášení toohello `passport-azure-ad` modul:

```JavaScript

// Our Auth routes (Section 3)

// GET /auth/openid
//   Use passport.authenticate() as route middleware tooauthenticate the
//   request. hello first step in OpenID authentication involves redirecting
//   hello user tooan OpenID provider. After hello user is authenticated,
//   hello OpenID provider redirects hello user back toothis application at
//   /auth/openid/return

app.get('/auth/openid',
  passport.authenticate('azuread-openidconnect', { failureRedirect: '/login' }),
  function(req, res) {
    log.info('Authentication was called in hello Sample');
    res.redirect('/');
  });

// GET /auth/openid/return
//   Use passport.authenticate() as route middleware tooauthenticate the
//   request. If authentication fails, hello user will be redirected back toothe
//   sign-in page. Otherwise, hello primary route function will be called.
//   In this example, it redirects hello user toohello home page.
app.get('/auth/openid/return',
  passport.authenticate('azuread-openidconnect', { failureRedirect: '/login' }),
  function(req, res) {

    res.redirect('/');
  });

// POST /auth/openid/return
//   Use passport.authenticate() as route middleware tooauthenticate the
//   request. If authentication fails, hello user will be redirected back toothe
//   sign-in page. Otherwise, hello primary route function will be called.
//   In this example, it will redirect hello user toohello home page.

app.post('/auth/openid/return',
  passport.authenticate('azuread-openidconnect', { failureRedirect: '/login' }),
  function(req, res) {

    res.redirect('/');
  });
```

## <a name="use-passport-tooissue-sign-in-and-sign-out-requests-tooazure-ad"></a>Použít Passport tooissue-přihlášení a odhlášení požadavky tooAzure AD

Aplikace je nyní správně nakonfigurované toocommunicate s hello koncového bodu v2.0 pomocí ověřovacího protokolu OpenID Connect hello. `passport-azure-ad`má postaráno hello podrobnosti o věnujte zpráv ověřování, ověřování tokenů z Azure AD a údržbě uživatelské relace. Všechno, co zůstane je toogive vaši uživatelé toosign způsob, jak v a přihlaste se na více systémů a toogather Další informace o uživatele, kteří mají přihlášení.

Nejprve přidejte hello výchozí, přihlášení, účet a odhlášení metody tooyour `app.js` souboru:

```JavaScript

//Routes (Section 4)

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

tooreview tyto metody podrobně:
- Hello `/` trasa přesměruje toohello `index.ejs` zobrazení předáním hello uživatele v požadavku hello (pokud existuje).
- Hello `/account` trasy nejprve ověří, zda jste přihlášeni (hello implementace pro toto je níže). Pak předá hello uživatele v požadavku hello tak, aby mohli získat dodatečné informace o uživateli hello.
- Hello `/login` trasy volání hello `azuread-openidconnect` authenticator z `passport-azure-ad`. Pokud dojde k neúspěchu, trasa hello přesměruje uživatele hello zpět příliš`/login`.
- `/logout` jednoduše volá `logout.ejs` (a jeho trasu). Zruší soubory cookie a pak se vrátí hello zpět uživatele příliš`index.ejs`.


Pro poslední část hello `app.js`, přidejte hello `EnsureAuthenticated` metoda, která se používá v hello `/account` trasy.

```JavaScript

// Simple route middleware tooensure that hello user is authenticated. (Section 4)

//   Use this route middleware on any resource that needs toobe protected. If
//   hello request is authenticated (typically via a persistent sign-in session),
//   then hello request will proceed. Otherwise, hello user will be redirected toothe
//   sign-in page.
function ensureAuthenticated(req, res, next) {
  if (req.isAuthenticated()) { return next(); }
  res.redirect('/login')
}

```

Nakonec vytvořte samotný server hello v `app.js`.

```JavaScript

app.listen(3000);

```


## <a name="create-hello-views-and-routes-in-express-toocall-your-policies"></a>Vytvoření zobrazení hello a směruje v Express toocall zásad

Vaše `app.js` je nyní hotová. Bude třeba jen tooadd hello trasy a zobrazení, které vám umožňují toocall hello přihlášení a odhlášení zásady. Ty zpracovávají také dříve hello `/logout` a `/login` vytvořené trasy.

Vytvoření hello `/routes/index.js` trasy pod kořenovým adresářem hello.

```JavaScript

/*
 * GET home page.
 */

exports.index = function(req, res){
  res.render('index', { title: 'Express' });
};
```

Vytvoření hello `/routes/user.js` trasy pod kořenovým adresářem hello.

```JavaScript

/*
 * GET users listing.
 */

exports.list = function(req, res){
  res.send("respond with a resource");
};
```

Tyto jednoduché trasy předávají požadavky tooyour zobrazení. Obsahují hello uživatele, pokud je k dispozici.

Vytvoření hello `/views/index.ejs` zobrazení pod kořenovým adresářem hello. Toto je jednoduchá stránka, která volá zásady pro přihlášení a odhlášení. Můžete ji použít i toograb informace o účtu. Všimněte si, které můžete použít hello podmíněného `if (!user)` jako předávání hello uživatele v požadavku hello tooprovide důkaz, že uživatele hello je přihlášený.

```JavaScript
<% if (!user) { %>
    <h2>Welcome! Please sign in.</h2>
    <a href="/login/?p=your facebook policy">Sign in with Facebook</a>
    <a href="/login/?p=your email sign-in policy">Sign in with email</a>
    <a href="/login/?p=your email sign-up policy">Sign up with email</a>
<% } else { %>
    <h2>Hello, <%= user.displayName %>.</h2>
    <a href="/account">Account info</a></br>
    <a href="/logout">Log out</a>
<% } %>
```

Vytvoření hello `/views/account.ejs` zobrazení pod kořenovým adresářem hello, takže si můžete zobrazit další informace, `passport-azure-ad` put v požadavku uživatele hello.

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
<p>Full Claims</p>
<%- JSON.stringify(user) %>
<p></p>
<a href="/logout">Log Out</a>
<% } %>
```

Nyní můžete sestavit a spustit svoji aplikaci.

Spustit `node app.js` a přejděte příliš`http://localhost:3000`


Registrace nebo přihlášení toohello aplikace pomocí emailu nebo Facebooku. Odhlaste se a přihlaste se jako jiný uživatel.

##<a name="next-steps"></a>Další kroky

Pro referenci hello dokončit ukázka (bez vašich hodnot nastavení) [je k dispozici jako soubor ZIP](https://github.com/AzureADQuickStarts/B2C-WebApp-OpenIDConnect-NodeJS/archive/complete.zip). Můžete ho také klonovat z GitHubu:

```git clone --branch complete https://github.com/AzureADQuickStarts/B2C-WebApp-OpenIDConnect-nodejs.git```

Nyní se můžete přesunout na toomore advanced témata. Můžete vyzkoušet:

[Zabezpečení webového rozhraní API pomocí modelu hello B2C v Node.js](active-directory-b2c-devquickstarts-api-node.md)

<!--

For additional resources, check out:
You can now move on toomore advanced B2C topics. You might try:

[Call a Node.js web API from a Node.js web app]()

[Customizing hello your B2C App's UX >>]()

-->
