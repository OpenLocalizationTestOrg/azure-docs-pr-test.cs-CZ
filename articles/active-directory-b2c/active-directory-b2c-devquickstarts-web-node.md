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
# <a name="azure-ad-b2c-add-sign-in-tooa-nodejs-web-app"></a><span data-ttu-id="120b9-103">Azure AD B2C: Přidání webové aplikace Node.js tooa přihlášení</span><span class="sxs-lookup"><span data-stu-id="120b9-103">Azure AD B2C: Add sign-in tooa Node.js web app</span></span>

<span data-ttu-id="120b9-104">**Passport** je ověřovací middleware pro Node.js.</span><span class="sxs-lookup"><span data-stu-id="120b9-104">**Passport** is authentication middleware for Node.js.</span></span> <span data-ttu-id="120b9-105">Passport je velmi flexibilní a modulární a lze ho snadno nainstalovat v jakékoli webové aplikaci využívající Express nebo Restify.</span><span class="sxs-lookup"><span data-stu-id="120b9-105">Extremely flexible and modular, Passport can be unobtrusively installed in any Express-based or Restify web application.</span></span> <span data-ttu-id="120b9-106">Komplexní sada strategií podporuje ověřování pomocí uživatelského jména a hesla, Facebooku, Twitteru a dalších.</span><span class="sxs-lookup"><span data-stu-id="120b9-106">A comprehensive set of strategies supports authentication by using a user name and password, Facebook, Twitter, and more.</span></span>

<span data-ttu-id="120b9-107">Vyvinuli jsme strategii pro Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="120b9-107">We have developed a strategy for Azure Active Directory (Azure AD).</span></span> <span data-ttu-id="120b9-108">Nainstalujete tento modul a poté přidejte hello Azure AD `passport-azure-ad` modulu plug-in.</span><span class="sxs-lookup"><span data-stu-id="120b9-108">You will install this module and then add hello Azure AD `passport-azure-ad` plug-in.</span></span>

<span data-ttu-id="120b9-109">toodo, budete muset:</span><span class="sxs-lookup"><span data-stu-id="120b9-109">toodo this, you need to:</span></span>

1. <span data-ttu-id="120b9-110">Zaregistrovat aplikaci pomocí Azure AD.</span><span class="sxs-lookup"><span data-stu-id="120b9-110">Register an application by using Azure AD.</span></span>
2. <span data-ttu-id="120b9-111">Nastavení vaší aplikace hello toouse `passport-azure-ad` modulu plug-in.</span><span class="sxs-lookup"><span data-stu-id="120b9-111">Set up your app toouse hello `passport-azure-ad` plug-in.</span></span>
3. <span data-ttu-id="120b9-112">Použijte Passport tooissue-přihlášení a odhlášení požadavky tooAzure AD.</span><span class="sxs-lookup"><span data-stu-id="120b9-112">Use Passport tooissue sign-in and sign-out requests tooAzure AD.</span></span>
4. <span data-ttu-id="120b9-113">Tisknout data o uživatelích.</span><span class="sxs-lookup"><span data-stu-id="120b9-113">Print user data.</span></span>

<span data-ttu-id="120b9-114">Hello kód pro tento kurz [je udržovaný na Githubu](https://github.com/AzureADQuickStarts/B2C-WebApp-OpenIDConnect-NodeJS).</span><span class="sxs-lookup"><span data-stu-id="120b9-114">hello code for this tutorial [is maintained on GitHub](https://github.com/AzureADQuickStarts/B2C-WebApp-OpenIDConnect-NodeJS).</span></span> <span data-ttu-id="120b9-115">toofollow společně, můžete [stáhnout kostru aplikace hello jako soubor ZIP](https://github.com/AzureADQuickStarts/B2C-WebApp-OpenIDConnect-NodeJS/archive/skeleton.zip).</span><span class="sxs-lookup"><span data-stu-id="120b9-115">toofollow along, you can [download hello app's skeleton as a .zip file](https://github.com/AzureADQuickStarts/B2C-WebApp-OpenIDConnect-NodeJS/archive/skeleton.zip).</span></span> <span data-ttu-id="120b9-116">Můžete také hello kostru klonovat:</span><span class="sxs-lookup"><span data-stu-id="120b9-116">You can also clone hello skeleton:</span></span>

```git clone --branch skeleton https://github.com/AzureADQuickStarts/B2C-WebApp-OpenIDConnect-NodeJS.git```

<span data-ttu-id="120b9-117">aplikace Hello dokončit, je k dispozici na konci hello tohoto kurzu.</span><span class="sxs-lookup"><span data-stu-id="120b9-117">hello completed application is provided at hello end of this tutorial.</span></span>

## <a name="get-an-azure-ad-b2c-directory"></a><span data-ttu-id="120b9-118">Získání adresáře služby Azure AD B2C</span><span class="sxs-lookup"><span data-stu-id="120b9-118">Get an Azure AD B2C directory</span></span>

<span data-ttu-id="120b9-119">Před použitím Azure AD B2C musíte vytvořit adresář, nebo klienta.</span><span class="sxs-lookup"><span data-stu-id="120b9-119">Before you can use Azure AD B2C, you must create a directory, or tenant.</span></span>  <span data-ttu-id="120b9-120">Adresář je kontejner pro všechny vaše uživatele, aplikace, skupiny a další.</span><span class="sxs-lookup"><span data-stu-id="120b9-120">A directory is a container for all of your users, apps, groups, and more.</span></span> <span data-ttu-id="120b9-121">Pokud ho ještě nemáte, [vytvořte adresář B2C](active-directory-b2c-get-started.md) předtím, než budete pokračovat.</span><span class="sxs-lookup"><span data-stu-id="120b9-121">If you don't have one already, [create a B2C directory](active-directory-b2c-get-started.md) before you continue in this guide.</span></span>

## <a name="create-an-application"></a><span data-ttu-id="120b9-122">Vytvoření aplikace</span><span class="sxs-lookup"><span data-stu-id="120b9-122">Create an application</span></span>

<span data-ttu-id="120b9-123">Dále musíte toocreate aplikace ve svém adresáři B2C.</span><span class="sxs-lookup"><span data-stu-id="120b9-123">Next, you need toocreate an app in your B2C directory.</span></span> <span data-ttu-id="120b9-124">Díky tomu získá informace o Azure AD, je nutné toocommunicate bezpečně s vaší aplikací.</span><span class="sxs-lookup"><span data-stu-id="120b9-124">This gives Azure AD information that it needs toocommunicate securely with your app.</span></span> <span data-ttu-id="120b9-125">Obě hello klientské aplikace a webové rozhraní API budou odpovídat jedné **ID aplikace**, protože společně tvoří jednu logickou aplikaci.</span><span class="sxs-lookup"><span data-stu-id="120b9-125">Both hello client app and web API will be represented by a single **Application ID**, because they comprise one logical app.</span></span> <span data-ttu-id="120b9-126">toocreate na aplikace, postupujte podle [tyto pokyny](active-directory-b2c-app-registration.md).</span><span class="sxs-lookup"><span data-stu-id="120b9-126">toocreate an app, follow [these instructions](active-directory-b2c-app-registration.md).</span></span> <span data-ttu-id="120b9-127">Ujistěte se, že:</span><span class="sxs-lookup"><span data-stu-id="120b9-127">Be sure to:</span></span>

- <span data-ttu-id="120b9-128">Zahrnout **webové aplikace**/**webové rozhraní API** v aplikaci hello.</span><span class="sxs-lookup"><span data-stu-id="120b9-128">Include a **web app**/**web API** in hello application.</span></span>
- <span data-ttu-id="120b9-129">Jste do pole **Adresa URL odpovědi** vyplnili `http://localhost:3000/auth/openid/return`.</span><span class="sxs-lookup"><span data-stu-id="120b9-129">Enter `http://localhost:3000/auth/openid/return` as a **Reply URL**.</span></span> <span data-ttu-id="120b9-130">Je hello výchozí adresa URL pro tuto ukázku kódu.</span><span class="sxs-lookup"><span data-stu-id="120b9-130">It is hello default URL for this code sample.</span></span>
- <span data-ttu-id="120b9-131">Vytvořte pro aplikaci **tajný klíč aplikace** a poznamenejte si ho.</span><span class="sxs-lookup"><span data-stu-id="120b9-131">Create an **Application secret** for your application and copy it.</span></span> <span data-ttu-id="120b9-132">Budete ho potřebovat později.</span><span class="sxs-lookup"><span data-stu-id="120b9-132">You will need it later.</span></span> <span data-ttu-id="120b9-133">Všimněte si, že tato hodnota se musí toobe [uvozena v XML](https://www.w3.org/TR/2006/REC-xml11-20060816/#dt-escape) dříve, než ho použijete.</span><span class="sxs-lookup"><span data-stu-id="120b9-133">Note that this value needs toobe [XML escaped](https://www.w3.org/TR/2006/REC-xml11-20060816/#dt-escape) before you use it.</span></span>
- <span data-ttu-id="120b9-134">Kopírování hello **ID aplikace** který je přiřazený tooyour aplikace.</span><span class="sxs-lookup"><span data-stu-id="120b9-134">Copy hello **Application ID** that is assigned tooyour app.</span></span> <span data-ttu-id="120b9-135">To také budete potřebovat později.</span><span class="sxs-lookup"><span data-stu-id="120b9-135">You'll also need this later.</span></span>

[!INCLUDE [active-directory-b2c-devquickstarts-v2-apps](../../includes/active-directory-b2c-devquickstarts-v2-apps.md)]

## <a name="create-your-policies"></a><span data-ttu-id="120b9-136">Vytvořte svoje zásady</span><span class="sxs-lookup"><span data-stu-id="120b9-136">Create your policies</span></span>

<span data-ttu-id="120b9-137">V Azure AD B2C je každé uživatelské rozhraní definováno [zásadou](active-directory-b2c-reference-policies.md).</span><span class="sxs-lookup"><span data-stu-id="120b9-137">In Azure AD B2C, every user experience is defined by a [policy](active-directory-b2c-reference-policies.md).</span></span> <span data-ttu-id="120b9-138">Tato aplikace obsahuje tři činnosti koncového uživatele: registrace, přihlášení a přihlášení pomocí Facebooku.</span><span class="sxs-lookup"><span data-stu-id="120b9-138">This app contains three identity experiences: sign up, sign in, and sign in by using Facebook.</span></span> <span data-ttu-id="120b9-139">Je třeba toocreate tuto zásadu každého typu, jak je popsáno v [článku o zásadách](active-directory-b2c-reference-policies.md#create-a-sign-up-policy).</span><span class="sxs-lookup"><span data-stu-id="120b9-139">You need toocreate this policy of each type, as described in the [policy reference article](active-directory-b2c-reference-policies.md#create-a-sign-up-policy).</span></span> <span data-ttu-id="120b9-140">Když vytváříte tyto tři zásady, nezapomeňte:</span><span class="sxs-lookup"><span data-stu-id="120b9-140">When you create your three policies, be sure to:</span></span>

- <span data-ttu-id="120b9-141">Zvolte hello **zobrazovaný název** a další atributy registrace ve svojí registrační zásadě.</span><span class="sxs-lookup"><span data-stu-id="120b9-141">Choose hello **Display name** and other sign-up attributes in your sign-up policy.</span></span>
- <span data-ttu-id="120b9-142">Zvolte hello **zobrazovaný název** a **ID objektu** deklarace identity aplikace v každé zásadě.</span><span class="sxs-lookup"><span data-stu-id="120b9-142">Choose hello **Display name** and **Object ID** application claims in every policy.</span></span> <span data-ttu-id="120b9-143">Můžete zvolit i další deklarace identity.</span><span class="sxs-lookup"><span data-stu-id="120b9-143">You can choose other claims as well.</span></span>
- <span data-ttu-id="120b9-144">Kopírování hello **název** po jejím vytvoření každé zásady.</span><span class="sxs-lookup"><span data-stu-id="120b9-144">Copy hello **Name** of each policy after you create it.</span></span> <span data-ttu-id="120b9-145">Měl by mít předponu hello `b2c_1_`.</span><span class="sxs-lookup"><span data-stu-id="120b9-145">It should have hello prefix `b2c_1_`.</span></span>  <span data-ttu-id="120b9-146">Tyto názvy zásad budete potřebovat později.</span><span class="sxs-lookup"><span data-stu-id="120b9-146">You'll need these policy names later.</span></span>

[!INCLUDE [active-directory-b2c-devquickstarts-policy](../../includes/active-directory-b2c-devquickstarts-policy.md)]

<span data-ttu-id="120b9-147">Po vytvoření tří zásad jste připravené toobuild vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="120b9-147">After you create your three policies, you're ready toobuild your app.</span></span>

<span data-ttu-id="120b9-148">Všimněte si, že tento článek nepopisuje, jak zásady hello toouse jste právě vytvořili.</span><span class="sxs-lookup"><span data-stu-id="120b9-148">Note that this article does not cover how toouse hello policies you just created.</span></span> <span data-ttu-id="120b9-149">toolearn o tom, jak fungují zásady v Azure AD B2C, začněte s hello [webové aplikace kurzem Začínáme .NET](active-directory-b2c-devquickstarts-web-dotnet.md).</span><span class="sxs-lookup"><span data-stu-id="120b9-149">toolearn about how policies work in Azure AD B2C, start with hello [.NET web app getting started tutorial](active-directory-b2c-devquickstarts-web-dotnet.md).</span></span>

## <a name="add-prerequisites-tooyour-directory"></a><span data-ttu-id="120b9-150">Přidejte adresář tooyour požadavky</span><span class="sxs-lookup"><span data-stu-id="120b9-150">Add prerequisites tooyour directory</span></span>

<span data-ttu-id="120b9-151">Z příkazového řádku hello změňte adresáře tooyour kořenové složky, pokud si nejste již existuje.</span><span class="sxs-lookup"><span data-stu-id="120b9-151">From hello command line, change directories tooyour root folder, if you're not already there.</span></span> <span data-ttu-id="120b9-152">Spusťte následující příkazy hello:</span><span class="sxs-lookup"><span data-stu-id="120b9-152">Run hello following commands:</span></span>

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

<span data-ttu-id="120b9-153">Kromě toho jsme použili `passport-azure-ad` pro náhled v kostře hello hello rychlý start naší.</span><span class="sxs-lookup"><span data-stu-id="120b9-153">In addition, we used `passport-azure-ad` for our preview in hello skeleton of hello Quickstart.</span></span>

- `npm install passport-azure-ad`

<span data-ttu-id="120b9-154">To nainstaluje knihovny hello který `passport-azure-ad` závisí na.</span><span class="sxs-lookup"><span data-stu-id="120b9-154">This will install hello libraries that `passport-azure-ad` depends on.</span></span>

## <a name="set-up-your-app-toouse-hello-passport-nodejs-strategy"></a><span data-ttu-id="120b9-155">Nastavení vaší aplikace toouse hello strategie Passport-Node.js</span><span class="sxs-lookup"><span data-stu-id="120b9-155">Set up your app toouse hello Passport-Node.js strategy</span></span>
<span data-ttu-id="120b9-156">Nakonfigurujte hello Express middleware toouse hello ověřovacího protokolu OpenID Connect.</span><span class="sxs-lookup"><span data-stu-id="120b9-156">Configure hello Express middleware toouse hello OpenID Connect authentication protocol.</span></span> <span data-ttu-id="120b9-157">Passport bude použité tooissue požadavků na přihlášení a odhlášení, správě uživatelských relací a získat informace o uživatelích, mimo jiné.</span><span class="sxs-lookup"><span data-stu-id="120b9-157">Passport will be used tooissue sign-in and sign-out requests, manage user sessions, and get information about users, among other actions.</span></span>

<span data-ttu-id="120b9-158">Otevřete hello `config.js` souboru v kořenovém hello hello projektu a zadejte hodnoty konfigurace vaší aplikace v hello `exports.creds` části.</span><span class="sxs-lookup"><span data-stu-id="120b9-158">Open hello `config.js` file in hello root of hello project and enter your app's configuration values in hello `exports.creds` section.</span></span>
- <span data-ttu-id="120b9-159">`clientID`: hello **ID aplikace** přiřazené tooyour aplikace v portálu pro registraci hello.</span><span class="sxs-lookup"><span data-stu-id="120b9-159">`clientID`: hello **Application ID** assigned tooyour app in hello registration portal.</span></span>
- <span data-ttu-id="120b9-160">`returnURL`: hello **identifikátor URI pro přesměrování** jste zadali v portálu hello.</span><span class="sxs-lookup"><span data-stu-id="120b9-160">`returnURL`: hello **Redirect URI** you entered in hello portal.</span></span>
- <span data-ttu-id="120b9-161">`tenantName`: název klienta hello vaší aplikace, například **contoso.onmicrosoft.com**.</span><span class="sxs-lookup"><span data-stu-id="120b9-161">`tenantName`: hello tenant name of your app, for example, **contoso.onmicrosoft.com**.</span></span>

[!INCLUDE [active-directory-b2c-devquickstarts-tenant-name](../../includes/active-directory-b2c-devquickstarts-tenant-name.md)]

<span data-ttu-id="120b9-162">Otevřete hello `app.js` soubor v kořenovém hello hello projektu.</span><span class="sxs-lookup"><span data-stu-id="120b9-162">Open hello `app.js` file in hello root of hello project.</span></span> <span data-ttu-id="120b9-163">Přidejte následující volání tooinvoke hello hello `OIDCStrategy` strategie, která se dodává s `passport-azure-ad`.</span><span class="sxs-lookup"><span data-stu-id="120b9-163">Add hello following call tooinvoke hello `OIDCStrategy` strategy that comes with `passport-azure-ad`.</span></span>


```JavaScript
var OIDCStrategy = require('passport-azure-ad').OIDCStrategy;

// Add some logging
var log = bunyan.createLogger({
    name: 'Microsoft OIDC Example Web Application'
});
```

<span data-ttu-id="120b9-164">Použijte právě přidanou toohandle žádostí o přihlášení strategii hello.</span><span class="sxs-lookup"><span data-stu-id="120b9-164">Use hello strategy you just referenced toohandle sign-in requests.</span></span>

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
<span data-ttu-id="120b9-165">Passport používá podobný princip pro všechny strategie (včetně Twitteru a Facebooku).</span><span class="sxs-lookup"><span data-stu-id="120b9-165">Passport uses a similar pattern for all of its strategies (including Twitter and Facebook).</span></span> <span data-ttu-id="120b9-166">Vzor toothis řídí všichni tvůrci strategií.</span><span class="sxs-lookup"><span data-stu-id="120b9-166">All strategy writers adhere toothis pattern.</span></span> <span data-ttu-id="120b9-167">Když se podíváte na hello strategie, uvidíte předejte ji `function()` s token a `done` jako parametry hello.</span><span class="sxs-lookup"><span data-stu-id="120b9-167">When you look at hello strategy, you can see that you pass it a `function()` that has a token and a `done` as hello parameters.</span></span> <span data-ttu-id="120b9-168">Hello strategie vrátí tooyou po dokončení veškeré práce.</span><span class="sxs-lookup"><span data-stu-id="120b9-168">hello strategy comes back tooyou after it has done all of its work.</span></span> <span data-ttu-id="120b9-169">Uložení hello uživatele a skrytí tokenu hello tak, aby tooask pro ni není nutné znovu.</span><span class="sxs-lookup"><span data-stu-id="120b9-169">Store hello user and stash hello token so that you don’t need tooask for it again.</span></span>

> [!IMPORTANT]
<span data-ttu-id="120b9-170">Hello předchozí kód přijme všechny uživatele, kterým ověří hello server.</span><span class="sxs-lookup"><span data-stu-id="120b9-170">hello preceding code takes all users whom hello server authenticates.</span></span> <span data-ttu-id="120b9-171">To je automatická registrace.</span><span class="sxs-lookup"><span data-stu-id="120b9-171">This is autoregistration.</span></span> <span data-ttu-id="120b9-172">Pokud používáte produkční servery, nechcete toolet mezi uživateli, pokud jste prošli registračním procesem, který jste nastavili.</span><span class="sxs-lookup"><span data-stu-id="120b9-172">When you use production servers, you don’t want toolet in users unless they have gone through a registration process that you have set up.</span></span> <span data-ttu-id="120b9-173">Tento princip je často k vidění u uživatelských aplikací.</span><span class="sxs-lookup"><span data-stu-id="120b9-173">You can often see this pattern in consumer apps.</span></span> <span data-ttu-id="120b9-174">Ty umožňují tooregister pomocí Facebooku, ale poté vás vyzvou toofill Další informace.</span><span class="sxs-lookup"><span data-stu-id="120b9-174">These allow you tooregister by using Facebook, but then they ask you toofill out additional information.</span></span> <span data-ttu-id="120b9-175">Naše aplikace nebyla pouze příklad, jsme pravděpodobně extrahování e-mailovou adresu z objektu tokenu hello, která je vrácena a pak požádejte uživatele toofill hello Další informace.</span><span class="sxs-lookup"><span data-stu-id="120b9-175">If our application wasn’t a sample, we could extract an email address from hello token object that is returned, and then ask hello user toofill out additional information.</span></span> <span data-ttu-id="120b9-176">Protože se jedná o testovací server, jednoduše přidáme uživatele databáze v paměti toohello.</span><span class="sxs-lookup"><span data-stu-id="120b9-176">Because this is a test server, we simply add users toohello in-memory database.</span></span>

<span data-ttu-id="120b9-177">Přidejte hello metody, které vám umožňují sledovat tookeep uživatelů, kteří mají přihlášení, jak vyžaduje Passport.</span><span class="sxs-lookup"><span data-stu-id="120b9-177">Add hello methods that allow you tookeep track of users who have signed in, as required by Passport.</span></span> <span data-ttu-id="120b9-178">To zahrnuje serializaci a deserializaci informací o uživateli:</span><span class="sxs-lookup"><span data-stu-id="120b9-178">This includes serializing and deserializing user information:</span></span>

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

<span data-ttu-id="120b9-179">Přidání modulu Express hello kód tooload hello.</span><span class="sxs-lookup"><span data-stu-id="120b9-179">Add hello code tooload hello Express engine.</span></span> <span data-ttu-id="120b9-180">V následující hello, uvidíte, že používáme výchozí hello `/views` a `/routes` vzor, který poskytuje Express.</span><span class="sxs-lookup"><span data-stu-id="120b9-180">In hello following, you can see that we use hello default `/views` and `/routes` pattern that Express provides.</span></span>

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

<span data-ttu-id="120b9-181">Přidat hello `POST` tras, které přebírají hello skutečné žádostí o přihlášení toohello `passport-azure-ad` modul:</span><span class="sxs-lookup"><span data-stu-id="120b9-181">Add hello `POST` routes that hand off hello actual sign-in requests toohello `passport-azure-ad` engine:</span></span>

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

## <a name="use-passport-tooissue-sign-in-and-sign-out-requests-tooazure-ad"></a><span data-ttu-id="120b9-182">Použít Passport tooissue-přihlášení a odhlášení požadavky tooAzure AD</span><span class="sxs-lookup"><span data-stu-id="120b9-182">Use Passport tooissue sign-in and sign-out requests tooAzure AD</span></span>

<span data-ttu-id="120b9-183">Aplikace je nyní správně nakonfigurované toocommunicate s hello koncového bodu v2.0 pomocí ověřovacího protokolu OpenID Connect hello.</span><span class="sxs-lookup"><span data-stu-id="120b9-183">Your app is now properly configured toocommunicate with hello v2.0 endpoint by using hello OpenID Connect authentication protocol.</span></span> <span data-ttu-id="120b9-184">`passport-azure-ad`má postaráno hello podrobnosti o věnujte zpráv ověřování, ověřování tokenů z Azure AD a údržbě uživatelské relace.</span><span class="sxs-lookup"><span data-stu-id="120b9-184">`passport-azure-ad` has taken care of hello details of crafting authentication messages, validating tokens from Azure AD, and maintaining user session.</span></span> <span data-ttu-id="120b9-185">Všechno, co zůstane je toogive vaši uživatelé toosign způsob, jak v a přihlaste se na více systémů a toogather Další informace o uživatele, kteří mají přihlášení.</span><span class="sxs-lookup"><span data-stu-id="120b9-185">All that remains is toogive your users a way toosign in and sign out, and toogather additional information on users who have signed in.</span></span>

<span data-ttu-id="120b9-186">Nejprve přidejte hello výchozí, přihlášení, účet a odhlášení metody tooyour `app.js` souboru:</span><span class="sxs-lookup"><span data-stu-id="120b9-186">First, add hello default, sign-in, account, and sign-out methods tooyour `app.js` file:</span></span>

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

<span data-ttu-id="120b9-187">tooreview tyto metody podrobně:</span><span class="sxs-lookup"><span data-stu-id="120b9-187">tooreview these methods in detail:</span></span>
- <span data-ttu-id="120b9-188">Hello `/` trasa přesměruje toohello `index.ejs` zobrazení předáním hello uživatele v požadavku hello (pokud existuje).</span><span class="sxs-lookup"><span data-stu-id="120b9-188">hello `/` route redirects toohello `index.ejs` view by passing hello user in hello request (if it exists).</span></span>
- <span data-ttu-id="120b9-189">Hello `/account` trasy nejprve ověří, zda jste přihlášeni (hello implementace pro toto je níže).</span><span class="sxs-lookup"><span data-stu-id="120b9-189">hello `/account` route first verifies that you are authenticated (hello implementation for this is below).</span></span> <span data-ttu-id="120b9-190">Pak předá hello uživatele v požadavku hello tak, aby mohli získat dodatečné informace o uživateli hello.</span><span class="sxs-lookup"><span data-stu-id="120b9-190">It then passes hello user in hello request so that you can get additional information about hello user.</span></span>
- <span data-ttu-id="120b9-191">Hello `/login` trasy volání hello `azuread-openidconnect` authenticator z `passport-azure-ad`.</span><span class="sxs-lookup"><span data-stu-id="120b9-191">hello `/login` route calls hello `azuread-openidconnect` authenticator from `passport-azure-ad`.</span></span> <span data-ttu-id="120b9-192">Pokud dojde k neúspěchu, trasa hello přesměruje uživatele hello zpět příliš`/login`.</span><span class="sxs-lookup"><span data-stu-id="120b9-192">If it doesn't succeed, hello route redirects hello user back too`/login`.</span></span>
- <span data-ttu-id="120b9-193">`/logout` jednoduše volá `logout.ejs` (a jeho trasu).</span><span class="sxs-lookup"><span data-stu-id="120b9-193">`/logout` simply calls `logout.ejs` (and its route).</span></span> <span data-ttu-id="120b9-194">Zruší soubory cookie a pak se vrátí hello zpět uživatele příliš`index.ejs`.</span><span class="sxs-lookup"><span data-stu-id="120b9-194">This clears cookies and then returns hello user back too`index.ejs`.</span></span>


<span data-ttu-id="120b9-195">Pro poslední část hello `app.js`, přidejte hello `EnsureAuthenticated` metoda, která se používá v hello `/account` trasy.</span><span class="sxs-lookup"><span data-stu-id="120b9-195">For hello last part of `app.js`, add hello `EnsureAuthenticated` method that is used in hello `/account` route.</span></span>

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

<span data-ttu-id="120b9-196">Nakonec vytvořte samotný server hello v `app.js`.</span><span class="sxs-lookup"><span data-stu-id="120b9-196">Finally, create hello server itself in `app.js`.</span></span>

```JavaScript

app.listen(3000);

```


## <a name="create-hello-views-and-routes-in-express-toocall-your-policies"></a><span data-ttu-id="120b9-197">Vytvoření zobrazení hello a směruje v Express toocall zásad</span><span class="sxs-lookup"><span data-stu-id="120b9-197">Create hello views and routes in Express toocall your policies</span></span>

<span data-ttu-id="120b9-198">Vaše `app.js` je nyní hotová.</span><span class="sxs-lookup"><span data-stu-id="120b9-198">Your `app.js` is now complete.</span></span> <span data-ttu-id="120b9-199">Bude třeba jen tooadd hello trasy a zobrazení, které vám umožňují toocall hello přihlášení a odhlášení zásady.</span><span class="sxs-lookup"><span data-stu-id="120b9-199">You just need tooadd hello routes and views that allow you toocall hello sign-in and sign-up policies.</span></span> <span data-ttu-id="120b9-200">Ty zpracovávají také dříve hello `/logout` a `/login` vytvořené trasy.</span><span class="sxs-lookup"><span data-stu-id="120b9-200">These also handle hello `/logout` and `/login` routes you created.</span></span>

<span data-ttu-id="120b9-201">Vytvoření hello `/routes/index.js` trasy pod kořenovým adresářem hello.</span><span class="sxs-lookup"><span data-stu-id="120b9-201">Create hello `/routes/index.js` route under hello root directory.</span></span>

```JavaScript

/*
 * GET home page.
 */

exports.index = function(req, res){
  res.render('index', { title: 'Express' });
};
```

<span data-ttu-id="120b9-202">Vytvoření hello `/routes/user.js` trasy pod kořenovým adresářem hello.</span><span class="sxs-lookup"><span data-stu-id="120b9-202">Create hello `/routes/user.js` route under hello root directory.</span></span>

```JavaScript

/*
 * GET users listing.
 */

exports.list = function(req, res){
  res.send("respond with a resource");
};
```

<span data-ttu-id="120b9-203">Tyto jednoduché trasy předávají požadavky tooyour zobrazení.</span><span class="sxs-lookup"><span data-stu-id="120b9-203">These simple routes pass along requests tooyour views.</span></span> <span data-ttu-id="120b9-204">Obsahují hello uživatele, pokud je k dispozici.</span><span class="sxs-lookup"><span data-stu-id="120b9-204">They include hello user, if one is present.</span></span>

<span data-ttu-id="120b9-205">Vytvoření hello `/views/index.ejs` zobrazení pod kořenovým adresářem hello.</span><span class="sxs-lookup"><span data-stu-id="120b9-205">Create hello `/views/index.ejs` view under hello root directory.</span></span> <span data-ttu-id="120b9-206">Toto je jednoduchá stránka, která volá zásady pro přihlášení a odhlášení. Můžete ji použít i toograb informace o účtu.</span><span class="sxs-lookup"><span data-stu-id="120b9-206">This is a simple page that calls policies for sign-in and sign-out. You can also use it toograb account information.</span></span> <span data-ttu-id="120b9-207">Všimněte si, které můžete použít hello podmíněného `if (!user)` jako předávání hello uživatele v požadavku hello tooprovide důkaz, že uživatele hello je přihlášený.</span><span class="sxs-lookup"><span data-stu-id="120b9-207">Note that you can use hello conditional `if (!user)` as hello user is passed through in hello request tooprovide evidence that hello user is signed in.</span></span>

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

<span data-ttu-id="120b9-208">Vytvoření hello `/views/account.ejs` zobrazení pod kořenovým adresářem hello, takže si můžete zobrazit další informace, `passport-azure-ad` put v požadavku uživatele hello.</span><span class="sxs-lookup"><span data-stu-id="120b9-208">Create hello `/views/account.ejs` view under hello root directory so that you can view additional information that `passport-azure-ad` put in hello user request.</span></span>

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

<span data-ttu-id="120b9-209">Nyní můžete sestavit a spustit svoji aplikaci.</span><span class="sxs-lookup"><span data-stu-id="120b9-209">You can now build and run your app.</span></span>

<span data-ttu-id="120b9-210">Spustit `node app.js` a přejděte příliš`http://localhost:3000`</span><span class="sxs-lookup"><span data-stu-id="120b9-210">Run `node app.js` and navigate too`http://localhost:3000`</span></span>


<span data-ttu-id="120b9-211">Registrace nebo přihlášení toohello aplikace pomocí emailu nebo Facebooku.</span><span class="sxs-lookup"><span data-stu-id="120b9-211">Sign up or sign in toohello app by using email or Facebook.</span></span> <span data-ttu-id="120b9-212">Odhlaste se a přihlaste se jako jiný uživatel.</span><span class="sxs-lookup"><span data-stu-id="120b9-212">Sign out and sign back in as a different user.</span></span>

##<a name="next-steps"></a><span data-ttu-id="120b9-213">Další kroky</span><span class="sxs-lookup"><span data-stu-id="120b9-213">Next steps</span></span>

<span data-ttu-id="120b9-214">Pro referenci hello dokončit ukázka (bez vašich hodnot nastavení) [je k dispozici jako soubor ZIP](https://github.com/AzureADQuickStarts/B2C-WebApp-OpenIDConnect-NodeJS/archive/complete.zip).</span><span class="sxs-lookup"><span data-stu-id="120b9-214">For reference, hello completed sample (without your configuration values) [is provided as a .zip file](https://github.com/AzureADQuickStarts/B2C-WebApp-OpenIDConnect-NodeJS/archive/complete.zip).</span></span> <span data-ttu-id="120b9-215">Můžete ho také klonovat z GitHubu:</span><span class="sxs-lookup"><span data-stu-id="120b9-215">You can also clone it from GitHub:</span></span>

```git clone --branch complete https://github.com/AzureADQuickStarts/B2C-WebApp-OpenIDConnect-nodejs.git```

<span data-ttu-id="120b9-216">Nyní se můžete přesunout na toomore advanced témata.</span><span class="sxs-lookup"><span data-stu-id="120b9-216">You can now move on toomore advanced topics.</span></span> <span data-ttu-id="120b9-217">Můžete vyzkoušet:</span><span class="sxs-lookup"><span data-stu-id="120b9-217">You might try:</span></span>

[<span data-ttu-id="120b9-218">Zabezpečení webového rozhraní API pomocí modelu hello B2C v Node.js</span><span class="sxs-lookup"><span data-stu-id="120b9-218">Secure a web API by using hello B2C model in Node.js</span></span>](active-directory-b2c-devquickstarts-api-node.md)

<!--

For additional resources, check out:
You can now move on toomore advanced B2C topics. You might try:

[Call a Node.js web API from a Node.js web app]()

[Customizing hello your B2C App's UX >>]()

-->
