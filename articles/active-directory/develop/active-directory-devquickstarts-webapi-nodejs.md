---
title: "aaaAzure AD Node.js Začínáme | Microsoft Docs"
description: "Jak toobuild webového rozhraní API Node.js REST, umožňuje integraci se službou Azure AD pro ověřování."
services: active-directory
documentationcenter: nodejs
author: navyasric
manager: mbaldwin
editor: 
ms.assetid: 7654ab4c-4489-4ea5-aba9-d7cdc256e42a
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: javascript
ms.topic: article
ms.date: 01/07/2017
ms.author: nacanuma
ms.custom: aaddev
ms.openlocfilehash: 512ae6de9acfde8b58c0447ab4a6b573fb6407c3
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-web-apis-for-nodejs"></a>Začínáme s webových rozhraní API pro Node.js
[!INCLUDE [active-directory-devguide](../../../includes/active-directory-devguide.md)]

*Passport* je ověřovací middleware pro Node.js. Flexibilní a modulární, Passport lze snadno vyřadit v tooany využívající Express nebo Restify webové aplikace. Komplexní sada strategií podporují ověřování pomocí uživatelského jména a hesla, Facebook, Twitter a další. Vyvinuli jsme strategii pro Microsoft Azure Active Directory (Azure AD). Jsme nainstalujete tento modul a poté přidejte hello Microsoft Azure Active Directory `passport-azure-ad` modulu plug-in.

toodo, budete muset:

1. Zaregistrovat aplikaci s Azure AD.
2. Nastavení vaší aplikace toouse Passport je `passport-azure-ad` modulu plug-in.
3. Konfigurace klienta aplikace toocall hello tooDo seznamu webového rozhraní API.

Hello kód v tomto kurzu se udržuje [na Githubu](https://github.com/Azure-Samples/active-directory-node-webapi).

> [!NOTE]
> Tento článek nezahrnuje jak tooimplement přihlášení, registrace, nebo profil správy s Azure AD B2C. Zaměřuje se na volání webových rozhraní API po hello uživatel je již ověřen.  Doporučujeme spouštět s [jak toointegrate s Azure Active Directory dokumentu](active-directory-how-to-integrate.md) toolearn o hello Základy služby Azure Active Directory.
>
>

Vydala společnost Microsoft všechny hello zdrojového kódu v tomto příkladu spuštěné v Githubu pod licencí MIT, takže působí volné tooclone (nebo i lépe rozvětvení) a poskytnout zpětnou vazbu a požadavky pro vyžádání obsahu.

## <a name="about-nodejs-modules"></a>O modulů Node.js
V tomto návodu použijeme modulů Node.js. Moduly jsou načíst JavaScript balíčky, které poskytují funkce specifická pro vaši aplikaci. Obvykle nainstalujete moduly pomocí hello Node.js nástroj příkazového řádku na NPM v hello NPM instalační adresář. Některé moduly, jako je například modul HTTP hello, ale jsou součástí balíčku Node.js hello jádra.

Nainstalované moduly jsou uloženy ve hello **node_modules** adresář hello kořenové adresáře instalace Node.js. Každý modul v hello **node_modules** directory udržuje vlastní **node_modules** adresář, který obsahuje všechny moduly, které závisí na. Navíc má každý požadované modulu **node_modules** adresáře. Tato struktura adresáře rekurzivní představuje řetězec závislostí hello.

Tato struktura řetězu závislostí za následek větší nároků aplikace. Můžete ale také zaručuje, že jsou splněné všechny závislosti a danou verzi hello hello modulů, který se používá v vývoj se také používá v produkčním prostředí. To usnadňuje předvídatelnější chování aplikace hello produkční a zabrání problémům s verzemi, které mohou ovlivnit uživatele.

## <a name="step-1-register-an-azure-ad-tenant"></a>Krok 1: Registrace klienta Azure AD
toouse to ukázkové, musíte klienta služby Azure Active Directory. Pokud si nejste jistí je co klienta nebo jak zjistit, tooget, [jak tooget na Azure AD klienta](active-directory-howto-tenant.md).

## <a name="step-2-create-an-application"></a>Krok 2: Vytvoření aplikace
Dále vytvoříte aplikaci v adresáři, že poskytuje Azure AD informace, že tato služba vyžaduje toosecurely komunikaci s vaší aplikací.  Hello klientská aplikace i webové rozhraní API jsou reprezentované pomocí jedné **ID aplikace** v tomto případě protože společně tvoří jednu logickou aplikaci.  toocreate na aplikace, postupujte podle [tyto pokyny](active-directory-how-applications-are-added.md). Pokud vytváříte-obchodní aplikace, [mohou být užitečné tyto další pokyny](../active-directory-applications-guiding-developers-for-lob-applications.md).

toocreate aplikace:

1. Přihlaste se toohello [portál Azure](https://portal.azure.com).

2. V horní nabídce hello vyberte svůj účet. Potom v části hello **Directory** vyberte místo, kam chcete tooregister klienta služby Active Directory hello vaší aplikace.

3. V nabídce hello na levé straně hello vyberte **více služeb**a potom vyberte **Azure Active Directory**.

4. Vyberte **registrace aplikace**a potom vyberte **přidat**.

5. Postupujte podle pokynů toocreate hello **webové aplikace nebo WebAPI**.

      * Hello **název** z hello aplikace popisuje tooend uživatelů vaší aplikace.

      * Hello **přihlašovací adresa URL** je hello základní adresu URL aplikace.  Výchozí adresa URL aplikace hello ukázkový kód je Hello `https://localhost:8080`.

6. Po registraci, Azure AD přiřadí aplikace jedinečné ID aplikace Je třeba tuto hodnotu v dalších částech hello, takže zkopírujte jej ze stránky aplikace hello.

7. Z hello **nastavení** -> **vlastnosti** stránky pro aplikace, aktualizujte hello identifikátor ID URI aplikace. Hello **identifikátor ID URI aplikace** je jedinečný identifikátor pro vaši aplikaci. konvence Hello je toouse `https://<tenant-domain>/<app-name>`, například: `https://contoso.onmicrosoft.com/my-first-aad-app`.

8. Vytvoření **klíč** pro vaši aplikaci z hello **nastavení** stránky a pak ji někam zkopírujte. Budete je potřebovat za chvíli.

## <a name="step-3-download-nodejs-for-your-platform"></a>Krok 3: Stažení Node.js pro vaši platformu
toosuccessfully tuto ukázku použít, musíte mít funkční instalací Node.js.

Nainstalujte si Node.js z [http://nodejs.org](http://nodejs.org).

## <a name="step-4-install-mongodb-on-your-platform"></a>Krok 4: Instalace MongoDB na vaši platformu
toosuccessfully tuto ukázku použít, musíte mít funkční instalací MongoDB. Používáte MongoDB toomake hello trvalé REST API napříč instancemi serveru.

Nainstalujte MongoDB z [http://mongodb.org](http://www.mongodb.org).

> [!NOTE]
> Tento návod předpokládá, že používáte hello výchozí instalaci a koncové body serveru pro MongoDB, což v době psaní tohoto textu hello je mongodb://localhost.
>
>

## <a name="step-5-install-hello-restify-modules-in-your-web-api"></a>Krok 5: Instalace hello modulů Restify ve webovém rozhraní API
Restify toobuild používáme našem REST API. Restify je minimalistické a flexibilní Node.js aplikace rozhraní, které je odvozené z Express. Obsahuje robustní sadu funkcí pro sestavování rozhraní REST API postavených na protokolu Connect.

### <a name="install-restify"></a>Instalace Restify
1. Z příkazového řádku hello, změňte adresáře toohello **azuread** adresáře. Pokud hello **azuread** adresář neexistuje, vytvořte ho.

        `cd azuread - or- mkdir azuread; cd azuread`

2. Zadejte hello následující příkaz:

    `npm install restify`

    Tento příkaz nainstaluje Restify.

#### <a name="did-you-get-an-error"></a>Obdrželi jste chybu?
Použijete-li NPM u některých operačních systémů, můžete obdržet chybu, která uvádí, že **Chyba: EPERM chmod '/ usr/místní/bin /..'** a návrhu, zkuste to spuštěné hello účet jako správce. Pokud k tomu dojde, použijte hello sudo příkaz toorun NPM na vyšší úrovni oprávnění.

#### <a name="did-you-get-an-error-regarding-dtrace"></a>Obdrželi jste chybu týkající se DTRACE?
Při instalaci Restify může zobrazit chyba takto:

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
Restify poskytuje výkonný mechanismus pro trasování volání REST pomocí DTrace. Řada operačních systémů, ale nemají DTrace. Tyto chyby můžete bezpečně ignorovat.

Hello výstup tohoto příkazu by měl vypadat podobně jako toohello následující výstup:

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


## <a name="step-6-install-passportjs-in-your-web-api"></a>Krok 6: Instalace Passport.js ve vašem webovém rozhraní API
[Passport](http://passportjs.org/) je ověřovací middleware pro Node.js. Flexibilní a modulární, Passport lze snadno vyřadit v tooany využívající Express nebo Restify webové aplikace. Komplexní sada strategií podporují ověřování pomocí uživatelského jména a hesla, Facebook, Twitter a další.

Vyvinuli jsme strategii pro Azure Active Directory. Jsme nainstalujete tento modul a poté přidejte hello plug-in strategie Azure Active Directory.

1. Z příkazového řádku hello, změňte adresáře toohello **azuread** adresáře.

2. tooinstall passport.js, zadejte následující příkaz hello:

    `npm install passport`

    výstup Hello hello příkazu by měl vypadat podobně jako toohello následující:

``
        passport@0.1.17 node_modules\passport
        ├── pause@0.0.1
        └── pkginfo@0.2.3
``

## <a name="step-7-add-passport-azure-ad-tooyour-web-api"></a>Krok 7: Přidejte Passport-Azure-AD tooyour webového rozhraní API
Další přidáme hello strategii OAuth pomocí `passport-azure-ad`, sady strategií, které se připojují tooPassport Azure Active Directory. Používáme tuto strategii pro nosné tokeny v této ukázce REST API.

> [!NOTE]
> Přestože OAuth2 poskytuje rozhraní, ve kterém můžou být vystavené všechny známé typy tokenů, běžně se používají pouze určitým typům tokenů. Tokeny hello nejčastěji používaná pro ochranu koncových bodů jsou nosné tokeny. Jsou nejčastěji vydané hello typem tokenů v OAuth2. Mnoho implementací předpokládá, že jsou nosné tokeny jediným typem hello tokeny, které jsou vystavené.
>
>

Z příkazového řádku hello, změňte adresáře toohello **azuread** adresáře.

Typ hello následující příkaz tooinstall hello Passport.js `passport-azure-ad module`:

`npm install passport-azure-ad`

výstup Hello hello příkazu by měl vypadat podobně jako toohello následující výstup:


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



## <a name="step-8-add-mongodb-modules-tooyour-web-api"></a>Krok 8: Přidejte MongoDB moduly tooyour webového rozhraní API
MongoDB používáme jako naše úložiště. Z tohoto důvodu potřebujeme tooinstall hello nejběžněji používané modulu plug-in volané Mongoose toomanage modelů a schémat. Také potřebujeme tooinstall hello databáze ovladačů pro MongoDB (což je také nazývaný MongoDB).

 `npm install mongoose`

## <a name="step-9-install-additional-modules"></a>Krok 9: Instalace dalších modulů
Další nainstalujeme hello zbývající požadované moduly.

1. Z příkazového řádku hello, změňte adresáře toohello **azuread** složky, pokud si nejste již existuje.

    `cd azuread`

2. Zadejte následující příkazy tooinstall hello tyto moduly v vaše **node_modules** directory:

    * `npm install assert-plus`
    * `npm install bunyan`
    * `npm update`

## <a name="step-10-create-a-serverjs-with-your-dependencies"></a>Krok 10: Vytvoření server.js se závislostmi
souboru server.js Hello poskytuje většinu hello funkce pro naše webového rozhraní API serveru. Přidáme většina našich toothis souboru kódu. Pro produkční účely doporučujeme, aby zrefaktorujete hello funkce do menších souborů, jako je například samostatné trasy a ovladače. V této ukázce používáme server.js pro tuto funkci.

1. Z příkazového řádku hello, změňte adresáře toohello **azuread** složky, pokud si nejste již existuje.

    `cd azuread`

2. Vytvoření `server.js` souboru ve svém oblíbeném editoru a poté přidejte hello následující informace:

    ```Javascript
        'use strict';

        /**
         * Module dependencies.
         */

        var fs = require('fs');
        var path = require('path');
        var util = require('util');
        var assert = require('assert-plus');
        var bunyan = require('bunyan');
        var getopt = require('posix-getopt');
        var mongoose = require('mongoose/');
        var restify = require('restify');
        var passport = require('passport');
      var BearerStrategy = require('passport-azure-ad').BearerStrategy;
    ```

3. Uložte soubor hello. Vrátíme tooit za chvíli.

## <a name="step-11-create-a-config-file-toostore-your-azure-ad-settings"></a>Krok 11: Vytvoření souboru toostore konfigurace nastavení Azure AD
Tento soubor s kódem předává parametry konfigurace hello z portálu tooPassport.js vaší služby Azure Active Directory. Tyto hodnoty konfigurace jste vytvořili, když jste přidali hello webové rozhraní API toohello portálu v první části hello hello návodu. Po zkopírování hello kód objasníme, jaké tooput v hello hodnoty těchto parametrů.

1. Z příkazového řádku hello, změňte adresáře toohello **azuread** složky, pokud si nejste již existuje.

    `cd azuread`

2. Vytvoření `config.js` souboru ve svém oblíbeném editoru a poté přidejte hello následující informace:

    ```Javascript
         exports.creds = {
             mongoose_auth_local: 'mongodb://localhost/tasklist', // Your mongo auth uri goes here
             clientID: 'your client ID',
             audience: 'your application URL',
            // you cannot have users from multiple tenants sign in tooyour server unless you use hello common endpoint
          // example: https://login.microsoftonline.com/common/.well-known/openid-configuration
             identityMetadata: 'https://login.microsoftonline.com/<your tenant id>/.well-known/openid-configuration',
             validateIssuer: true, // if you have validation on, you cannot have users from multiple tenants sign in tooyour server
             passReqToCallback: false,
             loggingLevel: 'info' // valid are 'info', 'warn', 'error'. Error always goes toostderr in Unix.

         };
    ```
3. Uložte soubor hello.

## <a name="step-12-add-configuration-values-tooyour-serverjs-file"></a>Krok 12: Přidání souboru server.js tooyour hodnoty konfigurace
Tooread potřebujeme v naší aplikaci tyto hodnoty ze souboru .config hello, kterou jste vytvořili. toodo tohoto souboru .config hello přidáme jako požadovaný prostředek v naší aplikaci. Potom nastaví hello globální proměnné toomatch hello proměnné v dokumentu config.js hello.

1. Z příkazového řádku hello, změňte adresáře toohello **azuread** složky, pokud si nejste již existuje.

    `cd azuread`

2. Otevřete váš `server.js` souboru ve svém oblíbeném editoru a poté přidejte hello následující informace:

    ```Javascript
    var config = require('./config');
    ```
3. Pak přidejte nový oddíl příliš`server.js` s hello následující kód:

    ```Javascript
    var options = {
        // hello URL of hello metadata document for your app. We will put hello keys for token validation from hello URL found in hello jwks_uri tag of hello in hello metadata.
        identityMetadata: config.creds.identityMetadata,
        clientID: config.creds.clientID,
        validateIssuer: config.creds.validateIssuer,
        audience: config.creds.audience,
        passReqToCallback: config.creds.passReqToCallback,
        loggingLevel: config.creds.loggingLevel

    };

    // Array toohold logged in users and hello current logged in user (owner).
    var users = [];
    var owner = null;

    // Our logger.
    var log = bunyan.createLogger({
        name: 'Azure Active Directory Bearer Sample',
             streams: [
            {
                stream: process.stderr,
                level: "error",
                name: "error"
            },
            {
                stream: process.stdout,
                level: "warn",
                name: "console"
            }, ]
    });

      // If hello logging level is specified, switch tooit.
      if (config.creds.loggingLevel) { log.levels("console", config.creds.loggingLevel); }

    // MongoDB setup.
    // Set up some configuration.
    var serverPort = process.env.PORT || 8080;
    var serverURI = (process.env.PORT) ? config.creds.mongoose_auth_mongohq : config.creds.mongoose_auth_local;
    ```

4. Uložte soubor hello.

## <a name="step-13-add-hello-mongodb-model-and-schema-information-by-using-mongoose"></a>Krok 13: Přidejte hello MongoDB modelu a schématu informace pomocí Mongoose
Nyní bude tato Příprava toostart platícího, protože jsme sloučit tyto tři soubory ve službu REST API.

V tomto návodu použijeme MongoDB toostore naše úlohy popsané v kroku 4.

V hello `config.js` souboru, že jsme vytvořili v kroku 11, volali jsme naše databáze `tasklist`, protože, který byl co jsme uveďte na konci hello naše **mogoose_auth_local** adresy URL pro připojení. Nepotřebujete toocreate tato databáze předem v MongoDB. Místo toho MongoDB vytvoří to pro nás na hello nejprve spusťte naše aplikace server (za předpokladu, že ještě neexistuje hello databázi).

Teď, když jsme jsme vás vyzval hello server kterou databázi MongoDB rádi bychom znali toouse, potřebujeme toowrite některé další kód toocreate hello modelu a schématu pro úlohy naše serveru.

### <a name="discussion-of-hello-model"></a>Informace o modelu hello
Naše model schématu je jednoduché. Rozbalte podle potřeby.

Název: název hello hello osobě, která je přiřazena toohello úloh. A **řetězec**.

ÚLOHA: hello vlastní úloha. A **řetězec**.

Datum hello datum: tuto úlohu hello je kvůli. A **DATA A ČASU**.

DOKONČENO: Pokud má úloha hello hotové nebo ne. A **BOOLEAN**.

### <a name="creating-hello-schema-in-hello-code"></a>Vytváření hello schématu v kódu hello
1. Z příkazového řádku hello, změňte adresáře toohello **azuread** složky, pokud si nejste již existuje.

    `cd azuread`

2. Otevřete váš `server.js` souboru ve svém oblíbeném editoru a poté přidejte následující informace pod položku konfigurace hello hello:

    ```Javascript
    // Connect tooMongoDB.
    global.db = mongoose.connect(serverURI);
    var Schema = mongoose.Schema;
    log.info('MongoDB Schema loaded');

    // Here we create a schema toostore our tasks and users. It's a fairly simple schema for now.
    var TaskSchema = new Schema({
        owner: String,
        task: String,
        completed: Boolean,
        date: Date
    });

    // Use hello schema tooregister a model.
    mongoose.model('Task', TaskSchema);
    var Task = mongoose.model('Task');
    ```
Jak se dá zjistit z hello kódu, jsme naše schématu nejprve vytvořit. Poté vytvoříme objekt modelu, který používáme toostore našich dat v rámci hello kódu, když jsme definovali naše **trasy**.

## <a name="step-14-add-our-routes-for-our-task-rest-api-server"></a>Krok 14: Přidejte naše trasy pro naše serveru úloh REST API
Teď, když máme toowork modelu databázi s, Pojďme přidat trasy hello Snažíme se má použít pro naše server REST API.

### <a name="about-routes-in-restify"></a>O trasách v Restify
Pracovní trasy v Restify hello stejný způsob, jak se v hello Express zásobníku. Trasy se definují pomocí identifikátoru URI, které předpokládáte hello klienta aplikace toocall hello. Trasy se obvykle definovat v samostatném souboru. Pro naše účely jsme do souboru server.js hello umístit naše trasy. Doporučujeme, abyste je zvážit tyto trasy do své vlastní souboru pro použití v provozním prostředí.

Typický vzor trasy Restify je následující:

```Javascript
function createObject(req, res, next) {

// Do work on object.

 _object.name = req.params.object; // passed value is in req.params under object

 ///...

return next(); // Keep hello server going.
}

....

server.post('/service/:add/:object', createObject); // Calls createObject on routes that match this.

```


Toto je vzor hello na nejzákladnější úrovni. Restify (a Express) poskytují mnohem hlubší funkčnost, jako je například definování typů aplikací a zajištění komplexního trasování napříč různými koncovými body. Pro naše účely jsme jsou zachování tyto trasy jednoduché.

### <a name="add-default-routes-tooour-server"></a>Přidat výchozí trasy tooour server
Jsme teď přidejte hello základní trasy CRUD vytvořit, načtení, aktualizace a odstranění.

1. Z příkazového řádku hello, změňte adresáře toohello **azuread** složky, pokud si nejste již existuje:

    `cd azuread`

2. Otevřete hello `server.js` souboru ve svém oblíbeném editoru a poté přidejte následující informace níže hello databáze položky, které jste provedli hello:

```Javascript

/**
 *
 * APIs for our REST Task server.
 */

// Create a task.

function createTask(req, res, next) {

    // Restify currently has a bug which doesn't allow you tooset default headers.
    // These headers comply with CORS and allow us toomongodbServer our response tooany origin.

    res.header("Access-Control-Allow-Origin", "*");
    res.header("Access-Control-Allow-Headers", "X-Requested-With");

    // Create a new task model, fill it, and save it tooMongodb.
    var _task = new Task();

    if (!req.params.task) {
        req.log.warn('createTodo: missing task');
        next(new MissingTaskError());
        return;
    }

    _task.owner = owner;
    _task.task = req.params.task;
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


// Delete a task by name.

function removeTask(req, res, next) {

    Task.remove({
        task: req.params.task,
        owner: owner
    }, function(err) {
        if (err) {
            req.log.warn(err,
                'removeTask: unable toodelete %s',
                req.params.task);
            next(err);
        } else {
            log.info('Deleted task:', req.params.task);
            res.send(204);
            next();
        }
    });
}

// Delete all tasks.

function removeAll(req, res, next) {
    Task.remove();
    res.send(204);
    return next();
}


// Get a specific task based on name.

function getTask(req, res, next) {

    log.info('getTask was called for: ', owner);
    Task.find({
        owner: owner
    }, function(err, data) {
        if (err) {
            req.log.warn(err, 'get: unable tooread %s', owner);
            next(err);
            return;
        }

        res.json(data);
    });

    return next();
}

/// Simple returns hello list of TODOs that were loaded.

function listTasks(req, res, next) {
    // Restify currently has a bug which doesn't allow you tooset default headers.
    // These headers comply with CORS and allow us toomongodbServer our response tooany origin.

    res.header("Access-Control-Allow-Origin", "*");
    res.header("Access-Control-Allow-Headers", "X-Requested-With");

    log.info("listTasks was called for: ", owner);

    Task.find({
        owner: owner
    }).limit(20).sort('date').exec(function(err, data) {

        if (err) {
            return next(err);
        }

        if (data.length > 0) {
            log.info(data);
        }

        if (!data.length) {
            log.warn(err, "There is no tasks in hello database. Did you initialize hello database as stated in hello README?");
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

### <a name="add-error-handling-in-our-apis"></a>Přidání zpracování chyb v našem rozhraní API
```

///--- Errors for communicating something interesting back toohello client.

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


## <a name="step-15-create-your-server"></a>Krok 15: Vytvoření serveru
Jsme definovali naše databáze a naše trasy jsou na místě. poslední věcí toodo Hello je přidat hello instanci serveru, která spravuje naše volání.

V Restify (a Express) lze provádět mnoho přizpůsobení serveru REST API, ale znova budeme toouse hello nejzákladnější nastavení pro naše účely.

```Javascript
/**
 * Our server.
 */


var server = restify.createServer({
    name: "Azure Active Directroy TODO Server",
    version: "2.0.1"
});

// Ensure we don't drop data on uploads.
server.pre(restify.pre.pause());

// Clean up sloppy paths like //todo//////1//.
server.pre(restify.pre.sanitizePath());

// Handle annoying user agents (curl).
server.pre(restify.pre.userAgentConnection());

// Set a per request bunyan logger (with requestid filled in).
server.use(restify.requestLogger());

// Allow five requests per second by IP, and burst too10.
server.use(restify.throttle({
    burst: 10,
    rate: 5,
    ip: true,
}));

// Use hello common stuff you probably want.
server.use(restify.acceptParser(server.acceptable));
server.use(restify.dateParser());
server.use(restify.queryParser());
server.use(restify.gzipResponse());
server.use(restify.bodyParser({
    mapParams: true
})); // Allow for JSON mapping tooREST.
```

## <a name="step-16-add-hello-routes-toohello-server-without-authentication-for-now"></a>Krok 16: Přidejte server toohello hello tras (bez ověřování prozatím)
```Javascript
/// Now hello real handlers. Here we just CRUD.
/**
/*
/* Each of these handlers is protected by our OIDCBearerStrategy by invoking 'oidc-bearer'.
/* In hello pasport.authenticate() method. We set 'session: false' because REST is stateless and
/* we don't need toomaintain session state. You can experiment with removing API protection
/* by removing hello passport.authenticate() method as follows:
/*
/* server.get('/tasks', listTasks);
/*
**/
server.get('/tasks', listTasks);
server.get('/tasks', listTasks);
server.get('/tasks/:owner', getTask);
server.head('/tasks/:owner', getTask);
server.post('/tasks/:owner/:task', createTask);
server.post('/tasks', createTask);
server.del('/tasks/:owner/:task', removeTask);
server.del('/tasks/:owner', removeTask);
server.del('/tasks', removeTask);
server.del('/tasks', removeAll, function respond(req, res, next) {
res.send(204);
next();
});
// Register a default '/' handler.
server.get('/', function root(req, res, next) {
var routes = [
'GET /',
'POST /tasks/:owner/:task',
'POST /tasks (for JSON body)',
'GET /tasks',
'PUT /tasks/:owner',
'GET /tasks/:owner',
'DELETE /tasks/:owner/:task'
];
res.send(200, routes);
next();
});
server.listen(serverPort, function() {
var consoleMessage = '\n Microsoft Azure Active Directory Tutorial';
consoleMessage += '\n +++++++++++++++++++++++++++++++++++++++++++++++++++++';
consoleMessage += '\n %s server is listening at %s';
consoleMessage += '\n Open your browser too%s/tasks\n';
consoleMessage += '+++++++++++++++++++++++++++++++++++++++++++++++++++++ \n';
consoleMessage += '\n !!! why not try a $curl -isS %s | json tooget some ideas? \n';
consoleMessage += '+++++++++++++++++++++++++++++++++++++++++++++++++++++ \n\n';
});
```

## <a name="step-17-run-hello-server-before-adding-oauth-support"></a>Krok 17: Spusťte hello server (před přidáním podpory OAuth)
Otestovat váš server před přidáme ověřování.

Nejjednodušší způsob, jak tootest Hello serveru je pomocí curl v příkazovém řádku. Než to, potřebujeme nástroj, který umožňuje nám tooparse výstup jako JSON.

1. Nainstalujte hello následující nástroj JSON (Tento nástroj použijte všechny hello následující příklady):

    `$npm install -g jsontool`

    To globálně nainstaluje nástroj JSON hello. Teď, když jsme který udělat, budeme přehrání hello serveru:

2. Nejprve se ujistěte, že je spuštěna mongoDB instance:

    `$sudo mongod`

3. Potom změňte adresář toohello a spustit kulmy:

    `$ cd azuread` `$ node server.js`

    `$ curl -isS http://127.0.0.1:8080 | json`

    ```Shell
    HTTP/1.1 200 OK
    Connection: close
    Content-Type: application/json
    Content-Length: 171
    Date: Tue, 14 Jul 2015 05:43:38 GMT
    [
    "GET /",
    "POST /tasks/:owner/:task",
    "POST /tasks (for JSON body)",
    "GET /tasks",
    "PUT /tasks/:owner",
    "GET /tasks/:owner",
    "DELETE /tasks/:owner/:task"
    ]
    ```

4. Potom jsme můžete přidat úloha tímto způsobem:

    `$ curl -isS -X POST http://127.0.0.1:8080/tasks/brandon/Hello`

    Hello odpověď by měla být:

        ```Shell
        HTTP/1.1 201 Created
        Connection: close
        Access-Control-Allow-Origin: *
        Access-Control-Allow-Headers: X-Requested-With
        Content-Type: application/x-www-form-urlencoded
        Content-Length: 5
        Date: Tue, 04 Feb 2014 01:02:26 GMT
        Hello
        ```
    A zde jsou uvedeny úlohy pro Brandon tímto způsobem:

        `$ curl -isS http://127.0.0.1:8080/tasks/brandon/`

Pokud to vše funguje, nám server REST API toohello připraven tooadd OAuth.

Máte server REST API s MongoDB!

## <a name="step-18-add-authentication-tooour-rest-api-server"></a>Krok 18: Přidejte server REST API tooour ověřování
Teď, když máme spuštěné rozhraní REST API, Začněme jeho užitečnost s Azure AD.

Z příkazového řádku hello, změňte adresáře toohello **azuread** složky, pokud si nejste již existuje.

`cd azuread`

### <a name="use-hello-oidcbearerstrategy-that-is-included-with-passport-azure-ad"></a>Hello použití OIDCBearerStrategy, která je součástí passport-azure-ad
Pokud jsme jste vytvořili typický server REST se seznamem úkolů bez jakéhokoli druhu ověřování. Toto je, kde začneme, které připravuje umístění.

1. Nejdřív potřebujeme tooindicate, že má být toouse Passport. Toto právo PUT po ostatní konfigurace serveru:

    ```Javascript
            // Let's start using Passport.js.

            server.use(passport.initialize()); // Starts passport.
            server.use(passport.session()); // Provides session support.
    ```
    > [!TIP]
    > Při psaní rozhraní API, doporučujeme vždy odkaz hello data toosomething jedinečné z hello token, který hello uživatel nemůže zfalšovat. Pokud tento server ukládá položky úkolů, uloží je na základě ID objektu hello hello uživatele v hello tokenu (zavolaném prostřednictvím token.oid), který jsme umístit do pole "vlastník" hello. To zajišťuje, aby pouze tento uživatel může přístup k jejich TODOs. Není nijak neprojevuje v hello rozhraní API "vlastník", takže externí uživatel může požádat o hello TODOs jiných, i když jsou ověřeni.                    

2. Další použijeme hello nosnou strategii, která se dodává s `passport-azure-ad`. Podívejte se na kód hello prozatím a vysvětlíme hello rest za chvíli. To uvedli po vložení výše:

```Javascript
    /**
    /*
    /* Calling hello OIDCBearerStrategy and managing users.
    /*
    /* Passport pattern provides hello need toomanage users and info tokens
    /* with a FindorCreate() method that must be provided by hello implementor.
    /* Here we just auto-register any user and implement a FindById().
    /* You'll want toodo something smarter.
    **/

    var findById = function(id, fn) {
        for (var i = 0, len = users.length; i < len; i++) {
            var user = users[i];
            if (user.sub === id) {
                log.info('Found user: ', user);
                return fn(null, user);
            }
        }
        return fn(null, null);
    };


    var bearerStrategy = new BearerStrategy(options,
        function(token, done) {
            log.info('verifying hello user');
            log.info(token, 'was hello token retreived');
            findById(token.sub, function(err, user) {
                if (err) {
                    return done(err);
                }
                if (!user) {
                    // "Auto-registration"
                    log.info('User was added automatically as they were new. Their sub is: ', token.sub);
                    users.push(token);
                    owner = token.sub;
                    return done(null, token);
                }
                owner = token.sub;
                return done(null, user, token);
            });
        }
    );

    passport.use(bearerStrategy);
```

Passport používá podobný Princip pro všechny svoje strategie (Twitteru, Facebooku a tak dále), které řídí všichni autoři strategií k. Prohlížení hello strategie, uvidíte, že jsme předat funkci, která má token a done jako parametry hello. po jeho činnosti provede se dodává zpět toous strategie Hello. Po Ano, uložíme hello uživatele a dočasné ukládání hello token tak nebude potřebujeme tooask pro něj znovu.

> [!IMPORTANT]
> Předchozí kód Hello trvá každý uživatel, který se stane tooauthenticate tooour serveru. To se označuje jako Automatická registrace. Na produkčních serverech, které doporučujeme si nechat každý, kdo aniž by bylo nejdříve je přejdete prostřednictvím procesu registrace, který se rozhodnete. Je to obvykle hello vzor, který můžete vidět u uživatelských aplikací, které umožňují tooregister službou Facebook, ale poté vás požádají toofill Další informace. Pokud to nebyli příkazového řádku programu, mohli bychom extrahovat hello e-mailu z hello tokenu objektu, který je vrácen a poté požádat uživatele toofill hello Další informace. Protože se jedná o testovací server, jednoduše přidáme je toohello databáze v paměti.
>
>

### <a name="protect-some-endpoints"></a>Ochrana některé koncových bodů
Ochrana koncových bodů tak, že zadáte hello `passport.authenticate()` volání s hello protokolu, které chcete toouse.

toomake naše kódu serveru udělat něco další zajímavé, Pojďme upravit hello směrování.

```Javascript
server.get('/tasks', passport.authenticate('oauth-bearer', {
session: false
}), listTasks);
server.get('/tasks', passport.authenticate('oauth-bearer', {
session: false
}), listTasks);
server.get('/tasks/:owner', passport.authenticate('oauth-bearer', {
session: false
}), getTask);
server.head('/tasks/:owner', passport.authenticate('oauth-bearer', {
session: false
}), getTask);
server.post('/tasks/:owner/:task', passport.authenticate('oauth-bearer', {
session: false
}), createTask);
server.post('/tasks', passport.authenticate('oauth-bearer', {
session: false
}), createTask);
server.del('/tasks/:owner/:task', passport.authenticate('oauth-bearer', {
session: false
}), removeTask);
server.del('/tasks/:owner', passport.authenticate('oauth-bearer', {
session: false
}), removeTask);
server.del('/tasks', passport.authenticate('oauth-bearer', {
session: false
}), removeTask);
server.del('/tasks', passport.authenticate('oauth-bearer', {
session: false
}), removeAll, function respond(req, res, next) {
res.send(204);
next();
});
```

## <a name="step-19-run-your-server-application-again-and-ensure-it-rejects-you"></a>Krok 19: Spusťte aplikaci server znovu a ujistěte se, že vás odmítne
Použijeme `curl` znovu toosee, pokud bychom nyní měli chráněné pomocí OAuth2 proti naše koncové body. Provedeme tento test před s některým z klientskou sadu SDK proti tomuto koncovému bodu. Hello vrácené hlavičky by měl být dostatek tootell nám Pokud vytvoříme dolů hello správné cestě.

1. Nejprve se ujistěte, že je spuštěna mongoDB instance:

    `$sudo mongod`

2. Potom změnit adresář, toohello a spusťte kulmy.

      `$ cd azuread` `$ node server.js`

3. Zkuste základní požadavek POST.

    `$ curl -isS -X POST http://127.0.0.1:8080/tasks/brandon/Hello`

    ```Shell
    HTTP/1.1 401 Unauthorized
    Connection: close
    WWW-Authenticate: Bearer realm="Users"
    Date: Tue, 14 Jul 2015 05:45:03 GMT
    Transfer-Encoding: chunked
    ```

401 je odpověď hello, kterou hledáte sem. Tato odezva označuje, že vrstva Passportu hello se pokouší tooredirect toohello autorizovaný koncový bod, který je právě co chcete použít.

## <a name="next-steps"></a>Další kroky
Jste došli nejdál, co můžete s tímto serverem bez použití klientem kompatibilní OAuth2. Budete potřebovat toogo prostřednictvím další návod.

Naučili jste se teď jak tooimplement rozhraní REST API pomocí Restify a OAuth2. Máte víc než dost kód tookeep vývoj služby a učení jak toobuild v tomto příkladu.

Pokud vás zajímají další kroky hello ve vaší ADAL cesty, tady jsou některé podporované klienty ADAL, doporučujeme, můžete pokračovat v práci s.

Klonování dolů tooyour vývojáře počítače a nakonfigurovat, jak je popsáno v Průvodci hello.

[ADAL pro iOS](https://github.com/MSOpenTech/azure-activedirectory-library-for-ios)

[ADAL pro Android](https://github.com/MSOpenTech/azure-activedirectory-library-for-android)

[!INCLUDE [active-directory-devquickstarts-additional-resources](../../../includes/active-directory-devquickstarts-additional-resources.md)]
