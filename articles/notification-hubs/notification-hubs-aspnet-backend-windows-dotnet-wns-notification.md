---
title: "aaaAzure upozornění uživatelů centra oznámení s .NET back-end"
description: "Zjistěte, jak toosend zabezpečené nabízená oznámení v Azure. Ukázky kódu jsou vytvořené v C# s použitím hello .NET API."
documentationcenter: windows
author: ysxu
manager: erikre
services: notification-hubs
editor: 
ms.assetid: 012529f2-fdbc-43c4-8634-2698164b5880
ms.service: notification-hubs
ms.workload: mobile
ms.tgt_pltfrm: mobile-windows
ms.devlang: dotnet
ms.topic: article
ms.date: 10/03/2016
ms.author: yuaxu
ms.openlocfilehash: a366181faa81e78adf4de61435ef2790c3aa29d1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-notification-hubs-notify-users-with-net-backend"></a>Azure upozornění uživatelů centra oznámení s .NET back-end
[!INCLUDE [notification-hubs-selector-aspnet-backend-notify-users](../../includes/notification-hubs-selector-aspnet-backend-notify-users.md)]

## <a name="overview"></a>Přehled
Podpora nabízená oznámení v Azure můžete tooaccess snadné použití, multiplatform a nabízené škálovanou infrastrukturu, která výrazně zjednodušuje hello implementace nabízených oznámení spotřebních a podnikových aplikací pro mobilní platformy. Tento kurz ukazuje, jak Azure Notification Hubs toosend toouse nabízená oznámení tooa konkrétní aplikace uživatele na konkrétní zařízení. Backendu ASP.NET WebAPI je použité tooauthenticate klientů. Pomocí hello ověřeného uživatele klienta a značky automaticky přidá hello back-end toonotification registrace. Tato značka bude použité toosend podle hello back-end toogenerate oznámení pro konkrétního uživatele. Další informace o registrace pro oznámení pomocí back-end aplikace najdete v tématu hello pokyny [registrace z back-end aplikace](http://msdn.microsoft.com/library/dn743807.aspx). V tomto kurzu vychází hello Centrum oznámení a projekt, který jste vytvořili v hello [Začínáme s Notification Hubs] kurzu.

V tomto kurzu je také požadovaných toohello hello [zabezpečení Push] kurzu. Po dokončení kroků hello v tomto kurzu, můžete přejít toohello [zabezpečení Push] kurz, který ukazuje, jak toomodify hello kód na tento kurz toosend nabízených oznámení bezpečně.

## <a name="before-you-begin"></a>Než začnete
Záleží nám na vašem názoru. Pokud máte jakékoli problémy dokončení tohoto tématu nebo doporučit vylepšení tohoto obsahu, by nám chcete sdělit svůj názor hello dolní části stránky hello.

Hello dokončit kód v tomto kurzu lze nalézt na Githubu [zde](https://github.com/Azure/azure-notificationhubs-samples/tree/master/dotnet/NotifyUsers). 

## <a name="prerequisites"></a>Požadavky
Než začnete tento kurz, musíte jste již dokončili tato kurzech používání Mobile Services:

* [Začínáme s Notification Hubs]<br/>Vytvoření centra oznámení a rezervovat název aplikace hello a zaregistrujte tooreceive oznámení v tomto kurzu. Tento kurz předpokládá, že jste už dokončili kroky. Pokud ne, postupujte podle kroků hello v [Začínáme s Notification Hubs (pro Windows Store)](notification-hubs-windows-store-dotnet-get-started-wns-push-notification.md); konkrétně hello části [registrace aplikace pro Windows Store hello](notification-hubs-windows-store-dotnet-get-started-wns-push-notification.md#register-your-app-for-the-windows-store) a [konfigurace Vaše centrum oznámení](notification-hubs-windows-store-dotnet-get-started-wns-push-notification.md#configure-your-notification-hub). Konkrétně se ujistěte, zda jste zadali hello **SID balíčku** a **tajný klíč klienta** hodnoty v portálu hello hello **konfigurace** kartě pro vaše Centrum oznámení. Tento postup konfigurace je popsaný v části hello [Konfigurace centra oznámení](notification-hubs-windows-store-dotnet-get-started-wns-push-notification.md#configure-your-notification-hub). Toto je důležitý krok: Pokud hello pověření na portálu hello neodpovídají možností určených pro název aplikace hello zvolíte, se nezdaří hello nabízených oznámení.

> [!NOTE]
> Pokud používáte v Azure App Service Mobile Apps jako back-end službu, přečtěte si téma hello [Mobile Apps verze](../app-service-mobile/app-service-mobile-windows-store-dotnet-get-started-push.md) tohoto kurzu.
> 
> 

&nbsp;

[!INCLUDE [notification-hubs-aspnet-backend-notifyusers](../../includes/notification-hubs-aspnet-backend-notifyusers.md)]

## <a name="update-hello-code-for-hello-client-project"></a>Kód hello aktualizace pro hello klientského projektu
V této části můžete aktualizovat hello kódu v projektu hello dokončeno pro hello [Začínáme s Notification Hubs] kurzu. Hello by už přidružené hello úložiště a nakonfigurovaný pro vaše Centrum oznámení. V této části přidáte kód toocall hello nový WebAPI back-end a používat ho pro registraci a odesílání oznámení.

1. V sadě Visual Studio otevřete řešení hello hello jste vytvořili pro hello [Začínáme s Notification Hubs] kurzu.
2. V Průzkumníku řešení klikněte pravým tlačítkem na hello **(Windows 8.1)** projektu a pak klikněte na **spravovat balíčky NuGet**.
3. Na levé straně hello, klikněte na tlačítko **Online**.
4. V hello **vyhledávání** zadejte **klienta Http**.
5. V seznamu výsledků hello, klikněte na tlačítko **knihovny klienta HTTP Microsoft**a potom klikněte na **nainstalovat**. Dokončení instalace hello.
6. Zpět v hello NuGet **vyhledávání** zadejte **Json.net**. Nainstalujte hello **Json.NET** balíček a pak zavřete hello okno Správce balíčků NuGet.
7. Opakujte kroky hello výše pro hello **(Windows Phone 8.1)** projektu tooinstall hello **JSON.NET** balíček NuGet pro projekt Windows Phone hello.
8. V Průzkumníku řešení v hello **(Windows 8.1)** projektu, klikněte dvakrát na **MainPage.xaml** tooopen ji v editoru Visual Studio hello.
9. V hello **MainPage.xaml** kód XML, nahraďte hello `<Grid>` oddíl s hello následující kód. Tento kód přidá, že se budou ověřovat textové pole uživatelské jméno a heslo, které hello uživatele. Také přidá textových polí pro hello oznámení a značky hello uživatelské jméno, které by měly dostávat oznámení hello:
   
        <Grid>
            <Grid.RowDefinitions>
                <RowDefinition Height="Auto"/>
                <RowDefinition Height="*"/>
            </Grid.RowDefinitions>
   
            <TextBlock Grid.Row="0" Text="Notify Users" HorizontalAlignment="Center" FontSize="48"/>
   
            <StackPanel Grid.Row="1" VerticalAlignment="Center">
                <Grid>
                    <Grid.RowDefinitions>
                        <RowDefinition Height="Auto"/>
                        <RowDefinition Height="Auto"/>
                        <RowDefinition Height="Auto"/>
                        <RowDefinition Height="Auto"/>
                        <RowDefinition Height="Auto"/>
                        <RowDefinition Height="Auto"/>
                        <RowDefinition Height="Auto"/>
                        <RowDefinition Height="Auto"/>
                        <RowDefinition Height="Auto"/>
                        <RowDefinition Height="Auto"/>
                        <RowDefinition Height="Auto"/>
                    </Grid.RowDefinitions>
                    <Grid.ColumnDefinitions>
                        <ColumnDefinition></ColumnDefinition>
                        <ColumnDefinition></ColumnDefinition>
                        <ColumnDefinition></ColumnDefinition>
                    </Grid.ColumnDefinitions>
                    <TextBlock Grid.Row="0" Grid.ColumnSpan="3" Text="Username" FontSize="24" Margin="20,0,20,0"/>
                    <TextBox Name="UsernameTextBox" Grid.Row="1" Grid.ColumnSpan="3" Margin="20,0,20,0"/>
                    <TextBlock Grid.Row="2" Grid.ColumnSpan="3" Text="Password" FontSize="24" Margin="20,0,20,0" />
                    <PasswordBox Name="PasswordTextBox" Grid.Row="3" Grid.ColumnSpan="3" Margin="20,0,20,0"/>
   
                    <Button Grid.Row="4" Grid.ColumnSpan="3" HorizontalAlignment="Center" VerticalAlignment="Center"
                                Content="1. Login and register" Click="LoginAndRegisterClick" Margin="0,0,0,20"/>
   
                    <ToggleButton Name="toggleWNS" Grid.Row="5" Grid.Column="0" HorizontalAlignment="Right" Content="WNS" IsChecked="True" />
                    <ToggleButton Name="toggleGCM" Grid.Row="5" Grid.Column="1" HorizontalAlignment="Center" Content="GCM" />
                    <ToggleButton Name="toggleAPNS" Grid.Row="5" Grid.Column="2" HorizontalAlignment="Left" Content="APNS" />
   
                    <TextBlock Grid.Row="6" Grid.ColumnSpan="3" Text="Username Tag tooSend To" FontSize="24" Margin="20,0,20,0"/>
                    <TextBox Name="ToUserTagTextBox" Grid.Row="7" Grid.ColumnSpan="3" Margin="20,0,20,0" TextWrapping="Wrap" />
                    <TextBlock Grid.Row="8" Grid.ColumnSpan="3" Text="Enter Notification Message" FontSize="24" Margin="20,0,20,0"/>
                    <TextBox Name="NotificationMessageTextBox" Grid.Row="9" Grid.ColumnSpan="3" Margin="20,0,20,0" TextWrapping="Wrap" />
                    <Button Grid.Row="10" Grid.ColumnSpan="3" HorizontalAlignment="Center" Content="2. Send push" Click="PushClick" Name="SendPushButton" />
                </Grid>
            </StackPanel>
        </Grid>
10. V Průzkumníku řešení v hello **(Windows Phone 8.1)** projekt, otevřete **MainPage.xaml** a nahraďte hello Windows Phone 8.1 `<Grid>` oddíl s Tento stejný kód výše. Hello rozhraní by měla vypadat podobně jako toowhats vidíte níže.
    
    ![][13]
11. V Průzkumníku řešení otevřete hello **MainPage.xaml.cs** souboru hello **(Windows 8.1)** a **(Windows Phone 8.1)** projekty. Přidejte následující hello `using` příkazy hello horní části oba soubory:
    
        using System.Net.Http;
        using Windows.Storage;
        using System.Net.Http.Headers;
        using Windows.Networking.PushNotifications;
        using Windows.UI.Popups;
        using System.Threading.Tasks;
12. V **MainPage.xaml.cs** pro hello **(Windows 8.1)** a **(Windows Phone 8.1)** projekty, přidejte následující člen toohello hello `MainPage` třídy. Být jisti tooreplace `<Enter Your Backend Endpoint>` s hello váš koncový bod skutečné back-end získali dříve. Například, `http://mybackend.azurewebsites.net`.
    
        private static string BACKEND_ENDPOINT = "<Enter Your Backend Endpoint>";
13. Přidat kód hello pod toohello MainPage třídy v **MainPage.xaml.cs** pro hello **(Windows 8.1)** a **(Windows Phone 8.1)** projekty.
    
    Hello `PushClick` metoda je hello klikněte na obslužnou rutinu pro hello **odeslat nabízené** tlačítko. Zařízení se značkou uživatelského jména, která odpovídá hello volá hello back-end tootrigger tooall oznámení `to_tag` parametr. jako obsah JSON v textu žádosti hello je odeslána zpráva oznámení Hello.
    
    Hello `LoginAndRegisterClick` metoda je hello klikněte na obslužnou rutinu pro hello **přihlášení a registrace** tlačítko. Ukládá hello basic ověřovací token v místním úložišti (Upozorňujeme, že to představuje všechny token používá vaše schéma ověřování), pak použije `RegisterClient` tooregister pro oznámení pomocí back-end hello.

        private async void PushClick(object sender, RoutedEventArgs e)
        {
            if (toggleWNS.IsChecked.Value)
            {
                await sendPush("wns", ToUserTagTextBox.Text, this.NotificationMessageTextBox.Text);
            }
            if (toggleGCM.IsChecked.Value)
            {
                await sendPush("gcm", ToUserTagTextBox.Text, this.NotificationMessageTextBox.Text);
            }
            if (toggleAPNS.IsChecked.Value)
            {
                await sendPush("apns", ToUserTagTextBox.Text, this.NotificationMessageTextBox.Text);

            }
        }

        private async Task sendPush(string pns, string userTag, string message)
        {
            var POST_URL = BACKEND_ENDPOINT + "/api/notifications?pns=" +
                pns + "&to_tag=" + userTag;

            using (var httpClient = new HttpClient())
            {
                var settings = ApplicationData.Current.LocalSettings.Values;
                httpClient.DefaultRequestHeaders.Authorization = new AuthenticationHeaderValue("Basic", (string)settings["AuthenticationToken"]);

                try
                {
                    await httpClient.PostAsync(POST_URL, new StringContent("\"" + message + "\"",
                        System.Text.Encoding.UTF8, "application/json"));
                }
                catch (Exception ex)
                {
                    MessageDialog alert = new MessageDialog(ex.Message, "Failed toosend " + pns + " message");
                    alert.ShowAsync();
                }
            }
        }

        private async void LoginAndRegisterClick(object sender, RoutedEventArgs e)
        {
            SetAuthenticationTokenInLocalStorage();

            var channel = await PushNotificationChannelManager.CreatePushNotificationChannelForApplicationAsync();

            // hello "username:<user name>" tag gets automatically added by hello message handler in hello backend.
            // hello tag passed here can be whatever other tags you may want toouse.
            try
            {
                // hello device handle used will be different depending on hello device and PNS. 
                // Windows devices use hello channel uri as hello PNS handle.
                await new RegisterClient(BACKEND_ENDPOINT).RegisterAsync(channel.Uri, new string[] { "myTag" });

                var dialog = new MessageDialog("Registered as: " + UsernameTextBox.Text);
                dialog.Commands.Add(new UICommand("OK"));
                await dialog.ShowAsync();
                SendPushButton.IsEnabled = true;
            }
            catch (Exception ex)
            {
                MessageDialog alert = new MessageDialog(ex.Message, "Failed tooregister with RegisterClient");
                alert.ShowAsync();
            }
        }

        private void SetAuthenticationTokenInLocalStorage()
        {
            string username = UsernameTextBox.Text;
            string password = PasswordTextBox.Password;

            var token = Convert.ToBase64String(System.Text.Encoding.UTF8.GetBytes(username + ":" + password));
            ApplicationData.Current.LocalSettings.Values["AuthenticationToken"] = token;
        }



1. V Průzkumníku řešení klikněte v části hello **sdílené** projekt, otevřete hello **App.xaml.cs** souboru. Najít hello volání příliš`InitNotificationsAsync()` v hello `OnLaunched()` obslužné rutiny události. Komentář nebo odstranit hello volání příliš`InitNotificationsAsync()`. registrace oznámení bude inicializovat obslužná rutina tlačítko Hello přidaných výše.

        protected override void OnLaunched(LaunchActivatedEventArgs e)
        {
            //InitNotificationsAsync();


1. V Průzkumníku řešení klikněte pravým tlačítkem na hello **sdílené** projektu a pak klikněte na **přidat**a potom klikněte na **třída**. Název třídy hello **RegisterClient.cs**, pak klikněte na tlačítko **OK** toogenerate hello třídy.
   
   Tato třída bude zalomení hello REST volání požadované toocontact hello back-end aplikace, v pořadí tooregister pro nabízená oznámení. Také místně ukládá hello *registrationIds* vytvořené hello centra oznámení podle popisu v [registrace z back-end aplikace](http://msdn.microsoft.com/library/dn743807.aspx). Všimněte si, že se používá autorizační token uložená v místním úložišti, po kliknutí na tlačítko hello **přihlášení a registrace** tlačítko.
2. Přidejte následující hello `using` příkazy hello horní části souboru RegisterClient.cs hello:
   
       using Windows.Storage;
       using System.Net;
       using System.Net.Http;
       using System.Net.Http.Headers;
       using Newtonsoft.Json;
       using System.Threading.Tasks;
       using System.Linq;
3. Přidejte následující kód do hello hello `RegisterClient` definici třídy.
   
       private string POST_URL;
   
       private class DeviceRegistration
       {
           public string Platform { get; set; }
           public string Handle { get; set; }
           public string[] Tags { get; set; }
       }
   
       public RegisterClient(string backendEndpoint)
       {
           POST_URL = backendEndpoint + "/api/register";
       }
   
       public async Task RegisterAsync(string handle, IEnumerable<string> tags)
       {
           var regId = await RetrieveRegistrationIdOrRequestNewOneAsync();
   
           var deviceRegistration = new DeviceRegistration
           {
               Platform = "wns",
               Handle = handle,
               Tags = tags.ToArray<string>()
           };
   
           var statusCode = await UpdateRegistrationAsync(regId, deviceRegistration);
   
           if (statusCode == HttpStatusCode.Gone)
           {
               // regId is expired, deleting from local storage & recreating
               var settings = ApplicationData.Current.LocalSettings.Values;
               settings.Remove("__NHRegistrationId");
               regId = await RetrieveRegistrationIdOrRequestNewOneAsync();
               statusCode = await UpdateRegistrationAsync(regId, deviceRegistration);
           }
   
           if (statusCode != HttpStatusCode.Accepted)
           {
               // log or throw
               throw new System.Net.WebException(statusCode.ToString());
           }
       }
   
       private async Task<HttpStatusCode> UpdateRegistrationAsync(string regId, DeviceRegistration deviceRegistration)
       {
           using (var httpClient = new HttpClient())
           {
               var settings = ApplicationData.Current.LocalSettings.Values;
               httpClient.DefaultRequestHeaders.Authorization = new AuthenticationHeaderValue("Basic", (string) settings["AuthenticationToken"]);
   
               var putUri = POST_URL + "/" + regId;
   
               string json = JsonConvert.SerializeObject(deviceRegistration);
                               var response = await httpClient.PutAsync(putUri, new StringContent(json, Encoding.UTF8, "application/json"));
               return response.StatusCode;
           }
       }
   
       private async Task<string> RetrieveRegistrationIdOrRequestNewOneAsync()
       {
           var settings = ApplicationData.Current.LocalSettings.Values;
           if (!settings.ContainsKey("__NHRegistrationId"))
           {
               using (var httpClient = new HttpClient())
               {
                   httpClient.DefaultRequestHeaders.Authorization = new AuthenticationHeaderValue("Basic", (string)settings["AuthenticationToken"]);
   
                   var response = await httpClient.PostAsync(POST_URL, new StringContent(""));
                   if (response.IsSuccessStatusCode)
                   {
                       string regId = await response.Content.ReadAsStringAsync();
                       regId = regId.Substring(1, regId.Length - 2);
                       settings.Add("__NHRegistrationId", regId);
                   }
                   else
                   {
                       throw new System.Net.WebException(response.StatusCode.ToString());
                   }
               }
           }
           return (string)settings["__NHRegistrationId"];
   
       }
4. Uložte všechny provedené změny.

## <a name="testing-hello-application"></a>Testování aplikace hello
1. Spuštění aplikace hello na Windows 8.1 a Windows Phone 8.1. Pro Windows Phone 8.1 můžete spustit hello instance v emulátoru hello nebo skutečné zařízení.
2. V instanci aplikace hello na hello Windows 8.1, zadejte **uživatelské jméno** a **heslo** jak je znázorněno níže úvodní obrazovka. Ho by se měly lišit od hello uživatelské jméno a heslo, které zadáte na Windows Phone.
3. Klikněte na tlačítko **přihlášení a registrace** a ověřte, zobrazí se dialogové okno ukazuje, že jste byli přihlášeni. To vám také umožní hello **odeslat nabízené** tlačítko.
   
    ![][14]
4. Na instanci hello Windows Phone 8.1, zadejte řetězec názvu uživatel v obou hello **uživatelské jméno** a **heslo** pole klikněte **přihlášení a registrace**.
5. Potom v hello **značky uživatelské jméno příjemce** pole, zadejte uživatelské jméno hello registrovaný na Windows 8.1. Zadejte zprávu oznámení a klikněte na tlačítko **odeslat nabízené**.
   
    ![][16]
6. Hello oznámení zpráva se zobrazí pouze hello zařízení, která byla zaregistrovaná hello odpovídající značky uživatelské jméno.
   
    ![][15]

## <a name="next-steps"></a>Další kroky
* Pokud chcete toosegment uživatele podle zájmových skupin, najdete v části [toosend použití centra oznámení nejnovější zprávy přes].
* najdete v části Další o toolearn toouse Notification Hubs, [pokyny centra oznámení].

[9]: ./media/notification-hubs-aspnet-backend-windows-dotnet-notify-users/notification-hubs-secure-push9.png
[10]: ./media/notification-hubs-aspnet-backend-windows-dotnet-notify-users/notification-hubs-secure-push10.png
[11]: ./media/notification-hubs-aspnet-backend-windows-dotnet-notify-users/notification-hubs-secure-push11.png
[12]: ./media/notification-hubs-aspnet-backend-windows-dotnet-notify-users/notification-hubs-secure-push12.png
[13]: ./media/notification-hubs-aspnet-backend-windows-dotnet-notify-users/notification-hubs-wp-ui.png
[14]: ./media/notification-hubs-aspnet-backend-windows-dotnet-notify-users/notification-hubs-windows-instance-username.png
[15]: ./media/notification-hubs-aspnet-backend-windows-dotnet-notify-users/notification-hubs-notification-received.png
[16]: ./media/notification-hubs-aspnet-backend-windows-dotnet-notify-users/notification-hubs-wp-send-message.png



<!-- URLs. -->
[Začínáme s Notification Hubs]: notification-hubs-windows-store-dotnet-get-started-wns-push-notification.md
[zabezpečení Push]: notification-hubs-aspnet-backend-windows-dotnet-wns-secure-push-notification.md
[toosend použití centra oznámení nejnovější zprávy přes]: notification-hubs-windows-notification-dotnet-push-xplat-segmented-wns.md
[pokyny centra oznámení]: http://msdn.microsoft.com/library/jj927170.aspx
