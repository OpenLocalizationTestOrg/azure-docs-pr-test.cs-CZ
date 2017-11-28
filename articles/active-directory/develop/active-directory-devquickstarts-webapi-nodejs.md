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
# <a name="get-started-with-web-apis-for-nodejs"></a><span data-ttu-id="4c94e-103">Začínáme s webových rozhraní API pro Node.js</span><span class="sxs-lookup"><span data-stu-id="4c94e-103">Get started with web APIs for Node.js</span></span>
[!INCLUDE [active-directory-devguide](../../../includes/active-directory-devguide.md)]

<span data-ttu-id="4c94e-104">*Passport* je ověřovací middleware pro Node.js.</span><span class="sxs-lookup"><span data-stu-id="4c94e-104">*Passport* is authentication middleware for Node.js.</span></span> <span data-ttu-id="4c94e-105">Flexibilní a modulární, Passport lze snadno vyřadit v tooany využívající Express nebo Restify webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="4c94e-105">Flexible and modular, Passport can be unobtrusively dropped in tooany Express-based or Restify web application.</span></span> <span data-ttu-id="4c94e-106">Komplexní sada strategií podporují ověřování pomocí uživatelského jména a hesla, Facebook, Twitter a další.</span><span class="sxs-lookup"><span data-stu-id="4c94e-106">A comprehensive set of strategies support authentication with a username and password, Facebook, Twitter, and more.</span></span> <span data-ttu-id="4c94e-107">Vyvinuli jsme strategii pro Microsoft Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="4c94e-107">We have developed a strategy for Microsoft Azure Active Directory (Azure AD).</span></span> <span data-ttu-id="4c94e-108">Jsme nainstalujete tento modul a poté přidejte hello Microsoft Azure Active Directory `passport-azure-ad` modulu plug-in.</span><span class="sxs-lookup"><span data-stu-id="4c94e-108">We install this module and then add hello Microsoft Azure Active Directory `passport-azure-ad` plug-in.</span></span>

<span data-ttu-id="4c94e-109">toodo, budete muset:</span><span class="sxs-lookup"><span data-stu-id="4c94e-109">toodo this, you need to:</span></span>

1. <span data-ttu-id="4c94e-110">Zaregistrovat aplikaci s Azure AD.</span><span class="sxs-lookup"><span data-stu-id="4c94e-110">Register an application with Azure AD.</span></span>
2. <span data-ttu-id="4c94e-111">Nastavení vaší aplikace toouse Passport je `passport-azure-ad` modulu plug-in.</span><span class="sxs-lookup"><span data-stu-id="4c94e-111">Set up your app toouse Passport's `passport-azure-ad` plug-in.</span></span>
3. <span data-ttu-id="4c94e-112">Konfigurace klienta aplikace toocall hello tooDo seznamu webového rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="4c94e-112">Configure a client application toocall hello tooDo List web API.</span></span>

<span data-ttu-id="4c94e-113">Hello kód v tomto kurzu se udržuje [na Githubu](https://github.com/Azure-Samples/active-directory-node-webapi).</span><span class="sxs-lookup"><span data-stu-id="4c94e-113">hello code for this tutorial is maintained [on GitHub](https://github.com/Azure-Samples/active-directory-node-webapi).</span></span>

> [!NOTE]
> <span data-ttu-id="4c94e-114">Tento článek nezahrnuje jak tooimplement přihlášení, registrace, nebo profil správy s Azure AD B2C.</span><span class="sxs-lookup"><span data-stu-id="4c94e-114">This article doesn't cover how tooimplement sign-in, sign-up, or profile management with Azure AD B2C.</span></span> <span data-ttu-id="4c94e-115">Zaměřuje se na volání webových rozhraní API po hello uživatel je již ověřen.</span><span class="sxs-lookup"><span data-stu-id="4c94e-115">It focuses on calling web APIs after hello user is already authenticated.</span></span>  <span data-ttu-id="4c94e-116">Doporučujeme spouštět s [jak toointegrate s Azure Active Directory dokumentu](active-directory-how-to-integrate.md) toolearn o hello Základy služby Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="4c94e-116">We recommend that you start with [How toointegrate with Azure Active Directory document](active-directory-how-to-integrate.md) toolearn about hello basics of Azure Active Directory.</span></span>
>
>

<span data-ttu-id="4c94e-117">Vydala společnost Microsoft všechny hello zdrojového kódu v tomto příkladu spuštěné v Githubu pod licencí MIT, takže působí volné tooclone (nebo i lépe rozvětvení) a poskytnout zpětnou vazbu a požadavky pro vyžádání obsahu.</span><span class="sxs-lookup"><span data-stu-id="4c94e-117">We've released all hello source code for this running example in GitHub under an MIT license, so feel free tooclone (or even better, fork), and provide feedback and pull requests.</span></span>

## <a name="about-nodejs-modules"></a><span data-ttu-id="4c94e-118">O modulů Node.js</span><span class="sxs-lookup"><span data-stu-id="4c94e-118">About Node.js modules</span></span>
<span data-ttu-id="4c94e-119">V tomto návodu použijeme modulů Node.js.</span><span class="sxs-lookup"><span data-stu-id="4c94e-119">We use Node.js modules in this walkthrough.</span></span> <span data-ttu-id="4c94e-120">Moduly jsou načíst JavaScript balíčky, které poskytují funkce specifická pro vaši aplikaci.</span><span class="sxs-lookup"><span data-stu-id="4c94e-120">Modules are loadable JavaScript packages that provide specific functionality for your application.</span></span> <span data-ttu-id="4c94e-121">Obvykle nainstalujete moduly pomocí hello Node.js nástroj příkazového řádku na NPM v hello NPM instalační adresář.</span><span class="sxs-lookup"><span data-stu-id="4c94e-121">You usually install modules by using hello Node.js an NPM command-line tool in hello NPM installation directory.</span></span> <span data-ttu-id="4c94e-122">Některé moduly, jako je například modul HTTP hello, ale jsou součástí balíčku Node.js hello jádra.</span><span class="sxs-lookup"><span data-stu-id="4c94e-122">However, some modules, such as hello HTTP module, are included in hello core Node.js package.</span></span>

<span data-ttu-id="4c94e-123">Nainstalované moduly jsou uloženy ve hello **node_modules** adresář hello kořenové adresáře instalace Node.js.</span><span class="sxs-lookup"><span data-stu-id="4c94e-123">Installed modules are saved in hello **node_modules** directory at hello root of your Node.js installation directory.</span></span> <span data-ttu-id="4c94e-124">Každý modul v hello **node_modules** directory udržuje vlastní **node_modules** adresář, který obsahuje všechny moduly, které závisí na.</span><span class="sxs-lookup"><span data-stu-id="4c94e-124">Each module in hello **node_modules** directory maintains its own **node_modules** directory that contains any modules that it depends on.</span></span> <span data-ttu-id="4c94e-125">Navíc má každý požadované modulu **node_modules** adresáře.</span><span class="sxs-lookup"><span data-stu-id="4c94e-125">Also, each required module has a **node_modules** directory.</span></span> <span data-ttu-id="4c94e-126">Tato struktura adresáře rekurzivní představuje řetězec závislostí hello.</span><span class="sxs-lookup"><span data-stu-id="4c94e-126">This recursive directory structure represents hello dependency chain.</span></span>

<span data-ttu-id="4c94e-127">Tato struktura řetězu závislostí za následek větší nároků aplikace.</span><span class="sxs-lookup"><span data-stu-id="4c94e-127">This dependency chain structure results in a larger application footprint.</span></span> <span data-ttu-id="4c94e-128">Můžete ale také zaručuje, že jsou splněné všechny závislosti a danou verzi hello hello modulů, který se používá v vývoj se také používá v produkčním prostředí.</span><span class="sxs-lookup"><span data-stu-id="4c94e-128">But it also guarantees that all dependencies are met and that hello version of hello modules that's used in development is also used in production.</span></span> <span data-ttu-id="4c94e-129">To usnadňuje předvídatelnější chování aplikace hello produkční a zabrání problémům s verzemi, které mohou ovlivnit uživatele.</span><span class="sxs-lookup"><span data-stu-id="4c94e-129">This makes hello production app behavior more predictable and prevents versioning problems that might affect users.</span></span>

## <a name="step-1-register-an-azure-ad-tenant"></a><span data-ttu-id="4c94e-130">Krok 1: Registrace klienta Azure AD</span><span class="sxs-lookup"><span data-stu-id="4c94e-130">Step 1: Register an Azure AD tenant</span></span>
<span data-ttu-id="4c94e-131">toouse to ukázkové, musíte klienta služby Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="4c94e-131">toouse this sample, you need an Azure Active Directory tenant.</span></span> <span data-ttu-id="4c94e-132">Pokud si nejste jistí je co klienta nebo jak zjistit, tooget, [jak tooget na Azure AD klienta](active-directory-howto-tenant.md).</span><span class="sxs-lookup"><span data-stu-id="4c94e-132">If you're not sure what a tenant is or how tooget one, see [How tooget an Azure AD tenant](active-directory-howto-tenant.md).</span></span>

## <a name="step-2-create-an-application"></a><span data-ttu-id="4c94e-133">Krok 2: Vytvoření aplikace</span><span class="sxs-lookup"><span data-stu-id="4c94e-133">Step 2: Create an application</span></span>
<span data-ttu-id="4c94e-134">Dále vytvoříte aplikaci v adresáři, že poskytuje Azure AD informace, že tato služba vyžaduje toosecurely komunikaci s vaší aplikací.</span><span class="sxs-lookup"><span data-stu-id="4c94e-134">Next you create an app in your directory that gives Azure AD information that it needs toosecurely communicate with your app.</span></span>  <span data-ttu-id="4c94e-135">Hello klientská aplikace i webové rozhraní API jsou reprezentované pomocí jedné **ID aplikace** v tomto případě protože společně tvoří jednu logickou aplikaci.</span><span class="sxs-lookup"><span data-stu-id="4c94e-135">Both hello client app and web API are represented by a single **Application ID** in this case, because they comprise one logical app.</span></span>  <span data-ttu-id="4c94e-136">toocreate na aplikace, postupujte podle [tyto pokyny](active-directory-how-applications-are-added.md).</span><span class="sxs-lookup"><span data-stu-id="4c94e-136">toocreate an app, follow [these instructions](active-directory-how-applications-are-added.md).</span></span> <span data-ttu-id="4c94e-137">Pokud vytváříte-obchodní aplikace, [mohou být užitečné tyto další pokyny](../active-directory-applications-guiding-developers-for-lob-applications.md).</span><span class="sxs-lookup"><span data-stu-id="4c94e-137">If you are building a line-of-business app, [these additional instructions might be useful](../active-directory-applications-guiding-developers-for-lob-applications.md).</span></span>

<span data-ttu-id="4c94e-138">toocreate aplikace:</span><span class="sxs-lookup"><span data-stu-id="4c94e-138">toocreate an application:</span></span>

1. <span data-ttu-id="4c94e-139">Přihlaste se toohello [portál Azure](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="4c94e-139">Sign in toohello [Azure portal](https://portal.azure.com).</span></span>

2. <span data-ttu-id="4c94e-140">V horní nabídce hello vyberte svůj účet.</span><span class="sxs-lookup"><span data-stu-id="4c94e-140">On hello top menu, select your account.</span></span> <span data-ttu-id="4c94e-141">Potom v části hello **Directory** vyberte místo, kam chcete tooregister klienta služby Active Directory hello vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="4c94e-141">Then, under hello **Directory** list, choose hello Active Directory tenant where you want tooregister your application.</span></span>

3. <span data-ttu-id="4c94e-142">V nabídce hello na levé straně hello vyberte **více služeb**a potom vyberte **Azure Active Directory**.</span><span class="sxs-lookup"><span data-stu-id="4c94e-142">In hello menu on hello left, select **More Services**, and then select **Azure Active Directory**.</span></span>

4. <span data-ttu-id="4c94e-143">Vyberte **registrace aplikace**a potom vyberte **přidat**.</span><span class="sxs-lookup"><span data-stu-id="4c94e-143">Select **App registrations**, and then select **Add**.</span></span>

5. <span data-ttu-id="4c94e-144">Postupujte podle pokynů toocreate hello **webové aplikace nebo WebAPI**.</span><span class="sxs-lookup"><span data-stu-id="4c94e-144">Follow hello prompts toocreate a **Web Application and/or WebAPI**.</span></span>

      * <span data-ttu-id="4c94e-145">Hello **název** z hello aplikace popisuje tooend uživatelů vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="4c94e-145">hello **name** of hello application describes your application tooend users.</span></span>

      * <span data-ttu-id="4c94e-146">Hello **přihlašovací adresa URL** je hello základní adresu URL aplikace.</span><span class="sxs-lookup"><span data-stu-id="4c94e-146">hello **Sign-On URL** is hello base URL of your app.</span></span>  <span data-ttu-id="4c94e-147">Výchozí adresa URL aplikace hello ukázkový kód je Hello `https://localhost:8080`.</span><span class="sxs-lookup"><span data-stu-id="4c94e-147">hello default URL of hello sample code is `https://localhost:8080`.</span></span>

6. <span data-ttu-id="4c94e-148">Po registraci, Azure AD přiřadí aplikace jedinečné ID aplikace</span><span class="sxs-lookup"><span data-stu-id="4c94e-148">After you register, Azure AD assigns your app a unique Application ID.</span></span> <span data-ttu-id="4c94e-149">Je třeba tuto hodnotu v dalších částech hello, takže zkopírujte jej ze stránky aplikace hello.</span><span class="sxs-lookup"><span data-stu-id="4c94e-149">You need this value in hello next sections, so copy it from hello application page.</span></span>

7. <span data-ttu-id="4c94e-150">Z hello **nastavení** -> **vlastnosti** stránky pro aplikace, aktualizujte hello identifikátor ID URI aplikace.</span><span class="sxs-lookup"><span data-stu-id="4c94e-150">From hello **Settings** -> **Properties** page for your application, update hello App ID URI.</span></span> <span data-ttu-id="4c94e-151">Hello **identifikátor ID URI aplikace** je jedinečný identifikátor pro vaši aplikaci.</span><span class="sxs-lookup"><span data-stu-id="4c94e-151">hello **App ID URI** is a unique identifier for your application.</span></span> <span data-ttu-id="4c94e-152">konvence Hello je toouse `https://<tenant-domain>/<app-name>`, například: `https://contoso.onmicrosoft.com/my-first-aad-app`.</span><span class="sxs-lookup"><span data-stu-id="4c94e-152">hello convention is toouse `https://<tenant-domain>/<app-name>`, for example: `https://contoso.onmicrosoft.com/my-first-aad-app`.</span></span>

8. <span data-ttu-id="4c94e-153">Vytvoření **klíč** pro vaši aplikaci z hello **nastavení** stránky a pak ji někam zkopírujte.</span><span class="sxs-lookup"><span data-stu-id="4c94e-153">Create a **Key** for your application from hello **Settings** page, and then copy it somewhere.</span></span> <span data-ttu-id="4c94e-154">Budete je potřebovat za chvíli.</span><span class="sxs-lookup"><span data-stu-id="4c94e-154">You'll need it shortly.</span></span>

## <a name="step-3-download-nodejs-for-your-platform"></a><span data-ttu-id="4c94e-155">Krok 3: Stažení Node.js pro vaši platformu</span><span class="sxs-lookup"><span data-stu-id="4c94e-155">Step 3: Download Node.js for your platform</span></span>
<span data-ttu-id="4c94e-156">toosuccessfully tuto ukázku použít, musíte mít funkční instalací Node.js.</span><span class="sxs-lookup"><span data-stu-id="4c94e-156">toosuccessfully use this sample, you must have a working installation of Node.js.</span></span>

<span data-ttu-id="4c94e-157">Nainstalujte si Node.js z [http://nodejs.org](http://nodejs.org).</span><span class="sxs-lookup"><span data-stu-id="4c94e-157">Install Node.js from [http://nodejs.org](http://nodejs.org).</span></span>

## <a name="step-4-install-mongodb-on-your-platform"></a><span data-ttu-id="4c94e-158">Krok 4: Instalace MongoDB na vaši platformu</span><span class="sxs-lookup"><span data-stu-id="4c94e-158">Step 4: Install MongoDB on your platform</span></span>
<span data-ttu-id="4c94e-159">toosuccessfully tuto ukázku použít, musíte mít funkční instalací MongoDB.</span><span class="sxs-lookup"><span data-stu-id="4c94e-159">toosuccessfully use this sample, you must have a working installation of MongoDB.</span></span> <span data-ttu-id="4c94e-160">Používáte MongoDB toomake hello trvalé REST API napříč instancemi serveru.</span><span class="sxs-lookup"><span data-stu-id="4c94e-160">You use MongoDB toomake hello REST API persistent across server instances.</span></span>

<span data-ttu-id="4c94e-161">Nainstalujte MongoDB z [http://mongodb.org](http://www.mongodb.org).</span><span class="sxs-lookup"><span data-stu-id="4c94e-161">Install MongoDB from [http://mongodb.org](http://www.mongodb.org).</span></span>

> [!NOTE]
> <span data-ttu-id="4c94e-162">Tento návod předpokládá, že používáte hello výchozí instalaci a koncové body serveru pro MongoDB, což v době psaní tohoto textu hello je mongodb://localhost.</span><span class="sxs-lookup"><span data-stu-id="4c94e-162">This walkthrough assumes that you use hello default installation and server endpoints for MongoDB, which at hello time of this writing is mongodb://localhost.</span></span>
>
>

## <a name="step-5-install-hello-restify-modules-in-your-web-api"></a><span data-ttu-id="4c94e-163">Krok 5: Instalace hello modulů Restify ve webovém rozhraní API</span><span class="sxs-lookup"><span data-stu-id="4c94e-163">Step 5: Install hello Restify modules in your web API</span></span>
<span data-ttu-id="4c94e-164">Restify toobuild používáme našem REST API.</span><span class="sxs-lookup"><span data-stu-id="4c94e-164">We are using Restify toobuild our REST API.</span></span> <span data-ttu-id="4c94e-165">Restify je minimalistické a flexibilní Node.js aplikace rozhraní, které je odvozené z Express.</span><span class="sxs-lookup"><span data-stu-id="4c94e-165">Restify is a minimal and flexible Node.js application framework that's derived from Express.</span></span> <span data-ttu-id="4c94e-166">Obsahuje robustní sadu funkcí pro sestavování rozhraní REST API postavených na protokolu Connect.</span><span class="sxs-lookup"><span data-stu-id="4c94e-166">It has a robust set of features for building REST APIs on top of Connect.</span></span>

### <a name="install-restify"></a><span data-ttu-id="4c94e-167">Instalace Restify</span><span class="sxs-lookup"><span data-stu-id="4c94e-167">Install Restify</span></span>
1. <span data-ttu-id="4c94e-168">Z příkazového řádku hello, změňte adresáře toohello **azuread** adresáře.</span><span class="sxs-lookup"><span data-stu-id="4c94e-168">From hello command line, change directories toohello **azuread** directory.</span></span> <span data-ttu-id="4c94e-169">Pokud hello **azuread** adresář neexistuje, vytvořte ho.</span><span class="sxs-lookup"><span data-stu-id="4c94e-169">If hello **azuread** directory does not exist, create it.</span></span>

        `cd azuread - or- mkdir azuread; cd azuread`

2. <span data-ttu-id="4c94e-170">Zadejte hello následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="4c94e-170">Type hello following command:</span></span>

    `npm install restify`

    <span data-ttu-id="4c94e-171">Tento příkaz nainstaluje Restify.</span><span class="sxs-lookup"><span data-stu-id="4c94e-171">This command installs Restify.</span></span>

#### <a name="did-you-get-an-error"></a><span data-ttu-id="4c94e-172">Obdrželi jste chybu?</span><span class="sxs-lookup"><span data-stu-id="4c94e-172">Did you get an error?</span></span>
<span data-ttu-id="4c94e-173">Použijete-li NPM u některých operačních systémů, můžete obdržet chybu, která uvádí, že **Chyba: EPERM chmod '/ usr/místní/bin /..'**</span><span class="sxs-lookup"><span data-stu-id="4c94e-173">When you use NPM on some operating systems, you might receive an error that says **Error: EPERM, chmod '/usr/local/bin/..'**</span></span> <span data-ttu-id="4c94e-174">a návrhu, zkuste to spuštěné hello účet jako správce.</span><span class="sxs-lookup"><span data-stu-id="4c94e-174">and a suggestion that you try running hello account as an administrator.</span></span> <span data-ttu-id="4c94e-175">Pokud k tomu dojde, použijte hello sudo příkaz toorun NPM na vyšší úrovni oprávnění.</span><span class="sxs-lookup"><span data-stu-id="4c94e-175">If this occurs, use hello sudo command toorun NPM at a higher privilege level.</span></span>

#### <a name="did-you-get-an-error-regarding-dtrace"></a><span data-ttu-id="4c94e-176">Obdrželi jste chybu týkající se DTRACE?</span><span class="sxs-lookup"><span data-stu-id="4c94e-176">Did you get an error regarding DTRACE?</span></span>
<span data-ttu-id="4c94e-177">Při instalaci Restify může zobrazit chyba takto:</span><span class="sxs-lookup"><span data-stu-id="4c94e-177">You might see an error like this when installing Restify:</span></span>

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
<span data-ttu-id="4c94e-178">Restify poskytuje výkonný mechanismus pro trasování volání REST pomocí DTrace.</span><span class="sxs-lookup"><span data-stu-id="4c94e-178">Restify provides a powerful mechanism for tracing REST calls by using DTrace.</span></span> <span data-ttu-id="4c94e-179">Řada operačních systémů, ale nemají DTrace.</span><span class="sxs-lookup"><span data-stu-id="4c94e-179">However, many operating systems do not have DTrace.</span></span> <span data-ttu-id="4c94e-180">Tyto chyby můžete bezpečně ignorovat.</span><span class="sxs-lookup"><span data-stu-id="4c94e-180">You can safely ignore these errors.</span></span>

<span data-ttu-id="4c94e-181">Hello výstup tohoto příkazu by měl vypadat podobně jako toohello následující výstup:</span><span class="sxs-lookup"><span data-stu-id="4c94e-181">hello output of this command should look similar toohello following output:</span></span>

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


## <a name="step-6-install-passportjs-in-your-web-api"></a><span data-ttu-id="4c94e-182">Krok 6: Instalace Passport.js ve vašem webovém rozhraní API</span><span class="sxs-lookup"><span data-stu-id="4c94e-182">Step 6: Install Passport.js in your web API</span></span>
<span data-ttu-id="4c94e-183">[Passport](http://passportjs.org/) je ověřovací middleware pro Node.js.</span><span class="sxs-lookup"><span data-stu-id="4c94e-183">[Passport](http://passportjs.org/) is authentication middleware for Node.js.</span></span> <span data-ttu-id="4c94e-184">Flexibilní a modulární, Passport lze snadno vyřadit v tooany využívající Express nebo Restify webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="4c94e-184">Flexible and modular, Passport can be unobtrusively dropped in tooany Express-based or Restify web application.</span></span> <span data-ttu-id="4c94e-185">Komplexní sada strategií podporují ověřování pomocí uživatelského jména a hesla, Facebook, Twitter a další.</span><span class="sxs-lookup"><span data-stu-id="4c94e-185">A comprehensive set of strategies support authentication with a username and password, Facebook, Twitter, and more.</span></span>

<span data-ttu-id="4c94e-186">Vyvinuli jsme strategii pro Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="4c94e-186">We have developed a strategy for Azure Active Directory.</span></span> <span data-ttu-id="4c94e-187">Jsme nainstalujete tento modul a poté přidejte hello plug-in strategie Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="4c94e-187">We install this module and then add hello Azure Active Directory strategy plug-in.</span></span>

1. <span data-ttu-id="4c94e-188">Z příkazového řádku hello, změňte adresáře toohello **azuread** adresáře.</span><span class="sxs-lookup"><span data-stu-id="4c94e-188">From hello command line, change directories toohello **azuread** directory.</span></span>

2. <span data-ttu-id="4c94e-189">tooinstall passport.js, zadejte následující příkaz hello:</span><span class="sxs-lookup"><span data-stu-id="4c94e-189">tooinstall passport.js, enter hello following command :</span></span>

    `npm install passport`

    <span data-ttu-id="4c94e-190">výstup Hello hello příkazu by měl vypadat podobně jako toohello následující:</span><span class="sxs-lookup"><span data-stu-id="4c94e-190">hello output of hello command should look similar toohello following:</span></span>

``
        passport@0.1.17 node_modules\passport
        ├── pause@0.0.1
        └── pkginfo@0.2.3
``

## <a name="step-7-add-passport-azure-ad-tooyour-web-api"></a><span data-ttu-id="4c94e-191">Krok 7: Přidejte Passport-Azure-AD tooyour webového rozhraní API</span><span class="sxs-lookup"><span data-stu-id="4c94e-191">Step 7: Add Passport-Azure-AD tooyour web API</span></span>
<span data-ttu-id="4c94e-192">Další přidáme hello strategii OAuth pomocí `passport-azure-ad`, sady strategií, které se připojují tooPassport Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="4c94e-192">Next we add hello OAuth strategy by using `passport-azure-ad`, a suite of strategies that connect Azure Active Directory tooPassport.</span></span> <span data-ttu-id="4c94e-193">Používáme tuto strategii pro nosné tokeny v této ukázce REST API.</span><span class="sxs-lookup"><span data-stu-id="4c94e-193">We use this strategy for bearer tokens in this REST API sample.</span></span>

> [!NOTE]
> <span data-ttu-id="4c94e-194">Přestože OAuth2 poskytuje rozhraní, ve kterém můžou být vystavené všechny známé typy tokenů, běžně se používají pouze určitým typům tokenů.</span><span class="sxs-lookup"><span data-stu-id="4c94e-194">Although OAuth2 provides a framework in which any known token type can be issued, only certain token types are commonly used.</span></span> <span data-ttu-id="4c94e-195">Tokeny hello nejčastěji používaná pro ochranu koncových bodů jsou nosné tokeny.</span><span class="sxs-lookup"><span data-stu-id="4c94e-195">Bearer tokens are hello most commonly used tokens for protecting endpoints.</span></span> <span data-ttu-id="4c94e-196">Jsou nejčastěji vydané hello typem tokenů v OAuth2.</span><span class="sxs-lookup"><span data-stu-id="4c94e-196">They are hello most widely issued type of token in OAuth2.</span></span> <span data-ttu-id="4c94e-197">Mnoho implementací předpokládá, že jsou nosné tokeny jediným typem hello tokeny, které jsou vystavené.</span><span class="sxs-lookup"><span data-stu-id="4c94e-197">Many implementations assume that bearer tokens are hello only type of tokens that are issued.</span></span>
>
>

<span data-ttu-id="4c94e-198">Z příkazového řádku hello, změňte adresáře toohello **azuread** adresáře.</span><span class="sxs-lookup"><span data-stu-id="4c94e-198">From hello command line, change directories toohello **azuread** directory.</span></span>

<span data-ttu-id="4c94e-199">Typ hello následující příkaz tooinstall hello Passport.js `passport-azure-ad module`:</span><span class="sxs-lookup"><span data-stu-id="4c94e-199">Type hello following command tooinstall hello Passport.js `passport-azure-ad module`:</span></span>

`npm install passport-azure-ad`

<span data-ttu-id="4c94e-200">výstup Hello hello příkazu by měl vypadat podobně jako toohello následující výstup:</span><span class="sxs-lookup"><span data-stu-id="4c94e-200">hello output of hello command should look similar toohello following output:</span></span>


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



## <a name="step-8-add-mongodb-modules-tooyour-web-api"></a><span data-ttu-id="4c94e-201">Krok 8: Přidejte MongoDB moduly tooyour webového rozhraní API</span><span class="sxs-lookup"><span data-stu-id="4c94e-201">Step 8: Add MongoDB modules tooyour web API</span></span>
<span data-ttu-id="4c94e-202">MongoDB používáme jako naše úložiště.</span><span class="sxs-lookup"><span data-stu-id="4c94e-202">We use MongoDB as our datastore.</span></span> <span data-ttu-id="4c94e-203">Z tohoto důvodu potřebujeme tooinstall hello nejběžněji používané modulu plug-in volané Mongoose toomanage modelů a schémat.</span><span class="sxs-lookup"><span data-stu-id="4c94e-203">For that reason, we need tooinstall hello widely used plug-in called Mongoose toomanage models and schemas.</span></span> <span data-ttu-id="4c94e-204">Také potřebujeme tooinstall hello databáze ovladačů pro MongoDB (což je také nazývaný MongoDB).</span><span class="sxs-lookup"><span data-stu-id="4c94e-204">We also need tooinstall hello database driver for MongoDB (which is also called MongoDB).</span></span>

 `npm install mongoose`

## <a name="step-9-install-additional-modules"></a><span data-ttu-id="4c94e-205">Krok 9: Instalace dalších modulů</span><span class="sxs-lookup"><span data-stu-id="4c94e-205">Step 9: Install additional modules</span></span>
<span data-ttu-id="4c94e-206">Další nainstalujeme hello zbývající požadované moduly.</span><span class="sxs-lookup"><span data-stu-id="4c94e-206">Next we install hello remaining required modules.</span></span>

1. <span data-ttu-id="4c94e-207">Z příkazového řádku hello, změňte adresáře toohello **azuread** složky, pokud si nejste již existuje.</span><span class="sxs-lookup"><span data-stu-id="4c94e-207">From hello command line, change directories toohello **azuread** folder if you're not already there.</span></span>

    `cd azuread`

2. <span data-ttu-id="4c94e-208">Zadejte následující příkazy tooinstall hello tyto moduly v vaše **node_modules** directory:</span><span class="sxs-lookup"><span data-stu-id="4c94e-208">Enter hello following commands tooinstall these modules in your **node_modules** directory:</span></span>

    * `npm install assert-plus`
    * `npm install bunyan`
    * `npm update`

## <a name="step-10-create-a-serverjs-with-your-dependencies"></a><span data-ttu-id="4c94e-209">Krok 10: Vytvoření server.js se závislostmi</span><span class="sxs-lookup"><span data-stu-id="4c94e-209">Step 10: Create a server.js with your dependencies</span></span>
<span data-ttu-id="4c94e-210">souboru server.js Hello poskytuje většinu hello funkce pro naše webového rozhraní API serveru.</span><span class="sxs-lookup"><span data-stu-id="4c94e-210">hello server.js file provides most of hello functionality for our web API server.</span></span> <span data-ttu-id="4c94e-211">Přidáme většina našich toothis souboru kódu.</span><span class="sxs-lookup"><span data-stu-id="4c94e-211">We add most of our code toothis file.</span></span> <span data-ttu-id="4c94e-212">Pro produkční účely doporučujeme, aby zrefaktorujete hello funkce do menších souborů, jako je například samostatné trasy a ovladače.</span><span class="sxs-lookup"><span data-stu-id="4c94e-212">For production purposes, we recommend that you refactor hello functionality into smaller files, such as separate routes and controllers.</span></span> <span data-ttu-id="4c94e-213">V této ukázce používáme server.js pro tuto funkci.</span><span class="sxs-lookup"><span data-stu-id="4c94e-213">In this demo, we use server.js for this functionality.</span></span>

1. <span data-ttu-id="4c94e-214">Z příkazového řádku hello, změňte adresáře toohello **azuread** složky, pokud si nejste již existuje.</span><span class="sxs-lookup"><span data-stu-id="4c94e-214">From hello command line, change directories toohello **azuread** folder if you're not already there.</span></span>

    `cd azuread`

2. <span data-ttu-id="4c94e-215">Vytvoření `server.js` souboru ve svém oblíbeném editoru a poté přidejte hello následující informace:</span><span class="sxs-lookup"><span data-stu-id="4c94e-215">Create a `server.js` file in your favorite editor, and then add hello following information:</span></span>

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

3. <span data-ttu-id="4c94e-216">Uložte soubor hello.</span><span class="sxs-lookup"><span data-stu-id="4c94e-216">Save hello file.</span></span> <span data-ttu-id="4c94e-217">Vrátíme tooit za chvíli.</span><span class="sxs-lookup"><span data-stu-id="4c94e-217">We'll return tooit shortly.</span></span>

## <a name="step-11-create-a-config-file-toostore-your-azure-ad-settings"></a><span data-ttu-id="4c94e-218">Krok 11: Vytvoření souboru toostore konfigurace nastavení Azure AD</span><span class="sxs-lookup"><span data-stu-id="4c94e-218">Step 11: Create a config file toostore your Azure AD settings</span></span>
<span data-ttu-id="4c94e-219">Tento soubor s kódem předává parametry konfigurace hello z portálu tooPassport.js vaší služby Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="4c94e-219">This code file passes hello configuration parameters from your Azure Active Directory portal tooPassport.js.</span></span> <span data-ttu-id="4c94e-220">Tyto hodnoty konfigurace jste vytvořili, když jste přidali hello webové rozhraní API toohello portálu v první části hello hello návodu.</span><span class="sxs-lookup"><span data-stu-id="4c94e-220">You created these configuration values when you added hello web API toohello portal in hello first part of hello walkthrough.</span></span> <span data-ttu-id="4c94e-221">Po zkopírování hello kód objasníme, jaké tooput v hello hodnoty těchto parametrů.</span><span class="sxs-lookup"><span data-stu-id="4c94e-221">We explain what tooput in hello values of these parameters after you copy hello code.</span></span>

1. <span data-ttu-id="4c94e-222">Z příkazového řádku hello, změňte adresáře toohello **azuread** složky, pokud si nejste již existuje.</span><span class="sxs-lookup"><span data-stu-id="4c94e-222">From hello command line, change directories toohello **azuread** folder if you're not already there.</span></span>

    `cd azuread`

2. <span data-ttu-id="4c94e-223">Vytvoření `config.js` souboru ve svém oblíbeném editoru a poté přidejte hello následující informace:</span><span class="sxs-lookup"><span data-stu-id="4c94e-223">Create a `config.js` file in your favorite editor, and then add hello following information:</span></span>

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
3. <span data-ttu-id="4c94e-224">Uložte soubor hello.</span><span class="sxs-lookup"><span data-stu-id="4c94e-224">Save hello file.</span></span>

## <a name="step-12-add-configuration-values-tooyour-serverjs-file"></a><span data-ttu-id="4c94e-225">Krok 12: Přidání souboru server.js tooyour hodnoty konfigurace</span><span class="sxs-lookup"><span data-stu-id="4c94e-225">Step 12: Add configuration values tooyour server.js file</span></span>
<span data-ttu-id="4c94e-226">Tooread potřebujeme v naší aplikaci tyto hodnoty ze souboru .config hello, kterou jste vytvořili.</span><span class="sxs-lookup"><span data-stu-id="4c94e-226">We need tooread these values from hello .config file that you created across our application.</span></span> <span data-ttu-id="4c94e-227">toodo tohoto souboru .config hello přidáme jako požadovaný prostředek v naší aplikaci.</span><span class="sxs-lookup"><span data-stu-id="4c94e-227">toodo this, we add hello .config file as a required resource in our application.</span></span> <span data-ttu-id="4c94e-228">Potom nastaví hello globální proměnné toomatch hello proměnné v dokumentu config.js hello.</span><span class="sxs-lookup"><span data-stu-id="4c94e-228">Then we set hello global variables toomatch hello variables in hello config.js document.</span></span>

1. <span data-ttu-id="4c94e-229">Z příkazového řádku hello, změňte adresáře toohello **azuread** složky, pokud si nejste již existuje.</span><span class="sxs-lookup"><span data-stu-id="4c94e-229">From hello command line, change directories toohello **azuread** folder if you're not already there.</span></span>

    `cd azuread`

2. <span data-ttu-id="4c94e-230">Otevřete váš `server.js` souboru ve svém oblíbeném editoru a poté přidejte hello následující informace:</span><span class="sxs-lookup"><span data-stu-id="4c94e-230">Open your `server.js` file in your favorite editor, and then add hello following information:</span></span>

    ```Javascript
    var config = require('./config');
    ```
3. <span data-ttu-id="4c94e-231">Pak přidejte nový oddíl příliš`server.js` s hello následující kód:</span><span class="sxs-lookup"><span data-stu-id="4c94e-231">Then add a new section too`server.js` with hello following code:</span></span>

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

4. <span data-ttu-id="4c94e-232">Uložte soubor hello.</span><span class="sxs-lookup"><span data-stu-id="4c94e-232">Save hello file.</span></span>

## <a name="step-13-add-hello-mongodb-model-and-schema-information-by-using-mongoose"></a><span data-ttu-id="4c94e-233">Krok 13: Přidejte hello MongoDB modelu a schématu informace pomocí Mongoose</span><span class="sxs-lookup"><span data-stu-id="4c94e-233">Step 13: Add hello MongoDB Model and schema information by using Mongoose</span></span>
<span data-ttu-id="4c94e-234">Nyní bude tato Příprava toostart platícího, protože jsme sloučit tyto tři soubory ve službu REST API.</span><span class="sxs-lookup"><span data-stu-id="4c94e-234">Now all this preparation is going toostart paying off as we combine these three files into a REST API service.</span></span>

<span data-ttu-id="4c94e-235">V tomto návodu použijeme MongoDB toostore naše úlohy popsané v kroku 4.</span><span class="sxs-lookup"><span data-stu-id="4c94e-235">For this walkthrough, we use MongoDB toostore our tasks as discussed in Step 4.</span></span>

<span data-ttu-id="4c94e-236">V hello `config.js` souboru, že jsme vytvořili v kroku 11, volali jsme naše databáze `tasklist`, protože, který byl co jsme uveďte na konci hello naše **mogoose_auth_local** adresy URL pro připojení.</span><span class="sxs-lookup"><span data-stu-id="4c94e-236">In hello `config.js` file that we created in Step 11, we called our database `tasklist`, because that was what we put at hello end of our **mogoose_auth_local** connection URL.</span></span> <span data-ttu-id="4c94e-237">Nepotřebujete toocreate tato databáze předem v MongoDB.</span><span class="sxs-lookup"><span data-stu-id="4c94e-237">You don't need toocreate this database beforehand in MongoDB.</span></span> <span data-ttu-id="4c94e-238">Místo toho MongoDB vytvoří to pro nás na hello nejprve spusťte naše aplikace server (za předpokladu, že ještě neexistuje hello databázi).</span><span class="sxs-lookup"><span data-stu-id="4c94e-238">Instead, MongoDB creates this for us on hello first run of our server application (assuming that hello database doesn't already exist).</span></span>

<span data-ttu-id="4c94e-239">Teď, když jsme jsme vás vyzval hello server kterou databázi MongoDB rádi bychom znali toouse, potřebujeme toowrite některé další kód toocreate hello modelu a schématu pro úlohy naše serveru.</span><span class="sxs-lookup"><span data-stu-id="4c94e-239">Now that we've told hello server which MongoDB database we'd like toouse, we need toowrite some additional code toocreate hello model and schema for our server's tasks.</span></span>

### <a name="discussion-of-hello-model"></a><span data-ttu-id="4c94e-240">Informace o modelu hello</span><span class="sxs-lookup"><span data-stu-id="4c94e-240">Discussion of hello model</span></span>
<span data-ttu-id="4c94e-241">Naše model schématu je jednoduché.</span><span class="sxs-lookup"><span data-stu-id="4c94e-241">Our schema model is simple.</span></span> <span data-ttu-id="4c94e-242">Rozbalte podle potřeby.</span><span class="sxs-lookup"><span data-stu-id="4c94e-242">You expand it as required.</span></span>

<span data-ttu-id="4c94e-243">Název: název hello hello osobě, která je přiřazena toohello úloh.</span><span class="sxs-lookup"><span data-stu-id="4c94e-243">NAME: hello name of hello person who is assigned toohello task.</span></span> <span data-ttu-id="4c94e-244">A **řetězec**.</span><span class="sxs-lookup"><span data-stu-id="4c94e-244">A **String**.</span></span>

<span data-ttu-id="4c94e-245">ÚLOHA: hello vlastní úloha.</span><span class="sxs-lookup"><span data-stu-id="4c94e-245">TASK: hello task itself.</span></span> <span data-ttu-id="4c94e-246">A **řetězec**.</span><span class="sxs-lookup"><span data-stu-id="4c94e-246">A **String**.</span></span>

<span data-ttu-id="4c94e-247">Datum hello datum: tuto úlohu hello je kvůli.</span><span class="sxs-lookup"><span data-stu-id="4c94e-247">DATE: hello date that hello task is due.</span></span> <span data-ttu-id="4c94e-248">A **DATA A ČASU**.</span><span class="sxs-lookup"><span data-stu-id="4c94e-248">A **DATETIME**.</span></span>

<span data-ttu-id="4c94e-249">DOKONČENO: Pokud má úloha hello hotové nebo ne.</span><span class="sxs-lookup"><span data-stu-id="4c94e-249">COMPLETED: If hello task has been completed or not.</span></span> <span data-ttu-id="4c94e-250">A **BOOLEAN**.</span><span class="sxs-lookup"><span data-stu-id="4c94e-250">A **BOOLEAN**.</span></span>

### <a name="creating-hello-schema-in-hello-code"></a><span data-ttu-id="4c94e-251">Vytváření hello schématu v kódu hello</span><span class="sxs-lookup"><span data-stu-id="4c94e-251">Creating hello schema in hello code</span></span>
1. <span data-ttu-id="4c94e-252">Z příkazového řádku hello, změňte adresáře toohello **azuread** složky, pokud si nejste již existuje.</span><span class="sxs-lookup"><span data-stu-id="4c94e-252">From hello command line, change directories toohello **azuread** folder if you're not already there.</span></span>

    `cd azuread`

2. <span data-ttu-id="4c94e-253">Otevřete váš `server.js` souboru ve svém oblíbeném editoru a poté přidejte následující informace pod položku konfigurace hello hello:</span><span class="sxs-lookup"><span data-stu-id="4c94e-253">Open your `server.js` file in your favorite editor, and then add hello following information below hello configuration entry:</span></span>

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
<span data-ttu-id="4c94e-254">Jak se dá zjistit z hello kódu, jsme naše schématu nejprve vytvořit.</span><span class="sxs-lookup"><span data-stu-id="4c94e-254">As you can tell from hello code, we create our schema first.</span></span> <span data-ttu-id="4c94e-255">Poté vytvoříme objekt modelu, který používáme toostore našich dat v rámci hello kódu, když jsme definovali naše **trasy**.</span><span class="sxs-lookup"><span data-stu-id="4c94e-255">Then we create a model object that we use toostore our data throughout hello code when we define our **Routes**.</span></span>

## <a name="step-14-add-our-routes-for-our-task-rest-api-server"></a><span data-ttu-id="4c94e-256">Krok 14: Přidejte naše trasy pro naše serveru úloh REST API</span><span class="sxs-lookup"><span data-stu-id="4c94e-256">Step 14: Add our routes for our task REST API server</span></span>
<span data-ttu-id="4c94e-257">Teď, když máme toowork modelu databázi s, Pojďme přidat trasy hello Snažíme se má použít pro naše server REST API.</span><span class="sxs-lookup"><span data-stu-id="4c94e-257">Now that we have a database model toowork with, let's add hello routes we are going use for our REST API server.</span></span>

### <a name="about-routes-in-restify"></a><span data-ttu-id="4c94e-258">O trasách v Restify</span><span class="sxs-lookup"><span data-stu-id="4c94e-258">About routes in Restify</span></span>
<span data-ttu-id="4c94e-259">Pracovní trasy v Restify hello stejný způsob, jak se v hello Express zásobníku.</span><span class="sxs-lookup"><span data-stu-id="4c94e-259">Routes work in Restify hello same way they do in hello Express stack.</span></span> <span data-ttu-id="4c94e-260">Trasy se definují pomocí identifikátoru URI, které předpokládáte hello klienta aplikace toocall hello.</span><span class="sxs-lookup"><span data-stu-id="4c94e-260">You define routes by using hello URI that you expect hello client applications toocall.</span></span> <span data-ttu-id="4c94e-261">Trasy se obvykle definovat v samostatném souboru.</span><span class="sxs-lookup"><span data-stu-id="4c94e-261">Usually, you define your routes in a separate file.</span></span> <span data-ttu-id="4c94e-262">Pro naše účely jsme do souboru server.js hello umístit naše trasy.</span><span class="sxs-lookup"><span data-stu-id="4c94e-262">For our purposes, we put our routes in hello server.js file.</span></span> <span data-ttu-id="4c94e-263">Doporučujeme, abyste je zvážit tyto trasy do své vlastní souboru pro použití v provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="4c94e-263">We recommend that you factor these routes into their own file for production use.</span></span>

<span data-ttu-id="4c94e-264">Typický vzor trasy Restify je následující:</span><span class="sxs-lookup"><span data-stu-id="4c94e-264">A typical pattern for a Restify route is as follows:</span></span>

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


<span data-ttu-id="4c94e-265">Toto je vzor hello na nejzákladnější úrovni.</span><span class="sxs-lookup"><span data-stu-id="4c94e-265">This is hello pattern at its most basic level.</span></span> <span data-ttu-id="4c94e-266">Restify (a Express) poskytují mnohem hlubší funkčnost, jako je například definování typů aplikací a zajištění komplexního trasování napříč různými koncovými body.</span><span class="sxs-lookup"><span data-stu-id="4c94e-266">Restify (and Express) provide much deeper functionality, such as defining application types and providing complex routing across different endpoints.</span></span> <span data-ttu-id="4c94e-267">Pro naše účely jsme jsou zachování tyto trasy jednoduché.</span><span class="sxs-lookup"><span data-stu-id="4c94e-267">For our purposes, we are keeping these routes simple.</span></span>

### <a name="add-default-routes-tooour-server"></a><span data-ttu-id="4c94e-268">Přidat výchozí trasy tooour server</span><span class="sxs-lookup"><span data-stu-id="4c94e-268">Add default routes tooour server</span></span>
<span data-ttu-id="4c94e-269">Jsme teď přidejte hello základní trasy CRUD vytvořit, načtení, aktualizace a odstranění.</span><span class="sxs-lookup"><span data-stu-id="4c94e-269">We now add hello basic CRUD routes of Create, Retrieve, Update, and Delete.</span></span>

1. <span data-ttu-id="4c94e-270">Z příkazového řádku hello, změňte adresáře toohello **azuread** složky, pokud si nejste již existuje:</span><span class="sxs-lookup"><span data-stu-id="4c94e-270">From hello command line, change directories toohello **azuread** folder if you're not already there:</span></span>

    `cd azuread`

2. <span data-ttu-id="4c94e-271">Otevřete hello `server.js` souboru ve svém oblíbeném editoru a poté přidejte následující informace níže hello databáze položky, které jste provedli hello:</span><span class="sxs-lookup"><span data-stu-id="4c94e-271">Open hello `server.js` file in your favorite editor, and then add hello following information below hello previous database entries that you made:</span></span>

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

### <a name="add-error-handling-in-our-apis"></a><span data-ttu-id="4c94e-272">Přidání zpracování chyb v našem rozhraní API</span><span class="sxs-lookup"><span data-stu-id="4c94e-272">Add error handling in our APIs</span></span>
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


## <a name="step-15-create-your-server"></a><span data-ttu-id="4c94e-273">Krok 15: Vytvoření serveru</span><span class="sxs-lookup"><span data-stu-id="4c94e-273">Step 15: Create your server</span></span>
<span data-ttu-id="4c94e-274">Jsme definovali naše databáze a naše trasy jsou na místě.</span><span class="sxs-lookup"><span data-stu-id="4c94e-274">We have defined our database and our routes are in place.</span></span> <span data-ttu-id="4c94e-275">poslední věcí toodo Hello je přidat hello instanci serveru, která spravuje naše volání.</span><span class="sxs-lookup"><span data-stu-id="4c94e-275">hello last thing toodo is add hello server instance that manages our calls.</span></span>

<span data-ttu-id="4c94e-276">V Restify (a Express) lze provádět mnoho přizpůsobení serveru REST API, ale znova budeme toouse hello nejzákladnější nastavení pro naše účely.</span><span class="sxs-lookup"><span data-stu-id="4c94e-276">In Restify (and Express) you can do a lot of customization for a REST API server, but again we are going toouse hello most basic setup for our purposes.</span></span>

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

## <a name="step-16-add-hello-routes-toohello-server-without-authentication-for-now"></a><span data-ttu-id="4c94e-277">Krok 16: Přidejte server toohello hello tras (bez ověřování prozatím)</span><span class="sxs-lookup"><span data-stu-id="4c94e-277">Step 16: Add hello routes toohello server (without authentication for now)</span></span>
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

## <a name="step-17-run-hello-server-before-adding-oauth-support"></a><span data-ttu-id="4c94e-278">Krok 17: Spusťte hello server (před přidáním podpory OAuth)</span><span class="sxs-lookup"><span data-stu-id="4c94e-278">Step 17: Run hello server (before adding OAuth support)</span></span>
<span data-ttu-id="4c94e-279">Otestovat váš server před přidáme ověřování.</span><span class="sxs-lookup"><span data-stu-id="4c94e-279">Test out your server before we add authentication.</span></span>

<span data-ttu-id="4c94e-280">Nejjednodušší způsob, jak tootest Hello serveru je pomocí curl v příkazovém řádku.</span><span class="sxs-lookup"><span data-stu-id="4c94e-280">hello easiest way tootest your server is by using curl in a command line.</span></span> <span data-ttu-id="4c94e-281">Než to, potřebujeme nástroj, který umožňuje nám tooparse výstup jako JSON.</span><span class="sxs-lookup"><span data-stu-id="4c94e-281">Before we do that, we need a utility that allows us tooparse output as JSON.</span></span>

1. <span data-ttu-id="4c94e-282">Nainstalujte hello následující nástroj JSON (Tento nástroj použijte všechny hello následující příklady):</span><span class="sxs-lookup"><span data-stu-id="4c94e-282">Install hello following JSON tool (all hello following examples use this tool):</span></span>

    `$npm install -g jsontool`

    <span data-ttu-id="4c94e-283">To globálně nainstaluje nástroj JSON hello.</span><span class="sxs-lookup"><span data-stu-id="4c94e-283">This installs hello JSON tool globally.</span></span> <span data-ttu-id="4c94e-284">Teď, když jsme který udělat, budeme přehrání hello serveru:</span><span class="sxs-lookup"><span data-stu-id="4c94e-284">Now that we’ve accomplished that, let’s play with hello server:</span></span>

2. <span data-ttu-id="4c94e-285">Nejprve se ujistěte, že je spuštěna mongoDB instance:</span><span class="sxs-lookup"><span data-stu-id="4c94e-285">First, make sure that your mongoDB instance is running:</span></span>

    `$sudo mongod`

3. <span data-ttu-id="4c94e-286">Potom změňte adresář toohello a spustit kulmy:</span><span class="sxs-lookup"><span data-stu-id="4c94e-286">Then, change toohello directory and start curling:</span></span>

    <span data-ttu-id="4c94e-287">`$ cd azuread` `$ node server.js`</span><span class="sxs-lookup"><span data-stu-id="4c94e-287">`$ cd azuread` `$ node server.js`</span></span>

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

4. <span data-ttu-id="4c94e-288">Potom jsme můžete přidat úloha tímto způsobem:</span><span class="sxs-lookup"><span data-stu-id="4c94e-288">Then, we can add a task this way:</span></span>

    `$ curl -isS -X POST http://127.0.0.1:8080/tasks/brandon/Hello`

    <span data-ttu-id="4c94e-289">Hello odpověď by měla být:</span><span class="sxs-lookup"><span data-stu-id="4c94e-289">hello response should be:</span></span>

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
    <span data-ttu-id="4c94e-290">A zde jsou uvedeny úlohy pro Brandon tímto způsobem:</span><span class="sxs-lookup"><span data-stu-id="4c94e-290">And we can list tasks for Brandon this way:</span></span>

        `$ curl -isS http://127.0.0.1:8080/tasks/brandon/`

<span data-ttu-id="4c94e-291">Pokud to vše funguje, nám server REST API toohello připraven tooadd OAuth.</span><span class="sxs-lookup"><span data-stu-id="4c94e-291">If all this works, we're ready tooadd OAuth toohello REST API server.</span></span>

<span data-ttu-id="4c94e-292">Máte server REST API s MongoDB!</span><span class="sxs-lookup"><span data-stu-id="4c94e-292">You have a REST API server with MongoDB!</span></span>

## <a name="step-18-add-authentication-tooour-rest-api-server"></a><span data-ttu-id="4c94e-293">Krok 18: Přidejte server REST API tooour ověřování</span><span class="sxs-lookup"><span data-stu-id="4c94e-293">Step 18: Add authentication tooour REST API server</span></span>
<span data-ttu-id="4c94e-294">Teď, když máme spuštěné rozhraní REST API, Začněme jeho užitečnost s Azure AD.</span><span class="sxs-lookup"><span data-stu-id="4c94e-294">Now that we have a running REST API, let's start making it useful with Azure AD.</span></span>

<span data-ttu-id="4c94e-295">Z příkazového řádku hello, změňte adresáře toohello **azuread** složky, pokud si nejste již existuje.</span><span class="sxs-lookup"><span data-stu-id="4c94e-295">From hello command line, change directories toohello **azuread** folder if you're not already there.</span></span>

`cd azuread`

### <a name="use-hello-oidcbearerstrategy-that-is-included-with-passport-azure-ad"></a><span data-ttu-id="4c94e-296">Hello použití OIDCBearerStrategy, která je součástí passport-azure-ad</span><span class="sxs-lookup"><span data-stu-id="4c94e-296">Use hello OIDCBearerStrategy that is included with passport-azure-ad</span></span>
<span data-ttu-id="4c94e-297">Pokud jsme jste vytvořili typický server REST se seznamem úkolů bez jakéhokoli druhu ověřování.</span><span class="sxs-lookup"><span data-stu-id="4c94e-297">So far we have built a typical REST TODO server without any kind of authorization.</span></span> <span data-ttu-id="4c94e-298">Toto je, kde začneme, které připravuje umístění.</span><span class="sxs-lookup"><span data-stu-id="4c94e-298">This is where we start putting that together.</span></span>

1. <span data-ttu-id="4c94e-299">Nejdřív potřebujeme tooindicate, že má být toouse Passport.</span><span class="sxs-lookup"><span data-stu-id="4c94e-299">First, we need tooindicate that we want toouse Passport.</span></span> <span data-ttu-id="4c94e-300">Toto právo PUT po ostatní konfigurace serveru:</span><span class="sxs-lookup"><span data-stu-id="4c94e-300">Put this right after your other server configuration:</span></span>

    ```Javascript
            // Let's start using Passport.js.

            server.use(passport.initialize()); // Starts passport.
            server.use(passport.session()); // Provides session support.
    ```
    > [!TIP]
    > <span data-ttu-id="4c94e-301">Při psaní rozhraní API, doporučujeme vždy odkaz hello data toosomething jedinečné z hello token, který hello uživatel nemůže zfalšovat.</span><span class="sxs-lookup"><span data-stu-id="4c94e-301">When you write APIs, we recommend that you always link hello data toosomething unique from hello token that hello user can’t spoof.</span></span> <span data-ttu-id="4c94e-302">Pokud tento server ukládá položky úkolů, uloží je na základě ID objektu hello hello uživatele v hello tokenu (zavolaném prostřednictvím token.oid), který jsme umístit do pole "vlastník" hello.</span><span class="sxs-lookup"><span data-stu-id="4c94e-302">When this server stores TODO items, it stores them based on hello object ID of hello user in hello token (called through token.oid), which we put in hello “owner” field.</span></span> <span data-ttu-id="4c94e-303">To zajišťuje, aby pouze tento uživatel může přístup k jejich TODOs.</span><span class="sxs-lookup"><span data-stu-id="4c94e-303">This ensures that only that user can access their TODOs.</span></span> <span data-ttu-id="4c94e-304">Není nijak neprojevuje v hello rozhraní API "vlastník", takže externí uživatel může požádat o hello TODOs jiných, i když jsou ověřeni.</span><span class="sxs-lookup"><span data-stu-id="4c94e-304">There is no exposure in hello API of “owner,” so an external user can request hello TODOs of others even if they are authenticated.</span></span>                    

2. <span data-ttu-id="4c94e-305">Další použijeme hello nosnou strategii, která se dodává s `passport-azure-ad`.</span><span class="sxs-lookup"><span data-stu-id="4c94e-305">Next let’s use hello bearer strategy that comes with `passport-azure-ad`.</span></span> <span data-ttu-id="4c94e-306">Podívejte se na kód hello prozatím a vysvětlíme hello rest za chvíli.</span><span class="sxs-lookup"><span data-stu-id="4c94e-306">Look at hello code for now and we'll explain hello rest shortly.</span></span> <span data-ttu-id="4c94e-307">To uvedli po vložení výše:</span><span class="sxs-lookup"><span data-stu-id="4c94e-307">Put this after what you pasted above:</span></span>

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

<span data-ttu-id="4c94e-308">Passport používá podobný Princip pro všechny svoje strategie (Twitteru, Facebooku a tak dále), které řídí všichni autoři strategií k.</span><span class="sxs-lookup"><span data-stu-id="4c94e-308">Passport uses a similar pattern for all its strategies (Twitter, Facebook, and so on) that all strategy writers adhere to.</span></span> <span data-ttu-id="4c94e-309">Prohlížení hello strategie, uvidíte, že jsme předat funkci, která má token a done jako parametry hello.</span><span class="sxs-lookup"><span data-stu-id="4c94e-309">Looking at hello strategy, you see we pass it a function that has a token and a done as hello parameters.</span></span> <span data-ttu-id="4c94e-310">po jeho činnosti provede se dodává zpět toous strategie Hello.</span><span class="sxs-lookup"><span data-stu-id="4c94e-310">hello strategy comes back toous after it does its work.</span></span> <span data-ttu-id="4c94e-311">Po Ano, uložíme hello uživatele a dočasné ukládání hello token tak nebude potřebujeme tooask pro něj znovu.</span><span class="sxs-lookup"><span data-stu-id="4c94e-311">After it does, we store hello user and stash hello token so we won’t need tooask for it again.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="4c94e-312">Předchozí kód Hello trvá každý uživatel, který se stane tooauthenticate tooour serveru.</span><span class="sxs-lookup"><span data-stu-id="4c94e-312">hello previous code takes any user that happens tooauthenticate tooour server.</span></span> <span data-ttu-id="4c94e-313">To se označuje jako Automatická registrace.</span><span class="sxs-lookup"><span data-stu-id="4c94e-313">This is known as auto-registration.</span></span> <span data-ttu-id="4c94e-314">Na produkčních serverech, které doporučujeme si nechat každý, kdo aniž by bylo nejdříve je přejdete prostřednictvím procesu registrace, který se rozhodnete.</span><span class="sxs-lookup"><span data-stu-id="4c94e-314">In production servers, you we recommend that you don't let anyone in without first having them go through a registration process that you decide on.</span></span> <span data-ttu-id="4c94e-315">Je to obvykle hello vzor, který můžete vidět u uživatelských aplikací, které umožňují tooregister službou Facebook, ale poté vás požádají toofill Další informace.</span><span class="sxs-lookup"><span data-stu-id="4c94e-315">This is usually hello pattern you see in consumer apps, which allow you tooregister with Facebook but then ask you toofill out additional information.</span></span> <span data-ttu-id="4c94e-316">Pokud to nebyli příkazového řádku programu, mohli bychom extrahovat hello e-mailu z hello tokenu objektu, který je vrácen a poté požádat uživatele toofill hello Další informace.</span><span class="sxs-lookup"><span data-stu-id="4c94e-316">If this wasn’t a command-line program, we could have extracted hello email from hello token object that is returned and then asked hello user toofill out additional information.</span></span> <span data-ttu-id="4c94e-317">Protože se jedná o testovací server, jednoduše přidáme je toohello databáze v paměti.</span><span class="sxs-lookup"><span data-stu-id="4c94e-317">Because this is a test server, we simply add them toohello in-memory database.</span></span>
>
>

### <a name="protect-some-endpoints"></a><span data-ttu-id="4c94e-318">Ochrana některé koncových bodů</span><span class="sxs-lookup"><span data-stu-id="4c94e-318">Protect some endpoints</span></span>
<span data-ttu-id="4c94e-319">Ochrana koncových bodů tak, že zadáte hello `passport.authenticate()` volání s hello protokolu, které chcete toouse.</span><span class="sxs-lookup"><span data-stu-id="4c94e-319">You protect endpoints by specifying hello `passport.authenticate()` call with hello protocol that you want toouse.</span></span>

<span data-ttu-id="4c94e-320">toomake naše kódu serveru udělat něco další zajímavé, Pojďme upravit hello směrování.</span><span class="sxs-lookup"><span data-stu-id="4c94e-320">toomake our server code do something more interesting, let’s edit hello route.</span></span>

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

## <a name="step-19-run-your-server-application-again-and-ensure-it-rejects-you"></a><span data-ttu-id="4c94e-321">Krok 19: Spusťte aplikaci server znovu a ujistěte se, že vás odmítne</span><span class="sxs-lookup"><span data-stu-id="4c94e-321">Step 19: Run your server application again and ensure it rejects you</span></span>
<span data-ttu-id="4c94e-322">Použijeme `curl` znovu toosee, pokud bychom nyní měli chráněné pomocí OAuth2 proti naše koncové body.</span><span class="sxs-lookup"><span data-stu-id="4c94e-322">Let's use `curl` again toosee if we now have OAuth2 protection against our endpoints.</span></span> <span data-ttu-id="4c94e-323">Provedeme tento test před s některým z klientskou sadu SDK proti tomuto koncovému bodu.</span><span class="sxs-lookup"><span data-stu-id="4c94e-323">We do this test before running any of our client SDKs against this endpoint.</span></span> <span data-ttu-id="4c94e-324">Hello vrácené hlavičky by měl být dostatek tootell nám Pokud vytvoříme dolů hello správné cestě.</span><span class="sxs-lookup"><span data-stu-id="4c94e-324">hello headers that are returned should be enough tootell us if we're going down hello right path.</span></span>

1. <span data-ttu-id="4c94e-325">Nejprve se ujistěte, že je spuštěna mongoDB instance:</span><span class="sxs-lookup"><span data-stu-id="4c94e-325">First, make sure that your mongoDB instance is running:</span></span>

    `$sudo mongod`

2. <span data-ttu-id="4c94e-326">Potom změnit adresář, toohello a spusťte kulmy.</span><span class="sxs-lookup"><span data-stu-id="4c94e-326">Then, change toohello directory and start curling.</span></span>

      <span data-ttu-id="4c94e-327">`$ cd azuread` `$ node server.js`</span><span class="sxs-lookup"><span data-stu-id="4c94e-327">`$ cd azuread` `$ node server.js`</span></span>

3. <span data-ttu-id="4c94e-328">Zkuste základní požadavek POST.</span><span class="sxs-lookup"><span data-stu-id="4c94e-328">Try a basic POST.</span></span>

    `$ curl -isS -X POST http://127.0.0.1:8080/tasks/brandon/Hello`

    ```Shell
    HTTP/1.1 401 Unauthorized
    Connection: close
    WWW-Authenticate: Bearer realm="Users"
    Date: Tue, 14 Jul 2015 05:45:03 GMT
    Transfer-Encoding: chunked
    ```

<span data-ttu-id="4c94e-329">401 je odpověď hello, kterou hledáte sem.</span><span class="sxs-lookup"><span data-stu-id="4c94e-329">A 401 is hello response you are looking for here.</span></span> <span data-ttu-id="4c94e-330">Tato odezva označuje, že vrstva Passportu hello se pokouší tooredirect toohello autorizovaný koncový bod, který je právě co chcete použít.</span><span class="sxs-lookup"><span data-stu-id="4c94e-330">This response indicates that hello Passport layer is trying tooredirect toohello authorized endpoint, which is exactly what you want.</span></span>

## <a name="next-steps"></a><span data-ttu-id="4c94e-331">Další kroky</span><span class="sxs-lookup"><span data-stu-id="4c94e-331">Next steps</span></span>
<span data-ttu-id="4c94e-332">Jste došli nejdál, co můžete s tímto serverem bez použití klientem kompatibilní OAuth2.</span><span class="sxs-lookup"><span data-stu-id="4c94e-332">You've gone as far as you can with this server without using an OAuth2 compatible client.</span></span> <span data-ttu-id="4c94e-333">Budete potřebovat toogo prostřednictvím další návod.</span><span class="sxs-lookup"><span data-stu-id="4c94e-333">You will need toogo through an additional walkthrough.</span></span>

<span data-ttu-id="4c94e-334">Naučili jste se teď jak tooimplement rozhraní REST API pomocí Restify a OAuth2.</span><span class="sxs-lookup"><span data-stu-id="4c94e-334">You've now learned how tooimplement a REST API by using Restify and OAuth2.</span></span> <span data-ttu-id="4c94e-335">Máte víc než dost kód tookeep vývoj služby a učení jak toobuild v tomto příkladu.</span><span class="sxs-lookup"><span data-stu-id="4c94e-335">You also have more than enough code tookeep developing your service and learning how toobuild on this example.</span></span>

<span data-ttu-id="4c94e-336">Pokud vás zajímají další kroky hello ve vaší ADAL cesty, tady jsou některé podporované klienty ADAL, doporučujeme, můžete pokračovat v práci s.</span><span class="sxs-lookup"><span data-stu-id="4c94e-336">If you are interested in hello next steps in your ADAL journey, here are some supported ADAL clients we recommend that you  keep working with.</span></span>

<span data-ttu-id="4c94e-337">Klonování dolů tooyour vývojáře počítače a nakonfigurovat, jak je popsáno v Průvodci hello.</span><span class="sxs-lookup"><span data-stu-id="4c94e-337">Clone down tooyour developer machine and configure as described in hello walkthrough.</span></span>

[<span data-ttu-id="4c94e-338">ADAL pro iOS</span><span class="sxs-lookup"><span data-stu-id="4c94e-338">ADAL for iOS</span></span>](https://github.com/MSOpenTech/azure-activedirectory-library-for-ios)

[<span data-ttu-id="4c94e-339">ADAL pro Android</span><span class="sxs-lookup"><span data-stu-id="4c94e-339">ADAL for Android</span></span>](https://github.com/MSOpenTech/azure-activedirectory-library-for-android)

[!INCLUDE [active-directory-devquickstarts-additional-resources](../../../includes/active-directory-devquickstarts-additional-resources.md)]
