---
title: aaaHow toowork se serverem back-end Node.js hello SDK pro Mobile Apps | Microsoft Docs
description: "Zjistěte, jak toowork s hello serveru back-end Node.js SDK pro Azure App Service Mobile Apps."
services: app-service\mobile
documentationcenter: 
author: ggailey777
manager: syntaxc4
editor: 
ms.assetid: e7d97d3b-356e-4fb3-ba88-38ecbda5ea50
ms.service: app-service-mobile
ms.workload: mobile
ms.tgt_pltfrm: mobile-multiple
ms.devlang: node
ms.topic: article
ms.date: 10/01/2016
ms.author: glenga
ms.openlocfilehash: 2b1ea5fda6f6ca422b92fe29ff8d16bf035018d9
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="how-toouse-hello-azure-mobile-apps-nodejs-sdk"></a><span data-ttu-id="166bc-103">Jak toouse hello Azure Mobile Apps Node.js SDK</span><span class="sxs-lookup"><span data-stu-id="166bc-103">How toouse hello Azure Mobile Apps Node.js SDK</span></span>
[!INCLUDE [app-service-mobile-selector-server-sdk](../../includes/app-service-mobile-selector-server-sdk.md)]

<span data-ttu-id="166bc-104">Tento článek obsahuje podrobné informace a příklady zobrazující jak toowork s back-end Node.js v Azure App Service Mobile Apps.</span><span class="sxs-lookup"><span data-stu-id="166bc-104">This article provides detailed information and examples showing how toowork with a Node.js backend in Azure App Service Mobile Apps.</span></span>

## <span data-ttu-id="166bc-105"><a name="Introduction"></a>Úvod</span><span class="sxs-lookup"><span data-stu-id="166bc-105"><a name="Introduction"></a>Introduction</span></span>
<span data-ttu-id="166bc-106">Azure App Service Mobile Apps poskytuje hello schopností tooadd mobile optimalizovaná data přístup webového rozhraní API tooa webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="166bc-106">Azure App Service Mobile Apps provides hello capability tooadd a mobile-optimized data access Web API tooa web application.</span></span>  <span data-ttu-id="166bc-107">Hello Azure App Service Mobile Apps SDK se poskytuje pro webové aplikace ASP.NET a Node.js.</span><span class="sxs-lookup"><span data-stu-id="166bc-107">hello Azure App Service Mobile Apps SDK is provided for ASP.NET and Node.js web applications.</span></span>  <span data-ttu-id="166bc-108">Hello SDK poskytuje hello následující operace:</span><span class="sxs-lookup"><span data-stu-id="166bc-108">hello SDK provides hello following operations:</span></span>

* <span data-ttu-id="166bc-109">Operace s tabulkou (pro čtení, příkaz Insert, Update, Delete) pro přístup k datům</span><span class="sxs-lookup"><span data-stu-id="166bc-109">Table operations (Read, Insert, Update, Delete) for data access</span></span>
* <span data-ttu-id="166bc-110">Operace vlastní rozhraní API</span><span class="sxs-lookup"><span data-stu-id="166bc-110">Custom API operations</span></span>

<span data-ttu-id="166bc-111">Obě operace zajištění ověřování napříč všech zprostředkovatelů identity povolené ve službě Azure App Service, včetně poskytovatelů sociálních identit jako je Facebook, Twitter, Google a Microsoft a také Azure Active Directory pro podnikové identity.</span><span class="sxs-lookup"><span data-stu-id="166bc-111">Both operations provide for authentication across all identity providers allowed by Azure App Service, including social identity providers such as Facebook, Twitter, Google and Microsoft as well as Azure Active Directory for enterprise identity.</span></span>

<span data-ttu-id="166bc-112">Můžete najít ukázky pro každý případ použití v hello [ukázky adresáře na Githubu].</span><span class="sxs-lookup"><span data-stu-id="166bc-112">You can find samples for each use case in hello [samples directory on GitHub].</span></span>

## <a name="supported-platforms"></a><span data-ttu-id="166bc-113">Podporované platformy</span><span class="sxs-lookup"><span data-stu-id="166bc-113">Supported Platforms</span></span>
<span data-ttu-id="166bc-114">Hello Azure Mobile Apps uzlu SDK podporuje hello LTS aktuální verzi uzlu a později.</span><span class="sxs-lookup"><span data-stu-id="166bc-114">hello Azure Mobile Apps Node SDK supports hello current LTS release of Node and later.</span></span>  <span data-ttu-id="166bc-115">Od verze zápis, nejnovější verze LTS hello je v4.5.0 uzlu.</span><span class="sxs-lookup"><span data-stu-id="166bc-115">As of writing, hello latest LTS version is Node v4.5.0.</span></span>  <span data-ttu-id="166bc-116">Jiné verze uzlu může fungovat, ale nejsou podporovány.</span><span class="sxs-lookup"><span data-stu-id="166bc-116">Other versions of Node may work, but are not supported.</span></span>

<span data-ttu-id="166bc-117">Hello Azure Mobile Apps uzlu SDK podporuje dvě databáze ovladače – hello uzlu mssql ovladač podporuje SQL Azure a místní instance systému SQL Server.</span><span class="sxs-lookup"><span data-stu-id="166bc-117">hello Azure Mobile Apps Node SDK supports two database drivers - hello node-mssql driver supports SQL Azure and local SQL Server instances.</span></span>  <span data-ttu-id="166bc-118">ovladač sqlite3 Hello podporuje pouze do jedné instance databáze SQLite.</span><span class="sxs-lookup"><span data-stu-id="166bc-118">hello sqlite3 driver supports SQLite databases on a single instance only.</span></span>

### <span data-ttu-id="166bc-119"><a name="howto-cmdline-basicapp"></a>Postupy: vytvoření základní Node.js back-end pomocí hello příkazového řádku</span><span class="sxs-lookup"><span data-stu-id="166bc-119"><a name="howto-cmdline-basicapp"></a>How to: Create a Basic Node.js backend using hello Command Line</span></span>
<span data-ttu-id="166bc-120">Všechny back-end Azure App Service Mobile aplikace Node.js se spustí jako ExpressJS aplikace.</span><span class="sxs-lookup"><span data-stu-id="166bc-120">Every Azure App Service Mobile App Node.js backend starts as an ExpressJS application.</span></span>  <span data-ttu-id="166bc-121">ExpressJS je hello nejoblíbenější webové služby rozhraní k dispozici pro Node.js.</span><span class="sxs-lookup"><span data-stu-id="166bc-121">ExpressJS is hello most popular web service framework available for Node.js.</span></span>  <span data-ttu-id="166bc-122">Můžete vytvořit základní [Express] aplikace následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="166bc-122">You can create a basic [Express] application as follows:</span></span>

1. <span data-ttu-id="166bc-123">V příkazu nebo v okně prostředí PowerShell vytvořte adresář pro váš projekt.</span><span class="sxs-lookup"><span data-stu-id="166bc-123">In a command or PowerShell window, create a directory for your project.</span></span>

        mkdir basicapp
2. <span data-ttu-id="166bc-124">Spusťte struktury balíčku npm init tooinitialize hello.</span><span class="sxs-lookup"><span data-stu-id="166bc-124">Run npm init tooinitialize hello package structure.</span></span>

        cd basicapp
        npm init

    <span data-ttu-id="166bc-125">příkaz init npm Hello požádá sadu otázky tooinitialize hello projektu.</span><span class="sxs-lookup"><span data-stu-id="166bc-125">hello npm init command asks a set of questions tooinitialize hello project.</span></span>  <span data-ttu-id="166bc-126">V tématu hello příklad výstupu:</span><span class="sxs-lookup"><span data-stu-id="166bc-126">See hello example output:</span></span>

    ![výstup init npm Hello][0]
3. <span data-ttu-id="166bc-128">Nainstalujte hello express a aplikace azure mobile knihovny z úložiště npm hello.</span><span class="sxs-lookup"><span data-stu-id="166bc-128">Install hello express and azure-mobile-apps libraries from hello npm repository.</span></span>

        npm install --save express azure-mobile-apps
4. <span data-ttu-id="166bc-129">Vytvoření app.js souboru tooimplement hello základní mobilní serveru.</span><span class="sxs-lookup"><span data-stu-id="166bc-129">Create an app.js file tooimplement hello basic mobile server.</span></span>

        var express = require('express'),
            azureMobileApps = require('azure-mobile-apps');

        var app = express(),
            mobile = azureMobileApps();

        // Define a TodoItem table
        mobile.tables.add('TodoItem');

        // Add hello mobile API so it is accessible as a Web API
        app.use(mobile);

        // Start listening on HTTP
        app.listen(process.env.PORT || 3000);

<span data-ttu-id="166bc-130">Tato aplikace vytvoří WebAPI se mobile optimalizované s jeden koncový bod (`/tables/TodoItem`), který poskytuje přístup bez ověřování tooan základní úložiště dat SQL pomocí dynamické schématu.</span><span class="sxs-lookup"><span data-stu-id="166bc-130">This application creates a mobile-optimized WebAPI with a single endpoint (`/tables/TodoItem`) that provides unauthenticated access tooan underlying SQL data store using a dynamic schema.</span></span>  <span data-ttu-id="166bc-131">Je vhodné pro následující rychlé spuštění knihovny klienta:</span><span class="sxs-lookup"><span data-stu-id="166bc-131">It is suitable for following the client library quick starts:</span></span>

* <span data-ttu-id="166bc-132">[Rychlý start Android klienta]</span><span class="sxs-lookup"><span data-stu-id="166bc-132">[Android Client QuickStart]</span></span>
* <span data-ttu-id="166bc-133">[Rychlý start Apache Cordova klienta]</span><span class="sxs-lookup"><span data-stu-id="166bc-133">[Apache Cordova Client QuickStart]</span></span>
* <span data-ttu-id="166bc-134">[iOS klienta rychlý start]</span><span class="sxs-lookup"><span data-stu-id="166bc-134">[iOS Client QuickStart]</span></span>
* <span data-ttu-id="166bc-135">[Rychlé spuštění klienta Windows Store]</span><span class="sxs-lookup"><span data-stu-id="166bc-135">[Windows Store Client QuickStart]</span></span>
* <span data-ttu-id="166bc-136">[Rychlý start Xamarin.iOS klienta]</span><span class="sxs-lookup"><span data-stu-id="166bc-136">[Xamarin.iOS Client QuickStart]</span></span>
* <span data-ttu-id="166bc-137">[Rychlé spuštění klienta Xamarin.Android]</span><span class="sxs-lookup"><span data-stu-id="166bc-137">[Xamarin.Android Client QuickStart]</span></span>
* <span data-ttu-id="166bc-138">[Rychlý start Xamarin.Forms klienta]</span><span class="sxs-lookup"><span data-stu-id="166bc-138">[Xamarin.Forms Client QuickStart]</span></span>

<span data-ttu-id="166bc-139">Můžete najít hello kód pro tuto základní aplikaci v hello [basicapp ukázce na Githubu].</span><span class="sxs-lookup"><span data-stu-id="166bc-139">You can find hello code for this basic application in hello [basicapp sample on GitHub].</span></span>

### <span data-ttu-id="166bc-140"><a name="howto-vs2015-basicapp"></a>Postupy: vytvoření back-end uzlu pomocí sady Visual Studio 2015</span><span class="sxs-lookup"><span data-stu-id="166bc-140"><a name="howto-vs2015-basicapp"></a>How to: Create a Node backend with Visual Studio 2015</span></span>
<span data-ttu-id="166bc-141">Visual Studio 2015 vyžaduje rozšíření toodevelop Node.js aplikací v rámci hello IDE.</span><span class="sxs-lookup"><span data-stu-id="166bc-141">Visual Studio 2015 requires an extension toodevelop Node.js applications within hello IDE.</span></span>  <span data-ttu-id="166bc-142">toostart, instalace hello [Node.js Tools 1.1 pro sadu Visual Studio].</span><span class="sxs-lookup"><span data-stu-id="166bc-142">toostart, install hello [Node.js Tools 1.1 for Visual Studio].</span></span>  <span data-ttu-id="166bc-143">Po instalaci nástrojů pro Node.js hello pro sadu Visual Studio vytvořte aplikaci 4.x Express:</span><span class="sxs-lookup"><span data-stu-id="166bc-143">Once hello Node.js Tools for Visual Studio are installed, create an Express 4.x application:</span></span>

1. <span data-ttu-id="166bc-144">Otevřete hello **nový projekt** dialogové okno (z **soubor** > **nový** > **projekt...** ).</span><span class="sxs-lookup"><span data-stu-id="166bc-144">Open hello **New Project** dialog (from **File** > **New** > **Project...**).</span></span>
2. <span data-ttu-id="166bc-145">Rozbalte položku **šablony** > **JavaScript** > **Node.js**.</span><span class="sxs-lookup"><span data-stu-id="166bc-145">Expand **Templates** > **JavaScript** > **Node.js**.</span></span>
3. <span data-ttu-id="166bc-146">Vyberte hello **základní Azure Node.js Express 4 aplikační**.</span><span class="sxs-lookup"><span data-stu-id="166bc-146">Select hello **Basic Azure Node.js Express 4 Application**.</span></span>
4. <span data-ttu-id="166bc-147">Zadejte název projektu hello.</span><span class="sxs-lookup"><span data-stu-id="166bc-147">Fill in hello project name.</span></span>  <span data-ttu-id="166bc-148">Klikněte na tlačítko *OK*.</span><span class="sxs-lookup"><span data-stu-id="166bc-148">Click *OK*.</span></span>

    ![Nový projekt sady Visual Studio 2015][1]
5. <span data-ttu-id="166bc-150">Klikněte pravým tlačítkem na hello **npm** uzel a vyberte možnost **nainstalovat nové balíčky npm...** .</span><span class="sxs-lookup"><span data-stu-id="166bc-150">Right-click hello **npm** node and select **Install New npm packages...**.</span></span>
6. <span data-ttu-id="166bc-151">Může být nutné toorefresh hello npm katalogu na vytvoření vaší první aplikace Node.js.</span><span class="sxs-lookup"><span data-stu-id="166bc-151">You may need toorefresh hello npm catalog on creating your first Node.js application.</span></span>  <span data-ttu-id="166bc-152">Klikněte na tlačítko **aktualizovat** v případě potřeby.</span><span class="sxs-lookup"><span data-stu-id="166bc-152">Click **Refresh** if necessary.</span></span>
7. <span data-ttu-id="166bc-153">Zadejte *aplikace azure mobile* hello vyhledávacího pole.</span><span class="sxs-lookup"><span data-stu-id="166bc-153">Enter *azure-mobile-apps* in hello search box.</span></span>  <span data-ttu-id="166bc-154">Klikněte na tlačítko hello **azure mobile apps 2.0.0** balíček a potom klikněte na **instalovat balíček**.</span><span class="sxs-lookup"><span data-stu-id="166bc-154">Click hello **azure-mobile-apps 2.0.0** package, then click **Install Package**.</span></span>

    ![Instalace nové balíčky npm][2]
8. <span data-ttu-id="166bc-156">Klikněte na **Zavřít**.</span><span class="sxs-lookup"><span data-stu-id="166bc-156">Click **Close**.</span></span>
9. <span data-ttu-id="166bc-157">Otevřete hello *app.js* tooadd podpora pro Azure Mobile Apps SDK hello souborů.</span><span class="sxs-lookup"><span data-stu-id="166bc-157">Open hello *app.js* file tooadd support for hello Azure Mobile Apps SDK.</span></span>  <span data-ttu-id="166bc-158">V řádku 6 at hello dolní hello knihovně vyžadují příkazy, přidejte následující kód hello:</span><span class="sxs-lookup"><span data-stu-id="166bc-158">At line 6 at hello bottom of hello library require statements, add hello following code:</span></span>

        var bodyParser = require('body-parser');
        var azureMobileApps = require('azure-mobile-apps');

    <span data-ttu-id="166bc-159">Na přibližně řádku 27 po hello přidat další příkazy app.use hello následující kód:</span><span class="sxs-lookup"><span data-stu-id="166bc-159">At approximately line 27 after hello other app.use statements, add hello following code:</span></span>

        app.use('/users', users);

        // Azure Mobile Apps Initialization
        var mobile = azureMobileApps();
        mobile.tables.add('TodoItem');
        app.use(mobile);

    <span data-ttu-id="166bc-160">Uložte soubor hello.</span><span class="sxs-lookup"><span data-stu-id="166bc-160">Save hello file.</span></span>
10. <span data-ttu-id="166bc-161">Spusťte místně aplikace hello (hello rozhraní API je vyhovovat na http://localhost: 3000) nebo publikování tooAzure.</span><span class="sxs-lookup"><span data-stu-id="166bc-161">Either run hello application locally (hello API is served on http://localhost:3000) or publish tooAzure.</span></span>

### <span data-ttu-id="166bc-162"><a name="create-node-backend-portal"></a>Postupy: vytvoření back-end Node.js, pomocí hello portálu Azure</span><span class="sxs-lookup"><span data-stu-id="166bc-162"><a name="create-node-backend-portal"></a>How to: Create a Node.js backend using hello Azure portal</span></span>
<span data-ttu-id="166bc-163">Můžete vytvořit právo back-end mobilní aplikace hello [portál Azure].</span><span class="sxs-lookup"><span data-stu-id="166bc-163">You can create a Mobile App backend right in hello [Azure portal].</span></span> <span data-ttu-id="166bc-164">Můžete buď provést hello následující kroky nebo vytvořte klienta a serveru společně následující hello [vytvoření mobilní aplikace](app-service-mobile-ios-get-started.md) kurzu.</span><span class="sxs-lookup"><span data-stu-id="166bc-164">You can either follow hello following steps or create a client and server together by following hello [Create a mobile app](app-service-mobile-ios-get-started.md) tutorial.</span></span> <span data-ttu-id="166bc-165">kurz Hello obsahuje zjednodušenou verzi tyto pokyny a je nejvhodnější pro ověření projekty na konceptu.</span><span class="sxs-lookup"><span data-stu-id="166bc-165">hello tutorial contains a simplified version of these instructions and is best for proof of concept projects.</span></span>

[!INCLUDE [app-service-mobile-dotnet-backend-create-new-service-classic](../../includes/app-service-mobile-dotnet-backend-create-new-service-classic.md)]

<span data-ttu-id="166bc-166">Zpět v hello *Začínáme* okno, v části **vytvořit tabulku rozhraní API**, zvolte **Node.js** jako vaše **back-end jazyk**.</span><span class="sxs-lookup"><span data-stu-id="166bc-166">Back in hello *Get started* blade, under **Create a table API**, choose **Node.js** as your **Backend language**.</span></span>
<span data-ttu-id="166bc-167">Zaškrtněte políčko hello pro "**beru na vědomí, že tato akce přepíše všechny lokality obsahu.**", pak klikněte na tlačítko **vytvořit tabulku TodoItem**.</span><span class="sxs-lookup"><span data-stu-id="166bc-167">Check hello box for "**I acknowledge that this will overwrite all site contents.**", then click **Create TodoItem table**.</span></span>

### <span data-ttu-id="166bc-168"><a name="download-quickstart"></a>Postupy: stažení hello Node.js back-end rychlé spuštění kódu projektu pomocí Git</span><span class="sxs-lookup"><span data-stu-id="166bc-168"><a name="download-quickstart"></a>How to: Download hello Node.js backend quickstart code project using Git</span></span>
<span data-ttu-id="166bc-169">Při vytváření back-end mobilní aplikace Node.js pomocí portálu hello **úvodní** okně projekt Node.js se vytvoří a nasazené tooyour lokality.</span><span class="sxs-lookup"><span data-stu-id="166bc-169">When you create a Node.js Mobile App backend by using hello portal **Quick start** blade, a Node.js project is created for you and deployed tooyour site.</span></span> <span data-ttu-id="166bc-170">Můžete přidat tabulky a rozhraní API a upravit soubory kódu pro back-end Node.js hello hello portálu.</span><span class="sxs-lookup"><span data-stu-id="166bc-170">You can add tables and APIs and edit code files for hello Node.js backend in hello portal.</span></span> <span data-ttu-id="166bc-171">Můžete také použít různé nasazení nástroje toodownload hello back-end projektu, takže můžete přidat nebo upravit tabulek a rozhraní API a pak znovu publikovat hello projektu.</span><span class="sxs-lookup"><span data-stu-id="166bc-171">You can also use various deployment tools toodownload hello backend project so that you can add or modify tables and APIs, then republish hello project.</span></span> <span data-ttu-id="166bc-172">Další informace najdete v tématu [Příručka pro nasazení Azure App Service].</span><span class="sxs-lookup"><span data-stu-id="166bc-172">For more information, see the [Azure App Service Deployment Guide].</span></span> <span data-ttu-id="166bc-173">Hello následující postup používá kód Git úložiště toodownload hello rychlé spuštění projektu.</span><span class="sxs-lookup"><span data-stu-id="166bc-173">hello following procedure uses a Git repository toodownload hello quickstart project code.</span></span>

1. <span data-ttu-id="166bc-174">Pokud jste tak již neučinili, nainstalujte Git.</span><span class="sxs-lookup"><span data-stu-id="166bc-174">Install Git, if you haven't already done so.</span></span> <span data-ttu-id="166bc-175">Hello kroky požadované tooinstall Git lišit podle operačních systémů.</span><span class="sxs-lookup"><span data-stu-id="166bc-175">hello steps required tooinstall Git vary between operating systems.</span></span> <span data-ttu-id="166bc-176">V tématu [instalace Git](http://git-scm.com/book/en/Getting-Started-Installing-Git) distribuce specifické pro operační systém a instalaci.</span><span class="sxs-lookup"><span data-stu-id="166bc-176">See [Installing Git](http://git-scm.com/book/en/Getting-Started-Installing-Git) for operating system-specific distributions and installation guidance.</span></span>
2. <span data-ttu-id="166bc-177">Postupujte podle kroků hello v [povolit hello úložiště aplikace služby App Service](../app-service-web/app-service-deploy-local-git.md#Step3) tooenable hello úložiště Git pro vaši lokalitu back-end, provádění poznamenejte si hello nasazení uživatelské jméno a heslo.</span><span class="sxs-lookup"><span data-stu-id="166bc-177">Follow hello steps in [Enable hello App Service app repository](../app-service-web/app-service-deploy-local-git.md#Step3) tooenable hello Git repository for your backend site, making a note of hello deployment username and password.</span></span>
3. <span data-ttu-id="166bc-178">V okně hello back-endu mobilní aplikace, poznamenejte si hello **adresy URL pro klon Git** nastavení.</span><span class="sxs-lookup"><span data-stu-id="166bc-178">In hello blade for your Mobile App backend, make a note of hello **Git clone URL** setting.</span></span>
4. <span data-ttu-id="166bc-179">Spuštění hello `git clone` příkaz pomocí hello Git adresa URL klonování, zadat heslo, pokud jsou povinné, jako v následujícím příkladu:</span><span class="sxs-lookup"><span data-stu-id="166bc-179">Execute hello `git clone` command using hello Git clone URL, entering your password when required, as in the following example:</span></span>

        $ git clone https://username@todolist.scm.azurewebsites.net:443/todolist.git
5. <span data-ttu-id="166bc-180">Byly staženy procházet toolocal adresář, který v předchozím příkladu hello je /todolist a Všimněte si, že soubory projektu.</span><span class="sxs-lookup"><span data-stu-id="166bc-180">Browse toolocal directory, which in hello preceding example is /todolist, and notice that project files have been downloaded.</span></span> <span data-ttu-id="166bc-181">Vyhledejte hello `todoitem.json` souboru v hello `/tables` adresáře.</span><span class="sxs-lookup"><span data-stu-id="166bc-181">Locate hello `todoitem.json` file in hello `/tables` directory.</span></span>  <span data-ttu-id="166bc-182">Tento soubor definuje oprávnění v tabulce.</span><span class="sxs-lookup"><span data-stu-id="166bc-182">This file defines permissions on the table.</span></span>  <span data-ttu-id="166bc-183">Také umožňuje vyhledat hello `todoitem.js` souboru v hello stejný adresář, který definuje, že operace CRUD skriptů pro tabulku hello.</span><span class="sxs-lookup"><span data-stu-id="166bc-183">Also find hello `todoitem.js` file in hello same directory, which defines that CRUD operation scripts for hello table.</span></span>
6. <span data-ttu-id="166bc-184">Po provedení změn tooproject soubory, spusťte následující příkazy tooadd, potvrzení a pak nahrajte webu toohello změny hello:</span><span class="sxs-lookup"><span data-stu-id="166bc-184">After you have made changes tooproject files, execute hello following commands tooadd, commit, then upload the changes toohello site:</span></span>

        $ git commit -m "updated hello table script"
        $ git push origin master

    <span data-ttu-id="166bc-185">Když přidáte nový projekt toohello soubory, musíte nejprve tooexecute hello `git add .` příkaz.</span><span class="sxs-lookup"><span data-stu-id="166bc-185">When you add new files toohello project, you first need tooexecute hello `git add .` command.</span></span>

<span data-ttu-id="166bc-186">Hello lokality znovu publikován pokaždé, když je novou sadu potvrzení nabídnutých toohello lokality.</span><span class="sxs-lookup"><span data-stu-id="166bc-186">hello site is republished every time a new set of commits is pushed toohello site.</span></span>

### <span data-ttu-id="166bc-187"><a name="howto-publish-to-azure"></a>Postupy: publikování vašeho tooAzure back-end Node.js</span><span class="sxs-lookup"><span data-stu-id="166bc-187"><a name="howto-publish-to-azure"></a>How to: Publish your Node.js backend tooAzure</span></span>
<span data-ttu-id="166bc-188">Microsoft Azure poskytuje mnoho mechanismy pro publikování váš back-end Azure App Service Mobile aplikace Node.js hello služby Azure.</span><span class="sxs-lookup"><span data-stu-id="166bc-188">Microsoft Azure provides many mechanisms for publishing your Azure App Service Mobile Apps Node.js backend to hello Azure service.</span></span>  <span data-ttu-id="166bc-189">Jedná se o použití nástroje pro nasazení, které jsou integrované do sady Visual Studio, nástroje příkazového řádku a průběžné nasazování pro různé zdrojového kódu.</span><span class="sxs-lookup"><span data-stu-id="166bc-189">These include utilizing deployment tools integrated into Visual Studio, command-line tools, and continuous deployment options based on source control.</span></span>  <span data-ttu-id="166bc-190">Další informace v tomto tématu najdete v tématu [Příručka pro nasazení Azure App Service].</span><span class="sxs-lookup"><span data-stu-id="166bc-190">For more information on this topic, see the [Azure App Service Deployment Guide].</span></span>

<span data-ttu-id="166bc-191">Aplikační služba Azure má konkrétní Rady, jak aplikaci Node.js, která si měli projít před nasazením:</span><span class="sxs-lookup"><span data-stu-id="166bc-191">Azure App Service has specific advice for Node.js application that you should review before deploying:</span></span>

* <span data-ttu-id="166bc-192">Jak příliš[zadejte hello uzlu verze]</span><span class="sxs-lookup"><span data-stu-id="166bc-192">How too[specify hello Node Version]</span></span>
* <span data-ttu-id="166bc-193">Jak příliš[použijte moduly uzlu]</span><span class="sxs-lookup"><span data-stu-id="166bc-193">How too[use Node modules]</span></span>

### <span data-ttu-id="166bc-194"><a name="howto-enable-homepage"></a>Postupy: povolení domovskou stránku pro aplikaci</span><span class="sxs-lookup"><span data-stu-id="166bc-194"><a name="howto-enable-homepage"></a>How to: Enable a Home Page for your application</span></span>
<span data-ttu-id="166bc-195">Mnoho aplikací jsou kombinací webových a mobilních aplikací a hello ExpressJS framework vám umožní toocombine dva omezující vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="166bc-195">Many applications are a combination of web and mobile apps and hello ExpressJS framework allows you toocombine the two facets.</span></span>  <span data-ttu-id="166bc-196">V některých případech ale můžete tooonly implementace mobilní rozhraní.</span><span class="sxs-lookup"><span data-stu-id="166bc-196">Sometimes, however, you may wish tooonly implement a mobile interface.</span></span>  <span data-ttu-id="166bc-197">Je užitečné tooprovide cílová stránka tooensure hello aplikační služby je spuštěná.</span><span class="sxs-lookup"><span data-stu-id="166bc-197">It is useful tooprovide a landing page tooensure hello app service is up and running.</span></span>  <span data-ttu-id="166bc-198">Můžete zadat vlastní domovské stránky, nebo povolit dočasné domovskou stránku.</span><span class="sxs-lookup"><span data-stu-id="166bc-198">You can either provide your own home page or enable a temporary home page.</span></span>  <span data-ttu-id="166bc-199">tooenable dočasné domovskou stránku, použijte následující tooinstantiate Azure Mobile Apps hello:</span><span class="sxs-lookup"><span data-stu-id="166bc-199">tooenable a temporary home page, use hello following tooinstantiate Azure Mobile Apps:</span></span>

    var mobile = azureMobileApps({ homePage: true });

<span data-ttu-id="166bc-200">Pokud chcete pouze tuto možnost k dispozici při vývoji místně, můžete přidat tato nastavení tooyour `azureMobile.js` souboru.</span><span class="sxs-lookup"><span data-stu-id="166bc-200">If you only want this option available when developing locally, you can add this setting tooyour `azureMobile.js` file.</span></span>

## <span data-ttu-id="166bc-201"><a name="TableOperations"></a>Operace s tabulkou</span><span class="sxs-lookup"><span data-stu-id="166bc-201"><a name="TableOperations"></a>Table operations</span></span>
<span data-ttu-id="166bc-202">Hello aplikace azure mobile Node.js Server SDK poskytuje mechanismy tooexpose data tabulek uložených v databázi SQL Azure jako WebAPI.</span><span class="sxs-lookup"><span data-stu-id="166bc-202">hello azure-mobile-apps Node.js Server SDK provides mechanisms tooexpose data tables stored in Azure SQL Database as a WebAPI.</span></span>  <span data-ttu-id="166bc-203">Jsou k dispozici pět operace.</span><span class="sxs-lookup"><span data-stu-id="166bc-203">Five operations are provided.</span></span>

| <span data-ttu-id="166bc-204">Operace</span><span class="sxs-lookup"><span data-stu-id="166bc-204">Operation</span></span> | <span data-ttu-id="166bc-205">Popis</span><span class="sxs-lookup"><span data-stu-id="166bc-205">Description</span></span> |
| --- | --- |
| <span data-ttu-id="166bc-206">GET /tables/*tablename*</span><span class="sxs-lookup"><span data-stu-id="166bc-206">GET /tables/*tablename*</span></span> |<span data-ttu-id="166bc-207">Získat všechny záznamy v tabulce hello</span><span class="sxs-lookup"><span data-stu-id="166bc-207">Get all records in hello table</span></span> |
| <span data-ttu-id="166bc-208">GET /tables/*tablename*/:id</span><span class="sxs-lookup"><span data-stu-id="166bc-208">GET /tables/*tablename*/:id</span></span> |<span data-ttu-id="166bc-209">Získejte konkrétní záznam v tabulce hello</span><span class="sxs-lookup"><span data-stu-id="166bc-209">Get a specific record in hello table</span></span> |
| <span data-ttu-id="166bc-210">POST /tables/*tablename*</span><span class="sxs-lookup"><span data-stu-id="166bc-210">POST /tables/*tablename*</span></span> |<span data-ttu-id="166bc-211">Vytvořit záznam v tabulce hello</span><span class="sxs-lookup"><span data-stu-id="166bc-211">Create a record in hello table</span></span> |
| <span data-ttu-id="166bc-212">Oprava /tables/*tablename*/:id</span><span class="sxs-lookup"><span data-stu-id="166bc-212">PATCH /tables/*tablename*/:id</span></span> |<span data-ttu-id="166bc-213">Aktualizace záznamu v tabulce hello</span><span class="sxs-lookup"><span data-stu-id="166bc-213">Update a record in hello table</span></span> |
| <span data-ttu-id="166bc-214">ODSTRANĚNÍ /tables/*tablename*/:id</span><span class="sxs-lookup"><span data-stu-id="166bc-214">DELETE /tables/*tablename*/:id</span></span> |<span data-ttu-id="166bc-215">Odstranit záznam v tabulce hello</span><span class="sxs-lookup"><span data-stu-id="166bc-215">Delete a record in hello table</span></span> |

<span data-ttu-id="166bc-216">Podporuje tato WebAPI [OData] a rozšiřuje hello tabulky schématu toosupport [synchronizaci dat offline].</span><span class="sxs-lookup"><span data-stu-id="166bc-216">This WebAPI supports [OData] and extends hello table schema toosupport [offline data sync].</span></span>

### <span data-ttu-id="166bc-217"><a name="howto-dynamicschema"></a>Postupy: definování tabulky pomocí dynamické schématu</span><span class="sxs-lookup"><span data-stu-id="166bc-217"><a name="howto-dynamicschema"></a>How to: Define tables using a dynamic schema</span></span>
<span data-ttu-id="166bc-218">Před použitím tabulky, musí být definovaný.</span><span class="sxs-lookup"><span data-stu-id="166bc-218">Before a table can be used, it must be defined.</span></span>  <span data-ttu-id="166bc-219">Tabulky lze definovat pomocí statické schématu (kde hello vývojáře definuje hello sloupců ve schématu hello) nebo dynamicky (kde hello SDK řídí hello schéma založené na příchozí požadavky).</span><span class="sxs-lookup"><span data-stu-id="166bc-219">Tables can be defined with a static schema (where hello developer defines hello columns within hello schema) or dynamically (where hello SDK controls hello schema based on incoming requests).</span></span> <span data-ttu-id="166bc-220">Kromě toho můžete řídit hello vývojáře konkrétních aspektů hello WebAPI přidáním definice toohello kódu Javascript.</span><span class="sxs-lookup"><span data-stu-id="166bc-220">In addition, hello developer can control specific aspects of hello WebAPI by adding Javascript code toohello definition.</span></span>

<span data-ttu-id="166bc-221">Jako osvědčený postup musí definovat každá tabulka v souboru jazyka Javascript v adresáři hello tabulky, potom použijte tables.import() metoda tooimport hello tabulky.</span><span class="sxs-lookup"><span data-stu-id="166bc-221">As a best practice, you should define each table in a Javascript file in hello tables directory, then use the tables.import() method tooimport hello tables.</span></span>  <span data-ttu-id="166bc-222">Rozšíření hello basic – aplikace, souboru app.js hello se upraví:</span><span class="sxs-lookup"><span data-stu-id="166bc-222">Extending hello basic-app, hello app.js file would be adjusted:</span></span>

    var express = require('express'),
        azureMobileApps = require('azure-mobile-apps');

    var app = express(),
        mobile = azureMobileApps();

    // Define hello database schema that is exposed
    mobile.tables.import('./tables');

    // Provide initialization of any tables that are statically defined
    mobile.tables.initialize().then(function () {
        // Add hello mobile API so it is accessible as a Web API
        app.use(mobile);

        // Start listening on HTTP
        app.listen(process.env.PORT || 3000);
    });

<span data-ttu-id="166bc-223">Definovat hello tabulky ve. / tables/TodoItem.js:</span><span class="sxs-lookup"><span data-stu-id="166bc-223">Define hello table in ./tables/TodoItem.js:</span></span>

    var azureMobileApps = require('azure-mobile-apps');

    var table = azureMobileApps.table();

    // Additional configuration for hello table goes here

    module.exports = table;

<span data-ttu-id="166bc-224">Tabulky ve výchozím nastavení používá dynamické schématu.</span><span class="sxs-lookup"><span data-stu-id="166bc-224">Tables use dynamic schema by default.</span></span>  <span data-ttu-id="166bc-225">tooturn vypnout dynamické schématu globálně, nastavte hello nastavení aplikace **MS_DynamicSchema** toofalse v rámci hello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="166bc-225">tooturn off dynamic schema globally, set hello App Setting **MS_DynamicSchema** toofalse within hello Azure portal.</span></span>

<span data-ttu-id="166bc-226">Úplný příklad můžete najít v hello [todo ukázce na Githubu].</span><span class="sxs-lookup"><span data-stu-id="166bc-226">You can find a complete example in hello [todo sample on GitHub].</span></span>

### <span data-ttu-id="166bc-227"><a name="howto-staticschema"></a>Postupy: definování tabulky pomocí statické schématu</span><span class="sxs-lookup"><span data-stu-id="166bc-227"><a name="howto-staticschema"></a>How to: Define tables using a static schema</span></span>
<span data-ttu-id="166bc-228">Můžete definovat explicitně hello sloupce tooexpose prostřednictvím hello WebAPI.</span><span class="sxs-lookup"><span data-stu-id="166bc-228">You can explicitly define hello columns tooexpose via hello WebAPI.</span></span>  <span data-ttu-id="166bc-229">Hello aplikace azure mobile Node.js SDK automaticky přidá žádné další sloupce, které jsou potřebné pro seznam toohello synchronizaci dat offline, který zadáte.</span><span class="sxs-lookup"><span data-stu-id="166bc-229">hello azure-mobile-apps Node.js SDK automatically adds any additional columns required for offline data sync toohello list that you provide.</span></span>  <span data-ttu-id="166bc-230">Například klientské aplikace rychlý start vyžadovat tabulku s dva sloupce: text (řetězec) a dokončete (boolean).</span><span class="sxs-lookup"><span data-stu-id="166bc-230">For example, the QuickStart client applications require a table with two columns: text (a string) and complete (a boolean).</span></span>  
<span data-ttu-id="166bc-231">Tabulka Hello je možné definovat v hello tabulky definice soubor JavaScript (umístěný v adresáři tabulky hello) následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="166bc-231">hello table can be defined in hello table definition JavaScript file (located in hello tables directory) as follows:</span></span>

    var azureMobileApps = require('azure-mobile-apps');

    var table = azureMobileApps.table();

    // Define hello columns within hello table
    table.columns = {
        "text": "string",
        "complete": "boolean"
    };

    // Turn off dynamic schema
    table.dynamicSchema = false;

    module.exports = table;

<span data-ttu-id="166bc-232">Pokud definujete tabulky staticky, pak musíte také zavolat hello tables.initialize() metoda toocreate hello schéma databáze při spuštění.</span><span class="sxs-lookup"><span data-stu-id="166bc-232">If you define tables statically, then you must also call hello tables.initialize() method toocreate hello database schema on startup.</span></span>  <span data-ttu-id="166bc-233">vrátí metoda tables.initialize() Hello [Promise] tak, aby hello webová služba neobsluhuje žádosti před hello databáze během inicializace.</span><span class="sxs-lookup"><span data-stu-id="166bc-233">hello tables.initialize() method returns a [Promise] so that hello web service does not serve requests before hello database being initialized.</span></span>

### <span data-ttu-id="166bc-234"><a name="howto-sqlexpress-setup"></a>Postupy: použití SQL Express jako úložiště dat vývoj v místním počítači</span><span class="sxs-lookup"><span data-stu-id="166bc-234"><a name="howto-sqlexpress-setup"></a>How to: Use SQL Express as a development data store on your local machine</span></span>
<span data-ttu-id="166bc-235">Hello Azure Mobile Apps hello AzureMobile aplikace uzlu SDK poskytuje tři možnosti pro obsluhující data předinstalované hello: Sada SDK poskytuje tři možnosti obsluhující data předinstalované hello:</span><span class="sxs-lookup"><span data-stu-id="166bc-235">hello Azure Mobile Apps hello AzureMobile Apps Node SDK provides three options for serving data out of hello box: SDK provides three options for serving data out of hello box:</span></span>

* <span data-ttu-id="166bc-236">Použití hello **paměti** úložiště příklad tooprovide dočasnou ovladačů</span><span class="sxs-lookup"><span data-stu-id="166bc-236">Use hello **memory** driver tooprovide a non-persistent example store</span></span>
* <span data-ttu-id="166bc-237">Použití hello **mssql** tooprovide ovladačů úložiště dat SQL Express pro vývoj</span><span class="sxs-lookup"><span data-stu-id="166bc-237">Use hello **mssql** driver tooprovide a SQL Express data store for development</span></span>
* <span data-ttu-id="166bc-238">Použití hello **mssql** tooprovide ovladačů úložiště dat služby Azure SQL Database pro produkční prostředí</span><span class="sxs-lookup"><span data-stu-id="166bc-238">Use hello **mssql** driver tooprovide an Azure SQL Database data store for production</span></span>

<span data-ttu-id="166bc-239">Hello Azure Mobile Apps Node.js SDK používá hello [mssql Node.js balíček] tooestablish a použití tooboth připojení SQL Express a databáze SQL.</span><span class="sxs-lookup"><span data-stu-id="166bc-239">hello Azure Mobile Apps Node.js SDK uses hello [mssql Node.js package] tooestablish and use a connection tooboth SQL Express and SQL Database.</span></span>  <span data-ttu-id="166bc-240">Tento balíček vyžaduje, abyste povolili připojení TCP ve vaší instanci SQL Express.</span><span class="sxs-lookup"><span data-stu-id="166bc-240">This package requires that you enable TCP connections on your SQL Express instance.</span></span>

> [!TIP]
> <span data-ttu-id="166bc-241">Ovladač paměti Hello neposkytuje kompletní sadu zařízení pro testování.</span><span class="sxs-lookup"><span data-stu-id="166bc-241">hello memory driver does not provide a complete set of facilities for testing.</span></span>  <span data-ttu-id="166bc-242">Pokud chcete tootest váš back-end místně, doporučujeme hello použití úložiště dat SQL Express a hello mssql ovladačů.</span><span class="sxs-lookup"><span data-stu-id="166bc-242">If you wish tootest your backend locally, we recommend hello use of a SQL Express data store and hello mssql driver.</span></span>
>
>

1. <span data-ttu-id="166bc-243">Stáhněte a nainstalujte [Microsoft SQL Server 2014 Express].</span><span class="sxs-lookup"><span data-stu-id="166bc-243">Download and install [Microsoft SQL Server 2014 Express].</span></span>  <span data-ttu-id="166bc-244">Zkontrolujte, zda že nainstalovat SQL Server 2014 Express hello s edicí nástroje.</span><span class="sxs-lookup"><span data-stu-id="166bc-244">Ensure you install hello SQL Server 2014 Express with Tools edition.</span></span>  <span data-ttu-id="166bc-245">Pokud požadujete explicitně podpora 64bitových technologií, hello 32bitová verze spotřebovává méně paměti při spuštění.</span><span class="sxs-lookup"><span data-stu-id="166bc-245">Unless you explicitly require 64-bit support, hello 32-bit version consumes less memory when running.</span></span>
2. <span data-ttu-id="166bc-246">Spusťte Správce konfigurace systému SQL Server 2014 hello.</span><span class="sxs-lookup"><span data-stu-id="166bc-246">Run hello SQL Server 2014 Configuration Manager.</span></span>

   1. <span data-ttu-id="166bc-247">Rozbalte hello **konfigurace sítě serveru SQL Server** uzlu v nabídce levé stromu hello.</span><span class="sxs-lookup"><span data-stu-id="166bc-247">Expand hello **SQL Server Network Configuration** node in hello left-hand tree menu.</span></span>
   2. <span data-ttu-id="166bc-248">Klikněte na tlačítko **protokoly pro funkci SQLEXPRESS**.</span><span class="sxs-lookup"><span data-stu-id="166bc-248">Click **Protocols for SQLEXPRESS**.</span></span>
   3. <span data-ttu-id="166bc-249">Klikněte pravým tlačítkem na **TCP/IP** a vyberte **povolit**.</span><span class="sxs-lookup"><span data-stu-id="166bc-249">Right-click **TCP/IP** and select **Enable**.</span></span>  <span data-ttu-id="166bc-250">Klikněte na tlačítko **OK** v místním dialogovém okně hello.</span><span class="sxs-lookup"><span data-stu-id="166bc-250">Click **OK** in hello pop-up dialog.</span></span>
   4. <span data-ttu-id="166bc-251">Klikněte pravým tlačítkem na **TCP/IP** a vyberte **vlastnosti**.</span><span class="sxs-lookup"><span data-stu-id="166bc-251">Right-click **TCP/IP** and select **Properties**.</span></span>
   5. <span data-ttu-id="166bc-252">Klikněte na tlačítko hello **IP adresy** kartě.</span><span class="sxs-lookup"><span data-stu-id="166bc-252">Click hello **IP Addresses** tab.</span></span>
   6. <span data-ttu-id="166bc-253">Najde hello **IPAll** uzlu.</span><span class="sxs-lookup"><span data-stu-id="166bc-253">Find hello **IPAll** node.</span></span>  <span data-ttu-id="166bc-254">V hello **TCP Port** zadejte **1433**.</span><span class="sxs-lookup"><span data-stu-id="166bc-254">In hello **TCP Port** field, enter **1433**.</span></span>

          ![Configure SQL Express for TCP/IP][3]
   7. <span data-ttu-id="166bc-255">Klikněte na **OK**.</span><span class="sxs-lookup"><span data-stu-id="166bc-255">Click **OK**.</span></span>  <span data-ttu-id="166bc-256">Klikněte na tlačítko **OK** v místním dialogovém okně hello.</span><span class="sxs-lookup"><span data-stu-id="166bc-256">Click **OK** in hello pop-up dialog.</span></span>
   8. <span data-ttu-id="166bc-257">Klikněte na tlačítko **služby SQL Server** v nabídce levé stromu hello.</span><span class="sxs-lookup"><span data-stu-id="166bc-257">Click **SQL Server Services** in hello left-hand tree menu.</span></span>
   9. <span data-ttu-id="166bc-258">Klikněte pravým tlačítkem na **SQL Server (SQLEXPRESS)** a vyberte **restartovat**</span><span class="sxs-lookup"><span data-stu-id="166bc-258">Right-click **SQL Server (SQLEXPRESS)** and select **Restart**</span></span>
   10. <span data-ttu-id="166bc-259">Zavřete hello Správce konfigurace systému SQL Server 2014.</span><span class="sxs-lookup"><span data-stu-id="166bc-259">Close hello SQL Server 2014 Configuration Manager.</span></span>
3. <span data-ttu-id="166bc-260">Spustit hello SQL Server 2014 Management Studio a připojte tooyour místní instance SQL Express</span><span class="sxs-lookup"><span data-stu-id="166bc-260">Run hello SQL Server 2014 Management Studio and connect tooyour local SQL Express instance</span></span>

   1. <span data-ttu-id="166bc-261">Klikněte pravým tlačítkem na instanci v hello Průzkumník objektů a vyberte **vlastnosti**</span><span class="sxs-lookup"><span data-stu-id="166bc-261">Right-click your instance in hello Object Explorer and select **Properties**</span></span>
   2. <span data-ttu-id="166bc-262">Vyberte hello **zabezpečení** stránky.</span><span class="sxs-lookup"><span data-stu-id="166bc-262">Select hello **Security** page.</span></span>
   3. <span data-ttu-id="166bc-263">Ujistěte se, hello **režimu SQL Server a ověřování systému Windows** je vybrána</span><span class="sxs-lookup"><span data-stu-id="166bc-263">Ensure hello **SQL Server and Windows Authentication mode** is selected</span></span>
   4. <span data-ttu-id="166bc-264">Klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="166bc-264">Click **OK**</span></span>

          ![Configure SQL Express Authentication][4]
   5. <span data-ttu-id="166bc-265">Rozbalte položku **zabezpečení** > **přihlášení** v hello Průzkumník objektů</span><span class="sxs-lookup"><span data-stu-id="166bc-265">Expand **Security** > **Logins** in hello Object Explorer</span></span>
   6. <span data-ttu-id="166bc-266">Klikněte pravým tlačítkem na **přihlášení** a vyberte **nové přihlašovací údaje...**</span><span class="sxs-lookup"><span data-stu-id="166bc-266">Right-click **Logins** and select **New Login...**</span></span>
   7. <span data-ttu-id="166bc-267">Zadejte přihlašovací jméno.</span><span class="sxs-lookup"><span data-stu-id="166bc-267">Enter a Login name.</span></span>  <span data-ttu-id="166bc-268">Vyberte **Ověřování SQL Serveru**.</span><span class="sxs-lookup"><span data-stu-id="166bc-268">Select **SQL Server authentication**.</span></span>  <span data-ttu-id="166bc-269">Zadejte heslo a potom zadejte hello stejné heslo v **potvrzení hesla**.</span><span class="sxs-lookup"><span data-stu-id="166bc-269">Enter a Password, then enter hello same password in **Confirm password**.</span></span>  <span data-ttu-id="166bc-270">Hello heslo musí splňovat požadavky na složitost systému Windows.</span><span class="sxs-lookup"><span data-stu-id="166bc-270">hello password must meet Windows complexity requirements.</span></span>
   8. <span data-ttu-id="166bc-271">Klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="166bc-271">Click **OK**</span></span>

          ![Add a new user tooSQL Express][5]
   9. <span data-ttu-id="166bc-272">Klikněte pravým tlačítkem na nové přihlášení a vyberte **vlastnosti**</span><span class="sxs-lookup"><span data-stu-id="166bc-272">Right-click your new login and select **Properties**</span></span>
   10. <span data-ttu-id="166bc-273">Vyberte hello **role serveru** stránky</span><span class="sxs-lookup"><span data-stu-id="166bc-273">Select hello **Server Roles** page</span></span>
   11. <span data-ttu-id="166bc-274">Zkontrolujte další toohello pole hello **dbcreator** role serveru</span><span class="sxs-lookup"><span data-stu-id="166bc-274">Check hello box next toohello **dbcreator** server role</span></span>
   12. <span data-ttu-id="166bc-275">Klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="166bc-275">Click **OK**</span></span>
   13. <span data-ttu-id="166bc-276">Zavřete hello SQL Server 2015 Management Studio</span><span class="sxs-lookup"><span data-stu-id="166bc-276">Close hello SQL Server 2015 Management Studio</span></span>

<span data-ttu-id="166bc-277">Zajistěte, můžete záznam hello uživatelské jméno a heslo, které jste vybrali.</span><span class="sxs-lookup"><span data-stu-id="166bc-277">Ensure you record hello username and password you selected.</span></span>  <span data-ttu-id="166bc-278">V závislosti na požadavcích vaší konkrétní databáze může být nutné tooassign dalších serverových rolí nebo oprávnění.</span><span class="sxs-lookup"><span data-stu-id="166bc-278">You may need tooassign additional server roles or permissions depending on your specific database requirements.</span></span>

<span data-ttu-id="166bc-279">přečte Hello aplikace Node.js hello **SQLCONNSTR_MS_TableConnectionString** proměnnou prostředí pro hello připojovací řetězec pro tuto databázi.</span><span class="sxs-lookup"><span data-stu-id="166bc-279">hello Node.js application reads hello **SQLCONNSTR_MS_TableConnectionString** environment variable for hello connection string for this database.</span></span>  <span data-ttu-id="166bc-280">Tuto proměnnou lze nastavit v rámci vašeho prostředí.</span><span class="sxs-lookup"><span data-stu-id="166bc-280">You can set this variable within your environment.</span></span>  <span data-ttu-id="166bc-281">Můžete například použít PowerShell tooset této proměnné prostředí:</span><span class="sxs-lookup"><span data-stu-id="166bc-281">For example, you can use PowerShell tooset this environment variable:</span></span>

    $env:SQLCONNSTR_MS_TableConnectionString = "Server=127.0.0.1; Database=mytestdatabase; User Id=azuremobile; Password=T3stPa55word;"

<span data-ttu-id="166bc-282">Přístup k databázi hello přes připojení TCP/IP a zadejte uživatelské jméno a heslo pro připojení hello.</span><span class="sxs-lookup"><span data-stu-id="166bc-282">Access hello database through a TCP/IP connection and provide a username and password for hello connection.</span></span>

### <span data-ttu-id="166bc-283"><a name="howto-config-localdev"></a>Postupy: nakonfigurujete svůj projekt pro místní vývoj</span><span class="sxs-lookup"><span data-stu-id="166bc-283"><a name="howto-config-localdev"></a>How to: Configure your project for local development</span></span>
<span data-ttu-id="166bc-284">Azure Mobile Apps přečte soubor JavaScript s názvem *azureMobile.js* z místního systému souborů hello.</span><span class="sxs-lookup"><span data-stu-id="166bc-284">Azure Mobile Apps reads a JavaScript file called *azureMobile.js* from hello local filesystem.</span></span>  <span data-ttu-id="166bc-285">Použít tento soubor tooconfigure hello Azure Mobile Apps SDK v provozním prostředí není – použijte nastavení aplikace v rámci hello [portál Azure] místo.</span><span class="sxs-lookup"><span data-stu-id="166bc-285">Do not use this file tooconfigure hello Azure Mobile Apps SDK in production - use App Settings within hello [Azure portal] instead.</span></span>  <span data-ttu-id="166bc-286">Hello *azureMobile.js* soubor by měl exportovat objekt konfigurace.</span><span class="sxs-lookup"><span data-stu-id="166bc-286">hello *azureMobile.js* file should export a configuration object.</span></span>  <span data-ttu-id="166bc-287">jsou Hello nejběžnější nastavení:</span><span class="sxs-lookup"><span data-stu-id="166bc-287">hello most common settings are:</span></span>

* <span data-ttu-id="166bc-288">Nastavení databáze</span><span class="sxs-lookup"><span data-stu-id="166bc-288">Database Settings</span></span>
* <span data-ttu-id="166bc-289">Nastavení protokolování diagnostiky</span><span class="sxs-lookup"><span data-stu-id="166bc-289">Diagnostic Logging Settings</span></span>
* <span data-ttu-id="166bc-290">Alternativní nastavení CORS</span><span class="sxs-lookup"><span data-stu-id="166bc-290">Alternate CORS Settings</span></span>

<span data-ttu-id="166bc-291">Příklad *azureMobile.js* soubor implementace hello předcházející způsobem nastavení databáze:</span><span class="sxs-lookup"><span data-stu-id="166bc-291">An example *azureMobile.js* file implementing hello preceding database settings follows:</span></span>

    module.exports = {
        cors: {
            origins: [ 'localhost' ]
        },
        data: {
            provider: 'mssql',
            server: '127.0.0.1',
            database: 'mytestdatabase',
            user: 'azuremobile',
            password: 'T3stPa55word'
        },
        logging: {
            level: 'verbose'
        }
    };

<span data-ttu-id="166bc-292">Doporučujeme, abyste přidali *azureMobile.js* tooyour *.gitignore* souboru (nebo jiné zdrojového kódu ignorovat souboru) tooprevent hesla z ukládají v cloudu hello.</span><span class="sxs-lookup"><span data-stu-id="166bc-292">We recommend that you add *azureMobile.js* tooyour *.gitignore* file (or other source code control ignore file) tooprevent passwords from being stored in hello cloud.</span></span>  <span data-ttu-id="166bc-293">Vždy konfigurujte nastavení produkční v nastavení aplikace v rámci hello [portál Azure].</span><span class="sxs-lookup"><span data-stu-id="166bc-293">Always configure production settings in App Settings within hello [Azure portal].</span></span>

### <span data-ttu-id="166bc-294"><a name="howto-appsettings"></a>Postupy: Konfigurace nastavení aplikace pro mobilní aplikace</span><span class="sxs-lookup"><span data-stu-id="166bc-294"><a name="howto-appsettings"></a>How: Configure App Settings for your Mobile App</span></span>
<span data-ttu-id="166bc-295">Většina nastavení v hello *azureMobile.js* soubor mít ekvivalentní nastavení aplikace v hello [portál Azure].</span><span class="sxs-lookup"><span data-stu-id="166bc-295">Most settings in hello *azureMobile.js* file have an equivalent App Setting in hello [Azure portal].</span></span>  <span data-ttu-id="166bc-296">Použití hello následující seznam tooconfigure vaší aplikace v nastavení aplikace:</span><span class="sxs-lookup"><span data-stu-id="166bc-296">Use hello following list tooconfigure your app in App Settings:</span></span>

| <span data-ttu-id="166bc-297">Nastavení aplikace</span><span class="sxs-lookup"><span data-stu-id="166bc-297">App Setting</span></span> | <span data-ttu-id="166bc-298">*azureMobile.js* nastavení</span><span class="sxs-lookup"><span data-stu-id="166bc-298">*azureMobile.js* Setting</span></span> | <span data-ttu-id="166bc-299">Popis</span><span class="sxs-lookup"><span data-stu-id="166bc-299">Description</span></span> | <span data-ttu-id="166bc-300">Platné hodnoty</span><span class="sxs-lookup"><span data-stu-id="166bc-300">Valid Values</span></span> |
|:--- |:--- |:--- |:--- |
| <span data-ttu-id="166bc-301">**MS_MobileAppName**</span><span class="sxs-lookup"><span data-stu-id="166bc-301">**MS_MobileAppName**</span></span> |<span data-ttu-id="166bc-302">jméno</span><span class="sxs-lookup"><span data-stu-id="166bc-302">name</span></span> |<span data-ttu-id="166bc-303">Hello název aplikace hello</span><span class="sxs-lookup"><span data-stu-id="166bc-303">hello name of hello app</span></span> |<span data-ttu-id="166bc-304">Řetězec</span><span class="sxs-lookup"><span data-stu-id="166bc-304">string</span></span> |
| <span data-ttu-id="166bc-305">**MS_MobileLoggingLevel**</span><span class="sxs-lookup"><span data-stu-id="166bc-305">**MS_MobileLoggingLevel**</span></span> |<span data-ttu-id="166bc-306">Logging.level</span><span class="sxs-lookup"><span data-stu-id="166bc-306">logging.level</span></span> |<span data-ttu-id="166bc-307">Minimální protokolu úroveň toolog zprávy</span><span class="sxs-lookup"><span data-stu-id="166bc-307">Minimum log level of messages toolog</span></span> |<span data-ttu-id="166bc-308">Chyba, upozornění, informace o podrobné nastavení, ladění, i</span><span class="sxs-lookup"><span data-stu-id="166bc-308">error, warning, info, verbose, debug, silly</span></span> |
| <span data-ttu-id="166bc-309">**MS_DebugMode**</span><span class="sxs-lookup"><span data-stu-id="166bc-309">**MS_DebugMode**</span></span> |<span data-ttu-id="166bc-310">Ladění</span><span class="sxs-lookup"><span data-stu-id="166bc-310">debug</span></span> |<span data-ttu-id="166bc-311">Povolit nebo zakázat režim ladění</span><span class="sxs-lookup"><span data-stu-id="166bc-311">Enable or Disable debug mode</span></span> |<span data-ttu-id="166bc-312">Hodnota TRUE, false</span><span class="sxs-lookup"><span data-stu-id="166bc-312">true, false</span></span> |
| <span data-ttu-id="166bc-313">**MS_TableSchema**</span><span class="sxs-lookup"><span data-stu-id="166bc-313">**MS_TableSchema**</span></span> |<span data-ttu-id="166bc-314">data.Schema</span><span class="sxs-lookup"><span data-stu-id="166bc-314">data.schema</span></span> |<span data-ttu-id="166bc-315">Výchozí název schématu pro tabulky SQL</span><span class="sxs-lookup"><span data-stu-id="166bc-315">Default schema name for SQL tables</span></span> |<span data-ttu-id="166bc-316">String (výchozí: dbo)</span><span class="sxs-lookup"><span data-stu-id="166bc-316">string (default: dbo)</span></span> |
| <span data-ttu-id="166bc-317">**MS_DynamicSchema**</span><span class="sxs-lookup"><span data-stu-id="166bc-317">**MS_DynamicSchema**</span></span> |<span data-ttu-id="166bc-318">data.dynamicSchema</span><span class="sxs-lookup"><span data-stu-id="166bc-318">data.dynamicSchema</span></span> |<span data-ttu-id="166bc-319">Povolit nebo zakázat režim ladění</span><span class="sxs-lookup"><span data-stu-id="166bc-319">Enable or Disable debug mode</span></span> |<span data-ttu-id="166bc-320">Hodnota TRUE, false</span><span class="sxs-lookup"><span data-stu-id="166bc-320">true, false</span></span> |
| <span data-ttu-id="166bc-321">**MS_DisableVersionHeader**</span><span class="sxs-lookup"><span data-stu-id="166bc-321">**MS_DisableVersionHeader**</span></span> |<span data-ttu-id="166bc-322">verze (sada tooundefined)</span><span class="sxs-lookup"><span data-stu-id="166bc-322">version (set tooundefined)</span></span> |<span data-ttu-id="166bc-323">Zakáže hello X-záhlaví ZUMO-Server-Version záhlaví</span><span class="sxs-lookup"><span data-stu-id="166bc-323">Disables hello X-ZUMO-Server-Version header</span></span> |<span data-ttu-id="166bc-324">Hodnota TRUE, false</span><span class="sxs-lookup"><span data-stu-id="166bc-324">true, false</span></span> |
| <span data-ttu-id="166bc-325">**MS_SkipVersionCheck**</span><span class="sxs-lookup"><span data-stu-id="166bc-325">**MS_SkipVersionCheck**</span></span> |<span data-ttu-id="166bc-326">skipversioncheck</span><span class="sxs-lookup"><span data-stu-id="166bc-326">skipversioncheck</span></span> |<span data-ttu-id="166bc-327">Zakáže Kontrola verze klientského rozhraní API hello</span><span class="sxs-lookup"><span data-stu-id="166bc-327">Disables hello client API version check</span></span> |<span data-ttu-id="166bc-328">Hodnota TRUE, false</span><span class="sxs-lookup"><span data-stu-id="166bc-328">true, false</span></span> |

<span data-ttu-id="166bc-329">tooset nastavení aplikace:</span><span class="sxs-lookup"><span data-stu-id="166bc-329">tooset an App Setting:</span></span>

1. <span data-ttu-id="166bc-330">Přihlaste se toohello [portál Azure].</span><span class="sxs-lookup"><span data-stu-id="166bc-330">Log in toohello [Azure portal].</span></span>
2. <span data-ttu-id="166bc-331">Vyberte **všechny prostředky** nebo **App Services** pak klikněte na název hello mobilní aplikace.</span><span class="sxs-lookup"><span data-stu-id="166bc-331">Select **All resources** or **App Services** then click hello name of your Mobile App.</span></span>
3. <span data-ttu-id="166bc-332">Otevře se okno nastavení Hello ve výchozím nastavení.</span><span class="sxs-lookup"><span data-stu-id="166bc-332">hello Settings blade opens by default.</span></span> <span data-ttu-id="166bc-333">Pokud není, klikněte na tlačítko **nastavení**.</span><span class="sxs-lookup"><span data-stu-id="166bc-333">If it doesn't, click **Settings**.</span></span>
4. <span data-ttu-id="166bc-334">Klikněte na tlačítko **nastavení aplikace** v nabídce Obecné hello.</span><span class="sxs-lookup"><span data-stu-id="166bc-334">Click **Application settings** in hello GENERAL menu.</span></span>
5. <span data-ttu-id="166bc-335">Posuňte se toohello část nastavení aplikace.</span><span class="sxs-lookup"><span data-stu-id="166bc-335">Scroll toohello App Settings section.</span></span>
6. <span data-ttu-id="166bc-336">Pokud vaše aplikace, nastavení už existuje, klikněte na tlačítko hello hodnota hello aplikace nastavení tooedit hello hodnoty.</span><span class="sxs-lookup"><span data-stu-id="166bc-336">If your app setting already exists, click hello value of hello app setting tooedit hello value.</span></span>
7. <span data-ttu-id="166bc-337">Pokud vaše aplikace nastavení neexistuje, zadejte do hello klíč a hello hodnota v poli hodnota hello hello nastavení aplikace.</span><span class="sxs-lookup"><span data-stu-id="166bc-337">If your app setting does not exist, enter hello App Setting in hello Key box and hello value in hello Value box.</span></span>
8. <span data-ttu-id="166bc-338">Po dokončení, klikněte na tlačítko **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="166bc-338">Once you are complete, click **Save**.</span></span>

<span data-ttu-id="166bc-339">Změna většinu nastavení aplikace vyžaduje restartování služby.</span><span class="sxs-lookup"><span data-stu-id="166bc-339">Changing most app settings requires a service restart.</span></span>

### <span data-ttu-id="166bc-340"><a name="howto-use-sqlazure"></a>Postupy: použití SQL Database jako produkční úložiště dat.</span><span class="sxs-lookup"><span data-stu-id="166bc-340"><a name="howto-use-sqlazure"></a>How to: Use SQL Database as your production data store</span></span>
<!--- ALTERNATE INCLUDE - we can't use ../includes/app-service-mobile-dotnet-backend-create-new-service.md - slightly different semantics -->

<span data-ttu-id="166bc-341">Používání Azure SQL Database jako úložiště dat je stejný jako mezi všechny typy aplikací Azure App Service.</span><span class="sxs-lookup"><span data-stu-id="166bc-341">Using Azure SQL Database as a data store is identical across all Azure App Service application types.</span></span> <span data-ttu-id="166bc-342">Pokud jste to ještě neudělali, postupujte podle těchto kroků toocreate back-end mobilní aplikace.</span><span class="sxs-lookup"><span data-stu-id="166bc-342">If you have not done so already, follow these steps toocreate a Mobile App backend.</span></span>

1. <span data-ttu-id="166bc-343">Přihlaste se toohello [portál Azure].</span><span class="sxs-lookup"><span data-stu-id="166bc-343">Log in toohello [Azure portal].</span></span>
2. <span data-ttu-id="166bc-344">V hello levém horním rohu okna hello, klikněte na tlačítko hello **+ nový** tlačítko > **Web + mobilní** > **mobilní aplikace**, pak zadejte název pro váš back-end mobilní aplikace.</span><span class="sxs-lookup"><span data-stu-id="166bc-344">In hello top left of hello window, click hello **+NEW** button > **Web + Mobile** > **Mobile App**, then provide a name for your Mobile App backend.</span></span>
3. <span data-ttu-id="166bc-345">V hello **skupiny prostředků** zadejte hello stejný název jako vaše aplikace.</span><span class="sxs-lookup"><span data-stu-id="166bc-345">In hello **Resource Group** box, enter hello same name as your app.</span></span>
4. <span data-ttu-id="166bc-346">plán aplikační služby výchozí Hello je vybrán.</span><span class="sxs-lookup"><span data-stu-id="166bc-346">hello Default App Service plan is selected.</span></span>  <span data-ttu-id="166bc-347">Pokud chcete toochange plán služby App Service, můžete provést kliknutím hello plán služby App Service > **+ vytvořit nový**.</span><span class="sxs-lookup"><span data-stu-id="166bc-347">If you wish toochange your App Service plan, you can do so by clicking hello App Service Plan > **+ Create New**.</span></span>  <span data-ttu-id="166bc-348">Zadejte název nového plánu služby App Service hello a vyberte požadované místo.</span><span class="sxs-lookup"><span data-stu-id="166bc-348">Provide a name of hello new App Service plan and select an appropriate location.</span></span>  <span data-ttu-id="166bc-349">Klikněte na tlačítko hello cenová úroveň a vyberte příslušné cenovou úroveň služby hello.</span><span class="sxs-lookup"><span data-stu-id="166bc-349">Click hello Pricing tier and select an appropriate pricing tier for hello service.</span></span> <span data-ttu-id="166bc-350">Vyberte **zobrazit všechny** tooview více ceny možnosti, jako například **volné** a **sdílené**.</span><span class="sxs-lookup"><span data-stu-id="166bc-350">Select **View all** tooview more pricing options, such as **Free** and **Shared**.</span></span>  <span data-ttu-id="166bc-351">Jakmile vyberete cenovou úroveň, klikněte na tlačítko hello **vyberte** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="166bc-351">Once you have selected the pricing tier, click hello **Select** button.</span></span>  <span data-ttu-id="166bc-352">Zpět v hello **plán služby App Service** okně klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="166bc-352">Back in hello **App Service plan** blade, click **OK**.</span></span>
5. <span data-ttu-id="166bc-353">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="166bc-353">Click **Create**.</span></span> <span data-ttu-id="166bc-354">Zřizování back-end mobilní aplikace může trvat několik minut.</span><span class="sxs-lookup"><span data-stu-id="166bc-354">Provisioning a Mobile App backend can take a couple of minutes.</span></span>  <span data-ttu-id="166bc-355">Po zřízení back-end mobilní aplikace hello hello portál otevře hello **nastavení** okno pro back-end mobilní aplikace hello.</span><span class="sxs-lookup"><span data-stu-id="166bc-355">Once hello Mobile App backend is provisioned, hello portal opens hello **Settings** blade for hello Mobile App backend.</span></span>

<span data-ttu-id="166bc-356">Po vytvoření back-end mobilní aplikace hello tooeither můžete připojit existující SQL databáze tooyour mobilní aplikace back-end nebo vytvořte novou databázi SQL.</span><span class="sxs-lookup"><span data-stu-id="166bc-356">Once hello Mobile App backend is created, you can choose tooeither connect an existing SQL database tooyour Mobile App backend or create a new SQL database.</span></span>  <span data-ttu-id="166bc-357">V této části vytvoříme databázi SQL.</span><span class="sxs-lookup"><span data-stu-id="166bc-357">In this section, we create a SQL database.</span></span>

> [!NOTE]
> <span data-ttu-id="166bc-358">Pokud už máte databázi v hello stejné umístění jako back-end mobilní aplikace hello, můžete místo toho zvolíte **použít existující databázi** a pak vyberte tuto databázi.</span><span class="sxs-lookup"><span data-stu-id="166bc-358">If you already have a database in hello same location as hello mobile app backend, you can instead choose **Use an existing database** and then select that database.</span></span> <span data-ttu-id="166bc-359">z důvodu vyšší latence se nedoporučuje Hello použití databáze v jiném umístění.</span><span class="sxs-lookup"><span data-stu-id="166bc-359">hello use of a database in a different location is not recommended because of higher latencies.</span></span>
>
>

1. <span data-ttu-id="166bc-360">V hello nový mobilní back-end aplikace, klikněte na **nastavení** > **mobilní aplikace** > **Data** > **+ přidat**.</span><span class="sxs-lookup"><span data-stu-id="166bc-360">In hello new Mobile App backend, click **Settings** > **Mobile App** > **Data** > **+Add**.</span></span>
2. <span data-ttu-id="166bc-361">V hello **přidat datové připojení** okně klikněte na tlačítko **SQL Database – nakonfigurujte požadovaná nastavení** > **vytvořit novou databázi**.</span><span class="sxs-lookup"><span data-stu-id="166bc-361">In hello **Add data connection** blade, click **SQL Database - Configure required settings** > **Create a new database**.</span></span>  <span data-ttu-id="166bc-362">Zadejte název nové databáze hello hello do hello **název** pole.</span><span class="sxs-lookup"><span data-stu-id="166bc-362">Enter hello name of hello new database in hello **Name** field.</span></span>
3. <span data-ttu-id="166bc-363">Klikněte na tlačítko **Server**.</span><span class="sxs-lookup"><span data-stu-id="166bc-363">Click **Server**.</span></span>  <span data-ttu-id="166bc-364">V hello **nový server** okno, zadejte název jedinečné serveru v hello **název serveru** pole a zadejte vhodný **přihlašovací jméno správce serveru** a **heslo**.</span><span class="sxs-lookup"><span data-stu-id="166bc-364">In hello **New server** blade, enter a unique server name in hello **Server name** field, and provide a suitable **Server admin login** and **Password**.</span></span>  <span data-ttu-id="166bc-365">Ujistěte se, **povolit službám azure tooaccess serveru** je zaškrtnuté.</span><span class="sxs-lookup"><span data-stu-id="166bc-365">Ensure **Allow azure services tooaccess server** is checked.</span></span>  <span data-ttu-id="166bc-366">Klikněte na **OK**.</span><span class="sxs-lookup"><span data-stu-id="166bc-366">Click **OK**.</span></span>

    ![Vytvoření databáze Azure SQL][6]
4. <span data-ttu-id="166bc-368">Na hello **novou databázi** okně klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="166bc-368">On hello **New database** blade, click **OK**.</span></span>
5. <span data-ttu-id="166bc-369">Zpět na hello **přidat datové připojení** vyberte **připojovací řetězec**, zadejte hello přihlašovací jméno a heslo, které jste zadali při vytváření databáze hello.</span><span class="sxs-lookup"><span data-stu-id="166bc-369">Back on hello **Add data connection** blade, select **Connection string**, enter hello login and password that you provided when creating hello database.</span></span>  <span data-ttu-id="166bc-370">Pokud použijete existující databázi, zadejte hello přihlašovací údaje pro tuto databázi.</span><span class="sxs-lookup"><span data-stu-id="166bc-370">If you use an existing database, provide hello login credentials for that database.</span></span>  <span data-ttu-id="166bc-371">Po zadání, klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="166bc-371">Once entered, click **OK**.</span></span>
6. <span data-ttu-id="166bc-372">Zpět na hello **přidat datové připojení** znovu, klikněte na **OK** toocreate hello databáze.</span><span class="sxs-lookup"><span data-stu-id="166bc-372">Back on hello **Add data connection** blade again, click **OK** toocreate hello database.</span></span>

<!--- END OF ALTERNATE INCLUDE -->

<span data-ttu-id="166bc-373">Vytvoření databáze hello může trvat několik minut.</span><span class="sxs-lookup"><span data-stu-id="166bc-373">Creation of hello database can take a few minutes.</span></span>  <span data-ttu-id="166bc-374">Použití hello **oznámení** oblasti toomonitor hello průběh nasazení hello.</span><span class="sxs-lookup"><span data-stu-id="166bc-374">Use hello **Notifications** area toomonitor hello progress of hello deployment.</span></span>  <span data-ttu-id="166bc-375">Není průběhu dokud hello databáze byla úspěšně nasazena.</span><span class="sxs-lookup"><span data-stu-id="166bc-375">Do not progress until hello database has been deployed successfully.</span></span>  <span data-ttu-id="166bc-376">Jakmile úspěšně nasazena, je pro instanci služby SQL Database hello ve vaší mobilní back-end nastavení aplikace vytvořit připojovací řetězec.</span><span class="sxs-lookup"><span data-stu-id="166bc-376">Once successfully deployed, a Connection String is created for hello SQL Database instance in your Mobile backend App Settings.</span></span>  <span data-ttu-id="166bc-377">Zobrazí se toto nastavení aplikace hello **nastavení** > **nastavení aplikace** > **připojovací řetězce**.</span><span class="sxs-lookup"><span data-stu-id="166bc-377">You can see this app setting in hello **Settings** > **Application settings** > **Connection strings**.</span></span>

### <span data-ttu-id="166bc-378"><a name="howto-tables-auth"></a>Postupy: ověřování vyžadovat pro přístup k tootables</span><span class="sxs-lookup"><span data-stu-id="166bc-378"><a name="howto-tables-auth"></a>How to: Require authentication for access tootables</span></span>
<span data-ttu-id="166bc-379">Pokud chcete toouse ověřování aplikace služby s koncovým bodem hello tabulky, musíte nakonfigurovat aplikaci služby ověřování v hello [portál Azure] první.</span><span class="sxs-lookup"><span data-stu-id="166bc-379">If you wish toouse App Service Authentication with hello tables endpoint, you must configure App Service Authentication in hello [Azure portal] first.</span></span>  <span data-ttu-id="166bc-380">Pro další podrobnosti o konfiguraci ověřování v Azure App Service, zkontrolujte hello Průvodci konfigurací pro zprostředkovatele identity hello hodláte toouse:</span><span class="sxs-lookup"><span data-stu-id="166bc-380">For more details about configuring authentication in an Azure App Service, review hello Configuration Guide for hello identity provider you intend toouse:</span></span>

* <span data-ttu-id="166bc-381">[Jak tooconfigure ověřování Azure Active Directory]</span><span class="sxs-lookup"><span data-stu-id="166bc-381">[How tooconfigure Azure Active Directory Authentication]</span></span>
* <span data-ttu-id="166bc-382">[Jak tooconfigure ověřování Facebook]</span><span class="sxs-lookup"><span data-stu-id="166bc-382">[How tooconfigure Facebook Authentication]</span></span>
* <span data-ttu-id="166bc-383">[Jak tooconfigure ověřování Google]</span><span class="sxs-lookup"><span data-stu-id="166bc-383">[How tooconfigure Google Authentication]</span></span>
* <span data-ttu-id="166bc-384">[Jak tooconfigure Microsoft Authentication]</span><span class="sxs-lookup"><span data-stu-id="166bc-384">[How tooconfigure Microsoft Authentication]</span></span>
* <span data-ttu-id="166bc-385">[Jak tooconfigure ověřování služby Twitter]</span><span class="sxs-lookup"><span data-stu-id="166bc-385">[How tooconfigure Twitter Authentication]</span></span>

<span data-ttu-id="166bc-386">Každá tabulka měla vlastností, které lze použít toocontrol přístup toohello tabulky.</span><span class="sxs-lookup"><span data-stu-id="166bc-386">Each table has an access property that can be used toocontrol access toohello table.</span></span>  <span data-ttu-id="166bc-387">Následující ukázka Hello zobrazuje staticky definovaných tabulku s je vyžadováno ověření.</span><span class="sxs-lookup"><span data-stu-id="166bc-387">hello following sample shows a statically defined table with authentication required.</span></span>

    var azureMobileApps = require('azure-mobile-apps');

    var table = azureMobileApps.table();

    // Define hello columns within hello table
    table.columns = {
        "text": "string",
        "complete": "boolean"
    };

    // Turn off dynamic schema
    table.dynamicSchema = false;

    // Require authentication tooaccess hello table
    table.access = 'authenticated';

    module.exports = table;

<span data-ttu-id="166bc-388">Vlastnost Hello přístupu může trvat jednu ze tří hodnot</span><span class="sxs-lookup"><span data-stu-id="166bc-388">hello access property can take one of three values</span></span>

* <span data-ttu-id="166bc-389">*Anonymní* označuje, že klientská aplikace hello je povolit tooread dat bez ověřování</span><span class="sxs-lookup"><span data-stu-id="166bc-389">*anonymous* indicates that hello client application is allowed tooread data without authentication</span></span>
* <span data-ttu-id="166bc-390">*ověření* indikuje, že klientská aplikace hello musí poslat platný ověřovací token požadavku hello</span><span class="sxs-lookup"><span data-stu-id="166bc-390">*authenticated* indicates that hello client application must send a valid authentication token with hello request</span></span>
* <span data-ttu-id="166bc-391">*zakázané* označuje, že tato tabulka je aktuálně zakázán</span><span class="sxs-lookup"><span data-stu-id="166bc-391">*disabled* indicates that this table is currently disabled</span></span>

<span data-ttu-id="166bc-392">Pokud vlastnost hello přístupu není definován, je povolený přístup bez ověřování.</span><span class="sxs-lookup"><span data-stu-id="166bc-392">If hello access property is undefined, unauthenticated access is allowed.</span></span>

### <span data-ttu-id="166bc-393"><a name="howto-tables-getidentity"></a>Postupy: použití ověřování deklarací identity s tabulkami</span><span class="sxs-lookup"><span data-stu-id="166bc-393"><a name="howto-tables-getidentity"></a>How to: Use authentication claims with your tables</span></span>
<span data-ttu-id="166bc-394">Můžete nastavit různé deklarace identity, které jsou požadovány při nastavení ověřování.</span><span class="sxs-lookup"><span data-stu-id="166bc-394">You can set up various claims that are requested when authentication is set up.</span></span>  <span data-ttu-id="166bc-395">Tyto deklarace nejsou obvykle dostupné prostřednictvím hello `context.user` objektu.</span><span class="sxs-lookup"><span data-stu-id="166bc-395">These claims are not normally available through hello `context.user` object.</span></span>  <span data-ttu-id="166bc-396">Však mohou být načteny pomocí hello `context.user.getIdentity()` metoda.</span><span class="sxs-lookup"><span data-stu-id="166bc-396">However, they can be retrieved using hello `context.user.getIdentity()` method.</span></span>  <span data-ttu-id="166bc-397">Hello `getIdentity()` metoda vrátí Promise, která přeloží tooan objektu.</span><span class="sxs-lookup"><span data-stu-id="166bc-397">hello `getIdentity()` method returns a Promise that resolves tooan object.</span></span>  <span data-ttu-id="166bc-398">objekt Hello se ukládá do klíčů metodou ověřování (facebook, google, twitter, microsoftaccount nebo aad).</span><span class="sxs-lookup"><span data-stu-id="166bc-398">hello object is keyed by the authentication method (facebook, google, twitter, microsoftaccount, or aad).</span></span>

<span data-ttu-id="166bc-399">Například pokud nastavíte Account Microsoft ověřování a deklarace identity požadavek hello e-mailové adresy, můžete přidat záznam hello e-mailové adresy toohello pomocí následující tabulky řadiče hello:</span><span class="sxs-lookup"><span data-stu-id="166bc-399">For example, if you set up Microsoft Account authentication and request hello email addresses claim, you can add hello email address toohello record with hello following table controller:</span></span>

    var azureMobileApps = require('azure-mobile-apps');

    // Create a new table definition
    var table = azureMobileApps.table();

    table.columns = {
        "emailAddress": "string",
        "text": "string",
        "complete": "boolean"
    };
    table.dynamicSchema = false;
    table.access = 'authenticated';

    /**
    * Limit hello context query toothose records with hello authenticated user email address
    * @param {Context} context hello operation context
    * @returns {Promise} context execution Promise
    */
    function queryContextForEmail(context) {
        return context.user.getIdentity().then((data) => {
            context.query.where({ emailAddress: data.microsoftaccount.claims.emailaddress });
            return context.execute();
        });
    }

    /**
    * Adds hello email address from hello claims toohello context item - used for
    * insert operations
    * @param {Context} context hello operation context
    * @returns {Promise} context execution Promise
    */
    function addEmailToContext(context) {
        return context.user.getIdentity().then((data) => {
            context.item.emailAddress = data.microsoftaccount.claims.emailaddress;
            return context.execute();
        });
    }

    // Configure specific code when hello client does a request
    // READ - only return records belonging toohello authenticated user
    table.read(queryContextForEmail);

    // CREATE - add or overwrite hello userId based on hello authenticated user
    table.insert(addEmailToContext);

    // UPDATE - only allow updating of record belong toohello authenticated user
    table.update(queryContextForEmail);

    // DELETE - only allow deletion of records belong toohello authenticated uer
    table.delete(queryContextForEmail);

    module.exports = table;

<span data-ttu-id="166bc-400">toosee jaké deklarace identity jsou k dispozici, použijte webové prohlížeče tooview hello `/.auth/me` koncový bod vašeho webu.</span><span class="sxs-lookup"><span data-stu-id="166bc-400">toosee what claims are available, use a web browser tooview hello `/.auth/me` endpoint of your site.</span></span>

### <span data-ttu-id="166bc-401"><a name="howto-tables-disabled"></a>Postupy: operace s tabulkou toospecific zakázat přístup</span><span class="sxs-lookup"><span data-stu-id="166bc-401"><a name="howto-tables-disabled"></a>How to: Disable access toospecific table operations</span></span>
<span data-ttu-id="166bc-402">V přidání tooappearing v tabulce hello může být vlastnost přístupu hello použité toocontrol jednotlivých operací.</span><span class="sxs-lookup"><span data-stu-id="166bc-402">In addition tooappearing on hello table, hello access property can be used toocontrol individual operations.</span></span>  <span data-ttu-id="166bc-403">Existují čtyři operace:</span><span class="sxs-lookup"><span data-stu-id="166bc-403">There are four operations:</span></span>

* <span data-ttu-id="166bc-404">*Přečtěte si* hello RESTful získat operace pro tabulku hello</span><span class="sxs-lookup"><span data-stu-id="166bc-404">*read* is hello RESTful GET operation on hello table</span></span>
* <span data-ttu-id="166bc-405">*Vložit* hello RESTful POST operace v tabulce hello</span><span class="sxs-lookup"><span data-stu-id="166bc-405">*insert* is hello RESTful POST operation on hello table</span></span>
* <span data-ttu-id="166bc-406">*Aktualizovat* hello RESTful oprava operace v tabulce hello</span><span class="sxs-lookup"><span data-stu-id="166bc-406">*update* is hello RESTful PATCH operation on hello table</span></span>
* <span data-ttu-id="166bc-407">*Odstranit* hello RESTful odstranit operace v tabulce hello</span><span class="sxs-lookup"><span data-stu-id="166bc-407">*delete* is hello RESTful DELETE operation on hello table</span></span>

<span data-ttu-id="166bc-408">Například může přát tooprovide neověřené tabulky jen pro čtení:</span><span class="sxs-lookup"><span data-stu-id="166bc-408">For example, you may wish tooprovide a read-only unauthenticated table:</span></span>

    var azureMobileApps = require('azure-mobile-apps');

    var table = azureMobileApps.table();

    // Read-Only table - only allow READ operations
    table.read.access = 'anonymous';
    table.insert.access = 'disabled';
    table.update.access = 'disabled';
    table.delete.access = 'disabled';

    module.exports = table;

### <span data-ttu-id="166bc-409"><a name="howto-tables-query"></a>Postupy: Úprava hello dotazu, který se používá s operace s tabulkou</span><span class="sxs-lookup"><span data-stu-id="166bc-409"><a name="howto-tables-query"></a>How to: Adjust hello query that is used with table operations</span></span>
<span data-ttu-id="166bc-410">Běžné požadavky pro operace s tabulkou je tooprovide omezeným zobrazením dat hello.</span><span class="sxs-lookup"><span data-stu-id="166bc-410">A common requirement for table operations is tooprovide a restricted view of hello data.</span></span>  <span data-ttu-id="166bc-411">Například může zadat tabulku, která je označené hello ověřeného ID uživatele tak, že můžete pouze pro čtení nebo aktualizovat vlastní záznamy.</span><span class="sxs-lookup"><span data-stu-id="166bc-411">For example, you may provide a table that is tagged with hello authenticated user ID such that you can only read or update your own records.</span></span>  <span data-ttu-id="166bc-412">Tuto funkci zajišťuje Hello následující definici tabulky:</span><span class="sxs-lookup"><span data-stu-id="166bc-412">hello following table definition provides this functionality:</span></span>

    var azureMobileApps = require('azure-mobile-apps');

    var table = azureMobileApps.table();

    // Define a static schema for hello table
    table.columns = {
        "userId": "string",
        "text": "string",
        "complete": "boolean"
    };
    table.dynamicSchema = false;

    // Require authentication for this table
    table.access = 'authenticated';

    // Ensure that only records for hello authenticated user are retrieved
    table.read(function (context) {
        context.query.where({ userId: context.user.id });
        return context.execute();
    });

    // When adding records, add or overwrite hello userId with hello authenticated user
    table.insert(function (context) {
        context.item.userId = context.user.id;
        return context.execute();
    });

    module.exports = table;

<span data-ttu-id="166bc-413">Operace, které obvykle spuštění dotazu mají vlastnost dotazu, můžete upravit místo, kde klauzule.</span><span class="sxs-lookup"><span data-stu-id="166bc-413">Operations that normally execute a query have a query property that you can adjust with a where clause.</span></span> <span data-ttu-id="166bc-414">Vlastnost Hello dotaz je [QueryJS] objektu, který je použité tooconvert toosomething dotazu OData, která hello back-end dat může zpracovat.</span><span class="sxs-lookup"><span data-stu-id="166bc-414">hello query property is a [QueryJS] object that is used tooconvert an OData query toosomething that hello data backend can process.</span></span>  <span data-ttu-id="166bc-415">Jednoduché rovnosti případech (např. hello předcházející jeden) lze mapu.</span><span class="sxs-lookup"><span data-stu-id="166bc-415">For simple equality cases (like hello preceding one), a map can be used.</span></span> <span data-ttu-id="166bc-416">Můžete také přidat konkrétní klauzule SQL:</span><span class="sxs-lookup"><span data-stu-id="166bc-416">You can also add specific SQL clauses:</span></span>

    context.query.where('myfield eq ?', 'value');

### <span data-ttu-id="166bc-417"><a name="howto-tables-softdelete"></a>Postupy: Konfigurace obnovitelného odstranění na tabulce</span><span class="sxs-lookup"><span data-stu-id="166bc-417"><a name="howto-tables-softdelete"></a>How to: Configure soft delete on a table</span></span>
<span data-ttu-id="166bc-418">Obnovitelného odstranění neodstraní ve skutečnosti záznamy.</span><span class="sxs-lookup"><span data-stu-id="166bc-418">Soft Delete does not actually delete records.</span></span>  <span data-ttu-id="166bc-419">Místo toho se označí je jako v databázi hello odstranit nastavením tootrue hello odstranit sloupce.</span><span class="sxs-lookup"><span data-stu-id="166bc-419">Instead it marks them as deleted within hello database by setting hello deleted column tootrue.</span></span>  <span data-ttu-id="166bc-420">dočasně odstraněné záznamy Hello Azure Mobile Apps SDK automaticky odebere z výsledků, pokud hello Mobile Client SDK používá IncludeDeleted().</span><span class="sxs-lookup"><span data-stu-id="166bc-420">hello Azure Mobile Apps SDK automatically removes soft-deleted records from results unless hello Mobile Client SDK uses IncludeDeleted().</span></span>  <span data-ttu-id="166bc-421">tooconfigure odstranit tabulku soft, nastavte hello `softDelete` vlastnost v souboru definice tabulky hello:</span><span class="sxs-lookup"><span data-stu-id="166bc-421">tooconfigure a table for soft delete, set hello `softDelete` property in hello table definition file:</span></span>

    var azureMobileApps = require('azure-mobile-apps');

    var table = azureMobileApps.table();

    // Define hello columns within hello table
    table.columns = {
        "text": "string",
        "complete": "boolean"
    };

    // Turn off dynamic schema
    table.dynamicSchema = false;

    // Turn on Soft Delete
    table.softDelete = true;

    // Require authentication tooaccess hello table
    table.access = 'authenticated';

    module.exports = table;

<span data-ttu-id="166bc-422">Byste měli zavést mechanismus pro vymazání záznamů – buď z klientské aplikace, prostřednictvím webové úlohy, funkce Azure nebo prostřednictvím vlastní rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="166bc-422">You should establish a mechanism for purging records - either from a client application, via a WebJob, Azure Function or through a custom API.</span></span>

### <span data-ttu-id="166bc-423"><a name="howto-tables-seeding"></a>Postupy: počáteční hodnoty databázi daty</span><span class="sxs-lookup"><span data-stu-id="166bc-423"><a name="howto-tables-seeding"></a>How to: Seed your database with data</span></span>
<span data-ttu-id="166bc-424">Při vytváření nové aplikace, můžete tooseed tabulku s daty.</span><span class="sxs-lookup"><span data-stu-id="166bc-424">When creating a new application, you may wish tooseed a table with data.</span></span>  <span data-ttu-id="166bc-425">To lze provést v rámci soubor JavaScript definice tabulky hello následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="166bc-425">This can be done within hello table definition JavaScript file as follows:</span></span>

    var azureMobileApps = require('azure-mobile-apps');

    var table = azureMobileApps.table();

    // Define hello columns within hello table
    table.columns = {
        "text": "string",
        "complete": "boolean"
    };
    table.seed = [
        { text: 'Example 1', complete: false },
        { text: 'Example 2', complete: true }
    ];

    // Turn off dynamic schema
    table.dynamicSchema = false;

    // Require authentication tooaccess hello table
    table.access = 'authenticated';

    module.exports = table;

<span data-ttu-id="166bc-426">Synchronizace replik indexů data je možné pouze při vytváření tabulky hello podle hello Azure Mobile Apps SDK.</span><span class="sxs-lookup"><span data-stu-id="166bc-426">Seeding of data is only done when hello table is created by hello Azure Mobile Apps SDK.</span></span>  <span data-ttu-id="166bc-427">Pokud hello tabulka již existuje v rámci hello databáze, je žádná data vloženy do tabulky hello.</span><span class="sxs-lookup"><span data-stu-id="166bc-427">If hello table already exists within hello database, no data is injected into hello table.</span></span>  <span data-ttu-id="166bc-428">Pokud je zapnutá dynamická schéma, schéma odvodit z dat hello nasadí.</span><span class="sxs-lookup"><span data-stu-id="166bc-428">If dynamic schema is turned on, then the schema is inferred from hello seeded data.</span></span>

<span data-ttu-id="166bc-429">Doporučujeme, abyste výslovně volání hello `tables.initialize()` metoda toocreate hello tabulky při spuštění služby hello.</span><span class="sxs-lookup"><span data-stu-id="166bc-429">We recommend that you explicitly call hello `tables.initialize()` method toocreate hello table when hello service starts running.</span></span>

### <span data-ttu-id="166bc-430"><a name="Swagger"></a>Postupy: Povolení podpory Swagger</span><span class="sxs-lookup"><span data-stu-id="166bc-430"><a name="Swagger"></a>How to: Enable Swagger support</span></span>
<span data-ttu-id="166bc-431">Azure App Service Mobile Apps se dodává s integrovanou [Swagger] podporovat.</span><span class="sxs-lookup"><span data-stu-id="166bc-431">Azure App Service Mobile Apps comes with built-in [Swagger] support.</span></span>  <span data-ttu-id="166bc-432">tooenable Swagger podpory nejdřív nainstalovat swagger uživatelského rozhraní hello jako závislost:</span><span class="sxs-lookup"><span data-stu-id="166bc-432">tooenable Swagger support, first install hello swagger-ui as a dependency:</span></span>

    npm install --save swagger-ui

<span data-ttu-id="166bc-433">Po instalaci můžete povolit podporu Swagger v konstruktoru Azure Mobile Apps hello:</span><span class="sxs-lookup"><span data-stu-id="166bc-433">Once installed, you can enable Swagger support in hello Azure Mobile Apps constructor:</span></span>

    var mobile = azureMobileApps({ swagger: true });

<span data-ttu-id="166bc-434">Budete pravděpodobně pouze chcete tooenable Swagger podporovaných v edicích vývoj.</span><span class="sxs-lookup"><span data-stu-id="166bc-434">You probably only want tooenable Swagger support in development editions.</span></span>  <span data-ttu-id="166bc-435">Můžete to provést s využitím `NODE_ENV` nastavení aplikace:</span><span class="sxs-lookup"><span data-stu-id="166bc-435">You can do this by utilizing the `NODE_ENV` app setting:</span></span>

    var mobile = azureMobileApps({ swagger: process.env.NODE_ENV !== 'production' });

<span data-ttu-id="166bc-436">Hello koncového bodu swaggeru se nachází v http://*yoursite*.azurewebsites.net/swagger.</span><span class="sxs-lookup"><span data-stu-id="166bc-436">hello swagger endpoint is located at http://*yoursite*.azurewebsites.net/swagger.</span></span>  <span data-ttu-id="166bc-437">Dostanete hello uživatelské rozhraní Swagger prostřednictvím hello `/swagger/ui` koncový bod.</span><span class="sxs-lookup"><span data-stu-id="166bc-437">You can access hello Swagger UI via hello `/swagger/ui` endpoint.</span></span>  <span data-ttu-id="166bc-438">Pokud zvolíte ověřování toorequire napříč celou aplikaci, vytvoří Swagger k chybě.</span><span class="sxs-lookup"><span data-stu-id="166bc-438">if you choose toorequire authentication across your entire application, Swagger produces an error.</span></span>  <span data-ttu-id="166bc-439">Nejlepších výsledků dosáhnete, zvolte tooallow neověřené požadavky prostřednictvím v hello ověřování služby Azure App / nastavení autorizace, pak řídit ověřování pomocí hello `table.access` vlastnost.</span><span class="sxs-lookup"><span data-stu-id="166bc-439">For best results, choose tooallow unauthenticated requests through in hello Azure App Service Authentication / Authorization settings, then control authentication using hello `table.access` property.</span></span>

<span data-ttu-id="166bc-440">Můžete také přidat hello Swagger možnost tooyour `azureMobile.js` souboru, pouze pokud chcete podporu Swagger při vývoji místně.</span><span class="sxs-lookup"><span data-stu-id="166bc-440">You can also add hello Swagger option tooyour `azureMobile.js` file if you only want Swagger support when developing locally.</span></span>

## <a name="a-namepushpush-notifications"></a><span data-ttu-id="166bc-441"><a name="push">Nabízená oznámení</span><span class="sxs-lookup"><span data-stu-id="166bc-441"><a name="push">Push notifications</span></span>
<span data-ttu-id="166bc-442">Mobilní aplikace se integruje s Azure Notification Hubs tooenable jste toosend cílem nabízená oznámení toomillions zařízení pro všechny hlavní platformy.</span><span class="sxs-lookup"><span data-stu-id="166bc-442">Mobile Apps integrates with Azure Notification Hubs tooenable you toosend targeted push notifications toomillions of devices across all major platforms.</span></span> <span data-ttu-id="166bc-443">Pomocí centra oznámení můžete odesílat nabízená oznámení tooiOS, zařízení s Androidem a Windows.</span><span class="sxs-lookup"><span data-stu-id="166bc-443">By using Notification Hubs, you can send push notifications tooiOS, Android and Windows devices.</span></span> <span data-ttu-id="166bc-444">toolearn informace o všech funkcí, které můžete provést s Notification Hubs najdete v části [přehledu této služby](../notification-hubs/notification-hubs-push-notification-overview.md).</span><span class="sxs-lookup"><span data-stu-id="166bc-444">toolearn more about all that you can do with Notification Hubs, see [Notification Hubs Overview](../notification-hubs/notification-hubs-push-notification-overview.md).</span></span>

### <span data-ttu-id="166bc-445"></a><a name="send-push"></a>Postupy: odesílání nabízených oznámení</span><span class="sxs-lookup"><span data-stu-id="166bc-445"></a><a name="send-push"></a>How to: Send push notifications</span></span>
<span data-ttu-id="166bc-446">Hello následující kód ukazuje, jak toouse hello nabízené objekt toosend vysílání nabízených oznámení tooregistered iOS zařízení:</span><span class="sxs-lookup"><span data-stu-id="166bc-446">hello following code shows how toouse hello push object toosend a broadcast push notification tooregistered iOS devices:</span></span>

    // Create an APNS payload.
    var payload = '{"aps": {"alert": "This is an APNS payload."}}';

    // Only do hello push if configured
    if (context.push) {
        // Send a push notification using APNS.
        context.push.apns.send(null, payload, function (error) {
            if (error) {
                // Do something or log hello error.
            }
        });
    }

<span data-ttu-id="166bc-447">Vytvořením šablony nabízené registrace z hello klienta, můžete místo toho odeslat šablony nabízená zpráva toodevices na všech podporovaných platformách.</span><span class="sxs-lookup"><span data-stu-id="166bc-447">By creating a template push registration from hello client, you can instead send a template push message toodevices on all supported platforms.</span></span> <span data-ttu-id="166bc-448">Následující kód ukazuje, jak Hello toosend šablony oznámení:</span><span class="sxs-lookup"><span data-stu-id="166bc-448">hello following code shows how toosend a template notification:</span></span>

    // Define hello template payload.
    var payload = '{"messageParam": "This is a template payload."}';

    // Only do hello push if configured
    if (context.push) {
        // Send a template notification.
        context.push.send(null, payload, function (error) {
            if (error) {
                // Do something or log hello error.
            }
        });
    }


### <span data-ttu-id="166bc-449"><a name="push-user"></a>Postupy: odeslání nabízených oznámení tooan ověřit uživatele pomocí značek</span><span class="sxs-lookup"><span data-stu-id="166bc-449"><a name="push-user"></a>How to: Send push notifications tooan authenticated user using tags</span></span>
<span data-ttu-id="166bc-450">Pokud ověřený uživatel zaregistruje pro nabízená oznámení, značku ID uživatele se automaticky přidá toohello registrace.</span><span class="sxs-lookup"><span data-stu-id="166bc-450">When an authenticated user registers for push notifications, a user ID tag is automatically added toohello registration.</span></span> <span data-ttu-id="166bc-451">Pomocí této značky může odesílat nabízená oznámení tooall zařízení registrovaná podle konkrétního uživatele.</span><span class="sxs-lookup"><span data-stu-id="166bc-451">By using this tag, you can send push notifications tooall devices registered by a specific user.</span></span> <span data-ttu-id="166bc-452">Hello následující kód získá hello identifikátor SID uživatele, který vytvořil požadavek hello a odešle registrace zařízení k tooevery šablony nabízených oznámení pro tohoto uživatele:</span><span class="sxs-lookup"><span data-stu-id="166bc-452">hello following code gets hello SID of user making hello request and sends a template push notification tooevery device registration for that user:</span></span>

    // Only do hello push if configured
    if (context.push) {
        // Send a notification toohello current user.
        context.push.send(context.user.id, payload, function (error) {
            if (error) {
                // Do something or log hello error.
            }
        });
    }

<span data-ttu-id="166bc-453">Při registraci pro nabízená oznámení z ověřený klient, ujistěte se, že před pokusem o registraci dokončením ověřování.</span><span class="sxs-lookup"><span data-stu-id="166bc-453">When registering for push notifications from an authenticated client, make sure that authentication is complete before attempting registration.</span></span>

## <span data-ttu-id="166bc-454"><a name="CustomAPI"></a>Vlastní rozhraní API</span><span class="sxs-lookup"><span data-stu-id="166bc-454"><a name="CustomAPI"></a> Custom APIs</span></span>
### <span data-ttu-id="166bc-455"><a name="howto-customapi-basic"></a>Postupy: definování vlastní rozhraní API</span><span class="sxs-lookup"><span data-stu-id="166bc-455"><a name="howto-customapi-basic"></a>How to: Define a custom API</span></span>
<span data-ttu-id="166bc-456">Kromě toho toohello přístup k datům rozhraní API prostřednictvím koncového bodu /tables hello, Azure Mobile Apps můžete zadat vlastní pokrytí rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="166bc-456">In addition toohello data access API via hello /tables endpoint, Azure Mobile Apps can provide custom API coverage.</span></span>  <span data-ttu-id="166bc-457">Vlastní rozhraní API jsou definovány v podobné definice tabulek toohello způsob a přístup k veškerým hello stejné zařízení, včetně ověřování.</span><span class="sxs-lookup"><span data-stu-id="166bc-457">Custom APIs are defined in a similar way toohello table definitions and can access all hello same facilities, including authentication.</span></span>

<span data-ttu-id="166bc-458">Pokud chcete toouse ověřování aplikace služby s rozhraním API vlastní, musíte nakonfigurovat aplikaci služby ověřování v hello [portál Azure] první.</span><span class="sxs-lookup"><span data-stu-id="166bc-458">If you wish toouse App Service Authentication with a Custom API, you must configure App Service Authentication in hello [Azure portal] first.</span></span>  <span data-ttu-id="166bc-459">Pro další podrobnosti o konfiguraci ověřování v Azure App Service, zkontrolujte hello Průvodci konfigurací pro zprostředkovatele identity hello hodláte toouse:</span><span class="sxs-lookup"><span data-stu-id="166bc-459">For more details about configuring authentication in an Azure App Service, review hello Configuration Guide for hello identity provider you intend toouse:</span></span>

* <span data-ttu-id="166bc-460">[Jak tooconfigure ověřování Azure Active Directory]</span><span class="sxs-lookup"><span data-stu-id="166bc-460">[How tooconfigure Azure Active Directory Authentication]</span></span>
* <span data-ttu-id="166bc-461">[Jak tooconfigure ověřování Facebook]</span><span class="sxs-lookup"><span data-stu-id="166bc-461">[How tooconfigure Facebook Authentication]</span></span>
* <span data-ttu-id="166bc-462">[Jak tooconfigure ověřování Google]</span><span class="sxs-lookup"><span data-stu-id="166bc-462">[How tooconfigure Google Authentication]</span></span>
* <span data-ttu-id="166bc-463">[Jak tooconfigure Microsoft Authentication]</span><span class="sxs-lookup"><span data-stu-id="166bc-463">[How tooconfigure Microsoft Authentication]</span></span>
* <span data-ttu-id="166bc-464">[Jak tooconfigure ověřování služby Twitter]</span><span class="sxs-lookup"><span data-stu-id="166bc-464">[How tooconfigure Twitter Authentication]</span></span>

<span data-ttu-id="166bc-465">Vlastní rozhraní API jsou definovány v hodně hello stejným způsobem, jak hello tabulky rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="166bc-465">Custom APIs are defined in much hello same way as hello Tables API.</span></span>

1. <span data-ttu-id="166bc-466">Vytvoření **rozhraní api** adresáře</span><span class="sxs-lookup"><span data-stu-id="166bc-466">Create an **api** directory</span></span>
2. <span data-ttu-id="166bc-467">Vytvořte soubor JavaScript definice rozhraní API v hello **rozhraní api** adresáře.</span><span class="sxs-lookup"><span data-stu-id="166bc-467">Create an API definition JavaScript file in hello **api** directory.</span></span>
3. <span data-ttu-id="166bc-468">Použití hello import metoda tooimport hello **rozhraní api** adresáře.</span><span class="sxs-lookup"><span data-stu-id="166bc-468">Use hello import method tooimport hello **api** directory.</span></span>

<span data-ttu-id="166bc-469">Zde je definice rozhraní api prototypu hello na základě vzorku basic aplikace hello, které jsme použili předtím.</span><span class="sxs-lookup"><span data-stu-id="166bc-469">Here is hello prototype api definition based on hello basic-app sample we used earlier.</span></span>

    var express = require('express'),
        azureMobileApps = require('azure-mobile-apps');

    var app = express(),
        mobile = azureMobileApps();

    // Import hello Custom API
    mobile.api.import('./api');

    // Add hello mobile API so it is accessible as a Web API
    app.use(mobile);

    // Start listening on HTTP
    app.listen(process.env.PORT || 3000);

<span data-ttu-id="166bc-470">Podívejme příklad rozhraní API, které vrátí datum server hello pomocí hello *Date.now()* metoda.</span><span class="sxs-lookup"><span data-stu-id="166bc-470">Let's take an example API that returns hello server date using hello *Date.now()* method.</span></span>  <span data-ttu-id="166bc-471">Zde je soubor api/date.js hello:</span><span class="sxs-lookup"><span data-stu-id="166bc-471">Here is hello api/date.js file:</span></span>

    var api = {
        get: function (req, res, next) {
            var date = { currentTime: Date.now() };
            res.status(200).type('application/json').send(date);
        });
    };

    module.exports = api;

<span data-ttu-id="166bc-472">Každý parametr je jedním z hello standardní RESTful příkazy - GET, POST, PATCH nebo odstranit.</span><span class="sxs-lookup"><span data-stu-id="166bc-472">Each parameter is one of hello standard RESTful verbs - GET, POST, PATCH, or DELETE.</span></span>  <span data-ttu-id="166bc-473">Metoda Hello je standard [ExpressJS Middleware] funkce, která odesílá výstup hello vyžaduje.</span><span class="sxs-lookup"><span data-stu-id="166bc-473">hello method is a standard [ExpressJS Middleware] function that sends hello required output.</span></span>

### <span data-ttu-id="166bc-474"><a name="howto-customapi-auth"></a>Postupy: ověřování vyžadovat pro přístup k tooa vlastního rozhraní API</span><span class="sxs-lookup"><span data-stu-id="166bc-474"><a name="howto-customapi-auth"></a>How to: Require authentication for access tooa custom API</span></span>
<span data-ttu-id="166bc-475">Azure Mobile Apps SDK implementuje ověřování v hello stejným způsobem jako pro koncový bod tabulky hello i vlastní rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="166bc-475">Azure Mobile Apps SDK implements authentication in hello same way for both hello tables endpoint and custom APIs.</span></span>  <span data-ttu-id="166bc-476">Chcete-li přidat rozhraní API ověřování toohello vyvinuté v předchozí části hello, přidejte **přístup** vlastnost:</span><span class="sxs-lookup"><span data-stu-id="166bc-476">To add authentication toohello API developed in hello previous section, add an **access** property:</span></span>

    var api = {
        get: function (req, res, next) {
            var date = { currentTime: Date.now() };
            res.status(200).type('application/json').send(date);
        });
    };
    // All methods must be authenticated.
    api.access = 'authenticated';

    module.exports = api;

<span data-ttu-id="166bc-477">Můžete také zadat ověřování na konkrétní operace:</span><span class="sxs-lookup"><span data-stu-id="166bc-477">You can also specify authentication on specific operations:</span></span>

    var api = {
        get: function (req, res, next) {
            var date = { currentTime: Date.now() };
            res.status(200).type('application/json').send(date);
        }
    };
    // hello GET methods must be authenticated.
    api.get.access = 'authenticated';

    module.exports = api;

<span data-ttu-id="166bc-478">stejný token, který se používá pro koncový bod tabulky hello Hello musí použít pro vlastní rozhraní API, které vyžadují ověřování.</span><span class="sxs-lookup"><span data-stu-id="166bc-478">hello same token that is used for hello tables endpoint must be used for custom APIs requiring authentication.</span></span>

### <span data-ttu-id="166bc-479"><a name="howto-customapi-auth"></a>Postupy: zpracování nahrávání velkých souborů</span><span class="sxs-lookup"><span data-stu-id="166bc-479"><a name="howto-customapi-auth"></a>How to: Handle large file uploads</span></span>
<span data-ttu-id="166bc-480">Azure Mobile Apps SDK používá hello [textu analyzátor middleware](https://github.com/expressjs/body-parser) tooaccept a dekódování obsah textu v vaše žádost.</span><span class="sxs-lookup"><span data-stu-id="166bc-480">Azure Mobile Apps SDK uses hello [body-parser middleware](https://github.com/expressjs/body-parser) tooaccept and decode body content in your submission.</span></span>  <span data-ttu-id="166bc-481">Můžete předkonfigurovat textu analyzátor tooaccept větší nahrávání souborů:</span><span class="sxs-lookup"><span data-stu-id="166bc-481">You can pre-configure body-parser tooaccept larger file uploads:</span></span>

    var express = require('express'),
        bodyParser = require('body-parser'),
        azureMobileApps = require('azure-mobile-apps');

    var app = express(),
        mobile = azureMobileApps();

    // Set up large body content handling
    app.use(bodyParser.json({ limit: '50mb' }));
    app.use(bodyParser.urlencoded({ limit: '50mb', extended: true }));

    // Import hello Custom API
    mobile.api.import('./api');

    // Add hello mobile API so it is accessible as a Web API
    app.use(mobile);

    // Start listening on HTTP
    app.listen(process.env.PORT || 3000);

<span data-ttu-id="166bc-482">soubor Hello je před samotným přenosem kódování base-64.</span><span class="sxs-lookup"><span data-stu-id="166bc-482">hello file is base-64 encoded before transmission.</span></span>  <span data-ttu-id="166bc-483">Zvyšuje se tím velikost hello skutečné nahrávání hello (a proto hello velikost, kterou musíte vzít v úvahu pro).</span><span class="sxs-lookup"><span data-stu-id="166bc-483">This increases hello size of hello actual upload (and hence hello size you must account for).</span></span>

### <span data-ttu-id="166bc-484"><a name="howto-customapi-sql"></a>Postup: provedení vlastní SQL příkazy</span><span class="sxs-lookup"><span data-stu-id="166bc-484"><a name="howto-customapi-sql"></a>How to: Execute custom SQL statements</span></span>
<span data-ttu-id="166bc-485">Hello Azure Mobile Apps SDK umožňuje přístup toohello celý kontextu prostřednictvím objektu žádosti hello, což vám tooexecute snadno parametry zprostředkovatel definované datové toohello příkazy SQL:</span><span class="sxs-lookup"><span data-stu-id="166bc-485">hello Azure Mobile Apps SDK allows access toohello entire Context through hello request object, allowing you tooexecute parameterized SQL statements toohello defined data provider easily:</span></span>

    var api = {
        get: function (request, response, next) {
            // Check for parameters - if not there, pass on tooa later API call
            if (typeof request.params.completed === 'undefined')
                return next();

            // Define hello query - anything that can be handled by hello mssql
            // driver is allowed.
            var query = {
                sql: 'UPDATE TodoItem SET complete=@completed',
                parameters: [{
                    completed: request.params.completed
                }]
            };

            // Execute hello query.  hello context for Azure Mobile Apps is available through
            // request.azureMobile - hello data object contains hello configured data provider.
            request.azureMobile.data.execute(query)
            .then(function (results) {
                response.json(results);
            });
        }
    };

    api.get.access = 'authenticated';
    module.exports = api;

## <span data-ttu-id="166bc-486"><a name="Debugging"></a>Ladění, snadno tabulek a snadno rozhraní API</span><span class="sxs-lookup"><span data-stu-id="166bc-486"><a name="Debugging"></a>Debugging, Easy Tables, and Easy APIs</span></span>
### <span data-ttu-id="166bc-487"><a name="howto-diagnostic-logs"></a>Postupy: ladění, Diagnostika a řešení potíží s Azure Mobile apps</span><span class="sxs-lookup"><span data-stu-id="166bc-487"><a name="howto-diagnostic-logs"></a>How to: Debug, diagnose, and troubleshoot Azure Mobile apps</span></span>
<span data-ttu-id="166bc-488">Hello Azure App Service poskytuje několik ladění a řešení potíží s techniky pro aplikace Node.js.</span><span class="sxs-lookup"><span data-stu-id="166bc-488">hello Azure App Service provides several debugging and troubleshooting techniques for Node.js applications.</span></span>
<span data-ttu-id="166bc-489">Odkažte toohello následující články tooget spuštěna při řešení potíží váš back-end Node.js Mobile:</span><span class="sxs-lookup"><span data-stu-id="166bc-489">Refer toohello following articles tooget started in troubleshooting your Node.js Mobile backend:</span></span>

* <span data-ttu-id="166bc-490">[Monitorování služby Azure App Service]</span><span class="sxs-lookup"><span data-stu-id="166bc-490">[Monitoring an Azure App Service]</span></span>
* <span data-ttu-id="166bc-491">[Povolit protokolování diagnostiky v Azure App Service]</span><span class="sxs-lookup"><span data-stu-id="166bc-491">[Enable Diagnostic Logging in Azure App Service]</span></span>
* <span data-ttu-id="166bc-492">[Řešení potíží s Azure App Service v sadě Visual Studio]</span><span class="sxs-lookup"><span data-stu-id="166bc-492">[Troubleshoot an Azure App Service in Visual Studio]</span></span>

<span data-ttu-id="166bc-493">Aplikace Node.js mají přístup tooa širokou škálu protokolů diagnostiky nástroje.</span><span class="sxs-lookup"><span data-stu-id="166bc-493">Node.js applications have access tooa wide range of diagnostic log tools.</span></span>  <span data-ttu-id="166bc-494">Interně hello používá Azure Mobile Apps Node.js SDK [Winstona] pro protokolování diagnostiky.</span><span class="sxs-lookup"><span data-stu-id="166bc-494">Internally, hello Azure Mobile Apps Node.js SDK uses [Winston] for diagnostic logging.</span></span>  <span data-ttu-id="166bc-495">Povolení režimu ladění, nebo nastavení hello automaticky povoleno protokolování **MS_DebugMode** tootrue nastavení aplikace v hello [portál Azure].</span><span class="sxs-lookup"><span data-stu-id="166bc-495">Logging is automatically enabled by enabling debug mode or by setting hello **MS_DebugMode** app setting tootrue in hello [Azure portal].</span></span> <span data-ttu-id="166bc-496">Vygenerovaný protokolů se objeví v hello diagnostické protokoly na hello [portál Azure].</span><span class="sxs-lookup"><span data-stu-id="166bc-496">Generated logs appear in hello Diagnostic Logs on hello [Azure portal].</span></span>

### <span data-ttu-id="166bc-497"><a name="in-portal-editing"></a><a name="work-easy-tables"></a>Postupy: práce s tabulkami snadno v hello portálu Azure</span><span class="sxs-lookup"><span data-stu-id="166bc-497"><a name="in-portal-editing"></a><a name="work-easy-tables"></a>How to: Work with Easy Tables in hello Azure portal</span></span>
<span data-ttu-id="166bc-498">Snadno tabulek portálu hello umožňují vytváření a práci s pravé tabulky hello portálu.</span><span class="sxs-lookup"><span data-stu-id="166bc-498">Easy Tables in hello portal let you create and work with tables right in hello portal.</span></span> <span data-ttu-id="166bc-499">Dokonce můžete upravit operace s tabulkou pomocí hello Editor služby aplikace.</span><span class="sxs-lookup"><span data-stu-id="166bc-499">You can even edit table operations using hello App Service Editor.</span></span>

<span data-ttu-id="166bc-500">Když kliknete na tlačítko **snadno tabulky** v nastavení vašeho back-end serveru, můžete přidávat, upravovat nebo odstranění tabulky.</span><span class="sxs-lookup"><span data-stu-id="166bc-500">When you click **Easy tables** in your backend site settings, you can add, modify, or delete a table.</span></span> <span data-ttu-id="166bc-501">Můžete také zobrazit data v tabulce hello.</span><span class="sxs-lookup"><span data-stu-id="166bc-501">You can also see data in hello table.</span></span>

![Práce s tabulkami snadno](./media/app-service-mobile-node-backend-how-to-use-server-sdk/mobile-apps-easy-tables.png)

<span data-ttu-id="166bc-503">Hello následující příkazy jsou k dispozici na panelu příkazů hello pro tabulku:</span><span class="sxs-lookup"><span data-stu-id="166bc-503">hello following commands are available on hello command bar for a table:</span></span>

* <span data-ttu-id="166bc-504">**Změna oprávnění** – upravit hello oprávnění pro čtení, vložit, aktualizovat a odstranit operací v tabulce hello.</span><span class="sxs-lookup"><span data-stu-id="166bc-504">**Change permissions** - modify hello permission for read, insert, update and delete operations on hello table.</span></span>
  <span data-ttu-id="166bc-505">Možnosti jsou tooallow anonymní přístup, toorequire ověřování nebo toodisable všechny toohello operace přístupu k.</span><span class="sxs-lookup"><span data-stu-id="166bc-505">Options are tooallow anonymous access, toorequire authentication, or toodisable all access toohello operation.</span></span>
* <span data-ttu-id="166bc-506">**Upravte skript** -hello souboru skriptu pro tabulku hello je otevřen v hello Editor aplikace služby.</span><span class="sxs-lookup"><span data-stu-id="166bc-506">**Edit script** - hello script file for hello table is opened in hello App Service Editor.</span></span>
* <span data-ttu-id="166bc-507">**Správa schématu** – přidání nebo odstranění sloupce nebo změna hello tabulky indexu.</span><span class="sxs-lookup"><span data-stu-id="166bc-507">**Manage schema** - add or delete columns or change hello table index.</span></span>
* <span data-ttu-id="166bc-508">**Odstranit tabulku** -zkrátí existující tabulky se odstraňuje všechny řádky dat, ale ponechat hello schématu beze změny.</span><span class="sxs-lookup"><span data-stu-id="166bc-508">**Clear table** - truncates an existing table be deleting all data rows but leaving hello schema unchanged.</span></span>
* <span data-ttu-id="166bc-509">**Odstranit řádky** -odstranit jednotlivé řádky dat.</span><span class="sxs-lookup"><span data-stu-id="166bc-509">**Delete rows** - delete individual rows of data.</span></span>
* <span data-ttu-id="166bc-510">**Protokoly streamování zobrazení** -propojení toohello streamování služby protokolování pro svůj web.</span><span class="sxs-lookup"><span data-stu-id="166bc-510">**View streaming logs** - connects you toohello streaming log service for your site.</span></span>

### <span data-ttu-id="166bc-511"><a name="work-easy-apis"></a>Postupy: práci s rozhraními API snadno v hello portálu Azure</span><span class="sxs-lookup"><span data-stu-id="166bc-511"><a name="work-easy-apis"></a>How to: Work with Easy APIs in hello Azure portal</span></span>
<span data-ttu-id="166bc-512">Snadno rozhraní API portálu hello umožňují vytvářet a pracovat s vlastní práva rozhraní API portálu hello.</span><span class="sxs-lookup"><span data-stu-id="166bc-512">Easy APIs in hello portal let you create and work with custom APIs right in hello portal.</span></span> <span data-ttu-id="166bc-513">Můžete upravit skriptů rozhraní API pomocí hello Editor aplikace služby.</span><span class="sxs-lookup"><span data-stu-id="166bc-513">You can edit API scripts using hello App Service Editor.</span></span>

<span data-ttu-id="166bc-514">Když kliknete na tlačítko **rozhraní API pro snadný** v nastavení vašeho back-end serveru, můžete přidávat, upravovat nebo odstranit vlastní koncový bod rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="166bc-514">When you click **Easy APIs** in your backend site settings, you can add, modify, or delete a custom API endpoint.</span></span>

![Práci s rozhraními API snadno](./media/app-service-mobile-node-backend-how-to-use-server-sdk/mobile-apps-easy-apis.png)

<span data-ttu-id="166bc-516">Hello portálu můžete změnit hello přístupová oprávnění pro danou akci HTTP, upravte soubor skriptu rozhraní API hello v editoru služby aplikace nebo zobrazit protokoly streamování hello.</span><span class="sxs-lookup"><span data-stu-id="166bc-516">In hello portal, you can change hello access permissions for a given HTTP action, edit hello API script file in the App Service Editor, or view hello streaming logs.</span></span>

### <span data-ttu-id="166bc-517"><a name="online-editor"></a>Postupy: úpravy kódu v hello Editor služby aplikace</span><span class="sxs-lookup"><span data-stu-id="166bc-517"><a name="online-editor"></a>How to: Edit code in hello App Service Editor</span></span>
<span data-ttu-id="166bc-518">Hello portál Azure umožňuje upravit soubory skriptu back-end Node.js v hello Editor služby aplikace bez nutnosti stáhnout hello projektu tooyour místního počítače.</span><span class="sxs-lookup"><span data-stu-id="166bc-518">hello Azure portal lets you edit your Node.js backend script files in hello App Service Editor without having to download hello project tooyour local computer.</span></span> <span data-ttu-id="166bc-519">soubory skriptu tooedit v online editoru hello:</span><span class="sxs-lookup"><span data-stu-id="166bc-519">tooedit script files in hello online editor:</span></span>

1. <span data-ttu-id="166bc-520">V okně back-end mobilní aplikace, klikněte na tlačítko **všechna nastavení** > buď **snadno tabulky** nebo **rozhraní API pro snadný**, klikněte na tabulku nebo rozhraní API a pak klikněte na tlačítko **upravte skript**.</span><span class="sxs-lookup"><span data-stu-id="166bc-520">In your Mobile App backend blade, click **All settings** > either **Easy tables** or **Easy APIs**, click a table or API, then click **Edit script**.</span></span> <span data-ttu-id="166bc-521">soubor skriptu Hello je otevřen v hello Editor aplikace služby.</span><span class="sxs-lookup"><span data-stu-id="166bc-521">hello script file is opened in hello App Service Editor.</span></span>

    ![Editor služby aplikace](./media/app-service-mobile-node-backend-how-to-use-server-sdk/mobile-apps-visual-studio-editor.png)
2. <span data-ttu-id="166bc-523">Zkontrolujte souboru změny toohello kódu v editoru online hello.</span><span class="sxs-lookup"><span data-stu-id="166bc-523">Make your changes toohello code file in hello online editor.</span></span> <span data-ttu-id="166bc-524">Změny se při psaní automaticky uloží.</span><span class="sxs-lookup"><span data-stu-id="166bc-524">Changes are saved automatically as you type.</span></span>

<!-- Images -->
[0]: ./media/app-service-mobile-node-backend-how-to-use-server-sdk/npm-init.png
[1]: ./media/app-service-mobile-node-backend-how-to-use-server-sdk/vs2015-new-project.png
[2]: ./media/app-service-mobile-node-backend-how-to-use-server-sdk/vs2015-install-npm.png
[3]: ./media/app-service-mobile-node-backend-how-to-use-server-sdk/sqlexpress-config.png
[4]: ./media/app-service-mobile-node-backend-how-to-use-server-sdk/sqlexpress-authconfig.png
[5]: ./media/app-service-mobile-node-backend-how-to-use-server-sdk/sqlexpress-newuser-1.png
[6]: ./media/app-service-mobile-node-backend-how-to-use-server-sdk/dotnet-backend-create-db.png

<!-- URLs -->
[Rychlý start Android klienta]: app-service-mobile-android-get-started.md
[Rychlý start Apache Cordova klienta]: app-service-mobile-cordova-get-started.md
[iOS klienta rychlý start]: app-service-mobile-ios-get-started.md
[Rychlý start Xamarin.iOS klienta]: app-service-mobile-xamarin-ios-get-started.md
[Rychlé spuštění klienta Xamarin.Android]: app-service-mobile-xamarin-android-get-started.md
[Rychlý start Xamarin.Forms klienta]: app-service-mobile-xamarin-forms-get-started.md
[Rychlé spuštění klienta Windows Store]: app-service-mobile-windows-store-dotnet-get-started.md
[HTML/Javascript Client QuickStart]: app-service-html-get-started.md
[synchronizaci dat offline]: app-service-mobile-offline-data-sync.md
[Jak tooconfigure ověřování Azure Active Directory]: app-service-mobile-how-to-configure-active-directory-authentication.md
[Jak tooconfigure ověřování Facebook]: app-service-mobile-how-to-configure-facebook-authentication.md
[Jak tooconfigure ověřování Google]: app-service-mobile-how-to-configure-google-authentication.md
[Jak tooconfigure Microsoft Authentication]: app-service-mobile-how-to-configure-microsoft-authentication.md
[Jak tooconfigure ověřování služby Twitter]: app-service-mobile-how-to-configure-twitter-authentication.md
[Příručka pro nasazení Azure App Service]: ../app-service-web/web-sites-deploy.md
[Monitorování služby Azure App Service]: ../app-service-web/web-sites-monitor.md
[Povolit protokolování diagnostiky v Azure App Service]: ../app-service-web/web-sites-enable-diagnostic-log.md
[Řešení potíží s Azure App Service v sadě Visual Studio]: ../app-service-web/web-sites-dotnet-troubleshoot-visual-studio.md
[zadejte hello uzlu verze]: ../nodejs-specify-node-version-azure-apps.md
[použijte moduly uzlu]: ../nodejs-use-node-modules-azure-apps.md
[Create a new Azure App Service]: ../app-service-web/
[azure-mobile-apps]: https://www.npmjs.com/package/azure-mobile-apps
[Express]: http://expressjs.com/
[Swagger]: http://swagger.io/

[portál Azure]: https://portal.azure.com/
[OData]: http://www.odata.org
[Promise]: https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise
[basicapp ukázce na Githubu]: https://github.com/azure/azure-mobile-apps-node/tree/master/samples/basic-app
[todo ukázce na Githubu]: https://github.com/azure/azure-mobile-apps-node/tree/master/samples/todo
[ukázky adresáře na Githubu]: https://github.com/azure/azure-mobile-apps-node/tree/master/samples
[static-schema sample on GitHub]: https://github.com/azure/azure-mobile-apps-node/tree/master/samples/static-schema
[QueryJS]: https://github.com/Azure/queryjs
[Node.js Tools 1.1 pro sadu Visual Studio]: https://github.com/Microsoft/nodejstools/releases/tag/v1.1-RC.2.1
[mssql Node.js balíček]: https://www.npmjs.com/package/mssql
[Microsoft SQL Server 2014 Express]: http://www.microsoft.com/en-us/server-cloud/Products/sql-server-editions/sql-server-express.aspx
[ExpressJS Middleware]: http://expressjs.com/guide/using-middleware.html
[Winstona]: https://github.com/winstonjs/winston
