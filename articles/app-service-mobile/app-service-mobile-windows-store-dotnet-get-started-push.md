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
# <a name="add-push-notifications-tooyour-windows-app"></a>Přidat nabízená oznámení tooyour Windows aplikaci
[!INCLUDE [app-service-mobile-selector-get-started-push](../../includes/app-service-mobile-selector-get-started-push.md)]

## <a name="overview"></a>Přehled
V tomto kurzu přidáte nabízená oznámení toohello [Windows úvodní](app-service-mobile-windows-store-dotnet-get-started.md) projektu tak, aby nabízených oznámení je odesláno toohello zařízení pokaždé, když vložení záznamu.

Pokud nepoužijete hello stáhli úvodní serverový projekt, bude nutné hello nabízených oznámení v balíčku rozšíření. V tématu [pracovat s hello .NET back-end serveru SDK pro Azure Mobile Apps](app-service-mobile-dotnet-backend-how-to-use-server-sdk.md) Další informace.

## <a name="configure-hub"></a>Konfigurace centra oznámení
[!INCLUDE [app-service-mobile-configure-notification-hub](../../includes/app-service-mobile-configure-notification-hub.md)]

## <a name="register-your-app-for-push-notifications"></a>Registrace aplikace pro nabízená oznámení
Můžete potřebovat toosubmit toohello vaší aplikace Windows Store, a potom nakonfigurovat toosend nabízené služby oznámení Windows (WNS) vašeho projektu toointegrate serveru.

1. V Průzkumníku řešení Visual Studio, klikněte pravým tlačítkem na projekt aplikace UPW hello, klikněte na tlačítko **úložiště** > **propojit aplikaci se hello úložiště...** .

    ![Přidružit aplikace Windows Store](./media/app-service-mobile-windows-store-dotnet-get-started-push/notification-hub-associate-uwp-app.png)
2. V Průvodci hello, klikněte na tlačítko **Další**, přihlaste se pomocí účtu Microsoft, zadejte název aplikace v rámci **rezervovat nový název aplikace**, pak klikněte na tlačítko **rezervy**.
3. Po registraci aplikace hello je úspěšně vytvořil, vyberte hello nový název aplikace, klikněte na tlačítko **Další**a potom klikněte na **přidružit**. Tento postup přidá požadované hello Windows Store registrační informace toohello manifest aplikace.  
4. Přejděte toohello [Windows Dev Center](https://dev.windows.com/en-us/overview), přihlaste se pomocí účtu Microsoft, klikněte na nové registrace aplikace hello v **Moje aplikace**, pak rozbalte **služby**  >  **Nabízená oznámení**.
5. V hello **nabízená oznámení** klikněte na tlačítko **Web služeb Live Services** pod **mobilní služby Microsoft Azure**.
6. Na stránce registrace hello, poznamenejte si hodnotu hello **tajné klíče aplikace** a hello **SID balíčku**, které budou vedle použijete tooconfigure váš back-end mobilní aplikace.

    ![Přidružit aplikace Windows Store](./media/app-service-mobile-windows-store-dotnet-get-started-push/app-service-mobile-uwp-app-push-auth.png)

   > [!IMPORTANT]
   > Hello tajný klíč klienta a SID balíčku jsou důležitá pověření zabezpečení. Tyto hodnoty s nikým nesdílejte ani je nedistribuujte s vaší aplikací. Hello **Id aplikace** se používá s ověřováním tajný tooconfigure Account Microsoft hello.
   >
   >

## <a name="configure-hello-backend-toosend-push-notifications"></a>Konfigurace hello back-end toosend nabízená oznámení
[!INCLUDE [app-service-mobile-configure-wns](../../includes/app-service-mobile-configure-wns.md)]

## <a id="update-service"></a>Aktualizovat hello serveru toosend nabízená oznámení
Pomocí postupu hello níže, který odpovídá typu vašeho projektu back-end&mdash;buď [.NET back-end](#dotnet) nebo [back-end Node.js](#nodejs).

### <a name="dotnet"></a>Rozhraní .NET back-end projektu
1. V sadě Visual Studio, klikněte pravým tlačítkem na hello serverový projekt a klikněte na tlačítko **spravovat balíčky NuGet**, vyhledejte Microsoft.Azure.NotificationHubs a pak klikněte na tlačítko **nainstalovat**. Tím se nainstaluje Klientská knihovna pro hello centra oznámení.
2. Rozbalte položku **řadiče**, otevřete TodoItemController.cs a přidejte hello následující příkazy:

        using System.Collections.Generic;
        using Microsoft.Azure.NotificationHubs;
        using Microsoft.Azure.Mobile.Server.Config;
3. V hello **PostTodoItem** metoda, přidejte následující kód po volání hello příliš hello**InsertAsync**:

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

    Tento kód informuje toosend centra oznámení hello po vložení novou položku nabízeného oznámení.
4. Znovu publikujte hello serverový projekt.

### <a name="nodejs"></a>Projektu back-end Node.js
1. Pokud jste zatím žádný nevytvořili, [stažení projektu typu rychlý start hello](app-service-mobile-node-backend-how-to-use-server-sdk.md#download-quickstart) nebo jinak použijte hello [online editor v hello portál Azure](app-service-mobile-node-backend-how-to-use-server-sdk.md#online-editor).
2. Nahraďte hello existující kód v souboru todoitem.js hello hello následující:

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

    Tím se odešle oznámení s informační zprávou WNS, který obsahuje hello item.text při vložení nová položka todo.
3. Při úpravách souboru hello v místním počítači, znovu publikujte hello serverový projekt.

## <a id="update-app"></a>Přidat nabízená oznámení tooyour aplikaci
V dalším kroku musí aplikaci zaregistrovat pro nabízená oznámení na spuštění. Pokud už jste povolili ověřování, zajistěte, aby tento uživatel hello přihlásí a teprve potom zkusili tooregister pro nabízená oznámení.

1. Otevřete hello **App.xaml.cs** souboru projektu a přidejte následující hello `using` příkazy:

        using System.Threading.Tasks;
        using Windows.Networking.PushNotifications;
2. V hello stejný soubor, přidejte následující hello **InitNotificationsAsync** metoda definice toohello **aplikace** třídy:

        private async Task InitNotificationsAsync()
        {
            // Get a channel URI from WNS.
            var channel = await PushNotificationChannelManager
                .CreatePushNotificationChannelForApplicationAsync();

            // Register hello channel URI with Notification Hubs.
            await App.MobileService.GetPush().RegisterAsync(channel.Uri);
        }

    Tento kód načte hello ChannelURI pro hello aplikaci z WNS a pak zaregistruje tento ChannelURI pomocí aplikace služby mobilní aplikace.
3. Hello horní části hello **OnLaunched** obslužné rutiny událostí v **App.xaml.cs**, přidejte hello **asynchronní** modifikátor toohello metoda definice a přidejte následující hello volání nové toohello **InitNotificationsAsync** metoda jako hello následující ukázka:

        protected async override void OnLaunched(LaunchActivatedEventArgs e)
        {
            await InitNotificationsAsync();

            // ...
        }

    To zaručuje, že hello krátkodobou ChannelURI registrovaná pokaždé, když hello aplikace spuštěna.
4. Znovu sestavte projekt aplikace UPW. Aplikace je nyní připraven tooreceive informační zprávy.

## <a id="test"></a>Nabízená oznámení v aplikaci
[!INCLUDE [app-service-mobile-windows-universal-test-push](../../includes/app-service-mobile-windows-universal-test-push.md)]

## <a id="more"></a>Další kroky
Další informace o nabízených oznámení:

* [Jak toouse hello spravovat klienta pro Azure Mobile Apps](app-service-mobile-dotnet-how-to-use-client-library.md#pushnotifications)  
  Šablony umožňují nabízených oznámení napříč platformami toosend flexibilitu a lokalizované nabízených oznámení. Zjistěte, jak tooregister šablony.
* [Diagnostikovat problémy nabízená oznámení](../notification-hubs/notification-hubs-push-notification-fixer.md)  
  Existují různé důvody, proč oznámení může získat vyřadit ani nekončí na zařízení. Toto téma ukazuje, jak tooanalyze a vyřešení hello kořenové způsobit selhání nabízená oznámení.

Vezměte v úvahu pokračovat na tooone hello následující kurzy:

* [Přidat aplikaci tooyour ověřování](app-service-mobile-windows-store-dotnet-get-started-users.md)  
  Zjistěte, jak tooauthenticate uživatele vaší aplikace pomocí zprostředkovatele identity.
* [Povolení offline synchronizace u aplikace](app-service-mobile-windows-store-dotnet-get-started-offline-data.md)  
  Zjistěte, jak tooadd offline podporují aplikace pomocí back-end mobilní aplikace. Offline synchronizace umožňuje koncovým uživatelům toointeract s mobilní aplikací&mdash;zobrazení, přidávat a upravovat data&mdash;i v případě, že není žádné síťové připojení.

<!-- Anchors. -->

<!-- URLs. -->
[Azure Portal]: https://portal.azure.com/

<!-- Images. -->
