---
title: "nahrávání souborů aaaUnderstand Azure IoT Hub | Microsoft Docs"
description: "Příručka vývojáře - použití hello souboru nahrávání funkcí služby IoT Hub toomanage nahrávání souborů z kontejneru objektů blob úložiště Azure tooan zařízení."
services: iot-hub
documentationcenter: .net
author: dominicbetts
manager: timlt
editor: 
ms.assetid: a0427925-3e40-4fcd-96c1-2a31d1ddc14b
ms.service: iot-hub
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/08/2017
ms.author: dobett
ms.openlocfilehash: d44f9303ead4fa282dc0baf70887af1b8a03293d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="upload-files-with-iot-hub"></a>Nahrání souborů s centrem IoT

Podle popisu v hello [koncové body centra IoT] [ lnk-endpoints] článku zařízení můžete iniciovat načtení souboru odesílání oznámení prostřednictvím koncového bodu směřujících zařízení (**/devices/ {deviceId} / soubory**). Když zařízení oznámí IoT Hub, nahrávaný je dokončena, IoT Hub odešle zprávu souboru odesílání oznámení prostřednictvím hello **/messages/servicebound/filenotifications** koncový bod služby přístupem.

Místo zprostředkování zprávy prostřednictvím služby IoT Hub, samotné, IoT Hub místo toho slouží jako tooan dispečera přidruženého účtu úložiště Azure. Zařízení s požadavky, úložiště token ze služby IoT Hub, který je konkrétní toohello souboru hello zařízení, které tooupload. zařízení Hello používá hello identifikátor URI pro SAS tooupload hello souboru toostorage a po dokončení nahrávání hello hello zařízení odešle oznámení o dokončení tooIoT rozbočovače. IoT Hub zkontroluje nahrávání souborů hello je a pak přidá soubor odesílání oznámení zpráva toohello služby směřujících soubor oznámení koncový bod.

Před nahráním souboru tooIoT rozbočovače ze zařízení, musí Konfigurace centra pomocí [přidružení Azure Storage] [ lnk-associate-storage] účet tooit.

Pak můžete zařízení [inicializovat nahrávaný] [ lnk-initialize] a potom [upozornění služby IoT hub] [ lnk-notify] po dokončení nahrávání hello. Volitelně, když zařízení oznámí IoT Hub, že nahrání hello je dokončena, služba hello může generovat [oznámení][lnk-service-notification].

### <a name="when-toouse"></a>Když toouse

Pomocí souborů médií toosend nahrávání souboru a velké telemetrie dávek odeslaný občas připojená zařízení nebo komprimované toosave šířky pásma.

Odkazovat příliš[pokyny komunikace zařízení cloud] [ lnk-d2c-guidance] Pokud máte pochybnosti mezi použitím hlášen vlastnostech, zpráv typu zařízení cloud nebo nahrávání souborů.

## <a name="associate-an-azure-storage-account-with-iot-hub"></a>Přidružit účet úložiště Azure IoT Hub

funkce pro nahrávání souborů hello toouse, je nutné nejprve propojit toohello účet úložiště Azure IoT Hub. Tento úkol můžete dokončit buď prostřednictvím hello [portál Azure][lnk-management-portal], nebo prostřednictvím kódu programu prostřednictvím hello [zprostředkovatele prostředků služby IoT Hub rozhraní REST API] [ lnk-resource-provider-apis]. Jakmile přidružený účet úložiště Azure IoT Hub, hello služby vrátí identifikátor URI SAS tooa zařízení v případě zařízení hello zahájí žádost o odeslání souboru.

> [!NOTE]
> Hello [SDK služby Azure IoT] [ lnk-sdks] automaticky načítání popisovače hello identifikátor URI SAS, odeslání souboru hello a oznamování IoT Hub dokončená nahrávání.


## <a name="initialize-a-file-upload"></a>Inicializace nahrávání souborů
IoT Hub má koncový bod speciálně pro zařízení toorequest identifikátor URI SAS pro úložiště tooupload soubor. Proces odesílání souboru tooinitiate hello, hello zařízení odešle požadavek POST příliš`{iot hub}.azure-devices.net/devices/{deviceId}/files` s hello následující text JSON:

```json
{
    "blobName": "{name of hello file for which a SAS URI will be generated}"
}
```

IoT Hub vrací hello dat, které zařízení hello používá soubor hello tooupload následující:

```json
{
    "correlationId": "somecorrelationid",
    "hostName": "contoso.azure-devices.net",
    "containerName": "testcontainer",
    "blobName": "test-device1/image.jpg",
    "sasToken": "1234asdfSAStoken"
}
```

### <a name="deprecated-initialize-a-file-upload-with-a-get"></a>Zastaralé: Inicializace nahrání souboru s GET

> [!NOTE]
> Tato část popisuje, jak zastaralé funkce tooreceive identifikátor URI SAS ze služby IoT Hub. Použijte metodu POST hello popsané.

IoT Hub má dva REST koncové body toosupport soubor nahrát, tooget hello jeden identifikátor URI SAS úložiště a dalších toonotify hello IoT hub dokončená nahrávání hello. Hello zařízení zahájí proces odesílání souboru hello odesláním GET toohello IoT hub v `{iot hub}.azure-devices.net/devices/{deviceId}/files/{filename}`. Vrátí Hello IoT hub:

* Identifikátor URI SAS toobe konkrétní toohello soubor nahrát.
* Toobe ID korelace používá po dokončení nahrávání hello.

## <a name="notify-iot-hub-of-a-completed-file-upload"></a>Oznámit IoT Hub odeslání dokončené souboru

Hello zařízení je zodpovědná za odesílání hello souboru toostorage pomocí sady SDK úložiště Azure hello. Po dokončení nahrávání hello hello zařízení odešle požadavek POST příliš`{iot hub}.azure-devices.net/devices/{deviceId}/files/notifications` s hello následující text JSON:

```json
{
    "correlationId": "{correlation ID received from hello initial request}",
    "isSuccess": bool,
    "statusCode": XXX,
    "statusDescription": "Description of status"
}
```

Hello hodnota `isSuccess` je logická hodnota udávající, zda má soubor hello se úspěšně nahrál. Hello stavový kód pro `statusCode` hello stav pro nahrávání hello hello souboru toostorage a hello `statusDescription` odpovídá toohello `statusCode`.

## <a name="reference-topics"></a>Témata odkazů:

Hello následující odkazy na témata poskytují další informace o nahrávání souborů ze zařízení.

## <a name="file-upload-notifications"></a>Oznámení o odeslání souboru

Volitelně když zařízení oznámí IoT Hub, nahrávaný je dokončena, IoT Hub můžete vygenerovat zprávu oznámení, který obsahuje hello a jeho název umístění souboru hello.

Jak je popsáno v [koncové body][lnk-endpoints], IoT Hub zajišťuje oznámení o odeslání souboru prostřednictvím koncového bodu služby přístupem (**/messages/servicebound/fileuploadnotifications**) jako zprávy. Hello přijímat sémantiku pro oznámení o odeslání souboru jsou hello stejné jako u zprávy typu cloud zařízení a mít stejný hello [životní cyklus zpráv][lnk-lifecycle]. Každou zprávu načíst z hello koncového bodu oznámení nahrávání souboru je záznam JSON s hello následující vlastnosti:

| Vlastnost | Popis |
| --- | --- |
| EnqueuedTimeUtc |Časové razítko označující vytvoření hello oznámení. |
| ID zařízení |**DeviceId** hello zařízení, která nahrát soubor hello. |
| BlobUri |Identifikátor URI hello nahrát soubor. |
| BlobName |Název hello nahrát soubor. |
| LastUpdatedTime |Časové razítko označující poslední aktualizace souboru hello. |
| BlobSizeInBytes |Velikost hello nahrát soubor. |

**Příklad**. Tento příklad ukazuje textu hello souboru odeslat zprávu oznámení.

```json
{
    "deviceId":"mydevice",
    "blobUri":"https://{storage account}.blob.core.windows.net/{container name}/mydevice/myfile.jpg",
    "blobName":"mydevice/myfile.jpg",
    "lastUpdatedTime":"2016-06-01T21:22:41+00:00",
    "blobSizeInBytes":1234,
    "enqueuedTimeUtc":"2016-06-01T21:22:43.7996883Z"
}
```

## <a name="file-upload-notification-configuration-options"></a>Možnosti konfigurace oznámení nahrávání souborů

Každý IoT hub zpřístupní hello následující možnosti konfigurace pro oznámení o odeslání souboru:

| Vlastnost | Popis | Rozsah a výchozí |
| --- | --- | --- |
| **enableFileUploadNotifications** |Určuje, zda oznámení o odeslání souboru se zapisují koncového bodu oznámení toohello souboru. |Logická hodnota. Výchozí: True. |
| **fileNotifications.ttlAsIso8601** |Výchozí hodnota TTL pro oznámení o odeslání souboru. |Interval ISO_8601 až too48H (minimální 1 minutu). Výchozí hodnota: 1 hodina. |
| **fileNotifications.lockDuration** |Doba trvání uzamčení pro fronty pro odesílání oznámení hello souborů. |5 sekund too300 (minimální 5 sekund). Výchozí: 60 sekund. |
| **fileNotifications.maxDeliveryCount** |Počet maximální doručení pro hello soubor nahrát fronta oznámení. |1 too100. Výchozí: 100. |

## <a name="additional-reference-material"></a>Odkaz na další materiály

Další témata referenční příručka vývojáře pro službu IoT Hub hello patří:

* [Koncové body centra IoT] [ lnk-endpoints] popisuje hello různých koncových bodů, které každý IoT hub zpřístupní pro spuštění a management operace.
* [Omezování a kvóty] [ lnk-quotas] popisuje hello kvót a omezování chování, které se vztahují toohello služby IoT Hub.
* [Azure IoT zařízení a služby sady SDK] [ lnk-sdks] seznamy hello různé jazykové sady SDK můžete použít při vývoji aplikace zařízení a služby, které interakci s centrem IoT.
* [IoT Hub dotazovací jazyk] [ lnk-query] popisuje hello dotazovací jazyk tooretrieve informace ze služby IoT Hub o úlohách a dvojčata zařízení můžete použít.
* [Podpora IoT Hub MQTT] [ lnk-devguide-mqtt] poskytuje další informace o podpoře služby IoT Hub pro protokol MQTT hello.

## <a name="next-steps"></a>Další kroky

Nyní jste se naučili, jak tooupload soubory ze zařízení pomocí služby IoT Hub, může být zájem o hello následující témata Průvodce vývojáře IoT Hub:

* [Správa identit zařízení IoT hub][lnk-devguide-identities]
* [Řízení přístupu tooIoT rozbočovače][lnk-devguide-security]
* [Používání stavu toosynchronize dvojčata zařízení a konfigurací][lnk-devguide-device-twins]
* [Volání metody přímé na zařízení][lnk-devguide-directmethods]
* [Plánování úloh na několika zařízeních][lnk-devguide-jobs]

Pokud chcete tootry některé z hello konceptů popsaných v tomto článku, může být zájem o hello následující kurzu IoT Hub:

* [Jak tooupload soubory ze zařízení toohello cloudové službou IoT Hub][lnk-fileupload-tutorial]

[lnk-resource-provider-apis]: https://docs.microsoft.com/rest/api/iothub/iothubresource
[lnk-endpoints]: iot-hub-devguide-endpoints.md
[lnk-quotas]: iot-hub-devguide-quotas-throttling.md
[lnk-sdks]: iot-hub-devguide-sdks.md
[lnk-query]: iot-hub-devguide-query-language.md
[lnk-devguide-mqtt]: iot-hub-mqtt-support.md
[lnk-management-portal]: https://portal.azure.com
[lnk-fileupload-tutorial]: iot-hub-csharp-csharp-file-upload.md
[lnk-associate-storage]: iot-hub-devguide-file-upload.md#associate-an-azure-storage-account-with-iot-hub
[lnk-initialize]: iot-hub-devguide-file-upload.md#initialize-a-file-upload
[lnk-notify]: iot-hub-devguide-file-upload.md#notify-iot-hub-of-a-completed-file-upload
[lnk-service-notification]: iot-hub-devguide-file-upload.md#file-upload-notifications
[lnk-lifecycle]: iot-hub-devguide-messages-c2d.md#the-cloud-to-device-message-lifecycle
[lnk-d2c-guidance]: iot-hub-devguide-d2c-guidance.md

[lnk-devguide-identities]: iot-hub-devguide-identity-registry.md
[lnk-devguide-security]: iot-hub-devguide-security.md
[lnk-devguide-device-twins]: iot-hub-devguide-device-twins.md
[lnk-devguide-directmethods]: iot-hub-devguide-direct-methods.md
[lnk-devguide-jobs]: iot-hub-devguide-jobs.md
