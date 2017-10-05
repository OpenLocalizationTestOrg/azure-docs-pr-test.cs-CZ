---
title: "Odesílání nabízených oznámení pomocí Azure Notification Hubs a Node.js"
description: "Naučte se používat Notification Hubs k odesílání nabízených oznámení z aplikace Node.js."
keywords: "nabízená oznámení, nabízené notifications,node.js nabízet, ios push"
services: notification-hubs
documentationcenter: nodejs
author: ysxu
manager: erikre
editor: 
ms.assetid: ded4749c-6c39-4ff8-b2cf-1927b3e92f93
ms.service: notification-hubs
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: javascript
ms.topic: article
ms.date: 10/25/2016
ms.author: yuaxu
ms.openlocfilehash: dc4987b16b2e930641c6c90eff8b65c1bf8d573c
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="sending-push-notifications-with-azure-notification-hubs-and-nodejs"></a><span data-ttu-id="716e2-104">Odesílání nabízených oznámení pomocí Azure Notification Hubs a Node.js</span><span class="sxs-lookup"><span data-stu-id="716e2-104">Sending push notifications with Azure Notification Hubs and Node.js</span></span>
[!INCLUDE [notification-hubs-backend-how-to-selector](../../includes/notification-hubs-backend-how-to-selector.md)]

## <a name="overview"></a><span data-ttu-id="716e2-105">Přehled</span><span class="sxs-lookup"><span data-stu-id="716e2-105">Overview</span></span>
> [!IMPORTANT]
> <span data-ttu-id="716e2-106">K dokončení tohoto kurzu potřebujete mít aktivní účet Azure.</span><span class="sxs-lookup"><span data-stu-id="716e2-106">To complete this tutorial, you must have an active Azure account.</span></span> <span data-ttu-id="716e2-107">Pokud účet nemáte, můžete si během několika minut vytvořit bezplatný zkušební účet.</span><span class="sxs-lookup"><span data-stu-id="716e2-107">If you don't have an account, you can create a free trial account in just a couple of minutes.</span></span> <span data-ttu-id="716e2-108">Podrobnosti najdete v článku [Bezplatná zkušební verze Azure](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A643EE910&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdocumentation%2Farticles%2Fnotification-hubs-nodejs-how-to-use-notification-hubs).</span><span class="sxs-lookup"><span data-stu-id="716e2-108">For details, see [Azure Free Trial](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A643EE910&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdocumentation%2Farticles%2Fnotification-hubs-nodejs-how-to-use-notification-hubs).</span></span>
> 
> 

<span data-ttu-id="716e2-109">Tento průvodce vám ukáže, jak odesílat nabízená oznámení pomocí Azure Notification hubs přímo z aplikace Node.js.</span><span class="sxs-lookup"><span data-stu-id="716e2-109">This guide will show you how to send push notifications with the help of Azure Notification Hubs directly from a Node.js application.</span></span> 

<span data-ttu-id="716e2-110">Pokryté scénáře zahrnují odesílání nabízených oznámení do aplikace na následujících platformách:</span><span class="sxs-lookup"><span data-stu-id="716e2-110">The scenarios covered include sending push notifications to applications on the following platforms:</span></span>

* <span data-ttu-id="716e2-111">Android</span><span class="sxs-lookup"><span data-stu-id="716e2-111">Android</span></span>
* <span data-ttu-id="716e2-112">iOS</span><span class="sxs-lookup"><span data-stu-id="716e2-112">iOS</span></span>
* <span data-ttu-id="716e2-113">telefon se systémem Windows</span><span class="sxs-lookup"><span data-stu-id="716e2-113">Windows Phone</span></span>
* <span data-ttu-id="716e2-114">Univerzální platforma pro Windows</span><span class="sxs-lookup"><span data-stu-id="716e2-114">Universal Windows Platform</span></span> 

<span data-ttu-id="716e2-115">Další informace o notification hubs najdete v tématu [další kroky](#next) části.</span><span class="sxs-lookup"><span data-stu-id="716e2-115">For more information on notification hubs, see the [Next Steps](#next) section.</span></span>

## <a name="what-are-notification-hubs"></a><span data-ttu-id="716e2-116">Co jsou Notification Hubs?</span><span class="sxs-lookup"><span data-stu-id="716e2-116">What are Notification Hubs?</span></span>
<span data-ttu-id="716e2-117">Azure Notification Hubs poskytuje snadno použitelnou, více platformami, škálovatelné infrastruktury pro odesílání nabízených oznámení do mobilních zařízení.</span><span class="sxs-lookup"><span data-stu-id="716e2-117">Azure Notification Hubs provide an easy-to-use, multi-platform, scalable infrastructure for sending push notifications to mobile devices.</span></span> <span data-ttu-id="716e2-118">Podrobnosti o infrastrukturu služeb, najdete v článku [Azure Notification Hubs](http://msdn.microsoft.com/library/windowsazure/jj927170.aspx) stránky.</span><span class="sxs-lookup"><span data-stu-id="716e2-118">For details on the service infrastructure, see the [Azure Notification Hubs](http://msdn.microsoft.com/library/windowsazure/jj927170.aspx) page.</span></span>

## <a name="create-a-nodejs-application"></a><span data-ttu-id="716e2-119">Vytvoření aplikace Node.js</span><span class="sxs-lookup"><span data-stu-id="716e2-119">Create a Node.js Application</span></span>
<span data-ttu-id="716e2-120">Prvním krokem v tomto kurzu je vytvoření nové prázdné aplikace Node.js.</span><span class="sxs-lookup"><span data-stu-id="716e2-120">The first step in this tutorial is creating a new blank Node.js application.</span></span> <span data-ttu-id="716e2-121">Pokyny pro vytvoření aplikace Node.js, naleznete v části [vytvoření a nasazení aplikace Node.js na webu Azure][nodejswebsite], [Node.js Cloudová služba] [ Node.js Cloud Service] pomocí prostředí Windows PowerShell nebo [webu pomocí služby WebMatrix].</span><span class="sxs-lookup"><span data-stu-id="716e2-121">For instructions on creating a Node.js application, see [Create and deploy a Node.js application to Azure Web Site][nodejswebsite], [Node.js Cloud Service][Node.js Cloud Service] using Windows PowerShell, or [Web Site with WebMatrix].</span></span>

## <a name="configure-your-application-to-use-notification-hubs"></a><span data-ttu-id="716e2-122">Konfigurace aplikace pro použití služby Notification Hubs</span><span class="sxs-lookup"><span data-stu-id="716e2-122">Configure Your Application to Use Notification Hubs</span></span>
<span data-ttu-id="716e2-123">Chcete-li používat Azure Notification Hubs, musíte stáhnout a použít Node.js [balíček azure](https://www.npmjs.com/package/azure), což zahrnuje integrovanou sadu knihoven pomocné rutiny, které komunikují s REST služeb nabízených oznámení.</span><span class="sxs-lookup"><span data-stu-id="716e2-123">To use Azure Notification Hubs, you need to download and use the Node.js [azure package](https://www.npmjs.com/package/azure), which includes a built-in set of helper libraries that communicate with the push notification REST services.</span></span>

### <a name="use-node-package-manager-npm-to-obtain-the-package"></a><span data-ttu-id="716e2-124">Získat balíček pomocí uzlu balíček správce (NPM)</span><span class="sxs-lookup"><span data-stu-id="716e2-124">Use Node Package Manager (NPM) to obtain the package</span></span>
1. <span data-ttu-id="716e2-125">Pomocí rozhraní příkazového řádku, jako například **prostředí PowerShell** (Windows), **Terminálové** (Mac), nebo **Bash** (Linux) a přejděte do složky, které jste vytvořili aplikaci prázdné.</span><span class="sxs-lookup"><span data-stu-id="716e2-125">Use a command-line interface such as **PowerShell** (Windows), **Terminal** (Mac), or **Bash** (Linux) and navigate to the folder where you created your blank application.</span></span>
2. <span data-ttu-id="716e2-126">Typ **npm nainstalujte azure sb** v příkazovém okně.</span><span class="sxs-lookup"><span data-stu-id="716e2-126">Type **npm install azure-sb** in the command window.</span></span>
3. <span data-ttu-id="716e2-127">Můžete ručně spustit **ls** nebo **dir** příkazu ověřte, že **uzlu\_moduly** složka byla vytvořena.</span><span class="sxs-lookup"><span data-stu-id="716e2-127">You can manually run the **ls** or **dir** command to verify that a **node\_modules** folder was created.</span></span> <span data-ttu-id="716e2-128">V této složce najít **azure** balíček, který obsahuje knihovny, které potřebujete získat přístup k centru oznámení.</span><span class="sxs-lookup"><span data-stu-id="716e2-128">Inside that folder, find the **azure** package, which contains the libraries you need to access the Notification Hub.</span></span>

> [!NOTE]
> <span data-ttu-id="716e2-129">Další informace o instalaci NPM na oficiální [NPM blog](http://blog.npmjs.org/post/85484771375/how-to-install-npm).</span><span class="sxs-lookup"><span data-stu-id="716e2-129">You can learn more about installing NPM on the official [NPM blog](http://blog.npmjs.org/post/85484771375/how-to-install-npm).</span></span> 
> 
> 

### <a name="import-the-module"></a><span data-ttu-id="716e2-130">Naimportujte modul</span><span class="sxs-lookup"><span data-stu-id="716e2-130">Import the module</span></span>
<span data-ttu-id="716e2-131">Pomocí textového editoru, přidejte následující do horní části **server.js** souboru aplikace:</span><span class="sxs-lookup"><span data-stu-id="716e2-131">Using a text editor, add the following to the top of the **server.js** file of the application:</span></span>

    var azure = require('azure');

### <a name="setup-an-azure-notification-hub-connection"></a><span data-ttu-id="716e2-132">Nastavit připojení k Azure Notification Hubs</span><span class="sxs-lookup"><span data-stu-id="716e2-132">Setup an Azure Notification Hub connection</span></span>
<span data-ttu-id="716e2-133">**NotificationHubService** objekt vám umožňuje spolupracovat s centry oznámení.</span><span class="sxs-lookup"><span data-stu-id="716e2-133">The **NotificationHubService** object lets you work with notification hubs.</span></span> <span data-ttu-id="716e2-134">Následující kód vytvoří **NotificationHubService** objekt s názvem centra nofication **hubname**.</span><span class="sxs-lookup"><span data-stu-id="716e2-134">The following code creates a **NotificationHubService** object for the nofication hub named **hubname**.</span></span> <span data-ttu-id="716e2-135">Přidejte do horní části **server.js** souboru po příkazu k importu modulu azure:</span><span class="sxs-lookup"><span data-stu-id="716e2-135">Add it near the top of the **server.js** file, after the statement to import the azure module:</span></span>

    var notificationHubService = azure.createNotificationHubService('hubname','connectionstring');

<span data-ttu-id="716e2-136">Připojení **connectionstring** nelze získat hodnotu z [portálu Azure] provedením následujících kroků:</span><span class="sxs-lookup"><span data-stu-id="716e2-136">The connection **connectionstring** value can be obtained from the [Azure Portal] by performing the following steps:</span></span>

1. <span data-ttu-id="716e2-137">V levém navigačním podokně klikněte na tlačítko **Procházet**.</span><span class="sxs-lookup"><span data-stu-id="716e2-137">In the left navigation pane, click **Browse**.</span></span>
2. <span data-ttu-id="716e2-138">Vyberte **Notification Hubs**a pak vyhledejte rozbočovače, které chcete použít pro vzorovou.</span><span class="sxs-lookup"><span data-stu-id="716e2-138">Select **Notification Hubs**, and then find the hub you wish to use for the sample.</span></span> <span data-ttu-id="716e2-139">Můžete se podívat do [Windows Store Začínáme kurzu](notification-hubs-windows-store-dotnet-get-started-wns-push-notification.md) Pokud potřebujete pomoc, vytváření nového centra oznámení.</span><span class="sxs-lookup"><span data-stu-id="716e2-139">You can refer to the [Windows Store Getting Started tutorial](notification-hubs-windows-store-dotnet-get-started-wns-push-notification.md) if you need help creating a new Notification Hub.</span></span>
3. <span data-ttu-id="716e2-140">Vyberte **nastavení**.</span><span class="sxs-lookup"><span data-stu-id="716e2-140">Select **Settings**.</span></span>
4. <span data-ttu-id="716e2-141">Klikněte na **zásady přístupu**.</span><span class="sxs-lookup"><span data-stu-id="716e2-141">Click on **Access Policies**.</span></span> <span data-ttu-id="716e2-142">Zobrazí se oba připojovací řetězce sdílené a úplný přístup.</span><span class="sxs-lookup"><span data-stu-id="716e2-142">You will see both shared and full access connection strings.</span></span>

![Portál Azure – centra oznámení](./media/notification-hubs-nodejs-how-to-use-notification-hubs/notification-hubs-portal.png)

> [!NOTE]
> <span data-ttu-id="716e2-144">Můžete také načíst připojovací řetězec pomocí **Get-AzureSbNamespace** rutiny poskytované [prostředí Azure PowerShell](/powershell/azureps-cmdlets-docs) nebo **azure sb obor názvů zobrazit** s [Rozhraní příkazového řádku azure (Azure CLI)](../cli-install-nodejs.md).</span><span class="sxs-lookup"><span data-stu-id="716e2-144">You can also retrieve the connection string using the **Get-AzureSbNamespace** cmdlet provided by [Azure PowerShell](/powershell/azureps-cmdlets-docs) or the **azure sb namespace show** command with the [Azure Command-Line Interface (Azure CLI)](../cli-install-nodejs.md).</span></span>
> 
> 

## <a name="general-architecture"></a><span data-ttu-id="716e2-145">Obecná architektura</span><span class="sxs-lookup"><span data-stu-id="716e2-145">General architecture</span></span>
<span data-ttu-id="716e2-146">**NotificationHubService** objekt poskytuje následující instance objektů pro odesílání nabízených oznámení pro konkrétní zařízení a aplikace:</span><span class="sxs-lookup"><span data-stu-id="716e2-146">The **NotificationHubService** object exposes the following object instances for sending push notifications to specific devices and applications:</span></span>

* <span data-ttu-id="716e2-147">**Android** -použít **GcmService** objekt, který je k dispozici na **notificationHubService.gcm**</span><span class="sxs-lookup"><span data-stu-id="716e2-147">**Android** - use the **GcmService** object, which is available at **notificationHubService.gcm**</span></span>
* <span data-ttu-id="716e2-148">**iOS** -použít **ApnsService** objekt, který je přístupná na **notificationHubService.apns**</span><span class="sxs-lookup"><span data-stu-id="716e2-148">**iOS** - use the **ApnsService** object, which is accessible at **notificationHubService.apns**</span></span>
* <span data-ttu-id="716e2-149">**Windows Phone** -použít **MpnsService** objekt, který je k dispozici na **notificationHubService.mpns**</span><span class="sxs-lookup"><span data-stu-id="716e2-149">**Windows Phone** - use the **MpnsService** object, which is available at **notificationHubService.mpns**</span></span>
* <span data-ttu-id="716e2-150">**Univerzální platformu Windows** -použít **WnsService** objekt, který je k dispozici na **notificationHubService.wns**</span><span class="sxs-lookup"><span data-stu-id="716e2-150">**Universal Windows Platform** - use the **WnsService** object, which is available at **notificationHubService.wns**</span></span>

### <a name="how-to-send-push-notifications-to-android-applications"></a><span data-ttu-id="716e2-151">Postupy: odesílání nabízených oznámení do aplikace pro Android</span><span class="sxs-lookup"><span data-stu-id="716e2-151">How to: Send push notifications to Android applications</span></span>
<span data-ttu-id="716e2-152">**GcmService** objekt poskytuje **odeslat** metoda, která slouží k odesílání nabízených oznámení do aplikace pro Android.</span><span class="sxs-lookup"><span data-stu-id="716e2-152">The **GcmService** object provides a **send** method that can be used to send push notifications to Android applications.</span></span> <span data-ttu-id="716e2-153">**Odeslat** metoda přijímá následující parametry:</span><span class="sxs-lookup"><span data-stu-id="716e2-153">The **send** method accepts the following parameters:</span></span>

* <span data-ttu-id="716e2-154">**Značky** -identifikátor značky.</span><span class="sxs-lookup"><span data-stu-id="716e2-154">**Tags** - the tag identifier.</span></span> <span data-ttu-id="716e2-155">Pokud je k dispozici žádná značka, odešle se oznámení na všechny klienty.</span><span class="sxs-lookup"><span data-stu-id="716e2-155">If no tag is provided, the notification will be sent to all clients.</span></span>
* <span data-ttu-id="716e2-156">**Datová část** -JSON nebo nezpracovaný řetězec datovou část zprávy.</span><span class="sxs-lookup"><span data-stu-id="716e2-156">**Payload** - the message's JSON or raw string payload.</span></span>
* <span data-ttu-id="716e2-157">**Zpětné volání** – funkce zpětného volání.</span><span class="sxs-lookup"><span data-stu-id="716e2-157">**Callback** - the callback function.</span></span>

<span data-ttu-id="716e2-158">Další informace o formátu datové části najdete v tématu **datové části** části [implementace serveru GCM](http://developer.android.com/google/gcm/server.html#payload) dokumentu.</span><span class="sxs-lookup"><span data-stu-id="716e2-158">For more information on the payload format, see the **Payload** section of the [Implementing GCM Server](http://developer.android.com/google/gcm/server.html#payload) document.</span></span>

<span data-ttu-id="716e2-159">Následující kód používá **GcmService** instance vystavené **NotificationHubService** k odesílání nabízených oznámení na všechny klientské počítače registrované.</span><span class="sxs-lookup"><span data-stu-id="716e2-159">The following code uses the **GcmService** instance exposed by the **NotificationHubService** to send a push notification to all registered clients.</span></span>

    var payload = {
      data: {
        message: 'Hello!'
      }
    };
    notificationHubService.gcm.send(null, payload, function(error){
      if(!error){
        //notification sent
      }
    });

### <a name="how-to-send-push-notifications-to-ios-applications"></a><span data-ttu-id="716e2-160">Postupy: odesílání nabízených oznámení do aplikace pro iOS</span><span class="sxs-lookup"><span data-stu-id="716e2-160">How to: Send push notifications to iOS applications</span></span>
<span data-ttu-id="716e2-161">Stejné jako s popsané výše, aplikace pro Android **ApnsService** objekt poskytuje **odeslat** metoda, která slouží k odesílání nabízených oznámení do aplikace pro iOS.</span><span class="sxs-lookup"><span data-stu-id="716e2-161">Same as with Android applications described above, the **ApnsService** object provides a **send** method that can be used to send push notifications to iOS applications.</span></span> <span data-ttu-id="716e2-162">**Odeslat** metoda přijímá následující parametry:</span><span class="sxs-lookup"><span data-stu-id="716e2-162">The **send** method accepts the following parameters:</span></span>

* <span data-ttu-id="716e2-163">**Značky** -identifikátor značky.</span><span class="sxs-lookup"><span data-stu-id="716e2-163">**Tags** - the tag identifier.</span></span> <span data-ttu-id="716e2-164">Pokud je k dispozici žádná značka, odešle se oznámení na všechny klienty.</span><span class="sxs-lookup"><span data-stu-id="716e2-164">If no tag is provided, the notification will be sent to all clients.</span></span>
* <span data-ttu-id="716e2-165">**Datová část** -datovou část JSON nebo řetězec zprávy.</span><span class="sxs-lookup"><span data-stu-id="716e2-165">**Payload** - the message's JSON or string payload.</span></span>
* <span data-ttu-id="716e2-166">**Zpětné volání** – funkce zpětného volání.</span><span class="sxs-lookup"><span data-stu-id="716e2-166">**Callback** - the callback function.</span></span>

<span data-ttu-id="716e2-167">Formát datové části Další informace najdete v tématu **datová část oznámení** části [průvodci místních a nabízených oznámení programování](http://developer.apple.com/library/ios/#documentation/NetworkingInternet/Conceptual/RemoteNotificationsPG/ApplePushService/ApplePushService.html) dokumentu.</span><span class="sxs-lookup"><span data-stu-id="716e2-167">For more information the payload format, see The **Notification Payload** section of the [Local and Push Notification Programming Guide](http://developer.apple.com/library/ios/#documentation/NetworkingInternet/Conceptual/RemoteNotificationsPG/ApplePushService/ApplePushService.html) document.</span></span>

<span data-ttu-id="716e2-168">Následující kód používá **ApnsService** instance vystavené **NotificationHubService** k odeslání zprávy upozornění pro všechny klienty:</span><span class="sxs-lookup"><span data-stu-id="716e2-168">The following code uses the **ApnsService** instance exposed by the **NotificationHubService** to send an alert message to all clients:</span></span>

    var payload={
        alert: 'Hello!'
      };
    notificationHubService.apns.send(null, payload, function(error){
      if(!error){
         // notification sent
      }
    });

### <a name="how-to-send-push-notifications-to-windows-phone-applications"></a><span data-ttu-id="716e2-169">Postupy: odesílání nabízených oznámení do aplikací Windows Phone</span><span class="sxs-lookup"><span data-stu-id="716e2-169">How to: Send push notifications to Windows Phone applications</span></span>
<span data-ttu-id="716e2-170">**MpnsService** objekt poskytuje **odeslat** metoda, která slouží k odesílání nabízených oznámení do aplikace Windows Phone.</span><span class="sxs-lookup"><span data-stu-id="716e2-170">The **MpnsService** object provides a **send** method that can be used to send push notifications to Windows Phone applications.</span></span> <span data-ttu-id="716e2-171">**Odeslat** metoda přijímá následující parametry:</span><span class="sxs-lookup"><span data-stu-id="716e2-171">The **send** method accepts the following parameters:</span></span>

* <span data-ttu-id="716e2-172">**Značky** -identifikátor značky.</span><span class="sxs-lookup"><span data-stu-id="716e2-172">**Tags** - the tag identifier.</span></span> <span data-ttu-id="716e2-173">Pokud je k dispozici žádná značka, odešle se oznámení na všechny klienty.</span><span class="sxs-lookup"><span data-stu-id="716e2-173">If no tag is provided, the notification will be sent to all clients.</span></span>
* <span data-ttu-id="716e2-174">**Datová část** -datovou část XML zprávy.</span><span class="sxs-lookup"><span data-stu-id="716e2-174">**Payload** - the message's XML payload.</span></span>
* <span data-ttu-id="716e2-175">**TargetName**  -  `toast` pro informační zprávy.</span><span class="sxs-lookup"><span data-stu-id="716e2-175">**TargetName** - `toast` for toast notifications.</span></span> <span data-ttu-id="716e2-176">`token`pro dlaždici oznámení.</span><span class="sxs-lookup"><span data-stu-id="716e2-176">`token` for tile notifications.</span></span>
* <span data-ttu-id="716e2-177">**NotificationClass** -prioritu oznámení.</span><span class="sxs-lookup"><span data-stu-id="716e2-177">**NotificationClass** - The priority of the notification.</span></span> <span data-ttu-id="716e2-178">Najdete v článku **prvky záhlaví HTTP** části [nabízená oznámení ze serveru](http://msdn.microsoft.com/library/hh221551.aspx) dokumentu platné hodnoty.</span><span class="sxs-lookup"><span data-stu-id="716e2-178">See the **HTTP Header Elements** section of the [Push notifications from a server](http://msdn.microsoft.com/library/hh221551.aspx) document for valid values.</span></span>
* <span data-ttu-id="716e2-179">**Možnosti** – volitelné hlavičky požadavku.</span><span class="sxs-lookup"><span data-stu-id="716e2-179">**Options** - optional request headers.</span></span>
* <span data-ttu-id="716e2-180">**Zpětné volání** – funkce zpětného volání.</span><span class="sxs-lookup"><span data-stu-id="716e2-180">**Callback** - the callback function.</span></span>

<span data-ttu-id="716e2-181">Seznam platný **TargetName**, **NotificationClass** a možnosti hlaviček, podívejte se [nabízená oznámení ze serveru](http://msdn.microsoft.com/library/hh221551.aspx) stránky.</span><span class="sxs-lookup"><span data-stu-id="716e2-181">For a list of valid **TargetName**, **NotificationClass** and header options, check out the [Push notifications from a server](http://msdn.microsoft.com/library/hh221551.aspx) page.</span></span>

<span data-ttu-id="716e2-182">Následující ukázkový kód používá **MpnsService** instance vystavené **NotificationHubService** k odesílání nabízených oznámení:</span><span class="sxs-lookup"><span data-stu-id="716e2-182">The following sample code uses the **MpnsService** instance exposed by the **NotificationHubService** to send a toast push notification:</span></span>

    var payload = '<?xml version="1.0" encoding="utf-8"?><wp:Notification xmlns:wp="WPNotification"><wp:Toast><wp:Text1>string</wp:Text1><wp:Text2>string</wp:Text2></wp:Toast></wp:Notification>';
    notificationHubService.mpns.send(null, payload, 'toast', 22, function(error){
      if(!error){
        //notification sent
      }
    });

### <a name="how-to-send-push-notifications-to-universal-windows-platform-uwp-applications"></a><span data-ttu-id="716e2-183">Postupy: odesílání nabízených oznámení do aplikací pro univerzální platformu Windows (UWP)</span><span class="sxs-lookup"><span data-stu-id="716e2-183">How to: Send push notifications to Universal Windows Platform (UWP) applications</span></span>
<span data-ttu-id="716e2-184">**WnsService** objekt poskytuje **odeslat** metoda, která slouží k odesílání nabízených oznámení do aplikace pro univerzální platformu Windows.</span><span class="sxs-lookup"><span data-stu-id="716e2-184">The **WnsService** object provides a **send** method that can be used to send push notifications to Universal Windows Platform applications.</span></span>  <span data-ttu-id="716e2-185">**Odeslat** metoda přijímá následující parametry:</span><span class="sxs-lookup"><span data-stu-id="716e2-185">The **send** method accepts the following parameters:</span></span>

* <span data-ttu-id="716e2-186">**Značky** -identifikátor značky.</span><span class="sxs-lookup"><span data-stu-id="716e2-186">**Tags** - the tag identifier.</span></span> <span data-ttu-id="716e2-187">Pokud neexistuje žádná značka je k dispozici, odešle se oznámení na všechny klientské počítače registrované.</span><span class="sxs-lookup"><span data-stu-id="716e2-187">If no tag is provided, the notification will be sent to all registered clients.</span></span>
* <span data-ttu-id="716e2-188">**Datová část** -datovou část zprávy XML.</span><span class="sxs-lookup"><span data-stu-id="716e2-188">**Payload** - the XML message payload.</span></span>
* <span data-ttu-id="716e2-189">**Typ** – typ oznámení.</span><span class="sxs-lookup"><span data-stu-id="716e2-189">**Type** - the notification type.</span></span>
* <span data-ttu-id="716e2-190">**Možnosti** – volitelné hlavičky požadavku.</span><span class="sxs-lookup"><span data-stu-id="716e2-190">**Options** - optional request headers.</span></span>
* <span data-ttu-id="716e2-191">**Zpětné volání** – funkce zpětného volání.</span><span class="sxs-lookup"><span data-stu-id="716e2-191">**Callback** - the callback function.</span></span>

<span data-ttu-id="716e2-192">Seznam platné typy a hlavičky požadavku, naleznete v části [nabízená oznámení hlavičkách žádostí a odpovědí služby](http://msdn.microsoft.com/library/windows/apps/hh465435.aspx).</span><span class="sxs-lookup"><span data-stu-id="716e2-192">For a list of valid types and request headers, see [Push notification service request and response headers](http://msdn.microsoft.com/library/windows/apps/hh465435.aspx).</span></span>

<span data-ttu-id="716e2-193">Následující kód používá **WnsService** instance vystavené **NotificationHubService** odesílat nabízená oznámení do aplikace UWP:</span><span class="sxs-lookup"><span data-stu-id="716e2-193">The following code uses the **WnsService** instance exposed by the **NotificationHubService** to send a toast push notification to a UWP app:</span></span>

    var payload = '<toast><visual><binding template="ToastText01"><text id="1">Hello!</text></binding></visual></toast>';
    notificationHubService.wns.send(null, payload , 'wns/toast', function(error){
      if(!error){
         // notification sent
      }
    });

## <a name="next-steps"></a><span data-ttu-id="716e2-194">Další kroky</span><span class="sxs-lookup"><span data-stu-id="716e2-194">Next Steps</span></span>
<span data-ttu-id="716e2-195">Výše uvedené ukázkové fragmenty umožňují snadno vytvořit infrastrukturu služby Doručovat nabízená oznámení k široké škále zařízení.</span><span class="sxs-lookup"><span data-stu-id="716e2-195">The sample snippets above allow you to easily build service infrastructure to deliver push notifications to a wide variety of devices.</span></span> <span data-ttu-id="716e2-196">Teď, když jste se naučili základy používání centra oznámení s node.js, postupujte podle následujících odkazech na další informace o tom, jak můžete rozšířit tyto další možnosti.</span><span class="sxs-lookup"><span data-stu-id="716e2-196">Now that you've learned the basics of using Notification Hubs with node.js, follow these links to learn more about how you can extend these capabilities further.</span></span>

* <span data-ttu-id="716e2-197">Najdete v článku na webu MSDN odkazy [Azure Notification Hubs](https://msdn.microsoft.com/library/azure/jj927170.aspx).</span><span class="sxs-lookup"><span data-stu-id="716e2-197">See the MSDN Reference for [Azure Notification Hubs](https://msdn.microsoft.com/library/azure/jj927170.aspx).</span></span>
* <span data-ttu-id="716e2-198">Přejděte [Azure SDK pro uzel] úložišti na Githubu Další ukázky a podrobnosti implementace.</span><span class="sxs-lookup"><span data-stu-id="716e2-198">Visit the [Azure SDK for Node] repository on GitHub for more samples and implementation details.</span></span>

<span data-ttu-id="716e2-199">[Azure SDK pro uzel]: https://github.com/WindowsAzure/azure-sdk-for-node</span><span class="sxs-lookup"><span data-stu-id="716e2-199">[Azure SDK for Node]: https://github.com/WindowsAzure/azure-sdk-for-node</span></span>
[Next Steps]: #nextsteps
[What are Service Bus Topics and Subscriptions?]: #what-are-service-bus-topics
[Create a Service Namespace]: #create-a-service-namespace
[Obtain the Default Management Credentials for the Namespace]: #obtain-default-credentials
[Create a Node.js Application]: #Create_a_Nodejs_Application
[Configure Your Application to Use Service Bus]: #Configure_Your_Application_to_Use_Service_Bus
[How to: Create a Topic]: #How_to_Create_a_Topic
[How to: Create Subscriptions]: #How_to_Create_Subscriptions
[How to: Send Messages to a Topic]: #How_to_Send_Messages_to_a_Topic
[How to: Receive Messages from a Subscription]: #How_to_Receive_Messages_from_a_Subscription
[How to: Handle Application Crashes and Unreadable Messages]: #How_to_Handle_Application_Crashes_and_Unreadable_Messages
[How to: Delete Topics and Subscriptions]: #How_to_Delete_Topics_and_Subscriptions
[1]: #Next_Steps
[Topic Concepts]: .media/notification-hubs-nodejs-how-to-use-notification-hubs/sb-topics-01.png
[Azure Classic Portal]: http://manage.windowsazure.com
[image]: .media/notification-hubs-nodejs-how-to-use-notification-hubs/sb-queues-03.png
[2]: .media/notification-hubs-nodejs-how-to-use-notification-hubs/sb-queues-04.png
[3]: .media/notification-hubs-nodejs-how-to-use-notification-hubs/sb-queues-05.png
[4]: .media/notification-hubs-nodejs-how-to-use-notification-hubs/sb-queues-06.png
[5]: .media/notification-hubs-nodejs-how-to-use-notification-hubs/sb-queues-07.png
[SqlFilter.SqlExpression]: http://msdn.microsoft.com/library/windowsazure/microsoft.servicebus.messaging.sqlfilter.sqlexpression.aspx
[Azure Service Bus Notification Hubs]: http://msdn.microsoft.com/library/windowsazure/jj927170.aspx
[SqlFilter]: http://msdn.microsoft.com/library/windowsazure/microsoft.servicebus.messaging.sqlfilter.aspx
<span data-ttu-id="716e2-200">[webu pomocí služby WebMatrix]: /develop/nodejs/tutorials/web-site-with-webmatrix/</span><span class="sxs-lookup"><span data-stu-id="716e2-200">[Web Site with WebMatrix]: /develop/nodejs/tutorials/web-site-with-webmatrix/</span></span>
[Node.js Cloud Service]: ../cloud-services/cloud-services-nodejs-develop-deploy-app.md
[Previous Management Portal]: .media/notification-hubs-nodejs-how-to-use-notification-hubs/previous-portal.png
[nodejswebsite]: /develop/nodejs/tutorials/create-a-website-(mac)/
[Node.js Cloud Service with Storage]: /develop/nodejs/tutorials/web-app-with-storage/
[Node.js Web Application with Storage]: /develop/nodejs/tutorials/web-site-with-storage/
<span data-ttu-id="716e2-201">[portálu Azure]: https://portal.azure.com</span><span class="sxs-lookup"><span data-stu-id="716e2-201">[Azure Portal]: https://portal.azure.com</span></span>
