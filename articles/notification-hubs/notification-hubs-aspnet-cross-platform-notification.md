---
title: "Odesílat oznámení napříč platformami uživatelům s centra oznámení (ASP.NET)"
description: "Zjistěte, jak používat šablony Notification Hubs k odesílání v jedné žádosti, bez ohledu na platformu oznámení, že cílem všechny platformy."
services: notification-hubs
documentationcenter: 
author: ysxu
manager: erikre
editor: 
ms.assetid: 11d2131b-f683-47fd-a691-4cdfc696f62b
ms.service: notification-hubs
ms.workload: mobile
ms.tgt_pltfrm: mobile-windows
ms.devlang: multiple
ms.topic: article
ms.date: 10/03/2016
ms.author: yuaxu
ms.openlocfilehash: ef971fcfe68978ea9ce0810c69efbe134bb15f8a
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="send-cross-platform-notifications-to-users-with-notification-hubs"></a><span data-ttu-id="95c08-103">Odesílat oznámení napříč platformami uživatelům s centry oznámení</span><span class="sxs-lookup"><span data-stu-id="95c08-103">Send cross-platform notifications to users with Notification Hubs</span></span>
<span data-ttu-id="95c08-104">V tomto kurzu předchozí [upozorněte uživatele s Notification Hubs], jste se dozvěděli, jak nabízená oznámení pro všechna zařízení zaregistrovat podle konkrétního ověřeného uživatele.</span><span class="sxs-lookup"><span data-stu-id="95c08-104">In the previous tutorial [Notify users with Notification Hubs], you learned how to push notifications to all devices registered by a specific authenticated user.</span></span> <span data-ttu-id="95c08-105">V tomto kurzu bylo potřeba víc požadavků na odeslání oznámení do každé podporované klientské platformy.</span><span class="sxs-lookup"><span data-stu-id="95c08-105">In that tutorial, multiple requests were required to send a notification to each supported client platform.</span></span> <span data-ttu-id="95c08-106">Notification Hubs podporuje šablony, které umožňují určit, jak chce dostávat oznámení konkrétní zařízení.</span><span class="sxs-lookup"><span data-stu-id="95c08-106">Notification Hubs supports templates, which let you specify how a specific device wants to receive notifications.</span></span> <span data-ttu-id="95c08-107">Tato funkce zjednodušuje odesílání oznámení napříč platformami.</span><span class="sxs-lookup"><span data-stu-id="95c08-107">This simplifies sending cross-platform notifications.</span></span> <span data-ttu-id="95c08-108">Toto téma ukazuje, jak chcete využít výhod šablony odeslat v jedné žádosti, bez ohledu na platformu oznámení, že cílem všechny platformy.</span><span class="sxs-lookup"><span data-stu-id="95c08-108">This topic demonstrates how to take advantage of templates to send, in a single request, a platform-agnostic notification that targets all platforms.</span></span> <span data-ttu-id="95c08-109">Podrobnější informace o šablonách najdete v článku [přehledu této služby Azure][Templates].</span><span class="sxs-lookup"><span data-stu-id="95c08-109">For more detailed information about templates, see [Azure Notification Hubs Overview][Templates].</span></span>
> [!IMPORTANT]
> <span data-ttu-id="95c08-110">Projekty Windows Phone 8.1 a starší nejsou podporovány pomocí Visual Studio 2017.</span><span class="sxs-lookup"><span data-stu-id="95c08-110">Windows Phone projects 8.1 and earlier are not supported using Visual Studio 2017.</span></span> <span data-ttu-id="95c08-111">Další informace najdete v tématu [Cílení na platformy a kompatibilita v sadě Visual Studio 2017](https://www.visualstudio.com/en-us/productinfo/vs2017-compatibility-vs).</span><span class="sxs-lookup"><span data-stu-id="95c08-111">For more information, see [Visual Studio 2017 Platform Targeting and Compatibility](https://www.visualstudio.com/en-us/productinfo/vs2017-compatibility-vs).</span></span>

> [!NOTE]
> <span data-ttu-id="95c08-112">Centra oznámení umožňuje zařízení k registraci více šablon se stejnou značkou.</span><span class="sxs-lookup"><span data-stu-id="95c08-112">Notification Hubs allows a device to register multiple templates with the same tag.</span></span> <span data-ttu-id="95c08-113">V takovém případě příchozí zprávy cílení na konkrétní značkou výsledkem několik oznámení doručit do zařízení, jeden pro každé šablony.</span><span class="sxs-lookup"><span data-stu-id="95c08-113">In this case, an incoming message targeting that tag results in multiple notifications delivered to the device, one for each template.</span></span> <span data-ttu-id="95c08-114">To umožňuje zobrazit stejná zpráva v několika visual oznámení, například jako oznámení "BADGE" i jako informační zpráva v aplikaci Windows Store.</span><span class="sxs-lookup"><span data-stu-id="95c08-114">This enables you to display the same message in multiple visual notifications, such as both as a badge and as a toast notification in a Windows Store app.</span></span>
> 
> 

<span data-ttu-id="95c08-115">Proveďte následující kroky k odesílání oznámení napříč platformami pomocí šablon:</span><span class="sxs-lookup"><span data-stu-id="95c08-115">Complete the following steps to send cross-platform notifications using templates:</span></span>

1. <span data-ttu-id="95c08-116">V Průzkumníku řešení v sadě Visual Studio, rozbalte **řadiče** složku a pak otevřete soubor RegisterController.cs.</span><span class="sxs-lookup"><span data-stu-id="95c08-116">In the Solution Explorer in Visual Studio, expand the **Controllers** folder, then open the RegisterController.cs file.</span></span>
2. <span data-ttu-id="95c08-117">Vyhledejte blok kódu **Put** metodu, která vytvoří novou registraci nahradit `switch` obsahu s následujícím kódem:</span><span class="sxs-lookup"><span data-stu-id="95c08-117">Locate the block of code in the **Put** method that creates a new registration replace the `switch` content with the following code:</span></span>
   
        switch (deviceUpdate.Platform)
        {
            case "mpns":
                var toastTemplate = "<?xml version=\"1.0\" encoding=\"utf-8\"?>" +
                    "<wp:Notification xmlns:wp=\"WPNotification\">" +
                       "<wp:Toast>" +
                            "<wp:Text1>$(message)</wp:Text1>" +
                       "</wp:Toast> " +
                    "</wp:Notification>";
                registration = new MpnsTemplateRegistrationDescription(deviceUpdate.Handle, toastTemplate);
                break;
            case "wns":
                toastTemplate = @"<toast><visual><binding template=""ToastText01""><text id=""1"">$(message)</text></binding></visual></toast>";
                registration = new WindowsTemplateRegistrationDescription(deviceUpdate.Handle, toastTemplate);
                break;
            case "apns":
                var alertTemplate = "{\"aps\":{\"alert\":\"$(message)\"}}";
                registration = new AppleTemplateRegistrationDescription(deviceUpdate.Handle, alertTemplate);
                break;
            case "gcm":
                var messageTemplate = "{\"data\":{\"message\":\"$(message)\"}}";
                registration = new GcmTemplateRegistrationDescription(deviceUpdate.Handle, messageTemplate);
                break;
            default:
                throw new HttpResponseException(HttpStatusCode.BadRequest);
        }
   
    <span data-ttu-id="95c08-118">Tento kód volá metodu specifické pro platformu pro vytvoření šablony registrace místo nativní registrace.</span><span class="sxs-lookup"><span data-stu-id="95c08-118">This code calls the platform-specific method to create a template registration instead of a native registration.</span></span> <span data-ttu-id="95c08-119">Existující registrace nemusí upravit, protože šablona registrace odvozena z nativní registrací.</span><span class="sxs-lookup"><span data-stu-id="95c08-119">Existing registrations need not be modified because template registrations derive from native registrations.</span></span>
3. <span data-ttu-id="95c08-120">V **oznámení** řadiče, nahraďte **sendNotification** metoda následujícím kódem:</span><span class="sxs-lookup"><span data-stu-id="95c08-120">In the **Notifications** controller, replace the **sendNotification** method with the following code:</span></span>
   
        public async Task<HttpResponseMessage> Post()
        {
            var user = HttpContext.Current.User.Identity.Name;
            var userTag = "username:" + user;
   
            var notification = new Dictionary<string, string> { { "message", "Hello, " + user } };
            await Notifications.Instance.Hub.SendTemplateNotificationAsync(notification, userTag);
   
            return Request.CreateResponse(HttpStatusCode.OK);
        }
   
    <span data-ttu-id="95c08-121">Tento kód odešle oznámení na všechny platformy ve stejnou dobu a bez nutnosti zadávat nativní datové části.</span><span class="sxs-lookup"><span data-stu-id="95c08-121">This code sends a notification to all platforms at the same time and without having to specify a native payload.</span></span> <span data-ttu-id="95c08-122">Notification Hubs sestavení a doručí správné datové části pro každé zařízení poskytnutým *značky* hodnoty zadané v registrovaných šablony.</span><span class="sxs-lookup"><span data-stu-id="95c08-122">Notification Hubs builds and delivers the correct payload to every device with the provided *tag* value, as specified in the registered templates.</span></span>
4. <span data-ttu-id="95c08-123">Znovu publikujte projektu WebApi back-end.</span><span class="sxs-lookup"><span data-stu-id="95c08-123">Re-publish your WebApi back-end project.</span></span>
5. <span data-ttu-id="95c08-124">Znovu spustit klientskou aplikaci a ověřte, že registrace úspěšná.</span><span class="sxs-lookup"><span data-stu-id="95c08-124">Run the client app again and verify that registration succeeds.</span></span>
6. <span data-ttu-id="95c08-125">(Volitelné) Nasazení aplikace klienta na použití druhého zařízení a potom spusťte aplikaci.</span><span class="sxs-lookup"><span data-stu-id="95c08-125">(Optional) Deploy the client app to a second device, then run the app.</span></span>
   
    <span data-ttu-id="95c08-126">Všimněte si, že na každém zařízení se zobrazí oznámení.</span><span class="sxs-lookup"><span data-stu-id="95c08-126">Note that a notification is displayed on each device.</span></span>

## <a name="next-steps"></a><span data-ttu-id="95c08-127">Další kroky</span><span class="sxs-lookup"><span data-stu-id="95c08-127">Next Steps</span></span>
<span data-ttu-id="95c08-128">Teď, když jste dokončili tento kurz, získáte další informace o Notification Hubs a šablon v těchto tématech:</span><span class="sxs-lookup"><span data-stu-id="95c08-128">Now that you have completed this tutorial, find out more about Notification Hubs and templates in these topics:</span></span>

* <span data-ttu-id="95c08-129">**[Použití centra oznámení k odesílání novinek]**</span><span class="sxs-lookup"><span data-stu-id="95c08-129">**[Use Notification Hubs to send breaking news]**</span></span> <br/><span data-ttu-id="95c08-130">Demonstruje jiný scénář pro použití šablon</span><span class="sxs-lookup"><span data-stu-id="95c08-130">Demonstrates another scenario for using templates</span></span>
* <span data-ttu-id="95c08-131">**[Přehled služby Azure Notification Hubs][Templates]**</span><span class="sxs-lookup"><span data-stu-id="95c08-131">**[Azure Notification Hubs Overview][Templates]**</span></span><br/><span data-ttu-id="95c08-132">Přehled téma obsahuje podrobnější informace o šablonách.</span><span class="sxs-lookup"><span data-stu-id="95c08-132">Overview topic has more detailed information on templates.</span></span>

<!-- Anchors. -->

<!-- Images. -->




<!-- URLs. -->
[Push to users ASP.NET]: /manage/services/notification-hubs/notify-users-aspnet
[Push to users Mobile Services]: /manage/services/notification-hubs/notify-users/
[Visual Studio 2012 Express for Windows 8]: http://go.microsoft.com/fwlink/?LinkId=257546

<span data-ttu-id="95c08-133">[Použití centra oznámení k odesílání novinek]: notification-hubs-windows-notification-dotnet-push-xplat-segmented-wns.md</span><span class="sxs-lookup"><span data-stu-id="95c08-133">[Use Notification Hubs to send breaking news]: notification-hubs-windows-notification-dotnet-push-xplat-segmented-wns.md</span></span>
[Azure Notification Hubs]: http://go.microsoft.com/fwlink/p/?LinkId=314257
<span data-ttu-id="95c08-134">[upozorněte uživatele s Notification Hubs]: notification-hubs-aspnet-backend-windows-dotnet-wns-notification.md</span><span class="sxs-lookup"><span data-stu-id="95c08-134">[Notify users with Notification Hubs]: notification-hubs-aspnet-backend-windows-dotnet-wns-notification.md</span></span>
[Templates]: http://go.microsoft.com/fwlink/p/?LinkId=317339
[Notification Hub How to for Windows Store]: http://msdn.microsoft.com/library/windowsazure/jj927172.aspx
