---
title: "Lokalizované nejnovější novinky kurzu centra oznámení"
description: "Zjistěte, jak používat Azure Notification Hubs k odesílání oznámení o lokalizované aktuálních zprávách."
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
ms.openlocfilehash: e864e832b4c50644bf4062dee29d34ff9fe2774e
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="use-notification-hubs-to-send-localized-breaking-news"></a><span data-ttu-id="db348-103">Použití centra oznámení k odesílání novinek lokalizované</span><span class="sxs-lookup"><span data-stu-id="db348-103">Use Notification Hubs to send localized breaking news</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="db348-104">Windows Store jazyka C#</span><span class="sxs-lookup"><span data-stu-id="db348-104">Windows Store C#</span></span>](notification-hubs-windows-store-dotnet-xplat-localized-wns-push-notification.md)
> * [<span data-ttu-id="db348-105">iOS</span><span class="sxs-lookup"><span data-stu-id="db348-105">iOS</span></span>](notification-hubs-ios-xplat-localized-apns-push-notification.md)
> 
> 

## <a name="overview"></a><span data-ttu-id="db348-106">Přehled</span><span class="sxs-lookup"><span data-stu-id="db348-106">Overview</span></span>
<span data-ttu-id="db348-107">Toto téma ukazuje, jak používat **šablony** funkce Azure Notification Hubs k vysílání oznámení o aktuálních zprávách, lokalizované podle jazyka a zařízení.</span><span class="sxs-lookup"><span data-stu-id="db348-107">This topic shows you how to use the **template** feature of Azure Notification Hubs to broadcast breaking news notifications that have been localized by language and device.</span></span> <span data-ttu-id="db348-108">V tomto kurzu začnete s aplikace Windows Store vytvořená v [použití centra oznámení k odesílání novinek].</span><span class="sxs-lookup"><span data-stu-id="db348-108">In this tutorial you start with the Windows Store app created in [Use Notification Hubs to send breaking news].</span></span> <span data-ttu-id="db348-109">Po dokončení, že bude možné registrovat kategorií, které vás zajímají, zadat jazyk, ve které chcete dostávat oznámení a přijímat pouze nabízená oznámení pro vybrané kategorie v daném jazyce.</span><span class="sxs-lookup"><span data-stu-id="db348-109">When complete, you will be able to register for categories you are interested in, specify a language in which to receive the notifications, and receive only push notifications for the selected categories in that language.</span></span>

<span data-ttu-id="db348-110">Existují tento scénář se skládá ze dvou částí:</span><span class="sxs-lookup"><span data-stu-id="db348-110">There are two parts to this scenario:</span></span>

* <span data-ttu-id="db348-111">aplikace pro Windows Store, umožňuje klientským zařízení můžete určit jazyk a k odběru kategorií různých nejnovější zprávy;</span><span class="sxs-lookup"><span data-stu-id="db348-111">the Windows Store app allows client devices to specify a language, and to subscribe to different breaking news categories;</span></span>
* <span data-ttu-id="db348-112">back-end vysílá oznámení, pomocí **značka** a **šablony** feautres Azure Notification Hubs.</span><span class="sxs-lookup"><span data-stu-id="db348-112">the back-end broadcasts the notifications, using the **tag** and **template** feautres of Azure Notification Hubs.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="db348-113">Požadavky</span><span class="sxs-lookup"><span data-stu-id="db348-113">Prerequisites</span></span>
<span data-ttu-id="db348-114">Musí jste již dokončili [použití centra oznámení k odesílání novinek] kurz a mít kód k dispozici, protože v tomto kurzu staví přímo na tento kód.</span><span class="sxs-lookup"><span data-stu-id="db348-114">You must have already completed the [Use Notification Hubs to send breaking news] tutorial and have the code available, because this tutorial builds directly upon that code.</span></span>

<span data-ttu-id="db348-115">Také musíte Visual Studio 2012 nebo novějším.</span><span class="sxs-lookup"><span data-stu-id="db348-115">You also need Visual Studio 2012 or later.</span></span>

## <a name="template-concepts"></a><span data-ttu-id="db348-116">Koncepty šablon</span><span class="sxs-lookup"><span data-stu-id="db348-116">Template concepts</span></span>
<span data-ttu-id="db348-117">V [použití centra oznámení k odesílání novinek] jste vytvořili aplikaci, která používá **značky** přihlášení k odběru oznámení pro různé zprávy kategorie.</span><span class="sxs-lookup"><span data-stu-id="db348-117">In [Use Notification Hubs to send breaking news] you built an app that used **tags** to subscribe to notifications for different news categories.</span></span>
<span data-ttu-id="db348-118">Velký počet aplikací, ale cíli více trhů a vyžadují lokalizace.</span><span class="sxs-lookup"><span data-stu-id="db348-118">Many apps, however, target multiple markets and require localization.</span></span> <span data-ttu-id="db348-119">To znamená, že obsah sami oznámení musí být lokalizovaný a doručí na správnou sadu zařízení.</span><span class="sxs-lookup"><span data-stu-id="db348-119">This means that the content of the notifications themselves have to be localized and delivered to the correct set of devices.</span></span>
<span data-ttu-id="db348-120">V tomto tématu ukážeme, jak používat **šablony** funkce centra oznámení snadno dodávat služby vhodné oznámení o lokalizované aktuálních zprávách.</span><span class="sxs-lookup"><span data-stu-id="db348-120">In this topic we will show how to use the **template** feature of Notification Hubs to easily deliver localized breaking news notifications.</span></span>

<span data-ttu-id="db348-121">Poznámka: jeden způsob, jak odeslat lokalizované oznámení je vytvoření více verzí jednotlivé značky.</span><span class="sxs-lookup"><span data-stu-id="db348-121">Note: one way to send localized notifications is to create multiple versions of each tag.</span></span> <span data-ttu-id="db348-122">Například pro podporu angličtinu, francouzštinu a Mandarínština, by potřebujeme tří různých značek pro world zprávy: "world_en", "world_fr" a "world_ch".</span><span class="sxs-lookup"><span data-stu-id="db348-122">For instance, to support English, French, and Mandarin, we would need three different tags for world news: "world_en", "world_fr", and "world_ch".</span></span> <span data-ttu-id="db348-123">Pak nám odeslat lokalizované verzi world zprávy pro každé z těchto značek.</span><span class="sxs-lookup"><span data-stu-id="db348-123">We would then have to send a localized version of the world news to each of these tags.</span></span> <span data-ttu-id="db348-124">V tomto tématu používáme šablony předejdete tím, jak narůstá značek a požadavek odeslat více zpráv.</span><span class="sxs-lookup"><span data-stu-id="db348-124">In this topic we use templates to avoid the proliferation of tags and the requirement of sending multiple messages.</span></span>

<span data-ttu-id="db348-125">Na vysoké úrovni šablony jsou způsob, jak určit, jak by měla určité zařízení zasláno oznámení.</span><span class="sxs-lookup"><span data-stu-id="db348-125">At a high level, templates are a way to specify how a specific device should receive a notification.</span></span> <span data-ttu-id="db348-126">Šablona specifikuje formát datové části přesně tím, že odkazuje na vlastnosti, které jsou součástí zprávy odeslané ve vašem back-end aplikace.</span><span class="sxs-lookup"><span data-stu-id="db348-126">The template specifies the exact payload format by referring to properties that are part of the message sent by your app back-end.</span></span> <span data-ttu-id="db348-127">V našem případě pošleme zprávu bez ohledu na národním prostředí obsahující všechny podporované jazyky:</span><span class="sxs-lookup"><span data-stu-id="db348-127">In our case, we will send a locale-agnostic message containing all supported languages:</span></span>

    {
        "News_English": "...",
        "News_French": "...",
        "News_Mandarin": "..."
    }

<span data-ttu-id="db348-128">Potom jsme zajistí, že zařízení zaregistrovat pomocí šablony, která odkazuje na správný vlastnost.</span><span class="sxs-lookup"><span data-stu-id="db348-128">Then we will ensure that devices register with a template that refers to the correct property.</span></span> <span data-ttu-id="db348-129">Například aplikace pro Windows Store, který chce dostávat jednoduché informační zpráva bude registrace pro následující šablony s všechny odpovídající značky:</span><span class="sxs-lookup"><span data-stu-id="db348-129">For instance, a Windows Store app that wants to receive a simple toast message will register for the following template with any corresponding tags:</span></span>

    <toast>
      <visual>
        <binding template=\"ToastText01\">
          <text id=\"1\">$(News_English)</text>
        </binding>
      </visual>
    </toast>



<span data-ttu-id="db348-130">Šablony jsou velmi výkonné funkce, můžete další informace o možnostech v našem [šablony](notification-hubs-templates-cross-platform-push-messages.md) článku.</span><span class="sxs-lookup"><span data-stu-id="db348-130">Templates are a very powerful feature you can learn more about in our [Templates](notification-hubs-templates-cross-platform-push-messages.md) article.</span></span> 

## <a name="the-app-user-interface"></a><span data-ttu-id="db348-131">Uživatelské rozhraní aplikace</span><span class="sxs-lookup"><span data-stu-id="db348-131">The app user interface</span></span>
<span data-ttu-id="db348-132">Nyní jsme upraví novinkách aplikaci, kterou jste vytvořili v tématu [použití centra oznámení k odesílání novinek] odeslat lokalizované novinky pomocí šablon.</span><span class="sxs-lookup"><span data-stu-id="db348-132">We will now modify the Breaking News app that you created in the topic [Use Notification Hubs to send breaking news] to send localized breaking news using templates.</span></span>

<span data-ttu-id="db348-133">V aplikaci Windows Store:</span><span class="sxs-lookup"><span data-stu-id="db348-133">In your Windows Store app:</span></span>

<span data-ttu-id="db348-134">Změna vaší MainPage.xaml zahrnout combobox národního prostředí:</span><span class="sxs-lookup"><span data-stu-id="db348-134">Change your MainPage.xaml to include a locale combobox:</span></span>

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

## <a name="building-the-windows-store-client-app"></a><span data-ttu-id="db348-135">Vytváření klientské aplikace Windows Store</span><span class="sxs-lookup"><span data-stu-id="db348-135">Building the Windows Store client app</span></span>
1. <span data-ttu-id="db348-136">Ve třídě oznámení přidat parametr národního prostředí pro vaše *StoreCategoriesAndSubscribe* a *SubscribeToCateories* metody.</span><span class="sxs-lookup"><span data-stu-id="db348-136">In your Notifications class, add a locale parameter to your  *StoreCategoriesAndSubscribe* and *SubscribeToCateories* methods.</span></span>
   
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
            // Using the localized tags based on locale selected.
            string templateBodyWNS = String.Format("<toast><visual><binding template=\"ToastText01\"><text id=\"1\">$(News_{0})</text></binding></visual></toast>", locale);
   
            return await hub.RegisterTemplateAsync(channel.Uri, templateBodyWNS, "localizedWNSTemplateExample", categories);
        }
   
    <span data-ttu-id="db348-137">Všimněte si, že se namísto volání *RegisterNativeAsync* metoda říkáme *RegisterTemplateAsync*: jsme registraci konkrétní oznámení formát, ve kterém šablony závisí na národním prostředí.</span><span class="sxs-lookup"><span data-stu-id="db348-137">Note that instead of calling the *RegisterNativeAsync* method we call *RegisterTemplateAsync*: we are registering a specific notification format in which the template depends on the locale.</span></span> <span data-ttu-id="db348-138">Poskytujeme také název šablony ("localizedWNSTemplateExample"), protože jsme chtít zaregistrovat více než jednu šablonu (například jeden pro informační zprávy) a jeden pro dlaždice a musíme název je, aby bylo možné aktualizovat nebo odstranit.</span><span class="sxs-lookup"><span data-stu-id="db348-138">We also provide a name for the template ("localizedWNSTemplateExample"), because we might want to register more than one template (for instance one for toast notifications and one for tiles) and we need to name them in order to be able to update or delete them.</span></span>
   
    <span data-ttu-id="db348-139">Všimněte si, že pokud se zařízení zaregistruje několik šablon se stejnou značkou, příchozí zprávy cílení, značka bude mít za následek více oznámení doručit do zařízení (jeden pro každé šablony).</span><span class="sxs-lookup"><span data-stu-id="db348-139">Note that if a device registers multiple templates with the same tag, an incoming message targeting that tag will result in multiple notifications delivered to the device (one for each template).</span></span> <span data-ttu-id="db348-140">Toto chování je užitečné, když stejné logické zpráva má za následek více visual oznámení, například zobrazující oznámení a oznámení v aplikaci pro Windows Store.</span><span class="sxs-lookup"><span data-stu-id="db348-140">This behavior is useful when the same logical message has to result in multiple visual notifications, for instance showing both a badge and a toast in a Windows Store application.</span></span>
2. <span data-ttu-id="db348-141">Přidejte následující metodu pro načtení uložené národního prostředí:</span><span class="sxs-lookup"><span data-stu-id="db348-141">Add the following method to retrieve the stored locale:</span></span>
   
        public string RetrieveLocale()
        {
            var locale = (string) ApplicationData.Current.LocalSettings.Values["locale"];
            return locale != null ? locale : "English";
        }
3. <span data-ttu-id="db348-142">V MainPage.xaml.cs, aktualizovat vaše tlačítko načítání aktuální hodnota pole se seznamem národního prostředí a zajištění k volání do třídy oznámení, klikněte na obslužnou rutinu, jak je znázorněno:</span><span class="sxs-lookup"><span data-stu-id="db348-142">In your MainPage.xaml.cs, update your button click handler by retrieving the current value of the Locale combo box and providing it to the call to the Notifications class, as shown:</span></span>
   
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
4. <span data-ttu-id="db348-143">Nakonec v souboru App.xaml.cs, nezapomeňte aktualizovat vaše `InitNotificationsAsync` metodu za účelem načtení národní prostředí a jeho použití při přihlášení k odběru:</span><span class="sxs-lookup"><span data-stu-id="db348-143">Finally, in your App.xaml.cs file, make sure to update your `InitNotificationsAsync` method to retrieve the locale and use it when subscribing:</span></span>
   
        private async void InitNotificationsAsync()
        {
            var result = await notifications.SubscribeToCategories(notifications.RetrieveLocale());
   
            // Displays the registration ID so you know it was successful
            if (result.RegistrationId != null)
            {
                var dialog = new MessageDialog("Registration successful: " + result.RegistrationId);
                dialog.Commands.Add(new UICommand("OK"));
                await dialog.ShowAsync();
            }
        }

## <a name="send-localized-notifications-from-your-back-end"></a><span data-ttu-id="db348-144">Odesílání lokalizované oznámení z back-end</span><span class="sxs-lookup"><span data-stu-id="db348-144">Send localized notifications from your back-end</span></span>
[!INCLUDE [notification-hubs-localized-back-end](../../includes/notification-hubs-localized-back-end.md)]

<!-- Anchors. -->
[Template concepts]: #concepts
[The app user interface]: #ui
[Building the Windows Store client app]: #building-client
[Send notifications from your back-end]: #send
[Next Steps]:#next-steps

<!-- Images. -->

<!-- URLs. -->
[Mobile Service]: /develop/mobile/tutorials/get-started
[Notify users with Notification Hubs: ASP.NET]: /manage/services/notification-hubs/notify-users-aspnet
[Notify users with Notification Hubs: Mobile Services]: /manage/services/notification-hubs/notify-users
<span data-ttu-id="db348-145">[použití centra oznámení k odesílání novinek]: /manage/services/notification-hubs/breaking-news-dotnet</span><span class="sxs-lookup"><span data-stu-id="db348-145">[Use Notification Hubs to send breaking news]: /manage/services/notification-hubs/breaking-news-dotnet</span></span>

[Submit an app page]: http://go.microsoft.com/fwlink/p/?LinkID=266582
[My Applications]: http://go.microsoft.com/fwlink/p/?LinkId=262039
[Live SDK for Windows]: http://go.microsoft.com/fwlink/p/?LinkId=262253
[Get started with Mobile Services]: /develop/mobile/tutorials/get-started/#create-new-service
[Get started with data]: /develop/mobile/tutorials/get-started-with-data-dotnet
[Get started with authentication]: /develop/mobile/tutorials/get-started-with-users-dotnet
[Get started with push notifications]: /develop/mobile/tutorials/get-started-with-push-dotnet
[Push notifications to app users]: /develop/mobile/tutorials/push-notifications-to-app-users-dotnet
[Authorize users with scripts]: /develop/mobile/tutorials/authorize-users-in-scripts-dotnet
[JavaScript and HTML]: /develop/mobile/tutorials/get-started-with-push-js

[wns object]: http://go.microsoft.com/fwlink/p/?LinkId=260591
[Notification Hubs Guidance]: http://msdn.microsoft.com/library/jj927170.aspx
[Notification Hubs How-To for iOS]: http://msdn.microsoft.com/library/jj927168.aspx
[Notification Hubs How-To for Windows Store]: http://msdn.microsoft.com/library/jj927172.aspx
