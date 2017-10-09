



Při odesílání šablony oznámení, že potřebujete jenom tooprovide sadu vlastností v našem případě zašleme hello sadu vlastností obsahující lokalizované verze hello hello aktuální příspěvků, například:

    {
        "News_English": "World News in English!",
        "News_French": "World News in French!",
        "News_Mandarin": "World News in Mandarin!"
    }


Tato část uvádí, jak toosend oznámení pomocí konzolové aplikace

Hello zahrnuty kód všesměrové tooboth Windows Store a zařízení s iOS, vzhledem k tomu, že back-end hello můžete vysílání tooany hello podporované zařízení.

### <a name="toosend-notifications-using-a-c-console-app"></a>toosend oznámení pomocí konzolové aplikace jazyka C#
Upravit hello `SendTemplateNotificationAsync` metoda v hello konzolovou aplikaci jste dříve vytvořili pomocí hello následující kód. Všimněte si, jak v tomto případě je bez nutnosti toosend několik oznámení pro různá národní prostředí a platformy.

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


Všimněte si, že toto jednoduché volání dodá hello lokalizované část zprávy příliš**všechny** zařízení, bez ohledu na platformu hello jako vaše Centrum oznámení sestavení a doručí hello správné předplatné nativní datové části tooall hello zařízení tooa konkrétní značka.

### <a name="sending-hello-notification-with-mobile-services"></a>Odesílání oznámení hello s Mobile Services
V Plánovač mobilních služeb můžete použít hello následující skript:

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


