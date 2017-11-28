---
title: "Změny provedené na projekt WebApi při připojení k Azure AD | Microsoft Docs"
description: "Popisuje, co se stane do projektu WebApi při připojení k Azure AD pomocí sady Visual Studio"
services: active-directory
documentationcenter: 
author: kraigb
manager: ghogen
editor: 
ms.assetid: 57630aee-26a2-4326-9dbb-ea2a66daa8b0
ms.service: active-directory
ms.workload: web
ms.tgt_pltfrm: vs-what-happened
ms.devlang: na
ms.topic: article
ms.date: 03/01/2017
ms.author: kraigb
ms.custom: aaddev
ms.openlocfilehash: 086e5a9622cad681cd282345d97e0c28ee7de2fa
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="what-happened-to-my-webapi-project-visual-studio-azure-active-directory-connected-service"></a><span data-ttu-id="7a578-103">Co se stalo s WebApi projektu (Visual Studio Azure Active Directory připojení služby)</span><span class="sxs-lookup"><span data-stu-id="7a578-103">What happened to my WebApi project (Visual Studio Azure Active Directory connected service)</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="7a578-104">Začínáme</span><span class="sxs-lookup"><span data-stu-id="7a578-104">Getting Started</span></span>](vs-active-directory-webapi-getting-started.md)
> * [<span data-ttu-id="7a578-105">Co se přihodilo</span><span class="sxs-lookup"><span data-stu-id="7a578-105">What Happened</span></span>](vs-active-directory-webapi-what-happened.md)
> 
> 

## <a name="references-have-been-added"></a><span data-ttu-id="7a578-106">Byly přidány odkazy</span><span class="sxs-lookup"><span data-stu-id="7a578-106">References have been added</span></span>
### <a name="nuget-package-references"></a><span data-ttu-id="7a578-107">Odkazy na balíček NuGet</span><span class="sxs-lookup"><span data-stu-id="7a578-107">NuGet package references</span></span>
* `Microsoft.Owin`
* `Microsoft.Owin.Host.SystemWeb`
* `Microsoft.Owin.Security`
* `Microsoft.Owin.Security.ActiveDirectory`
* `Microsoft.Owin.Security.Jwt`
* `Microsoft.Owin.Security.OAuth`
* `Owin`
* `System.IdentityModel.Tokens.Jwt`

### <a name="net-references"></a><span data-ttu-id="7a578-108">Odkazy na rozhraní .NET</span><span class="sxs-lookup"><span data-stu-id="7a578-108">.NET references</span></span>
* `Microsoft.Owin`
* `Microsoft.Owin.Host.SystemWeb`
* `Microsoft.Owin.Security`
* `Microsoft.Owin.Security.ActiveDirectory`
* `Microsoft.Owin.Security.Jwt`
* `Microsoft.Owin.Security.OAuth`
* `Owin`
* `System.IdentityModel.Tokens.Jwt`

## <a name="code-changes"></a><span data-ttu-id="7a578-109">Změny kódu</span><span class="sxs-lookup"><span data-stu-id="7a578-109">Code changes</span></span>
### <a name="code-files-were-added-to-your-project"></a><span data-ttu-id="7a578-110">Soubory kódu byly přidány do projektu</span><span class="sxs-lookup"><span data-stu-id="7a578-110">Code files were added to your project</span></span>
<span data-ttu-id="7a578-111">Spuštění třídu ověřování **App_Start/Startup.Auth.cs** byl přidán do projektu obsahující logika spuštění pro ověřování Azure AD.</span><span class="sxs-lookup"><span data-stu-id="7a578-111">An authentication startup class, **App_Start/Startup.Auth.cs** was added to your project containing startup logic for Azure AD authentication.</span></span>

### <a name="startup-code-was-added-to-your-project"></a><span data-ttu-id="7a578-112">Kód spuštění byl přidán do projektu</span><span class="sxs-lookup"><span data-stu-id="7a578-112">Startup code was added to your project</span></span>
<span data-ttu-id="7a578-113">Pokud jste již měli třída při spuštění ve vašem projektu **konfigurace** metoda byla aktualizována zahrnout volání `ConfigureAuth(app)`.</span><span class="sxs-lookup"><span data-stu-id="7a578-113">If you already had a Startup class in your project, the **Configuration** method was updated to include a call to `ConfigureAuth(app)`.</span></span> <span data-ttu-id="7a578-114">Třída při spuštění, jinak hodnota byl přidán do projektu.</span><span class="sxs-lookup"><span data-stu-id="7a578-114">Otherwise, a Startup class was added to your project.</span></span>

### <a name="your-appconfig-or-webconfig-file-has-new-configuration-values"></a><span data-ttu-id="7a578-115">Váš soubor app.config nebo web.config obsahuje nové hodnoty konfigurace.</span><span class="sxs-lookup"><span data-stu-id="7a578-115">Your app.config or web.config file has new configuration values.</span></span>
<span data-ttu-id="7a578-116">Byly přidány následující položky konfigurace.</span><span class="sxs-lookup"><span data-stu-id="7a578-116">The following configuration entries have been added.</span></span>

```
    <appSettings>
            <add key="ida:ClientId" value="ClientId from the new Azure AD App" />
            <add key="ida:Tenant" value="Your selected Azure AD Tenant" />
            <add key="ida:Audience" value="The App ID Uri from the wizard" />
    </appSettings>`
```

### <a name="an-azure-ad-app-was-created"></a><span data-ttu-id="7a578-117">Byla vytvořena aplikace Azure AD</span><span class="sxs-lookup"><span data-stu-id="7a578-117">An Azure AD App was created</span></span>
<span data-ttu-id="7a578-118">Aplikaci Azure AD byl vytvořen v adresáři, který jste vybrali v průvodci.</span><span class="sxs-lookup"><span data-stu-id="7a578-118">An Azure AD Application was created in the directory that you selected in the wizard.</span></span>

[<span data-ttu-id="7a578-119">Další informace o službě Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="7a578-119">Learn more about Azure Active Directory</span></span>](https://azure.microsoft.com/services/active-directory/)

## <a name="if-i-checked-disable-individual-user-accounts-authentication-what-additional-changes-were-made-to-my-project"></a><span data-ttu-id="7a578-120">Pokud po kontrole *zakázat jednotlivé uživatelské účty ověřování*, jaké další změny byly provedeny na Můj projekt?</span><span class="sxs-lookup"><span data-stu-id="7a578-120">If I checked *disable Individual User Accounts authentication*, what additional changes were made to my project?</span></span>
<span data-ttu-id="7a578-121">Byly odebrány odkazy balíčku NuGet, a soubory byly odebrány a zálohovat.</span><span class="sxs-lookup"><span data-stu-id="7a578-121">NuGet package references were removed, and files were removed and backed up.</span></span> <span data-ttu-id="7a578-122">V závislosti na stavu projektu možná budete muset ručně odebrat další odkazy nebo soubory, nebo upravit kód podle potřeby.</span><span class="sxs-lookup"><span data-stu-id="7a578-122">Depending on the state of your project, you may have to manually remove additional references or files, or modify code as appropriate.</span></span>

### <a name="nuget-package-references-removed-for-those-present"></a><span data-ttu-id="7a578-123">Odkazy na balíček NuGet odebrat (pro ty existuje)</span><span class="sxs-lookup"><span data-stu-id="7a578-123">NuGet package references removed (for those present)</span></span>
* `Microsoft.AspNet.Identity.Core`
* `Microsoft.AspNet.Identity.EntityFramework`
* `Microsoft.AspNet.Identity.Owin`

### <a name="code-files-backed-up-and-removed-for-those-present"></a><span data-ttu-id="7a578-124">Soubory kódu zálohovat a odebrat (pro ty existuje)</span><span class="sxs-lookup"><span data-stu-id="7a578-124">Code files backed up and removed (for those present)</span></span>
<span data-ttu-id="7a578-125">Každý z následujících souborů byl zálohován a odebrat z projektu.</span><span class="sxs-lookup"><span data-stu-id="7a578-125">Each of following files was backed up and removed from the project.</span></span> <span data-ttu-id="7a578-126">Záložní soubory jsou umístěny ve složce 'Zálohování' v kořenovém adresáři projektu.</span><span class="sxs-lookup"><span data-stu-id="7a578-126">Backup files are located in a 'Backup' folder at the root of the project's directory.</span></span>

* `App_Start\IdentityConfig.cs`
* `Controllers\AccountController.cs`
* `Controllers\ManageController.cs`
* `Models\IdentityModels.cs`
* `Providers\ApplicationOAuthProvider.cs`

### <a name="code-files-backed-up-for-those-present"></a><span data-ttu-id="7a578-127">Soubory kódu zálohovat (pro ty existuje)</span><span class="sxs-lookup"><span data-stu-id="7a578-127">Code files backed up (for those present)</span></span>
<span data-ttu-id="7a578-128">Každý z následujících souborů byla zálohována před nahrazují.</span><span class="sxs-lookup"><span data-stu-id="7a578-128">Each of following files was backed up before being replaced.</span></span> <span data-ttu-id="7a578-129">Záložní soubory jsou umístěny ve složce 'Zálohování' v kořenovém adresáři projektu.</span><span class="sxs-lookup"><span data-stu-id="7a578-129">Backup files are located in a 'Backup' folder at the root of the project's directory.</span></span>

* `Startup.cs`
* `App_Start\Startup.Auth.cs`

## <a name="if-i-checked-read-directory-data-what-additional-changes-were-made-to-my-project"></a><span data-ttu-id="7a578-130">Pokud po kontrole *čtení dat adresáře*, jaké další změny byly provedeny na Můj projekt?</span><span class="sxs-lookup"><span data-stu-id="7a578-130">If I checked *Read directory data*, what additional changes were made to my project?</span></span>
### <a name="additional-changes-were-made-to-your-appconfig-or-webconfig"></a><span data-ttu-id="7a578-131">Byly provedeny další změny app.config nebo web.config</span><span class="sxs-lookup"><span data-stu-id="7a578-131">Additional changes were made to your app.config or web.config</span></span>
<span data-ttu-id="7a578-132">Byly přidány následující položky další konfiguraci.</span><span class="sxs-lookup"><span data-stu-id="7a578-132">The following additional configuration entries have been added.</span></span>

```
    <appSettings>
        <add key="ida:Password" value="Your Azure AD App's new password" />
    </appSettings>`
```

### <a name="your-azure-active-directory-app-was-updated"></a><span data-ttu-id="7a578-133">Aplikace Azure Active Directory byla aktualizována.</span><span class="sxs-lookup"><span data-stu-id="7a578-133">Your Azure Active Directory App was updated</span></span>
<span data-ttu-id="7a578-134">Aplikace Azure Active Directory byla aktualizována zahrnout *čtení dat adresáře* oprávnění a další klíč byl vytvořen, které se pak použije jako *ida: heslo* v `web.config` souboru.</span><span class="sxs-lookup"><span data-stu-id="7a578-134">Your Azure Active Directory App was updated to include the *Read directory data* permission and an additional key was created which was then used as the *ida:Password* in the `web.config` file.</span></span>

## <a name="next-steps"></a><span data-ttu-id="7a578-135">Další kroky</span><span class="sxs-lookup"><span data-stu-id="7a578-135">Next steps</span></span>
- [<span data-ttu-id="7a578-136">Další informace o službě Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="7a578-136">Learn more about Azure Active Directory</span></span>](https://azure.microsoft.com/services/active-directory/)

