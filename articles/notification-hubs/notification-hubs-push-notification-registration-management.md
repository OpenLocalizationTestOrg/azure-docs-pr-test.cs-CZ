---
title: "aaaRegistration správy"
description: "Toto téma vysvětluje, jak zařízení tooregister pomocí centra oznámení v pořadí tooreceive nabízená oznámení."
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
ms.openlocfilehash: 76471a45c7a0da1614ceed82b73cdb3319979ff7
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="registration-management"></a><span data-ttu-id="c1550-103">Správa registrací</span><span class="sxs-lookup"><span data-stu-id="c1550-103">Registration management</span></span>
## <a name="overview"></a><span data-ttu-id="c1550-104">Přehled</span><span class="sxs-lookup"><span data-stu-id="c1550-104">Overview</span></span>
<span data-ttu-id="c1550-105">Toto téma vysvětluje, jak zařízení tooregister pomocí centra oznámení v pořadí tooreceive nabízená oznámení.</span><span class="sxs-lookup"><span data-stu-id="c1550-105">This topic explains how tooregister devices with notification hubs in order tooreceive push notifications.</span></span> <span data-ttu-id="c1550-106">Hello téma popisuje registrace na vysoké úrovni a potom zavádí hello dva hlavní vzory pro registraci zařízení: registrace z hello zařízení přímo toohello centra oznámení a registraci prostřednictvím back-end aplikace.</span><span class="sxs-lookup"><span data-stu-id="c1550-106">hello topic describes registrations at a high level, then introduces hello two main patterns for registering devices: registering from hello device directly toohello notification hub, and registering through an application backend.</span></span> 

## <a name="what-is-device-registration"></a><span data-ttu-id="c1550-107">Co je registrace zařízení</span><span class="sxs-lookup"><span data-stu-id="c1550-107">What is device registration</span></span>
<span data-ttu-id="c1550-108">Registrace zařízení s centrem oznámení se provádí pomocí **registrace** nebo **instalace**.</span><span class="sxs-lookup"><span data-stu-id="c1550-108">Device registration with a Notification Hub is accomplished using a **Registration** or **Installation**.</span></span>

#### <a name="registrations"></a><span data-ttu-id="c1550-109">Registrace</span><span class="sxs-lookup"><span data-stu-id="c1550-109">Registrations</span></span>
<span data-ttu-id="c1550-110">Registrace přidruží hello zpracovat oznámení služby platformy (PNS) pro zařízení s značky a případně šablonu.</span><span class="sxs-lookup"><span data-stu-id="c1550-110">A registration associates hello Platform Notification Service (PNS) handle for a device with tags and possibly a template.</span></span> <span data-ttu-id="c1550-111">popisovače systému PNS Hello může být ChannelURI, token zařízení nebo id registrace GCM. Značky jsou použité tooroute oznámení toohello správná sada popisovačů zařízení.</span><span class="sxs-lookup"><span data-stu-id="c1550-111">hello PNS handle could be a ChannelURI, device token, or GCM registration id. Tags are used tooroute notifications toohello correct set of device handles.</span></span> <span data-ttu-id="c1550-112">Další informace najdete v tématu [směrování a značky výrazy](notification-hubs-tags-segment-push-message.md).</span><span class="sxs-lookup"><span data-stu-id="c1550-112">For more information, see [Routing and Tag Expressions](notification-hubs-tags-segment-push-message.md).</span></span> <span data-ttu-id="c1550-113">Šablony jsou použité tooimplement za registraci transformace.</span><span class="sxs-lookup"><span data-stu-id="c1550-113">Templates are used tooimplement per-registration transformation.</span></span> <span data-ttu-id="c1550-114">Další informace najdete v tématu [šablony](notification-hubs-templates-cross-platform-push-messages.md).</span><span class="sxs-lookup"><span data-stu-id="c1550-114">For more information, see [Templates](notification-hubs-templates-cross-platform-push-messages.md).</span></span>

#### <a name="installations"></a><span data-ttu-id="c1550-115">Instalace</span><span class="sxs-lookup"><span data-stu-id="c1550-115">Installations</span></span>
<span data-ttu-id="c1550-116">Instalace je vylepšený registrace, který zahrnuje kontejner nabízené souvisejících vlastností.</span><span class="sxs-lookup"><span data-stu-id="c1550-116">An Installation is an enhanced registration that includes a bag of push related properties.</span></span> <span data-ttu-id="c1550-117">Je hello tooregistering nejnovější a nejlepší přístup zařízení.</span><span class="sxs-lookup"><span data-stu-id="c1550-117">It is hello latest and best approach tooregistering your devices.</span></span> <span data-ttu-id="c1550-118">Však není podporován na straně klienta .NET SDK ([SDK centra oznámení pro back-end operace](https://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/)) ještě.</span><span class="sxs-lookup"><span data-stu-id="c1550-118">However, it is not supported by client side .NET SDK ([Notification Hub SDK for backend operations](https://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/)) as of yet.</span></span>  <span data-ttu-id="c1550-119">To znamená, že pokud se registrujete ze samotného klientského zařízení hello, byste měli toouse hello [rozhraní API REST centra oznámení](https://msdn.microsoft.com/library/mt621153.aspx) přístupu toosupport instalace.</span><span class="sxs-lookup"><span data-stu-id="c1550-119">This means if you are registering from hello client device itself, you would have toouse hello [Notification Hubs REST API](https://msdn.microsoft.com/library/mt621153.aspx) approach toosupport installations.</span></span> <span data-ttu-id="c1550-120">Pokud používáte back-end službu, byste měli mít toouse [SDK centra oznámení pro back-end operace](https://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/).</span><span class="sxs-lookup"><span data-stu-id="c1550-120">If you are using a backend service, you should be able toouse [Notification Hub SDK for backend operations](https://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/).</span></span>

<span data-ttu-id="c1550-121">Hello Následují některé instalace toousing klíčových výhod:</span><span class="sxs-lookup"><span data-stu-id="c1550-121">hello following are some key advantages toousing installations:</span></span>

* <span data-ttu-id="c1550-122">Vytvoření nebo aktualizace instalace je plně idempotent.</span><span class="sxs-lookup"><span data-stu-id="c1550-122">Creating or updating an installation is fully idempotent.</span></span> <span data-ttu-id="c1550-123">Proto ji můžete zopakovat bez jakékoli pochybnostmi duplicitní registrace.</span><span class="sxs-lookup"><span data-stu-id="c1550-123">So you can retry it without any concerns about duplicate registrations.</span></span>
* <span data-ttu-id="c1550-124">Instalační model Hello umožňuje snadno toodo jednotlivých nabízených oznámení - cílení na konkrétní zařízení.</span><span class="sxs-lookup"><span data-stu-id="c1550-124">hello installation model makes it easy toodo individual pushes - targeting specific device.</span></span> <span data-ttu-id="c1550-125">Značku systému **"$InstallationId: [installationId]"** se automaticky přidá s každou instalace na základě registrace.</span><span class="sxs-lookup"><span data-stu-id="c1550-125">A system tag **"$InstallationId:[installationId]"** is automatically added with each installation based registration.</span></span> <span data-ttu-id="c1550-126">Proto můžete volat odeslání toothis značky tootarget určité zařízení bez nutnosti toodo další kódování.</span><span class="sxs-lookup"><span data-stu-id="c1550-126">So you can call a send toothis tag tootarget a specific device without having toodo any additional coding.</span></span>
* <span data-ttu-id="c1550-127">Použití instalace umožňuje také můžete toodo registrace částečné aktualizace.</span><span class="sxs-lookup"><span data-stu-id="c1550-127">Using installations also enables you toodo partial registration updates.</span></span> <span data-ttu-id="c1550-128">Hello částečné aktualizace instalace se požaduje pomocí metody PATCH pomocí hello [JSON-Patch standard](https://tools.ietf.org/html/rfc6902).</span><span class="sxs-lookup"><span data-stu-id="c1550-128">hello partial update of an installation is requested with a PATCH method using hello [JSON-Patch standard](https://tools.ietf.org/html/rfc6902).</span></span> <span data-ttu-id="c1550-129">To je zvlášť užitečné, když chcete, aby tooupdate značky na hello registrace.</span><span class="sxs-lookup"><span data-stu-id="c1550-129">This is particularly useful when you want tooupdate tags on hello registration.</span></span> <span data-ttu-id="c1550-130">Nemáte toopull dolů hello celý registrace a pak znovu odeslat všechny předchozí značky hello.</span><span class="sxs-lookup"><span data-stu-id="c1550-130">You don't have toopull down hello entire registration and then resend all hello previous tags again.</span></span>

<span data-ttu-id="c1550-131">Instalace může obsahovat hello hello následující vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="c1550-131">An installation can contain hello hello following properties.</span></span> <span data-ttu-id="c1550-132">Úplný seznam najdete vlastnosti hello instalace [vytvoření nebo instalaci přepsat REST API](https://msdn.microsoft.com/library/azure/mt621153.aspx) nebo [vlastnosti instalace](https://msdn.microsoft.com/library/azure/microsoft.azure.notificationhubs.installation_properties.aspx) pro hello.</span><span class="sxs-lookup"><span data-stu-id="c1550-132">For a complete listing of hello installation properties see, [Create or Overwrite an Installation with REST API](https://msdn.microsoft.com/library/azure/mt621153.aspx) or [Installation Properties](https://msdn.microsoft.com/library/azure/microsoft.azure.notificationhubs.installation_properties.aspx) for hello .</span></span>

    // Example installation format tooshow some supported properties
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



<span data-ttu-id="c1550-133">Je důležité toonote, který už platnost registrace a instalace ve výchozím nastavení.</span><span class="sxs-lookup"><span data-stu-id="c1550-133">It is important toonote that registrations and installations by default no longer expire.</span></span>

<span data-ttu-id="c1550-134">Registrace a instalace musí obsahovat platný popisovače systému PNS pro každé zařízení nebo kanál.</span><span class="sxs-lookup"><span data-stu-id="c1550-134">Registrations and installations must contain a valid PNS handle for each device/channel.</span></span> <span data-ttu-id="c1550-135">Protože popisovačů systému PNS se dá získat jenom v aplikaci klienta na hello zařízení, je jeden vzor tooregister přímo na daném zařízení s hello klientskou aplikaci.</span><span class="sxs-lookup"><span data-stu-id="c1550-135">Because PNS handles can only be obtained in a client app on hello device, one pattern is tooregister directly on that device with hello client app.</span></span> <span data-ttu-id="c1550-136">Na hello jiné straně, důležité informace o zabezpečení a obchodní logiku související s tootags může vyžadovat toomanage registrace zařízení v back-end aplikace hello.</span><span class="sxs-lookup"><span data-stu-id="c1550-136">On hello other hand, security considerations and business logic related tootags might require you toomanage device registration in hello app back-end.</span></span> 

#### <a name="templates"></a><span data-ttu-id="c1550-137">Šablony</span><span class="sxs-lookup"><span data-stu-id="c1550-137">Templates</span></span>
<span data-ttu-id="c1550-138">Pokud chcete, aby toouse [šablony](notification-hubs-templates-cross-platform-push-messages.md), instalace zařízení hello také obsahovat všechny šablony, které jsou spojené s tímto zařízením v JSON formátu (viz ukázka výše).</span><span class="sxs-lookup"><span data-stu-id="c1550-138">If you want toouse [Templates](notification-hubs-templates-cross-platform-push-messages.md), hello device installation also hold all templates associated with that device in a JSON format (see sample above).</span></span> <span data-ttu-id="c1550-139">Hello názvy šablon pomůže cíle různé šablony pro hello stejné zařízení.</span><span class="sxs-lookup"><span data-stu-id="c1550-139">hello template names help target different templates for hello same device.</span></span>

<span data-ttu-id="c1550-140">Všimněte si, že každý název šablony mapuje tooa šablony textu a volitelná sada značky.</span><span class="sxs-lookup"><span data-stu-id="c1550-140">Note that each template name maps tooa template body and an optional set of tags.</span></span> <span data-ttu-id="c1550-141">Kromě toho každou platformu můžete mít další šablony vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="c1550-141">Moreover, each platform can have additional template properties.</span></span> <span data-ttu-id="c1550-142">Pro Windows Store (pomocí WNS) a Windows Phone 8 (pomocí MPNS) může být další sadu hlaviček součástí hello šablony.</span><span class="sxs-lookup"><span data-stu-id="c1550-142">For Windows Store (using WNS) and Windows Phone 8 (using MPNS), an additional set of headers can be part of hello template.</span></span> <span data-ttu-id="c1550-143">V případě hello služby APN můžete nastavit vlastnost tooeither vypršení platnosti konstanta nebo tooa výraz šablony.</span><span class="sxs-lookup"><span data-stu-id="c1550-143">In hello case of APNs, you can set an expiry property tooeither a constant or tooa template expression.</span></span> <span data-ttu-id="c1550-144">Úplný seznam najdete vlastnosti hello instalace [vytvoření nebo instalaci přepsat REST](https://msdn.microsoft.com/library/azure/mt621153.aspx) tématu.</span><span class="sxs-lookup"><span data-stu-id="c1550-144">For a complete listing of hello installation properties see, [Create or Overwrite an Installation with REST](https://msdn.microsoft.com/library/azure/mt621153.aspx) topic.</span></span>

#### <a name="secondary-tiles-for-windows-store-apps"></a><span data-ttu-id="c1550-145">Sekundární dlaždice pro aplikace pro Windows Store</span><span class="sxs-lookup"><span data-stu-id="c1550-145">Secondary Tiles for Windows Store Apps</span></span>
<span data-ttu-id="c1550-146">Pro klientské aplikace Windows Store hello odesílání oznámení dlaždice toosecondary je stejný jako odesláním toohello primární.</span><span class="sxs-lookup"><span data-stu-id="c1550-146">For Windows Store client applications, sending notifications toosecondary tiles is hello same as sending them toohello primary one.</span></span> <span data-ttu-id="c1550-147">To je podporováno také v instalaci.</span><span class="sxs-lookup"><span data-stu-id="c1550-147">This is also supported in installations.</span></span> <span data-ttu-id="c1550-148">Všimněte si, že sekundární dlaždice mají různé ChannelUri, které hello SDK na vaší klientské aplikace zpracovává transparentně.</span><span class="sxs-lookup"><span data-stu-id="c1550-148">Note that secondary tiles have a different ChannelUri, which hello SDK on your client app handles transparently.</span></span>

<span data-ttu-id="c1550-149">používá slovník SecondaryTiles Hello hello stejné TileId, který je použité toocreate hello SecondaryTiles objekt v aplikaci Windows Store.</span><span class="sxs-lookup"><span data-stu-id="c1550-149">hello SecondaryTiles dictionary uses hello same TileId that is used toocreate hello SecondaryTiles object in your Windows Store app.</span></span>
<span data-ttu-id="c1550-150">Jako s hello primární ChannelUri, ChannelUris sekundární dlaždice můžete změnit v každém okamžiku.</span><span class="sxs-lookup"><span data-stu-id="c1550-150">As with hello primary ChannelUri, ChannelUris of secondary tiles can change at any moment.</span></span> <span data-ttu-id="c1550-151">V pořadí tookeep hello instalace v centru oznámení hello aktualizovat hello zařízení musíte aktualizovat je s hello aktuální ChannelUris hello sekundární dlaždice.</span><span class="sxs-lookup"><span data-stu-id="c1550-151">In order tookeep hello installations in hello notification hub updated, hello device must refresh them with hello current ChannelUris of hello secondary tiles.</span></span>

## <a name="registration-management-from-hello-device"></a><span data-ttu-id="c1550-152">Správa registraci ze zařízení hello</span><span class="sxs-lookup"><span data-stu-id="c1550-152">Registration management from hello device</span></span>
<span data-ttu-id="c1550-153">Při správě registraci zařízení z klientské aplikace, je odpovědná za zasílání oznámení pouze hello back-end.</span><span class="sxs-lookup"><span data-stu-id="c1550-153">When managing device registration from client apps, hello backend is only responsible for sending notifications.</span></span> <span data-ttu-id="c1550-154">Klientské aplikace udržovat popisovačů systému PNS až toodate a zaregistrujte značky.</span><span class="sxs-lookup"><span data-stu-id="c1550-154">Client apps keep PNS handles up toodate, and register tags.</span></span> <span data-ttu-id="c1550-155">Hello následující obrázek znázorňuje tento vzor.</span><span class="sxs-lookup"><span data-stu-id="c1550-155">hello following picture illustrates this pattern.</span></span>

![](./media/notification-hubs-registration-management/notification-hubs-registering-on-device.png)

<span data-ttu-id="c1550-156">Hello zařízení nejprve načte hello systém PNS zpracování z hello Správou, a poté zaregistruje pomocí centra oznámení hello přímo.</span><span class="sxs-lookup"><span data-stu-id="c1550-156">hello device first retrieves hello PNS handle from hello PNS, then registers with hello notification hub directly.</span></span> <span data-ttu-id="c1550-157">Po úspěšné registraci hello back-end aplikace hello můžete poslat oznámení cílení k registraci.</span><span class="sxs-lookup"><span data-stu-id="c1550-157">After hello registration is successful, hello app backend can send a notification targeting that registration.</span></span> <span data-ttu-id="c1550-158">Další informace o tom, najdete v části toosend oznámení [směrování a značky výrazy](notification-hubs-tags-segment-push-message.md).</span><span class="sxs-lookup"><span data-stu-id="c1550-158">For more information about how toosend notifications, see [Routing and Tag Expressions](notification-hubs-tags-segment-push-message.md).</span></span>
<span data-ttu-id="c1550-159">Všimněte si, že v takovém případě budete používat jenom naslouchat práva tooaccess vašeho centra oznámení z hello zařízení.</span><span class="sxs-lookup"><span data-stu-id="c1550-159">Note that in this case, you will use only Listen rights tooaccess your notification hubs from hello device.</span></span> <span data-ttu-id="c1550-160">Další informace najdete v tématu [zabezpečení](notification-hubs-push-notification-security.md).</span><span class="sxs-lookup"><span data-stu-id="c1550-160">For more information, see [Security](notification-hubs-push-notification-security.md).</span></span>

<span data-ttu-id="c1550-161">Registrace zařízení hello je hello Nejjednodušším způsobem, ale má některé nevýhody.</span><span class="sxs-lookup"><span data-stu-id="c1550-161">Registering from hello device is hello simplest method, but it has some drawbacks.</span></span>
<span data-ttu-id="c1550-162">Hello první nevýhodou je, že klientská aplikace lze aktualizovat pouze jeho značky když aplikace hello je aktivní.</span><span class="sxs-lookup"><span data-stu-id="c1550-162">hello first drawback is that a client app can only update its tags when hello app is active.</span></span> <span data-ttu-id="c1550-163">Například pokud má uživatel dvě zařízení, které registrují týmy související toosport značky, pokud první zařízení hello zaregistruje pro další značky (například Seahawks), hello druhé zařízení obdrží hello upozornění o hello Seahawks až po aplikace hello na hello druhé zařízení je provést ještě jednou.</span><span class="sxs-lookup"><span data-stu-id="c1550-163">For example, if a user has two devices that register tags related toosport teams, when hello first device registers for an additional tag (for example, Seahawks), hello second device will not receive hello notifications about hello Seahawks until hello app on hello second device is executed a second time.</span></span> <span data-ttu-id="c1550-164">Obecně platí když značky jsou ovlivněné více zařízení, správa značky z back-end hello je žádoucí možnost.</span><span class="sxs-lookup"><span data-stu-id="c1550-164">More generally, when tags are affected by multiple devices, managing tags from hello backend is a desirable option.</span></span>
<span data-ttu-id="c1550-165">druhý nevýhodou Hello registrace správy z klienta aplikace hello je, že vzhledem k tomu, že aplikace může hacker, zabezpečení hello registrace toospecific značky vyžaduje zvláštní pozornost, jak je popsáno v části hello "zabezpečení na úrovni značky."</span><span class="sxs-lookup"><span data-stu-id="c1550-165">hello second drawback of registration management from hello client app is that, since apps can be hacked, securing hello registration toospecific tags requires extra care, as explained in hello section “Tag-level security.”</span></span>

#### <a name="example-code-tooregister-with-a-notification-hub-from-a-device-using-an-installation"></a><span data-ttu-id="c1550-166">Příklad kódu tooregister centra oznámení ze zařízení pomocí instalace</span><span class="sxs-lookup"><span data-stu-id="c1550-166">Example code tooregister with a notification hub from a device using an installation</span></span>
<span data-ttu-id="c1550-167">V tuto chvíli je podporováno pouze pomocí hello [rozhraní API REST centra oznámení](https://msdn.microsoft.com/library/mt621153.aspx).</span><span class="sxs-lookup"><span data-stu-id="c1550-167">At this time, this is only supported using hello [Notification Hubs REST API](https://msdn.microsoft.com/library/mt621153.aspx).</span></span>

<span data-ttu-id="c1550-168">Můžete také použít metodu PATCH hello pomocí hello [JSON-Patch standard](https://tools.ietf.org/html/rfc6902) aktualizace hello instalace.</span><span class="sxs-lookup"><span data-stu-id="c1550-168">You can also use hello PATCH method using hello [JSON-Patch standard](https://tools.ietf.org/html/rfc6902) for updating hello installation.</span></span>

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

        // Determine hello targetUri that we will sign
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



#### <a name="example-code-tooregister-with-a-notification-hub-from-a-device-using-a-registration"></a><span data-ttu-id="c1550-169">Příklad kódu tooregister centra oznámení z pomocí registrace zařízení</span><span class="sxs-lookup"><span data-stu-id="c1550-169">Example code tooregister with a notification hub from a device using a registration</span></span>
<span data-ttu-id="c1550-170">Tyto metody vytvořit nebo aktualizovat registraci pro hello zařízení, na které se nazývají.</span><span class="sxs-lookup"><span data-stu-id="c1550-170">These methods create or update a registration for hello device on which they are called.</span></span> <span data-ttu-id="c1550-171">To znamená, musí přepsat v pořadí tooupdate hello popisovač nebo hello značek, hello celý registrace.</span><span class="sxs-lookup"><span data-stu-id="c1550-171">This means that in order tooupdate hello handle or hello tags, you must overwrite hello entire registration.</span></span> <span data-ttu-id="c1550-172">Mějte na paměti, že jsou registrace přechodný, a proto byste měli mít vždy spolehlivé úložiště s hello aktuální značky, které potřebuje určité zařízení.</span><span class="sxs-lookup"><span data-stu-id="c1550-172">Remember that registrations are transient, so you should always have a reliable store with hello current tags that a specific device needs.</span></span>

    // Initialize hello Notification Hub
    NotificationHubClient hub = NotificationHubClient.CreateClientFromConnectionString(listenConnString, hubName);

    // hello Device id from hello PNS
    var pushChannel = await PushNotificationChannelManager.CreatePushNotificationChannelForApplicationAsync();

    // If you are registering from hello client itself, then store this registration id in device
    // storage. Then when hello app starts, you can check if a registration id already exists or not before
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


## <a name="registration-management-from-a-backend"></a><span data-ttu-id="c1550-173">Registrace správy z back-end</span><span class="sxs-lookup"><span data-stu-id="c1550-173">Registration management from a backend</span></span>
<span data-ttu-id="c1550-174">Správa registrace z back-end hello vyžaduje zápis další kód.</span><span class="sxs-lookup"><span data-stu-id="c1550-174">Managing registrations from hello backend requires writing additional code.</span></span> <span data-ttu-id="c1550-175">aplikace Hello z hello zařízení musíte zadat při každém spuštění aplikace hello (společně se značkami a šablon) hello aktualizovaný systém PNS popisovač toohello back-end a back-end hello musíte aktualizovat tento popisovač v centru oznámení hello.</span><span class="sxs-lookup"><span data-stu-id="c1550-175">hello app from hello device must provide hello updated PNS handle toohello backend every time hello app starts (along with tags and templates), and hello backend must update this handle on hello notification hub.</span></span> <span data-ttu-id="c1550-176">Hello následující obrázek znázorňuje tento návrh.</span><span class="sxs-lookup"><span data-stu-id="c1550-176">hello following picture illustrates this design.</span></span>

![](./media/notification-hubs-registration-management/notification-hubs-registering-on-backend.png)

<span data-ttu-id="c1550-177">Hello výhody správy registrace z back-end hello zahrnují hello možnost toomodify značky tooregistrations i v případě, že hello odpovídající aplikace na zařízení hello je neaktivní a tooauthenticate hello klientské aplikace před přidáním registrace tooits značky.</span><span class="sxs-lookup"><span data-stu-id="c1550-177">hello advantages of managing registrations from hello backend include hello ability toomodify tags tooregistrations even when hello corresponding app on hello device is inactive, and tooauthenticate hello client app before adding a tag tooits registration.</span></span>

#### <a name="example-code-tooregister-with-a-notification-hub-from-a-backend-using-an-installation"></a><span data-ttu-id="c1550-178">Příklad kódu tooregister centra oznámení z back-end pomocí instalace</span><span class="sxs-lookup"><span data-stu-id="c1550-178">Example code tooregister with a notification hub from a backend using an installation</span></span>
<span data-ttu-id="c1550-179">Hello klientské zařízení stále získá jeho popisovače systému PNS a vlastností instalace relevantní jako dříve a volání vlastní rozhraní API na back-end hello, které můžete provést registraci hello a autorizaci značky atd hello back-end můžete využít hello [SDK centra oznámení pro operace back-end](https://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/).</span><span class="sxs-lookup"><span data-stu-id="c1550-179">hello client device still gets its PNS handle and relevant installation properties as before and calls a custom API on hello backend that can perform hello registration and authorize tags etc. hello backend can leverage hello [Notification Hub SDK for backend operations](https://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/).</span></span>

<span data-ttu-id="c1550-180">Můžete také použít metodu PATCH hello pomocí hello [JSON-Patch standard](https://tools.ietf.org/html/rfc6902) aktualizace hello instalace.</span><span class="sxs-lookup"><span data-stu-id="c1550-180">You can also use hello PATCH method using hello [JSON-Patch standard](https://tools.ietf.org/html/rfc6902) for updating hello installation.</span></span>

    // Initialize hello Notification Hub
    NotificationHubClient hub = NotificationHubClient.CreateClientFromConnectionString(listenConnString, hubName);

    // Custom API on hello backend
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


        // In hello backend we can control if a user is allowed tooadd tags
        //installation.Tags = new List<string>(deviceUpdate.Tags);
        //installation.Tags.Add("username:" + username);

        await hub.CreateOrUpdateInstallationAsync(installation);

        return Request.CreateResponse(HttpStatusCode.OK);
    }


#### <a name="example-code-tooregister-with-a-notification-hub-from-a-device-using-a-registration-id"></a><span data-ttu-id="c1550-181">Příklad kódu tooregister centra oznámení z pomocí id registrace zařízení</span><span class="sxs-lookup"><span data-stu-id="c1550-181">Example code tooregister with a notification hub from a device using a registration id</span></span>
<span data-ttu-id="c1550-182">Z vašeho back-end aplikace můžete provádět základní operace CRUDS při registraci.</span><span class="sxs-lookup"><span data-stu-id="c1550-182">From your app backend, you can perform basic CRUDS operations on registrations.</span></span> <span data-ttu-id="c1550-183">Například:</span><span class="sxs-lookup"><span data-stu-id="c1550-183">For example:</span></span>

    var hub = NotificationHubClient.CreateClientFromConnectionString("{connectionString}", "hubName");

    // create a registration description object of hello correct type, e.g.
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


<span data-ttu-id="c1550-184">back-end Hello musí zpracovávat souběžnosti mezi aktualizace registrace.</span><span class="sxs-lookup"><span data-stu-id="c1550-184">hello backend must handle concurrency between registration updates.</span></span> <span data-ttu-id="c1550-185">Service Bus nabízí optimistické řízení souběžného pro správu registrace.</span><span class="sxs-lookup"><span data-stu-id="c1550-185">Service Bus offers optimistic concurrency control for registration management.</span></span> <span data-ttu-id="c1550-186">Na úrovni hello HTTP tato možnost je implementovaná pomocí hello použití značky ETag na operace správy registrace.</span><span class="sxs-lookup"><span data-stu-id="c1550-186">At hello HTTP level, this is implemented with hello use of ETag on registration management operations.</span></span> <span data-ttu-id="c1550-187">Tato funkce slouží transparentně SDKs Microsoft, která je vyvolána výjimka, pokud aktualizace byl odmítnut z důvodů souběžnosti.</span><span class="sxs-lookup"><span data-stu-id="c1550-187">This feature is transparently used by Microsoft SDKs, which throw an exception if an update is rejected for concurrency reasons.</span></span> <span data-ttu-id="c1550-188">back-end aplikace Hello je zodpovědná za zpracování těchto výjimek a v případě potřeby opakováním hello aktualizace.</span><span class="sxs-lookup"><span data-stu-id="c1550-188">hello app backend is responsible for handling these exceptions and retrying hello update if required.</span></span>

