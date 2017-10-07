---
title: "aaaUnderstand hello dotazovacího jazyka pro Azure IoT Hub | Microsoft Docs"
description: "Příručka vývojáře – popis dotazu jazyka SQL jako IoT Hub hello používá tooretrieve informace o úlohách ze služby IoT hub a dvojčata zařízení."
services: iot-hub
documentationcenter: .net
author: fsautomata
manager: timlt
editor: 
ms.assetid: 851a9ed3-b69e-422e-8a5d-1d79f91ddf15
ms.service: iot-hub
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 05/25/17
ms.author: elioda
ms.openlocfilehash: 01a7c8ffdf44c6c27b834739d02c8fef1dd3d3fd
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="reference---iot-hub-query-language-for-device-twins-jobs-and-message-routing"></a><span data-ttu-id="8f50d-103">Referenční dokumentace - IoT Hub dotazovacího jazyka pro dvojčata zařízení, úlohy a směrování zpráv</span><span class="sxs-lookup"><span data-stu-id="8f50d-103">Reference - IoT Hub query language for device twins, jobs, and message routing</span></span>

<span data-ttu-id="8f50d-104">Služba IoT Hub zajišťuje a výkonné jazyka SQL tooretrieve informace ohledně [dvojčata zařízení] [ lnk-twins] a [úlohy][lnk-jobs]a [směrování zpráv][lnk-devguide-messaging-routes].</span><span class="sxs-lookup"><span data-stu-id="8f50d-104">IoT Hub provides a powerful SQL-like language tooretrieve information regarding [device twins][lnk-twins] and [jobs][lnk-jobs], and [message routing][lnk-devguide-messaging-routes].</span></span> <span data-ttu-id="8f50d-105">Tento článek představuje:</span><span class="sxs-lookup"><span data-stu-id="8f50d-105">This article presents:</span></span>

* <span data-ttu-id="8f50d-106">Úvod toohello hlavní funkce hello dotazovacího jazyka pro IoT Hub, a</span><span class="sxs-lookup"><span data-stu-id="8f50d-106">An introduction toohello major features of hello IoT Hub query language, and</span></span>
* <span data-ttu-id="8f50d-107">Hello podrobný popis jazyka hello.</span><span class="sxs-lookup"><span data-stu-id="8f50d-107">hello detailed description of hello language.</span></span>

## <a name="get-started-with-device-twin-queries"></a><span data-ttu-id="8f50d-108">Začínáme s dotazy twin zařízení</span><span class="sxs-lookup"><span data-stu-id="8f50d-108">Get started with device twin queries</span></span>
<span data-ttu-id="8f50d-109">[Dvojčata zařízení] [ lnk-twins] může obsahovat libovolné objekty JSON vlastnosti a značky.</span><span class="sxs-lookup"><span data-stu-id="8f50d-109">[Device twins][lnk-twins] can contain arbitrary JSON objects as both tags and properties.</span></span> <span data-ttu-id="8f50d-110">IoT Hub můžete dvojčata zařízení tooquery jako jeden dokument JSON obsahující všechny informace o dvojici zařízení.</span><span class="sxs-lookup"><span data-stu-id="8f50d-110">IoT Hub enables you tooquery device twins as a single JSON document containing all device twin information.</span></span>
<span data-ttu-id="8f50d-111">Předpokládejme například, vaše dvojčata zařízení IoT hub měli hello strukturu:</span><span class="sxs-lookup"><span data-stu-id="8f50d-111">Assume, for instance, that your IoT hub device twins have hello following structure:</span></span>

```json
{
    "deviceId": "myDeviceId",
    "etag": "AAAAAAAAAAc=",
    "tags": {
        "location": {
            "region": "US",
            "plant": "Redmond43"
        }
    },
    "properties": {
        "desired": {
            "telemetryConfig": {
                "configId": "db00ebf5-eeeb-42be-86a1-458cccb69e57",
                "sendFrequencyInSecs": 300
            },
            "$metadata": {
            ...
            },
            "$version": 4
        },
        "reported": {
            "connectivity": {
                "type": "cellular"
            },
            "telemetryConfig": {
                "configId": "db00ebf5-eeeb-42be-86a1-458cccb69e57",
                "sendFrequencyInSecs": 300,
                "status": "Success"
            },
            "$metadata": {
            ...
            },
            "$version": 7
        }
    }
}
```

<span data-ttu-id="8f50d-112">IoT Hub zpřístupní dvojčata zařízení hello jako kolekce dokumentů s názvem **zařízení**.</span><span class="sxs-lookup"><span data-stu-id="8f50d-112">IoT Hub exposes hello device twins as a document collection called **devices**.</span></span>
<span data-ttu-id="8f50d-113">Proto hello následující dotaz načte hello celou sadu dvojčata zařízení:</span><span class="sxs-lookup"><span data-stu-id="8f50d-113">So hello following query retrieves hello whole set of device twins:</span></span>

```sql
SELECT * FROM devices
```

> [!NOTE]
> <span data-ttu-id="8f50d-114">[Sady SDK služby Azure IoT] [ lnk-hub-sdks] podporu stránkování výsledků velký.</span><span class="sxs-lookup"><span data-stu-id="8f50d-114">[Azure IoT SDKs][lnk-hub-sdks] support paging of large results.</span></span>

<span data-ttu-id="8f50d-115">Centrum IoT vám umožní dvojčata zařízení tooretrieve filtrování pomocí libovolné podmínky.</span><span class="sxs-lookup"><span data-stu-id="8f50d-115">IoT Hub allows you tooretrieve device twins filtering with arbitrary conditions.</span></span> <span data-ttu-id="8f50d-116">Pro instanci,</span><span class="sxs-lookup"><span data-stu-id="8f50d-116">For instance,</span></span>

```sql
SELECT * FROM devices
WHERE tags.location.region = 'US'
```

<span data-ttu-id="8f50d-117">načte hello dvojčata zařízení s hello **location.region** značky nastavit příliš**USA**.</span><span class="sxs-lookup"><span data-stu-id="8f50d-117">retrieves hello device twins with hello **location.region** tag set too**US**.</span></span>
<span data-ttu-id="8f50d-118">Logické operátory a aritmetické porovnání jsou podporovány také, například</span><span class="sxs-lookup"><span data-stu-id="8f50d-118">Boolean operators and arithmetic comparisons are supported as well, for example</span></span>

```sql
SELECT * FROM devices
WHERE tags.location.region = 'US'
    AND properties.reported.telemetryConfig.sendFrequencyInSecs >= 60
```

<span data-ttu-id="8f50d-119">načte všechny dvojčata zařízení nachází v hello nám nakonfigurované toosend telemetrie méně často než každou minutu.</span><span class="sxs-lookup"><span data-stu-id="8f50d-119">retrieves all device twins located in hello US configured toosend telemetry less often than every minute.</span></span> <span data-ttu-id="8f50d-120">Pro potřeby je také možné toouse konstanty pole s hello **IN** a **NV** operátory (ne v).</span><span class="sxs-lookup"><span data-stu-id="8f50d-120">As a convenience, it is also possible toouse array constants with hello **IN** and **NIN** (not in) operators.</span></span> <span data-ttu-id="8f50d-121">Pro instanci,</span><span class="sxs-lookup"><span data-stu-id="8f50d-121">For instance,</span></span>

```sql
SELECT * FROM devices
WHERE properties.reported.connectivity IN ['wired', 'wifi']
```

<span data-ttu-id="8f50d-122">načte všechny dvojčata zařízení, které ohlásil Wi-Fi nebo drátové připojení.</span><span class="sxs-lookup"><span data-stu-id="8f50d-122">retrieves all device twins that reported WiFi or wired connectivity.</span></span> <span data-ttu-id="8f50d-123">Ho je často nutné tooidentify všechny dvojčata zařízení, které obsahují určité vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="8f50d-123">It is often necessary tooidentify all device twins that contain a specific property.</span></span> <span data-ttu-id="8f50d-124">IoT Hub podporuje funkce hello `is_defined()` pro tento účel.</span><span class="sxs-lookup"><span data-stu-id="8f50d-124">IoT Hub supports hello function `is_defined()` for this purpose.</span></span> <span data-ttu-id="8f50d-125">Pro instanci,</span><span class="sxs-lookup"><span data-stu-id="8f50d-125">For instance,</span></span>

```SQL
SELECT * FROM devices
WHERE is_defined(properties.reported.connectivity)
```

<span data-ttu-id="8f50d-126">načíst všechny dvojčata zařízení, které definují hello `connectivity` hlášené vlastnost.</span><span class="sxs-lookup"><span data-stu-id="8f50d-126">retrieved all device twins that define hello `connectivity` reported property.</span></span> <span data-ttu-id="8f50d-127">Odkazovat toohello [klauzule WHERE] [ lnk-query-where] části pro úplný přehled hello hello možností filtrování.</span><span class="sxs-lookup"><span data-stu-id="8f50d-127">Refer toohello [WHERE clause][lnk-query-where] section for hello full reference of hello filtering capabilities.</span></span>

<span data-ttu-id="8f50d-128">Seskupení a agregace jsou také podporovány.</span><span class="sxs-lookup"><span data-stu-id="8f50d-128">Grouping and aggregations are also supported.</span></span> <span data-ttu-id="8f50d-129">Pro instanci,</span><span class="sxs-lookup"><span data-stu-id="8f50d-129">For instance,</span></span>

```sql
SELECT properties.reported.telemetryConfig.status AS status,
    COUNT() AS numberOfDevices
FROM devices
GROUP BY properties.reported.telemetryConfig.status
```

<span data-ttu-id="8f50d-130">Vrátí hello počet zařízení hello každý stav konfigurace telemetrie.</span><span class="sxs-lookup"><span data-stu-id="8f50d-130">returns hello count of hello devices in each telemetry configuration status.</span></span>

```json
[
    {
        "numberOfDevices": 3,
        "status": "Success"
    },
    {
        "numberOfDevices": 2,
        "status": "Pending"
    },
    {
        "numberOfDevices": 1,
        "status": "Error"
    }
]
```

<span data-ttu-id="8f50d-131">Hello předchozí příklad znázorňuje situaci, kdy tři zařízení hlášené úspěšná konfigurace, dva jsou stále aplikování hello konfigurace a jeden ohlásil chybu.</span><span class="sxs-lookup"><span data-stu-id="8f50d-131">hello preceding example illustrates a situation where three devices reported successful configuration, two are still applying hello configuration, and one reported an error.</span></span>

### <a name="c-example"></a><span data-ttu-id="8f50d-132">Příklad jazyka C#</span><span class="sxs-lookup"><span data-stu-id="8f50d-132">C# example</span></span>
<span data-ttu-id="8f50d-133">Hello dotazu funkce je zveřejněna rozhraním hello [C# sady SDK služby] [ lnk-hub-sdks] v hello **RegistryManager** třídy.</span><span class="sxs-lookup"><span data-stu-id="8f50d-133">hello query functionality is exposed by hello [C# service SDK][lnk-hub-sdks] in hello **RegistryManager** class.</span></span>
<span data-ttu-id="8f50d-134">Tady je příklad jednoduchého dotazu:</span><span class="sxs-lookup"><span data-stu-id="8f50d-134">Here is an example of a simple query:</span></span>

```csharp
var query = registryManager.CreateQuery("SELECT * FROM devices", 100);
while (query.HasMoreResults)
{
    var page = await query.GetNextAsTwinAsync();
    foreach (var twin in page)
    {
        // do work on twin object
    }
}
```

<span data-ttu-id="8f50d-135">Poznámka: jak hello **dotazu** vytvořit instanci objektu s velikostí stránky (až too1000) a pak může být více stránek načíst volání hello **GetNextAsTwinAsync** metody vícekrát.</span><span class="sxs-lookup"><span data-stu-id="8f50d-135">Note how hello **query** object is instantiated with a page size (up too1000), and then multiple pages can be retrieved by calling hello **GetNextAsTwinAsync** methods multiple times.</span></span>
<span data-ttu-id="8f50d-136">Poznámka: Tento objekt dotazu hello vystavuje více **Další\***, v závislosti na možnost hello deserializaci vyžadované hello dotazu, například objekty zařízení twin nebo úlohy, nebo nešifrovaná toobe JSON pro používání projekce.</span><span class="sxs-lookup"><span data-stu-id="8f50d-136">Note that hello query object exposes multiple **Next\***, depending on hello deserialization option required by hello query, such as device twin or job objects, or plain JSON toobe used when using projections.</span></span>

### <a name="nodejs-example"></a><span data-ttu-id="8f50d-137">Příklad Node.js</span><span class="sxs-lookup"><span data-stu-id="8f50d-137">Node.js example</span></span>
<span data-ttu-id="8f50d-138">Hello dotazu funkce je zveřejněna rozhraním hello [sady SDK služby Azure IoT pro Node.js] [ lnk-hub-sdks] v hello **registru** objektu.</span><span class="sxs-lookup"><span data-stu-id="8f50d-138">hello query functionality is exposed by hello [Azure IoT service SDK for Node.js][lnk-hub-sdks] in hello **Registry** object.</span></span>
<span data-ttu-id="8f50d-139">Tady je příklad jednoduchého dotazu:</span><span class="sxs-lookup"><span data-stu-id="8f50d-139">Here is an example of a simple query:</span></span>

```nodejs
var query = registry.createQuery('SELECT * FROM devices', 100);
var onResults = function(err, results) {
    if (err) {
        console.error('Failed toofetch hello results: ' + err.message);
    } else {
        // Do something with hello results
        results.forEach(function(twin) {
            console.log(twin.deviceId);
        });

        if (query.hasMoreResults) {
            query.nextAsTwin(onResults);
        }
    }
};
query.nextAsTwin(onResults);
```

<span data-ttu-id="8f50d-140">Poznámka: jak hello **dotazu** vytvořit instanci objektu s velikostí stránky (až too1000) a pak může být více stránek načíst volání hello **nextAsTwin** metody vícekrát.</span><span class="sxs-lookup"><span data-stu-id="8f50d-140">Note how hello **query** object is instantiated with a page size (up too1000), and then multiple pages can be retrieved by calling hello **nextAsTwin** methods multiple times.</span></span>
<span data-ttu-id="8f50d-141">Poznámka: Tento objekt dotazu hello vystavuje více **Další\***, v závislosti na možnost hello deserializaci vyžadované hello dotazu, například objekty zařízení twin nebo úlohy, nebo nešifrovaná toobe JSON pro používání projekce.</span><span class="sxs-lookup"><span data-stu-id="8f50d-141">Note that hello query object exposes multiple **next\***, depending on hello deserialization option required by hello query, such as device twin or job objects, or plain JSON toobe used when using projections.</span></span>

### <a name="limitations"></a><span data-ttu-id="8f50d-142">Omezení</span><span class="sxs-lookup"><span data-stu-id="8f50d-142">Limitations</span></span>
> [!IMPORTANT]
> <span data-ttu-id="8f50d-143">Výsledky dotazu může mít několik minut zpoždění s ohledem toohello nejnovější hodnoty v dvojčata zařízení.</span><span class="sxs-lookup"><span data-stu-id="8f50d-143">Query results can have a few minutes of delay with respect toohello latest values in device twins.</span></span> <span data-ttu-id="8f50d-144">Pokud dotazuje dvojčata jednotlivých zařízení podle id, vždycky je vhodnější toouse hello načíst twin rozhraní API pro zařízení, která vždy obsahuje nejnovější hodnoty hello a má, vyšší omezení.</span><span class="sxs-lookup"><span data-stu-id="8f50d-144">If querying individual device twins by id, it is always preferable toouse hello retrieve device twin API, which always contains hello latest values and has higher throttling limits.</span></span>

<span data-ttu-id="8f50d-145">V současné době porovnání jsou podporovány pouze mezi primitivní typy (žádné objekty), například `... WHERE properties.desired.config = properties.reported.config` je podporováno pouze v případě, že tyto vlastnosti mají primitivní hodnoty.</span><span class="sxs-lookup"><span data-stu-id="8f50d-145">Currently, comparisons are supported only between primitive types (no objects), for instance `... WHERE properties.desired.config = properties.reported.config` is supported only if those properties have primitive values.</span></span>

## <a name="get-started-with-jobs-queries"></a><span data-ttu-id="8f50d-146">Začínáme s dotazy úlohy</span><span class="sxs-lookup"><span data-stu-id="8f50d-146">Get started with jobs queries</span></span>
<span data-ttu-id="8f50d-147">[Úlohy] [ lnk-jobs] poskytují způsob, jak tooexecute operace na sadu zařízení.</span><span class="sxs-lookup"><span data-stu-id="8f50d-147">[Jobs][lnk-jobs] provide a way tooexecute operations on sets of devices.</span></span> <span data-ttu-id="8f50d-148">Každý dvojče zařízení obsahuje informace o hello hello úloh, které je součástí v kolekci s názvem **úlohy**.</span><span class="sxs-lookup"><span data-stu-id="8f50d-148">Each device twin contains hello information of hello jobs of which it is part in a collection called **jobs**.</span></span>
<span data-ttu-id="8f50d-149">Logicky,</span><span class="sxs-lookup"><span data-stu-id="8f50d-149">Logically,</span></span>

```json
{
    "deviceId": "myDeviceId",
    "etag": "AAAAAAAAAAc=",
    "tags": {
        ...
    },
    "properties": {
        ...
    },
    "jobs": [
        {
            "deviceId": "myDeviceId",
            "jobId": "myJobId",
            "jobType": "scheduleTwinUpdate",
            "status": "completed",
            "startTimeUtc": "2016-09-29T18:18:52.7418462",
            "endTimeUtc": "2016-09-29T18:20:52.7418462",
            "createdDateTimeUtc": "2016-09-29T18:18:56.7787107Z",
            "lastUpdatedDateTimeUtc": "2016-09-29T18:18:56.8894408Z",
            "outcome": {
                "deviceMethodResponse": null
            }
        },
        ...
    ]
}
```

<span data-ttu-id="8f50d-150">Tato kolekce je v současné době dotazovatelné jako **devices.jobs** v hello dotazovacího jazyka pro IoT Hub.</span><span class="sxs-lookup"><span data-stu-id="8f50d-150">Currently, this collection is queryable as **devices.jobs** in hello IoT Hub query language.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="8f50d-151">V současné době je nikdy vrácena vlastnost úlohy hello při dotazování dvojčata zařízení (to znamená, dotazy, které obsahuje, ze zařízení").</span><span class="sxs-lookup"><span data-stu-id="8f50d-151">Currently, hello jobs property is never returned when querying device twins (that is, queries that contains 'FROM devices').</span></span> <span data-ttu-id="8f50d-152">Lze přistupovat pouze přímo s dotazy pomocí `FROM devices.jobs`.</span><span class="sxs-lookup"><span data-stu-id="8f50d-152">It can only be accessed directly with queries using `FROM devices.jobs`.</span></span>
>
>

<span data-ttu-id="8f50d-153">Například tooget všechny úlohy (posledních a plánované) ovlivňující jedno zařízení, můžete použít hello následující dotaz:</span><span class="sxs-lookup"><span data-stu-id="8f50d-153">For instance, tooget all jobs (past and scheduled) that affect a single device, you can use hello following query:</span></span>

```sql
SELECT * FROM devices.jobs
WHERE devices.jobs.deviceId = 'myDeviceId'
```

<span data-ttu-id="8f50d-154">Všimněte si, jak tento dotaz poskytuje hello konkrétní zařízení stav (a případně hello přímá metoda odpověď) každé úlohy vrátila.</span><span class="sxs-lookup"><span data-stu-id="8f50d-154">Note how this query provides hello device-specific status (and possibly hello direct method response) of each job returned.</span></span>
<span data-ttu-id="8f50d-155">Je také možné toofilter s libovolnými Boolean podmínek na všechny vlastnosti objektu v hello **devices.jobs** kolekce.</span><span class="sxs-lookup"><span data-stu-id="8f50d-155">It is also possible toofilter with arbitrary Boolean conditions on all object properties in hello **devices.jobs** collection.</span></span>
<span data-ttu-id="8f50d-156">Například hello následující dotaz:</span><span class="sxs-lookup"><span data-stu-id="8f50d-156">For instance, hello following query:</span></span>

```sql
SELECT * FROM devices.jobs
WHERE devices.jobs.deviceId = 'myDeviceId'
    AND devices.jobs.jobType = 'scheduleTwinUpdate'
    AND devices.jobs.status = 'completed'
    AND devices.jobs.createdTimeUtc > '2016-09-01'
```

<span data-ttu-id="8f50d-157">načte všechny dokončit zařízení twin aktualizace úlohy pro zařízení **myDeviceId** vytvořených po září 2016.</span><span class="sxs-lookup"><span data-stu-id="8f50d-157">retrieves all completed device twin update jobs for device **myDeviceId** that were created after September 2016.</span></span>

<span data-ttu-id="8f50d-158">Je také možné tooretrieve hello podle zařízení výstupy jednu úlohu.</span><span class="sxs-lookup"><span data-stu-id="8f50d-158">It is also possible tooretrieve hello per-device outcomes of a single job.</span></span>

```sql
SELECT * FROM devices.jobs
WHERE devices.jobs.jobId = 'myJobId'
```

### <a name="limitations"></a><span data-ttu-id="8f50d-159">Omezení</span><span class="sxs-lookup"><span data-stu-id="8f50d-159">Limitations</span></span>
<span data-ttu-id="8f50d-160">V současné době se dotazuje na **devices.jobs** nepodporují:</span><span class="sxs-lookup"><span data-stu-id="8f50d-160">Currently, queries on **devices.jobs** do not support:</span></span>

* <span data-ttu-id="8f50d-161">Projekce, proto pouze `SELECT *` je možné.</span><span class="sxs-lookup"><span data-stu-id="8f50d-161">Projections, therefore only `SELECT *` is possible.</span></span>
* <span data-ttu-id="8f50d-162">Podmínky, které odkazují dvojče zařízení toohello v přidání vlastnosti toojob (viz hello předcházející části).</span><span class="sxs-lookup"><span data-stu-id="8f50d-162">Conditions that refer toohello device twin in addition toojob properties (see hello preceding section).</span></span>
* <span data-ttu-id="8f50d-163">Provádění agregace, jako je počet, avg, Seskupit podle.</span><span class="sxs-lookup"><span data-stu-id="8f50d-163">Performing aggregations, such as count, avg, group by.</span></span>

## <a name="get-started-with-device-to-cloud-message-routes-query-expressions"></a><span data-ttu-id="8f50d-164">Začínáme s výrazy dotazů trasy zprávu typu zařízení cloud</span><span class="sxs-lookup"><span data-stu-id="8f50d-164">Get started with device-to-cloud message routes query expressions</span></span>

<span data-ttu-id="8f50d-165">Pomocí [zařízení cloud trasy][lnk-devguide-messaging-routes], můžete nakonfigurovat IoT Hub toodispatch zařízení cloud zprávy koncové body toodifferent založené na výrazech vyhodnotit na základě jednotlivých zpráv.</span><span class="sxs-lookup"><span data-stu-id="8f50d-165">Using [device-to-cloud routes][lnk-devguide-messaging-routes], you can configure IoT Hub toodispatch device-to-cloud messages toodifferent endpoints based on expressions evaluated against individual messages.</span></span>

<span data-ttu-id="8f50d-166">Hello trasy [podmínku] [ lnk-query-expressions] hello používá stejné dotazovací jazyk IoT Hub jako podmínky v dotazech twin a úlohy.</span><span class="sxs-lookup"><span data-stu-id="8f50d-166">hello route [condition][lnk-query-expressions] uses hello same IoT Hub query language as conditions in twin and job queries.</span></span> <span data-ttu-id="8f50d-167">Podmínky trasy se vyhodnocují na hlavičky zpráv hello a text.</span><span class="sxs-lookup"><span data-stu-id="8f50d-167">Route conditions are evaluated on hello message headers and body.</span></span> <span data-ttu-id="8f50d-168">Směrování výraz dotazu může zahrnovat pouze hlavičky zpráv, pouze tělo zprávy hello, nebo obě zpráva hlavičky a tělo zprávy.</span><span class="sxs-lookup"><span data-stu-id="8f50d-168">Your routing query expression may involve only message headers, only hello message body, or both message headers and message body.</span></span> <span data-ttu-id="8f50d-169">IoT Hub předpokládá konkrétní schéma pro hello hlavičky a tělo zprávy ve zprávách tooroute pořadí a hello následující části popisují, co je požadované pro trasu tooproperly IoT Hub:</span><span class="sxs-lookup"><span data-stu-id="8f50d-169">IoT Hub assumes a specific schema for hello headers and message body in order tooroute messages, and hello following sections describe what is required for IoT Hub tooproperly route:</span></span>

### <a name="routing-on-message-headers"></a><span data-ttu-id="8f50d-170">Směrování v záhlaví zpráv</span><span class="sxs-lookup"><span data-stu-id="8f50d-170">Routing on message headers</span></span>

<span data-ttu-id="8f50d-171">IoT Hub předpokládá hello následující reprezentace JSON hlavičky zpráv pro směrování zpráv:</span><span class="sxs-lookup"><span data-stu-id="8f50d-171">IoT Hub assumes hello following JSON representation of message headers for message routing:</span></span>

```json
{
    "$messageId": "",
    "$enqueuedTime": "",
    "$to": "",
    "$expiryTimeUtc": "",
    "$correlationId": "",
    "$userId": "",
    "$ack": "",
    "$connectionDeviceId": "",
    "$connectionDeviceGenerationId": "",
    "$connectionAuthMethod": "",
    "$content-type": "",
    "$content-encoding": "",

    "userProperty1": "",
    "userProperty2": ""
}
```

<span data-ttu-id="8f50d-172">Vlastnosti zprávu systému mají předponu hello `'$'` symbol.</span><span class="sxs-lookup"><span data-stu-id="8f50d-172">Message system properties are prefixed with hello `'$'` symbol.</span></span>
<span data-ttu-id="8f50d-173">Vlastnosti uživatelů jsou vždy přistupovat pomocí jeho názvu.</span><span class="sxs-lookup"><span data-stu-id="8f50d-173">User properties are always accessed with their name.</span></span> <span data-ttu-id="8f50d-174">Pokud název vlastnosti uživatele se stane toocoincide s vlastností systému (například `$to`), bude načten hello vlastnost uživatele s hello `$to` výraz.</span><span class="sxs-lookup"><span data-stu-id="8f50d-174">If a user property name happens toocoincide with a system property (such as `$to`), hello user property will be retrieved with hello `$to` expression.</span></span>
<span data-ttu-id="8f50d-175">Vždy přístup k vlastnosti systému hello pomocí závorek `{}`: například můžete použít výraz hello `{$to}` tooaccess hello vlastnost systému `to`.</span><span class="sxs-lookup"><span data-stu-id="8f50d-175">You can always access hello system property using brackets `{}`: for instance, you can use hello expression `{$to}` tooaccess hello system property `to`.</span></span> <span data-ttu-id="8f50d-176">Názvy vlastností v závorkách vždy načítají hello odpovídající vlastnost systému.</span><span class="sxs-lookup"><span data-stu-id="8f50d-176">Bracketed property names always retrieve hello corresponding system property.</span></span>

<span data-ttu-id="8f50d-177">Mějte na paměti, že jsou názvy vlastností malá a velká písmena.</span><span class="sxs-lookup"><span data-stu-id="8f50d-177">Remember that property names are case insensitive.</span></span>

> [!NOTE]
> <span data-ttu-id="8f50d-178">Všechny vlastnosti zprávy jsou řetězce.</span><span class="sxs-lookup"><span data-stu-id="8f50d-178">All message properties are strings.</span></span> <span data-ttu-id="8f50d-179">Vlastnosti systému, jak je popsáno v hello [Příručka vývojáře][lnk-devguide-messaging-format], nejsou aktuálně k dispozici toouse v dotazech.</span><span class="sxs-lookup"><span data-stu-id="8f50d-179">System properties, as described in hello [developer guide][lnk-devguide-messaging-format], are currently not available toouse in queries.</span></span>
>

<span data-ttu-id="8f50d-180">Například, pokud používáte `messageType` vlastnost, můžete chtít tooroute všechny endpoint tooone telemetrie a všechny výstrahy tooanother koncového bodu.</span><span class="sxs-lookup"><span data-stu-id="8f50d-180">For example, if you use a `messageType` property, you might want tooroute all telemetry tooone endpoint, and all alerts tooanother endpoint.</span></span> <span data-ttu-id="8f50d-181">Můžete napsat hello následující výraz tooroute hello telemetrie:</span><span class="sxs-lookup"><span data-stu-id="8f50d-181">You can write hello following expression tooroute hello telemetry:</span></span>

```sql
messageType = 'telemetry'
```

<span data-ttu-id="8f50d-182">A hello následující výraz tooroute hello výstražných zpráv:</span><span class="sxs-lookup"><span data-stu-id="8f50d-182">And hello following expression tooroute hello alert messages:</span></span>

```sql
messageType = 'alert'
```

<span data-ttu-id="8f50d-183">Logické výrazy a funkce jsou také podporovány.</span><span class="sxs-lookup"><span data-stu-id="8f50d-183">Boolean expressions and functions are also supported.</span></span> <span data-ttu-id="8f50d-184">Tato funkce umožňuje toodistinguish mezi úroveň závažnosti, například:</span><span class="sxs-lookup"><span data-stu-id="8f50d-184">This feature enables you toodistinguish between severity level, for example:</span></span>

```sql
messageType = 'alerts' AND as_number(severity) <= 2
```

<span data-ttu-id="8f50d-185">Odkazovat toohello [výraz a podmínky] [ lnk-query-expressions] části hello úplný seznam podporovaných operátory a funkce.</span><span class="sxs-lookup"><span data-stu-id="8f50d-185">Refer toohello [Expression and conditions][lnk-query-expressions] section for hello full list of supported operators and functions.</span></span>

### <a name="routing-on-message-bodies"></a><span data-ttu-id="8f50d-186">Směrování na obsah zpráv</span><span class="sxs-lookup"><span data-stu-id="8f50d-186">Routing on message bodies</span></span>

<span data-ttu-id="8f50d-187">IoT Hub můžete pouze směrování podle tělo zprávy obsah, pokud je text zprávy hello správně vytvořen JSON kódování UTF-8, UTF-16 nebo UTF-32.</span><span class="sxs-lookup"><span data-stu-id="8f50d-187">IoT Hub can only route based on message body contents if hello message body is properly formed JSON encoded in either UTF-8, UTF-16, or UTF-32.</span></span> <span data-ttu-id="8f50d-188">Je nutné nastavit typ obsahu hello hello zprávy příliš`application/json` a hello obsahu kódování tooone hello podporované kódování UTF v tooallow záhlaví zprávy hello IoT Hub tooroute uvítací zprávu podle obsah textu hello.</span><span class="sxs-lookup"><span data-stu-id="8f50d-188">You must set hello content type of hello message too`application/json` and hello content encoding tooone of hello supported UTF encodings in hello message headers tooallow IoT Hub tooroute hello message based on hello body contents.</span></span> <span data-ttu-id="8f50d-189">Pokud není zadán buď hello hlaviček, nepokusí IoT Hub tooevaluate jakýkoli výraz dotazu zahrnující textu hello proti uvítací zprávu.</span><span class="sxs-lookup"><span data-stu-id="8f50d-189">If either of hello headers is not specified, IoT Hub will not attempt tooevaluate any query expression involving hello body against hello message.</span></span> <span data-ttu-id="8f50d-190">Pokud vaše zpráva není zprávu JSON, nebo pokud uvítací zprávu neurčuje hello typu obsahu a kódování obsahu, můžete stále používat směrování podle hlavičky zpráv hello tooroute uvítací zprávu zprávy.</span><span class="sxs-lookup"><span data-stu-id="8f50d-190">If your message is not a JSON message, or if hello message does not specify hello content type and content encoding, you may still use message routing tooroute hello message based on hello message headers.</span></span>

<span data-ttu-id="8f50d-191">Můžete použít `$body` hello dotazu výraz tooroute hello zprávy.</span><span class="sxs-lookup"><span data-stu-id="8f50d-191">You can use `$body` in hello query expression tooroute hello message.</span></span> <span data-ttu-id="8f50d-192">Jednoduchý text odkazu, odkaz na pole text nebo více odkazů text můžete použít ve výrazu dotazu hello.</span><span class="sxs-lookup"><span data-stu-id="8f50d-192">You can use a simple body reference, body array reference, or multiple body references in hello query expression.</span></span> <span data-ttu-id="8f50d-193">Výraz dotazu můžete také kombinovat textu odkaz s odkazem na záhlaví zprávy.</span><span class="sxs-lookup"><span data-stu-id="8f50d-193">Your query expression can also combine a body reference with a message header reference.</span></span> <span data-ttu-id="8f50d-194">Například následující hello jsou všechny výrazy platný dotaz:</span><span class="sxs-lookup"><span data-stu-id="8f50d-194">For example, hello following are all valid query expressions:</span></span>

```sql
$body.message.Weather.Location.State = 'WA'
$body.Weather.HistoricalData[0].Month = 'Feb'
$body.Weather.Temperature = 50 AND $body.message.Weather.IsEnabled
length($body.Weather.Location.State) = 2
$body.Weather.Temperature = 50 AND Status = 'Active'
```

## <a name="basics-of-an-iot-hub-query"></a><span data-ttu-id="8f50d-195">Základní informace o dotaz služby IoT Hub</span><span class="sxs-lookup"><span data-stu-id="8f50d-195">Basics of an IoT Hub query</span></span>
<span data-ttu-id="8f50d-196">Každé centrum IoT dotaz sestává s výběrem a klauzulí FROM a volitelné WHERE a GROUP BY – klauzule.</span><span class="sxs-lookup"><span data-stu-id="8f50d-196">Every IoT Hub query consists of a SELECT and FROM clauses and by optional WHERE and GROUP BY clauses.</span></span> <span data-ttu-id="8f50d-197">Každý dotaz je spuštěn na kolekci dokumentů JSON, například dvojčata zařízení.</span><span class="sxs-lookup"><span data-stu-id="8f50d-197">Every query is run on a collection of JSON documents, for example device twins.</span></span> <span data-ttu-id="8f50d-198">klauzule FROM Hello označuje hello dokumentu kolekce toobe vstupní na (**zařízení** nebo **devices.jobs**).</span><span class="sxs-lookup"><span data-stu-id="8f50d-198">hello FROM clause indicates hello document collection toobe iterated on (**devices** or **devices.jobs**).</span></span> <span data-ttu-id="8f50d-199">Potom hello filtru v hello v případě použití klauzule.</span><span class="sxs-lookup"><span data-stu-id="8f50d-199">Then, hello filter in hello WHERE clause is applied.</span></span> <span data-ttu-id="8f50d-200">S agregace, hello výsledky tohoto kroku seskupeny jako hello zadaný v klauzuli GROUP BY, a pro každou skupinu, je generována řádek uvedené v klauzuli SELECT hello.</span><span class="sxs-lookup"><span data-stu-id="8f50d-200">With aggregations, hello results of this step are grouped as specified in hello GROUP BY clause and, for each group, a row is generated as specified in hello SELECT clause.</span></span>

```sql
SELECT <select_list>
FROM <from_specification>
[WHERE <filter_condition>]
[GROUP BY <group_specification>]
```

## <a name="from-clause"></a><span data-ttu-id="8f50d-201">FROM – klauzule</span><span class="sxs-lookup"><span data-stu-id="8f50d-201">FROM clause</span></span>
<span data-ttu-id="8f50d-202">Hello **z < from_specification >** klauzule můžete předpokládat pouze dvě hodnoty: **ze zařízení**, tooquery dvojčata zařízení, nebo **z devices.jobs**, tooquery úlohy na zařízení Podrobnosti.</span><span class="sxs-lookup"><span data-stu-id="8f50d-202">hello **FROM <from_specification>** clause can assume only two values: **FROM devices**, tooquery device twins, or **FROM devices.jobs**, tooquery job per-device details.</span></span>

## <a name="where-clause"></a><span data-ttu-id="8f50d-203">Klauzule WHERE</span><span class="sxs-lookup"><span data-stu-id="8f50d-203">WHERE clause</span></span>
<span data-ttu-id="8f50d-204">Hello **kde < filter_condition >** klauzule je volitelný.</span><span class="sxs-lookup"><span data-stu-id="8f50d-204">hello **WHERE <filter_condition>** clause is optional.</span></span> <span data-ttu-id="8f50d-205">Určuje jeden nebo více podmínky, že dokumenty JSON hello v kolekci FROM hello musí splňovat toobe jako součást výsledků hello.</span><span class="sxs-lookup"><span data-stu-id="8f50d-205">It specifies one or more conditions that hello JSON documents in hello FROM collection must satisfy toobe included as part of hello result.</span></span> <span data-ttu-id="8f50d-206">Dokumentu JSON se musí vyhodnotit hello zadané podmínky příliš "true" toobe součástí hello výsledku.</span><span class="sxs-lookup"><span data-stu-id="8f50d-206">Any JSON document must evaluate hello specified conditions too"true" toobe included in hello result.</span></span>

<span data-ttu-id="8f50d-207">Hello povolené podmínky jsou popsány v části [výrazy a podmínky][lnk-query-expressions].</span><span class="sxs-lookup"><span data-stu-id="8f50d-207">hello allowed conditions are described in section [Expressions and conditions][lnk-query-expressions].</span></span>

## <a name="select-clause"></a><span data-ttu-id="8f50d-208">Klauzule SELECT</span><span class="sxs-lookup"><span data-stu-id="8f50d-208">SELECT clause</span></span>
<span data-ttu-id="8f50d-209">Klauzule SELECT Hello (**vyberte < select_list >**) je povinná a určuje, jaké hodnoty jsou načteny z dotazu hello.</span><span class="sxs-lookup"><span data-stu-id="8f50d-209">hello SELECT clause (**SELECT <select_list>**) is mandatory and specifies what values are retrieved from hello query.</span></span> <span data-ttu-id="8f50d-210">Určuje, že toobe hodnoty JSON hello používá toogenerate nové objekty JSON.</span><span class="sxs-lookup"><span data-stu-id="8f50d-210">It specifies hello JSON values toobe used toogenerate new JSON objects.</span></span>
<span data-ttu-id="8f50d-211">Pro každý element hello filtrované (a volitelně seskupené) podmnožinu hello FROM kolekce, fáze projekce hello vygeneruje nový objekt JSON, zkonstruovat s hello hodnot zadaných v klauzuli SELECT hello.</span><span class="sxs-lookup"><span data-stu-id="8f50d-211">For each element of hello filtered (and optionally grouped) subset of hello FROM collection, hello projection phase generates a new JSON object, constructed with hello values specified in hello SELECT clause.</span></span>

<span data-ttu-id="8f50d-212">Toto je hello gramatika vyberte klauzule hello:</span><span class="sxs-lookup"><span data-stu-id="8f50d-212">Following is hello grammar of hello SELECT clause:</span></span>

```
SELECT [TOP <max number>] <projection list>

<projection_list> ::=
    '*'
    | <projection_element> AS alias [, <projection_element> AS alias]+

<projection_element> :==
    attribute_name
    | <projection_element> '.' attribute_name
    | <aggregate>

<aggregate> :==
    count()
    | avg(<projection_element>)
    | sum(<projection_element>)
    | min(<projection_element>)
    | max(<projection_element>)
```

<span data-ttu-id="8f50d-213">kde **attribute_name** odkazuje vlastnost tooany hello dokumentu JSON v kolekci FROM hello.</span><span class="sxs-lookup"><span data-stu-id="8f50d-213">where **attribute_name** refers tooany property of hello JSON document in hello FROM collection.</span></span> <span data-ttu-id="8f50d-214">Některé příklady klauzule FROM lze nalézt v hello [Začínáme s dotazy twin zařízení] [ lnk-query-getstarted] části.</span><span class="sxs-lookup"><span data-stu-id="8f50d-214">Some examples of SELECT clauses can be found in hello [Getting started with device twin queries][lnk-query-getstarted] section.</span></span>

<span data-ttu-id="8f50d-215">V současné době výběr klauzule liší od **vyberte \***  jsou podporovány pouze v agregační dotazy na dvojčata zařízení.</span><span class="sxs-lookup"><span data-stu-id="8f50d-215">Currently, selection clauses different than **SELECT \*** are only supported in aggregate queries on device twins.</span></span>

## <a name="group-by-clause"></a><span data-ttu-id="8f50d-216">klauzule GROUP BY</span><span class="sxs-lookup"><span data-stu-id="8f50d-216">GROUP BY clause</span></span>
<span data-ttu-id="8f50d-217">Hello **GROUP BY < group_specification >** klauzule je volitelný krok, který lze provést po hello filtr zadaný v hello klauzule WHERE a před hello projekce určená v hello vyberte.</span><span class="sxs-lookup"><span data-stu-id="8f50d-217">hello **GROUP BY <group_specification>** clause is an optional step that can be executed after hello filter specified in hello WHERE clause, and before hello projection specified in hello SELECT.</span></span> <span data-ttu-id="8f50d-218">Seskupuje dokumentů podle hello hodnotu atributu.</span><span class="sxs-lookup"><span data-stu-id="8f50d-218">It groups documents based on hello value of an attribute.</span></span> <span data-ttu-id="8f50d-219">Tyto skupiny jsou použité toogenerate agregovat hodnoty zadané v klauzuli SELECT hello.</span><span class="sxs-lookup"><span data-stu-id="8f50d-219">These groups are used toogenerate aggregated values as specified in hello SELECT clause.</span></span>

<span data-ttu-id="8f50d-220">Příklad dotazu pomocí skupiny je:</span><span class="sxs-lookup"><span data-stu-id="8f50d-220">An example of a query using GROUP BY is:</span></span>

```sql
SELECT properties.reported.telemetryConfig.status AS status,
    COUNT() AS numberOfDevices
FROM devices
GROUP BY properties.reported.telemetryConfig.status
```

<span data-ttu-id="8f50d-221">Tady je syntax formální Hello Seskupit podle:</span><span class="sxs-lookup"><span data-stu-id="8f50d-221">hello formal syntax for GROUP BY is:</span></span>

```
GROUP BY <group_by_element>
<group_by_element> :==
    attribute_name
    | < group_by_element > '.' attribute_name
```

<span data-ttu-id="8f50d-222">kde **attribute_name** odkazuje vlastnost tooany hello dokumentu JSON v kolekci FROM hello.</span><span class="sxs-lookup"><span data-stu-id="8f50d-222">where **attribute_name** refers tooany property of hello JSON document in hello FROM collection.</span></span>

<span data-ttu-id="8f50d-223">Klauzule GROUP BY hello je v současné době podporována pouze při dotazování dvojčata zařízení.</span><span class="sxs-lookup"><span data-stu-id="8f50d-223">Currently, hello GROUP BY clause is only supported when querying device twins.</span></span>

## <a name="expressions-and-conditions"></a><span data-ttu-id="8f50d-224">Výrazy a podmínky</span><span class="sxs-lookup"><span data-stu-id="8f50d-224">Expressions and conditions</span></span>
<span data-ttu-id="8f50d-225">Na vysoké úrovni *výraz*:</span><span class="sxs-lookup"><span data-stu-id="8f50d-225">At a high level, an *expression*:</span></span>

* <span data-ttu-id="8f50d-226">Vyhodnotí tooan instanci typu JSON (například logická hodnota, čísla, řetězce, pole nebo objekt), a</span><span class="sxs-lookup"><span data-stu-id="8f50d-226">Evaluates tooan instance of a JSON type (such as Boolean, number, string, array, or object), and</span></span>
* <span data-ttu-id="8f50d-227">Je definován manipulace s daty z dokumentu JSON hello zařízení a konstant pomocí předdefinovaných operátory a funkce.</span><span class="sxs-lookup"><span data-stu-id="8f50d-227">Is defined by manipulating data coming from hello device JSON document and constants using built-in operators and functions.</span></span>

<span data-ttu-id="8f50d-228">*Podmínky* jsou výrazy, která se vyhodnotí tooa logická hodnota.</span><span class="sxs-lookup"><span data-stu-id="8f50d-228">*Conditions* are expressions that evaluate tooa Boolean.</span></span> <span data-ttu-id="8f50d-229">Žádné jiné než logická hodnota konstanta **true** se považuje za **false** (včetně **null**, **nedefinované**, objekt nebo pole instance, všechny řetězce a jasně hello logická hodnota **false**).</span><span class="sxs-lookup"><span data-stu-id="8f50d-229">Any constant different than Boolean **true** is considered as **false** (including **null**, **undefined**, any object or array instance, any string, and clearly hello Boolean **false**).</span></span>

<span data-ttu-id="8f50d-230">Hello syntaxe pro výrazy je:</span><span class="sxs-lookup"><span data-stu-id="8f50d-230">hello syntax for expressions is:</span></span>

```
<expression> ::=
    <constant> |
    attribute_name |
    <function_call> |
    <expression> binary_operator <expression> |
    <create_array_expression> |
    '(' <expression> ')'

<function_call> ::=
    <function_name> '(' expression ')'

<constant> ::=
    <undefined_constant>
    | <null_constant>
    | <number_constant>
    | <string_constant>
    | <array_constant>

<undefined_constant> ::= undefined
<null_constant> ::= null
<number_constant> ::= decimal_literal | hexadecimal_literal
<string_constant> ::= string_literal
<array_constant> ::= '[' <constant> [, <constant>]+ ']'
```

<span data-ttu-id="8f50d-231">Kde:</span><span class="sxs-lookup"><span data-stu-id="8f50d-231">where:</span></span>

| <span data-ttu-id="8f50d-232">Symbol</span><span class="sxs-lookup"><span data-stu-id="8f50d-232">Symbol</span></span> | <span data-ttu-id="8f50d-233">Definice</span><span class="sxs-lookup"><span data-stu-id="8f50d-233">Definition</span></span> |
| --- | --- |
| <span data-ttu-id="8f50d-234">attribute_name</span><span class="sxs-lookup"><span data-stu-id="8f50d-234">attribute_name</span></span> | <span data-ttu-id="8f50d-235">Vlastnost hello dokumentu JSON v hello **FROM** kolekce.</span><span class="sxs-lookup"><span data-stu-id="8f50d-235">Any property of hello JSON document in hello **FROM** collection.</span></span> |
| <span data-ttu-id="8f50d-236">binary_operator</span><span class="sxs-lookup"><span data-stu-id="8f50d-236">binary_operator</span></span> | <span data-ttu-id="8f50d-237">Všechny binární operátor uvedené v hello [operátory](#operators) části.</span><span class="sxs-lookup"><span data-stu-id="8f50d-237">Any binary operator listed in hello [Operators](#operators) section.</span></span> |
| <span data-ttu-id="8f50d-238">Název_funkce</span><span class="sxs-lookup"><span data-stu-id="8f50d-238">function_name</span></span>| <span data-ttu-id="8f50d-239">Všechny funkce uvedené v hello [funkce](#functions) části.</span><span class="sxs-lookup"><span data-stu-id="8f50d-239">Any function listed in hello [Functions](#functions) section.</span></span> |
| <span data-ttu-id="8f50d-240">decimal_literal</span><span class="sxs-lookup"><span data-stu-id="8f50d-240">decimal_literal</span></span> |<span data-ttu-id="8f50d-241">Float, vyjádřené v desítkovém zápisu.</span><span class="sxs-lookup"><span data-stu-id="8f50d-241">A float expressed in decimal notation.</span></span> |
| <span data-ttu-id="8f50d-242">hexadecimal_literal</span><span class="sxs-lookup"><span data-stu-id="8f50d-242">hexadecimal_literal</span></span> |<span data-ttu-id="8f50d-243">Číslo vyjádřená hello řetězec '0 x, za nímž následuje řetězec šestnáctkových číslic.</span><span class="sxs-lookup"><span data-stu-id="8f50d-243">A number expressed by hello string ‘0x’ followed by a string of hexadecimal digits.</span></span> |
| <span data-ttu-id="8f50d-244">string_literal</span><span class="sxs-lookup"><span data-stu-id="8f50d-244">string_literal</span></span> |<span data-ttu-id="8f50d-245">Textové literály jsou reprezentována posloupnost nula nebo více znaků Unicode nebo řídicí sekvence řetězců v kódu Unicode.</span><span class="sxs-lookup"><span data-stu-id="8f50d-245">String literals are Unicode strings represented by a sequence of zero or more Unicode characters or escape sequences.</span></span> <span data-ttu-id="8f50d-246">Textové literály jsou uzavřená do jednoduchých uvozovek (apostrof: ') nebo dvojité uvozovky (uvozovky: ").</span><span class="sxs-lookup"><span data-stu-id="8f50d-246">String literals are enclosed in single quotes (apostrophe: ' ) or double quotes (quotation mark: ").</span></span> <span data-ttu-id="8f50d-247">Povolené řídicí sekvence: `\'`, `\"`, `\\`, `\uXXXX` pro znaky znakové sady Unicode, které jsou definované 4 hexadecimální číslice.</span><span class="sxs-lookup"><span data-stu-id="8f50d-247">Allowed escapes: `\'`, `\"`, `\\`, `\uXXXX` for Unicode characters defined by 4 hexadecimal digits.</span></span> |

### <a name="operators"></a><span data-ttu-id="8f50d-248">Operátory</span><span class="sxs-lookup"><span data-stu-id="8f50d-248">Operators</span></span>
<span data-ttu-id="8f50d-249">jsou podporovány Hello následující operátory:</span><span class="sxs-lookup"><span data-stu-id="8f50d-249">hello following operators are supported:</span></span>

| <span data-ttu-id="8f50d-250">Rodina</span><span class="sxs-lookup"><span data-stu-id="8f50d-250">Family</span></span> | <span data-ttu-id="8f50d-251">Operátory</span><span class="sxs-lookup"><span data-stu-id="8f50d-251">Operators</span></span> |
| --- | --- |
| <span data-ttu-id="8f50d-252">Aritmetické operace</span><span class="sxs-lookup"><span data-stu-id="8f50d-252">Arithmetic</span></span> |<span data-ttu-id="8f50d-253">+,-,*,/,%</span><span class="sxs-lookup"><span data-stu-id="8f50d-253">+,-,*,/,%</span></span> |
| <span data-ttu-id="8f50d-254">Logické</span><span class="sxs-lookup"><span data-stu-id="8f50d-254">Logical</span></span> |<span data-ttu-id="8f50d-255">A, NEBO NE</span><span class="sxs-lookup"><span data-stu-id="8f50d-255">AND, OR, NOT</span></span> |
| <span data-ttu-id="8f50d-256">Porovnání</span><span class="sxs-lookup"><span data-stu-id="8f50d-256">Comparison</span></span> |<span data-ttu-id="8f50d-257">=, !=, <, >, <=, >=, <></span><span class="sxs-lookup"><span data-stu-id="8f50d-257">=, !=, <, >, <=, >=, <></span></span> |

### <a name="functions"></a><span data-ttu-id="8f50d-258">Funkce</span><span class="sxs-lookup"><span data-stu-id="8f50d-258">Functions</span></span>
<span data-ttu-id="8f50d-259">Při dotazování úlohy a dvojčata hello podporovány pouze funkce je:</span><span class="sxs-lookup"><span data-stu-id="8f50d-259">When querying twins and jobs hello only supported function is:</span></span>

| <span data-ttu-id="8f50d-260">Funkce</span><span class="sxs-lookup"><span data-stu-id="8f50d-260">Function</span></span> | <span data-ttu-id="8f50d-261">Popis</span><span class="sxs-lookup"><span data-stu-id="8f50d-261">Description</span></span> |
| -------- | ----------- |
| <span data-ttu-id="8f50d-262">IS_DEFINED(Property)</span><span class="sxs-lookup"><span data-stu-id="8f50d-262">IS_DEFINED(property)</span></span> | <span data-ttu-id="8f50d-263">Vrátí logickou hodnotu udávající, pokud byla vlastnost hello přiřazenou hodnotu (včetně `null`).</span><span class="sxs-lookup"><span data-stu-id="8f50d-263">Returns a Boolean indicating if hello property has been assigned a value (including `null`).</span></span> |

<span data-ttu-id="8f50d-264">V podmínkách trasy jsou podporovány následující matematické funkce hello:</span><span class="sxs-lookup"><span data-stu-id="8f50d-264">In routes conditions, hello following math functions are supported:</span></span>

| <span data-ttu-id="8f50d-265">Funkce</span><span class="sxs-lookup"><span data-stu-id="8f50d-265">Function</span></span> | <span data-ttu-id="8f50d-266">Popis</span><span class="sxs-lookup"><span data-stu-id="8f50d-266">Description</span></span> |
| -------- | ----------- |
| <span data-ttu-id="8f50d-267">Abs(x)</span><span class="sxs-lookup"><span data-stu-id="8f50d-267">ABS(x)</span></span> | <span data-ttu-id="8f50d-268">Vrátí hello absolutní (kladné) hello zadána hodnota číselného výrazu.</span><span class="sxs-lookup"><span data-stu-id="8f50d-268">Returns hello absolute (positive) value of hello specified numeric expression.</span></span> |
| <span data-ttu-id="8f50d-269">Exp(x)</span><span class="sxs-lookup"><span data-stu-id="8f50d-269">EXP(x)</span></span> | <span data-ttu-id="8f50d-270">Vrátí hodnotu exponenciálního hello Dobrý den zadán číselný výraz (e ^ x).</span><span class="sxs-lookup"><span data-stu-id="8f50d-270">Returns hello exponential value of hello specified numeric expression (e^x).</span></span> |
| <span data-ttu-id="8f50d-271">Power(x,y)</span><span class="sxs-lookup"><span data-stu-id="8f50d-271">POWER(x,y)</span></span> | <span data-ttu-id="8f50d-272">Vrátí hello hodnotu hello zadaný výraz toohello zadaný power (x ^ y).</span><span class="sxs-lookup"><span data-stu-id="8f50d-272">Returns hello value of hello specified expression toohello specified power (x^y).</span></span>|
| <span data-ttu-id="8f50d-273">Square(x)</span><span class="sxs-lookup"><span data-stu-id="8f50d-273">SQUARE(x)</span></span> | <span data-ttu-id="8f50d-274">Vrátí hello odmocnina z hello zadat číselnou hodnotu.</span><span class="sxs-lookup"><span data-stu-id="8f50d-274">Returns hello square of hello specified numeric value.</span></span> |
| <span data-ttu-id="8f50d-275">CEILING(x)</span><span class="sxs-lookup"><span data-stu-id="8f50d-275">CEILING(x)</span></span> | <span data-ttu-id="8f50d-276">Vrátí hello nejmenší celé číslo větší než nebo rovno, hello zadaný číselný výraz.</span><span class="sxs-lookup"><span data-stu-id="8f50d-276">Returns hello smallest integer value greater than, or equal to, hello specified numeric expression.</span></span> |
| <span data-ttu-id="8f50d-277">Floor(x)</span><span class="sxs-lookup"><span data-stu-id="8f50d-277">FLOOR(x)</span></span> | <span data-ttu-id="8f50d-278">Vrátí hello největší celé číslo menší než nebo rovna toohello zadán číselný výraz.</span><span class="sxs-lookup"><span data-stu-id="8f50d-278">Returns hello largest integer less than or equal toohello specified numeric expression.</span></span> |
| <span data-ttu-id="8f50d-279">Sign(x)</span><span class="sxs-lookup"><span data-stu-id="8f50d-279">SIGN(x)</span></span> | <span data-ttu-id="8f50d-280">Vrátí hello kladnou (+ 1), nula (0), nebo záporné znaménko (-1) hello zadán číselný výraz.</span><span class="sxs-lookup"><span data-stu-id="8f50d-280">Returns hello positive (+1), zero (0), or negative (-1) sign of hello specified numeric expression.</span></span>|
| <span data-ttu-id="8f50d-281">Sqrt(x)</span><span class="sxs-lookup"><span data-stu-id="8f50d-281">SQRT(x)</span></span> | <span data-ttu-id="8f50d-282">Vrátí hello odmocnina z hello zadat číselnou hodnotu.</span><span class="sxs-lookup"><span data-stu-id="8f50d-282">Returns hello square of hello specified numeric value.</span></span> |

<span data-ttu-id="8f50d-283">V podmínkách trasy jsou podporovány následující typ kontrolu a přetypování funkce hello:</span><span class="sxs-lookup"><span data-stu-id="8f50d-283">In routes conditions, hello following type checking and casting functions are supported:</span></span>

| <span data-ttu-id="8f50d-284">Funkce</span><span class="sxs-lookup"><span data-stu-id="8f50d-284">Function</span></span> | <span data-ttu-id="8f50d-285">Popis</span><span class="sxs-lookup"><span data-stu-id="8f50d-285">Description</span></span> |
| -------- | ----------- |
| <span data-ttu-id="8f50d-286">AS_NUMBER</span><span class="sxs-lookup"><span data-stu-id="8f50d-286">AS_NUMBER</span></span> | <span data-ttu-id="8f50d-287">Převede hello vstupní řetězec tooa číslo.</span><span class="sxs-lookup"><span data-stu-id="8f50d-287">Converts hello input string tooa number.</span></span> <span data-ttu-id="8f50d-288">`noop`Pokud vstup je číslo; `Undefined` Pokud řetězec nepředstavuje číslo.</span><span class="sxs-lookup"><span data-stu-id="8f50d-288">`noop` if input is a number; `Undefined` if string does not represent a number.</span></span>|
| <span data-ttu-id="8f50d-289">IS_ARRAY</span><span class="sxs-lookup"><span data-stu-id="8f50d-289">IS_ARRAY</span></span> | <span data-ttu-id="8f50d-290">Vrátí logickou hodnotu udávající, pokud typ hello hello zadaný výraz je pole.</span><span class="sxs-lookup"><span data-stu-id="8f50d-290">Returns a Boolean value indicating if hello type of hello specified expression is an array.</span></span> |
| <span data-ttu-id="8f50d-291">IS_BOOL</span><span class="sxs-lookup"><span data-stu-id="8f50d-291">IS_BOOL</span></span> | <span data-ttu-id="8f50d-292">Vrátí logickou hodnotu udávající, pokud typ hello hello zadaný výraz je logická hodnota.</span><span class="sxs-lookup"><span data-stu-id="8f50d-292">Returns a Boolean value indicating if hello type of hello specified expression is a Boolean.</span></span> |
| <span data-ttu-id="8f50d-293">IS_DEFINED</span><span class="sxs-lookup"><span data-stu-id="8f50d-293">IS_DEFINED</span></span> | <span data-ttu-id="8f50d-294">Vrátí logickou hodnotu udávající, pokud byla vlastnost hello přiřazena hodnota.</span><span class="sxs-lookup"><span data-stu-id="8f50d-294">Returns a Boolean indicating if hello property has been assigned a value.</span></span> |
| <span data-ttu-id="8f50d-295">IS_NULL</span><span class="sxs-lookup"><span data-stu-id="8f50d-295">IS_NULL</span></span> | <span data-ttu-id="8f50d-296">Vrátí logickou hodnotu udávající, pokud zadaný typ hello hello výraz má hodnotu null.</span><span class="sxs-lookup"><span data-stu-id="8f50d-296">Returns a Boolean value indicating if hello type of hello specified expression is null.</span></span> |
| <span data-ttu-id="8f50d-297">IS_NUMBER</span><span class="sxs-lookup"><span data-stu-id="8f50d-297">IS_NUMBER</span></span> | <span data-ttu-id="8f50d-298">Vrátí logickou hodnotu udávající, pokud typ hello hello zadaný výraz je číslo.</span><span class="sxs-lookup"><span data-stu-id="8f50d-298">Returns a Boolean value indicating if hello type of hello specified expression is a number.</span></span> |
| <span data-ttu-id="8f50d-299">IS_OBJECT</span><span class="sxs-lookup"><span data-stu-id="8f50d-299">IS_OBJECT</span></span> | <span data-ttu-id="8f50d-300">Vrátí logickou hodnotu udávající, pokud typ hello hello zadaný výraz je objekt JSON.</span><span class="sxs-lookup"><span data-stu-id="8f50d-300">Returns a Boolean value indicating if hello type of hello specified expression is a JSON object.</span></span> |
| <span data-ttu-id="8f50d-301">IS_PRIMITIVE</span><span class="sxs-lookup"><span data-stu-id="8f50d-301">IS_PRIMITIVE</span></span> | <span data-ttu-id="8f50d-302">Vrátí logickou hodnotu udávající, pokud typ hello hello zadaný výraz je primitivní (řetězec, logická hodnota, číselná nebo `null`).</span><span class="sxs-lookup"><span data-stu-id="8f50d-302">Returns a Boolean value indicating if hello type of hello specified expression is a primitive (string, Boolean, numeric or `null`).</span></span> |
| <span data-ttu-id="8f50d-303">IS_STRING</span><span class="sxs-lookup"><span data-stu-id="8f50d-303">IS_STRING</span></span> | <span data-ttu-id="8f50d-304">Vrátí logickou hodnotu udávající, pokud typ hello hello zadaný výraz obsahuje řetězec.</span><span class="sxs-lookup"><span data-stu-id="8f50d-304">Returns a Boolean value indicating if hello type of hello specified expression is a string.</span></span> |

<span data-ttu-id="8f50d-305">V podmínkách trasy jsou podporovány následující funkce řetězce hello:</span><span class="sxs-lookup"><span data-stu-id="8f50d-305">In routes conditions, hello following string functions are supported:</span></span>

| <span data-ttu-id="8f50d-306">Funkce</span><span class="sxs-lookup"><span data-stu-id="8f50d-306">Function</span></span> | <span data-ttu-id="8f50d-307">Popis</span><span class="sxs-lookup"><span data-stu-id="8f50d-307">Description</span></span> |
| -------- | ----------- |
| <span data-ttu-id="8f50d-308">CONCAT(x,...)</span><span class="sxs-lookup"><span data-stu-id="8f50d-308">CONCAT(x, …)</span></span> | <span data-ttu-id="8f50d-309">Vrátí řetězec, který je výsledkem hello zřetězení dvou nebo více řetězcové hodnoty.</span><span class="sxs-lookup"><span data-stu-id="8f50d-309">Returns a string that is hello result of concatenating two or more string values.</span></span> |
| <span data-ttu-id="8f50d-310">Length(x)</span><span class="sxs-lookup"><span data-stu-id="8f50d-310">LENGTH(x)</span></span> | <span data-ttu-id="8f50d-311">Vrátí hello počet znaků, který hello zadán řetězcovým výrazem.</span><span class="sxs-lookup"><span data-stu-id="8f50d-311">Returns hello number of characters of hello specified string expression.</span></span>|
| <span data-ttu-id="8f50d-312">LOWER(x)</span><span class="sxs-lookup"><span data-stu-id="8f50d-312">LOWER(x)</span></span> | <span data-ttu-id="8f50d-313">Vrací výraz řetězce po převodu dat toolowercase velké písmeno.</span><span class="sxs-lookup"><span data-stu-id="8f50d-313">Returns a string expression after converting uppercase character data toolowercase.</span></span> |
| <span data-ttu-id="8f50d-314">UPPER(x)</span><span class="sxs-lookup"><span data-stu-id="8f50d-314">UPPER(x)</span></span> | <span data-ttu-id="8f50d-315">Vrací výraz řetězce po převodu dat toouppercase malé písmeno.</span><span class="sxs-lookup"><span data-stu-id="8f50d-315">Returns a string expression after converting lowercase character data toouppercase.</span></span> |
| <span data-ttu-id="8f50d-316">Dílčí řetězec (string, spuštění [, délka])</span><span class="sxs-lookup"><span data-stu-id="8f50d-316">SUBSTRING(string, start [, length])</span></span> | <span data-ttu-id="8f50d-317">Vrátí část výrazu řetězce začínající hello zadaný znak pozice s nulovým základem a pokračuje v toohello Zadaná délka nebo toohello konce řetězce hello.</span><span class="sxs-lookup"><span data-stu-id="8f50d-317">Returns part of a string expression starting at hello specified character zero-based position and continues toohello specified length, or toohello end of hello string.</span></span> |
| <span data-ttu-id="8f50d-318">INDEX_OF (řetězec, fragment)</span><span class="sxs-lookup"><span data-stu-id="8f50d-318">INDEX_OF(string, fragment)</span></span> | <span data-ttu-id="8f50d-319">Vrátí počáteční pozici prvního výskytu hello hello druhý řetězcového výrazu v rámci zadaného řetězcového výrazu první hello nebo -1, pokud není nalezen řetězec hello hello.</span><span class="sxs-lookup"><span data-stu-id="8f50d-319">Returns hello starting position of hello first occurrence of hello second string expression within hello first specified string expression, or -1 if hello string is not found.</span></span>|
| <span data-ttu-id="8f50d-320">STARTS_WITH (x, y)</span><span class="sxs-lookup"><span data-stu-id="8f50d-320">STARTS_WITH(x, y)</span></span> | <span data-ttu-id="8f50d-321">Vrátí logickou hodnotu udávající, zda text hello první řetězcového výrazu začíná hello druhý.</span><span class="sxs-lookup"><span data-stu-id="8f50d-321">Returns a Boolean indicating whether hello first string expression starts with hello second.</span></span> |
| <span data-ttu-id="8f50d-322">ENDS_WITH (x, y)</span><span class="sxs-lookup"><span data-stu-id="8f50d-322">ENDS_WITH(x, y)</span></span> | <span data-ttu-id="8f50d-323">Vrátí logickou hodnotu udávající, zda text hello první řetězcového výrazu končí hello druhý.</span><span class="sxs-lookup"><span data-stu-id="8f50d-323">Returns a Boolean indicating whether hello first string expression ends with hello second.</span></span> |
| <span data-ttu-id="8f50d-324">CONTAINS(x,y)</span><span class="sxs-lookup"><span data-stu-id="8f50d-324">CONTAINS(x,y)</span></span> | <span data-ttu-id="8f50d-325">Vrátí logickou hodnotu udávající, zda text hello první řetězcového výrazu obsahuje hello druhý.</span><span class="sxs-lookup"><span data-stu-id="8f50d-325">Returns a Boolean indicating whether hello first string expression contains hello second.</span></span> |

## <a name="next-steps"></a><span data-ttu-id="8f50d-326">Další kroky</span><span class="sxs-lookup"><span data-stu-id="8f50d-326">Next steps</span></span>
<span data-ttu-id="8f50d-327">Zjistěte, jak tooexecute dotazuje ve svých aplikacích pomocí [SDK služby Azure IoT][lnk-hub-sdks].</span><span class="sxs-lookup"><span data-stu-id="8f50d-327">Learn how tooexecute queries in your apps using [Azure IoT SDKs][lnk-hub-sdks].</span></span>

[lnk-query-where]: iot-hub-devguide-query-language.md#where-clause
[lnk-query-expressions]: iot-hub-devguide-query-language.md#expressions-and-conditions
[lnk-query-getstarted]: iot-hub-devguide-query-language.md#get-started-with-device-twin-queries

[lnk-twins]: iot-hub-devguide-device-twins.md
[lnk-jobs]: iot-hub-devguide-jobs.md
[lnk-devguide-endpoints]: iot-hub-devguide-endpoints.md
[lnk-devguide-quotas]: iot-hub-devguide-quotas-throttling.md
[lnk-devguide-mqtt]: iot-hub-mqtt-support.md
[lnk-devguide-messaging-routes]: iot-hub-devguide-messages-read-custom.md
[lnk-devguide-messaging-format]: iot-hub-devguide-messages-construct.md
[lnk-devguide-messaging-routes]: ./iot-hub-devguide-messages-read-custom.md

[lnk-hub-sdks]: iot-hub-devguide-sdks.md
