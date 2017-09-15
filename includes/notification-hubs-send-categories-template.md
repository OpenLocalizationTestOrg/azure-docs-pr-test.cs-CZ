
<span data-ttu-id="67364-101">V této části ukazuje, jak k odesílání novinek jako s příznakem šablony oznámení z konzoly aplikace .NET.</span><span class="sxs-lookup"><span data-stu-id="67364-101">This section shows how to send breaking news as tagged template notifications from a .NET console app.</span></span>

<span data-ttu-id="67364-102">Pokud používáte Mobile Apps naleznete [přidat nabízená oznámení pro Mobile Apps] kurzu a vyberte platformu v horní části.</span><span class="sxs-lookup"><span data-stu-id="67364-102">If you are using Mobile Apps please refer to the [Add push notifications for Mobile Apps] tutorial and select your platform at the top.</span></span>

<span data-ttu-id="67364-103">Pokud chcete použít Java nebo PHP prostudujte si [jak používat centra oznámení z Java/PHP].</span><span class="sxs-lookup"><span data-stu-id="67364-103">If you want to use Java or PHP refer to [How to use Notification Hubs from Java/PHP].</span></span> <span data-ttu-id="67364-104">Oznámení můžete odesílat z jakékoli back-end pomocí [rozhraní REST centra oznámení].</span><span class="sxs-lookup"><span data-stu-id="67364-104">You can send notifications from any backend using the [Notification Hubs REST interface].</span></span>

<span data-ttu-id="67364-105">Přeskočit kroky 1 – 3, pokud jste vytvořili konzolovou aplikaci pro odesílání oznámení, když jste dokončili [Začínáme s Notification Hubs].</span><span class="sxs-lookup"><span data-stu-id="67364-105">Skip steps 1-3 if you created the console app for sending notifications when you completed [Get started with Notification Hubs].</span></span>

1. <span data-ttu-id="67364-106">V sadě Visual Studio vytvořte novou aplikaci konzoly Visual C#:</span><span class="sxs-lookup"><span data-stu-id="67364-106">In Visual Studio create a new Visual C# console application:</span></span>
   
       ![][13]
2. <span data-ttu-id="67364-107">V hlavní nabídce sady Visual Studio, klikněte na **nástroje**, **Správce balíčků knihoven**, a **Konzola správce balíčků**, pak v okně konzoly zadejte následující příkaz a stiskněte klávesu  **Zadejte**:</span><span class="sxs-lookup"><span data-stu-id="67364-107">In the Visual Studio main menu, click **Tools**, **Library Package Manager**, and **Package Manager Console**, then in the console window type the  following and press **Enter**:</span></span>
   
        Install-Package Microsoft.Azure.NotificationHubs
   
    <span data-ttu-id="67364-108">Ten přidá odkaz na sadu SDK centra oznámení Azure pomocí [balíčku Microsoft.Azure.Notification Hubs NuGet].</span><span class="sxs-lookup"><span data-stu-id="67364-108">This adds a reference to the Azure Notification Hubs SDK using the [Microsoft.Azure.Notification Hubs NuGet package].</span></span>
3. <span data-ttu-id="67364-109">Otevřete soubor Program.cs a přidejte následující `using` příkaz:</span><span class="sxs-lookup"><span data-stu-id="67364-109">Open the file Program.cs and add the following `using` statement:</span></span>
   
        using Microsoft.Azure.NotificationHubs;
4. <span data-ttu-id="67364-110">V `Program` třídy, přidejte následující metodu nebo ho nahradit, pokud již existuje:</span><span class="sxs-lookup"><span data-stu-id="67364-110">In the `Program` class, add the following method, or replace it if it already exists:</span></span>
   
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
   
    <span data-ttu-id="67364-111">Tento kód odešle šablony oznámení pro každou z šesti značky v poli řetězců.</span><span class="sxs-lookup"><span data-stu-id="67364-111">This code sends a template notification for each of the six tags in the string array.</span></span> <span data-ttu-id="67364-112">Použití značek zajišťuje, že zařízení obdrží oznámení pouze pro registrované kategorie.</span><span class="sxs-lookup"><span data-stu-id="67364-112">The use of tags makes sure that devices receive notifications only for the registered categories.</span></span>
5. <span data-ttu-id="67364-113">Ve výše uvedeném kódu nahraďte `<hub name>` a `<connection string with full access>` zástupné symboly pomocí názvu centra oznámení a připojovacího řetězce pro *DefaultFullSharedAccessSignature* na řídicím panelu Centra oznámení.</span><span class="sxs-lookup"><span data-stu-id="67364-113">In the above code, replace the `<hub name>` and `<connection string with full access>` placeholders with your notification hub name and the connection  string for *DefaultFullSharedAccessSignature* from the dashboard of your notification hub.</span></span>
6. <span data-ttu-id="67364-114">Přidejte následující řádky do metody **Hlavní**:</span><span class="sxs-lookup"><span data-stu-id="67364-114">Add the following lines in the **Main** method:</span></span>
   
         SendTemplateNotificationAsync();
         Console.ReadLine();
7. <span data-ttu-id="67364-115">Vytvoření konzolové aplikace.</span><span class="sxs-lookup"><span data-stu-id="67364-115">Build the console app.</span></span>

<!-- Images. -->
[13]: ./media/notification-hubs-back-end/notification-hub-create-console-app.png

<!-- URLs. -->
<span data-ttu-id="67364-116">[Začínáme s Notification Hubs]: ../articles/notification-hubs/notification-hubs-windows-store-dotnet-get-started-wns-push-notification.md</span><span class="sxs-lookup"><span data-stu-id="67364-116">[Get started with Notification Hubs]: ../articles/notification-hubs/notification-hubs-windows-store-dotnet-get-started-wns-push-notification.md</span></span>
<span data-ttu-id="67364-117">[rozhraní REST centra oznámení]: http://msdn.microsoft.com/library/windowsazure/dn223264.aspx</span><span class="sxs-lookup"><span data-stu-id="67364-117">[Notification Hubs REST interface]: http://msdn.microsoft.com/library/windowsazure/dn223264.aspx</span></span>
<span data-ttu-id="67364-118">[přidat nabízená oznámení pro Mobile Apps]: ../articles/app-service-mobile/app-service-mobile-windows-store-dotnet-get-started-push.md</span><span class="sxs-lookup"><span data-stu-id="67364-118">[Add push notifications for Mobile Apps]: ../articles/app-service-mobile/app-service-mobile-windows-store-dotnet-get-started-push.md</span></span>
<span data-ttu-id="67364-119">[jak používat centra oznámení z Java/PHP]: ../articles/notification-hubs/notification-hubs-java-push-notification-tutorial.md</span><span class="sxs-lookup"><span data-stu-id="67364-119">[How to use Notification Hubs from Java/PHP]: ../articles/notification-hubs/notification-hubs-java-push-notification-tutorial.md</span></span>
<span data-ttu-id="67364-120">[balíčku Microsoft.Azure.Notification Hubs NuGet]: http://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/</span><span class="sxs-lookup"><span data-stu-id="67364-120">[Microsoft.Azure.Notification Hubs NuGet package]: http://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/</span></span>
