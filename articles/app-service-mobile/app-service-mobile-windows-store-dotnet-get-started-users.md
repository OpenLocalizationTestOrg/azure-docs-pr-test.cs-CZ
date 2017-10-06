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
# <a name="add-authentication-tooyour-windows-app"></a>Přidat aplikace pro Windows tooyour ověřování
[!INCLUDE [app-service-mobile-selector-get-started-users](../../includes/app-service-mobile-selector-get-started-users.md)]

Toto téma ukazuje, jak tooadd cloudové ověřování tooyour mobilní aplikace. V tomto kurzu přidáte ověřování toohello univerzální platformu Windows (UWP) rychlé spuštění projektu pro mobilní aplikace pomocí zprostředkovatele identity, která je podporována službou Azure App Service. Po se úspěšně ověří a autorizuje váš back-end mobilní aplikace, zobrazí se hodnota ID uživatele hello.

V tomto kurzu vychází z mobilní aplikace hello rychlý start. Je třeba nejprve provést hello kurzu [Začínáme s Mobile Apps](app-service-mobile-windows-store-dotnet-get-started.md).

## <a name="register"></a>Registrace aplikace pro ověřování a nakonfigurujte hello služby App Service
[!INCLUDE [app-service-mobile-register-authentication](../../includes/app-service-mobile-register-authentication.md)]

## <a name="redirecturl"></a>Přidání adresy URL pro vaše aplikace toohello povoleno externí přesměrování

Zabezpečené ověřování vyžaduje, můžete definovat nové schéma adresy URL pro vaši aplikaci. To umožňuje hello ověřování systému tooredirect back tooyour aplikaci po dokončení procesu ověřování hello. V tomto kurzu používáme schéma adresy URL hello _appname_ v průběhu. Můžete však použít žádné schéma adresy URL, které zvolíte. Mělo by být jedinečný tooyour mobilních aplikací. tooenable hello přesměrování na straně serveru hello:

1. V [portál Azure] hello vyberte App Service.

2. Klikněte na tlačítko hello **ověřování / autorizace** možnost nabídky.

3. V hello **povoleno externí adres URL pro přesměrování**, zadejte `url_scheme_of_your_app://easyauth.callback`.  Hello **url_scheme_of_your_app** v tento řetězec je hello schéma adresy URL pro mobilní aplikace.  Měl by splňovat specifikaci normální adresu URL pro určitý protokol (používejte písmena a čísla pouze a začněte s písmenem).  Měli byste si poznamenat hello řetězce, který zvolíte, bude nutné tooadjust kód mobilní aplikace s hello schéma adresy URL na několika místech.

4. Klikněte na **OK**.

5. Klikněte na **Uložit**.

## <a name="permissions"></a>Omezte oprávnění tooauthenticated
[!INCLUDE [app-service-mobile-restrict-permissions-dotnet-backend](../../includes/app-service-mobile-restrict-permissions-dotnet-backend.md)]

Nyní můžete ověřit, že back-end tooyour anonymní přístup je zakázané. S projekt aplikace UPW hello nastavit jako hello spuštění projekt nasazení a spuštění aplikace hello; Ověřte, že se po spuštění aplikace hello vyvolá k neošetřené výjimce s stavový kód 401 (Neautorizováno). K tomu dojde, protože aplikace hello pokusí tooaccess kódu mobilní aplikace jako neověřený uživatel, ale hello *TodoItem* tabulka nyní vyžaduje ověření.

Potom aktualizujte uživatele tooauthenticate aplikace hello před vyžádáním prostředky z vaší služby App Service.

## <a name="add-authentication"></a>Přidat aplikaci toohello ověřování
1. V projekt aplikace UPW hello souboru MainPage.xaml.cs a přidejte následující fragment kódu hello:
   
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
   
    Tento kód ověřuje hello uživatele s přihlašovacími údaji, Facebook. Pokud používáte zprostředkovatele identity než Facebook, změňte hodnotu hello **MobileServiceAuthenticationProvider** větší než hodnota toohello pro poskytovatele.
2. Nahraďte hello **OnNavigatedTo()** metoda v MainPage.xaml.cs. V dalším kroku přidáte **přihlášení** tlačítko toohello aplikaci, která aktivuje ověřování.

        protected override async void OnNavigatedTo(NavigationEventArgs e)
        {
            if (e.Parameter is Uri)
            {
                App.MobileService.ResumeWithURL(e.Parameter as Uri);
            }
        }

3. Přidejte následující kód fragment kódu toohello MainPage.xaml.cs hello:
   
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
4. Otevřete soubor projektu MainPage.xaml hello, vyhledejte hello element, který definuje hello **Uložit** tlačítko a nahraďte ji metodou hello následující kód:
   
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
5. Přidejte následující kód fragment kódu toohello App.xaml.cs hello:

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
6. Otevřete soubor Package.appxmanifest, přejděte příliš**deklarace**v **dostupné deklarace** rozevíracího seznamu vyberte **protokol** a klikněte na tlačítko **přidat** tlačítko. Teď nakonfigurovat hello **vlastnosti** z hello **protokol** deklarace. V **zobrazovaný název**, přidejte název hello chcete toodisplay toousers vaší aplikace. V **název**, přidejte vaše {url_scheme_of_your_app}.
7. Stiskněte klávesu hello F5 klíče toorun hello aplikace, klikněte na tlačítko hello **přihlášení** tlačítko a přihlaste se k hello aplikace pomocí zprostředkovatele identity vybrané. Po přihlášení s úspěšné hello aplikace běží bez chyb a jsou možné tooquery váš back-end a ujistěte se, toodata aktualizace.

## <a name="tokens"></a>Úložiště hello ověřovací token v klientovi hello
Hello předchozí příklad ukázal u standardní přihlášení, které vyžaduje klient toocontact hello obou zprostředkovatele identity hello a hello služby App Service při každém spuštění této aplikace hello. Ne jenom je neefektivní, můžete spustit tuto metodu do využití-má vztah problémy mnoho zákazníků opakovat toostart aplikaci ve hello stejnou dobu. Lepším řešením je, že toocache hello autorizační token vrácený služby App Service a zkuste to toouse to před použitím nejprve u založenou na poskytovateli přihlášení.

> [!NOTE]
> Můžete mezipaměti hello tokenem vydaným službou App Services bez ohledu na to, jestli používáte ověřování klienta spravovat nebo spravované služby. Tento kurz používá službu spravovat ověřování.
> 
> 

[!INCLUDE [mobile-windows-universal-dotnet-authenticate-app-with-token](../../includes/mobile-windows-universal-dotnet-authenticate-app-with-token.md)]

## <a name="next-steps"></a>Další kroky
Teď, když jste dokončili kurz základní ověřování, zvažte, budete pokračovat v tooone hello následující kurzy:

* [Přidat nabízená oznámení tooyour aplikaci](app-service-mobile-windows-store-dotnet-get-started-push.md)  
  Zjistěte, jak tooadd nabízená oznámení podporují tooyour aplikace a konfigurace mobilní aplikace back-end toouse Azure Notification Hubs toosend nabízených oznámení.
* [Povolení offline synchronizace u aplikace](app-service-mobile-windows-store-dotnet-get-started-offline-data.md)  
  Zjistěte, jak tooadd offline podporují aplikace pomocí back-end mobilní aplikace. Offline synchronizace umožňuje koncovým uživatelům toointeract s mobilní aplikací&mdash;zobrazení, přidávat a upravovat data&mdash;i v případě, že není žádné síťové připojení.

<!-- URLs. -->
[Get started with your mobile app]: app-service-mobile-windows-store-dotnet-get-started.md
