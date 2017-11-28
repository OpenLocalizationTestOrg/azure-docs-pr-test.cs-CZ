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
# <a name="getting-started-with-notification-hubs-for-windows-universal-platform-apps"></a><span data-ttu-id="294af-103">Začínáme používat službu Notification Hubs pro aplikace Univerzální platformy Windows</span><span class="sxs-lookup"><span data-stu-id="294af-103">Getting started with Notification Hubs for Windows Universal Platform Apps</span></span>
[!INCLUDE [notification-hubs-selector-get-started](../../includes/notification-hubs-selector-get-started.md)]

## <a name="overview"></a><span data-ttu-id="294af-104">Přehled</span><span class="sxs-lookup"><span data-stu-id="294af-104">Overview</span></span>
<span data-ttu-id="294af-105">Tento kurz ukazuje, jak Azure Notification Hubs toosend toouse nabízená oznámení tooa univerzální platformu Windows (UWP) aplikace.</span><span class="sxs-lookup"><span data-stu-id="294af-105">This tutorial shows you how toouse Azure Notification Hubs toosend push notifications tooa Universal Windows Platform (UWP) app.</span></span>

<span data-ttu-id="294af-106">V tomto kurzu vytvoříte prázdnou aplikaci Windows Store, která obdrží nabízená oznámení pomocí hello služby nabízených oznámení Windows (WNS).</span><span class="sxs-lookup"><span data-stu-id="294af-106">In this tutorial, you create a blank Windows Store app that receives push notifications by using hello Windows Push Notification Service (WNS).</span></span> <span data-ttu-id="294af-107">Jakmile budete hotovi, budete moct toouse vaše toobroadcast centra oznámení nabízená oznámení tooall hello zařízení používající vaši aplikaci.</span><span class="sxs-lookup"><span data-stu-id="294af-107">When you're finished, you'll be able toouse your notification hub toobroadcast push notifications tooall hello devices running your app.</span></span>

## <a name="before-you-begin"></a><span data-ttu-id="294af-108">Než začnete</span><span class="sxs-lookup"><span data-stu-id="294af-108">Before you begin</span></span>
[!INCLUDE [notification-hubs-hero-slug](../../includes/notification-hubs-hero-slug.md)]

<span data-ttu-id="294af-109">Hello dokončit kód v tomto kurzu lze nalézt na Githubu [zde](https://github.com/Azure/azure-notificationhubs-samples/tree/master/dotnet/GetStartedWindowsUniversal).</span><span class="sxs-lookup"><span data-stu-id="294af-109">hello completed code for this tutorial can be found on GitHub [here](https://github.com/Azure/azure-notificationhubs-samples/tree/master/dotnet/GetStartedWindowsUniversal).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="294af-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="294af-110">Prerequisites</span></span>
<span data-ttu-id="294af-111">Tento kurz vyžaduje hello následující:</span><span class="sxs-lookup"><span data-stu-id="294af-111">This tutorial requires hello following:</span></span>

* <span data-ttu-id="294af-112">[Microsoft Visual Studio Community 2015](https://www.visualstudio.com/products/visual-studio-community-vs) nebo novější</span><span class="sxs-lookup"><span data-stu-id="294af-112">[Microsoft Visual Studio Community 2015](https://www.visualstudio.com/products/visual-studio-community-vs) or later</span></span>
* [<span data-ttu-id="294af-113">Nainstalované nástroje pro vývoj univerzálních aplikací pro Windows</span><span class="sxs-lookup"><span data-stu-id="294af-113">Universal Windows App Development Tools installed</span></span>](https://msdn.microsoft.com/windows/uwp/get-started/get-set-up)
* <span data-ttu-id="294af-114">Aktivní účet Azure</span><span class="sxs-lookup"><span data-stu-id="294af-114">An active Azure account</span></span> <br/><span data-ttu-id="294af-115">Pokud účet nemáte, můžete si během několika minut vytvořit bezplatný zkušební účet.</span><span class="sxs-lookup"><span data-stu-id="294af-115">If you don't have an account, you can create a free trial account in just a couple of minutes.</span></span> <span data-ttu-id="294af-116">Podrobnosti najdete v článku [Bezplatná zkušební verze Azure](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A0E0E5C02&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdocumentation%2Farticles%2Fnotification-hubs-windows-store-dotnet-get-started%2F).</span><span class="sxs-lookup"><span data-stu-id="294af-116">For details, see [Azure Free Trial](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A0E0E5C02&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdocumentation%2Farticles%2Fnotification-hubs-windows-store-dotnet-get-started%2F).</span></span>
* <span data-ttu-id="294af-117">Aktivní účet Windows Store</span><span class="sxs-lookup"><span data-stu-id="294af-117">An active Windows Store account</span></span>

<span data-ttu-id="294af-118">Dokončení tohoto kurzu je předpokladem pro všechny ostatní kurzy služby Notification Hubs pro aplikace Univerzální platformy Windows.</span><span class="sxs-lookup"><span data-stu-id="294af-118">Completing this tutorial is a prerequisite for all other Notification Hubs tutorials for Windows Universal Platform apps.</span></span>

## <a name="register-your-app-for-hello-windows-store"></a><span data-ttu-id="294af-119">Registrace aplikace pro Windows Store hello</span><span class="sxs-lookup"><span data-stu-id="294af-119">Register your app for hello Windows Store</span></span>
<span data-ttu-id="294af-120">toosend nabízená oznámení tooUWP aplikace, je nutné přidružit toohello vaší aplikace Windows Store.</span><span class="sxs-lookup"><span data-stu-id="294af-120">toosend push notifications tooUWP apps, you must associate your app toohello Windows Store.</span></span> <span data-ttu-id="294af-121">Pak musíte nakonfigurovat vaše toointegrate centra oznámení s WNS.</span><span class="sxs-lookup"><span data-stu-id="294af-121">You must then configure your notification hub toointegrate with WNS.</span></span>

1. <span data-ttu-id="294af-122">Pokud jste ještě nezaregistrovali vaší aplikace, přejděte toohello [Windows Dev Center](https://dev.windows.com/overview), přihlaste se pomocí účtu Microsoft a pak klikněte na tlačítko **vytvořit novou aplikaci**.</span><span class="sxs-lookup"><span data-stu-id="294af-122">If you have not already registered your app, navigate toohello [Windows Dev Center](https://dev.windows.com/overview), sign in with your Microsoft account, and then click **Create a new app**.</span></span>

2. <span data-ttu-id="294af-123">Zadejte název aplikace a klikněte na tlačítko **Rezervovat název aplikace**.</span><span class="sxs-lookup"><span data-stu-id="294af-123">Type a name for your app and click **Reserve app name**.</span></span> <span data-ttu-id="294af-124">Tím se vytvoří nová registrace Windows Store pro vaši aplikaci.</span><span class="sxs-lookup"><span data-stu-id="294af-124">This creates a new Windows Store registration for your app.</span></span>

3. <span data-ttu-id="294af-125">V sadě Visual Studio vytvořte nový projekt aplikace Visual C# Store pomocí univerzální pro Windows hello **prázdnou aplikaci** šablonu a klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="294af-125">In Visual Studio, create a new Visual C# Store Apps project by using hello Windows Universal **Blank App** template and click **OK**.</span></span>

4. <span data-ttu-id="294af-126">Přijměte výchozí hodnoty hello hello cíl a verze minimální platforem.</span><span class="sxs-lookup"><span data-stu-id="294af-126">Accept hello defaults for hello target and minimum platform versions.</span></span>

5. <span data-ttu-id="294af-127">V Průzkumníku řešení klikněte pravým tlačítkem na projekt aplikace Windows Store hello, klikněte na tlačítko **úložiště**a pak klikněte na tlačítko **propojit aplikaci se hello úložiště...** . hello **přidružením aplikace hello Windows Store** zobrazí se průvodce.</span><span class="sxs-lookup"><span data-stu-id="294af-127">In Solution Explorer, right-click hello Windows Store app project, click **Store**, and then click **Associate App with hello Store...**. hello **Associate Your App with hello Windows Store** wizard appears.</span></span>

6. <span data-ttu-id="294af-128">V Průvodci hello Přihlaste se pomocí účtu Microsoft.</span><span class="sxs-lookup"><span data-stu-id="294af-128">In hello wizard, sign in with your Microsoft account.</span></span>

7. <span data-ttu-id="294af-129">Klikněte na tlačítko hello aplikaci zaregistrovanou v kroku 2, klikněte na tlačítko **Další**a potom klikněte na **přidružit**.</span><span class="sxs-lookup"><span data-stu-id="294af-129">Click hello app that you registered in step 2, click **Next**, and then click **Associate**.</span></span> <span data-ttu-id="294af-130">Tento postup přidá požadované hello Windows Store registrační informace toohello manifest aplikace.</span><span class="sxs-lookup"><span data-stu-id="294af-130">This adds hello required Windows Store registration information toohello application manifest.</span></span>

8. <span data-ttu-id="294af-131">Zpět na hello [Windows Dev Center](http://dev.windows.com/overview) stránky pro novou aplikaci, klikněte na tlačítko **služby**, klikněte na tlačítko **nabízená oznámení**a potom klikněte na **WNS nebo MPNS**.</span><span class="sxs-lookup"><span data-stu-id="294af-131">Back on hello [Windows Dev Center](http://dev.windows.com/overview) page for your new app, click **Services**, click **Push notifications**, and then click **WNS/MPNS**.</span></span>

9. <span data-ttu-id="294af-132">Klikněte na **Nové oznámení**.</span><span class="sxs-lookup"><span data-stu-id="294af-132">Click **New Notification**.</span></span>

10. <span data-ttu-id="294af-133">Klikněte na šablonu **Prázdná (informační zpráva)** a potom klikněte na **OK**.</span><span class="sxs-lookup"><span data-stu-id="294af-133">Click **Blank (Toast)** template and then click **OK**.</span></span>

11. <span data-ttu-id="294af-134">Zadejte **Název** oznámení a vizuální **kontextovou** zprávu.</span><span class="sxs-lookup"><span data-stu-id="294af-134">Enter a notification **Name** and Visual **Context** message.</span></span> <span data-ttu-id="294af-135">Potom klikněte na **Uložit jako koncept**.</span><span class="sxs-lookup"><span data-stu-id="294af-135">Then click **Save as draft**.</span></span>

12. <span data-ttu-id="294af-136">Přejděte toohello [portálu pro registraci aplikace](http://apps.dev.microsoft.com) a přihlaste se.</span><span class="sxs-lookup"><span data-stu-id="294af-136">Navigate toohello [Application Registration Portal](http://apps.dev.microsoft.com) and log in.</span></span>

13. <span data-ttu-id="294af-137">Klikněte na název vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="294af-137">Click on your application name.</span></span> <span data-ttu-id="294af-138">Poznamenejte si hello **tajný klíč aplikace** heslo a hello **identifikátor zabezpečení (SID) balíčku** umístěný v hello **Windows Store** nastavení platformy.</span><span class="sxs-lookup"><span data-stu-id="294af-138">Make a note of hello **Application Secret** password and hello **Package security identifier (SID)** located in hello **Windows Store** platform settings.</span></span>

     > [AZURE.WARNING]
    <span data-ttu-id="294af-139">tajný klíč aplikace Hello a SID balíčku jsou důležitá pověření zabezpečení.</span><span class="sxs-lookup"><span data-stu-id="294af-139">hello application secret and package SID are important security credentials.</span></span> <span data-ttu-id="294af-140">Tyto hodnoty s nikým nesdílejte ani je nedistribuujte s vaší aplikací.</span><span class="sxs-lookup"><span data-stu-id="294af-140">Do not share these values with anyone or distribute them with your app.</span></span>

## <a name="configure-your-notification-hub"></a><span data-ttu-id="294af-141">Konfigurace centra oznámení</span><span class="sxs-lookup"><span data-stu-id="294af-141">Configure your notification hub</span></span>
[!INCLUDE [notification-hubs-portal-create-new-hub](../../includes/notification-hubs-portal-create-new-hub.md)]

<ol start="6">
<li><p><span data-ttu-id="294af-142">Vyberte hello <b>služby oznámení</b> možnost a hello <b>Windows (WNS)</b> možnost.</span><span class="sxs-lookup"><span data-stu-id="294af-142">Select hello <b>Notification Services</b> option and hello <b>Windows (WNS)</b> option.</span></span> <span data-ttu-id="294af-143">Potom zadejte hello <b>tajný klíč aplikace</b> heslo v hello <b>klíč zabezpečení</b> pole.</span><span class="sxs-lookup"><span data-stu-id="294af-143">Then enter hello <b>Application secret</b> password in hello <b>Security Key</b> field.</span></span> <span data-ttu-id="294af-144">Zadejte vaše <b>SID balíčku</b> hodnotu, který jste získali z WNS v předchozí části hello a pak klikněte na tlačítko <b>Uložit</b>.</span><span class="sxs-lookup"><span data-stu-id="294af-144">Enter your <b>Package SID</b> value that you obtained from WNS in hello previous section, and then click <b>Save</b>.</span></span></p>
</li>
</ol>

&emsp;&emsp;![](./media/notification-hubs-windows-store-dotnet-get-started/notification-hub-configure-wns.png)

<span data-ttu-id="294af-145">Vaše centrum oznámení je nyní nakonfigurované toowork s WNS a máte hello připojovací řetězce tooregister vaší aplikace a odesílání oznámení.</span><span class="sxs-lookup"><span data-stu-id="294af-145">Your notification hub is now configured toowork with WNS, and you have hello connection strings tooregister your app and send notifications.</span></span>

## <a name="connect-your-app-toohello-notification-hub"></a><span data-ttu-id="294af-146">Připojit vaše Centrum oznámení toohello aplikace</span><span class="sxs-lookup"><span data-stu-id="294af-146">Connect your app toohello notification hub</span></span>
1. <span data-ttu-id="294af-147">V sadě Visual Studio, klikněte pravým tlačítkem na řešení hello a pak klikněte na tlačítko **spravovat balíčky NuGet**.</span><span class="sxs-lookup"><span data-stu-id="294af-147">In Visual Studio, right-click hello solution, and then click **Manage NuGet Packages**.</span></span>
   
    <span data-ttu-id="294af-148">Zobrazí se hello **spravovat balíčky NuGet** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="294af-148">This displays hello **Manage NuGet Packages** dialog box.</span></span>
2. <span data-ttu-id="294af-149">Vyhledejte `WindowsAzure.Messaging.Managed` a klikněte na tlačítko **nainstalovat**a přijměte podmínky použití hello.</span><span class="sxs-lookup"><span data-stu-id="294af-149">Search for `WindowsAzure.Messaging.Managed` and click **Install**, and accept hello terms of use.</span></span>
   
    ![][20]
   
    <span data-ttu-id="294af-150">To stáhne, nainstaluje a přidá knihovnu zasílání zpráv Azure toohello referenční dokumentace pro systém Windows pomocí hello <a href="http://nuget.org/packages/WindowsAzure.Messaging.Managed/">balíčku nuget WindowsAzure.Messaging.Managed</a>.</span><span class="sxs-lookup"><span data-stu-id="294af-150">This downloads, installs, and adds a reference toohello Azure Messaging library for Windows by using hello <a href="http://nuget.org/packages/WindowsAzure.Messaging.Managed/">WindowsAzure.Messaging.Managed NuGet package</a>.</span></span>
3. <span data-ttu-id="294af-151">Otevřete soubor projektu App.xaml.cs hello a přidejte následující hello `using` příkazy.</span><span class="sxs-lookup"><span data-stu-id="294af-151">Open hello App.xaml.cs project file and add hello following `using` statements.</span></span> 
   
        using Windows.Networking.PushNotifications;
        using Microsoft.WindowsAzure.Messaging;
        using Windows.UI.Popups;
4. <span data-ttu-id="294af-152">Také v souboru App.xaml.cs přidejte následující hello **InitNotificationsAsync** metoda definice toohello **aplikace** třídy:</span><span class="sxs-lookup"><span data-stu-id="294af-152">Also in App.xaml.cs, add hello following **InitNotificationsAsync** method definition toohello **App** class:</span></span>
   
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
   
    <span data-ttu-id="294af-153">Tento kód načte hello kanál URI pro hello aplikaci z WNS a pak zaregistruje tento kanál URI pomocí centra oznámení.</span><span class="sxs-lookup"><span data-stu-id="294af-153">This code retrieves hello channel URI for hello app from WNS, and then registers that channel URI with your notification hub.</span></span>
   
   > [!NOTE]
   > <span data-ttu-id="294af-154">Ujistěte se, že tooreplace hello zástupný symbol "název centra" hello název centra oznámení hello, který se zobrazí v hello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="294af-154">Make sure tooreplace hello "your hub name" placeholder with hello name of hello notification hub that appears in hello Azure Portal.</span></span> <span data-ttu-id="294af-155">Také nahraďte zástupný symbol připojovacího řetězce hello hello **DefaultListenSharedAccessSignature** připojovací řetězec, který jste získali z hello **zásady přístupu** stránky centra oznámení v předchozí části.</span><span class="sxs-lookup"><span data-stu-id="294af-155">Also replace hello connection string placeholder with hello **DefaultListenSharedAccessSignature** connection string that you obtained from hello **Access Polices** page of your Notification Hub in a previous section.</span></span>
   > 
   > 
5. <span data-ttu-id="294af-156">Hello horní části hello **OnLaunched** obslužné rutiny událostí v souboru App.xaml.cs přidejte následující volání toohello nové hello **InitNotificationsAsync** metoda:</span><span class="sxs-lookup"><span data-stu-id="294af-156">At hello top of hello **OnLaunched** event handler in App.xaml.cs, add hello following call toohello new **InitNotificationsAsync** method:</span></span>
   
        InitNotificationsAsync();
   
    <span data-ttu-id="294af-157">To zaručuje, že hello kanál URI je registrován v centru oznámení, které jednotlivé aplikace hello čas spuštění.</span><span class="sxs-lookup"><span data-stu-id="294af-157">This guarantees that hello channel URI is registered in your notification hub each time hello application is launched.</span></span>
6. <span data-ttu-id="294af-158">Stiskněte klávesu hello **F5** klíče toorun hello aplikace.</span><span class="sxs-lookup"><span data-stu-id="294af-158">Press hello **F5** key toorun hello app.</span></span> <span data-ttu-id="294af-159">Se zobrazí automaticky otevírané okno dialog, který obsahuje registrační klíč hello.</span><span class="sxs-lookup"><span data-stu-id="294af-159">A pop-up dialog that contains hello registration key is displayed.</span></span>

<span data-ttu-id="294af-160">Aplikace je nyní připraven tooreceive informační zprávy.</span><span class="sxs-lookup"><span data-stu-id="294af-160">Your app is now ready tooreceive toast notifications.</span></span>

## <a name="send-notifications"></a><span data-ttu-id="294af-161">Odeslat oznámení</span><span class="sxs-lookup"><span data-stu-id="294af-161">Send notifications</span></span>
<span data-ttu-id="294af-162">Můžete rychle otestovat příjem oznámení ve vaší aplikaci odesláním oznámení hello [portálu Azure](https://portal.azure.com/) pomocí hello **testovací odeslání** tlačítko hello centra oznámení, jak je znázorněno níže úvodní obrazovka.</span><span class="sxs-lookup"><span data-stu-id="294af-162">You can quickly test receiving notifications in your app by sending notifications in hello [Azure Portal](https://portal.azure.com/) using hello **Test Send** button on hello notification hub, as shown in hello screen below.</span></span>

![](./media/notification-hubs-windows-store-dotnet-get-started/notification-hub-test-send-wns.png)

<span data-ttu-id="294af-163">Nabízená oznámení se většinou posílají ve službě back-end, jako je služba Mobile Services, nebo v technologii ASP.NET pomocí kompatibilní knihovny.</span><span class="sxs-lookup"><span data-stu-id="294af-163">Push notifications are normally sent in a back-end service like Mobile Services or ASP.NET using a compatible library.</span></span> <span data-ttu-id="294af-164">Můžete taky hello REST API přímo toosend oznámení zprávy Pokud knihovny není k dispozici pro back-end.</span><span class="sxs-lookup"><span data-stu-id="294af-164">You can also use hello REST API directly toosend notification messages if a library is not available for your back-end.</span></span> 

<span data-ttu-id="294af-165">V tomto kurzu jsme dělat nic složitého a jednoduše předvedeme testování vaší klientské aplikace pomocí odesílání oznámení hello .NET SDK pro centra oznámení v konzolové aplikaci, místo back-end službu.</span><span class="sxs-lookup"><span data-stu-id="294af-165">In this tutorial, we will keep it simple and just demonstrate testing your client app by sending notifications using hello .NET SDK for notification hubs in a console application instead of a backend service.</span></span> <span data-ttu-id="294af-166">Doporučujeme, abyste hello [použití centra oznámení toopush oznámení toousers] kurz jako hello další krok pro odesílání oznámení z backendu ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="294af-166">We recommend hello [Use Notification Hubs toopush notifications toousers] tutorial as hello next step for sending notifications from an ASP.NET backend.</span></span> <span data-ttu-id="294af-167">Hello následující přístupy lze však použít pro zasílání oznámení:</span><span class="sxs-lookup"><span data-stu-id="294af-167">However, hello following approaches can be used for sending notifications:</span></span>

* <span data-ttu-id="294af-168">**Rozhraní REST**: oznámení můžete podporovat na jakékoli platformě back-end pomocí hello [rozhraní REST](http://msdn.microsoft.com/library/windowsazure/dn223264.aspx).</span><span class="sxs-lookup"><span data-stu-id="294af-168">**REST Interface**:  You can support notification on any backend platform using  hello [REST interface](http://msdn.microsoft.com/library/windowsazure/dn223264.aspx).</span></span>
* <span data-ttu-id="294af-169">**Microsoft Azure oznámení centra .NET SDK**: hello Správce balíčků Nuget pro Visual Studio, spusťte [Install-Package Microsoft.Azure.NotificationHubs](https://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/).</span><span class="sxs-lookup"><span data-stu-id="294af-169">**Microsoft Azure Notification Hubs .NET SDK**: In hello Nuget Package Manager for Visual Studio, run [Install-Package Microsoft.Azure.NotificationHubs](https://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/).</span></span>
* <span data-ttu-id="294af-170">**Node.js** : [jak toouse Notification Hubs z Node.js](notification-hubs-nodejs-push-notification-tutorial.md).</span><span class="sxs-lookup"><span data-stu-id="294af-170">**Node.js** : [How toouse Notification Hubs from Node.js](notification-hubs-nodejs-push-notification-tutorial.md).</span></span>
* <span data-ttu-id="294af-171">**Azure Mobile Apps**: pro příklad toosend oznámení z mobilní aplikace Azure, které jsou integrovány v centrech oznámení najdete v části [přidat nabízená oznámení pro Mobile Apps](../app-service-mobile/app-service-mobile-windows-store-dotnet-get-started-push.md).</span><span class="sxs-lookup"><span data-stu-id="294af-171">**Azure Mobile Apps**: For an example of how toosend notifications from an Azure Mobile App that's integrated with Notification Hubs, see [Add push notifications for Mobile Apps](../app-service-mobile/app-service-mobile-windows-store-dotnet-get-started-push.md).</span></span>
* <span data-ttu-id="294af-172">**Java / PHP**: Příklad jak hello toosend oznámení pomocí rozhraní REST API, naleznete v části "jak toouse centra oznámení z Java/PHP" ([Java](notification-hubs-java-push-notification-tutorial.md) | [PHP](notification-hubs-php-push-notification-tutorial.md)).</span><span class="sxs-lookup"><span data-stu-id="294af-172">**Java / PHP**: For an example of how toosend notifications by using hello REST APIs, see "How toouse Notification Hubs from Java/PHP" ([Java](notification-hubs-java-push-notification-tutorial.md) | [PHP](notification-hubs-php-push-notification-tutorial.md)).</span></span>

## <a name="optional-send-notifications-from-a-console-app"></a><span data-ttu-id="294af-173">(Volitelné) Odesílání oznámení z konzoly aplikace</span><span class="sxs-lookup"><span data-stu-id="294af-173">(Optional) Send notifications from a console app</span></span>
<span data-ttu-id="294af-174">toosend oznámení pomocí konzolové aplikace .NET postupujte podle těchto kroků.</span><span class="sxs-lookup"><span data-stu-id="294af-174">toosend notifications by using a .NET console application follow these steps.</span></span> 

1. <span data-ttu-id="294af-175">Hello řešení klikněte pravým tlačítkem, vyberte **přidat** a **nový projekt...** a potom v části **Visual C#**, klikněte na tlačítko **Windows** a **konzolové aplikace**a klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="294af-175">Right-click hello solution, select **Add** and **New Project...**, and then under **Visual C#**, click **Windows** and **Console Application**, and click **OK**.</span></span>
   
    <span data-ttu-id="294af-176">Tento postup přidá nové Visual C# konzole aplikace toohello řešení.</span><span class="sxs-lookup"><span data-stu-id="294af-176">This adds a new Visual C# console application toohello solution.</span></span> <span data-ttu-id="294af-177">Tento postup také můžete využít v samostatném řešení.</span><span class="sxs-lookup"><span data-stu-id="294af-177">You can also do this in a separate solution.</span></span>

2. <span data-ttu-id="294af-178">Ve Visual Studiu klikněte na položku **Nástroje**, klikněte na **Správce balíčků NuGet** a pak klikněte na **Konzola Správce balíčků**.</span><span class="sxs-lookup"><span data-stu-id="294af-178">In Visual Studio, click **Tools**, click **NuGet Package Manager**, and then click **Package Manager Console**.</span></span>
   
    <span data-ttu-id="294af-179">V sadě Visual Studio se zobrazí hello Konzola správce balíčků.</span><span class="sxs-lookup"><span data-stu-id="294af-179">This displays hello Package Manager Console in Visual Studio.</span></span>
3. <span data-ttu-id="294af-180">V okně konzoly Správce balíčků hello, nastavte hello **výchozí projekt** tooyour nové konzoly projekt aplikace a potom v okně konzoly hello, spusťte následující příkaz hello:</span><span class="sxs-lookup"><span data-stu-id="294af-180">In hello Package Manager Console window, set hello **Default project** tooyour new console application project, and then in hello console window, execute hello following command:</span></span>
   
        Install-Package Microsoft.Azure.NotificationHubs
   
    <span data-ttu-id="294af-181">Tento postup přidá odkaz toohello SDK centra oznámení Azure pomocí hello <a href="http://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/">balíčku Microsoft.Azure.Notification Hubs NuGet</a>.</span><span class="sxs-lookup"><span data-stu-id="294af-181">This adds a reference toohello Azure Notification Hubs SDK using hello <a href="http://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/">Microsoft.Azure.Notification Hubs NuGet package</a>.</span></span>
   
    ![](./media/notification-hubs-windows-store-dotnet-get-started/notification-hub-package-manager.png)
4. <span data-ttu-id="294af-182">Otevřete soubor Program.cs hello a přidejte následující hello `using` příkaz:</span><span class="sxs-lookup"><span data-stu-id="294af-182">Open hello Program.cs file and add hello following `using` statement:</span></span>
   
        using Microsoft.Azure.NotificationHubs;
5. <span data-ttu-id="294af-183">V hello **Program** třídy, přidejte následující metodu hello:</span><span class="sxs-lookup"><span data-stu-id="294af-183">In hello **Program** class, add hello following method:</span></span>
   
        private static async void SendNotificationAsync()
        {
            NotificationHubClient hub = NotificationHubClient
                .CreateClientFromConnectionString("<connection string with full access>", "<hub name>");
            var toast = @"<toast><visual><binding template=""ToastText01""><text id=""1"">Hello from a .NET App!</text></binding></visual></toast>";
            await hub.SendWindowsNativeNotificationAsync(toast);
        }
   
       Make sure tooreplace hello "hub name" placeholder with hello name of hello notification hub that as it appears in hello Azure Portal. Also, replace hello connection string placeholder with hello **DefaultFullSharedAccessSignature** connection string that you obtained from hello **Access Policies** page of your Notification Hub in hello section called "Configure your notification hub."
   
   > [!NOTE]
   > <span data-ttu-id="294af-184">Ujistěte se, že používáte hello připojovací řetězec, který má **úplné** přístupem, nikoli **naslouchání** přístup.</span><span class="sxs-lookup"><span data-stu-id="294af-184">Make sure that you use hello connection string that has **Full** access, not **Listen** access.</span></span> <span data-ttu-id="294af-185">řetězec s přístupem k naslouchání Hello nemá oprávnění toosend oznámení.</span><span class="sxs-lookup"><span data-stu-id="294af-185">hello listen-access string does not have permissions toosend notifications.</span></span>
   > 
   > 
6. <span data-ttu-id="294af-186">Přidejte následující řádky do hello hello **hlavní** metoda:</span><span class="sxs-lookup"><span data-stu-id="294af-186">Add hello following lines in hello **Main** method:</span></span>
   
         SendNotificationAsync();
         Console.ReadLine();
7. <span data-ttu-id="294af-187">Klikněte pravým tlačítkem na projekt konzolové aplikace hello v sadě Visual Studio a klikněte na tlačítko **nastavit jako spouštěný projekt** tooset ho jako projekt po spuštění hello.</span><span class="sxs-lookup"><span data-stu-id="294af-187">Right-click hello console application project in Visual Studio, and click **Set as StartUp Project** tooset it as hello startup project.</span></span> <span data-ttu-id="294af-188">Stiskněte klávesu hello **F5** klíče toorun hello aplikace.</span><span class="sxs-lookup"><span data-stu-id="294af-188">Then press hello **F5** key toorun hello application.</span></span>
   
    <span data-ttu-id="294af-189">Na všech registrovaných zařízeních se zobrazí informační zprávy.</span><span class="sxs-lookup"><span data-stu-id="294af-189">You will receive a toast notification on all registered devices.</span></span> <span data-ttu-id="294af-190">Kliknutí nebo klepnutí hello informační nápis načte aplikaci hello.</span><span class="sxs-lookup"><span data-stu-id="294af-190">Clicking or tapping hello toast banner loads hello app.</span></span>

<span data-ttu-id="294af-191">Můžete najít všechny hello podporované datové části v hello [katalog informační zprávy], [katalog dlaždic], a [přehled odznaků] témata na webu MSDN.</span><span class="sxs-lookup"><span data-stu-id="294af-191">You can find all hello supported payloads in hello [toast catalog], [tile catalog], and [badge overview] topics on MSDN.</span></span>

## <a name="next-steps"></a><span data-ttu-id="294af-192">Další kroky</span><span class="sxs-lookup"><span data-stu-id="294af-192">Next steps</span></span>
<span data-ttu-id="294af-193">V tomto jednoduchém příkladu jste zaslali oznámení vysílání tooall vaše zařízení s Windows pomocí portálu hello nebo konzolovou aplikaci.</span><span class="sxs-lookup"><span data-stu-id="294af-193">In this simple example, you sent broadcast notifications tooall your Windows devices using hello portal or a console app.</span></span> <span data-ttu-id="294af-194">Doporučujeme, abyste hello [použití centra oznámení toopush oznámení toousers] kurz jako další krok hello.</span><span class="sxs-lookup"><span data-stu-id="294af-194">We recommend hello [Use Notification Hubs toopush notifications toousers] tutorial as hello next step.</span></span> <span data-ttu-id="294af-195">Zobrazí se jak toosend oznámení z back-end ASP.NET pomocí značky tootarget konkrétním uživatelům.</span><span class="sxs-lookup"><span data-stu-id="294af-195">It will show you how toosend notifications from an ASP.NET backend using tags tootarget specific users.</span></span>

<span data-ttu-id="294af-196">Pokud chcete toosegment uživatele podle zájmových skupin, najdete v části [toosend použití centra oznámení nejnovější zprávy přes].</span><span class="sxs-lookup"><span data-stu-id="294af-196">If you want toosegment your users by interest groups, see [Use Notification Hubs toosend breaking news].</span></span> 

<span data-ttu-id="294af-197">toolearn další obecné informace o centrech oznámení naleznete v [pokyny centra oznámení](notification-hubs-push-notification-overview.md).</span><span class="sxs-lookup"><span data-stu-id="294af-197">toolearn more general information about Notification Hubs, see [Notification Hubs Guidance](notification-hubs-push-notification-overview.md).</span></span>

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
