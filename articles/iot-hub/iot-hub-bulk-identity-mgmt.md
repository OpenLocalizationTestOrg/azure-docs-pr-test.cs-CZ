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
# <a name="manage-your-iot-hub-device-identities-in-bulk"></a>Spravovat vaše identit zařízení IoT Hub hromadně

Každé centrum IoT má registru identit toocreate prostředků na zařízení můžete použít ve službě hello. registr identit Hello můžete také toocontrol přístup toohello zařízení přístupem koncových bodů. Tento článek popisuje, jak tooimport a export identit zařízení v hromadné tooand z registru identit.

Import a export operace proběhla v kontextu hello *úlohy* , umožňují tooexecute hromadné operace služby proti služby IoT hub.

Hello **RegistryManager** třída zahrnuje hello **ExportDevicesAsync** a **ImportDevicesAsync** metody, které používají hello **úlohy** Framework. Tyto metody umožňují tooexport, importu a synchronizaci hello celého registru identit IoT hub.

## <a name="what-are-jobs"></a>Jaké jsou úlohy?

Operace s registrem identit pomocí hello **úlohy** systému při hello operace:

* Má potenciálně dlouhou dobu spuštění porovná toostandard spuštění operace.
* Vrací velké množství dat toohello uživatele.

Místo jednoho volání rozhraní API čekání nebo blokování na hello výsledek operace hello, operace hello asynchronně vytvoří **úlohy** tohoto centra IoT. operace Hello pak okamžitě vrátí **JobProperties** objektu.

Hello následující C# code fragment kódu ukazuje, jak toocreate úlohy exportu:

```csharp
// Call an export job on hello IoT Hub tooretrieve all devices
JobProperties exportJob = await registryManager.ExportDevicesAsync(containerSasUri, false);
```

> [!NOTE]
> toouse hello **RegistryManager** třídy v kódu jazyka C#, přidejte hello **Microsoft.Azure.Devices** projektu tooyour balíček NuGet. Hello **RegistryManager** třída je v hello **Microsoft.Azure.Devices** oboru názvů.

Můžete použít hello **RegistryManager** třídy tooquery hello stav hello **úlohy** pomocí hello vrátil **JobProperties** metadat.

Hello následující fragment kódu jazyka C# ukazuje, jak toopoll toosee každých pět sekund, pokud hello úlohy skončil:

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

## <a name="export-devices"></a>Export zařízení

Použití hello **ExportDevicesAsync** metoda tooexport hello celého IoT hub identity registru tooan [Azure Storage](../storage/index.md) pomocí kontejneru objektů blob [sdíleného přístupového podpisu](../storage/common/storage-security-guide.md#data-plane-security).

Tato metoda umožňuje toocreate spolehlivé zálohování vašich zařízení informací v kontejneru objektů blob, který ovládáte.

Hello **ExportDevicesAsync** metoda vyžaduje dva parametry:

* A *řetězec* obsahující identifikátor URI kontejner objektů blob. Tento identifikátor URI musí obsahovat token SAS, která uděluje přístup pro zápis toohello kontejneru. Úloha Hello vytvoří objekt blob bloku data tento kontejner toostore hello serializovat export zařízení. Hello SAS token musí zahrnovat tato oprávnění:

   ```csharp
   SharedAccessBlobPermissions.Write | SharedAccessBlobPermissions.Read | SharedAccessBlobPermissions.Delete
   ```

* A *boolean* určující, pokud chcete exportovat data tooexclude ověřovací klíče. Pokud **false**, ověřovací klíče jsou zahrnuty ve výstupu export. Jinak, jsou klíče exportovány jako **null**.

Hello následující fragment kódu jazyka C# popisuje, jak tooinitiate úlohu export, který obsahuje klíče ověřování zařízení v hello exportovat data a pak dotazování na dokončení:

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

Hello úlohy ukládá její výstup v kontejneru objektů blob hello zadané jako objekt blob bloku s názvem hello **devices.txt**. Hello výstupní data se skládá z data zařízení serializován do formátu JSON, s jedno zařízení na každý řádek.

Hello následující příklad ukazuje hello výstupní data:

```json
{"id":"Device1","eTag":"MA==","status":"enabled","authentication":{"symmetricKey":{"primaryKey":"abc=","secondaryKey":"def="}}}
{"id":"Device2","eTag":"MA==","status":"enabled","authentication":{"symmetricKey":{"primaryKey":"abc=","secondaryKey":"def="}}}
{"id":"Device3","eTag":"MA==","status":"disabled","authentication":{"symmetricKey":{"primaryKey":"abc=","secondaryKey":"def="}}}
{"id":"Device4","eTag":"MA==","status":"disabled","authentication":{"symmetricKey":{"primaryKey":"abc=","secondaryKey":"def="}}}
{"id":"Device5","eTag":"MA==","status":"enabled","authentication":{"symmetricKey":{"primaryKey":"abc=","secondaryKey":"def="}}}
```

Pokud zařízení obsahuje twin dat, hello twin data také exportovat společně s hello data zařízení. Hello následující příklad ukazuje tento formát. Všechna data z řádku "twinETag" hello dokud hello end jsou twin data.

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

Pokud budete potřebovat přístup k datům toothis v kódu, můžete snadno rekonstruovat tato data pomocí hello **ExportImportDevice** třídy. Hello následující fragment kódu jazyka C# ukazuje, jak tooread informace o zařízení, která byla předtím exportovali objekt blob bloku tooa:

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
> Můžete taky hello **GetDevicesAsync** metoda hello **RegistryManager** třída toofetch seznam zařízení. Však tento přístup má pevný cap 1000 hello počtu zařízení objekty, které jsou vráceny. Hello očekává případ použití pro hello **GetDevicesAsync** metoda je pro vývoj scénáře tooaid ladění a se nedoporučuje pro produkční zatížení.

## <a name="import-devices"></a>Import zařízení

Hello **ImportDevicesAsync** metoda v hello **RegistryManager** třída umožňuje operace importu a synchronizaci hromadné tooperform v registru identit IoT hub. Jako hello **ExportDevicesAsync** metoda, hello **ImportDevicesAsync** metoda používá hello **úlohy** framework.

Vezměte v potaz pomocí hello **ImportDevicesAsync** metoda vzhledem k tomu, že v tooprovisioning nová zařízení přidání v registru identit, můžete také aktualizovat a odstranit existující zařízení.

> [!WARNING]
> Operace importu nelze vrátit zpět. Vždy zálohovat stávající data pomocí hello **ExportDevicesAsync** kontejner objektů blob tooanother metoda před provedením hromadné změny registru identit tooyour.

Hello **ImportDevicesAsync** metoda přebírá dva parametry:

* A *řetězec* identifikátor URI, který obsahuje [Azure Storage](../storage/index.md) blob kontejneru toouse jako *vstupní* toohello úlohy. Tento identifikátor URI musí obsahovat token SAS, která uděluje přístup pro čtení toohello kontejneru. Tento kontejner musí obsahovat objekt blob s názvem hello **devices.txt** obsahující hello serializovat zařízení data tooimport do registru identit. Hello import dat musí obsahovat informace o zařízení v hello stejné JSON formátu této hello **ExportImportDevice** úloha používá při vytváření **devices.txt** objektů blob. Hello SAS token musí zahrnovat tato oprávnění:

   ```csharp
   SharedAccessBlobPermissions.Read
   ```
* A *řetězec* identifikátor URI, který obsahuje [Azure Storage](https://azure.microsoft.com/documentation/services/storage/) blob kontejneru toouse jako *výstup* z úlohy hello. Hello úloha vytvoří objekt blob bloku v tento kontejner toostore veškeré informace o chybě z hello dokončení importu **úlohy**. Hello SAS token musí zahrnovat tato oprávnění:

   ```csharp
   SharedAccessBlobPermissions.Write | SharedAccessBlobPermissions.Read | SharedAccessBlobPermissions.Delete
   ```

> [!NOTE]
> Hello dva parametry může ukazovat toohello stejný kontejner objektů blob. samostatné parametry Hello jednoduše povolte větší kontrolu nad daty jako kontejner výstup hello vyžaduje další oprávnění.

Hello následující C# code fragment kódu ukazuje, jak tooinitiate úlohy importu:

```csharp
JobProperties importJob = await registryManager.ImportDevicesAsync(containerSasUri, containerSasUri);
```

Tato metoda může být také použít tooimport hello data pro dvojče zařízení hello. Hello formát pro hello datového vstupu je hello stejný jako formát hello ukazuje hello **ExportDevicesAsync** části. Tímto způsobem můžete znovu naimportovat data hello exportovali. Hello **$metadata** je volitelný.

## <a name="import-behavior"></a>Import chování

Můžete použít hello **ImportDevicesAsync** metoda tooperform hello následující hromadné operace v registru identit:

* Hromadné registrace nové zařízení
* Hromadné odstranění stávajících zařízení
* Hromadné změny stavu (povolit nebo zakázat zařízení)
* Hromadné přiřazení nových klíčů ověřování zařízení
* Hromadné auto-nové vygenerování klíče pro ověřování zařízení
* Hromadné aktualizace dat twin

Můžete nastavit libovolnou kombinaci hello předcházející operace v rámci jednoho **ImportDevicesAsync** volání. Například můžete zaregistrovat nové zařízení a odstranění nebo aktualizaci stávajících zařízení v hello stejnou dobu. Při použití spolu s hello **ExportDevicesAsync** metody úplně můžete migrovat všechna vaše zařízení z jedné tooanother centra IoT.

Obsahuje-li soubor importu hello twin metadata, tato metadata přepíše existující metadata twin hello. Pokud soubor importu hello neobsahuje twin metadata, pak pouze hello `lastUpdateTime` metadat je aktualizovat pomocí hello aktuální čas.

Volitelné použití hello **režimem importu** vlastnost hello import serializace dat pro každé zařízení toocontrol hello import proces podle zařízení. Hello **režimem importu** vlastnost má hello následující možnosti:

| režimem importu | Popis |
| --- | --- |
| **createOrUpdate** |Pokud zařízení s hello zadaný neexistuje **id**, bude nově zaregistrovaný. <br/>Pokud zařízení hello již existuje, dojde k přepsání hello poskytl vstupní data bez ohledem toohello stávající informace o **značka ETag** hodnotu. <br> uživatel Hello Volitelně můžete zadat data twin spolu s daty hello zařízení. značka etag Hello twin,-li zadána, je zpracovat nezávisle z hello zařízení etag. Pokud se neshodují s hello existující twin etag, se zapíše chyba toohello souboru protokolu. |
| **vytvoření** |Pokud zařízení s hello zadaný neexistuje **id**, bude nově zaregistrovaný. <br/>Pokud zařízení hello již existuje, se zapíše chyba toohello souboru protokolu. <br> uživatel Hello Volitelně můžete zadat data twin spolu s daty hello zařízení. značka etag Hello twin,-li zadána, je zpracovat nezávisle z hello zařízení etag. Pokud se neshodují s hello existující twin etag, se zapíše chyba toohello souboru protokolu. |
| **aktualizace** |Pokud zařízení už existuje s hello zadaný **id**, přepíše stávající informace o hello poskytl vstupní data bez ohledem toohello **značka ETag** hodnotu. <br/>Pokud zařízení hello neexistuje, se zapíše chyba toohello souboru protokolu. |
| **updateIfMatchETag** |Pokud zařízení už existuje s hello zadaný **id**, přepíše stávající informace o hello poskytl vstupní data jenom v případě, že je **značka ETag** shodovat. <br/>Pokud zařízení hello neexistuje, se zapíše chyba toohello souboru protokolu. <br/>Pokud dojde **značka ETag** neshoda, se zapíše chyba toohello souboru protokolu. |
| **createOrUpdateIfMatchETag** |Pokud zařízení s hello zadaný neexistuje **id**, bude nově zaregistrovaný. <br/>Pokud zařízení hello již existuje, stávající informace o přepsána hello poskytl vstupní data jenom v případě, že je **značka ETag** shodovat. <br/>Pokud dojde **značka ETag** neshoda, se zapíše chyba toohello souboru protokolu. <br> uživatel Hello Volitelně můžete zadat data twin spolu s daty hello zařízení. značka etag Hello twin,-li zadána, je zpracovat nezávisle z hello zařízení etag. Pokud se neshodují s hello existující twin etag, se zapíše chyba toohello souboru protokolu. |
| **odstranění** |Pokud zařízení už existuje s hello zadaný **id**, odstraní se bez ohledem toohello **značka ETag** hodnotu. <br/>Pokud zařízení hello neexistuje, se zapíše chyba toohello souboru protokolu. |
| **deleteIfMatchETag** |Pokud zařízení už existuje s hello zadaný **id**, odstraní se pouze v případě, že je **značka ETag** shodovat. Pokud zařízení hello neexistuje, se zapíše chyba toohello souboru protokolu. <br/>Pokud dojde neshoda značek ETag, se zapíše chyba toohello souboru protokolu. |

> [!NOTE]
> Pokud data serializace hello nedefinuje explicitně **režimem importu** příznak pro zařízení, nastaví se jako výchozí příliš**createOrUpdate** během hello operace importu.

## <a name="import-devices-example--bulk-device-provisioning"></a>Import příklad zařízení – hromadné zřizování zařízení

ukazuje, Hello následující ukázka kódu C# jak toogenerate více identit zařízení který:

* Zahrnout ověřovací klíče.
* Objekt blob bloku tohoto zařízení informace tooa zápisu
* Importujte do registru identit hello hello zařízení.

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

## <a name="import-devices-example--bulk-deletion"></a>Import zařízení příklad – hromadné odstranění

Hello následující ukázka kódu se dozvíte, jak toodelete hello zařízení, které jste přidali pomocí hello předchozí ukázka kódu:

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

## <a name="get-hello-container-sas-uri"></a>Získat hello kontejneru SAS URI

Hello následující příklad kódu ukazuje, jak toogenerate [identifikátor URI pro SAS](../storage/blobs/storage-dotnet-shared-access-signature-part-2.md) čtení, zápisu a odstranění oprávnění pro kontejner objektů blob:

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

## <a name="next-steps"></a>Další kroky

V tomto článku jste se dozvěděli, jak tooperform hromadné operace u hello registru identit služby IoT hub. Použijte tyto odkazy toolearn informace o správě Azure IoT Hub:

* [Metriky služby IoT Hub][lnk-metrics]
* [Monitorování operací][lnk-monitor]

toofurther prozkoumat hello služby IoT Hub, najdete v tématu:

* [Příručka vývojáře pro službu IoT Hub][lnk-devguide]
* [Simulaci zařízení s hranou IoT][lnk-iotedge]

[lnk-metrics]: iot-hub-metrics.md
[lnk-monitor]: iot-hub-operations-monitoring.md

[lnk-devguide]: iot-hub-devguide.md
[lnk-iotedge]: iot-hub-linux-iot-edge-simulated-device.md
