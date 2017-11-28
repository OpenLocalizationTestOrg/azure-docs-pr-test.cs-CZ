---
title: "Azure Notification Hubs zabezpečené Push"
description: "Zjistěte, jak odeslat zabezpečené nabízená oznámení v Azure. Ukázky kódu jsou vytvořeny v C# s použitím .NET API."
documentationcenter: windows
author: ysxu
manager: erikre
editor: 
services: notification-hubs
ms.assetid: 5aef50f4-80b3-460e-a9a7-7435001273bd
ms.service: notification-hubs
ms.workload: mobile
ms.tgt_pltfrm: windows
ms.devlang: dotnet
ms.topic: article
ms.date: 06/29/2016
ms.author: yuaxu
ms.openlocfilehash: 9c626ec1534c4899588150a58c0da57b9d963f6f
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="azure-notification-hubs-secure-push"></a><span data-ttu-id="340fc-104">Azure Notification Hubs zabezpečené Push</span><span class="sxs-lookup"><span data-stu-id="340fc-104">Azure Notification Hubs Secure Push</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="340fc-105">Univerzální pro Windows</span><span class="sxs-lookup"><span data-stu-id="340fc-105">Windows Universal</span></span>](notification-hubs-aspnet-backend-windows-dotnet-wns-secure-push-notification.md)
> * [<span data-ttu-id="340fc-106">iOS</span><span class="sxs-lookup"><span data-stu-id="340fc-106">iOS</span></span>](notification-hubs-aspnet-backend-ios-push-apple-apns-secure-notification.md)
> * [<span data-ttu-id="340fc-107">Android</span><span class="sxs-lookup"><span data-stu-id="340fc-107">Android</span></span>](notification-hubs-aspnet-backend-android-secure-google-gcm-push-notification.md)
> 
> 

## <a name="overview"></a><span data-ttu-id="340fc-108">Přehled</span><span class="sxs-lookup"><span data-stu-id="340fc-108">Overview</span></span>
<span data-ttu-id="340fc-109">Podpora nabízená oznámení v Microsoft Azure umožňuje přístup k infrastruktuře snadno použitelnou, multiplatformní a upraveným nabízené, což výrazně zjednodušuje implementaci nabízená oznámení spotřebních a podnikových aplikací pro mobilní platformy.</span><span class="sxs-lookup"><span data-stu-id="340fc-109">Push notification support in Microsoft Azure enables you to access an easy-to-use, multiplatform, scaled-out push infrastructure, which greatly simplifies the implementation of push notifications for both consumer and enterprise applications for mobile platforms.</span></span>

<span data-ttu-id="340fc-110">Kvůli zákonným omezení zabezpečení, někdy aplikace může chtít zahrnout něco v oznámení, kterou nelze přenést prostřednictvím infrastrukturu pro standardní nabízená oznámení.</span><span class="sxs-lookup"><span data-stu-id="340fc-110">Due to regulatory or security constraints, sometimes an application might want to include something in the notification that cannot be transmitted through the standard push notification infrastructure.</span></span> <span data-ttu-id="340fc-111">Tento kurz popisuje, jak zajistit stejné prostředí posíláním důvěrných informací o prostřednictvím zabezpečeného a ověřené připojení mezi klientské zařízení a back-end aplikace.</span><span class="sxs-lookup"><span data-stu-id="340fc-111">This tutorial describes how to achieve the same experience by sending sensitive information through a secure, authenticated connection between the client device and the app backend.</span></span>

<span data-ttu-id="340fc-112">Na vysoké úrovni tok je následující:</span><span class="sxs-lookup"><span data-stu-id="340fc-112">At a high level, the flow is as follows:</span></span>

1. <span data-ttu-id="340fc-113">Back-end aplikace:</span><span class="sxs-lookup"><span data-stu-id="340fc-113">The app back-end:</span></span>
   * <span data-ttu-id="340fc-114">Zabezpečení datové úložiště v databázi back-end.</span><span class="sxs-lookup"><span data-stu-id="340fc-114">Stores secure payload in back-end database.</span></span>
   * <span data-ttu-id="340fc-115">ID tohoto oznámení se odešle do zařízení (zabezpečené nebudou odeslány žádné informace).</span><span class="sxs-lookup"><span data-stu-id="340fc-115">Sends the ID of this notification to the device (no secure information is sent).</span></span>
2. <span data-ttu-id="340fc-116">Aplikace na zařízení, když obdrží oznámení:</span><span class="sxs-lookup"><span data-stu-id="340fc-116">The app on the device, when receiving the notification:</span></span>
   * <span data-ttu-id="340fc-117">Zařízení kontaktuje back-end vyžaduje zabezpečené datové části.</span><span class="sxs-lookup"><span data-stu-id="340fc-117">The device contacts the back-end requesting the secure payload.</span></span>
   * <span data-ttu-id="340fc-118">Aplikace můžete zobrazit datové části jako upozornění na zařízení.</span><span class="sxs-lookup"><span data-stu-id="340fc-118">The app can show the payload as a notification on the device.</span></span>

<span data-ttu-id="340fc-119">Je důležité si uvědomit, že v předchozím toku (a v tomto kurzu) předpokládáme, že zařízení ukládá ověřovací token do místního úložiště, po přihlášení uživatele.</span><span class="sxs-lookup"><span data-stu-id="340fc-119">It is important to note that in the preceding flow (and in this tutorial), we assume that the device stores an authentication token in local storage, after the user logs in.</span></span> <span data-ttu-id="340fc-120">Zaručí se tím úplně jednoduché prostředí, protože zařízení můžete načíst pomocí tohoto tokenu zabezpečení datové na oznámení.</span><span class="sxs-lookup"><span data-stu-id="340fc-120">This guarantees a completely seamless experience, as the device can retrieve the notification’s secure payload using this token.</span></span> <span data-ttu-id="340fc-121">Pokud vaše aplikace nejsou uložené tokeny ověřování v zařízení, nebo pokud tyto tokeny můžete vypršela platnost, by měla aplikace zařízení při přijetí oznámení zobrazit obecné oznámení uživateli zobrazuje výzvu spusťte aplikaci.</span><span class="sxs-lookup"><span data-stu-id="340fc-121">If your application does not store authentication tokens on the device, or if these tokens can be expired, the device app, upon receiving the notification should display a generic notification prompting the user to launch the app.</span></span> <span data-ttu-id="340fc-122">Aplikace pak ověřuje uživatele a ukazuje datová část oznámení.</span><span class="sxs-lookup"><span data-stu-id="340fc-122">The app then authenticates the user and shows the notification payload.</span></span>

<span data-ttu-id="340fc-123">V tomto kurzu zabezpečení nabízené ukazuje, jak bezpečně odesílání nabízených oznámení.</span><span class="sxs-lookup"><span data-stu-id="340fc-123">This Secure Push tutorial shows how to send a push notification securely.</span></span> <span data-ttu-id="340fc-124">Tento kurz je založený na [upozornění uživatelů](notification-hubs-aspnet-backend-windows-dotnet-wns-notification.md) kurzu, a proto kroky musí dokončit v tomto kurzu první.</span><span class="sxs-lookup"><span data-stu-id="340fc-124">The tutorial builds on the [Notify Users](notification-hubs-aspnet-backend-windows-dotnet-wns-notification.md) tutorial, so you should complete the steps in that tutorial first.</span></span>

> [!NOTE]
> <span data-ttu-id="340fc-125">V tomto kurzu se předpokládá, že jste vytvořili a nakonfigurovali vaše Centrum oznámení, jak je popsáno v [Začínáme s Notification Hubs (pro Windows Store)](notification-hubs-windows-store-dotnet-get-started-wns-push-notification.md).</span><span class="sxs-lookup"><span data-stu-id="340fc-125">This tutorial assumes that you have created and configured your notification hub as described in [Getting Started with Notification Hubs (Windows Store)](notification-hubs-windows-store-dotnet-get-started-wns-push-notification.md).</span></span>
> <span data-ttu-id="340fc-126">Všimněte si také, že Windows Phone 8.1 vyžaduje pověření systému Windows (ne Windows Phone), a že úlohy na pozadí nefungují na Windows Phone 8.0 nebo Silverlight 8.1.</span><span class="sxs-lookup"><span data-stu-id="340fc-126">Also, note that Windows Phone 8.1 requires Windows (not Windows Phone) credentials, and that background tasks do not work on Windows Phone 8.0 or Silverlight 8.1.</span></span> <span data-ttu-id="340fc-127">Pro aplikace pro Windows Store, můžete přijímat oznámení prostřednictvím úlohy na pozadí pouze v případě, že aplikace je povoleno uzamčení obrazovky (klikněte na zaškrtávací políčko v Appmanifest).</span><span class="sxs-lookup"><span data-stu-id="340fc-127">For Windows Store applications, you can receive notifications via a background task only if the app is lock-screen enabled (click the checkbox in the Appmanifest).</span></span>
> 
> 

[!INCLUDE [notification-hubs-aspnet-backend-securepush](../../includes/notification-hubs-aspnet-backend-securepush.md)]

## <a name="modify-the-windows-phone-project"></a><span data-ttu-id="340fc-128">Upravit projektu Windows Phone</span><span class="sxs-lookup"><span data-stu-id="340fc-128">Modify the Windows Phone Project</span></span>
1. <span data-ttu-id="340fc-129">V **NotifyUserWindowsPhone** projekt, přidejte následující kód do souboru App.xaml.cs k registraci úlohy na pozadí push.</span><span class="sxs-lookup"><span data-stu-id="340fc-129">In the **NotifyUserWindowsPhone** project, add the following code to App.xaml.cs to register the push background task.</span></span> <span data-ttu-id="340fc-130">Přidejte následující řádek kódu na konci `OnLaunched()` metoda:</span><span class="sxs-lookup"><span data-stu-id="340fc-130">Add the following line of code at the end of the `OnLaunched()` method:</span></span>
   
        RegisterBackgroundTask();
2. <span data-ttu-id="340fc-131">Stále v souboru App.xaml.cs přidejte následující kód bezprostředně po `OnLaunched()` metoda:</span><span class="sxs-lookup"><span data-stu-id="340fc-131">Still in App.xaml.cs, add the following code immediately after the `OnLaunched()` method:</span></span>
   
        private async void RegisterBackgroundTask()
        {
            if (!Windows.ApplicationModel.Background.BackgroundTaskRegistration.AllTasks.Any(i => i.Value.Name == "PushBackgroundTask"))
            {
                var result = await BackgroundExecutionManager.RequestAccessAsync();
                var builder = new BackgroundTaskBuilder();
   
                builder.Name = "PushBackgroundTask";
                builder.TaskEntryPoint = typeof(PushBackgroundComponent.PushBackgroundTask).FullName;
                builder.SetTrigger(new Windows.ApplicationModel.Background.PushNotificationTrigger());
                BackgroundTaskRegistration task = builder.Register();
            }
        }
3. <span data-ttu-id="340fc-132">Přidejte následující `using` příkazy v horní části souboru App.xaml.cs:</span><span class="sxs-lookup"><span data-stu-id="340fc-132">Add the following `using` statements at the top of the App.xaml.cs file:</span></span>
   
        using Windows.Networking.PushNotifications;
        using Windows.ApplicationModel.Background;
4. <span data-ttu-id="340fc-133">Ve Visual Studiu zvolte v nabídce **Soubor** možnost **Uložit vše**.</span><span class="sxs-lookup"><span data-stu-id="340fc-133">From the **File** menu in Visual Studio, click **Save All**.</span></span>

## <a name="create-the-push-background-component"></a><span data-ttu-id="340fc-134">Vytvořit komponentu nabízené pozadí</span><span class="sxs-lookup"><span data-stu-id="340fc-134">Create the Push Background Component</span></span>
<span data-ttu-id="340fc-135">Dalším krokem je vytvoření komponentu nabízených pozadí.</span><span class="sxs-lookup"><span data-stu-id="340fc-135">The next step is to create the push background component.</span></span>

1. <span data-ttu-id="340fc-136">V Průzkumníku řešení klikněte pravým tlačítkem na uzel na nejvyšší úrovni řešení (**řešení SecurePush** v tomto případě), pak klikněte na tlačítko **přidat**, pak klikněte na tlačítko **nový projekt**.</span><span class="sxs-lookup"><span data-stu-id="340fc-136">In Solution Explorer, right-click the top-level node of the solution (**Solution SecurePush** in this case), then click **Add**, then click **New Project**.</span></span>
2. <span data-ttu-id="340fc-137">Rozbalte položku **aplikacích pro Store**, pak klikněte na tlačítko **aplikace Windows Phone**, pak klikněte na tlačítko **komponenty prostředí Windows Runtime (Windows Phone)**.</span><span class="sxs-lookup"><span data-stu-id="340fc-137">Expand **Store Apps**, then click **Windows Phone Apps**, then click **Windows Runtime Component (Windows Phone)**.</span></span> <span data-ttu-id="340fc-138">Název projektu **PushBackgroundComponent**a potom klikněte na **OK** a vytvořte tak projekt.</span><span class="sxs-lookup"><span data-stu-id="340fc-138">Name the project **PushBackgroundComponent**, and then click **OK** to create the project.</span></span>
   
    ![][12]
3. <span data-ttu-id="340fc-139">V Průzkumníku řešení klikněte pravým tlačítkem myši **PushBackgroundComponent (Windows Phone 8.1)** projektu a pak klikněte na **přidat**, pak klikněte na tlačítko **třída**.</span><span class="sxs-lookup"><span data-stu-id="340fc-139">In Solution Explorer, right-click the **PushBackgroundComponent (Windows Phone 8.1)** project, then click **Add**, then click **Class**.</span></span> <span data-ttu-id="340fc-140">Pojmenujte novou třídu **PushBackgroundTask.cs**.</span><span class="sxs-lookup"><span data-stu-id="340fc-140">Name the new class **PushBackgroundTask.cs**.</span></span> <span data-ttu-id="340fc-141">Klikněte na tlačítko **přidat** ke generování třídy.</span><span class="sxs-lookup"><span data-stu-id="340fc-141">Click **Add** to generate the class.</span></span>
4. <span data-ttu-id="340fc-142">Nahradí celý obsah **PushBackgroundComponent** definici oboru názvů následujícím kódem, nahraďte zástupný symbol `{back-end endpoint}` s koncovým bodem back-end získali při nasazování back-end:</span><span class="sxs-lookup"><span data-stu-id="340fc-142">Replace the entire contents of the **PushBackgroundComponent** namespace definition with the following code, substituting the placeholder `{back-end endpoint}` with the back-end endpoint obtained while deploying your back-end:</span></span>
   
        public sealed class Notification
            {
                public int Id { get; set; }
                public string Payload { get; set; }
                public bool Read { get; set; }
            }
   
            public sealed class PushBackgroundTask : IBackgroundTask
            {
                private string GET_URL = "{back-end endpoint}/api/notifications/";
   
                async void IBackgroundTask.Run(IBackgroundTaskInstance taskInstance)
                {
                    // Store the content received from the notification so it can be retrieved from the UI.
                    RawNotification raw = (RawNotification)taskInstance.TriggerDetails;
                    var notificationId = raw.Content;
   
                    // retrieve content
                    BackgroundTaskDeferral deferral = taskInstance.GetDeferral();
                    var httpClient = new HttpClient();
                    var settings = ApplicationData.Current.LocalSettings.Values;
                    httpClient.DefaultRequestHeaders.Authorization = new AuthenticationHeaderValue("Basic", (string)settings["AuthenticationToken"]);
   
                    var notificationString = await httpClient.GetStringAsync(GET_URL + notificationId);
   
                    var notification = JsonConvert.DeserializeObject<Notification>(notificationString);
   
                    ShowToast(notification);
   
                    deferral.Complete();
                }
   
                private void ShowToast(Notification notification)
                {
                    ToastTemplateType toastTemplate = ToastTemplateType.ToastText01;
                    XmlDocument toastXml = ToastNotificationManager.GetTemplateContent(toastTemplate);
                    XmlNodeList toastTextElements = toastXml.GetElementsByTagName("text");
                    toastTextElements[0].AppendChild(toastXml.CreateTextNode(notification.Payload));
                    ToastNotification toast = new ToastNotification(toastXml);
                    ToastNotificationManager.CreateToastNotifier().Show(toast);
                }
            }
5. <span data-ttu-id="340fc-143">V Průzkumníku řešení klikněte pravým tlačítkem myši **PushBackgroundComponent (Windows Phone 8.1)** projektu a pak klikněte na **spravovat balíčky NuGet**.</span><span class="sxs-lookup"><span data-stu-id="340fc-143">In Solution Explorer, right-click the **PushBackgroundComponent (Windows Phone 8.1)** project and then click **Manage NuGet Packages**.</span></span>
6. <span data-ttu-id="340fc-144">Na levé straně klikněte na **Online**.</span><span class="sxs-lookup"><span data-stu-id="340fc-144">On the left-hand side, click **Online**.</span></span>
7. <span data-ttu-id="340fc-145">V **vyhledávání** zadejte **klienta Http**.</span><span class="sxs-lookup"><span data-stu-id="340fc-145">In the **Search** box, type **Http Client**.</span></span>
8. <span data-ttu-id="340fc-146">V seznamu výsledků klepněte na **knihovny klienta HTTP Microsoft**a potom klikněte na **nainstalovat**.</span><span class="sxs-lookup"><span data-stu-id="340fc-146">In the results list, click **Microsoft HTTP Client Libraries**, and then click **Install**.</span></span> <span data-ttu-id="340fc-147">Dokončete instalaci.</span><span class="sxs-lookup"><span data-stu-id="340fc-147">Complete the installation.</span></span>
9. <span data-ttu-id="340fc-148">Zpět v NuGet **vyhledávání** zadejte **Json.net**.</span><span class="sxs-lookup"><span data-stu-id="340fc-148">Back in the NuGet **Search** box, type **Json.net**.</span></span> <span data-ttu-id="340fc-149">Nainstalujte **Json.NET** balíček a potom zavřete okno Správce balíčků NuGet.</span><span class="sxs-lookup"><span data-stu-id="340fc-149">Install the **Json.NET** package, then close the NuGet Package Manager window.</span></span>
10. <span data-ttu-id="340fc-150">Přidejte následující `using` příkazy v horní části **PushBackgroundTask.cs** souboru:</span><span class="sxs-lookup"><span data-stu-id="340fc-150">Add the following `using` statements at the top of the **PushBackgroundTask.cs** file:</span></span>
    
        using Windows.ApplicationModel.Background;
        using Windows.Networking.PushNotifications;
        using System.Net.Http;
        using Windows.Storage;
        using System.Net.Http.Headers;
        using Newtonsoft.Json;
        using Windows.UI.Notifications;
        using Windows.Data.Xml.Dom;
11. <span data-ttu-id="340fc-151">V Průzkumníku řešení v **NotifyUserWindowsPhone (Windows Phone 8.1)** projektu, klikněte pravým tlačítkem na **odkazy**, pak klikněte na tlačítko **přidat odkaz na...** .</span><span class="sxs-lookup"><span data-stu-id="340fc-151">In Solution Explorer, in the **NotifyUserWindowsPhone (Windows Phone 8.1)** project, right-click **References**, then click **Add Reference...**.</span></span> <span data-ttu-id="340fc-152">V dialogovém okně Správce odkazů, zaškrtněte políčko vedle **PushBackgroundComponent**a potom klikněte na **OK**.</span><span class="sxs-lookup"><span data-stu-id="340fc-152">In the Reference Manager dialog, check the box next to **PushBackgroundComponent**, and then click **OK**.</span></span>
12. <span data-ttu-id="340fc-153">V Průzkumníku řešení klikněte dvakrát na **Package.appxmanifest** v **NotifyUserWindowsPhone (Windows Phone 8.1)** projektu.</span><span class="sxs-lookup"><span data-stu-id="340fc-153">In Solution Explorer, double-click **Package.appxmanifest** in the **NotifyUserWindowsPhone (Windows Phone 8.1)** project.</span></span> <span data-ttu-id="340fc-154">V části **oznámení**, nastavte **informační podporující** k **Ano**.</span><span class="sxs-lookup"><span data-stu-id="340fc-154">Under **Notifications**, set **Toast Capable** to **Yes**.</span></span>
    
    ![][3]
13. <span data-ttu-id="340fc-155">Pořád ještě v **Package.appxmanifest**, klikněte **deklarace** v horní nabídce.</span><span class="sxs-lookup"><span data-stu-id="340fc-155">Still in **Package.appxmanifest**, click the **Declarations** menu near the top.</span></span> <span data-ttu-id="340fc-156">V **dostupné deklarace** rozevíracího seznamu, klikněte na tlačítko **úlohy na pozadí**a potom klikněte na **přidat**.</span><span class="sxs-lookup"><span data-stu-id="340fc-156">In the **Available Declarations** dropdown, click **Background Tasks**, and then click **Add**.</span></span>
14. <span data-ttu-id="340fc-157">V **Package.appxmanifest**v části **vlastnosti**, zkontrolujte **nabízená oznámení**.</span><span class="sxs-lookup"><span data-stu-id="340fc-157">In **Package.appxmanifest**, under **Properties**, check **Push notification**.</span></span>
15. <span data-ttu-id="340fc-158">V **Package.appxmanifest**v části **nastavení aplikace**, typ **PushBackgroundComponent.PushBackgroundTask** v **vstupní bod** pole.</span><span class="sxs-lookup"><span data-stu-id="340fc-158">In **Package.appxmanifest**, under **App Settings**, type **PushBackgroundComponent.PushBackgroundTask** in the **Entry Point** field.</span></span>
    
    ![][13]
16. <span data-ttu-id="340fc-159">V nabídce **Soubor** klikněte na **Uložit vše**.</span><span class="sxs-lookup"><span data-stu-id="340fc-159">From the **File** menu, click **Save All**.</span></span>

## <a name="run-the-application"></a><span data-ttu-id="340fc-160">Spuštění aplikace</span><span class="sxs-lookup"><span data-stu-id="340fc-160">Run the Application</span></span>
<span data-ttu-id="340fc-161">Ke spuštění aplikace, postupujte takto:</span><span class="sxs-lookup"><span data-stu-id="340fc-161">To run the application, do the following:</span></span>

1. <span data-ttu-id="340fc-162">V sadě Visual Studio, spusťte **AppBackend** aplikace webového rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="340fc-162">In Visual Studio, run the **AppBackend** Web API application.</span></span> <span data-ttu-id="340fc-163">Zobrazí se webová stránka ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="340fc-163">An ASP.NET web page is displayed.</span></span>
2. <span data-ttu-id="340fc-164">V sadě Visual Studio, spusťte **NotifyUserWindowsPhone (Windows Phone 8.1)** aplikace Windows Phone.</span><span class="sxs-lookup"><span data-stu-id="340fc-164">In Visual Studio, run the **NotifyUserWindowsPhone (Windows Phone 8.1)** Windows Phone app.</span></span> <span data-ttu-id="340fc-165">Emulátor Windows Phone spustí a automaticky načte aplikaci.</span><span class="sxs-lookup"><span data-stu-id="340fc-165">The Windows Phone emulator runs and loads the app automatically.</span></span>
3. <span data-ttu-id="340fc-166">V **NotifyUserWindowsPhone** aplikace uživatelského rozhraní, zadejte uživatelské jméno a heslo.</span><span class="sxs-lookup"><span data-stu-id="340fc-166">In the **NotifyUserWindowsPhone** app UI, enter a username and password.</span></span> <span data-ttu-id="340fc-167">Mohou to být libovolný řetězec, ale musí být stejnou hodnotu.</span><span class="sxs-lookup"><span data-stu-id="340fc-167">These can be any string, but they must be the same value.</span></span>
4. <span data-ttu-id="340fc-168">V **NotifyUserWindowsPhone** aplikace uživatelského rozhraní, klikněte na tlačítko **přihlášení a registrace**.</span><span class="sxs-lookup"><span data-stu-id="340fc-168">In the **NotifyUserWindowsPhone** app UI, click **Log in and register**.</span></span> <span data-ttu-id="340fc-169">Pak klikněte na tlačítko **odeslat nabízené**.</span><span class="sxs-lookup"><span data-stu-id="340fc-169">Then click **Send push**.</span></span>

[3]: ./media/notification-hubs-aspnet-backend-windows-dotnet-secure-push/notification-hubs-secure-push3.png
[12]: ./media/notification-hubs-aspnet-backend-windows-dotnet-secure-push/notification-hubs-secure-push12.png
[13]: ./media/notification-hubs-aspnet-backend-windows-dotnet-secure-push/notification-hubs-secure-push13.png
