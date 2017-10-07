---
title: "aaaSending nabízená oznámení pomocí Azure Notification Hubs a Node.js"
description: "Zjistěte, jak toouse Notification Hubs toosend nabízená oznámení z aplikace Node.js."
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
ms.openlocfilehash: 151d224fa6dd07e4acdc3a4887c4e95ee03168c4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="sending-push-notifications-with-azure-notification-hubs-and-nodejs"></a><span data-ttu-id="9b5b2-104">Odesílání nabízených oznámení pomocí Azure Notification Hubs a Node.js</span><span class="sxs-lookup"><span data-stu-id="9b5b2-104">Sending push notifications with Azure Notification Hubs and Node.js</span></span>
[!INCLUDE [notification-hubs-backend-how-to-selector](../../includes/notification-hubs-backend-how-to-selector.md)]

## <a name="overview"></a><span data-ttu-id="9b5b2-105">Přehled</span><span class="sxs-lookup"><span data-stu-id="9b5b2-105">Overview</span></span>
> [!IMPORTANT]
> <span data-ttu-id="9b5b2-106">toocomplete tento kurz, musíte mít aktivní účet Azure.</span><span class="sxs-lookup"><span data-stu-id="9b5b2-106">toocomplete this tutorial, you must have an active Azure account.</span></span> <span data-ttu-id="9b5b2-107">Pokud účet nemáte, můžete si během několika minut vytvořit bezplatný zkušební účet.</span><span class="sxs-lookup"><span data-stu-id="9b5b2-107">If you don't have an account, you can create a free trial account in just a couple of minutes.</span></span> <span data-ttu-id="9b5b2-108">Podrobnosti najdete v článku [Bezplatná zkušební verze Azure](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A643EE910&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdocumentation%2Farticles%2Fnotification-hubs-nodejs-how-to-use-notification-hubs).</span><span class="sxs-lookup"><span data-stu-id="9b5b2-108">For details, see [Azure Free Trial](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A643EE910&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdocumentation%2Farticles%2Fnotification-hubs-nodejs-how-to-use-notification-hubs).</span></span>
> 
> 

<span data-ttu-id="9b5b2-109">Tento průvodce vám ukáže, jak toosend nabízená oznámení pomocí hello Azure Notification Hubs přímo z aplikace Node.js.</span><span class="sxs-lookup"><span data-stu-id="9b5b2-109">This guide will show you how toosend push notifications with hello help of Azure Notification Hubs directly from a Node.js application.</span></span> 

<span data-ttu-id="9b5b2-110">Hello pokryté scénáře zahrnují, odesílání nabízených oznámení tooapplications na hello následující platformy:</span><span class="sxs-lookup"><span data-stu-id="9b5b2-110">hello scenarios covered include sending push notifications tooapplications on hello following platforms:</span></span>

* <span data-ttu-id="9b5b2-111">Android</span><span class="sxs-lookup"><span data-stu-id="9b5b2-111">Android</span></span>
* <span data-ttu-id="9b5b2-112">iOS</span><span class="sxs-lookup"><span data-stu-id="9b5b2-112">iOS</span></span>
* <span data-ttu-id="9b5b2-113">telefon se systémem Windows</span><span class="sxs-lookup"><span data-stu-id="9b5b2-113">Windows Phone</span></span>
* <span data-ttu-id="9b5b2-114">Univerzální platforma pro Windows</span><span class="sxs-lookup"><span data-stu-id="9b5b2-114">Universal Windows Platform</span></span> 

<span data-ttu-id="9b5b2-115">Další informace o notification hubs najdete v tématu hello [další kroky](#next) části.</span><span class="sxs-lookup"><span data-stu-id="9b5b2-115">For more information on notification hubs, see hello [Next Steps](#next) section.</span></span>

## <a name="what-are-notification-hubs"></a><span data-ttu-id="9b5b2-116">Co jsou Notification Hubs?</span><span class="sxs-lookup"><span data-stu-id="9b5b2-116">What are Notification Hubs?</span></span>
<span data-ttu-id="9b5b2-117">Azure Notification Hubs poskytuje snadno použitelnou, více platformami, škálovatelné infrastruktury pro odesílání nabízených oznámení toomobile zařízení.</span><span class="sxs-lookup"><span data-stu-id="9b5b2-117">Azure Notification Hubs provide an easy-to-use, multi-platform, scalable infrastructure for sending push notifications toomobile devices.</span></span> <span data-ttu-id="9b5b2-118">Podrobnosti o hello služby infrastruktury, najdete v části hello [Azure Notification Hubs](http://msdn.microsoft.com/library/windowsazure/jj927170.aspx) stránky.</span><span class="sxs-lookup"><span data-stu-id="9b5b2-118">For details on hello service infrastructure, see hello [Azure Notification Hubs](http://msdn.microsoft.com/library/windowsazure/jj927170.aspx) page.</span></span>

## <a name="create-a-nodejs-application"></a><span data-ttu-id="9b5b2-119">Vytvoření aplikace Node.js</span><span class="sxs-lookup"><span data-stu-id="9b5b2-119">Create a Node.js Application</span></span>
<span data-ttu-id="9b5b2-120">Hello první krok v tomto kurzu je vytvoření nové prázdné aplikace Node.js.</span><span class="sxs-lookup"><span data-stu-id="9b5b2-120">hello first step in this tutorial is creating a new blank Node.js application.</span></span> <span data-ttu-id="9b5b2-121">Pokyny pro vytvoření aplikace Node.js, naleznete v části [vytvořit a nasadit tooAzure aplikace Node.js webu][nodejswebsite], [Node.js Cloudová služba] [ Node.js Cloud Service] pomocí prostředí Windows PowerShell nebo [webu pomocí služby WebMatrix].</span><span class="sxs-lookup"><span data-stu-id="9b5b2-121">For instructions on creating a Node.js application, see [Create and deploy a Node.js application tooAzure Web Site][nodejswebsite], [Node.js Cloud Service][Node.js Cloud Service] using Windows PowerShell, or [Web Site with WebMatrix].</span></span>

## <a name="configure-your-application-toouse-notification-hubs"></a><span data-ttu-id="9b5b2-122">Konfigurace centra oznámení tooUse vaše aplikace</span><span class="sxs-lookup"><span data-stu-id="9b5b2-122">Configure Your Application tooUse Notification Hubs</span></span>
<span data-ttu-id="9b5b2-123">toouse Azure Notification Hubs, budete potřebovat toodownload a použití hello Node.js [balíček azure](https://www.npmjs.com/package/azure), což zahrnuje integrovanou sadu pomocné knihovny, které komunikují REST služeb hello nabízených oznámení.</span><span class="sxs-lookup"><span data-stu-id="9b5b2-123">toouse Azure Notification Hubs, you need toodownload and use hello Node.js [azure package](https://www.npmjs.com/package/azure), which includes a built-in set of helper libraries that communicate with hello push notification REST services.</span></span>

### <a name="use-node-package-manager-npm-tooobtain-hello-package"></a><span data-ttu-id="9b5b2-124">Použijte uzel balíček správce (NPM) tooobtain hello balíček</span><span class="sxs-lookup"><span data-stu-id="9b5b2-124">Use Node Package Manager (NPM) tooobtain hello package</span></span>
1. <span data-ttu-id="9b5b2-125">Pomocí rozhraní příkazového řádku, jako například **prostředí PowerShell** (Windows), **Terminálové** (Mac), nebo **Bash** (Linux) a přejděte toohello složku, kde můžete vytvořit prázdnou aplikaci .</span><span class="sxs-lookup"><span data-stu-id="9b5b2-125">Use a command-line interface such as **PowerShell** (Windows), **Terminal** (Mac), or **Bash** (Linux) and navigate toohello folder where you created your blank application.</span></span>
2. <span data-ttu-id="9b5b2-126">Typ **npm nainstalujte azure sb** v příkazovém okně hello.</span><span class="sxs-lookup"><span data-stu-id="9b5b2-126">Type **npm install azure-sb** in hello command window.</span></span>
3. <span data-ttu-id="9b5b2-127">Můžete ručně spustit hello **ls** nebo **dir** tooverify příkaz, **uzlu\_moduly** složka byla vytvořena.</span><span class="sxs-lookup"><span data-stu-id="9b5b2-127">You can manually run hello **ls** or **dir** command tooverify that a **node\_modules** folder was created.</span></span> <span data-ttu-id="9b5b2-128">V této složce najít hello **azure** balíček, který obsahuje hello knihovny, je nutné tooaccess hello centra oznámení.</span><span class="sxs-lookup"><span data-stu-id="9b5b2-128">Inside that folder, find hello **azure** package, which contains hello libraries you need tooaccess hello Notification Hub.</span></span>

> [!NOTE]
> <span data-ttu-id="9b5b2-129">Další informace o instalaci NPM hello oficiální [NPM blog](http://blog.npmjs.org/post/85484771375/how-to-install-npm).</span><span class="sxs-lookup"><span data-stu-id="9b5b2-129">You can learn more about installing NPM on hello official [NPM blog](http://blog.npmjs.org/post/85484771375/how-to-install-npm).</span></span> 
> 
> 

### <a name="import-hello-module"></a><span data-ttu-id="9b5b2-130">Import modulu hello</span><span class="sxs-lookup"><span data-stu-id="9b5b2-130">Import hello module</span></span>
<span data-ttu-id="9b5b2-131">Pomocí textového editoru, přidejte následující toohello horní části hello hello **server.js** souboru aplikace hello:</span><span class="sxs-lookup"><span data-stu-id="9b5b2-131">Using a text editor, add hello following toohello top of hello **server.js** file of hello application:</span></span>

    var azure = require('azure');

### <a name="setup-an-azure-notification-hub-connection"></a><span data-ttu-id="9b5b2-132">Nastavit připojení k Azure Notification Hubs</span><span class="sxs-lookup"><span data-stu-id="9b5b2-132">Setup an Azure Notification Hub connection</span></span>
<span data-ttu-id="9b5b2-133">Hello **NotificationHubService** objekt vám umožňuje spolupracovat s centry oznámení.</span><span class="sxs-lookup"><span data-stu-id="9b5b2-133">hello **NotificationHubService** object lets you work with notification hubs.</span></span> <span data-ttu-id="9b5b2-134">Hello následující kód vytvoří **NotificationHubService** objekt pro rozbočovač nofication hello s názvem **hubname**.</span><span class="sxs-lookup"><span data-stu-id="9b5b2-134">hello following code creates a **NotificationHubService** object for hello nofication hub named **hubname**.</span></span> <span data-ttu-id="9b5b2-135">Přidat v hello horní části hello **server.js** souboru po hello příkaz tooimport hello azure modul:</span><span class="sxs-lookup"><span data-stu-id="9b5b2-135">Add it near hello top of hello **server.js** file, after hello statement tooimport hello azure module:</span></span>

    var notificationHubService = azure.createNotificationHubService('hubname','connectionstring');

<span data-ttu-id="9b5b2-136">Hello připojení **connectionstring** nelze získat hodnotu z hello [portálu Azure] provedením hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="9b5b2-136">hello connection **connectionstring** value can be obtained from hello [Azure Portal] by performing hello following steps:</span></span>

1. <span data-ttu-id="9b5b2-137">V levém navigačním podokně hello, klikněte na **Procházet**.</span><span class="sxs-lookup"><span data-stu-id="9b5b2-137">In hello left navigation pane, click **Browse**.</span></span>
2. <span data-ttu-id="9b5b2-138">Vyberte **Notification Hubs**a potom najít hello centra chcete toouse pro ukázku hello.</span><span class="sxs-lookup"><span data-stu-id="9b5b2-138">Select **Notification Hubs**, and then find hello hub you wish toouse for hello sample.</span></span> <span data-ttu-id="9b5b2-139">Můžete se podívat toohello [Windows Store Začínáme kurzu](notification-hubs-windows-store-dotnet-get-started-wns-push-notification.md) Pokud potřebujete pomoc, vytváření nového centra oznámení.</span><span class="sxs-lookup"><span data-stu-id="9b5b2-139">You can refer toohello [Windows Store Getting Started tutorial](notification-hubs-windows-store-dotnet-get-started-wns-push-notification.md) if you need help creating a new Notification Hub.</span></span>
3. <span data-ttu-id="9b5b2-140">Vyberte **nastavení**.</span><span class="sxs-lookup"><span data-stu-id="9b5b2-140">Select **Settings**.</span></span>
4. <span data-ttu-id="9b5b2-141">Klikněte na **zásady přístupu**.</span><span class="sxs-lookup"><span data-stu-id="9b5b2-141">Click on **Access Policies**.</span></span> <span data-ttu-id="9b5b2-142">Zobrazí se oba připojovací řetězce sdílené a úplný přístup.</span><span class="sxs-lookup"><span data-stu-id="9b5b2-142">You will see both shared and full access connection strings.</span></span>

![Portál Azure – centra oznámení](./media/notification-hubs-nodejs-how-to-use-notification-hubs/notification-hubs-portal.png)

> [!NOTE]
> <span data-ttu-id="9b5b2-144">Můžete také načíst hello připojovací řetězec pomocí hello **Get-AzureSbNamespace** rutiny poskytované [prostředí Azure PowerShell](/powershell/azureps-cmdlets-docs) nebo hello **azure sb obor názvů zobrazit** s Hello [rozhraní příkazového řádku Azure (Azure CLI)](../cli-install-nodejs.md).</span><span class="sxs-lookup"><span data-stu-id="9b5b2-144">You can also retrieve hello connection string using hello **Get-AzureSbNamespace** cmdlet provided by [Azure PowerShell](/powershell/azureps-cmdlets-docs) or hello **azure sb namespace show** command with hello [Azure Command-Line Interface (Azure CLI)](../cli-install-nodejs.md).</span></span>
> 
> 

## <a name="general-architecture"></a><span data-ttu-id="9b5b2-145">Obecná architektura</span><span class="sxs-lookup"><span data-stu-id="9b5b2-145">General architecture</span></span>
<span data-ttu-id="9b5b2-146">Hello **NotificationHubService** objekt poskytuje hello následující instance objektů pro odesílání nabízených oznámení toospecific zařízení a aplikací:</span><span class="sxs-lookup"><span data-stu-id="9b5b2-146">hello **NotificationHubService** object exposes hello following object instances for sending push notifications toospecific devices and applications:</span></span>

* <span data-ttu-id="9b5b2-147">**Android** -použít hello **GcmService** objekt, který je k dispozici na **notificationHubService.gcm**</span><span class="sxs-lookup"><span data-stu-id="9b5b2-147">**Android** - use hello **GcmService** object, which is available at **notificationHubService.gcm**</span></span>
* <span data-ttu-id="9b5b2-148">**iOS** -použít hello **ApnsService** objekt, který je přístupná na **notificationHubService.apns**</span><span class="sxs-lookup"><span data-stu-id="9b5b2-148">**iOS** - use hello **ApnsService** object, which is accessible at **notificationHubService.apns**</span></span>
* <span data-ttu-id="9b5b2-149">**Windows Phone** -použít hello **MpnsService** objekt, který je k dispozici na **notificationHubService.mpns**</span><span class="sxs-lookup"><span data-stu-id="9b5b2-149">**Windows Phone** - use hello **MpnsService** object, which is available at **notificationHubService.mpns**</span></span>
* <span data-ttu-id="9b5b2-150">**Univerzální platformu Windows** -použít hello **WnsService** objekt, který je k dispozici na **notificationHubService.wns**</span><span class="sxs-lookup"><span data-stu-id="9b5b2-150">**Universal Windows Platform** - use hello **WnsService** object, which is available at **notificationHubService.wns**</span></span>

### <a name="how-to-send-push-notifications-tooandroid-applications"></a><span data-ttu-id="9b5b2-151">Postupy: odeslání nabízených oznámení tooAndroid aplikace</span><span class="sxs-lookup"><span data-stu-id="9b5b2-151">How to: Send push notifications tooAndroid applications</span></span>
<span data-ttu-id="9b5b2-152">Hello **GcmService** objekt poskytuje **odeslat** metoda, kterou lze použít toosend nabízená oznámení tooAndroid aplikace.</span><span class="sxs-lookup"><span data-stu-id="9b5b2-152">hello **GcmService** object provides a **send** method that can be used toosend push notifications tooAndroid applications.</span></span> <span data-ttu-id="9b5b2-153">Hello **odeslat** metoda přijímá hello následující parametry:</span><span class="sxs-lookup"><span data-stu-id="9b5b2-153">hello **send** method accepts hello following parameters:</span></span>

* <span data-ttu-id="9b5b2-154">**Značky** -hello identifikátor značky.</span><span class="sxs-lookup"><span data-stu-id="9b5b2-154">**Tags** - hello tag identifier.</span></span> <span data-ttu-id="9b5b2-155">Pokud je k dispozici žádná značka, hello odešle upozornění tooall klientů.</span><span class="sxs-lookup"><span data-stu-id="9b5b2-155">If no tag is provided, hello notification will be sent tooall clients.</span></span>
* <span data-ttu-id="9b5b2-156">**Datová část** -hello JSON nebo nezpracovaný řetězec datovou část zprávy.</span><span class="sxs-lookup"><span data-stu-id="9b5b2-156">**Payload** - hello message's JSON or raw string payload.</span></span>
* <span data-ttu-id="9b5b2-157">**Zpětné volání** -hello funkce zpětného volání.</span><span class="sxs-lookup"><span data-stu-id="9b5b2-157">**Callback** - hello callback function.</span></span>

<span data-ttu-id="9b5b2-158">Další informace o formát datové části hello najdete v tématu hello **datové části** části hello [implementace serveru GCM](http://developer.android.com/google/gcm/server.html#payload) dokumentu.</span><span class="sxs-lookup"><span data-stu-id="9b5b2-158">For more information on hello payload format, see hello **Payload** section of hello [Implementing GCM Server](http://developer.android.com/google/gcm/server.html#payload) document.</span></span>

<span data-ttu-id="9b5b2-159">Hello následující kód používá hello **GcmService** instance vystavené hello **NotificationHubService** toosend nabízených oznámení tooall zaregistrovat klienty.</span><span class="sxs-lookup"><span data-stu-id="9b5b2-159">hello following code uses hello **GcmService** instance exposed by hello **NotificationHubService** toosend a push notification tooall registered clients.</span></span>

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

### <a name="how-to-send-push-notifications-tooios-applications"></a><span data-ttu-id="9b5b2-160">Postupy: odeslání nabízených oznámení tooiOS aplikace</span><span class="sxs-lookup"><span data-stu-id="9b5b2-160">How to: Send push notifications tooiOS applications</span></span>
<span data-ttu-id="9b5b2-161">Stejné jako u aplikací pro Android popsané výše, hello **ApnsService** objekt poskytuje **odeslat** metoda, kterou lze použít toosend nabízená oznámení tooiOS aplikace.</span><span class="sxs-lookup"><span data-stu-id="9b5b2-161">Same as with Android applications described above, hello **ApnsService** object provides a **send** method that can be used toosend push notifications tooiOS applications.</span></span> <span data-ttu-id="9b5b2-162">Hello **odeslat** metoda přijímá hello následující parametry:</span><span class="sxs-lookup"><span data-stu-id="9b5b2-162">hello **send** method accepts hello following parameters:</span></span>

* <span data-ttu-id="9b5b2-163">**Značky** -hello identifikátor značky.</span><span class="sxs-lookup"><span data-stu-id="9b5b2-163">**Tags** - hello tag identifier.</span></span> <span data-ttu-id="9b5b2-164">Pokud je k dispozici žádná značka, hello odešle upozornění tooall klientů.</span><span class="sxs-lookup"><span data-stu-id="9b5b2-164">If no tag is provided, hello notification will be sent tooall clients.</span></span>
* <span data-ttu-id="9b5b2-165">**Datová část** – hello JSON dané zprávy nebo string datové části.</span><span class="sxs-lookup"><span data-stu-id="9b5b2-165">**Payload** - hello message's JSON or string payload.</span></span>
* <span data-ttu-id="9b5b2-166">**Zpětné volání** -hello funkce zpětného volání.</span><span class="sxs-lookup"><span data-stu-id="9b5b2-166">**Callback** - hello callback function.</span></span>

<span data-ttu-id="9b5b2-167">Formát datové části hello Další informace naleznete v tématu hello **datová část oznámení** části hello [průvodci místních a nabízených oznámení programování](http://developer.apple.com/library/ios/#documentation/NetworkingInternet/Conceptual/RemoteNotificationsPG/ApplePushService/ApplePushService.html) dokumentu.</span><span class="sxs-lookup"><span data-stu-id="9b5b2-167">For more information hello payload format, see hello **Notification Payload** section of hello [Local and Push Notification Programming Guide](http://developer.apple.com/library/ios/#documentation/NetworkingInternet/Conceptual/RemoteNotificationsPG/ApplePushService/ApplePushService.html) document.</span></span>

<span data-ttu-id="9b5b2-168">Hello následující kód používá hello **ApnsService** instance vystavené hello **NotificationHubService** toosend výstrahu zprávy tooall klientů:</span><span class="sxs-lookup"><span data-stu-id="9b5b2-168">hello following code uses hello **ApnsService** instance exposed by hello **NotificationHubService** toosend an alert message tooall clients:</span></span>

    var payload={
        alert: 'Hello!'
      };
    notificationHubService.apns.send(null, payload, function(error){
      if(!error){
         // notification sent
      }
    });

### <a name="how-to-send-push-notifications-toowindows-phone-applications"></a><span data-ttu-id="9b5b2-169">Postupy: odeslání nabízených oznámení tooWindows telefonní aplikace</span><span class="sxs-lookup"><span data-stu-id="9b5b2-169">How to: Send push notifications tooWindows Phone applications</span></span>
<span data-ttu-id="9b5b2-170">Hello **MpnsService** objekt poskytuje **odeslat** metoda, kterou lze použít toosend nabízená oznámení tooWindows telefonní aplikace.</span><span class="sxs-lookup"><span data-stu-id="9b5b2-170">hello **MpnsService** object provides a **send** method that can be used toosend push notifications tooWindows Phone applications.</span></span> <span data-ttu-id="9b5b2-171">Hello **odeslat** metoda přijímá hello následující parametry:</span><span class="sxs-lookup"><span data-stu-id="9b5b2-171">hello **send** method accepts hello following parameters:</span></span>

* <span data-ttu-id="9b5b2-172">**Značky** -hello identifikátor značky.</span><span class="sxs-lookup"><span data-stu-id="9b5b2-172">**Tags** - hello tag identifier.</span></span> <span data-ttu-id="9b5b2-173">Pokud je k dispozici žádná značka, hello odešle upozornění tooall klientů.</span><span class="sxs-lookup"><span data-stu-id="9b5b2-173">If no tag is provided, hello notification will be sent tooall clients.</span></span>
* <span data-ttu-id="9b5b2-174">**Datová část** -hello datovou část zprávy XML.</span><span class="sxs-lookup"><span data-stu-id="9b5b2-174">**Payload** - hello message's XML payload.</span></span>
* <span data-ttu-id="9b5b2-175">**TargetName**  -  `toast` pro informační zprávy.</span><span class="sxs-lookup"><span data-stu-id="9b5b2-175">**TargetName** - `toast` for toast notifications.</span></span> <span data-ttu-id="9b5b2-176">`token`pro dlaždici oznámení.</span><span class="sxs-lookup"><span data-stu-id="9b5b2-176">`token` for tile notifications.</span></span>
* <span data-ttu-id="9b5b2-177">**NotificationClass** -hello prioritu hello oznámení.</span><span class="sxs-lookup"><span data-stu-id="9b5b2-177">**NotificationClass** - hello priority of hello notification.</span></span> <span data-ttu-id="9b5b2-178">V tématu hello **prvky záhlaví HTTP** části hello [nabízená oznámení ze serveru](http://msdn.microsoft.com/library/hh221551.aspx) dokumentu platné hodnoty.</span><span class="sxs-lookup"><span data-stu-id="9b5b2-178">See hello **HTTP Header Elements** section of hello [Push notifications from a server](http://msdn.microsoft.com/library/hh221551.aspx) document for valid values.</span></span>
* <span data-ttu-id="9b5b2-179">**Možnosti** – volitelné hlavičky požadavku.</span><span class="sxs-lookup"><span data-stu-id="9b5b2-179">**Options** - optional request headers.</span></span>
* <span data-ttu-id="9b5b2-180">**Zpětné volání** -hello funkce zpětného volání.</span><span class="sxs-lookup"><span data-stu-id="9b5b2-180">**Callback** - hello callback function.</span></span>

<span data-ttu-id="9b5b2-181">Seznam platný **TargetName**, **NotificationClass** a možnosti hlaviček, podívejte se na hello [nabízená oznámení ze serveru](http://msdn.microsoft.com/library/hh221551.aspx) stránky.</span><span class="sxs-lookup"><span data-stu-id="9b5b2-181">For a list of valid **TargetName**, **NotificationClass** and header options, check out hello [Push notifications from a server](http://msdn.microsoft.com/library/hh221551.aspx) page.</span></span>

<span data-ttu-id="9b5b2-182">Následující ukázkový kód Hello používá hello **MpnsService** instance vystavené hello **NotificationHubService** toosend nabízených oznámení:</span><span class="sxs-lookup"><span data-stu-id="9b5b2-182">hello following sample code uses hello **MpnsService** instance exposed by hello **NotificationHubService** toosend a toast push notification:</span></span>

    var payload = '<?xml version="1.0" encoding="utf-8"?><wp:Notification xmlns:wp="WPNotification"><wp:Toast><wp:Text1>string</wp:Text1><wp:Text2>string</wp:Text2></wp:Toast></wp:Notification>';
    notificationHubService.mpns.send(null, payload, 'toast', 22, function(error){
      if(!error){
        //notification sent
      }
    });

### <a name="how-to-send-push-notifications-toouniversal-windows-platform-uwp-applications"></a><span data-ttu-id="9b5b2-183">Postupy: odeslání nabízených oznámení tooUniversal platformu Windows (UWP) aplikací</span><span class="sxs-lookup"><span data-stu-id="9b5b2-183">How to: Send push notifications tooUniversal Windows Platform (UWP) applications</span></span>
<span data-ttu-id="9b5b2-184">Hello **WnsService** objekt poskytuje **odeslat** metoda, kterou lze použít toosend nabízená oznámení tooUniversal platformu Windows aplikace.</span><span class="sxs-lookup"><span data-stu-id="9b5b2-184">hello **WnsService** object provides a **send** method that can be used toosend push notifications tooUniversal Windows Platform applications.</span></span>  <span data-ttu-id="9b5b2-185">Hello **odeslat** metoda přijímá hello následující parametry:</span><span class="sxs-lookup"><span data-stu-id="9b5b2-185">hello **send** method accepts hello following parameters:</span></span>

* <span data-ttu-id="9b5b2-186">**Značky** -hello identifikátor značky.</span><span class="sxs-lookup"><span data-stu-id="9b5b2-186">**Tags** - hello tag identifier.</span></span> <span data-ttu-id="9b5b2-187">Pokud je k dispozici žádná značka, hello odešle upozornění tooall zaregistrovat klienty.</span><span class="sxs-lookup"><span data-stu-id="9b5b2-187">If no tag is provided, hello notification will be sent tooall registered clients.</span></span>
* <span data-ttu-id="9b5b2-188">**Datová část** -hello datovou část zprávy XML.</span><span class="sxs-lookup"><span data-stu-id="9b5b2-188">**Payload** - hello XML message payload.</span></span>
* <span data-ttu-id="9b5b2-189">**Typ** -hello typ oznámení.</span><span class="sxs-lookup"><span data-stu-id="9b5b2-189">**Type** - hello notification type.</span></span>
* <span data-ttu-id="9b5b2-190">**Možnosti** – volitelné hlavičky požadavku.</span><span class="sxs-lookup"><span data-stu-id="9b5b2-190">**Options** - optional request headers.</span></span>
* <span data-ttu-id="9b5b2-191">**Zpětné volání** -hello funkce zpětného volání.</span><span class="sxs-lookup"><span data-stu-id="9b5b2-191">**Callback** - hello callback function.</span></span>

<span data-ttu-id="9b5b2-192">Seznam platné typy a hlavičky požadavku, naleznete v části [nabízená oznámení hlavičkách žádostí a odpovědí služby](http://msdn.microsoft.com/library/windows/apps/hh465435.aspx).</span><span class="sxs-lookup"><span data-stu-id="9b5b2-192">For a list of valid types and request headers, see [Push notification service request and response headers](http://msdn.microsoft.com/library/windows/apps/hh465435.aspx).</span></span>

<span data-ttu-id="9b5b2-193">Hello následující kód používá hello **WnsService** instance vystavené hello **NotificationHubService** toosend aplikací UWP tooa nabízená oznámení informační zprávy:</span><span class="sxs-lookup"><span data-stu-id="9b5b2-193">hello following code uses hello **WnsService** instance exposed by hello **NotificationHubService** toosend a toast push notification tooa UWP app:</span></span>

    var payload = '<toast><visual><binding template="ToastText01"><text id="1">Hello!</text></binding></visual></toast>';
    notificationHubService.wns.send(null, payload , 'wns/toast', function(error){
      if(!error){
         // notification sent
      }
    });

## <a name="next-steps"></a><span data-ttu-id="9b5b2-194">Další kroky</span><span class="sxs-lookup"><span data-stu-id="9b5b2-194">Next Steps</span></span>
<span data-ttu-id="9b5b2-195">výše uvedené fragmenty ukázka Hello umožňují vám tooeasily sestavení služby infrastruktury toodeliver nabízená oznámení tooa širokou škálu zařízení.</span><span class="sxs-lookup"><span data-stu-id="9b5b2-195">hello sample snippets above allow you tooeasily build service infrastructure toodeliver push notifications tooa wide variety of devices.</span></span> <span data-ttu-id="9b5b2-196">Teď, když jste se naučili základy používání centra oznámení s node.js hello, postupujte podle těchto odkazů toolearn informace o tom, jak můžete rozšířit tyto další možnosti.</span><span class="sxs-lookup"><span data-stu-id="9b5b2-196">Now that you've learned hello basics of using Notification Hubs with node.js, follow these links toolearn more about how you can extend these capabilities further.</span></span>

* <span data-ttu-id="9b5b2-197">V tématu hello referenční dokumentace MSDN pro [Azure Notification Hubs](https://msdn.microsoft.com/library/azure/jj927170.aspx).</span><span class="sxs-lookup"><span data-stu-id="9b5b2-197">See hello MSDN Reference for [Azure Notification Hubs](https://msdn.microsoft.com/library/azure/jj927170.aspx).</span></span>
* <span data-ttu-id="9b5b2-198">Navštivte hello [Azure SDK pro uzel] úložišti na Githubu Další ukázky a podrobnosti implementace.</span><span class="sxs-lookup"><span data-stu-id="9b5b2-198">Visit hello [Azure SDK for Node] repository on GitHub for more samples and implementation details.</span></span>

[Azure SDK pro uzel]: https://github.com/WindowsAzure/azure-sdk-for-node
[Next Steps]: #nextsteps
[What are Service Bus Topics and Subscriptions?]: #what-are-service-bus-topics
[Create a Service Namespace]: #create-a-service-namespace
[Obtain hello Default Management Credentials for hello Namespace]: #obtain-default-credentials
[Create a Node.js Application]: #Create_a_Nodejs_Application
[Configure Your Application tooUse Service Bus]: #Configure_Your_Application_to_Use_Service_Bus
[How to: Create a Topic]: #How_to_Create_a_Topic
[How to: Create Subscriptions]: #How_to_Create_Subscriptions
[How to: Send Messages tooa Topic]: #How_to_Send_Messages_to_a_Topic
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
[Webový server pomocí služby WebMatrix]: /develop/nodejs/tutorials/web-site-with-webmatrix/
[Node.js Cloud Service]: ../cloud-services/cloud-services-nodejs-develop-deploy-app.md
[Previous Management Portal]: .media/notification-hubs-nodejs-how-to-use-notification-hubs/previous-portal.png
[nodejswebsite]: /develop/nodejs/tutorials/create-a-website-(mac)/
[Node.js Cloud Service with Storage]: /develop/nodejs/tutorials/web-app-with-storage/
[Node.js Web Application with Storage]: /develop/nodejs/tutorials/web-site-with-storage/
[Azure Portal]: https://portal.azure.com
