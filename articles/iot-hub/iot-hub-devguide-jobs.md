---
title: "Pochopení úlohy Azure IoT Hub | Microsoft Docs"
description: "Příručka vývojáře - plánování úloh spouštět na několika zařízeních připojení do služby IoT hub. Úlohy můžete aktualizovat značky a požadované vlastnosti a volat přímé metody na několika zařízeních."
services: iot-hub
documentationcenter: .net
author: juanjperez
manager: timlt
editor: 
ms.assetid: fe78458f-4f14-4358-ac83-4f7bd14ee8da
ms.service: iot-hub
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 09/30/2016
ms.author: juanpere
ms.openlocfilehash: abb7f80662650efa8f158f32125ebc5350cb4f62
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="schedule-jobs-on-multiple-devices"></a><span data-ttu-id="6a412-104">Plánování úloh na několika zařízeních</span><span class="sxs-lookup"><span data-stu-id="6a412-104">Schedule jobs on multiple devices</span></span>
## <a name="overview"></a><span data-ttu-id="6a412-105">Přehled</span><span class="sxs-lookup"><span data-stu-id="6a412-105">Overview</span></span>
<span data-ttu-id="6a412-106">Popsané v předchozí články Azure IoT Hub umožňuje počet stavební bloky ([zařízení twin vlastnosti a značky] [ lnk-twin-devguide] a [přímé metody] [ lnk-dev-methods]).</span><span class="sxs-lookup"><span data-stu-id="6a412-106">As described by previous articles, Azure IoT Hub enables a number of building blocks ([device twin properties and tags][lnk-twin-devguide] and [direct methods][lnk-dev-methods]).</span></span>  <span data-ttu-id="6a412-107">Back-end aplikace obvykle povolit operátory a Správci zařízení k aktualizaci a komunikovat se zařízeními IoT hromadně a v naplánovaném čase.</span><span class="sxs-lookup"><span data-stu-id="6a412-107">Typically, back-end apps enable device administrators and operators to update and interact with IoT devices in bulk and at a scheduled time.</span></span>  <span data-ttu-id="6a412-108">Úlohy pro zapouzdření provádění aktualizací twin zařízení a přímé metody oproti sadě zařízení současně plán.</span><span class="sxs-lookup"><span data-stu-id="6a412-108">Jobs encapsulate the execution of device twin updates and direct methods against a set of devices at a schedule time.</span></span>  <span data-ttu-id="6a412-109">Operátor byste například použili back-end aplikaci, která by zahájení a sledovat úlohu restartovat sadu zařízení při vytváření 43 a podlaží 3 v čase, který nebude rušivý pro operace vytvoření.</span><span class="sxs-lookup"><span data-stu-id="6a412-109">For example, an operator would use a back-end app that would initiate and track a job to reboot a set of devices in building 43 and floor 3 at a time that would not be disruptive to the operations of the building.</span></span>

### <a name="when-to-use"></a><span data-ttu-id="6a412-110">Kdy je použít</span><span class="sxs-lookup"><span data-stu-id="6a412-110">When to use</span></span>
<span data-ttu-id="6a412-111">Zvažte použití úlohy při: řešení back-end musí plánovat a sledovat průběh některý z následujících aktivit na sadu zařízení:</span><span class="sxs-lookup"><span data-stu-id="6a412-111">Consider using jobs when: a solution back end needs to schedule and track progress any of the following activities on a set of device:</span></span>

* <span data-ttu-id="6a412-112">Aktualizace požadovaných vlastností</span><span class="sxs-lookup"><span data-stu-id="6a412-112">Update desired properties</span></span>
* <span data-ttu-id="6a412-113">Aktualizace značky</span><span class="sxs-lookup"><span data-stu-id="6a412-113">Update tags</span></span>
* <span data-ttu-id="6a412-114">Vyvolání metody přímé</span><span class="sxs-lookup"><span data-stu-id="6a412-114">Invoke direct methods</span></span>

## <a name="job-lifecycle"></a><span data-ttu-id="6a412-115">Životní cyklus úlohy</span><span class="sxs-lookup"><span data-stu-id="6a412-115">Job lifecycle</span></span>
<span data-ttu-id="6a412-116">Úlohy jsou iniciovaná back-end řešení a udržované pomocí služby IoT Hub.</span><span class="sxs-lookup"><span data-stu-id="6a412-116">Jobs are initiated by the solution back end and maintained by IoT Hub.</span></span>  <span data-ttu-id="6a412-117">Můžete spustit úlohu pomocí identifikátoru URI služby přístupem (`{iot hub}/jobs/v2/{device id}/methods/<jobID>?api-version=2016-11-14`) a dotaz na průběh na úlohu prováděné prostřednictvím identifikátoru URI služby přístupem (`{iot hub}/jobs/v2/<jobId>?api-version=2016-11-14`).</span><span class="sxs-lookup"><span data-stu-id="6a412-117">You can initiate a job through a service-facing URI (`{iot hub}/jobs/v2/{device id}/methods/<jobID>?api-version=2016-11-14`) and query for progress on an executing job through a service-facing URI (`{iot hub}/jobs/v2/<jobId>?api-version=2016-11-14`).</span></span>  <span data-ttu-id="6a412-118">Jakmile je spuštění úlohy, dotazování pro úlohy umožňuje back-end aplikačním k aktualizaci stavu spuštěných úloh.</span><span class="sxs-lookup"><span data-stu-id="6a412-118">Once a job is initiated, querying for jobs enables the back-end app to refresh the status of running jobs.</span></span>

> [!NOTE]
> <span data-ttu-id="6a412-119">Při zahájení úlohy, názvů a hodnot vlastností mohou obsahovat pouze US-ASCII tisknutelná alfanumerické znaky, s výjimkou těch následující sady: ``{'$', '(', ')', '<', '>', '@', ',', ';', ':', '\', '"', '/', '[', ']', '?', '=', '{', '}', SP, HT}``.</span><span class="sxs-lookup"><span data-stu-id="6a412-119">When you initiate a job, property names and values can only contain US-ASCII printable alphanumeric, except any in the following set: ``{'$', '(', ')', '<', '>', '@', ',', ';', ':', '\', '"', '/', '[', ']', '?', '=', '{', '}', SP, HT}``.</span></span>
> 
> 

## <a name="reference-topics"></a><span data-ttu-id="6a412-120">Témata odkazů:</span><span class="sxs-lookup"><span data-stu-id="6a412-120">Reference topics:</span></span>
<span data-ttu-id="6a412-121">Následující referenční témata poskytují další informace o používání úlohy.</span><span class="sxs-lookup"><span data-stu-id="6a412-121">The following reference topics provide you with more information about using jobs.</span></span>

## <a name="jobs-to-execute-direct-methods"></a><span data-ttu-id="6a412-122">Úlohy provést přímý metody</span><span class="sxs-lookup"><span data-stu-id="6a412-122">Jobs to execute direct methods</span></span>
<span data-ttu-id="6a412-123">Toto je podrobnosti požadavku HTTP 1.1 pro provádění [přímá metoda] [ lnk-dev-methods] na sadu zařízení pomocí úlohy:</span><span class="sxs-lookup"><span data-stu-id="6a412-123">The following is the HTTP 1.1 request details for executing a [direct method][lnk-dev-methods] on a set of devices using a job:</span></span>

    ```
    PUT /jobs/v2/<jobId>?api-version=2016-11-14

    Authorization: <config.sharedAccessSignature>
    Content-Type: application/json; charset=utf-8
    Request-Id: <guid>
    User-Agent: <sdk-name>/<sdk-version>

    {
        jobId: '<jobId>',
        type: 'scheduleDirectRequest', 
        cloudToDeviceMethod: {
            methodName: '<methodName>',
            payload: <payload>,                 
            responseTimeoutInSeconds: methodTimeoutInSeconds 
        },
        queryCondition: '<queryOrDevices>', // query condition
        startTime: <jobStartTime>,          // as an ISO-8601 date string
        maxExecutionTimeInSeconds: <maxExecutionTimeInSeconds>        
    }
    ```
<span data-ttu-id="6a412-124">Podmínka dotazu může být také na jedno zařízení Id nebo seznam ID, jak je uvedeno níže zařízení</span><span class="sxs-lookup"><span data-stu-id="6a412-124">The query condition can also be on a single device Id or on a list of device Ids as shown below</span></span>

<span data-ttu-id="6a412-125">**Příklady**</span><span class="sxs-lookup"><span data-stu-id="6a412-125">**Examples**</span></span>
```
queryCondition = "deviceId = 'MyDevice1'"
queryCondition = "deviceId IN ['MyDevice1','MyDevice2']"
queryCondition = "deviceId IN ['MyDevice1']
```
<span data-ttu-id="6a412-126">[IoT Hub dotazovací jazyk] [ lnk-query] obsahuje dotazovací jazyk IoT Hub v další podrobnosti.</span><span class="sxs-lookup"><span data-stu-id="6a412-126">[IoT Hub Query Language][lnk-query] covers IoT Hub query language in additional detail.</span></span>

## <a name="jobs-to-update-device-twin-properties"></a><span data-ttu-id="6a412-127">Úlohy aktualizace twin vlastnosti zařízení</span><span class="sxs-lookup"><span data-stu-id="6a412-127">Jobs to update device twin properties</span></span>
<span data-ttu-id="6a412-128">Toto je podrobnosti požadavku HTTP 1.1 pro aktualizace zařízení dvojici vlastností pomocí úlohy:</span><span class="sxs-lookup"><span data-stu-id="6a412-128">The following is the HTTP 1.1 request details for updating device twin properties using a job:</span></span>

    ```
    PUT /jobs/v2/<jobId>?api-version=2016-11-14
    Authorization: <config.sharedAccessSignature>
    Content-Type: application/json; charset=utf-8
    Request-Id: <guid>
    User-Agent: <sdk-name>/<sdk-version>

    {
        jobId: '<jobId>',
        type: 'scheduleTwinUpdate', 
        updateTwin: <patch>                 // Valid JSON object
        queryCondition: '<queryOrDevices>', // query condition
        startTime: <jobStartTime>,          // as an ISO-8601 date string
        maxExecutionTimeInSeconds: <maxExecutionTimeInSeconds>        // format TBD
    }
    ```

## <a name="querying-for-progress-on-jobs"></a><span data-ttu-id="6a412-129">Dotaz na průběh na úlohách</span><span class="sxs-lookup"><span data-stu-id="6a412-129">Querying for progress on jobs</span></span>
<span data-ttu-id="6a412-130">Toto je podrobnosti požadavku HTTP 1.1 pro [dotazování pro úlohy][lnk-query]:</span><span class="sxs-lookup"><span data-stu-id="6a412-130">The following is the HTTP 1.1 request details for [querying for jobs][lnk-query]:</span></span>

    ```
    GET /jobs/v2/query?api-version=2016-11-14[&jobType=<jobType>][&jobStatus=<jobStatus>][&pageSize=<pageSize>][&continuationToken=<continuationToken>]

    Authorization: <config.sharedAccessSignature>
    Content-Type: application/json; charset=utf-8
    Request-Id: <guid>
    User-Agent: <sdk-name>/<sdk-version>
    ```

<span data-ttu-id="6a412-131">Je k dispozici continuationToken z odpovědi.</span><span class="sxs-lookup"><span data-stu-id="6a412-131">The continuationToken is provided from the response.</span></span>  

## <a name="jobs-properties"></a><span data-ttu-id="6a412-132">Vlastnosti úlohy</span><span class="sxs-lookup"><span data-stu-id="6a412-132">Jobs Properties</span></span>
<span data-ttu-id="6a412-133">Následuje seznam vlastností a odpovídající popisy, které se dají použít při dotazování pro úlohy nebo výsledky úlohy.</span><span class="sxs-lookup"><span data-stu-id="6a412-133">The following is a list of properties and corresponding descriptions, which can be used when querying for jobs or job results.</span></span>

| <span data-ttu-id="6a412-134">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="6a412-134">Property</span></span> | <span data-ttu-id="6a412-135">Popis</span><span class="sxs-lookup"><span data-stu-id="6a412-135">Description</span></span> |
| --- | --- |
| <span data-ttu-id="6a412-136">**jobId**</span><span class="sxs-lookup"><span data-stu-id="6a412-136">**jobId**</span></span> |<span data-ttu-id="6a412-137">Aplikace zadat ID pro úlohu.</span><span class="sxs-lookup"><span data-stu-id="6a412-137">Application provided ID for the job.</span></span> |
| <span data-ttu-id="6a412-138">**čas spuštění**</span><span class="sxs-lookup"><span data-stu-id="6a412-138">**startTime**</span></span> |<span data-ttu-id="6a412-139">Aplikace zadat čas spuštění (ISO-8601) pro úlohu.</span><span class="sxs-lookup"><span data-stu-id="6a412-139">Application provided start time (ISO-8601) for the job.</span></span> |
| <span data-ttu-id="6a412-140">**čas ukončení**</span><span class="sxs-lookup"><span data-stu-id="6a412-140">**endTime**</span></span> |<span data-ttu-id="6a412-141">Centrum IoT k dispozici po dokončení úlohy datum (ISO-8601).</span><span class="sxs-lookup"><span data-stu-id="6a412-141">IoT Hub provided date (ISO-8601) for when the job completed.</span></span> <span data-ttu-id="6a412-142">Platné jenom po úlohu dosáhne stavu 'dokončení'.</span><span class="sxs-lookup"><span data-stu-id="6a412-142">Valid only after the job reaches the 'completed' state.</span></span> |
| <span data-ttu-id="6a412-143">**Typ**</span><span class="sxs-lookup"><span data-stu-id="6a412-143">**type**</span></span> |<span data-ttu-id="6a412-144">Typy úloh:</span><span class="sxs-lookup"><span data-stu-id="6a412-144">Types of jobs:</span></span> |
| <span data-ttu-id="6a412-145">**scheduledUpdateTwin**: úlohu použít k aktualizaci sadu požadované vlastnosti a značky.</span><span class="sxs-lookup"><span data-stu-id="6a412-145">**scheduledUpdateTwin**: A job used to update a set of desired properties or tags.</span></span> | |
| <span data-ttu-id="6a412-146">**scheduledDeviceMethod**: úloha používá k volání metody zařízení na sadu dvojčata zařízení.</span><span class="sxs-lookup"><span data-stu-id="6a412-146">**scheduledDeviceMethod**: A job used to invoke a device method on a set of device twins.</span></span> | |
| <span data-ttu-id="6a412-147">**Stav**</span><span class="sxs-lookup"><span data-stu-id="6a412-147">**status**</span></span> |<span data-ttu-id="6a412-148">Aktuální stav úlohy.</span><span class="sxs-lookup"><span data-stu-id="6a412-148">Current state of the job.</span></span> <span data-ttu-id="6a412-149">Možné hodnoty pro stav:</span><span class="sxs-lookup"><span data-stu-id="6a412-149">Possible values for status:</span></span> |
| <span data-ttu-id="6a412-150">**Čekající** : naplánováno a čeká se na být zachyceny pomocí služby úlohy.</span><span class="sxs-lookup"><span data-stu-id="6a412-150">**pending** : Scheduled and waiting to be picked up by the job service.</span></span> | |
| <span data-ttu-id="6a412-151">**naplánované** : naplánováno na dobu v budoucnu.</span><span class="sxs-lookup"><span data-stu-id="6a412-151">**scheduled** : Scheduled for a time in the future.</span></span> | |
| <span data-ttu-id="6a412-152">**spuštění** : aktuálně aktivní úlohy.</span><span class="sxs-lookup"><span data-stu-id="6a412-152">**running** : Currently active job.</span></span> | |
| <span data-ttu-id="6a412-153">**zrušena** : Úloha byla zrušena.</span><span class="sxs-lookup"><span data-stu-id="6a412-153">**cancelled** : Job has been cancelled.</span></span> | |
| <span data-ttu-id="6a412-154">**se nezdařilo** : Nepodařilo se provést úlohu.</span><span class="sxs-lookup"><span data-stu-id="6a412-154">**failed** : Job failed.</span></span> | |
| <span data-ttu-id="6a412-155">**Dokončit** : Úloha byla dokončena.</span><span class="sxs-lookup"><span data-stu-id="6a412-155">**completed** : Job has completed.</span></span> | |
| <span data-ttu-id="6a412-156">**deviceJobStatistics**</span><span class="sxs-lookup"><span data-stu-id="6a412-156">**deviceJobStatistics**</span></span> |<span data-ttu-id="6a412-157">Statistika týkající se spouštění úlohy.</span><span class="sxs-lookup"><span data-stu-id="6a412-157">Statistics about the job's execution.</span></span> |

<span data-ttu-id="6a412-158">**deviceJobStatistics** vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="6a412-158">**deviceJobStatistics** properties.</span></span>

| <span data-ttu-id="6a412-159">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="6a412-159">Property</span></span> | <span data-ttu-id="6a412-160">Popis</span><span class="sxs-lookup"><span data-stu-id="6a412-160">Description</span></span> |
| --- | --- |
| <span data-ttu-id="6a412-161">**deviceJobStatistics.deviceCount**</span><span class="sxs-lookup"><span data-stu-id="6a412-161">**deviceJobStatistics.deviceCount**</span></span> |<span data-ttu-id="6a412-162">Počet zařízení pro úlohu.</span><span class="sxs-lookup"><span data-stu-id="6a412-162">Number of devices in the job.</span></span> |
| <span data-ttu-id="6a412-163">**deviceJobStatistics.failedCount**</span><span class="sxs-lookup"><span data-stu-id="6a412-163">**deviceJobStatistics.failedCount**</span></span> |<span data-ttu-id="6a412-164">Počet zařízení, kde úloha se nezdařila.</span><span class="sxs-lookup"><span data-stu-id="6a412-164">Number of devices where the job failed.</span></span> |
| <span data-ttu-id="6a412-165">**deviceJobStatistics.succeededCount**</span><span class="sxs-lookup"><span data-stu-id="6a412-165">**deviceJobStatistics.succeededCount**</span></span> |<span data-ttu-id="6a412-166">Počet zařízení, kde byla úloha úspěšná.</span><span class="sxs-lookup"><span data-stu-id="6a412-166">Number of devices where the job succeeded.</span></span> |
| <span data-ttu-id="6a412-167">**deviceJobStatistics.runningCount**</span><span class="sxs-lookup"><span data-stu-id="6a412-167">**deviceJobStatistics.runningCount**</span></span> |<span data-ttu-id="6a412-168">Počet zařízení, které jsou aktuálně spuštěné úlohy.</span><span class="sxs-lookup"><span data-stu-id="6a412-168">Number of devices that are currently running the job.</span></span> |
| <span data-ttu-id="6a412-169">**deviceJobStatistics.pendingCount**</span><span class="sxs-lookup"><span data-stu-id="6a412-169">**deviceJobStatistics.pendingCount**</span></span> |<span data-ttu-id="6a412-170">Počet zařízení, která čekají na vyřízení. Pokud chcete spustit úlohu.</span><span class="sxs-lookup"><span data-stu-id="6a412-170">Number of devices that are pending to run the job.</span></span> |

### <a name="additional-reference-material"></a><span data-ttu-id="6a412-171">Odkaz na další materiály</span><span class="sxs-lookup"><span data-stu-id="6a412-171">Additional reference material</span></span>
<span data-ttu-id="6a412-172">Další témata referenční příručka vývojáře IoT Hub patří:</span><span class="sxs-lookup"><span data-stu-id="6a412-172">Other reference topics in the IoT Hub developer guide include:</span></span>

* <span data-ttu-id="6a412-173">[Koncové body centra IoT] [ lnk-endpoints] popisuje různé koncových bodů, které každý IoT hub zpřístupní pro spuštění a management operace.</span><span class="sxs-lookup"><span data-stu-id="6a412-173">[IoT Hub endpoints][lnk-endpoints] describes the various endpoints that each IoT hub exposes for run-time and management operations.</span></span>
* <span data-ttu-id="6a412-174">[Omezování a kvóty] [ lnk-quotas] popisuje kvóty, které platí pro službu IoT Hub a omezení chování se očekává při použití služby.</span><span class="sxs-lookup"><span data-stu-id="6a412-174">[Throttling and quotas][lnk-quotas] describes the quotas that apply to the IoT Hub service and the throttling behavior to expect when you use the service.</span></span>
* <span data-ttu-id="6a412-175">[Azure IoT zařízení a služby sady SDK] [ lnk-sdks] uvádí různé jazykové sady SDK můžete použití při vývoji aplikace zařízení a služby, které interakci s centrem IoT.</span><span class="sxs-lookup"><span data-stu-id="6a412-175">[Azure IoT device and service SDKs][lnk-sdks] lists the various language SDKs you an use when you develop both device and service apps that interact with IoT Hub.</span></span>
* <span data-ttu-id="6a412-176">[IoT Hub dotazovacího jazyka pro dvojčata zařízení, úlohy a směrování zpráv] [ lnk-query] popisuje dotazovací jazyk Centrum IoT, můžete použít k načtení informací ze služby IoT Hub o úlohách a dvojčata zařízení.</span><span class="sxs-lookup"><span data-stu-id="6a412-176">[IoT Hub query language for device twins, jobs, and message routing][lnk-query] describes the IoT Hub query language you can use to retrieve information from IoT Hub about your device twins and jobs.</span></span>
* <span data-ttu-id="6a412-177">[Podpora IoT Hub MQTT] [ lnk-devguide-mqtt] poskytuje další informace o podpoře služby IoT Hub pro protokol MQTT.</span><span class="sxs-lookup"><span data-stu-id="6a412-177">[IoT Hub MQTT support][lnk-devguide-mqtt] provides more information about IoT Hub support for the MQTT protocol.</span></span>

## <a name="next-steps"></a><span data-ttu-id="6a412-178">Další kroky</span><span class="sxs-lookup"><span data-stu-id="6a412-178">Next steps</span></span>
<span data-ttu-id="6a412-179">Pokud chcete vyzkoušet některé konceptů popsaných v tomto článku, může zajímat v následujícím kurzu IoT Hub:</span><span class="sxs-lookup"><span data-stu-id="6a412-179">If you would like to try out some of the concepts described in this article, you may be interested in the following IoT Hub tutorial:</span></span>

* <span data-ttu-id="6a412-180">[Plán a všesměrovým úlohy][lnk-jobs-tutorial]</span><span class="sxs-lookup"><span data-stu-id="6a412-180">[Schedule and broadcast jobs][lnk-jobs-tutorial]</span></span>

<!-- links and images -->

[lnk-endpoints]: iot-hub-devguide-endpoints.md
[lnk-quotas]: iot-hub-devguide-quotas-throttling.md
[lnk-sdks]: iot-hub-devguide-sdks.md
[lnk-query]: iot-hub-devguide-query-language.md
[lnk-devguide-mqtt]: iot-hub-mqtt-support.md
[lnk-jobs-tutorial]: iot-hub-node-node-schedule-jobs.md
[lnk-c2d-methods]: iot-hub-node-node-direct-methods.md
[lnk-dev-methods]: iot-hub-devguide-direct-methods.md
[lnk-get-started-twin]: iot-hub-node-node-twin-getstarted.md
[lnk-twin-devguide]: iot-hub-devguide-device-twins.md
