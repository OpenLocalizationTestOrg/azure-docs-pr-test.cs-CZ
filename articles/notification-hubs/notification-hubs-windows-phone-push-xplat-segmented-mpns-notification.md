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
# <a name="use-notification-hubs-toosend-breaking-news"></a><span data-ttu-id="82a35-103">Použít nejnovější zprávy přes toosend centra oznámení</span><span class="sxs-lookup"><span data-stu-id="82a35-103">Use Notification Hubs toosend breaking news</span></span>
[!INCLUDE [notification-hubs-selector-breaking-news](../../includes/notification-hubs-selector-breaking-news.md)]

## <a name="overview"></a><span data-ttu-id="82a35-104">Přehled</span><span class="sxs-lookup"><span data-stu-id="82a35-104">Overview</span></span>
<span data-ttu-id="82a35-105">Toto téma ukazuje, jak toouse Azure Notification Hubs toobroadcast nejnovější zprávy oznámení tooa aplikace Windows Phone 8.0/8.1 Silverlight.</span><span class="sxs-lookup"><span data-stu-id="82a35-105">This topic shows you how toouse Azure Notification Hubs toobroadcast breaking news notifications tooa Windows Phone 8.0/8.1 Silverlight app.</span></span> <span data-ttu-id="82a35-106">Pokud cílíte na Windows Store nebo Windows Phone 8.1 aplikace, naleznete v tootoohello [univerzální pro Windows](notification-hubs-windows-notification-dotnet-push-xplat-segmented-wns.md) verze.</span><span class="sxs-lookup"><span data-stu-id="82a35-106">If you are targeting Windows Store or Windows Phone 8.1 app, please refer tootoohello [Windows Universal](notification-hubs-windows-notification-dotnet-push-xplat-segmented-wns.md) version.</span></span> <span data-ttu-id="82a35-107">Po dokončení budete mít možnost tooregister pro nejnovější novinky kategorií, které vás zajímají a přijímat pouze nabízená oznámení pro tyto kategorie.</span><span class="sxs-lookup"><span data-stu-id="82a35-107">When complete, you will be able tooregister for breaking news categories you are interested in, and receive only push notifications for those categories.</span></span> <span data-ttu-id="82a35-108">Tento scénář je běžný vzor velký počet aplikací, kde mají oznámení odeslaných toobe toogroups uživatelů, kteří mají dříve deklarované zájem o jejich, např. čtečku RSS, aplikace pro Hudba ventilátory, atd.</span><span class="sxs-lookup"><span data-stu-id="82a35-108">This scenario is a common pattern for many apps where notifications have toobe sent toogroups of users that have previously declared interest in them, e.g. RSS reader, apps for music fans, etc.</span></span>

<span data-ttu-id="82a35-109">Všesměrového vysílání scénáře jsou povolené zahrnutím jeden nebo více *značky* při vytváření registrace v centru oznámení hello.</span><span class="sxs-lookup"><span data-stu-id="82a35-109">Broadcast scenarios are enabled by including one or more *tags* when creating a registration in hello notification hub.</span></span> <span data-ttu-id="82a35-110">Pokud jsou oznámení odesílána tooa značky, všechna zařízení, která jste zaregistrovali hello značky obdrží oznámení hello.</span><span class="sxs-lookup"><span data-stu-id="82a35-110">When notifications are sent tooa tag, all devices that have registered for hello tag will receive hello notification.</span></span> <span data-ttu-id="82a35-111">Protože značky jsou jednoduše řetězce, nemají toobe předem zřízený.</span><span class="sxs-lookup"><span data-stu-id="82a35-111">Because tags are simply strings, they do not have toobe provisioned in advance.</span></span> <span data-ttu-id="82a35-112">Další informace o značkách najdete v části příliš[směrování centra oznámení a značky výrazy](notification-hubs-tags-segment-push-message.md).</span><span class="sxs-lookup"><span data-stu-id="82a35-112">For more information about tags, refer too[Notification Hubs Routing and Tag Expressions](notification-hubs-tags-segment-push-message.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="82a35-113">Požadavky</span><span class="sxs-lookup"><span data-stu-id="82a35-113">Prerequisites</span></span>
<span data-ttu-id="82a35-114">Toto téma je založený na hello aplikace, které jste vytvořili v [Začínáme s Notification Hubs].</span><span class="sxs-lookup"><span data-stu-id="82a35-114">This topic builds on hello app you created in [Get started with Notification Hubs].</span></span> <span data-ttu-id="82a35-115">Před zahájením tohoto kurzu, musí jste již dokončili [Začínáme s Notification Hubs].</span><span class="sxs-lookup"><span data-stu-id="82a35-115">Before starting this tutorial, you must have already completed [Get started with Notification Hubs].</span></span>

## <a name="add-category-selection-toohello-app"></a><span data-ttu-id="82a35-116">Přidat aplikaci toohello výběru kategorie</span><span class="sxs-lookup"><span data-stu-id="82a35-116">Add category selection toohello app</span></span>
<span data-ttu-id="82a35-117">Hello prvním krokem je tooadd hello uživatelského rozhraní elementy tooyour existující hlavní stránce umožňujících hello uživatele tooselect kategorie tooregister.</span><span class="sxs-lookup"><span data-stu-id="82a35-117">hello first step is tooadd hello UI elements tooyour existing main page that enable hello user tooselect categories tooregister.</span></span> <span data-ttu-id="82a35-118">kategorie Hello vybrané uživatelem se ukládají na hello zařízení.</span><span class="sxs-lookup"><span data-stu-id="82a35-118">hello categories selected by a user are stored on hello device.</span></span> <span data-ttu-id="82a35-119">Při spuštění aplikace hello, registrace zařízení se vytvoří v centru oznámení s hello vybrané kategorie jako značky.</span><span class="sxs-lookup"><span data-stu-id="82a35-119">When hello app starts, a device registration is created in your notification hub with hello selected categories as tags.</span></span>

1. <span data-ttu-id="82a35-120">Otevřete soubor projektu MainPage.xaml hello a pak nahraďte hello **mřížky** elementů s názvem `TitlePanel` a `ContentPanel` s hello následující kód:</span><span class="sxs-lookup"><span data-stu-id="82a35-120">Open hello MainPage.xaml project file, then replace hello **Grid** elements named `TitlePanel` and `ContentPanel` with hello following code:</span></span>
   
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
2. <span data-ttu-id="82a35-121">V projektu hello, vytvořte novou třídu s názvem **oznámení**, přidejte hello **veřejné** toohello Modifikátor třídy definice a potom přidejte následující hello **pomocí** příkazy toohello nový soubor kódu:</span><span class="sxs-lookup"><span data-stu-id="82a35-121">In hello project, create a new class named **Notifications**, add hello **public** modifier toohello class definition, then add hello following **using** statements toohello new code file:</span></span>
   
        using Microsoft.Phone.Notification;
        using Microsoft.WindowsAzure.Messaging;
        using System.IO.IsolatedStorage;
        using System.Windows;
3. <span data-ttu-id="82a35-122">Kopírování hello následující kód do nové hello **oznámení** třídy:</span><span class="sxs-lookup"><span data-stu-id="82a35-122">Copy hello following code into hello new **Notifications** class:</span></span>
   
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

    <span data-ttu-id="82a35-123">Tato třída se používá hello izolované úložiště toostore hello kategorie příspěvků, že toto zařízení je tooreceive.</span><span class="sxs-lookup"><span data-stu-id="82a35-123">This class uses hello isolated storage toostore hello categories of news that this device is tooreceive.</span></span> <span data-ttu-id="82a35-124">Obsahuje také metody tooregister pro tyto kategorie pomocí [šablony](notification-hubs-templates-cross-platform-push-messages.md) registrace oznámení.</span><span class="sxs-lookup"><span data-stu-id="82a35-124">It also contains methods tooregister for these categories using a [template](notification-hubs-templates-cross-platform-push-messages.md) notification registration.</span></span>


1. <span data-ttu-id="82a35-125">Soubor projektu App.xaml.cs hello, přidejte následující vlastnost toohello hello **aplikace** třídy.</span><span class="sxs-lookup"><span data-stu-id="82a35-125">In hello App.xaml.cs project file, add hello following property toohello **App** class.</span></span> <span data-ttu-id="82a35-126">Nahraďte hello `<hub name>` a `<connection string with listen access>` zástupné symboly oznámení centra název a hello připojovacím řetězcem pro *DefaultListenSharedAccessSignature* kterou jste získali dříve.</span><span class="sxs-lookup"><span data-stu-id="82a35-126">Replace hello `<hub name>` and `<connection string with listen access>` placeholders with your notification hub name and hello connection string for *DefaultListenSharedAccessSignature* that you obtained earlier.</span></span>
   
        public Notifications notifications = new Notifications("<hub name>", "<connection string with listen access>");
   
   > [!NOTE]
   > <span data-ttu-id="82a35-127">Protože přihlašovací údaje, které jsou distribuované s klientskou aplikaci nejsou obecně bezpečné, musí distribuovat hello klíč pro přístup k naslouchání pouze s vaší klientské aplikace.</span><span class="sxs-lookup"><span data-stu-id="82a35-127">Because credentials that are distributed with a client app are not generally secure, you should only distribute hello key for listen access with your client app.</span></span> <span data-ttu-id="82a35-128">Poslechněte umožní přístup k vaší aplikaci tooregister oznámení, ale existující registrace nemůže být upravena a nelze odeslat oznámení.</span><span class="sxs-lookup"><span data-stu-id="82a35-128">Listen access enables your app tooregister for notifications, but existing registrations cannot be modified and notifications cannot be sent.</span></span> <span data-ttu-id="82a35-129">Hello úplné přístupový klíč se používá ve službě Zabezpečené back-end pro zasílání oznámení a změna existující registrace.</span><span class="sxs-lookup"><span data-stu-id="82a35-129">hello full access key is used in a secured backend service for sending notifications and changing existing registrations.</span></span>
   > 
   > 
2. <span data-ttu-id="82a35-130">V MainPage.xaml.cs přidejte následující řádek hello:</span><span class="sxs-lookup"><span data-stu-id="82a35-130">In your MainPage.xaml.cs, add hello following line:</span></span>
   
        using Windows.UI.Popups;
3. <span data-ttu-id="82a35-131">V souboru projektu hello MainPage.xaml.cs přidejte následující metodu hello:</span><span class="sxs-lookup"><span data-stu-id="82a35-131">In hello MainPage.xaml.cs project file, add hello following method:</span></span>
   
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
   
    <span data-ttu-id="82a35-132">Tato metoda vytvoří seznam kategorií a používá hello **oznámení** třídy toostore hello seznamu v místním úložišti hello a zaregistrujte hello značky odpovídající pomocí centra oznámení.</span><span class="sxs-lookup"><span data-stu-id="82a35-132">This method creates a list of categories and uses hello **Notifications** class toostore hello list in hello local storage and register hello corresponding tags with your notification hub.</span></span> <span data-ttu-id="82a35-133">Při změně kategorií, hello registrace se znovu vytvoří se nové kategorie hello.</span><span class="sxs-lookup"><span data-stu-id="82a35-133">When categories are changed, hello registration is recreated with hello new categories.</span></span>

<span data-ttu-id="82a35-134">Aplikace je nyní možné toostore sadu kategorií v místním úložišti na hello zařízení a zaregistrujte hello centra oznámení pokaždé, když změny uživatelů hello hello výběru kategorie.</span><span class="sxs-lookup"><span data-stu-id="82a35-134">Your app is now able toostore a set of categories in local storage on hello device and register with hello notification hub whenever hello user changes hello selection of categories.</span></span>

## <a name="register-for-notifications"></a><span data-ttu-id="82a35-135">Registrace pro oznámení</span><span class="sxs-lookup"><span data-stu-id="82a35-135">Register for notifications</span></span>
<span data-ttu-id="82a35-136">Tyto kroky zaregistrovat hello centra oznámení na spuštění pomocí hello kategorií, které byly uloženy v místním úložišti.</span><span class="sxs-lookup"><span data-stu-id="82a35-136">These steps register with hello notification hub on startup using hello categories that have been stored in local storage.</span></span>

> [!NOTE]
> <span data-ttu-id="82a35-137">Protože hello kanál URI přiřadila hello Microsoft nabízené služby oznámení (MPNS) můžete změnit kdykoli, byste měli zaregistrovat pro oznámení často tooavoid oznámení selhání.</span><span class="sxs-lookup"><span data-stu-id="82a35-137">Because hello channel URI assigned by hello Microsoft Push Notification Service (MPNS) can change at any time, you should register for notifications frequently tooavoid notification failures.</span></span> <span data-ttu-id="82a35-138">Tento příklad zaregistruje oznámení pokaždé, když spustí aplikaci hello.</span><span class="sxs-lookup"><span data-stu-id="82a35-138">This example registers for notifications every time that hello app starts.</span></span> <span data-ttu-id="82a35-139">Pro aplikace, které jsou často spouštíte více než jednou denně, pravděpodobně Pokud můžete přeskočit šířky pásma toopreserve registrace od předchozí registrace hello uplynul méně než jeden den.</span><span class="sxs-lookup"><span data-stu-id="82a35-139">For apps that are run frequently, more than once a day, you can probably skip registration toopreserve bandwidth if less than a day has passed since hello previous registration.</span></span>
> 
> 

1. <span data-ttu-id="82a35-140">Otevřete soubor App.xaml.cs hello a přidejte hello **asynchronní** modifikátor příliš**Application_Launching** metodu a nahraďte kód registrace Notification Hubs hello, který jste přidali v [Začínáme s Notification Hubs] s hello následující kód:</span><span class="sxs-lookup"><span data-stu-id="82a35-140">Open hello App.xaml.cs file and add hello **async** modifier too**Application_Launching** method and replace hello Notification Hubs registration code that you added in [Get started with Notification Hubs] with hello following code:</span></span>
   
        private async void Application_Launching(object sender, LaunchingEventArgs e)
        {
            var result = await notifications.SubscribeToCategories();
   
            if (result != null)
                System.Windows.Deployment.Current.Dispatcher.BeginInvoke(() =>
                {
                    MessageBox.Show("Registration Id :" + result.RegistrationId, "Registered", MessageBoxButton.OK);
                });
        }
   
    <span data-ttu-id="82a35-141">Tím je zajištěno, že při každém spuštění aplikace hello načte kategorie hello z místního úložiště a požadavky registraci pro tyto kategorie.</span><span class="sxs-lookup"><span data-stu-id="82a35-141">This makes sure that every time hello app starts it retrieves hello categories from local storage and requests a registration for these categories.</span></span>
2. <span data-ttu-id="82a35-142">V souboru projektu hello MainPage.xaml.cs přidejte následující kód, který implementuje hello hello **OnNavigatedTo** metoda:</span><span class="sxs-lookup"><span data-stu-id="82a35-142">In hello MainPage.xaml.cs project file, add hello following code that implements hello **OnNavigatedTo** method:</span></span>
   
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
   
    <span data-ttu-id="82a35-143">Tato aktualizace hello hlavní stránky založené na stav hello dříve uložit kategorií.</span><span class="sxs-lookup"><span data-stu-id="82a35-143">This updates hello main page based on hello status of previously saved categories.</span></span>

<span data-ttu-id="82a35-144">Hello aplikace je nyní dokončen a může ukládat sadu kategorií hello zařízení používá místní úložiště tooregister hello centra oznámení pokaždé, když změny uživatelů hello hello výběru kategorie.</span><span class="sxs-lookup"><span data-stu-id="82a35-144">hello app is now complete and can store a set of categories in hello device local storage used tooregister with hello notification hub whenever hello user changes hello selection of categories.</span></span> <span data-ttu-id="82a35-145">V dalším kroku bude definujeme back-end, který může odesílat kategorie oznámení toothis aplikace.</span><span class="sxs-lookup"><span data-stu-id="82a35-145">Next, we will define a backend that can send category notifications toothis app.</span></span>

## <a name="sending-tagged-notifications"></a><span data-ttu-id="82a35-146">Odesílání oznámení s příznakem</span><span class="sxs-lookup"><span data-stu-id="82a35-146">Sending tagged notifications</span></span>
[!INCLUDE [notification-hubs-send-categories-template](../../includes/notification-hubs-send-categories-template.md)]

## <a name="run-hello-app-and-generate-notifications"></a><span data-ttu-id="82a35-147">Spuštění aplikace hello a generovat oznámení</span><span class="sxs-lookup"><span data-stu-id="82a35-147">Run hello app and generate notifications</span></span>
1. <span data-ttu-id="82a35-148">V sadě Visual Studio stiskněte klávesu F5 toocompile a spusťte aplikaci hello.</span><span class="sxs-lookup"><span data-stu-id="82a35-148">In Visual Studio, press F5 toocompile and start hello app.</span></span>
   
    ![][1]
   
    <span data-ttu-id="82a35-149">Všimněte si, že přepíná hello aplikaci, kterou poskytuje sadu uživatelského rozhraní, která umožňuje vybrat toosubscribe kategorie hello k.</span><span class="sxs-lookup"><span data-stu-id="82a35-149">Note that hello app UI provides a set of toggles that lets you choose hello categories toosubscribe to.</span></span>
2. <span data-ttu-id="82a35-150">Povolit jednu nebo více kategorií přepínačů a potom klikněte na **přihlásit k odběru**.</span><span class="sxs-lookup"><span data-stu-id="82a35-150">Enable one or more categories toggles, then click **Subscribe**.</span></span>
   
    <span data-ttu-id="82a35-151">aplikace Hello převede hello vybrané kategorie značky a požaduje novou registraci zařízení pro hello vybrané značky z centra oznámení hello.</span><span class="sxs-lookup"><span data-stu-id="82a35-151">hello app converts hello selected categories into tags and requests a new device registration for hello selected tags from hello notification hub.</span></span> <span data-ttu-id="82a35-152">Hello registrované kategorie se vrátí a zobrazí v dialogu.</span><span class="sxs-lookup"><span data-stu-id="82a35-152">hello registered categories are returned and displayed in a dialog.</span></span>
   
    ![][2]
3. <span data-ttu-id="82a35-153">Po přijetí potvrzení, že byly vaše kategorie odběru byla dokončena, spusťte hello konzole aplikace toosend oznámení pro každé kategorie.</span><span class="sxs-lookup"><span data-stu-id="82a35-153">After receiving a confirmation that your categories were subscription completed, run hello console app toosend notifications for each categories.</span></span> <span data-ttu-id="82a35-154">Ověřte, že dostanete oznámení pro přihlášení k odběru kategorií hello.</span><span class="sxs-lookup"><span data-stu-id="82a35-154">Verify you only receive a notification for hello categories you have subscribed to.</span></span>
   
    ![][3]

<span data-ttu-id="82a35-155">Dokončili jste v tomto tématu.</span><span class="sxs-lookup"><span data-stu-id="82a35-155">You have completed this topic.</span></span>

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

