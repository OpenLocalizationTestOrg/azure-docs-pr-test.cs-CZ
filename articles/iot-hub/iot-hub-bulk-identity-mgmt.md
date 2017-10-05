---
title: "Import a export identit zařízení Azure IoT Hub | Microsoft Docs"
description: "Jak používat sady SDK služby Azure IoT provádět hromadné operace proti registru identit pro import a export identit zařízení. Operace importu umožňují vytvářet, aktualizovat a odstraňovat identit zařízení hromadně."
services: iot-hub
documentationcenter: .net
author: dominicbetts
manager: timlt
editor: 
ms.assetid: 2ade1494-45ea-46a7-ade7-cf6e11ce62da
ms.service: iot-hub
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/03/2017
ms.author: dobett
ms.openlocfilehash: ad2c6d585eef5450f7f0912ffa4753fe80d86b37
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="manage-your-iot-hub-device-identities-in-bulk"></a><span data-ttu-id="5f09b-104">Spravovat vaše identit zařízení IoT Hub hromadně</span><span class="sxs-lookup"><span data-stu-id="5f09b-104">Manage your IoT Hub device identities in bulk</span></span>

<span data-ttu-id="5f09b-105">Každé centrum IoT má registru identit, které můžete použít k vytvoření prostředků na zařízení ve službě.</span><span class="sxs-lookup"><span data-stu-id="5f09b-105">Each IoT hub has an identity registry you can use to create per-device resources in the service.</span></span> <span data-ttu-id="5f09b-106">Registr identit umožňuje řídit přístup k zařízení přístupem koncových bodů.</span><span class="sxs-lookup"><span data-stu-id="5f09b-106">The identity registry also enables you to control access to the device-facing endpoints.</span></span> <span data-ttu-id="5f09b-107">Tento článek popisuje, jak importovat a exportovat identit zařízení hromadné do a z registru identit.</span><span class="sxs-lookup"><span data-stu-id="5f09b-107">This article describes how to import and export device identities in bulk to and from an identity registry.</span></span>

<span data-ttu-id="5f09b-108">Import a export operace proběhla v kontextu *úlohy* které umožňují provádět hromadné operace služby proti služby IoT hub.</span><span class="sxs-lookup"><span data-stu-id="5f09b-108">Import and export operations take place in the context of *Jobs* that enable you to execute bulk service operations against an IoT hub.</span></span>

<span data-ttu-id="5f09b-109">**RegistryManager** třída zahrnuje **ExportDevicesAsync** a **ImportDevicesAsync** metody, které používají **úlohy** framework.</span><span class="sxs-lookup"><span data-stu-id="5f09b-109">The **RegistryManager** class includes the **ExportDevicesAsync** and **ImportDevicesAsync** methods that use the **Job** framework.</span></span> <span data-ttu-id="5f09b-110">Tyto metody umožňují exportovat, import a synchronizaci celého registru identit IoT hub.</span><span class="sxs-lookup"><span data-stu-id="5f09b-110">These methods enable you to export, import, and synchronize the entirety of an IoT hub identity registry.</span></span>

## <a name="what-are-jobs"></a><span data-ttu-id="5f09b-111">Jaké jsou úlohy?</span><span class="sxs-lookup"><span data-stu-id="5f09b-111">What are jobs?</span></span>

<span data-ttu-id="5f09b-112">Operace s registrem identit pomocí **úlohy** systému při operaci:</span><span class="sxs-lookup"><span data-stu-id="5f09b-112">Identity registry operations use the **Job** system when the operation:</span></span>

* <span data-ttu-id="5f09b-113">Má potenciálně dlouhou dobu spuštění ve srovnání s standardní spuštění operace.</span><span class="sxs-lookup"><span data-stu-id="5f09b-113">Has a potentially long execution time compared to standard run-time operations.</span></span>
* <span data-ttu-id="5f09b-114">Vrátí velký objem dat pro uživatele.</span><span class="sxs-lookup"><span data-stu-id="5f09b-114">Returns a large amount of data to the user.</span></span>

<span data-ttu-id="5f09b-115">Místo jednoho volání rozhraní API čekání nebo blokování na výsledek operace, operace asynchronně vytvoří **úlohy** tohoto centra IoT.</span><span class="sxs-lookup"><span data-stu-id="5f09b-115">Instead of a single API call waiting or blocking on the result of the operation, the operation asynchronously creates a **Job** for that IoT hub.</span></span> <span data-ttu-id="5f09b-116">Operace pak okamžitě vrátí **JobProperties** objektu.</span><span class="sxs-lookup"><span data-stu-id="5f09b-116">The operation then immediately returns a **JobProperties** object.</span></span>

<span data-ttu-id="5f09b-117">Následující fragment kódu jazyka C# ukazuje postup vytvoření úlohy exportu:</span><span class="sxs-lookup"><span data-stu-id="5f09b-117">The following C# code snippet shows how to create an export job:</span></span>

```csharp
// Call an export job on the IoT Hub to retrieve all devices
JobProperties exportJob = await registryManager.ExportDevicesAsync(containerSasUri, false);
```

> [!NOTE]
> <span data-ttu-id="5f09b-118">Použít **RegistryManager** třídy v kódu jazyka C#, přidejte **Microsoft.Azure.Devices** balíček NuGet do projektu.</span><span class="sxs-lookup"><span data-stu-id="5f09b-118">To use the **RegistryManager** class in your C# code, add the **Microsoft.Azure.Devices** NuGet package to your project.</span></span> <span data-ttu-id="5f09b-119">**RegistryManager** třída je v **Microsoft.Azure.Devices** oboru názvů.</span><span class="sxs-lookup"><span data-stu-id="5f09b-119">The **RegistryManager** class is in the **Microsoft.Azure.Devices** namespace.</span></span>

<span data-ttu-id="5f09b-120">Můžete použít **RegistryManager** třída k dotazování na stav systému **úlohy** pomocí vráceného **JobProperties** metadat.</span><span class="sxs-lookup"><span data-stu-id="5f09b-120">You can use the **RegistryManager** class to query the state of the **Job** using the returned **JobProperties** metadata.</span></span>

<span data-ttu-id="5f09b-121">Následující fragment kódu jazyka C# ukazuje, jak pokud chcete zobrazit, pokud úloha dokončí provádění dotazování každých pět sekund:</span><span class="sxs-lookup"><span data-stu-id="5f09b-121">The following C# code snippet shows how to poll every five seconds to see if the job has finished executing:</span></span>

```csharp
// Wait until job is finished
while(true)
{
  exportJob = await registryManager.GetJobAsync(exportJob.JobId);
  if (exportJob.Status == JobStatus.Completed || 
      exportJob.Status == JobStatus.Failed ||
      exportJob.Status == JobStatus.Cancelled)
  {
    // Job has finished executing
    break;
  }

  await Task.Delay(TimeSpan.FromSeconds(5));
}
```

## <a name="export-devices"></a><span data-ttu-id="5f09b-122">Export zařízení</span><span class="sxs-lookup"><span data-stu-id="5f09b-122">Export devices</span></span>

<span data-ttu-id="5f09b-123">Použití **ExportDevicesAsync** metodu exportu celého IoT hub identity registru [Azure Storage](../storage/index.md) pomocí kontejneru objektů blob [sdíleného přístupového podpisu](../storage/common/storage-security-guide.md#data-plane-security).</span><span class="sxs-lookup"><span data-stu-id="5f09b-123">Use the **ExportDevicesAsync** method to export the entirety of an IoT hub identity registry to an [Azure Storage](../storage/index.md) blob container using a [Shared Access Signature](../storage/common/storage-security-guide.md#data-plane-security).</span></span>

<span data-ttu-id="5f09b-124">Tato metoda umožňuje vytvářet spolehlivá zálohy zařízení informací v kontejneru objektů blob, který ovládáte.</span><span class="sxs-lookup"><span data-stu-id="5f09b-124">This method enables you to create reliable backups of your device information in a blob container that you control.</span></span>

<span data-ttu-id="5f09b-125">**ExportDevicesAsync** metoda vyžaduje dva parametry:</span><span class="sxs-lookup"><span data-stu-id="5f09b-125">The **ExportDevicesAsync** method requires two parameters:</span></span>

* <span data-ttu-id="5f09b-126">A *řetězec* obsahující identifikátor URI kontejner objektů blob.</span><span class="sxs-lookup"><span data-stu-id="5f09b-126">A *string* that contains a URI of a blob container.</span></span> <span data-ttu-id="5f09b-127">Tento identifikátor URI musí obsahovat token SAS, která uděluje oprávnění k zápisu do kontejneru.</span><span class="sxs-lookup"><span data-stu-id="5f09b-127">This URI must contain a SAS token that grants write access to the container.</span></span> <span data-ttu-id="5f09b-128">Tato úloha vytvoří objekt blob bloku v tomto kontejneru pro ukládání dat zařízení serializovaných export.</span><span class="sxs-lookup"><span data-stu-id="5f09b-128">The job creates a block blob in this container to store the serialized export device data.</span></span> <span data-ttu-id="5f09b-129">SAS token musí zahrnovat tato oprávnění:</span><span class="sxs-lookup"><span data-stu-id="5f09b-129">The SAS token must include these permissions:</span></span>

   ```csharp
   SharedAccessBlobPermissions.Write | SharedAccessBlobPermissions.Read | SharedAccessBlobPermissions.Delete
   ```

* <span data-ttu-id="5f09b-130">A *boolean* určující, zda chcete vyloučit ověřovací klíče exportovat data.</span><span class="sxs-lookup"><span data-stu-id="5f09b-130">A *boolean* that indicates if you want to exclude authentication keys from your export data.</span></span> <span data-ttu-id="5f09b-131">Pokud **false**, ověřovací klíče jsou zahrnuty ve výstupu export.</span><span class="sxs-lookup"><span data-stu-id="5f09b-131">If **false**, authentication keys are included in export output.</span></span> <span data-ttu-id="5f09b-132">Jinak, jsou klíče exportovány jako **null**.</span><span class="sxs-lookup"><span data-stu-id="5f09b-132">Otherwise, keys are exported as **null**.</span></span>

<span data-ttu-id="5f09b-133">Následující fragment kódu jazyka C# ukazuje, jak spustit úlohy exportu obsahující klíče ověřování zařízení v exportu dat a pak dotazování na dokončení:</span><span class="sxs-lookup"><span data-stu-id="5f09b-133">The following C# code snippet shows how to initiate an export job that includes device authentication keys in the export data and then poll for completion:</span></span>

```csharp
// Call an export job on the IoT Hub to retrieve all devices
JobProperties exportJob = await registryManager.ExportDevicesAsync(containerSasUri, false);

// Wait until job is finished
while(true)
{
    exportJob = await registryManager.GetJobAsync(exportJob.JobId);
    if (exportJob.Status == JobStatus.Completed || 
        exportJob.Status == JobStatus.Failed ||
        exportJob.Status == JobStatus.Cancelled)
    {
    // Job has finished executing
    break;
    }

    await Task.Delay(TimeSpan.FromSeconds(5));
}
```

<span data-ttu-id="5f09b-134">Úloha ukládá její výstup v kontejneru objektů blob zadané jako objekt blob bloku s názvem **devices.txt**.</span><span class="sxs-lookup"><span data-stu-id="5f09b-134">The job stores its output in the provided blob container as a block blob with the name **devices.txt**.</span></span> <span data-ttu-id="5f09b-135">Výstupní data se skládá z data zařízení serializován do formátu JSON, s jedno zařízení na každý řádek.</span><span class="sxs-lookup"><span data-stu-id="5f09b-135">The output data consists of JSON serialized device data, with one device per line.</span></span>

<span data-ttu-id="5f09b-136">Následující příklad ukazuje výstupní data:</span><span class="sxs-lookup"><span data-stu-id="5f09b-136">The following example shows the output data:</span></span>

```json
{"id":"Device1","eTag":"MA==","status":"enabled","authentication":{"symmetricKey":{"primaryKey":"abc=","secondaryKey":"def="}}}
{"id":"Device2","eTag":"MA==","status":"enabled","authentication":{"symmetricKey":{"primaryKey":"abc=","secondaryKey":"def="}}}
{"id":"Device3","eTag":"MA==","status":"disabled","authentication":{"symmetricKey":{"primaryKey":"abc=","secondaryKey":"def="}}}
{"id":"Device4","eTag":"MA==","status":"disabled","authentication":{"symmetricKey":{"primaryKey":"abc=","secondaryKey":"def="}}}
{"id":"Device5","eTag":"MA==","status":"enabled","authentication":{"symmetricKey":{"primaryKey":"abc=","secondaryKey":"def="}}}
```

Pokud zařízení obsahuje twin dat, twin data také exportovat společně s dat zařízení. Následující příklad ukazuje tento formát. <span data-ttu-id="5f09b-139">Všechna data z řádku "twinETag" až do konce jsou twin data.</span><span class="sxs-lookup"><span data-stu-id="5f09b-139">All data from the "twinETag" line until the end are twin data.</span></span>

```json
{
   "id":"export-6d84f075-0",
   "eTag":"MQ==",
   "status":"enabled",
   "statusReason":"firstUpdate",
   "authentication":null,
   "twinETag":"AAAAAAAAAAI=",
   "tags":{
      "Location":"LivingRoom"
   },
   "properties":{
      "desired":{
         "Thermostat":{
            "Temperature":75.1,
            "Unit":"F"
         },
         "$metadata":{
            "$lastUpdated":"2017-03-09T18:30:52.3167248Z",
            "$lastUpdatedVersion":2,
            "Thermostat":{
               "$lastUpdated":"2017-03-09T18:30:52.3167248Z",
               "$lastUpdatedVersion":2,
               "Temperature":{
                  "$lastUpdated":"2017-03-09T18:30:52.3167248Z",
                  "$lastUpdatedVersion":2
               },
               "Unit":{
                  "$lastUpdated":"2017-03-09T18:30:52.3167248Z",
                  "$lastUpdatedVersion":2
               }
            }
         },
         "$version":2
      },
      "reported":{
         "$metadata":{
            "$lastUpdated":"2017-03-09T18:30:51.1309437Z"
         },
         "$version":1
      }
   }
}
```

<span data-ttu-id="5f09b-140">Pokud budete potřebovat přístup k těmto datům v kódu, můžete snadno rekonstruovat tato data pomocí **ExportImportDevice** třídy.</span><span class="sxs-lookup"><span data-stu-id="5f09b-140">If you need access to this data in code, you can easily deserialize this data using the **ExportImportDevice** class.</span></span> <span data-ttu-id="5f09b-141">Následující fragment kódu jazyka C# ukazuje, jak číst informace o zařízení, který jste předtím exportovali do objektu blob bloku:</span><span class="sxs-lookup"><span data-stu-id="5f09b-141">The following C# code snippet shows how to read device information that was previously exported to a block blob:</span></span>

```csharp
var exportedDevices = new List<ExportImportDevice>();

using (var streamReader = new StreamReader(await blob.OpenReadAsync(AccessCondition.GenerateIfExistsCondition(), null, null), Encoding.UTF8))
{
  while (streamReader.Peek() != -1)
  {
    string line = await streamReader.ReadLineAsync();
    var device = JsonConvert.DeserializeObject<ExportImportDevice>(line);
    exportedDevices.Add(device);
  }
}
```

> [!NOTE]
> <span data-ttu-id="5f09b-142">Můžete také **GetDevicesAsync** metodu **RegistryManager** třída načíst seznam zařízení.</span><span class="sxs-lookup"><span data-stu-id="5f09b-142">You can also use the **GetDevicesAsync** method of the **RegistryManager** class to fetch a list of your devices.</span></span> <span data-ttu-id="5f09b-143">Však tento přístup má pevný cap 1000 na počtu zařízení objekty, které jsou vráceny.</span><span class="sxs-lookup"><span data-stu-id="5f09b-143">However, this approach has a hard cap of 1000 on the number of device objects that are returned.</span></span> <span data-ttu-id="5f09b-144">Případ použití očekávané **GetDevicesAsync** metoda je pro vývojové scénáře, které pomáhají ladění a se nedoporučuje pro produkční zatížení.</span><span class="sxs-lookup"><span data-stu-id="5f09b-144">The expected use case for the **GetDevicesAsync** method is for development scenarios to aid debugging and is not recommended for production workloads.</span></span>

## <a name="import-devices"></a><span data-ttu-id="5f09b-145">Import zařízení</span><span class="sxs-lookup"><span data-stu-id="5f09b-145">Import devices</span></span>

<span data-ttu-id="5f09b-146">**ImportDevicesAsync** metoda v **RegistryManager** vám umožňuje provádět operace hromadného importu a synchronizaci v registru identit IoT hub.</span><span class="sxs-lookup"><span data-stu-id="5f09b-146">The **ImportDevicesAsync** method in the **RegistryManager** class enables you to perform bulk import and synchronization operations in an IoT hub identity registry.</span></span> <span data-ttu-id="5f09b-147">Jako **ExportDevicesAsync** metody **ImportDevicesAsync** metoda používá **úlohy** framework.</span><span class="sxs-lookup"><span data-stu-id="5f09b-147">Like the **ExportDevicesAsync** method, the **ImportDevicesAsync** method uses the **Job** framework.</span></span>

<span data-ttu-id="5f09b-148">Vezměte v potaz, pomocí **ImportDevicesAsync** metoda protože kromě zřizování nových zařízení v registru identit, můžete taky aktualizovat a odstranit existující zařízení.</span><span class="sxs-lookup"><span data-stu-id="5f09b-148">Take care using the **ImportDevicesAsync** method because in addition to provisioning new devices in your identity registry, it can also update and delete existing devices.</span></span>

> [!WARNING]
> <span data-ttu-id="5f09b-149">Operace importu nelze vrátit zpět.</span><span class="sxs-lookup"><span data-stu-id="5f09b-149">An import operation cannot be undone.</span></span> <span data-ttu-id="5f09b-150">Vždy zálohovat stávající data pomocí **ExportDevicesAsync** metodu pro jiný kontejner objektů blob, před provedením hromadné změny registru vaší identity.</span><span class="sxs-lookup"><span data-stu-id="5f09b-150">Always back up your existing data using the **ExportDevicesAsync** method to another blob container before you make bulk changes to your identity registry.</span></span>

<span data-ttu-id="5f09b-151">**ImportDevicesAsync** metoda přebírá dva parametry:</span><span class="sxs-lookup"><span data-stu-id="5f09b-151">The **ImportDevicesAsync** method takes two parameters:</span></span>

* <span data-ttu-id="5f09b-152">A *řetězec* identifikátor URI, který obsahuje [Azure Storage](../storage/index.md) kontejner objektů blob, které chcete použít jako *vstupní* do úlohy.</span><span class="sxs-lookup"><span data-stu-id="5f09b-152">A *string* that contains a URI of an [Azure Storage](../storage/index.md) blob container to use as *input* to the job.</span></span> <span data-ttu-id="5f09b-153">Tento identifikátor URI musí obsahovat token SAS, která uděluje přístup pro čtení ke kontejneru.</span><span class="sxs-lookup"><span data-stu-id="5f09b-153">This URI must contain a SAS token that grants read access to the container.</span></span> <span data-ttu-id="5f09b-154">Tento kontejner musí obsahovat objekt blob s názvem **devices.txt** obsahující data serializovaná zařízení určená k importu do registru identit.</span><span class="sxs-lookup"><span data-stu-id="5f09b-154">This container must contain a blob with the name **devices.txt** that contains the serialized device data to import into your identity registry.</span></span> <span data-ttu-id="5f09b-155">Import dat musí obsahovat informace o zařízení ve stejném formátu JSON, který **ExportImportDevice** úloha používá při vytváření **devices.txt** objektů blob.</span><span class="sxs-lookup"><span data-stu-id="5f09b-155">The import data must contain device information in the same JSON format that the **ExportImportDevice** job uses when it creates a **devices.txt** blob.</span></span> <span data-ttu-id="5f09b-156">SAS token musí zahrnovat tato oprávnění:</span><span class="sxs-lookup"><span data-stu-id="5f09b-156">The SAS token must include these permissions:</span></span>

   ```csharp
   SharedAccessBlobPermissions.Read
   ```
* <span data-ttu-id="5f09b-157">A *řetězec* identifikátor URI, který obsahuje [Azure Storage](https://azure.microsoft.com/documentation/services/storage/) kontejner objektů blob, které chcete použít jako *výstup* z úlohy.</span><span class="sxs-lookup"><span data-stu-id="5f09b-157">A *string* that contains a URI of an [Azure Storage](https://azure.microsoft.com/documentation/services/storage/) blob container to use as *output* from the job.</span></span> <span data-ttu-id="5f09b-158">Tato úloha vytvoří objekt blob bloku v tomto kontejneru ukládat všechny informace o chybě z dokončené import **úlohy**.</span><span class="sxs-lookup"><span data-stu-id="5f09b-158">The job creates a block blob in this container to store any error information from the completed import **Job**.</span></span> <span data-ttu-id="5f09b-159">SAS token musí zahrnovat tato oprávnění:</span><span class="sxs-lookup"><span data-stu-id="5f09b-159">The SAS token must include these permissions:</span></span>

   ```csharp
   SharedAccessBlobPermissions.Write | SharedAccessBlobPermissions.Read | SharedAccessBlobPermissions.Delete
   ```

> [!NOTE]
> <span data-ttu-id="5f09b-160">Dva parametry může ukazovat na stejném kontejneru objektů blob.</span><span class="sxs-lookup"><span data-stu-id="5f09b-160">The two parameters can point to the same blob container.</span></span> <span data-ttu-id="5f09b-161">Samostatné parametry jednoduše povolte větší kontrolu nad daty jako výstup kontejneru vyžaduje další oprávnění.</span><span class="sxs-lookup"><span data-stu-id="5f09b-161">The separate parameters simply enable more control over your data as the output container requires additional permissions.</span></span>

<span data-ttu-id="5f09b-162">Následující fragment kódu jazyka C# ukazuje, jak zahájit úlohy importu:</span><span class="sxs-lookup"><span data-stu-id="5f09b-162">The following C# code snippet shows how to initiate an import job:</span></span>

```csharp
JobProperties importJob = await registryManager.ImportDevicesAsync(containerSasUri, containerSasUri);
```

<span data-ttu-id="5f09b-163">Tato metoda slouží také k importu dat pro dvojče zařízení.</span><span class="sxs-lookup"><span data-stu-id="5f09b-163">This method can also be used to import the data for the device twin.</span></span> <span data-ttu-id="5f09b-164">Formát pro vložení dat je stejný jako formát ukazuje **ExportDevicesAsync** části.</span><span class="sxs-lookup"><span data-stu-id="5f09b-164">The format for the data input is the same as the format shown in the **ExportDevicesAsync** section.</span></span> <span data-ttu-id="5f09b-165">Tímto způsobem můžete znovu naimportovat exportovaná data.</span><span class="sxs-lookup"><span data-stu-id="5f09b-165">In this way, you can reimport the exported data.</span></span> <span data-ttu-id="5f09b-166">**$Metadata** je volitelný.</span><span class="sxs-lookup"><span data-stu-id="5f09b-166">The **$metadata** is optional.</span></span>

## <a name="import-behavior"></a><span data-ttu-id="5f09b-167">Import chování</span><span class="sxs-lookup"><span data-stu-id="5f09b-167">Import behavior</span></span>

<span data-ttu-id="5f09b-168">Můžete použít **ImportDevicesAsync** možností, jak provést následující operace hromadného v registru identit:</span><span class="sxs-lookup"><span data-stu-id="5f09b-168">You can use the **ImportDevicesAsync** method to perform the following bulk operations in your identity registry:</span></span>

* <span data-ttu-id="5f09b-169">Hromadné registrace nové zařízení</span><span class="sxs-lookup"><span data-stu-id="5f09b-169">Bulk registration of new devices</span></span>
* <span data-ttu-id="5f09b-170">Hromadné odstranění stávajících zařízení</span><span class="sxs-lookup"><span data-stu-id="5f09b-170">Bulk deletions of existing devices</span></span>
* <span data-ttu-id="5f09b-171">Hromadné změny stavu (povolit nebo zakázat zařízení)</span><span class="sxs-lookup"><span data-stu-id="5f09b-171">Bulk status changes (enable or disable devices)</span></span>
* <span data-ttu-id="5f09b-172">Hromadné přiřazení nových klíčů ověřování zařízení</span><span class="sxs-lookup"><span data-stu-id="5f09b-172">Bulk assignment of new device authentication keys</span></span>
* <span data-ttu-id="5f09b-173">Hromadné auto-nové vygenerování klíče pro ověřování zařízení</span><span class="sxs-lookup"><span data-stu-id="5f09b-173">Bulk auto-regeneration of device authentication keys</span></span>
* <span data-ttu-id="5f09b-174">Hromadné aktualizace dat twin</span><span class="sxs-lookup"><span data-stu-id="5f09b-174">Bulk update of twin data</span></span>

<span data-ttu-id="5f09b-175">Může vykonávat jakoukoli kombinaci předchozí operace v rámci jednoho **ImportDevicesAsync** volání.</span><span class="sxs-lookup"><span data-stu-id="5f09b-175">You can perform any combination of the preceding operations within a single **ImportDevicesAsync** call.</span></span> <span data-ttu-id="5f09b-176">Můžete například zaregistrovat nové zařízení a odstranění nebo aktualizaci existujících zařízení pracujících s ve stejnou dobu.</span><span class="sxs-lookup"><span data-stu-id="5f09b-176">For example, you can register new devices and delete or update existing devices at the same time.</span></span> <span data-ttu-id="5f09b-177">Při použití spolu s **ExportDevicesAsync** metody úplně můžete migrovat všechna vaše zařízení z jedné služby IoT hub do jiného.</span><span class="sxs-lookup"><span data-stu-id="5f09b-177">When used along with the **ExportDevicesAsync** method, you can completely migrate all your devices from one IoT hub to another.</span></span>

<span data-ttu-id="5f09b-178">Pokud soubor importu obsahuje twin metadata, tato metadata přepíše existující twin metadata.</span><span class="sxs-lookup"><span data-stu-id="5f09b-178">If the import file includes twin metadata, then this metadata overwrites the existing twin metadata.</span></span> <span data-ttu-id="5f09b-179">Pokud soubor importu neobsahuje twin metadata, pak pouze `lastUpdateTime` metadat je aktualizovat pomocí aktuálního času.</span><span class="sxs-lookup"><span data-stu-id="5f09b-179">If the import file does not include twin metadata, then only the `lastUpdateTime` metadata is updated using the current time.</span></span>

<span data-ttu-id="5f09b-180">Použít nepovinný **režimem importu** vlastnost importovat data serializace pro každé zařízení k řízení import proces za zařízení.</span><span class="sxs-lookup"><span data-stu-id="5f09b-180">Use the optional **importMode** property in the import serialization data for each device to control the import process per-device.</span></span> <span data-ttu-id="5f09b-181">**Režimem importu** vlastnost obsahuje následující možnosti:</span><span class="sxs-lookup"><span data-stu-id="5f09b-181">The **importMode** property has the following options:</span></span>

| <span data-ttu-id="5f09b-182">režimem importu</span><span class="sxs-lookup"><span data-stu-id="5f09b-182">importMode</span></span> | <span data-ttu-id="5f09b-183">Popis</span><span class="sxs-lookup"><span data-stu-id="5f09b-183">Description</span></span> |
| --- | --- |
| <span data-ttu-id="5f09b-184">**createOrUpdate**</span><span class="sxs-lookup"><span data-stu-id="5f09b-184">**createOrUpdate**</span></span> |<span data-ttu-id="5f09b-185">Pokud zařízení se zadaným neexistuje **id**, bude nově zaregistrovaný.</span><span class="sxs-lookup"><span data-stu-id="5f09b-185">If a device does not exist with the specified **id**, it is newly registered.</span></span> <br/><span data-ttu-id="5f09b-186">Pokud už zařízení existuje, dojde k přepsání zadaný vstupní data bez ohledem na stávající informace o **značka ETag** hodnotu.</span><span class="sxs-lookup"><span data-stu-id="5f09b-186">If the device already exists, existing information is overwritten with the provided input data without regard to the **ETag** value.</span></span> <br> <span data-ttu-id="5f09b-187">Uživatel Volitelně můžete zadat data twin spolu s daty zařízení.</span><span class="sxs-lookup"><span data-stu-id="5f09b-187">The user can optionally specify twin data along with the device data.</span></span> <span data-ttu-id="5f09b-188">Značka etag twin,-li zadána, je zpracovat nezávisle ze zařízení etag.</span><span class="sxs-lookup"><span data-stu-id="5f09b-188">The twin’s etag, if specified, is processed independently from the device’s etag.</span></span> <span data-ttu-id="5f09b-189">Pokud se neshodují s existující twin etag, do souboru protokolu se zapíše chyba.</span><span class="sxs-lookup"><span data-stu-id="5f09b-189">If there is a mismatch with the existing twin’s etag, an error is written to the log file.</span></span> |
| <span data-ttu-id="5f09b-190">**vytvoření**</span><span class="sxs-lookup"><span data-stu-id="5f09b-190">**create**</span></span> |<span data-ttu-id="5f09b-191">Pokud zařízení se zadaným neexistuje **id**, bude nově zaregistrovaný.</span><span class="sxs-lookup"><span data-stu-id="5f09b-191">If a device does not exist with the specified **id**, it is newly registered.</span></span> <br/><span data-ttu-id="5f09b-192">Pokud už zařízení existuje, je zapíše chybu do souboru protokolu.</span><span class="sxs-lookup"><span data-stu-id="5f09b-192">If the device already exists, an error is written to the log file.</span></span> <br> <span data-ttu-id="5f09b-193">Uživatel Volitelně můžete zadat data twin spolu s daty zařízení.</span><span class="sxs-lookup"><span data-stu-id="5f09b-193">The user can optionally specify twin data along with the device data.</span></span> <span data-ttu-id="5f09b-194">Značka etag twin,-li zadána, je zpracovat nezávisle ze zařízení etag.</span><span class="sxs-lookup"><span data-stu-id="5f09b-194">The twin’s etag, if specified, is processed independently from the device’s etag.</span></span> <span data-ttu-id="5f09b-195">Pokud se neshodují s existující twin etag, do souboru protokolu se zapíše chyba.</span><span class="sxs-lookup"><span data-stu-id="5f09b-195">If there is a mismatch with the existing twin’s etag, an error is written to the log file.</span></span> |
| <span data-ttu-id="5f09b-196">**aktualizace**</span><span class="sxs-lookup"><span data-stu-id="5f09b-196">**update**</span></span> |<span data-ttu-id="5f09b-197">Pokud zařízení už se zadaným **id**, přepíše stávající informace o zadané vstupní data bez ohledem na **značka ETag** hodnotu.</span><span class="sxs-lookup"><span data-stu-id="5f09b-197">If a device already exists with the specified **id**, existing information is overwritten with the provided input data without regard to the **ETag** value.</span></span> <br/><span data-ttu-id="5f09b-198">Pokud zařízení neexistuje, je zapíše chybu do souboru protokolu.</span><span class="sxs-lookup"><span data-stu-id="5f09b-198">If the device does not exist, an error is written to the log file.</span></span> |
| <span data-ttu-id="5f09b-199">**updateIfMatchETag**</span><span class="sxs-lookup"><span data-stu-id="5f09b-199">**updateIfMatchETag**</span></span> |<span data-ttu-id="5f09b-200">Pokud zařízení už se zadaným **id**, přepíše stávající informace o zadané vstupní data jenom v případě, že je **značka ETag** shodovat.</span><span class="sxs-lookup"><span data-stu-id="5f09b-200">If a device already exists with the specified **id**, existing information is overwritten with the provided input data only if there is an **ETag** match.</span></span> <br/><span data-ttu-id="5f09b-201">Pokud zařízení neexistuje, je zapíše chybu do souboru protokolu.</span><span class="sxs-lookup"><span data-stu-id="5f09b-201">If the device does not exist, an error is written to the log file.</span></span> <br/><span data-ttu-id="5f09b-202">Pokud dojde **značka ETag** neshoda, do souboru protokolu se zapíše chyba.</span><span class="sxs-lookup"><span data-stu-id="5f09b-202">If there is an **ETag** mismatch, an error is written to the log file.</span></span> |
| <span data-ttu-id="5f09b-203">**createOrUpdateIfMatchETag**</span><span class="sxs-lookup"><span data-stu-id="5f09b-203">**createOrUpdateIfMatchETag**</span></span> |<span data-ttu-id="5f09b-204">Pokud zařízení se zadaným neexistuje **id**, bude nově zaregistrovaný.</span><span class="sxs-lookup"><span data-stu-id="5f09b-204">If a device does not exist with the specified **id**, it is newly registered.</span></span> <br/><span data-ttu-id="5f09b-205">Pokud už zařízení existuje, stávající informace o je přepsána zadaný vstupní data jenom v případě, že je **značka ETag** shodovat.</span><span class="sxs-lookup"><span data-stu-id="5f09b-205">If the device already exists, existing information is overwritten with the provided input data only if there is an **ETag** match.</span></span> <br/><span data-ttu-id="5f09b-206">Pokud dojde **značka ETag** neshoda, do souboru protokolu se zapíše chyba.</span><span class="sxs-lookup"><span data-stu-id="5f09b-206">If there is an **ETag** mismatch, an error is written to the log file.</span></span> <br> <span data-ttu-id="5f09b-207">Uživatel Volitelně můžete zadat data twin spolu s daty zařízení.</span><span class="sxs-lookup"><span data-stu-id="5f09b-207">The user can optionally specify twin data along with the device data.</span></span> <span data-ttu-id="5f09b-208">Značka etag twin,-li zadána, je zpracovat nezávisle ze zařízení etag.</span><span class="sxs-lookup"><span data-stu-id="5f09b-208">The twin’s etag, if specified, is processed independently from the device’s etag.</span></span> <span data-ttu-id="5f09b-209">Pokud se neshodují s existující twin etag, do souboru protokolu se zapíše chyba.</span><span class="sxs-lookup"><span data-stu-id="5f09b-209">If there is a mismatch with the existing twin’s etag, an error is written to the log file.</span></span> |
| <span data-ttu-id="5f09b-210">**odstranění**</span><span class="sxs-lookup"><span data-stu-id="5f09b-210">**delete**</span></span> |<span data-ttu-id="5f09b-211">Pokud zařízení už se zadaným **id**, odstraní se bez ohledem na **značka ETag** hodnotu.</span><span class="sxs-lookup"><span data-stu-id="5f09b-211">If a device already exists with the specified **id**, it is deleted without regard to the **ETag** value.</span></span> <br/><span data-ttu-id="5f09b-212">Pokud zařízení neexistuje, je zapíše chybu do souboru protokolu.</span><span class="sxs-lookup"><span data-stu-id="5f09b-212">If the device does not exist, an error is written to the log file.</span></span> |
| <span data-ttu-id="5f09b-213">**deleteIfMatchETag**</span><span class="sxs-lookup"><span data-stu-id="5f09b-213">**deleteIfMatchETag**</span></span> |<span data-ttu-id="5f09b-214">Pokud zařízení už se zadaným **id**, odstraní se pouze v případě, že dojde **značka ETag** shodovat.</span><span class="sxs-lookup"><span data-stu-id="5f09b-214">If a device already exists with the specified **id**, it is deleted only if there is an **ETag** match.</span></span> <span data-ttu-id="5f09b-215">Pokud zařízení neexistuje, je zapíše chybu do souboru protokolu.</span><span class="sxs-lookup"><span data-stu-id="5f09b-215">If the device does not exist, an error is written to the log file.</span></span> <br/><span data-ttu-id="5f09b-216">Pokud dojde neshoda značek ETag, do souboru protokolu se zapíše chyba.</span><span class="sxs-lookup"><span data-stu-id="5f09b-216">If there is an ETag mismatch, an error is written to the log file.</span></span> |

> [!NOTE]
> <span data-ttu-id="5f09b-217">Pokud data serializace nejsou explicitně definovány **režimem importu** příznak pro zařízení, nastaví se jako výchozí **createOrUpdate** během operace importu.</span><span class="sxs-lookup"><span data-stu-id="5f09b-217">If the serialization data does not explicitly define an **importMode** flag for a device, it defaults to **createOrUpdate** during the import operation.</span></span>

## <a name="import-devices-example--bulk-device-provisioning"></a><span data-ttu-id="5f09b-218">Import příklad zařízení – hromadné zřizování zařízení</span><span class="sxs-lookup"><span data-stu-id="5f09b-218">Import devices example – bulk device provisioning</span></span>

<span data-ttu-id="5f09b-219">Následující ukázka kódu C# ukazuje, jak vygenerovat víc identit zařízení který:</span><span class="sxs-lookup"><span data-stu-id="5f09b-219">The following C# code sample illustrates how to generate multiple device identities that:</span></span>

* <span data-ttu-id="5f09b-220">Zahrnout ověřovací klíče.</span><span class="sxs-lookup"><span data-stu-id="5f09b-220">Include authentication keys.</span></span>
* <span data-ttu-id="5f09b-221">Informace o tomto zařízení zapisovat do objektů blob bloku.</span><span class="sxs-lookup"><span data-stu-id="5f09b-221">Write that device information to a block blob.</span></span>
* <span data-ttu-id="5f09b-222">Importujte do registru identit zařízení.</span><span class="sxs-lookup"><span data-stu-id="5f09b-222">Import the devices into the identity registry.</span></span>

```csharp
// Provision 1,000 more devices
var serializedDevices = new List<string>();

for (var i = 0; i < 1000; i++)
{
  // Create a new ExportImportDevice
  // CryptoKeyGenerator is in the Microsoft.Azure.Devices.Common namespace
  var deviceToAdd = new ExportImportDevice()
  {
    Id = Guid.NewGuid().ToString(),
    Status = DeviceStatus.Enabled,
    Authentication = new AuthenticationMechanism()
    {
      SymmetricKey = new SymmetricKey()
      {
        PrimaryKey = CryptoKeyGenerator.GenerateKey(32),
        SecondaryKey = CryptoKeyGenerator.GenerateKey(32)
      }
    },
    ImportMode = ImportMode.Create
  };

  // Add device to the list
  serializedDevices.Add(JsonConvert.SerializeObject(deviceToAdd));
}

// Write the list to the blob
var sb = new StringBuilder();
serializedDevices.ForEach(serializedDevice => sb.AppendLine(serializedDevice));
await blob.DeleteIfExistsAsync();

using (CloudBlobStream stream = await blob.OpenWriteAsync())
{
  byte[] bytes = Encoding.UTF8.GetBytes(sb.ToString());
  for (var i = 0; i < bytes.Length; i += 500)
  {
    int length = Math.Min(bytes.Length - i, 500);
    await stream.WriteAsync(bytes, i, length);
  }
}

// Call import using the blob to add new devices
// Log information related to the job is written to the same container
// This normally takes 1 minute per 100 devices
JobProperties importJob = await registryManager.ImportDevicesAsync(containerSasUri, containerSasUri);

// Wait until job is finished
while(true)
{
  importJob = await registryManager.GetJobAsync(importJob.JobId);
  if (importJob.Status == JobStatus.Completed || 
      importJob.Status == JobStatus.Failed ||
      importJob.Status == JobStatus.Cancelled)
  {
    // Job has finished executing
    break;
  }

  await Task.Delay(TimeSpan.FromSeconds(5));
}
```

## <a name="import-devices-example--bulk-deletion"></a><span data-ttu-id="5f09b-223">Import zařízení příklad – hromadné odstranění</span><span class="sxs-lookup"><span data-stu-id="5f09b-223">Import devices example – bulk deletion</span></span>

<span data-ttu-id="5f09b-224">Následující příklad kódu ukazuje, jak odstranit zařízení, které jste přidali v předchozím příkladu kódu pomocí:</span><span class="sxs-lookup"><span data-stu-id="5f09b-224">The following code sample shows you how to delete the devices you added using the previous code sample:</span></span>

```csharp
// Step 1: Update each device's ImportMode to be Delete
sb = new StringBuilder();
serializedDevices.ForEach(serializedDevice =>
{
  // Deserialize back to an ExportImportDevice
  var device = JsonConvert.DeserializeObject<ExportImportDevice>(serializedDevice);

  // Update property
  device.ImportMode = ImportMode.Delete;

  // Re-serialize
  sb.AppendLine(JsonConvert.SerializeObject(device));
});

// Step 2: Write the new import data back to the block blob
await blob.DeleteIfExistsAsync();
using (CloudBlobStream stream = await blob.OpenWriteAsync())
{
  byte[] bytes = Encoding.UTF8.GetBytes(sb.ToString());
  for (var i = 0; i < bytes.Length; i += 500)
  {
    int length = Math.Min(bytes.Length - i, 500);
    await stream.WriteAsync(bytes, i, length);
  }
}

// Step 3: Call import using the same blob to delete all devices
importJob = await registryManager.ImportDevicesAsync(containerSasUri, containerSasUri);

// Wait until job is finished
while(true)
{
  importJob = await registryManager.GetJobAsync(importJob.JobId);
  if (importJob.Status == JobStatus.Completed || 
      importJob.Status == JobStatus.Failed ||
      importJob.Status == JobStatus.Cancelled)
  {
    // Job has finished executing
    break;
  }

  await Task.Delay(TimeSpan.FromSeconds(5));
}
```

## <a name="get-the-container-sas-uri"></a><span data-ttu-id="5f09b-225">Získat identifikátor URI pro SAS kontejneru</span><span class="sxs-lookup"><span data-stu-id="5f09b-225">Get the container SAS URI</span></span>

<span data-ttu-id="5f09b-226">Následující příklad kódu ukazuje, jak vygenerovat [identifikátor URI pro SAS](../storage/blobs/storage-dotnet-shared-access-signature-part-2.md) čtení, zápisu a odstranění oprávnění pro kontejner objektů blob:</span><span class="sxs-lookup"><span data-stu-id="5f09b-226">The following code sample shows you how to generate a [SAS URI](../storage/blobs/storage-dotnet-shared-access-signature-part-2.md) with read, write, and delete permissions for a blob container:</span></span>

```csharp
static string GetContainerSasUri(CloudBlobContainer container)
{
  // Set the expiry time and permissions for the container.
  // In this case no start time is specified, so the
  // shared access signature becomes valid immediately.
  var sasConstraints = new SharedAccessBlobPolicy();
  sasConstraints.SharedAccessExpiryTime = DateTime.UtcNow.AddHours(24);
  sasConstraints.Permissions = 
    SharedAccessBlobPermissions.Write | 
    SharedAccessBlobPermissions.Read | 
    SharedAccessBlobPermissions.Delete;

  // Generate the shared access signature on the container,
  // setting the constraints directly on the signature.
  string sasContainerToken = container.GetSharedAccessSignature(sasConstraints);

  // Return the URI string for the container,
  // including the SAS token.
  return container.Uri + sasContainerToken;
}
```

## <a name="next-steps"></a><span data-ttu-id="5f09b-227">Další kroky</span><span class="sxs-lookup"><span data-stu-id="5f09b-227">Next steps</span></span>

<span data-ttu-id="5f09b-228">V tomto článku jste zjistili, jak provést hromadné operace proti registru identit služby IoT hub.</span><span class="sxs-lookup"><span data-stu-id="5f09b-228">In this article, you learned how to perform bulk operations against the identity registry in an IoT hub.</span></span> <span data-ttu-id="5f09b-229">Další informace o správě Azure IoT Hub na následujících odkazech:</span><span class="sxs-lookup"><span data-stu-id="5f09b-229">Follow these links to learn more about managing Azure IoT Hub:</span></span>

* <span data-ttu-id="5f09b-230">[Metriky služby IoT Hub][lnk-metrics]</span><span class="sxs-lookup"><span data-stu-id="5f09b-230">[IoT Hub metrics][lnk-metrics]</span></span>
* <span data-ttu-id="5f09b-231">[Monitorování operací][lnk-monitor]</span><span class="sxs-lookup"><span data-stu-id="5f09b-231">[Operations monitoring][lnk-monitor]</span></span>

<span data-ttu-id="5f09b-232">Pokud chcete prozkoumat další možnosti IoT Hub, najdete v části:</span><span class="sxs-lookup"><span data-stu-id="5f09b-232">To further explore the capabilities of IoT Hub, see:</span></span>

* <span data-ttu-id="5f09b-233">[Příručka vývojáře pro službu IoT Hub][lnk-devguide]</span><span class="sxs-lookup"><span data-stu-id="5f09b-233">[IoT Hub developer guide][lnk-devguide]</span></span>
* <span data-ttu-id="5f09b-234">[Simulaci zařízení s hranou IoT][lnk-iotedge]</span><span class="sxs-lookup"><span data-stu-id="5f09b-234">[Simulating a device with IoT Edge][lnk-iotedge]</span></span>

[lnk-metrics]: iot-hub-metrics.md
[lnk-monitor]: iot-hub-operations-monitoring.md

[lnk-devguide]: iot-hub-devguide.md
[lnk-iotedge]: iot-hub-linux-iot-edge-simulated-device.md
