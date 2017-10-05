---
title: "Odesílání nabízených oznámení pomocí Azure Notification Hubs ve Windows Phone | Dokumentace Microsoftu"
description: "V tomto kurzu zjistíte, jak používat Azure Notification Hubs k odesílání nabízených oznámení do aplikace Windows Phone 8 nebo Windows Phone 8.1 Silverlight."
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
ms.openlocfilehash: f0bfe81f849813d146d644b32490af657b1071b5
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="sending-push-notifications-with-azure-notification-hubs-on-windows-phone"></a><span data-ttu-id="bb37c-104">Odesílání nabízených oznámení pomocí Azure Notification Hubs ve Windows Phone</span><span class="sxs-lookup"><span data-stu-id="bb37c-104">Sending push notifications with Azure Notification Hubs on Windows Phone</span></span>
[!INCLUDE [notification-hubs-selector-get-started](../../includes/notification-hubs-selector-get-started.md)]

## <a name="overview"></a><span data-ttu-id="bb37c-105">Přehled</span><span class="sxs-lookup"><span data-stu-id="bb37c-105">Overview</span></span>
> [!NOTE]
> <span data-ttu-id="bb37c-106">K dokončení tohoto kurzu potřebujete mít aktivní účet Azure.</span><span class="sxs-lookup"><span data-stu-id="bb37c-106">To complete this tutorial, you must have an active Azure account.</span></span> <span data-ttu-id="bb37c-107">Pokud účet nemáte, můžete si během několika minut vytvořit bezplatný zkušební účet.</span><span class="sxs-lookup"><span data-stu-id="bb37c-107">If you don't have an account, you can create a free trial account in just a couple of minutes.</span></span> <span data-ttu-id="bb37c-108">Podrobnosti najdete v článku [Bezplatná zkušební verze Azure](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A0E0E5C02&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdocumentation%2Farticles%2Fnotification-hubs-windows-phone-get-started%2F).</span><span class="sxs-lookup"><span data-stu-id="bb37c-108">For details, see [Azure Free Trial](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A0E0E5C02&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdocumentation%2Farticles%2Fnotification-hubs-windows-phone-get-started%2F).</span></span>
> 
> 

<span data-ttu-id="bb37c-109">V tomto kurzu zjistíte, jak používat Azure Notification Hubs k odesílání nabízených oznámení do aplikace Windows Phone 8 nebo Windows Phone 8.1 Silverlight.</span><span class="sxs-lookup"><span data-stu-id="bb37c-109">This tutorial shows you how to use Azure Notification Hubs to send push notifications to a Windows Phone 8 or Windows Phone 8.1 Silverlight application.</span></span> <span data-ttu-id="bb37c-110">Pokud cílíte na Windows Phone 8.1 (ne Silverlight), přečtěte si informace k verzi [Univerzální pro Windows](notification-hubs-windows-store-dotnet-get-started-wns-push-notification.md).</span><span class="sxs-lookup"><span data-stu-id="bb37c-110">If you are targeting Windows Phone 8.1 (non-Silverlight), then refer to the [Windows Universal](notification-hubs-windows-store-dotnet-get-started-wns-push-notification.md) version.</span></span>
<span data-ttu-id="bb37c-111">V tomto kurzu vytvoříte prázdnou aplikaci pro Windows Phone 8, která obdrží nabízená oznámení pomocí služby nabízených oznámení Microsoft (MPNS).</span><span class="sxs-lookup"><span data-stu-id="bb37c-111">In this tutorial, you create a blank Windows Phone 8 app that receives push notifications by using the Microsoft Push Notification Service (MPNS).</span></span> <span data-ttu-id="bb37c-112">Jakmile budete hotovi, budete moci používat vaše centra oznámení k vysílání nabízených oznámení pro všechna zařízení používající vaši aplikaci.</span><span class="sxs-lookup"><span data-stu-id="bb37c-112">When you're finished, you'll be able to use your notification hub to broadcast push notifications to all the devices running your app.</span></span>

> [!NOTE]
> <span data-ttu-id="bb37c-113">Sada SDK centra oznámení Windows Phone nepodporuje použití služby nabízených oznámení Windows (WNS) s aplikacemi pro Windows Phone 8.1 Silverlight.</span><span class="sxs-lookup"><span data-stu-id="bb37c-113">The Notification Hubs Windows Phone SDK does not support using the Windows Push Notification Service (WNS) with Windows Phone 8.1 Silverlight apps.</span></span> <span data-ttu-id="bb37c-114">Chcete-li používat WNS (namísto MPNS) s aplikacemi pro Windows Phone 8.1 Silverlight, postupujte podle [kurzu Centra oznámení – Windows Phone Silverlight], který používá rozhraní REST API.</span><span class="sxs-lookup"><span data-stu-id="bb37c-114">To use WNS (instead of MPNS) with Windows Phone 8.1 Silverlight apps, follow the [Notification Hubs - Windows Phone Silverlight tutorial], which uses REST APIs.</span></span>
> 
> 

<span data-ttu-id="bb37c-115">Tento kurz představuje scénář jednoduchého vysílání přes centra oznámení.</span><span class="sxs-lookup"><span data-stu-id="bb37c-115">The tutorial demonstrates the simple broadcast scenario in using Notification Hubs.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="bb37c-116">Požadavky</span><span class="sxs-lookup"><span data-stu-id="bb37c-116">Prerequisites</span></span>
<span data-ttu-id="bb37c-117">V tomto kurzu budete potřebovat následující:</span><span class="sxs-lookup"><span data-stu-id="bb37c-117">This tutorial requires the following:</span></span>

* <span data-ttu-id="bb37c-118">[Visual Studio 2012 Express pro Windows Phone] nebo novější verzi</span><span class="sxs-lookup"><span data-stu-id="bb37c-118">[Visual Studio 2012 Express for Windows Phone], or a later version.</span></span>

<span data-ttu-id="bb37c-119">Dokončení tohoto kurzu je předpokladem pro všechny ostatní kurzy Notification Hubs pro aplikace Windows Phone 8.</span><span class="sxs-lookup"><span data-stu-id="bb37c-119">Completing this tutorial is a prerequisite for all other Notification Hubs tutorials for Windows Phone 8 apps.</span></span>

## <a name="create-your-notification-hub"></a><span data-ttu-id="bb37c-120">Vytvoření centra oznámení</span><span class="sxs-lookup"><span data-stu-id="bb37c-120">Create your notification hub</span></span>
[!INCLUDE [notification-hubs-portal-create-new-hub](../../includes/notification-hubs-portal-create-new-hub.md)]

<ol start="6">
<li><p><span data-ttu-id="bb37c-121">Klikněte na část <b>Služby oznámení</b> (v rámci <i>Nastavení</i>), klikněte na položku <b>Windows Phone (MPNS)</b> a pak klikněte na políčko <b>Povolit neověřené nabízená oznámení</b>.</span><span class="sxs-lookup"><span data-stu-id="bb37c-121">Click the <b>Notification Services</b> section (within <i>Settings</i>), click on <b>Windows Phone (MPNS)</b> and then click the <b>Enable unauthenticated push</b> check box.</span></span></p>
</li>
</ol>

&emsp;&emsp;![Portál Azure – povolit neověřená nabízená oznámení](./media/notification-hubs-windows-phone-get-started/azure-portal-unauth.png)

<span data-ttu-id="bb37c-123">Centrum se teď vytvoří a nakonfiguruje pro odeslání neověřeného oznámení pro Windows Phone.</span><span class="sxs-lookup"><span data-stu-id="bb37c-123">Your hub is now created and configured to send unauthenticated notification for Windows Phone.</span></span>

> [!NOTE]
> <span data-ttu-id="bb37c-124">Tento kurz využívá MPNS v neověřeném režimu.</span><span class="sxs-lookup"><span data-stu-id="bb37c-124">This tutorial uses MPNS in unauthenticated mode.</span></span> <span data-ttu-id="bb37c-125">Neověřený režim MPNS přichází s omezeními na oznámení, která můžete odeslat na každý kanál.</span><span class="sxs-lookup"><span data-stu-id="bb37c-125">MPNS unauthenticated mode comes with restrictions on notifications that you can send to each channel.</span></span> <span data-ttu-id="bb37c-126">Centra oznámení podporují [ověřený režim MPNS](http://msdn.microsoft.com/library/windowsphone/develop/ff941099.aspx) povolením odeslání vašeho certifikátu.</span><span class="sxs-lookup"><span data-stu-id="bb37c-126">Notification Hubs supports [MPNS authenticated mode](http://msdn.microsoft.com/library/windowsphone/develop/ff941099.aspx) by allowing you to upload your certificate.</span></span>
> 
> 

## <a name="connecting-your-app-to-the-notification-hub"></a><span data-ttu-id="bb37c-127">Připojování aplikace k centru oznámení</span><span class="sxs-lookup"><span data-stu-id="bb37c-127">Connecting your app to the notification hub</span></span>
1. <span data-ttu-id="bb37c-128">V sadě Visual Studio vytvořte novou aplikaci pro Windows Phone 8.</span><span class="sxs-lookup"><span data-stu-id="bb37c-128">In Visual Studio, create a new Windows Phone 8 application.</span></span>
   
       ![Visual Studio - New Project - Windows Phone App][13]
   
    <span data-ttu-id="bb37c-129">Ve Visual Studio 2013 Update 2 nebo novější verzi místo toho vytvořte aplikaci Windows Phone Silverlight.</span><span class="sxs-lookup"><span data-stu-id="bb37c-129">In Visual Studio 2013 Update 2 or later, you instead create a Windows Phone Silverlight application.</span></span>
   
    ![Visual Studio – Nový projekt – Prázdná aplikace – Windows Phone Silverlight][11]
2. <span data-ttu-id="bb37c-131">Ve Visual Studiu klikněte pravým tlačítkem na řešení a potom na **Spravovat balíčky NuGet**.</span><span class="sxs-lookup"><span data-stu-id="bb37c-131">In Visual Studio, right-click the solution, and then click **Manage NuGet Packages**.</span></span>
   
    <span data-ttu-id="bb37c-132">Zobrazí se dialogové okno **Správa balíčků NuGet**.</span><span class="sxs-lookup"><span data-stu-id="bb37c-132">This displays the **Manage NuGet Packages** dialog box.</span></span>
3. <span data-ttu-id="bb37c-133">Vyhledejte `WindowsAzure.Messaging.Managed` a klikněte na tlačítko **Instalovat** a pak přijměte podmínky použití.</span><span class="sxs-lookup"><span data-stu-id="bb37c-133">Search for `WindowsAzure.Messaging.Managed` and click **Install**, and then accept the terms of use.</span></span>
   
    ![Visual Studio – Správce balíčků NuGet][20]
   
    <span data-ttu-id="bb37c-135">Tento správce stáhne, nainstaluje a přidá odkaz na knihovnu zasílání zpráv Azure pro Windows pomocí <a href="http://nuget.org/packages/WindowsAzure.Messaging.Managed/">balíčku NuGet WindowsAzure.Messaging.Managed</a>.</span><span class="sxs-lookup"><span data-stu-id="bb37c-135">This downloads, installs, and adds a reference to the Azure Messaging library for Windows by using the <a href="http://nuget.org/packages/WindowsAzure.Messaging.Managed/">WindowsAzure.Messaging.Managed NuGet package</a>.</span></span>
4. <span data-ttu-id="bb37c-136">Otevřete soubor App.xaml.cs a přidejte následující příkazy `using`:</span><span class="sxs-lookup"><span data-stu-id="bb37c-136">Open the file App.xaml.cs and add the following `using` statements:</span></span>
   
        using Microsoft.Phone.Notification;
        using Microsoft.WindowsAzure.Messaging;
5. <span data-ttu-id="bb37c-137">Přidejte následující kód v horní části metody **Application_Launching** v souboru App.xaml.cs:</span><span class="sxs-lookup"><span data-stu-id="bb37c-137">Add the following code at the top of **Application_Launching** method in App.xaml.cs:</span></span>
   
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
   > <span data-ttu-id="bb37c-138">Hodnota **MyPushChannel** je index, který se použije k vyhledání existujícího kanálu v kolekci [HttpNotificationChannel](https://msdn.microsoft.com/library/windows/apps/microsoft.phone.notification.httpnotificationchannel.aspx).</span><span class="sxs-lookup"><span data-stu-id="bb37c-138">The value **MyPushChannel** is an index that is used to lookup an existing channel in the [HttpNotificationChannel](https://msdn.microsoft.com/library/windows/apps/microsoft.phone.notification.httpnotificationchannel.aspx) collection.</span></span> <span data-ttu-id="bb37c-139">Pokud zde není k dispozici, vytvořte novou položku s tímto názvem.</span><span class="sxs-lookup"><span data-stu-id="bb37c-139">If there isn't one there, create a new entry with that name.</span></span>
   > 
   > 
   
    <span data-ttu-id="bb37c-140">Nezapomeňte vložit název rozbočovače a připojovací řetězec s názvem **DefaultListenSharedAccessSignature**, který jste získali v předchozím oddílu.</span><span class="sxs-lookup"><span data-stu-id="bb37c-140">Make sure to insert the name of your hub and the connection string called **DefaultListenSharedAccessSignature** that you obtained in the previous section.</span></span>
    <span data-ttu-id="bb37c-141">Tento kód načte identifikátor URI kanálu pro aplikaci z MPNS a pak zaregistruje tento kanál URI pomocí centra oznámení.</span><span class="sxs-lookup"><span data-stu-id="bb37c-141">This code retrieves the channel URI for the app from MPNS, and then registers that channel URI with your notification hub.</span></span> <span data-ttu-id="bb37c-142">Také zaručuje, že kanál URI je registrován v centru oznámení pokaždé, když je aplikace spuštěna.</span><span class="sxs-lookup"><span data-stu-id="bb37c-142">It also guarantees that the channel URI is registered in your notification hub each time the application is launched.</span></span>
   
   > [!NOTE]
   > <span data-ttu-id="bb37c-143">V tomto kurzu se odešle informační zpráva do zařízení.</span><span class="sxs-lookup"><span data-stu-id="bb37c-143">This tutorial sends a toast notification to the device.</span></span> <span data-ttu-id="bb37c-144">Při odesílání oznámení dlaždice musíte místo toho volat metodu **BindToShellTile** na kanálu.</span><span class="sxs-lookup"><span data-stu-id="bb37c-144">When you send a tile notification, you must instead call the **BindToShellTile** method on the channel.</span></span> <span data-ttu-id="bb37c-145">Pro podporu obou oznámení a oznámení v dlaždici volejte jak metodu **BindToShellTile**, tak i metodu **BindToShellToast**.</span><span class="sxs-lookup"><span data-stu-id="bb37c-145">To support both toast and tile notifications, call both **BindToShellTile** and  **BindToShellToast**.</span></span>
   > 
   > 
6. <span data-ttu-id="bb37c-146">V Průzkumníku řešení rozbalte **Vlastnosti**, otevřete soubor `WMAppManifest.xml`, klikněte na kartu **Možnosti** a ujistěte se, že je zaškrtnuta schopnost **ID_CAP_PUSH_NOTIFICATION**.</span><span class="sxs-lookup"><span data-stu-id="bb37c-146">In Solution Explorer, expand **Properties**, open the `WMAppManifest.xml` file, click the **Capabilities** tab, and make sure that the **ID_CAP_PUSH_NOTIFICATION** capability is checked.</span></span>
   
       ![Visual Studio - Windows Phone App Capabilities][14]
   
       This ensures that your app can receive push notifications. Without it, any attempt to send a push notification to the app will fail.
7. <span data-ttu-id="bb37c-147">Stiskněte klávesu `F5` a spusťte aplikaci.</span><span class="sxs-lookup"><span data-stu-id="bb37c-147">Press the `F5` key to run the app.</span></span>
   
    <span data-ttu-id="bb37c-148">V aplikaci se zobrazí zpráva registrace.</span><span class="sxs-lookup"><span data-stu-id="bb37c-148">A registration message is displayed in the app.</span></span>
8. <span data-ttu-id="bb37c-149">Zavřete aplikaci.</span><span class="sxs-lookup"><span data-stu-id="bb37c-149">Close the app.</span></span>  
   
   > [!NOTE]
   > <span data-ttu-id="bb37c-150">Pro příjem nabízených oznámení nesmí být aplikace spuštěná v popředí.</span><span class="sxs-lookup"><span data-stu-id="bb37c-150">To receive a toast push notification, the application must not be running in the foreground.</span></span>
   > 
   > 

## <a name="send-push-notifications-from-your-backend"></a><span data-ttu-id="bb37c-151">Odesílání nabízených oznámení z backendu</span><span class="sxs-lookup"><span data-stu-id="bb37c-151">Send push notifications from your backend</span></span>
<span data-ttu-id="bb37c-152">Pomocí našeho veřejného <a href="http://msdn.microsoft.com/library/windowsazure/dn223264.aspx">rozhraní REST</a> je možné pomocí center oznámení posílat nabízená oznámení z jakéhokoli backendu.</span><span class="sxs-lookup"><span data-stu-id="bb37c-152">You can send push notifications by using Notification Hubs from any backend via the public <a href="http://msdn.microsoft.com/library/windowsazure/dn223264.aspx">REST interface</a>.</span></span> <span data-ttu-id="bb37c-153">V tomto kurzu zašlete nabízená oznámení pomocí konzolové aplikace .NET.</span><span class="sxs-lookup"><span data-stu-id="bb37c-153">In this tutorial, you send push notifications using a .NET console application.</span></span> 

<span data-ttu-id="bb37c-154">Příklad odesílání nabízených oznámení z backendu ASP.NET WebAPI, který je integrovaný do Notification Hubs, najdete v článku [Azure Notification Hubs upozorňují uživatele pomocí backendu .NET](notification-hubs-aspnet-backend-windows-dotnet-wns-notification.md).</span><span class="sxs-lookup"><span data-stu-id="bb37c-154">For an example of how to send push notifications from an ASP.NET WebAPI backend that's integrated with Notification Hubs, see [Azure Notification Hubs Notify Users with .NET backend](notification-hubs-aspnet-backend-windows-dotnet-wns-notification.md).</span></span>  

<span data-ttu-id="bb37c-155">Příklad odesílání nabízených oznámení pomocí [rozhraní REST API](https://msdn.microsoft.com/library/azure/dn223264.aspx) najdete v článku [Jak používat Notification Hubs z Javy](notification-hubs-java-push-notification-tutorial.md) a [Jak používat Notification Hubs z PHP](notification-hubs-php-push-notification-tutorial.md).</span><span class="sxs-lookup"><span data-stu-id="bb37c-155">For an example of how to send push notifications by using the [REST APIs](https://msdn.microsoft.com/library/azure/dn223264.aspx), check out [How to use Notification Hubs from Java](notification-hubs-java-push-notification-tutorial.md) and [How to use Notification Hubs from PHP](notification-hubs-php-push-notification-tutorial.md).</span></span>

1. <span data-ttu-id="bb37c-156">Klikněte pravým tlačítkem myši na řešení, vyberte možnost **Přidat** a **Nový projekt...** a pak v části **Visual C#** klikněte na tlačítko **Windows** a **Konzolové aplikace** a klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="bb37c-156">Right-click the solution, select **Add** and **New Project...**, and then under **Visual C#**, click **Windows** and **Console Application**, and click **OK**.</span></span>
   
       ![Visual Studio - New Project - Console Application][6]
   
    <span data-ttu-id="bb37c-157">Tento postup přidá novou aplikaci Visual C# do řešení.</span><span class="sxs-lookup"><span data-stu-id="bb37c-157">This adds a new Visual C# console application to the solution.</span></span> <span data-ttu-id="bb37c-158">Tento postup také můžete využít v samostatném řešení.</span><span class="sxs-lookup"><span data-stu-id="bb37c-158">You can also do this in a separate solution.</span></span>
2. <span data-ttu-id="bb37c-159">Klikněte na položku **Nástroje**, klikněte na **Správce balíčků knihoven** a pak na **Konzola správce balíčků**.</span><span class="sxs-lookup"><span data-stu-id="bb37c-159">Click **Tools**, click **Library Package Manager**, and then click **Package Manager Console**.</span></span>
   
    <span data-ttu-id="bb37c-160">Tím se zobrazí Konzola Správce balíčků.</span><span class="sxs-lookup"><span data-stu-id="bb37c-160">This displays the Package Manager Console.</span></span>
3. <span data-ttu-id="bb37c-161">V okně **konzoly Správce balíčků** nastavte **Výchozí projekt** na nový projekt konzolové aplikace a pak v okně konzoly spusťte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="bb37c-161">In the **Package Manager Console** window, set the **Default project** to your new console application project, and then in the console window, execute the following command:</span></span>
   
       Install-Package Microsoft.Azure.NotificationHubs
   
   <span data-ttu-id="bb37c-162">Ten přidá odkaz na sadu SDK centra oznámení Azure pomocí <a href="http://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/">balíčku Microsoft.Azure.Notification Hubs NuGet</a>.</span><span class="sxs-lookup"><span data-stu-id="bb37c-162">This adds a reference to the Azure Notification Hubs SDK using the <a href="http://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/">Microsoft.Azure.Notification Hubs NuGet package</a>.</span></span>
4. <span data-ttu-id="bb37c-163">Otevřete soubor `Program.cs` a přidejte následující příkaz `using`:</span><span class="sxs-lookup"><span data-stu-id="bb37c-163">Open the `Program.cs` file and add the following `using` statement:</span></span>
   
        using Microsoft.Azure.NotificationHubs;
5. <span data-ttu-id="bb37c-164">Do třídy `Program` přidejte následující metodu:</span><span class="sxs-lookup"><span data-stu-id="bb37c-164">In the `Program` class, add the following method:</span></span>
   
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
   
    <span data-ttu-id="bb37c-165">Zástupný symbol `<hub name>` je nutné nahradit názvem centra oznámení, který se zobrazí na portálu.</span><span class="sxs-lookup"><span data-stu-id="bb37c-165">Make sure to replace the `<hub name>` placeholder with the name of the notification hub that appears in the portal.</span></span> <span data-ttu-id="bb37c-166">Také nahraďte zástupný symbol připojovacího řetězce připojovacím řetězcem s názvem **DefaultFullSharedAccessSignature**, který jste získali v části „Konfigurace centra oznámení“.</span><span class="sxs-lookup"><span data-stu-id="bb37c-166">Also, replace the connection string placeholder with the connection string called **DefaultFullSharedAccessSignature** that you obtained in the section "Configure your notification hub."</span></span>
   
   > [!NOTE]
   > <span data-ttu-id="bb37c-167">Ujistěte se, že používáte připojovací řetězec s **úplným** přístupem, nikoli s přístupem **Naslouchat**.</span><span class="sxs-lookup"><span data-stu-id="bb37c-167">Make sure that you use the connection string with **Full** access, not **Listen** access.</span></span> <span data-ttu-id="bb37c-168">Řetězec s přístupem k naslouchání neposkytuje oprávnění k zasílání nabízených oznámení.</span><span class="sxs-lookup"><span data-stu-id="bb37c-168">The listen-access string does not have permissions to send push notifications.</span></span>
   > 
   > 
6. <span data-ttu-id="bb37c-169">Do metody `Main` přidejte následující řádek:</span><span class="sxs-lookup"><span data-stu-id="bb37c-169">Add the following line in your `Main` method:</span></span>
   
         SendNotificationAsync();
         Console.ReadLine();
7. <span data-ttu-id="bb37c-170">Se spuštěným emulátorem systému Windows Phone a zavřenou aplikací nastavte projekt konzolové aplikace jako výchozí projekt po spuštění a pak stiskněte klávesu `F5` pro spuštění aplikace.</span><span class="sxs-lookup"><span data-stu-id="bb37c-170">With your Windows Phone emulator running and your app closed, set the console application project as the default startup project, and then press the `F5` key to run the app.</span></span>
   
    <span data-ttu-id="bb37c-171">Obdržíte nabízená oznámení.</span><span class="sxs-lookup"><span data-stu-id="bb37c-171">You will receive a toast push notification.</span></span> <span data-ttu-id="bb37c-172">Klepnutím na informační nápis načtete aplikaci.</span><span class="sxs-lookup"><span data-stu-id="bb37c-172">Tapping the toast banner loads the app.</span></span>

<span data-ttu-id="bb37c-173">Na webu MSDN můžete najít všechny možné datové části v tématech [katalog informačních zpráv] a [katalog dlaždic].</span><span class="sxs-lookup"><span data-stu-id="bb37c-173">You can find all the possible payloads in the [toast catalog] and [tile catalog] topics on MSDN.</span></span>

## <a name="next-steps"></a><span data-ttu-id="bb37c-174">Další kroky</span><span class="sxs-lookup"><span data-stu-id="bb37c-174">Next steps</span></span>
<span data-ttu-id="bb37c-175">V tomto jednoduchém příkladu jste vysílali nabízená oznámení pro všechna vaše zařízení Windows Phone 8.</span><span class="sxs-lookup"><span data-stu-id="bb37c-175">In this simple example, you broadcasted push notifications to all your Windows Phone 8 devices.</span></span> 

<span data-ttu-id="bb37c-176">Chcete-li se zaměřit na konkrétní uživatele, využijte kurz [Použití centra oznámení pro nabízená oznámení uživatelům].</span><span class="sxs-lookup"><span data-stu-id="bb37c-176">In order to target specific users, refer to the [Use Notification Hubs to push notifications to users] tutorial.</span></span> 

<span data-ttu-id="bb37c-177">Pokud chcete segmentovat uživatele podle zájmových skupin, můžete si přečíst kurz [Používání centra oznámení k odesílání novinek].</span><span class="sxs-lookup"><span data-stu-id="bb37c-177">If you want to segment your users by interest groups, you can read [Use Notification Hubs to send breaking news].</span></span> 

<span data-ttu-id="bb37c-178">Další informace o centrech oznámení najdete v [Průvodce centry oznámení].</span><span class="sxs-lookup"><span data-stu-id="bb37c-178">Learn more about how to use Notification Hubs in [Notification Hubs Guidance].</span></span>

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
<span data-ttu-id="bb37c-179">[Visual Studio 2012 Express pro Windows Phone]: https://go.microsoft.com/fwLink/p/?LinkID=268374</span><span class="sxs-lookup"><span data-stu-id="bb37c-179">[Visual Studio 2012 Express for Windows Phone]: https://go.microsoft.com/fwLink/p/?LinkID=268374</span></span>
<span data-ttu-id="bb37c-180">[Průvodce centry oznámení]: http://msdn.microsoft.com/library/jj927170.aspx</span><span class="sxs-lookup"><span data-stu-id="bb37c-180">[Notification Hubs Guidance]: http://msdn.microsoft.com/library/jj927170.aspx</span></span>
[MPNS authenticated mode]: http://msdn.microsoft.com/library/windowsphone/develop/ff941099(v=vs.105).aspx
<span data-ttu-id="bb37c-181">[Použití centra oznámení pro nabízená oznámení uživatelům]: notification-hubs-aspnet-backend-windows-dotnet-wns-notification.md</span><span class="sxs-lookup"><span data-stu-id="bb37c-181">[Use Notification Hubs to push notifications to users]: notification-hubs-aspnet-backend-windows-dotnet-wns-notification.md</span></span>
<span data-ttu-id="bb37c-182">[Používání centra oznámení k odesílání novinek]: notification-hubs-windows-phone-push-xplat-segmented-mpns-notification.md</span><span class="sxs-lookup"><span data-stu-id="bb37c-182">[Use Notification Hubs to send breaking news]: notification-hubs-windows-phone-push-xplat-segmented-mpns-notification.md</span></span>
<span data-ttu-id="bb37c-183">[katalog informačních zpráv]: http://msdn.microsoft.com/library/windowsphone/develop/jj662938(v=vs.105).aspx</span><span class="sxs-lookup"><span data-stu-id="bb37c-183">[toast catalog]: http://msdn.microsoft.com/library/windowsphone/develop/jj662938(v=vs.105).aspx</span></span>
<span data-ttu-id="bb37c-184">[katalog dlaždic]: http://msdn.microsoft.com/library/windowsphone/develop/hh202948(v=vs.105).aspx</span><span class="sxs-lookup"><span data-stu-id="bb37c-184">[tile catalog]: http://msdn.microsoft.com/library/windowsphone/develop/hh202948(v=vs.105).aspx</span></span>
<span data-ttu-id="bb37c-185">[kurzu Centra oznámení – Windows Phone Silverlight]: https://github.com/Azure/azure-notificationhubs-samples/tree/master/PushToSLPhoneApp</span><span class="sxs-lookup"><span data-stu-id="bb37c-185">[Notification Hubs - Windows Phone Silverlight tutorial]: https://github.com/Azure/azure-notificationhubs-samples/tree/master/PushToSLPhoneApp</span></span>

