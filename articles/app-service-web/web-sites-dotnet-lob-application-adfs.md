---
title: "aaaCreate aplikace obchodní Azure ověřování službou AD FS | Microsoft Docs"
description: "Zjistěte, jak toocreate-obchodní aplikace ve službě Azure App Service, který ověřuje pomocí místní služby tokenů zabezpečení. V tomto kurzu cílem služby AD FS jako hello místní služby tokenů zabezpečení."
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
ms.openlocfilehash: cfd6f14837642fbf58ca5da9dee72db69cd5ab7f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-line-of-business-azure-app-with-ad-fs-authentication"></a><span data-ttu-id="23575-104">Vytvoření aplikace Azure-obchodní ověřování službou AD FS</span><span class="sxs-lookup"><span data-stu-id="23575-104">Create a line-of-business Azure app with AD FS authentication</span></span>
<span data-ttu-id="23575-105">Tento článek ukazuje, jak toocreate rozhraní ASP.NET MVC – obchodní aplikace v [Azure App Service](../app-service/app-service-value-prop-what-is.md) pomocí místní [Active Directory Federation Services](http://technet.microsoft.com/library/hh831502.aspx) jako zprostředkovatele identity hello.</span><span class="sxs-lookup"><span data-stu-id="23575-105">This article shows you how toocreate an ASP.NET MVC line-of-business application in [Azure App Service](../app-service/app-service-value-prop-what-is.md) using an on-premises [Active Directory Federation Services](http://technet.microsoft.com/library/hh831502.aspx) as hello identity provider.</span></span> <span data-ttu-id="23575-106">V tomto scénáři můžete pracovat, když chcete toocreate-obchodní aplikace ve službě Azure App Service, ale vaše organizace vyžaduje toobe directory data uložená na místě.</span><span class="sxs-lookup"><span data-stu-id="23575-106">This scenario can work when you want toocreate line-of-business applications in Azure App Service but your organization requires directory data toobe stored on-site.</span></span>

> [!NOTE]
> <span data-ttu-id="23575-107">Přehled možností hello různých enterprise ověřování a autorizace pro službu Azure App Service naleznete v tématu [ověřit pomocí místní služby Active Directory v aplikaci Azure](web-sites-authentication-authorization.md).</span><span class="sxs-lookup"><span data-stu-id="23575-107">For an overview of hello different enterprise authentication and authorization options for Azure App Service, see [Authenticate with on-premises Active Directory in your Azure app](web-sites-authentication-authorization.md).</span></span>
> 
> 

<a name="bkmk_build"></a>

## <a name="what-you-will-build"></a><span data-ttu-id="23575-108">Co se sestavení</span><span class="sxs-lookup"><span data-stu-id="23575-108">What you will build</span></span>
<span data-ttu-id="23575-109">Vytvoříte základní aplikaci ASP.NET v Azure App Service Web Apps s hello následující funkce:</span><span class="sxs-lookup"><span data-stu-id="23575-109">You will build a basic ASP.NET application in Azure App Service Web Apps with hello following features:</span></span>

* <span data-ttu-id="23575-110">Ověřuje uživatele na základě služby AD FS</span><span class="sxs-lookup"><span data-stu-id="23575-110">Authenticates users against AD FS</span></span>
* <span data-ttu-id="23575-111">Používá `[Authorize]` tooauthorize uživatelů pro různé akce</span><span class="sxs-lookup"><span data-stu-id="23575-111">Uses `[Authorize]` tooauthorize users for different actions</span></span>
* <span data-ttu-id="23575-112">Konfigurace statické pro ladění v sadě Visual Studio i publikování tooApp Service Web Apps (konfigurace jednou, ladění a kdykoli publikování)</span><span class="sxs-lookup"><span data-stu-id="23575-112">Static configuration for both debugging in Visual Studio and publishing tooApp Service Web Apps (configure once, debug and publish anytime)</span></span>  

<a name="bkmk_need"></a>

## <a name="what-you-need"></a><span data-ttu-id="23575-113">Co potřebujete</span><span class="sxs-lookup"><span data-stu-id="23575-113">What you need</span></span>
[!INCLUDE [free-trial-note](../../includes/free-trial-note.md)]

<span data-ttu-id="23575-114">V tomto kurzu se třeba hello následující toocomplete:</span><span class="sxs-lookup"><span data-stu-id="23575-114">You need hello following toocomplete this tutorial:</span></span>

* <span data-ttu-id="23575-115">Místní nasazení služby AD FS (návod začátku do konce hello testovacím prostředí použili v tomto kurzu, najdete v části [testovacího prostředí: samostatné služby tokenů zabezpečení s AD FS ve virtuálním počítači Azure (pro test pouze)](https://blogs.msdn.microsoft.com/cephalin/2014/12/21/test-lab-standalone-sts-with-ad-fs-in-azure-vm-for-test-only/))</span><span class="sxs-lookup"><span data-stu-id="23575-115">An on-premises AD FS deployment (for an end-to-end walkthrough of hello test lab used in this tutorial, see [Test Lab: Standalone STS with AD FS in Azure VM (for test only)](https://blogs.msdn.microsoft.com/cephalin/2014/12/21/test-lab-standalone-sts-with-ad-fs-in-azure-vm-for-test-only/))</span></span>
* <span data-ttu-id="23575-116">Vztahy důvěryhodnosti předávající strany toocreate oprávnění v Správa služby AD FS</span><span class="sxs-lookup"><span data-stu-id="23575-116">Permissions toocreate relying party trusts in AD FS Management</span></span>
* <span data-ttu-id="23575-117">Visual Studio 2013 Update 4 nebo novější</span><span class="sxs-lookup"><span data-stu-id="23575-117">Visual Studio 2013 Update 4 or later</span></span>
* <span data-ttu-id="23575-118">[Azure SDK 2.8.1](http://go.microsoft.com/fwlink/p/?linkid=323510&clcid=0x409) nebo novější</span><span class="sxs-lookup"><span data-stu-id="23575-118">[Azure SDK 2.8.1](http://go.microsoft.com/fwlink/p/?linkid=323510&clcid=0x409) or later</span></span>

<a name="bkmk_sample"></a>

## <a name="use-sample-application-for-line-of-business-template"></a><span data-ttu-id="23575-119">Použití ukázkovou aplikaci pro obchodní – šablony</span><span class="sxs-lookup"><span data-stu-id="23575-119">Use sample application for line-of-business template</span></span>
<span data-ttu-id="23575-120">Hello ukázkovou aplikaci v tomto kurzu [WebApp. WSFederation DotNet)](https://github.com/AzureADSamples/WebApp-WSFederation-DotNet), vytvoří tým služby Azure Active Directory hello.</span><span class="sxs-lookup"><span data-stu-id="23575-120">hello sample application in this tutorial, [WebApp-WSFederation-DotNet)](https://github.com/AzureADSamples/WebApp-WSFederation-DotNet), is created by hello Azure Active Directory team.</span></span> <span data-ttu-id="23575-121">Vzhledem k tomu, že služba AD FS podporuje služby WS-Federation, můžete snadno použít jako šablony toocreate-obchodní aplikace.</span><span class="sxs-lookup"><span data-stu-id="23575-121">Since AD FS supports WS-Federation, you can use it as a template toocreate line-of-business applications with ease.</span></span> <span data-ttu-id="23575-122">Má hello následující funkce:</span><span class="sxs-lookup"><span data-stu-id="23575-122">It has hello following features:</span></span>

* <span data-ttu-id="23575-123">Používá [WS-Federation](http://msdn.microsoft.com/library/bb498017.aspx) tooauthenticate s místní nasazení služby AD FS</span><span class="sxs-lookup"><span data-stu-id="23575-123">Uses [WS-Federation](http://msdn.microsoft.com/library/bb498017.aspx) tooauthenticate with an on-premises AD FS deployment</span></span>
* <span data-ttu-id="23575-124">Funkce přihlášení a odhlášení</span><span class="sxs-lookup"><span data-stu-id="23575-124">Sign-in and sign-out functionality</span></span>
* <span data-ttu-id="23575-125">Používá [Microsoft.Owin](http://www.asp.net/aspnet/overview/owin-and-katana/an-overview-of-project-katana) (namísto technologie Windows Identity Foundation), který je hello budoucí technologie ASP.NET a mnohem jednodušší tooset pro ověřování a autorizace než WIF</span><span class="sxs-lookup"><span data-stu-id="23575-125">Uses [Microsoft.Owin](http://www.asp.net/aspnet/overview/owin-and-katana/an-overview-of-project-katana) (instead of Windows Identity Foundation), which is hello future of ASP.NET and much simpler tooset up for authentication and authorization than WIF</span></span>

<a name="bkmk_setup"></a>

## <a name="set-up-hello-sample-application"></a><span data-ttu-id="23575-126">Nastavit hello ukázkové aplikace</span><span class="sxs-lookup"><span data-stu-id="23575-126">Set up hello sample application</span></span>
1. <span data-ttu-id="23575-127">Klonovat nebo stáhnout hello ukázkové řešení v [WebApp. WSFederation DotNet](https://github.com/AzureADSamples/WebApp-WSFederation-DotNet) tooyour místního adresáře.</span><span class="sxs-lookup"><span data-stu-id="23575-127">Clone or download hello sample solution at [WebApp-WSFederation-DotNet](https://github.com/AzureADSamples/WebApp-WSFederation-DotNet) tooyour local directory.</span></span>
   
   > [!NOTE]
   > <span data-ttu-id="23575-128">Hello pokynů [README.md](https://github.com/AzureADSamples/WebApp-WSFederation-DotNet/blob/master/README.md) ukazují, jak tooset až hello aplikací s Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="23575-128">hello instructions at [README.md](https://github.com/AzureADSamples/WebApp-WSFederation-DotNet/blob/master/README.md) show you how tooset up hello application with Azure Active Directory.</span></span> <span data-ttu-id="23575-129">Ale v tomto kurzu, můžete ho nastavit s AD FS, tak místo toho postupujte podle zde uvedených kroků hello.</span><span class="sxs-lookup"><span data-stu-id="23575-129">But in this tutorial, you set it up with AD FS, so follow hello steps here instead.</span></span>
   > 
   > 
2. <span data-ttu-id="23575-130">Otevřete hello řešení a pak otevřete Controllers\AccountController.cs v hello **Průzkumníku řešení**.</span><span class="sxs-lookup"><span data-stu-id="23575-130">Open hello solution, and then open Controllers\AccountController.cs in hello **Solution Explorer**.</span></span>
   
   <span data-ttu-id="23575-131">Zobrazí se, že hello kód jednoduše problémy uživatele hello tooauthenticate výzvu ověřování pomocí protokolu WS-Federation.</span><span class="sxs-lookup"><span data-stu-id="23575-131">You will see that hello code simply issues an authentication challenge tooauthenticate hello user using WS-Federation.</span></span> <span data-ttu-id="23575-132">V App_Start\Startup.Auth.cs je nakonfigurované všechny ověřování.</span><span class="sxs-lookup"><span data-stu-id="23575-132">All authentication is configured in App_Start\Startup.Auth.cs.</span></span>
3. <span data-ttu-id="23575-133">Otevřete App_Start\Startup.Auth.cs.</span><span class="sxs-lookup"><span data-stu-id="23575-133">Open App_Start\Startup.Auth.cs.</span></span> <span data-ttu-id="23575-134">V hello `ConfigureAuth` metoda, Poznámka hello řádku:</span><span class="sxs-lookup"><span data-stu-id="23575-134">In hello `ConfigureAuth` method, note hello line:</span></span>
   
       app.UseWsFederationAuthentication(
           new WsFederationAuthenticationOptions
           {
               Wtrealm = realm,
               MetadataAddress = metadata                                      
           });
   
   <span data-ttu-id="23575-135">V hello, world OWIN tento fragment kódu je ve skutečnosti hello úplné že minimální potřebujete tooconfigure WS-Federation ověřování.</span><span class="sxs-lookup"><span data-stu-id="23575-135">In hello OWIN world, this snippet is really hello bare minimum you need tooconfigure WS-Federation authentication.</span></span> <span data-ttu-id="23575-136">Je mnohem jednodušší a více elegantní než WIF, kde je soubor Web.config vložit se souborem XML po celém hello místě.</span><span class="sxs-lookup"><span data-stu-id="23575-136">It is much simpler and more elegant than WIF, where Web.config is injected with XML all over hello place.</span></span> <span data-ttu-id="23575-137">Hello pouze informace, které potřebujete je hello předávající stranu pro identifikátor a hello adresa URL služby AD FS metadata souboru.</span><span class="sxs-lookup"><span data-stu-id="23575-137">hello only information you need is hello relying party's (RP) identifier and hello URL of your AD FS service's metadata file.</span></span> <span data-ttu-id="23575-138">Tady je příklad:</span><span class="sxs-lookup"><span data-stu-id="23575-138">Here's an example:</span></span>
   
   * <span data-ttu-id="23575-139">Identifikátor RP:`https://contoso.com/MyLOBApp`</span><span class="sxs-lookup"><span data-stu-id="23575-139">RP identifier: `https://contoso.com/MyLOBApp`</span></span>
   * <span data-ttu-id="23575-140">Metadata adresa:`http://adfs.contoso.com/FederationMetadata/2007-06/FederationMetadata.xml`</span><span class="sxs-lookup"><span data-stu-id="23575-140">Metadata address: `http://adfs.contoso.com/FederationMetadata/2007-06/FederationMetadata.xml`</span></span>
4. <span data-ttu-id="23575-141">V App_Start\Startup.Auth.cs změňte hello následující definice statických řetězec:</span><span class="sxs-lookup"><span data-stu-id="23575-141">In App_Start\Startup.Auth.cs, change hello following static string definitions:</span></span>  
   
   <pre class="prettyprint">
   private static string realm = ConfigurationManager.AppSettings["ida:<mark>RPIdentifier</mark>"];
   <mark><del>private static string aadInstance = ConfigurationManager.AppSettings["ida:AADInstance"];</del></mark>
   <mark><del>private static string tenant = ConfigurationManager.AppSettings["ida:Tenant"];</del></mark>
   <mark><del>private static string metadata = string.Format("{0}/{1}/federationmetadata/2007-06/federationmetadata.xml", aadInstance, tenant);</del></mark>
   <mark>private static string metadata = string.Format("https://{0}/federationmetadata/2007-06/federationmetadata.xml", ConfigurationManager.AppSettings["ida:ADFS"]);</mark>
   
   <mark><del>string authority = String.Format(CultureInfo.InvariantCulture, aadInstance, tenant);</del></mark>
   </pre>
5. <span data-ttu-id="23575-142">Nyní proveďte hello odpovídající změny v souboru Web.config. Otevřete soubor Web.config hello a upravte následující nastavení aplikace hello:</span><span class="sxs-lookup"><span data-stu-id="23575-142">Now, make hello corresponding changes in Web.config. Open hello Web.config and modify hello following app settings:</span></span>  
   
   <pre class="prettyprint">
   &lt;appSettings&gt;
   &lt;add key="webpages:Version" value="3.0.0.0" /&gt;
   &lt;add key="webpages:Enabled" value="false" /&gt;
   &lt;add key="ClientValidationEnabled" value="true" /&gt;
   &lt;add key="UnobtrusiveJavaScriptEnabled" value="true" /&gt;
   <mark><del>&lt;add key="ida:Wtrealm" value="[Enter hello App ID URI of WebApp-WSFederation-DotNet https://contoso.onmicrosoft.com/WebApp-WSFederation-DotNet]" /&gt;</del></mark>
   <mark><del>&lt;add key="ida:AADInstance" value="https://login.microsoftonline.com" /&gt;</del></mark>
   <mark><del>&lt;add key="ida:Tenant" value="[Enter tenant name, e.g. contoso.onmicrosoft.com]" /&gt;</del></mark>
   <mark>&lt;add key="ida:RPIdentifier" value="[Enter hello relying party identifier as configured in AD FS, e.g. https://localhost:44320/]" /&gt;</mark>
   <mark>&lt;add key="ida:ADFS" value="[Enter hello FQDN of AD FS service, e.g. adfs.contoso.com]" /&gt;</mark>
   
   &lt;/appSettings&gt;
   </pre>
   
   <span data-ttu-id="23575-143">Vyplňte hodnoty klíče hello podle příslušné prostředí.</span><span class="sxs-lookup"><span data-stu-id="23575-143">Fill in hello key values based on your respective environment.</span></span>
6. <span data-ttu-id="23575-144">Sestavení toomake aplikace hello se, že nejsou žádné chyby.</span><span class="sxs-lookup"><span data-stu-id="23575-144">Build hello application toomake sure there are no errors.</span></span>

<span data-ttu-id="23575-145">A je to.</span><span class="sxs-lookup"><span data-stu-id="23575-145">That's it.</span></span> <span data-ttu-id="23575-146">Hello vzorová aplikace je nyní připraven toowork se službou AD FS.</span><span class="sxs-lookup"><span data-stu-id="23575-146">Now hello sample application is ready toowork with AD FS.</span></span> <span data-ttu-id="23575-147">Stále potřebujete tooconfigure s tuto aplikaci ve službě AD FS později vztah důvěryhodnosti RP.</span><span class="sxs-lookup"><span data-stu-id="23575-147">You still need tooconfigure an RP trust with this application in AD FS later.</span></span>

<a name="bkmk_deploy"></a>

## <a name="deploy-hello-sample-application-tooazure-app-service-web-apps"></a><span data-ttu-id="23575-148">Nasazení hello ukázkové aplikace tooAzure App Service Web Apps</span><span class="sxs-lookup"><span data-stu-id="23575-148">Deploy hello sample application tooAzure App Service Web Apps</span></span>
<span data-ttu-id="23575-149">Zde můžete publikovat hello aplikace tooa webové aplikace v App Service Web Apps při zachování hello ladění prostředí.</span><span class="sxs-lookup"><span data-stu-id="23575-149">Here, you publish hello application tooa web app in App Service Web Apps while preserving hello debug environment.</span></span> <span data-ttu-id="23575-150">Všimněte si, že předtím, než ho má vztah důvěryhodnosti RP se službou AD FS, tak ověřování se stále nefunguje ještě budete toopublish hello aplikace.</span><span class="sxs-lookup"><span data-stu-id="23575-150">Note that you're going toopublish hello application before it has an RP trust with AD FS, so authentication still doesn't work yet.</span></span> <span data-ttu-id="23575-151">Ale pokud je to provést nyní můžete máte hello adresa URL webové aplikace, které můžete použít důvěryhodnosti RP tooconfigure hello později.</span><span class="sxs-lookup"><span data-stu-id="23575-151">However, if you do it now you can have hello web app URL that you can use tooconfigure hello RP trust later.</span></span>

1. <span data-ttu-id="23575-152">Klikněte pravým tlačítkem na projekt a vyberte **publikovat**.</span><span class="sxs-lookup"><span data-stu-id="23575-152">Right-click your project and select **Publish**.</span></span>
   
    ![](./media/web-sites-dotnet-lob-application-adfs/01-publish-website.png)
2. <span data-ttu-id="23575-153">Vyberte **služby Microsoft Azure App Service**.</span><span class="sxs-lookup"><span data-stu-id="23575-153">Select **Microsoft Azure App Service**.</span></span>
3. <span data-ttu-id="23575-154">Pokud nejste přihlášeni tooAzure, klikněte na tlačítko **přihlásit** hello účtu Microsoft a používat pro vaše předplatné Azure toosign v.</span><span class="sxs-lookup"><span data-stu-id="23575-154">If you haven't signed in tooAzure, click **Sign In** and use hello Microsoft account for your Azure subscription toosign in.</span></span>
4. <span data-ttu-id="23575-155">Jakmile se přihlásíte, klikněte na tlačítko **nový** toocreate webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="23575-155">Once signed in, click **New** toocreate a web app.</span></span>
5. <span data-ttu-id="23575-156">Vyplňte všechna povinná pole.</span><span class="sxs-lookup"><span data-stu-id="23575-156">Fill in all required fields.</span></span> <span data-ttu-id="23575-157">Chystáte tooconnect tooon místní data později, tak nevytvářejte databáze pro tuto webovou aplikaci.</span><span class="sxs-lookup"><span data-stu-id="23575-157">You are going tooconnect tooon-premises data later, so don't create a database for this web app.</span></span>
   
    ![](./media/web-sites-dotnet-lob-application-adfs/02-create-website.png)
6. <span data-ttu-id="23575-158">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="23575-158">Click **Create**.</span></span> <span data-ttu-id="23575-159">Po vytvoření webové aplikace hello dialogového okna Publikovat Web hello je otevřené.</span><span class="sxs-lookup"><span data-stu-id="23575-159">Once hello web app is created, hello Publish Web dialog is opened.</span></span>
7. <span data-ttu-id="23575-160">V **cílová adresa URL**, změňte **http** příliš**https**.</span><span class="sxs-lookup"><span data-stu-id="23575-160">In **Destination URL**, change **http** too**https**.</span></span> <span data-ttu-id="23575-161">Zkopírujte hello celou adresu URL tooa textový editor pro pozdější použití.</span><span class="sxs-lookup"><span data-stu-id="23575-161">Copy hello entire URL tooa text editor for later use.</span></span> <span data-ttu-id="23575-162">Potom klikněte na **publikovat**.</span><span class="sxs-lookup"><span data-stu-id="23575-162">Then, click **Publish**.</span></span>
   
    ![](./media/web-sites-dotnet-lob-application-adfs/03-destination-url.png)
8. <span data-ttu-id="23575-163">V sadě Visual Studio otevřete **Web.Release.config** ve vašem projektu.</span><span class="sxs-lookup"><span data-stu-id="23575-163">In Visual Studio, open **Web.Release.config** in your project.</span></span> <span data-ttu-id="23575-164">Vložte následující XML do hello hello `<configuration>` značky a nahraďte hodnotu klíče hello vaší webové aplikace publikovat adresu URL.</span><span class="sxs-lookup"><span data-stu-id="23575-164">Insert hello following XML into hello `<configuration>` tag, and replace hello key value with your publish web app's URL.</span></span>  
   
   <pre class="prettyprint">
   &lt;appSettings&gt;
   &lt;add key="ida:RPIdentifier" value="<mark>[e.g. https://mylobapp.azurewebsites.net/]</mark>" xdt:Transform="SetAttributes" xdt:Locator="Match(key)" /&gt;
   &lt;/appSettings&gt;</pre>

<span data-ttu-id="23575-165">Když jste hotovi, máte dva identifikátory RP nakonfigurované ve vašem projektu, jednu pro vaše prostředí ladění v sadě Visual Studio, a jeden pro hello publikované webové aplikace v Azure.</span><span class="sxs-lookup"><span data-stu-id="23575-165">When you're done, you have two RP identifiers configured in your project, one for your debug environment in Visual Studio, and one for hello published web app in Azure.</span></span> <span data-ttu-id="23575-166">Vztah důvěryhodnosti RP nastavíte pro každou ze dvou prostředí hello ve službě AD FS.</span><span class="sxs-lookup"><span data-stu-id="23575-166">You will set up an RP trust for each of hello two environments in AD FS.</span></span> <span data-ttu-id="23575-167">Během ladění, jsou nastavení aplikace hello v souboru Web.config použité toomake vaše **ladění** konfigurační práce se službou AD FS.</span><span class="sxs-lookup"><span data-stu-id="23575-167">During debugging, hello app settings in Web.config are used toomake your **Debug** configuration work with AD FS.</span></span> <span data-ttu-id="23575-168">Když je publikována (ve výchozím nastavení, hello **verze** konfigurace je publikována), transformovaných Web.config je načtený, která zahrnuje změny nastavení aplikace hello v Web.Release.config.</span><span class="sxs-lookup"><span data-stu-id="23575-168">When it's published (by default, hello **Release** configuration is published), a transformed Web.config is uploaded that incorporates hello app setting changes in Web.Release.config.</span></span>

<span data-ttu-id="23575-169">Pokud chcete tooattach hello publikované webové aplikace v Azure toohello ladicí program (tj. musíte nahrát symboly ladění kódu v hello publikované webové aplikace), můžete vytvořit klon hello konfiguraci pro Azure ladění ladění, ale s jeho vlastní vlastní Web.config transformace ( například Web.AzureDebug.config) používající nastavení aplikace hello z Web.Release.config. To vám umožní toomaintain konfiguraci statického hello různých prostředích.</span><span class="sxs-lookup"><span data-stu-id="23575-169">If you want tooattach hello published web app in Azure toohello debugger (i.e. you must upload debug symbols of your code in hello published web app), you can create a clone of hello Debug configuration for Azure debugging, but with its own custom Web.config transform (e.g. Web.AzureDebug.config) that uses hello app settings from Web.Release.config. This allows you toomaintain a static configuration across hello different environments.</span></span>

<a name="bkmk_rptrusts"></a>

## <a name="configure-relying-party-trusts-in-ad-fs-management"></a><span data-ttu-id="23575-170">Nakonfigurujte vztahy důvěryhodnosti předávající strany v Správa služby AD FS</span><span class="sxs-lookup"><span data-stu-id="23575-170">Configure relying party trusts in AD FS Management</span></span>
<span data-ttu-id="23575-171">Nyní je nutné tooconfigure v Správa služby AD FS vztah důvěryhodnosti RP, než bude možné použít ukázkovou aplikaci a ve skutečnosti ověření pomocí služby AD FS.</span><span class="sxs-lookup"><span data-stu-id="23575-171">Now you need tooconfigure an RP trust in AD FS Management before you can use your sample application and actually authenticate with AD FS.</span></span> <span data-ttu-id="23575-172">Budete potřebovat tooset až dva vztahy důvěryhodnosti samostatných RP, jeden pro vaše prostředí ladění a jeden pro publikované webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="23575-172">You will need tooset up two separate RP trusts, one for your debug environment and one for your published web app.</span></span>

> [!NOTE]
> <span data-ttu-id="23575-173">Zajistěte, aby zopakovat hello následujících kroků pro oba prostředí.</span><span class="sxs-lookup"><span data-stu-id="23575-173">Make sure that you repeat hello following steps for both of your environments.</span></span>
> 
> 

1. <span data-ttu-id="23575-174">Na serveru služby AD FS Přihlaste se pomocí přihlašovacích údajů, které mají tooAD rights management služby FS.</span><span class="sxs-lookup"><span data-stu-id="23575-174">On your AD FS server, log in with credentials that have management rights tooAD FS.</span></span>
2. <span data-ttu-id="23575-175">Otevřete správu služby AD FS.</span><span class="sxs-lookup"><span data-stu-id="23575-175">Open AD FS Management.</span></span> <span data-ttu-id="23575-176">Klikněte pravým tlačítkem na **AD FS\Trusted Relationships\Relying strany důvěřuje** a vyberte **přidat vztah důvěryhodnosti předávající strany**.</span><span class="sxs-lookup"><span data-stu-id="23575-176">Right-click **AD FS\Trusted Relationships\Relying Party Trusts** and select **Add Relying Party Trust**.</span></span>
   
   ![](./media/web-sites-dotnet-lob-application-adfs/1-add-rptrust.png)
3. <span data-ttu-id="23575-177">V hello **vybrat zdroj dat** vyberte **ručně zadat data o předávající straně hello**.</span><span class="sxs-lookup"><span data-stu-id="23575-177">In hello **Select Data Source** page, select **Enter data about hello relying party manually**.</span></span> 
   
   ![](./media/web-sites-dotnet-lob-application-adfs/2-enter-rp-manually.png)
4. <span data-ttu-id="23575-178">V hello **zadání zobrazovaného názvu** , zadejte zobrazovaný název pro hello aplikaci a klikněte na tlačítko **Další**.</span><span class="sxs-lookup"><span data-stu-id="23575-178">In hello **Specify Display Name** page, type a display name for hello application and click **Next**.</span></span>
5. <span data-ttu-id="23575-179">V hello **zvolte protokol** klikněte na tlačítko **Další**.</span><span class="sxs-lookup"><span data-stu-id="23575-179">In hello **Choose Protocol** page, click **Next**.</span></span>
6. <span data-ttu-id="23575-180">V hello **konfigurace certifikátu** klikněte na tlačítko **Další**.</span><span class="sxs-lookup"><span data-stu-id="23575-180">In hello **Configure Certificate** page, click **Next**.</span></span>
   
   > [!NOTE]
   > <span data-ttu-id="23575-181">Vzhledem k tomu, měli byste použít protokol HTTPS již, šifrovaných tokenů jsou volitelné.</span><span class="sxs-lookup"><span data-stu-id="23575-181">Since you should be using HTTPS already, encrypted tokens are optional.</span></span> <span data-ttu-id="23575-182">Pokud chcete skutečně tooencrypt tokeny od služby AD FS na této stránce, musíte taky přidat logiku dešifrování tokenů v kódu.</span><span class="sxs-lookup"><span data-stu-id="23575-182">If you really want tooencrypt tokens from AD FS on this page, you must also add token-decrypting logic in your code.</span></span> <span data-ttu-id="23575-183">Další informace najdete v tématu [ruční konfiguraci middlewaru OWIN WS-Federation a přijímat šifrované tokeny](http://chris.59north.com/post/2014/08/21/Manually-configuring-OWIN-WS-Federation-middleware-and-accepting-encrypted-tokens.aspx).</span><span class="sxs-lookup"><span data-stu-id="23575-183">For more information, see [Manually configuring OWIN WS-Federation middleware and accepting encrypted tokens](http://chris.59north.com/post/2014/08/21/Manually-configuring-OWIN-WS-Federation-middleware-and-accepting-encrypted-tokens.aspx).</span></span>
   > 
   > 
7. <span data-ttu-id="23575-184">Než můžete přesunout na další krok text hello, je třeba jednu část informací z projektu sady Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="23575-184">Before you move onto hello next step, you need one piece of information from your Visual Studio project.</span></span> <span data-ttu-id="23575-185">V okně Vlastnosti projektu hello, Všimněte si, hello **SSL URL** aplikace hello.</span><span class="sxs-lookup"><span data-stu-id="23575-185">In hello project properties, note hello **SSL URL** of hello application.</span></span> 
   
   ![](./media/web-sites-dotnet-lob-application-adfs/3-ssl-url.png)
8. <span data-ttu-id="23575-186">Zpět v Správa služby AD FS, v hello **konfigurace adresy URL** stránku hello **předávající strany Průvodce přidáním vztahu důvěryhodnosti**, vyberte **povolit podporu pasivního protokolu WS-Federation hello** a Zadejte v hello SSL URL projektu sady Visual Studio, že jste si poznamenali v předchozím kroku hello.</span><span class="sxs-lookup"><span data-stu-id="23575-186">Back in AD FS Management, in hello **Configure URL** page of hello **Add Relying Party Trust Wizard**, select **Enable support for hello WS-Federation Passive protocol** and type in hello SSL URL of your Visual Studio project that you noted in hello previous step.</span></span> <span data-ttu-id="23575-187">Pak klikněte na **Další**.</span><span class="sxs-lookup"><span data-stu-id="23575-187">Then, click **Next**.</span></span>
   
   ![](./media/web-sites-dotnet-lob-application-adfs/4-configure-url.png)
   
   > [!NOTE]
   > <span data-ttu-id="23575-188">Adresa URL určuje, kde toosend hello klienta po ověření úspěšné.</span><span class="sxs-lookup"><span data-stu-id="23575-188">URL specifies where toosend hello client after authentication succeeds.</span></span> <span data-ttu-id="23575-189">Pro prostředí hello ladění, by měla být <code>https://localhost:&lt;port&gt;/</code>.</span><span class="sxs-lookup"><span data-stu-id="23575-189">For hello debug environment, it should be <code>https://localhost:&lt;port&gt;/</code>.</span></span> <span data-ttu-id="23575-190">Pro hello publikované webové aplikaci mělo by být adresa URL webové aplikace hello.</span><span class="sxs-lookup"><span data-stu-id="23575-190">For hello published web app, it should be hello web app URL.</span></span>
   > 
   > 
9. <span data-ttu-id="23575-191">V hello **konfigurovat identifikátory** , zkontrolujte, že je již uvedené projektu adresy URL protokolu SSL a klikněte na tlačítko **Další**.</span><span class="sxs-lookup"><span data-stu-id="23575-191">In hello **Configure Identifiers** page, verify that your project SSL URL is already listed and click **Next**.</span></span> <span data-ttu-id="23575-192">Klikněte na tlačítko **Další** všechny hello toohello způsob, jak na konci průvodce hello s výchozí možnosti.</span><span class="sxs-lookup"><span data-stu-id="23575-192">Click **Next** all hello way toohello end of hello wizard with default selections.</span></span>
   
   > [!NOTE]
   > <span data-ttu-id="23575-193">V App_Start\Startup.Auth.cs projektu sady Visual Studio, je tento identifikátor porovnání hodnota hello <code>WsFederationAuthenticationOptions.Wtrealm</code> během federovaného ověřování.</span><span class="sxs-lookup"><span data-stu-id="23575-193">In App_Start\Startup.Auth.cs of your Visual Studio project, this identifier is matched against hello value of <code>WsFederationAuthenticationOptions.Wtrealm</code> during federated authentication.</span></span> <span data-ttu-id="23575-194">Ve výchozím nastavení přidá se jako identifikátor RP adresa URL aplikace hello hello v předchozím kroku.</span><span class="sxs-lookup"><span data-stu-id="23575-194">By default, hello application's URL from hello previous step is added as an RP identifier.</span></span>
   > 
   > 
10. <span data-ttu-id="23575-195">Nyní dokončení konfigurace hello RP aplikací pro váš projekt ve službě AD FS.</span><span class="sxs-lookup"><span data-stu-id="23575-195">You have now finished configuring hello RP application for your project in AD FS.</span></span> <span data-ttu-id="23575-196">V dalším kroku nakonfigurujte tuto aplikaci toosend hello deklarace identity potřeby vaší aplikací.</span><span class="sxs-lookup"><span data-stu-id="23575-196">Next, you configure this application toosend hello claims needed by your application.</span></span> <span data-ttu-id="23575-197">Hello **upravit pravidla deklarací identity** dialogu je ve výchozím nastavení pro vás otevřen na konci hello hello průvodce, abyste mohli začít okamžitě.</span><span class="sxs-lookup"><span data-stu-id="23575-197">hello **Edit Claim Rules** dialog is opened by default for you at hello end of hello wizard so you can start immediately.</span></span> <span data-ttu-id="23575-198">Umožňuje nakonfigurovat alespoň hello následující deklarace identity (s schémata v závorkách):</span><span class="sxs-lookup"><span data-stu-id="23575-198">Let's configure at least hello following claims (with schemas in parentheses):</span></span>
    
    * <span data-ttu-id="23575-199">Název (http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name) - používaný ASP.NET toohydrate `User.Identity.Name`.</span><span class="sxs-lookup"><span data-stu-id="23575-199">Name (http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name) - used by ASP.NET toohydrate `User.Identity.Name`.</span></span>
    * <span data-ttu-id="23575-200">Hlavní název (http://schemas.xmlsoap.org/ws/2005/05/identity/claims/upn) - použít toouniquely uživatele identifikaci uživatelů v organizaci hello.</span><span class="sxs-lookup"><span data-stu-id="23575-200">User principal name (http://schemas.xmlsoap.org/ws/2005/05/identity/claims/upn) - used toouniquely identify users in hello organization.</span></span>
    * <span data-ttu-id="23575-201">Členství ve skupinách jako pro role (http://schemas.microsoft.com/ws/2008/06/identity/claims/role) - lze použít s `[Authorize(Roles="role1, role2,...")]` decoration tooauthorize řadiče nebo akce.</span><span class="sxs-lookup"><span data-stu-id="23575-201">Group memberships as roles (http://schemas.microsoft.com/ws/2008/06/identity/claims/role) - can be used with `[Authorize(Roles="role1, role2,...")]` decoration tooauthorize controllers/actions.</span></span> <span data-ttu-id="23575-202">Ve skutečnosti nemusí tento přístup hello většina původce pro povolení role.</span><span class="sxs-lookup"><span data-stu-id="23575-202">In reality, this approach may not be hello most performant for role authorization.</span></span> <span data-ttu-id="23575-203">Pokud vaši uživatelé AD patří toohundreds skupin zabezpečení, budou stovky deklarace rolí v tokenu SAML hello.</span><span class="sxs-lookup"><span data-stu-id="23575-203">If your AD users belong toohundreds of security groups, they become hundreds of role claims in hello SAML token.</span></span> <span data-ttu-id="23575-204">Alternativní způsob je toosend jedné role deklarací podmíněně v závislosti na členství hello uživatele v dané skupině.</span><span class="sxs-lookup"><span data-stu-id="23575-204">An alternative approach is toosend a single role claim conditionally depending on hello user's membership in a particular group.</span></span> <span data-ttu-id="23575-205">Ale jsme budete jednoduchost pro účely tohoto kurzu.</span><span class="sxs-lookup"><span data-stu-id="23575-205">However, we'll keep it simple for this tutorial.</span></span>
    * <span data-ttu-id="23575-206">Název ID (http://schemas.xmlsoap.org/ws/2005/05/identity/claims/nameidentifier) – lze použít pro ověření proti padělání.</span><span class="sxs-lookup"><span data-stu-id="23575-206">Name ID (http://schemas.xmlsoap.org/ws/2005/05/identity/claims/nameidentifier) - can be used for anti-forgery validation.</span></span> <span data-ttu-id="23575-207">Další informace o toomake ho fungování s proti padělání ověřování najdete v tématu hello **přidat funkce – obchodní** části [vytvoření-obchodní aplikace pro Azure pomocí ověřování Azure Active Directory ](web-sites-dotnet-lob-application-azure-ad.md#bkmk_crud).</span><span class="sxs-lookup"><span data-stu-id="23575-207">For more information on how toomake it work with anti-forgery validation, see hello **Add line-of-business functionality** section of [Create a line-of-business Azure app with Azure Active Directory authentication](web-sites-dotnet-lob-application-azure-ad.md#bkmk_crud).</span></span>
    
    > [!NOTE]
    > <span data-ttu-id="23575-208">Hello deklarace identity typy budete potřebovat tooconfigure pro vaše aplikace je určen podle potřeb vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="23575-208">hello claim types you need tooconfigure for your application is determined by your application's needs.</span></span> <span data-ttu-id="23575-209">Hello seznam deklarací identity, které podporuje aplikace Azure Active Directory (tj. vztahy důvěryhodnosti RP), například naleznete v části [podporované tokenu a typy deklarací identity](http://msdn.microsoft.com/library/azure/dn195587.aspx).</span><span class="sxs-lookup"><span data-stu-id="23575-209">For hello list of claims supported by Azure Active Directory applications (i.e. RP trusts), for example, see [Supported Token and Claim Types](http://msdn.microsoft.com/library/azure/dn195587.aspx).</span></span>
    > 
    > 
11. <span data-ttu-id="23575-210">V dialogovém okně upravit pravidla deklarací identity hello, klikněte na tlačítko **přidat pravidlo**.</span><span class="sxs-lookup"><span data-stu-id="23575-210">In hello Edit Claim Rules dialog, click **Add Rule**.</span></span>
12. <span data-ttu-id="23575-211">Nakonfigurujte hello název UPN a role deklarací identity, jak je znázorněno v hello – snímek obrazovky a klikněte na **Dokončit**.</span><span class="sxs-lookup"><span data-stu-id="23575-211">Configure hello name, UPN, and role claims as shown in hello screenshot and click **Finish**.</span></span>
    
    ![](./media/web-sites-dotnet-lob-application-adfs/5-ldap-claims.png)
    
    <span data-ttu-id="23575-212">Dále vytvoříte přechodný název ID deklarace identity pomocí kroků hello ukázáno v [identifikátory názvů v kontrolní výrazy SAML](http://blogs.msdn.com/b/card/archive/2010/02/17/name-identifiers-in-saml-assertions.aspx).</span><span class="sxs-lookup"><span data-stu-id="23575-212">Next, you create a transient name ID claim using hello steps demonstrated in [Name Identifiers in SAML assertions](http://blogs.msdn.com/b/card/archive/2010/02/17/name-identifiers-in-saml-assertions.aspx).</span></span>
13. <span data-ttu-id="23575-213">Klikněte na tlačítko **přidat pravidlo** znovu.</span><span class="sxs-lookup"><span data-stu-id="23575-213">Click **Add Rule** again.</span></span>
14. <span data-ttu-id="23575-214">Vyberte **odesílat deklarace pomocí vlastního pravidla** a klikněte na tlačítko **Další**.</span><span class="sxs-lookup"><span data-stu-id="23575-214">Select **Send Claims Using a Custom Rule** and click **Next**.</span></span>
15. <span data-ttu-id="23575-215">Vložení hello následující pravidlo jazyk do hello **vlastní pravidlo** pole, název hello pravidlo **za identifikátor relace** a klikněte na tlačítko **Dokončit**.</span><span class="sxs-lookup"><span data-stu-id="23575-215">Paste hello following rule language into hello **Custom rule** box, name hello rule **Per Session Identifier** and click **Finish**.</span></span>  
    
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
    
    <span data-ttu-id="23575-216">Vlastní pravidlo by měl vypadat takto:</span><span class="sxs-lookup"><span data-stu-id="23575-216">Your custom rule should look like this screenshot:</span></span>
    
    ![](./media/web-sites-dotnet-lob-application-adfs/6-per-session-identifier.png)
16. <span data-ttu-id="23575-217">Klikněte na tlačítko **přidat pravidlo** znovu.</span><span class="sxs-lookup"><span data-stu-id="23575-217">Click **Add Rule** again.</span></span>
17. <span data-ttu-id="23575-218">Vyberte **transformovat příchozí deklarace** a klikněte na tlačítko **Další**.</span><span class="sxs-lookup"><span data-stu-id="23575-218">Select **Transform an Incoming Claim** and click **Next**.</span></span>
18. <span data-ttu-id="23575-219">Konfigurace hello pravidla, jak je znázorněno na snímku obrazovky hello (pomocí typu deklarace identity hello jste vytvořili v vlastní pravidlo hello) a klikněte na tlačítko **Dokončit**.</span><span class="sxs-lookup"><span data-stu-id="23575-219">Configure hello rule as shown in hello screenshot (using hello claim type you created in hello custom rule) and click **Finish**.</span></span>
    
    ![](./media/web-sites-dotnet-lob-application-adfs/7-transient-name-id.png)
    
    <span data-ttu-id="23575-220">Podrobné informace o hello kroky pro přechodný deklarace ID názvu hello najdete v tématu [identifikátory názvů v kontrolní výrazy SAML](http://blogs.msdn.com/b/card/archive/2010/02/17/name-identifiers-in-saml-assertions.aspx).</span><span class="sxs-lookup"><span data-stu-id="23575-220">For detailed information on hello steps for hello transient Name ID claim, see [Name Identifiers in SAML assertions](http://blogs.msdn.com/b/card/archive/2010/02/17/name-identifiers-in-saml-assertions.aspx).</span></span>
19. <span data-ttu-id="23575-221">Klikněte na tlačítko **použít** v hello **upravit pravidla deklarací identity** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="23575-221">Click **Apply** in hello **Edit Claim Rules** dialog.</span></span> <span data-ttu-id="23575-222">Teď by měl vypadat jako hello následující snímek obrazovky:</span><span class="sxs-lookup"><span data-stu-id="23575-222">It should now look like hello following screenshot:</span></span>
    
    ![](./media/web-sites-dotnet-lob-application-adfs/8-all-claim-rules.png)
    
    > [!NOTE]
    > <span data-ttu-id="23575-223">Znovu zkontrolujte, zda tento postup opakujte pro vaše prostředí ladění a publikovanou webovou aplikací.</span><span class="sxs-lookup"><span data-stu-id="23575-223">Again, make sure that you repeat these steps for both your debug environment and published web app.</span></span>
    > 
    > 

<a name="bkmk_test"></a>

## <a name="test-federated-authentication-for-your-application"></a><span data-ttu-id="23575-224">Test federovaného ověřování pro aplikaci</span><span class="sxs-lookup"><span data-stu-id="23575-224">Test federated authentication for your application</span></span>
<span data-ttu-id="23575-225">Můžete je připraven tootest logiku ověřování aplikace u služby AD FS.</span><span class="sxs-lookup"><span data-stu-id="23575-225">You are ready tootest your application's authentication logic against AD FS.</span></span> <span data-ttu-id="23575-226">V mé testovacího prostředí služby AD FS je nutné testovacího uživatele, který patří tooa testovací skupiny ve službě Active Directory (AD).</span><span class="sxs-lookup"><span data-stu-id="23575-226">In my AD FS lab environment, I have a test user that belongs tooa test group in Active Directory (AD).</span></span>

![](./media/web-sites-dotnet-lob-application-adfs/10-test-user-and-group.png)

<span data-ttu-id="23575-227">tootest ověřování v ladicím programu hello, stačí toodo nyní je typ `F5`.</span><span class="sxs-lookup"><span data-stu-id="23575-227">tootest authentication in hello debugger, all you need toodo now is type `F5`.</span></span> <span data-ttu-id="23575-228">Pokud chcete ověřování tootest v hello publikované webové aplikaci, přejděte toohello adresy URL.</span><span class="sxs-lookup"><span data-stu-id="23575-228">If you want tootest authentication in hello published web app, navigate toohello URL.</span></span>

<span data-ttu-id="23575-229">Po načtení hello webové aplikace, klikněte na tlačítko **přihlásit**.</span><span class="sxs-lookup"><span data-stu-id="23575-229">After hello web application loads, click **Sign In**.</span></span> <span data-ttu-id="23575-230">Měli byste obdržet teď buď přihlašovací dialogové okno nebo hello přihlašovací stránky obsloužených služby AD FS, v závislosti na metodě ověřování hello vybrané službou AD FS.</span><span class="sxs-lookup"><span data-stu-id="23575-230">You should now get either a login dialog or hello login page served by AD FS, depending on hello authentication method chosen by AD FS.</span></span> <span data-ttu-id="23575-231">Zde je zobrazila v Internet Exploreru 11.</span><span class="sxs-lookup"><span data-stu-id="23575-231">Here's what I get in Internet Explorer 11.</span></span>

![](./media/web-sites-dotnet-lob-application-adfs/9-test-debugging.png)

<span data-ttu-id="23575-232">Jakmile se můžete přihlásit jako uživatel v doméně hello AD nasazení hello AD FS, měli byste vidět domovskou stránku hello znovu s **Hello, <User Name>!**</span><span class="sxs-lookup"><span data-stu-id="23575-232">Once you log in with a user in hello AD domain of hello AD FS deployment, you should now see hello homepage again with **Hello, <User Name>!**</span></span> <span data-ttu-id="23575-233">v horním hello.</span><span class="sxs-lookup"><span data-stu-id="23575-233">in hello corner.</span></span> <span data-ttu-id="23575-234">Zde je možné získat.</span><span class="sxs-lookup"><span data-stu-id="23575-234">Here's what I get.</span></span>

![](./media/web-sites-dotnet-lob-application-adfs/11-test-debugging-success.png)

<span data-ttu-id="23575-235">Dosavadní práce můžete po úspěšném hello následující způsoby:</span><span class="sxs-lookup"><span data-stu-id="23575-235">So far, you've succeeded in hello following ways:</span></span>

* <span data-ttu-id="23575-236">Vaše aplikace úspěšně dosáhl služby AD FS a identifikátor odpovídající RP se nachází v hello databáze služby AD FS</span><span class="sxs-lookup"><span data-stu-id="23575-236">Your application has successfully reached AD FS and a matching RP identifier is found in hello AD FS database</span></span>
* <span data-ttu-id="23575-237">Služby AD FS byl úspěšně ověřen AD uživatele a přesměrování zpět toohello aplikace domovské stránky</span><span class="sxs-lookup"><span data-stu-id="23575-237">AD FS has successfully authenticated an AD user and redirect you back toohello application's homepage</span></span>
* <span data-ttu-id="23575-238">Služby AD FS jako název úspěšně poslala hello deklarace identity aplikace tooyour (http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name), jak je uvedeno hello fakt tento uživatel hello název se zobrazí v hello rohu.</span><span class="sxs-lookup"><span data-stu-id="23575-238">AD FS as successfully sent hello name claim (http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name) tooyour application, as indicated by hello fact that hello user name is displayed in hello corner.</span></span> 

<span data-ttu-id="23575-239">Pokud deklarace identity názvu hello chybí, jste viděli by **Hello,!**.</span><span class="sxs-lookup"><span data-stu-id="23575-239">If hello name claim is missing, you would have seen **Hello, !**.</span></span> <span data-ttu-id="23575-240">Pokud se podíváte na Views\Shared\_LoginPartial.cshtml, zjistíte, že používá `User.Identity.Name` toodisplay hello uživatelské jméno.</span><span class="sxs-lookup"><span data-stu-id="23575-240">If you look at Views\Shared\_LoginPartial.cshtml, you find that it uses `User.Identity.Name` toodisplay hello user name.</span></span> <span data-ttu-id="23575-241">Jak je uvedeno nahoře, pokud název hello deklarací z hello ověření uživatele je k dispozici v tokenu SAML hello, ASP.NET hydráty tato vlastnost s ním.</span><span class="sxs-lookup"><span data-stu-id="23575-241">As mentioned previously, if hello name claim of hello authenticated user is available in hello SAML token, ASP.NET hydrates this property with it.</span></span> <span data-ttu-id="23575-242">toosee, které všechny hello deklarací, že jsou odeslány službou AD FS, do hello umístit zarážky v Controllers\HomeController.cs, metoda akce indexu.</span><span class="sxs-lookup"><span data-stu-id="23575-242">toosee all hello claims that are sent by AD FS, put a breakpoint in Controllers\HomeController.cs, in hello Index action method.</span></span> <span data-ttu-id="23575-243">Po ověření uživatele hello zkontrolovat hello `System.Security.Claims.Current.Claims` kolekce.</span><span class="sxs-lookup"><span data-stu-id="23575-243">After hello user is authenticated, inspect hello `System.Security.Claims.Current.Claims` collection.</span></span>

![](./media/web-sites-dotnet-lob-application-adfs/12-test-debugging-all-claims.png) 

<a name="bkmk_authorize"></a>

## <a name="authorize-users-for-specific-controllers-or-actions"></a><span data-ttu-id="23575-244">Autorizace uživatelů pro konkrétní řadiče nebo akce</span><span class="sxs-lookup"><span data-stu-id="23575-244">Authorize users for specific controllers or actions</span></span>
<span data-ttu-id="23575-245">Vzhledem k tomu, že jste zahrnuli členství ve skupinách jako deklarace identity role ve vaší konfiguraci RP vztahu důvěryhodnosti, teď můžete například vytvořit přímo v hello `[Authorize(Roles="...")]` decoration pro kontrolery a akce.</span><span class="sxs-lookup"><span data-stu-id="23575-245">Since you have included group memberships as role claims in your RP trust configuration, you can now use them directly in hello `[Authorize(Roles="...")]` decoration for controllers and actions.</span></span> <span data-ttu-id="23575-246">V-obchodní aplikace s hello vytvořit-čtení-aktualizace-odstranění (CRUD) vzor můžete povolit konkrétní role tooaccess každá akce.</span><span class="sxs-lookup"><span data-stu-id="23575-246">In a line-of-business application with hello Create-Read-Update-Delete (CRUD) pattern, you can authorize specific roles tooaccess each action.</span></span> <span data-ttu-id="23575-247">Prozatím se právě vyzkoušejte tuto funkci na existující řadič domovské hello.</span><span class="sxs-lookup"><span data-stu-id="23575-247">For now, you will just try out this feature on hello existing Home controller.</span></span>

1. <span data-ttu-id="23575-248">Otevřete Controllers\HomeController.cs.</span><span class="sxs-lookup"><span data-stu-id="23575-248">Open Controllers\HomeController.cs.</span></span>
2. <span data-ttu-id="23575-249">Uspořádání hello `About` a `Contact` akce metody podobné toohello následující kód, pomocí členství ve skupinách zabezpečení, které má ověřeného uživatele.</span><span class="sxs-lookup"><span data-stu-id="23575-249">Decorate hello `About` and `Contact` action methods similar toohello following code, using security group memberships that your authenticated user has.</span></span>  
   
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
   
    <span data-ttu-id="23575-250">Vzhledem k tomu, že po přidání **testovacího uživatele** příliš**testovací skupina** v mé testovacího prostředí služby AD FS I budete používat testovací skupina tootest autorizace na `About`.</span><span class="sxs-lookup"><span data-stu-id="23575-250">Since I added **Test User** too**Test Group** in my AD FS lab environment, I'll use Test Group tootest authorization on `About`.</span></span> <span data-ttu-id="23575-251">Pro `Contact`, I budete testovat hello malá záporné **Domain Admins**, toowhich **testovací uživatele** nepatří.</span><span class="sxs-lookup"><span data-stu-id="23575-251">For `Contact`, I'll test hello negative case of **Domain Admins**, toowhich **Test User** doesn't belong.</span></span>
3. <span data-ttu-id="23575-252">Spuštění ladicího programu hello zadáním `F5` a přihlaste se a pak klikněte na **o**.</span><span class="sxs-lookup"><span data-stu-id="23575-252">Start hello debugger by typing `F5` and sign in, then click **About**.</span></span> <span data-ttu-id="23575-253">Má teď se zobrazil hello `~/About/Index` stránka úspěšně, pokud ověřený uživatel má oprávnění pro tuto akci.</span><span class="sxs-lookup"><span data-stu-id="23575-253">You should now be viewing hello `~/About/Index` page successfully, if your authenticated user is authorized for that action.</span></span>
4. <span data-ttu-id="23575-254">Klikněte na tlačítko **kontaktujte**, v mém případě by neměl Autorizovat **testovacího uživatele** pro akci hello.</span><span class="sxs-lookup"><span data-stu-id="23575-254">Now click **Contact**, which in my case should not authorize **Test User** for hello action.</span></span> <span data-ttu-id="23575-255">Prohlížeč hello je však přesměrovaného tooAD služby FS, který nakonec zobrazí tato zpráva:</span><span class="sxs-lookup"><span data-stu-id="23575-255">However, hello browser is redirected tooAD FS, which eventually shows this message:</span></span>
   
    ![](./media/web-sites-dotnet-lob-application-adfs/13-authorize-adfs-error.png)
   
    <span data-ttu-id="23575-256">Pokud byste prozkoumat této chybě v prohlížeči událostí na hello serveru služby AD FS, zobrazí tato zpráva o výjimce:</span><span class="sxs-lookup"><span data-stu-id="23575-256">If you investigate this error in Event Viewer on hello AD FS server, you see this exception message:</span></span>  
   
    <pre class="prettyprint">
    Microsoft.IdentityServer.Web.InvalidRequestException: MSIS7042: <mark>hello same client browser session has made '6' requests in hello last '11' seconds.</mark> Contact your administrator for details.
       at Microsoft.IdentityServer.Web.Protocols.PassiveProtocolHandler.UpdateLoopDetectionCookie(WrappedHttpListenerContext context)
       at Microsoft.IdentityServer.Web.Protocols.WSFederation.WSFederationProtocolHandler.SendSignInResponse(WSFederationContext context, MSISSignInResponse response)
       at Microsoft.IdentityServer.Web.PassiveProtocolListener.ProcessProtocolRequest(ProtocolContext protocolContext, PassiveProtocolHandler protocolHandler)
       at Microsoft.IdentityServer.Web.PassiveProtocolListener.OnGetContext(WrappedHttpListenerContext context)
    </pre>
   
    <span data-ttu-id="23575-257">Hello důvodem této chyby je, že ve výchozím nastavení, MVC vrátí 401 Neautorizováno, pokud nemáte oprávnění role uživatele.</span><span class="sxs-lookup"><span data-stu-id="23575-257">hello reason for this error is that by default, MVC returns a 401 Unauthorized when a user's roles are not authorized.</span></span> <span data-ttu-id="23575-258">Tím se spustí opětovné ověření požadavku tooyour zprostředkovatele identity (AD FS).</span><span class="sxs-lookup"><span data-stu-id="23575-258">This triggers a reauthentication request tooyour identity provider (AD FS).</span></span> <span data-ttu-id="23575-259">Vzhledem k tomu, že uživatel hello je již ověřen, služba AD FS vrátí toohello stejné stránky, pak vydá jiné 401, vytváření smyčku přesměrování.</span><span class="sxs-lookup"><span data-stu-id="23575-259">Since hello user is already authenticated, AD FS returns toohello same page, which then issues another 401, creating a redirect loop.</span></span> <span data-ttu-id="23575-260">Přepíše třídy AuthorizeAttribute na `HandleUnauthorizedRequest` metoda s jednoduché logiku tooshow něco, co má smysl místo trvalého přesměrování smyčky hello.</span><span class="sxs-lookup"><span data-stu-id="23575-260">You will override AuthorizeAttribute's `HandleUnauthorizedRequest` method with simple logic tooshow something that makes sense instead of continuing hello redirect loop.</span></span>
5. <span data-ttu-id="23575-261">Vytvořte soubor v hello projekt s názvem AuthorizeAttribute.cs a hello vložte následující kód do ní.</span><span class="sxs-lookup"><span data-stu-id="23575-261">Create a file in hello project called AuthorizeAttribute.cs, and paste hello following code into it.</span></span>
   
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
   
    <span data-ttu-id="23575-262">Hello přepsat kód zasílá protokolu HTTP 403 (zakázáno) místo protokolu HTTP 401 (Neautorizováno) v případech ověřený, ale neoprávněným.</span><span class="sxs-lookup"><span data-stu-id="23575-262">hello override code sends an HTTP 403 (Forbidden) instead of HTTP 401 (Unauthorized) in  authenticated but unauthorized cases.</span></span>
6. <span data-ttu-id="23575-263">Spuštění ladicího programu hello znovu s `F5`.</span><span class="sxs-lookup"><span data-stu-id="23575-263">Run hello debugger again with `F5`.</span></span> <span data-ttu-id="23575-264">Kliknutím na tlačítko **kontaktujte** nyní zobrazuje informativnější (i když rozdělení nežádoucí) chybovou zprávu:</span><span class="sxs-lookup"><span data-stu-id="23575-264">Clicking **Contact** now shows a more informative (albeit unattractive) error message:</span></span>
   
    ![](./media/web-sites-dotnet-lob-application-adfs/14-unauthorized-forbidden.png)
7. <span data-ttu-id="23575-265">Znovu publikovat tooAzure aplikace hello App Service Web Apps a testování hello chování hello živé aplikace.</span><span class="sxs-lookup"><span data-stu-id="23575-265">Publish hello application tooAzure App Service Web Apps again, and test hello behavior of hello live application.</span></span>

<a name="bkmk_data"></a>

## <a name="connect-tooon-premises-data"></a><span data-ttu-id="23575-266">Připojení místní tooon dat</span><span class="sxs-lookup"><span data-stu-id="23575-266">Connect tooon-premises data</span></span>
<span data-ttu-id="23575-267">Zda je vhodnější tooimplement-obchodní aplikace se službou AD FS místo Azure Active Directory je důvod problémy s dodržováním předpisů s udržováním organizace dat mimo místní.</span><span class="sxs-lookup"><span data-stu-id="23575-267">A reason that you would want tooimplement your line-of-business application with AD FS instead of Azure Active Directory is compliance issues with keeping organization data off-premise.</span></span> <span data-ttu-id="23575-268">Také to může znamenat, že webové aplikace v Azure musí přístup k místní databáze, vzhledem k tomu, že nejsou povoleny toouse [SQL Database](/services/sql-database/) jako hello datové vrstvy pro webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="23575-268">This may also mean that your web app in Azure must access on-premises databases, since you are not allowed toouse [SQL Database](/services/sql-database/) as hello data tier for your web apps.</span></span>

<span data-ttu-id="23575-269">Azure App Service Web Apps podporuje přístupu k místní databáze s dva přístupy: [hybridní připojení](../biztalk-services/integration-hybrid-connection-overview.md) a [virtuální sítě](web-sites-integrate-with-vnet.md).</span><span class="sxs-lookup"><span data-stu-id="23575-269">Azure App Service Web Apps supports accessing on-premises databases with two approaches: [Hybrid Connections](../biztalk-services/integration-hybrid-connection-overview.md) and [Virtual Networks](web-sites-integrate-with-vnet.md).</span></span> <span data-ttu-id="23575-270">Další informace najdete v tématu [integrace pomocí virtuální sítě a hybridních připojení s Azure App Service Web Apps](https://azure.microsoft.com/blog/2014/10/30/using-vnet-or-hybrid-conn-with-websites/).</span><span class="sxs-lookup"><span data-stu-id="23575-270">For more information, see [Using VNET integration and Hybrid connections with Azure App Service Web Apps](https://azure.microsoft.com/blog/2014/10/30/using-vnet-or-hybrid-conn-with-websites/).</span></span>

<a name="bkmk_resources"></a>

## <a name="further-resources"></a><span data-ttu-id="23575-271">Další prostředky</span><span class="sxs-lookup"><span data-stu-id="23575-271">Further resources</span></span>
* [<span data-ttu-id="23575-272">Ověření pomocí místní služby Active Directory v aplikaci Azure</span><span class="sxs-lookup"><span data-stu-id="23575-272">Authenticate with on-premises Active Directory in your Azure app</span></span>](web-sites-authentication-authorization.md)
* [<span data-ttu-id="23575-273">Vytvoření aplikace Azure-obchodní pomocí ověřování Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="23575-273">Create a line-of-business Azure app with Azure Active Directory authentication</span></span>](web-sites-dotnet-lob-application-azure-ad.md)
* [<span data-ttu-id="23575-274">Použití hello místní organizace ověřování možnost (ADFS) s ASP.NET v sadě Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="23575-274">Use hello On-Premises Organizational Authentication Option (ADFS) With ASP.NET in Visual Studio 2013</span></span>](http://www.cloudidentity.com/blog/2014/02/12/use-the-on-premises-organizational-authentication-option-adfs-with-asp-net-in-visual-studio-2013/)
* [<span data-ttu-id="23575-275">Migrace tooKatana VS2013 webového projektu ze WIF</span><span class="sxs-lookup"><span data-stu-id="23575-275">Migrate a VS2013 Web Project From WIF tooKatana</span></span>](http://www.cloudidentity.com/blog/2014/09/15/MIGRATE-A-VS2013-WEB-PROJECT-FROM-WIF-TO-KATANA/)
* [<span data-ttu-id="23575-276">Přehled služby Active Directory Federation Services</span><span class="sxs-lookup"><span data-stu-id="23575-276">Active Directory Federation Services Overview</span></span>](http://technet.microsoft.com/library/hh831502.aspx)
* [<span data-ttu-id="23575-277">Specifikace protokolu WS-Federation 1.1</span><span class="sxs-lookup"><span data-stu-id="23575-277">WS-Federation 1.1 specification</span></span>](http://download.boulder.ibm.com/ibmdl/pub/software/dw/specs/ws-fed/WS-Federation-V1-1B.pdf?S_TACT=105AGX04&S_CMP=LP)

