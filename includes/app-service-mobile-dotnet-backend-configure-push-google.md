<span data-ttu-id="5448b-101">Hello postupem, který odpovídá typu vašeho projektu back-end&mdash;buď [.NET back-end](#dotnet) nebo [back-end Node.js](#nodejs).</span><span class="sxs-lookup"><span data-stu-id="5448b-101">Use hello procedure that matches your back-end project type&mdash;either [.NET back end](#dotnet) or [Node.js back end](#nodejs).</span></span>

### <span data-ttu-id="5448b-102"><a name="dotnet"></a>Rozhraní .NET back-end projektu</span><span class="sxs-lookup"><span data-stu-id="5448b-102"><a name="dotnet"></a>.NET back-end project</span></span>
1. <span data-ttu-id="5448b-103">V sadě Visual Studio, klikněte pravým tlačítkem na hello serverový projekt a klikněte na tlačítko **spravovat balíčky NuGet**.</span><span class="sxs-lookup"><span data-stu-id="5448b-103">In Visual Studio, right-click hello server project, and click **Manage NuGet Packages**.</span></span> <span data-ttu-id="5448b-104">Vyhledejte `Microsoft.Azure.NotificationHubs`a potom klikněte na **nainstalovat**.</span><span class="sxs-lookup"><span data-stu-id="5448b-104">Search for `Microsoft.Azure.NotificationHubs`, and then click **Install**.</span></span> <span data-ttu-id="5448b-105">Tím se nainstaluje Klientská knihovna pro hello centra oznámení.</span><span class="sxs-lookup"><span data-stu-id="5448b-105">This installs hello Notification Hubs client library.</span></span>
2. <span data-ttu-id="5448b-106">Ve složce hello řadiče, otevřete TodoItemController.cs a přidejte následující hello `using` příkazy:</span><span class="sxs-lookup"><span data-stu-id="5448b-106">In hello Controllers folder, open TodoItemController.cs and add hello following `using` statements:</span></span>

        using Microsoft.Azure.Mobile.Server.Config;
        using Microsoft.Azure.NotificationHubs;
3. <span data-ttu-id="5448b-107">Nahraďte hello `PostTodoItem` metoda s hello následující kód:</span><span class="sxs-lookup"><span data-stu-id="5448b-107">Replace hello `PostTodoItem` method with hello following code:</span></span>  

        public async Task<IHttpActionResult> PostTodoItem(TodoItem item)
        {
            TodoItem current = await InsertAsync(item);
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

            // Android payload
            var androidNotificationPayload = "{ \"data\" : {\"message\":\"" + item.Text + "\"}}";

            try
            {
                // Send hello push notification and log hello results.
                var result = await hub.SendGcmNativeNotificationAsync(androidNotificationPayload);

                // Write hello success result toohello logs.
                config.Services.GetTraceWriter().Info(result.State.ToString());
            }
            catch (System.Exception ex)
            {
                // Write hello failure result toohello logs.
                config.Services.GetTraceWriter()
                    .Error(ex.Message, null, "Push.SendAsync Error");
            }
            return CreatedAtRoute("Tables", new { id = current.Id }, current);
        }

4. <span data-ttu-id="5448b-108">Znovu publikujte hello serverový projekt.</span><span class="sxs-lookup"><span data-stu-id="5448b-108">Republish hello server project.</span></span>

### <span data-ttu-id="5448b-109"><a name="nodejs"></a>Projektu back-end Node.js</span><span class="sxs-lookup"><span data-stu-id="5448b-109"><a name="nodejs"></a>Node.js back-end project</span></span>
1. <span data-ttu-id="5448b-110">Pokud jste zatím žádný nevytvořili, [stažení projektu typu rychlý start hello](../articles/app-service-mobile/app-service-mobile-node-backend-how-to-use-server-sdk.md#download-quickstart), nebo jinak použijte hello [online editor v hello portál Azure](../articles/app-service-mobile/app-service-mobile-node-backend-how-to-use-server-sdk.md#online-editor).</span><span class="sxs-lookup"><span data-stu-id="5448b-110">If you haven't already done so, [download hello quickstart project](../articles/app-service-mobile/app-service-mobile-node-backend-how-to-use-server-sdk.md#download-quickstart), or else use hello [online editor in hello Azure portal](../articles/app-service-mobile/app-service-mobile-node-backend-how-to-use-server-sdk.md#online-editor).</span></span>
2. <span data-ttu-id="5448b-111">Nahraďte hello existující kód v souboru todoitem.js hello hello následující:</span><span class="sxs-lookup"><span data-stu-id="5448b-111">Replace hello existing code in hello todoitem.js file with hello following:</span></span>

        var azureMobileApps = require('azure-mobile-apps'),
        promises = require('azure-mobile-apps/src/utilities/promises'),
        logger = require('azure-mobile-apps/src/logger');

        var table = azureMobileApps.table();

        table.insert(function (context) {
        // For more information about hello Notification Hubs JavaScript SDK,
        // see http://aka.ms/nodejshubs
        logger.info('Running TodoItem.insert');

        // Define hello GCM payload.
        var payload = {
            "data": {
                "message": context.item.text
            }
        };   

        // Execute hello insert.  hello insert returns hello results as a Promise,
        // Do hello push as a post-execute action within hello promise flow.
        return context.execute()
            .then(function (results) {
                // Only do hello push if configured
                if (context.push) {
                    // Send a GCM native notification.
                    context.push.gcm.send(null, payload, function (error) {
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

    <span data-ttu-id="5448b-112">Tím se odešle oznámení GCM, že obsahuje hello item.text při vložení nová položka todo.</span><span class="sxs-lookup"><span data-stu-id="5448b-112">This sends a GCM notification that contains hello item.text when a new todo item is inserted.</span></span>
3. <span data-ttu-id="5448b-113">Při úpravě souboru hello v místním počítači, znovu publikujte hello serverový projekt.</span><span class="sxs-lookup"><span data-stu-id="5448b-113">When editing hello file in your local computer, republish hello server project.</span></span>
