---
title: "Azure AD B2C: Zabezpečení webového rozhraní API pomocí Node.js | Dokumentace Microsoftu"
description: "Jak toobuild Node.js webové rozhraní API, které přijímá tokeny z klienta B2C"
services: active-directory-b2c
documentationcenter: 
author: dstrockis
manager: mbaldwin
editor: 
ms.assetid: fc2b9af8-fbda-44e0-962a-8b963449106a
ms.service: active-directory-b2c
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: javascript
ms.topic: hero-article
ms.date: 01/07/2017
ms.author: xerners
ms.openlocfilehash: 47f5bae025a9ba2f486e36acef36aa37cfb43543
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-ad-b2c-secure-a-web-api-by-using-nodejs"></a>Azure AD B2C: Zabezpečení webového rozhraní API pomocí Node.js
<!-- TODO [AZURE.INCLUDE [active-directory-b2c-devquickstarts-web-switcher](../../includes/active-directory-b2c-devquickstarts-web-switcher.md)]-->

S Azure Active Directory (Azure AD) B2C můžete zabezpečit webové rozhraní API pomocí přístupových tokenů OAuth 2.0. Tyto tokeny umožňují vaší klientské aplikace, které používají Azure AD B2C tooauthenticate toohello API. Tento článek ukazuje, jak toocreate rozhraní API "seznam úkolů", umožňuje uživatelům tooadd a seznam úloh. Hello webového rozhraní API zabezpečené pomocí Azure AD B2C a pouze umožňuje ověřeným uživatelům toomanage jejich seznamu úkolů.

> [!NOTE]
> Tato ukázka byla napsána toobe připojené tooby pomocí našich [ukázkové aplikace iOS B2C](active-directory-b2c-devquickstarts-ios.md). Nejprve hello aktuální návodu a pak postupujte podle ukázky.
>
>

**Passport** je ověřovací middleware pro Node.js. Passport je flexibilní a modulární a lze ho snadno nainstalovat v jakékoli webové aplikaci využívající Express nebo Restify. Komplexní sada strategií podporuje ověřování pomocí uživatelského jména a hesla, Facebooku, Twitteru a dalších. Vyvinuli jsme strategii pro Azure Active Directory (Azure AD). Nainstalujete tento modul a poté přidejte hello Azure AD `passport-azure-ad` modulu plug-in.

toodo této ukázce, budete muset:

1. Zaregistrovat aplikaci s Azure AD.
2. Nastavení vaší aplikace toouse Passport je `azure-ad-passport` modulu plug-in.
3. Konfigurace klienta aplikace toocall hello "seznam úkolů" webového rozhraní API.

## <a name="get-an-azure-ad-b2c-directory"></a>Získání adresáře služby Azure AD B2C
Před použitím Azure AD B2C musíte vytvořit adresář, nebo klienta.  Adresář je kontejner pro všechny uživatele, aplikace, skupiny a další.  Pokud ho ještě nemáte, [vytvořte adresář B2C](active-directory-b2c-get-started.md) předtím, než budete pokračovat.

## <a name="create-an-application"></a>Vytvoření aplikace
Dále musíte toocreate aplikace ve svém adresáři B2C, která poskytne Azure AD nějaké informace, že tato služba vyžaduje toosecurely komunikaci s vaší aplikací. V takovém případě hello klientská aplikace i webové rozhraní API jsou reprezentované pomocí jedné **ID aplikace**, protože společně tvoří jednu logickou aplikaci. toocreate na aplikace, postupujte podle [tyto pokyny](active-directory-b2c-app-registration.md). Ujistěte se, že:

* Zahrnout **webovou aplikaci nebo webové rozhraní api** v aplikaci hello
* Jste do pole **Adresa URL odpovědi** vyplnili `http://localhost/TodoListService`. Je hello výchozí adresa URL pro tuto ukázku kódu.
* Vytvořte pro aplikaci **tajný klíč aplikace** a poznamenejte si ho. Tato data budete potřebovat později. Všimněte si, že tato hodnota se musí toobe [uvozena v XML](https://www.w3.org/TR/2006/REC-xml11-20060816/#dt-escape) dříve, než ho použijete.
* Kopírování hello **ID aplikace** který je přiřazený tooyour aplikace. Tato data budete potřebovat později.

[!INCLUDE [active-directory-b2c-devquickstarts-v2-apps](../../includes/active-directory-b2c-devquickstarts-v2-apps.md)]

## <a name="create-your-policies"></a>Vytvořte svoje zásady
V Azure AD B2C je každé uživatelské rozhraní definováno [zásadou](active-directory-b2c-reference-policies.md). Tato aplikace obsahuje dvě možnosti pro identitu: registraci a přihlášení. Je třeba jedna zásada toocreate každého typu, jak je popsáno v [článku o zásadách](active-directory-b2c-reference-policies.md#create-a-sign-up-policy).  Když vytváříte tyto tři zásady, nezapomeňte:

* Zvolte hello **zobrazovaný název** a další atributy registrace ve svojí registrační zásadě.
* Zvolte hello **zobrazovaný název** a **ID objektu** deklarace identity aplikace v každé zásadě.  Můžete zvolit i další deklarace identity.
* Poznamenejte hello **název** po jejím vytvoření každé zásady. Měl by mít předponu hello `b2c_1_`.  Tyto názvy zásad budete potřebovat později.

[!INCLUDE [active-directory-b2c-devquickstarts-policy](../../includes/active-directory-b2c-devquickstarts-policy.md)]

Po vytvoření tří zásad jste připravené toobuild vaší aplikace.

toolearn o tom, jak fungují zásady v Azure AD B2C, začněte s hello [webové aplikace kurzem Začínáme .NET](active-directory-b2c-devquickstarts-web-dotnet.md).

## <a name="download-hello-code"></a>Stáhněte si kód hello
Hello kód pro tento kurz [je udržovaný na Githubu](https://github.com/AzureADQuickStarts/B2C-WebAPI-NodeJS). Ukázka hello toobuild jako můžete přejít, můžete [stáhnout kostru projektu v souboru ZIP](https://github.com/AzureADQuickStarts/B2C-WebAPI-NodeJS/archive/skeleton.zip). Můžete také hello kostru klonovat:

```
git clone --branch skeleton https://github.com/AzureADQuickStarts/B2C-WebAPI-NodeJS.git
```

aplikace Hello dokončit, je také [k dispozici jako soubor ZIP](https://github.com/AzureADQuickStarts/B2C-WebAPI-NodeJS/archive/complete.zip) nebo na hello `complete` větve hello stejného úložiště.

## <a name="download-nodejs-for-your-platform"></a>Stažení Node.js pro vaši platformu
toosuccessfully tuto ukázku použít, potřebujete funkční instalací Node.js.

Nainstalujte si Node.js z [nodejs.org](http://nodejs.org).

## <a name="install-mongodb-for-your-platform"></a>Instalace MongoDB pro vaši platformu
toosuccessfully tuto ukázku použít, potřebujete funkční instalací MongoDB. MongoDB toomake používáme napříč instancemi serveru trvalé rozhraní REST API.

Nainstalujte MongoDB z [mongodb.org](http://www.mongodb.org).

> [!NOTE]
> Tento podrobný návod předpokládá, že používáte hello výchozí instalaci a koncové body serveru pro MongoDB, které jsou v době psaní tohoto textu hello je `mongodb://localhost`.
>
>

## <a name="install-hello-restify-modules-in-your-web-api"></a>Instalace modulů Restify hello ve vašem webovém rozhraní API
Restify toobuild používáme REST API. Restify je minimalistické a flexibilní rozhraní Node.js odvozené z Express. Obsahuje robustní sadu funkcí pro sestavování rozhraní REST API postavených na protokolu Connect.

### <a name="install-restify"></a>Instalace Restify
Hello příkazovém řádku přejděte do adresáře příliš`azuread`. Pokud hello `azuread` adresář neexistuje, vytvořte ho.

`cd azuread` nebo `mkdir azuread;`

Zadejte hello následující příkaz:

`npm install restify`

Tento příkaz nainstaluje Restify.

#### <a name="did-you-get-an-error"></a>Obdrželi jste chybu?
V některých operačních systémech, při použití `npm`, může se zobrazit chyba hello `Error: EPERM, chmod '/usr/local/bin/..'` a žádost o hello účet Spustit jako správce. Pokud vznikne tento problém používat hello `sudo` příkaz toorun `npm` na vyšší úrovni oprávnění.

#### <a name="did-you-get-a-dtrace-error"></a>Obdrželi jste chybu DTrace?
Při instalaci Restify se může zobrazit podobný text:

```Shell
clang: error: no such file or directory: 'HD/azuread/node_modules/restify/node_modules/dtrace-provider/libusdt'
make: *** [Release/DTraceProviderBindings.node] Error 1
gyp ERR! build error
gyp ERR! stack Error: `make` failed with exit code: 2
gyp ERR! stack     at ChildProcess.onExit (/usr/local/lib/node_modules/npm/node_modules/node-gyp/lib/build.js:267:23)
gyp ERR! stack     at ChildProcess.EventEmitter.emit (events.js:98:17)
gyp ERR! stack     at Process.ChildProcess._handle.onexit (child_process.js:789:12)
gyp ERR! System Darwin 13.1.0
gyp ERR! command "node" "/usr/local/lib/node_modules/npm/node_modules/node-gyp/bin/node-gyp.js" "rebuild"
gyp ERR! cwd /Volumes/Development HD/azuread/node_modules/restify/node_modules/dtrace-provider
gyp ERR! node -v v0.10.11
gyp ERR! node-gyp -v v0.10.0
gyp ERR! not ok
npm WARN optional dep failed, continuing dtrace-provider@0.2.8
```

Restify poskytuje výkonný mechanismus pro trasování volání REST pomocí DTrace. Nicméně, na mnoha operačních systémech není DTrace k dispozici. Tyto chyby můžete bezpečně ignorovat.

výstup Hello hello příkazu by měla vypadat podobně jako toothis text:

    restify@2.6.1 node_modules/restify
    ├── assert-plus@0.1.4
    ├── once@1.3.0
    ├── deep-equal@0.0.0
    ├── escape-regexp-component@1.0.2
    ├── qs@0.6.5
    ├── tunnel-agent@0.3.0
    ├── keep-alive-agent@0.0.1
    ├── lru-cache@2.3.1
    ├── node-uuid@1.4.0
    ├── negotiator@0.3.0
    ├── mime@1.2.11
    ├── semver@2.2.1
    ├── spdy@1.14.12
    ├── backoff@2.3.0
    ├── formidable@1.0.14
    ├── verror@1.3.6 (extsprintf@1.0.2)
    ├── csv@0.3.6
    ├── http-signature@0.10.0 (assert-plus@0.1.2, asn1@0.1.11, ctype@0.5.2)
    └── bunyan@0.22.0 (mv@0.0.5)

## <a name="install-passport-in-your-web-api"></a>Instalace Passportu ve vašem webovém rozhraní API
Hello příkazovém řádku přejděte do adresáře příliš`azuread`, pokud ještě není.

Instalace Passportu pomocí hello následující příkaz:

`npm install passport`

Hello výstup hello příkazu by měl vypadat podobně jako toothis text:

    passport@0.1.17 node_modules\passport
    ├── pause@0.0.1
    └── pkginfo@0.2.3

## <a name="add-passport-azuread-tooyour-web-api"></a>Přidání passport-azuread tooyour webového rozhraní API
V dalším kroku přidejte strategii OAuth hello pomocí `passport-azuread`, sady strategií, které propojují Azure AD s Passport. Použijte tuto strategii pro nosné tokeny v ukázce REST API hello.

> [!NOTE]
> Přestože OAuth2 poskytuje rozhraní, ve kterém mohou být vydávané všechny známé typy tokenů, rozšířeného využití se dostalo pouze určitým typům tokenů. Hello tokeny pro ochranu koncových bodů jsou nosné tokeny. Tyto typy tokenů jsou hello nejčastěji vydané v OAuth2. Mnoho implementací předpokládá, že jsou nosné tokeny jediným typem vydávaných tokenů hello.
>
>

Hello příkazovém řádku přejděte do adresáře příliš`azuread`, pokud ještě není.

Nainstalujte hello Passport `passport-azure-ad` modulu pomocí hello následující příkaz:

`npm install passport-azure-ad`

Hello výstup hello příkazu by měl vypadat podobně jako toothis text:

``
passport-azure-ad@1.0.0 node_modules/passport-azure-ad
├── xtend@4.0.0
├── xmldom@0.1.19
├── passport-http-bearer@1.0.1 (passport-strategy@1.0.0)
├── underscore@1.8.3
├── async@1.3.0
├── jsonwebtoken@5.0.2
├── xml-crypto@0.5.27 (xpath.js@1.0.6)
├── ursa@0.8.5 (bindings@1.2.1, nan@1.8.4)
├── jws@3.0.0 (jwa@1.0.1, base64url@1.0.4)
├── request@2.58.0 (caseless@0.10.0, aws-sign2@0.5.0, forever-agent@0.6.1, stringstream@0.0.4, tunnel-agent@0.4.1, oauth-sign@0.8.0, isstream@0.1.2, extend@2.0.1, json-stringify-safe@5.0.1, node-uuid@1.4.3, qs@3.1.0, combined-stream@1.0.5, mime-types@2.0.14, form-data@1.0.0-rc1, http-signature@0.11.0, bl@0.9.4, tough-cookie@2.0.0, hawk@2.3.1, har-validator@1.8.0)
└── xml2js@0.4.9 (sax@0.6.1, xmlbuilder@2.6.4)
``

## <a name="add-mongodb-modules-tooyour-web-api"></a>Přidání modulů MongoDB tooyour rozhraní web API
V této ukázce se jako úložiště dat používá MongoDB. Pro tento účel nainstalujte Mongoose, což je běžně používaný modul plug-in pro správu modelů a schémat.

* `npm install mongoose`

## <a name="install-additional-modules"></a>Instalace dalších modulů
Dále nainstalujte zbývající požadované moduly hello.

Hello příkazovém řádku přejděte do adresáře příliš`azuread`, pokud ještě není:

`cd azuread`

Instalace modulů hello ve vaší `node_modules` directory:

* `npm install assert-plus`
* `npm install ejs`
* `npm install ejs-locals`
* `npm install express`
* `npm install bunyan`

## <a name="create-a-serverjs-file-with-your-dependencies"></a>Vytvoření souboru server.js se závislostmi
Hello `server.js` soubor poskytuje hello většina hello funkce pro server webového rozhraní API.

Hello příkazovém řádku přejděte do adresáře příliš`azuread`, pokud ještě není:

`cd azuread`

V editoru vytvořte soubor `server.js`. Přidejte hello následující informace:

```Javascript
'use strict';
/**
* Module dependencies.
*/
var fs = require('fs');
var path = require('path');
var util = require('util');
var assert = require('assert-plus');
var mongoose = require('mongoose/');
var bunyan = require('bunyan');
var restify = require('restify');
var config = require('./config');
var passport = require('passport');
var OIDCBearerStrategy = require('passport-azure-ad').BearerStrategy;
```

Uložte soubor hello. Vrátí tooit později.

## <a name="create-a-configjs-file-toostore-your-azure-ad-settings"></a>Vytvoření toostore souboru config.js nastavení Azure AD
Tento soubor s kódem předává parametry konfigurace hello z vaší portálu Azure AD toohello `Passport.js` souboru. Tyto hodnoty konfigurace jste vytvořili, když jste přidali hello webové rozhraní API toohello portálu v první části návodu hello hello. Po zkopírování hello kód objasníme, jaké tooput v hello hodnoty těchto parametrů.

Hello příkazovém řádku přejděte do adresáře příliš`azuread`, pokud ještě není:

`cd azuread`

V editoru vytvořte soubor `config.js`. Přidejte hello následující informace:

```Javascript
// Don't commit this file tooyour public repos. This config is for first-run
exports.creds = {
clientID: <your client ID for this Web API you created in hello portal>
mongoose_auth_local: 'mongodb://localhost/tasklist', // Your mongo auth uri goes here
audience: '<your audience URI>', // hello Client ID of hello application that is calling your API, usually a web API or native client
identityMetadata: 'https://login.microsoftonline.com/<tenant name>/.well-known/openid-configuration', // Make sure you add hello B2C tenant name in hello <tenant name> area
tenantName:'<tenant name>',
policyName:'b2c_1_<sign in policy name>' // This is hello policy you'll want toovalidate against in B2C. Usually this is your Sign-in policy (as users sign in toothis API)
passReqToCallback: false // This is a node.js construct that lets you pass hello req all hello way back tooany upstream caller. We turn this off as there is no upstream caller.
};

```

[!INCLUDE [active-directory-b2c-devquickstarts-tenant-name](../../includes/active-directory-b2c-devquickstarts-tenant-name.md)]

### <a name="required-values"></a>Požadované hodnoty
`clientID`: hello ID klienta aplikace webového rozhraní API.

`IdentityMetadata`: Tady bude `passport-azure-ad` vyhledá konfigurační data pro zprostředkovatele identity hello. Vypadá to také pro webové tokeny hello JSON toovalidate hello klíče.

`audience`: identifikátor URI (URI) z hello portálu, který identifikuje volající aplikace hello.

`tenantName`: Název vašeho klienta (například **contoso.onmicrosoft.com**).

`policyName`: hello zásady, které chcete toovalidate hello tokenů přicházejících tooyour serveru. Tato zásada by měla být hello stejné zásady, které používáte pro přihlášení na hello klientské aplikace.

> [!NOTE]
> Prozatím použijte hello stejné zásady v nastavení klient i server. Pokud jste již dokončili návod a tyto zásady vytvořili, nemusíte toodo proto znovu. Vzhledem k tomu, že jste dokončili návod hello, neměli byste potřebovat tooset si nové zásady u návodů pro klienty v lokalitě hello.
>
>

## <a name="add-configuration-tooyour-serverjs-file"></a>Přidání konfiguračního souboru server.js tooyour
tooread hello hodnoty z hello `config.js` souboru, kterou jste vytvořili, přidejte hello `.config` souboru jako požadovaný prostředek vaší aplikace a pak nastavte globální proměnné toothose hello v hello `config.js` dokumentu.

Hello příkazovém řádku přejděte do adresáře příliš`azuread`, pokud ještě není:

`cd azuread`

Otevřete hello `server.js` souboru v editoru. Přidejte hello následující informace:

```Javascript
var config = require('./config');
```
Přidání nové části příliš`server.js` hello následující kód, který obsahuje:

```Javascript
// We pass these options in toohello ODICBearerStrategy.

var options = {
    // hello URL of hello metadata document for your app. We put hello keys for token validation from hello URL found in hello jwks_uri tag of hello in hello metadata.
    identityMetadata: config.creds.identityMetadata,
    clientID: config.creds.clientID,
    tenantName: config.creds.tenantName,
    policyName: config.creds.policyName,
    validateIssuer: config.creds.validateIssuer,
    audience: config.creds.audience,
    passReqToCallback: config.creds.passReqToCallback

};
```

V dalším kroku přidejme některé zástupné symboly pro uživatele hello obdržíme od našich volání aplikací.

```Javascript
// array toohold logged in users and hello current logged in user (owner)
var users = [];
var owner = null;
```

Nyní pokračujme a vytvořme i protokolovací nástroj.

```Javascript
// Our logger
var log = bunyan.createLogger({
    name: 'Microsoft Azure Active Directory Sample'
});
```

## <a name="add-hello-mongodb-model-and-schema-information-by-using-mongoose"></a>Přidat hello informace modelu a schématu MongoDB pomocí Mongoose
Hello předchozí Příprava platí přineste tyto tři soubory dohromady ve službu REST API.

Pro tento návod použijte MongoDB toostore úkolů, jak je popsáno výše.

V hello `config.js` souboru, jste nazvali databázi **tasklist**. Tento název se taky uveďte na konci hello hello `mongoose_auth_local` adresy URL pro připojení. Nepotřebujete toocreate tato databáze předem v MongoDB. Vytvoří hello databáze pro vás při hello prvního spuštění vaší serverové aplikace.

Poté, co sdělíte serveru hello které toouse databázi MongoDB, musíte toowrite některé další kód toocreate hello modelu a schématu pro serverové úlohy.

### <a name="expand-hello-model"></a>Rozbalte položku modelu hello
Tento model schématu je jednoduchý. Podle potřeby ho můžete rozšířit.

`owner`: Kdo je přiřazen toohello úloh. Tento objekt je typu **string**.  

`Text`: vlastní úloha hello. Tento objekt je typu **string**.

`date`: data hello je kvůli tuto úlohu hello. Tento objekt je typu **datetime**.

`completed`: Pokud hello úkol je dokončen. Tento objekt je typu **Boolean**.

### <a name="create-hello-schema-in-hello-code"></a>Vytvoření hello schématu v kódu hello
Hello příkazovém řádku přejděte do adresáře příliš`azuread`, pokud ještě není:

`cd azuread`

Otevřete hello `server.js` souboru v editoru. Přidejte následující informace pod položku konfigurace hello hello:

```Javascript
// MongoDB setup
// Setup some configuration
var serverPort = process.env.PORT || 3000; // Note we are hosting our API on port 3000
var serverURI = (process.env.PORT) ? config.creds.mongoose_auth_mongohq : config.creds.mongoose_auth_local;

// Connect tooMongoDB
global.db = mongoose.connect(serverURI);
var Schema = mongoose.Schema;
log.info('MongoDB Schema loaded');

// Here we create a schema toostore our tasks and users. Pretty simple schema for now.
var TaskSchema = new Schema({
    owner: String,
    Text: String,
    completed: Boolean,
    date: Date
});

// Use hello schema tooregister a model
mongoose.model('Task', TaskSchema);
var Task = mongoose.model('Task');
```
Nejprve vytvoříte schéma hello a pak vytvoříte objekt modelu, který používáte toostore dat napříč hello kódu při definování vaše **trasy**.

## <a name="add-routes-for-your-rest-api-task-server"></a>Přidání tras pro serveru úloh REST API
Teď, když máte toowork modelu databázi s, přidejte hello trasy, který použijete pro svůj server REST API.

### <a name="about-routes-in-restify"></a>O trasách v Restify
Trasy v Restify ve fungovat hello stejné jako pracují při použití balíku Express hello. Trasy se definují pomocí identifikátoru URI, které předpokládáte hello klienta aplikace toocall hello.

Typický vzor trasy Restify je:

```Javascript
function createObject(req, res, next) {
// do work on Object
_object.name = req.params.object; // passed value is in req.params under object
///...
return next(); // keep hello server going
}
....
server.post('/service/:add/:object', createObject); // calls createObject on routes that match this.
```

Restify a Express poskytují mnohem hlubší funkčnost, jako například definování typů aplikací a provádění komplexního trasování napříč různými koncovými body. Pro účely tohoto kurzu hello jsme zjednodušení tyto trasy.

#### <a name="add-default-routes-tooyour-server"></a>Přidat výchozí trasy tooyour server
Nyní přidáte hello základní trasy CRUD **vytvořit** a **seznamu** pro naše REST API. Dalším postupům lze nalézt v hello `complete` větve hello ukázky.

Hello příkazovém řádku přejděte do adresáře příliš`azuread`, pokud ještě není:

`cd azuread`

Otevřete hello `server.js` souboru v editoru. Pod položky databáze hello provedené vyšší přidat hello následující informace:

```Javascript
/**
 *
 * APIs for our REST Task server
 */

// Create a task

function createTask(req, res, next) {

    // Resitify currently has a bug which doesn't allow you tooset default headers
    // This headers comply with CORS and allow us toomongodbServer our response tooany origin

    res.header("Access-Control-Allow-Origin", "*");
    res.header("Access-Control-Allow-Headers", "X-Requested-With");

    // Create a new task model, fill it up and save it tooMongodb
    var _task = new Task();

    if (!req.params.Text) {
        req.log.warn({
            params: req.params
        }, 'createTodo: missing task');
        next(new MissingTaskError());
        return;
    }

    _task.owner = owner;
    _task.Text = req.params.Text;
    _task.date = new Date();

    _task.save(function(err) {
        if (err) {
            req.log.warn(err, 'createTask: unable toosave');
            next(err);
        } else {
            res.send(201, _task);

        }
    });

    return next();

}
```

```Javascript
/// Simple returns hello list of TODOs that were loaded.

function listTasks(req, res, next) {
    // Resitify currently has a bug which doesn't allow you tooset default headers
    // This headers comply with CORS and allow us toomongodbServer our response tooany origin

    res.header("Access-Control-Allow-Origin", "*");
    res.header("Access-Control-Allow-Headers", "X-Requested-With");

    log.info("listTasks was called for: ", owner);

    Task.find({
        owner: owner
    }).limit(20).sort('date').exec(function(err, data) {

        if (err)
            return next(err);

        if (data.length > 0) {
            log.info(data);
        }

        if (!data.length) {
            log.warn(err, "There is no tasks in hello database. Add one!");
        }

        if (!owner) {
            log.warn(err, "You did not pass an owner when listing tasks.");
        } else {

            res.json(data);

        }
    });

    return next();
}
```


#### <a name="add-error-handling-for-hello-routes"></a>Přidání zpracování chyb pro trasy hello
Přidáte nějaké zpracování chyb, aby mohl komunikovat potíže narazíte back toohello klienta tak, že můžete porozumět.

Přidejte následující kód hello:

```Javascript
///--- Errors for communicating something interesting back toohello client
function MissingTaskError() {
restify.RestError.call(this, {
statusCode: 409,
restCode: 'MissingTask',
message: '"task" is a required parameter',
constructorOpt: MissingTaskError
});
this.name = 'MissingTaskError';
}
util.inherits(MissingTaskError, restify.RestError);
function TaskExistsError(owner) {
assert.string(owner, 'owner');
restify.RestError.call(this, {
statusCode: 409,
restCode: 'TaskExists',
message: owner + ' already exists',
constructorOpt: TaskExistsError
});
this.name = 'TaskExistsError';
}
util.inherits(TaskExistsError, restify.RestError);
function TaskNotFoundError(owner) {
assert.string(owner, 'owner');
restify.RestError.call(this, {
statusCode: 404,
restCode: 'TaskNotFound',
message: owner + ' was not found',
constructorOpt: TaskNotFoundError
});
this.name = 'TaskNotFoundError';
}
util.inherits(TaskNotFoundError, restify.RestError);
```


## <a name="create-your-server"></a>Vytvoření serveru
Nyní jste definovali databázi a přidali trasy. Hello poslední věcí, kterou jste toodo je instance serveru hello tooadd, která spravuje vaše volání.

Restify a Express poskytují široké možnosti přizpůsobení serveru REST API, ale tady používáme hello nejzákladnější nastavení.

```Javascript

/**
 * Our Server
 */


var server = restify.createServer({
    name: "Microsoft Azure Active Directroy TODO Server",
    version: "2.0.1"
});

// Ensure we don't drop data on uploads
server.pre(restify.pre.pause());

// Clean up sloppy paths like //todo//////1//
server.pre(restify.pre.sanitizePath());

// Handles annoying user agents (curl)
server.pre(restify.pre.userAgentConnection());

// Set a per request bunyan logger (with requestid filled in)
server.use(restify.requestLogger());

// Allow 5 requests/second by IP, and burst too10
server.use(restify.throttle({
    burst: 10,
    rate: 5,
    ip: true,
}));

// Use hello common stuff you probably want
server.use(restify.acceptParser(server.acceptable));
server.use(restify.dateParser());
server.use(restify.queryParser());
server.use(restify.gzipResponse());
server.use(restify.bodyParser({
    mapParams: true
})); // Allows for JSON mapping tooREST
server.use(restify.authorizationParser()); // Looks for authorization headers

// Let's start using Passport.js

server.use(passport.initialize()); // Starts passport
server.use(passport.session()); // Provides session support


```
## <a name="add-hello-routes-toohello-server-without-authentication"></a>Přidání serveru toohello hello tras (bez ověřování)
```Javascript
server.get('/api/tasks', passport.authenticate('oauth-bearer', {
    session: false
}), listTasks);
server.get('/api/tasks', passport.authenticate('oauth-bearer', {
    session: false
}), listTasks);
server.get('/api/tasks/:owner', passport.authenticate('oauth-bearer', {
    session: false
}), getTask);
server.head('/api/tasks/:owner', passport.authenticate('oauth-bearer', {
    session: false
}), getTask);
server.post('/api/tasks/:owner/:task', passport.authenticate('oauth-bearer', {
    session: false
}), createTask);
server.post('/api/tasks', passport.authenticate('oauth-bearer', {
    session: false
}), createTask);
server.del('/api/tasks/:owner/:task', passport.authenticate('oauth-bearer', {
    session: false
}), removeTask);
server.del('/api/tasks/:owner', passport.authenticate('oauth-bearer', {
    session: false
}), removeTask);
server.del('/api/tasks', passport.authenticate('oauth-bearer', {
    session: false
}), removeTask);
server.del('/api/tasks', passport.authenticate('oauth-bearer', {
    session: false
}), removeAll, function respond(req, res, next) {
    res.send(204);
    next();
});


// Register a default '/' handler

server.get('/', function root(req, res, next) {
    var routes = [
        'GET     /',
        'POST    /api/tasks/:owner/:task',
        'POST    /api/tasks (for JSON body)',
        'GET     /api/tasks',
        'PUT     /api/tasks/:owner',
        'GET     /api/tasks/:owner',
        'DELETE  /api/tasks/:owner/:task'
    ];
    res.send(200, routes);
    next();
});
```

```Javascript

server.listen(serverPort, function() {

    var consoleMessage = '\n Microsoft Azure Active Directory Tutorial';
    consoleMessage += '\n +++++++++++++++++++++++++++++++++++++++++++++++++++++';
    consoleMessage += '\n %s server is listening at %s';
    consoleMessage += '\n Open your browser too%s/api/tasks\n';
    consoleMessage += '+++++++++++++++++++++++++++++++++++++++++++++++++++++ \n';
    consoleMessage += '\n !!! why not try a $curl -isS %s | json tooget some ideas? \n';
    consoleMessage += '+++++++++++++++++++++++++++++++++++++++++++++++++++++ \n\n';

    //log.info(consoleMessage, server.name, server.url, server.url, server.url);

});

```

## <a name="add-authentication-tooyour-rest-api-server"></a>Přidat server REST API tooyour ověřování
Když teď máte fungující server REST API, můžete ho využít s Azure AD.

Hello příkazovém řádku přejděte do adresáře příliš`azuread`, pokud ještě není:

`cd azuread`

### <a name="use-hello-oidcbearerstrategy-that-is-included-with-passport-azure-ad"></a>Hello použití OIDCBearerStrategy, která je součástí passport-azure-ad
> [!TIP]
> Při psaní rozhraní API, byste vždy měli propojit hello data toosomething jedinečné z hello token, který hello uživatel nemůže zfalšovat. Pokud hello server ukládá položky úkolů, nebude proto podle hello **oid** hello uživatele v hello tokenu (zavolaném prostřednictvím token.oid), které je třeba do pole hello "vlastník". Tato hodnota zajišťuje, že pouze tento uživatel bude mít přístup ke svým vlastním položkám ToDo. Není nijak neprojevuje v hello rozhraní API "vlastník", takže externí uživatel může požadovat položkám úkolů jiných uživatelů, i když jsou ověřeni.
>
>

Dále použijte nosnou strategii hello, která se dodává s `passport-azure-ad`.

```Javascript
var findById = function(id, fn) {
    for (var i = 0, len = users.length; i < len; i++) {
        var user = users[i];
        if (user.oid === id) {
            log.info('Found user: ', user);
            return fn(null, user);
        }
    }
    return fn(null, null);
};


var oidcStrategy = new OIDCBearerStrategy(options,
    function(token, done) {
        log.info('verifying hello user');
        log.info(token, 'was hello token retreived');
        findById(token.sub, function(err, user) {
            if (err) {
                return done(err);
            }
            if (!user) {
                // "Auto-registration"
                log.info('User was added automatically as they were new. Their sub is: ', token.oid);
                users.push(token);
                owner = token.oid;
                return done(null, token);
            }
            owner = token.sub;
            return done(null, user, token);
        });
    }
);

passport.use(oidcStrategy);
```

Passport používá hello stejný vzor pro všechny svoje strategie. Předáváte jí `function()`, která jako parametry přijímá `token` a `done`. Hello strategie vrátí tooyou po dokončení veškeré práce. By pak uložte hello uživatele a uložit hello token, abyste tooask pro ni není nutné znovu.

> [!IMPORTANT]
> výše uvedený kód Hello přijímá každý uživatel, který se stane tooauthenticate tooyour serveru. Tento proces se nazývá automatická registrace. Na produkčních serverech Nenechte v jakéhokoli rozhraní API hello přístup uživatelé bez toho, aby předtím prošli registračním procesem. Tento proces je obvykle hello vzor, který můžete vidět u uživatelských aplikací, které umožňují tooregister pomocí Facebooku, ale poté vás požádají toofill Další informace. Pokud tento program nebyl příkazového řádku programu, mohli bychom extrahovat hello e-mailu z hello tokenu objektu, který je vrácen a poté požádat uživatele toofill Další informace. Protože to je ukázka, přidáme je tooan databáze v paměti.
>
>

## <a name="run-your-server-application-tooverify-that-it-rejects-you"></a>Spuštění vaší aplikace tooverify serveru to vás odmítne
Můžete použít `curl` toosee, pokud máte nyní chráněné pomocí OAuth2 koncové body. Hello hlavičky vrátil by měl být dostatek tootell jste, že jste na správné cestě hello.

Ujistěte se, že je spuštěna instance MongoDB.

    $sudo mongodb

Změňte adresář toohello a spuštění hello serveru:

    $ cd azuread
    $ node server.js

V novém okně terminálu spusťte příkaz `curl`

Zkuste základní požadavek POST:

`$ curl -isS -X POST http://127.0.0.1:3000/api/tasks/brandon/Hello`

```Shell
HTTP/1.1 401 Unauthorized
Connection: close
WWW-Authenticate: Bearer realm="Users"
Date: Tue, 14 Jul 2015 05:45:03 GMT
Transfer-Encoding: chunked
```

Chyba 401 je reakce na hello. Označuje, že vrstva Passportu hello se pokouší tooredirect toohello zajistí autorizaci koncového bodu.

## <a name="you-now-have-a-rest-api-service-that-uses-oauth2"></a>Nyní máte službu REST API, která používá OAuth2
Implementovali jste rozhraní REST API s použitím Restify a OAuth! Nyní máte dostatečný kód, aby mohli pokračovat toodevelop služby a na tomto příkladu stavět. S tímto serverem jste došli nejdál, co to jde bez použití klienta kompatibilního s OAuth2. Tento další krok použít další návod jako naše [připojit tooa webového rozhraní API pomocí iOS s B2C](active-directory-b2c-devquickstarts-ios.md) návod.

## <a name="next-steps"></a>Další kroky
Nyní se můžete přesunout toomore advanced témata, jako například:

[Připojit tooa webového rozhraní API pomocí iOS s B2C](active-directory-b2c-devquickstarts-ios.md)
