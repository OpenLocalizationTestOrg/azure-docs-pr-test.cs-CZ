---
title: "aaaNotification lokalizované nejnovější novinky kurz Hubs"
description: "Zjistěte, jak Azure Notification Hubs toosend toouse lokalizované oznámení o aktuálních zprávách."
services: notification-hubs
documentationcenter: windows
author: ysxu
manager: erikre
editor: 
ms.assetid: c454f5a3-a06b-45ac-91c7-f91210889b25
ms.service: notification-hubs
ms.workload: mobile
ms.tgt_pltfrm: mobile-windows
ms.devlang: dotnet
ms.topic: article
ms.date: 06/29/2016
ms.author: yuaxu
ms.openlocfilehash: d273a6b384df311dea7b76ca83ccd94d9a989c4e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="use-notification-hubs-toosend-localized-breaking-news"></a>Použití centra oznámení toosend lokalizované novinek
> [!div class="op_single_selector"]
> * [Windows Store jazyka C#](notification-hubs-windows-store-dotnet-xplat-localized-wns-push-notification.md)
> * [iOS](notification-hubs-ios-xplat-localized-apns-push-notification.md)
> 
> 

## <a name="overview"></a>Přehled
Toto téma ukazuje, jak toouse hello **šablony** funkce Azure Notification Hubs toobroadcast nejnovější zprávy oznámení, které lokalizované podle jazyka a zařízení. V tomto kurzu začnete s vytvořené v aplikaci Windows Store hello [toosend použití centra oznámení nejnovější zprávy přes]. Po dokončení, že bude možné tooregister kategorií, které vás zajímají, zadejte jazyk v oznámení, která tooreceive hello a přijímat pouze nabízená oznámení pro hello vybrané kategorie v daném jazyce.

Existují dva scénáře toothis částí:

* aplikace pro Windows Store Hello umožňuje klienta zařízení toospecify jazyk a toosubscribe toodifferent nejnovější novinky kategorií;
* Hello back-end vysílá hello oznámení, pomocí hello **značka** a **šablony** feautres Azure Notification Hubs.

## <a name="prerequisites"></a>Požadavky
Musí jste již dokončili hello [toosend použití centra oznámení nejnovější zprávy přes] kurz a mít hello kód je k dispozici, protože v tomto kurzu staví přímo na tento kód.

Také musíte Visual Studio 2012 nebo novějším.

## <a name="template-concepts"></a>Koncepty šablon
V [toosend použití centra oznámení nejnovější zprávy přes] jste vytvořili aplikaci, která používá **značky** toosubscribe toonotifications pro různé zprávy kategorie.
Velký počet aplikací, ale cíli více trhů a vyžadují lokalizace. To znamená, že mají toobe lokalizovaný obsah hello hello oznámení, sami a doručené toohello opravte sadu zařízení.
V tomto tématu ukážeme, jak toouse hello **šablony** lokalizované funkce centra oznámení tooeasily doručování oznámení o aktuálních zprávách.

Poznámka: jedním ze způsobů toosend lokalizované oznámení je toocreate více verzí jednotlivé značky. Například toosupport angličtinu, francouzštinu a Mandarínština, musíme tří různých značek pro world zprávy: "world_en", "world_fr" a "world_ch". Pak nám toosend lokalizované verzi hello world zprávy tooeach tyto značky. V tomto tématu používáme šablony tooavoid hello, jak narůstá počet značek a hello požadavek odeslat více zpráv.

Na vysoké úrovni, šablony jsou toospecify způsob jak určité zařízení měli obdržet oznámení. Šablona Hello Určuje formát datové části přesně hello tím, že odkazuje tooproperties, které jsou součástí uvítací zprávu poslal váš back-end aplikace. V našem případě pošleme zprávu bez ohledu na národním prostředí obsahující všechny podporované jazyky:

    {
        "News_English": "...",
        "News_French": "...",
        "News_Mandarin": "..."
    }

Potom jsme zajistí, že zařízení zaregistrovat pomocí šablony, která odkazuje vlastnost toohello správné. Například aplikace pro Windows Store, který chce tooreceive bude zaregistrujte jednoduché informační zprávu pro hello následující šablonu s všechny odpovídající značky:

    <toast>
      <visual>
        <binding template=\"ToastText01\">
          <text id=\"1\">$(News_English)</text>
        </binding>
      </visual>
    </toast>



Šablony jsou velmi výkonné funkce, můžete další informace o možnostech v našem [šablony](notification-hubs-templates-cross-platform-push-messages.md) článku. 

## <a name="hello-app-user-interface"></a>uživatelské rozhraní aplikace Hello
Nyní jsme upraví hello novinkách aplikaci, kterou jste vytvořili v tématu hello [toosend použití centra oznámení nejnovější zprávy přes] toosend lokalizované novinek pomocí šablon.

V aplikaci Windows Store:

Změna vaše MainPage.xaml tooinclude combobox národního prostředí:

    <Grid Margin="120, 58, 120, 80"  
            Background="{StaticResource ApplicationPageBackgroundThemeBrush}">
        <Grid.RowDefinitions>
            <RowDefinition />
            <RowDefinition />
            <RowDefinition />
            <RowDefinition />
            <RowDefinition />
            <RowDefinition />
        </Grid.RowDefinitions>
        <Grid.ColumnDefinitions>
            <ColumnDefinition />
            <ColumnDefinition />
        </Grid.ColumnDefinitions>
        <TextBlock Grid.Row="0" Grid.Column="0" Grid.ColumnSpan="2"  TextWrapping="Wrap" Text="Breaking News" FontSize="42" VerticalAlignment="Top"/>
        <ComboBox Name="Locale" HorizontalAlignment="Left" VerticalAlignment="Center" Width="200" Grid.Row="1" Grid.Column="0">
            <x:String>English</x:String>
            <x:String>French</x:String>
            <x:String>Mandarin</x:String>
        </ComboBox>
        <ToggleSwitch Header="World" Name="WorldToggle" Grid.Row="2" Grid.Column="0"/>
        <ToggleSwitch Header="Politics" Name="PoliticsToggle" Grid.Row="3" Grid.Column="0"/>
        <ToggleSwitch Header="Business" Name="BusinessToggle" Grid.Row="4" Grid.Column="0"/>
        <ToggleSwitch Header="Technology" Name="TechnologyToggle" Grid.Row="2" Grid.Column="1"/>
        <ToggleSwitch Header="Science" Name="ScienceToggle" Grid.Row="3" Grid.Column="1"/>
        <ToggleSwitch Header="Sports" Name="SportsToggle" Grid.Row="4" Grid.Column="1"/>
        <Button Content="Subscribe" HorizontalAlignment="Center" Grid.Row="5" Grid.Column="0" Grid.ColumnSpan="2" Click="SubscribeButton_Click" />
    </Grid>

## <a name="building-hello-windows-store-client-app"></a>Vytváření hello klientskou aplikaci pro Windows Store
1. Ve třídě oznámení přidat tooyour parametr národního prostředí *StoreCategoriesAndSubscribe* a *SubscribeToCateories* metody.
   
        public async Task<Registration> StoreCategoriesAndSubscribe(string locale, IEnumerable<string> categories)
        {
            ApplicationData.Current.LocalSettings.Values["categories"] = string.Join(",", categories);
            ApplicationData.Current.LocalSettings.Values["locale"] = locale;
            return await SubscribeToCategories(categories);
        }
   
        public async Task<Registration> SubscribeToCategories(string locale, IEnumerable<string> categories = null)
        {
            var channel = await PushNotificationChannelManager.CreatePushNotificationChannelForApplicationAsync();
   
            if (categories == null)
            {
                categories = RetrieveCategories();
            }
   
            // Using a template registration. This makes supporting notifications across other platforms much easier.
            // Using hello localized tags based on locale selected.
            string templateBodyWNS = String.Format("<toast><visual><binding template=\"ToastText01\"><text id=\"1\">$(News_{0})</text></binding></visual></toast>", locale);
   
            return await hub.RegisterTemplateAsync(channel.Uri, templateBodyWNS, "localizedWNSTemplateExample", categories);
        }
   
    Všimněte si, že se namísto volání hello *RegisterNativeAsync* metoda říkáme *RegisterTemplateAsync*: jsme registraci konkrétní oznámení formát, ve které hello šablony závisí na hello národního prostředí. Poskytujeme také název šablony hello ("localizedWNSTemplateExample"), protože jsme chtít tooregister více než jednu šablonu (například jeden pro informační zprávy) a jeden pro dlaždice a potřebujeme tooname je v pořadí možné tooupdate toobe nebo je odstranit.
   
    Všimněte si, že pokud se zařízení zaregistruje několik šablon s hello stejná značka, příchozí zprávy cílení, že bude mít za následek značky doručit více oznámení toohello zařízení, (jeden pro každé šablony). Toto chování je užitečné, když hello stejné logické zpráva má tooresult v rámci více visual oznámení pro instanci zobrazující oznámení a oznámení v aplikaci pro Windows Store.
2. Přidejte následující národní prostředí metoda tooretrieve hello uložené hello:
   
        public string RetrieveLocale()
        {
            var locale = (string) ApplicationData.Current.LocalSettings.Values["locale"];
            return locale != null ? locale : "English";
        }
3. V MainPage.xaml.cs, aktualizovat vaše tlačítko načítání hello aktuální hodnota hello národní prostředí – pole se seznamem a jeho jak je znázorněno poskytnutím třídy toohello volání toohello oznámení, klikněte na obslužnou rutinu:
   
        private async void SubscribeButton_Click(object sender, RoutedEventArgs e)
        {
            var locale = (string)Locale.SelectedItem;
   
            var categories = new HashSet<string>();
            if (WorldToggle.IsOn) categories.Add("World");
            if (PoliticsToggle.IsOn) categories.Add("Politics");
            if (BusinessToggle.IsOn) categories.Add("Business");
            if (TechnologyToggle.IsOn) categories.Add("Technology");
            if (ScienceToggle.IsOn) categories.Add("Science");
            if (SportsToggle.IsOn) categories.Add("Sports");
   
            var result = await ((App)Application.Current).notifications.StoreCategoriesAndSubscribe(locale,
                 categories);
   
            var dialog = new MessageDialog("Locale: " + locale + " Subscribed to: " + 
                string.Join(",", categories) + " on registration Id: " + result.RegistrationId);
            dialog.Commands.Add(new UICommand("OK"));
            await dialog.ShowAsync();
        }
4. Nakonec v souboru App.xaml.cs, ujistěte se, že tooupdate vaše `InitNotificationsAsync` metoda tooretrieve hello národního prostředí a použít ho při přihlášení k odběru:
   
        private async void InitNotificationsAsync()
        {
            var result = await notifications.SubscribeToCategories(notifications.RetrieveLocale());
   
            // Displays hello registration ID so you know it was successful
            if (result.RegistrationId != null)
            {
                var dialog = new MessageDialog("Registration successful: " + result.RegistrationId);
                dialog.Commands.Add(new UICommand("OK"));
                await dialog.ShowAsync();
            }
        }

## <a name="send-localized-notifications-from-your-back-end"></a>Odesílání lokalizované oznámení z back-end
[!INCLUDE [notification-hubs-localized-back-end](../../includes/notification-hubs-localized-back-end.md)]

<!-- Anchors. -->
[Template concepts]: #concepts
[hello app user interface]: #ui
[Building hello Windows Store client app]: #building-client
[Send notifications from your back-end]: #send
[Next Steps]:#next-steps

<!-- Images. -->

<!-- URLs. -->
[Mobile Service]: /develop/mobile/tutorials/get-started
[Notify users with Notification Hubs: ASP.NET]: /manage/services/notification-hubs/notify-users-aspnet
[Notify users with Notification Hubs: Mobile Services]: /manage/services/notification-hubs/notify-users
[toosend použití centra oznámení nejnovější zprávy přes]: /manage/services/notification-hubs/breaking-news-dotnet

[Submit an app page]: http://go.microsoft.com/fwlink/p/?LinkID=266582
[My Applications]: http://go.microsoft.com/fwlink/p/?LinkId=262039
[Live SDK for Windows]: http://go.microsoft.com/fwlink/p/?LinkId=262253
[Get started with Mobile Services]: /develop/mobile/tutorials/get-started/#create-new-service
[Get started with data]: /develop/mobile/tutorials/get-started-with-data-dotnet
[Get started with authentication]: /develop/mobile/tutorials/get-started-with-users-dotnet
[Get started with push notifications]: /develop/mobile/tutorials/get-started-with-push-dotnet
[Push notifications tooapp users]: /develop/mobile/tutorials/push-notifications-to-app-users-dotnet
[Authorize users with scripts]: /develop/mobile/tutorials/authorize-users-in-scripts-dotnet
[JavaScript and HTML]: /develop/mobile/tutorials/get-started-with-push-js

[wns object]: http://go.microsoft.com/fwlink/p/?LinkId=260591
[Notification Hubs Guidance]: http://msdn.microsoft.com/library/jj927170.aspx
[Notification Hubs How-toofor iOS]: http://msdn.microsoft.com/library/jj927168.aspx
[Notification Hubs How-toofor Windows Store]: http://msdn.microsoft.com/library/jj927172.aspx
