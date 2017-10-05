---
title: "Začínáme používat službu Azure Notification Hubs pro aplikace Univerzální platformy Windows | Dokumentace Microsoftu"
description: "V tomto kurzu zjistíte, jak pomocí služby Azure Notification Hubs odesílat nabízená oznámení do aplikace Univerzální platformy Windows."
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
ms.openlocfilehash: 9b50f1cca81348b69f7ff2d702c6c72871afe0a0
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="getting-started-with-notification-hubs-for-windows-universal-platform-apps"></a><span data-ttu-id="4b91c-103">Začínáme používat službu Notification Hubs pro aplikace Univerzální platformy Windows</span><span class="sxs-lookup"><span data-stu-id="4b91c-103">Getting started with Notification Hubs for Windows Universal Platform Apps</span></span>
[!INCLUDE [notification-hubs-selector-get-started](../../includes/notification-hubs-selector-get-started.md)]

## <a name="overview"></a><span data-ttu-id="4b91c-104">Přehled</span><span class="sxs-lookup"><span data-stu-id="4b91c-104">Overview</span></span>
<span data-ttu-id="4b91c-105">V tomto kurzu zjistíte, jak pomocí služby Azure Notification Hubs k odesílat nabízená oznámení do aplikace Univerzální platformy Windows (UPW).</span><span class="sxs-lookup"><span data-stu-id="4b91c-105">This tutorial shows you how to use Azure Notification Hubs to send push notifications to a Universal Windows Platform (UWP) app.</span></span>

<span data-ttu-id="4b91c-106">V tomto kurzu vytvoříte prázdnou aplikaci pro Windows Store, která obdrží nabízená oznámení pomocí služby nabízených oznámení Windows (WNS).</span><span class="sxs-lookup"><span data-stu-id="4b91c-106">In this tutorial, you create a blank Windows Store app that receives push notifications by using the Windows Push Notification Service (WNS).</span></span> <span data-ttu-id="4b91c-107">Jakmile budete hotovi, budete moci používat vaše centra oznámení k vysílání nabízených oznámení pro všechna zařízení používající vaši aplikaci.</span><span class="sxs-lookup"><span data-stu-id="4b91c-107">When you're finished, you'll be able to use your notification hub to broadcast push notifications to all the devices running your app.</span></span>

## <a name="before-you-begin"></a><span data-ttu-id="4b91c-108">Než začnete</span><span class="sxs-lookup"><span data-stu-id="4b91c-108">Before you begin</span></span>
[!INCLUDE [notification-hubs-hero-slug](../../includes/notification-hubs-hero-slug.md)]

<span data-ttu-id="4b91c-109">Dokončený kód v tomto kurzu lze najít v části Github [zde](https://github.com/Azure/azure-notificationhubs-samples/tree/master/dotnet/GetStartedWindowsUniversal).</span><span class="sxs-lookup"><span data-stu-id="4b91c-109">The completed code for this tutorial can be found on GitHub [here](https://github.com/Azure/azure-notificationhubs-samples/tree/master/dotnet/GetStartedWindowsUniversal).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="4b91c-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="4b91c-110">Prerequisites</span></span>
<span data-ttu-id="4b91c-111">V tomto kurzu budete potřebovat následující:</span><span class="sxs-lookup"><span data-stu-id="4b91c-111">This tutorial requires the following:</span></span>

* <span data-ttu-id="4b91c-112">[Microsoft Visual Studio Community 2015](https://www.visualstudio.com/products/visual-studio-community-vs) nebo novější</span><span class="sxs-lookup"><span data-stu-id="4b91c-112">[Microsoft Visual Studio Community 2015](https://www.visualstudio.com/products/visual-studio-community-vs) or later</span></span>
* [<span data-ttu-id="4b91c-113">Nainstalované nástroje pro vývoj univerzálních aplikací pro Windows</span><span class="sxs-lookup"><span data-stu-id="4b91c-113">Universal Windows App Development Tools installed</span></span>](https://msdn.microsoft.com/windows/uwp/get-started/get-set-up)
* <span data-ttu-id="4b91c-114">Aktivní účet Azure</span><span class="sxs-lookup"><span data-stu-id="4b91c-114">An active Azure account</span></span> <br/><span data-ttu-id="4b91c-115">Pokud účet nemáte, můžete si během několika minut vytvořit bezplatný zkušební účet.</span><span class="sxs-lookup"><span data-stu-id="4b91c-115">If you don't have an account, you can create a free trial account in just a couple of minutes.</span></span> <span data-ttu-id="4b91c-116">Podrobnosti najdete v článku [Bezplatná zkušební verze Azure](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A0E0E5C02&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdocumentation%2Farticles%2Fnotification-hubs-windows-store-dotnet-get-started%2F).</span><span class="sxs-lookup"><span data-stu-id="4b91c-116">For details, see [Azure Free Trial](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A0E0E5C02&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdocumentation%2Farticles%2Fnotification-hubs-windows-store-dotnet-get-started%2F).</span></span>
* <span data-ttu-id="4b91c-117">Aktivní účet Windows Store</span><span class="sxs-lookup"><span data-stu-id="4b91c-117">An active Windows Store account</span></span>

<span data-ttu-id="4b91c-118">Dokončení tohoto kurzu je předpokladem pro všechny ostatní kurzy služby Notification Hubs pro aplikace Univerzální platformy Windows.</span><span class="sxs-lookup"><span data-stu-id="4b91c-118">Completing this tutorial is a prerequisite for all other Notification Hubs tutorials for Windows Universal Platform apps.</span></span>

## <a name="register-your-app-for-the-windows-store"></a><span data-ttu-id="4b91c-119">Registrace aplikace pro Windows Store</span><span class="sxs-lookup"><span data-stu-id="4b91c-119">Register your app for the Windows Store</span></span>
<span data-ttu-id="4b91c-120">Chcete-li odesílat nabízená oznámení do aplikací UWP, musíte aplikaci přidružit k Windows Storu.</span><span class="sxs-lookup"><span data-stu-id="4b91c-120">To send push notifications to UWP apps, you must associate your app to the Windows Store.</span></span> <span data-ttu-id="4b91c-121">Pak musíte nakonfigurovat centrum oznámení pro integraci s WNS.</span><span class="sxs-lookup"><span data-stu-id="4b91c-121">You must then configure your notification hub to integrate with WNS.</span></span>

1. <span data-ttu-id="4b91c-122">Pokud jste ještě aplikaci nezaregistrovali, přejděte na [Windows Dev Center](https://dev.windows.com/overview), přihlaste se pomocí účtu Microsoft a pak klikněte na tlačítko **Vytvořit novou aplikaci**.</span><span class="sxs-lookup"><span data-stu-id="4b91c-122">If you have not already registered your app, navigate to the [Windows Dev Center](https://dev.windows.com/overview), sign in with your Microsoft account, and then click **Create a new app**.</span></span>

2. <span data-ttu-id="4b91c-123">Zadejte název aplikace a klikněte na tlačítko **Rezervovat název aplikace**.</span><span class="sxs-lookup"><span data-stu-id="4b91c-123">Type a name for your app and click **Reserve app name**.</span></span> <span data-ttu-id="4b91c-124">Tím se vytvoří nová registrace Windows Store pro vaši aplikaci.</span><span class="sxs-lookup"><span data-stu-id="4b91c-124">This creates a new Windows Store registration for your app.</span></span>

3. <span data-ttu-id="4b91c-125">V sadě Visual Studio vytvořte nový projekt aplikace Visual C# Store pomocí univerzální šablony **Prázdná aplikace** pro Windows a klikněte na **OK**.</span><span class="sxs-lookup"><span data-stu-id="4b91c-125">In Visual Studio, create a new Visual C# Store Apps project by using the Windows Universal **Blank App** template and click **OK**.</span></span>

4. <span data-ttu-id="4b91c-126">Přijměte výchozí hodnoty cíle a minimální verze platformy.</span><span class="sxs-lookup"><span data-stu-id="4b91c-126">Accept the defaults for the target and minimum platform versions.</span></span>

5. <span data-ttu-id="4b91c-127">V Průzkumníku řešení klikněte pravým tlačítkem na projekt aplikace Windows Store, klikněte na tlačítko **Úložiště** a pak klikněte na tlačítko **Přidružit aplikaci ve Store...**.</span><span class="sxs-lookup"><span data-stu-id="4b91c-127">In Solution Explorer, right-click the Windows Store app project, click **Store**, and then click **Associate App with the Store...**.</span></span> <span data-ttu-id="4b91c-128">Zobrazí se průvodce **Přidružením aplikace k Windows Store**.</span><span class="sxs-lookup"><span data-stu-id="4b91c-128">The **Associate Your App with the Windows Store** wizard appears.</span></span>

6. <span data-ttu-id="4b91c-129">V průvodci se přihlaste pomocí svého účtu Microsoft.</span><span class="sxs-lookup"><span data-stu-id="4b91c-129">In the wizard, sign in with your Microsoft account.</span></span>

7. <span data-ttu-id="4b91c-130">Klikněte na aplikaci zaregistrovanou v kroku 2, klikněte na tlačítko **Další** a pak klikněte na tlačítko **Přidružit**.</span><span class="sxs-lookup"><span data-stu-id="4b91c-130">Click the app that you registered in step 2, click **Next**, and then click **Associate**.</span></span> <span data-ttu-id="4b91c-131">Tento postup přidá požadované informace o registraci Windows Store do manifestu aplikace.</span><span class="sxs-lookup"><span data-stu-id="4b91c-131">This adds the required Windows Store registration information to the application manifest.</span></span>

8. <span data-ttu-id="4b91c-132">Zpátky na stránce [Windows Dev Center](http://dev.windows.com/overview) pro novou aplikaci klikněte postupně na **Služby**, **Nabízená oznámení** a potom na **WNS/MPNS**.</span><span class="sxs-lookup"><span data-stu-id="4b91c-132">Back on the [Windows Dev Center](http://dev.windows.com/overview) page for your new app, click **Services**, click **Push notifications**, and then click **WNS/MPNS**.</span></span>

9. <span data-ttu-id="4b91c-133">Klikněte na **Nové oznámení**.</span><span class="sxs-lookup"><span data-stu-id="4b91c-133">Click **New Notification**.</span></span>

10. <span data-ttu-id="4b91c-134">Klikněte na šablonu **Prázdná (informační zpráva)** a potom klikněte na **OK**.</span><span class="sxs-lookup"><span data-stu-id="4b91c-134">Click **Blank (Toast)** template and then click **OK**.</span></span>

11. <span data-ttu-id="4b91c-135">Zadejte **Název** oznámení a vizuální **kontextovou** zprávu.</span><span class="sxs-lookup"><span data-stu-id="4b91c-135">Enter a notification **Name** and Visual **Context** message.</span></span> <span data-ttu-id="4b91c-136">Potom klikněte na **Uložit jako koncept**.</span><span class="sxs-lookup"><span data-stu-id="4b91c-136">Then click **Save as draft**.</span></span>

12. <span data-ttu-id="4b91c-137">Přejděte na [Portál pro registraci aplikací](http://apps.dev.microsoft.com) a přihlaste se.</span><span class="sxs-lookup"><span data-stu-id="4b91c-137">Navigate to the [Application Registration Portal](http://apps.dev.microsoft.com) and log in.</span></span>

13. <span data-ttu-id="4b91c-138">Klikněte na název vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="4b91c-138">Click on your application name.</span></span> <span data-ttu-id="4b91c-139">Poznamenejte si heslo **Tajný klíč aplikace** a **Identifikátor zabezpečení (SID) balíčku**, které najdete v nastavení platformy **Windows Store**.</span><span class="sxs-lookup"><span data-stu-id="4b91c-139">Make a note of the **Application Secret** password and the **Package security identifier (SID)** located in the **Windows Store** platform settings.</span></span>

     > [AZURE.WARNING]
    <span data-ttu-id="4b91c-140">Tajný klíč aplikace a SID balíčku jsou důležité přihlašovací údaje zabezpečení.</span><span class="sxs-lookup"><span data-stu-id="4b91c-140">The application secret and package SID are important security credentials.</span></span> <span data-ttu-id="4b91c-141">Tyto hodnoty s nikým nesdílejte ani je nedistribuujte s vaší aplikací.</span><span class="sxs-lookup"><span data-stu-id="4b91c-141">Do not share these values with anyone or distribute them with your app.</span></span>

## <a name="configure-your-notification-hub"></a><span data-ttu-id="4b91c-142">Konfigurace centra oznámení</span><span class="sxs-lookup"><span data-stu-id="4b91c-142">Configure your notification hub</span></span>
[!INCLUDE [notification-hubs-portal-create-new-hub](../../includes/notification-hubs-portal-create-new-hub.md)]

<ol start="6">
<li><p><span data-ttu-id="4b91c-143">Vyberte možnosti <b>Notification Services</b> a <b>Služba nabízených oznámení Windows (WNS)</b>.</span><span class="sxs-lookup"><span data-stu-id="4b91c-143">Select the <b>Notification Services</b> option and the <b>Windows (WNS)</b> option.</span></span> <span data-ttu-id="4b91c-144">Potom zadejte <b>Tajný klíč aplikace</b> do pole <b>Klíč zabezpečení</b>.</span><span class="sxs-lookup"><span data-stu-id="4b91c-144">Then enter the <b>Application secret</b> password in the <b>Security Key</b> field.</span></span> <span data-ttu-id="4b91c-145">Zadejte hodnotu <b>Identifikátor SID balíčku</b>, kterou jste ve službě WNS získali v předchozí části, a klikněte na <b>Uložit</b>.</span><span class="sxs-lookup"><span data-stu-id="4b91c-145">Enter your <b>Package SID</b> value that you obtained from WNS in the previous section, and then click <b>Save</b>.</span></span></p>
</li>
</ol>

&emsp;&emsp;![](./media/notification-hubs-windows-store-dotnet-get-started/notification-hub-configure-wns.png)

<span data-ttu-id="4b91c-146">Vaše centrum oznámení je nyní nakonfigurováno pro práci se službou WNS. Zároveň máte připojovací řetězce, pomocí kterých můžete svou aplikaci zaregistrovat pro odesílání oznámení.</span><span class="sxs-lookup"><span data-stu-id="4b91c-146">Your notification hub is now configured to work with WNS, and you have the connection strings to register your app and send notifications.</span></span>

## <a name="connect-your-app-to-the-notification-hub"></a><span data-ttu-id="4b91c-147">Připojte aplikaci k centru oznámení</span><span class="sxs-lookup"><span data-stu-id="4b91c-147">Connect your app to the notification hub</span></span>
1. <span data-ttu-id="4b91c-148">Ve Visual Studiu klikněte pravým tlačítkem na řešení a potom na **Spravovat balíčky NuGet**.</span><span class="sxs-lookup"><span data-stu-id="4b91c-148">In Visual Studio, right-click the solution, and then click **Manage NuGet Packages**.</span></span>
   
    <span data-ttu-id="4b91c-149">Zobrazí se dialogové okno **Správa balíčků NuGet**.</span><span class="sxs-lookup"><span data-stu-id="4b91c-149">This displays the **Manage NuGet Packages** dialog box.</span></span>
2. <span data-ttu-id="4b91c-150">Vyhledejte `WindowsAzure.Messaging.Managed`, klikněte na **Instalovat** a potom přijměte podmínky použití.</span><span class="sxs-lookup"><span data-stu-id="4b91c-150">Search for `WindowsAzure.Messaging.Managed` and click **Install**, and accept the terms of use.</span></span>
   
    ![][20]
   
    <span data-ttu-id="4b91c-151">Tento správce stáhne, nainstaluje a přidá odkaz na knihovnu zasílání zpráv Azure pro Windows pomocí <a href="http://nuget.org/packages/WindowsAzure.Messaging.Managed/">balíčku NuGet WindowsAzure.Messaging.Managed</a>.</span><span class="sxs-lookup"><span data-stu-id="4b91c-151">This downloads, installs, and adds a reference to the Azure Messaging library for Windows by using the <a href="http://nuget.org/packages/WindowsAzure.Messaging.Managed/">WindowsAzure.Messaging.Managed NuGet package</a>.</span></span>
3. <span data-ttu-id="4b91c-152">Otevřete soubor projektu App.xaml.cs a přidejte následující příkazy `using`:</span><span class="sxs-lookup"><span data-stu-id="4b91c-152">Open the App.xaml.cs project file and add the following `using` statements.</span></span> 
   
        using Windows.Networking.PushNotifications;
        using Microsoft.WindowsAzure.Messaging;
        using Windows.UI.Popups;
4. <span data-ttu-id="4b91c-153">Také do souboru App.xaml.cs přidejte následující definici metody **InitNotificationsAsync** do třídy **Aplikace**:</span><span class="sxs-lookup"><span data-stu-id="4b91c-153">Also in App.xaml.cs, add the following **InitNotificationsAsync** method definition to the **App** class:</span></span>
   
        private async void InitNotificationsAsync()
        {
            var channel = await PushNotificationChannelManager.CreatePushNotificationChannelForApplicationAsync();
   
            var hub = new NotificationHub("< your hub name>", "<Your DefaultListenSharedAccessSignature connection string>");
            var result = await hub.RegisterNativeAsync(channel.Uri);
   
            // Displays the registration ID so you know it was successful
            if (result.RegistrationId != null)
            {
                var dialog = new MessageDialog("Registration successful: " + result.RegistrationId);
                dialog.Commands.Add(new UICommand("OK"));
                await dialog.ShowAsync();
            }
   
        }
   
    <span data-ttu-id="4b91c-154">Tento kód načte identifikátor URI kanálu pro aplikaci z WNS a pak zaregistruje tento kanál URI pomocí centra oznámení.</span><span class="sxs-lookup"><span data-stu-id="4b91c-154">This code retrieves the channel URI for the app from WNS, and then registers that channel URI with your notification hub.</span></span>
   
   > [!NOTE]
   > <span data-ttu-id="4b91c-155">Zástupný text „název vašeho centra“ je potřeba nahradit názvem centra oznámení, který se zobrazí na webu Azure Portal.</span><span class="sxs-lookup"><span data-stu-id="4b91c-155">Make sure to replace the "your hub name" placeholder with the name of the notification hub that appears in the Azure Portal.</span></span> <span data-ttu-id="4b91c-156">Taky nahraďte zástupný symbol připojovacího řetězce připojovacím řetězcem **DefaultListenSharedAccessSignature**, který jste získali v předchozí části na stránce **Zásady přístupu** vašeho centra oznámení.</span><span class="sxs-lookup"><span data-stu-id="4b91c-156">Also replace the connection string placeholder with the **DefaultListenSharedAccessSignature** connection string that you obtained from the **Access Polices** page of your Notification Hub in a previous section.</span></span>
   > 
   > 
5. <span data-ttu-id="4b91c-157">V horní části **OnLaunched** obslužné rutiny událostí v souboru App.xaml.cs přidejte následující volání do nové metody **InitNotificationsAsync**:</span><span class="sxs-lookup"><span data-stu-id="4b91c-157">At the top of the **OnLaunched** event handler in App.xaml.cs, add the following call to the new **InitNotificationsAsync** method:</span></span>
   
        InitNotificationsAsync();
   
    <span data-ttu-id="4b91c-158">Tento postup zaručuje, že bude kanál URI registrován v centru oznámení při každém spuštění aplikace.</span><span class="sxs-lookup"><span data-stu-id="4b91c-158">This guarantees that the channel URI is registered in your notification hub each time the application is launched.</span></span>
6. <span data-ttu-id="4b91c-159">Stiskněte klávesu **F5** a spusťte aplikaci.</span><span class="sxs-lookup"><span data-stu-id="4b91c-159">Press the **F5** key to run the app.</span></span> <span data-ttu-id="4b91c-160">Zobrazí se automaticky otevíraný dialog, který obsahuje registrační klíč.</span><span class="sxs-lookup"><span data-stu-id="4b91c-160">A pop-up dialog that contains the registration key is displayed.</span></span>

<span data-ttu-id="4b91c-161">Vaše aplikace je teď připravena přijímat oznámení informačního nápisu.</span><span class="sxs-lookup"><span data-stu-id="4b91c-161">Your app is now ready to receive toast notifications.</span></span>

## <a name="send-notifications"></a><span data-ttu-id="4b91c-162">Odeslat oznámení</span><span class="sxs-lookup"><span data-stu-id="4b91c-162">Send notifications</span></span>
<span data-ttu-id="4b91c-163">Příjem oznámení ve vaší aplikaci můžete rychle otestovat zasláním oznámení z webu [Azure Portal](https://portal.azure.com/) prostřednictvím tlačítka **Poslat na zkoušku** v centru oznámení, jak vidíte na následující obrazovce.</span><span class="sxs-lookup"><span data-stu-id="4b91c-163">You can quickly test receiving notifications in your app by sending notifications in the [Azure Portal](https://portal.azure.com/) using the **Test Send** button on the notification hub, as shown in the screen below.</span></span>

![](./media/notification-hubs-windows-store-dotnet-get-started/notification-hub-test-send-wns.png)

<span data-ttu-id="4b91c-164">Nabízená oznámení se většinou posílají ve službě back-end, jako je služba Mobile Services, nebo v technologii ASP.NET pomocí kompatibilní knihovny.</span><span class="sxs-lookup"><span data-stu-id="4b91c-164">Push notifications are normally sent in a back-end service like Mobile Services or ASP.NET using a compatible library.</span></span> <span data-ttu-id="4b91c-165">Můžete také použít rozhraní API REST k přímému zasílání oznámení, pokud pro vaše prostředí back-end není dostupná žádná knihovna.</span><span class="sxs-lookup"><span data-stu-id="4b91c-165">You can also use the REST API directly to send notification messages if a library is not available for your back-end.</span></span> 

<span data-ttu-id="4b91c-166">V tomto kurzu nebudeme dělat nic složitého a jednoduše předvedeme testování vaší klientské aplikace pomocí odesílání oznámení v sadě SDK .NET pro centra oznámení v konzolové aplikaci, místo back-end služby.</span><span class="sxs-lookup"><span data-stu-id="4b91c-166">In this tutorial, we will keep it simple and just demonstrate testing your client app by sending notifications using the .NET SDK for notification hubs in a console application instead of a backend service.</span></span> <span data-ttu-id="4b91c-167">Jako další krok pro odesílání oznámení z backendu ASP.NET doporučujeme absolvovat kurz [Použití Notification Hubs k odeslání nabízených oznámení uživatelům].</span><span class="sxs-lookup"><span data-stu-id="4b91c-167">We recommend the [Use Notification Hubs to push notifications to users] tutorial as the next step for sending notifications from an ASP.NET backend.</span></span> <span data-ttu-id="4b91c-168">Následující přístupy lze však použít pro zasílání oznámení:</span><span class="sxs-lookup"><span data-stu-id="4b91c-168">However, the following approaches can be used for sending notifications:</span></span>

* <span data-ttu-id="4b91c-169">**Rozhraní REST**: oznámení můžete podporovat na jakékoli backend platformě pomocí [rozhraní REST](http://msdn.microsoft.com/library/windowsazure/dn223264.aspx).</span><span class="sxs-lookup"><span data-stu-id="4b91c-169">**REST Interface**:  You can support notification on any backend platform using  the [REST interface](http://msdn.microsoft.com/library/windowsazure/dn223264.aspx).</span></span>
* <span data-ttu-id="4b91c-170">**Microsoft Azure oznámení centra .NET SDK**: Ve správci balíčků Nuget pro Visual Studio spusťte položku [Install-Package Microsoft.Azure.NotificationHubs](https://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/).</span><span class="sxs-lookup"><span data-stu-id="4b91c-170">**Microsoft Azure Notification Hubs .NET SDK**: In the Nuget Package Manager for Visual Studio, run [Install-Package Microsoft.Azure.NotificationHubs](https://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/).</span></span>
* <span data-ttu-id="4b91c-171">**Node.js**: [Jak používat Notification Hubs z Node.js](notification-hubs-nodejs-push-notification-tutorial.md).</span><span class="sxs-lookup"><span data-stu-id="4b91c-171">**Node.js** : [How to use Notification Hubs from Node.js](notification-hubs-nodejs-push-notification-tutorial.md).</span></span>
* <span data-ttu-id="4b91c-172">**Azure Mobile Apps**: Příklad zasílání oznámení ze služby Azure Mobile Apps integrované se službou Notification Hubs najdete v tématu [Přidání nabízených oznámení do mobilních aplikací](../app-service-mobile/app-service-mobile-windows-store-dotnet-get-started-push.md).</span><span class="sxs-lookup"><span data-stu-id="4b91c-172">**Azure Mobile Apps**: For an example of how to send notifications from an Azure Mobile App that's integrated with Notification Hubs, see [Add push notifications for Mobile Apps](../app-service-mobile/app-service-mobile-windows-store-dotnet-get-started-push.md).</span></span>
* <span data-ttu-id="4b91c-173">**Java / PHP**: Příklad odesílání oznámení pomocí rozhraní API REST najdete v části „Použití centra oznámení z Java/PHP“ ([Java](notification-hubs-java-push-notification-tutorial.md) | [PHP](notification-hubs-php-push-notification-tutorial.md)).</span><span class="sxs-lookup"><span data-stu-id="4b91c-173">**Java / PHP**: For an example of how to send notifications by using the REST APIs, see "How to use Notification Hubs from Java/PHP" ([Java](notification-hubs-java-push-notification-tutorial.md) | [PHP](notification-hubs-php-push-notification-tutorial.md)).</span></span>

## <a name="optional-send-notifications-from-a-console-app"></a><span data-ttu-id="4b91c-174">(Volitelné) Odesílání oznámení z konzoly aplikace</span><span class="sxs-lookup"><span data-stu-id="4b91c-174">(Optional) Send notifications from a console app</span></span>
<span data-ttu-id="4b91c-175">K odeslání oznámení pomocí konzolové aplikace .NET postupujte následovně.</span><span class="sxs-lookup"><span data-stu-id="4b91c-175">To send notifications by using a .NET console application follow these steps.</span></span> 

1. <span data-ttu-id="4b91c-176">Klikněte pravým tlačítkem myši na řešení, vyberte možnost **Přidat** a **Nový projekt...** a pak v části **Visual C#** klikněte na tlačítko **Windows** a **Konzolové aplikace** a klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="4b91c-176">Right-click the solution, select **Add** and **New Project...**, and then under **Visual C#**, click **Windows** and **Console Application**, and click **OK**.</span></span>
   
    <span data-ttu-id="4b91c-177">Tento postup přidá novou aplikaci Visual C# do řešení.</span><span class="sxs-lookup"><span data-stu-id="4b91c-177">This adds a new Visual C# console application to the solution.</span></span> <span data-ttu-id="4b91c-178">Tento postup také můžete využít v samostatném řešení.</span><span class="sxs-lookup"><span data-stu-id="4b91c-178">You can also do this in a separate solution.</span></span>

2. <span data-ttu-id="4b91c-179">Ve Visual Studiu klikněte na položku **Nástroje**, klikněte na **Správce balíčků NuGet** a pak klikněte na **Konzola Správce balíčků**.</span><span class="sxs-lookup"><span data-stu-id="4b91c-179">In Visual Studio, click **Tools**, click **NuGet Package Manager**, and then click **Package Manager Console**.</span></span>
   
    <span data-ttu-id="4b91c-180">Tím se zobrazí Konzola Správce balíčků ve Visual Studiu.</span><span class="sxs-lookup"><span data-stu-id="4b91c-180">This displays the Package Manager Console in Visual Studio.</span></span>
3. <span data-ttu-id="4b91c-181">V okně konzoly Správce balíčků nastavte **Výchozí projekt** na nový projekt konzolové aplikace a pak v okně konzoly spusťte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="4b91c-181">In the Package Manager Console window, set the **Default project** to your new console application project, and then in the console window, execute the following command:</span></span>
   
        Install-Package Microsoft.Azure.NotificationHubs
   
    <span data-ttu-id="4b91c-182">Ten přidá odkaz na sadu SDK centra oznámení Azure pomocí <a href="http://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/">balíčku Microsoft.Azure.Notification Hubs NuGet</a>.</span><span class="sxs-lookup"><span data-stu-id="4b91c-182">This adds a reference to the Azure Notification Hubs SDK using the <a href="http://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/">Microsoft.Azure.Notification Hubs NuGet package</a>.</span></span>
   
    ![](./media/notification-hubs-windows-store-dotnet-get-started/notification-hub-package-manager.png)
4. <span data-ttu-id="4b91c-183">Otevřete soubor Program.cs a přidejte následující příkaz `using`:</span><span class="sxs-lookup"><span data-stu-id="4b91c-183">Open the Program.cs file and add the following `using` statement:</span></span>
   
        using Microsoft.Azure.NotificationHubs;
5. <span data-ttu-id="4b91c-184">Přidejte následující metodu do třídy **Program**:</span><span class="sxs-lookup"><span data-stu-id="4b91c-184">In the **Program** class, add the following method:</span></span>
   
        private static async void SendNotificationAsync()
        {
            NotificationHubClient hub = NotificationHubClient
                .CreateClientFromConnectionString("<connection string with full access>", "<hub name>");
            var toast = @"<toast><visual><binding template=""ToastText01""><text id=""1"">Hello from a .NET App!</text></binding></visual></toast>";
            await hub.SendWindowsNativeNotificationAsync(toast);
        }
   
       Make sure to replace the "hub name" placeholder with the name of the notification hub that as it appears in the Azure Portal. Also, replace the connection string placeholder with the **DefaultFullSharedAccessSignature** connection string that you obtained from the **Access Policies** page of your Notification Hub in the section called "Configure your notification hub."
   
   > [!NOTE]
   > <span data-ttu-id="4b91c-185">Ujistěte se, že používáte připojovací řetězec s **Úplným** přístupem, nikoli s přístupem **Naslouchat**.</span><span class="sxs-lookup"><span data-stu-id="4b91c-185">Make sure that you use the connection string that has **Full** access, not **Listen** access.</span></span> <span data-ttu-id="4b91c-186">Řetězec s přístupem k naslouchání neposkytuje oprávnění k zasílání oznámení.</span><span class="sxs-lookup"><span data-stu-id="4b91c-186">The listen-access string does not have permissions to send notifications.</span></span>
   > 
   > 
6. <span data-ttu-id="4b91c-187">Přidejte následující řádky do metody **Hlavní**:</span><span class="sxs-lookup"><span data-stu-id="4b91c-187">Add the following lines in the **Main** method:</span></span>
   
         SendNotificationAsync();
         Console.ReadLine();
7. <span data-ttu-id="4b91c-188">Klikněte pravým tlačítkem na projekt konzolové aplikace ve Visual Studiu a kliknutím na tlačítko **Nastavit jako spouštěný projekt** nastavte projekt jako spouštěný.</span><span class="sxs-lookup"><span data-stu-id="4b91c-188">Right-click the console application project in Visual Studio, and click **Set as StartUp Project** to set it as the startup project.</span></span> <span data-ttu-id="4b91c-189">Pak stiskněte klávesu **F5** a spusťte aplikaci.</span><span class="sxs-lookup"><span data-stu-id="4b91c-189">Then press the **F5** key to run the application.</span></span>
   
    <span data-ttu-id="4b91c-190">Na všech registrovaných zařízeních se zobrazí informační zprávy.</span><span class="sxs-lookup"><span data-stu-id="4b91c-190">You will receive a toast notification on all registered devices.</span></span> <span data-ttu-id="4b91c-191">Kliknutí nebo klepnutí na informační nápis načte aplikaci.</span><span class="sxs-lookup"><span data-stu-id="4b91c-191">Clicking or tapping the toast banner loads the app.</span></span>

<span data-ttu-id="4b91c-192">Všechny podporované datové části v tématech [katalog informační zprávy], [katalog dlaždic] a [přehled odznaků] naleznete na webu MSDN.</span><span class="sxs-lookup"><span data-stu-id="4b91c-192">You can find all the supported payloads in the [toast catalog], [tile catalog], and [badge overview] topics on MSDN.</span></span>

## <a name="next-steps"></a><span data-ttu-id="4b91c-193">Další kroky</span><span class="sxs-lookup"><span data-stu-id="4b91c-193">Next steps</span></span>
<span data-ttu-id="4b91c-194">V tomto jednoduchém příkladu jste zaslali oznámení vysílání pro všechna vaše zařízení systému Windows pomocí portálu nebo aplikace konzoly.</span><span class="sxs-lookup"><span data-stu-id="4b91c-194">In this simple example, you sent broadcast notifications to all your Windows devices using the portal or a console app.</span></span> <span data-ttu-id="4b91c-195">Jako další krok doporučujeme tutoriál [Použití Notification Hubs k odeslání nabízených oznámení uživatelům].</span><span class="sxs-lookup"><span data-stu-id="4b91c-195">We recommend the [Use Notification Hubs to push notifications to users] tutorial as the next step.</span></span> <span data-ttu-id="4b91c-196">Zobrazí se postup odesílání oznámení z ASP.NET back-end pomocí značek pro cílové konkrétní uživatele.</span><span class="sxs-lookup"><span data-stu-id="4b91c-196">It will show you how to send notifications from an ASP.NET backend using tags to target specific users.</span></span>

<span data-ttu-id="4b91c-197">Pokud chcete segmentovat uživatele podle zájmových skupin, přečtěte si kurz [Používání centra oznámení k odesílání novinek].</span><span class="sxs-lookup"><span data-stu-id="4b91c-197">If you want to segment your users by interest groups, see [Use Notification Hubs to send breaking news].</span></span> 

<span data-ttu-id="4b91c-198">Další obecné informace o službě Notification Hubs najdete v tématu [Průvodce centry oznámení](notification-hubs-push-notification-overview.md).</span><span class="sxs-lookup"><span data-stu-id="4b91c-198">To learn more general information about Notification Hubs, see [Notification Hubs Guidance](notification-hubs-push-notification-overview.md).</span></span>

<!-- Images. -->
[13]: ./media/notification-hubs-windows-store-dotnet-get-started/notification-hub-create-console-app.png
[14]: ./media/notification-hubs-windows-store-dotnet-get-started/notification-hub-windows-toast.png
[19]: ./media/notification-hubs-windows-store-dotnet-get-started/notification-hub-windows-reg.png
[20]: ./media/notification-hubs-windows-store-dotnet-get-started/notification-hub-windows-universal-app-install-package.png

<!-- URLs. -->

<span data-ttu-id="4b91c-199">[Použití Notification Hubs k odeslání nabízených oznámení uživatelům]: notification-hubs-aspnet-backend-windows-dotnet-wns-notification.md</span><span class="sxs-lookup"><span data-stu-id="4b91c-199">[Use Notification Hubs to push notifications to users]: notification-hubs-aspnet-backend-windows-dotnet-wns-notification.md</span></span>
<span data-ttu-id="4b91c-200">[Používání centra oznámení k odesílání novinek]: notification-hubs-windows-notification-dotnet-push-xplat-segmented-wns.md</span><span class="sxs-lookup"><span data-stu-id="4b91c-200">[Use Notification Hubs to send breaking news]: notification-hubs-windows-notification-dotnet-push-xplat-segmented-wns.md</span></span>

<span data-ttu-id="4b91c-201">[katalog informační zprávy]: http://msdn.microsoft.com/library/windows/apps/hh761494.aspx</span><span class="sxs-lookup"><span data-stu-id="4b91c-201">[toast catalog]: http://msdn.microsoft.com/library/windows/apps/hh761494.aspx</span></span>
<span data-ttu-id="4b91c-202">[katalog dlaždic]: http://msdn.microsoft.com/library/windows/apps/hh761491.aspx</span><span class="sxs-lookup"><span data-stu-id="4b91c-202">[tile catalog]: http://msdn.microsoft.com/library/windows/apps/hh761491.aspx</span></span>
<span data-ttu-id="4b91c-203">[přehled odznaků]: http://msdn.microsoft.com/library/windows/apps/hh779719.aspx</span><span class="sxs-lookup"><span data-stu-id="4b91c-203">[badge overview]: http://msdn.microsoft.com/library/windows/apps/hh779719.aspx</span></span>
