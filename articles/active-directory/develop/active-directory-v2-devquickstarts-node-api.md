---
title: "aaaSecure Azure Active Directory v2.0 webového rozhraní API pomocí Node.js | Microsoft Docs"
description: "Zjistěte, jak toobuild Node.js webové rozhraní API, které přijímá tokeny z osobního účtu Microsoft a z pracovní nebo školní účty."
services: active-directory
documentationcenter: nodejs
author: navyasric
manager: mbaldwin
editor: 
ms.assetid: 0b572fc1-2aaf-4cb6-82de-63010fb1941d
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: javascript
ms.topic: article
ms.date: 05/13/2017
ms.author: nacanuma
ms.custom: aaddev
ms.openlocfilehash: 219e324cca11e107186b7e5f995589b9260af8a7
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="secure-a-web-api-by-using-nodejs"></a><span data-ttu-id="cdc91-103">Zabezpečení webového rozhraní API pomocí Node.js</span><span class="sxs-lookup"><span data-stu-id="cdc91-103">Secure a web API by using Node.js</span></span>
> [!NOTE]
> <span data-ttu-id="cdc91-104">Ne všechny funkce a scénáře Azure Active Directory fungovat s koncovým bodem v2.0 hello.</span><span class="sxs-lookup"><span data-stu-id="cdc91-104">Not all Azure Active Directory scenarios and features work with hello v2.0 endpoint.</span></span> <span data-ttu-id="cdc91-105">toodetermine zda byste měli používat koncového bodu v2.0 hello nebo koncový bod hello verze 1.0, přečtěte si informace o [v2.0 omezení](active-directory-v2-limitations.md).</span><span class="sxs-lookup"><span data-stu-id="cdc91-105">toodetermine whether you should use hello v2.0 endpoint or hello v1.0 endpoint, read about [v2.0 limitations](active-directory-v2-limitations.md).</span></span>
> 
> 

<span data-ttu-id="cdc91-106">Pokud používáte koncového bodu v2.0 hello Azure Active Directory (Azure AD), můžete použít [OAuth 2.0](active-directory-v2-protocols.md) tokeny přístupu tooprotect webového rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="cdc91-106">When you use hello Azure Active Directory (Azure AD) v2.0 endpoint, you can use [OAuth 2.0](active-directory-v2-protocols.md) access tokens tooprotect your web API.</span></span> <span data-ttu-id="cdc91-107">S OAuth 2.0 tokeny přístupu, uživatelé, kteří mají osobní účet Microsoft i pracovní nebo školní účty mít bezpečný přístup k vašemu webovému rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="cdc91-107">With OAuth 2.0 access tokens, users who have both a personal Microsoft account and work or school accounts can securely access your web API.</span></span>

<span data-ttu-id="cdc91-108">*Passport* je ověřovací middleware pro Node.js.</span><span class="sxs-lookup"><span data-stu-id="cdc91-108">*Passport* is authentication middleware for Node.js.</span></span> <span data-ttu-id="cdc91-109">Flexibilní a modulární, můžete do jakéhokoli nenápadně vyřadit Passport využívající Express nebo restify webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="cdc91-109">Flexible and modular, Passport can be unobtrusively dropped into any Express-based or restify web application.</span></span> <span data-ttu-id="cdc91-110">Komplexní sada strategií Passport, podporují ověřování pomocí uživatelského jména a hesla, Facebook, Twitter a další možnosti.</span><span class="sxs-lookup"><span data-stu-id="cdc91-110">In Passport, a comprehensive set of strategies support authentication by using a username and password, Facebook, Twitter, or other options.</span></span> <span data-ttu-id="cdc91-111">Vyvinuli jsme strategii pro Azure AD.</span><span class="sxs-lookup"><span data-stu-id="cdc91-111">We have developed a strategy for Azure AD.</span></span> <span data-ttu-id="cdc91-112">V tomto článku jsme ukážeme, jak tooinstall hello modul a poté přidejte hello Azure AD `passport-azure-ad` modulu plug-in.</span><span class="sxs-lookup"><span data-stu-id="cdc91-112">In this article, we show you how tooinstall hello module, and then add hello Azure AD `passport-azure-ad` plug-in.</span></span>

## <a name="download"></a><span data-ttu-id="cdc91-113">Ke stažení</span><span class="sxs-lookup"><span data-stu-id="cdc91-113">Download</span></span>
<span data-ttu-id="cdc91-114">Hello kód v tomto kurzu se udržuje [na Githubu](https://github.com/AzureADQuickStarts/AppModelv2-WebAPI-nodejs).</span><span class="sxs-lookup"><span data-stu-id="cdc91-114">hello code for this tutorial is maintained [on GitHub](https://github.com/AzureADQuickStarts/AppModelv2-WebAPI-nodejs).</span></span> <span data-ttu-id="cdc91-115">toofollow hello kurzu můžete [stáhnout kostru aplikace hello jako soubor ZIP](https://github.com/AzureADQuickStarts/AppModelv2-WebAPI-nodejs/archive/skeleton.zip), nebo hello kostru klonovat:</span><span class="sxs-lookup"><span data-stu-id="cdc91-115">toofollow hello tutorial, you can [download hello app's skeleton as a .zip file](https://github.com/AzureADQuickStarts/AppModelv2-WebAPI-nodejs/archive/skeleton.zip), or clone hello skeleton:</span></span>

```git clone --branch skeleton https://github.com/AzureADQuickStarts/AppModelv2-WebAPI-nodejs.git```

<span data-ttu-id="cdc91-116">Také můžete získat aplikace hello dokončit na konci hello tohoto kurzu.</span><span class="sxs-lookup"><span data-stu-id="cdc91-116">You also can get hello completed application at hello end of this tutorial.</span></span>

## <a name="1-register-an-app"></a><span data-ttu-id="cdc91-117">1: registrace aplikace</span><span class="sxs-lookup"><span data-stu-id="cdc91-117">1: Register an app</span></span>
<span data-ttu-id="cdc91-118">Vytvoření nové aplikace v [apps.dev.microsoft.com](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList), nebo postupujte podle [tyto podrobné kroky](active-directory-v2-app-registration.md) tooregister aplikace.</span><span class="sxs-lookup"><span data-stu-id="cdc91-118">Create a new app at [apps.dev.microsoft.com](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList), or follow [these detailed steps](active-directory-v2-app-registration.md) tooregister an app.</span></span> <span data-ttu-id="cdc91-119">Ověřte, že je:</span><span class="sxs-lookup"><span data-stu-id="cdc91-119">Make sure you:</span></span>

* <span data-ttu-id="cdc91-120">Kopírování hello **Id aplikace** přiřazené tooyour aplikace.</span><span class="sxs-lookup"><span data-stu-id="cdc91-120">Copy hello **Application Id** assigned tooyour app.</span></span> <span data-ttu-id="cdc91-121">Budete ho potřebovat pro účely tohoto kurzu.</span><span class="sxs-lookup"><span data-stu-id="cdc91-121">You'll need it for this tutorial.</span></span>
* <span data-ttu-id="cdc91-122">Přidat hello **Mobile** platformu pro vaši aplikaci.</span><span class="sxs-lookup"><span data-stu-id="cdc91-122">Add hello **Mobile** platform for your app.</span></span>
* <span data-ttu-id="cdc91-123">Kopírování hello **identifikátor URI pro přesměrování** z portálu hello.</span><span class="sxs-lookup"><span data-stu-id="cdc91-123">Copy hello **Redirect URI** from hello portal.</span></span> <span data-ttu-id="cdc91-124">Musíte použít hello výchozí hodnota URI `urn:ietf:wg:oauth:2.0:oob`.</span><span class="sxs-lookup"><span data-stu-id="cdc91-124">You must use hello default URI value of `urn:ietf:wg:oauth:2.0:oob`.</span></span>

## <a name="2-install-nodejs"></a><span data-ttu-id="cdc91-125">2: Instalace softwaru Node.js</span><span class="sxs-lookup"><span data-stu-id="cdc91-125">2: Install Node.js</span></span>
<span data-ttu-id="cdc91-126">Ukázka hello toouse pro tento kurz, musíte [instalace softwaru Node.js](http://nodejs.org).</span><span class="sxs-lookup"><span data-stu-id="cdc91-126">toouse hello sample for this tutorial, you must [install Node.js](http://nodejs.org).</span></span>

## <a name="3-install-mongodb"></a><span data-ttu-id="cdc91-127">3: instalace MongoDB</span><span class="sxs-lookup"><span data-stu-id="cdc91-127">3: Install MongoDB</span></span>
<span data-ttu-id="cdc91-128">toosuccessfully tuto ukázku použít, musíte [nainstalujte MongoDB](http://www.mongodb.org).</span><span class="sxs-lookup"><span data-stu-id="cdc91-128">toosuccessfully use this sample, you must [install MongoDB](http://www.mongodb.org).</span></span> <span data-ttu-id="cdc91-129">V této ukázce použijete MongoDB toomake trvalé rozhraní REST API napříč instancemi serveru.</span><span class="sxs-lookup"><span data-stu-id="cdc91-129">In this sample, you use MongoDB toomake your REST API persistent across server instances.</span></span>

> [!NOTE]
> <span data-ttu-id="cdc91-130">V tomto článku předpokládáme, že používáte hello výchozí instalaci a koncové body serveru pro MongoDB: mongodb://localhost.</span><span class="sxs-lookup"><span data-stu-id="cdc91-130">In this article, we assume that you use hello default installation and server endpoints for MongoDB: mongodb://localhost.</span></span>
> 
> 

## <a name="4-install-hello-restify-modules-in-your-web-api"></a><span data-ttu-id="cdc91-131">4: instalace hello restify modulů ve vašem webovém rozhraní API</span><span class="sxs-lookup"><span data-stu-id="cdc91-131">4: Install hello restify modules in your web API</span></span>
<span data-ttu-id="cdc91-132">Používáme Resitfy toobuild našem REST API.</span><span class="sxs-lookup"><span data-stu-id="cdc91-132">We use Resitfy toobuild our REST API.</span></span> <span data-ttu-id="cdc91-133">Restify je minimalistické a flexibilní Node.js aplikace rozhraní, které je odvozené z Express.</span><span class="sxs-lookup"><span data-stu-id="cdc91-133">Restify is a minimal and flexible Node.js application framework that's derived from Express.</span></span> <span data-ttu-id="cdc91-134">Restify obsahuje robustní sadu funkcí, které můžete použít toobuild rozhraní REST API postavených na protokolu Connect.</span><span class="sxs-lookup"><span data-stu-id="cdc91-134">Restify has a robust set of features that you can use toobuild REST APIs on top of Connect.</span></span>

### <a name="install-restify"></a><span data-ttu-id="cdc91-135">Instalace restify</span><span class="sxs-lookup"><span data-stu-id="cdc91-135">Install restify</span></span>
1.  <span data-ttu-id="cdc91-136">V příkazovém řádku změňte adresář hello příliš**azuread**:</span><span class="sxs-lookup"><span data-stu-id="cdc91-136">At a command prompt, change hello directory too**azuread**:</span></span>

    `cd azuread`

    <span data-ttu-id="cdc91-137">Pokud hello **azuread** adresář neexistuje, vytvořte ho:</span><span class="sxs-lookup"><span data-stu-id="cdc91-137">If hello **azuread** directory does not exist, create it:</span></span>

    `mkdir azuread`

2.  <span data-ttu-id="cdc91-138">Instalace restify:</span><span class="sxs-lookup"><span data-stu-id="cdc91-138">Install restify:</span></span>

    `npm install restify`

    <span data-ttu-id="cdc91-139">Hello výstup tohoto příkazu by měl vypadat takto:</span><span class="sxs-lookup"><span data-stu-id="cdc91-139">hello output of this command should look like this:</span></span>

    ```
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
    └── bunyan@0.22.0(mv@0.0.5)
    ```

#### <a name="did-you-get-an-error"></a><span data-ttu-id="cdc91-140">Obdrželi jste chybu?</span><span class="sxs-lookup"><span data-stu-id="cdc91-140">Did you get an error?</span></span>
<span data-ttu-id="cdc91-141">V některých operačních systémech, když používáte hello `npm` příkaz, může se zobrazit tato zpráva: `Error: EPERM, chmod '/usr/local/bin/..'`.</span><span class="sxs-lookup"><span data-stu-id="cdc91-141">On some operating systems, when you use hello `npm` command, you might see this message: `Error: EPERM, chmod '/usr/local/bin/..'`.</span></span> <span data-ttu-id="cdc91-142">Chyba Hello následuje žádost zkuste spuštěné hello účet jako správce.</span><span class="sxs-lookup"><span data-stu-id="cdc91-142">hello error is followed by a request that you try running hello account as an administrator.</span></span> <span data-ttu-id="cdc91-143">Pokud k tomu dojde, použijte příkaz hello `sudo` toorun `npm` na vyšší úrovni oprávnění.</span><span class="sxs-lookup"><span data-stu-id="cdc91-143">If this occurs, use hello command `sudo` toorun `npm` at a higher privilege level.</span></span>

#### <a name="did-you-get-an-error-related-toodtrace"></a><span data-ttu-id="cdc91-144">Obdrželi jste chybu související tooDTrace?</span><span class="sxs-lookup"><span data-stu-id="cdc91-144">Did you get an error related tooDTrace?</span></span>
<span data-ttu-id="cdc91-145">Při instalaci restify, může se zobrazit tato zpráva:</span><span class="sxs-lookup"><span data-stu-id="cdc91-145">When you install restify, you might see this message:</span></span>

```Shell
clang: error: no such file or directory: 'HD/azuread/node_modules/restify/node_modules/dtrace-provider/libusdt'
make: *** [Release/DTraceProviderBindings.node] Error 1
gyp ERR! build error
gyp ERR! stack Error: `make` failed with exit code: two
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

<span data-ttu-id="cdc91-146">Restify má efektivní mechanismus tootrace, volání REST pomocí DTrace.</span><span class="sxs-lookup"><span data-stu-id="cdc91-146">Restify has a powerful mechanism tootrace REST calls by using DTrace.</span></span> <span data-ttu-id="cdc91-147">DTrace však není k dispozici v operačních systémech.</span><span class="sxs-lookup"><span data-stu-id="cdc91-147">However, DTrace is not available on many operating systems.</span></span> <span data-ttu-id="cdc91-148">Tato chybová zpráva můžete bezpečně ignorovat.</span><span class="sxs-lookup"><span data-stu-id="cdc91-148">You can safely ignore this error message.</span></span>


## <a name="5-install-passportjs-in-your-web-api"></a><span data-ttu-id="cdc91-149">5: instalace Passport.js ve vašem webovém rozhraní API</span><span class="sxs-lookup"><span data-stu-id="cdc91-149">5: Install Passport.js in your web API</span></span>
1.  <span data-ttu-id="cdc91-150">Na příkazovém řádku hello změnit adresář, hello příliš**azuread**.</span><span class="sxs-lookup"><span data-stu-id="cdc91-150">At hello command prompt, change hello directory too**azuread**.</span></span>

2.  <span data-ttu-id="cdc91-151">Nainstalujte Passport.js:</span><span class="sxs-lookup"><span data-stu-id="cdc91-151">Install Passport.js:</span></span>

    `npm install passport`

    <span data-ttu-id="cdc91-152">výstup Hello hello příkazu by měl vypadat takto:</span><span class="sxs-lookup"><span data-stu-id="cdc91-152">hello output of hello command should look like this:</span></span>

    ```
     passport@0.1.17 node_modules\passport
    ├── pause@0.0.1
    └── pkginfo@0.2.3
    ```

## <a name="6-add-passport-azure-ad-tooyour-web-api"></a><span data-ttu-id="cdc91-153">6: Přidání passport-azure-ad tooyour webového rozhraní API</span><span class="sxs-lookup"><span data-stu-id="cdc91-153">6: Add passport-azure-ad tooyour web API</span></span>
<span data-ttu-id="cdc91-154">Dál přidejte strategii OAuth hello, pomocí passport-azuread.</span><span class="sxs-lookup"><span data-stu-id="cdc91-154">Next, add hello OAuth strategy, by using passport-azuread.</span></span> <span data-ttu-id="cdc91-155">`passport-azuread`je sada strategií, které propojují Azure AD s Passport.</span><span class="sxs-lookup"><span data-stu-id="cdc91-155">`passport-azuread` is a suite of strategies that connect Azure AD with Passport.</span></span> <span data-ttu-id="cdc91-156">Používáme tuto strategii pro nosné tokeny v této ukázce REST API.</span><span class="sxs-lookup"><span data-stu-id="cdc91-156">We use this strategy for bearer tokens in this REST API sample.</span></span>

> [!NOTE]
> <span data-ttu-id="cdc91-157">I když OAuth 2.0 poskytuje rozhraní, ve kterém můžou být vystavené všechny známé typy tokenů, se běžně používají určitým typům tokenů.</span><span class="sxs-lookup"><span data-stu-id="cdc91-157">Although OAuth 2.0 provides a framework in which any known token type can be issued, certain token types are commonly used.</span></span> <span data-ttu-id="cdc91-158">Běžně používané tooprotect koncových bodů jsou nosné tokeny.</span><span class="sxs-lookup"><span data-stu-id="cdc91-158">Bearer tokens are commonly used tooprotect endpoints.</span></span> <span data-ttu-id="cdc91-159">Jsou nosné tokeny hello nejčastěji vydané typem tokenů v OAuth 2.0.</span><span class="sxs-lookup"><span data-stu-id="cdc91-159">Bearer tokens are hello most widely issued type of token in OAuth 2.0.</span></span> <span data-ttu-id="cdc91-160">Mnoho implementací OAuth 2.0 předpokládá, že jsou nosné tokeny jediným typem vydávaných tokenů hello.</span><span class="sxs-lookup"><span data-stu-id="cdc91-160">Many OAuth 2.0 implementations assume that bearer tokens are hello only type of token issued.</span></span>
> 
> 

1.  <span data-ttu-id="cdc91-161">V příkazovém řádku změňte adresář hello příliš**azuread**.</span><span class="sxs-lookup"><span data-stu-id="cdc91-161">At a command prompt, change hello directory too**azuread**.</span></span>

    `cd azuread`

2.  <span data-ttu-id="cdc91-162">Nainstalujte hello Passport.js `passport-azure-ad` modul:</span><span class="sxs-lookup"><span data-stu-id="cdc91-162">Install hello Passport.js `passport-azure-ad` module:</span></span>

    `npm install passport-azure-ad`

    <span data-ttu-id="cdc91-163">výstup Hello hello příkazu by měl vypadat takto:</span><span class="sxs-lookup"><span data-stu-id="cdc91-163">hello output of hello command should look like this:</span></span>

    ```
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
    ```

## <a name="7-add-mongodb-modules-tooyour-web-api"></a><span data-ttu-id="cdc91-164">7: přidejte MongoDB moduly tooyour webového rozhraní API</span><span class="sxs-lookup"><span data-stu-id="cdc91-164">7: Add MongoDB modules tooyour web API</span></span>
<span data-ttu-id="cdc91-165">V této ukázce používáme jako naše úložiště dat MongoDB.</span><span class="sxs-lookup"><span data-stu-id="cdc91-165">In this sample, we use MongoDB as our data store.</span></span> 

1.  <span data-ttu-id="cdc91-166">Nainstalujte Mongoose, často používaný modul plug-in, toomanage modelů a schémat:</span><span class="sxs-lookup"><span data-stu-id="cdc91-166">Install Mongoose, a widely used plug-in, toomanage models and schemas:</span></span> 

    `npm install mongoose`

2.  <span data-ttu-id="cdc91-167">Nainstalujte hello databáze ovladačů pro MongoDB, které je také nazývaný MongoDB:</span><span class="sxs-lookup"><span data-stu-id="cdc91-167">Install hello database driver for MongoDB, which is also called MongoDB:</span></span>

    `npm install mongodb`

## <a name="8-install-additional-modules"></a><span data-ttu-id="cdc91-168">8: instalace dalších modulů</span><span class="sxs-lookup"><span data-stu-id="cdc91-168">8: Install additional modules</span></span>
<span data-ttu-id="cdc91-169">Nainstalujte zbývající požadované moduly hello.</span><span class="sxs-lookup"><span data-stu-id="cdc91-169">Install hello remaining required modules.</span></span>

1.  <span data-ttu-id="cdc91-170">V příkazovém řádku změňte adresář hello příliš**azuread**:</span><span class="sxs-lookup"><span data-stu-id="cdc91-170">At a command prompt, change hello directory too**azuread**:</span></span>

    `cd azuread`

2.  <span data-ttu-id="cdc91-171">Zadejte následující příkazy hello.</span><span class="sxs-lookup"><span data-stu-id="cdc91-171">Enter hello following commands.</span></span> <span data-ttu-id="cdc91-172">příkazy Hello nainstalovat následující moduly ve vašem adresáři node_modules hello:</span><span class="sxs-lookup"><span data-stu-id="cdc91-172">hello commands install hello following modules in your node_modules directory:</span></span>

    *   `npm install crypto`
    *   `npm install assert-plus`
    *   `npm install posix-getopt`
    *   `npm install util`
    *   `npm install path`
    *   `npm install connect`
    *   `npm install xml-crypto`
    *   `npm install xml2js`
    *   `npm install xmldom`
    *   `npm install async`
    *   `npm install request`
    *   `npm install underscore`
    *   `npm install grunt-contrib-jshint@0.1.1`
    *   `npm install grunt-contrib-nodeunit@0.1.2`
    *   `npm install grunt-contrib-watch@0.2.0`
    *   `npm install grunt@0.4.1`
    *   `npm install xtend@2.0.3`
    *   `npm install bunyan`
    *   `npm update`

## <a name="9-create-a-serverjs-file-for-your-dependencies"></a><span data-ttu-id="cdc91-173">9: vytvoření souboru Server.js pro svoje závislosti</span><span class="sxs-lookup"><span data-stu-id="cdc91-173">9: Create a Server.js file for your dependencies</span></span>
<span data-ttu-id="cdc91-174">Soubor Server.js obsahuje většinu hello hello funkcí webového rozhraní API serveru.</span><span class="sxs-lookup"><span data-stu-id="cdc91-174">A Server.js file holds hello majority of hello functionality for your web API server.</span></span> <span data-ttu-id="cdc91-175">Přidáte většinu kódu toothis souboru.</span><span class="sxs-lookup"><span data-stu-id="cdc91-175">Add most of your code toothis file.</span></span> <span data-ttu-id="cdc91-176">Pro produkční účely můžete Refaktorovat hello funkce do menších souborů, jako je pro samostatné trasy a ovladače.</span><span class="sxs-lookup"><span data-stu-id="cdc91-176">For production purposes, you can refactor hello functionality into smaller files, like for separate routes and controllers.</span></span> <span data-ttu-id="cdc91-177">V tomto článku používáme Server.js pro tento účel.</span><span class="sxs-lookup"><span data-stu-id="cdc91-177">In this article, we use Server.js for this purpose.</span></span>

1.  <span data-ttu-id="cdc91-178">V příkazovém řádku změňte adresář hello příliš**azuread**:</span><span class="sxs-lookup"><span data-stu-id="cdc91-178">At a command prompt, change hello directory too**azuread**:</span></span>

    `cd azuread`

2.  <span data-ttu-id="cdc91-179">Pomocí zvoleného editoru, vytvořte soubor Server.js.</span><span class="sxs-lookup"><span data-stu-id="cdc91-179">Using an editor of your choice, create a Server.js file.</span></span> <span data-ttu-id="cdc91-180">Přidejte následující informace toohello soubor hello:</span><span class="sxs-lookup"><span data-stu-id="cdc91-180">Add hello following information toohello file:</span></span>

    ```Javascript
    'use strict';
    /**
    * Module dependencies.
    */
    var util = require('util');
    var assert = require('assert-plus');
    var mongoose = require('mongoose/');
    var bunyan = require('bunyan');
    var restify = require('restify');
    var config = require('./config');
    var passport = require('passport');
    var OIDCBearerStrategy = require('passport-azure-ad').OIDCStrategy;
    ```

3.  <span data-ttu-id="cdc91-181">Uložte soubor hello.</span><span class="sxs-lookup"><span data-stu-id="cdc91-181">Save hello file.</span></span> <span data-ttu-id="cdc91-182">Vrátíte se tooit za chvíli.</span><span class="sxs-lookup"><span data-stu-id="cdc91-182">You will return tooit shortly.</span></span>

## <a name="10-create-a-config-file-toostore-your-azure-ad-settings"></a><span data-ttu-id="cdc91-183">10: vytvoření souboru toostore konfigurace nastavení Azure AD</span><span class="sxs-lookup"><span data-stu-id="cdc91-183">10: Create a config file toostore your Azure AD settings</span></span>
<span data-ttu-id="cdc91-184">Tento soubor s kódem předává parametry konfigurace hello z vaší tooPassport.js portálu Azure AD.</span><span class="sxs-lookup"><span data-stu-id="cdc91-184">This code file passes hello configuration parameters from your Azure AD portal tooPassport.js.</span></span> <span data-ttu-id="cdc91-185">Tyto hodnoty konfigurace jste vytvořili, když jste přidali hello webové rozhraní API toohello portálu na začátku hello hello článku.</span><span class="sxs-lookup"><span data-stu-id="cdc91-185">You created these configuration values when you added hello web API toohello portal at hello beginning of hello article.</span></span> <span data-ttu-id="cdc91-186">Po zkopírování hello kód vysvětlíme, co tooput v hello hodnoty těchto parametrů.</span><span class="sxs-lookup"><span data-stu-id="cdc91-186">After you copy hello code, we'll explain what tooput in hello values of these parameters.</span></span>

1.  <span data-ttu-id="cdc91-187">V příkazovém řádku změňte adresář hello příliš**azuread**:</span><span class="sxs-lookup"><span data-stu-id="cdc91-187">At a command prompt, change hello directory too**azuread**:</span></span>

    `cd azuread`

2.  <span data-ttu-id="cdc91-188">V editoru vytvoření souboru Config.js.</span><span class="sxs-lookup"><span data-stu-id="cdc91-188">In an editor, create a Config.js file.</span></span> <span data-ttu-id="cdc91-189">Přidejte hello následující informace:</span><span class="sxs-lookup"><span data-stu-id="cdc91-189">Add hello following information:</span></span>

    ```Javascript
    // Don't commit this file tooyour public repos. This config is for first-run.
    exports.creds = {
    mongoose_auth_local: 'mongodb://localhost/tasklist', // Your Mongo auth URI goes here.
    issuer: 'https://sts.windows.net/**<your application id>**/',
    audience: '<your redirect URI>',
    identityMetadata: 'https://login.microsoftonline.com/common/.well-known/openid-configuration' // For Microsoft, you should never need toochange this.
    };

    ```



### <a name="required-values"></a><span data-ttu-id="cdc91-190">Požadované hodnoty</span><span class="sxs-lookup"><span data-stu-id="cdc91-190">Required values</span></span>

*   <span data-ttu-id="cdc91-191">**IdentityMetadata**: to je, kdy `passport-azure-ad` vyhledá konfigurační data pro hello zprostředkovatele identity (IDP) a hello klíče toovalidate hello webové tokeny JSON (Jwt).</span><span class="sxs-lookup"><span data-stu-id="cdc91-191">**IdentityMetadata**: This is where `passport-azure-ad` looks for your configuration data for hello identity provider (IDP) and hello keys toovalidate hello JSON Web Tokens (JWTs).</span></span> <span data-ttu-id="cdc91-192">Pokud používáte Azure AD, pravděpodobně nechcete, aby toochange to.</span><span class="sxs-lookup"><span data-stu-id="cdc91-192">If you are using Azure AD, you probably don't want toochange this.</span></span>

*   <span data-ttu-id="cdc91-193">**Cílová skupina**: váš identifikátor URI přesměrování z portálu hello.</span><span class="sxs-lookup"><span data-stu-id="cdc91-193">**audience**: Your redirect URI from hello portal.</span></span>

> [!NOTE]
> <span data-ttu-id="cdc91-194">Vrátit klíče v pravidelných intervalech.</span><span class="sxs-lookup"><span data-stu-id="cdc91-194">Roll your keys at frequent intervals.</span></span> <span data-ttu-id="cdc91-195">Ujistěte se, že vždy načítat z adresy URL hello "openid_keys" a tuto aplikaci hello mají přístup hello Internetu.</span><span class="sxs-lookup"><span data-stu-id="cdc91-195">Be sure that you always pull from hello "openid_keys" URL, and that hello app can access hello Internet.</span></span>
> 
> 

## <a name="11-add-hello-configuration-tooyour-serverjs-file"></a><span data-ttu-id="cdc91-196">11: přidání souboru Server.js tooyour konfigurace hello</span><span class="sxs-lookup"><span data-stu-id="cdc91-196">11: Add hello configuration tooyour Server.js file</span></span>
<span data-ttu-id="cdc91-197">Aplikace musí tooread hello hodnoty z hello konfiguračního souboru, který jste právě vytvořili.</span><span class="sxs-lookup"><span data-stu-id="cdc91-197">Your application needs tooread hello values from hello config file you just created.</span></span> <span data-ttu-id="cdc91-198">Přidejte soubor .config hello jako požadovaný prostředek vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="cdc91-198">Add hello .config file as a required resource in your application.</span></span> <span data-ttu-id="cdc91-199">Nastavit toothose hello globální proměnné, které jsou v souboru Config.js.</span><span class="sxs-lookup"><span data-stu-id="cdc91-199">Set hello global variables toothose that are in Config.js.</span></span>

1.  <span data-ttu-id="cdc91-200">Na příkazovém řádku hello změnit adresář, hello příliš**azuread**:</span><span class="sxs-lookup"><span data-stu-id="cdc91-200">At hello command prompt, change hello directory too**azuread**:</span></span>

    `cd azuread`

2.  <span data-ttu-id="cdc91-201">V editoru otevřete Server.js.</span><span class="sxs-lookup"><span data-stu-id="cdc91-201">In an editor, open Server.js.</span></span> <span data-ttu-id="cdc91-202">Přidejte hello následující informace:</span><span class="sxs-lookup"><span data-stu-id="cdc91-202">Add hello following information:</span></span>

    ```Javascript
    var config = require('./config');
    ```

3.  <span data-ttu-id="cdc91-203">Přidejte novou část tooServer.js:</span><span class="sxs-lookup"><span data-stu-id="cdc91-203">Add a new section tooServer.js:</span></span>

    ```Javascript
    // Pass these options in toohello ODICBearerStrategy.
    var options = {
    // hello URL of hello metadata document for your app. Put hello keys for token validation from hello URL found in hello jwks_uri tag in hello metadata.
    identityMetadata: config.creds.identityMetadata,
    issuer: config.creds.issuer,
    audience: config.creds.audience
    };
    // Array toohold signed-in users and hello current signed-in user (owner).
    var users = [];
    var owner = null;
    // Your logger
    var log = bunyan.createLogger({
    name: 'Microsoft Azure Active Directory Sample'
    });
    ```

## <a name="12-add-hello-mongodb-model-and-schema-information-by-using-mongoose"></a><span data-ttu-id="cdc91-204">12: přidejte hello informace modelu a schématu MongoDB pomocí Mongoose</span><span class="sxs-lookup"><span data-stu-id="cdc91-204">12: Add hello MongoDB model and schema information by using Mongoose</span></span>
<span data-ttu-id="cdc91-205">V dalším kroku připojte tyto tři soubory ve službě REST API.</span><span class="sxs-lookup"><span data-stu-id="cdc91-205">Next, connect these three files in a REST API service.</span></span>

<span data-ttu-id="cdc91-206">V tomto článku používáme MongoDB toostore naše úlohy.</span><span class="sxs-lookup"><span data-stu-id="cdc91-206">In this article, we use MongoDB toostore our tasks.</span></span> <span data-ttu-id="cdc91-207">To probereme *krok 4*.</span><span class="sxs-lookup"><span data-stu-id="cdc91-207">We discuss this in *step 4*.</span></span>

<span data-ttu-id="cdc91-208">V souboru Config.js hello jste vytvořili v kroku 11, se nazývá databáze *tasklist*.</span><span class="sxs-lookup"><span data-stu-id="cdc91-208">In hello Config.js file you created in step 11, your database is called *tasklist*.</span></span> <span data-ttu-id="cdc91-209">Který byl uveďte na konci hello mongoose_auth_local adresy URL připojení.</span><span class="sxs-lookup"><span data-stu-id="cdc91-209">That was what you put at hello end of your mongoose_auth_local connection URL.</span></span> <span data-ttu-id="cdc91-210">Nepotřebujete toocreate tato databáze předem v MongoDB.</span><span class="sxs-lookup"><span data-stu-id="cdc91-210">You don't need toocreate this database beforehand in MongoDB.</span></span> <span data-ttu-id="cdc91-211">Hello databáze byla vytvořena na hello první spuštění vaší serverové aplikace (za předpokladu, že databáze hello již neexistuje).</span><span class="sxs-lookup"><span data-stu-id="cdc91-211">hello database is created on hello first run of your server application (assuming hello database does not already exist).</span></span>

<span data-ttu-id="cdc91-212">Hello server jste sdělili co toouse databázi MongoDB.</span><span class="sxs-lookup"><span data-stu-id="cdc91-212">You've told hello server what MongoDB database toouse.</span></span> <span data-ttu-id="cdc91-213">Dále musíte toowrite některé další kód toocreate hello modelu a schématu pro váš server úloh.</span><span class="sxs-lookup"><span data-stu-id="cdc91-213">Next, you need toowrite some additional code toocreate hello model and schema for your server's tasks.</span></span>

### <a name="hello-model"></a><span data-ttu-id="cdc91-214">Hello model</span><span class="sxs-lookup"><span data-stu-id="cdc91-214">hello model</span></span>
<span data-ttu-id="cdc91-215">Hello model schématu je velmi základní.</span><span class="sxs-lookup"><span data-stu-id="cdc91-215">hello schema model is very basic.</span></span> <span data-ttu-id="cdc91-216">Ho můžete rozšířit, pokud je potřeba.</span><span class="sxs-lookup"><span data-stu-id="cdc91-216">You can expand it if you need to.</span></span> 

<span data-ttu-id="cdc91-217">model schématu Hello má tyto hodnoty:</span><span class="sxs-lookup"><span data-stu-id="cdc91-217">hello schema model has these values:</span></span>

*   <span data-ttu-id="cdc91-218">**NÁZEV**.</span><span class="sxs-lookup"><span data-stu-id="cdc91-218">**NAME**.</span></span> <span data-ttu-id="cdc91-219">Úloha přiřazené toohello osoba Hello.</span><span class="sxs-lookup"><span data-stu-id="cdc91-219">hello person assigned toohello task.</span></span> <span data-ttu-id="cdc91-220">Toto je **řetězec** hodnotu.</span><span class="sxs-lookup"><span data-stu-id="cdc91-220">This is a **string** value.</span></span>
*   <span data-ttu-id="cdc91-221">**ÚLOHA**.</span><span class="sxs-lookup"><span data-stu-id="cdc91-221">**TASK**.</span></span> <span data-ttu-id="cdc91-222">Název Hello hello úlohy.</span><span class="sxs-lookup"><span data-stu-id="cdc91-222">hello name of hello task.</span></span> <span data-ttu-id="cdc91-223">Toto je **řetězec** hodnotu.</span><span class="sxs-lookup"><span data-stu-id="cdc91-223">This is a **string** value.</span></span>
*   <span data-ttu-id="cdc91-224">**DATUM**.</span><span class="sxs-lookup"><span data-stu-id="cdc91-224">**DATE**.</span></span> <span data-ttu-id="cdc91-225">Datum Hello je kvůli tuto úlohu hello.</span><span class="sxs-lookup"><span data-stu-id="cdc91-225">hello date that hello task is due.</span></span> <span data-ttu-id="cdc91-226">Toto je **data a času** hodnotu.</span><span class="sxs-lookup"><span data-stu-id="cdc91-226">This is a **datetime** value.</span></span>
*   <span data-ttu-id="cdc91-227">**DOKONČIT**.</span><span class="sxs-lookup"><span data-stu-id="cdc91-227">**COMPLETED**.</span></span> <span data-ttu-id="cdc91-228">Jestli dokončení úkolu hello.</span><span class="sxs-lookup"><span data-stu-id="cdc91-228">Whether hello task is completed.</span></span> <span data-ttu-id="cdc91-229">Toto je **Boolean** hodnotu.</span><span class="sxs-lookup"><span data-stu-id="cdc91-229">This is a **Boolean** value.</span></span>

### <a name="create-hello-schema-in-hello-code"></a><span data-ttu-id="cdc91-230">Vytvoření hello schématu v kódu hello</span><span class="sxs-lookup"><span data-stu-id="cdc91-230">Create hello schema in hello code</span></span>
1.  <span data-ttu-id="cdc91-231">V příkazovém řádku změňte adresář hello příliš**azuread**:</span><span class="sxs-lookup"><span data-stu-id="cdc91-231">At a command prompt, change hello directory too**azuread**:</span></span>

    `cd azuread`

2.  <span data-ttu-id="cdc91-232">V editoru otevřete Server.js.</span><span class="sxs-lookup"><span data-stu-id="cdc91-232">In your editor, open Server.js.</span></span> <span data-ttu-id="cdc91-233">Pod položku konfigurace hello přidejte hello následující informace:</span><span class="sxs-lookup"><span data-stu-id="cdc91-233">Below hello configuration entry, add hello following information:</span></span>

    ```Javascript
    // MongoDB setup.
    // Set up some configuration.
    var serverPort = process.env.PORT || 8080;
    var serverURI = (process.env.PORT) ? config.creds.mongoose_auth_mongohq : config.creds.mongoose_auth_local;
    // Connect tooMongoDB.
    global.db = mongoose.connect(serverURI);
    var Schema = mongoose.Schema;
    log.info('MongoDB Schema loaded');
    ```

<span data-ttu-id="cdc91-234">Tento kód se připojí toohello serveru MongoDB.</span><span class="sxs-lookup"><span data-stu-id="cdc91-234">This code connects toohello MongoDB server.</span></span> <span data-ttu-id="cdc91-235">Vrátí také objekt schématu.</span><span class="sxs-lookup"><span data-stu-id="cdc91-235">It also returns a schema object.</span></span>

#### <a name="using-hello-schema-create-your-model-in-hello-code"></a><span data-ttu-id="cdc91-236">Pomocí hello schématu, vytvoření modelu v kódu hello</span><span class="sxs-lookup"><span data-stu-id="cdc91-236">Using hello schema, create your model in hello code</span></span>
<span data-ttu-id="cdc91-237">Pod hello předcházející kód přidejte následující kód hello:</span><span class="sxs-lookup"><span data-stu-id="cdc91-237">Below hello preceding code, add hello following code:</span></span>

```Javascript
// Create a basic schema toostore your tasks and users.
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

<span data-ttu-id="cdc91-238">Jak se dá zjistit z hello kódu, nejprve vytvoříte schéma.</span><span class="sxs-lookup"><span data-stu-id="cdc91-238">As you can tell from hello code, first you create your schema.</span></span> <span data-ttu-id="cdc91-239">V dalším kroku vytvoříte objekt modelu.</span><span class="sxs-lookup"><span data-stu-id="cdc91-239">Next, you create a model object.</span></span> <span data-ttu-id="cdc91-240">Použít hello modelu objektu toostore dat napříč hello kódu při definování vaše **trasy**.</span><span class="sxs-lookup"><span data-stu-id="cdc91-240">You use hello model object toostore your data throughout hello code when you define your **routes**.</span></span>

## <a name="13-add-your-routes-for-your-task-rest-api-server"></a><span data-ttu-id="cdc91-241">13: Přidat trasy pro serveru úloh REST API</span><span class="sxs-lookup"><span data-stu-id="cdc91-241">13: Add your routes for your task REST API server</span></span>
<span data-ttu-id="cdc91-242">Teď, když máte toowork modelu databázi s, přidejte hello trasy, který budete používat pro svůj server REST API.</span><span class="sxs-lookup"><span data-stu-id="cdc91-242">Now that you have a database model toowork with, add hello routes you'll use for your REST API server.</span></span>

### <a name="about-routes-in-restify"></a><span data-ttu-id="cdc91-243">O trasách v restify</span><span class="sxs-lookup"><span data-stu-id="cdc91-243">About routes in restify</span></span>
<span data-ttu-id="cdc91-244">Trasy v restify pracovní přesně hello stejné stejným způsobem jako při použití hello balíku Express.</span><span class="sxs-lookup"><span data-stu-id="cdc91-244">Routes in restify work exactly hello same way they do when you use hello Express stack.</span></span> <span data-ttu-id="cdc91-245">Trasy se definují pomocí identifikátoru URI, které předpokládáte hello klienta aplikace toocall hello.</span><span class="sxs-lookup"><span data-stu-id="cdc91-245">You define routes by using hello URI that you expect hello client applications toocall.</span></span> <span data-ttu-id="cdc91-246">Trasy se obvykle definovat v samostatném souboru.</span><span class="sxs-lookup"><span data-stu-id="cdc91-246">Usually, you define your routes in a separate file.</span></span> <span data-ttu-id="cdc91-247">V tomto kurzu jsme uveďte naše trasy v Server.js.</span><span class="sxs-lookup"><span data-stu-id="cdc91-247">In this tutorial, we put our routes in Server.js.</span></span> <span data-ttu-id="cdc91-248">Pro použití v provozním prostředí doporučujeme, abyste je do svých vlastních souboru zvážit trasy.</span><span class="sxs-lookup"><span data-stu-id="cdc91-248">For production use, we recommend that you factor routes into their own file.</span></span>

<span data-ttu-id="cdc91-249">Typický vzor trasy restify je:</span><span class="sxs-lookup"><span data-stu-id="cdc91-249">A typical pattern for a restify route is:</span></span>

```Javascript
function createObject(req, res, next) {
// Do work on object.
_object.name = req.params.object; // Passed value is in req.params under object.
///...
return next(); // Keep hello server going.
}
....
server.post('/service/:add/:object', createObject); // calls createObject on routes that match this.
```


<span data-ttu-id="cdc91-250">Toto je vzor hello nejzákladnější úrovni hello.</span><span class="sxs-lookup"><span data-stu-id="cdc91-250">This is hello pattern at hello most basic level.</span></span> <span data-ttu-id="cdc91-251">Restify (a Express) poskytují mnohem hlubší funkčnost, jako jsou typy aplikací toodefine možnost hello a komplexního trasování napříč různými koncovými body.</span><span class="sxs-lookup"><span data-stu-id="cdc91-251">Restify (and Express) provide much deeper functionality, like hello ability toodefine application types, and complex routing across different endpoints.</span></span>

#### <a name="add-default-routes-tooyour-server"></a><span data-ttu-id="cdc91-252">Přidat výchozí trasy tooyour server</span><span class="sxs-lookup"><span data-stu-id="cdc91-252">Add default routes tooyour server</span></span>
<span data-ttu-id="cdc91-253">Přidat základní trasy CRUD hello: **vytvořit**, **načíst**, **aktualizace**, a **odstranit**.</span><span class="sxs-lookup"><span data-stu-id="cdc91-253">Add hello basic CRUD routes: **create**, **retrieve**, **update**, and **delete**.</span></span>

1.  <span data-ttu-id="cdc91-254">V příkazovém řádku změňte adresář hello příliš**azuread**:</span><span class="sxs-lookup"><span data-stu-id="cdc91-254">At a command prompt, change hello directory too**azuread**:</span></span>

    `cd azuread`

2.  <span data-ttu-id="cdc91-255">V editoru otevřete Server.js.</span><span class="sxs-lookup"><span data-stu-id="cdc91-255">In an editor, open Server.js.</span></span> <span data-ttu-id="cdc91-256">Pod položky databáze hello jste provedli dříve, přidejte hello následující informace:</span><span class="sxs-lookup"><span data-stu-id="cdc91-256">Below hello database entries you made earlier, add hello following information:</span></span>

    ```Javascript
    /**
    *
    * APIs for your REST task server
    */
    // Create a task.
    function createTask(req, res, next) {
    // Resitify currently has a bug that doesn't allow you tooset default headers.
    // These headers comply with CORS, and allow you toouse MongoDB Server as your response tooany origin.
    res.header("Access-Control-Allow-Origin", "*");
    res.header("Access-Control-Allow-Headers", "X-Requested-With");
    // Create a new task model, fill it, and save it tooMongoDB.
    var _task = new Task();
    if (!req.params.task) {
    req.log.warn({
    params: p
    }, 'createTodo: missing task');
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
    /// Returns hello list of TODOs that were loaded.
    function listTasks(req, res, next) {
    // Resitify currently has a bug that doesn't allow you tooset default headers.
    // These headers comply with CORS, and allow us toouse MongoDB Server as our response tooany origin.
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
    log.warn(err, "There are no tasks in hello database. Add one!");
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

### <a name="add-error-handling-for-hello-routes"></a><span data-ttu-id="cdc91-257">Přidání zpracování chyb pro trasy hello</span><span class="sxs-lookup"><span data-stu-id="cdc91-257">Add error handling for hello routes</span></span>
<span data-ttu-id="cdc91-258">Přidáte nějaké zpracování chyb pro komunikaci klienta back toohello o hello problému, který jste se setkali.</span><span class="sxs-lookup"><span data-stu-id="cdc91-258">Add some error handling so you can communicate back toohello client about hello problem you encountered.</span></span>

<span data-ttu-id="cdc91-259">Přidejte následující kód pod hello kód, který jste již zapsány hello:</span><span class="sxs-lookup"><span data-stu-id="cdc91-259">Add hello following code below hello code, which you've already written:</span></span>

```Javascript
///--- Errors for communicating something more information back toohello client.
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


## <a name="14-create-your-server"></a><span data-ttu-id="cdc91-260">14: vytvoření serveru</span><span class="sxs-lookup"><span data-stu-id="cdc91-260">14: Create your server</span></span>
<span data-ttu-id="cdc91-261">Hello poslední věcí toodo je tooadd vaší instance serveru.</span><span class="sxs-lookup"><span data-stu-id="cdc91-261">hello last thing toodo is tooadd your server instance.</span></span> <span data-ttu-id="cdc91-262">instance serveru Hello spravuje vaše volání.</span><span class="sxs-lookup"><span data-stu-id="cdc91-262">hello server instance manages your calls.</span></span>

<span data-ttu-id="cdc91-263">Restify (a Express) mají přímý přizpůsobení, které můžete použít s server REST API.</span><span class="sxs-lookup"><span data-stu-id="cdc91-263">Restify (and Express) have deep customization that you can use with a REST API server.</span></span> <span data-ttu-id="cdc91-264">V tomto kurzu používáme hello nejzákladnější nastavení.</span><span class="sxs-lookup"><span data-stu-id="cdc91-264">In this tutorial, we use hello most basic setup.</span></span>

```Javascript
/**
* Your server
*/
var server = restify.createServer({
name: "Microsoft Azure Active Directory TODO Server",
version: "2.0.1"
});
// Ensure that you don't drop data on uploads.
server.pre(restify.pre.pause());
// Clean up imprecise paths like //todo//////1//.
server.pre(restify.pre.sanitizePath());
// Handle annoying user agents (curl).
server.pre(restify.pre.userAgentConnection());
// Set a per-request Bunyan logger (with requestid filled in).
server.use(restify.requestLogger());
// Allow 5 requests/second by IP address, and burst too10.
server.use(restify.throttle({
burst: 10,
rate: 5,
ip: true,
}));
// Use common commands, such as:
server.use(restify.acceptParser(server.acceptable));
server.use(restify.dateParser());
server.use(restify.queryParser());
server.use(restify.gzipResponse());
server.use(restify.bodyParser({
mapParams: true
}));
```
## <a name="15-add-hello-routes-without-authentication-for-now"></a><span data-ttu-id="cdc91-265">15: Přidání hello tras (bez ověřování, teď)</span><span class="sxs-lookup"><span data-stu-id="cdc91-265">15: Add hello routes (without authentication, for now)</span></span>
```Javascript
/// Use CRUD tooadd hello real handlers.
/**
/*
/* Each of these handlers is protected by your Open ID Connect Bearer strategy. Invoke 'oidc-bearer'
/* in hello pasport.authenticate() method. Because REST is stateless, set 'session: false'. You 
/* don't need toomaintain session state. You can experiment with removing API protection.
/* toodo this, remove hello passport.authenticate() method:
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
// Register a default '/' handler
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
## <a name="16-run-hello-server"></a><span data-ttu-id="cdc91-266">16: Spusťte hello server</span><span class="sxs-lookup"><span data-stu-id="cdc91-266">16: Run hello server</span></span>
<span data-ttu-id="cdc91-267">Je vhodné tootest váš server před přidáním ověřování.</span><span class="sxs-lookup"><span data-stu-id="cdc91-267">It's a good idea tootest your server before you add authentication.</span></span>

<span data-ttu-id="cdc91-268">Nejjednodušší způsob, jak tootest Hello serveru je pomocí curl na příkazovém řádku.</span><span class="sxs-lookup"><span data-stu-id="cdc91-268">hello easiest way tootest your server is by using curl at a command prompt.</span></span> <span data-ttu-id="cdc91-269">toodo, budete potřebovat jednoduchý nástroj, které můžete tooparse výstup jako JSON.</span><span class="sxs-lookup"><span data-stu-id="cdc91-269">toodo this, you need a simple utility that you can use tooparse output as JSON.</span></span> 

1.  <span data-ttu-id="cdc91-270">Nainstalujte nástroj hello JSON, který používáme v hello následující příklady:</span><span class="sxs-lookup"><span data-stu-id="cdc91-270">Install hello JSON tool that we use in hello following examples:</span></span>

    `$npm install -g jsontool`

    <span data-ttu-id="cdc91-271">To globálně nainstaluje nástroj JSON hello.</span><span class="sxs-lookup"><span data-stu-id="cdc91-271">This installs hello JSON tool globally.</span></span>

2.  <span data-ttu-id="cdc91-272">Ujistěte se, že je spuštěna instance MongoDB.</span><span class="sxs-lookup"><span data-stu-id="cdc91-272">Make sure that your MongoDB instance is running:</span></span>

    `$sudo mongod`

3.  <span data-ttu-id="cdc91-273">Změnit adresář, hello příliš**azuread**, a poté spusťte curl:</span><span class="sxs-lookup"><span data-stu-id="cdc91-273">Change hello directory too**azuread**, and then run curl:</span></span>

    `$ cd azuread`
    `$ node server.js`

    `$ curl -isS http://127.0.0.1:8080 | json`

    ```Shell
    HTTP/1.1 2.0OK
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

4.  <span data-ttu-id="cdc91-274">tooadd úlohy:</span><span class="sxs-lookup"><span data-stu-id="cdc91-274">tooadd a task:</span></span>

    `$ curl -isS -X POST http://127.0.0.1:8888/tasks/brandon/Hello`

    <span data-ttu-id="cdc91-275">Hello odpověď by měla být:</span><span class="sxs-lookup"><span data-stu-id="cdc91-275">hello response should be:</span></span>

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

5.  <span data-ttu-id="cdc91-276">Seznam úloh pro Brandon:</span><span class="sxs-lookup"><span data-stu-id="cdc91-276">List tasks for Brandon:</span></span>

    `$ curl -isS http://127.0.0.1:8080/tasks/brandon/`

<span data-ttu-id="cdc91-277">Pokud tyto příkazy spustí bez chyb, jste server REST API toohello připraven tooadd OAuth.</span><span class="sxs-lookup"><span data-stu-id="cdc91-277">If all these commands run without errors, you are ready tooadd OAuth toohello REST API server.</span></span>

<span data-ttu-id="cdc91-278">*Nyní máte server REST API s MongoDB!*</span><span class="sxs-lookup"><span data-stu-id="cdc91-278">*You now have a REST API server with MongoDB!*</span></span>

## <a name="17-add-authentication-tooyour-rest-api-server"></a><span data-ttu-id="cdc91-279">17: Přidat server REST API tooyour ověřování</span><span class="sxs-lookup"><span data-stu-id="cdc91-279">17: Add authentication tooyour REST API server</span></span>
<span data-ttu-id="cdc91-280">Teď, když máte spuštěný rozhraní REST API, nastavit tak toouse se službou Azure AD.</span><span class="sxs-lookup"><span data-stu-id="cdc91-280">Now that you have a running REST API, set it up toouse it with Azure AD.</span></span>

<span data-ttu-id="cdc91-281">V příkazovém řádku změňte adresář hello příliš**azuread**:</span><span class="sxs-lookup"><span data-stu-id="cdc91-281">At a command prompt, change hello directory too**azuread**:</span></span>

`cd azuread`

### <a name="use-hello-oidcbearerstrategy-thats-included-with-passport-azure-ad"></a><span data-ttu-id="cdc91-282">Použití hello oidcbearerstrategy, která je součástí passport-azure-ad</span><span class="sxs-lookup"><span data-stu-id="cdc91-282">Use hello oidcbearerstrategy that's included with passport-azure-ad</span></span>
<span data-ttu-id="cdc91-283">Zatím jste vytvořili typický server REST se seznamem úkolů bez jakéhokoli druhu ověřování.</span><span class="sxs-lookup"><span data-stu-id="cdc91-283">So far, you've built a typical REST TODO server without any kind of authorization.</span></span> <span data-ttu-id="cdc91-284">Nyní přidáte ověřování.</span><span class="sxs-lookup"><span data-stu-id="cdc91-284">Now, add authentication.</span></span>

<span data-ttu-id="cdc91-285">Nejprve znamená to, že toouse Passport.</span><span class="sxs-lookup"><span data-stu-id="cdc91-285">First,  indicate that you want toouse Passport.</span></span> <span data-ttu-id="cdc91-286">Po konfiguraci serveru starší PUT tato práva:</span><span class="sxs-lookup"><span data-stu-id="cdc91-286">Put this right after your earlier server configuration:</span></span>

```Javascript
// Start using Passport.js.

server.use(passport.initialize()); // Starts passport
server.use(passport.session()); // Provides session support
```

> [!TIP]
> <span data-ttu-id="cdc91-287">Při psaní rozhraní API, je vhodné odkaz hello tooalways, které data toosomething jedinečné z hello token, který hello uživatel nemůže zfalšovat.</span><span class="sxs-lookup"><span data-stu-id="cdc91-287">When you write APIs, it's a good idea tooalways link hello data toosomething unique from hello token that hello user can’t spoof.</span></span> <span data-ttu-id="cdc91-288">Pokud tento server ukládá položky úkolů, uloží je na základě ID předplatného hello uživatele v hello tokenu (zavolaném prostřednictvím token.sub).</span><span class="sxs-lookup"><span data-stu-id="cdc91-288">When this server stores TODO items, it stores them based on hello user subscription ID in hello token (called through token.sub).</span></span> <span data-ttu-id="cdc91-289">Do pole "vlastník" hello vložíte hello token.sub.</span><span class="sxs-lookup"><span data-stu-id="cdc91-289">You put hello token.sub in hello “owner” field.</span></span> <span data-ttu-id="cdc91-290">To zajišťuje, aby pouze tento uživatel může přístup k TODOs hello uživatele.</span><span class="sxs-lookup"><span data-stu-id="cdc91-290">This ensures that only that user can access hello user's TODOs.</span></span> <span data-ttu-id="cdc91-291">Nikdo jiný přístup k hello TODOs, které jste zadali.</span><span class="sxs-lookup"><span data-stu-id="cdc91-291">No one else can access hello TODOs that were entered.</span></span> <span data-ttu-id="cdc91-292">Neexistuje vystavení v hello rozhraní API pro "vlastník".</span><span class="sxs-lookup"><span data-stu-id="cdc91-292">There is no exposure in hello API for “owner.”</span></span> <span data-ttu-id="cdc91-293">Externí uživatel může požádat o TODOs jiní uživatelé, i když jsou ověřeni.</span><span class="sxs-lookup"><span data-stu-id="cdc91-293">An external user can request other users' TODOs, even if they are authenticated.</span></span>
> 
> 

<span data-ttu-id="cdc91-294">Pak pomocí hello Open ID Connect nosnou strategii, která se dodává s `passport-azure-ad`.</span><span class="sxs-lookup"><span data-stu-id="cdc91-294">Next, use hello Open ID Connect Bearer strategy that comes with `passport-azure-ad`.</span></span> <span data-ttu-id="cdc91-295">To uvedli po jste vložili dříve:</span><span class="sxs-lookup"><span data-stu-id="cdc91-295">Put this after what you pasted earlier:</span></span>

```Javascript
/**
/*
/* Calling hello OIDCBearerStrategy and managing users.
/*
/* Because of hello Passport pattern, you need toomanage users and info tokens
/* with a FindorCreate() method. hello method must be provided by hello implementor.
/* In hello following code, you autoregister any user and implement a FindById().
/* It's a good idea toodo something more advanced.
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
var oidcStrategy = new OIDCBearerStrategy(options,
function(token, done) {
log.info('verifying hello user');
log.info(token, 'was hello token retrieved');
findById(token.sub, function(err, user) {
if (err) {
return done(err);
}
if (!user) {
// "Auto-registration"
log.info('User was added automatically, because they were new. Their sub is: ', token.sub);
users.push(token);
owner = token.sub;
return done(null, token);
}
owner = token.sub;
return done(null, user, token);
});
}
);
passport.use(oidcStrategy);
```

<span data-ttu-id="cdc91-296">Passport používá podobný Princip pro všechny svoje strategie (Twitteru, Facebooku a tak dále).</span><span class="sxs-lookup"><span data-stu-id="cdc91-296">Passport uses a similar pattern for all its strategies (Twitter, Facebook, and so on).</span></span> <span data-ttu-id="cdc91-297">Vzor toohello řídí všichni tvůrci strategií.</span><span class="sxs-lookup"><span data-stu-id="cdc91-297">All strategy writers adhere toohello pattern.</span></span> <span data-ttu-id="cdc91-298">Předat hello strategie `function()` token, který používá a `done` jako parametry.</span><span class="sxs-lookup"><span data-stu-id="cdc91-298">Pass hello strategy a `function()` that uses a token and `done` as parameters.</span></span> <span data-ttu-id="cdc91-299">strategie Hello se vrátí po dělá svou práci.</span><span class="sxs-lookup"><span data-stu-id="cdc91-299">hello strategy is returned after it does all its work.</span></span> <span data-ttu-id="cdc91-300">Uložení hello uživatele a dočasné ukládání hello token, takže není nutné tooask pro něj znovu.</span><span class="sxs-lookup"><span data-stu-id="cdc91-300">Store hello user and stash hello token so you don’t need tooask for it again.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="cdc91-301">Hello předchozí kód přijme všechny uživatele, který může ověřit tooyour serveru.</span><span class="sxs-lookup"><span data-stu-id="cdc91-301">hello preceding code takes any user that can authenticate tooyour server.</span></span> <span data-ttu-id="cdc91-302">To se označuje jako Automatická registrace.</span><span class="sxs-lookup"><span data-stu-id="cdc91-302">This is known as auto-registration.</span></span> <span data-ttu-id="cdc91-303">Na provozním serveru nebude chcete toolet každý, kdo v bez toho, aby předtím prošli registračním procesem, který zvolíte.</span><span class="sxs-lookup"><span data-stu-id="cdc91-303">On a production server, you wouldn’t want toolet anyone in without first having them go through a registration process that you choose.</span></span> <span data-ttu-id="cdc91-304">Je to obvykle hello vzor, který můžete vidět u uživatelských aplikací.</span><span class="sxs-lookup"><span data-stu-id="cdc91-304">This is usually hello pattern you see in consumer apps.</span></span> <span data-ttu-id="cdc91-305">aplikace Hello může povolit tooregister službou Facebook, ale pak musíte tooenter Další informace.</span><span class="sxs-lookup"><span data-stu-id="cdc91-305">hello app might allow you tooregister with Facebook, but then it asks you tooenter additional information.</span></span> <span data-ttu-id="cdc91-306">Pokud v tomto kurzu nebyly pomocí příkazového řádku programu, může extrahuje hello e-mailu z hello tokenu objekt, který je vrácen.</span><span class="sxs-lookup"><span data-stu-id="cdc91-306">If you weren't using a command-line program for this tutorial, you could extract hello email from hello token object that is returned.</span></span> <span data-ttu-id="cdc91-307">Potom může zobrazit dotaz hello uživatele tooenter Další informace.</span><span class="sxs-lookup"><span data-stu-id="cdc91-307">Then, you might ask hello user tooenter additional information.</span></span> <span data-ttu-id="cdc91-308">Protože se jedná o testovací server, můžete přidat uživatele hello přímo toohello databázi v paměti.</span><span class="sxs-lookup"><span data-stu-id="cdc91-308">Because this is a test server, you add hello user directly toohello in-memory database.</span></span>
> 
> 

### <a name="protect-endpoints"></a><span data-ttu-id="cdc91-309">Ochrana koncových bodů</span><span class="sxs-lookup"><span data-stu-id="cdc91-309">Protect endpoints</span></span>
<span data-ttu-id="cdc91-310">Ochrana koncových bodů tak, že zadáte hello **passport.authenticate()** volání s hello protokolu, které chcete toouse.</span><span class="sxs-lookup"><span data-stu-id="cdc91-310">Protect endpoints by specifying hello **passport.authenticate()** call with hello protocol that you want toouse.</span></span>

<span data-ttu-id="cdc91-311">Můžete upravit trasu v kódu serveru pro pokročilejší možnosti:</span><span class="sxs-lookup"><span data-stu-id="cdc91-311">You can edit your route in your server code for a more advanced option:</span></span>

```Javascript
server.get('/tasks', passport.authenticate('oidc-bearer', {
session: false
}), listTasks);
server.get('/tasks', passport.authenticate('oidc-bearer', {
session: false
}), listTasks);
server.get('/tasks/:owner', passport.authenticate('oidc-bearer', {
session: false
}), getTask);
server.head('/tasks/:owner', passport.authenticate('oidc-bearer', {
session: false
}), getTask);
server.post('/tasks/:owner/:task', passport.authenticate('oidc-bearer', {
session: false
}), createTask);
server.post('/tasks', passport.authenticate('oidc-bearer', {
session: false
}), createTask);
server.del('/tasks/:owner/:task', passport.authenticate('oidc-bearer', {
session: false
}), removeTask);
server.del('/tasks/:owner', passport.authenticate('oidc-bearer', {
session: false
}), removeTask);
server.del('/tasks', passport.authenticate('oidc-bearer', {
session: false
}), removeTask);
server.del('/tasks', passport.authenticate('oidc-bearer', {
session: false
}), removeAll, function respond(req, res, next) {
res.send(204);
next();
});
```

## <a name="18-run-your-server-application-again"></a><span data-ttu-id="cdc91-312">18: Spusťte serverovou aplikaci znovu</span><span class="sxs-lookup"><span data-stu-id="cdc91-312">18: Run your server application again</span></span>
<span data-ttu-id="cdc91-313">Použití curl znovu toosee, pokud je nastavená ochrana OAuth 2.0 koncové body.</span><span class="sxs-lookup"><span data-stu-id="cdc91-313">Use curl again toosee if you have OAuth 2.0 protection against your endpoints.</span></span> <span data-ttu-id="cdc91-314">To udělejte předtím, než spustíte všechny klientské sady SDK proti tomuto koncovému bodu.</span><span class="sxs-lookup"><span data-stu-id="cdc91-314">Do this before you run any of your client SDKs against this endpoint.</span></span> <span data-ttu-id="cdc91-315">Vrátí hlavičky Hello by mělo být uvedeno, zda ověřování pracuje správně.</span><span class="sxs-lookup"><span data-stu-id="cdc91-315">hello headers returned should tell you if your authentication is working correctly.</span></span>

1.  <span data-ttu-id="cdc91-316">Ujistěte se, zda je spuštěna vaše isntance MongoDB:</span><span class="sxs-lookup"><span data-stu-id="cdc91-316">Make sure that your MongoDB isntance is running:</span></span>

    `$sudo mongod`

2.  <span data-ttu-id="cdc91-317">Změnit toohello **azuread** adresář a potom pomocí curl:</span><span class="sxs-lookup"><span data-stu-id="cdc91-317">Change toohello **azuread** directory, and then use curl:</span></span>

    `$ cd azuread`

    `$ node server.js`

3.  <span data-ttu-id="cdc91-318">Zkuste základní požadavek POST:</span><span class="sxs-lookup"><span data-stu-id="cdc91-318">Try a basic POST:</span></span>

    `$ curl -isS -X POST http://127.0.0.1:8080/tasks/brandon/Hello`

    ```Shell
    HTTP/1.1 401 Unauthorized
    Connection: close
    WWW-Authenticate: Bearer realm="Users"
    Date: Tue, 14 Jul 2015 05:45:03 GMT
    Transfer-Encoding: chunked
    ```

<span data-ttu-id="cdc91-319">401 odpovědi vyplývá, že vrstva Passportu hello se pokouší tooredirect toohello zajistí autorizaci koncového bodu, který je právě co chcete použít.</span><span class="sxs-lookup"><span data-stu-id="cdc91-319">A 401 response indicates that hello Passport layer is trying tooredirect toohello authorize endpoint, which is exactly what you want.</span></span>

<span data-ttu-id="cdc91-320">*Nyní máte službu REST API, která používá OAuth 2.0!*</span><span class="sxs-lookup"><span data-stu-id="cdc91-320">*You now have a REST API service that uses OAuth 2.0!*</span></span>

<span data-ttu-id="cdc91-321">Jste došli nejdál, co můžete bez použití klienta OAuth 2.0 kompatibilní s tímto serverem.</span><span class="sxs-lookup"><span data-stu-id="cdc91-321">You've gone as far as you can with this server without using an OAuth 2.0-compatible client.</span></span> <span data-ttu-id="cdc91-322">K tomu budete potřebovat tooreview další kurzu.</span><span class="sxs-lookup"><span data-stu-id="cdc91-322">For that, you will need tooreview an additional tutorial.</span></span>

## <a name="next-steps"></a><span data-ttu-id="cdc91-323">Další kroky</span><span class="sxs-lookup"><span data-stu-id="cdc91-323">Next steps</span></span>
<span data-ttu-id="cdc91-324">Pro srovnání je hello dokončit ukázka (bez vašich hodnot nastavení) k dispozici jako [soubor .zip](https://github.com/AzureADQuickStarts/AppModelv2-WebAPI-nodejs/archive/complete.zip).</span><span class="sxs-lookup"><span data-stu-id="cdc91-324">For reference, hello completed sample (without your configuration values) is provided as [a .zip file](https://github.com/AzureADQuickStarts/AppModelv2-WebAPI-nodejs/archive/complete.zip).</span></span> <span data-ttu-id="cdc91-325">Vám může ho také klonovat z Githubu:</span><span class="sxs-lookup"><span data-stu-id="cdc91-325">You also can clone it from GitHub:</span></span>

```git clone --branch complete https://github.com/AzureADQuickStarts/AppModelv2-WebAPI-nodejs.git```

<span data-ttu-id="cdc91-326">Teď můžete přesunout na toomore advanced témata.</span><span class="sxs-lookup"><span data-stu-id="cdc91-326">Now, you can move on toomore advanced topics.</span></span> <span data-ttu-id="cdc91-327">Můžete chtít tootry [zabezpečení webové aplikace Node.js pomocí koncového bodu v2.0 hello](active-directory-v2-devquickstarts-node-web.md).</span><span class="sxs-lookup"><span data-stu-id="cdc91-327">You might want tootry [Secure a Node.js web app using hello v2.0 endpoint](active-directory-v2-devquickstarts-node-web.md).</span></span>

<span data-ttu-id="cdc91-328">Zde jsou některé další zdroje informací:</span><span class="sxs-lookup"><span data-stu-id="cdc91-328">Here are some additional resources:</span></span>

* [<span data-ttu-id="cdc91-329">Příručka vývojáře v2.0 služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="cdc91-329">Azure AD v2.0 developer guide</span></span>](active-directory-appmodel-v2-overview.md)
* [<span data-ttu-id="cdc91-330">Značka "azure-active-directory" přetečení zásobníku</span><span class="sxs-lookup"><span data-stu-id="cdc91-330">Stack Overflow "azure-active-directory" tag</span></span>](http://stackoverflow.com/questions/tagged/azure-active-directory)

### <a name="get-security-updates-for-our-products"></a><span data-ttu-id="cdc91-331">Získejte bezpečnostní aktualizace našich produktů</span><span class="sxs-lookup"><span data-stu-id="cdc91-331">Get security updates for our products</span></span>
<span data-ttu-id="cdc91-332">Doporučujeme toosign až toobe upozorněni, když bezpečnostních incidentech.</span><span class="sxs-lookup"><span data-stu-id="cdc91-332">We encourage you toosign up toobe notified when security incidents occur.</span></span> <span data-ttu-id="cdc91-333">Na hello [technické oznámení zabezpečení Microsoft](https://technet.microsoft.com/security/dd252948) stránky, přihlášení k odběru tooSecurity zpravodaje výstrahy.</span><span class="sxs-lookup"><span data-stu-id="cdc91-333">On hello [Microsoft Technical Security Notifications](https://technet.microsoft.com/security/dd252948) page, subscribe tooSecurity Advisories Alerts.</span></span>

