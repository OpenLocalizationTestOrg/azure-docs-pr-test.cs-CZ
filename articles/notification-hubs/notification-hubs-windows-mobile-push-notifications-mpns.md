---
title: "aaaSending nabízená oznámení pomocí Azure Notification Hubs ve Windows Phone | Microsoft Docs"
description: "V tomto kurzu zjistíte, jak oznámení toopush Azure Notification Hubs toouse tooa Windows Phone 8 nebo Windows Phone 8.1 Silverlight aplikace."
services: notification-hubs
documentationcenter: windows
keywords: "nabízené oznámení,nabízená oznámení,nabízení windows phone"
author: ysxu
manager: erikre
editor: erikre
ms.assetid: d872d8dc-4658-4d65-9e71-fa8e34fae96e
ms.service: notification-hubs
ms.workload: mobile
ms.tgt_pltfrm: mobile-windows-phone
ms.devlang: dotnet
ms.topic: hero-article
ms.date: 10/03/2016
ms.author: yuaxu
ms.openlocfilehash: 1a0ad238fe7788ae2e4f47f02d113391af03dd1d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="sending-push-notifications-with-azure-notification-hubs-on-windows-phone"></a>Odesílání nabízených oznámení pomocí Azure Notification Hubs ve Windows Phone
[!INCLUDE [notification-hubs-selector-get-started](../../includes/notification-hubs-selector-get-started.md)]

## <a name="overview"></a>Přehled
> [!NOTE]
> toocomplete tento kurz, musíte mít aktivní účet Azure. Pokud účet nemáte, můžete si během několika minut vytvořit bezplatný zkušební účet. Podrobnosti najdete v článku [Bezplatná zkušební verze Azure](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A0E0E5C02&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdocumentation%2Farticles%2Fnotification-hubs-windows-phone-get-started%2F).
> 
> 

Tento kurz ukazuje, jak Azure Notification Hubs toosend toouse nabízená oznámení aplikace Windows Phone 8 nebo Windows Phone 8.1 Silverlight tooa. Pokud cílíte na Windows Phone 8.1 (bez Silverlight), pak odkazovat toohello [univerzální pro Windows](notification-hubs-windows-store-dotnet-get-started-wns-push-notification.md) verze.
V tomto kurzu vytvoříte prázdnou aplikaci Windows Phone 8, která obdrží nabízená oznámení pomocí hello služby nabízených oznámení Microsoft (MPNS). Jakmile budete hotovi, budete moct toouse vaše toobroadcast centra oznámení nabízená oznámení tooall hello zařízení používající vaši aplikaci.

> [!NOTE]
> Hello centra oznámení Windows Phone SDK nepodporuje použití hello nabízené služby oznámení Windows (WNS) s aplikacemi pro Windows Phone 8.1 Silverlight. toouse WNS (namísto MPNS) s aplikacemi pro Windows Phone 8.1 Silverlight, postupujte podle hello [kurzu centra oznámení – Windows Phone Silverlight], který používá rozhraní REST API.
> 
> 

Hello kurz představuje scénář jednoduchého vysílání hello přes centra oznámení.

## <a name="prerequisites"></a>Požadavky
Tento kurz vyžaduje hello následující:

* [Visual Studio 2012 Express pro Windows Phone] nebo novější verzi

Dokončení tohoto kurzu je předpokladem pro všechny ostatní kurzy Notification Hubs pro aplikace Windows Phone 8.

## <a name="create-your-notification-hub"></a>Vytvoření centra oznámení
[!INCLUDE [notification-hubs-portal-create-new-hub](../../includes/notification-hubs-portal-create-new-hub.md)]

<ol start="6">
<li><p>Klikněte na tlačítko hello <b>služby oznámení</b> část (v rámci <i>nastavení</i>), klikněte na <b>Windows Phone (MPNS)</b> a pak klikněte na tlačítko hello <b>Povolit neověřené nabízená oznámení </b> zaškrtávací políčko.</p>
</li>
</ol>

&emsp;&emsp;![Portál Azure – povolit neověřená nabízená oznámení](./media/notification-hubs-windows-phone-get-started/azure-portal-unauth.png)

Vaše centrum je teď vytvořený a nakonfigurovaný toosend neověřené oznámení pro Windows Phone.

> [!NOTE]
> Tento kurz využívá MPNS v neověřeném režimu. Neověřený režim MPNS přichází s omezeními na oznámení můžete odesílat tooeach kanál. Centra oznámení podporují [ověřený režim MPNS](http://msdn.microsoft.com/library/windowsphone/develop/ff941099.aspx) tak, že umožní tooupload svůj certifikát.
> 
> 

## <a name="connecting-your-app-toohello-notification-hub"></a>Připojení aplikace toohello centra oznámení
1. V sadě Visual Studio vytvořte novou aplikaci pro Windows Phone 8.
   
       ![Visual Studio - New Project - Windows Phone App][13]
   
    Ve Visual Studio 2013 Update 2 nebo novější verzi místo toho vytvořte aplikaci Windows Phone Silverlight.
   
    ![Visual Studio – Nový projekt – Prázdná aplikace – Windows Phone Silverlight][11]
2. V sadě Visual Studio, klikněte pravým tlačítkem na řešení hello a pak klikněte na tlačítko **spravovat balíčky NuGet**.
   
    Zobrazí se hello **spravovat balíčky NuGet** dialogové okno.
3. Vyhledejte `WindowsAzure.Messaging.Managed` a klikněte na tlačítko **nainstalovat**a pak přijměte podmínky použití hello.
   
    ![Visual Studio – Správce balíčků NuGet][20]
   
    To stáhne, nainstaluje a přidá knihovnu zasílání zpráv Azure toohello referenční dokumentace pro systém Windows pomocí hello <a href="http://nuget.org/packages/WindowsAzure.Messaging.Managed/">balíčku nuget WindowsAzure.Messaging.Managed</a>.
4. Otevřete soubor hello App.xaml.cs a přidejte následující hello `using` příkazy:
   
        using Microsoft.Phone.Notification;
        using Microsoft.WindowsAzure.Messaging;
5. Přidejte následující kód na začátku hello hello **Application_Launching** metoda v souboru App.xaml.cs:
   
        var channel = HttpNotificationChannel.Find("MyPushChannel");
        if (channel == null)
        {
            channel = new HttpNotificationChannel("MyPushChannel");
            channel.Open();
            channel.BindToShellToast();
        }
   
        channel.ChannelUriUpdated += new EventHandler<NotificationChannelUriEventArgs>(async (o, args) =>
        {
            var hub = new NotificationHub("<hub name>", "<connection string>");
            var result = await hub.RegisterNativeAsync(args.ChannelUri.ToString());
   
            System.Windows.Deployment.Current.Dispatcher.BeginInvoke(() =>
            {
                MessageBox.Show("Registration :" + result.RegistrationId, "Registered", MessageBoxButton.OK);
            });
        });
   
   > [!NOTE]
   > Hello hodnotu **MyPushChannel** je index, který je použité toolookup existujícího kanálu v hello [HttpNotificationChannel](https://msdn.microsoft.com/library/windows/apps/microsoft.phone.notification.httpnotificationchannel.aspx) kolekce. Pokud zde není k dispozici, vytvořte novou položku s tímto názvem.
   > 
   > 
   
    Zajistěte, aby tooinsert hello název připojovacího řetězce rozbočovače a hello volat **DefaultListenSharedAccessSignature** který jste získali v předchozí části hello.
    Tento kód načte hello kanál URI pro hello aplikaci z MPNS a pak zaregistruje tento kanál URI pomocí centra oznámení. Také zaručuje, že hello kanál URI je registrován v centru oznámení, které jednotlivé aplikace hello čas spuštění.
   
   > [!NOTE]
   > V tomto kurzu odešle zařízení toohello oznámení s informační zprávou. Při odesílání oznámení dlaždice musíte místo toho volat hello **BindToShellTile** metoda na hello kanálu. toosupport oznámení informační zprávy a dlaždice volání obě **BindToShellTile** a **BindToShellToast**.
   > 
   > 
6. V Průzkumníku řešení rozbalte **vlastnosti**, otevřete hello `WMAppManifest.xml` souboru, klikněte na tlačítko hello **možnosti** kartě a ujistěte se, že hello **ID_CAP_PUSH_NOTIFICATION** funkce je zaškrtnuté.
   
       ![Visual Studio - Windows Phone App Capabilities][14]
   
       This ensures that your app can receive push notifications. Without it, any attempt toosend a push notification toohello app will fail.
7. Stiskněte klávesu hello `F5` klíče toorun hello aplikace.
   
    V aplikaci hello se zobrazí zpráva registrace.
8. Zavřít hello aplikace.  
   
   > [!NOTE]
   > tooreceive nabízená oznámení, nesmí být hello aplikace spuštěná v popředí hello.
   > 
   > 

## <a name="send-push-notifications-from-your-backend"></a>Odesílání nabízených oznámení z backendu
Můžete odesílat nabízená oznámení pomocí centra oznámení z jakéhokoli backendu prostřednictvím veřejného hello <a href="http://msdn.microsoft.com/library/windowsazure/dn223264.aspx">rozhraní REST</a>. V tomto kurzu zašlete nabízená oznámení pomocí konzolové aplikace .NET. 

Příklad jak toosend nabízená oznámení z backendu ASP.NET WebAPI, které jsou integrovány v centrech oznámení naleznete v části [Azure upozornění uživatelů centra oznámení s .NET back-end](notification-hubs-aspnet-backend-windows-dotnet-wns-notification.md).  

Příklad jak toosend nabízená oznámení pomocí hello [rozhraní REST API](https://msdn.microsoft.com/library/azure/dn223264.aspx), podívejte se na [jak toouse centra oznámení z Java](notification-hubs-java-push-notification-tutorial.md) a [jak toouse Notification Hubs z PHP](notification-hubs-php-push-notification-tutorial.md) .

1. Hello řešení klikněte pravým tlačítkem, vyberte **přidat** a **nový projekt...** a potom v části **Visual C#**, klikněte na tlačítko **Windows** a **konzolové aplikace**a klikněte na tlačítko **OK**.
   
       ![Visual Studio - New Project - Console Application][6]
   
    Tento postup přidá nové Visual C# konzole aplikace toohello řešení. Tento postup také můžete využít v samostatném řešení.
2. Klikněte na položku **Nástroje**, klikněte na **Správce balíčků knihoven** a pak na **Konzola správce balíčků**.
   
    Zobrazí se hello Konzola správce balíčků.
3. V hello **Konzola správce balíčků** okno, sada hello **výchozí projekt** tooyour nové konzoly projekt aplikace a potom v okně konzoly hello, spusťte následující příkaz hello:
   
       Install-Package Microsoft.Azure.NotificationHubs
   
   Tento postup přidá odkaz toohello SDK centra oznámení Azure pomocí hello <a href="http://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/">balíčku Microsoft.Azure.Notification Hubs NuGet</a>.
4. Otevřete hello `Program.cs` souboru a přidejte následující hello `using` příkaz:
   
        using Microsoft.Azure.NotificationHubs;
5. V hello `Program` třídy, přidejte následující metodu hello:
   
        private static async void SendNotificationAsync()
        {
            NotificationHubClient hub = NotificationHubClient
                .CreateClientFromConnectionString("<connection string with full access>", "<hub name>");
            string toast = "<?xml version=\"1.0\" encoding=\"utf-8\"?>" +
                "<wp:Notification xmlns:wp=\"WPNotification\">" +
                   "<wp:Toast>" +
                        "<wp:Text1>Hello from a .NET App!</wp:Text1>" +
                   "</wp:Toast> " +
                "</wp:Notification>";
            await hub.SendMpnsNativeNotificationAsync(toast);
        }
   
    Ujistěte se, zda text hello tooreplace `<hub name>` zástupný symbol hello název centra oznámení hello, který se zobrazí na portálu hello. Také nahraďte zástupný symbol připojovacího řetězce hello s hello připojovací řetězec s názvem **DefaultFullSharedAccessSignature** který jste získali v části hello "Konfigurace centra oznámení".
   
   > [!NOTE]
   > Ujistěte se, že používáte připojovací řetězec hello s **úplné** přístupem, nikoli **naslouchání** přístup. řetězec s přístupem k naslouchání Hello nemá oprávnění toosend nabízená oznámení.
   > 
   > 
6. Přidejte následující řádek ve hello vaší `Main` metoda:
   
         SendNotificationAsync();
         Console.ReadLine();
7. S váš Windows Phone spuštěným emulátorem vaší aplikace hello uzavřené, nastavte projekt konzolové aplikace jako hello výchozí spouštěný projekt a stiskněte klávesu hello `F5` klíče toorun hello aplikace.
   
    Obdržíte nabízená oznámení. Klepnutím hello informační nápis načte aplikaci hello.

Můžete najít všechny možné datové části hello v hello [katalog informační zprávy] a [katalog dlaždic] témata na webu MSDN.

## <a name="next-steps"></a>Další kroky
V tomto jednoduchém příkladu jste vysílali nabízená oznámení tooall zařízení Windows Phone 8. 

V pořadí tootarget konkrétní uživatele, získáte toohello [použití centra oznámení toopush oznámení toousers] kurzu. 

Pokud chcete toosegment uživatele podle zájmových skupin, můžete si přečíst [toosend použití centra oznámení nejnovější zprávy přes]. 

Další informace o Notification Hubs toouse v [pokyny centra oznámení].

<!-- Images. -->
[6]: ./media/notification-hubs-windows-phone-get-started/notification-hub-create-console-app.png
[7]: ./media/notification-hubs-windows-phone-get-started/notification-hub-create-from-portal.png
[8]: ./media/notification-hubs-windows-phone-get-started/notification-hub-create-from-portal2.png
[9]: ./media/notification-hubs-windows-phone-get-started/notification-hub-select-from-portal.png
[10]: ./media/notification-hubs-windows-phone-get-started/notification-hub-select-from-portal2.png
[11]: ./media/notification-hubs-windows-phone-get-started/notification-hub-create-wp-silverlight-app.png
[12]: ./media/notification-hubs-windows-phone-get-started/notification-hub-connection-strings.png

[13]: ./media/notification-hubs-windows-phone-get-started/notification-hub-create-wp-app.png
[14]: ./media/notification-hubs-windows-phone-get-started/mobile-app-enable-push-wp8.png
[15]: ./media/notification-hubs-windows-phone-get-started/notification-hub-pushauth.png
[20]: ./media/notification-hubs-windows-phone-get-started/notification-hub-windows-universal-app-install-package.png
[213]: ./media/notification-hubs-windows-phone-get-started/notification-hub-create-console-app.png





<!-- URLs. -->
[Visual Studio 2012 Express pro Windows Phone]: https://go.microsoft.com/fwLink/p/?LinkID=268374
[pokyny centra oznámení]: http://msdn.microsoft.com/library/jj927170.aspx
[MPNS authenticated mode]: http://msdn.microsoft.com/library/windowsphone/develop/ff941099(v=vs.105).aspx
[použití centra oznámení toopush oznámení toousers]: notification-hubs-aspnet-backend-windows-dotnet-wns-notification.md
[toosend použití centra oznámení nejnovější zprávy přes]: notification-hubs-windows-phone-push-xplat-segmented-mpns-notification.md
[katalog informační zprávy]: http://msdn.microsoft.com/library/windowsphone/develop/jj662938(v=vs.105).aspx
[katalog dlaždic]: http://msdn.microsoft.com/library/windowsphone/develop/hh202948(v=vs.105).aspx
[kurzu centra oznámení – Windows Phone Silverlight]: https://github.com/Azure/azure-notificationhubs-samples/tree/master/PushToSLPhoneApp

