## <a name="webapi-project"></a><span data-ttu-id="3266b-101">WebAPI projektu</span><span class="sxs-lookup"><span data-stu-id="3266b-101">WebAPI Project</span></span>
1. <span data-ttu-id="3266b-102">V sadě Visual Studio, otevřete **AppBackend** projekt, který jste vytvořili v **upozornění uživatelů** kurzu.</span><span class="sxs-lookup"><span data-stu-id="3266b-102">In Visual Studio, open the **AppBackend** project that you created in the **Notify Users** tutorial.</span></span>
2. <span data-ttu-id="3266b-103">V Notifications.cs, nahraďte celek **oznámení** třídy následujícím kódem.</span><span class="sxs-lookup"><span data-stu-id="3266b-103">In Notifications.cs, replace the whole **Notifications** class with the following code.</span></span> <span data-ttu-id="3266b-104">Ujistěte se, že nahraďte zástupné symboly připojovací řetězec (s úplným přístupem) pro vaše Centrum oznámení a název rozbočovače.</span><span class="sxs-lookup"><span data-stu-id="3266b-104">Be sure to replace the placeholders with your connection string (with full access) for your notification hub, and the hub name.</span></span> <span data-ttu-id="3266b-105">Můžete získat z těchto hodnot [portálu Azure Classic](http://manage.windowsazure.com).</span><span class="sxs-lookup"><span data-stu-id="3266b-105">You can obtain these values from the [Azure Classic Portal](http://manage.windowsazure.com).</span></span> <span data-ttu-id="3266b-106">Tento modul představuje teď jiné zabezpečené oznámení, které se budou odesílat.</span><span class="sxs-lookup"><span data-stu-id="3266b-106">This module now represents the different secure notifications that will be sent.</span></span> <span data-ttu-id="3266b-107">Do dokončení implementace se uloží oznámení v databázi. pro jednoduchost v takovém případě jsme je uložit v paměti.</span><span class="sxs-lookup"><span data-stu-id="3266b-107">In a complete implementation, the notifications will be stored in a database; for simplicity, in this case we store them in memory.</span></span>
   
        public class Notification
        {
            public int Id { get; set; }
            public string Payload { get; set; }
            public bool Read { get; set; }
        }

        public class Notifications
        {
            public static Notifications Instance = new Notifications();

            private List<Notification> notifications = new List<Notification>();

            public NotificationHubClient Hub { get; set; }

            private Notifications() {
                Hub = NotificationHubClient.CreateClientFromConnectionString("{conn string with full access}",     "{hub name}");
            }

            public Notification CreateNotification(string payload)
            {
                var notification = new Notification() {
                Id = notifications.Count,
                Payload = payload,
                Read = false
                };

                notifications.Add(notification);

                return notification;
            }

            public Notification ReadNotification(int id)
            {
                return notifications.ElementAt(id);
            }
        }

1. <span data-ttu-id="3266b-108">V NotificationsController.cs, nahraďte kód uvnitř **NotificationsController** třídy definice následujícím kódem.</span><span class="sxs-lookup"><span data-stu-id="3266b-108">In NotificationsController.cs, replace the code inside the **NotificationsController** class definition with the following code.</span></span> <span data-ttu-id="3266b-109">Tato součást implementuje způsob, jak zařízení bezpečně načíst oznámení a také poskytuje způsob (pro účely tohoto kurzu) k aktivaci zabezpečené oznámení do zařízení.</span><span class="sxs-lookup"><span data-stu-id="3266b-109">This component implements a way for the device to retrieve the notification securely, and also provides a way (for the purposes of this tutorial) to trigger a secure push to your devices.</span></span> <span data-ttu-id="3266b-110">Všimněte si, že při odesílání oznámení do centra oznámení, jenom odešleme nezpracovaná oznámení s ID oznámení (a žádná skutečná zpráva):</span><span class="sxs-lookup"><span data-stu-id="3266b-110">Note that when sending the notification to the notification hub, we only send a raw notification with the ID of the notification (and no actual message):</span></span>
   
       public NotificationsController()
       {
           Notifications.Instance.CreateNotification("This is a secure notification!");
       }
   
       // GET api/notifications/id
       public Notification Get(int id)
       {
           return Notifications.Instance.ReadNotification(id);
       }
   
       public async Task<HttpResponseMessage> Post()
       {
           var secureNotificationInTheBackend = Notifications.Instance.CreateNotification("Secure confirmation.");
           var usernameTag = "username:" + HttpContext.Current.User.Identity.Name;
   
           // windows
           var rawNotificationToBeSent = new Microsoft.Azure.NotificationHubs.WindowsNotification(secureNotificationInTheBackend.Id.ToString(),
                           new Dictionary<string, string> {
                               {"X-WNS-Type", "wns/raw"}
                           });
           await Notifications.Instance.Hub.SendNotificationAsync(rawNotificationToBeSent, usernameTag);
   
           // apns
           await Notifications.Instance.Hub.SendAppleNativeNotificationAsync("{\"aps\": {\"content-available\": 1}, \"secureId\": \"" + secureNotificationInTheBackend.Id.ToString() + "\"}", usernameTag);
   
           // gcm
           await Notifications.Instance.Hub.SendGcmNativeNotificationAsync("{\"data\": {\"secureId\": \"" + secureNotificationInTheBackend.Id.ToString() + "\"}}", usernameTag);

            return Request.CreateResponse(HttpStatusCode.OK);
        }


<span data-ttu-id="3266b-111">Všimněte si, že `Post` metoda teď neodešle oznámení s informační zprávou.</span><span class="sxs-lookup"><span data-stu-id="3266b-111">Note that the `Post` method now does not send a toast notification.</span></span> <span data-ttu-id="3266b-112">Odešle nezpracovaná oznámení, že obsahuje pouze ID oznámení a ne všechny citlivého obsahu.</span><span class="sxs-lookup"><span data-stu-id="3266b-112">It sends a raw notification that contains only the notification ID, and not any sensitive content.</span></span> <span data-ttu-id="3266b-113">Zkontrolujte taky, okomentujte operaci odeslání pro platformy, pro které nemáte přihlašovací údaje, které jsou nakonfigurované v centru oznámení, jak bude vést k chybám.</span><span class="sxs-lookup"><span data-stu-id="3266b-113">Also, make sure to comment the send operation for the platforms for which you do not have credentials configured on your notification hub, as they will result in errors.</span></span>

1. <span data-ttu-id="3266b-114">Nyní jsme bude znovu nasaďte tuto aplikaci na web Azure aby přístupná ze všech zařízení.</span><span class="sxs-lookup"><span data-stu-id="3266b-114">Now we will re-deploy this app to an Azure Website in order to make it accessible from all devices.</span></span> <span data-ttu-id="3266b-115">Klikněte pravým tlačítkem na projekt **AppBackend** a vyberte **Publikovat**.</span><span class="sxs-lookup"><span data-stu-id="3266b-115">Right-click on the **AppBackend** project and select **Publish**.</span></span>
2. <span data-ttu-id="3266b-116">Vyberte web Azure jako váš cíl publikování.</span><span class="sxs-lookup"><span data-stu-id="3266b-116">Select Azure Website as your publish target.</span></span> <span data-ttu-id="3266b-117">Přihlaste se pomocí účtu Azure a vyberte stávajícího nebo nového webu a poznamenejte si **cílová adresa URL** vlastnost **připojení** kartě.</span><span class="sxs-lookup"><span data-stu-id="3266b-117">Log in with your Azure account and select an existing or new Website, and make a note of the **destination URL** property in the **Connection** tab.</span></span> <span data-ttu-id="3266b-118">Na tuto adresu URL budeme odkazovat jako na *koncový bod back-endu* později v tomto kurzu.</span><span class="sxs-lookup"><span data-stu-id="3266b-118">We will refer to this URL as your *backend endpoint* later in this tutorial.</span></span> <span data-ttu-id="3266b-119">Klikněte na **Publikovat**.</span><span class="sxs-lookup"><span data-stu-id="3266b-119">Click **Publish**.</span></span>

