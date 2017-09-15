## <a name="create-the-webapi-project"></a><span data-ttu-id="11a55-101">Vytvoření projektu WebAPI</span><span class="sxs-lookup"><span data-stu-id="11a55-101">Create the WebAPI Project</span></span>
<span data-ttu-id="11a55-102">V následujících částech se vytvoří nový back-end ASP.NET WebAPI, který bude sloužit ke třem hlavním účelům:</span><span class="sxs-lookup"><span data-stu-id="11a55-102">A new ASP.NET WebAPI backend will be created in the sections that follow and it will have three main purposes:</span></span>

1. <span data-ttu-id="11a55-103">**Ověřování klientů:** Později se přidá popisovač zprávy, který bude ověřovat požadavky klientů a přiřazovat k požadavkům uživatele.</span><span class="sxs-lookup"><span data-stu-id="11a55-103">**Authenticating Clients**: A message handler will be added later to authenticate client requests and associate the user with the request.</span></span>
2. <span data-ttu-id="11a55-104">**Registrace klientských oznámení:** Později přidáte kontroler, který bude zpracovávat nové registrace klientských zařízení k přijímání oznámení.</span><span class="sxs-lookup"><span data-stu-id="11a55-104">**Client Notification Registrations**: Later, you will add a controller to handle new registrations for a client device to receive notifications.</span></span> <span data-ttu-id="11a55-105">Ověřené uživatelské jméno se automaticky přidá do registrace jako [značka](https://msdn.microsoft.com/library/azure/dn530749.aspx).</span><span class="sxs-lookup"><span data-stu-id="11a55-105">The authenticated user name will automatically be added to the registration as a [tag](https://msdn.microsoft.com/library/azure/dn530749.aspx).</span></span>
3. <span data-ttu-id="11a55-106">**Odesílání oznámení klientům:** Později přidáte také kontroler, který uživateli umožní aktivovat zabezpečené nabízení do zařízení a klientů přidružených ke značce.</span><span class="sxs-lookup"><span data-stu-id="11a55-106">**Sending Notifications to Clients**: Later, you will also add a controller to provide a way for a user to trigger a secure push to devices and clients associated with the tag.</span></span> 

<span data-ttu-id="11a55-107">Následující kroky ukazují, jak vytvořit nový back-end ASP.NET WebAPI:</span><span class="sxs-lookup"><span data-stu-id="11a55-107">The following steps show how to create the new ASP.NET WebAPI backend:</span></span> 

> [!IMPORTANT]
> <span data-ttu-id="11a55-108">Pokud používáte sadu Visual Studio 2015 nebo starší, před zahájením tohoto kurzu se ujistěte, že máte nainstalovanou nejnovější verzi správce balíčků NuGet.</span><span class="sxs-lookup"><span data-stu-id="11a55-108">If you are using Visual Studio 2015 or earlier, before starting this tutorial, please ensure that you have installed the latest version of the NuGet Package Manager.</span></span> <span data-ttu-id="11a55-109">Pokud to chcete zkontrolovat, spusťte sadu Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="11a55-109">To check, start Visual Studio.</span></span> <span data-ttu-id="11a55-110">V nabídce **Nástroje** klikněte na **Rozšíření a aktualizace**.</span><span class="sxs-lookup"><span data-stu-id="11a55-110">From the **Tools** menu, click **Extensions and Updates**.</span></span> <span data-ttu-id="11a55-111">Vyhledejte **Správce balíčků NuGet** pro vaši verzi sady Visual Studio a ujistěte se, že máte nejnovější verzi.</span><span class="sxs-lookup"><span data-stu-id="11a55-111">Search for **NuGet Package Manager** for your version of Visual Studio, and make sure you have the latest version.</span></span> <span data-ttu-id="11a55-112">Pokud ne, odinstalujte a znovu nainstalujte správce balíčků NuGet.</span><span class="sxs-lookup"><span data-stu-id="11a55-112">If not, please uninstall, then reinstall the NuGet Package Manager.</span></span>
> 
> ![][B4]
> 
> [!NOTE]
> <span data-ttu-id="11a55-113">Ujistěte se, že máte nainstalovanou sadu [Azure SDK](https://azure.microsoft.com/downloads/) pro vývoj pro web.</span><span class="sxs-lookup"><span data-stu-id="11a55-113">Make sure you have installed the Visual Studio [Azure SDK](https://azure.microsoft.com/downloads/) for website deployment.</span></span>
> 
> 

1. <span data-ttu-id="11a55-114">Spusťte sadu Visual Studio nebo Visual Studio Express.</span><span class="sxs-lookup"><span data-stu-id="11a55-114">Start Visual Studio or Visual Studio Express.</span></span> <span data-ttu-id="11a55-115">Klikněte na **Průzkumník serveru** a přihlaste se ke svému účtu Azure.</span><span class="sxs-lookup"><span data-stu-id="11a55-115">Click **Server Explorer** and sign in to your Azure account.</span></span> <span data-ttu-id="11a55-116">Sada Visual Studio bude potřebovat vaše přihlášení, aby ve vašem účtu mohla vytvořit prostředky webu.</span><span class="sxs-lookup"><span data-stu-id="11a55-116">Visual Studio will need you signed in to create the web site resources on your account.</span></span>
2. <span data-ttu-id="11a55-117">V sadě Visual Studio klikněte na **Soubor**, potom na **Nový**, **Projekt**, rozbalte **Šablony**, **Visual C#**, klikněte na **Web** a **Webová aplikace ASP.NET**, zadejte název **AppBackend** a klikněte na **OK**.</span><span class="sxs-lookup"><span data-stu-id="11a55-117">In Visual Studio, click **File**, then click **New**, then **Project**, expand **Templates**, **Visual C#**, then click **Web** and **ASP.NET Web Application**, type the name **AppBackend**, and then click **OK**.</span></span> 
   
    ![][B1]
3. <span data-ttu-id="11a55-118">V dialogovém okně **Nový projekt ASP.NET** klikněte na **Web API** a potom na **OK**.</span><span class="sxs-lookup"><span data-stu-id="11a55-118">In the **New ASP.NET Project** dialog, click **Web API**, then click **OK**.</span></span>
   
    ![][B2]
4. <span data-ttu-id="11a55-119">V dialogovém okně **Konfigurace webové aplikace Microsoft Azure** zvolte předplatné a **Plán služby App Service**, který už jste vytvořili.</span><span class="sxs-lookup"><span data-stu-id="11a55-119">In the **Configure Microsoft Azure Web App** dialog, choose a subscription, and an **App Service plan** you have already created.</span></span> <span data-ttu-id="11a55-120">Můžete také zvolit možnost **Vytvořit nový plán služby App Service** a vytvořit nový plán v dialogovém okně.</span><span class="sxs-lookup"><span data-stu-id="11a55-120">You can also choose **Create a new app service plan** and create one from the dialog.</span></span> <span data-ttu-id="11a55-121">Pro účely tohoto kurzu nepotřebujete databázi.</span><span class="sxs-lookup"><span data-stu-id="11a55-121">You do not need a database for this tutorial.</span></span> <span data-ttu-id="11a55-122">Jakmile vyberete plán služby App Service, kliknutím na **OK** vytvořte projekt.</span><span class="sxs-lookup"><span data-stu-id="11a55-122">Once you have selected your app service plan, click **OK** to create the project.</span></span>
   
    ![][B5]

## <a name="authenticating-clients-to-the-webapi-backend"></a><span data-ttu-id="11a55-123">Ověřování klientů v back-endu WebAPI</span><span class="sxs-lookup"><span data-stu-id="11a55-123">Authenticating Clients to the WebAPI Backend</span></span>
<span data-ttu-id="11a55-124">V této části pro nový back-end vytvoříte novou třídu popisovače zprávy **AuthenticationTestHandler**.</span><span class="sxs-lookup"><span data-stu-id="11a55-124">In this section, you will create a new message handler class named **AuthenticationTestHandler** for the new backend.</span></span> <span data-ttu-id="11a55-125">Tato třída je odvozená od třídy [DelegatingHandler](https://msdn.microsoft.com/library/system.net.http.delegatinghandler.aspx) a přidaná jako popisovač zprávy, aby mohla zpracovávat všechny požadavky přicházející na back-end.</span><span class="sxs-lookup"><span data-stu-id="11a55-125">This class is derived from [DelegatingHandler](https://msdn.microsoft.com/library/system.net.http.delegatinghandler.aspx) and added as a message handler so it can process all requests coming into the backend.</span></span> 

1. <span data-ttu-id="11a55-126">V Průzkumníku řešení klikněte pravým tlačítkem na projekt **AppBackend**, klikněte na **Přidat** a potom klikněte na **Třída**.</span><span class="sxs-lookup"><span data-stu-id="11a55-126">In Solution Explorer, right-click the **AppBackend** project, click **Add**, then click **Class**.</span></span> <span data-ttu-id="11a55-127">Pojmenujte novou třídu **AuthenticationTestHandler.cs** a kliknutím na **Přidat** ji vygenerujte.</span><span class="sxs-lookup"><span data-stu-id="11a55-127">Name the new class **AuthenticationTestHandler.cs**, and click **Add** to generate the class.</span></span> <span data-ttu-id="11a55-128">Tato třída bude pro zjednodušení sloužit k ověřování uživatelů pomocí *základního ověřování*.</span><span class="sxs-lookup"><span data-stu-id="11a55-128">This class will be used to authenticate users using *Basic Authentication* for simplicity.</span></span> <span data-ttu-id="11a55-129">Nezapomeňte, že vaše aplikace může používat jakékoli schéma ověřování.</span><span class="sxs-lookup"><span data-stu-id="11a55-129">Note that your app can use any authentication scheme.</span></span>
2. <span data-ttu-id="11a55-130">V souboru AuthenticationTestHandler.cs přidejte následující příkazy `using`:</span><span class="sxs-lookup"><span data-stu-id="11a55-130">In AuthenticationTestHandler.cs, add the following `using` statements:</span></span>
   
        using System.Net.Http;
        using System.Threading;
        using System.Security.Principal;
        using System.Net;
        using System.Text;
        using System.Threading.Tasks;

3. <span data-ttu-id="11a55-131">V souboru AuthenticationTestHandler.cs nahraďte definici třídy `AuthenticationTestHandler` následujícím kódem.</span><span class="sxs-lookup"><span data-stu-id="11a55-131">In AuthenticationTestHandler.cs, replacing the `AuthenticationTestHandler` class definition with the following code.</span></span> 
   
    <span data-ttu-id="11a55-132">Tato obslužná rutina ověří požadavek při splnění všech těchto tří podmínek:</span><span class="sxs-lookup"><span data-stu-id="11a55-132">This handler will authorize the request when the following three conditions are all true:</span></span>
   
   * <span data-ttu-id="11a55-133">Požadavek obsahuje *autorizační* hlavičku.</span><span class="sxs-lookup"><span data-stu-id="11a55-133">The request included an *Authorization* header.</span></span> 
   * <span data-ttu-id="11a55-134">Požadavek používá *základní* ověřování.</span><span class="sxs-lookup"><span data-stu-id="11a55-134">The request uses *basic* authentication.</span></span> 
   * <span data-ttu-id="11a55-135">Řetězce uživatelského jména a hesla jsou stejné.</span><span class="sxs-lookup"><span data-stu-id="11a55-135">The user name string and the password string are the same string.</span></span>
     
     <span data-ttu-id="11a55-136">Jinak bude požadavek zamítnut.</span><span class="sxs-lookup"><span data-stu-id="11a55-136">Otherwise, the request will be rejected.</span></span> <span data-ttu-id="11a55-137">Toto není správný přístup k ověřování a autorizaci.</span><span class="sxs-lookup"><span data-stu-id="11a55-137">This is not a true authentication and authorization approach.</span></span> <span data-ttu-id="11a55-138">Je to jenom velmi jednoduchý příklad pro účely tohoto kurzu.</span><span class="sxs-lookup"><span data-stu-id="11a55-138">It is just a very simple example for this tutorial.</span></span>
     
     <span data-ttu-id="11a55-139">Pokud třída `AuthenticationTestHandler` ověří a autorizuje zprávu požadavku, pak se uživatel základního ověřování připojí k aktuálnímu požadavku v objektu [HttpContext](https://msdn.microsoft.com/library/system.web.httpcontext.current.aspx).</span><span class="sxs-lookup"><span data-stu-id="11a55-139">If the request message is authenticated and authorized by the `AuthenticationTestHandler`, then the basic authentication user will be attached to the current request on the [HttpContext](https://msdn.microsoft.com/library/system.web.httpcontext.current.aspx).</span></span> <span data-ttu-id="11a55-140">Informace o uživateli v objektu HttpContext později použije jiný kontroler (RegisterController) pro přidání [značky](https://msdn.microsoft.com/library/azure/dn530749.aspx) do požadavku na registraci oznámení.</span><span class="sxs-lookup"><span data-stu-id="11a55-140">User information in the HttpContext will be used by another controller (RegisterController) later to add a [tag](https://msdn.microsoft.com/library/azure/dn530749.aspx) to the notification registration request.</span></span>
     
       <span data-ttu-id="11a55-141">public class AuthenticationTestHandler : DelegatingHandler   {       protected override Task<HttpResponseMessage> SendAsync(       HttpRequestMessage request, CancellationToken cancellationToken)       {           var authorizationHeader = request.Headers.GetValues("Authorization").First();</span><span class="sxs-lookup"><span data-stu-id="11a55-141">public class AuthenticationTestHandler : DelegatingHandler   {       protected override Task<HttpResponseMessage> SendAsync(       HttpRequestMessage request, CancellationToken cancellationToken)       {           var authorizationHeader = request.Headers.GetValues("Authorization").First();</span></span>
     
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
                       // Attach the new principal object to the current HttpContext object
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
       <span data-ttu-id="11a55-142">}</span><span class="sxs-lookup"><span data-stu-id="11a55-142">}</span></span>
     
     > [!NOTE]
     > <span data-ttu-id="11a55-143">**Poznámka k zabezpečení:** Třída `AuthenticationTestHandler` nezajišťuje skutečné ověřování.</span><span class="sxs-lookup"><span data-stu-id="11a55-143">**Security Note**: The `AuthenticationTestHandler` class does not provide true authentication.</span></span> <span data-ttu-id="11a55-144">Používá se pouze k napodobení základního ověřování a není bezpečná.</span><span class="sxs-lookup"><span data-stu-id="11a55-144">It is used only to mimic basic authentication and is not secure.</span></span> <span data-ttu-id="11a55-145">Ve svých produkčních aplikacích a službách musíte implementovat mechanismus zabezpečeného ověřování.</span><span class="sxs-lookup"><span data-stu-id="11a55-145">You must implement a secure authentication mechanism in your production applications and services.</span></span>                
     > 
     > 
4. <span data-ttu-id="11a55-146">Přidejte následující kód pro registraci popisovače zprávy na konec metody `Register` ve třídě **App_Start/WebApiConfig.cs**:</span><span class="sxs-lookup"><span data-stu-id="11a55-146">Add the following code at the end of the `Register` method in the **App_Start/WebApiConfig.cs** class to register the message handler:</span></span>
   
        config.MessageHandlers.Add(new AuthenticationTestHandler());
5. <span data-ttu-id="11a55-147">Uložte provedené změny.</span><span class="sxs-lookup"><span data-stu-id="11a55-147">Save your changes.</span></span>

## <a name="registering-for-notifications-using-the-webapi-backend"></a><span data-ttu-id="11a55-148">Registrace k oznámením pomocí back-endu WebAPI</span><span class="sxs-lookup"><span data-stu-id="11a55-148">Registering for Notifications using the WebAPI Backend</span></span>
<span data-ttu-id="11a55-149">V této části přidáme do back-endu WebAPI nový kontroler, který bude zpracovávat požadavky na registraci uživatele a zařízení k oznámením pomocí klientské knihovny pro centra oznámení.</span><span class="sxs-lookup"><span data-stu-id="11a55-149">In this section, we will add a new controller to the WebAPI backend to handle requests to register a user and device for notifications using the client library for notification hubs.</span></span> <span data-ttu-id="11a55-150">Kontroler přidá značku uživatele pro uživatele, který byl ověřen a připojen k objektu HttpContext třídou `AuthenticationTestHandler`.</span><span class="sxs-lookup"><span data-stu-id="11a55-150">The controller will add a user tag for the user that was authenticated and attached to the HttpContext by the `AuthenticationTestHandler`.</span></span> <span data-ttu-id="11a55-151">Značka bude mít formát řetězce `"username:<actual username>"`.</span><span class="sxs-lookup"><span data-stu-id="11a55-151">The tag will have the string format, `"username:<actual username>"`.</span></span>

1. <span data-ttu-id="11a55-152">V Průzkumníku řešení klikněte pravým tlačítkem na projekt **AppBackend** a potom klikněte na **Správa balíčků NuGet**.</span><span class="sxs-lookup"><span data-stu-id="11a55-152">In Solution Explorer, right-click the **AppBackend** project and then click **Manage NuGet Packages**.</span></span>
2. <span data-ttu-id="11a55-153">Na levé straně klikněte na **Online** a ve **vyhledávacím** poli vyhledejte **Microsoft.Azure.NotificationHubs**.</span><span class="sxs-lookup"><span data-stu-id="11a55-153">On the left-hand side, click **Online**, and search for **Microsoft.Azure.NotificationHubs** in the **Search** box.</span></span>
3. <span data-ttu-id="11a55-154">V seznamů výsledků klikněte na **Microsoft Azure Notification Hubs** a potom klikněte na **Nainstalovat**.</span><span class="sxs-lookup"><span data-stu-id="11a55-154">In the results list, click **Microsoft Azure Notification Hubs**, and then click **Install**.</span></span> <span data-ttu-id="11a55-155">Dokončete instalaci a potom zavřete okno správce balíčků NuGet.</span><span class="sxs-lookup"><span data-stu-id="11a55-155">Complete the installation, then close the NuGet package manager window.</span></span>
   
    <span data-ttu-id="11a55-156">Ten přidá odkaz na sadu SDK centra oznámení Azure pomocí <a href="http://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/">balíčku Microsoft.Azure.Notification Hubs NuGet</a>.</span><span class="sxs-lookup"><span data-stu-id="11a55-156">This adds a reference to the Azure Notification Hubs SDK using the <a href="http://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/">Microsoft.Azure.Notification Hubs NuGet package</a>.</span></span>
4. <span data-ttu-id="11a55-157">Nyní vytvoříme nový soubor třídy, která představuje propojení s centrem událostí sloužícím k odesílání oznámení.</span><span class="sxs-lookup"><span data-stu-id="11a55-157">We will now create a new class file that represents the connection with notification hub used to send notifications.</span></span> <span data-ttu-id="11a55-158">V Průzkumníku řešení klikněte pravým tlačítkem na složku **Modely**, klikněte na **Přidat** a potom klikněte na **Třída**.</span><span class="sxs-lookup"><span data-stu-id="11a55-158">In the Solution Explorer, right-click the **Models** folder, click **Add**, then click **Class**.</span></span> <span data-ttu-id="11a55-159">Pojmenujte novou třídu **Notifications.cs** a potom ji kliknutím na **Přidat** vygenerujte.</span><span class="sxs-lookup"><span data-stu-id="11a55-159">Name the new class **Notifications.cs**, then click **Add** to generate the class.</span></span> 
   
    ![][B6]
5. <span data-ttu-id="11a55-160">Na začátek souboru Notifications.cs přidejte následující příkaz `using`:</span><span class="sxs-lookup"><span data-stu-id="11a55-160">In Notifications.cs, add the following `using` statement at the top of the file:</span></span>
   
        using Microsoft.Azure.NotificationHubs;
6. <span data-ttu-id="11a55-161">Nahraďte definici třídy `Notifications` následujícím kódem a nezapomeňte přitom nahradit dva zástupné symboly připojovacím řetězcem (pro úplný přístup) vašeho centra událostí a názvem centra (najdete ho na [portálu Azure Classic](http://manage.windowsazure.com)):</span><span class="sxs-lookup"><span data-stu-id="11a55-161">Replace the `Notifications` class definition with the following and make sure to replace the two placeholders with the connection string (with full access) for your notification hub, and the hub name (available at [Azure Classic Portal](http://manage.windowsazure.com)):</span></span>
   
        public class Notifications
        {
            public static Notifications Instance = new Notifications();
   
            public NotificationHubClient Hub { get; set; }
   
            private Notifications() {
                Hub = NotificationHubClient.CreateClientFromConnectionString("<your hub's DefaultFullSharedAccessSignature>", 
                                                                             "<hub name>");
            }
        }
7. <span data-ttu-id="11a55-162">Dále vytvoříme nový kontroler **RegisterController**.</span><span class="sxs-lookup"><span data-stu-id="11a55-162">Next we will create a new controller named **RegisterController**.</span></span> <span data-ttu-id="11a55-163">V Průzkumníku řešení klikněte pravým tlačítkem na složku **Kontrolery**, klikněte na **Přidat** a potom na **Kontroler**.</span><span class="sxs-lookup"><span data-stu-id="11a55-163">In Solution Explorer, right-click the **Controllers** folder, then click **Add**, then click **Controller**.</span></span> <span data-ttu-id="11a55-164">Klikněte na položku **Kontroler rozhraní Web API 2 – Prázdný** a potom klikněte na **Přidat**.</span><span class="sxs-lookup"><span data-stu-id="11a55-164">Click the **Web API 2 Controller -- Empty** item, and then click **Add**.</span></span> <span data-ttu-id="11a55-165">Pojmenujte novou třídu **RegisterController** a znovu klikněte na **Přidat**. Vygeneruje se kontroler.</span><span class="sxs-lookup"><span data-stu-id="11a55-165">Name the new class **RegisterController**, and then click **Add** again to generate the controller.</span></span>
   
    ![][B7]
   
    ![][B8]
8. <span data-ttu-id="11a55-166">V souboru RegisterController.cs přidejte následující příkazy `using`:</span><span class="sxs-lookup"><span data-stu-id="11a55-166">In RegisterController.cs, add the following `using` statements:</span></span>
   
        using Microsoft.Azure.NotificationHubs;
        using Microsoft.Azure.NotificationHubs.Messaging;
        using AppBackend.Models;
        using System.Threading.Tasks;
        using System.Web;
9. <span data-ttu-id="11a55-167">Do definice třídy `RegisterController` přidejte následující kód.</span><span class="sxs-lookup"><span data-stu-id="11a55-167">Add the following code inside the `RegisterController` class definition.</span></span> <span data-ttu-id="11a55-168">Všimněte si, že v tomto kódu přidáváme značku uživatele pro uživatele, který je připojený k objektu HttpContext.</span><span class="sxs-lookup"><span data-stu-id="11a55-168">Note that in this code, we add a user tag for the user this is attached to the HttpContext.</span></span> <span data-ttu-id="11a55-169">Tento uživatel byl ověřen a připojen k objektu HttpContext filtrem zpráv `AuthenticationTestHandler`, který jsme vytvořili.</span><span class="sxs-lookup"><span data-stu-id="11a55-169">The user was authenticated and attached to the HttpContext by the message filter we added, `AuthenticationTestHandler`.</span></span> <span data-ttu-id="11a55-170">Můžete také přidat volitelné kontroly pro ověření, že uživatel má práva pro registraci k požadovaným značkám.</span><span class="sxs-lookup"><span data-stu-id="11a55-170">You can also add optional checks to verify that the user has rights to register for the requested tags.</span></span>
   
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
        // This creates or updates a registration (with provided channelURI) at the specified id
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
   
            // add check if user is allowed to add these tags
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
10. <span data-ttu-id="11a55-171">Uložte provedené změny.</span><span class="sxs-lookup"><span data-stu-id="11a55-171">Save your changes.</span></span>

## <a name="sending-notifications-from-the-webapi-backend"></a><span data-ttu-id="11a55-172">Odesílání oznámení z back-endu WebAPI</span><span class="sxs-lookup"><span data-stu-id="11a55-172">Sending Notifications from the WebAPI Backend</span></span>
<span data-ttu-id="11a55-173">V této části přidáte nový kontroler, který klientským zařízením zpřístupní možnost odesílat oznámení na základě značky uživatelského jména pomocí knihovny pro správu služby Azure Notification Hubs v back-endu ASP.NET WebAPI.</span><span class="sxs-lookup"><span data-stu-id="11a55-173">In this section you add a new controller that exposes a way for client devices to send a notification based on the username tag using Azure Notification Hubs Service Management Library in the ASP.NET WebAPI backend.</span></span>

1. <span data-ttu-id="11a55-174">Vytvořte další nový kontroler **NotificationsController**.</span><span class="sxs-lookup"><span data-stu-id="11a55-174">Create another new controller named **NotificationsController**.</span></span> <span data-ttu-id="11a55-175">Vytvořte ho stejným způsobem jako kontroler **RegisterController** v předchozí části.</span><span class="sxs-lookup"><span data-stu-id="11a55-175">Create it the same way you created the **RegisterController** in the previous section.</span></span>
2. <span data-ttu-id="11a55-176">V souboru NotificationsController.cs přidejte následující příkazy `using`:</span><span class="sxs-lookup"><span data-stu-id="11a55-176">In NotificationsController.cs, add the following `using` statements:</span></span>
   
        using AppBackend.Models;
        using System.Threading.Tasks;
        using System.Web;
3. <span data-ttu-id="11a55-177">Do třídy **NotificationsController** přidejte následující metodu.</span><span class="sxs-lookup"><span data-stu-id="11a55-177">Add the following method to the **NotificationsController** class.</span></span>
   
    <span data-ttu-id="11a55-178">Tento kód odesílá typ oznámení na základě parametru `pns` systému oznámení platformy.</span><span class="sxs-lookup"><span data-stu-id="11a55-178">This code send a notification type based on the Platform Notification Service (PNS) `pns` parameter.</span></span> <span data-ttu-id="11a55-179">Hodnota `to_tag` slouží k nastavení značky *username* (uživatelské jméno) pro zprávu.</span><span class="sxs-lookup"><span data-stu-id="11a55-179">The value of `to_tag` is used to set the *username* tag on the message.</span></span> <span data-ttu-id="11a55-180">Tato značka musí odpovídat značce uživatelského jména aktivní registrace k centru událostí.</span><span class="sxs-lookup"><span data-stu-id="11a55-180">This tag must match a username tag of an active notification hub registration.</span></span> <span data-ttu-id="11a55-181">Zpráva oznámení se přetáhne z textu požadavku POST a naformátuje se pro cílový systém oznámení platformy.</span><span class="sxs-lookup"><span data-stu-id="11a55-181">The notification message is pulled from the body of the POST request and formatted for the target PNS.</span></span> 
   
    <span data-ttu-id="11a55-182">V závislosti na systému oznámení platformy, který vaše zařízení používá k přijímání oznámení, jsou podporována různá oznámení v různých formátech.</span><span class="sxs-lookup"><span data-stu-id="11a55-182">Depending on the Platform Notification Service (PNS) that your supported devices use to receive notifications, different notifications are supported using different formats.</span></span> <span data-ttu-id="11a55-183">Například na zařízeních s Windows byste mohli použít [informační zprávu pomocí Služby nabízených oznámení Windows](https://msdn.microsoft.com/library/windows/apps/br230849.aspx), kterou ostatní systémy oznámení platformy přímo nepodporují.</span><span class="sxs-lookup"><span data-stu-id="11a55-183">For example on Windows devices, you could use a [toast notification with WNS](https://msdn.microsoft.com/library/windows/apps/br230849.aspx) that isn't directly supported by another PNS.</span></span> <span data-ttu-id="11a55-184">Takže by váš back-end musel oznámení formátovat na podporované oznámení pro systémy oznámení platformy zařízení, která chcete podporovat.</span><span class="sxs-lookup"><span data-stu-id="11a55-184">So your backend would need to format the notification into a supported notification for the PNS of devices you plan to support.</span></span> <span data-ttu-id="11a55-185">Ve [třídě NotificationHubClient](https://msdn.microsoft.com/library/azure/microsoft.azure.notificationhubs.notificationhubclient_methods.aspx) tedy použijte vhodné rozhraní API pro odesílání.</span><span class="sxs-lookup"><span data-stu-id="11a55-185">Then use the appropriate send API on the [NotificationHubClient class](https://msdn.microsoft.com/library/azure/microsoft.azure.notificationhubs.notificationhubclient_methods.aspx)</span></span>
   
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
4. <span data-ttu-id="11a55-186">Stisknutím klávesy **F5** aplikaci spusťte a ověřte, že jste zatím postupovali správně.</span><span class="sxs-lookup"><span data-stu-id="11a55-186">Press **F5** to run the application and to ensure the accuracy of your work so far.</span></span> <span data-ttu-id="11a55-187">Aplikace by měla otevřít webový prohlížeč a zobrazit domovskou stránku ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="11a55-187">The app should launch a web browser and display the ASP.NET home page.</span></span> 

## <a name="publish-the-new-webapi-backend"></a><span data-ttu-id="11a55-188">Publikování nového back-endu WebAPI</span><span class="sxs-lookup"><span data-stu-id="11a55-188">Publish the new WebAPI Backend</span></span>
1. <span data-ttu-id="11a55-189">Nyní tuto aplikaci nasadíme na web Azure, aby byla přístupná ze všech zařízení.</span><span class="sxs-lookup"><span data-stu-id="11a55-189">Now we will deploy this app to an Azure Website in order to make it accessible from all devices.</span></span> <span data-ttu-id="11a55-190">Klikněte pravým tlačítkem na projekt **AppBackend** a vyberte **Publikovat**.</span><span class="sxs-lookup"><span data-stu-id="11a55-190">Right-click on the **AppBackend** project and select **Publish**.</span></span>
2. <span data-ttu-id="11a55-191">Jako cíl publikování vyberte **Microsoft Azure App Service** a klikněte na **Publikovat**.</span><span class="sxs-lookup"><span data-stu-id="11a55-191">Select **Microsoft Azure App Service** as your publish target and click **Publish**.</span></span> <span data-ttu-id="11a55-192">Tím se otevře dialogové okno Vytvoření služby App Service, které vám pomůže vytvořit všechny prostředky Azure potřebné ke spuštění vaší webové aplikace ASP.NET v Azure.</span><span class="sxs-lookup"><span data-stu-id="11a55-192">This opens the Create App Service dialog, which helps you create all the necessary Azure resources to run the ASP.NET web app in Azure.</span></span>

    ![][B15]
3. <span data-ttu-id="11a55-193">V dialogovém okně **Vytvoření služby App Service** vyberte váš účet Azure.</span><span class="sxs-lookup"><span data-stu-id="11a55-193">In the **Create App Service** dialog, select your Azure account.</span></span> <span data-ttu-id="11a55-194">Klikněte na **Změnit typ** a vyberte **Webová aplikace**.</span><span class="sxs-lookup"><span data-stu-id="11a55-194">Click **Change Type** and select **Web App**.</span></span> <span data-ttu-id="11a55-195">Ponechte vyplněný **Název webové aplikace** a vyberte **Předplatné**, **Skupinu prostředků** a **Plán služby App Service**.</span><span class="sxs-lookup"><span data-stu-id="11a55-195">Keep the **Web App Name** given and select the **Subscription**, **Resource Group**, and **App Service Plan**.</span></span>  <span data-ttu-id="11a55-196">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="11a55-196">Click **Create**.</span></span>

4. <span data-ttu-id="11a55-197">Poznamenejte si vlastnost **Adresa URL webu** v části **Souhrn**.</span><span class="sxs-lookup"><span data-stu-id="11a55-197">Make a note of the **Site URL** property in the **Summary** section.</span></span> <span data-ttu-id="11a55-198">Na tuto adresu URL budeme odkazovat jako na *koncový bod back-endu* později v tomto kurzu.</span><span class="sxs-lookup"><span data-stu-id="11a55-198">We will refer to this URL as your *backend endpoint* later in this tutorial.</span></span> <span data-ttu-id="11a55-199">Klikněte na **Publikovat**.</span><span class="sxs-lookup"><span data-stu-id="11a55-199">Click **Publish**.</span></span>

5. <span data-ttu-id="11a55-200">Průvodce po dokončení publikuje webovou aplikaci ASP.NET do služby Azure a pak aplikaci spustí ve výchozím prohlížeči.</span><span class="sxs-lookup"><span data-stu-id="11a55-200">Once the wizard completes, it publishes the ASP.NET web app to Azure, and then launches the app in the default browser.</span></span>  <span data-ttu-id="11a55-201">Vaši aplikaci bude možné zobrazit ve službě Azure App Service.</span><span class="sxs-lookup"><span data-stu-id="11a55-201">Your application will be viewable in Azure App Services.</span></span>

<span data-ttu-id="11a55-202">Adresa URL používá dříve zadaný název webové aplikace ve formátu http://<název_aplikace>.azurewebsites.net.</span><span class="sxs-lookup"><span data-stu-id="11a55-202">The URL uses the web app name that you specified earlier, with the format http://<app_name>.azurewebsites.net.</span></span>

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
