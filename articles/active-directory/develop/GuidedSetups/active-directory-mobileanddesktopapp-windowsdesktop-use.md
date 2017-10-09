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
## <a name="use-hello-microsoft-authentication-library-msal-tooget-a-token-for-hello-microsoft-graph-api"></a><span data-ttu-id="f407d-103">Použít Microsoft ověřování knihovny (MSAL) tooget hello token pro hello Microsoft Graph API</span><span class="sxs-lookup"><span data-stu-id="f407d-103">Use hello Microsoft Authentication Library (MSAL) tooget a token for hello Microsoft Graph API</span></span>

<span data-ttu-id="f407d-104">Tato část uvádí, jak toouse MSAL tooget token hello Microsoft Graph API.</span><span class="sxs-lookup"><span data-stu-id="f407d-104">This section shows how toouse MSAL tooget a token hello Microsoft Graph API.</span></span>

1.  <span data-ttu-id="f407d-105">V `MainWindow.xaml.cs`, přidat hello odkaz pro třídu toohello MSAL knihovny:</span><span class="sxs-lookup"><span data-stu-id="f407d-105">In `MainWindow.xaml.cs`, add hello reference for MSAL library toohello class:</span></span>

```csharp
using Microsoft.Identity.Client;
```
<!-- Workaround for Docs conversion bug -->
<ol start="2">
<li>
<span data-ttu-id="f407d-106">Nahraďte <code>MainWindow</code> kódu s použitím třídy:</span><span class="sxs-lookup"><span data-stu-id="f407d-106">Replace <code>MainWindow</code> class code with:</span></span>
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
### <a name="more-information"></a><span data-ttu-id="f407d-107">Další informace</span><span class="sxs-lookup"><span data-stu-id="f407d-107">More Information</span></span>
#### <a name="getting-a-user-token-interactive"></a><span data-ttu-id="f407d-108">Získávání interaktivní token uživatele</span><span class="sxs-lookup"><span data-stu-id="f407d-108">Getting a user token interactive</span></span>
<span data-ttu-id="f407d-109">Volání hello `AcquireTokenAsync` metoda výsledky v okně výzvy hello toosign uživatele v.</span><span class="sxs-lookup"><span data-stu-id="f407d-109">Calling hello `AcquireTokenAsync` method results in a window prompting hello user toosign in.</span></span> <span data-ttu-id="f407d-110">Aplikace obvykle vyžadují toosign uživatele v interaktivním hello poprvé potřebují tooaccess chráněného prostředku nebo při bezobslužné operace tooacquire token selže (například hello uživatele platnost hesla).</span><span class="sxs-lookup"><span data-stu-id="f407d-110">Applications usually require a user toosign in interactively hello first time they need tooaccess a protected resource, or when a silent operation tooacquire a token fails (e.g. hello user’s password expired).</span></span>

#### <a name="getting-a-user-token-silently"></a><span data-ttu-id="f407d-111">Získání tokenu uživatele bez upozornění</span><span class="sxs-lookup"><span data-stu-id="f407d-111">Getting a user token silently</span></span>
<span data-ttu-id="f407d-112">`AcquireTokenSilentAsync`zpracovává tokenu pořízení a obnovení bez nutnosti zásahu uživatele.</span><span class="sxs-lookup"><span data-stu-id="f407d-112">`AcquireTokenSilentAsync` handles token acquisitions and renewal without any user interaction.</span></span> <span data-ttu-id="f407d-113">Po `AcquireTokenAsync` je spouštěna hello poprvé, `AcquireTokenSilentAsync` je hello obvykle metodu použitou tooobtain použití tokenů tooaccess chráněné zdroje pro následující volání - jako toorequest volání nebo tokeny obnovení jsou vytvářeny bezobslužně.</span><span class="sxs-lookup"><span data-stu-id="f407d-113">After `AcquireTokenAsync` is executed for hello first time, `AcquireTokenSilentAsync` is hello usual method used tooobtain tokens used tooaccess protected resources for subsequent calls - as calls toorequest or renew tokens are made silently.</span></span>
<span data-ttu-id="f407d-114">Nakonec `AcquireTokenSilentAsync` selže – např. uživatel hello má odhlášení, nebo došlo ke změně hesla na jiném zařízení.</span><span class="sxs-lookup"><span data-stu-id="f407d-114">Eventually, `AcquireTokenSilentAsync` will fail – e.g. hello user has signed out, or has changed their password on another device.</span></span> <span data-ttu-id="f407d-115">Když MSAL zjistí, že hello problém lze vyřešit tím, že vyžaduje interaktivní akce, aktivuje se ho `MsalUiRequiredException`.</span><span class="sxs-lookup"><span data-stu-id="f407d-115">When MSAL detects that hello issue can be resolved by requiring an interactive action, it fires an `MsalUiRequiredException`.</span></span> <span data-ttu-id="f407d-116">Aplikace může zpracovat výjimku dvěma způsoby:</span><span class="sxs-lookup"><span data-stu-id="f407d-116">Your application can handle this exception in two ways:</span></span>

1.  <span data-ttu-id="f407d-117">Volání proti `AcquireTokenAsync` okamžitě, výsledkem výzvy uživatele hello toosign v.</span><span class="sxs-lookup"><span data-stu-id="f407d-117">Make a call against `AcquireTokenAsync` immediately, which results in prompting hello user toosign-in.</span></span> <span data-ttu-id="f407d-118">Tento vzor se obvykle používá v online aplikace tam, kde není žádná offline obsah v aplikaci hello k dispozici pro uživatele hello.</span><span class="sxs-lookup"><span data-stu-id="f407d-118">This pattern is usually used in online applications where there is no offline content in hello application available for hello user.</span></span> <span data-ttu-id="f407d-119">Hello ukázka generované touto s průvodcem instalací používá tento vzor: Zobrazí se v akce hello poprvé spustit ukázkový hello: protože žádný uživatel nikdy používá aplikace hello `PublicClientApp.Users.FirstOrDefault()` bude obsahovat hodnotu null a `MsalUiRequiredException` dojde k výjimce.</span><span class="sxs-lookup"><span data-stu-id="f407d-119">hello sample generated by this guided setup uses this pattern: you can see it in action hello first time you execute hello sample: because no user ever used hello application, `PublicClientApp.Users.FirstOrDefault()` will contain a null value, and an `MsalUiRequiredException` exception will be thrown.</span></span> <span data-ttu-id="f407d-120">Hello kód v ukázce hello pak obslužných rutin výjimek hello voláním `AcquireTokenAsync` výsledkem výzvy uživatele hello toosign v.</span><span class="sxs-lookup"><span data-stu-id="f407d-120">hello code in hello sample then handles hello exception by calling `AcquireTokenAsync` resulting in prompting hello user toosign-in.</span></span>

2.  <span data-ttu-id="f407d-121">Aplikace lze také nastavit vizuální označení toohello uživatele, který interaktivní přihlášení je povinné, takže hello uživatele můžete vybrat hello správný čas toosign v, nebo můžete zkusit aplikace hello `AcquireTokenSilentAsync` později.</span><span class="sxs-lookup"><span data-stu-id="f407d-121">Applications can also make a visual indication toohello user that an interactive sign-in is required, so hello user can select hello right time toosign in, or hello application can retry `AcquireTokenSilentAsync` at a later time.</span></span> <span data-ttu-id="f407d-122">To se obvykle používá při hello uživatele může používat další funkce aplikace hello bez narušení – například je offline obsah k dispozici v aplikaci hello.</span><span class="sxs-lookup"><span data-stu-id="f407d-122">This is usually used when hello user can use other functionality of hello application without being disrupted - for example, there is offline content available in hello application.</span></span> <span data-ttu-id="f407d-123">V takovém případě se můžete rozhodnout hello uživatele, když chtějí toosign v tooaccess hello chráněný prostředek, nebo toorefresh hello zastaralé informace nebo aplikace se můžete rozhodnout tooretry `AcquireTokenSilentAsync` obnovení po síti poté, co byla dočasně nedostupná.</span><span class="sxs-lookup"><span data-stu-id="f407d-123">In this case, hello user can decide when they want toosign in tooaccess hello protected resource, or toorefresh hello outdated information, or your application can decide tooretry `AcquireTokenSilentAsync` when network is restored after being unavailable temporarily.</span></span>
<!--end-collapse-->

## <a name="call-hello-microsoft-graph-api-using-hello-token-you-just-obtained"></a><span data-ttu-id="f407d-124">Volání rozhraní API Microsoft Graph hello pomocí hello tokenu, který jste obdrželi</span><span class="sxs-lookup"><span data-stu-id="f407d-124">Call hello Microsoft Graph API using hello token you just obtained</span></span>

1. <span data-ttu-id="f407d-125">Přidat nové metody hello níže tooyour `MainWindow.xaml.cs`.</span><span class="sxs-lookup"><span data-stu-id="f407d-125">Add hello new method below tooyour `MainWindow.xaml.cs`.</span></span> <span data-ttu-id="f407d-126">Hello metoda je použité toomake `GET` požadavek na rozhraní Graph API pomocí hlavičku autorizovat:</span><span class="sxs-lookup"><span data-stu-id="f407d-126">hello method is used toomake a `GET` request against Graph API using an Authorize header:</span></span>

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
### <a name="more-information-on-making-a-rest-call-against-a-protected-api"></a><span data-ttu-id="f407d-127">Další informace o volání REST chráněné rozhraní API</span><span class="sxs-lookup"><span data-stu-id="f407d-127">More information on making a REST call against a protected API</span></span>

<span data-ttu-id="f407d-128">V této ukázkové aplikaci hello `GetHttpContentWithToken` metoda je použité toomake HTTP `GET` požadavku vůči chráněný prostředek, který vyžaduje volající obsahu toohello tokenu a potom se vraťte hello.</span><span class="sxs-lookup"><span data-stu-id="f407d-128">In this sample application, hello `GetHttpContentWithToken` method is used toomake an HTTP `GET` request against a protected resource that requires a token and then return hello content toohello caller.</span></span> <span data-ttu-id="f407d-129">Tato metoda přidá hello získat token v hello *HTTP autorizační hlavičky*.</span><span class="sxs-lookup"><span data-stu-id="f407d-129">This method adds hello acquired token in hello *HTTP Authorization header*.</span></span> <span data-ttu-id="f407d-130">Tato ukázka je hello prostředků hello Microsoft Graph API *mi* koncového bodu – zobrazí informace o profilu uživatele hello.</span><span class="sxs-lookup"><span data-stu-id="f407d-130">For this sample, hello resource is hello Microsoft Graph API *me* endpoint – which displays hello user's profile information.</span></span>
<!--end-collapse-->

## <a name="add-a-method-toosign-out-hello-user"></a><span data-ttu-id="f407d-131">Přidat metoda toosign out hello uživatele</span><span class="sxs-lookup"><span data-stu-id="f407d-131">Add a method toosign out hello user</span></span>

1. <span data-ttu-id="f407d-132">Přidejte následující metodu tooyour hello `MainWindow.xaml.cs` toosign out hello uživatele:</span><span class="sxs-lookup"><span data-stu-id="f407d-132">Add hello following method tooyour `MainWindow.xaml.cs` toosign out hello user:</span></span>

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
### <a name="more-info-on-sign-out"></a><span data-ttu-id="f407d-133">Další informace o odhlášení</span><span class="sxs-lookup"><span data-stu-id="f407d-133">More info on Sign-Out</span></span>

<span data-ttu-id="f407d-134">`SignOutButton_Click`Odebere hello uživatele z mezipaměti uživatele MSAL – se efektivně tak dozví, MSAL tooforget hello aktuální uživatel tak tooacquire budoucí žádosti o token bude úspěšné, pouze pokud je k toobe interaktivní.</span><span class="sxs-lookup"><span data-stu-id="f407d-134">`SignOutButton_Click` removes hello user from MSAL user cache – this will effectively tell MSAL tooforget hello current user so a future request tooacquire a token will only succeed if it is made toobe interactive.</span></span>
<span data-ttu-id="f407d-135">I když aplikace hello v této ukázce podporuje jenom jednoho konkrétního uživatele, MSAL podporuje scénáře, kde může být více účtů přihlášeného v hello současně – příklad je e-mailové aplikace kde uživatel, který má více účtů.</span><span class="sxs-lookup"><span data-stu-id="f407d-135">Although hello application in this sample supports a single user, MSAL supports scenarios where multiple accounts can be signed-in at hello same time – an example is an email application where a user has multiple accounts.</span></span>
<!--end-collapse-->

## <a name="display-basic-token-information"></a><span data-ttu-id="f407d-136">Zobrazit základní informace o tokenu</span><span class="sxs-lookup"><span data-stu-id="f407d-136">Display Basic Token Information</span></span>

1. <span data-ttu-id="f407d-137">Přidejte následující metodu tootooyour hello `MainWindow.xaml.cs` toodisplay základní informace o tokenu hello:</span><span class="sxs-lookup"><span data-stu-id="f407d-137">Add hello following method tootooyour `MainWindow.xaml.cs` toodisplay basic information about hello token:</span></span>

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
### <a name="more-information"></a><span data-ttu-id="f407d-138">Další informace</span><span class="sxs-lookup"><span data-stu-id="f407d-138">More Information</span></span>

<span data-ttu-id="f407d-139">Tokeny získaných prostřednictvím *OpenID Connect* také obsahovat malou podmnožinu informací o příslušné toohello uživateli.</span><span class="sxs-lookup"><span data-stu-id="f407d-139">Tokens acquired via *OpenID Connect* also contain a small subset of information pertinent toohello user.</span></span> <span data-ttu-id="f407d-140">`DisplayBasicTokenInfo`Zobrazí základních informací obsažených v tokenu hello: například hello uživatele zobrazí název a ID, jakož i hello řetězce data a hello vypršení platnosti tokenu představující hello přístupový token, sám sebe.</span><span class="sxs-lookup"><span data-stu-id="f407d-140">`DisplayBasicTokenInfo` displays basic information contained in hello token: for example, hello user's display name and ID, as well as hello token expiration date and hello string representing hello access token itself.</span></span> <span data-ttu-id="f407d-141">Tyto informace se zobrazí pro toosee je.</span><span class="sxs-lookup"><span data-stu-id="f407d-141">This information is displayed for you toosee.</span></span> <span data-ttu-id="f407d-142">Kliknutí hello *volání Microsoft Graph API* tlačítko vícekrát a zjistit, že hello stejný token byl znovu použít pro následné požadavky.</span><span class="sxs-lookup"><span data-stu-id="f407d-142">You can hit hello *Call Microsoft Graph API* button multiple times and see that hello same token was reused for subsequent requests.</span></span> <span data-ttu-id="f407d-143">Můžete také zjistit datum vypršení platnosti hello rozšiřovanou při MSAL rozhodne, že je čas toorenew hello token.</span><span class="sxs-lookup"><span data-stu-id="f407d-143">You can also see hello expiration date being extended when MSAL decides it is time toorenew hello token.</span></span>
<!--end-collapse-->

