---
title: "aaaDeploy ASP.NET MVC 5 mobilní webové aplikace v Azure App Service"
description: "Kurz, který se naučíte, jak toodeploy tooAzure webové aplikace služby App Service pomocí mobilní funkce v architektuře ASP.NET MVC 5 webové aplikace."
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
ms.openlocfilehash: 01119c07246c0252fd357562774a2e90b3ef77d0
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="deploy-an-aspnet-mvc-5-mobile-web-app-in-azure-app-service"></a><span data-ttu-id="4fed6-103">Nasazení mobilní webové aplikace ASP.NET MVC 5 v Azure App Service</span><span class="sxs-lookup"><span data-stu-id="4fed6-103">Deploy an ASP.NET MVC 5 mobile web app in Azure App Service</span></span>
<span data-ttu-id="4fed6-104">V tomto kurzu se naučit hello základní informace o tom, jak toobuild ASP.NET MVC 5 webové aplikaci, která je mobilní zařízení a nasadit tooAzure služby App Service.</span><span class="sxs-lookup"><span data-stu-id="4fed6-104">This tutorial will teach you hello basics of how toobuild an ASP.NET MVC 5 web app that is mobile-friendly and deploy it tooAzure App Service.</span></span> <span data-ttu-id="4fed6-105">V tomto kurzu budete potřebovat [Visual Studio Express 2013 pro Web] [ Visual Studio Express 2013] nebo edice professional hello sady Visual Studio, pokud již máte který.</span><span class="sxs-lookup"><span data-stu-id="4fed6-105">For this tutorial, you need [Visual Studio Express 2013 for Web][Visual Studio Express 2013] or hello professional edition of Visual Studio if you already have that.</span></span> <span data-ttu-id="4fed6-106">Můžete použít [Visual Studio 2015] , ale snímky obrazovky hello se bude lišit a je nutné použít hello ASP.NET 4.x šablony.</span><span class="sxs-lookup"><span data-stu-id="4fed6-106">You can use [Visual Studio 2015] but hello screen shots will be different and you must use hello ASP.NET 4.x templates.</span></span>

[!INCLUDE [create-account-and-websites-note](../../includes/create-account-and-websites-note.md)]

## <a name="what-youll-build"></a><span data-ttu-id="4fed6-107">Co budete sestavení</span><span class="sxs-lookup"><span data-stu-id="4fed6-107">What You'll Build</span></span>
<span data-ttu-id="4fed6-108">V tomto kurzu přidáte mobilní funkce toohello jednoduchou konferenční výpis aplikaci, která je k dispozici v hello [starter projektu][StarterProject].</span><span class="sxs-lookup"><span data-stu-id="4fed6-108">For this tutorial, you'll add mobile features toohello simple conference-listing application that's provided in hello [starter project][StarterProject].</span></span> <span data-ttu-id="4fed6-109">Hello následující snímek obrazovky ukazuje relací ASP.NET hello v aplikaci hello dokončit, jak je vidět v emulátoru hello prohlížeče v Internet Exploreru 11 F12 nástroje pro vývojáře.</span><span class="sxs-lookup"><span data-stu-id="4fed6-109">hello following screenshot shows hello ASP.NET sessions in hello completed application, as seen in hello browser emulator in Internet Explorer 11 F12 developer tools.</span></span>

![][FixedSessionsByTag]

<span data-ttu-id="4fed6-110">Můžete použít nástroje pro vývojáře hello Internet Explorer 11 F12 a hello [nástroj Fiddler] [ Fiddler] toohelp ladění aplikace.</span><span class="sxs-lookup"><span data-stu-id="4fed6-110">You can use hello Internet Explorer 11 F12 developer tools and hello [Fiddler tool][Fiddler] toohelp debug your application.</span></span> 

## <a name="skills-youll-learn"></a><span data-ttu-id="4fed6-111">Dovedností, které se dozvíte</span><span class="sxs-lookup"><span data-stu-id="4fed6-111">Skills You'll Learn</span></span>
<span data-ttu-id="4fed6-112">Zde je, co se dozvíte:</span><span class="sxs-lookup"><span data-stu-id="4fed6-112">Here's what you'll learn:</span></span>

* <span data-ttu-id="4fed6-113">Jak toouse Visual Studio 2013 toopublish webové aplikace přímo tooa webové aplikace v Azure App Service.</span><span class="sxs-lookup"><span data-stu-id="4fed6-113">How toouse Visual Studio 2013 toopublish your web application directly tooa web app in Azure App Service.</span></span>
* <span data-ttu-id="4fed6-114">Jak hello ASP.NET MVC 5 šablony pomocí šablon stylů CSS Bootstrap framework hello zlepšit zobrazení na mobilních zařízeních</span><span class="sxs-lookup"><span data-stu-id="4fed6-114">How hello ASP.NET MVC 5 templates use hello CSS Bootstrap framework to improve display on mobile devices</span></span>
* <span data-ttu-id="4fed6-115">Jak toocreate specifické mobilní zobrazení tootarget konkrétní mobilní prohlížeče, jako je hello iPhone a Android</span><span class="sxs-lookup"><span data-stu-id="4fed6-115">How toocreate mobile-specific views tootarget specific mobile browsers, such as hello iPhone and Android</span></span>
* <span data-ttu-id="4fed6-116">Jak toocreate přizpůsobivý zobrazení (zobrazení, které reagují na zařízeních toodifferent prohlížeče)</span><span class="sxs-lookup"><span data-stu-id="4fed6-116">How toocreate responsive views (views that respond toodifferent browsers across devices)</span></span>

## <a name="set-up-hello-development-environment"></a><span data-ttu-id="4fed6-117">Nastavit hello vývojového prostředí</span><span class="sxs-lookup"><span data-stu-id="4fed6-117">Set up hello development environment</span></span>
<span data-ttu-id="4fed6-118">Nastavení vývojového prostředí instalací hello Azure SDK pro .NET 2.5.1 nebo novější.</span><span class="sxs-lookup"><span data-stu-id="4fed6-118">Set up your development environment by installing hello Azure SDK for .NET 2.5.1 or later.</span></span> 

1. <span data-ttu-id="4fed6-119">tooinstall hello Azure SDK pro platformu .NET, klikněte na níže uvedený odkaz hello.</span><span class="sxs-lookup"><span data-stu-id="4fed6-119">tooinstall hello Azure SDK for .NET, click hello link below.</span></span> <span data-ttu-id="4fed6-120">Pokud nemáte Visual Studio 2013 ještě nainstalované, nainstaluje se podle hello propojení.</span><span class="sxs-lookup"><span data-stu-id="4fed6-120">If you don't have Visual Studio 2013 installed yet, it will be installed by hello link.</span></span> <span data-ttu-id="4fed6-121">Tento kurz vyžaduje Visual Studio 2013.</span><span class="sxs-lookup"><span data-stu-id="4fed6-121">This tutorial requires Visual Studio 2013.</span></span> <span data-ttu-id="4fed6-122">[Azure SDK pro Visual Studio 2013][AzureSDKVs2013]</span><span class="sxs-lookup"><span data-stu-id="4fed6-122">[Azure SDK for Visual Studio 2013][AzureSDKVs2013]</span></span>
2. <span data-ttu-id="4fed6-123">V okně hello instalačního programu webové platformy, klikněte na tlačítko **nainstalovat** a pokračovat v instalaci hello.</span><span class="sxs-lookup"><span data-stu-id="4fed6-123">In hello Web Platform Installer window, click **Install** and proceed with hello installation.</span></span>

<span data-ttu-id="4fed6-124">Budete také potřebovat emulátoru prohlížeč pro mobilní zařízení.</span><span class="sxs-lookup"><span data-stu-id="4fed6-124">You will also need a mobile browser emulator.</span></span> <span data-ttu-id="4fed6-125">Některé z následujících hello bude fungovat:</span><span class="sxs-lookup"><span data-stu-id="4fed6-125">Any of hello following will work:</span></span>

* <span data-ttu-id="4fed6-126">Emulátor prohlížeče v [nástroje pro vývojáře aplikace Internet Explorer 11 F12] [ EmulatorIE11] (používá se v všechny snímky obrazovky prohlížeč pro mobilní zařízení).</span><span class="sxs-lookup"><span data-stu-id="4fed6-126">Browser Emulator in [Internet Explorer 11 F12 developer tools][EmulatorIE11] (used in all mobile browser screenshots).</span></span> <span data-ttu-id="4fed6-127">Má přednastavení řetězec uživatelského agenta pro Windows Phone 8, Windows Phone 7 a Apple iPad.</span><span class="sxs-lookup"><span data-stu-id="4fed6-127">It has user agent string presets for Windows Phone 8, Windows Phone 7, and Apple iPad.</span></span>
* <span data-ttu-id="4fed6-128">Emulátor prohlížeče v [Google Chrome DevTools][EmulatorChrome].</span><span class="sxs-lookup"><span data-stu-id="4fed6-128">Browser Emulator in [Google Chrome DevTools][EmulatorChrome].</span></span> <span data-ttu-id="4fed6-129">Obsahuje přednastavení množství zařízení se systémem Android, a také Apple iPhone, Apple iPad a Amazon Kindle ještě efektivněji.</span><span class="sxs-lookup"><span data-stu-id="4fed6-129">It contains presets for numerous Android devices, as well as Apple iPhone, Apple iPad, and Amazon Kindle Fire.</span></span> <span data-ttu-id="4fed6-130">Emuluje také touch události.</span><span class="sxs-lookup"><span data-stu-id="4fed6-130">It also emulates touch events.</span></span>
* <span data-ttu-id="4fed6-131">[Emulátoru mobilního Opera][EmulatorOpera]</span><span class="sxs-lookup"><span data-stu-id="4fed6-131">[Opera Mobile Emulator][EmulatorOpera]</span></span>

<span data-ttu-id="4fed6-132">Projekty Visual Studio s C\# zdrojového kódu jsou k dispozici tooaccompany v tomto tématu:</span><span class="sxs-lookup"><span data-stu-id="4fed6-132">Visual Studio projects with C\# source code are available tooaccompany this topic:</span></span>

* <span data-ttu-id="4fed6-133">[Stažení Starter projektu][StarterProject]</span><span class="sxs-lookup"><span data-stu-id="4fed6-133">[Starter project download][StarterProject]</span></span>
* <span data-ttu-id="4fed6-134">[Dokončení stažení projektu][CompletedProject]</span><span class="sxs-lookup"><span data-stu-id="4fed6-134">[Completed project download][CompletedProject]</span></span>

## <span data-ttu-id="4fed6-135"><a name="bkmk_DeployStarterProject"></a>Nasazení hello starter projektu tooan Azure webové aplikace</span><span class="sxs-lookup"><span data-stu-id="4fed6-135"><a name="bkmk_DeployStarterProject"></a>Deploy hello starter project tooan Azure web app</span></span>
1. <span data-ttu-id="4fed6-136">Stažení aplikace hello konferenční výpis [starter projektu][StarterProject].</span><span class="sxs-lookup"><span data-stu-id="4fed6-136">Download hello conference-listing application [starter project][StarterProject].</span></span>
2. <span data-ttu-id="4fed6-137">Potom v Průzkumníku Windows, klikněte pravým tlačítkem na soubor ZIP hello stáhli a zvolte *vlastnosti*.</span><span class="sxs-lookup"><span data-stu-id="4fed6-137">Then in Windows Explorer, right-click hello downloaded ZIP file and choose *Properties*.</span></span>
3. <span data-ttu-id="4fed6-138">V hello **vlastnosti** dialogovém okně vyberte hello **Odblokovat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="4fed6-138">In hello **Properties** dialog box, choose hello **Unblock** button.</span></span> <span data-ttu-id="4fed6-139">(Odblokování brání upozornění zabezpečení, která nastane, když toouse *.zip* souboru, který jste stáhli z webové hello.)</span><span class="sxs-lookup"><span data-stu-id="4fed6-139">(Unblocking prevents a security warning that occurs when you try toouse a *.zip* file that you've downloaded from hello web.)</span></span>
4. <span data-ttu-id="4fed6-140">Klikněte pravým tlačítkem na soubor ZIP hello a vyberte **Extrahovat vše** dekomprimovat soubor hello.</span><span class="sxs-lookup"><span data-stu-id="4fed6-140">Right-click hello ZIP file and select **Extract All** to unzip hello file.</span></span> 
5. <span data-ttu-id="4fed6-141">V sadě Visual Studio otevřete hello *C#\Mvc5Mobile.sln* souboru.</span><span class="sxs-lookup"><span data-stu-id="4fed6-141">In Visual Studio, open hello *C#\Mvc5Mobile.sln* file.</span></span>
6. <span data-ttu-id="4fed6-142">V Průzkumníku řešení klikněte pravým tlačítkem na projekt hello a klikněte na tlačítko **publikovat**.</span><span class="sxs-lookup"><span data-stu-id="4fed6-142">In Solution Explorer, right-click hello project and click **Publish**.</span></span>
   
   ![][DeployClickPublish]
7. <span data-ttu-id="4fed6-143">V Publikovat Web, klikněte na **Microsoft Azure App Service**.</span><span class="sxs-lookup"><span data-stu-id="4fed6-143">In Publish Web, click **Microsoft Azure App Service**.</span></span>
   
   ![][DeployClickWebSites]
8. <span data-ttu-id="4fed6-144">Pokud již jste přihlášeni do Azure, klikněte na tlačítko **přidat účet**.</span><span class="sxs-lookup"><span data-stu-id="4fed6-144">If you haven't already logged into Azure, click **Add an account**.</span></span>
   
   ![][DeploySignIn]
9. <span data-ttu-id="4fed6-145">Postupujte podle pokynů toolog hello ke svému účtu Azure.</span><span class="sxs-lookup"><span data-stu-id="4fed6-145">Follow hello prompts toolog into your Azure account.</span></span>
10. <span data-ttu-id="4fed6-146">Hello dialogovém okně App Service, by měl nyní může zobrazit můžete jako přihlášení.</span><span class="sxs-lookup"><span data-stu-id="4fed6-146">hello App Service dialog should now show you as signed in.</span></span> <span data-ttu-id="4fed6-147">Klikněte na možnost **Nové**.</span><span class="sxs-lookup"><span data-stu-id="4fed6-147">Click **New**.</span></span>
    
    ![][DeployNewWebsite]  
11. <span data-ttu-id="4fed6-148">V hello **název webové aplikace** pole, zadejte předponu aplikace jedinečný název.</span><span class="sxs-lookup"><span data-stu-id="4fed6-148">In hello **Web App Name** field, specify a unique app name prefix.</span></span> <span data-ttu-id="4fed6-149">Váš název plně kvalifikovaný webové aplikace bude  *&lt;předpony >*. azurewebsites.net.</span><span class="sxs-lookup"><span data-stu-id="4fed6-149">Your fully-qualified web app name will be *&lt;prefix>*.azurewebsites.net.</span></span> <span data-ttu-id="4fed6-150">Také vyberte nebo zadejte nový název skupiny prostředků v **skupiny prostředků**.</span><span class="sxs-lookup"><span data-stu-id="4fed6-150">Also, select or specify a new resource group name in **Resource group**.</span></span> <span data-ttu-id="4fed6-151">Potom klikněte na **nový** toocreate nový plán aplikační služby.</span><span class="sxs-lookup"><span data-stu-id="4fed6-151">Then, click **New** toocreate a new App Service plan.</span></span>
    
    ![][DeploySiteSettings]
12. <span data-ttu-id="4fed6-152">Nakonfigurujte hello nový plán aplikační služby a klikněte na **OK**.</span><span class="sxs-lookup"><span data-stu-id="4fed6-152">Configure hello new App Service plan and click **OK**.</span></span> 
    
    ![](./media/web-sites-dotnet-deploy-aspnet-mvc-mobile-app/deploy-to-azure-website-7a.png)
13. <span data-ttu-id="4fed6-153">Zpět v dialogovém okně vytvořit službu App Service hello, klikněte na tlačítko **vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="4fed6-153">Back in hello Create App Service dialog, click **Create**.</span></span>
    
    ![](./media/web-sites-dotnet-deploy-aspnet-mvc-mobile-app/deploy-to-azure-website-7b.png) 
14. <span data-ttu-id="4fed6-154">Po hello prostředků Azure jsou vytvoření dialogového okna Publikovat Web hello bude vyplněn hello nastavení pro novou aplikaci.</span><span class="sxs-lookup"><span data-stu-id="4fed6-154">After hello Azure resources are created, hello Publish Web dialog will be filled with hello settings for your new app.</span></span> <span data-ttu-id="4fed6-155">Klikněte na **Publikovat**.</span><span class="sxs-lookup"><span data-stu-id="4fed6-155">Click **Publish**.</span></span>
    
    ![][DeployPublishSite]
    
    <span data-ttu-id="4fed6-156">Po dokončení publikování hello úvodní projektu toohello Azure webovou aplikaci Visual Studio otevře prohlížeč pro stolní počítač hello toodisplay hello živou webovou aplikaci.</span><span class="sxs-lookup"><span data-stu-id="4fed6-156">Once Visual Studio finishes publishing hello starter project toohello Azure web app, hello desktop browser opens toodisplay hello live web app.</span></span>
15. <span data-ttu-id="4fed6-157">Spusťte emulátor váš prohlížeč pro mobilní zařízení, zkopírujte adresu URL aplikace hello konferenční hello (*<prefix>*. azurewebsites.net) do hello emulátoru a pak klikněte na tlačítko pravém horním a vyberte **Procházet podle značky**.</span><span class="sxs-lookup"><span data-stu-id="4fed6-157">Start your mobile browser emulator, copy hello URL for hello conference application (*<prefix>*.azurewebsites.net) into hello emulator, and then click the top-right button and select **Browse by tag**.</span></span> <span data-ttu-id="4fed6-158">Pokud používáte Internet Explorer 11 jako hello výchozí prohlížeč, bude třeba jen tootype `F12`, pak `Ctrl+8`a poté změňte hello prohlížeče profil příliš**Windows Phone**.</span><span class="sxs-lookup"><span data-stu-id="4fed6-158">If you are using Internet Explorer 11 as hello default browser, you just need tootype `F12`, then `Ctrl+8`, and then change hello browser profile too**Windows Phone**.</span></span> <span data-ttu-id="4fed6-159">Následující obrázek ukazuje hello *AllTags* zobrazení v režimu na výšku (z výběru **Procházet podle značky**).</span><span class="sxs-lookup"><span data-stu-id="4fed6-159">The image below shows hello *AllTags* view in portrait mode (from choosing **Browse by tag**).</span></span>
    
    ![][AllTags]

> [!TIP]
> <span data-ttu-id="4fed6-160">Zatímco lze ladit aplikace MVC 5 v sadě Visual Studio, můžete publikovat vaší webové aplikace tooAzure znovu tooverify hello živou webovou aplikaci přímo z prohlížeče emulátoru nebo prohlížeč pro mobilní zařízení.</span><span class="sxs-lookup"><span data-stu-id="4fed6-160">While you can debug your MVC 5 application from within Visual Studio, you can publish your web app tooAzure again tooverify hello live web app directly from your mobile browser or a browser emulator.</span></span>
> 
> 

<span data-ttu-id="4fed6-161">zobrazení Hello je velmi čitelná na mobilním zařízení.</span><span class="sxs-lookup"><span data-stu-id="4fed6-161">hello display is very readable on a mobile device.</span></span> <span data-ttu-id="4fed6-162">Můžete také již zobrazit některé vizuální efekty hello použít rámcem hello Bootstrap šablon stylů CSS.</span><span class="sxs-lookup"><span data-stu-id="4fed6-162">You can also already see some of hello visual effects applied by hello Bootstrap CSS framework.</span></span>
<span data-ttu-id="4fed6-163">Klikněte na tlačítko hello **ASP.NET** odkaz.</span><span class="sxs-lookup"><span data-stu-id="4fed6-163">Click hello **ASP.NET** link.</span></span>

![][SessionsByTagASP.NET]

<span data-ttu-id="4fed6-164">zobrazení značek ASP.NET Hello je namontováno přiblížení toohello obrazovce, která nemá Bootstrap pro vás automaticky.</span><span class="sxs-lookup"><span data-stu-id="4fed6-164">hello ASP.NET tag view is zoom-fitted toohello screen, which Bootstrap does for you automatically.</span></span> <span data-ttu-id="4fed6-165">Však může zvýšit tento zobrazení toobetter barvy hello prohlížeč pro mobilní zařízení.</span><span class="sxs-lookup"><span data-stu-id="4fed6-165">However, you can improve this view toobetter suit hello mobile browser.</span></span> <span data-ttu-id="4fed6-166">Například hello **datum** sloupec je obtížné číst.</span><span class="sxs-lookup"><span data-stu-id="4fed6-166">For example, hello **Date** column is difficult to read.</span></span> <span data-ttu-id="4fed6-167">Později v kurzu hello změníte hello *AllTags* zobrazení toomake je mobilní zařízení.</span><span class="sxs-lookup"><span data-stu-id="4fed6-167">Later in hello tutorial you'll change hello *AllTags* view toomake it mobile-friendly.</span></span>

## <span data-ttu-id="4fed6-168"><a name="bkmk_bootstrap"></a>Framework Bootstrap šablon stylů CSS</span><span class="sxs-lookup"><span data-stu-id="4fed6-168"><a name="bkmk_bootstrap"></a> Bootstrap CSS Framework</span></span>
<span data-ttu-id="4fed6-169">V hello MVC 5 nová šablona je integrovanou podporu zavedení.</span><span class="sxs-lookup"><span data-stu-id="4fed6-169">New in hello MVC 5 template is built-in Bootstrap support.</span></span> <span data-ttu-id="4fed6-170">Jste už viděli, jak okamžitě vylepšuje hello různá zobrazení ve vaší aplikaci.</span><span class="sxs-lookup"><span data-stu-id="4fed6-170">You have already seen how it immediately improves hello different views in your application.</span></span> <span data-ttu-id="4fed6-171">Navigační panel hello v horní části hello je například automaticky sbalitelné po menší šířka prohlížeče hello.</span><span class="sxs-lookup"><span data-stu-id="4fed6-171">For example, hello navigation bar at hello top is automatically collapsible when hello browser width is smaller.</span></span> <span data-ttu-id="4fed6-172">Na prohlížeč pro stolní počítač hello pokuste se změna velikosti hello okno prohlížeče a v tématu Jak hello navigační panel změní jeho vzhled a chování.</span><span class="sxs-lookup"><span data-stu-id="4fed6-172">On hello desktop browser, try resizing hello browser window and see how hello navigation bar changes its look and feel.</span></span> <span data-ttu-id="4fed6-173">Toto je návrh hello přizpůsobivý webu, který je součástí Bootstrap.</span><span class="sxs-lookup"><span data-stu-id="4fed6-173">This is hello responsive web design that is built into Bootstrap.</span></span>

<span data-ttu-id="4fed6-174">toosee jak webová aplikace hello vypadat bez Bootstrap, otevřete *aplikace\_spustit\\BundleConfig.cs* a Odkomentujte hello řádky, které obsahují *bootstrap.js* a *bootstrap.css*.</span><span class="sxs-lookup"><span data-stu-id="4fed6-174">toosee how hello Web app would look without Bootstrap, open *App\_Start\\BundleConfig.cs* and comment out hello lines that contain *bootstrap.js* and *bootstrap.css*.</span></span> <span data-ttu-id="4fed6-175">Hello následující kód ukazuje hello poslední dva příkazy hello `RegisterBundles` metoda po změně hello:</span><span class="sxs-lookup"><span data-stu-id="4fed6-175">hello following code shows hello last two statements of hello `RegisterBundles` method after hello change:</span></span>

     bundles.Add(new ScriptBundle("~/bundles/bootstrap").Include(
              //"~/Scripts/bootstrap.js",
              "~/Scripts/respond.js"));

    bundles.Add(new StyleBundle("~/Content/css").Include(
              //"~/Content/bootstrap.css",
              "~/Content/site.css"));

<span data-ttu-id="4fed6-176">Stiskněte klávesu `Ctrl+F5` toorun hello aplikace.</span><span class="sxs-lookup"><span data-stu-id="4fed6-176">Press `Ctrl+F5` toorun hello application.</span></span>

<span data-ttu-id="4fed6-177">Zkontrolujte, že tento hello sbalitelné navigační panel je teď právě obyčejnou neuspořádaný seznam.</span><span class="sxs-lookup"><span data-stu-id="4fed6-177">Observe that hello collapsible navigation bar is now just an ordinary unordered list.</span></span> <span data-ttu-id="4fed6-178">Klikněte na tlačítko **Procházet podle značky** znovu, pak klikněte na tlačítko **ASP.NET**.</span><span class="sxs-lookup"><span data-stu-id="4fed6-178">Click **Browse by tag** again, then click **ASP.NET**.</span></span>
<span data-ttu-id="4fed6-179">V zobrazení emulátoru mobilního hello uvidíte nyní, když je už namontováno přiblížení toohello obrazovky a musí ze strany posouvání pořadí toosee hello pravé straně hello tabulky.</span><span class="sxs-lookup"><span data-stu-id="4fed6-179">In hello mobile emulator view, you can see now that it is no longer zoom-fitted toohello screen, and you must scroll sideways in order toosee hello right side of hello table.</span></span>

![][SessionsByTagASP.NETNoBootstrap]

<span data-ttu-id="4fed6-180">Vrátit zpět změny a aktualizujte hello prohlížeč pro mobilní zařízení tooverify obnovila zobrazení hello mobilní zařízení.</span><span class="sxs-lookup"><span data-stu-id="4fed6-180">Undo your changes and refresh hello mobile browser tooverify that hello mobile-friendly display has been restored.</span></span>

<span data-ttu-id="4fed6-181">Bootstrap není konkrétní tooASP.NET MVC 5 a můžete využít výhod těchto funkcí v jakékoli webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="4fed6-181">Bootstrap is not specific tooASP.NET MVC 5, and you can take advantage of these features in any web application.</span></span> <span data-ttu-id="4fed6-182">Ale nyní integrovaná do šablony projektu ASP.NET MVC 5, tak, aby MVC 5 webové aplikace mohou využít výhod Bootstrap ve výchozím nastavení.</span><span class="sxs-lookup"><span data-stu-id="4fed6-182">But it is now built into the ASP.NET MVC 5 project template, so that your MVC 5 Web application can take advantage of Bootstrap by default.</span></span>

<span data-ttu-id="4fed6-183">Další informace o Bootstrap přejděte toothe [Bootstrap] [ BootstrapSite] lokality.</span><span class="sxs-lookup"><span data-stu-id="4fed6-183">For more information about Bootstrap, go toothe [Bootstrap][BootstrapSite] site.</span></span>

<span data-ttu-id="4fed6-184">V další části hello uvidíte jak tooprovide mobilní prohlížeče konkrétní zobrazení.</span><span class="sxs-lookup"><span data-stu-id="4fed6-184">In hello next section you'll see how tooprovide mobile-browser specific views.</span></span>

## <span data-ttu-id="4fed6-185"><a name="bkmk_overrideviews"></a>Přepsání hello zobrazení, rozložení a částečné zobrazení</span><span class="sxs-lookup"><span data-stu-id="4fed6-185"><a name="bkmk_overrideviews"></a> Override hello Views, Layouts, and Partial Views</span></span>
<span data-ttu-id="4fed6-186">Pro mobilní prohlížeče obecně pro jednotlivé mobilního prohlížeče nebo pro jakékoli konkrétní prohlížeč můžete přepsat všechna zobrazení (včetně rozložení a částečné zobrazení).</span><span class="sxs-lookup"><span data-stu-id="4fed6-186">You can override any view (including layouts and partial views) for mobile browsers in general, for an individual mobile browser, or for any specific browser.</span></span> <span data-ttu-id="4fed6-187">Zobrazit tooprovide konkrétního mobile, můžete zkopírovat soubor zobrazení a přidat *. Mobilní* toohello název souboru.</span><span class="sxs-lookup"><span data-stu-id="4fed6-187">tooprovide a mobile-specific view, you can copy a view file and add *.Mobile* toohello file name.</span></span> <span data-ttu-id="4fed6-188">Například toocreate mobilních *Index* zobrazení, můžete zkopírovat *zobrazení\\Domů\\Index.cshtml* k *zobrazení\\Domů\\ Index.Mobile.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="4fed6-188">For example, toocreate a mobile *Index* view, you can copy *Views\\Home\\Index.cshtml* to *Views\\Home\\Index.Mobile.cshtml*.</span></span>

<span data-ttu-id="4fed6-189">V této části vytvoříte soubor specifické mobilní rozložení.</span><span class="sxs-lookup"><span data-stu-id="4fed6-189">In this section, you'll create a mobile-specific layout file.</span></span>

<span data-ttu-id="4fed6-190">toostart kopie *zobrazení\\sdílené\\\_Layout.cshtml* k *zobrazení\\sdílené\\\_Layout.Mobile.cshtml* .</span><span class="sxs-lookup"><span data-stu-id="4fed6-190">toostart, copy *Views\\Shared\\\_Layout.cshtml* to *Views\\Shared\\\_Layout.Mobile.cshtml*.</span></span> <span data-ttu-id="4fed6-191">Otevřete  *\_Layout.Mobile.cshtml* a změňte název hello z **MVC5 aplikace** příliš**MVC5 aplikace (mobilní)**.</span><span class="sxs-lookup"><span data-stu-id="4fed6-191">Open *\_Layout.Mobile.cshtml* and change hello title from **MVC5 Application** too**MVC5 Application (Mobile)**.</span></span>

<span data-ttu-id="4fed6-192">V každé `Html.ActionLink` volání pro hello navigačním panelu, odeberte "Procházet podle" v každé propojení *ActionLink*.</span><span class="sxs-lookup"><span data-stu-id="4fed6-192">In each `Html.ActionLink` call for hello navigation bar, remove "Browse by" in each link *ActionLink*.</span></span> <span data-ttu-id="4fed6-193">Hello následující kód ukazuje hello Dokončit `<ul class="nav navbar-nav">` značky hello mobilní rozložení souboru.</span><span class="sxs-lookup"><span data-stu-id="4fed6-193">hello following code shows hello completed `<ul class="nav navbar-nav">` tag of hello mobile layout file.</span></span>

    <ul class="nav navbar-nav">
        <li>@Html.ActionLink("Home", "Index", "Home")</li>
        <li>@Html.ActionLink("Date", "AllDates", "Home")</li>
        <li>@Html.ActionLink("Speaker", "AllSpeakers", "Home")</li>
        <li>@Html.ActionLink("Tag", "AllTags", "Home")</li>
    </ul>

<span data-ttu-id="4fed6-194">Kopírování hello *zobrazení\\Domů\\AllTags.cshtml* do souboru *zobrazení\\Domů\\AllTags.Mobile.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="4fed6-194">Copy hello *Views\\Home\\AllTags.cshtml* file to *Views\\Home\\AllTags.Mobile.cshtml*.</span></span> <span data-ttu-id="4fed6-195">Otevřete nový soubor hello a změňte `<h2>` element z "Značky" příliš "značky (M)":</span><span class="sxs-lookup"><span data-stu-id="4fed6-195">Open hello new file and change the `<h2>` element from "Tags" too"Tags (M)":</span></span>

    <h2>Tags (M)</h2>

<span data-ttu-id="4fed6-196">Procházet toohello značky stránky pomocí prohlížeč pro stolní počítač a pomocí emulátoru prohlížeč pro mobilní zařízení.</span><span class="sxs-lookup"><span data-stu-id="4fed6-196">Browse toohello tags page using a desktop browser and using mobile browser emulator.</span></span> <span data-ttu-id="4fed6-197">Hello prohlížeč pro mobilní zařízení emulátoru ukazuje hello dvě změny (hello titul z  *\_Layout.Mobile.cshtml* a hello titul z *AllTags.Mobile.cshtml*).</span><span class="sxs-lookup"><span data-stu-id="4fed6-197">hello mobile browser emulator shows hello two changes you made (hello title from *\_Layout.Mobile.cshtml* and hello title from *AllTags.Mobile.cshtml*).</span></span>

![][AllTagsMobile_LayoutMobile]

<span data-ttu-id="4fed6-198">Naproti tomu nedošlo ke změně zobrazení plochy hello (s názvy z  *\_Layout.cshtml* a *AllTags.cshtml*).</span><span class="sxs-lookup"><span data-stu-id="4fed6-198">In contrast, hello desktop display has not changed (with titles from *\_Layout.cshtml* and *AllTags.cshtml*).</span></span>

![][AllTagsMobile_LayoutMobileDesktop]

## <span data-ttu-id="4fed6-199"><a name="bkmk_browserviews"></a>Vytvoření vlastních zobrazení</span><span class="sxs-lookup"><span data-stu-id="4fed6-199"><a name="bkmk_browserviews"></a> Create Browser-Specific Views</span></span>
<span data-ttu-id="4fed6-200">Kromě toho toomobile-desktop specifické a zobrazení, můžete vytvořit zobrazení pro jednotlivé prohlížeče.</span><span class="sxs-lookup"><span data-stu-id="4fed6-200">In addition toomobile-specific and desktop-specific views, you can create views for an individual browser.</span></span> <span data-ttu-id="4fed6-201">Můžete například vytvořit zobrazení, které jsou speciálně určené pro hello iPhone nebo hello prohlížeč systému Android.</span><span class="sxs-lookup"><span data-stu-id="4fed6-201">For example, you can create views that are specifically for hello iPhone or hello Android browser.</span></span> <span data-ttu-id="4fed6-202">V této části vytvoříte rozložení pro prohlížeč hello iPhone a na zařízení iPhone verzi hello *AllTags* zobrazení.</span><span class="sxs-lookup"><span data-stu-id="4fed6-202">In this section, you'll create a layout for hello iPhone browser and an iPhone version of hello *AllTags* view.</span></span>

<span data-ttu-id="4fed6-203">Otevřete hello *Global.asax* souboru a přidejte následující kód toohello dolní části hello `Application_Start` metoda.</span><span class="sxs-lookup"><span data-stu-id="4fed6-203">Open hello *Global.asax* file and add hello following code toohello bottom of the `Application_Start` method.</span></span>

    DisplayModeProvider.Instance.Modes.Insert(0, new DefaultDisplayMode("iPhone")
    {
        ContextCondition = (context => context.GetOverriddenUserAgent().IndexOf
            ("iPhone", StringComparison.OrdinalIgnoreCase) >= 0)
    });

<span data-ttu-id="4fed6-204">Tento kód definuje nový režim zobrazení s názvem "iPhone", který bude porovnání jednotlivých příchozích požadavků.</span><span class="sxs-lookup"><span data-stu-id="4fed6-204">This code defines a new display mode named "iPhone" that will be matched against each incoming request.</span></span> <span data-ttu-id="4fed6-205">Pokud hello příchozí požadavek odpovídá podmínku, kterou jste definovali (to znamená, pokud uživatelský agent hello obsahuje řetězec hello "iPhone"), rozhraní ASP.NET MVC bude hledat zobrazení, jejíž název obsahuje příponu "iPhone".</span><span class="sxs-lookup"><span data-stu-id="4fed6-205">If hello incoming request matches the condition you defined (that is, if hello user agent contains hello string "iPhone"), ASP.NET MVC will look for views whose name contains the "iPhone" suffix.</span></span>

> [!NOTE]
> <span data-ttu-id="4fed6-206">Při přidávání mobilní prohlížeče specifické pro režimy zobrazení, například pro iPhone a Android, že tooset hello první argument je příliš`0` toomake (vložení hello horní části seznamu hello), že tento režim specifické pro prohlížeč hello má přednost před hello mobilní šablony (*. Mobile.cshtml).</span><span class="sxs-lookup"><span data-stu-id="4fed6-206">When adding mobile browser-specific display modes, such as for iPhone and Android, be sure tooset hello first argument too`0` (insert at hello top of hello list) toomake sure that hello browser-specific mode takes precedence over hello mobile template (*.Mobile.cshtml).</span></span> <span data-ttu-id="4fed6-207">Pokud mobilní šablony hello hello seznamu hello první místo, bude vybrána přes vaše režim určený zobrazení (hello první shodu wins a mobilní šablona hello odpovídá všechny mobilní prohlížeče).</span><span class="sxs-lookup"><span data-stu-id="4fed6-207">If hello mobile template is at hello top of hello list instead, it will be selected over your intended display mode (hello first match wins, and hello mobile template matches all mobile browsers).</span></span> 
> 
> 

<span data-ttu-id="4fed6-208">V kódu hello, klikněte pravým tlačítkem na `DefaultDisplayMode`, zvolte **vyřešit**a potom vyberte `using System.Web.WebPages;`.</span><span class="sxs-lookup"><span data-stu-id="4fed6-208">In hello code, right-click `DefaultDisplayMode`, choose **Resolve**, and then choose `using System.Web.WebPages;`.</span></span> <span data-ttu-id="4fed6-209">Tento postup přidá odkaz toothe `System.Web.WebPages` názvů, který je tam, kde `DisplayModeProvider` a `DefaultDisplayMode` typy jsou definované.</span><span class="sxs-lookup"><span data-stu-id="4fed6-209">This adds a reference toothe `System.Web.WebPages` namespace, which is where the `DisplayModeProvider` and `DefaultDisplayMode` types are defined.</span></span>

![][ResolveDefaultDisplayMode]

<span data-ttu-id="4fed6-210">Alternativně můžete právě ručně přidat hello následující řádek toothe `using` hello souboru.</span><span class="sxs-lookup"><span data-stu-id="4fed6-210">Alternatively, you can just manually add hello following line toothe `using` section of hello file.</span></span>

    using System.Web.WebPages;

<span data-ttu-id="4fed6-211">Uložte změny hello.</span><span class="sxs-lookup"><span data-stu-id="4fed6-211">Save hello changes.</span></span> <span data-ttu-id="4fed6-212">Kopírování *zobrazení\\sdílené\\\_Layout.Mobile.cshtml* do souboru *zobrazení\\sdílené\\\_Layout.iPhone.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="4fed6-212">Copy the *Views\\Shared\\\_Layout.Mobile.cshtml* file to *Views\\Shared\\\_Layout.iPhone.cshtml*.</span></span> <span data-ttu-id="4fed6-213">Otevřete nový soubor hello a potom změňte název hello z `MVC5 Application (Mobile)` k `MVC5 Application (iPhone)`.</span><span class="sxs-lookup"><span data-stu-id="4fed6-213">Open hello new file and then change hello title from `MVC5 Application (Mobile)` to `MVC5 Application (iPhone)`.</span></span>

<span data-ttu-id="4fed6-214">Kopírování hello *zobrazení\\Domů\\AllTags.Mobile.cshtml* do souboru *zobrazení\\Domů\\AllTags.iPhone.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="4fed6-214">Copy hello *Views\\Home\\AllTags.Mobile.cshtml* file to *Views\\Home\\AllTags.iPhone.cshtml*.</span></span> <span data-ttu-id="4fed6-215">V novém souboru hello změnit hello `<h2>` element z "značky (M)" příliš "značky (iPhone)".</span><span class="sxs-lookup"><span data-stu-id="4fed6-215">In hello new file, change hello `<h2>` element from "Tags (M)" too"Tags (iPhone)".</span></span>

<span data-ttu-id="4fed6-216">Spusťte aplikaci hello.</span><span class="sxs-lookup"><span data-stu-id="4fed6-216">Run hello application.</span></span> <span data-ttu-id="4fed6-217">Spustit prohlížeč pro mobilní zařízení emulátoru, ujistěte se, jeho uživatelský agent je nastaven příliš "iPhone" a vyhledejte toohello *AllTags* zobrazení.</span><span class="sxs-lookup"><span data-stu-id="4fed6-217">Run a mobile browser emulator, make sure its user agent is set too"iPhone", and browse toohello *AllTags* view.</span></span> <span data-ttu-id="4fed6-218">Pokud používáte emulátor hello v Internet Exploreru 11 F12 nástroje pro vývojáře, konfigurace emulace toohello následující:</span><span class="sxs-lookup"><span data-stu-id="4fed6-218">If you are using hello emulator in Internet Explorer 11 F12 developer tools, configure emulation toohello following:</span></span>

* <span data-ttu-id="4fed6-219">Profil Browser = **Windows Phone**</span><span class="sxs-lookup"><span data-stu-id="4fed6-219">Browser profile = **Windows Phone**</span></span>
* <span data-ttu-id="4fed6-220">Řetězec uživatelského agenta = **vlastní**</span><span class="sxs-lookup"><span data-stu-id="4fed6-220">User agent string =  **Custom**</span></span>
* <span data-ttu-id="4fed6-221">Vlastní řetězec = **Apple-iPhone5C1/1001.525**</span><span class="sxs-lookup"><span data-stu-id="4fed6-221">Custom string = **Apple-iPhone5C1/1001.525**</span></span>

<span data-ttu-id="4fed6-222">Hello následující snímek obrazovky ukazuje hello *AllTags* zobrazení vykreslen v emulátoru v Internet Exploreru 11 F12 nástroje pro vývojáře řetězcem hello vlastního uživatelského agenta (jedná se řetězec iPhone 5 C uživatelského agenta).</span><span class="sxs-lookup"><span data-stu-id="4fed6-222">hello following screenshot shows hello *AllTags* view rendered in the emulator in Internet Explorer 11 F12 developer tools with hello custom user agent string (this is an iPhone 5C user agent string).</span></span>

![][AllTagsIPhone_LayoutIPhone]

<span data-ttu-id="4fed6-223">V hello prohlížeč pro mobilní zařízení, vyberte hello **Řečníci** odkaz.</span><span class="sxs-lookup"><span data-stu-id="4fed6-223">In hello mobile browser, select hello **Speakers** link.</span></span> <span data-ttu-id="4fed6-224">Protože mobilní zobrazení není (*AllSpeakers.Mobile.cshtml*), zobrazit výchozí mluvčí hello (*AllSpeakers.cshtml*) je vykreslen pomocí zobrazení mobilní rozložení hello ( *\_ Layout.Mobile.cshtml*).</span><span class="sxs-lookup"><span data-stu-id="4fed6-224">Because there's not a mobile view (*AllSpeakers.Mobile.cshtml*), hello default speakers view (*AllSpeakers.cshtml*) is rendered using hello mobile layout view (*\_Layout.Mobile.cshtml*).</span></span> <span data-ttu-id="4fed6-225">Jak je uvedeno níže, název hello **MVC5 aplikace (mobilní)** je definována v  *\_Layout.Mobile.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="4fed6-225">As shown below, hello title **MVC5 Application (Mobile)** is defined in *\_Layout.Mobile.cshtml*.</span></span>

![][AllSpeakers_LayoutMobile]

<span data-ttu-id="4fed6-226">Výchozí zobrazení (jiných než mobilních) z vykreslování uvnitř mobilní rozložení můžete zakázat globálně nastavením `RequireConsistentDisplayMode` k `true` v hello *zobrazení\\\_ViewStart.cshtml* souboru, například takto:</span><span class="sxs-lookup"><span data-stu-id="4fed6-226">You can globally disable a default (non-mobile) view from rendering inside a mobile layout by setting `RequireConsistentDisplayMode` to `true` in hello *Views\\\_ViewStart.cshtml* file, like this:</span></span>

    @{
        Layout = "~/Views/Shared/_Layout.cshtml";
        DisplayModeProvider.Instance.RequireConsistentDisplayMode = true;
    }

<span data-ttu-id="4fed6-227">Když `RequireConsistentDisplayMode` je nastaven příliš`true`, mobilní rozložení hello (*\_Layout.Mobile.cshtml*) se používá pouze pro mobilní zobrazení (tj. Po zobrazení souboru ve formátu hello  ***ViewName** . Mobile.cshtml*).</span><span class="sxs-lookup"><span data-stu-id="4fed6-227">When `RequireConsistentDisplayMode` is set too`true`, hello mobile layout (*\_Layout.Mobile.cshtml*) is used only for mobile views (i.e. when the view file is of hello form ***ViewName**.Mobile.cshtml*).</span></span> <span data-ttu-id="4fed6-228">Můžete chtít tooset `RequireConsistentDisplayMode` příliš`true` Pokud vaše mobilní rozložení nebude fungovat dobře u jiných než mobilních zobrazení.</span><span class="sxs-lookup"><span data-stu-id="4fed6-228">You might want tooset `RequireConsistentDisplayMode` too`true` if your mobile layout doesn't work well with your non-mobile views.</span></span> <span data-ttu-id="4fed6-229">Hello – snímek obrazovky níže znázorňuje způsob hello *Řečníci* stránka vykresluje při `RequireConsistentDisplayMode` je nastaven příliš`true` (bez hello řetězec "(mobilní)" hello navigační panel v horní části hello).</span><span class="sxs-lookup"><span data-stu-id="4fed6-229">hello screenshot below shows how hello *Speakers* page renders when `RequireConsistentDisplayMode` is set too`true` (without hello string "(Mobile)" in hello navigational bar at hello top).</span></span>

![][AllSpeakers_LayoutMobileOverridden]

<span data-ttu-id="4fed6-230">Konzistentní režim zobrazení v určitém zobrazení můžete zakázat nastavením `RequireConsistentDisplayMode` příliš`false` v souboru zobrazení hello.</span><span class="sxs-lookup"><span data-stu-id="4fed6-230">You can disable consistent display mode in a specific view by setting `RequireConsistentDisplayMode` too`false` in hello view file.</span></span> <span data-ttu-id="4fed6-231">Následující kód v hello *zobrazení\\Domů\\AllSpeakers.cshtml* souboru nastaví `RequireConsistentDisplayMode` příliš`false`:</span><span class="sxs-lookup"><span data-stu-id="4fed6-231">The following markup in hello *Views\\Home\\AllSpeakers.cshtml* file sets `RequireConsistentDisplayMode` too`false`:</span></span>

    @model IEnumerable<string>

    @{
        ViewBag.Title = "All speakers";
        DisplayModeProvider.Instance.RequireConsistentDisplayMode = false;
    }

<span data-ttu-id="4fed6-232">V této části jsme viděli jak toocreate mobilní rozložení a zobrazení a jak toocreate rozložení a zobrazení pro konkrétní zařízení, jako hello iPhone.</span><span class="sxs-lookup"><span data-stu-id="4fed6-232">In this section we've seen how toocreate mobile layouts and views and how toocreate layouts and views for specific devices such as hello iPhone.</span></span>
<span data-ttu-id="4fed6-233">Hlavní výhodou hello Bootstrap CSS framework hello je však přizpůsobivé rozložení, což znamená, že jeden šablony stylů mohou být použity u plochy, phone a tablet prohlížeče toocreate konzistentní vzhled a chování.</span><span class="sxs-lookup"><span data-stu-id="4fed6-233">However, hello main advantage of hello Bootstrap CSS framework is the responsive layout, which means that a single stylesheet can be applied across desktop, phone, and tablet browsers toocreate a consistent look and feel.</span></span> <span data-ttu-id="4fed6-234">V další části hello uvidíte, jak tooleverage Bootstrap mobilní zařízení toocreate zobrazení.</span><span class="sxs-lookup"><span data-stu-id="4fed6-234">In hello next section you'll see how tooleverage Bootstrap toocreate mobile-friendly views.</span></span>

## <span data-ttu-id="4fed6-235"><a name="bkmk_Improvespeakerslist"></a>Zlepšení hello mluvčí seznamu</span><span class="sxs-lookup"><span data-stu-id="4fed6-235"><a name="bkmk_Improvespeakerslist"></a> Improve hello Speakers List</span></span>
<span data-ttu-id="4fed6-236">Protože jste právě viděli, hello *Řečníci* zobrazení je čitelná, ale hello odkazy jsou malé a jsou těžko tootap na mobilním zařízení.</span><span class="sxs-lookup"><span data-stu-id="4fed6-236">As you just saw, hello *Speakers* view is readable, but hello links are small and are difficult tootap on a mobile device.</span></span> <span data-ttu-id="4fed6-237">V této části, budete mít hello *AllSpeakers* zobrazení mobilní zařízení, která zobrazuje velké, snadno klepněte odkazy a obsahuje tooquickly pole hledání najít mluvčí.</span><span class="sxs-lookup"><span data-stu-id="4fed6-237">In this section, you'll make hello *AllSpeakers* view mobile-friendly, which displays large, easy-to-tap links and contains a search box tooquickly find speakers.</span></span>

<span data-ttu-id="4fed6-238">Můžete použít hello Bootstrap [skupiny odkazovaného seznamu] [ linked list group] stylů ke zlepšení hello *Řečníci* zobrazení.</span><span class="sxs-lookup"><span data-stu-id="4fed6-238">You can use hello Bootstrap [linked list group][linked list group] styling to improve hello *Speakers* view.</span></span> <span data-ttu-id="4fed6-239">V *zobrazení\\Domů\\AllSpeakers.cshtml*, hello obsah souboru nástroje Razor hello nahraďte hello kód níže.</span><span class="sxs-lookup"><span data-stu-id="4fed6-239">In *Views\\Home\\AllSpeakers.cshtml*, replace hello contents of hello Razor file with hello code below.</span></span>

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

<span data-ttu-id="4fed6-240">Hello `class="list-group"` atribut v hello `<div>` značky platí Bootstrap seznamu stylů a hello `class="input-group-item"` atribut platí Bootstrap seznamu položky stylů tooeach odkaz.</span><span class="sxs-lookup"><span data-stu-id="4fed6-240">hello `class="list-group"` attribute in hello `<div>` tag applies the Bootstrap list styling, and hello `class="input-group-item"` attribute applies Bootstrap list item styling tooeach link.</span></span>

<span data-ttu-id="4fed6-241">Aktualizujte hello mobilní prohlížeče.</span><span class="sxs-lookup"><span data-stu-id="4fed6-241">Refresh hello mobile browser.</span></span> <span data-ttu-id="4fed6-242">Hello aktualizovat zobrazení vypadá takto:</span><span class="sxs-lookup"><span data-stu-id="4fed6-242">hello updated view looks like this:</span></span>

![][AllSpeakersFixed]

<span data-ttu-id="4fed6-243">Hello Bootstrap [skupiny odkazovaného seznamu] [ linked list group] stylů díky hello celé pole pro každý odkaz můžete kliknout, což je mnohem lepší prostředí.</span><span class="sxs-lookup"><span data-stu-id="4fed6-243">hello Bootstrap [linked list group][linked list group] styling makes hello entire box for each link clickable, which is a much better user experience.</span></span> <span data-ttu-id="4fed6-244">Přepněte zobrazení plochy toothe a sledovat hello konzistentní vzhled a chování.</span><span class="sxs-lookup"><span data-stu-id="4fed6-244">Switch toothe desktop view and observe hello consistent look and feel.</span></span>

![][AllSpeakersFixedDesktop]

<span data-ttu-id="4fed6-245">I když se zlepšila zobrazení hello prohlížeč pro mobilní zařízení, je obtížné přejděte hello dlouhý seznam mluvčí.</span><span class="sxs-lookup"><span data-stu-id="4fed6-245">Although hello mobile browser view has improved, it's difficult to navigate hello long list of speakers.</span></span> <span data-ttu-id="4fed6-246">Bootstrap neposkytuje hledání filtru funkce out-of-the-box, ale můžete jej přidat pár řádků kódu.</span><span class="sxs-lookup"><span data-stu-id="4fed6-246">Bootstrap doesn't provide a search filter functionality out-of-the-box, but you can add it with a few lines of code.</span></span> <span data-ttu-id="4fed6-247">Nejprve bude přidejte zobrazení toohello pole hledání a pak propojte s hello kódu jazyka JavaScript pro funkci filtru hello.</span><span class="sxs-lookup"><span data-stu-id="4fed6-247">You will first add a search box toohello view, then hook up with hello JavaScript code for hello filter function.</span></span> <span data-ttu-id="4fed6-248">V *zobrazení\\Domů\\AllSpeakers.cshtml*, přidejte \<formuláře\> značky bezprostředně za hello \<h2\> značka, jak je uvedeno níže:</span><span class="sxs-lookup"><span data-stu-id="4fed6-248">In *Views\\Home\\AllSpeakers.cshtml*, add a \<form\> tag just after hello \<h2\> tag, as shown below:</span></span>

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

<span data-ttu-id="4fed6-249">Všimněte si, že hello `<form>` a `<input>` značky oba mají toothem Bootstrap styly použít hello.</span><span class="sxs-lookup"><span data-stu-id="4fed6-249">Notice that hello `<form>` and `<input>` tags both have hello Bootstrap styles applied toothem.</span></span> <span data-ttu-id="4fed6-250">Hello `<span>` element přidá Bootstrap [glyphicon] [ glyphicon] toothe vyhledávacího pole.</span><span class="sxs-lookup"><span data-stu-id="4fed6-250">hello `<span>` element adds a Bootstrap [glyphicon][glyphicon] toothe search box.</span></span>

<span data-ttu-id="4fed6-251">V hello *skripty* složky, přidejte do souboru JavaScript s názvem *filter.js*.</span><span class="sxs-lookup"><span data-stu-id="4fed6-251">In hello *Scripts* folder, add a JavaScript file called *filter.js*.</span></span> <span data-ttu-id="4fed6-252">Otevřete soubor hello a vložte následující kód do ní hello:</span><span class="sxs-lookup"><span data-stu-id="4fed6-252">Open hello file and paste hello following code into it:</span></span>

    $(function () {

        // reset hello search form when hello page loads
        $("form").each(function () {
            this.reset();
        });

        // wire up hello events toohello <input> element for search/filter
        $("input").bind("keyup change", function () {
            var searchtxt = this.value.toLowerCase();
            var items = $(".list-group-item");

            // show all speakers that begin with hello typed text and hide others
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

<span data-ttu-id="4fed6-253">Budete také potřebovat tooinclude filter.js v registrovaných sad.</span><span class="sxs-lookup"><span data-stu-id="4fed6-253">You also need tooinclude filter.js in your registered bundles.</span></span> <span data-ttu-id="4fed6-254">Otevřete *aplikace\_spustit\\BundleConfig.cs* a změňte hello první sady.</span><span class="sxs-lookup"><span data-stu-id="4fed6-254">Open *App\_Start\\BundleConfig.cs* and change hello first bundles.</span></span> <span data-ttu-id="4fed6-255">Změnit první `bundles.Add` – příkaz (pro hello **jquery** sady) tooinclude *skripty\\filter.js*, a to takto:</span><span class="sxs-lookup"><span data-stu-id="4fed6-255">Change the first `bundles.Add` statement (for hello **jquery** bundle) tooinclude *Scripts\\filter.js*, as follows:</span></span>

     bundles.Add(new ScriptBundle("~/bundles/jquery").Include(
                "~/Scripts/jquery-{version}.js",
                "~/Scripts/filter.js"));

<span data-ttu-id="4fed6-256">Hello **jquery** sady vykreslením již ve výchozím nastavení hello  *\_rozložení* zobrazení.</span><span class="sxs-lookup"><span data-stu-id="4fed6-256">hello **jquery** bundle is already rendered by hello default *\_Layout* view.</span></span> <span data-ttu-id="4fed6-257">Později, můžete využít hello stejné JavaScript code tooapply zobrazení seznamu tooother funkce filtru.</span><span class="sxs-lookup"><span data-stu-id="4fed6-257">Later, you can utilize hello same JavaScript code tooapply the filter functionality tooother list views.</span></span>

<span data-ttu-id="4fed6-258">Aktualizujte hello mobilní prohlížeče a přejděte toohello *AllSpeakers* zobrazení.</span><span class="sxs-lookup"><span data-stu-id="4fed6-258">Refresh hello mobile browser and go toohello *AllSpeakers* view.</span></span> <span data-ttu-id="4fed6-259">Do vyhledávacího pole zadejte "sc".</span><span class="sxs-lookup"><span data-stu-id="4fed6-259">In the search box, type "sc".</span></span> <span data-ttu-id="4fed6-260">seznam mluvčí Hello by měl být nyní filtrovaný podle tooyour hledaný řetězec.</span><span class="sxs-lookup"><span data-stu-id="4fed6-260">hello speakers list should now be filtered according tooyour search string.</span></span>

![][AllSpeakersFixedSearchBySC]

## <span data-ttu-id="4fed6-261"><a name="bkmk_improvetags"></a>Zlepšení hello seznam značek</span><span class="sxs-lookup"><span data-stu-id="4fed6-261"><a name="bkmk_improvetags"></a> Improve hello Tags List</span></span>
<span data-ttu-id="4fed6-262">Jako hello *Řečníci* zobrazit, hello *značky* zobrazení je čitelná, ale hello odkazy jsou malé a obtížně tootap na mobilním zařízení.</span><span class="sxs-lookup"><span data-stu-id="4fed6-262">Like hello *Speakers* view, hello *Tags* view is readable, but hello links are small and difficult tootap on a mobile device.</span></span> <span data-ttu-id="4fed6-263">Můžete je vyřešit hello *značky* zobrazení hello stejný způsob, jak opravit hello *Řečníci* zobrazit, je-li použít změny kódu hello popsané výše, ale s následující hello `Html.ActionLink` syntaxe využívající metody v  *Zobrazení\\Domů\\AllTags.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="4fed6-263">You can fix hello *Tags* view hello same way you fix hello *Speakers* view, if you use hello code changes described earlier, but with hello following `Html.ActionLink` method syntax in *Views\\Home\\AllTags.cshtml*:</span></span>

    @Html.ActionLink(tag, 
                     "SessionsByTag", 
                     new { tag }, 
                     new { @class = "list-group-item" })

<span data-ttu-id="4fed6-264">Hello aktualizovat prohlížeč pro stolní počítač vypadá takto:</span><span class="sxs-lookup"><span data-stu-id="4fed6-264">hello refreshed desktop browser looks as follows:</span></span>

![][AllTagsFixedDesktop]

<span data-ttu-id="4fed6-265">A hello aktualizují prohlížeč pro mobilní zařízení vypadá takto:</span><span class="sxs-lookup"><span data-stu-id="4fed6-265">And hello refreshed mobile browser looks as follows:</span></span> 

![][AllTagsFixed]

> [!NOTE]
> <span data-ttu-id="4fed6-266">Pokud si všimnete, že hello původní formátování seznamu je stále existuje v hello prohlížeč pro mobilní zařízení a zajímat, co se stalo tooyour dobrý Bootstrap stylů, to je artefakt starší akce toocreate mobilní konkrétní zobrazení.</span><span class="sxs-lookup"><span data-stu-id="4fed6-266">If you notice that hello original list formatting is still there in hello mobile browser and wonder what happened tooyour nice Bootstrap styling, this is an artifact of your earlier action toocreate mobile specific views.</span></span> <span data-ttu-id="4fed6-267">Teď, když používáte hello Bootstrap CSS framework toocreate návrhu přizpůsobivý webu, přejděte head a odebrat tyto specifické mobilní zobrazení a zobrazení konkrétní mobilní rozložení hello.</span><span class="sxs-lookup"><span data-stu-id="4fed6-267">However, now that you are using hello Bootstrap CSS framework toocreate a responsive web design, go head and remove these mobile-specific views and hello mobile-specific layout views.</span></span> <span data-ttu-id="4fed6-268">Po dokončení tak hello aktualizovat prohlížeč pro mobilní zařízení se zobrazí hello Bootstrap stylů.</span><span class="sxs-lookup"><span data-stu-id="4fed6-268">Once you have done so, hello refreshed mobile browser will show hello Bootstrap styling.</span></span>
> 
> 

## <span data-ttu-id="4fed6-269"><a name="bkmk_improvedates"></a>Zlepšení hello seznamu kalendářních dat</span><span class="sxs-lookup"><span data-stu-id="4fed6-269"><a name="bkmk_improvedates"></a> Improve hello Dates List</span></span>
<span data-ttu-id="4fed6-270">Můžete zvýšit hello *data* zobrazit jako vylepšené hello *Řečníci* a *značky* zobrazení, pokud používáte hello změn kódu popsané výše, ale s hello následující `Html.ActionLink` syntaxe využívající metody v *zobrazení\\Domů\\AllDates.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="4fed6-270">You can improve hello *Dates* view like you improved hello *Speakers* and *Tags* views if you use hello code changes described earlier, but with hello following `Html.ActionLink` method syntax in *Views\\Home\\AllDates.cshtml*:</span></span>

    @Html.ActionLink(date.ToString("ddd, MMM dd, h:mm tt"), 
                     "SessionsByDate", 
                     new { date }, 
                     new { @class = "list-group-item" })

<span data-ttu-id="4fed6-271">Zobrazí se zobrazení aktualizovat prohlížeč pro mobilní zařízení takto:</span><span class="sxs-lookup"><span data-stu-id="4fed6-271">You will get a refreshed mobile browser view like this:</span></span>

![][AllDatesFixed]

<span data-ttu-id="4fed6-272">Lze dále zvýšit hello *data* zobrazení uspořádání hello hodnoty data a času podle data.</span><span class="sxs-lookup"><span data-stu-id="4fed6-272">You can further improve hello *Dates* view by organizing hello date-time values by date.</span></span> <span data-ttu-id="4fed6-273">To lze provést pomocí hello Bootstrap [panelů] [ panels] styly.</span><span class="sxs-lookup"><span data-stu-id="4fed6-273">This can be done with hello Bootstrap [panels][panels] styling.</span></span> <span data-ttu-id="4fed6-274">Nahraďte obsah hello hello *zobrazení\\Domů\\AllDates.cshtml* soubor s následujícím kódem:</span><span class="sxs-lookup"><span data-stu-id="4fed6-274">Replace hello contents of hello *Views\\Home\\AllDates.cshtml* file with the following code:</span></span>

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

<span data-ttu-id="4fed6-275">Tento kód vytvoří samostatné `<div class="panel panel-primary">` značky pro každý odlišné datum v seznamu hello a hello používá [skupiny odkazovaného seznamu] [ linked list group] pro příslušné odkazy jako předtím.</span><span class="sxs-lookup"><span data-stu-id="4fed6-275">This code creates a separate `<div class="panel panel-primary">` tag for each distinct date in hello list, and uses hello [linked list group][linked list group] for the respective links as before.</span></span> <span data-ttu-id="4fed6-276">Tady je co hello mobilní prohlížeče vypadá jako při spuštění tohoto kódu:</span><span class="sxs-lookup"><span data-stu-id="4fed6-276">Here's what hello mobile browser looks like when this code runs:</span></span>

![][AllDatesFixed2]

<span data-ttu-id="4fed6-277">Přepínač toohello prohlížeč pro stolní počítač.</span><span class="sxs-lookup"><span data-stu-id="4fed6-277">Switch toohello desktop browser.</span></span> <span data-ttu-id="4fed6-278">Znovu si všimněte konzistentní vzhled hello.</span><span class="sxs-lookup"><span data-stu-id="4fed6-278">Again, note hello consistent look.</span></span>

![][AllDatesFixed2Desktop]

## <span data-ttu-id="4fed6-279"><a name="bkmk_improvesessionstable"></a>Zlepšení hello SessionsTable zobrazení</span><span class="sxs-lookup"><span data-stu-id="4fed6-279"><a name="bkmk_improvesessionstable"></a> Improve hello SessionsTable View</span></span>
<span data-ttu-id="4fed6-280">V této části, budete mít hello *SessionsTable* zobrazit další mobilní zařízení.</span><span class="sxs-lookup"><span data-stu-id="4fed6-280">In this section, you'll make hello *SessionsTable* view more mobile-friendly.</span></span> <span data-ttu-id="4fed6-281">Tuto změnu je rozsáhlejší hello předchozí změny.</span><span class="sxs-lookup"><span data-stu-id="4fed6-281">This change is more extensive hello previous changes.</span></span>

<span data-ttu-id="4fed6-282">V hello prohlížeč pro mobilní zařízení, klepněte na hello **značka** tlačítko a potom zadejte `asp` do vyhledávacího pole.</span><span class="sxs-lookup"><span data-stu-id="4fed6-282">In hello mobile browser, tap hello **Tag** button, then enter `asp` in the search box.</span></span>

![][AllTagsFixedSearchByASP]

<span data-ttu-id="4fed6-283">Klepněte na hello **ASP.NET** odkaz.</span><span class="sxs-lookup"><span data-stu-id="4fed6-283">Tap hello **ASP.NET** link.</span></span>

![][SessionsTableTagASP.NET]

<span data-ttu-id="4fed6-284">Jak vidíte, zobrazení hello je naformátován jako tabulku, která je aktuálně navrženou toobe zobrazit v prohlížeči počítače hello.</span><span class="sxs-lookup"><span data-stu-id="4fed6-284">As you can see, hello display is formatted as a table, which is currently designed toobe viewed in hello desktop browser.</span></span> <span data-ttu-id="4fed6-285">Je však chvíli obtížné tooread na prohlížeč pro mobilní zařízení.</span><span class="sxs-lookup"><span data-stu-id="4fed6-285">However, it's a little bit difficult tooread on a mobile browser.</span></span> <span data-ttu-id="4fed6-286">toofix tuto, otevřete *zobrazení\\Domů\\SessionsTable.cshtml* a hello obsah souboru nahraďte hello následující kód:</span><span class="sxs-lookup"><span data-stu-id="4fed6-286">toofix this, open *Views\\Home\\SessionsTable.cshtml* and then replace hello contents of the file with hello following code:</span></span>

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

<span data-ttu-id="4fed6-287">Kód Hello provádí 3 věci:</span><span class="sxs-lookup"><span data-stu-id="4fed6-287">hello code does 3 things:</span></span>

* <span data-ttu-id="4fed6-288">používá hello Bootstrap [skupiny vlastní odkazovaného seznamu] [ custom linked list group] tooformat hello informací o relaci ve svislém směru, aby tyto informace je čitelná na prohlížeč pro mobilní zařízení (pomocí třídy, jako seznam skupiny--text položky)</span><span class="sxs-lookup"><span data-stu-id="4fed6-288">uses hello Bootstrap [custom linked list group][custom linked list group] tooformat hello session information vertically, so that all this information is readable on a mobile browser (using classes such as list-group-item-text)</span></span>
* <span data-ttu-id="4fed6-289">platí hello [mřížky systému] [ grid system] toothe rozložení, takže této relaci hello položky tok v prohlížeč pro stolní počítač hello vodorovně a svisle ve hello prohlížeč pro mobilní zařízení (pomocí třídy hello sloupec md-4)</span><span class="sxs-lookup"><span data-stu-id="4fed6-289">applies hello [grid system][grid system] toothe layout, so that hello session items flow horizontally in hello desktop browser and vertically in hello mobile browser (using hello col-md-4 class)</span></span>
* <span data-ttu-id="4fed6-290">hello používá [přizpůsobivý nástroje] [ responsive utilities] ke skrytí hello relace značky v hello prohlížeč pro mobilní zařízení (pomocí třídy skryté xs hello)</span><span class="sxs-lookup"><span data-stu-id="4fed6-290">uses hello [responsive utilities][responsive utilities] to hide hello session tags when viewed in hello mobile browser (using hello hidden-xs class)</span></span>

<span data-ttu-id="4fed6-291">Klepnutím na název odkazu toogo toohello příslušné relace.</span><span class="sxs-lookup"><span data-stu-id="4fed6-291">You can also tap a title link toogo toohello respective session.</span></span> <span data-ttu-id="4fed6-292">Následující obrázek Hello odráží změny kódu hello.</span><span class="sxs-lookup"><span data-stu-id="4fed6-292">hello image below reflects hello code changes.</span></span>

![][FixedSessionsByTag]

<span data-ttu-id="4fed6-293">Hello Bootstrap mřížky systém, který můžete použít automaticky uspořádá relací svisle v hello prohlížeč pro mobilní zařízení.</span><span class="sxs-lookup"><span data-stu-id="4fed6-293">hello Bootstrap grid system that you applied automatically arranges the sessions vertically in hello mobile browser.</span></span> <span data-ttu-id="4fed6-294">Všimněte si také, že nejsou zobrazeny hello značky.</span><span class="sxs-lookup"><span data-stu-id="4fed6-294">Also, notice that hello tags are not shown.</span></span> <span data-ttu-id="4fed6-295">Přepínač toohello prohlížeč pro stolní počítač.</span><span class="sxs-lookup"><span data-stu-id="4fed6-295">Switch toohello desktop browser.</span></span>

![][SessionsTableFixedTagASP.NETDesktop]

<span data-ttu-id="4fed6-296">V hello prohlížeč pro stolní počítač Všimněte si, že hello značky se teď zobrazují.</span><span class="sxs-lookup"><span data-stu-id="4fed6-296">In hello desktop browser, notice that hello tags are now displayed.</span></span> <span data-ttu-id="4fed6-297">Zkontrolujte také, že hello Bootstrap mřížky systému, který jste použili uspořádá hello relace položky v dva sloupce.</span><span class="sxs-lookup"><span data-stu-id="4fed6-297">Also, you can see that hello Bootstrap grid system you applied arranges hello session items in two columns.</span></span> <span data-ttu-id="4fed6-298">V případě zvětšení v prohlížeči, zobrazí se, že změní uspořádání hello toothree sloupce.</span><span class="sxs-lookup"><span data-stu-id="4fed6-298">If you enlarge the browser, you will see that hello arrangement changes toothree columns.</span></span>

## <span data-ttu-id="4fed6-299"><a name="bkmk_improvesessionbycode"></a>Zlepšení hello SessionByCode zobrazení</span><span class="sxs-lookup"><span data-stu-id="4fed6-299"><a name="bkmk_improvesessionbycode"></a> Improve hello SessionByCode View</span></span>
<span data-ttu-id="4fed6-300">Nakonec budete opravte hello *SessionByCode* zobrazení toomake je mobilní zařízení.</span><span class="sxs-lookup"><span data-stu-id="4fed6-300">Finally, you'll fix hello *SessionByCode* view toomake it mobile-friendly.</span></span>

<span data-ttu-id="4fed6-301">V hello prohlížeč pro mobilní zařízení, klepněte na hello **značka** tlačítko a potom zadejte `asp` do vyhledávacího pole.</span><span class="sxs-lookup"><span data-stu-id="4fed6-301">In hello mobile browser, tap hello **Tag** button, then enter `asp` in the search box.</span></span>

![][AllTagsFixedSearchByASP]

<span data-ttu-id="4fed6-302">Klepněte na hello **ASP.NET** odkaz.</span><span class="sxs-lookup"><span data-stu-id="4fed6-302">Tap hello **ASP.NET** link.</span></span> <span data-ttu-id="4fed6-303">Zobrazí se relace značky ASP.NET hello.</span><span class="sxs-lookup"><span data-stu-id="4fed6-303">Sessions for hello ASP.NET tag are displayed.</span></span>

![][FixedSessionsByTag]

<span data-ttu-id="4fed6-304">Zvolte hello **vytváření jedné stránky aplikace s ASP.NET a AngularJS** odkaz.</span><span class="sxs-lookup"><span data-stu-id="4fed6-304">Choose hello **Building a Single Page Application with ASP.NET and AngularJS** link.</span></span>

![][SessionByCode3-644]

<span data-ttu-id="4fed6-305">zobrazení plochy výchozí Hello je v pořádku, ale můžete zlepšit vzhled hello snadno pomocí některé součásti Bootstrap grafickým uživatelským rozhraním.</span><span class="sxs-lookup"><span data-stu-id="4fed6-305">hello default desktop view is fine, but you can improve hello look easily by using some Bootstrap GUI components.</span></span>

<span data-ttu-id="4fed6-306">Otevřete *zobrazení\\Domů\\SessionByCode.cshtml* a nahraďte obsah hello hello následující kód:</span><span class="sxs-lookup"><span data-stu-id="4fed6-306">Open *Views\\Home\\SessionByCode.cshtml* and replace hello contents with hello following markup:</span></span>

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

<span data-ttu-id="4fed6-307">nový kód Hello používá Bootstrap panelů styly tooimprove hello mobilní zobrazení.</span><span class="sxs-lookup"><span data-stu-id="4fed6-307">hello new markup uses Bootstrap panels styling tooimprove hello mobile view.</span></span> 

<span data-ttu-id="4fed6-308">Aktualizujte hello mobilní prohlížeče.</span><span class="sxs-lookup"><span data-stu-id="4fed6-308">Refresh hello mobile browser.</span></span> <span data-ttu-id="4fed6-309">Hello následující obrázek zobrazuje hello změn kódu, které jste právě vytvořili:</span><span class="sxs-lookup"><span data-stu-id="4fed6-309">hello following image reflects hello code changes that you just made:</span></span>

![][SessionByCodeFixed3-644]

## <a name="wrap-up-and-review"></a><span data-ttu-id="4fed6-310">Zabalení a zkontrolovat</span><span class="sxs-lookup"><span data-stu-id="4fed6-310">Wrap Up and Review</span></span>
<span data-ttu-id="4fed6-311">Tento kurz vám ukázal jak toouse ASP.NET MVC 5 toodevelop mobilní zařízení webových aplikací.</span><span class="sxs-lookup"><span data-stu-id="4fed6-311">This tutorial has shown you how toouse ASP.NET MVC 5 toodevelop mobile-friendly Web applications.</span></span> <span data-ttu-id="4fed6-312">Mezi ně patří:</span><span class="sxs-lookup"><span data-stu-id="4fed6-312">These include:</span></span>

* <span data-ttu-id="4fed6-313">Nasazení tooan aplikace ASP.NET MVC 5 webové aplikace App Service</span><span class="sxs-lookup"><span data-stu-id="4fed6-313">Deploy an ASP.NET MVC 5 application tooan App Service web app</span></span>
* <span data-ttu-id="4fed6-314">Použít rozložení Bootstrap toocreate přizpůsobivý webové stránky v aplikaci MVC 5</span><span class="sxs-lookup"><span data-stu-id="4fed6-314">Use Bootstrap toocreate responsive web layout in your MVC 5 application</span></span>
* <span data-ttu-id="4fed6-315">Přepsání částečné zobrazení, zobrazení a rozložení globálně i pro jednotlivé zobrazení</span><span class="sxs-lookup"><span data-stu-id="4fed6-315">Override layout, views, and partial views, both globally and for an individual view</span></span>
* <span data-ttu-id="4fed6-316">Rozložení ovládacích prvků a částečné přepsat pomocí vynucení `RequireConsistentDisplayMode` vlastnost</span><span class="sxs-lookup"><span data-stu-id="4fed6-316">Control layout and partial override enforcement using the `RequireConsistentDisplayMode` property</span></span>
* <span data-ttu-id="4fed6-317">Vytvoření zobrazení, které cílí určité prohlížeče, jako je třeba hello iPhone prohlížeč</span><span class="sxs-lookup"><span data-stu-id="4fed6-317">Create views that target specific browsers, such as hello iPhone browser</span></span>
* <span data-ttu-id="4fed6-318">Použít Bootstrap stylů v kódu Razor</span><span class="sxs-lookup"><span data-stu-id="4fed6-318">Apply Bootstrap styling in Razor code</span></span>

## <a name="see-also"></a><span data-ttu-id="4fed6-319">Viz také</span><span class="sxs-lookup"><span data-stu-id="4fed6-319">See Also</span></span>
* [<span data-ttu-id="4fed6-320">9 základní principy návrhu přizpůsobivý webu</span><span class="sxs-lookup"><span data-stu-id="4fed6-320">9 basic principles of responsive web design</span></span>](http://blog.froont.com/9-basic-principles-of-responsive-web-design/)
* <span data-ttu-id="4fed6-321">[Bootstrap][BootstrapSite]</span><span class="sxs-lookup"><span data-stu-id="4fed6-321">[Bootstrap][BootstrapSite]</span></span>
* <span data-ttu-id="4fed6-322">[Oficiální Blog zavedení][Official Bootstrap Blog]</span><span class="sxs-lookup"><span data-stu-id="4fed6-322">[Official Bootstrap Blog][Official Bootstrap Blog]</span></span>
* <span data-ttu-id="4fed6-323">[Twitter Bootstrap kurzu z kurzu republika][Twitter Bootstrap Tutorial from Tutorial Republic]</span><span class="sxs-lookup"><span data-stu-id="4fed6-323">[Twitter Bootstrap Tutorial from Tutorial Republic][Twitter Bootstrap Tutorial from Tutorial Republic]</span></span>
* <span data-ttu-id="4fed6-324">[Hello Bootstrap Playground][hello Bootstrap Playground]</span><span class="sxs-lookup"><span data-stu-id="4fed6-324">[hello Bootstrap Playground][hello Bootstrap Playground]</span></span>
* <span data-ttu-id="4fed6-325">[Osvědčené postupy W3C doporučení mobilní webové aplikace][W3C Recommendation Mobile Web Application Best Practices]</span><span class="sxs-lookup"><span data-stu-id="4fed6-325">[W3C Recommendation Mobile Web Application Best Practices][W3C Recommendation Mobile Web Application Best Practices]</span></span>
* <span data-ttu-id="4fed6-326">[W3C Candidate doporučení pro dotazy na média][W3C Candidate Recommendation for media queries]</span><span class="sxs-lookup"><span data-stu-id="4fed6-326">[W3C Candidate Recommendation for media queries][W3C Candidate Recommendation for media queries]</span></span>

## <a name="whats-changed"></a><span data-ttu-id="4fed6-327">Co se změnilo</span><span class="sxs-lookup"><span data-stu-id="4fed6-327">What's changed</span></span>
* <span data-ttu-id="4fed6-328">Průvodce toohello změnu z tooApp weby služby najdete v tématu: [Azure App Service a její vliv na stávající služby Azure](http://go.microsoft.com/fwlink/?LinkId=529714)</span><span class="sxs-lookup"><span data-stu-id="4fed6-328">For a guide toohello change from Websites tooApp Service see: [Azure App Service and Its Impact on Existing Azure Services](http://go.microsoft.com/fwlink/?LinkId=529714)</span></span>

<!-- Internal Links -->
[Deploy hello starter project tooan Azure web app]: #bkmk_DeployStarterProject
[Bootstrap CSS Framework]: #bkmk_bootstrap
[Override hello Views, Layouts, and Partial Views]: #bkmk_overrideviews
[Create Browser-Specific Views]:#bkmk_browserviews
[Improve hello Speakers List]: #bkmk_Improvespeakerslist
[Improve hello Tags List]: #bkmk_improvetags
[Improve hello Dates List]: #bkmk_improvedates
[Improve hello SessionsTable View]: #bkmk_improvesessionstable
[Improve hello SessionByCode View]: #bkmk_improvesessionbycode

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
[hello Bootstrap Playground]: http://www.bootply.com/
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

