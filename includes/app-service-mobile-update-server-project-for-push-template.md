<span data-ttu-id="b05b1-101">V této části aktualizujte kód v existující projekt back-end mobilní aplikace k odesílání nabízených oznámení pokaždé, když se při přidání nové položky.</span><span class="sxs-lookup"><span data-stu-id="b05b1-101">In this section, you update code in your existing Mobile Apps back-end project to send a push notification every time a new item is added.</span></span> <span data-ttu-id="b05b1-102">To používá technologii [šablony](../articles/notification-hubs/notification-hubs-templates-cross-platform-push-messages.md) funkce Azure Notification Hubs, povolení nabízených oznámení napříč platformami.</span><span class="sxs-lookup"><span data-stu-id="b05b1-102">This is powered by the [template](../articles/notification-hubs/notification-hubs-templates-cross-platform-push-messages.md) feature of Azure Notification Hubs, enabling cross-platform pushes.</span></span> <span data-ttu-id="b05b1-103">Různých klientů jsou registrované pro nabízená oznámení pomocí šablony a jeden universal push můžete získat na všechny klientské platformy.</span><span class="sxs-lookup"><span data-stu-id="b05b1-103">The various clients are registered for push notifications using templates, and a single universal push can get to all client platforms.</span></span>

<span data-ttu-id="b05b1-104">Vyberte jednu z následujících postupů, které odpovídá typu vašeho projektu back-end&mdash;buď [.NET back-end](#dotnet) nebo [back-end Node.js](#nodejs).</span><span class="sxs-lookup"><span data-stu-id="b05b1-104">Choose one of the following procedures that matches your back-end project type&mdash;either [.NET back end](#dotnet) or [Node.js back end](#nodejs).</span></span>

### <span data-ttu-id="b05b1-105"><a name="dotnet"></a>Rozhraní .NET back-end projektu</span><span class="sxs-lookup"><span data-stu-id="b05b1-105"><a name="dotnet"></a>.NET back-end project</span></span>
1. <span data-ttu-id="b05b1-106">V sadě Visual Studio, klikněte pravým tlačítkem na serverový projekt a klikněte na tlačítko **spravovat balíčky NuGet**.</span><span class="sxs-lookup"><span data-stu-id="b05b1-106">In Visual Studio, right-click the server project and click **Manage NuGet Packages**.</span></span> <span data-ttu-id="b05b1-107">Vyhledejte `Microsoft.Azure.NotificationHubs`a potom klikněte na **nainstalovat**.</span><span class="sxs-lookup"><span data-stu-id="b05b1-107">Search for `Microsoft.Azure.NotificationHubs`, and then click **Install**.</span></span> <span data-ttu-id="b05b1-108">To nainstaluje knihovny centra oznámení pro odesílání oznámení z back-end vašeho.</span><span class="sxs-lookup"><span data-stu-id="b05b1-108">This installs the Notification Hubs library for sending notifications from your back end.</span></span>
2. <span data-ttu-id="b05b1-109">V projektu serveru otevřete **řadiče** > **TodoItemController.cs**a přidejte následující příkazy:</span><span class="sxs-lookup"><span data-stu-id="b05b1-109">In the server project, open **Controllers** > **TodoItemController.cs**, and add the following using statements:</span></span>

        using System.Collections.Generic;
        using Microsoft.Azure.NotificationHubs;
        using Microsoft.Azure.Mobile.Server.Config;
3. <span data-ttu-id="b05b1-110">V **PostTodoItem** metoda, přidejte následující kód po volání **InsertAsync**:</span><span class="sxs-lookup"><span data-stu-id="b05b1-110">In the **PostTodoItem** method, add the following code after the call to **InsertAsync**:</span></span>  

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

        // Sending the message so that all template registrations that contain "messageParam"
        // will receive the notifications. This includes APNS, GCM, WNS, and MPNS template registrations.
        Dictionary<string,string> templateParams = new Dictionary<string,string>();
        templateParams["messageParam"] = item.Text + " was added to the list.";

        try
        {
            // Send the push notification and log the results.
            var result = await hub.SendTemplateNotificationAsync(templateParams);

            // Write the success result to the logs.
            config.Services.GetTraceWriter().Info(result.State.ToString());
        }
        catch (System.Exception ex)
        {
            // Write the failure result to the logs.
            config.Services.GetTraceWriter()
                .Error(ex.Message, null, "Push.SendAsync Error");
        }

    <span data-ttu-id="b05b1-111">Tím se odešle šablony oznámení, který obsahuje položky. Text, když je vložit novou položku.</span><span class="sxs-lookup"><span data-stu-id="b05b1-111">This sends a template notification that contains the item.Text when a new item is inserted.</span></span>
4. <span data-ttu-id="b05b1-112">Znovu publikujte serverový projekt.</span><span class="sxs-lookup"><span data-stu-id="b05b1-112">Republish the server project.</span></span>

### <span data-ttu-id="b05b1-113"><a name="nodejs"></a>Projektu back-end Node.js</span><span class="sxs-lookup"><span data-stu-id="b05b1-113"><a name="nodejs"></a>Node.js back-end project</span></span>
1. <span data-ttu-id="b05b1-114">Pokud jste zatím žádný nevytvořili, [stáhnout back-end projektu pro rychlý Start](../articles/app-service-mobile/app-service-mobile-node-backend-how-to-use-server-sdk.md#download-quickstart), nebo použijte jiný [online editor na webu Azure portal](../articles/app-service-mobile/app-service-mobile-node-backend-how-to-use-server-sdk.md#online-editor).</span><span class="sxs-lookup"><span data-stu-id="b05b1-114">If you haven't already done so, [download the quickstart back-end project](../articles/app-service-mobile/app-service-mobile-node-backend-how-to-use-server-sdk.md#download-quickstart), or else use the [online editor in the Azure portal](../articles/app-service-mobile/app-service-mobile-node-backend-how-to-use-server-sdk.md#online-editor).</span></span>
2. <span data-ttu-id="b05b1-115">Nahraďte stávající kód v todoitem.js s následujícími službami:</span><span class="sxs-lookup"><span data-stu-id="b05b1-115">Replace the existing code in todoitem.js with the following:</span></span>

        var azureMobileApps = require('azure-mobile-apps'),
        promises = require('azure-mobile-apps/src/utilities/promises'),
        logger = require('azure-mobile-apps/src/logger');

        var table = azureMobileApps.table();

        table.insert(function (context) {
        // For more information about the Notification Hubs JavaScript SDK,
        // see http://aka.ms/nodejshubs
        logger.info('Running TodoItem.insert');

        // Define the template payload.
        var payload = '{"messageParam": "' + context.item.text + '" }';  

        // Execute the insert.  The insert returns the results as a Promise,
        // Do the push as a post-execute action within the promise flow.
        return context.execute()
            .then(function (results) {
                // Only do the push if configured
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
                // Don't forget to return the results from the context.execute()
                return results;
            })
            .catch(function (error) {
                logger.error('Error while running context.execute: ', error);
            });
        });

        module.exports = table;  

    <span data-ttu-id="b05b1-116">Tím se odešle oznámení šablona obsahující item.text při vložení nové položky.</span><span class="sxs-lookup"><span data-stu-id="b05b1-116">This sends a template notification that contains the item.text when a new item is inserted.</span></span>
3. <span data-ttu-id="b05b1-117">Při úpravách souboru v místním počítači, znovu publikujte serverový projekt.</span><span class="sxs-lookup"><span data-stu-id="b05b1-117">When editing the file on your local computer, republish the server project.</span></span>
