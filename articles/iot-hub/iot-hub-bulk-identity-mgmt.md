---
title: "export aaaImport identit zařízení Azure IoT Hub | Microsoft Docs"
description: "Jak toouse hello Azure IoT služby SDK tooperform hromadné operace u hello identity registru tooimport a exportovat identit zařízení. Operace importu umožňují toocreate, aktualizace a odstranění zařízení identity hromadně."
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
ms.openlocfilehash: b67777d381e03de05d9c007b5ce6bdaf15bbb8f0
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="manage-your-iot-hub-device-identities-in-bulk"></a><span data-ttu-id="53852-104">Spravovat vaše identit zařízení IoT Hub hromadně</span><span class="sxs-lookup"><span data-stu-id="53852-104">Manage your IoT Hub device identities in bulk</span></span>

<span data-ttu-id="53852-105">Každé centrum IoT má registru identit toocreate prostředků na zařízení můžete použít ve službě hello.</span><span class="sxs-lookup"><span data-stu-id="53852-105">Each IoT hub has an identity registry you can use toocreate per-device resources in hello service.</span></span> <span data-ttu-id="53852-106">registr identit Hello můžete také toocontrol přístup toohello zařízení přístupem koncových bodů.</span><span class="sxs-lookup"><span data-stu-id="53852-106">hello identity registry also enables you toocontrol access toohello device-facing endpoints.</span></span> <span data-ttu-id="53852-107">Tento článek popisuje, jak tooimport a export identit zařízení v hromadné tooand z registru identit.</span><span class="sxs-lookup"><span data-stu-id="53852-107">This article describes how tooimport and export device identities in bulk tooand from an identity registry.</span></span>

<span data-ttu-id="53852-108">Import a export operace proběhla v kontextu hello *úlohy* , umožňují tooexecute hromadné operace služby proti služby IoT hub.</span><span class="sxs-lookup"><span data-stu-id="53852-108">Import and export operations take place in hello context of *Jobs* that enable you tooexecute bulk service operations against an IoT hub.</span></span>

<span data-ttu-id="53852-109">Hello **RegistryManager** třída zahrnuje hello **ExportDevicesAsync** a **ImportDevicesAsync** metody, které používají hello **úlohy** Framework.</span><span class="sxs-lookup"><span data-stu-id="53852-109">hello **RegistryManager** class includes hello **ExportDevicesAsync** and **ImportDevicesAsync** methods that use hello **Job** framework.</span></span> <span data-ttu-id="53852-110">Tyto metody umožňují tooexport, importu a synchronizaci hello celého registru identit IoT hub.</span><span class="sxs-lookup"><span data-stu-id="53852-110">These methods enable you tooexport, import, and synchronize hello entirety of an IoT hub identity registry.</span></span>

## <a name="what-are-jobs"></a><span data-ttu-id="53852-111">Jaké jsou úlohy?</span><span class="sxs-lookup"><span data-stu-id="53852-111">What are jobs?</span></span>

<span data-ttu-id="53852-112">Operace s registrem identit pomocí hello **úlohy** systému při hello operace:</span><span class="sxs-lookup"><span data-stu-id="53852-112">Identity registry operations use hello **Job** system when hello operation:</span></span>

* <span data-ttu-id="53852-113">Má potenciálně dlouhou dobu spuštění porovná toostandard spuštění operace.</span><span class="sxs-lookup"><span data-stu-id="53852-113">Has a potentially long execution time compared toostandard run-time operations.</span></span>
* <span data-ttu-id="53852-114">Vrací velké množství dat toohello uživatele.</span><span class="sxs-lookup"><span data-stu-id="53852-114">Returns a large amount of data toohello user.</span></span>

<span data-ttu-id="53852-115">Místo jednoho volání rozhraní API čekání nebo blokování na hello výsledek operace hello, operace hello asynchronně vytvoří **úlohy** tohoto centra IoT.</span><span class="sxs-lookup"><span data-stu-id="53852-115">Instead of a single API call waiting or blocking on hello result of hello operation, hello operation asynchronously creates a **Job** for that IoT hub.</span></span> <span data-ttu-id="53852-116">operace Hello pak okamžitě vrátí **JobProperties** objektu.</span><span class="sxs-lookup"><span data-stu-id="53852-116">hello operation then immediately returns a **JobProperties** object.</span></span>

<span data-ttu-id="53852-117">Hello následující C# code fragment kódu ukazuje, jak toocreate úlohy exportu:</span><span class="sxs-lookup"><span data-stu-id="53852-117">hello following C# code snippet shows how toocreate an export job:</span></span>

```csharp
// Call an export job on hello IoT Hub tooretrieve all devices
JobProperties exportJob = await registryManager.ExportDevicesAsync(containerSasUri, false);
```

> [!NOTE]
> <span data-ttu-id="53852-118">toouse hello **RegistryManager** třídy v kódu jazyka C#, přidejte hello **Microsoft.Azure.Devices** projektu tooyour balíček NuGet.</span><span class="sxs-lookup"><span data-stu-id="53852-118">toouse hello **RegistryManager** class in your C# code, add hello **Microsoft.Azure.Devices** NuGet package tooyour project.</span></span> <span data-ttu-id="53852-119">Hello **RegistryManager** třída je v hello **Microsoft.Azure.Devices** oboru názvů.</span><span class="sxs-lookup"><span data-stu-id="53852-119">hello **RegistryManager** class is in hello **Microsoft.Azure.Devices** namespace.</span></span>

<span data-ttu-id="53852-120">Můžete použít hello **RegistryManager** třídy tooquery hello stav hello **úlohy** pomocí hello vrátil **JobProperties** metadat.</span><span class="sxs-lookup"><span data-stu-id="53852-120">You can use hello **RegistryManager** class tooquery hello state of hello **Job** using hello returned **JobProperties** metadata.</span></span>

<span data-ttu-id="53852-121">Hello následující fragment kódu jazyka C# ukazuje, jak toopoll toosee každých pět sekund, pokud hello úlohy skončil:</span><span class="sxs-lookup"><span data-stu-id="53852-121">hello following C# code snippet shows how toopoll every five seconds toosee if hello job has finished executing:</span></span>

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

## <a name="export-devices"></a><span data-ttu-id="53852-122">Export zařízení</span><span class="sxs-lookup"><span data-stu-id="53852-122">Export devices</span></span>

<span data-ttu-id="53852-123">Použití hello **ExportDevicesAsync** metoda tooexport hello celého IoT hub identity registru tooan [Azure Storage](../storage/index.md) pomocí kontejneru objektů blob [sdíleného přístupového podpisu](../storage/common/storage-security-guide.md#data-plane-security).</span><span class="sxs-lookup"><span data-stu-id="53852-123">Use hello **ExportDevicesAsync** method tooexport hello entirety of an IoT hub identity registry tooan [Azure Storage](../storage/index.md) blob container using a [Shared Access Signature](../storage/common/storage-security-guide.md#data-plane-security).</span></span>

<span data-ttu-id="53852-124">Tato metoda umožňuje toocreate spolehlivé zálohování vašich zařízení informací v kontejneru objektů blob, který ovládáte.</span><span class="sxs-lookup"><span data-stu-id="53852-124">This method enables you toocreate reliable backups of your device information in a blob container that you control.</span></span>

<span data-ttu-id="53852-125">Hello **ExportDevicesAsync** metoda vyžaduje dva parametry:</span><span class="sxs-lookup"><span data-stu-id="53852-125">hello **ExportDevicesAsync** method requires two parameters:</span></span>

* <span data-ttu-id="53852-126">A *řetězec* obsahující identifikátor URI kontejner objektů blob.</span><span class="sxs-lookup"><span data-stu-id="53852-126">A *string* that contains a URI of a blob container.</span></span> <span data-ttu-id="53852-127">Tento identifikátor URI musí obsahovat token SAS, která uděluje přístup pro zápis toohello kontejneru.</span><span class="sxs-lookup"><span data-stu-id="53852-127">This URI must contain a SAS token that grants write access toohello container.</span></span> <span data-ttu-id="53852-128">Úloha Hello vytvoří objekt blob bloku data tento kontejner toostore hello serializovat export zařízení.</span><span class="sxs-lookup"><span data-stu-id="53852-128">hello job creates a block blob in this container toostore hello serialized export device data.</span></span> <span data-ttu-id="53852-129">Hello SAS token musí zahrnovat tato oprávnění:</span><span class="sxs-lookup"><span data-stu-id="53852-129">hello SAS token must include these permissions:</span></span>

   ```csharp
   SharedAccessBlobPermissions.Write | SharedAccessBlobPermissions.Read | SharedAccessBlobPermissions.Delete
   ```

* <span data-ttu-id="53852-130">A *boolean* určující, pokud chcete exportovat data tooexclude ověřovací klíče.</span><span class="sxs-lookup"><span data-stu-id="53852-130">A *boolean* that indicates if you want tooexclude authentication keys from your export data.</span></span> <span data-ttu-id="53852-131">Pokud **false**, ověřovací klíče jsou zahrnuty ve výstupu export.</span><span class="sxs-lookup"><span data-stu-id="53852-131">If **false**, authentication keys are included in export output.</span></span> <span data-ttu-id="53852-132">Jinak, jsou klíče exportovány jako **null**.</span><span class="sxs-lookup"><span data-stu-id="53852-132">Otherwise, keys are exported as **null**.</span></span>

<span data-ttu-id="53852-133">Hello následující fragment kódu jazyka C# popisuje, jak tooinitiate úlohu export, který obsahuje klíče ověřování zařízení v hello exportovat data a pak dotazování na dokončení:</span><span class="sxs-lookup"><span data-stu-id="53852-133">hello following C# code snippet shows how tooinitiate an export job that includes device authentication keys in hello export data and then poll for completion:</span></span>

```csharp
// Call an export job on hello IoT Hub tooretrieve all devices
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

<span data-ttu-id="53852-134">Hello úlohy ukládá její výstup v kontejneru objektů blob hello zadané jako objekt blob bloku s názvem hello **devices.txt**.</span><span class="sxs-lookup"><span data-stu-id="53852-134">hello job stores its output in hello provided blob container as a block blob with hello name **devices.txt**.</span></span> <span data-ttu-id="53852-135">Hello výstupní data se skládá z data zařízení serializován do formátu JSON, s jedno zařízení na každý řádek.</span><span class="sxs-lookup"><span data-stu-id="53852-135">hello output data consists of JSON serialized device data, with one device per line.</span></span>

<span data-ttu-id="53852-136">Hello následující příklad ukazuje hello výstupní data:</span><span class="sxs-lookup"><span data-stu-id="53852-136">hello following example shows hello output data:</span></span>

```json
{"id":"Device1","eTag":"MA==","status":"enabled","authentication":{"symmetricKey":{"primaryKey":"abc=","secondaryKey":"def="}}}
{"id":"Device2","eTag":"MA==","status":"enabled","authentication":{"symmetricKey":{"primaryKey":"abc=","secondaryKey":"def="}}}
{"id":"Device3","eTag":"MA==","status":"disabled","authentication":{"symmetricKey":{"primaryKey":"abc=","secondaryKey":"def="}}}
{"id":"Device4","eTag":"MA==","status":"disabled","authentication":{"symmetricKey":{"primaryKey":"abc=","secondaryKey":"def="}}}
{"id":"Device5","eTag":"MA==","status":"enabled","authentication":{"symmetricKey":{"primaryKey":"abc=","secondaryKey":"def="}}}
```

Pokud zařízení obsahuje twin dat, hello twin data také exportovat společně s hello data zařízení. Hello následující příklad ukazuje tento formát. <span data-ttu-id="53852-139">Všechna data z řádku "twinETag" hello dokud hello end jsou twin data.</span><span class="sxs-lookup"><span data-stu-id="53852-139">All data from hello "twinETag" line until hello end are twin data.</span></span>

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

<span data-ttu-id="53852-140">Pokud budete potřebovat přístup k datům toothis v kódu, můžete snadno rekonstruovat tato data pomocí hello **ExportImportDevice** třídy.</span><span class="sxs-lookup"><span data-stu-id="53852-140">If you need access toothis data in code, you can easily deserialize this data using hello **ExportImportDevice** class.</span></span> <span data-ttu-id="53852-141">Hello následující fragment kódu jazyka C# ukazuje, jak tooread informace o zařízení, která byla předtím exportovali objekt blob bloku tooa:</span><span class="sxs-lookup"><span data-stu-id="53852-141">hello following C# code snippet shows how tooread device information that was previously exported tooa block blob:</span></span>

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
> <span data-ttu-id="53852-142">Můžete taky hello **GetDevicesAsync** metoda hello **RegistryManager** třída toofetch seznam zařízení.</span><span class="sxs-lookup"><span data-stu-id="53852-142">You can also use hello **GetDevicesAsync** method of hello **RegistryManager** class toofetch a list of your devices.</span></span> <span data-ttu-id="53852-143">Však tento přístup má pevný cap 1000 hello počtu zařízení objekty, které jsou vráceny.</span><span class="sxs-lookup"><span data-stu-id="53852-143">However, this approach has a hard cap of 1000 on hello number of device objects that are returned.</span></span> <span data-ttu-id="53852-144">Hello očekává případ použití pro hello **GetDevicesAsync** metoda je pro vývoj scénáře tooaid ladění a se nedoporučuje pro produkční zatížení.</span><span class="sxs-lookup"><span data-stu-id="53852-144">hello expected use case for hello **GetDevicesAsync** method is for development scenarios tooaid debugging and is not recommended for production workloads.</span></span>

## <a name="import-devices"></a><span data-ttu-id="53852-145">Import zařízení</span><span class="sxs-lookup"><span data-stu-id="53852-145">Import devices</span></span>

<span data-ttu-id="53852-146">Hello **ImportDevicesAsync** metoda v hello **RegistryManager** třída umožňuje operace importu a synchronizaci hromadné tooperform v registru identit IoT hub.</span><span class="sxs-lookup"><span data-stu-id="53852-146">hello **ImportDevicesAsync** method in hello **RegistryManager** class enables you tooperform bulk import and synchronization operations in an IoT hub identity registry.</span></span> <span data-ttu-id="53852-147">Jako hello **ExportDevicesAsync** metoda, hello **ImportDevicesAsync** metoda používá hello **úlohy** framework.</span><span class="sxs-lookup"><span data-stu-id="53852-147">Like hello **ExportDevicesAsync** method, hello **ImportDevicesAsync** method uses hello **Job** framework.</span></span>

<span data-ttu-id="53852-148">Vezměte v potaz pomocí hello **ImportDevicesAsync** metoda vzhledem k tomu, že v tooprovisioning nová zařízení přidání v registru identit, můžete také aktualizovat a odstranit existující zařízení.</span><span class="sxs-lookup"><span data-stu-id="53852-148">Take care using hello **ImportDevicesAsync** method because in addition tooprovisioning new devices in your identity registry, it can also update and delete existing devices.</span></span>

> [!WARNING]
> <span data-ttu-id="53852-149">Operace importu nelze vrátit zpět.</span><span class="sxs-lookup"><span data-stu-id="53852-149">An import operation cannot be undone.</span></span> <span data-ttu-id="53852-150">Vždy zálohovat stávající data pomocí hello **ExportDevicesAsync** kontejner objektů blob tooanother metoda před provedením hromadné změny registru identit tooyour.</span><span class="sxs-lookup"><span data-stu-id="53852-150">Always back up your existing data using hello **ExportDevicesAsync** method tooanother blob container before you make bulk changes tooyour identity registry.</span></span>

<span data-ttu-id="53852-151">Hello **ImportDevicesAsync** metoda přebírá dva parametry:</span><span class="sxs-lookup"><span data-stu-id="53852-151">hello **ImportDevicesAsync** method takes two parameters:</span></span>

* <span data-ttu-id="53852-152">A *řetězec* identifikátor URI, který obsahuje [Azure Storage](../storage/index.md) blob kontejneru toouse jako *vstupní* toohello úlohy.</span><span class="sxs-lookup"><span data-stu-id="53852-152">A *string* that contains a URI of an [Azure Storage](../storage/index.md) blob container toouse as *input* toohello job.</span></span> <span data-ttu-id="53852-153">Tento identifikátor URI musí obsahovat token SAS, která uděluje přístup pro čtení toohello kontejneru.</span><span class="sxs-lookup"><span data-stu-id="53852-153">This URI must contain a SAS token that grants read access toohello container.</span></span> <span data-ttu-id="53852-154">Tento kontejner musí obsahovat objekt blob s názvem hello **devices.txt** obsahující hello serializovat zařízení data tooimport do registru identit.</span><span class="sxs-lookup"><span data-stu-id="53852-154">This container must contain a blob with hello name **devices.txt** that contains hello serialized device data tooimport into your identity registry.</span></span> <span data-ttu-id="53852-155">Hello import dat musí obsahovat informace o zařízení v hello stejné JSON formátu této hello **ExportImportDevice** úloha používá při vytváření **devices.txt** objektů blob.</span><span class="sxs-lookup"><span data-stu-id="53852-155">hello import data must contain device information in hello same JSON format that hello **ExportImportDevice** job uses when it creates a **devices.txt** blob.</span></span> <span data-ttu-id="53852-156">Hello SAS token musí zahrnovat tato oprávnění:</span><span class="sxs-lookup"><span data-stu-id="53852-156">hello SAS token must include these permissions:</span></span>

   ```csharp
   SharedAccessBlobPermissions.Read
   ```
* <span data-ttu-id="53852-157">A *řetězec* identifikátor URI, který obsahuje [Azure Storage](https://azure.microsoft.com/documentation/services/storage/) blob kontejneru toouse jako *výstup* z úlohy hello.</span><span class="sxs-lookup"><span data-stu-id="53852-157">A *string* that contains a URI of an [Azure Storage](https://azure.microsoft.com/documentation/services/storage/) blob container toouse as *output* from hello job.</span></span> <span data-ttu-id="53852-158">Hello úloha vytvoří objekt blob bloku v tento kontejner toostore veškeré informace o chybě z hello dokončení importu **úlohy**.</span><span class="sxs-lookup"><span data-stu-id="53852-158">hello job creates a block blob in this container toostore any error information from hello completed import **Job**.</span></span> <span data-ttu-id="53852-159">Hello SAS token musí zahrnovat tato oprávnění:</span><span class="sxs-lookup"><span data-stu-id="53852-159">hello SAS token must include these permissions:</span></span>

   ```csharp
   SharedAccessBlobPermissions.Write | SharedAccessBlobPermissions.Read | SharedAccessBlobPermissions.Delete
   ```

> [!NOTE]
> <span data-ttu-id="53852-160">Hello dva parametry může ukazovat toohello stejný kontejner objektů blob.</span><span class="sxs-lookup"><span data-stu-id="53852-160">hello two parameters can point toohello same blob container.</span></span> <span data-ttu-id="53852-161">samostatné parametry Hello jednoduše povolte větší kontrolu nad daty jako kontejner výstup hello vyžaduje další oprávnění.</span><span class="sxs-lookup"><span data-stu-id="53852-161">hello separate parameters simply enable more control over your data as hello output container requires additional permissions.</span></span>

<span data-ttu-id="53852-162">Hello následující C# code fragment kódu ukazuje, jak tooinitiate úlohy importu:</span><span class="sxs-lookup"><span data-stu-id="53852-162">hello following C# code snippet shows how tooinitiate an import job:</span></span>

```csharp
JobProperties importJob = await registryManager.ImportDevicesAsync(containerSasUri, containerSasUri);
```

<span data-ttu-id="53852-163">Tato metoda může být také použít tooimport hello data pro dvojče zařízení hello.</span><span class="sxs-lookup"><span data-stu-id="53852-163">This method can also be used tooimport hello data for hello device twin.</span></span> <span data-ttu-id="53852-164">Hello formát pro hello datového vstupu je hello stejný jako formát hello ukazuje hello **ExportDevicesAsync** části.</span><span class="sxs-lookup"><span data-stu-id="53852-164">hello format for hello data input is hello same as hello format shown in hello **ExportDevicesAsync** section.</span></span> <span data-ttu-id="53852-165">Tímto způsobem můžete znovu naimportovat data hello exportovali.</span><span class="sxs-lookup"><span data-stu-id="53852-165">In this way, you can reimport hello exported data.</span></span> <span data-ttu-id="53852-166">Hello **$metadata** je volitelný.</span><span class="sxs-lookup"><span data-stu-id="53852-166">hello **$metadata** is optional.</span></span>

## <a name="import-behavior"></a><span data-ttu-id="53852-167">Import chování</span><span class="sxs-lookup"><span data-stu-id="53852-167">Import behavior</span></span>

<span data-ttu-id="53852-168">Můžete použít hello **ImportDevicesAsync** metoda tooperform hello následující hromadné operace v registru identit:</span><span class="sxs-lookup"><span data-stu-id="53852-168">You can use hello **ImportDevicesAsync** method tooperform hello following bulk operations in your identity registry:</span></span>

* <span data-ttu-id="53852-169">Hromadné registrace nové zařízení</span><span class="sxs-lookup"><span data-stu-id="53852-169">Bulk registration of new devices</span></span>
* <span data-ttu-id="53852-170">Hromadné odstranění stávajících zařízení</span><span class="sxs-lookup"><span data-stu-id="53852-170">Bulk deletions of existing devices</span></span>
* <span data-ttu-id="53852-171">Hromadné změny stavu (povolit nebo zakázat zařízení)</span><span class="sxs-lookup"><span data-stu-id="53852-171">Bulk status changes (enable or disable devices)</span></span>
* <span data-ttu-id="53852-172">Hromadné přiřazení nových klíčů ověřování zařízení</span><span class="sxs-lookup"><span data-stu-id="53852-172">Bulk assignment of new device authentication keys</span></span>
* <span data-ttu-id="53852-173">Hromadné auto-nové vygenerování klíče pro ověřování zařízení</span><span class="sxs-lookup"><span data-stu-id="53852-173">Bulk auto-regeneration of device authentication keys</span></span>
* <span data-ttu-id="53852-174">Hromadné aktualizace dat twin</span><span class="sxs-lookup"><span data-stu-id="53852-174">Bulk update of twin data</span></span>

<span data-ttu-id="53852-175">Můžete nastavit libovolnou kombinaci hello předcházející operace v rámci jednoho **ImportDevicesAsync** volání.</span><span class="sxs-lookup"><span data-stu-id="53852-175">You can perform any combination of hello preceding operations within a single **ImportDevicesAsync** call.</span></span> <span data-ttu-id="53852-176">Například můžete zaregistrovat nové zařízení a odstranění nebo aktualizaci stávajících zařízení v hello stejnou dobu.</span><span class="sxs-lookup"><span data-stu-id="53852-176">For example, you can register new devices and delete or update existing devices at hello same time.</span></span> <span data-ttu-id="53852-177">Při použití spolu s hello **ExportDevicesAsync** metody úplně můžete migrovat všechna vaše zařízení z jedné tooanother centra IoT.</span><span class="sxs-lookup"><span data-stu-id="53852-177">When used along with hello **ExportDevicesAsync** method, you can completely migrate all your devices from one IoT hub tooanother.</span></span>

<span data-ttu-id="53852-178">Obsahuje-li soubor importu hello twin metadata, tato metadata přepíše existující metadata twin hello.</span><span class="sxs-lookup"><span data-stu-id="53852-178">If hello import file includes twin metadata, then this metadata overwrites hello existing twin metadata.</span></span> <span data-ttu-id="53852-179">Pokud soubor importu hello neobsahuje twin metadata, pak pouze hello `lastUpdateTime` metadat je aktualizovat pomocí hello aktuální čas.</span><span class="sxs-lookup"><span data-stu-id="53852-179">If hello import file does not include twin metadata, then only hello `lastUpdateTime` metadata is updated using hello current time.</span></span>

<span data-ttu-id="53852-180">Volitelné použití hello **režimem importu** vlastnost hello import serializace dat pro každé zařízení toocontrol hello import proces podle zařízení.</span><span class="sxs-lookup"><span data-stu-id="53852-180">Use hello optional **importMode** property in hello import serialization data for each device toocontrol hello import process per-device.</span></span> <span data-ttu-id="53852-181">Hello **režimem importu** vlastnost má hello následující možnosti:</span><span class="sxs-lookup"><span data-stu-id="53852-181">hello **importMode** property has hello following options:</span></span>

| <span data-ttu-id="53852-182">režimem importu</span><span class="sxs-lookup"><span data-stu-id="53852-182">importMode</span></span> | <span data-ttu-id="53852-183">Popis</span><span class="sxs-lookup"><span data-stu-id="53852-183">Description</span></span> |
| --- | --- |
| <span data-ttu-id="53852-184">**createOrUpdate**</span><span class="sxs-lookup"><span data-stu-id="53852-184">**createOrUpdate**</span></span> |<span data-ttu-id="53852-185">Pokud zařízení s hello zadaný neexistuje **id**, bude nově zaregistrovaný.</span><span class="sxs-lookup"><span data-stu-id="53852-185">If a device does not exist with hello specified **id**, it is newly registered.</span></span> <br/><span data-ttu-id="53852-186">Pokud zařízení hello již existuje, dojde k přepsání hello poskytl vstupní data bez ohledem toohello stávající informace o **značka ETag** hodnotu.</span><span class="sxs-lookup"><span data-stu-id="53852-186">If hello device already exists, existing information is overwritten with hello provided input data without regard toohello **ETag** value.</span></span> <br> <span data-ttu-id="53852-187">uživatel Hello Volitelně můžete zadat data twin spolu s daty hello zařízení.</span><span class="sxs-lookup"><span data-stu-id="53852-187">hello user can optionally specify twin data along with hello device data.</span></span> <span data-ttu-id="53852-188">značka etag Hello twin,-li zadána, je zpracovat nezávisle z hello zařízení etag.</span><span class="sxs-lookup"><span data-stu-id="53852-188">hello twin’s etag, if specified, is processed independently from hello device’s etag.</span></span> <span data-ttu-id="53852-189">Pokud se neshodují s hello existující twin etag, se zapíše chyba toohello souboru protokolu.</span><span class="sxs-lookup"><span data-stu-id="53852-189">If there is a mismatch with hello existing twin’s etag, an error is written toohello log file.</span></span> |
| <span data-ttu-id="53852-190">**vytvoření**</span><span class="sxs-lookup"><span data-stu-id="53852-190">**create**</span></span> |<span data-ttu-id="53852-191">Pokud zařízení s hello zadaný neexistuje **id**, bude nově zaregistrovaný.</span><span class="sxs-lookup"><span data-stu-id="53852-191">If a device does not exist with hello specified **id**, it is newly registered.</span></span> <br/><span data-ttu-id="53852-192">Pokud zařízení hello již existuje, se zapíše chyba toohello souboru protokolu.</span><span class="sxs-lookup"><span data-stu-id="53852-192">If hello device already exists, an error is written toohello log file.</span></span> <br> <span data-ttu-id="53852-193">uživatel Hello Volitelně můžete zadat data twin spolu s daty hello zařízení.</span><span class="sxs-lookup"><span data-stu-id="53852-193">hello user can optionally specify twin data along with hello device data.</span></span> <span data-ttu-id="53852-194">značka etag Hello twin,-li zadána, je zpracovat nezávisle z hello zařízení etag.</span><span class="sxs-lookup"><span data-stu-id="53852-194">hello twin’s etag, if specified, is processed independently from hello device’s etag.</span></span> <span data-ttu-id="53852-195">Pokud se neshodují s hello existující twin etag, se zapíše chyba toohello souboru protokolu.</span><span class="sxs-lookup"><span data-stu-id="53852-195">If there is a mismatch with hello existing twin’s etag, an error is written toohello log file.</span></span> |
| <span data-ttu-id="53852-196">**aktualizace**</span><span class="sxs-lookup"><span data-stu-id="53852-196">**update**</span></span> |<span data-ttu-id="53852-197">Pokud zařízení už existuje s hello zadaný **id**, přepíše stávající informace o hello poskytl vstupní data bez ohledem toohello **značka ETag** hodnotu.</span><span class="sxs-lookup"><span data-stu-id="53852-197">If a device already exists with hello specified **id**, existing information is overwritten with hello provided input data without regard toohello **ETag** value.</span></span> <br/><span data-ttu-id="53852-198">Pokud zařízení hello neexistuje, se zapíše chyba toohello souboru protokolu.</span><span class="sxs-lookup"><span data-stu-id="53852-198">If hello device does not exist, an error is written toohello log file.</span></span> |
| <span data-ttu-id="53852-199">**updateIfMatchETag**</span><span class="sxs-lookup"><span data-stu-id="53852-199">**updateIfMatchETag**</span></span> |<span data-ttu-id="53852-200">Pokud zařízení už existuje s hello zadaný **id**, přepíše stávající informace o hello poskytl vstupní data jenom v případě, že je **značka ETag** shodovat.</span><span class="sxs-lookup"><span data-stu-id="53852-200">If a device already exists with hello specified **id**, existing information is overwritten with hello provided input data only if there is an **ETag** match.</span></span> <br/><span data-ttu-id="53852-201">Pokud zařízení hello neexistuje, se zapíše chyba toohello souboru protokolu.</span><span class="sxs-lookup"><span data-stu-id="53852-201">If hello device does not exist, an error is written toohello log file.</span></span> <br/><span data-ttu-id="53852-202">Pokud dojde **značka ETag** neshoda, se zapíše chyba toohello souboru protokolu.</span><span class="sxs-lookup"><span data-stu-id="53852-202">If there is an **ETag** mismatch, an error is written toohello log file.</span></span> |
| <span data-ttu-id="53852-203">**createOrUpdateIfMatchETag**</span><span class="sxs-lookup"><span data-stu-id="53852-203">**createOrUpdateIfMatchETag**</span></span> |<span data-ttu-id="53852-204">Pokud zařízení s hello zadaný neexistuje **id**, bude nově zaregistrovaný.</span><span class="sxs-lookup"><span data-stu-id="53852-204">If a device does not exist with hello specified **id**, it is newly registered.</span></span> <br/><span data-ttu-id="53852-205">Pokud zařízení hello již existuje, stávající informace o přepsána hello poskytl vstupní data jenom v případě, že je **značka ETag** shodovat.</span><span class="sxs-lookup"><span data-stu-id="53852-205">If hello device already exists, existing information is overwritten with hello provided input data only if there is an **ETag** match.</span></span> <br/><span data-ttu-id="53852-206">Pokud dojde **značka ETag** neshoda, se zapíše chyba toohello souboru protokolu.</span><span class="sxs-lookup"><span data-stu-id="53852-206">If there is an **ETag** mismatch, an error is written toohello log file.</span></span> <br> <span data-ttu-id="53852-207">uživatel Hello Volitelně můžete zadat data twin spolu s daty hello zařízení.</span><span class="sxs-lookup"><span data-stu-id="53852-207">hello user can optionally specify twin data along with hello device data.</span></span> <span data-ttu-id="53852-208">značka etag Hello twin,-li zadána, je zpracovat nezávisle z hello zařízení etag.</span><span class="sxs-lookup"><span data-stu-id="53852-208">hello twin’s etag, if specified, is processed independently from hello device’s etag.</span></span> <span data-ttu-id="53852-209">Pokud se neshodují s hello existující twin etag, se zapíše chyba toohello souboru protokolu.</span><span class="sxs-lookup"><span data-stu-id="53852-209">If there is a mismatch with hello existing twin’s etag, an error is written toohello log file.</span></span> |
| <span data-ttu-id="53852-210">**odstranění**</span><span class="sxs-lookup"><span data-stu-id="53852-210">**delete**</span></span> |<span data-ttu-id="53852-211">Pokud zařízení už existuje s hello zadaný **id**, odstraní se bez ohledem toohello **značka ETag** hodnotu.</span><span class="sxs-lookup"><span data-stu-id="53852-211">If a device already exists with hello specified **id**, it is deleted without regard toohello **ETag** value.</span></span> <br/><span data-ttu-id="53852-212">Pokud zařízení hello neexistuje, se zapíše chyba toohello souboru protokolu.</span><span class="sxs-lookup"><span data-stu-id="53852-212">If hello device does not exist, an error is written toohello log file.</span></span> |
| <span data-ttu-id="53852-213">**deleteIfMatchETag**</span><span class="sxs-lookup"><span data-stu-id="53852-213">**deleteIfMatchETag**</span></span> |<span data-ttu-id="53852-214">Pokud zařízení už existuje s hello zadaný **id**, odstraní se pouze v případě, že je **značka ETag** shodovat.</span><span class="sxs-lookup"><span data-stu-id="53852-214">If a device already exists with hello specified **id**, it is deleted only if there is an **ETag** match.</span></span> <span data-ttu-id="53852-215">Pokud zařízení hello neexistuje, se zapíše chyba toohello souboru protokolu.</span><span class="sxs-lookup"><span data-stu-id="53852-215">If hello device does not exist, an error is written toohello log file.</span></span> <br/><span data-ttu-id="53852-216">Pokud dojde neshoda značek ETag, se zapíše chyba toohello souboru protokolu.</span><span class="sxs-lookup"><span data-stu-id="53852-216">If there is an ETag mismatch, an error is written toohello log file.</span></span> |

> [!NOTE]
> <span data-ttu-id="53852-217">Pokud data serializace hello nedefinuje explicitně **režimem importu** příznak pro zařízení, nastaví se jako výchozí příliš**createOrUpdate** během hello operace importu.</span><span class="sxs-lookup"><span data-stu-id="53852-217">If hello serialization data does not explicitly define an **importMode** flag for a device, it defaults too**createOrUpdate** during hello import operation.</span></span>

## <a name="import-devices-example--bulk-device-provisioning"></a><span data-ttu-id="53852-218">Import příklad zařízení – hromadné zřizování zařízení</span><span class="sxs-lookup"><span data-stu-id="53852-218">Import devices example – bulk device provisioning</span></span>

<span data-ttu-id="53852-219">ukazuje, Hello následující ukázka kódu C# jak toogenerate více identit zařízení který:</span><span class="sxs-lookup"><span data-stu-id="53852-219">hello following C# code sample illustrates how toogenerate multiple device identities that:</span></span>

* <span data-ttu-id="53852-220">Zahrnout ověřovací klíče.</span><span class="sxs-lookup"><span data-stu-id="53852-220">Include authentication keys.</span></span>
* <span data-ttu-id="53852-221">Objekt blob bloku tohoto zařízení informace tooa zápisu</span><span class="sxs-lookup"><span data-stu-id="53852-221">Write that device information tooa block blob.</span></span>
* <span data-ttu-id="53852-222">Importujte do registru identit hello hello zařízení.</span><span class="sxs-lookup"><span data-stu-id="53852-222">Import hello devices into hello identity registry.</span></span>

```csharp
// Provision 1,000 more devices
var serializedDevices = new List<string>();

for (var i = 0; i < 1000; i++)
{
  // Create a new ExportImportDevice
  // CryptoKeyGenerator is in hello Microsoft.Azure.Devices.Common namespace
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

  // Add device toohello list
  serializedDevices.Add(JsonConvert.SerializeObject(deviceToAdd));
}

// Write hello list toohello blob
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

// Call import using hello blob tooadd new devices
// Log information related toohello job is written toohello same container
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

## <a name="import-devices-example--bulk-deletion"></a><span data-ttu-id="53852-223">Import zařízení příklad – hromadné odstranění</span><span class="sxs-lookup"><span data-stu-id="53852-223">Import devices example – bulk deletion</span></span>

<span data-ttu-id="53852-224">Hello následující ukázka kódu se dozvíte, jak toodelete hello zařízení, které jste přidali pomocí hello předchozí ukázka kódu:</span><span class="sxs-lookup"><span data-stu-id="53852-224">hello following code sample shows you how toodelete hello devices you added using hello previous code sample:</span></span>

```csharp
// Step 1: Update each device's ImportMode toobe Delete
sb = new StringBuilder();
serializedDevices.ForEach(serializedDevice =>
{
  // Deserialize back tooan ExportImportDevice
  var device = JsonConvert.DeserializeObject<ExportImportDevice>(serializedDevice);

  // Update property
  device.ImportMode = ImportMode.Delete;

  // Re-serialize
  sb.AppendLine(JsonConvert.SerializeObject(device));
});

// Step 2: Write hello new import data back toohello block blob
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

// Step 3: Call import using hello same blob toodelete all devices
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

## <a name="get-hello-container-sas-uri"></a><span data-ttu-id="53852-225">Získat hello kontejneru SAS URI</span><span class="sxs-lookup"><span data-stu-id="53852-225">Get hello container SAS URI</span></span>

<span data-ttu-id="53852-226">Hello následující příklad kódu ukazuje, jak toogenerate [identifikátor URI pro SAS](../storage/blobs/storage-dotnet-shared-access-signature-part-2.md) čtení, zápisu a odstranění oprávnění pro kontejner objektů blob:</span><span class="sxs-lookup"><span data-stu-id="53852-226">hello following code sample shows you how toogenerate a [SAS URI](../storage/blobs/storage-dotnet-shared-access-signature-part-2.md) with read, write, and delete permissions for a blob container:</span></span>

```csharp
static string GetContainerSasUri(CloudBlobContainer container)
{
  // Set hello expiry time and permissions for hello container.
  // In this case no start time is specified, so the
  // shared access signature becomes valid immediately.
  var sasConstraints = new SharedAccessBlobPolicy();
  sasConstraints.SharedAccessExpiryTime = DateTime.UtcNow.AddHours(24);
  sasConstraints.Permissions = 
    SharedAccessBlobPermissions.Write | 
    SharedAccessBlobPermissions.Read | 
    SharedAccessBlobPermissions.Delete;

  // Generate hello shared access signature on hello container,
  // setting hello constraints directly on hello signature.
  string sasContainerToken = container.GetSharedAccessSignature(sasConstraints);

  // Return hello URI string for hello container,
  // including hello SAS token.
  return container.Uri + sasContainerToken;
}
```

## <a name="next-steps"></a><span data-ttu-id="53852-227">Další kroky</span><span class="sxs-lookup"><span data-stu-id="53852-227">Next steps</span></span>

<span data-ttu-id="53852-228">V tomto článku jste se dozvěděli, jak tooperform hromadné operace u hello registru identit služby IoT hub.</span><span class="sxs-lookup"><span data-stu-id="53852-228">In this article, you learned how tooperform bulk operations against hello identity registry in an IoT hub.</span></span> <span data-ttu-id="53852-229">Použijte tyto odkazy toolearn informace o správě Azure IoT Hub:</span><span class="sxs-lookup"><span data-stu-id="53852-229">Follow these links toolearn more about managing Azure IoT Hub:</span></span>

* <span data-ttu-id="53852-230">[Metriky služby IoT Hub][lnk-metrics]</span><span class="sxs-lookup"><span data-stu-id="53852-230">[IoT Hub metrics][lnk-metrics]</span></span>
* <span data-ttu-id="53852-231">[Monitorování operací][lnk-monitor]</span><span class="sxs-lookup"><span data-stu-id="53852-231">[Operations monitoring][lnk-monitor]</span></span>

<span data-ttu-id="53852-232">toofurther prozkoumat hello služby IoT Hub, najdete v tématu:</span><span class="sxs-lookup"><span data-stu-id="53852-232">toofurther explore hello capabilities of IoT Hub, see:</span></span>

* <span data-ttu-id="53852-233">[Příručka vývojáře pro službu IoT Hub][lnk-devguide]</span><span class="sxs-lookup"><span data-stu-id="53852-233">[IoT Hub developer guide][lnk-devguide]</span></span>
* <span data-ttu-id="53852-234">[Simulaci zařízení s hranou IoT][lnk-iotedge]</span><span class="sxs-lookup"><span data-stu-id="53852-234">[Simulating a device with IoT Edge][lnk-iotedge]</span></span>

[lnk-metrics]: iot-hub-metrics.md
[lnk-monitor]: iot-hub-operations-monitoring.md

[lnk-devguide]: iot-hub-devguide.md
[lnk-iotedge]: iot-hub-linux-iot-edge-simulated-device.md
