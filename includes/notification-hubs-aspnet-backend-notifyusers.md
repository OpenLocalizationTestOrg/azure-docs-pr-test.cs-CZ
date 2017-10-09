## <a name="create-hello-webapi-project"></a>Vytvoření hello WebAPI projektu
Vytvoří se nový back-end ASP.NET WebAPI v hello oddíly, které následují a bude mít tři hlavní účely:

1. **Ověřování klientů**: obslužné rutiny zpráv přidá novější požadavky klientů tooauthenticate a přidružení hello uživatele s hello požadavku.
2. **Registrace oznámení klienta**: později budete přidávat nové registrace řadiče toohandle klienta zařízení tooreceive oznámení. Hello jméno ověřeného uživatele, se automaticky přidají toohello registrace jako [značky](https://msdn.microsoft.com/library/azure/dn530749.aspx).
3. **Odesílání oznámení tooClients**: později, přidejte také řadiče tooprovide způsob, jak uživatele tootrigger toodevices zabezpečení nabízené a klienty přidružené k hello značky. 

Hello následující kroky ukazují, jak toocreate hello nový back-end ASP.NET WebAPI: 

> [!IMPORTANT]
> Pokud používáte Visual Studio 2015 nebo starší, před zahájením tohoto kurzu, Zkontrolujte prosím, že jste nainstalovali nejnovější verzi hello hello Správce balíčků NuGet. toocheck, spuštění sady Visual Studio. Z hello **nástroje** nabídky, klikněte na tlačítko **rozšíření a aktualizace**. Vyhledejte **Správce balíčků NuGet** pro vaši verzi sady Visual Studio a ujistěte se, že máte nejnovější verzi hello. Pokud ne, odinstalujte a pak znovu nainstalujte hello Správce balíčků NuGet.
> 
> ![][B4]
> 
> [!NOTE]
> Ujistěte se, že máte nainstalovanou hello Visual Studio [Azure SDK](https://azure.microsoft.com/downloads/) pro nasazení webu.
> 
> 

1. Spusťte sadu Visual Studio nebo Visual Studio Express. Klikněte na tlačítko **Průzkumníka serveru** a přihlaste se tooyour účet Azure. Visual Studio bude nutné vás přihlásit toocreate hello webu prostředky na vašem účtu.
2. V sadě Visual Studio, klikněte na tlačítko **soubor**, pak klikněte na tlačítko **nový**, pak **projektu**, rozbalte položku **šablony**, **Visual C#**, pak klikněte na tlačítko **webové** a **webové aplikace ASP.NET**, název typu hello **AppBackend**a potom klikněte na **OK**. 
   
    ![][B1]
3. V hello **nový projekt ASP.NET** dialogové okno, klikněte na tlačítko **webového rozhraní API**, pak klikněte na tlačítko **OK**.
   
    ![][B2]
4. V hello **konfigurace webové aplikace Azure Microsoft** dialogovém okně, vyberte předplatné a **plán služby App Service** jste už vytvořili. Můžete také **vytvořit nový plán služby app** a vytvořit z dialogového okna hello. Pro účely tohoto kurzu nepotřebujete databázi. Jakmile vyberete váš plán služby app service, klikněte na tlačítko **OK** toocreate hello projektu.
   
    ![][B5]

## <a name="authenticating-clients-toohello-webapi-backend"></a>Ověřování klientů toohello WebAPI back-end
V této části vytvoříte novou třídu obslužné rutiny zpráv s názvem **AuthenticationTestHandler** pro nový back-end hello. Tato třída je odvozený od [DelegatingHandler](https://msdn.microsoft.com/library/system.net.http.delegatinghandler.aspx) a přidány jako obslužné rutiny zpráv tak může zpracovat všechny žádosti přicházející do hello back-end. 

1. V Průzkumníku řešení klikněte pravým tlačítkem na hello **AppBackend** projektu, klikněte na tlačítko **přidat**, pak klikněte na tlačítko **třída**. Pojmenujte novou třídu hello **AuthenticationTestHandler.cs**a klikněte na tlačítko **přidat** toogenerate hello třídy. Tato třída bude použité tooauthenticate uživatele, kteří používají *základní ověřování* pro jednoduchost. Nezapomeňte, že vaše aplikace může používat jakékoli schéma ověřování.
2. V AuthenticationTestHandler.cs, přidejte následující hello `using` příkazy:
   
        using System.Net.Http;
        using System.Threading;
        using System.Security.Principal;
        using System.Net;
        using System.Text;
        using System.Threading.Tasks;

3. V AuthenticationTestHandler.cs, nahraďte hello `AuthenticationTestHandler` definici třídy s hello následující kód. 
   
    Tato obslužná rutina ověření požadavku hello při hello následující tři podmínky jsou splněny všechny:
   
   * Hello požadavek zahrnuty *autorizace* záhlaví. 
   * žádost o Hello používá *základní* ověřování. 
   * řetězec názvu Hello uživatele a heslo řetězec hello jsou hello jednoho řetězce.
     
     V opačném hello žádost odmítnuta. Toto není správný přístup k ověřování a autorizaci. Je to jenom velmi jednoduchý příklad pro účely tohoto kurzu.
     
     Pokud zpráva požadavku hello ověří a autorizuje podle hello `AuthenticationTestHandler`, hello základní ověřování uživatele bude připojené toohello aktuální požadavek na hello [HttpContext](https://msdn.microsoft.com/library/system.web.httpcontext.current.aspx). Informace o uživateli v hello HttpContext budou používat jiný řadič (RegisterController) novější tooadd [značka](https://msdn.microsoft.com/library/azure/dn530749.aspx) toohello žádost o registraci oznámení.
     
       public class AuthenticationTestHandler : DelegatingHandler   {       protected override Task<HttpResponseMessage> SendAsync(       HttpRequestMessage request, CancellationToken cancellationToken)       {           var authorizationHeader = request.Headers.GetValues("Authorization").First();
     
               if (authorizationHeader != null && authorizationHeader
                   .StartsWith("Basic ", StringComparison.InvariantCultureIgnoreCase))
               {
                   string authorizationUserAndPwdBase64 =
                       authorizationHeader.Substring("Basic ".Length);
                   string authorizationUserAndPwd = Encoding.Default
                       .GetString(Convert.FromBase64String(authorizationUserAndPwdBase64));
                   string user = authorizationUserAndPwd.Split(':')[0];
                   string password = authorizationUserAndPwd.Split(':')[1];
     
                   if (verifyUserAndPwd(user, password))
                   {
                       // Attach hello new principal object toohello current HttpContext object
                       HttpContext.Current.User =
                           new GenericPrincipal(new GenericIdentity(user), new string[0]);
                       System.Threading.Thread.CurrentPrincipal =
                           System.Web.HttpContext.Current.User;
                   }
                   else return Unauthorized();
               }
               else return Unauthorized();
     
               return base.SendAsync(request, cancellationToken);
           }
     
           private bool verifyUserAndPwd(string user, string password)
           {
               // This is not a real authentication scheme.
               return user == password;
           }
     
           private Task<HttpResponseMessage> Unauthorized()
           {
               var response = new HttpResponseMessage(HttpStatusCode.Forbidden);
               var tsc = new TaskCompletionSource<HttpResponseMessage>();
               tsc.SetResult(response);
               return tsc.Task;
           }
       }
     
     > [!NOTE]
     > **Poznámka k zabezpečení**: hello `AuthenticationTestHandler` třída neposkytuje true ověřování. Je použít jenom toomimic základní ověřování a není zabezpečený. Ve svých produkčních aplikacích a službách musíte implementovat mechanismus zabezpečeného ověřování.                
     > 
     > 
4. Přidejte následující kód na konci hello hello hello `Register` metoda v hello **App_Start/WebApiConfig.cs** třídu obslužné rutiny zpráv hello tooregister:
   
        config.MessageHandlers.Add(new AuthenticationTestHandler());
5. Uložte provedené změny.

## <a name="registering-for-notifications-using-hello-webapi-backend"></a>Registrace pro oznámení prostřednictvím hello WebAPI back-end
V této části přidáme toohandle WebAPI back-end sady nové řadiče toohello požadavky tooregister uživatele a zařízení pro oznámení pomocí hello klientské knihovny pro centra oznámení. Přidá značku uživatele pro hello uživatele, který byl ověřen a připojené toohello HttpContext podle hello Hello řadiče `AuthenticationTestHandler`. značka Hello bude obsahovat řetězec formátu hello, `"username:<actual username>"`.

1. V Průzkumníku řešení klikněte pravým tlačítkem na hello **AppBackend** projektu a pak klikněte na **spravovat balíčky NuGet**.
2. Na levé straně hello, klikněte na tlačítko **Online**a vyhledejte **Microsoft.Azure.NotificationHubs** v hello **vyhledávání** pole.
3. V seznamu výsledků hello, klikněte na tlačítko **Microsoft Azure Notification Hubs**a potom klikněte na **nainstalovat**. Dokončení instalace hello a potom zavřete okno Správce balíčků NuGet hello.
   
    Tento postup přidá odkaz toohello SDK centra oznámení Azure pomocí hello <a href="http://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/">balíčku Microsoft.Azure.Notification Hubs NuGet</a>.
4. Nyní vytvoříme nový soubor třídy, který představuje připojení hello s oznámení toosend použít Centrum oznámení. V hello Průzkumníku řešení klikněte pravým tlačítkem na hello **modely** složku, klikněte na tlačítko **přidat**, pak klikněte na tlačítko **třída**. Pojmenujte novou třídu hello **Notifications.cs**, pak klikněte na tlačítko **přidat** toogenerate hello třídy. 
   
    ![][B6]
5. V Notifications.cs, přidejte následující hello `using` příkaz hello horní části souboru hello:
   
        using Microsoft.Azure.NotificationHubs;
6. Nahraďte hello `Notifications` definici s hello následující třídy a ujistěte se, že tooreplace hello dva zástupné symboly hello připojovací řetězec (s úplným přístupem) pro vaše Centrum oznámení a hello název centra (k dispozici na [portálu Azure Classic ](http://manage.windowsazure.com)):
   
        public class Notifications
        {
            public static Notifications Instance = new Notifications();
   
            public NotificationHubClient Hub { get; set; }
   
            private Notifications() {
                Hub = NotificationHubClient.CreateClientFromConnectionString("<your hub's DefaultFullSharedAccessSignature>", 
                                                                             "<hub name>");
            }
        }
7. Dále vytvoříme nový kontroler **RegisterController**. V Průzkumníku řešení klikněte pravým tlačítkem na hello **řadiče** složku, pak klikněte na tlačítko **přidat**, pak klikněte na tlačítko **řadič**. Klikněte na tlačítko hello **webové rozhraní API 2 řadiče – prázdný** položky a pak klikněte na **přidat**. Pojmenujte novou třídu hello **RegisterController**a potom klikněte na **přidat** znovu toogenerate hello řadiče.
   
    ![][B7]
   
    ![][B8]
8. V RegisterController.cs, přidejte následující hello `using` příkazy:
   
        using Microsoft.Azure.NotificationHubs;
        using Microsoft.Azure.NotificationHubs.Messaging;
        using AppBackend.Models;
        using System.Threading.Tasks;
        using System.Web;
9. Přidejte následující kód do hello hello `RegisterController` definici třídy. Všimněte si, že se v tomto kódu, přidáme, aby byl značku uživatele pro uživatele hello, to je připojen toohello položka HttpContext. Hello uživatele došlo k ověření a připojené toohello HttpContext filtrem hello zprávu jsme přidali, `AuthenticationTestHandler`. Můžete také přidat tooverify volitelné kontroly, které hello uživatel má oprávnění tooregister pro hello požadované značky.
   
        private NotificationHubClient hub;
   
        public RegisterController()
        {
            hub = Notifications.Instance.Hub;
        }
   
        public class DeviceRegistration
        {
            public string Platform { get; set; }
            public string Handle { get; set; }
            public string[] Tags { get; set; }
        }
   
        // POST api/register
        // This creates a registration id
        public async Task<string> Post(string handle = null)
        {
            string newRegistrationId = null;
   
            // make sure there are no existing registrations for this push handle (used for iOS and Android)
            if (handle != null)
            {
                var registrations = await hub.GetRegistrationsByChannelAsync(handle, 100);
   
                foreach (RegistrationDescription registration in registrations)
                {
                    if (newRegistrationId == null)
                    {
                        newRegistrationId = registration.RegistrationId;
                    }
                    else
                    {
                        await hub.DeleteRegistrationAsync(registration);
                    }
                }
            }
   
            if (newRegistrationId == null) 
                newRegistrationId = await hub.CreateRegistrationIdAsync();
   
            return newRegistrationId;
        }
   
        // PUT api/register/5
        // This creates or updates a registration (with provided channelURI) at hello specified id
        public async Task<HttpResponseMessage> Put(string id, DeviceRegistration deviceUpdate)
        {
            RegistrationDescription registration = null;
            switch (deviceUpdate.Platform)
            {
                case "mpns":
                    registration = new MpnsRegistrationDescription(deviceUpdate.Handle);
                    break;
                case "wns":
                    registration = new WindowsRegistrationDescription(deviceUpdate.Handle);
                    break;
                case "apns":
                    registration = new AppleRegistrationDescription(deviceUpdate.Handle);
                    break;
                case "gcm":
                    registration = new GcmRegistrationDescription(deviceUpdate.Handle);
                    break;
                default:
                    throw new HttpResponseException(HttpStatusCode.BadRequest);
            }
   
            registration.RegistrationId = id;
            var username = HttpContext.Current.User.Identity.Name;
   
            // add check if user is allowed tooadd these tags
            registration.Tags = new HashSet<string>(deviceUpdate.Tags);
            registration.Tags.Add("username:" + username);
   
            try
            {
                await hub.CreateOrUpdateRegistrationAsync(registration);
            }
            catch (MessagingException e)
            {
                ReturnGoneIfHubResponseIsGone(e);
            }
   
            return Request.CreateResponse(HttpStatusCode.OK);
        }
   
        // DELETE api/register/5
        public async Task<HttpResponseMessage> Delete(string id)
        {
            await hub.DeleteRegistrationAsync(id);
            return Request.CreateResponse(HttpStatusCode.OK);
        }
   
        private static void ReturnGoneIfHubResponseIsGone(MessagingException e)
        {
            var webex = e.InnerException as WebException;
            if (webex.Status == WebExceptionStatus.ProtocolError)
            {
                var response = (HttpWebResponse)webex.Response;
                if (response.StatusCode == HttpStatusCode.Gone)
                    throw new HttpRequestException(HttpStatusCode.Gone.ToString());
            }
        }
10. Uložte provedené změny.

## <a name="sending-notifications-from-hello-webapi-backend"></a>Odesílání oznámení z hello WebAPI back-end
V této části přidáte nový řadič, který zveřejňuje způsob, jak klientské zařízení toosend oznámení podle značky hello uživatelské jméno pomocí knihovny správy služby centra oznámení Azure v hello ASP.NET WebAPI back-end.

1. Vytvořte další nový kontroler **NotificationsController**. Vytvořit hello stejně jako jste vytvořili hello **RegisterController** v předchozí části hello.
2. V NotificationsController.cs, přidejte následující hello `using` příkazy:
   
        using AppBackend.Models;
        using System.Threading.Tasks;
        using System.Web;
3. Přidejte následující metodu toohello hello **NotificationsController** třídy.
   
    Tento kód odeslat typu oznámení založené na hello platformy oznámení služby (PNS) `pns` parametr. Hello hodnotu `to_tag` je použité tooset hello *uživatelské jméno* značky na uvítací zprávu. Tato značka musí odpovídat značce uživatelského jména aktivní registrace k centru událostí. Hello oznámení je načtený z textu hello hello požadavku POST a formátovat cílový hello systém PNS. 
   
    V závislosti na hello platformy oznámení služby (PNS), podporovaných zařízení používat tooreceive oznámení podporují různé oznámení pomocí různých formátech. Například na zařízeních s Windows byste mohli použít [informační zprávu pomocí Služby nabízených oznámení Windows](https://msdn.microsoft.com/library/windows/apps/br230849.aspx), kterou ostatní systémy oznámení platformy přímo nepodporují. Aby váš back-end potřebovat tooformat hello oznámení do podporovaných oznámení pro hello systému PNS zařízení plánujete toosupport. Potom pomocí rozhraní API odpovídající odesílání hello na hello [NotificationHubClient – třída](https://msdn.microsoft.com/library/azure/microsoft.azure.notificationhubs.notificationhubclient_methods.aspx)
   
        public async Task<HttpResponseMessage> Post(string pns, [FromBody]string message, string to_tag)
        {
            var user = HttpContext.Current.User.Identity.Name;
            string[] userTag = new string[2];
            userTag[0] = "username:" + to_tag;
            userTag[1] = "from:" + user;
   
            Microsoft.Azure.NotificationHubs.NotificationOutcome outcome = null;
            HttpStatusCode ret = HttpStatusCode.InternalServerError;
   
            switch (pns.ToLower())
            {
                case "wns":
                    // Windows 8.1 / Windows Phone 8.1
                    var toast = @"<toast><visual><binding template=""ToastText01""><text id=""1"">" + 
                                "From " + user + ": " + message + "</text></binding></visual></toast>";
                    outcome = await Notifications.Instance.Hub.SendWindowsNativeNotificationAsync(toast, userTag);
                    break;
                case "apns":
                    // iOS
                    var alert = "{\"aps\":{\"alert\":\"" + "From " + user + ": " + message + "\"}}";
                    outcome = await Notifications.Instance.Hub.SendAppleNativeNotificationAsync(alert, userTag);
                    break;
                case "gcm":
                    // Android
                    var notif = "{ \"data\" : {\"message\":\"" + "From " + user + ": " + message + "\"}}";
                    outcome = await Notifications.Instance.Hub.SendGcmNativeNotificationAsync(notif, userTag);
                    break;
            }
   
            if (outcome != null)
            {
                if (!((outcome.State == Microsoft.Azure.NotificationHubs.NotificationOutcomeState.Abandoned) ||
                    (outcome.State == Microsoft.Azure.NotificationHubs.NotificationOutcomeState.Unknown)))
                {
                    ret = HttpStatusCode.OK;
                }
            }
   
            return Request.CreateResponse(ret);
        }
4. Stiskněte klávesu **F5** toorun hello aplikace a tooensure hello přesnost své dosavadní práce. Hello aplikace by měla spustit webový prohlížeč a zobrazit domovskou stránku ASP.NET hello. 

## <a name="publish-hello-new-webapi-backend"></a>Publikování hello nový back-end WebAPI
1. Nyní nasadíme tuto aplikaci tooan webu Azure v pořadí toomake je přístupná ze všech zařízení. Klikněte pravým tlačítkem na hello **AppBackend** projektu a vyberte **publikovat**.
2. Jako cíl publikování vyberte **Microsoft Azure App Service** a klikněte na **Publikovat**. Otevře se dialog hello vytvořit službu App Service, který vám pomůže vytvořit všechny hello potřebné prostředky Azure toorun hello webové aplikace ASP.NET v Azure.

    ![][B15]
3. V hello **vytvořit službu App Service** dialogovém okně, vyberte svůj účet Azure. Klikněte na **Změnit typ** a vyberte **Webová aplikace**. Zachovat hello **název webové aplikace** dané a vyberte možnost hello **předplatné**, **skupiny prostředků**, a **plán služby App Service**.  Klikněte na možnost **Vytvořit**.

4. Poznamenejte si hello **adresa URL webu** vlastnost hello **Souhrn** části. Označujeme toothis adresu URL jako vaše *koncový bod back-end* dál v tomto kurzu. Klikněte na **Publikovat**.

5. Po dokončení Průvodce hello, tato možnost publikuje hello ASP.NET webové aplikace tooAzure a pak spustí hello aplikace v hello výchozí prohlížeč.  Vaši aplikaci bude možné zobrazit ve službě Azure App Service.

Adresa URL Hello používá název hello webové aplikace, který jste zadali dříve, s http://<app_name>.azurewebsites.net formátu hello.

[B1]: ./media/notification-hubs-aspnet-backend-notifyusers/notification-hubs-secure-push1.png
[B2]: ./media/notification-hubs-aspnet-backend-notifyusers/notification-hubs-secure-push2.png
[B3]: ./media/notification-hubs-aspnet-backend-notifyusers/notification-hubs-secure-push3.png
[B4]: ./media/notification-hubs-aspnet-backend-notifyusers/notification-hubs-secure-push4.png
[B5]: ./media/notification-hubs-aspnet-backend-notifyusers/notification-hubs-secure-push5.png
[B6]: ./media/notification-hubs-aspnet-backend-notifyusers/notification-hubs-secure-push6.png
[B7]: ./media/notification-hubs-aspnet-backend-notifyusers/notification-hubs-secure-push7.png
[B8]: ./media/notification-hubs-aspnet-backend-notifyusers/notification-hubs-secure-push8.png
[B14]: ./media/notification-hubs-aspnet-backend-notifyusers/notification-hubs-secure-push14.png
[B15]: ./media/notification-hubs-aspnet-backend-notifyusers/publish-to-app-service.png
[B16]: ./media/notification-hubs-aspnet-backend-notifyusers/notification-hubs-notify-users16.PNG
[B18]: ./media/notification-hubs-aspnet-backend-notifyusers/notification-hubs-notify-users18.PNG
