---
title: Jak pracovat se serverem back-end Node.js SDK pro Mobile Apps | Microsoft Docs
description: "Naučte se pracovat se serverem back-end Node.js SDK pro Azure App Service Mobile Apps."
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
ms.openlocfilehash: 1d3aa7a0089279a8eafeb0ded951a5238e189eaa
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="how-to-use-the-azure-mobile-apps-nodejs-sdk"></a><span data-ttu-id="4e161-103">Jak používat Azure Mobile Apps Node.js SDK</span><span class="sxs-lookup"><span data-stu-id="4e161-103">How to use the Azure Mobile Apps Node.js SDK</span></span>
[!INCLUDE [app-service-mobile-selector-server-sdk](../../includes/app-service-mobile-selector-server-sdk.md)]

<span data-ttu-id="4e161-104">Tento článek obsahuje podrobné informace a příklady znázorňující, jak pracovat s back-end Node.js v Azure App Service Mobile Apps.</span><span class="sxs-lookup"><span data-stu-id="4e161-104">This article provides detailed information and examples showing how to work with a Node.js backend in Azure App Service Mobile Apps.</span></span>

## <span data-ttu-id="4e161-105"><a name="Introduction"></a>Úvod</span><span class="sxs-lookup"><span data-stu-id="4e161-105"><a name="Introduction"></a>Introduction</span></span>
<span data-ttu-id="4e161-106">Azure App Service Mobile Apps poskytuje možnost Přidat mobilní optimalizovaná data přístup webového rozhraní API k webové aplikaci.</span><span class="sxs-lookup"><span data-stu-id="4e161-106">Azure App Service Mobile Apps provides the capability to add a mobile-optimized data access Web API to a web application.</span></span>  <span data-ttu-id="4e161-107">Azure App Service Mobile Apps SDK se poskytuje pro webové aplikace ASP.NET a Node.js.</span><span class="sxs-lookup"><span data-stu-id="4e161-107">The Azure App Service Mobile Apps SDK is provided for ASP.NET and Node.js web applications.</span></span>  <span data-ttu-id="4e161-108">Sada SDK poskytuje následující operace:</span><span class="sxs-lookup"><span data-stu-id="4e161-108">The SDK provides the following operations:</span></span>

* <span data-ttu-id="4e161-109">Operace s tabulkou (pro čtení, příkaz Insert, Update, Delete) pro přístup k datům</span><span class="sxs-lookup"><span data-stu-id="4e161-109">Table operations (Read, Insert, Update, Delete) for data access</span></span>
* <span data-ttu-id="4e161-110">Operace vlastní rozhraní API</span><span class="sxs-lookup"><span data-stu-id="4e161-110">Custom API operations</span></span>

<span data-ttu-id="4e161-111">Obě operace zajištění ověřování napříč všech zprostředkovatelů identity povolené ve službě Azure App Service, včetně poskytovatelů sociálních identit jako je Facebook, Twitter, Google a Microsoft a také Azure Active Directory pro podnikové identity.</span><span class="sxs-lookup"><span data-stu-id="4e161-111">Both operations provide for authentication across all identity providers allowed by Azure App Service, including social identity providers such as Facebook, Twitter, Google and Microsoft as well as Azure Active Directory for enterprise identity.</span></span>

<span data-ttu-id="4e161-112">Můžete najít ukázky pro každý případ použití v [ukázky adresáře na Githubu].</span><span class="sxs-lookup"><span data-stu-id="4e161-112">You can find samples for each use case in the [samples directory on GitHub].</span></span>

## <a name="supported-platforms"></a><span data-ttu-id="4e161-113">Podporované platformy</span><span class="sxs-lookup"><span data-stu-id="4e161-113">Supported Platforms</span></span>
<span data-ttu-id="4e161-114">Azure Mobile Apps uzlu SDK podporuje aktuální verzi LTS uzlu a novější.</span><span class="sxs-lookup"><span data-stu-id="4e161-114">The Azure Mobile Apps Node SDK supports the current LTS release of Node and later.</span></span>  <span data-ttu-id="4e161-115">Od verze zápis, je na nejnovější verzi LTS v4.5.0 uzlu.</span><span class="sxs-lookup"><span data-stu-id="4e161-115">As of writing, the latest LTS version is Node v4.5.0.</span></span>  <span data-ttu-id="4e161-116">Jiné verze uzlu může fungovat, ale nejsou podporovány.</span><span class="sxs-lookup"><span data-stu-id="4e161-116">Other versions of Node may work, but are not supported.</span></span>

<span data-ttu-id="4e161-117">Azure Mobile Apps uzlu SDK podporuje dvě databáze ovladače – ovladač uzlu mssql podporuje SQL Azure a místní instance systému SQL Server.</span><span class="sxs-lookup"><span data-stu-id="4e161-117">The Azure Mobile Apps Node SDK supports two database drivers - the node-mssql driver supports SQL Azure and local SQL Server instances.</span></span>  <span data-ttu-id="4e161-118">Ovladač sqlite3 podporuje pouze do jedné instance databáze SQLite.</span><span class="sxs-lookup"><span data-stu-id="4e161-118">The sqlite3 driver supports SQLite databases on a single instance only.</span></span>

### <span data-ttu-id="4e161-119"><a name="howto-cmdline-basicapp"></a>Postupy: vytvoření základní Node.js back-end pomocí příkazového řádku</span><span class="sxs-lookup"><span data-stu-id="4e161-119"><a name="howto-cmdline-basicapp"></a>How to: Create a Basic Node.js backend using the Command Line</span></span>
<span data-ttu-id="4e161-120">Všechny back-end Azure App Service Mobile aplikace Node.js se spustí jako ExpressJS aplikace.</span><span class="sxs-lookup"><span data-stu-id="4e161-120">Every Azure App Service Mobile App Node.js backend starts as an ExpressJS application.</span></span>  <span data-ttu-id="4e161-121">ExpressJS je nejoblíbenější webové služby rozhraní k dispozici pro Node.js.</span><span class="sxs-lookup"><span data-stu-id="4e161-121">ExpressJS is the most popular web service framework available for Node.js.</span></span>  <span data-ttu-id="4e161-122">Můžete vytvořit základní [Express] aplikace následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="4e161-122">You can create a basic [Express] application as follows:</span></span>

1. <span data-ttu-id="4e161-123">V příkazu nebo v okně prostředí PowerShell vytvořte adresář pro váš projekt.</span><span class="sxs-lookup"><span data-stu-id="4e161-123">In a command or PowerShell window, create a directory for your project.</span></span>

        mkdir basicapp
2. <span data-ttu-id="4e161-124">Spusťte npm init inicializovat struktura balíčku.</span><span class="sxs-lookup"><span data-stu-id="4e161-124">Run npm init to initialize the package structure.</span></span>

        cd basicapp
        npm init

    <span data-ttu-id="4e161-125">Příkaz init npm požádá sadu otázky k chybě při inicializaci projektu.</span><span class="sxs-lookup"><span data-stu-id="4e161-125">The npm init command asks a set of questions to initialize the project.</span></span>  <span data-ttu-id="4e161-126">Najdete příklad výstupu:</span><span class="sxs-lookup"><span data-stu-id="4e161-126">See the example output:</span></span>

    ![Init – výstup npm][0]
3. <span data-ttu-id="4e161-128">Z úložiště npm nainstalujte knihovny express a aplikace azure mobile.</span><span class="sxs-lookup"><span data-stu-id="4e161-128">Install the express and azure-mobile-apps libraries from the npm repository.</span></span>

        npm install --save express azure-mobile-apps
4. <span data-ttu-id="4e161-129">Vytvoření souboru app.js k implementaci základní mobilní serveru.</span><span class="sxs-lookup"><span data-stu-id="4e161-129">Create an app.js file to implement the basic mobile server.</span></span>

        var express = require('express'),
            azureMobileApps = require('azure-mobile-apps');

        var app = express(),
            mobile = azureMobileApps();

        // Define a TodoItem table
        mobile.tables.add('TodoItem');

        // Add the mobile API so it is accessible as a Web API
        app.use(mobile);

        // Start listening on HTTP
        app.listen(process.env.PORT || 3000);

<span data-ttu-id="4e161-130">Tato aplikace vytvoří WebAPI se mobile optimalizované s jeden koncový bod (`/tables/TodoItem`), který poskytuje přístup bez ověřování pro základní úložiště dat SQL pomocí dynamické schématu.</span><span class="sxs-lookup"><span data-stu-id="4e161-130">This application creates a mobile-optimized WebAPI with a single endpoint (`/tables/TodoItem`) that provides unauthenticated access to an underlying SQL data store using a dynamic schema.</span></span>  <span data-ttu-id="4e161-131">Je vhodné pro následující rychlé spuštění knihovny klienta:</span><span class="sxs-lookup"><span data-stu-id="4e161-131">It is suitable for following the client library quick starts:</span></span>

* <span data-ttu-id="4e161-132">[Rychlý start Android klienta]</span><span class="sxs-lookup"><span data-stu-id="4e161-132">[Android Client QuickStart]</span></span>
* <span data-ttu-id="4e161-133">[Rychlý start Apache Cordova klienta]</span><span class="sxs-lookup"><span data-stu-id="4e161-133">[Apache Cordova Client QuickStart]</span></span>
* <span data-ttu-id="4e161-134">[iOS klienta rychlý start]</span><span class="sxs-lookup"><span data-stu-id="4e161-134">[iOS Client QuickStart]</span></span>
* <span data-ttu-id="4e161-135">[Rychlé spuštění klienta Windows Store]</span><span class="sxs-lookup"><span data-stu-id="4e161-135">[Windows Store Client QuickStart]</span></span>
* <span data-ttu-id="4e161-136">[Rychlý start Xamarin.iOS klienta]</span><span class="sxs-lookup"><span data-stu-id="4e161-136">[Xamarin.iOS Client QuickStart]</span></span>
* <span data-ttu-id="4e161-137">[Rychlé spuštění klienta Xamarin.Android]</span><span class="sxs-lookup"><span data-stu-id="4e161-137">[Xamarin.Android Client QuickStart]</span></span>
* <span data-ttu-id="4e161-138">[Rychlý start Xamarin.Forms klienta]</span><span class="sxs-lookup"><span data-stu-id="4e161-138">[Xamarin.Forms Client QuickStart]</span></span>

<span data-ttu-id="4e161-139">Můžete najít kód pro tuto základní aplikaci v [basicapp ukázce na Githubu].</span><span class="sxs-lookup"><span data-stu-id="4e161-139">You can find the code for this basic application in the [basicapp sample on GitHub].</span></span>

### <span data-ttu-id="4e161-140"><a name="howto-vs2015-basicapp"></a>Postupy: vytvoření back-end uzlu pomocí sady Visual Studio 2015</span><span class="sxs-lookup"><span data-stu-id="4e161-140"><a name="howto-vs2015-basicapp"></a>How to: Create a Node backend with Visual Studio 2015</span></span>
<span data-ttu-id="4e161-141">Visual Studio 2015 vyžaduje rozšíření při vývoji aplikací Node.js v prostředí IDE.</span><span class="sxs-lookup"><span data-stu-id="4e161-141">Visual Studio 2015 requires an extension to develop Node.js applications within the IDE.</span></span>  <span data-ttu-id="4e161-142">Chcete-li začít, nainstalujte [Node.js Tools 1.1 pro sadu Visual Studio].</span><span class="sxs-lookup"><span data-stu-id="4e161-142">To start, install the [Node.js Tools 1.1 for Visual Studio].</span></span>  <span data-ttu-id="4e161-143">Po instalaci nástrojů pro Node.js pro sadu Visual Studio vytvořte aplikaci 4.x Express:</span><span class="sxs-lookup"><span data-stu-id="4e161-143">Once the Node.js Tools for Visual Studio are installed, create an Express 4.x application:</span></span>

1. <span data-ttu-id="4e161-144">Otevřete **nový projekt** dialogové okno (z **soubor** > **nový** > **projekt...** ).</span><span class="sxs-lookup"><span data-stu-id="4e161-144">Open the **New Project** dialog (from **File** > **New** > **Project...**).</span></span>
2. <span data-ttu-id="4e161-145">Rozbalte položku **šablony** > **JavaScript** > **Node.js**.</span><span class="sxs-lookup"><span data-stu-id="4e161-145">Expand **Templates** > **JavaScript** > **Node.js**.</span></span>
3. <span data-ttu-id="4e161-146">Vyberte **aplikace základní Azure Node.js Express 4**.</span><span class="sxs-lookup"><span data-stu-id="4e161-146">Select the **Basic Azure Node.js Express 4 Application**.</span></span>
4. <span data-ttu-id="4e161-147">Zadejte název projektu.</span><span class="sxs-lookup"><span data-stu-id="4e161-147">Fill in the project name.</span></span>  <span data-ttu-id="4e161-148">Klikněte na tlačítko *OK*.</span><span class="sxs-lookup"><span data-stu-id="4e161-148">Click *OK*.</span></span>

    ![Nový projekt sady Visual Studio 2015][1]
5. <span data-ttu-id="4e161-150">Klikněte pravým tlačítkem myši **npm** uzel a vyberte možnost **nainstalovat nové balíčky npm...** .</span><span class="sxs-lookup"><span data-stu-id="4e161-150">Right-click the **npm** node and select **Install New npm packages...**.</span></span>
6. <span data-ttu-id="4e161-151">Musíte aktualizovat katalog npm na vytvoření vaší první aplikace Node.js.</span><span class="sxs-lookup"><span data-stu-id="4e161-151">You may need to refresh the npm catalog on creating your first Node.js application.</span></span>  <span data-ttu-id="4e161-152">Klikněte na tlačítko **aktualizovat** v případě potřeby.</span><span class="sxs-lookup"><span data-stu-id="4e161-152">Click **Refresh** if necessary.</span></span>
7. <span data-ttu-id="4e161-153">Zadejte *aplikace azure mobile* do vyhledávacího pole.</span><span class="sxs-lookup"><span data-stu-id="4e161-153">Enter *azure-mobile-apps* in the search box.</span></span>  <span data-ttu-id="4e161-154">Klikněte **azure mobile apps 2.0.0** balíček a potom klikněte na **instalovat balíček**.</span><span class="sxs-lookup"><span data-stu-id="4e161-154">Click the **azure-mobile-apps 2.0.0** package, then click **Install Package**.</span></span>

    ![Instalace nové balíčky npm][2]
8. <span data-ttu-id="4e161-156">Klikněte na **Zavřít**.</span><span class="sxs-lookup"><span data-stu-id="4e161-156">Click **Close**.</span></span>
9. <span data-ttu-id="4e161-157">Otevřete *app.js* souboru přidání podpory pro Azure Mobile Apps SDK.</span><span class="sxs-lookup"><span data-stu-id="4e161-157">Open the *app.js* file to add support for the Azure Mobile Apps SDK.</span></span>  <span data-ttu-id="4e161-158">Na řádku 6 at dolní knihovny vyžadují příkazy, přidejte následující kód:</span><span class="sxs-lookup"><span data-stu-id="4e161-158">At line 6 at the bottom of the library require statements, add the following code:</span></span>

        var bodyParser = require('body-parser');
        var azureMobileApps = require('azure-mobile-apps');

    <span data-ttu-id="4e161-159">Na přibližně řádku 27 po další app.use příkazy přidejte následující kód:</span><span class="sxs-lookup"><span data-stu-id="4e161-159">At approximately line 27 after the other app.use statements, add the following code:</span></span>

        app.use('/users', users);

        // Azure Mobile Apps Initialization
        var mobile = azureMobileApps();
        mobile.tables.add('TodoItem');
        app.use(mobile);

    <span data-ttu-id="4e161-160">Uložte soubor.</span><span class="sxs-lookup"><span data-stu-id="4e161-160">Save the file.</span></span>
10. <span data-ttu-id="4e161-161">Spusťte aplikaci místně (rozhraní API je zpracování na http://localhost: 3000) nebo publikování v Azure.</span><span class="sxs-lookup"><span data-stu-id="4e161-161">Either run the application locally (the API is served on http://localhost:3000) or publish to Azure.</span></span>

### <span data-ttu-id="4e161-162"><a name="create-node-backend-portal"></a>Postupy: vytvoření back-end Node.js, pomocí portálu Azure</span><span class="sxs-lookup"><span data-stu-id="4e161-162"><a name="create-node-backend-portal"></a>How to: Create a Node.js backend using the Azure portal</span></span>
<span data-ttu-id="4e161-163">Můžete vytvořit právo back-end mobilní aplikace v [portál Azure].</span><span class="sxs-lookup"><span data-stu-id="4e161-163">You can create a Mobile App backend right in the [Azure portal].</span></span> <span data-ttu-id="4e161-164">Můžete buď postupujte podle následujících kroků nebo vytvořit pomocí následujících klientských a serverových společně [vytvoření mobilní aplikace](app-service-mobile-ios-get-started.md) kurzu.</span><span class="sxs-lookup"><span data-stu-id="4e161-164">You can either follow the following steps or create a client and server together by following the [Create a mobile app](app-service-mobile-ios-get-started.md) tutorial.</span></span> <span data-ttu-id="4e161-165">Tento kurz obsahuje zjednodušenou verzi tyto pokyny a je nejvhodnější pro ověření projekty na konceptu.</span><span class="sxs-lookup"><span data-stu-id="4e161-165">The tutorial contains a simplified version of these instructions and is best for proof of concept projects.</span></span>

[!INCLUDE [app-service-mobile-dotnet-backend-create-new-service-classic](../../includes/app-service-mobile-dotnet-backend-create-new-service-classic.md)]

<span data-ttu-id="4e161-166">Zpět v *Začínáme* okno, v části **vytvořit tabulku rozhraní API**, zvolte **Node.js** jako vaše **back-end jazyk**.</span><span class="sxs-lookup"><span data-stu-id="4e161-166">Back in the *Get started* blade, under **Create a table API**, choose **Node.js** as your **Backend language**.</span></span>
<span data-ttu-id="4e161-167">Zaškrtněte políčko pro "**beru na vědomí, že tato akce přepíše všechny lokality obsahu.**", pak klikněte na tlačítko **vytvořit tabulku TodoItem**.</span><span class="sxs-lookup"><span data-stu-id="4e161-167">Check the box for "**I acknowledge that this will overwrite all site contents.**", then click **Create TodoItem table**.</span></span>

### <span data-ttu-id="4e161-168"><a name="download-quickstart"></a>Postupy: stažení projektu pro rychlý start kódu Node.js back-end pomocí Git</span><span class="sxs-lookup"><span data-stu-id="4e161-168"><a name="download-quickstart"></a>How to: Download the Node.js backend quickstart code project using Git</span></span>
<span data-ttu-id="4e161-169">Při vytváření back-end mobilní aplikace Node.js pomocí portálu **úvodní** okně projekt Node.js se vytvoří a nasadí na váš web.</span><span class="sxs-lookup"><span data-stu-id="4e161-169">When you create a Node.js Mobile App backend by using the portal **Quick start** blade, a Node.js project is created for you and deployed to your site.</span></span> <span data-ttu-id="4e161-170">Můžete přidat tabulky a rozhraní API a upravit soubory kódu pro back-end Node.js na portálu.</span><span class="sxs-lookup"><span data-stu-id="4e161-170">You can add tables and APIs and edit code files for the Node.js backend in the portal.</span></span> <span data-ttu-id="4e161-171">Různé nástroje pro nasazení můžete také stáhnout projektu back-end, můžete přidat nebo upravit tabulek a rozhraní API a pak znovu publikovat projektu.</span><span class="sxs-lookup"><span data-stu-id="4e161-171">You can also use various deployment tools to download the backend project so that you can add or modify tables and APIs, then republish the project.</span></span> <span data-ttu-id="4e161-172">Další informace najdete v tématu [Příručka pro nasazení Azure App Service].</span><span class="sxs-lookup"><span data-stu-id="4e161-172">For more information, see the [Azure App Service Deployment Guide].</span></span> <span data-ttu-id="4e161-173">Následující postup používá úložiště Git ke stažení rychlé spuštění kódu projektu.</span><span class="sxs-lookup"><span data-stu-id="4e161-173">the following procedure uses a Git repository to download the quickstart project code.</span></span>

1. <span data-ttu-id="4e161-174">Pokud jste tak již neučinili, nainstalujte Git.</span><span class="sxs-lookup"><span data-stu-id="4e161-174">Install Git, if you haven't already done so.</span></span> <span data-ttu-id="4e161-175">Kroky potřebné k instalaci Git lišit podle operačních systémů.</span><span class="sxs-lookup"><span data-stu-id="4e161-175">The steps required to install Git vary between operating systems.</span></span> <span data-ttu-id="4e161-176">V tématu [instalace Git](http://git-scm.com/book/en/Getting-Started-Installing-Git) distribuce specifické pro operační systém a instalaci.</span><span class="sxs-lookup"><span data-stu-id="4e161-176">See [Installing Git](http://git-scm.com/book/en/Getting-Started-Installing-Git) for operating system-specific distributions and installation guidance.</span></span>
2. <span data-ttu-id="4e161-177">Postupujte podle kroků v [povolit úložišti aplikace služby App Service](../app-service-web/app-service-deploy-local-git.md#Step3) povolit úložiště Git pro váš web back-end, proto poznámku o nasazení uživatelské jméno a heslo.</span><span class="sxs-lookup"><span data-stu-id="4e161-177">Follow the steps in [Enable the App Service app repository](../app-service-web/app-service-deploy-local-git.md#Step3) to enable the Git repository for your backend site, making a note of the deployment username and password.</span></span>
3. <span data-ttu-id="4e161-178">V okně pro váš back-end mobilní aplikace, poznamenejte si **adresy URL pro klon Git** nastavení.</span><span class="sxs-lookup"><span data-stu-id="4e161-178">In the blade for your Mobile App backend, make a note of the **Git clone URL** setting.</span></span>
4. <span data-ttu-id="4e161-179">Spuštění `git clone` příkaz pomocí Gitu klonovat adresu URL, zadat heslo, pokud jsou povinné, jako v následujícím příkladu:</span><span class="sxs-lookup"><span data-stu-id="4e161-179">Execute the `git clone` command using the Git clone URL, entering your password when required, as in the following example:</span></span>

        $ git clone https://username@todolist.scm.azurewebsites.net:443/todolist.git
5. <span data-ttu-id="4e161-180">Přejděte do místního adresáře, který v předchozím příkladu je /todolist a Všimněte si, že byly staženy soubory projektu.</span><span class="sxs-lookup"><span data-stu-id="4e161-180">Browse to local directory, which in the preceding example is /todolist, and notice that project files have been downloaded.</span></span> <span data-ttu-id="4e161-181">Vyhledejte `todoitem.json` v soubor `/tables` adresáře.</span><span class="sxs-lookup"><span data-stu-id="4e161-181">Locate the `todoitem.json` file in the `/tables` directory.</span></span>  <span data-ttu-id="4e161-182">Tento soubor definuje oprávnění v tabulce.</span><span class="sxs-lookup"><span data-stu-id="4e161-182">This file defines permissions on the table.</span></span>  <span data-ttu-id="4e161-183">Také umožňuje vyhledat `todoitem.js` souboru ve stejném adresáři, který definuje této operace CRUD skriptů pro tabulku.</span><span class="sxs-lookup"><span data-stu-id="4e161-183">Also find the `todoitem.js` file in the same directory, which defines that CRUD operation scripts for the table.</span></span>
6. <span data-ttu-id="4e161-184">Po provedení změn do souborů projektu, spusťte následující příkaz pro přidání, potvrzení a pak odeslat změny na web:</span><span class="sxs-lookup"><span data-stu-id="4e161-184">After you have made changes to project files, execute the following commands to add, commit, then upload the changes to the site:</span></span>

        $ git commit -m "updated the table script"
        $ git push origin master

    <span data-ttu-id="4e161-185">Když přidáte nové soubory do projektu, musíte nejdřív provést `git add .` příkaz.</span><span class="sxs-lookup"><span data-stu-id="4e161-185">When you add new files to the project, you first need to execute the `git add .` command.</span></span>

<span data-ttu-id="4e161-186">Pokaždé, když novou sadu potvrzení vložena do lokality, je znovu publikovat web.</span><span class="sxs-lookup"><span data-stu-id="4e161-186">The site is republished every time a new set of commits is pushed to the site.</span></span>

### <span data-ttu-id="4e161-187"><a name="howto-publish-to-azure"></a>Postupy: publikování back-end Node.js v Azure</span><span class="sxs-lookup"><span data-stu-id="4e161-187"><a name="howto-publish-to-azure"></a>How to: Publish your Node.js backend to Azure</span></span>
<span data-ttu-id="4e161-188">Microsoft Azure poskytuje mnoho mechanismy pro publikování váš back-end Azure App Service Mobile aplikace Node.js ve službě Azure.</span><span class="sxs-lookup"><span data-stu-id="4e161-188">Microsoft Azure provides many mechanisms for publishing your Azure App Service Mobile Apps Node.js backend to the Azure service.</span></span>  <span data-ttu-id="4e161-189">Jedná se o použití nástroje pro nasazení, které jsou integrované do sady Visual Studio, nástroje příkazového řádku a průběžné nasazování pro různé zdrojového kódu.</span><span class="sxs-lookup"><span data-stu-id="4e161-189">These include utilizing deployment tools integrated into Visual Studio, command-line tools, and continuous deployment options based on source control.</span></span>  <span data-ttu-id="4e161-190">Další informace v tomto tématu najdete v tématu [Příručka pro nasazení Azure App Service].</span><span class="sxs-lookup"><span data-stu-id="4e161-190">For more information on this topic, see the [Azure App Service Deployment Guide].</span></span>

<span data-ttu-id="4e161-191">Aplikační služba Azure má konkrétní Rady, jak aplikaci Node.js, která si měli projít před nasazením:</span><span class="sxs-lookup"><span data-stu-id="4e161-191">Azure App Service has specific advice for Node.js application that you should review before deploying:</span></span>

* <span data-ttu-id="4e161-192">Postup [zadejte verzi uzlu]</span><span class="sxs-lookup"><span data-stu-id="4e161-192">How to [specify the Node Version]</span></span>
* <span data-ttu-id="4e161-193">Postup [použijte moduly uzlu]</span><span class="sxs-lookup"><span data-stu-id="4e161-193">How to [use Node modules]</span></span>

### <span data-ttu-id="4e161-194"><a name="howto-enable-homepage"></a>Postupy: povolení domovskou stránku pro aplikaci</span><span class="sxs-lookup"><span data-stu-id="4e161-194"><a name="howto-enable-homepage"></a>How to: Enable a Home Page for your application</span></span>
<span data-ttu-id="4e161-195">Mnoho aplikací jsou kombinací webové a mobilní aplikace a rozhraní ExpressJS umožňuje zkombinovat dva omezující vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="4e161-195">Many applications are a combination of web and mobile apps and the ExpressJS framework allows you to combine the two facets.</span></span>  <span data-ttu-id="4e161-196">V některých případech ale můžete chtít pouze mobilní rozhraní implementovat.</span><span class="sxs-lookup"><span data-stu-id="4e161-196">Sometimes, however, you may wish to only implement a mobile interface.</span></span>  <span data-ttu-id="4e161-197">Je vhodné zajistit, že cílová stránka zajistit app service je spuštěná.</span><span class="sxs-lookup"><span data-stu-id="4e161-197">It is useful to provide a landing page to ensure the app service is up and running.</span></span>  <span data-ttu-id="4e161-198">Můžete zadat vlastní domovské stránky, nebo povolit dočasné domovskou stránku.</span><span class="sxs-lookup"><span data-stu-id="4e161-198">You can either provide your own home page or enable a temporary home page.</span></span>  <span data-ttu-id="4e161-199">Pokud chcete povolit dočasné domovskou stránku, použijte následující postupy k vytváření instancí Azure Mobile Apps:</span><span class="sxs-lookup"><span data-stu-id="4e161-199">To enable a temporary home page, use the following to instantiate Azure Mobile Apps:</span></span>

    var mobile = azureMobileApps({ homePage: true });

<span data-ttu-id="4e161-200">Pokud chcete pouze tuto možnost k dispozici při vývoji místně, můžete přidat toto nastavení vaší `azureMobile.js` souboru.</span><span class="sxs-lookup"><span data-stu-id="4e161-200">If you only want this option available when developing locally, you can add this setting to your `azureMobile.js` file.</span></span>

## <span data-ttu-id="4e161-201"><a name="TableOperations"></a>Operace s tabulkou</span><span class="sxs-lookup"><span data-stu-id="4e161-201"><a name="TableOperations"></a>Table operations</span></span>
<span data-ttu-id="4e161-202">Server SDK pro Node.js aplikace azure mobile poskytuje mechanismus ke zveřejnění tabulek dat uložených ve službě Azure SQL Database jako WebAPI.</span><span class="sxs-lookup"><span data-stu-id="4e161-202">The azure-mobile-apps Node.js Server SDK provides mechanisms to expose data tables stored in Azure SQL Database as a WebAPI.</span></span>  <span data-ttu-id="4e161-203">Jsou k dispozici pět operace.</span><span class="sxs-lookup"><span data-stu-id="4e161-203">Five operations are provided.</span></span>

| <span data-ttu-id="4e161-204">Operace</span><span class="sxs-lookup"><span data-stu-id="4e161-204">Operation</span></span> | <span data-ttu-id="4e161-205">Popis</span><span class="sxs-lookup"><span data-stu-id="4e161-205">Description</span></span> |
| --- | --- |
| <span data-ttu-id="4e161-206">GET /tables/*tablename*</span><span class="sxs-lookup"><span data-stu-id="4e161-206">GET /tables/*tablename*</span></span> |<span data-ttu-id="4e161-207">Získat všechny záznamy v tabulce</span><span class="sxs-lookup"><span data-stu-id="4e161-207">Get all records in the table</span></span> |
| <span data-ttu-id="4e161-208">GET /tables/*tablename*/:id</span><span class="sxs-lookup"><span data-stu-id="4e161-208">GET /tables/*tablename*/:id</span></span> |<span data-ttu-id="4e161-209">Získejte konkrétní záznam v tabulce</span><span class="sxs-lookup"><span data-stu-id="4e161-209">Get a specific record in the table</span></span> |
| <span data-ttu-id="4e161-210">POST /tables/*tablename*</span><span class="sxs-lookup"><span data-stu-id="4e161-210">POST /tables/*tablename*</span></span> |<span data-ttu-id="4e161-211">Vytvořit záznam v tabulce</span><span class="sxs-lookup"><span data-stu-id="4e161-211">Create a record in the table</span></span> |
| <span data-ttu-id="4e161-212">Oprava /tables/*tablename*/:id</span><span class="sxs-lookup"><span data-stu-id="4e161-212">PATCH /tables/*tablename*/:id</span></span> |<span data-ttu-id="4e161-213">Aktualizace záznamu v tabulce</span><span class="sxs-lookup"><span data-stu-id="4e161-213">Update a record in the table</span></span> |
| <span data-ttu-id="4e161-214">ODSTRANĚNÍ /tables/*tablename*/:id</span><span class="sxs-lookup"><span data-stu-id="4e161-214">DELETE /tables/*tablename*/:id</span></span> |<span data-ttu-id="4e161-215">Odstranit záznam v tabulce</span><span class="sxs-lookup"><span data-stu-id="4e161-215">Delete a record in the table</span></span> |

<span data-ttu-id="4e161-216">Podporuje tato WebAPI [OData] a rozšiřuje schéma tabulky pro podporu [synchronizaci dat offline].</span><span class="sxs-lookup"><span data-stu-id="4e161-216">This WebAPI supports [OData] and extends the table schema to support [offline data sync].</span></span>

### <span data-ttu-id="4e161-217"><a name="howto-dynamicschema"></a>Postupy: definování tabulky pomocí dynamické schématu</span><span class="sxs-lookup"><span data-stu-id="4e161-217"><a name="howto-dynamicschema"></a>How to: Define tables using a dynamic schema</span></span>
<span data-ttu-id="4e161-218">Před použitím tabulky, musí být definovaný.</span><span class="sxs-lookup"><span data-stu-id="4e161-218">Before a table can be used, it must be defined.</span></span>  <span data-ttu-id="4e161-219">Tabulky lze definovat pomocí statické schématu (kde vývojář definuje sloupců ve schématu) nebo dynamicky (kde sady SDK řídí schéma založené na příchozí požadavky).</span><span class="sxs-lookup"><span data-stu-id="4e161-219">Tables can be defined with a static schema (where the developer defines the columns within the schema) or dynamically (where the SDK controls the schema based on incoming requests).</span></span> <span data-ttu-id="4e161-220">Kromě toho můžete řídit vývojář konkrétních aspektů WebAPI přidáním kódu jazyka Javascript do definice.</span><span class="sxs-lookup"><span data-stu-id="4e161-220">In addition, the developer can control specific aspects of the WebAPI by adding Javascript code to the definition.</span></span>

<span data-ttu-id="4e161-221">Jako osvědčený postup musí definovat každá tabulka v souboru jazyka Javascript v adresáři tabulky a potom použít metodu tables.import() importovat tabulky.</span><span class="sxs-lookup"><span data-stu-id="4e161-221">As a best practice, you should define each table in a Javascript file in the tables directory, then use the tables.import() method to import the tables.</span></span>  <span data-ttu-id="4e161-222">Rozšíření aplikace basic, souboru app.js se upraví:</span><span class="sxs-lookup"><span data-stu-id="4e161-222">Extending the basic-app, the app.js file would be adjusted:</span></span>

    var express = require('express'),
        azureMobileApps = require('azure-mobile-apps');

    var app = express(),
        mobile = azureMobileApps();

    // Define the database schema that is exposed
    mobile.tables.import('./tables');

    // Provide initialization of any tables that are statically defined
    mobile.tables.initialize().then(function () {
        // Add the mobile API so it is accessible as a Web API
        app.use(mobile);

        // Start listening on HTTP
        app.listen(process.env.PORT || 3000);
    });

<span data-ttu-id="4e161-223">Definice tabulky. / tables/TodoItem.js:</span><span class="sxs-lookup"><span data-stu-id="4e161-223">Define the table in ./tables/TodoItem.js:</span></span>

    var azureMobileApps = require('azure-mobile-apps');

    var table = azureMobileApps.table();

    // Additional configuration for the table goes here

    module.exports = table;

<span data-ttu-id="4e161-224">Tabulky ve výchozím nastavení používá dynamické schématu.</span><span class="sxs-lookup"><span data-stu-id="4e161-224">Tables use dynamic schema by default.</span></span>  <span data-ttu-id="4e161-225">Chcete-li vypnout dynamické schématu globálně, nastavte nastavení aplikace **MS_DynamicSchema** má hodnotu false v rámci portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="4e161-225">To turn off dynamic schema globally, set the App Setting **MS_DynamicSchema** to false within the Azure portal.</span></span>

<span data-ttu-id="4e161-226">Kompletní příklad v můžete najít [todo ukázce na Githubu].</span><span class="sxs-lookup"><span data-stu-id="4e161-226">You can find a complete example in the [todo sample on GitHub].</span></span>

### <span data-ttu-id="4e161-227"><a name="howto-staticschema"></a>Postupy: definování tabulky pomocí statické schématu</span><span class="sxs-lookup"><span data-stu-id="4e161-227"><a name="howto-staticschema"></a>How to: Define tables using a static schema</span></span>
<span data-ttu-id="4e161-228">Můžete definovat explicitně sloupce, které chcete zpřístupnit prostřednictvím WebAPI.</span><span class="sxs-lookup"><span data-stu-id="4e161-228">You can explicitly define the columns to expose via the WebAPI.</span></span>  <span data-ttu-id="4e161-229">Aplikace azure mobile Node.js SDK automaticky přidá žádné další sloupce požadované pro synchronizaci dat offline do seznamu, který zadáte.</span><span class="sxs-lookup"><span data-stu-id="4e161-229">The azure-mobile-apps Node.js SDK automatically adds any additional columns required for offline data sync to the list that you provide.</span></span>  <span data-ttu-id="4e161-230">Například klientské aplikace rychlý start vyžadovat tabulku s dva sloupce: text (řetězec) a dokončete (boolean).</span><span class="sxs-lookup"><span data-stu-id="4e161-230">For example, the QuickStart client applications require a table with two columns: text (a string) and complete (a boolean).</span></span>  
<span data-ttu-id="4e161-231">Tabulka je možné definovat v tabulce JavaScript souboru definice (umístěný v adresáři tabulky) následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="4e161-231">The table can be defined in the table definition JavaScript file (located in the tables directory) as follows:</span></span>

    var azureMobileApps = require('azure-mobile-apps');

    var table = azureMobileApps.table();

    // Define the columns within the table
    table.columns = {
        "text": "string",
        "complete": "boolean"
    };

    // Turn off dynamic schema
    table.dynamicSchema = false;

    module.exports = table;

<span data-ttu-id="4e161-232">Pokud definujete tabulky staticky, musíte také zavolat metodu tables.initialize() vytvořit schéma databáze při spuštění.</span><span class="sxs-lookup"><span data-stu-id="4e161-232">If you define tables statically, then you must also call the tables.initialize() method to create the database schema on startup.</span></span>  <span data-ttu-id="4e161-233">Vrátí metodu tables.initialize() [Promise] tak, aby webové služby neobsluhuje žádosti před databáze během inicializace.</span><span class="sxs-lookup"><span data-stu-id="4e161-233">The tables.initialize() method returns a [Promise] so that the web service does not serve requests before the database being initialized.</span></span>

### <span data-ttu-id="4e161-234"><a name="howto-sqlexpress-setup"></a>Postupy: použití SQL Express jako úložiště dat vývoj v místním počítači</span><span class="sxs-lookup"><span data-stu-id="4e161-234"><a name="howto-sqlexpress-setup"></a>How to: Use SQL Express as a development data store on your local machine</span></span>
<span data-ttu-id="4e161-235">Azure Mobile Apps AzureMobile aplikace uzlu SDK poskytuje tři možnosti obsluhující data z pole: Sada SDK poskytuje tři možnosti obsluhující data z pole:</span><span class="sxs-lookup"><span data-stu-id="4e161-235">The Azure Mobile Apps The AzureMobile Apps Node SDK provides three options for serving data out of the box: SDK provides three options for serving data out of the box:</span></span>

* <span data-ttu-id="4e161-236">Použití **paměti** ovladače k poskytování úložiště dočasnou příklad</span><span class="sxs-lookup"><span data-stu-id="4e161-236">Use the **memory** driver to provide a non-persistent example store</span></span>
* <span data-ttu-id="4e161-237">Použití **mssql** ovladače k poskytování úložiště dat SQL Express pro vývoj</span><span class="sxs-lookup"><span data-stu-id="4e161-237">Use the **mssql** driver to provide a SQL Express data store for development</span></span>
* <span data-ttu-id="4e161-238">Použití **mssql** ovladače k poskytování úložiště dat služby Azure SQL Database pro produkční prostředí</span><span class="sxs-lookup"><span data-stu-id="4e161-238">Use the **mssql** driver to provide an Azure SQL Database data store for production</span></span>

<span data-ttu-id="4e161-239">Azure Mobile Apps Node.js SDK používá [mssql Node.js balíček] vytvořit a používat připojení k SQL Express a SQL Database.</span><span class="sxs-lookup"><span data-stu-id="4e161-239">The Azure Mobile Apps Node.js SDK uses the [mssql Node.js package] to establish and use a connection to both SQL Express and SQL Database.</span></span>  <span data-ttu-id="4e161-240">Tento balíček vyžaduje, abyste povolili připojení TCP ve vaší instanci SQL Express.</span><span class="sxs-lookup"><span data-stu-id="4e161-240">This package requires that you enable TCP connections on your SQL Express instance.</span></span>

> [!TIP]
> <span data-ttu-id="4e161-241">Ovladač paměti neposkytuje kompletní sadu zařízení pro testování.</span><span class="sxs-lookup"><span data-stu-id="4e161-241">The memory driver does not provide a complete set of facilities for testing.</span></span>  <span data-ttu-id="4e161-242">Pokud chcete otestovat váš back-end místně, doporučujeme použít úložiště dat SQL Express a ovladač mssql.</span><span class="sxs-lookup"><span data-stu-id="4e161-242">If you wish to test your backend locally, we recommend the use of a SQL Express data store and the mssql driver.</span></span>
>
>

1. <span data-ttu-id="4e161-243">Stáhněte a nainstalujte [Microsoft SQL Server 2014 Express].</span><span class="sxs-lookup"><span data-stu-id="4e161-243">Download and install [Microsoft SQL Server 2014 Express].</span></span>  <span data-ttu-id="4e161-244">Zkontrolujte, zda že nainstalovat systém SQL Server 2014 Express s edicí nástroje.</span><span class="sxs-lookup"><span data-stu-id="4e161-244">Ensure you install the SQL Server 2014 Express with Tools edition.</span></span>  <span data-ttu-id="4e161-245">Pokud požadujete explicitně podpora 64bitových technologií, 32bitová verze spotřebovává méně paměti při spuštění.</span><span class="sxs-lookup"><span data-stu-id="4e161-245">Unless you explicitly require 64-bit support, the 32-bit version consumes less memory when running.</span></span>
2. <span data-ttu-id="4e161-246">Spusťte Správce konfigurace SQL serveru 2014.</span><span class="sxs-lookup"><span data-stu-id="4e161-246">Run the SQL Server 2014 Configuration Manager.</span></span>

   1. <span data-ttu-id="4e161-247">Rozbalte **konfigurace sítě serveru SQL Server** uzel ve stromu levé nabídce.</span><span class="sxs-lookup"><span data-stu-id="4e161-247">Expand the **SQL Server Network Configuration** node in the left-hand tree menu.</span></span>
   2. <span data-ttu-id="4e161-248">Klikněte na tlačítko **protokoly pro funkci SQLEXPRESS**.</span><span class="sxs-lookup"><span data-stu-id="4e161-248">Click **Protocols for SQLEXPRESS**.</span></span>
   3. <span data-ttu-id="4e161-249">Klikněte pravým tlačítkem na **TCP/IP** a vyberte **povolit**.</span><span class="sxs-lookup"><span data-stu-id="4e161-249">Right-click **TCP/IP** and select **Enable**.</span></span>  <span data-ttu-id="4e161-250">Klikněte na tlačítko **OK** v dialogovém okně automaticky otevírané okno.</span><span class="sxs-lookup"><span data-stu-id="4e161-250">Click **OK** in the pop-up dialog.</span></span>
   4. <span data-ttu-id="4e161-251">Klikněte pravým tlačítkem na **TCP/IP** a vyberte **vlastnosti**.</span><span class="sxs-lookup"><span data-stu-id="4e161-251">Right-click **TCP/IP** and select **Properties**.</span></span>
   5. <span data-ttu-id="4e161-252">Klikněte **IP adresy** kartě.</span><span class="sxs-lookup"><span data-stu-id="4e161-252">Click the **IP Addresses** tab.</span></span>
   6. <span data-ttu-id="4e161-253">Najít **IPAll** uzlu.</span><span class="sxs-lookup"><span data-stu-id="4e161-253">Find the **IPAll** node.</span></span>  <span data-ttu-id="4e161-254">V **TCP Port** zadejte **1433**.</span><span class="sxs-lookup"><span data-stu-id="4e161-254">In the **TCP Port** field, enter **1433**.</span></span>

          ![Configure SQL Express for TCP/IP][3]
   7. <span data-ttu-id="4e161-255">Klikněte na **OK**.</span><span class="sxs-lookup"><span data-stu-id="4e161-255">Click **OK**.</span></span>  <span data-ttu-id="4e161-256">Klikněte na tlačítko **OK** v dialogovém okně automaticky otevírané okno.</span><span class="sxs-lookup"><span data-stu-id="4e161-256">Click **OK** in the pop-up dialog.</span></span>
   8. <span data-ttu-id="4e161-257">Klikněte na tlačítko **služby SQL Server** v nabídce levé stromu.</span><span class="sxs-lookup"><span data-stu-id="4e161-257">Click **SQL Server Services** in the left-hand tree menu.</span></span>
   9. <span data-ttu-id="4e161-258">Klikněte pravým tlačítkem na **SQL Server (SQLEXPRESS)** a vyberte **restartovat**</span><span class="sxs-lookup"><span data-stu-id="4e161-258">Right-click **SQL Server (SQLEXPRESS)** and select **Restart**</span></span>
   10. <span data-ttu-id="4e161-259">Zavřete Správci konfigurace systému SQL Server 2014.</span><span class="sxs-lookup"><span data-stu-id="4e161-259">Close the SQL Server 2014 Configuration Manager.</span></span>
3. <span data-ttu-id="4e161-260">Spusťte SQL Server 2014 Management Studio a připojte se k místní instanci SQL Express</span><span class="sxs-lookup"><span data-stu-id="4e161-260">Run the SQL Server 2014 Management Studio and connect to your local SQL Express instance</span></span>

   1. <span data-ttu-id="4e161-261">Klikněte pravým tlačítkem na vaší instance v Průzkumníku objektů a vyberte **vlastnosti**</span><span class="sxs-lookup"><span data-stu-id="4e161-261">Right-click your instance in the Object Explorer and select **Properties**</span></span>
   2. <span data-ttu-id="4e161-262">Vyberte **zabezpečení** stránky.</span><span class="sxs-lookup"><span data-stu-id="4e161-262">Select the **Security** page.</span></span>
   3. <span data-ttu-id="4e161-263">Ujistěte se, **režimu SQL Server a ověřování systému Windows** je vybrána</span><span class="sxs-lookup"><span data-stu-id="4e161-263">Ensure the **SQL Server and Windows Authentication mode** is selected</span></span>
   4. <span data-ttu-id="4e161-264">Klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="4e161-264">Click **OK**</span></span>

          ![Configure SQL Express Authentication][4]
   5. <span data-ttu-id="4e161-265">Rozbalte položku **zabezpečení** > **přihlášení** v Průzkumníku objektů</span><span class="sxs-lookup"><span data-stu-id="4e161-265">Expand **Security** > **Logins** in the Object Explorer</span></span>
   6. <span data-ttu-id="4e161-266">Klikněte pravým tlačítkem na **přihlášení** a vyberte **nové přihlašovací údaje...**</span><span class="sxs-lookup"><span data-stu-id="4e161-266">Right-click **Logins** and select **New Login...**</span></span>
   7. <span data-ttu-id="4e161-267">Zadejte přihlašovací jméno.</span><span class="sxs-lookup"><span data-stu-id="4e161-267">Enter a Login name.</span></span>  <span data-ttu-id="4e161-268">Vyberte **Ověřování SQL Serveru**.</span><span class="sxs-lookup"><span data-stu-id="4e161-268">Select **SQL Server authentication**.</span></span>  <span data-ttu-id="4e161-269">Zadejte heslo a potom zadejte stejné heslo v **potvrzení hesla**.</span><span class="sxs-lookup"><span data-stu-id="4e161-269">Enter a Password, then enter the same password in **Confirm password**.</span></span>  <span data-ttu-id="4e161-270">Heslo musí splňovat požadavky na složitost systému Windows.</span><span class="sxs-lookup"><span data-stu-id="4e161-270">The password must meet Windows complexity requirements.</span></span>
   8. <span data-ttu-id="4e161-271">Klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="4e161-271">Click **OK**</span></span>

          ![Add a new user to SQL Express][5]
   9. <span data-ttu-id="4e161-272">Klikněte pravým tlačítkem na nové přihlášení a vyberte **vlastnosti**</span><span class="sxs-lookup"><span data-stu-id="4e161-272">Right-click your new login and select **Properties**</span></span>
   10. <span data-ttu-id="4e161-273">Vyberte **role serveru** stránky</span><span class="sxs-lookup"><span data-stu-id="4e161-273">Select the **Server Roles** page</span></span>
   11. <span data-ttu-id="4e161-274">Zaškrtněte políčko vedle položky **dbcreator** role serveru</span><span class="sxs-lookup"><span data-stu-id="4e161-274">Check the box next to the **dbcreator** server role</span></span>
   12. <span data-ttu-id="4e161-275">Klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="4e161-275">Click **OK**</span></span>
   13. <span data-ttu-id="4e161-276">Zavřete SQL Management Studio Server 2015</span><span class="sxs-lookup"><span data-stu-id="4e161-276">Close the SQL Server 2015 Management Studio</span></span>

<span data-ttu-id="4e161-277">Zkontrolujte, zda že záznam uživatelské jméno a heslo, které jste vybrali.</span><span class="sxs-lookup"><span data-stu-id="4e161-277">Ensure you record the username and password you selected.</span></span>  <span data-ttu-id="4e161-278">Musíte přiřadit další role serveru nebo oprávnění v závislosti na požadavcích vaší konkrétní databáze.</span><span class="sxs-lookup"><span data-stu-id="4e161-278">You may need to assign additional server roles or permissions depending on your specific database requirements.</span></span>

<span data-ttu-id="4e161-279">Čtení aplikace Node.js **SQLCONNSTR_MS_TableConnectionString** proměnnou prostředí pro připojovací řetězec pro tuto databázi.</span><span class="sxs-lookup"><span data-stu-id="4e161-279">The Node.js application reads the **SQLCONNSTR_MS_TableConnectionString** environment variable for the connection string for this database.</span></span>  <span data-ttu-id="4e161-280">Tuto proměnnou lze nastavit v rámci vašeho prostředí.</span><span class="sxs-lookup"><span data-stu-id="4e161-280">You can set this variable within your environment.</span></span>  <span data-ttu-id="4e161-281">Můžete například použít PowerShell k nastavení této proměnné prostředí:</span><span class="sxs-lookup"><span data-stu-id="4e161-281">For example, you can use PowerShell to set this environment variable:</span></span>

    $env:SQLCONNSTR_MS_TableConnectionString = "Server=127.0.0.1; Database=mytestdatabase; User Id=azuremobile; Password=T3stPa55word;"

<span data-ttu-id="4e161-282">Přístup k databázi přes připojení TCP/IP a zadejte uživatelské jméno a heslo pro připojení.</span><span class="sxs-lookup"><span data-stu-id="4e161-282">Access the database through a TCP/IP connection and provide a username and password for the connection.</span></span>

### <span data-ttu-id="4e161-283"><a name="howto-config-localdev"></a>Postupy: nakonfigurujete svůj projekt pro místní vývoj</span><span class="sxs-lookup"><span data-stu-id="4e161-283"><a name="howto-config-localdev"></a>How to: Configure your project for local development</span></span>
<span data-ttu-id="4e161-284">Azure Mobile Apps přečte soubor JavaScript s názvem *azureMobile.js* z místního systému souborů.</span><span class="sxs-lookup"><span data-stu-id="4e161-284">Azure Mobile Apps reads a JavaScript file called *azureMobile.js* from the local filesystem.</span></span>  <span data-ttu-id="4e161-285">Nepoužívejte pro konfiguraci Azure Mobile Apps SDK v produkčním prostředí – použijte nastavení aplikace v rámci tohoto souboru [portál Azure] místo.</span><span class="sxs-lookup"><span data-stu-id="4e161-285">Do not use this file to configure the Azure Mobile Apps SDK in production - use App Settings within the [Azure portal] instead.</span></span>  <span data-ttu-id="4e161-286">*AzureMobile.js* soubor by měl exportovat objekt konfigurace.</span><span class="sxs-lookup"><span data-stu-id="4e161-286">The *azureMobile.js* file should export a configuration object.</span></span>  <span data-ttu-id="4e161-287">Nejběžnější nastavení jsou:</span><span class="sxs-lookup"><span data-stu-id="4e161-287">The most common settings are:</span></span>

* <span data-ttu-id="4e161-288">Nastavení databáze</span><span class="sxs-lookup"><span data-stu-id="4e161-288">Database Settings</span></span>
* <span data-ttu-id="4e161-289">Nastavení protokolování diagnostiky</span><span class="sxs-lookup"><span data-stu-id="4e161-289">Diagnostic Logging Settings</span></span>
* <span data-ttu-id="4e161-290">Alternativní nastavení CORS</span><span class="sxs-lookup"><span data-stu-id="4e161-290">Alternate CORS Settings</span></span>

<span data-ttu-id="4e161-291">Příklad *azureMobile.js* odpovídá souboru implementace předchozí nastavení databáze:</span><span class="sxs-lookup"><span data-stu-id="4e161-291">An example *azureMobile.js* file implementing the preceding database settings follows:</span></span>

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

<span data-ttu-id="4e161-292">Doporučujeme, abyste přidali *azureMobile.js* k vaší *.gitignore* souboru (nebo jiné zdrojového kódu ignorovat souboru) zabránit hesla ukládají v cloudu.</span><span class="sxs-lookup"><span data-stu-id="4e161-292">We recommend that you add *azureMobile.js* to your *.gitignore* file (or other source code control ignore file) to prevent passwords from being stored in the cloud.</span></span>  <span data-ttu-id="4e161-293">Vždy konfigurujte nastavení produkční v nastavení aplikace v rámci [portál Azure].</span><span class="sxs-lookup"><span data-stu-id="4e161-293">Always configure production settings in App Settings within the [Azure portal].</span></span>

### <span data-ttu-id="4e161-294"><a name="howto-appsettings"></a>Postupy: Konfigurace nastavení aplikace pro mobilní aplikace</span><span class="sxs-lookup"><span data-stu-id="4e161-294"><a name="howto-appsettings"></a>How: Configure App Settings for your Mobile App</span></span>
<span data-ttu-id="4e161-295">Většina nastavení v *azureMobile.js* soubor mít ekvivalentní nastavení aplikace [portál Azure].</span><span class="sxs-lookup"><span data-stu-id="4e161-295">Most settings in the *azureMobile.js* file have an equivalent App Setting in the [Azure portal].</span></span>  <span data-ttu-id="4e161-296">Ke konfiguraci vaší aplikace v nastavení aplikace použijte následující seznam:</span><span class="sxs-lookup"><span data-stu-id="4e161-296">Use the following list to configure your app in App Settings:</span></span>

| <span data-ttu-id="4e161-297">Nastavení aplikace</span><span class="sxs-lookup"><span data-stu-id="4e161-297">App Setting</span></span> | <span data-ttu-id="4e161-298">*azureMobile.js* nastavení</span><span class="sxs-lookup"><span data-stu-id="4e161-298">*azureMobile.js* Setting</span></span> | <span data-ttu-id="4e161-299">Popis</span><span class="sxs-lookup"><span data-stu-id="4e161-299">Description</span></span> | <span data-ttu-id="4e161-300">Platné hodnoty</span><span class="sxs-lookup"><span data-stu-id="4e161-300">Valid Values</span></span> |
|:--- |:--- |:--- |:--- |
| <span data-ttu-id="4e161-301">**MS_MobileAppName**</span><span class="sxs-lookup"><span data-stu-id="4e161-301">**MS_MobileAppName**</span></span> |<span data-ttu-id="4e161-302">jméno</span><span class="sxs-lookup"><span data-stu-id="4e161-302">name</span></span> |<span data-ttu-id="4e161-303">Název aplikace</span><span class="sxs-lookup"><span data-stu-id="4e161-303">The name of the app</span></span> |<span data-ttu-id="4e161-304">Řetězec</span><span class="sxs-lookup"><span data-stu-id="4e161-304">string</span></span> |
| <span data-ttu-id="4e161-305">**MS_MobileLoggingLevel**</span><span class="sxs-lookup"><span data-stu-id="4e161-305">**MS_MobileLoggingLevel**</span></span> |<span data-ttu-id="4e161-306">Logging.level</span><span class="sxs-lookup"><span data-stu-id="4e161-306">logging.level</span></span> |<span data-ttu-id="4e161-307">Úroveň minimální protokolu zpráv do protokolu</span><span class="sxs-lookup"><span data-stu-id="4e161-307">Minimum log level of messages to log</span></span> |<span data-ttu-id="4e161-308">Chyba, upozornění, informace o podrobné nastavení, ladění, i</span><span class="sxs-lookup"><span data-stu-id="4e161-308">error, warning, info, verbose, debug, silly</span></span> |
| <span data-ttu-id="4e161-309">**MS_DebugMode**</span><span class="sxs-lookup"><span data-stu-id="4e161-309">**MS_DebugMode**</span></span> |<span data-ttu-id="4e161-310">Ladění</span><span class="sxs-lookup"><span data-stu-id="4e161-310">debug</span></span> |<span data-ttu-id="4e161-311">Povolit nebo zakázat režim ladění</span><span class="sxs-lookup"><span data-stu-id="4e161-311">Enable or Disable debug mode</span></span> |<span data-ttu-id="4e161-312">Hodnota TRUE, false</span><span class="sxs-lookup"><span data-stu-id="4e161-312">true, false</span></span> |
| <span data-ttu-id="4e161-313">**MS_TableSchema**</span><span class="sxs-lookup"><span data-stu-id="4e161-313">**MS_TableSchema**</span></span> |<span data-ttu-id="4e161-314">data.Schema</span><span class="sxs-lookup"><span data-stu-id="4e161-314">data.schema</span></span> |<span data-ttu-id="4e161-315">Výchozí název schématu pro tabulky SQL</span><span class="sxs-lookup"><span data-stu-id="4e161-315">Default schema name for SQL tables</span></span> |<span data-ttu-id="4e161-316">String (výchozí: dbo)</span><span class="sxs-lookup"><span data-stu-id="4e161-316">string (default: dbo)</span></span> |
| <span data-ttu-id="4e161-317">**MS_DynamicSchema**</span><span class="sxs-lookup"><span data-stu-id="4e161-317">**MS_DynamicSchema**</span></span> |<span data-ttu-id="4e161-318">data.dynamicSchema</span><span class="sxs-lookup"><span data-stu-id="4e161-318">data.dynamicSchema</span></span> |<span data-ttu-id="4e161-319">Povolit nebo zakázat režim ladění</span><span class="sxs-lookup"><span data-stu-id="4e161-319">Enable or Disable debug mode</span></span> |<span data-ttu-id="4e161-320">Hodnota TRUE, false</span><span class="sxs-lookup"><span data-stu-id="4e161-320">true, false</span></span> |
| <span data-ttu-id="4e161-321">**MS_DisableVersionHeader**</span><span class="sxs-lookup"><span data-stu-id="4e161-321">**MS_DisableVersionHeader**</span></span> |<span data-ttu-id="4e161-322">verze (je nastavený na nedefinované)</span><span class="sxs-lookup"><span data-stu-id="4e161-322">version (set to undefined)</span></span> |<span data-ttu-id="4e161-323">Zakáže hlavičky X-záhlaví ZUMO-Server-Version</span><span class="sxs-lookup"><span data-stu-id="4e161-323">Disables the X-ZUMO-Server-Version header</span></span> |<span data-ttu-id="4e161-324">Hodnota TRUE, false</span><span class="sxs-lookup"><span data-stu-id="4e161-324">true, false</span></span> |
| <span data-ttu-id="4e161-325">**MS_SkipVersionCheck**</span><span class="sxs-lookup"><span data-stu-id="4e161-325">**MS_SkipVersionCheck**</span></span> |<span data-ttu-id="4e161-326">skipversioncheck</span><span class="sxs-lookup"><span data-stu-id="4e161-326">skipversioncheck</span></span> |<span data-ttu-id="4e161-327">Zakáže Kontrola verze klientského rozhraní API</span><span class="sxs-lookup"><span data-stu-id="4e161-327">Disables the client API version check</span></span> |<span data-ttu-id="4e161-328">Hodnota TRUE, false</span><span class="sxs-lookup"><span data-stu-id="4e161-328">true, false</span></span> |

<span data-ttu-id="4e161-329">Nastavení aplikace nastavit:</span><span class="sxs-lookup"><span data-stu-id="4e161-329">To set an App Setting:</span></span>

1. <span data-ttu-id="4e161-330">Přihlaste se k portálu [portál Azure].</span><span class="sxs-lookup"><span data-stu-id="4e161-330">Log in to the [Azure portal].</span></span>
2. <span data-ttu-id="4e161-331">Vyberte **všechny prostředky** nebo **App Services** pak klikněte na název vaší mobilní aplikace.</span><span class="sxs-lookup"><span data-stu-id="4e161-331">Select **All resources** or **App Services** then click the name of your Mobile App.</span></span>
3. <span data-ttu-id="4e161-332">Otevře se okno nastavení ve výchozím nastavení.</span><span class="sxs-lookup"><span data-stu-id="4e161-332">The Settings blade opens by default.</span></span> <span data-ttu-id="4e161-333">Pokud není, klikněte na tlačítko **nastavení**.</span><span class="sxs-lookup"><span data-stu-id="4e161-333">If it doesn't, click **Settings**.</span></span>
4. <span data-ttu-id="4e161-334">Klikněte na tlačítko **nastavení aplikace** v hlavní nabídce.</span><span class="sxs-lookup"><span data-stu-id="4e161-334">Click **Application settings** in the GENERAL menu.</span></span>
5. <span data-ttu-id="4e161-335">Přejděte do části Nastavení aplikace.</span><span class="sxs-lookup"><span data-stu-id="4e161-335">Scroll to the App Settings section.</span></span>
6. <span data-ttu-id="4e161-336">Pokud vaše aplikace, nastavení už existuje, klikněte na hodnotu nastavení aplikace a příslušnou hodnotu upravte.</span><span class="sxs-lookup"><span data-stu-id="4e161-336">If your app setting already exists, click the value of the app setting to edit the value.</span></span>
7. <span data-ttu-id="4e161-337">Pokud vaše aplikace nastavení neexistuje, zadejte do pole klíč a hodnotu v poli hodnota nastavení aplikace.</span><span class="sxs-lookup"><span data-stu-id="4e161-337">If your app setting does not exist, enter the App Setting in the Key box and the value in the Value box.</span></span>
8. <span data-ttu-id="4e161-338">Po dokončení, klikněte na tlačítko **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="4e161-338">Once you are complete, click **Save**.</span></span>

<span data-ttu-id="4e161-339">Změna většinu nastavení aplikace vyžaduje restartování služby.</span><span class="sxs-lookup"><span data-stu-id="4e161-339">Changing most app settings requires a service restart.</span></span>

### <span data-ttu-id="4e161-340"><a name="howto-use-sqlazure"></a>Postupy: použití SQL Database jako produkční úložiště dat.</span><span class="sxs-lookup"><span data-stu-id="4e161-340"><a name="howto-use-sqlazure"></a>How to: Use SQL Database as your production data store</span></span>
<!--- ALTERNATE INCLUDE - we can't use ../includes/app-service-mobile-dotnet-backend-create-new-service.md - slightly different semantics -->

<span data-ttu-id="4e161-341">Používání Azure SQL Database jako úložiště dat je stejný jako mezi všechny typy aplikací Azure App Service.</span><span class="sxs-lookup"><span data-stu-id="4e161-341">Using Azure SQL Database as a data store is identical across all Azure App Service application types.</span></span> <span data-ttu-id="4e161-342">Pokud jste to ještě neudělali, postupujte podle těchto kroků můžete vytvořit back-end mobilní aplikace.</span><span class="sxs-lookup"><span data-stu-id="4e161-342">If you have not done so already, follow these steps to create a Mobile App backend.</span></span>

1. <span data-ttu-id="4e161-343">Přihlaste se k portálu [portál Azure].</span><span class="sxs-lookup"><span data-stu-id="4e161-343">Log in to the [Azure portal].</span></span>
2. <span data-ttu-id="4e161-344">V horní části levé části okna, klikněte na tlačítko **+ nový** tlačítko > **Web + mobilní** > **mobilní aplikace**, pak zadejte název pro váš back-end mobilní aplikace.</span><span class="sxs-lookup"><span data-stu-id="4e161-344">In the top left of the window, click the **+NEW** button > **Web + Mobile** > **Mobile App**, then provide a name for your Mobile App backend.</span></span>
3. <span data-ttu-id="4e161-345">V **skupiny prostředků** zadejte stejný název jako vaše aplikace.</span><span class="sxs-lookup"><span data-stu-id="4e161-345">In the **Resource Group** box, enter the same name as your app.</span></span>
4. <span data-ttu-id="4e161-346">Plán aplikační služby výchozí je vybrána.</span><span class="sxs-lookup"><span data-stu-id="4e161-346">The Default App Service plan is selected.</span></span>  <span data-ttu-id="4e161-347">Pokud chcete změnit plán služby App Service, můžete provést kliknutím plán služby App Service > **+ vytvořit nový**.</span><span class="sxs-lookup"><span data-stu-id="4e161-347">If you wish to change your App Service plan, you can do so by clicking the App Service Plan > **+ Create New**.</span></span>  <span data-ttu-id="4e161-348">Zadejte název nového plánu služby App Service a vyberte požadované místo.</span><span class="sxs-lookup"><span data-stu-id="4e161-348">Provide a name of the new App Service plan and select an appropriate location.</span></span>  <span data-ttu-id="4e161-349">Klikněte na tlačítko cenová úroveň a vyberte příslušné cenovou úroveň pro službu.</span><span class="sxs-lookup"><span data-stu-id="4e161-349">Click the Pricing tier and select an appropriate pricing tier for the service.</span></span> <span data-ttu-id="4e161-350">Vyberte **zobrazit všechny** do zobrazení více ceny možnosti, jako například **volné** a **sdílené**.</span><span class="sxs-lookup"><span data-stu-id="4e161-350">Select **View all** to view more pricing options, such as **Free** and **Shared**.</span></span>  <span data-ttu-id="4e161-351">Až vyberete cenovou úroveň, klikněte na **vyberte** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="4e161-351">Once you have selected the pricing tier, click the **Select** button.</span></span>  <span data-ttu-id="4e161-352">Zpět v **plán služby App Service** okně klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="4e161-352">Back in the **App Service plan** blade, click **OK**.</span></span>
5. <span data-ttu-id="4e161-353">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="4e161-353">Click **Create**.</span></span> <span data-ttu-id="4e161-354">Zřizování back-end mobilní aplikace může trvat několik minut.</span><span class="sxs-lookup"><span data-stu-id="4e161-354">Provisioning a Mobile App backend can take a couple of minutes.</span></span>  <span data-ttu-id="4e161-355">Po zřízení back-end mobilní aplikace, portál ho otevře **nastavení** okno pro back-end mobilní aplikace.</span><span class="sxs-lookup"><span data-stu-id="4e161-355">Once the Mobile App backend is provisioned, the portal opens the **Settings** blade for the Mobile App backend.</span></span>

<span data-ttu-id="4e161-356">Po vytvoření back-end mobilní aplikace, můžete se připojit existující databázi SQL k váš back-end mobilní aplikace nebo vytvořit novou databázi SQL.</span><span class="sxs-lookup"><span data-stu-id="4e161-356">Once the Mobile App backend is created, you can choose to either connect an existing SQL database to your Mobile App backend or create a new SQL database.</span></span>  <span data-ttu-id="4e161-357">V této části vytvoříme databázi SQL.</span><span class="sxs-lookup"><span data-stu-id="4e161-357">In this section, we create a SQL database.</span></span>

> [!NOTE]
> <span data-ttu-id="4e161-358">Pokud už máte databázi ve stejném umístění jako back-end mobilní aplikace, můžete místo toho zvolíte **použít existující databázi** a pak vyberte tuto databázi.</span><span class="sxs-lookup"><span data-stu-id="4e161-358">If you already have a database in the same location as the mobile app backend, you can instead choose **Use an existing database** and then select that database.</span></span> <span data-ttu-id="4e161-359">Používání databáze v jiném umístění se nedoporučuje kvůli vyšší latence.</span><span class="sxs-lookup"><span data-stu-id="4e161-359">The use of a database in a different location is not recommended because of higher latencies.</span></span>
>
>

1. <span data-ttu-id="4e161-360">V nové mobilní aplikace back-end, klikněte na **nastavení** > **mobilní aplikace** > **Data** > **+ přidat**.</span><span class="sxs-lookup"><span data-stu-id="4e161-360">In the new Mobile App backend, click **Settings** > **Mobile App** > **Data** > **+Add**.</span></span>
2. <span data-ttu-id="4e161-361">V **přidat datové připojení** okně klikněte na tlačítko **SQL Database – nakonfigurujte požadovaná nastavení** > **vytvořit novou databázi**.</span><span class="sxs-lookup"><span data-stu-id="4e161-361">In the **Add data connection** blade, click **SQL Database - Configure required settings** > **Create a new database**.</span></span>  <span data-ttu-id="4e161-362">Zadejte název nové databáze v **název** pole.</span><span class="sxs-lookup"><span data-stu-id="4e161-362">Enter the name of the new database in the **Name** field.</span></span>
3. <span data-ttu-id="4e161-363">Klikněte na tlačítko **Server**.</span><span class="sxs-lookup"><span data-stu-id="4e161-363">Click **Server**.</span></span>  <span data-ttu-id="4e161-364">V **nový server** okno, zadejte název jedinečné serveru v **název serveru** pole a zadejte vhodný **přihlašovací jméno správce serveru** a **heslo**.</span><span class="sxs-lookup"><span data-stu-id="4e161-364">In the **New server** blade, enter a unique server name in the **Server name** field, and provide a suitable **Server admin login** and **Password**.</span></span>  <span data-ttu-id="4e161-365">Ujistěte se, **povolit službám azure přístup k serveru** je zaškrtnuté.</span><span class="sxs-lookup"><span data-stu-id="4e161-365">Ensure **Allow azure services to access server** is checked.</span></span>  <span data-ttu-id="4e161-366">Klikněte na **OK**.</span><span class="sxs-lookup"><span data-stu-id="4e161-366">Click **OK**.</span></span>

    ![Vytvoření databáze Azure SQL][6]
4. <span data-ttu-id="4e161-368">Na **novou databázi** okně klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="4e161-368">On the **New database** blade, click **OK**.</span></span>
5. <span data-ttu-id="4e161-369">Zpět na **přidat datové připojení** vyberte **připojovací řetězec**, zadejte přihlašovací jméno a heslo, které jste zadali při vytváření databáze.</span><span class="sxs-lookup"><span data-stu-id="4e161-369">Back on the **Add data connection** blade, select **Connection string**, enter the login and password that you provided when creating the database.</span></span>  <span data-ttu-id="4e161-370">Pokud použijete existující databázi, zadejte přihlašovací údaje pro tuto databázi.</span><span class="sxs-lookup"><span data-stu-id="4e161-370">If you use an existing database, provide the login credentials for that database.</span></span>  <span data-ttu-id="4e161-371">Po zadání, klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="4e161-371">Once entered, click **OK**.</span></span>
6. <span data-ttu-id="4e161-372">Zpět na **přidat datové připojení** znovu, klikněte na **OK** k vytvoření databáze.</span><span class="sxs-lookup"><span data-stu-id="4e161-372">Back on the **Add data connection** blade again, click **OK** to create the database.</span></span>

<!--- END OF ALTERNATE INCLUDE -->

<span data-ttu-id="4e161-373">Vytváření databáze může trvat několik minut.</span><span class="sxs-lookup"><span data-stu-id="4e161-373">Creation of the database can take a few minutes.</span></span>  <span data-ttu-id="4e161-374">Použití **oznámení** oblasti můžete sledovat průběh nasazení.</span><span class="sxs-lookup"><span data-stu-id="4e161-374">Use the **Notifications** area to monitor the progress of the deployment.</span></span>  <span data-ttu-id="4e161-375">Není průběhu až do databáze byla úspěšně nasazena.</span><span class="sxs-lookup"><span data-stu-id="4e161-375">Do not progress until the database has been deployed successfully.</span></span>  <span data-ttu-id="4e161-376">Jakmile úspěšně nasazena, je pro instanci databáze SQL na váš mobilní back-end nastavení aplikace vytvořit připojovací řetězec.</span><span class="sxs-lookup"><span data-stu-id="4e161-376">Once successfully deployed, a Connection String is created for the SQL Database instance in your Mobile backend App Settings.</span></span>  <span data-ttu-id="4e161-377">Zobrazí se toto nastavení aplikace **nastavení** > **nastavení aplikace** > **připojovací řetězce**.</span><span class="sxs-lookup"><span data-stu-id="4e161-377">You can see this app setting in the **Settings** > **Application settings** > **Connection strings**.</span></span>

### <span data-ttu-id="4e161-378"><a name="howto-tables-auth"></a>Postupy: ověřování vyžadovat pro přístup k tabulkám</span><span class="sxs-lookup"><span data-stu-id="4e161-378"><a name="howto-tables-auth"></a>How to: Require authentication for access to tables</span></span>
<span data-ttu-id="4e161-379">Pokud chcete použít ověřování aplikace služby s koncovým bodem tabulky, je nutné nakonfigurovat aplikaci služby ověřování v [portál Azure] první.</span><span class="sxs-lookup"><span data-stu-id="4e161-379">If you wish to use App Service Authentication with the tables endpoint, you must configure App Service Authentication in the [Azure portal] first.</span></span>  <span data-ttu-id="4e161-380">Další podrobnosti o konfiguraci ověřování ve službě Azure App Service projděte si v Průvodci konfigurace pro zprostředkovatele identity, kterou chcete použít:</span><span class="sxs-lookup"><span data-stu-id="4e161-380">For more details about configuring authentication in an Azure App Service, review the Configuration Guide for the identity provider you intend to use:</span></span>

* <span data-ttu-id="4e161-381">[Postup konfigurace ověřování Azure Active Directory]</span><span class="sxs-lookup"><span data-stu-id="4e161-381">[How to configure Azure Active Directory Authentication]</span></span>
* <span data-ttu-id="4e161-382">[Postup konfigurace ověřování Facebook]</span><span class="sxs-lookup"><span data-stu-id="4e161-382">[How to configure Facebook Authentication]</span></span>
* <span data-ttu-id="4e161-383">[Postup konfigurace ověřování Google]</span><span class="sxs-lookup"><span data-stu-id="4e161-383">[How to configure Google Authentication]</span></span>
* <span data-ttu-id="4e161-384">[Jak nakonfigurovat Microsoft Authentication]</span><span class="sxs-lookup"><span data-stu-id="4e161-384">[How to configure Microsoft Authentication]</span></span>
* <span data-ttu-id="4e161-385">[Jak nakonfigurovat ověřování služby Twitter]</span><span class="sxs-lookup"><span data-stu-id="4e161-385">[How to configure Twitter Authentication]</span></span>

<span data-ttu-id="4e161-386">Každá tabulka měla vlastností, které můžete použít k řízení přístupu k tabulce.</span><span class="sxs-lookup"><span data-stu-id="4e161-386">Each table has an access property that can be used to control access to the table.</span></span>  <span data-ttu-id="4e161-387">Následující příklad ukazuje staticky definovaných tabulku s je vyžadováno ověření.</span><span class="sxs-lookup"><span data-stu-id="4e161-387">The following sample shows a statically defined table with authentication required.</span></span>

    var azureMobileApps = require('azure-mobile-apps');

    var table = azureMobileApps.table();

    // Define the columns within the table
    table.columns = {
        "text": "string",
        "complete": "boolean"
    };

    // Turn off dynamic schema
    table.dynamicSchema = false;

    // Require authentication to access the table
    table.access = 'authenticated';

    module.exports = table;

<span data-ttu-id="4e161-388">Vlastnost přístupu může trvat jednu ze tří hodnot</span><span class="sxs-lookup"><span data-stu-id="4e161-388">The access property can take one of three values</span></span>

* <span data-ttu-id="4e161-389">*Anonymní* označuje, že klientská aplikace může číst data bez ověřování</span><span class="sxs-lookup"><span data-stu-id="4e161-389">*anonymous* indicates that the client application is allowed to read data without authentication</span></span>
* <span data-ttu-id="4e161-390">*ověření* indikuje, že klientská aplikace, musí poslat platný ověřovací token s požadavkem</span><span class="sxs-lookup"><span data-stu-id="4e161-390">*authenticated* indicates that the client application must send a valid authentication token with the request</span></span>
* <span data-ttu-id="4e161-391">*zakázané* označuje, že tato tabulka je aktuálně zakázán</span><span class="sxs-lookup"><span data-stu-id="4e161-391">*disabled* indicates that this table is currently disabled</span></span>

<span data-ttu-id="4e161-392">Pokud vlastnost přístupu není definován, je povolený přístup bez ověřování.</span><span class="sxs-lookup"><span data-stu-id="4e161-392">If the access property is undefined, unauthenticated access is allowed.</span></span>

### <span data-ttu-id="4e161-393"><a name="howto-tables-getidentity"></a>Postupy: použití ověřování deklarací identity s tabulkami</span><span class="sxs-lookup"><span data-stu-id="4e161-393"><a name="howto-tables-getidentity"></a>How to: Use authentication claims with your tables</span></span>
<span data-ttu-id="4e161-394">Můžete nastavit různé deklarace identity, které jsou požadovány při nastavení ověřování.</span><span class="sxs-lookup"><span data-stu-id="4e161-394">You can set up various claims that are requested when authentication is set up.</span></span>  <span data-ttu-id="4e161-395">Tyto deklarace nejsou obvykle dostupné prostřednictvím `context.user` objektu.</span><span class="sxs-lookup"><span data-stu-id="4e161-395">These claims are not normally available through the `context.user` object.</span></span>  <span data-ttu-id="4e161-396">Však mohou být načteny pomocí `context.user.getIdentity()` metoda.</span><span class="sxs-lookup"><span data-stu-id="4e161-396">However, they can be retrieved using the `context.user.getIdentity()` method.</span></span>  <span data-ttu-id="4e161-397">`getIdentity()` Metoda vrátí Promise, který se přeloží na objekt.</span><span class="sxs-lookup"><span data-stu-id="4e161-397">The `getIdentity()` method returns a Promise that resolves to an object.</span></span>  <span data-ttu-id="4e161-398">Objekt se ukládá do klíčů metodou ověřování (facebook, google, twitter, microsoftaccount nebo aad).</span><span class="sxs-lookup"><span data-stu-id="4e161-398">The object is keyed by the authentication method (facebook, google, twitter, microsoftaccount, or aad).</span></span>

<span data-ttu-id="4e161-399">Například pokud jste nastavili Account Microsoft ověřování a žádosti, které deklarace identity e-mailové adresy, můžete přidat e-mailovou adresu na záznam s řadičem následující tabulku:</span><span class="sxs-lookup"><span data-stu-id="4e161-399">For example, if you set up Microsoft Account authentication and request the email addresses claim, you can add the email address to the record with the following table controller:</span></span>

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
    * Limit the context query to those records with the authenticated user email address
    * @param {Context} context the operation context
    * @returns {Promise} context execution Promise
    */
    function queryContextForEmail(context) {
        return context.user.getIdentity().then((data) => {
            context.query.where({ emailAddress: data.microsoftaccount.claims.emailaddress });
            return context.execute();
        });
    }

    /**
    * Adds the email address from the claims to the context item - used for
    * insert operations
    * @param {Context} context the operation context
    * @returns {Promise} context execution Promise
    */
    function addEmailToContext(context) {
        return context.user.getIdentity().then((data) => {
            context.item.emailAddress = data.microsoftaccount.claims.emailaddress;
            return context.execute();
        });
    }

    // Configure specific code when the client does a request
    // READ - only return records belonging to the authenticated user
    table.read(queryContextForEmail);

    // CREATE - add or overwrite the userId based on the authenticated user
    table.insert(addEmailToContext);

    // UPDATE - only allow updating of record belong to the authenticated user
    table.update(queryContextForEmail);

    // DELETE - only allow deletion of records belong to the authenticated uer
    table.delete(queryContextForEmail);

    module.exports = table;

<span data-ttu-id="4e161-400">Pokud chcete zobrazit, jaké deklarace identity jsou k dispozici, zobrazíte pomocí webového prohlížeče `/.auth/me` koncový bod vašeho webu.</span><span class="sxs-lookup"><span data-stu-id="4e161-400">To see what claims are available, use a web browser to view the `/.auth/me` endpoint of your site.</span></span>

### <span data-ttu-id="4e161-401"><a name="howto-tables-disabled"></a>Postupy: zakázat přístup k určité tabulky operací</span><span class="sxs-lookup"><span data-stu-id="4e161-401"><a name="howto-tables-disabled"></a>How to: Disable access to specific table operations</span></span>
<span data-ttu-id="4e161-402">Kromě zobrazování v tabulce, vlastnost přístupu slouží k řízení jednotlivých operací.</span><span class="sxs-lookup"><span data-stu-id="4e161-402">In addition to appearing on the table, the access property can be used to control individual operations.</span></span>  <span data-ttu-id="4e161-403">Existují čtyři operace:</span><span class="sxs-lookup"><span data-stu-id="4e161-403">There are four operations:</span></span>

* <span data-ttu-id="4e161-404">*Přečtěte si* RESTful získat operace pro tabulku</span><span class="sxs-lookup"><span data-stu-id="4e161-404">*read* is the RESTful GET operation on the table</span></span>
* <span data-ttu-id="4e161-405">*Vložit* je operace RESTful POST v tabulce</span><span class="sxs-lookup"><span data-stu-id="4e161-405">*insert* is the RESTful POST operation on the table</span></span>
* <span data-ttu-id="4e161-406">*Aktualizovat* je operace RESTful opravy v tabulce</span><span class="sxs-lookup"><span data-stu-id="4e161-406">*update* is the RESTful PATCH operation on the table</span></span>
* <span data-ttu-id="4e161-407">*Odstranit* RESTful odstranit operace v tabulce</span><span class="sxs-lookup"><span data-stu-id="4e161-407">*delete* is the RESTful DELETE operation on the table</span></span>

<span data-ttu-id="4e161-408">Můžete například zadat jen pro čtení neověřené tabulku:</span><span class="sxs-lookup"><span data-stu-id="4e161-408">For example, you may wish to provide a read-only unauthenticated table:</span></span>

    var azureMobileApps = require('azure-mobile-apps');

    var table = azureMobileApps.table();

    // Read-Only table - only allow READ operations
    table.read.access = 'anonymous';
    table.insert.access = 'disabled';
    table.update.access = 'disabled';
    table.delete.access = 'disabled';

    module.exports = table;

### <span data-ttu-id="4e161-409"><a name="howto-tables-query"></a>Postupy: Upravit dotaz, který se používá s operace s tabulkou</span><span class="sxs-lookup"><span data-stu-id="4e161-409"><a name="howto-tables-query"></a>How to: Adjust the query that is used with table operations</span></span>
<span data-ttu-id="4e161-410">Běžné požadavky pro operace s tabulkou je poskytnout omezeným zobrazením data.</span><span class="sxs-lookup"><span data-stu-id="4e161-410">A common requirement for table operations is to provide a restricted view of the data.</span></span>  <span data-ttu-id="4e161-411">Například může zadat tabulku, která je označené ID ověřeného uživatele tak, že můžete pouze pro čtení nebo aktualizovat vlastní záznamy.</span><span class="sxs-lookup"><span data-stu-id="4e161-411">For example, you may provide a table that is tagged with the authenticated user ID such that you can only read or update your own records.</span></span>  <span data-ttu-id="4e161-412">Následující definice tabulky obsahuje tato funkce:</span><span class="sxs-lookup"><span data-stu-id="4e161-412">The following table definition provides this functionality:</span></span>

    var azureMobileApps = require('azure-mobile-apps');

    var table = azureMobileApps.table();

    // Define a static schema for the table
    table.columns = {
        "userId": "string",
        "text": "string",
        "complete": "boolean"
    };
    table.dynamicSchema = false;

    // Require authentication for this table
    table.access = 'authenticated';

    // Ensure that only records for the authenticated user are retrieved
    table.read(function (context) {
        context.query.where({ userId: context.user.id });
        return context.execute();
    });

    // When adding records, add or overwrite the userId with the authenticated user
    table.insert(function (context) {
        context.item.userId = context.user.id;
        return context.execute();
    });

    module.exports = table;

<span data-ttu-id="4e161-413">Operace, které obvykle spuštění dotazu mají vlastnost dotazu, můžete upravit místo, kde klauzule.</span><span class="sxs-lookup"><span data-stu-id="4e161-413">Operations that normally execute a query have a query property that you can adjust with a where clause.</span></span> <span data-ttu-id="4e161-414">Vlastnost dotaz je [QueryJS] objekt, který se používá k převodu dotaz OData na jinou hodnotu, která může zpracovat data back-end.</span><span class="sxs-lookup"><span data-stu-id="4e161-414">The query property is a [QueryJS] object that is used to convert an OData query to something that the data backend can process.</span></span>  <span data-ttu-id="4e161-415">Jednoduché rovnosti případech (např. předchozí jeden) lze mapu.</span><span class="sxs-lookup"><span data-stu-id="4e161-415">For simple equality cases (like the preceding one), a map can be used.</span></span> <span data-ttu-id="4e161-416">Můžete také přidat konkrétní klauzule SQL:</span><span class="sxs-lookup"><span data-stu-id="4e161-416">You can also add specific SQL clauses:</span></span>

    context.query.where('myfield eq ?', 'value');

### <span data-ttu-id="4e161-417"><a name="howto-tables-softdelete"></a>Postupy: Konfigurace obnovitelného odstranění na tabulce</span><span class="sxs-lookup"><span data-stu-id="4e161-417"><a name="howto-tables-softdelete"></a>How to: Configure soft delete on a table</span></span>
<span data-ttu-id="4e161-418">Obnovitelného odstranění neodstraní ve skutečnosti záznamy.</span><span class="sxs-lookup"><span data-stu-id="4e161-418">Soft Delete does not actually delete records.</span></span>  <span data-ttu-id="4e161-419">Místo toho se označí je jako v databázi odstranit nastavením odstraněný sloupec na hodnotu true.</span><span class="sxs-lookup"><span data-stu-id="4e161-419">Instead it marks them as deleted within the database by setting the deleted column to true.</span></span>  <span data-ttu-id="4e161-420">Azure Mobile Apps SDK automaticky odebere obnovitelné odstranění záznamů z výsledků, pokud Mobile Client SDK používá IncludeDeleted().</span><span class="sxs-lookup"><span data-stu-id="4e161-420">The Azure Mobile Apps SDK automatically removes soft-deleted records from results unless the Mobile Client SDK uses IncludeDeleted().</span></span>  <span data-ttu-id="4e161-421">Pokud chcete nakonfigurovat tabulku pro obnovitelného odstranění, nastavte `softDelete` vlastnost v souboru definice tabulky:</span><span class="sxs-lookup"><span data-stu-id="4e161-421">To configure a table for soft delete, set the `softDelete` property in the table definition file:</span></span>

    var azureMobileApps = require('azure-mobile-apps');

    var table = azureMobileApps.table();

    // Define the columns within the table
    table.columns = {
        "text": "string",
        "complete": "boolean"
    };

    // Turn off dynamic schema
    table.dynamicSchema = false;

    // Turn on Soft Delete
    table.softDelete = true;

    // Require authentication to access the table
    table.access = 'authenticated';

    module.exports = table;

<span data-ttu-id="4e161-422">Byste měli zavést mechanismus pro vymazání záznamů – buď z klientské aplikace, prostřednictvím webové úlohy, funkce Azure nebo prostřednictvím vlastní rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="4e161-422">You should establish a mechanism for purging records - either from a client application, via a WebJob, Azure Function or through a custom API.</span></span>

### <span data-ttu-id="4e161-423"><a name="howto-tables-seeding"></a>Postupy: počáteční hodnoty databázi daty</span><span class="sxs-lookup"><span data-stu-id="4e161-423"><a name="howto-tables-seeding"></a>How to: Seed your database with data</span></span>
<span data-ttu-id="4e161-424">Při vytváření nové aplikace, budete možná chtít počáteční hodnoty tabulku s daty.</span><span class="sxs-lookup"><span data-stu-id="4e161-424">When creating a new application, you may wish to seed a table with data.</span></span>  <span data-ttu-id="4e161-425">To lze provést v rámci soubor JavaScript definice tabulky následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="4e161-425">This can be done within the table definition JavaScript file as follows:</span></span>

    var azureMobileApps = require('azure-mobile-apps');

    var table = azureMobileApps.table();

    // Define the columns within the table
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

    // Require authentication to access the table
    table.access = 'authenticated';

    module.exports = table;

<span data-ttu-id="4e161-426">Synchronizace replik indexů data je možné pouze při vytváření tabulky pomocí Azure Mobile Apps SDK.</span><span class="sxs-lookup"><span data-stu-id="4e161-426">Seeding of data is only done when the table is created by the Azure Mobile Apps SDK.</span></span>  <span data-ttu-id="4e161-427">Pokud v tabulce již existuje v databázi, je do tabulky vložit žádná data.</span><span class="sxs-lookup"><span data-stu-id="4e161-427">If the table already exists within the database, no data is injected into the table.</span></span>  <span data-ttu-id="4e161-428">Pokud je zapnutá dynamická schéma, schéma odvodit z dosazená data.</span><span class="sxs-lookup"><span data-stu-id="4e161-428">If dynamic schema is turned on, then the schema is inferred from the seeded data.</span></span>

<span data-ttu-id="4e161-429">Doporučujeme, aby explicitně volání `tables.initialize()` metodu pro vytvoření tabulky při spuštění služby.</span><span class="sxs-lookup"><span data-stu-id="4e161-429">We recommend that you explicitly call the `tables.initialize()` method to create the table when the service starts running.</span></span>

### <span data-ttu-id="4e161-430"><a name="Swagger"></a>Postupy: Povolení podpory Swagger</span><span class="sxs-lookup"><span data-stu-id="4e161-430"><a name="Swagger"></a>How to: Enable Swagger support</span></span>
<span data-ttu-id="4e161-431">Azure App Service Mobile Apps se dodává s integrovanou [Swagger] podporovat.</span><span class="sxs-lookup"><span data-stu-id="4e161-431">Azure App Service Mobile Apps comes with built-in [Swagger] support.</span></span>  <span data-ttu-id="4e161-432">Povolení podpory Swagger, nejprve nainstalujte rozhraní swagger jako závislost:</span><span class="sxs-lookup"><span data-stu-id="4e161-432">To enable Swagger support, first install the swagger-ui as a dependency:</span></span>

    npm install --save swagger-ui

<span data-ttu-id="4e161-433">Po instalaci můžete povolit podporu Swagger v konstruktoru sady Azure Mobile Apps:</span><span class="sxs-lookup"><span data-stu-id="4e161-433">Once installed, you can enable Swagger support in the Azure Mobile Apps constructor:</span></span>

    var mobile = azureMobileApps({ swagger: true });

<span data-ttu-id="4e161-434">Budete pravděpodobně pouze chcete povolit podporu Swagger v edicích vývoj.</span><span class="sxs-lookup"><span data-stu-id="4e161-434">You probably only want to enable Swagger support in development editions.</span></span>  <span data-ttu-id="4e161-435">Můžete to provést s využitím `NODE_ENV` nastavení aplikace:</span><span class="sxs-lookup"><span data-stu-id="4e161-435">You can do this by utilizing the `NODE_ENV` app setting:</span></span>

    var mobile = azureMobileApps({ swagger: process.env.NODE_ENV !== 'production' });

<span data-ttu-id="4e161-436">Koncový bod swagger se nachází v http://*yoursite*.azurewebsites.net/swagger.</span><span class="sxs-lookup"><span data-stu-id="4e161-436">The swagger endpoint is located at http://*yoursite*.azurewebsites.net/swagger.</span></span>  <span data-ttu-id="4e161-437">Dostanete uživatelské rozhraní Swagger pomocí `/swagger/ui` koncový bod.</span><span class="sxs-lookup"><span data-stu-id="4e161-437">You can access the Swagger UI via the `/swagger/ui` endpoint.</span></span>  <span data-ttu-id="4e161-438">Pokud budete chtít vyžadovat ověřování napříč celou aplikaci, vytvoří Swagger k chybě.</span><span class="sxs-lookup"><span data-stu-id="4e161-438">if you choose to require authentication across your entire application, Swagger produces an error.</span></span>  <span data-ttu-id="4e161-439">Nejlepších výsledků dosáhnete, zvolte tak, aby měl neověřené požadavky pomocí ověřování Azure App Service / nastavení autorizace, pak řídit, ověřování pomocí `table.access` vlastnost.</span><span class="sxs-lookup"><span data-stu-id="4e161-439">For best results, choose to allow unauthenticated requests through in the Azure App Service Authentication / Authorization settings, then control authentication using the `table.access` property.</span></span>

<span data-ttu-id="4e161-440">Můžete také přidat možnost Swagger vaše `azureMobile.js` souboru, pouze pokud chcete podporu Swagger při vývoji místně.</span><span class="sxs-lookup"><span data-stu-id="4e161-440">You can also add the Swagger option to your `azureMobile.js` file if you only want Swagger support when developing locally.</span></span>

## <a name="a-namepushpush-notifications"></a><span data-ttu-id="4e161-441"><a name="push">Nabízená oznámení</span><span class="sxs-lookup"><span data-stu-id="4e161-441"><a name="push">Push notifications</span></span>
<span data-ttu-id="4e161-442">Mobilní aplikace se integruje s Azure Notification Hubs umožnit odesílání cílové nabízených oznámení na miliony zařízení pro všechny hlavní platformy.</span><span class="sxs-lookup"><span data-stu-id="4e161-442">Mobile Apps integrates with Azure Notification Hubs to enable you to send targeted push notifications to millions of devices across all major platforms.</span></span> <span data-ttu-id="4e161-443">Pomocí centra oznámení můžete odesílat nabízená oznámení iOS, Android a Windows zařízení.</span><span class="sxs-lookup"><span data-stu-id="4e161-443">By using Notification Hubs, you can send push notifications to iOS, Android and Windows devices.</span></span> <span data-ttu-id="4e161-444">Další informace o všech, že lze provádět pomocí Notification Hubs najdete v tématu [přehledu této služby](../notification-hubs/notification-hubs-push-notification-overview.md).</span><span class="sxs-lookup"><span data-stu-id="4e161-444">To learn more about all that you can do with Notification Hubs, see [Notification Hubs Overview](../notification-hubs/notification-hubs-push-notification-overview.md).</span></span>

### <span data-ttu-id="4e161-445"></a><a name="send-push"></a>Postupy: odesílání nabízených oznámení</span><span class="sxs-lookup"><span data-stu-id="4e161-445"></a><a name="send-push"></a>How to: Send push notifications</span></span>
<span data-ttu-id="4e161-446">Následující kód ukazuje, jak používat nabízená objekt k odesílání vysílání nabízených oznámení do registrovaná zařízení iOS:</span><span class="sxs-lookup"><span data-stu-id="4e161-446">The following code shows how to use the push object to send a broadcast push notification to registered iOS devices:</span></span>

    // Create an APNS payload.
    var payload = '{"aps": {"alert": "This is an APNS payload."}}';

    // Only do the push if configured
    if (context.push) {
        // Send a push notification using APNS.
        context.push.apns.send(null, payload, function (error) {
            if (error) {
                // Do something or log the error.
            }
        });
    }

<span data-ttu-id="4e161-447">Vytvořením šablony nabízené registrace z klienta, můžete místo toho odeslat zprávu nabízené šablony do zařízení na všech podporovaných platformách.</span><span class="sxs-lookup"><span data-stu-id="4e161-447">By creating a template push registration from the client, you can instead send a template push message to devices on all supported platforms.</span></span> <span data-ttu-id="4e161-448">Následující kód ukazuje, jak odeslat oznámení šablony:</span><span class="sxs-lookup"><span data-stu-id="4e161-448">The following code shows how to send a template notification:</span></span>

    // Define the template payload.
    var payload = '{"messageParam": "This is a template payload."}';

    // Only do the push if configured
    if (context.push) {
        // Send a template notification.
        context.push.send(null, payload, function (error) {
            if (error) {
                // Do something or log the error.
            }
        });
    }


### <span data-ttu-id="4e161-449"><a name="push-user"></a>Postupy: odesílání nabízených oznámení pro ověřené uživatele pomocí značek</span><span class="sxs-lookup"><span data-stu-id="4e161-449"><a name="push-user"></a>How to: Send push notifications to an authenticated user using tags</span></span>
<span data-ttu-id="4e161-450">Pokud ověřený uživatel zaregistruje pro nabízená oznámení, se automaticky přidá značku ID uživatele pro registraci.</span><span class="sxs-lookup"><span data-stu-id="4e161-450">When an authenticated user registers for push notifications, a user ID tag is automatically added to the registration.</span></span> <span data-ttu-id="4e161-451">Pomocí této značky možné posílat nabízená oznámení pro všechna zařízení zaregistrovat podle konkrétního uživatele.</span><span class="sxs-lookup"><span data-stu-id="4e161-451">By using this tag, you can send push notifications to all devices registered by a specific user.</span></span> <span data-ttu-id="4e161-452">Následující kód získá identifikátor SID uživatele, které zadal žádost a odešle šablony nabízených oznámení do každé registrace zařízení pro tohoto uživatele:</span><span class="sxs-lookup"><span data-stu-id="4e161-452">The following code gets the SID of user making the request and sends a template push notification to every device registration for that user:</span></span>

    // Only do the push if configured
    if (context.push) {
        // Send a notification to the current user.
        context.push.send(context.user.id, payload, function (error) {
            if (error) {
                // Do something or log the error.
            }
        });
    }

<span data-ttu-id="4e161-453">Při registraci pro nabízená oznámení z ověřený klient, ujistěte se, že před pokusem o registraci dokončením ověřování.</span><span class="sxs-lookup"><span data-stu-id="4e161-453">When registering for push notifications from an authenticated client, make sure that authentication is complete before attempting registration.</span></span>

## <span data-ttu-id="4e161-454"><a name="CustomAPI"></a>Vlastní rozhraní API</span><span class="sxs-lookup"><span data-stu-id="4e161-454"><a name="CustomAPI"></a> Custom APIs</span></span>
### <span data-ttu-id="4e161-455"><a name="howto-customapi-basic"></a>Postupy: definování vlastní rozhraní API</span><span class="sxs-lookup"><span data-stu-id="4e161-455"><a name="howto-customapi-basic"></a>How to: Define a custom API</span></span>
<span data-ttu-id="4e161-456">Kromě přístupu k datům rozhraní API prostřednictvím koncového bodu /tables Azure Mobile Apps může poskytnout vlastní pokrytí rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="4e161-456">In addition to the data access API via the /tables endpoint, Azure Mobile Apps can provide custom API coverage.</span></span>  <span data-ttu-id="4e161-457">Vlastní rozhraní API jsou definovány podobným způsobem jako definice tabulek a stejná přístup zařízení, včetně ověřování.</span><span class="sxs-lookup"><span data-stu-id="4e161-457">Custom APIs are defined in a similar way to the table definitions and can access all the same facilities, including authentication.</span></span>

<span data-ttu-id="4e161-458">Pokud chcete použít ověřování aplikace služby s rozhraním API vlastní, musíte nakonfigurovat aplikaci služby ověřování v [portál Azure] první.</span><span class="sxs-lookup"><span data-stu-id="4e161-458">If you wish to use App Service Authentication with a Custom API, you must configure App Service Authentication in the [Azure portal] first.</span></span>  <span data-ttu-id="4e161-459">Další podrobnosti o konfiguraci ověřování ve službě Azure App Service projděte si v Průvodci konfigurace pro zprostředkovatele identity, kterou chcete použít:</span><span class="sxs-lookup"><span data-stu-id="4e161-459">For more details about configuring authentication in an Azure App Service, review the Configuration Guide for the identity provider you intend to use:</span></span>

* <span data-ttu-id="4e161-460">[Postup konfigurace ověřování Azure Active Directory]</span><span class="sxs-lookup"><span data-stu-id="4e161-460">[How to configure Azure Active Directory Authentication]</span></span>
* <span data-ttu-id="4e161-461">[Postup konfigurace ověřování Facebook]</span><span class="sxs-lookup"><span data-stu-id="4e161-461">[How to configure Facebook Authentication]</span></span>
* <span data-ttu-id="4e161-462">[Postup konfigurace ověřování Google]</span><span class="sxs-lookup"><span data-stu-id="4e161-462">[How to configure Google Authentication]</span></span>
* <span data-ttu-id="4e161-463">[Jak nakonfigurovat Microsoft Authentication]</span><span class="sxs-lookup"><span data-stu-id="4e161-463">[How to configure Microsoft Authentication]</span></span>
* <span data-ttu-id="4e161-464">[Jak nakonfigurovat ověřování služby Twitter]</span><span class="sxs-lookup"><span data-stu-id="4e161-464">[How to configure Twitter Authentication]</span></span>

<span data-ttu-id="4e161-465">Vlastní rozhraní API jsou definovány stejným způsobem jako rozhraní API tabulky.</span><span class="sxs-lookup"><span data-stu-id="4e161-465">Custom APIs are defined in much the same way as the Tables API.</span></span>

1. <span data-ttu-id="4e161-466">Vytvoření **rozhraní api** adresáře</span><span class="sxs-lookup"><span data-stu-id="4e161-466">Create an **api** directory</span></span>
2. <span data-ttu-id="4e161-467">Vytvořte soubor JavaScript definice rozhraní API v **rozhraní api** adresáře.</span><span class="sxs-lookup"><span data-stu-id="4e161-467">Create an API definition JavaScript file in the **api** directory.</span></span>
3. <span data-ttu-id="4e161-468">Použít metodu import pro import **rozhraní api** adresáře.</span><span class="sxs-lookup"><span data-stu-id="4e161-468">Use the import method to import the **api** directory.</span></span>

<span data-ttu-id="4e161-469">Zde je definice rozhraní api prototypu založeny na basic aplikace ukázku, kterou jsme použili předtím.</span><span class="sxs-lookup"><span data-stu-id="4e161-469">Here is the prototype api definition based on the basic-app sample we used earlier.</span></span>

    var express = require('express'),
        azureMobileApps = require('azure-mobile-apps');

    var app = express(),
        mobile = azureMobileApps();

    // Import the Custom API
    mobile.api.import('./api');

    // Add the mobile API so it is accessible as a Web API
    app.use(mobile);

    // Start listening on HTTP
    app.listen(process.env.PORT || 3000);

<span data-ttu-id="4e161-470">Podívejme příklad rozhraní API, která vrací data na serveru pomocí *Date.now()* metoda.</span><span class="sxs-lookup"><span data-stu-id="4e161-470">Let's take an example API that returns the server date using the *Date.now()* method.</span></span>  <span data-ttu-id="4e161-471">Tady je soubor api/date.js:</span><span class="sxs-lookup"><span data-stu-id="4e161-471">Here is the api/date.js file:</span></span>

    var api = {
        get: function (req, res, next) {
            var date = { currentTime: Date.now() };
            res.status(200).type('application/json').send(date);
        });
    };

    module.exports = api;

<span data-ttu-id="4e161-472">Každý parametr je jedním z standardní RESTful příkazy - GET, POST, PATCH nebo odstranit.</span><span class="sxs-lookup"><span data-stu-id="4e161-472">Each parameter is one of the standard RESTful verbs - GET, POST, PATCH, or DELETE.</span></span>  <span data-ttu-id="4e161-473">Metoda je standard [ExpressJS Middleware] funkce, která odesílá požadované výstup.</span><span class="sxs-lookup"><span data-stu-id="4e161-473">The method is a standard [ExpressJS Middleware] function that sends the required output.</span></span>

### <span data-ttu-id="4e161-474"><a name="howto-customapi-auth"></a>Postupy: ověřování vyžadovat pro přístup k vlastní rozhraní API</span><span class="sxs-lookup"><span data-stu-id="4e161-474"><a name="howto-customapi-auth"></a>How to: Require authentication for access to a custom API</span></span>
<span data-ttu-id="4e161-475">Azure Mobile Apps SDK implementuje ověřování stejným způsobem pro koncový bod tabulky a vlastní rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="4e161-475">Azure Mobile Apps SDK implements authentication in the same way for both the tables endpoint and custom APIs.</span></span>  <span data-ttu-id="4e161-476">Chcete-li přidat ověřování do rozhraní API vyvinuté v předchozí části, přidejte **přístup** vlastnost:</span><span class="sxs-lookup"><span data-stu-id="4e161-476">To add authentication to the API developed in the previous section, add an **access** property:</span></span>

    var api = {
        get: function (req, res, next) {
            var date = { currentTime: Date.now() };
            res.status(200).type('application/json').send(date);
        });
    };
    // All methods must be authenticated.
    api.access = 'authenticated';

    module.exports = api;

<span data-ttu-id="4e161-477">Můžete také zadat ověřování na konkrétní operace:</span><span class="sxs-lookup"><span data-stu-id="4e161-477">You can also specify authentication on specific operations:</span></span>

    var api = {
        get: function (req, res, next) {
            var date = { currentTime: Date.now() };
            res.status(200).type('application/json').send(date);
        }
    };
    // The GET methods must be authenticated.
    api.get.access = 'authenticated';

    module.exports = api;

<span data-ttu-id="4e161-478">Pro vlastní rozhraní API, které vyžadují ověřování musí použít stejný token, který se používá pro koncový bod tabulky.</span><span class="sxs-lookup"><span data-stu-id="4e161-478">The same token that is used for the tables endpoint must be used for custom APIs requiring authentication.</span></span>

### <span data-ttu-id="4e161-479"><a name="howto-customapi-auth"></a>Postupy: zpracování nahrávání velkých souborů</span><span class="sxs-lookup"><span data-stu-id="4e161-479"><a name="howto-customapi-auth"></a>How to: Handle large file uploads</span></span>
<span data-ttu-id="4e161-480">Azure Mobile Apps SDK používá [textu analyzátor middleware](https://github.com/expressjs/body-parser) přijmout a dekódování obsah textu v vaše žádost.</span><span class="sxs-lookup"><span data-stu-id="4e161-480">Azure Mobile Apps SDK uses the [body-parser middleware](https://github.com/expressjs/body-parser) to accept and decode body content in your submission.</span></span>  <span data-ttu-id="4e161-481">Můžete předkonfigurovat textu analyzátor, který má přijmout nahrávání větší souborů:</span><span class="sxs-lookup"><span data-stu-id="4e161-481">You can pre-configure body-parser to accept larger file uploads:</span></span>

    var express = require('express'),
        bodyParser = require('body-parser'),
        azureMobileApps = require('azure-mobile-apps');

    var app = express(),
        mobile = azureMobileApps();

    // Set up large body content handling
    app.use(bodyParser.json({ limit: '50mb' }));
    app.use(bodyParser.urlencoded({ limit: '50mb', extended: true }));

    // Import the Custom API
    mobile.api.import('./api');

    // Add the mobile API so it is accessible as a Web API
    app.use(mobile);

    // Start listening on HTTP
    app.listen(process.env.PORT || 3000);

<span data-ttu-id="4e161-482">Soubor je před samotným přenosem kódování base-64.</span><span class="sxs-lookup"><span data-stu-id="4e161-482">The file is base-64 encoded before transmission.</span></span>  <span data-ttu-id="4e161-483">Tím se zvyšuje velikost skutečné nahrávání (a proto velikost musíte vzít v úvahu pro).</span><span class="sxs-lookup"><span data-stu-id="4e161-483">This increases the size of the actual upload (and hence the size you must account for).</span></span>

### <span data-ttu-id="4e161-484"><a name="howto-customapi-sql"></a>Postup: provedení vlastní SQL příkazy</span><span class="sxs-lookup"><span data-stu-id="4e161-484"><a name="howto-customapi-sql"></a>How to: Execute custom SQL statements</span></span>
<span data-ttu-id="4e161-485">Azure Mobile Apps SDK umožňuje přístup k celé kontextu prostřednictvím objektu žádosti umožňuje snadno provést parametrizované příkazy SQL k poskytovateli definované datové:</span><span class="sxs-lookup"><span data-stu-id="4e161-485">The Azure Mobile Apps SDK allows access to the entire Context through the request object, allowing you to execute parameterized SQL statements to the defined data provider easily:</span></span>

    var api = {
        get: function (request, response, next) {
            // Check for parameters - if not there, pass on to a later API call
            if (typeof request.params.completed === 'undefined')
                return next();

            // Define the query - anything that can be handled by the mssql
            // driver is allowed.
            var query = {
                sql: 'UPDATE TodoItem SET complete=@completed',
                parameters: [{
                    completed: request.params.completed
                }]
            };

            // Execute the query.  The context for Azure Mobile Apps is available through
            // request.azureMobile - the data object contains the configured data provider.
            request.azureMobile.data.execute(query)
            .then(function (results) {
                response.json(results);
            });
        }
    };

    api.get.access = 'authenticated';
    module.exports = api;

## <span data-ttu-id="4e161-486"><a name="Debugging"></a>Ladění, snadno tabulek a snadno rozhraní API</span><span class="sxs-lookup"><span data-stu-id="4e161-486"><a name="Debugging"></a>Debugging, Easy Tables, and Easy APIs</span></span>
### <span data-ttu-id="4e161-487"><a name="howto-diagnostic-logs"></a>Postupy: ladění, Diagnostika a řešení potíží s Azure Mobile apps</span><span class="sxs-lookup"><span data-stu-id="4e161-487"><a name="howto-diagnostic-logs"></a>How to: Debug, diagnose, and troubleshoot Azure Mobile apps</span></span>
<span data-ttu-id="4e161-488">Azure App Service poskytuje několik ladění a řešení potíží s techniky pro aplikace Node.js.</span><span class="sxs-lookup"><span data-stu-id="4e161-488">The Azure App Service provides several debugging and troubleshooting techniques for Node.js applications.</span></span>
<span data-ttu-id="4e161-489">Naleznete v následujících článcích začít při řešení potíží váš back-end Node.js Mobile:</span><span class="sxs-lookup"><span data-stu-id="4e161-489">Refer to the following articles to get started in troubleshooting your Node.js Mobile backend:</span></span>

* <span data-ttu-id="4e161-490">[Monitorování služby Azure App Service]</span><span class="sxs-lookup"><span data-stu-id="4e161-490">[Monitoring an Azure App Service]</span></span>
* <span data-ttu-id="4e161-491">[Povolit protokolování diagnostiky v Azure App Service]</span><span class="sxs-lookup"><span data-stu-id="4e161-491">[Enable Diagnostic Logging in Azure App Service]</span></span>
* <span data-ttu-id="4e161-492">[Řešení potíží s Azure App Service v sadě Visual Studio]</span><span class="sxs-lookup"><span data-stu-id="4e161-492">[Troubleshoot an Azure App Service in Visual Studio]</span></span>

<span data-ttu-id="4e161-493">Node.js aplikací mít přístup k širokou škálu protokolů diagnostiky nástroje.</span><span class="sxs-lookup"><span data-stu-id="4e161-493">Node.js applications have access to a wide range of diagnostic log tools.</span></span>  <span data-ttu-id="4e161-494">Interně používá Azure Mobile Apps Node.js SDK [Winstona] pro protokolování diagnostiky.</span><span class="sxs-lookup"><span data-stu-id="4e161-494">Internally, the Azure Mobile Apps Node.js SDK uses [Winston] for diagnostic logging.</span></span>  <span data-ttu-id="4e161-495">Protokolování je automaticky povolen povolením režimu ladění, nebo nastavením **MS_DebugMode** nastavení aplikace nastavte na hodnotu true v [portál Azure].</span><span class="sxs-lookup"><span data-stu-id="4e161-495">Logging is automatically enabled by enabling debug mode or by setting the **MS_DebugMode** app setting to true in the [Azure portal].</span></span> <span data-ttu-id="4e161-496">Vygenerovaný protokolů se objeví v diagnostických protokolů na [portál Azure].</span><span class="sxs-lookup"><span data-stu-id="4e161-496">Generated logs appear in the Diagnostic Logs on the [Azure portal].</span></span>

### <span data-ttu-id="4e161-497"><a name="in-portal-editing"></a><a name="work-easy-tables"></a>Postupy: práce s tabulkami snadno na portálu Azure</span><span class="sxs-lookup"><span data-stu-id="4e161-497"><a name="in-portal-editing"></a><a name="work-easy-tables"></a>How to: Work with Easy Tables in the Azure portal</span></span>
<span data-ttu-id="4e161-498">Snadno tabulek v portálu umožňuje vytvářet a pracovat s tabulkami přímo na portálu.</span><span class="sxs-lookup"><span data-stu-id="4e161-498">Easy Tables in the portal let you create and work with tables right in the portal.</span></span> <span data-ttu-id="4e161-499">I můžete upravit pomocí editoru služby aplikace operace s tabulkou.</span><span class="sxs-lookup"><span data-stu-id="4e161-499">You can even edit table operations using the App Service Editor.</span></span>

<span data-ttu-id="4e161-500">Když kliknete na tlačítko **snadno tabulky** v nastavení vašeho back-end serveru, můžete přidávat, upravovat nebo odstranění tabulky.</span><span class="sxs-lookup"><span data-stu-id="4e161-500">When you click **Easy tables** in your backend site settings, you can add, modify, or delete a table.</span></span> <span data-ttu-id="4e161-501">Můžete také zobrazit data v tabulce.</span><span class="sxs-lookup"><span data-stu-id="4e161-501">You can also see data in the table.</span></span>

![Práce s tabulkami snadno](./media/app-service-mobile-node-backend-how-to-use-server-sdk/mobile-apps-easy-tables.png)

<span data-ttu-id="4e161-503">Na panelu příkazů pro tabulku k dispozici jsou následující příkazy:</span><span class="sxs-lookup"><span data-stu-id="4e161-503">The following commands are available on the command bar for a table:</span></span>

* <span data-ttu-id="4e161-504">**Změna oprávnění** – změnit oprávnění pro čtení, vložit, aktualizovat a odstranit operací v tabulce.</span><span class="sxs-lookup"><span data-stu-id="4e161-504">**Change permissions** - modify the permission for read, insert, update and delete operations on the table.</span></span>
  <span data-ttu-id="4e161-505">Možnosti jsou k povolení anonymního přístupu, vyžadují ověřování, nebo zakázat veškerý přístup k operaci.</span><span class="sxs-lookup"><span data-stu-id="4e161-505">Options are to allow anonymous access, to require authentication, or to disable all access to the operation.</span></span>
* <span data-ttu-id="4e161-506">**Upravte skript** -souboru skriptu pro tabulku je otevřen v editoru služby aplikace.</span><span class="sxs-lookup"><span data-stu-id="4e161-506">**Edit script** - the script file for the table is opened in the App Service Editor.</span></span>
* <span data-ttu-id="4e161-507">**Správa schématu** – přidání nebo odstranění sloupce nebo změna tabulky indexu.</span><span class="sxs-lookup"><span data-stu-id="4e161-507">**Manage schema** - add or delete columns or change the table index.</span></span>
* <span data-ttu-id="4e161-508">**Odstranit tabulku** -zkrátí existující tabulky se odstraňuje všechny řádky dat, ale ponechat schématu beze změny.</span><span class="sxs-lookup"><span data-stu-id="4e161-508">**Clear table** - truncates an existing table be deleting all data rows but leaving the schema unchanged.</span></span>
* <span data-ttu-id="4e161-509">**Odstranit řádky** -odstranit jednotlivé řádky dat.</span><span class="sxs-lookup"><span data-stu-id="4e161-509">**Delete rows** - delete individual rows of data.</span></span>
* <span data-ttu-id="4e161-510">**Protokoly streamování zobrazení** -vás spojí se služba streamování protokolu pro váš web.</span><span class="sxs-lookup"><span data-stu-id="4e161-510">**View streaming logs** - connects you to the streaming log service for your site.</span></span>

### <span data-ttu-id="4e161-511"><a name="work-easy-apis"></a>Postupy: práce s snadno rozhraní API na portálu Azure</span><span class="sxs-lookup"><span data-stu-id="4e161-511"><a name="work-easy-apis"></a>How to: Work with Easy APIs in the Azure portal</span></span>
<span data-ttu-id="4e161-512">Snadno rozhraní API na portálu umožňuje vytvářet a pracovat s vlastní rozhraní API přímo na portálu.</span><span class="sxs-lookup"><span data-stu-id="4e161-512">Easy APIs in the portal let you create and work with custom APIs right in the portal.</span></span> <span data-ttu-id="4e161-513">Můžete upravit skripty rozhraní API pomocí editoru služby aplikace.</span><span class="sxs-lookup"><span data-stu-id="4e161-513">You can edit API scripts using the App Service Editor.</span></span>

<span data-ttu-id="4e161-514">Když kliknete na tlačítko **rozhraní API pro snadný** v nastavení vašeho back-end serveru, můžete přidávat, upravovat nebo odstranit vlastní koncový bod rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="4e161-514">When you click **Easy APIs** in your backend site settings, you can add, modify, or delete a custom API endpoint.</span></span>

![Práci s rozhraními API snadno](./media/app-service-mobile-node-backend-how-to-use-server-sdk/mobile-apps-easy-apis.png)

<span data-ttu-id="4e161-516">Na portálu můžete změnit přístupová oprávnění pro danou akci HTTP, upravte soubor skriptu rozhraní API v editoru služby aplikace nebo zobrazení protokolů streamování.</span><span class="sxs-lookup"><span data-stu-id="4e161-516">In the portal, you can change the access permissions for a given HTTP action, edit the API script file in the App Service Editor, or view the streaming logs.</span></span>

### <span data-ttu-id="4e161-517"><a name="online-editor"></a>Postupy: úpravy kódu v editoru služby aplikace</span><span class="sxs-lookup"><span data-stu-id="4e161-517"><a name="online-editor"></a>How to: Edit code in the App Service Editor</span></span>
<span data-ttu-id="4e161-518">Portál Azure umožňuje upravit soubory skriptu back-end Node.js v editoru služby aplikace bez nutnosti stáhnout projektu do místního počítače.</span><span class="sxs-lookup"><span data-stu-id="4e161-518">The Azure portal lets you edit your Node.js backend script files in the App Service Editor without having to download the project to your local computer.</span></span> <span data-ttu-id="4e161-519">Postup úpravy souborů skriptů v editoru online:</span><span class="sxs-lookup"><span data-stu-id="4e161-519">To edit script files in the online editor:</span></span>

1. <span data-ttu-id="4e161-520">V okně back-end mobilní aplikace, klikněte na tlačítko **všechna nastavení** > buď **snadno tabulky** nebo **rozhraní API pro snadný**, klikněte na tabulku nebo rozhraní API a pak klikněte na tlačítko **upravte skript**.</span><span class="sxs-lookup"><span data-stu-id="4e161-520">In your Mobile App backend blade, click **All settings** > either **Easy tables** or **Easy APIs**, click a table or API, then click **Edit script**.</span></span> <span data-ttu-id="4e161-521">Soubor skriptu je otevřen v editoru služby aplikace.</span><span class="sxs-lookup"><span data-stu-id="4e161-521">The script file is opened in the App Service Editor.</span></span>

    ![Editor služby aplikace](./media/app-service-mobile-node-backend-how-to-use-server-sdk/mobile-apps-visual-studio-editor.png)
2. <span data-ttu-id="4e161-523">Provedené změny do souboru kódu v editoru online.</span><span class="sxs-lookup"><span data-stu-id="4e161-523">Make your changes to the code file in the online editor.</span></span> <span data-ttu-id="4e161-524">Změny se při psaní automaticky uloží.</span><span class="sxs-lookup"><span data-stu-id="4e161-524">Changes are saved automatically as you type.</span></span>

<!-- Images -->
[0]: ./media/app-service-mobile-node-backend-how-to-use-server-sdk/npm-init.png
[1]: ./media/app-service-mobile-node-backend-how-to-use-server-sdk/vs2015-new-project.png
[2]: ./media/app-service-mobile-node-backend-how-to-use-server-sdk/vs2015-install-npm.png
[3]: ./media/app-service-mobile-node-backend-how-to-use-server-sdk/sqlexpress-config.png
[4]: ./media/app-service-mobile-node-backend-how-to-use-server-sdk/sqlexpress-authconfig.png
[5]: ./media/app-service-mobile-node-backend-how-to-use-server-sdk/sqlexpress-newuser-1.png
[6]: ./media/app-service-mobile-node-backend-how-to-use-server-sdk/dotnet-backend-create-db.png

<!-- URLs -->
<span data-ttu-id="4e161-525">[Rychlý start Android klienta]: app-service-mobile-android-get-started.md</span><span class="sxs-lookup"><span data-stu-id="4e161-525">[Android Client QuickStart]: app-service-mobile-android-get-started.md</span></span>
<span data-ttu-id="4e161-526">[Rychlý start Apache Cordova klienta]: app-service-mobile-cordova-get-started.md</span><span class="sxs-lookup"><span data-stu-id="4e161-526">[Apache Cordova Client QuickStart]: app-service-mobile-cordova-get-started.md</span></span>
<span data-ttu-id="4e161-527">[iOS klienta rychlý start]: app-service-mobile-ios-get-started.md</span><span class="sxs-lookup"><span data-stu-id="4e161-527">[iOS Client QuickStart]: app-service-mobile-ios-get-started.md</span></span>
<span data-ttu-id="4e161-528">[Rychlý start Xamarin.iOS klienta]: app-service-mobile-xamarin-ios-get-started.md</span><span class="sxs-lookup"><span data-stu-id="4e161-528">[Xamarin.iOS Client QuickStart]: app-service-mobile-xamarin-ios-get-started.md</span></span>
<span data-ttu-id="4e161-529">[Rychlé spuštění klienta Xamarin.Android]: app-service-mobile-xamarin-android-get-started.md</span><span class="sxs-lookup"><span data-stu-id="4e161-529">[Xamarin.Android Client QuickStart]: app-service-mobile-xamarin-android-get-started.md</span></span>
<span data-ttu-id="4e161-530">[Rychlý start Xamarin.Forms klienta]: app-service-mobile-xamarin-forms-get-started.md</span><span class="sxs-lookup"><span data-stu-id="4e161-530">[Xamarin.Forms Client QuickStart]: app-service-mobile-xamarin-forms-get-started.md</span></span>
<span data-ttu-id="4e161-531">[Rychlé spuštění klienta Windows Store]: app-service-mobile-windows-store-dotnet-get-started.md</span><span class="sxs-lookup"><span data-stu-id="4e161-531">[Windows Store Client QuickStart]: app-service-mobile-windows-store-dotnet-get-started.md</span></span>
[HTML/Javascript Client QuickStart]: app-service-html-get-started.md
<span data-ttu-id="4e161-532">[synchronizaci dat offline]: app-service-mobile-offline-data-sync.md</span><span class="sxs-lookup"><span data-stu-id="4e161-532">[offline data sync]: app-service-mobile-offline-data-sync.md</span></span>
<span data-ttu-id="4e161-533">[Postup konfigurace ověřování Azure Active Directory]: app-service-mobile-how-to-configure-active-directory-authentication.md</span><span class="sxs-lookup"><span data-stu-id="4e161-533">[How to configure Azure Active Directory Authentication]: app-service-mobile-how-to-configure-active-directory-authentication.md</span></span>
<span data-ttu-id="4e161-534">[Postup konfigurace ověřování Facebook]: app-service-mobile-how-to-configure-facebook-authentication.md</span><span class="sxs-lookup"><span data-stu-id="4e161-534">[How to configure Facebook Authentication]: app-service-mobile-how-to-configure-facebook-authentication.md</span></span>
<span data-ttu-id="4e161-535">[Postup konfigurace ověřování Google]: app-service-mobile-how-to-configure-google-authentication.md</span><span class="sxs-lookup"><span data-stu-id="4e161-535">[How to configure Google Authentication]: app-service-mobile-how-to-configure-google-authentication.md</span></span>
<span data-ttu-id="4e161-536">[Jak nakonfigurovat Microsoft Authentication]: app-service-mobile-how-to-configure-microsoft-authentication.md</span><span class="sxs-lookup"><span data-stu-id="4e161-536">[How to configure Microsoft Authentication]: app-service-mobile-how-to-configure-microsoft-authentication.md</span></span>
<span data-ttu-id="4e161-537">[Jak nakonfigurovat ověřování služby Twitter]: app-service-mobile-how-to-configure-twitter-authentication.md</span><span class="sxs-lookup"><span data-stu-id="4e161-537">[How to configure Twitter Authentication]: app-service-mobile-how-to-configure-twitter-authentication.md</span></span>
<span data-ttu-id="4e161-538">[Příručka pro nasazení Azure App Service]: ../app-service-web/web-sites-deploy.md</span><span class="sxs-lookup"><span data-stu-id="4e161-538">[Azure App Service Deployment Guide]: ../app-service-web/web-sites-deploy.md</span></span>
<span data-ttu-id="4e161-539">[Monitorování služby Azure App Service]: ../app-service-web/web-sites-monitor.md</span><span class="sxs-lookup"><span data-stu-id="4e161-539">[Monitoring an Azure App Service]: ../app-service-web/web-sites-monitor.md</span></span>
<span data-ttu-id="4e161-540">[Povolit protokolování diagnostiky v Azure App Service]: ../app-service-web/web-sites-enable-diagnostic-log.md</span><span class="sxs-lookup"><span data-stu-id="4e161-540">[Enable Diagnostic Logging in Azure App Service]: ../app-service-web/web-sites-enable-diagnostic-log.md</span></span>
<span data-ttu-id="4e161-541">[Řešení potíží s Azure App Service v sadě Visual Studio]: ../app-service-web/web-sites-dotnet-troubleshoot-visual-studio.md</span><span class="sxs-lookup"><span data-stu-id="4e161-541">[Troubleshoot an Azure App Service in Visual Studio]: ../app-service-web/web-sites-dotnet-troubleshoot-visual-studio.md</span></span>
<span data-ttu-id="4e161-542">[zadejte verzi uzlu]: ../nodejs-specify-node-version-azure-apps.md</span><span class="sxs-lookup"><span data-stu-id="4e161-542">[specify the Node Version]: ../nodejs-specify-node-version-azure-apps.md</span></span>
<span data-ttu-id="4e161-543">[použijte moduly uzlu]: ../nodejs-use-node-modules-azure-apps.md</span><span class="sxs-lookup"><span data-stu-id="4e161-543">[use Node modules]: ../nodejs-use-node-modules-azure-apps.md</span></span>
[Create a new Azure App Service]: ../app-service-web/
[azure-mobile-apps]: https://www.npmjs.com/package/azure-mobile-apps
<span data-ttu-id="4e161-544">[Express]: http://expressjs.com/</span><span class="sxs-lookup"><span data-stu-id="4e161-544">[Express]: http://expressjs.com/</span></span>
<span data-ttu-id="4e161-545">[Swagger]: http://swagger.io/</span><span class="sxs-lookup"><span data-stu-id="4e161-545">[Swagger]: http://swagger.io/</span></span>

<span data-ttu-id="4e161-546">[portál Azure]: https://portal.azure.com/</span><span class="sxs-lookup"><span data-stu-id="4e161-546">[Azure portal]: https://portal.azure.com/</span></span>
<span data-ttu-id="4e161-547">[OData]: http://www.odata.org</span><span class="sxs-lookup"><span data-stu-id="4e161-547">[OData]: http://www.odata.org</span></span>
<span data-ttu-id="4e161-548">[Promise]: https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise</span><span class="sxs-lookup"><span data-stu-id="4e161-548">[Promise]: https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise</span></span>
<span data-ttu-id="4e161-549">[basicapp ukázce na Githubu]: https://github.com/azure/azure-mobile-apps-node/tree/master/samples/basic-app</span><span class="sxs-lookup"><span data-stu-id="4e161-549">[basicapp sample on GitHub]: https://github.com/azure/azure-mobile-apps-node/tree/master/samples/basic-app</span></span>
<span data-ttu-id="4e161-550">[todo ukázce na Githubu]: https://github.com/azure/azure-mobile-apps-node/tree/master/samples/todo</span><span class="sxs-lookup"><span data-stu-id="4e161-550">[todo sample on GitHub]: https://github.com/azure/azure-mobile-apps-node/tree/master/samples/todo</span></span>
<span data-ttu-id="4e161-551">[ukázky adresáře na Githubu]: https://github.com/azure/azure-mobile-apps-node/tree/master/samples</span><span class="sxs-lookup"><span data-stu-id="4e161-551">[samples directory on GitHub]: https://github.com/azure/azure-mobile-apps-node/tree/master/samples</span></span>
[static-schema sample on GitHub]: https://github.com/azure/azure-mobile-apps-node/tree/master/samples/static-schema
<span data-ttu-id="4e161-552">[QueryJS]: https://github.com/Azure/queryjs</span><span class="sxs-lookup"><span data-stu-id="4e161-552">[QueryJS]: https://github.com/Azure/queryjs</span></span>
<span data-ttu-id="4e161-553">[Node.js Tools 1.1 pro sadu Visual Studio]: https://github.com/Microsoft/nodejstools/releases/tag/v1.1-RC.2.1</span><span class="sxs-lookup"><span data-stu-id="4e161-553">[Node.js Tools 1.1 for Visual Studio]: https://github.com/Microsoft/nodejstools/releases/tag/v1.1-RC.2.1</span></span>
<span data-ttu-id="4e161-554">[mssql Node.js balíček]: https://www.npmjs.com/package/mssql</span><span class="sxs-lookup"><span data-stu-id="4e161-554">[mssql Node.js package]: https://www.npmjs.com/package/mssql</span></span>
<span data-ttu-id="4e161-555">[Microsoft SQL Server 2014 Express]: http://www.microsoft.com/en-us/server-cloud/Products/sql-server-editions/sql-server-express.aspx</span><span class="sxs-lookup"><span data-stu-id="4e161-555">[Microsoft SQL Server 2014 Express]: http://www.microsoft.com/en-us/server-cloud/Products/sql-server-editions/sql-server-express.aspx</span></span>
<span data-ttu-id="4e161-556">[ExpressJS Middleware]: http://expressjs.com/guide/using-middleware.html</span><span class="sxs-lookup"><span data-stu-id="4e161-556">[ExpressJS Middleware]: http://expressjs.com/guide/using-middleware.html</span></span>
<span data-ttu-id="4e161-557">[Winstona]: https://github.com/winstonjs/winston</span><span class="sxs-lookup"><span data-stu-id="4e161-557">[Winston]: https://github.com/winstonjs/winston</span></span>
