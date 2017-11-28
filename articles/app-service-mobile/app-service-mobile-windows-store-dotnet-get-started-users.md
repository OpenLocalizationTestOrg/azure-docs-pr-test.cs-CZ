---
title: "aplikace aaaAdd authentication tooyour univerzální platformu Windows (UWP) | Microsoft Docs"
description: "Zjistěte, jak toouse Azure App Service Mobile Apps tooauthenticate uživatele vaší aplikace univerzální platformu Windows (UWP) pomocí různých zprostředkovatelů identity, včetně: AAD, Google, Facebook, Twitter a společnosti Microsoft."
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
ms.openlocfilehash: ad4477e9509f1c40c33e71818e268f6857fe1e80
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="add-authentication-tooyour-windows-app"></a><span data-ttu-id="0ade3-103">Přidat aplikace pro Windows tooyour ověřování</span><span class="sxs-lookup"><span data-stu-id="0ade3-103">Add authentication tooyour Windows app</span></span>
[!INCLUDE [app-service-mobile-selector-get-started-users](../../includes/app-service-mobile-selector-get-started-users.md)]

<span data-ttu-id="0ade3-104">Toto téma ukazuje, jak tooadd cloudové ověřování tooyour mobilní aplikace.</span><span class="sxs-lookup"><span data-stu-id="0ade3-104">This topic shows you how tooadd cloud-based authentication tooyour mobile app.</span></span> <span data-ttu-id="0ade3-105">V tomto kurzu přidáte ověřování toohello univerzální platformu Windows (UWP) rychlé spuštění projektu pro mobilní aplikace pomocí zprostředkovatele identity, která je podporována službou Azure App Service.</span><span class="sxs-lookup"><span data-stu-id="0ade3-105">In this tutorial, you add authentication toohello Universal Windows Platform (UWP) quickstart project for Mobile Apps using an identity provider that is supported by Azure App Service.</span></span> <span data-ttu-id="0ade3-106">Po se úspěšně ověří a autorizuje váš back-end mobilní aplikace, zobrazí se hodnota ID uživatele hello.</span><span class="sxs-lookup"><span data-stu-id="0ade3-106">After being successfully authenticated and authorized by your Mobile App backend, hello user ID value is displayed.</span></span>

<span data-ttu-id="0ade3-107">V tomto kurzu vychází z mobilní aplikace hello rychlý start.</span><span class="sxs-lookup"><span data-stu-id="0ade3-107">This tutorial is based on hello Mobile Apps quickstart.</span></span> <span data-ttu-id="0ade3-108">Je třeba nejprve provést hello kurzu [Začínáme s Mobile Apps](app-service-mobile-windows-store-dotnet-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="0ade3-108">You must first complete hello tutorial [Get started with Mobile Apps](app-service-mobile-windows-store-dotnet-get-started.md).</span></span>

## <span data-ttu-id="0ade3-109"><a name="register"></a>Registrace aplikace pro ověřování a nakonfigurujte hello služby App Service</span><span class="sxs-lookup"><span data-stu-id="0ade3-109"><a name="register"></a>Register your app for authentication and configure hello App Service</span></span>
[!INCLUDE [app-service-mobile-register-authentication](../../includes/app-service-mobile-register-authentication.md)]

## <span data-ttu-id="0ade3-110"><a name="redirecturl"></a>Přidání adresy URL pro vaše aplikace toohello povoleno externí přesměrování</span><span class="sxs-lookup"><span data-stu-id="0ade3-110"><a name="redirecturl"></a>Add your app toohello Allowed External Redirect URLs</span></span>

<span data-ttu-id="0ade3-111">Zabezpečené ověřování vyžaduje, můžete definovat nové schéma adresy URL pro vaši aplikaci.</span><span class="sxs-lookup"><span data-stu-id="0ade3-111">Secure authentication requires that you define a new URL scheme for your app.</span></span> <span data-ttu-id="0ade3-112">To umožňuje hello ověřování systému tooredirect back tooyour aplikaci po dokončení procesu ověřování hello.</span><span class="sxs-lookup"><span data-stu-id="0ade3-112">This allows hello authentication system tooredirect back tooyour app once hello authentication process is complete.</span></span> <span data-ttu-id="0ade3-113">V tomto kurzu používáme schéma adresy URL hello _appname_ v průběhu.</span><span class="sxs-lookup"><span data-stu-id="0ade3-113">In this tutorial, we use hello URL scheme _appname_ throughout.</span></span> <span data-ttu-id="0ade3-114">Můžete však použít žádné schéma adresy URL, které zvolíte.</span><span class="sxs-lookup"><span data-stu-id="0ade3-114">However, you can use any URL scheme you choose.</span></span> <span data-ttu-id="0ade3-115">Mělo by být jedinečný tooyour mobilních aplikací.</span><span class="sxs-lookup"><span data-stu-id="0ade3-115">It should be unique tooyour mobile application.</span></span> <span data-ttu-id="0ade3-116">tooenable hello přesměrování na straně serveru hello:</span><span class="sxs-lookup"><span data-stu-id="0ade3-116">tooenable hello redirection on hello server side:</span></span>

1. <span data-ttu-id="0ade3-117">V [portál Azure] hello vyberte App Service.</span><span class="sxs-lookup"><span data-stu-id="0ade3-117">In hello [Azure portal], select your App Service.</span></span>

2. <span data-ttu-id="0ade3-118">Klikněte na tlačítko hello **ověřování / autorizace** možnost nabídky.</span><span class="sxs-lookup"><span data-stu-id="0ade3-118">Click hello **Authentication / Authorization** menu option.</span></span>

3. <span data-ttu-id="0ade3-119">V hello **povoleno externí adres URL pro přesměrování**, zadejte `url_scheme_of_your_app://easyauth.callback`.</span><span class="sxs-lookup"><span data-stu-id="0ade3-119">In hello **Allowed External Redirect URLs**, enter `url_scheme_of_your_app://easyauth.callback`.</span></span>  <span data-ttu-id="0ade3-120">Hello **url_scheme_of_your_app** v tento řetězec je hello schéma adresy URL pro mobilní aplikace.</span><span class="sxs-lookup"><span data-stu-id="0ade3-120">hello **url_scheme_of_your_app** in this string is hello URL Scheme for your mobile application.</span></span>  <span data-ttu-id="0ade3-121">Měl by splňovat specifikaci normální adresu URL pro určitý protokol (používejte písmena a čísla pouze a začněte s písmenem).</span><span class="sxs-lookup"><span data-stu-id="0ade3-121">It should follow normal URL specification for a protocol (use letters and numbers only, and start with a letter).</span></span>  <span data-ttu-id="0ade3-122">Měli byste si poznamenat hello řetězce, který zvolíte, bude nutné tooadjust kód mobilní aplikace s hello schéma adresy URL na několika místech.</span><span class="sxs-lookup"><span data-stu-id="0ade3-122">You should make a note of hello string that you choose as you will need tooadjust your mobile application code with hello URL Scheme in several places.</span></span>

4. <span data-ttu-id="0ade3-123">Klikněte na **OK**.</span><span class="sxs-lookup"><span data-stu-id="0ade3-123">Click **OK**.</span></span>

5. <span data-ttu-id="0ade3-124">Klikněte na **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="0ade3-124">Click **Save**.</span></span>

## <span data-ttu-id="0ade3-125"><a name="permissions"></a>Omezte oprávnění tooauthenticated</span><span class="sxs-lookup"><span data-stu-id="0ade3-125"><a name="permissions"></a>Restrict permissions tooauthenticated users</span></span>
[!INCLUDE [app-service-mobile-restrict-permissions-dotnet-backend](../../includes/app-service-mobile-restrict-permissions-dotnet-backend.md)]

<span data-ttu-id="0ade3-126">Nyní můžete ověřit, že back-end tooyour anonymní přístup je zakázané.</span><span class="sxs-lookup"><span data-stu-id="0ade3-126">Now, you can verify that anonymous access tooyour backend has been disabled.</span></span> <span data-ttu-id="0ade3-127">S projekt aplikace UPW hello nastavit jako hello spuštění projekt nasazení a spuštění aplikace hello; Ověřte, že se po spuštění aplikace hello vyvolá k neošetřené výjimce s stavový kód 401 (Neautorizováno).</span><span class="sxs-lookup"><span data-stu-id="0ade3-127">With hello UWP app project set as hello start-up project, deploy and run hello app; verify that an unhandled exception with a status code of 401 (Unauthorized) is raised after hello app starts.</span></span> <span data-ttu-id="0ade3-128">K tomu dojde, protože aplikace hello pokusí tooaccess kódu mobilní aplikace jako neověřený uživatel, ale hello *TodoItem* tabulka nyní vyžaduje ověření.</span><span class="sxs-lookup"><span data-stu-id="0ade3-128">This happens because hello app attempts tooaccess your Mobile App Code as an unauthenticated user, but hello *TodoItem* table now requires authentication.</span></span>

<span data-ttu-id="0ade3-129">Potom aktualizujte uživatele tooauthenticate aplikace hello před vyžádáním prostředky z vaší služby App Service.</span><span class="sxs-lookup"><span data-stu-id="0ade3-129">Next, you will update hello app tooauthenticate users before requesting resources from your App Service.</span></span>

## <span data-ttu-id="0ade3-130"><a name="add-authentication"></a>Přidat aplikaci toohello ověřování</span><span class="sxs-lookup"><span data-stu-id="0ade3-130"><a name="add-authentication"></a>Add authentication toohello app</span></span>
1. <span data-ttu-id="0ade3-131">V projekt aplikace UPW hello souboru MainPage.xaml.cs a přidejte následující fragment kódu hello:</span><span class="sxs-lookup"><span data-stu-id="0ade3-131">In hello UWP app project file MainPage.xaml.cs and add hello following code snippet:</span></span>
   
        // Define a member variable for storing hello signed-in user. 
        private MobileServiceUser user;
   
        // Define a method that performs hello authentication process
        // using a Facebook sign-in. 
        private async System.Threading.Tasks.Task<bool> AuthenticateAsync()
        {
            string message;
            bool success = false;
            try
            {
                // Change 'MobileService' toohello name of your MobileServiceClient instance.
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
   
    <span data-ttu-id="0ade3-132">Tento kód ověřuje hello uživatele s přihlašovacími údaji, Facebook.</span><span class="sxs-lookup"><span data-stu-id="0ade3-132">This code authenticates hello user with a Facebook login.</span></span> <span data-ttu-id="0ade3-133">Pokud používáte zprostředkovatele identity než Facebook, změňte hodnotu hello **MobileServiceAuthenticationProvider** větší než hodnota toohello pro poskytovatele.</span><span class="sxs-lookup"><span data-stu-id="0ade3-133">If you are using an identity provider other than Facebook, change hello value of **MobileServiceAuthenticationProvider** above toohello value for your provider.</span></span>
2. <span data-ttu-id="0ade3-134">Nahraďte hello **OnNavigatedTo()** metoda v MainPage.xaml.cs.</span><span class="sxs-lookup"><span data-stu-id="0ade3-134">Replace hello **OnNavigatedTo()** method in MainPage.xaml.cs.</span></span> <span data-ttu-id="0ade3-135">V dalším kroku přidáte **přihlášení** tlačítko toohello aplikaci, která aktivuje ověřování.</span><span class="sxs-lookup"><span data-stu-id="0ade3-135">Next, you will add a **Sign in** button toohello app that triggers authentication.</span></span>

        protected override async void OnNavigatedTo(NavigationEventArgs e)
        {
            if (e.Parameter is Uri)
            {
                App.MobileService.ResumeWithURL(e.Parameter as Uri);
            }
        }

3. <span data-ttu-id="0ade3-136">Přidejte následující kód fragment kódu toohello MainPage.xaml.cs hello:</span><span class="sxs-lookup"><span data-stu-id="0ade3-136">Add hello following code snippet toohello MainPage.xaml.cs:</span></span>
   
        private async void ButtonLogin_Click(object sender, RoutedEventArgs e)
        {
            // Login hello user and then load data from hello mobile app.
            if (await AuthenticateAsync())
            {
                // Switch hello buttons and load items from hello mobile app.
                ButtonLogin.Visibility = Visibility.Collapsed;
                ButtonSave.Visibility = Visibility.Visible;
                //await InitLocalStoreAsync(); //offline sync support.
                await RefreshTodoItems();
            }
        }
4. <span data-ttu-id="0ade3-137">Otevřete soubor projektu MainPage.xaml hello, vyhledejte hello element, který definuje hello **Uložit** tlačítko a nahraďte ji metodou hello následující kód:</span><span class="sxs-lookup"><span data-stu-id="0ade3-137">Open hello MainPage.xaml project file, locate hello element that defines hello **Save** button and replace it with hello following code:</span></span>
   
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
5. <span data-ttu-id="0ade3-138">Přidejte následující kód fragment kódu toohello App.xaml.cs hello:</span><span class="sxs-lookup"><span data-stu-id="0ade3-138">Add hello following code snippet toohello App.xaml.cs:</span></span>

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
6. <span data-ttu-id="0ade3-139">Otevřete soubor Package.appxmanifest, přejděte příliš**deklarace**v **dostupné deklarace** rozevíracího seznamu vyberte **protokol** a klikněte na tlačítko **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="0ade3-139">Open Package.appxmanifest file, navigate too**Declarations**, in **Available Declarations** dropdown list, select **Protocol** and click **Add** button.</span></span> <span data-ttu-id="0ade3-140">Teď nakonfigurovat hello **vlastnosti** z hello **protokol** deklarace.</span><span class="sxs-lookup"><span data-stu-id="0ade3-140">Now configure hello **Properties** of hello **Protocol** declaration.</span></span> <span data-ttu-id="0ade3-141">V **zobrazovaný název**, přidejte název hello chcete toodisplay toousers vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="0ade3-141">In **Display name**, add hello name you wish toodisplay toousers of your application.</span></span> <span data-ttu-id="0ade3-142">V **název**, přidejte vaše {url_scheme_of_your_app}.</span><span class="sxs-lookup"><span data-stu-id="0ade3-142">In **Name**, add your {url_scheme_of_your_app}.</span></span>
7. <span data-ttu-id="0ade3-143">Stiskněte klávesu hello F5 klíče toorun hello aplikace, klikněte na tlačítko hello **přihlášení** tlačítko a přihlaste se k hello aplikace pomocí zprostředkovatele identity vybrané.</span><span class="sxs-lookup"><span data-stu-id="0ade3-143">Press hello F5 key toorun hello app, click hello **Sign in** button, and sign into hello app with your chosen identity provider.</span></span> <span data-ttu-id="0ade3-144">Po přihlášení s úspěšné hello aplikace běží bez chyb a jsou možné tooquery váš back-end a ujistěte se, toodata aktualizace.</span><span class="sxs-lookup"><span data-stu-id="0ade3-144">After your sign-in is successful, hello app runs without errors and you are able tooquery your backend and make updates toodata.</span></span>

## <span data-ttu-id="0ade3-145"><a name="tokens"></a>Úložiště hello ověřovací token v klientovi hello</span><span class="sxs-lookup"><span data-stu-id="0ade3-145"><a name="tokens"></a>Store hello authentication token on hello client</span></span>
<span data-ttu-id="0ade3-146">Hello předchozí příklad ukázal u standardní přihlášení, které vyžaduje klient toocontact hello obou zprostředkovatele identity hello a hello služby App Service při každém spuštění této aplikace hello.</span><span class="sxs-lookup"><span data-stu-id="0ade3-146">hello previous example showed a standard sign-in, which requires hello client toocontact both hello identity provider and hello App Service every time that hello app starts.</span></span> <span data-ttu-id="0ade3-147">Ne jenom je neefektivní, můžete spustit tuto metodu do využití-má vztah problémy mnoho zákazníků opakovat toostart aplikaci ve hello stejnou dobu.</span><span class="sxs-lookup"><span data-stu-id="0ade3-147">Not only is this method inefficient, you can run into usage-relates issues should many customers try toostart you app at hello same time.</span></span> <span data-ttu-id="0ade3-148">Lepším řešením je, že toocache hello autorizační token vrácený služby App Service a zkuste to toouse to před použitím nejprve u založenou na poskytovateli přihlášení.</span><span class="sxs-lookup"><span data-stu-id="0ade3-148">A better approach is toocache hello authorization token returned by your App Service and try toouse this first before using a provider-based sign-in.</span></span>

> [!NOTE]
> <span data-ttu-id="0ade3-149">Můžete mezipaměti hello tokenem vydaným službou App Services bez ohledu na to, jestli používáte ověřování klienta spravovat nebo spravované služby.</span><span class="sxs-lookup"><span data-stu-id="0ade3-149">You can cache hello token issued by App Services regardless of whether you are using client-managed or service-managed authentication.</span></span> <span data-ttu-id="0ade3-150">Tento kurz používá službu spravovat ověřování.</span><span class="sxs-lookup"><span data-stu-id="0ade3-150">This tutorial uses service-managed authentication.</span></span>
> 
> 

[!INCLUDE [mobile-windows-universal-dotnet-authenticate-app-with-token](../../includes/mobile-windows-universal-dotnet-authenticate-app-with-token.md)]

## <a name="next-steps"></a><span data-ttu-id="0ade3-151">Další kroky</span><span class="sxs-lookup"><span data-stu-id="0ade3-151">Next steps</span></span>
<span data-ttu-id="0ade3-152">Teď, když jste dokončili kurz základní ověřování, zvažte, budete pokračovat v tooone hello následující kurzy:</span><span class="sxs-lookup"><span data-stu-id="0ade3-152">Now that you completed this basic authentication tutorial, consider continuing on tooone of hello following tutorials:</span></span>

* [<span data-ttu-id="0ade3-153">Přidat nabízená oznámení tooyour aplikaci</span><span class="sxs-lookup"><span data-stu-id="0ade3-153">Add push notifications tooyour app</span></span>](app-service-mobile-windows-store-dotnet-get-started-push.md)  
  <span data-ttu-id="0ade3-154">Zjistěte, jak tooadd nabízená oznámení podporují tooyour aplikace a konfigurace mobilní aplikace back-end toouse Azure Notification Hubs toosend nabízených oznámení.</span><span class="sxs-lookup"><span data-stu-id="0ade3-154">Learn how tooadd push notifications support tooyour app and configure your Mobile App backend toouse Azure Notification Hubs toosend push notifications.</span></span>
* [<span data-ttu-id="0ade3-155">Povolení offline synchronizace u aplikace</span><span class="sxs-lookup"><span data-stu-id="0ade3-155">Enable offline sync for your app</span></span>](app-service-mobile-windows-store-dotnet-get-started-offline-data.md)  
  <span data-ttu-id="0ade3-156">Zjistěte, jak tooadd offline podporují aplikace pomocí back-end mobilní aplikace.</span><span class="sxs-lookup"><span data-stu-id="0ade3-156">Learn how tooadd offline support your app using an Mobile App backend.</span></span> <span data-ttu-id="0ade3-157">Offline synchronizace umožňuje koncovým uživatelům toointeract s mobilní aplikací&mdash;zobrazení, přidávat a upravovat data&mdash;i v případě, že není žádné síťové připojení.</span><span class="sxs-lookup"><span data-stu-id="0ade3-157">Offline sync allows end-users toointeract with a mobile app&mdash;viewing, adding, or modifying data&mdash;even when there is no network connection.</span></span>

<!-- URLs. -->
[Get started with your mobile app]: app-service-mobile-windows-store-dotnet-get-started.md
