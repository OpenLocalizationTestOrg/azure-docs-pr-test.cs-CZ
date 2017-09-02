
V této části ukazuje, jak k odesílání novinek jako s příznakem šablony oznámení z konzoly aplikace .NET.

Pokud používáte Mobile Apps naleznete [přidat nabízená oznámení pro Mobile Apps] kurzu a vyberte platformu v horní části.

Pokud chcete použít Java nebo PHP prostudujte si [jak používat centra oznámení z Java/PHP]. Oznámení můžete odesílat z jakékoli back-end pomocí [rozhraní REST centra oznámení].

Přeskočit kroky 1 – 3, pokud jste vytvořili konzolovou aplikaci pro odesílání oznámení, když jste dokončili [Začínáme s Notification Hubs].

1. V sadě Visual Studio vytvořte novou aplikaci konzoly Visual C#:
   
       ![][13]
2. V hlavní nabídce sady Visual Studio, klikněte na **nástroje**, **Správce balíčků knihoven**, a **Konzola správce balíčků**, pak v okně konzoly zadejte následující příkaz a stiskněte klávesu  **Zadejte**:
   
        Install-Package Microsoft.Azure.NotificationHubs
   
    Ten přidá odkaz na sadu SDK centra oznámení Azure pomocí [balíčku Microsoft.Azure.Notification Hubs NuGet].
3. Otevřete soubor Program.cs a přidejte následující `using` příkaz:
   
        using Microsoft.Azure.NotificationHubs;
4. V `Program` třídy, přidejte následující metodu nebo ho nahradit, pokud již existuje:
   
        private static async void SendTemplateNotificationAsync()
        {
            // Define the notification hub.
            NotificationHubClient hub =
                NotificationHubClient.CreateClientFromConnectionString(
                    "<connection string with full access>", "<hub name>");
   
            // Create an array of breaking news categories.
            var categories = new string[] { "World", "Politics", "Business",
                                            "Technology", "Science", "Sports"};
   
            // Sending the notification as a template notification. All template registrations that contain
            // "messageParam" and the proper tags will receive the notifications.
            // This includes APNS, GCM, WNS, and MPNS template registrations.
   
            Dictionary<string, string> templateParams = new Dictionary<string, string>();
   
            foreach (var category in categories)
            {
                templateParams["messageParam"] = "Breaking " + category + " News!";
                await hub.SendTemplateNotificationAsync(templateParams, category);
            }
         }
   
    Tento kód odešle šablony oznámení pro každou z šesti značky v poli řetězců. Použití značek zajišťuje, že zařízení obdrží oznámení pouze pro registrované kategorie.
5. Ve výše uvedeném kódu nahraďte `<hub name>` a `<connection string with full access>` zástupné symboly pomocí názvu centra oznámení a připojovacího řetězce pro *DefaultFullSharedAccessSignature* na řídicím panelu Centra oznámení.
6. Přidejte následující řádky do metody **Hlavní**:
   
         SendTemplateNotificationAsync();
         Console.ReadLine();
7. Vytvoření konzolové aplikace.

<!-- Images. -->
[13]: ./media/notification-hubs-back-end/notification-hub-create-console-app.png

<!-- URLs. -->
[Začínáme s Notification Hubs]: ../articles/notification-hubs/notification-hubs-windows-store-dotnet-get-started-wns-push-notification.md
[rozhraní REST centra oznámení]: http://msdn.microsoft.com/library/windowsazure/dn223264.aspx
[přidat nabízená oznámení pro Mobile Apps]: ../articles/app-service-mobile/app-service-mobile-windows-store-dotnet-get-started-push.md
[jak používat centra oznámení z Java/PHP]: ../articles/notification-hubs/notification-hubs-java-push-notification-tutorial.md
[balíčku Microsoft.Azure.Notification Hubs NuGet]: http://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/
