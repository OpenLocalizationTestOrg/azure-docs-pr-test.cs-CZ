## <a name="create-hello-webapi-project"></a><span data-ttu-id="3ac7a-101">Vytvoření hello WebAPI projektu</span><span class="sxs-lookup"><span data-stu-id="3ac7a-101">Create hello WebAPI Project</span></span>
<span data-ttu-id="3ac7a-102">Vytvoří se nový back-end ASP.NET WebAPI v hello oddíly, které následují a bude mít tři hlavní účely:</span><span class="sxs-lookup"><span data-stu-id="3ac7a-102">A new ASP.NET WebAPI backend will be created in hello sections that follow and it will have three main purposes:</span></span>

1. <span data-ttu-id="3ac7a-103">**Ověřování klientů**: obslužné rutiny zpráv přidá novější požadavky klientů tooauthenticate a přidružení hello uživatele s hello požadavku.</span><span class="sxs-lookup"><span data-stu-id="3ac7a-103">**Authenticating Clients**: A message handler will be added later tooauthenticate client requests and associate hello user with hello request.</span></span>
2. <span data-ttu-id="3ac7a-104">**Registrace oznámení klienta**: později budete přidávat nové registrace řadiče toohandle klienta zařízení tooreceive oznámení.</span><span class="sxs-lookup"><span data-stu-id="3ac7a-104">**Client Notification Registrations**: Later, you will add a controller toohandle new registrations for a client device tooreceive notifications.</span></span> <span data-ttu-id="3ac7a-105">Hello jméno ověřeného uživatele, se automaticky přidají toohello registrace jako [značky](https://msdn.microsoft.com/library/azure/dn530749.aspx).</span><span class="sxs-lookup"><span data-stu-id="3ac7a-105">hello authenticated user name will automatically be added toohello registration as a [tag](https://msdn.microsoft.com/library/azure/dn530749.aspx).</span></span>
3. <span data-ttu-id="3ac7a-106">**Odesílání oznámení tooClients**: později, přidejte také řadiče tooprovide způsob, jak uživatele tootrigger toodevices zabezpečení nabízené a klienty přidružené k hello značky.</span><span class="sxs-lookup"><span data-stu-id="3ac7a-106">**Sending Notifications tooClients**: Later, you will also add a controller tooprovide a way for a user tootrigger a secure push toodevices and clients associated with hello tag.</span></span> 

<span data-ttu-id="3ac7a-107">Hello následující kroky ukazují, jak toocreate hello nový back-end ASP.NET WebAPI:</span><span class="sxs-lookup"><span data-stu-id="3ac7a-107">hello following steps show how toocreate hello new ASP.NET WebAPI backend:</span></span> 

> [!IMPORTANT]
> <span data-ttu-id="3ac7a-108">Pokud používáte Visual Studio 2015 nebo starší, před zahájením tohoto kurzu, Zkontrolujte prosím, že jste nainstalovali nejnovější verzi hello hello Správce balíčků NuGet.</span><span class="sxs-lookup"><span data-stu-id="3ac7a-108">If you are using Visual Studio 2015 or earlier, before starting this tutorial, please ensure that you have installed hello latest version of hello NuGet Package Manager.</span></span> <span data-ttu-id="3ac7a-109">toocheck, spuštění sady Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="3ac7a-109">toocheck, start Visual Studio.</span></span> <span data-ttu-id="3ac7a-110">Z hello **nástroje** nabídky, klikněte na tlačítko **rozšíření a aktualizace**.</span><span class="sxs-lookup"><span data-stu-id="3ac7a-110">From hello **Tools** menu, click **Extensions and Updates**.</span></span> <span data-ttu-id="3ac7a-111">Vyhledejte **Správce balíčků NuGet** pro vaši verzi sady Visual Studio a ujistěte se, že máte nejnovější verzi hello.</span><span class="sxs-lookup"><span data-stu-id="3ac7a-111">Search for **NuGet Package Manager** for your version of Visual Studio, and make sure you have hello latest version.</span></span> <span data-ttu-id="3ac7a-112">Pokud ne, odinstalujte a pak znovu nainstalujte hello Správce balíčků NuGet.</span><span class="sxs-lookup"><span data-stu-id="3ac7a-112">If not, please uninstall, then reinstall hello NuGet Package Manager.</span></span>
> 
> ![][B4]
> 
> [!NOTE]
> <span data-ttu-id="3ac7a-113">Ujistěte se, že máte nainstalovanou hello Visual Studio [Azure SDK](https://azure.microsoft.com/downloads/) pro nasazení webu.</span><span class="sxs-lookup"><span data-stu-id="3ac7a-113">Make sure you have installed hello Visual Studio [Azure SDK](https://azure.microsoft.com/downloads/) for website deployment.</span></span>
> 
> 

1. <span data-ttu-id="3ac7a-114">Spusťte sadu Visual Studio nebo Visual Studio Express.</span><span class="sxs-lookup"><span data-stu-id="3ac7a-114">Start Visual Studio or Visual Studio Express.</span></span> <span data-ttu-id="3ac7a-115">Klikněte na tlačítko **Průzkumníka serveru** a přihlaste se tooyour účet Azure.</span><span class="sxs-lookup"><span data-stu-id="3ac7a-115">Click **Server Explorer** and sign in tooyour Azure account.</span></span> <span data-ttu-id="3ac7a-116">Visual Studio bude nutné vás přihlásit toocreate hello webu prostředky na vašem účtu.</span><span class="sxs-lookup"><span data-stu-id="3ac7a-116">Visual Studio will need you signed in toocreate hello web site resources on your account.</span></span>
2. <span data-ttu-id="3ac7a-117">V sadě Visual Studio, klikněte na tlačítko **soubor**, pak klikněte na tlačítko **nový**, pak **projektu**, rozbalte položku **šablony**, **Visual C#**, pak klikněte na tlačítko **webové** a **webové aplikace ASP.NET**, název typu hello **AppBackend**a potom klikněte na **OK**.</span><span class="sxs-lookup"><span data-stu-id="3ac7a-117">In Visual Studio, click **File**, then click **New**, then **Project**, expand **Templates**, **Visual C#**, then click **Web** and **ASP.NET Web Application**, type hello name **AppBackend**, and then click **OK**.</span></span> 
   
    ![][B1]
3. <span data-ttu-id="3ac7a-118">V hello **nový projekt ASP.NET** dialogové okno, klikněte na tlačítko **webového rozhraní API**, pak klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="3ac7a-118">In hello **New ASP.NET Project** dialog, click **Web API**, then click **OK**.</span></span>
   
    ![][B2]
4. <span data-ttu-id="3ac7a-119">V hello **konfigurace webové aplikace Azure Microsoft** dialogovém okně, vyberte předplatné a **plán služby App Service** jste už vytvořili.</span><span class="sxs-lookup"><span data-stu-id="3ac7a-119">In hello **Configure Microsoft Azure Web App** dialog, choose a subscription, and an **App Service plan** you have already created.</span></span> <span data-ttu-id="3ac7a-120">Můžete také **vytvořit nový plán služby app** a vytvořit z dialogového okna hello.</span><span class="sxs-lookup"><span data-stu-id="3ac7a-120">You can also choose **Create a new app service plan** and create one from hello dialog.</span></span> <span data-ttu-id="3ac7a-121">Pro účely tohoto kurzu nepotřebujete databázi.</span><span class="sxs-lookup"><span data-stu-id="3ac7a-121">You do not need a database for this tutorial.</span></span> <span data-ttu-id="3ac7a-122">Jakmile vyberete váš plán služby app service, klikněte na tlačítko **OK** toocreate hello projektu.</span><span class="sxs-lookup"><span data-stu-id="3ac7a-122">Once you have selected your app service plan, click **OK** toocreate hello project.</span></span>
   
    ![][B5]

## <a name="authenticating-clients-toohello-webapi-backend"></a><span data-ttu-id="3ac7a-123">Ověřování klientů toohello WebAPI back-end</span><span class="sxs-lookup"><span data-stu-id="3ac7a-123">Authenticating Clients toohello WebAPI Backend</span></span>
<span data-ttu-id="3ac7a-124">V této části vytvoříte novou třídu obslužné rutiny zpráv s názvem **AuthenticationTestHandler** pro nový back-end hello.</span><span class="sxs-lookup"><span data-stu-id="3ac7a-124">In this section, you will create a new message handler class named **AuthenticationTestHandler** for hello new backend.</span></span> <span data-ttu-id="3ac7a-125">Tato třída je odvozený od [DelegatingHandler](https://msdn.microsoft.com/library/system.net.http.delegatinghandler.aspx) a přidány jako obslužné rutiny zpráv tak může zpracovat všechny žádosti přicházející do hello back-end.</span><span class="sxs-lookup"><span data-stu-id="3ac7a-125">This class is derived from [DelegatingHandler](https://msdn.microsoft.com/library/system.net.http.delegatinghandler.aspx) and added as a message handler so it can process all requests coming into hello backend.</span></span> 

1. <span data-ttu-id="3ac7a-126">V Průzkumníku řešení klikněte pravým tlačítkem na hello **AppBackend** projektu, klikněte na tlačítko **přidat**, pak klikněte na tlačítko **třída**.</span><span class="sxs-lookup"><span data-stu-id="3ac7a-126">In Solution Explorer, right-click hello **AppBackend** project, click **Add**, then click **Class**.</span></span> <span data-ttu-id="3ac7a-127">Pojmenujte novou třídu hello **AuthenticationTestHandler.cs**a klikněte na tlačítko **přidat** toogenerate hello třídy.</span><span class="sxs-lookup"><span data-stu-id="3ac7a-127">Name hello new class **AuthenticationTestHandler.cs**, and click **Add** toogenerate hello class.</span></span> <span data-ttu-id="3ac7a-128">Tato třída bude použité tooauthenticate uživatele, kteří používají *základní ověřování* pro jednoduchost.</span><span class="sxs-lookup"><span data-stu-id="3ac7a-128">This class will be used tooauthenticate users using *Basic Authentication* for simplicity.</span></span> <span data-ttu-id="3ac7a-129">Nezapomeňte, že vaše aplikace může používat jakékoli schéma ověřování.</span><span class="sxs-lookup"><span data-stu-id="3ac7a-129">Note that your app can use any authentication scheme.</span></span>
2. <span data-ttu-id="3ac7a-130">V AuthenticationTestHandler.cs, přidejte následující hello `using` příkazy:</span><span class="sxs-lookup"><span data-stu-id="3ac7a-130">In AuthenticationTestHandler.cs, add hello following `using` statements:</span></span>
   
        using System.Net.Http;
        using System.Threading;
        using System.Security.Principal;
        using System.Net;
        using System.Text;
        using System.Threading.Tasks;

3. <span data-ttu-id="3ac7a-131">V AuthenticationTestHandler.cs, nahraďte hello `AuthenticationTestHandler` definici třídy s hello následující kód.</span><span class="sxs-lookup"><span data-stu-id="3ac7a-131">In AuthenticationTestHandler.cs, replacing hello `AuthenticationTestHandler` class definition with hello following code.</span></span> 
   
    <span data-ttu-id="3ac7a-132">Tato obslužná rutina ověření požadavku hello při hello následující tři podmínky jsou splněny všechny:</span><span class="sxs-lookup"><span data-stu-id="3ac7a-132">This handler will authorize hello request when hello following three conditions are all true:</span></span>
   
   * <span data-ttu-id="3ac7a-133">Hello požadavek zahrnuty *autorizace* záhlaví.</span><span class="sxs-lookup"><span data-stu-id="3ac7a-133">hello request included an *Authorization* header.</span></span> 
   * <span data-ttu-id="3ac7a-134">žádost o Hello používá *základní* ověřování.</span><span class="sxs-lookup"><span data-stu-id="3ac7a-134">hello request uses *basic* authentication.</span></span> 
   * <span data-ttu-id="3ac7a-135">řetězec názvu Hello uživatele a heslo řetězec hello jsou hello jednoho řetězce.</span><span class="sxs-lookup"><span data-stu-id="3ac7a-135">hello user name string and hello password string are hello same string.</span></span>
     
     <span data-ttu-id="3ac7a-136">V opačném hello žádost odmítnuta.</span><span class="sxs-lookup"><span data-stu-id="3ac7a-136">Otherwise, hello request will be rejected.</span></span> <span data-ttu-id="3ac7a-137">Toto není správný přístup k ověřování a autorizaci.</span><span class="sxs-lookup"><span data-stu-id="3ac7a-137">This is not a true authentication and authorization approach.</span></span> <span data-ttu-id="3ac7a-138">Je to jenom velmi jednoduchý příklad pro účely tohoto kurzu.</span><span class="sxs-lookup"><span data-stu-id="3ac7a-138">It is just a very simple example for this tutorial.</span></span>
     
     <span data-ttu-id="3ac7a-139">Pokud zpráva požadavku hello ověří a autorizuje podle hello `AuthenticationTestHandler`, hello základní ověřování uživatele bude připojené toohello aktuální požadavek na hello [HttpContext](https://msdn.microsoft.com/library/system.web.httpcontext.current.aspx).</span><span class="sxs-lookup"><span data-stu-id="3ac7a-139">If hello request message is authenticated and authorized by hello `AuthenticationTestHandler`, then hello basic authentication user will be attached toohello current request on hello [HttpContext](https://msdn.microsoft.com/library/system.web.httpcontext.current.aspx).</span></span> <span data-ttu-id="3ac7a-140">Informace o uživateli v hello HttpContext budou používat jiný řadič (RegisterController) novější tooadd [značka](https://msdn.microsoft.com/library/azure/dn530749.aspx) toohello žádost o registraci oznámení.</span><span class="sxs-lookup"><span data-stu-id="3ac7a-140">User information in hello HttpContext will be used by another controller (RegisterController) later tooadd a [tag](https://msdn.microsoft.com/library/azure/dn530749.aspx) toohello notification registration request.</span></span>
     
       <span data-ttu-id="3ac7a-141">public class AuthenticationTestHandler : DelegatingHandler   {       protected override Task<HttpResponseMessage> SendAsync(       HttpRequestMessage request, CancellationToken cancellationToken)       {           var authorizationHeader = request.Headers.GetValues("Authorization").First();</span><span class="sxs-lookup"><span data-stu-id="3ac7a-141">public class AuthenticationTestHandler : DelegatingHandler   {       protected override Task<HttpResponseMessage> SendAsync(       HttpRequestMessage request, CancellationToken cancellationToken)       {           var authorizationHeader = request.Headers.GetValues("Authorization").First();</span></span>
     
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
       <span data-ttu-id="3ac7a-142">}</span><span class="sxs-lookup"><span data-stu-id="3ac7a-142">}</span></span>
     
     > [!NOTE]
     > <span data-ttu-id="3ac7a-143">**Poznámka k zabezpečení**: hello `AuthenticationTestHandler` třída neposkytuje true ověřování.</span><span class="sxs-lookup"><span data-stu-id="3ac7a-143">**Security Note**: hello `AuthenticationTestHandler` class does not provide true authentication.</span></span> <span data-ttu-id="3ac7a-144">Je použít jenom toomimic základní ověřování a není zabezpečený.</span><span class="sxs-lookup"><span data-stu-id="3ac7a-144">It is used only toomimic basic authentication and is not secure.</span></span> <span data-ttu-id="3ac7a-145">Ve svých produkčních aplikacích a službách musíte implementovat mechanismus zabezpečeného ověřování.</span><span class="sxs-lookup"><span data-stu-id="3ac7a-145">You must implement a secure authentication mechanism in your production applications and services.</span></span>                
     > 
     > 
4. <span data-ttu-id="3ac7a-146">Přidejte následující kód na konci hello hello hello `Register` metoda v hello **App_Start/WebApiConfig.cs** třídu obslužné rutiny zpráv hello tooregister:</span><span class="sxs-lookup"><span data-stu-id="3ac7a-146">Add hello following code at hello end of hello `Register` method in hello **App_Start/WebApiConfig.cs** class tooregister hello message handler:</span></span>
   
        config.MessageHandlers.Add(new AuthenticationTestHandler());
5. <span data-ttu-id="3ac7a-147">Uložte provedené změny.</span><span class="sxs-lookup"><span data-stu-id="3ac7a-147">Save your changes.</span></span>

## <a name="registering-for-notifications-using-hello-webapi-backend"></a><span data-ttu-id="3ac7a-148">Registrace pro oznámení prostřednictvím hello WebAPI back-end</span><span class="sxs-lookup"><span data-stu-id="3ac7a-148">Registering for Notifications using hello WebAPI Backend</span></span>
<span data-ttu-id="3ac7a-149">V této části přidáme toohandle WebAPI back-end sady nové řadiče toohello požadavky tooregister uživatele a zařízení pro oznámení pomocí hello klientské knihovny pro centra oznámení.</span><span class="sxs-lookup"><span data-stu-id="3ac7a-149">In this section, we will add a new controller toohello WebAPI backend toohandle requests tooregister a user and device for notifications using hello client library for notification hubs.</span></span> <span data-ttu-id="3ac7a-150">Přidá značku uživatele pro hello uživatele, který byl ověřen a připojené toohello HttpContext podle hello Hello řadiče `AuthenticationTestHandler`.</span><span class="sxs-lookup"><span data-stu-id="3ac7a-150">hello controller will add a user tag for hello user that was authenticated and attached toohello HttpContext by hello `AuthenticationTestHandler`.</span></span> <span data-ttu-id="3ac7a-151">značka Hello bude obsahovat řetězec formátu hello, `"username:<actual username>"`.</span><span class="sxs-lookup"><span data-stu-id="3ac7a-151">hello tag will have hello string format, `"username:<actual username>"`.</span></span>

1. <span data-ttu-id="3ac7a-152">V Průzkumníku řešení klikněte pravým tlačítkem na hello **AppBackend** projektu a pak klikněte na **spravovat balíčky NuGet**.</span><span class="sxs-lookup"><span data-stu-id="3ac7a-152">In Solution Explorer, right-click hello **AppBackend** project and then click **Manage NuGet Packages**.</span></span>
2. <span data-ttu-id="3ac7a-153">Na levé straně hello, klikněte na tlačítko **Online**a vyhledejte **Microsoft.Azure.NotificationHubs** v hello **vyhledávání** pole.</span><span class="sxs-lookup"><span data-stu-id="3ac7a-153">On hello left-hand side, click **Online**, and search for **Microsoft.Azure.NotificationHubs** in hello **Search** box.</span></span>
3. <span data-ttu-id="3ac7a-154">V seznamu výsledků hello, klikněte na tlačítko **Microsoft Azure Notification Hubs**a potom klikněte na **nainstalovat**.</span><span class="sxs-lookup"><span data-stu-id="3ac7a-154">In hello results list, click **Microsoft Azure Notification Hubs**, and then click **Install**.</span></span> <span data-ttu-id="3ac7a-155">Dokončení instalace hello a potom zavřete okno Správce balíčků NuGet hello.</span><span class="sxs-lookup"><span data-stu-id="3ac7a-155">Complete hello installation, then close hello NuGet package manager window.</span></span>
   
    <span data-ttu-id="3ac7a-156">Tento postup přidá odkaz toohello SDK centra oznámení Azure pomocí hello <a href="http://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/">balíčku Microsoft.Azure.Notification Hubs NuGet</a>.</span><span class="sxs-lookup"><span data-stu-id="3ac7a-156">This adds a reference toohello Azure Notification Hubs SDK using hello <a href="http://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/">Microsoft.Azure.Notification Hubs NuGet package</a>.</span></span>
4. <span data-ttu-id="3ac7a-157">Nyní vytvoříme nový soubor třídy, který představuje připojení hello s oznámení toosend použít Centrum oznámení.</span><span class="sxs-lookup"><span data-stu-id="3ac7a-157">We will now create a new class file that represents hello connection with notification hub used toosend notifications.</span></span> <span data-ttu-id="3ac7a-158">V hello Průzkumníku řešení klikněte pravým tlačítkem na hello **modely** složku, klikněte na tlačítko **přidat**, pak klikněte na tlačítko **třída**.</span><span class="sxs-lookup"><span data-stu-id="3ac7a-158">In hello Solution Explorer, right-click hello **Models** folder, click **Add**, then click **Class**.</span></span> <span data-ttu-id="3ac7a-159">Pojmenujte novou třídu hello **Notifications.cs**, pak klikněte na tlačítko **přidat** toogenerate hello třídy.</span><span class="sxs-lookup"><span data-stu-id="3ac7a-159">Name hello new class **Notifications.cs**, then click **Add** toogenerate hello class.</span></span> 
   
    ![][B6]
5. <span data-ttu-id="3ac7a-160">V Notifications.cs, přidejte následující hello `using` příkaz hello horní části souboru hello:</span><span class="sxs-lookup"><span data-stu-id="3ac7a-160">In Notifications.cs, add hello following `using` statement at hello top of hello file:</span></span>
   
        using Microsoft.Azure.NotificationHubs;
6. <span data-ttu-id="3ac7a-161">Nahraďte hello `Notifications` definici s hello následující třídy a ujistěte se, že tooreplace hello dva zástupné symboly hello připojovací řetězec (s úplným přístupem) pro vaše Centrum oznámení a hello název centra (k dispozici na [portálu Azure Classic ](http://manage.windowsazure.com)):</span><span class="sxs-lookup"><span data-stu-id="3ac7a-161">Replace hello `Notifications` class definition with hello following and make sure tooreplace hello two placeholders with hello connection string (with full access) for your notification hub, and hello hub name (available at [Azure Classic Portal](http://manage.windowsazure.com)):</span></span>
   
        public class Notifications
        {
            public static Notifications Instance = new Notifications();
   
            public NotificationHubClient Hub { get; set; }
   
            private Notifications() {
                Hub = NotificationHubClient.CreateClientFromConnectionString("<your hub's DefaultFullSharedAccessSignature>", 
                                                                             "<hub name>");
            }
        }
7. <span data-ttu-id="3ac7a-162">Dále vytvoříme nový kontroler **RegisterController**.</span><span class="sxs-lookup"><span data-stu-id="3ac7a-162">Next we will create a new controller named **RegisterController**.</span></span> <span data-ttu-id="3ac7a-163">V Průzkumníku řešení klikněte pravým tlačítkem na hello **řadiče** složku, pak klikněte na tlačítko **přidat**, pak klikněte na tlačítko **řadič**.</span><span class="sxs-lookup"><span data-stu-id="3ac7a-163">In Solution Explorer, right-click hello **Controllers** folder, then click **Add**, then click **Controller**.</span></span> <span data-ttu-id="3ac7a-164">Klikněte na tlačítko hello **webové rozhraní API 2 řadiče – prázdný** položky a pak klikněte na **přidat**.</span><span class="sxs-lookup"><span data-stu-id="3ac7a-164">Click hello **Web API 2 Controller -- Empty** item, and then click **Add**.</span></span> <span data-ttu-id="3ac7a-165">Pojmenujte novou třídu hello **RegisterController**a potom klikněte na **přidat** znovu toogenerate hello řadiče.</span><span class="sxs-lookup"><span data-stu-id="3ac7a-165">Name hello new class **RegisterController**, and then click **Add** again toogenerate hello controller.</span></span>
   
    ![][B7]
   
    ![][B8]
8. <span data-ttu-id="3ac7a-166">V RegisterController.cs, přidejte následující hello `using` příkazy:</span><span class="sxs-lookup"><span data-stu-id="3ac7a-166">In RegisterController.cs, add hello following `using` statements:</span></span>
   
        using Microsoft.Azure.NotificationHubs;
        using Microsoft.Azure.NotificationHubs.Messaging;
        using AppBackend.Models;
        using System.Threading.Tasks;
        using System.Web;
9. <span data-ttu-id="3ac7a-167">Přidejte následující kód do hello hello `RegisterController` definici třídy.</span><span class="sxs-lookup"><span data-stu-id="3ac7a-167">Add hello following code inside hello `RegisterController` class definition.</span></span> <span data-ttu-id="3ac7a-168">Všimněte si, že se v tomto kódu, přidáme, aby byl značku uživatele pro uživatele hello, to je připojen toohello položka HttpContext.</span><span class="sxs-lookup"><span data-stu-id="3ac7a-168">Note that in this code, we add a user tag for hello user this is attached toohello HttpContext.</span></span> <span data-ttu-id="3ac7a-169">Hello uživatele došlo k ověření a připojené toohello HttpContext filtrem hello zprávu jsme přidali, `AuthenticationTestHandler`.</span><span class="sxs-lookup"><span data-stu-id="3ac7a-169">hello user was authenticated and attached toohello HttpContext by hello message filter we added, `AuthenticationTestHandler`.</span></span> <span data-ttu-id="3ac7a-170">Můžete také přidat tooverify volitelné kontroly, které hello uživatel má oprávnění tooregister pro hello požadované značky.</span><span class="sxs-lookup"><span data-stu-id="3ac7a-170">You can also add optional checks tooverify that hello user has rights tooregister for hello requested tags.</span></span>
   
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
10. <span data-ttu-id="3ac7a-171">Uložte provedené změny.</span><span class="sxs-lookup"><span data-stu-id="3ac7a-171">Save your changes.</span></span>

## <a name="sending-notifications-from-hello-webapi-backend"></a><span data-ttu-id="3ac7a-172">Odesílání oznámení z hello WebAPI back-end</span><span class="sxs-lookup"><span data-stu-id="3ac7a-172">Sending Notifications from hello WebAPI Backend</span></span>
<span data-ttu-id="3ac7a-173">V této části přidáte nový řadič, který zveřejňuje způsob, jak klientské zařízení toosend oznámení podle značky hello uživatelské jméno pomocí knihovny správy služby centra oznámení Azure v hello ASP.NET WebAPI back-end.</span><span class="sxs-lookup"><span data-stu-id="3ac7a-173">In this section you add a new controller that exposes a way for client devices toosend a notification based on hello username tag using Azure Notification Hubs Service Management Library in hello ASP.NET WebAPI backend.</span></span>

1. <span data-ttu-id="3ac7a-174">Vytvořte další nový kontroler **NotificationsController**.</span><span class="sxs-lookup"><span data-stu-id="3ac7a-174">Create another new controller named **NotificationsController**.</span></span> <span data-ttu-id="3ac7a-175">Vytvořit hello stejně jako jste vytvořili hello **RegisterController** v předchozí části hello.</span><span class="sxs-lookup"><span data-stu-id="3ac7a-175">Create it hello same way you created hello **RegisterController** in hello previous section.</span></span>
2. <span data-ttu-id="3ac7a-176">V NotificationsController.cs, přidejte následující hello `using` příkazy:</span><span class="sxs-lookup"><span data-stu-id="3ac7a-176">In NotificationsController.cs, add hello following `using` statements:</span></span>
   
        using AppBackend.Models;
        using System.Threading.Tasks;
        using System.Web;
3. <span data-ttu-id="3ac7a-177">Přidejte následující metodu toohello hello **NotificationsController** třídy.</span><span class="sxs-lookup"><span data-stu-id="3ac7a-177">Add hello following method toohello **NotificationsController** class.</span></span>
   
    <span data-ttu-id="3ac7a-178">Tento kód odeslat typu oznámení založené na hello platformy oznámení služby (PNS) `pns` parametr.</span><span class="sxs-lookup"><span data-stu-id="3ac7a-178">This code send a notification type based on hello Platform Notification Service (PNS) `pns` parameter.</span></span> <span data-ttu-id="3ac7a-179">Hello hodnotu `to_tag` je použité tooset hello *uživatelské jméno* značky na uvítací zprávu.</span><span class="sxs-lookup"><span data-stu-id="3ac7a-179">hello value of `to_tag` is used tooset hello *username* tag on hello message.</span></span> <span data-ttu-id="3ac7a-180">Tato značka musí odpovídat značce uživatelského jména aktivní registrace k centru událostí.</span><span class="sxs-lookup"><span data-stu-id="3ac7a-180">This tag must match a username tag of an active notification hub registration.</span></span> <span data-ttu-id="3ac7a-181">Hello oznámení je načtený z textu hello hello požadavku POST a formátovat cílový hello systém PNS.</span><span class="sxs-lookup"><span data-stu-id="3ac7a-181">hello notification message is pulled from hello body of hello POST request and formatted for hello target PNS.</span></span> 
   
    <span data-ttu-id="3ac7a-182">V závislosti na hello platformy oznámení služby (PNS), podporovaných zařízení používat tooreceive oznámení podporují různé oznámení pomocí různých formátech.</span><span class="sxs-lookup"><span data-stu-id="3ac7a-182">Depending on hello Platform Notification Service (PNS) that your supported devices use tooreceive notifications, different notifications are supported using different formats.</span></span> <span data-ttu-id="3ac7a-183">Například na zařízeních s Windows byste mohli použít [informační zprávu pomocí Služby nabízených oznámení Windows](https://msdn.microsoft.com/library/windows/apps/br230849.aspx), kterou ostatní systémy oznámení platformy přímo nepodporují.</span><span class="sxs-lookup"><span data-stu-id="3ac7a-183">For example on Windows devices, you could use a [toast notification with WNS](https://msdn.microsoft.com/library/windows/apps/br230849.aspx) that isn't directly supported by another PNS.</span></span> <span data-ttu-id="3ac7a-184">Aby váš back-end potřebovat tooformat hello oznámení do podporovaných oznámení pro hello systému PNS zařízení plánujete toosupport.</span><span class="sxs-lookup"><span data-stu-id="3ac7a-184">So your backend would need tooformat hello notification into a supported notification for hello PNS of devices you plan toosupport.</span></span> <span data-ttu-id="3ac7a-185">Potom pomocí rozhraní API odpovídající odesílání hello na hello [NotificationHubClient – třída](https://msdn.microsoft.com/library/azure/microsoft.azure.notificationhubs.notificationhubclient_methods.aspx)</span><span class="sxs-lookup"><span data-stu-id="3ac7a-185">Then use hello appropriate send API on hello [NotificationHubClient class](https://msdn.microsoft.com/library/azure/microsoft.azure.notificationhubs.notificationhubclient_methods.aspx)</span></span>
   
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
4. <span data-ttu-id="3ac7a-186">Stiskněte klávesu **F5** toorun hello aplikace a tooensure hello přesnost své dosavadní práce.</span><span class="sxs-lookup"><span data-stu-id="3ac7a-186">Press **F5** toorun hello application and tooensure hello accuracy of your work so far.</span></span> <span data-ttu-id="3ac7a-187">Hello aplikace by měla spustit webový prohlížeč a zobrazit domovskou stránku ASP.NET hello.</span><span class="sxs-lookup"><span data-stu-id="3ac7a-187">hello app should launch a web browser and display hello ASP.NET home page.</span></span> 

## <a name="publish-hello-new-webapi-backend"></a><span data-ttu-id="3ac7a-188">Publikování hello nový back-end WebAPI</span><span class="sxs-lookup"><span data-stu-id="3ac7a-188">Publish hello new WebAPI Backend</span></span>
1. <span data-ttu-id="3ac7a-189">Nyní nasadíme tuto aplikaci tooan webu Azure v pořadí toomake je přístupná ze všech zařízení.</span><span class="sxs-lookup"><span data-stu-id="3ac7a-189">Now we will deploy this app tooan Azure Website in order toomake it accessible from all devices.</span></span> <span data-ttu-id="3ac7a-190">Klikněte pravým tlačítkem na hello **AppBackend** projektu a vyberte **publikovat**.</span><span class="sxs-lookup"><span data-stu-id="3ac7a-190">Right-click on hello **AppBackend** project and select **Publish**.</span></span>
2. <span data-ttu-id="3ac7a-191">Jako cíl publikování vyberte **Microsoft Azure App Service** a klikněte na **Publikovat**.</span><span class="sxs-lookup"><span data-stu-id="3ac7a-191">Select **Microsoft Azure App Service** as your publish target and click **Publish**.</span></span> <span data-ttu-id="3ac7a-192">Otevře se dialog hello vytvořit službu App Service, který vám pomůže vytvořit všechny hello potřebné prostředky Azure toorun hello webové aplikace ASP.NET v Azure.</span><span class="sxs-lookup"><span data-stu-id="3ac7a-192">This opens hello Create App Service dialog, which helps you create all hello necessary Azure resources toorun hello ASP.NET web app in Azure.</span></span>

    ![][B15]
3. <span data-ttu-id="3ac7a-193">V hello **vytvořit službu App Service** dialogovém okně, vyberte svůj účet Azure.</span><span class="sxs-lookup"><span data-stu-id="3ac7a-193">In hello **Create App Service** dialog, select your Azure account.</span></span> <span data-ttu-id="3ac7a-194">Klikněte na **Změnit typ** a vyberte **Webová aplikace**.</span><span class="sxs-lookup"><span data-stu-id="3ac7a-194">Click **Change Type** and select **Web App**.</span></span> <span data-ttu-id="3ac7a-195">Zachovat hello **název webové aplikace** dané a vyberte možnost hello **předplatné**, **skupiny prostředků**, a **plán služby App Service**.</span><span class="sxs-lookup"><span data-stu-id="3ac7a-195">Keep hello **Web App Name** given and select hello **Subscription**, **Resource Group**, and **App Service Plan**.</span></span>  <span data-ttu-id="3ac7a-196">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="3ac7a-196">Click **Create**.</span></span>

4. <span data-ttu-id="3ac7a-197">Poznamenejte si hello **adresa URL webu** vlastnost hello **Souhrn** části.</span><span class="sxs-lookup"><span data-stu-id="3ac7a-197">Make a note of hello **Site URL** property in hello **Summary** section.</span></span> <span data-ttu-id="3ac7a-198">Označujeme toothis adresu URL jako vaše *koncový bod back-end* dál v tomto kurzu.</span><span class="sxs-lookup"><span data-stu-id="3ac7a-198">We will refer toothis URL as your *backend endpoint* later in this tutorial.</span></span> <span data-ttu-id="3ac7a-199">Klikněte na **Publikovat**.</span><span class="sxs-lookup"><span data-stu-id="3ac7a-199">Click **Publish**.</span></span>

5. <span data-ttu-id="3ac7a-200">Po dokončení Průvodce hello, tato možnost publikuje hello ASP.NET webové aplikace tooAzure a pak spustí hello aplikace v hello výchozí prohlížeč.</span><span class="sxs-lookup"><span data-stu-id="3ac7a-200">Once hello wizard completes, it publishes hello ASP.NET web app tooAzure, and then launches hello app in hello default browser.</span></span>  <span data-ttu-id="3ac7a-201">Vaši aplikaci bude možné zobrazit ve službě Azure App Service.</span><span class="sxs-lookup"><span data-stu-id="3ac7a-201">Your application will be viewable in Azure App Services.</span></span>

<span data-ttu-id="3ac7a-202">Adresa URL Hello používá název hello webové aplikace, který jste zadali dříve, s http://<app_name>.azurewebsites.net formátu hello.</span><span class="sxs-lookup"><span data-stu-id="3ac7a-202">hello URL uses hello web app name that you specified earlier, with hello format http://<app_name>.azurewebsites.net.</span></span>

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
