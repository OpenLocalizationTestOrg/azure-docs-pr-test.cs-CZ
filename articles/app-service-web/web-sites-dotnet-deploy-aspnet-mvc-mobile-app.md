---
title: "Nasazení mobilní webové aplikace ASP.NET MVC 5 v Azure App Service"
description: "Kurz, který se naučíte, jak nasadit webovou aplikaci do služby Azure App Service pomocí funkce mobilní webové aplikace ASP.NET MVC 5."
services: app-service
documentationcenter: .net
author: cephalin
manager: erikre
editor: jimbe
ms.assetid: 0752c802-8609-4956-a755-686116913645
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 01/12/2016
ms.author: cephalin
ms.openlocfilehash: c98e9b485c52a82e5be5c0f6b0b67912d1e890b9
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="deploy-an-aspnet-mvc-5-mobile-web-app-in-azure-app-service"></a><span data-ttu-id="6c96a-103">Nasazení mobilní webové aplikace ASP.NET MVC 5 v Azure App Service</span><span class="sxs-lookup"><span data-stu-id="6c96a-103">Deploy an ASP.NET MVC 5 mobile web app in Azure App Service</span></span>
<span data-ttu-id="6c96a-104">V tomto kurzu naučit základní informace o tom, jak sestavit webové aplikace ASP.NET MVC 5, který je mobilní zařízení a nasadíte ho do Azure App Service.</span><span class="sxs-lookup"><span data-stu-id="6c96a-104">This tutorial will teach you the basics of how to build an ASP.NET MVC 5 web app that is mobile-friendly and deploy it to Azure App Service.</span></span> <span data-ttu-id="6c96a-105">V tomto kurzu budete potřebovat [Visual Studio Express 2013 pro Web] [ Visual Studio Express 2013] nebo edice sady Visual Studio, pokud již máte, professional.</span><span class="sxs-lookup"><span data-stu-id="6c96a-105">For this tutorial, you need [Visual Studio Express 2013 for Web][Visual Studio Express 2013] or the professional edition of Visual Studio if you already have that.</span></span> <span data-ttu-id="6c96a-106">Můžete použít [Visual Studio 2015] , ale snímky obrazovky se bude lišit a je nutné použít šablony ASP.NET 4.x.</span><span class="sxs-lookup"><span data-stu-id="6c96a-106">You can use [Visual Studio 2015] but the screen shots will be different and you must use the ASP.NET 4.x templates.</span></span>

[!INCLUDE [create-account-and-websites-note](../../includes/create-account-and-websites-note.md)]

## <a name="what-youll-build"></a><span data-ttu-id="6c96a-107">Co budete sestavení</span><span class="sxs-lookup"><span data-stu-id="6c96a-107">What You'll Build</span></span>
<span data-ttu-id="6c96a-108">V tomto kurzu přidáte mobilní funkce pro jednoduchou aplikaci seznamu konference, která je součástí [starter projektu][StarterProject].</span><span class="sxs-lookup"><span data-stu-id="6c96a-108">For this tutorial, you'll add mobile features to the simple conference-listing application that's provided in the [starter project][StarterProject].</span></span> <span data-ttu-id="6c96a-109">Následující snímek obrazovky ukazuje relace ASP.NET v hotová aplikace, jak je vidět v emulátoru prohlížeče v Internet Exploreru 11 F12 nástroje pro vývojáře.</span><span class="sxs-lookup"><span data-stu-id="6c96a-109">The following screenshot shows the ASP.NET sessions in the completed application, as seen in the browser emulator in Internet Explorer 11 F12 developer tools.</span></span>

![][FixedSessionsByTag]

<span data-ttu-id="6c96a-110">Můžete použít nástroje pro vývojáře Internet Explorer 11 F12 a [nástroj Fiddler] [ Fiddler] pomoci při ladění aplikace.</span><span class="sxs-lookup"><span data-stu-id="6c96a-110">You can use the Internet Explorer 11 F12 developer tools and the [Fiddler tool][Fiddler] to help debug your application.</span></span> 

## <a name="skills-youll-learn"></a><span data-ttu-id="6c96a-111">Dovedností, které se dozvíte</span><span class="sxs-lookup"><span data-stu-id="6c96a-111">Skills You'll Learn</span></span>
<span data-ttu-id="6c96a-112">Zde je, co se dozvíte:</span><span class="sxs-lookup"><span data-stu-id="6c96a-112">Here's what you'll learn:</span></span>

* <span data-ttu-id="6c96a-113">Postup publikování webové aplikace přímo do webové aplikace v Azure App Service pomocí sady Visual Studio 2013.</span><span class="sxs-lookup"><span data-stu-id="6c96a-113">How to use Visual Studio 2013 to publish your web application directly to a web app in Azure App Service.</span></span>
* <span data-ttu-id="6c96a-114">Jak šablony ASP.NET MVC 5 pomocí rozhraní šablon stylů CSS Bootstrap zlepšit zobrazení na mobilních zařízeních</span><span class="sxs-lookup"><span data-stu-id="6c96a-114">How the ASP.NET MVC 5 templates use the CSS Bootstrap framework to improve display on mobile devices</span></span>
* <span data-ttu-id="6c96a-115">Postup vytvoření zobrazení mobile specifické pro konkrétní mobilní prohlížeče, jako je iPhone a Android</span><span class="sxs-lookup"><span data-stu-id="6c96a-115">How to create mobile-specific views to target specific mobile browsers, such as the iPhone and Android</span></span>
* <span data-ttu-id="6c96a-116">Postup vytvoření přizpůsobivý zobrazení (zobrazení, které reagují na různé prohlížeče na zařízeních)</span><span class="sxs-lookup"><span data-stu-id="6c96a-116">How to create responsive views (views that respond to different browsers across devices)</span></span>

## <a name="set-up-the-development-environment"></a><span data-ttu-id="6c96a-117">Nastavení vývojového prostředí</span><span class="sxs-lookup"><span data-stu-id="6c96a-117">Set up the development environment</span></span>
<span data-ttu-id="6c96a-118">Nastavení vývojového prostředí instalací sady Azure SDK pro .NET 2.5.1 nebo novější.</span><span class="sxs-lookup"><span data-stu-id="6c96a-118">Set up your development environment by installing the Azure SDK for .NET 2.5.1 or later.</span></span> 

1. <span data-ttu-id="6c96a-119">Chcete-li nainstalovat sadu Azure SDK pro .NET, klikněte na níže uvedený odkaz.</span><span class="sxs-lookup"><span data-stu-id="6c96a-119">To install the Azure SDK for .NET, click the link below.</span></span> <span data-ttu-id="6c96a-120">Pokud nemáte dosud nainstalován Visual Studio 2013, bude nainstalována ve odkaz.</span><span class="sxs-lookup"><span data-stu-id="6c96a-120">If you don't have Visual Studio 2013 installed yet, it will be installed by the link.</span></span> <span data-ttu-id="6c96a-121">Tento kurz vyžaduje Visual Studio 2013.</span><span class="sxs-lookup"><span data-stu-id="6c96a-121">This tutorial requires Visual Studio 2013.</span></span> <span data-ttu-id="6c96a-122">[Azure SDK pro Visual Studio 2013][AzureSDKVs2013]</span><span class="sxs-lookup"><span data-stu-id="6c96a-122">[Azure SDK for Visual Studio 2013][AzureSDKVs2013]</span></span>
2. <span data-ttu-id="6c96a-123">V okně webové platformy, klikněte na **nainstalovat** a pokračujte v instalaci.</span><span class="sxs-lookup"><span data-stu-id="6c96a-123">In the Web Platform Installer window, click **Install** and proceed with the installation.</span></span>

<span data-ttu-id="6c96a-124">Budete také potřebovat emulátoru prohlížeč pro mobilní zařízení.</span><span class="sxs-lookup"><span data-stu-id="6c96a-124">You will also need a mobile browser emulator.</span></span> <span data-ttu-id="6c96a-125">Bude fungovat některé z následujících:</span><span class="sxs-lookup"><span data-stu-id="6c96a-125">Any of the following will work:</span></span>

* <span data-ttu-id="6c96a-126">Emulátor prohlížeče v [nástroje pro vývojáře aplikace Internet Explorer 11 F12] [ EmulatorIE11] (používá se v všechny snímky obrazovky prohlížeč pro mobilní zařízení).</span><span class="sxs-lookup"><span data-stu-id="6c96a-126">Browser Emulator in [Internet Explorer 11 F12 developer tools][EmulatorIE11] (used in all mobile browser screenshots).</span></span> <span data-ttu-id="6c96a-127">Má přednastavení řetězec uživatelského agenta pro Windows Phone 8, Windows Phone 7 a Apple iPad.</span><span class="sxs-lookup"><span data-stu-id="6c96a-127">It has user agent string presets for Windows Phone 8, Windows Phone 7, and Apple iPad.</span></span>
* <span data-ttu-id="6c96a-128">Emulátor prohlížeče v [Google Chrome DevTools][EmulatorChrome].</span><span class="sxs-lookup"><span data-stu-id="6c96a-128">Browser Emulator in [Google Chrome DevTools][EmulatorChrome].</span></span> <span data-ttu-id="6c96a-129">Obsahuje přednastavení množství zařízení se systémem Android, a také Apple iPhone, Apple iPad a Amazon Kindle ještě efektivněji.</span><span class="sxs-lookup"><span data-stu-id="6c96a-129">It contains presets for numerous Android devices, as well as Apple iPhone, Apple iPad, and Amazon Kindle Fire.</span></span> <span data-ttu-id="6c96a-130">Emuluje také touch události.</span><span class="sxs-lookup"><span data-stu-id="6c96a-130">It also emulates touch events.</span></span>
* <span data-ttu-id="6c96a-131">[Emulátoru mobilního Opera][EmulatorOpera]</span><span class="sxs-lookup"><span data-stu-id="6c96a-131">[Opera Mobile Emulator][EmulatorOpera]</span></span>

<span data-ttu-id="6c96a-132">Projekty Visual Studio s C\# zdrojového kódu jsou k dispozici v tomto tématu:</span><span class="sxs-lookup"><span data-stu-id="6c96a-132">Visual Studio projects with C\# source code are available to accompany this topic:</span></span>

* <span data-ttu-id="6c96a-133">[Stažení Starter projektu][StarterProject]</span><span class="sxs-lookup"><span data-stu-id="6c96a-133">[Starter project download][StarterProject]</span></span>
* <span data-ttu-id="6c96a-134">[Dokončení stažení projektu][CompletedProject]</span><span class="sxs-lookup"><span data-stu-id="6c96a-134">[Completed project download][CompletedProject]</span></span>

## <span data-ttu-id="6c96a-135"><a name="bkmk_DeployStarterProject"></a>Nasazení projektu starter do webové aplikace Azure</span><span class="sxs-lookup"><span data-stu-id="6c96a-135"><a name="bkmk_DeployStarterProject"></a>Deploy the starter project to an Azure web app</span></span>
1. <span data-ttu-id="6c96a-136">Stažení aplikace konferenční výpis [starter projektu][StarterProject].</span><span class="sxs-lookup"><span data-stu-id="6c96a-136">Download the conference-listing application [starter project][StarterProject].</span></span>
2. <span data-ttu-id="6c96a-137">Potom v Průzkumníku Windows, klikněte pravým tlačítkem na stažený soubor ZIP a zvolte *vlastnosti*.</span><span class="sxs-lookup"><span data-stu-id="6c96a-137">Then in Windows Explorer, right-click the downloaded ZIP file and choose *Properties*.</span></span>
3. <span data-ttu-id="6c96a-138">V **vlastnosti** dialogovém okně vyberte **Odblokovat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="6c96a-138">In the **Properties** dialog box, choose the **Unblock** button.</span></span> <span data-ttu-id="6c96a-139">(Odblokování brání upozornění zabezpečení, která nastane, když se pokusíte použít *.zip* souboru, který jste stáhli z webu.)</span><span class="sxs-lookup"><span data-stu-id="6c96a-139">(Unblocking prevents a security warning that occurs when you try to use a *.zip* file that you've downloaded from the web.)</span></span>
4. <span data-ttu-id="6c96a-140">Klikněte pravým tlačítkem na soubor ZIP a vyberte **Extrahovat vše** soubor rozbalit.</span><span class="sxs-lookup"><span data-stu-id="6c96a-140">Right-click the ZIP file and select **Extract All** to unzip the file.</span></span> 
5. <span data-ttu-id="6c96a-141">V sadě Visual Studio, otevřete *C#\Mvc5Mobile.sln* souboru.</span><span class="sxs-lookup"><span data-stu-id="6c96a-141">In Visual Studio, open the *C#\Mvc5Mobile.sln* file.</span></span>
6. <span data-ttu-id="6c96a-142">V Průzkumníku řešení klikněte pravým tlačítkem na projekt a klikněte na tlačítko **publikovat**.</span><span class="sxs-lookup"><span data-stu-id="6c96a-142">In Solution Explorer, right-click the project and click **Publish**.</span></span>
   
   ![][DeployClickPublish]
7. <span data-ttu-id="6c96a-143">V Publikovat Web, klikněte na **Microsoft Azure App Service**.</span><span class="sxs-lookup"><span data-stu-id="6c96a-143">In Publish Web, click **Microsoft Azure App Service**.</span></span>
   
   ![][DeployClickWebSites]
8. <span data-ttu-id="6c96a-144">Pokud již jste přihlášeni do Azure, klikněte na tlačítko **přidat účet**.</span><span class="sxs-lookup"><span data-stu-id="6c96a-144">If you haven't already logged into Azure, click **Add an account**.</span></span>
   
   ![][DeploySignIn]
9. <span data-ttu-id="6c96a-145">Postupujte podle pokynů pro přihlášení k účtu Azure.</span><span class="sxs-lookup"><span data-stu-id="6c96a-145">Follow the prompts to log into your Azure account.</span></span>
10. <span data-ttu-id="6c96a-146">Dialogovém okně App Service, by měl nyní může zobrazit můžete jako přihlášení.</span><span class="sxs-lookup"><span data-stu-id="6c96a-146">The App Service dialog should now show you as signed in.</span></span> <span data-ttu-id="6c96a-147">Klikněte na možnost **Nové**.</span><span class="sxs-lookup"><span data-stu-id="6c96a-147">Click **New**.</span></span>
    
    ![][DeployNewWebsite]  
11. <span data-ttu-id="6c96a-148">V **název webové aplikace** pole, zadejte předponu aplikace jedinečný název.</span><span class="sxs-lookup"><span data-stu-id="6c96a-148">In the **Web App Name** field, specify a unique app name prefix.</span></span> <span data-ttu-id="6c96a-149">Váš název plně kvalifikovaný webové aplikace bude  *&lt;předpony >*. azurewebsites.net.</span><span class="sxs-lookup"><span data-stu-id="6c96a-149">Your fully-qualified web app name will be *&lt;prefix>*.azurewebsites.net.</span></span> <span data-ttu-id="6c96a-150">Také vyberte nebo zadejte nový název skupiny prostředků v **skupiny prostředků**.</span><span class="sxs-lookup"><span data-stu-id="6c96a-150">Also, select or specify a new resource group name in **Resource group**.</span></span> <span data-ttu-id="6c96a-151">Potom klikněte na **nový** vytvořit nový plán aplikační služby.</span><span class="sxs-lookup"><span data-stu-id="6c96a-151">Then, click **New** to create a new App Service plan.</span></span>
    
    ![][DeploySiteSettings]
12. <span data-ttu-id="6c96a-152">Nakonfigurujte nový plán aplikační služby a klikněte na **OK**.</span><span class="sxs-lookup"><span data-stu-id="6c96a-152">Configure the new App Service plan and click **OK**.</span></span> 
    
    ![](./media/web-sites-dotnet-deploy-aspnet-mvc-mobile-app/deploy-to-azure-website-7a.png)
13. <span data-ttu-id="6c96a-153">Zpět v dialogovém okně vytvořit službu App Service, klikněte na tlačítko **vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="6c96a-153">Back in the Create App Service dialog, click **Create**.</span></span>
    
    ![](./media/web-sites-dotnet-deploy-aspnet-mvc-mobile-app/deploy-to-azure-website-7b.png) 
14. <span data-ttu-id="6c96a-154">Po Azure prostředky jsou vytvořeny, Publikovat Web dialogové okno bude vyplněn nastavení pro novou aplikaci.</span><span class="sxs-lookup"><span data-stu-id="6c96a-154">After the Azure resources are created, the Publish Web dialog will be filled with the settings for your new app.</span></span> <span data-ttu-id="6c96a-155">Klikněte na **Publikovat**.</span><span class="sxs-lookup"><span data-stu-id="6c96a-155">Click **Publish**.</span></span>
    
    ![][DeployPublishSite]
    
    <span data-ttu-id="6c96a-156">Jakmile sady Visual Studio dokončí publikování starter projektu do webové aplikace Azure, otevře prohlížeč pro stolní počítač se živou webovou aplikaci.</span><span class="sxs-lookup"><span data-stu-id="6c96a-156">Once Visual Studio finishes publishing the starter project to the Azure web app, the desktop browser opens to display the live web app.</span></span>
15. <span data-ttu-id="6c96a-157">Spusťte emulátor váš prohlížeč pro mobilní zařízení, zkopírujte adresu URL pro aplikaci konferenční (*<prefix>*. azurewebsites.net) do emulátoru a pak klikněte na tlačítko pravém horním a vyberte **Procházet podle značky**.</span><span class="sxs-lookup"><span data-stu-id="6c96a-157">Start your mobile browser emulator, copy the URL for the conference application (*<prefix>*.azurewebsites.net) into the emulator, and then click the top-right button and select **Browse by tag**.</span></span> <span data-ttu-id="6c96a-158">Pokud používáte Internet Explorer 11 jako výchozího prohlížeče, bude třeba jen na typ `F12`, pak `Ctrl+8`a poté změňte prohlížeče profil, který se **Windows Phone**.</span><span class="sxs-lookup"><span data-stu-id="6c96a-158">If you are using Internet Explorer 11 as the default browser, you just need to type `F12`, then `Ctrl+8`, and then change the browser profile to **Windows Phone**.</span></span> <span data-ttu-id="6c96a-159">Obrázek níže znázorňuje *AllTags* zobrazení v režimu na výšku (z výběru **Procházet podle značky**).</span><span class="sxs-lookup"><span data-stu-id="6c96a-159">The image below shows the *AllTags* view in portrait mode (from choosing **Browse by tag**).</span></span>
    
    ![][AllTags]

> [!TIP]
> <span data-ttu-id="6c96a-160">Zatímco lze ladit aplikace MVC 5 v sadě Visual Studio, můžete publikovat webovou aplikaci do Azure znovu k ověření živou webovou aplikaci přímo z prohlížeče emulátoru nebo prohlížeč pro mobilní zařízení.</span><span class="sxs-lookup"><span data-stu-id="6c96a-160">While you can debug your MVC 5 application from within Visual Studio, you can publish your web app to Azure again to verify the live web app directly from your mobile browser or a browser emulator.</span></span>
> 
> 

<span data-ttu-id="6c96a-161">Zobrazení je velmi čitelná na mobilním zařízení.</span><span class="sxs-lookup"><span data-stu-id="6c96a-161">The display is very readable on a mobile device.</span></span> <span data-ttu-id="6c96a-162">Můžete také již zobrazit některé vizuální efekty použít rámcem Bootstrap šablon stylů CSS.</span><span class="sxs-lookup"><span data-stu-id="6c96a-162">You can also already see some of the visual effects applied by the Bootstrap CSS framework.</span></span>
<span data-ttu-id="6c96a-163">Klikněte **ASP.NET** odkaz.</span><span class="sxs-lookup"><span data-stu-id="6c96a-163">Click the **ASP.NET** link.</span></span>

![][SessionsByTagASP.NET]

<span data-ttu-id="6c96a-164">Zobrazení značek ASP.NET je přiblížení namontováno k obrazovce, která zajišťuje Bootstrap pro vás automaticky.</span><span class="sxs-lookup"><span data-stu-id="6c96a-164">The ASP.NET tag view is zoom-fitted to the screen, which Bootstrap does for you automatically.</span></span> <span data-ttu-id="6c96a-165">Však může zvýšit toto zobrazení tak, aby lépe vyhovoval prohlížeč pro mobilní zařízení.</span><span class="sxs-lookup"><span data-stu-id="6c96a-165">However, you can improve this view to better suit the mobile browser.</span></span> <span data-ttu-id="6c96a-166">Například **datum** sloupec je obtížné číst.</span><span class="sxs-lookup"><span data-stu-id="6c96a-166">For example, the **Date** column is difficult to read.</span></span> <span data-ttu-id="6c96a-167">Později v tomto kurzu budete změníte *AllTags* zobrazení, aby bylo mobilní zařízení.</span><span class="sxs-lookup"><span data-stu-id="6c96a-167">Later in the tutorial you'll change the *AllTags* view to make it mobile-friendly.</span></span>

## <span data-ttu-id="6c96a-168"><a name="bkmk_bootstrap"></a>Framework Bootstrap šablon stylů CSS</span><span class="sxs-lookup"><span data-stu-id="6c96a-168"><a name="bkmk_bootstrap"></a> Bootstrap CSS Framework</span></span>
<span data-ttu-id="6c96a-169">V MVC 5 nová šablona je integrovanou podporu zavedení.</span><span class="sxs-lookup"><span data-stu-id="6c96a-169">New in the MVC 5 template is built-in Bootstrap support.</span></span> <span data-ttu-id="6c96a-170">Jste už viděli, jak okamžitě vylepšuje různá zobrazení ve vaší aplikaci.</span><span class="sxs-lookup"><span data-stu-id="6c96a-170">You have already seen how it immediately improves the different views in your application.</span></span> <span data-ttu-id="6c96a-171">Navigační panel v horní části je například automaticky sbalitelné po menší šířku prohlížeče.</span><span class="sxs-lookup"><span data-stu-id="6c96a-171">For example, the navigation bar at the top is automatically collapsible when the browser width is smaller.</span></span> <span data-ttu-id="6c96a-172">Na ploše prohlížeč zkuste upravit velikost okna prohlížeče a najdete v části Jak navigační panel změní jeho vzhled a chování.</span><span class="sxs-lookup"><span data-stu-id="6c96a-172">On the desktop browser, try resizing the browser window and see how the navigation bar changes its look and feel.</span></span> <span data-ttu-id="6c96a-173">Toto je návrh přizpůsobivý webu, který je součástí Bootstrap.</span><span class="sxs-lookup"><span data-stu-id="6c96a-173">This is the responsive web design that is built into Bootstrap.</span></span>

<span data-ttu-id="6c96a-174">Chcete-li zjistit, jak by bez Bootstrap zobrazovat webové aplikace, otevřete *aplikace\_spustit\\BundleConfig.cs* a Odkomentujte řádky, které obsahují *bootstrap.js* a  *Bootstrap.CSS*.</span><span class="sxs-lookup"><span data-stu-id="6c96a-174">To see how the Web app would look without Bootstrap, open *App\_Start\\BundleConfig.cs* and comment out the lines that contain *bootstrap.js* and *bootstrap.css*.</span></span> <span data-ttu-id="6c96a-175">Následující kód ukazuje posledních dvou prohlášení o `RegisterBundles` metoda po provedení změny:</span><span class="sxs-lookup"><span data-stu-id="6c96a-175">The following code shows the last two statements of the `RegisterBundles` method after the change:</span></span>

     bundles.Add(new ScriptBundle("~/bundles/bootstrap").Include(
              //"~/Scripts/bootstrap.js",
              "~/Scripts/respond.js"));

    bundles.Add(new StyleBundle("~/Content/css").Include(
              //"~/Content/bootstrap.css",
              "~/Content/site.css"));

<span data-ttu-id="6c96a-176">Stiskněte klávesu `Ctrl+F5` ke spuštění aplikace.</span><span class="sxs-lookup"><span data-stu-id="6c96a-176">Press `Ctrl+F5` to run the application.</span></span>

<span data-ttu-id="6c96a-177">Sledujte sbalitelné navigačním panelu je teď právě obyčejnou neuspořádaný seznam.</span><span class="sxs-lookup"><span data-stu-id="6c96a-177">Observe that the collapsible navigation bar is now just an ordinary unordered list.</span></span> <span data-ttu-id="6c96a-178">Klikněte na tlačítko **Procházet podle značky** znovu, pak klikněte na tlačítko **ASP.NET**.</span><span class="sxs-lookup"><span data-stu-id="6c96a-178">Click **Browse by tag** again, then click **ASP.NET**.</span></span>
<span data-ttu-id="6c96a-179">V emulátoru mobilního zobrazení uvidíte teď, když je už přiblížení namontováno na obrazovku a musí přejděte do stran Chcete-li zobrazit pravé tabulky.</span><span class="sxs-lookup"><span data-stu-id="6c96a-179">In the mobile emulator view, you can see now that it is no longer zoom-fitted to the screen, and you must scroll sideways in order to see the right side of the table.</span></span>

![][SessionsByTagASP.NETNoBootstrap]

<span data-ttu-id="6c96a-180">Vrátit zpět změny a aktualizujte prohlížeč pro mobilní zařízení k ověření, že byla obnovena zobrazení mobilní zařízení.</span><span class="sxs-lookup"><span data-stu-id="6c96a-180">Undo your changes and refresh the mobile browser to verify that the mobile-friendly display has been restored.</span></span>

<span data-ttu-id="6c96a-181">Bootstrap není specifické pro ASP.NET MVC 5 a můžete využít výhod těchto funkcí v jakékoli webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="6c96a-181">Bootstrap is not specific to ASP.NET MVC 5, and you can take advantage of these features in any web application.</span></span> <span data-ttu-id="6c96a-182">Ale nyní integrovaná do šablony projektu ASP.NET MVC 5, tak, aby MVC 5 webové aplikace mohou využít výhod Bootstrap ve výchozím nastavení.</span><span class="sxs-lookup"><span data-stu-id="6c96a-182">But it is now built into the ASP.NET MVC 5 project template, so that your MVC 5 Web application can take advantage of Bootstrap by default.</span></span>

<span data-ttu-id="6c96a-183">Další informace o Bootstrap, přejděte na [Bootstrap] [ BootstrapSite] lokality.</span><span class="sxs-lookup"><span data-stu-id="6c96a-183">For more information about Bootstrap, go to the [Bootstrap][BootstrapSite] site.</span></span>

<span data-ttu-id="6c96a-184">V další části se zobrazí, jak poskytnout konkrétní zobrazení mobilní prohlížeče.</span><span class="sxs-lookup"><span data-stu-id="6c96a-184">In the next section you'll see how to provide mobile-browser specific views.</span></span>

## <span data-ttu-id="6c96a-185"><a name="bkmk_overrideviews"></a>Přepsání, zobrazení, rozložení a částečné zobrazení</span><span class="sxs-lookup"><span data-stu-id="6c96a-185"><a name="bkmk_overrideviews"></a> Override the Views, Layouts, and Partial Views</span></span>
<span data-ttu-id="6c96a-186">Pro mobilní prohlížeče obecně pro jednotlivé mobilního prohlížeče nebo pro jakékoli konkrétní prohlížeč můžete přepsat všechna zobrazení (včetně rozložení a částečné zobrazení).</span><span class="sxs-lookup"><span data-stu-id="6c96a-186">You can override any view (including layouts and partial views) for mobile browsers in general, for an individual mobile browser, or for any specific browser.</span></span> <span data-ttu-id="6c96a-187">Abyste si mohli zobrazit konkrétní mobilní, můžete zkopírovat soubor zobrazení a přidat *. Mobilní* k názvu souboru.</span><span class="sxs-lookup"><span data-stu-id="6c96a-187">To provide a mobile-specific view, you can copy a view file and add *.Mobile* to the file name.</span></span> <span data-ttu-id="6c96a-188">Chcete-li například vytvořit mobilních *Index* zobrazení, můžete zkopírovat *zobrazení\\Domů\\Index.cshtml* k *zobrazení\\Domů\\ Index.Mobile.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="6c96a-188">For example, to create a mobile *Index* view, you can copy *Views\\Home\\Index.cshtml* to *Views\\Home\\Index.Mobile.cshtml*.</span></span>

<span data-ttu-id="6c96a-189">V této části vytvoříte soubor specifické mobilní rozložení.</span><span class="sxs-lookup"><span data-stu-id="6c96a-189">In this section, you'll create a mobile-specific layout file.</span></span>

<span data-ttu-id="6c96a-190">Chcete-li začít, zkopírujte *zobrazení\\sdílené\\\_Layout.cshtml* k *zobrazení\\sdílené\\\_Layout.Mobile.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="6c96a-190">To start, copy *Views\\Shared\\\_Layout.cshtml* to *Views\\Shared\\\_Layout.Mobile.cshtml*.</span></span> <span data-ttu-id="6c96a-191">Otevřete  *\_Layout.Mobile.cshtml* a změňte název od **MVC5 aplikace** k **MVC5 aplikace (mobilní)**.</span><span class="sxs-lookup"><span data-stu-id="6c96a-191">Open *\_Layout.Mobile.cshtml* and change the title from **MVC5 Application** to **MVC5 Application (Mobile)**.</span></span>

<span data-ttu-id="6c96a-192">V každé `Html.ActionLink` volání pro navigační panel, odeberte "Procházet podle" v každé propojení *ActionLink*.</span><span class="sxs-lookup"><span data-stu-id="6c96a-192">In each `Html.ActionLink` call for the navigation bar, remove "Browse by" in each link *ActionLink*.</span></span> <span data-ttu-id="6c96a-193">Následující kód ukazuje dokončené `<ul class="nav navbar-nav">` značky mobilní rozložení souboru.</span><span class="sxs-lookup"><span data-stu-id="6c96a-193">The following code shows the completed `<ul class="nav navbar-nav">` tag of the mobile layout file.</span></span>

    <ul class="nav navbar-nav">
        <li>@Html.ActionLink("Home", "Index", "Home")</li>
        <li>@Html.ActionLink("Date", "AllDates", "Home")</li>
        <li>@Html.ActionLink("Speaker", "AllSpeakers", "Home")</li>
        <li>@Html.ActionLink("Tag", "AllTags", "Home")</li>
    </ul>

<span data-ttu-id="6c96a-194">Kopírování *zobrazení\\Domů\\AllTags.cshtml* do souboru *zobrazení\\Domů\\AllTags.Mobile.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="6c96a-194">Copy the *Views\\Home\\AllTags.cshtml* file to *Views\\Home\\AllTags.Mobile.cshtml*.</span></span> <span data-ttu-id="6c96a-195">Otevřete nový soubor a změňte `<h2>` element z "Značky" na "značky (M)":</span><span class="sxs-lookup"><span data-stu-id="6c96a-195">Open the new file and change the `<h2>` element from "Tags" to "Tags (M)":</span></span>

    <h2>Tags (M)</h2>

<span data-ttu-id="6c96a-196">Přejděte na stránku značky pomocí prohlížeč pro stolní počítač a pomocí emulátoru prohlížeč pro mobilní zařízení.</span><span class="sxs-lookup"><span data-stu-id="6c96a-196">Browse to the tags page using a desktop browser and using mobile browser emulator.</span></span> <span data-ttu-id="6c96a-197">Emulátor mobilní prohlížeče ukazuje dva provedené změny (titul z  *\_Layout.Mobile.cshtml* a titul z *AllTags.Mobile.cshtml*).</span><span class="sxs-lookup"><span data-stu-id="6c96a-197">The mobile browser emulator shows the two changes you made (the title from *\_Layout.Mobile.cshtml* and the title from *AllTags.Mobile.cshtml*).</span></span>

![][AllTagsMobile_LayoutMobile]

<span data-ttu-id="6c96a-198">Naproti tomu nedošlo ke změně zobrazení plochy (s názvy z  *\_Layout.cshtml* a *AllTags.cshtml*).</span><span class="sxs-lookup"><span data-stu-id="6c96a-198">In contrast, the desktop display has not changed (with titles from *\_Layout.cshtml* and *AllTags.cshtml*).</span></span>

![][AllTagsMobile_LayoutMobileDesktop]

## <span data-ttu-id="6c96a-199"><a name="bkmk_browserviews"></a>Vytvoření vlastních zobrazení</span><span class="sxs-lookup"><span data-stu-id="6c96a-199"><a name="bkmk_browserviews"></a> Create Browser-Specific Views</span></span>
<span data-ttu-id="6c96a-200">Kromě zobrazení specifická pro mobilní a desktop můžete vytvořit zobrazení pro jednotlivé prohlížeče.</span><span class="sxs-lookup"><span data-stu-id="6c96a-200">In addition to mobile-specific and desktop-specific views, you can create views for an individual browser.</span></span> <span data-ttu-id="6c96a-201">Můžete například vytvořit zobrazení, které jsou speciálně určené pro iPhone nebo prohlížeč systému Android.</span><span class="sxs-lookup"><span data-stu-id="6c96a-201">For example, you can create views that are specifically for the iPhone or the Android browser.</span></span> <span data-ttu-id="6c96a-202">V této části vytvoříte rozložení pro prohlížeč iPhone a na zařízení iPhone verzi *AllTags* zobrazení.</span><span class="sxs-lookup"><span data-stu-id="6c96a-202">In this section, you'll create a layout for the iPhone browser and an iPhone version of the *AllTags* view.</span></span>

<span data-ttu-id="6c96a-203">Otevřete *Global.asax* souboru a přidejte následující kód k dolnímu okraji `Application_Start` metoda.</span><span class="sxs-lookup"><span data-stu-id="6c96a-203">Open the *Global.asax* file and add the following code to the bottom of the `Application_Start` method.</span></span>

    DisplayModeProvider.Instance.Modes.Insert(0, new DefaultDisplayMode("iPhone")
    {
        ContextCondition = (context => context.GetOverriddenUserAgent().IndexOf
            ("iPhone", StringComparison.OrdinalIgnoreCase) >= 0)
    });

<span data-ttu-id="6c96a-204">Tento kód definuje nový režim zobrazení s názvem "iPhone", který bude porovnání jednotlivých příchozích požadavků.</span><span class="sxs-lookup"><span data-stu-id="6c96a-204">This code defines a new display mode named "iPhone" that will be matched against each incoming request.</span></span> <span data-ttu-id="6c96a-205">Pokud příchozí požadavek odpovídá podmínku, kterou jste definovali (Pokud uživatelský agent obsahuje řetězec "iPhone"), rozhraní ASP.NET MVC bude hledat zobrazení, jejíž název obsahuje příponu "iPhone".</span><span class="sxs-lookup"><span data-stu-id="6c96a-205">If the incoming request matches the condition you defined (that is, if the user agent contains the string "iPhone"), ASP.NET MVC will look for views whose name contains the "iPhone" suffix.</span></span>

> [!NOTE]
> <span data-ttu-id="6c96a-206">Při přidávání režimy mobilní zobrazení specifické pro prohlížeč, například konkrétní iPhone a Android, nezapomeňte nastavit první argument `0` (Vložit v horní části seznamu) a ujistěte se, specifické pro prohlížeč režim má přednost před mobilní šablony (*. Mobile.cshtml).</span><span class="sxs-lookup"><span data-stu-id="6c96a-206">When adding mobile browser-specific display modes, such as for iPhone and Android, be sure to set the first argument to `0` (insert at the top of the list) to make sure that the browser-specific mode takes precedence over the mobile template (*.Mobile.cshtml).</span></span> <span data-ttu-id="6c96a-207">Pokud mobilní šablona je k dispozici místo v horní části seznamu, bude vybrána přes vaše režim určený zobrazení (první shodu wins a mobilní šablonu odpovídá všechny mobilní prohlížeče).</span><span class="sxs-lookup"><span data-stu-id="6c96a-207">If the mobile template is at the top of the list instead, it will be selected over your intended display mode (the first match wins, and the mobile template matches all mobile browsers).</span></span> 
> 
> 

<span data-ttu-id="6c96a-208">V kódu, klikněte pravým tlačítkem na `DefaultDisplayMode`, zvolte **vyřešit**a potom vyberte `using System.Web.WebPages;`.</span><span class="sxs-lookup"><span data-stu-id="6c96a-208">In the code, right-click `DefaultDisplayMode`, choose **Resolve**, and then choose `using System.Web.WebPages;`.</span></span> <span data-ttu-id="6c96a-209">Tento postup přidá odkaz na `System.Web.WebPages` názvů, který je tam, kde `DisplayModeProvider` a `DefaultDisplayMode` typy jsou definované.</span><span class="sxs-lookup"><span data-stu-id="6c96a-209">This adds a reference to the `System.Web.WebPages` namespace, which is where the `DisplayModeProvider` and `DefaultDisplayMode` types are defined.</span></span>

![][ResolveDefaultDisplayMode]

<span data-ttu-id="6c96a-210">Alternativně můžete právě ručně přidejte následující řádek na `using` část souboru.</span><span class="sxs-lookup"><span data-stu-id="6c96a-210">Alternatively, you can just manually add the following line to the `using` section of the file.</span></span>

    using System.Web.WebPages;

<span data-ttu-id="6c96a-211">Uložte změny.</span><span class="sxs-lookup"><span data-stu-id="6c96a-211">Save the changes.</span></span> <span data-ttu-id="6c96a-212">Kopírování *zobrazení\\sdílené\\\_Layout.Mobile.cshtml* do souboru *zobrazení\\sdílené\\\_Layout.iPhone.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="6c96a-212">Copy the *Views\\Shared\\\_Layout.Mobile.cshtml* file to *Views\\Shared\\\_Layout.iPhone.cshtml*.</span></span> <span data-ttu-id="6c96a-213">Otevřete nový soubor a potom změňte název od `MVC5 Application (Mobile)` k `MVC5 Application (iPhone)`.</span><span class="sxs-lookup"><span data-stu-id="6c96a-213">Open the new file and then change the title from `MVC5 Application (Mobile)` to `MVC5 Application (iPhone)`.</span></span>

<span data-ttu-id="6c96a-214">Kopírování *zobrazení\\Domů\\AllTags.Mobile.cshtml* do souboru *zobrazení\\Domů\\AllTags.iPhone.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="6c96a-214">Copy the *Views\\Home\\AllTags.Mobile.cshtml* file to *Views\\Home\\AllTags.iPhone.cshtml*.</span></span> <span data-ttu-id="6c96a-215">Nový soubor, změňte `<h2>` element z "značky (M)" pro "Značky (iPhone)".</span><span class="sxs-lookup"><span data-stu-id="6c96a-215">In the new file, change the `<h2>` element from "Tags (M)" to "Tags (iPhone)".</span></span>

<span data-ttu-id="6c96a-216">Spusťte aplikaci.</span><span class="sxs-lookup"><span data-stu-id="6c96a-216">Run the application.</span></span> <span data-ttu-id="6c96a-217">Spustit prohlížeč pro mobilní zařízení emulátoru, ujistěte se jeho uživatelský agent je nastavena na "iPhone" a přejděte do *AllTags* zobrazení.</span><span class="sxs-lookup"><span data-stu-id="6c96a-217">Run a mobile browser emulator, make sure its user agent is set to "iPhone", and browse to the *AllTags* view.</span></span> <span data-ttu-id="6c96a-218">Pokud používáte emulátor serveru v Internet Exploreru 11 F12 nástroje pro vývojáře, konfigurace emulace s následujícím:</span><span class="sxs-lookup"><span data-stu-id="6c96a-218">If you are using the emulator in Internet Explorer 11 F12 developer tools, configure emulation to the following:</span></span>

* <span data-ttu-id="6c96a-219">Profil Browser = **Windows Phone**</span><span class="sxs-lookup"><span data-stu-id="6c96a-219">Browser profile = **Windows Phone**</span></span>
* <span data-ttu-id="6c96a-220">Řetězec uživatelského agenta = **vlastní**</span><span class="sxs-lookup"><span data-stu-id="6c96a-220">User agent string =  **Custom**</span></span>
* <span data-ttu-id="6c96a-221">Vlastní řetězec = **Apple-iPhone5C1/1001.525**</span><span class="sxs-lookup"><span data-stu-id="6c96a-221">Custom string = **Apple-iPhone5C1/1001.525**</span></span>

<span data-ttu-id="6c96a-222">Následující snímek obrazovky ukazuje *AllTags* zobrazení vykreslen v emulátoru v Internet Exploreru 11 F12 nástroje pro vývojáře s vlastní identifikační řetězec prohlížeče (jedná se řetězec iPhone 5 C uživatelského agenta).</span><span class="sxs-lookup"><span data-stu-id="6c96a-222">The following screenshot shows the *AllTags* view rendered in the emulator in Internet Explorer 11 F12 developer tools with the custom user agent string (this is an iPhone 5C user agent string).</span></span>

![][AllTagsIPhone_LayoutIPhone]

<span data-ttu-id="6c96a-223">Mobilní prohlížeče, vyberte **Řečníci** odkaz.</span><span class="sxs-lookup"><span data-stu-id="6c96a-223">In the mobile browser, select the **Speakers** link.</span></span> <span data-ttu-id="6c96a-224">Protože mobilní zobrazení není (*AllSpeakers.Mobile.cshtml*), zobrazit výchozí mluvčí (*AllSpeakers.cshtml*) je vykreslen pomocí mobilních rozložení zobrazení ( *\_ Layout.Mobile.cshtml*).</span><span class="sxs-lookup"><span data-stu-id="6c96a-224">Because there's not a mobile view (*AllSpeakers.Mobile.cshtml*), the default speakers view (*AllSpeakers.cshtml*) is rendered using the mobile layout view (*\_Layout.Mobile.cshtml*).</span></span> <span data-ttu-id="6c96a-225">Jak je uvedeno níže, název **MVC5 aplikace (mobilní)** je definována v  *\_Layout.Mobile.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="6c96a-225">As shown below, the title **MVC5 Application (Mobile)** is defined in *\_Layout.Mobile.cshtml*.</span></span>

![][AllSpeakers_LayoutMobile]

<span data-ttu-id="6c96a-226">Výchozí zobrazení (jiných než mobilních) z vykreslování uvnitř mobilní rozložení můžete zakázat globálně nastavením `RequireConsistentDisplayMode` k `true` v *zobrazení\\\_ViewStart.cshtml* souboru, například takto:</span><span class="sxs-lookup"><span data-stu-id="6c96a-226">You can globally disable a default (non-mobile) view from rendering inside a mobile layout by setting `RequireConsistentDisplayMode` to `true` in the *Views\\\_ViewStart.cshtml* file, like this:</span></span>

    @{
        Layout = "~/Views/Shared/_Layout.cshtml";
        DisplayModeProvider.Instance.RequireConsistentDisplayMode = true;
    }

<span data-ttu-id="6c96a-227">Když `RequireConsistentDisplayMode` je nastaven na `true`, mobilní rozložení (*\_Layout.Mobile.cshtml*) se používá pouze pro mobilní zobrazení (tj. Po zobrazení souboru ve formátu  ***ViewName**. Mobile.cshtml*).</span><span class="sxs-lookup"><span data-stu-id="6c96a-227">When `RequireConsistentDisplayMode` is set to `true`, the mobile layout (*\_Layout.Mobile.cshtml*) is used only for mobile views (i.e. when the view file is of the form ***ViewName**.Mobile.cshtml*).</span></span> <span data-ttu-id="6c96a-228">Můžete chtít nastavit `RequireConsistentDisplayMode` k `true` Pokud vaše mobilní rozložení nebude fungovat dobře u jiných než mobilních zobrazení.</span><span class="sxs-lookup"><span data-stu-id="6c96a-228">You might want to set `RequireConsistentDisplayMode` to `true` if your mobile layout doesn't work well with your non-mobile views.</span></span> <span data-ttu-id="6c96a-229">Na snímku obrazovky níže znázorňuje jak *Řečníci* stránka vykresluje při `RequireConsistentDisplayMode` je nastaven na `true` (bez řetězec "(mobilní)" v navigační panel v horní části).</span><span class="sxs-lookup"><span data-stu-id="6c96a-229">The screenshot below shows how the *Speakers* page renders when `RequireConsistentDisplayMode` is set to `true` (without the string "(Mobile)" in the navigational bar at the top).</span></span>

![][AllSpeakers_LayoutMobileOverridden]

<span data-ttu-id="6c96a-230">Konzistentní režim zobrazení v určitém zobrazení můžete zakázat nastavením `RequireConsistentDisplayMode` k `false` v souboru zobrazení.</span><span class="sxs-lookup"><span data-stu-id="6c96a-230">You can disable consistent display mode in a specific view by setting `RequireConsistentDisplayMode` to `false` in the view file.</span></span> <span data-ttu-id="6c96a-231">Následující kód v *zobrazení\\Domů\\AllSpeakers.cshtml* souboru nastaví `RequireConsistentDisplayMode` k `false`:</span><span class="sxs-lookup"><span data-stu-id="6c96a-231">The following markup in the *Views\\Home\\AllSpeakers.cshtml* file sets `RequireConsistentDisplayMode` to `false`:</span></span>

    @model IEnumerable<string>

    @{
        ViewBag.Title = "All speakers";
        DisplayModeProvider.Instance.RequireConsistentDisplayMode = false;
    }

<span data-ttu-id="6c96a-232">V této části jsme viděli postup vytvoření mobilní rozložení a zobrazení a postup vytvoření zobrazení pro konkrétní zařízení, jako je iPhone a rozložení.</span><span class="sxs-lookup"><span data-stu-id="6c96a-232">In this section we've seen how to create mobile layouts and views and how to create layouts and views for specific devices such as the iPhone.</span></span>
<span data-ttu-id="6c96a-233">Hlavní výhodou rozhraní Bootstrap šablon stylů CSS je však přizpůsobivé rozložení, což znamená, že jeden šablony stylů mohou být použity u plochy, phone a prohlížeče tabletu k vytvoření konzistentního vzhledu a chování.</span><span class="sxs-lookup"><span data-stu-id="6c96a-233">However, the main advantage of the Bootstrap CSS framework is the responsive layout, which means that a single stylesheet can be applied across desktop, phone, and tablet browsers to create a consistent look and feel.</span></span> <span data-ttu-id="6c96a-234">V další části se zobrazí, jak využít Bootstrap vytvořte zobrazení mobilní zařízení.</span><span class="sxs-lookup"><span data-stu-id="6c96a-234">In the next section you'll see how to leverage Bootstrap to create mobile-friendly views.</span></span>

## <span data-ttu-id="6c96a-235"><a name="bkmk_Improvespeakerslist"></a>Zlepšení seznamu mluvčí</span><span class="sxs-lookup"><span data-stu-id="6c96a-235"><a name="bkmk_Improvespeakerslist"></a> Improve the Speakers List</span></span>
<span data-ttu-id="6c96a-236">Protože jste právě viděli, *mluvčí* zobrazení je čitelná, ale odkazy jsou malé a obtížně se klepnout na mobilní zařízení.</span><span class="sxs-lookup"><span data-stu-id="6c96a-236">As you just saw, the *Speakers* view is readable, but the links are small and are difficult to tap on a mobile device.</span></span> <span data-ttu-id="6c96a-237">V této části, budete mít *AllSpeakers* zobrazit mobilní zařízení, která zobrazuje velké, snadno klepněte odkazy a obsahuje vyhledávací pole a rychle tak najít mluvčí.</span><span class="sxs-lookup"><span data-stu-id="6c96a-237">In this section, you'll make the *AllSpeakers* view mobile-friendly, which displays large, easy-to-tap links and contains a search box to quickly find speakers.</span></span>

<span data-ttu-id="6c96a-238">Můžete použít službou Bootstrap nástroje [skupiny odkazovaného seznamu] [ linked list group] stylů ke zlepšení *Řečníci* zobrazení.</span><span class="sxs-lookup"><span data-stu-id="6c96a-238">You can use the Bootstrap [linked list group][linked list group] styling to improve the *Speakers* view.</span></span> <span data-ttu-id="6c96a-239">V *zobrazení\\Domů\\AllSpeakers.cshtml*, nahraďte obsah souboru Razor kód níže.</span><span class="sxs-lookup"><span data-stu-id="6c96a-239">In *Views\\Home\\AllSpeakers.cshtml*, replace the contents of the Razor file with the code below.</span></span>

     @model IEnumerable<string>

    @{
        ViewBag.Title = "All Speakers";
    }

    <h2>Speakers</h2>

    <div class="list-group">
        @foreach (var speaker in Model)
        {
            @Html.ActionLink(speaker, "SessionsBySpeaker", new { speaker }, new { @class = "list-group-item" })
        }
    </div>

<span data-ttu-id="6c96a-240">`class="list-group"` Atribut `<div>` značky platí Bootstrap seznamu stylu a `class="input-group-item"` atribut platí stylů položky Bootstrap seznamu pro každý odkaz.</span><span class="sxs-lookup"><span data-stu-id="6c96a-240">The `class="list-group"` attribute in the `<div>` tag applies the Bootstrap list styling, and the `class="input-group-item"` attribute applies Bootstrap list item styling to each link.</span></span>

<span data-ttu-id="6c96a-241">Aktualizujte prohlížeč pro mobilní zařízení.</span><span class="sxs-lookup"><span data-stu-id="6c96a-241">Refresh the mobile browser.</span></span> <span data-ttu-id="6c96a-242">Aktualizovaná zobrazení vypadá takto:</span><span class="sxs-lookup"><span data-stu-id="6c96a-242">The updated view looks like this:</span></span>

![][AllSpeakersFixed]

<span data-ttu-id="6c96a-243">Službou Bootstrap nástroje [skupiny odkazovaného seznamu] [ linked list group] stylů díky celé pole pro každý odkaz můžete kliknout, což je mnohem lepší prostředí.</span><span class="sxs-lookup"><span data-stu-id="6c96a-243">The Bootstrap [linked list group][linked list group] styling makes the entire box for each link clickable, which is a much better user experience.</span></span> <span data-ttu-id="6c96a-244">Přepněte do zobrazení plochy a sledovat konzistentní vzhled a chování.</span><span class="sxs-lookup"><span data-stu-id="6c96a-244">Switch to the desktop view and observe the consistent look and feel.</span></span>

![][AllSpeakersFixedDesktop]

<span data-ttu-id="6c96a-245">I když se zlepšila zobrazení prohlížeč pro mobilní zařízení, je obtížné přejděte dlouhý seznam mluvčí.</span><span class="sxs-lookup"><span data-stu-id="6c96a-245">Although the mobile browser view has improved, it's difficult to navigate the long list of speakers.</span></span> <span data-ttu-id="6c96a-246">Bootstrap neposkytuje hledání filtru funkce out-of-the-box, ale můžete jej přidat pár řádků kódu.</span><span class="sxs-lookup"><span data-stu-id="6c96a-246">Bootstrap doesn't provide a search filter functionality out-of-the-box, but you can add it with a few lines of code.</span></span> <span data-ttu-id="6c96a-247">Nejprve bude přidejte vyhledávací pole k zobrazení a pak propojte s kód jazyka JavaScript pro funkci filtru.</span><span class="sxs-lookup"><span data-stu-id="6c96a-247">You will first add a search box to the view, then hook up with the JavaScript code for the filter function.</span></span> <span data-ttu-id="6c96a-248">V *zobrazení\\Domů\\AllSpeakers.cshtml*, přidejte \<formuláře\> hned za značkami \<h2\> značka, jak je uvedeno níže:</span><span class="sxs-lookup"><span data-stu-id="6c96a-248">In *Views\\Home\\AllSpeakers.cshtml*, add a \<form\> tag just after the \<h2\> tag, as shown below:</span></span>

    @model IEnumerable<string>

    @{
        ViewBag.Title = "All Speakers";
    }

    <h2>Speakers</h2>

    <form class="input-group">
        <span class="input-group-addon"><span class="glyphicon glyphicon-search"></span></span>
        <input type="text" class="form-control" placeholder="Search speaker">
    </form>
    <br />
    <div class="list-group">
        @foreach (var speaker in Model)
        {
            @Html.ActionLink(speaker, 
                             "SessionsBySpeaker", 
                             new { speaker }, 
                             new { @class = "list-group-item" })
        }
    </div>

<span data-ttu-id="6c96a-249">Všimněte si, že `<form>` a `<input>` značky oba mají Bootstrap stylů na ně použity.</span><span class="sxs-lookup"><span data-stu-id="6c96a-249">Notice that the `<form>` and `<input>` tags both have the Bootstrap styles applied to them.</span></span> <span data-ttu-id="6c96a-250">`<span>` Element přidá Bootstrap [glyphicon] [ glyphicon] do vyhledávacího pole.</span><span class="sxs-lookup"><span data-stu-id="6c96a-250">The `<span>` element adds a Bootstrap [glyphicon][glyphicon] to the search box.</span></span>

<span data-ttu-id="6c96a-251">V *skripty* složky, přidejte do souboru JavaScript s názvem *filter.js*.</span><span class="sxs-lookup"><span data-stu-id="6c96a-251">In the *Scripts* folder, add a JavaScript file called *filter.js*.</span></span> <span data-ttu-id="6c96a-252">Otevřete soubor a vložte do něj následující kód:</span><span class="sxs-lookup"><span data-stu-id="6c96a-252">Open the file and paste the following code into it:</span></span>

    $(function () {

        // reset the search form when the page loads
        $("form").each(function () {
            this.reset();
        });

        // wire up the events to the <input> element for search/filter
        $("input").bind("keyup change", function () {
            var searchtxt = this.value.toLowerCase();
            var items = $(".list-group-item");

            // show all speakers that begin with the typed text and hide others
            for (var i = 0; i < items.length; i++) {
                var val = items[i].text.toLowerCase();
                val = val.substring(0, searchtxt.length);
                if (val == searchtxt) {
                    $(items[i]).show();
                }
                else {
                    $(items[i]).hide();
                }
            }
        });
    });

<span data-ttu-id="6c96a-253">Musíte taky zahrnout filter.js registrovaných sad.</span><span class="sxs-lookup"><span data-stu-id="6c96a-253">You also need to include filter.js in your registered bundles.</span></span> <span data-ttu-id="6c96a-254">Otevřete *aplikace\_spustit\\BundleConfig.cs* a změňte první sady.</span><span class="sxs-lookup"><span data-stu-id="6c96a-254">Open *App\_Start\\BundleConfig.cs* and change the first bundles.</span></span> <span data-ttu-id="6c96a-255">Změnit první `bundles.Add` – příkaz (pro **jquery** sady) zahrnout *skripty\\filter.js*, a to takto:</span><span class="sxs-lookup"><span data-stu-id="6c96a-255">Change the first `bundles.Add` statement (for the **jquery** bundle) to include *Scripts\\filter.js*, as follows:</span></span>

     bundles.Add(new ScriptBundle("~/bundles/jquery").Include(
                "~/Scripts/jquery-{version}.js",
                "~/Scripts/filter.js"));

<span data-ttu-id="6c96a-256">**Jquery** sady je již vykreslen pomocí výchozí  *\_rozložení* zobrazení.</span><span class="sxs-lookup"><span data-stu-id="6c96a-256">The **jquery** bundle is already rendered by the default *\_Layout* view.</span></span> <span data-ttu-id="6c96a-257">Později můžete použít stejný kód JavaScript pro použití funkce filtru s dalšími zobrazeními seznamu.</span><span class="sxs-lookup"><span data-stu-id="6c96a-257">Later, you can utilize the same JavaScript code to apply the filter functionality to other list views.</span></span>

<span data-ttu-id="6c96a-258">Aktualizujte mobilní prohlížeče a přejděte na *AllSpeakers* zobrazení.</span><span class="sxs-lookup"><span data-stu-id="6c96a-258">Refresh the mobile browser and go to the *AllSpeakers* view.</span></span> <span data-ttu-id="6c96a-259">Do vyhledávacího pole zadejte "sc".</span><span class="sxs-lookup"><span data-stu-id="6c96a-259">In the search box, type "sc".</span></span> <span data-ttu-id="6c96a-260">Seznamu mluvčí by měl být nyní filtrovaných podle hledanému řetězci.</span><span class="sxs-lookup"><span data-stu-id="6c96a-260">The speakers list should now be filtered according to your search string.</span></span>

![][AllSpeakersFixedSearchBySC]

## <span data-ttu-id="6c96a-261"><a name="bkmk_improvetags"></a>Zlepšení seznam klíčových slov</span><span class="sxs-lookup"><span data-stu-id="6c96a-261"><a name="bkmk_improvetags"></a> Improve the Tags List</span></span>
<span data-ttu-id="6c96a-262">Jako *Řečníci* zobrazení, *značky* zobrazení je čitelná, ale jsou odkazy na malé a obtížně se klepnout na mobilní zařízení.</span><span class="sxs-lookup"><span data-stu-id="6c96a-262">Like the *Speakers* view, the *Tags* view is readable, but the links are small and difficult to tap on a mobile device.</span></span> <span data-ttu-id="6c96a-263">Můžete je vyřešit *značky* zobrazení stejným způsobem jako neopravíte *Řečníci* zobrazit, je-li použít změny kódu popsané výše, ale s následujícími službami `Html.ActionLink` syntaxe využívající metody v *zobrazení\\ Domů\\AllTags.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="6c96a-263">You can fix the *Tags* view the same way you fix the *Speakers* view, if you use the code changes described earlier, but with the following `Html.ActionLink` method syntax in *Views\\Home\\AllTags.cshtml*:</span></span>

    @Html.ActionLink(tag, 
                     "SessionsByTag", 
                     new { tag }, 
                     new { @class = "list-group-item" })

<span data-ttu-id="6c96a-264">Aktualizovat prohlížeč pro stolní počítač vypadá takto:</span><span class="sxs-lookup"><span data-stu-id="6c96a-264">The refreshed desktop browser looks as follows:</span></span>

![][AllTagsFixedDesktop]

<span data-ttu-id="6c96a-265">A aktualizovat mobilní prohlížeče vypadá takto:</span><span class="sxs-lookup"><span data-stu-id="6c96a-265">And the refreshed mobile browser looks as follows:</span></span> 

![][AllTagsFixed]

> [!NOTE]
> <span data-ttu-id="6c96a-266">Pokud si všimnete, že původní formátování seznamu stále existuje v prohlížeč pro mobilní zařízení a zajímat, co se stalo s vaší dobrý Bootstrap stylů, jde artefakt starší akci k vytvoření mobilní konkrétní zobrazení.</span><span class="sxs-lookup"><span data-stu-id="6c96a-266">If you notice that the original list formatting is still there in the mobile browser and wonder what happened to your nice Bootstrap styling, this is an artifact of your earlier action to create mobile specific views.</span></span> <span data-ttu-id="6c96a-267">Teď, když používáte framework Bootstrap CSS k vytvoření návrhu přizpůsobivý webu, přejděte head a odebrat tyto specifické mobilní zobrazení a rozložení specifické pro mobilní zobrazení.</span><span class="sxs-lookup"><span data-stu-id="6c96a-267">However, now that you are using the Bootstrap CSS framework to create a responsive web design, go head and remove these mobile-specific views and the mobile-specific layout views.</span></span> <span data-ttu-id="6c96a-268">Po dokončení tak aktualizovat mobilní prohlížeče zobrazí Bootstrap stylů.</span><span class="sxs-lookup"><span data-stu-id="6c96a-268">Once you have done so, the refreshed mobile browser will show the Bootstrap styling.</span></span>
> 
> 

## <span data-ttu-id="6c96a-269"><a name="bkmk_improvedates"></a>Zlepšení seznamu kalendářních dat</span><span class="sxs-lookup"><span data-stu-id="6c96a-269"><a name="bkmk_improvedates"></a> Improve the Dates List</span></span>
<span data-ttu-id="6c96a-270">Můžete zvýšit *data* zobrazit stejně, jako je vylepšená *Řečníci* a *značky* zobrazení, je-li použít změny kódu popsané výše, ale s následujícími službami `Html.ActionLink` Syntaxe využívající metody v *zobrazení\\Domů\\AllDates.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="6c96a-270">You can improve the *Dates* view like you improved the *Speakers* and *Tags* views if you use the code changes described earlier, but with the following `Html.ActionLink` method syntax in *Views\\Home\\AllDates.cshtml*:</span></span>

    @Html.ActionLink(date.ToString("ddd, MMM dd, h:mm tt"), 
                     "SessionsByDate", 
                     new { date }, 
                     new { @class = "list-group-item" })

<span data-ttu-id="6c96a-271">Zobrazí se zobrazení aktualizovat prohlížeč pro mobilní zařízení takto:</span><span class="sxs-lookup"><span data-stu-id="6c96a-271">You will get a refreshed mobile browser view like this:</span></span>

![][AllDatesFixed]

<span data-ttu-id="6c96a-272">Lze dále zvýšit *data* zobrazení uspořádání hodnoty data a času podle data.</span><span class="sxs-lookup"><span data-stu-id="6c96a-272">You can further improve the *Dates* view by organizing the date-time values by date.</span></span> <span data-ttu-id="6c96a-273">To lze provést pomocí Bootstrap [panelů] [ panels] styly.</span><span class="sxs-lookup"><span data-stu-id="6c96a-273">This can be done with the Bootstrap [panels][panels] styling.</span></span> <span data-ttu-id="6c96a-274">Nahraďte obsah *zobrazení\\Domů\\AllDates.cshtml* soubor s následujícím kódem:</span><span class="sxs-lookup"><span data-stu-id="6c96a-274">Replace the contents of the *Views\\Home\\AllDates.cshtml* file with the following code:</span></span>

    @model IEnumerable<DateTime>

    @{
        ViewBag.Title = "All Dates";
    }

    <h2>Dates</h2>

    @foreach (var dategroup in Model.GroupBy(x=>x.Date))
    {
        <div class="panel panel-primary">
            <div class="panel-heading">
                @dategroup.Key.ToString("ddd, MMM dd")
            </div>
            <div class="panel-body list-group">
                @foreach (var date in dategroup)
                {
                    @Html.ActionLink(date.ToString("h:mm tt"), 
                                     "SessionsByDate", 
                                     new { date }, 
                                     new { @class = "list-group-item" })
                }
            </div>
        </div>
    }

<span data-ttu-id="6c96a-275">Tento kód vytvoří samostatné `<div class="panel panel-primary">` značky pro každé odlišné datum v seznamu a použije [skupiny odkazovaného seznamu] [ linked list group] pro příslušné odkazy jako předtím.</span><span class="sxs-lookup"><span data-stu-id="6c96a-275">This code creates a separate `<div class="panel panel-primary">` tag for each distinct date in the list, and uses the [linked list group][linked list group] for the respective links as before.</span></span> <span data-ttu-id="6c96a-276">Tady je prohlížeč pro mobilní zařízení, které bude vypadat takto spuštění tento kód:</span><span class="sxs-lookup"><span data-stu-id="6c96a-276">Here's what the mobile browser looks like when this code runs:</span></span>

![][AllDatesFixed2]

<span data-ttu-id="6c96a-277">Přepnout na prohlížeč pro stolní počítač.</span><span class="sxs-lookup"><span data-stu-id="6c96a-277">Switch to the desktop browser.</span></span> <span data-ttu-id="6c96a-278">Znovu si všimněte konzistentní vzhled.</span><span class="sxs-lookup"><span data-stu-id="6c96a-278">Again, note the consistent look.</span></span>

![][AllDatesFixed2Desktop]

## <span data-ttu-id="6c96a-279"><a name="bkmk_improvesessionstable"></a>Zlepšení SessionsTable zobrazení</span><span class="sxs-lookup"><span data-stu-id="6c96a-279"><a name="bkmk_improvesessionstable"></a> Improve the SessionsTable View</span></span>
<span data-ttu-id="6c96a-280">V této části, budete mít *SessionsTable* zobrazit další mobilní zařízení.</span><span class="sxs-lookup"><span data-stu-id="6c96a-280">In this section, you'll make the *SessionsTable* view more mobile-friendly.</span></span> <span data-ttu-id="6c96a-281">Tuto změnu je rozsáhlejší předchozí změny.</span><span class="sxs-lookup"><span data-stu-id="6c96a-281">This change is more extensive the previous changes.</span></span>

<span data-ttu-id="6c96a-282">Mobilní prohlížeče, klepněte na **značka** tlačítko a potom zadejte `asp` do vyhledávacího pole.</span><span class="sxs-lookup"><span data-stu-id="6c96a-282">In the mobile browser, tap the **Tag** button, then enter `asp` in the search box.</span></span>

![][AllTagsFixedSearchByASP]

<span data-ttu-id="6c96a-283">Klepněte **ASP.NET** odkaz.</span><span class="sxs-lookup"><span data-stu-id="6c96a-283">Tap the **ASP.NET** link.</span></span>

![][SessionsTableTagASP.NET]

<span data-ttu-id="6c96a-284">Jak vidíte, zobrazení je naformátován jako tabulku, který je aktuálně určený lze zobrazit v prohlížeči počítače.</span><span class="sxs-lookup"><span data-stu-id="6c96a-284">As you can see, the display is formatted as a table, which is currently designed to be viewed in the desktop browser.</span></span> <span data-ttu-id="6c96a-285">Je však chvíli obtížné číst na prohlížeč pro mobilní zařízení.</span><span class="sxs-lookup"><span data-stu-id="6c96a-285">However, it's a little bit difficult to read on a mobile browser.</span></span> <span data-ttu-id="6c96a-286">Chcete-li odstranit tento problém, otevřete *zobrazení\\Domů\\SessionsTable.cshtml* a obsah souboru nahraďte následujícím kódem:</span><span class="sxs-lookup"><span data-stu-id="6c96a-286">To fix this, open *Views\\Home\\SessionsTable.cshtml* and then replace the contents of the file with the following code:</span></span>

    @model IEnumerable<Mvc5Mobile.Models.Session>

    <h2>@ViewBag.Title</h2>

    <div class="container">
        <div class="row">
            @foreach (var session in Model)
            {
                <div class="col-md-4">
                    <div class="list-group">
                        @Html.ActionLink(session.Title, 
                                         "SessionByCode", 
                                         new { session.Code }, 
                                         new { @class="list-group-item active" })
                        <div class="list-group-item">
                            <div class="list-group-item-text">
                                @Html.Partial("_SpeakersLinks", session)
                            </div>
                            <div class="list-group-item-info">
                                @session.DateText
                            </div>
                            <div class="list-group-item-info small hidden-xs">
                                @Html.Partial("_TagsLinks", session)
                            </div>
                        </div>
                    </div>
                </div>
            }
        </div>
    </div>

<span data-ttu-id="6c96a-287">Kód provádí 3 věci:</span><span class="sxs-lookup"><span data-stu-id="6c96a-287">The code does 3 things:</span></span>

* <span data-ttu-id="6c96a-288">používá službou Bootstrap nástroje [skupiny vlastní odkazovaného seznamu] [ custom linked list group] k formátování informací o relaci ve svislém směru, aby tyto informace je čitelná na prohlížeč pro mobilní zařízení (pomocí třídy, jako je seznam skupiny položek textu)</span><span class="sxs-lookup"><span data-stu-id="6c96a-288">uses the Bootstrap [custom linked list group][custom linked list group] to format the session information vertically, so that all this information is readable on a mobile browser (using classes such as list-group-item-text)</span></span>
* <span data-ttu-id="6c96a-289">se vztahuje [mřížky systému] [ grid system] k rozložení, tak, aby položek relací toku vodorovně v desktopového prohlížeče a svisle mobilní prohlížeče (pomocí třídy sloupec md-4)</span><span class="sxs-lookup"><span data-stu-id="6c96a-289">applies the [grid system][grid system] to the layout, so that the session items flow horizontally in the desktop browser and vertically in the mobile browser (using the col-md-4 class)</span></span>
* <span data-ttu-id="6c96a-290">používá [přizpůsobivý nástroje] [ responsive utilities] skrýt značky relace v mobilní prohlížeče (pomocí třídy skryté xs)</span><span class="sxs-lookup"><span data-stu-id="6c96a-290">uses the [responsive utilities][responsive utilities] to hide the session tags when viewed in the mobile browser (using the hidden-xs class)</span></span>

<span data-ttu-id="6c96a-291">Taky můžou klepnout na odkaz title přejděte na příslušné relace.</span><span class="sxs-lookup"><span data-stu-id="6c96a-291">You can also tap a title link to go to the respective session.</span></span> <span data-ttu-id="6c96a-292">Následující obrázek zobrazuje změny kódu.</span><span class="sxs-lookup"><span data-stu-id="6c96a-292">The image below reflects the code changes.</span></span>

![][FixedSessionsByTag]

<span data-ttu-id="6c96a-293">Zavedení mřížky systém, který můžete použít automaticky uspořádá relací svisle v prohlížeč pro mobilní zařízení.</span><span class="sxs-lookup"><span data-stu-id="6c96a-293">The Bootstrap grid system that you applied automatically arranges the sessions vertically in the mobile browser.</span></span> <span data-ttu-id="6c96a-294">Všimněte si také, že nejsou zobrazeny značek.</span><span class="sxs-lookup"><span data-stu-id="6c96a-294">Also, notice that the tags are not shown.</span></span> <span data-ttu-id="6c96a-295">Přepnout na prohlížeč pro stolní počítač.</span><span class="sxs-lookup"><span data-stu-id="6c96a-295">Switch to the desktop browser.</span></span>

![][SessionsTableFixedTagASP.NETDesktop]

<span data-ttu-id="6c96a-296">V prohlížeči počítače Všimněte si, že značky se teď zobrazují.</span><span class="sxs-lookup"><span data-stu-id="6c96a-296">In the desktop browser, notice that the tags are now displayed.</span></span> <span data-ttu-id="6c96a-297">Zkontrolujte také, že systém Bootstrap mřížky, které jste provedli Uspořádá položky relace v dva sloupce.</span><span class="sxs-lookup"><span data-stu-id="6c96a-297">Also, you can see that the Bootstrap grid system you applied arranges the session items in two columns.</span></span> <span data-ttu-id="6c96a-298">V případě zvětšení v prohlížeči, zobrazí se, že změní uspořádání na tři sloupce.</span><span class="sxs-lookup"><span data-stu-id="6c96a-298">If you enlarge the browser, you will see that the arrangement changes to three columns.</span></span>

## <span data-ttu-id="6c96a-299"><a name="bkmk_improvesessionbycode"></a>Zlepšení SessionByCode zobrazení</span><span class="sxs-lookup"><span data-stu-id="6c96a-299"><a name="bkmk_improvesessionbycode"></a> Improve the SessionByCode View</span></span>
<span data-ttu-id="6c96a-300">Nakonec budete opravit *SessionByCode* zobrazení, aby bylo mobilní zařízení.</span><span class="sxs-lookup"><span data-stu-id="6c96a-300">Finally, you'll fix the *SessionByCode* view to make it mobile-friendly.</span></span>

<span data-ttu-id="6c96a-301">Mobilní prohlížeče, klepněte na **značka** tlačítko a potom zadejte `asp` do vyhledávacího pole.</span><span class="sxs-lookup"><span data-stu-id="6c96a-301">In the mobile browser, tap the **Tag** button, then enter `asp` in the search box.</span></span>

![][AllTagsFixedSearchByASP]

<span data-ttu-id="6c96a-302">Klepněte **ASP.NET** odkaz.</span><span class="sxs-lookup"><span data-stu-id="6c96a-302">Tap the **ASP.NET** link.</span></span> <span data-ttu-id="6c96a-303">Relace pro značku ASP.NET se zobrazí.</span><span class="sxs-lookup"><span data-stu-id="6c96a-303">Sessions for the ASP.NET tag are displayed.</span></span>

![][FixedSessionsByTag]

<span data-ttu-id="6c96a-304">Vyberte **vytváření jedné stránky aplikace s ASP.NET a AngularJS** odkaz.</span><span class="sxs-lookup"><span data-stu-id="6c96a-304">Choose the **Building a Single Page Application with ASP.NET and AngularJS** link.</span></span>

![][SessionByCode3-644]

<span data-ttu-id="6c96a-305">Zobrazení plochy výchozí je v pořádku, ale můžete zlepšit vzhled snadno pomocí některé součásti Bootstrap grafického uživatelského rozhraní.</span><span class="sxs-lookup"><span data-stu-id="6c96a-305">The default desktop view is fine, but you can improve the look easily by using some Bootstrap GUI components.</span></span>

<span data-ttu-id="6c96a-306">Otevřete *zobrazení\\Domů\\SessionByCode.cshtml* a nahraďte jeho obsah s následující kód:</span><span class="sxs-lookup"><span data-stu-id="6c96a-306">Open *Views\\Home\\SessionByCode.cshtml* and replace the contents with the following markup:</span></span>

    @model Mvc5Mobile.Models.Session

    @{
        ViewBag.Title = "Session details";
    }
    <h3>@Model.Title (@Model.Code)</h3>
    <p>
        <strong>@Model.DateText</strong> in <strong>@Model.Room</strong>
    </p>

    <div class="panel panel-primary">
        <div class="panel-heading">
            Speakers
        </div>
        @foreach (var speaker in Model.Speakers)
        {
            @Html.ActionLink(speaker, 
                             "SessionsBySpeaker", 
                             new { speaker }, 
                             new { @class="panel-body" })
        }
    </div>

    <p>@Model.Abstract</p>

    <div class="panel panel-primary">
        <div class="panel-heading">
            Tags
        </div>
        @foreach (var tag in Model.Tags)
        {
            @Html.ActionLink(tag, 
                             "SessionsByTag", 
                             new { tag }, 
                             new { @class = "panel-body" })
        }
    </div>

<span data-ttu-id="6c96a-307">Nový kód používá Bootstrap panelů styly ke zlepšení mobilní zobrazení.</span><span class="sxs-lookup"><span data-stu-id="6c96a-307">The new markup uses Bootstrap panels styling to improve the mobile view.</span></span> 

<span data-ttu-id="6c96a-308">Aktualizujte prohlížeč pro mobilní zařízení.</span><span class="sxs-lookup"><span data-stu-id="6c96a-308">Refresh the mobile browser.</span></span> <span data-ttu-id="6c96a-309">Následující obrázek zobrazuje změny kódu, které jste právě vytvořili:</span><span class="sxs-lookup"><span data-stu-id="6c96a-309">The following image reflects the code changes that you just made:</span></span>

![][SessionByCodeFixed3-644]

## <a name="wrap-up-and-review"></a><span data-ttu-id="6c96a-310">Zabalení a zkontrolovat</span><span class="sxs-lookup"><span data-stu-id="6c96a-310">Wrap Up and Review</span></span>
<span data-ttu-id="6c96a-311">Tento kurz vám ukázal jak vyvíjet webové aplikace pro mobilní zařízení pomocí ASP.NET MVC 5.</span><span class="sxs-lookup"><span data-stu-id="6c96a-311">This tutorial has shown you how to use ASP.NET MVC 5 to develop mobile-friendly Web applications.</span></span> <span data-ttu-id="6c96a-312">Mezi ně patří:</span><span class="sxs-lookup"><span data-stu-id="6c96a-312">These include:</span></span>

* <span data-ttu-id="6c96a-313">Nasazení aplikace ASP.NET MVC 5 do webové aplikace služby App Service</span><span class="sxs-lookup"><span data-stu-id="6c96a-313">Deploy an ASP.NET MVC 5 application to an App Service web app</span></span>
* <span data-ttu-id="6c96a-314">Použít Bootstrap k vytvoření rozložení přizpůsobivý webové stránky v aplikaci MVC 5</span><span class="sxs-lookup"><span data-stu-id="6c96a-314">Use Bootstrap to create responsive web layout in your MVC 5 application</span></span>
* <span data-ttu-id="6c96a-315">Přepsání částečné zobrazení, zobrazení a rozložení globálně i pro jednotlivé zobrazení</span><span class="sxs-lookup"><span data-stu-id="6c96a-315">Override layout, views, and partial views, both globally and for an individual view</span></span>
* <span data-ttu-id="6c96a-316">Rozložení ovládacích prvků a částečné přepsat pomocí vynucení `RequireConsistentDisplayMode` vlastnost</span><span class="sxs-lookup"><span data-stu-id="6c96a-316">Control layout and partial override enforcement using the `RequireConsistentDisplayMode` property</span></span>
* <span data-ttu-id="6c96a-317">Vytvoření zobrazení, které cílí určité prohlížeče, jako je třeba iPhone prohlížeč</span><span class="sxs-lookup"><span data-stu-id="6c96a-317">Create views that target specific browsers, such as the iPhone browser</span></span>
* <span data-ttu-id="6c96a-318">Použít Bootstrap stylů v kódu Razor</span><span class="sxs-lookup"><span data-stu-id="6c96a-318">Apply Bootstrap styling in Razor code</span></span>

## <a name="see-also"></a><span data-ttu-id="6c96a-319">Viz také</span><span class="sxs-lookup"><span data-stu-id="6c96a-319">See Also</span></span>
* [<span data-ttu-id="6c96a-320">9 základní principy návrhu přizpůsobivý webu</span><span class="sxs-lookup"><span data-stu-id="6c96a-320">9 basic principles of responsive web design</span></span>](http://blog.froont.com/9-basic-principles-of-responsive-web-design/)
* <span data-ttu-id="6c96a-321">[Bootstrap][BootstrapSite]</span><span class="sxs-lookup"><span data-stu-id="6c96a-321">[Bootstrap][BootstrapSite]</span></span>
* <span data-ttu-id="6c96a-322">[Oficiální Blog zavedení][Official Bootstrap Blog]</span><span class="sxs-lookup"><span data-stu-id="6c96a-322">[Official Bootstrap Blog][Official Bootstrap Blog]</span></span>
* <span data-ttu-id="6c96a-323">[Twitter Bootstrap kurzu z kurzu republika][Twitter Bootstrap Tutorial from Tutorial Republic]</span><span class="sxs-lookup"><span data-stu-id="6c96a-323">[Twitter Bootstrap Tutorial from Tutorial Republic][Twitter Bootstrap Tutorial from Tutorial Republic]</span></span>
* <span data-ttu-id="6c96a-324">[Zavedení Playground][The Bootstrap Playground]</span><span class="sxs-lookup"><span data-stu-id="6c96a-324">[The Bootstrap Playground][The Bootstrap Playground]</span></span>
* <span data-ttu-id="6c96a-325">[Osvědčené postupy W3C doporučení mobilní webové aplikace][W3C Recommendation Mobile Web Application Best Practices]</span><span class="sxs-lookup"><span data-stu-id="6c96a-325">[W3C Recommendation Mobile Web Application Best Practices][W3C Recommendation Mobile Web Application Best Practices]</span></span>
* <span data-ttu-id="6c96a-326">[W3C Candidate doporučení pro dotazy na média][W3C Candidate Recommendation for media queries]</span><span class="sxs-lookup"><span data-stu-id="6c96a-326">[W3C Candidate Recommendation for media queries][W3C Candidate Recommendation for media queries]</span></span>

## <a name="whats-changed"></a><span data-ttu-id="6c96a-327">Co se změnilo</span><span class="sxs-lookup"><span data-stu-id="6c96a-327">What's changed</span></span>
* <span data-ttu-id="6c96a-328">Průvodce změnou z webů na službu App Service naleznete v tématu: [Služba Azure App Service a její vliv na stávající služby Azure](http://go.microsoft.com/fwlink/?LinkId=529714)</span><span class="sxs-lookup"><span data-stu-id="6c96a-328">For a guide to the change from Websites to App Service see: [Azure App Service and Its Impact on Existing Azure Services](http://go.microsoft.com/fwlink/?LinkId=529714)</span></span>

<!-- Internal Links -->
[Deploy the starter project to an Azure web app]: #bkmk_DeployStarterProject
[Bootstrap CSS Framework]: #bkmk_bootstrap
[Override the Views, Layouts, and Partial Views]: #bkmk_overrideviews
[Create Browser-Specific Views]:#bkmk_browserviews
[Improve the Speakers List]: #bkmk_Improvespeakerslist
[Improve the Tags List]: #bkmk_improvetags
[Improve the Dates List]: #bkmk_improvedates
[Improve the SessionsTable View]: #bkmk_improvesessionstable
[Improve the SessionByCode View]: #bkmk_improvesessionbycode

<!-- External Links -->
[Visual Studio Express 2013]: http://www.visualstudio.com/downloads/download-visual-studio-vs#d-express-web
[Visual Studio 2015]: https://www.visualstudio.com/downloads/download-visual-studio-vs
[AzureSDKVs2013]: http://go.microsoft.com/fwlink/p/?linkid=323510&clcid=0x409
[Fiddler]: http://www.fiddler2.com/fiddler2/
[EmulatorIE11]: http://msdn.microsoft.com/library/ie/dn255001.aspx
[EmulatorChrome]: https://developers.google.com/chrome-developer-tools/docs/mobile-emulation
[EmulatorOpera]: http://www.opera.com/developer/tools/mobile/
[StarterProject]: http://go.microsoft.com/fwlink/?LinkID=398780&clcid=0x409
[CompletedProject]: http://go.microsoft.com/fwlink/?LinkID=398781&clcid=0x409
[BootstrapSite]: http://getbootstrap.com/
[WebPIAzureSdk23NetVS13]: ./media/web-sites-dotnet-deploy-aspnet-mvc-mobile-app/WebPIAzureSdk23NetVS13.png
[linked list group]: http://getbootstrap.com/components/#list-group-linked
[glyphicon]: http://getbootstrap.com/components/#glyphicons
[panels]: http://getbootstrap.com/components/#panels
[custom linked list group]: http://getbootstrap.com/components/#list-group-custom-content
[grid system]: http://getbootstrap.com/css/#grid
[responsive utilities]: http://getbootstrap.com/css/#responsive-utilities
[Official Bootstrap Blog]: http://blog.getbootstrap.com/
[Twitter Bootstrap Tutorial from Tutorial Republic]: http://www.tutorialrepublic.com/twitter-bootstrap-tutorial/
[The Bootstrap Playground]: http://www.bootply.com/
[W3C Recommendation Mobile Web Application Best Practices]: http://www.w3.org/TR/mwabp/
[W3C Candidate Recommendation for media queries]: http://www.w3.org/TR/css3-mediaqueries/

<!-- Images -->
[DeployClickPublish]: ./media/web-sites-dotnet-deploy-aspnet-mvc-mobile-app/deploy-to-azure-website-1.png
[DeployClickWebSites]: ./media/web-sites-dotnet-deploy-aspnet-mvc-mobile-app/deploy-to-azure-website-2.png
[DeploySignIn]: ./media/web-sites-dotnet-deploy-aspnet-mvc-mobile-app/deploy-to-azure-website-3.png
[DeployUsername]: ./media/web-sites-dotnet-deploy-aspnet-mvc-mobile-app/deploy-to-azure-website-4.png
[DeployPassword]: ./media/web-sites-dotnet-deploy-aspnet-mvc-mobile-app/deploy-to-azure-website-5.png
[DeployNewWebsite]: ./media/web-sites-dotnet-deploy-aspnet-mvc-mobile-app/deploy-to-azure-website-6.png
[DeploySiteSettings]: ./media/web-sites-dotnet-deploy-aspnet-mvc-mobile-app/deploy-to-azure-website-7.png
[DeployPublishSite]: ./media/web-sites-dotnet-deploy-aspnet-mvc-mobile-app/deploy-to-azure-website-8.png
[MobileHomePage]: ./media/web-sites-dotnet-deploy-aspnet-mvc-mobile-app/mobile-home-page.png
[FixedSessionsByTag]: ./media/web-sites-dotnet-deploy-aspnet-mvc-mobile-app/SessionsByTag-ASP.NET-Fixed.png
[AllTags]: ./media/web-sites-dotnet-deploy-aspnet-mvc-mobile-app/AllTags.png
[SessionsByTagASP.NET]: ./media/web-sites-dotnet-deploy-aspnet-mvc-mobile-app/SessionsByTag-ASP.NET.png
[SessionsByTagASP.NETNoBootstrap]: ./media/web-sites-dotnet-deploy-aspnet-mvc-mobile-app/SessionsByTag-ASP.NET-NoBootstrap.png
[AllTagsMobile_LayoutMobile]: ./media/web-sites-dotnet-deploy-aspnet-mvc-mobile-app/AllTagsMobile-_LayoutMobile.png
[AllTagsMobile_LayoutMobileDesktop]: ./media/web-sites-dotnet-deploy-aspnet-mvc-mobile-app/AllTagsMobile-_LayoutMobile-Desktop.png
[ResolveDefaultDisplayMode]: ./media/web-sites-dotnet-deploy-aspnet-mvc-mobile-app/Resolve-DefaultDisplayMode.png
[AllTagsIPhone_LayoutIPhone]: ./media/web-sites-dotnet-deploy-aspnet-mvc-mobile-app/AllTagsIPhone-_LayoutIPhone.png
[AllSpeakers_LayoutMobile]: ./media/web-sites-dotnet-deploy-aspnet-mvc-mobile-app/AllSpeakers-_LayoutMobile.png
[AllSpeakers_LayoutMobileOverridden]: ./media/web-sites-dotnet-deploy-aspnet-mvc-mobile-app/AllSpeakers-_LayoutMobile-Overridden.png
[AllSpeakersFixed]: ./media/web-sites-dotnet-deploy-aspnet-mvc-mobile-app/AllSpeakers-Fixed.png
[AllSpeakersFixedDesktop]: ./media/web-sites-dotnet-deploy-aspnet-mvc-mobile-app/AllSpeakers-Fixed-Desktop.png
[AllSpeakersFixedSearchBySC]: ./media/web-sites-dotnet-deploy-aspnet-mvc-mobile-app/AllSpeakers-Fixed-SearchBySC.png
[AllTagsFixedDesktop]: ./media/web-sites-dotnet-deploy-aspnet-mvc-mobile-app/AllTags-Fixed-Desktop.png 
[AllTagsFixed]: ./media/web-sites-dotnet-deploy-aspnet-mvc-mobile-app/AllTags-Fixed.png
[AllDatesFixed]: ./media/web-sites-dotnet-deploy-aspnet-mvc-mobile-app/AllDates-Fixed.png
[AllDatesFixed2]: ./media/web-sites-dotnet-deploy-aspnet-mvc-mobile-app/AllDates-Fixed2.png
[AllDatesFixed2Desktop]: ./media/web-sites-dotnet-deploy-aspnet-mvc-mobile-app/AllDates-Fixed2-Desktop.png
[AllTagsFixedSearchByASP]: ./media/web-sites-dotnet-deploy-aspnet-mvc-mobile-app/AllTags-Fixed-SearchByASP.png
[SessionsTableTagASP.NET]: ./media/web-sites-dotnet-deploy-aspnet-mvc-mobile-app/SessionsTable-Tag-ASP.NET.png
[SessionsTableFixedTagASP.NETDesktop]: ./media/web-sites-dotnet-deploy-aspnet-mvc-mobile-app/SessionsTable-Fixed-Tag-ASP.NET-Desktop.png
[SessionByCode3-644]: ./media/web-sites-dotnet-deploy-aspnet-mvc-mobile-app/SessionByCode-3-644.png
[SessionByCodeFixed3-644]: ./media/web-sites-dotnet-deploy-aspnet-mvc-mobile-app/SessionByCode-Fixed-3-644.png

