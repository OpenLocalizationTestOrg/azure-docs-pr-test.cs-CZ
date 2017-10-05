---
title: Jak pracovat s .NET back-end serveru SDK pro Mobile Apps | Microsoft Docs
description: "Naučte se pracovat s .NET back-end serveru SDK pro Azure App Service Mobile Apps."
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
ms.openlocfilehash: 657fea16e47c15efd262c86d6a150a721476134a
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="work-with-the-net-backend-server-sdk-for-azure-mobile-apps"></a><span data-ttu-id="de6a1-104">Práce se serverovou sadou .NET back-end SDK v prostředí Azure Mobile Apps</span><span class="sxs-lookup"><span data-stu-id="de6a1-104">Work with the .NET backend server SDK for Azure Mobile Apps</span></span>
[!INCLUDE [app-service-mobile-selector-server-sdk](../../includes/app-service-mobile-selector-server-sdk.md)]

<span data-ttu-id="de6a1-105">Toto téma ukazuje, jak používat rozhraní .NET back-end serveru SDK v klíčové scénáře Azure App Service Mobile Apps.</span><span class="sxs-lookup"><span data-stu-id="de6a1-105">This topic shows you how to use the .NET backend server SDK in key Azure App Service Mobile Apps scenarios.</span></span> <span data-ttu-id="de6a1-106">Azure Mobile Apps SDK umožňuje pracovat s mobilních klientů z vaší aplikace ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="de6a1-106">The Azure Mobile Apps SDK helps you work with mobile clients from your ASP.NET application.</span></span>

> [!TIP]
> <span data-ttu-id="de6a1-107">[Server .NET SDK pro Azure Mobile Apps] [ 2] je open source na Githubu.</span><span class="sxs-lookup"><span data-stu-id="de6a1-107">The [.NET server SDK for Azure Mobile Apps][2] is open source on GitHub.</span></span> <span data-ttu-id="de6a1-108">Úložiště obsahuje všechny zdrojový kód, včetně celý server SDK jednotky testovací sady a některé ukázkové projekty.</span><span class="sxs-lookup"><span data-stu-id="de6a1-108">The repository contains all source code including the entire server SDK unit test suite and some sample projects.</span></span>
>
>

## <a name="reference-documentation"></a><span data-ttu-id="de6a1-109">Referenční dokumentace</span><span class="sxs-lookup"><span data-stu-id="de6a1-109">Reference documentation</span></span>
<span data-ttu-id="de6a1-110">Referenční dokumentaci k nástroji pro server SDK se nachází zde: [Azure Mobile Apps .NET – referenční informace][1].</span><span class="sxs-lookup"><span data-stu-id="de6a1-110">The reference documentation for the server SDK is located here: [Azure Mobile Apps .NET Reference][1].</span></span>

## <span data-ttu-id="de6a1-111"><a name="create-app"></a>Postupy: vytvoření back-endu mobilní aplikace .NET</span><span class="sxs-lookup"><span data-stu-id="de6a1-111"><a name="create-app"></a>How to: Create a .NET Mobile App backend</span></span>
<span data-ttu-id="de6a1-112">Pokud spouštíte nový projekt, můžete vytvořit buď pomocí aplikace služby App Service [portál Azure] nebo Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="de6a1-112">If you are starting a new project, you can create an App Service application using either the [Azure portal] or Visual Studio.</span></span> <span data-ttu-id="de6a1-113">Můžete spustit aplikace služby App Service místně nebo publikování tohoto projektu cloudové mobilní aplikace služby App Service.</span><span class="sxs-lookup"><span data-stu-id="de6a1-113">You can run the App Service application locally or publish the project to your cloud-based App Service mobile app.</span></span>

<span data-ttu-id="de6a1-114">Pokud přidáváte mobilní funkce do existujícího projektu, najdete v článku [stáhnout a inicializujte sadu SDK](#install-sdk) části.</span><span class="sxs-lookup"><span data-stu-id="de6a1-114">If you are adding mobile capabilities to an existing project, see the [Download and initialize the SDK](#install-sdk) section.</span></span>

### <a name="create-a-net-backend-using-the-azure-portal"></a><span data-ttu-id="de6a1-115">Vytvořit rozhraní .NET back-end pomocí portálu Azure</span><span class="sxs-lookup"><span data-stu-id="de6a1-115">Create a .NET backend using the Azure portal</span></span>
<span data-ttu-id="de6a1-116">K vytvoření App Service mobilního back-endu, buď použijte [rychlý úvodní kurz] [ 3] nebo postupujte podle těchto kroků:</span><span class="sxs-lookup"><span data-stu-id="de6a1-116">To create an App Service mobile backend, either follow the [Quickstart tutorial][3] or follow these steps:</span></span>

[!INCLUDE [app-service-mobile-dotnet-backend-create-new-service-classic](../../includes/app-service-mobile-dotnet-backend-create-new-service-classic.md)]

<span data-ttu-id="de6a1-117">Zpět v *Začínáme* okno, v části **vytvořit tabulku rozhraní API**, zvolte **C#** jako vaše **back-end jazyk**.</span><span class="sxs-lookup"><span data-stu-id="de6a1-117">Back in the *Get started* blade, under **Create a table API**, choose **C#** as your **Backend language**.</span></span> <span data-ttu-id="de6a1-118">Klikněte na tlačítko **Stáhnout**, extrahujte komprimované soubory projektu do místního počítače a otevřete řešení v sadě Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="de6a1-118">Click **Download**, extract the compressed project files to your local computer, and open the solution in Visual Studio.</span></span>

### <a name="create-a-net-backend-using-visual-studio-2013-and-visual-studio-2015"></a><span data-ttu-id="de6a1-119">Vytvořit rozhraní .NET back-end pomocí Visual Studio 2013 a Visual Studio 2015</span><span class="sxs-lookup"><span data-stu-id="de6a1-119">Create a .NET backend using Visual Studio 2013 and Visual Studio 2015</span></span>
<span data-ttu-id="de6a1-120">Nainstalujte [Azure SDK for .NET] [ 4] (verze 2.9.0 nebo novější) k vytvoření projektu Azure Mobile Apps v sadě Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="de6a1-120">Install the [Azure SDK for .NET][4] (version 2.9.0 or later) to create an Azure Mobile Apps project in Visual Studio.</span></span> <span data-ttu-id="de6a1-121">Po dokončení instalace sady SDK, vytvořte aplikaci ASP.NET pomocí následujících kroků:</span><span class="sxs-lookup"><span data-stu-id="de6a1-121">Once you have installed the SDK, create an ASP.NET application using the following steps:</span></span>

1. <span data-ttu-id="de6a1-122">Otevřete **nový projekt** dialogové okno (z *soubor* > **nový** > **projekt...** ).</span><span class="sxs-lookup"><span data-stu-id="de6a1-122">Open the **New Project** dialog (from *File* > **New** > **Project...**).</span></span>
2. <span data-ttu-id="de6a1-123">Rozbalte položku **šablony** > **Visual C#**a vyberte **webové**.</span><span class="sxs-lookup"><span data-stu-id="de6a1-123">Expand **Templates** > **Visual C#**, and select **Web**.</span></span>
3. <span data-ttu-id="de6a1-124">Vyberte **webové aplikace ASP.NET**.</span><span class="sxs-lookup"><span data-stu-id="de6a1-124">Select **ASP.NET Web Application**.</span></span>
4. <span data-ttu-id="de6a1-125">Zadejte název projektu.</span><span class="sxs-lookup"><span data-stu-id="de6a1-125">Fill in the project name.</span></span> <span data-ttu-id="de6a1-126">Pak klikněte na **OK**.</span><span class="sxs-lookup"><span data-stu-id="de6a1-126">Then click **OK**.</span></span>
5. <span data-ttu-id="de6a1-127">V části *ASP.NET 4.5.2 šablony*, vyberte **mobilní aplikace Azure**.</span><span class="sxs-lookup"><span data-stu-id="de6a1-127">Under *ASP.NET 4.5.2 Templates*, select **Azure Mobile App**.</span></span> <span data-ttu-id="de6a1-128">Zkontrolujte **hostitel v cloudu** vytvoření mobilního back-end v cloudu, do které můžete publikovat tento projekt.</span><span class="sxs-lookup"><span data-stu-id="de6a1-128">Check **Host in the cloud** to create a mobile backend in the cloud to which you can publish this project.</span></span>
6. <span data-ttu-id="de6a1-129">Klikněte na **OK**.</span><span class="sxs-lookup"><span data-stu-id="de6a1-129">Click **OK**.</span></span>

## <span data-ttu-id="de6a1-130"><a name="install-sdk"></a>Postupy: stažení a inicializujte sadu SDK</span><span class="sxs-lookup"><span data-stu-id="de6a1-130"><a name="install-sdk"></a>How to: Download and initialize the SDK</span></span>
<span data-ttu-id="de6a1-131">Sada SDK je k dispozici na [NuGet.org].</span><span class="sxs-lookup"><span data-stu-id="de6a1-131">The SDK is available on [NuGet.org].</span></span> <span data-ttu-id="de6a1-132">Tento balíček obsahuje základní funkce vyžaduje, abyste mohli začít používat sadu SDK.</span><span class="sxs-lookup"><span data-stu-id="de6a1-132">This package includes the base functionality required to get started using the SDK.</span></span> <span data-ttu-id="de6a1-133">K inicializaci sady SDK, je nutné k provedení akce na **HttpConfiguration** objektu.</span><span class="sxs-lookup"><span data-stu-id="de6a1-133">To initialize the SDK, you need to perform actions on the **HttpConfiguration** object.</span></span>

### <a name="install-the-sdk"></a><span data-ttu-id="de6a1-134">Instalace sady SDK</span><span class="sxs-lookup"><span data-stu-id="de6a1-134">Install the SDK</span></span>
<span data-ttu-id="de6a1-135">Chcete-li nainstalovat sadu SDK, klikněte pravým tlačítkem na serverový projekt v sadě Visual Studio, vyberte **spravovat balíčky NuGet**, vyhledejte [Microsoft.Azure.Mobile.Server] balíček a potom klikněte na **instalace** .</span><span class="sxs-lookup"><span data-stu-id="de6a1-135">To install the SDK, right-click on the server project in Visual Studio, select **Manage NuGet Packages**, search for the [Microsoft.Azure.Mobile.Server] package, then click **Install**.</span></span>

### <span data-ttu-id="de6a1-136"><a name="server-project-setup"></a>Inicializace serverový projekt</span><span class="sxs-lookup"><span data-stu-id="de6a1-136"><a name="server-project-setup"></a> Initialize the server project</span></span>
<span data-ttu-id="de6a1-137">Projekt .NET back-end server je inicializován podobně jako ostatní projekty ASP.NET, včetně třídy pro spuštění OWIN.</span><span class="sxs-lookup"><span data-stu-id="de6a1-137">A .NET backend server project is initialized similar to other ASP.NET projects, by including an OWIN startup class.</span></span> <span data-ttu-id="de6a1-138">Ujistěte se, že máte odkazuje balíček NuGet `Microsoft.Owin.Host.SystemWeb`.</span><span class="sxs-lookup"><span data-stu-id="de6a1-138">Ensure that you have referenced the NuGet package `Microsoft.Owin.Host.SystemWeb`.</span></span> <span data-ttu-id="de6a1-139">Postup přidání této třídy v sadě Visual Studio, klikněte pravým tlačítkem na serverový projekt a vyberte **přidat** >
**nová položka**, pak **webové**  >  ** Obecné** > **třídy pro spuštění OWIN**.</span><span class="sxs-lookup"><span data-stu-id="de6a1-139">To add this class in Visual Studio, right-click on your server project and select **Add** >
**New Item**, then **Web** > **General** > **OWIN Startup class**.</span></span>  <span data-ttu-id="de6a1-140">Třída je generována následující atribut:</span><span class="sxs-lookup"><span data-stu-id="de6a1-140">A class is generated with the following attribute:</span></span>

    [assembly: OwinStartup(typeof(YourServiceName.YourStartupClassName))]

<span data-ttu-id="de6a1-141">V `Configuration()` metoda vaší třídy pro spuštění OWIN, použijte **HttpConfiguration** objekt konfigurace prostředí Azure Mobile Apps.</span><span class="sxs-lookup"><span data-stu-id="de6a1-141">In the `Configuration()` method of your OWIN startup class, use an **HttpConfiguration** object to configure the Azure Mobile Apps environment.</span></span>
<span data-ttu-id="de6a1-142">Následující příklad inicializuje serverový projekt s žádné nové funkce:</span><span class="sxs-lookup"><span data-stu-id="de6a1-142">The following example initializes the server project with no added features:</span></span>

    // in OWIN startup class
    public void Configuration(IAppBuilder app)
    {
        HttpConfiguration config = new HttpConfiguration();

        new MobileAppConfiguration()
            // no added features
            .ApplyTo(config);

        app.UseWebApi(config);
    }

<span data-ttu-id="de6a1-143">Povolit jednotlivé funkce, musí volat metody rozšíření pro **MobileAppConfiguration** objekt před voláním **metodu**.</span><span class="sxs-lookup"><span data-stu-id="de6a1-143">To enable individual features, you must call extension methods on the **MobileAppConfiguration** object before calling **ApplyTo**.</span></span> <span data-ttu-id="de6a1-144">Například následující kód přidá výchozí trasy na všechny řadiče rozhraní API, které mají atribut `[MobileAppController]` během inicializace:</span><span class="sxs-lookup"><span data-stu-id="de6a1-144">For example, the following code adds the default routes to all API controllers that have the attribute `[MobileAppController]` during initialization:</span></span>

    new MobileAppConfiguration()
        .MapApiControllers()
        .ApplyTo(config);

<span data-ttu-id="de6a1-145">Rychlé spuštění serveru z portálu Azure volání **UseDefaultConfiguration()**.</span><span class="sxs-lookup"><span data-stu-id="de6a1-145">The server quickstart from the Azure portal calls **UseDefaultConfiguration()**.</span></span> <span data-ttu-id="de6a1-146">Tento ekvivalent následující nastavení:</span><span class="sxs-lookup"><span data-stu-id="de6a1-146">This equivalent to the following setup:</span></span>

        new MobileAppConfiguration()
            .AddMobileAppHomeController()             // from the Home package
            .MapApiControllers()
            .AddTables(                               // from the Tables package
                new MobileAppTableConfiguration()
                    .MapTableControllers()
                    .AddEntityFramework()             // from the Entity package
                )
            .AddPushNotifications()                   // from the Notifications package
            .MapLegacyCrossDomainController()         // from the CrossDomain package
            .ApplyTo(config);

<span data-ttu-id="de6a1-147">Rozšiřující metody používané jsou:</span><span class="sxs-lookup"><span data-stu-id="de6a1-147">The extension methods used are:</span></span>

* <span data-ttu-id="de6a1-148">`AddMobileAppHomeController()`poskytuje výchozí Azure Mobile Apps domovskou stránku.</span><span class="sxs-lookup"><span data-stu-id="de6a1-148">`AddMobileAppHomeController()` provides the default Azure Mobile Apps home page.</span></span>
* <span data-ttu-id="de6a1-149">`MapApiControllers()`poskytuje možnosti vlastního rozhraní API pro WebAPI řadiče označených pomocí `[MobileAppController]` atribut.</span><span class="sxs-lookup"><span data-stu-id="de6a1-149">`MapApiControllers()` provides custom API capabilities for WebAPI controllers decorated with the `[MobileAppController]` attribute.</span></span>
* <span data-ttu-id="de6a1-150">`AddTables()`poskytuje mapování `/tables` koncové body k tabulce řadiče.</span><span class="sxs-lookup"><span data-stu-id="de6a1-150">`AddTables()` provides a mapping of the `/tables` endpoints to table controllers.</span></span>
* <span data-ttu-id="de6a1-151">`AddTablesWithEntityFramework()`je sdružený pro mapování `/tables` používající rozhraní Entity Framework koncových bodů na základě řadiče.</span><span class="sxs-lookup"><span data-stu-id="de6a1-151">`AddTablesWithEntityFramework()` is a short-hand for mapping the `/tables` endpoints using Entity Framework based controllers.</span></span>
* <span data-ttu-id="de6a1-152">`AddPushNotifications()`poskytuje jednoduchý způsob registrace zařízení pro centra oznámení.</span><span class="sxs-lookup"><span data-stu-id="de6a1-152">`AddPushNotifications()` provides a simple method of registering devices for Notification Hubs.</span></span>
* <span data-ttu-id="de6a1-153">`MapLegacyCrossDomainController()`poskytuje standardní hlavičky CORS pro místní vývoj.</span><span class="sxs-lookup"><span data-stu-id="de6a1-153">`MapLegacyCrossDomainController()` provides standard CORS headers for local development.</span></span>

### <a name="sdk-extensions"></a><span data-ttu-id="de6a1-154">Rozšíření sady SDK</span><span class="sxs-lookup"><span data-stu-id="de6a1-154">SDK extensions</span></span>
<span data-ttu-id="de6a1-155">Následující balíčky NuGet prostřednictvím rozšíření poskytují různé mobilní funkce, které mohou být využívána vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="de6a1-155">The following NuGet-based extension packages provide various mobile features that can be used by your application.</span></span> <span data-ttu-id="de6a1-156">Povolit rozšíření během inicializace s použitím **MobileAppConfiguration** objektu.</span><span class="sxs-lookup"><span data-stu-id="de6a1-156">You enable extensions during initialization by using the **MobileAppConfiguration** object.</span></span>

* <span data-ttu-id="de6a1-157">[Microsoft.Azure.Mobile.Server.Quickstart] podporuje základní nastavení mobilní aplikace.</span><span class="sxs-lookup"><span data-stu-id="de6a1-157">[Microsoft.Azure.Mobile.Server.Quickstart] Supports the basic Mobile Apps setup.</span></span> <span data-ttu-id="de6a1-158">V konfiguraci přidaný voláním **UseDefaultConfiguration** metoda rozšíření během inicializace.</span><span class="sxs-lookup"><span data-stu-id="de6a1-158">Added to the configuration by calling the **UseDefaultConfiguration** extension method during initialization.</span></span> <span data-ttu-id="de6a1-159">Toto rozšíření zahrnuje následující rozšíření: oznámení, ověřování, entita, tabulky, mezi doménami a domovské balíčky.</span><span class="sxs-lookup"><span data-stu-id="de6a1-159">This extension includes following extensions: Notifications, Authentication, Entity, Tables, Cross-domain, and Home packages.</span></span> <span data-ttu-id="de6a1-160">Tento balíček je používán k dispozici na portálu Azure mobilní aplikace pro rychlý start.</span><span class="sxs-lookup"><span data-stu-id="de6a1-160">This package is used by the Mobile Apps Quickstart available on the Azure portal.</span></span>
* <span data-ttu-id="de6a1-161">[Microsoft.Azure.Mobile.Server.Home](http://www.nuget.org/packages/Microsoft.Azure.Mobile.Server.Home/) implementuje výchozí *tuto aplikaci je spuštěný a funkční stránky* pro kořenový adresář webu.</span><span class="sxs-lookup"><span data-stu-id="de6a1-161">[Microsoft.Azure.Mobile.Server.Home](http://www.nuget.org/packages/Microsoft.Azure.Mobile.Server.Home/) Implements the default *this mobile app is up and running page* for the web site root.</span></span> <span data-ttu-id="de6a1-162">Přidejte ke konfiguraci voláním **AddMobileAppHomeController** metoda rozšíření.</span><span class="sxs-lookup"><span data-stu-id="de6a1-162">Add to the configuration by calling the **AddMobileAppHomeController** extension method.</span></span>
* <span data-ttu-id="de6a1-163">[Microsoft.Azure.Mobile.Server.Tables](http://www.nuget.org/packages/Microsoft.Azure.Mobile.Server.Tables/) obsahuje třídy pro práci s daty a nastaví up datovém kanálu.</span><span class="sxs-lookup"><span data-stu-id="de6a1-163">[Microsoft.Azure.Mobile.Server.Tables](http://www.nuget.org/packages/Microsoft.Azure.Mobile.Server.Tables/) includes classes for working with data and sets-up the data pipeline.</span></span> <span data-ttu-id="de6a1-164">Přidejte ke konfiguraci voláním **AddTables** metoda rozšíření.</span><span class="sxs-lookup"><span data-stu-id="de6a1-164">Add to the configuration by calling the **AddTables** extension method.</span></span>
* <span data-ttu-id="de6a1-165">[Microsoft.Azure.Mobile.Server.Entity](http://www.nuget.org/packages/Microsoft.Azure.Mobile.Server.Entity/) umožňuje Entity Framework pro přístup k datům v databázi SQL.</span><span class="sxs-lookup"><span data-stu-id="de6a1-165">[Microsoft.Azure.Mobile.Server.Entity](http://www.nuget.org/packages/Microsoft.Azure.Mobile.Server.Entity/) Enables the Entity Framework to access data in the SQL Database.</span></span> <span data-ttu-id="de6a1-166">Přidejte ke konfiguraci voláním **AddTablesWithEntityFramework** metoda rozšíření.</span><span class="sxs-lookup"><span data-stu-id="de6a1-166">Add to the configuration by calling the **AddTablesWithEntityFramework** extension method.</span></span>
* <span data-ttu-id="de6a1-167">[Microsoft.Azure.Mobile.Server.Authentication] umožňuje ověřování a nastaví up middlewaru OWIN, který slouží k ověření tokenů.</span><span class="sxs-lookup"><span data-stu-id="de6a1-167">[Microsoft.Azure.Mobile.Server.Authentication] Enables authentication and sets-up the OWIN middleware used to validate tokens.</span></span> <span data-ttu-id="de6a1-168">Přidejte ke konfiguraci voláním **AddAppServiceAuthentication** a **IAppBuilder**. **UseAppServiceAuthentication** rozšiřující metody.</span><span class="sxs-lookup"><span data-stu-id="de6a1-168">Add to the configuration by calling the **AddAppServiceAuthentication** and **IAppBuilder**.**UseAppServiceAuthentication** extension methods.</span></span>
* <span data-ttu-id="de6a1-169">[Microsoft.Azure.Mobile.Server.Notifications] povoluje nabízená oznámení a definuje koncový bod nabízené registrace.</span><span class="sxs-lookup"><span data-stu-id="de6a1-169">[Microsoft.Azure.Mobile.Server.Notifications] Enables push notifications and defines a push registration endpoint.</span></span> <span data-ttu-id="de6a1-170">Přidejte ke konfiguraci voláním **AddPushNotifications** metoda rozšíření.</span><span class="sxs-lookup"><span data-stu-id="de6a1-170">Add to the configuration by calling the **AddPushNotifications** extension method.</span></span>
* <span data-ttu-id="de6a1-171">[Microsoft.Azure.Mobile.Server.CrossDomain](http://www.nuget.org/packages/Microsoft.Azure.Mobile.Server.CrossDomain/) vytvoří kontroler, který slouží data do starší verze webových prohlížečů z mobilní aplikace.</span><span class="sxs-lookup"><span data-stu-id="de6a1-171">[Microsoft.Azure.Mobile.Server.CrossDomain](http://www.nuget.org/packages/Microsoft.Azure.Mobile.Server.CrossDomain/) Creates a controller that serves data to legacy web browsers from your Mobile App.</span></span> <span data-ttu-id="de6a1-172">Přidejte ke konfiguraci voláním **MapLegacyCrossDomainController** metoda rozšíření.</span><span class="sxs-lookup"><span data-stu-id="de6a1-172">Add to the configuration by calling the **MapLegacyCrossDomainController** extension method.</span></span>
* <span data-ttu-id="de6a1-173">[Microsoft.Azure.Mobile.Server.Login] poskytuje AppServiceLoginHandler.CreateToken() metodu, která se používají při ověřování vlastní scénáře statickou metodu.</span><span class="sxs-lookup"><span data-stu-id="de6a1-173">[Microsoft.Azure.Mobile.Server.Login] Provides the AppServiceLoginHandler.CreateToken() method, which is a static method used during custom authentication scenarios.</span></span>

## <span data-ttu-id="de6a1-174"><a name="publish-server-project"></a>Postupy: publikování projektu serveru</span><span class="sxs-lookup"><span data-stu-id="de6a1-174"><a name="publish-server-project"></a>How to: Publish the server project</span></span>
<span data-ttu-id="de6a1-175">V této části se dozvíte, jak k publikování projektu back-end .NET ze sady Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="de6a1-175">This section shows you how to publish your .NET backend project from Visual Studio.</span></span> <span data-ttu-id="de6a1-176">Můžete taky nasadit projektu back-end pomocí Git nebo libovolné jiné metody popsané v [dokumentaci k nasazení služby Azure App Service](../app-service-web/web-sites-deploy.md).</span><span class="sxs-lookup"><span data-stu-id="de6a1-176">You can also deploy your backend project using Git or any of the other methods covered in the [Azure App Service deployment documentation](../app-service-web/web-sites-deploy.md).</span></span>

1. <span data-ttu-id="de6a1-177">V sadě Visual Studio znovu sestavte projekt pro obnovování balíčků NuGet.</span><span class="sxs-lookup"><span data-stu-id="de6a1-177">In Visual Studio, rebuild the project to restore NuGet packages.</span></span>
2. <span data-ttu-id="de6a1-178">V Průzkumníku řešení klikněte pravým tlačítkem na projekt, klikněte na tlačítko **publikovat**.</span><span class="sxs-lookup"><span data-stu-id="de6a1-178">In Solution Explorer, right-click the project, click **Publish**.</span></span> <span data-ttu-id="de6a1-179">Při prvním publikování, musíte zadat profil publikování.</span><span class="sxs-lookup"><span data-stu-id="de6a1-179">The first time you publish, you need to define a publishing profile.</span></span> <span data-ttu-id="de6a1-180">Pokud již máte profil definované, můžete jej vybrat a klikněte na tlačítko **publikovat**.</span><span class="sxs-lookup"><span data-stu-id="de6a1-180">When you already have a profile defined, you can select it and click **Publish**.</span></span>
3. <span data-ttu-id="de6a1-181">Pokud se zobrazí dotaz, vyberte cíl publikování, klikněte na tlačítko **Microsoft Azure App Service** > **Další**, pak (v případě potřeby) přihlásit se pomocí svých přihlašovacích údajů Azure.</span><span class="sxs-lookup"><span data-stu-id="de6a1-181">If asked to select a publish target, click **Microsoft Azure App Service** > **Next**, then (if needed) sign-in with your Azure credentials.</span></span>
   <span data-ttu-id="de6a1-182">Visual Studio stáhne a bezpečně uloží vaše nastavení publikování přímo z Azure.</span><span class="sxs-lookup"><span data-stu-id="de6a1-182">Visual Studio downloads and securely stores your publish settings directly from Azure.</span></span>

    ![](./media/app-service-mobile-dotnet-backend-how-to-use-server-sdk/publish-wizard-1.png)
4. <span data-ttu-id="de6a1-183">Zvolte vaše **předplatné**, vyberte **typ prostředku** z **zobrazení**, rozbalte položku **mobilní aplikace**a klikněte na váš back-end mobilní aplikace a pak klikněte na **OK**.</span><span class="sxs-lookup"><span data-stu-id="de6a1-183">Choose your **Subscription**, select **Resource Type** from **View**, expand **Mobile App**, and click your Mobile App backend, then click **OK**.</span></span>

    ![](./media/app-service-mobile-dotnet-backend-how-to-use-server-sdk/publish-wizard-2.png)
5. <span data-ttu-id="de6a1-184">Zkontrolujte zadané informace o profilu publikování a klikněte na tlačítko **publikovat**.</span><span class="sxs-lookup"><span data-stu-id="de6a1-184">Verify the publish profile information and click **Publish**.</span></span>

    ![](./media/app-service-mobile-dotnet-backend-how-to-use-server-sdk/publish-wizard-3.png)

    <span data-ttu-id="de6a1-185">Pokud váš back-end mobilní aplikace byla úspěšně publikována, zobrazí se cílová stránka udávající úspěch.</span><span class="sxs-lookup"><span data-stu-id="de6a1-185">When your Mobile App backend has published successfully, you see a landing page indicating success.</span></span>

    ![](./media/app-service-mobile-dotnet-backend-how-to-use-server-sdk/publish-success.png)

## <span data-ttu-id="de6a1-186"><a name="define-table-controller"></a>Postupy: definování řadič tabulky</span><span class="sxs-lookup"><span data-stu-id="de6a1-186"><a name="define-table-controller"></a> How to: Define a table controller</span></span>
<span data-ttu-id="de6a1-187">Zadejte řadič tabulky vystavit tabulku SQL, aby mobilních klientů.</span><span class="sxs-lookup"><span data-stu-id="de6a1-187">Define a Table Controller to expose a SQL table to mobile clients.</span></span>  <span data-ttu-id="de6a1-188">Konfigurace řadič tabulky vyžaduje tři kroky:</span><span class="sxs-lookup"><span data-stu-id="de6a1-188">Configuring a Table Controller requires three steps:</span></span>

1. <span data-ttu-id="de6a1-189">Vytvořte třídu objektu pro přenos dat (DTO).</span><span class="sxs-lookup"><span data-stu-id="de6a1-189">Create a Data Transfer Object (DTO) class.</span></span>
2. <span data-ttu-id="de6a1-190">Nakonfigurujte odkaz na tabulku v třídy Mobile DbContext.</span><span class="sxs-lookup"><span data-stu-id="de6a1-190">Configure a table reference in the Mobile DbContext class.</span></span>
3. <span data-ttu-id="de6a1-191">Vytvořte řadič tabulky.</span><span class="sxs-lookup"><span data-stu-id="de6a1-191">Create a table controller.</span></span>

<span data-ttu-id="de6a1-192">Objekt pro přenos dat (DTO) je prostý C# objekt, který dědí z `EntityData`.</span><span class="sxs-lookup"><span data-stu-id="de6a1-192">A Data Transfer Object (DTO) is a plain C# object that inherits from `EntityData`.</span></span>  <span data-ttu-id="de6a1-193">Například:</span><span class="sxs-lookup"><span data-stu-id="de6a1-193">For example:</span></span>

    public class TodoItem : EntityData
    {
        public string Text { get; set; }
        public bool Complete {get; set;}
    }

<span data-ttu-id="de6a1-194">DTO se používá k definování tabulky v databázi SQL.</span><span class="sxs-lookup"><span data-stu-id="de6a1-194">The DTO is used to define the table within the SQL database.</span></span>  <span data-ttu-id="de6a1-195">Chcete-li vytvořit položku databáze, přidejte `DbSet<>` vlastnost, která má DbContext, kterou používáte.</span><span class="sxs-lookup"><span data-stu-id="de6a1-195">To create the database entry, add a `DbSet<>` property to the DbContext you are using.</span></span>  <span data-ttu-id="de6a1-196">Ve výchozím nastavení šablona projektu pro Azure Mobile Apps, se nazývá DbContext `Models\MobileServiceContext.cs`:</span><span class="sxs-lookup"><span data-stu-id="de6a1-196">In the default project template for Azure Mobile Apps, the DbContext is called `Models\MobileServiceContext.cs`:</span></span>

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

<span data-ttu-id="de6a1-197">Pokud máte sadu Azure SDK nainstalovat, nyní můžete vytvořit řadič tabulky šablony následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="de6a1-197">If you have the Azure SDK installed, you can now create a template table controller as follows:</span></span>

1. <span data-ttu-id="de6a1-198">Klikněte pravým tlačítkem na složku řadiče a vyberte **přidat** > **řadiče...** .</span><span class="sxs-lookup"><span data-stu-id="de6a1-198">Right-click on the Controllers folder and select **Add** > **Controller...**.</span></span>
2. <span data-ttu-id="de6a1-199">Vyberte **Azure Mobile Apps tabulky řadič** možnost a potom klikněte na **přidat**.</span><span class="sxs-lookup"><span data-stu-id="de6a1-199">Select the **Azure Mobile Apps Table Controller** option, then click **Add**.</span></span>
3. <span data-ttu-id="de6a1-200">V **přidat kontroler** dialogové okno:</span><span class="sxs-lookup"><span data-stu-id="de6a1-200">In the **Add Controller** dialog:</span></span>
   * <span data-ttu-id="de6a1-201">V **třída modelu** rozevíracího seznamu, vyberte vaše nové DTO.</span><span class="sxs-lookup"><span data-stu-id="de6a1-201">In the **Model class** dropdown, select your new DTO.</span></span>
   * <span data-ttu-id="de6a1-202">V **DbContext** rozevíracího seznamu, vyberte třídy DbContext služby Mobile.</span><span class="sxs-lookup"><span data-stu-id="de6a1-202">In the **DbContext** dropdown, select the Mobile Service DbContext class.</span></span>
   * <span data-ttu-id="de6a1-203">Název Kontroleru je vytvořen.</span><span class="sxs-lookup"><span data-stu-id="de6a1-203">The Controller name is created for you.</span></span>
4. <span data-ttu-id="de6a1-204">Klikněte na tlačítko **Přidat**.</span><span class="sxs-lookup"><span data-stu-id="de6a1-204">Click **Add**.</span></span>

<span data-ttu-id="de6a1-205">Serverový projekt quickstart obsahuje příklad pro jednoduchý **TodoItemController**.</span><span class="sxs-lookup"><span data-stu-id="de6a1-205">The quickstart server project contains an example for a simple **TodoItemController**.</span></span>

### <span data-ttu-id="de6a1-206"><a name="adjust-pagesize"></a>Postupy: přizpůsobení velikosti stránkování tabulky</span><span class="sxs-lookup"><span data-stu-id="de6a1-206"><a name="adjust-pagesize"></a>How to: Adjust the table paging size</span></span>
<span data-ttu-id="de6a1-207">Ve výchozím nastavení vrátí Azure Mobile Apps 50 záznamů na základě požadavku.</span><span class="sxs-lookup"><span data-stu-id="de6a1-207">By default, Azure Mobile Apps returns 50 records per request.</span></span>  <span data-ttu-id="de6a1-208">Stránkování zajistí, že klient neblokuje jejich vlákna uživatelského rozhraní ani serveru příliš dlouho, zajišťuje vhodné uživatelské prostředí.</span><span class="sxs-lookup"><span data-stu-id="de6a1-208">Paging ensures that the client does not tie up their UI thread nor the server for too long, ensuring a good user experience.</span></span> <span data-ttu-id="de6a1-209">Chcete-li změnit velikost tabulky stránkování, zvýšit na straně serveru "povolená velikost dotazu" a velikost stránky na straně klienta na straně serveru "povolená velikost dotazu" se upraví pomocí `EnableQuery` atribut:</span><span class="sxs-lookup"><span data-stu-id="de6a1-209">To change the table paging size, increase the server side "allowed query size" and the client-side page size The server side "allowed query size" is adjusted using the `EnableQuery` attribute:</span></span>

    [EnableQuery(PageSize = 500)]

<span data-ttu-id="de6a1-210">Ujistěte se, hodnota vlastnosti PageSize je stejná nebo větší než velikost, kterou klient požaduje.</span><span class="sxs-lookup"><span data-stu-id="de6a1-210">Ensure the PageSize is the same or larger than the size requested by the client.</span></span>  <span data-ttu-id="de6a1-211">Odkazovat na konkrétním klientovi postupy dokumentaci informace o změně velikosti stránky klienta.</span><span class="sxs-lookup"><span data-stu-id="de6a1-211">Refer to the specific client HOWTO documentation for details on changing the client page size.</span></span>

## <a name="how-to-define-a-custom-api-controller"></a><span data-ttu-id="de6a1-212">Postupy: definování vlastní kontroler API</span><span class="sxs-lookup"><span data-stu-id="de6a1-212">How to: Define a custom API controller</span></span>
<span data-ttu-id="de6a1-213">Vlastní kontroler API nabízí základní funkce na váš back-end mobilní aplikace při zavedení koncový bod.</span><span class="sxs-lookup"><span data-stu-id="de6a1-213">The custom API controller provides the most basic functionality to your Mobile App backend by exposing an endpoint.</span></span> <span data-ttu-id="de6a1-214">Můžete zaregistrovat řadič specifické mobilní rozhraní API pomocí atributu [MobileAppController].</span><span class="sxs-lookup"><span data-stu-id="de6a1-214">You can register a mobile-specific API controller using the [MobileAppController] attribute.</span></span> <span data-ttu-id="de6a1-215">`MobileAppController` Atribut zaregistruje trasu, nastaví serializátor JSON mobilní aplikace a zapne [kontrolu verzí klienta](app-service-mobile-client-and-server-versioning.md).</span><span class="sxs-lookup"><span data-stu-id="de6a1-215">The `MobileAppController` attribute registers the route, sets up the Mobile Apps JSON serializer, and turns on [client version checking](app-service-mobile-client-and-server-versioning.md).</span></span>

1. <span data-ttu-id="de6a1-216">V sadě Visual Studio, klikněte pravým tlačítkem na složku řadiče a pak klikněte na **přidat** > **řadič**, vyberte **kontroler Web API 2&mdash;prázdný** a Klikněte na tlačítko **přidat**.</span><span class="sxs-lookup"><span data-stu-id="de6a1-216">In Visual Studio, right-click the Controllers folder, then click **Add** > **Controller**, select **Web API 2 Controller&mdash;Empty** and click **Add**.</span></span>
2. <span data-ttu-id="de6a1-217">Zadejte **názvu Kontroleru**, jako například `CustomController`a klikněte na tlačítko **přidat**.</span><span class="sxs-lookup"><span data-stu-id="de6a1-217">Supply a **Controller name**, such as `CustomController`, and click **Add**.</span></span>
3. <span data-ttu-id="de6a1-218">V soubor nové třídy kontroleru, přidejte následující příkaz using:</span><span class="sxs-lookup"><span data-stu-id="de6a1-218">In the new controller class file, add the following using statement:</span></span>

        using Microsoft.Azure.Mobile.Server.Config;
4. <span data-ttu-id="de6a1-219">Použít **[MobileAppController]** atribut definici třídy kontroleru rozhraní API, jako v následujícím příkladu:</span><span class="sxs-lookup"><span data-stu-id="de6a1-219">Apply the **[MobileAppController]** attribute to the API controller class definition, as in the following example:</span></span>

        [MobileAppController]
        public class CustomController : ApiController
        {
              //...
        }
5. <span data-ttu-id="de6a1-220">V souboru App_Start/Startup.MobileApp.cs, že přidáte volání **MapApiControllers** – metoda rozšíření, jako v následujícím příkladu:</span><span class="sxs-lookup"><span data-stu-id="de6a1-220">In App_Start/Startup.MobileApp.cs file, add a call to the **MapApiControllers** extension method, as in the following example:</span></span>

        new MobileAppConfiguration()
            .MapApiControllers()
            .ApplyTo(config);

<span data-ttu-id="de6a1-221">Můžete také `UseDefaultConfiguration()` metoda rozšíření místo `MapApiControllers()`.</span><span class="sxs-lookup"><span data-stu-id="de6a1-221">You can also use the `UseDefaultConfiguration()` extension method instead of `MapApiControllers()`.</span></span> <span data-ttu-id="de6a1-222">Každý kontroler, který nemá **MobileAppControllerAttribute** použít můžete i nadále přistupovat klienti, ale nemusí být zpracován správně klienty pomocí libovolného mobilní aplikace klienta SDK.</span><span class="sxs-lookup"><span data-stu-id="de6a1-222">Any controller that does not have **MobileAppControllerAttribute** applied can still be accessed by clients, but it may not be correctly consumed by clients using any Mobile App client SDK.</span></span>

## <a name="how-to-work-with-authentication"></a><span data-ttu-id="de6a1-223">Postupy: práce s ověřováním</span><span class="sxs-lookup"><span data-stu-id="de6a1-223">How to: Work with authentication</span></span>
<span data-ttu-id="de6a1-224">Azure Mobile Apps používá aplikace služby ověřování / autorizace k zabezpečení mobilního back-endu.</span><span class="sxs-lookup"><span data-stu-id="de6a1-224">Azure Mobile Apps uses App Service Authentication / Authorization to secure your mobile backend.</span></span>  <span data-ttu-id="de6a1-225">V této části se dozvíte, jak provádět následující úlohy souvisejících s ověřováním v projektu .NET back-end serveru:</span><span class="sxs-lookup"><span data-stu-id="de6a1-225">This section shows you how to perform the following authentication-related tasks in your .NET backend server project:</span></span>

* [<span data-ttu-id="de6a1-226">Postupy: přidání ověřování do projektu serveru</span><span class="sxs-lookup"><span data-stu-id="de6a1-226">How to: Add authentication to a server project</span></span>](#add-auth)
* [<span data-ttu-id="de6a1-227">Postupy: použití vlastního ověřování pro aplikaci</span><span class="sxs-lookup"><span data-stu-id="de6a1-227">How to: Use custom authentication for your application</span></span>](#custom-auth)
* [<span data-ttu-id="de6a1-228">Postupy: načtení ověřit informace o uživateli</span><span class="sxs-lookup"><span data-stu-id="de6a1-228">How to: Retrieve authenticated user information</span></span>](#user-info)
* [<span data-ttu-id="de6a1-229">Postupy: omezení přístupu k datům Autorizovaní uživatelé</span><span class="sxs-lookup"><span data-stu-id="de6a1-229">How to: Restrict data access for authorized users</span></span>](#authorize)

### <span data-ttu-id="de6a1-230"><a name="add-auth"></a>Postupy: přidání ověřování do projektu serveru</span><span class="sxs-lookup"><span data-stu-id="de6a1-230"><a name="add-auth"></a>How to: Add authentication to a server project</span></span>
<span data-ttu-id="de6a1-231">Ověřování můžete přidat do projektu serveru tím, že rozšíří **MobileAppConfiguration** objekt a konfiguraci middlewaru OWIN.</span><span class="sxs-lookup"><span data-stu-id="de6a1-231">You can add authentication to your server project by extending the **MobileAppConfiguration** object and configuring OWIN middleware.</span></span> <span data-ttu-id="de6a1-232">Při instalaci [Microsoft.Azure.Mobile.Server.Quickstart] balíčku a volání **UseDefaultConfiguration** metoda rozšíření, můžete přeskočit ke kroku 3.</span><span class="sxs-lookup"><span data-stu-id="de6a1-232">When you install the [Microsoft.Azure.Mobile.Server.Quickstart] package and call the **UseDefaultConfiguration** extension method, you can skip to step 3.</span></span>

1. <span data-ttu-id="de6a1-233">V sadě Visual Studio, nainstalujte [Microsoft.Azure.Mobile.Server.Authentication] balíčku.</span><span class="sxs-lookup"><span data-stu-id="de6a1-233">In Visual Studio, install the [Microsoft.Azure.Mobile.Server.Authentication] package.</span></span>
2. <span data-ttu-id="de6a1-234">V souboru Startup.cs projektu, přidejte následující řádek kódu na začátku **konfigurace** metoda:</span><span class="sxs-lookup"><span data-stu-id="de6a1-234">In the Startup.cs project file, add the following line of code at the beginning of the **Configuration** method:</span></span>

        app.UseAppServiceAuthentication(config);

    <span data-ttu-id="de6a1-235">Tato komponenta middlewaru OWIN ověřuje tokeny vydané bránou přidružené služby App Service.</span><span class="sxs-lookup"><span data-stu-id="de6a1-235">This OWIN middleware component validates tokens issued by the associated App Service gateway.</span></span>
3. <span data-ttu-id="de6a1-236">Přidat `[Authorize]` atribut žádné řadiče nebo metod, které vyžaduje ověření.</span><span class="sxs-lookup"><span data-stu-id="de6a1-236">Add the `[Authorize]` attribute to any controller or method that requires authentication.</span></span>

<span data-ttu-id="de6a1-237">Další informace o ověřování klientů na váš back-end Mobile Apps naleznete v tématu [přidání ověřování do aplikace](app-service-mobile-ios-get-started-users.md).</span><span class="sxs-lookup"><span data-stu-id="de6a1-237">To learn about how to authenticate clients to your Mobile Apps backend, see [Add authentication to your app](app-service-mobile-ios-get-started-users.md).</span></span>

### <span data-ttu-id="de6a1-238"><a name="custom-auth"></a>Postupy: použití vlastního ověřování pro aplikaci</span><span class="sxs-lookup"><span data-stu-id="de6a1-238"><a name="custom-auth"></a>How to: Use custom authentication for your application</span></span>
<span data-ttu-id="de6a1-239">Pokud nechcete používat jednoho z poskytovatelů ověřování/autorizace služby App Service, můžete implementovat systému přihlášení.</span><span class="sxs-lookup"><span data-stu-id="de6a1-239">If you do not wish to use one of the App Service Authentication/Authorization providers, you can implement your own login system.</span></span> <span data-ttu-id="de6a1-240">Nainstalujte [Microsoft.Azure.Mobile.Server.Login] balíček vám pomůže při generování tokenů ověřování.</span><span class="sxs-lookup"><span data-stu-id="de6a1-240">Install the [Microsoft.Azure.Mobile.Server.Login] package to assist with authentication token generation.</span></span>  <span data-ttu-id="de6a1-241">Zadejte vlastní kód pro ověření pověření uživatele.</span><span class="sxs-lookup"><span data-stu-id="de6a1-241">Provide your own code for validating user credentials.</span></span> <span data-ttu-id="de6a1-242">Můžete například zkontrolovat proti solené a hash hesla v databázi.</span><span class="sxs-lookup"><span data-stu-id="de6a1-242">For example, you might check against salted and hashed passwords in a database.</span></span> <span data-ttu-id="de6a1-243">V následujícím příkladu `isValidAssertion()` – metoda (definovanou jinde) zodpovídá za tyto kontroly.</span><span class="sxs-lookup"><span data-stu-id="de6a1-243">In the example below, the `isValidAssertion()` method (defined elsewhere) is responsible for these checks.</span></span>

<span data-ttu-id="de6a1-244">Vlastní ověřování je zveřejněný prostřednictvím objektu ApiController vytvoření a vystavení `register` a `login` akce.</span><span class="sxs-lookup"><span data-stu-id="de6a1-244">The custom authentication is exposed by creating an ApiController and exposing `register` and `login` actions.</span></span> <span data-ttu-id="de6a1-245">Klient musí použít vlastní uživatelské rozhraní shromažďovat informace od uživatele.</span><span class="sxs-lookup"><span data-stu-id="de6a1-245">The client should use a custom UI to collect the information from the user.</span></span>  <span data-ttu-id="de6a1-246">Informace je poté odeslán do rozhraní API pomocí standardní volání HTTP POST.</span><span class="sxs-lookup"><span data-stu-id="de6a1-246">The information is then submitted to the API with a standard HTTP POST call.</span></span> <span data-ttu-id="de6a1-247">Jakmile server ověřuje kontrolní výraz, pomocí tokenu je vydat `AppServiceLoginHandler.CreateToken()` metoda.</span><span class="sxs-lookup"><span data-stu-id="de6a1-247">Once the server validates the assertion, a token is issued using the `AppServiceLoginHandler.CreateToken()` method.</span></span>  <span data-ttu-id="de6a1-248">Objektu ApiController **neměli** použít `[MobileAppController]` atribut.</span><span class="sxs-lookup"><span data-stu-id="de6a1-248">The ApiController **should not** use the `[MobileAppController]` attribute.</span></span>

<span data-ttu-id="de6a1-249">Příklad `login` akce:</span><span class="sxs-lookup"><span data-stu-id="de6a1-249">An example `login` action:</span></span>

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

<span data-ttu-id="de6a1-250">V předchozím příkladu jsou LoginResult a LoginResultUser serializovatelné objekty vystavení požadované vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="de6a1-250">In the preceding example, LoginResult and LoginResultUser are serializable objects exposing required properties.</span></span> <span data-ttu-id="de6a1-251">Klient očekává přihlášení odpovědi má být vrácen jako objekty JSON ve tvaru:</span><span class="sxs-lookup"><span data-stu-id="de6a1-251">The client expects login responses to be returned as JSON objects of the form:</span></span>

        {
            "authenticationToken": "<token>",
            "user": {
                "userId": "<userId>"
            }
        }

<span data-ttu-id="de6a1-252">`AppServiceLoginHandler.CreateToken()` Metoda zahrnuje *cílovou skupinu* a *vystavitele* parametr.</span><span class="sxs-lookup"><span data-stu-id="de6a1-252">The `AppServiceLoginHandler.CreateToken()` method includes an *audience* and an *issuer* parameter.</span></span> <span data-ttu-id="de6a1-253">Oba tyto parametry jsou nastaveny na adresu URL kořenového adresáře aplikace pomocí schéma HTTPS.</span><span class="sxs-lookup"><span data-stu-id="de6a1-253">Both of these parameters are set to the URL of your application root, using the HTTPS scheme.</span></span> <span data-ttu-id="de6a1-254">Podobně byste měli nastavit *secretKey* jako hodnota vaší aplikace je podpisový klíč.</span><span class="sxs-lookup"><span data-stu-id="de6a1-254">Similarly you should set *secretKey* to be the value of your application's signing key.</span></span> <span data-ttu-id="de6a1-255">Nedistribuovat podpisový klíč v klientovi, jako je lze použít k Mátová klíče a zosobnit uživatele.</span><span class="sxs-lookup"><span data-stu-id="de6a1-255">Do not distribute the signing key in a client as it can be used to mint keys and impersonate users.</span></span> <span data-ttu-id="de6a1-256">Můžete získat podpisový klíč při odkazování na hostované ve službě App Service *webu\_AUTH\_PODPISOVÁNÍ\_klíč* proměnné prostředí.</span><span class="sxs-lookup"><span data-stu-id="de6a1-256">You can obtain the signing key while hosted in App Service by referencing the *WEBSITE\_AUTH\_SIGNING\_KEY* environment variable.</span></span> <span data-ttu-id="de6a1-257">V případě potřeby v kontextu místního ladění, postupujte podle pokynů [místní ladění pomocí ověřování](#local-debug) oddílu načíst klíč a uložte ho jako nastavení aplikace.</span><span class="sxs-lookup"><span data-stu-id="de6a1-257">If needed in a local debugging context, follow the instructions in the [Local debugging with authentication](#local-debug) section to retrieve the key and store it as an application setting.</span></span>

<span data-ttu-id="de6a1-258">Vystavený token může zahrnovat také další deklarace identity a datum vypršení platnosti.</span><span class="sxs-lookup"><span data-stu-id="de6a1-258">The issued token may also include other claims and an expiry date.</span></span>  <span data-ttu-id="de6a1-259">Minimálně vydaný token musí obsahovat předmět (**sub**) deklarace identity.</span><span class="sxs-lookup"><span data-stu-id="de6a1-259">Minimally, the issued token must include a subject (**sub**) claim.</span></span>

<span data-ttu-id="de6a1-260">Může podporovat standard klienta `loginAsync()` metoda podle přetížení ověřování trasy.</span><span class="sxs-lookup"><span data-stu-id="de6a1-260">You can support the standard client `loginAsync()` method by overloading the authentication route.</span></span>  <span data-ttu-id="de6a1-261">Pokud klient volá `client.loginAsync('custom');` k přihlášení, musí být vaše trasy `/.auth/login/custom`.</span><span class="sxs-lookup"><span data-stu-id="de6a1-261">If the client calls `client.loginAsync('custom');` to log in, your route must be `/.auth/login/custom`.</span></span>  <span data-ttu-id="de6a1-262">Můžete nastavit trasy pro vlastní ověřování pomocí řadiče `MapHttpRoute()`:</span><span class="sxs-lookup"><span data-stu-id="de6a1-262">You can set the route for the custom authentication controller using `MapHttpRoute()`:</span></span>

    config.Routes.MapHttpRoute("custom", ".auth/login/custom", new { controller = "CustomAuth" });

> [!TIP]
> <span data-ttu-id="de6a1-263">Pomocí `loginAsync()` přístup zajišťuje, že ověřovací token je připojen k všechny následné volání služby.</span><span class="sxs-lookup"><span data-stu-id="de6a1-263">Using the `loginAsync()` approach ensures that the authentication token is attached to every subsequent call to the service.</span></span>
>
>

### <span data-ttu-id="de6a1-264"><a name="user-info"></a>Postupy: načtení ověřit informace o uživateli</span><span class="sxs-lookup"><span data-stu-id="de6a1-264"><a name="user-info"></a>How to: Retrieve authenticated user information</span></span>
<span data-ttu-id="de6a1-265">Když je uživatel ověřen službou App Service, můžete přistupovat k přiřazené ID uživatele a další informace v rozhraní .NET back-end kódu.</span><span class="sxs-lookup"><span data-stu-id="de6a1-265">When a user is authenticated by App Service, you can access the assigned user ID and other information in your .NET backend code.</span></span> <span data-ttu-id="de6a1-266">Informace o uživateli můžete použít pro při autorizačním rozhodování v back-end.</span><span class="sxs-lookup"><span data-stu-id="de6a1-266">The user information can be used for making authorization decisions in the backend.</span></span> <span data-ttu-id="de6a1-267">Následující kód získá ID uživatele, který je přidružený k požadavku:</span><span class="sxs-lookup"><span data-stu-id="de6a1-267">The following code obtains the user ID associated with a request:</span></span>

    // Get the SID of the current user.
    var claimsPrincipal = this.User as ClaimsPrincipal;
    string sid = claimsPrincipal.FindFirst(ClaimTypes.NameIdentifier).Value;

<span data-ttu-id="de6a1-268">Identifikátor SID je odvozen od ID uživatele specifický pro zprostředkovatele a je statický pro daného uživatele a zprostředkovatele přihlášení.</span><span class="sxs-lookup"><span data-stu-id="de6a1-268">The SID is derived from the provider-specific user ID and is static for a given user and login provider.</span></span>  <span data-ttu-id="de6a1-269">Identifikátor SID je pro tokeny neplatným ověřovacím hodnotu null.</span><span class="sxs-lookup"><span data-stu-id="de6a1-269">The SID is null for invalid authentication tokens.</span></span>

<span data-ttu-id="de6a1-270">Služby App Service umožňuje taky deklarace identity specifické požadavky na poskytovatele přihlášení.</span><span class="sxs-lookup"><span data-stu-id="de6a1-270">App Service also lets you request specific claims from your login provider.</span></span> <span data-ttu-id="de6a1-271">Každý poskytovatel identity může poskytovat další informace pomocí zprostředkovatele identity SDK.</span><span class="sxs-lookup"><span data-stu-id="de6a1-271">Each identity provider can provide more information using the identity provider SDK.</span></span>  <span data-ttu-id="de6a1-272">Například můžete použít rozhraní Graph API sítě Facebook informace přátel.</span><span class="sxs-lookup"><span data-stu-id="de6a1-272">For example, you can use the Facebook Graph API for friends information.</span></span>  <span data-ttu-id="de6a1-273">Zadávat lze deklarace identity, které jsou požadovány v okně zprostředkovatele na portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="de6a1-273">You can specify claims that are requested in the provider blade in the Azure portal.</span></span> <span data-ttu-id="de6a1-274">Některé deklarace identity vyžadovat dodatečnou konfiguraci pomocí zprostředkovatele identity.</span><span class="sxs-lookup"><span data-stu-id="de6a1-274">Some claims require additional configuration with the identity provider.</span></span>

<span data-ttu-id="de6a1-275">Následující kód volání **GetAppServiceIdentityAsync** metodu rozšíření k získání tokenu přihlašovací údaje, které zahrnují přístup potřeba provádět požadavky na rozhraní Graph API sítě Facebook:</span><span class="sxs-lookup"><span data-stu-id="de6a1-275">The following code calls the **GetAppServiceIdentityAsync** extension method to get the login credentials, which include the access token needed to make requests against the Facebook Graph API:</span></span>

    // Get the credentials for the logged-in user.
    var credentials =
        await this.User
        .GetAppServiceIdentityAsync<FacebookCredentials>(this.Request);

    if (credentials.Provider == "Facebook")
    {
        // Create a query string with the Facebook access token.
        var fbRequestUrl = "https://graph.facebook.com/me/feed?access_token="
            + credentials.AccessToken;

        // Create an HttpClient request.
        var client = new System.Net.Http.HttpClient();

        // Request the current user info from Facebook.
        var resp = await client.GetAsync(fbRequestUrl);
        resp.EnsureSuccessStatusCode();

        // Do something here with the Facebook user information.
        var fbInfo = await resp.Content.ReadAsStringAsync();
    }

<span data-ttu-id="de6a1-276">Přidat pomocí příkazu pro `System.Security.Principal` zajistit **GetAppServiceIdentityAsync** metoda rozšíření.</span><span class="sxs-lookup"><span data-stu-id="de6a1-276">Add a using statement for `System.Security.Principal` to provide the **GetAppServiceIdentityAsync** extension method.</span></span>

### <span data-ttu-id="de6a1-277"><a name="authorize"></a>Postupy: omezení přístupu k datům Autorizovaní uživatelé</span><span class="sxs-lookup"><span data-stu-id="de6a1-277"><a name="authorize"></a>How to: Restrict data access for authorized users</span></span>
<span data-ttu-id="de6a1-278">V předchozí části jsme vám ukázal, jak načíst ID uživatele ověřeného uživatele.</span><span class="sxs-lookup"><span data-stu-id="de6a1-278">In the previous section, we showed how to retrieve the user ID of an authenticated user.</span></span> <span data-ttu-id="de6a1-279">Můžete omezit přístup k datům a jiným prostředkům na základě této hodnoty.</span><span class="sxs-lookup"><span data-stu-id="de6a1-279">You can restrict access to data and other resources based on this value.</span></span> <span data-ttu-id="de6a1-280">Například přidávání do tabulek sloupec ID uživatele a filtrování výsledků dotazu pomocí ID uživatele je jednoduchý způsob, jak omezit vrácená data jenom na oprávněné uživatele.</span><span class="sxs-lookup"><span data-stu-id="de6a1-280">For example, adding a userId column to tables and filtering the query results by the user ID is a simple way to limit returned data only to authorized users.</span></span> <span data-ttu-id="de6a1-281">Následující kód vrátí řádky dat pouze v případě hodnoty ve sloupci ID uživatele na tabulku TodoItem odpovídá SID:</span><span class="sxs-lookup"><span data-stu-id="de6a1-281">The following code returns data rows only when the SID matches the value in the UserId column on the TodoItem table:</span></span>

    // Get the SID of the current user.
    var claimsPrincipal = this.User as ClaimsPrincipal;
    string sid = claimsPrincipal.FindFirst(ClaimTypes.NameIdentifier).Value;

    // Only return data rows that belong to the current user.
    return Query().Where(t => t.UserId == sid);

<span data-ttu-id="de6a1-282">`Query()` Metoda vrátí `IQueryable` který smí uživatel manipulovat LINQ pro zpracování filtrování.</span><span class="sxs-lookup"><span data-stu-id="de6a1-282">The `Query()` method returns an `IQueryable` that can be manipulated by LINQ to handle filtering.</span></span>

## <a name="how-to-add-push-notifications-to-a-server-project"></a><span data-ttu-id="de6a1-283">Postupy: Přidání nabízených oznámení do projektu serveru</span><span class="sxs-lookup"><span data-stu-id="de6a1-283">How to: Add push notifications to a server project</span></span>
<span data-ttu-id="de6a1-284">Přidání nabízených oznámení do projektu serveru tím, že rozšíří **MobileAppConfiguration** objekt a vytvoření centra oznámení klienta.</span><span class="sxs-lookup"><span data-stu-id="de6a1-284">Add push notifications to your server project by extending the **MobileAppConfiguration** object and creating a Notification Hubs client.</span></span>

1. <span data-ttu-id="de6a1-285">V sadě Visual Studio, klikněte pravým tlačítkem na serverový projekt a klikněte na tlačítko **spravovat balíčky NuGet**, vyhledejte `Microsoft.Azure.Mobile.Server.Notifications`, pak klikněte na tlačítko **nainstalovat**.</span><span class="sxs-lookup"><span data-stu-id="de6a1-285">In Visual Studio, right-click the server project and click **Manage NuGet Packages**, search for `Microsoft.Azure.Mobile.Server.Notifications`, then click **Install**.</span></span>
2. <span data-ttu-id="de6a1-286">Opakováním tohoto kroku můžete nainstalovat `Microsoft.Azure.NotificationHubs` balíčku, která zahrnuje klientské knihovny centra oznámení.</span><span class="sxs-lookup"><span data-stu-id="de6a1-286">Repeat this step to install the `Microsoft.Azure.NotificationHubs` package, which includes the Notification Hubs client library.</span></span>
3. <span data-ttu-id="de6a1-287">V App_Start/Startup.MobileApp.cs a přidejte volání **AddPushNotifications()** metoda rozšíření během inicializace:</span><span class="sxs-lookup"><span data-stu-id="de6a1-287">In App_Start/Startup.MobileApp.cs, and add a call to the **AddPushNotifications()** extension method during initialization:</span></span>

        new MobileAppConfiguration()
            // other features...
            .AddPushNotifications()
            .ApplyTo(config);
4. <span data-ttu-id="de6a1-288">Přidejte následující kód, který vytvoří klienta centra oznámení:</span><span class="sxs-lookup"><span data-stu-id="de6a1-288">Add the following code that creates a Notification Hubs client:</span></span>

        // Get the settings for the server project.
        HttpConfiguration config = this.Configuration;
        MobileAppSettingsDictionary settings =
            config.GetMobileAppSettingsProvider().GetMobileAppSettings();

        // Get the Notification Hubs credentials for the Mobile App.
        string notificationHubName = settings.NotificationHubName;
        string notificationHubConnection = settings
            .Connections[MobileAppSettingsKeys.NotificationHubConnectionString].ConnectionString;

        // Create a new Notification Hub client.
        NotificationHubClient hub = NotificationHubClient
            .CreateClientFromConnectionString(notificationHubConnection, notificationHubName);

<span data-ttu-id="de6a1-289">Teď můžete klienta služby Notification Hubs k odesílání nabízených oznámení na zaregistrovaných zařízení.</span><span class="sxs-lookup"><span data-stu-id="de6a1-289">You can now use the Notification Hubs client to send push notifications to registered devices.</span></span> <span data-ttu-id="de6a1-290">Další informace najdete v tématu [přidat nabízená oznámení do vaší aplikace](app-service-mobile-ios-get-started-push.md).</span><span class="sxs-lookup"><span data-stu-id="de6a1-290">For more information, see [Add push notifications to your app](app-service-mobile-ios-get-started-push.md).</span></span> <span data-ttu-id="de6a1-291">Další informace o centrech oznámení naleznete v tématu [přehledu této služby](../notification-hubs/notification-hubs-push-notification-overview.md).</span><span class="sxs-lookup"><span data-stu-id="de6a1-291">To learn more about Notification Hubs, see [Notification Hubs Overview](../notification-hubs/notification-hubs-push-notification-overview.md).</span></span>

## <span data-ttu-id="de6a1-292"><a name="tags"></a>Postupy: povolení cílové nabízené pomocí značek</span><span class="sxs-lookup"><span data-stu-id="de6a1-292"><a name="tags"></a>How to: Enable targeted push using Tags</span></span>
<span data-ttu-id="de6a1-293">Centra oznámení umožňuje odesílat cílená oznámení na konkrétní registrací pomocí značek.</span><span class="sxs-lookup"><span data-stu-id="de6a1-293">Notification Hubs lets you send targeted notifications to specific registrations by using tags.</span></span> <span data-ttu-id="de6a1-294">Automaticky vytvoří několik značky:</span><span class="sxs-lookup"><span data-stu-id="de6a1-294">Several tags are created automatically:</span></span>

* <span data-ttu-id="de6a1-295">ID instalace identifikuje konkrétní zařízení.</span><span class="sxs-lookup"><span data-stu-id="de6a1-295">The Installation ID identifies a specific device.</span></span>
* <span data-ttu-id="de6a1-296">Id uživatele podle ověřené SID identifikuje konkrétního uživatele.</span><span class="sxs-lookup"><span data-stu-id="de6a1-296">The User Id based on the authenticated SID identifies a specific user.</span></span>

<span data-ttu-id="de6a1-297">ID je přístupná z instalace **installationId** vlastnost na **MobileServiceClient**.</span><span class="sxs-lookup"><span data-stu-id="de6a1-297">The installation ID can be accessed from the **installationId** property on the **MobileServiceClient**.</span></span>  <span data-ttu-id="de6a1-298">Následující příklad ukazuje, jak přidat značky do konkrétní instalace v Notification Hubs pomocí ID instalace:</span><span class="sxs-lookup"><span data-stu-id="de6a1-298">The following example shows how to use an installation ID to add a tag to a specific installation in Notification Hubs:</span></span>

    hub.PatchInstallation("my-installation-id", new[]
    {
        new PartialUpdateOperation
        {
            Operation = UpdateOperationType.Add,
            Path = "/tags",
            Value = "{my-tag}"
        }
    });

<span data-ttu-id="de6a1-299">Back-end jsou ignorovány všechny značky dodaný klientem během registrace nabízených oznámení, při vytváření instalace.</span><span class="sxs-lookup"><span data-stu-id="de6a1-299">Any tags supplied by the client during push notification registration are ignored by the backend when creating the installation.</span></span> <span data-ttu-id="de6a1-300">Chcete-li klienta tak, aby se přidat značky do instalace, musíte vytvořit vlastní rozhraní API, které přidá značky pomocí předchozího vzoru.</span><span class="sxs-lookup"><span data-stu-id="de6a1-300">To enable a client to add tags to the installation, you must create a custom API that adds tags using the preceding pattern.</span></span>

<span data-ttu-id="de6a1-301">V tématu [klienta přidat nabízená oznámení značky] [ 5] v ukázce dokončené rychlý start App Service Mobile Apps příklad.</span><span class="sxs-lookup"><span data-stu-id="de6a1-301">See [Client-added push notification tags][5] in the App Service Mobile Apps completed quickstart sample for an example.</span></span>

## <span data-ttu-id="de6a1-302"><a name="push-user"></a>Postupy: odesílání nabízených oznámení do ověřeného uživatele</span><span class="sxs-lookup"><span data-stu-id="de6a1-302"><a name="push-user"></a>How to: Send push notifications to an authenticated user</span></span>
<span data-ttu-id="de6a1-303">Pokud ověřený uživatel zaregistruje pro nabízená oznámení, se automaticky přidá značku ID uživatele pro registraci.</span><span class="sxs-lookup"><span data-stu-id="de6a1-303">When an authenticated user registers for push notifications, a user ID tag is automatically added to the registration.</span></span> <span data-ttu-id="de6a1-304">Pomocí této značky možné posílat nabízená oznámení pro všechna zařízení registrovaných osoba.</span><span class="sxs-lookup"><span data-stu-id="de6a1-304">By using this tag, you can send push notifications to all devices registered by that person.</span></span> <span data-ttu-id="de6a1-305">Následující kód získá identifikátor SID uživatele, které zadal žádost a odešle šablony nabízených oznámení do každé registrace zařízení pro tuto osobu:</span><span class="sxs-lookup"><span data-stu-id="de6a1-305">The following code gets the SID of user making the request and sends a template push notification to every device registration for that person:</span></span>

    // Get the current user SID and create a tag for the current user.
    var claimsPrincipal = this.User as ClaimsPrincipal;
    string sid = claimsPrincipal.FindFirst(ClaimTypes.NameIdentifier).Value;
    string userTag = "_UserId:" + sid;

    // Build a dictionary for the template with the item message text.
    var notification = new Dictionary<string, string> { { "message", item.Text } };

    // Send a template notification to the user ID.
    await hub.SendTemplateNotificationAsync(notification, userTag);

<span data-ttu-id="de6a1-306">Při registraci pro nabízená oznámení z ověřený klient, ujistěte se, že před pokusem o registraci dokončením ověřování.</span><span class="sxs-lookup"><span data-stu-id="de6a1-306">When registering for push notifications from an authenticated client, make sure that authentication is complete before attempting registration.</span></span> <span data-ttu-id="de6a1-307">Další informace najdete v tématu [Push uživatelům] [ 6] v ukázce App Service Mobile Apps dokončené rychlý start pro .NET back-end.</span><span class="sxs-lookup"><span data-stu-id="de6a1-307">For more information, see [Push to users][6] in the App Service Mobile Apps completed quickstart sample for .NET backend.</span></span>

## <a name="how-to-debug-and-troubleshoot-the-net-server-sdk"></a><span data-ttu-id="de6a1-308">Postupy: ladění a řešení potíží s .NET SDK služby Server</span><span class="sxs-lookup"><span data-stu-id="de6a1-308">How to: Debug and troubleshoot the .NET Server SDK</span></span>
<span data-ttu-id="de6a1-309">Azure App Service poskytuje několik ladění a řešení potíží s techniky pro aplikace ASP.NET:</span><span class="sxs-lookup"><span data-stu-id="de6a1-309">Azure App Service provides several debugging and troubleshooting techniques for ASP.NET applications:</span></span>

* [<span data-ttu-id="de6a1-310">Monitorování služby Azure App Service</span><span class="sxs-lookup"><span data-stu-id="de6a1-310">Monitoring an Azure App Service</span></span>](../app-service-web/web-sites-monitor.md)
* [<span data-ttu-id="de6a1-311">Povolit protokolování diagnostiky v Azure App Service</span><span class="sxs-lookup"><span data-stu-id="de6a1-311">Enable Diagnostic Logging in Azure App Service</span></span>](../app-service-web/web-sites-enable-diagnostic-log.md)
* [<span data-ttu-id="de6a1-312">Řešení potíží s Azure App Service v sadě Visual Studio</span><span class="sxs-lookup"><span data-stu-id="de6a1-312">Troubleshoot an Azure App Service in Visual Studio</span></span>](../app-service-web/web-sites-dotnet-troubleshoot-visual-studio.md)

### <a name="logging"></a><span data-ttu-id="de6a1-313">Protokolování</span><span class="sxs-lookup"><span data-stu-id="de6a1-313">Logging</span></span>
<span data-ttu-id="de6a1-314">Pomocí standardní zápis trasování ASP.NET, může se zapsat do služby App Service diagnostické protokoly.</span><span class="sxs-lookup"><span data-stu-id="de6a1-314">You can write to App Service diagnostic logs by using the standard ASP.NET trace writing.</span></span> <span data-ttu-id="de6a1-315">Před zápisem do protokolů, je nutné povolit diagnostiky v váš back-end mobilní aplikace.</span><span class="sxs-lookup"><span data-stu-id="de6a1-315">Before you can write to the logs, you must enable diagnostics in your Mobile App backend.</span></span>

<span data-ttu-id="de6a1-316">Povolte diagnostiku a zapisovat do protokolů:</span><span class="sxs-lookup"><span data-stu-id="de6a1-316">To enable diagnostics and write to the logs:</span></span>

1. <span data-ttu-id="de6a1-317">Postupujte podle kroků v [povolení diagnostiky](../app-service-web/web-sites-enable-diagnostic-log.md#enablediag).</span><span class="sxs-lookup"><span data-stu-id="de6a1-317">Follow the steps in [How to enable diagnostics](../app-service-web/web-sites-enable-diagnostic-log.md#enablediag).</span></span>
2. <span data-ttu-id="de6a1-318">Přidejte následující pomocí příkazu v souboru kódu:</span><span class="sxs-lookup"><span data-stu-id="de6a1-318">Add the following using statement in your code file:</span></span>

        using System.Web.Http.Tracing;
3. <span data-ttu-id="de6a1-319">Vytvořte zapisovač trasování pro zápis z back-end .NET do diagnostickým protokolům následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="de6a1-319">Create a trace writer to write from the .NET backend to the diagnostic logs, as follows:</span></span>

        ITraceWriter traceWriter = this.Configuration.Services.GetTraceWriter();
        traceWriter.Info("Hello, World");
4. <span data-ttu-id="de6a1-320">Znovu publikovat serverový projekt a přístup k back-end mobilní aplikace provést cestu protokolování do kódu.</span><span class="sxs-lookup"><span data-stu-id="de6a1-320">Republish your server project, and access the Mobile App backend to execute the code path with the logging.</span></span>
5. <span data-ttu-id="de6a1-321">Stáhněte si a vyhodnocovat u nich protokoly, jak je popsáno v [postupy: stažení protokolů](../app-service-web/web-sites-enable-diagnostic-log.md#download).</span><span class="sxs-lookup"><span data-stu-id="de6a1-321">Download and evaluate the logs, as described in [How to: Download logs](../app-service-web/web-sites-enable-diagnostic-log.md#download).</span></span>

### <span data-ttu-id="de6a1-322"><a name="local-debug"></a>Místní ladění pomocí ověřování</span><span class="sxs-lookup"><span data-stu-id="de6a1-322"><a name="local-debug"></a>Local debugging with authentication</span></span>
<span data-ttu-id="de6a1-323">Můžete spustit aplikaci místně pro testování změny před publikováním do cloudu.</span><span class="sxs-lookup"><span data-stu-id="de6a1-323">You can run your application locally to test changes before publishing them to the cloud.</span></span> <span data-ttu-id="de6a1-324">Většina Azure Mobile Apps back-EndY, stiskněte klávesu *F5* při v sadě Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="de6a1-324">For most Azure Mobile Apps backends, press *F5* while in Visual Studio.</span></span> <span data-ttu-id="de6a1-325">Existují však některé další aspekty při použití ověřování.</span><span class="sxs-lookup"><span data-stu-id="de6a1-325">However, there are some additional considerations when using authentication.</span></span>

<span data-ttu-id="de6a1-326">Musíte mít mobilní aplikaci s ověřování/autorizace služby App Service nakonfigurované založené na cloudu a váš klient musí mít zadaného jako hostitel alternativního přihlašovacího koncového bodu cloudu.</span><span class="sxs-lookup"><span data-stu-id="de6a1-326">You must have a cloud-based mobile app with App Service Authentication/Authorization configured, and your client must have the cloud endpoint specified as the alternate login host.</span></span> <span data-ttu-id="de6a1-327">Najdete v dokumentaci pro vaše klientská platforma pro určité kroky.</span><span class="sxs-lookup"><span data-stu-id="de6a1-327">See the documentation for your client platform for the specific steps required.</span></span>

<span data-ttu-id="de6a1-328">Zajistěte, aby mobilního back-endu [Microsoft.Azure.Mobile.Server.Authentication] nainstalována.</span><span class="sxs-lookup"><span data-stu-id="de6a1-328">Ensure that your mobile backend has [Microsoft.Azure.Mobile.Server.Authentication] installed.</span></span> <span data-ttu-id="de6a1-329">Potom v třídy pro spuštění OWIN vaší aplikace, přidejte následující, po `MobileAppConfiguration` byl použit vaší `HttpConfiguration`:</span><span class="sxs-lookup"><span data-stu-id="de6a1-329">Then, in your application's OWIN startup class, add the following, after `MobileAppConfiguration` has been applied to your `HttpConfiguration`:</span></span>

        app.UseAppServiceAuthentication(new AppServiceAuthenticationOptions()
        {
            SigningKey = ConfigurationManager.AppSettings["authSigningKey"],
            ValidAudiences = new[] { ConfigurationManager.AppSettings["authAudience"] },
            ValidIssuers = new[] { ConfigurationManager.AppSettings["authIssuer"] },
            TokenHandler = config.GetAppServiceTokenHandler()
        });

<span data-ttu-id="de6a1-330">V předchozím příkladu, měli byste nakonfigurovat *authAudience* a *authIssuer* nastavení aplikace v souboru Web.config souboru pro každý být adresa URL kořenového adresáře aplikace pomocí schéma HTTPS.</span><span class="sxs-lookup"><span data-stu-id="de6a1-330">In the preceding example, you should configure the *authAudience* and *authIssuer* application settings within your Web.config file to each be the URL of your application root, using the HTTPS scheme.</span></span> <span data-ttu-id="de6a1-331">Podobně byste měli nastavit *authSigningKey* jako hodnota vaší aplikace je podpisový klíč.</span><span class="sxs-lookup"><span data-stu-id="de6a1-331">Similarly you should set *authSigningKey* to be the value of your application's signing key.</span></span>
<span data-ttu-id="de6a1-332">Postup získání podpisový klíč:</span><span class="sxs-lookup"><span data-stu-id="de6a1-332">To obtain the signing key:</span></span>

1. <span data-ttu-id="de6a1-333">Přejděte do vaší aplikace v rámci [portál Azure]</span><span class="sxs-lookup"><span data-stu-id="de6a1-333">Navigate to your app within the [Azure portal]</span></span>
2. <span data-ttu-id="de6a1-334">Klikněte na tlačítko **nástroje**, **Kudu**, **přejděte**.</span><span class="sxs-lookup"><span data-stu-id="de6a1-334">Click **Tools**, **Kudu**, **Go**.</span></span>
3. <span data-ttu-id="de6a1-335">V modulu Kudu správy lokality, klikněte na **prostředí**.</span><span class="sxs-lookup"><span data-stu-id="de6a1-335">In the Kudu Management site, click **Environment**.</span></span>
4. <span data-ttu-id="de6a1-336">Najít hodnotu pro *webu\_AUTH\_PODPISOVÁNÍ\_klíč*.</span><span class="sxs-lookup"><span data-stu-id="de6a1-336">Find the value for *WEBSITE\_AUTH\_SIGNING\_KEY*.</span></span>

<span data-ttu-id="de6a1-337">Podpisový klíč pro použití *authSigningKey* parametr v konfiguraci vaší místní aplikace.</span><span class="sxs-lookup"><span data-stu-id="de6a1-337">Use the signing key for the *authSigningKey* parameter in your local application config.</span></span>  <span data-ttu-id="de6a1-338">Mobilního back-endu je nyní vybavené, aby ověřoval tokeny při spuštění místně, které klient obdrží tokenu z koncového bodu cloudu.</span><span class="sxs-lookup"><span data-stu-id="de6a1-338">Your mobile backend is now equipped to validate tokens when running locally, which the client obtains the token from the cloud-based endpoint.</span></span>

[1]: https://msdn.microsoft.com/library/azure/dn961176.aspx
[2]: https://github.com/Azure/azure-mobile-apps-net-server
[3]: app-service-mobile-ios-get-started.md
[4]: https://azure.microsoft.com/downloads/
[5]: https://github.com/Azure-Samples/app-service-mobile-dotnet-backend-quickstart/blob/master/README.md#client-added-push-notification-tags
[6]: https://github.com/Azure-Samples/app-service-mobile-dotnet-backend-quickstart/blob/master/README.md#push-to-users
<span data-ttu-id="de6a1-339">[portál Azure]: https://portal.azure.com</span><span class="sxs-lookup"><span data-stu-id="de6a1-339">[Azure portal]: https://portal.azure.com</span></span>
<span data-ttu-id="de6a1-340">[NuGet.org]: http://www.nuget.org/</span><span class="sxs-lookup"><span data-stu-id="de6a1-340">[NuGet.org]: http://www.nuget.org/</span></span>
<span data-ttu-id="de6a1-341">[Microsoft.Azure.Mobile.Server]: http://www.nuget.org/packages/Microsoft.Azure.Mobile.Server/</span><span class="sxs-lookup"><span data-stu-id="de6a1-341">[Microsoft.Azure.Mobile.Server]: http://www.nuget.org/packages/Microsoft.Azure.Mobile.Server/</span></span>
<span data-ttu-id="de6a1-342">[Microsoft.Azure.Mobile.Server.Quickstart]: http://www.nuget.org/packages/Microsoft.Azure.Mobile.Server.Quickstart/</span><span class="sxs-lookup"><span data-stu-id="de6a1-342">[Microsoft.Azure.Mobile.Server.Quickstart]: http://www.nuget.org/packages/Microsoft.Azure.Mobile.Server.Quickstart/</span></span>
<span data-ttu-id="de6a1-343">[Microsoft.Azure.Mobile.Server.Authentication]: http://www.nuget.org/packages/Microsoft.Azure.Mobile.Server.Authentication/</span><span class="sxs-lookup"><span data-stu-id="de6a1-343">[Microsoft.Azure.Mobile.Server.Authentication]: http://www.nuget.org/packages/Microsoft.Azure.Mobile.Server.Authentication/</span></span>
<span data-ttu-id="de6a1-344">[Microsoft.Azure.Mobile.Server.Login]: http://www.nuget.org/packages/Microsoft.Azure.Mobile.Server.Login/</span><span class="sxs-lookup"><span data-stu-id="de6a1-344">[Microsoft.Azure.Mobile.Server.Login]: http://www.nuget.org/packages/Microsoft.Azure.Mobile.Server.Login/</span></span>
<span data-ttu-id="de6a1-345">[Microsoft.Azure.Mobile.Server.Notifications]: http://www.nuget.org/packages/Microsoft.Azure.Mobile.Server.Notifications/</span><span class="sxs-lookup"><span data-stu-id="de6a1-345">[Microsoft.Azure.Mobile.Server.Notifications]: http://www.nuget.org/packages/Microsoft.Azure.Mobile.Server.Notifications/</span></span>
[MapHttpAttributeRoutes]: https://msdn.microsoft.com/library/dn479134(v=vs.118).aspx
