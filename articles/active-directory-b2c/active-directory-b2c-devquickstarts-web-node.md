---
title: "Přidání přihlašování do webové aplikace Node.js pro Azure B2C | Dokumentace Microsoftu"
description: "Jak sestavit webovou aplikaci Node.js s přihlašováním uživatelů pomocí klienta B2C."
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
ms.openlocfilehash: c85b8f8434d1e837ac96ac63b9b37f990677ed6e
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="azure-ad-b2c-add-sign-in-to-a-nodejs-web-app"></a><span data-ttu-id="bc225-103">Azure AD B2C: Přidání přihlašování do webové aplikace Node.js</span><span class="sxs-lookup"><span data-stu-id="bc225-103">Azure AD B2C: Add sign-in to a Node.js web app</span></span>

<span data-ttu-id="bc225-104">**Passport** je ověřovací middleware pro Node.js.</span><span class="sxs-lookup"><span data-stu-id="bc225-104">**Passport** is authentication middleware for Node.js.</span></span> <span data-ttu-id="bc225-105">Passport je velmi flexibilní a modulární a lze ho snadno nainstalovat v jakékoli webové aplikaci využívající Express nebo Restify.</span><span class="sxs-lookup"><span data-stu-id="bc225-105">Extremely flexible and modular, Passport can be unobtrusively installed in any Express-based or Restify web application.</span></span> <span data-ttu-id="bc225-106">Komplexní sada strategií podporuje ověřování pomocí uživatelského jména a hesla, Facebooku, Twitteru a dalších.</span><span class="sxs-lookup"><span data-stu-id="bc225-106">A comprehensive set of strategies supports authentication by using a user name and password, Facebook, Twitter, and more.</span></span>

<span data-ttu-id="bc225-107">Vyvinuli jsme strategii pro Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="bc225-107">We have developed a strategy for Azure Active Directory (Azure AD).</span></span> <span data-ttu-id="bc225-108">Nainstalujete tento modul a poté přidáte modul plug-in Azure AD `passport-azure-ad`.</span><span class="sxs-lookup"><span data-stu-id="bc225-108">You will install this module and then add the Azure AD `passport-azure-ad` plug-in.</span></span>

<span data-ttu-id="bc225-109">Budete muset:</span><span class="sxs-lookup"><span data-stu-id="bc225-109">To do this, you need to:</span></span>

1. <span data-ttu-id="bc225-110">Zaregistrovat aplikaci pomocí Azure AD.</span><span class="sxs-lookup"><span data-stu-id="bc225-110">Register an application by using Azure AD.</span></span>
2. <span data-ttu-id="bc225-111">Nastavit aplikaci tak pro používání modulu plug-in `passport-azure-ad`.</span><span class="sxs-lookup"><span data-stu-id="bc225-111">Set up your app to use the `passport-azure-ad` plug-in.</span></span>
3. <span data-ttu-id="bc225-112">Použít Passport pro zasílání požadavků na přihlášení a odhlášení do Azure AD.</span><span class="sxs-lookup"><span data-stu-id="bc225-112">Use Passport to issue sign-in and sign-out requests to Azure AD.</span></span>
4. <span data-ttu-id="bc225-113">Tisknout data o uživatelích.</span><span class="sxs-lookup"><span data-stu-id="bc225-113">Print user data.</span></span>

<span data-ttu-id="bc225-114">Kód k tomuto kurzu [je udržovaný na GitHubu](https://github.com/AzureADQuickStarts/B2C-WebApp-OpenIDConnect-NodeJS).</span><span class="sxs-lookup"><span data-stu-id="bc225-114">The code for this tutorial [is maintained on GitHub](https://github.com/AzureADQuickStarts/B2C-WebApp-OpenIDConnect-NodeJS).</span></span> <span data-ttu-id="bc225-115">Chcete-li kód sledovat, můžete si [stáhnout kostru aplikace jako soubor ZIP](https://github.com/AzureADQuickStarts/B2C-WebApp-OpenIDConnect-NodeJS/archive/skeleton.zip).</span><span class="sxs-lookup"><span data-stu-id="bc225-115">To follow along, you can [download the app's skeleton as a .zip file](https://github.com/AzureADQuickStarts/B2C-WebApp-OpenIDConnect-NodeJS/archive/skeleton.zip).</span></span> <span data-ttu-id="bc225-116">Kostru můžete také klonovat:</span><span class="sxs-lookup"><span data-stu-id="bc225-116">You can also clone the skeleton:</span></span>

```git clone --branch skeleton https://github.com/AzureADQuickStarts/B2C-WebApp-OpenIDConnect-NodeJS.git```

<span data-ttu-id="bc225-117">Hotová aplikace je k dispozici na konci tohoto kurzu.</span><span class="sxs-lookup"><span data-stu-id="bc225-117">The completed application is provided at the end of this tutorial.</span></span>

## <a name="get-an-azure-ad-b2c-directory"></a><span data-ttu-id="bc225-118">Získání adresáře služby Azure AD B2C</span><span class="sxs-lookup"><span data-stu-id="bc225-118">Get an Azure AD B2C directory</span></span>

<span data-ttu-id="bc225-119">Před použitím Azure AD B2C musíte vytvořit adresář, nebo klienta.</span><span class="sxs-lookup"><span data-stu-id="bc225-119">Before you can use Azure AD B2C, you must create a directory, or tenant.</span></span>  <span data-ttu-id="bc225-120">Adresář je kontejner pro všechny vaše uživatele, aplikace, skupiny a další.</span><span class="sxs-lookup"><span data-stu-id="bc225-120">A directory is a container for all of your users, apps, groups, and more.</span></span> <span data-ttu-id="bc225-121">Pokud ho ještě nemáte, [vytvořte adresář B2C](active-directory-b2c-get-started.md) předtím, než budete pokračovat.</span><span class="sxs-lookup"><span data-stu-id="bc225-121">If you don't have one already, [create a B2C directory](active-directory-b2c-get-started.md) before you continue in this guide.</span></span>

## <a name="create-an-application"></a><span data-ttu-id="bc225-122">Vytvoření aplikace</span><span class="sxs-lookup"><span data-stu-id="bc225-122">Create an application</span></span>

<span data-ttu-id="bc225-123">Dále musíte vytvořit aplikaci v adresáři B2C.</span><span class="sxs-lookup"><span data-stu-id="bc225-123">Next, you need to create an app in your B2C directory.</span></span> <span data-ttu-id="bc225-124">Azure AD díky tomu získá informace potřebné k bezpečné komunikaci s vaší aplikací.</span><span class="sxs-lookup"><span data-stu-id="bc225-124">This gives Azure AD information that it needs to communicate securely with your app.</span></span> <span data-ttu-id="bc225-125">Klientská aplikace i webové rozhraní API budou mít stejné **ID aplikace**, protože společně tvoří jednu logickou aplikaci.</span><span class="sxs-lookup"><span data-stu-id="bc225-125">Both the client app and web API will be represented by a single **Application ID**, because they comprise one logical app.</span></span> <span data-ttu-id="bc225-126">Chcete-li vytvořit aplikaci, postupujte podle [těchto pokynů](active-directory-b2c-app-registration.md).</span><span class="sxs-lookup"><span data-stu-id="bc225-126">To create an app, follow [these instructions](active-directory-b2c-app-registration.md).</span></span> <span data-ttu-id="bc225-127">Ujistěte se, že:</span><span class="sxs-lookup"><span data-stu-id="bc225-127">Be sure to:</span></span>

- <span data-ttu-id="bc225-128">Jste do aplikace zahrnuli **webovou aplikaci** / **webové rozhraní API**.</span><span class="sxs-lookup"><span data-stu-id="bc225-128">Include a **web app**/**web API** in the application.</span></span>
- <span data-ttu-id="bc225-129">Jste do pole **Adresa URL odpovědi** vyplnili `http://localhost:3000/auth/openid/return`.</span><span class="sxs-lookup"><span data-stu-id="bc225-129">Enter `http://localhost:3000/auth/openid/return` as a **Reply URL**.</span></span> <span data-ttu-id="bc225-130">To je výchozí URL pro tento příklad.</span><span class="sxs-lookup"><span data-stu-id="bc225-130">It is the default URL for this code sample.</span></span>
- <span data-ttu-id="bc225-131">Vytvořte pro aplikaci **tajný klíč aplikace** a poznamenejte si ho.</span><span class="sxs-lookup"><span data-stu-id="bc225-131">Create an **Application secret** for your application and copy it.</span></span> <span data-ttu-id="bc225-132">Budete ho potřebovat později.</span><span class="sxs-lookup"><span data-stu-id="bc225-132">You will need it later.</span></span> <span data-ttu-id="bc225-133">Před tím, než tuto hodnotu použijete, musí být [uvozena v XML](https://www.w3.org/TR/2006/REC-xml11-20060816/#dt-escape).</span><span class="sxs-lookup"><span data-stu-id="bc225-133">Note that this value needs to be [XML escaped](https://www.w3.org/TR/2006/REC-xml11-20060816/#dt-escape) before you use it.</span></span>
- <span data-ttu-id="bc225-134">Poznamenejte si **ID aplikace** přiřazené vaší aplikaci.</span><span class="sxs-lookup"><span data-stu-id="bc225-134">Copy the **Application ID** that is assigned to your app.</span></span> <span data-ttu-id="bc225-135">To také budete potřebovat později.</span><span class="sxs-lookup"><span data-stu-id="bc225-135">You'll also need this later.</span></span>

[!INCLUDE [active-directory-b2c-devquickstarts-v2-apps](../../includes/active-directory-b2c-devquickstarts-v2-apps.md)]

## <a name="create-your-policies"></a><span data-ttu-id="bc225-136">Vytvořte svoje zásady</span><span class="sxs-lookup"><span data-stu-id="bc225-136">Create your policies</span></span>

<span data-ttu-id="bc225-137">V Azure AD B2C je každé uživatelské rozhraní definováno [zásadou](active-directory-b2c-reference-policies.md).</span><span class="sxs-lookup"><span data-stu-id="bc225-137">In Azure AD B2C, every user experience is defined by a [policy](active-directory-b2c-reference-policies.md).</span></span> <span data-ttu-id="bc225-138">Tato aplikace obsahuje tři činnosti koncového uživatele: registrace, přihlášení a přihlášení pomocí Facebooku.</span><span class="sxs-lookup"><span data-stu-id="bc225-138">This app contains three identity experiences: sign up, sign in, and sign in by using Facebook.</span></span> <span data-ttu-id="bc225-139">Tuto zásadu je třeba vytvořit pro každý typ činnosti, jak je popsáno v [článku o zásadách](active-directory-b2c-reference-policies.md#create-a-sign-up-policy).</span><span class="sxs-lookup"><span data-stu-id="bc225-139">You need to create this policy of each type, as described in the [policy reference article](active-directory-b2c-reference-policies.md#create-a-sign-up-policy).</span></span> <span data-ttu-id="bc225-140">Když vytváříte tyto tři zásady, nezapomeňte:</span><span class="sxs-lookup"><span data-stu-id="bc225-140">When you create your three policies, be sure to:</span></span>

- <span data-ttu-id="bc225-141">Zvolit **Zobrazovaný název** a další atributy registrace ve svojí registrační zásadě.</span><span class="sxs-lookup"><span data-stu-id="bc225-141">Choose the **Display name** and other sign-up attributes in your sign-up policy.</span></span>
- <span data-ttu-id="bc225-142">Zvolit deklarace identity aplikace **Zobrazovaný název** a **ID objektu** v každé zásadě.</span><span class="sxs-lookup"><span data-stu-id="bc225-142">Choose the **Display name** and **Object ID** application claims in every policy.</span></span> <span data-ttu-id="bc225-143">Můžete zvolit i další deklarace identity.</span><span class="sxs-lookup"><span data-stu-id="bc225-143">You can choose other claims as well.</span></span>
- <span data-ttu-id="bc225-144">Po vytvoření každé zásady si poznamenejte její **Název**.</span><span class="sxs-lookup"><span data-stu-id="bc225-144">Copy the **Name** of each policy after you create it.</span></span> <span data-ttu-id="bc225-145">Měl by mít předponu `b2c_1_`.</span><span class="sxs-lookup"><span data-stu-id="bc225-145">It should have the prefix `b2c_1_`.</span></span>  <span data-ttu-id="bc225-146">Tyto názvy zásad budete potřebovat později.</span><span class="sxs-lookup"><span data-stu-id="bc225-146">You'll need these policy names later.</span></span>

[!INCLUDE [active-directory-b2c-devquickstarts-policy](../../includes/active-directory-b2c-devquickstarts-policy.md)]

<span data-ttu-id="bc225-147">Jakmile vytvoříte tyto tři zásady, budete připraveni k sestavení aplikace.</span><span class="sxs-lookup"><span data-stu-id="bc225-147">After you create your three policies, you're ready to build your app.</span></span>

<span data-ttu-id="bc225-148">Tento článek nepopisuje používání právě vytvořených zásad.</span><span class="sxs-lookup"><span data-stu-id="bc225-148">Note that this article does not cover how to use the policies you just created.</span></span> <span data-ttu-id="bc225-149">Chcete-li se dozvědět, jak fungují zásady v Azure AD B2C, začněte [kurzem Začínáme s webovými aplikacemi .NET](active-directory-b2c-devquickstarts-web-dotnet.md).</span><span class="sxs-lookup"><span data-stu-id="bc225-149">To learn about how policies work in Azure AD B2C, start with the [.NET web app getting started tutorial](active-directory-b2c-devquickstarts-web-dotnet.md).</span></span>

## <a name="add-prerequisites-to-your-directory"></a><span data-ttu-id="bc225-150">Přidání požadovaných součástí do adresáře</span><span class="sxs-lookup"><span data-stu-id="bc225-150">Add prerequisites to your directory</span></span>

<span data-ttu-id="bc225-151">V příkazovém řádku přejděte do kořenové složky, pokud v ní ještě nejste.</span><span class="sxs-lookup"><span data-stu-id="bc225-151">From the command line, change directories to your root folder, if you're not already there.</span></span> <span data-ttu-id="bc225-152">Spusťte následující příkazy:</span><span class="sxs-lookup"><span data-stu-id="bc225-152">Run the following commands:</span></span>

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

<span data-ttu-id="bc225-153">Kromě toho jsme pro náhled v naší kostře Rychlého startu použili `passport-azure-ad`.</span><span class="sxs-lookup"><span data-stu-id="bc225-153">In addition, we used `passport-azure-ad` for our preview in the skeleton of the Quickstart.</span></span>

- `npm install passport-azure-ad`

<span data-ttu-id="bc225-154">To nainstaluje knihovny potřebné pro `passport-azure-ad`.</span><span class="sxs-lookup"><span data-stu-id="bc225-154">This will install the libraries that `passport-azure-ad` depends on.</span></span>

## <a name="set-up-your-app-to-use-the-passport-nodejs-strategy"></a><span data-ttu-id="bc225-155">Nastavení aplikace pro použití strategie Passport-Node.js</span><span class="sxs-lookup"><span data-stu-id="bc225-155">Set up your app to use the Passport-Node.js strategy</span></span>
<span data-ttu-id="bc225-156">Nakonfigurujte middleware Express pro použití ověřovacího protokolu OpenID Connect.</span><span class="sxs-lookup"><span data-stu-id="bc225-156">Configure the Express middleware to use the OpenID Connect authentication protocol.</span></span> <span data-ttu-id="bc225-157">Passport bude mimo jiné sloužit k zasílání požadavků na přihlášení a odhlášení, ke správě uživatelských relací a k získávání informací o uživatelích.</span><span class="sxs-lookup"><span data-stu-id="bc225-157">Passport will be used to issue sign-in and sign-out requests, manage user sessions, and get information about users, among other actions.</span></span>

<span data-ttu-id="bc225-158">V kořenovém adresáři projektu otevřete soubor `config.js` a v oddílu `exports.creds` zadejte hodnoty konfigurace.</span><span class="sxs-lookup"><span data-stu-id="bc225-158">Open the `config.js` file in the root of the project and enter your app's configuration values in the `exports.creds` section.</span></span>
- <span data-ttu-id="bc225-159">`clientID`: **ID aplikace** přiřazené vaší aplikaci v portálu registrace.</span><span class="sxs-lookup"><span data-stu-id="bc225-159">`clientID`: The **Application ID** assigned to your app in the registration portal.</span></span>
- <span data-ttu-id="bc225-160">`returnURL`: **Identifikátor URI přesměrování**, který jste zadali v portálu.</span><span class="sxs-lookup"><span data-stu-id="bc225-160">`returnURL`: The **Redirect URI** you entered in the portal.</span></span>
- <span data-ttu-id="bc225-161">`tenantName`: Klientský název vaší aplikace, například **contoso.onmicrosoft.com**.</span><span class="sxs-lookup"><span data-stu-id="bc225-161">`tenantName`: The tenant name of your app, for example, **contoso.onmicrosoft.com**.</span></span>

[!INCLUDE [active-directory-b2c-devquickstarts-tenant-name](../../includes/active-directory-b2c-devquickstarts-tenant-name.md)]

<span data-ttu-id="bc225-162">Otevřete soubor `app.js` umístěný v kořenovém adresáři projektu.</span><span class="sxs-lookup"><span data-stu-id="bc225-162">Open the `app.js` file in the root of the project.</span></span> <span data-ttu-id="bc225-163">Přidejte následující volání pro vyvolání strategie `OIDCStrategy`, která přichází s `passport-azure-ad`.</span><span class="sxs-lookup"><span data-stu-id="bc225-163">Add the following call to invoke the `OIDCStrategy` strategy that comes with `passport-azure-ad`.</span></span>


```JavaScript
var OIDCStrategy = require('passport-azure-ad').OIDCStrategy;

// Add some logging
var log = bunyan.createLogger({
    name: 'Microsoft OIDC Example Web Application'
});
```

<span data-ttu-id="bc225-164">Použijte právě přidanou strategii pro zpracování požadavků na přihlášení.</span><span class="sxs-lookup"><span data-stu-id="bc225-164">Use the strategy you just referenced to handle sign-in requests.</span></span>

```JavaScript
// Use the OIDCStrategy in Passport (Section 2).
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
<span data-ttu-id="bc225-165">Passport používá podobný princip pro všechny strategie (včetně Twitteru a Facebooku).</span><span class="sxs-lookup"><span data-stu-id="bc225-165">Passport uses a similar pattern for all of its strategies (including Twitter and Facebook).</span></span> <span data-ttu-id="bc225-166">Tímto principem se řídí všichni tvůrci strategií.</span><span class="sxs-lookup"><span data-stu-id="bc225-166">All strategy writers adhere to this pattern.</span></span> <span data-ttu-id="bc225-167">Při pohledu na strategii uvidíte, že jí předáváte `function()` s parametry token a `done`.</span><span class="sxs-lookup"><span data-stu-id="bc225-167">When you look at the strategy, you can see that you pass it a `function()` that has a token and a `done` as the parameters.</span></span> <span data-ttu-id="bc225-168">Po dokončení veškeré práce se k vám strategie vrátí.</span><span class="sxs-lookup"><span data-stu-id="bc225-168">The strategy comes back to you after it has done all of its work.</span></span> <span data-ttu-id="bc225-169">Uložení uživatele a skrytí tokenu, takže je nebudete muset požadovat znovu.</span><span class="sxs-lookup"><span data-stu-id="bc225-169">Store the user and stash the token so that you don’t need to ask for it again.</span></span>

> [!IMPORTANT]
<span data-ttu-id="bc225-170">Předchozí kód přijme všechny uživatele, ověřené serverem.</span><span class="sxs-lookup"><span data-stu-id="bc225-170">The preceding code takes all users whom the server authenticates.</span></span> <span data-ttu-id="bc225-171">To je automatická registrace.</span><span class="sxs-lookup"><span data-stu-id="bc225-171">This is autoregistration.</span></span> <span data-ttu-id="bc225-172">Pokud používáte produkční servery, nechcete vpustit uživatele, kteří neprošli vámi nastaveným procesem registrace.</span><span class="sxs-lookup"><span data-stu-id="bc225-172">When you use production servers, you don’t want to let in users unless they have gone through a registration process that you have set up.</span></span> <span data-ttu-id="bc225-173">Tento princip je často k vidění u uživatelských aplikací.</span><span class="sxs-lookup"><span data-stu-id="bc225-173">You can often see this pattern in consumer apps.</span></span> <span data-ttu-id="bc225-174">Ty umožňují registraci pomocí Facebooku, ale poté vás vyzvou k vyplnění dodatečných informací.</span><span class="sxs-lookup"><span data-stu-id="bc225-174">These allow you to register by using Facebook, but then they ask you to fill out additional information.</span></span> <span data-ttu-id="bc225-175">Kdyby naše aplikace nebyla pouze příklad, mohli bychom extrahovat emailovou adresu z vráceného objektu tokenu a poté vyzvat uživatele k vyplnění dodatečných informací.</span><span class="sxs-lookup"><span data-stu-id="bc225-175">If our application wasn’t a sample, we could extract an email address from the token object that is returned, and then ask the user to fill out additional information.</span></span> <span data-ttu-id="bc225-176">Protože se jedná o testovací server, jednoduše přidáme uživatele do databáze v paměti.</span><span class="sxs-lookup"><span data-stu-id="bc225-176">Because this is a test server, we simply add users to the in-memory database.</span></span>

<span data-ttu-id="bc225-177">Přidejte metody, které vám umožní sledovat přihlášené uživatele, jak vyžaduje Passport.</span><span class="sxs-lookup"><span data-stu-id="bc225-177">Add the methods that allow you to keep track of users who have signed in, as required by Passport.</span></span> <span data-ttu-id="bc225-178">To zahrnuje serializaci a deserializaci informací o uživateli:</span><span class="sxs-lookup"><span data-stu-id="bc225-178">This includes serializing and deserializing user information:</span></span>

```JavaScript

// Passport session setup. (Section 2)

//   To support persistent sign-in sessions, Passport needs to be able to
//   serialize users into and deserialize users out of sessions. Typically,
//   this is as simple as storing the user ID when Passport serializes a user
//   and finding the user by ID when Passport deserializes that user.
passport.serializeUser(function(user, done) {
  done(null, user.email);
});

passport.deserializeUser(function(id, done) {
  findByEmail(id, function (err, user) {
    done(err, user);
  });
});

// Array to hold users who have signed in
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

<span data-ttu-id="bc225-179">Přidejte kód pro načtení modulu Express.</span><span class="sxs-lookup"><span data-stu-id="bc225-179">Add the code to load the Express engine.</span></span> <span data-ttu-id="bc225-180">V následujícím příkladu vidíte, že používáme výchozí `/views` a vzor `/routes` poskytované modulem Express.</span><span class="sxs-lookup"><span data-stu-id="bc225-180">In the following, you can see that we use the default `/views` and `/routes` pattern that Express provides.</span></span>

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
  // Initialize Passport!  Also use passport.session() middleware to support
  // persistent sign-in sessions (recommended).
  app.use(passport.initialize());
  app.use(passport.session());
  app.use(app.router);
  app.use(express.static(__dirname + '/../../public'));
});

```

<span data-ttu-id="bc225-181">Přidání tras `POST`, které přebírají vlastní požadavky na přihlášení do modulu `passport-azure-ad`:</span><span class="sxs-lookup"><span data-stu-id="bc225-181">Add the `POST` routes that hand off the actual sign-in requests to the `passport-azure-ad` engine:</span></span>

```JavaScript

// Our Auth routes (Section 3)

// GET /auth/openid
//   Use passport.authenticate() as route middleware to authenticate the
//   request. The first step in OpenID authentication involves redirecting
//   the user to an OpenID provider. After the user is authenticated,
//   the OpenID provider redirects the user back to this application at
//   /auth/openid/return

app.get('/auth/openid',
  passport.authenticate('azuread-openidconnect', { failureRedirect: '/login' }),
  function(req, res) {
    log.info('Authentication was called in the Sample');
    res.redirect('/');
  });

// GET /auth/openid/return
//   Use passport.authenticate() as route middleware to authenticate the
//   request. If authentication fails, the user will be redirected back to the
//   sign-in page. Otherwise, the primary route function will be called.
//   In this example, it redirects the user to the home page.
app.get('/auth/openid/return',
  passport.authenticate('azuread-openidconnect', { failureRedirect: '/login' }),
  function(req, res) {

    res.redirect('/');
  });

// POST /auth/openid/return
//   Use passport.authenticate() as route middleware to authenticate the
//   request. If authentication fails, the user will be redirected back to the
//   sign-in page. Otherwise, the primary route function will be called.
//   In this example, it will redirect the user to the home page.

app.post('/auth/openid/return',
  passport.authenticate('azuread-openidconnect', { failureRedirect: '/login' }),
  function(req, res) {

    res.redirect('/');
  });
```

## <a name="use-passport-to-issue-sign-in-and-sign-out-requests-to-azure-ad"></a><span data-ttu-id="bc225-182">Použití Passportu pro zasílání požadavků na přihlášení a odhlášení do Azure AD</span><span class="sxs-lookup"><span data-stu-id="bc225-182">Use Passport to issue sign-in and sign-out requests to Azure AD</span></span>

<span data-ttu-id="bc225-183">Vaše aplikace je nyní správně nastavená pro komunikaci s koncovým bodem v2.0 pomocí ověřovacího protokolu OpenID Connect.</span><span class="sxs-lookup"><span data-stu-id="bc225-183">Your app is now properly configured to communicate with the v2.0 endpoint by using the OpenID Connect authentication protocol.</span></span> <span data-ttu-id="bc225-184">`passport-azure-ad` se už postaral o podrobnosti ohledně vytváření ověřovacích zpráv, ověřování tokenů z Azure AD a udržování uživatelských relací.</span><span class="sxs-lookup"><span data-stu-id="bc225-184">`passport-azure-ad` has taken care of the details of crafting authentication messages, validating tokens from Azure AD, and maintaining user session.</span></span> <span data-ttu-id="bc225-185">Už jen zbývá umožnit uživatelům přihlášení a odhlášení a sbírat dodatečné informace o přihlášených uživatelích.</span><span class="sxs-lookup"><span data-stu-id="bc225-185">All that remains is to give your users a way to sign in and sign out, and to gather additional information on users who have signed in.</span></span>

<span data-ttu-id="bc225-186">Nejprve do souboru `app.js` přidejte metody výchozí, přihlášení, účet a odhlášení:</span><span class="sxs-lookup"><span data-stu-id="bc225-186">First, add the default, sign-in, account, and sign-out methods to your `app.js` file:</span></span>

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
    log.info('Login was called in the Sample');
    res.redirect('/');
});

app.get('/logout', function(req, res){
  req.logout();
  res.redirect('/');
});

```

<span data-ttu-id="bc225-187">Chcete-li si tyto metody prohlédnout podrobně:</span><span class="sxs-lookup"><span data-stu-id="bc225-187">To review these methods in detail:</span></span>
- <span data-ttu-id="bc225-188">Trasa `/` se přesměruje na zobrazení `index.ejs` předáním uživatele v požadavku (pokud existuje).</span><span class="sxs-lookup"><span data-stu-id="bc225-188">The `/` route redirects to the `index.ejs` view by passing the user in the request (if it exists).</span></span>
- <span data-ttu-id="bc225-189">Trasa `/account` nejprve ověří, zda jste přihlášeni (příklad implementace je níže).</span><span class="sxs-lookup"><span data-stu-id="bc225-189">The `/account` route first verifies that you are authenticated (the implementation for this is below).</span></span> <span data-ttu-id="bc225-190">Poté předá uživatele v požadavku, abyste o něm mohli získat dodatečné informace.</span><span class="sxs-lookup"><span data-stu-id="bc225-190">It then passes the user in the request so that you can get additional information about the user.</span></span>
- <span data-ttu-id="bc225-191">Trasa `/login` zavolá `azuread-openidconnect` Authenticator z `passport-azure-ad`.</span><span class="sxs-lookup"><span data-stu-id="bc225-191">The `/login` route calls the `azuread-openidconnect` authenticator from `passport-azure-ad`.</span></span> <span data-ttu-id="bc225-192">Pokud dojde k neúspěchu, trasa přesměruje uživatele zpět na `/login`.</span><span class="sxs-lookup"><span data-stu-id="bc225-192">If it doesn't succeed, the route redirects the user back to `/login`.</span></span>
- <span data-ttu-id="bc225-193">`/logout` jednoduše volá `logout.ejs` (a jeho trasu).</span><span class="sxs-lookup"><span data-stu-id="bc225-193">`/logout` simply calls `logout.ejs` (and its route).</span></span> <span data-ttu-id="bc225-194">Zruší soubory cookies a poté vrátí uživatele zpět na `index.ejs`.</span><span class="sxs-lookup"><span data-stu-id="bc225-194">This clears cookies and then returns the user back to `index.ejs`.</span></span>


<span data-ttu-id="bc225-195">V poslední části `app.js` přidejte metodu `EnsureAuthenticated`, která je použitá v trase `/account`.</span><span class="sxs-lookup"><span data-stu-id="bc225-195">For the last part of `app.js`, add the `EnsureAuthenticated` method that is used in the `/account` route.</span></span>

```JavaScript

// Simple route middleware to ensure that the user is authenticated. (Section 4)

//   Use this route middleware on any resource that needs to be protected. If
//   the request is authenticated (typically via a persistent sign-in session),
//   then the request will proceed. Otherwise, the user will be redirected to the
//   sign-in page.
function ensureAuthenticated(req, res, next) {
  if (req.isAuthenticated()) { return next(); }
  res.redirect('/login')
}

```

<span data-ttu-id="bc225-196">Nakonec v `app.js` vytvořte samotný server.</span><span class="sxs-lookup"><span data-stu-id="bc225-196">Finally, create the server itself in `app.js`.</span></span>

```JavaScript

app.listen(3000);

```


## <a name="create-the-views-and-routes-in-express-to-call-your-policies"></a><span data-ttu-id="bc225-197">Vytvoření zobrazení a tras v Expressu pro volání zásad</span><span class="sxs-lookup"><span data-stu-id="bc225-197">Create the views and routes in Express to call your policies</span></span>

<span data-ttu-id="bc225-198">Vaše `app.js` je nyní hotová.</span><span class="sxs-lookup"><span data-stu-id="bc225-198">Your `app.js` is now complete.</span></span> <span data-ttu-id="bc225-199">Potřebujete pouze přidat trasy a zobrazení, které vám umožní volat zásady pro přihlášení a odhlášení.</span><span class="sxs-lookup"><span data-stu-id="bc225-199">You just need to add the routes and views that allow you to call the sign-in and sign-up policies.</span></span> <span data-ttu-id="bc225-200">Ty zpracovávají také dříve vytvořené trasy `/logout` a `/login`.</span><span class="sxs-lookup"><span data-stu-id="bc225-200">These also handle the `/logout` and `/login` routes you created.</span></span>

<span data-ttu-id="bc225-201">V kořenovém adresáři vytvořte trasu `/routes/index.js`.</span><span class="sxs-lookup"><span data-stu-id="bc225-201">Create the `/routes/index.js` route under the root directory.</span></span>

```JavaScript

/*
 * GET home page.
 */

exports.index = function(req, res){
  res.render('index', { title: 'Express' });
};
```

<span data-ttu-id="bc225-202">V kořenovém adresáři vytvořte trasu `/routes/user.js`.</span><span class="sxs-lookup"><span data-stu-id="bc225-202">Create the `/routes/user.js` route under the root directory.</span></span>

```JavaScript

/*
 * GET users listing.
 */

exports.list = function(req, res){
  res.send("respond with a resource");
};
```

<span data-ttu-id="bc225-203">Tyto jednoduché trasy předávají požadavky zobrazením.</span><span class="sxs-lookup"><span data-stu-id="bc225-203">These simple routes pass along requests to your views.</span></span> <span data-ttu-id="bc225-204">Obsahují i uživatele, pokud je přítomen.</span><span class="sxs-lookup"><span data-stu-id="bc225-204">They include the user, if one is present.</span></span>

<span data-ttu-id="bc225-205">V kořenovém adresáři vytvořte zobrazení `/views/index.ejs`.</span><span class="sxs-lookup"><span data-stu-id="bc225-205">Create the `/views/index.ejs` view under the root directory.</span></span> <span data-ttu-id="bc225-206">Toto je jednoduchá stránka, která volá zásady pro přihlášení a odhlášení.</span><span class="sxs-lookup"><span data-stu-id="bc225-206">This is a simple page that calls policies for sign-in and sign-out.</span></span> <span data-ttu-id="bc225-207">Můžete ji také použít ke sběru informací o účtu.</span><span class="sxs-lookup"><span data-stu-id="bc225-207">You can also use it to grab account information.</span></span> <span data-ttu-id="bc225-208">Všimněte si, že můžete využít podmínku `if (!user)` při předávání uživatele v požadavku pro prokázání, že je přihlášen.</span><span class="sxs-lookup"><span data-stu-id="bc225-208">Note that you can use the conditional `if (!user)` as the user is passed through in the request to provide evidence that the user is signed in.</span></span>

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

<span data-ttu-id="bc225-209">V kořenovém adresáři vytvořte zobrazení `/views/account.ejs` pro zobrazení dodatečných informací, které `passport-azure-ad` vložil do uživatelského požadavku.</span><span class="sxs-lookup"><span data-stu-id="bc225-209">Create the `/views/account.ejs` view under the root directory so that you can view additional information that `passport-azure-ad` put in the user request.</span></span>

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

<span data-ttu-id="bc225-210">Nyní můžete sestavit a spustit svoji aplikaci.</span><span class="sxs-lookup"><span data-stu-id="bc225-210">You can now build and run your app.</span></span>

<span data-ttu-id="bc225-211">Spusťte `node app.js` a přejděte na `http://localhost:3000`.</span><span class="sxs-lookup"><span data-stu-id="bc225-211">Run `node app.js` and navigate to `http://localhost:3000`</span></span>


<span data-ttu-id="bc225-212">Pomocí emailu nebo Facebooku se zaregistrujte nebo přihlaste k aplikaci.</span><span class="sxs-lookup"><span data-stu-id="bc225-212">Sign up or sign in to the app by using email or Facebook.</span></span> <span data-ttu-id="bc225-213">Odhlaste se a přihlaste se jako jiný uživatel.</span><span class="sxs-lookup"><span data-stu-id="bc225-213">Sign out and sign back in as a different user.</span></span>

##<a name="next-steps"></a><span data-ttu-id="bc225-214">Další kroky</span><span class="sxs-lookup"><span data-stu-id="bc225-214">Next steps</span></span>

<span data-ttu-id="bc225-215">Pro srovnání je hotová ukázka (bez vašich hodnot nastavení) [k dispozici jako soubor .zip](https://github.com/AzureADQuickStarts/B2C-WebApp-OpenIDConnect-NodeJS/archive/complete.zip).</span><span class="sxs-lookup"><span data-stu-id="bc225-215">For reference, the completed sample (without your configuration values) [is provided as a .zip file](https://github.com/AzureADQuickStarts/B2C-WebApp-OpenIDConnect-NodeJS/archive/complete.zip).</span></span> <span data-ttu-id="bc225-216">Můžete ho také klonovat z GitHubu:</span><span class="sxs-lookup"><span data-stu-id="bc225-216">You can also clone it from GitHub:</span></span>

```git clone --branch complete https://github.com/AzureADQuickStarts/B2C-WebApp-OpenIDConnect-nodejs.git```

<span data-ttu-id="bc225-217">Nyní se můžete přesunout k pokročilejším tématům.</span><span class="sxs-lookup"><span data-stu-id="bc225-217">You can now move on to more advanced topics.</span></span> <span data-ttu-id="bc225-218">Můžete vyzkoušet:</span><span class="sxs-lookup"><span data-stu-id="bc225-218">You might try:</span></span>

[<span data-ttu-id="bc225-219">Zabezpečení webového rozhraní API pomocí modelu B2C v Node.js</span><span class="sxs-lookup"><span data-stu-id="bc225-219">Secure a web API by using the B2C model in Node.js</span></span>](active-directory-b2c-devquickstarts-api-node.md)

<!--

For additional resources, check out:
You can now move on to more advanced B2C topics. You might try:

[Call a Node.js web API from a Node.js web app]()

[Customizing the your B2C App's UX >>]()

-->
