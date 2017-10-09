---
title: "projektu WebApi tooa aaaChanges provedeny, když připojíte tooAzure AD | Microsoft Docs"
description: "Popisuje, co se stane tooyour WebApi projektu, jakmile se připojíte tooAzure AD pomocí sady Visual Studio"
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
ms.openlocfilehash: 1ea77b6c75b2dc273219fa6c43f02c7a7c5312ba
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="what-happened-toomy-webapi-project-visual-studio-azure-active-directory-connected-service"></a><span data-ttu-id="f0aae-103">Co se stalo toomy WebApi projektu (Visual Studio Azure Active Directory připojení služby)</span><span class="sxs-lookup"><span data-stu-id="f0aae-103">What happened toomy WebApi project (Visual Studio Azure Active Directory connected service)</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="f0aae-104">Začínáme</span><span class="sxs-lookup"><span data-stu-id="f0aae-104">Getting Started</span></span>](vs-active-directory-webapi-getting-started.md)
> * [<span data-ttu-id="f0aae-105">Co se přihodilo</span><span class="sxs-lookup"><span data-stu-id="f0aae-105">What Happened</span></span>](vs-active-directory-webapi-what-happened.md)
> 
> 

## <a name="references-have-been-added"></a><span data-ttu-id="f0aae-106">Byly přidány odkazy</span><span class="sxs-lookup"><span data-stu-id="f0aae-106">References have been added</span></span>
### <a name="nuget-package-references"></a><span data-ttu-id="f0aae-107">Odkazy na balíček NuGet</span><span class="sxs-lookup"><span data-stu-id="f0aae-107">NuGet package references</span></span>
* `Microsoft.Owin`
* `Microsoft.Owin.Host.SystemWeb`
* `Microsoft.Owin.Security`
* `Microsoft.Owin.Security.ActiveDirectory`
* `Microsoft.Owin.Security.Jwt`
* `Microsoft.Owin.Security.OAuth`
* `Owin`
* `System.IdentityModel.Tokens.Jwt`

### <a name="net-references"></a><span data-ttu-id="f0aae-108">Odkazy na rozhraní .NET</span><span class="sxs-lookup"><span data-stu-id="f0aae-108">.NET references</span></span>
* `Microsoft.Owin`
* `Microsoft.Owin.Host.SystemWeb`
* `Microsoft.Owin.Security`
* `Microsoft.Owin.Security.ActiveDirectory`
* `Microsoft.Owin.Security.Jwt`
* `Microsoft.Owin.Security.OAuth`
* `Owin`
* `System.IdentityModel.Tokens.Jwt`

## <a name="code-changes"></a><span data-ttu-id="f0aae-109">Změny kódu</span><span class="sxs-lookup"><span data-stu-id="f0aae-109">Code changes</span></span>
### <a name="code-files-were-added-tooyour-project"></a><span data-ttu-id="f0aae-110">Soubory kódu byly přidány tooyour projektu</span><span class="sxs-lookup"><span data-stu-id="f0aae-110">Code files were added tooyour project</span></span>
<span data-ttu-id="f0aae-111">Spuštění třídu ověřování **App_Start/Startup.Auth.cs** byl přidán tooyour projekt obsahující logika spuštění pro ověřování Azure AD.</span><span class="sxs-lookup"><span data-stu-id="f0aae-111">An authentication startup class, **App_Start/Startup.Auth.cs** was added tooyour project containing startup logic for Azure AD authentication.</span></span>

### <a name="startup-code-was-added-tooyour-project"></a><span data-ttu-id="f0aae-112">Kód spuštění byl přidán tooyour projektu</span><span class="sxs-lookup"><span data-stu-id="f0aae-112">Startup code was added tooyour project</span></span>
<span data-ttu-id="f0aae-113">Pokud jste již měli třída při spuštění ve vašem projektu, hello **konfigurace** metoda byla aktualizovaná tooinclude volání příliš`ConfigureAuth(app)`.</span><span class="sxs-lookup"><span data-stu-id="f0aae-113">If you already had a Startup class in your project, hello **Configuration** method was updated tooinclude a call too`ConfigureAuth(app)`.</span></span> <span data-ttu-id="f0aae-114">Třída při spuštění, jinak hodnota přidala tooyour projektu.</span><span class="sxs-lookup"><span data-stu-id="f0aae-114">Otherwise, a Startup class was added tooyour project.</span></span>

### <a name="your-appconfig-or-webconfig-file-has-new-configuration-values"></a><span data-ttu-id="f0aae-115">Váš soubor app.config nebo web.config obsahuje nové hodnoty konfigurace.</span><span class="sxs-lookup"><span data-stu-id="f0aae-115">Your app.config or web.config file has new configuration values.</span></span>
<span data-ttu-id="f0aae-116">byly přidány následující položky konfigurace Hello.</span><span class="sxs-lookup"><span data-stu-id="f0aae-116">hello following configuration entries have been added.</span></span>

```
    <appSettings>
            <add key="ida:ClientId" value="ClientId from hello new Azure AD App" />
            <add key="ida:Tenant" value="Your selected Azure AD Tenant" />
            <add key="ida:Audience" value="hello App ID Uri from hello wizard" />
    </appSettings>`
```

### <a name="an-azure-ad-app-was-created"></a><span data-ttu-id="f0aae-117">Byla vytvořena aplikace Azure AD</span><span class="sxs-lookup"><span data-stu-id="f0aae-117">An Azure AD App was created</span></span>
<span data-ttu-id="f0aae-118">Azure AD aplikace byla vytvořena v hello adresář, který jste vybrali v Průvodci hello.</span><span class="sxs-lookup"><span data-stu-id="f0aae-118">An Azure AD Application was created in hello directory that you selected in hello wizard.</span></span>

[<span data-ttu-id="f0aae-119">Další informace o službě Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="f0aae-119">Learn more about Azure Active Directory</span></span>](https://azure.microsoft.com/services/active-directory/)

## <a name="if-i-checked-disable-individual-user-accounts-authentication-what-additional-changes-were-made-toomy-project"></a><span data-ttu-id="f0aae-120">Pokud po kontrole *zakázat jednotlivé uživatelské účty ověřování*, jaké další změny byly provedeny toomy projektu?</span><span class="sxs-lookup"><span data-stu-id="f0aae-120">If I checked *disable Individual User Accounts authentication*, what additional changes were made toomy project?</span></span>
<span data-ttu-id="f0aae-121">Byly odebrány odkazy balíčku NuGet, a soubory byly odebrány a zálohovat.</span><span class="sxs-lookup"><span data-stu-id="f0aae-121">NuGet package references were removed, and files were removed and backed up.</span></span> <span data-ttu-id="f0aae-122">V závislosti na stavu hello projektu může mít toomanually odebrat další odkazy nebo soubory, nebo změnit kód podle potřeby.</span><span class="sxs-lookup"><span data-stu-id="f0aae-122">Depending on hello state of your project, you may have toomanually remove additional references or files, or modify code as appropriate.</span></span>

### <a name="nuget-package-references-removed-for-those-present"></a><span data-ttu-id="f0aae-123">Odkazy na balíček NuGet odebrat (pro ty existuje)</span><span class="sxs-lookup"><span data-stu-id="f0aae-123">NuGet package references removed (for those present)</span></span>
* `Microsoft.AspNet.Identity.Core`
* `Microsoft.AspNet.Identity.EntityFramework`
* `Microsoft.AspNet.Identity.Owin`

### <a name="code-files-backed-up-and-removed-for-those-present"></a><span data-ttu-id="f0aae-124">Soubory kódu zálohovat a odebrat (pro ty existuje)</span><span class="sxs-lookup"><span data-stu-id="f0aae-124">Code files backed up and removed (for those present)</span></span>
<span data-ttu-id="f0aae-125">Každý z následujících souborů byl zálohován a odebrat z projektu hello.</span><span class="sxs-lookup"><span data-stu-id="f0aae-125">Each of following files was backed up and removed from hello project.</span></span> <span data-ttu-id="f0aae-126">Záložní soubory jsou umístěny ve složce 'Zálohování' v kořenovém adresáři projektu hello hello.</span><span class="sxs-lookup"><span data-stu-id="f0aae-126">Backup files are located in a 'Backup' folder at hello root of hello project's directory.</span></span>

* `App_Start\IdentityConfig.cs`
* `Controllers\AccountController.cs`
* `Controllers\ManageController.cs`
* `Models\IdentityModels.cs`
* `Providers\ApplicationOAuthProvider.cs`

### <a name="code-files-backed-up-for-those-present"></a><span data-ttu-id="f0aae-127">Soubory kódu zálohovat (pro ty existuje)</span><span class="sxs-lookup"><span data-stu-id="f0aae-127">Code files backed up (for those present)</span></span>
<span data-ttu-id="f0aae-128">Každý z následujících souborů byla zálohována před nahrazují.</span><span class="sxs-lookup"><span data-stu-id="f0aae-128">Each of following files was backed up before being replaced.</span></span> <span data-ttu-id="f0aae-129">Záložní soubory jsou umístěny ve složce 'Zálohování' v kořenovém adresáři projektu hello hello.</span><span class="sxs-lookup"><span data-stu-id="f0aae-129">Backup files are located in a 'Backup' folder at hello root of hello project's directory.</span></span>

* `Startup.cs`
* `App_Start\Startup.Auth.cs`

## <a name="if-i-checked-read-directory-data-what-additional-changes-were-made-toomy-project"></a><span data-ttu-id="f0aae-130">Pokud po kontrole *čtení dat adresáře*, jaké další změny byly provedeny toomy projektu?</span><span class="sxs-lookup"><span data-stu-id="f0aae-130">If I checked *Read directory data*, what additional changes were made toomy project?</span></span>
### <a name="additional-changes-were-made-tooyour-appconfig-or-webconfig"></a><span data-ttu-id="f0aae-131">Další změny, které byly provedeny tooyour app.config nebo web.config</span><span class="sxs-lookup"><span data-stu-id="f0aae-131">Additional changes were made tooyour app.config or web.config</span></span>
<span data-ttu-id="f0aae-132">Hello následující další konfigurační položky byly přidány.</span><span class="sxs-lookup"><span data-stu-id="f0aae-132">hello following additional configuration entries have been added.</span></span>

```
    <appSettings>
        <add key="ida:Password" value="Your Azure AD App's new password" />
    </appSettings>`
```

### <a name="your-azure-active-directory-app-was-updated"></a><span data-ttu-id="f0aae-133">Aplikace Azure Active Directory byla aktualizována.</span><span class="sxs-lookup"><span data-stu-id="f0aae-133">Your Azure Active Directory App was updated</span></span>
<span data-ttu-id="f0aae-134">Aplikace Azure Active Directory byl aktualizovaný tooinclude hello *čtení dat adresáře* oprávnění a další klíč byl vytvořen, které se pak použije jako hello *ida: heslo* v hello `web.config` souboru.</span><span class="sxs-lookup"><span data-stu-id="f0aae-134">Your Azure Active Directory App was updated tooinclude hello *Read directory data* permission and an additional key was created which was then used as hello *ida:Password* in hello `web.config` file.</span></span>

## <a name="next-steps"></a><span data-ttu-id="f0aae-135">Další kroky</span><span class="sxs-lookup"><span data-stu-id="f0aae-135">Next steps</span></span>
- [<span data-ttu-id="f0aae-136">Další informace o službě Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="f0aae-136">Learn more about Azure Active Directory</span></span>](https://azure.microsoft.com/services/active-directory/)

