---
title: "aaaAzure oznámení Centra zabezpečení Push."
description: "Zjistěte, jak toosend zabezpečené nabízená oznámení v Azure. Ukázky kódu jsou vytvořené v C# s použitím hello .NET API."
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
ms.openlocfilehash: b6fe16c96d28d75ff698278409fb012472ba6396
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-notification-hubs-secure-push"></a><span data-ttu-id="2f233-104">Azure Notification Hubs zabezpečené Push</span><span class="sxs-lookup"><span data-stu-id="2f233-104">Azure Notification Hubs Secure Push</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="2f233-105">Univerzální pro Windows</span><span class="sxs-lookup"><span data-stu-id="2f233-105">Windows Universal</span></span>](notification-hubs-aspnet-backend-windows-dotnet-wns-secure-push-notification.md)
> * [<span data-ttu-id="2f233-106">iOS</span><span class="sxs-lookup"><span data-stu-id="2f233-106">iOS</span></span>](notification-hubs-aspnet-backend-ios-push-apple-apns-secure-notification.md)
> * [<span data-ttu-id="2f233-107">Android</span><span class="sxs-lookup"><span data-stu-id="2f233-107">Android</span></span>](notification-hubs-aspnet-backend-android-secure-google-gcm-push-notification.md)
> 
> 

## <a name="overview"></a><span data-ttu-id="2f233-108">Přehled</span><span class="sxs-lookup"><span data-stu-id="2f233-108">Overview</span></span>
<span data-ttu-id="2f233-109">Podpora nabízená oznámení v Microsoft Azure umožňuje tooaccess nabízené snadno použitelnou, multiplatformní a škálovanou infrastrukturu, což výrazně zjednodušuje hello implementace nabízených oznámení spotřebních a podnikových aplikací pro mobilní platformy.</span><span class="sxs-lookup"><span data-stu-id="2f233-109">Push notification support in Microsoft Azure enables you tooaccess an easy-to-use, multiplatform, scaled-out push infrastructure, which greatly simplifies hello implementation of push notifications for both consumer and enterprise applications for mobile platforms.</span></span>

<span data-ttu-id="2f233-110">Z důvodu omezení tooregulatory nebo zabezpečení někdy aplikace může být vhodné tooinclude něco v hello oznámení, kterou nelze přenést prostřednictvím infrastrukturu pro hello standardní nabízená oznámení.</span><span class="sxs-lookup"><span data-stu-id="2f233-110">Due tooregulatory or security constraints, sometimes an application might want tooinclude something in hello notification that cannot be transmitted through hello standard push notification infrastructure.</span></span> <span data-ttu-id="2f233-111">Tento kurz popisuje, jak tooachieve hello stejné prostředí posíláním důvěrných informací o prostřednictvím zabezpečeného a ověřené připojení mezi hello klientského zařízení a back-end aplikace hello.</span><span class="sxs-lookup"><span data-stu-id="2f233-111">This tutorial describes how tooachieve hello same experience by sending sensitive information through a secure, authenticated connection between hello client device and hello app backend.</span></span>

<span data-ttu-id="2f233-112">Na vysoké úrovni tok hello vypadá takto:</span><span class="sxs-lookup"><span data-stu-id="2f233-112">At a high level, hello flow is as follows:</span></span>

1. <span data-ttu-id="2f233-113">back-end Hello aplikace:</span><span class="sxs-lookup"><span data-stu-id="2f233-113">hello app back-end:</span></span>
   * <span data-ttu-id="2f233-114">Zabezpečení datové úložiště v databázi back-end.</span><span class="sxs-lookup"><span data-stu-id="2f233-114">Stores secure payload in back-end database.</span></span>
   * <span data-ttu-id="2f233-115">Odešle hello ID tohoto zařízení toohello oznámení (zabezpečené nebudou odeslány žádné informace).</span><span class="sxs-lookup"><span data-stu-id="2f233-115">Sends hello ID of this notification toohello device (no secure information is sent).</span></span>
2. <span data-ttu-id="2f233-116">aplikace Hello na hello zařízení při přijetí oznámení hello:</span><span class="sxs-lookup"><span data-stu-id="2f233-116">hello app on hello device, when receiving hello notification:</span></span>
   * <span data-ttu-id="2f233-117">Hello zařízení kontaktuje hello back-end žádajícího hello zabezpečené datové části.</span><span class="sxs-lookup"><span data-stu-id="2f233-117">hello device contacts hello back-end requesting hello secure payload.</span></span>
   * <span data-ttu-id="2f233-118">Hello aplikace můžete zobrazit datové části hello jako upozornění na hello zařízení.</span><span class="sxs-lookup"><span data-stu-id="2f233-118">hello app can show hello payload as a notification on hello device.</span></span>

<span data-ttu-id="2f233-119">Je důležité, že toonote, v předchozím toku hello (a v tomto kurzu) předpokládáme, že hello zařízení ukládá ověřovací token do místního úložiště, po přihlášení uživatele hello.</span><span class="sxs-lookup"><span data-stu-id="2f233-119">It is important toonote that in hello preceding flow (and in this tutorial), we assume that hello device stores an authentication token in local storage, after hello user logs in.</span></span> <span data-ttu-id="2f233-120">Zaručí se tím úplně jednoduché prostředí, protože hello zařízení můžete načíst pomocí tohoto tokenu zabezpečení datové hello oznámení.</span><span class="sxs-lookup"><span data-stu-id="2f233-120">This guarantees a completely seamless experience, as hello device can retrieve hello notification’s secure payload using this token.</span></span> <span data-ttu-id="2f233-121">Pokud vaše aplikace nejsou uložené ověřovací tokeny na hello zařízení nebo pokud tyto tokeny můžete vypršela platnost, hello aplikaci zařízení při přijetí oznámení hello by měl zobrazit obecné oznámení výzvy hello uživatele toolaunch hello aplikace.</span><span class="sxs-lookup"><span data-stu-id="2f233-121">If your application does not store authentication tokens on hello device, or if these tokens can be expired, hello device app, upon receiving hello notification should display a generic notification prompting hello user toolaunch hello app.</span></span> <span data-ttu-id="2f233-122">aplikace Hello pak ověřuje uživatele hello a ukazuje datová část oznámení hello.</span><span class="sxs-lookup"><span data-stu-id="2f233-122">hello app then authenticates hello user and shows hello notification payload.</span></span>

<span data-ttu-id="2f233-123">Tento kurz zabezpečení nabízené ukazuje, jak toosend nabízených oznámení bezpečně.</span><span class="sxs-lookup"><span data-stu-id="2f233-123">This Secure Push tutorial shows how toosend a push notification securely.</span></span> <span data-ttu-id="2f233-124">Hello kurzu vychází hello [upozornění uživatelů](notification-hubs-aspnet-backend-windows-dotnet-wns-notification.md) kurzu, a proto hello kroky musí dokončit v tomto kurzu první.</span><span class="sxs-lookup"><span data-stu-id="2f233-124">hello tutorial builds on hello [Notify Users](notification-hubs-aspnet-backend-windows-dotnet-wns-notification.md) tutorial, so you should complete hello steps in that tutorial first.</span></span>

> [!NOTE]
> <span data-ttu-id="2f233-125">V tomto kurzu se předpokládá, že jste vytvořili a nakonfigurovali vaše Centrum oznámení, jak je popsáno v [Začínáme s Notification Hubs (pro Windows Store)](notification-hubs-windows-store-dotnet-get-started-wns-push-notification.md).</span><span class="sxs-lookup"><span data-stu-id="2f233-125">This tutorial assumes that you have created and configured your notification hub as described in [Getting Started with Notification Hubs (Windows Store)](notification-hubs-windows-store-dotnet-get-started-wns-push-notification.md).</span></span>
> <span data-ttu-id="2f233-126">Všimněte si také, že Windows Phone 8.1 vyžaduje pověření systému Windows (ne Windows Phone), a že úlohy na pozadí nefungují na Windows Phone 8.0 nebo Silverlight 8.1.</span><span class="sxs-lookup"><span data-stu-id="2f233-126">Also, note that Windows Phone 8.1 requires Windows (not Windows Phone) credentials, and that background tasks do not work on Windows Phone 8.0 or Silverlight 8.1.</span></span> <span data-ttu-id="2f233-127">Pro aplikace pro Windows Store, můžete přijímat oznámení prostřednictvím úlohy na pozadí pouze v případě, že je povoleno uzamčení obrazovky aplikace hello (klikněte na zaškrtávací políčko hello v hello Appmanifest).</span><span class="sxs-lookup"><span data-stu-id="2f233-127">For Windows Store applications, you can receive notifications via a background task only if hello app is lock-screen enabled (click hello checkbox in hello Appmanifest).</span></span>
> 
> 

[!INCLUDE [notification-hubs-aspnet-backend-securepush](../../includes/notification-hubs-aspnet-backend-securepush.md)]

## <a name="modify-hello-windows-phone-project"></a><span data-ttu-id="2f233-128">Upravit hello projektu Windows Phone</span><span class="sxs-lookup"><span data-stu-id="2f233-128">Modify hello Windows Phone Project</span></span>
1. <span data-ttu-id="2f233-129">V hello **NotifyUserWindowsPhone** projekt, přidejte následující kód tooApp.xaml.cs tooregister hello nabízené pozadí úloh hello.</span><span class="sxs-lookup"><span data-stu-id="2f233-129">In hello **NotifyUserWindowsPhone** project, add hello following code tooApp.xaml.cs tooregister hello push background task.</span></span> <span data-ttu-id="2f233-130">Přidejte následující řádek kódu na konci hello hello hello `OnLaunched()` metoda:</span><span class="sxs-lookup"><span data-stu-id="2f233-130">Add hello following line of code at hello end of hello `OnLaunched()` method:</span></span>
   
        RegisterBackgroundTask();
2. <span data-ttu-id="2f233-131">Stále v souboru App.xaml.cs přidejte následující kód bezprostředně po hello hello `OnLaunched()` metoda:</span><span class="sxs-lookup"><span data-stu-id="2f233-131">Still in App.xaml.cs, add hello following code immediately after hello `OnLaunched()` method:</span></span>
   
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
3. <span data-ttu-id="2f233-132">Přidejte následující hello `using` příkazy hello horní části souboru App.xaml.cs hello:</span><span class="sxs-lookup"><span data-stu-id="2f233-132">Add hello following `using` statements at hello top of hello App.xaml.cs file:</span></span>
   
        using Windows.Networking.PushNotifications;
        using Windows.ApplicationModel.Background;
4. <span data-ttu-id="2f233-133">Z hello **soubor** nabídky v sadě Visual Studio, klikněte na tlačítko **Uložit vše**.</span><span class="sxs-lookup"><span data-stu-id="2f233-133">From hello **File** menu in Visual Studio, click **Save All**.</span></span>

## <a name="create-hello-push-background-component"></a><span data-ttu-id="2f233-134">Vytvoření hello Push pozadí součásti</span><span class="sxs-lookup"><span data-stu-id="2f233-134">Create hello Push Background Component</span></span>
<span data-ttu-id="2f233-135">dalším krokem Hello je toocreate hello nabízené pozadí součásti.</span><span class="sxs-lookup"><span data-stu-id="2f233-135">hello next step is toocreate hello push background component.</span></span>

1. <span data-ttu-id="2f233-136">V Průzkumníku řešení klikněte pravým tlačítkem na uzel nejvyšší úrovně hello hello řešení (**řešení SecurePush** v tomto případě), pak klikněte na tlačítko **přidat**, pak klikněte na tlačítko **nový projekt**.</span><span class="sxs-lookup"><span data-stu-id="2f233-136">In Solution Explorer, right-click hello top-level node of hello solution (**Solution SecurePush** in this case), then click **Add**, then click **New Project**.</span></span>
2. <span data-ttu-id="2f233-137">Rozbalte položku **aplikacích pro Store**, pak klikněte na tlačítko **aplikace Windows Phone**, pak klikněte na tlačítko **komponenty prostředí Windows Runtime (Windows Phone)**.</span><span class="sxs-lookup"><span data-stu-id="2f233-137">Expand **Store Apps**, then click **Windows Phone Apps**, then click **Windows Runtime Component (Windows Phone)**.</span></span> <span data-ttu-id="2f233-138">Název projektu hello **PushBackgroundComponent**a potom klikněte na **OK** toocreate hello projektu.</span><span class="sxs-lookup"><span data-stu-id="2f233-138">Name hello project **PushBackgroundComponent**, and then click **OK** toocreate hello project.</span></span>
   
    ![][12]
3. <span data-ttu-id="2f233-139">V Průzkumníku řešení klikněte pravým tlačítkem na hello **PushBackgroundComponent (Windows Phone 8.1)** projektu a pak klikněte na **přidat**, pak klikněte na tlačítko **třída**.</span><span class="sxs-lookup"><span data-stu-id="2f233-139">In Solution Explorer, right-click hello **PushBackgroundComponent (Windows Phone 8.1)** project, then click **Add**, then click **Class**.</span></span> <span data-ttu-id="2f233-140">Pojmenujte novou třídu hello **PushBackgroundTask.cs**.</span><span class="sxs-lookup"><span data-stu-id="2f233-140">Name hello new class **PushBackgroundTask.cs**.</span></span> <span data-ttu-id="2f233-141">Klikněte na tlačítko **přidat** toogenerate hello třídy.</span><span class="sxs-lookup"><span data-stu-id="2f233-141">Click **Add** toogenerate hello class.</span></span>
4. <span data-ttu-id="2f233-142">Nahraďte hello celý obsah hello **PushBackgroundComponent** definici oboru názvů s hello následující kód, nahraďte zástupný symbol hello `{back-end endpoint}` s koncovým bodem back-end hello získali při nasazení vaší back-end:</span><span class="sxs-lookup"><span data-stu-id="2f233-142">Replace hello entire contents of hello **PushBackgroundComponent** namespace definition with hello following code, substituting hello placeholder `{back-end endpoint}` with hello back-end endpoint obtained while deploying your back-end:</span></span>
   
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
                    // Store hello content received from hello notification so it can be retrieved from hello UI.
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
5. <span data-ttu-id="2f233-143">V Průzkumníku řešení klikněte pravým tlačítkem na hello **PushBackgroundComponent (Windows Phone 8.1)** projektu a pak klikněte na **spravovat balíčky NuGet**.</span><span class="sxs-lookup"><span data-stu-id="2f233-143">In Solution Explorer, right-click hello **PushBackgroundComponent (Windows Phone 8.1)** project and then click **Manage NuGet Packages**.</span></span>
6. <span data-ttu-id="2f233-144">Na levé straně hello, klikněte na tlačítko **Online**.</span><span class="sxs-lookup"><span data-stu-id="2f233-144">On hello left-hand side, click **Online**.</span></span>
7. <span data-ttu-id="2f233-145">V hello **vyhledávání** zadejte **klienta Http**.</span><span class="sxs-lookup"><span data-stu-id="2f233-145">In hello **Search** box, type **Http Client**.</span></span>
8. <span data-ttu-id="2f233-146">V seznamu výsledků hello, klikněte na tlačítko **knihovny klienta HTTP Microsoft**a potom klikněte na **nainstalovat**.</span><span class="sxs-lookup"><span data-stu-id="2f233-146">In hello results list, click **Microsoft HTTP Client Libraries**, and then click **Install**.</span></span> <span data-ttu-id="2f233-147">Dokončení instalace hello.</span><span class="sxs-lookup"><span data-stu-id="2f233-147">Complete hello installation.</span></span>
9. <span data-ttu-id="2f233-148">Zpět v hello NuGet **vyhledávání** zadejte **Json.net**.</span><span class="sxs-lookup"><span data-stu-id="2f233-148">Back in hello NuGet **Search** box, type **Json.net**.</span></span> <span data-ttu-id="2f233-149">Nainstalujte hello **Json.NET** balíček a potom hello zavřít okno Správce balíčků NuGet.</span><span class="sxs-lookup"><span data-stu-id="2f233-149">Install hello **Json.NET** package, then close hello NuGet Package Manager window.</span></span>
10. <span data-ttu-id="2f233-150">Přidejte následující hello `using` příkazy hello horní části hello **PushBackgroundTask.cs** souboru:</span><span class="sxs-lookup"><span data-stu-id="2f233-150">Add hello following `using` statements at hello top of hello **PushBackgroundTask.cs** file:</span></span>
    
        using Windows.ApplicationModel.Background;
        using Windows.Networking.PushNotifications;
        using System.Net.Http;
        using Windows.Storage;
        using System.Net.Http.Headers;
        using Newtonsoft.Json;
        using Windows.UI.Notifications;
        using Windows.Data.Xml.Dom;
11. <span data-ttu-id="2f233-151">V Průzkumníku řešení v hello **NotifyUserWindowsPhone (Windows Phone 8.1)** projektu, klikněte pravým tlačítkem na **odkazy**, pak klikněte na tlačítko **přidat odkaz na...** . V dialogovém okně hello správce odkazů, políčko hello vedle příliš**PushBackgroundComponent**a potom klikněte na **OK**.</span><span class="sxs-lookup"><span data-stu-id="2f233-151">In Solution Explorer, in hello **NotifyUserWindowsPhone (Windows Phone 8.1)** project, right-click **References**, then click **Add Reference...**. In hello Reference Manager dialog, check hello box next too**PushBackgroundComponent**, and then click **OK**.</span></span>
12. <span data-ttu-id="2f233-152">V Průzkumníku řešení klikněte dvakrát na **Package.appxmanifest** v hello **NotifyUserWindowsPhone (Windows Phone 8.1)** projektu.</span><span class="sxs-lookup"><span data-stu-id="2f233-152">In Solution Explorer, double-click **Package.appxmanifest** in hello **NotifyUserWindowsPhone (Windows Phone 8.1)** project.</span></span> <span data-ttu-id="2f233-153">V části **oznámení**, nastavte **informační podporující** příliš**Ano**.</span><span class="sxs-lookup"><span data-stu-id="2f233-153">Under **Notifications**, set **Toast Capable** too**Yes**.</span></span>
    
    ![][3]
13. <span data-ttu-id="2f233-154">Pořád ještě v **Package.appxmanifest**, klikněte na tlačítko hello **deklarace** nabídce v horní hello.</span><span class="sxs-lookup"><span data-stu-id="2f233-154">Still in **Package.appxmanifest**, click hello **Declarations** menu near hello top.</span></span> <span data-ttu-id="2f233-155">V hello **dostupné deklarace** rozevíracího seznamu, klikněte na tlačítko **úlohy na pozadí**a potom klikněte na **přidat**.</span><span class="sxs-lookup"><span data-stu-id="2f233-155">In hello **Available Declarations** dropdown, click **Background Tasks**, and then click **Add**.</span></span>
14. <span data-ttu-id="2f233-156">V **Package.appxmanifest**v části **vlastnosti**, zkontrolujte **nabízená oznámení**.</span><span class="sxs-lookup"><span data-stu-id="2f233-156">In **Package.appxmanifest**, under **Properties**, check **Push notification**.</span></span>
15. <span data-ttu-id="2f233-157">V **Package.appxmanifest**v části **nastavení aplikace**, typ **PushBackgroundComponent.PushBackgroundTask** v hello **vstupní bod** pole.</span><span class="sxs-lookup"><span data-stu-id="2f233-157">In **Package.appxmanifest**, under **App Settings**, type **PushBackgroundComponent.PushBackgroundTask** in hello **Entry Point** field.</span></span>
    
    ![][13]
16. <span data-ttu-id="2f233-158">Z hello **soubor** nabídky, klikněte na tlačítko **Uložit vše**.</span><span class="sxs-lookup"><span data-stu-id="2f233-158">From hello **File** menu, click **Save All**.</span></span>

## <a name="run-hello-application"></a><span data-ttu-id="2f233-159">Spustit hello aplikace</span><span class="sxs-lookup"><span data-stu-id="2f233-159">Run hello Application</span></span>
<span data-ttu-id="2f233-160">toorun hello aplikace, hello následující:</span><span class="sxs-lookup"><span data-stu-id="2f233-160">toorun hello application, do hello following:</span></span>

1. <span data-ttu-id="2f233-161">V sadě Visual Studio spustit hello **AppBackend** aplikace webového rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="2f233-161">In Visual Studio, run hello **AppBackend** Web API application.</span></span> <span data-ttu-id="2f233-162">Zobrazí se webová stránka ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="2f233-162">An ASP.NET web page is displayed.</span></span>
2. <span data-ttu-id="2f233-163">V sadě Visual Studio spustit hello **NotifyUserWindowsPhone (Windows Phone 8.1)** aplikace Windows Phone.</span><span class="sxs-lookup"><span data-stu-id="2f233-163">In Visual Studio, run hello **NotifyUserWindowsPhone (Windows Phone 8.1)** Windows Phone app.</span></span> <span data-ttu-id="2f233-164">emulátor Windows Phone Hello spustí a automaticky načte aplikace hello.</span><span class="sxs-lookup"><span data-stu-id="2f233-164">hello Windows Phone emulator runs and loads hello app automatically.</span></span>
3. <span data-ttu-id="2f233-165">V hello **NotifyUserWindowsPhone** aplikace uživatelského rozhraní, zadejte uživatelské jméno a heslo.</span><span class="sxs-lookup"><span data-stu-id="2f233-165">In hello **NotifyUserWindowsPhone** app UI, enter a username and password.</span></span> <span data-ttu-id="2f233-166">Mohou to být libovolný řetězec, ale musí být hello stejnou hodnotu.</span><span class="sxs-lookup"><span data-stu-id="2f233-166">These can be any string, but they must be hello same value.</span></span>
4. <span data-ttu-id="2f233-167">V hello **NotifyUserWindowsPhone** aplikace uživatelského rozhraní, klikněte na tlačítko **přihlášení a registrace**.</span><span class="sxs-lookup"><span data-stu-id="2f233-167">In hello **NotifyUserWindowsPhone** app UI, click **Log in and register**.</span></span> <span data-ttu-id="2f233-168">Pak klikněte na tlačítko **odeslat nabízené**.</span><span class="sxs-lookup"><span data-stu-id="2f233-168">Then click **Send push**.</span></span>

[3]: ./media/notification-hubs-aspnet-backend-windows-dotnet-secure-push/notification-hubs-secure-push3.png
[12]: ./media/notification-hubs-aspnet-backend-windows-dotnet-secure-push/notification-hubs-secure-push12.png
[13]: ./media/notification-hubs-aspnet-backend-windows-dotnet-secure-push/notification-hubs-secure-push13.png
