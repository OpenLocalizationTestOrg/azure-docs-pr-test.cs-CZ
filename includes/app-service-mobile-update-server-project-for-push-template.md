V této části aktualizujte kód ve vaší stávající toosend projektu back-end mobilní aplikace nabízených oznámení pokaždé, když se při přidání nové položky. Používá technologii hello [šablony](../articles/notification-hubs/notification-hubs-templates-cross-platform-push-messages.md) funkce Azure Notification Hubs, povolení nabízených oznámení napříč platformami. Hello různých klientů jsou registrované pro nabízená oznámení pomocí šablony, a jeden universal push můžete získat tooall klientských platforem.

Vyberte jednu z následujících postupů hello, který odpovídá typu vašeho projektu back-end&mdash;buď [.NET back-end](#dotnet) nebo [back-end Node.js](#nodejs).

### <a name="dotnet"></a>Rozhraní .NET back-end projektu
1. V sadě Visual Studio, klikněte pravým tlačítkem na hello serverový projekt a klikněte na tlačítko **spravovat balíčky NuGet**. Vyhledejte `Microsoft.Azure.NotificationHubs`a potom klikněte na **nainstalovat**. Tím se nainstaluje hello knihovně centra oznámení pro odesílání oznámení z back-end vašeho.
2. V projektu hello serveru otevřete **řadiče** > **TodoItemController.cs**a přidejte hello následující příkazy:

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

        // Create a new Notification Hub client.
        NotificationHubClient hub = NotificationHubClient
        .CreateClientFromConnectionString(notificationHubConnection, notificationHubName);

        // Sending hello message so that all template registrations that contain "messageParam"
        // will receive hello notifications. This includes APNS, GCM, WNS, and MPNS template registrations.
        Dictionary<string,string> templateParams = new Dictionary<string,string>();
        templateParams["messageParam"] = item.Text + " was added toohello list.";

        try
        {
            // Send hello push notification and log hello results.
            var result = await hub.SendTemplateNotificationAsync(templateParams);

            // Write hello success result toohello logs.
            config.Services.GetTraceWriter().Info(result.State.ToString());
        }
        catch (System.Exception ex)
        {
            // Write hello failure result toohello logs.
            config.Services.GetTraceWriter()
                .Error(ex.Message, null, "Push.SendAsync Error");
        }

    Tím se odešle šablony oznámení, že obsahuje položku hello. Text, když je vložit novou položku.
4. Znovu publikujte hello serverový projekt.

### <a name="nodejs"></a>Projektu back-end Node.js
1. Pokud jste zatím žádný nevytvořili, [stáhnout hello rychlý start back-end projektu](../articles/app-service-mobile/app-service-mobile-node-backend-how-to-use-server-sdk.md#download-quickstart), nebo jinak použijte hello [online editor v hello portál Azure](../articles/app-service-mobile/app-service-mobile-node-backend-how-to-use-server-sdk.md#online-editor).
2. Nahraďte existující kód hello v todoitem.js hello následující:

        var azureMobileApps = require('azure-mobile-apps'),
        promises = require('azure-mobile-apps/src/utilities/promises'),
        logger = require('azure-mobile-apps/src/logger');

        var table = azureMobileApps.table();

        table.insert(function (context) {
        // For more information about hello Notification Hubs JavaScript SDK,
        // see http://aka.ms/nodejshubs
        logger.info('Running TodoItem.insert');

        // Define hello template payload.
        var payload = '{"messageParam": "' + context.item.text + '" }';  

        // Execute hello insert.  hello insert returns hello results as a Promise,
        // Do hello push as a post-execute action within hello promise flow.
        return context.execute()
            .then(function (results) {
                // Only do hello push if configured
                if (context.push) {
                    // Send a template notification.
                    context.push.send(null, payload, function (error) {
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

    Tím se odešle šablony oznámení, že obsahuje hello item.text při vložení nové položky.
3. Při úpravách souboru hello v místním počítači, znovu publikujte hello serverový projekt.
