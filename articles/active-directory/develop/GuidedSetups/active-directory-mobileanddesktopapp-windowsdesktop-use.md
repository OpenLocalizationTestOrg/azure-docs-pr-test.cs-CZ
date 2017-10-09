---
title: "aaaAzure AD v2 Windows Desktop Začínáme - použití | Microsoft Docs"
description: "Jak aplikace Windows Desktop .NET (XAML) můžete volat rozhraní API, které vyžadují přístupové tokeny bodem v2 Azure Active Directory"
services: active-directory
documentationcenter: dev-center-name
author: andretms
manager: mbaldwin
editor: 
ms.assetid: 820acdb7-d316-4c3b-8de9-79df48ba3b06
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 05/09/2017
ms.author: andret
ms.custom: aaddev
ms.openlocfilehash: bb258fe5f523ec727ca02716fd823d853d3349b8
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
## <a name="use-hello-microsoft-authentication-library-msal-tooget-a-token-for-hello-microsoft-graph-api"></a>Použít Microsoft ověřování knihovny (MSAL) tooget hello token pro hello Microsoft Graph API

Tato část uvádí, jak toouse MSAL tooget token hello Microsoft Graph API.

1.  V `MainWindow.xaml.cs`, přidat hello odkaz pro třídu toohello MSAL knihovny:

```csharp
using Microsoft.Identity.Client;
```
<!-- Workaround for Docs conversion bug -->
<ol start="2">
<li>
Nahraďte <code>MainWindow</code> kódu s použitím třídy:
</li>
</ol>

```csharp
public partial class MainWindow : Window
{
    //Set hello API Endpoint tooGraph 'me' endpoint
    string _graphAPIEndpoint = "https://graph.microsoft.com/v1.0/me";

    //Set hello scope for API call toouser.read
    string[] _scopes = new string[] { "user.read" };


    public MainWindow()
    {
        InitializeComponent();
    }

    /// <summary>
    /// Call AcquireTokenAsync - tooacquire a token requiring user toosign-in
    /// </summary>
    private async void CallGraphButton_Click(object sender, RoutedEventArgs e)
    {
        AuthenticationResult authResult = null;

        try
        {
            authResult = await App.PublicClientApp.AcquireTokenSilentAsync(_scopes, App.PublicClientApp.Users.FirstOrDefault());
        }
        catch (MsalUiRequiredException ex)
        {
            // A MsalUiRequiredException happened on AcquireTokenSilentAsync. This indicates you need toocall AcquireTokenAsync tooacquire a token
            System.Diagnostics.Debug.WriteLine($"MsalUiRequiredException: {ex.Message}");

            try
            {
                authResult = await App.PublicClientApp.AcquireTokenAsync(_scopes);
            }
            catch (MsalException msalex)
            {
                ResultText.Text = $"Error Acquiring Token:{System.Environment.NewLine}{msalex}";
            }
        }
        catch (Exception ex)
        {
            ResultText.Text = $"Error Acquiring Token Silently:{System.Environment.NewLine}{ex}";
            return;
        }

        if (authResult != null)
        {
            ResultText.Text = await GetHttpContentWithToken(_graphAPIEndpoint, authResult.AccessToken);
            DisplayBasicTokenInfo(authResult);
            this.SignOutButton.Visibility = Visibility.Visible;
        }
    }
}
```

<!--start-collapse-->
### <a name="more-information"></a>Další informace
#### <a name="getting-a-user-token-interactive"></a>Získávání interaktivní token uživatele
Volání hello `AcquireTokenAsync` metoda výsledky v okně výzvy hello toosign uživatele v. Aplikace obvykle vyžadují toosign uživatele v interaktivním hello poprvé potřebují tooaccess chráněného prostředku nebo při bezobslužné operace tooacquire token selže (například hello uživatele platnost hesla).

#### <a name="getting-a-user-token-silently"></a>Získání tokenu uživatele bez upozornění
`AcquireTokenSilentAsync`zpracovává tokenu pořízení a obnovení bez nutnosti zásahu uživatele. Po `AcquireTokenAsync` je spouštěna hello poprvé, `AcquireTokenSilentAsync` je hello obvykle metodu použitou tooobtain použití tokenů tooaccess chráněné zdroje pro následující volání - jako toorequest volání nebo tokeny obnovení jsou vytvářeny bezobslužně.
Nakonec `AcquireTokenSilentAsync` selže – např. uživatel hello má odhlášení, nebo došlo ke změně hesla na jiném zařízení. Když MSAL zjistí, že hello problém lze vyřešit tím, že vyžaduje interaktivní akce, aktivuje se ho `MsalUiRequiredException`. Aplikace může zpracovat výjimku dvěma způsoby:

1.  Volání proti `AcquireTokenAsync` okamžitě, výsledkem výzvy uživatele hello toosign v. Tento vzor se obvykle používá v online aplikace tam, kde není žádná offline obsah v aplikaci hello k dispozici pro uživatele hello. Hello ukázka generované touto s průvodcem instalací používá tento vzor: Zobrazí se v akce hello poprvé spustit ukázkový hello: protože žádný uživatel nikdy používá aplikace hello `PublicClientApp.Users.FirstOrDefault()` bude obsahovat hodnotu null a `MsalUiRequiredException` dojde k výjimce. Hello kód v ukázce hello pak obslužných rutin výjimek hello voláním `AcquireTokenAsync` výsledkem výzvy uživatele hello toosign v.

2.  Aplikace lze také nastavit vizuální označení toohello uživatele, který interaktivní přihlášení je povinné, takže hello uživatele můžete vybrat hello správný čas toosign v, nebo můžete zkusit aplikace hello `AcquireTokenSilentAsync` později. To se obvykle používá při hello uživatele může používat další funkce aplikace hello bez narušení – například je offline obsah k dispozici v aplikaci hello. V takovém případě se můžete rozhodnout hello uživatele, když chtějí toosign v tooaccess hello chráněný prostředek, nebo toorefresh hello zastaralé informace nebo aplikace se můžete rozhodnout tooretry `AcquireTokenSilentAsync` obnovení po síti poté, co byla dočasně nedostupná.
<!--end-collapse-->

## <a name="call-hello-microsoft-graph-api-using-hello-token-you-just-obtained"></a>Volání rozhraní API Microsoft Graph hello pomocí hello tokenu, který jste obdrželi

1. Přidat nové metody hello níže tooyour `MainWindow.xaml.cs`. Hello metoda je použité toomake `GET` požadavek na rozhraní Graph API pomocí hlavičku autorizovat:

```csharp
/// <summary>
/// Perform an HTTP GET request tooa URL using an HTTP Authorization header
/// </summary>
/// <param name="url">hello URL</param>
/// <param name="token">hello token</param>
/// <returns>String containing hello results of hello GET operation</returns>
public async Task<string> GetHttpContentWithToken(string url, string token)
{
    var httpClient = new System.Net.Http.HttpClient();
    System.Net.Http.HttpResponseMessage response;
    try
    {
        var request = new System.Net.Http.HttpRequestMessage(System.Net.Http.HttpMethod.Get, url);
        //Add hello token in Authorization header
        request.Headers.Authorization = new System.Net.Http.Headers.AuthenticationHeaderValue("Bearer", token);
        response = await httpClient.SendAsync(request);
        var content = await response.Content.ReadAsStringAsync();
        return content;
    }
    catch (Exception ex)
    {
        return ex.ToString();
    }
}
```
<!--start-collapse-->
### <a name="more-information-on-making-a-rest-call-against-a-protected-api"></a>Další informace o volání REST chráněné rozhraní API

V této ukázkové aplikaci hello `GetHttpContentWithToken` metoda je použité toomake HTTP `GET` požadavku vůči chráněný prostředek, který vyžaduje volající obsahu toohello tokenu a potom se vraťte hello. Tato metoda přidá hello získat token v hello *HTTP autorizační hlavičky*. Tato ukázka je hello prostředků hello Microsoft Graph API *mi* koncového bodu – zobrazí informace o profilu uživatele hello.
<!--end-collapse-->

## <a name="add-a-method-toosign-out-hello-user"></a>Přidat metoda toosign out hello uživatele

1. Přidejte následující metodu tooyour hello `MainWindow.xaml.cs` toosign out hello uživatele:

```csharp
/// <summary>
/// Sign out hello current user
/// </summary>
private void SignOutButton_Click(object sender, RoutedEventArgs e)
{
    if (App.PublicClientApp.Users.Any())
    {
        try
        {
            App.PublicClientApp.Remove(App.PublicClientApp.Users.FirstOrDefault());
            this.ResultText.Text = "User has signed-out";
            this.CallGraphButton.Visibility = Visibility.Visible;
            this.SignOutButton.Visibility = Visibility.Collapsed;
        }
        catch (MsalException ex)
        {
            ResultText.Text = $"Error signing-out user: {ex.Message}";
        }
    }
}
```
<!--start-collapse-->
### <a name="more-info-on-sign-out"></a>Další informace o odhlášení

`SignOutButton_Click`Odebere hello uživatele z mezipaměti uživatele MSAL – se efektivně tak dozví, MSAL tooforget hello aktuální uživatel tak tooacquire budoucí žádosti o token bude úspěšné, pouze pokud je k toobe interaktivní.
I když aplikace hello v této ukázce podporuje jenom jednoho konkrétního uživatele, MSAL podporuje scénáře, kde může být více účtů přihlášeného v hello současně – příklad je e-mailové aplikace kde uživatel, který má více účtů.
<!--end-collapse-->

## <a name="display-basic-token-information"></a>Zobrazit základní informace o tokenu

1. Přidejte následující metodu tootooyour hello `MainWindow.xaml.cs` toodisplay základní informace o tokenu hello:

```csharp
/// <summary>
/// Display basic information contained in hello token
/// </summary>
private void DisplayBasicTokenInfo(AuthenticationResult authResult)
{
    TokenInfoText.Text = "";
    if (authResult != null)
    {
        TokenInfoText.Text += $"Name: {authResult.User.Name}" + Environment.NewLine;
        TokenInfoText.Text += $"Username: {authResult.User.DisplayableId}" + Environment.NewLine;
        TokenInfoText.Text += $"Token Expires: {authResult.ExpiresOn.ToLocalTime()}" + Environment.NewLine;
        TokenInfoText.Text += $"Access Token: {authResult.AccessToken}" + Environment.NewLine;
    }
}
```
<!--start-collapse-->
### <a name="more-information"></a>Další informace

Tokeny získaných prostřednictvím *OpenID Connect* také obsahovat malou podmnožinu informací o příslušné toohello uživateli. `DisplayBasicTokenInfo`Zobrazí základních informací obsažených v tokenu hello: například hello uživatele zobrazí název a ID, jakož i hello řetězce data a hello vypršení platnosti tokenu představující hello přístupový token, sám sebe. Tyto informace se zobrazí pro toosee je. Kliknutí hello *volání Microsoft Graph API* tlačítko vícekrát a zjistit, že hello stejný token byl znovu použít pro následné požadavky. Můžete také zjistit datum vypršení platnosti hello rozšiřovanou při MSAL rozhodne, že je čas toorenew hello token.
<!--end-collapse-->

