---
title: "Použití služby Notification Hubs k odesílání novinek (univerzální pro Windows)"
description: "Pomocí značky v registraci použijte Azure Notification Hubs k odesílání novinek do univerzální aplikace pro Windows."
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
ms.openlocfilehash: 0e945b5626a08fcb428131f2abb465c2c141011a
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="use-notification-hubs-to-send-breaking-news"></a>Používání centra oznámení k odesílání novinek
[!INCLUDE [notification-hubs-selector-breaking-news](../../includes/notification-hubs-selector-breaking-news.md)]

## <a name="overview"></a>Přehled
Toto téma ukazuje, jak používat Azure Notification Hubs k vysílání oznámení o aktuálních zprávách do Windows Store nebo Windows Phone 8.1 (bez Silverlight) aplikace. Pokud cílíte na Windows Phone 8.1 Silverlight, získáte informace [Windows Phone](notification-hubs-windows-phone-push-xplat-segmented-mpns-notification.md) verze. Po dokončení bude moci zaregistrovat pro nejnovější novinky kategorií, které vás zajímají a přijímat pouze nabízená oznámení pro tyto kategorie. Tento scénář je běžný vzor velký počet aplikací, kde mají oznámení k odeslání do skupiny uživatelů, které jste předtím nebyl deklarovaný zájem o je například čtečku RSS, aplikace pro Hudba ventilátory a tak dále. 

Všesměrového vysílání scénáře jsou povolené zahrnutím jeden nebo více *značky* při vytváření registrace v centru oznámení. Pokud oznámení se odesílají do značku, všechna zařízení, která byla zaregistrovaná pro značku obdrží oznámení. Protože značky jsou jednoduše řetězce, nemají být předem zřízená. Další informace o značkách najdete v části [směrování centra oznámení a značky výrazy](notification-hubs-tags-segment-push-message.md).

> [!NOTE]
> Windows Store a verze projekty Windows Phone 8.1 a starší nejsou podporovány v aplikaci Visual Studio 2017.  Další informace najdete v tématu [Cílení na platformy a kompatibilita v sadě Visual Studio 2017](https://www.visualstudio.com/en-us/productinfo/vs2017-compatibility-vs). 

## <a name="prerequisites"></a>Požadavky
Toto téma je založený na aplikaci, kterou jste vytvořili v [Začínáme s Notification Hubs][get-started]. Před zahájením tohoto kurzu, musí jste již dokončili [Začínáme s Notification Hubs][get-started].

## <a name="add-category-selection-to-the-app"></a>Přidat výběru kategorie do aplikace
Prvním krokem je přidání prvky uživatelského rozhraní na existující stránku pro hlavní, který uživateli umožňuje výběr kategorií k registraci. Kategorie, které uživatel jsou uloženy v zařízení. Při spuštění aplikace registrace zařízení se vytvoří v centru oznámení s vybrané kategorie jako značky.

1. Otevřete soubor projektu MainPage.xaml a pak zkopírujte následující kód v **mřížky** element:
   
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
2. Klikněte pravým tlačítkem **sdílené** projekt a přidejte novou třídu s názvem **oznámení**, přidejte **veřejné** modifikátor v definici třídy, přidejte následující  **pomocí** příkazy na nový soubor kódu:
   
        using Windows.Networking.PushNotifications;
        using Microsoft.WindowsAzure.Messaging;
        using Windows.Storage;
        using System.Threading.Tasks;
3. Zkopírujte následující kód do nové **oznámení** třídy:
   
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
   
            // Using a template registration to support notifications across platforms.
            // Any template notifications that contain messageParam and a corresponding tag expression
            // will be delivered for this registration.
   
            const string templateBodyWNS = "<toast><visual><binding template=\"ToastText01\"><text id=\"1\">$(messageParam)</text></binding></visual></toast>";
   
            return await hub.RegisterTemplateAsync(channel.Uri, templateBodyWNS, "simpleWNSTemplateExample",
                    categories);
        }
   
    Tato třída používá místní úložiště k ukládání kategorie příspěvků, který toto zařízení má přijmout. Všimněte si, že se namísto volání *RegisterNativeAsync* metoda říkáme *RegisterTemplateAsync* registrovat kategorie pomocí šablony registrace. 
   
    Poskytujeme také název šablony ("simpleWNSTemplateExample"), protože jsme chtít zaregistrovat více než jednu šablonu (například jeden pro informační zprávy) a jeden pro dlaždice a musíme název je, aby bylo možné aktualizovat nebo odstranit.
   
    Všimněte si, že pokud se zařízení zaregistruje několik šablon se stejnou značkou, příchozí zprávy cílení, značka bude mít za následek více oznámení doručit do zařízení (jeden pro každé šablony). Toto chování je užitečné, když stejné logické zpráva má za následek více visual oznámení, například zobrazující oznámení a oznámení v aplikaci pro Windows Store.
   
    Další informace o šablonách najdete v tématu [šablony](notification-hubs-templates-cross-platform-push-messages.md).
4. V souboru projektu App.xaml.cs přidejte následující vlastnosti, která má **aplikace** třídy:
   
        public Notifications notifications = new Notifications("<hub name>", "<connection string with listen access>");
   
    Tato vlastnost slouží k vytváření a přístup **oznámení** instance.
   
    Ve výše uvedeném kódu nahraďte `<hub name>` a `<connection string with listen access>` zástupné symboly pomocí názvu centra oznámení a připojovacího řetězce pro *DefaultListenSharedAccessSignature* kterou jste získali dříve.
   
   > [!NOTE]
   > Protože přihlašovací údaje, které jsou distribuované s klientskou aplikaci není obvykle zabezpečení, by měl distribuovat klíč pro naslouchání přístup pouze s vaší klientské aplikace. Poslechněte umožní přístup k aplikaci zaregistrovat pro oznámení, ale existující registrace nemůže být upravena a nelze odeslat oznámení. Úplný přístup klíč se používá ve službě Zabezpečené back-end pro zasílání oznámení a změna existující registrace.
   > 
   > 
5. V MainPage.xaml.cs přidejte následující řádek:
   
        using Windows.UI.Popups;
6. V souboru projektu MainPage.xaml.cs přidejte následující metodu:
   
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
   
    Tato metoda vytvoří seznam kategorií a používá **oznámení** značky třída pro uložení seznamu v místním úložišti a zaregistrujte odpovídající pomocí centra oznámení. Při změně kategorií, registrace se znovu vytvoří se nové kategorie.

Aplikace je teď možné uložit sadu kategorií místní úložiště v zařízení a zaregistrovat do centra oznámení pokaždé, když uživatel změní výběr kategorie.

## <a name="register-for-notifications"></a>Registrace pro oznámení
Tyto kroky zaregistrovat do centra oznámení na spouštění pomocí kategorií, které byly uloženy v místním úložišti.

> [!NOTE]
> Vzhledem k tomu, že kanál URI přiřazené pomocí služby oznámení Windows (WNS) můžete změnit kdykoli, byste měli zaregistrovat pro oznámení často, aby se zabránilo selhání oznámení. V tomto příkladu se zaregistruje pro oznámení při každém spuštění aplikace. Pro aplikace, které jsou často spouštíte více než jednou denně, můžete pravděpodobně přeskočit registraci byla zachována šířka pásma, pokud od předchozí registrace uplynul méně než jeden den.
> 
> 

1. Otevřete soubor App.xaml.cs a aktualizace **InitNotificationsAsync** metoda se má použít `notifications` k odběru na základě kategorií.
   
        // *** Remove or comment out these lines *** 
        //var channel = await PushNotificationChannelManager.CreatePushNotificationChannelForApplicationAsync();
        //var hub = new NotificationHub("your hub name", "your listen connection string");
        //var result = await hub.RegisterNativeAsync(channel.Uri);
   
        var result = await notifications.SubscribeToCategories();
   
    Tím je zajištěno, že při každém spuštění aplikace načte kategorie z místního úložiště a požadavky registrace pro tyto kategorie. **InitNotificationsAsync** metoda byla vytvořena jako součást [Začínáme s Notification Hubs] [ get-started] kurzu.
2. V souboru projektu MainPage.xaml.cs, přidejte následující kód, který *OnNavigatedTo* metoda:
   
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
   
    Tím se aktualizuje na základě stavu dříve uloženou kategorií hlavní stránce.

Aplikace je nyní dokončen a sadu kategorií můžete uložit do místního úložiště zařízení používá k registraci do centra oznámení pokaždé, když uživatel změní výběr kategorie. V dalším kroku bude definujeme back-end, který kategorie oznámení můžete odesílat do této aplikace.

## <a name="sending-tagged-notifications"></a>Odesílání oznámení s příznakem
[!INCLUDE [notification-hubs-send-categories-template](../../includes/notification-hubs-send-categories-template.md)]

## <a name="run-the-app-and-generate-notifications"></a>Spusťte aplikaci a generovat upozornění
1. Ve Visual Studiu stisknutím klávesy F5 zkompilování a spuštění aplikace.
   
    ![][1]
   
    Všimněte si, že aplikace uživatelského rozhraní, poskytuje sadu přepínačů, která vám umožní vybrat kategorie pro přihlášení k odběru.
2. Povolit jednu nebo více kategorií přepínačů a potom klikněte na **přihlásit k odběru**.
   
    Aplikace převede vybraných kategorií značky a požaduje novou registraci zařízení pro vybranou značky z centra oznámení. Registrovaný kategorie se vrátí a zobrazí v dialogu.
   
    ![][19]
3. Odeslání nové oznámení z back-end v jednom z následujících způsobů:
   
   * **Konzolové aplikace:** Spusťte konzolovou aplikaci.
   * **Java/PHP:** spuštění vaší aplikace nebo skriptu.
     
     Oznámení pro vybrané kategorie se zobrazí jako informační zprávy.
     
     ![][14]

## <a name="next-steps"></a>Další kroky
V tomto kurzu jsme zjistili, jak k vysílání novinek podle kategorie. Vezměte v úvahu dokončení jednu z následujících kurzů upozorňující na jiné pokročilé scénáře centra oznámení:

* [Použití centra oznámení k vysílání lokalizované novinek]
  
    Zjistěte, jak rozšířit aplikace nejnovější zprávy k povolení odesílání lokalizované upozornění.

<!-- Anchors. -->
[Add category selection to the app]: #adding-categories
[Register for notifications]: #register
[Send notifications from your back-end]: #send
[Run the app and generate notifications]: #test-app
[Next Steps]: #next-steps

<!-- Images. -->
[1]: ./media/notification-hubs-windows-store-dotnet-send-breaking-news/notification-hub-breakingnews-win1.png

[14]: ./media/notification-hubs-windows-store-dotnet-send-breaking-news/notification-hub-windows-toast-2.png


[19]: ./media/notification-hubs-windows-store-dotnet-send-breaking-news/notification-hub-windows-reg-2.png

<!-- URLs.-->
[get-started]: /manage/services/notification-hubs/getting-started-windows-dotnet/
[Použití centra oznámení k vysílání lokalizované novinek]: /manage/services/notification-hubs/breaking-news-localized-dotnet/
[Notify users with Notification Hubs]: /manage/services/notification-hubs/notify-users
[Mobile Service]: /develop/mobile/tutorials/get-started/
[Notification Hubs Guidance]: http://msdn.microsoft.com/library/jj927170.aspx
[Notification Hubs How-To for Windows Store]: http://msdn.microsoft.com/library/jj927172.aspx
[Submit an app page]: http://go.microsoft.com/fwlink/p/?LinkID=266582
[My Applications]: http://go.microsoft.com/fwlink/p/?LinkId=262039
[Live SDK for Windows]: http://go.microsoft.com/fwlink/p/?LinkId=262253

[wns object]: http://go.microsoft.com/fwlink/p/?LinkId=260591
