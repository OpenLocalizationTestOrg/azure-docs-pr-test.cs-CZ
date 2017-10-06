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
# <a name="registration-management"></a>Správa registrací
## <a name="overview"></a>Přehled
Toto téma vysvětluje, jak zařízení tooregister pomocí centra oznámení v pořadí tooreceive nabízená oznámení. Hello téma popisuje registrace na vysoké úrovni a potom zavádí hello dva hlavní vzory pro registraci zařízení: registrace z hello zařízení přímo toohello centra oznámení a registraci prostřednictvím back-end aplikace. 

## <a name="what-is-device-registration"></a>Co je registrace zařízení
Registrace zařízení s centrem oznámení se provádí pomocí **registrace** nebo **instalace**.

#### <a name="registrations"></a>Registrace
Registrace přidruží hello zpracovat oznámení služby platformy (PNS) pro zařízení s značky a případně šablonu. popisovače systému PNS Hello může být ChannelURI, token zařízení nebo id registrace GCM. Značky jsou použité tooroute oznámení toohello správná sada popisovačů zařízení. Další informace najdete v tématu [směrování a značky výrazy](notification-hubs-tags-segment-push-message.md). Šablony jsou použité tooimplement za registraci transformace. Další informace najdete v tématu [šablony](notification-hubs-templates-cross-platform-push-messages.md).

#### <a name="installations"></a>Instalace
Instalace je vylepšený registrace, který zahrnuje kontejner nabízené souvisejících vlastností. Je hello tooregistering nejnovější a nejlepší přístup zařízení. Však není podporován na straně klienta .NET SDK ([SDK centra oznámení pro back-end operace](https://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/)) ještě.  To znamená, že pokud se registrujete ze samotného klientského zařízení hello, byste měli toouse hello [rozhraní API REST centra oznámení](https://msdn.microsoft.com/library/mt621153.aspx) přístupu toosupport instalace. Pokud používáte back-end službu, byste měli mít toouse [SDK centra oznámení pro back-end operace](https://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/).

Hello Následují některé instalace toousing klíčových výhod:

* Vytvoření nebo aktualizace instalace je plně idempotent. Proto ji můžete zopakovat bez jakékoli pochybnostmi duplicitní registrace.
* Instalační model Hello umožňuje snadno toodo jednotlivých nabízených oznámení - cílení na konkrétní zařízení. Značku systému **"$InstallationId: [installationId]"** se automaticky přidá s každou instalace na základě registrace. Proto můžete volat odeslání toothis značky tootarget určité zařízení bez nutnosti toodo další kódování.
* Použití instalace umožňuje také můžete toodo registrace částečné aktualizace. Hello částečné aktualizace instalace se požaduje pomocí metody PATCH pomocí hello [JSON-Patch standard](https://tools.ietf.org/html/rfc6902). To je zvlášť užitečné, když chcete, aby tooupdate značky na hello registrace. Nemáte toopull dolů hello celý registrace a pak znovu odeslat všechny předchozí značky hello.

Instalace může obsahovat hello hello následující vlastnosti. Úplný seznam najdete vlastnosti hello instalace [vytvoření nebo instalaci přepsat REST API](https://msdn.microsoft.com/library/azure/mt621153.aspx) nebo [vlastnosti instalace](https://msdn.microsoft.com/library/azure/microsoft.azure.notificationhubs.installation_properties.aspx) pro hello.

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



Je důležité toonote, který už platnost registrace a instalace ve výchozím nastavení.

Registrace a instalace musí obsahovat platný popisovače systému PNS pro každé zařízení nebo kanál. Protože popisovačů systému PNS se dá získat jenom v aplikaci klienta na hello zařízení, je jeden vzor tooregister přímo na daném zařízení s hello klientskou aplikaci. Na hello jiné straně, důležité informace o zabezpečení a obchodní logiku související s tootags může vyžadovat toomanage registrace zařízení v back-end aplikace hello. 

#### <a name="templates"></a>Šablony
Pokud chcete, aby toouse [šablony](notification-hubs-templates-cross-platform-push-messages.md), instalace zařízení hello také obsahovat všechny šablony, které jsou spojené s tímto zařízením v JSON formátu (viz ukázka výše). Hello názvy šablon pomůže cíle různé šablony pro hello stejné zařízení.

Všimněte si, že každý název šablony mapuje tooa šablony textu a volitelná sada značky. Kromě toho každou platformu můžete mít další šablony vlastnosti. Pro Windows Store (pomocí WNS) a Windows Phone 8 (pomocí MPNS) může být další sadu hlaviček součástí hello šablony. V případě hello služby APN můžete nastavit vlastnost tooeither vypršení platnosti konstanta nebo tooa výraz šablony. Úplný seznam najdete vlastnosti hello instalace [vytvoření nebo instalaci přepsat REST](https://msdn.microsoft.com/library/azure/mt621153.aspx) tématu.

#### <a name="secondary-tiles-for-windows-store-apps"></a>Sekundární dlaždice pro aplikace pro Windows Store
Pro klientské aplikace Windows Store hello odesílání oznámení dlaždice toosecondary je stejný jako odesláním toohello primární. To je podporováno také v instalaci. Všimněte si, že sekundární dlaždice mají různé ChannelUri, které hello SDK na vaší klientské aplikace zpracovává transparentně.

používá slovník SecondaryTiles Hello hello stejné TileId, který je použité toocreate hello SecondaryTiles objekt v aplikaci Windows Store.
Jako s hello primární ChannelUri, ChannelUris sekundární dlaždice můžete změnit v každém okamžiku. V pořadí tookeep hello instalace v centru oznámení hello aktualizovat hello zařízení musíte aktualizovat je s hello aktuální ChannelUris hello sekundární dlaždice.

## <a name="registration-management-from-hello-device"></a>Správa registraci ze zařízení hello
Při správě registraci zařízení z klientské aplikace, je odpovědná za zasílání oznámení pouze hello back-end. Klientské aplikace udržovat popisovačů systému PNS až toodate a zaregistrujte značky. Hello následující obrázek znázorňuje tento vzor.

![](./media/notification-hubs-registration-management/notification-hubs-registering-on-device.png)

Hello zařízení nejprve načte hello systém PNS zpracování z hello Správou, a poté zaregistruje pomocí centra oznámení hello přímo. Po úspěšné registraci hello back-end aplikace hello můžete poslat oznámení cílení k registraci. Další informace o tom, najdete v části toosend oznámení [směrování a značky výrazy](notification-hubs-tags-segment-push-message.md).
Všimněte si, že v takovém případě budete používat jenom naslouchat práva tooaccess vašeho centra oznámení z hello zařízení. Další informace najdete v tématu [zabezpečení](notification-hubs-push-notification-security.md).

Registrace zařízení hello je hello Nejjednodušším způsobem, ale má některé nevýhody.
Hello první nevýhodou je, že klientská aplikace lze aktualizovat pouze jeho značky když aplikace hello je aktivní. Například pokud má uživatel dvě zařízení, které registrují týmy související toosport značky, pokud první zařízení hello zaregistruje pro další značky (například Seahawks), hello druhé zařízení obdrží hello upozornění o hello Seahawks až po aplikace hello na hello druhé zařízení je provést ještě jednou. Obecně platí když značky jsou ovlivněné více zařízení, správa značky z back-end hello je žádoucí možnost.
druhý nevýhodou Hello registrace správy z klienta aplikace hello je, že vzhledem k tomu, že aplikace může hacker, zabezpečení hello registrace toospecific značky vyžaduje zvláštní pozornost, jak je popsáno v části hello "zabezpečení na úrovni značky."

#### <a name="example-code-tooregister-with-a-notification-hub-from-a-device-using-an-installation"></a>Příklad kódu tooregister centra oznámení ze zařízení pomocí instalace
V tuto chvíli je podporováno pouze pomocí hello [rozhraní API REST centra oznámení](https://msdn.microsoft.com/library/mt621153.aspx).

Můžete také použít metodu PATCH hello pomocí hello [JSON-Patch standard](https://tools.ietf.org/html/rfc6902) aktualizace hello instalace.

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



#### <a name="example-code-tooregister-with-a-notification-hub-from-a-device-using-a-registration"></a>Příklad kódu tooregister centra oznámení z pomocí registrace zařízení
Tyto metody vytvořit nebo aktualizovat registraci pro hello zařízení, na které se nazývají. To znamená, musí přepsat v pořadí tooupdate hello popisovač nebo hello značek, hello celý registrace. Mějte na paměti, že jsou registrace přechodný, a proto byste měli mít vždy spolehlivé úložiště s hello aktuální značky, které potřebuje určité zařízení.

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


## <a name="registration-management-from-a-backend"></a>Registrace správy z back-end
Správa registrace z back-end hello vyžaduje zápis další kód. aplikace Hello z hello zařízení musíte zadat při každém spuštění aplikace hello (společně se značkami a šablon) hello aktualizovaný systém PNS popisovač toohello back-end a back-end hello musíte aktualizovat tento popisovač v centru oznámení hello. Hello následující obrázek znázorňuje tento návrh.

![](./media/notification-hubs-registration-management/notification-hubs-registering-on-backend.png)

Hello výhody správy registrace z back-end hello zahrnují hello možnost toomodify značky tooregistrations i v případě, že hello odpovídající aplikace na zařízení hello je neaktivní a tooauthenticate hello klientské aplikace před přidáním registrace tooits značky.

#### <a name="example-code-tooregister-with-a-notification-hub-from-a-backend-using-an-installation"></a>Příklad kódu tooregister centra oznámení z back-end pomocí instalace
Hello klientské zařízení stále získá jeho popisovače systému PNS a vlastností instalace relevantní jako dříve a volání vlastní rozhraní API na back-end hello, které můžete provést registraci hello a autorizaci značky atd hello back-end můžete využít hello [SDK centra oznámení pro operace back-end](https://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/).

Můžete také použít metodu PATCH hello pomocí hello [JSON-Patch standard](https://tools.ietf.org/html/rfc6902) aktualizace hello instalace.

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


#### <a name="example-code-tooregister-with-a-notification-hub-from-a-device-using-a-registration-id"></a>Příklad kódu tooregister centra oznámení z pomocí id registrace zařízení
Z vašeho back-end aplikace můžete provádět základní operace CRUDS při registraci. Například:

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


back-end Hello musí zpracovávat souběžnosti mezi aktualizace registrace. Service Bus nabízí optimistické řízení souběžného pro správu registrace. Na úrovni hello HTTP tato možnost je implementovaná pomocí hello použití značky ETag na operace správy registrace. Tato funkce slouží transparentně SDKs Microsoft, která je vyvolána výjimka, pokud aktualizace byl odmítnut z důvodů souběžnosti. back-end aplikace Hello je zodpovědná za zpracování těchto výjimek a v případě potřeby opakováním hello aktualizace.

