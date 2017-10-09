## <a name="webapi-project"></a>WebAPI projektu
1. V sadě Visual Studio otevřete hello **AppBackend** projekt, který jste vytvořili v hello **upozornění uživatelů** kurzu.
2. V Notifications.cs, nahraďte hello celou **oznámení** se hello následující kód. Být jisti zástupné symboly tooreplace hello připojovacím řetězcem (s úplným přístupem) pro vaše Centrum oznámení a název centra hello. Tyto hodnoty můžete získat z hello [portálu Azure Classic](http://manage.windowsazure.com). Tento modul představuje teď hello jiné zabezpečené oznámení, která bude odeslána. Do dokončení implementace se uloží hello oznámení v databázi. pro jednoduchost v takovém případě jsme je uložit v paměti.
   
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

1. V NotificationsController.cs, nahraďte kód hello uvnitř hello **NotificationsController** definici třídy s hello následující kód. Tato součást implementuje hello zařízení tooretrieve hello oznámení způsob, jak bezpečně a poskytuje i tootrigger způsob (pro účely tohoto kurzu hello) zabezpečené nabízené tooyour zařízení. Všimněte si, že při odesílání centra oznámení toohello hello oznámení, jenom odešleme nezpracovaná oznámení s hello ID hello oznámení (a žádná skutečná zpráva):
   
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


Všimněte si, že hello `Post` metoda teď neodešle oznámení s informační zprávou. Odešle nezpracovaná oznámení, že obsahuje pouze ID oznámení hello a ne všechny citlivého obsahu. Ujistěte se také, zda toocomment hello odeslat operaci hello platforem, pro které nemáte přihlašovací údaje, které jsou nakonfigurované v centru oznámení, jak bude vést k chybám.

1. Nyní nasadíme znovu tuto aplikaci tooan webu Azure v pořadí toomake je přístupná ze všech zařízení. Klikněte pravým tlačítkem na hello **AppBackend** projektu a vyberte **publikovat**.
2. Vyberte web Azure jako váš cíl publikování. Přihlaste se pomocí účtu Azure a vyberte stávajícího nebo nového webu a poznamenejte si hello **cílová adresa URL** vlastnost hello **připojení** kartě. Označujeme toothis adresu URL jako vaše *koncový bod back-end* dál v tomto kurzu. Klikněte na **Publikovat**.

