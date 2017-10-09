
Tato část uvádí, jak toosend nejnovější zprávy jako přes označené šablony oznámení z konzoly aplikace .NET.

Pokud používáte Mobile Apps naleznete toohello [přidat nabízená oznámení pro Mobile Apps] kurzu a vyberte platformu v horní části hello.

Pokud chcete toouse Java nebo PHP najdete příliš[jak toouse centra oznámení z Java/PHP]. Oznámení můžete odesílat z jakékoli back-end pomocí [rozhraní REST centra oznámení].

Přeskočit kroky 1 – 3, pokud jste vytvořili hello konzolovou aplikaci pro odesílání oznámení, když jste dokončili [Začínáme s Notification Hubs].

1. V sadě Visual Studio vytvořte novou aplikaci konzoly Visual C#:
   
       ![][13]
2. V hlavní nabídce hello sady Visual Studio, klikněte na **nástroje**, **Správce balíčků knihoven**, a **Konzola správce balíčků**, pak v okně konzoly hello zadejte následující příkaz a stiskněte klávesu **Zadejte**:
   
        Install-Package Microsoft.Azure.NotificationHubs
   
    Tento postup přidá odkaz toohello SDK centra oznámení Azure pomocí hello [balíčku Microsoft.Azure.Notification Hubs NuGet].
3. Otevřete hello soubor Program.cs a přidejte následující hello `using` příkaz:
   
        using Microsoft.Azure.NotificationHubs;
4. V hello `Program` třídy, přidejte následující metodu hello nebo ho nahradit, pokud již existuje:
   
        private static async void SendTemplateNotificationAsync()
        {
            // Define hello notification hub.
            NotificationHubClient hub =
                NotificationHubClient.CreateClientFromConnectionString(
                    "<connection string with full access>", "<hub name>");
   
            // Create an array of breaking news categories.
            var categories = new string[] { "World", "Politics", "Business",
                                            "Technology", "Science", "Sports"};
   
            // Sending hello notification as a template notification. All template registrations that contain
            // "messageParam" and hello proper tags will receive hello notifications.
            // This includes APNS, GCM, WNS, and MPNS template registrations.
   
            Dictionary<string, string> templateParams = new Dictionary<string, string>();
   
            foreach (var category in categories)
            {
                templateParams["messageParam"] = "Breaking " + category + " News!";
                await hub.SendTemplateNotificationAsync(templateParams, category);
            }
         }
   
    Tento kód odešle šablony oznámení pro každou z šesti značky hello v hello pole řetězců. Hello použití značky zajišťuje, že zařízení obdrží oznámení pouze pro hello zaregistrován kategorie.
5. V hello výše kódu, nahraďte hello `<hub name>` a `<connection string with full access>` zástupné symboly oznámení centra název a hello připojovacím řetězcem pro *DefaultFullSharedAccessSignature* z řídicího panelu hello centra oznámení .
6. Přidejte následující řádky do hello hello **hlavní** metoda:
   
         SendTemplateNotificationAsync();
         Console.ReadLine();
7. Vytvoření konzolové aplikace hello.

<!-- Images. -->
[13]: ./media/notification-hubs-back-end/notification-hub-create-console-app.png

<!-- URLs. -->
[Začínáme s Notification Hubs]: ../articles/notification-hubs/notification-hubs-windows-store-dotnet-get-started-wns-push-notification.md
[rozhraní REST centra oznámení]: http://msdn.microsoft.com/library/windowsazure/dn223264.aspx
[přidat nabízená oznámení pro Mobile Apps]: ../articles/app-service-mobile/app-service-mobile-windows-store-dotnet-get-started-push.md
[jak toouse centra oznámení z Java/PHP]: ../articles/notification-hubs/notification-hubs-java-push-notification-tutorial.md
[balíčku Microsoft.Azure.Notification Hubs NuGet]: http://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/
