---
title: "Pochopení nahrávání souborů Azure IoT Hub | Microsoft Docs"
description: "Příručka vývojáře - použijte funkci nahrávání souboru IoT Hub pro správu nahrávání souborů ze zařízení do kontejner objektu blob úložiště Azure."
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
ms.openlocfilehash: 75a6b9bc3ecfe6d6901bb38e312d62333f38daf1
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="upload-files-with-iot-hub"></a><span data-ttu-id="42c30-103">Nahrání souborů s centrem IoT</span><span class="sxs-lookup"><span data-stu-id="42c30-103">Upload files with IoT Hub</span></span>

<span data-ttu-id="42c30-104">Podle popisu v [koncové body centra IoT] [ lnk-endpoints] článku zařízení můžete iniciovat načtení souboru odesílání oznámení prostřednictvím koncového bodu směřujících zařízení (**/devices/ {deviceId} / soubory**).</span><span class="sxs-lookup"><span data-stu-id="42c30-104">As detailed in the [IoT Hub endpoints][lnk-endpoints] article, a device can initiate a file upload by sending a notification through a device-facing endpoint (**/devices/{deviceId}/files**).</span></span> <span data-ttu-id="42c30-105">Když zařízení oznámí IoT Hub, nahrávaný je dokončena, IoT Hub odešle zprávu souboru odesílání oznámení prostřednictvím **/messages/servicebound/filenotifications** koncový bod služby přístupem.</span><span class="sxs-lookup"><span data-stu-id="42c30-105">When a device notifies IoT Hub that an upload is complete, IoT Hub sends a file upload notification message through the **/messages/servicebound/filenotifications** service-facing endpoint.</span></span>

<span data-ttu-id="42c30-106">Místo zprostředkovatelské zprávy prostřednictvím služby IoT Hub, samotné místo toho IoT Hub funguje jako dispečera do přidruženého účtu úložiště Azure.</span><span class="sxs-lookup"><span data-stu-id="42c30-106">Instead of brokering messages through IoT Hub itself, IoT Hub instead acts as a dispatcher to an associated Azure Storage account.</span></span> <span data-ttu-id="42c30-107">Zařízení požadavků úložiště token ze služby IoT Hub, která je specifická pro soubor, který zařízení, které chcete nahrát.</span><span class="sxs-lookup"><span data-stu-id="42c30-107">A device requests a storage token from IoT Hub that is specific to the file the device wishes to upload.</span></span> <span data-ttu-id="42c30-108">Zařízení používá identifikátor URI SAS nahrát soubor do úložiště a po dokončení nahrávání zařízení odesílá do služby IoT Hub oznámení o dokončení.</span><span class="sxs-lookup"><span data-stu-id="42c30-108">The device uses the SAS URI to upload the file to storage, and when the upload is complete the device sends a notification of completion to IoT Hub.</span></span> <span data-ttu-id="42c30-109">IoT Hub zkontroluje nahrávání souborů je a pak přidá soubor odesílání oznámení do koncového bodu služby směřujících soubor oznámení.</span><span class="sxs-lookup"><span data-stu-id="42c30-109">IoT Hub checks the file upload is complete and then adds a file upload notification message to the service-facing file notification endpoint.</span></span>

<span data-ttu-id="42c30-110">Nahrát soubor do služby IoT Hub ze zařízení, musíte nakonfigurovat pomocí vašeho centra [přidružení Azure Storage] [ lnk-associate-storage] účet k němu.</span><span class="sxs-lookup"><span data-stu-id="42c30-110">Before you upload a file to IoT Hub from a device, you must configure your hub by [associating an Azure Storage][lnk-associate-storage] account to it.</span></span>

<span data-ttu-id="42c30-111">Pak můžete zařízení [inicializovat nahrávaný] [ lnk-initialize] a potom [upozornění služby IoT hub] [ lnk-notify] po dokončení nahrávání.</span><span class="sxs-lookup"><span data-stu-id="42c30-111">Your device can then [initialize an upload][lnk-initialize] and then [notify IoT hub][lnk-notify] when the upload completes.</span></span> <span data-ttu-id="42c30-112">Volitelně, když zařízení oznámí IoT Hub, nahrávání je dokončena, služba může generovat [oznámení][lnk-service-notification].</span><span class="sxs-lookup"><span data-stu-id="42c30-112">Optionally, when a device notifies IoT Hub that the upload is complete, the service can generate a [notification message][lnk-service-notification].</span></span>

### <a name="when-to-use"></a><span data-ttu-id="42c30-113">Kdy je použít</span><span class="sxs-lookup"><span data-stu-id="42c30-113">When to use</span></span>

<span data-ttu-id="42c30-114">Nahrávání souborů použijte k odesílání souborů médií a velké telemetrie dávek odeslaný občas připojená zařízení nebo Komprimovat pro snížení šířky pásma.</span><span class="sxs-lookup"><span data-stu-id="42c30-114">Use file upload to send media files and large telemetry batches uploaded by intermittently connected devices or compressed to save bandwidth.</span></span>

<span data-ttu-id="42c30-115">Odkazovat na [pokyny komunikace zařízení cloud] [ lnk-d2c-guidance] Pokud máte pochybnosti mezi použitím hlášen vlastnostech, zpráv typu zařízení cloud nebo nahrávání souborů.</span><span class="sxs-lookup"><span data-stu-id="42c30-115">Refer to [Device-to-cloud communication guidance][lnk-d2c-guidance] if in doubt between using reported properties, device-to-cloud messages, or file upload.</span></span>

## <a name="associate-an-azure-storage-account-with-iot-hub"></a><span data-ttu-id="42c30-116">Přidružit účet úložiště Azure IoT Hub</span><span class="sxs-lookup"><span data-stu-id="42c30-116">Associate an Azure Storage account with IoT Hub</span></span>

<span data-ttu-id="42c30-117">Pokud chcete používat funkci nahrávání souboru, je nutné nejprve propojit účet úložiště Azure do služby IoT Hub.</span><span class="sxs-lookup"><span data-stu-id="42c30-117">To use the file upload functionality, you must first link an Azure Storage account to the IoT Hub.</span></span> <span data-ttu-id="42c30-118">Můžete tuto úlohu dokončit buď prostřednictvím [portál Azure][lnk-management-portal], nebo programově pomocí [zprostředkovatele prostředků služby IoT Hub rozhraní REST API][lnk-resource-provider-apis].</span><span class="sxs-lookup"><span data-stu-id="42c30-118">You can complete this task either through the [Azure portal][lnk-management-portal], or programmatically through the [IoT Hub resource provider REST APIs][lnk-resource-provider-apis].</span></span> <span data-ttu-id="42c30-119">Po přidružený účet úložiště Azure IoT Hub, službu vrátí identifikátor URI pro SAS na zařízení, pokud zařízení zahájí žádost o odeslání souboru.</span><span class="sxs-lookup"><span data-stu-id="42c30-119">Once you have associated an Azure Storage account with your IoT Hub, the service returns a SAS URI to a device when the device initiates a file upload request.</span></span>

> [!NOTE]
> <span data-ttu-id="42c30-120">[SDK služby Azure IoT] [ lnk-sdks] automaticky zpracování načítání identifikátor URI SAS, nahrávání souboru a oznamování IoT Hub dokončená nahrávání.</span><span class="sxs-lookup"><span data-stu-id="42c30-120">The [Azure IoT SDKs][lnk-sdks] automatically handle retrieving the SAS URI, uploading the file, and notifying IoT Hub of a completed upload.</span></span>


## <a name="initialize-a-file-upload"></a><span data-ttu-id="42c30-121">Inicializace nahrávání souborů</span><span class="sxs-lookup"><span data-stu-id="42c30-121">Initialize a file upload</span></span>
<span data-ttu-id="42c30-122">IoT Hub má koncový bod speciálně pro zařízení do požadavku URI SAS pro úložiště pro nahrání souboru.</span><span class="sxs-lookup"><span data-stu-id="42c30-122">IoT Hub has an endpoint specifically for devices to request a SAS URI for storage to upload a file.</span></span> <span data-ttu-id="42c30-123">K zahájení procesu nahrávání souborů, zařízení, odešle požadavek POST do `{iot hub}.azure-devices.net/devices/{deviceId}/files` spolu s následujícím textem JSON:</span><span class="sxs-lookup"><span data-stu-id="42c30-123">To initiate the file upload process, the device sends a POST request to `{iot hub}.azure-devices.net/devices/{deviceId}/files` with the following JSON body:</span></span>

```json
{
    "blobName": "{name of the file for which a SAS URI will be generated}"
}
```

<span data-ttu-id="42c30-124">IoT Hub vrací následující data, která zařízení používá k odeslání souboru:</span><span class="sxs-lookup"><span data-stu-id="42c30-124">IoT Hub returns the following data, which the device uses to upload the file:</span></span>

```json
{
    "correlationId": "somecorrelationid",
    "hostName": "contoso.azure-devices.net",
    "containerName": "testcontainer",
    "blobName": "test-device1/image.jpg",
    "sasToken": "1234asdfSAStoken"
}
```

### <a name="deprecated-initialize-a-file-upload-with-a-get"></a><span data-ttu-id="42c30-125">Zastaralé: Inicializace nahrání souboru s GET</span><span class="sxs-lookup"><span data-stu-id="42c30-125">Deprecated: initialize a file upload with a GET</span></span>

> [!NOTE]
> <span data-ttu-id="42c30-126">Tato část popisuje, jak získat identifikátor URI SAS ze služby IoT Hub zastaralé funkce.</span><span class="sxs-lookup"><span data-stu-id="42c30-126">This section describes deprecated functionality for how to receive a SAS URI from IoT Hub.</span></span> <span data-ttu-id="42c30-127">Použijte metodu POST popsané.</span><span class="sxs-lookup"><span data-stu-id="42c30-127">Use the POST method described previously.</span></span>

<span data-ttu-id="42c30-128">IoT Hub má dva koncové body REST pro podporu nahrávání souborů, jednu pro získat identifikátor URI SAS pro úložiště a další služby IoT hub dokončené odeslání oznámení.</span><span class="sxs-lookup"><span data-stu-id="42c30-128">IoT Hub has two REST endpoints to support file upload, one to get the SAS URI for storage and the other to notify the IoT hub of a completed upload.</span></span> <span data-ttu-id="42c30-129">Zařízení iniciuje procesu nahrávání souboru zasláním GET do služby IoT hub v `{iot hub}.azure-devices.net/devices/{deviceId}/files/{filename}`.</span><span class="sxs-lookup"><span data-stu-id="42c30-129">The device initiates the file upload process by sending a GET to the IoT hub at `{iot hub}.azure-devices.net/devices/{deviceId}/files/{filename}`.</span></span> <span data-ttu-id="42c30-130">Služba IoT hub vrací:</span><span class="sxs-lookup"><span data-stu-id="42c30-130">The IoT hub returns:</span></span>

* <span data-ttu-id="42c30-131">URI specifické pro soubor SAS k odeslání.</span><span class="sxs-lookup"><span data-stu-id="42c30-131">A SAS URI specific to the file to be uploaded.</span></span>
* <span data-ttu-id="42c30-132">ID korelace, který se má použít po dokončení nahrávání.</span><span class="sxs-lookup"><span data-stu-id="42c30-132">A correlation ID to be used once the upload is completed.</span></span>

## <a name="notify-iot-hub-of-a-completed-file-upload"></a><span data-ttu-id="42c30-133">Oznámit IoT Hub odeslání dokončené souboru</span><span class="sxs-lookup"><span data-stu-id="42c30-133">Notify IoT Hub of a completed file upload</span></span>

<span data-ttu-id="42c30-134">Zařízení je zodpovědná za nahrát soubor do úložiště pomocí sady SDK úložiště Azure.</span><span class="sxs-lookup"><span data-stu-id="42c30-134">The device is responsible for uploading the file to storage using the Azure Storage SDKs.</span></span> <span data-ttu-id="42c30-135">Po dokončení nahrávání zařízení odešle požadavek POST do `{iot hub}.azure-devices.net/devices/{deviceId}/files/notifications` spolu s následujícím textem JSON:</span><span class="sxs-lookup"><span data-stu-id="42c30-135">When the upload is complete, the device sends a POST request to `{iot hub}.azure-devices.net/devices/{deviceId}/files/notifications` with the following JSON body:</span></span>

```json
{
    "correlationId": "{correlation ID received from the initial request}",
    "isSuccess": bool,
    "statusCode": XXX,
    "statusDescription": "Description of status"
}
```

<span data-ttu-id="42c30-136">Hodnota `isSuccess` je logická hodnota udávající, zda má soubor byl úspěšně odeslán.</span><span class="sxs-lookup"><span data-stu-id="42c30-136">The value of `isSuccess` is a Boolean representing whether the file was uploaded successfully.</span></span> <span data-ttu-id="42c30-137">Stavový kód pro `statusCode` je stav pro nahrávání souborů do úložiště a `statusDescription` odpovídá `statusCode`.</span><span class="sxs-lookup"><span data-stu-id="42c30-137">The status code for `statusCode` is the status for the upload of the file to storage, and the `statusDescription` corresponds to the `statusCode`.</span></span>

## <a name="reference-topics"></a><span data-ttu-id="42c30-138">Témata odkazů:</span><span class="sxs-lookup"><span data-stu-id="42c30-138">Reference topics:</span></span>

<span data-ttu-id="42c30-139">Následující referenční témata poskytují další informace o nahrávání souborů ze zařízení.</span><span class="sxs-lookup"><span data-stu-id="42c30-139">The following reference topics provide you with more information about uploading files from a device.</span></span>

## <a name="file-upload-notifications"></a><span data-ttu-id="42c30-140">Oznámení o odeslání souboru</span><span class="sxs-lookup"><span data-stu-id="42c30-140">File upload notifications</span></span>

<span data-ttu-id="42c30-141">Volitelně když zařízení oznámí IoT Hub, nahrávaný je dokončena, IoT Hub můžete vygenerovat zprávu oznámení, která obsahuje umístění a jeho název souboru.</span><span class="sxs-lookup"><span data-stu-id="42c30-141">Optionally, when a device notifies IoT Hub that an upload is complete, IoT Hub can generate a notification message that contains the name and storage location of the file.</span></span>

<span data-ttu-id="42c30-142">Jak je popsáno v [koncové body][lnk-endpoints], IoT Hub zajišťuje oznámení o odeslání souboru prostřednictvím koncového bodu služby přístupem (**/messages/servicebound/fileuploadnotifications**) jako zprávy.</span><span class="sxs-lookup"><span data-stu-id="42c30-142">As explained in [Endpoints][lnk-endpoints], IoT Hub delivers file upload notifications through a service-facing endpoint (**/messages/servicebound/fileuploadnotifications**) as messages.</span></span> <span data-ttu-id="42c30-143">Sémantika receive pro oznámení o odeslání souboru je stejný jako u zprávy typu cloud zařízení a mít stejný [životní cyklus zpráv][lnk-lifecycle].</span><span class="sxs-lookup"><span data-stu-id="42c30-143">The receive semantics for file upload notifications are the same as for cloud-to-device messages and have the same [message lifecycle][lnk-lifecycle].</span></span> <span data-ttu-id="42c30-144">Každou zprávu, získán z koncového bodu oznámení nahrávání souboru je záznam JSON s následujícími vlastnostmi:</span><span class="sxs-lookup"><span data-stu-id="42c30-144">Each message retrieved from the file upload notification endpoint is a JSON record with the following properties:</span></span>

| <span data-ttu-id="42c30-145">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="42c30-145">Property</span></span> | <span data-ttu-id="42c30-146">Popis</span><span class="sxs-lookup"><span data-stu-id="42c30-146">Description</span></span> |
| --- | --- |
| <span data-ttu-id="42c30-147">EnqueuedTimeUtc</span><span class="sxs-lookup"><span data-stu-id="42c30-147">EnqueuedTimeUtc</span></span> |<span data-ttu-id="42c30-148">Časové razítko označující vytvoření oznámení.</span><span class="sxs-lookup"><span data-stu-id="42c30-148">Timestamp indicating when the notification was created.</span></span> |
| <span data-ttu-id="42c30-149">ID zařízení</span><span class="sxs-lookup"><span data-stu-id="42c30-149">DeviceId</span></span> |<span data-ttu-id="42c30-150">**DeviceId** zařízení, která nahrát soubor.</span><span class="sxs-lookup"><span data-stu-id="42c30-150">**DeviceId** of the device which uploaded the file.</span></span> |
| <span data-ttu-id="42c30-151">BlobUri</span><span class="sxs-lookup"><span data-stu-id="42c30-151">BlobUri</span></span> |<span data-ttu-id="42c30-152">Identifikátor URI nahrávaný soubor.</span><span class="sxs-lookup"><span data-stu-id="42c30-152">URI of the uploaded file.</span></span> |
| <span data-ttu-id="42c30-153">BlobName</span><span class="sxs-lookup"><span data-stu-id="42c30-153">BlobName</span></span> |<span data-ttu-id="42c30-154">Název uloženého souboru.</span><span class="sxs-lookup"><span data-stu-id="42c30-154">Name of the uploaded file.</span></span> |
| <span data-ttu-id="42c30-155">LastUpdatedTime</span><span class="sxs-lookup"><span data-stu-id="42c30-155">LastUpdatedTime</span></span> |<span data-ttu-id="42c30-156">Časové razítko označující poslední aktualizace souboru.</span><span class="sxs-lookup"><span data-stu-id="42c30-156">Timestamp indicating when the file was last updated.</span></span> |
| <span data-ttu-id="42c30-157">BlobSizeInBytes</span><span class="sxs-lookup"><span data-stu-id="42c30-157">BlobSizeInBytes</span></span> |<span data-ttu-id="42c30-158">Velikost nahrávaný soubor.</span><span class="sxs-lookup"><span data-stu-id="42c30-158">Size of the uploaded file.</span></span> |

<span data-ttu-id="42c30-159">**Příklad**.</span><span class="sxs-lookup"><span data-stu-id="42c30-159">**Example**.</span></span> <span data-ttu-id="42c30-160">Tento příklad ukazuje, text soubor odeslat zprávu oznámení.</span><span class="sxs-lookup"><span data-stu-id="42c30-160">This example shows the body of a file upload notification message.</span></span>

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

## <a name="file-upload-notification-configuration-options"></a><span data-ttu-id="42c30-161">Možnosti konfigurace oznámení nahrávání souborů</span><span class="sxs-lookup"><span data-stu-id="42c30-161">File upload notification configuration options</span></span>

<span data-ttu-id="42c30-162">Každý IoT hub zpřístupní následující možnosti konfigurace pro oznámení o odeslání souboru:</span><span class="sxs-lookup"><span data-stu-id="42c30-162">Each IoT hub exposes the following configuration options for file upload notifications:</span></span>

| <span data-ttu-id="42c30-163">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="42c30-163">Property</span></span> | <span data-ttu-id="42c30-164">Popis</span><span class="sxs-lookup"><span data-stu-id="42c30-164">Description</span></span> | <span data-ttu-id="42c30-165">Rozsah a výchozí</span><span class="sxs-lookup"><span data-stu-id="42c30-165">Range and default</span></span> |
| --- | --- | --- |
| <span data-ttu-id="42c30-166">**enableFileUploadNotifications**</span><span class="sxs-lookup"><span data-stu-id="42c30-166">**enableFileUploadNotifications**</span></span> |<span data-ttu-id="42c30-167">Určuje, zda jsou oznámení o odeslání souboru zapisovány do koncového bodu oznámení souboru.</span><span class="sxs-lookup"><span data-stu-id="42c30-167">Controls whether file upload notifications are written to the file notifications endpoint.</span></span> |<span data-ttu-id="42c30-168">Logická hodnota.</span><span class="sxs-lookup"><span data-stu-id="42c30-168">Bool.</span></span> <span data-ttu-id="42c30-169">Výchozí: True.</span><span class="sxs-lookup"><span data-stu-id="42c30-169">Default: True.</span></span> |
| <span data-ttu-id="42c30-170">**fileNotifications.ttlAsIso8601**</span><span class="sxs-lookup"><span data-stu-id="42c30-170">**fileNotifications.ttlAsIso8601**</span></span> |<span data-ttu-id="42c30-171">Výchozí hodnota TTL pro oznámení o odeslání souboru.</span><span class="sxs-lookup"><span data-stu-id="42c30-171">Default TTL for file upload notifications.</span></span> |<span data-ttu-id="42c30-172">ISO_8601 interval až 48 H (minimální 1 minutu).</span><span class="sxs-lookup"><span data-stu-id="42c30-172">ISO_8601 interval up to 48H (minimum 1 minute).</span></span> <span data-ttu-id="42c30-173">Výchozí hodnota: 1 hodina.</span><span class="sxs-lookup"><span data-stu-id="42c30-173">Default: 1 hour.</span></span> |
| <span data-ttu-id="42c30-174">**fileNotifications.lockDuration**</span><span class="sxs-lookup"><span data-stu-id="42c30-174">**fileNotifications.lockDuration**</span></span> |<span data-ttu-id="42c30-175">Doba trvání uzamčení pro frontu oznámení nahrávání souboru.</span><span class="sxs-lookup"><span data-stu-id="42c30-175">Lock duration for the file upload notifications queue.</span></span> |<span data-ttu-id="42c30-176">5 do 300 sekund (minimální 5 sekund).</span><span class="sxs-lookup"><span data-stu-id="42c30-176">5 to 300 seconds (minimum 5 seconds).</span></span> <span data-ttu-id="42c30-177">Výchozí: 60 sekund.</span><span class="sxs-lookup"><span data-stu-id="42c30-177">Default: 60 seconds.</span></span> |
| <span data-ttu-id="42c30-178">**fileNotifications.maxDeliveryCount**</span><span class="sxs-lookup"><span data-stu-id="42c30-178">**fileNotifications.maxDeliveryCount**</span></span> |<span data-ttu-id="42c30-179">Počet maximální doručení pro soubor nahrát fronta oznámení.</span><span class="sxs-lookup"><span data-stu-id="42c30-179">Maximum delivery count for the file upload notification queue.</span></span> |<span data-ttu-id="42c30-180">1 až 100.</span><span class="sxs-lookup"><span data-stu-id="42c30-180">1 to 100.</span></span> <span data-ttu-id="42c30-181">Výchozí: 100.</span><span class="sxs-lookup"><span data-stu-id="42c30-181">Default: 100.</span></span> |

## <a name="additional-reference-material"></a><span data-ttu-id="42c30-182">Odkaz na další materiály</span><span class="sxs-lookup"><span data-stu-id="42c30-182">Additional reference material</span></span>

<span data-ttu-id="42c30-183">Další témata referenční příručka vývojáře IoT Hub patří:</span><span class="sxs-lookup"><span data-stu-id="42c30-183">Other reference topics in the IoT Hub developer guide include:</span></span>

* <span data-ttu-id="42c30-184">[Koncové body centra IoT] [ lnk-endpoints] popisuje různé koncových bodů, které každý IoT hub zpřístupní pro spuštění a management operace.</span><span class="sxs-lookup"><span data-stu-id="42c30-184">[IoT Hub endpoints][lnk-endpoints] describes the various endpoints that each IoT hub exposes for run-time and management operations.</span></span>
* <span data-ttu-id="42c30-185">[Omezování a kvóty] [ lnk-quotas] popisuje kvóty a omezení chování, které se vztahují ke službě IoT Hub.</span><span class="sxs-lookup"><span data-stu-id="42c30-185">[Throttling and quotas][lnk-quotas] describes the quotas and throttling behaviors that apply to the IoT Hub service.</span></span>
* <span data-ttu-id="42c30-186">[Azure IoT zařízení a služby sady SDK] [ lnk-sdks] uvádí různé jazykové sady SDK můžete použít při vývoji aplikace zařízení a služby, které interakci s centrem IoT.</span><span class="sxs-lookup"><span data-stu-id="42c30-186">[Azure IoT device and service SDKs][lnk-sdks] lists the various language SDKs you can use when you develop both device and service apps that interact with IoT Hub.</span></span>
* <span data-ttu-id="42c30-187">[IoT Hub dotazovací jazyk] [ lnk-query] popisuje dotazovací jazyk, můžete použít k načtení informací ze služby IoT Hub o úlohách a dvojčata zařízení.</span><span class="sxs-lookup"><span data-stu-id="42c30-187">[IoT Hub query language][lnk-query] describes the query language you can use to retrieve information from IoT Hub about your device twins and jobs.</span></span>
* <span data-ttu-id="42c30-188">[Podpora IoT Hub MQTT] [ lnk-devguide-mqtt] poskytuje další informace o podpoře služby IoT Hub pro protokol MQTT.</span><span class="sxs-lookup"><span data-stu-id="42c30-188">[IoT Hub MQTT support][lnk-devguide-mqtt] provides more information about IoT Hub support for the MQTT protocol.</span></span>

## <a name="next-steps"></a><span data-ttu-id="42c30-189">Další kroky</span><span class="sxs-lookup"><span data-stu-id="42c30-189">Next steps</span></span>

<span data-ttu-id="42c30-190">Nyní jste se naučili Postup nahrání souborů ze zařízení pomocí služby IoT Hub, může zajímat v následujících tématech Příručka vývojáře IoT Hub:</span><span class="sxs-lookup"><span data-stu-id="42c30-190">Now you have learned how to upload files from devices using IoT Hub, you may be interested in the following IoT Hub developer guide topics:</span></span>

* <span data-ttu-id="42c30-191">[Správa identit zařízení IoT hub][lnk-devguide-identities]</span><span class="sxs-lookup"><span data-stu-id="42c30-191">[Manage device identities in IoT Hub][lnk-devguide-identities]</span></span>
* <span data-ttu-id="42c30-192">[Řízení přístupu ke službě IoT Hub][lnk-devguide-security]</span><span class="sxs-lookup"><span data-stu-id="42c30-192">[Control access to IoT Hub][lnk-devguide-security]</span></span>
* <span data-ttu-id="42c30-193">[Pomocí dvojčata zařízení synchronizovat stavu a konfigurace][lnk-devguide-device-twins]</span><span class="sxs-lookup"><span data-stu-id="42c30-193">[Use device twins to synchronize state and configurations][lnk-devguide-device-twins]</span></span>
* <span data-ttu-id="42c30-194">[Volání metody přímé na zařízení][lnk-devguide-directmethods]</span><span class="sxs-lookup"><span data-stu-id="42c30-194">[Invoke a direct method on a device][lnk-devguide-directmethods]</span></span>
* <span data-ttu-id="42c30-195">[Plánování úloh na několika zařízeních][lnk-devguide-jobs]</span><span class="sxs-lookup"><span data-stu-id="42c30-195">[Schedule jobs on multiple devices][lnk-devguide-jobs]</span></span>

<span data-ttu-id="42c30-196">Pokud chcete vyzkoušet některé konceptů popsaných v tomto článku, může zajímat v následujícím kurzu IoT Hub:</span><span class="sxs-lookup"><span data-stu-id="42c30-196">If you would like to try out some of the concepts described in this article, you may be interested in the following IoT Hub tutorial:</span></span>

* <span data-ttu-id="42c30-197">[Postup nahrání souborů ze zařízení do cloudu s centrem IoT][lnk-fileupload-tutorial]</span><span class="sxs-lookup"><span data-stu-id="42c30-197">[How to upload files from devices to the cloud with IoT Hub][lnk-fileupload-tutorial]</span></span>

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
