## <a name="webapi-project"></a><span data-ttu-id="ae077-101">WebAPI projektu</span><span class="sxs-lookup"><span data-stu-id="ae077-101">WebAPI Project</span></span>
1. <span data-ttu-id="ae077-102">V sadě Visual Studio otevřete hello **AppBackend** projekt, který jste vytvořili v hello **upozornění uživatelů** kurzu.</span><span class="sxs-lookup"><span data-stu-id="ae077-102">In Visual Studio, open hello **AppBackend** project that you created in hello **Notify Users** tutorial.</span></span>
2. <span data-ttu-id="ae077-103">V Notifications.cs, nahraďte hello celou **oznámení** se hello následující kód.</span><span class="sxs-lookup"><span data-stu-id="ae077-103">In Notifications.cs, replace hello whole **Notifications** class with hello following code.</span></span> <span data-ttu-id="ae077-104">Být jisti zástupné symboly tooreplace hello připojovacím řetězcem (s úplným přístupem) pro vaše Centrum oznámení a název centra hello.</span><span class="sxs-lookup"><span data-stu-id="ae077-104">Be sure tooreplace hello placeholders with your connection string (with full access) for your notification hub, and hello hub name.</span></span> <span data-ttu-id="ae077-105">Tyto hodnoty můžete získat z hello [portálu Azure Classic](http://manage.windowsazure.com).</span><span class="sxs-lookup"><span data-stu-id="ae077-105">You can obtain these values from hello [Azure Classic Portal](http://manage.windowsazure.com).</span></span> <span data-ttu-id="ae077-106">Tento modul představuje teď hello jiné zabezpečené oznámení, která bude odeslána.</span><span class="sxs-lookup"><span data-stu-id="ae077-106">This module now represents hello different secure notifications that will be sent.</span></span> <span data-ttu-id="ae077-107">Do dokončení implementace se uloží hello oznámení v databázi. pro jednoduchost v takovém případě jsme je uložit v paměti.</span><span class="sxs-lookup"><span data-stu-id="ae077-107">In a complete implementation, hello notifications will be stored in a database; for simplicity, in this case we store them in memory.</span></span>
   
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

1. <span data-ttu-id="ae077-108">V NotificationsController.cs, nahraďte kód hello uvnitř hello **NotificationsController** definici třídy s hello následující kód.</span><span class="sxs-lookup"><span data-stu-id="ae077-108">In NotificationsController.cs, replace hello code inside hello **NotificationsController** class definition with hello following code.</span></span> <span data-ttu-id="ae077-109">Tato součást implementuje hello zařízení tooretrieve hello oznámení způsob, jak bezpečně a poskytuje i tootrigger způsob (pro účely tohoto kurzu hello) zabezpečené nabízené tooyour zařízení.</span><span class="sxs-lookup"><span data-stu-id="ae077-109">This component implements a way for hello device tooretrieve hello notification securely, and also provides a way (for hello purposes of this tutorial) tootrigger a secure push tooyour devices.</span></span> <span data-ttu-id="ae077-110">Všimněte si, že při odesílání centra oznámení toohello hello oznámení, jenom odešleme nezpracovaná oznámení s hello ID hello oznámení (a žádná skutečná zpráva):</span><span class="sxs-lookup"><span data-stu-id="ae077-110">Note that when sending hello notification toohello notification hub, we only send a raw notification with hello ID of hello notification (and no actual message):</span></span>
   
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


<span data-ttu-id="ae077-111">Všimněte si, že hello `Post` metoda teď neodešle oznámení s informační zprávou.</span><span class="sxs-lookup"><span data-stu-id="ae077-111">Note that hello `Post` method now does not send a toast notification.</span></span> <span data-ttu-id="ae077-112">Odešle nezpracovaná oznámení, že obsahuje pouze ID oznámení hello a ne všechny citlivého obsahu.</span><span class="sxs-lookup"><span data-stu-id="ae077-112">It sends a raw notification that contains only hello notification ID, and not any sensitive content.</span></span> <span data-ttu-id="ae077-113">Ujistěte se také, zda toocomment hello odeslat operaci hello platforem, pro které nemáte přihlašovací údaje, které jsou nakonfigurované v centru oznámení, jak bude vést k chybám.</span><span class="sxs-lookup"><span data-stu-id="ae077-113">Also, make sure toocomment hello send operation for hello platforms for which you do not have credentials configured on your notification hub, as they will result in errors.</span></span>

1. <span data-ttu-id="ae077-114">Nyní nasadíme znovu tuto aplikaci tooan webu Azure v pořadí toomake je přístupná ze všech zařízení.</span><span class="sxs-lookup"><span data-stu-id="ae077-114">Now we will re-deploy this app tooan Azure Website in order toomake it accessible from all devices.</span></span> <span data-ttu-id="ae077-115">Klikněte pravým tlačítkem na hello **AppBackend** projektu a vyberte **publikovat**.</span><span class="sxs-lookup"><span data-stu-id="ae077-115">Right-click on hello **AppBackend** project and select **Publish**.</span></span>
2. <span data-ttu-id="ae077-116">Vyberte web Azure jako váš cíl publikování.</span><span class="sxs-lookup"><span data-stu-id="ae077-116">Select Azure Website as your publish target.</span></span> <span data-ttu-id="ae077-117">Přihlaste se pomocí účtu Azure a vyberte stávajícího nebo nového webu a poznamenejte si hello **cílová adresa URL** vlastnost hello **připojení** kartě. Označujeme toothis adresu URL jako vaše *koncový bod back-end* dál v tomto kurzu.</span><span class="sxs-lookup"><span data-stu-id="ae077-117">Log in with your Azure account and select an existing or new Website, and make a note of hello **destination URL** property in hello **Connection** tab. We will refer toothis URL as your *backend endpoint* later in this tutorial.</span></span> <span data-ttu-id="ae077-118">Klikněte na **Publikovat**.</span><span class="sxs-lookup"><span data-stu-id="ae077-118">Click **Publish**.</span></span>

