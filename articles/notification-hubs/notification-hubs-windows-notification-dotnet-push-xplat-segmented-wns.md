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
# <a name="use-notification-hubs-to-send-breaking-news"></a><span data-ttu-id="25c7c-103">Používání centra oznámení k odesílání novinek</span><span class="sxs-lookup"><span data-stu-id="25c7c-103">Use Notification Hubs to send breaking news</span></span>
[!INCLUDE [notification-hubs-selector-breaking-news](../../includes/notification-hubs-selector-breaking-news.md)]

## <a name="overview"></a><span data-ttu-id="25c7c-104">Přehled</span><span class="sxs-lookup"><span data-stu-id="25c7c-104">Overview</span></span>
<span data-ttu-id="25c7c-105">Toto téma ukazuje, jak používat Azure Notification Hubs k vysílání oznámení o aktuálních zprávách do Windows Store nebo Windows Phone 8.1 (bez Silverlight) aplikace.</span><span class="sxs-lookup"><span data-stu-id="25c7c-105">This topic shows you how to use Azure Notification Hubs to broadcast breaking news notifications to a Windows Store or Windows Phone 8.1 (non-Silverlight) app.</span></span> <span data-ttu-id="25c7c-106">Pokud cílíte na Windows Phone 8.1 Silverlight, získáte informace [Windows Phone](notification-hubs-windows-phone-push-xplat-segmented-mpns-notification.md) verze.</span><span class="sxs-lookup"><span data-stu-id="25c7c-106">If you are targeting Windows Phone 8.1 Silverlight, please refer to the [Windows Phone](notification-hubs-windows-phone-push-xplat-segmented-mpns-notification.md) version.</span></span> <span data-ttu-id="25c7c-107">Po dokončení bude moci zaregistrovat pro nejnovější novinky kategorií, které vás zajímají a přijímat pouze nabízená oznámení pro tyto kategorie.</span><span class="sxs-lookup"><span data-stu-id="25c7c-107">When complete, you will be able to register for breaking news categories you are interested in, and receive only push notifications for those categories.</span></span> <span data-ttu-id="25c7c-108">Tento scénář je běžný vzor velký počet aplikací, kde mají oznámení k odeslání do skupiny uživatelů, které jste předtím nebyl deklarovaný zájem o je například čtečku RSS, aplikace pro Hudba ventilátory a tak dále.</span><span class="sxs-lookup"><span data-stu-id="25c7c-108">This scenario is a common pattern for many apps where notifications have to be sent to groups of users that have previously declared interest in them, e.g. RSS reader, apps for music fans, and so on.</span></span> 

<span data-ttu-id="25c7c-109">Všesměrového vysílání scénáře jsou povolené zahrnutím jeden nebo více *značky* při vytváření registrace v centru oznámení.</span><span class="sxs-lookup"><span data-stu-id="25c7c-109">Broadcast scenarios are enabled by including one or more *tags* when creating a registration in the notification hub.</span></span> <span data-ttu-id="25c7c-110">Pokud oznámení se odesílají do značku, všechna zařízení, která byla zaregistrovaná pro značku obdrží oznámení.</span><span class="sxs-lookup"><span data-stu-id="25c7c-110">When notifications are sent to a tag, all devices that have registered for the tag will receive the notification.</span></span> <span data-ttu-id="25c7c-111">Protože značky jsou jednoduše řetězce, nemají být předem zřízená.</span><span class="sxs-lookup"><span data-stu-id="25c7c-111">Because tags are simply strings, they do not have to be provisioned in advance.</span></span> <span data-ttu-id="25c7c-112">Další informace o značkách najdete v části [směrování centra oznámení a značky výrazy](notification-hubs-tags-segment-push-message.md).</span><span class="sxs-lookup"><span data-stu-id="25c7c-112">For more information about tags, refer to [Notification Hubs Routing and Tag Expressions](notification-hubs-tags-segment-push-message.md).</span></span>

> [!NOTE]
> <span data-ttu-id="25c7c-113">Windows Store a verze projekty Windows Phone 8.1 a starší nejsou podporovány v aplikaci Visual Studio 2017.</span><span class="sxs-lookup"><span data-stu-id="25c7c-113">Windows Store and Windows Phone projects version 8.1 and earlier are not supported in Visual Studio 2017.</span></span>  <span data-ttu-id="25c7c-114">Další informace najdete v tématu [Cílení na platformy a kompatibilita v sadě Visual Studio 2017](https://www.visualstudio.com/en-us/productinfo/vs2017-compatibility-vs).</span><span class="sxs-lookup"><span data-stu-id="25c7c-114">For more information, see [Visual Studio 2017 Platform Targeting and Compatibility](https://www.visualstudio.com/en-us/productinfo/vs2017-compatibility-vs).</span></span> 

## <a name="prerequisites"></a><span data-ttu-id="25c7c-115">Požadavky</span><span class="sxs-lookup"><span data-stu-id="25c7c-115">Prerequisites</span></span>
<span data-ttu-id="25c7c-116">Toto téma je založený na aplikaci, kterou jste vytvořili v [Začínáme s Notification Hubs][get-started].</span><span class="sxs-lookup"><span data-stu-id="25c7c-116">This topic builds on the app you created in [Get started with Notification Hubs][get-started].</span></span> <span data-ttu-id="25c7c-117">Před zahájením tohoto kurzu, musí jste již dokončili [Začínáme s Notification Hubs][get-started].</span><span class="sxs-lookup"><span data-stu-id="25c7c-117">Before starting this tutorial, you must have already completed [Get started with Notification Hubs][get-started].</span></span>

## <a name="add-category-selection-to-the-app"></a><span data-ttu-id="25c7c-118">Přidat výběru kategorie do aplikace</span><span class="sxs-lookup"><span data-stu-id="25c7c-118">Add category selection to the app</span></span>
<span data-ttu-id="25c7c-119">Prvním krokem je přidání prvky uživatelského rozhraní na existující stránku pro hlavní, který uživateli umožňuje výběr kategorií k registraci.</span><span class="sxs-lookup"><span data-stu-id="25c7c-119">The first step is to add the UI elements to your existing main page that enable the user to select categories to register.</span></span> <span data-ttu-id="25c7c-120">Kategorie, které uživatel jsou uloženy v zařízení.</span><span class="sxs-lookup"><span data-stu-id="25c7c-120">The categories selected by a user are stored on the device.</span></span> <span data-ttu-id="25c7c-121">Při spuštění aplikace registrace zařízení se vytvoří v centru oznámení s vybrané kategorie jako značky.</span><span class="sxs-lookup"><span data-stu-id="25c7c-121">When the app starts, a device registration is created in your notification hub with the selected categories as tags.</span></span>

1. <span data-ttu-id="25c7c-122">Otevřete soubor projektu MainPage.xaml a pak zkopírujte následující kód v **mřížky** element:</span><span class="sxs-lookup"><span data-stu-id="25c7c-122">Open the MainPage.xaml project file, then copy the following code in the **Grid** element:</span></span>
   
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
2. <span data-ttu-id="25c7c-123">Klikněte pravým tlačítkem **sdílené** projekt a přidejte novou třídu s názvem **oznámení**, přidejte **veřejné** modifikátor v definici třídy, přidejte následující  **pomocí** příkazy na nový soubor kódu:</span><span class="sxs-lookup"><span data-stu-id="25c7c-123">Right click the **Shared** project and add a new class named **Notifications**, add the **public** modifier to the class definition, then add the following **using** statements to the new code file:</span></span>
   
        using Windows.Networking.PushNotifications;
        using Microsoft.WindowsAzure.Messaging;
        using Windows.Storage;
        using System.Threading.Tasks;
3. <span data-ttu-id="25c7c-124">Zkopírujte následující kód do nové **oznámení** třídy:</span><span class="sxs-lookup"><span data-stu-id="25c7c-124">Copy the following code into the new **Notifications** class:</span></span>
   
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
   
    <span data-ttu-id="25c7c-125">Tato třída používá místní úložiště k ukládání kategorie příspěvků, který toto zařízení má přijmout.</span><span class="sxs-lookup"><span data-stu-id="25c7c-125">This class uses the local storage to store the categories of news that this device has to receive.</span></span> <span data-ttu-id="25c7c-126">Všimněte si, že se namísto volání *RegisterNativeAsync* metoda říkáme *RegisterTemplateAsync* registrovat kategorie pomocí šablony registrace.</span><span class="sxs-lookup"><span data-stu-id="25c7c-126">Note that instead of calling the *RegisterNativeAsync* method we call *RegisterTemplateAsync* to register for the categories using a template registration.</span></span> 
   
    <span data-ttu-id="25c7c-127">Poskytujeme také název šablony ("simpleWNSTemplateExample"), protože jsme chtít zaregistrovat více než jednu šablonu (například jeden pro informační zprávy) a jeden pro dlaždice a musíme název je, aby bylo možné aktualizovat nebo odstranit.</span><span class="sxs-lookup"><span data-stu-id="25c7c-127">We also provide a name for the template ("simpleWNSTemplateExample"), because we might want to register more than one template (for instance one for toast notifications and one for tiles) and we need to name them in order to be able to update or delete them.</span></span>
   
    <span data-ttu-id="25c7c-128">Všimněte si, že pokud se zařízení zaregistruje několik šablon se stejnou značkou, příchozí zprávy cílení, značka bude mít za následek více oznámení doručit do zařízení (jeden pro každé šablony).</span><span class="sxs-lookup"><span data-stu-id="25c7c-128">Note that if a device registers multiple templates with the same tag, an incoming message targeting that tag will result in multiple notifications delivered to the device (one for each template).</span></span> <span data-ttu-id="25c7c-129">Toto chování je užitečné, když stejné logické zpráva má za následek více visual oznámení, například zobrazující oznámení a oznámení v aplikaci pro Windows Store.</span><span class="sxs-lookup"><span data-stu-id="25c7c-129">This behavior is useful when the same logical message has to result in multiple visual notifications, for instance showing both a badge and a toast in a Windows Store application.</span></span>
   
    <span data-ttu-id="25c7c-130">Další informace o šablonách najdete v tématu [šablony](notification-hubs-templates-cross-platform-push-messages.md).</span><span class="sxs-lookup"><span data-stu-id="25c7c-130">For more information on templates, see [Templates](notification-hubs-templates-cross-platform-push-messages.md).</span></span>
4. <span data-ttu-id="25c7c-131">V souboru projektu App.xaml.cs přidejte následující vlastnosti, která má **aplikace** třídy:</span><span class="sxs-lookup"><span data-stu-id="25c7c-131">In the App.xaml.cs project file, add the following property to the **App** class:</span></span>
   
        public Notifications notifications = new Notifications("<hub name>", "<connection string with listen access>");
   
    <span data-ttu-id="25c7c-132">Tato vlastnost slouží k vytváření a přístup **oznámení** instance.</span><span class="sxs-lookup"><span data-stu-id="25c7c-132">This property is used to create and access a **Notifications** instance.</span></span>
   
    <span data-ttu-id="25c7c-133">Ve výše uvedeném kódu nahraďte `<hub name>` a `<connection string with listen access>` zástupné symboly pomocí názvu centra oznámení a připojovacího řetězce pro *DefaultListenSharedAccessSignature* kterou jste získali dříve.</span><span class="sxs-lookup"><span data-stu-id="25c7c-133">In the above code, replace the `<hub name>` and `<connection string with listen access>` placeholders with your notification hub name and the connection string for *DefaultListenSharedAccessSignature* that you obtained earlier.</span></span>
   
   > [!NOTE]
   > <span data-ttu-id="25c7c-134">Protože přihlašovací údaje, které jsou distribuované s klientskou aplikaci není obvykle zabezpečení, by měl distribuovat klíč pro naslouchání přístup pouze s vaší klientské aplikace.</span><span class="sxs-lookup"><span data-stu-id="25c7c-134">Because credentials that are distributed with a client app are not generally secure, you should only distribute the key for listen access with your client app.</span></span> <span data-ttu-id="25c7c-135">Poslechněte umožní přístup k aplikaci zaregistrovat pro oznámení, ale existující registrace nemůže být upravena a nelze odeslat oznámení.</span><span class="sxs-lookup"><span data-stu-id="25c7c-135">Listen access enables your app to register for notifications, but existing registrations cannot be modified and notifications cannot be sent.</span></span> <span data-ttu-id="25c7c-136">Úplný přístup klíč se používá ve službě Zabezpečené back-end pro zasílání oznámení a změna existující registrace.</span><span class="sxs-lookup"><span data-stu-id="25c7c-136">The full access key is used in a secured backend service for sending notifications and changing existing registrations.</span></span>
   > 
   > 
5. <span data-ttu-id="25c7c-137">V MainPage.xaml.cs přidejte následující řádek:</span><span class="sxs-lookup"><span data-stu-id="25c7c-137">In your MainPage.xaml.cs, add the following line:</span></span>
   
        using Windows.UI.Popups;
6. <span data-ttu-id="25c7c-138">V souboru projektu MainPage.xaml.cs přidejte následující metodu:</span><span class="sxs-lookup"><span data-stu-id="25c7c-138">In the MainPage.xaml.cs project file, add the following method:</span></span>
   
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
   
    <span data-ttu-id="25c7c-139">Tato metoda vytvoří seznam kategorií a používá **oznámení** značky třída pro uložení seznamu v místním úložišti a zaregistrujte odpovídající pomocí centra oznámení.</span><span class="sxs-lookup"><span data-stu-id="25c7c-139">This method creates a list of categories and uses the **Notifications** class to store the list in the local storage and register the corresponding tags with your notification hub.</span></span> <span data-ttu-id="25c7c-140">Při změně kategorií, registrace se znovu vytvoří se nové kategorie.</span><span class="sxs-lookup"><span data-stu-id="25c7c-140">When categories are changed, the registration is recreated with the new categories.</span></span>

<span data-ttu-id="25c7c-141">Aplikace je teď možné uložit sadu kategorií místní úložiště v zařízení a zaregistrovat do centra oznámení pokaždé, když uživatel změní výběr kategorie.</span><span class="sxs-lookup"><span data-stu-id="25c7c-141">Your app is now able to store a set of categories in local storage on the device and register with the notification hub whenever the user changes the selection of categories.</span></span>

## <a name="register-for-notifications"></a><span data-ttu-id="25c7c-142">Registrace pro oznámení</span><span class="sxs-lookup"><span data-stu-id="25c7c-142">Register for notifications</span></span>
<span data-ttu-id="25c7c-143">Tyto kroky zaregistrovat do centra oznámení na spouštění pomocí kategorií, které byly uloženy v místním úložišti.</span><span class="sxs-lookup"><span data-stu-id="25c7c-143">These steps register with the notification hub on startup using the categories that have been stored in local storage.</span></span>

> [!NOTE]
> <span data-ttu-id="25c7c-144">Vzhledem k tomu, že kanál URI přiřazené pomocí služby oznámení Windows (WNS) můžete změnit kdykoli, byste měli zaregistrovat pro oznámení často, aby se zabránilo selhání oznámení.</span><span class="sxs-lookup"><span data-stu-id="25c7c-144">Because the channel URI assigned by the Windows Notification Service (WNS) can change at any time, you should register for notifications frequently to avoid notification failures.</span></span> <span data-ttu-id="25c7c-145">V tomto příkladu se zaregistruje pro oznámení při každém spuštění aplikace.</span><span class="sxs-lookup"><span data-stu-id="25c7c-145">This example registers for notification every time that the app starts.</span></span> <span data-ttu-id="25c7c-146">Pro aplikace, které jsou často spouštíte více než jednou denně, můžete pravděpodobně přeskočit registraci byla zachována šířka pásma, pokud od předchozí registrace uplynul méně než jeden den.</span><span class="sxs-lookup"><span data-stu-id="25c7c-146">For apps that are run frequently, more than once a day, you can probably skip registration to preserve bandwidth if less than a day has passed since the previous registration.</span></span>
> 
> 

1. <span data-ttu-id="25c7c-147">Otevřete soubor App.xaml.cs a aktualizace **InitNotificationsAsync** metoda se má použít `notifications` k odběru na základě kategorií.</span><span class="sxs-lookup"><span data-stu-id="25c7c-147">Open the App.xaml.cs file and update the **InitNotificationsAsync** method to use the `notifications` class to subscribe based on categories.</span></span>
   
        // *** Remove or comment out these lines *** 
        //var channel = await PushNotificationChannelManager.CreatePushNotificationChannelForApplicationAsync();
        //var hub = new NotificationHub("your hub name", "your listen connection string");
        //var result = await hub.RegisterNativeAsync(channel.Uri);
   
        var result = await notifications.SubscribeToCategories();
   
    <span data-ttu-id="25c7c-148">Tím je zajištěno, že při každém spuštění aplikace načte kategorie z místního úložiště a požadavky registrace pro tyto kategorie.</span><span class="sxs-lookup"><span data-stu-id="25c7c-148">This makes sure that every time the app starts it retrieves the categories from local storage and requests a registeration for these categories.</span></span> <span data-ttu-id="25c7c-149">**InitNotificationsAsync** metoda byla vytvořena jako součást [Začínáme s Notification Hubs] [ get-started] kurzu.</span><span class="sxs-lookup"><span data-stu-id="25c7c-149">The **InitNotificationsAsync** method was created as part of the [Get started with Notification Hubs][get-started] tutorial.</span></span>
2. <span data-ttu-id="25c7c-150">V souboru projektu MainPage.xaml.cs, přidejte následující kód, který *OnNavigatedTo* metoda:</span><span class="sxs-lookup"><span data-stu-id="25c7c-150">In the MainPage.xaml.cs project file, add the following code to the *OnNavigatedTo* method:</span></span>
   
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
   
    <span data-ttu-id="25c7c-151">Tím se aktualizuje na základě stavu dříve uloženou kategorií hlavní stránce.</span><span class="sxs-lookup"><span data-stu-id="25c7c-151">This updates the main page based on the status of previously saved categories.</span></span>

<span data-ttu-id="25c7c-152">Aplikace je nyní dokončen a sadu kategorií můžete uložit do místního úložiště zařízení používá k registraci do centra oznámení pokaždé, když uživatel změní výběr kategorie.</span><span class="sxs-lookup"><span data-stu-id="25c7c-152">The app is now complete and can store a set of categories in the device local storage used to register with the notification hub whenever the user changes the selection of categories.</span></span> <span data-ttu-id="25c7c-153">V dalším kroku bude definujeme back-end, který kategorie oznámení můžete odesílat do této aplikace.</span><span class="sxs-lookup"><span data-stu-id="25c7c-153">Next, we will define a backend that can send category notifications to this app.</span></span>

## <a name="sending-tagged-notifications"></a><span data-ttu-id="25c7c-154">Odesílání oznámení s příznakem</span><span class="sxs-lookup"><span data-stu-id="25c7c-154">Sending tagged notifications</span></span>
[!INCLUDE [notification-hubs-send-categories-template](../../includes/notification-hubs-send-categories-template.md)]

## <a name="run-the-app-and-generate-notifications"></a><span data-ttu-id="25c7c-155">Spusťte aplikaci a generovat upozornění</span><span class="sxs-lookup"><span data-stu-id="25c7c-155">Run the app and generate notifications</span></span>
1. <span data-ttu-id="25c7c-156">Ve Visual Studiu stisknutím klávesy F5 zkompilování a spuštění aplikace.</span><span class="sxs-lookup"><span data-stu-id="25c7c-156">In Visual Studio, press F5 to compile and start the app.</span></span>
   
    ![][1]
   
    <span data-ttu-id="25c7c-157">Všimněte si, že aplikace uživatelského rozhraní, poskytuje sadu přepínačů, která vám umožní vybrat kategorie pro přihlášení k odběru.</span><span class="sxs-lookup"><span data-stu-id="25c7c-157">Note that the app UI provides a set of toggles that lets you choose the categories to subscribe to.</span></span>
2. <span data-ttu-id="25c7c-158">Povolit jednu nebo více kategorií přepínačů a potom klikněte na **přihlásit k odběru**.</span><span class="sxs-lookup"><span data-stu-id="25c7c-158">Enable one or more categories toggles, then click **Subscribe**.</span></span>
   
    <span data-ttu-id="25c7c-159">Aplikace převede vybraných kategorií značky a požaduje novou registraci zařízení pro vybranou značky z centra oznámení.</span><span class="sxs-lookup"><span data-stu-id="25c7c-159">The app converts the selected categories into tags and requests a new device registration for the selected tags from the notification hub.</span></span> <span data-ttu-id="25c7c-160">Registrovaný kategorie se vrátí a zobrazí v dialogu.</span><span class="sxs-lookup"><span data-stu-id="25c7c-160">The registered categories are returned and displayed in a dialog.</span></span>
   
    ![][19]
3. <span data-ttu-id="25c7c-161">Odeslání nové oznámení z back-end v jednom z následujících způsobů:</span><span class="sxs-lookup"><span data-stu-id="25c7c-161">Send a new notification from the backend in one of the following ways:</span></span>
   
   * <span data-ttu-id="25c7c-162">**Konzolové aplikace:** Spusťte konzolovou aplikaci.</span><span class="sxs-lookup"><span data-stu-id="25c7c-162">**Console app:** start the console app.</span></span>
   * <span data-ttu-id="25c7c-163">**Java/PHP:** spuštění vaší aplikace nebo skriptu.</span><span class="sxs-lookup"><span data-stu-id="25c7c-163">**Java/PHP:** run your app/script.</span></span>
     
     <span data-ttu-id="25c7c-164">Oznámení pro vybrané kategorie se zobrazí jako informační zprávy.</span><span class="sxs-lookup"><span data-stu-id="25c7c-164">Notifications for the selected categories appear as toast notifications.</span></span>
     
     ![][14]

## <a name="next-steps"></a><span data-ttu-id="25c7c-165">Další kroky</span><span class="sxs-lookup"><span data-stu-id="25c7c-165">Next steps</span></span>
<span data-ttu-id="25c7c-166">V tomto kurzu jsme zjistili, jak k vysílání novinek podle kategorie.</span><span class="sxs-lookup"><span data-stu-id="25c7c-166">In this tutorial we learned how to broadcast breaking news by category.</span></span> <span data-ttu-id="25c7c-167">Vezměte v úvahu dokončení jednu z následujících kurzů upozorňující na jiné pokročilé scénáře centra oznámení:</span><span class="sxs-lookup"><span data-stu-id="25c7c-167">Consider completing one of the following tutorials that highlight other advanced Notification Hubs scenarios:</span></span>

* <span data-ttu-id="25c7c-168">[Použití centra oznámení k vysílání lokalizované novinek]</span><span class="sxs-lookup"><span data-stu-id="25c7c-168">[Use Notification Hubs to broadcast localized breaking news]</span></span>
  
    <span data-ttu-id="25c7c-169">Zjistěte, jak rozšířit aplikace nejnovější zprávy k povolení odesílání lokalizované upozornění.</span><span class="sxs-lookup"><span data-stu-id="25c7c-169">Learn how to expand the breaking news app to enable sending localized notifications.</span></span>

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
<span data-ttu-id="25c7c-170">[Použití centra oznámení k vysílání lokalizované novinek]: /manage/services/notification-hubs/breaking-news-localized-dotnet/</span><span class="sxs-lookup"><span data-stu-id="25c7c-170">[Use Notification Hubs to broadcast localized breaking news]: /manage/services/notification-hubs/breaking-news-localized-dotnet/</span></span>
[Notify users with Notification Hubs]: /manage/services/notification-hubs/notify-users
[Mobile Service]: /develop/mobile/tutorials/get-started/
[Notification Hubs Guidance]: http://msdn.microsoft.com/library/jj927170.aspx
[Notification Hubs How-To for Windows Store]: http://msdn.microsoft.com/library/jj927172.aspx
[Submit an app page]: http://go.microsoft.com/fwlink/p/?LinkID=266582
[My Applications]: http://go.microsoft.com/fwlink/p/?LinkId=262039
[Live SDK for Windows]: http://go.microsoft.com/fwlink/p/?LinkId=262253

[wns object]: http://go.microsoft.com/fwlink/p/?LinkId=260591
