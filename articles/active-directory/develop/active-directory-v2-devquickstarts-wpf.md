---
title: "aaaAzure služby Active Directory v2.0 nativní aplikace .NET | Microsoft Docs"
description: "Jak toobuild nativní aplikace .NET, přihlášení uživatelů s Account Microsoft i osobní a pracovní nebo školní účty."
services: active-directory
documentationcenter: 
author: jmprieur
manager: mbaldwin
editor: 
ms.assetid: 46d81e09-bad0-44ce-9026-881805976e72
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 07/30/2016
ms.author: jmprieur
ms.custom: aaddev
ms.openlocfilehash: 9418eeba02b800feee5cb00219574eb16506f0a1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="add-sign-in-tooa-windows-desktop-app"></a>Přidat aplikaci Windows Desktop tooa přihlášení
S koncovým bodem v2.0 hello hello můžete rychle přidat ověřování tooyour desktopové aplikace s podporou pro oba osobní účty Microsoft a pracovní nebo školní účty.  Je také umožňuje vaší aplikaci toosecurely komunikovat s back-end webové rozhraní api, a také [hello Microsoft Graph](https://graph.microsoft.io) a několik hello [Office 365 jednotné rozhraní API](https://www.msdn.com/office/office365/howto/authenticate-Office-365-APIs-using-v2).

> [!NOTE]
> Ne všechny scénáře Azure Active Directory (AD) a funkce jsou podporovány koncového bodu v2.0 hello.  toodetermine Pokud byste měli používat koncového bodu v2.0 hello, přečtěte si informace o [v2.0 omezení](active-directory-v2-limitations.md).
> 
> 

Pro [nativní aplikace .NET, které běží na zařízení](active-directory-v2-flows.md#mobile-and-native-apps), Azure AD poskytuje knihovnu ověřování Identity Microsoft nebo MSAL hello.  Jediným účelem je MSAL v životnosti je toomake ho snadno pro vaše aplikace tooget tokeny pro volání webové služby.  toodemonstrate, jak lze snadno, je, zde jsme budete sestavení aplikace seznam úkolů WPF .NET který:

* Přihlásí hello uživatele v & získá přístup k tokeny pomocí hello [protokol ověřování OAuth 2.0](active-directory-v2-protocols.md).
* Bezpečně volá back-end seznamu úkolů webové služby, která je také zabezpečeny OAuth 2.0.
* Přihlásí uživatele hello se.

## <a name="download-sample-code"></a>Stáhněte si ukázkový kód
Hello kód v tomto kurzu se udržuje [na Githubu](https://github.com/AzureADQuickStarts/AppModelv2-NativeClient-DotNet).  toofollow společně, můžete [stáhnout kostru aplikace hello jako ZIP](https://github.com/AzureADQuickStarts/AppModelv2-NativeClient-DotNet/archive/skeleton.zip) nebo hello kostru klonovat:

    git clone --branch skeleton https://github.com/AzureADQuickStarts/AppModelv2-NativeClient-DotNet.git

aplikace Hello dokončit, je k dispozici na konci hello také tohoto kurzu.

## <a name="register-an-app"></a>Registrace aplikace
Vytvoření nové aplikace v [apps.dev.microsoft.com](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList), nebo postupujte podle těchto [podrobné kroky](active-directory-v2-app-registration.md).  Zkontrolujte, že:

* Poznamenejte hello **Id aplikace** přiřazen tooyour aplikace, budete ho potřebovat brzy k dispozici.
* Přidat hello **Mobile** platformu pro vaši aplikaci.

## <a name="install--configure-msal"></a>Instalace a konfigurace MSAL
Teď, když máte aplikaci zaregistrovat se společností Microsoft, můžete nainstalovat MSAL a zadejte kód, týkající se identity.  Aby MSAL toobe možné toocommunicate hello koncového bodu v2.0, budete potřebovat tooprovide její některé informace o registraci vaší aplikace.

* Začněte tím, že přidání MSAL toohello TodoListClient projekt pomocí hello Konzola správce balíčků.

```
PM> Install-Package Microsoft.Identity.Client -ProjectName TodoListClient -IncludePrerelease
```

* V projektu TodoListClient hello otevřete `app.config`.  Nahraďte hodnoty hello hello elementů v hello `<appSettings>` hello tooreflect část hodnoty, vstup do portálu pro registraci aplikace hello.  Váš kód bude odkazovat tyto hodnoty vždy, když používá MSAL.
  
  * Hello `ida:ClientId` je hello **Id aplikace** vaší aplikace, které jste zkopírovali z portálu hello.
* V projektu hello seznamu úkolů služby otevřete `web.config` v kořenovém hello hello projektu.  
  
  * Nahraďte hello `ida:Audience` hodnotu s hello stejné **Id aplikace** z portálu hello.

## <a name="use-msal-tooget-tokens"></a>Použít MSAL tooget tokeny
Hello základní princip za MSAL je vždy, když aplikace potřebuje přístupový token, jednoduše volat `app.AcquireToken(...)`, a MSAL hello rest.  

* V hello `TodoListClient` projekt, otevřete `MainWindow.xaml.cs` a vyhledejte hello `OnInitialized(...)` metoda.  prvním krokem Hello je tooinitialize vaší aplikace `PublicClientApplication` -MSAL na primární Třída reprezentující nativních aplikací.  Toto je, kde je předat MSAL hello souřadnice potřebuje toocommunicate s Azure AD a určit, jak toocache tokeny.

```C#
protected override async void OnInitialized(EventArgs e)
{
        base.OnInitialized(e);

        app = new PublicClientApplication(new FileCache());
        AuthenticationResult result = null;
        ...
}
```

* Po spuštění aplikace hello jsme má toocheck a v tématu, pokud uživatel hello je již přihlášen do aplikace hello.  Však zatím nechceme tooinvoke uživatelského rozhraní přihlášení – vytočit hello uživatele, klikněte na tlačítko "Přihlásit" toodo.  Také v hello `OnInitialized(...)` metoda:

```C#
// As hello app starts, we want toocheck toosee if hello user is already signed in.
// You can do so by trying tooget a token from MSAL, using hello method
// AcquireTokenSilent.  This forces MSAL toothrow an exception if it cannot
// get a token for hello user without showing a UI.
try
{
    result = await app.AcquireTokenSilentAsync(new string[] { clientId });
    // If we got here, a valid token is in hello cache - or MSAL was able tooget a new oen via refresh token.
    // Proceed toofetch hello user's tasks from hello TodoListService via hello GetTodoList() method.

    SignInButton.Content = "Clear Cache";
    GetTodoList();
}
catch (MsalException ex)
{
    if (ex.ErrorCode == "user_interaction_required")
    {
        // If user interaction is required, hello app should take no action,
        // and simply show hello user hello sign in button.
    }
    else
    {
        // Here, we catch all other MsalExceptions
        string message = ex.Message;
        if (ex.InnerException != null)
        {
            message += "Inner Exception : " + ex.InnerException.Message;
        }
        MessageBox.Show(message);
    }
}

```

* Pokud hello uživatel není přihlášený a klepnutím na tlačítko "Sign In" hello, jsme má tooinvoke přihlašovací údaje uživatelského rozhraní a mít zadat své přihlašovací údaje uživatele hello.  Implementujte hello přihlašovací tlačítko rutinu:

```C#
private async void SignIn(object sender = null, RoutedEventArgs args = null)
{
        // TODO: Sign hello user out if they clicked hello "Clear Cache" button

// If hello user clicked hello 'Sign-In' button, force
// MSAL tooprompt hello user for credentials by using
// AcquireTokenAsync, a method that is guaranteed tooshow a prompt toohello user.
// MSAL will get a token for hello TodoListService and cache it for you.

AuthenticationResult result = null;
try
{
    result = await app.AcquireTokenAsync(new string[] { clientId });
    SignInButton.Content = "Clear Cache";
    GetTodoList();
}
catch (MsalException ex)
{
    // If MSAL cannot get a token, it will throw an exception.
    // If hello user canceled hello login, it will result in the
    // error code 'authentication_canceled'.

    if (ex.ErrorCode == "authentication_canceled")
    {
        MessageBox.Show("Sign in was canceled by hello user");
    }
    else
    {
        // An unexpected error occurred.
        string message = ex.Message;
        if (ex.InnerException != null)
        {
            message += "Inner Exception : " + ex.InnerException.Message;
        }

        MessageBox.Show(message);
    }

    return;
}


}
```

* Pokud hello uživatel úspěšně přihlásí, bude přijímat a token do mezipaměti pro vás MSAL a můžete pokračovat toocall hello `GetTodoList()` metoda s jistotou.  Všechno, co opustil tooget úlohy uživatele je tooimplement hello `GetTodoList()` metoda.

```C#
private async void GetTodoList()
{

AuthenticationResult result = null;
try
{
    // Here, we try tooget an access token toocall hello TodoListService
    // without invoking any UI prompt.  AcquireTokenSilentAsync forces
    // MSAL toothrow an exception if it cannot get a token silently.


    result = await app.AcquireTokenSilentAsync(new string[] { clientId });
}
catch (MsalException ex)
{
    // MSAL couldn't get a token silently, so show hello user a message
    // and let them click hello Sign-In button.

    if (ex.ErrorCode == "user_interaction_required")
    {
        MessageBox.Show("Please sign in first");
        SignInButton.Content = "Sign In";
    }
    else
    {
        // In any other case, an unexpected error occurred.

        string message = ex.Message;
        if (ex.InnerException != null)
        {
            message += "Inner Exception : " + ex.InnerException.Message;
        }
        MessageBox.Show(message);
    }

    return;
}

// Once hello token has been returned by MSAL,
// add it toohello http authorization header,
// before making hello call tooaccess hello tooDo list service.

httpClient.DefaultRequestHeaders.Authorization = new AuthenticationHeaderValue("Bearer", result.Token);


        ...
...


- When hello user is done managing their To-Do List, they may finally sign out of hello app by clicking hello "Clear Cache" button.

```C#
private async void SignIn(object sender = null, RoutedEventArgs args = null)
{
        // If hello user clicked hello 'clear cache' button,
        // clear hello MSAL token cache and show hello user as signed out.
        // It's also necessary tooclear hello cookies from hello browser
        // control so hello next user has a chance toosign in.

        if (SignInButton.Content.ToString() == "Clear Cache")
        {
                TodoList.ItemsSource = string.Empty;
                app.UserTokenCache.Clear(app.ClientId);
                ClearCookies();
                SignInButton.Content = "Sign In";
                return;
        }

        ...
```

## <a name="run"></a>Spusťte
Blahopřejeme! Teď máte funkční aplikaci WPF v rozhraní .NET, která má hello možnost tooauthenticate uživatelé & bezpečně volání webového rozhraní API pomocí OAuth 2.0.  Spusťte oba projekty a přihlaste se pomocí osobního účtu Microsoft nebo pracovní nebo školní účet.  Přidejte seznam úkolů uživatele toothat úlohy.  Odhlásit se a přihlaste se zpět jako jiný uživatel tooview jejich seznamu úkolů.  Zavření aplikace hello a znovu jej spusťte.  Všimněte si, jak hello uživatelské relace zůstává nedotčeno – je to způsobeno hello aplikace ukládá do mezipaměti tokenů v místním souboru.

MSAL umožňuje snadno tooincorporate běžné funkce identity do vaší aplikace pomocí osobní i pracovní účty.  Postará všechny pracovní dirty hello vám - Správa mezipaměti, podpora protokolu OAuth, prezentací hello uživatele s přihlašovacími údaji uživatelského rozhraní, aktualizace vypršení platnosti tokenů a další.  Všechny skutečně potřebujete tooknow je jednoho volání rozhraní API `app.AcquireTokenAsync(...)`.

Pro referenci hello dokončit ukázka (bez vašich hodnot nastavení) [je k dispozici jako ZIP zde](https://github.com/AzureADQuickStarts/AppModelv2-NativeClient-DotNet/archive/complete.zip), nebo ji můžete klonovat z Githubu:

```git clone --branch complete https://github.com/AzureADQuickStarts/AppModelv2-NativeClient-DotNet.git```

## <a name="next-steps"></a>Další kroky
Nyní se můžete přesunout na pokročilejší témata.  Může být vhodné tootry:

* [Zabezpečení hello TodoListService webového rozhraní API s koncovým bodem v2.0 hello](active-directory-v2-devquickstarts-dotnet-api.md)

Další zdroje projděte si:  

* [Příručka vývojáře v2.0 Hello >>](active-directory-appmodel-v2-overview.md)
* [Značka "msal" StackOverflow >>](http://stackoverflow.com/questions/tagged/msal)

## <a name="get-security-updates-for-our-products"></a>Získejte bezpečnostní aktualizace našich produktů
Doporučujeme vám tooget oznámení o bezpečnostních incidentech navštivte stránky [tuto stránku](https://technet.microsoft.com/security/dd252948) a přihlášení k odběru tooSecurity Advisory Alerts.

