---
title: "Přidání ověřování do aplikace pro univerzální platformu Windows (UWP) | Microsoft Docs"
description: "Naučte se používat Azure App Service Mobile Apps ověřovat uživatele vaší aplikace univerzální platformu Windows (UWP) pomocí různých zprostředkovatelů identity, včetně: AAD, Google, Facebook, Twitter a společnosti Microsoft."
services: app-service\mobile
documentationcenter: windows
author: ggailey777
manager: panarasi
editor: 
ms.assetid: 6cffd951-893e-4ce5-97ac-86e3f5ad9466
ms.service: app-service-mobile
ms.workload: mobile
ms.tgt_pltfrm: mobile-windows
ms.devlang: dotnet
ms.topic: article
ms.date: 07/05/2017
ms.author: panarasi
ms.openlocfilehash: 47da343d4ec956ec2e669757f56e853675f887a3
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="add-authentication-to-your-windows-app"></a><span data-ttu-id="1ebc9-103">Přidání ověřování do aplikace pro Windows</span><span class="sxs-lookup"><span data-stu-id="1ebc9-103">Add authentication to your Windows app</span></span>
[!INCLUDE [app-service-mobile-selector-get-started-users](../../includes/app-service-mobile-selector-get-started-users.md)]

<span data-ttu-id="1ebc9-104">Toto téma ukazuje, jak přidat cloudové ověřování do mobilní aplikace.</span><span class="sxs-lookup"><span data-stu-id="1ebc9-104">This topic shows you how to add cloud-based authentication to your mobile app.</span></span> <span data-ttu-id="1ebc9-105">V tomto kurzu jste přidání ověřování do projektu pro rychlý start univerzální platformu Windows (UWP) pro mobilní aplikace pomocí zprostředkovatele identity, která je podporována službou Azure App Service.</span><span class="sxs-lookup"><span data-stu-id="1ebc9-105">In this tutorial, you add authentication to the Universal Windows Platform (UWP) quickstart project for Mobile Apps using an identity provider that is supported by Azure App Service.</span></span> <span data-ttu-id="1ebc9-106">Po se úspěšně ověří a autorizuje váš back-end mobilní aplikace, se zobrazí hodnota ID uživatele.</span><span class="sxs-lookup"><span data-stu-id="1ebc9-106">After being successfully authenticated and authorized by your Mobile App backend, the user ID value is displayed.</span></span>

<span data-ttu-id="1ebc9-107">V tomto kurzu vychází z rychlého startu mobilní aplikace.</span><span class="sxs-lookup"><span data-stu-id="1ebc9-107">This tutorial is based on the Mobile Apps quickstart.</span></span> <span data-ttu-id="1ebc9-108">Musíte nejdřív dokončit tento kurz [Začínáme s Mobile Apps](app-service-mobile-windows-store-dotnet-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="1ebc9-108">You must first complete the tutorial [Get started with Mobile Apps](app-service-mobile-windows-store-dotnet-get-started.md).</span></span>

## <span data-ttu-id="1ebc9-109"><a name="register"></a>Registrace aplikace pro ověřování a nakonfigurovat App Service</span><span class="sxs-lookup"><span data-stu-id="1ebc9-109"><a name="register"></a>Register your app for authentication and configure the App Service</span></span>
[!INCLUDE [app-service-mobile-register-authentication](../../includes/app-service-mobile-register-authentication.md)]

## <span data-ttu-id="1ebc9-110"><a name="redirecturl"></a>Přidání aplikace do adresy URL pro povolené externí přesměrování</span><span class="sxs-lookup"><span data-stu-id="1ebc9-110"><a name="redirecturl"></a>Add your app to the Allowed External Redirect URLs</span></span>

<span data-ttu-id="1ebc9-111">Zabezpečené ověřování vyžaduje, můžete definovat nové schéma adresy URL pro vaši aplikaci.</span><span class="sxs-lookup"><span data-stu-id="1ebc9-111">Secure authentication requires that you define a new URL scheme for your app.</span></span> <span data-ttu-id="1ebc9-112">To umožňuje ověřování systému přesměrovat zpět do aplikace po dokončení procesu ověřování.</span><span class="sxs-lookup"><span data-stu-id="1ebc9-112">This allows the authentication system to redirect back to your app once the authentication process is complete.</span></span> <span data-ttu-id="1ebc9-113">V tomto kurzu používáme schématu adresy URL _appname_ v průběhu.</span><span class="sxs-lookup"><span data-stu-id="1ebc9-113">In this tutorial, we use the URL scheme _appname_ throughout.</span></span> <span data-ttu-id="1ebc9-114">Můžete však použít žádné schéma adresy URL, které zvolíte.</span><span class="sxs-lookup"><span data-stu-id="1ebc9-114">However, you can use any URL scheme you choose.</span></span> <span data-ttu-id="1ebc9-115">Musí být jedinečné pro mobilní aplikace.</span><span class="sxs-lookup"><span data-stu-id="1ebc9-115">It should be unique to your mobile application.</span></span> <span data-ttu-id="1ebc9-116">Chcete povolit přesměrování na straně serveru:</span><span class="sxs-lookup"><span data-stu-id="1ebc9-116">To enable the redirection on the server side:</span></span>

1. <span data-ttu-id="1ebc9-117">V [portál Azure] vyberte App Service.</span><span class="sxs-lookup"><span data-stu-id="1ebc9-117">In the [Azure portal], select your App Service.</span></span>

2. <span data-ttu-id="1ebc9-118">Klikněte **ověřování / autorizace** možnost nabídky.</span><span class="sxs-lookup"><span data-stu-id="1ebc9-118">Click the **Authentication / Authorization** menu option.</span></span>

3. <span data-ttu-id="1ebc9-119">V **povoleno externí adres URL pro přesměrování**, zadejte `url_scheme_of_your_app://easyauth.callback`.</span><span class="sxs-lookup"><span data-stu-id="1ebc9-119">In the **Allowed External Redirect URLs**, enter `url_scheme_of_your_app://easyauth.callback`.</span></span>  <span data-ttu-id="1ebc9-120">**Url_scheme_of_your_app** v tento řetězec je schéma adresy URL pro mobilní aplikace.</span><span class="sxs-lookup"><span data-stu-id="1ebc9-120">The **url_scheme_of_your_app** in this string is the URL Scheme for your mobile application.</span></span>  <span data-ttu-id="1ebc9-121">Měl by splňovat specifikaci normální adresu URL pro určitý protokol (používejte písmena a čísla pouze a začněte s písmenem).</span><span class="sxs-lookup"><span data-stu-id="1ebc9-121">It should follow normal URL specification for a protocol (use letters and numbers only, and start with a letter).</span></span>  <span data-ttu-id="1ebc9-122">Měli byste si poznamenat řetězce, který zvolíte, jako je třeba upravit kód mobilní aplikace s schéma adresy URL na několika místech.</span><span class="sxs-lookup"><span data-stu-id="1ebc9-122">You should make a note of the string that you choose as you will need to adjust your mobile application code with the URL Scheme in several places.</span></span>

4. <span data-ttu-id="1ebc9-123">Klikněte na **OK**.</span><span class="sxs-lookup"><span data-stu-id="1ebc9-123">Click **OK**.</span></span>

5. <span data-ttu-id="1ebc9-124">Klikněte na **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="1ebc9-124">Click **Save**.</span></span>

## <span data-ttu-id="1ebc9-125"><a name="permissions"></a>Omezit oprávnění k ověření uživatelé</span><span class="sxs-lookup"><span data-stu-id="1ebc9-125"><a name="permissions"></a>Restrict permissions to authenticated users</span></span>
[!INCLUDE [app-service-mobile-restrict-permissions-dotnet-backend](../../includes/app-service-mobile-restrict-permissions-dotnet-backend.md)]

<span data-ttu-id="1ebc9-126">Nyní můžete ověřit, že byl zakázán anonymní přístup k vaší back-end.</span><span class="sxs-lookup"><span data-stu-id="1ebc9-126">Now, you can verify that anonymous access to your backend has been disabled.</span></span> <span data-ttu-id="1ebc9-127">S projekt aplikace UPW nastavit jako projekt spuštění, nasazení a spuštění aplikace; Ověřte, že k neošetřené výjimce s stavový kód 401 (Neautorizováno) se vyvolá po spuštění aplikace.</span><span class="sxs-lookup"><span data-stu-id="1ebc9-127">With the UWP app project set as the start-up project, deploy and run the app; verify that an unhandled exception with a status code of 401 (Unauthorized) is raised after the app starts.</span></span> <span data-ttu-id="1ebc9-128">K tomu dojde, protože se aplikace pokusí o přístup k kódu mobilní aplikace jako neověřený uživatel, ale *TodoItem* tabulka nyní vyžaduje ověření.</span><span class="sxs-lookup"><span data-stu-id="1ebc9-128">This happens because the app attempts to access your Mobile App Code as an unauthenticated user, but the *TodoItem* table now requires authentication.</span></span>

<span data-ttu-id="1ebc9-129">Potom aktualizujte aplikace k ověření uživatelů před vyžádáním prostředky z vaší služby App Service.</span><span class="sxs-lookup"><span data-stu-id="1ebc9-129">Next, you will update the app to authenticate users before requesting resources from your App Service.</span></span>

## <span data-ttu-id="1ebc9-130"><a name="add-authentication"></a>Přidání ověřování do aplikace</span><span class="sxs-lookup"><span data-stu-id="1ebc9-130"><a name="add-authentication"></a>Add authentication to the app</span></span>
1. <span data-ttu-id="1ebc9-131">Aplikace UWP souboru MainPage.xaml.cs projektu a přidejte následující fragment kódu:</span><span class="sxs-lookup"><span data-stu-id="1ebc9-131">In the UWP app project file MainPage.xaml.cs and add the following code snippet:</span></span>
   
        // Define a member variable for storing the signed-in user. 
        private MobileServiceUser user;
   
        // Define a method that performs the authentication process
        // using a Facebook sign-in. 
        private async System.Threading.Tasks.Task<bool> AuthenticateAsync()
        {
            string message;
            bool success = false;
            try
            {
                // Change 'MobileService' to the name of your MobileServiceClient instance.
                // Sign-in using Facebook authentication.
                user = await App.MobileService
                    .LoginAsync(MobileServiceAuthenticationProvider.Facebook, "{url_scheme_of_your_app}");
                message =
                    string.Format("You are now signed in - {0}", user.UserId);
   
                success = true;
            }
            catch (InvalidOperationException)
            {
                message = "You must log in. Login Required";
            }
   
            var dialog = new MessageDialog(message);
            dialog.Commands.Add(new UICommand("OK"));
            await dialog.ShowAsync();
            return success;
        }
   
    <span data-ttu-id="1ebc9-132">Tento kód ověřuje uživatele s přihlašovacími údaji, Facebook.</span><span class="sxs-lookup"><span data-stu-id="1ebc9-132">This code authenticates the user with a Facebook login.</span></span> <span data-ttu-id="1ebc9-133">Pokud používáte zprostředkovatele identity než Facebook, změňte hodnotu **MobileServiceAuthenticationProvider** výše na hodnotu pro poskytovatele.</span><span class="sxs-lookup"><span data-stu-id="1ebc9-133">If you are using an identity provider other than Facebook, change the value of **MobileServiceAuthenticationProvider** above to the value for your provider.</span></span>
2. <span data-ttu-id="1ebc9-134">Nahraďte **OnNavigatedTo()** metoda v MainPage.xaml.cs.</span><span class="sxs-lookup"><span data-stu-id="1ebc9-134">Replace the **OnNavigatedTo()** method in MainPage.xaml.cs.</span></span> <span data-ttu-id="1ebc9-135">V dalším kroku přidáte **přihlášení** tlačítko na aplikaci, která aktivuje ověřování.</span><span class="sxs-lookup"><span data-stu-id="1ebc9-135">Next, you will add a **Sign in** button to the app that triggers authentication.</span></span>

        protected override async void OnNavigatedTo(NavigationEventArgs e)
        {
            if (e.Parameter is Uri)
            {
                App.MobileService.ResumeWithURL(e.Parameter as Uri);
            }
        }

3. <span data-ttu-id="1ebc9-136">MainPage.xaml.cs přidejte následující fragment kódu:</span><span class="sxs-lookup"><span data-stu-id="1ebc9-136">Add the following code snippet to the MainPage.xaml.cs:</span></span>
   
        private async void ButtonLogin_Click(object sender, RoutedEventArgs e)
        {
            // Login the user and then load data from the mobile app.
            if (await AuthenticateAsync())
            {
                // Switch the buttons and load items from the mobile app.
                ButtonLogin.Visibility = Visibility.Collapsed;
                ButtonSave.Visibility = Visibility.Visible;
                //await InitLocalStoreAsync(); //offline sync support.
                await RefreshTodoItems();
            }
        }
4. <span data-ttu-id="1ebc9-137">Otevřete soubor projektu MainPage.xaml, vyhledejte elementu, který definuje **Uložit** tlačítko a nahraďte ji následujícím kódem:</span><span class="sxs-lookup"><span data-stu-id="1ebc9-137">Open the MainPage.xaml project file, locate the element that defines the **Save** button and replace it with the following code:</span></span>
   
        <Button Name="ButtonSave" Visibility="Collapsed" Margin="0,8,8,0" 
                Click="ButtonSave_Click">
            <StackPanel Orientation="Horizontal">
                <SymbolIcon Symbol="Add"/>
                <TextBlock Margin="5">Save</TextBlock>
            </StackPanel>
        </Button>
        <Button Name="ButtonLogin" Visibility="Visible" Margin="0,8,8,0" 
                Click="ButtonLogin_Click" TabIndex="0">
            <StackPanel Orientation="Horizontal">
                <SymbolIcon Symbol="Permissions"/>
                <TextBlock Margin="5">Sign in</TextBlock> 
            </StackPanel>
        </Button>
5. <span data-ttu-id="1ebc9-138">Souboru App.xaml.cs přidejte následující fragment kódu:</span><span class="sxs-lookup"><span data-stu-id="1ebc9-138">Add the following code snippet to the App.xaml.cs:</span></span>

        protected override void OnActivated(IActivatedEventArgs args)
        {
            if (args.Kind == ActivationKind.Protocol)
            {
                ProtocolActivatedEventArgs protocolArgs = args as ProtocolActivatedEventArgs;
                Frame content = Window.Current.Content as Frame;
                if (content.Content.GetType() == typeof(MainPage))
                {
                    content.Navigate(typeof(MainPage), protocolArgs.Uri);
                }
            }
            Window.Current.Activate();
            base.OnActivated(args);
        }
6. <span data-ttu-id="1ebc9-139">Otevřete soubor Package.appxmanifest, přejděte na **deklarace**v **dostupné deklarace** rozevíracího seznamu vyberte **protokol** a klikněte na tlačítko **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="1ebc9-139">Open Package.appxmanifest file, navigate to **Declarations**, in **Available Declarations** dropdown list, select **Protocol** and click **Add** button.</span></span> <span data-ttu-id="1ebc9-140">Teď nakonfigurovat **vlastnosti** z **protokol** deklarace.</span><span class="sxs-lookup"><span data-stu-id="1ebc9-140">Now configure the **Properties** of the **Protocol** declaration.</span></span> <span data-ttu-id="1ebc9-141">V **zobrazovaný název**, přidejte název si přejete zobrazit uživatelům vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="1ebc9-141">In **Display name**, add the name you wish to display to users of your application.</span></span> <span data-ttu-id="1ebc9-142">V **název**, přidejte vaše {url_scheme_of_your_app}.</span><span class="sxs-lookup"><span data-stu-id="1ebc9-142">In **Name**, add your {url_scheme_of_your_app}.</span></span>
7. <span data-ttu-id="1ebc9-143">Stisknutím klávesy F5 spusťte aplikaci, klikněte na tlačítko **přihlášení** tlačítko a přihlaste se k aplikaci pomocí zprostředkovatele identity vybrané.</span><span class="sxs-lookup"><span data-stu-id="1ebc9-143">Press the F5 key to run the app, click the **Sign in** button, and sign into the app with your chosen identity provider.</span></span> <span data-ttu-id="1ebc9-144">Po přihlášení s úspěšné aplikace běží bez chyb a je možné provést aktualizace dat a dotaz na váš back-end.</span><span class="sxs-lookup"><span data-stu-id="1ebc9-144">After your sign-in is successful, the app runs without errors and you are able to query your backend and make updates to data.</span></span>

## <span data-ttu-id="1ebc9-145"><a name="tokens"></a>Ukládání tokenu ověřování na straně klienta</span><span class="sxs-lookup"><span data-stu-id="1ebc9-145"><a name="tokens"></a>Store the authentication token on the client</span></span>
<span data-ttu-id="1ebc9-146">Předchozí příklad ukázal standardní přihlášení, která vyžaduje, aby klient kontaktovat poskytovatele identit a App Service při každém spuštění aplikace.</span><span class="sxs-lookup"><span data-stu-id="1ebc9-146">The previous example showed a standard sign-in, which requires the client to contact both the identity provider and the App Service every time that the app starts.</span></span> <span data-ttu-id="1ebc9-147">Ne jenom je neefektivní, můžete spustit tuto metodu do využití-má vztah problémy mnoho zákazníků využít při spuštění aplikace ve stejnou dobu.</span><span class="sxs-lookup"><span data-stu-id="1ebc9-147">Not only is this method inefficient, you can run into usage-relates issues should many customers try to start you app at the same time.</span></span> <span data-ttu-id="1ebc9-148">Lepším řešením je ukládat do mezipaměti autorizační token vrácený App Service a pokuste se použít tento první před použitím u založenou na poskytovateli přihlášení.</span><span class="sxs-lookup"><span data-stu-id="1ebc9-148">A better approach is to cache the authorization token returned by your App Service and try to use this first before using a provider-based sign-in.</span></span>

> [!NOTE]
> <span data-ttu-id="1ebc9-149">Můžete mezipaměti tokenem vydaným službou App Services bez ohledu na to, jestli používáte ověřování klienta spravovat nebo spravované služby.</span><span class="sxs-lookup"><span data-stu-id="1ebc9-149">You can cache the token issued by App Services regardless of whether you are using client-managed or service-managed authentication.</span></span> <span data-ttu-id="1ebc9-150">Tento kurz používá službu spravovat ověřování.</span><span class="sxs-lookup"><span data-stu-id="1ebc9-150">This tutorial uses service-managed authentication.</span></span>
> 
> 

[!INCLUDE [mobile-windows-universal-dotnet-authenticate-app-with-token](../../includes/mobile-windows-universal-dotnet-authenticate-app-with-token.md)]

## <a name="next-steps"></a><span data-ttu-id="1ebc9-151">Další kroky</span><span class="sxs-lookup"><span data-stu-id="1ebc9-151">Next steps</span></span>
<span data-ttu-id="1ebc9-152">Teď, když jste dokončili kurz základní ověřování, vezměte v úvahu pokračovat na jednu z následujících kurzů:</span><span class="sxs-lookup"><span data-stu-id="1ebc9-152">Now that you completed this basic authentication tutorial, consider continuing on to one of the following tutorials:</span></span>

* [<span data-ttu-id="1ebc9-153">Přidání nabízených oznámení do aplikace</span><span class="sxs-lookup"><span data-stu-id="1ebc9-153">Add push notifications to your app</span></span>](app-service-mobile-windows-store-dotnet-get-started-push.md)  
  <span data-ttu-id="1ebc9-154">Naučte se přidávat do aplikace podporu nabízených oznámení a konfigurovat back-end mobilní aplikace tak, aby k zasílání nabízených oznámení používal Azure Notification Hubs.</span><span class="sxs-lookup"><span data-stu-id="1ebc9-154">Learn how to add push notifications support to your app and configure your Mobile App backend to use Azure Notification Hubs to send push notifications.</span></span>
* [<span data-ttu-id="1ebc9-155">Povolení offline synchronizace u aplikace</span><span class="sxs-lookup"><span data-stu-id="1ebc9-155">Enable offline sync for your app</span></span>](app-service-mobile-windows-store-dotnet-get-started-offline-data.md)  
  <span data-ttu-id="1ebc9-156">Naučte se, jak pomocí back-endu mobilní aplikace přidat do aplikace podporu offline režimu.</span><span class="sxs-lookup"><span data-stu-id="1ebc9-156">Learn how to add offline support your app using an Mobile App backend.</span></span> <span data-ttu-id="1ebc9-157">Offline synchronizace umožňuje koncovým uživatelům pracovat s mobilní aplikací &mdash; zobrazovat, přidávat a upravovat data &mdash; i v případě, že nemají připojení k síti.</span><span class="sxs-lookup"><span data-stu-id="1ebc9-157">Offline sync allows end-users to interact with a mobile app&mdash;viewing, adding, or modifying data&mdash;even when there is no network connection.</span></span>

<!-- URLs. -->
[Get started with your mobile app]: app-service-mobile-windows-store-dotnet-get-started.md
