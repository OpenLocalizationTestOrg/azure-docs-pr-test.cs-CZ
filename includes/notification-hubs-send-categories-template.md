
<span data-ttu-id="4ae42-101">Tato část uvádí, jak toosend nejnovější zprávy jako přes označené šablony oznámení z konzoly aplikace .NET.</span><span class="sxs-lookup"><span data-stu-id="4ae42-101">This section shows how toosend breaking news as tagged template notifications from a .NET console app.</span></span>

<span data-ttu-id="4ae42-102">Pokud používáte Mobile Apps naleznete toohello [přidat nabízená oznámení pro Mobile Apps] kurzu a vyberte platformu v horní části hello.</span><span class="sxs-lookup"><span data-stu-id="4ae42-102">If you are using Mobile Apps please refer toohello [Add push notifications for Mobile Apps] tutorial and select your platform at hello top.</span></span>

<span data-ttu-id="4ae42-103">Pokud chcete toouse Java nebo PHP najdete příliš[jak toouse centra oznámení z Java/PHP].</span><span class="sxs-lookup"><span data-stu-id="4ae42-103">If you want toouse Java or PHP refer too[How toouse Notification Hubs from Java/PHP].</span></span> <span data-ttu-id="4ae42-104">Oznámení můžete odesílat z jakékoli back-end pomocí [rozhraní REST centra oznámení].</span><span class="sxs-lookup"><span data-stu-id="4ae42-104">You can send notifications from any backend using the [Notification Hubs REST interface].</span></span>

<span data-ttu-id="4ae42-105">Přeskočit kroky 1 – 3, pokud jste vytvořili hello konzolovou aplikaci pro odesílání oznámení, když jste dokončili [Začínáme s Notification Hubs].</span><span class="sxs-lookup"><span data-stu-id="4ae42-105">Skip steps 1-3 if you created hello console app for sending notifications when you completed [Get started with Notification Hubs].</span></span>

1. <span data-ttu-id="4ae42-106">V sadě Visual Studio vytvořte novou aplikaci konzoly Visual C#:</span><span class="sxs-lookup"><span data-stu-id="4ae42-106">In Visual Studio create a new Visual C# console application:</span></span>
   
       ![][13]
2. <span data-ttu-id="4ae42-107">V hlavní nabídce hello sady Visual Studio, klikněte na **nástroje**, **Správce balíčků knihoven**, a **Konzola správce balíčků**, pak v okně konzoly hello zadejte následující příkaz a stiskněte klávesu **Zadejte**:</span><span class="sxs-lookup"><span data-stu-id="4ae42-107">In hello Visual Studio main menu, click **Tools**, **Library Package Manager**, and **Package Manager Console**, then in hello console window type the  following and press **Enter**:</span></span>
   
        Install-Package Microsoft.Azure.NotificationHubs
   
    <span data-ttu-id="4ae42-108">Tento postup přidá odkaz toohello SDK centra oznámení Azure pomocí hello [balíčku Microsoft.Azure.Notification Hubs NuGet].</span><span class="sxs-lookup"><span data-stu-id="4ae42-108">This adds a reference toohello Azure Notification Hubs SDK using hello [Microsoft.Azure.Notification Hubs NuGet package].</span></span>
3. <span data-ttu-id="4ae42-109">Otevřete hello soubor Program.cs a přidejte následující hello `using` příkaz:</span><span class="sxs-lookup"><span data-stu-id="4ae42-109">Open hello file Program.cs and add hello following `using` statement:</span></span>
   
        using Microsoft.Azure.NotificationHubs;
4. <span data-ttu-id="4ae42-110">V hello `Program` třídy, přidejte následující metodu hello nebo ho nahradit, pokud již existuje:</span><span class="sxs-lookup"><span data-stu-id="4ae42-110">In hello `Program` class, add hello following method, or replace it if it already exists:</span></span>
   
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
   
    <span data-ttu-id="4ae42-111">Tento kód odešle šablony oznámení pro každou z šesti značky hello v hello pole řetězců.</span><span class="sxs-lookup"><span data-stu-id="4ae42-111">This code sends a template notification for each of hello six tags in hello string array.</span></span> <span data-ttu-id="4ae42-112">Hello použití značky zajišťuje, že zařízení obdrží oznámení pouze pro hello zaregistrován kategorie.</span><span class="sxs-lookup"><span data-stu-id="4ae42-112">hello use of tags makes sure that devices receive notifications only for hello registered categories.</span></span>
5. <span data-ttu-id="4ae42-113">V hello výše kódu, nahraďte hello `<hub name>` a `<connection string with full access>` zástupné symboly oznámení centra název a hello připojovacím řetězcem pro *DefaultFullSharedAccessSignature* z řídicího panelu hello centra oznámení .</span><span class="sxs-lookup"><span data-stu-id="4ae42-113">In hello above code, replace hello `<hub name>` and `<connection string with full access>` placeholders with your notification hub name and hello connection  string for *DefaultFullSharedAccessSignature* from hello dashboard of your notification hub.</span></span>
6. <span data-ttu-id="4ae42-114">Přidejte následující řádky do hello hello **hlavní** metoda:</span><span class="sxs-lookup"><span data-stu-id="4ae42-114">Add hello following lines in hello **Main** method:</span></span>
   
         SendTemplateNotificationAsync();
         Console.ReadLine();
7. <span data-ttu-id="4ae42-115">Vytvoření konzolové aplikace hello.</span><span class="sxs-lookup"><span data-stu-id="4ae42-115">Build hello console app.</span></span>

<!-- Images. -->
[13]: ./media/notification-hubs-back-end/notification-hub-create-console-app.png

<!-- URLs. -->
[Začínáme s Notification Hubs]: ../articles/notification-hubs/notification-hubs-windows-store-dotnet-get-started-wns-push-notification.md
[rozhraní REST centra oznámení]: http://msdn.microsoft.com/library/windowsazure/dn223264.aspx
[přidat nabízená oznámení pro Mobile Apps]: ../articles/app-service-mobile/app-service-mobile-windows-store-dotnet-get-started-push.md
[jak toouse centra oznámení z Java/PHP]: ../articles/notification-hubs/notification-hubs-java-push-notification-tutorial.md
[balíčku Microsoft.Azure.Notification Hubs NuGet]: http://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/
