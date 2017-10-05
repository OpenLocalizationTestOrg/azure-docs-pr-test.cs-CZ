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
# <a name="send-cross-platform-notifications-to-users-with-notification-hubs"></a>Odesílat oznámení napříč platformami uživatelům s centry oznámení
V tomto kurzu předchozí [upozorněte uživatele s Notification Hubs], jste se dozvěděli, jak nabízená oznámení pro všechna zařízení zaregistrovat podle konkrétního ověřeného uživatele. V tomto kurzu bylo potřeba víc požadavků na odeslání oznámení do každé podporované klientské platformy. Notification Hubs podporuje šablony, které umožňují určit, jak chce dostávat oznámení konkrétní zařízení. Tato funkce zjednodušuje odesílání oznámení napříč platformami. Toto téma ukazuje, jak chcete využít výhod šablony odeslat v jedné žádosti, bez ohledu na platformu oznámení, že cílem všechny platformy. Podrobnější informace o šablonách najdete v článku [přehledu této služby Azure][Templates].
> [!IMPORTANT]
> Projekty Windows Phone 8.1 a starší nejsou podporovány pomocí Visual Studio 2017. Další informace najdete v tématu [Cílení na platformy a kompatibilita v sadě Visual Studio 2017](https://www.visualstudio.com/en-us/productinfo/vs2017-compatibility-vs).

> [!NOTE]
> Centra oznámení umožňuje zařízení k registraci více šablon se stejnou značkou. V takovém případě příchozí zprávy cílení na konkrétní značkou výsledkem několik oznámení doručit do zařízení, jeden pro každé šablony. To umožňuje zobrazit stejná zpráva v několika visual oznámení, například jako oznámení "BADGE" i jako informační zpráva v aplikaci Windows Store.
> 
> 

Proveďte následující kroky k odesílání oznámení napříč platformami pomocí šablon:

1. V Průzkumníku řešení v sadě Visual Studio, rozbalte **řadiče** složku a pak otevřete soubor RegisterController.cs.
2. Vyhledejte blok kódu **Put** metodu, která vytvoří novou registraci nahradit `switch` obsahu s následujícím kódem:
   
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
   
    Tento kód volá metodu specifické pro platformu pro vytvoření šablony registrace místo nativní registrace. Existující registrace nemusí upravit, protože šablona registrace odvozena z nativní registrací.
3. V **oznámení** řadiče, nahraďte **sendNotification** metoda následujícím kódem:
   
        public async Task<HttpResponseMessage> Post()
        {
            var user = HttpContext.Current.User.Identity.Name;
            var userTag = "username:" + user;
   
            var notification = new Dictionary<string, string> { { "message", "Hello, " + user } };
            await Notifications.Instance.Hub.SendTemplateNotificationAsync(notification, userTag);
   
            return Request.CreateResponse(HttpStatusCode.OK);
        }
   
    Tento kód odešle oznámení na všechny platformy ve stejnou dobu a bez nutnosti zadávat nativní datové části. Notification Hubs sestavení a doručí správné datové části pro každé zařízení poskytnutým *značky* hodnoty zadané v registrovaných šablony.
4. Znovu publikujte projektu WebApi back-end.
5. Znovu spustit klientskou aplikaci a ověřte, že registrace úspěšná.
6. (Volitelné) Nasazení aplikace klienta na použití druhého zařízení a potom spusťte aplikaci.
   
    Všimněte si, že na každém zařízení se zobrazí oznámení.

## <a name="next-steps"></a>Další kroky
Teď, když jste dokončili tento kurz, získáte další informace o Notification Hubs a šablon v těchto tématech:

* **[Použití centra oznámení k odesílání novinek]** <br/>Demonstruje jiný scénář pro použití šablon
* **[Přehled služby Azure Notification Hubs][Templates]**<br/>Přehled téma obsahuje podrobnější informace o šablonách.

<!-- Anchors. -->

<!-- Images. -->




<!-- URLs. -->
[Push to users ASP.NET]: /manage/services/notification-hubs/notify-users-aspnet
[Push to users Mobile Services]: /manage/services/notification-hubs/notify-users/
[Visual Studio 2012 Express for Windows 8]: http://go.microsoft.com/fwlink/?LinkId=257546

[Použití centra oznámení k odesílání novinek]: notification-hubs-windows-notification-dotnet-push-xplat-segmented-wns.md
[Azure Notification Hubs]: http://go.microsoft.com/fwlink/p/?LinkId=314257
[upozorněte uživatele s Notification Hubs]: notification-hubs-aspnet-backend-windows-dotnet-wns-notification.md
[Templates]: http://go.microsoft.com/fwlink/p/?LinkId=317339
[Notification Hub How to for Windows Store]: http://msdn.microsoft.com/library/windowsazure/jj927172.aspx
