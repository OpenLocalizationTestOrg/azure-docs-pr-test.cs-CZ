---
title: toosend Notification Hubs aaaUse Novinky (Windows Phone)
description: "Pomocí Azure Notification Hubs toouse značky v registrace toosend nejnovější aplikace Windows Phone tooa zprávy."
services: notification-hubs
documentationcenter: windows
author: ysxu
manager: erikre
editor: 
ms.assetid: 42726bf5-cc82-438d-9eaa-238da3322d80
ms.service: notification-hubs
ms.workload: mobile
ms.tgt_pltfrm: mobile-windows-phone
ms.devlang: dotnet
ms.topic: article
ms.date: 06/29/2016
ms.author: yuaxu
ms.openlocfilehash: 3519a8701105f88198afe288e59e9204420234db
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="use-notification-hubs-toosend-breaking-news"></a>Použít nejnovější zprávy přes toosend centra oznámení
[!INCLUDE [notification-hubs-selector-breaking-news](../../includes/notification-hubs-selector-breaking-news.md)]

## <a name="overview"></a>Přehled
Toto téma ukazuje, jak toouse Azure Notification Hubs toobroadcast nejnovější zprávy oznámení tooa aplikace Windows Phone 8.0/8.1 Silverlight. Pokud cílíte na Windows Store nebo Windows Phone 8.1 aplikace, naleznete v tootoohello [univerzální pro Windows](notification-hubs-windows-notification-dotnet-push-xplat-segmented-wns.md) verze. Po dokončení budete mít možnost tooregister pro nejnovější novinky kategorií, které vás zajímají a přijímat pouze nabízená oznámení pro tyto kategorie. Tento scénář je běžný vzor velký počet aplikací, kde mají oznámení odeslaných toobe toogroups uživatelů, kteří mají dříve deklarované zájem o jejich, např. čtečku RSS, aplikace pro Hudba ventilátory, atd.

Všesměrového vysílání scénáře jsou povolené zahrnutím jeden nebo více *značky* při vytváření registrace v centru oznámení hello. Pokud jsou oznámení odesílána tooa značky, všechna zařízení, která jste zaregistrovali hello značky obdrží oznámení hello. Protože značky jsou jednoduše řetězce, nemají toobe předem zřízený. Další informace o značkách najdete v části příliš[směrování centra oznámení a značky výrazy](notification-hubs-tags-segment-push-message.md).

## <a name="prerequisites"></a>Požadavky
Toto téma je založený na hello aplikace, které jste vytvořili v [Začínáme s Notification Hubs]. Před zahájením tohoto kurzu, musí jste již dokončili [Začínáme s Notification Hubs].

## <a name="add-category-selection-toohello-app"></a>Přidat aplikaci toohello výběru kategorie
Hello prvním krokem je tooadd hello uživatelského rozhraní elementy tooyour existující hlavní stránce umožňujících hello uživatele tooselect kategorie tooregister. kategorie Hello vybrané uživatelem se ukládají na hello zařízení. Při spuštění aplikace hello, registrace zařízení se vytvoří v centru oznámení s hello vybrané kategorie jako značky.

1. Otevřete soubor projektu MainPage.xaml hello a pak nahraďte hello **mřížky** elementů s názvem `TitlePanel` a `ContentPanel` s hello následující kód:
   
        <StackPanel x:Name="TitlePanel" Grid.Row="0" Margin="12,17,0,28">
            <TextBlock Text="Breaking News" Style="{StaticResource PhoneTextNormalStyle}" Margin="12,0"/>
            <TextBlock Text="Categories" Margin="9,-7,0,0" Style="{StaticResource PhoneTextTitle1Style}"/>
        </StackPanel>
   
        <Grid Name="ContentPanel" Grid.Row="1" Margin="12,0,12,0">
            <Grid.RowDefinitions>
                <RowDefinition Height="auto"/>
                <RowDefinition Height="auto" />
                <RowDefinition Height="auto" />
                <RowDefinition Height="auto" />
            </Grid.RowDefinitions>
            <Grid.ColumnDefinitions>
                <ColumnDefinition />
                <ColumnDefinition />
            </Grid.ColumnDefinitions>
            <CheckBox Name="WorldCheckBox" Grid.Row="0" Grid.Column="0">World</CheckBox>
            <CheckBox Name="PoliticsCheckBox" Grid.Row="1" Grid.Column="0">Politics</CheckBox>
            <CheckBox Name="BusinessCheckBox" Grid.Row="2" Grid.Column="0">Business</CheckBox>
            <CheckBox Name="TechnologyCheckBox" Grid.Row="0" Grid.Column="1">Technology</CheckBox>
            <CheckBox Name="ScienceCheckBox" Grid.Row="1" Grid.Column="1">Science</CheckBox>
            <CheckBox Name="SportsCheckBox" Grid.Row="2" Grid.Column="1">Sports</CheckBox>
            <Button Name="SubscribeButton" Content="Subscribe" HorizontalAlignment="Center" Grid.Row="3" Grid.Column="0" Grid.ColumnSpan="2" Click="SubscribeButton_Click" />
        </Grid>
2. V projektu hello, vytvořte novou třídu s názvem **oznámení**, přidejte hello **veřejné** toohello Modifikátor třídy definice a potom přidejte následující hello **pomocí** příkazy toohello nový soubor kódu:
   
        using Microsoft.Phone.Notification;
        using Microsoft.WindowsAzure.Messaging;
        using System.IO.IsolatedStorage;
        using System.Windows;
3. Kopírování hello následující kód do nové hello **oznámení** třídy:
   
        private NotificationHub hub;
   
        // Registration task toocomplete registration in hello ChannelUriUpdated event handler
        private TaskCompletionSource<Registration> registrationTask;
   
        public Notifications(string hubName, string listenConnectionString)
        {
            hub = new NotificationHub(hubName, listenConnectionString);
        }
   
        public IEnumerable<string> RetrieveCategories()
        {
            var categories = (string)IsolatedStorageSettings.ApplicationSettings["categories"];
            return categories != null ? categories.Split(',') : new string[0];
        }
   
        public async Task<Registration> StoreCategoriesAndSubscribe(IEnumerable<string> categories)
        {
            var categoriesAsString = string.Join(",", categories);
            var settings = IsolatedStorageSettings.ApplicationSettings;
            if (!settings.Contains("categories"))
            {
                settings.Add("categories", categoriesAsString);
            }
            else
            {
                settings["categories"] = categoriesAsString;
            }
            settings.Save();
   
            return await SubscribeToCategories();
        }
   
        public async Task<Registration> SubscribeToCategories()
        {
            registrationTask = new TaskCompletionSource<Registration>();
   
            var channel = HttpNotificationChannel.Find("MyPushChannel");
   
            if (channel == null)
            {
                channel = new HttpNotificationChannel("MyPushChannel");
                channel.Open();
                channel.BindToShellToast();
                channel.ChannelUriUpdated += channel_ChannelUriUpdated;
   
                // This is optional, used tooreceive notifications while hello app is running.
                channel.ShellToastNotificationReceived += channel_ShellToastNotificationReceived;
            }
   
            // If channel.ChannelUri is not null, we will complete hello registrationTask here.  
            // If it is null, hello registrationTask will be completed in hello ChannelUriUpdated event handler.
            if (channel.ChannelUri != null)
            {
                await RegisterTemplate(channel.ChannelUri);
            }
   
            return await registrationTask.Task;
        }
   
        async void channel_ChannelUriUpdated(object sender, NotificationChannelUriEventArgs e)
        {
            await RegisterTemplate(e.ChannelUri);
        }
   
        async Task<Registration> RegisterTemplate(Uri channelUri)
        {
            // Using a template registration toosupport notifications across platforms.
            // Any template notifications that contain messageParam and a corresponding tag expression
            // will be delivered for this registration.
   
            const string templateBodyMPNS = "<wp:Notification xmlns:wp=\"WPNotification\">" +
                                                "<wp:Toast>" +
                                                    "<wp:Text1>$(messageParam)</wp:Text1>" +
                                                "</wp:Toast>" +
                                            "</wp:Notification>";
   
            // hello stored categories tags are passed with hello template registration.
   
            registrationTask.SetResult(await hub.RegisterTemplateAsync(channelUri.ToString(), 
                templateBodyMPNS, "simpleMPNSTemplateExample", this.RetrieveCategories()));
   
            return await registrationTask.Task;
        }
   
        // This is optional. It is used tooreceive notifications while hello app is running.
        void channel_ShellToastNotificationReceived(object sender, NotificationEventArgs e)
        {
            StringBuilder message = new StringBuilder();
            string relativeUri = string.Empty;
   
            message.AppendFormat("Received Toast {0}:\n", DateTime.Now.ToShortTimeString());
   
            // Parse out hello information that was part of hello message.
            foreach (string key in e.Collection.Keys)
            {
                message.AppendFormat("{0}: {1}\n", key, e.Collection[key]);
   
                if (string.Compare(
                    key,
                    "wp:Param",
                    System.Globalization.CultureInfo.InvariantCulture,
                    System.Globalization.CompareOptions.IgnoreCase) == 0)
                {
                    relativeUri = e.Collection[key];
                }
            }
   
            // Display a dialog of all hello fields in hello toast.
            System.Windows.Deployment.Current.Dispatcher.BeginInvoke(() => 
            { 
                MessageBox.Show(message.ToString()); 
            });
        }

    Tato třída se používá hello izolované úložiště toostore hello kategorie příspěvků, že toto zařízení je tooreceive. Obsahuje také metody tooregister pro tyto kategorie pomocí [šablony](notification-hubs-templates-cross-platform-push-messages.md) registrace oznámení.


1. Soubor projektu App.xaml.cs hello, přidejte následující vlastnost toohello hello **aplikace** třídy. Nahraďte hello `<hub name>` a `<connection string with listen access>` zástupné symboly oznámení centra název a hello připojovacím řetězcem pro *DefaultListenSharedAccessSignature* kterou jste získali dříve.
   
        public Notifications notifications = new Notifications("<hub name>", "<connection string with listen access>");
   
   > [!NOTE]
   > Protože přihlašovací údaje, které jsou distribuované s klientskou aplikaci nejsou obecně bezpečné, musí distribuovat hello klíč pro přístup k naslouchání pouze s vaší klientské aplikace. Poslechněte umožní přístup k vaší aplikaci tooregister oznámení, ale existující registrace nemůže být upravena a nelze odeslat oznámení. Hello úplné přístupový klíč se používá ve službě Zabezpečené back-end pro zasílání oznámení a změna existující registrace.
   > 
   > 
2. V MainPage.xaml.cs přidejte následující řádek hello:
   
        using Windows.UI.Popups;
3. V souboru projektu hello MainPage.xaml.cs přidejte následující metodu hello:
   
        private async void SubscribeButton_Click(object sender, RoutedEventArgs e)
        {
          var categories = new HashSet<string>();
          if (WorldCheckBox.IsChecked == true) categories.Add("World");
          if (PoliticsCheckBox.IsChecked == true) categories.Add("Politics");
          if (BusinessCheckBox.IsChecked == true) categories.Add("Business");
          if (TechnologyCheckBox.IsChecked == true) categories.Add("Technology");
          if (ScienceCheckBox.IsChecked == true) categories.Add("Science");
          if (SportsCheckBox.IsChecked == true) categories.Add("Sports");
   
          var result = await ((App)Application.Current).notifications.StoreCategoriesAndSubscribe(categories);
   
          MessageBox.Show("Subscribed to: " + string.Join(",", categories) + " on registration id : " +
             result.RegistrationId);
        }
   
    Tato metoda vytvoří seznam kategorií a používá hello **oznámení** třídy toostore hello seznamu v místním úložišti hello a zaregistrujte hello značky odpovídající pomocí centra oznámení. Při změně kategorií, hello registrace se znovu vytvoří se nové kategorie hello.

Aplikace je nyní možné toostore sadu kategorií v místním úložišti na hello zařízení a zaregistrujte hello centra oznámení pokaždé, když změny uživatelů hello hello výběru kategorie.

## <a name="register-for-notifications"></a>Registrace pro oznámení
Tyto kroky zaregistrovat hello centra oznámení na spuštění pomocí hello kategorií, které byly uloženy v místním úložišti.

> [!NOTE]
> Protože hello kanál URI přiřadila hello Microsoft nabízené služby oznámení (MPNS) můžete změnit kdykoli, byste měli zaregistrovat pro oznámení často tooavoid oznámení selhání. Tento příklad zaregistruje oznámení pokaždé, když spustí aplikaci hello. Pro aplikace, které jsou často spouštíte více než jednou denně, pravděpodobně Pokud můžete přeskočit šířky pásma toopreserve registrace od předchozí registrace hello uplynul méně než jeden den.
> 
> 

1. Otevřete soubor App.xaml.cs hello a přidejte hello **asynchronní** modifikátor příliš**Application_Launching** metodu a nahraďte kód registrace Notification Hubs hello, který jste přidali v [Začínáme s Notification Hubs] s hello následující kód:
   
        private async void Application_Launching(object sender, LaunchingEventArgs e)
        {
            var result = await notifications.SubscribeToCategories();
   
            if (result != null)
                System.Windows.Deployment.Current.Dispatcher.BeginInvoke(() =>
                {
                    MessageBox.Show("Registration Id :" + result.RegistrationId, "Registered", MessageBoxButton.OK);
                });
        }
   
    Tím je zajištěno, že při každém spuštění aplikace hello načte kategorie hello z místního úložiště a požadavky registraci pro tyto kategorie.
2. V souboru projektu hello MainPage.xaml.cs přidejte následující kód, který implementuje hello hello **OnNavigatedTo** metoda:
   
        protected override void OnNavigatedTo(NavigationEventArgs e)
        {
            var categories = ((App)Application.Current).notifications.RetrieveCategories();
   
            if (categories.Contains("World")) WorldCheckBox.IsChecked = true;
            if (categories.Contains("Politics")) PoliticsCheckBox.IsChecked = true;
            if (categories.Contains("Business")) BusinessCheckBox.IsChecked = true;
            if (categories.Contains("Technology")) TechnologyCheckBox.IsChecked = true;
            if (categories.Contains("Science")) ScienceCheckBox.IsChecked = true;
            if (categories.Contains("Sports")) SportsCheckBox.IsChecked = true;
        }
   
    Tato aktualizace hello hlavní stránky založené na stav hello dříve uložit kategorií.

Hello aplikace je nyní dokončen a může ukládat sadu kategorií hello zařízení používá místní úložiště tooregister hello centra oznámení pokaždé, když změny uživatelů hello hello výběru kategorie. V dalším kroku bude definujeme back-end, který může odesílat kategorie oznámení toothis aplikace.

## <a name="sending-tagged-notifications"></a>Odesílání oznámení s příznakem
[!INCLUDE [notification-hubs-send-categories-template](../../includes/notification-hubs-send-categories-template.md)]

## <a name="run-hello-app-and-generate-notifications"></a>Spuštění aplikace hello a generovat oznámení
1. V sadě Visual Studio stiskněte klávesu F5 toocompile a spusťte aplikaci hello.
   
    ![][1]
   
    Všimněte si, že přepíná hello aplikaci, kterou poskytuje sadu uživatelského rozhraní, která umožňuje vybrat toosubscribe kategorie hello k.
2. Povolit jednu nebo více kategorií přepínačů a potom klikněte na **přihlásit k odběru**.
   
    aplikace Hello převede hello vybrané kategorie značky a požaduje novou registraci zařízení pro hello vybrané značky z centra oznámení hello. Hello registrované kategorie se vrátí a zobrazí v dialogu.
   
    ![][2]
3. Po přijetí potvrzení, že byly vaše kategorie odběru byla dokončena, spusťte hello konzole aplikace toosend oznámení pro každé kategorie. Ověřte, že dostanete oznámení pro přihlášení k odběru kategorií hello.
   
    ![][3]

Dokončili jste v tomto tématu.

<!--##Next steps

In this tutorial we learned how toobroadcast breaking news by category. Consider completing one of hello following tutorials that highlight other advanced Notification Hubs scenarios:

+ [Use Notification Hubs toobroadcast localized breaking news]

    Learn how tooexpand hello breaking news app tooenable sending localized notifications.

+ [Notify users with Notification Hubs]

    Learn how toopush notifications toospecific authenticated users. This is a good solution for sending notifications only toospecific users.
-->

<!-- Anchors. -->
[Add category selection toohello app]: #adding-categories
[Register for notifications]: #register
[Send notifications from your back-end]: #send
[Run hello app and generate notifications]: #test-app
[Next Steps]: #next-steps

<!-- Images. -->
[1]: ./media/notification-hubs-windows-phone-send-breaking-news/notification-hub-breakingnews.png
[2]: ./media/notification-hubs-windows-phone-send-breaking-news/notification-hub-registration.png
[3]: ./media/notification-hubs-windows-phone-send-breaking-news/notification-hub-toast.png



<!-- URLs.-->
[Začínáme s Notification Hubs]: /manage/services/notification-hubs/get-started-notification-hubs-wp8/
[Use Notification Hubs toobroadcast localized breaking news]: ../breakingnews-localized-wp8.md
[Notify users with Notification Hubs]: /manage/services/notification-hubs/notify-users/
[Mobile Service]: /develop/mobile/tutorials/get-started
[Notification Hubs Guidance]: http://msdn.microsoft.com/library/jj927170.aspx
[Notification Hubs How-toofor Windows Phone]: ??

