---
title: aaaAzure Active Directory B2C | Microsoft Docs
description: "Jak toobuild desktopová aplikace systému Windows, zahrnuje přihlášení, registrace a správy profilů pomocí Azure Active Directory B2C."
services: active-directory-b2c
documentationcenter: .net
author: dstrockis
manager: mbaldwin
editor: 
ms.assetid: 9da14362-8216-4485-960e-af17cd5ba3bd
ms.service: active-directory-b2c
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 01/07/2017
ms.author: dastrock
ms.openlocfilehash: f22b0299ff74bfba2f3fea88f006da609859dda5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-ad-b2c-build-a-windows-desktop-app"></a>Azure AD B2C: Sestavení aplikace na ploše systému Windows
Pomocí Azure Active Directory (Azure AD) B2C můžete přidat výkonné samoobslužná služba identity management funkce tooyour desktopová aplikace v několika krocích. Tento článek vám ukáže, jak toocreate .NET Windows Presentation Foundation (WPF) "seznam úkolů" aplikaci, která zahrnuje uživatelskou registraci, přihlašování a správu profilu. aplikace Hello bude zahrnují podporu pro registraci a přihlaste se pomocí uživatelského jména nebo e-mailu. Podporu registrace a přihlašování bude zahrnovat také pomocí sociálních účty například Facebook nebo Google.

## <a name="get-an-azure-ad-b2c-directory"></a>Získání adresáře služby Azure AD B2C
Před použitím Azure AD B2C musíte vytvořit adresář, nebo klienta.  Adresář je kontejner pro všechny vaše uživatele, aplikace, skupiny a další. Pokud ho ještě nemáte, [vytvořte adresář B2C](active-directory-b2c-get-started.md) předtím, než budete pokračovat.

## <a name="create-an-application"></a>Vytvoření aplikace
Dále musíte toocreate aplikace ve svém adresáři B2C. Díky tomu získá informace o Azure AD, se vyžaduje toosecurely komunikaci s vaší aplikací. toocreate na aplikace, postupujte podle [tyto pokyny](active-directory-b2c-app-registration.md).  Ujistěte se, že:

* Zahrnout **nativního klienta** v aplikaci hello.
* Kopírování hello **identifikátor URI pro přesměrování** `urn:ietf:wg:oauth:2.0:oob`. Je hello výchozí adresa URL pro tuto ukázku kódu.
* Kopírování hello **ID aplikace** který je přiřazený tooyour aplikace. Budete ho potřebovat později.

[!INCLUDE [active-directory-b2c-devquickstarts-v2-apps](../../includes/active-directory-b2c-devquickstarts-v2-apps.md)]

## <a name="create-your-policies"></a>Vytvořte svoje zásady
V Azure AD B2C je každé uživatelské rozhraní definováno [zásadou](active-directory-b2c-reference-policies.md). Tato ukázka kódu obsahuje tři činnosti identity: registrace, přihlášení a úprava profilu. Jak je popsáno v musíte pro každý typ toocreate zásadu [článku o zásadách](active-directory-b2c-reference-policies.md#create-a-sign-up-policy). Když vytvoříte hello tři zásady, nezapomeňte:

* Vyberte buď **registrace pomocí ID uživatele** nebo **e-mailu registrace** v okně zprostředkovatelé identity hello.
* Zvolit **Zobrazovaný název** a další atributy registrace ve svojí registrační zásadě.
* Zvolit **Zobrazovaný název** a deklarace identity **ID objektu** jako deklarace identity aplikace v každé zásadě. Můžete zvolit i další deklarace identity.
* Kopírování hello **název** po jejím vytvoření každé zásady. Měl by mít předponu hello `b2c_1_`.  Tyto názvy zásad budete potřebovat později.

[!INCLUDE [active-directory-b2c-devquickstarts-policy](../../includes/active-directory-b2c-devquickstarts-policy.md)]

Po úspěšném vytvoření hello tři zásady, jste připravené toobuild vaší aplikace.

## <a name="download-hello-code"></a>Stáhněte si kód hello
Hello kód pro tento kurz [je udržovaný na Githubu](https://github.com/AzureADQuickStarts/B2C-NativeClient-DotNet). Ukázka hello toobuild jako můžete přejít, můžete [stáhnout kostru projektu v souboru ZIP](https://github.com/AzureADQuickStarts/B2C-NativeClient-DotNet/archive/skeleton.zip). Můžete také hello kostru klonovat:

```
git clone --branch skeleton https://github.com/AzureADQuickStarts/B2C-NativeClient-DotNet.git
```

aplikace Hello dokončit, je také [k dispozici jako soubor ZIP](https://github.com/AzureADQuickStarts/B2C-NativeClient-DotNet/archive/complete.zip) nebo na hello `complete` větve hello stejného úložiště.

Po stažení ukázkového kódu hello hello otevřete Visual Studio .sln souboru tooget spustit. Hello `TaskClient` je projekt hello WPF plochy aplikace, která hello uživatel komunikuje se službou. Pro účely tohoto kurzu hello zavolá back-end úloh webového rozhraní API, hostované v Azure, která ukládá seznam úkolů každého uživatele.  Není nutné toobuild hello webového rozhraní API, jsme ji systémem pro vás už máte.

toolearn jak webové rozhraní API bezpečně ověřuje požadavků pomocí Azure AD B2C, podívejte se [webové rozhraní API Začínáme článku](active-directory-b2c-devquickstarts-api-dotnet.md).

## <a name="execute-policies"></a>Spuštění zásady
Aplikace komunikuje se službou Azure AD B2C odesláním zprávy ověřování, které zadejte zásady hello chtějí tooexecute jako součást požadavku hello protokolu HTTP. U aplikací klasické pracovní plochy rozhraní .NET, můžete použít hello náhled zpráv ověřování OAuth 2.0 toosend Microsoft ověřování knihovny (MSAL), zásady spouštění a získat tokeny tohoto volání webových rozhraní API.

### <a name="install-msal"></a>Nainstalujte MSAL
Přidat MSAL toohello `TaskClient` projektu pomocí hello Konzola správce balíčků Visual Studio.

```
PM> Install-Package Microsoft.Identity.Client -IncludePrerelease
```

### <a name="enter-your-b2c-details"></a>Zadejte podrobnosti o svém B2C
Soubor otevřete hello `Globals.cs` a nahraďte všechny hodnoty vlastností hello vlastní. Tato třída se používá v rámci `TaskClient` tooreference běžně používané hodnoty.

```C#
public static class Globals
{
    ...

    // TODO: Replace these five default with your own configuration values
    public static string tenant = "fabrikamb2c.onmicrosoft.com";
    public static string clientId = "90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6";
    public static string signInPolicy = "b2c_1_sign_in";
    public static string signUpPolicy = "b2c_1_sign_up";
    public static string editProfilePolicy = "b2c_1_edit_profile";

    ...
}
```

[!INCLUDE [active-directory-b2c-devquickstarts-tenant-name](../../includes/active-directory-b2c-devquickstarts-tenant-name.md)]

### <a name="create-hello-publicclientapplication"></a>Vytvoření hello PublicClientApplication
Hello primární třída MSAL je `PublicClientApplication`. Tato třída reprezentuje vaší aplikace v systému hello Azure AD B2C. Když hello initalizes aplikace, vytvořte instanci `PublicClientApplication` v `MainWindow.xaml.cs`. To lze použít v celé okno hello.

```C#
protected async override void OnInitialized(EventArgs e)
{
    base.OnInitialized(e);

    pca = new PublicClientApplication(Globals.clientId)
    {
        // MSAL implements an in-memory cache by default.  Since we want tokens toopersist when hello user closes hello app,
        // we've extended hello MSAL TokenCache and created a simple FileCache in this app.
        UserTokenCache = new FileCache(),
    };

    ...
```

### <a name="initiate-a-sign-up-flow"></a>Zahájení registrace toku
Když uživatel požádá toosigns nahoru, budete chtít tooinitiate registrace toku, který používá hello registrační zásadě, kterou jste vytvořili. Pomocí MSAL jen zavoláte `pca.AcquireTokenAsync(...)`. Hello parametry předáte příliš`AcquireTokenAsync(...)` určit, které token se zobrazí, hello zásady používané v žádosti o ověření hello a další.

```C#
private async void SignUp(object sender, RoutedEventArgs e)
{
    AuthenticationResult result = null;
    try
    {
        // Use hello app's clientId here as hello scope parameter, indicating that
        // you want a token toohello your app's backend web API (represented by
        // hello cloud hosted task API).  Use hello UiOptions.ForceLogin flag to
        // indicate tooMSAL that it should show a sign-up UI no matter what.
        result = await pca.AcquireTokenAsync(new string[] { Globals.clientId },
                string.Empty, UiOptions.ForceLogin, null, null, Globals.authority,
                Globals.signUpPolicy);

        // Upon success, indicate in hello app that hello user is signed in.
        SignInButton.Visibility = Visibility.Collapsed;
        SignUpButton.Visibility = Visibility.Collapsed;
        EditProfileButton.Visibility = Visibility.Visible;
        SignOutButton.Visibility = Visibility.Visible;

        // When hello request completes successfully, you can get user
        // information from hello AuthenticationResult
        UsernameLabel.Content = result.User.Name;

        // After hello sign up successfully completes, display hello user's To-Do List
        GetTodoList();
    }

    // Handle any exeptions that occurred during execution of hello policy.
    catch (MsalException ex)
    {
        if (ex.ErrorCode != "authentication_canceled")
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

### <a name="initiate-a-sign-in-flow"></a>Zahájit toku přihlášení
Můžete zahájit tok přihlášení jako hello stejným způsobem jako zahájení registrace toku. Když se uživatel přihlásí, ujistěte se, hello stejné volání tooMSAL, tentokrát pomocí přihlášení zásad:

```C#
private async void SignIn(object sender = null, RoutedEventArgs args = null)
{
    AuthenticationResult result = null;
    try
    {
        result = await pca.AcquireTokenAsync(new string[] { Globals.clientId },
                    string.Empty, UiOptions.ForceLogin, null, null, Globals.authority,
                    Globals.signInPolicy);
        ...
```

### <a name="initiate-an-edit-profile-flow"></a>Zahájit tok úpravy profilu
Znovu, můžete zásadu upravit profil spustit v hello stejným způsobem:

```C#
private async void EditProfile(object sender, RoutedEventArgs e)
{
    AuthenticationResult result = null;
    try
    {
        result = await pca.AcquireTokenAsync(new string[] { Globals.clientId },
                    string.Empty, UiOptions.ForceLogin, null, null, Globals.authority,
                    Globals.editProfilePolicy);
```

Ve všech těchto případech MSAL buď vrátí token v `AuthenticationResult` nebo vyvolá výjimku. Pokaždé, když získání tokenu z MSAL, můžete použít hello `AuthenticationResult.User` objektu tooupdate hello uživatelská data v hello aplikaci, například hello uživatelského rozhraní. ADAL také mezipamětí hello token pro použití v dalších částí aplikace hello.

### <a name="check-for-tokens-on-app-start"></a>Zkontrolujte pro tokeny při spuštění aplikace
Můžete taky sledovat tookeep MSAL stavu přihlášení uživatele hello.  V této aplikaci chceme tooremain uživatele hello přihlášení i po jejich zavření aplikace hello & ho znovu otevřete.  Zpět v hello `OnInitialized` přepsat, použijte na MSAL `AcquireTokenSilent` toocheck metoda pro tokeny, uložené v mezipaměti:

```C#
AuthenticationResult result = null;
try
{
    // If hello user has has a token cached with any policy, we'll display them as signed-in.
    TokenCacheItem tci = pca.UserTokenCache.ReadItems(Globals.clientId).Where(i => i.Scope.Contains(Globals.clientId) && !string.IsNullOrEmpty(i.Policy)).FirstOrDefault();
    string existingPolicy = tci == null ? null : tci.Policy;
    result = await pca.AcquireTokenSilentAsync(new string[] { Globals.clientId }, string.Empty, Globals.authority, existingPolicy, false);

    SignInButton.Visibility = Visibility.Collapsed;
    SignUpButton.Visibility = Visibility.Collapsed;
    EditProfileButton.Visibility = Visibility.Visible;
    SignOutButton.Visibility = Visibility.Visible;
    UsernameLabel.Content = result.User.Name;
    GetTodoList();
}
catch (MsalException ex)
{
    if (ex.ErrorCode == "failed_to_acquire_token_silently")
    {
        // There are no tokens in hello cache.  Proceed without calling hello tooDo list service.
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
```

## <a name="call-hello-task-api"></a>Volání rozhraní API hello úloh
Nyní použili MSAL tooexecute zásady a získat tokeny.  Když chcete toouse jeden tyto tokeny toocall hello úloh API, můžete znovu použít na MSAL `AcquireTokenSilent` toocheck metoda pro tokeny, uložené v mezipaměti:

```C#
private async void GetTodoList()
{
    AuthenticationResult result = null;
    try
    {
        // Here we want toocheck for a cached token, independent of whatever policy was used tooacquire it.
        TokenCacheItem tci = pca.UserTokenCache.ReadItems(Globals.clientId).Where(i => i.Scope.Contains(Globals.clientId) && !string.IsNullOrEmpty(i.Policy)).FirstOrDefault();
        string existingPolicy = tci == null ? null : tci.Policy;

        // Use AcquireTokenSilent tooindicate that MSAL should throw an exception if a token cannot be acquired
        result = await pca.AcquireTokenSilentAsync(new string[] { Globals.clientId }, string.Empty, Globals.authority, existingPolicy, false);

    }
    // If a token could not be acquired silently, we'll catch hello exception and show hello user a message.
    catch (MsalException ex)
    {
        // There is no access token in hello cache, so prompt hello user toosign-in.
        if (ex.ErrorCode == "failed_to_acquire_token_silently")
        {
            MessageBox.Show("Please sign up or sign in first");
            SignInButton.Visibility = Visibility.Visible;
            SignUpButton.Visibility = Visibility.Visible;
            EditProfileButton.Visibility = Visibility.Collapsed;
            SignOutButton.Visibility = Visibility.Collapsed;
            UsernameLabel.Content = string.Empty;
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
    ...
```

Při volání příliš hello`AcquireTokenSilentAsync(...)` úspěšné a nebude nalezen token hello mezipaměti, můžete přidat hello token toohello `Authorization` hlavičky požadavku hello HTTP. Hello úloh webového rozhraní API bude používat toto záhlaví tooauthenticate hello požadavek tooread hello seznam úkolů uživatele:

```C#
    ...
    // Once hello token has been returned by MSAL, add it toohello http authorization header, before making hello call tooaccess hello tooDo list service.
    httpClient.DefaultRequestHeaders.Authorization = new AuthenticationHeaderValue("Bearer", result.Token);

    // Call hello tooDo list service.
    HttpResponseMessage response = await httpClient.GetAsync(Globals.taskServiceUrl + "/api/tasks");
    ...
```

## <a name="sign-hello-user-out"></a>Přihlášení uživatele hello out
Nakonec můžete použít MSAL tooend relace uživatele s aplikací hello při hello uživatel vybere **Odhlásit se**.  Při použití MSAL, toho dosahuje tím, že zrušíte všechny tokeny hello z tokenu mezipaměti hello:

```C#
private void SignOut(object sender, RoutedEventArgs e)
{
    // Clear any remnants of hello user's session.
    pca.UserTokenCache.Clear(Globals.clientId);

    // This is a helper method that clears browser cookies in hello browser control that MSAL uses, it is not part of MSAL.
    ClearCookies();

    // Update hello UI tooshow hello user as signed out.
    TaskList.ItemsSource = string.Empty;
    SignInButton.Visibility = Visibility.Visible;
    SignUpButton.Visibility = Visibility.Visible;
    EditProfileButton.Visibility = Visibility.Collapsed;
    SignOutButton.Visibility = Visibility.Collapsed;
    return;
}
```

## <a name="run-hello-sample-app"></a>Spuštění ukázkové aplikace hello
Nakonec sestavte a spusťte ukázkové hello.  Zaregistrujte se pro aplikaci hello pomocí e-mailové adresy nebo uživatelské jméno. Odhlaste se a přihlaste se znovu jako hello stejné uživatele. Upravte profil uživatele. Odhlásit se a přihlásit pomocí jiného uživatele.

## <a name="add-social-idps"></a>Přidat sociálních IDPs
V současné době hello aplikace podporuje pouze uživatele registrace a přihlášení, použít **místní účty**. Toto jsou účty uložené v adresáři B2C, které používají uživatelské jméno a heslo. Pomocí Azure AD B2C můžete přidat podporu pro jiných poskytovatelů identit (IDPs) beze změny některé z vašeho kódu.

tooadd sociálních IDPs tooyour aplikace, začněte tím, že následující hello podrobné pokyny v těchto článcích. Pro každé rozšíření IDP chcete toosupport, potřebujete tooregister aplikace v daném systému a získat ID klienta.

* [Nastavení sítě Facebook jako IDP](active-directory-b2c-setup-fb-app.md)
* [Nastavit Google jako IDP](active-directory-b2c-setup-goog-app.md)
* [Nastavit Amazon jako IDP](active-directory-b2c-setup-amzn-app.md)
* [Nastavit LinkedIn jako IDP](active-directory-b2c-setup-li-app.md)

Po přidání directory tooyour B2C zprostředkovatelé identity hello potřebujete tooedit každé tři zásady tooinclude hello nové IDPs jako popsané v hello [článku o zásadách](active-directory-b2c-reference-policies.md). Po uložení zásad hello a znovu spusťte aplikaci. Měli byste vidět hello přidají nové IDPs jako přihlášení a odhlášení možnosti v každé z vaší činnosti identity.

Můžete experimentovat s vašimi zásadami a sledovat účinky hello na ukázkové aplikace. Přidat nebo odebrat IDPs, pracovat s deklarace identity aplikace nebo změnit atributy registrace. Experiment, dokud se nezobrazí, jak zásady, žádosti o ověření a MSAL tie společně.

Pro referenci hello dokončit ukázka [je k dispozici jako soubor ZIP](https://github.com/AzureADQuickStarts/B2C-NativeClient-DotNet/archive/complete.zip). Můžete ho také klonovat z GitHubu:

```git clone --branch complete https://github.com/AzureADQuickStarts/B2C-NativeClient-DotNet.git```
