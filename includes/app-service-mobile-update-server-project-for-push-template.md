<span data-ttu-id="130b3-101">V této části aktualizujte kód ve vaší stávající toosend projektu back-end mobilní aplikace nabízených oznámení pokaždé, když se při přidání nové položky.</span><span class="sxs-lookup"><span data-stu-id="130b3-101">In this section, you update code in your existing Mobile Apps back-end project toosend a push notification every time a new item is added.</span></span> <span data-ttu-id="130b3-102">Používá technologii hello [šablony](../articles/notification-hubs/notification-hubs-templates-cross-platform-push-messages.md) funkce Azure Notification Hubs, povolení nabízených oznámení napříč platformami.</span><span class="sxs-lookup"><span data-stu-id="130b3-102">This is powered by hello [template](../articles/notification-hubs/notification-hubs-templates-cross-platform-push-messages.md) feature of Azure Notification Hubs, enabling cross-platform pushes.</span></span> <span data-ttu-id="130b3-103">Hello různých klientů jsou registrované pro nabízená oznámení pomocí šablony, a jeden universal push můžete získat tooall klientských platforem.</span><span class="sxs-lookup"><span data-stu-id="130b3-103">hello various clients are registered for push notifications using templates, and a single universal push can get tooall client platforms.</span></span>

<span data-ttu-id="130b3-104">Vyberte jednu z následujících postupů hello, který odpovídá typu vašeho projektu back-end&mdash;buď [.NET back-end](#dotnet) nebo [back-end Node.js](#nodejs).</span><span class="sxs-lookup"><span data-stu-id="130b3-104">Choose one of hello following procedures that matches your back-end project type&mdash;either [.NET back end](#dotnet) or [Node.js back end](#nodejs).</span></span>

### <span data-ttu-id="130b3-105"><a name="dotnet"></a>Rozhraní .NET back-end projektu</span><span class="sxs-lookup"><span data-stu-id="130b3-105"><a name="dotnet"></a>.NET back-end project</span></span>
1. <span data-ttu-id="130b3-106">V sadě Visual Studio, klikněte pravým tlačítkem na hello serverový projekt a klikněte na tlačítko **spravovat balíčky NuGet**.</span><span class="sxs-lookup"><span data-stu-id="130b3-106">In Visual Studio, right-click hello server project and click **Manage NuGet Packages**.</span></span> <span data-ttu-id="130b3-107">Vyhledejte `Microsoft.Azure.NotificationHubs`a potom klikněte na **nainstalovat**.</span><span class="sxs-lookup"><span data-stu-id="130b3-107">Search for `Microsoft.Azure.NotificationHubs`, and then click **Install**.</span></span> <span data-ttu-id="130b3-108">Tím se nainstaluje hello knihovně centra oznámení pro odesílání oznámení z back-end vašeho.</span><span class="sxs-lookup"><span data-stu-id="130b3-108">This installs hello Notification Hubs library for sending notifications from your back end.</span></span>
2. <span data-ttu-id="130b3-109">V projektu hello serveru otevřete **řadiče** > **TodoItemController.cs**a přidejte hello následující příkazy:</span><span class="sxs-lookup"><span data-stu-id="130b3-109">In hello server project, open **Controllers** > **TodoItemController.cs**, and add hello following using statements:</span></span>

        using System.Collections.Generic;
        using Microsoft.Azure.NotificationHubs;
        using Microsoft.Azure.Mobile.Server.Config;
3. <span data-ttu-id="130b3-110">V hello **PostTodoItem** metoda, přidejte následující kód po volání hello příliš hello**InsertAsync**:</span><span class="sxs-lookup"><span data-stu-id="130b3-110">In hello **PostTodoItem** method, add hello following code after hello call too**InsertAsync**:</span></span>  

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

    <span data-ttu-id="130b3-111">Tím se odešle šablony oznámení, že obsahuje položku hello. Text, když je vložit novou položku.</span><span class="sxs-lookup"><span data-stu-id="130b3-111">This sends a template notification that contains hello item.Text when a new item is inserted.</span></span>
4. <span data-ttu-id="130b3-112">Znovu publikujte hello serverový projekt.</span><span class="sxs-lookup"><span data-stu-id="130b3-112">Republish hello server project.</span></span>

### <span data-ttu-id="130b3-113"><a name="nodejs"></a>Projektu back-end Node.js</span><span class="sxs-lookup"><span data-stu-id="130b3-113"><a name="nodejs"></a>Node.js back-end project</span></span>
1. <span data-ttu-id="130b3-114">Pokud jste zatím žádný nevytvořili, [stáhnout hello rychlý start back-end projektu](../articles/app-service-mobile/app-service-mobile-node-backend-how-to-use-server-sdk.md#download-quickstart), nebo jinak použijte hello [online editor v hello portál Azure](../articles/app-service-mobile/app-service-mobile-node-backend-how-to-use-server-sdk.md#online-editor).</span><span class="sxs-lookup"><span data-stu-id="130b3-114">If you haven't already done so, [download hello quickstart back-end project](../articles/app-service-mobile/app-service-mobile-node-backend-how-to-use-server-sdk.md#download-quickstart), or else use hello [online editor in hello Azure portal](../articles/app-service-mobile/app-service-mobile-node-backend-how-to-use-server-sdk.md#online-editor).</span></span>
2. <span data-ttu-id="130b3-115">Nahraďte existující kód hello v todoitem.js hello následující:</span><span class="sxs-lookup"><span data-stu-id="130b3-115">Replace hello existing code in todoitem.js with hello following:</span></span>

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

    <span data-ttu-id="130b3-116">Tím se odešle šablony oznámení, že obsahuje hello item.text při vložení nové položky.</span><span class="sxs-lookup"><span data-stu-id="130b3-116">This sends a template notification that contains hello item.text when a new item is inserted.</span></span>
3. <span data-ttu-id="130b3-117">Při úpravách souboru hello v místním počítači, znovu publikujte hello serverový projekt.</span><span class="sxs-lookup"><span data-stu-id="130b3-117">When editing hello file on your local computer, republish hello server project.</span></span>
