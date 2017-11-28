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
# <a name="sending-push-notifications-with-azure-notification-hubs-on-windows-phone"></a><span data-ttu-id="1f1b5-104">Odesílání nabízených oznámení pomocí Azure Notification Hubs ve Windows Phone</span><span class="sxs-lookup"><span data-stu-id="1f1b5-104">Sending push notifications with Azure Notification Hubs on Windows Phone</span></span>
[!INCLUDE [notification-hubs-selector-get-started](../../includes/notification-hubs-selector-get-started.md)]

## <a name="overview"></a><span data-ttu-id="1f1b5-105">Přehled</span><span class="sxs-lookup"><span data-stu-id="1f1b5-105">Overview</span></span>
> [!NOTE]
> <span data-ttu-id="1f1b5-106">toocomplete tento kurz, musíte mít aktivní účet Azure.</span><span class="sxs-lookup"><span data-stu-id="1f1b5-106">toocomplete this tutorial, you must have an active Azure account.</span></span> <span data-ttu-id="1f1b5-107">Pokud účet nemáte, můžete si během několika minut vytvořit bezplatný zkušební účet.</span><span class="sxs-lookup"><span data-stu-id="1f1b5-107">If you don't have an account, you can create a free trial account in just a couple of minutes.</span></span> <span data-ttu-id="1f1b5-108">Podrobnosti najdete v článku [Bezplatná zkušební verze Azure](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A0E0E5C02&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdocumentation%2Farticles%2Fnotification-hubs-windows-phone-get-started%2F).</span><span class="sxs-lookup"><span data-stu-id="1f1b5-108">For details, see [Azure Free Trial](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A0E0E5C02&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdocumentation%2Farticles%2Fnotification-hubs-windows-phone-get-started%2F).</span></span>
> 
> 

<span data-ttu-id="1f1b5-109">Tento kurz ukazuje, jak Azure Notification Hubs toosend toouse nabízená oznámení aplikace Windows Phone 8 nebo Windows Phone 8.1 Silverlight tooa.</span><span class="sxs-lookup"><span data-stu-id="1f1b5-109">This tutorial shows you how toouse Azure Notification Hubs toosend push notifications tooa Windows Phone 8 or Windows Phone 8.1 Silverlight application.</span></span> <span data-ttu-id="1f1b5-110">Pokud cílíte na Windows Phone 8.1 (bez Silverlight), pak odkazovat toohello [univerzální pro Windows](notification-hubs-windows-store-dotnet-get-started-wns-push-notification.md) verze.</span><span class="sxs-lookup"><span data-stu-id="1f1b5-110">If you are targeting Windows Phone 8.1 (non-Silverlight), then refer toohello [Windows Universal](notification-hubs-windows-store-dotnet-get-started-wns-push-notification.md) version.</span></span>
<span data-ttu-id="1f1b5-111">V tomto kurzu vytvoříte prázdnou aplikaci Windows Phone 8, která obdrží nabízená oznámení pomocí hello služby nabízených oznámení Microsoft (MPNS).</span><span class="sxs-lookup"><span data-stu-id="1f1b5-111">In this tutorial, you create a blank Windows Phone 8 app that receives push notifications by using hello Microsoft Push Notification Service (MPNS).</span></span> <span data-ttu-id="1f1b5-112">Jakmile budete hotovi, budete moct toouse vaše toobroadcast centra oznámení nabízená oznámení tooall hello zařízení používající vaši aplikaci.</span><span class="sxs-lookup"><span data-stu-id="1f1b5-112">When you're finished, you'll be able toouse your notification hub toobroadcast push notifications tooall hello devices running your app.</span></span>

> [!NOTE]
> <span data-ttu-id="1f1b5-113">Hello centra oznámení Windows Phone SDK nepodporuje použití hello nabízené služby oznámení Windows (WNS) s aplikacemi pro Windows Phone 8.1 Silverlight.</span><span class="sxs-lookup"><span data-stu-id="1f1b5-113">hello Notification Hubs Windows Phone SDK does not support using hello Windows Push Notification Service (WNS) with Windows Phone 8.1 Silverlight apps.</span></span> <span data-ttu-id="1f1b5-114">toouse WNS (namísto MPNS) s aplikacemi pro Windows Phone 8.1 Silverlight, postupujte podle hello [kurzu centra oznámení – Windows Phone Silverlight], který používá rozhraní REST API.</span><span class="sxs-lookup"><span data-stu-id="1f1b5-114">toouse WNS (instead of MPNS) with Windows Phone 8.1 Silverlight apps, follow hello [Notification Hubs - Windows Phone Silverlight tutorial], which uses REST APIs.</span></span>
> 
> 

<span data-ttu-id="1f1b5-115">Hello kurz představuje scénář jednoduchého vysílání hello přes centra oznámení.</span><span class="sxs-lookup"><span data-stu-id="1f1b5-115">hello tutorial demonstrates hello simple broadcast scenario in using Notification Hubs.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="1f1b5-116">Požadavky</span><span class="sxs-lookup"><span data-stu-id="1f1b5-116">Prerequisites</span></span>
<span data-ttu-id="1f1b5-117">Tento kurz vyžaduje hello následující:</span><span class="sxs-lookup"><span data-stu-id="1f1b5-117">This tutorial requires hello following:</span></span>

* <span data-ttu-id="1f1b5-118">[Visual Studio 2012 Express pro Windows Phone] nebo novější verzi</span><span class="sxs-lookup"><span data-stu-id="1f1b5-118">[Visual Studio 2012 Express for Windows Phone], or a later version.</span></span>

<span data-ttu-id="1f1b5-119">Dokončení tohoto kurzu je předpokladem pro všechny ostatní kurzy Notification Hubs pro aplikace Windows Phone 8.</span><span class="sxs-lookup"><span data-stu-id="1f1b5-119">Completing this tutorial is a prerequisite for all other Notification Hubs tutorials for Windows Phone 8 apps.</span></span>

## <a name="create-your-notification-hub"></a><span data-ttu-id="1f1b5-120">Vytvoření centra oznámení</span><span class="sxs-lookup"><span data-stu-id="1f1b5-120">Create your notification hub</span></span>
[!INCLUDE [notification-hubs-portal-create-new-hub](../../includes/notification-hubs-portal-create-new-hub.md)]

<ol start="6">
<li><p><span data-ttu-id="1f1b5-121">Klikněte na tlačítko hello <b>služby oznámení</b> část (v rámci <i>nastavení</i>), klikněte na <b>Windows Phone (MPNS)</b> a pak klikněte na tlačítko hello <b>Povolit neověřené nabízená oznámení </b> zaškrtávací políčko.</span><span class="sxs-lookup"><span data-stu-id="1f1b5-121">Click hello <b>Notification Services</b> section (within <i>Settings</i>), click on <b>Windows Phone (MPNS)</b> and then click hello <b>Enable unauthenticated push</b> check box.</span></span></p>
</li>
</ol>

&emsp;&emsp;![Portál Azure – povolit neověřená nabízená oznámení](./media/notification-hubs-windows-phone-get-started/azure-portal-unauth.png)

<span data-ttu-id="1f1b5-123">Vaše centrum je teď vytvořený a nakonfigurovaný toosend neověřené oznámení pro Windows Phone.</span><span class="sxs-lookup"><span data-stu-id="1f1b5-123">Your hub is now created and configured toosend unauthenticated notification for Windows Phone.</span></span>

> [!NOTE]
> <span data-ttu-id="1f1b5-124">Tento kurz využívá MPNS v neověřeném režimu.</span><span class="sxs-lookup"><span data-stu-id="1f1b5-124">This tutorial uses MPNS in unauthenticated mode.</span></span> <span data-ttu-id="1f1b5-125">Neověřený režim MPNS přichází s omezeními na oznámení můžete odesílat tooeach kanál.</span><span class="sxs-lookup"><span data-stu-id="1f1b5-125">MPNS unauthenticated mode comes with restrictions on notifications that you can send tooeach channel.</span></span> <span data-ttu-id="1f1b5-126">Centra oznámení podporují [ověřený režim MPNS](http://msdn.microsoft.com/library/windowsphone/develop/ff941099.aspx) tak, že umožní tooupload svůj certifikát.</span><span class="sxs-lookup"><span data-stu-id="1f1b5-126">Notification Hubs supports [MPNS authenticated mode](http://msdn.microsoft.com/library/windowsphone/develop/ff941099.aspx) by allowing you tooupload your certificate.</span></span>
> 
> 

## <a name="connecting-your-app-toohello-notification-hub"></a><span data-ttu-id="1f1b5-127">Připojení aplikace toohello centra oznámení</span><span class="sxs-lookup"><span data-stu-id="1f1b5-127">Connecting your app toohello notification hub</span></span>
1. <span data-ttu-id="1f1b5-128">V sadě Visual Studio vytvořte novou aplikaci pro Windows Phone 8.</span><span class="sxs-lookup"><span data-stu-id="1f1b5-128">In Visual Studio, create a new Windows Phone 8 application.</span></span>
   
       ![Visual Studio - New Project - Windows Phone App][13]
   
    <span data-ttu-id="1f1b5-129">Ve Visual Studio 2013 Update 2 nebo novější verzi místo toho vytvořte aplikaci Windows Phone Silverlight.</span><span class="sxs-lookup"><span data-stu-id="1f1b5-129">In Visual Studio 2013 Update 2 or later, you instead create a Windows Phone Silverlight application.</span></span>
   
    ![Visual Studio – Nový projekt – Prázdná aplikace – Windows Phone Silverlight][11]
2. <span data-ttu-id="1f1b5-131">V sadě Visual Studio, klikněte pravým tlačítkem na řešení hello a pak klikněte na tlačítko **spravovat balíčky NuGet**.</span><span class="sxs-lookup"><span data-stu-id="1f1b5-131">In Visual Studio, right-click hello solution, and then click **Manage NuGet Packages**.</span></span>
   
    <span data-ttu-id="1f1b5-132">Zobrazí se hello **spravovat balíčky NuGet** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="1f1b5-132">This displays hello **Manage NuGet Packages** dialog box.</span></span>
3. <span data-ttu-id="1f1b5-133">Vyhledejte `WindowsAzure.Messaging.Managed` a klikněte na tlačítko **nainstalovat**a pak přijměte podmínky použití hello.</span><span class="sxs-lookup"><span data-stu-id="1f1b5-133">Search for `WindowsAzure.Messaging.Managed` and click **Install**, and then accept hello terms of use.</span></span>
   
    ![Visual Studio – Správce balíčků NuGet][20]
   
    <span data-ttu-id="1f1b5-135">To stáhne, nainstaluje a přidá knihovnu zasílání zpráv Azure toohello referenční dokumentace pro systém Windows pomocí hello <a href="http://nuget.org/packages/WindowsAzure.Messaging.Managed/">balíčku nuget WindowsAzure.Messaging.Managed</a>.</span><span class="sxs-lookup"><span data-stu-id="1f1b5-135">This downloads, installs, and adds a reference toohello Azure Messaging library for Windows by using hello <a href="http://nuget.org/packages/WindowsAzure.Messaging.Managed/">WindowsAzure.Messaging.Managed NuGet package</a>.</span></span>
4. <span data-ttu-id="1f1b5-136">Otevřete soubor hello App.xaml.cs a přidejte následující hello `using` příkazy:</span><span class="sxs-lookup"><span data-stu-id="1f1b5-136">Open hello file App.xaml.cs and add hello following `using` statements:</span></span>
   
        using Microsoft.Phone.Notification;
        using Microsoft.WindowsAzure.Messaging;
5. <span data-ttu-id="1f1b5-137">Přidejte následující kód na začátku hello hello **Application_Launching** metoda v souboru App.xaml.cs:</span><span class="sxs-lookup"><span data-stu-id="1f1b5-137">Add hello following code at hello top of **Application_Launching** method in App.xaml.cs:</span></span>
   
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
   > <span data-ttu-id="1f1b5-138">Hello hodnotu **MyPushChannel** je index, který je použité toolookup existujícího kanálu v hello [HttpNotificationChannel](https://msdn.microsoft.com/library/windows/apps/microsoft.phone.notification.httpnotificationchannel.aspx) kolekce.</span><span class="sxs-lookup"><span data-stu-id="1f1b5-138">hello value **MyPushChannel** is an index that is used toolookup an existing channel in hello [HttpNotificationChannel](https://msdn.microsoft.com/library/windows/apps/microsoft.phone.notification.httpnotificationchannel.aspx) collection.</span></span> <span data-ttu-id="1f1b5-139">Pokud zde není k dispozici, vytvořte novou položku s tímto názvem.</span><span class="sxs-lookup"><span data-stu-id="1f1b5-139">If there isn't one there, create a new entry with that name.</span></span>
   > 
   > 
   
    <span data-ttu-id="1f1b5-140">Zajistěte, aby tooinsert hello název připojovacího řetězce rozbočovače a hello volat **DefaultListenSharedAccessSignature** který jste získali v předchozí části hello.</span><span class="sxs-lookup"><span data-stu-id="1f1b5-140">Make sure tooinsert hello name of your hub and hello connection string called **DefaultListenSharedAccessSignature** that you obtained in hello previous section.</span></span>
    <span data-ttu-id="1f1b5-141">Tento kód načte hello kanál URI pro hello aplikaci z MPNS a pak zaregistruje tento kanál URI pomocí centra oznámení.</span><span class="sxs-lookup"><span data-stu-id="1f1b5-141">This code retrieves hello channel URI for hello app from MPNS, and then registers that channel URI with your notification hub.</span></span> <span data-ttu-id="1f1b5-142">Také zaručuje, že hello kanál URI je registrován v centru oznámení, které jednotlivé aplikace hello čas spuštění.</span><span class="sxs-lookup"><span data-stu-id="1f1b5-142">It also guarantees that hello channel URI is registered in your notification hub each time hello application is launched.</span></span>
   
   > [!NOTE]
   > <span data-ttu-id="1f1b5-143">V tomto kurzu odešle zařízení toohello oznámení s informační zprávou.</span><span class="sxs-lookup"><span data-stu-id="1f1b5-143">This tutorial sends a toast notification toohello device.</span></span> <span data-ttu-id="1f1b5-144">Při odesílání oznámení dlaždice musíte místo toho volat hello **BindToShellTile** metoda na hello kanálu.</span><span class="sxs-lookup"><span data-stu-id="1f1b5-144">When you send a tile notification, you must instead call hello **BindToShellTile** method on hello channel.</span></span> <span data-ttu-id="1f1b5-145">toosupport oznámení informační zprávy a dlaždice volání obě **BindToShellTile** a **BindToShellToast**.</span><span class="sxs-lookup"><span data-stu-id="1f1b5-145">toosupport both toast and tile notifications, call both **BindToShellTile** and  **BindToShellToast**.</span></span>
   > 
   > 
6. <span data-ttu-id="1f1b5-146">V Průzkumníku řešení rozbalte **vlastnosti**, otevřete hello `WMAppManifest.xml` souboru, klikněte na tlačítko hello **možnosti** kartě a ujistěte se, že hello **ID_CAP_PUSH_NOTIFICATION** funkce je zaškrtnuté.</span><span class="sxs-lookup"><span data-stu-id="1f1b5-146">In Solution Explorer, expand **Properties**, open hello `WMAppManifest.xml` file, click hello **Capabilities** tab, and make sure that hello **ID_CAP_PUSH_NOTIFICATION** capability is checked.</span></span>
   
       ![Visual Studio - Windows Phone App Capabilities][14]
   
       This ensures that your app can receive push notifications. Without it, any attempt toosend a push notification toohello app will fail.
7. <span data-ttu-id="1f1b5-147">Stiskněte klávesu hello `F5` klíče toorun hello aplikace.</span><span class="sxs-lookup"><span data-stu-id="1f1b5-147">Press hello `F5` key toorun hello app.</span></span>
   
    <span data-ttu-id="1f1b5-148">V aplikaci hello se zobrazí zpráva registrace.</span><span class="sxs-lookup"><span data-stu-id="1f1b5-148">A registration message is displayed in hello app.</span></span>
8. <span data-ttu-id="1f1b5-149">Zavřít hello aplikace.</span><span class="sxs-lookup"><span data-stu-id="1f1b5-149">Close hello app.</span></span>  
   
   > [!NOTE]
   > <span data-ttu-id="1f1b5-150">tooreceive nabízená oznámení, nesmí být hello aplikace spuštěná v popředí hello.</span><span class="sxs-lookup"><span data-stu-id="1f1b5-150">tooreceive a toast push notification, hello application must not be running in hello foreground.</span></span>
   > 
   > 

## <a name="send-push-notifications-from-your-backend"></a><span data-ttu-id="1f1b5-151">Odesílání nabízených oznámení z backendu</span><span class="sxs-lookup"><span data-stu-id="1f1b5-151">Send push notifications from your backend</span></span>
<span data-ttu-id="1f1b5-152">Můžete odesílat nabízená oznámení pomocí centra oznámení z jakéhokoli backendu prostřednictvím veřejného hello <a href="http://msdn.microsoft.com/library/windowsazure/dn223264.aspx">rozhraní REST</a>.</span><span class="sxs-lookup"><span data-stu-id="1f1b5-152">You can send push notifications by using Notification Hubs from any backend via hello public <a href="http://msdn.microsoft.com/library/windowsazure/dn223264.aspx">REST interface</a>.</span></span> <span data-ttu-id="1f1b5-153">V tomto kurzu zašlete nabízená oznámení pomocí konzolové aplikace .NET.</span><span class="sxs-lookup"><span data-stu-id="1f1b5-153">In this tutorial, you send push notifications using a .NET console application.</span></span> 

<span data-ttu-id="1f1b5-154">Příklad jak toosend nabízená oznámení z backendu ASP.NET WebAPI, které jsou integrovány v centrech oznámení naleznete v části [Azure upozornění uživatelů centra oznámení s .NET back-end](notification-hubs-aspnet-backend-windows-dotnet-wns-notification.md).</span><span class="sxs-lookup"><span data-stu-id="1f1b5-154">For an example of how toosend push notifications from an ASP.NET WebAPI backend that's integrated with Notification Hubs, see [Azure Notification Hubs Notify Users with .NET backend](notification-hubs-aspnet-backend-windows-dotnet-wns-notification.md).</span></span>  

<span data-ttu-id="1f1b5-155">Příklad jak toosend nabízená oznámení pomocí hello [rozhraní REST API](https://msdn.microsoft.com/library/azure/dn223264.aspx), podívejte se na [jak toouse centra oznámení z Java](notification-hubs-java-push-notification-tutorial.md) a [jak toouse Notification Hubs z PHP](notification-hubs-php-push-notification-tutorial.md) .</span><span class="sxs-lookup"><span data-stu-id="1f1b5-155">For an example of how toosend push notifications by using hello [REST APIs](https://msdn.microsoft.com/library/azure/dn223264.aspx), check out [How toouse Notification Hubs from Java](notification-hubs-java-push-notification-tutorial.md) and [How toouse Notification Hubs from PHP](notification-hubs-php-push-notification-tutorial.md).</span></span>

1. <span data-ttu-id="1f1b5-156">Hello řešení klikněte pravým tlačítkem, vyberte **přidat** a **nový projekt...** a potom v části **Visual C#**, klikněte na tlačítko **Windows** a **konzolové aplikace**a klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="1f1b5-156">Right-click hello solution, select **Add** and **New Project...**, and then under **Visual C#**, click **Windows** and **Console Application**, and click **OK**.</span></span>
   
       ![Visual Studio - New Project - Console Application][6]
   
    <span data-ttu-id="1f1b5-157">Tento postup přidá nové Visual C# konzole aplikace toohello řešení.</span><span class="sxs-lookup"><span data-stu-id="1f1b5-157">This adds a new Visual C# console application toohello solution.</span></span> <span data-ttu-id="1f1b5-158">Tento postup také můžete využít v samostatném řešení.</span><span class="sxs-lookup"><span data-stu-id="1f1b5-158">You can also do this in a separate solution.</span></span>
2. <span data-ttu-id="1f1b5-159">Klikněte na položku **Nástroje**, klikněte na **Správce balíčků knihoven** a pak na **Konzola správce balíčků**.</span><span class="sxs-lookup"><span data-stu-id="1f1b5-159">Click **Tools**, click **Library Package Manager**, and then click **Package Manager Console**.</span></span>
   
    <span data-ttu-id="1f1b5-160">Zobrazí se hello Konzola správce balíčků.</span><span class="sxs-lookup"><span data-stu-id="1f1b5-160">This displays hello Package Manager Console.</span></span>
3. <span data-ttu-id="1f1b5-161">V hello **Konzola správce balíčků** okno, sada hello **výchozí projekt** tooyour nové konzoly projekt aplikace a potom v okně konzoly hello, spusťte následující příkaz hello:</span><span class="sxs-lookup"><span data-stu-id="1f1b5-161">In hello **Package Manager Console** window, set hello **Default project** tooyour new console application project, and then in hello console window, execute hello following command:</span></span>
   
       Install-Package Microsoft.Azure.NotificationHubs
   
   <span data-ttu-id="1f1b5-162">Tento postup přidá odkaz toohello SDK centra oznámení Azure pomocí hello <a href="http://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/">balíčku Microsoft.Azure.Notification Hubs NuGet</a>.</span><span class="sxs-lookup"><span data-stu-id="1f1b5-162">This adds a reference toohello Azure Notification Hubs SDK using hello <a href="http://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/">Microsoft.Azure.Notification Hubs NuGet package</a>.</span></span>
4. <span data-ttu-id="1f1b5-163">Otevřete hello `Program.cs` souboru a přidejte následující hello `using` příkaz:</span><span class="sxs-lookup"><span data-stu-id="1f1b5-163">Open hello `Program.cs` file and add hello following `using` statement:</span></span>
   
        using Microsoft.Azure.NotificationHubs;
5. <span data-ttu-id="1f1b5-164">V hello `Program` třídy, přidejte následující metodu hello:</span><span class="sxs-lookup"><span data-stu-id="1f1b5-164">In hello `Program` class, add hello following method:</span></span>
   
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
   
    <span data-ttu-id="1f1b5-165">Ujistěte se, zda text hello tooreplace `<hub name>` zástupný symbol hello název centra oznámení hello, který se zobrazí na portálu hello.</span><span class="sxs-lookup"><span data-stu-id="1f1b5-165">Make sure tooreplace hello `<hub name>` placeholder with hello name of hello notification hub that appears in hello portal.</span></span> <span data-ttu-id="1f1b5-166">Také nahraďte zástupný symbol připojovacího řetězce hello s hello připojovací řetězec s názvem **DefaultFullSharedAccessSignature** který jste získali v části hello "Konfigurace centra oznámení".</span><span class="sxs-lookup"><span data-stu-id="1f1b5-166">Also, replace hello connection string placeholder with hello connection string called **DefaultFullSharedAccessSignature** that you obtained in hello section "Configure your notification hub."</span></span>
   
   > [!NOTE]
   > <span data-ttu-id="1f1b5-167">Ujistěte se, že používáte připojovací řetězec hello s **úplné** přístupem, nikoli **naslouchání** přístup.</span><span class="sxs-lookup"><span data-stu-id="1f1b5-167">Make sure that you use hello connection string with **Full** access, not **Listen** access.</span></span> <span data-ttu-id="1f1b5-168">řetězec s přístupem k naslouchání Hello nemá oprávnění toosend nabízená oznámení.</span><span class="sxs-lookup"><span data-stu-id="1f1b5-168">hello listen-access string does not have permissions toosend push notifications.</span></span>
   > 
   > 
6. <span data-ttu-id="1f1b5-169">Přidejte následující řádek ve hello vaší `Main` metoda:</span><span class="sxs-lookup"><span data-stu-id="1f1b5-169">Add hello following line in your `Main` method:</span></span>
   
         SendNotificationAsync();
         Console.ReadLine();
7. <span data-ttu-id="1f1b5-170">S váš Windows Phone spuštěným emulátorem vaší aplikace hello uzavřené, nastavte projekt konzolové aplikace jako hello výchozí spouštěný projekt a stiskněte klávesu hello `F5` klíče toorun hello aplikace.</span><span class="sxs-lookup"><span data-stu-id="1f1b5-170">With your Windows Phone emulator running and your app closed, set hello console application project as hello default startup project, and then press hello `F5` key toorun hello app.</span></span>
   
    <span data-ttu-id="1f1b5-171">Obdržíte nabízená oznámení.</span><span class="sxs-lookup"><span data-stu-id="1f1b5-171">You will receive a toast push notification.</span></span> <span data-ttu-id="1f1b5-172">Klepnutím hello informační nápis načte aplikaci hello.</span><span class="sxs-lookup"><span data-stu-id="1f1b5-172">Tapping hello toast banner loads hello app.</span></span>

<span data-ttu-id="1f1b5-173">Můžete najít všechny možné datové části hello v hello [katalog informační zprávy] a [katalog dlaždic] témata na webu MSDN.</span><span class="sxs-lookup"><span data-stu-id="1f1b5-173">You can find all hello possible payloads in hello [toast catalog] and [tile catalog] topics on MSDN.</span></span>

## <a name="next-steps"></a><span data-ttu-id="1f1b5-174">Další kroky</span><span class="sxs-lookup"><span data-stu-id="1f1b5-174">Next steps</span></span>
<span data-ttu-id="1f1b5-175">V tomto jednoduchém příkladu jste vysílali nabízená oznámení tooall zařízení Windows Phone 8.</span><span class="sxs-lookup"><span data-stu-id="1f1b5-175">In this simple example, you broadcasted push notifications tooall your Windows Phone 8 devices.</span></span> 

<span data-ttu-id="1f1b5-176">V pořadí tootarget konkrétní uživatele, získáte toohello [použití centra oznámení toopush oznámení toousers] kurzu.</span><span class="sxs-lookup"><span data-stu-id="1f1b5-176">In order tootarget specific users, refer toohello [Use Notification Hubs toopush notifications toousers] tutorial.</span></span> 

<span data-ttu-id="1f1b5-177">Pokud chcete toosegment uživatele podle zájmových skupin, můžete si přečíst [toosend použití centra oznámení nejnovější zprávy přes].</span><span class="sxs-lookup"><span data-stu-id="1f1b5-177">If you want toosegment your users by interest groups, you can read [Use Notification Hubs toosend breaking news].</span></span> 

<span data-ttu-id="1f1b5-178">Další informace o Notification Hubs toouse v [pokyny centra oznámení].</span><span class="sxs-lookup"><span data-stu-id="1f1b5-178">Learn more about how toouse Notification Hubs in [Notification Hubs Guidance].</span></span>

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

