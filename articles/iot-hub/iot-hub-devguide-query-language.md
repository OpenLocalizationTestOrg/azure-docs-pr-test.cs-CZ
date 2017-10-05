---
title: "Pochopení dotazovací jazyk Azure IoT Hub | Microsoft Docs"
description: "Příručka vývojáře – popis dotazu jazyka SQL jako IoT Hub používá k načtení informací o úlohách a dvojčata zařízení ze služby IoT hub."
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
ms.openlocfilehash: a7650104eda58923558892f6f0f6666d16dbce28
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="reference---iot-hub-query-language-for-device-twins-jobs-and-message-routing"></a><span data-ttu-id="ec9e6-103">Referenční dokumentace - IoT Hub dotazovacího jazyka pro dvojčata zařízení, úlohy a směrování zpráv</span><span class="sxs-lookup"><span data-stu-id="ec9e6-103">Reference - IoT Hub query language for device twins, jobs, and message routing</span></span>

<span data-ttu-id="ec9e6-104">Služba IoT Hub zajišťuje efektivní jazyka SQL jako k načtení informací o [dvojčata zařízení] [ lnk-twins] a [úlohy][lnk-jobs], a [směrování zpráv][lnk-devguide-messaging-routes].</span><span class="sxs-lookup"><span data-stu-id="ec9e6-104">IoT Hub provides a powerful SQL-like language to retrieve information regarding [device twins][lnk-twins] and [jobs][lnk-jobs], and [message routing][lnk-devguide-messaging-routes].</span></span> <span data-ttu-id="ec9e6-105">Tento článek představuje:</span><span class="sxs-lookup"><span data-stu-id="ec9e6-105">This article presents:</span></span>

* <span data-ttu-id="ec9e6-106">Úvod do hlavní funkce jazyka dotazů služby IoT Hub, a</span><span class="sxs-lookup"><span data-stu-id="ec9e6-106">An introduction to the major features of the IoT Hub query language, and</span></span>
* <span data-ttu-id="ec9e6-107">Podrobný popis jazyka.</span><span class="sxs-lookup"><span data-stu-id="ec9e6-107">The detailed description of the language.</span></span>

## <a name="get-started-with-device-twin-queries"></a><span data-ttu-id="ec9e6-108">Začínáme s dotazy twin zařízení</span><span class="sxs-lookup"><span data-stu-id="ec9e6-108">Get started with device twin queries</span></span>
<span data-ttu-id="ec9e6-109">[Dvojčata zařízení] [ lnk-twins] může obsahovat libovolné objekty JSON vlastnosti a značky.</span><span class="sxs-lookup"><span data-stu-id="ec9e6-109">[Device twins][lnk-twins] can contain arbitrary JSON objects as both tags and properties.</span></span> <span data-ttu-id="ec9e6-110">IoT Hub umožňuje dvojčata zařízení dotaz jako jeden dokument JSON obsahující všechny informace o dvojici zařízení.</span><span class="sxs-lookup"><span data-stu-id="ec9e6-110">IoT Hub enables you to query device twins as a single JSON document containing all device twin information.</span></span>
<span data-ttu-id="ec9e6-111">Předpokládejme například, že vaše dvojčata zařízení IoT hub mít následující strukturu:</span><span class="sxs-lookup"><span data-stu-id="ec9e6-111">Assume, for instance, that your IoT hub device twins have the following structure:</span></span>

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

<span data-ttu-id="ec9e6-112">IoT Hub zpřístupní dvojčata zařízení jako kolekce dokumentů s názvem **zařízení**.</span><span class="sxs-lookup"><span data-stu-id="ec9e6-112">IoT Hub exposes the device twins as a document collection called **devices**.</span></span>
<span data-ttu-id="ec9e6-113">Následující dotaz načte tak celou sadu dvojčata zařízení:</span><span class="sxs-lookup"><span data-stu-id="ec9e6-113">So the following query retrieves the whole set of device twins:</span></span>

```sql
SELECT * FROM devices
```

> [!NOTE]
> <span data-ttu-id="ec9e6-114">[Sady SDK služby Azure IoT] [ lnk-hub-sdks] podporu stránkování výsledků velký.</span><span class="sxs-lookup"><span data-stu-id="ec9e6-114">[Azure IoT SDKs][lnk-hub-sdks] support paging of large results.</span></span>

<span data-ttu-id="ec9e6-115">IoT Hub můžete načíst dvojčata zařízení filtrování pomocí libovolné podmínky.</span><span class="sxs-lookup"><span data-stu-id="ec9e6-115">IoT Hub allows you to retrieve device twins filtering with arbitrary conditions.</span></span> <span data-ttu-id="ec9e6-116">Pro instanci,</span><span class="sxs-lookup"><span data-stu-id="ec9e6-116">For instance,</span></span>

```sql
SELECT * FROM devices
WHERE tags.location.region = 'US'
```

<span data-ttu-id="ec9e6-117">načte dvojčata zařízení pomocí **location.region** značky nastavit na **USA**.</span><span class="sxs-lookup"><span data-stu-id="ec9e6-117">retrieves the device twins with the **location.region** tag set to **US**.</span></span>
<span data-ttu-id="ec9e6-118">Logické operátory a aritmetické porovnání jsou podporovány také, například</span><span class="sxs-lookup"><span data-stu-id="ec9e6-118">Boolean operators and arithmetic comparisons are supported as well, for example</span></span>

```sql
SELECT * FROM devices
WHERE tags.location.region = 'US'
    AND properties.reported.telemetryConfig.sendFrequencyInSecs >= 60
```

<span data-ttu-id="ec9e6-119">načte všechny dvojčata zařízení nachází v USA, konfigurované k odesílání telemetrie méně často než každou minutu.</span><span class="sxs-lookup"><span data-stu-id="ec9e6-119">retrieves all device twins located in the US configured to send telemetry less often than every minute.</span></span> <span data-ttu-id="ec9e6-120">Pro potřeby je také možné použít konstanty pole s **IN** a **NV** operátory (ne v).</span><span class="sxs-lookup"><span data-stu-id="ec9e6-120">As a convenience, it is also possible to use array constants with the **IN** and **NIN** (not in) operators.</span></span> <span data-ttu-id="ec9e6-121">Pro instanci,</span><span class="sxs-lookup"><span data-stu-id="ec9e6-121">For instance,</span></span>

```sql
SELECT * FROM devices
WHERE properties.reported.connectivity IN ['wired', 'wifi']
```

<span data-ttu-id="ec9e6-122">načte všechny dvojčata zařízení, které ohlásil Wi-Fi nebo drátové připojení.</span><span class="sxs-lookup"><span data-stu-id="ec9e6-122">retrieves all device twins that reported WiFi or wired connectivity.</span></span> <span data-ttu-id="ec9e6-123">Často je potřeba určit všechny dvojčata zařízení, které obsahují určité vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="ec9e6-123">It is often necessary to identify all device twins that contain a specific property.</span></span> <span data-ttu-id="ec9e6-124">IoT Hub podporuje funkce `is_defined()` pro tento účel.</span><span class="sxs-lookup"><span data-stu-id="ec9e6-124">IoT Hub supports the function `is_defined()` for this purpose.</span></span> <span data-ttu-id="ec9e6-125">Pro instanci,</span><span class="sxs-lookup"><span data-stu-id="ec9e6-125">For instance,</span></span>

```SQL
SELECT * FROM devices
WHERE is_defined(properties.reported.connectivity)
```

<span data-ttu-id="ec9e6-126">načíst všechny dvojčata zařízení, které definují `connectivity` hlášené vlastnost.</span><span class="sxs-lookup"><span data-stu-id="ec9e6-126">retrieved all device twins that define the `connectivity` reported property.</span></span> <span data-ttu-id="ec9e6-127">Odkazovat [klauzule WHERE] [ lnk-query-where] části pro úplný přehled funkcí filtrování.</span><span class="sxs-lookup"><span data-stu-id="ec9e6-127">Refer to the [WHERE clause][lnk-query-where] section for the full reference of the filtering capabilities.</span></span>

<span data-ttu-id="ec9e6-128">Seskupení a agregace jsou také podporovány.</span><span class="sxs-lookup"><span data-stu-id="ec9e6-128">Grouping and aggregations are also supported.</span></span> <span data-ttu-id="ec9e6-129">Pro instanci,</span><span class="sxs-lookup"><span data-stu-id="ec9e6-129">For instance,</span></span>

```sql
SELECT properties.reported.telemetryConfig.status AS status,
    COUNT() AS numberOfDevices
FROM devices
GROUP BY properties.reported.telemetryConfig.status
```

<span data-ttu-id="ec9e6-130">Vrátí počet zařízení v každé stav konfigurace telemetrie.</span><span class="sxs-lookup"><span data-stu-id="ec9e6-130">returns the count of the devices in each telemetry configuration status.</span></span>

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

<span data-ttu-id="ec9e6-131">Předchozí příklad znázorňuje situaci, kdy tři zařízení hlášené úspěšná konfigurace, dva jsou stále aplikování konfigurace a jeden ohlásil chybu.</span><span class="sxs-lookup"><span data-stu-id="ec9e6-131">The preceding example illustrates a situation where three devices reported successful configuration, two are still applying the configuration, and one reported an error.</span></span>

### <a name="c-example"></a><span data-ttu-id="ec9e6-132">Příklad jazyka C#</span><span class="sxs-lookup"><span data-stu-id="ec9e6-132">C# example</span></span>
<span data-ttu-id="ec9e6-133">Funkce dotazu je zveřejněna rozhraním [C# sady SDK služby] [ lnk-hub-sdks] v **RegistryManager** třídy.</span><span class="sxs-lookup"><span data-stu-id="ec9e6-133">The query functionality is exposed by the [C# service SDK][lnk-hub-sdks] in the **RegistryManager** class.</span></span>
<span data-ttu-id="ec9e6-134">Tady je příklad jednoduchého dotazu:</span><span class="sxs-lookup"><span data-stu-id="ec9e6-134">Here is an example of a simple query:</span></span>

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

<span data-ttu-id="ec9e6-135">Poznámka: Jak **dotazu** vytvořit instanci objektu s velikostí stránky (až 1 000), a pak může načíst více stránek volání **GetNextAsTwinAsync** metody vícekrát.</span><span class="sxs-lookup"><span data-stu-id="ec9e6-135">Note how the **query** object is instantiated with a page size (up to 1000), and then multiple pages can be retrieved by calling the **GetNextAsTwinAsync** methods multiple times.</span></span>
<span data-ttu-id="ec9e6-136">Všimněte si, že objektu dotazu vystavuje více **Další\***, v závislosti na možnost deserializaci vyžadované dotazu, například zařízení úlohy nebo dvojici objektů nebo prostý JSON má být použit při použití projekce.</span><span class="sxs-lookup"><span data-stu-id="ec9e6-136">Note that the query object exposes multiple **Next\***, depending on the deserialization option required by the query, such as device twin or job objects, or plain JSON to be used when using projections.</span></span>

### <a name="nodejs-example"></a><span data-ttu-id="ec9e6-137">Příklad Node.js</span><span class="sxs-lookup"><span data-stu-id="ec9e6-137">Node.js example</span></span>
<span data-ttu-id="ec9e6-138">Funkce dotazu je zveřejněna rozhraním [sady SDK služby Azure IoT pro Node.js] [ lnk-hub-sdks] v **registru** objektu.</span><span class="sxs-lookup"><span data-stu-id="ec9e6-138">The query functionality is exposed by the [Azure IoT service SDK for Node.js][lnk-hub-sdks] in the **Registry** object.</span></span>
<span data-ttu-id="ec9e6-139">Tady je příklad jednoduchého dotazu:</span><span class="sxs-lookup"><span data-stu-id="ec9e6-139">Here is an example of a simple query:</span></span>

```nodejs
var query = registry.createQuery('SELECT * FROM devices', 100);
var onResults = function(err, results) {
    if (err) {
        console.error('Failed to fetch the results: ' + err.message);
    } else {
        // Do something with the results
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

<span data-ttu-id="ec9e6-140">Poznámka: Jak **dotazu** vytvořit instanci objektu s velikostí stránky (až 1 000), a pak může načíst více stránek volání **nextAsTwin** metody vícekrát.</span><span class="sxs-lookup"><span data-stu-id="ec9e6-140">Note how the **query** object is instantiated with a page size (up to 1000), and then multiple pages can be retrieved by calling the **nextAsTwin** methods multiple times.</span></span>
<span data-ttu-id="ec9e6-141">Všimněte si, že objektu dotazu vystavuje více **Další\***, v závislosti na možnost deserializaci vyžadované dotazu, například zařízení úlohy nebo dvojici objektů nebo prostý JSON má být použit při použití projekce.</span><span class="sxs-lookup"><span data-stu-id="ec9e6-141">Note that the query object exposes multiple **next\***, depending on the deserialization option required by the query, such as device twin or job objects, or plain JSON to be used when using projections.</span></span>

### <a name="limitations"></a><span data-ttu-id="ec9e6-142">Omezení</span><span class="sxs-lookup"><span data-stu-id="ec9e6-142">Limitations</span></span>
> [!IMPORTANT]
> <span data-ttu-id="ec9e6-143">Výsledky dotazu může mít několik minut zpoždění s ohledem na něj nejnovější hodnoty v dvojčata zařízení.</span><span class="sxs-lookup"><span data-stu-id="ec9e6-143">Query results can have a few minutes of delay with respect to the latest values in device twins.</span></span> <span data-ttu-id="ec9e6-144">Pokud dotazuje dvojčata jednotlivých zařízení podle id, vždycky je vhodnější použít načtení twin rozhraní API pro zařízení, která vždy obsahuje nejnovější hodnoty a má, vyšší omezení.</span><span class="sxs-lookup"><span data-stu-id="ec9e6-144">If querying individual device twins by id, it is always preferable to use the retrieve device twin API, which always contains the latest values and has higher throttling limits.</span></span>

<span data-ttu-id="ec9e6-145">V současné době porovnání jsou podporovány pouze mezi primitivní typy (žádné objekty), například `... WHERE properties.desired.config = properties.reported.config` je podporováno pouze v případě, že tyto vlastnosti mají primitivní hodnoty.</span><span class="sxs-lookup"><span data-stu-id="ec9e6-145">Currently, comparisons are supported only between primitive types (no objects), for instance `... WHERE properties.desired.config = properties.reported.config` is supported only if those properties have primitive values.</span></span>

## <a name="get-started-with-jobs-queries"></a><span data-ttu-id="ec9e6-146">Začínáme s dotazy úlohy</span><span class="sxs-lookup"><span data-stu-id="ec9e6-146">Get started with jobs queries</span></span>
<span data-ttu-id="ec9e6-147">[Úlohy] [ lnk-jobs] poskytují způsob k provedení operací na sadu zařízení.</span><span class="sxs-lookup"><span data-stu-id="ec9e6-147">[Jobs][lnk-jobs] provide a way to execute operations on sets of devices.</span></span> <span data-ttu-id="ec9e6-148">Každý dvojče zařízení obsahuje informace o úlohách, které je součástí v kolekci s názvem **úlohy**.</span><span class="sxs-lookup"><span data-stu-id="ec9e6-148">Each device twin contains the information of the jobs of which it is part in a collection called **jobs**.</span></span>
<span data-ttu-id="ec9e6-149">Logicky,</span><span class="sxs-lookup"><span data-stu-id="ec9e6-149">Logically,</span></span>

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

<span data-ttu-id="ec9e6-150">Tato kolekce je v současné době dotazovatelné jako **devices.jobs** ve službě IoT Hub dotazu jazyka.</span><span class="sxs-lookup"><span data-stu-id="ec9e6-150">Currently, this collection is queryable as **devices.jobs** in the IoT Hub query language.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="ec9e6-151">V současné době je nikdy vrácena vlastnost úlohy při dotazování dvojčata zařízení (to znamená, dotazy, které obsahuje, ze zařízení").</span><span class="sxs-lookup"><span data-stu-id="ec9e6-151">Currently, the jobs property is never returned when querying device twins (that is, queries that contains 'FROM devices').</span></span> <span data-ttu-id="ec9e6-152">Lze přistupovat pouze přímo s dotazy pomocí `FROM devices.jobs`.</span><span class="sxs-lookup"><span data-stu-id="ec9e6-152">It can only be accessed directly with queries using `FROM devices.jobs`.</span></span>
>
>

<span data-ttu-id="ec9e6-153">Například k získání všech úloh (posledních a plánované), které mají vliv jedno zařízení, můžete použít následující dotaz:</span><span class="sxs-lookup"><span data-stu-id="ec9e6-153">For instance, to get all jobs (past and scheduled) that affect a single device, you can use the following query:</span></span>

```sql
SELECT * FROM devices.jobs
WHERE devices.jobs.deviceId = 'myDeviceId'
```

<span data-ttu-id="ec9e6-154">Všimněte si, jak tento dotaz obsahuje stav konkrétní zařízení (a případně přímá metoda odpověď) každé úlohy vrátil.</span><span class="sxs-lookup"><span data-stu-id="ec9e6-154">Note how this query provides the device-specific status (and possibly the direct method response) of each job returned.</span></span>
<span data-ttu-id="ec9e6-155">Je také možné filtrovat pomocí libovolné Boolean podmínek na všechny vlastnosti objektu v **devices.jobs** kolekce.</span><span class="sxs-lookup"><span data-stu-id="ec9e6-155">It is also possible to filter with arbitrary Boolean conditions on all object properties in the **devices.jobs** collection.</span></span>
<span data-ttu-id="ec9e6-156">Například následující dotaz:</span><span class="sxs-lookup"><span data-stu-id="ec9e6-156">For instance, the following query:</span></span>

```sql
SELECT * FROM devices.jobs
WHERE devices.jobs.deviceId = 'myDeviceId'
    AND devices.jobs.jobType = 'scheduleTwinUpdate'
    AND devices.jobs.status = 'completed'
    AND devices.jobs.createdTimeUtc > '2016-09-01'
```

<span data-ttu-id="ec9e6-157">načte všechny dokončit zařízení twin aktualizace úlohy pro zařízení **myDeviceId** vytvořených po září 2016.</span><span class="sxs-lookup"><span data-stu-id="ec9e6-157">retrieves all completed device twin update jobs for device **myDeviceId** that were created after September 2016.</span></span>

<span data-ttu-id="ec9e6-158">Je také možné načíst výstupy podle zařízení jednu úlohu.</span><span class="sxs-lookup"><span data-stu-id="ec9e6-158">It is also possible to retrieve the per-device outcomes of a single job.</span></span>

```sql
SELECT * FROM devices.jobs
WHERE devices.jobs.jobId = 'myJobId'
```

### <a name="limitations"></a><span data-ttu-id="ec9e6-159">Omezení</span><span class="sxs-lookup"><span data-stu-id="ec9e6-159">Limitations</span></span>
<span data-ttu-id="ec9e6-160">V současné době se dotazuje na **devices.jobs** nepodporují:</span><span class="sxs-lookup"><span data-stu-id="ec9e6-160">Currently, queries on **devices.jobs** do not support:</span></span>

* <span data-ttu-id="ec9e6-161">Projekce, proto pouze `SELECT *` je možné.</span><span class="sxs-lookup"><span data-stu-id="ec9e6-161">Projections, therefore only `SELECT *` is possible.</span></span>
* <span data-ttu-id="ec9e6-162">Podmínky, které odkazují na dvojče zařízení kromě vlastnosti úlohy (viz předchozí části).</span><span class="sxs-lookup"><span data-stu-id="ec9e6-162">Conditions that refer to the device twin in addition to job properties (see the preceding section).</span></span>
* <span data-ttu-id="ec9e6-163">Provádění agregace, jako je počet, avg, Seskupit podle.</span><span class="sxs-lookup"><span data-stu-id="ec9e6-163">Performing aggregations, such as count, avg, group by.</span></span>

## <a name="get-started-with-device-to-cloud-message-routes-query-expressions"></a><span data-ttu-id="ec9e6-164">Začínáme s výrazy dotazů trasy zprávu typu zařízení cloud</span><span class="sxs-lookup"><span data-stu-id="ec9e6-164">Get started with device-to-cloud message routes query expressions</span></span>

<span data-ttu-id="ec9e6-165">Pomocí [zařízení cloud trasy][lnk-devguide-messaging-routes], můžete nakonfigurovat Centrum IoT k odesílání zpráv typu zařízení cloud k různým koncovým bodům založené na výrazech vyhodnotit na základě jednotlivých zpráv.</span><span class="sxs-lookup"><span data-stu-id="ec9e6-165">Using [device-to-cloud routes][lnk-devguide-messaging-routes], you can configure IoT Hub to dispatch device-to-cloud messages to different endpoints based on expressions evaluated against individual messages.</span></span>

<span data-ttu-id="ec9e6-166">Trasa [podmínku] [ lnk-query-expressions] používá stejný dotaz jazyk IoT Hub jako podmínky v dotazech twin a úlohy.</span><span class="sxs-lookup"><span data-stu-id="ec9e6-166">The route [condition][lnk-query-expressions] uses the same IoT Hub query language as conditions in twin and job queries.</span></span> <span data-ttu-id="ec9e6-167">Podmínky trasy se vyhodnocují na hlavičky zpráv a text.</span><span class="sxs-lookup"><span data-stu-id="ec9e6-167">Route conditions are evaluated on the message headers and body.</span></span> <span data-ttu-id="ec9e6-168">Směrování výraz dotazu může zahrnovat pouze hlavičky zpráv, pouze text zprávy, nebo obě zpráva hlavičky a tělo zprávy.</span><span class="sxs-lookup"><span data-stu-id="ec9e6-168">Your routing query expression may involve only message headers, only the message body, or both message headers and message body.</span></span> <span data-ttu-id="ec9e6-169">IoT Hub předpokládá konkrétní schéma pro záhlaví a text zprávy, aby bylo možné směrovat zprávy a následující části popisují, co je vyžadována pro IoT Hub, která správně trasy:</span><span class="sxs-lookup"><span data-stu-id="ec9e6-169">IoT Hub assumes a specific schema for the headers and message body in order to route messages, and the following sections describe what is required for IoT Hub to properly route:</span></span>

### <a name="routing-on-message-headers"></a><span data-ttu-id="ec9e6-170">Směrování v záhlaví zpráv</span><span class="sxs-lookup"><span data-stu-id="ec9e6-170">Routing on message headers</span></span>

<span data-ttu-id="ec9e6-171">IoT Hub předpokládá následující reprezentace JSON hlavičky zpráv pro směrování zpráv:</span><span class="sxs-lookup"><span data-stu-id="ec9e6-171">IoT Hub assumes the following JSON representation of message headers for message routing:</span></span>

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

<span data-ttu-id="ec9e6-172">Vlastnosti zprávu systému mají předponu `'$'` symbol.</span><span class="sxs-lookup"><span data-stu-id="ec9e6-172">Message system properties are prefixed with the `'$'` symbol.</span></span>
<span data-ttu-id="ec9e6-173">Vlastnosti uživatelů jsou vždy přistupovat pomocí jeho názvu.</span><span class="sxs-lookup"><span data-stu-id="ec9e6-173">User properties are always accessed with their name.</span></span> <span data-ttu-id="ec9e6-174">Pokud se stane název vlastnosti uživatele, se shoduje s vlastností systému (například `$to`), bude načten vlastnost uživatele s `$to` výraz.</span><span class="sxs-lookup"><span data-stu-id="ec9e6-174">If a user property name happens to coincide with a system property (such as `$to`), the user property will be retrieved with the `$to` expression.</span></span>
<span data-ttu-id="ec9e6-175">Vždy přístup k vlastnosti systému pomocí závorek `{}`: například můžete použít ve výrazu `{$to}` pro přístup k vlastnosti systému `to`.</span><span class="sxs-lookup"><span data-stu-id="ec9e6-175">You can always access the system property using brackets `{}`: for instance, you can use the expression `{$to}` to access the system property `to`.</span></span> <span data-ttu-id="ec9e6-176">Názvy vlastností v závorkách vždy načítají odpovídající vlastnost systému.</span><span class="sxs-lookup"><span data-stu-id="ec9e6-176">Bracketed property names always retrieve the corresponding system property.</span></span>

<span data-ttu-id="ec9e6-177">Mějte na paměti, že jsou názvy vlastností malá a velká písmena.</span><span class="sxs-lookup"><span data-stu-id="ec9e6-177">Remember that property names are case insensitive.</span></span>

> [!NOTE]
> <span data-ttu-id="ec9e6-178">Všechny vlastnosti zprávy jsou řetězce.</span><span class="sxs-lookup"><span data-stu-id="ec9e6-178">All message properties are strings.</span></span> <span data-ttu-id="ec9e6-179">Vlastnosti systému, jak je popsáno v [Příručka vývojáře][lnk-devguide-messaging-format], nejsou aktuálně k dispozici pro použití v dotazech.</span><span class="sxs-lookup"><span data-stu-id="ec9e6-179">System properties, as described in the [developer guide][lnk-devguide-messaging-format], are currently not available to use in queries.</span></span>
>

<span data-ttu-id="ec9e6-180">Například, pokud používáte `messageType` vlastnost, můžete chtít směrovat všechny telemetrická jeden koncový bod a všechny výstrahy pro jiný koncový bod.</span><span class="sxs-lookup"><span data-stu-id="ec9e6-180">For example, if you use a `messageType` property, you might want to route all telemetry to one endpoint, and all alerts to another endpoint.</span></span> <span data-ttu-id="ec9e6-181">Můžete napsat následující výraz směrovat telemetrie:</span><span class="sxs-lookup"><span data-stu-id="ec9e6-181">You can write the following expression to route the telemetry:</span></span>

```sql
messageType = 'telemetry'
```

<span data-ttu-id="ec9e6-182">A následující výraz směrovat výstražných zpráv:</span><span class="sxs-lookup"><span data-stu-id="ec9e6-182">And the following expression to route the alert messages:</span></span>

```sql
messageType = 'alert'
```

<span data-ttu-id="ec9e6-183">Logické výrazy a funkce jsou také podporovány.</span><span class="sxs-lookup"><span data-stu-id="ec9e6-183">Boolean expressions and functions are also supported.</span></span> <span data-ttu-id="ec9e6-184">Tato funkce umožňuje rozlišovat mezi úroveň závažnosti, například:</span><span class="sxs-lookup"><span data-stu-id="ec9e6-184">This feature enables you to distinguish between severity level, for example:</span></span>

```sql
messageType = 'alerts' AND as_number(severity) <= 2
```

<span data-ttu-id="ec9e6-185">Odkazovat [výraz a podmínky] [ lnk-query-expressions] části úplný seznam podporovaných operátory a funkce.</span><span class="sxs-lookup"><span data-stu-id="ec9e6-185">Refer to the [Expression and conditions][lnk-query-expressions] section for the full list of supported operators and functions.</span></span>

### <a name="routing-on-message-bodies"></a><span data-ttu-id="ec9e6-186">Směrování na obsah zpráv</span><span class="sxs-lookup"><span data-stu-id="ec9e6-186">Routing on message bodies</span></span>

<span data-ttu-id="ec9e6-187">IoT Hub můžete pouze směrování podle tělo zprávy obsah, pokud text zprávy je správně vytvořen JSON kódování UTF-8, UTF-16 nebo UTF-32.</span><span class="sxs-lookup"><span data-stu-id="ec9e6-187">IoT Hub can only route based on message body contents if the message body is properly formed JSON encoded in either UTF-8, UTF-16, or UTF-32.</span></span> <span data-ttu-id="ec9e6-188">Je nutné nastavit typ obsahu zprávy `application/json` a kódování obsahu na jednu z podporovaných kódování UTF v záhlaví zprávy umožňující IoT Hub pro směrování zpráv na základě obsahu textu.</span><span class="sxs-lookup"><span data-stu-id="ec9e6-188">You must set the content type of the message to `application/json` and the content encoding to one of the supported UTF encodings in the message headers to allow IoT Hub to route the message based on the body contents.</span></span> <span data-ttu-id="ec9e6-189">Pokud není zadán buď z hlaviček, IoT Hub se nebude pokoušet vyhodnotit jakýkoli výraz dotazu zahrnující těla proti zprávy.</span><span class="sxs-lookup"><span data-stu-id="ec9e6-189">If either of the headers is not specified, IoT Hub will not attempt to evaluate any query expression involving the body against the message.</span></span> <span data-ttu-id="ec9e6-190">Pokud vaše zpráva není zprávu JSON, nebo pokud zpráva neurčuje typu obsahu a kódování obsahu, může stále používáte směrování zpráv směrovat zprávy založené na záhlaví zprávy.</span><span class="sxs-lookup"><span data-stu-id="ec9e6-190">If your message is not a JSON message, or if the message does not specify the content type and content encoding, you may still use message routing to route the message based on the message headers.</span></span>

<span data-ttu-id="ec9e6-191">Můžete použít `$body` ve výrazu dotazu pro odesílání zpráv.</span><span class="sxs-lookup"><span data-stu-id="ec9e6-191">You can use `$body` in the query expression to route the message.</span></span> <span data-ttu-id="ec9e6-192">Jednoduchý text odkazu, odkaz na pole text nebo více odkazů text můžete použít ve výrazu dotazu.</span><span class="sxs-lookup"><span data-stu-id="ec9e6-192">You can use a simple body reference, body array reference, or multiple body references in the query expression.</span></span> <span data-ttu-id="ec9e6-193">Výraz dotazu můžete také kombinovat textu odkaz s odkazem na záhlaví zprávy.</span><span class="sxs-lookup"><span data-stu-id="ec9e6-193">Your query expression can also combine a body reference with a message header reference.</span></span> <span data-ttu-id="ec9e6-194">Tady jsou například všechny výrazy platný dotaz:</span><span class="sxs-lookup"><span data-stu-id="ec9e6-194">For example, the following are all valid query expressions:</span></span>

```sql
$body.message.Weather.Location.State = 'WA'
$body.Weather.HistoricalData[0].Month = 'Feb'
$body.Weather.Temperature = 50 AND $body.message.Weather.IsEnabled
length($body.Weather.Location.State) = 2
$body.Weather.Temperature = 50 AND Status = 'Active'
```

## <a name="basics-of-an-iot-hub-query"></a><span data-ttu-id="ec9e6-195">Základní informace o dotaz služby IoT Hub</span><span class="sxs-lookup"><span data-stu-id="ec9e6-195">Basics of an IoT Hub query</span></span>
<span data-ttu-id="ec9e6-196">Každé centrum IoT dotaz sestává s výběrem a klauzulí FROM a volitelné WHERE a GROUP BY – klauzule.</span><span class="sxs-lookup"><span data-stu-id="ec9e6-196">Every IoT Hub query consists of a SELECT and FROM clauses and by optional WHERE and GROUP BY clauses.</span></span> <span data-ttu-id="ec9e6-197">Každý dotaz je spuštěn na kolekci dokumentů JSON, například dvojčata zařízení.</span><span class="sxs-lookup"><span data-stu-id="ec9e6-197">Every query is run on a collection of JSON documents, for example device twins.</span></span> <span data-ttu-id="ec9e6-198">V klauzuli FROM označuje kolekce dokumentu. Chcete-li být vstupní na (**zařízení** nebo **devices.jobs**).</span><span class="sxs-lookup"><span data-stu-id="ec9e6-198">The FROM clause indicates the document collection to be iterated on (**devices** or **devices.jobs**).</span></span> <span data-ttu-id="ec9e6-199">Pak je použit filtr v klauzuli WHERE.</span><span class="sxs-lookup"><span data-stu-id="ec9e6-199">Then, the filter in the WHERE clause is applied.</span></span> <span data-ttu-id="ec9e6-200">S agregace, výsledky tohoto kroku seskupeny jako zadaný v klauzuli GROUP BY a pro každou skupinu, je generována řádek uvedené v klauzuli SELECT.</span><span class="sxs-lookup"><span data-stu-id="ec9e6-200">With aggregations, the results of this step are grouped as specified in the GROUP BY clause and, for each group, a row is generated as specified in the SELECT clause.</span></span>

```sql
SELECT <select_list>
FROM <from_specification>
[WHERE <filter_condition>]
[GROUP BY <group_specification>]
```

## <a name="from-clause"></a><span data-ttu-id="ec9e6-201">FROM – klauzule</span><span class="sxs-lookup"><span data-stu-id="ec9e6-201">FROM clause</span></span>
<span data-ttu-id="ec9e6-202">**z < from_specification >** klauzule můžete předpokládat pouze dvě hodnoty: **ze zařízení**, aby dotaz dvojčata zařízení nebo **z devices.jobs**, k dotazování úlohy podle zařízení podrobnosti.</span><span class="sxs-lookup"><span data-stu-id="ec9e6-202">The **FROM <from_specification>** clause can assume only two values: **FROM devices**, to query device twins, or **FROM devices.jobs**, to query job per-device details.</span></span>

## <a name="where-clause"></a><span data-ttu-id="ec9e6-203">Klauzule WHERE</span><span class="sxs-lookup"><span data-stu-id="ec9e6-203">WHERE clause</span></span>
<span data-ttu-id="ec9e6-204">**Kde < filter_condition >** klauzule je volitelný.</span><span class="sxs-lookup"><span data-stu-id="ec9e6-204">The **WHERE <filter_condition>** clause is optional.</span></span> <span data-ttu-id="ec9e6-205">Určuje, že jeden nebo více podmínky, že dokumenty JSON v kolekci FROM musí splňovat zahrnuty jako součást výsledku.</span><span class="sxs-lookup"><span data-stu-id="ec9e6-205">It specifies one or more conditions that the JSON documents in the FROM collection must satisfy to be included as part of the result.</span></span> <span data-ttu-id="ec9e6-206">Dokumentu JSON se musí vyhodnotit na hodnotu true, mají být zahrnuty do výsledku k zadaným podmínkám.</span><span class="sxs-lookup"><span data-stu-id="ec9e6-206">Any JSON document must evaluate the specified conditions to "true" to be included in the result.</span></span>

<span data-ttu-id="ec9e6-207">Povolené podmínky jsou popsány v části [výrazy a podmínky][lnk-query-expressions].</span><span class="sxs-lookup"><span data-stu-id="ec9e6-207">The allowed conditions are described in section [Expressions and conditions][lnk-query-expressions].</span></span>

## <a name="select-clause"></a><span data-ttu-id="ec9e6-208">Klauzule SELECT</span><span class="sxs-lookup"><span data-stu-id="ec9e6-208">SELECT clause</span></span>
<span data-ttu-id="ec9e6-209">Klauzule SELECT (**vyberte < select_list >**) je povinná a určuje, jaké hodnoty jsou načteny z dotazu.</span><span class="sxs-lookup"><span data-stu-id="ec9e6-209">The SELECT clause (**SELECT <select_list>**) is mandatory and specifies what values are retrieved from the query.</span></span> <span data-ttu-id="ec9e6-210">Určuje hodnoty JSON, které mají být použita ke generování nových objektů JSON.</span><span class="sxs-lookup"><span data-stu-id="ec9e6-210">It specifies the JSON values to be used to generate new JSON objects.</span></span>
<span data-ttu-id="ec9e6-211">Pro každý prvek filtrované (a volitelně seskupené) podmnožinu kolekce FROM fázi projekce generuje nový objekt JSON, zkonstruovat s hodnot zadaných v klauzuli SELECT.</span><span class="sxs-lookup"><span data-stu-id="ec9e6-211">For each element of the filtered (and optionally grouped) subset of the FROM collection, the projection phase generates a new JSON object, constructed with the values specified in the SELECT clause.</span></span>

<span data-ttu-id="ec9e6-212">Toto je gramatiky klauzuli SELECT:</span><span class="sxs-lookup"><span data-stu-id="ec9e6-212">Following is the grammar of the SELECT clause:</span></span>

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

<span data-ttu-id="ec9e6-213">kde **attribute_name** odkazuje na žádnou vlastnost dokumentu JSON v kolekci FROM.</span><span class="sxs-lookup"><span data-stu-id="ec9e6-213">where **attribute_name** refers to any property of the JSON document in the FROM collection.</span></span> <span data-ttu-id="ec9e6-214">Některé příklady klauzule FROM lze nalézt v [Začínáme s dotazy twin zařízení] [ lnk-query-getstarted] části.</span><span class="sxs-lookup"><span data-stu-id="ec9e6-214">Some examples of SELECT clauses can be found in the [Getting started with device twin queries][lnk-query-getstarted] section.</span></span>

<span data-ttu-id="ec9e6-215">V současné době výběr klauzule liší od **vyberte \***  jsou podporovány pouze v agregační dotazy na dvojčata zařízení.</span><span class="sxs-lookup"><span data-stu-id="ec9e6-215">Currently, selection clauses different than **SELECT \*** are only supported in aggregate queries on device twins.</span></span>

## <a name="group-by-clause"></a><span data-ttu-id="ec9e6-216">klauzule GROUP BY</span><span class="sxs-lookup"><span data-stu-id="ec9e6-216">GROUP BY clause</span></span>
<span data-ttu-id="ec9e6-217">**GROUP BY < group_specification >** klauzule je volitelný krok, který lze provést po filtr zadaný v klauzuli WHERE a před projekce určená v SELECT.</span><span class="sxs-lookup"><span data-stu-id="ec9e6-217">The **GROUP BY <group_specification>** clause is an optional step that can be executed after the filter specified in the WHERE clause, and before the projection specified in the SELECT.</span></span> <span data-ttu-id="ec9e6-218">Seskupuje dokumentů na základě hodnoty atributu.</span><span class="sxs-lookup"><span data-stu-id="ec9e6-218">It groups documents based on the value of an attribute.</span></span> <span data-ttu-id="ec9e6-219">Tyto skupiny se používají ke generování agregované hodnoty zadané v klauzuli SELECT.</span><span class="sxs-lookup"><span data-stu-id="ec9e6-219">These groups are used to generate aggregated values as specified in the SELECT clause.</span></span>

<span data-ttu-id="ec9e6-220">Příklad dotazu pomocí skupiny je:</span><span class="sxs-lookup"><span data-stu-id="ec9e6-220">An example of a query using GROUP BY is:</span></span>

```sql
SELECT properties.reported.telemetryConfig.status AS status,
    COUNT() AS numberOfDevices
FROM devices
GROUP BY properties.reported.telemetryConfig.status
```

<span data-ttu-id="ec9e6-221">Formální syntaxe GROUP BY je:</span><span class="sxs-lookup"><span data-stu-id="ec9e6-221">The formal syntax for GROUP BY is:</span></span>

```
GROUP BY <group_by_element>
<group_by_element> :==
    attribute_name
    | < group_by_element > '.' attribute_name
```

<span data-ttu-id="ec9e6-222">kde **attribute_name** odkazuje na žádnou vlastnost dokumentu JSON v kolekci FROM.</span><span class="sxs-lookup"><span data-stu-id="ec9e6-222">where **attribute_name** refers to any property of the JSON document in the FROM collection.</span></span>

<span data-ttu-id="ec9e6-223">V klauzuli GROUP BY je v současné době podporována pouze při dotazování dvojčata zařízení.</span><span class="sxs-lookup"><span data-stu-id="ec9e6-223">Currently, the GROUP BY clause is only supported when querying device twins.</span></span>

## <a name="expressions-and-conditions"></a><span data-ttu-id="ec9e6-224">Výrazy a podmínky</span><span class="sxs-lookup"><span data-stu-id="ec9e6-224">Expressions and conditions</span></span>
<span data-ttu-id="ec9e6-225">Na vysoké úrovni *výraz*:</span><span class="sxs-lookup"><span data-stu-id="ec9e6-225">At a high level, an *expression*:</span></span>

* <span data-ttu-id="ec9e6-226">Vyhodnotí na instanci typu JSON (například logická hodnota, čísla, řetězce, pole nebo objekt), a</span><span class="sxs-lookup"><span data-stu-id="ec9e6-226">Evaluates to an instance of a JSON type (such as Boolean, number, string, array, or object), and</span></span>
* <span data-ttu-id="ec9e6-227">Je definován manipulace s daty z dokumentu JSON zařízení a konstanty pomocí předdefinovaných operátory a funkce.</span><span class="sxs-lookup"><span data-stu-id="ec9e6-227">Is defined by manipulating data coming from the device JSON document and constants using built-in operators and functions.</span></span>

<span data-ttu-id="ec9e6-228">*Podmínky* jsou výrazy, které vyhodnocena jako logická hodnota.</span><span class="sxs-lookup"><span data-stu-id="ec9e6-228">*Conditions* are expressions that evaluate to a Boolean.</span></span> <span data-ttu-id="ec9e6-229">Žádné jiné než logická hodnota konstanta **true** se považuje za **false** (včetně **null**, **nedefinované**, všechny objekt nebo pole instance, libovolného řetězce a jasně logickou hodnotu **false**).</span><span class="sxs-lookup"><span data-stu-id="ec9e6-229">Any constant different than Boolean **true** is considered as **false** (including **null**, **undefined**, any object or array instance, any string, and clearly the Boolean **false**).</span></span>

<span data-ttu-id="ec9e6-230">Syntaxe pro výrazy je:</span><span class="sxs-lookup"><span data-stu-id="ec9e6-230">The syntax for expressions is:</span></span>

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

<span data-ttu-id="ec9e6-231">Kde:</span><span class="sxs-lookup"><span data-stu-id="ec9e6-231">where:</span></span>

| <span data-ttu-id="ec9e6-232">Symbol</span><span class="sxs-lookup"><span data-stu-id="ec9e6-232">Symbol</span></span> | <span data-ttu-id="ec9e6-233">Definice</span><span class="sxs-lookup"><span data-stu-id="ec9e6-233">Definition</span></span> |
| --- | --- |
| <span data-ttu-id="ec9e6-234">attribute_name</span><span class="sxs-lookup"><span data-stu-id="ec9e6-234">attribute_name</span></span> | <span data-ttu-id="ec9e6-235">Všechny vlastnosti v dokumentu JSON **FROM** kolekce.</span><span class="sxs-lookup"><span data-stu-id="ec9e6-235">Any property of the JSON document in the **FROM** collection.</span></span> |
| <span data-ttu-id="ec9e6-236">binary_operator</span><span class="sxs-lookup"><span data-stu-id="ec9e6-236">binary_operator</span></span> | <span data-ttu-id="ec9e6-237">Všechny binární operátor uvedené v [operátory](#operators) části.</span><span class="sxs-lookup"><span data-stu-id="ec9e6-237">Any binary operator listed in the [Operators](#operators) section.</span></span> |
| <span data-ttu-id="ec9e6-238">Název_funkce</span><span class="sxs-lookup"><span data-stu-id="ec9e6-238">function_name</span></span>| <span data-ttu-id="ec9e6-239">Všechny funkce uvedené v [funkce](#functions) části.</span><span class="sxs-lookup"><span data-stu-id="ec9e6-239">Any function listed in the [Functions](#functions) section.</span></span> |
| <span data-ttu-id="ec9e6-240">decimal_literal</span><span class="sxs-lookup"><span data-stu-id="ec9e6-240">decimal_literal</span></span> |<span data-ttu-id="ec9e6-241">Float, vyjádřené v desítkovém zápisu.</span><span class="sxs-lookup"><span data-stu-id="ec9e6-241">A float expressed in decimal notation.</span></span> |
| <span data-ttu-id="ec9e6-242">hexadecimal_literal</span><span class="sxs-lookup"><span data-stu-id="ec9e6-242">hexadecimal_literal</span></span> |<span data-ttu-id="ec9e6-243">Číslo vyjádřená řetězec '0 x, za nímž následuje řetězec šestnáctkových číslic.</span><span class="sxs-lookup"><span data-stu-id="ec9e6-243">A number expressed by the string ‘0x’ followed by a string of hexadecimal digits.</span></span> |
| <span data-ttu-id="ec9e6-244">string_literal</span><span class="sxs-lookup"><span data-stu-id="ec9e6-244">string_literal</span></span> |<span data-ttu-id="ec9e6-245">Textové literály jsou reprezentována posloupnost nula nebo více znaků Unicode nebo řídicí sekvence řetězců v kódu Unicode.</span><span class="sxs-lookup"><span data-stu-id="ec9e6-245">String literals are Unicode strings represented by a sequence of zero or more Unicode characters or escape sequences.</span></span> <span data-ttu-id="ec9e6-246">Textové literály jsou uzavřená do jednoduchých uvozovek (apostrof: ') nebo dvojité uvozovky (uvozovky: ").</span><span class="sxs-lookup"><span data-stu-id="ec9e6-246">String literals are enclosed in single quotes (apostrophe: ' ) or double quotes (quotation mark: ").</span></span> <span data-ttu-id="ec9e6-247">Povolené řídicí sekvence: `\'`, `\"`, `\\`, `\uXXXX` pro znaky znakové sady Unicode, které jsou definované 4 hexadecimální číslice.</span><span class="sxs-lookup"><span data-stu-id="ec9e6-247">Allowed escapes: `\'`, `\"`, `\\`, `\uXXXX` for Unicode characters defined by 4 hexadecimal digits.</span></span> |

### <a name="operators"></a><span data-ttu-id="ec9e6-248">Operátory</span><span class="sxs-lookup"><span data-stu-id="ec9e6-248">Operators</span></span>
<span data-ttu-id="ec9e6-249">Podporovány jsou následující operátory:</span><span class="sxs-lookup"><span data-stu-id="ec9e6-249">The following operators are supported:</span></span>

| <span data-ttu-id="ec9e6-250">Rodina</span><span class="sxs-lookup"><span data-stu-id="ec9e6-250">Family</span></span> | <span data-ttu-id="ec9e6-251">Operátory</span><span class="sxs-lookup"><span data-stu-id="ec9e6-251">Operators</span></span> |
| --- | --- |
| <span data-ttu-id="ec9e6-252">Aritmetické operace</span><span class="sxs-lookup"><span data-stu-id="ec9e6-252">Arithmetic</span></span> |<span data-ttu-id="ec9e6-253">+,-,*,/,%</span><span class="sxs-lookup"><span data-stu-id="ec9e6-253">+,-,*,/,%</span></span> |
| <span data-ttu-id="ec9e6-254">Logické</span><span class="sxs-lookup"><span data-stu-id="ec9e6-254">Logical</span></span> |<span data-ttu-id="ec9e6-255">A, NEBO NE</span><span class="sxs-lookup"><span data-stu-id="ec9e6-255">AND, OR, NOT</span></span> |
| <span data-ttu-id="ec9e6-256">Porovnání</span><span class="sxs-lookup"><span data-stu-id="ec9e6-256">Comparison</span></span> |<span data-ttu-id="ec9e6-257">=, !=, <, >, <=, >=, <></span><span class="sxs-lookup"><span data-stu-id="ec9e6-257">=, !=, <, >, <=, >=, <></span></span> |

### <a name="functions"></a><span data-ttu-id="ec9e6-258">Funkce</span><span class="sxs-lookup"><span data-stu-id="ec9e6-258">Functions</span></span>
<span data-ttu-id="ec9e6-259">Při dotazování dvojčata a úlohám, které jediný podporovaný je funkce:</span><span class="sxs-lookup"><span data-stu-id="ec9e6-259">When querying twins and jobs the only supported function is:</span></span>

| <span data-ttu-id="ec9e6-260">Funkce</span><span class="sxs-lookup"><span data-stu-id="ec9e6-260">Function</span></span> | <span data-ttu-id="ec9e6-261">Popis</span><span class="sxs-lookup"><span data-stu-id="ec9e6-261">Description</span></span> |
| -------- | ----------- |
| <span data-ttu-id="ec9e6-262">IS_DEFINED(Property)</span><span class="sxs-lookup"><span data-stu-id="ec9e6-262">IS_DEFINED(property)</span></span> | <span data-ttu-id="ec9e6-263">Vrátí logickou hodnotu udávající, pokud byla vlastnost přiřazenou hodnotu (včetně `null`).</span><span class="sxs-lookup"><span data-stu-id="ec9e6-263">Returns a Boolean indicating if the property has been assigned a value (including `null`).</span></span> |

<span data-ttu-id="ec9e6-264">V podmínkách trasy jsou podporovány následující matematické funkce:</span><span class="sxs-lookup"><span data-stu-id="ec9e6-264">In routes conditions, the following math functions are supported:</span></span>

| <span data-ttu-id="ec9e6-265">Funkce</span><span class="sxs-lookup"><span data-stu-id="ec9e6-265">Function</span></span> | <span data-ttu-id="ec9e6-266">Popis</span><span class="sxs-lookup"><span data-stu-id="ec9e6-266">Description</span></span> |
| -------- | ----------- |
| <span data-ttu-id="ec9e6-267">Abs(x)</span><span class="sxs-lookup"><span data-stu-id="ec9e6-267">ABS(x)</span></span> | <span data-ttu-id="ec9e6-268">Vrátí absolutní hodnotu (kladné) zadaný číselný výraz.</span><span class="sxs-lookup"><span data-stu-id="ec9e6-268">Returns the absolute (positive) value of the specified numeric expression.</span></span> |
| <span data-ttu-id="ec9e6-269">Exp(x)</span><span class="sxs-lookup"><span data-stu-id="ec9e6-269">EXP(x)</span></span> | <span data-ttu-id="ec9e6-270">Vrátí hodnotu exponenciálního zadaný číselný výraz (e ^ x).</span><span class="sxs-lookup"><span data-stu-id="ec9e6-270">Returns the exponential value of the specified numeric expression (e^x).</span></span> |
| <span data-ttu-id="ec9e6-271">Power(x,y)</span><span class="sxs-lookup"><span data-stu-id="ec9e6-271">POWER(x,y)</span></span> | <span data-ttu-id="ec9e6-272">Vrátí hodnotu zadaného výrazu určenou mocninu (x ^ y).</span><span class="sxs-lookup"><span data-stu-id="ec9e6-272">Returns the value of the specified expression to the specified power (x^y).</span></span>|
| <span data-ttu-id="ec9e6-273">Square(x)</span><span class="sxs-lookup"><span data-stu-id="ec9e6-273">SQUARE(x)</span></span> | <span data-ttu-id="ec9e6-274">Vrátí druhou mocninu Zadaná číselná hodnota.</span><span class="sxs-lookup"><span data-stu-id="ec9e6-274">Returns the square of the specified numeric value.</span></span> |
| <span data-ttu-id="ec9e6-275">CEILING(x)</span><span class="sxs-lookup"><span data-stu-id="ec9e6-275">CEILING(x)</span></span> | <span data-ttu-id="ec9e6-276">Vrátí nejmenší hodnotu, celé číslo větší než nebo rovna hodnotě zadané číselný výraz.</span><span class="sxs-lookup"><span data-stu-id="ec9e6-276">Returns the smallest integer value greater than, or equal to, the specified numeric expression.</span></span> |
| <span data-ttu-id="ec9e6-277">Floor(x)</span><span class="sxs-lookup"><span data-stu-id="ec9e6-277">FLOOR(x)</span></span> | <span data-ttu-id="ec9e6-278">Vrátí největší celé číslo menší než nebo rovna zadané číselný výraz.</span><span class="sxs-lookup"><span data-stu-id="ec9e6-278">Returns the largest integer less than or equal to the specified numeric expression.</span></span> |
| <span data-ttu-id="ec9e6-279">Sign(x)</span><span class="sxs-lookup"><span data-stu-id="ec9e6-279">SIGN(x)</span></span> | <span data-ttu-id="ec9e6-280">Vrátí kladnou (+ 1), nula (0) nebo záporné znaménko (-1) zadaný číselný výraz.</span><span class="sxs-lookup"><span data-stu-id="ec9e6-280">Returns the positive (+1), zero (0), or negative (-1) sign of the specified numeric expression.</span></span>|
| <span data-ttu-id="ec9e6-281">Sqrt(x)</span><span class="sxs-lookup"><span data-stu-id="ec9e6-281">SQRT(x)</span></span> | <span data-ttu-id="ec9e6-282">Vrátí druhou mocninu Zadaná číselná hodnota.</span><span class="sxs-lookup"><span data-stu-id="ec9e6-282">Returns the square of the specified numeric value.</span></span> |

<span data-ttu-id="ec9e6-283">V podmínkách trasy jsou podporovány následující kontrola typu a přetypování funkce:</span><span class="sxs-lookup"><span data-stu-id="ec9e6-283">In routes conditions, the following type checking and casting functions are supported:</span></span>

| <span data-ttu-id="ec9e6-284">Funkce</span><span class="sxs-lookup"><span data-stu-id="ec9e6-284">Function</span></span> | <span data-ttu-id="ec9e6-285">Popis</span><span class="sxs-lookup"><span data-stu-id="ec9e6-285">Description</span></span> |
| -------- | ----------- |
| <span data-ttu-id="ec9e6-286">AS_NUMBER</span><span class="sxs-lookup"><span data-stu-id="ec9e6-286">AS_NUMBER</span></span> | <span data-ttu-id="ec9e6-287">Převede vstupní řetězec na číslo.</span><span class="sxs-lookup"><span data-stu-id="ec9e6-287">Converts the input string to a number.</span></span> <span data-ttu-id="ec9e6-288">`noop`Pokud vstup je číslo; `Undefined` Pokud řetězec nepředstavuje číslo.</span><span class="sxs-lookup"><span data-stu-id="ec9e6-288">`noop` if input is a number; `Undefined` if string does not represent a number.</span></span>|
| <span data-ttu-id="ec9e6-289">IS_ARRAY</span><span class="sxs-lookup"><span data-stu-id="ec9e6-289">IS_ARRAY</span></span> | <span data-ttu-id="ec9e6-290">Vrátí logickou hodnotu udávající, pokud je typ zadaný výraz pole.</span><span class="sxs-lookup"><span data-stu-id="ec9e6-290">Returns a Boolean value indicating if the type of the specified expression is an array.</span></span> |
| <span data-ttu-id="ec9e6-291">IS_BOOL</span><span class="sxs-lookup"><span data-stu-id="ec9e6-291">IS_BOOL</span></span> | <span data-ttu-id="ec9e6-292">Vrátí logickou hodnotu udávající, pokud typ zadaný výraz je logická hodnota.</span><span class="sxs-lookup"><span data-stu-id="ec9e6-292">Returns a Boolean value indicating if the type of the specified expression is a Boolean.</span></span> |
| <span data-ttu-id="ec9e6-293">IS_DEFINED</span><span class="sxs-lookup"><span data-stu-id="ec9e6-293">IS_DEFINED</span></span> | <span data-ttu-id="ec9e6-294">Vrátí logickou hodnotu udávající, pokud byla vlastnost přiřazenou hodnotu.</span><span class="sxs-lookup"><span data-stu-id="ec9e6-294">Returns a Boolean indicating if the property has been assigned a value.</span></span> |
| <span data-ttu-id="ec9e6-295">IS_NULL</span><span class="sxs-lookup"><span data-stu-id="ec9e6-295">IS_NULL</span></span> | <span data-ttu-id="ec9e6-296">Vrátí logickou hodnotu udávající, pokud není typ zadaného výrazu hodnotu null.</span><span class="sxs-lookup"><span data-stu-id="ec9e6-296">Returns a Boolean value indicating if the type of the specified expression is null.</span></span> |
| <span data-ttu-id="ec9e6-297">IS_NUMBER</span><span class="sxs-lookup"><span data-stu-id="ec9e6-297">IS_NUMBER</span></span> | <span data-ttu-id="ec9e6-298">Vrátí logickou hodnotu udávající, pokud typ zadaný výraz je číslo.</span><span class="sxs-lookup"><span data-stu-id="ec9e6-298">Returns a Boolean value indicating if the type of the specified expression is a number.</span></span> |
| <span data-ttu-id="ec9e6-299">IS_OBJECT</span><span class="sxs-lookup"><span data-stu-id="ec9e6-299">IS_OBJECT</span></span> | <span data-ttu-id="ec9e6-300">Vrátí logickou hodnotu udávající, pokud typ zadaný výraz je objekt JSON.</span><span class="sxs-lookup"><span data-stu-id="ec9e6-300">Returns a Boolean value indicating if the type of the specified expression is a JSON object.</span></span> |
| <span data-ttu-id="ec9e6-301">IS_PRIMITIVE</span><span class="sxs-lookup"><span data-stu-id="ec9e6-301">IS_PRIMITIVE</span></span> | <span data-ttu-id="ec9e6-302">Vrátí logickou hodnotu udávající, pokud není typ zadaného výrazu jednoduchého typu (řetězec, logická hodnota, číselná nebo `null`).</span><span class="sxs-lookup"><span data-stu-id="ec9e6-302">Returns a Boolean value indicating if the type of the specified expression is a primitive (string, Boolean, numeric or `null`).</span></span> |
| <span data-ttu-id="ec9e6-303">IS_STRING</span><span class="sxs-lookup"><span data-stu-id="ec9e6-303">IS_STRING</span></span> | <span data-ttu-id="ec9e6-304">Vrátí logickou hodnotu udávající, pokud typ zadaný výraz obsahuje řetězec.</span><span class="sxs-lookup"><span data-stu-id="ec9e6-304">Returns a Boolean value indicating if the type of the specified expression is a string.</span></span> |

<span data-ttu-id="ec9e6-305">V podmínkách trasy jsou podporovány následující funkce řetězce:</span><span class="sxs-lookup"><span data-stu-id="ec9e6-305">In routes conditions, the following string functions are supported:</span></span>

| <span data-ttu-id="ec9e6-306">Funkce</span><span class="sxs-lookup"><span data-stu-id="ec9e6-306">Function</span></span> | <span data-ttu-id="ec9e6-307">Popis</span><span class="sxs-lookup"><span data-stu-id="ec9e6-307">Description</span></span> |
| -------- | ----------- |
| <span data-ttu-id="ec9e6-308">CONCAT(x,...)</span><span class="sxs-lookup"><span data-stu-id="ec9e6-308">CONCAT(x, …)</span></span> | <span data-ttu-id="ec9e6-309">Vrátí řetězec, který je výsledkem zřetězení dvou nebo více řetězcové hodnoty.</span><span class="sxs-lookup"><span data-stu-id="ec9e6-309">Returns a string that is the result of concatenating two or more string values.</span></span> |
| <span data-ttu-id="ec9e6-310">Length(x)</span><span class="sxs-lookup"><span data-stu-id="ec9e6-310">LENGTH(x)</span></span> | <span data-ttu-id="ec9e6-311">Vrátí počet znaků ze zadaného řetězcového výrazu.</span><span class="sxs-lookup"><span data-stu-id="ec9e6-311">Returns the number of characters of the specified string expression.</span></span>|
| <span data-ttu-id="ec9e6-312">LOWER(x)</span><span class="sxs-lookup"><span data-stu-id="ec9e6-312">LOWER(x)</span></span> | <span data-ttu-id="ec9e6-313">Vrací výraz řetězce po převodu dat velké písmeno na malá písmena.</span><span class="sxs-lookup"><span data-stu-id="ec9e6-313">Returns a string expression after converting uppercase character data to lowercase.</span></span> |
| <span data-ttu-id="ec9e6-314">UPPER(x)</span><span class="sxs-lookup"><span data-stu-id="ec9e6-314">UPPER(x)</span></span> | <span data-ttu-id="ec9e6-315">Vrací výraz řetězce po převodu dat malé písmeno na velká písmena.</span><span class="sxs-lookup"><span data-stu-id="ec9e6-315">Returns a string expression after converting lowercase character data to uppercase.</span></span> |
| <span data-ttu-id="ec9e6-316">Dílčí řetězec (string, spuštění [, délka])</span><span class="sxs-lookup"><span data-stu-id="ec9e6-316">SUBSTRING(string, start [, length])</span></span> | <span data-ttu-id="ec9e6-317">Vrátí část řetězcového výrazu od pozice s nulovým základem zadaný znak a pokračuje na určenou délku nebo na konci řetězce.</span><span class="sxs-lookup"><span data-stu-id="ec9e6-317">Returns part of a string expression starting at the specified character zero-based position and continues to the specified length, or to the end of the string.</span></span> |
| <span data-ttu-id="ec9e6-318">INDEX_OF (řetězec, fragment)</span><span class="sxs-lookup"><span data-stu-id="ec9e6-318">INDEX_OF(string, fragment)</span></span> | <span data-ttu-id="ec9e6-319">Vrátí počáteční pozici prvního výskytu druhý řetězec výrazu v rámci první zadaného řetězcového výrazu nebo -1, pokud není nalezen řetězec.</span><span class="sxs-lookup"><span data-stu-id="ec9e6-319">Returns the starting position of the first occurrence of the second string expression within the first specified string expression, or -1 if the string is not found.</span></span>|
| <span data-ttu-id="ec9e6-320">STARTS_WITH (x, y)</span><span class="sxs-lookup"><span data-stu-id="ec9e6-320">STARTS_WITH(x, y)</span></span> | <span data-ttu-id="ec9e6-321">Vrátí logická hodnota, která určuje zda první řetězec výraz začíná druhý.</span><span class="sxs-lookup"><span data-stu-id="ec9e6-321">Returns a Boolean indicating whether the first string expression starts with the second.</span></span> |
| <span data-ttu-id="ec9e6-322">ENDS_WITH (x, y)</span><span class="sxs-lookup"><span data-stu-id="ec9e6-322">ENDS_WITH(x, y)</span></span> | <span data-ttu-id="ec9e6-323">Vrátí logická hodnota, která určuje zda první řetězec výraz končí druhý.</span><span class="sxs-lookup"><span data-stu-id="ec9e6-323">Returns a Boolean indicating whether the first string expression ends with the second.</span></span> |
| <span data-ttu-id="ec9e6-324">CONTAINS(x,y)</span><span class="sxs-lookup"><span data-stu-id="ec9e6-324">CONTAINS(x,y)</span></span> | <span data-ttu-id="ec9e6-325">Vrátí logická hodnota, která určuje zda první řetězec výraz obsahuje druhý.</span><span class="sxs-lookup"><span data-stu-id="ec9e6-325">Returns a Boolean indicating whether the first string expression contains the second.</span></span> |

## <a name="next-steps"></a><span data-ttu-id="ec9e6-326">Další kroky</span><span class="sxs-lookup"><span data-stu-id="ec9e6-326">Next steps</span></span>
<span data-ttu-id="ec9e6-327">Zjistěte, jak na provedení dotazů ve svých aplikacích pomocí [SDK služby Azure IoT][lnk-hub-sdks].</span><span class="sxs-lookup"><span data-stu-id="ec9e6-327">Learn how to execute queries in your apps using [Azure IoT SDKs][lnk-hub-sdks].</span></span>

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
