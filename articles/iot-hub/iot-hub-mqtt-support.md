---
title: "Pochopení podpory Azure IoT Hub MQTT | Microsoft Docs"
description: "Příručka vývojáře – podpora pro zařízení připojující se k zařízení směřujících koncový bod služby IoT Hub pomocí protokolu MQTT. Obsahuje informace o předdefinovaných MQTT podporovat v sady SDK zařízení Azure IoT."
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
ms.openlocfilehash: eab70c1aa9c49a137c2ac1012449d57915fb0d27
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="communicate-with-your-iot-hub-using-the-mqtt-protocol"></a><span data-ttu-id="61539-104">Komunikovat se službou IoT hub pomocí protokolu MQTT</span><span class="sxs-lookup"><span data-stu-id="61539-104">Communicate with your IoT hub using the MQTT protocol</span></span>

<span data-ttu-id="61539-105">Umožňuje zařízením komunikovat s koncovými body zařízení IoT Hub pomocí služby IoT Hub [protokoly MQTT v3.1.1] [ lnk-mqtt-org] protokolu na port 8883 nebo protokoly MQTT v3.1.1 přes protokol WebSocket na portu 443.</span><span class="sxs-lookup"><span data-stu-id="61539-105">IoT Hub enables devices to communicate with the IoT Hub device endpoints using the [MQTT v3.1.1][lnk-mqtt-org] protocol on port 8883 or MQTT v3.1.1 over WebSocket protocol on port 443.</span></span> <span data-ttu-id="61539-106">IoT Hub vyžaduje veškerá komunikace zařízení být zabezpečené pomocí protokolu TLS/SSL (proto IoT Hub nepodporuje nezabezpečené připojení přes port 1883).</span><span class="sxs-lookup"><span data-stu-id="61539-106">IoT Hub requires all device communication to be secured using TLS/SSL (hence, IoT Hub doesn’t support non-secure connections over port 1883).</span></span>

## <a name="connecting-to-iot-hub"></a><span data-ttu-id="61539-107">Připojení ke službě IoT Hub</span><span class="sxs-lookup"><span data-stu-id="61539-107">Connecting to IoT Hub</span></span>

<span data-ttu-id="61539-108">Zařízení můžete použít protokol MQTT pro připojení do služby IoT hub, buď pomocí knihovny v [SDK služby Azure IoT] [ lnk-device-sdks] nebo přímo pomocí MQTT protokolu.</span><span class="sxs-lookup"><span data-stu-id="61539-108">A device can use the MQTT protocol to connect to an IoT hub either by using the libraries in the [Azure IoT SDKs][lnk-device-sdks] or by using the MQTT protocol directly.</span></span>

## <a name="using-the-device-sdks"></a><span data-ttu-id="61539-109">Pomocí sady SDK pro zařízení</span><span class="sxs-lookup"><span data-stu-id="61539-109">Using the device SDKs</span></span>

<span data-ttu-id="61539-110">[Sady SDK zařízení] [ lnk-device-sdks] podporující protokol MQTT jsou k dispozici pro Java, Node.js, C, C# a Python.</span><span class="sxs-lookup"><span data-stu-id="61539-110">[Device SDKs][lnk-device-sdks] that support the MQTT protocol are available for Java, Node.js, C, C#, and Python.</span></span> <span data-ttu-id="61539-111">Sady SDK zařízení použít standardní připojovací řetězec služby IoT Hub navázat připojení do služby IoT hub.</span><span class="sxs-lookup"><span data-stu-id="61539-111">The device SDKs use the standard IoT Hub connection string to establish a connection to an IoT hub.</span></span> <span data-ttu-id="61539-112">Chcete-li používat protokol MQTT, musí být parametr protokol klienta nastavena **MQTT**.</span><span class="sxs-lookup"><span data-stu-id="61539-112">To use the MQTT protocol, the client protocol parameter must be set to **MQTT**.</span></span> <span data-ttu-id="61539-113">Ve výchozím nastavení, zařízení sady SDK připojení do služby IoT Hub s **CleanSession** příznak nastaven na hodnotu **0** a používat **QoS 1** k výměně zpráv službou IoT hub.</span><span class="sxs-lookup"><span data-stu-id="61539-113">By default, the device SDKs connect to an IoT Hub with the **CleanSession** flag set to **0** and use **QoS 1** for message exchange with the IoT hub.</span></span>

<span data-ttu-id="61539-114">Když je zařízení připojené ke službě IoT hub, sady SDK zařízení poskytují metody, které umožní zařízení zprávy odesílat a přijímat zprávy ze služby IoT hub.</span><span class="sxs-lookup"><span data-stu-id="61539-114">When a device is connected to an IoT hub, the device SDKs provide methods that enable the device to send messages to and receive messages from an IoT hub.</span></span>

<span data-ttu-id="61539-115">Následující tabulka obsahuje odkazy na ukázky kódu pro každý podporovaný jazyk a určuje parametr, který chcete použít k navázání připojení ke službě IoT Hub pomocí protokolu MQTT.</span><span class="sxs-lookup"><span data-stu-id="61539-115">The following table contains links to code samples for each supported language and specifies the parameter to use to establish a connection to IoT Hub using the MQTT protocol.</span></span>

| <span data-ttu-id="61539-116">Jazyk</span><span class="sxs-lookup"><span data-stu-id="61539-116">Language</span></span> | <span data-ttu-id="61539-117">Parametr protokolu</span><span class="sxs-lookup"><span data-stu-id="61539-117">Protocol parameter</span></span> |
| --- | --- |
| <span data-ttu-id="61539-118">[Node.js][lnk-sample-node]</span><span class="sxs-lookup"><span data-stu-id="61539-118">[Node.js][lnk-sample-node]</span></span> |<span data-ttu-id="61539-119">Azure-iot zařízení mqtt</span><span class="sxs-lookup"><span data-stu-id="61539-119">azure-iot-device-mqtt</span></span> |
| <span data-ttu-id="61539-120">[Java][lnk-sample-java]</span><span class="sxs-lookup"><span data-stu-id="61539-120">[Java][lnk-sample-java]</span></span> |<span data-ttu-id="61539-121">IotHubClientProtocol.MQTT</span><span class="sxs-lookup"><span data-stu-id="61539-121">IotHubClientProtocol.MQTT</span></span> |
| <span data-ttu-id="61539-122">[C][lnk-sample-c]</span><span class="sxs-lookup"><span data-stu-id="61539-122">[C][lnk-sample-c]</span></span> |<span data-ttu-id="61539-123">MQTT_Protocol</span><span class="sxs-lookup"><span data-stu-id="61539-123">MQTT_Protocol</span></span> |
| <span data-ttu-id="61539-124">[C#][lnk-sample-csharp]</span><span class="sxs-lookup"><span data-stu-id="61539-124">[C#][lnk-sample-csharp]</span></span> |<span data-ttu-id="61539-125">TransportType.Mqtt</span><span class="sxs-lookup"><span data-stu-id="61539-125">TransportType.Mqtt</span></span> |
| <span data-ttu-id="61539-126">[Python][lnk-sample-python]</span><span class="sxs-lookup"><span data-stu-id="61539-126">[Python][lnk-sample-python]</span></span> |<span data-ttu-id="61539-127">IoTHubTransportProvider.MQTT</span><span class="sxs-lookup"><span data-stu-id="61539-127">IoTHubTransportProvider.MQTT</span></span> |

### <a name="migrating-a-device-app-from-amqp-to-mqtt"></a><span data-ttu-id="61539-128">Migrace aplikace na zařízení z AMQP na MQTT</span><span class="sxs-lookup"><span data-stu-id="61539-128">Migrating a device app from AMQP to MQTT</span></span>

<span data-ttu-id="61539-129">Pokud používáte [sady SDK pro zařízení][lnk-device-sdks], přepínání pomocí protokolu AMQP k MQTT potřeba změnit parametr protokolu v inicializace klienta, jak bylo uvedeno dříve.</span><span class="sxs-lookup"><span data-stu-id="61539-129">If you are using the [device SDKs][lnk-device-sdks], switching from using AMQP to MQTT requires changing the protocol parameter in the client initialization as stated previously.</span></span>

<span data-ttu-id="61539-130">Když to uděláte, nezapomeňte zaškrtnout následující položky:</span><span class="sxs-lookup"><span data-stu-id="61539-130">When doing so, make sure to check the following items:</span></span>

* <span data-ttu-id="61539-131">AMQP vrátí chyby pro mnoho podmínek, zatímco MQTT ukončí připojení.</span><span class="sxs-lookup"><span data-stu-id="61539-131">AMQP returns errors for many conditions, while MQTT terminates the connection.</span></span> <span data-ttu-id="61539-132">V důsledku vaše zpracování logiky výjimek může vyžadovat některé změny.</span><span class="sxs-lookup"><span data-stu-id="61539-132">As a result your exception handling logic might require some changes.</span></span>
* <span data-ttu-id="61539-133">MQTT nepodporuje *odmítnout* operace při přijímání [zprávy typu cloud zařízení][lnk-messaging].</span><span class="sxs-lookup"><span data-stu-id="61539-133">MQTT does not support the *reject* operations when receiving [cloud-to-device messages][lnk-messaging].</span></span> <span data-ttu-id="61539-134">Pokud vaše aplikace back-end musí, obdrží odpověď z aplikace zařízení, zvažte použití [přímé metody][lnk-methods].</span><span class="sxs-lookup"><span data-stu-id="61539-134">If your back-end app needs to receive a response from the device app, consider using [direct methods][lnk-methods].</span></span>

## <a name="using-the-mqtt-protocol-directly"></a><span data-ttu-id="61539-135">Přímo pomocí protokolu MQTT</span><span class="sxs-lookup"><span data-stu-id="61539-135">Using the MQTT protocol directly</span></span>
<span data-ttu-id="61539-136">Pokud zařízení nelze použít sady SDK pro zařízení, můžete pořád připojit ke koncovým bodům veřejné zařízení pomocí protokolu MQTT na portu 8883.</span><span class="sxs-lookup"><span data-stu-id="61539-136">If a device cannot use the device SDKs, it can still connect to the public device endpoints using the MQTT protocol on port 8883.</span></span> <span data-ttu-id="61539-137">V **CONNECT** paketu zařízení by měl použijte následující hodnoty:</span><span class="sxs-lookup"><span data-stu-id="61539-137">In the **CONNECT** packet the device should use the following values:</span></span>

* <span data-ttu-id="61539-138">Pro **ClientId** pole, použijte **deviceId**.</span><span class="sxs-lookup"><span data-stu-id="61539-138">For the **ClientId** field, use the **deviceId**.</span></span>
* <span data-ttu-id="61539-139">Pro **uživatelské jméno** pole, použijte `{iothubhostname}/{device_id}/api-version=2016-11-14`, kde {iothubhostname} je úplná CName služby IoT hub.</span><span class="sxs-lookup"><span data-stu-id="61539-139">For the **Username** field, use `{iothubhostname}/{device_id}/api-version=2016-11-14`, where {iothubhostname} is the full CName of the IoT hub.</span></span>

    <span data-ttu-id="61539-140">Například, pokud je název služby IoT hub **contoso.azure devices.net** a pokud je název vašeho zařízení **MyDevice01**, kompletní **uživatelské jméno** pole musí obsahovat `contoso.azure-devices.net/MyDevice01/api-version=2016-11-14`.</span><span class="sxs-lookup"><span data-stu-id="61539-140">For example, if the name of your IoT hub is **contoso.azure-devices.net** and if the name of your device is **MyDevice01**, the full **Username** field should contain `contoso.azure-devices.net/MyDevice01/api-version=2016-11-14`.</span></span>
* <span data-ttu-id="61539-141">Pro **heslo** pole, použijte SAS token.</span><span class="sxs-lookup"><span data-stu-id="61539-141">For the **Password** field, use a SAS token.</span></span> <span data-ttu-id="61539-142">Formát tokenu SAS je stejné jako pro protokoly HTTP i AMQP:</span><span class="sxs-lookup"><span data-stu-id="61539-142">The format of the SAS token is the same as for both the HTTP and AMQP protocols:</span></span><br/><span data-ttu-id="61539-143">`SharedAccessSignature sig={signature-string}&se={expiry}&sr={URL-encoded-resourceURI}`.</span><span class="sxs-lookup"><span data-stu-id="61539-143">`SharedAccessSignature sig={signature-string}&se={expiry}&sr={URL-encoded-resourceURI}`.</span></span>

    <span data-ttu-id="61539-144">Další informace o tom, jak generovat tokeny SAS, najdete v části zařízení [tokeny zabezpečení pomocí služby IoT Hub][lnk-sas-tokens].</span><span class="sxs-lookup"><span data-stu-id="61539-144">For more information about how to generate SAS tokens, see the device section of [Using IoT Hub security tokens][lnk-sas-tokens].</span></span>

    <span data-ttu-id="61539-145">Při testování, můžete také použít [explorer zařízení] [ lnk-device-explorer] nástroj, který rychle vytvořit token SAS, který můžete zkopírovat a vložit do vlastního kódu:</span><span class="sxs-lookup"><span data-stu-id="61539-145">When testing, you can also use the [device explorer][lnk-device-explorer] tool to quickly generate a SAS token that you can copy and paste into your own code:</span></span>

  1. <span data-ttu-id="61539-146">Přejděte na **správy** kartě v **Explorer zařízení**.</span><span class="sxs-lookup"><span data-stu-id="61539-146">Go to the **Management** tab in **Device Explorer**.</span></span>
  2. <span data-ttu-id="61539-147">Klikněte na tlačítko **tokenu SAS** (pravého horního).</span><span class="sxs-lookup"><span data-stu-id="61539-147">Click **SAS Token** (top right).</span></span>
  3. <span data-ttu-id="61539-148">Na **SASTokenForm**, vyberte své zařízení do **DeviceID** rozevírací nabídku.</span><span class="sxs-lookup"><span data-stu-id="61539-148">On **SASTokenForm**, select your device in the **DeviceID** drop down.</span></span> <span data-ttu-id="61539-149">Nastavit vaše **TTL**.</span><span class="sxs-lookup"><span data-stu-id="61539-149">Set your **TTL**.</span></span>
  4. <span data-ttu-id="61539-150">Klikněte na tlačítko **generování** k vytvoření vašeho tokenu.</span><span class="sxs-lookup"><span data-stu-id="61539-150">Click **Generate** to create your token.</span></span>

     <span data-ttu-id="61539-151">Token SAS, který se vygeneruje má tuto strukturu: `HostName={your hub name}.azure-devices.net;DeviceId=javadevice;SharedAccessSignature=SharedAccessSignature sr={your hub name}.azure-devices.net%2Fdevices%2FMyDevice01%2Fapi-version%3D2016-11-14&sig=vSgHBMUG.....Ntg%3d&se=1456481802`.</span><span class="sxs-lookup"><span data-stu-id="61539-151">The SAS token that's generated has this structure: `HostName={your hub name}.azure-devices.net;DeviceId=javadevice;SharedAccessSignature=SharedAccessSignature sr={your hub name}.azure-devices.net%2Fdevices%2FMyDevice01%2Fapi-version%3D2016-11-14&sig=vSgHBMUG.....Ntg%3d&se=1456481802`.</span></span>

     <span data-ttu-id="61539-152">Součást tento token, který bude použit jako **heslo** pole a připojte se pomocí MQTT: `SharedAccessSignature sr={your hub name}.azure-devices.net%2Fdevices%2FMyDevice01%2Fapi-version%3D2016-11-14&sig=vSgHBMUG.....Ntg%3d&se=1456481802`.</span><span class="sxs-lookup"><span data-stu-id="61539-152">The part of this token to use as the **Password** field to connect using MQTT is: `SharedAccessSignature sr={your hub name}.azure-devices.net%2Fdevices%2FMyDevice01%2Fapi-version%3D2016-11-14&sig=vSgHBMUG.....Ntg%3d&se=1456481802`.</span></span>

<span data-ttu-id="61539-153">MQTT připojit a odpojit paketů, IoT Hub vydá událost na **monitorování Operations** kanál s další informace, které vám může pomoct vyřešit problémy s připojením.</span><span class="sxs-lookup"><span data-stu-id="61539-153">For MQTT connect and disconnect packets, IoT Hub issues an event on the **Operations Monitoring** channel with additional information that can help you to troubleshoot connectivity issues.</span></span>

### <a name="sending-device-to-cloud-messages"></a><span data-ttu-id="61539-154">Odesílání zpráv typu zařízení cloud</span><span class="sxs-lookup"><span data-stu-id="61539-154">Sending device-to-cloud messages</span></span>

<span data-ttu-id="61539-155">Po provedení úspěšného připojení, zařízení mohou zasílat zprávy do služby IoT Hub pomocí `devices/{device_id}/messages/events/` nebo `devices/{device_id}/messages/events/{property_bag}` jako **název tématu**.</span><span class="sxs-lookup"><span data-stu-id="61539-155">After making a successful connection, a device can send messages to IoT Hub using `devices/{device_id}/messages/events/` or `devices/{device_id}/messages/events/{property_bag}` as a **Topic Name**.</span></span> <span data-ttu-id="61539-156">`{property_bag}` Element povoluje, aby zařízení k odesílání zpráv bez dalších vlastností ve formátu kódovaná adresou url.</span><span class="sxs-lookup"><span data-stu-id="61539-156">The `{property_bag}` element enables the device to send messages with additional properties in a url-encoded format.</span></span> <span data-ttu-id="61539-157">Například:</span><span class="sxs-lookup"><span data-stu-id="61539-157">For example:</span></span>

```
RFC 2396-encoded(<PropertyName1>)=RFC 2396-encoded(<PropertyValue1>)&RFC 2396-encoded(<PropertyName2>)=RFC 2396-encoded(<PropertyValue2>)…
```

> [!NOTE]
> <span data-ttu-id="61539-158">To `{property_bag}` element používá stejné kódování jako řetězce dotazů v protokolu HTTP.</span><span class="sxs-lookup"><span data-stu-id="61539-158">This `{property_bag}` element uses the same encoding as for query strings in the HTTP protocol.</span></span>
>
>

<span data-ttu-id="61539-159">Aplikace zařízení můžete také použít `devices/{device_id}/messages/events/{property_bag}` jako **název tématu bude** k definování *bude zprávy* k přeposlání jako zprávu telemetrie.</span><span class="sxs-lookup"><span data-stu-id="61539-159">The device app can also use `devices/{device_id}/messages/events/{property_bag}` as the **Will topic name** to define *Will messages* to be forwarded as a telemetry message.</span></span>

- <span data-ttu-id="61539-160">IoT Hub nepodporuje QoS 2 zprávy.</span><span class="sxs-lookup"><span data-stu-id="61539-160">IoT Hub does not support QoS 2 messages.</span></span> <span data-ttu-id="61539-161">Pokud aplikace na zařízení publikuje zprávu s **QoS 2**, IoT Hub ukončí síťové připojení.</span><span class="sxs-lookup"><span data-stu-id="61539-161">If a device app publishes a message with **QoS 2**, IoT Hub closes the network connection.</span></span>
- <span data-ttu-id="61539-162">IoT Hub není zachována zachovat zprávy.</span><span class="sxs-lookup"><span data-stu-id="61539-162">IoT Hub does not persist Retain messages.</span></span> <span data-ttu-id="61539-163">Pokud zařízení odesílá zprávy s **zachovat** příznak nastaven na hodnotu 1, přidá IoT Hub **x-opt zachovat** vlastnosti aplikace ke zprávě.</span><span class="sxs-lookup"><span data-stu-id="61539-163">If a device sends a message with the **RETAIN** flag set to 1, IoT Hub adds the **x-opt-retain** application property to the message.</span></span> <span data-ttu-id="61539-164">V takovém případě místo uchování zpráv zachovat, IoT Hub předává je na back-end aplikace.</span><span class="sxs-lookup"><span data-stu-id="61539-164">In this case, instead of persisting the retain message, IoT Hub passes it to the backend app.</span></span>
- <span data-ttu-id="61539-165">IoT Hub podporuje pouze jeden aktivní připojení MQTT na jedno zařízení.</span><span class="sxs-lookup"><span data-stu-id="61539-165">IoT Hub only supports one active MQTT connection per device.</span></span> <span data-ttu-id="61539-166">Všechny nové připojení MQTT jménem stejné ID zařízení způsobí, že IoT Hub, chcete-li vyřadit existující připojení.</span><span class="sxs-lookup"><span data-stu-id="61539-166">Any new MQTT connection on behalf of the same device ID causes IoT Hub to drop the existing connection.</span></span>

<span data-ttu-id="61539-167">Další informace najdete v tématu [– Příručka vývojáře pro zasílání zpráv][lnk-messaging].</span><span class="sxs-lookup"><span data-stu-id="61539-167">For more information, see [Messaging developer's guide][lnk-messaging].</span></span>

### <a name="receiving-cloud-to-device-messages"></a><span data-ttu-id="61539-168">Příjem zpráv typu cloud zařízení</span><span class="sxs-lookup"><span data-stu-id="61539-168">Receiving cloud-to-device messages</span></span>

<span data-ttu-id="61539-169">Pro příjem zpráv ze služby IoT Hub, musí zařízení přihlásit se pomocí `devices/{device_id}/messages/devicebound/#` jako **tématu filtru**.</span><span class="sxs-lookup"><span data-stu-id="61539-169">To receive messages from IoT Hub, a device should subscribe using `devices/{device_id}/messages/devicebound/#` as a **Topic Filter**.</span></span> <span data-ttu-id="61539-170">Víceúrovňovou zástupného  **#**  ve filtru téma, které se používají pouze pro umožňuje zařízení přijímat další vlastnosti v názvu tématu.</span><span class="sxs-lookup"><span data-stu-id="61539-170">The multi-level wildcard **#** in the Topic Filter is used only to allow the device to receive additional properties in the topic name.</span></span> <span data-ttu-id="61539-171">IoT Hub neumožňuje použití  **#**  nebo **?**</span><span class="sxs-lookup"><span data-stu-id="61539-171">IoT Hub does not allow the usage of the **#** or **?**</span></span> <span data-ttu-id="61539-172">zástupné znaky pro filtrování dílčí témata.</span><span class="sxs-lookup"><span data-stu-id="61539-172">wildcards for filtering of sub-topics.</span></span> <span data-ttu-id="61539-173">Vzhledem k tomu, že Centrum IoT není zprostředkovatel zasílání zpráv protokol pub-sub obecné účely, podporuje pouze zdokumentovaných tématu názvy a téma filtry.</span><span class="sxs-lookup"><span data-stu-id="61539-173">Since IoT Hub is not a general purpose pub-sub messaging broker, it only supports the documented topic names and topic filters.</span></span>

<span data-ttu-id="61539-174">Zařízení neobdrží všechny zprávy ze služby IoT Hub, dokud ho se úspěšně připojila ke koncovému bodu jeho konkrétní zařízení, reprezentována `devices/{device_id}/messages/devicebound/#` tématu filtru.</span><span class="sxs-lookup"><span data-stu-id="61539-174">The device does not receive any messages from IoT Hub, until it has successfully subscribed to its device-specific endpoint, represented by the `devices/{device_id}/messages/devicebound/#` topic filter.</span></span> <span data-ttu-id="61539-175">Po úspěšné odběru bylo úspěšně navázáno, zařízení se spustí, přijímá jen zprávy cloud zařízení, které byly odeslány do ní po čase předplatného.</span><span class="sxs-lookup"><span data-stu-id="61539-175">After successful subscription has been established, the device starts receiving only cloud-to-device messages that have been sent to it after the time of the subscription.</span></span> <span data-ttu-id="61539-176">Pokud se zařízení připojuje s **CleanSession** příznak nastaven na hodnotu **0**, odběr je trvalé v jiných relacích.</span><span class="sxs-lookup"><span data-stu-id="61539-176">If the device connects with **CleanSession** flag set to **0**, the subscription is persisted across different sessions.</span></span> <span data-ttu-id="61539-177">V takovém případě při příštím připojení s **CleanSession 0** zařízení obdrží nezpracovaných zprávy, které byly odeslány do ní době, kdy byl odpojen.</span><span class="sxs-lookup"><span data-stu-id="61539-177">In this case, the next time it connects with **CleanSession 0** the device receives outstanding messages that have been sent to it while it was disconnected.</span></span> <span data-ttu-id="61539-178">Pokud zařízení používá **CleanSession** příznak nastaven na hodnotu **1** ale, že neobdrží všechny zprávy ze služby IoT Hub až přihlásí se k jeho zařízení koncového bodu.</span><span class="sxs-lookup"><span data-stu-id="61539-178">If the device uses **CleanSession** flag set to **1** though, it does not receive any messages from IoT Hub until it subscribes to its device-endpoint.</span></span>

<span data-ttu-id="61539-179">IoT Hub zajišťuje zprávy s **název tématu** `devices/{device_id}/messages/devicebound/`, nebo `devices/{device_id}/messages/devicebound/{property_bag}` Pokud nejsou k dispozici žádné vlastnosti zprávy.</span><span class="sxs-lookup"><span data-stu-id="61539-179">IoT Hub delivers messages with the **Topic Name** `devices/{device_id}/messages/devicebound/`, or `devices/{device_id}/messages/devicebound/{property_bag}` if there are any message properties.</span></span> <span data-ttu-id="61539-180">`{property_bag}`obsahuje dvojice klíč/hodnota kódovaná adresou url vlastností zpráv.</span><span class="sxs-lookup"><span data-stu-id="61539-180">`{property_bag}` contains url-encoded key/value pairs of message properties.</span></span> <span data-ttu-id="61539-181">Pouze vlastnosti aplikace a vlastnosti uživatele nastavit systému (například **messageId** nebo **correlationId**) jsou zahrnuty v kontejneru objektů.</span><span class="sxs-lookup"><span data-stu-id="61539-181">Only application properties and user-settable system properties (such as **messageId** or **correlationId**) are included in the property bag.</span></span> <span data-ttu-id="61539-182">Názvy vlastností systému mají předponu  **$** , vlastnosti aplikace pomocí žádná předpona. původní název vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="61539-182">System property names have the prefix **$**, application properties use the original property name with no prefix.</span></span>

<span data-ttu-id="61539-183">Když aplikace na zařízení předplatila téma se **QoS 2**, IoT Hub uděluje maximální QoS úrovně 1 v **SUBACK** paketů.</span><span class="sxs-lookup"><span data-stu-id="61539-183">When a device app subscribes to a topic with **QoS 2**, IoT Hub grants maximum QoS level 1 in the **SUBACK** packet.</span></span> <span data-ttu-id="61539-184">Potom IoT Hub do zařízení pomocí technologie QoS 1 dodává zprávy.</span><span class="sxs-lookup"><span data-stu-id="61539-184">After that, IoT Hub delivers messages to the device using QoS 1.</span></span>

### <a name="retrieving-a-device-twins-properties"></a><span data-ttu-id="61539-185">Načítání vlastností dvojče zařízení</span><span class="sxs-lookup"><span data-stu-id="61539-185">Retrieving a device twin's properties</span></span>

<span data-ttu-id="61539-186">Nejprve přihlásí k odběru zařízení `$iothub/twin/res/#`, pro příjem odpovědí operaci.</span><span class="sxs-lookup"><span data-stu-id="61539-186">First, a device subscribes to `$iothub/twin/res/#`, to receive the operation's responses.</span></span> <span data-ttu-id="61539-187">Pak se odešle zprávu o prázdný tématu `$iothub/twin/GET/?$rid={request id}`, s hodnotou vyplněná **id žádosti**.</span><span class="sxs-lookup"><span data-stu-id="61539-187">Then, it sends an empty message to topic `$iothub/twin/GET/?$rid={request id}`, with a populated value for **request id**.</span></span> <span data-ttu-id="61539-188">Služba pak odešle zprávu odpovědi, která obsahuje data twin zařízení k tématu `$iothub/twin/res/{status}/?$rid={request id}`, používající stejný **id žádosti** jako požadavek.</span><span class="sxs-lookup"><span data-stu-id="61539-188">The service then sends a response message containing the device twin data on topic `$iothub/twin/res/{status}/?$rid={request id}`, using the same **request id** as the request.</span></span>

<span data-ttu-id="61539-189">Id žádosti může být libovolná platná hodnota pro hodnotu vlastnosti zprávy dle [IoT Hub – Příručka vývojáře pro zasílání zpráv][lnk-messaging], a stav byl ověřen jako celé číslo.</span><span class="sxs-lookup"><span data-stu-id="61539-189">Request id can be any valid value for a message property value, as per [IoT Hub messaging developer's guide][lnk-messaging], and status is validated as an integer.</span></span>
<span data-ttu-id="61539-190">Text odpovědi obsahuje sekce properties dvojče zařízení:</span><span class="sxs-lookup"><span data-stu-id="61539-190">The response body contains the properties section of the device twin:</span></span>

<span data-ttu-id="61539-191">Text položky registru identit omezené na člen "vlastnosti", například:</span><span class="sxs-lookup"><span data-stu-id="61539-191">The body of the identity registry entry limited to the “properties” member, for example:</span></span>

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

<span data-ttu-id="61539-192">Možné stavové kódy jsou:</span><span class="sxs-lookup"><span data-stu-id="61539-192">The possible status codes are:</span></span>

|<span data-ttu-id="61539-193">Status</span><span class="sxs-lookup"><span data-stu-id="61539-193">Status</span></span> | <span data-ttu-id="61539-194">Popis</span><span class="sxs-lookup"><span data-stu-id="61539-194">Description</span></span> |
| ----- | ----------- |
| <span data-ttu-id="61539-195">200</span><span class="sxs-lookup"><span data-stu-id="61539-195">200</span></span> | <span data-ttu-id="61539-196">Úspěch</span><span class="sxs-lookup"><span data-stu-id="61539-196">Success</span></span> |
| <span data-ttu-id="61539-197">429</span><span class="sxs-lookup"><span data-stu-id="61539-197">429</span></span> | <span data-ttu-id="61539-198">Příliš mnoho požadavků (omezení), jako za [IoT Hub, omezení šířky pásma][lnk-quotas]</span><span class="sxs-lookup"><span data-stu-id="61539-198">Too many requests (throttled), as per [IoT Hub throttling][lnk-quotas]</span></span> |
| <span data-ttu-id="61539-199">5**</span><span class="sxs-lookup"><span data-stu-id="61539-199">5**</span></span> | <span data-ttu-id="61539-200">Chyby serveru</span><span class="sxs-lookup"><span data-stu-id="61539-200">Server errors</span></span> |

<span data-ttu-id="61539-201">Další informace najdete v tématu [– Příručka vývojáře dvojčata zařízení][lnk-devguide-twin].</span><span class="sxs-lookup"><span data-stu-id="61539-201">For more information, see [Device twins developer's guide][lnk-devguide-twin].</span></span>

### <a name="update-device-twins-reported-properties"></a><span data-ttu-id="61539-202">Aktualizovat vlastnosti hlášené dvojče zařízení</span><span class="sxs-lookup"><span data-stu-id="61539-202">Update device twin's reported properties</span></span>

<span data-ttu-id="61539-203">Následující text popisuje, jak zařízení aktualizuje vlastnosti na hlášené dvojče zařízení IoT hub:</span><span class="sxs-lookup"><span data-stu-id="61539-203">The following sequence describes how a device updates the reported properties in the device twin in IoT Hub:</span></span>

1. <span data-ttu-id="61539-204">Zařízení musí nejdřív přihlásit k odběru `$iothub/twin/res/#` tématu pro příjem odpovědí operaci ze služby IoT Hub.</span><span class="sxs-lookup"><span data-stu-id="61539-204">A device must first subscribe to the `$iothub/twin/res/#` topic to receive the operation's responses from IoT Hub.</span></span>

1. <span data-ttu-id="61539-205">Zařízení odesílá zprávu, která obsahuje aktualizace twin zařízení, která se `$iothub/twin/PATCH/properties/reported/?$rid={request id}` tématu.</span><span class="sxs-lookup"><span data-stu-id="61539-205">A device sends a message that contains the device twin update to the `$iothub/twin/PATCH/properties/reported/?$rid={request id}` topic.</span></span> <span data-ttu-id="61539-206">Tato zpráva obsahuje **id žádosti** hodnotu.</span><span class="sxs-lookup"><span data-stu-id="61539-206">This message includes a **request id** value.</span></span>

1. <span data-ttu-id="61539-207">Služba pak odešle zprávu odpovědi, která obsahuje novou hodnotu ETag hlášené vlastnosti kolekce k tématu `$iothub/twin/res/{status}/?$rid={request id}`.</span><span class="sxs-lookup"><span data-stu-id="61539-207">The service then sends a response message that contains the new ETag value for the reported properties collection on topic `$iothub/twin/res/{status}/?$rid={request id}`.</span></span> <span data-ttu-id="61539-208">Tuto zprávu s odpovědí používá stejnou **id žádosti** jako požadavek.</span><span class="sxs-lookup"><span data-stu-id="61539-208">This response message uses the same **request id** as the request.</span></span>

<span data-ttu-id="61539-209">Tělo zprávy požadavku obsahuje dokument JSON, který poskytuje nové hodnoty pro hlášené vlastnosti, které (lze upravit žádná vlastnost nebo metadata).</span><span class="sxs-lookup"><span data-stu-id="61539-209">The request message body contains a JSON document, which provides new values for reported properties (no other property or metadata can be modified).</span></span>
<span data-ttu-id="61539-210">Každý člen v dokumentu JSON aktualizací nebo přidejte odpovídající člen v dokumentu dvojče zařízení.</span><span class="sxs-lookup"><span data-stu-id="61539-210">Each member in the JSON document updates or add the corresponding member in the device twin’s document.</span></span> <span data-ttu-id="61539-211">Sadu členů do `null`, odstraní člena z nadřazeného objektu.</span><span class="sxs-lookup"><span data-stu-id="61539-211">A member set to `null`, deletes the member from the containing object.</span></span> <span data-ttu-id="61539-212">Například:</span><span class="sxs-lookup"><span data-stu-id="61539-212">For example:</span></span>

        {
            "telemetrySendFrequency": "35m",
            "batteryLevel": 60
        }

<span data-ttu-id="61539-213">Možné stavové kódy jsou:</span><span class="sxs-lookup"><span data-stu-id="61539-213">The possible status codes are:</span></span>

|<span data-ttu-id="61539-214">Status</span><span class="sxs-lookup"><span data-stu-id="61539-214">Status</span></span> | <span data-ttu-id="61539-215">Popis</span><span class="sxs-lookup"><span data-stu-id="61539-215">Description</span></span> |
| ----- | ----------- |
| <span data-ttu-id="61539-216">200</span><span class="sxs-lookup"><span data-stu-id="61539-216">200</span></span> | <span data-ttu-id="61539-217">Úspěch</span><span class="sxs-lookup"><span data-stu-id="61539-217">Success</span></span> |
| <span data-ttu-id="61539-218">400</span><span class="sxs-lookup"><span data-stu-id="61539-218">400</span></span> | <span data-ttu-id="61539-219">Chybný požadavek.</span><span class="sxs-lookup"><span data-stu-id="61539-219">Bad Request.</span></span> <span data-ttu-id="61539-220">Nesprávný formát JSON</span><span class="sxs-lookup"><span data-stu-id="61539-220">Malformed JSON</span></span> |
| <span data-ttu-id="61539-221">429</span><span class="sxs-lookup"><span data-stu-id="61539-221">429</span></span> | <span data-ttu-id="61539-222">Příliš mnoho požadavků (omezení), jako za [IoT Hub, omezení šířky pásma][lnk-quotas]</span><span class="sxs-lookup"><span data-stu-id="61539-222">Too many requests (throttled), as per [IoT Hub throttling][lnk-quotas]</span></span> |
| <span data-ttu-id="61539-223">5**</span><span class="sxs-lookup"><span data-stu-id="61539-223">5**</span></span> | <span data-ttu-id="61539-224">Chyby serveru</span><span class="sxs-lookup"><span data-stu-id="61539-224">Server errors</span></span> |

<span data-ttu-id="61539-225">Další informace najdete v tématu [– Příručka vývojáře dvojčata zařízení][lnk-devguide-twin].</span><span class="sxs-lookup"><span data-stu-id="61539-225">For more information, see [Device twins developer's guide][lnk-devguide-twin].</span></span>

### <a name="receiving-desired-properties-update-notifications"></a><span data-ttu-id="61539-226">Přijímající oznámení o aktualizaci požadované vlastnosti</span><span class="sxs-lookup"><span data-stu-id="61539-226">Receiving desired properties update notifications</span></span>

<span data-ttu-id="61539-227">Když je zařízení připojené, IoT Hub odešle oznámení do tématu `$iothub/twin/PATCH/properties/desired/?$version={new version}`, které obsahují obsah aktualizace provádí back-end řešení.</span><span class="sxs-lookup"><span data-stu-id="61539-227">When a device is connected, IoT Hub sends notifications to the topic `$iothub/twin/PATCH/properties/desired/?$version={new version}`, which contain the content of the update performed by the solution back end.</span></span> <span data-ttu-id="61539-228">Například:</span><span class="sxs-lookup"><span data-stu-id="61539-228">For example:</span></span>

        {
            "telemetrySendFrequency": "5m",
            "route": null
        }

<span data-ttu-id="61539-229">Jako aktualizace vlastností `null` hodnoty znamená, že se odstraňuje členem objektu JSON.</span><span class="sxs-lookup"><span data-stu-id="61539-229">As for property updates, `null` values means that the JSON object member is being deleted.</span></span>


> [!IMPORTANT] 
> <span data-ttu-id="61539-230">IoT Hub generuje oznámení o změnách jenom v případě, že zařízení je připojených.</span><span class="sxs-lookup"><span data-stu-id="61539-230">IoT Hub generates change notifications only when devices are connected.</span></span> <span data-ttu-id="61539-231">Zajistěte, aby k implementaci [toku opětovné připojení zařízení] [ lnk-devguide-twin-reconnection] zachovat požadované vlastnosti synchronizovat mezi IoT Hub a aplikace zařízení.</span><span class="sxs-lookup"><span data-stu-id="61539-231">Make sure to implement the [device reconnection flow][lnk-devguide-twin-reconnection] to keep the desired properties synchronized between IoT Hub and the device app.</span></span>

<span data-ttu-id="61539-232">Další informace najdete v tématu [– Příručka vývojáře dvojčata zařízení][lnk-devguide-twin].</span><span class="sxs-lookup"><span data-stu-id="61539-232">For more information, see [Device twins developer's guide][lnk-devguide-twin].</span></span>

### <a name="respond-to-a-direct-method"></a><span data-ttu-id="61539-233">Reakce na přímá metoda</span><span class="sxs-lookup"><span data-stu-id="61539-233">Respond to a direct method</span></span>

<span data-ttu-id="61539-234">První, zařízení má přihlásit k odběru `$iothub/methods/POST/#`.</span><span class="sxs-lookup"><span data-stu-id="61539-234">First, a device has to subscribe to `$iothub/methods/POST/#`.</span></span> <span data-ttu-id="61539-235">IoT Hub odešle požadavky metod v tématu `$iothub/methods/POST/{method name}/?$rid={request id}`, buď platný kód JSON nebo prázdným textem zprávy.</span><span class="sxs-lookup"><span data-stu-id="61539-235">IoT Hub sends method requests to the topic `$iothub/methods/POST/{method name}/?$rid={request id}`, with either a valid JSON or an empty body.</span></span>

<span data-ttu-id="61539-236">Reagovat, zařízení, odešle zprávu platný kód JSON nebo prázdným textem zprávy do tématu `$iothub/methods/res/{status}/?$rid={request id}`, kde **id žádosti** musí odpovídat jedné ve zprávě požadavku, a **stav** musí být celé číslo.</span><span class="sxs-lookup"><span data-stu-id="61539-236">To respond, the device sends a message with a valid JSON or empty body to the topic `$iothub/methods/res/{status}/?$rid={request id}`, where **request id** has to match the one in the request message, and **status** has to be an integer.</span></span>

<span data-ttu-id="61539-237">Další informace najdete v tématu [přímé Příručka pro vývojáře metoda][lnk-methods].</span><span class="sxs-lookup"><span data-stu-id="61539-237">For more information, see [Direct method developer's guide][lnk-methods].</span></span>

### <a name="additional-considerations"></a><span data-ttu-id="61539-238">Další aspekty</span><span class="sxs-lookup"><span data-stu-id="61539-238">Additional considerations</span></span>

<span data-ttu-id="61539-239">Jako poslední zabývat, pokud budete potřebovat k přizpůsobení chování MQTT protokolu na straně cloudu, přečtěte si [brány protokolu Azure IoT] [ lnk-azure-protocol-gateway] umožňující můžete nasadit vlastní vysoce výkonné Brána protokolu, který rozhraní přímo službou IoT Hub.</span><span class="sxs-lookup"><span data-stu-id="61539-239">As a final consideration, if you need to customize the MQTT protocol behavior on the cloud side, you should review the [Azure IoT protocol gateway][lnk-azure-protocol-gateway] that enables you to deploy a high-performance custom protocol gateway that interfaces directly with IoT Hub.</span></span> <span data-ttu-id="61539-240">Brány protokolu Azure IoT umožňuje přizpůsobit protokol zařízení pro přizpůsobení brownfield MQTT nasazení nebo jiné vlastní protokoly.</span><span class="sxs-lookup"><span data-stu-id="61539-240">The Azure IoT protocol gateway enables you to customize the device protocol to accommodate brownfield MQTT deployments or other custom protocols.</span></span> <span data-ttu-id="61539-241">Tento přístup, ale nevyžaduje spouštět a provozovat bránu vlastního protokolu.</span><span class="sxs-lookup"><span data-stu-id="61539-241">This approach does require, however, that you run and operate a custom protocol gateway.</span></span>

## <a name="next-steps"></a><span data-ttu-id="61539-242">Další kroky</span><span class="sxs-lookup"><span data-stu-id="61539-242">Next steps</span></span>

<span data-ttu-id="61539-243">Další informace o protokolu MQTT, najdete v článku [MQTT dokumentace][lnk-mqtt-docs].</span><span class="sxs-lookup"><span data-stu-id="61539-243">To learn more about the MQTT protocol, see the [MQTT documentation][lnk-mqtt-docs].</span></span>

<span data-ttu-id="61539-244">Další informace o plánování nasazení služby IoT Hub naleznete v tématu:</span><span class="sxs-lookup"><span data-stu-id="61539-244">To learn more about planning your IoT Hub deployment, see:</span></span>

* <span data-ttu-id="61539-245">[Azure certifikované pro katalog zařízení IoT][lnk-devices]</span><span class="sxs-lookup"><span data-stu-id="61539-245">[Azure Certified for IoT device catalog][lnk-devices]</span></span>
* <span data-ttu-id="61539-246">[Podpora dalších protokolů][lnk-protocols]</span><span class="sxs-lookup"><span data-stu-id="61539-246">[Support additional protocols][lnk-protocols]</span></span>
* <span data-ttu-id="61539-247">[Porovnání s služby Event Hubs][lnk-compare]</span><span class="sxs-lookup"><span data-stu-id="61539-247">[Compare with Event Hubs][lnk-compare]</span></span>
* <span data-ttu-id="61539-248">[Změna velikosti, HA a zotavení po Havárii][lnk-scaling]</span><span class="sxs-lookup"><span data-stu-id="61539-248">[Scaling, HA, and DR][lnk-scaling]</span></span>

<span data-ttu-id="61539-249">Pokud chcete prozkoumat další možnosti IoT Hub, najdete v části:</span><span class="sxs-lookup"><span data-stu-id="61539-249">To further explore the capabilities of IoT Hub, see:</span></span>

* <span data-ttu-id="61539-250">[Příručka vývojáře pro službu IoT Hub][lnk-devguide]</span><span class="sxs-lookup"><span data-stu-id="61539-250">[IoT Hub developer guide][lnk-devguide]</span></span>
* <span data-ttu-id="61539-251">[Simulaci zařízení s Azure IoT Edge][lnk-iotedge]</span><span class="sxs-lookup"><span data-stu-id="61539-251">[Simulating a device with Azure IoT Edge][lnk-iotedge]</span></span>

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
