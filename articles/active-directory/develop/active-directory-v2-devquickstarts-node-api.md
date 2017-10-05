---
title: "Zabezpečení Azure Active Directory v2.0 webového rozhraní API pomocí Node.js | Microsoft Docs"
description: "Naučte se vytvářet webové aplikace Node.js API, které přijímá tokeny z osobního účtu Microsoft a z pracovní nebo školní účty."
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
ms.openlocfilehash: 94e945a52b9df7c495de1611baa08083357670c9
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="secure-a-web-api-by-using-nodejs"></a><span data-ttu-id="ff431-103">Zabezpečení webového rozhraní API pomocí Node.js</span><span class="sxs-lookup"><span data-stu-id="ff431-103">Secure a web API by using Node.js</span></span>
> [!NOTE]
> <span data-ttu-id="ff431-104">Ne všechny funkce a scénáře Azure Active Directory fungovat s koncovým bodem v2.0.</span><span class="sxs-lookup"><span data-stu-id="ff431-104">Not all Azure Active Directory scenarios and features work with the v2.0 endpoint.</span></span> <span data-ttu-id="ff431-105">Pokud chcete zjistit, zda by měl používat koncového bodu v2.0 nebo koncový bod verze 1.0, přečtěte si informace o [v2.0 omezení](active-directory-v2-limitations.md).</span><span class="sxs-lookup"><span data-stu-id="ff431-105">To determine whether you should use the v2.0 endpoint or the v1.0 endpoint, read about [v2.0 limitations](active-directory-v2-limitations.md).</span></span>
> 
> 

<span data-ttu-id="ff431-106">Pokud používáte koncového bodu v2.0 Azure Active Directory (Azure AD), můžete použít [OAuth 2.0](active-directory-v2-protocols.md) přístup tokeny k ochraně vašeho webového rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="ff431-106">When you use the Azure Active Directory (Azure AD) v2.0 endpoint, you can use [OAuth 2.0](active-directory-v2-protocols.md) access tokens to protect your web API.</span></span> <span data-ttu-id="ff431-107">S OAuth 2.0 tokeny přístupu, uživatelé, kteří mají osobní účet Microsoft i pracovní nebo školní účty mít bezpečný přístup k vašemu webovému rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="ff431-107">With OAuth 2.0 access tokens, users who have both a personal Microsoft account and work or school accounts can securely access your web API.</span></span>

<span data-ttu-id="ff431-108">*Passport* je ověřovací middleware pro Node.js.</span><span class="sxs-lookup"><span data-stu-id="ff431-108">*Passport* is authentication middleware for Node.js.</span></span> <span data-ttu-id="ff431-109">Flexibilní a modulární, můžete do jakéhokoli nenápadně vyřadit Passport využívající Express nebo restify webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="ff431-109">Flexible and modular, Passport can be unobtrusively dropped into any Express-based or restify web application.</span></span> <span data-ttu-id="ff431-110">Komplexní sada strategií Passport, podporují ověřování pomocí uživatelského jména a hesla, Facebook, Twitter a další možnosti.</span><span class="sxs-lookup"><span data-stu-id="ff431-110">In Passport, a comprehensive set of strategies support authentication by using a username and password, Facebook, Twitter, or other options.</span></span> <span data-ttu-id="ff431-111">Vyvinuli jsme strategii pro Azure AD.</span><span class="sxs-lookup"><span data-stu-id="ff431-111">We have developed a strategy for Azure AD.</span></span> <span data-ttu-id="ff431-112">V tomto článku jsme ukazují, jak nainstalovat modul a poté přidejte Azure AD `passport-azure-ad` modulu plug-in.</span><span class="sxs-lookup"><span data-stu-id="ff431-112">In this article, we show you how to install the module, and then add the Azure AD `passport-azure-ad` plug-in.</span></span>

## <a name="download"></a><span data-ttu-id="ff431-113">Ke stažení</span><span class="sxs-lookup"><span data-stu-id="ff431-113">Download</span></span>
<span data-ttu-id="ff431-114">Kód k tomuto kurzu je udržovaný [na GitHubu](https://github.com/AzureADQuickStarts/AppModelv2-WebAPI-nodejs).</span><span class="sxs-lookup"><span data-stu-id="ff431-114">The code for this tutorial is maintained [on GitHub](https://github.com/AzureADQuickStarts/AppModelv2-WebAPI-nodejs).</span></span> <span data-ttu-id="ff431-115">Chcete-li postupovat v kurzu, můžete [stáhnout kostru aplikace jako soubor ZIP](https://github.com/AzureADQuickStarts/AppModelv2-WebAPI-nodejs/archive/skeleton.zip), nebo tuto kostru klonovat:</span><span class="sxs-lookup"><span data-stu-id="ff431-115">To follow the tutorial, you can [download the app's skeleton as a .zip file](https://github.com/AzureADQuickStarts/AppModelv2-WebAPI-nodejs/archive/skeleton.zip), or clone the skeleton:</span></span>

```git clone --branch skeleton https://github.com/AzureADQuickStarts/AppModelv2-WebAPI-nodejs.git```

<span data-ttu-id="ff431-116">Také můžete získat hotová aplikace na konci tohoto kurzu.</span><span class="sxs-lookup"><span data-stu-id="ff431-116">You also can get the completed application at the end of this tutorial.</span></span>

## <a name="1-register-an-app"></a><span data-ttu-id="ff431-117">1: registrace aplikace</span><span class="sxs-lookup"><span data-stu-id="ff431-117">1: Register an app</span></span>
<span data-ttu-id="ff431-118">Vytvoření nové aplikace v [apps.dev.microsoft.com](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList), nebo postupujte podle [tyto podrobné kroky](active-directory-v2-app-registration.md) k registraci aplikace.</span><span class="sxs-lookup"><span data-stu-id="ff431-118">Create a new app at [apps.dev.microsoft.com](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList), or follow [these detailed steps](active-directory-v2-app-registration.md) to register an app.</span></span> <span data-ttu-id="ff431-119">Ověřte, že je:</span><span class="sxs-lookup"><span data-stu-id="ff431-119">Make sure you:</span></span>

* <span data-ttu-id="ff431-120">Kopírování **Id aplikace** přiřazené vaší aplikaci.</span><span class="sxs-lookup"><span data-stu-id="ff431-120">Copy the **Application Id** assigned to your app.</span></span> <span data-ttu-id="ff431-121">Budete ho potřebovat pro účely tohoto kurzu.</span><span class="sxs-lookup"><span data-stu-id="ff431-121">You'll need it for this tutorial.</span></span>
* <span data-ttu-id="ff431-122">Přidat **Mobile** platformu pro vaši aplikaci.</span><span class="sxs-lookup"><span data-stu-id="ff431-122">Add the **Mobile** platform for your app.</span></span>
* <span data-ttu-id="ff431-123">Kopírování **identifikátor URI pro přesměrování** z portálu.</span><span class="sxs-lookup"><span data-stu-id="ff431-123">Copy the **Redirect URI** from the portal.</span></span> <span data-ttu-id="ff431-124">Musíte použít výchozí hodnotu identifikátoru URI `urn:ietf:wg:oauth:2.0:oob`.</span><span class="sxs-lookup"><span data-stu-id="ff431-124">You must use the default URI value of `urn:ietf:wg:oauth:2.0:oob`.</span></span>

## <a name="2-install-nodejs"></a><span data-ttu-id="ff431-125">2: Instalace softwaru Node.js</span><span class="sxs-lookup"><span data-stu-id="ff431-125">2: Install Node.js</span></span>
<span data-ttu-id="ff431-126">Ukázku použít pro tento kurz, musíte [instalace softwaru Node.js](http://nodejs.org).</span><span class="sxs-lookup"><span data-stu-id="ff431-126">To use the sample for this tutorial, you must [install Node.js](http://nodejs.org).</span></span>

## <a name="3-install-mongodb"></a><span data-ttu-id="ff431-127">3: instalace MongoDB</span><span class="sxs-lookup"><span data-stu-id="ff431-127">3: Install MongoDB</span></span>
<span data-ttu-id="ff431-128">Pro úspěšné fungování této ukázky musíte [nainstalujte MongoDB](http://www.mongodb.org).</span><span class="sxs-lookup"><span data-stu-id="ff431-128">To successfully use this sample, you must [install MongoDB](http://www.mongodb.org).</span></span> <span data-ttu-id="ff431-129">V této ukázce použijete k zajištění trvalosti REST API trvalé napříč instancemi serveru MongoDB.</span><span class="sxs-lookup"><span data-stu-id="ff431-129">In this sample, you use MongoDB to make your REST API persistent across server instances.</span></span>

> [!NOTE]
> <span data-ttu-id="ff431-130">V tomto článku předpokládáme, že používáte výchozí instalaci a server koncové body pro MongoDB: mongodb://localhost.</span><span class="sxs-lookup"><span data-stu-id="ff431-130">In this article, we assume that you use the default installation and server endpoints for MongoDB: mongodb://localhost.</span></span>
> 
> 

## <a name="4-install-the-restify-modules-in-your-web-api"></a><span data-ttu-id="ff431-131">4: Instalace modulů restify ve webovém rozhraní API</span><span class="sxs-lookup"><span data-stu-id="ff431-131">4: Install the restify modules in your web API</span></span>
<span data-ttu-id="ff431-132">Používáme Resitfy k sestavení našem REST API.</span><span class="sxs-lookup"><span data-stu-id="ff431-132">We use Resitfy to build our REST API.</span></span> <span data-ttu-id="ff431-133">Restify je minimalistické a flexibilní Node.js aplikace rozhraní, které je odvozené z Express.</span><span class="sxs-lookup"><span data-stu-id="ff431-133">Restify is a minimal and flexible Node.js application framework that's derived from Express.</span></span> <span data-ttu-id="ff431-134">Restify obsahuje robustní sadu funkcí, které můžete použít k sestavení REST API postavených na protokolu Connect.</span><span class="sxs-lookup"><span data-stu-id="ff431-134">Restify has a robust set of features that you can use to build REST APIs on top of Connect.</span></span>

### <a name="install-restify"></a><span data-ttu-id="ff431-135">Instalace restify</span><span class="sxs-lookup"><span data-stu-id="ff431-135">Install restify</span></span>
1.  <span data-ttu-id="ff431-136">Na příkazovém řádku změňte adresář na **azuread**:</span><span class="sxs-lookup"><span data-stu-id="ff431-136">At a command prompt, change the directory to **azuread**:</span></span>

    `cd azuread`

    <span data-ttu-id="ff431-137">Pokud **azuread** adresář neexistuje, vytvořte ho:</span><span class="sxs-lookup"><span data-stu-id="ff431-137">If the **azuread** directory does not exist, create it:</span></span>

    `mkdir azuread`

2.  <span data-ttu-id="ff431-138">Instalace restify:</span><span class="sxs-lookup"><span data-stu-id="ff431-138">Install restify:</span></span>

    `npm install restify`

    <span data-ttu-id="ff431-139">Výstup tohoto příkazu by měl vypadat takto:</span><span class="sxs-lookup"><span data-stu-id="ff431-139">The output of this command should look like this:</span></span>

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

#### <a name="did-you-get-an-error"></a><span data-ttu-id="ff431-140">Obdrželi jste chybu?</span><span class="sxs-lookup"><span data-stu-id="ff431-140">Did you get an error?</span></span>
<span data-ttu-id="ff431-141">V některých operačních systémech se při použití `npm` příkaz, může se zobrazit tato zpráva: `Error: EPERM, chmod '/usr/local/bin/..'`.</span><span class="sxs-lookup"><span data-stu-id="ff431-141">On some operating systems, when you use the `npm` command, you might see this message: `Error: EPERM, chmod '/usr/local/bin/..'`.</span></span> <span data-ttu-id="ff431-142">Chyba následuje žádost zkuste účet Spustit jako správce.</span><span class="sxs-lookup"><span data-stu-id="ff431-142">The error is followed by a request that you try running the account as an administrator.</span></span> <span data-ttu-id="ff431-143">Pokud k tomu dojde, použijte příkaz `sudo` ke spuštění `npm` na vyšší úrovni oprávnění.</span><span class="sxs-lookup"><span data-stu-id="ff431-143">If this occurs, use the command `sudo` to run `npm` at a higher privilege level.</span></span>

#### <a name="did-you-get-an-error-related-to-dtrace"></a><span data-ttu-id="ff431-144">Obdrželi jste chybu související s DTrace?</span><span class="sxs-lookup"><span data-stu-id="ff431-144">Did you get an error related to DTrace?</span></span>
<span data-ttu-id="ff431-145">Při instalaci restify, může se zobrazit tato zpráva:</span><span class="sxs-lookup"><span data-stu-id="ff431-145">When you install restify, you might see this message:</span></span>

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

<span data-ttu-id="ff431-146">Restify má efektivní mechanismus pro trasování volání REST pomocí DTrace.</span><span class="sxs-lookup"><span data-stu-id="ff431-146">Restify has a powerful mechanism to trace REST calls by using DTrace.</span></span> <span data-ttu-id="ff431-147">DTrace však není k dispozici v operačních systémech.</span><span class="sxs-lookup"><span data-stu-id="ff431-147">However, DTrace is not available on many operating systems.</span></span> <span data-ttu-id="ff431-148">Tato chybová zpráva můžete bezpečně ignorovat.</span><span class="sxs-lookup"><span data-stu-id="ff431-148">You can safely ignore this error message.</span></span>


## <a name="5-install-passportjs-in-your-web-api"></a><span data-ttu-id="ff431-149">5: instalace Passport.js ve vašem webovém rozhraní API</span><span class="sxs-lookup"><span data-stu-id="ff431-149">5: Install Passport.js in your web API</span></span>
1.  <span data-ttu-id="ff431-150">Na příkazovém řádku změňte adresář na **azuread**.</span><span class="sxs-lookup"><span data-stu-id="ff431-150">At the command prompt, change the directory to **azuread**.</span></span>

2.  <span data-ttu-id="ff431-151">Nainstalujte Passport.js:</span><span class="sxs-lookup"><span data-stu-id="ff431-151">Install Passport.js:</span></span>

    `npm install passport`

    <span data-ttu-id="ff431-152">Výstup příkazu by měl vypadat takto:</span><span class="sxs-lookup"><span data-stu-id="ff431-152">The output of the command should look like this:</span></span>

    ```
     passport@0.1.17 node_modules\passport
    ├── pause@0.0.1
    └── pkginfo@0.2.3
    ```

## <a name="6-add-passport-azure-ad-to-your-web-api"></a><span data-ttu-id="ff431-153">6: Přidání passport-azure-ad do webového rozhraní API</span><span class="sxs-lookup"><span data-stu-id="ff431-153">6: Add passport-azure-ad to your web API</span></span>
<span data-ttu-id="ff431-154">Dál přidejte strategii OAuth pomocí passport-azuread.</span><span class="sxs-lookup"><span data-stu-id="ff431-154">Next, add the OAuth strategy, by using passport-azuread.</span></span> <span data-ttu-id="ff431-155">`passport-azuread`je sada strategií, které propojují Azure AD s Passport.</span><span class="sxs-lookup"><span data-stu-id="ff431-155">`passport-azuread` is a suite of strategies that connect Azure AD with Passport.</span></span> <span data-ttu-id="ff431-156">Používáme tuto strategii pro nosné tokeny v této ukázce REST API.</span><span class="sxs-lookup"><span data-stu-id="ff431-156">We use this strategy for bearer tokens in this REST API sample.</span></span>

> [!NOTE]
> <span data-ttu-id="ff431-157">I když OAuth 2.0 poskytuje rozhraní, ve kterém můžou být vystavené všechny známé typy tokenů, se běžně používají určitým typům tokenů.</span><span class="sxs-lookup"><span data-stu-id="ff431-157">Although OAuth 2.0 provides a framework in which any known token type can be issued, certain token types are commonly used.</span></span> <span data-ttu-id="ff431-158">Nosné tokeny se běžně používají k ochraně koncových bodů.</span><span class="sxs-lookup"><span data-stu-id="ff431-158">Bearer tokens are commonly used to protect endpoints.</span></span> <span data-ttu-id="ff431-159">Nosné tokeny jsou nejčastěji vydávaným typem tokenů v OAuth 2.0.</span><span class="sxs-lookup"><span data-stu-id="ff431-159">Bearer tokens are the most widely issued type of token in OAuth 2.0.</span></span> <span data-ttu-id="ff431-160">Mnoho implementací OAuth 2.0 předpokládá, že jsou nosné tokeny jediným typem vydávaných tokenů.</span><span class="sxs-lookup"><span data-stu-id="ff431-160">Many OAuth 2.0 implementations assume that bearer tokens are the only type of token issued.</span></span>
> 
> 

1.  <span data-ttu-id="ff431-161">Na příkazovém řádku změňte adresář na **azuread**.</span><span class="sxs-lookup"><span data-stu-id="ff431-161">At a command prompt, change the directory to **azuread**.</span></span>

    `cd azuread`

2.  <span data-ttu-id="ff431-162">Nainstalujte Passport.js `passport-azure-ad` modul:</span><span class="sxs-lookup"><span data-stu-id="ff431-162">Install the Passport.js `passport-azure-ad` module:</span></span>

    `npm install passport-azure-ad`

    <span data-ttu-id="ff431-163">Výstup příkazu by měl vypadat takto:</span><span class="sxs-lookup"><span data-stu-id="ff431-163">The output of the command should look like this:</span></span>

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

## <a name="7-add-mongodb-modules-to-your-web-api"></a><span data-ttu-id="ff431-164">7: přidání modulů MongoDB do webového rozhraní API</span><span class="sxs-lookup"><span data-stu-id="ff431-164">7: Add MongoDB modules to your web API</span></span>
<span data-ttu-id="ff431-165">V této ukázce používáme jako naše úložiště dat MongoDB.</span><span class="sxs-lookup"><span data-stu-id="ff431-165">In this sample, we use MongoDB as our data store.</span></span> 

1.  <span data-ttu-id="ff431-166">Nainstalujte Mongoose, často používaný modul plug-in pro správu modelů a schémat:</span><span class="sxs-lookup"><span data-stu-id="ff431-166">Install Mongoose, a widely used plug-in, to manage models and schemas:</span></span> 

    `npm install mongoose`

2.  <span data-ttu-id="ff431-167">Nainstalujte ovladač databáze pro MongoDB, které je také nazývaný MongoDB:</span><span class="sxs-lookup"><span data-stu-id="ff431-167">Install the database driver for MongoDB, which is also called MongoDB:</span></span>

    `npm install mongodb`

## <a name="8-install-additional-modules"></a><span data-ttu-id="ff431-168">8: instalace dalších modulů</span><span class="sxs-lookup"><span data-stu-id="ff431-168">8: Install additional modules</span></span>
<span data-ttu-id="ff431-169">Nainstalujte zbývající požadované moduly.</span><span class="sxs-lookup"><span data-stu-id="ff431-169">Install the remaining required modules.</span></span>

1.  <span data-ttu-id="ff431-170">Na příkazovém řádku změňte adresář na **azuread**:</span><span class="sxs-lookup"><span data-stu-id="ff431-170">At a command prompt, change the directory to **azuread**:</span></span>

    `cd azuread`

2.  <span data-ttu-id="ff431-171">Zadejte následující příkazy.</span><span class="sxs-lookup"><span data-stu-id="ff431-171">Enter the following commands.</span></span> <span data-ttu-id="ff431-172">Příkazy instalace následující modulů ve vašem adresáři node_modules:</span><span class="sxs-lookup"><span data-stu-id="ff431-172">The commands install the following modules in your node_modules directory:</span></span>

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

## <a name="9-create-a-serverjs-file-for-your-dependencies"></a><span data-ttu-id="ff431-173">9: vytvoření souboru Server.js pro svoje závislosti</span><span class="sxs-lookup"><span data-stu-id="ff431-173">9: Create a Server.js file for your dependencies</span></span>
<span data-ttu-id="ff431-174">Soubor Server.js obsahuje většinu funkcí webového rozhraní API serveru.</span><span class="sxs-lookup"><span data-stu-id="ff431-174">A Server.js file holds the majority of the functionality for your web API server.</span></span> <span data-ttu-id="ff431-175">Do tohoto souboru přidáte většinu kódu.</span><span class="sxs-lookup"><span data-stu-id="ff431-175">Add most of your code to this file.</span></span> <span data-ttu-id="ff431-176">Pro produkční účely můžete můžete funkčnost rozdělit do menších souborů, jako je pro samostatné trasy a ovladače.</span><span class="sxs-lookup"><span data-stu-id="ff431-176">For production purposes, you can refactor the functionality into smaller files, like for separate routes and controllers.</span></span> <span data-ttu-id="ff431-177">V tomto článku používáme Server.js pro tento účel.</span><span class="sxs-lookup"><span data-stu-id="ff431-177">In this article, we use Server.js for this purpose.</span></span>

1.  <span data-ttu-id="ff431-178">Na příkazovém řádku změňte adresář na **azuread**:</span><span class="sxs-lookup"><span data-stu-id="ff431-178">At a command prompt, change the directory to **azuread**:</span></span>

    `cd azuread`

2.  <span data-ttu-id="ff431-179">Pomocí zvoleného editoru, vytvořte soubor Server.js.</span><span class="sxs-lookup"><span data-stu-id="ff431-179">Using an editor of your choice, create a Server.js file.</span></span> <span data-ttu-id="ff431-180">Do souboru přidejte následující informace:</span><span class="sxs-lookup"><span data-stu-id="ff431-180">Add the following information to the file:</span></span>

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

3.  <span data-ttu-id="ff431-181">Uložte soubor.</span><span class="sxs-lookup"><span data-stu-id="ff431-181">Save the file.</span></span> <span data-ttu-id="ff431-182">Vrátíte se k němu za chvíli.</span><span class="sxs-lookup"><span data-stu-id="ff431-182">You will return to it shortly.</span></span>

## <a name="10-create-a-config-file-to-store-your-azure-ad-settings"></a><span data-ttu-id="ff431-183">10: Vytvořte konfigurační soubor pro uložení nastavení Azure AD</span><span class="sxs-lookup"><span data-stu-id="ff431-183">10: Create a config file to store your Azure AD settings</span></span>
<span data-ttu-id="ff431-184">Tento soubor s kódem předává parametry konfigurace z portálu Azure AD Passport.js.</span><span class="sxs-lookup"><span data-stu-id="ff431-184">This code file passes the configuration parameters from your Azure AD portal to Passport.js.</span></span> <span data-ttu-id="ff431-185">Tyto hodnoty konfigurace jste vytvořili, když jste přidali webové rozhraní API na portálu na začátku článku.</span><span class="sxs-lookup"><span data-stu-id="ff431-185">You created these configuration values when you added the web API to the portal at the beginning of the article.</span></span> <span data-ttu-id="ff431-186">Po zkopírování kód vysvětlíme, co zadat jako hodnoty těchto parametrů.</span><span class="sxs-lookup"><span data-stu-id="ff431-186">After you copy the code, we'll explain what to put in the values of these parameters.</span></span>

1.  <span data-ttu-id="ff431-187">Na příkazovém řádku změňte adresář na **azuread**:</span><span class="sxs-lookup"><span data-stu-id="ff431-187">At a command prompt, change the directory to **azuread**:</span></span>

    `cd azuread`

2.  <span data-ttu-id="ff431-188">V editoru vytvoření souboru Config.js.</span><span class="sxs-lookup"><span data-stu-id="ff431-188">In an editor, create a Config.js file.</span></span> <span data-ttu-id="ff431-189">Přidejte následující informace:</span><span class="sxs-lookup"><span data-stu-id="ff431-189">Add the following information:</span></span>

    ```Javascript
    // Don't commit this file to your public repos. This config is for first-run.
    exports.creds = {
    mongoose_auth_local: 'mongodb://localhost/tasklist', // Your Mongo auth URI goes here.
    issuer: 'https://sts.windows.net/**<your application id>**/',
    audience: '<your redirect URI>',
    identityMetadata: 'https://login.microsoftonline.com/common/.well-known/openid-configuration' // For Microsoft, you should never need to change this.
    };

    ```



### <a name="required-values"></a><span data-ttu-id="ff431-190">Požadované hodnoty</span><span class="sxs-lookup"><span data-stu-id="ff431-190">Required values</span></span>

*   <span data-ttu-id="ff431-191">**IdentityMetadata**: to je, kdy `passport-azure-ad` vyhledá konfigurační data pro zprostředkovatele identity (IDP) a klíče k ověření webových tokenů JSON (Jwt).</span><span class="sxs-lookup"><span data-stu-id="ff431-191">**IdentityMetadata**: This is where `passport-azure-ad` looks for your configuration data for the identity provider (IDP) and the keys to validate the JSON Web Tokens (JWTs).</span></span> <span data-ttu-id="ff431-192">Pokud používáte Azure AD, pravděpodobně nechcete toto nastavení změnit.</span><span class="sxs-lookup"><span data-stu-id="ff431-192">If you are using Azure AD, you probably don't want to change this.</span></span>

*   <span data-ttu-id="ff431-193">**Cílová skupina**: váš identifikátor URI přesměrování z portálu.</span><span class="sxs-lookup"><span data-stu-id="ff431-193">**audience**: Your redirect URI from the portal.</span></span>

> [!NOTE]
> <span data-ttu-id="ff431-194">Vrátit klíče v pravidelných intervalech.</span><span class="sxs-lookup"><span data-stu-id="ff431-194">Roll your keys at frequent intervals.</span></span> <span data-ttu-id="ff431-195">Ujistěte se, že jste vždy stáhněte z adresy URL pro "openid_keys" a aplikaci můžete získat přístup k Internetu.</span><span class="sxs-lookup"><span data-stu-id="ff431-195">Be sure that you always pull from the "openid_keys" URL, and that the app can access the Internet.</span></span>
> 
> 

## <a name="11-add-the-configuration-to-your-serverjs-file"></a><span data-ttu-id="ff431-196">11: přidejte konfiguraci do souboru Server.js</span><span class="sxs-lookup"><span data-stu-id="ff431-196">11: Add the configuration to your Server.js file</span></span>
<span data-ttu-id="ff431-197">Aplikace musí čtení hodnoty z konfigurační soubor, který jste právě vytvořili.</span><span class="sxs-lookup"><span data-stu-id="ff431-197">Your application needs to read the values from the config file you just created.</span></span> <span data-ttu-id="ff431-198">Přidejte soubor .config jako požadovaný prostředek vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="ff431-198">Add the .config file as a required resource in your application.</span></span> <span data-ttu-id="ff431-199">Nastavte globální proměnné na ty, které jsou v souboru Config.js.</span><span class="sxs-lookup"><span data-stu-id="ff431-199">Set the global variables to those that are in Config.js.</span></span>

1.  <span data-ttu-id="ff431-200">Na příkazovém řádku změňte adresář na **azuread**:</span><span class="sxs-lookup"><span data-stu-id="ff431-200">At the command prompt, change the directory to **azuread**:</span></span>

    `cd azuread`

2.  <span data-ttu-id="ff431-201">V editoru otevřete Server.js.</span><span class="sxs-lookup"><span data-stu-id="ff431-201">In an editor, open Server.js.</span></span> <span data-ttu-id="ff431-202">Přidejte následující informace:</span><span class="sxs-lookup"><span data-stu-id="ff431-202">Add the following information:</span></span>

    ```Javascript
    var config = require('./config');
    ```

3.  <span data-ttu-id="ff431-203">Přidejte novou část Server.js:</span><span class="sxs-lookup"><span data-stu-id="ff431-203">Add a new section to Server.js:</span></span>

    ```Javascript
    // Pass these options in to the ODICBearerStrategy.
    var options = {
    // The URL of the metadata document for your app. Put the keys for token validation from the URL found in the jwks_uri tag in the metadata.
    identityMetadata: config.creds.identityMetadata,
    issuer: config.creds.issuer,
    audience: config.creds.audience
    };
    // Array to hold signed-in users and the current signed-in user (owner).
    var users = [];
    var owner = null;
    // Your logger
    var log = bunyan.createLogger({
    name: 'Microsoft Azure Active Directory Sample'
    });
    ```

## <a name="12-add-the-mongodb-model-and-schema-information-by-using-mongoose"></a><span data-ttu-id="ff431-204">12: přidejte informace modelu a schématu MongoDB pomocí Mongoose</span><span class="sxs-lookup"><span data-stu-id="ff431-204">12: Add the MongoDB model and schema information by using Mongoose</span></span>
<span data-ttu-id="ff431-205">V dalším kroku připojte tyto tři soubory ve službě REST API.</span><span class="sxs-lookup"><span data-stu-id="ff431-205">Next, connect these three files in a REST API service.</span></span>

<span data-ttu-id="ff431-206">V tomto článku používáme k uložení naše úlohy MongoDB.</span><span class="sxs-lookup"><span data-stu-id="ff431-206">In this article, we use MongoDB to store our tasks.</span></span> <span data-ttu-id="ff431-207">To probereme *krok 4*.</span><span class="sxs-lookup"><span data-stu-id="ff431-207">We discuss this in *step 4*.</span></span>

<span data-ttu-id="ff431-208">V souboru Config.js jste vytvořili v kroku 11, se nazývá databáze *tasklist*.</span><span class="sxs-lookup"><span data-stu-id="ff431-208">In the Config.js file you created in step 11, your database is called *tasklist*.</span></span> <span data-ttu-id="ff431-209">Který byl uveďte na konci adresy URL mongoose_auth_local připojení.</span><span class="sxs-lookup"><span data-stu-id="ff431-209">That was what you put at the end of your mongoose_auth_local connection URL.</span></span> <span data-ttu-id="ff431-210">Tuto databázi nemusíte předem vytvářet v MongoDB.</span><span class="sxs-lookup"><span data-stu-id="ff431-210">You don't need to create this database beforehand in MongoDB.</span></span> <span data-ttu-id="ff431-211">Databáze se vytvoří během prvního spuštění vaší serverové aplikace (za předpokladu, že databáze ještě neexistuje).</span><span class="sxs-lookup"><span data-stu-id="ff431-211">The database is created on the first run of your server application (assuming the database does not already exist).</span></span>

<span data-ttu-id="ff431-212">Jste sdělili serveru, jaké databázi MongoDB má použít.</span><span class="sxs-lookup"><span data-stu-id="ff431-212">You've told the server what MongoDB database to use.</span></span> <span data-ttu-id="ff431-213">Potom budete muset napsat další kód pro vytvoření modelu a schématu pro váš server úloh.</span><span class="sxs-lookup"><span data-stu-id="ff431-213">Next, you need to write some additional code to create the model and schema for your server's tasks.</span></span>

### <a name="the-model"></a><span data-ttu-id="ff431-214">Model</span><span class="sxs-lookup"><span data-stu-id="ff431-214">The model</span></span>
<span data-ttu-id="ff431-215">Model schématu je velmi jednoduché.</span><span class="sxs-lookup"><span data-stu-id="ff431-215">The schema model is very basic.</span></span> <span data-ttu-id="ff431-216">Ho můžete rozšířit, pokud je potřeba.</span><span class="sxs-lookup"><span data-stu-id="ff431-216">You can expand it if you need to.</span></span> 

<span data-ttu-id="ff431-217">Schéma modelu má tyto hodnoty:</span><span class="sxs-lookup"><span data-stu-id="ff431-217">The schema model has these values:</span></span>

*   <span data-ttu-id="ff431-218">**NÁZEV**.</span><span class="sxs-lookup"><span data-stu-id="ff431-218">**NAME**.</span></span> <span data-ttu-id="ff431-219">Osoba, která úlohu.</span><span class="sxs-lookup"><span data-stu-id="ff431-219">The person assigned to the task.</span></span> <span data-ttu-id="ff431-220">Toto je **řetězec** hodnotu.</span><span class="sxs-lookup"><span data-stu-id="ff431-220">This is a **string** value.</span></span>
*   <span data-ttu-id="ff431-221">**ÚLOHA**.</span><span class="sxs-lookup"><span data-stu-id="ff431-221">**TASK**.</span></span> <span data-ttu-id="ff431-222">Název úlohy.</span><span class="sxs-lookup"><span data-stu-id="ff431-222">The name of the task.</span></span> <span data-ttu-id="ff431-223">Toto je **řetězec** hodnotu.</span><span class="sxs-lookup"><span data-stu-id="ff431-223">This is a **string** value.</span></span>
*   <span data-ttu-id="ff431-224">**DATUM**.</span><span class="sxs-lookup"><span data-stu-id="ff431-224">**DATE**.</span></span> <span data-ttu-id="ff431-225">Datum, tato úloha je kvůli.</span><span class="sxs-lookup"><span data-stu-id="ff431-225">The date that the task is due.</span></span> <span data-ttu-id="ff431-226">Toto je **data a času** hodnotu.</span><span class="sxs-lookup"><span data-stu-id="ff431-226">This is a **datetime** value.</span></span>
*   <span data-ttu-id="ff431-227">**DOKONČIT**.</span><span class="sxs-lookup"><span data-stu-id="ff431-227">**COMPLETED**.</span></span> <span data-ttu-id="ff431-228">Zda je úloha dokončena.</span><span class="sxs-lookup"><span data-stu-id="ff431-228">Whether the task is completed.</span></span> <span data-ttu-id="ff431-229">Toto je **Boolean** hodnotu.</span><span class="sxs-lookup"><span data-stu-id="ff431-229">This is a **Boolean** value.</span></span>

### <a name="create-the-schema-in-the-code"></a><span data-ttu-id="ff431-230">Vytvoření schématu v kódu</span><span class="sxs-lookup"><span data-stu-id="ff431-230">Create the schema in the code</span></span>
1.  <span data-ttu-id="ff431-231">Na příkazovém řádku změňte adresář na **azuread**:</span><span class="sxs-lookup"><span data-stu-id="ff431-231">At a command prompt, change the directory to **azuread**:</span></span>

    `cd azuread`

2.  <span data-ttu-id="ff431-232">V editoru otevřete Server.js.</span><span class="sxs-lookup"><span data-stu-id="ff431-232">In your editor, open Server.js.</span></span> <span data-ttu-id="ff431-233">Pod položku konfigurace přidejte následující informace:</span><span class="sxs-lookup"><span data-stu-id="ff431-233">Below the configuration entry, add the following information:</span></span>

    ```Javascript
    // MongoDB setup.
    // Set up some configuration.
    var serverPort = process.env.PORT || 8080;
    var serverURI = (process.env.PORT) ? config.creds.mongoose_auth_mongohq : config.creds.mongoose_auth_local;
    // Connect to MongoDB.
    global.db = mongoose.connect(serverURI);
    var Schema = mongoose.Schema;
    log.info('MongoDB Schema loaded');
    ```

<span data-ttu-id="ff431-234">Tento kód se připojí k serveru MongoDB.</span><span class="sxs-lookup"><span data-stu-id="ff431-234">This code connects to the MongoDB server.</span></span> <span data-ttu-id="ff431-235">Vrátí také objekt schématu.</span><span class="sxs-lookup"><span data-stu-id="ff431-235">It also returns a schema object.</span></span>

#### <a name="using-the-schema-create-your-model-in-the-code"></a><span data-ttu-id="ff431-236">Pomocí schématu, vytvoření modelu v kódu</span><span class="sxs-lookup"><span data-stu-id="ff431-236">Using the schema, create your model in the code</span></span>
<span data-ttu-id="ff431-237">Pod předchozí kód přidejte následující kód:</span><span class="sxs-lookup"><span data-stu-id="ff431-237">Below the preceding code, add the following code:</span></span>

```Javascript
// Create a basic schema to store your tasks and users.
var TaskSchema = new Schema({
owner: String,
task: String,
completed: Boolean,
date: Date
});
// Use the schema to register a model.
mongoose.model('Task', TaskSchema);
var Task = mongoose.model('Task');
```

<span data-ttu-id="ff431-238">Jak se dá zjistit z kódu, nejprve vytvoříte schéma.</span><span class="sxs-lookup"><span data-stu-id="ff431-238">As you can tell from the code, first you create your schema.</span></span> <span data-ttu-id="ff431-239">V dalším kroku vytvoříte objekt modelu.</span><span class="sxs-lookup"><span data-stu-id="ff431-239">Next, you create a model object.</span></span> <span data-ttu-id="ff431-240">Použít objekt modelu k ukládání dat napříč kódem, když definujete vaše **trasy**.</span><span class="sxs-lookup"><span data-stu-id="ff431-240">You use the model object to store your data throughout the code when you define your **routes**.</span></span>

## <a name="13-add-your-routes-for-your-task-rest-api-server"></a><span data-ttu-id="ff431-241">13: Přidat trasy pro serveru úloh REST API</span><span class="sxs-lookup"><span data-stu-id="ff431-241">13: Add your routes for your task REST API server</span></span>
<span data-ttu-id="ff431-242">Teď, když máte model databáze můžete pracovat, přidejte trasy, které budete používat pro svůj server REST API.</span><span class="sxs-lookup"><span data-stu-id="ff431-242">Now that you have a database model to work with, add the routes you'll use for your REST API server.</span></span>

### <a name="about-routes-in-restify"></a><span data-ttu-id="ff431-243">O trasách v restify</span><span class="sxs-lookup"><span data-stu-id="ff431-243">About routes in restify</span></span>
<span data-ttu-id="ff431-244">Trasy v restify pracovní úplně stejně, jako při použití balíku Express.</span><span class="sxs-lookup"><span data-stu-id="ff431-244">Routes in restify work exactly the same way they do when you use the Express stack.</span></span> <span data-ttu-id="ff431-245">Trasy se definují pomocí identifikátoru URI, který by měly volat klientské aplikace. </span><span class="sxs-lookup"><span data-stu-id="ff431-245">You define routes by using the URI that you expect the client applications to call.</span></span> <span data-ttu-id="ff431-246">Trasy se obvykle definovat v samostatném souboru.</span><span class="sxs-lookup"><span data-stu-id="ff431-246">Usually, you define your routes in a separate file.</span></span> <span data-ttu-id="ff431-247">V tomto kurzu jsme uveďte naše trasy v Server.js.</span><span class="sxs-lookup"><span data-stu-id="ff431-247">In this tutorial, we put our routes in Server.js.</span></span> <span data-ttu-id="ff431-248">Pro použití v provozním prostředí doporučujeme, abyste je do svých vlastních souboru zvážit trasy.</span><span class="sxs-lookup"><span data-stu-id="ff431-248">For production use, we recommend that you factor routes into their own file.</span></span>

<span data-ttu-id="ff431-249">Typický vzor trasy restify je:</span><span class="sxs-lookup"><span data-stu-id="ff431-249">A typical pattern for a restify route is:</span></span>

```Javascript
function createObject(req, res, next) {
// Do work on object.
_object.name = req.params.object; // Passed value is in req.params under object.
///...
return next(); // Keep the server going.
}
....
server.post('/service/:add/:object', createObject); // calls createObject on routes that match this.
```


<span data-ttu-id="ff431-250">Toto je vzor na nejzákladnější úrovni.</span><span class="sxs-lookup"><span data-stu-id="ff431-250">This is the pattern at the most basic level.</span></span> <span data-ttu-id="ff431-251">Restify (a Express) poskytují mnohem hlubší funkčnost, jako je schopnost definovat typy aplikací a komplexního trasování napříč různými koncovými body.</span><span class="sxs-lookup"><span data-stu-id="ff431-251">Restify (and Express) provide much deeper functionality, like the ability to define application types, and complex routing across different endpoints.</span></span>

#### <a name="add-default-routes-to-your-server"></a><span data-ttu-id="ff431-252">Přidání výchozích tras na server</span><span class="sxs-lookup"><span data-stu-id="ff431-252">Add default routes to your server</span></span>
<span data-ttu-id="ff431-253">Přidat do základní trasy CRUD: **vytvořit**, **načíst**, **aktualizace**, a **odstranit**.</span><span class="sxs-lookup"><span data-stu-id="ff431-253">Add the basic CRUD routes: **create**, **retrieve**, **update**, and **delete**.</span></span>

1.  <span data-ttu-id="ff431-254">Na příkazovém řádku změňte adresář na **azuread**:</span><span class="sxs-lookup"><span data-stu-id="ff431-254">At a command prompt, change the directory to **azuread**:</span></span>

    `cd azuread`

2.  <span data-ttu-id="ff431-255">V editoru otevřete Server.js.</span><span class="sxs-lookup"><span data-stu-id="ff431-255">In an editor, open Server.js.</span></span> <span data-ttu-id="ff431-256">Níže položky databáze, které jste provedli dříve, přidejte následující informace:</span><span class="sxs-lookup"><span data-stu-id="ff431-256">Below the database entries you made earlier, add the following information:</span></span>

    ```Javascript
    /**
    *
    * APIs for your REST task server
    */
    // Create a task.
    function createTask(req, res, next) {
    // Resitify currently has a bug that doesn't allow you to set default headers.
    // These headers comply with CORS, and allow you to use MongoDB Server as your response to any origin.
    res.header("Access-Control-Allow-Origin", "*");
    res.header("Access-Control-Allow-Headers", "X-Requested-With");
    // Create a new task model, fill it, and save it to MongoDB.
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
    req.log.warn(err, 'createTask: unable to save');
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
    'removeTask: unable to delete %s',
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
    req.log.warn(err, 'get: unable to read %s', owner);
    next(err);
    return;
    }
    res.json(data);
    });
    return next();
    }
    /// Returns the list of TODOs that were loaded.
    function listTasks(req, res, next) {
    // Resitify currently has a bug that doesn't allow you to set default headers.
    // These headers comply with CORS, and allow us to use MongoDB Server as our response to any origin.
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
    log.warn(err, "There are no tasks in the database. Add one!");
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

### <a name="add-error-handling-for-the-routes"></a><span data-ttu-id="ff431-257">Přidání zpracování chyb pro trasy</span><span class="sxs-lookup"><span data-stu-id="ff431-257">Add error handling for the routes</span></span>
<span data-ttu-id="ff431-258">Přidáte nějaké zpracování chyb pro komunikaci zpět do klienta o problému, který jste se setkali.</span><span class="sxs-lookup"><span data-stu-id="ff431-258">Add some error handling so you can communicate back to the client about the problem you encountered.</span></span>

<span data-ttu-id="ff431-259">Přidejte následující kód pod kód, který jste již zapsány:</span><span class="sxs-lookup"><span data-stu-id="ff431-259">Add the following code below the code, which you've already written:</span></span>

```Javascript
///--- Errors for communicating something more information back to the client.
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


## <a name="14-create-your-server"></a><span data-ttu-id="ff431-260">14: vytvoření serveru</span><span class="sxs-lookup"><span data-stu-id="ff431-260">14: Create your server</span></span>
<span data-ttu-id="ff431-261">Poslední věcí, kterou chcete je přidání vaší instance serveru.</span><span class="sxs-lookup"><span data-stu-id="ff431-261">The last thing to do is to add your server instance.</span></span> <span data-ttu-id="ff431-262">Instance serveru spravuje vaše volání.</span><span class="sxs-lookup"><span data-stu-id="ff431-262">The server instance manages your calls.</span></span>

<span data-ttu-id="ff431-263">Restify (a Express) mají přímý přizpůsobení, které můžete použít s server REST API.</span><span class="sxs-lookup"><span data-stu-id="ff431-263">Restify (and Express) have deep customization that you can use with a REST API server.</span></span> <span data-ttu-id="ff431-264">V tomto kurzu používáme nejzákladnější nastavení.</span><span class="sxs-lookup"><span data-stu-id="ff431-264">In this tutorial, we use the most basic setup.</span></span>

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
// Allow 5 requests/second by IP address, and burst to 10.
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
## <a name="15-add-the-routes-without-authentication-for-now"></a><span data-ttu-id="ff431-265">15: přidání tras (bez ověřování, teď)</span><span class="sxs-lookup"><span data-stu-id="ff431-265">15: Add the routes (without authentication, for now)</span></span>
```Javascript
/// Use CRUD to add the real handlers.
/**
/*
/* Each of these handlers is protected by your Open ID Connect Bearer strategy. Invoke 'oidc-bearer'
/* in the pasport.authenticate() method. Because REST is stateless, set 'session: false'. You 
/* don't need to maintain session state. You can experiment with removing API protection.
/* To do this, remove the passport.authenticate() method:
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
consoleMessage += '\n Open your browser to %s/tasks\n';
consoleMessage += '+++++++++++++++++++++++++++++++++++++++++++++++++++++ \n';
consoleMessage += '\n !!! why not try a $curl -isS %s | json to get some ideas? \n';
consoleMessage += '+++++++++++++++++++++++++++++++++++++++++++++++++++++ \n\n';
});
```
## <a name="16-run-the-server"></a><span data-ttu-id="ff431-266">16: Spusťte serveru</span><span class="sxs-lookup"><span data-stu-id="ff431-266">16: Run the server</span></span>
<span data-ttu-id="ff431-267">Je vhodné před přidáním ověřování otestovat svůj server.</span><span class="sxs-lookup"><span data-stu-id="ff431-267">It's a good idea to test your server before you add authentication.</span></span>

<span data-ttu-id="ff431-268">Nejjednodušší způsob, jak otestovat svůj server je pomocí curl na příkazovém řádku.</span><span class="sxs-lookup"><span data-stu-id="ff431-268">The easiest way to test your server is by using curl at a command prompt.</span></span> <span data-ttu-id="ff431-269">Chcete-li to provést, musíte jednoduchý nástroj, který můžete použít k analýze výstup jako JSON.</span><span class="sxs-lookup"><span data-stu-id="ff431-269">To do this, you need a simple utility that you can use to parse output as JSON.</span></span> 

1.  <span data-ttu-id="ff431-270">Nainstalujte nástroj JSON, který používáme v následujících příkladech:</span><span class="sxs-lookup"><span data-stu-id="ff431-270">Install the JSON tool that we use in the following examples:</span></span>

    `$npm install -g jsontool`

    <span data-ttu-id="ff431-271">Tím se globálně nainstaluje nástroj JSON.</span><span class="sxs-lookup"><span data-stu-id="ff431-271">This installs the JSON tool globally.</span></span>

2.  <span data-ttu-id="ff431-272">Ujistěte se, že je spuštěna instance MongoDB.</span><span class="sxs-lookup"><span data-stu-id="ff431-272">Make sure that your MongoDB instance is running:</span></span>

    `$sudo mongod`

3.  <span data-ttu-id="ff431-273">Změňte adresář na **azuread**, a poté spusťte curl:</span><span class="sxs-lookup"><span data-stu-id="ff431-273">Change the directory to **azuread**, and then run curl:</span></span>

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

4.  <span data-ttu-id="ff431-274">Chcete-li přidat úloha:</span><span class="sxs-lookup"><span data-stu-id="ff431-274">To add a task:</span></span>

    `$ curl -isS -X POST http://127.0.0.1:8888/tasks/brandon/Hello`

    <span data-ttu-id="ff431-275">Odpověď by měla být:</span><span class="sxs-lookup"><span data-stu-id="ff431-275">The response should be:</span></span>

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

5.  <span data-ttu-id="ff431-276">Seznam úloh pro Brandon:</span><span class="sxs-lookup"><span data-stu-id="ff431-276">List tasks for Brandon:</span></span>

    `$ curl -isS http://127.0.0.1:8080/tasks/brandon/`

<span data-ttu-id="ff431-277">Pokud tyto příkazy spustí bez chyb, jste připraveni pro přidání OAuth na server REST API.</span><span class="sxs-lookup"><span data-stu-id="ff431-277">If all these commands run without errors, you are ready to add OAuth to the REST API server.</span></span>

<span data-ttu-id="ff431-278">*Nyní máte server REST API s MongoDB!*</span><span class="sxs-lookup"><span data-stu-id="ff431-278">*You now have a REST API server with MongoDB!*</span></span>

## <a name="17-add-authentication-to-your-rest-api-server"></a><span data-ttu-id="ff431-279">17: přidání ověřování na server REST API</span><span class="sxs-lookup"><span data-stu-id="ff431-279">17: Add authentication to your REST API server</span></span>
<span data-ttu-id="ff431-280">Teď, když máte spuštěný rozhraní REST API, nastavte tak, aby jeho použití s Azure AD.</span><span class="sxs-lookup"><span data-stu-id="ff431-280">Now that you have a running REST API, set it up to use it with Azure AD.</span></span>

<span data-ttu-id="ff431-281">Na příkazovém řádku změňte adresář na **azuread**:</span><span class="sxs-lookup"><span data-stu-id="ff431-281">At a command prompt, change the directory to **azuread**:</span></span>

`cd azuread`

### <a name="use-the-oidcbearerstrategy-thats-included-with-passport-azure-ad"></a><span data-ttu-id="ff431-282">Použití oidcbearerstrategy, která je součástí passport-azure-ad</span><span class="sxs-lookup"><span data-stu-id="ff431-282">Use the oidcbearerstrategy that's included with passport-azure-ad</span></span>
<span data-ttu-id="ff431-283">Zatím jste vytvořili typický server REST se seznamem úkolů bez jakéhokoli druhu ověřování.</span><span class="sxs-lookup"><span data-stu-id="ff431-283">So far, you've built a typical REST TODO server without any kind of authorization.</span></span> <span data-ttu-id="ff431-284">Nyní přidáte ověřování.</span><span class="sxs-lookup"><span data-stu-id="ff431-284">Now, add authentication.</span></span>

<span data-ttu-id="ff431-285">Nejprve znamenat, že chcete použít Passport.</span><span class="sxs-lookup"><span data-stu-id="ff431-285">First,  indicate that you want to use Passport.</span></span> <span data-ttu-id="ff431-286">Po konfiguraci serveru starší PUT tato práva:</span><span class="sxs-lookup"><span data-stu-id="ff431-286">Put this right after your earlier server configuration:</span></span>

```Javascript
// Start using Passport.js.

server.use(passport.initialize()); // Starts passport
server.use(passport.session()); // Provides session support
```

> [!TIP]
> <span data-ttu-id="ff431-287">Při psaní rozhraní API, je vhodné vždy měli propojit data s něčím jedinečným z tokenu, který uživatel nemůže zfalšovat.</span><span class="sxs-lookup"><span data-stu-id="ff431-287">When you write APIs, it's a good idea to always link the data to something unique from the token that the user can’t spoof.</span></span> <span data-ttu-id="ff431-288">Pokud tento server ukládá položky úkolů, uloží je na základě předplatného ID uživatele v tokenu (zavolaném prostřednictvím token.sub).</span><span class="sxs-lookup"><span data-stu-id="ff431-288">When this server stores TODO items, it stores them based on the user subscription ID in the token (called through token.sub).</span></span> <span data-ttu-id="ff431-289">Do pole "vlastník" Vložit token.sub.</span><span class="sxs-lookup"><span data-stu-id="ff431-289">You put the token.sub in the “owner” field.</span></span> <span data-ttu-id="ff431-290">To zajišťuje, že pouze tento uživatel přístup TODOs uživatele.</span><span class="sxs-lookup"><span data-stu-id="ff431-290">This ensures that only that user can access the user's TODOs.</span></span> <span data-ttu-id="ff431-291">Nikdo jiný přístup k TODOs, které jste zadali.</span><span class="sxs-lookup"><span data-stu-id="ff431-291">No one else can access the TODOs that were entered.</span></span> <span data-ttu-id="ff431-292">Neexistuje vystavení v rozhraní API pro "vlastník".</span><span class="sxs-lookup"><span data-stu-id="ff431-292">There is no exposure in the API for “owner.”</span></span> <span data-ttu-id="ff431-293">Externí uživatel může požádat o TODOs jiní uživatelé, i když jsou ověřeni.</span><span class="sxs-lookup"><span data-stu-id="ff431-293">An external user can request other users' TODOs, even if they are authenticated.</span></span>
> 
> 

<span data-ttu-id="ff431-294">Pak pomocí Open ID Connect nosnou strategii, která se dodává s `passport-azure-ad`.</span><span class="sxs-lookup"><span data-stu-id="ff431-294">Next, use the Open ID Connect Bearer strategy that comes with `passport-azure-ad`.</span></span> <span data-ttu-id="ff431-295">To uvedli po jste vložili dříve:</span><span class="sxs-lookup"><span data-stu-id="ff431-295">Put this after what you pasted earlier:</span></span>

```Javascript
/**
/*
/* Calling the OIDCBearerStrategy and managing users.
/*
/* Because of the Passport pattern, you need to manage users and info tokens
/* with a FindorCreate() method. The method must be provided by the implementor.
/* In the following code, you autoregister any user and implement a FindById().
/* It's a good idea to do something more advanced.
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
log.info('verifying the user');
log.info(token, 'was the token retrieved');
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

<span data-ttu-id="ff431-296">Passport používá podobný Princip pro všechny svoje strategie (Twitteru, Facebooku a tak dále).</span><span class="sxs-lookup"><span data-stu-id="ff431-296">Passport uses a similar pattern for all its strategies (Twitter, Facebook, and so on).</span></span> <span data-ttu-id="ff431-297">Řídí všichni autoři strategií se vzorem.</span><span class="sxs-lookup"><span data-stu-id="ff431-297">All strategy writers adhere to the pattern.</span></span> <span data-ttu-id="ff431-298">Předat strategie `function()` token, který používá a `done` jako parametry.</span><span class="sxs-lookup"><span data-stu-id="ff431-298">Pass the strategy a `function()` that uses a token and `done` as parameters.</span></span> <span data-ttu-id="ff431-299">Strategie se vrátí po dělá svou práci.</span><span class="sxs-lookup"><span data-stu-id="ff431-299">The strategy is returned after it does all its work.</span></span> <span data-ttu-id="ff431-300">Uložení uživatele a skrytí tokenu, takže je nebudete muset požadovat znovu.</span><span class="sxs-lookup"><span data-stu-id="ff431-300">Store the user and stash the token so you don’t need to ask for it again.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="ff431-301">Předchozí kód přijme všechny uživatele, který může ověřit na váš server.</span><span class="sxs-lookup"><span data-stu-id="ff431-301">The preceding code takes any user that can authenticate to your server.</span></span> <span data-ttu-id="ff431-302">To se označuje jako Automatická registrace.</span><span class="sxs-lookup"><span data-stu-id="ff431-302">This is known as auto-registration.</span></span> <span data-ttu-id="ff431-303">Na provozním serveru byste neměli chtít vpouštět každý, kdo bez toho, aby předtím prošli registračním procesem, který zvolíte.</span><span class="sxs-lookup"><span data-stu-id="ff431-303">On a production server, you wouldn’t want to let anyone in without first having them go through a registration process that you choose.</span></span> <span data-ttu-id="ff431-304">To je obvykle vzor, který můžete vidět u uživatelských aplikací.</span><span class="sxs-lookup"><span data-stu-id="ff431-304">This is usually the pattern you see in consumer apps.</span></span> <span data-ttu-id="ff431-305">Aplikace může umožňují registraci pomocí Facebooku, ale následně požádá, můžete k zadání dalších informací.</span><span class="sxs-lookup"><span data-stu-id="ff431-305">The app might allow you to register with Facebook, but then it asks you to enter additional information.</span></span> <span data-ttu-id="ff431-306">Pokud v tomto kurzu nebyly pomocí příkazového řádku programu, může extrahovat e-mail z vráceného objektu tokenu.</span><span class="sxs-lookup"><span data-stu-id="ff431-306">If you weren't using a command-line program for this tutorial, you could extract the email from the token object that is returned.</span></span> <span data-ttu-id="ff431-307">Potom může požádat uživatele k zadání dalších informací.</span><span class="sxs-lookup"><span data-stu-id="ff431-307">Then, you might ask the user to enter additional information.</span></span> <span data-ttu-id="ff431-308">Protože se jedná o testovací server, je třeba přidat uživatele přímo k databázi v paměti.</span><span class="sxs-lookup"><span data-stu-id="ff431-308">Because this is a test server, you add the user directly to the in-memory database.</span></span>
> 
> 

### <a name="protect-endpoints"></a><span data-ttu-id="ff431-309">Ochrana koncových bodů</span><span class="sxs-lookup"><span data-stu-id="ff431-309">Protect endpoints</span></span>
<span data-ttu-id="ff431-310">Ochrana koncových bodů tak, že zadáte **passport.authenticate()** volání s protokol, který chcete použít.</span><span class="sxs-lookup"><span data-stu-id="ff431-310">Protect endpoints by specifying the **passport.authenticate()** call with the protocol that you want to use.</span></span>

<span data-ttu-id="ff431-311">Můžete upravit trasu v kódu serveru pro pokročilejší možnosti:</span><span class="sxs-lookup"><span data-stu-id="ff431-311">You can edit your route in your server code for a more advanced option:</span></span>

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

## <a name="18-run-your-server-application-again"></a><span data-ttu-id="ff431-312">18: Spusťte serverovou aplikaci znovu</span><span class="sxs-lookup"><span data-stu-id="ff431-312">18: Run your server application again</span></span>
<span data-ttu-id="ff431-313">Znovu použijte curl a zkontrolujte, jestli máte OAuth 2.0 ochrany koncové body.</span><span class="sxs-lookup"><span data-stu-id="ff431-313">Use curl again to see if you have OAuth 2.0 protection against your endpoints.</span></span> <span data-ttu-id="ff431-314">To udělejte předtím, než spustíte všechny klientské sady SDK proti tomuto koncovému bodu.</span><span class="sxs-lookup"><span data-stu-id="ff431-314">Do this before you run any of your client SDKs against this endpoint.</span></span> <span data-ttu-id="ff431-315">Vrátí hlavičky by mělo být uvedeno, zda ověřování pracuje správně.</span><span class="sxs-lookup"><span data-stu-id="ff431-315">The headers returned should tell you if your authentication is working correctly.</span></span>

1.  <span data-ttu-id="ff431-316">Ujistěte se, zda je spuštěna vaše isntance MongoDB:</span><span class="sxs-lookup"><span data-stu-id="ff431-316">Make sure that your MongoDB isntance is running:</span></span>

    `$sudo mongod`

2.  <span data-ttu-id="ff431-317">Změnit na **azuread** adresář a potom pomocí curl:</span><span class="sxs-lookup"><span data-stu-id="ff431-317">Change to the **azuread** directory, and then use curl:</span></span>

    `$ cd azuread`

    `$ node server.js`

3.  <span data-ttu-id="ff431-318">Zkuste základní požadavek POST:</span><span class="sxs-lookup"><span data-stu-id="ff431-318">Try a basic POST:</span></span>

    `$ curl -isS -X POST http://127.0.0.1:8080/tasks/brandon/Hello`

    ```Shell
    HTTP/1.1 401 Unauthorized
    Connection: close
    WWW-Authenticate: Bearer realm="Users"
    Date: Tue, 14 Jul 2015 05:45:03 GMT
    Transfer-Encoding: chunked
    ```

<span data-ttu-id="ff431-319">401 odpovědi vyplývá, že se vrstva Passportu pokouší přesměrovat na koncový bod authorize, který je právě co chcete použít.</span><span class="sxs-lookup"><span data-stu-id="ff431-319">A 401 response indicates that the Passport layer is trying to redirect to the authorize endpoint, which is exactly what you want.</span></span>

<span data-ttu-id="ff431-320">*Nyní máte službu REST API, která používá OAuth 2.0!*</span><span class="sxs-lookup"><span data-stu-id="ff431-320">*You now have a REST API service that uses OAuth 2.0!*</span></span>

<span data-ttu-id="ff431-321">Jste došli nejdál, co můžete bez použití klienta OAuth 2.0 kompatibilní s tímto serverem.</span><span class="sxs-lookup"><span data-stu-id="ff431-321">You've gone as far as you can with this server without using an OAuth 2.0-compatible client.</span></span> <span data-ttu-id="ff431-322">Pro tento musíte zkontrolovat další kurzu.</span><span class="sxs-lookup"><span data-stu-id="ff431-322">For that, you will need to review an additional tutorial.</span></span>

## <a name="next-steps"></a><span data-ttu-id="ff431-323">Další kroky</span><span class="sxs-lookup"><span data-stu-id="ff431-323">Next steps</span></span>
<span data-ttu-id="ff431-324">Pro srovnání je hotová ukázka (bez vašich hodnot nastavení) k dispozici jako [soubor .zip](https://github.com/AzureADQuickStarts/AppModelv2-WebAPI-nodejs/archive/complete.zip).</span><span class="sxs-lookup"><span data-stu-id="ff431-324">For reference, the completed sample (without your configuration values) is provided as [a .zip file](https://github.com/AzureADQuickStarts/AppModelv2-WebAPI-nodejs/archive/complete.zip).</span></span> <span data-ttu-id="ff431-325">Vám může ho také klonovat z Githubu:</span><span class="sxs-lookup"><span data-stu-id="ff431-325">You also can clone it from GitHub:</span></span>

```git clone --branch complete https://github.com/AzureADQuickStarts/AppModelv2-WebAPI-nodejs.git```

<span data-ttu-id="ff431-326">Teď můžete přesunout pokročilejší témata.</span><span class="sxs-lookup"><span data-stu-id="ff431-326">Now, you can move on to more advanced topics.</span></span> <span data-ttu-id="ff431-327">Můžete chtít zkuste [zabezpečení webové aplikace Node.js pomocí koncového bodu v2.0](active-directory-v2-devquickstarts-node-web.md).</span><span class="sxs-lookup"><span data-stu-id="ff431-327">You might want to try [Secure a Node.js web app using the v2.0 endpoint](active-directory-v2-devquickstarts-node-web.md).</span></span>

<span data-ttu-id="ff431-328">Zde jsou některé další zdroje informací:</span><span class="sxs-lookup"><span data-stu-id="ff431-328">Here are some additional resources:</span></span>

* [<span data-ttu-id="ff431-329">Příručka vývojáře v2.0 služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="ff431-329">Azure AD v2.0 developer guide</span></span>](active-directory-appmodel-v2-overview.md)
* [<span data-ttu-id="ff431-330">Značka "azure-active-directory" přetečení zásobníku</span><span class="sxs-lookup"><span data-stu-id="ff431-330">Stack Overflow "azure-active-directory" tag</span></span>](http://stackoverflow.com/questions/tagged/azure-active-directory)

### <a name="get-security-updates-for-our-products"></a><span data-ttu-id="ff431-331">Získejte bezpečnostní aktualizace našich produktů</span><span class="sxs-lookup"><span data-stu-id="ff431-331">Get security updates for our products</span></span>
<span data-ttu-id="ff431-332">Doporučujeme registrace oznámení o bezpečnostních incidentech.</span><span class="sxs-lookup"><span data-stu-id="ff431-332">We encourage you to sign up to be notified when security incidents occur.</span></span> <span data-ttu-id="ff431-333">Na [technické oznámení zabezpečení Microsoft](https://technet.microsoft.com/security/dd252948) stránky, odběr výstrah zpravodaje zabezpečení.</span><span class="sxs-lookup"><span data-stu-id="ff431-333">On the [Microsoft Technical Security Notifications](https://technet.microsoft.com/security/dd252948) page, subscribe to Security Advisories Alerts.</span></span>

