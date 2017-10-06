---
title: "aaaGet začít s Azure centra oznámení pro univerzální platformu aplikace Windows | Microsoft Docs"
description: "V tomto kurzu zjistíte, jak toouse Azure Notification Hubs toopush oznámení tooa aplikace pro univerzální platformu Windows."
services: notification-hubs
documentationcenter: windows
author: ysxu
manager: erikre
editor: erikre
ms.assetid: cf307cf3-8c58-4628-9c63-8751e6a0ef43
ms.service: notification-hubs
ms.workload: mobile
ms.tgt_pltfrm: mobile-windows
ms.devlang: dotnet
ms.topic: hero-article
ms.date: 10/03/2016
ms.author: yuaxu
ms.openlocfilehash: 11056842d205522ed493dc61c76ecf78ebb5a363
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="getting-started-with-notification-hubs-for-windows-universal-platform-apps"></a>Začínáme používat službu Notification Hubs pro aplikace Univerzální platformy Windows
[!INCLUDE [notification-hubs-selector-get-started](../../includes/notification-hubs-selector-get-started.md)]

## <a name="overview"></a>Přehled
Tento kurz ukazuje, jak Azure Notification Hubs toosend toouse nabízená oznámení tooa univerzální platformu Windows (UWP) aplikace.

V tomto kurzu vytvoříte prázdnou aplikaci Windows Store, která obdrží nabízená oznámení pomocí hello služby nabízených oznámení Windows (WNS). Jakmile budete hotovi, budete moct toouse vaše toobroadcast centra oznámení nabízená oznámení tooall hello zařízení používající vaši aplikaci.

## <a name="before-you-begin"></a>Než začnete
[!INCLUDE [notification-hubs-hero-slug](../../includes/notification-hubs-hero-slug.md)]

Hello dokončit kód v tomto kurzu lze nalézt na Githubu [zde](https://github.com/Azure/azure-notificationhubs-samples/tree/master/dotnet/GetStartedWindowsUniversal).

## <a name="prerequisites"></a>Požadavky
Tento kurz vyžaduje hello následující:

* [Microsoft Visual Studio Community 2015](https://www.visualstudio.com/products/visual-studio-community-vs) nebo novější
* [Nainstalované nástroje pro vývoj univerzálních aplikací pro Windows](https://msdn.microsoft.com/windows/uwp/get-started/get-set-up)
* Aktivní účet Azure <br/>Pokud účet nemáte, můžete si během několika minut vytvořit bezplatný zkušební účet. Podrobnosti najdete v článku [Bezplatná zkušební verze Azure](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A0E0E5C02&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdocumentation%2Farticles%2Fnotification-hubs-windows-store-dotnet-get-started%2F).
* Aktivní účet Windows Store

Dokončení tohoto kurzu je předpokladem pro všechny ostatní kurzy služby Notification Hubs pro aplikace Univerzální platformy Windows.

## <a name="register-your-app-for-hello-windows-store"></a>Registrace aplikace pro Windows Store hello
toosend nabízená oznámení tooUWP aplikace, je nutné přidružit toohello vaší aplikace Windows Store. Pak musíte nakonfigurovat vaše toointegrate centra oznámení s WNS.

1. Pokud jste ještě nezaregistrovali vaší aplikace, přejděte toohello [Windows Dev Center](https://dev.windows.com/overview), přihlaste se pomocí účtu Microsoft a pak klikněte na tlačítko **vytvořit novou aplikaci**.

2. Zadejte název aplikace a klikněte na tlačítko **Rezervovat název aplikace**. Tím se vytvoří nová registrace Windows Store pro vaši aplikaci.

3. V sadě Visual Studio vytvořte nový projekt aplikace Visual C# Store pomocí univerzální pro Windows hello **prázdnou aplikaci** šablonu a klikněte na tlačítko **OK**.

4. Přijměte výchozí hodnoty hello hello cíl a verze minimální platforem.

5. V Průzkumníku řešení klikněte pravým tlačítkem na projekt aplikace Windows Store hello, klikněte na tlačítko **úložiště**a pak klikněte na tlačítko **propojit aplikaci se hello úložiště...** . hello **přidružením aplikace hello Windows Store** zobrazí se průvodce.

6. V Průvodci hello Přihlaste se pomocí účtu Microsoft.

7. Klikněte na tlačítko hello aplikaci zaregistrovanou v kroku 2, klikněte na tlačítko **Další**a potom klikněte na **přidružit**. Tento postup přidá požadované hello Windows Store registrační informace toohello manifest aplikace.

8. Zpět na hello [Windows Dev Center](http://dev.windows.com/overview) stránky pro novou aplikaci, klikněte na tlačítko **služby**, klikněte na tlačítko **nabízená oznámení**a potom klikněte na **WNS nebo MPNS**.

9. Klikněte na **Nové oznámení**.

10. Klikněte na šablonu **Prázdná (informační zpráva)** a potom klikněte na **OK**.

11. Zadejte **Název** oznámení a vizuální **kontextovou** zprávu. Potom klikněte na **Uložit jako koncept**.

12. Přejděte toohello [portálu pro registraci aplikace](http://apps.dev.microsoft.com) a přihlaste se.

13. Klikněte na název vaší aplikace. Poznamenejte si hello **tajný klíč aplikace** heslo a hello **identifikátor zabezpečení (SID) balíčku** umístěný v hello **Windows Store** nastavení platformy.

     > [AZURE.WARNING]
    tajný klíč aplikace Hello a SID balíčku jsou důležitá pověření zabezpečení. Tyto hodnoty s nikým nesdílejte ani je nedistribuujte s vaší aplikací.

## <a name="configure-your-notification-hub"></a>Konfigurace centra oznámení
[!INCLUDE [notification-hubs-portal-create-new-hub](../../includes/notification-hubs-portal-create-new-hub.md)]

<ol start="6">
<li><p>Vyberte hello <b>služby oznámení</b> možnost a hello <b>Windows (WNS)</b> možnost. Potom zadejte hello <b>tajný klíč aplikace</b> heslo v hello <b>klíč zabezpečení</b> pole. Zadejte vaše <b>SID balíčku</b> hodnotu, který jste získali z WNS v předchozí části hello a pak klikněte na tlačítko <b>Uložit</b>.</p>
</li>
</ol>

&emsp;&emsp;![](./media/notification-hubs-windows-store-dotnet-get-started/notification-hub-configure-wns.png)

Vaše centrum oznámení je nyní nakonfigurované toowork s WNS a máte hello připojovací řetězce tooregister vaší aplikace a odesílání oznámení.

## <a name="connect-your-app-toohello-notification-hub"></a>Připojit vaše Centrum oznámení toohello aplikace
1. V sadě Visual Studio, klikněte pravým tlačítkem na řešení hello a pak klikněte na tlačítko **spravovat balíčky NuGet**.
   
    Zobrazí se hello **spravovat balíčky NuGet** dialogové okno.
2. Vyhledejte `WindowsAzure.Messaging.Managed` a klikněte na tlačítko **nainstalovat**a přijměte podmínky použití hello.
   
    ![][20]
   
    To stáhne, nainstaluje a přidá knihovnu zasílání zpráv Azure toohello referenční dokumentace pro systém Windows pomocí hello <a href="http://nuget.org/packages/WindowsAzure.Messaging.Managed/">balíčku nuget WindowsAzure.Messaging.Managed</a>.
3. Otevřete soubor projektu App.xaml.cs hello a přidejte následující hello `using` příkazy. 
   
        using Windows.Networking.PushNotifications;
        using Microsoft.WindowsAzure.Messaging;
        using Windows.UI.Popups;
4. Také v souboru App.xaml.cs přidejte následující hello **InitNotificationsAsync** metoda definice toohello **aplikace** třídy:
   
        private async void InitNotificationsAsync()
        {
            var channel = await PushNotificationChannelManager.CreatePushNotificationChannelForApplicationAsync();
   
            var hub = new NotificationHub("< your hub name>", "<Your DefaultListenSharedAccessSignature connection string>");
            var result = await hub.RegisterNativeAsync(channel.Uri);
   
            // Displays hello registration ID so you know it was successful
            if (result.RegistrationId != null)
            {
                var dialog = new MessageDialog("Registration successful: " + result.RegistrationId);
                dialog.Commands.Add(new UICommand("OK"));
                await dialog.ShowAsync();
            }
   
        }
   
    Tento kód načte hello kanál URI pro hello aplikaci z WNS a pak zaregistruje tento kanál URI pomocí centra oznámení.
   
   > [!NOTE]
   > Ujistěte se, že tooreplace hello zástupný symbol "název centra" hello název centra oznámení hello, který se zobrazí v hello portálu Azure. Také nahraďte zástupný symbol připojovacího řetězce hello hello **DefaultListenSharedAccessSignature** připojovací řetězec, který jste získali z hello **zásady přístupu** stránky centra oznámení v předchozí části.
   > 
   > 
5. Hello horní části hello **OnLaunched** obslužné rutiny událostí v souboru App.xaml.cs přidejte následující volání toohello nové hello **InitNotificationsAsync** metoda:
   
        InitNotificationsAsync();
   
    To zaručuje, že hello kanál URI je registrován v centru oznámení, které jednotlivé aplikace hello čas spuštění.
6. Stiskněte klávesu hello **F5** klíče toorun hello aplikace. Se zobrazí automaticky otevírané okno dialog, který obsahuje registrační klíč hello.

Aplikace je nyní připraven tooreceive informační zprávy.

## <a name="send-notifications"></a>Odeslat oznámení
Můžete rychle otestovat příjem oznámení ve vaší aplikaci odesláním oznámení hello [portálu Azure](https://portal.azure.com/) pomocí hello **testovací odeslání** tlačítko hello centra oznámení, jak je znázorněno níže úvodní obrazovka.

![](./media/notification-hubs-windows-store-dotnet-get-started/notification-hub-test-send-wns.png)

Nabízená oznámení se většinou posílají ve službě back-end, jako je služba Mobile Services, nebo v technologii ASP.NET pomocí kompatibilní knihovny. Můžete taky hello REST API přímo toosend oznámení zprávy Pokud knihovny není k dispozici pro back-end. 

V tomto kurzu jsme dělat nic složitého a jednoduše předvedeme testování vaší klientské aplikace pomocí odesílání oznámení hello .NET SDK pro centra oznámení v konzolové aplikaci, místo back-end službu. Doporučujeme, abyste hello [použití centra oznámení toopush oznámení toousers] kurz jako hello další krok pro odesílání oznámení z backendu ASP.NET. Hello následující přístupy lze však použít pro zasílání oznámení:

* **Rozhraní REST**: oznámení můžete podporovat na jakékoli platformě back-end pomocí hello [rozhraní REST](http://msdn.microsoft.com/library/windowsazure/dn223264.aspx).
* **Microsoft Azure oznámení centra .NET SDK**: hello Správce balíčků Nuget pro Visual Studio, spusťte [Install-Package Microsoft.Azure.NotificationHubs](https://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/).
* **Node.js** : [jak toouse Notification Hubs z Node.js](notification-hubs-nodejs-push-notification-tutorial.md).
* **Azure Mobile Apps**: pro příklad toosend oznámení z mobilní aplikace Azure, které jsou integrovány v centrech oznámení najdete v části [přidat nabízená oznámení pro Mobile Apps](../app-service-mobile/app-service-mobile-windows-store-dotnet-get-started-push.md).
* **Java / PHP**: Příklad jak hello toosend oznámení pomocí rozhraní REST API, naleznete v části "jak toouse centra oznámení z Java/PHP" ([Java](notification-hubs-java-push-notification-tutorial.md) | [PHP](notification-hubs-php-push-notification-tutorial.md)).

## <a name="optional-send-notifications-from-a-console-app"></a>(Volitelné) Odesílání oznámení z konzoly aplikace
toosend oznámení pomocí konzolové aplikace .NET postupujte podle těchto kroků. 

1. Hello řešení klikněte pravým tlačítkem, vyberte **přidat** a **nový projekt...** a potom v části **Visual C#**, klikněte na tlačítko **Windows** a **konzolové aplikace**a klikněte na tlačítko **OK**.
   
    Tento postup přidá nové Visual C# konzole aplikace toohello řešení. Tento postup také můžete využít v samostatném řešení.

2. Ve Visual Studiu klikněte na položku **Nástroje**, klikněte na **Správce balíčků NuGet** a pak klikněte na **Konzola Správce balíčků**.
   
    V sadě Visual Studio se zobrazí hello Konzola správce balíčků.
3. V okně konzoly Správce balíčků hello, nastavte hello **výchozí projekt** tooyour nové konzoly projekt aplikace a potom v okně konzoly hello, spusťte následující příkaz hello:
   
        Install-Package Microsoft.Azure.NotificationHubs
   
    Tento postup přidá odkaz toohello SDK centra oznámení Azure pomocí hello <a href="http://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/">balíčku Microsoft.Azure.Notification Hubs NuGet</a>.
   
    ![](./media/notification-hubs-windows-store-dotnet-get-started/notification-hub-package-manager.png)
4. Otevřete soubor Program.cs hello a přidejte následující hello `using` příkaz:
   
        using Microsoft.Azure.NotificationHubs;
5. V hello **Program** třídy, přidejte následující metodu hello:
   
        private static async void SendNotificationAsync()
        {
            NotificationHubClient hub = NotificationHubClient
                .CreateClientFromConnectionString("<connection string with full access>", "<hub name>");
            var toast = @"<toast><visual><binding template=""ToastText01""><text id=""1"">Hello from a .NET App!</text></binding></visual></toast>";
            await hub.SendWindowsNativeNotificationAsync(toast);
        }
   
       Make sure tooreplace hello "hub name" placeholder with hello name of hello notification hub that as it appears in hello Azure Portal. Also, replace hello connection string placeholder with hello **DefaultFullSharedAccessSignature** connection string that you obtained from hello **Access Policies** page of your Notification Hub in hello section called "Configure your notification hub."
   
   > [!NOTE]
   > Ujistěte se, že používáte hello připojovací řetězec, který má **úplné** přístupem, nikoli **naslouchání** přístup. řetězec s přístupem k naslouchání Hello nemá oprávnění toosend oznámení.
   > 
   > 
6. Přidejte následující řádky do hello hello **hlavní** metoda:
   
         SendNotificationAsync();
         Console.ReadLine();
7. Klikněte pravým tlačítkem na projekt konzolové aplikace hello v sadě Visual Studio a klikněte na tlačítko **nastavit jako spouštěný projekt** tooset ho jako projekt po spuštění hello. Stiskněte klávesu hello **F5** klíče toorun hello aplikace.
   
    Na všech registrovaných zařízeních se zobrazí informační zprávy. Kliknutí nebo klepnutí hello informační nápis načte aplikaci hello.

Můžete najít všechny hello podporované datové části v hello [katalog informační zprávy], [katalog dlaždic], a [přehled odznaků] témata na webu MSDN.

## <a name="next-steps"></a>Další kroky
V tomto jednoduchém příkladu jste zaslali oznámení vysílání tooall vaše zařízení s Windows pomocí portálu hello nebo konzolovou aplikaci. Doporučujeme, abyste hello [použití centra oznámení toopush oznámení toousers] kurz jako další krok hello. Zobrazí se jak toosend oznámení z back-end ASP.NET pomocí značky tootarget konkrétním uživatelům.

Pokud chcete toosegment uživatele podle zájmových skupin, najdete v části [toosend použití centra oznámení nejnovější zprávy přes]. 

toolearn další obecné informace o centrech oznámení naleznete v [pokyny centra oznámení](notification-hubs-push-notification-overview.md).

<!-- Images. -->
[13]: ./media/notification-hubs-windows-store-dotnet-get-started/notification-hub-create-console-app.png
[14]: ./media/notification-hubs-windows-store-dotnet-get-started/notification-hub-windows-toast.png
[19]: ./media/notification-hubs-windows-store-dotnet-get-started/notification-hub-windows-reg.png
[20]: ./media/notification-hubs-windows-store-dotnet-get-started/notification-hub-windows-universal-app-install-package.png

<!-- URLs. -->

[použití centra oznámení toopush oznámení toousers]: notification-hubs-aspnet-backend-windows-dotnet-wns-notification.md
[toosend použití centra oznámení nejnovější zprávy přes]: notification-hubs-windows-notification-dotnet-push-xplat-segmented-wns.md

[katalog informační zprávy]: http://msdn.microsoft.com/library/windows/apps/hh761494.aspx
[katalog dlaždic]: http://msdn.microsoft.com/library/windows/apps/hh761491.aspx
[přehled odznaků]: http://msdn.microsoft.com/library/windows/apps/hh779719.aspx
