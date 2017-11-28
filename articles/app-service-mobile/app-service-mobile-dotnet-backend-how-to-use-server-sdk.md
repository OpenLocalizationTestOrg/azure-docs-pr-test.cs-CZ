---
title: aaaHow toowork s hello .NET back-end serveru SDK pro Mobile Apps | Microsoft Docs
description: "Zjistěte, jak hello toowork s .NET back-end serveru SDK pro Azure App Service Mobile Apps."
keywords: "služby App service, služby azure app service, mobilní aplikace, mobilní služby, nasazení aplikací nasazení azure škálovatelné, aplikace stupnice"
services: app-service\mobile
documentationcenter: 
author: ggailey777
manager: syntaxc4
editor: 
ms.assetid: 0620554f-9590-40a8-9f47-61c48c21076b
ms.service: app-service-mobile
ms.workload: mobile
ms.tgt_pltfrm: mobile-multiple
ms.devlang: dotnet
ms.topic: article
ms.date: 10/01/2016
ms.author: glenga
ms.openlocfilehash: 2946c5ba4424565ac764e2ce5597bf42362fcedf
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="work-with-hello-net-backend-server-sdk-for-azure-mobile-apps"></a><span data-ttu-id="df946-104">Práce s hello .NET back-end serveru SDK pro Azure Mobile Apps</span><span class="sxs-lookup"><span data-stu-id="df946-104">Work with hello .NET backend server SDK for Azure Mobile Apps</span></span>
[!INCLUDE [app-service-mobile-selector-server-sdk](../../includes/app-service-mobile-selector-server-sdk.md)]

<span data-ttu-id="df946-105">Toto téma ukazuje, jak toouse hello .NET back-end serveru SDK v klíčové scénáře Azure App Service Mobile Apps.</span><span class="sxs-lookup"><span data-stu-id="df946-105">This topic shows you how toouse hello .NET backend server SDK in key Azure App Service Mobile Apps scenarios.</span></span> <span data-ttu-id="df946-106">Hello Azure Mobile Apps SDK umožňuje pracovat s mobilních klientů z vaší aplikace ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="df946-106">hello Azure Mobile Apps SDK helps you work with mobile clients from your ASP.NET application.</span></span>

> [!TIP]
> <span data-ttu-id="df946-107">Hello [server .NET SDK pro Azure Mobile Apps] [ 2] je open source na Githubu.</span><span class="sxs-lookup"><span data-stu-id="df946-107">hello [.NET server SDK for Azure Mobile Apps][2] is open source on GitHub.</span></span> <span data-ttu-id="df946-108">Hello úložiště obsahuje všechny zdrojový kód, včetně hello celý server SDK jednotky testovací sady a některé ukázkové projekty.</span><span class="sxs-lookup"><span data-stu-id="df946-108">hello repository contains all source code including hello entire server SDK unit test suite and some sample projects.</span></span>
>
>

## <a name="reference-documentation"></a><span data-ttu-id="df946-109">Referenční dokumentace</span><span class="sxs-lookup"><span data-stu-id="df946-109">Reference documentation</span></span>
<span data-ttu-id="df946-110">Hello referenční dokumentaci k nástroji pro hello server SDK se nachází zde: [Azure Mobile Apps .NET – referenční informace][1].</span><span class="sxs-lookup"><span data-stu-id="df946-110">hello reference documentation for hello server SDK is located here: [Azure Mobile Apps .NET Reference][1].</span></span>

## <span data-ttu-id="df946-111"><a name="create-app"></a>Postupy: vytvoření back-endu mobilní aplikace .NET</span><span class="sxs-lookup"><span data-stu-id="df946-111"><a name="create-app"></a>How to: Create a .NET Mobile App backend</span></span>
<span data-ttu-id="df946-112">Pokud spouštíte nový projekt, můžete vytvořit aplikaci služby App Service pomocí buď hello [portál Azure] nebo Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="df946-112">If you are starting a new project, you can create an App Service application using either hello [Azure portal] or Visual Studio.</span></span> <span data-ttu-id="df946-113">Můžete spustit místně hello aplikace služby App Service nebo publikování hello projektu tooyour cloudové služby App Service mobilní aplikace.</span><span class="sxs-lookup"><span data-stu-id="df946-113">You can run hello App Service application locally or publish hello project tooyour cloud-based App Service mobile app.</span></span>

<span data-ttu-id="df946-114">Chcete-li přidat existující projekt tooan mobilní funkce, najdete v části hello [stáhnout a inicializace hello SDK](#install-sdk) části.</span><span class="sxs-lookup"><span data-stu-id="df946-114">If you are adding mobile capabilities tooan existing project, see hello [Download and initialize hello SDK](#install-sdk) section.</span></span>

### <a name="create-a-net-backend-using-hello-azure-portal"></a><span data-ttu-id="df946-115">Vytvořit rozhraní .NET back-end pomocí hello portálu Azure</span><span class="sxs-lookup"><span data-stu-id="df946-115">Create a .NET backend using hello Azure portal</span></span>
<span data-ttu-id="df946-116">toocreate backendu mobilní služby App Service, buď použijte hello [rychlý úvodní kurz] [ 3] nebo postupujte podle těchto kroků:</span><span class="sxs-lookup"><span data-stu-id="df946-116">toocreate an App Service mobile backend, either follow hello [Quickstart tutorial][3] or follow these steps:</span></span>

[!INCLUDE [app-service-mobile-dotnet-backend-create-new-service-classic](../../includes/app-service-mobile-dotnet-backend-create-new-service-classic.md)]

<span data-ttu-id="df946-117">Zpět v hello *Začínáme* okno, v části **vytvořit tabulku rozhraní API**, zvolte **C#** jako vaše **back-end jazyk**.</span><span class="sxs-lookup"><span data-stu-id="df946-117">Back in hello *Get started* blade, under **Create a table API**, choose **C#** as your **Backend language**.</span></span> <span data-ttu-id="df946-118">Klikněte na tlačítko **Stáhnout**, extrahujte komprimované projektu soubory tooyour místního počítače a otevřete hello řešení v sadě Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="df946-118">Click **Download**, extract the compressed project files tooyour local computer, and open hello solution in Visual Studio.</span></span>

### <a name="create-a-net-backend-using-visual-studio-2013-and-visual-studio-2015"></a><span data-ttu-id="df946-119">Vytvořit rozhraní .NET back-end pomocí Visual Studio 2013 a Visual Studio 2015</span><span class="sxs-lookup"><span data-stu-id="df946-119">Create a .NET backend using Visual Studio 2013 and Visual Studio 2015</span></span>
<span data-ttu-id="df946-120">Nainstalujte hello [Azure SDK for .NET] [ 4] (verze 2.9.0 nebo novější) toocreate projektu Azure Mobile Apps v sadě Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="df946-120">Install hello [Azure SDK for .NET][4] (version 2.9.0 or later) toocreate an Azure Mobile Apps project in Visual Studio.</span></span> <span data-ttu-id="df946-121">Po instalaci hello SDK, vytvořte aplikaci ASP.NET pomocí hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="df946-121">Once you have installed hello SDK, create an ASP.NET application using hello following steps:</span></span>

1. <span data-ttu-id="df946-122">Otevřete hello **nový projekt** dialogové okno (z *soubor* > **nový** > **projekt...** ).</span><span class="sxs-lookup"><span data-stu-id="df946-122">Open hello **New Project** dialog (from *File* > **New** > **Project...**).</span></span>
2. <span data-ttu-id="df946-123">Rozbalte položku **šablony** > **Visual C#**a vyberte **webové**.</span><span class="sxs-lookup"><span data-stu-id="df946-123">Expand **Templates** > **Visual C#**, and select **Web**.</span></span>
3. <span data-ttu-id="df946-124">Vyberte **webové aplikace ASP.NET**.</span><span class="sxs-lookup"><span data-stu-id="df946-124">Select **ASP.NET Web Application**.</span></span>
4. <span data-ttu-id="df946-125">Zadejte název projektu hello.</span><span class="sxs-lookup"><span data-stu-id="df946-125">Fill in hello project name.</span></span> <span data-ttu-id="df946-126">Pak klikněte na **OK**.</span><span class="sxs-lookup"><span data-stu-id="df946-126">Then click **OK**.</span></span>
5. <span data-ttu-id="df946-127">V části *ASP.NET 4.5.2 šablony*, vyberte **mobilní aplikace Azure**.</span><span class="sxs-lookup"><span data-stu-id="df946-127">Under *ASP.NET 4.5.2 Templates*, select **Azure Mobile App**.</span></span> <span data-ttu-id="df946-128">Zkontrolujte **hostitel v cloudu hello** toocreate mobilního back-endu hello cloudu toowhich můžete publikovat tento projekt.</span><span class="sxs-lookup"><span data-stu-id="df946-128">Check **Host in hello cloud** toocreate a mobile backend in hello cloud toowhich you can publish this project.</span></span>
6. <span data-ttu-id="df946-129">Klikněte na **OK**.</span><span class="sxs-lookup"><span data-stu-id="df946-129">Click **OK**.</span></span>

## <span data-ttu-id="df946-130"><a name="install-sdk"></a>Postupy: stažení a inicializace hello SDK</span><span class="sxs-lookup"><span data-stu-id="df946-130"><a name="install-sdk"></a>How to: Download and initialize hello SDK</span></span>
<span data-ttu-id="df946-131">Hello SDK je k dispozici na [NuGet.org]. Tento balíček obsahuje tooget základní funkci požadovanou hello pomocí hello SDK spuštěna.</span><span class="sxs-lookup"><span data-stu-id="df946-131">hello SDK is available on [NuGet.org]. This package includes hello base functionality required tooget started using hello SDK.</span></span> <span data-ttu-id="df946-132">tooinitialize hello SDK, musíte tooperform akce na hello **HttpConfiguration** objektu.</span><span class="sxs-lookup"><span data-stu-id="df946-132">tooinitialize hello SDK, you need tooperform actions on hello **HttpConfiguration** object.</span></span>

### <a name="install-hello-sdk"></a><span data-ttu-id="df946-133">Nainstalujte hello SDK</span><span class="sxs-lookup"><span data-stu-id="df946-133">Install hello SDK</span></span>
<span data-ttu-id="df946-134">hello tooinstall SDK, klikněte pravým tlačítkem na hello serverový projekt v sadě Visual Studio, vyberte **spravovat balíčky NuGet**, vyhledejte hello [Microsoft.Azure.Mobile.Server] balíček a potom klikněte na  **Nainstalujte**.</span><span class="sxs-lookup"><span data-stu-id="df946-134">tooinstall hello SDK, right-click on hello server project in Visual Studio, select **Manage NuGet Packages**, search for hello [Microsoft.Azure.Mobile.Server] package, then click **Install**.</span></span>

### <span data-ttu-id="df946-135"><a name="server-project-setup"></a>Inicializace hello serverový projekt</span><span class="sxs-lookup"><span data-stu-id="df946-135"><a name="server-project-setup"></a> Initialize hello server project</span></span>
<span data-ttu-id="df946-136">Projekt .NET back-end server, je inicializovaného podobné tooother projekty ASP.NET, včetně třídy pro spuštění OWIN.</span><span class="sxs-lookup"><span data-stu-id="df946-136">A .NET backend server project is initialized similar tooother ASP.NET projects, by including an OWIN startup class.</span></span> <span data-ttu-id="df946-137">Ujistěte se, že máte odkazuje balíček NuGet hello `Microsoft.Owin.Host.SystemWeb`.</span><span class="sxs-lookup"><span data-stu-id="df946-137">Ensure that you have referenced hello NuGet package `Microsoft.Owin.Host.SystemWeb`.</span></span> <span data-ttu-id="df946-138">tooadd této třídy v sadě Visual Studio, klikněte pravým tlačítkem myši na serverový projekt a vyberte **přidat** >
**nová položka**, pak **webové**  >  ** Obecné** > **třídy pro spuštění OWIN**.</span><span class="sxs-lookup"><span data-stu-id="df946-138">tooadd this class in Visual Studio, right-click on your server project and select **Add** >
**New Item**, then **Web** > **General** > **OWIN Startup class**.</span></span>  <span data-ttu-id="df946-139">Třída je generována hello následující atribut:</span><span class="sxs-lookup"><span data-stu-id="df946-139">A class is generated with hello following attribute:</span></span>

    [assembly: OwinStartup(typeof(YourServiceName.YourStartupClassName))]

<span data-ttu-id="df946-140">V hello `Configuration()` metoda vaší třídy pro spuštění OWIN, použijte **HttpConfiguration** objekt tooconfigure hello Azure Mobile Apps prostředí.</span><span class="sxs-lookup"><span data-stu-id="df946-140">In hello `Configuration()` method of your OWIN startup class, use an **HttpConfiguration** object tooconfigure hello Azure Mobile Apps environment.</span></span>
<span data-ttu-id="df946-141">Následující ukázka Hello inicializuje serverový projekt hello se žádné nové funkce:</span><span class="sxs-lookup"><span data-stu-id="df946-141">hello following example initializes hello server project with no added features:</span></span>

    // in OWIN startup class
    public void Configuration(IAppBuilder app)
    {
        HttpConfiguration config = new HttpConfiguration();

        new MobileAppConfiguration()
            // no added features
            .ApplyTo(config);

        app.UseWebApi(config);
    }

<span data-ttu-id="df946-142">tooenable jednotlivé funkce, musí volat metody rozšíření pro hello **MobileAppConfiguration** objekt před voláním **metodu**.</span><span class="sxs-lookup"><span data-stu-id="df946-142">tooenable individual features, you must call extension methods on hello **MobileAppConfiguration** object before calling **ApplyTo**.</span></span> <span data-ttu-id="df946-143">Například následující kód hello přidá výchozí hello směruje řadiče tooall rozhraní API, které mají atribut hello `[MobileAppController]` během inicializace:</span><span class="sxs-lookup"><span data-stu-id="df946-143">For example, hello following code adds hello default routes tooall API controllers that have hello attribute `[MobileAppController]` during initialization:</span></span>

    new MobileAppConfiguration()
        .MapApiControllers()
        .ApplyTo(config);

<span data-ttu-id="df946-144">Rychlý start server Hello z hello Azure portálu volání **UseDefaultConfiguration()**.</span><span class="sxs-lookup"><span data-stu-id="df946-144">hello server quickstart from hello Azure portal calls **UseDefaultConfiguration()**.</span></span> <span data-ttu-id="df946-145">Tato ekvivalentní toohello následující nastavení:</span><span class="sxs-lookup"><span data-stu-id="df946-145">This equivalent toohello following setup:</span></span>

        new MobileAppConfiguration()
            .AddMobileAppHomeController()             // from hello Home package
            .MapApiControllers()
            .AddTables(                               // from hello Tables package
                new MobileAppTableConfiguration()
                    .MapTableControllers()
                    .AddEntityFramework()             // from hello Entity package
                )
            .AddPushNotifications()                   // from hello Notifications package
            .MapLegacyCrossDomainController()         // from hello CrossDomain package
            .ApplyTo(config);

<span data-ttu-id="df946-146">Hello rozšiřující metody používané jsou:</span><span class="sxs-lookup"><span data-stu-id="df946-146">hello extension methods used are:</span></span>

* <span data-ttu-id="df946-147">`AddMobileAppHomeController()`poskytuje hello výchozí Azure Mobile Apps domovskou stránku.</span><span class="sxs-lookup"><span data-stu-id="df946-147">`AddMobileAppHomeController()` provides hello default Azure Mobile Apps home page.</span></span>
* <span data-ttu-id="df946-148">`MapApiControllers()`poskytuje možnosti vlastního rozhraní API pro WebAPI řadiče označených pomocí hello `[MobileAppController]` atribut.</span><span class="sxs-lookup"><span data-stu-id="df946-148">`MapApiControllers()` provides custom API capabilities for WebAPI controllers decorated with hello `[MobileAppController]` attribute.</span></span>
* <span data-ttu-id="df946-149">`AddTables()`poskytuje mapování hello `/tables` tootable řadiče koncové body.</span><span class="sxs-lookup"><span data-stu-id="df946-149">`AddTables()` provides a mapping of hello `/tables` endpoints tootable controllers.</span></span>
* <span data-ttu-id="df946-150">`AddTablesWithEntityFramework()`je sdružený pro mapování hello `/tables` používající rozhraní Entity Framework koncových bodů na základě řadiče.</span><span class="sxs-lookup"><span data-stu-id="df946-150">`AddTablesWithEntityFramework()` is a short-hand for mapping hello `/tables` endpoints using Entity Framework based controllers.</span></span>
* <span data-ttu-id="df946-151">`AddPushNotifications()`poskytuje jednoduchý způsob registrace zařízení pro centra oznámení.</span><span class="sxs-lookup"><span data-stu-id="df946-151">`AddPushNotifications()` provides a simple method of registering devices for Notification Hubs.</span></span>
* <span data-ttu-id="df946-152">`MapLegacyCrossDomainController()`poskytuje standardní hlavičky CORS pro místní vývoj.</span><span class="sxs-lookup"><span data-stu-id="df946-152">`MapLegacyCrossDomainController()` provides standard CORS headers for local development.</span></span>

### <a name="sdk-extensions"></a><span data-ttu-id="df946-153">Rozšíření sady SDK</span><span class="sxs-lookup"><span data-stu-id="df946-153">SDK extensions</span></span>
<span data-ttu-id="df946-154">Hello následující balíčky NuGet prostřednictvím rozšíření poskytují různé mobilní funkce, které mohou být využívána vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="df946-154">hello following NuGet-based extension packages provide various mobile features that can be used by your application.</span></span> <span data-ttu-id="df946-155">Povolit rozšíření během inicializace s použitím hello **MobileAppConfiguration** objektu.</span><span class="sxs-lookup"><span data-stu-id="df946-155">You enable extensions during initialization by using hello **MobileAppConfiguration** object.</span></span>

* <span data-ttu-id="df946-156">[Microsoft.Azure.Mobile.Server.Quickstart] podporuje hello základní nastavení mobilní aplikace.</span><span class="sxs-lookup"><span data-stu-id="df946-156">[Microsoft.Azure.Mobile.Server.Quickstart] Supports hello basic Mobile Apps setup.</span></span> <span data-ttu-id="df946-157">Konfigurace přidané toohello pomocí volání hello **UseDefaultConfiguration** metoda rozšíření během inicializace.</span><span class="sxs-lookup"><span data-stu-id="df946-157">Added toohello configuration by calling hello **UseDefaultConfiguration** extension method during initialization.</span></span> <span data-ttu-id="df946-158">Toto rozšíření zahrnuje následující rozšíření: oznámení, ověřování, entita, tabulky, mezi doménami a domovské balíčky.</span><span class="sxs-lookup"><span data-stu-id="df946-158">This extension includes following extensions: Notifications, Authentication, Entity, Tables, Cross-domain, and Home packages.</span></span> <span data-ttu-id="df946-159">Tento balíček je používán hello mobilní aplikace Quickstart na hello portál Azure k dispozici.</span><span class="sxs-lookup"><span data-stu-id="df946-159">This package is used by hello Mobile Apps Quickstart available on hello Azure portal.</span></span>
* <span data-ttu-id="df946-160">[Microsoft.Azure.Mobile.Server.Home](http://www.nuget.org/packages/Microsoft.Azure.Mobile.Server.Home/) implementuje výchozí hello *tuto aplikaci je spuštěný a funkční stránky* pro kořenový adresář webu hello.</span><span class="sxs-lookup"><span data-stu-id="df946-160">[Microsoft.Azure.Mobile.Server.Home](http://www.nuget.org/packages/Microsoft.Azure.Mobile.Server.Home/) Implements hello default *this mobile app is up and running page* for hello web site root.</span></span> <span data-ttu-id="df946-161">Přidat konfiguraci toohello voláním **AddMobileAppHomeController** metoda rozšíření.</span><span class="sxs-lookup"><span data-stu-id="df946-161">Add toohello configuration by calling the **AddMobileAppHomeController** extension method.</span></span>
* <span data-ttu-id="df946-162">[Microsoft.Azure.Mobile.Server.Tables](http://www.nuget.org/packages/Microsoft.Azure.Mobile.Server.Tables/) obsahuje třídy pro práci s dat a nastaví až hello datovém kanálu.</span><span class="sxs-lookup"><span data-stu-id="df946-162">[Microsoft.Azure.Mobile.Server.Tables](http://www.nuget.org/packages/Microsoft.Azure.Mobile.Server.Tables/) includes classes for working with data and sets-up hello data pipeline.</span></span> <span data-ttu-id="df946-163">Přidat konfiguraci toohello volání hello **AddTables** metoda rozšíření.</span><span class="sxs-lookup"><span data-stu-id="df946-163">Add toohello configuration by calling hello **AddTables** extension method.</span></span>
* <span data-ttu-id="df946-164">[Microsoft.Azure.Mobile.Server.Entity](http://www.nuget.org/packages/Microsoft.Azure.Mobile.Server.Entity/) umožňuje hello Entity Framework tooaccess data v hello databáze SQL.</span><span class="sxs-lookup"><span data-stu-id="df946-164">[Microsoft.Azure.Mobile.Server.Entity](http://www.nuget.org/packages/Microsoft.Azure.Mobile.Server.Entity/) Enables hello Entity Framework tooaccess data in hello SQL Database.</span></span> <span data-ttu-id="df946-165">Přidat konfiguraci toohello volání hello **AddTablesWithEntityFramework** metoda rozšíření.</span><span class="sxs-lookup"><span data-stu-id="df946-165">Add toohello configuration by calling hello **AddTablesWithEntityFramework** extension method.</span></span>
* <span data-ttu-id="df946-166">[Microsoft.Azure.Mobile.Server.Authentication] umožňuje ověřování a nastaví up hello middlewaru OWIN, který používá toovalidate tokeny.</span><span class="sxs-lookup"><span data-stu-id="df946-166">[Microsoft.Azure.Mobile.Server.Authentication] Enables authentication and sets-up hello OWIN middleware used toovalidate tokens.</span></span> <span data-ttu-id="df946-167">Přidat konfiguraci toohello volání hello **AddAppServiceAuthentication** a **IAppBuilder**. **UseAppServiceAuthentication** rozšiřující metody.</span><span class="sxs-lookup"><span data-stu-id="df946-167">Add toohello configuration by calling hello **AddAppServiceAuthentication** and **IAppBuilder**.**UseAppServiceAuthentication** extension methods.</span></span>
* <span data-ttu-id="df946-168">[Microsoft.Azure.Mobile.Server.Notifications] povoluje nabízená oznámení a definuje koncový bod nabízené registrace.</span><span class="sxs-lookup"><span data-stu-id="df946-168">[Microsoft.Azure.Mobile.Server.Notifications] Enables push notifications and defines a push registration endpoint.</span></span> <span data-ttu-id="df946-169">Přidat konfiguraci toohello volání hello **AddPushNotifications** metoda rozšíření.</span><span class="sxs-lookup"><span data-stu-id="df946-169">Add toohello configuration by calling hello **AddPushNotifications** extension method.</span></span>
* <span data-ttu-id="df946-170">[Microsoft.Azure.Mobile.Server.CrossDomain](http://www.nuget.org/packages/Microsoft.Azure.Mobile.Server.CrossDomain/) vytvoří kontroler, který slouží data toolegacy webových prohlížečů z mobilní aplikace.</span><span class="sxs-lookup"><span data-stu-id="df946-170">[Microsoft.Azure.Mobile.Server.CrossDomain](http://www.nuget.org/packages/Microsoft.Azure.Mobile.Server.CrossDomain/) Creates a controller that serves data toolegacy web browsers from your Mobile App.</span></span> <span data-ttu-id="df946-171">Přidat konfiguraci toohello voláním **MapLegacyCrossDomainController** metoda rozšíření.</span><span class="sxs-lookup"><span data-stu-id="df946-171">Add toohello configuration by calling the **MapLegacyCrossDomainController** extension method.</span></span>
* <span data-ttu-id="df946-172">[Microsoft.Azure.Mobile.Server.Login] poskytuje hello AppServiceLoginHandler.CreateToken() metodu, která se používají při ověřování vlastní scénáře statickou metodu.</span><span class="sxs-lookup"><span data-stu-id="df946-172">[Microsoft.Azure.Mobile.Server.Login] Provides hello AppServiceLoginHandler.CreateToken() method, which is a static method used during custom authentication scenarios.</span></span>

## <span data-ttu-id="df946-173"><a name="publish-server-project"></a>Postupy: publikování projektu server hello</span><span class="sxs-lookup"><span data-stu-id="df946-173"><a name="publish-server-project"></a>How to: Publish hello server project</span></span>
<span data-ttu-id="df946-174">Tato část uvádí, jak toopublish váš back-end .NET projektu ze sady Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="df946-174">This section shows you how toopublish your .NET backend project from Visual Studio.</span></span> <span data-ttu-id="df946-175">Můžete taky nasadit projektu back-end pomocí Git nebo žádné z hello jiné metody popsané v hello [dokumentaci k nasazení služby Azure App Service](../app-service-web/web-sites-deploy.md).</span><span class="sxs-lookup"><span data-stu-id="df946-175">You can also deploy your backend project using Git or any of hello other methods covered in hello [Azure App Service deployment documentation](../app-service-web/web-sites-deploy.md).</span></span>

1. <span data-ttu-id="df946-176">V sadě Visual Studio znovu sestavte balíčky NuGet toorestore hello projektu.</span><span class="sxs-lookup"><span data-stu-id="df946-176">In Visual Studio, rebuild hello project toorestore NuGet packages.</span></span>
2. <span data-ttu-id="df946-177">V Průzkumníku řešení klikněte pravým tlačítkem na projekt hello, klikněte na tlačítko **publikovat**.</span><span class="sxs-lookup"><span data-stu-id="df946-177">In Solution Explorer, right-click hello project, click **Publish**.</span></span> <span data-ttu-id="df946-178">Hello prvním publikujete, je nutné toodefine profil publikování.</span><span class="sxs-lookup"><span data-stu-id="df946-178">hello first time you publish, you need toodefine a publishing profile.</span></span> <span data-ttu-id="df946-179">Pokud již máte profil definované, můžete jej vybrat a klikněte na tlačítko **publikovat**.</span><span class="sxs-lookup"><span data-stu-id="df946-179">When you already have a profile defined, you can select it and click **Publish**.</span></span>
3. <span data-ttu-id="df946-180">Pokud se zobrazí dotaz tooselect cíl publikování, klikněte na tlačítko **Microsoft Azure App Service** > **Další**, pak (v případě potřeby) přihlásit se pomocí svých přihlašovacích údajů Azure.</span><span class="sxs-lookup"><span data-stu-id="df946-180">If asked tooselect a publish target, click **Microsoft Azure App Service** > **Next**, then (if needed) sign-in with your Azure credentials.</span></span>
   <span data-ttu-id="df946-181">Visual Studio stáhne a bezpečně uloží vaše nastavení publikování přímo z Azure.</span><span class="sxs-lookup"><span data-stu-id="df946-181">Visual Studio downloads and securely stores your publish settings directly from Azure.</span></span>

    ![](./media/app-service-mobile-dotnet-backend-how-to-use-server-sdk/publish-wizard-1.png)
4. <span data-ttu-id="df946-182">Zvolte vaše **předplatné**, vyberte **typ prostředku** z **zobrazení**, rozbalte položku **mobilní aplikace**a klikněte na váš back-end mobilní aplikace a pak klikněte na **OK**.</span><span class="sxs-lookup"><span data-stu-id="df946-182">Choose your **Subscription**, select **Resource Type** from **View**, expand **Mobile App**, and click your Mobile App backend, then click **OK**.</span></span>

    ![](./media/app-service-mobile-dotnet-backend-how-to-use-server-sdk/publish-wizard-2.png)
5. <span data-ttu-id="df946-183">Ověřte hello publikovat informace o profilu a klikněte na tlačítko **publikovat**.</span><span class="sxs-lookup"><span data-stu-id="df946-183">Verify hello publish profile information and click **Publish**.</span></span>

    ![](./media/app-service-mobile-dotnet-backend-how-to-use-server-sdk/publish-wizard-3.png)

    <span data-ttu-id="df946-184">Pokud váš back-end mobilní aplikace byla úspěšně publikována, zobrazí se cílová stránka udávající úspěch.</span><span class="sxs-lookup"><span data-stu-id="df946-184">When your Mobile App backend has published successfully, you see a landing page indicating success.</span></span>

    ![](./media/app-service-mobile-dotnet-backend-how-to-use-server-sdk/publish-success.png)

## <span data-ttu-id="df946-185"><a name="define-table-controller"></a>Postupy: definování řadič tabulky</span><span class="sxs-lookup"><span data-stu-id="df946-185"><a name="define-table-controller"></a> How to: Define a table controller</span></span>
<span data-ttu-id="df946-186">Definujte tooexpose tabulky řadiče klienti toomobile tabulky SQL.</span><span class="sxs-lookup"><span data-stu-id="df946-186">Define a Table Controller tooexpose a SQL table toomobile clients.</span></span>  <span data-ttu-id="df946-187">Konfigurace řadič tabulky vyžaduje tři kroky:</span><span class="sxs-lookup"><span data-stu-id="df946-187">Configuring a Table Controller requires three steps:</span></span>

1. <span data-ttu-id="df946-188">Vytvořte třídu objektu pro přenos dat (DTO).</span><span class="sxs-lookup"><span data-stu-id="df946-188">Create a Data Transfer Object (DTO) class.</span></span>
2. <span data-ttu-id="df946-189">Nakonfigurujte odkaz na tabulku v hello třídy Mobile DbContext.</span><span class="sxs-lookup"><span data-stu-id="df946-189">Configure a table reference in hello Mobile DbContext class.</span></span>
3. <span data-ttu-id="df946-190">Vytvořte řadič tabulky.</span><span class="sxs-lookup"><span data-stu-id="df946-190">Create a table controller.</span></span>

<span data-ttu-id="df946-191">Objekt pro přenos dat (DTO) je prostý C# objekt, který dědí z `EntityData`.</span><span class="sxs-lookup"><span data-stu-id="df946-191">A Data Transfer Object (DTO) is a plain C# object that inherits from `EntityData`.</span></span>  <span data-ttu-id="df946-192">Například:</span><span class="sxs-lookup"><span data-stu-id="df946-192">For example:</span></span>

    public class TodoItem : EntityData
    {
        public string Text { get; set; }
        public bool Complete {get; set;}
    }

<span data-ttu-id="df946-193">Hello DTO je použité toodefine hello tabulky v databázi SQL hello.</span><span class="sxs-lookup"><span data-stu-id="df946-193">hello DTO is used toodefine hello table within hello SQL database.</span></span>  <span data-ttu-id="df946-194">toocreate hello databáze položky, přidejte `DbSet<>` vlastnost hello DbContext, kterou používáte.</span><span class="sxs-lookup"><span data-stu-id="df946-194">toocreate hello database entry, add a `DbSet<>` property to hello DbContext you are using.</span></span>  <span data-ttu-id="df946-195">V hello výchozí šablona projektu pro Azure Mobile Apps, hello DbContext nazývá `Models\MobileServiceContext.cs`:</span><span class="sxs-lookup"><span data-stu-id="df946-195">In hello default project template for Azure Mobile Apps, hello DbContext is called `Models\MobileServiceContext.cs`:</span></span>

    public class MobileServiceContext : DbContext
    {
        private const string connectionStringName = "Name=MS_TableConnectionString";

        public MobileServiceContext() : base(connectionStringName)
        {

        }

        public DbSet<TodoItem> TodoItems { get; set; }

        protected override void OnModelCreating(DbModelBuilder modelBuilder)
        {
            modelBuilder.Conventions.Add(
                new AttributeToColumnAnnotationConvention<TableColumnAttribute, string>(
                    "ServiceColumnTable", (property, attributes) => attributes.Single().ColumnType.ToString()));
        }
    }

<span data-ttu-id="df946-196">Pokud máte hello instalace Azure SDK, nyní můžete vytvořit řadič tabulky šablony následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="df946-196">If you have hello Azure SDK installed, you can now create a template table controller as follows:</span></span>

1. <span data-ttu-id="df946-197">Klikněte pravým tlačítkem na složku hello řadiče a vyberte **přidat** > **řadiče...** .</span><span class="sxs-lookup"><span data-stu-id="df946-197">Right-click on hello Controllers folder and select **Add** > **Controller...**.</span></span>
2. <span data-ttu-id="df946-198">Vyberte hello **Azure Mobile Apps tabulky řadič** možnost a potom klikněte na **přidat**.</span><span class="sxs-lookup"><span data-stu-id="df946-198">Select hello **Azure Mobile Apps Table Controller** option, then click **Add**.</span></span>
3. <span data-ttu-id="df946-199">V hello **přidat kontroler** dialogové okno:</span><span class="sxs-lookup"><span data-stu-id="df946-199">In hello **Add Controller** dialog:</span></span>
   * <span data-ttu-id="df946-200">V hello **třída modelu** rozevíracího seznamu, vyberte vaše nové DTO.</span><span class="sxs-lookup"><span data-stu-id="df946-200">In hello **Model class** dropdown, select your new DTO.</span></span>
   * <span data-ttu-id="df946-201">V hello **DbContext** rozevíracím třídy DbContext služby Mobile vyberte hello.</span><span class="sxs-lookup"><span data-stu-id="df946-201">In hello **DbContext** dropdown, select hello Mobile Service DbContext class.</span></span>
   * <span data-ttu-id="df946-202">název řadiče Hello je vytvořen.</span><span class="sxs-lookup"><span data-stu-id="df946-202">hello Controller name is created for you.</span></span>
4. <span data-ttu-id="df946-203">Klikněte na tlačítko **Přidat**.</span><span class="sxs-lookup"><span data-stu-id="df946-203">Click **Add**.</span></span>

<span data-ttu-id="df946-204">obsahuje Hello rychlý start serverový projekt pro jednoduchý příklad **TodoItemController**.</span><span class="sxs-lookup"><span data-stu-id="df946-204">hello quickstart server project contains an example for a simple **TodoItemController**.</span></span>

### <span data-ttu-id="df946-205"><a name="adjust-pagesize"></a>Postupy: přizpůsobení velikosti stránkování tabulky hello</span><span class="sxs-lookup"><span data-stu-id="df946-205"><a name="adjust-pagesize"></a>How to: Adjust hello table paging size</span></span>
<span data-ttu-id="df946-206">Ve výchozím nastavení vrátí Azure Mobile Apps 50 záznamů na základě požadavku.</span><span class="sxs-lookup"><span data-stu-id="df946-206">By default, Azure Mobile Apps returns 50 records per request.</span></span>  <span data-ttu-id="df946-207">Stránkování zajišťuje, že klient hello neblokuje jejich uživatelského rozhraní vlákno ani hello server příliš dlouho, zajišťuje vhodné uživatelské prostředí.</span><span class="sxs-lookup"><span data-stu-id="df946-207">Paging ensures that hello client does not tie up their UI thread nor hello server for too long, ensuring a good user experience.</span></span> <span data-ttu-id="df946-208">velikost toochange hello tabulky stránkování, zvýšení na straně serveru hello "povolená velikost dotazu" a hello stránky na straně klienta velikost hello straně serveru "povolená velikost dotazu" je upravit pomocí hello `EnableQuery` atribut:</span><span class="sxs-lookup"><span data-stu-id="df946-208">toochange hello table paging size, increase hello server side "allowed query size" and hello client-side page size hello server side "allowed query size" is adjusted using hello `EnableQuery` attribute:</span></span>

    [EnableQuery(PageSize = 500)]

<span data-ttu-id="df946-209">Zkontrolujte, zda text hello PageSize hello stejné nebo větší než požadovaná klientem hello velikost hello.</span><span class="sxs-lookup"><span data-stu-id="df946-209">Ensure hello PageSize is hello same or larger than hello size requested by hello client.</span></span>  <span data-ttu-id="df946-210">Další informace o změně velikosti stránky hello klienta naleznete v toohello konkrétního klienta postupy dokumentaci.</span><span class="sxs-lookup"><span data-stu-id="df946-210">Refer toohello specific client HOWTO documentation for details on changing hello client page size.</span></span>

## <a name="how-to-define-a-custom-api-controller"></a><span data-ttu-id="df946-211">Postupy: definování vlastní kontroler API</span><span class="sxs-lookup"><span data-stu-id="df946-211">How to: Define a custom API controller</span></span>
<span data-ttu-id="df946-212">kontroler API vlastní Hello poskytuje hello nejzákladnější funkce tooyour mobilní back-end aplikace tím, že vystavení koncový bod.</span><span class="sxs-lookup"><span data-stu-id="df946-212">hello custom API controller provides hello most basic functionality tooyour Mobile App backend by exposing an endpoint.</span></span> <span data-ttu-id="df946-213">Můžete zaregistrovat řadič specifické mobilní rozhraní API pomocí atributu hello [MobileAppController].</span><span class="sxs-lookup"><span data-stu-id="df946-213">You can register a mobile-specific API controller using hello [MobileAppController] attribute.</span></span> <span data-ttu-id="df946-214">Hello `MobileAppController` atribut zaregistruje trasu hello, nastaví serializátor JSON mobilní aplikace hello a zapne [kontrolu verzí klienta](app-service-mobile-client-and-server-versioning.md).</span><span class="sxs-lookup"><span data-stu-id="df946-214">hello `MobileAppController` attribute registers hello route, sets up hello Mobile Apps JSON serializer, and turns on [client version checking](app-service-mobile-client-and-server-versioning.md).</span></span>

1. <span data-ttu-id="df946-215">V sadě Visual Studio, klikněte pravým tlačítkem na složku řadiče hello, pak klikněte na tlačítko **přidat** > **řadič**, vyberte **kontroler Web API 2&mdash;prázdný** a Klikněte na tlačítko **přidat**.</span><span class="sxs-lookup"><span data-stu-id="df946-215">In Visual Studio, right-click hello Controllers folder, then click **Add** > **Controller**, select **Web API 2 Controller&mdash;Empty** and click **Add**.</span></span>
2. <span data-ttu-id="df946-216">Zadejte **názvu Kontroleru**, jako například `CustomController`a klikněte na tlačítko **přidat**.</span><span class="sxs-lookup"><span data-stu-id="df946-216">Supply a **Controller name**, such as `CustomController`, and click **Add**.</span></span>
3. <span data-ttu-id="df946-217">Hello nový soubor třídy kontroleru, přidejte hello následující příkaz using:</span><span class="sxs-lookup"><span data-stu-id="df946-217">In hello new controller class file, add hello following using statement:</span></span>

        using Microsoft.Azure.Mobile.Server.Config;
4. <span data-ttu-id="df946-218">Použít hello **[MobileAppController]** atribut definice třídy řadiče toohello rozhraní API, stejně jako hello následující ukázka:</span><span class="sxs-lookup"><span data-stu-id="df946-218">Apply hello **[MobileAppController]** attribute toohello API controller class definition, as in hello following example:</span></span>

        [MobileAppController]
        public class CustomController : ApiController
        {
              //...
        }
5. <span data-ttu-id="df946-219">V souboru App_Start/Startup.MobileApp.cs přidejte volání toohello **MapApiControllers** – metoda rozšíření, jako hello následující ukázka:</span><span class="sxs-lookup"><span data-stu-id="df946-219">In App_Start/Startup.MobileApp.cs file, add a call toohello **MapApiControllers** extension method, as in hello following example:</span></span>

        new MobileAppConfiguration()
            .MapApiControllers()
            .ApplyTo(config);

<span data-ttu-id="df946-220">Můžete taky hello `UseDefaultConfiguration()` metoda rozšíření místo `MapApiControllers()`.</span><span class="sxs-lookup"><span data-stu-id="df946-220">You can also use hello `UseDefaultConfiguration()` extension method instead of `MapApiControllers()`.</span></span> <span data-ttu-id="df946-221">Každý kontroler, který nemá **MobileAppControllerAttribute** použít můžete i nadále přistupovat klienti, ale nemusí být zpracován správně klienty pomocí libovolného mobilní aplikace klienta SDK.</span><span class="sxs-lookup"><span data-stu-id="df946-221">Any controller that does not have **MobileAppControllerAttribute** applied can still be accessed by clients, but it may not be correctly consumed by clients using any Mobile App client SDK.</span></span>

## <a name="how-to-work-with-authentication"></a><span data-ttu-id="df946-222">Postupy: práce s ověřováním</span><span class="sxs-lookup"><span data-stu-id="df946-222">How to: Work with authentication</span></span>
<span data-ttu-id="df946-223">Azure Mobile Apps používá aplikace služby ověřování / autorizace toosecure mobilního back-endu.</span><span class="sxs-lookup"><span data-stu-id="df946-223">Azure Mobile Apps uses App Service Authentication / Authorization toosecure your mobile backend.</span></span>  <span data-ttu-id="df946-224">V této části se dozvíte, jak tooperform hello následující úkolech týkajících se ověřování v rozhraní .NET back-end serveru projektu:</span><span class="sxs-lookup"><span data-stu-id="df946-224">This section shows you how tooperform hello following authentication-related tasks in your .NET backend server project:</span></span>

* [<span data-ttu-id="df946-225">Postupy: přidání ověřování tooa serverový projekt</span><span class="sxs-lookup"><span data-stu-id="df946-225">How to: Add authentication tooa server project</span></span>](#add-auth)
* [<span data-ttu-id="df946-226">Postupy: použití vlastního ověřování pro aplikaci</span><span class="sxs-lookup"><span data-stu-id="df946-226">How to: Use custom authentication for your application</span></span>](#custom-auth)
* [<span data-ttu-id="df946-227">Postupy: načtení ověřit informace o uživateli</span><span class="sxs-lookup"><span data-stu-id="df946-227">How to: Retrieve authenticated user information</span></span>](#user-info)
* [<span data-ttu-id="df946-228">Postupy: omezení přístupu k datům Autorizovaní uživatelé</span><span class="sxs-lookup"><span data-stu-id="df946-228">How to: Restrict data access for authorized users</span></span>](#authorize)

### <span data-ttu-id="df946-229"><a name="add-auth"></a>Postupy: přidání ověřování tooa serverový projekt</span><span class="sxs-lookup"><span data-stu-id="df946-229"><a name="add-auth"></a>How to: Add authentication tooa server project</span></span>
<span data-ttu-id="df946-230">Můžete přidat ověřování tooyour serverový projekt rozšířením hello **MobileAppConfiguration** objekt a konfiguraci middlewaru OWIN.</span><span class="sxs-lookup"><span data-stu-id="df946-230">You can add authentication tooyour server project by extending hello **MobileAppConfiguration** object and configuring OWIN middleware.</span></span> <span data-ttu-id="df946-231">Při instalaci hello [Microsoft.Azure.Mobile.Server.Quickstart] balíčku a volání hello **UseDefaultConfiguration** metoda rozšíření, můžete přeskočit toostep 3.</span><span class="sxs-lookup"><span data-stu-id="df946-231">When you install hello [Microsoft.Azure.Mobile.Server.Quickstart] package and call hello **UseDefaultConfiguration** extension method, you can skip toostep 3.</span></span>

1. <span data-ttu-id="df946-232">V sadě Visual Studio, nainstalujte hello [Microsoft.Azure.Mobile.Server.Authentication] balíčku.</span><span class="sxs-lookup"><span data-stu-id="df946-232">In Visual Studio, install hello [Microsoft.Azure.Mobile.Server.Authentication] package.</span></span>
2. <span data-ttu-id="df946-233">V souboru Startup.cs projektu hello, přidejte následující řádek kódu na začátku hello hello hello **konfigurace** metoda:</span><span class="sxs-lookup"><span data-stu-id="df946-233">In hello Startup.cs project file, add hello following line of code at hello beginning of hello **Configuration** method:</span></span>

        app.UseAppServiceAuthentication(config);

    <span data-ttu-id="df946-234">Tato komponenta middlewaru OWIN ověřuje tokeny vydané bránou hello související služby App Service.</span><span class="sxs-lookup"><span data-stu-id="df946-234">This OWIN middleware component validates tokens issued by hello associated App Service gateway.</span></span>
3. <span data-ttu-id="df946-235">Přidat hello `[Authorize]` atribut tooany kontroler nebo metodu, která vyžaduje ověření.</span><span class="sxs-lookup"><span data-stu-id="df946-235">Add hello `[Authorize]` attribute tooany controller or method that requires authentication.</span></span>

<span data-ttu-id="df946-236">toolearn o tooauthenticate klienti tooyour back-end mobilní aplikace, najdete v části [přidat ověřování tooyour aplikace](app-service-mobile-ios-get-started-users.md).</span><span class="sxs-lookup"><span data-stu-id="df946-236">toolearn about how tooauthenticate clients tooyour Mobile Apps backend, see [Add authentication tooyour app](app-service-mobile-ios-get-started-users.md).</span></span>

### <span data-ttu-id="df946-237"><a name="custom-auth"></a>Postupy: použití vlastního ověřování pro aplikaci</span><span class="sxs-lookup"><span data-stu-id="df946-237"><a name="custom-auth"></a>How to: Use custom authentication for your application</span></span>
<span data-ttu-id="df946-238">Pokud nechcete, aby toouse jednoho z poskytovatelů hello ověřování/autorizace služby App Service, můžete implementovat systému přihlášení.</span><span class="sxs-lookup"><span data-stu-id="df946-238">If you do not wish toouse one of hello App Service Authentication/Authorization providers, you can implement your own login system.</span></span> <span data-ttu-id="df946-239">Nainstalujte hello [Microsoft.Azure.Mobile.Server.Login] balíček tooassist s generování tokenů ověřování.</span><span class="sxs-lookup"><span data-stu-id="df946-239">Install hello [Microsoft.Azure.Mobile.Server.Login] package tooassist with authentication token generation.</span></span>  <span data-ttu-id="df946-240">Zadejte vlastní kód pro ověření pověření uživatele.</span><span class="sxs-lookup"><span data-stu-id="df946-240">Provide your own code for validating user credentials.</span></span> <span data-ttu-id="df946-241">Můžete například zkontrolovat proti solené a hash hesla v databázi.</span><span class="sxs-lookup"><span data-stu-id="df946-241">For example, you might check against salted and hashed passwords in a database.</span></span> <span data-ttu-id="df946-242">V příkladu hello níže hello `isValidAssertion()` – metoda (definovanou jinde) zodpovídá za tyto kontroly.</span><span class="sxs-lookup"><span data-stu-id="df946-242">In hello example below, hello `isValidAssertion()` method (defined elsewhere) is responsible for these checks.</span></span>

<span data-ttu-id="df946-243">vlastní ověřování Hello je zveřejněný prostřednictvím objektu ApiController vytvoření a vystavení `register` a `login` akce.</span><span class="sxs-lookup"><span data-stu-id="df946-243">hello custom authentication is exposed by creating an ApiController and exposing `register` and `login` actions.</span></span> <span data-ttu-id="df946-244">Hello klient musí použít vlastní informace hello toocollect uživatelského rozhraní od uživatele hello.</span><span class="sxs-lookup"><span data-stu-id="df946-244">hello client should use a custom UI toocollect hello information from hello user.</span></span>  <span data-ttu-id="df946-245">informace o Hello je zavolat odeslaná toohello rozhraní API s standardní HTTP POST.</span><span class="sxs-lookup"><span data-stu-id="df946-245">hello information is then submitted toohello API with a standard HTTP POST call.</span></span> <span data-ttu-id="df946-246">Jakmile hello server ověří hello assertion, pomocí hello token je vydat `AppServiceLoginHandler.CreateToken()` metoda.</span><span class="sxs-lookup"><span data-stu-id="df946-246">Once hello server validates hello assertion, a token is issued using hello `AppServiceLoginHandler.CreateToken()` method.</span></span>  <span data-ttu-id="df946-247">Hello objektu ApiController **neměli** použít hello `[MobileAppController]` atribut.</span><span class="sxs-lookup"><span data-stu-id="df946-247">hello ApiController **should not** use hello `[MobileAppController]` attribute.</span></span>

<span data-ttu-id="df946-248">Příklad `login` akce:</span><span class="sxs-lookup"><span data-stu-id="df946-248">An example `login` action:</span></span>

        public IHttpActionResult Post([FromBody] JObject assertion)
        {
            if (isValidAssertion(assertion)) // user-defined function, checks against a database
            {
                JwtSecurityToken token = AppServiceLoginHandler.CreateToken(new Claim[] { new Claim(JwtRegisteredClaimNames.Sub, assertion["username"]) },
                    mySigningKey,
                    myAppURL,
                    myAppURL,
                    TimeSpan.FromHours(24) );
                return Ok(new LoginResult()
                {
                    AuthenticationToken = token.RawData,
                    User = new LoginResultUser() { UserId = userName.ToString() }
                });
            }
            else // user assertion was not valid
            {
                return this.Request.CreateUnauthorizedResponse();
            }
        }

<span data-ttu-id="df946-249">V předchozím příkladu hello LoginResult a LoginResultUser jsou serializovatelné objekty vystavení požadované vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="df946-249">In hello preceding example, LoginResult and LoginResultUser are serializable objects exposing required properties.</span></span> <span data-ttu-id="df946-250">Klient Hello očekává toobe odpovědi přihlášení vrátí jako objekty JSON hello formuláře:</span><span class="sxs-lookup"><span data-stu-id="df946-250">hello client expects login responses toobe returned as JSON objects of hello form:</span></span>

        {
            "authenticationToken": "<token>",
            "user": {
                "userId": "<userId>"
            }
        }

<span data-ttu-id="df946-251">Hello `AppServiceLoginHandler.CreateToken()` metoda zahrnuje *cílovou skupinu* a *vystavitele* parametr.</span><span class="sxs-lookup"><span data-stu-id="df946-251">hello `AppServiceLoginHandler.CreateToken()` method includes an *audience* and an *issuer* parameter.</span></span> <span data-ttu-id="df946-252">Oba tyto parametry se nastavují toohello URL kořenového adresáře aplikace pomocí hello schéma HTTPS.</span><span class="sxs-lookup"><span data-stu-id="df946-252">Both of these parameters are set toohello URL of your application root, using hello HTTPS scheme.</span></span> <span data-ttu-id="df946-253">Podobně byste měli nastavit *secretKey* toobe hello hodnotu vaší aplikace je podpisový klíč.</span><span class="sxs-lookup"><span data-stu-id="df946-253">Similarly you should set *secretKey* toobe hello value of your application's signing key.</span></span> <span data-ttu-id="df946-254">Nedistribuovat hello podpisového klíče v klientovi jako může být použité toomint klíče a zosobnit uživatele.</span><span class="sxs-lookup"><span data-stu-id="df946-254">Do not distribute hello signing key in a client as it can be used toomint keys and impersonate users.</span></span> <span data-ttu-id="df946-255">Můžete získat hello podpisového klíče při odkazování na hello hostované ve službě App Service *webu\_AUTH\_PODPISOVÁNÍ\_klíč* proměnné prostředí.</span><span class="sxs-lookup"><span data-stu-id="df946-255">You can obtain hello signing key while hosted in App Service by referencing hello *WEBSITE\_AUTH\_SIGNING\_KEY* environment variable.</span></span> <span data-ttu-id="df946-256">V případě potřeby v kontextu místního ladění, postupujte podle pokynů hello v hello [místní ladění pomocí ověřování](#local-debug) části tooretrieve hello klíč a uložte ho jako nastavení aplikace.</span><span class="sxs-lookup"><span data-stu-id="df946-256">If needed in a local debugging context, follow hello instructions in hello [Local debugging with authentication](#local-debug) section tooretrieve hello key and store it as an application setting.</span></span>

<span data-ttu-id="df946-257">token vydán Hello zahrnovat také další deklarace identity a datum vypršení platnosti.</span><span class="sxs-lookup"><span data-stu-id="df946-257">hello issued token may also include other claims and an expiry date.</span></span>  <span data-ttu-id="df946-258">Minimálně hello vydán token musí obsahovat předmět (**sub**) deklarace identity.</span><span class="sxs-lookup"><span data-stu-id="df946-258">Minimally, hello issued token must include a subject (**sub**) claim.</span></span>

<span data-ttu-id="df946-259">Může podporovat standard klienta hello `loginAsync()` metoda podle přetížení hello ověřování trasy.</span><span class="sxs-lookup"><span data-stu-id="df946-259">You can support hello standard client `loginAsync()` method by overloading hello authentication route.</span></span>  <span data-ttu-id="df946-260">Pokud klient hello volá `client.loginAsync('custom');` toolog ve vašem cesty musí být `/.auth/login/custom`.</span><span class="sxs-lookup"><span data-stu-id="df946-260">If hello client calls `client.loginAsync('custom');` toolog in, your route must be `/.auth/login/custom`.</span></span>  <span data-ttu-id="df946-261">Můžete nastavit hello trasu pro hello vlastní ověřování řadiče pomocí `MapHttpRoute()`:</span><span class="sxs-lookup"><span data-stu-id="df946-261">You can set hello route for hello custom authentication controller using `MapHttpRoute()`:</span></span>

    config.Routes.MapHttpRoute("custom", ".auth/login/custom", new { controller = "CustomAuth" });

> [!TIP]
> <span data-ttu-id="df946-262">Pomocí hello `loginAsync()` přístup zajišťuje, že ověřovací token hello je připojen tooevery následných volání toohello služby.</span><span class="sxs-lookup"><span data-stu-id="df946-262">Using hello `loginAsync()` approach ensures that hello authentication token is attached tooevery subsequent call toohello service.</span></span>
>
>

### <span data-ttu-id="df946-263"><a name="user-info"></a>Postupy: načtení ověřit informace o uživateli</span><span class="sxs-lookup"><span data-stu-id="df946-263"><a name="user-info"></a>How to: Retrieve authenticated user information</span></span>
<span data-ttu-id="df946-264">Pokud je uživatel ověřen službou App Service, můžete přistupovat hello přiřazené ID uživatele a další informace v rozhraní .NET back-end kódu.</span><span class="sxs-lookup"><span data-stu-id="df946-264">When a user is authenticated by App Service, you can access hello assigned user ID and other information in your .NET backend code.</span></span> <span data-ttu-id="df946-265">informace o uživateli Hello slouží pro při autorizačním rozhodování v back-end hello.</span><span class="sxs-lookup"><span data-stu-id="df946-265">hello user information can be used for making authorization decisions in hello backend.</span></span> <span data-ttu-id="df946-266">Hello následující kód získá ID uživatele hello přidružený k požadavku:</span><span class="sxs-lookup"><span data-stu-id="df946-266">hello following code obtains hello user ID associated with a request:</span></span>

    // Get hello SID of hello current user.
    var claimsPrincipal = this.User as ClaimsPrincipal;
    string sid = claimsPrincipal.FindFirst(ClaimTypes.NameIdentifier).Value;

<span data-ttu-id="df946-267">Hello SID je odvozen od ID uživatele specifický pro zprostředkovatele hello a je statický pro daného uživatele a zprostředkovatele přihlášení.</span><span class="sxs-lookup"><span data-stu-id="df946-267">hello SID is derived from hello provider-specific user ID and is static for a given user and login provider.</span></span>  <span data-ttu-id="df946-268">Hello SID má hodnotu null pro tokeny neplatný ověřování.</span><span class="sxs-lookup"><span data-stu-id="df946-268">hello SID is null for invalid authentication tokens.</span></span>

<span data-ttu-id="df946-269">Služby App Service umožňuje taky deklarace identity specifické požadavky na poskytovatele přihlášení.</span><span class="sxs-lookup"><span data-stu-id="df946-269">App Service also lets you request specific claims from your login provider.</span></span> <span data-ttu-id="df946-270">Každý poskytovatel identity může poskytovat další informace pomocí zprostředkovatele identity SDK.</span><span class="sxs-lookup"><span data-stu-id="df946-270">Each identity provider can provide more information using the identity provider SDK.</span></span>  <span data-ttu-id="df946-271">Například můžete hello Graph API sítě Facebook informace přátel.</span><span class="sxs-lookup"><span data-stu-id="df946-271">For example, you can use hello Facebook Graph API for friends information.</span></span>  <span data-ttu-id="df946-272">Zadávat lze deklarace identity, které jsou požadovány v okně zprostředkovatele hello v hello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="df946-272">You can specify claims that are requested in hello provider blade in hello Azure portal.</span></span> <span data-ttu-id="df946-273">Některé deklarace identity vyžadovat dodatečnou konfiguraci pomocí zprostředkovatele identity hello.</span><span class="sxs-lookup"><span data-stu-id="df946-273">Some claims require additional configuration with hello identity provider.</span></span>

<span data-ttu-id="df946-274">Hello následující kód volání hello **GetAppServiceIdentityAsync** rozšíření metoda tooget hello přihlašovacích údajů, které zahrnují požadavky na přístup tokenu potřebné toomake hello proti hello Graph API sítě Facebook:</span><span class="sxs-lookup"><span data-stu-id="df946-274">hello following code calls hello **GetAppServiceIdentityAsync** extension method tooget hello login credentials, which include hello access token needed toomake requests against hello Facebook Graph API:</span></span>

    // Get hello credentials for hello logged-in user.
    var credentials =
        await this.User
        .GetAppServiceIdentityAsync<FacebookCredentials>(this.Request);

    if (credentials.Provider == "Facebook")
    {
        // Create a query string with hello Facebook access token.
        var fbRequestUrl = "https://graph.facebook.com/me/feed?access_token="
            + credentials.AccessToken;

        // Create an HttpClient request.
        var client = new System.Net.Http.HttpClient();

        // Request hello current user info from Facebook.
        var resp = await client.GetAsync(fbRequestUrl);
        resp.EnsureSuccessStatusCode();

        // Do something here with hello Facebook user information.
        var fbInfo = await resp.Content.ReadAsStringAsync();
    }

<span data-ttu-id="df946-275">Přidat pomocí příkazu pro `System.Security.Principal` tooprovide hello **GetAppServiceIdentityAsync** metoda rozšíření.</span><span class="sxs-lookup"><span data-stu-id="df946-275">Add a using statement for `System.Security.Principal` tooprovide hello **GetAppServiceIdentityAsync** extension method.</span></span>

### <span data-ttu-id="df946-276"><a name="authorize"></a>Postupy: omezení přístupu k datům Autorizovaní uživatelé</span><span class="sxs-lookup"><span data-stu-id="df946-276"><a name="authorize"></a>How to: Restrict data access for authorized users</span></span>
<span data-ttu-id="df946-277">V předchozí části hello jsme vám ukázal, jak tooretrieve hello ID uživatele ověřeného uživatele.</span><span class="sxs-lookup"><span data-stu-id="df946-277">In hello previous section, we showed how tooretrieve hello user ID of an authenticated user.</span></span> <span data-ttu-id="df946-278">Můžete omezit přístup toodata a dalším prostředkům na základě této hodnoty.</span><span class="sxs-lookup"><span data-stu-id="df946-278">You can restrict access toodata and other resources based on this value.</span></span> <span data-ttu-id="df946-279">Jednoduchý způsob toolimit, vrácená data jenom tooauthorized uživatelé je například přidávání tootables sloupec ID uživatele a filtrování výsledků dotazu hello podle ID uživatele hello.</span><span class="sxs-lookup"><span data-stu-id="df946-279">For example, adding a userId column tootables and filtering hello query results by hello user ID is a simple way toolimit returned data only tooauthorized users.</span></span> <span data-ttu-id="df946-280">Hello následující kód vrátí řádky dat pouze v případě hello SID odpovídá hodnotě v hello UserId sloupce v tabulce TodoItem hello:</span><span class="sxs-lookup"><span data-stu-id="df946-280">hello following code returns data rows only when hello SID matches the value in hello UserId column on hello TodoItem table:</span></span>

    // Get hello SID of hello current user.
    var claimsPrincipal = this.User as ClaimsPrincipal;
    string sid = claimsPrincipal.FindFirst(ClaimTypes.NameIdentifier).Value;

    // Only return data rows that belong toohello current user.
    return Query().Where(t => t.UserId == sid);

<span data-ttu-id="df946-281">Hello `Query()` metoda vrátí `IQueryable` který smí uživatel manipulovat LINQ toohandle filtrování.</span><span class="sxs-lookup"><span data-stu-id="df946-281">hello `Query()` method returns an `IQueryable` that can be manipulated by LINQ toohandle filtering.</span></span>

## <a name="how-to-add-push-notifications-tooa-server-project"></a><span data-ttu-id="df946-282">Postupy: Přidání nabízených oznámení tooa serverový projekt</span><span class="sxs-lookup"><span data-stu-id="df946-282">How to: Add push notifications tooa server project</span></span>
<span data-ttu-id="df946-283">Přidat nabízená oznámení tooyour serverový projekt rozšířením hello **MobileAppConfiguration** objekt a vytvoření centra oznámení klienta.</span><span class="sxs-lookup"><span data-stu-id="df946-283">Add push notifications tooyour server project by extending hello **MobileAppConfiguration** object and creating a Notification Hubs client.</span></span>

1. <span data-ttu-id="df946-284">V sadě Visual Studio, klikněte pravým tlačítkem na hello serverový projekt a klikněte na tlačítko **spravovat balíčky NuGet**, vyhledejte `Microsoft.Azure.Mobile.Server.Notifications`, pak klikněte na tlačítko **nainstalovat**.</span><span class="sxs-lookup"><span data-stu-id="df946-284">In Visual Studio, right-click hello server project and click **Manage NuGet Packages**, search for `Microsoft.Azure.Mobile.Server.Notifications`, then click **Install**.</span></span>
2. <span data-ttu-id="df946-285">Opakujte tento krok tooinstall hello `Microsoft.Azure.NotificationHubs` balíček, která zahrnuje klientské knihovny pro hello centra oznámení.</span><span class="sxs-lookup"><span data-stu-id="df946-285">Repeat this step tooinstall hello `Microsoft.Azure.NotificationHubs` package, which includes hello Notification Hubs client library.</span></span>
3. <span data-ttu-id="df946-286">V App_Start/Startup.MobileApp.cs a přidejte volání toohello **AddPushNotifications()** metoda rozšíření během inicializace:</span><span class="sxs-lookup"><span data-stu-id="df946-286">In App_Start/Startup.MobileApp.cs, and add a call toohello **AddPushNotifications()** extension method during initialization:</span></span>

        new MobileAppConfiguration()
            // other features...
            .AddPushNotifications()
            .ApplyTo(config);
4. <span data-ttu-id="df946-287">Přidejte následující kód, který vytvoří klienta Notification Hubs hello:</span><span class="sxs-lookup"><span data-stu-id="df946-287">Add hello following code that creates a Notification Hubs client:</span></span>

        // Get hello settings for hello server project.
        HttpConfiguration config = this.Configuration;
        MobileAppSettingsDictionary settings =
            config.GetMobileAppSettingsProvider().GetMobileAppSettings();

        // Get hello Notification Hubs credentials for hello Mobile App.
        string notificationHubName = settings.NotificationHubName;
        string notificationHubConnection = settings
            .Connections[MobileAppSettingsKeys.NotificationHubConnectionString].ConnectionString;

        // Create a new Notification Hub client.
        NotificationHubClient hub = NotificationHubClient
            .CreateClientFromConnectionString(notificationHubConnection, notificationHubName);

<span data-ttu-id="df946-288">Teď můžete použít hello centra oznámení klienta toosend nabízená oznámení tooregistered zařízení.</span><span class="sxs-lookup"><span data-stu-id="df946-288">You can now use hello Notification Hubs client toosend push notifications tooregistered devices.</span></span> <span data-ttu-id="df946-289">Další informace najdete v tématu [přidat nabízená oznámení tooyour aplikace](app-service-mobile-ios-get-started-push.md).</span><span class="sxs-lookup"><span data-stu-id="df946-289">For more information, see [Add push notifications tooyour app](app-service-mobile-ios-get-started-push.md).</span></span> <span data-ttu-id="df946-290">toolearn Další informace o Notification Hubs najdete v části [přehledu této služby](../notification-hubs/notification-hubs-push-notification-overview.md).</span><span class="sxs-lookup"><span data-stu-id="df946-290">toolearn more about Notification Hubs, see [Notification Hubs Overview](../notification-hubs/notification-hubs-push-notification-overview.md).</span></span>

## <span data-ttu-id="df946-291"><a name="tags"></a>Postupy: povolení cílové nabízené pomocí značek</span><span class="sxs-lookup"><span data-stu-id="df946-291"><a name="tags"></a>How to: Enable targeted push using Tags</span></span>
<span data-ttu-id="df946-292">Centra oznámení můžete odeslat toospecific registrace cílená oznámení pomocí značek.</span><span class="sxs-lookup"><span data-stu-id="df946-292">Notification Hubs lets you send targeted notifications toospecific registrations by using tags.</span></span> <span data-ttu-id="df946-293">Automaticky vytvoří několik značky:</span><span class="sxs-lookup"><span data-stu-id="df946-293">Several tags are created automatically:</span></span>

* <span data-ttu-id="df946-294">Hello ID instalace identifikuje konkrétní zařízení.</span><span class="sxs-lookup"><span data-stu-id="df946-294">hello Installation ID identifies a specific device.</span></span>
* <span data-ttu-id="df946-295">Hello Id uživatele na základě hello ověřený SID identifikuje konkrétního uživatele.</span><span class="sxs-lookup"><span data-stu-id="df946-295">hello User Id based on hello authenticated SID identifies a specific user.</span></span>

<span data-ttu-id="df946-296">Hello instalace ID je přístupná z hello **installationId** vlastnost hello **MobileServiceClient**.</span><span class="sxs-lookup"><span data-stu-id="df946-296">hello installation ID can be accessed from hello **installationId** property on hello **MobileServiceClient**.</span></span>  <span data-ttu-id="df946-297">Hello následující příklad ukazuje, jak používat tooadd ID instalace instalaci specifické tooa značky v Notification Hubs:</span><span class="sxs-lookup"><span data-stu-id="df946-297">hello following example shows how to use an installation ID tooadd a tag tooa specific installation in Notification Hubs:</span></span>

    hub.PatchInstallation("my-installation-id", new[]
    {
        new PartialUpdateOperation
        {
            Operation = UpdateOperationType.Add,
            Path = "/tags",
            Value = "{my-tag}"
        }
    });

<span data-ttu-id="df946-298">Všechny značky poskytl hello klienta během registrace nabízených oznámení se ignorují hello back-end, při vytváření hello instalace.</span><span class="sxs-lookup"><span data-stu-id="df946-298">Any tags supplied by hello client during push notification registration are ignored by hello backend when creating hello installation.</span></span> <span data-ttu-id="df946-299">tooenable tooadd klienta značky toohello instalace, musíte vytvořit vlastní rozhraní API, které přidá značky pomocí hello předcházející vzor.</span><span class="sxs-lookup"><span data-stu-id="df946-299">tooenable a client tooadd tags toohello installation, you must create a custom API that adds tags using hello preceding pattern.</span></span>

<span data-ttu-id="df946-300">V tématu [klienta přidat nabízená oznámení značky] [ 5] v ukázce dokončené rychlý start App Service Mobile Apps hello příklad.</span><span class="sxs-lookup"><span data-stu-id="df946-300">See [Client-added push notification tags][5] in hello App Service Mobile Apps completed quickstart sample for an example.</span></span>

## <span data-ttu-id="df946-301"><a name="push-user"></a>Postupy: odeslání nabízených oznámení tooan ověřeného uživatele</span><span class="sxs-lookup"><span data-stu-id="df946-301"><a name="push-user"></a>How to: Send push notifications tooan authenticated user</span></span>
<span data-ttu-id="df946-302">Pokud ověřený uživatel zaregistruje pro nabízená oznámení, značku ID uživatele se automaticky přidá toohello registrace.</span><span class="sxs-lookup"><span data-stu-id="df946-302">When an authenticated user registers for push notifications, a user ID tag is automatically added toohello registration.</span></span> <span data-ttu-id="df946-303">Pomocí této značky může odesílat nabízená oznámení tooall zařízení registrovaná touto osobou.</span><span class="sxs-lookup"><span data-stu-id="df946-303">By using this tag, you can send push notifications tooall devices registered by that person.</span></span> <span data-ttu-id="df946-304">Hello následující kód získá hello identifikátor SID uživatele, který vytvořil žádost a odešle registrace zařízení k tooevery šablony nabízených oznámení pro tuto osobu:</span><span class="sxs-lookup"><span data-stu-id="df946-304">hello following code gets hello SID of user making the request and sends a template push notification tooevery device registration for that person:</span></span>

    // Get hello current user SID and create a tag for hello current user.
    var claimsPrincipal = this.User as ClaimsPrincipal;
    string sid = claimsPrincipal.FindFirst(ClaimTypes.NameIdentifier).Value;
    string userTag = "_UserId:" + sid;

    // Build a dictionary for hello template with hello item message text.
    var notification = new Dictionary<string, string> { { "message", item.Text } };

    // Send a template notification toohello user ID.
    await hub.SendTemplateNotificationAsync(notification, userTag);

<span data-ttu-id="df946-305">Při registraci pro nabízená oznámení z ověřený klient, ujistěte se, že před pokusem o registraci dokončením ověřování.</span><span class="sxs-lookup"><span data-stu-id="df946-305">When registering for push notifications from an authenticated client, make sure that authentication is complete before attempting registration.</span></span> <span data-ttu-id="df946-306">Další informace najdete v tématu [Push toousers] [ 6] v ukázce dokončené rychlý start hello App Service Mobile Apps pro rozhraní .NET back-end.</span><span class="sxs-lookup"><span data-stu-id="df946-306">For more information, see [Push toousers][6] in hello App Service Mobile Apps completed quickstart sample for .NET backend.</span></span>

## <a name="how-to-debug-and-troubleshoot-hello-net-server-sdk"></a><span data-ttu-id="df946-307">Postupy: ladění a odstraňování potíží hello sady SDK pro .NET serveru</span><span class="sxs-lookup"><span data-stu-id="df946-307">How to: Debug and troubleshoot hello .NET Server SDK</span></span>
<span data-ttu-id="df946-308">Azure App Service poskytuje několik ladění a řešení potíží s techniky pro aplikace ASP.NET:</span><span class="sxs-lookup"><span data-stu-id="df946-308">Azure App Service provides several debugging and troubleshooting techniques for ASP.NET applications:</span></span>

* [<span data-ttu-id="df946-309">Monitorování služby Azure App Service</span><span class="sxs-lookup"><span data-stu-id="df946-309">Monitoring an Azure App Service</span></span>](../app-service-web/web-sites-monitor.md)
* [<span data-ttu-id="df946-310">Povolit protokolování diagnostiky v Azure App Service</span><span class="sxs-lookup"><span data-stu-id="df946-310">Enable Diagnostic Logging in Azure App Service</span></span>](../app-service-web/web-sites-enable-diagnostic-log.md)
* [<span data-ttu-id="df946-311">Řešení potíží s Azure App Service v sadě Visual Studio</span><span class="sxs-lookup"><span data-stu-id="df946-311">Troubleshoot an Azure App Service in Visual Studio</span></span>](../app-service-web/web-sites-dotnet-troubleshoot-visual-studio.md)

### <a name="logging"></a><span data-ttu-id="df946-312">Protokolování</span><span class="sxs-lookup"><span data-stu-id="df946-312">Logging</span></span>
<span data-ttu-id="df946-313">Diagnostické protokoly služby tooApp můžete zapsat pomocí hello zápis trasování standardní technologie ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="df946-313">You can write tooApp Service diagnostic logs by using hello standard ASP.NET trace writing.</span></span> <span data-ttu-id="df946-314">Než napíšete toohello protokoly, je nutné povolit diagnostiky v váš back-end mobilní aplikace.</span><span class="sxs-lookup"><span data-stu-id="df946-314">Before you can write toohello logs, you must enable diagnostics in your Mobile App backend.</span></span>

<span data-ttu-id="df946-315">tooenable zápisu a Diagnostika toohello protokoly:</span><span class="sxs-lookup"><span data-stu-id="df946-315">tooenable diagnostics and write toohello logs:</span></span>

1. <span data-ttu-id="df946-316">Postupujte podle kroků hello v [jak Diagnostika tooenable](../app-service-web/web-sites-enable-diagnostic-log.md#enablediag).</span><span class="sxs-lookup"><span data-stu-id="df946-316">Follow hello steps in [How tooenable diagnostics](../app-service-web/web-sites-enable-diagnostic-log.md#enablediag).</span></span>
2. <span data-ttu-id="df946-317">Přidejte následující hello pomocí příkazu v souboru kódu:</span><span class="sxs-lookup"><span data-stu-id="df946-317">Add hello following using statement in your code file:</span></span>

        using System.Web.Http.Tracing;
3. <span data-ttu-id="df946-318">Vytvoření toowrite zapisovač trasování ze hello .NET back-end toohello diagnostické protokoly, následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="df946-318">Create a trace writer toowrite from hello .NET backend toohello diagnostic logs, as follows:</span></span>

        ITraceWriter traceWriter = this.Configuration.Services.GetTraceWriter();
        traceWriter.Info("Hello, World");
4. <span data-ttu-id="df946-319">Znovu publikovat serverový projekt a přístup k hello mobilní aplikace back-end tooexecute hello kód cestě s protokolováním hello.</span><span class="sxs-lookup"><span data-stu-id="df946-319">Republish your server project, and access hello Mobile App backend tooexecute hello code path with hello logging.</span></span>
5. <span data-ttu-id="df946-320">Stáhněte si a vyhodnocovat u nich hello protokoly, jak je popsáno v [postupy: stažení protokolů](../app-service-web/web-sites-enable-diagnostic-log.md#download).</span><span class="sxs-lookup"><span data-stu-id="df946-320">Download and evaluate hello logs, as described in [How to: Download logs](../app-service-web/web-sites-enable-diagnostic-log.md#download).</span></span>

### <span data-ttu-id="df946-321"><a name="local-debug"></a>Místní ladění pomocí ověřování</span><span class="sxs-lookup"><span data-stu-id="df946-321"><a name="local-debug"></a>Local debugging with authentication</span></span>
<span data-ttu-id="df946-322">Aplikaci můžete spustit místně tootest změní před publikováním toohello cloudu.</span><span class="sxs-lookup"><span data-stu-id="df946-322">You can run your application locally tootest changes before publishing them toohello cloud.</span></span> <span data-ttu-id="df946-323">Většina Azure Mobile Apps back-EndY, stiskněte klávesu *F5* při v sadě Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="df946-323">For most Azure Mobile Apps backends, press *F5* while in Visual Studio.</span></span> <span data-ttu-id="df946-324">Existují však některé další aspekty při použití ověřování.</span><span class="sxs-lookup"><span data-stu-id="df946-324">However, there are some additional considerations when using authentication.</span></span>

<span data-ttu-id="df946-325">Musí mít mobilní aplikaci s ověřování/autorizace služby App Service nakonfigurované založené na cloudu a vašeho klienta musí mít zadaný jako hostitele alternativního přihlašovacího hello hello koncového bodu cloudu.</span><span class="sxs-lookup"><span data-stu-id="df946-325">You must have a cloud-based mobile app with App Service Authentication/Authorization configured, and your client must have hello cloud endpoint specified as hello alternate login host.</span></span> <span data-ttu-id="df946-326">Vaše klientská platforma hello konkrétní kroky potřebné naleznete v dokumentaci hello.</span><span class="sxs-lookup"><span data-stu-id="df946-326">See hello documentation for your client platform for hello specific steps required.</span></span>

<span data-ttu-id="df946-327">Zajistěte, aby mobilního back-endu [Microsoft.Azure.Mobile.Server.Authentication] nainstalována.</span><span class="sxs-lookup"><span data-stu-id="df946-327">Ensure that your mobile backend has [Microsoft.Azure.Mobile.Server.Authentication] installed.</span></span> <span data-ttu-id="df946-328">Potom v třídy pro spuštění OWIN vaší aplikace, přidat následující hello po `MobileAppConfiguration` bylo použité tooyour `HttpConfiguration`:</span><span class="sxs-lookup"><span data-stu-id="df946-328">Then, in your application's OWIN startup class, add hello following, after `MobileAppConfiguration` has been applied tooyour `HttpConfiguration`:</span></span>

        app.UseAppServiceAuthentication(new AppServiceAuthenticationOptions()
        {
            SigningKey = ConfigurationManager.AppSettings["authSigningKey"],
            ValidAudiences = new[] { ConfigurationManager.AppSettings["authAudience"] },
            ValidIssuers = new[] { ConfigurationManager.AppSettings["authIssuer"] },
            TokenHandler = config.GetAppServiceTokenHandler()
        });

<span data-ttu-id="df946-329">V předchozím příkladu hello, měli byste nakonfigurovat hello *authAudience* a *authIssuer* nastavení aplikace v souboru Web.config souboru tooeach být adresa URL kořenového adresáře aplikace pomocí hello HTTPS schéma.</span><span class="sxs-lookup"><span data-stu-id="df946-329">In hello preceding example, you should configure hello *authAudience* and *authIssuer* application settings within your Web.config file tooeach be the URL of your application root, using hello HTTPS scheme.</span></span> <span data-ttu-id="df946-330">Podobně byste měli nastavit *authSigningKey* toobe hello hodnotu vaší aplikace je podpisový klíč.</span><span class="sxs-lookup"><span data-stu-id="df946-330">Similarly you should set *authSigningKey* toobe hello value of your application's signing key.</span></span>
<span data-ttu-id="df946-331">tooobtain hello podpisový klíč:</span><span class="sxs-lookup"><span data-stu-id="df946-331">tooobtain hello signing key:</span></span>

1. <span data-ttu-id="df946-332">Přejděte tooyour aplikace v rámci hello [portál Azure]</span><span class="sxs-lookup"><span data-stu-id="df946-332">Navigate tooyour app within hello [Azure portal]</span></span>
2. <span data-ttu-id="df946-333">Klikněte na tlačítko **nástroje**, **Kudu**, **přejděte**.</span><span class="sxs-lookup"><span data-stu-id="df946-333">Click **Tools**, **Kudu**, **Go**.</span></span>
3. <span data-ttu-id="df946-334">V lokalitě hello Kudu správy, klikněte na tlačítko **prostředí**.</span><span class="sxs-lookup"><span data-stu-id="df946-334">In hello Kudu Management site, click **Environment**.</span></span>
4. <span data-ttu-id="df946-335">Najít hodnotu hello *webu\_AUTH\_PODPISOVÁNÍ\_klíč*.</span><span class="sxs-lookup"><span data-stu-id="df946-335">Find hello value for *WEBSITE\_AUTH\_SIGNING\_KEY*.</span></span>

<span data-ttu-id="df946-336">Použití hello podpisového klíče pro hello *authSigningKey* parametr v konfiguraci vaší místní aplikace.  Mobilního back-endu je nyní vybavené toovalidate tokeny při spuštění místně, které hello klient obdrží hello tokenu z koncového bodu cloudu hello.</span><span class="sxs-lookup"><span data-stu-id="df946-336">Use hello signing key for hello *authSigningKey* parameter in your local application config.  Your mobile backend is now equipped toovalidate tokens when running locally, which hello client obtains hello token from hello cloud-based endpoint.</span></span>

[1]: https://msdn.microsoft.com/library/azure/dn961176.aspx
[2]: https://github.com/Azure/azure-mobile-apps-net-server
[3]: app-service-mobile-ios-get-started.md
[4]: https://azure.microsoft.com/downloads/
[5]: https://github.com/Azure-Samples/app-service-mobile-dotnet-backend-quickstart/blob/master/README.md#client-added-push-notification-tags
[6]: https://github.com/Azure-Samples/app-service-mobile-dotnet-backend-quickstart/blob/master/README.md#push-to-users
[portál Azure]: https://portal.azure.com
[NuGet.org]: http://www.nuget.org/
[Microsoft.Azure.Mobile.Server]: http://www.nuget.org/packages/Microsoft.Azure.Mobile.Server/
[Microsoft.Azure.Mobile.Server.Quickstart]: http://www.nuget.org/packages/Microsoft.Azure.Mobile.Server.Quickstart/
[Microsoft.Azure.Mobile.Server.Authentication]: http://www.nuget.org/packages/Microsoft.Azure.Mobile.Server.Authentication/
[Microsoft.Azure.Mobile.Server.Login]: http://www.nuget.org/packages/Microsoft.Azure.Mobile.Server.Login/
[Microsoft.Azure.Mobile.Server.Notifications]: http://www.nuget.org/packages/Microsoft.Azure.Mobile.Server.Notifications/
[MapHttpAttributeRoutes]: https://msdn.microsoft.com/library/dn479134(v=vs.118).aspx
