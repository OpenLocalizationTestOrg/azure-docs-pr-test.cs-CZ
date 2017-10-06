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
# <a name="use-notification-hubs-toosend-localized-breaking-news"></a><span data-ttu-id="66090-103">Použití centra oznámení toosend lokalizované novinek</span><span class="sxs-lookup"><span data-stu-id="66090-103">Use Notification Hubs toosend localized breaking news</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="66090-104">Windows Store jazyka C#</span><span class="sxs-lookup"><span data-stu-id="66090-104">Windows Store C#</span></span>](notification-hubs-windows-store-dotnet-xplat-localized-wns-push-notification.md)
> * [<span data-ttu-id="66090-105">iOS</span><span class="sxs-lookup"><span data-stu-id="66090-105">iOS</span></span>](notification-hubs-ios-xplat-localized-apns-push-notification.md)
> 
> 

## <a name="overview"></a><span data-ttu-id="66090-106">Přehled</span><span class="sxs-lookup"><span data-stu-id="66090-106">Overview</span></span>
<span data-ttu-id="66090-107">Toto téma ukazuje, jak toouse hello **šablony** funkce Azure Notification Hubs toobroadcast nejnovější zprávy oznámení, které lokalizované podle jazyka a zařízení.</span><span class="sxs-lookup"><span data-stu-id="66090-107">This topic shows you how toouse hello **template** feature of Azure Notification Hubs toobroadcast breaking news notifications that have been localized by language and device.</span></span> <span data-ttu-id="66090-108">V tomto kurzu začnete s vytvořené v aplikaci Windows Store hello [toosend použití centra oznámení nejnovější zprávy přes].</span><span class="sxs-lookup"><span data-stu-id="66090-108">In this tutorial you start with hello Windows Store app created in [Use Notification Hubs toosend breaking news].</span></span> <span data-ttu-id="66090-109">Po dokončení, že bude možné tooregister kategorií, které vás zajímají, zadejte jazyk v oznámení, která tooreceive hello a přijímat pouze nabízená oznámení pro hello vybrané kategorie v daném jazyce.</span><span class="sxs-lookup"><span data-stu-id="66090-109">When complete, you will be able tooregister for categories you are interested in, specify a language in which tooreceive hello notifications, and receive only push notifications for hello selected categories in that language.</span></span>

<span data-ttu-id="66090-110">Existují dva scénáře toothis částí:</span><span class="sxs-lookup"><span data-stu-id="66090-110">There are two parts toothis scenario:</span></span>

* <span data-ttu-id="66090-111">aplikace pro Windows Store Hello umožňuje klienta zařízení toospecify jazyk a toosubscribe toodifferent nejnovější novinky kategorií;</span><span class="sxs-lookup"><span data-stu-id="66090-111">hello Windows Store app allows client devices toospecify a language, and toosubscribe toodifferent breaking news categories;</span></span>
* <span data-ttu-id="66090-112">Hello back-end vysílá hello oznámení, pomocí hello **značka** a **šablony** feautres Azure Notification Hubs.</span><span class="sxs-lookup"><span data-stu-id="66090-112">hello back-end broadcasts hello notifications, using hello **tag** and **template** feautres of Azure Notification Hubs.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="66090-113">Požadavky</span><span class="sxs-lookup"><span data-stu-id="66090-113">Prerequisites</span></span>
<span data-ttu-id="66090-114">Musí jste již dokončili hello [toosend použití centra oznámení nejnovější zprávy přes] kurz a mít hello kód je k dispozici, protože v tomto kurzu staví přímo na tento kód.</span><span class="sxs-lookup"><span data-stu-id="66090-114">You must have already completed hello [Use Notification Hubs toosend breaking news] tutorial and have hello code available, because this tutorial builds directly upon that code.</span></span>

<span data-ttu-id="66090-115">Také musíte Visual Studio 2012 nebo novějším.</span><span class="sxs-lookup"><span data-stu-id="66090-115">You also need Visual Studio 2012 or later.</span></span>

## <a name="template-concepts"></a><span data-ttu-id="66090-116">Koncepty šablon</span><span class="sxs-lookup"><span data-stu-id="66090-116">Template concepts</span></span>
<span data-ttu-id="66090-117">V [toosend použití centra oznámení nejnovější zprávy přes] jste vytvořili aplikaci, která používá **značky** toosubscribe toonotifications pro různé zprávy kategorie.</span><span class="sxs-lookup"><span data-stu-id="66090-117">In [Use Notification Hubs toosend breaking news] you built an app that used **tags** toosubscribe toonotifications for different news categories.</span></span>
<span data-ttu-id="66090-118">Velký počet aplikací, ale cíli více trhů a vyžadují lokalizace.</span><span class="sxs-lookup"><span data-stu-id="66090-118">Many apps, however, target multiple markets and require localization.</span></span> <span data-ttu-id="66090-119">To znamená, že mají toobe lokalizovaný obsah hello hello oznámení, sami a doručené toohello opravte sadu zařízení.</span><span class="sxs-lookup"><span data-stu-id="66090-119">This means that hello content of hello notifications themselves have toobe localized and delivered toohello correct set of devices.</span></span>
<span data-ttu-id="66090-120">V tomto tématu ukážeme, jak toouse hello **šablony** lokalizované funkce centra oznámení tooeasily doručování oznámení o aktuálních zprávách.</span><span class="sxs-lookup"><span data-stu-id="66090-120">In this topic we will show how toouse hello **template** feature of Notification Hubs tooeasily deliver localized breaking news notifications.</span></span>

<span data-ttu-id="66090-121">Poznámka: jedním ze způsobů toosend lokalizované oznámení je toocreate více verzí jednotlivé značky.</span><span class="sxs-lookup"><span data-stu-id="66090-121">Note: one way toosend localized notifications is toocreate multiple versions of each tag.</span></span> <span data-ttu-id="66090-122">Například toosupport angličtinu, francouzštinu a Mandarínština, musíme tří různých značek pro world zprávy: "world_en", "world_fr" a "world_ch".</span><span class="sxs-lookup"><span data-stu-id="66090-122">For instance, toosupport English, French, and Mandarin, we would need three different tags for world news: "world_en", "world_fr", and "world_ch".</span></span> <span data-ttu-id="66090-123">Pak nám toosend lokalizované verzi hello world zprávy tooeach tyto značky.</span><span class="sxs-lookup"><span data-stu-id="66090-123">We would then have toosend a localized version of hello world news tooeach of these tags.</span></span> <span data-ttu-id="66090-124">V tomto tématu používáme šablony tooavoid hello, jak narůstá počet značek a hello požadavek odeslat více zpráv.</span><span class="sxs-lookup"><span data-stu-id="66090-124">In this topic we use templates tooavoid hello proliferation of tags and hello requirement of sending multiple messages.</span></span>

<span data-ttu-id="66090-125">Na vysoké úrovni, šablony jsou toospecify způsob jak určité zařízení měli obdržet oznámení.</span><span class="sxs-lookup"><span data-stu-id="66090-125">At a high level, templates are a way toospecify how a specific device should receive a notification.</span></span> <span data-ttu-id="66090-126">Šablona Hello Určuje formát datové části přesně hello tím, že odkazuje tooproperties, které jsou součástí uvítací zprávu poslal váš back-end aplikace.</span><span class="sxs-lookup"><span data-stu-id="66090-126">hello template specifies hello exact payload format by referring tooproperties that are part of hello message sent by your app back-end.</span></span> <span data-ttu-id="66090-127">V našem případě pošleme zprávu bez ohledu na národním prostředí obsahující všechny podporované jazyky:</span><span class="sxs-lookup"><span data-stu-id="66090-127">In our case, we will send a locale-agnostic message containing all supported languages:</span></span>

    {
        "News_English": "...",
        "News_French": "...",
        "News_Mandarin": "..."
    }

<span data-ttu-id="66090-128">Potom jsme zajistí, že zařízení zaregistrovat pomocí šablony, která odkazuje vlastnost toohello správné.</span><span class="sxs-lookup"><span data-stu-id="66090-128">Then we will ensure that devices register with a template that refers toohello correct property.</span></span> <span data-ttu-id="66090-129">Například aplikace pro Windows Store, který chce tooreceive bude zaregistrujte jednoduché informační zprávu pro hello následující šablonu s všechny odpovídající značky:</span><span class="sxs-lookup"><span data-stu-id="66090-129">For instance, a Windows Store app that wants tooreceive a simple toast message will register for hello following template with any corresponding tags:</span></span>

    <toast>
      <visual>
        <binding template=\"ToastText01\">
          <text id=\"1\">$(News_English)</text>
        </binding>
      </visual>
    </toast>



<span data-ttu-id="66090-130">Šablony jsou velmi výkonné funkce, můžete další informace o možnostech v našem [šablony](notification-hubs-templates-cross-platform-push-messages.md) článku.</span><span class="sxs-lookup"><span data-stu-id="66090-130">Templates are a very powerful feature you can learn more about in our [Templates](notification-hubs-templates-cross-platform-push-messages.md) article.</span></span> 

## <a name="hello-app-user-interface"></a><span data-ttu-id="66090-131">uživatelské rozhraní aplikace Hello</span><span class="sxs-lookup"><span data-stu-id="66090-131">hello app user interface</span></span>
<span data-ttu-id="66090-132">Nyní jsme upraví hello novinkách aplikaci, kterou jste vytvořili v tématu hello [toosend použití centra oznámení nejnovější zprávy přes] toosend lokalizované novinek pomocí šablon.</span><span class="sxs-lookup"><span data-stu-id="66090-132">We will now modify hello Breaking News app that you created in hello topic [Use Notification Hubs toosend breaking news] toosend localized breaking news using templates.</span></span>

<span data-ttu-id="66090-133">V aplikaci Windows Store:</span><span class="sxs-lookup"><span data-stu-id="66090-133">In your Windows Store app:</span></span>

<span data-ttu-id="66090-134">Změna vaše MainPage.xaml tooinclude combobox národního prostředí:</span><span class="sxs-lookup"><span data-stu-id="66090-134">Change your MainPage.xaml tooinclude a locale combobox:</span></span>

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

## <a name="building-hello-windows-store-client-app"></a><span data-ttu-id="66090-135">Vytváření hello klientskou aplikaci pro Windows Store</span><span class="sxs-lookup"><span data-stu-id="66090-135">Building hello Windows Store client app</span></span>
1. <span data-ttu-id="66090-136">Ve třídě oznámení přidat tooyour parametr národního prostředí *StoreCategoriesAndSubscribe* a *SubscribeToCateories* metody.</span><span class="sxs-lookup"><span data-stu-id="66090-136">In your Notifications class, add a locale parameter tooyour  *StoreCategoriesAndSubscribe* and *SubscribeToCateories* methods.</span></span>
   
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
   
    <span data-ttu-id="66090-137">Všimněte si, že se namísto volání hello *RegisterNativeAsync* metoda říkáme *RegisterTemplateAsync*: jsme registraci konkrétní oznámení formát, ve které hello šablony závisí na hello národního prostředí.</span><span class="sxs-lookup"><span data-stu-id="66090-137">Note that instead of calling hello *RegisterNativeAsync* method we call *RegisterTemplateAsync*: we are registering a specific notification format in which hello template depends on hello locale.</span></span> <span data-ttu-id="66090-138">Poskytujeme také název šablony hello ("localizedWNSTemplateExample"), protože jsme chtít tooregister více než jednu šablonu (například jeden pro informační zprávy) a jeden pro dlaždice a potřebujeme tooname je v pořadí možné tooupdate toobe nebo je odstranit.</span><span class="sxs-lookup"><span data-stu-id="66090-138">We also provide a name for hello template ("localizedWNSTemplateExample"), because we might want tooregister more than one template (for instance one for toast notifications and one for tiles) and we need tooname them in order toobe able tooupdate or delete them.</span></span>
   
    <span data-ttu-id="66090-139">Všimněte si, že pokud se zařízení zaregistruje několik šablon s hello stejná značka, příchozí zprávy cílení, že bude mít za následek značky doručit více oznámení toohello zařízení, (jeden pro každé šablony).</span><span class="sxs-lookup"><span data-stu-id="66090-139">Note that if a device registers multiple templates with hello same tag, an incoming message targeting that tag will result in multiple notifications delivered toohello device (one for each template).</span></span> <span data-ttu-id="66090-140">Toto chování je užitečné, když hello stejné logické zpráva má tooresult v rámci více visual oznámení pro instanci zobrazující oznámení a oznámení v aplikaci pro Windows Store.</span><span class="sxs-lookup"><span data-stu-id="66090-140">This behavior is useful when hello same logical message has tooresult in multiple visual notifications, for instance showing both a badge and a toast in a Windows Store application.</span></span>
2. <span data-ttu-id="66090-141">Přidejte následující národní prostředí metoda tooretrieve hello uložené hello:</span><span class="sxs-lookup"><span data-stu-id="66090-141">Add hello following method tooretrieve hello stored locale:</span></span>
   
        public string RetrieveLocale()
        {
            var locale = (string) ApplicationData.Current.LocalSettings.Values["locale"];
            return locale != null ? locale : "English";
        }
3. <span data-ttu-id="66090-142">V MainPage.xaml.cs, aktualizovat vaše tlačítko načítání hello aktuální hodnota hello národní prostředí – pole se seznamem a jeho jak je znázorněno poskytnutím třídy toohello volání toohello oznámení, klikněte na obslužnou rutinu:</span><span class="sxs-lookup"><span data-stu-id="66090-142">In your MainPage.xaml.cs, update your button click handler by retrieving hello current value of hello Locale combo box and providing it toohello call toohello Notifications class, as shown:</span></span>
   
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
4. <span data-ttu-id="66090-143">Nakonec v souboru App.xaml.cs, ujistěte se, že tooupdate vaše `InitNotificationsAsync` metoda tooretrieve hello národního prostředí a použít ho při přihlášení k odběru:</span><span class="sxs-lookup"><span data-stu-id="66090-143">Finally, in your App.xaml.cs file, make sure tooupdate your `InitNotificationsAsync` method tooretrieve hello locale and use it when subscribing:</span></span>
   
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

## <a name="send-localized-notifications-from-your-back-end"></a><span data-ttu-id="66090-144">Odesílání lokalizované oznámení z back-end</span><span class="sxs-lookup"><span data-stu-id="66090-144">Send localized notifications from your back-end</span></span>
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
