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
# <a name="use-notification-hubs-toosend-breaking-news"></a><span data-ttu-id="7886a-103">Použít nejnovější zprávy přes toosend centra oznámení</span><span class="sxs-lookup"><span data-stu-id="7886a-103">Use Notification Hubs toosend breaking news</span></span>
[!INCLUDE [notification-hubs-selector-breaking-news](../../includes/notification-hubs-selector-breaking-news.md)]

## <a name="overview"></a><span data-ttu-id="7886a-104">Přehled</span><span class="sxs-lookup"><span data-stu-id="7886a-104">Overview</span></span>
<span data-ttu-id="7886a-105">Toto téma ukazuje, jak toobroadcast Azure Notification Hubs toouse nejnovější zprávy oznámení tooa Windows Store nebo Windows Phone 8.1 (bez Silverlight) aplikace.</span><span class="sxs-lookup"><span data-stu-id="7886a-105">This topic shows you how toouse Azure Notification Hubs toobroadcast breaking news notifications tooa Windows Store or Windows Phone 8.1 (non-Silverlight) app.</span></span> <span data-ttu-id="7886a-106">Pokud cílíte na Windows Phone 8.1 Silverlight, naleznete v toohello [Windows Phone](notification-hubs-windows-phone-push-xplat-segmented-mpns-notification.md) verze.</span><span class="sxs-lookup"><span data-stu-id="7886a-106">If you are targeting Windows Phone 8.1 Silverlight, please refer toohello [Windows Phone](notification-hubs-windows-phone-push-xplat-segmented-mpns-notification.md) version.</span></span> <span data-ttu-id="7886a-107">Po dokončení budete mít možnost tooregister pro nejnovější novinky kategorií, které vás zajímají a přijímat pouze nabízená oznámení pro tyto kategorie.</span><span class="sxs-lookup"><span data-stu-id="7886a-107">When complete, you will be able tooregister for breaking news categories you are interested in, and receive only push notifications for those categories.</span></span> <span data-ttu-id="7886a-108">Tento scénář je běžný vzor velký počet aplikací, kde mají oznámení odeslaných toobe toogroups uživatelů, kteří mají zájem o je například čtečku RSS, aplikace pro Hudba ventilátory, dříve deklarované a tak dále.</span><span class="sxs-lookup"><span data-stu-id="7886a-108">This scenario is a common pattern for many apps where notifications have toobe sent toogroups of users that have previously declared interest in them, e.g. RSS reader, apps for music fans, and so on.</span></span> 

<span data-ttu-id="7886a-109">Všesměrového vysílání scénáře jsou povolené zahrnutím jeden nebo více *značky* při vytváření registrace v centru oznámení hello.</span><span class="sxs-lookup"><span data-stu-id="7886a-109">Broadcast scenarios are enabled by including one or more *tags* when creating a registration in hello notification hub.</span></span> <span data-ttu-id="7886a-110">Pokud jsou oznámení odesílána tooa značky, všechna zařízení, která jste zaregistrovali hello značky obdrží oznámení hello.</span><span class="sxs-lookup"><span data-stu-id="7886a-110">When notifications are sent tooa tag, all devices that have registered for hello tag will receive hello notification.</span></span> <span data-ttu-id="7886a-111">Protože značky jsou jednoduše řetězce, nemají toobe předem zřízený.</span><span class="sxs-lookup"><span data-stu-id="7886a-111">Because tags are simply strings, they do not have toobe provisioned in advance.</span></span> <span data-ttu-id="7886a-112">Další informace o značkách najdete v části příliš[směrování centra oznámení a značky výrazy](notification-hubs-tags-segment-push-message.md).</span><span class="sxs-lookup"><span data-stu-id="7886a-112">For more information about tags, refer too[Notification Hubs Routing and Tag Expressions](notification-hubs-tags-segment-push-message.md).</span></span>

> [!NOTE]
> <span data-ttu-id="7886a-113">Windows Store a verze projekty Windows Phone 8.1 a starší nejsou podporovány v aplikaci Visual Studio 2017.</span><span class="sxs-lookup"><span data-stu-id="7886a-113">Windows Store and Windows Phone projects version 8.1 and earlier are not supported in Visual Studio 2017.</span></span>  <span data-ttu-id="7886a-114">Další informace najdete v tématu [Cílení na platformy a kompatibilita v sadě Visual Studio 2017](https://www.visualstudio.com/en-us/productinfo/vs2017-compatibility-vs).</span><span class="sxs-lookup"><span data-stu-id="7886a-114">For more information, see [Visual Studio 2017 Platform Targeting and Compatibility](https://www.visualstudio.com/en-us/productinfo/vs2017-compatibility-vs).</span></span> 

## <a name="prerequisites"></a><span data-ttu-id="7886a-115">Požadavky</span><span class="sxs-lookup"><span data-stu-id="7886a-115">Prerequisites</span></span>
<span data-ttu-id="7886a-116">Toto téma je založený na hello aplikace, které jste vytvořili v [Začínáme s Notification Hubs][get-started].</span><span class="sxs-lookup"><span data-stu-id="7886a-116">This topic builds on hello app you created in [Get started with Notification Hubs][get-started].</span></span> <span data-ttu-id="7886a-117">Před zahájením tohoto kurzu, musí jste již dokončili [Začínáme s Notification Hubs][get-started].</span><span class="sxs-lookup"><span data-stu-id="7886a-117">Before starting this tutorial, you must have already completed [Get started with Notification Hubs][get-started].</span></span>

## <a name="add-category-selection-toohello-app"></a><span data-ttu-id="7886a-118">Přidat aplikaci toohello výběru kategorie</span><span class="sxs-lookup"><span data-stu-id="7886a-118">Add category selection toohello app</span></span>
<span data-ttu-id="7886a-119">Hello prvním krokem je tooadd hello uživatelského rozhraní elementy tooyour existující hlavní stránce umožňujících hello uživatele tooselect kategorie tooregister.</span><span class="sxs-lookup"><span data-stu-id="7886a-119">hello first step is tooadd hello UI elements tooyour existing main page that enable hello user tooselect categories tooregister.</span></span> <span data-ttu-id="7886a-120">kategorie Hello vybrané uživatelem se ukládají na hello zařízení.</span><span class="sxs-lookup"><span data-stu-id="7886a-120">hello categories selected by a user are stored on hello device.</span></span> <span data-ttu-id="7886a-121">Při spuštění aplikace hello, registrace zařízení se vytvoří v centru oznámení s hello vybrané kategorie jako značky.</span><span class="sxs-lookup"><span data-stu-id="7886a-121">When hello app starts, a device registration is created in your notification hub with hello selected categories as tags.</span></span>

1. <span data-ttu-id="7886a-122">Otevřete soubor projektu MainPage.xaml hello pak kopie hello následující kód v hello **mřížky** element:</span><span class="sxs-lookup"><span data-stu-id="7886a-122">Open hello MainPage.xaml project file, then copy hello following code in hello **Grid** element:</span></span>
   
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
2. <span data-ttu-id="7886a-123">Klikněte pravým tlačítkem na hello **sdílené** projekt a přidejte novou třídu s názvem **oznámení**, přidejte hello **veřejné** modifikátor toohello definici třídy a pak přidejte následující hello **pomocí** příkazy toohello nového souboru kódu:</span><span class="sxs-lookup"><span data-stu-id="7886a-123">Right click hello **Shared** project and add a new class named **Notifications**, add hello **public** modifier toohello class definition, then add hello following **using** statements toohello new code file:</span></span>
   
        using Windows.Networking.PushNotifications;
        using Microsoft.WindowsAzure.Messaging;
        using Windows.Storage;
        using System.Threading.Tasks;
3. <span data-ttu-id="7886a-124">Kopírování hello následující kód do nové hello **oznámení** třídy:</span><span class="sxs-lookup"><span data-stu-id="7886a-124">Copy hello following code into hello new **Notifications** class:</span></span>
   
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
   
    <span data-ttu-id="7886a-125">Tato třída se používá místní úložiště hello toostore hello kategorie zprávy, že toto zařízení má tooreceive.</span><span class="sxs-lookup"><span data-stu-id="7886a-125">This class uses hello local storage toostore hello categories of news that this device has tooreceive.</span></span> <span data-ttu-id="7886a-126">Všimněte si, že se namísto volání hello *RegisterNativeAsync* metoda říkáme *RegisterTemplateAsync* tooregister hello kategorií pomocí šablony registrace.</span><span class="sxs-lookup"><span data-stu-id="7886a-126">Note that instead of calling hello *RegisterNativeAsync* method we call *RegisterTemplateAsync* tooregister for hello categories using a template registration.</span></span> 
   
    <span data-ttu-id="7886a-127">Poskytujeme také název šablony hello ("simpleWNSTemplateExample"), protože jsme chtít tooregister více než jednu šablonu (například jeden pro informační zprávy) a jeden pro dlaždice a potřebujeme tooname je v pořadí možné tooupdate toobe nebo je odstranit.</span><span class="sxs-lookup"><span data-stu-id="7886a-127">We also provide a name for hello template ("simpleWNSTemplateExample"), because we might want tooregister more than one template (for instance one for toast notifications and one for tiles) and we need tooname them in order toobe able tooupdate or delete them.</span></span>
   
    <span data-ttu-id="7886a-128">Všimněte si, že pokud se zařízení zaregistruje několik šablon s hello stejná značka, příchozí zprávy cílení, že bude mít za následek značky doručit více oznámení toohello zařízení, (jeden pro každé šablony).</span><span class="sxs-lookup"><span data-stu-id="7886a-128">Note that if a device registers multiple templates with hello same tag, an incoming message targeting that tag will result in multiple notifications delivered toohello device (one for each template).</span></span> <span data-ttu-id="7886a-129">Toto chování je užitečné, když hello stejné logické zpráva má tooresult v rámci více visual oznámení pro instanci zobrazující oznámení a oznámení v aplikaci pro Windows Store.</span><span class="sxs-lookup"><span data-stu-id="7886a-129">This behavior is useful when hello same logical message has tooresult in multiple visual notifications, for instance showing both a badge and a toast in a Windows Store application.</span></span>
   
    <span data-ttu-id="7886a-130">Další informace o šablonách najdete v tématu [šablony](notification-hubs-templates-cross-platform-push-messages.md).</span><span class="sxs-lookup"><span data-stu-id="7886a-130">For more information on templates, see [Templates](notification-hubs-templates-cross-platform-push-messages.md).</span></span>
4. <span data-ttu-id="7886a-131">Soubor projektu App.xaml.cs hello, přidejte následující vlastnost toohello hello **aplikace** třídy:</span><span class="sxs-lookup"><span data-stu-id="7886a-131">In hello App.xaml.cs project file, add hello following property toohello **App** class:</span></span>
   
        public Notifications notifications = new Notifications("<hub name>", "<connection string with listen access>");
   
    <span data-ttu-id="7886a-132">Tato vlastnost je použité toocreate a přístup **oznámení** instance.</span><span class="sxs-lookup"><span data-stu-id="7886a-132">This property is used toocreate and access a **Notifications** instance.</span></span>
   
    <span data-ttu-id="7886a-133">V hello výše kódu, nahraďte hello `<hub name>` a `<connection string with listen access>` zástupné symboly oznámení centra název a hello připojovacím řetězcem pro *DefaultListenSharedAccessSignature* kterou jste získali dříve.</span><span class="sxs-lookup"><span data-stu-id="7886a-133">In hello above code, replace hello `<hub name>` and `<connection string with listen access>` placeholders with your notification hub name and hello connection string for *DefaultListenSharedAccessSignature* that you obtained earlier.</span></span>
   
   > [!NOTE]
   > <span data-ttu-id="7886a-134">Protože přihlašovací údaje, které jsou distribuované s klientskou aplikaci nejsou obecně bezpečné, musí distribuovat hello klíč pro přístup k naslouchání pouze s vaší klientské aplikace.</span><span class="sxs-lookup"><span data-stu-id="7886a-134">Because credentials that are distributed with a client app are not generally secure, you should only distribute hello key for listen access with your client app.</span></span> <span data-ttu-id="7886a-135">Poslechněte umožní přístup k vaší aplikaci tooregister oznámení, ale existující registrace nemůže být upravena a nelze odeslat oznámení.</span><span class="sxs-lookup"><span data-stu-id="7886a-135">Listen access enables your app tooregister for notifications, but existing registrations cannot be modified and notifications cannot be sent.</span></span> <span data-ttu-id="7886a-136">Hello úplné přístupový klíč se používá ve službě Zabezpečené back-end pro zasílání oznámení a změna existující registrace.</span><span class="sxs-lookup"><span data-stu-id="7886a-136">hello full access key is used in a secured backend service for sending notifications and changing existing registrations.</span></span>
   > 
   > 
5. <span data-ttu-id="7886a-137">V MainPage.xaml.cs přidejte následující řádek hello:</span><span class="sxs-lookup"><span data-stu-id="7886a-137">In your MainPage.xaml.cs, add hello following line:</span></span>
   
        using Windows.UI.Popups;
6. <span data-ttu-id="7886a-138">V souboru projektu hello MainPage.xaml.cs přidejte následující metodu hello:</span><span class="sxs-lookup"><span data-stu-id="7886a-138">In hello MainPage.xaml.cs project file, add hello following method:</span></span>
   
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
   
    <span data-ttu-id="7886a-139">Tato metoda vytvoří seznam kategorií a používá hello **oznámení** třídy toostore hello seznamu v místním úložišti hello a zaregistrujte hello značky odpovídající pomocí centra oznámení.</span><span class="sxs-lookup"><span data-stu-id="7886a-139">This method creates a list of categories and uses hello **Notifications** class toostore hello list in hello local storage and register hello corresponding tags with your notification hub.</span></span> <span data-ttu-id="7886a-140">Při změně kategorií, hello registrace se znovu vytvoří se nové kategorie hello.</span><span class="sxs-lookup"><span data-stu-id="7886a-140">When categories are changed, hello registration is recreated with hello new categories.</span></span>

<span data-ttu-id="7886a-141">Aplikace je nyní možné toostore sadu kategorií v místním úložišti na hello zařízení a zaregistrujte hello centra oznámení pokaždé, když změny uživatelů hello hello výběru kategorie.</span><span class="sxs-lookup"><span data-stu-id="7886a-141">Your app is now able toostore a set of categories in local storage on hello device and register with hello notification hub whenever hello user changes hello selection of categories.</span></span>

## <a name="register-for-notifications"></a><span data-ttu-id="7886a-142">Registrace pro oznámení</span><span class="sxs-lookup"><span data-stu-id="7886a-142">Register for notifications</span></span>
<span data-ttu-id="7886a-143">Tyto kroky zaregistrovat hello centra oznámení na spuštění pomocí hello kategorií, které byly uloženy v místním úložišti.</span><span class="sxs-lookup"><span data-stu-id="7886a-143">These steps register with hello notification hub on startup using hello categories that have been stored in local storage.</span></span>

> [!NOTE]
> <span data-ttu-id="7886a-144">Protože hello kanál URI přiřadila hello služby oznámení Windows (WNS) můžete změnit kdykoli, byste měli zaregistrovat pro oznámení často tooavoid oznámení selhání.</span><span class="sxs-lookup"><span data-stu-id="7886a-144">Because hello channel URI assigned by hello Windows Notification Service (WNS) can change at any time, you should register for notifications frequently tooavoid notification failures.</span></span> <span data-ttu-id="7886a-145">Tento příklad zaregistruje oznámení pokaždé, když spustí aplikaci hello.</span><span class="sxs-lookup"><span data-stu-id="7886a-145">This example registers for notification every time that hello app starts.</span></span> <span data-ttu-id="7886a-146">Pro aplikace, které jsou často spouštíte více než jednou denně, pravděpodobně Pokud můžete přeskočit šířky pásma toopreserve registrace od předchozí registrace hello uplynul méně než jeden den.</span><span class="sxs-lookup"><span data-stu-id="7886a-146">For apps that are run frequently, more than once a day, you can probably skip registration toopreserve bandwidth if less than a day has passed since hello previous registration.</span></span>
> 
> 

1. <span data-ttu-id="7886a-147">Otevřete hello App.xaml.cs soubor a aktualizace hello **InitNotificationsAsync** metoda toouse hello `notifications` toosubscribe třídy založené na kategoriích.</span><span class="sxs-lookup"><span data-stu-id="7886a-147">Open hello App.xaml.cs file and update hello **InitNotificationsAsync** method toouse hello `notifications` class toosubscribe based on categories.</span></span>
   
        // *** Remove or comment out these lines *** 
        //var channel = await PushNotificationChannelManager.CreatePushNotificationChannelForApplicationAsync();
        //var hub = new NotificationHub("your hub name", "your listen connection string");
        //var result = await hub.RegisterNativeAsync(channel.Uri);
   
        var result = await notifications.SubscribeToCategories();
   
    <span data-ttu-id="7886a-148">Tím je zajištěno, že při každém spuštění aplikace hello načte kategorie hello z místního úložiště a požadavky registrace pro tyto kategorie.</span><span class="sxs-lookup"><span data-stu-id="7886a-148">This makes sure that every time hello app starts it retrieves hello categories from local storage and requests a registeration for these categories.</span></span> <span data-ttu-id="7886a-149">Hello **InitNotificationsAsync** metoda byla vytvořena jako součást hello [Začínáme s Notification Hubs] [ get-started] kurzu.</span><span class="sxs-lookup"><span data-stu-id="7886a-149">hello **InitNotificationsAsync** method was created as part of hello [Get started with Notification Hubs][get-started] tutorial.</span></span>
2. <span data-ttu-id="7886a-150">V souboru projektu hello MainPage.xaml.cs přidejte následující kód toohello hello *OnNavigatedTo* metoda:</span><span class="sxs-lookup"><span data-stu-id="7886a-150">In hello MainPage.xaml.cs project file, add hello following code toohello *OnNavigatedTo* method:</span></span>
   
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
   
    <span data-ttu-id="7886a-151">Tato aktualizace hello hlavní stránky založené na stav hello dříve uložit kategorií.</span><span class="sxs-lookup"><span data-stu-id="7886a-151">This updates hello main page based on hello status of previously saved categories.</span></span>

<span data-ttu-id="7886a-152">Hello aplikace je nyní dokončen a může ukládat sadu kategorií hello zařízení používá místní úložiště tooregister hello centra oznámení pokaždé, když změny uživatelů hello hello výběru kategorie.</span><span class="sxs-lookup"><span data-stu-id="7886a-152">hello app is now complete and can store a set of categories in hello device local storage used tooregister with hello notification hub whenever hello user changes hello selection of categories.</span></span> <span data-ttu-id="7886a-153">V dalším kroku bude definujeme back-end, který může odesílat kategorie oznámení toothis aplikace.</span><span class="sxs-lookup"><span data-stu-id="7886a-153">Next, we will define a backend that can send category notifications toothis app.</span></span>

## <a name="sending-tagged-notifications"></a><span data-ttu-id="7886a-154">Odesílání oznámení s příznakem</span><span class="sxs-lookup"><span data-stu-id="7886a-154">Sending tagged notifications</span></span>
[!INCLUDE [notification-hubs-send-categories-template](../../includes/notification-hubs-send-categories-template.md)]

## <a name="run-hello-app-and-generate-notifications"></a><span data-ttu-id="7886a-155">Spuštění aplikace hello a generovat oznámení</span><span class="sxs-lookup"><span data-stu-id="7886a-155">Run hello app and generate notifications</span></span>
1. <span data-ttu-id="7886a-156">V sadě Visual Studio stiskněte klávesu F5 toocompile a spusťte aplikaci hello.</span><span class="sxs-lookup"><span data-stu-id="7886a-156">In Visual Studio, press F5 toocompile and start hello app.</span></span>
   
    ![][1]
   
    <span data-ttu-id="7886a-157">Všimněte si, že přepíná hello aplikaci, kterou poskytuje sadu uživatelského rozhraní, která umožňuje vybrat toosubscribe kategorie hello k.</span><span class="sxs-lookup"><span data-stu-id="7886a-157">Note that hello app UI provides a set of toggles that lets you choose hello categories toosubscribe to.</span></span>
2. <span data-ttu-id="7886a-158">Povolit jednu nebo více kategorií přepínačů a potom klikněte na **přihlásit k odběru**.</span><span class="sxs-lookup"><span data-stu-id="7886a-158">Enable one or more categories toggles, then click **Subscribe**.</span></span>
   
    <span data-ttu-id="7886a-159">aplikace Hello převede hello vybrané kategorie značky a požaduje novou registraci zařízení pro hello vybrané značky z centra oznámení hello.</span><span class="sxs-lookup"><span data-stu-id="7886a-159">hello app converts hello selected categories into tags and requests a new device registration for hello selected tags from hello notification hub.</span></span> <span data-ttu-id="7886a-160">Hello registrované kategorie se vrátí a zobrazí v dialogu.</span><span class="sxs-lookup"><span data-stu-id="7886a-160">hello registered categories are returned and displayed in a dialog.</span></span>
   
    ![][19]
3. <span data-ttu-id="7886a-161">Odeslání nové oznámení z back-end hello v jednom z následujících způsobů hello:</span><span class="sxs-lookup"><span data-stu-id="7886a-161">Send a new notification from hello backend in one of hello following ways:</span></span>
   
   * <span data-ttu-id="7886a-162">**Konzolové aplikace:** spustit hello konzolovou aplikaci.</span><span class="sxs-lookup"><span data-stu-id="7886a-162">**Console app:** start hello console app.</span></span>
   * <span data-ttu-id="7886a-163">**Java/PHP:** spuštění vaší aplikace nebo skriptu.</span><span class="sxs-lookup"><span data-stu-id="7886a-163">**Java/PHP:** run your app/script.</span></span>
     
     <span data-ttu-id="7886a-164">Oznámení pro hello vybrané kategorie se zobrazí jako informační zprávy.</span><span class="sxs-lookup"><span data-stu-id="7886a-164">Notifications for hello selected categories appear as toast notifications.</span></span>
     
     ![][14]

## <a name="next-steps"></a><span data-ttu-id="7886a-165">Další kroky</span><span class="sxs-lookup"><span data-stu-id="7886a-165">Next steps</span></span>
<span data-ttu-id="7886a-166">V tomto kurzu jsme se dozvěděli, jak toobroadcast nejnovější zprávy přes podle kategorie.</span><span class="sxs-lookup"><span data-stu-id="7886a-166">In this tutorial we learned how toobroadcast breaking news by category.</span></span> <span data-ttu-id="7886a-167">Vezměte v úvahu dokončení jednu z následujících návodů, které zvýrazněte pokročilé scénáře Notification Hubs hello:</span><span class="sxs-lookup"><span data-stu-id="7886a-167">Consider completing one of hello following tutorials that highlight other advanced Notification Hubs scenarios:</span></span>

* <span data-ttu-id="7886a-168">[Použití centra oznámení toobroadcast lokalizované novinek]</span><span class="sxs-lookup"><span data-stu-id="7886a-168">[Use Notification Hubs toobroadcast localized breaking news]</span></span>
  
    <span data-ttu-id="7886a-169">Zjistěte, jak lokalizované tooexpand hello nejnovější novinky aplikace tooenable odesílání oznámení.</span><span class="sxs-lookup"><span data-stu-id="7886a-169">Learn how tooexpand hello breaking news app tooenable sending localized notifications.</span></span>

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
