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
# <a name="azure-ad-b2c-build-a-windows-desktop-app"></a><span data-ttu-id="3e50d-103">Azure AD B2C: Sestavení aplikace na ploše systému Windows</span><span class="sxs-lookup"><span data-stu-id="3e50d-103">Azure AD B2C: Build a Windows desktop app</span></span>
<span data-ttu-id="3e50d-104">Pomocí Azure Active Directory (Azure AD) B2C můžete přidat výkonné samoobslužná služba identity management funkce tooyour desktopová aplikace v několika krocích.</span><span class="sxs-lookup"><span data-stu-id="3e50d-104">By using Azure Active Directory (Azure AD) B2C, you can add powerful self-service identity management features tooyour desktop app in a few short steps.</span></span> <span data-ttu-id="3e50d-105">Tento článek vám ukáže, jak toocreate .NET Windows Presentation Foundation (WPF) "seznam úkolů" aplikaci, která zahrnuje uživatelskou registraci, přihlašování a správu profilu.</span><span class="sxs-lookup"><span data-stu-id="3e50d-105">This article will show you how toocreate a .NET Windows Presentation Foundation (WPF) "to-do list" app that includes user sign-up, sign-in, and profile management.</span></span> <span data-ttu-id="3e50d-106">aplikace Hello bude zahrnují podporu pro registraci a přihlaste se pomocí uživatelského jména nebo e-mailu.</span><span class="sxs-lookup"><span data-stu-id="3e50d-106">hello app will include support for sign-up and sign-in by using a user name or email.</span></span> <span data-ttu-id="3e50d-107">Podporu registrace a přihlašování bude zahrnovat také pomocí sociálních účty například Facebook nebo Google.</span><span class="sxs-lookup"><span data-stu-id="3e50d-107">It will also include support for sign-up and sign-in by using social accounts such as Facebook and Google.</span></span>

## <a name="get-an-azure-ad-b2c-directory"></a><span data-ttu-id="3e50d-108">Získání adresáře služby Azure AD B2C</span><span class="sxs-lookup"><span data-stu-id="3e50d-108">Get an Azure AD B2C directory</span></span>
<span data-ttu-id="3e50d-109">Před použitím Azure AD B2C musíte vytvořit adresář, nebo klienta.</span><span class="sxs-lookup"><span data-stu-id="3e50d-109">Before you can use Azure AD B2C, you must create a directory, or tenant.</span></span>  <span data-ttu-id="3e50d-110">Adresář je kontejner pro všechny vaše uživatele, aplikace, skupiny a další.</span><span class="sxs-lookup"><span data-stu-id="3e50d-110">A directory is a container for all of your users, apps, groups, and more.</span></span> <span data-ttu-id="3e50d-111">Pokud ho ještě nemáte, [vytvořte adresář B2C](active-directory-b2c-get-started.md) předtím, než budete pokračovat.</span><span class="sxs-lookup"><span data-stu-id="3e50d-111">If you don't have one already, [create a B2C directory](active-directory-b2c-get-started.md) before you continue in this guide.</span></span>

## <a name="create-an-application"></a><span data-ttu-id="3e50d-112">Vytvoření aplikace</span><span class="sxs-lookup"><span data-stu-id="3e50d-112">Create an application</span></span>
<span data-ttu-id="3e50d-113">Dále musíte toocreate aplikace ve svém adresáři B2C.</span><span class="sxs-lookup"><span data-stu-id="3e50d-113">Next, you need toocreate an app in your B2C directory.</span></span> <span data-ttu-id="3e50d-114">Díky tomu získá informace o Azure AD, se vyžaduje toosecurely komunikaci s vaší aplikací.</span><span class="sxs-lookup"><span data-stu-id="3e50d-114">This gives Azure AD information that it needs toosecurely communicate with your app.</span></span> <span data-ttu-id="3e50d-115">toocreate na aplikace, postupujte podle [tyto pokyny](active-directory-b2c-app-registration.md).</span><span class="sxs-lookup"><span data-stu-id="3e50d-115">toocreate an app, follow [these instructions](active-directory-b2c-app-registration.md).</span></span>  <span data-ttu-id="3e50d-116">Ujistěte se, že:</span><span class="sxs-lookup"><span data-stu-id="3e50d-116">Be sure to:</span></span>

* <span data-ttu-id="3e50d-117">Zahrnout **nativního klienta** v aplikaci hello.</span><span class="sxs-lookup"><span data-stu-id="3e50d-117">Include a **native client** in hello application.</span></span>
* <span data-ttu-id="3e50d-118">Kopírování hello **identifikátor URI pro přesměrování** `urn:ietf:wg:oauth:2.0:oob`.</span><span class="sxs-lookup"><span data-stu-id="3e50d-118">Copy hello **Redirect URI** `urn:ietf:wg:oauth:2.0:oob`.</span></span> <span data-ttu-id="3e50d-119">Je hello výchozí adresa URL pro tuto ukázku kódu.</span><span class="sxs-lookup"><span data-stu-id="3e50d-119">It is hello default URL for this code sample.</span></span>
* <span data-ttu-id="3e50d-120">Kopírování hello **ID aplikace** který je přiřazený tooyour aplikace.</span><span class="sxs-lookup"><span data-stu-id="3e50d-120">Copy hello **Application ID** that is assigned tooyour app.</span></span> <span data-ttu-id="3e50d-121">Budete ho potřebovat později.</span><span class="sxs-lookup"><span data-stu-id="3e50d-121">You will need it later.</span></span>

[!INCLUDE [active-directory-b2c-devquickstarts-v2-apps](../../includes/active-directory-b2c-devquickstarts-v2-apps.md)]

## <a name="create-your-policies"></a><span data-ttu-id="3e50d-122">Vytvořte svoje zásady</span><span class="sxs-lookup"><span data-stu-id="3e50d-122">Create your policies</span></span>
<span data-ttu-id="3e50d-123">V Azure AD B2C je každé uživatelské rozhraní definováno [zásadou](active-directory-b2c-reference-policies.md).</span><span class="sxs-lookup"><span data-stu-id="3e50d-123">In Azure AD B2C, every user experience is defined by a [policy](active-directory-b2c-reference-policies.md).</span></span> <span data-ttu-id="3e50d-124">Tato ukázka kódu obsahuje tři činnosti identity: registrace, přihlášení a úprava profilu.</span><span class="sxs-lookup"><span data-stu-id="3e50d-124">This code sample contains three identity experiences: sign up, sign in, and edit profile.</span></span> <span data-ttu-id="3e50d-125">Jak je popsáno v musíte pro každý typ toocreate zásadu [článku o zásadách](active-directory-b2c-reference-policies.md#create-a-sign-up-policy).</span><span class="sxs-lookup"><span data-stu-id="3e50d-125">You need toocreate a policy for each type, as described in the [policy reference article](active-directory-b2c-reference-policies.md#create-a-sign-up-policy).</span></span> <span data-ttu-id="3e50d-126">Když vytvoříte hello tři zásady, nezapomeňte:</span><span class="sxs-lookup"><span data-stu-id="3e50d-126">When you create hello three policies, be sure to:</span></span>

* <span data-ttu-id="3e50d-127">Vyberte buď **registrace pomocí ID uživatele** nebo **e-mailu registrace** v okně zprostředkovatelé identity hello.</span><span class="sxs-lookup"><span data-stu-id="3e50d-127">Choose either **User ID sign-up** or **Email sign-up** in hello identity providers blade.</span></span>
* <span data-ttu-id="3e50d-128">Zvolit **Zobrazovaný název** a další atributy registrace ve svojí registrační zásadě.</span><span class="sxs-lookup"><span data-stu-id="3e50d-128">Choose **Display name** and other sign-up attributes in your sign-up policy.</span></span>
* <span data-ttu-id="3e50d-129">Zvolit **Zobrazovaný název** a deklarace identity **ID objektu** jako deklarace identity aplikace v každé zásadě.</span><span class="sxs-lookup"><span data-stu-id="3e50d-129">Choose **Display name** and **Object ID** claims as application claims for every policy.</span></span> <span data-ttu-id="3e50d-130">Můžete zvolit i další deklarace identity.</span><span class="sxs-lookup"><span data-stu-id="3e50d-130">You can choose other claims as well.</span></span>
* <span data-ttu-id="3e50d-131">Kopírování hello **název** po jejím vytvoření každé zásady.</span><span class="sxs-lookup"><span data-stu-id="3e50d-131">Copy hello **Name** of each policy after you create it.</span></span> <span data-ttu-id="3e50d-132">Měl by mít předponu hello `b2c_1_`.</span><span class="sxs-lookup"><span data-stu-id="3e50d-132">It should have hello prefix `b2c_1_`.</span></span>  <span data-ttu-id="3e50d-133">Tyto názvy zásad budete potřebovat později.</span><span class="sxs-lookup"><span data-stu-id="3e50d-133">You'll need these policy names later.</span></span>

[!INCLUDE [active-directory-b2c-devquickstarts-policy](../../includes/active-directory-b2c-devquickstarts-policy.md)]

<span data-ttu-id="3e50d-134">Po úspěšném vytvoření hello tři zásady, jste připravené toobuild vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="3e50d-134">After you have successfully created hello three policies, you're ready toobuild your app.</span></span>

## <a name="download-hello-code"></a><span data-ttu-id="3e50d-135">Stáhněte si kód hello</span><span class="sxs-lookup"><span data-stu-id="3e50d-135">Download hello code</span></span>
<span data-ttu-id="3e50d-136">Hello kód pro tento kurz [je udržovaný na Githubu](https://github.com/AzureADQuickStarts/B2C-NativeClient-DotNet).</span><span class="sxs-lookup"><span data-stu-id="3e50d-136">hello code for this tutorial [is maintained on GitHub](https://github.com/AzureADQuickStarts/B2C-NativeClient-DotNet).</span></span> <span data-ttu-id="3e50d-137">Ukázka hello toobuild jako můžete přejít, můžete [stáhnout kostru projektu v souboru ZIP](https://github.com/AzureADQuickStarts/B2C-NativeClient-DotNet/archive/skeleton.zip).</span><span class="sxs-lookup"><span data-stu-id="3e50d-137">toobuild hello sample as you go, you can [download a skeleton project as a .zip file](https://github.com/AzureADQuickStarts/B2C-NativeClient-DotNet/archive/skeleton.zip).</span></span> <span data-ttu-id="3e50d-138">Můžete také hello kostru klonovat:</span><span class="sxs-lookup"><span data-stu-id="3e50d-138">You can also clone hello skeleton:</span></span>

```
git clone --branch skeleton https://github.com/AzureADQuickStarts/B2C-NativeClient-DotNet.git
```

<span data-ttu-id="3e50d-139">aplikace Hello dokončit, je také [k dispozici jako soubor ZIP](https://github.com/AzureADQuickStarts/B2C-NativeClient-DotNet/archive/complete.zip) nebo na hello `complete` větve hello stejného úložiště.</span><span class="sxs-lookup"><span data-stu-id="3e50d-139">hello completed app is also [available as a .zip file](https://github.com/AzureADQuickStarts/B2C-NativeClient-DotNet/archive/complete.zip) or on hello `complete` branch of hello same repository.</span></span>

<span data-ttu-id="3e50d-140">Po stažení ukázkového kódu hello hello otevřete Visual Studio .sln souboru tooget spustit.</span><span class="sxs-lookup"><span data-stu-id="3e50d-140">After you download hello sample code, open hello Visual Studio .sln file tooget started.</span></span> <span data-ttu-id="3e50d-141">Hello `TaskClient` je projekt hello WPF plochy aplikace, která hello uživatel komunikuje se službou.</span><span class="sxs-lookup"><span data-stu-id="3e50d-141">hello `TaskClient` project is hello WPF desktop application that hello user interacts with.</span></span> <span data-ttu-id="3e50d-142">Pro účely tohoto kurzu hello zavolá back-end úloh webového rozhraní API, hostované v Azure, která ukládá seznam úkolů každého uživatele.</span><span class="sxs-lookup"><span data-stu-id="3e50d-142">For hello purposes of this tutorial, it calls a back-end task web API, hosted in Azure, that stores each user's to-do list.</span></span>  <span data-ttu-id="3e50d-143">Není nutné toobuild hello webového rozhraní API, jsme ji systémem pro vás už máte.</span><span class="sxs-lookup"><span data-stu-id="3e50d-143">You do not need toobuild hello web API, we already have it running for you.</span></span>

<span data-ttu-id="3e50d-144">toolearn jak webové rozhraní API bezpečně ověřuje požadavků pomocí Azure AD B2C, podívejte se [webové rozhraní API Začínáme článku](active-directory-b2c-devquickstarts-api-dotnet.md).</span><span class="sxs-lookup"><span data-stu-id="3e50d-144">toolearn how a web API securely authenticates requests by using Azure AD B2C, check out the [web API getting started article](active-directory-b2c-devquickstarts-api-dotnet.md).</span></span>

## <a name="execute-policies"></a><span data-ttu-id="3e50d-145">Spuštění zásady</span><span class="sxs-lookup"><span data-stu-id="3e50d-145">Execute policies</span></span>
<span data-ttu-id="3e50d-146">Aplikace komunikuje se službou Azure AD B2C odesláním zprávy ověřování, které zadejte zásady hello chtějí tooexecute jako součást požadavku hello protokolu HTTP.</span><span class="sxs-lookup"><span data-stu-id="3e50d-146">Your app communicates with Azure AD B2C by sending authentication messages that specify hello policy they want tooexecute as part of hello HTTP request.</span></span> <span data-ttu-id="3e50d-147">U aplikací klasické pracovní plochy rozhraní .NET, můžete použít hello náhled zpráv ověřování OAuth 2.0 toosend Microsoft ověřování knihovny (MSAL), zásady spouštění a získat tokeny tohoto volání webových rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="3e50d-147">For .NET desktop applications, you can use hello preview Microsoft Authentication Library (MSAL) toosend OAuth 2.0 authentication messages, execute policies, and get tokens that call web APIs.</span></span>

### <a name="install-msal"></a><span data-ttu-id="3e50d-148">Nainstalujte MSAL</span><span class="sxs-lookup"><span data-stu-id="3e50d-148">Install MSAL</span></span>
<span data-ttu-id="3e50d-149">Přidat MSAL toohello `TaskClient` projektu pomocí hello Konzola správce balíčků Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="3e50d-149">Add MSAL toohello `TaskClient` project by using hello Visual Studio Package Manager Console.</span></span>

```
PM> Install-Package Microsoft.Identity.Client -IncludePrerelease
```

### <a name="enter-your-b2c-details"></a><span data-ttu-id="3e50d-150">Zadejte podrobnosti o svém B2C</span><span class="sxs-lookup"><span data-stu-id="3e50d-150">Enter your B2C details</span></span>
<span data-ttu-id="3e50d-151">Soubor otevřete hello `Globals.cs` a nahraďte všechny hodnoty vlastností hello vlastní.</span><span class="sxs-lookup"><span data-stu-id="3e50d-151">Open hello file `Globals.cs` and replace each of hello property values with your own.</span></span> <span data-ttu-id="3e50d-152">Tato třída se používá v rámci `TaskClient` tooreference běžně používané hodnoty.</span><span class="sxs-lookup"><span data-stu-id="3e50d-152">This class is used throughout `TaskClient` tooreference commonly used values.</span></span>

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

### <a name="create-hello-publicclientapplication"></a><span data-ttu-id="3e50d-153">Vytvoření hello PublicClientApplication</span><span class="sxs-lookup"><span data-stu-id="3e50d-153">Create hello PublicClientApplication</span></span>
<span data-ttu-id="3e50d-154">Hello primární třída MSAL je `PublicClientApplication`.</span><span class="sxs-lookup"><span data-stu-id="3e50d-154">hello primary class of MSAL is `PublicClientApplication`.</span></span> <span data-ttu-id="3e50d-155">Tato třída reprezentuje vaší aplikace v systému hello Azure AD B2C.</span><span class="sxs-lookup"><span data-stu-id="3e50d-155">This class represents your application in hello Azure AD B2C system.</span></span> <span data-ttu-id="3e50d-156">Když hello initalizes aplikace, vytvořte instanci `PublicClientApplication` v `MainWindow.xaml.cs`.</span><span class="sxs-lookup"><span data-stu-id="3e50d-156">When hello app initalizes, create an instance of `PublicClientApplication` in `MainWindow.xaml.cs`.</span></span> <span data-ttu-id="3e50d-157">To lze použít v celé okno hello.</span><span class="sxs-lookup"><span data-stu-id="3e50d-157">This can be used throughout hello window.</span></span>

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

### <a name="initiate-a-sign-up-flow"></a><span data-ttu-id="3e50d-158">Zahájení registrace toku</span><span class="sxs-lookup"><span data-stu-id="3e50d-158">Initiate a sign-up flow</span></span>
<span data-ttu-id="3e50d-159">Když uživatel požádá toosigns nahoru, budete chtít tooinitiate registrace toku, který používá hello registrační zásadě, kterou jste vytvořili.</span><span class="sxs-lookup"><span data-stu-id="3e50d-159">When a user opts toosigns up, you want tooinitiate a sign-up flow that uses hello sign-up policy you created.</span></span> <span data-ttu-id="3e50d-160">Pomocí MSAL jen zavoláte `pca.AcquireTokenAsync(...)`.</span><span class="sxs-lookup"><span data-stu-id="3e50d-160">By using MSAL, you just call `pca.AcquireTokenAsync(...)`.</span></span> <span data-ttu-id="3e50d-161">Hello parametry předáte příliš`AcquireTokenAsync(...)` určit, které token se zobrazí, hello zásady používané v žádosti o ověření hello a další.</span><span class="sxs-lookup"><span data-stu-id="3e50d-161">hello parameters you pass too`AcquireTokenAsync(...)` determine which token you receive, hello policy used in hello authentication request, and more.</span></span>

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

### <a name="initiate-a-sign-in-flow"></a><span data-ttu-id="3e50d-162">Zahájit toku přihlášení</span><span class="sxs-lookup"><span data-stu-id="3e50d-162">Initiate a sign-in flow</span></span>
<span data-ttu-id="3e50d-163">Můžete zahájit tok přihlášení jako hello stejným způsobem jako zahájení registrace toku.</span><span class="sxs-lookup"><span data-stu-id="3e50d-163">You can initiate a sign-in flow in hello same way that you initiate a sign-up flow.</span></span> <span data-ttu-id="3e50d-164">Když se uživatel přihlásí, ujistěte se, hello stejné volání tooMSAL, tentokrát pomocí přihlášení zásad:</span><span class="sxs-lookup"><span data-stu-id="3e50d-164">When a user signs in, make hello same call tooMSAL, this time by using your sign-in policy:</span></span>

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

### <a name="initiate-an-edit-profile-flow"></a><span data-ttu-id="3e50d-165">Zahájit tok úpravy profilu</span><span class="sxs-lookup"><span data-stu-id="3e50d-165">Initiate an edit-profile flow</span></span>
<span data-ttu-id="3e50d-166">Znovu, můžete zásadu upravit profil spustit v hello stejným způsobem:</span><span class="sxs-lookup"><span data-stu-id="3e50d-166">Again, you can execute an edit-profile policy in hello same fashion:</span></span>

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

<span data-ttu-id="3e50d-167">Ve všech těchto případech MSAL buď vrátí token v `AuthenticationResult` nebo vyvolá výjimku.</span><span class="sxs-lookup"><span data-stu-id="3e50d-167">In all of these cases, MSAL either returns a token in `AuthenticationResult` or throws an exception.</span></span> <span data-ttu-id="3e50d-168">Pokaždé, když získání tokenu z MSAL, můžete použít hello `AuthenticationResult.User` objektu tooupdate hello uživatelská data v hello aplikaci, například hello uživatelského rozhraní.</span><span class="sxs-lookup"><span data-stu-id="3e50d-168">Each time you get a token from MSAL, you can use hello `AuthenticationResult.User` object tooupdate hello user data in hello app, such as hello UI.</span></span> <span data-ttu-id="3e50d-169">ADAL také mezipamětí hello token pro použití v dalších částí aplikace hello.</span><span class="sxs-lookup"><span data-stu-id="3e50d-169">ADAL also caches hello token for use in other parts of hello application.</span></span>

### <a name="check-for-tokens-on-app-start"></a><span data-ttu-id="3e50d-170">Zkontrolujte pro tokeny při spuštění aplikace</span><span class="sxs-lookup"><span data-stu-id="3e50d-170">Check for tokens on app start</span></span>
<span data-ttu-id="3e50d-171">Můžete taky sledovat tookeep MSAL stavu přihlášení uživatele hello.</span><span class="sxs-lookup"><span data-stu-id="3e50d-171">You can also use MSAL tookeep track of hello user's sign-in state.</span></span>  <span data-ttu-id="3e50d-172">V této aplikaci chceme tooremain uživatele hello přihlášení i po jejich zavření aplikace hello & ho znovu otevřete.</span><span class="sxs-lookup"><span data-stu-id="3e50d-172">In this app, we want hello user tooremain signed in even after they close hello app & re-open it.</span></span>  <span data-ttu-id="3e50d-173">Zpět v hello `OnInitialized` přepsat, použijte na MSAL `AcquireTokenSilent` toocheck metoda pro tokeny, uložené v mezipaměti:</span><span class="sxs-lookup"><span data-stu-id="3e50d-173">Back inside hello `OnInitialized` override, use MSAL's `AcquireTokenSilent` method toocheck for cached tokens:</span></span>

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

## <a name="call-hello-task-api"></a><span data-ttu-id="3e50d-174">Volání rozhraní API hello úloh</span><span class="sxs-lookup"><span data-stu-id="3e50d-174">Call hello task API</span></span>
<span data-ttu-id="3e50d-175">Nyní použili MSAL tooexecute zásady a získat tokeny.</span><span class="sxs-lookup"><span data-stu-id="3e50d-175">You have now used MSAL tooexecute policies and get tokens.</span></span>  <span data-ttu-id="3e50d-176">Když chcete toouse jeden tyto tokeny toocall hello úloh API, můžete znovu použít na MSAL `AcquireTokenSilent` toocheck metoda pro tokeny, uložené v mezipaměti:</span><span class="sxs-lookup"><span data-stu-id="3e50d-176">When you want toouse one these tokens toocall hello task API, you can again use MSAL's `AcquireTokenSilent` method toocheck for cached tokens:</span></span>

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

<span data-ttu-id="3e50d-177">Při volání příliš hello`AcquireTokenSilentAsync(...)` úspěšné a nebude nalezen token hello mezipaměti, můžete přidat hello token toohello `Authorization` hlavičky požadavku hello HTTP.</span><span class="sxs-lookup"><span data-stu-id="3e50d-177">When hello call too`AcquireTokenSilentAsync(...)` succeeds and a token is found in hello cache, you can add hello token toohello `Authorization` header of hello HTTP request.</span></span> <span data-ttu-id="3e50d-178">Hello úloh webového rozhraní API bude používat toto záhlaví tooauthenticate hello požadavek tooread hello seznam úkolů uživatele:</span><span class="sxs-lookup"><span data-stu-id="3e50d-178">hello task web API will use this header tooauthenticate hello request tooread hello user's to-do list:</span></span>

```C#
    ...
    // Once hello token has been returned by MSAL, add it toohello http authorization header, before making hello call tooaccess hello tooDo list service.
    httpClient.DefaultRequestHeaders.Authorization = new AuthenticationHeaderValue("Bearer", result.Token);

    // Call hello tooDo list service.
    HttpResponseMessage response = await httpClient.GetAsync(Globals.taskServiceUrl + "/api/tasks");
    ...
```

## <a name="sign-hello-user-out"></a><span data-ttu-id="3e50d-179">Přihlášení uživatele hello out</span><span class="sxs-lookup"><span data-stu-id="3e50d-179">Sign hello user out</span></span>
<span data-ttu-id="3e50d-180">Nakonec můžete použít MSAL tooend relace uživatele s aplikací hello při hello uživatel vybere **Odhlásit se**.  Při použití MSAL, toho dosahuje tím, že zrušíte všechny tokeny hello z tokenu mezipaměti hello:</span><span class="sxs-lookup"><span data-stu-id="3e50d-180">Finally, you can use MSAL tooend a user's session with hello app when hello user selects **Sign out**.  When using MSAL, this is accomplished by clearing all of hello tokens from hello token cache:</span></span>

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

## <a name="run-hello-sample-app"></a><span data-ttu-id="3e50d-181">Spuštění ukázkové aplikace hello</span><span class="sxs-lookup"><span data-stu-id="3e50d-181">Run hello sample app</span></span>
<span data-ttu-id="3e50d-182">Nakonec sestavte a spusťte ukázkové hello.</span><span class="sxs-lookup"><span data-stu-id="3e50d-182">Finally, build and run hello sample.</span></span>  <span data-ttu-id="3e50d-183">Zaregistrujte se pro aplikaci hello pomocí e-mailové adresy nebo uživatelské jméno.</span><span class="sxs-lookup"><span data-stu-id="3e50d-183">Sign up for hello app by using an email address or user name.</span></span> <span data-ttu-id="3e50d-184">Odhlaste se a přihlaste se znovu jako hello stejné uživatele.</span><span class="sxs-lookup"><span data-stu-id="3e50d-184">Sign out and sign back in as hello same user.</span></span> <span data-ttu-id="3e50d-185">Upravte profil uživatele.</span><span class="sxs-lookup"><span data-stu-id="3e50d-185">Edit that user's profile.</span></span> <span data-ttu-id="3e50d-186">Odhlásit se a přihlásit pomocí jiného uživatele.</span><span class="sxs-lookup"><span data-stu-id="3e50d-186">Sign out and sign up by using a different user.</span></span>

## <a name="add-social-idps"></a><span data-ttu-id="3e50d-187">Přidat sociálních IDPs</span><span class="sxs-lookup"><span data-stu-id="3e50d-187">Add social IDPs</span></span>
<span data-ttu-id="3e50d-188">V současné době hello aplikace podporuje pouze uživatele registrace a přihlášení, použít **místní účty**.</span><span class="sxs-lookup"><span data-stu-id="3e50d-188">Currently, hello app supports only user sign-up and sign-in that use **local accounts**.</span></span> <span data-ttu-id="3e50d-189">Toto jsou účty uložené v adresáři B2C, které používají uživatelské jméno a heslo.</span><span class="sxs-lookup"><span data-stu-id="3e50d-189">These are accounts stored in your B2C directory that use a user name and password.</span></span> <span data-ttu-id="3e50d-190">Pomocí Azure AD B2C můžete přidat podporu pro jiných poskytovatelů identit (IDPs) beze změny některé z vašeho kódu.</span><span class="sxs-lookup"><span data-stu-id="3e50d-190">By using Azure AD B2C, you can add support for other identity providers (IDPs) without changing any of your code.</span></span>

<span data-ttu-id="3e50d-191">tooadd sociálních IDPs tooyour aplikace, začněte tím, že následující hello podrobné pokyny v těchto článcích.</span><span class="sxs-lookup"><span data-stu-id="3e50d-191">tooadd social IDPs tooyour app, begin by following hello detailed instructions in these articles.</span></span> <span data-ttu-id="3e50d-192">Pro každé rozšíření IDP chcete toosupport, potřebujete tooregister aplikace v daném systému a získat ID klienta.</span><span class="sxs-lookup"><span data-stu-id="3e50d-192">For each IDP you want toosupport, you need tooregister an application in that system and obtain a client ID.</span></span>

* [<span data-ttu-id="3e50d-193">Nastavení sítě Facebook jako IDP</span><span class="sxs-lookup"><span data-stu-id="3e50d-193">Set up Facebook as an IDP</span></span>](active-directory-b2c-setup-fb-app.md)
* [<span data-ttu-id="3e50d-194">Nastavit Google jako IDP</span><span class="sxs-lookup"><span data-stu-id="3e50d-194">Set up Google as an IDP</span></span>](active-directory-b2c-setup-goog-app.md)
* [<span data-ttu-id="3e50d-195">Nastavit Amazon jako IDP</span><span class="sxs-lookup"><span data-stu-id="3e50d-195">Set up Amazon as an IDP</span></span>](active-directory-b2c-setup-amzn-app.md)
* [<span data-ttu-id="3e50d-196">Nastavit LinkedIn jako IDP</span><span class="sxs-lookup"><span data-stu-id="3e50d-196">Set up LinkedIn as an IDP</span></span>](active-directory-b2c-setup-li-app.md)

<span data-ttu-id="3e50d-197">Po přidání directory tooyour B2C zprostředkovatelé identity hello potřebujete tooedit každé tři zásady tooinclude hello nové IDPs jako popsané v hello [článku o zásadách](active-directory-b2c-reference-policies.md).</span><span class="sxs-lookup"><span data-stu-id="3e50d-197">After you add hello identity providers tooyour B2C directory, you need tooedit each of your three policies tooinclude hello new IDPs, as described in hello [policy reference article](active-directory-b2c-reference-policies.md).</span></span> <span data-ttu-id="3e50d-198">Po uložení zásad hello a znovu spusťte aplikaci.</span><span class="sxs-lookup"><span data-stu-id="3e50d-198">After you save your policies, run hello app again.</span></span> <span data-ttu-id="3e50d-199">Měli byste vidět hello přidají nové IDPs jako přihlášení a odhlášení možnosti v každé z vaší činnosti identity.</span><span class="sxs-lookup"><span data-stu-id="3e50d-199">You should see hello new IDPs added as sign-in and sign-up options in each of your identity experiences.</span></span>

<span data-ttu-id="3e50d-200">Můžete experimentovat s vašimi zásadami a sledovat účinky hello na ukázkové aplikace.</span><span class="sxs-lookup"><span data-stu-id="3e50d-200">You can experiment with your policies and observe hello effects on your sample app.</span></span> <span data-ttu-id="3e50d-201">Přidat nebo odebrat IDPs, pracovat s deklarace identity aplikace nebo změnit atributy registrace.</span><span class="sxs-lookup"><span data-stu-id="3e50d-201">Add or remove IDPs, manipulate application claims, or change sign-up attributes.</span></span> <span data-ttu-id="3e50d-202">Experiment, dokud se nezobrazí, jak zásady, žádosti o ověření a MSAL tie společně.</span><span class="sxs-lookup"><span data-stu-id="3e50d-202">Experiment until you can see how policies, authentication requests, and MSAL tie together.</span></span>

<span data-ttu-id="3e50d-203">Pro referenci hello dokončit ukázka [je k dispozici jako soubor ZIP](https://github.com/AzureADQuickStarts/B2C-NativeClient-DotNet/archive/complete.zip).</span><span class="sxs-lookup"><span data-stu-id="3e50d-203">For reference, hello completed sample [is provided as a .zip file](https://github.com/AzureADQuickStarts/B2C-NativeClient-DotNet/archive/complete.zip).</span></span> <span data-ttu-id="3e50d-204">Můžete ho také klonovat z GitHubu:</span><span class="sxs-lookup"><span data-stu-id="3e50d-204">You can also clone it from GitHub:</span></span>

```git clone --branch complete https://github.com/AzureADQuickStarts/B2C-NativeClient-DotNet.git```
