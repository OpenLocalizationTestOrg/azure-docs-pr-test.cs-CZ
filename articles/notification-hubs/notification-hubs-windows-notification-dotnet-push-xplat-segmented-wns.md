---
title: "toosend Notification Hubs aaaUse Novinky (univerzální pro Windows)"
description: "Použití centra oznámení Azure se značkami v toosend registrace hello nejnovější novinky tooa univerzální aplikace pro Windows."
services: notification-hubs
documentationcenter: windows
author: ysxu
manager: erikre
editor: 
ms.assetid: 994d2eed-f62e-433c-bf65-4afebf1c0561
ms.service: notification-hubs
ms.workload: mobile
ms.tgt_pltfrm: mobile-windows
ms.devlang: dotnet
ms.topic: article
ms.date: 06/29/2016
ms.author: yuaxu
ms.openlocfilehash: f102d286d2c7bd387fcfa2c7eab2ba31a0298517
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="use-notification-hubs-toosend-breaking-news"></a>Použít nejnovější zprávy přes toosend centra oznámení
[!INCLUDE [notification-hubs-selector-breaking-news](../../includes/notification-hubs-selector-breaking-news.md)]

## <a name="overview"></a>Přehled
Toto téma ukazuje, jak toobroadcast Azure Notification Hubs toouse nejnovější zprávy oznámení tooa Windows Store nebo Windows Phone 8.1 (bez Silverlight) aplikace. Pokud cílíte na Windows Phone 8.1 Silverlight, naleznete v toohello [Windows Phone](notification-hubs-windows-phone-push-xplat-segmented-mpns-notification.md) verze. Po dokončení budete mít možnost tooregister pro nejnovější novinky kategorií, které vás zajímají a přijímat pouze nabízená oznámení pro tyto kategorie. Tento scénář je běžný vzor velký počet aplikací, kde mají oznámení odeslaných toobe toogroups uživatelů, kteří mají zájem o je například čtečku RSS, aplikace pro Hudba ventilátory, dříve deklarované a tak dále. 

Všesměrového vysílání scénáře jsou povolené zahrnutím jeden nebo více *značky* při vytváření registrace v centru oznámení hello. Pokud jsou oznámení odesílána tooa značky, všechna zařízení, která jste zaregistrovali hello značky obdrží oznámení hello. Protože značky jsou jednoduše řetězce, nemají toobe předem zřízený. Další informace o značkách najdete v části příliš[směrování centra oznámení a značky výrazy](notification-hubs-tags-segment-push-message.md).

> [!NOTE]
> Windows Store a verze projekty Windows Phone 8.1 a starší nejsou podporovány v aplikaci Visual Studio 2017.  Další informace najdete v tématu [Cílení na platformy a kompatibilita v sadě Visual Studio 2017](https://www.visualstudio.com/en-us/productinfo/vs2017-compatibility-vs). 

## <a name="prerequisites"></a>Požadavky
Toto téma je založený na hello aplikace, které jste vytvořili v [Začínáme s Notification Hubs][get-started]. Před zahájením tohoto kurzu, musí jste již dokončili [Začínáme s Notification Hubs][get-started].

## <a name="add-category-selection-toohello-app"></a>Přidat aplikaci toohello výběru kategorie
Hello prvním krokem je tooadd hello uživatelského rozhraní elementy tooyour existující hlavní stránce umožňujících hello uživatele tooselect kategorie tooregister. kategorie Hello vybrané uživatelem se ukládají na hello zařízení. Při spuštění aplikace hello, registrace zařízení se vytvoří v centru oznámení s hello vybrané kategorie jako značky.

1. Otevřete soubor projektu MainPage.xaml hello pak kopie hello následující kód v hello **mřížky** element:
   
        <Grid>
            <Grid.RowDefinitions>
                <RowDefinition/>
                <RowDefinition/>
                <RowDefinition/>
                <RowDefinition/>
                <RowDefinition/>
            </Grid.RowDefinitions>
            <Grid.ColumnDefinitions>
                <ColumnDefinition/>
                <ColumnDefinition/>
            </Grid.ColumnDefinitions>
            <TextBlock Grid.Row="0" Grid.Column="0" Grid.ColumnSpan="2"  TextWrapping="Wrap" Text="Breaking News" FontSize="42" VerticalAlignment="Top" HorizontalAlignment="Center"/>
            <ToggleSwitch Header="World" Name="WorldToggle" Grid.Row="1" Grid.Column="0" HorizontalAlignment="Center"/>
            <ToggleSwitch Header="Politics" Name="PoliticsToggle" Grid.Row="2" Grid.Column="0" HorizontalAlignment="Center"/>
            <ToggleSwitch Header="Business" Name="BusinessToggle" Grid.Row="3" Grid.Column="0" HorizontalAlignment="Center"/>
            <ToggleSwitch Header="Technology" Name="TechnologyToggle" Grid.Row="1" Grid.Column="1" HorizontalAlignment="Center"/>
            <ToggleSwitch Header="Science" Name="ScienceToggle" Grid.Row="2" Grid.Column="1" HorizontalAlignment="Center"/>
            <ToggleSwitch Header="Sports" Name="SportsToggle" Grid.Row="3" Grid.Column="1" HorizontalAlignment="Center"/>
            <Button Name="SubscribeButton" Content="Subscribe" HorizontalAlignment="Center" Grid.Row="4" Grid.Column="0" Grid.ColumnSpan="2" Click="SubscribeButton_Click"/>
        </Grid>
2. Klikněte pravým tlačítkem na hello **sdílené** projekt a přidejte novou třídu s názvem **oznámení**, přidejte hello **veřejné** modifikátor toohello definici třídy a pak přidejte následující hello **pomocí** příkazy toohello nového souboru kódu:
   
        using Windows.Networking.PushNotifications;
        using Microsoft.WindowsAzure.Messaging;
        using Windows.Storage;
        using System.Threading.Tasks;
3. Kopírování hello následující kód do nové hello **oznámení** třídy:
   
        private NotificationHub hub;
   
        public Notifications(string hubName, string listenConnectionString)
        {
            hub = new NotificationHub(hubName, listenConnectionString);
        }
   
        public async Task<Registration> StoreCategoriesAndSubscribe(IEnumerable<string> categories)
        {
            ApplicationData.Current.LocalSettings.Values["categories"] = string.Join(",", categories);
            return await SubscribeToCategories(categories);
        }
   
        public IEnumerable<string> RetrieveCategories()
        {
            var categories = (string) ApplicationData.Current.LocalSettings.Values["categories"];
            return categories != null ? categories.Split(','): new string[0];
        }
   
        public async Task<Registration> SubscribeToCategories(IEnumerable<string> categories = null)
        {
            var channel = await PushNotificationChannelManager.CreatePushNotificationChannelForApplicationAsync();
   
            if (categories == null)
            {
                categories = RetrieveCategories();
            }
   
            // Using a template registration toosupport notifications across platforms.
            // Any template notifications that contain messageParam and a corresponding tag expression
            // will be delivered for this registration.
   
            const string templateBodyWNS = "<toast><visual><binding template=\"ToastText01\"><text id=\"1\">$(messageParam)</text></binding></visual></toast>";
   
            return await hub.RegisterTemplateAsync(channel.Uri, templateBodyWNS, "simpleWNSTemplateExample",
                    categories);
        }
   
    Tato třída se používá místní úložiště hello toostore hello kategorie zprávy, že toto zařízení má tooreceive. Všimněte si, že se namísto volání hello *RegisterNativeAsync* metoda říkáme *RegisterTemplateAsync* tooregister hello kategorií pomocí šablony registrace. 
   
    Poskytujeme také název šablony hello ("simpleWNSTemplateExample"), protože jsme chtít tooregister více než jednu šablonu (například jeden pro informační zprávy) a jeden pro dlaždice a potřebujeme tooname je v pořadí možné tooupdate toobe nebo je odstranit.
   
    Všimněte si, že pokud se zařízení zaregistruje několik šablon s hello stejná značka, příchozí zprávy cílení, že bude mít za následek značky doručit více oznámení toohello zařízení, (jeden pro každé šablony). Toto chování je užitečné, když hello stejné logické zpráva má tooresult v rámci více visual oznámení pro instanci zobrazující oznámení a oznámení v aplikaci pro Windows Store.
   
    Další informace o šablonách najdete v tématu [šablony](notification-hubs-templates-cross-platform-push-messages.md).
4. Soubor projektu App.xaml.cs hello, přidejte následující vlastnost toohello hello **aplikace** třídy:
   
        public Notifications notifications = new Notifications("<hub name>", "<connection string with listen access>");
   
    Tato vlastnost je použité toocreate a přístup **oznámení** instance.
   
    V hello výše kódu, nahraďte hello `<hub name>` a `<connection string with listen access>` zástupné symboly oznámení centra název a hello připojovacím řetězcem pro *DefaultListenSharedAccessSignature* kterou jste získali dříve.
   
   > [!NOTE]
   > Protože přihlašovací údaje, které jsou distribuované s klientskou aplikaci nejsou obecně bezpečné, musí distribuovat hello klíč pro přístup k naslouchání pouze s vaší klientské aplikace. Poslechněte umožní přístup k vaší aplikaci tooregister oznámení, ale existující registrace nemůže být upravena a nelze odeslat oznámení. Hello úplné přístupový klíč se používá ve službě Zabezpečené back-end pro zasílání oznámení a změna existující registrace.
   > 
   > 
5. V MainPage.xaml.cs přidejte následující řádek hello:
   
        using Windows.UI.Popups;
6. V souboru projektu hello MainPage.xaml.cs přidejte následující metodu hello:
   
        private async void SubscribeButton_Click(object sender, RoutedEventArgs e)
        {
            var categories = new HashSet<string>();
            if (WorldToggle.IsOn) categories.Add("World");
            if (PoliticsToggle.IsOn) categories.Add("Politics");
            if (BusinessToggle.IsOn) categories.Add("Business");
            if (TechnologyToggle.IsOn) categories.Add("Technology");
            if (ScienceToggle.IsOn) categories.Add("Science");
            if (SportsToggle.IsOn) categories.Add("Sports");
   
            var result = await ((App)Application.Current).notifications.StoreCategoriesAndSubscribe(categories);
   
            var dialog = new MessageDialog("Subscribed to: " + string.Join(",", categories) + " on registration Id: " + result.RegistrationId);
            dialog.Commands.Add(new UICommand("OK"));
            await dialog.ShowAsync();
        }
   
    Tato metoda vytvoří seznam kategorií a používá hello **oznámení** třídy toostore hello seznamu v místním úložišti hello a zaregistrujte hello značky odpovídající pomocí centra oznámení. Při změně kategorií, hello registrace se znovu vytvoří se nové kategorie hello.

Aplikace je nyní možné toostore sadu kategorií v místním úložišti na hello zařízení a zaregistrujte hello centra oznámení pokaždé, když změny uživatelů hello hello výběru kategorie.

## <a name="register-for-notifications"></a>Registrace pro oznámení
Tyto kroky zaregistrovat hello centra oznámení na spuštění pomocí hello kategorií, které byly uloženy v místním úložišti.

> [!NOTE]
> Protože hello kanál URI přiřadila hello služby oznámení Windows (WNS) můžete změnit kdykoli, byste měli zaregistrovat pro oznámení často tooavoid oznámení selhání. Tento příklad zaregistruje oznámení pokaždé, když spustí aplikaci hello. Pro aplikace, které jsou často spouštíte více než jednou denně, pravděpodobně Pokud můžete přeskočit šířky pásma toopreserve registrace od předchozí registrace hello uplynul méně než jeden den.
> 
> 

1. Otevřete hello App.xaml.cs soubor a aktualizace hello **InitNotificationsAsync** metoda toouse hello `notifications` toosubscribe třídy založené na kategoriích.
   
        // *** Remove or comment out these lines *** 
        //var channel = await PushNotificationChannelManager.CreatePushNotificationChannelForApplicationAsync();
        //var hub = new NotificationHub("your hub name", "your listen connection string");
        //var result = await hub.RegisterNativeAsync(channel.Uri);
   
        var result = await notifications.SubscribeToCategories();
   
    Tím je zajištěno, že při každém spuštění aplikace hello načte kategorie hello z místního úložiště a požadavky registrace pro tyto kategorie. Hello **InitNotificationsAsync** metoda byla vytvořena jako součást hello [Začínáme s Notification Hubs] [ get-started] kurzu.
2. V souboru projektu hello MainPage.xaml.cs přidejte následující kód toohello hello *OnNavigatedTo* metoda:
   
        protected override void OnNavigatedTo(NavigationEventArgs e)
        {
            var categories = ((App)Application.Current).notifications.RetrieveCategories();
   
            if (categories.Contains("World")) WorldToggle.IsOn = true;
            if (categories.Contains("Politics")) PoliticsToggle.IsOn = true;
            if (categories.Contains("Business")) BusinessToggle.IsOn = true;
            if (categories.Contains("Technology")) TechnologyToggle.IsOn = true;
            if (categories.Contains("Science")) ScienceToggle.IsOn = true;
            if (categories.Contains("Sports")) SportsToggle.IsOn = true;
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
   
    ![][19]
3. Odeslání nové oznámení z back-end hello v jednom z následujících způsobů hello:
   
   * **Konzolové aplikace:** spustit hello konzolovou aplikaci.
   * **Java/PHP:** spuštění vaší aplikace nebo skriptu.
     
     Oznámení pro hello vybrané kategorie se zobrazí jako informační zprávy.
     
     ![][14]

## <a name="next-steps"></a>Další kroky
V tomto kurzu jsme se dozvěděli, jak toobroadcast nejnovější zprávy přes podle kategorie. Vezměte v úvahu dokončení jednu z následujících návodů, které zvýrazněte pokročilé scénáře Notification Hubs hello:

* [Použití centra oznámení toobroadcast lokalizované novinek]
  
    Zjistěte, jak lokalizované tooexpand hello nejnovější novinky aplikace tooenable odesílání oznámení.

<!-- Anchors. -->
[Add category selection toohello app]: #adding-categories
[Register for notifications]: #register
[Send notifications from your back-end]: #send
[Run hello app and generate notifications]: #test-app
[Next Steps]: #next-steps

<!-- Images. -->
[1]: ./media/notification-hubs-windows-store-dotnet-send-breaking-news/notification-hub-breakingnews-win1.png

[14]: ./media/notification-hubs-windows-store-dotnet-send-breaking-news/notification-hub-windows-toast-2.png


[19]: ./media/notification-hubs-windows-store-dotnet-send-breaking-news/notification-hub-windows-reg-2.png

<!-- URLs.-->
[get-started]: /manage/services/notification-hubs/getting-started-windows-dotnet/
[Použití centra oznámení toobroadcast lokalizované novinek]: /manage/services/notification-hubs/breaking-news-localized-dotnet/
[Notify users with Notification Hubs]: /manage/services/notification-hubs/notify-users
[Mobile Service]: /develop/mobile/tutorials/get-started/
[Notification Hubs Guidance]: http://msdn.microsoft.com/library/jj927170.aspx
[Notification Hubs How-toofor Windows Store]: http://msdn.microsoft.com/library/jj927172.aspx
[Submit an app page]: http://go.microsoft.com/fwlink/p/?LinkID=266582
[My Applications]: http://go.microsoft.com/fwlink/p/?LinkId=262039
[Live SDK for Windows]: http://go.microsoft.com/fwlink/p/?LinkId=262253

[wns object]: http://go.microsoft.com/fwlink/p/?LinkId=260591
