---
title: aaaUnderstand podpory Azure IoT Hub MQTT | Microsoft Docs
description: "Vývojáře průvodce – podpora zařízení připojující se tooan pomocí koncového bodu směřujících zařízení IoT Hub hello MQTT protokolu. Obsahuje informace o integrovanou podporu MQTT v hello SDK pro zařízení Azure IoT."
services: iot-hub
documentationcenter: .net
author: kdotchkoff
manager: timlt
editor: 
ms.assetid: 1d71c27c-b466-4a40-b95b-d6550cf85144
ms.service: iot-hub
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/11/2017
ms.author: kdotchko
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: e461687963138987acdf1f4e0e34c453744ea191
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="communicate-with-your-iot-hub-using-hello-mqtt-protocol"></a><span data-ttu-id="3b739-104">Komunikovat se službou IoT hub pomocí protokolu MQTT hello</span><span class="sxs-lookup"><span data-stu-id="3b739-104">Communicate with your IoT hub using hello MQTT protocol</span></span>

<span data-ttu-id="3b739-105">IoT Hub umožňuje toocommunicate zařízení s hello zařízení koncové body centra IoT pomocí hello [protokoly MQTT v3.1.1] [ lnk-mqtt-org] protokolu na port 8883 nebo protokoly MQTT v3.1.1 přes protokol WebSocket na portu 443.</span><span class="sxs-lookup"><span data-stu-id="3b739-105">IoT Hub enables devices toocommunicate with hello IoT Hub device endpoints using hello [MQTT v3.1.1][lnk-mqtt-org] protocol on port 8883 or MQTT v3.1.1 over WebSocket protocol on port 443.</span></span> <span data-ttu-id="3b739-106">IoT Hub vyžaduje toobe komunikace všechna zařízení, které jsou zabezpečené pomocí protokolu TLS/SSL (proto IoT Hub nepodporuje nezabezpečené připojení přes port 1883).</span><span class="sxs-lookup"><span data-stu-id="3b739-106">IoT Hub requires all device communication toobe secured using TLS/SSL (hence, IoT Hub doesn’t support non-secure connections over port 1883).</span></span>

## <a name="connecting-tooiot-hub"></a><span data-ttu-id="3b739-107">Připojení tooIoT rozbočovače</span><span class="sxs-lookup"><span data-stu-id="3b739-107">Connecting tooIoT Hub</span></span>

<span data-ttu-id="3b739-108">Zařízení můžete použít Centrum IoT hello MQTT protokol tooconnect tooan buď pomocí knihovny hello v hello [SDK služby Azure IoT] [ lnk-device-sdks] nebo přímo pomocí protokolu MQTT hello.</span><span class="sxs-lookup"><span data-stu-id="3b739-108">A device can use hello MQTT protocol tooconnect tooan IoT hub either by using hello libraries in hello [Azure IoT SDKs][lnk-device-sdks] or by using hello MQTT protocol directly.</span></span>

## <a name="using-hello-device-sdks"></a><span data-ttu-id="3b739-109">Pomocí sady SDK pro zařízení hello</span><span class="sxs-lookup"><span data-stu-id="3b739-109">Using hello device SDKs</span></span>

<span data-ttu-id="3b739-110">[Sady SDK zařízení] [ lnk-device-sdks] tuto podporu hello MQTT protokolu jsou k dispozici pro Java, Node.js, C, C# a Python.</span><span class="sxs-lookup"><span data-stu-id="3b739-110">[Device SDKs][lnk-device-sdks] that support hello MQTT protocol are available for Java, Node.js, C, C#, and Python.</span></span> <span data-ttu-id="3b739-111">sady SDK pro zařízení Hello použijte hello standardní IoT Hub připojovací řetězec tooestablish připojení tooan IoT hub.</span><span class="sxs-lookup"><span data-stu-id="3b739-111">hello device SDKs use hello standard IoT Hub connection string tooestablish a connection tooan IoT hub.</span></span> <span data-ttu-id="3b739-112">protokol MQTT hello toouse, hello klienta protokolu parametr musí být nastaven příliš**MQTT**.</span><span class="sxs-lookup"><span data-stu-id="3b739-112">toouse hello MQTT protocol, hello client protocol parameter must be set too**MQTT**.</span></span> <span data-ttu-id="3b739-113">Ve výchozím nastavení, sady SDK pro zařízení hello připojit tooan IoT Hub s hello **CleanSession** příliš nastaven příznak**0** a používat **QoS 1** k výměně zpráv službou hello IoT hub.</span><span class="sxs-lookup"><span data-stu-id="3b739-113">By default, hello device SDKs connect tooan IoT Hub with hello **CleanSession** flag set too**0** and use **QoS 1** for message exchange with hello IoT hub.</span></span>

<span data-ttu-id="3b739-114">Když je zařízení připojené tooan služby IoT hub, sady SDK pro zařízení hello poskytují metody, které umožní zprávy toosend zařízení hello tooand přijímat zprávy ze služby IoT hub.</span><span class="sxs-lookup"><span data-stu-id="3b739-114">When a device is connected tooan IoT hub, hello device SDKs provide methods that enable hello device toosend messages tooand receive messages from an IoT hub.</span></span>

<span data-ttu-id="3b739-115">Hello následující tabulka obsahuje odkazy toocode ukázky pro každý podporovaný jazyk a určuje hello parametr toouse tooestablish tooIoT připojení rozbočovače pomocí protokolu MQTT hello.</span><span class="sxs-lookup"><span data-stu-id="3b739-115">hello following table contains links toocode samples for each supported language and specifies hello parameter toouse tooestablish a connection tooIoT Hub using hello MQTT protocol.</span></span>

| <span data-ttu-id="3b739-116">Jazyk</span><span class="sxs-lookup"><span data-stu-id="3b739-116">Language</span></span> | <span data-ttu-id="3b739-117">Parametr protokolu</span><span class="sxs-lookup"><span data-stu-id="3b739-117">Protocol parameter</span></span> |
| --- | --- |
| <span data-ttu-id="3b739-118">[Node.js][lnk-sample-node]</span><span class="sxs-lookup"><span data-stu-id="3b739-118">[Node.js][lnk-sample-node]</span></span> |<span data-ttu-id="3b739-119">Azure-iot zařízení mqtt</span><span class="sxs-lookup"><span data-stu-id="3b739-119">azure-iot-device-mqtt</span></span> |
| <span data-ttu-id="3b739-120">[Java][lnk-sample-java]</span><span class="sxs-lookup"><span data-stu-id="3b739-120">[Java][lnk-sample-java]</span></span> |<span data-ttu-id="3b739-121">IotHubClientProtocol.MQTT</span><span class="sxs-lookup"><span data-stu-id="3b739-121">IotHubClientProtocol.MQTT</span></span> |
| <span data-ttu-id="3b739-122">[C][lnk-sample-c]</span><span class="sxs-lookup"><span data-stu-id="3b739-122">[C][lnk-sample-c]</span></span> |<span data-ttu-id="3b739-123">MQTT_Protocol</span><span class="sxs-lookup"><span data-stu-id="3b739-123">MQTT_Protocol</span></span> |
| <span data-ttu-id="3b739-124">[C#][lnk-sample-csharp]</span><span class="sxs-lookup"><span data-stu-id="3b739-124">[C#][lnk-sample-csharp]</span></span> |<span data-ttu-id="3b739-125">TransportType.Mqtt</span><span class="sxs-lookup"><span data-stu-id="3b739-125">TransportType.Mqtt</span></span> |
| <span data-ttu-id="3b739-126">[Python][lnk-sample-python]</span><span class="sxs-lookup"><span data-stu-id="3b739-126">[Python][lnk-sample-python]</span></span> |<span data-ttu-id="3b739-127">IoTHubTransportProvider.MQTT</span><span class="sxs-lookup"><span data-stu-id="3b739-127">IoTHubTransportProvider.MQTT</span></span> |

### <a name="migrating-a-device-app-from-amqp-toomqtt"></a><span data-ttu-id="3b739-128">Migrace z AMQP tooMQTT aplikace na zařízení</span><span class="sxs-lookup"><span data-stu-id="3b739-128">Migrating a device app from AMQP tooMQTT</span></span>

<span data-ttu-id="3b739-129">Pokud používáte hello [sady SDK pro zařízení][lnk-device-sdks], přepínání pomocí protokolu AMQP vyžaduje tooMQTT změna hello protokol parametr hello inicializace klienta, jak bylo uvedeno dříve.</span><span class="sxs-lookup"><span data-stu-id="3b739-129">If you are using hello [device SDKs][lnk-device-sdks], switching from using AMQP tooMQTT requires changing hello protocol parameter in hello client initialization as stated previously.</span></span>

<span data-ttu-id="3b739-130">Když to uděláte, ujistěte se, zda text hello toocheck následující položky:</span><span class="sxs-lookup"><span data-stu-id="3b739-130">When doing so, make sure toocheck hello following items:</span></span>

* <span data-ttu-id="3b739-131">AMQP vrátí chyby pro mnoho podmínek, zatímco MQTT ukončí hello připojení.</span><span class="sxs-lookup"><span data-stu-id="3b739-131">AMQP returns errors for many conditions, while MQTT terminates hello connection.</span></span> <span data-ttu-id="3b739-132">V důsledku vaše zpracování logiky výjimek může vyžadovat některé změny.</span><span class="sxs-lookup"><span data-stu-id="3b739-132">As a result your exception handling logic might require some changes.</span></span>
* <span data-ttu-id="3b739-133">MQTT nepodporuje hello *odmítnout* operace při přijímání [zprávy typu cloud zařízení][lnk-messaging].</span><span class="sxs-lookup"><span data-stu-id="3b739-133">MQTT does not support hello *reject* operations when receiving [cloud-to-device messages][lnk-messaging].</span></span> <span data-ttu-id="3b739-134">Pokud vaše aplikace back-end musí tooreceive odpověď z aplikace hello zařízení, zvažte použití [přímé metody][lnk-methods].</span><span class="sxs-lookup"><span data-stu-id="3b739-134">If your back-end app needs tooreceive a response from hello device app, consider using [direct methods][lnk-methods].</span></span>

## <a name="using-hello-mqtt-protocol-directly"></a><span data-ttu-id="3b739-135">Přímo pomocí protokolu MQTT hello</span><span class="sxs-lookup"><span data-stu-id="3b739-135">Using hello MQTT protocol directly</span></span>
<span data-ttu-id="3b739-136">Pokud zařízení nelze použít sady SDK pro zařízení hello, se může pořád připojit toohello zařízení veřejné koncové body pomocí protokolu MQTT hello na portu 8883.</span><span class="sxs-lookup"><span data-stu-id="3b739-136">If a device cannot use hello device SDKs, it can still connect toohello public device endpoints using hello MQTT protocol on port 8883.</span></span> <span data-ttu-id="3b739-137">V hello **CONNECT** paketu hello zařízení musí používat hello následující hodnoty:</span><span class="sxs-lookup"><span data-stu-id="3b739-137">In hello **CONNECT** packet hello device should use hello following values:</span></span>

* <span data-ttu-id="3b739-138">Pro hello **ClientId** pole, použijte hello **deviceId**.</span><span class="sxs-lookup"><span data-stu-id="3b739-138">For hello **ClientId** field, use hello **deviceId**.</span></span>
* <span data-ttu-id="3b739-139">Pro hello **uživatelské jméno** pole, použijte `{iothubhostname}/{device_id}/api-version=2016-11-14`, kde {iothubhostname} je hello úplné CName hello IoT hub.</span><span class="sxs-lookup"><span data-stu-id="3b739-139">For hello **Username** field, use `{iothubhostname}/{device_id}/api-version=2016-11-14`, where {iothubhostname} is hello full CName of hello IoT hub.</span></span>

    <span data-ttu-id="3b739-140">Například pokud hello název služby IoT hub je **contoso.azure devices.net** a pokud je název vašeho zařízení hello **MyDevice01**, hello úplné **uživatelské jméno** by měl obsahovat pole `contoso.azure-devices.net/MyDevice01/api-version=2016-11-14`.</span><span class="sxs-lookup"><span data-stu-id="3b739-140">For example, if hello name of your IoT hub is **contoso.azure-devices.net** and if hello name of your device is **MyDevice01**, hello full **Username** field should contain `contoso.azure-devices.net/MyDevice01/api-version=2016-11-14`.</span></span>
* <span data-ttu-id="3b739-141">Pro hello **heslo** pole, použijte SAS token.</span><span class="sxs-lookup"><span data-stu-id="3b739-141">For hello **Password** field, use a SAS token.</span></span> <span data-ttu-id="3b739-142">Formát Hello hello SAS token je hello stejné jako u hello HTTP a protokoly AMQP:</span><span class="sxs-lookup"><span data-stu-id="3b739-142">hello format of hello SAS token is hello same as for both hello HTTP and AMQP protocols:</span></span><br/><span data-ttu-id="3b739-143">`SharedAccessSignature sig={signature-string}&se={expiry}&sr={URL-encoded-resourceURI}`.</span><span class="sxs-lookup"><span data-stu-id="3b739-143">`SharedAccessSignature sig={signature-string}&se={expiry}&sr={URL-encoded-resourceURI}`.</span></span>

    <span data-ttu-id="3b739-144">Další informace o tom, toogenerate tokeny SAS, najdete v části zařízení hello [tokeny zabezpečení pomocí služby IoT Hub][lnk-sas-tokens].</span><span class="sxs-lookup"><span data-stu-id="3b739-144">For more information about how toogenerate SAS tokens, see hello device section of [Using IoT Hub security tokens][lnk-sas-tokens].</span></span>

    <span data-ttu-id="3b739-145">Při testování, můžete použít také hello [explorer zařízení] [ lnk-device-explorer] tooquickly nástroj vygenerovat token SAS, který můžete zkopírovat a vložit do vlastního kódu:</span><span class="sxs-lookup"><span data-stu-id="3b739-145">When testing, you can also use hello [device explorer][lnk-device-explorer] tool tooquickly generate a SAS token that you can copy and paste into your own code:</span></span>

  1. <span data-ttu-id="3b739-146">Přejděte toohello **správy** kartě v **Explorer zařízení**.</span><span class="sxs-lookup"><span data-stu-id="3b739-146">Go toohello **Management** tab in **Device Explorer**.</span></span>
  2. <span data-ttu-id="3b739-147">Klikněte na tlačítko **tokenu SAS** (pravého horního).</span><span class="sxs-lookup"><span data-stu-id="3b739-147">Click **SAS Token** (top right).</span></span>
  3. <span data-ttu-id="3b739-148">Na **SASTokenForm**, vyberte příslušné zařízení v hello **DeviceID** rozevírací nabídku.</span><span class="sxs-lookup"><span data-stu-id="3b739-148">On **SASTokenForm**, select your device in hello **DeviceID** drop down.</span></span> <span data-ttu-id="3b739-149">Nastavit vaše **TTL**.</span><span class="sxs-lookup"><span data-stu-id="3b739-149">Set your **TTL**.</span></span>
  4. <span data-ttu-id="3b739-150">Klikněte na tlačítko **generování** toocreate tokenu.</span><span class="sxs-lookup"><span data-stu-id="3b739-150">Click **Generate** toocreate your token.</span></span>

     <span data-ttu-id="3b739-151">Hello tokenu SAS, aby se vygenerovala má tuto strukturu: `HostName={your hub name}.azure-devices.net;DeviceId=javadevice;SharedAccessSignature=SharedAccessSignature sr={your hub name}.azure-devices.net%2Fdevices%2FMyDevice01%2Fapi-version%3D2016-11-14&sig=vSgHBMUG.....Ntg%3d&se=1456481802`.</span><span class="sxs-lookup"><span data-stu-id="3b739-151">hello SAS token that's generated has this structure: `HostName={your hub name}.azure-devices.net;DeviceId=javadevice;SharedAccessSignature=SharedAccessSignature sr={your hub name}.azure-devices.net%2Fdevices%2FMyDevice01%2Fapi-version%3D2016-11-14&sig=vSgHBMUG.....Ntg%3d&se=1456481802`.</span></span>

     <span data-ttu-id="3b739-152">Hello součástí tento token toouse jako hello **heslo** je pole tooconnect pomocí MQTT: `SharedAccessSignature sr={your hub name}.azure-devices.net%2Fdevices%2FMyDevice01%2Fapi-version%3D2016-11-14&sig=vSgHBMUG.....Ntg%3d&se=1456481802`.</span><span class="sxs-lookup"><span data-stu-id="3b739-152">hello part of this token toouse as hello **Password** field tooconnect using MQTT is: `SharedAccessSignature sr={your hub name}.azure-devices.net%2Fdevices%2FMyDevice01%2Fapi-version%3D2016-11-14&sig=vSgHBMUG.....Ntg%3d&se=1456481802`.</span></span>

<span data-ttu-id="3b739-153">MQTT připojit a odpojit paketů, IoT Hub vydá událost na hello **monitorování Operations** kanál společně s dalšími informacemi, které vám pomůžou tootroubleshoot problémy s připojením.</span><span class="sxs-lookup"><span data-stu-id="3b739-153">For MQTT connect and disconnect packets, IoT Hub issues an event on hello **Operations Monitoring** channel with additional information that can help you tootroubleshoot connectivity issues.</span></span>

### <a name="sending-device-to-cloud-messages"></a><span data-ttu-id="3b739-154">Odesílání zpráv typu zařízení cloud</span><span class="sxs-lookup"><span data-stu-id="3b739-154">Sending device-to-cloud messages</span></span>

<span data-ttu-id="3b739-155">Po provedení úspěšného připojení, můžete odeslat zařízení pomocí Centra zpráv tooIoT `devices/{device_id}/messages/events/` nebo `devices/{device_id}/messages/events/{property_bag}` jako **název tématu**.</span><span class="sxs-lookup"><span data-stu-id="3b739-155">After making a successful connection, a device can send messages tooIoT Hub using `devices/{device_id}/messages/events/` or `devices/{device_id}/messages/events/{property_bag}` as a **Topic Name**.</span></span> <span data-ttu-id="3b739-156">Hello `{property_bag}` prvek umožní zprávy toosend hello zařízení bez dalších vlastností ve formátu kódovaná adresou url.</span><span class="sxs-lookup"><span data-stu-id="3b739-156">hello `{property_bag}` element enables hello device toosend messages with additional properties in a url-encoded format.</span></span> <span data-ttu-id="3b739-157">Například:</span><span class="sxs-lookup"><span data-stu-id="3b739-157">For example:</span></span>

```
RFC 2396-encoded(<PropertyName1>)=RFC 2396-encoded(<PropertyValue1>)&RFC 2396-encoded(<PropertyName2>)=RFC 2396-encoded(<PropertyValue2>)…
```

> [!NOTE]
> <span data-ttu-id="3b739-158">To `{property_bag}` element hello používá stejné kódování jako řetězce dotazu v protokolu hello protokolu HTTP.</span><span class="sxs-lookup"><span data-stu-id="3b739-158">This `{property_bag}` element uses hello same encoding as for query strings in hello HTTP protocol.</span></span>
>
>

<span data-ttu-id="3b739-159">Můžete také použít aplikaci zařízení Hello `devices/{device_id}/messages/events/{property_bag}` jako hello **název tématu bude** toodefine *bude zprávy* toobe předávány jako zprávu telemetrie.</span><span class="sxs-lookup"><span data-stu-id="3b739-159">hello device app can also use `devices/{device_id}/messages/events/{property_bag}` as hello **Will topic name** toodefine *Will messages* toobe forwarded as a telemetry message.</span></span>

- <span data-ttu-id="3b739-160">IoT Hub nepodporuje QoS 2 zprávy.</span><span class="sxs-lookup"><span data-stu-id="3b739-160">IoT Hub does not support QoS 2 messages.</span></span> <span data-ttu-id="3b739-161">Pokud aplikace na zařízení publikuje zprávu s **QoS 2**, IoT Hub ukončí hello síťové připojení.</span><span class="sxs-lookup"><span data-stu-id="3b739-161">If a device app publishes a message with **QoS 2**, IoT Hub closes hello network connection.</span></span>
- <span data-ttu-id="3b739-162">IoT Hub není zachována zachovat zprávy.</span><span class="sxs-lookup"><span data-stu-id="3b739-162">IoT Hub does not persist Retain messages.</span></span> <span data-ttu-id="3b739-163">Pokud zařízení odesílá zprávy s hello **zachovat** nastaven příznak too1, IoT Hub přidá hello **x-opt zachovat** aplikace vlastnost toohello zpráva.</span><span class="sxs-lookup"><span data-stu-id="3b739-163">If a device sends a message with hello **RETAIN** flag set too1, IoT Hub adds hello **x-opt-retain** application property toohello message.</span></span> <span data-ttu-id="3b739-164">V takovém případě místo zachování hello zachovat zpráva, IoT Hub předá toohello back-end aplikace.</span><span class="sxs-lookup"><span data-stu-id="3b739-164">In this case, instead of persisting hello retain message, IoT Hub passes it toohello backend app.</span></span>
- <span data-ttu-id="3b739-165">IoT Hub podporuje pouze jeden aktivní připojení MQTT na jedno zařízení.</span><span class="sxs-lookup"><span data-stu-id="3b739-165">IoT Hub only supports one active MQTT connection per device.</span></span> <span data-ttu-id="3b739-166">Všechny nové připojení MQTT jménem hello způsobí stejné ID zařízení IoT Hub toodrop hello existující připojení.</span><span class="sxs-lookup"><span data-stu-id="3b739-166">Any new MQTT connection on behalf of hello same device ID causes IoT Hub toodrop hello existing connection.</span></span>

<span data-ttu-id="3b739-167">Další informace najdete v tématu [– Příručka vývojáře pro zasílání zpráv][lnk-messaging].</span><span class="sxs-lookup"><span data-stu-id="3b739-167">For more information, see [Messaging developer's guide][lnk-messaging].</span></span>

### <a name="receiving-cloud-to-device-messages"></a><span data-ttu-id="3b739-168">Příjem zpráv typu cloud zařízení</span><span class="sxs-lookup"><span data-stu-id="3b739-168">Receiving cloud-to-device messages</span></span>

<span data-ttu-id="3b739-169">tooreceive zprávy ze služby IoT Hub, zařízení by měl přihlásit se pomocí `devices/{device_id}/messages/devicebound/#` jako **tématu filtru**.</span><span class="sxs-lookup"><span data-stu-id="3b739-169">tooreceive messages from IoT Hub, a device should subscribe using `devices/{device_id}/messages/devicebound/#` as a **Topic Filter**.</span></span> <span data-ttu-id="3b739-170">Hello víceúrovňovou zástupné  **#**  v hello tématu filtr se používá jen tooallow hello zařízení tooreceive další vlastnosti v název tématu hello.</span><span class="sxs-lookup"><span data-stu-id="3b739-170">hello multi-level wildcard **#** in hello Topic Filter is used only tooallow hello device tooreceive additional properties in hello topic name.</span></span> <span data-ttu-id="3b739-171">IoT Hub neumožňuje použití hello hello  **#**  nebo **?**</span><span class="sxs-lookup"><span data-stu-id="3b739-171">IoT Hub does not allow hello usage of hello **#** or **?**</span></span> <span data-ttu-id="3b739-172">zástupné znaky pro filtrování dílčí témata.</span><span class="sxs-lookup"><span data-stu-id="3b739-172">wildcards for filtering of sub-topics.</span></span> <span data-ttu-id="3b739-173">Vzhledem k tomu, že Centrum IoT není zprostředkovatel zasílání zpráv protokol pub-sub obecné účely, podporuje pouze názvy tématu hello zdokumentované a téma filtry.</span><span class="sxs-lookup"><span data-stu-id="3b739-173">Since IoT Hub is not a general purpose pub-sub messaging broker, it only supports hello documented topic names and topic filters.</span></span>

<span data-ttu-id="3b739-174">Hello zařízení neobdrží všechny zprávy ze služby IoT Hub, dokud ho se úspěšně připojila tooits konkrétní zařízení koncového bodu, reprezentována hello `devices/{device_id}/messages/devicebound/#` tématu filtru.</span><span class="sxs-lookup"><span data-stu-id="3b739-174">hello device does not receive any messages from IoT Hub, until it has successfully subscribed tooits device-specific endpoint, represented by hello `devices/{device_id}/messages/devicebound/#` topic filter.</span></span> <span data-ttu-id="3b739-175">Po úspěšné odběru bylo úspěšně navázáno, spustí hello zařízení, přijímání zpráv pouze cloud zařízení, které byly odeslány tooit po době hello hello předplatného.</span><span class="sxs-lookup"><span data-stu-id="3b739-175">After successful subscription has been established, hello device starts receiving only cloud-to-device messages that have been sent tooit after hello time of hello subscription.</span></span> <span data-ttu-id="3b739-176">Pokud zařízení hello připojí **CleanSession** příliš nastaven příznak**0**, hello předplatného je trvalé v jiných relacích.</span><span class="sxs-lookup"><span data-stu-id="3b739-176">If hello device connects with **CleanSession** flag set too**0**, hello subscription is persisted across different sessions.</span></span> <span data-ttu-id="3b739-177">V takovém případě hello zase připojí s **CleanSession 0** hello zařízení přijímá nezpracovaných zprávy, které byly odeslány tooit době, kdy byl odpojen.</span><span class="sxs-lookup"><span data-stu-id="3b739-177">In this case, hello next time it connects with **CleanSession 0** hello device receives outstanding messages that have been sent tooit while it was disconnected.</span></span> <span data-ttu-id="3b739-178">Pokud zařízení hello používá **CleanSession** příliš nastaven příznak**1** ale, že neobdrží všechny zprávy ze služby IoT Hub dokud přihlásí tooits zařízení-koncový bod.</span><span class="sxs-lookup"><span data-stu-id="3b739-178">If hello device uses **CleanSession** flag set too**1** though, it does not receive any messages from IoT Hub until it subscribes tooits device-endpoint.</span></span>

<span data-ttu-id="3b739-179">IoT Hub zajišťuje zprávy s hello **název tématu** `devices/{device_id}/messages/devicebound/`, nebo `devices/{device_id}/messages/devicebound/{property_bag}` Pokud nejsou k dispozici žádné vlastnosti zprávy.</span><span class="sxs-lookup"><span data-stu-id="3b739-179">IoT Hub delivers messages with hello **Topic Name** `devices/{device_id}/messages/devicebound/`, or `devices/{device_id}/messages/devicebound/{property_bag}` if there are any message properties.</span></span> <span data-ttu-id="3b739-180">`{property_bag}`obsahuje dvojice klíč/hodnota kódovaná adresou url vlastností zpráv.</span><span class="sxs-lookup"><span data-stu-id="3b739-180">`{property_bag}` contains url-encoded key/value pairs of message properties.</span></span> <span data-ttu-id="3b739-181">Pouze vlastnosti aplikace a vlastnosti uživatele nastavit systému (například **messageId** nebo **correlationId**) jsou zahrnuty v kontejneru objektů a dat hello.</span><span class="sxs-lookup"><span data-stu-id="3b739-181">Only application properties and user-settable system properties (such as **messageId** or **correlationId**) are included in hello property bag.</span></span> <span data-ttu-id="3b739-182">Názvy vlastností systému mají následující předpony hello  **$** , vlastnosti aplikace pomocí žádná předpona. původní název vlastnosti hello.</span><span class="sxs-lookup"><span data-stu-id="3b739-182">System property names have hello prefix **$**, application properties use hello original property name with no prefix.</span></span>

<span data-ttu-id="3b739-183">Když aplikace na zařízení odběratel tooa téma se **QoS 2**, IoT Hub uděluje maximální QoS úrovně 1 v hello **SUBACK** paketů.</span><span class="sxs-lookup"><span data-stu-id="3b739-183">When a device app subscribes tooa topic with **QoS 2**, IoT Hub grants maximum QoS level 1 in hello **SUBACK** packet.</span></span> <span data-ttu-id="3b739-184">Potom přináší IoT Hub zařízení toohello zprávy pomocí technologie QoS 1.</span><span class="sxs-lookup"><span data-stu-id="3b739-184">After that, IoT Hub delivers messages toohello device using QoS 1.</span></span>

### <a name="retrieving-a-device-twins-properties"></a><span data-ttu-id="3b739-185">Načítání vlastností dvojče zařízení</span><span class="sxs-lookup"><span data-stu-id="3b739-185">Retrieving a device twin's properties</span></span>

<span data-ttu-id="3b739-186">Nejprve zařízení odběratel příliš`$iothub/twin/res/#`, tooreceive hello operace odpovědi.</span><span class="sxs-lookup"><span data-stu-id="3b739-186">First, a device subscribes too`$iothub/twin/res/#`, tooreceive hello operation's responses.</span></span> <span data-ttu-id="3b739-187">Pak odešle prázdná zpráva tootopic `$iothub/twin/GET/?$rid={request id}`, s hodnotou vyplněná **id žádosti**. hello služby pak odešle zprávu odpovědi, která obsahuje data twin hello zařízení k tématu `$iothub/twin/res/{status}/?$rid={request id}`, pomocí stejné hello  **id žádosti** jako hello požadavku.</span><span class="sxs-lookup"><span data-stu-id="3b739-187">Then, it sends an empty message tootopic `$iothub/twin/GET/?$rid={request id}`, with a populated value for **request id**. hello service then sends a response message containing hello device twin data on topic `$iothub/twin/res/{status}/?$rid={request id}`, using hello same **request id** as hello request.</span></span>

<span data-ttu-id="3b739-188">Id žádosti může být libovolná platná hodnota pro hodnotu vlastnosti zprávy dle [IoT Hub – Příručka vývojáře pro zasílání zpráv][lnk-messaging], a stav byl ověřen jako celé číslo.</span><span class="sxs-lookup"><span data-stu-id="3b739-188">Request id can be any valid value for a message property value, as per [IoT Hub messaging developer's guide][lnk-messaging], and status is validated as an integer.</span></span>
<span data-ttu-id="3b739-189">text odpovědi Hello obsahuje sekce properties hello hello dvojče zařízení:</span><span class="sxs-lookup"><span data-stu-id="3b739-189">hello response body contains hello properties section of hello device twin:</span></span>

<span data-ttu-id="3b739-190">textu Hello položky registru identit hello omezené toohello člen "vlastnosti", například:</span><span class="sxs-lookup"><span data-stu-id="3b739-190">hello body of hello identity registry entry limited toohello “properties” member, for example:</span></span>

        {
            "properties": {
                "desired": {
                    "telemetrySendFrequency": "5m",
                    "$version": 12
                },
                "reported": {
                    "telemetrySendFrequency": "5m",
                    "batteryLevel": 55,
                    "$version": 123
                }
            }
        }

<span data-ttu-id="3b739-191">Hello možné stavové kódy jsou:</span><span class="sxs-lookup"><span data-stu-id="3b739-191">hello possible status codes are:</span></span>

|<span data-ttu-id="3b739-192">Status</span><span class="sxs-lookup"><span data-stu-id="3b739-192">Status</span></span> | <span data-ttu-id="3b739-193">Popis</span><span class="sxs-lookup"><span data-stu-id="3b739-193">Description</span></span> |
| ----- | ----------- |
| <span data-ttu-id="3b739-194">200</span><span class="sxs-lookup"><span data-stu-id="3b739-194">200</span></span> | <span data-ttu-id="3b739-195">Úspěch</span><span class="sxs-lookup"><span data-stu-id="3b739-195">Success</span></span> |
| <span data-ttu-id="3b739-196">429</span><span class="sxs-lookup"><span data-stu-id="3b739-196">429</span></span> | <span data-ttu-id="3b739-197">Příliš mnoho požadavků (omezení), jako za [IoT Hub, omezení šířky pásma][lnk-quotas]</span><span class="sxs-lookup"><span data-stu-id="3b739-197">Too many requests (throttled), as per [IoT Hub throttling][lnk-quotas]</span></span> |
| <span data-ttu-id="3b739-198">5**</span><span class="sxs-lookup"><span data-stu-id="3b739-198">5**</span></span> | <span data-ttu-id="3b739-199">Chyby serveru</span><span class="sxs-lookup"><span data-stu-id="3b739-199">Server errors</span></span> |

<span data-ttu-id="3b739-200">Další informace najdete v tématu [– Příručka vývojáře dvojčata zařízení][lnk-devguide-twin].</span><span class="sxs-lookup"><span data-stu-id="3b739-200">For more information, see [Device twins developer's guide][lnk-devguide-twin].</span></span>

### <a name="update-device-twins-reported-properties"></a><span data-ttu-id="3b739-201">Aktualizovat vlastnosti hlášené dvojče zařízení</span><span class="sxs-lookup"><span data-stu-id="3b739-201">Update device twin's reported properties</span></span>

<span data-ttu-id="3b739-202">Hello následující text popisuje jak zařízení aktualizuje hello hlášené vlastnosti v hello dvojče zařízení IoT hub:</span><span class="sxs-lookup"><span data-stu-id="3b739-202">hello following sequence describes how a device updates hello reported properties in hello device twin in IoT Hub:</span></span>

1. <span data-ttu-id="3b739-203">Zařízení musí nejprve odběru toohello `$iothub/twin/res/#` tématu tooreceive hello operace odpovědi ze služby IoT Hub.</span><span class="sxs-lookup"><span data-stu-id="3b739-203">A device must first subscribe toohello `$iothub/twin/res/#` topic tooreceive hello operation's responses from IoT Hub.</span></span>

1. <span data-ttu-id="3b739-204">Zařízení odesílá zprávu, která obsahuje hello zařízení twin aktualizace toohello `$iothub/twin/PATCH/properties/reported/?$rid={request id}` tématu.</span><span class="sxs-lookup"><span data-stu-id="3b739-204">A device sends a message that contains hello device twin update toohello `$iothub/twin/PATCH/properties/reported/?$rid={request id}` topic.</span></span> <span data-ttu-id="3b739-205">Tato zpráva obsahuje **id žádosti** hodnotu.</span><span class="sxs-lookup"><span data-stu-id="3b739-205">This message includes a **request id** value.</span></span>

1. <span data-ttu-id="3b739-206">Hello služby pak odešle zprávu odpovědi, která obsahuje hello novou hodnotu ETag hello hlášené vlastnosti kolekce k tématu `$iothub/twin/res/{status}/?$rid={request id}`.</span><span class="sxs-lookup"><span data-stu-id="3b739-206">hello service then sends a response message that contains hello new ETag value for hello reported properties collection on topic `$iothub/twin/res/{status}/?$rid={request id}`.</span></span> <span data-ttu-id="3b739-207">Tuto zprávu s odpovědí hello používá stejný **id žádosti** jako hello požadavku.</span><span class="sxs-lookup"><span data-stu-id="3b739-207">This response message uses hello same **request id** as hello request.</span></span>

<span data-ttu-id="3b739-208">tělo zprávy požadavku Hello obsahuje dokument JSON, který poskytuje nové hodnoty pro hlášené vlastnosti, které (lze upravit žádná vlastnost nebo metadata).</span><span class="sxs-lookup"><span data-stu-id="3b739-208">hello request message body contains a JSON document, which provides new values for reported properties (no other property or metadata can be modified).</span></span>
<span data-ttu-id="3b739-209">Každý člen v dokumentu JSON hello aktualizací nebo přidejte hello odpovídající člen v dokumentu dvojče zařízení hello.</span><span class="sxs-lookup"><span data-stu-id="3b739-209">Each member in hello JSON document updates or add hello corresponding member in hello device twin’s document.</span></span> <span data-ttu-id="3b739-210">Člen nastavit příliš`null`, odstraní hello člena z hello obsahující objektu.</span><span class="sxs-lookup"><span data-stu-id="3b739-210">A member set too`null`, deletes hello member from hello containing object.</span></span> <span data-ttu-id="3b739-211">Například:</span><span class="sxs-lookup"><span data-stu-id="3b739-211">For example:</span></span>

        {
            "telemetrySendFrequency": "35m",
            "batteryLevel": 60
        }

<span data-ttu-id="3b739-212">Hello možné stavové kódy jsou:</span><span class="sxs-lookup"><span data-stu-id="3b739-212">hello possible status codes are:</span></span>

|<span data-ttu-id="3b739-213">Status</span><span class="sxs-lookup"><span data-stu-id="3b739-213">Status</span></span> | <span data-ttu-id="3b739-214">Popis</span><span class="sxs-lookup"><span data-stu-id="3b739-214">Description</span></span> |
| ----- | ----------- |
| <span data-ttu-id="3b739-215">200</span><span class="sxs-lookup"><span data-stu-id="3b739-215">200</span></span> | <span data-ttu-id="3b739-216">Úspěch</span><span class="sxs-lookup"><span data-stu-id="3b739-216">Success</span></span> |
| <span data-ttu-id="3b739-217">400</span><span class="sxs-lookup"><span data-stu-id="3b739-217">400</span></span> | <span data-ttu-id="3b739-218">Chybný požadavek.</span><span class="sxs-lookup"><span data-stu-id="3b739-218">Bad Request.</span></span> <span data-ttu-id="3b739-219">Nesprávný formát JSON</span><span class="sxs-lookup"><span data-stu-id="3b739-219">Malformed JSON</span></span> |
| <span data-ttu-id="3b739-220">429</span><span class="sxs-lookup"><span data-stu-id="3b739-220">429</span></span> | <span data-ttu-id="3b739-221">Příliš mnoho požadavků (omezení), jako za [IoT Hub, omezení šířky pásma][lnk-quotas]</span><span class="sxs-lookup"><span data-stu-id="3b739-221">Too many requests (throttled), as per [IoT Hub throttling][lnk-quotas]</span></span> |
| <span data-ttu-id="3b739-222">5**</span><span class="sxs-lookup"><span data-stu-id="3b739-222">5**</span></span> | <span data-ttu-id="3b739-223">Chyby serveru</span><span class="sxs-lookup"><span data-stu-id="3b739-223">Server errors</span></span> |

<span data-ttu-id="3b739-224">Další informace najdete v tématu [– Příručka vývojáře dvojčata zařízení][lnk-devguide-twin].</span><span class="sxs-lookup"><span data-stu-id="3b739-224">For more information, see [Device twins developer's guide][lnk-devguide-twin].</span></span>

### <a name="receiving-desired-properties-update-notifications"></a><span data-ttu-id="3b739-225">Přijímající oznámení o aktualizaci požadované vlastnosti</span><span class="sxs-lookup"><span data-stu-id="3b739-225">Receiving desired properties update notifications</span></span>

<span data-ttu-id="3b739-226">Když je zařízení připojené, IoT Hub odešle oznámení toohello tématu `$iothub/twin/PATCH/properties/desired/?$version={new version}`, které obsahují obsah hello hello aktualizace provádí back-end hello řešení.</span><span class="sxs-lookup"><span data-stu-id="3b739-226">When a device is connected, IoT Hub sends notifications toohello topic `$iothub/twin/PATCH/properties/desired/?$version={new version}`, which contain hello content of hello update performed by hello solution back end.</span></span> <span data-ttu-id="3b739-227">Například:</span><span class="sxs-lookup"><span data-stu-id="3b739-227">For example:</span></span>

        {
            "telemetrySendFrequency": "5m",
            "route": null
        }

<span data-ttu-id="3b739-228">Jako aktualizace vlastností `null` znamená hodnoty, které hello JSON objektu člen se odstraňuje.</span><span class="sxs-lookup"><span data-stu-id="3b739-228">As for property updates, `null` values means that hello JSON object member is being deleted.</span></span>


> [!IMPORTANT] 
> <span data-ttu-id="3b739-229">IoT Hub generuje oznámení o změnách jenom v případě, že zařízení je připojených.</span><span class="sxs-lookup"><span data-stu-id="3b739-229">IoT Hub generates change notifications only when devices are connected.</span></span> <span data-ttu-id="3b739-230">Ujistěte se, zda text hello tooimplement [toku opětovné připojení zařízení] [ lnk-devguide-twin-reconnection] tookeep hello požadovaných vlastností, které jsou synchronizovány mezi IoT Hub a aplikace hello zařízení.</span><span class="sxs-lookup"><span data-stu-id="3b739-230">Make sure tooimplement hello [device reconnection flow][lnk-devguide-twin-reconnection] tookeep hello desired properties synchronized between IoT Hub and hello device app.</span></span>

<span data-ttu-id="3b739-231">Další informace najdete v tématu [– Příručka vývojáře dvojčata zařízení][lnk-devguide-twin].</span><span class="sxs-lookup"><span data-stu-id="3b739-231">For more information, see [Device twins developer's guide][lnk-devguide-twin].</span></span>

### <a name="respond-tooa-direct-method"></a><span data-ttu-id="3b739-232">Přímá metoda tooa reakce</span><span class="sxs-lookup"><span data-stu-id="3b739-232">Respond tooa direct method</span></span>

<span data-ttu-id="3b739-233">První, zařízení má toosubscribe příliš`$iothub/methods/POST/#`.</span><span class="sxs-lookup"><span data-stu-id="3b739-233">First, a device has toosubscribe too`$iothub/methods/POST/#`.</span></span> <span data-ttu-id="3b739-234">IoT Hub odešle, metoda žádosti toohello tématu `$iothub/methods/POST/{method name}/?$rid={request id}`, buď platný kód JSON nebo prázdným textem zprávy.</span><span class="sxs-lookup"><span data-stu-id="3b739-234">IoT Hub sends method requests toohello topic `$iothub/methods/POST/{method name}/?$rid={request id}`, with either a valid JSON or an empty body.</span></span>

<span data-ttu-id="3b739-235">toorespond, hello zařízení odesílá zprávy s prázdným textem zprávy toohello tématu nebo platný JSON `$iothub/methods/res/{status}/?$rid={request id}`, kde **id žádosti** má toomatch hello jeden ve zprávě požadavku hello, a **stav** má toobe celé číslo .</span><span class="sxs-lookup"><span data-stu-id="3b739-235">toorespond, hello device sends a message with a valid JSON or empty body toohello topic `$iothub/methods/res/{status}/?$rid={request id}`, where **request id** has toomatch hello one in hello request message, and **status** has toobe an integer.</span></span>

<span data-ttu-id="3b739-236">Další informace najdete v tématu [přímé Příručka pro vývojáře metoda][lnk-methods].</span><span class="sxs-lookup"><span data-stu-id="3b739-236">For more information, see [Direct method developer's guide][lnk-methods].</span></span>

### <a name="additional-considerations"></a><span data-ttu-id="3b739-237">Další aspekty</span><span class="sxs-lookup"><span data-stu-id="3b739-237">Additional considerations</span></span>

<span data-ttu-id="3b739-238">Jako poslední zabývat, pokud potřebujete toocustomize hello MQTT chování protokolu na straně cloudu hello, byste měli zkontrolovat hello [brány protokolu Azure IoT] [ lnk-azure-protocol-gateway] , která umožní toodeploy vysoce výkonné vlastní protokol brána, která rozhraní přímo službou IoT Hub.</span><span class="sxs-lookup"><span data-stu-id="3b739-238">As a final consideration, if you need toocustomize hello MQTT protocol behavior on hello cloud side, you should review hello [Azure IoT protocol gateway][lnk-azure-protocol-gateway] that enables you toodeploy a high-performance custom protocol gateway that interfaces directly with IoT Hub.</span></span> <span data-ttu-id="3b739-239">brány protokolu Azure IoT Hello umožňuje vám toocustomize hello zařízení protokol tooaccommodate brownfield MQTT nasazení nebo jiné vlastní protokoly.</span><span class="sxs-lookup"><span data-stu-id="3b739-239">hello Azure IoT protocol gateway enables you toocustomize hello device protocol tooaccommodate brownfield MQTT deployments or other custom protocols.</span></span> <span data-ttu-id="3b739-240">Tento přístup, ale nevyžaduje spouštět a provozovat bránu vlastního protokolu.</span><span class="sxs-lookup"><span data-stu-id="3b739-240">This approach does require, however, that you run and operate a custom protocol gateway.</span></span>

## <a name="next-steps"></a><span data-ttu-id="3b739-241">Další kroky</span><span class="sxs-lookup"><span data-stu-id="3b739-241">Next steps</span></span>

<span data-ttu-id="3b739-242">toolearn Další informace o protokolu MQTT hello, najdete v části hello [MQTT dokumentace][lnk-mqtt-docs].</span><span class="sxs-lookup"><span data-stu-id="3b739-242">toolearn more about hello MQTT protocol, see hello [MQTT documentation][lnk-mqtt-docs].</span></span>

<span data-ttu-id="3b739-243">toolearn Další informace o plánování nasazení služby IoT Hub, najdete v části:</span><span class="sxs-lookup"><span data-stu-id="3b739-243">toolearn more about planning your IoT Hub deployment, see:</span></span>

* <span data-ttu-id="3b739-244">[Azure certifikované pro katalog zařízení IoT][lnk-devices]</span><span class="sxs-lookup"><span data-stu-id="3b739-244">[Azure Certified for IoT device catalog][lnk-devices]</span></span>
* <span data-ttu-id="3b739-245">[Podpora dalších protokolů][lnk-protocols]</span><span class="sxs-lookup"><span data-stu-id="3b739-245">[Support additional protocols][lnk-protocols]</span></span>
* <span data-ttu-id="3b739-246">[Porovnání s služby Event Hubs][lnk-compare]</span><span class="sxs-lookup"><span data-stu-id="3b739-246">[Compare with Event Hubs][lnk-compare]</span></span>
* <span data-ttu-id="3b739-247">[Změna velikosti, HA a zotavení po Havárii][lnk-scaling]</span><span class="sxs-lookup"><span data-stu-id="3b739-247">[Scaling, HA, and DR][lnk-scaling]</span></span>

<span data-ttu-id="3b739-248">toofurther prozkoumat hello služby IoT Hub, najdete v tématu:</span><span class="sxs-lookup"><span data-stu-id="3b739-248">toofurther explore hello capabilities of IoT Hub, see:</span></span>

* <span data-ttu-id="3b739-249">[Příručka vývojáře pro službu IoT Hub][lnk-devguide]</span><span class="sxs-lookup"><span data-stu-id="3b739-249">[IoT Hub developer guide][lnk-devguide]</span></span>
* <span data-ttu-id="3b739-250">[Simulaci zařízení s Azure IoT Edge][lnk-iotedge]</span><span class="sxs-lookup"><span data-stu-id="3b739-250">[Simulating a device with Azure IoT Edge][lnk-iotedge]</span></span>

[lnk-device-sdks]: https://github.com/Azure/azure-iot-sdks
[lnk-mqtt-org]: http://mqtt.org/
[lnk-mqtt-docs]: http://mqtt.org/documentation
[lnk-sample-node]: https://github.com/Azure/azure-iot-sdk-node/blob/master/device/samples/simple_sample_device.js
[lnk-sample-java]: https://github.com/Azure/azure-iot-sdk-java/tree/master/device/samples/send-receive-sample/src/main/java/samples/com/microsoft/azure/iothub/SendReceive.java
[lnk-sample-c]: https://github.com/Azure/azure-iot-sdk-c/tree/master/iothub_client/samples/iothub_client_sample_mqtt
[lnk-sample-csharp]: https://github.com/Azure/azure-iot-sdk-csharp/tree/master/device/samples
[lnk-sample-python]: https://github.com/Azure/azure-iot-sdk-python/tree/master/device/samples
[lnk-device-explorer]: https://github.com/Azure/azure-iot-sdk-csharp/blob/master/tools/DeviceExplorer
[lnk-sas-tokens]: iot-hub-devguide-security.md#use-sas-tokens-in-a-device-app
[lnk-azure-protocol-gateway]: iot-hub-protocol-gateway.md

[lnk-devices]: https://catalog.azureiotsuite.com/
[lnk-protocols]: iot-hub-protocol-gateway.md
[lnk-compare]: iot-hub-compare-event-hubs.md
[lnk-scaling]: iot-hub-scaling.md
[lnk-devguide]: iot-hub-devguide.md
[lnk-iotedge]: iot-hub-linux-iot-edge-simulated-device.md

[lnk-methods]: iot-hub-devguide-direct-methods.md
[lnk-messaging]: iot-hub-devguide-messaging.md

[lnk-quotas]: iot-hub-devguide-quotas-throttling.md
[lnk-devguide-twin-reconnection]: iot-hub-devguide-device-twins.md#device-reconnection-flow
[lnk-devguide-twin]: iot-hub-devguide-device-twins.md
