---
title: "Vytvoření aplikace Azure-obchodní ověřování službou AD FS | Microsoft Docs"
description: "Postup vytvoření-obchodní aplikace ve službě Azure App Service se ověřuje pomocí místní služby tokenů zabezpečení. V tomto kurzu cílem služby AD FS jako místní služba tokenů zabezpečení."
services: app-service\web
documentationcenter: .net
author: cephalin
manager: erikre
editor: 
ms.assetid: 0fa9f7a1-37bd-4d11-845f-aeff6fc9e4ca
ms.service: app-service-web
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: web
ms.date: 08/31/2016
ms.author: cephalin
ms.openlocfilehash: f9a8984400378d154a504af8a41609900128d052
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="create-a-line-of-business-azure-app-with-ad-fs-authentication"></a><span data-ttu-id="505a5-104">Vytvoření aplikace Azure-obchodní ověřování službou AD FS</span><span class="sxs-lookup"><span data-stu-id="505a5-104">Create a line-of-business Azure app with AD FS authentication</span></span>
<span data-ttu-id="505a5-105">V tomto článku se dozvíte, jak vytvořit aplikaci ASP.NET MVC – obchodní v [Azure App Service](../app-service/app-service-value-prop-what-is.md) pomocí místní [Active Directory Federation Services](http://technet.microsoft.com/library/hh831502.aspx) jako zprostředkovatele identity.</span><span class="sxs-lookup"><span data-stu-id="505a5-105">This article shows you how to create an ASP.NET MVC line-of-business application in [Azure App Service](../app-service/app-service-value-prop-what-is.md) using an on-premises [Active Directory Federation Services](http://technet.microsoft.com/library/hh831502.aspx) as the identity provider.</span></span> <span data-ttu-id="505a5-106">V tomto scénáři můžete pracovat, pokud chcete vytvořit-obchodní aplikace ve službě Azure App Service, ale vaše organizace vyžaduje dat adresáře ukládaly na místě.</span><span class="sxs-lookup"><span data-stu-id="505a5-106">This scenario can work when you want to create line-of-business applications in Azure App Service but your organization requires directory data to be stored on-site.</span></span>

> [!NOTE]
> <span data-ttu-id="505a5-107">Přehled různých enterprise možnosti ověřování a autorizace pro službu Azure App Service najdete v tématu [ověřit pomocí místní služby Active Directory v aplikaci Azure](web-sites-authentication-authorization.md).</span><span class="sxs-lookup"><span data-stu-id="505a5-107">For an overview of the different enterprise authentication and authorization options for Azure App Service, see [Authenticate with on-premises Active Directory in your Azure app](web-sites-authentication-authorization.md).</span></span>
> 
> 

<a name="bkmk_build"></a>

## <a name="what-you-will-build"></a><span data-ttu-id="505a5-108">Co se sestavení</span><span class="sxs-lookup"><span data-stu-id="505a5-108">What you will build</span></span>
<span data-ttu-id="505a5-109">Vytvoříte základní aplikaci ASP.NET v Azure App Service Web Apps s následující funkce:</span><span class="sxs-lookup"><span data-stu-id="505a5-109">You will build a basic ASP.NET application in Azure App Service Web Apps with the following features:</span></span>

* <span data-ttu-id="505a5-110">Ověřuje uživatele na základě služby AD FS</span><span class="sxs-lookup"><span data-stu-id="505a5-110">Authenticates users against AD FS</span></span>
* <span data-ttu-id="505a5-111">Používá `[Authorize]` autorizace uživatelů pro různé akce</span><span class="sxs-lookup"><span data-stu-id="505a5-111">Uses `[Authorize]` to authorize users for different actions</span></span>
* <span data-ttu-id="505a5-112">Konfigurace statické pro ladění v sadě Visual Studio a publikování do App Service Web Apps (konfigurace jednou, ladění a kdykoli publikování)</span><span class="sxs-lookup"><span data-stu-id="505a5-112">Static configuration for both debugging in Visual Studio and publishing to App Service Web Apps (configure once, debug and publish anytime)</span></span>  

<a name="bkmk_need"></a>

## <a name="what-you-need"></a><span data-ttu-id="505a5-113">Co potřebujete</span><span class="sxs-lookup"><span data-stu-id="505a5-113">What you need</span></span>
[!INCLUDE [free-trial-note](../../includes/free-trial-note.md)]

<span data-ttu-id="505a5-114">Budete potřebovat k dokončení tohoto kurzu:</span><span class="sxs-lookup"><span data-stu-id="505a5-114">You need the following to complete this tutorial:</span></span>

* <span data-ttu-id="505a5-115">Místní nasazení služby AD FS (návod začátku do konce tohoto testovacího prostředí používá v tomto kurzu, najdete v části [testovacího prostředí: samostatné služby tokenů zabezpečení s AD FS ve virtuálním počítači Azure (pro test pouze)](https://blogs.msdn.microsoft.com/cephalin/2014/12/21/test-lab-standalone-sts-with-ad-fs-in-azure-vm-for-test-only/))</span><span class="sxs-lookup"><span data-stu-id="505a5-115">An on-premises AD FS deployment (for an end-to-end walkthrough of the test lab used in this tutorial, see [Test Lab: Standalone STS with AD FS in Azure VM (for test only)](https://blogs.msdn.microsoft.com/cephalin/2014/12/21/test-lab-standalone-sts-with-ad-fs-in-azure-vm-for-test-only/))</span></span>
* <span data-ttu-id="505a5-116">Oprávnění k vytvoření předávající strany vztahy důvěryhodnosti v Správa služby AD FS</span><span class="sxs-lookup"><span data-stu-id="505a5-116">Permissions to create relying party trusts in AD FS Management</span></span>
* <span data-ttu-id="505a5-117">Visual Studio 2013 Update 4 nebo novější</span><span class="sxs-lookup"><span data-stu-id="505a5-117">Visual Studio 2013 Update 4 or later</span></span>
* <span data-ttu-id="505a5-118">[Azure SDK 2.8.1](http://go.microsoft.com/fwlink/p/?linkid=323510&clcid=0x409) nebo novější</span><span class="sxs-lookup"><span data-stu-id="505a5-118">[Azure SDK 2.8.1](http://go.microsoft.com/fwlink/p/?linkid=323510&clcid=0x409) or later</span></span>

<a name="bkmk_sample"></a>

## <a name="use-sample-application-for-line-of-business-template"></a><span data-ttu-id="505a5-119">Použití ukázkovou aplikaci pro obchodní – šablony</span><span class="sxs-lookup"><span data-stu-id="505a5-119">Use sample application for line-of-business template</span></span>
<span data-ttu-id="505a5-120">Ukázkové aplikace v tomto kurzu [WebApp. WSFederation DotNet)](https://github.com/AzureADSamples/WebApp-WSFederation-DotNet), se vytvoří tým služby Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="505a5-120">The sample application in this tutorial, [WebApp-WSFederation-DotNet)](https://github.com/AzureADSamples/WebApp-WSFederation-DotNet), is created by the Azure Active Directory team.</span></span> <span data-ttu-id="505a5-121">Vzhledem k tomu, že služba AD FS podporuje služby WS-Federation, můžete ho použít jako šablonu pro vytvoření-obchodní aplikace s snadné.</span><span class="sxs-lookup"><span data-stu-id="505a5-121">Since AD FS supports WS-Federation, you can use it as a template to create line-of-business applications with ease.</span></span> <span data-ttu-id="505a5-122">Má následující funkce:</span><span class="sxs-lookup"><span data-stu-id="505a5-122">It has the following features:</span></span>

* <span data-ttu-id="505a5-123">Používá [WS-Federation](http://msdn.microsoft.com/library/bb498017.aspx) k ověřování pro místní nasazení služby AD FS</span><span class="sxs-lookup"><span data-stu-id="505a5-123">Uses [WS-Federation](http://msdn.microsoft.com/library/bb498017.aspx) to authenticate with an on-premises AD FS deployment</span></span>
* <span data-ttu-id="505a5-124">Funkce přihlášení a odhlášení</span><span class="sxs-lookup"><span data-stu-id="505a5-124">Sign-in and sign-out functionality</span></span>
* <span data-ttu-id="505a5-125">Používá [Microsoft.Owin](http://www.asp.net/aspnet/overview/owin-and-katana/an-overview-of-project-katana) (namísto technologie Windows Identity Foundation), což je budoucí technologie ASP.NET a mnohem jednodušší nastavení pro ověřování a autorizaci než WIF</span><span class="sxs-lookup"><span data-stu-id="505a5-125">Uses [Microsoft.Owin](http://www.asp.net/aspnet/overview/owin-and-katana/an-overview-of-project-katana) (instead of Windows Identity Foundation), which is the future of ASP.NET and much simpler to set up for authentication and authorization than WIF</span></span>

<a name="bkmk_setup"></a>

## <a name="set-up-the-sample-application"></a><span data-ttu-id="505a5-126">Nastavení ukázkové aplikace</span><span class="sxs-lookup"><span data-stu-id="505a5-126">Set up the sample application</span></span>
1. <span data-ttu-id="505a5-127">Klonovat nebo stáhnout ukázkové řešení v [WebApp. WSFederation DotNet](https://github.com/AzureADSamples/WebApp-WSFederation-DotNet) do vašeho místního adresáře.</span><span class="sxs-lookup"><span data-stu-id="505a5-127">Clone or download the sample solution at [WebApp-WSFederation-DotNet](https://github.com/AzureADSamples/WebApp-WSFederation-DotNet) to your local directory.</span></span>
   
   > [!NOTE]
   > <span data-ttu-id="505a5-128">Postupujte podle pokynů v [README.md](https://github.com/AzureADSamples/WebApp-WSFederation-DotNet/blob/master/README.md) ukazují, jak nastavit aplikaci s Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="505a5-128">The instructions at [README.md](https://github.com/AzureADSamples/WebApp-WSFederation-DotNet/blob/master/README.md) show you how to set up the application with Azure Active Directory.</span></span> <span data-ttu-id="505a5-129">Ale v tomto kurzu, můžete ho nastavit s AD FS, tak místo toho postupujte podle kroků v tomto poli.</span><span class="sxs-lookup"><span data-stu-id="505a5-129">But in this tutorial, you set it up with AD FS, so follow the steps here instead.</span></span>
   > 
   > 
2. <span data-ttu-id="505a5-130">Otevřete řešení a pak otevřete Controllers\AccountController.cs v **Průzkumníku řešení**.</span><span class="sxs-lookup"><span data-stu-id="505a5-130">Open the solution, and then open Controllers\AccountController.cs in the **Solution Explorer**.</span></span>
   
   <span data-ttu-id="505a5-131">Zobrazí se, že kód jednoduše problémy výzvu ověřování k ověření uživatele pomocí protokolu WS-Federation.</span><span class="sxs-lookup"><span data-stu-id="505a5-131">You will see that the code simply issues an authentication challenge to authenticate the user using WS-Federation.</span></span> <span data-ttu-id="505a5-132">V App_Start\Startup.Auth.cs je nakonfigurované všechny ověřování.</span><span class="sxs-lookup"><span data-stu-id="505a5-132">All authentication is configured in App_Start\Startup.Auth.cs.</span></span>
3. <span data-ttu-id="505a5-133">Otevřete App_Start\Startup.Auth.cs.</span><span class="sxs-lookup"><span data-stu-id="505a5-133">Open App_Start\Startup.Auth.cs.</span></span> <span data-ttu-id="505a5-134">V `ConfigureAuth` metoda, poznamenejte si řádek:</span><span class="sxs-lookup"><span data-stu-id="505a5-134">In the `ConfigureAuth` method, note the line:</span></span>
   
       app.UseWsFederationAuthentication(
           new WsFederationAuthenticationOptions
           {
               Wtrealm = realm,
               MetadataAddress = metadata                                      
           });
   
   <span data-ttu-id="505a5-135">Na světě OWIN tento fragment kódu je skutečně minimum, budete muset nakonfigurovat ověřování WS-Federation.</span><span class="sxs-lookup"><span data-stu-id="505a5-135">In the OWIN world, this snippet is really the bare minimum you need to configure WS-Federation authentication.</span></span> <span data-ttu-id="505a5-136">Je mnohem jednodušší a více elegantní než WIF, kde je soubor Web.config vložit se souborem XML po celém na místě.</span><span class="sxs-lookup"><span data-stu-id="505a5-136">It is much simpler and more elegant than WIF, where Web.config is injected with XML all over the place.</span></span> <span data-ttu-id="505a5-137">Pouze informace, které potřebujete je předávající strany (RP) identifikátor a adresu URL souboru metadat služby AD FS.</span><span class="sxs-lookup"><span data-stu-id="505a5-137">The only information you need is the relying party's (RP) identifier and the URL of your AD FS service's metadata file.</span></span> <span data-ttu-id="505a5-138">Tady je příklad:</span><span class="sxs-lookup"><span data-stu-id="505a5-138">Here's an example:</span></span>
   
   * <span data-ttu-id="505a5-139">Identifikátor RP:`https://contoso.com/MyLOBApp`</span><span class="sxs-lookup"><span data-stu-id="505a5-139">RP identifier: `https://contoso.com/MyLOBApp`</span></span>
   * <span data-ttu-id="505a5-140">Metadata adresa:`http://adfs.contoso.com/FederationMetadata/2007-06/FederationMetadata.xml`</span><span class="sxs-lookup"><span data-stu-id="505a5-140">Metadata address: `http://adfs.contoso.com/FederationMetadata/2007-06/FederationMetadata.xml`</span></span>
4. <span data-ttu-id="505a5-141">V App_Start\Startup.Auth.cs změňte následující definice statických řetězec:</span><span class="sxs-lookup"><span data-stu-id="505a5-141">In App_Start\Startup.Auth.cs, change the following static string definitions:</span></span>  
   
   <pre class="prettyprint">
   private static string realm = ConfigurationManager.AppSettings["ida:<mark>RPIdentifier</mark>"];
   <mark><del>private static string aadInstance = ConfigurationManager.AppSettings["ida:AADInstance"];</del></mark>
   <mark><del>private static string tenant = ConfigurationManager.AppSettings["ida:Tenant"];</del></mark>
   <mark><del>private static string metadata = string.Format("{0}/{1}/federationmetadata/2007-06/federationmetadata.xml", aadInstance, tenant);</del></mark>
   <mark>private static string metadata = string.Format("https://{0}/federationmetadata/2007-06/federationmetadata.xml", ConfigurationManager.AppSettings["ida:ADFS"]);</mark>
   
   <mark><del>string authority = String.Format(CultureInfo.InvariantCulture, aadInstance, tenant);</del></mark>
   </pre>
5. <span data-ttu-id="505a5-142">Nyní proveďte odpovídající změny v souboru Web.config.</span><span class="sxs-lookup"><span data-stu-id="505a5-142">Now, make the corresponding changes in Web.config.</span></span> <span data-ttu-id="505a5-143">Otevřete soubor Web.config a upravit následující nastavení aplikace:</span><span class="sxs-lookup"><span data-stu-id="505a5-143">Open the Web.config and modify the following app settings:</span></span>  
   
   <pre class="prettyprint">
   &lt;appSettings&gt;
   &lt;add key="webpages:Version" value="3.0.0.0" /&gt;
   &lt;add key="webpages:Enabled" value="false" /&gt;
   &lt;add key="ClientValidationEnabled" value="true" /&gt;
   &lt;add key="UnobtrusiveJavaScriptEnabled" value="true" /&gt;
   <mark><del>&lt;add key="ida:Wtrealm" value="[Enter the App ID URI of WebApp-WSFederation-DotNet https://contoso.onmicrosoft.com/WebApp-WSFederation-DotNet]" /&gt;</del></mark>
   <mark><del>&lt;add key="ida:AADInstance" value="https://login.microsoftonline.com" /&gt;</del></mark>
   <mark><del>&lt;add key="ida:Tenant" value="[Enter tenant name, e.g. contoso.onmicrosoft.com]" /&gt;</del></mark>
   <mark>&lt;add key="ida:RPIdentifier" value="[Enter the relying party identifier as configured in AD FS, e.g. https://localhost:44320/]" /&gt;</mark>
   <mark>&lt;add key="ida:ADFS" value="[Enter the FQDN of AD FS service, e.g. adfs.contoso.com]" /&gt;</mark>
   
   &lt;/appSettings&gt;
   </pre>
   
   <span data-ttu-id="505a5-144">Vyplňte hodnoty klíče podle příslušné prostředí.</span><span class="sxs-lookup"><span data-stu-id="505a5-144">Fill in the key values based on your respective environment.</span></span>
6. <span data-ttu-id="505a5-145">Sestavení aplikace a ujistěte se, že nejsou žádné chyby.</span><span class="sxs-lookup"><span data-stu-id="505a5-145">Build the application to make sure there are no errors.</span></span>

<span data-ttu-id="505a5-146">A je to.</span><span class="sxs-lookup"><span data-stu-id="505a5-146">That's it.</span></span> <span data-ttu-id="505a5-147">Ukázkové aplikace je nyní připraven pro práci se službou AD FS.</span><span class="sxs-lookup"><span data-stu-id="505a5-147">Now the sample application is ready to work with AD FS.</span></span> <span data-ttu-id="505a5-148">Dál musíte nakonfigurovat vztah důvěryhodnosti RP pomocí této aplikace ve službě AD FS později.</span><span class="sxs-lookup"><span data-stu-id="505a5-148">You still need to configure an RP trust with this application in AD FS later.</span></span>

<a name="bkmk_deploy"></a>

## <a name="deploy-the-sample-application-to-azure-app-service-web-apps"></a><span data-ttu-id="505a5-149">Nasadit ukázkovou aplikaci pro Azure App Service Web Apps</span><span class="sxs-lookup"><span data-stu-id="505a5-149">Deploy the sample application to Azure App Service Web Apps</span></span>
<span data-ttu-id="505a5-150">Zde publikování aplikace do webové aplikace v App Service Web Apps při zachování prostředí ladění.</span><span class="sxs-lookup"><span data-stu-id="505a5-150">Here, you publish the application to a web app in App Service Web Apps while preserving the debug environment.</span></span> <span data-ttu-id="505a5-151">Všimněte si, že se chystáte publikovat aplikaci, než ho má vztah důvěryhodnosti RP se službou AD FS, tak ověřování se stále nefunguje ještě.</span><span class="sxs-lookup"><span data-stu-id="505a5-151">Note that you're going to publish the application before it has an RP trust with AD FS, so authentication still doesn't work yet.</span></span> <span data-ttu-id="505a5-152">Pokud se teď můžete máte ale adresa URL webové aplikace, které můžete použít ke konfiguraci vztahu důvěryhodnosti RP později.</span><span class="sxs-lookup"><span data-stu-id="505a5-152">However, if you do it now you can have the web app URL that you can use to configure the RP trust later.</span></span>

1. <span data-ttu-id="505a5-153">Klikněte pravým tlačítkem na projekt a vyberte **publikovat**.</span><span class="sxs-lookup"><span data-stu-id="505a5-153">Right-click your project and select **Publish**.</span></span>
   
    ![](./media/web-sites-dotnet-lob-application-adfs/01-publish-website.png)
2. <span data-ttu-id="505a5-154">Vyberte **služby Microsoft Azure App Service**.</span><span class="sxs-lookup"><span data-stu-id="505a5-154">Select **Microsoft Azure App Service**.</span></span>
3. <span data-ttu-id="505a5-155">Pokud nejste přihlášeni do Azure, klikněte na tlačítko **přihlásit** a přihlaste se pomocí účtu Microsoft pro vaše předplatné Azure.</span><span class="sxs-lookup"><span data-stu-id="505a5-155">If you haven't signed in to Azure, click **Sign In** and use the Microsoft account for your Azure subscription to sign in.</span></span>
4. <span data-ttu-id="505a5-156">Jakmile se přihlásíte, klikněte na tlačítko **nový** k vytvoření webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="505a5-156">Once signed in, click **New** to create a web app.</span></span>
5. <span data-ttu-id="505a5-157">Vyplňte všechna povinná pole.</span><span class="sxs-lookup"><span data-stu-id="505a5-157">Fill in all required fields.</span></span> <span data-ttu-id="505a5-158">Chystáte se připojit k místním datům později, tak nevytvářejte databáze pro tuto webovou aplikaci.</span><span class="sxs-lookup"><span data-stu-id="505a5-158">You are going to connect to on-premises data later, so don't create a database for this web app.</span></span>
   
    ![](./media/web-sites-dotnet-lob-application-adfs/02-create-website.png)
6. <span data-ttu-id="505a5-159">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="505a5-159">Click **Create**.</span></span> <span data-ttu-id="505a5-160">Po vytvoření webové aplikace se otevře dialogové okno publikování webu.</span><span class="sxs-lookup"><span data-stu-id="505a5-160">Once the web app is created, the Publish Web dialog is opened.</span></span>
7. <span data-ttu-id="505a5-161">V **cílová adresa URL**, změňte **http** k **https**.</span><span class="sxs-lookup"><span data-stu-id="505a5-161">In **Destination URL**, change **http** to **https**.</span></span> <span data-ttu-id="505a5-162">Zkopírujte celou adresu URL do textového editoru pro pozdější použití.</span><span class="sxs-lookup"><span data-stu-id="505a5-162">Copy the entire URL to a text editor for later use.</span></span> <span data-ttu-id="505a5-163">Potom klikněte na **publikovat**.</span><span class="sxs-lookup"><span data-stu-id="505a5-163">Then, click **Publish**.</span></span>
   
    ![](./media/web-sites-dotnet-lob-application-adfs/03-destination-url.png)
8. <span data-ttu-id="505a5-164">V sadě Visual Studio otevřete **Web.Release.config** ve vašem projektu.</span><span class="sxs-lookup"><span data-stu-id="505a5-164">In Visual Studio, open **Web.Release.config** in your project.</span></span> <span data-ttu-id="505a5-165">Vložte následující kód XML do `<configuration>` značky a nahraďte hodnotu klíče s adresou URL vaší publikování webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="505a5-165">Insert the following XML into the `<configuration>` tag, and replace the key value with your publish web app's URL.</span></span>  
   
   <pre class="prettyprint">
   &lt;appSettings&gt;
   &lt;add key="ida:RPIdentifier" value="<mark>[e.g. https://mylobapp.azurewebsites.net/]</mark>" xdt:Transform="SetAttributes" xdt:Locator="Match(key)" /&gt;
   &lt;/appSettings&gt;</pre>

<span data-ttu-id="505a5-166">Když jste hotovi, máte dva identifikátory RP nakonfigurované v projektu, jeden pro vaše prostředí ladění v sadě Visual Studio a jeden pro publikované webové aplikaci v Azure.</span><span class="sxs-lookup"><span data-stu-id="505a5-166">When you're done, you have two RP identifiers configured in your project, one for your debug environment in Visual Studio, and one for the published web app in Azure.</span></span> <span data-ttu-id="505a5-167">Vztah důvěryhodnosti RP nastavíte pro každou ze dvou prostředí ve službě AD FS.</span><span class="sxs-lookup"><span data-stu-id="505a5-167">You will set up an RP trust for each of the two environments in AD FS.</span></span> <span data-ttu-id="505a5-168">Během ladění, se používají nastavení aplikace v souboru Web.config, aby vaše **ladění** konfigurační práce se službou AD FS.</span><span class="sxs-lookup"><span data-stu-id="505a5-168">During debugging, the app settings in Web.config are used to make your **Debug** configuration work with AD FS.</span></span> <span data-ttu-id="505a5-169">Když je publikována (ve výchozím nastavení, **verze** konfigurace je publikována), transformovaných Web.config je načtený, která zahrnuje změny nastavení aplikace v Web.Release.config.</span><span class="sxs-lookup"><span data-stu-id="505a5-169">When it's published (by default, the **Release** configuration is published), a transformed Web.config is uploaded that incorporates the app setting changes in Web.Release.config.</span></span>

<span data-ttu-id="505a5-170">Pokud se chcete připojit publikované webové aplikace v Azure a ladicí program (tj. musíte nahrát symboly ladění kódu v publikované webové aplikaci), můžete vytvořit klon konfiguraci ladění pro Azure ladění, ale s jeho vlastní vlastní transformace Web.config (např.) Web.AzureDebug.config) používající nastavení aplikace z Web.Release.config.</span><span class="sxs-lookup"><span data-stu-id="505a5-170">If you want to attach the published web app in Azure to the debugger (i.e. you must upload debug symbols of your code in the published web app), you can create a clone of the Debug configuration for Azure debugging, but with its own custom Web.config transform (e.g. Web.AzureDebug.config) that uses the app settings from Web.Release.config.</span></span> <span data-ttu-id="505a5-171">To umožňuje udržovat statické konfiguraci různých prostředích.</span><span class="sxs-lookup"><span data-stu-id="505a5-171">This allows you to maintain a static configuration across the different environments.</span></span>

<a name="bkmk_rptrusts"></a>

## <a name="configure-relying-party-trusts-in-ad-fs-management"></a><span data-ttu-id="505a5-172">Nakonfigurujte vztahy důvěryhodnosti předávající strany v Správa služby AD FS</span><span class="sxs-lookup"><span data-stu-id="505a5-172">Configure relying party trusts in AD FS Management</span></span>
<span data-ttu-id="505a5-173">Teď je potřeba nakonfigurovat v Správa služby AD FS vztah důvěryhodnosti RP, než bude možné použít ukázkovou aplikaci a ve skutečnosti ověření pomocí služby AD FS.</span><span class="sxs-lookup"><span data-stu-id="505a5-173">Now you need to configure an RP trust in AD FS Management before you can use your sample application and actually authenticate with AD FS.</span></span> <span data-ttu-id="505a5-174">Musíte nastavit dva vztahy důvěryhodnosti samostatných RP, jeden pro vaše prostředí ladění a jeden pro publikované webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="505a5-174">You will need to set up two separate RP trusts, one for your debug environment and one for your published web app.</span></span>

> [!NOTE]
> <span data-ttu-id="505a5-175">Ujistěte se, opakujte tyto kroky pro obě prostředí.</span><span class="sxs-lookup"><span data-stu-id="505a5-175">Make sure that you repeat the following steps for both of your environments.</span></span>
> 
> 

1. <span data-ttu-id="505a5-176">Na serveru služby AD FS Přihlaste se pomocí přihlašovacích údajů, které mají práva pro správu do služby AD FS.</span><span class="sxs-lookup"><span data-stu-id="505a5-176">On your AD FS server, log in with credentials that have management rights to AD FS.</span></span>
2. <span data-ttu-id="505a5-177">Otevřete správu služby AD FS.</span><span class="sxs-lookup"><span data-stu-id="505a5-177">Open AD FS Management.</span></span> <span data-ttu-id="505a5-178">Klikněte pravým tlačítkem na **AD FS\Trusted Relationships\Relying strany důvěřuje** a vyberte **přidat vztah důvěryhodnosti předávající strany**.</span><span class="sxs-lookup"><span data-stu-id="505a5-178">Right-click **AD FS\Trusted Relationships\Relying Party Trusts** and select **Add Relying Party Trust**.</span></span>
   
   ![](./media/web-sites-dotnet-lob-application-adfs/1-add-rptrust.png)
3. <span data-ttu-id="505a5-179">V **vybrat zdroj dat** vyberte **zadat data o předávající straně ručně**.</span><span class="sxs-lookup"><span data-stu-id="505a5-179">In the **Select Data Source** page, select **Enter data about the relying party manually**.</span></span> 
   
   ![](./media/web-sites-dotnet-lob-application-adfs/2-enter-rp-manually.png)
4. <span data-ttu-id="505a5-180">V **zadání zobrazovaného názvu** , zadejte zobrazovaný název pro aplikaci a klikněte na tlačítko **Další**.</span><span class="sxs-lookup"><span data-stu-id="505a5-180">In the **Specify Display Name** page, type a display name for the application and click **Next**.</span></span>
5. <span data-ttu-id="505a5-181">V **zvolte protokol** klikněte na tlačítko **Další**.</span><span class="sxs-lookup"><span data-stu-id="505a5-181">In the **Choose Protocol** page, click **Next**.</span></span>
6. <span data-ttu-id="505a5-182">V **konfigurace certifikátu** klikněte na tlačítko **Další**.</span><span class="sxs-lookup"><span data-stu-id="505a5-182">In the **Configure Certificate** page, click **Next**.</span></span>
   
   > [!NOTE]
   > <span data-ttu-id="505a5-183">Vzhledem k tomu, měli byste použít protokol HTTPS již, šifrovaných tokenů jsou volitelné.</span><span class="sxs-lookup"><span data-stu-id="505a5-183">Since you should be using HTTPS already, encrypted tokens are optional.</span></span> <span data-ttu-id="505a5-184">Pokud chcete skutečně šifrování tokenů ze služby AD FS na této stránce, musíte taky přidat logiku dešifrování tokenů v kódu.</span><span class="sxs-lookup"><span data-stu-id="505a5-184">If you really want to encrypt tokens from AD FS on this page, you must also add token-decrypting logic in your code.</span></span> <span data-ttu-id="505a5-185">Další informace najdete v tématu [ruční konfiguraci middlewaru OWIN WS-Federation a přijímat šifrované tokeny](http://chris.59north.com/post/2014/08/21/Manually-configuring-OWIN-WS-Federation-middleware-and-accepting-encrypted-tokens.aspx).</span><span class="sxs-lookup"><span data-stu-id="505a5-185">For more information, see [Manually configuring OWIN WS-Federation middleware and accepting encrypted tokens](http://chris.59north.com/post/2014/08/21/Manually-configuring-OWIN-WS-Federation-middleware-and-accepting-encrypted-tokens.aspx).</span></span>
   > 
   > 
7. <span data-ttu-id="505a5-186">Teprve potom přejděte na další krok, musíte jednu část informací z projektu sady Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="505a5-186">Before you move onto the next step, you need one piece of information from your Visual Studio project.</span></span> <span data-ttu-id="505a5-187">Ve vlastnostech projektu, Upozorňujeme **SSL URL** aplikace.</span><span class="sxs-lookup"><span data-stu-id="505a5-187">In the project properties, note the **SSL URL** of the application.</span></span> 
   
   ![](./media/web-sites-dotnet-lob-application-adfs/3-ssl-url.png)
8. <span data-ttu-id="505a5-188">Zpět na správu služby AD FS, v **konfigurace adresy URL** stránky **předávající strany Průvodce přidáním vztahu důvěryhodnosti**, vyberte **povolit podporu pasivního protokolu WS-Federation protokolu** a zadejte Adresa URL SSL projektu sady Visual Studio, kterou jste si poznamenali v předchozím kroku.</span><span class="sxs-lookup"><span data-stu-id="505a5-188">Back in AD FS Management, in the **Configure URL** page of the **Add Relying Party Trust Wizard**, select **Enable support for the WS-Federation Passive protocol** and type in the SSL URL of your Visual Studio project that you noted in the previous step.</span></span> <span data-ttu-id="505a5-189">Pak klikněte na **Další**.</span><span class="sxs-lookup"><span data-stu-id="505a5-189">Then, click **Next**.</span></span>
   
   ![](./media/web-sites-dotnet-lob-application-adfs/4-configure-url.png)
   
   > [!NOTE]
   > <span data-ttu-id="505a5-190">Adresa URL určuje, kam má posílat klienta po ověření úspěšné.</span><span class="sxs-lookup"><span data-stu-id="505a5-190">URL specifies where to send the client after authentication succeeds.</span></span> <span data-ttu-id="505a5-191">Pro prostředí ladění, by měla být <code>https://localhost:&lt;port&gt;/</code>.</span><span class="sxs-lookup"><span data-stu-id="505a5-191">For the debug environment, it should be <code>https://localhost:&lt;port&gt;/</code>.</span></span> <span data-ttu-id="505a5-192">Pro publikované webové aplikaci mělo by být adresa URL webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="505a5-192">For the published web app, it should be the web app URL.</span></span>
   > 
   > 
9. <span data-ttu-id="505a5-193">V **konfigurovat identifikátory** , zkontrolujte, že je již uvedené projektu adresy URL protokolu SSL a klikněte na tlačítko **Další**.</span><span class="sxs-lookup"><span data-stu-id="505a5-193">In the **Configure Identifiers** page, verify that your project SSL URL is already listed and click **Next**.</span></span> <span data-ttu-id="505a5-194">Klikněte na tlačítko **Další** zcela na konci průvodce výchozí možnosti.</span><span class="sxs-lookup"><span data-stu-id="505a5-194">Click **Next** all the way to the end of the wizard with default selections.</span></span>
   
   > [!NOTE]
   > <span data-ttu-id="505a5-195">V App_Start\Startup.Auth.cs projektu sady Visual Studio, tento identifikátor je nalezena shoda s hodnotou <code>WsFederationAuthenticationOptions.Wtrealm</code> během federovaného ověřování.</span><span class="sxs-lookup"><span data-stu-id="505a5-195">In App_Start\Startup.Auth.cs of your Visual Studio project, this identifier is matched against the value of <code>WsFederationAuthenticationOptions.Wtrealm</code> during federated authentication.</span></span> <span data-ttu-id="505a5-196">Ve výchozím nastavení přidá se jako identifikátor RP adresa URL aplikace z předchozího kroku.</span><span class="sxs-lookup"><span data-stu-id="505a5-196">By default, the application's URL from the previous step is added as an RP identifier.</span></span>
   > 
   > 
10. <span data-ttu-id="505a5-197">Nyní dokončení konfigurace aplikace RP pro svůj projekt ve službě AD FS.</span><span class="sxs-lookup"><span data-stu-id="505a5-197">You have now finished configuring the RP application for your project in AD FS.</span></span> <span data-ttu-id="505a5-198">Dále je nutné nakonfigurovat k posílání deklarací identit vyžaduje vaše aplikace tuto aplikaci.</span><span class="sxs-lookup"><span data-stu-id="505a5-198">Next, you configure this application to send the claims needed by your application.</span></span> <span data-ttu-id="505a5-199">**Upravit pravidla deklarací identity** dialogu je ve výchozím nastavení pro vás otevřen na konci tohoto průvodce, abyste mohli začít okamžitě.</span><span class="sxs-lookup"><span data-stu-id="505a5-199">The **Edit Claim Rules** dialog is opened by default for you at the end of the wizard so you can start immediately.</span></span> <span data-ttu-id="505a5-200">Umožňuje nakonfigurovat alespoň následující deklarace identity (s schémata v závorkách):</span><span class="sxs-lookup"><span data-stu-id="505a5-200">Let's configure at least the following claims (with schemas in parentheses):</span></span>
    
    * <span data-ttu-id="505a5-201">Název (http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name) - použitý technologií ASP.NET k hydrate `User.Identity.Name`.</span><span class="sxs-lookup"><span data-stu-id="505a5-201">Name (http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name) - used by ASP.NET to hydrate `User.Identity.Name`.</span></span>
    * <span data-ttu-id="505a5-202">Hlavní název uživatele (http://schemas.xmlsoap.org/ws/2005/05/identity/claims/upn) - použít k jednoznačné identifikaci uživatelů v organizaci.</span><span class="sxs-lookup"><span data-stu-id="505a5-202">User principal name (http://schemas.xmlsoap.org/ws/2005/05/identity/claims/upn) - used to uniquely identify users in the organization.</span></span>
    * <span data-ttu-id="505a5-203">Členství ve skupinách jako pro role (http://schemas.microsoft.com/ws/2008/06/identity/claims/role) - lze použít s `[Authorize(Roles="role1, role2,...")]` decoration autorizovat řadiče nebo akce.</span><span class="sxs-lookup"><span data-stu-id="505a5-203">Group memberships as roles (http://schemas.microsoft.com/ws/2008/06/identity/claims/role) - can be used with `[Authorize(Roles="role1, role2,...")]` decoration to authorize controllers/actions.</span></span> <span data-ttu-id="505a5-204">Ve skutečnosti nemusí být tento přístup většina původce pro povolení role.</span><span class="sxs-lookup"><span data-stu-id="505a5-204">In reality, this approach may not be the most performant for role authorization.</span></span> <span data-ttu-id="505a5-205">Pokud vaši uživatelé AD patří stovky skupin zabezpečení, budou stovky deklarace rolí v tokenu SAML.</span><span class="sxs-lookup"><span data-stu-id="505a5-205">If your AD users belong to hundreds of security groups, they become hundreds of role claims in the SAML token.</span></span> <span data-ttu-id="505a5-206">Alternativní způsob je poslat deklaraci identity jedné role podmíněně v závislosti na členství uživatele v dané skupině.</span><span class="sxs-lookup"><span data-stu-id="505a5-206">An alternative approach is to send a single role claim conditionally depending on the user's membership in a particular group.</span></span> <span data-ttu-id="505a5-207">Ale jsme budete jednoduchost pro účely tohoto kurzu.</span><span class="sxs-lookup"><span data-stu-id="505a5-207">However, we'll keep it simple for this tutorial.</span></span>
    * <span data-ttu-id="505a5-208">Název ID (http://schemas.xmlsoap.org/ws/2005/05/identity/claims/nameidentifier) – lze použít pro ověření proti padělání.</span><span class="sxs-lookup"><span data-stu-id="505a5-208">Name ID (http://schemas.xmlsoap.org/ws/2005/05/identity/claims/nameidentifier) - can be used for anti-forgery validation.</span></span> <span data-ttu-id="505a5-209">Další informace o tom, jak aby fungoval s ověřování proti padělání najdete v tématu **přidat funkce – obchodní** části [vytvoření-obchodní aplikace Azure s Azure Active Directory authentication](web-sites-dotnet-lob-application-azure-ad.md#bkmk_crud).</span><span class="sxs-lookup"><span data-stu-id="505a5-209">For more information on how to make it work with anti-forgery validation, see the **Add line-of-business functionality** section of [Create a line-of-business Azure app with Azure Active Directory authentication](web-sites-dotnet-lob-application-azure-ad.md#bkmk_crud).</span></span>
    
    > [!NOTE]
    > <span data-ttu-id="505a5-210">Typy deklarací, které budete muset nakonfigurovat pro vaši aplikaci je určen podle potřeb vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="505a5-210">The claim types you need to configure for your application is determined by your application's needs.</span></span> <span data-ttu-id="505a5-211">Seznam deklarací identity, které podporuje aplikace Azure Active Directory (tj. vztahy důvěryhodnosti RP), například v tématu [podporované tokenu a typy deklarací identity](http://msdn.microsoft.com/library/azure/dn195587.aspx).</span><span class="sxs-lookup"><span data-stu-id="505a5-211">For the list of claims supported by Azure Active Directory applications (i.e. RP trusts), for example, see [Supported Token and Claim Types](http://msdn.microsoft.com/library/azure/dn195587.aspx).</span></span>
    > 
    > 
11. <span data-ttu-id="505a5-212">V dialogovém okně upravit pravidla deklarací identity, klikněte na tlačítko **přidat pravidlo**.</span><span class="sxs-lookup"><span data-stu-id="505a5-212">In the Edit Claim Rules dialog, click **Add Rule**.</span></span>
12. <span data-ttu-id="505a5-213">Nakonfigurujte název UPN a role deklarace identity, jak je znázorněno v snímek obrazovky a klikněte na **Dokončit**.</span><span class="sxs-lookup"><span data-stu-id="505a5-213">Configure the name, UPN, and role claims as shown in the screenshot and click **Finish**.</span></span>
    
    ![](./media/web-sites-dotnet-lob-application-adfs/5-ldap-claims.png)
    
    <span data-ttu-id="505a5-214">Dále vytvoříte přechodný název ID deklarace identity pomocí kroků ukázáno v [identifikátory názvů v kontrolní výrazy SAML](http://blogs.msdn.com/b/card/archive/2010/02/17/name-identifiers-in-saml-assertions.aspx).</span><span class="sxs-lookup"><span data-stu-id="505a5-214">Next, you create a transient name ID claim using the steps demonstrated in [Name Identifiers in SAML assertions](http://blogs.msdn.com/b/card/archive/2010/02/17/name-identifiers-in-saml-assertions.aspx).</span></span>
13. <span data-ttu-id="505a5-215">Klikněte na tlačítko **přidat pravidlo** znovu.</span><span class="sxs-lookup"><span data-stu-id="505a5-215">Click **Add Rule** again.</span></span>
14. <span data-ttu-id="505a5-216">Vyberte **odesílat deklarace pomocí vlastního pravidla** a klikněte na tlačítko **Další**.</span><span class="sxs-lookup"><span data-stu-id="505a5-216">Select **Send Claims Using a Custom Rule** and click **Next**.</span></span>
15. <span data-ttu-id="505a5-217">Vložte následující jazyka pravidel do **vlastní pravidlo** pole, název pravidla **za identifikátor relace** a klikněte na tlačítko **Dokončit**.</span><span class="sxs-lookup"><span data-stu-id="505a5-217">Paste the following rule language into the **Custom rule** box, name the rule **Per Session Identifier** and click **Finish**.</span></span>  
    
    <pre class="prettyprint">
    c1:[Type == "http://schemas.microsoft.com/ws/2008/06/identity/claims/windowsaccountname"] &amp;&amp;
    c2:[Type == "http://schemas.microsoft.com/ws/2008/06/identity/claims/authenticationinstant"]
     => add(
         store = "_OpaqueIdStore",
         types = ("<mark>http://contoso.com/internal/sessionid</mark>"),
         query = "{0};{1};{2};{3};{4}",
         param = "useEntropy",
         param = c1.Value,
         param = c1.OriginalIssuer,
         param = "",
         param = c2.Value);
    </pre>
    
    <span data-ttu-id="505a5-218">Vlastní pravidlo by měl vypadat takto:</span><span class="sxs-lookup"><span data-stu-id="505a5-218">Your custom rule should look like this screenshot:</span></span>
    
    ![](./media/web-sites-dotnet-lob-application-adfs/6-per-session-identifier.png)
16. <span data-ttu-id="505a5-219">Klikněte na tlačítko **přidat pravidlo** znovu.</span><span class="sxs-lookup"><span data-stu-id="505a5-219">Click **Add Rule** again.</span></span>
17. <span data-ttu-id="505a5-220">Vyberte **transformovat příchozí deklarace** a klikněte na tlačítko **Další**.</span><span class="sxs-lookup"><span data-stu-id="505a5-220">Select **Transform an Incoming Claim** and click **Next**.</span></span>
18. <span data-ttu-id="505a5-221">Konfiguraci pravidla, jak je znázorněno na snímku obrazovky (typ deklarace identity, kterou jste vytvořili v vlastní pravidlo pomocí) a klikněte na tlačítko **Dokončit**.</span><span class="sxs-lookup"><span data-stu-id="505a5-221">Configure the rule as shown in the screenshot (using the claim type you created in the custom rule) and click **Finish**.</span></span>
    
    ![](./media/web-sites-dotnet-lob-application-adfs/7-transient-name-id.png)
    
    <span data-ttu-id="505a5-222">Podrobné informace o krocích pro přechodný deklarace ID názvu najdete v tématu [identifikátory názvů v kontrolní výrazy SAML](http://blogs.msdn.com/b/card/archive/2010/02/17/name-identifiers-in-saml-assertions.aspx).</span><span class="sxs-lookup"><span data-stu-id="505a5-222">For detailed information on the steps for the transient Name ID claim, see [Name Identifiers in SAML assertions](http://blogs.msdn.com/b/card/archive/2010/02/17/name-identifiers-in-saml-assertions.aspx).</span></span>
19. <span data-ttu-id="505a5-223">Klikněte na tlačítko **použít** v **upravit pravidla deklarací identity** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="505a5-223">Click **Apply** in the **Edit Claim Rules** dialog.</span></span> <span data-ttu-id="505a5-224">Teď by měl vypadat jako na následujícím snímku obrazovky:</span><span class="sxs-lookup"><span data-stu-id="505a5-224">It should now look like the following screenshot:</span></span>
    
    ![](./media/web-sites-dotnet-lob-application-adfs/8-all-claim-rules.png)
    
    > [!NOTE]
    > <span data-ttu-id="505a5-225">Znovu zkontrolujte, zda tento postup opakujte pro vaše prostředí ladění a publikovanou webovou aplikací.</span><span class="sxs-lookup"><span data-stu-id="505a5-225">Again, make sure that you repeat these steps for both your debug environment and published web app.</span></span>
    > 
    > 

<a name="bkmk_test"></a>

## <a name="test-federated-authentication-for-your-application"></a><span data-ttu-id="505a5-226">Test federovaného ověřování pro aplikaci</span><span class="sxs-lookup"><span data-stu-id="505a5-226">Test federated authentication for your application</span></span>
<span data-ttu-id="505a5-227">Jste připraveni k testování vaší aplikace logiky ověřování u služby AD FS.</span><span class="sxs-lookup"><span data-stu-id="505a5-227">You are ready to test your application's authentication logic against AD FS.</span></span> <span data-ttu-id="505a5-228">V mé testovacího prostředí služby AD FS je nutné testovacího uživatele, který patří do testovací skupiny ve službě Active Directory (AD).</span><span class="sxs-lookup"><span data-stu-id="505a5-228">In my AD FS lab environment, I have a test user that belongs to a test group in Active Directory (AD).</span></span>

![](./media/web-sites-dotnet-lob-application-adfs/10-test-user-and-group.png)

<span data-ttu-id="505a5-229">K testování ověřování v ladicím programu, musíte udělat teď je typ `F5`.</span><span class="sxs-lookup"><span data-stu-id="505a5-229">To test authentication in the debugger, all you need to do now is type `F5`.</span></span> <span data-ttu-id="505a5-230">Pokud chcete otestovat ověření v publikované webové aplikaci, přejděte na adresu URL.</span><span class="sxs-lookup"><span data-stu-id="505a5-230">If you want to test authentication in the published web app, navigate to the URL.</span></span>

<span data-ttu-id="505a5-231">Po načtení webové aplikace klikněte na tlačítko **přihlásit**.</span><span class="sxs-lookup"><span data-stu-id="505a5-231">After the web application loads, click **Sign In**.</span></span> <span data-ttu-id="505a5-232">Měli byste nyní získat přihlašovací dialogové okno nebo přihlašovací stránku obsloužených služby AD FS, v závislosti na zvolené službou AD FS metodu ověřování.</span><span class="sxs-lookup"><span data-stu-id="505a5-232">You should now get either a login dialog or the login page served by AD FS, depending on the authentication method chosen by AD FS.</span></span> <span data-ttu-id="505a5-233">Zde je zobrazila v Internet Exploreru 11.</span><span class="sxs-lookup"><span data-stu-id="505a5-233">Here's what I get in Internet Explorer 11.</span></span>

![](./media/web-sites-dotnet-lob-application-adfs/9-test-debugging.png)

<span data-ttu-id="505a5-234">Jakmile se můžete přihlásit jako uživatel v doméně AD nasazení služby AD FS, měli byste vidět domovské stránky znovu s **Hello, <User Name>!**</span><span class="sxs-lookup"><span data-stu-id="505a5-234">Once you log in with a user in the AD domain of the AD FS deployment, you should now see the homepage again with **Hello, <User Name>!**</span></span> <span data-ttu-id="505a5-235">v rohu.</span><span class="sxs-lookup"><span data-stu-id="505a5-235">in the corner.</span></span> <span data-ttu-id="505a5-236">Zde je možné získat.</span><span class="sxs-lookup"><span data-stu-id="505a5-236">Here's what I get.</span></span>

![](./media/web-sites-dotnet-lob-application-adfs/11-test-debugging-success.png)

<span data-ttu-id="505a5-237">Zatím jste úspěšně následujícími způsoby:</span><span class="sxs-lookup"><span data-stu-id="505a5-237">So far, you've succeeded in the following ways:</span></span>

* <span data-ttu-id="505a5-238">Vaše aplikace úspěšně dosáhl služby AD FS a identifikátor odpovídající RP se nachází v databázi služby AD FS</span><span class="sxs-lookup"><span data-stu-id="505a5-238">Your application has successfully reached AD FS and a matching RP identifier is found in the AD FS database</span></span>
* <span data-ttu-id="505a5-239">Služba AD FS byl úspěšně ověřen služby Active Directory a přesměrování, které zpět na domovskou stránku aplikace</span><span class="sxs-lookup"><span data-stu-id="505a5-239">AD FS has successfully authenticated an AD user and redirect you back to the application's homepage</span></span>
* <span data-ttu-id="505a5-240">Služby AD FS jako úspěšně odeslána deklaraci identity názvu (http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name) k vaší aplikaci, jak je uvedeno fakt, že uživatelské jméno se zobrazí v horním.</span><span class="sxs-lookup"><span data-stu-id="505a5-240">AD FS as successfully sent the name claim (http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name) to your application, as indicated by the fact that the user name is displayed in the corner.</span></span> 

<span data-ttu-id="505a5-241">Pokud chybí deklarace identity názvu, jste viděli by **Hello,!**.</span><span class="sxs-lookup"><span data-stu-id="505a5-241">If the name claim is missing, you would have seen **Hello, !**.</span></span> <span data-ttu-id="505a5-242">Pokud se podíváte na Views\Shared\_LoginPartial.cshtml, zjistíte, že používá `User.Identity.Name` zobrazíte uživatelské jméno.</span><span class="sxs-lookup"><span data-stu-id="505a5-242">If you look at Views\Shared\_LoginPartial.cshtml, you find that it uses `User.Identity.Name` to display the user name.</span></span> <span data-ttu-id="505a5-243">Jak je uvedeno nahoře, pokud je k dispozici v tokenu SAML deklarace identity názvu ověřeného uživatele, ASP.NET hydráty tato vlastnost s ním.</span><span class="sxs-lookup"><span data-stu-id="505a5-243">As mentioned previously, if the name claim of the authenticated user is available in the SAML token, ASP.NET hydrates this property with it.</span></span> <span data-ttu-id="505a5-244">Pokud chcete zobrazit všechny deklarace identity, které se odesílají přes službu AD FS, uveďte zarážky v Controllers\HomeController.cs, v metodě akce indexu.</span><span class="sxs-lookup"><span data-stu-id="505a5-244">To see all the claims that are sent by AD FS, put a breakpoint in Controllers\HomeController.cs, in the Index action method.</span></span> <span data-ttu-id="505a5-245">Po ověření uživatele, zkontrolujte `System.Security.Claims.Current.Claims` kolekce.</span><span class="sxs-lookup"><span data-stu-id="505a5-245">After the user is authenticated, inspect the `System.Security.Claims.Current.Claims` collection.</span></span>

![](./media/web-sites-dotnet-lob-application-adfs/12-test-debugging-all-claims.png) 

<a name="bkmk_authorize"></a>

## <a name="authorize-users-for-specific-controllers-or-actions"></a><span data-ttu-id="505a5-246">Autorizace uživatelů pro konkrétní řadiče nebo akce</span><span class="sxs-lookup"><span data-stu-id="505a5-246">Authorize users for specific controllers or actions</span></span>
<span data-ttu-id="505a5-247">Vzhledem k tomu, že jste zahrnuli členství ve skupinách jako deklarace identity role ve vaší konfiguraci RP vztahu důvěryhodnosti, teď můžete například vytvořit přímo v `[Authorize(Roles="...")]` decoration pro kontrolery a akce.</span><span class="sxs-lookup"><span data-stu-id="505a5-247">Since you have included group memberships as role claims in your RP trust configuration, you can now use them directly in the `[Authorize(Roles="...")]` decoration for controllers and actions.</span></span> <span data-ttu-id="505a5-248">Obchodní aplikace pomocí vzoru vytvořit-čtení-aktualizace-odstranění (CRUD) můžete povolit konkrétní role pro přístup k každá akce.</span><span class="sxs-lookup"><span data-stu-id="505a5-248">In a line-of-business application with the Create-Read-Update-Delete (CRUD) pattern, you can authorize specific roles to access each action.</span></span> <span data-ttu-id="505a5-249">Prozatím se právě vyzkoušejte tuto funkci u existujícího řadiče domovské.</span><span class="sxs-lookup"><span data-stu-id="505a5-249">For now, you will just try out this feature on the existing Home controller.</span></span>

1. <span data-ttu-id="505a5-250">Otevřete Controllers\HomeController.cs.</span><span class="sxs-lookup"><span data-stu-id="505a5-250">Open Controllers\HomeController.cs.</span></span>
2. <span data-ttu-id="505a5-251">Uspořádání `About` a `Contact` metody akce podobná následující kód, pomocí zabezpečení členství ve skupinách, které má ověřeného uživatele.</span><span class="sxs-lookup"><span data-stu-id="505a5-251">Decorate the `About` and `Contact` action methods similar to the following code, using security group memberships that your authenticated user has.</span></span>  
   
    <pre class="prettyprint">
    <mark>[Authorize(Roles="Test Group")]</mark>
    public ActionResult About()
    {
        ViewBag.Message = "Your application description page.";
   
        return View();
    }
   
    <mark>[Authorize(Roles="Domain Admins")]</mark>
    public ActionResult Contact()
    {
        ViewBag.Message = "Your contact page.";
   
        return View();
    }
    </pre>
   
    <span data-ttu-id="505a5-252">Vzhledem k tomu, že po přidání **testovací uživatele** k **testovací skupina** v mé testovacího prostředí služby AD FS I použijete testovací skupiny k testování autorizace na `About`.</span><span class="sxs-lookup"><span data-stu-id="505a5-252">Since I added **Test User** to **Test Group** in my AD FS lab environment, I'll use Test Group to test authorization on `About`.</span></span> <span data-ttu-id="505a5-253">Pro `Contact`, I budete testovat záporné malá **Domain Admins**, do které **testovací uživatele** nepatří.</span><span class="sxs-lookup"><span data-stu-id="505a5-253">For `Contact`, I'll test the negative case of **Domain Admins**, to which **Test User** doesn't belong.</span></span>
3. <span data-ttu-id="505a5-254">Spuštění ladicího programu zadáním `F5` a přihlaste se a pak klikněte na **o**.</span><span class="sxs-lookup"><span data-stu-id="505a5-254">Start the debugger by typing `F5` and sign in, then click **About**.</span></span> <span data-ttu-id="505a5-255">Má teď se zobrazil `~/About/Index` stránka úspěšně, pokud ověřený uživatel má oprávnění pro tuto akci.</span><span class="sxs-lookup"><span data-stu-id="505a5-255">You should now be viewing the `~/About/Index` page successfully, if your authenticated user is authorized for that action.</span></span>
4. <span data-ttu-id="505a5-256">Klikněte na tlačítko **kontaktujte**, v mém případě by neměl Autorizovat **testovacího uživatele** pro akci.</span><span class="sxs-lookup"><span data-stu-id="505a5-256">Now click **Contact**, which in my case should not authorize **Test User** for the action.</span></span> <span data-ttu-id="505a5-257">V prohlížeči se však přesměruje na službu AD FS, který nakonec zobrazí tato zpráva:</span><span class="sxs-lookup"><span data-stu-id="505a5-257">However, the browser is redirected to AD FS, which eventually shows this message:</span></span>
   
    ![](./media/web-sites-dotnet-lob-application-adfs/13-authorize-adfs-error.png)
   
    <span data-ttu-id="505a5-258">Pokud byste prozkoumat této chybě v prohlížeči událostí na serveru služby AD FS, zobrazí tato zpráva o výjimce:</span><span class="sxs-lookup"><span data-stu-id="505a5-258">If you investigate this error in Event Viewer on the AD FS server, you see this exception message:</span></span>  
   
    <pre class="prettyprint">
    Microsoft.IdentityServer.Web.InvalidRequestException: MSIS7042: <mark>The same client browser session has made '6' requests in the last '11' seconds.</mark> Contact your administrator for details.
       at Microsoft.IdentityServer.Web.Protocols.PassiveProtocolHandler.UpdateLoopDetectionCookie(WrappedHttpListenerContext context)
       at Microsoft.IdentityServer.Web.Protocols.WSFederation.WSFederationProtocolHandler.SendSignInResponse(WSFederationContext context, MSISSignInResponse response)
       at Microsoft.IdentityServer.Web.PassiveProtocolListener.ProcessProtocolRequest(ProtocolContext protocolContext, PassiveProtocolHandler protocolHandler)
       at Microsoft.IdentityServer.Web.PassiveProtocolListener.OnGetContext(WrappedHttpListenerContext context)
    </pre>
   
    <span data-ttu-id="505a5-259">Z důvodu této chyby je, že ve výchozím nastavení, MVC vrátí 401 Neautorizováno, pokud nemáte oprávnění role uživatele.</span><span class="sxs-lookup"><span data-stu-id="505a5-259">The reason for this error is that by default, MVC returns a 401 Unauthorized when a user's roles are not authorized.</span></span> <span data-ttu-id="505a5-260">Tím se spustí opětovné ověření požadavek na zprostředkovatele identity (AD FS).</span><span class="sxs-lookup"><span data-stu-id="505a5-260">This triggers a reauthentication request to your identity provider (AD FS).</span></span> <span data-ttu-id="505a5-261">Vzhledem k tomu, že uživatel je již ověřen, služba AD FS vrátí na stejné stránce, která pak vydává jiný 401, vytváření smyčku přesměrování.</span><span class="sxs-lookup"><span data-stu-id="505a5-261">Since the user is already authenticated, AD FS returns to the same page, which then issues another 401, creating a redirect loop.</span></span> <span data-ttu-id="505a5-262">Přepíše třídy AuthorizeAttribute na `HandleUnauthorizedRequest` metodu se zobrazit něco jednoduché logiky, která má smysl pokračovat smyčky přesměrování.</span><span class="sxs-lookup"><span data-stu-id="505a5-262">You will override AuthorizeAttribute's `HandleUnauthorizedRequest` method with simple logic to show something that makes sense instead of continuing the redirect loop.</span></span>
5. <span data-ttu-id="505a5-263">Vytvořte soubor v projektu názvem AuthorizeAttribute.cs a vložte následující kód do ní.</span><span class="sxs-lookup"><span data-stu-id="505a5-263">Create a file in the project called AuthorizeAttribute.cs, and paste the following code into it.</span></span>
   
        using System;
        using System.Web.Mvc;
        using System.Web.Routing;
   
        namespace WebApp_WSFederation_DotNet
        {
            [AttributeUsage(AttributeTargets.Class | AttributeTargets.Method, Inherited = true, AllowMultiple = true)]
            public class AuthorizeAttribute : System.Web.Mvc.AuthorizeAttribute
            {
                protected override void HandleUnauthorizedRequest(AuthorizationContext filterContext)
                {
                    if (filterContext.HttpContext.Request.IsAuthenticated)
                    {
                        filterContext.Result = new System.Web.Mvc.HttpStatusCodeResult((int)System.Net.HttpStatusCode.Forbidden);
                    }
                    else
                    {
                        base.HandleUnauthorizedRequest(filterContext);
                    }
                }
            }
        }
   
    <span data-ttu-id="505a5-264">Kód přepsání odešle v případech ověřený, ale neoprávněným HTTP 403 (zakázáno) místo protokolu HTTP 401 (Neautorizováno).</span><span class="sxs-lookup"><span data-stu-id="505a5-264">The override code sends an HTTP 403 (Forbidden) instead of HTTP 401 (Unauthorized) in  authenticated but unauthorized cases.</span></span>
6. <span data-ttu-id="505a5-265">Spustit ladicí program znovu s `F5`.</span><span class="sxs-lookup"><span data-stu-id="505a5-265">Run the debugger again with `F5`.</span></span> <span data-ttu-id="505a5-266">Kliknutím na tlačítko **kontaktujte** nyní zobrazuje informativnější (i když rozdělení nežádoucí) chybovou zprávu:</span><span class="sxs-lookup"><span data-stu-id="505a5-266">Clicking **Contact** now shows a more informative (albeit unattractive) error message:</span></span>
   
    ![](./media/web-sites-dotnet-lob-application-adfs/14-unauthorized-forbidden.png)
7. <span data-ttu-id="505a5-267">Publikování aplikace do Azure App Service Web Apps znovu a testování chování živé aplikace.</span><span class="sxs-lookup"><span data-stu-id="505a5-267">Publish the application to Azure App Service Web Apps again, and test the behavior of the live application.</span></span>

<a name="bkmk_data"></a>

## <a name="connect-to-on-premises-data"></a><span data-ttu-id="505a5-268">Připojení k místním datům</span><span class="sxs-lookup"><span data-stu-id="505a5-268">Connect to on-premises data</span></span>
<span data-ttu-id="505a5-269">Problémy s dodržováním předpisů s udržováním organizace dat mimo místní je důvod, že chcete implementovat-obchodní aplikace se službou AD FS místo Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="505a5-269">A reason that you would want to implement your line-of-business application with AD FS instead of Azure Active Directory is compliance issues with keeping organization data off-premise.</span></span> <span data-ttu-id="505a5-270">Také to může znamenat, že webové aplikace v Azure musí přístup k místní databáze, vzhledem k tomu, že nebudete moci používat [SQL Database](/services/sql-database/) jako datové vrstvy pro webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="505a5-270">This may also mean that your web app in Azure must access on-premises databases, since you are not allowed to use [SQL Database](/services/sql-database/) as the data tier for your web apps.</span></span>

<span data-ttu-id="505a5-271">Azure App Service Web Apps podporuje přístupu k místní databáze s dva přístupy: [hybridní připojení](../biztalk-services/integration-hybrid-connection-overview.md) a [virtuální sítě](web-sites-integrate-with-vnet.md).</span><span class="sxs-lookup"><span data-stu-id="505a5-271">Azure App Service Web Apps supports accessing on-premises databases with two approaches: [Hybrid Connections](../biztalk-services/integration-hybrid-connection-overview.md) and [Virtual Networks](web-sites-integrate-with-vnet.md).</span></span> <span data-ttu-id="505a5-272">Další informace najdete v tématu [integrace pomocí virtuální sítě a hybridních připojení s Azure App Service Web Apps](https://azure.microsoft.com/blog/2014/10/30/using-vnet-or-hybrid-conn-with-websites/).</span><span class="sxs-lookup"><span data-stu-id="505a5-272">For more information, see [Using VNET integration and Hybrid connections with Azure App Service Web Apps](https://azure.microsoft.com/blog/2014/10/30/using-vnet-or-hybrid-conn-with-websites/).</span></span>

<a name="bkmk_resources"></a>

## <a name="further-resources"></a><span data-ttu-id="505a5-273">Další prostředky</span><span class="sxs-lookup"><span data-stu-id="505a5-273">Further resources</span></span>
* [<span data-ttu-id="505a5-274">Ověření pomocí místní služby Active Directory v aplikaci Azure</span><span class="sxs-lookup"><span data-stu-id="505a5-274">Authenticate with on-premises Active Directory in your Azure app</span></span>](web-sites-authentication-authorization.md)
* [<span data-ttu-id="505a5-275">Vytvoření aplikace Azure-obchodní pomocí ověřování Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="505a5-275">Create a line-of-business Azure app with Azure Active Directory authentication</span></span>](web-sites-dotnet-lob-application-azure-ad.md)
* [<span data-ttu-id="505a5-276">Použijte možnost organizační ověřování místní (služby AD FS) pomocí technologie ASP.NET v sadě Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="505a5-276">Use the On-Premises Organizational Authentication Option (ADFS) With ASP.NET in Visual Studio 2013</span></span>](http://www.cloudidentity.com/blog/2014/02/12/use-the-on-premises-organizational-authentication-option-adfs-with-asp-net-in-visual-studio-2013/)
* [<span data-ttu-id="505a5-277">Migrovat VS2013 webového projektu ze WIF Katana</span><span class="sxs-lookup"><span data-stu-id="505a5-277">Migrate a VS2013 Web Project From WIF to Katana</span></span>](http://www.cloudidentity.com/blog/2014/09/15/MIGRATE-A-VS2013-WEB-PROJECT-FROM-WIF-TO-KATANA/)
* [<span data-ttu-id="505a5-278">Přehled služby Active Directory Federation Services</span><span class="sxs-lookup"><span data-stu-id="505a5-278">Active Directory Federation Services Overview</span></span>](http://technet.microsoft.com/library/hh831502.aspx)
* [<span data-ttu-id="505a5-279">Specifikace protokolu WS-Federation 1.1</span><span class="sxs-lookup"><span data-stu-id="505a5-279">WS-Federation 1.1 specification</span></span>](http://download.boulder.ibm.com/ibmdl/pub/software/dw/specs/ws-fed/WS-Federation-V1-1B.pdf?S_TACT=105AGX04&S_CMP=LP)

