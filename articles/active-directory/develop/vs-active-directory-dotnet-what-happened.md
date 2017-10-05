---
title: "Změny provedené na projekt MVC při připojení k Azure AD | Microsoft Docs"
description: "Popisuje, co se stane do projektu MVC při připojení k Azure AD pomocí sady Visual Studio připojené služby"
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
ms.openlocfilehash: 095411a7fc854f4dce11921adb0f57c5389a8e13
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="what-happened-to-my-mvc-project-visual-studio-azure-active-directory-connected-service"></a><span data-ttu-id="3bab8-103">Co se stalo s MVC projektu (Visual Studio Azure Active Directory připojeno service)?</span><span class="sxs-lookup"><span data-stu-id="3bab8-103">What happened to my MVC project (Visual Studio Azure Active Directory connected service)?</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="3bab8-104">Začínáme</span><span class="sxs-lookup"><span data-stu-id="3bab8-104">Getting Started</span></span>](vs-active-directory-dotnet-getting-started.md)
> * [<span data-ttu-id="3bab8-105">Co se přihodilo</span><span class="sxs-lookup"><span data-stu-id="3bab8-105">What Happened</span></span>](vs-active-directory-dotnet-what-happened.md)
> 
> 

## <a name="references-have-been-added"></a><span data-ttu-id="3bab8-106">Byly přidány odkazy</span><span class="sxs-lookup"><span data-stu-id="3bab8-106">References have been added</span></span>
### <a name="nuget-package-references"></a><span data-ttu-id="3bab8-107">Odkazy na balíček NuGet</span><span class="sxs-lookup"><span data-stu-id="3bab8-107">NuGet package references</span></span>
* <span data-ttu-id="3bab8-108">**Microsoft.IdentityModel.Protocol.Extensions**</span><span class="sxs-lookup"><span data-stu-id="3bab8-108">**Microsoft.IdentityModel.Protocol.Extensions**</span></span>
* <span data-ttu-id="3bab8-109">**Microsoft.Owin**</span><span class="sxs-lookup"><span data-stu-id="3bab8-109">**Microsoft.Owin**</span></span>
* <span data-ttu-id="3bab8-110">**Microsoft.Owin.Host.SystemWeb**</span><span class="sxs-lookup"><span data-stu-id="3bab8-110">**Microsoft.Owin.Host.SystemWeb**</span></span>
* <span data-ttu-id="3bab8-111">**Microsoft.Owin.Security**</span><span class="sxs-lookup"><span data-stu-id="3bab8-111">**Microsoft.Owin.Security**</span></span>
* <span data-ttu-id="3bab8-112">**Microsoft.Owin.Security.Cookies**</span><span class="sxs-lookup"><span data-stu-id="3bab8-112">**Microsoft.Owin.Security.Cookies**</span></span>
* <span data-ttu-id="3bab8-113">**Microsoft.Owin.Security.OpenIdConnect**</span><span class="sxs-lookup"><span data-stu-id="3bab8-113">**Microsoft.Owin.Security.OpenIdConnect**</span></span>
* <span data-ttu-id="3bab8-114">**Owin**</span><span class="sxs-lookup"><span data-stu-id="3bab8-114">**Owin**</span></span>
* <span data-ttu-id="3bab8-115">**System.IdentityModel.Tokens.Jwt**</span><span class="sxs-lookup"><span data-stu-id="3bab8-115">**System.IdentityModel.Tokens.Jwt**</span></span>

### <a name="net-references"></a><span data-ttu-id="3bab8-116">Odkazy na rozhraní .NET</span><span class="sxs-lookup"><span data-stu-id="3bab8-116">.NET references</span></span>
* <span data-ttu-id="3bab8-117">**Microsoft.IdentityModel.Protocol.Extensions**</span><span class="sxs-lookup"><span data-stu-id="3bab8-117">**Microsoft.IdentityModel.Protocol.Extensions**</span></span>
* <span data-ttu-id="3bab8-118">**Microsoft.Owin**</span><span class="sxs-lookup"><span data-stu-id="3bab8-118">**Microsoft.Owin**</span></span>
* <span data-ttu-id="3bab8-119">**Microsoft.Owin.Host.SystemWeb**</span><span class="sxs-lookup"><span data-stu-id="3bab8-119">**Microsoft.Owin.Host.SystemWeb**</span></span>
* <span data-ttu-id="3bab8-120">**Microsoft.Owin.Security**</span><span class="sxs-lookup"><span data-stu-id="3bab8-120">**Microsoft.Owin.Security**</span></span>
* <span data-ttu-id="3bab8-121">**Microsoft.Owin.Security.Cookies**</span><span class="sxs-lookup"><span data-stu-id="3bab8-121">**Microsoft.Owin.Security.Cookies**</span></span>
* <span data-ttu-id="3bab8-122">**Microsoft.Owin.Security.OpenIdConnect**</span><span class="sxs-lookup"><span data-stu-id="3bab8-122">**Microsoft.Owin.Security.OpenIdConnect**</span></span>
* <span data-ttu-id="3bab8-123">**Owin**</span><span class="sxs-lookup"><span data-stu-id="3bab8-123">**Owin**</span></span>
* <span data-ttu-id="3bab8-124">**System.IdentityModel**</span><span class="sxs-lookup"><span data-stu-id="3bab8-124">**System.IdentityModel**</span></span>
* <span data-ttu-id="3bab8-125">**System.IdentityModel.Tokens.Jwt**</span><span class="sxs-lookup"><span data-stu-id="3bab8-125">**System.IdentityModel.Tokens.Jwt**</span></span>
* <span data-ttu-id="3bab8-126">**System.Runtime.Serialization**</span><span class="sxs-lookup"><span data-stu-id="3bab8-126">**System.Runtime.Serialization**</span></span>

## <a name="code-has-been-added"></a><span data-ttu-id="3bab8-127">Byla přidána kódu</span><span class="sxs-lookup"><span data-stu-id="3bab8-127">Code has been added</span></span>
### <a name="code-files-were-added-to-your-project"></a><span data-ttu-id="3bab8-128">Soubory kódu byly přidány do projektu</span><span class="sxs-lookup"><span data-stu-id="3bab8-128">Code files were added to your project</span></span>
<span data-ttu-id="3bab8-129">Spuštění třídu ověřování **App_Start/Startup.Auth.cs** byl přidán do projektu obsahující logika spuštění pro ověřování Azure AD.</span><span class="sxs-lookup"><span data-stu-id="3bab8-129">An authentication startup class, **App_Start/Startup.Auth.cs** was added to your project containing startup logic for Azure AD authentication.</span></span> <span data-ttu-id="3bab8-130">Třída kontroleru, Controllers/AccountController.cs byla přidána navíc obsahující **SignIn()** a **SignOut()** metody.</span><span class="sxs-lookup"><span data-stu-id="3bab8-130">Also, a controller class, Controllers/AccountController.cs was added which contains **SignIn()** and **SignOut()** methods.</span></span> <span data-ttu-id="3bab8-131">Nakonec částečné zobrazení, **Views/Shared/_LoginPartial.cshtml** byl přidán obsahující odkaz akce pro přihlášení/odhlášení.</span><span class="sxs-lookup"><span data-stu-id="3bab8-131">Finally, a partial view, **Views/Shared/_LoginPartial.cshtml** was added containing an action link for SignIn/SignOut.</span></span>

### <a name="startup-code-was-added-to-your-project"></a><span data-ttu-id="3bab8-132">Kód spuštění byl přidán do projektu</span><span class="sxs-lookup"><span data-stu-id="3bab8-132">Startup code was added to your project</span></span>
<span data-ttu-id="3bab8-133">Pokud jste již měli třída při spuštění ve vašem projektu **konfigurace** metoda byla aktualizována zahrnout volání **ConfigureAuth(app)**.</span><span class="sxs-lookup"><span data-stu-id="3bab8-133">If you already had a Startup class in your project, the **Configuration** method was updated to include a call to **ConfigureAuth(app)**.</span></span> <span data-ttu-id="3bab8-134">Třída při spuštění, jinak hodnota byl přidán do projektu.</span><span class="sxs-lookup"><span data-stu-id="3bab8-134">Otherwise, a Startup class was added to your project.</span></span>

### <a name="your-appconfig-or-webconfig-has-new-configuration-values"></a><span data-ttu-id="3bab8-135">App.config nebo web.config obsahuje nové hodnoty konfigurace</span><span class="sxs-lookup"><span data-stu-id="3bab8-135">Your app.config or web.config has new configuration values</span></span>
<span data-ttu-id="3bab8-136">Byly přidány následující položky konfigurace.</span><span class="sxs-lookup"><span data-stu-id="3bab8-136">The following configuration entries have been added.</span></span>

    <appSettings>
        <add key="ida:ClientId" value="ClientId from the new Azure AD App" />
        <add key="ida:AADInstance" value="https://login.microsoftonline.com/" />
        <add key="ida:Domain" value="The selected Azure AD Domain" />
        <add key="ida:TenantId" value="The Id of your selected Azure AD Tenant" />
        <add key="ida:PostLogoutRedirectUri" value="Your project start page" />
    </appSettings>

### <a name="an-azure-active-directory-ad-app-was-created"></a><span data-ttu-id="3bab8-137">Aplikace Azure Active Directory (AD) byla vytvořena.</span><span class="sxs-lookup"><span data-stu-id="3bab8-137">An Azure Active Directory (AD) App was created</span></span>
<span data-ttu-id="3bab8-138">Aplikaci Azure AD byl vytvořen v adresáři, který jste vybrali v průvodci.</span><span class="sxs-lookup"><span data-stu-id="3bab8-138">An Azure AD Application was created in the directory that you selected in the wizard.</span></span>

## <a name="if-i-checked-disable-individual-user-accounts-authentication-what-additional-changes-were-made-to-my-project"></a><span data-ttu-id="3bab8-139">Pokud po kontrole *zakázat jednotlivé uživatelské účty ověřování*, jaké další změny byly provedeny na Můj projekt?</span><span class="sxs-lookup"><span data-stu-id="3bab8-139">If I checked *disable Individual User Accounts authentication*, what additional changes were made to my project?</span></span>
<span data-ttu-id="3bab8-140">Byly odebrány odkazy balíčku NuGet, a soubory byly odebrány a zálohovat.</span><span class="sxs-lookup"><span data-stu-id="3bab8-140">NuGet package references were removed, and files were removed and backed up.</span></span> <span data-ttu-id="3bab8-141">V závislosti na stavu projektu možná budete muset ručně odebrat další odkazy nebo soubory, nebo upravit kód podle potřeby.</span><span class="sxs-lookup"><span data-stu-id="3bab8-141">Depending on the state of your project, you may have to manually remove additional references or files, or modify code as appropriate.</span></span>

### <a name="nuget-package-references-removed-for-those-present"></a><span data-ttu-id="3bab8-142">Odkazy na balíček NuGet odebrat (pro ty existuje)</span><span class="sxs-lookup"><span data-stu-id="3bab8-142">NuGet package references removed (for those present)</span></span>
* <span data-ttu-id="3bab8-143">**Microsoft.AspNet.Identity.Core**</span><span class="sxs-lookup"><span data-stu-id="3bab8-143">**Microsoft.AspNet.Identity.Core**</span></span>
* <span data-ttu-id="3bab8-144">**Microsoft.AspNet.Identity.EntityFramework**</span><span class="sxs-lookup"><span data-stu-id="3bab8-144">**Microsoft.AspNet.Identity.EntityFramework**</span></span>
* <span data-ttu-id="3bab8-145">**Microsoft.AspNet.Identity.Owin**</span><span class="sxs-lookup"><span data-stu-id="3bab8-145">**Microsoft.AspNet.Identity.Owin**</span></span>

### <a name="code-files-backed-up-and-removed-for-those-present"></a><span data-ttu-id="3bab8-146">Soubory kódu zálohovat a odebrat (pro ty existuje)</span><span class="sxs-lookup"><span data-stu-id="3bab8-146">Code files backed up and removed (for those present)</span></span>
<span data-ttu-id="3bab8-147">Každý z následujících souborů byl zálohován a odebrat z projektu.</span><span class="sxs-lookup"><span data-stu-id="3bab8-147">Each of following files was backed up and removed from the project.</span></span> <span data-ttu-id="3bab8-148">Záložní soubory jsou umístěny ve složce 'Zálohování' v kořenovém adresáři projektu.</span><span class="sxs-lookup"><span data-stu-id="3bab8-148">Backup files are located in a 'Backup' folder at the root of the project's directory.</span></span>

* <span data-ttu-id="3bab8-149">**App_Start\IdentityConfig.cs**</span><span class="sxs-lookup"><span data-stu-id="3bab8-149">**App_Start\IdentityConfig.cs**</span></span>
* <span data-ttu-id="3bab8-150">**Controllers\ManageController.cs**</span><span class="sxs-lookup"><span data-stu-id="3bab8-150">**Controllers\ManageController.cs**</span></span>
* <span data-ttu-id="3bab8-151">**Models\IdentityModels.cs**</span><span class="sxs-lookup"><span data-stu-id="3bab8-151">**Models\IdentityModels.cs**</span></span>
* <span data-ttu-id="3bab8-152">**Models\ManageViewModels.cs**</span><span class="sxs-lookup"><span data-stu-id="3bab8-152">**Models\ManageViewModels.cs**</span></span>

### <a name="code-files-backed-up-for-those-present"></a><span data-ttu-id="3bab8-153">Soubory kódu zálohovat (pro ty existuje)</span><span class="sxs-lookup"><span data-stu-id="3bab8-153">Code files backed up (for those present)</span></span>
<span data-ttu-id="3bab8-154">Každý z následujících souborů byla zálohována před nahrazují.</span><span class="sxs-lookup"><span data-stu-id="3bab8-154">Each of following files was backed up before being replaced.</span></span> <span data-ttu-id="3bab8-155">Záložní soubory jsou umístěny ve složce 'Zálohování' v kořenovém adresáři projektu.</span><span class="sxs-lookup"><span data-stu-id="3bab8-155">Backup files are located in a 'Backup' folder at the root of the project's directory.</span></span>

* <span data-ttu-id="3bab8-156">**Startup.cs**</span><span class="sxs-lookup"><span data-stu-id="3bab8-156">**Startup.cs**</span></span>
* <span data-ttu-id="3bab8-157">**App_Start\Startup.auth.cs**</span><span class="sxs-lookup"><span data-stu-id="3bab8-157">**App_Start\Startup.Auth.cs**</span></span>
* <span data-ttu-id="3bab8-158">**Controllers\AccountController.cs**</span><span class="sxs-lookup"><span data-stu-id="3bab8-158">**Controllers\AccountController.cs**</span></span>
* <span data-ttu-id="3bab8-159">**Views\Shared\_LoginPartial.cshtml**</span><span class="sxs-lookup"><span data-stu-id="3bab8-159">**Views\Shared\_LoginPartial.cshtml**</span></span>

## <a name="if-i-checked-read-directory-data-what-additional-changes-were-made-to-my-project"></a><span data-ttu-id="3bab8-160">Pokud po kontrole *čtení dat adresáře*, jaké další změny byly provedeny na Můj projekt?</span><span class="sxs-lookup"><span data-stu-id="3bab8-160">If I checked *Read directory data*, what additional changes were made to my project?</span></span>
<span data-ttu-id="3bab8-161">Byly přidány další odkazy.</span><span class="sxs-lookup"><span data-stu-id="3bab8-161">Additional references have been added.</span></span>

### <a name="additional-nuget-package-references"></a><span data-ttu-id="3bab8-162">Další informace o balíčku NuGet</span><span class="sxs-lookup"><span data-stu-id="3bab8-162">Additional NuGet package references</span></span>
* <span data-ttu-id="3bab8-163">**EntityFramework**</span><span class="sxs-lookup"><span data-stu-id="3bab8-163">**EntityFramework**</span></span>
* <span data-ttu-id="3bab8-164">**Microsoft.Azure.ActiveDirectory.GraphClient**</span><span class="sxs-lookup"><span data-stu-id="3bab8-164">**Microsoft.Azure.ActiveDirectory.GraphClient**</span></span>
* <span data-ttu-id="3bab8-165">**Microsoft.Data.Edm**</span><span class="sxs-lookup"><span data-stu-id="3bab8-165">**Microsoft.Data.Edm**</span></span>
* <span data-ttu-id="3bab8-166">**Microsoft.Data.OData**</span><span class="sxs-lookup"><span data-stu-id="3bab8-166">**Microsoft.Data.OData**</span></span>
* <span data-ttu-id="3bab8-167">**Microsoft.Data.Services.Client**</span><span class="sxs-lookup"><span data-stu-id="3bab8-167">**Microsoft.Data.Services.Client**</span></span>
* <span data-ttu-id="3bab8-168">**Microsoft.IdentityModel.Clients.ActiveDirectory**</span><span class="sxs-lookup"><span data-stu-id="3bab8-168">**Microsoft.IdentityModel.Clients.ActiveDirectory**</span></span>
* <span data-ttu-id="3bab8-169">**System.Spatial**</span><span class="sxs-lookup"><span data-stu-id="3bab8-169">**System.Spatial**</span></span>

### <a name="additional-net-references"></a><span data-ttu-id="3bab8-170">Další odkazy na rozhraní .NET</span><span class="sxs-lookup"><span data-stu-id="3bab8-170">Additional .NET references</span></span>
* <span data-ttu-id="3bab8-171">**EntityFramework**</span><span class="sxs-lookup"><span data-stu-id="3bab8-171">**EntityFramework**</span></span>
* <span data-ttu-id="3bab8-172">**EntityFramework.SqlServer**</span><span class="sxs-lookup"><span data-stu-id="3bab8-172">**EntityFramework.SqlServer**</span></span>
* <span data-ttu-id="3bab8-173">**Microsoft.Azure.ActiveDirectory.GraphClient**</span><span class="sxs-lookup"><span data-stu-id="3bab8-173">**Microsoft.Azure.ActiveDirectory.GraphClient**</span></span>
* <span data-ttu-id="3bab8-174">**Microsoft.Data.Edm**</span><span class="sxs-lookup"><span data-stu-id="3bab8-174">**Microsoft.Data.Edm**</span></span>
* <span data-ttu-id="3bab8-175">**Microsoft.Data.OData**</span><span class="sxs-lookup"><span data-stu-id="3bab8-175">**Microsoft.Data.OData**</span></span>
* <span data-ttu-id="3bab8-176">**Microsoft.Data.Services.Client**</span><span class="sxs-lookup"><span data-stu-id="3bab8-176">**Microsoft.Data.Services.Client**</span></span>
* <span data-ttu-id="3bab8-177">**Microsoft.IdentityModel.Clients.ActiveDirectory**</span><span class="sxs-lookup"><span data-stu-id="3bab8-177">**Microsoft.IdentityModel.Clients.ActiveDirectory**</span></span>
* <span data-ttu-id="3bab8-178">**Microsoft.IdentityModel.Clients.ActiveDirectory.WindowsForms**</span><span class="sxs-lookup"><span data-stu-id="3bab8-178">**Microsoft.IdentityModel.Clients.ActiveDirectory.WindowsForms**</span></span>
* <span data-ttu-id="3bab8-179">**System.Spatial**</span><span class="sxs-lookup"><span data-stu-id="3bab8-179">**System.Spatial**</span></span>

### <a name="additional-code-files-were-added-to-your-project"></a><span data-ttu-id="3bab8-180">Další kód byly přidány do projektu</span><span class="sxs-lookup"><span data-stu-id="3bab8-180">Additional Code files were added to your project</span></span>
<span data-ttu-id="3bab8-181">K podpoře ukládání tokenu do mezipaměti nebyly přidány dva soubory: **Models\ADALTokenCache.cs** a **Models\ApplicationDbContext.cs**.</span><span class="sxs-lookup"><span data-stu-id="3bab8-181">Two files were added to support token caching: **Models\ADALTokenCache.cs** and **Models\ApplicationDbContext.cs**.</span></span>  <span data-ttu-id="3bab8-182">Pro ilustraci informace o přístupu k profilu uživatele pomocí Azure graph API byly přidány dalšího řadiče a zobrazení.</span><span class="sxs-lookup"><span data-stu-id="3bab8-182">An additional controller and view were added to illustrate accessing user profile information using Azure graph APIs.</span></span>  <span data-ttu-id="3bab8-183">Tyto soubory jsou **Controllers\UserProfileController.cs** a **Views\UserProfile\Index.cshtml**.</span><span class="sxs-lookup"><span data-stu-id="3bab8-183">These files are **Controllers\UserProfileController.cs** and **Views\UserProfile\Index.cshtml**.</span></span>

### <a name="additional-startup-code-was-added-to-your-project"></a><span data-ttu-id="3bab8-184">Další spouštěcí kód byl přidán do projektu</span><span class="sxs-lookup"><span data-stu-id="3bab8-184">Additional Startup code was added to your project</span></span>
<span data-ttu-id="3bab8-185">V **startup.auth.cs** souboru novou **OpenIdConnectAuthenticationNotifications** objekt byl přidán do **oznámení** členem  **OpenIdConnectAuthenticationOptions**.</span><span class="sxs-lookup"><span data-stu-id="3bab8-185">In the **startup.auth.cs** file, a new **OpenIdConnectAuthenticationNotifications** object was added to the **Notifications** member of the **OpenIdConnectAuthenticationOptions**.</span></span>  <span data-ttu-id="3bab8-186">Toto je umožnit přijetí kód OAuth a výměna ho pro přístupový token.</span><span class="sxs-lookup"><span data-stu-id="3bab8-186">This is to enable receiving the OAuth code and exchanging it for an access token.</span></span>

### <a name="additional-changes-were-made-to-your-appconfig-or-webconfig"></a><span data-ttu-id="3bab8-187">Byly provedeny další změny app.config nebo web.config</span><span class="sxs-lookup"><span data-stu-id="3bab8-187">Additional changes were made to your app.config or web.config</span></span>
<span data-ttu-id="3bab8-188">Byly přidány následující položky další konfiguraci.</span><span class="sxs-lookup"><span data-stu-id="3bab8-188">The following additional configuration entries have been added.</span></span>

    <appSettings>
        <add key="ida:ClientSecret" value="Your Azure AD App's new client secret" />
    </appSettings>

<span data-ttu-id="3bab8-189">Byly přidány následující konfigurační oddíly a připojovací řetězec.</span><span class="sxs-lookup"><span data-stu-id="3bab8-189">The following configuration sections and connection string have been added.</span></span>

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


### <a name="your-azure-active-directory-app-was-updated"></a><span data-ttu-id="3bab8-190">Aplikace Azure Active Directory byla aktualizována.</span><span class="sxs-lookup"><span data-stu-id="3bab8-190">Your Azure Active Directory App was updated</span></span>
<span data-ttu-id="3bab8-191">Aplikace Azure Active Directory byla aktualizována zahrnout *čtení dat adresáře* oprávnění a další klíč byl vytvořen, které se pak použije jako *ida: ClientSecret* v  **soubor Web.config** souboru.</span><span class="sxs-lookup"><span data-stu-id="3bab8-191">Your Azure Active Directory App was updated to include the *Read directory data* permission and an additional key was created which was then used as the *ida:ClientSecret* in the **web.config** file.</span></span>

## <a name="next-steps"></a><span data-ttu-id="3bab8-192">Další kroky</span><span class="sxs-lookup"><span data-stu-id="3bab8-192">Next steps</span></span>
- [<span data-ttu-id="3bab8-193">Další informace o službě Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="3bab8-193">Learn more about Azure Active Directory</span></span>](https://azure.microsoft.com/services/active-directory/)

