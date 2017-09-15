
* <span data-ttu-id="9d11f-101">**Rozhraní .NET back-end (C#)**:</span><span class="sxs-lookup"><span data-stu-id="9d11f-101">**.NET backend (C#)**:</span></span>      
  
  1. <span data-ttu-id="9d11f-102">V sadě Visual Studio, klikněte pravým tlačítkem na serverový projekt a klikněte na tlačítko **spravovat balíčky NuGet**, vyhledejte `Microsoft.Azure.NotificationHubs`, pak klikněte na tlačítko **nainstalovat**.</span><span class="sxs-lookup"><span data-stu-id="9d11f-102">In Visual Studio, right-click the server project and click **Manage NuGet Packages**, search for `Microsoft.Azure.NotificationHubs`, then click **Install**.</span></span> <span data-ttu-id="9d11f-103">To nainstaluje knihovny centra oznámení pro odesílání oznámení z backendu.</span><span class="sxs-lookup"><span data-stu-id="9d11f-103">This installs the Notification Hubs library for sending notifications from your backend.</span></span>
  2. <span data-ttu-id="9d11f-104">V projektu sady Visual Studio back-end, otevřete **řadiče** > **TodoItemController.cs**.</span><span class="sxs-lookup"><span data-stu-id="9d11f-104">In the backend's Visual Studio project, open **Controllers** > **TodoItemController.cs**.</span></span> <span data-ttu-id="9d11f-105">Na začátek souboru přidejte následující `using` příkaz:</span><span class="sxs-lookup"><span data-stu-id="9d11f-105">At the top of the file, add the following `using` statement:</span></span>
     
          using Microsoft.Azure.Mobile.Server.Config;
          using Microsoft.Azure.NotificationHubs;

    3. <span data-ttu-id="9d11f-106">Nahraďte `PostTodoItem` metoda následujícím kódem:</span><span class="sxs-lookup"><span data-stu-id="9d11f-106">Replace the `PostTodoItem` method with the following code:</span></span>  

            public async Task<IHttpActionResult> PostTodoItem(TodoItem item)
            {
                TodoItem current = await InsertAsync(item);
                // Get the settings for the server project.
                HttpConfiguration config = this.Configuration;

                MobileAppSettingsDictionary settings = 
                    this.Configuration.GetMobileAppSettingsProvider().GetMobileAppSettings();

                // Get the Notification Hubs credentials for the Mobile App.
                string notificationHubName = settings.NotificationHubName;
                string notificationHubConnection = settings
                    .Connections[MobileAppSettingsKeys.NotificationHubConnectionString].ConnectionString;

                // Create a new Notification Hub client.
                NotificationHubClient hub = NotificationHubClient
                .CreateClientFromConnectionString(notificationHubConnection, notificationHubName);

                // iOS payload
                var appleNotificationPayload = "{\"aps\":{\"alert\":\"" + item.Text + "\"}}";

                try
                {
                    // Send the push notification and log the results.
                    var result = await hub.SendAppleNativeNotificationAsync(appleNotificationPayload);

                    // Write the success result to the logs.
                    config.Services.GetTraceWriter().Info(result.State.ToString());
                }
                catch (System.Exception ex)
                {
                    // Write the failure result to the logs.
                    config.Services.GetTraceWriter()
                        .Error(ex.Message, null, "Push.SendAsync Error");
                }
                return CreatedAtRoute("Tables", new { id = current.Id }, current);
            }

    4. <span data-ttu-id="9d11f-107">Znovu publikujte serverový projekt.</span><span class="sxs-lookup"><span data-stu-id="9d11f-107">Republish the server project.</span></span>

* <span data-ttu-id="9d11f-108">**Back-end Node.js** :</span><span class="sxs-lookup"><span data-stu-id="9d11f-108">**Node.js backend** :</span></span> 
  
  1. <span data-ttu-id="9d11f-109">Pokud jste zatím žádný nevytvořili, [stažení projektu pro rychlý Start](../articles/app-service-mobile/app-service-mobile-node-backend-how-to-use-server-sdk.md#download-quickstart) nebo použijte jiný [online editor na webu Azure portal](../articles/app-service-mobile/app-service-mobile-node-backend-how-to-use-server-sdk.md#online-editor).</span><span class="sxs-lookup"><span data-stu-id="9d11f-109">If you haven't already done so, [download the quickstart project](../articles/app-service-mobile/app-service-mobile-node-backend-how-to-use-server-sdk.md#download-quickstart) or else use the [online editor in the Azure portal](../articles/app-service-mobile/app-service-mobile-node-backend-how-to-use-server-sdk.md#online-editor).</span></span>    
  2. <span data-ttu-id="9d11f-110">Skript tabulky todoitem.js nahraďte následujícím kódem:</span><span class="sxs-lookup"><span data-stu-id="9d11f-110">Replace the todoitem.js table script with the following code:</span></span>

            var azureMobileApps = require('azure-mobile-apps'),
                promises = require('azure-mobile-apps/src/utilities/promises'),
                logger = require('azure-mobile-apps/src/logger');

            var table = azureMobileApps.table();

            // When adding record, send a push notification via APNS
            table.insert(function (context) {
                // For details of the Notification Hubs JavaScript SDK, 
                // see http://aka.ms/nodejshubs
                logger.info('Running TodoItem.insert');

                // Create a payload that contains the new item Text.
                var payload = "{\"aps\":{\"alert\":\"" + context.item.text + "\"}}";

                // Execute the insert; Push as a post-execute action when results are returned as a Promise.
                return context.execute()
                    .then(function (results) {
                        // Only do the push if configured
                        if (context.push) {
                            context.push.apns.send(null, payload, function (error) {
                                if (error) {
                                    logger.error('Error while sending push notification: ', error);
                                } else {
                                    logger.info('Push notification sent successfully!');
                                }
                            });
                        }
                        return results;
                    })
                    .catch(function (error) {
                        logger.error('Error while running context.execute: ', error);
                    });
            });

            module.exports = table;

    2. <span data-ttu-id="9d11f-111">Při úpravách souboru v místním počítači, znovu publikujte serverový projekt.</span><span class="sxs-lookup"><span data-stu-id="9d11f-111">When editing the file on your local computer, republish the server project.</span></span>