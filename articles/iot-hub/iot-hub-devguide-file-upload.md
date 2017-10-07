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
# <a name="upload-files-with-iot-hub"></a><span data-ttu-id="dbb70-103">Nahrání souborů s centrem IoT</span><span class="sxs-lookup"><span data-stu-id="dbb70-103">Upload files with IoT Hub</span></span>

<span data-ttu-id="dbb70-104">Podle popisu v hello [koncové body centra IoT] [ lnk-endpoints] článku zařízení můžete iniciovat načtení souboru odesílání oznámení prostřednictvím koncového bodu směřujících zařízení (**/devices/ {deviceId} / soubory**).</span><span class="sxs-lookup"><span data-stu-id="dbb70-104">As detailed in hello [IoT Hub endpoints][lnk-endpoints] article, a device can initiate a file upload by sending a notification through a device-facing endpoint (**/devices/{deviceId}/files**).</span></span> <span data-ttu-id="dbb70-105">Když zařízení oznámí IoT Hub, nahrávaný je dokončena, IoT Hub odešle zprávu souboru odesílání oznámení prostřednictvím hello **/messages/servicebound/filenotifications** koncový bod služby přístupem.</span><span class="sxs-lookup"><span data-stu-id="dbb70-105">When a device notifies IoT Hub that an upload is complete, IoT Hub sends a file upload notification message through hello **/messages/servicebound/filenotifications** service-facing endpoint.</span></span>

<span data-ttu-id="dbb70-106">Místo zprostředkování zprávy prostřednictvím služby IoT Hub, samotné, IoT Hub místo toho slouží jako tooan dispečera přidruženého účtu úložiště Azure.</span><span class="sxs-lookup"><span data-stu-id="dbb70-106">Instead of brokering messages through IoT Hub itself, IoT Hub instead acts as a dispatcher tooan associated Azure Storage account.</span></span> <span data-ttu-id="dbb70-107">Zařízení s požadavky, úložiště token ze služby IoT Hub, který je konkrétní toohello souboru hello zařízení, které tooupload.</span><span class="sxs-lookup"><span data-stu-id="dbb70-107">A device requests a storage token from IoT Hub that is specific toohello file hello device wishes tooupload.</span></span> <span data-ttu-id="dbb70-108">zařízení Hello používá hello identifikátor URI pro SAS tooupload hello souboru toostorage a po dokončení nahrávání hello hello zařízení odešle oznámení o dokončení tooIoT rozbočovače.</span><span class="sxs-lookup"><span data-stu-id="dbb70-108">hello device uses hello SAS URI tooupload hello file toostorage, and when hello upload is complete hello device sends a notification of completion tooIoT Hub.</span></span> <span data-ttu-id="dbb70-109">IoT Hub zkontroluje nahrávání souborů hello je a pak přidá soubor odesílání oznámení zpráva toohello služby směřujících soubor oznámení koncový bod.</span><span class="sxs-lookup"><span data-stu-id="dbb70-109">IoT Hub checks hello file upload is complete and then adds a file upload notification message toohello service-facing file notification endpoint.</span></span>

<span data-ttu-id="dbb70-110">Před nahráním souboru tooIoT rozbočovače ze zařízení, musí Konfigurace centra pomocí [přidružení Azure Storage] [ lnk-associate-storage] účet tooit.</span><span class="sxs-lookup"><span data-stu-id="dbb70-110">Before you upload a file tooIoT Hub from a device, you must configure your hub by [associating an Azure Storage][lnk-associate-storage] account tooit.</span></span>

<span data-ttu-id="dbb70-111">Pak můžete zařízení [inicializovat nahrávaný] [ lnk-initialize] a potom [upozornění služby IoT hub] [ lnk-notify] po dokončení nahrávání hello.</span><span class="sxs-lookup"><span data-stu-id="dbb70-111">Your device can then [initialize an upload][lnk-initialize] and then [notify IoT hub][lnk-notify] when hello upload completes.</span></span> <span data-ttu-id="dbb70-112">Volitelně, když zařízení oznámí IoT Hub, že nahrání hello je dokončena, služba hello může generovat [oznámení][lnk-service-notification].</span><span class="sxs-lookup"><span data-stu-id="dbb70-112">Optionally, when a device notifies IoT Hub that hello upload is complete, hello service can generate a [notification message][lnk-service-notification].</span></span>

### <a name="when-toouse"></a><span data-ttu-id="dbb70-113">Když toouse</span><span class="sxs-lookup"><span data-stu-id="dbb70-113">When toouse</span></span>

<span data-ttu-id="dbb70-114">Pomocí souborů médií toosend nahrávání souboru a velké telemetrie dávek odeslaný občas připojená zařízení nebo komprimované toosave šířky pásma.</span><span class="sxs-lookup"><span data-stu-id="dbb70-114">Use file upload toosend media files and large telemetry batches uploaded by intermittently connected devices or compressed toosave bandwidth.</span></span>

<span data-ttu-id="dbb70-115">Odkazovat příliš[pokyny komunikace zařízení cloud] [ lnk-d2c-guidance] Pokud máte pochybnosti mezi použitím hlášen vlastnostech, zpráv typu zařízení cloud nebo nahrávání souborů.</span><span class="sxs-lookup"><span data-stu-id="dbb70-115">Refer too[Device-to-cloud communication guidance][lnk-d2c-guidance] if in doubt between using reported properties, device-to-cloud messages, or file upload.</span></span>

## <a name="associate-an-azure-storage-account-with-iot-hub"></a><span data-ttu-id="dbb70-116">Přidružit účet úložiště Azure IoT Hub</span><span class="sxs-lookup"><span data-stu-id="dbb70-116">Associate an Azure Storage account with IoT Hub</span></span>

<span data-ttu-id="dbb70-117">funkce pro nahrávání souborů hello toouse, je nutné nejprve propojit toohello účet úložiště Azure IoT Hub.</span><span class="sxs-lookup"><span data-stu-id="dbb70-117">toouse hello file upload functionality, you must first link an Azure Storage account toohello IoT Hub.</span></span> <span data-ttu-id="dbb70-118">Tento úkol můžete dokončit buď prostřednictvím hello [portál Azure][lnk-management-portal], nebo prostřednictvím kódu programu prostřednictvím hello [zprostředkovatele prostředků služby IoT Hub rozhraní REST API] [ lnk-resource-provider-apis].</span><span class="sxs-lookup"><span data-stu-id="dbb70-118">You can complete this task either through hello [Azure portal][lnk-management-portal], or programmatically through hello [IoT Hub resource provider REST APIs][lnk-resource-provider-apis].</span></span> <span data-ttu-id="dbb70-119">Jakmile přidružený účet úložiště Azure IoT Hub, hello služby vrátí identifikátor URI SAS tooa zařízení v případě zařízení hello zahájí žádost o odeslání souboru.</span><span class="sxs-lookup"><span data-stu-id="dbb70-119">Once you have associated an Azure Storage account with your IoT Hub, hello service returns a SAS URI tooa device when hello device initiates a file upload request.</span></span>

> [!NOTE]
> <span data-ttu-id="dbb70-120">Hello [SDK služby Azure IoT] [ lnk-sdks] automaticky načítání popisovače hello identifikátor URI SAS, odeslání souboru hello a oznamování IoT Hub dokončená nahrávání.</span><span class="sxs-lookup"><span data-stu-id="dbb70-120">hello [Azure IoT SDKs][lnk-sdks] automatically handle retrieving hello SAS URI, uploading hello file, and notifying IoT Hub of a completed upload.</span></span>


## <a name="initialize-a-file-upload"></a><span data-ttu-id="dbb70-121">Inicializace nahrávání souborů</span><span class="sxs-lookup"><span data-stu-id="dbb70-121">Initialize a file upload</span></span>
<span data-ttu-id="dbb70-122">IoT Hub má koncový bod speciálně pro zařízení toorequest identifikátor URI SAS pro úložiště tooupload soubor.</span><span class="sxs-lookup"><span data-stu-id="dbb70-122">IoT Hub has an endpoint specifically for devices toorequest a SAS URI for storage tooupload a file.</span></span> <span data-ttu-id="dbb70-123">Proces odesílání souboru tooinitiate hello, hello zařízení odešle požadavek POST příliš`{iot hub}.azure-devices.net/devices/{deviceId}/files` s hello následující text JSON:</span><span class="sxs-lookup"><span data-stu-id="dbb70-123">tooinitiate hello file upload process, hello device sends a POST request too`{iot hub}.azure-devices.net/devices/{deviceId}/files` with hello following JSON body:</span></span>

```json
{
    "blobName": "{name of hello file for which a SAS URI will be generated}"
}
```

<span data-ttu-id="dbb70-124">IoT Hub vrací hello dat, které zařízení hello používá soubor hello tooupload následující:</span><span class="sxs-lookup"><span data-stu-id="dbb70-124">IoT Hub returns hello following data, which hello device uses tooupload hello file:</span></span>

```json
{
    "correlationId": "somecorrelationid",
    "hostName": "contoso.azure-devices.net",
    "containerName": "testcontainer",
    "blobName": "test-device1/image.jpg",
    "sasToken": "1234asdfSAStoken"
}
```

### <a name="deprecated-initialize-a-file-upload-with-a-get"></a><span data-ttu-id="dbb70-125">Zastaralé: Inicializace nahrání souboru s GET</span><span class="sxs-lookup"><span data-stu-id="dbb70-125">Deprecated: initialize a file upload with a GET</span></span>

> [!NOTE]
> <span data-ttu-id="dbb70-126">Tato část popisuje, jak zastaralé funkce tooreceive identifikátor URI SAS ze služby IoT Hub.</span><span class="sxs-lookup"><span data-stu-id="dbb70-126">This section describes deprecated functionality for how tooreceive a SAS URI from IoT Hub.</span></span> <span data-ttu-id="dbb70-127">Použijte metodu POST hello popsané.</span><span class="sxs-lookup"><span data-stu-id="dbb70-127">Use hello POST method described previously.</span></span>

<span data-ttu-id="dbb70-128">IoT Hub má dva REST koncové body toosupport soubor nahrát, tooget hello jeden identifikátor URI SAS úložiště a dalších toonotify hello IoT hub dokončená nahrávání hello.</span><span class="sxs-lookup"><span data-stu-id="dbb70-128">IoT Hub has two REST endpoints toosupport file upload, one tooget hello SAS URI for storage and hello other toonotify hello IoT hub of a completed upload.</span></span> <span data-ttu-id="dbb70-129">Hello zařízení zahájí proces odesílání souboru hello odesláním GET toohello IoT hub v `{iot hub}.azure-devices.net/devices/{deviceId}/files/{filename}`.</span><span class="sxs-lookup"><span data-stu-id="dbb70-129">hello device initiates hello file upload process by sending a GET toohello IoT hub at `{iot hub}.azure-devices.net/devices/{deviceId}/files/{filename}`.</span></span> <span data-ttu-id="dbb70-130">Vrátí Hello IoT hub:</span><span class="sxs-lookup"><span data-stu-id="dbb70-130">hello IoT hub returns:</span></span>

* <span data-ttu-id="dbb70-131">Identifikátor URI SAS toobe konkrétní toohello soubor nahrát.</span><span class="sxs-lookup"><span data-stu-id="dbb70-131">A SAS URI specific toohello file toobe uploaded.</span></span>
* <span data-ttu-id="dbb70-132">Toobe ID korelace používá po dokončení nahrávání hello.</span><span class="sxs-lookup"><span data-stu-id="dbb70-132">A correlation ID toobe used once hello upload is completed.</span></span>

## <a name="notify-iot-hub-of-a-completed-file-upload"></a><span data-ttu-id="dbb70-133">Oznámit IoT Hub odeslání dokončené souboru</span><span class="sxs-lookup"><span data-stu-id="dbb70-133">Notify IoT Hub of a completed file upload</span></span>

<span data-ttu-id="dbb70-134">Hello zařízení je zodpovědná za odesílání hello souboru toostorage pomocí sady SDK úložiště Azure hello.</span><span class="sxs-lookup"><span data-stu-id="dbb70-134">hello device is responsible for uploading hello file toostorage using hello Azure Storage SDKs.</span></span> <span data-ttu-id="dbb70-135">Po dokončení nahrávání hello hello zařízení odešle požadavek POST příliš`{iot hub}.azure-devices.net/devices/{deviceId}/files/notifications` s hello následující text JSON:</span><span class="sxs-lookup"><span data-stu-id="dbb70-135">When hello upload is complete, hello device sends a POST request too`{iot hub}.azure-devices.net/devices/{deviceId}/files/notifications` with hello following JSON body:</span></span>

```json
{
    "correlationId": "{correlation ID received from hello initial request}",
    "isSuccess": bool,
    "statusCode": XXX,
    "statusDescription": "Description of status"
}
```

<span data-ttu-id="dbb70-136">Hello hodnota `isSuccess` je logická hodnota udávající, zda má soubor hello se úspěšně nahrál.</span><span class="sxs-lookup"><span data-stu-id="dbb70-136">hello value of `isSuccess` is a Boolean representing whether hello file was uploaded successfully.</span></span> <span data-ttu-id="dbb70-137">Hello stavový kód pro `statusCode` hello stav pro nahrávání hello hello souboru toostorage a hello `statusDescription` odpovídá toohello `statusCode`.</span><span class="sxs-lookup"><span data-stu-id="dbb70-137">hello status code for `statusCode` is hello status for hello upload of hello file toostorage, and hello `statusDescription` corresponds toohello `statusCode`.</span></span>

## <a name="reference-topics"></a><span data-ttu-id="dbb70-138">Témata odkazů:</span><span class="sxs-lookup"><span data-stu-id="dbb70-138">Reference topics:</span></span>

<span data-ttu-id="dbb70-139">Hello následující odkazy na témata poskytují další informace o nahrávání souborů ze zařízení.</span><span class="sxs-lookup"><span data-stu-id="dbb70-139">hello following reference topics provide you with more information about uploading files from a device.</span></span>

## <a name="file-upload-notifications"></a><span data-ttu-id="dbb70-140">Oznámení o odeslání souboru</span><span class="sxs-lookup"><span data-stu-id="dbb70-140">File upload notifications</span></span>

<span data-ttu-id="dbb70-141">Volitelně když zařízení oznámí IoT Hub, nahrávaný je dokončena, IoT Hub můžete vygenerovat zprávu oznámení, který obsahuje hello a jeho název umístění souboru hello.</span><span class="sxs-lookup"><span data-stu-id="dbb70-141">Optionally, when a device notifies IoT Hub that an upload is complete, IoT Hub can generate a notification message that contains hello name and storage location of hello file.</span></span>

<span data-ttu-id="dbb70-142">Jak je popsáno v [koncové body][lnk-endpoints], IoT Hub zajišťuje oznámení o odeslání souboru prostřednictvím koncového bodu služby přístupem (**/messages/servicebound/fileuploadnotifications**) jako zprávy.</span><span class="sxs-lookup"><span data-stu-id="dbb70-142">As explained in [Endpoints][lnk-endpoints], IoT Hub delivers file upload notifications through a service-facing endpoint (**/messages/servicebound/fileuploadnotifications**) as messages.</span></span> <span data-ttu-id="dbb70-143">Hello přijímat sémantiku pro oznámení o odeslání souboru jsou hello stejné jako u zprávy typu cloud zařízení a mít stejný hello [životní cyklus zpráv][lnk-lifecycle].</span><span class="sxs-lookup"><span data-stu-id="dbb70-143">hello receive semantics for file upload notifications are hello same as for cloud-to-device messages and have hello same [message lifecycle][lnk-lifecycle].</span></span> <span data-ttu-id="dbb70-144">Každou zprávu načíst z hello koncového bodu oznámení nahrávání souboru je záznam JSON s hello následující vlastnosti:</span><span class="sxs-lookup"><span data-stu-id="dbb70-144">Each message retrieved from hello file upload notification endpoint is a JSON record with hello following properties:</span></span>

| <span data-ttu-id="dbb70-145">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="dbb70-145">Property</span></span> | <span data-ttu-id="dbb70-146">Popis</span><span class="sxs-lookup"><span data-stu-id="dbb70-146">Description</span></span> |
| --- | --- |
| <span data-ttu-id="dbb70-147">EnqueuedTimeUtc</span><span class="sxs-lookup"><span data-stu-id="dbb70-147">EnqueuedTimeUtc</span></span> |<span data-ttu-id="dbb70-148">Časové razítko označující vytvoření hello oznámení.</span><span class="sxs-lookup"><span data-stu-id="dbb70-148">Timestamp indicating when hello notification was created.</span></span> |
| <span data-ttu-id="dbb70-149">ID zařízení</span><span class="sxs-lookup"><span data-stu-id="dbb70-149">DeviceId</span></span> |<span data-ttu-id="dbb70-150">**DeviceId** hello zařízení, která nahrát soubor hello.</span><span class="sxs-lookup"><span data-stu-id="dbb70-150">**DeviceId** of hello device which uploaded hello file.</span></span> |
| <span data-ttu-id="dbb70-151">BlobUri</span><span class="sxs-lookup"><span data-stu-id="dbb70-151">BlobUri</span></span> |<span data-ttu-id="dbb70-152">Identifikátor URI hello nahrát soubor.</span><span class="sxs-lookup"><span data-stu-id="dbb70-152">URI of hello uploaded file.</span></span> |
| <span data-ttu-id="dbb70-153">BlobName</span><span class="sxs-lookup"><span data-stu-id="dbb70-153">BlobName</span></span> |<span data-ttu-id="dbb70-154">Název hello nahrát soubor.</span><span class="sxs-lookup"><span data-stu-id="dbb70-154">Name of hello uploaded file.</span></span> |
| <span data-ttu-id="dbb70-155">LastUpdatedTime</span><span class="sxs-lookup"><span data-stu-id="dbb70-155">LastUpdatedTime</span></span> |<span data-ttu-id="dbb70-156">Časové razítko označující poslední aktualizace souboru hello.</span><span class="sxs-lookup"><span data-stu-id="dbb70-156">Timestamp indicating when hello file was last updated.</span></span> |
| <span data-ttu-id="dbb70-157">BlobSizeInBytes</span><span class="sxs-lookup"><span data-stu-id="dbb70-157">BlobSizeInBytes</span></span> |<span data-ttu-id="dbb70-158">Velikost hello nahrát soubor.</span><span class="sxs-lookup"><span data-stu-id="dbb70-158">Size of hello uploaded file.</span></span> |

<span data-ttu-id="dbb70-159">**Příklad**.</span><span class="sxs-lookup"><span data-stu-id="dbb70-159">**Example**.</span></span> <span data-ttu-id="dbb70-160">Tento příklad ukazuje textu hello souboru odeslat zprávu oznámení.</span><span class="sxs-lookup"><span data-stu-id="dbb70-160">This example shows hello body of a file upload notification message.</span></span>

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

## <a name="file-upload-notification-configuration-options"></a><span data-ttu-id="dbb70-161">Možnosti konfigurace oznámení nahrávání souborů</span><span class="sxs-lookup"><span data-stu-id="dbb70-161">File upload notification configuration options</span></span>

<span data-ttu-id="dbb70-162">Každý IoT hub zpřístupní hello následující možnosti konfigurace pro oznámení o odeslání souboru:</span><span class="sxs-lookup"><span data-stu-id="dbb70-162">Each IoT hub exposes hello following configuration options for file upload notifications:</span></span>

| <span data-ttu-id="dbb70-163">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="dbb70-163">Property</span></span> | <span data-ttu-id="dbb70-164">Popis</span><span class="sxs-lookup"><span data-stu-id="dbb70-164">Description</span></span> | <span data-ttu-id="dbb70-165">Rozsah a výchozí</span><span class="sxs-lookup"><span data-stu-id="dbb70-165">Range and default</span></span> |
| --- | --- | --- |
| <span data-ttu-id="dbb70-166">**enableFileUploadNotifications**</span><span class="sxs-lookup"><span data-stu-id="dbb70-166">**enableFileUploadNotifications**</span></span> |<span data-ttu-id="dbb70-167">Určuje, zda oznámení o odeslání souboru se zapisují koncového bodu oznámení toohello souboru.</span><span class="sxs-lookup"><span data-stu-id="dbb70-167">Controls whether file upload notifications are written toohello file notifications endpoint.</span></span> |<span data-ttu-id="dbb70-168">Logická hodnota.</span><span class="sxs-lookup"><span data-stu-id="dbb70-168">Bool.</span></span> <span data-ttu-id="dbb70-169">Výchozí: True.</span><span class="sxs-lookup"><span data-stu-id="dbb70-169">Default: True.</span></span> |
| <span data-ttu-id="dbb70-170">**fileNotifications.ttlAsIso8601**</span><span class="sxs-lookup"><span data-stu-id="dbb70-170">**fileNotifications.ttlAsIso8601**</span></span> |<span data-ttu-id="dbb70-171">Výchozí hodnota TTL pro oznámení o odeslání souboru.</span><span class="sxs-lookup"><span data-stu-id="dbb70-171">Default TTL for file upload notifications.</span></span> |<span data-ttu-id="dbb70-172">Interval ISO_8601 až too48H (minimální 1 minutu).</span><span class="sxs-lookup"><span data-stu-id="dbb70-172">ISO_8601 interval up too48H (minimum 1 minute).</span></span> <span data-ttu-id="dbb70-173">Výchozí hodnota: 1 hodina.</span><span class="sxs-lookup"><span data-stu-id="dbb70-173">Default: 1 hour.</span></span> |
| <span data-ttu-id="dbb70-174">**fileNotifications.lockDuration**</span><span class="sxs-lookup"><span data-stu-id="dbb70-174">**fileNotifications.lockDuration**</span></span> |<span data-ttu-id="dbb70-175">Doba trvání uzamčení pro fronty pro odesílání oznámení hello souborů.</span><span class="sxs-lookup"><span data-stu-id="dbb70-175">Lock duration for hello file upload notifications queue.</span></span> |<span data-ttu-id="dbb70-176">5 sekund too300 (minimální 5 sekund).</span><span class="sxs-lookup"><span data-stu-id="dbb70-176">5 too300 seconds (minimum 5 seconds).</span></span> <span data-ttu-id="dbb70-177">Výchozí: 60 sekund.</span><span class="sxs-lookup"><span data-stu-id="dbb70-177">Default: 60 seconds.</span></span> |
| <span data-ttu-id="dbb70-178">**fileNotifications.maxDeliveryCount**</span><span class="sxs-lookup"><span data-stu-id="dbb70-178">**fileNotifications.maxDeliveryCount**</span></span> |<span data-ttu-id="dbb70-179">Počet maximální doručení pro hello soubor nahrát fronta oznámení.</span><span class="sxs-lookup"><span data-stu-id="dbb70-179">Maximum delivery count for hello file upload notification queue.</span></span> |<span data-ttu-id="dbb70-180">1 too100.</span><span class="sxs-lookup"><span data-stu-id="dbb70-180">1 too100.</span></span> <span data-ttu-id="dbb70-181">Výchozí: 100.</span><span class="sxs-lookup"><span data-stu-id="dbb70-181">Default: 100.</span></span> |

## <a name="additional-reference-material"></a><span data-ttu-id="dbb70-182">Odkaz na další materiály</span><span class="sxs-lookup"><span data-stu-id="dbb70-182">Additional reference material</span></span>

<span data-ttu-id="dbb70-183">Další témata referenční příručka vývojáře pro službu IoT Hub hello patří:</span><span class="sxs-lookup"><span data-stu-id="dbb70-183">Other reference topics in hello IoT Hub developer guide include:</span></span>

* <span data-ttu-id="dbb70-184">[Koncové body centra IoT] [ lnk-endpoints] popisuje hello různých koncových bodů, které každý IoT hub zpřístupní pro spuštění a management operace.</span><span class="sxs-lookup"><span data-stu-id="dbb70-184">[IoT Hub endpoints][lnk-endpoints] describes hello various endpoints that each IoT hub exposes for run-time and management operations.</span></span>
* <span data-ttu-id="dbb70-185">[Omezování a kvóty] [ lnk-quotas] popisuje hello kvót a omezování chování, které se vztahují toohello služby IoT Hub.</span><span class="sxs-lookup"><span data-stu-id="dbb70-185">[Throttling and quotas][lnk-quotas] describes hello quotas and throttling behaviors that apply toohello IoT Hub service.</span></span>
* <span data-ttu-id="dbb70-186">[Azure IoT zařízení a služby sady SDK] [ lnk-sdks] seznamy hello různé jazykové sady SDK můžete použít při vývoji aplikace zařízení a služby, které interakci s centrem IoT.</span><span class="sxs-lookup"><span data-stu-id="dbb70-186">[Azure IoT device and service SDKs][lnk-sdks] lists hello various language SDKs you can use when you develop both device and service apps that interact with IoT Hub.</span></span>
* <span data-ttu-id="dbb70-187">[IoT Hub dotazovací jazyk] [ lnk-query] popisuje hello dotazovací jazyk tooretrieve informace ze služby IoT Hub o úlohách a dvojčata zařízení můžete použít.</span><span class="sxs-lookup"><span data-stu-id="dbb70-187">[IoT Hub query language][lnk-query] describes hello query language you can use tooretrieve information from IoT Hub about your device twins and jobs.</span></span>
* <span data-ttu-id="dbb70-188">[Podpora IoT Hub MQTT] [ lnk-devguide-mqtt] poskytuje další informace o podpoře služby IoT Hub pro protokol MQTT hello.</span><span class="sxs-lookup"><span data-stu-id="dbb70-188">[IoT Hub MQTT support][lnk-devguide-mqtt] provides more information about IoT Hub support for hello MQTT protocol.</span></span>

## <a name="next-steps"></a><span data-ttu-id="dbb70-189">Další kroky</span><span class="sxs-lookup"><span data-stu-id="dbb70-189">Next steps</span></span>

<span data-ttu-id="dbb70-190">Nyní jste se naučili, jak tooupload soubory ze zařízení pomocí služby IoT Hub, může být zájem o hello následující témata Průvodce vývojáře IoT Hub:</span><span class="sxs-lookup"><span data-stu-id="dbb70-190">Now you have learned how tooupload files from devices using IoT Hub, you may be interested in hello following IoT Hub developer guide topics:</span></span>

* <span data-ttu-id="dbb70-191">[Správa identit zařízení IoT hub][lnk-devguide-identities]</span><span class="sxs-lookup"><span data-stu-id="dbb70-191">[Manage device identities in IoT Hub][lnk-devguide-identities]</span></span>
* <span data-ttu-id="dbb70-192">[Řízení přístupu tooIoT rozbočovače][lnk-devguide-security]</span><span class="sxs-lookup"><span data-stu-id="dbb70-192">[Control access tooIoT Hub][lnk-devguide-security]</span></span>
* <span data-ttu-id="dbb70-193">[Používání stavu toosynchronize dvojčata zařízení a konfigurací][lnk-devguide-device-twins]</span><span class="sxs-lookup"><span data-stu-id="dbb70-193">[Use device twins toosynchronize state and configurations][lnk-devguide-device-twins]</span></span>
* <span data-ttu-id="dbb70-194">[Volání metody přímé na zařízení][lnk-devguide-directmethods]</span><span class="sxs-lookup"><span data-stu-id="dbb70-194">[Invoke a direct method on a device][lnk-devguide-directmethods]</span></span>
* <span data-ttu-id="dbb70-195">[Plánování úloh na několika zařízeních][lnk-devguide-jobs]</span><span class="sxs-lookup"><span data-stu-id="dbb70-195">[Schedule jobs on multiple devices][lnk-devguide-jobs]</span></span>

<span data-ttu-id="dbb70-196">Pokud chcete tootry některé z hello konceptů popsaných v tomto článku, může být zájem o hello následující kurzu IoT Hub:</span><span class="sxs-lookup"><span data-stu-id="dbb70-196">If you would like tootry out some of hello concepts described in this article, you may be interested in hello following IoT Hub tutorial:</span></span>

* <span data-ttu-id="dbb70-197">[Jak tooupload soubory ze zařízení toohello cloudové službou IoT Hub][lnk-fileupload-tutorial]</span><span class="sxs-lookup"><span data-stu-id="dbb70-197">[How tooupload files from devices toohello cloud with IoT Hub][lnk-fileupload-tutorial]</span></span>

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
