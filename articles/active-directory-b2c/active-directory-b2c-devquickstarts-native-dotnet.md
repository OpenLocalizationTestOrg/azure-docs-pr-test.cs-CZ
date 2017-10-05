---
title: Azure Active Directory B2C | Microsoft Docs
description: "Jak sestavit aplikaci plochy Windows, která zahrnuje přihlášení, registrace, a správy profilů pomocí Azure Active Directory B2C."
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
ms.openlocfilehash: 8e2b5c704230ee2ba1395dc76a1551aaa8e7af7f
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="azure-ad-b2c-build-a-windows-desktop-app"></a><span data-ttu-id="72f88-103">Azure AD B2C: Sestavení aplikace na ploše systému Windows</span><span class="sxs-lookup"><span data-stu-id="72f88-103">Azure AD B2C: Build a Windows desktop app</span></span>
<span data-ttu-id="72f88-104">Pomocí Azure Active Directory (Azure AD) B2C můžete přidat výkonné identity samoobslužné funkce pro správu k vaší aplikace na ploše v několika krocích.</span><span class="sxs-lookup"><span data-stu-id="72f88-104">By using Azure Active Directory (Azure AD) B2C, you can add powerful self-service identity management features to your desktop app in a few short steps.</span></span> <span data-ttu-id="72f88-105">Tento článek vám ukáže, jak vytvořit aplikaci "seznam úkolů".NET Windows Presentation Foundation (WPF), která zahrnuje uživatelskou registraci, přihlašování a správu profilu.</span><span class="sxs-lookup"><span data-stu-id="72f88-105">This article will show you how to create a .NET Windows Presentation Foundation (WPF) "to-do list" app that includes user sign-up, sign-in, and profile management.</span></span> <span data-ttu-id="72f88-106">Aplikace bude zahrnují podporu pro registraci a přihlaste se pomocí uživatelského jména nebo e-mailu.</span><span class="sxs-lookup"><span data-stu-id="72f88-106">The app will include support for sign-up and sign-in by using a user name or email.</span></span> <span data-ttu-id="72f88-107">Podporu registrace a přihlašování bude zahrnovat také pomocí sociálních účty například Facebook nebo Google.</span><span class="sxs-lookup"><span data-stu-id="72f88-107">It will also include support for sign-up and sign-in by using social accounts such as Facebook and Google.</span></span>

## <a name="get-an-azure-ad-b2c-directory"></a><span data-ttu-id="72f88-108">Získání adresáře služby Azure AD B2C</span><span class="sxs-lookup"><span data-stu-id="72f88-108">Get an Azure AD B2C directory</span></span>
<span data-ttu-id="72f88-109">Před použitím Azure AD B2C musíte vytvořit adresář, nebo klienta.</span><span class="sxs-lookup"><span data-stu-id="72f88-109">Before you can use Azure AD B2C, you must create a directory, or tenant.</span></span>  <span data-ttu-id="72f88-110">Adresář je kontejner pro všechny vaše uživatele, aplikace, skupiny a další.</span><span class="sxs-lookup"><span data-stu-id="72f88-110">A directory is a container for all of your users, apps, groups, and more.</span></span> <span data-ttu-id="72f88-111">Pokud ho ještě nemáte, [vytvořte adresář B2C](active-directory-b2c-get-started.md) předtím, než budete pokračovat.</span><span class="sxs-lookup"><span data-stu-id="72f88-111">If you don't have one already, [create a B2C directory](active-directory-b2c-get-started.md) before you continue in this guide.</span></span>

## <a name="create-an-application"></a><span data-ttu-id="72f88-112">Vytvoření aplikace</span><span class="sxs-lookup"><span data-stu-id="72f88-112">Create an application</span></span>
<span data-ttu-id="72f88-113">Dále musíte vytvořit aplikaci v adresáři B2C.</span><span class="sxs-lookup"><span data-stu-id="72f88-113">Next, you need to create an app in your B2C directory.</span></span> <span data-ttu-id="72f88-114">Azure AD díky tomu získá informace potřebné k bezpečné komunikaci s vaší aplikací.</span><span class="sxs-lookup"><span data-stu-id="72f88-114">This gives Azure AD information that it needs to securely communicate with your app.</span></span> <span data-ttu-id="72f88-115">Chcete-li vytvořit aplikaci, postupujte podle [těchto pokynů](active-directory-b2c-app-registration.md).</span><span class="sxs-lookup"><span data-stu-id="72f88-115">To create an app, follow [these instructions](active-directory-b2c-app-registration.md).</span></span>  <span data-ttu-id="72f88-116">Ujistěte se, že:</span><span class="sxs-lookup"><span data-stu-id="72f88-116">Be sure to:</span></span>

* <span data-ttu-id="72f88-117">Zahrnout **nativního klienta** v aplikaci.</span><span class="sxs-lookup"><span data-stu-id="72f88-117">Include a **native client** in the application.</span></span>
* <span data-ttu-id="72f88-118">Kopírování **identifikátor URI pro přesměrování** `urn:ietf:wg:oauth:2.0:oob`.</span><span class="sxs-lookup"><span data-stu-id="72f88-118">Copy the **Redirect URI** `urn:ietf:wg:oauth:2.0:oob`.</span></span> <span data-ttu-id="72f88-119">To je výchozí URL pro tento příklad.</span><span class="sxs-lookup"><span data-stu-id="72f88-119">It is the default URL for this code sample.</span></span>
* <span data-ttu-id="72f88-120">Poznamenejte si **ID aplikace** přiřazené vaší aplikaci.</span><span class="sxs-lookup"><span data-stu-id="72f88-120">Copy the **Application ID** that is assigned to your app.</span></span> <span data-ttu-id="72f88-121">Budete ho potřebovat později.</span><span class="sxs-lookup"><span data-stu-id="72f88-121">You will need it later.</span></span>

[!INCLUDE [active-directory-b2c-devquickstarts-v2-apps](../../includes/active-directory-b2c-devquickstarts-v2-apps.md)]

## <a name="create-your-policies"></a><span data-ttu-id="72f88-122">Vytvořte svoje zásady</span><span class="sxs-lookup"><span data-stu-id="72f88-122">Create your policies</span></span>
<span data-ttu-id="72f88-123">V Azure AD B2C je každé uživatelské rozhraní definováno [zásadou](active-directory-b2c-reference-policies.md).</span><span class="sxs-lookup"><span data-stu-id="72f88-123">In Azure AD B2C, every user experience is defined by a [policy](active-directory-b2c-reference-policies.md).</span></span> <span data-ttu-id="72f88-124">Tato ukázka kódu obsahuje tři činnosti identity: registrace, přihlášení a úprava profilu.</span><span class="sxs-lookup"><span data-stu-id="72f88-124">This code sample contains three identity experiences: sign up, sign in, and edit profile.</span></span> <span data-ttu-id="72f88-125">Je třeba vytvořit zásadu pro každý typ, jak je popsáno v [článku o zásadách](active-directory-b2c-reference-policies.md#create-a-sign-up-policy).</span><span class="sxs-lookup"><span data-stu-id="72f88-125">You need to create a policy for each type, as described in the [policy reference article](active-directory-b2c-reference-policies.md#create-a-sign-up-policy).</span></span> <span data-ttu-id="72f88-126">Když vytváříte tyto tři zásady, nezapomeňte:</span><span class="sxs-lookup"><span data-stu-id="72f88-126">When you create the three policies, be sure to:</span></span>

* <span data-ttu-id="72f88-127">Zvolit v okně zprostředkovatelé identity buď **Registrace pomocí ID uživatele** nebo **Registrace pomocí e-mailu**.</span><span class="sxs-lookup"><span data-stu-id="72f88-127">Choose either **User ID sign-up** or **Email sign-up** in the identity providers blade.</span></span>
* <span data-ttu-id="72f88-128">Zvolit **Zobrazovaný název** a další atributy registrace ve svojí registrační zásadě.</span><span class="sxs-lookup"><span data-stu-id="72f88-128">Choose **Display name** and other sign-up attributes in your sign-up policy.</span></span>
* <span data-ttu-id="72f88-129">Zvolit **Zobrazovaný název** a deklarace identity **ID objektu** jako deklarace identity aplikace v každé zásadě.</span><span class="sxs-lookup"><span data-stu-id="72f88-129">Choose **Display name** and **Object ID** claims as application claims for every policy.</span></span> <span data-ttu-id="72f88-130">Můžete zvolit i další deklarace identity.</span><span class="sxs-lookup"><span data-stu-id="72f88-130">You can choose other claims as well.</span></span>
* <span data-ttu-id="72f88-131">Po vytvoření každé zásady si poznamenejte její **Název**.</span><span class="sxs-lookup"><span data-stu-id="72f88-131">Copy the **Name** of each policy after you create it.</span></span> <span data-ttu-id="72f88-132">Měl by mít předponu `b2c_1_`.</span><span class="sxs-lookup"><span data-stu-id="72f88-132">It should have the prefix `b2c_1_`.</span></span>  <span data-ttu-id="72f88-133">Tyto názvy zásad budete potřebovat později.</span><span class="sxs-lookup"><span data-stu-id="72f88-133">You'll need these policy names later.</span></span>

[!INCLUDE [active-directory-b2c-devquickstarts-policy](../../includes/active-directory-b2c-devquickstarts-policy.md)]

<span data-ttu-id="72f88-134">Po úspěšném vytvoření těchto tří zásad jste připraveni k sestavení aplikace.</span><span class="sxs-lookup"><span data-stu-id="72f88-134">After you have successfully created the three policies, you're ready to build your app.</span></span>

## <a name="download-the-code"></a><span data-ttu-id="72f88-135">Stáhněte si kód</span><span class="sxs-lookup"><span data-stu-id="72f88-135">Download the code</span></span>
<span data-ttu-id="72f88-136">Kód k tomuto kurzu [je udržovaný na GitHubu](https://github.com/AzureADQuickStarts/B2C-NativeClient-DotNet).</span><span class="sxs-lookup"><span data-stu-id="72f88-136">The code for this tutorial [is maintained on GitHub](https://github.com/AzureADQuickStarts/B2C-NativeClient-DotNet).</span></span> <span data-ttu-id="72f88-137">Chcete-li během čtení tohoto návodu rovnou sestavit ukázku, můžete si [stáhnout kostru projektu v souboru ZIP](https://github.com/AzureADQuickStarts/B2C-NativeClient-DotNet/archive/skeleton.zip).</span><span class="sxs-lookup"><span data-stu-id="72f88-137">To build the sample as you go, you can [download a skeleton project as a .zip file](https://github.com/AzureADQuickStarts/B2C-NativeClient-DotNet/archive/skeleton.zip).</span></span> <span data-ttu-id="72f88-138">Kostru můžete také klonovat:</span><span class="sxs-lookup"><span data-stu-id="72f88-138">You can also clone the skeleton:</span></span>

```
git clone --branch skeleton https://github.com/AzureADQuickStarts/B2C-NativeClient-DotNet.git
```

<span data-ttu-id="72f88-139">Dokončená aplikace je také [k dispozici jako soubor ZIP](https://github.com/AzureADQuickStarts/B2C-NativeClient-DotNet/archive/complete.zip) nebo ve větvi `complete` stejného úložiště.</span><span class="sxs-lookup"><span data-stu-id="72f88-139">The completed app is also [available as a .zip file](https://github.com/AzureADQuickStarts/B2C-NativeClient-DotNet/archive/complete.zip) or on the `complete` branch of the same repository.</span></span>

<span data-ttu-id="72f88-140">Po stažení ukázkového kódu otevřete soubor Visual Studio .sln, abyste mohli začít.</span><span class="sxs-lookup"><span data-stu-id="72f88-140">After you download the sample code, open the Visual Studio .sln file to get started.</span></span> <span data-ttu-id="72f88-141">`TaskClient` Je projekt WPF plochy aplikace, která uživatel komunikuje.</span><span class="sxs-lookup"><span data-stu-id="72f88-141">The `TaskClient` project is the WPF desktop application that the user interacts with.</span></span> <span data-ttu-id="72f88-142">Pro účely tohoto kurzu zavolá back-end úloh webového rozhraní API, hostované v Azure, která ukládá seznam úkolů každého uživatele.</span><span class="sxs-lookup"><span data-stu-id="72f88-142">For the purposes of this tutorial, it calls a back-end task web API, hosted in Azure, that stores each user's to-do list.</span></span>  <span data-ttu-id="72f88-143">Není potřeba vytvářet webové rozhraní API, jsme ji systémem pro vás už máte.</span><span class="sxs-lookup"><span data-stu-id="72f88-143">You do not need to build the web API, we already have it running for you.</span></span>

<span data-ttu-id="72f88-144">Informace o tom, jak webové rozhraní API bezpečně ověřuje požadavků pomocí Azure AD B2C, podívejte se [webové rozhraní API Začínáme článku](active-directory-b2c-devquickstarts-api-dotnet.md).</span><span class="sxs-lookup"><span data-stu-id="72f88-144">To learn how a web API securely authenticates requests by using Azure AD B2C, check out the [web API getting started article](active-directory-b2c-devquickstarts-api-dotnet.md).</span></span>

## <a name="execute-policies"></a><span data-ttu-id="72f88-145">Spuštění zásady</span><span class="sxs-lookup"><span data-stu-id="72f88-145">Execute policies</span></span>
<span data-ttu-id="72f88-146">Aplikace komunikuje se službou Azure AD B2C odesláním zprávy ověřování, které zadejte zásady, které chtějí spouštět jako součást požadavku HTTP.</span><span class="sxs-lookup"><span data-stu-id="72f88-146">Your app communicates with Azure AD B2C by sending authentication messages that specify the policy they want to execute as part of the HTTP request.</span></span> <span data-ttu-id="72f88-147">Pro rozhraní .NET aplikací klasické pracovní plochy můžete pomocí náhledu Microsoft ověřování knihovny (MSAL) odesílat zprávy ověřování OAuth 2.0, spustit zásady a získat tokeny, které volají webové rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="72f88-147">For .NET desktop applications, you can use the preview Microsoft Authentication Library (MSAL) to send OAuth 2.0 authentication messages, execute policies, and get tokens that call web APIs.</span></span>

### <a name="install-msal"></a><span data-ttu-id="72f88-148">Nainstalujte MSAL</span><span class="sxs-lookup"><span data-stu-id="72f88-148">Install MSAL</span></span>
<span data-ttu-id="72f88-149">Přidat MSAL k `TaskClient` projektu pomocí konzole Správce balíčků Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="72f88-149">Add MSAL to the `TaskClient` project by using the Visual Studio Package Manager Console.</span></span>

```
PM> Install-Package Microsoft.Identity.Client -IncludePrerelease
```

### <a name="enter-your-b2c-details"></a><span data-ttu-id="72f88-150">Zadejte podrobnosti o svém B2C</span><span class="sxs-lookup"><span data-stu-id="72f88-150">Enter your B2C details</span></span>
<span data-ttu-id="72f88-151">Otevřete soubor `Globals.cs` a všechny hodnoty vlastností, nahraďte vlastními.</span><span class="sxs-lookup"><span data-stu-id="72f88-151">Open the file `Globals.cs` and replace each of the property values with your own.</span></span> <span data-ttu-id="72f88-152">Tato třída se používá v rámci `TaskClient` odkaz běžně používané hodnoty.</span><span class="sxs-lookup"><span data-stu-id="72f88-152">This class is used throughout `TaskClient` to reference commonly used values.</span></span>

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

### <a name="create-the-publicclientapplication"></a><span data-ttu-id="72f88-153">Vytvořte PublicClientApplication</span><span class="sxs-lookup"><span data-stu-id="72f88-153">Create the PublicClientApplication</span></span>
<span data-ttu-id="72f88-154">Primární třída MSAL je `PublicClientApplication`.</span><span class="sxs-lookup"><span data-stu-id="72f88-154">The primary class of MSAL is `PublicClientApplication`.</span></span> <span data-ttu-id="72f88-155">Tato třída reprezentuje vaší aplikace v systému Azure AD B2C.</span><span class="sxs-lookup"><span data-stu-id="72f88-155">This class represents your application in the Azure AD B2C system.</span></span> <span data-ttu-id="72f88-156">Když initalizes aplikace, vytvořte instanci `PublicClientApplication` v `MainWindow.xaml.cs`.</span><span class="sxs-lookup"><span data-stu-id="72f88-156">When the app initalizes, create an instance of `PublicClientApplication` in `MainWindow.xaml.cs`.</span></span> <span data-ttu-id="72f88-157">To lze použít v celé okno.</span><span class="sxs-lookup"><span data-stu-id="72f88-157">This can be used throughout the window.</span></span>

```C#
protected async override void OnInitialized(EventArgs e)
{
    base.OnInitialized(e);

    pca = new PublicClientApplication(Globals.clientId)
    {
        // MSAL implements an in-memory cache by default.  Since we want tokens to persist when the user closes the app,
        // we've extended the MSAL TokenCache and created a simple FileCache in this app.
        UserTokenCache = new FileCache(),
    };

    ...
```

### <a name="initiate-a-sign-up-flow"></a><span data-ttu-id="72f88-158">Zahájení registrace toku</span><span class="sxs-lookup"><span data-stu-id="72f88-158">Initiate a sign-up flow</span></span>
<span data-ttu-id="72f88-159">Když uživatel rozhodne pro přihlásí nahoru, chcete zahájit registraci toku, který používá registrační zásadě, kterou jste vytvořili.</span><span class="sxs-lookup"><span data-stu-id="72f88-159">When a user opts to signs up, you want to initiate a sign-up flow that uses the sign-up policy you created.</span></span> <span data-ttu-id="72f88-160">Pomocí MSAL jen zavoláte `pca.AcquireTokenAsync(...)`.</span><span class="sxs-lookup"><span data-stu-id="72f88-160">By using MSAL, you just call `pca.AcquireTokenAsync(...)`.</span></span> <span data-ttu-id="72f88-161">Parametry, které předat `AcquireTokenAsync(...)` určit, které token se zobrazí, zásady používané v žádosti o ověření a další.</span><span class="sxs-lookup"><span data-stu-id="72f88-161">The parameters you pass to `AcquireTokenAsync(...)` determine which token you receive, the policy used in the authentication request, and more.</span></span>

```C#
private async void SignUp(object sender, RoutedEventArgs e)
{
    AuthenticationResult result = null;
    try
    {
        // Use the app's clientId here as the scope parameter, indicating that
        // you want a token to the your app's backend web API (represented by
        // the cloud hosted task API).  Use the UiOptions.ForceLogin flag to
        // indicate to MSAL that it should show a sign-up UI no matter what.
        result = await pca.AcquireTokenAsync(new string[] { Globals.clientId },
                string.Empty, UiOptions.ForceLogin, null, null, Globals.authority,
                Globals.signUpPolicy);

        // Upon success, indicate in the app that the user is signed in.
        SignInButton.Visibility = Visibility.Collapsed;
        SignUpButton.Visibility = Visibility.Collapsed;
        EditProfileButton.Visibility = Visibility.Visible;
        SignOutButton.Visibility = Visibility.Visible;

        // When the request completes successfully, you can get user
        // information from the AuthenticationResult
        UsernameLabel.Content = result.User.Name;

        // After the sign up successfully completes, display the user's To-Do List
        GetTodoList();
    }

    // Handle any exeptions that occurred during execution of the policy.
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

### <a name="initiate-a-sign-in-flow"></a><span data-ttu-id="72f88-162">Zahájit toku přihlášení</span><span class="sxs-lookup"><span data-stu-id="72f88-162">Initiate a sign-in flow</span></span>
<span data-ttu-id="72f88-163">Tok přihlášení můžete zahájit stejným způsobem zahájení registrace toku.</span><span class="sxs-lookup"><span data-stu-id="72f88-163">You can initiate a sign-in flow in the same way that you initiate a sign-up flow.</span></span> <span data-ttu-id="72f88-164">Když se uživatel přihlásí, ujistěte se stejným volání MSAL, tentokrát pomocí přihlášení zásad:</span><span class="sxs-lookup"><span data-stu-id="72f88-164">When a user signs in, make the same call to MSAL, this time by using your sign-in policy:</span></span>

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

### <a name="initiate-an-edit-profile-flow"></a><span data-ttu-id="72f88-165">Zahájit tok úpravy profilu</span><span class="sxs-lookup"><span data-stu-id="72f88-165">Initiate an edit-profile flow</span></span>
<span data-ttu-id="72f88-166">Znovu můžete zásadu upravit profil spustit stejným způsobem:</span><span class="sxs-lookup"><span data-stu-id="72f88-166">Again, you can execute an edit-profile policy in the same fashion:</span></span>

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

<span data-ttu-id="72f88-167">Ve všech těchto případech MSAL buď vrátí token v `AuthenticationResult` nebo vyvolá výjimku.</span><span class="sxs-lookup"><span data-stu-id="72f88-167">In all of these cases, MSAL either returns a token in `AuthenticationResult` or throws an exception.</span></span> <span data-ttu-id="72f88-168">Pokaždé, když získání tokenu z MSAL, můžete použít `AuthenticationResult.User` objekt, který chcete aktualizovat data uživatele v aplikaci, jako je například uživatelské rozhraní.</span><span class="sxs-lookup"><span data-stu-id="72f88-168">Each time you get a token from MSAL, you can use the `AuthenticationResult.User` object to update the user data in the app, such as the UI.</span></span> <span data-ttu-id="72f88-169">ADAL také ukládá do mezipaměti token pro použití v dalších částí aplikace.</span><span class="sxs-lookup"><span data-stu-id="72f88-169">ADAL also caches the token for use in other parts of the application.</span></span>

### <a name="check-for-tokens-on-app-start"></a><span data-ttu-id="72f88-170">Zkontrolujte pro tokeny při spuštění aplikace</span><span class="sxs-lookup"><span data-stu-id="72f88-170">Check for tokens on app start</span></span>
<span data-ttu-id="72f88-171">MSAL můžete také použít ke sledování stavu přihlášení uživatele.</span><span class="sxs-lookup"><span data-stu-id="72f88-171">You can also use MSAL to keep track of the user's sign-in state.</span></span>  <span data-ttu-id="72f88-172">V této aplikaci chceme uživateli zůstanou přihlášeného i po jejich zavřete aplikaci a znovu ho otevřete.</span><span class="sxs-lookup"><span data-stu-id="72f88-172">In this app, we want the user to remain signed in even after they close the app & re-open it.</span></span>  <span data-ttu-id="72f88-173">Zpět v `OnInitialized` přepsat, použijte na MSAL `AcquireTokenSilent` metoda zkontrolujte s mezipamětí tokenů:</span><span class="sxs-lookup"><span data-stu-id="72f88-173">Back inside the `OnInitialized` override, use MSAL's `AcquireTokenSilent` method to check for cached tokens:</span></span>

```C#
AuthenticationResult result = null;
try
{
    // If the user has has a token cached with any policy, we'll display them as signed-in.
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
        // There are no tokens in the cache.  Proceed without calling the To Do list service.
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

## <a name="call-the-task-api"></a><span data-ttu-id="72f88-174">Volání rozhraní API úloh</span><span class="sxs-lookup"><span data-stu-id="72f88-174">Call the task API</span></span>
<span data-ttu-id="72f88-175">Teď používáte MSAL ke spouštění zásad a získat tokeny.</span><span class="sxs-lookup"><span data-stu-id="72f88-175">You have now used MSAL to execute policies and get tokens.</span></span>  <span data-ttu-id="72f88-176">Když chcete použít tyto tokeny k volání rozhraní API úloh, můžete znovu použít na MSAL `AcquireTokenSilent` metoda zkontrolujte s mezipamětí tokenů:</span><span class="sxs-lookup"><span data-stu-id="72f88-176">When you want to use one these tokens to call the task API, you can again use MSAL's `AcquireTokenSilent` method to check for cached tokens:</span></span>

```C#
private async void GetTodoList()
{
    AuthenticationResult result = null;
    try
    {
        // Here we want to check for a cached token, independent of whatever policy was used to acquire it.
        TokenCacheItem tci = pca.UserTokenCache.ReadItems(Globals.clientId).Where(i => i.Scope.Contains(Globals.clientId) && !string.IsNullOrEmpty(i.Policy)).FirstOrDefault();
        string existingPolicy = tci == null ? null : tci.Policy;

        // Use AcquireTokenSilent to indicate that MSAL should throw an exception if a token cannot be acquired
        result = await pca.AcquireTokenSilentAsync(new string[] { Globals.clientId }, string.Empty, Globals.authority, existingPolicy, false);

    }
    // If a token could not be acquired silently, we'll catch the exception and show the user a message.
    catch (MsalException ex)
    {
        // There is no access token in the cache, so prompt the user to sign-in.
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

<span data-ttu-id="72f88-177">Při volání `AcquireTokenSilentAsync(...)` úspěšné a nebude nalezen token v mezipaměti, můžete přidat token, který má `Authorization` hlavičky požadavku HTTP.</span><span class="sxs-lookup"><span data-stu-id="72f88-177">When the call to `AcquireTokenSilentAsync(...)` succeeds and a token is found in the cache, you can add the token to the `Authorization` header of the HTTP request.</span></span> <span data-ttu-id="72f88-178">Úloha webové rozhraní API bude tuto hlavičku používají k ověření požadavek na čtení seznamu úkolů uživatele:</span><span class="sxs-lookup"><span data-stu-id="72f88-178">The task web API will use this header to authenticate the request to read the user's to-do list:</span></span>

```C#
    ...
    // Once the token has been returned by MSAL, add it to the http authorization header, before making the call to access the To Do list service.
    httpClient.DefaultRequestHeaders.Authorization = new AuthenticationHeaderValue("Bearer", result.Token);

    // Call the To Do list service.
    HttpResponseMessage response = await httpClient.GetAsync(Globals.taskServiceUrl + "/api/tasks");
    ...
```

## <a name="sign-the-user-out"></a><span data-ttu-id="72f88-179">Odhlášení uživatele</span><span class="sxs-lookup"><span data-stu-id="72f88-179">Sign the user out</span></span>
<span data-ttu-id="72f88-180">Nakonec můžete MSAL k ukončení relace uživatele s aplikací, když uživatel vybere **Odhlásit se**.</span><span class="sxs-lookup"><span data-stu-id="72f88-180">Finally, you can use MSAL to end a user's session with the app when the user selects **Sign out**.</span></span>  <span data-ttu-id="72f88-181">Při použití MSAL, toho dosahuje tím, že zrušíte všechny tokeny z tokenu mezipaměti:</span><span class="sxs-lookup"><span data-stu-id="72f88-181">When using MSAL, this is accomplished by clearing all of the tokens from the token cache:</span></span>

```C#
private void SignOut(object sender, RoutedEventArgs e)
{
    // Clear any remnants of the user's session.
    pca.UserTokenCache.Clear(Globals.clientId);

    // This is a helper method that clears browser cookies in the browser control that MSAL uses, it is not part of MSAL.
    ClearCookies();

    // Update the UI to show the user as signed out.
    TaskList.ItemsSource = string.Empty;
    SignInButton.Visibility = Visibility.Visible;
    SignUpButton.Visibility = Visibility.Visible;
    EditProfileButton.Visibility = Visibility.Collapsed;
    SignOutButton.Visibility = Visibility.Collapsed;
    return;
}
```

## <a name="run-the-sample-app"></a><span data-ttu-id="72f88-182">Spuštění ukázkové aplikace</span><span class="sxs-lookup"><span data-stu-id="72f88-182">Run the sample app</span></span>
<span data-ttu-id="72f88-183">Nakonec sestavte a spusťte vzorku.</span><span class="sxs-lookup"><span data-stu-id="72f88-183">Finally, build and run the sample.</span></span>  <span data-ttu-id="72f88-184">Zaregistrujte se pro aplikace pomocí e-mailové adresy nebo uživatelské jméno.</span><span class="sxs-lookup"><span data-stu-id="72f88-184">Sign up for the app by using an email address or user name.</span></span> <span data-ttu-id="72f88-185">Odhlásit se a znovu se přihlaste jako jeden uživatel.</span><span class="sxs-lookup"><span data-stu-id="72f88-185">Sign out and sign back in as the same user.</span></span> <span data-ttu-id="72f88-186">Upravte profil uživatele.</span><span class="sxs-lookup"><span data-stu-id="72f88-186">Edit that user's profile.</span></span> <span data-ttu-id="72f88-187">Odhlásit se a přihlásit pomocí jiného uživatele.</span><span class="sxs-lookup"><span data-stu-id="72f88-187">Sign out and sign up by using a different user.</span></span>

## <a name="add-social-idps"></a><span data-ttu-id="72f88-188">Přidat sociálních IDPs</span><span class="sxs-lookup"><span data-stu-id="72f88-188">Add social IDPs</span></span>
<span data-ttu-id="72f88-189">V současné době aplikace podporuje pouze uživatele registrace a přihlášení, použít **místní účty**.</span><span class="sxs-lookup"><span data-stu-id="72f88-189">Currently, the app supports only user sign-up and sign-in that use **local accounts**.</span></span> <span data-ttu-id="72f88-190">Toto jsou účty uložené v adresáři B2C, které používají uživatelské jméno a heslo.</span><span class="sxs-lookup"><span data-stu-id="72f88-190">These are accounts stored in your B2C directory that use a user name and password.</span></span> <span data-ttu-id="72f88-191">Pomocí Azure AD B2C můžete přidat podporu pro jiných poskytovatelů identit (IDPs) beze změny některé z vašeho kódu.</span><span class="sxs-lookup"><span data-stu-id="72f88-191">By using Azure AD B2C, you can add support for other identity providers (IDPs) without changing any of your code.</span></span>

<span data-ttu-id="72f88-192">Sociální IDPs přidat do vaší aplikace, začněte tím, že následující podrobné pokyny v těchto článcích.</span><span class="sxs-lookup"><span data-stu-id="72f88-192">To add social IDPs to your app, begin by following the detailed instructions in these articles.</span></span> <span data-ttu-id="72f88-193">Pro každý deklarací identity, které chcete podporovat je třeba zaregistrovat aplikaci v daném systému a získat ID klienta.</span><span class="sxs-lookup"><span data-stu-id="72f88-193">For each IDP you want to support, you need to register an application in that system and obtain a client ID.</span></span>

* [<span data-ttu-id="72f88-194">Nastavení sítě Facebook jako IDP</span><span class="sxs-lookup"><span data-stu-id="72f88-194">Set up Facebook as an IDP</span></span>](active-directory-b2c-setup-fb-app.md)
* [<span data-ttu-id="72f88-195">Nastavit Google jako IDP</span><span class="sxs-lookup"><span data-stu-id="72f88-195">Set up Google as an IDP</span></span>](active-directory-b2c-setup-goog-app.md)
* [<span data-ttu-id="72f88-196">Nastavit Amazon jako IDP</span><span class="sxs-lookup"><span data-stu-id="72f88-196">Set up Amazon as an IDP</span></span>](active-directory-b2c-setup-amzn-app.md)
* [<span data-ttu-id="72f88-197">Nastavit LinkedIn jako IDP</span><span class="sxs-lookup"><span data-stu-id="72f88-197">Set up LinkedIn as an IDP</span></span>](active-directory-b2c-setup-li-app.md)

<span data-ttu-id="72f88-198">Po přidání zprostředkovatelů identity do vašeho adresáře B2C, budete muset upravit každou z vaší tři zásady, které zahrnují nové IDPs, jak je popsáno v [článku o zásadách](active-directory-b2c-reference-policies.md).</span><span class="sxs-lookup"><span data-stu-id="72f88-198">After you add the identity providers to your B2C directory, you need to edit each of your three policies to include the new IDPs, as described in the [policy reference article](active-directory-b2c-reference-policies.md).</span></span> <span data-ttu-id="72f88-199">Po uložení zásad, znovu spusťte aplikaci.</span><span class="sxs-lookup"><span data-stu-id="72f88-199">After you save your policies, run the app again.</span></span> <span data-ttu-id="72f88-200">Měli byste vidět nové IDPs přidat jako přihlášení a registrace možnosti v každé z vaší identity činnost.</span><span class="sxs-lookup"><span data-stu-id="72f88-200">You should see the new IDPs added as sign-in and sign-up options in each of your identity experiences.</span></span>

<span data-ttu-id="72f88-201">Můžete experimentovat s vašimi zásadami a sledovat účinky na ukázkové aplikace.</span><span class="sxs-lookup"><span data-stu-id="72f88-201">You can experiment with your policies and observe the effects on your sample app.</span></span> <span data-ttu-id="72f88-202">Přidat nebo odebrat IDPs, pracovat s deklarace identity aplikace nebo změnit atributy registrace.</span><span class="sxs-lookup"><span data-stu-id="72f88-202">Add or remove IDPs, manipulate application claims, or change sign-up attributes.</span></span> <span data-ttu-id="72f88-203">Experiment, dokud se nezobrazí, jak zásady, žádosti o ověření a MSAL tie společně.</span><span class="sxs-lookup"><span data-stu-id="72f88-203">Experiment until you can see how policies, authentication requests, and MSAL tie together.</span></span>

<span data-ttu-id="72f88-204">Pro srovnání je hotová ukázka [je k dispozici jako soubor ZIP](https://github.com/AzureADQuickStarts/B2C-NativeClient-DotNet/archive/complete.zip).</span><span class="sxs-lookup"><span data-stu-id="72f88-204">For reference, the completed sample [is provided as a .zip file](https://github.com/AzureADQuickStarts/B2C-NativeClient-DotNet/archive/complete.zip).</span></span> <span data-ttu-id="72f88-205">Můžete ho také klonovat z GitHubu:</span><span class="sxs-lookup"><span data-stu-id="72f88-205">You can also clone it from GitHub:</span></span>

```git clone --branch complete https://github.com/AzureADQuickStarts/B2C-NativeClient-DotNet.git```
