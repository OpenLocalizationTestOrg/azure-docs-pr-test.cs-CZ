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
# <a name="communicate-with-your-iot-hub-using-hello-mqtt-protocol"></a>Komunikovat se službou IoT hub pomocí protokolu MQTT hello

IoT Hub umožňuje toocommunicate zařízení s hello zařízení koncové body centra IoT pomocí hello [protokoly MQTT v3.1.1] [ lnk-mqtt-org] protokolu na port 8883 nebo protokoly MQTT v3.1.1 přes protokol WebSocket na portu 443. IoT Hub vyžaduje toobe komunikace všechna zařízení, které jsou zabezpečené pomocí protokolu TLS/SSL (proto IoT Hub nepodporuje nezabezpečené připojení přes port 1883).

## <a name="connecting-tooiot-hub"></a>Připojení tooIoT rozbočovače

Zařízení můžete použít Centrum IoT hello MQTT protokol tooconnect tooan buď pomocí knihovny hello v hello [SDK služby Azure IoT] [ lnk-device-sdks] nebo přímo pomocí protokolu MQTT hello.

## <a name="using-hello-device-sdks"></a>Pomocí sady SDK pro zařízení hello

[Sady SDK zařízení] [ lnk-device-sdks] tuto podporu hello MQTT protokolu jsou k dispozici pro Java, Node.js, C, C# a Python. sady SDK pro zařízení Hello použijte hello standardní IoT Hub připojovací řetězec tooestablish připojení tooan IoT hub. protokol MQTT hello toouse, hello klienta protokolu parametr musí být nastaven příliš**MQTT**. Ve výchozím nastavení, sady SDK pro zařízení hello připojit tooan IoT Hub s hello **CleanSession** příliš nastaven příznak**0** a používat **QoS 1** k výměně zpráv službou hello IoT hub.

Když je zařízení připojené tooan služby IoT hub, sady SDK pro zařízení hello poskytují metody, které umožní zprávy toosend zařízení hello tooand přijímat zprávy ze služby IoT hub.

Hello následující tabulka obsahuje odkazy toocode ukázky pro každý podporovaný jazyk a určuje hello parametr toouse tooestablish tooIoT připojení rozbočovače pomocí protokolu MQTT hello.

| Jazyk | Parametr protokolu |
| --- | --- |
| [Node.js][lnk-sample-node] |Azure-iot zařízení mqtt |
| [Java][lnk-sample-java] |IotHubClientProtocol.MQTT |
| [C][lnk-sample-c] |MQTT_Protocol |
| [C#][lnk-sample-csharp] |TransportType.Mqtt |
| [Python][lnk-sample-python] |IoTHubTransportProvider.MQTT |

### <a name="migrating-a-device-app-from-amqp-toomqtt"></a>Migrace z AMQP tooMQTT aplikace na zařízení

Pokud používáte hello [sady SDK pro zařízení][lnk-device-sdks], přepínání pomocí protokolu AMQP vyžaduje tooMQTT změna hello protokol parametr hello inicializace klienta, jak bylo uvedeno dříve.

Když to uděláte, ujistěte se, zda text hello toocheck následující položky:

* AMQP vrátí chyby pro mnoho podmínek, zatímco MQTT ukončí hello připojení. V důsledku vaše zpracování logiky výjimek může vyžadovat některé změny.
* MQTT nepodporuje hello *odmítnout* operace při přijímání [zprávy typu cloud zařízení][lnk-messaging]. Pokud vaše aplikace back-end musí tooreceive odpověď z aplikace hello zařízení, zvažte použití [přímé metody][lnk-methods].

## <a name="using-hello-mqtt-protocol-directly"></a>Přímo pomocí protokolu MQTT hello
Pokud zařízení nelze použít sady SDK pro zařízení hello, se může pořád připojit toohello zařízení veřejné koncové body pomocí protokolu MQTT hello na portu 8883. V hello **CONNECT** paketu hello zařízení musí používat hello následující hodnoty:

* Pro hello **ClientId** pole, použijte hello **deviceId**.
* Pro hello **uživatelské jméno** pole, použijte `{iothubhostname}/{device_id}/api-version=2016-11-14`, kde {iothubhostname} je hello úplné CName hello IoT hub.

    Například pokud hello název služby IoT hub je **contoso.azure devices.net** a pokud je název vašeho zařízení hello **MyDevice01**, hello úplné **uživatelské jméno** by měl obsahovat pole `contoso.azure-devices.net/MyDevice01/api-version=2016-11-14`.
* Pro hello **heslo** pole, použijte SAS token. Formát Hello hello SAS token je hello stejné jako u hello HTTP a protokoly AMQP:<br/>`SharedAccessSignature sig={signature-string}&se={expiry}&sr={URL-encoded-resourceURI}`.

    Další informace o tom, toogenerate tokeny SAS, najdete v části zařízení hello [tokeny zabezpečení pomocí služby IoT Hub][lnk-sas-tokens].

    Při testování, můžete použít také hello [explorer zařízení] [ lnk-device-explorer] tooquickly nástroj vygenerovat token SAS, který můžete zkopírovat a vložit do vlastního kódu:

  1. Přejděte toohello **správy** kartě v **Explorer zařízení**.
  2. Klikněte na tlačítko **tokenu SAS** (pravého horního).
  3. Na **SASTokenForm**, vyberte příslušné zařízení v hello **DeviceID** rozevírací nabídku. Nastavit vaše **TTL**.
  4. Klikněte na tlačítko **generování** toocreate tokenu.

     Hello tokenu SAS, aby se vygenerovala má tuto strukturu: `HostName={your hub name}.azure-devices.net;DeviceId=javadevice;SharedAccessSignature=SharedAccessSignature sr={your hub name}.azure-devices.net%2Fdevices%2FMyDevice01%2Fapi-version%3D2016-11-14&sig=vSgHBMUG.....Ntg%3d&se=1456481802`.

     Hello součástí tento token toouse jako hello **heslo** je pole tooconnect pomocí MQTT: `SharedAccessSignature sr={your hub name}.azure-devices.net%2Fdevices%2FMyDevice01%2Fapi-version%3D2016-11-14&sig=vSgHBMUG.....Ntg%3d&se=1456481802`.

MQTT připojit a odpojit paketů, IoT Hub vydá událost na hello **monitorování Operations** kanál společně s dalšími informacemi, které vám pomůžou tootroubleshoot problémy s připojením.

### <a name="sending-device-to-cloud-messages"></a>Odesílání zpráv typu zařízení cloud

Po provedení úspěšného připojení, můžete odeslat zařízení pomocí Centra zpráv tooIoT `devices/{device_id}/messages/events/` nebo `devices/{device_id}/messages/events/{property_bag}` jako **název tématu**. Hello `{property_bag}` prvek umožní zprávy toosend hello zařízení bez dalších vlastností ve formátu kódovaná adresou url. Například:

```
RFC 2396-encoded(<PropertyName1>)=RFC 2396-encoded(<PropertyValue1>)&RFC 2396-encoded(<PropertyName2>)=RFC 2396-encoded(<PropertyValue2>)…
```

> [!NOTE]
> To `{property_bag}` element hello používá stejné kódování jako řetězce dotazu v protokolu hello protokolu HTTP.
>
>

Můžete také použít aplikaci zařízení Hello `devices/{device_id}/messages/events/{property_bag}` jako hello **název tématu bude** toodefine *bude zprávy* toobe předávány jako zprávu telemetrie.

- IoT Hub nepodporuje QoS 2 zprávy. Pokud aplikace na zařízení publikuje zprávu s **QoS 2**, IoT Hub ukončí hello síťové připojení.
- IoT Hub není zachována zachovat zprávy. Pokud zařízení odesílá zprávy s hello **zachovat** nastaven příznak too1, IoT Hub přidá hello **x-opt zachovat** aplikace vlastnost toohello zpráva. V takovém případě místo zachování hello zachovat zpráva, IoT Hub předá toohello back-end aplikace.
- IoT Hub podporuje pouze jeden aktivní připojení MQTT na jedno zařízení. Všechny nové připojení MQTT jménem hello způsobí stejné ID zařízení IoT Hub toodrop hello existující připojení.

Další informace najdete v tématu [– Příručka vývojáře pro zasílání zpráv][lnk-messaging].

### <a name="receiving-cloud-to-device-messages"></a>Příjem zpráv typu cloud zařízení

tooreceive zprávy ze služby IoT Hub, zařízení by měl přihlásit se pomocí `devices/{device_id}/messages/devicebound/#` jako **tématu filtru**. Hello víceúrovňovou zástupné  **#**  v hello tématu filtr se používá jen tooallow hello zařízení tooreceive další vlastnosti v název tématu hello. IoT Hub neumožňuje použití hello hello  **#**  nebo **?** zástupné znaky pro filtrování dílčí témata. Vzhledem k tomu, že Centrum IoT není zprostředkovatel zasílání zpráv protokol pub-sub obecné účely, podporuje pouze názvy tématu hello zdokumentované a téma filtry.

Hello zařízení neobdrží všechny zprávy ze služby IoT Hub, dokud ho se úspěšně připojila tooits konkrétní zařízení koncového bodu, reprezentována hello `devices/{device_id}/messages/devicebound/#` tématu filtru. Po úspěšné odběru bylo úspěšně navázáno, spustí hello zařízení, přijímání zpráv pouze cloud zařízení, které byly odeslány tooit po době hello hello předplatného. Pokud zařízení hello připojí **CleanSession** příliš nastaven příznak**0**, hello předplatného je trvalé v jiných relacích. V takovém případě hello zase připojí s **CleanSession 0** hello zařízení přijímá nezpracovaných zprávy, které byly odeslány tooit době, kdy byl odpojen. Pokud zařízení hello používá **CleanSession** příliš nastaven příznak**1** ale, že neobdrží všechny zprávy ze služby IoT Hub dokud přihlásí tooits zařízení-koncový bod.

IoT Hub zajišťuje zprávy s hello **název tématu** `devices/{device_id}/messages/devicebound/`, nebo `devices/{device_id}/messages/devicebound/{property_bag}` Pokud nejsou k dispozici žádné vlastnosti zprávy. `{property_bag}`obsahuje dvojice klíč/hodnota kódovaná adresou url vlastností zpráv. Pouze vlastnosti aplikace a vlastnosti uživatele nastavit systému (například **messageId** nebo **correlationId**) jsou zahrnuty v kontejneru objektů a dat hello. Názvy vlastností systému mají následující předpony hello  **$** , vlastnosti aplikace pomocí žádná předpona. původní název vlastnosti hello.

Když aplikace na zařízení odběratel tooa téma se **QoS 2**, IoT Hub uděluje maximální QoS úrovně 1 v hello **SUBACK** paketů. Potom přináší IoT Hub zařízení toohello zprávy pomocí technologie QoS 1.

### <a name="retrieving-a-device-twins-properties"></a>Načítání vlastností dvojče zařízení

Nejprve zařízení odběratel příliš`$iothub/twin/res/#`, tooreceive hello operace odpovědi. Pak odešle prázdná zpráva tootopic `$iothub/twin/GET/?$rid={request id}`, s hodnotou vyplněná **id žádosti**. hello služby pak odešle zprávu odpovědi, která obsahuje data twin hello zařízení k tématu `$iothub/twin/res/{status}/?$rid={request id}`, pomocí stejné hello  **id žádosti** jako hello požadavku.

Id žádosti může být libovolná platná hodnota pro hodnotu vlastnosti zprávy dle [IoT Hub – Příručka vývojáře pro zasílání zpráv][lnk-messaging], a stav byl ověřen jako celé číslo.
text odpovědi Hello obsahuje sekce properties hello hello dvojče zařízení:

textu Hello položky registru identit hello omezené toohello člen "vlastnosti", například:

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

Hello možné stavové kódy jsou:

|Status | Popis |
| ----- | ----------- |
| 200 | Úspěch |
| 429 | Příliš mnoho požadavků (omezení), jako za [IoT Hub, omezení šířky pásma][lnk-quotas] |
| 5** | Chyby serveru |

Další informace najdete v tématu [– Příručka vývojáře dvojčata zařízení][lnk-devguide-twin].

### <a name="update-device-twins-reported-properties"></a>Aktualizovat vlastnosti hlášené dvojče zařízení

Hello následující text popisuje jak zařízení aktualizuje hello hlášené vlastnosti v hello dvojče zařízení IoT hub:

1. Zařízení musí nejprve odběru toohello `$iothub/twin/res/#` tématu tooreceive hello operace odpovědi ze služby IoT Hub.

1. Zařízení odesílá zprávu, která obsahuje hello zařízení twin aktualizace toohello `$iothub/twin/PATCH/properties/reported/?$rid={request id}` tématu. Tato zpráva obsahuje **id žádosti** hodnotu.

1. Hello služby pak odešle zprávu odpovědi, která obsahuje hello novou hodnotu ETag hello hlášené vlastnosti kolekce k tématu `$iothub/twin/res/{status}/?$rid={request id}`. Tuto zprávu s odpovědí hello používá stejný **id žádosti** jako hello požadavku.

tělo zprávy požadavku Hello obsahuje dokument JSON, který poskytuje nové hodnoty pro hlášené vlastnosti, které (lze upravit žádná vlastnost nebo metadata).
Každý člen v dokumentu JSON hello aktualizací nebo přidejte hello odpovídající člen v dokumentu dvojče zařízení hello. Člen nastavit příliš`null`, odstraní hello člena z hello obsahující objektu. Například:

        {
            "telemetrySendFrequency": "35m",
            "batteryLevel": 60
        }

Hello možné stavové kódy jsou:

|Status | Popis |
| ----- | ----------- |
| 200 | Úspěch |
| 400 | Chybný požadavek. Nesprávný formát JSON |
| 429 | Příliš mnoho požadavků (omezení), jako za [IoT Hub, omezení šířky pásma][lnk-quotas] |
| 5** | Chyby serveru |

Další informace najdete v tématu [– Příručka vývojáře dvojčata zařízení][lnk-devguide-twin].

### <a name="receiving-desired-properties-update-notifications"></a>Přijímající oznámení o aktualizaci požadované vlastnosti

Když je zařízení připojené, IoT Hub odešle oznámení toohello tématu `$iothub/twin/PATCH/properties/desired/?$version={new version}`, které obsahují obsah hello hello aktualizace provádí back-end hello řešení. Například:

        {
            "telemetrySendFrequency": "5m",
            "route": null
        }

Jako aktualizace vlastností `null` znamená hodnoty, které hello JSON objektu člen se odstraňuje.


> [!IMPORTANT] 
> IoT Hub generuje oznámení o změnách jenom v případě, že zařízení je připojených. Ujistěte se, zda text hello tooimplement [toku opětovné připojení zařízení] [ lnk-devguide-twin-reconnection] tookeep hello požadovaných vlastností, které jsou synchronizovány mezi IoT Hub a aplikace hello zařízení.

Další informace najdete v tématu [– Příručka vývojáře dvojčata zařízení][lnk-devguide-twin].

### <a name="respond-tooa-direct-method"></a>Přímá metoda tooa reakce

První, zařízení má toosubscribe příliš`$iothub/methods/POST/#`. IoT Hub odešle, metoda žádosti toohello tématu `$iothub/methods/POST/{method name}/?$rid={request id}`, buď platný kód JSON nebo prázdným textem zprávy.

toorespond, hello zařízení odesílá zprávy s prázdným textem zprávy toohello tématu nebo platný JSON `$iothub/methods/res/{status}/?$rid={request id}`, kde **id žádosti** má toomatch hello jeden ve zprávě požadavku hello, a **stav** má toobe celé číslo .

Další informace najdete v tématu [přímé Příručka pro vývojáře metoda][lnk-methods].

### <a name="additional-considerations"></a>Další aspekty

Jako poslední zabývat, pokud potřebujete toocustomize hello MQTT chování protokolu na straně cloudu hello, byste měli zkontrolovat hello [brány protokolu Azure IoT] [ lnk-azure-protocol-gateway] , která umožní toodeploy vysoce výkonné vlastní protokol brána, která rozhraní přímo službou IoT Hub. brány protokolu Azure IoT Hello umožňuje vám toocustomize hello zařízení protokol tooaccommodate brownfield MQTT nasazení nebo jiné vlastní protokoly. Tento přístup, ale nevyžaduje spouštět a provozovat bránu vlastního protokolu.

## <a name="next-steps"></a>Další kroky

toolearn Další informace o protokolu MQTT hello, najdete v části hello [MQTT dokumentace][lnk-mqtt-docs].

toolearn Další informace o plánování nasazení služby IoT Hub, najdete v části:

* [Azure certifikované pro katalog zařízení IoT][lnk-devices]
* [Podpora dalších protokolů][lnk-protocols]
* [Porovnání s služby Event Hubs][lnk-compare]
* [Změna velikosti, HA a zotavení po Havárii][lnk-scaling]

toofurther prozkoumat hello služby IoT Hub, najdete v tématu:

* [Příručka vývojáře pro službu IoT Hub][lnk-devguide]
* [Simulaci zařízení s Azure IoT Edge][lnk-iotedge]

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
