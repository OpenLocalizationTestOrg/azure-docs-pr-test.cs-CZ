---
title: ".NET aaaAzure AD Začínáme | Microsoft Docs"
description: "Tom, jak toobuild .NET Windows Desktop aplikace, která se integruje se službou Azure AD pro přihlašování a volá Azure AD chráněný rozhraní API pomocí metody OAuth."
services: active-directory
documentationcenter: .net
author: jmprieur
manager: mbaldwin
editor: 
ms.assetid: ed33574f-6fa3-402c-b030-fae76fba84e1
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 01/23/2017
ms.author: jmprieur
ms.custom: aaddev
ms.openlocfilehash: c09b358f24c7bfb371b34cf72ca48c0a45042f5f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="integrate-azure-ad-into-a-windows-desktop-wpf-app"></a>Integrace Azure AD do aplikace Windows Desktop WPF
[!INCLUDE [active-directory-devquickstarts-switcher](../../../includes/active-directory-devquickstarts-switcher.md)]

[!INCLUDE [active-directory-devguide](../../../includes/active-directory-devguide.md)]

Pokud vyvíjíte aplikace pracovní plochy, Azure AD je jednoduchá a přímočará pro tooauthenticate můžete uživatelům pomocí jejich účtů služby Active Directory.  Umožňuje také toosecurely vaše aplikace využívat žádné webové rozhraní API, které jsou chráněné službou Azure AD, jako například hello rozhraní API Office 365 nebo hello rozhraní API služby Azure.

Pro nativní klienty .NET, které vyžadují tooaccess chráněné zdroje Azure AD poskytuje hello knihovna ověřování Active Directory nebo ADAL.  Jediným účelem je ADAL v životnosti je toomake ho snadno pro vaše aplikace tooget přístupových tokenů.  toodemonstrate, jak lze snadno, je, zde jsme budete sestavení aplikace seznam úkolů WPF .NET který:

* Získá přístup k tokeny pro volání rozhraní API Azure AD Graph hello pomocí hello [protokol ověřování OAuth 2.0](https://msdn.microsoft.com/library/azure/dn645545.aspx).
* Vyhledá adresář pro uživatele s danou alias.
* Uživatelé přihlásí se.

toobuild hello dokončení pracovní aplikace, budete muset:

1. Registrace vaší aplikace s Azure AD.
2. Instalace a konfigurace ADAL.
3. Pomocí ADAL tooget tokeny z Azure AD.

spuštění, tooget [stáhnout kostru aplikace hello](https://github.com/AzureADQuickStarts/NativeClient-DotNet/archive/skeleton.zip) nebo [stažení ukázky hello Dokončit](https://github.com/AzureADQuickStarts/NativeClient-DotNet/archive/complete.zip).  Budete také potřebovat klient služby Azure AD, ve kterém můžete vytvořit uživatele a zaregistrovat aplikaci.  Pokud ještě nemáte klienta, [zjistěte, jak tooget jeden](active-directory-howto-tenant.md).

## <a name="1-register-hello-directorysearcher-application"></a>1. Zaregistrovat hello DirectorySearcher aplikace
tooenable tokeny tooget vaší aplikace, je nutné nejdříve tooregister ji ve službě Azure AD klienta a udělit mu oprávnění tooaccess hello Azure AD Graph API:

1. Přihlaste se toohello [portál Azure](https://portal.azure.com).
2. Na horním panelu hello, klikněte na tlačítko na vašem účtu a v části hello **Directory** vyberte klienta hello služby Active Directory, kde chcete tooregister vaší aplikace.
3. Klikněte na **více služeb** v hello navigaci vlevo a zvolte **Azure Active Directory**.
4. Klikněte na **registrace aplikace** a zvolte **přidat**.
5. Postupujte podle pokynů hello a vytvořte novou **nativní klientská aplikace**.
  * Hello **název** z hello aplikace popíše aplikaci tooend uživatelé
  * Hello **identifikátor Uri pro přesměrování** schématu a řetězec kombinací, Azure AD použijete tooreturn odpovědi tokenu.  Zadejte například hodnotu konkrétní tooyour aplikace pomocí `http://DirectorySearcher`.
6. Po dokončení registrace AAD přiřadí vaší aplikace, jedinečné ID aplikace  Tuto hodnotu budete potřebovat v dalších částech hello, takže zkopírujte jej ze stránky aplikace hello.
7. Z hello **nastavení** vyberte **požadovaných oprávnění** a zvolte **přidat**. Vyberte hello **Microsoft Graph** jako hello rozhraní API a přidejte hello **čtení dat adresáře** oprávnění v rámci **delegovaná oprávnění**.  Tato akce povolí vaší aplikace tooquery hello rozhraní Graph API pro uživatele.

## <a name="2-install--configure-adal"></a>2. Instalace a konfigurace ADAL
Teď, když máte aplikaci ve službě Azure AD, můžete nainstalovat ADAL a zadejte kód, týkající se identity.  Aby mohl mít toocommunicate ADAL toobe s Azure AD, je nutné tooprovide její některé informace o registraci vaší aplikace.

* Začněte tím, že přidání ADAL toohello DirectorySearcher projekt pomocí hello Konzola správce balíčků.

```
PM> Install-Package Microsoft.IdentityModel.Clients.ActiveDirectory
```

* V projektu DirectorySearcher hello otevřete `app.config`.  Nahraďte hodnoty hello hello elementů v hello `<appSettings>` hello tooreflect část hodnoty, vstup do hello portálu Azure.  Váš kód bude odkazovat tyto hodnoty vždy, když ho využívá ADAL.
  * Hello `ida:Tenant` je hello domény klienta služby Azure AD, např. contoso.onmicrosoft.com
  * Hello `ida:ClientId` je hello clientId vaší aplikace, které jste zkopírovali z portálu hello.
  * Hello `ida:RedirectUri` je hello přesměrování URL adresy jste zaregistrovali hello portálu.

## <a name="3----use-adal-tooget-tokens-from-aad"></a>3.    Použití ADAL tooGet tokeny od AAD
Hello základní princip za ADAL je, že vždy, když aplikace potřebuje přístupový token, jednoduše volá `authContext.AcquireTokenAsync(...)`, a ADAL hello rest.  

* V hello `DirectorySearcher` projekt, otevřete `MainWindow.xaml.cs` a vyhledejte hello `MainWindow()` metoda.  prvním krokem Hello je tooinitialize vaší aplikace `AuthenticationContext` -ADAL je primární třídou.  Toto je, kde je předat ADAL hello souřadnice potřebuje toocommunicate s Azure AD a určit, jak toocache tokeny.

```C#
public MainWindow()
{
    InitializeComponent();

    authContext = new AuthenticationContext(authority, new FileCache());

    CheckForCachedToken();
}
```

* Nyní najít hello `Search(...)` metodu, která bude vyvolán při hello uživatele cliks hello tlačítko "Search" v uživatelském rozhraní aplikace hello.  Tato metoda vytváří tooquery toohello Azure AD Graph API požadavek GET pro uživatele, jehož UPN začíná hello zadaný hledaný termín.  Ale v pořadí tooquery hello rozhraní Graph API, musíte tooinclude access_token v hello `Authorization` záhlaví hello – to přichází ADAL požadavku.

```C#
private async void Search(object sender, RoutedEventArgs e)
{
    // Validate hello Input String
    if (string.IsNullOrEmpty(SearchText.Text))
    {
        MessageBox.Show("Please enter a value for hello tooDo item name");
        return;
    }

    // Get an Access Token for hello Graph API
    AuthenticationResult result = null;
    try
    {
        result = await authContext.AcquireTokenAsync(graphResourceId, clientId, redirectUri, new PlatformParameters(PromptBehavior.Auto));
        UserNameLabel.Content = result.UserInfo.DisplayableId;
        SignOutButton.Visibility = Visibility.Visible;
    }
    catch (AdalException ex)
    {
        // An unexpected error occurred, or user canceled hello sign in.
        if (ex.ErrorCode != "access_denied")
            MessageBox.Show(ex.Message);

        return;
    }

    ...
}
```
* Když vaše aplikace vyžaduje token voláním `AcquireTokenAsync(...)`, ADAL pokusí tooreturn token bez nutnosti hello uživatelské přihlašovací údaje.  Pokud ADAL zjistí, že tento uživatel hello je toosign v tooget token, se zobrazí dialogové okno přihlášení, shromažďování přihlašovacích údajů uživatele hello a vrací token, po úspěšném ověření.  Pokud ADAL nelze tooreturn token z jakéhokoli důvodu, vyvolá výjimku `AdalException`.
* Všimněte si, že hello `AuthenticationResult` objekt obsahuje `UserInfo` objekt, který lze použít toocollect informace může být nutné vaší aplikace.  V hello DirectorySearcher `UserInfo` je použité toocustomize hello uživatelském rozhraní aplikace s id uživatele hello.
* Když hello uživatel klikne na tlačítko "Odhlásit" hello, chceme tooensure, který hello další volání příliš`AcquireTokenAsync(...)` požádá uživatele toosign hello v.  Pomocí knihovny ADAL to je stejně snadná jako vymazání mezipamětí tokenů hello:

```C#
private void SignOut(object sender = null, RoutedEventArgs args = null)
{
    // Clear hello token cache
    authContext.TokenCache.Clear();

    ...
}
```

* Ale pokud hello uživatele není klikněte na tlačítko "Odhlásit" hello, budete chtít toomaintain hello uživatelské relace pro hello při příštím spuštění hello DirectorySearcher.  Při spuštění aplikace hello můžete zkontrolovat mezipamětí tokenů na ADAL pro existující token a hello uživatelského rozhraní se aktualizují odpovídajícím způsobem.  V hello `CheckForCachedToken()` metody volání jiného příliš`AcquireTokenAsync(...)`, tentokrát předávání v hello `PromptBehavior.Never` parametr.  `PromptBehavior.Never`ADAL bude informovat, že uživateli hello nesmí výzva k přihlášení a ADAL by měl místo vyvolána výjimka, pokud je nelze tooreturn token.

```C#
public async void CheckForCachedToken() 
{
    // As hello application starts, try tooget an access token without prompting hello user.  If one exists, show hello user as signed in.
    AuthenticationResult result = null;
    try
    {
        result = await authContext.AcquireTokenAsync(graphResourceId, clientId, redirectUri, new PlatformParameters(PromptBehavior.Never));
    }
    catch (AdalException ex)
    {
        if (ex.ErrorCode != "user_interaction_required")
        {
            // An unexpected error occurred.
            MessageBox.Show(ex.Message);
        }

        // If user interaction is required, proceed toomain page without singing hello user in.
        return;
    }

    // A valid token is in hello cache
    SignOutButton.Visibility = Visibility.Visible;
    UserNameLabel.Content = result.UserInfo.DisplayableId;
}
```

Blahopřejeme! Teď máte funkční aplikaci WPF v rozhraní .NET, která má hello možnost tooauthenticate uživatelé, bezpečně volání webového rozhraní API pomocí OAuth 2.0 a získat základní informace o uživateli hello.  Pokud jste to ještě neudělali, nyní je čas toopopulate hello vašeho klienta s některými uživateli.  Spuštění aplikace DirectorySearcher a přihlaste se pomocí jeden z těchto uživatelů.  Hledání jiných uživatelů podle jejich UPN.  Zavření aplikace hello a znovu jej spusťte.  Všimněte si, jak hello uživatelské relace zůstává beze změn.  Odhlásit se a přihlaste se zpět jako jiný uživatel.

ADAL umožňuje snadno tooincorporate všechny tyto běžné funkce identity do aplikace.  Postará všechny pracovní dirty hello vám - Správa mezipaměti, podpora protokolu OAuth, prezentací hello uživatele s přihlašovacími údaji uživatelského rozhraní, aktualizace vypršení platnosti tokenů a další.  Všechny skutečně potřebujete tooknow je jednoho volání rozhraní API `authContext.AcquireTokenAsync(...)`.

Pro odkaz, hello dokončit ukázka (bez vašich hodnot nastavení) zajišťuje [zde](https://github.com/AzureADQuickStarts/NativeClient-DotNet/archive/complete.zip).  Nyní se můžete přesunout na tooadditional scénáře.  Může být vhodné tootry:

[Zabezpečení rozhraní .NET webového rozhraní API pomocí Azure AD >>](active-directory-devquickstarts-webapi-dotnet.md)

[!INCLUDE [active-directory-devquickstarts-additional-resources](../../../includes/active-directory-devquickstarts-additional-resources.md)]

