---
title: "Použití služby Notification Hubs k odesílání novinek (Windows Phone)"
description: "Pomocí Azure Notification Hubs můžete použít značky v registrace k odesílání novinek do aplikace pro Windows Phone."
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
ms.openlocfilehash: 3a6a69bf555c7267d3fbeb03ff6c03054991960f
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="use-notification-hubs-to-send-breaking-news"></a><span data-ttu-id="6b059-103">Používání centra oznámení k odesílání novinek</span><span class="sxs-lookup"><span data-stu-id="6b059-103">Use Notification Hubs to send breaking news</span></span>
[!INCLUDE [notification-hubs-selector-breaking-news](../../includes/notification-hubs-selector-breaking-news.md)]

## <a name="overview"></a><span data-ttu-id="6b059-104">Přehled</span><span class="sxs-lookup"><span data-stu-id="6b059-104">Overview</span></span>
<span data-ttu-id="6b059-105">Toto téma ukazuje, jak používat Azure Notification Hubs k vysílání oznámení o aktuálních zprávách do aplikace pro Windows Phone 8.0/8.1 Silverlight.</span><span class="sxs-lookup"><span data-stu-id="6b059-105">This topic shows you how to use Azure Notification Hubs to broadcast breaking news notifications to a Windows Phone 8.0/8.1 Silverlight app.</span></span> <span data-ttu-id="6b059-106">Pokud cílíte na Windows Store nebo Windows Phone 8.1 aplikace, naleznete na [univerzální pro Windows](notification-hubs-windows-notification-dotnet-push-xplat-segmented-wns.md) verze.</span><span class="sxs-lookup"><span data-stu-id="6b059-106">If you are targeting Windows Store or Windows Phone 8.1 app, please refer to to the [Windows Universal](notification-hubs-windows-notification-dotnet-push-xplat-segmented-wns.md) version.</span></span> <span data-ttu-id="6b059-107">Po dokončení bude moci zaregistrovat pro nejnovější novinky kategorií, které vás zajímají a přijímat pouze nabízená oznámení pro tyto kategorie.</span><span class="sxs-lookup"><span data-stu-id="6b059-107">When complete, you will be able to register for breaking news categories you are interested in, and receive only push notifications for those categories.</span></span> <span data-ttu-id="6b059-108">Tento scénář je běžný vzor velký počet aplikací, kde mají oznámení k odeslání do skupiny uživatelů, které jste předtím nebyl deklarovaný zájem o jejich, např. čtečku RSS, aplikace pro Hudba ventilátory, atd.</span><span class="sxs-lookup"><span data-stu-id="6b059-108">This scenario is a common pattern for many apps where notifications have to be sent to groups of users that have previously declared interest in them, e.g. RSS reader, apps for music fans, etc.</span></span>

<span data-ttu-id="6b059-109">Všesměrového vysílání scénáře jsou povolené zahrnutím jeden nebo více *značky* při vytváření registrace v centru oznámení.</span><span class="sxs-lookup"><span data-stu-id="6b059-109">Broadcast scenarios are enabled by including one or more *tags* when creating a registration in the notification hub.</span></span> <span data-ttu-id="6b059-110">Pokud oznámení se odesílají do značku, všechna zařízení, která byla zaregistrovaná pro značku obdrží oznámení.</span><span class="sxs-lookup"><span data-stu-id="6b059-110">When notifications are sent to a tag, all devices that have registered for the tag will receive the notification.</span></span> <span data-ttu-id="6b059-111">Protože značky jsou jednoduše řetězce, nemají být předem zřízená.</span><span class="sxs-lookup"><span data-stu-id="6b059-111">Because tags are simply strings, they do not have to be provisioned in advance.</span></span> <span data-ttu-id="6b059-112">Další informace o značkách najdete v části [směrování centra oznámení a značky výrazy](notification-hubs-tags-segment-push-message.md).</span><span class="sxs-lookup"><span data-stu-id="6b059-112">For more information about tags, refer to [Notification Hubs Routing and Tag Expressions](notification-hubs-tags-segment-push-message.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="6b059-113">Požadavky</span><span class="sxs-lookup"><span data-stu-id="6b059-113">Prerequisites</span></span>
<span data-ttu-id="6b059-114">Toto téma je založený na aplikaci, kterou jste vytvořili v [Začínáme s Notification Hubs].</span><span class="sxs-lookup"><span data-stu-id="6b059-114">This topic builds on the app you created in [Get started with Notification Hubs].</span></span> <span data-ttu-id="6b059-115">Před zahájením tohoto kurzu, musí jste již dokončili [Začínáme s Notification Hubs].</span><span class="sxs-lookup"><span data-stu-id="6b059-115">Before starting this tutorial, you must have already completed [Get started with Notification Hubs].</span></span>

## <a name="add-category-selection-to-the-app"></a><span data-ttu-id="6b059-116">Přidat výběru kategorie do aplikace</span><span class="sxs-lookup"><span data-stu-id="6b059-116">Add category selection to the app</span></span>
<span data-ttu-id="6b059-117">Prvním krokem je přidání prvky uživatelského rozhraní na existující stránku pro hlavní, který uživateli umožňuje výběr kategorií k registraci.</span><span class="sxs-lookup"><span data-stu-id="6b059-117">The first step is to add the UI elements to your existing main page that enable the user to select categories to register.</span></span> <span data-ttu-id="6b059-118">Kategorie, které uživatel jsou uloženy v zařízení.</span><span class="sxs-lookup"><span data-stu-id="6b059-118">The categories selected by a user are stored on the device.</span></span> <span data-ttu-id="6b059-119">Při spuštění aplikace registrace zařízení se vytvoří v centru oznámení s vybrané kategorie jako značky.</span><span class="sxs-lookup"><span data-stu-id="6b059-119">When the app starts, a device registration is created in your notification hub with the selected categories as tags.</span></span>

1. <span data-ttu-id="6b059-120">Otevřete soubor projektu MainPage.xaml a nahraďte **mřížky** elementů s názvem `TitlePanel` a `ContentPanel` následujícím kódem:</span><span class="sxs-lookup"><span data-stu-id="6b059-120">Open the MainPage.xaml project file, then replace the **Grid** elements named `TitlePanel` and `ContentPanel` with the following code:</span></span>
   
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
2. <span data-ttu-id="6b059-121">V projektu, vytvořte novou třídu s názvem **oznámení**, přidejte **veřejné** modifikátor v definici třídy, přidejte následující **pomocí** příkazů do nového souboru kódu :</span><span class="sxs-lookup"><span data-stu-id="6b059-121">In the project, create a new class named **Notifications**, add the **public** modifier to the class definition, then add the following **using** statements to the new code file:</span></span>
   
        using Microsoft.Phone.Notification;
        using Microsoft.WindowsAzure.Messaging;
        using System.IO.IsolatedStorage;
        using System.Windows;
3. <span data-ttu-id="6b059-122">Zkopírujte následující kód do nové **oznámení** třídy:</span><span class="sxs-lookup"><span data-stu-id="6b059-122">Copy the following code into the new **Notifications** class:</span></span>
   
        private NotificationHub hub;
   
        // Registration task to complete registration in the ChannelUriUpdated event handler
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
   
                // This is optional, used to receive notifications while the app is running.
                channel.ShellToastNotificationReceived += channel_ShellToastNotificationReceived;
            }
   
            // If channel.ChannelUri is not null, we will complete the registrationTask here.  
            // If it is null, the registrationTask will be completed in the ChannelUriUpdated event handler.
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
            // Using a template registration to support notifications across platforms.
            // Any template notifications that contain messageParam and a corresponding tag expression
            // will be delivered for this registration.
   
            const string templateBodyMPNS = "<wp:Notification xmlns:wp=\"WPNotification\">" +
                                                "<wp:Toast>" +
                                                    "<wp:Text1>$(messageParam)</wp:Text1>" +
                                                "</wp:Toast>" +
                                            "</wp:Notification>";
   
            // The stored categories tags are passed with the template registration.
   
            registrationTask.SetResult(await hub.RegisterTemplateAsync(channelUri.ToString(), 
                templateBodyMPNS, "simpleMPNSTemplateExample", this.RetrieveCategories()));
   
            return await registrationTask.Task;
        }
   
        // This is optional. It is used to receive notifications while the app is running.
        void channel_ShellToastNotificationReceived(object sender, NotificationEventArgs e)
        {
            StringBuilder message = new StringBuilder();
            string relativeUri = string.Empty;
   
            message.AppendFormat("Received Toast {0}:\n", DateTime.Now.ToShortTimeString());
   
            // Parse out the information that was part of the message.
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
   
            // Display a dialog of all the fields in the toast.
            System.Windows.Deployment.Current.Dispatcher.BeginInvoke(() => 
            { 
                MessageBox.Show(message.ToString()); 
            });
        }

    <span data-ttu-id="6b059-123">Tato třída používá izolované úložiště k ukládání kategorie zprávy, které toto zařízení se zobrazí.</span><span class="sxs-lookup"><span data-stu-id="6b059-123">This class uses the isolated storage to store the categories of news that this device is to receive.</span></span> <span data-ttu-id="6b059-124">Také obsahuje metody pro registraci pro tyto kategorie pomocí [šablony](notification-hubs-templates-cross-platform-push-messages.md) registrace oznámení.</span><span class="sxs-lookup"><span data-stu-id="6b059-124">It also contains methods to register for these categories using a [template](notification-hubs-templates-cross-platform-push-messages.md) notification registration.</span></span>


1. <span data-ttu-id="6b059-125">V souboru projektu App.xaml.cs přidejte následující vlastnosti, která má **aplikace** třídy.</span><span class="sxs-lookup"><span data-stu-id="6b059-125">In the App.xaml.cs project file, add the following property to the **App** class.</span></span> <span data-ttu-id="6b059-126">Nahraďte `<hub name>` a `<connection string with listen access>` zástupné symboly pomocí názvu centra oznámení a připojovacího řetězce pro *DefaultListenSharedAccessSignature* kterou jste získali dříve.</span><span class="sxs-lookup"><span data-stu-id="6b059-126">Replace the `<hub name>` and `<connection string with listen access>` placeholders with your notification hub name and the connection string for *DefaultListenSharedAccessSignature* that you obtained earlier.</span></span>
   
        public Notifications notifications = new Notifications("<hub name>", "<connection string with listen access>");
   
   > [!NOTE]
   > <span data-ttu-id="6b059-127">Protože přihlašovací údaje, které jsou distribuované s klientskou aplikaci není obvykle zabezpečení, by měl distribuovat klíč pro naslouchání přístup pouze s vaší klientské aplikace.</span><span class="sxs-lookup"><span data-stu-id="6b059-127">Because credentials that are distributed with a client app are not generally secure, you should only distribute the key for listen access with your client app.</span></span> <span data-ttu-id="6b059-128">Poslechněte umožní přístup k aplikaci zaregistrovat pro oznámení, ale existující registrace nemůže být upravena a nelze odeslat oznámení.</span><span class="sxs-lookup"><span data-stu-id="6b059-128">Listen access enables your app to register for notifications, but existing registrations cannot be modified and notifications cannot be sent.</span></span> <span data-ttu-id="6b059-129">Úplný přístup klíč se používá ve službě Zabezpečené back-end pro zasílání oznámení a změna existující registrace.</span><span class="sxs-lookup"><span data-stu-id="6b059-129">The full access key is used in a secured backend service for sending notifications and changing existing registrations.</span></span>
   > 
   > 
2. <span data-ttu-id="6b059-130">V MainPage.xaml.cs přidejte následující řádek:</span><span class="sxs-lookup"><span data-stu-id="6b059-130">In your MainPage.xaml.cs, add the following line:</span></span>
   
        using Windows.UI.Popups;
3. <span data-ttu-id="6b059-131">V souboru projektu MainPage.xaml.cs přidejte následující metodu:</span><span class="sxs-lookup"><span data-stu-id="6b059-131">In the MainPage.xaml.cs project file, add the following method:</span></span>
   
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
   
    <span data-ttu-id="6b059-132">Tato metoda vytvoří seznam kategorií a používá **oznámení** značky třída pro uložení seznamu v místním úložišti a zaregistrujte odpovídající pomocí centra oznámení.</span><span class="sxs-lookup"><span data-stu-id="6b059-132">This method creates a list of categories and uses the **Notifications** class to store the list in the local storage and register the corresponding tags with your notification hub.</span></span> <span data-ttu-id="6b059-133">Při změně kategorií, registrace se znovu vytvoří se nové kategorie.</span><span class="sxs-lookup"><span data-stu-id="6b059-133">When categories are changed, the registration is recreated with the new categories.</span></span>

<span data-ttu-id="6b059-134">Aplikace je teď možné uložit sadu kategorií místní úložiště v zařízení a zaregistrovat do centra oznámení pokaždé, když uživatel změní výběr kategorie.</span><span class="sxs-lookup"><span data-stu-id="6b059-134">Your app is now able to store a set of categories in local storage on the device and register with the notification hub whenever the user changes the selection of categories.</span></span>

## <a name="register-for-notifications"></a><span data-ttu-id="6b059-135">Registrace pro oznámení</span><span class="sxs-lookup"><span data-stu-id="6b059-135">Register for notifications</span></span>
<span data-ttu-id="6b059-136">Tyto kroky zaregistrovat do centra oznámení na spouštění pomocí kategorií, které byly uloženy v místním úložišti.</span><span class="sxs-lookup"><span data-stu-id="6b059-136">These steps register with the notification hub on startup using the categories that have been stored in local storage.</span></span>

> [!NOTE]
> <span data-ttu-id="6b059-137">Vzhledem k tomu, že kanál URI přiřazené pomocí Microsoft nabízené služby oznámení (MPNS) můžete změnit kdykoli, byste měli zaregistrovat pro oznámení často, aby se zabránilo selhání oznámení.</span><span class="sxs-lookup"><span data-stu-id="6b059-137">Because the channel URI assigned by the Microsoft Push Notification Service (MPNS) can change at any time, you should register for notifications frequently to avoid notification failures.</span></span> <span data-ttu-id="6b059-138">Tento příklad zaregistruje pro oznámení při každém spuštění aplikace.</span><span class="sxs-lookup"><span data-stu-id="6b059-138">This example registers for notifications every time that the app starts.</span></span> <span data-ttu-id="6b059-139">Pro aplikace, které jsou často spouštíte více než jednou denně, můžete pravděpodobně přeskočit registraci byla zachována šířka pásma, pokud od předchozí registrace uplynul méně než jeden den.</span><span class="sxs-lookup"><span data-stu-id="6b059-139">For apps that are run frequently, more than once a day, you can probably skip registration to preserve bandwidth if less than a day has passed since the previous registration.</span></span>
> 
> 

1. <span data-ttu-id="6b059-140">Otevřete soubor App.xaml.cs a přidejte **asynchronní** modifikátor k **Application_Launching** metodu a nahraďte kód registrace centra oznámení, které jste přidali v [Začínáme s Notification Hubs] následujícím kódem:</span><span class="sxs-lookup"><span data-stu-id="6b059-140">Open the App.xaml.cs file and add the **async** modifier to **Application_Launching** method and replace the Notification Hubs registration code that you added in [Get started with Notification Hubs] with the following code:</span></span>
   
        private async void Application_Launching(object sender, LaunchingEventArgs e)
        {
            var result = await notifications.SubscribeToCategories();
   
            if (result != null)
                System.Windows.Deployment.Current.Dispatcher.BeginInvoke(() =>
                {
                    MessageBox.Show("Registration Id :" + result.RegistrationId, "Registered", MessageBoxButton.OK);
                });
        }
   
    <span data-ttu-id="6b059-141">Tím je zajištěno, že při každém spuštění aplikace načte kategorie z místního úložiště a požadavky registraci pro tyto kategorie.</span><span class="sxs-lookup"><span data-stu-id="6b059-141">This makes sure that every time the app starts it retrieves the categories from local storage and requests a registration for these categories.</span></span>
2. <span data-ttu-id="6b059-142">V souboru projektu MainPage.xaml.cs, přidejte následující kód, který implementuje **OnNavigatedTo** metoda:</span><span class="sxs-lookup"><span data-stu-id="6b059-142">In the MainPage.xaml.cs project file, add the following code that implements the **OnNavigatedTo** method:</span></span>
   
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
   
    <span data-ttu-id="6b059-143">Tím se aktualizuje na základě stavu dříve uloženou kategorií hlavní stránce.</span><span class="sxs-lookup"><span data-stu-id="6b059-143">This updates the main page based on the status of previously saved categories.</span></span>

<span data-ttu-id="6b059-144">Aplikace je nyní dokončen a sadu kategorií můžete uložit do místního úložiště zařízení používá k registraci do centra oznámení pokaždé, když uživatel změní výběr kategorie.</span><span class="sxs-lookup"><span data-stu-id="6b059-144">The app is now complete and can store a set of categories in the device local storage used to register with the notification hub whenever the user changes the selection of categories.</span></span> <span data-ttu-id="6b059-145">V dalším kroku bude definujeme back-end, který kategorie oznámení můžete odesílat do této aplikace.</span><span class="sxs-lookup"><span data-stu-id="6b059-145">Next, we will define a backend that can send category notifications to this app.</span></span>

## <a name="sending-tagged-notifications"></a><span data-ttu-id="6b059-146">Odesílání oznámení s příznakem</span><span class="sxs-lookup"><span data-stu-id="6b059-146">Sending tagged notifications</span></span>
[!INCLUDE [notification-hubs-send-categories-template](../../includes/notification-hubs-send-categories-template.md)]

## <a name="run-the-app-and-generate-notifications"></a><span data-ttu-id="6b059-147">Spusťte aplikaci a generovat upozornění</span><span class="sxs-lookup"><span data-stu-id="6b059-147">Run the app and generate notifications</span></span>
1. <span data-ttu-id="6b059-148">Ve Visual Studiu stisknutím klávesy F5 zkompilování a spuštění aplikace.</span><span class="sxs-lookup"><span data-stu-id="6b059-148">In Visual Studio, press F5 to compile and start the app.</span></span>
   
    ![][1]
   
    <span data-ttu-id="6b059-149">Všimněte si, že aplikace uživatelského rozhraní, poskytuje sadu přepínačů, která vám umožní vybrat kategorie pro přihlášení k odběru.</span><span class="sxs-lookup"><span data-stu-id="6b059-149">Note that the app UI provides a set of toggles that lets you choose the categories to subscribe to.</span></span>
2. <span data-ttu-id="6b059-150">Povolit jednu nebo více kategorií přepínačů a potom klikněte na **přihlásit k odběru**.</span><span class="sxs-lookup"><span data-stu-id="6b059-150">Enable one or more categories toggles, then click **Subscribe**.</span></span>
   
    <span data-ttu-id="6b059-151">Aplikace převede vybraných kategorií značky a požaduje novou registraci zařízení pro vybranou značky z centra oznámení.</span><span class="sxs-lookup"><span data-stu-id="6b059-151">The app converts the selected categories into tags and requests a new device registration for the selected tags from the notification hub.</span></span> <span data-ttu-id="6b059-152">Registrovaný kategorie se vrátí a zobrazí v dialogu.</span><span class="sxs-lookup"><span data-stu-id="6b059-152">The registered categories are returned and displayed in a dialog.</span></span>
   
    ![][2]
3. <span data-ttu-id="6b059-153">Po přijetí potvrzení, že byly vaše kategorie odběru byla dokončena, spusťte konzolovou aplikaci odesílat oznámení pro každé kategorie.</span><span class="sxs-lookup"><span data-stu-id="6b059-153">After receiving a confirmation that your categories were subscription completed, run the console app to send notifications for each categories.</span></span> <span data-ttu-id="6b059-154">Ověřte, že dostanete oznámení pro přihlášení k odběru kategorií.</span><span class="sxs-lookup"><span data-stu-id="6b059-154">Verify you only receive a notification for the categories you have subscribed to.</span></span>
   
    ![][3]

<span data-ttu-id="6b059-155">Dokončili jste v tomto tématu.</span><span class="sxs-lookup"><span data-stu-id="6b059-155">You have completed this topic.</span></span>

<!--##Next steps

In this tutorial we learned how to broadcast breaking news by category. Consider completing one of the following tutorials that highlight other advanced Notification Hubs scenarios:

+ [Use Notification Hubs to broadcast localized breaking news]

    Learn how to expand the breaking news app to enable sending localized notifications.

+ [Notify users with Notification Hubs]

    Learn how to push notifications to specific authenticated users. This is a good solution for sending notifications only to specific users.
-->

<!-- Anchors. -->
[Add category selection to the app]: #adding-categories
[Register for notifications]: #register
[Send notifications from your back-end]: #send
[Run the app and generate notifications]: #test-app
[Next Steps]: #next-steps

<!-- Images. -->
[1]: ./media/notification-hubs-windows-phone-send-breaking-news/notification-hub-breakingnews.png
[2]: ./media/notification-hubs-windows-phone-send-breaking-news/notification-hub-registration.png
[3]: ./media/notification-hubs-windows-phone-send-breaking-news/notification-hub-toast.png



<!-- URLs.-->
<span data-ttu-id="6b059-156">[Začínáme s Notification Hubs]: /manage/services/notification-hubs/get-started-notification-hubs-wp8/</span><span class="sxs-lookup"><span data-stu-id="6b059-156">[Get started with Notification Hubs]: /manage/services/notification-hubs/get-started-notification-hubs-wp8/</span></span>
[Use Notification Hubs to broadcast localized breaking news]: ../breakingnews-localized-wp8.md
[Notify users with Notification Hubs]: /manage/services/notification-hubs/notify-users/
[Mobile Service]: /develop/mobile/tutorials/get-started
[Notification Hubs Guidance]: http://msdn.microsoft.com/library/jj927170.aspx
[Notification Hubs How-To for Windows Phone]: ??

