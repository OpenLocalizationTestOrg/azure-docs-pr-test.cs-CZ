---
title: "Azure AD B2C: Zabezpečení webového rozhraní API pomocí Node.js | Dokumentace Microsoftu"
description: "Jak sestavit webové rozhraní API Node.js, které přijímá tokeny z klienta B2C"
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
ms.openlocfilehash: 6480be75c314ede1b786e959a79c0385dd2edea8
ms.sourcegitcommit: 73f159cdbc122ffe42f3e1f7a3de05f77b6a4725
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/28/2017
---
# <a name="azure-ad-b2c-secure-a-web-api-by-using-nodejs"></a><span data-ttu-id="f4ce0-103">Azure AD B2C: Zabezpečení webového rozhraní API pomocí Node.js</span><span class="sxs-lookup"><span data-stu-id="f4ce0-103">Azure AD B2C: Secure a web API by using Node.js</span></span>
<!-- TODO [AZURE.INCLUDE [active-directory-b2c-devquickstarts-web-switcher](../../includes/active-directory-b2c-devquickstarts-web-switcher.md)]-->

<span data-ttu-id="f4ce0-104">S Azure Active Directory (Azure AD) B2C můžete zabezpečit webové rozhraní API pomocí přístupových tokenů OAuth 2.0.</span><span class="sxs-lookup"><span data-stu-id="f4ce0-104">With Azure Active Directory (Azure AD) B2C, you can secure a web API by using OAuth 2.0 access tokens.</span></span> <span data-ttu-id="f4ce0-105">Tyto tokeny umožňují ověření přístupu klientských aplikací, které používají Azure AD B2C, do rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="f4ce0-105">These tokens allow your client apps that use Azure AD B2C to authenticate to the API.</span></span> <span data-ttu-id="f4ce0-106">Tento článek ukazuje, jak vytvořit rozhraní API „seznam úkolů“, které umožňuje uživatelům přidávat úkoly a vypisovat jejich seznam.</span><span class="sxs-lookup"><span data-stu-id="f4ce0-106">This article shows you how to create a "to-do list" API that allows users to add and list tasks.</span></span> <span data-ttu-id="f4ce0-107">Webové rozhraní API je zabezpečeno pomocí Azure AD B2C a umožňuje pouze ověřeným uživatelům spravovat své seznamy úkolů.</span><span class="sxs-lookup"><span data-stu-id="f4ce0-107">The web API is secured using Azure AD B2C and only allows authenticated users to manage their to-do list.</span></span>

> [!NOTE]
> <span data-ttu-id="f4ce0-108">Tato ukázka byla napsána pro připojení pomocí naší [ukázkové aplikace iOS B2C](active-directory-b2c-devquickstarts-ios.md).</span><span class="sxs-lookup"><span data-stu-id="f4ce0-108">This sample was written to be connected to by using our [iOS B2C sample application](active-directory-b2c-devquickstarts-ios.md).</span></span> <span data-ttu-id="f4ce0-109">Nejprve pokračujte podle aktuálního návodu a poté postupujte podle příslušné ukázky.</span><span class="sxs-lookup"><span data-stu-id="f4ce0-109">Do the current walk-through first, and then follow along with that sample.</span></span>
>
>

<span data-ttu-id="f4ce0-110">**Passport** je ověřovací middleware pro Node.js.</span><span class="sxs-lookup"><span data-stu-id="f4ce0-110">**Passport** is authentication middleware for Node.js.</span></span> <span data-ttu-id="f4ce0-111">Passport je flexibilní a modulární a lze ho snadno nainstalovat v jakékoli webové aplikaci využívající Express nebo Restify.</span><span class="sxs-lookup"><span data-stu-id="f4ce0-111">Flexible and modular, Passport can be unobtrusively installed in any Express-based or Restify web application.</span></span> <span data-ttu-id="f4ce0-112">Komplexní sada strategií podporuje ověřování pomocí uživatelského jména a hesla, Facebooku, Twitteru a dalších.</span><span class="sxs-lookup"><span data-stu-id="f4ce0-112">A comprehensive set of strategies supports authentication by using a user name and password, Facebook, Twitter, and more.</span></span> <span data-ttu-id="f4ce0-113">Vyvinuli jsme strategii pro Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="f4ce0-113">We have developed a strategy for Azure Active Directory (Azure AD).</span></span> <span data-ttu-id="f4ce0-114">Nainstalujte tento modul a poté přidejte modul plug-in Azure AD `passport-azure-ad`.</span><span class="sxs-lookup"><span data-stu-id="f4ce0-114">You install this module and then add the Azure AD `passport-azure-ad` plug-in.</span></span>

<span data-ttu-id="f4ce0-115">Chcete-li postupovat podle této ukázky, budete muset:</span><span class="sxs-lookup"><span data-stu-id="f4ce0-115">To do this sample, you need to:</span></span>

1. <span data-ttu-id="f4ce0-116">Zaregistrovat aplikaci s Azure AD.</span><span class="sxs-lookup"><span data-stu-id="f4ce0-116">Register an application with Azure AD.</span></span>
2. <span data-ttu-id="f4ce0-117">Nastavit aplikaci pro používání modulu plug-in `azure-ad-passport` Passportu.</span><span class="sxs-lookup"><span data-stu-id="f4ce0-117">Set up your application to use Passport's `azure-ad-passport` plug-in.</span></span>
3. <span data-ttu-id="f4ce0-118">Nakonfigurovat klientskou aplikaci, aby volala webové rozhraní API „seznam úkolů“.</span><span class="sxs-lookup"><span data-stu-id="f4ce0-118">Configure a client application to call the "to-do list" web API.</span></span>

## <a name="get-an-azure-ad-b2c-directory"></a><span data-ttu-id="f4ce0-119">Získání adresáře služby Azure AD B2C</span><span class="sxs-lookup"><span data-stu-id="f4ce0-119">Get an Azure AD B2C directory</span></span>
<span data-ttu-id="f4ce0-120">Před použitím Azure AD B2C musíte vytvořit adresář, nebo klienta.</span><span class="sxs-lookup"><span data-stu-id="f4ce0-120">Before you can use Azure AD B2C, you must create a directory, or tenant.</span></span>  <span data-ttu-id="f4ce0-121">Adresář je kontejner pro všechny uživatele, aplikace, skupiny a další.</span><span class="sxs-lookup"><span data-stu-id="f4ce0-121">A directory is a container for all users, apps, groups, and more.</span></span>  <span data-ttu-id="f4ce0-122">Pokud ho ještě nemáte, [vytvořte adresář B2C](active-directory-b2c-get-started.md) předtím, než budete pokračovat.</span><span class="sxs-lookup"><span data-stu-id="f4ce0-122">If you don't have one already, [create a B2C directory](active-directory-b2c-get-started.md) before you continue.</span></span>

## <a name="create-an-application"></a><span data-ttu-id="f4ce0-123">Vytvoření aplikace</span><span class="sxs-lookup"><span data-stu-id="f4ce0-123">Create an application</span></span>
<span data-ttu-id="f4ce0-124">Dále musíte ve svém adresáři B2C vytvořit aplikaci, která poskytne Azure AD informace potřebné k bezpečné komunikaci s vaší aplikací.</span><span class="sxs-lookup"><span data-stu-id="f4ce0-124">Next, you need to create an app in your B2C directory that gives Azure AD some information that it needs to securely communicate with your app.</span></span> <span data-ttu-id="f4ce0-125">V tomto případě mají klientská aplikace i webové rozhraní API stejné **ID aplikace**, protože společně tvoří jednu logickou aplikaci.</span><span class="sxs-lookup"><span data-stu-id="f4ce0-125">In this case, both the client app and web API are represented by a single **Application ID**, because they comprise one logical app.</span></span> <span data-ttu-id="f4ce0-126">Chcete-li vytvořit aplikaci, postupujte podle [těchto pokynů](active-directory-b2c-app-registration.md).</span><span class="sxs-lookup"><span data-stu-id="f4ce0-126">To create an app, follow [these instructions](active-directory-b2c-app-registration.md).</span></span> <span data-ttu-id="f4ce0-127">Ujistěte se, že:</span><span class="sxs-lookup"><span data-stu-id="f4ce0-127">Be sure to:</span></span>

* <span data-ttu-id="f4ce0-128">Jste do aplikace zahrnuli **webovou aplikaci nebo webové rozhraní API**.</span><span class="sxs-lookup"><span data-stu-id="f4ce0-128">Include a **web app/web api** in the application</span></span>
* <span data-ttu-id="f4ce0-129">Jste do pole **Adresa URL odpovědi** vyplnili `http://localhost/TodoListService`.</span><span class="sxs-lookup"><span data-stu-id="f4ce0-129">Enter `http://localhost/TodoListService` as a **Reply URL**.</span></span> <span data-ttu-id="f4ce0-130">To je výchozí URL pro tento příklad.</span><span class="sxs-lookup"><span data-stu-id="f4ce0-130">It is the default URL for this code sample.</span></span>
* <span data-ttu-id="f4ce0-131">Vytvořte pro aplikaci **tajný klíč aplikace** a poznamenejte si ho.</span><span class="sxs-lookup"><span data-stu-id="f4ce0-131">Create an **Application secret** for your application and copy it.</span></span> <span data-ttu-id="f4ce0-132">Tato data budete potřebovat později.</span><span class="sxs-lookup"><span data-stu-id="f4ce0-132">You need this data later.</span></span> <span data-ttu-id="f4ce0-133">Před tím, než tuto hodnotu použijete, musí být [uvozena v XML](https://www.w3.org/TR/2006/REC-xml11-20060816/#dt-escape).</span><span class="sxs-lookup"><span data-stu-id="f4ce0-133">Note that this value needs to be [XML escaped](https://www.w3.org/TR/2006/REC-xml11-20060816/#dt-escape) before you use it.</span></span>
* <span data-ttu-id="f4ce0-134">Poznamenejte si **ID aplikace** přiřazené vaší aplikaci.</span><span class="sxs-lookup"><span data-stu-id="f4ce0-134">Copy the **Application ID** that is assigned to your app.</span></span> <span data-ttu-id="f4ce0-135">Tato data budete potřebovat později.</span><span class="sxs-lookup"><span data-stu-id="f4ce0-135">You need this data later.</span></span>

[!INCLUDE [active-directory-b2c-devquickstarts-v2-apps](../../includes/active-directory-b2c-devquickstarts-v2-apps.md)]

## <a name="create-your-policies"></a><span data-ttu-id="f4ce0-136">Vytvořte svoje zásady</span><span class="sxs-lookup"><span data-stu-id="f4ce0-136">Create your policies</span></span>
<span data-ttu-id="f4ce0-137">V Azure AD B2C je každé uživatelské rozhraní definováno [zásadou](active-directory-b2c-reference-policies.md).</span><span class="sxs-lookup"><span data-stu-id="f4ce0-137">In Azure AD B2C, every user experience is defined by a [policy](active-directory-b2c-reference-policies.md).</span></span> <span data-ttu-id="f4ce0-138">Tato aplikace obsahuje dvě možnosti pro identitu: registraci a přihlášení.</span><span class="sxs-lookup"><span data-stu-id="f4ce0-138">This app contains two identity experiences: sign up and sign in.</span></span> <span data-ttu-id="f4ce0-139">Pro každý typ rozhraní musíte vytvořit zásadu, jak je popsáno v [článku o zásadách](active-directory-b2c-reference-policies.md#create-a-sign-up-policy).</span><span class="sxs-lookup"><span data-stu-id="f4ce0-139">You need to create one policy of each type, as described in the [policy reference article](active-directory-b2c-reference-policies.md#create-a-sign-up-policy).</span></span>  <span data-ttu-id="f4ce0-140">Když vytváříte tyto tři zásady, nezapomeňte:</span><span class="sxs-lookup"><span data-stu-id="f4ce0-140">When you create your three policies, be sure to:</span></span>

* <span data-ttu-id="f4ce0-141">Zvolit **Zobrazovaný název** a další atributy registrace ve svojí registrační zásadě.</span><span class="sxs-lookup"><span data-stu-id="f4ce0-141">Choose the **Display name** and other sign-up attributes in your sign-up policy.</span></span>
* <span data-ttu-id="f4ce0-142">Zvolit deklarace identity aplikace **Zobrazovaný název** a **ID objektu** v každé zásadě.</span><span class="sxs-lookup"><span data-stu-id="f4ce0-142">Choose the **Display name** and **Object ID** application claims in every policy.</span></span>  <span data-ttu-id="f4ce0-143">Můžete zvolit i další deklarace identity.</span><span class="sxs-lookup"><span data-stu-id="f4ce0-143">You can choose other claims as well.</span></span>
* <span data-ttu-id="f4ce0-144">Po vytvoření každé zásady si poznamenejte její **Název**.</span><span class="sxs-lookup"><span data-stu-id="f4ce0-144">Copy down the **Name** of each policy after you create it.</span></span> <span data-ttu-id="f4ce0-145">Měl by mít předponu `b2c_1_`.</span><span class="sxs-lookup"><span data-stu-id="f4ce0-145">It should have the prefix `b2c_1_`.</span></span>  <span data-ttu-id="f4ce0-146">Tyto názvy zásad budete potřebovat později.</span><span class="sxs-lookup"><span data-stu-id="f4ce0-146">You need those policy names later.</span></span>

[!INCLUDE [active-directory-b2c-devquickstarts-policy](../../includes/active-directory-b2c-devquickstarts-policy.md)]

<span data-ttu-id="f4ce0-147">Po vytvoření těchto tří zásad jste připraveni k sestavení aplikace.</span><span class="sxs-lookup"><span data-stu-id="f4ce0-147">After you have created your three policies, you're ready to build your app.</span></span>

<span data-ttu-id="f4ce0-148">Chcete-li se dozvědět, jak fungují zásady v Azure AD B2C, začněte [kurzem Začínáme s webovými aplikacemi .NET](active-directory-b2c-devquickstarts-web-dotnet.md).</span><span class="sxs-lookup"><span data-stu-id="f4ce0-148">To learn about how policies work in Azure AD B2C, start with the [.NET web app getting started tutorial](active-directory-b2c-devquickstarts-web-dotnet.md).</span></span>

## <a name="download-the-code"></a><span data-ttu-id="f4ce0-149">Stáhněte si kód</span><span class="sxs-lookup"><span data-stu-id="f4ce0-149">Download the code</span></span>
<span data-ttu-id="f4ce0-150">Kód k tomuto kurzu [je udržovaný na GitHubu](https://github.com/AzureADQuickStarts/B2C-WebAPI-NodeJS).</span><span class="sxs-lookup"><span data-stu-id="f4ce0-150">The code for this tutorial [is maintained on GitHub](https://github.com/AzureADQuickStarts/B2C-WebAPI-NodeJS).</span></span> <span data-ttu-id="f4ce0-151">Chcete-li během čtení tohoto návodu rovnou sestavit ukázku, můžete si [stáhnout kostru projektu v souboru ZIP](https://github.com/AzureADQuickStarts/B2C-WebAPI-NodeJS/archive/skeleton.zip).</span><span class="sxs-lookup"><span data-stu-id="f4ce0-151">To build the sample as you go, you can [download a skeleton project as a .zip file](https://github.com/AzureADQuickStarts/B2C-WebAPI-NodeJS/archive/skeleton.zip).</span></span> <span data-ttu-id="f4ce0-152">Kostru můžete také klonovat:</span><span class="sxs-lookup"><span data-stu-id="f4ce0-152">You can also clone the skeleton:</span></span>

```
git clone --branch skeleton https://github.com/AzureADQuickStarts/B2C-WebAPI-NodeJS.git
```

<span data-ttu-id="f4ce0-153">Dokončená aplikace je také [k dispozici jako soubor ZIP](https://github.com/AzureADQuickStarts/B2C-WebAPI-NodeJS/archive/complete.zip) nebo ve větvi `complete` stejného úložiště.</span><span class="sxs-lookup"><span data-stu-id="f4ce0-153">The completed app is also [available as a .zip file](https://github.com/AzureADQuickStarts/B2C-WebAPI-NodeJS/archive/complete.zip) or on the `complete` branch of the same repository.</span></span>

## <a name="download-nodejs-for-your-platform"></a><span data-ttu-id="f4ce0-154">Stažení Node.js pro vaši platformu</span><span class="sxs-lookup"><span data-stu-id="f4ce0-154">Download Node.js for your platform</span></span>
<span data-ttu-id="f4ce0-155">Pro úspěšné fungování této ukázky je třeba mít funkční instalaci Node.js.</span><span class="sxs-lookup"><span data-stu-id="f4ce0-155">To successfully use this sample, you need a working installation of Node.js.</span></span>

<span data-ttu-id="f4ce0-156">Nainstalujte si Node.js z [nodejs.org](http://nodejs.org).</span><span class="sxs-lookup"><span data-stu-id="f4ce0-156">Install Node.js from [nodejs.org](http://nodejs.org).</span></span>

## <a name="install-mongodb-for-your-platform"></a><span data-ttu-id="f4ce0-157">Instalace MongoDB pro vaši platformu</span><span class="sxs-lookup"><span data-stu-id="f4ce0-157">Install MongoDB for your platform</span></span>
<span data-ttu-id="f4ce0-158">Pro úspěšné fungování této ukázky je třeba mít funkční instalaci MongoDB.</span><span class="sxs-lookup"><span data-stu-id="f4ce0-158">To successfully use this sample, you need a working installation of MongoDB.</span></span> <span data-ttu-id="f4ce0-159">MongoDB budeme používat k zajištění trvalosti REST API napříč instancemi serveru.</span><span class="sxs-lookup"><span data-stu-id="f4ce0-159">We use MongoDB to make your REST API persistent across server instances.</span></span>

<span data-ttu-id="f4ce0-160">Nainstalujte MongoDB z [mongodb.org](http://www.mongodb.org).</span><span class="sxs-lookup"><span data-stu-id="f4ce0-160">Install MongoDB from [mongodb.org](http://www.mongodb.org).</span></span>

> [!NOTE]
> <span data-ttu-id="f4ce0-161">Tento návod předpokládá, že používáte výchozí instalaci a koncové body serveru pro MongoDB, což je v době psaní tohoto návodu `mongodb://localhost`.</span><span class="sxs-lookup"><span data-stu-id="f4ce0-161">This walk-through assumes that you use the default installation and server endpoints for MongoDB, which at the time of this writing is `mongodb://localhost`.</span></span>
>
>

## <a name="install-the-restify-modules-in-your-web-api"></a><span data-ttu-id="f4ce0-162">Instalace modulů Restify ve webovém rozhraní API</span><span class="sxs-lookup"><span data-stu-id="f4ce0-162">Install the Restify modules in your web API</span></span>
<span data-ttu-id="f4ce0-163">Restify použijeme k sestavení REST API.</span><span class="sxs-lookup"><span data-stu-id="f4ce0-163">We use Restify to build your REST API.</span></span> <span data-ttu-id="f4ce0-164">Restify je minimalistické a flexibilní rozhraní Node.js odvozené z Express.</span><span class="sxs-lookup"><span data-stu-id="f4ce0-164">Restify is a minimal and flexible Node.js application framework derived from Express.</span></span> <span data-ttu-id="f4ce0-165">Obsahuje robustní sadu funkcí pro sestavování rozhraní REST API postavených na protokolu Connect.</span><span class="sxs-lookup"><span data-stu-id="f4ce0-165">It has a robust set of features for building REST APIs on top of Connect.</span></span>

### <a name="install-restify"></a><span data-ttu-id="f4ce0-166">Instalace Restify</span><span class="sxs-lookup"><span data-stu-id="f4ce0-166">Install Restify</span></span>
<span data-ttu-id="f4ce0-167">V příkazovém řádku přejděte do adresáře `azuread`.</span><span class="sxs-lookup"><span data-stu-id="f4ce0-167">From the command line, change your directory to `azuread`.</span></span> <span data-ttu-id="f4ce0-168">Pokud adresář `azuread` neexistuje, vytvořte ho.</span><span class="sxs-lookup"><span data-stu-id="f4ce0-168">If the `azuread` directory doesn't exist, create it.</span></span>

<span data-ttu-id="f4ce0-169">`cd azuread` nebo `mkdir azuread;`</span><span class="sxs-lookup"><span data-stu-id="f4ce0-169">`cd azuread` or `mkdir azuread;`</span></span>

<span data-ttu-id="f4ce0-170">Zadejte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="f4ce0-170">Enter the following command:</span></span>

`npm install restify`

<span data-ttu-id="f4ce0-171">Tento příkaz nainstaluje Restify.</span><span class="sxs-lookup"><span data-stu-id="f4ce0-171">This command installs Restify.</span></span>

#### <a name="did-you-get-an-error"></a><span data-ttu-id="f4ce0-172">Obdrželi jste chybu?</span><span class="sxs-lookup"><span data-stu-id="f4ce0-172">Did you get an error?</span></span>
<span data-ttu-id="f4ce0-173">Při použití příkazu `npm` můžete v některých operačních systémech obdržet chybu `Error: EPERM, chmod '/usr/local/bin/..'` s žádostí, abyste spustili účet jako správce.</span><span class="sxs-lookup"><span data-stu-id="f4ce0-173">In some operating systems, when you use `npm`, you may receive the error `Error: EPERM, chmod '/usr/local/bin/..'` and a request that you run the account as an administrator.</span></span> <span data-ttu-id="f4ce0-174">Pokud se vyskytne tento problém, použijte příkaz `sudo` ke spuštění příkazu `npm` na vyšší úrovni oprávnění.</span><span class="sxs-lookup"><span data-stu-id="f4ce0-174">If this problem occurs, use the `sudo` command to run `npm` at a higher privilege level.</span></span>

#### <a name="did-you-get-a-dtrace-error"></a><span data-ttu-id="f4ce0-175">Obdrželi jste chybu DTrace?</span><span class="sxs-lookup"><span data-stu-id="f4ce0-175">Did you get a DTrace error?</span></span>
<span data-ttu-id="f4ce0-176">Při instalaci Restify se může zobrazit podobný text:</span><span class="sxs-lookup"><span data-stu-id="f4ce0-176">You may see something like this text when you install Restify:</span></span>

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

<span data-ttu-id="f4ce0-177">Restify poskytuje výkonný mechanismus pro trasování volání REST pomocí DTrace.</span><span class="sxs-lookup"><span data-stu-id="f4ce0-177">Restify provides a powerful mechanism for tracing REST calls by using DTrace.</span></span> <span data-ttu-id="f4ce0-178">Nicméně, na mnoha operačních systémech není DTrace k dispozici.</span><span class="sxs-lookup"><span data-stu-id="f4ce0-178">However, many operating systems do not have DTrace available.</span></span> <span data-ttu-id="f4ce0-179">Tyto chyby můžete bezpečně ignorovat.</span><span class="sxs-lookup"><span data-stu-id="f4ce0-179">You can safely ignore these errors.</span></span>

<span data-ttu-id="f4ce0-180">Výstup příkazu by měl vypadat podobně jako tento text:</span><span class="sxs-lookup"><span data-stu-id="f4ce0-180">The output of the command should appear similar to this text:</span></span>

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

## <a name="install-passport-in-your-web-api"></a><span data-ttu-id="f4ce0-181">Instalace Passportu ve vašem webovém rozhraní API</span><span class="sxs-lookup"><span data-stu-id="f4ce0-181">Install Passport in your web API</span></span>
<span data-ttu-id="f4ce0-182">V příkazovém řádku přejděte do adresáře `azuread`, pokud v něm ještě nejste.</span><span class="sxs-lookup"><span data-stu-id="f4ce0-182">From the command line, change your directory to `azuread`, if it's not already there.</span></span>

<span data-ttu-id="f4ce0-183">Nainstalujte Passport pomocí následujícího příkazu:</span><span class="sxs-lookup"><span data-stu-id="f4ce0-183">Install Passport using the following command:</span></span>

`npm install passport`

<span data-ttu-id="f4ce0-184">Výstup příkazu by měl se měl podobat tomuto textu:</span><span class="sxs-lookup"><span data-stu-id="f4ce0-184">The output of the command should be similar to this text:</span></span>

    passport@0.1.17 node_modules\passport
    ├── pause@0.0.1
    └── pkginfo@0.2.3

## <a name="add-passport-azuread-to-your-web-api"></a><span data-ttu-id="f4ce0-185">Přidání passport-azuread do webového rozhraní API</span><span class="sxs-lookup"><span data-stu-id="f4ce0-185">Add passport-azuread to your web API</span></span>
<span data-ttu-id="f4ce0-186">Nyní přidejte strategii OAuth pomocí `passport-azuread` – sady strategií, které propojují Azure AD s Passportem.</span><span class="sxs-lookup"><span data-stu-id="f4ce0-186">Next, add the OAuth strategy by using `passport-azuread`, a suite of strategies that connect Azure AD with Passport.</span></span> <span data-ttu-id="f4ce0-187">Použijte tuto strategii pro nosné tokeny v ukázce REST API.</span><span class="sxs-lookup"><span data-stu-id="f4ce0-187">Use this strategy for bearer tokens in the REST API sample.</span></span>

> [!NOTE]
> <span data-ttu-id="f4ce0-188">Přestože OAuth2 poskytuje rozhraní, ve kterém mohou být vydávané všechny známé typy tokenů, rozšířeného využití se dostalo pouze určitým typům tokenů.</span><span class="sxs-lookup"><span data-stu-id="f4ce0-188">Although OAuth2 provides a framework in which any known token type can be issued, only certain token types have gained widespread use.</span></span> <span data-ttu-id="f4ce0-189">Tokeny pro ochranu koncových bodů jsou nosné tokeny.</span><span class="sxs-lookup"><span data-stu-id="f4ce0-189">The tokens for protecting endpoints are bearer tokens.</span></span> <span data-ttu-id="f4ce0-190">Tyto typy tokenů jsou v OAuth2 vydávány nejčastěji.</span><span class="sxs-lookup"><span data-stu-id="f4ce0-190">These types of tokens are the most widely issued in OAuth2.</span></span> <span data-ttu-id="f4ce0-191">Mnoho implementací předpokládá, že jsou nosné tokeny jediným typem vydávaných tokenů.</span><span class="sxs-lookup"><span data-stu-id="f4ce0-191">Many implementations assume that bearer tokens are the only type of token issued.</span></span>
>
>

<span data-ttu-id="f4ce0-192">V příkazovém řádku přejděte do adresáře `azuread`, pokud v něm ještě nejste.</span><span class="sxs-lookup"><span data-stu-id="f4ce0-192">From the command line, change your directory to `azuread`, if it's not already there.</span></span>

<span data-ttu-id="f4ce0-193">Nainstalujte modulu `passport-azure-ad` Passport pomocí následujícího příkazu:</span><span class="sxs-lookup"><span data-stu-id="f4ce0-193">Install the Passport `passport-azure-ad` module using the following command:</span></span>

`npm install passport-azure-ad`

<span data-ttu-id="f4ce0-194">Výstup příkazu by měl se měl podobat tomuto textu:</span><span class="sxs-lookup"><span data-stu-id="f4ce0-194">The output of the command should be similar to this text:</span></span>

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

## <a name="add-mongodb-modules-to-your-web-api"></a><span data-ttu-id="f4ce0-195">Přidání modulů MongoDB do webového rozhraní API</span><span class="sxs-lookup"><span data-stu-id="f4ce0-195">Add MongoDB modules to your web API</span></span>
<span data-ttu-id="f4ce0-196">V této ukázce se jako úložiště dat používá MongoDB.</span><span class="sxs-lookup"><span data-stu-id="f4ce0-196">This sample uses MongoDB as your data store.</span></span> <span data-ttu-id="f4ce0-197">Pro tento účel nainstalujte Mongoose, což je běžně používaný modul plug-in pro správu modelů a schémat.</span><span class="sxs-lookup"><span data-stu-id="f4ce0-197">For that install Mongoose, a widely used plug-in for managing models and schemas.</span></span>

* `npm install mongoose`

## <a name="install-additional-modules"></a><span data-ttu-id="f4ce0-198">Instalace dalších modulů</span><span class="sxs-lookup"><span data-stu-id="f4ce0-198">Install additional modules</span></span>
<span data-ttu-id="f4ce0-199">Dále nainstalujte zbývající požadované moduly.</span><span class="sxs-lookup"><span data-stu-id="f4ce0-199">Next, install the remaining required modules.</span></span>

<span data-ttu-id="f4ce0-200">V příkazovém řádku přejděte do adresáře `azuread`, pokud v něm ještě nejste:</span><span class="sxs-lookup"><span data-stu-id="f4ce0-200">From the command line, change your directory to `azuread`, if it's not already there:</span></span>

`cd azuread`

<span data-ttu-id="f4ce0-201">Nainstalujte moduly v adresáři `node_modules`:</span><span class="sxs-lookup"><span data-stu-id="f4ce0-201">Install the modules in your `node_modules` directory:</span></span>

* `npm install assert-plus`
* `npm install ejs`
* `npm install ejs-locals`
* `npm install express`
* `npm install bunyan`

## <a name="create-a-serverjs-file-with-your-dependencies"></a><span data-ttu-id="f4ce0-202">Vytvoření souboru server.js se závislostmi</span><span class="sxs-lookup"><span data-stu-id="f4ce0-202">Create a server.js file with your dependencies</span></span>
<span data-ttu-id="f4ce0-203">Soubor `server.js` poskytuje většinu funkčnosti vašeho serveru webového rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="f4ce0-203">The `server.js` file provides the majority of the functionality for your Web API server.</span></span>

<span data-ttu-id="f4ce0-204">V příkazovém řádku přejděte do adresáře `azuread`, pokud v něm ještě nejste:</span><span class="sxs-lookup"><span data-stu-id="f4ce0-204">From the command line, change your directory to `azuread`, if it's not already there:</span></span>

`cd azuread`

<span data-ttu-id="f4ce0-205">V editoru vytvořte soubor `server.js`.</span><span class="sxs-lookup"><span data-stu-id="f4ce0-205">Create a `server.js` file in an editor.</span></span> <span data-ttu-id="f4ce0-206">Přidejte následující informace:</span><span class="sxs-lookup"><span data-stu-id="f4ce0-206">Add the following information:</span></span>

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

<span data-ttu-id="f4ce0-207">Uložte soubor.</span><span class="sxs-lookup"><span data-stu-id="f4ce0-207">Save the file.</span></span> <span data-ttu-id="f4ce0-208">Vrátíte se k němu později.</span><span class="sxs-lookup"><span data-stu-id="f4ce0-208">You return to it later.</span></span>

## <a name="create-a-configjs-file-to-store-your-azure-ad-settings"></a><span data-ttu-id="f4ce0-209">Vytvoření souboru config.js pro ukládání nastavení Azure AD</span><span class="sxs-lookup"><span data-stu-id="f4ce0-209">Create a config.js file to store your Azure AD settings</span></span>
<span data-ttu-id="f4ce0-210">Tento soubor s kódem předává parametry konfigurace z portálu Azure AD do souboru `Passport.js`.</span><span class="sxs-lookup"><span data-stu-id="f4ce0-210">This code file passes the configuration parameters from your Azure AD Portal to the `Passport.js` file.</span></span> <span data-ttu-id="f4ce0-211">Tyto hodnoty konfigurace jste vytvořili, když jste přidali webové rozhraní API do portálu v první části tohoto návodu. </span><span class="sxs-lookup"><span data-stu-id="f4ce0-211">You created these configuration values when you added the web API to the portal in the first part of the walk-through.</span></span> <span data-ttu-id="f4ce0-212">Vysvětlíme, co zadat jako hodnoty těchto parametrů, až zkopírujete kód.</span><span class="sxs-lookup"><span data-stu-id="f4ce0-212">We explain what to put in the values of these parameters after you copy the code.</span></span>

<span data-ttu-id="f4ce0-213">V příkazovém řádku přejděte do adresáře `azuread`, pokud v něm ještě nejste:</span><span class="sxs-lookup"><span data-stu-id="f4ce0-213">From the command line, change your directory to `azuread`, if it's not already there:</span></span>

`cd azuread`

<span data-ttu-id="f4ce0-214">V editoru vytvořte soubor `config.js`.</span><span class="sxs-lookup"><span data-stu-id="f4ce0-214">Create a `config.js` file in an editor.</span></span> <span data-ttu-id="f4ce0-215">Přidejte následující informace:</span><span class="sxs-lookup"><span data-stu-id="f4ce0-215">Add the following information:</span></span>

```Javascript
// Don't commit this file to your public repos. This config is for first-run
exports.creds = {
clientID: <your client ID for this Web API you created in the portal>
mongoose_auth_local: 'mongodb://localhost/tasklist', // Your mongo auth uri goes here
audience: '<your audience URI>', // the Client ID of the application that is calling your API, usually a web API or native client
identityMetadata: 'https://login.microsoftonline.com/<tenant name>/.well-known/openid-configuration', // Make sure you add the B2C tenant name in the <tenant name> area
tenantName:'<tenant name>',
policyName:'b2c_1_<sign in policy name>' // This is the policy you'll want to validate against in B2C. Usually this is your Sign-in policy (as users sign in to this API)
passReqToCallback: false // This is a node.js construct that lets you pass the req all the way back to any upstream caller. We turn this off as there is no upstream caller.
};

```

[!INCLUDE [active-directory-b2c-devquickstarts-tenant-name](../../includes/active-directory-b2c-devquickstarts-tenant-name.md)]

### <a name="required-values"></a><span data-ttu-id="f4ce0-216">Požadované hodnoty</span><span class="sxs-lookup"><span data-stu-id="f4ce0-216">Required values</span></span>
<span data-ttu-id="f4ce0-217">`clientID`: ID klienta aplikace webového rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="f4ce0-217">`clientID`: The client ID of your Web API application.</span></span>

<span data-ttu-id="f4ce0-218">`IdentityMetadata`: Tady `passport-azure-ad` hledá konfigurační data pro zprostředkovatele identity.</span><span class="sxs-lookup"><span data-stu-id="f4ce0-218">`IdentityMetadata`: This is where `passport-azure-ad` looks for your configuration data for the identity provider.</span></span> <span data-ttu-id="f4ce0-219">Také zde hledá klíče pro ověření webových tokenů JSON.</span><span class="sxs-lookup"><span data-stu-id="f4ce0-219">It also looks for the keys to validate the JSON web tokens.</span></span>

<span data-ttu-id="f4ce0-220">`audience`: Identifikátor URI z portálu, který identifikuje vaši volající aplikaci.</span><span class="sxs-lookup"><span data-stu-id="f4ce0-220">`audience`: The uniform resource identifier (URI) from the portal that identifies your calling application.</span></span>

<span data-ttu-id="f4ce0-221">`tenantName`: Název vašeho klienta (například **contoso.onmicrosoft.com**).</span><span class="sxs-lookup"><span data-stu-id="f4ce0-221">`tenantName`: Your tenant name (for example, **contoso.onmicrosoft.com**).</span></span>

<span data-ttu-id="f4ce0-222">`policyName`: Zásada, kterou chcete použít k ověřování tokenů přicházejících na váš server.</span><span class="sxs-lookup"><span data-stu-id="f4ce0-222">`policyName`: The policy that you want to validate the tokens coming in to your server.</span></span> <span data-ttu-id="f4ce0-223">Tato zásada by měla být stejná jako ta, kterou používáte pro přihlašování na klientské aplikaci.</span><span class="sxs-lookup"><span data-stu-id="f4ce0-223">This policy should be the same policy that you use on the client application for sign-in.</span></span>

> [!NOTE]
> <span data-ttu-id="f4ce0-224">Použijte stejné zásady v nastavení klienta i serveru.</span><span class="sxs-lookup"><span data-stu-id="f4ce0-224">For now, use the same policies across both client and server setup.</span></span> <span data-ttu-id="f4ce0-225">Pokud jste již dokončili návod a tyto zásady jste už vytvořili, nemusíte to dělat znovu.</span><span class="sxs-lookup"><span data-stu-id="f4ce0-225">If you have already completed a walk-through and created these policies, you don't need to do so again.</span></span> <span data-ttu-id="f4ce0-226">Vzhledem k tomu, že jste návod dokončili, nemělo by být třeba nastavovat nové zásady u návodů pro klienty na této stránce.</span><span class="sxs-lookup"><span data-stu-id="f4ce0-226">Because you completed the walk-through, you shouldn't need to set up new policies for client walk-throughs on the site.</span></span>
>
>

## <a name="add-configuration-to-your-serverjs-file"></a><span data-ttu-id="f4ce0-227">Přidání konfigurace do souboru server.js</span><span class="sxs-lookup"><span data-stu-id="f4ce0-227">Add configuration to your server.js file</span></span>
<span data-ttu-id="f4ce0-228">Chcete-li načíst hodnoty z vytvořeného souboru `config.js` přidejte soubor `.config` jako požadovaný prostředek ve vaší aplikaci a pak nastavte globální proměnné na hodnoty v dokumentu `config.js`.</span><span class="sxs-lookup"><span data-stu-id="f4ce0-228">To read the values from the `config.js` file you created, add the `.config` file as a required resource in your application, and then set the global variables to those in the `config.js` document.</span></span>

<span data-ttu-id="f4ce0-229">V příkazovém řádku přejděte do adresáře `azuread`, pokud v něm ještě nejste:</span><span class="sxs-lookup"><span data-stu-id="f4ce0-229">From the command line, change your directory to `azuread`, if it's not already there:</span></span>

`cd azuread`

<span data-ttu-id="f4ce0-230">V editoru otevřete soubor `server.js`.</span><span class="sxs-lookup"><span data-stu-id="f4ce0-230">Open the `server.js` file in an editor.</span></span> <span data-ttu-id="f4ce0-231">Přidejte následující informace:</span><span class="sxs-lookup"><span data-stu-id="f4ce0-231">Add the following information:</span></span>

```Javascript
var config = require('./config');
```
<span data-ttu-id="f4ce0-232">Do souboru `server.js` přidejte nový oddíl s následujícím kódem:</span><span class="sxs-lookup"><span data-stu-id="f4ce0-232">Add a new section to `server.js` that includes the following code:</span></span>

```Javascript
// We pass these options in to the ODICBearerStrategy.

var options = {
    // The URL of the metadata document for your app. We put the keys for token validation from the URL found in the jwks_uri tag of the in the metadata.
    identityMetadata: config.creds.identityMetadata,
    clientID: config.creds.clientID,
    tenantName: config.creds.tenantName,
    policyName: config.creds.policyName,
    validateIssuer: config.creds.validateIssuer,
    audience: config.creds.audience,
    passReqToCallback: config.creds.passReqToCallback

};
```

<span data-ttu-id="f4ce0-233">V dalším kroku přidáme několik zástupných symbolů pro uživatele obdržené z volajících aplikací.</span><span class="sxs-lookup"><span data-stu-id="f4ce0-233">Next, let's add some placeholders for the users we receive from our calling applications.</span></span>

```Javascript
// array to hold logged in users and the current logged in user (owner)
var users = [];
var owner = null;
```

<span data-ttu-id="f4ce0-234">Nyní pokračujme a vytvořme i protokolovací nástroj.</span><span class="sxs-lookup"><span data-stu-id="f4ce0-234">Let's go ahead and create our logger too.</span></span>

```Javascript
// Our logger
var log = bunyan.createLogger({
    name: 'Microsoft Azure Active Directory Sample'
});
```

## <a name="add-the-mongodb-model-and-schema-information-by-using-mongoose"></a><span data-ttu-id="f4ce0-235">Přidání informací o modelu a schématu MongoDB pomocí Mongoose</span><span class="sxs-lookup"><span data-stu-id="f4ce0-235">Add the MongoDB model and schema information by using Mongoose</span></span>
<span data-ttu-id="f4ce0-236">Předchozí příprava se vyplatí, protože tyto tři soubory spojíte dohromady ve službu REST API.</span><span class="sxs-lookup"><span data-stu-id="f4ce0-236">The earlier preparation pays off as you bring these three files together in a REST API service.</span></span>

<span data-ttu-id="f4ce0-237">Pro tento návod použijte k ukládání úkolů MongoDB, jak je uvedeno výše.</span><span class="sxs-lookup"><span data-stu-id="f4ce0-237">For this walk-through, use MongoDB to store your tasks, as discussed earlier.</span></span>

<span data-ttu-id="f4ce0-238">V souboru `config.js` jste databázi nazvali **tasklist**.</span><span class="sxs-lookup"><span data-stu-id="f4ce0-238">In the `config.js` file, you called your database **tasklist**.</span></span> <span data-ttu-id="f4ce0-239">Tento název jste také vložili na konec adresy URL připojení `mongoose_auth_local`.</span><span class="sxs-lookup"><span data-stu-id="f4ce0-239">That name was also what you put at the end of the `mongoose_auth_local` connection URL.</span></span> <span data-ttu-id="f4ce0-240">Tuto databázi nemusíte předem vytvářet v MongoDB.</span><span class="sxs-lookup"><span data-stu-id="f4ce0-240">You don't need to create this database beforehand in MongoDB.</span></span> <span data-ttu-id="f4ce0-241">Databáze se vytvoří během prvního spuštění vaší serverové aplikace.</span><span class="sxs-lookup"><span data-stu-id="f4ce0-241">It creates the database for you on the first run of your server application.</span></span>

<span data-ttu-id="f4ce0-242">Poté, co sdělíte serveru, kterou databázi MongoDB má použít, budete muset napsat další kód pro vytvoření modelu a schématu pro serverové úlohy.</span><span class="sxs-lookup"><span data-stu-id="f4ce0-242">After you tell the server which MongoDB database to use, you need to write some additional code to create the model and schema for your server tasks.</span></span>

### <a name="expand-the-model"></a><span data-ttu-id="f4ce0-243">Rozšíření modelu</span><span class="sxs-lookup"><span data-stu-id="f4ce0-243">Expand the model</span></span>
<span data-ttu-id="f4ce0-244">Tento model schématu je jednoduchý.</span><span class="sxs-lookup"><span data-stu-id="f4ce0-244">This schema model is simple.</span></span> <span data-ttu-id="f4ce0-245">Podle potřeby ho můžete rozšířit.</span><span class="sxs-lookup"><span data-stu-id="f4ce0-245">You can expand it as required.</span></span>

<span data-ttu-id="f4ce0-246">`owner`: Kdo je přiřazen k úloze.</span><span class="sxs-lookup"><span data-stu-id="f4ce0-246">`owner`: Who is assigned to the task.</span></span> <span data-ttu-id="f4ce0-247">Tento objekt je typu **string**.</span><span class="sxs-lookup"><span data-stu-id="f4ce0-247">This object is a **string**.</span></span>  

<span data-ttu-id="f4ce0-248">`Text`: Vlastní úloha.</span><span class="sxs-lookup"><span data-stu-id="f4ce0-248">`Text`: The task itself.</span></span> <span data-ttu-id="f4ce0-249">Tento objekt je typu **string**.</span><span class="sxs-lookup"><span data-stu-id="f4ce0-249">This object is a **string**.</span></span>

<span data-ttu-id="f4ce0-250">`date`: Doba, kdy se má úloha vykonat.</span><span class="sxs-lookup"><span data-stu-id="f4ce0-250">`date`: The date that the task is due.</span></span> <span data-ttu-id="f4ce0-251">Tento objekt je typu **datetime**.</span><span class="sxs-lookup"><span data-stu-id="f4ce0-251">This object is a **datetime**.</span></span>

<span data-ttu-id="f4ce0-252">`completed`: Pokud je úloha dokončena.</span><span class="sxs-lookup"><span data-stu-id="f4ce0-252">`completed`: If the task is complete.</span></span> <span data-ttu-id="f4ce0-253">Tento objekt je typu **Boolean**.</span><span class="sxs-lookup"><span data-stu-id="f4ce0-253">This object is a **Boolean**.</span></span>

### <a name="create-the-schema-in-the-code"></a><span data-ttu-id="f4ce0-254">Vytvoření schématu v kódu</span><span class="sxs-lookup"><span data-stu-id="f4ce0-254">Create the schema in the code</span></span>
<span data-ttu-id="f4ce0-255">V příkazovém řádku přejděte do adresáře `azuread`, pokud v něm ještě nejste:</span><span class="sxs-lookup"><span data-stu-id="f4ce0-255">From the command line, change your directory to `azuread`, if it's not already there:</span></span>

`cd azuread`

<span data-ttu-id="f4ce0-256">V editoru otevřete soubor `server.js`.</span><span class="sxs-lookup"><span data-stu-id="f4ce0-256">Open the `server.js` file in an editor.</span></span> <span data-ttu-id="f4ce0-257">Pod položku konfigurace přidejte následující informace:</span><span class="sxs-lookup"><span data-stu-id="f4ce0-257">Add the following information below the configuration entry:</span></span>

```Javascript
// MongoDB setup
// Setup some configuration
var serverPort = process.env.PORT || 3000; // Note we are hosting our API on port 3000
var serverURI = (process.env.PORT) ? config.creds.mongoose_auth_mongohq : config.creds.mongoose_auth_local;

// Connect to MongoDB
global.db = mongoose.connect(serverURI);
var Schema = mongoose.Schema;
log.info('MongoDB Schema loaded');

// Here we create a schema to store our tasks and users. Pretty simple schema for now.
var TaskSchema = new Schema({
    owner: String,
    Text: String,
    completed: Boolean,
    date: Date
});

// Use the schema to register a model
mongoose.model('Task', TaskSchema);
var Task = mongoose.model('Task');
```
<span data-ttu-id="f4ce0-258">Nejprve vytvoříte schéma a poté vytvoříte objekt modelu, který použijete k ukládání dat napříč kódem, když definujete svoje **trasy**.</span><span class="sxs-lookup"><span data-stu-id="f4ce0-258">You first create the schema, and then you create a model object that you use to store your data throughout the code when you define your **routes**.</span></span>

## <a name="add-routes-for-your-rest-api-task-server"></a><span data-ttu-id="f4ce0-259">Přidání tras pro serveru úloh REST API</span><span class="sxs-lookup"><span data-stu-id="f4ce0-259">Add routes for your REST API task server</span></span>
<span data-ttu-id="f4ce0-260">Teď, když máte model databáze, se kterým můžete pracovat, přidejte trasy, které budete používat pro svůj server REST API.</span><span class="sxs-lookup"><span data-stu-id="f4ce0-260">Now that you have a database model to work with, add the routes you use for your REST API server.</span></span>

### <a name="about-routes-in-restify"></a><span data-ttu-id="f4ce0-261">O trasách v Restify</span><span class="sxs-lookup"><span data-stu-id="f4ce0-261">About routes in Restify</span></span>
<span data-ttu-id="f4ce0-262">Trasy v Restify fungují stejně jako při použití balíku Express.</span><span class="sxs-lookup"><span data-stu-id="f4ce0-262">Routes work in Restify in the same way that they work when they use the Express stack.</span></span> <span data-ttu-id="f4ce0-263">Trasy se definují pomocí identifikátoru URI, který by měly volat klientské aplikace. </span><span class="sxs-lookup"><span data-stu-id="f4ce0-263">You define routes by using the URI that you expect the client applications to call.</span></span>

<span data-ttu-id="f4ce0-264">Typický vzor trasy Restify je:</span><span class="sxs-lookup"><span data-stu-id="f4ce0-264">A typical pattern for a Restify route is:</span></span>

```Javascript
function createObject(req, res, next) {
// do work on Object
_object.name = req.params.object; // passed value is in req.params under object
///...
return next(); // keep the server going
}
....
server.post('/service/:add/:object', createObject); // calls createObject on routes that match this.
```

<span data-ttu-id="f4ce0-265">Restify a Express poskytují mnohem hlubší funkčnost, jako například definování typů aplikací a provádění komplexního trasování napříč různými koncovými body.</span><span class="sxs-lookup"><span data-stu-id="f4ce0-265">Restify and Express can provide much deeper functionality, such as defining application types and doing complex routing across different endpoints.</span></span> <span data-ttu-id="f4ce0-266">Pro účely tohoto kurzu ponecháme tyto trasy co nejjednodušší.</span><span class="sxs-lookup"><span data-stu-id="f4ce0-266">For the purposes of this tutorial, we keep these routes simple.</span></span>

#### <a name="add-default-routes-to-your-server"></a><span data-ttu-id="f4ce0-267">Přidání výchozích tras na server</span><span class="sxs-lookup"><span data-stu-id="f4ce0-267">Add default routes to your server</span></span>
<span data-ttu-id="f4ce0-268">Nyní pro příslušné rozhraní REST API přidejte trasy základních operací CRUD **vytvoření** a **vypsání seznamu**.</span><span class="sxs-lookup"><span data-stu-id="f4ce0-268">You now add the basic CRUD routes of **create** and **list** for our REST API.</span></span> <span data-ttu-id="f4ce0-269">Další trasy najdete ve větvi `complete` ukázky.</span><span class="sxs-lookup"><span data-stu-id="f4ce0-269">Other routes can be found in the `complete` branch of the sample.</span></span>

<span data-ttu-id="f4ce0-270">V příkazovém řádku přejděte do adresáře `azuread`, pokud v něm ještě nejste:</span><span class="sxs-lookup"><span data-stu-id="f4ce0-270">From the command line, change your directory to `azuread`, if it's not already there:</span></span>

`cd azuread`

<span data-ttu-id="f4ce0-271">V editoru otevřete soubor `server.js`.</span><span class="sxs-lookup"><span data-stu-id="f4ce0-271">Open the `server.js` file in an editor.</span></span> <span data-ttu-id="f4ce0-272">Pod položky databáze, které jste vytvořili dříve, přidejte následující informace:</span><span class="sxs-lookup"><span data-stu-id="f4ce0-272">Below the database entries you made above add the following information:</span></span>

```Javascript
/**
 *
 * APIs for our REST Task server
 */

// Create a task

function createTask(req, res, next) {

    // Resitify currently has a bug which doesn't allow you to set default headers
    // This headers comply with CORS and allow us to mongodbServer our response to any origin

    res.header("Access-Control-Allow-Origin", "*");
    res.header("Access-Control-Allow-Headers", "X-Requested-With");

    // Create a new task model, fill it up and save it to Mongodb
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
            req.log.warn(err, 'createTask: unable to save');
            next(err);
        } else {
            res.send(201, _task);

        }
    });

    return next();

}
```

```Javascript
/// Simple returns the list of TODOs that were loaded.

function listTasks(req, res, next) {
    // Resitify currently has a bug which doesn't allow you to set default headers
    // This headers comply with CORS and allow us to mongodbServer our response to any origin

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
            log.warn(err, "There is no tasks in the database. Add one!");
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


#### <a name="add-error-handling-for-the-routes"></a><span data-ttu-id="f4ce0-273">Přidání zpracování chyb pro trasy</span><span class="sxs-lookup"><span data-stu-id="f4ce0-273">Add error handling for the routes</span></span>
<span data-ttu-id="f4ce0-274">Přidejte postupy zpracování chyb, abyste mohli srozumitelně sdělit klientu informace o jakýchkoli problémech, na které narazíte.</span><span class="sxs-lookup"><span data-stu-id="f4ce0-274">Add some error handling so that you can communicate any problems you encounter back to the client in a way that it can understand.</span></span>

<span data-ttu-id="f4ce0-275">Přidejte následující kód:</span><span class="sxs-lookup"><span data-stu-id="f4ce0-275">Add the following code:</span></span>

```Javascript
///--- Errors for communicating something interesting back to the client
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


## <a name="create-your-server"></a><span data-ttu-id="f4ce0-276">Vytvoření serveru</span><span class="sxs-lookup"><span data-stu-id="f4ce0-276">Create your server</span></span>
<span data-ttu-id="f4ce0-277">Nyní jste definovali databázi a přidali trasy.</span><span class="sxs-lookup"><span data-stu-id="f4ce0-277">You have now defined your database and put your routes in place.</span></span> <span data-ttu-id="f4ce0-278">Poslední věcí, kterou musíte udělat, je přidání instance serveru, která spravuje vaše volání.</span><span class="sxs-lookup"><span data-stu-id="f4ce0-278">The last thing for you to do is to add the server instance that manages your calls.</span></span>

<span data-ttu-id="f4ce0-279">Restify a Express poskytují široké možnosti přizpůsobení serveru REST API, ale zde použijeme to nejzákladnější nastavení.</span><span class="sxs-lookup"><span data-stu-id="f4ce0-279">Restify and Express provide deep customization for a REST API server, but we use the most basic setup here.</span></span>

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

// Allow 5 requests/second by IP, and burst to 10
server.use(restify.throttle({
    burst: 10,
    rate: 5,
    ip: true,
}));

// Use the common stuff you probably want
server.use(restify.acceptParser(server.acceptable));
server.use(restify.dateParser());
server.use(restify.queryParser());
server.use(restify.gzipResponse());
server.use(restify.bodyParser({
    mapParams: true
})); // Allows for JSON mapping to REST
server.use(restify.authorizationParser()); // Looks for authorization headers

// Let's start using Passport.js

server.use(passport.initialize()); // Starts passport
server.use(passport.session()); // Provides session support


```
## <a name="add-the-routes-to-the-server-without-authentication"></a><span data-ttu-id="f4ce0-280">Přidání tras na server (bez ověřování)</span><span class="sxs-lookup"><span data-stu-id="f4ce0-280">Add the routes to the server (without authentication)</span></span>
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
    consoleMessage += '\n Open your browser to %s/api/tasks\n';
    consoleMessage += '+++++++++++++++++++++++++++++++++++++++++++++++++++++ \n';
    consoleMessage += '\n !!! why not try a $curl -isS %s | json to get some ideas? \n';
    consoleMessage += '+++++++++++++++++++++++++++++++++++++++++++++++++++++ \n\n';

    //log.info(consoleMessage, server.name, server.url, server.url, server.url);

});

```

## <a name="add-authentication-to-your-rest-api-server"></a><span data-ttu-id="f4ce0-281">Přidání ověřování na server REST API</span><span class="sxs-lookup"><span data-stu-id="f4ce0-281">Add authentication to your REST API server</span></span>
<span data-ttu-id="f4ce0-282">Když teď máte fungující server REST API, můžete ho využít s Azure AD.</span><span class="sxs-lookup"><span data-stu-id="f4ce0-282">Now that you have a running REST API server, you can make it useful against Azure AD.</span></span>

<span data-ttu-id="f4ce0-283">V příkazovém řádku přejděte do adresáře `azuread`, pokud v něm ještě nejste:</span><span class="sxs-lookup"><span data-stu-id="f4ce0-283">From the command line, change your directory to `azuread`, if it's not already there:</span></span>

`cd azuread`

### <a name="use-the-oidcbearerstrategy-that-is-included-with-passport-azure-ad"></a><span data-ttu-id="f4ce0-284">Použití OIDCBearerStrategy, která je součástí passport-azure-ad</span><span class="sxs-lookup"><span data-stu-id="f4ce0-284">Use the OIDCBearerStrategy that is included with passport-azure-ad</span></span>
> [!TIP]
> <span data-ttu-id="f4ce0-285">Při psaní rozhraní API byste vždy měli propojit data s něčím jedinečným z tokenu, co uživatel nemůže zfalšovat.</span><span class="sxs-lookup"><span data-stu-id="f4ce0-285">When you write APIs, you should always link the data to something unique from the token that the user can’t spoof.</span></span> <span data-ttu-id="f4ce0-286">Server ukládá položky úkolů na základě hodnoty **oid** uživatele v tokenu (zavolaném prostřednictvím token.oid) a ten se ukládá do pole „vlastník“.</span><span class="sxs-lookup"><span data-stu-id="f4ce0-286">When the server stores ToDo items, it does so based on the **oid** of the user in the token (called through token.oid), which goes in the “owner” field.</span></span> <span data-ttu-id="f4ce0-287">Tato hodnota zajišťuje, že pouze tento uživatel bude mít přístup ke svým vlastním položkám ToDo.</span><span class="sxs-lookup"><span data-stu-id="f4ce0-287">This value ensures that only that user can access their own ToDo items.</span></span> <span data-ttu-id="f4ce0-288">V rozhraní API se „vlastník“ nijak neprojevuje, takže externí uživatelé nemohou přistupovat k položkám úkolů jiných uživatelů ani když jsou ověřeni.</span><span class="sxs-lookup"><span data-stu-id="f4ce0-288">There is no exposure in the API of “owner,” so an external user can request others’ ToDo items even if they are authenticated.</span></span>
>
>

<span data-ttu-id="f4ce0-289">Dále použijte nosnou strategii, která je součástí `passport-azure-ad`.</span><span class="sxs-lookup"><span data-stu-id="f4ce0-289">Next, use the bearer strategy that comes with `passport-azure-ad`.</span></span>

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
        log.info('verifying the user');
        log.info(token, 'was the token retreived');
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

<span data-ttu-id="f4ce0-290">Passport používá stejný vzor pro všechny své strategie.</span><span class="sxs-lookup"><span data-stu-id="f4ce0-290">Passport uses the same pattern for all its strategies.</span></span> <span data-ttu-id="f4ce0-291">Předáváte jí `function()`, která jako parametry přijímá `token` a `done`.</span><span class="sxs-lookup"><span data-stu-id="f4ce0-291">You pass it a `function()` that has `token` and `done` as parameters.</span></span> <span data-ttu-id="f4ce0-292">Po dokončení veškeré práce se k vám strategie vrátí.</span><span class="sxs-lookup"><span data-stu-id="f4ce0-292">The strategy comes back to you after it has done all of its work.</span></span> <span data-ttu-id="f4ce0-293">Poté byste měli uložit uživatele a token, abyste ho nemuseli znovu vyžadovat.</span><span class="sxs-lookup"><span data-stu-id="f4ce0-293">You should then store the user and save the token so that you don’t need to ask for it again.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="f4ce0-294">Výše uvedený kód přijímá jakéhokoli uživatele, který se na vašem serveru ověří.</span><span class="sxs-lookup"><span data-stu-id="f4ce0-294">The code above takes any user who happens to authenticate to your server.</span></span> <span data-ttu-id="f4ce0-295">Tento proces se nazývá automatická registrace.</span><span class="sxs-lookup"><span data-stu-id="f4ce0-295">This process is known as autoregistration.</span></span> <span data-ttu-id="f4ce0-296">Na produkčních serverech neumožňujte žádným uživatelům přístup k rozhraní API bez toho, aby předtím prošli registračním procesem.</span><span class="sxs-lookup"><span data-stu-id="f4ce0-296">In production servers, don't let in any users access the API without first having them go through a registration process.</span></span> <span data-ttu-id="f4ce0-297">Tento proces představujte obvyklý vzor, který můžete vidět u uživatelských aplikací, které vám umožňují zaregistrovat se pomocí Facebooku, ale poté vás požádají o vyplnění dodatečných informací.</span><span class="sxs-lookup"><span data-stu-id="f4ce0-297">This process is usually the pattern you see in consumer apps that allow you to register by using Facebook but then ask you to fill out additional information.</span></span> <span data-ttu-id="f4ce0-298">Pokud by se nejednalo o program v příkazovém řádku, mohli bychom extrahovat e-mail z vráceného objektu tokenu a poté požádat uživatele o vyplnění dodatečných informací.</span><span class="sxs-lookup"><span data-stu-id="f4ce0-298">If this program wasn’t a command-line program, we could have extracted the email from the token object that is returned and then asked users to fill out additional information.</span></span> <span data-ttu-id="f4ce0-299">Jelikož toto je ukázka, přidáme je do databáze v paměti.</span><span class="sxs-lookup"><span data-stu-id="f4ce0-299">Because this is a sample, we add them to an in-memory database.</span></span>
>
>

## <a name="run-your-server-application-to-verify-that-it-rejects-you"></a><span data-ttu-id="f4ce0-300">Spuštění serverové aplikace kvůli ověření, že vás odmítne</span><span class="sxs-lookup"><span data-stu-id="f4ce0-300">Run your server application to verify that it rejects you</span></span>
<span data-ttu-id="f4ce0-301">Můžete použít příkaz `curl`, abyste zjistili, zda jsou již koncové body chráněné pomocí OAuth2.</span><span class="sxs-lookup"><span data-stu-id="f4ce0-301">You can use `curl` to see if you now have OAuth2 protection against your endpoints.</span></span> <span data-ttu-id="f4ce0-302">Vrácené hlavičky by vám měly dostatečně napovědět, že jste na správné cestě.</span><span class="sxs-lookup"><span data-stu-id="f4ce0-302">The headers returned should be enough to tell you that you are on the right path.</span></span>

<span data-ttu-id="f4ce0-303">Ujistěte se, že je spuštěna instance MongoDB.</span><span class="sxs-lookup"><span data-stu-id="f4ce0-303">Make sure that your MongoDB instance is running:</span></span>

    $sudo mongodb

<span data-ttu-id="f4ce0-304">Přejděte do adresáře a spusťte server:</span><span class="sxs-lookup"><span data-stu-id="f4ce0-304">Change to the directory and run the server:</span></span>

    $ cd azuread
    $ node server.js

<span data-ttu-id="f4ce0-305">V novém okně terminálu spusťte příkaz `curl`</span><span class="sxs-lookup"><span data-stu-id="f4ce0-305">In a new terminal window, run `curl`</span></span>

<span data-ttu-id="f4ce0-306">Zkuste základní požadavek POST:</span><span class="sxs-lookup"><span data-stu-id="f4ce0-306">Try a basic POST:</span></span>

`$ curl -isS -X POST http://127.0.0.1:3000/api/tasks/brandon/Hello`

```Shell
HTTP/1.1 401 Unauthorized
Connection: close
WWW-Authenticate: Bearer realm="Users"
Date: Tue, 14 Jul 2015 05:45:03 GMT
Transfer-Encoding: chunked
```

<span data-ttu-id="f4ce0-307">Chyba 401 je odpověď, kterou si přejete.</span><span class="sxs-lookup"><span data-stu-id="f4ce0-307">A 401 error is the response you want.</span></span> <span data-ttu-id="f4ce0-308">Označuje, že se vrstva Passportu pokouší přesměrovat na koncový bod ověření.</span><span class="sxs-lookup"><span data-stu-id="f4ce0-308">It indicates that the Passport layer is trying to redirect to the authorize endpoint.</span></span>

## <a name="you-now-have-a-rest-api-service-that-uses-oauth2"></a><span data-ttu-id="f4ce0-309">Nyní máte službu REST API, která používá OAuth2</span><span class="sxs-lookup"><span data-stu-id="f4ce0-309">You now have a REST API service that uses OAuth2</span></span>
<span data-ttu-id="f4ce0-310">Implementovali jste rozhraní REST API s použitím Restify a OAuth!</span><span class="sxs-lookup"><span data-stu-id="f4ce0-310">You have implemented a REST API by using Restify and OAuth!</span></span> <span data-ttu-id="f4ce0-311">Nyní máte dostatek kódu pro pokračování ve vývoji vlastní služby a navázání na tento příklad.</span><span class="sxs-lookup"><span data-stu-id="f4ce0-311">You now have sufficient code so that you can continue to develop your service and build on this example.</span></span> <span data-ttu-id="f4ce0-312">S tímto serverem jste došli nejdál, co to jde bez použití klienta kompatibilního s OAuth2.</span><span class="sxs-lookup"><span data-stu-id="f4ce0-312">You have gone as far as you can with this server without using an OAuth2-compatible client.</span></span> <span data-ttu-id="f4ce0-313">Pro tento další krok použijte další podrobný postup, například náš návod [Připojení k webovému rozhraní API pomocí iOS s B2C](active-directory-b2c-devquickstarts-ios.md).</span><span class="sxs-lookup"><span data-stu-id="f4ce0-313">For that next step use an additional walk-through like our [Connect to a web API by using iOS with B2C](active-directory-b2c-devquickstarts-ios.md) walkthrough.</span></span>

## <a name="next-steps"></a><span data-ttu-id="f4ce0-314">Další postup</span><span class="sxs-lookup"><span data-stu-id="f4ce0-314">Next steps</span></span>
<span data-ttu-id="f4ce0-315">Nyní se můžete přesunout k pokročilejším tématům, jako například:</span><span class="sxs-lookup"><span data-stu-id="f4ce0-315">You can now move to more advanced topics, such as:</span></span>

[<span data-ttu-id="f4ce0-316">Připojení k webovému rozhraní API pomocí iOS s B2C</span><span class="sxs-lookup"><span data-stu-id="f4ce0-316">Connect to a web API by using iOS with B2C</span></span>](active-directory-b2c-devquickstarts-ios.md)
