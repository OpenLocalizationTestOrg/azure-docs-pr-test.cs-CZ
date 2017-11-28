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
# <a name="azure-ad-b2c-secure-a-web-api-by-using-nodejs"></a><span data-ttu-id="e3ec1-103">Azure AD B2C: Zabezpečení webového rozhraní API pomocí Node.js</span><span class="sxs-lookup"><span data-stu-id="e3ec1-103">Azure AD B2C: Secure a web API by using Node.js</span></span>
<!-- TODO [AZURE.INCLUDE [active-directory-b2c-devquickstarts-web-switcher](../../includes/active-directory-b2c-devquickstarts-web-switcher.md)]-->

<span data-ttu-id="e3ec1-104">S Azure Active Directory (Azure AD) B2C můžete zabezpečit webové rozhraní API pomocí přístupových tokenů OAuth 2.0.</span><span class="sxs-lookup"><span data-stu-id="e3ec1-104">With Azure Active Directory (Azure AD) B2C, you can secure a web API by using OAuth 2.0 access tokens.</span></span> <span data-ttu-id="e3ec1-105">Tyto tokeny umožňují vaší klientské aplikace, které používají Azure AD B2C tooauthenticate toohello API.</span><span class="sxs-lookup"><span data-stu-id="e3ec1-105">These tokens allow your client apps that use Azure AD B2C tooauthenticate toohello API.</span></span> <span data-ttu-id="e3ec1-106">Tento článek ukazuje, jak toocreate rozhraní API "seznam úkolů", umožňuje uživatelům tooadd a seznam úloh.</span><span class="sxs-lookup"><span data-stu-id="e3ec1-106">This article shows you how toocreate a "to-do list" API that allows users tooadd and list tasks.</span></span> <span data-ttu-id="e3ec1-107">Hello webového rozhraní API zabezpečené pomocí Azure AD B2C a pouze umožňuje ověřeným uživatelům toomanage jejich seznamu úkolů.</span><span class="sxs-lookup"><span data-stu-id="e3ec1-107">hello web API is secured using Azure AD B2C and only allows authenticated users toomanage their to-do list.</span></span>

> [!NOTE]
> <span data-ttu-id="e3ec1-108">Tato ukázka byla napsána toobe připojené tooby pomocí našich [ukázkové aplikace iOS B2C](active-directory-b2c-devquickstarts-ios.md).</span><span class="sxs-lookup"><span data-stu-id="e3ec1-108">This sample was written toobe connected tooby using our [iOS B2C sample application](active-directory-b2c-devquickstarts-ios.md).</span></span> <span data-ttu-id="e3ec1-109">Nejprve hello aktuální návodu a pak postupujte podle ukázky.</span><span class="sxs-lookup"><span data-stu-id="e3ec1-109">Do hello current walk-through first, and then follow along with that sample.</span></span>
>
>

<span data-ttu-id="e3ec1-110">**Passport** je ověřovací middleware pro Node.js.</span><span class="sxs-lookup"><span data-stu-id="e3ec1-110">**Passport** is authentication middleware for Node.js.</span></span> <span data-ttu-id="e3ec1-111">Passport je flexibilní a modulární a lze ho snadno nainstalovat v jakékoli webové aplikaci využívající Express nebo Restify.</span><span class="sxs-lookup"><span data-stu-id="e3ec1-111">Flexible and modular, Passport can be unobtrusively installed in any Express-based or Restify web application.</span></span> <span data-ttu-id="e3ec1-112">Komplexní sada strategií podporuje ověřování pomocí uživatelského jména a hesla, Facebooku, Twitteru a dalších.</span><span class="sxs-lookup"><span data-stu-id="e3ec1-112">A comprehensive set of strategies supports authentication by using a user name and password, Facebook, Twitter, and more.</span></span> <span data-ttu-id="e3ec1-113">Vyvinuli jsme strategii pro Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="e3ec1-113">We have developed a strategy for Azure Active Directory (Azure AD).</span></span> <span data-ttu-id="e3ec1-114">Nainstalujete tento modul a poté přidejte hello Azure AD `passport-azure-ad` modulu plug-in.</span><span class="sxs-lookup"><span data-stu-id="e3ec1-114">You install this module and then add hello Azure AD `passport-azure-ad` plug-in.</span></span>

<span data-ttu-id="e3ec1-115">toodo této ukázce, budete muset:</span><span class="sxs-lookup"><span data-stu-id="e3ec1-115">toodo this sample, you need to:</span></span>

1. <span data-ttu-id="e3ec1-116">Zaregistrovat aplikaci s Azure AD.</span><span class="sxs-lookup"><span data-stu-id="e3ec1-116">Register an application with Azure AD.</span></span>
2. <span data-ttu-id="e3ec1-117">Nastavení vaší aplikace toouse Passport je `azure-ad-passport` modulu plug-in.</span><span class="sxs-lookup"><span data-stu-id="e3ec1-117">Set up your application toouse Passport's `azure-ad-passport` plug-in.</span></span>
3. <span data-ttu-id="e3ec1-118">Konfigurace klienta aplikace toocall hello "seznam úkolů" webového rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="e3ec1-118">Configure a client application toocall hello "to-do list" web API.</span></span>

## <a name="get-an-azure-ad-b2c-directory"></a><span data-ttu-id="e3ec1-119">Získání adresáře služby Azure AD B2C</span><span class="sxs-lookup"><span data-stu-id="e3ec1-119">Get an Azure AD B2C directory</span></span>
<span data-ttu-id="e3ec1-120">Před použitím Azure AD B2C musíte vytvořit adresář, nebo klienta.</span><span class="sxs-lookup"><span data-stu-id="e3ec1-120">Before you can use Azure AD B2C, you must create a directory, or tenant.</span></span>  <span data-ttu-id="e3ec1-121">Adresář je kontejner pro všechny uživatele, aplikace, skupiny a další.</span><span class="sxs-lookup"><span data-stu-id="e3ec1-121">A directory is a container for all users, apps, groups, and more.</span></span>  <span data-ttu-id="e3ec1-122">Pokud ho ještě nemáte, [vytvořte adresář B2C](active-directory-b2c-get-started.md) předtím, než budete pokračovat.</span><span class="sxs-lookup"><span data-stu-id="e3ec1-122">If you don't have one already, [create a B2C directory](active-directory-b2c-get-started.md) before you continue.</span></span>

## <a name="create-an-application"></a><span data-ttu-id="e3ec1-123">Vytvoření aplikace</span><span class="sxs-lookup"><span data-stu-id="e3ec1-123">Create an application</span></span>
<span data-ttu-id="e3ec1-124">Dále musíte toocreate aplikace ve svém adresáři B2C, která poskytne Azure AD nějaké informace, že tato služba vyžaduje toosecurely komunikaci s vaší aplikací.</span><span class="sxs-lookup"><span data-stu-id="e3ec1-124">Next, you need toocreate an app in your B2C directory that gives Azure AD some information that it needs toosecurely communicate with your app.</span></span> <span data-ttu-id="e3ec1-125">V takovém případě hello klientská aplikace i webové rozhraní API jsou reprezentované pomocí jedné **ID aplikace**, protože společně tvoří jednu logickou aplikaci.</span><span class="sxs-lookup"><span data-stu-id="e3ec1-125">In this case, both hello client app and web API are represented by a single **Application ID**, because they comprise one logical app.</span></span> <span data-ttu-id="e3ec1-126">toocreate na aplikace, postupujte podle [tyto pokyny](active-directory-b2c-app-registration.md).</span><span class="sxs-lookup"><span data-stu-id="e3ec1-126">toocreate an app, follow [these instructions](active-directory-b2c-app-registration.md).</span></span> <span data-ttu-id="e3ec1-127">Ujistěte se, že:</span><span class="sxs-lookup"><span data-stu-id="e3ec1-127">Be sure to:</span></span>

* <span data-ttu-id="e3ec1-128">Zahrnout **webovou aplikaci nebo webové rozhraní api** v aplikaci hello</span><span class="sxs-lookup"><span data-stu-id="e3ec1-128">Include a **web app/web api** in hello application</span></span>
* <span data-ttu-id="e3ec1-129">Jste do pole **Adresa URL odpovědi** vyplnili `http://localhost/TodoListService`.</span><span class="sxs-lookup"><span data-stu-id="e3ec1-129">Enter `http://localhost/TodoListService` as a **Reply URL**.</span></span> <span data-ttu-id="e3ec1-130">Je hello výchozí adresa URL pro tuto ukázku kódu.</span><span class="sxs-lookup"><span data-stu-id="e3ec1-130">It is hello default URL for this code sample.</span></span>
* <span data-ttu-id="e3ec1-131">Vytvořte pro aplikaci **tajný klíč aplikace** a poznamenejte si ho.</span><span class="sxs-lookup"><span data-stu-id="e3ec1-131">Create an **Application secret** for your application and copy it.</span></span> <span data-ttu-id="e3ec1-132">Tato data budete potřebovat později.</span><span class="sxs-lookup"><span data-stu-id="e3ec1-132">You need this data later.</span></span> <span data-ttu-id="e3ec1-133">Všimněte si, že tato hodnota se musí toobe [uvozena v XML](https://www.w3.org/TR/2006/REC-xml11-20060816/#dt-escape) dříve, než ho použijete.</span><span class="sxs-lookup"><span data-stu-id="e3ec1-133">Note that this value needs toobe [XML escaped](https://www.w3.org/TR/2006/REC-xml11-20060816/#dt-escape) before you use it.</span></span>
* <span data-ttu-id="e3ec1-134">Kopírování hello **ID aplikace** který je přiřazený tooyour aplikace.</span><span class="sxs-lookup"><span data-stu-id="e3ec1-134">Copy hello **Application ID** that is assigned tooyour app.</span></span> <span data-ttu-id="e3ec1-135">Tato data budete potřebovat později.</span><span class="sxs-lookup"><span data-stu-id="e3ec1-135">You need this data later.</span></span>

[!INCLUDE [active-directory-b2c-devquickstarts-v2-apps](../../includes/active-directory-b2c-devquickstarts-v2-apps.md)]

## <a name="create-your-policies"></a><span data-ttu-id="e3ec1-136">Vytvořte svoje zásady</span><span class="sxs-lookup"><span data-stu-id="e3ec1-136">Create your policies</span></span>
<span data-ttu-id="e3ec1-137">V Azure AD B2C je každé uživatelské rozhraní definováno [zásadou](active-directory-b2c-reference-policies.md).</span><span class="sxs-lookup"><span data-stu-id="e3ec1-137">In Azure AD B2C, every user experience is defined by a [policy](active-directory-b2c-reference-policies.md).</span></span> <span data-ttu-id="e3ec1-138">Tato aplikace obsahuje dvě možnosti pro identitu: registraci a přihlášení.</span><span class="sxs-lookup"><span data-stu-id="e3ec1-138">This app contains two identity experiences: sign up and sign in.</span></span> <span data-ttu-id="e3ec1-139">Je třeba jedna zásada toocreate každého typu, jak je popsáno v [článku o zásadách](active-directory-b2c-reference-policies.md#create-a-sign-up-policy).</span><span class="sxs-lookup"><span data-stu-id="e3ec1-139">You need toocreate one policy of each type, as described in the [policy reference article](active-directory-b2c-reference-policies.md#create-a-sign-up-policy).</span></span>  <span data-ttu-id="e3ec1-140">Když vytváříte tyto tři zásady, nezapomeňte:</span><span class="sxs-lookup"><span data-stu-id="e3ec1-140">When you create your three policies, be sure to:</span></span>

* <span data-ttu-id="e3ec1-141">Zvolte hello **zobrazovaný název** a další atributy registrace ve svojí registrační zásadě.</span><span class="sxs-lookup"><span data-stu-id="e3ec1-141">Choose hello **Display name** and other sign-up attributes in your sign-up policy.</span></span>
* <span data-ttu-id="e3ec1-142">Zvolte hello **zobrazovaný název** a **ID objektu** deklarace identity aplikace v každé zásadě.</span><span class="sxs-lookup"><span data-stu-id="e3ec1-142">Choose hello **Display name** and **Object ID** application claims in every policy.</span></span>  <span data-ttu-id="e3ec1-143">Můžete zvolit i další deklarace identity.</span><span class="sxs-lookup"><span data-stu-id="e3ec1-143">You can choose other claims as well.</span></span>
* <span data-ttu-id="e3ec1-144">Poznamenejte hello **název** po jejím vytvoření každé zásady.</span><span class="sxs-lookup"><span data-stu-id="e3ec1-144">Copy down hello **Name** of each policy after you create it.</span></span> <span data-ttu-id="e3ec1-145">Měl by mít předponu hello `b2c_1_`.</span><span class="sxs-lookup"><span data-stu-id="e3ec1-145">It should have hello prefix `b2c_1_`.</span></span>  <span data-ttu-id="e3ec1-146">Tyto názvy zásad budete potřebovat později.</span><span class="sxs-lookup"><span data-stu-id="e3ec1-146">You need those policy names later.</span></span>

[!INCLUDE [active-directory-b2c-devquickstarts-policy](../../includes/active-directory-b2c-devquickstarts-policy.md)]

<span data-ttu-id="e3ec1-147">Po vytvoření tří zásad jste připravené toobuild vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="e3ec1-147">After you have created your three policies, you're ready toobuild your app.</span></span>

<span data-ttu-id="e3ec1-148">toolearn o tom, jak fungují zásady v Azure AD B2C, začněte s hello [webové aplikace kurzem Začínáme .NET](active-directory-b2c-devquickstarts-web-dotnet.md).</span><span class="sxs-lookup"><span data-stu-id="e3ec1-148">toolearn about how policies work in Azure AD B2C, start with hello [.NET web app getting started tutorial](active-directory-b2c-devquickstarts-web-dotnet.md).</span></span>

## <a name="download-hello-code"></a><span data-ttu-id="e3ec1-149">Stáhněte si kód hello</span><span class="sxs-lookup"><span data-stu-id="e3ec1-149">Download hello code</span></span>
<span data-ttu-id="e3ec1-150">Hello kód pro tento kurz [je udržovaný na Githubu](https://github.com/AzureADQuickStarts/B2C-WebAPI-NodeJS).</span><span class="sxs-lookup"><span data-stu-id="e3ec1-150">hello code for this tutorial [is maintained on GitHub](https://github.com/AzureADQuickStarts/B2C-WebAPI-NodeJS).</span></span> <span data-ttu-id="e3ec1-151">Ukázka hello toobuild jako můžete přejít, můžete [stáhnout kostru projektu v souboru ZIP](https://github.com/AzureADQuickStarts/B2C-WebAPI-NodeJS/archive/skeleton.zip).</span><span class="sxs-lookup"><span data-stu-id="e3ec1-151">toobuild hello sample as you go, you can [download a skeleton project as a .zip file](https://github.com/AzureADQuickStarts/B2C-WebAPI-NodeJS/archive/skeleton.zip).</span></span> <span data-ttu-id="e3ec1-152">Můžete také hello kostru klonovat:</span><span class="sxs-lookup"><span data-stu-id="e3ec1-152">You can also clone hello skeleton:</span></span>

```
git clone --branch skeleton https://github.com/AzureADQuickStarts/B2C-WebAPI-NodeJS.git
```

<span data-ttu-id="e3ec1-153">aplikace Hello dokončit, je také [k dispozici jako soubor ZIP](https://github.com/AzureADQuickStarts/B2C-WebAPI-NodeJS/archive/complete.zip) nebo na hello `complete` větve hello stejného úložiště.</span><span class="sxs-lookup"><span data-stu-id="e3ec1-153">hello completed app is also [available as a .zip file](https://github.com/AzureADQuickStarts/B2C-WebAPI-NodeJS/archive/complete.zip) or on hello `complete` branch of hello same repository.</span></span>

## <a name="download-nodejs-for-your-platform"></a><span data-ttu-id="e3ec1-154">Stažení Node.js pro vaši platformu</span><span class="sxs-lookup"><span data-stu-id="e3ec1-154">Download Node.js for your platform</span></span>
<span data-ttu-id="e3ec1-155">toosuccessfully tuto ukázku použít, potřebujete funkční instalací Node.js.</span><span class="sxs-lookup"><span data-stu-id="e3ec1-155">toosuccessfully use this sample, you need a working installation of Node.js.</span></span>

<span data-ttu-id="e3ec1-156">Nainstalujte si Node.js z [nodejs.org](http://nodejs.org).</span><span class="sxs-lookup"><span data-stu-id="e3ec1-156">Install Node.js from [nodejs.org](http://nodejs.org).</span></span>

## <a name="install-mongodb-for-your-platform"></a><span data-ttu-id="e3ec1-157">Instalace MongoDB pro vaši platformu</span><span class="sxs-lookup"><span data-stu-id="e3ec1-157">Install MongoDB for your platform</span></span>
<span data-ttu-id="e3ec1-158">toosuccessfully tuto ukázku použít, potřebujete funkční instalací MongoDB.</span><span class="sxs-lookup"><span data-stu-id="e3ec1-158">toosuccessfully use this sample, you need a working installation of MongoDB.</span></span> <span data-ttu-id="e3ec1-159">MongoDB toomake používáme napříč instancemi serveru trvalé rozhraní REST API.</span><span class="sxs-lookup"><span data-stu-id="e3ec1-159">We use MongoDB toomake your REST API persistent across server instances.</span></span>

<span data-ttu-id="e3ec1-160">Nainstalujte MongoDB z [mongodb.org](http://www.mongodb.org).</span><span class="sxs-lookup"><span data-stu-id="e3ec1-160">Install MongoDB from [mongodb.org](http://www.mongodb.org).</span></span>

> [!NOTE]
> <span data-ttu-id="e3ec1-161">Tento podrobný návod předpokládá, že používáte hello výchozí instalaci a koncové body serveru pro MongoDB, které jsou v době psaní tohoto textu hello je `mongodb://localhost`.</span><span class="sxs-lookup"><span data-stu-id="e3ec1-161">This walk-through assumes that you use hello default installation and server endpoints for MongoDB, which at hello time of this writing is `mongodb://localhost`.</span></span>
>
>

## <a name="install-hello-restify-modules-in-your-web-api"></a><span data-ttu-id="e3ec1-162">Instalace modulů Restify hello ve vašem webovém rozhraní API</span><span class="sxs-lookup"><span data-stu-id="e3ec1-162">Install hello Restify modules in your web API</span></span>
<span data-ttu-id="e3ec1-163">Restify toobuild používáme REST API.</span><span class="sxs-lookup"><span data-stu-id="e3ec1-163">We use Restify toobuild your REST API.</span></span> <span data-ttu-id="e3ec1-164">Restify je minimalistické a flexibilní rozhraní Node.js odvozené z Express.</span><span class="sxs-lookup"><span data-stu-id="e3ec1-164">Restify is a minimal and flexible Node.js application framework derived from Express.</span></span> <span data-ttu-id="e3ec1-165">Obsahuje robustní sadu funkcí pro sestavování rozhraní REST API postavených na protokolu Connect.</span><span class="sxs-lookup"><span data-stu-id="e3ec1-165">It has a robust set of features for building REST APIs on top of Connect.</span></span>

### <a name="install-restify"></a><span data-ttu-id="e3ec1-166">Instalace Restify</span><span class="sxs-lookup"><span data-stu-id="e3ec1-166">Install Restify</span></span>
<span data-ttu-id="e3ec1-167">Hello příkazovém řádku přejděte do adresáře příliš`azuread`.</span><span class="sxs-lookup"><span data-stu-id="e3ec1-167">From hello command line, change your directory too`azuread`.</span></span> <span data-ttu-id="e3ec1-168">Pokud hello `azuread` adresář neexistuje, vytvořte ho.</span><span class="sxs-lookup"><span data-stu-id="e3ec1-168">If hello `azuread` directory doesn't exist, create it.</span></span>

<span data-ttu-id="e3ec1-169">`cd azuread` nebo `mkdir azuread;`</span><span class="sxs-lookup"><span data-stu-id="e3ec1-169">`cd azuread` or `mkdir azuread;`</span></span>

<span data-ttu-id="e3ec1-170">Zadejte hello následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="e3ec1-170">Enter hello following command:</span></span>

`npm install restify`

<span data-ttu-id="e3ec1-171">Tento příkaz nainstaluje Restify.</span><span class="sxs-lookup"><span data-stu-id="e3ec1-171">This command installs Restify.</span></span>

#### <a name="did-you-get-an-error"></a><span data-ttu-id="e3ec1-172">Obdrželi jste chybu?</span><span class="sxs-lookup"><span data-stu-id="e3ec1-172">Did you get an error?</span></span>
<span data-ttu-id="e3ec1-173">V některých operačních systémech, při použití `npm`, může se zobrazit chyba hello `Error: EPERM, chmod '/usr/local/bin/..'` a žádost o hello účet Spustit jako správce.</span><span class="sxs-lookup"><span data-stu-id="e3ec1-173">In some operating systems, when you use `npm`, you may receive hello error `Error: EPERM, chmod '/usr/local/bin/..'` and a request that you run hello account as an administrator.</span></span> <span data-ttu-id="e3ec1-174">Pokud vznikne tento problém používat hello `sudo` příkaz toorun `npm` na vyšší úrovni oprávnění.</span><span class="sxs-lookup"><span data-stu-id="e3ec1-174">If this problem occurs, use hello `sudo` command toorun `npm` at a higher privilege level.</span></span>

#### <a name="did-you-get-a-dtrace-error"></a><span data-ttu-id="e3ec1-175">Obdrželi jste chybu DTrace?</span><span class="sxs-lookup"><span data-stu-id="e3ec1-175">Did you get a DTrace error?</span></span>
<span data-ttu-id="e3ec1-176">Při instalaci Restify se může zobrazit podobný text:</span><span class="sxs-lookup"><span data-stu-id="e3ec1-176">You may see something like this text when you install Restify:</span></span>

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

<span data-ttu-id="e3ec1-177">Restify poskytuje výkonný mechanismus pro trasování volání REST pomocí DTrace.</span><span class="sxs-lookup"><span data-stu-id="e3ec1-177">Restify provides a powerful mechanism for tracing REST calls by using DTrace.</span></span> <span data-ttu-id="e3ec1-178">Nicméně, na mnoha operačních systémech není DTrace k dispozici.</span><span class="sxs-lookup"><span data-stu-id="e3ec1-178">However, many operating systems do not have DTrace available.</span></span> <span data-ttu-id="e3ec1-179">Tyto chyby můžete bezpečně ignorovat.</span><span class="sxs-lookup"><span data-stu-id="e3ec1-179">You can safely ignore these errors.</span></span>

<span data-ttu-id="e3ec1-180">výstup Hello hello příkazu by měla vypadat podobně jako toothis text:</span><span class="sxs-lookup"><span data-stu-id="e3ec1-180">hello output of hello command should appear similar toothis text:</span></span>

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

## <a name="install-passport-in-your-web-api"></a><span data-ttu-id="e3ec1-181">Instalace Passportu ve vašem webovém rozhraní API</span><span class="sxs-lookup"><span data-stu-id="e3ec1-181">Install Passport in your web API</span></span>
<span data-ttu-id="e3ec1-182">Hello příkazovém řádku přejděte do adresáře příliš`azuread`, pokud ještě není.</span><span class="sxs-lookup"><span data-stu-id="e3ec1-182">From hello command line, change your directory too`azuread`, if it's not already there.</span></span>

<span data-ttu-id="e3ec1-183">Instalace Passportu pomocí hello následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="e3ec1-183">Install Passport using hello following command:</span></span>

`npm install passport`

<span data-ttu-id="e3ec1-184">Hello výstup hello příkazu by měl vypadat podobně jako toothis text:</span><span class="sxs-lookup"><span data-stu-id="e3ec1-184">hello output of hello command should be similar toothis text:</span></span>

    passport@0.1.17 node_modules\passport
    ├── pause@0.0.1
    └── pkginfo@0.2.3

## <a name="add-passport-azuread-tooyour-web-api"></a><span data-ttu-id="e3ec1-185">Přidání passport-azuread tooyour webového rozhraní API</span><span class="sxs-lookup"><span data-stu-id="e3ec1-185">Add passport-azuread tooyour web API</span></span>
<span data-ttu-id="e3ec1-186">V dalším kroku přidejte strategii OAuth hello pomocí `passport-azuread`, sady strategií, které propojují Azure AD s Passport.</span><span class="sxs-lookup"><span data-stu-id="e3ec1-186">Next, add hello OAuth strategy by using `passport-azuread`, a suite of strategies that connect Azure AD with Passport.</span></span> <span data-ttu-id="e3ec1-187">Použijte tuto strategii pro nosné tokeny v ukázce REST API hello.</span><span class="sxs-lookup"><span data-stu-id="e3ec1-187">Use this strategy for bearer tokens in hello REST API sample.</span></span>

> [!NOTE]
> <span data-ttu-id="e3ec1-188">Přestože OAuth2 poskytuje rozhraní, ve kterém mohou být vydávané všechny známé typy tokenů, rozšířeného využití se dostalo pouze určitým typům tokenů.</span><span class="sxs-lookup"><span data-stu-id="e3ec1-188">Although OAuth2 provides a framework in which any known token type can be issued, only certain token types have gained widespread use.</span></span> <span data-ttu-id="e3ec1-189">Hello tokeny pro ochranu koncových bodů jsou nosné tokeny.</span><span class="sxs-lookup"><span data-stu-id="e3ec1-189">hello tokens for protecting endpoints are bearer tokens.</span></span> <span data-ttu-id="e3ec1-190">Tyto typy tokenů jsou hello nejčastěji vydané v OAuth2.</span><span class="sxs-lookup"><span data-stu-id="e3ec1-190">These types of tokens are hello most widely issued in OAuth2.</span></span> <span data-ttu-id="e3ec1-191">Mnoho implementací předpokládá, že jsou nosné tokeny jediným typem vydávaných tokenů hello.</span><span class="sxs-lookup"><span data-stu-id="e3ec1-191">Many implementations assume that bearer tokens are hello only type of token issued.</span></span>
>
>

<span data-ttu-id="e3ec1-192">Hello příkazovém řádku přejděte do adresáře příliš`azuread`, pokud ještě není.</span><span class="sxs-lookup"><span data-stu-id="e3ec1-192">From hello command line, change your directory too`azuread`, if it's not already there.</span></span>

<span data-ttu-id="e3ec1-193">Nainstalujte hello Passport `passport-azure-ad` modulu pomocí hello následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="e3ec1-193">Install hello Passport `passport-azure-ad` module using hello following command:</span></span>

`npm install passport-azure-ad`

<span data-ttu-id="e3ec1-194">Hello výstup hello příkazu by měl vypadat podobně jako toothis text:</span><span class="sxs-lookup"><span data-stu-id="e3ec1-194">hello output of hello command should be similar toothis text:</span></span>

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

## <a name="add-mongodb-modules-tooyour-web-api"></a><span data-ttu-id="e3ec1-195">Přidání modulů MongoDB tooyour rozhraní web API</span><span class="sxs-lookup"><span data-stu-id="e3ec1-195">Add MongoDB modules tooyour web API</span></span>
<span data-ttu-id="e3ec1-196">V této ukázce se jako úložiště dat používá MongoDB.</span><span class="sxs-lookup"><span data-stu-id="e3ec1-196">This sample uses MongoDB as your data store.</span></span> <span data-ttu-id="e3ec1-197">Pro tento účel nainstalujte Mongoose, což je běžně používaný modul plug-in pro správu modelů a schémat.</span><span class="sxs-lookup"><span data-stu-id="e3ec1-197">For that install Mongoose, a widely used plug-in for managing models and schemas.</span></span>

* `npm install mongoose`

## <a name="install-additional-modules"></a><span data-ttu-id="e3ec1-198">Instalace dalších modulů</span><span class="sxs-lookup"><span data-stu-id="e3ec1-198">Install additional modules</span></span>
<span data-ttu-id="e3ec1-199">Dále nainstalujte zbývající požadované moduly hello.</span><span class="sxs-lookup"><span data-stu-id="e3ec1-199">Next, install hello remaining required modules.</span></span>

<span data-ttu-id="e3ec1-200">Hello příkazovém řádku přejděte do adresáře příliš`azuread`, pokud ještě není:</span><span class="sxs-lookup"><span data-stu-id="e3ec1-200">From hello command line, change your directory too`azuread`, if it's not already there:</span></span>

`cd azuread`

<span data-ttu-id="e3ec1-201">Instalace modulů hello ve vaší `node_modules` directory:</span><span class="sxs-lookup"><span data-stu-id="e3ec1-201">Install hello modules in your `node_modules` directory:</span></span>

* `npm install assert-plus`
* `npm install ejs`
* `npm install ejs-locals`
* `npm install express`
* `npm install bunyan`

## <a name="create-a-serverjs-file-with-your-dependencies"></a><span data-ttu-id="e3ec1-202">Vytvoření souboru server.js se závislostmi</span><span class="sxs-lookup"><span data-stu-id="e3ec1-202">Create a server.js file with your dependencies</span></span>
<span data-ttu-id="e3ec1-203">Hello `server.js` soubor poskytuje hello většina hello funkce pro server webového rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="e3ec1-203">hello `server.js` file provides hello majority of hello functionality for your Web API server.</span></span>

<span data-ttu-id="e3ec1-204">Hello příkazovém řádku přejděte do adresáře příliš`azuread`, pokud ještě není:</span><span class="sxs-lookup"><span data-stu-id="e3ec1-204">From hello command line, change your directory too`azuread`, if it's not already there:</span></span>

`cd azuread`

<span data-ttu-id="e3ec1-205">V editoru vytvořte soubor `server.js`.</span><span class="sxs-lookup"><span data-stu-id="e3ec1-205">Create a `server.js` file in an editor.</span></span> <span data-ttu-id="e3ec1-206">Přidejte hello následující informace:</span><span class="sxs-lookup"><span data-stu-id="e3ec1-206">Add hello following information:</span></span>

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

<span data-ttu-id="e3ec1-207">Uložte soubor hello.</span><span class="sxs-lookup"><span data-stu-id="e3ec1-207">Save hello file.</span></span> <span data-ttu-id="e3ec1-208">Vrátí tooit později.</span><span class="sxs-lookup"><span data-stu-id="e3ec1-208">You return tooit later.</span></span>

## <a name="create-a-configjs-file-toostore-your-azure-ad-settings"></a><span data-ttu-id="e3ec1-209">Vytvoření toostore souboru config.js nastavení Azure AD</span><span class="sxs-lookup"><span data-stu-id="e3ec1-209">Create a config.js file toostore your Azure AD settings</span></span>
<span data-ttu-id="e3ec1-210">Tento soubor s kódem předává parametry konfigurace hello z vaší portálu Azure AD toohello `Passport.js` souboru.</span><span class="sxs-lookup"><span data-stu-id="e3ec1-210">This code file passes hello configuration parameters from your Azure AD Portal toohello `Passport.js` file.</span></span> <span data-ttu-id="e3ec1-211">Tyto hodnoty konfigurace jste vytvořili, když jste přidali hello webové rozhraní API toohello portálu v první části návodu hello hello.</span><span class="sxs-lookup"><span data-stu-id="e3ec1-211">You created these configuration values when you added hello web API toohello portal in hello first part of hello walk-through.</span></span> <span data-ttu-id="e3ec1-212">Po zkopírování hello kód objasníme, jaké tooput v hello hodnoty těchto parametrů.</span><span class="sxs-lookup"><span data-stu-id="e3ec1-212">We explain what tooput in hello values of these parameters after you copy hello code.</span></span>

<span data-ttu-id="e3ec1-213">Hello příkazovém řádku přejděte do adresáře příliš`azuread`, pokud ještě není:</span><span class="sxs-lookup"><span data-stu-id="e3ec1-213">From hello command line, change your directory too`azuread`, if it's not already there:</span></span>

`cd azuread`

<span data-ttu-id="e3ec1-214">V editoru vytvořte soubor `config.js`.</span><span class="sxs-lookup"><span data-stu-id="e3ec1-214">Create a `config.js` file in an editor.</span></span> <span data-ttu-id="e3ec1-215">Přidejte hello následující informace:</span><span class="sxs-lookup"><span data-stu-id="e3ec1-215">Add hello following information:</span></span>

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

### <a name="required-values"></a><span data-ttu-id="e3ec1-216">Požadované hodnoty</span><span class="sxs-lookup"><span data-stu-id="e3ec1-216">Required values</span></span>
<span data-ttu-id="e3ec1-217">`clientID`: hello ID klienta aplikace webového rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="e3ec1-217">`clientID`: hello client ID of your Web API application.</span></span>

<span data-ttu-id="e3ec1-218">`IdentityMetadata`: Tady bude `passport-azure-ad` vyhledá konfigurační data pro zprostředkovatele identity hello.</span><span class="sxs-lookup"><span data-stu-id="e3ec1-218">`IdentityMetadata`: This is where `passport-azure-ad` looks for your configuration data for hello identity provider.</span></span> <span data-ttu-id="e3ec1-219">Vypadá to také pro webové tokeny hello JSON toovalidate hello klíče.</span><span class="sxs-lookup"><span data-stu-id="e3ec1-219">It also looks for hello keys toovalidate hello JSON web tokens.</span></span>

<span data-ttu-id="e3ec1-220">`audience`: identifikátor URI (URI) z hello portálu, který identifikuje volající aplikace hello.</span><span class="sxs-lookup"><span data-stu-id="e3ec1-220">`audience`: hello uniform resource identifier (URI) from hello portal that identifies your calling application.</span></span>

<span data-ttu-id="e3ec1-221">`tenantName`: Název vašeho klienta (například **contoso.onmicrosoft.com**).</span><span class="sxs-lookup"><span data-stu-id="e3ec1-221">`tenantName`: Your tenant name (for example, **contoso.onmicrosoft.com**).</span></span>

<span data-ttu-id="e3ec1-222">`policyName`: hello zásady, které chcete toovalidate hello tokenů přicházejících tooyour serveru.</span><span class="sxs-lookup"><span data-stu-id="e3ec1-222">`policyName`: hello policy that you want toovalidate hello tokens coming in tooyour server.</span></span> <span data-ttu-id="e3ec1-223">Tato zásada by měla být hello stejné zásady, které používáte pro přihlášení na hello klientské aplikace.</span><span class="sxs-lookup"><span data-stu-id="e3ec1-223">This policy should be hello same policy that you use on hello client application for sign-in.</span></span>

> [!NOTE]
> <span data-ttu-id="e3ec1-224">Prozatím použijte hello stejné zásady v nastavení klient i server.</span><span class="sxs-lookup"><span data-stu-id="e3ec1-224">For now, use hello same policies across both client and server setup.</span></span> <span data-ttu-id="e3ec1-225">Pokud jste již dokončili návod a tyto zásady vytvořili, nemusíte toodo proto znovu.</span><span class="sxs-lookup"><span data-stu-id="e3ec1-225">If you have already completed a walk-through and created these policies, you don't need toodo so again.</span></span> <span data-ttu-id="e3ec1-226">Vzhledem k tomu, že jste dokončili návod hello, neměli byste potřebovat tooset si nové zásady u návodů pro klienty v lokalitě hello.</span><span class="sxs-lookup"><span data-stu-id="e3ec1-226">Because you completed hello walk-through, you shouldn't need tooset up new policies for client walk-throughs on hello site.</span></span>
>
>

## <a name="add-configuration-tooyour-serverjs-file"></a><span data-ttu-id="e3ec1-227">Přidání konfiguračního souboru server.js tooyour</span><span class="sxs-lookup"><span data-stu-id="e3ec1-227">Add configuration tooyour server.js file</span></span>
<span data-ttu-id="e3ec1-228">tooread hello hodnoty z hello `config.js` souboru, kterou jste vytvořili, přidejte hello `.config` souboru jako požadovaný prostředek vaší aplikace a pak nastavte globální proměnné toothose hello v hello `config.js` dokumentu.</span><span class="sxs-lookup"><span data-stu-id="e3ec1-228">tooread hello values from hello `config.js` file you created, add hello `.config` file as a required resource in your application, and then set hello global variables toothose in hello `config.js` document.</span></span>

<span data-ttu-id="e3ec1-229">Hello příkazovém řádku přejděte do adresáře příliš`azuread`, pokud ještě není:</span><span class="sxs-lookup"><span data-stu-id="e3ec1-229">From hello command line, change your directory too`azuread`, if it's not already there:</span></span>

`cd azuread`

<span data-ttu-id="e3ec1-230">Otevřete hello `server.js` souboru v editoru.</span><span class="sxs-lookup"><span data-stu-id="e3ec1-230">Open hello `server.js` file in an editor.</span></span> <span data-ttu-id="e3ec1-231">Přidejte hello následující informace:</span><span class="sxs-lookup"><span data-stu-id="e3ec1-231">Add hello following information:</span></span>

```Javascript
var config = require('./config');
```
<span data-ttu-id="e3ec1-232">Přidání nové části příliš`server.js` hello následující kód, který obsahuje:</span><span class="sxs-lookup"><span data-stu-id="e3ec1-232">Add a new section too`server.js` that includes hello following code:</span></span>

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

<span data-ttu-id="e3ec1-233">V dalším kroku přidejme některé zástupné symboly pro uživatele hello obdržíme od našich volání aplikací.</span><span class="sxs-lookup"><span data-stu-id="e3ec1-233">Next, let's add some placeholders for hello users we receive from our calling applications.</span></span>

```Javascript
// array toohold logged in users and hello current logged in user (owner)
var users = [];
var owner = null;
```

<span data-ttu-id="e3ec1-234">Nyní pokračujme a vytvořme i protokolovací nástroj.</span><span class="sxs-lookup"><span data-stu-id="e3ec1-234">Let's go ahead and create our logger too.</span></span>

```Javascript
// Our logger
var log = bunyan.createLogger({
    name: 'Microsoft Azure Active Directory Sample'
});
```

## <a name="add-hello-mongodb-model-and-schema-information-by-using-mongoose"></a><span data-ttu-id="e3ec1-235">Přidat hello informace modelu a schématu MongoDB pomocí Mongoose</span><span class="sxs-lookup"><span data-stu-id="e3ec1-235">Add hello MongoDB model and schema information by using Mongoose</span></span>
<span data-ttu-id="e3ec1-236">Hello předchozí Příprava platí přineste tyto tři soubory dohromady ve službu REST API.</span><span class="sxs-lookup"><span data-stu-id="e3ec1-236">hello earlier preparation pays off as you bring these three files together in a REST API service.</span></span>

<span data-ttu-id="e3ec1-237">Pro tento návod použijte MongoDB toostore úkolů, jak je popsáno výše.</span><span class="sxs-lookup"><span data-stu-id="e3ec1-237">For this walk-through, use MongoDB toostore your tasks, as discussed earlier.</span></span>

<span data-ttu-id="e3ec1-238">V hello `config.js` souboru, jste nazvali databázi **tasklist**.</span><span class="sxs-lookup"><span data-stu-id="e3ec1-238">In hello `config.js` file, you called your database **tasklist**.</span></span> <span data-ttu-id="e3ec1-239">Tento název se taky uveďte na konci hello hello `mongoose_auth_local` adresy URL pro připojení.</span><span class="sxs-lookup"><span data-stu-id="e3ec1-239">That name was also what you put at hello end of hello `mongoose_auth_local` connection URL.</span></span> <span data-ttu-id="e3ec1-240">Nepotřebujete toocreate tato databáze předem v MongoDB.</span><span class="sxs-lookup"><span data-stu-id="e3ec1-240">You don't need toocreate this database beforehand in MongoDB.</span></span> <span data-ttu-id="e3ec1-241">Vytvoří hello databáze pro vás při hello prvního spuštění vaší serverové aplikace.</span><span class="sxs-lookup"><span data-stu-id="e3ec1-241">It creates hello database for you on hello first run of your server application.</span></span>

<span data-ttu-id="e3ec1-242">Poté, co sdělíte serveru hello které toouse databázi MongoDB, musíte toowrite některé další kód toocreate hello modelu a schématu pro serverové úlohy.</span><span class="sxs-lookup"><span data-stu-id="e3ec1-242">After you tell hello server which MongoDB database toouse, you need toowrite some additional code toocreate hello model and schema for your server tasks.</span></span>

### <a name="expand-hello-model"></a><span data-ttu-id="e3ec1-243">Rozbalte položku modelu hello</span><span class="sxs-lookup"><span data-stu-id="e3ec1-243">Expand hello model</span></span>
<span data-ttu-id="e3ec1-244">Tento model schématu je jednoduchý.</span><span class="sxs-lookup"><span data-stu-id="e3ec1-244">This schema model is simple.</span></span> <span data-ttu-id="e3ec1-245">Podle potřeby ho můžete rozšířit.</span><span class="sxs-lookup"><span data-stu-id="e3ec1-245">You can expand it as required.</span></span>

<span data-ttu-id="e3ec1-246">`owner`: Kdo je přiřazen toohello úloh.</span><span class="sxs-lookup"><span data-stu-id="e3ec1-246">`owner`: Who is assigned toohello task.</span></span> <span data-ttu-id="e3ec1-247">Tento objekt je typu **string**.</span><span class="sxs-lookup"><span data-stu-id="e3ec1-247">This object is a **string**.</span></span>  

<span data-ttu-id="e3ec1-248">`Text`: vlastní úloha hello.</span><span class="sxs-lookup"><span data-stu-id="e3ec1-248">`Text`: hello task itself.</span></span> <span data-ttu-id="e3ec1-249">Tento objekt je typu **string**.</span><span class="sxs-lookup"><span data-stu-id="e3ec1-249">This object is a **string**.</span></span>

<span data-ttu-id="e3ec1-250">`date`: data hello je kvůli tuto úlohu hello.</span><span class="sxs-lookup"><span data-stu-id="e3ec1-250">`date`: hello date that hello task is due.</span></span> <span data-ttu-id="e3ec1-251">Tento objekt je typu **datetime**.</span><span class="sxs-lookup"><span data-stu-id="e3ec1-251">This object is a **datetime**.</span></span>

<span data-ttu-id="e3ec1-252">`completed`: Pokud hello úkol je dokončen.</span><span class="sxs-lookup"><span data-stu-id="e3ec1-252">`completed`: If hello task is complete.</span></span> <span data-ttu-id="e3ec1-253">Tento objekt je typu **Boolean**.</span><span class="sxs-lookup"><span data-stu-id="e3ec1-253">This object is a **Boolean**.</span></span>

### <a name="create-hello-schema-in-hello-code"></a><span data-ttu-id="e3ec1-254">Vytvoření hello schématu v kódu hello</span><span class="sxs-lookup"><span data-stu-id="e3ec1-254">Create hello schema in hello code</span></span>
<span data-ttu-id="e3ec1-255">Hello příkazovém řádku přejděte do adresáře příliš`azuread`, pokud ještě není:</span><span class="sxs-lookup"><span data-stu-id="e3ec1-255">From hello command line, change your directory too`azuread`, if it's not already there:</span></span>

`cd azuread`

<span data-ttu-id="e3ec1-256">Otevřete hello `server.js` souboru v editoru.</span><span class="sxs-lookup"><span data-stu-id="e3ec1-256">Open hello `server.js` file in an editor.</span></span> <span data-ttu-id="e3ec1-257">Přidejte následující informace pod položku konfigurace hello hello:</span><span class="sxs-lookup"><span data-stu-id="e3ec1-257">Add hello following information below hello configuration entry:</span></span>

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
<span data-ttu-id="e3ec1-258">Nejprve vytvoříte schéma hello a pak vytvoříte objekt modelu, který používáte toostore dat napříč hello kódu při definování vaše **trasy**.</span><span class="sxs-lookup"><span data-stu-id="e3ec1-258">You first create hello schema, and then you create a model object that you use toostore your data throughout hello code when you define your **routes**.</span></span>

## <a name="add-routes-for-your-rest-api-task-server"></a><span data-ttu-id="e3ec1-259">Přidání tras pro serveru úloh REST API</span><span class="sxs-lookup"><span data-stu-id="e3ec1-259">Add routes for your REST API task server</span></span>
<span data-ttu-id="e3ec1-260">Teď, když máte toowork modelu databázi s, přidejte hello trasy, který použijete pro svůj server REST API.</span><span class="sxs-lookup"><span data-stu-id="e3ec1-260">Now that you have a database model toowork with, add hello routes you use for your REST API server.</span></span>

### <a name="about-routes-in-restify"></a><span data-ttu-id="e3ec1-261">O trasách v Restify</span><span class="sxs-lookup"><span data-stu-id="e3ec1-261">About routes in Restify</span></span>
<span data-ttu-id="e3ec1-262">Trasy v Restify ve fungovat hello stejné jako pracují při použití balíku Express hello.</span><span class="sxs-lookup"><span data-stu-id="e3ec1-262">Routes work in Restify in hello same way that they work when they use hello Express stack.</span></span> <span data-ttu-id="e3ec1-263">Trasy se definují pomocí identifikátoru URI, které předpokládáte hello klienta aplikace toocall hello.</span><span class="sxs-lookup"><span data-stu-id="e3ec1-263">You define routes by using hello URI that you expect hello client applications toocall.</span></span>

<span data-ttu-id="e3ec1-264">Typický vzor trasy Restify je:</span><span class="sxs-lookup"><span data-stu-id="e3ec1-264">A typical pattern for a Restify route is:</span></span>

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

<span data-ttu-id="e3ec1-265">Restify a Express poskytují mnohem hlubší funkčnost, jako například definování typů aplikací a provádění komplexního trasování napříč různými koncovými body.</span><span class="sxs-lookup"><span data-stu-id="e3ec1-265">Restify and Express can provide much deeper functionality, such as defining application types and doing complex routing across different endpoints.</span></span> <span data-ttu-id="e3ec1-266">Pro účely tohoto kurzu hello jsme zjednodušení tyto trasy.</span><span class="sxs-lookup"><span data-stu-id="e3ec1-266">For hello purposes of this tutorial, we keep these routes simple.</span></span>

#### <a name="add-default-routes-tooyour-server"></a><span data-ttu-id="e3ec1-267">Přidat výchozí trasy tooyour server</span><span class="sxs-lookup"><span data-stu-id="e3ec1-267">Add default routes tooyour server</span></span>
<span data-ttu-id="e3ec1-268">Nyní přidáte hello základní trasy CRUD **vytvořit** a **seznamu** pro naše REST API.</span><span class="sxs-lookup"><span data-stu-id="e3ec1-268">You now add hello basic CRUD routes of **create** and **list** for our REST API.</span></span> <span data-ttu-id="e3ec1-269">Dalším postupům lze nalézt v hello `complete` větve hello ukázky.</span><span class="sxs-lookup"><span data-stu-id="e3ec1-269">Other routes can be found in hello `complete` branch of hello sample.</span></span>

<span data-ttu-id="e3ec1-270">Hello příkazovém řádku přejděte do adresáře příliš`azuread`, pokud ještě není:</span><span class="sxs-lookup"><span data-stu-id="e3ec1-270">From hello command line, change your directory too`azuread`, if it's not already there:</span></span>

`cd azuread`

<span data-ttu-id="e3ec1-271">Otevřete hello `server.js` souboru v editoru.</span><span class="sxs-lookup"><span data-stu-id="e3ec1-271">Open hello `server.js` file in an editor.</span></span> <span data-ttu-id="e3ec1-272">Pod položky databáze hello provedené vyšší přidat hello následující informace:</span><span class="sxs-lookup"><span data-stu-id="e3ec1-272">Below hello database entries you made above add hello following information:</span></span>

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


#### <a name="add-error-handling-for-hello-routes"></a><span data-ttu-id="e3ec1-273">Přidání zpracování chyb pro trasy hello</span><span class="sxs-lookup"><span data-stu-id="e3ec1-273">Add error handling for hello routes</span></span>
<span data-ttu-id="e3ec1-274">Přidáte nějaké zpracování chyb, aby mohl komunikovat potíže narazíte back toohello klienta tak, že můžete porozumět.</span><span class="sxs-lookup"><span data-stu-id="e3ec1-274">Add some error handling so that you can communicate any problems you encounter back toohello client in a way that it can understand.</span></span>

<span data-ttu-id="e3ec1-275">Přidejte následující kód hello:</span><span class="sxs-lookup"><span data-stu-id="e3ec1-275">Add hello following code:</span></span>

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


## <a name="create-your-server"></a><span data-ttu-id="e3ec1-276">Vytvoření serveru</span><span class="sxs-lookup"><span data-stu-id="e3ec1-276">Create your server</span></span>
<span data-ttu-id="e3ec1-277">Nyní jste definovali databázi a přidali trasy.</span><span class="sxs-lookup"><span data-stu-id="e3ec1-277">You have now defined your database and put your routes in place.</span></span> <span data-ttu-id="e3ec1-278">Hello poslední věcí, kterou jste toodo je instance serveru hello tooadd, která spravuje vaše volání.</span><span class="sxs-lookup"><span data-stu-id="e3ec1-278">hello last thing for you toodo is tooadd hello server instance that manages your calls.</span></span>

<span data-ttu-id="e3ec1-279">Restify a Express poskytují široké možnosti přizpůsobení serveru REST API, ale tady používáme hello nejzákladnější nastavení.</span><span class="sxs-lookup"><span data-stu-id="e3ec1-279">Restify and Express provide deep customization for a REST API server, but we use hello most basic setup here.</span></span>

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
## <a name="add-hello-routes-toohello-server-without-authentication"></a><span data-ttu-id="e3ec1-280">Přidání serveru toohello hello tras (bez ověřování)</span><span class="sxs-lookup"><span data-stu-id="e3ec1-280">Add hello routes toohello server (without authentication)</span></span>
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

## <a name="add-authentication-tooyour-rest-api-server"></a><span data-ttu-id="e3ec1-281">Přidat server REST API tooyour ověřování</span><span class="sxs-lookup"><span data-stu-id="e3ec1-281">Add authentication tooyour REST API server</span></span>
<span data-ttu-id="e3ec1-282">Když teď máte fungující server REST API, můžete ho využít s Azure AD.</span><span class="sxs-lookup"><span data-stu-id="e3ec1-282">Now that you have a running REST API server, you can make it useful against Azure AD.</span></span>

<span data-ttu-id="e3ec1-283">Hello příkazovém řádku přejděte do adresáře příliš`azuread`, pokud ještě není:</span><span class="sxs-lookup"><span data-stu-id="e3ec1-283">From hello command line, change your directory too`azuread`, if it's not already there:</span></span>

`cd azuread`

### <a name="use-hello-oidcbearerstrategy-that-is-included-with-passport-azure-ad"></a><span data-ttu-id="e3ec1-284">Hello použití OIDCBearerStrategy, která je součástí passport-azure-ad</span><span class="sxs-lookup"><span data-stu-id="e3ec1-284">Use hello OIDCBearerStrategy that is included with passport-azure-ad</span></span>
> [!TIP]
> <span data-ttu-id="e3ec1-285">Při psaní rozhraní API, byste vždy měli propojit hello data toosomething jedinečné z hello token, který hello uživatel nemůže zfalšovat.</span><span class="sxs-lookup"><span data-stu-id="e3ec1-285">When you write APIs, you should always link hello data toosomething unique from hello token that hello user can’t spoof.</span></span> <span data-ttu-id="e3ec1-286">Pokud hello server ukládá položky úkolů, nebude proto podle hello **oid** hello uživatele v hello tokenu (zavolaném prostřednictvím token.oid), které je třeba do pole hello "vlastník".</span><span class="sxs-lookup"><span data-stu-id="e3ec1-286">When hello server stores ToDo items, it does so based on hello **oid** of hello user in hello token (called through token.oid), which goes in hello “owner” field.</span></span> <span data-ttu-id="e3ec1-287">Tato hodnota zajišťuje, že pouze tento uživatel bude mít přístup ke svým vlastním položkám ToDo.</span><span class="sxs-lookup"><span data-stu-id="e3ec1-287">This value ensures that only that user can access their own ToDo items.</span></span> <span data-ttu-id="e3ec1-288">Není nijak neprojevuje v hello rozhraní API "vlastník", takže externí uživatel může požadovat položkám úkolů jiných uživatelů, i když jsou ověřeni.</span><span class="sxs-lookup"><span data-stu-id="e3ec1-288">There is no exposure in hello API of “owner,” so an external user can request others’ ToDo items even if they are authenticated.</span></span>
>
>

<span data-ttu-id="e3ec1-289">Dále použijte nosnou strategii hello, která se dodává s `passport-azure-ad`.</span><span class="sxs-lookup"><span data-stu-id="e3ec1-289">Next, use hello bearer strategy that comes with `passport-azure-ad`.</span></span>

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

<span data-ttu-id="e3ec1-290">Passport používá hello stejný vzor pro všechny svoje strategie.</span><span class="sxs-lookup"><span data-stu-id="e3ec1-290">Passport uses hello same pattern for all its strategies.</span></span> <span data-ttu-id="e3ec1-291">Předáváte jí `function()`, která jako parametry přijímá `token` a `done`.</span><span class="sxs-lookup"><span data-stu-id="e3ec1-291">You pass it a `function()` that has `token` and `done` as parameters.</span></span> <span data-ttu-id="e3ec1-292">Hello strategie vrátí tooyou po dokončení veškeré práce.</span><span class="sxs-lookup"><span data-stu-id="e3ec1-292">hello strategy comes back tooyou after it has done all of its work.</span></span> <span data-ttu-id="e3ec1-293">By pak uložte hello uživatele a uložit hello token, abyste tooask pro ni není nutné znovu.</span><span class="sxs-lookup"><span data-stu-id="e3ec1-293">You should then store hello user and save hello token so that you don’t need tooask for it again.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="e3ec1-294">výše uvedený kód Hello přijímá každý uživatel, který se stane tooauthenticate tooyour serveru.</span><span class="sxs-lookup"><span data-stu-id="e3ec1-294">hello code above takes any user who happens tooauthenticate tooyour server.</span></span> <span data-ttu-id="e3ec1-295">Tento proces se nazývá automatická registrace.</span><span class="sxs-lookup"><span data-stu-id="e3ec1-295">This process is known as autoregistration.</span></span> <span data-ttu-id="e3ec1-296">Na produkčních serverech Nenechte v jakéhokoli rozhraní API hello přístup uživatelé bez toho, aby předtím prošli registračním procesem.</span><span class="sxs-lookup"><span data-stu-id="e3ec1-296">In production servers, don't let in any users access hello API without first having them go through a registration process.</span></span> <span data-ttu-id="e3ec1-297">Tento proces je obvykle hello vzor, který můžete vidět u uživatelských aplikací, které umožňují tooregister pomocí Facebooku, ale poté vás požádají toofill Další informace.</span><span class="sxs-lookup"><span data-stu-id="e3ec1-297">This process is usually hello pattern you see in consumer apps that allow you tooregister by using Facebook but then ask you toofill out additional information.</span></span> <span data-ttu-id="e3ec1-298">Pokud tento program nebyl příkazového řádku programu, mohli bychom extrahovat hello e-mailu z hello tokenu objektu, který je vrácen a poté požádat uživatele toofill Další informace.</span><span class="sxs-lookup"><span data-stu-id="e3ec1-298">If this program wasn’t a command-line program, we could have extracted hello email from hello token object that is returned and then asked users toofill out additional information.</span></span> <span data-ttu-id="e3ec1-299">Protože to je ukázka, přidáme je tooan databáze v paměti.</span><span class="sxs-lookup"><span data-stu-id="e3ec1-299">Because this is a sample, we add them tooan in-memory database.</span></span>
>
>

## <a name="run-your-server-application-tooverify-that-it-rejects-you"></a><span data-ttu-id="e3ec1-300">Spuštění vaší aplikace tooverify serveru to vás odmítne</span><span class="sxs-lookup"><span data-stu-id="e3ec1-300">Run your server application tooverify that it rejects you</span></span>
<span data-ttu-id="e3ec1-301">Můžete použít `curl` toosee, pokud máte nyní chráněné pomocí OAuth2 koncové body.</span><span class="sxs-lookup"><span data-stu-id="e3ec1-301">You can use `curl` toosee if you now have OAuth2 protection against your endpoints.</span></span> <span data-ttu-id="e3ec1-302">Hello hlavičky vrátil by měl být dostatek tootell jste, že jste na správné cestě hello.</span><span class="sxs-lookup"><span data-stu-id="e3ec1-302">hello headers returned should be enough tootell you that you are on hello right path.</span></span>

<span data-ttu-id="e3ec1-303">Ujistěte se, že je spuštěna instance MongoDB.</span><span class="sxs-lookup"><span data-stu-id="e3ec1-303">Make sure that your MongoDB instance is running:</span></span>

    $sudo mongodb

<span data-ttu-id="e3ec1-304">Změňte adresář toohello a spuštění hello serveru:</span><span class="sxs-lookup"><span data-stu-id="e3ec1-304">Change toohello directory and run hello server:</span></span>

    $ cd azuread
    $ node server.js

<span data-ttu-id="e3ec1-305">V novém okně terminálu spusťte příkaz `curl`</span><span class="sxs-lookup"><span data-stu-id="e3ec1-305">In a new terminal window, run `curl`</span></span>

<span data-ttu-id="e3ec1-306">Zkuste základní požadavek POST:</span><span class="sxs-lookup"><span data-stu-id="e3ec1-306">Try a basic POST:</span></span>

`$ curl -isS -X POST http://127.0.0.1:3000/api/tasks/brandon/Hello`

```Shell
HTTP/1.1 401 Unauthorized
Connection: close
WWW-Authenticate: Bearer realm="Users"
Date: Tue, 14 Jul 2015 05:45:03 GMT
Transfer-Encoding: chunked
```

<span data-ttu-id="e3ec1-307">Chyba 401 je reakce na hello.</span><span class="sxs-lookup"><span data-stu-id="e3ec1-307">A 401 error is hello response you want.</span></span> <span data-ttu-id="e3ec1-308">Označuje, že vrstva Passportu hello se pokouší tooredirect toohello zajistí autorizaci koncového bodu.</span><span class="sxs-lookup"><span data-stu-id="e3ec1-308">It indicates that hello Passport layer is trying tooredirect toohello authorize endpoint.</span></span>

## <a name="you-now-have-a-rest-api-service-that-uses-oauth2"></a><span data-ttu-id="e3ec1-309">Nyní máte službu REST API, která používá OAuth2</span><span class="sxs-lookup"><span data-stu-id="e3ec1-309">You now have a REST API service that uses OAuth2</span></span>
<span data-ttu-id="e3ec1-310">Implementovali jste rozhraní REST API s použitím Restify a OAuth!</span><span class="sxs-lookup"><span data-stu-id="e3ec1-310">You have implemented a REST API by using Restify and OAuth!</span></span> <span data-ttu-id="e3ec1-311">Nyní máte dostatečný kód, aby mohli pokračovat toodevelop služby a na tomto příkladu stavět.</span><span class="sxs-lookup"><span data-stu-id="e3ec1-311">You now have sufficient code so that you can continue toodevelop your service and build on this example.</span></span> <span data-ttu-id="e3ec1-312">S tímto serverem jste došli nejdál, co to jde bez použití klienta kompatibilního s OAuth2.</span><span class="sxs-lookup"><span data-stu-id="e3ec1-312">You have gone as far as you can with this server without using an OAuth2-compatible client.</span></span> <span data-ttu-id="e3ec1-313">Tento další krok použít další návod jako naše [připojit tooa webového rozhraní API pomocí iOS s B2C](active-directory-b2c-devquickstarts-ios.md) návod.</span><span class="sxs-lookup"><span data-stu-id="e3ec1-313">For that next step use an additional walk-through like our [Connect tooa web API by using iOS with B2C](active-directory-b2c-devquickstarts-ios.md) walkthrough.</span></span>

## <a name="next-steps"></a><span data-ttu-id="e3ec1-314">Další kroky</span><span class="sxs-lookup"><span data-stu-id="e3ec1-314">Next steps</span></span>
<span data-ttu-id="e3ec1-315">Nyní se můžete přesunout toomore advanced témata, jako například:</span><span class="sxs-lookup"><span data-stu-id="e3ec1-315">You can now move toomore advanced topics, such as:</span></span>

[<span data-ttu-id="e3ec1-316">Připojit tooa webového rozhraní API pomocí iOS s B2C</span><span class="sxs-lookup"><span data-stu-id="e3ec1-316">Connect tooa web API by using iOS with B2C</span></span>](active-directory-b2c-devquickstarts-ios.md)
