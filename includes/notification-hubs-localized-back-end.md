



<span data-ttu-id="566f3-101">Při odesílání šablony oznámení, že potřebujete jenom tooprovide sadu vlastností v našem případě zašleme hello sadu vlastností obsahující lokalizované verze hello hello aktuální příspěvků, například:</span><span class="sxs-lookup"><span data-stu-id="566f3-101">When you send template notifications you only need tooprovide a set of properties, in our case we will send hello set of properties containing hello localized version of hello current news, for instance:</span></span>

    {
        "News_English": "World News in English!",
        "News_French": "World News in French!",
        "News_Mandarin": "World News in Mandarin!"
    }


<span data-ttu-id="566f3-102">Tato část uvádí, jak toosend oznámení pomocí konzolové aplikace</span><span class="sxs-lookup"><span data-stu-id="566f3-102">This section shows how toosend notifications using a console app</span></span>

<span data-ttu-id="566f3-103">Hello zahrnuty kód všesměrové tooboth Windows Store a zařízení s iOS, vzhledem k tomu, že back-end hello můžete vysílání tooany hello podporované zařízení.</span><span class="sxs-lookup"><span data-stu-id="566f3-103">hello included code broadcasts tooboth Windows Store and iOS devices, since hello backend can broadcast tooany of hello supported devices.</span></span>

### <a name="toosend-notifications-using-a-c-console-app"></a><span data-ttu-id="566f3-104">toosend oznámení pomocí konzolové aplikace jazyka C#</span><span class="sxs-lookup"><span data-stu-id="566f3-104">toosend notifications using a C# console app</span></span>
<span data-ttu-id="566f3-105">Upravit hello `SendTemplateNotificationAsync` metoda v hello konzolovou aplikaci jste dříve vytvořili pomocí hello následující kód.</span><span class="sxs-lookup"><span data-stu-id="566f3-105">Modify hello `SendTemplateNotificationAsync` method in hello console app you previously created with hello following code.</span></span> <span data-ttu-id="566f3-106">Všimněte si, jak v tomto případě je bez nutnosti toosend několik oznámení pro různá národní prostředí a platformy.</span><span class="sxs-lookup"><span data-stu-id="566f3-106">Notice how in this case there is no need toosend multiple notifications for different locales and platforms.</span></span>

        private static async void SendTemplateNotificationAsync()
        {
            // Define hello notification hub.
            NotificationHubClient hub = 
                NotificationHubClient.CreateClientFromConnectionString(
                    "<connection string with full access>", "<hub name>");

            // Sending hello notification as a template notification. All template registrations that contain 
            // "messageParam" or "News_<local selected>" and hello proper tags will receive hello notifications. 
            // This includes APNS, GCM, WNS, and MPNS template registrations.
            Dictionary<string, string> templateParams = new Dictionary<string, string>();

            // Create an array of breaking news categories.
            var categories = new string[] { "World", "Politics", "Business", "Technology", "Science", "Sports"};
            var locales = new string[] { "English", "French", "Mandarin" };

            foreach (var category in categories)
            {
                templateParams["messageParam"] = "Breaking " + category + " News!";

                // Sending localized News for each tag too...
                foreach( var locale in locales)
                {
                    string key = "News_" + locale;

                    // Your real localized news content would go here.
                    templateParams[key] = "Breaking " + category + " News in " + locale + "!";
                }

                await hub.SendTemplateNotificationAsync(templateParams, category);
            }
        }


<span data-ttu-id="566f3-107">Všimněte si, že toto jednoduché volání dodá hello lokalizované část zprávy příliš**všechny** zařízení, bez ohledu na platformu hello jako vaše Centrum oznámení sestavení a doručí hello správné předplatné nativní datové části tooall hello zařízení tooa konkrétní značka.</span><span class="sxs-lookup"><span data-stu-id="566f3-107">Note that this simple call will deliver hello localized piece of news too**all** your devices, irrespective of hello platform, as your Notification Hub builds and delivers hello correct native payload tooall hello devices subscribed tooa specific tag.</span></span>

### <a name="sending-hello-notification-with-mobile-services"></a><span data-ttu-id="566f3-108">Odesílání oznámení hello s Mobile Services</span><span class="sxs-lookup"><span data-stu-id="566f3-108">Sending hello notification with Mobile Services</span></span>
<span data-ttu-id="566f3-109">V Plánovač mobilních služeb můžete použít hello následující skript:</span><span class="sxs-lookup"><span data-stu-id="566f3-109">In your Mobile Service scheduler, you can use hello following script:</span></span>

    var azure = require('azure');
    var notificationHubService = azure.createNotificationHubService('<hub name>', '<connection string with full access>');
    var notification = {
            "News_English": "World News in English!",
            "News_French": "World News in French!",
            "News_Mandarin", "World News in Mandarin!"
    }
    notificationHubService.send('World', notification, function(error) {
        if (!error) {
            console.warn("Notification successful");
        }
    });


