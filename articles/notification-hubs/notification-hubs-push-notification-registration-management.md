---
title: "Registrace správy"
description: "Toto téma vysvětluje, jak k registraci zařízení s centry oznámení, aby bylo možné přijímat nabízená oznámení."
services: notification-hubs
documentationcenter: .net
author: ysxu
manager: erikre
editor: 
ms.assetid: fd0ee230-132c-4143-b4f9-65cef7f463a1
ms.service: notification-hubs
ms.workload: mobile
ms.tgt_pltfrm: mobile-multiple
ms.devlang: dotnet
ms.topic: article
ms.date: 06/29/2016
ms.author: yuaxu
ms.openlocfilehash: a1a349150ef4c7837932706f0c4fcc8d022ec7ab
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="registration-management"></a><span data-ttu-id="99d79-103">Správa registrací</span><span class="sxs-lookup"><span data-stu-id="99d79-103">Registration management</span></span>
## <a name="overview"></a><span data-ttu-id="99d79-104">Přehled</span><span class="sxs-lookup"><span data-stu-id="99d79-104">Overview</span></span>
<span data-ttu-id="99d79-105">Toto téma vysvětluje, jak k registraci zařízení s centry oznámení, aby bylo možné přijímat nabízená oznámení.</span><span class="sxs-lookup"><span data-stu-id="99d79-105">This topic explains how to register devices with notification hubs in order to receive push notifications.</span></span> <span data-ttu-id="99d79-106">Téma popisuje registrace na vysoké úrovni a potom zavádí dvě hlavní vzory pro registraci zařízení: registraci ze zařízení přímo k centru oznámení a registraci prostřednictvím back-end aplikace.</span><span class="sxs-lookup"><span data-stu-id="99d79-106">The topic describes registrations at a high level, then introduces the two main patterns for registering devices: registering from the device directly to the notification hub, and registering through an application backend.</span></span> 

## <a name="what-is-device-registration"></a><span data-ttu-id="99d79-107">Co je registrace zařízení</span><span class="sxs-lookup"><span data-stu-id="99d79-107">What is device registration</span></span>
<span data-ttu-id="99d79-108">Registrace zařízení s centrem oznámení se provádí pomocí **registrace** nebo **instalace**.</span><span class="sxs-lookup"><span data-stu-id="99d79-108">Device registration with a Notification Hub is accomplished using a **Registration** or **Installation**.</span></span>

#### <a name="registrations"></a><span data-ttu-id="99d79-109">Registrace</span><span class="sxs-lookup"><span data-stu-id="99d79-109">Registrations</span></span>
<span data-ttu-id="99d79-110">Registrace přidruží popisovač služby oznámení platformy (PNS) pro zařízení značky a případně šablonu.</span><span class="sxs-lookup"><span data-stu-id="99d79-110">A registration associates the Platform Notification Service (PNS) handle for a device with tags and possibly a template.</span></span> <span data-ttu-id="99d79-111">Popisovače systému PNS může být ChannelURI, token zařízení nebo id registrace GCM.</span><span class="sxs-lookup"><span data-stu-id="99d79-111">The PNS handle could be a ChannelURI, device token, or GCM registration id.</span></span> <span data-ttu-id="99d79-112">Značky se používají k oznámení směrovat na správná sada popisovačů zařízení.</span><span class="sxs-lookup"><span data-stu-id="99d79-112">Tags are used to route notifications to the correct set of device handles.</span></span> <span data-ttu-id="99d79-113">Další informace najdete v tématu [směrování a značky výrazy](notification-hubs-tags-segment-push-message.md).</span><span class="sxs-lookup"><span data-stu-id="99d79-113">For more information, see [Routing and Tag Expressions](notification-hubs-tags-segment-push-message.md).</span></span> <span data-ttu-id="99d79-114">Šablony slouží k implementaci za registraci transformace.</span><span class="sxs-lookup"><span data-stu-id="99d79-114">Templates are used to implement per-registration transformation.</span></span> <span data-ttu-id="99d79-115">Další informace najdete v tématu [šablony](notification-hubs-templates-cross-platform-push-messages.md).</span><span class="sxs-lookup"><span data-stu-id="99d79-115">For more information, see [Templates](notification-hubs-templates-cross-platform-push-messages.md).</span></span>

#### <a name="installations"></a><span data-ttu-id="99d79-116">Instalace</span><span class="sxs-lookup"><span data-stu-id="99d79-116">Installations</span></span>
<span data-ttu-id="99d79-117">Instalace je vylepšený registrace, který zahrnuje kontejner nabízené souvisejících vlastností.</span><span class="sxs-lookup"><span data-stu-id="99d79-117">An Installation is an enhanced registration that includes a bag of push related properties.</span></span> <span data-ttu-id="99d79-118">Je nejnovější a nejlepší způsob registrace zařízení.</span><span class="sxs-lookup"><span data-stu-id="99d79-118">It is the latest and best approach to registering your devices.</span></span> <span data-ttu-id="99d79-119">Však není podporován na straně klienta .NET SDK ([SDK centra oznámení pro back-end operace](https://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/)) ještě.</span><span class="sxs-lookup"><span data-stu-id="99d79-119">However, it is not supported by client side .NET SDK ([Notification Hub SDK for backend operations](https://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/)) as of yet.</span></span>  <span data-ttu-id="99d79-120">To znamená, že pokud registrujete ze samotného klientského zařízení, je třeba použít [rozhraní API REST centra oznámení](https://msdn.microsoft.com/library/mt621153.aspx) přístup k podpoře instalaci.</span><span class="sxs-lookup"><span data-stu-id="99d79-120">This means if you are registering from the client device itself, you would have to use the [Notification Hubs REST API](https://msdn.microsoft.com/library/mt621153.aspx) approach to support installations.</span></span> <span data-ttu-id="99d79-121">Pokud používáte back-end službu, byste měli použít [SDK centra oznámení pro back-end operace](https://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/).</span><span class="sxs-lookup"><span data-stu-id="99d79-121">If you are using a backend service, you should be able to use [Notification Hub SDK for backend operations](https://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/).</span></span>

<span data-ttu-id="99d79-122">Tady jsou některé klíčové výhody pomocí instalace:</span><span class="sxs-lookup"><span data-stu-id="99d79-122">The following are some key advantages to using installations:</span></span>

* <span data-ttu-id="99d79-123">Vytvoření nebo aktualizace instalace je plně idempotent.</span><span class="sxs-lookup"><span data-stu-id="99d79-123">Creating or updating an installation is fully idempotent.</span></span> <span data-ttu-id="99d79-124">Proto ji můžete zopakovat bez jakékoli pochybnostmi duplicitní registrace.</span><span class="sxs-lookup"><span data-stu-id="99d79-124">So you can retry it without any concerns about duplicate registrations.</span></span>
* <span data-ttu-id="99d79-125">Instalační model usnadňuje provést jednotlivé nabízených oznámení - cílení na konkrétní zařízení.</span><span class="sxs-lookup"><span data-stu-id="99d79-125">The installation model makes it easy to do individual pushes - targeting specific device.</span></span> <span data-ttu-id="99d79-126">Značku systému **"$InstallationId: [installationId]"** se automaticky přidá s každou instalace na základě registrace.</span><span class="sxs-lookup"><span data-stu-id="99d79-126">A system tag **"$InstallationId:[installationId]"** is automatically added with each installation based registration.</span></span> <span data-ttu-id="99d79-127">Proto můžete volat odeslání na tuto značku zaměřit na konkrétní zařízení bez nutnosti psaní Další.</span><span class="sxs-lookup"><span data-stu-id="99d79-127">So you can call a send to this tag to target a specific device without having to do any additional coding.</span></span>
* <span data-ttu-id="99d79-128">Pomocí instalace také umožňuje provést registraci částečné aktualizace.</span><span class="sxs-lookup"><span data-stu-id="99d79-128">Using installations also enables you to do partial registration updates.</span></span> <span data-ttu-id="99d79-129">Částečné aktualizace instalace se požaduje pomocí metody PATCH [JSON-Patch standard](https://tools.ietf.org/html/rfc6902).</span><span class="sxs-lookup"><span data-stu-id="99d79-129">The partial update of an installation is requested with a PATCH method using the [JSON-Patch standard](https://tools.ietf.org/html/rfc6902).</span></span> <span data-ttu-id="99d79-130">To je zvlášť užitečné, pokud chcete aktualizovat značky na registraci.</span><span class="sxs-lookup"><span data-stu-id="99d79-130">This is particularly useful when you want to update tags on the registration.</span></span> <span data-ttu-id="99d79-131">Nemáte stahují celý registrace a pak znovu odeslat všechny předchozí značky.</span><span class="sxs-lookup"><span data-stu-id="99d79-131">You don't have to pull down the entire registration and then resend all the previous tags again.</span></span>

<span data-ttu-id="99d79-132">Instalace může obsahovat následující vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="99d79-132">An installation can contain the the following properties.</span></span> <span data-ttu-id="99d79-133">Úplný seznam najdete v tématu instalace vlastnosti [vytvoření nebo instalaci přepsat REST API](https://msdn.microsoft.com/library/azure/mt621153.aspx) nebo [vlastnosti instalace](https://msdn.microsoft.com/library/azure/microsoft.azure.notificationhubs.installation_properties.aspx) pro.</span><span class="sxs-lookup"><span data-stu-id="99d79-133">For a complete listing of the installation properties see, [Create or Overwrite an Installation with REST API](https://msdn.microsoft.com/library/azure/mt621153.aspx) or [Installation Properties](https://msdn.microsoft.com/library/azure/microsoft.azure.notificationhubs.installation_properties.aspx) for the .</span></span>

    // Example installation format to show some supported properties
    {
        installationId: "",
        expirationTime: "",
        tags: [],
        platform: "",
        pushChannel: "",
        ………
        templates: {
            "templateName1" : {
                body: "",
                tags: [] },
            "templateName2" : {
                body: "",
                // Headers are for Windows Store only
                headers: {
                    "X-WNS-Type": "wns/tile" }
                tags: [] }
        },
        secondaryTiles: {
            "tileId1": {
                pushChannel: "",
                tags: [],
                templates: {
                    "otherTemplate": {
                        bodyTemplate: "",
                        headers: {
                            ... }
                        tags: [] }
                }
            }
        }
    }



<span data-ttu-id="99d79-134">Je důležité si uvědomit, že registrace a instalace ve výchozím nastavení už platnost.</span><span class="sxs-lookup"><span data-stu-id="99d79-134">It is important to note that registrations and installations by default no longer expire.</span></span>

<span data-ttu-id="99d79-135">Registrace a instalace musí obsahovat platný popisovače systému PNS pro každé zařízení nebo kanál.</span><span class="sxs-lookup"><span data-stu-id="99d79-135">Registrations and installations must contain a valid PNS handle for each device/channel.</span></span> <span data-ttu-id="99d79-136">Vzhledem k tomu, že popisovačů systému PNS se dá získat jenom v klientskou aplikaci na zařízení, je jeden vzor zaregistrovat přímo na daném zařízení s klientské aplikace.</span><span class="sxs-lookup"><span data-stu-id="99d79-136">Because PNS handles can only be obtained in a client app on the device, one pattern is to register directly on that device with the client app.</span></span> <span data-ttu-id="99d79-137">Na druhé straně aspekty zabezpečení a obchodní logiku související s značky může vyžadovat umožňuje spravovat registraci zařízení v back-end aplikace.</span><span class="sxs-lookup"><span data-stu-id="99d79-137">On the other hand, security considerations and business logic related to tags might require you to manage device registration in the app back-end.</span></span> 

#### <a name="templates"></a><span data-ttu-id="99d79-138">Šablony</span><span class="sxs-lookup"><span data-stu-id="99d79-138">Templates</span></span>
<span data-ttu-id="99d79-139">Pokud chcete použít [šablony](notification-hubs-templates-cross-platform-push-messages.md), instalace zařízení také obsahovat všechny šablony, které jsou spojené s tímto zařízením v JSON formátu (viz ukázka výše).</span><span class="sxs-lookup"><span data-stu-id="99d79-139">If you want to use [Templates](notification-hubs-templates-cross-platform-push-messages.md), the device installation also hold all templates associated with that device in a JSON format (see sample above).</span></span> <span data-ttu-id="99d79-140">Názvy šablon pomoci cíl různé šablony pro stejné zařízení.</span><span class="sxs-lookup"><span data-stu-id="99d79-140">The template names help target different templates for the same device.</span></span>

<span data-ttu-id="99d79-141">Všimněte si, že každý název šablony se mapuje na text šablony a volitelná sada značky.</span><span class="sxs-lookup"><span data-stu-id="99d79-141">Note that each template name maps to a template body and an optional set of tags.</span></span> <span data-ttu-id="99d79-142">Kromě toho každou platformu můžete mít další šablony vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="99d79-142">Moreover, each platform can have additional template properties.</span></span> <span data-ttu-id="99d79-143">Pro Windows Store (pomocí WNS) a Windows Phone 8 (pomocí MPNS) může být další sadu hlaviček součástí šablony.</span><span class="sxs-lookup"><span data-stu-id="99d79-143">For Windows Store (using WNS) and Windows Phone 8 (using MPNS), an additional set of headers can be part of the template.</span></span> <span data-ttu-id="99d79-144">V případě služby APN můžete nastavit vlastnost vypršení platnosti buď konstanta, nebo výraz šablony.</span><span class="sxs-lookup"><span data-stu-id="99d79-144">In the case of APNs, you can set an expiry property to either a constant or to a template expression.</span></span> <span data-ttu-id="99d79-145">Úplný seznam najdete v tématu instalace vlastnosti [vytvoření nebo instalaci přepsat REST](https://msdn.microsoft.com/library/azure/mt621153.aspx) tématu.</span><span class="sxs-lookup"><span data-stu-id="99d79-145">For a complete listing of the installation properties see, [Create or Overwrite an Installation with REST](https://msdn.microsoft.com/library/azure/mt621153.aspx) topic.</span></span>

#### <a name="secondary-tiles-for-windows-store-apps"></a><span data-ttu-id="99d79-146">Sekundární dlaždice pro aplikace pro Windows Store</span><span class="sxs-lookup"><span data-stu-id="99d79-146">Secondary Tiles for Windows Store Apps</span></span>
<span data-ttu-id="99d79-147">Pro klientské aplikace Windows Store odesílání oznámení do sekundární dlaždice je stejný jako je pošlete na primární.</span><span class="sxs-lookup"><span data-stu-id="99d79-147">For Windows Store client applications, sending notifications to secondary tiles is the same as sending them to the primary one.</span></span> <span data-ttu-id="99d79-148">To je podporováno také v instalaci.</span><span class="sxs-lookup"><span data-stu-id="99d79-148">This is also supported in installations.</span></span> <span data-ttu-id="99d79-149">Všimněte si, že sekundární dlaždice mají různé ChannelUri, která zpracovává sadu SDK na vaší klientské aplikace transparentně.</span><span class="sxs-lookup"><span data-stu-id="99d79-149">Note that secondary tiles have a different ChannelUri, which the SDK on your client app handles transparently.</span></span>

<span data-ttu-id="99d79-150">Slovník SecondaryTiles používá stejné TileId, který se používá k vytvoření objektu SecondaryTiles v aplikaci Windows Store.</span><span class="sxs-lookup"><span data-stu-id="99d79-150">The SecondaryTiles dictionary uses the same TileId that is used to create the SecondaryTiles object in your Windows Store app.</span></span>
<span data-ttu-id="99d79-151">Stejně jako u primární ChannelUri ChannelUris sekundární dlaždice můžete změnit v každém okamžiku.</span><span class="sxs-lookup"><span data-stu-id="99d79-151">As with the primary ChannelUri, ChannelUris of secondary tiles can change at any moment.</span></span> <span data-ttu-id="99d79-152">Chcete-li zachovat instalace portálu v centru oznámení, aktualizovat, musíte aktualizovat zařízení s aktuální ChannelUris sekundární dlaždic je.</span><span class="sxs-lookup"><span data-stu-id="99d79-152">In order to keep the installations in the notification hub updated, the device must refresh them with the current ChannelUris of the secondary tiles.</span></span>

## <a name="registration-management-from-the-device"></a><span data-ttu-id="99d79-153">Správa registraci ze zařízení</span><span class="sxs-lookup"><span data-stu-id="99d79-153">Registration management from the device</span></span>
<span data-ttu-id="99d79-154">Při správě registraci zařízení z klientské aplikace, je odpovědná za zasílání oznámení pouze back-end.</span><span class="sxs-lookup"><span data-stu-id="99d79-154">When managing device registration from client apps, the backend is only responsible for sending notifications.</span></span> <span data-ttu-id="99d79-155">Klientské aplikace průběžně aktualizovat popisovačů systému PNS a zaregistrujte značky.</span><span class="sxs-lookup"><span data-stu-id="99d79-155">Client apps keep PNS handles up to date, and register tags.</span></span> <span data-ttu-id="99d79-156">Následující obrázek znázorňuje tento vzor.</span><span class="sxs-lookup"><span data-stu-id="99d79-156">The following picture illustrates this pattern.</span></span>

![](./media/notification-hubs-registration-management/notification-hubs-registering-on-device.png)

<span data-ttu-id="99d79-157">Zařízení se nejdřív načte popisovače systému PNS z systém PNS a potom zaregistruje s centrem oznámení přímo.</span><span class="sxs-lookup"><span data-stu-id="99d79-157">The device first retrieves the PNS handle from the PNS, then registers with the notification hub directly.</span></span> <span data-ttu-id="99d79-158">Po úspěšné registraci back-end aplikace může odesílat oznámení cílení k registraci.</span><span class="sxs-lookup"><span data-stu-id="99d79-158">After the registration is successful, the app backend can send a notification targeting that registration.</span></span> <span data-ttu-id="99d79-159">Další informace o tom, jak odesílat oznámení najdete v tématu [směrování a značky výrazy](notification-hubs-tags-segment-push-message.md).</span><span class="sxs-lookup"><span data-stu-id="99d79-159">For more information about how to send notifications, see [Routing and Tag Expressions](notification-hubs-tags-segment-push-message.md).</span></span>
<span data-ttu-id="99d79-160">Všimněte si, že v takovém případě budete používat jenom naslouchat práva pro přístup k vaší centra oznámení ze zařízení.</span><span class="sxs-lookup"><span data-stu-id="99d79-160">Note that in this case, you will use only Listen rights to access your notification hubs from the device.</span></span> <span data-ttu-id="99d79-161">Další informace najdete v tématu [zabezpečení](notification-hubs-push-notification-security.md).</span><span class="sxs-lookup"><span data-stu-id="99d79-161">For more information, see [Security](notification-hubs-push-notification-security.md).</span></span>

<span data-ttu-id="99d79-162">Nejjednodušším způsobem je registraci ze zařízení, ale má některé nevýhody.</span><span class="sxs-lookup"><span data-stu-id="99d79-162">Registering from the device is the simplest method, but it has some drawbacks.</span></span>
<span data-ttu-id="99d79-163">První nevýhodou je, že klientská aplikace lze aktualizovat pouze jeho značky když je aplikace aktivní.</span><span class="sxs-lookup"><span data-stu-id="99d79-163">The first drawback is that a client app can only update its tags when the app is active.</span></span> <span data-ttu-id="99d79-164">Například pokud má uživatel dvě zařízení, které registrují značky související s sport týmy během první zařízení zaregistruje pro další značky (například Seahawks), druhé zařízení nebude dostávat oznámení o Seahawks aplikace na druhé zařízení je proveden ještě jednou.</span><span class="sxs-lookup"><span data-stu-id="99d79-164">For example, if a user has two devices that register tags related to sport teams, when the first device registers for an additional tag (for example, Seahawks), the second device will not receive the notifications about the Seahawks until the app on the second device is executed a second time.</span></span> <span data-ttu-id="99d79-165">Obecně platí když značky jsou ovlivněné více zařízení, správa značky z back-end je žádoucí možnost.</span><span class="sxs-lookup"><span data-stu-id="99d79-165">More generally, when tags are affected by multiple devices, managing tags from the backend is a desirable option.</span></span>
<span data-ttu-id="99d79-166">Druhý nevýhodou registrace správy z klientské aplikace je, že vzhledem k tomu, že aplikace může hacker, zabezpečení, aby se registrace konkrétními značkami vyžaduje zvláštní pozornost, jak je popsáno v části "zabezpečení na úrovni značky."</span><span class="sxs-lookup"><span data-stu-id="99d79-166">The second drawback of registration management from the client app is that, since apps can be hacked, securing the registration to specific tags requires extra care, as explained in the section “Tag-level security.”</span></span>

#### <a name="example-code-to-register-with-a-notification-hub-from-a-device-using-an-installation"></a><span data-ttu-id="99d79-167">Ukázkový kód pro registraci se Centrum oznámení ze zařízení pomocí instalace</span><span class="sxs-lookup"><span data-stu-id="99d79-167">Example code to register with a notification hub from a device using an installation</span></span>
<span data-ttu-id="99d79-168">V tuto chvíli je podporováno pouze pomocí [rozhraní API REST centra oznámení](https://msdn.microsoft.com/library/mt621153.aspx).</span><span class="sxs-lookup"><span data-stu-id="99d79-168">At this time, this is only supported using the [Notification Hubs REST API](https://msdn.microsoft.com/library/mt621153.aspx).</span></span>

<span data-ttu-id="99d79-169">Můžete taky pomocí metody PATCH [JSON-Patch standard](https://tools.ietf.org/html/rfc6902) pro aktualizaci instalaci.</span><span class="sxs-lookup"><span data-stu-id="99d79-169">You can also use the PATCH method using the [JSON-Patch standard](https://tools.ietf.org/html/rfc6902) for updating the installation.</span></span>

    class DeviceInstallation
    {
        public string installationId { get; set; }
        public string platform { get; set; }
        public string pushChannel { get; set; }
        public string[] tags { get; set; }
    }

    private async Task<HttpStatusCode> CreateOrUpdateInstallationAsync(DeviceInstallation deviceInstallation,
         string hubName, string listenConnectionString)
    {
        if (deviceInstallation.installationId == null)
            return HttpStatusCode.BadRequest;

        // Parse connection string (https://msdn.microsoft.com/library/azure/dn495627.aspx)
        ConnectionStringUtility connectionSaSUtil = new ConnectionStringUtility(listenConnectionString);
        string hubResource = "installations/" + deviceInstallation.installationId + "?";
        string apiVersion = "api-version=2015-04";

        // Determine the targetUri that we will sign
        string uri = connectionSaSUtil.Endpoint + hubName + "/" + hubResource + apiVersion;

        //=== Generate SaS Security Token for Authorization header ===
        // See, https://msdn.microsoft.com/library/azure/dn495627.aspx
        string SasToken = connectionSaSUtil.getSaSToken(uri, 60);

        using (var httpClient = new HttpClient())
        {
            string json = JsonConvert.SerializeObject(deviceInstallation);

            httpClient.DefaultRequestHeaders.Add("Authorization", SasToken);

            var response = await httpClient.PutAsync(uri, new StringContent(json, System.Text.Encoding.UTF8, "application/json"));
            return response.StatusCode;
        }
    }

    var channel = await PushNotificationChannelManager.CreatePushNotificationChannelForApplicationAsync();

    string installationId = null;
    var settings = ApplicationData.Current.LocalSettings.Values;

    // If we have not stored a installation id in application data, create and store as application data.
    if (!settings.ContainsKey("__NHInstallationId"))
    {
        installationId = Guid.NewGuid().ToString();
        settings.Add("__NHInstallationId", installationId);
    }

    installationId = (string)settings["__NHInstallationId"];

    var deviceInstallation = new DeviceInstallation
    {
        installationId = installationId,
        platform = "wns",
        pushChannel = channel.Uri,
        //tags = tags.ToArray<string>()
    };

    var statusCode = await CreateOrUpdateInstallationAsync(deviceInstallation, 
                        "<HUBNAME>", "<SHARED LISTEN CONNECTION STRING>");

    if (statusCode != HttpStatusCode.Accepted)
    {
        var dialog = new MessageDialog(statusCode.ToString(), "Registration failed. Installation Id : " + installationId);
        dialog.Commands.Add(new UICommand("OK"));
        await dialog.ShowAsync();
    }
    else
    {
        var dialog = new MessageDialog("Registration successful using installation Id : " + installationId);
        dialog.Commands.Add(new UICommand("OK"));
        await dialog.ShowAsync();
    }



#### <a name="example-code-to-register-with-a-notification-hub-from-a-device-using-a-registration"></a><span data-ttu-id="99d79-170">Ukázkový kód pro registraci se Centrum oznámení ze zařízení pomocí registrace</span><span class="sxs-lookup"><span data-stu-id="99d79-170">Example code to register with a notification hub from a device using a registration</span></span>
<span data-ttu-id="99d79-171">Tyto metody vytvořit nebo aktualizovat registrace pro zařízení, na které se nazývají.</span><span class="sxs-lookup"><span data-stu-id="99d79-171">These methods create or update a registration for the device on which they are called.</span></span> <span data-ttu-id="99d79-172">To znamená, musí přepsat za účelem aktualizace popisovač nebo značky, celý registrace.</span><span class="sxs-lookup"><span data-stu-id="99d79-172">This means that in order to update the handle or the tags, you must overwrite the entire registration.</span></span> <span data-ttu-id="99d79-173">Mějte na paměti, že jsou registrace přechodný, a proto byste měli mít vždy spolehlivé úložiště s aktuální značky, které potřebuje určité zařízení.</span><span class="sxs-lookup"><span data-stu-id="99d79-173">Remember that registrations are transient, so you should always have a reliable store with the current tags that a specific device needs.</span></span>

    // Initialize the Notification Hub
    NotificationHubClient hub = NotificationHubClient.CreateClientFromConnectionString(listenConnString, hubName);

    // The Device id from the PNS
    var pushChannel = await PushNotificationChannelManager.CreatePushNotificationChannelForApplicationAsync();

    // If you are registering from the client itself, then store this registration id in device
    // storage. Then when the app starts, you can check if a registration id already exists or not before
    // creating.
    var settings = ApplicationData.Current.LocalSettings.Values;

    // If we have not stored a registration id in application data, store in application data.
    if (!settings.ContainsKey("__NHRegistrationId"))
    {
        // make sure there are no existing registrations for this push handle (used for iOS and Android)    
        string newRegistrationId = null;
        var registrations = await hub.GetRegistrationsByChannelAsync(pushChannel.Uri, 100);
        foreach (RegistrationDescription registration in registrations)
        {
            if (newRegistrationId == null)
            {
                newRegistrationId = registration.RegistrationId;
            }
            else
            {
                await hub.DeleteRegistrationAsync(registration);
            }
        }

        newRegistrationId = await hub.CreateRegistrationIdAsync();

        settings.Add("__NHRegistrationId", newRegistrationId);
    }

    string regId = (string)settings["__NHRegistrationId"];

    RegistrationDescription registration = new WindowsRegistrationDescription(pushChannel.Uri);
    registration.RegistrationId = regId;
    registration.Tags = new HashSet<string>(YourTags);

    try
    {
        await hub.CreateOrUpdateRegistrationAsync(registration);
    }
    catch (Microsoft.WindowsAzure.Messaging.RegistrationGoneException e)
    {
        settings.Remove("__NHRegistrationId");
    }


## <a name="registration-management-from-a-backend"></a><span data-ttu-id="99d79-174">Registrace správy z back-end</span><span class="sxs-lookup"><span data-stu-id="99d79-174">Registration management from a backend</span></span>
<span data-ttu-id="99d79-175">Správa registrace z back-end vyžaduje zápis další kód.</span><span class="sxs-lookup"><span data-stu-id="99d79-175">Managing registrations from the backend requires writing additional code.</span></span> <span data-ttu-id="99d79-176">Aplikace ze zařízení musíte zadat zpracovat aktualizovaný systém PNS a back-end při každém spuštění aplikace (společně se značkami a šablony) a back-end musíte aktualizovat tento popisovač v centru oznámení.</span><span class="sxs-lookup"><span data-stu-id="99d79-176">The app from the device must provide the updated PNS handle to the backend every time the app starts (along with tags and templates), and the backend must update this handle on the notification hub.</span></span> <span data-ttu-id="99d79-177">Následující obrázek znázorňuje tento návrh.</span><span class="sxs-lookup"><span data-stu-id="99d79-177">The following picture illustrates this design.</span></span>

![](./media/notification-hubs-registration-management/notification-hubs-registering-on-backend.png)

<span data-ttu-id="99d79-178">Výhody správy registrace z back-end, zahrnují možnost Upravit značky na registrací i v případě, že odpovídající aplikace na zařízení je neaktivní a k ověření klientské aplikace před přidáním značky k jeho registraci.</span><span class="sxs-lookup"><span data-stu-id="99d79-178">The advantages of managing registrations from the backend include the ability to modify tags to registrations even when the corresponding app on the device is inactive, and to authenticate the client app before adding a tag to its registration.</span></span>

#### <a name="example-code-to-register-with-a-notification-hub-from-a-backend-using-an-installation"></a><span data-ttu-id="99d79-179">Ukázkový kód pro registraci se centra oznámení z back-end pomocí instalace</span><span class="sxs-lookup"><span data-stu-id="99d79-179">Example code to register with a notification hub from a backend using an installation</span></span>
<span data-ttu-id="99d79-180">Klientské zařízení stále získá jeho popisovače systému PNS a vlastnosti relevantní instalace jako před a volání vlastní rozhraní API na back-end, které můžete provést registraci a autorizaci značky atd. Můžete využít back-end [SDK centra oznámení pro back-end operace](https://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/).</span><span class="sxs-lookup"><span data-stu-id="99d79-180">The client device still gets its PNS handle and relevant installation properties as before and calls a custom API on the backend that can perform the registration and authorize tags etc. The backend can leverage the [Notification Hub SDK for backend operations](https://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/).</span></span>

<span data-ttu-id="99d79-181">Můžete taky pomocí metody PATCH [JSON-Patch standard](https://tools.ietf.org/html/rfc6902) pro aktualizaci instalaci.</span><span class="sxs-lookup"><span data-stu-id="99d79-181">You can also use the PATCH method using the [JSON-Patch standard](https://tools.ietf.org/html/rfc6902) for updating the installation.</span></span>

    // Initialize the Notification Hub
    NotificationHubClient hub = NotificationHubClient.CreateClientFromConnectionString(listenConnString, hubName);

    // Custom API on the backend
    public async Task<HttpResponseMessage> Put(DeviceInstallation deviceUpdate)
    {

        Installation installation = new Installation();
        installation.InstallationId = deviceUpdate.InstallationId;
        installation.PushChannel = deviceUpdate.Handle;
        installation.Tags = deviceUpdate.Tags;

        switch (deviceUpdate.Platform)
        {
            case "mpns":
                installation.Platform = NotificationPlatform.Mpns;
                break;
            case "wns":
                installation.Platform = NotificationPlatform.Wns;
                break;
            case "apns":
                installation.Platform = NotificationPlatform.Apns;
                break;
            case "gcm":
                installation.Platform = NotificationPlatform.Gcm;
                break;
            default:
                throw new HttpResponseException(HttpStatusCode.BadRequest);
        }


        // In the backend we can control if a user is allowed to add tags
        //installation.Tags = new List<string>(deviceUpdate.Tags);
        //installation.Tags.Add("username:" + username);

        await hub.CreateOrUpdateInstallationAsync(installation);

        return Request.CreateResponse(HttpStatusCode.OK);
    }


#### <a name="example-code-to-register-with-a-notification-hub-from-a-device-using-a-registration-id"></a><span data-ttu-id="99d79-182">Ukázkový kód pro registraci se Centrum oznámení ze zařízení pomocí id registrace</span><span class="sxs-lookup"><span data-stu-id="99d79-182">Example code to register with a notification hub from a device using a registration id</span></span>
<span data-ttu-id="99d79-183">Z vašeho back-end aplikace můžete provádět základní operace CRUDS při registraci.</span><span class="sxs-lookup"><span data-stu-id="99d79-183">From your app backend, you can perform basic CRUDS operations on registrations.</span></span> <span data-ttu-id="99d79-184">Například:</span><span class="sxs-lookup"><span data-stu-id="99d79-184">For example:</span></span>

    var hub = NotificationHubClient.CreateClientFromConnectionString("{connectionString}", "hubName");

    // create a registration description object of the correct type, e.g.
    var reg = new WindowsRegistrationDescription(channelUri, tags);

    // Create
    await hub.CreateRegistrationAsync(reg);

    // Get by id
    var r = await hub.GetRegistrationAsync<RegistrationDescription>("id");

    // update
    r.Tags.Add("myTag");

    // update on hub
    await hub.UpdateRegistrationAsync(r);

    // delete
    await hub.DeleteRegistrationAsync(r);


<span data-ttu-id="99d79-185">Back-end musí zpracovávat souběžnosti mezi aktualizace registrace.</span><span class="sxs-lookup"><span data-stu-id="99d79-185">The backend must handle concurrency between registration updates.</span></span> <span data-ttu-id="99d79-186">Service Bus nabízí optimistické řízení souběžného pro správu registrace.</span><span class="sxs-lookup"><span data-stu-id="99d79-186">Service Bus offers optimistic concurrency control for registration management.</span></span> <span data-ttu-id="99d79-187">Na úrovni protokolu HTTP tato možnost je implementovaná pomocí použití značky ETag na operace správy registrace.</span><span class="sxs-lookup"><span data-stu-id="99d79-187">At the HTTP level, this is implemented with the use of ETag on registration management operations.</span></span> <span data-ttu-id="99d79-188">Tato funkce slouží transparentně SDKs Microsoft, která je vyvolána výjimka, pokud aktualizace byl odmítnut z důvodů souběžnosti.</span><span class="sxs-lookup"><span data-stu-id="99d79-188">This feature is transparently used by Microsoft SDKs, which throw an exception if an update is rejected for concurrency reasons.</span></span> <span data-ttu-id="99d79-189">Back-end aplikace je zodpovědná za zpracování těchto výjimek a v případě potřeby opakováním aktualizace.</span><span class="sxs-lookup"><span data-stu-id="99d79-189">The app backend is responsible for handling these exceptions and retrying the update if required.</span></span>

