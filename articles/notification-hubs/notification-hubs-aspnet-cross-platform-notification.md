---
title: "toousers aaaSend oznámení napříč platformami pomocí centra oznámení (ASP.NET)"
description: "Zjistěte, jak toouse toosend šablony centra oznámení, v jedné žádosti, bez ohledu na platformu oznámení, že cílem všechny platformy."
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
ms.openlocfilehash: f105b871b809e739dd5c05ea819ad135e842ebb0
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="send-cross-platform-notifications-toousers-with-notification-hubs"></a><span data-ttu-id="41a1c-103">Odeslat oznámení napříč platformami toousers s centry oznámení</span><span class="sxs-lookup"><span data-stu-id="41a1c-103">Send cross-platform notifications toousers with Notification Hubs</span></span>
<span data-ttu-id="41a1c-104">V předchozích kurzu hello [upozorněte uživatele s Notification Hubs], jste se dozvěděli, jak toopush oznámení tooall zařízení zaregistrovaná podle konkrétního ověřeného uživatele.</span><span class="sxs-lookup"><span data-stu-id="41a1c-104">In hello previous tutorial [Notify users with Notification Hubs], you learned how toopush notifications tooall devices registered by a specific authenticated user.</span></span> <span data-ttu-id="41a1c-105">V tomto kurzu se víc požadavků na požadované toosend oznámení tooeach podporované klientské platformy.</span><span class="sxs-lookup"><span data-stu-id="41a1c-105">In that tutorial, multiple requests were required toosend a notification tooeach supported client platform.</span></span> <span data-ttu-id="41a1c-106">Notification Hubs podporuje šablony, které umožňují určit, jak určité zařízení chce tooreceive oznámení.</span><span class="sxs-lookup"><span data-stu-id="41a1c-106">Notification Hubs supports templates, which let you specify how a specific device wants tooreceive notifications.</span></span> <span data-ttu-id="41a1c-107">Tato funkce zjednodušuje odesílání oznámení napříč platformami.</span><span class="sxs-lookup"><span data-stu-id="41a1c-107">This simplifies sending cross-platform notifications.</span></span> <span data-ttu-id="41a1c-108">Toto téma ukazuje, jak tootake výhod toosend šablony, v jedné žádosti, bez ohledu na platformu oznámení, že cílem všechny platformy.</span><span class="sxs-lookup"><span data-stu-id="41a1c-108">This topic demonstrates how tootake advantage of templates toosend, in a single request, a platform-agnostic notification that targets all platforms.</span></span> <span data-ttu-id="41a1c-109">Podrobnější informace o šablonách najdete v článku [přehledu této služby Azure][Templates].</span><span class="sxs-lookup"><span data-stu-id="41a1c-109">For more detailed information about templates, see [Azure Notification Hubs Overview][Templates].</span></span>
> [!IMPORTANT]
> <span data-ttu-id="41a1c-110">Projekty Windows Phone 8.1 a starší nejsou podporovány pomocí Visual Studio 2017.</span><span class="sxs-lookup"><span data-stu-id="41a1c-110">Windows Phone projects 8.1 and earlier are not supported using Visual Studio 2017.</span></span> <span data-ttu-id="41a1c-111">Další informace najdete v tématu [Cílení na platformy a kompatibilita v sadě Visual Studio 2017](https://www.visualstudio.com/en-us/productinfo/vs2017-compatibility-vs).</span><span class="sxs-lookup"><span data-stu-id="41a1c-111">For more information, see [Visual Studio 2017 Platform Targeting and Compatibility](https://www.visualstudio.com/en-us/productinfo/vs2017-compatibility-vs).</span></span>

> [!NOTE]
> <span data-ttu-id="41a1c-112">Centra oznámení umožňuje zařízení tooregister několik šablon s hello stejnou značku.</span><span class="sxs-lookup"><span data-stu-id="41a1c-112">Notification Hubs allows a device tooregister multiple templates with hello same tag.</span></span> <span data-ttu-id="41a1c-113">V takovém případě příchozí zprávy cílení, že značka výsledky v několika oznámení doručit toohello zařízení, jeden pro každé šablony.</span><span class="sxs-lookup"><span data-stu-id="41a1c-113">In this case, an incoming message targeting that tag results in multiple notifications delivered toohello device, one for each template.</span></span> <span data-ttu-id="41a1c-114">Tato umožňuje vám toodisplay hello stejnou zprávu v několika visual oznámení, například jako oznámení "BADGE" i jako informační zpráva v aplikaci Windows Store.</span><span class="sxs-lookup"><span data-stu-id="41a1c-114">This enables you toodisplay hello same message in multiple visual notifications, such as both as a badge and as a toast notification in a Windows Store app.</span></span>
> 
> 

<span data-ttu-id="41a1c-115">Proveďte následující kroky toosend napříč platformami oznámení pomocí šablony hello:</span><span class="sxs-lookup"><span data-stu-id="41a1c-115">Complete hello following steps toosend cross-platform notifications using templates:</span></span>

1. <span data-ttu-id="41a1c-116">V Průzkumníku řešení v sadě Visual Studio hello, rozbalte položku hello **řadiče** složku a potom otevřete hello RegisterController.cs souboru.</span><span class="sxs-lookup"><span data-stu-id="41a1c-116">In hello Solution Explorer in Visual Studio, expand hello **Controllers** folder, then open hello RegisterController.cs file.</span></span>
2. <span data-ttu-id="41a1c-117">Vyhledejte hello blok kódu v hello **Put** metodu, která vytvoří novou registraci nahradit hello `switch` obsahu s hello následující kód:</span><span class="sxs-lookup"><span data-stu-id="41a1c-117">Locate hello block of code in hello **Put** method that creates a new registration replace hello `switch` content with hello following code:</span></span>
   
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
   
    <span data-ttu-id="41a1c-118">Tento kód zavolá hello specifické pro platformu metoda toocreate šablony registrace místo registraci nativního.</span><span class="sxs-lookup"><span data-stu-id="41a1c-118">This code calls hello platform-specific method toocreate a template registration instead of a native registration.</span></span> <span data-ttu-id="41a1c-119">Existující registrace nemusí upravit, protože šablona registrace odvozena z nativní registrací.</span><span class="sxs-lookup"><span data-stu-id="41a1c-119">Existing registrations need not be modified because template registrations derive from native registrations.</span></span>
3. <span data-ttu-id="41a1c-120">V hello **oznámení** řadič, nahraďte hello **sendNotification** metoda s hello následující kód:</span><span class="sxs-lookup"><span data-stu-id="41a1c-120">In hello **Notifications** controller, replace hello **sendNotification** method with hello following code:</span></span>
   
        public async Task<HttpResponseMessage> Post()
        {
            var user = HttpContext.Current.User.Identity.Name;
            var userTag = "username:" + user;
   
            var notification = new Dictionary<string, string> { { "message", "Hello, " + user } };
            await Notifications.Instance.Hub.SendTemplateNotificationAsync(notification, userTag);
   
            return Request.CreateResponse(HttpStatusCode.OK);
        }
   
    <span data-ttu-id="41a1c-121">Tento kód platformy tooall odešle oznámení v hello stejný čas a bez nutnosti toospecify nativní datové části.</span><span class="sxs-lookup"><span data-stu-id="41a1c-121">This code sends a notification tooall platforms at hello same time and without having toospecify a native payload.</span></span> <span data-ttu-id="41a1c-122">Sestaví centra oznámení a doručí hello zařízení tooevery správnou datovou část s hello poskytuje *značky* hodnota uvedeného v šablonách hello zaregistrován.</span><span class="sxs-lookup"><span data-stu-id="41a1c-122">Notification Hubs builds and delivers hello correct payload tooevery device with hello provided *tag* value, as specified in hello registered templates.</span></span>
4. <span data-ttu-id="41a1c-123">Znovu publikujte projektu WebApi back-end.</span><span class="sxs-lookup"><span data-stu-id="41a1c-123">Re-publish your WebApi back-end project.</span></span>
5. <span data-ttu-id="41a1c-124">Znovu spustit klientskou aplikaci hello a ověřit, zda je úspěšné registrace.</span><span class="sxs-lookup"><span data-stu-id="41a1c-124">Run hello client app again and verify that registration succeeds.</span></span>
6. <span data-ttu-id="41a1c-125">(Volitelné) Nasadit hello klienta aplikace tooa druhé zařízení, a pak spusťte aplikaci hello.</span><span class="sxs-lookup"><span data-stu-id="41a1c-125">(Optional) Deploy hello client app tooa second device, then run hello app.</span></span>
   
    <span data-ttu-id="41a1c-126">Všimněte si, že na každém zařízení se zobrazí oznámení.</span><span class="sxs-lookup"><span data-stu-id="41a1c-126">Note that a notification is displayed on each device.</span></span>

## <a name="next-steps"></a><span data-ttu-id="41a1c-127">Další kroky</span><span class="sxs-lookup"><span data-stu-id="41a1c-127">Next Steps</span></span>
<span data-ttu-id="41a1c-128">Teď, když jste dokončili tento kurz, získáte další informace o Notification Hubs a šablon v těchto tématech:</span><span class="sxs-lookup"><span data-stu-id="41a1c-128">Now that you have completed this tutorial, find out more about Notification Hubs and templates in these topics:</span></span>

* <span data-ttu-id="41a1c-129">**[Použít nejnovější zprávy přes toosend centra oznámení]**</span><span class="sxs-lookup"><span data-stu-id="41a1c-129">**[Use Notification Hubs toosend breaking news]**</span></span> <br/><span data-ttu-id="41a1c-130">Demonstruje jiný scénář pro použití šablon</span><span class="sxs-lookup"><span data-stu-id="41a1c-130">Demonstrates another scenario for using templates</span></span>
* <span data-ttu-id="41a1c-131">**[Přehled služby Azure Notification Hubs][Templates]**</span><span class="sxs-lookup"><span data-stu-id="41a1c-131">**[Azure Notification Hubs Overview][Templates]**</span></span><br/><span data-ttu-id="41a1c-132">Přehled téma obsahuje podrobnější informace o šablonách.</span><span class="sxs-lookup"><span data-stu-id="41a1c-132">Overview topic has more detailed information on templates.</span></span>

<!-- Anchors. -->

<!-- Images. -->




<!-- URLs. -->
[Push toousers ASP.NET]: /manage/services/notification-hubs/notify-users-aspnet
[Push toousers Mobile Services]: /manage/services/notification-hubs/notify-users/
[Visual Studio 2012 Express for Windows 8]: http://go.microsoft.com/fwlink/?LinkId=257546

[Použít nejnovější zprávy přes toosend centra oznámení]: notification-hubs-windows-notification-dotnet-push-xplat-segmented-wns.md
[Azure Notification Hubs]: http://go.microsoft.com/fwlink/p/?LinkId=314257
[upozorněte uživatele s Notification Hubs]: notification-hubs-aspnet-backend-windows-dotnet-wns-notification.md
[Templates]: http://go.microsoft.com/fwlink/p/?LinkId=317339
[Notification Hub How toofor Windows Store]: http://msdn.microsoft.com/library/windowsazure/jj927172.aspx
