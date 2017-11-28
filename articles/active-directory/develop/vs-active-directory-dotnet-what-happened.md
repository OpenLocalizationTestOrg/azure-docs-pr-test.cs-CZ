---
title: "aaaChanges provedené tooa MVC projektu, jakmile se připojíte tooAzure AD | Microsoft Docs"
description: "Popisuje, co se stane projektu MVC tooyour, jakmile se připojíte pomocí sady Visual Studio připojené služby tooAzure AD"
services: active-directory
documentationcenter: na
author: kraigb
manager: ghogen
editor: 
ms.assetid: 8b24adde-547e-4ffe-824a-2029ba210216
ms.service: active-directory
ms.workload: web
ms.tgt_pltfrm: vs-what-happened
ms.devlang: na
ms.topic: article
ms.date: 03/01/2017
ms.author: kraigb
ms.custom: aaddev
ms.openlocfilehash: 5e6d4ce5331eacca5fc83429017ae454fadcc8e4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="what-happened-toomy-mvc-project-visual-studio-azure-active-directory-connected-service"></a><span data-ttu-id="edbd6-103">Co se stalo toomy MVC projektu (Visual Studio Azure Active Directory připojeno service)?</span><span class="sxs-lookup"><span data-stu-id="edbd6-103">What happened toomy MVC project (Visual Studio Azure Active Directory connected service)?</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="edbd6-104">Začínáme</span><span class="sxs-lookup"><span data-stu-id="edbd6-104">Getting Started</span></span>](vs-active-directory-dotnet-getting-started.md)
> * [<span data-ttu-id="edbd6-105">Co se přihodilo</span><span class="sxs-lookup"><span data-stu-id="edbd6-105">What Happened</span></span>](vs-active-directory-dotnet-what-happened.md)
> 
> 

## <a name="references-have-been-added"></a><span data-ttu-id="edbd6-106">Byly přidány odkazy</span><span class="sxs-lookup"><span data-stu-id="edbd6-106">References have been added</span></span>
### <a name="nuget-package-references"></a><span data-ttu-id="edbd6-107">Odkazy na balíček NuGet</span><span class="sxs-lookup"><span data-stu-id="edbd6-107">NuGet package references</span></span>
* <span data-ttu-id="edbd6-108">**Microsoft.IdentityModel.Protocol.Extensions**</span><span class="sxs-lookup"><span data-stu-id="edbd6-108">**Microsoft.IdentityModel.Protocol.Extensions**</span></span>
* <span data-ttu-id="edbd6-109">**Microsoft.Owin**</span><span class="sxs-lookup"><span data-stu-id="edbd6-109">**Microsoft.Owin**</span></span>
* <span data-ttu-id="edbd6-110">**Microsoft.Owin.Host.SystemWeb**</span><span class="sxs-lookup"><span data-stu-id="edbd6-110">**Microsoft.Owin.Host.SystemWeb**</span></span>
* <span data-ttu-id="edbd6-111">**Microsoft.Owin.Security**</span><span class="sxs-lookup"><span data-stu-id="edbd6-111">**Microsoft.Owin.Security**</span></span>
* <span data-ttu-id="edbd6-112">**Microsoft.Owin.Security.Cookies**</span><span class="sxs-lookup"><span data-stu-id="edbd6-112">**Microsoft.Owin.Security.Cookies**</span></span>
* <span data-ttu-id="edbd6-113">**Microsoft.Owin.Security.OpenIdConnect**</span><span class="sxs-lookup"><span data-stu-id="edbd6-113">**Microsoft.Owin.Security.OpenIdConnect**</span></span>
* <span data-ttu-id="edbd6-114">**Owin**</span><span class="sxs-lookup"><span data-stu-id="edbd6-114">**Owin**</span></span>
* <span data-ttu-id="edbd6-115">**System.IdentityModel.Tokens.Jwt**</span><span class="sxs-lookup"><span data-stu-id="edbd6-115">**System.IdentityModel.Tokens.Jwt**</span></span>

### <a name="net-references"></a><span data-ttu-id="edbd6-116">Odkazy na rozhraní .NET</span><span class="sxs-lookup"><span data-stu-id="edbd6-116">.NET references</span></span>
* <span data-ttu-id="edbd6-117">**Microsoft.IdentityModel.Protocol.Extensions**</span><span class="sxs-lookup"><span data-stu-id="edbd6-117">**Microsoft.IdentityModel.Protocol.Extensions**</span></span>
* <span data-ttu-id="edbd6-118">**Microsoft.Owin**</span><span class="sxs-lookup"><span data-stu-id="edbd6-118">**Microsoft.Owin**</span></span>
* <span data-ttu-id="edbd6-119">**Microsoft.Owin.Host.SystemWeb**</span><span class="sxs-lookup"><span data-stu-id="edbd6-119">**Microsoft.Owin.Host.SystemWeb**</span></span>
* <span data-ttu-id="edbd6-120">**Microsoft.Owin.Security**</span><span class="sxs-lookup"><span data-stu-id="edbd6-120">**Microsoft.Owin.Security**</span></span>
* <span data-ttu-id="edbd6-121">**Microsoft.Owin.Security.Cookies**</span><span class="sxs-lookup"><span data-stu-id="edbd6-121">**Microsoft.Owin.Security.Cookies**</span></span>
* <span data-ttu-id="edbd6-122">**Microsoft.Owin.Security.OpenIdConnect**</span><span class="sxs-lookup"><span data-stu-id="edbd6-122">**Microsoft.Owin.Security.OpenIdConnect**</span></span>
* <span data-ttu-id="edbd6-123">**Owin**</span><span class="sxs-lookup"><span data-stu-id="edbd6-123">**Owin**</span></span>
* <span data-ttu-id="edbd6-124">**System.IdentityModel**</span><span class="sxs-lookup"><span data-stu-id="edbd6-124">**System.IdentityModel**</span></span>
* <span data-ttu-id="edbd6-125">**System.IdentityModel.Tokens.Jwt**</span><span class="sxs-lookup"><span data-stu-id="edbd6-125">**System.IdentityModel.Tokens.Jwt**</span></span>
* <span data-ttu-id="edbd6-126">**System.Runtime.Serialization**</span><span class="sxs-lookup"><span data-stu-id="edbd6-126">**System.Runtime.Serialization**</span></span>

## <a name="code-has-been-added"></a><span data-ttu-id="edbd6-127">Byla přidána kódu</span><span class="sxs-lookup"><span data-stu-id="edbd6-127">Code has been added</span></span>
### <a name="code-files-were-added-tooyour-project"></a><span data-ttu-id="edbd6-128">Soubory kódu byly přidány tooyour projektu</span><span class="sxs-lookup"><span data-stu-id="edbd6-128">Code files were added tooyour project</span></span>
<span data-ttu-id="edbd6-129">Spuštění třídu ověřování **App_Start/Startup.Auth.cs** byl přidán tooyour projekt obsahující logika spuštění pro ověřování Azure AD.</span><span class="sxs-lookup"><span data-stu-id="edbd6-129">An authentication startup class, **App_Start/Startup.Auth.cs** was added tooyour project containing startup logic for Azure AD authentication.</span></span> <span data-ttu-id="edbd6-130">Třída kontroleru, Controllers/AccountController.cs byla přidána navíc obsahující **SignIn()** a **SignOut()** metody.</span><span class="sxs-lookup"><span data-stu-id="edbd6-130">Also, a controller class, Controllers/AccountController.cs was added which contains **SignIn()** and **SignOut()** methods.</span></span> <span data-ttu-id="edbd6-131">Nakonec částečné zobrazení, **Views/Shared/_LoginPartial.cshtml** byl přidán obsahující odkaz akce pro přihlášení/odhlášení.</span><span class="sxs-lookup"><span data-stu-id="edbd6-131">Finally, a partial view, **Views/Shared/_LoginPartial.cshtml** was added containing an action link for SignIn/SignOut.</span></span>

### <a name="startup-code-was-added-tooyour-project"></a><span data-ttu-id="edbd6-132">Kód spuštění byl přidán tooyour projektu</span><span class="sxs-lookup"><span data-stu-id="edbd6-132">Startup code was added tooyour project</span></span>
<span data-ttu-id="edbd6-133">Pokud jste již měli třída při spuštění ve vašem projektu, hello **konfigurace** metoda byla aktualizovaná tooinclude volání příliš**ConfigureAuth(app)**.</span><span class="sxs-lookup"><span data-stu-id="edbd6-133">If you already had a Startup class in your project, hello **Configuration** method was updated tooinclude a call too**ConfigureAuth(app)**.</span></span> <span data-ttu-id="edbd6-134">Třída při spuštění, jinak hodnota přidala tooyour projektu.</span><span class="sxs-lookup"><span data-stu-id="edbd6-134">Otherwise, a Startup class was added tooyour project.</span></span>

### <a name="your-appconfig-or-webconfig-has-new-configuration-values"></a><span data-ttu-id="edbd6-135">App.config nebo web.config obsahuje nové hodnoty konfigurace</span><span class="sxs-lookup"><span data-stu-id="edbd6-135">Your app.config or web.config has new configuration values</span></span>
<span data-ttu-id="edbd6-136">byly přidány následující položky konfigurace Hello.</span><span class="sxs-lookup"><span data-stu-id="edbd6-136">hello following configuration entries have been added.</span></span>

    <appSettings>
        <add key="ida:ClientId" value="ClientId from hello new Azure AD App" />
        <add key="ida:AADInstance" value="https://login.microsoftonline.com/" />
        <add key="ida:Domain" value="hello selected Azure AD Domain" />
        <add key="ida:TenantId" value="hello Id of your selected Azure AD Tenant" />
        <add key="ida:PostLogoutRedirectUri" value="Your project start page" />
    </appSettings>

### <a name="an-azure-active-directory-ad-app-was-created"></a><span data-ttu-id="edbd6-137">Aplikace Azure Active Directory (AD) byla vytvořena.</span><span class="sxs-lookup"><span data-stu-id="edbd6-137">An Azure Active Directory (AD) App was created</span></span>
<span data-ttu-id="edbd6-138">Azure AD aplikace byla vytvořena v hello adresář, který jste vybrali v Průvodci hello.</span><span class="sxs-lookup"><span data-stu-id="edbd6-138">An Azure AD Application was created in hello directory that you selected in hello wizard.</span></span>

## <a name="if-i-checked-disable-individual-user-accounts-authentication-what-additional-changes-were-made-toomy-project"></a><span data-ttu-id="edbd6-139">Pokud po kontrole *zakázat jednotlivé uživatelské účty ověřování*, jaké další změny byly provedeny toomy projektu?</span><span class="sxs-lookup"><span data-stu-id="edbd6-139">If I checked *disable Individual User Accounts authentication*, what additional changes were made toomy project?</span></span>
<span data-ttu-id="edbd6-140">Byly odebrány odkazy balíčku NuGet, a soubory byly odebrány a zálohovat.</span><span class="sxs-lookup"><span data-stu-id="edbd6-140">NuGet package references were removed, and files were removed and backed up.</span></span> <span data-ttu-id="edbd6-141">V závislosti na stavu hello projektu může mít toomanually odebrat další odkazy nebo soubory, nebo změnit kód podle potřeby.</span><span class="sxs-lookup"><span data-stu-id="edbd6-141">Depending on hello state of your project, you may have toomanually remove additional references or files, or modify code as appropriate.</span></span>

### <a name="nuget-package-references-removed-for-those-present"></a><span data-ttu-id="edbd6-142">Odkazy na balíček NuGet odebrat (pro ty existuje)</span><span class="sxs-lookup"><span data-stu-id="edbd6-142">NuGet package references removed (for those present)</span></span>
* <span data-ttu-id="edbd6-143">**Microsoft.AspNet.Identity.Core**</span><span class="sxs-lookup"><span data-stu-id="edbd6-143">**Microsoft.AspNet.Identity.Core**</span></span>
* <span data-ttu-id="edbd6-144">**Microsoft.AspNet.Identity.EntityFramework**</span><span class="sxs-lookup"><span data-stu-id="edbd6-144">**Microsoft.AspNet.Identity.EntityFramework**</span></span>
* <span data-ttu-id="edbd6-145">**Microsoft.AspNet.Identity.Owin**</span><span class="sxs-lookup"><span data-stu-id="edbd6-145">**Microsoft.AspNet.Identity.Owin**</span></span>

### <a name="code-files-backed-up-and-removed-for-those-present"></a><span data-ttu-id="edbd6-146">Soubory kódu zálohovat a odebrat (pro ty existuje)</span><span class="sxs-lookup"><span data-stu-id="edbd6-146">Code files backed up and removed (for those present)</span></span>
<span data-ttu-id="edbd6-147">Každý z následujících souborů byl zálohován a odebrat z projektu hello.</span><span class="sxs-lookup"><span data-stu-id="edbd6-147">Each of following files was backed up and removed from hello project.</span></span> <span data-ttu-id="edbd6-148">Záložní soubory jsou umístěny ve složce 'Zálohování' v kořenovém adresáři projektu hello hello.</span><span class="sxs-lookup"><span data-stu-id="edbd6-148">Backup files are located in a 'Backup' folder at hello root of hello project's directory.</span></span>

* <span data-ttu-id="edbd6-149">**App_Start\IdentityConfig.cs**</span><span class="sxs-lookup"><span data-stu-id="edbd6-149">**App_Start\IdentityConfig.cs**</span></span>
* <span data-ttu-id="edbd6-150">**Controllers\ManageController.cs**</span><span class="sxs-lookup"><span data-stu-id="edbd6-150">**Controllers\ManageController.cs**</span></span>
* <span data-ttu-id="edbd6-151">**Models\IdentityModels.cs**</span><span class="sxs-lookup"><span data-stu-id="edbd6-151">**Models\IdentityModels.cs**</span></span>
* <span data-ttu-id="edbd6-152">**Models\ManageViewModels.cs**</span><span class="sxs-lookup"><span data-stu-id="edbd6-152">**Models\ManageViewModels.cs**</span></span>

### <a name="code-files-backed-up-for-those-present"></a><span data-ttu-id="edbd6-153">Soubory kódu zálohovat (pro ty existuje)</span><span class="sxs-lookup"><span data-stu-id="edbd6-153">Code files backed up (for those present)</span></span>
<span data-ttu-id="edbd6-154">Každý z následujících souborů byla zálohována před nahrazují.</span><span class="sxs-lookup"><span data-stu-id="edbd6-154">Each of following files was backed up before being replaced.</span></span> <span data-ttu-id="edbd6-155">Záložní soubory jsou umístěny ve složce 'Zálohování' v kořenovém adresáři projektu hello hello.</span><span class="sxs-lookup"><span data-stu-id="edbd6-155">Backup files are located in a 'Backup' folder at hello root of hello project's directory.</span></span>

* <span data-ttu-id="edbd6-156">**Startup.cs**</span><span class="sxs-lookup"><span data-stu-id="edbd6-156">**Startup.cs**</span></span>
* <span data-ttu-id="edbd6-157">**App_Start\Startup.auth.cs**</span><span class="sxs-lookup"><span data-stu-id="edbd6-157">**App_Start\Startup.Auth.cs**</span></span>
* <span data-ttu-id="edbd6-158">**Controllers\AccountController.cs**</span><span class="sxs-lookup"><span data-stu-id="edbd6-158">**Controllers\AccountController.cs**</span></span>
* <span data-ttu-id="edbd6-159">**Views\Shared\_LoginPartial.cshtml**</span><span class="sxs-lookup"><span data-stu-id="edbd6-159">**Views\Shared\_LoginPartial.cshtml**</span></span>

## <a name="if-i-checked-read-directory-data-what-additional-changes-were-made-toomy-project"></a><span data-ttu-id="edbd6-160">Pokud po kontrole *čtení dat adresáře*, jaké další změny byly provedeny toomy projektu?</span><span class="sxs-lookup"><span data-stu-id="edbd6-160">If I checked *Read directory data*, what additional changes were made toomy project?</span></span>
<span data-ttu-id="edbd6-161">Byly přidány další odkazy.</span><span class="sxs-lookup"><span data-stu-id="edbd6-161">Additional references have been added.</span></span>

### <a name="additional-nuget-package-references"></a><span data-ttu-id="edbd6-162">Další informace o balíčku NuGet</span><span class="sxs-lookup"><span data-stu-id="edbd6-162">Additional NuGet package references</span></span>
* <span data-ttu-id="edbd6-163">**EntityFramework**</span><span class="sxs-lookup"><span data-stu-id="edbd6-163">**EntityFramework**</span></span>
* <span data-ttu-id="edbd6-164">**Microsoft.Azure.ActiveDirectory.GraphClient**</span><span class="sxs-lookup"><span data-stu-id="edbd6-164">**Microsoft.Azure.ActiveDirectory.GraphClient**</span></span>
* <span data-ttu-id="edbd6-165">**Microsoft.Data.Edm**</span><span class="sxs-lookup"><span data-stu-id="edbd6-165">**Microsoft.Data.Edm**</span></span>
* <span data-ttu-id="edbd6-166">**Microsoft.Data.OData**</span><span class="sxs-lookup"><span data-stu-id="edbd6-166">**Microsoft.Data.OData**</span></span>
* <span data-ttu-id="edbd6-167">**Microsoft.Data.Services.Client**</span><span class="sxs-lookup"><span data-stu-id="edbd6-167">**Microsoft.Data.Services.Client**</span></span>
* <span data-ttu-id="edbd6-168">**Microsoft.IdentityModel.Clients.ActiveDirectory**</span><span class="sxs-lookup"><span data-stu-id="edbd6-168">**Microsoft.IdentityModel.Clients.ActiveDirectory**</span></span>
* <span data-ttu-id="edbd6-169">**System.Spatial**</span><span class="sxs-lookup"><span data-stu-id="edbd6-169">**System.Spatial**</span></span>

### <a name="additional-net-references"></a><span data-ttu-id="edbd6-170">Další odkazy na rozhraní .NET</span><span class="sxs-lookup"><span data-stu-id="edbd6-170">Additional .NET references</span></span>
* <span data-ttu-id="edbd6-171">**EntityFramework**</span><span class="sxs-lookup"><span data-stu-id="edbd6-171">**EntityFramework**</span></span>
* <span data-ttu-id="edbd6-172">**EntityFramework.SqlServer**</span><span class="sxs-lookup"><span data-stu-id="edbd6-172">**EntityFramework.SqlServer**</span></span>
* <span data-ttu-id="edbd6-173">**Microsoft.Azure.ActiveDirectory.GraphClient**</span><span class="sxs-lookup"><span data-stu-id="edbd6-173">**Microsoft.Azure.ActiveDirectory.GraphClient**</span></span>
* <span data-ttu-id="edbd6-174">**Microsoft.Data.Edm**</span><span class="sxs-lookup"><span data-stu-id="edbd6-174">**Microsoft.Data.Edm**</span></span>
* <span data-ttu-id="edbd6-175">**Microsoft.Data.OData**</span><span class="sxs-lookup"><span data-stu-id="edbd6-175">**Microsoft.Data.OData**</span></span>
* <span data-ttu-id="edbd6-176">**Microsoft.Data.Services.Client**</span><span class="sxs-lookup"><span data-stu-id="edbd6-176">**Microsoft.Data.Services.Client**</span></span>
* <span data-ttu-id="edbd6-177">**Microsoft.IdentityModel.Clients.ActiveDirectory**</span><span class="sxs-lookup"><span data-stu-id="edbd6-177">**Microsoft.IdentityModel.Clients.ActiveDirectory**</span></span>
* <span data-ttu-id="edbd6-178">**Microsoft.IdentityModel.Clients.ActiveDirectory.WindowsForms**</span><span class="sxs-lookup"><span data-stu-id="edbd6-178">**Microsoft.IdentityModel.Clients.ActiveDirectory.WindowsForms**</span></span>
* <span data-ttu-id="edbd6-179">**System.Spatial**</span><span class="sxs-lookup"><span data-stu-id="edbd6-179">**System.Spatial**</span></span>

### <a name="additional-code-files-were-added-tooyour-project"></a><span data-ttu-id="edbd6-180">Další kód byly přidány tooyour projektu</span><span class="sxs-lookup"><span data-stu-id="edbd6-180">Additional Code files were added tooyour project</span></span>
<span data-ttu-id="edbd6-181">Dva soubory byly přidány toosupport ukládání tokenu do mezipaměti: **Models\ADALTokenCache.cs** a **Models\ApplicationDbContext.cs**.</span><span class="sxs-lookup"><span data-stu-id="edbd6-181">Two files were added toosupport token caching: **Models\ADALTokenCache.cs** and **Models\ApplicationDbContext.cs**.</span></span>  <span data-ttu-id="edbd6-182">Další řadiče a zobrazení byly přidány tooillustrate přístup k informacím o profilu uživatele pomocí Azure graph API.</span><span class="sxs-lookup"><span data-stu-id="edbd6-182">An additional controller and view were added tooillustrate accessing user profile information using Azure graph APIs.</span></span>  <span data-ttu-id="edbd6-183">Tyto soubory jsou **Controllers\UserProfileController.cs** a **Views\UserProfile\Index.cshtml**.</span><span class="sxs-lookup"><span data-stu-id="edbd6-183">These files are **Controllers\UserProfileController.cs** and **Views\UserProfile\Index.cshtml**.</span></span>

### <a name="additional-startup-code-was-added-tooyour-project"></a><span data-ttu-id="edbd6-184">Další spouštěcí kód byl přidán tooyour projektu</span><span class="sxs-lookup"><span data-stu-id="edbd6-184">Additional Startup code was added tooyour project</span></span>
<span data-ttu-id="edbd6-185">V hello **startup.auth.cs** souboru novou **OpenIdConnectAuthenticationNotifications** objekt byl přidán toohello **oznámení** členem hello  **OpenIdConnectAuthenticationOptions**.</span><span class="sxs-lookup"><span data-stu-id="edbd6-185">In hello **startup.auth.cs** file, a new **OpenIdConnectAuthenticationNotifications** object was added toohello **Notifications** member of hello **OpenIdConnectAuthenticationOptions**.</span></span>  <span data-ttu-id="edbd6-186">Toto je tooenable přijímáním hello OAuth kódu a výměna ho pro přístupový token.</span><span class="sxs-lookup"><span data-stu-id="edbd6-186">This is tooenable receiving hello OAuth code and exchanging it for an access token.</span></span>

### <a name="additional-changes-were-made-tooyour-appconfig-or-webconfig"></a><span data-ttu-id="edbd6-187">Další změny, které byly provedeny tooyour app.config nebo web.config</span><span class="sxs-lookup"><span data-stu-id="edbd6-187">Additional changes were made tooyour app.config or web.config</span></span>
<span data-ttu-id="edbd6-188">Hello následující další konfigurační položky byly přidány.</span><span class="sxs-lookup"><span data-stu-id="edbd6-188">hello following additional configuration entries have been added.</span></span>

    <appSettings>
        <add key="ida:ClientSecret" value="Your Azure AD App's new client secret" />
    </appSettings>

<span data-ttu-id="edbd6-189">Hello následující konfigurační oddíly a připojovací řetězec bylo přidáno.</span><span class="sxs-lookup"><span data-stu-id="edbd6-189">hello following configuration sections and connection string have been added.</span></span>

    <configSections>
        <!-- For more information on Entity Framework configuration, visit http://go.microsoft.com/fwlink/?LinkID=237468 -->
        <section name="entityFramework" type="System.Data.Entity.Internal.ConfigFile.EntityFrameworkSection, EntityFramework, Version=6.0.0.0, Culture=neutral, PublicKeyToken=b77a5c561934e089" requirePermission="false" />
    </configSections>
    <connectionStrings>
        <add name="DefaultConnection" connectionString="Data Source=(localdb)\MSSQLLocalDB;AttachDbFilename=|DataDirectory|\aspnet-[AppName + Generated Id].mdf;Initial Catalog=aspnet-[AppName + Generated Id];Integrated Security=True" providerName="System.Data.SqlClient" />
    </connectionStrings>
    <entityFramework>
        <defaultConnectionFactory type="System.Data.Entity.Infrastructure.LocalDbConnectionFactory, EntityFramework">
          <parameters>
            <parameter value="mssqllocaldb" />
          </parameters>
        </defaultConnectionFactory>
        <providers>
          <provider invariantName="System.Data.SqlClient" type="System.Data.Entity.SqlServer.SqlProviderServices, EntityFramework.SqlServer" />
        </providers>
    </entityFramework>


### <a name="your-azure-active-directory-app-was-updated"></a><span data-ttu-id="edbd6-190">Aplikace Azure Active Directory byla aktualizována.</span><span class="sxs-lookup"><span data-stu-id="edbd6-190">Your Azure Active Directory App was updated</span></span>
<span data-ttu-id="edbd6-191">Aplikace Azure Active Directory byl aktualizovaný tooinclude hello *čtení dat adresáře* oprávnění a další klíč byl vytvořen, které se pak použije jako hello *ida: ClientSecret* v hello  **soubor Web.config** souboru.</span><span class="sxs-lookup"><span data-stu-id="edbd6-191">Your Azure Active Directory App was updated tooinclude hello *Read directory data* permission and an additional key was created which was then used as hello *ida:ClientSecret* in hello **web.config** file.</span></span>

## <a name="next-steps"></a><span data-ttu-id="edbd6-192">Další kroky</span><span class="sxs-lookup"><span data-stu-id="edbd6-192">Next steps</span></span>
- [<span data-ttu-id="edbd6-193">Další informace o službě Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="edbd6-193">Learn more about Azure Active Directory</span></span>](https://azure.microsoft.com/services/active-directory/)

