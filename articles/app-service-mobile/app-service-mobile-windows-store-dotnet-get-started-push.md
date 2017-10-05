---
title: "Přidání nabízených oznámení do aplikace pro univerzální platformu Windows (UWP) | Microsoft Docs"
description: "Naučte se používat Azure App Service Mobile Apps a Azure Notification Hubs k odesílání nabízených oznámení do aplikace pro univerzální platformu Windows (UWP)."
services: app-service\mobile,notification-hubs
documentationcenter: windows
author: ysxu
manager: syntaxc4
editor: 
ms.assetid: 6de1b9d4-bd28-43e4-8db4-94cd3b187aa3
ms.service: app-service-mobile
ms.workload: mobile
ms.tgt_pltfrm: mobile-windows
ms.devlang: dotnet
ms.topic: article
ms.date: 10/12/2016
ms.author: yuaxu
ms.openlocfilehash: a14bb0320c1f6a563f766a6a0fad5cf556fe7b70
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="add-push-notifications-to-your-windows-app"></a><span data-ttu-id="804af-103">Přidání nabízených oznámení do aplikace pro Windows</span><span class="sxs-lookup"><span data-stu-id="804af-103">Add push notifications to your Windows app</span></span>
[!INCLUDE [app-service-mobile-selector-get-started-push](../../includes/app-service-mobile-selector-get-started-push.md)]

## <a name="overview"></a><span data-ttu-id="804af-104">Přehled</span><span class="sxs-lookup"><span data-stu-id="804af-104">Overview</span></span>
<span data-ttu-id="804af-105">V tomto kurzu přidáte nabízená oznámení [Windows úvodní](app-service-mobile-windows-store-dotnet-get-started.md) projektu tak, aby nabízených oznámení se odešle do zařízení pokaždé, když vložení záznamu.</span><span class="sxs-lookup"><span data-stu-id="804af-105">In this tutorial, you add push notifications to the [Windows quick start](app-service-mobile-windows-store-dotnet-get-started.md) project so that a push notification is sent to the device every time a record is inserted.</span></span>

<span data-ttu-id="804af-106">Pokud použijete serverový projekt stažené rychlý start, budete potřebovat balíček rozšíření nabízená oznámení.</span><span class="sxs-lookup"><span data-stu-id="804af-106">If you do not use the downloaded quick start server project, you will need the push notification extension package.</span></span> <span data-ttu-id="804af-107">V tématu [pracovat s .NET back-end serveru SDK pro Azure Mobile Apps](app-service-mobile-dotnet-backend-how-to-use-server-sdk.md) Další informace.</span><span class="sxs-lookup"><span data-stu-id="804af-107">See [Work with the .NET backend server SDK for Azure Mobile Apps](app-service-mobile-dotnet-backend-how-to-use-server-sdk.md) for more information.</span></span>

## <span data-ttu-id="804af-108"><a name="configure-hub"></a>Konfigurace centra oznámení</span><span class="sxs-lookup"><span data-stu-id="804af-108"><a name="configure-hub"></a>Configure a Notification Hub</span></span>
[!INCLUDE [app-service-mobile-configure-notification-hub](../../includes/app-service-mobile-configure-notification-hub.md)]

## <a name="register-your-app-for-push-notifications"></a><span data-ttu-id="804af-109">Registrace aplikace pro nabízená oznámení</span><span class="sxs-lookup"><span data-stu-id="804af-109">Register your app for push notifications</span></span>
<span data-ttu-id="804af-110">Budete muset odeslat v aplikaci Windows Store a pak nakonfigurujete svůj projekt server k integraci s Windows oznámení služby (WNS) k odesílání nabízených.</span><span class="sxs-lookup"><span data-stu-id="804af-110">You need to submit your app to the Windows Store, then configure your server project to integrate with Windows Notification Services (WNS) to send push.</span></span>

1. <span data-ttu-id="804af-111">V Průzkumníku řešení Visual Studio, klikněte pravým tlačítkem na projekt aplikace UPW, klikněte na tlačítko **úložiště** > **přidružit aplikaci ve Store...** .</span><span class="sxs-lookup"><span data-stu-id="804af-111">In Visual Studio Solution Explorer, right-click the UWP app project, click **Store** > **Associate App with the Store...**.</span></span>

    ![Přidružit aplikace Windows Store](./media/app-service-mobile-windows-store-dotnet-get-started-push/notification-hub-associate-uwp-app.png)
2. <span data-ttu-id="804af-113">V průvodci klikněte na tlačítko **Další**, přihlaste se pomocí účtu Microsoft, zadejte název aplikace v rámci **rezervovat nový název aplikace**, pak klikněte na tlačítko **rezervy**.</span><span class="sxs-lookup"><span data-stu-id="804af-113">In the wizard, click **Next**, sign in with your Microsoft account, type a name for your app in **Reserve a new app name**, then click **Reserve**.</span></span>
3. <span data-ttu-id="804af-114">Po registraci aplikace je úspěšně vytvořen, vyberte nový název aplikace, klikněte na **Další**a potom klikněte na **přidružit**.</span><span class="sxs-lookup"><span data-stu-id="804af-114">After the app registration is successfully created, select the new app name, click **Next**, and then click **Associate**.</span></span> <span data-ttu-id="804af-115">Tento postup přidá požadované informace o registraci Windows Store do manifestu aplikace.</span><span class="sxs-lookup"><span data-stu-id="804af-115">This adds the required Windows Store registration information to the application manifest.</span></span>  
4. <span data-ttu-id="804af-116">Přejděte na [Windows Dev Center](https://dev.windows.com/en-us/overview), přihlaste se pomocí účtu Microsoft, klikněte na novou registraci aplikace v **Moje aplikace**, pak rozbalte **služby**  >   **Nabízená oznámení**.</span><span class="sxs-lookup"><span data-stu-id="804af-116">Navigate to the [Windows Dev Center](https://dev.windows.com/en-us/overview), sign-in with your Microsoft account, click the new app registration in **My apps**, then expand **Services** > **Push notifications**.</span></span>
5. <span data-ttu-id="804af-117">V **nabízená oznámení** klikněte na tlačítko **Web služeb Live Services** pod **mobilní služby Microsoft Azure**.</span><span class="sxs-lookup"><span data-stu-id="804af-117">In the **Push notifications** page, click **Live Services site** under **Microsoft Azure Mobile Services**.</span></span>
6. <span data-ttu-id="804af-118">Na stránce registrace si poznamenejte hodnoty v části **tajné klíče aplikace** a **SID balíčku**, které budou vedle použít ke konfiguraci váš back-end mobilní aplikace.</span><span class="sxs-lookup"><span data-stu-id="804af-118">In the registration page, make a note of the value under **Application secrets** and the **Package SID**, which you will next use to configure your mobile app backend.</span></span>

    ![Přidružit aplikace Windows Store](./media/app-service-mobile-windows-store-dotnet-get-started-push/app-service-mobile-uwp-app-push-auth.png)

   > [!IMPORTANT]
   > <span data-ttu-id="804af-120">Tajný klíč klienta a SID balíčku jsou důležitá pověření zabezpečení.</span><span class="sxs-lookup"><span data-stu-id="804af-120">The client secret and package SID are important security credentials.</span></span> <span data-ttu-id="804af-121">Tyto hodnoty s nikým nesdílejte ani je nedistribuujte s vaší aplikací.</span><span class="sxs-lookup"><span data-stu-id="804af-121">Do not share these values with anyone or distribute them with your app.</span></span> <span data-ttu-id="804af-122">**Id aplikace** ke konfiguraci ověřování Account Microsoft se používá s tajným klíčem.</span><span class="sxs-lookup"><span data-stu-id="804af-122">The **Application Id** is used with the secret to configure Microsoft Account authentication.</span></span>
   >
   >

## <a name="configure-the-backend-to-send-push-notifications"></a><span data-ttu-id="804af-123">Konfigurace back-end k odesílání nabízených oznámení</span><span class="sxs-lookup"><span data-stu-id="804af-123">Configure the backend to send push notifications</span></span>
[!INCLUDE [app-service-mobile-configure-wns](../../includes/app-service-mobile-configure-wns.md)]

## <span data-ttu-id="804af-124"><a id="update-service"></a>Aktualizace serveru k odesílání nabízených oznámení</span><span class="sxs-lookup"><span data-stu-id="804af-124"><a id="update-service"></a>Update the server to send push notifications</span></span>
<span data-ttu-id="804af-125">Použijte následující postup, který odpovídá typu vašeho projektu back-end&mdash;buď [.NET back-end](#dotnet) nebo [back-end Node.js](#nodejs).</span><span class="sxs-lookup"><span data-stu-id="804af-125">Use the procedure below that matches your backend project type&mdash;either [.NET backend](#dotnet) or [Node.js backend](#nodejs).</span></span>

### <span data-ttu-id="804af-126"><a name="dotnet"></a>Rozhraní .NET back-end projektu</span><span class="sxs-lookup"><span data-stu-id="804af-126"><a name="dotnet"></a>.NET backend project</span></span>
1. <span data-ttu-id="804af-127">V sadě Visual Studio, klikněte pravým tlačítkem na serverový projekt a klikněte na tlačítko **spravovat balíčky NuGet**, vyhledejte Microsoft.Azure.NotificationHubs a pak klikněte na tlačítko **nainstalovat**.</span><span class="sxs-lookup"><span data-stu-id="804af-127">In Visual Studio, right-click the server project and click **Manage NuGet Packages**, search for Microsoft.Azure.NotificationHubs, then click **Install**.</span></span> <span data-ttu-id="804af-128">Tím se nainstaluje knihovnu klienta služby Notification Hubs.</span><span class="sxs-lookup"><span data-stu-id="804af-128">This installs the Notification Hubs client library.</span></span>
2. <span data-ttu-id="804af-129">Rozbalte položku **řadiče**, otevřete TodoItemController.cs a přidejte následující příkazy:</span><span class="sxs-lookup"><span data-stu-id="804af-129">Expand **Controllers**, open TodoItemController.cs, and add the following using statements:</span></span>

        using System.Collections.Generic;
        using Microsoft.Azure.NotificationHubs;
        using Microsoft.Azure.Mobile.Server.Config;
3. <span data-ttu-id="804af-130">V **PostTodoItem** metoda, přidejte následující kód po volání **InsertAsync**:</span><span class="sxs-lookup"><span data-stu-id="804af-130">In the **PostTodoItem** method, add the following code after the call to **InsertAsync**:</span></span>

        // Get the settings for the server project.
        HttpConfiguration config = this.Configuration;
        MobileAppSettingsDictionary settings =
            this.Configuration.GetMobileAppSettingsProvider().GetMobileAppSettings();

        // Get the Notification Hubs credentials for the Mobile App.
        string notificationHubName = settings.NotificationHubName;
        string notificationHubConnection = settings
            .Connections[MobileAppSettingsKeys.NotificationHubConnectionString].ConnectionString;

        // Create the notification hub client.
        NotificationHubClient hub = NotificationHubClient
            .CreateClientFromConnectionString(notificationHubConnection, notificationHubName);

        // Define a WNS payload
        var windowsToastPayload = @"<toast><visual><binding template=""ToastText01""><text id=""1"">"
                                + item.Text + @"</text></binding></visual></toast>";
        try
        {
            // Send the push notification.
            var result = await hub.SendWindowsNativeNotificationAsync(windowsToastPayload);

            // Write the success result to the logs.
            config.Services.GetTraceWriter().Info(result.State.ToString());
        }
        catch (System.Exception ex)
        {
            // Write the failure result to the logs.
            config.Services.GetTraceWriter()
                .Error(ex.Message, null, "Push.SendAsync Error");
        }

    <span data-ttu-id="804af-131">Tento kód informuje centra oznámení k odesílání nabízených oznámení po vložení nové položky.</span><span class="sxs-lookup"><span data-stu-id="804af-131">This code tells the notification hub to send a push notification after a new item is insertion.</span></span>
4. <span data-ttu-id="804af-132">Znovu publikujte serverový projekt.</span><span class="sxs-lookup"><span data-stu-id="804af-132">Republish the server project.</span></span>

### <span data-ttu-id="804af-133"><a name="nodejs"></a>Projektu back-end Node.js</span><span class="sxs-lookup"><span data-stu-id="804af-133"><a name="nodejs"></a>Node.js backend project</span></span>
1. <span data-ttu-id="804af-134">Pokud jste zatím žádný nevytvořili, [stažení projektu pro rychlý Start](app-service-mobile-node-backend-how-to-use-server-sdk.md#download-quickstart) nebo použijte jiný [online editor na webu Azure portal](app-service-mobile-node-backend-how-to-use-server-sdk.md#online-editor).</span><span class="sxs-lookup"><span data-stu-id="804af-134">If you haven't already done so, [download the quickstart project](app-service-mobile-node-backend-how-to-use-server-sdk.md#download-quickstart) or else use the [online editor in the Azure portal](app-service-mobile-node-backend-how-to-use-server-sdk.md#online-editor).</span></span>
2. <span data-ttu-id="804af-135">Nahraďte stávající kód v souboru todoitem.js s následujícími službami:</span><span class="sxs-lookup"><span data-stu-id="804af-135">Replace the existing code in the todoitem.js file with the following:</span></span>

        var azureMobileApps = require('azure-mobile-apps'),
        promises = require('azure-mobile-apps/src/utilities/promises'),
        logger = require('azure-mobile-apps/src/logger');

        var table = azureMobileApps.table();

        table.insert(function (context) {
        // For more information about the Notification Hubs JavaScript SDK,
        // see http://aka.ms/nodejshubs
        logger.info('Running TodoItem.insert');

        // Define the WNS payload that contains the new item Text.
        var payload = "<toast><visual><binding template=\ToastText01\><text id=\"1\">"
                                    + context.item.text + "</text></binding></visual></toast>";

        // Execute the insert.  The insert returns the results as a Promise,
        // Do the push as a post-execute action within the promise flow.
        return context.execute()
            .then(function (results) {
                // Only do the push if configured
                if (context.push) {
                    // Send a WNS native toast notification.
                    context.push.wns.sendToast(null, payload, function (error) {
                        if (error) {
                            logger.error('Error while sending push notification: ', error);
                        } else {
                            logger.info('Push notification sent successfully!');
                        }
                    });
                }
                // Don't forget to return the results from the context.execute()
                return results;
            })
            .catch(function (error) {
                logger.error('Error while running context.execute: ', error);
            });
        });

        module.exports = table;

    <span data-ttu-id="804af-136">Tím se odešle oznámení s informační zprávou WNS, který obsahuje item.text při vložení nová položka todo.</span><span class="sxs-lookup"><span data-stu-id="804af-136">This sends a WNS toast notification that contains the item.text when a new todo item is inserted.</span></span>
3. <span data-ttu-id="804af-137">Při úpravách souboru v místním počítači, znovu publikujte serverový projekt.</span><span class="sxs-lookup"><span data-stu-id="804af-137">When editing the file on your local computer, republish the server project.</span></span>

## <span data-ttu-id="804af-138"><a id="update-app"></a>Přidání nabízených oznámení do vaší aplikace</span><span class="sxs-lookup"><span data-stu-id="804af-138"><a id="update-app"></a>Add push notifications to your app</span></span>
<span data-ttu-id="804af-139">V dalším kroku musí aplikaci zaregistrovat pro nabízená oznámení na spuštění.</span><span class="sxs-lookup"><span data-stu-id="804af-139">Next, your app must register for push notifications on start-up.</span></span> <span data-ttu-id="804af-140">Pokud už jste povolili ověřování, ujistěte se, že uživatel přihlásí než se pokusíte zaregistrovat pro nabízená oznámení.</span><span class="sxs-lookup"><span data-stu-id="804af-140">When you have already enabled authentication, make sure that the user signs-in before trying to register for push notifications.</span></span>

1. <span data-ttu-id="804af-141">Otevřete **App.xaml.cs** souboru projektu a přidejte následující `using` příkazy:</span><span class="sxs-lookup"><span data-stu-id="804af-141">Open the **App.xaml.cs** project file and add the following `using` statements:</span></span>

        using System.Threading.Tasks;
        using Windows.Networking.PushNotifications;
2. <span data-ttu-id="804af-142">Do stejného souboru přidejte následující **InitNotificationsAsync** metoda definici tak, aby **aplikace** třídy:</span><span class="sxs-lookup"><span data-stu-id="804af-142">In the same file, add the following **InitNotificationsAsync** method definition to the **App** class:</span></span>

        private async Task InitNotificationsAsync()
        {
            // Get a channel URI from WNS.
            var channel = await PushNotificationChannelManager
                .CreatePushNotificationChannelForApplicationAsync();

            // Register the channel URI with Notification Hubs.
            await App.MobileService.GetPush().RegisterAsync(channel.Uri);
        }

    <span data-ttu-id="804af-143">Tento kód načte ChannelURI pro aplikaci z WNS a pak zaregistruje tento ChannelURI pomocí aplikace služby mobilní aplikace.</span><span class="sxs-lookup"><span data-stu-id="804af-143">This code retrieves the ChannelURI for the app from WNS, and then registers that ChannelURI with your App Service Mobile App.</span></span>
3. <span data-ttu-id="804af-144">V horní části **OnLaunched** obslužné rutiny událostí v **App.xaml.cs**, přidejte **asynchronní** modifikátor k definici metoda a přidejte následující volání do nového  **InitNotificationsAsync** metoda jako v následujícím příkladu:</span><span class="sxs-lookup"><span data-stu-id="804af-144">At the top of the **OnLaunched** event handler in **App.xaml.cs**, add the **async** modifier to the method definition and add the following call to the new **InitNotificationsAsync** method, as in the following example:</span></span>

        protected async override void OnLaunched(LaunchActivatedEventArgs e)
        {
            await InitNotificationsAsync();

            // ...
        }

    <span data-ttu-id="804af-145">To zaručuje, že krátkodobou ChannelURI je zaregistrován pokaždé, když je aplikace spuštěna.</span><span class="sxs-lookup"><span data-stu-id="804af-145">This guarantees that the short-lived ChannelURI is registered each time the application is launched.</span></span>
4. <span data-ttu-id="804af-146">Znovu sestavte projekt aplikace UPW.</span><span class="sxs-lookup"><span data-stu-id="804af-146">Rebuild your UWP app project.</span></span> <span data-ttu-id="804af-147">Vaše aplikace je teď připravena přijímat oznámení informačního nápisu.</span><span class="sxs-lookup"><span data-stu-id="804af-147">Your app is now ready to receive toast notifications.</span></span>

## <span data-ttu-id="804af-148"><a id="test"></a>Nabízená oznámení v aplikaci</span><span class="sxs-lookup"><span data-stu-id="804af-148"><a id="test"></a>Test push notifications in your app</span></span>
[!INCLUDE [app-service-mobile-windows-universal-test-push](../../includes/app-service-mobile-windows-universal-test-push.md)]

## <span data-ttu-id="804af-149"><a id="more"></a>Další kroky</span><span class="sxs-lookup"><span data-stu-id="804af-149"><a id="more"></a>Next steps</span></span>
<span data-ttu-id="804af-150">Další informace o nabízených oznámení:</span><span class="sxs-lookup"><span data-stu-id="804af-150">Learn more about push notifications:</span></span>

* [<span data-ttu-id="804af-151">Jak používat spravovaného klienta pro Azure Mobile Apps</span><span class="sxs-lookup"><span data-stu-id="804af-151">How to use the managed client for Azure Mobile Apps</span></span>](app-service-mobile-dotnet-how-to-use-client-library.md#pushnotifications)  
  <span data-ttu-id="804af-152">Šablony poskytují flexibilitu k odesílání nabízených oznámení napříč platformami a lokalizované nabízených oznámení.</span><span class="sxs-lookup"><span data-stu-id="804af-152">Templates give you flexibility to send cross-platform pushes and localized pushes.</span></span> <span data-ttu-id="804af-153">Zjistěte, jak zaregistrovat šablony.</span><span class="sxs-lookup"><span data-stu-id="804af-153">Learn how to register templates.</span></span>
* [<span data-ttu-id="804af-154">Diagnostikovat problémy nabízená oznámení</span><span class="sxs-lookup"><span data-stu-id="804af-154">Diagnose push notification issues</span></span>](../notification-hubs/notification-hubs-push-notification-fixer.md)  
  <span data-ttu-id="804af-155">Existují různé důvody, proč oznámení může získat vyřadit ani nekončí na zařízení.</span><span class="sxs-lookup"><span data-stu-id="804af-155">There are various reasons why notifications may get dropped or do not end up on devices.</span></span> <span data-ttu-id="804af-156">Toto téma ukazuje, jak analyzovat a zjistěte příčinu selhání nabízená oznámení.</span><span class="sxs-lookup"><span data-stu-id="804af-156">This topic shows you how to analyze and figure out the root cause of push notification failures.</span></span>

<span data-ttu-id="804af-157">Vezměte v úvahu pokračovat na jednu z následujících kurzů:</span><span class="sxs-lookup"><span data-stu-id="804af-157">Consider continuing on to one of the following tutorials:</span></span>

* [<span data-ttu-id="804af-158">Přidání ověřování do aplikace</span><span class="sxs-lookup"><span data-stu-id="804af-158">Add authentication to your app</span></span>](app-service-mobile-windows-store-dotnet-get-started-users.md)  
  <span data-ttu-id="804af-159">Zjistěte, jak ověřovat uživatele vaší aplikace pomocí zprostředkovatele identity.</span><span class="sxs-lookup"><span data-stu-id="804af-159">Learn how to authenticate users of your app with an identity provider.</span></span>
* [<span data-ttu-id="804af-160">Povolení offline synchronizace u aplikace</span><span class="sxs-lookup"><span data-stu-id="804af-160">Enable offline sync for your app</span></span>](app-service-mobile-windows-store-dotnet-get-started-offline-data.md)  
  <span data-ttu-id="804af-161">Naučte se, jak pomocí back-endu mobilní aplikace přidat do aplikace podporu offline režimu.</span><span class="sxs-lookup"><span data-stu-id="804af-161">Learn how to add offline support your app using an Mobile App backend.</span></span> <span data-ttu-id="804af-162">Offline synchronizace umožňuje koncovým uživatelům pracovat s mobilní aplikací &mdash; zobrazovat, přidávat a upravovat data &mdash; i v případě, že nemají připojení k síti.</span><span class="sxs-lookup"><span data-stu-id="804af-162">Offline sync allows end-users to interact with a mobile app&mdash;viewing, adding, or modifying data&mdash;even when there is no network connection.</span></span>

<!-- Anchors. -->

<!-- URLs. -->
[Azure Portal]: https://portal.azure.com/

<!-- Images. -->
