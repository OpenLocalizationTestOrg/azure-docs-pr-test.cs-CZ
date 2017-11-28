---
title: "aaaAdd nabízená oznámení tooyour univerzální platformu Windows (UWP) aplikace | Microsoft Docs"
description: "Zjistěte, jak toouse Azure App Service Mobile Apps a Azure Notification Hubs toosend nabízená oznámení tooyour univerzální platformu Windows (UWP) aplikace."
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
ms.openlocfilehash: 378ce59cab974830c0a3801108b24b30a21ae5cc
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="add-push-notifications-tooyour-windows-app"></a><span data-ttu-id="b0e0d-103">Přidat nabízená oznámení tooyour Windows aplikaci</span><span class="sxs-lookup"><span data-stu-id="b0e0d-103">Add push notifications tooyour Windows app</span></span>
[!INCLUDE [app-service-mobile-selector-get-started-push](../../includes/app-service-mobile-selector-get-started-push.md)]

## <a name="overview"></a><span data-ttu-id="b0e0d-104">Přehled</span><span class="sxs-lookup"><span data-stu-id="b0e0d-104">Overview</span></span>
<span data-ttu-id="b0e0d-105">V tomto kurzu přidáte nabízená oznámení toohello [Windows úvodní](app-service-mobile-windows-store-dotnet-get-started.md) projektu tak, aby nabízených oznámení je odesláno toohello zařízení pokaždé, když vložení záznamu.</span><span class="sxs-lookup"><span data-stu-id="b0e0d-105">In this tutorial, you add push notifications toohello [Windows quick start](app-service-mobile-windows-store-dotnet-get-started.md) project so that a push notification is sent toohello device every time a record is inserted.</span></span>

<span data-ttu-id="b0e0d-106">Pokud nepoužijete hello stáhli úvodní serverový projekt, bude nutné hello nabízených oznámení v balíčku rozšíření.</span><span class="sxs-lookup"><span data-stu-id="b0e0d-106">If you do not use hello downloaded quick start server project, you will need hello push notification extension package.</span></span> <span data-ttu-id="b0e0d-107">V tématu [pracovat s hello .NET back-end serveru SDK pro Azure Mobile Apps](app-service-mobile-dotnet-backend-how-to-use-server-sdk.md) Další informace.</span><span class="sxs-lookup"><span data-stu-id="b0e0d-107">See [Work with hello .NET backend server SDK for Azure Mobile Apps](app-service-mobile-dotnet-backend-how-to-use-server-sdk.md) for more information.</span></span>

## <span data-ttu-id="b0e0d-108"><a name="configure-hub"></a>Konfigurace centra oznámení</span><span class="sxs-lookup"><span data-stu-id="b0e0d-108"><a name="configure-hub"></a>Configure a Notification Hub</span></span>
[!INCLUDE [app-service-mobile-configure-notification-hub](../../includes/app-service-mobile-configure-notification-hub.md)]

## <a name="register-your-app-for-push-notifications"></a><span data-ttu-id="b0e0d-109">Registrace aplikace pro nabízená oznámení</span><span class="sxs-lookup"><span data-stu-id="b0e0d-109">Register your app for push notifications</span></span>
<span data-ttu-id="b0e0d-110">Můžete potřebovat toosubmit toohello vaší aplikace Windows Store, a potom nakonfigurovat toosend nabízené služby oznámení Windows (WNS) vašeho projektu toointegrate serveru.</span><span class="sxs-lookup"><span data-stu-id="b0e0d-110">You need toosubmit your app toohello Windows Store, then configure your server project toointegrate with Windows Notification Services (WNS) toosend push.</span></span>

1. <span data-ttu-id="b0e0d-111">V Průzkumníku řešení Visual Studio, klikněte pravým tlačítkem na projekt aplikace UPW hello, klikněte na tlačítko **úložiště** > **propojit aplikaci se hello úložiště...** .</span><span class="sxs-lookup"><span data-stu-id="b0e0d-111">In Visual Studio Solution Explorer, right-click hello UWP app project, click **Store** > **Associate App with hello Store...**.</span></span>

    ![Přidružit aplikace Windows Store](./media/app-service-mobile-windows-store-dotnet-get-started-push/notification-hub-associate-uwp-app.png)
2. <span data-ttu-id="b0e0d-113">V Průvodci hello, klikněte na tlačítko **Další**, přihlaste se pomocí účtu Microsoft, zadejte název aplikace v rámci **rezervovat nový název aplikace**, pak klikněte na tlačítko **rezervy**.</span><span class="sxs-lookup"><span data-stu-id="b0e0d-113">In hello wizard, click **Next**, sign in with your Microsoft account, type a name for your app in **Reserve a new app name**, then click **Reserve**.</span></span>
3. <span data-ttu-id="b0e0d-114">Po registraci aplikace hello je úspěšně vytvořil, vyberte hello nový název aplikace, klikněte na tlačítko **Další**a potom klikněte na **přidružit**.</span><span class="sxs-lookup"><span data-stu-id="b0e0d-114">After hello app registration is successfully created, select hello new app name, click **Next**, and then click **Associate**.</span></span> <span data-ttu-id="b0e0d-115">Tento postup přidá požadované hello Windows Store registrační informace toohello manifest aplikace.</span><span class="sxs-lookup"><span data-stu-id="b0e0d-115">This adds hello required Windows Store registration information toohello application manifest.</span></span>  
4. <span data-ttu-id="b0e0d-116">Přejděte toohello [Windows Dev Center](https://dev.windows.com/en-us/overview), přihlaste se pomocí účtu Microsoft, klikněte na nové registrace aplikace hello v **Moje aplikace**, pak rozbalte **služby**  >  **Nabízená oznámení**.</span><span class="sxs-lookup"><span data-stu-id="b0e0d-116">Navigate toohello [Windows Dev Center](https://dev.windows.com/en-us/overview), sign-in with your Microsoft account, click hello new app registration in **My apps**, then expand **Services** > **Push notifications**.</span></span>
5. <span data-ttu-id="b0e0d-117">V hello **nabízená oznámení** klikněte na tlačítko **Web služeb Live Services** pod **mobilní služby Microsoft Azure**.</span><span class="sxs-lookup"><span data-stu-id="b0e0d-117">In hello **Push notifications** page, click **Live Services site** under **Microsoft Azure Mobile Services**.</span></span>
6. <span data-ttu-id="b0e0d-118">Na stránce registrace hello, poznamenejte si hodnotu hello **tajné klíče aplikace** a hello **SID balíčku**, které budou vedle použijete tooconfigure váš back-end mobilní aplikace.</span><span class="sxs-lookup"><span data-stu-id="b0e0d-118">In hello registration page, make a note of hello value under **Application secrets** and hello **Package SID**, which you will next use tooconfigure your mobile app backend.</span></span>

    ![Přidružit aplikace Windows Store](./media/app-service-mobile-windows-store-dotnet-get-started-push/app-service-mobile-uwp-app-push-auth.png)

   > [!IMPORTANT]
   > <span data-ttu-id="b0e0d-120">Hello tajný klíč klienta a SID balíčku jsou důležitá pověření zabezpečení.</span><span class="sxs-lookup"><span data-stu-id="b0e0d-120">hello client secret and package SID are important security credentials.</span></span> <span data-ttu-id="b0e0d-121">Tyto hodnoty s nikým nesdílejte ani je nedistribuujte s vaší aplikací.</span><span class="sxs-lookup"><span data-stu-id="b0e0d-121">Do not share these values with anyone or distribute them with your app.</span></span> <span data-ttu-id="b0e0d-122">Hello **Id aplikace** se používá s ověřováním tajný tooconfigure Account Microsoft hello.</span><span class="sxs-lookup"><span data-stu-id="b0e0d-122">hello **Application Id** is used with hello secret tooconfigure Microsoft Account authentication.</span></span>
   >
   >

## <a name="configure-hello-backend-toosend-push-notifications"></a><span data-ttu-id="b0e0d-123">Konfigurace hello back-end toosend nabízená oznámení</span><span class="sxs-lookup"><span data-stu-id="b0e0d-123">Configure hello backend toosend push notifications</span></span>
[!INCLUDE [app-service-mobile-configure-wns](../../includes/app-service-mobile-configure-wns.md)]

## <span data-ttu-id="b0e0d-124"><a id="update-service"></a>Aktualizovat hello serveru toosend nabízená oznámení</span><span class="sxs-lookup"><span data-stu-id="b0e0d-124"><a id="update-service"></a>Update hello server toosend push notifications</span></span>
<span data-ttu-id="b0e0d-125">Pomocí postupu hello níže, který odpovídá typu vašeho projektu back-end&mdash;buď [.NET back-end](#dotnet) nebo [back-end Node.js](#nodejs).</span><span class="sxs-lookup"><span data-stu-id="b0e0d-125">Use hello procedure below that matches your backend project type&mdash;either [.NET backend](#dotnet) or [Node.js backend](#nodejs).</span></span>

### <span data-ttu-id="b0e0d-126"><a name="dotnet"></a>Rozhraní .NET back-end projektu</span><span class="sxs-lookup"><span data-stu-id="b0e0d-126"><a name="dotnet"></a>.NET backend project</span></span>
1. <span data-ttu-id="b0e0d-127">V sadě Visual Studio, klikněte pravým tlačítkem na hello serverový projekt a klikněte na tlačítko **spravovat balíčky NuGet**, vyhledejte Microsoft.Azure.NotificationHubs a pak klikněte na tlačítko **nainstalovat**.</span><span class="sxs-lookup"><span data-stu-id="b0e0d-127">In Visual Studio, right-click hello server project and click **Manage NuGet Packages**, search for Microsoft.Azure.NotificationHubs, then click **Install**.</span></span> <span data-ttu-id="b0e0d-128">Tím se nainstaluje Klientská knihovna pro hello centra oznámení.</span><span class="sxs-lookup"><span data-stu-id="b0e0d-128">This installs hello Notification Hubs client library.</span></span>
2. <span data-ttu-id="b0e0d-129">Rozbalte položku **řadiče**, otevřete TodoItemController.cs a přidejte hello následující příkazy:</span><span class="sxs-lookup"><span data-stu-id="b0e0d-129">Expand **Controllers**, open TodoItemController.cs, and add hello following using statements:</span></span>

        using System.Collections.Generic;
        using Microsoft.Azure.NotificationHubs;
        using Microsoft.Azure.Mobile.Server.Config;
3. <span data-ttu-id="b0e0d-130">V hello **PostTodoItem** metoda, přidejte následující kód po volání hello příliš hello**InsertAsync**:</span><span class="sxs-lookup"><span data-stu-id="b0e0d-130">In hello **PostTodoItem** method, add hello following code after hello call too**InsertAsync**:</span></span>

        // Get hello settings for hello server project.
        HttpConfiguration config = this.Configuration;
        MobileAppSettingsDictionary settings =
            this.Configuration.GetMobileAppSettingsProvider().GetMobileAppSettings();

        // Get hello Notification Hubs credentials for hello Mobile App.
        string notificationHubName = settings.NotificationHubName;
        string notificationHubConnection = settings
            .Connections[MobileAppSettingsKeys.NotificationHubConnectionString].ConnectionString;

        // Create hello notification hub client.
        NotificationHubClient hub = NotificationHubClient
            .CreateClientFromConnectionString(notificationHubConnection, notificationHubName);

        // Define a WNS payload
        var windowsToastPayload = @"<toast><visual><binding template=""ToastText01""><text id=""1"">"
                                + item.Text + @"</text></binding></visual></toast>";
        try
        {
            // Send hello push notification.
            var result = await hub.SendWindowsNativeNotificationAsync(windowsToastPayload);

            // Write hello success result toohello logs.
            config.Services.GetTraceWriter().Info(result.State.ToString());
        }
        catch (System.Exception ex)
        {
            // Write hello failure result toohello logs.
            config.Services.GetTraceWriter()
                .Error(ex.Message, null, "Push.SendAsync Error");
        }

    <span data-ttu-id="b0e0d-131">Tento kód informuje toosend centra oznámení hello po vložení novou položku nabízeného oznámení.</span><span class="sxs-lookup"><span data-stu-id="b0e0d-131">This code tells hello notification hub toosend a push notification after a new item is insertion.</span></span>
4. <span data-ttu-id="b0e0d-132">Znovu publikujte hello serverový projekt.</span><span class="sxs-lookup"><span data-stu-id="b0e0d-132">Republish hello server project.</span></span>

### <span data-ttu-id="b0e0d-133"><a name="nodejs"></a>Projektu back-end Node.js</span><span class="sxs-lookup"><span data-stu-id="b0e0d-133"><a name="nodejs"></a>Node.js backend project</span></span>
1. <span data-ttu-id="b0e0d-134">Pokud jste zatím žádný nevytvořili, [stažení projektu typu rychlý start hello](app-service-mobile-node-backend-how-to-use-server-sdk.md#download-quickstart) nebo jinak použijte hello [online editor v hello portál Azure](app-service-mobile-node-backend-how-to-use-server-sdk.md#online-editor).</span><span class="sxs-lookup"><span data-stu-id="b0e0d-134">If you haven't already done so, [download hello quickstart project](app-service-mobile-node-backend-how-to-use-server-sdk.md#download-quickstart) or else use hello [online editor in hello Azure portal](app-service-mobile-node-backend-how-to-use-server-sdk.md#online-editor).</span></span>
2. <span data-ttu-id="b0e0d-135">Nahraďte hello existující kód v souboru todoitem.js hello hello následující:</span><span class="sxs-lookup"><span data-stu-id="b0e0d-135">Replace hello existing code in hello todoitem.js file with hello following:</span></span>

        var azureMobileApps = require('azure-mobile-apps'),
        promises = require('azure-mobile-apps/src/utilities/promises'),
        logger = require('azure-mobile-apps/src/logger');

        var table = azureMobileApps.table();

        table.insert(function (context) {
        // For more information about hello Notification Hubs JavaScript SDK,
        // see http://aka.ms/nodejshubs
        logger.info('Running TodoItem.insert');

        // Define hello WNS payload that contains hello new item Text.
        var payload = "<toast><visual><binding template=\ToastText01\><text id=\"1\">"
                                    + context.item.text + "</text></binding></visual></toast>";

        // Execute hello insert.  hello insert returns hello results as a Promise,
        // Do hello push as a post-execute action within hello promise flow.
        return context.execute()
            .then(function (results) {
                // Only do hello push if configured
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
                // Don't forget tooreturn hello results from hello context.execute()
                return results;
            })
            .catch(function (error) {
                logger.error('Error while running context.execute: ', error);
            });
        });

        module.exports = table;

    <span data-ttu-id="b0e0d-136">Tím se odešle oznámení s informační zprávou WNS, který obsahuje hello item.text při vložení nová položka todo.</span><span class="sxs-lookup"><span data-stu-id="b0e0d-136">This sends a WNS toast notification that contains hello item.text when a new todo item is inserted.</span></span>
3. <span data-ttu-id="b0e0d-137">Při úpravách souboru hello v místním počítači, znovu publikujte hello serverový projekt.</span><span class="sxs-lookup"><span data-stu-id="b0e0d-137">When editing hello file on your local computer, republish hello server project.</span></span>

## <span data-ttu-id="b0e0d-138"><a id="update-app"></a>Přidat nabízená oznámení tooyour aplikaci</span><span class="sxs-lookup"><span data-stu-id="b0e0d-138"><a id="update-app"></a>Add push notifications tooyour app</span></span>
<span data-ttu-id="b0e0d-139">V dalším kroku musí aplikaci zaregistrovat pro nabízená oznámení na spuštění.</span><span class="sxs-lookup"><span data-stu-id="b0e0d-139">Next, your app must register for push notifications on start-up.</span></span> <span data-ttu-id="b0e0d-140">Pokud už jste povolili ověřování, zajistěte, aby tento uživatel hello přihlásí a teprve potom zkusili tooregister pro nabízená oznámení.</span><span class="sxs-lookup"><span data-stu-id="b0e0d-140">When you have already enabled authentication, make sure that hello user signs-in before trying tooregister for push notifications.</span></span>

1. <span data-ttu-id="b0e0d-141">Otevřete hello **App.xaml.cs** souboru projektu a přidejte následující hello `using` příkazy:</span><span class="sxs-lookup"><span data-stu-id="b0e0d-141">Open hello **App.xaml.cs** project file and add hello following `using` statements:</span></span>

        using System.Threading.Tasks;
        using Windows.Networking.PushNotifications;
2. <span data-ttu-id="b0e0d-142">V hello stejný soubor, přidejte následující hello **InitNotificationsAsync** metoda definice toohello **aplikace** třídy:</span><span class="sxs-lookup"><span data-stu-id="b0e0d-142">In hello same file, add hello following **InitNotificationsAsync** method definition toohello **App** class:</span></span>

        private async Task InitNotificationsAsync()
        {
            // Get a channel URI from WNS.
            var channel = await PushNotificationChannelManager
                .CreatePushNotificationChannelForApplicationAsync();

            // Register hello channel URI with Notification Hubs.
            await App.MobileService.GetPush().RegisterAsync(channel.Uri);
        }

    <span data-ttu-id="b0e0d-143">Tento kód načte hello ChannelURI pro hello aplikaci z WNS a pak zaregistruje tento ChannelURI pomocí aplikace služby mobilní aplikace.</span><span class="sxs-lookup"><span data-stu-id="b0e0d-143">This code retrieves hello ChannelURI for hello app from WNS, and then registers that ChannelURI with your App Service Mobile App.</span></span>
3. <span data-ttu-id="b0e0d-144">Hello horní části hello **OnLaunched** obslužné rutiny událostí v **App.xaml.cs**, přidejte hello **asynchronní** modifikátor toohello metoda definice a přidejte následující hello volání nové toohello **InitNotificationsAsync** metoda jako hello následující ukázka:</span><span class="sxs-lookup"><span data-stu-id="b0e0d-144">At hello top of hello **OnLaunched** event handler in **App.xaml.cs**, add hello **async** modifier toohello method definition and add hello following call toohello new **InitNotificationsAsync** method, as in hello following example:</span></span>

        protected async override void OnLaunched(LaunchActivatedEventArgs e)
        {
            await InitNotificationsAsync();

            // ...
        }

    <span data-ttu-id="b0e0d-145">To zaručuje, že hello krátkodobou ChannelURI registrovaná pokaždé, když hello aplikace spuštěna.</span><span class="sxs-lookup"><span data-stu-id="b0e0d-145">This guarantees that hello short-lived ChannelURI is registered each time hello application is launched.</span></span>
4. <span data-ttu-id="b0e0d-146">Znovu sestavte projekt aplikace UPW.</span><span class="sxs-lookup"><span data-stu-id="b0e0d-146">Rebuild your UWP app project.</span></span> <span data-ttu-id="b0e0d-147">Aplikace je nyní připraven tooreceive informační zprávy.</span><span class="sxs-lookup"><span data-stu-id="b0e0d-147">Your app is now ready tooreceive toast notifications.</span></span>

## <span data-ttu-id="b0e0d-148"><a id="test"></a>Nabízená oznámení v aplikaci</span><span class="sxs-lookup"><span data-stu-id="b0e0d-148"><a id="test"></a>Test push notifications in your app</span></span>
[!INCLUDE [app-service-mobile-windows-universal-test-push](../../includes/app-service-mobile-windows-universal-test-push.md)]

## <span data-ttu-id="b0e0d-149"><a id="more"></a>Další kroky</span><span class="sxs-lookup"><span data-stu-id="b0e0d-149"><a id="more"></a>Next steps</span></span>
<span data-ttu-id="b0e0d-150">Další informace o nabízených oznámení:</span><span class="sxs-lookup"><span data-stu-id="b0e0d-150">Learn more about push notifications:</span></span>

* [<span data-ttu-id="b0e0d-151">Jak toouse hello spravovat klienta pro Azure Mobile Apps</span><span class="sxs-lookup"><span data-stu-id="b0e0d-151">How toouse hello managed client for Azure Mobile Apps</span></span>](app-service-mobile-dotnet-how-to-use-client-library.md#pushnotifications)  
  <span data-ttu-id="b0e0d-152">Šablony umožňují nabízených oznámení napříč platformami toosend flexibilitu a lokalizované nabízených oznámení.</span><span class="sxs-lookup"><span data-stu-id="b0e0d-152">Templates give you flexibility toosend cross-platform pushes and localized pushes.</span></span> <span data-ttu-id="b0e0d-153">Zjistěte, jak tooregister šablony.</span><span class="sxs-lookup"><span data-stu-id="b0e0d-153">Learn how tooregister templates.</span></span>
* [<span data-ttu-id="b0e0d-154">Diagnostikovat problémy nabízená oznámení</span><span class="sxs-lookup"><span data-stu-id="b0e0d-154">Diagnose push notification issues</span></span>](../notification-hubs/notification-hubs-push-notification-fixer.md)  
  <span data-ttu-id="b0e0d-155">Existují různé důvody, proč oznámení může získat vyřadit ani nekončí na zařízení.</span><span class="sxs-lookup"><span data-stu-id="b0e0d-155">There are various reasons why notifications may get dropped or do not end up on devices.</span></span> <span data-ttu-id="b0e0d-156">Toto téma ukazuje, jak tooanalyze a vyřešení hello kořenové způsobit selhání nabízená oznámení.</span><span class="sxs-lookup"><span data-stu-id="b0e0d-156">This topic shows you how tooanalyze and figure out hello root cause of push notification failures.</span></span>

<span data-ttu-id="b0e0d-157">Vezměte v úvahu pokračovat na tooone hello následující kurzy:</span><span class="sxs-lookup"><span data-stu-id="b0e0d-157">Consider continuing on tooone of hello following tutorials:</span></span>

* [<span data-ttu-id="b0e0d-158">Přidat aplikaci tooyour ověřování</span><span class="sxs-lookup"><span data-stu-id="b0e0d-158">Add authentication tooyour app</span></span>](app-service-mobile-windows-store-dotnet-get-started-users.md)  
  <span data-ttu-id="b0e0d-159">Zjistěte, jak tooauthenticate uživatele vaší aplikace pomocí zprostředkovatele identity.</span><span class="sxs-lookup"><span data-stu-id="b0e0d-159">Learn how tooauthenticate users of your app with an identity provider.</span></span>
* [<span data-ttu-id="b0e0d-160">Povolení offline synchronizace u aplikace</span><span class="sxs-lookup"><span data-stu-id="b0e0d-160">Enable offline sync for your app</span></span>](app-service-mobile-windows-store-dotnet-get-started-offline-data.md)  
  <span data-ttu-id="b0e0d-161">Zjistěte, jak tooadd offline podporují aplikace pomocí back-end mobilní aplikace.</span><span class="sxs-lookup"><span data-stu-id="b0e0d-161">Learn how tooadd offline support your app using an Mobile App backend.</span></span> <span data-ttu-id="b0e0d-162">Offline synchronizace umožňuje koncovým uživatelům toointeract s mobilní aplikací&mdash;zobrazení, přidávat a upravovat data&mdash;i v případě, že není žádné síťové připojení.</span><span class="sxs-lookup"><span data-stu-id="b0e0d-162">Offline sync allows end-users toointeract with a mobile app&mdash;viewing, adding, or modifying data&mdash;even when there is no network connection.</span></span>

<!-- Anchors. -->

<!-- URLs. -->
[Azure Portal]: https://portal.azure.com/

<!-- Images. -->
