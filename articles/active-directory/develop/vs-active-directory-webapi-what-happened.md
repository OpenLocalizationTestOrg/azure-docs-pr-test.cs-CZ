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
# <a name="what-happened-toomy-webapi-project-visual-studio-azure-active-directory-connected-service"></a>Co se stalo toomy WebApi projektu (Visual Studio Azure Active Directory připojení služby)
> [!div class="op_single_selector"]
> * [Začínáme](vs-active-directory-webapi-getting-started.md)
> * [Co se přihodilo](vs-active-directory-webapi-what-happened.md)
> 
> 

## <a name="references-have-been-added"></a>Byly přidány odkazy
### <a name="nuget-package-references"></a>Odkazy na balíček NuGet
* `Microsoft.Owin`
* `Microsoft.Owin.Host.SystemWeb`
* `Microsoft.Owin.Security`
* `Microsoft.Owin.Security.ActiveDirectory`
* `Microsoft.Owin.Security.Jwt`
* `Microsoft.Owin.Security.OAuth`
* `Owin`
* `System.IdentityModel.Tokens.Jwt`

### <a name="net-references"></a>Odkazy na rozhraní .NET
* `Microsoft.Owin`
* `Microsoft.Owin.Host.SystemWeb`
* `Microsoft.Owin.Security`
* `Microsoft.Owin.Security.ActiveDirectory`
* `Microsoft.Owin.Security.Jwt`
* `Microsoft.Owin.Security.OAuth`
* `Owin`
* `System.IdentityModel.Tokens.Jwt`

## <a name="code-changes"></a>Změny kódu
### <a name="code-files-were-added-tooyour-project"></a>Soubory kódu byly přidány tooyour projektu
Spuštění třídu ověřování **App_Start/Startup.Auth.cs** byl přidán tooyour projekt obsahující logika spuštění pro ověřování Azure AD.

### <a name="startup-code-was-added-tooyour-project"></a>Kód spuštění byl přidán tooyour projektu
Pokud jste již měli třída při spuštění ve vašem projektu, hello **konfigurace** metoda byla aktualizovaná tooinclude volání příliš`ConfigureAuth(app)`. Třída při spuštění, jinak hodnota přidala tooyour projektu.

### <a name="your-appconfig-or-webconfig-file-has-new-configuration-values"></a>Váš soubor app.config nebo web.config obsahuje nové hodnoty konfigurace.
byly přidány následující položky konfigurace Hello.

```
    <appSettings>
            <add key="ida:ClientId" value="ClientId from hello new Azure AD App" />
            <add key="ida:Tenant" value="Your selected Azure AD Tenant" />
            <add key="ida:Audience" value="hello App ID Uri from hello wizard" />
    </appSettings>`
```

### <a name="an-azure-ad-app-was-created"></a>Byla vytvořena aplikace Azure AD
Azure AD aplikace byla vytvořena v hello adresář, který jste vybrali v Průvodci hello.

[Další informace o službě Azure Active Directory](https://azure.microsoft.com/services/active-directory/)

## <a name="if-i-checked-disable-individual-user-accounts-authentication-what-additional-changes-were-made-toomy-project"></a>Pokud po kontrole *zakázat jednotlivé uživatelské účty ověřování*, jaké další změny byly provedeny toomy projektu?
Byly odebrány odkazy balíčku NuGet, a soubory byly odebrány a zálohovat. V závislosti na stavu hello projektu může mít toomanually odebrat další odkazy nebo soubory, nebo změnit kód podle potřeby.

### <a name="nuget-package-references-removed-for-those-present"></a>Odkazy na balíček NuGet odebrat (pro ty existuje)
* `Microsoft.AspNet.Identity.Core`
* `Microsoft.AspNet.Identity.EntityFramework`
* `Microsoft.AspNet.Identity.Owin`

### <a name="code-files-backed-up-and-removed-for-those-present"></a>Soubory kódu zálohovat a odebrat (pro ty existuje)
Každý z následujících souborů byl zálohován a odebrat z projektu hello. Záložní soubory jsou umístěny ve složce 'Zálohování' v kořenovém adresáři projektu hello hello.

* `App_Start\IdentityConfig.cs`
* `Controllers\AccountController.cs`
* `Controllers\ManageController.cs`
* `Models\IdentityModels.cs`
* `Providers\ApplicationOAuthProvider.cs`

### <a name="code-files-backed-up-for-those-present"></a>Soubory kódu zálohovat (pro ty existuje)
Každý z následujících souborů byla zálohována před nahrazují. Záložní soubory jsou umístěny ve složce 'Zálohování' v kořenovém adresáři projektu hello hello.

* `Startup.cs`
* `App_Start\Startup.Auth.cs`

## <a name="if-i-checked-read-directory-data-what-additional-changes-were-made-toomy-project"></a>Pokud po kontrole *čtení dat adresáře*, jaké další změny byly provedeny toomy projektu?
### <a name="additional-changes-were-made-tooyour-appconfig-or-webconfig"></a>Další změny, které byly provedeny tooyour app.config nebo web.config
Hello následující další konfigurační položky byly přidány.

```
    <appSettings>
        <add key="ida:Password" value="Your Azure AD App's new password" />
    </appSettings>`
```

### <a name="your-azure-active-directory-app-was-updated"></a>Aplikace Azure Active Directory byla aktualizována.
Aplikace Azure Active Directory byl aktualizovaný tooinclude hello *čtení dat adresáře* oprávnění a další klíč byl vytvořen, které se pak použije jako hello *ida: heslo* v hello `web.config` souboru.

## <a name="next-steps"></a>Další kroky
- [Další informace o službě Azure Active Directory](https://azure.microsoft.com/services/active-directory/)

