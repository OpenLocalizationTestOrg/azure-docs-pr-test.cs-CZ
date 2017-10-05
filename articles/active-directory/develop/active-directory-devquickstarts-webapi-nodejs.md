---
title: "Začínáme se službou Azure AD Node.js | Microsoft Docs"
description: "Jak sestavit webové Node.js REST API, které se integruje se službou Azure AD pro ověřování."
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
ms.openlocfilehash: 4f58177f540c14172d7ece8b4bc8c8a2b9787f8f
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="get-started-with-web-apis-for-nodejs"></a><span data-ttu-id="cc4af-103">Začínáme s webových rozhraní API pro Node.js</span><span class="sxs-lookup"><span data-stu-id="cc4af-103">Get started with web APIs for Node.js</span></span>
[!INCLUDE [active-directory-devguide](../../../includes/active-directory-devguide.md)]

<span data-ttu-id="cc4af-104">*Passport* je ověřovací middleware pro Node.js.</span><span class="sxs-lookup"><span data-stu-id="cc4af-104">*Passport* is authentication middleware for Node.js.</span></span> <span data-ttu-id="cc4af-105">Flexibilní a modulární, Passport lze snadno vyřadit k žádnému využívající Express nebo Restify webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="cc4af-105">Flexible and modular, Passport can be unobtrusively dropped in to any Express-based or Restify web application.</span></span> <span data-ttu-id="cc4af-106">Komplexní sada strategií podporují ověřování pomocí uživatelského jména a hesla, Facebook, Twitter a další.</span><span class="sxs-lookup"><span data-stu-id="cc4af-106">A comprehensive set of strategies support authentication with a username and password, Facebook, Twitter, and more.</span></span> <span data-ttu-id="cc4af-107">Vyvinuli jsme strategii pro Microsoft Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="cc4af-107">We have developed a strategy for Microsoft Azure Active Directory (Azure AD).</span></span> <span data-ttu-id="cc4af-108">Jsme nainstalujete tento modul a poté přidejte Microsoft Azure Active Directory `passport-azure-ad` modulu plug-in.</span><span class="sxs-lookup"><span data-stu-id="cc4af-108">We install this module and then add the Microsoft Azure Active Directory `passport-azure-ad` plug-in.</span></span>

<span data-ttu-id="cc4af-109">Budete muset:</span><span class="sxs-lookup"><span data-stu-id="cc4af-109">To do this, you need to:</span></span>

1. <span data-ttu-id="cc4af-110">Zaregistrovat aplikaci s Azure AD.</span><span class="sxs-lookup"><span data-stu-id="cc4af-110">Register an application with Azure AD.</span></span>
2. <span data-ttu-id="cc4af-111">Nastavit aplikaci pro používání `passport-azure-ad` modulu plug-in.</span><span class="sxs-lookup"><span data-stu-id="cc4af-111">Set up your app to use Passport's `passport-azure-ad` plug-in.</span></span>
3. <span data-ttu-id="cc4af-112">Nakonfigurujte klientskou aplikaci, aby volání seznam úkolů webové rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="cc4af-112">Configure a client application to call the To Do List web API.</span></span>

<span data-ttu-id="cc4af-113">Kód k tomuto kurzu je udržovaný [na GitHubu](https://github.com/Azure-Samples/active-directory-node-webapi).</span><span class="sxs-lookup"><span data-stu-id="cc4af-113">The code for this tutorial is maintained [on GitHub](https://github.com/Azure-Samples/active-directory-node-webapi).</span></span>

> [!NOTE]
> <span data-ttu-id="cc4af-114">Tento článek nezahrnuje, jak implementovat přihlášení, registrace a správy profilů pomocí Azure AD B2C.</span><span class="sxs-lookup"><span data-stu-id="cc4af-114">This article doesn't cover how to implement sign-in, sign-up, or profile management with Azure AD B2C.</span></span> <span data-ttu-id="cc4af-115">Zaměřuje se na volání webových rozhraní API po již byl uživatel ověřen.</span><span class="sxs-lookup"><span data-stu-id="cc4af-115">It focuses on calling web APIs after the user is already authenticated.</span></span>  <span data-ttu-id="cc4af-116">Doporučujeme spouštět s [postup při integraci s Azure Active Directory dokumentu](active-directory-how-to-integrate.md) se dozvíte základní informace o službě Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="cc4af-116">We recommend that you start with [How to integrate with Azure Active Directory document](active-directory-how-to-integrate.md) to learn about the basics of Azure Active Directory.</span></span>
>
>

<span data-ttu-id="cc4af-117">Jsme jste vydaná zdrojového kódu v tomto příkladu spuštěné v Githubu pod licencí MIT, takže Nebojte se klonování (nebo i lépe rozvětvení) a poskytnout zpětnou vazbu a požadavky pro vyžádání obsahu.</span><span class="sxs-lookup"><span data-stu-id="cc4af-117">We've released all the source code for this running example in GitHub under an MIT license, so feel free to clone (or even better, fork), and provide feedback and pull requests.</span></span>

## <a name="about-nodejs-modules"></a><span data-ttu-id="cc4af-118">O modulů Node.js</span><span class="sxs-lookup"><span data-stu-id="cc4af-118">About Node.js modules</span></span>
<span data-ttu-id="cc4af-119">V tomto návodu použijeme modulů Node.js.</span><span class="sxs-lookup"><span data-stu-id="cc4af-119">We use Node.js modules in this walkthrough.</span></span> <span data-ttu-id="cc4af-120">Moduly jsou načíst JavaScript balíčky, které poskytují funkce specifická pro vaši aplikaci.</span><span class="sxs-lookup"><span data-stu-id="cc4af-120">Modules are loadable JavaScript packages that provide specific functionality for your application.</span></span> <span data-ttu-id="cc4af-121">Obvykle nainstalujete moduly pomocí Node.js nástroj příkazového řádku na NPM v instalačním adresáři NPM.</span><span class="sxs-lookup"><span data-stu-id="cc4af-121">You usually install modules by using the Node.js an NPM command-line tool in the NPM installation directory.</span></span> <span data-ttu-id="cc4af-122">Některé moduly, jako je například modul HTTP, jsou však součástí základní Node.js balíček.</span><span class="sxs-lookup"><span data-stu-id="cc4af-122">However, some modules, such as the HTTP module, are included in the core Node.js package.</span></span>

<span data-ttu-id="cc4af-123">Nainstalované moduly jsou uloženy **node_modules** adresář v kořenovém adresáři instalace Node.js.</span><span class="sxs-lookup"><span data-stu-id="cc4af-123">Installed modules are saved in the **node_modules** directory at the root of your Node.js installation directory.</span></span> <span data-ttu-id="cc4af-124">Každý modul v **node_modules** directory udržuje vlastní **node_modules** adresář, který obsahuje všechny moduly, které závisí na.</span><span class="sxs-lookup"><span data-stu-id="cc4af-124">Each module in the **node_modules** directory maintains its own **node_modules** directory that contains any modules that it depends on.</span></span> <span data-ttu-id="cc4af-125">Navíc má každý požadované modulu **node_modules** adresáře.</span><span class="sxs-lookup"><span data-stu-id="cc4af-125">Also, each required module has a **node_modules** directory.</span></span> <span data-ttu-id="cc4af-126">Tato struktura adresáře rekurzivní představuje řetězec závislostí.</span><span class="sxs-lookup"><span data-stu-id="cc4af-126">This recursive directory structure represents the dependency chain.</span></span>

<span data-ttu-id="cc4af-127">Tato struktura řetězu závislostí za následek větší nároků aplikace.</span><span class="sxs-lookup"><span data-stu-id="cc4af-127">This dependency chain structure results in a larger application footprint.</span></span> <span data-ttu-id="cc4af-128">Můžete ale také zaručuje, že byly splněny všechny závislosti a že verzi moduly, který se používá v vývoj slouží také v produkčním prostředí.</span><span class="sxs-lookup"><span data-stu-id="cc4af-128">But it also guarantees that all dependencies are met and that the version of the modules that's used in development is also used in production.</span></span> <span data-ttu-id="cc4af-129">To usnadňuje předvídatelnější chování aplikace produkční a zabrání problémům s verzemi, které mohou ovlivnit uživatele.</span><span class="sxs-lookup"><span data-stu-id="cc4af-129">This makes the production app behavior more predictable and prevents versioning problems that might affect users.</span></span>

## <a name="step-1-register-an-azure-ad-tenant"></a><span data-ttu-id="cc4af-130">Krok 1: Registrace klienta Azure AD</span><span class="sxs-lookup"><span data-stu-id="cc4af-130">Step 1: Register an Azure AD tenant</span></span>
<span data-ttu-id="cc4af-131">Chcete-li tuto ukázku použít, musíte klienta služby Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="cc4af-131">To use this sample, you need an Azure Active Directory tenant.</span></span> <span data-ttu-id="cc4af-132">Pokud si nejste jisti je co klienta nebo jak získat, najdete v části [jak získat klienta Azure AD](active-directory-howto-tenant.md).</span><span class="sxs-lookup"><span data-stu-id="cc4af-132">If you're not sure what a tenant is or how to get one, see [How to get an Azure AD tenant](active-directory-howto-tenant.md).</span></span>

## <a name="step-2-create-an-application"></a><span data-ttu-id="cc4af-133">Krok 2: Vytvoření aplikace</span><span class="sxs-lookup"><span data-stu-id="cc4af-133">Step 2: Create an application</span></span>
<span data-ttu-id="cc4af-134">Dále vytvoříte aplikaci v adresáři, který dává Azure AD informace potřebné k bezpečné komunikaci s vaší aplikací.</span><span class="sxs-lookup"><span data-stu-id="cc4af-134">Next you create an app in your directory that gives Azure AD information that it needs to securely communicate with your app.</span></span>  <span data-ttu-id="cc4af-135">Klientská aplikace i webové rozhraní API jsou reprezentované pomocí jedné **ID aplikace** v tomto případě protože společně tvoří jednu logickou aplikaci.</span><span class="sxs-lookup"><span data-stu-id="cc4af-135">Both the client app and web API are represented by a single **Application ID** in this case, because they comprise one logical app.</span></span>  <span data-ttu-id="cc4af-136">Chcete-li vytvořit aplikaci, postupujte podle [těchto pokynů](active-directory-how-applications-are-added.md).</span><span class="sxs-lookup"><span data-stu-id="cc4af-136">To create an app, follow [these instructions](active-directory-how-applications-are-added.md).</span></span> <span data-ttu-id="cc4af-137">Pokud vytváříte-obchodní aplikace, [mohou být užitečné tyto další pokyny](../active-directory-applications-guiding-developers-for-lob-applications.md).</span><span class="sxs-lookup"><span data-stu-id="cc4af-137">If you are building a line-of-business app, [these additional instructions might be useful](../active-directory-applications-guiding-developers-for-lob-applications.md).</span></span>

<span data-ttu-id="cc4af-138">Vytvoření aplikace:</span><span class="sxs-lookup"><span data-stu-id="cc4af-138">To create an application:</span></span>

1. <span data-ttu-id="cc4af-139">Přihlaste se k webu [Azure Portal](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="cc4af-139">Sign in to the [Azure portal](https://portal.azure.com).</span></span>

2. <span data-ttu-id="cc4af-140">V horní nabídce vyberte svůj účet.</span><span class="sxs-lookup"><span data-stu-id="cc4af-140">On the top menu, select your account.</span></span> <span data-ttu-id="cc4af-141">Potom v části **Directory** vyberte klienta služby Active Directory, kde chcete registrace vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="cc4af-141">Then, under the **Directory** list, choose the Active Directory tenant where you want to register your application.</span></span>

3. <span data-ttu-id="cc4af-142">V nabídce na levé straně vyberte **více služeb**a potom vyberte **Azure Active Directory**.</span><span class="sxs-lookup"><span data-stu-id="cc4af-142">In the menu on the left, select **More Services**, and then select **Azure Active Directory**.</span></span>

4. <span data-ttu-id="cc4af-143">Vyberte **registrace aplikace**a potom vyberte **přidat**.</span><span class="sxs-lookup"><span data-stu-id="cc4af-143">Select **App registrations**, and then select **Add**.</span></span>

5. <span data-ttu-id="cc4af-144">Postupujte podle výzev a vytvořte **webové aplikace nebo WebAPI**.</span><span class="sxs-lookup"><span data-stu-id="cc4af-144">Follow the prompts to create a **Web Application and/or WebAPI**.</span></span>

      * <span data-ttu-id="cc4af-145">**Název** aplikace popisuje vaší aplikace pro koncové uživatele.</span><span class="sxs-lookup"><span data-stu-id="cc4af-145">The **name** of the application describes your application to end users.</span></span>

      * <span data-ttu-id="cc4af-146">**Přihlašovací adresa URL** je základní adresu URL aplikace.</span><span class="sxs-lookup"><span data-stu-id="cc4af-146">The **Sign-On URL** is the base URL of your app.</span></span>  <span data-ttu-id="cc4af-147">Výchozí adresa URL ukázkového kódu je `https://localhost:8080`.</span><span class="sxs-lookup"><span data-stu-id="cc4af-147">The default URL of the sample code is `https://localhost:8080`.</span></span>

6. <span data-ttu-id="cc4af-148">Po registraci, Azure AD přiřadí aplikace jedinečné ID aplikace</span><span class="sxs-lookup"><span data-stu-id="cc4af-148">After you register, Azure AD assigns your app a unique Application ID.</span></span> <span data-ttu-id="cc4af-149">Je třeba tuto hodnotu v další části, zkopírujte jej ze stránky aplikace.</span><span class="sxs-lookup"><span data-stu-id="cc4af-149">You need this value in the next sections, so copy it from the application page.</span></span>

7. <span data-ttu-id="cc4af-150">Z **nastavení** -> **vlastnosti** stránky pro aplikace, aktualizujte identifikátor ID URI aplikace.</span><span class="sxs-lookup"><span data-stu-id="cc4af-150">From the **Settings** -> **Properties** page for your application, update the App ID URI.</span></span> <span data-ttu-id="cc4af-151">**Identifikátor ID URI aplikace** je jedinečný identifikátor pro vaši aplikaci.</span><span class="sxs-lookup"><span data-stu-id="cc4af-151">The **App ID URI** is a unique identifier for your application.</span></span> <span data-ttu-id="cc4af-152">Konvence, je použít `https://<tenant-domain>/<app-name>`, například: `https://contoso.onmicrosoft.com/my-first-aad-app`.</span><span class="sxs-lookup"><span data-stu-id="cc4af-152">The convention is to use `https://<tenant-domain>/<app-name>`, for example: `https://contoso.onmicrosoft.com/my-first-aad-app`.</span></span>

8. <span data-ttu-id="cc4af-153">Vytvoření **klíč** pro vaši aplikaci z **nastavení** stránky a pak ji někam zkopírujte.</span><span class="sxs-lookup"><span data-stu-id="cc4af-153">Create a **Key** for your application from the **Settings** page, and then copy it somewhere.</span></span> <span data-ttu-id="cc4af-154">Budete je potřebovat za chvíli.</span><span class="sxs-lookup"><span data-stu-id="cc4af-154">You'll need it shortly.</span></span>

## <a name="step-3-download-nodejs-for-your-platform"></a><span data-ttu-id="cc4af-155">Krok 3: Stažení Node.js pro vaši platformu</span><span class="sxs-lookup"><span data-stu-id="cc4af-155">Step 3: Download Node.js for your platform</span></span>
<span data-ttu-id="cc4af-156">Pro úspěšné fungování této ukázky musíte mít funkční instalací Node.js.</span><span class="sxs-lookup"><span data-stu-id="cc4af-156">To successfully use this sample, you must have a working installation of Node.js.</span></span>

<span data-ttu-id="cc4af-157">Nainstalujte si Node.js z [http://nodejs.org](http://nodejs.org).</span><span class="sxs-lookup"><span data-stu-id="cc4af-157">Install Node.js from [http://nodejs.org](http://nodejs.org).</span></span>

## <a name="step-4-install-mongodb-on-your-platform"></a><span data-ttu-id="cc4af-158">Krok 4: Instalace MongoDB na vaši platformu</span><span class="sxs-lookup"><span data-stu-id="cc4af-158">Step 4: Install MongoDB on your platform</span></span>
<span data-ttu-id="cc4af-159">Pro úspěšné fungování této ukázky, musí mít funkční instalací MongoDB.</span><span class="sxs-lookup"><span data-stu-id="cc4af-159">To successfully use this sample, you must have a working installation of MongoDB.</span></span> <span data-ttu-id="cc4af-160">Chcete-li trvalé rozhraní REST API napříč instancemi serveru používáte MongoDB.</span><span class="sxs-lookup"><span data-stu-id="cc4af-160">You use MongoDB to make the REST API persistent across server instances.</span></span>

<span data-ttu-id="cc4af-161">Nainstalujte MongoDB z [http://mongodb.org](http://www.mongodb.org).</span><span class="sxs-lookup"><span data-stu-id="cc4af-161">Install MongoDB from [http://mongodb.org](http://www.mongodb.org).</span></span>

> [!NOTE]
> <span data-ttu-id="cc4af-162">Tento návod předpokládá, že používáte výchozí instalaci a server koncové body pro MongoDB, které jsou v době psaní tohoto textu je mongodb://localhost.</span><span class="sxs-lookup"><span data-stu-id="cc4af-162">This walkthrough assumes that you use the default installation and server endpoints for MongoDB, which at the time of this writing is mongodb://localhost.</span></span>
>
>

## <a name="step-5-install-the-restify-modules-in-your-web-api"></a><span data-ttu-id="cc4af-163">Krok 5: Instalace modulů Restify ve webovém rozhraní API</span><span class="sxs-lookup"><span data-stu-id="cc4af-163">Step 5: Install the Restify modules in your web API</span></span>
<span data-ttu-id="cc4af-164">Restify se používá k vytvoření našem REST API.</span><span class="sxs-lookup"><span data-stu-id="cc4af-164">We are using Restify to build our REST API.</span></span> <span data-ttu-id="cc4af-165">Restify je minimalistické a flexibilní Node.js aplikace rozhraní, které je odvozené z Express.</span><span class="sxs-lookup"><span data-stu-id="cc4af-165">Restify is a minimal and flexible Node.js application framework that's derived from Express.</span></span> <span data-ttu-id="cc4af-166">Obsahuje robustní sadu funkcí pro sestavování rozhraní REST API postavených na protokolu Connect.</span><span class="sxs-lookup"><span data-stu-id="cc4af-166">It has a robust set of features for building REST APIs on top of Connect.</span></span>

### <a name="install-restify"></a><span data-ttu-id="cc4af-167">Instalace Restify</span><span class="sxs-lookup"><span data-stu-id="cc4af-167">Install Restify</span></span>
1. <span data-ttu-id="cc4af-168">V příkazovém řádku přejděte do adresáře **azuread** adresáře.</span><span class="sxs-lookup"><span data-stu-id="cc4af-168">From the command line, change directories to the **azuread** directory.</span></span> <span data-ttu-id="cc4af-169">Pokud **azuread** adresář neexistuje, vytvořte ho.</span><span class="sxs-lookup"><span data-stu-id="cc4af-169">If the **azuread** directory does not exist, create it.</span></span>

        `cd azuread - or- mkdir azuread; cd azuread`

2. <span data-ttu-id="cc4af-170">Zadejte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="cc4af-170">Type the following command:</span></span>

    `npm install restify`

    <span data-ttu-id="cc4af-171">Tento příkaz nainstaluje Restify.</span><span class="sxs-lookup"><span data-stu-id="cc4af-171">This command installs Restify.</span></span>

#### <a name="did-you-get-an-error"></a><span data-ttu-id="cc4af-172">Obdrželi jste chybu?</span><span class="sxs-lookup"><span data-stu-id="cc4af-172">Did you get an error?</span></span>
<span data-ttu-id="cc4af-173">Použijete-li NPM u některých operačních systémů, můžete obdržet chybu, která uvádí, že **Chyba: EPERM chmod '/ usr/místní/bin /..'**</span><span class="sxs-lookup"><span data-stu-id="cc4af-173">When you use NPM on some operating systems, you might receive an error that says **Error: EPERM, chmod '/usr/local/bin/..'**</span></span> <span data-ttu-id="cc4af-174">a zlepšení zkuste účet Spustit jako správce.</span><span class="sxs-lookup"><span data-stu-id="cc4af-174">and a suggestion that you try running the account as an administrator.</span></span> <span data-ttu-id="cc4af-175">Pokud k tomu dojde, pomocí příkazu sudo spustit NPM na vyšší úrovni oprávnění.</span><span class="sxs-lookup"><span data-stu-id="cc4af-175">If this occurs, use the sudo command to run NPM at a higher privilege level.</span></span>

#### <a name="did-you-get-an-error-regarding-dtrace"></a><span data-ttu-id="cc4af-176">Obdrželi jste chybu týkající se DTRACE?</span><span class="sxs-lookup"><span data-stu-id="cc4af-176">Did you get an error regarding DTRACE?</span></span>
<span data-ttu-id="cc4af-177">Při instalaci Restify může zobrazit chyba takto:</span><span class="sxs-lookup"><span data-stu-id="cc4af-177">You might see an error like this when installing Restify:</span></span>

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
<span data-ttu-id="cc4af-178">Restify poskytuje výkonný mechanismus pro trasování volání REST pomocí DTrace.</span><span class="sxs-lookup"><span data-stu-id="cc4af-178">Restify provides a powerful mechanism for tracing REST calls by using DTrace.</span></span> <span data-ttu-id="cc4af-179">Řada operačních systémů, ale nemají DTrace.</span><span class="sxs-lookup"><span data-stu-id="cc4af-179">However, many operating systems do not have DTrace.</span></span> <span data-ttu-id="cc4af-180">Tyto chyby můžete bezpečně ignorovat.</span><span class="sxs-lookup"><span data-stu-id="cc4af-180">You can safely ignore these errors.</span></span>

<span data-ttu-id="cc4af-181">Výstup tohoto příkazu by měl vypadat podobně jako následující výstup:</span><span class="sxs-lookup"><span data-stu-id="cc4af-181">The output of this command should look similar to the following output:</span></span>

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


## <a name="step-6-install-passportjs-in-your-web-api"></a><span data-ttu-id="cc4af-182">Krok 6: Instalace Passport.js ve vašem webovém rozhraní API</span><span class="sxs-lookup"><span data-stu-id="cc4af-182">Step 6: Install Passport.js in your web API</span></span>
<span data-ttu-id="cc4af-183">[Passport](http://passportjs.org/) je ověřovací middleware pro Node.js.</span><span class="sxs-lookup"><span data-stu-id="cc4af-183">[Passport](http://passportjs.org/) is authentication middleware for Node.js.</span></span> <span data-ttu-id="cc4af-184">Flexibilní a modulární, Passport lze snadno vyřadit k žádnému využívající Express nebo Restify webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="cc4af-184">Flexible and modular, Passport can be unobtrusively dropped in to any Express-based or Restify web application.</span></span> <span data-ttu-id="cc4af-185">Komplexní sada strategií podporují ověřování pomocí uživatelského jména a hesla, Facebook, Twitter a další.</span><span class="sxs-lookup"><span data-stu-id="cc4af-185">A comprehensive set of strategies support authentication with a username and password, Facebook, Twitter, and more.</span></span>

<span data-ttu-id="cc4af-186">Vyvinuli jsme strategii pro Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="cc4af-186">We have developed a strategy for Azure Active Directory.</span></span> <span data-ttu-id="cc4af-187">Jsme nainstalujete tento modul a poté přidáte modul plug-in strategie Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="cc4af-187">We install this module and then add the Azure Active Directory strategy plug-in.</span></span>

1. <span data-ttu-id="cc4af-188">V příkazovém řádku přejděte do adresáře **azuread** adresáře.</span><span class="sxs-lookup"><span data-stu-id="cc4af-188">From the command line, change directories to the **azuread** directory.</span></span>

2. <span data-ttu-id="cc4af-189">Chcete-li nainstalovat passport.js, zadejte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="cc4af-189">To install passport.js, enter the following command :</span></span>

    `npm install passport`

    <span data-ttu-id="cc4af-190">Výstup příkazu by měl vypadat podobně jako následující:</span><span class="sxs-lookup"><span data-stu-id="cc4af-190">The output of the command should look similar to the following:</span></span>

``
        passport@0.1.17 node_modules\passport
        ├── pause@0.0.1
        └── pkginfo@0.2.3
``

## <a name="step-7-add-passport-azure-ad-to-your-web-api"></a><span data-ttu-id="cc4af-191">Krok 7: Přidání Passport-Azure-AD do webového rozhraní API</span><span class="sxs-lookup"><span data-stu-id="cc4af-191">Step 7: Add Passport-Azure-AD to your web API</span></span>
<span data-ttu-id="cc4af-192">Další přidáme strategii OAuth pomocí `passport-azure-ad`, sada strategií, které propojují Azure Active Directory do služby Passport.</span><span class="sxs-lookup"><span data-stu-id="cc4af-192">Next we add the OAuth strategy by using `passport-azure-ad`, a suite of strategies that connect Azure Active Directory to Passport.</span></span> <span data-ttu-id="cc4af-193">Používáme tuto strategii pro nosné tokeny v této ukázce REST API.</span><span class="sxs-lookup"><span data-stu-id="cc4af-193">We use this strategy for bearer tokens in this REST API sample.</span></span>

> [!NOTE]
> <span data-ttu-id="cc4af-194">Přestože OAuth2 poskytuje rozhraní, ve kterém můžou být vystavené všechny známé typy tokenů, běžně se používají pouze určitým typům tokenů.</span><span class="sxs-lookup"><span data-stu-id="cc4af-194">Although OAuth2 provides a framework in which any known token type can be issued, only certain token types are commonly used.</span></span> <span data-ttu-id="cc4af-195">Nejčastěji používané tokeny pro ochranu koncových bodů jsou nosné tokeny.</span><span class="sxs-lookup"><span data-stu-id="cc4af-195">Bearer tokens are the most commonly used tokens for protecting endpoints.</span></span> <span data-ttu-id="cc4af-196">Jsou nejčastěji vydávaným typem tokenů v OAuth2.</span><span class="sxs-lookup"><span data-stu-id="cc4af-196">They are the most widely issued type of token in OAuth2.</span></span> <span data-ttu-id="cc4af-197">Mnoho implementací předpokládá, že jsou nosné tokeny jediným typem tokeny, které jsou vystavené.</span><span class="sxs-lookup"><span data-stu-id="cc4af-197">Many implementations assume that bearer tokens are the only type of tokens that are issued.</span></span>
>
>

<span data-ttu-id="cc4af-198">V příkazovém řádku přejděte do adresáře **azuread** adresáře.</span><span class="sxs-lookup"><span data-stu-id="cc4af-198">From the command line, change directories to the **azuread** directory.</span></span>

<span data-ttu-id="cc4af-199">Zadejte následující příkaz k instalaci Passport.js `passport-azure-ad module`:</span><span class="sxs-lookup"><span data-stu-id="cc4af-199">Type the following command to install the Passport.js `passport-azure-ad module`:</span></span>

`npm install passport-azure-ad`

<span data-ttu-id="cc4af-200">Výstup příkazu by měl vypadat podobně jako následující výstup:</span><span class="sxs-lookup"><span data-stu-id="cc4af-200">The output of the command should look similar to the following output:</span></span>


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



## <a name="step-8-add-mongodb-modules-to-your-web-api"></a><span data-ttu-id="cc4af-201">Krok 8: Přidání modulů MongoDB do webového rozhraní API</span><span class="sxs-lookup"><span data-stu-id="cc4af-201">Step 8: Add MongoDB modules to your web API</span></span>
<span data-ttu-id="cc4af-202">MongoDB používáme jako naše úložiště.</span><span class="sxs-lookup"><span data-stu-id="cc4af-202">We use MongoDB as our datastore.</span></span> <span data-ttu-id="cc4af-203">Z tohoto důvodu je potřeba nainstalovat často používaný modul plug-in volané Mongoose ke správě modelů a schémat.</span><span class="sxs-lookup"><span data-stu-id="cc4af-203">For that reason, we need to install the widely used plug-in called Mongoose to manage models and schemas.</span></span> <span data-ttu-id="cc4af-204">Také je potřeba nainstalovat ovladač databáze pro MongoDB (což je také nazývaný MongoDB).</span><span class="sxs-lookup"><span data-stu-id="cc4af-204">We also need to install the database driver for MongoDB (which is also called MongoDB).</span></span>

 `npm install mongoose`

## <a name="step-9-install-additional-modules"></a><span data-ttu-id="cc4af-205">Krok 9: Instalace dalších modulů</span><span class="sxs-lookup"><span data-stu-id="cc4af-205">Step 9: Install additional modules</span></span>
<span data-ttu-id="cc4af-206">Jsme dále nainstalujte zbývající požadované moduly.</span><span class="sxs-lookup"><span data-stu-id="cc4af-206">Next we install the remaining required modules.</span></span>

1. <span data-ttu-id="cc4af-207">V příkazovém řádku přejděte do adresáře **azuread** složky, pokud si nejste již existuje.</span><span class="sxs-lookup"><span data-stu-id="cc4af-207">From the command line, change directories to the **azuread** folder if you're not already there.</span></span>

    `cd azuread`

2. <span data-ttu-id="cc4af-208">Zadejte následující příkazy pro instalaci těchto modulů ve vašem **node_modules** directory:</span><span class="sxs-lookup"><span data-stu-id="cc4af-208">Enter the following commands to install these modules in your **node_modules** directory:</span></span>

    * `npm install assert-plus`
    * `npm install bunyan`
    * `npm update`

## <a name="step-10-create-a-serverjs-with-your-dependencies"></a><span data-ttu-id="cc4af-209">Krok 10: Vytvoření server.js se závislostmi</span><span class="sxs-lookup"><span data-stu-id="cc4af-209">Step 10: Create a server.js with your dependencies</span></span>
<span data-ttu-id="cc4af-210">V souboru server.js poskytuje většinu funkcí pro naše webového rozhraní API serveru.</span><span class="sxs-lookup"><span data-stu-id="cc4af-210">The server.js file provides most of the functionality for our web API server.</span></span> <span data-ttu-id="cc4af-211">Do tohoto souboru jsme přidávat většinu kódu.</span><span class="sxs-lookup"><span data-stu-id="cc4af-211">We add most of our code to this file.</span></span> <span data-ttu-id="cc4af-212">Pro produkční účely doporučujeme, aby vám funkčnost rozdělit do menších souborů, jako je například samostatné trasy a ovladače.</span><span class="sxs-lookup"><span data-stu-id="cc4af-212">For production purposes, we recommend that you refactor the functionality into smaller files, such as separate routes and controllers.</span></span> <span data-ttu-id="cc4af-213">V této ukázce používáme server.js pro tuto funkci.</span><span class="sxs-lookup"><span data-stu-id="cc4af-213">In this demo, we use server.js for this functionality.</span></span>

1. <span data-ttu-id="cc4af-214">V příkazovém řádku přejděte do adresáře **azuread** složky, pokud si nejste již existuje.</span><span class="sxs-lookup"><span data-stu-id="cc4af-214">From the command line, change directories to the **azuread** folder if you're not already there.</span></span>

    `cd azuread`

2. <span data-ttu-id="cc4af-215">Vytvoření `server.js` souboru ve svém oblíbeném editoru a poté přidejte následující informace:</span><span class="sxs-lookup"><span data-stu-id="cc4af-215">Create a `server.js` file in your favorite editor, and then add the following information:</span></span>

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

3. <span data-ttu-id="cc4af-216">Uložte soubor.</span><span class="sxs-lookup"><span data-stu-id="cc4af-216">Save the file.</span></span> <span data-ttu-id="cc4af-217">Se vrátíme k němu za chvíli.</span><span class="sxs-lookup"><span data-stu-id="cc4af-217">We'll return to it shortly.</span></span>

## <a name="step-11-create-a-config-file-to-store-your-azure-ad-settings"></a><span data-ttu-id="cc4af-218">Krok 11: Vytvořte konfigurační soubor pro uložení nastavení Azure AD</span><span class="sxs-lookup"><span data-stu-id="cc4af-218">Step 11: Create a config file to store your Azure AD settings</span></span>
<span data-ttu-id="cc4af-219">Tento soubor s kódem předává parametry konfigurace z portálu Azure Active Directory Passport.js.</span><span class="sxs-lookup"><span data-stu-id="cc4af-219">This code file passes the configuration parameters from your Azure Active Directory portal to Passport.js.</span></span> <span data-ttu-id="cc4af-220">Tyto hodnoty konfigurace jste vytvořili, když jste přidali webové rozhraní API do portálu v první části tohoto návodu.</span><span class="sxs-lookup"><span data-stu-id="cc4af-220">You created these configuration values when you added the web API to the portal in the first part of the walkthrough.</span></span> <span data-ttu-id="cc4af-221">Vysvětlíme, co zadat jako hodnoty těchto parametrů, až zkopírujete kód.</span><span class="sxs-lookup"><span data-stu-id="cc4af-221">We explain what to put in the values of these parameters after you copy the code.</span></span>

1. <span data-ttu-id="cc4af-222">V příkazovém řádku přejděte do adresáře **azuread** složky, pokud si nejste již existuje.</span><span class="sxs-lookup"><span data-stu-id="cc4af-222">From the command line, change directories to the **azuread** folder if you're not already there.</span></span>

    `cd azuread`

2. <span data-ttu-id="cc4af-223">Vytvoření `config.js` souboru ve svém oblíbeném editoru a poté přidejte následující informace:</span><span class="sxs-lookup"><span data-stu-id="cc4af-223">Create a `config.js` file in your favorite editor, and then add the following information:</span></span>

    ```Javascript
         exports.creds = {
             mongoose_auth_local: 'mongodb://localhost/tasklist', // Your mongo auth uri goes here
             clientID: 'your client ID',
             audience: 'your application URL',
            // you cannot have users from multiple tenants sign in to your server unless you use the common endpoint
          // example: https://login.microsoftonline.com/common/.well-known/openid-configuration
             identityMetadata: 'https://login.microsoftonline.com/<your tenant id>/.well-known/openid-configuration',
             validateIssuer: true, // if you have validation on, you cannot have users from multiple tenants sign in to your server
             passReqToCallback: false,
             loggingLevel: 'info' // valid are 'info', 'warn', 'error'. Error always goes to stderr in Unix.

         };
    ```
3. <span data-ttu-id="cc4af-224">Uložte soubor.</span><span class="sxs-lookup"><span data-stu-id="cc4af-224">Save the file.</span></span>

## <a name="step-12-add-configuration-values-to-your-serverjs-file"></a><span data-ttu-id="cc4af-225">Krok 12: Přidejte hodnoty konfigurace do souboru server.js</span><span class="sxs-lookup"><span data-stu-id="cc4af-225">Step 12: Add configuration values to your server.js file</span></span>
<span data-ttu-id="cc4af-226">Je potřeba tyto hodnoty čtení ze souboru .config, který jste vytvořili v naší aplikaci.</span><span class="sxs-lookup"><span data-stu-id="cc4af-226">We need to read these values from the .config file that you created across our application.</span></span> <span data-ttu-id="cc4af-227">K tomuto účelu přidáme souboru .config jako požadovaný prostředek v naší aplikaci.</span><span class="sxs-lookup"><span data-stu-id="cc4af-227">To do this, we add the .config file as a required resource in our application.</span></span> <span data-ttu-id="cc4af-228">Potom jsme nastavte globální proměnné tak, aby odpovídaly proměnné v dokumentu config.js.</span><span class="sxs-lookup"><span data-stu-id="cc4af-228">Then we set the global variables to match the variables in the config.js document.</span></span>

1. <span data-ttu-id="cc4af-229">V příkazovém řádku přejděte do adresáře **azuread** složky, pokud si nejste již existuje.</span><span class="sxs-lookup"><span data-stu-id="cc4af-229">From the command line, change directories to the **azuread** folder if you're not already there.</span></span>

    `cd azuread`

2. <span data-ttu-id="cc4af-230">Otevřete váš `server.js` souboru ve svém oblíbeném editoru a poté přidejte následující informace:</span><span class="sxs-lookup"><span data-stu-id="cc4af-230">Open your `server.js` file in your favorite editor, and then add the following information:</span></span>

    ```Javascript
    var config = require('./config');
    ```
3. <span data-ttu-id="cc4af-231">Pak přidejte novou část, která `server.js` následujícím kódem:</span><span class="sxs-lookup"><span data-stu-id="cc4af-231">Then add a new section to `server.js` with the following code:</span></span>

    ```Javascript
    var options = {
        // The URL of the metadata document for your app. We will put the keys for token validation from the URL found in the jwks_uri tag of the in the metadata.
        identityMetadata: config.creds.identityMetadata,
        clientID: config.creds.clientID,
        validateIssuer: config.creds.validateIssuer,
        audience: config.creds.audience,
        passReqToCallback: config.creds.passReqToCallback,
        loggingLevel: config.creds.loggingLevel

    };

    // Array to hold logged in users and the current logged in user (owner).
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

      // If the logging level is specified, switch to it.
      if (config.creds.loggingLevel) { log.levels("console", config.creds.loggingLevel); }

    // MongoDB setup.
    // Set up some configuration.
    var serverPort = process.env.PORT || 8080;
    var serverURI = (process.env.PORT) ? config.creds.mongoose_auth_mongohq : config.creds.mongoose_auth_local;
    ```

4. <span data-ttu-id="cc4af-232">Uložte soubor.</span><span class="sxs-lookup"><span data-stu-id="cc4af-232">Save the file.</span></span>

## <a name="step-13-add-the-mongodb-model-and-schema-information-by-using-mongoose"></a><span data-ttu-id="cc4af-233">Krok 13: Přidejte MongoDB modelu a schématu informace pomocí Mongoose</span><span class="sxs-lookup"><span data-stu-id="cc4af-233">Step 13: Add The MongoDB Model and schema information by using Mongoose</span></span>
<span data-ttu-id="cc4af-234">Této přípravy se teď bude spustit platícího, protože jsme sloučit tyto tři soubory ve službu REST API.</span><span class="sxs-lookup"><span data-stu-id="cc4af-234">Now all this preparation is going to start paying off as we combine these three files into a REST API service.</span></span>

<span data-ttu-id="cc4af-235">V tomto návodu použijeme k uložení naše úlohy popsané v kroku 4 MongoDB.</span><span class="sxs-lookup"><span data-stu-id="cc4af-235">For this walkthrough, we use MongoDB to store our tasks as discussed in Step 4.</span></span>

<span data-ttu-id="cc4af-236">V `config.js` souboru, že jsme vytvořili v kroku 11, volali jsme naše databáze `tasklist`, protože, který byl co jsme uveďte na konci naše **mogoose_auth_local** adresy URL pro připojení.</span><span class="sxs-lookup"><span data-stu-id="cc4af-236">In the `config.js` file that we created in Step 11, we called our database `tasklist`, because that was what we put at the end of our **mogoose_auth_local** connection URL.</span></span> <span data-ttu-id="cc4af-237">Tuto databázi nemusíte předem vytvářet v MongoDB.</span><span class="sxs-lookup"><span data-stu-id="cc4af-237">You don't need to create this database beforehand in MongoDB.</span></span> <span data-ttu-id="cc4af-238">Místo toho MongoDB vytvoří to nám při prvním spuštění aplikace naše server (za předpokladu, že databáze ještě neexistuje).</span><span class="sxs-lookup"><span data-stu-id="cc4af-238">Instead, MongoDB creates this for us on the first run of our server application (assuming that the database doesn't already exist).</span></span>

<span data-ttu-id="cc4af-239">Teď, když server jsme jste sdělili kterou databázi MongoDB jsme chtěli používat, je potřeba napsat další kód pro vytvoření modelu a schématu pro úlohy naše serveru.</span><span class="sxs-lookup"><span data-stu-id="cc4af-239">Now that we've told the server which MongoDB database we'd like to use, we need to write some additional code to create the model and schema for our server's tasks.</span></span>

### <a name="discussion-of-the-model"></a><span data-ttu-id="cc4af-240">Informace o modelu</span><span class="sxs-lookup"><span data-stu-id="cc4af-240">Discussion of the model</span></span>
<span data-ttu-id="cc4af-241">Naše model schématu je jednoduché.</span><span class="sxs-lookup"><span data-stu-id="cc4af-241">Our schema model is simple.</span></span> <span data-ttu-id="cc4af-242">Rozbalte podle potřeby.</span><span class="sxs-lookup"><span data-stu-id="cc4af-242">You expand it as required.</span></span>

<span data-ttu-id="cc4af-243">Název: Název osoby, která je přiřazen k úloze.</span><span class="sxs-lookup"><span data-stu-id="cc4af-243">NAME: The name of the person who is assigned to the task.</span></span> <span data-ttu-id="cc4af-244">A **řetězec**.</span><span class="sxs-lookup"><span data-stu-id="cc4af-244">A **String**.</span></span>

<span data-ttu-id="cc4af-245">ÚLOHA: Vlastní úloha.</span><span class="sxs-lookup"><span data-stu-id="cc4af-245">TASK: The task itself.</span></span> <span data-ttu-id="cc4af-246">A **řetězec**.</span><span class="sxs-lookup"><span data-stu-id="cc4af-246">A **String**.</span></span>

<span data-ttu-id="cc4af-247">DATUM: Datum, tato úloha je kvůli.</span><span class="sxs-lookup"><span data-stu-id="cc4af-247">DATE: The date that the task is due.</span></span> <span data-ttu-id="cc4af-248">A **DATA A ČASU**.</span><span class="sxs-lookup"><span data-stu-id="cc4af-248">A **DATETIME**.</span></span>

<span data-ttu-id="cc4af-249">DOKONČENO: Pokud je úloha byla dokončena nebo ne.</span><span class="sxs-lookup"><span data-stu-id="cc4af-249">COMPLETED: If the task has been completed or not.</span></span> <span data-ttu-id="cc4af-250">A **BOOLEAN**.</span><span class="sxs-lookup"><span data-stu-id="cc4af-250">A **BOOLEAN**.</span></span>

### <a name="creating-the-schema-in-the-code"></a><span data-ttu-id="cc4af-251">Vytvoření schématu v kódu</span><span class="sxs-lookup"><span data-stu-id="cc4af-251">Creating the schema in the code</span></span>
1. <span data-ttu-id="cc4af-252">V příkazovém řádku přejděte do adresáře **azuread** složky, pokud si nejste již existuje.</span><span class="sxs-lookup"><span data-stu-id="cc4af-252">From the command line, change directories to the **azuread** folder if you're not already there.</span></span>

    `cd azuread`

2. <span data-ttu-id="cc4af-253">Otevřete váš `server.js` souboru ve svém oblíbeném editoru a poté přidejte následující informace pod položku konfigurace:</span><span class="sxs-lookup"><span data-stu-id="cc4af-253">Open your `server.js` file in your favorite editor, and then add the following information below the configuration entry:</span></span>

    ```Javascript
    // Connect to MongoDB.
    global.db = mongoose.connect(serverURI);
    var Schema = mongoose.Schema;
    log.info('MongoDB Schema loaded');

    // Here we create a schema to store our tasks and users. It's a fairly simple schema for now.
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
<span data-ttu-id="cc4af-254">Jak se dá zjistit z kódu, jsme naše schématu nejprve vytvořit.</span><span class="sxs-lookup"><span data-stu-id="cc4af-254">As you can tell from the code, we create our schema first.</span></span> <span data-ttu-id="cc4af-255">Poté vytvoříme objekt modelu, který používáme k uložení našich dat napříč kódem, když jsme definovali naše **trasy**.</span><span class="sxs-lookup"><span data-stu-id="cc4af-255">Then we create a model object that we use to store our data throughout the code when we define our **Routes**.</span></span>

## <a name="step-14-add-our-routes-for-our-task-rest-api-server"></a><span data-ttu-id="cc4af-256">Krok 14: Přidejte naše trasy pro naše serveru úloh REST API</span><span class="sxs-lookup"><span data-stu-id="cc4af-256">Step 14: Add our routes for our task REST API server</span></span>
<span data-ttu-id="cc4af-257">Teď, když máme model databáze pro práci s, přidejme trasy, které budeme používat naše server REST API.</span><span class="sxs-lookup"><span data-stu-id="cc4af-257">Now that we have a database model to work with, let's add the routes we are going use for our REST API server.</span></span>

### <a name="about-routes-in-restify"></a><span data-ttu-id="cc4af-258">O trasách v Restify</span><span class="sxs-lookup"><span data-stu-id="cc4af-258">About routes in Restify</span></span>
<span data-ttu-id="cc4af-259">Trasy v Restify fungují stejně tak v balíku Express.</span><span class="sxs-lookup"><span data-stu-id="cc4af-259">Routes work in Restify the same way they do in the Express stack.</span></span> <span data-ttu-id="cc4af-260">Trasy se definují pomocí identifikátoru URI, který by měly volat klientské aplikace. </span><span class="sxs-lookup"><span data-stu-id="cc4af-260">You define routes by using the URI that you expect the client applications to call.</span></span> <span data-ttu-id="cc4af-261">Trasy se obvykle definovat v samostatném souboru.</span><span class="sxs-lookup"><span data-stu-id="cc4af-261">Usually, you define your routes in a separate file.</span></span> <span data-ttu-id="cc4af-262">Pro naše účely jsme do souboru server.js umístit naše trasy.</span><span class="sxs-lookup"><span data-stu-id="cc4af-262">For our purposes, we put our routes in the server.js file.</span></span> <span data-ttu-id="cc4af-263">Doporučujeme, abyste je zvážit tyto trasy do své vlastní souboru pro použití v provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="cc4af-263">We recommend that you factor these routes into their own file for production use.</span></span>

<span data-ttu-id="cc4af-264">Typický vzor trasy Restify je následující:</span><span class="sxs-lookup"><span data-stu-id="cc4af-264">A typical pattern for a Restify route is as follows:</span></span>

```Javascript
function createObject(req, res, next) {

// Do work on object.

 _object.name = req.params.object; // passed value is in req.params under object

 ///...

return next(); // Keep the server going.
}

....

server.post('/service/:add/:object', createObject); // Calls createObject on routes that match this.

```


<span data-ttu-id="cc4af-265">Toto je vzor na naprosto základní úrovni.</span><span class="sxs-lookup"><span data-stu-id="cc4af-265">This is the pattern at its most basic level.</span></span> <span data-ttu-id="cc4af-266">Restify (a Express) poskytují mnohem hlubší funkčnost, jako je například definování typů aplikací a zajištění komplexního trasování napříč různými koncovými body.</span><span class="sxs-lookup"><span data-stu-id="cc4af-266">Restify (and Express) provide much deeper functionality, such as defining application types and providing complex routing across different endpoints.</span></span> <span data-ttu-id="cc4af-267">Pro naše účely jsme jsou zachování tyto trasy jednoduché.</span><span class="sxs-lookup"><span data-stu-id="cc4af-267">For our purposes, we are keeping these routes simple.</span></span>

### <a name="add-default-routes-to-our-server"></a><span data-ttu-id="cc4af-268">Přidání výchozích tras na našem serveru</span><span class="sxs-lookup"><span data-stu-id="cc4af-268">Add default routes to our server</span></span>
<span data-ttu-id="cc4af-269">Jsme teď přidejte do základní trasy CRUD vytvořit, načtení, aktualizace a odstranění.</span><span class="sxs-lookup"><span data-stu-id="cc4af-269">We now add the basic CRUD routes of Create, Retrieve, Update, and Delete.</span></span>

1. <span data-ttu-id="cc4af-270">V příkazovém řádku přejděte do adresáře **azuread** složky, pokud si nejste již existuje:</span><span class="sxs-lookup"><span data-stu-id="cc4af-270">From the command line, change directories to the **azuread** folder if you're not already there:</span></span>

    `cd azuread`

2. <span data-ttu-id="cc4af-271">Otevřete `server.js` souboru ve svém oblíbeném editoru a poté přidejte níže předchozí položky databáze, které jste provedli následující informace:</span><span class="sxs-lookup"><span data-stu-id="cc4af-271">Open the `server.js` file in your favorite editor, and then add the following information below the previous database entries that you made:</span></span>

```Javascript

/**
 *
 * APIs for our REST Task server.
 */

// Create a task.

function createTask(req, res, next) {

    // Restify currently has a bug which doesn't allow you to set default headers.
    // These headers comply with CORS and allow us to mongodbServer our response to any origin.

    res.header("Access-Control-Allow-Origin", "*");
    res.header("Access-Control-Allow-Headers", "X-Requested-With");

    // Create a new task model, fill it, and save it to Mongodb.
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

/// Simple returns the list of TODOs that were loaded.

function listTasks(req, res, next) {
    // Restify currently has a bug which doesn't allow you to set default headers.
    // These headers comply with CORS and allow us to mongodbServer our response to any origin.

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
            log.warn(err, "There is no tasks in the database. Did you initialize the database as stated in the README?");
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

### <a name="add-error-handling-in-our-apis"></a><span data-ttu-id="cc4af-272">Přidání zpracování chyb v našem rozhraní API</span><span class="sxs-lookup"><span data-stu-id="cc4af-272">Add error handling in our APIs</span></span>
```

///--- Errors for communicating something interesting back to the client.

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


## <a name="step-15-create-your-server"></a><span data-ttu-id="cc4af-273">Krok 15: Vytvoření serveru</span><span class="sxs-lookup"><span data-stu-id="cc4af-273">Step 15: Create your server</span></span>
<span data-ttu-id="cc4af-274">Jsme definovali naše databáze a naše trasy jsou na místě.</span><span class="sxs-lookup"><span data-stu-id="cc4af-274">We have defined our database and our routes are in place.</span></span> <span data-ttu-id="cc4af-275">Poslední krokem je přidání instance serveru, který spravuje naše volání.</span><span class="sxs-lookup"><span data-stu-id="cc4af-275">The last thing to do is add the server instance that manages our calls.</span></span>

<span data-ttu-id="cc4af-276">V Restify (a Express) lze provádět mnoho přizpůsobení serveru REST API, ale znovu jsme se chystáte použít nejzákladnější nastavení pro naše účely.</span><span class="sxs-lookup"><span data-stu-id="cc4af-276">In Restify (and Express) you can do a lot of customization for a REST API server, but again we are going to use the most basic setup for our purposes.</span></span>

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

// Allow five requests per second by IP, and burst to 10.
server.use(restify.throttle({
    burst: 10,
    rate: 5,
    ip: true,
}));

// Use the common stuff you probably want.
server.use(restify.acceptParser(server.acceptable));
server.use(restify.dateParser());
server.use(restify.queryParser());
server.use(restify.gzipResponse());
server.use(restify.bodyParser({
    mapParams: true
})); // Allow for JSON mapping to REST.
```

## <a name="step-16-add-the-routes-to-the-server-without-authentication-for-now"></a><span data-ttu-id="cc4af-277">Krok 16: Přidání tras na server (bez ověřování prozatím)</span><span class="sxs-lookup"><span data-stu-id="cc4af-277">Step 16: Add the routes to the server (without authentication for now)</span></span>
```Javascript
/// Now the real handlers. Here we just CRUD.
/**
/*
/* Each of these handlers is protected by our OIDCBearerStrategy by invoking 'oidc-bearer'.
/* In the pasport.authenticate() method. We set 'session: false' because REST is stateless and
/* we don't need to maintain session state. You can experiment with removing API protection
/* by removing the passport.authenticate() method as follows:
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
consoleMessage += '\n Open your browser to %s/tasks\n';
consoleMessage += '+++++++++++++++++++++++++++++++++++++++++++++++++++++ \n';
consoleMessage += '\n !!! why not try a $curl -isS %s | json to get some ideas? \n';
consoleMessage += '+++++++++++++++++++++++++++++++++++++++++++++++++++++ \n\n';
});
```

## <a name="step-17-run-the-server-before-adding-oauth-support"></a><span data-ttu-id="cc4af-278">Krok 17: Spuštění serveru (před přidáním podpory OAuth)</span><span class="sxs-lookup"><span data-stu-id="cc4af-278">Step 17: Run the server (before adding OAuth support)</span></span>
<span data-ttu-id="cc4af-279">Otestovat váš server před přidáme ověřování.</span><span class="sxs-lookup"><span data-stu-id="cc4af-279">Test out your server before we add authentication.</span></span>

<span data-ttu-id="cc4af-280">Nejjednodušší způsob, jak otestovat svůj server je pomocí curl v příkazovém řádku.</span><span class="sxs-lookup"><span data-stu-id="cc4af-280">The easiest way to test your server is by using curl in a command line.</span></span> <span data-ttu-id="cc4af-281">Než to, potřebujeme nástroj, který umožňuje parsovat výstup jako JSON.</span><span class="sxs-lookup"><span data-stu-id="cc4af-281">Before we do that, we need a utility that allows us to parse output as JSON.</span></span>

1. <span data-ttu-id="cc4af-282">Nainstalujte nástroj následující JSON (Tento nástroj použijte v následujících příkladech):</span><span class="sxs-lookup"><span data-stu-id="cc4af-282">Install the following JSON tool (all the following examples use this tool):</span></span>

    `$npm install -g jsontool`

    <span data-ttu-id="cc4af-283">Tím se globálně nainstaluje nástroj JSON.</span><span class="sxs-lookup"><span data-stu-id="cc4af-283">This installs the JSON tool globally.</span></span> <span data-ttu-id="cc4af-284">Teď, když jsme který udělat, budeme přehrání se serverem:</span><span class="sxs-lookup"><span data-stu-id="cc4af-284">Now that we’ve accomplished that, let’s play with the server:</span></span>

2. <span data-ttu-id="cc4af-285">Nejprve se ujistěte, že je spuštěna mongoDB instance:</span><span class="sxs-lookup"><span data-stu-id="cc4af-285">First, make sure that your mongoDB instance is running:</span></span>

    `$sudo mongod`

3. <span data-ttu-id="cc4af-286">Pak přejděte do adresáře a spusťte kulmy:</span><span class="sxs-lookup"><span data-stu-id="cc4af-286">Then, change to the directory and start curling:</span></span>

    <span data-ttu-id="cc4af-287">`$ cd azuread` `$ node server.js`</span><span class="sxs-lookup"><span data-stu-id="cc4af-287">`$ cd azuread` `$ node server.js`</span></span>

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

4. <span data-ttu-id="cc4af-288">Potom jsme můžete přidat úloha tímto způsobem:</span><span class="sxs-lookup"><span data-stu-id="cc4af-288">Then, we can add a task this way:</span></span>

    `$ curl -isS -X POST http://127.0.0.1:8080/tasks/brandon/Hello`

    <span data-ttu-id="cc4af-289">Odpověď by měla být:</span><span class="sxs-lookup"><span data-stu-id="cc4af-289">The response should be:</span></span>

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
    <span data-ttu-id="cc4af-290">A zde jsou uvedeny úlohy pro Brandon tímto způsobem:</span><span class="sxs-lookup"><span data-stu-id="cc4af-290">And we can list tasks for Brandon this way:</span></span>

        `$ curl -isS http://127.0.0.1:8080/tasks/brandon/`

<span data-ttu-id="cc4af-291">Pokud to vše funguje, jsme připraveni pro přidání OAuth na server REST API.</span><span class="sxs-lookup"><span data-stu-id="cc4af-291">If all this works, we're ready to add OAuth to the REST API server.</span></span>

<span data-ttu-id="cc4af-292">Máte server REST API s MongoDB!</span><span class="sxs-lookup"><span data-stu-id="cc4af-292">You have a REST API server with MongoDB!</span></span>

## <a name="step-18-add-authentication-to-our-rest-api-server"></a><span data-ttu-id="cc4af-293">Krok 18: Přidání ověřování do našich server REST API</span><span class="sxs-lookup"><span data-stu-id="cc4af-293">Step 18: Add authentication to our REST API server</span></span>
<span data-ttu-id="cc4af-294">Teď, když máme spuštěné rozhraní REST API, Začněme jeho užitečnost s Azure AD.</span><span class="sxs-lookup"><span data-stu-id="cc4af-294">Now that we have a running REST API, let's start making it useful with Azure AD.</span></span>

<span data-ttu-id="cc4af-295">V příkazovém řádku přejděte do adresáře **azuread** složky, pokud si nejste již existuje.</span><span class="sxs-lookup"><span data-stu-id="cc4af-295">From the command line, change directories to the **azuread** folder if you're not already there.</span></span>

`cd azuread`

### <a name="use-the-oidcbearerstrategy-that-is-included-with-passport-azure-ad"></a><span data-ttu-id="cc4af-296">Použití OIDCBearerStrategy, která je součástí passport-azure-ad</span><span class="sxs-lookup"><span data-stu-id="cc4af-296">Use the OIDCBearerStrategy that is included with passport-azure-ad</span></span>
<span data-ttu-id="cc4af-297">Pokud jsme jste vytvořili typický server REST se seznamem úkolů bez jakéhokoli druhu ověřování.</span><span class="sxs-lookup"><span data-stu-id="cc4af-297">So far we have built a typical REST TODO server without any kind of authorization.</span></span> <span data-ttu-id="cc4af-298">Toto je, kde začneme, které připravuje umístění.</span><span class="sxs-lookup"><span data-stu-id="cc4af-298">This is where we start putting that together.</span></span>

1. <span data-ttu-id="cc4af-299">Nejprve musíme znamenat, že má být použít Passport.</span><span class="sxs-lookup"><span data-stu-id="cc4af-299">First, we need to indicate that we want to use Passport.</span></span> <span data-ttu-id="cc4af-300">Toto právo PUT po ostatní konfigurace serveru:</span><span class="sxs-lookup"><span data-stu-id="cc4af-300">Put this right after your other server configuration:</span></span>

    ```Javascript
            // Let's start using Passport.js.

            server.use(passport.initialize()); // Starts passport.
            server.use(passport.session()); // Provides session support.
    ```
    > [!TIP]
    > <span data-ttu-id="cc4af-301">Při psaní rozhraní API, doporučujeme vám, že jste vždy měli propojit data s něčím jedinečným z tokenu, který uživatel nemůže zfalšovat.</span><span class="sxs-lookup"><span data-stu-id="cc4af-301">When you write APIs, we recommend that you always link the data to something unique from the token that the user can’t spoof.</span></span> <span data-ttu-id="cc4af-302">Pokud tento server ukládá položky úkolů, uloží je na základě Identifikátoru objektu uživatele v tokenu (zavolaném prostřednictvím token.oid), který jsme umístit do pole "vlastník".</span><span class="sxs-lookup"><span data-stu-id="cc4af-302">When this server stores TODO items, it stores them based on the object ID of the user in the token (called through token.oid), which we put in the “owner” field.</span></span> <span data-ttu-id="cc4af-303">To zajišťuje, aby pouze tento uživatel může přístup k jejich TODOs.</span><span class="sxs-lookup"><span data-stu-id="cc4af-303">This ensures that only that user can access their TODOs.</span></span> <span data-ttu-id="cc4af-304">Není nijak neprojevuje v rozhraní API se "vlastník", takže externí uživatel může požádat o TODOs jiných, i když jsou ověřeni.</span><span class="sxs-lookup"><span data-stu-id="cc4af-304">There is no exposure in the API of “owner,” so an external user can request the TODOs of others even if they are authenticated.</span></span>                    

2. <span data-ttu-id="cc4af-305">Další umožňuje použijte nosnou strategii, která se dodává s `passport-azure-ad`.</span><span class="sxs-lookup"><span data-stu-id="cc4af-305">Next let’s use the bearer strategy that comes with `passport-azure-ad`.</span></span> <span data-ttu-id="cc4af-306">Podívejte se na kód prozatím a vysvětlíme zbývající za chvíli.</span><span class="sxs-lookup"><span data-stu-id="cc4af-306">Look at the code for now and we'll explain the rest shortly.</span></span> <span data-ttu-id="cc4af-307">To uvedli po vložení výše:</span><span class="sxs-lookup"><span data-stu-id="cc4af-307">Put this after what you pasted above:</span></span>

```Javascript
    /**
    /*
    /* Calling the OIDCBearerStrategy and managing users.
    /*
    /* Passport pattern provides the need to manage users and info tokens
    /* with a FindorCreate() method that must be provided by the implementor.
    /* Here we just auto-register any user and implement a FindById().
    /* You'll want to do something smarter.
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
            log.info('verifying the user');
            log.info(token, 'was the token retreived');
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

<span data-ttu-id="cc4af-308">Passport používá podobný Princip pro všechny svoje strategie (Twitteru, Facebooku a tak dále), které řídí všichni autoři strategií k.</span><span class="sxs-lookup"><span data-stu-id="cc4af-308">Passport uses a similar pattern for all its strategies (Twitter, Facebook, and so on) that all strategy writers adhere to.</span></span> <span data-ttu-id="cc4af-309">Prohlížení strategie, uvidíte, že jsme předáváme funkci, která má token a done jako parametry.</span><span class="sxs-lookup"><span data-stu-id="cc4af-309">Looking at the strategy, you see we pass it a function that has a token and a done as the parameters.</span></span> <span data-ttu-id="cc4af-310">Strategie vrátí do us po jeho činnosti provede.</span><span class="sxs-lookup"><span data-stu-id="cc4af-310">The strategy comes back to us after it does its work.</span></span> <span data-ttu-id="cc4af-311">Po překročení se jsme uložení uživatele a skrytí tokenu, takže jsme nebudete muset požadovat znovu.</span><span class="sxs-lookup"><span data-stu-id="cc4af-311">After it does, we store the user and stash the token so we won’t need to ask for it again.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="cc4af-312">Předchozí kód přijímá jakéhokoli uživatele, které dochází k ověřování na našem server.</span><span class="sxs-lookup"><span data-stu-id="cc4af-312">The previous code takes any user that happens to authenticate to our server.</span></span> <span data-ttu-id="cc4af-313">To se označuje jako Automatická registrace.</span><span class="sxs-lookup"><span data-stu-id="cc4af-313">This is known as auto-registration.</span></span> <span data-ttu-id="cc4af-314">Na produkčních serverech, které doporučujeme si nechat každý, kdo aniž by bylo nejdříve je přejdete prostřednictvím procesu registrace, který se rozhodnete.</span><span class="sxs-lookup"><span data-stu-id="cc4af-314">In production servers, you we recommend that you don't let anyone in without first having them go through a registration process that you decide on.</span></span> <span data-ttu-id="cc4af-315">To je obvykle vzor, který můžete vidět u uživatelských aplikací, které umožňují registraci pomocí Facebooku, ale poté vás požádají o vyplnění dodatečných informací.</span><span class="sxs-lookup"><span data-stu-id="cc4af-315">This is usually the pattern you see in consumer apps, which allow you to register with Facebook but then ask you to fill out additional information.</span></span> <span data-ttu-id="cc4af-316">Pokud to nebyli příkazového řádku programu, mohli bychom extrahovat e-mail z tokenu objektu, který se vrátí a poté požádat uživatele k vyplnění dodatečných informací.</span><span class="sxs-lookup"><span data-stu-id="cc4af-316">If this wasn’t a command-line program, we could have extracted the email from the token object that is returned and then asked the user to fill out additional information.</span></span> <span data-ttu-id="cc4af-317">Protože se jedná o testovací server, jednoduše je přidáme do databáze v paměti.</span><span class="sxs-lookup"><span data-stu-id="cc4af-317">Because this is a test server, we simply add them to the in-memory database.</span></span>
>
>

### <a name="protect-some-endpoints"></a><span data-ttu-id="cc4af-318">Ochrana některé koncových bodů</span><span class="sxs-lookup"><span data-stu-id="cc4af-318">Protect some endpoints</span></span>
<span data-ttu-id="cc4af-319">Ochrana koncových bodů tak, že zadáte `passport.authenticate()` volání s protokol, který chcete použít.</span><span class="sxs-lookup"><span data-stu-id="cc4af-319">You protect endpoints by specifying the `passport.authenticate()` call with the protocol that you want to use.</span></span>

<span data-ttu-id="cc4af-320">Chcete-li naše serverový kód dělala něco zajímavějšího, Pojďme upravit trasy.</span><span class="sxs-lookup"><span data-stu-id="cc4af-320">To make our server code do something more interesting, let’s edit the route.</span></span>

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

## <a name="step-19-run-your-server-application-again-and-ensure-it-rejects-you"></a><span data-ttu-id="cc4af-321">Krok 19: Spusťte aplikaci server znovu a ujistěte se, že vás odmítne</span><span class="sxs-lookup"><span data-stu-id="cc4af-321">Step 19: Run your server application again and ensure it rejects you</span></span>
<span data-ttu-id="cc4af-322">Použijeme `curl` zjistíte, pokud máme teď chráněné pomocí OAuth2 proti naše koncové body.</span><span class="sxs-lookup"><span data-stu-id="cc4af-322">Let's use `curl` again to see if we now have OAuth2 protection against our endpoints.</span></span> <span data-ttu-id="cc4af-323">Provedeme tento test před s některým z klientskou sadu SDK proti tomuto koncovému bodu.</span><span class="sxs-lookup"><span data-stu-id="cc4af-323">We do this test before running any of our client SDKs against this endpoint.</span></span> <span data-ttu-id="cc4af-324">Vrácené hlavičky by vám měly dostatečně Řekněte nám, pokud vytvoříme dolů správné cestě.</span><span class="sxs-lookup"><span data-stu-id="cc4af-324">The headers that are returned should be enough to tell us if we're going down the right path.</span></span>

1. <span data-ttu-id="cc4af-325">Nejprve se ujistěte, že je spuštěna mongoDB instance:</span><span class="sxs-lookup"><span data-stu-id="cc4af-325">First, make sure that your mongoDB instance is running:</span></span>

    `$sudo mongod`

2. <span data-ttu-id="cc4af-326">Pak přejděte do adresáře a spusťte kulmy.</span><span class="sxs-lookup"><span data-stu-id="cc4af-326">Then, change to the directory and start curling.</span></span>

      <span data-ttu-id="cc4af-327">`$ cd azuread` `$ node server.js`</span><span class="sxs-lookup"><span data-stu-id="cc4af-327">`$ cd azuread` `$ node server.js`</span></span>

3. <span data-ttu-id="cc4af-328">Zkuste základní požadavek POST.</span><span class="sxs-lookup"><span data-stu-id="cc4af-328">Try a basic POST.</span></span>

    `$ curl -isS -X POST http://127.0.0.1:8080/tasks/brandon/Hello`

    ```Shell
    HTTP/1.1 401 Unauthorized
    Connection: close
    WWW-Authenticate: Bearer realm="Users"
    Date: Tue, 14 Jul 2015 05:45:03 GMT
    Transfer-Encoding: chunked
    ```

<span data-ttu-id="cc4af-329">401 je odpověď, kterou hledáte sem.</span><span class="sxs-lookup"><span data-stu-id="cc4af-329">A 401 is the response you are looking for here.</span></span> <span data-ttu-id="cc4af-330">Tato odpovědi vyplývá, že se vrstva Passportu pokouší přesměrovat na autorizovaný koncový bod, který je právě co chcete použít.</span><span class="sxs-lookup"><span data-stu-id="cc4af-330">This response indicates that the Passport layer is trying to redirect to the authorized endpoint, which is exactly what you want.</span></span>

## <a name="next-steps"></a><span data-ttu-id="cc4af-331">Další kroky</span><span class="sxs-lookup"><span data-stu-id="cc4af-331">Next steps</span></span>
<span data-ttu-id="cc4af-332">Jste došli nejdál, co můžete s tímto serverem bez použití klientem kompatibilní OAuth2.</span><span class="sxs-lookup"><span data-stu-id="cc4af-332">You've gone as far as you can with this server without using an OAuth2 compatible client.</span></span> <span data-ttu-id="cc4af-333">Musíte absolvovat další návod.</span><span class="sxs-lookup"><span data-stu-id="cc4af-333">You will need to go through an additional walkthrough.</span></span>

<span data-ttu-id="cc4af-334">Teď když jste se naučili jak implementovat REST API pomocí Restify a OAuth2.</span><span class="sxs-lookup"><span data-stu-id="cc4af-334">You've now learned how to implement a REST API by using Restify and OAuth2.</span></span> <span data-ttu-id="cc4af-335">Máte také víc než dost kód zachovat vývoj služby a naučit, jak stavět na tomto příkladu.</span><span class="sxs-lookup"><span data-stu-id="cc4af-335">You also have more than enough code to keep developing your service and learning how to build on this example.</span></span>

<span data-ttu-id="cc4af-336">Pokud vás zajímají další kroky v vaší ADAL cesty, tady jsou některé podporované klienty ADAL, doporučujeme, můžete pokračovat v práci s.</span><span class="sxs-lookup"><span data-stu-id="cc4af-336">If you are interested in the next steps in your ADAL journey, here are some supported ADAL clients we recommend that you  keep working with.</span></span>

<span data-ttu-id="cc4af-337">Klonovat na počítači pro vývojáře a nakonfigurujte, jak je popsáno v tomto návodu.</span><span class="sxs-lookup"><span data-stu-id="cc4af-337">Clone down to your developer machine and configure as described in the walkthrough.</span></span>

[<span data-ttu-id="cc4af-338">ADAL pro iOS</span><span class="sxs-lookup"><span data-stu-id="cc4af-338">ADAL for iOS</span></span>](https://github.com/MSOpenTech/azure-activedirectory-library-for-ios)

[<span data-ttu-id="cc4af-339">ADAL pro Android</span><span class="sxs-lookup"><span data-stu-id="cc4af-339">ADAL for Android</span></span>](https://github.com/MSOpenTech/azure-activedirectory-library-for-android)

[!INCLUDE [active-directory-devquickstarts-additional-resources](../../../includes/active-directory-devquickstarts-additional-resources.md)]
