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
# <a name="send-cross-platform-notifications-toousers-with-notification-hubs"></a>Odeslat oznámení napříč platformami toousers s centry oznámení
V předchozích kurzu hello [upozorněte uživatele s Notification Hubs], jste se dozvěděli, jak toopush oznámení tooall zařízení zaregistrovaná podle konkrétního ověřeného uživatele. V tomto kurzu se víc požadavků na požadované toosend oznámení tooeach podporované klientské platformy. Notification Hubs podporuje šablony, které umožňují určit, jak určité zařízení chce tooreceive oznámení. Tato funkce zjednodušuje odesílání oznámení napříč platformami. Toto téma ukazuje, jak tootake výhod toosend šablony, v jedné žádosti, bez ohledu na platformu oznámení, že cílem všechny platformy. Podrobnější informace o šablonách najdete v článku [přehledu této služby Azure][Templates].
> [!IMPORTANT]
> Projekty Windows Phone 8.1 a starší nejsou podporovány pomocí Visual Studio 2017. Další informace najdete v tématu [Cílení na platformy a kompatibilita v sadě Visual Studio 2017](https://www.visualstudio.com/en-us/productinfo/vs2017-compatibility-vs).

> [!NOTE]
> Centra oznámení umožňuje zařízení tooregister několik šablon s hello stejnou značku. V takovém případě příchozí zprávy cílení, že značka výsledky v několika oznámení doručit toohello zařízení, jeden pro každé šablony. Tato umožňuje vám toodisplay hello stejnou zprávu v několika visual oznámení, například jako oznámení "BADGE" i jako informační zpráva v aplikaci Windows Store.
> 
> 

Proveďte následující kroky toosend napříč platformami oznámení pomocí šablony hello:

1. V Průzkumníku řešení v sadě Visual Studio hello, rozbalte položku hello **řadiče** složku a potom otevřete hello RegisterController.cs souboru.
2. Vyhledejte hello blok kódu v hello **Put** metodu, která vytvoří novou registraci nahradit hello `switch` obsahu s hello následující kód:
   
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
   
    Tento kód zavolá hello specifické pro platformu metoda toocreate šablony registrace místo registraci nativního. Existující registrace nemusí upravit, protože šablona registrace odvozena z nativní registrací.
3. V hello **oznámení** řadič, nahraďte hello **sendNotification** metoda s hello následující kód:
   
        public async Task<HttpResponseMessage> Post()
        {
            var user = HttpContext.Current.User.Identity.Name;
            var userTag = "username:" + user;
   
            var notification = new Dictionary<string, string> { { "message", "Hello, " + user } };
            await Notifications.Instance.Hub.SendTemplateNotificationAsync(notification, userTag);
   
            return Request.CreateResponse(HttpStatusCode.OK);
        }
   
    Tento kód platformy tooall odešle oznámení v hello stejný čas a bez nutnosti toospecify nativní datové části. Sestaví centra oznámení a doručí hello zařízení tooevery správnou datovou část s hello poskytuje *značky* hodnota uvedeného v šablonách hello zaregistrován.
4. Znovu publikujte projektu WebApi back-end.
5. Znovu spustit klientskou aplikaci hello a ověřit, zda je úspěšné registrace.
6. (Volitelné) Nasadit hello klienta aplikace tooa druhé zařízení, a pak spusťte aplikaci hello.
   
    Všimněte si, že na každém zařízení se zobrazí oznámení.

## <a name="next-steps"></a>Další kroky
Teď, když jste dokončili tento kurz, získáte další informace o Notification Hubs a šablon v těchto tématech:

* **[Použít nejnovější zprávy přes toosend centra oznámení]** <br/>Demonstruje jiný scénář pro použití šablon
* **[Přehled služby Azure Notification Hubs][Templates]**<br/>Přehled téma obsahuje podrobnější informace o šablonách.

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
