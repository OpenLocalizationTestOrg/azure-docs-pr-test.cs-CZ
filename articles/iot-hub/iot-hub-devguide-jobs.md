---
title: "úlohy Azure IoT Hub aaaUnderstand | Microsoft Docs"
description: "Příručka vývojáře - plánování úloh toorun na několika zařízeních připojen tooyour IoT hub. Úlohy můžete aktualizovat značky a požadované vlastnosti a volat přímé metody na několika zařízeních."
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
ms.openlocfilehash: 8be134e6c379feae5087df8f562a74505c57afee
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="schedule-jobs-on-multiple-devices"></a><span data-ttu-id="c6b78-104">Plánování úloh na několika zařízeních</span><span class="sxs-lookup"><span data-stu-id="c6b78-104">Schedule jobs on multiple devices</span></span>
## <a name="overview"></a><span data-ttu-id="c6b78-105">Přehled</span><span class="sxs-lookup"><span data-stu-id="c6b78-105">Overview</span></span>
<span data-ttu-id="c6b78-106">Popsané v předchozí články Azure IoT Hub umožňuje počet stavební bloky ([zařízení twin vlastnosti a značky] [ lnk-twin-devguide] a [přímé metody] [ lnk-dev-methods]).</span><span class="sxs-lookup"><span data-stu-id="c6b78-106">As described by previous articles, Azure IoT Hub enables a number of building blocks ([device twin properties and tags][lnk-twin-devguide] and [direct methods][lnk-dev-methods]).</span></span>  <span data-ttu-id="c6b78-107">Back-end aplikace obvykle povolit tooupdate operátory a Správci zařízení a interagovat se zařízení IoT hromadně a v naplánovaném čase.</span><span class="sxs-lookup"><span data-stu-id="c6b78-107">Typically, back-end apps enable device administrators and operators tooupdate and interact with IoT devices in bulk and at a scheduled time.</span></span>  <span data-ttu-id="c6b78-108">Úlohy pro zapouzdření hello provádění aktualizací twin zařízení a přímé metody oproti sadě zařízení současně plán.</span><span class="sxs-lookup"><span data-stu-id="c6b78-108">Jobs encapsulate hello execution of device twin updates and direct methods against a set of devices at a schedule time.</span></span>  <span data-ttu-id="c6b78-109">Operátor byste například použili back-end aplikaci, která by zahájení a sledovat úlohy tooreboot skupiny zařízení při vytváření 43 a podlaží 3 v čase, který nebude rušivý toohello operace vytváření hello.</span><span class="sxs-lookup"><span data-stu-id="c6b78-109">For example, an operator would use a back-end app that would initiate and track a job tooreboot a set of devices in building 43 and floor 3 at a time that would not be disruptive toohello operations of hello building.</span></span>

### <a name="when-toouse"></a><span data-ttu-id="c6b78-110">Když toouse</span><span class="sxs-lookup"><span data-stu-id="c6b78-110">When toouse</span></span>
<span data-ttu-id="c6b78-111">Zvažte použití úlohy při: tooschedule potřebám back-end řešení a sledování průběhu některé z následujících aktivit na sadu zařízení hello:</span><span class="sxs-lookup"><span data-stu-id="c6b78-111">Consider using jobs when: a solution back end needs tooschedule and track progress any of hello following activities on a set of device:</span></span>

* <span data-ttu-id="c6b78-112">Aktualizace požadovaných vlastností</span><span class="sxs-lookup"><span data-stu-id="c6b78-112">Update desired properties</span></span>
* <span data-ttu-id="c6b78-113">Aktualizace značky</span><span class="sxs-lookup"><span data-stu-id="c6b78-113">Update tags</span></span>
* <span data-ttu-id="c6b78-114">Vyvolání metody přímé</span><span class="sxs-lookup"><span data-stu-id="c6b78-114">Invoke direct methods</span></span>

## <a name="job-lifecycle"></a><span data-ttu-id="c6b78-115">Životní cyklus úlohy</span><span class="sxs-lookup"><span data-stu-id="c6b78-115">Job lifecycle</span></span>
<span data-ttu-id="c6b78-116">Úlohy jsou iniciovaná back-end hello řešení a udržované pomocí služby IoT Hub.</span><span class="sxs-lookup"><span data-stu-id="c6b78-116">Jobs are initiated by hello solution back end and maintained by IoT Hub.</span></span>  <span data-ttu-id="c6b78-117">Můžete spustit úlohu pomocí identifikátoru URI služby přístupem (`{iot hub}/jobs/v2/{device id}/methods/<jobID>?api-version=2016-11-14`) a dotaz na průběh na úlohu prováděné prostřednictvím identifikátoru URI služby přístupem (`{iot hub}/jobs/v2/<jobId>?api-version=2016-11-14`).</span><span class="sxs-lookup"><span data-stu-id="c6b78-117">You can initiate a job through a service-facing URI (`{iot hub}/jobs/v2/{device id}/methods/<jobID>?api-version=2016-11-14`) and query for progress on an executing job through a service-facing URI (`{iot hub}/jobs/v2/<jobId>?api-version=2016-11-14`).</span></span>  <span data-ttu-id="c6b78-118">Jakmile je zahájena úlohu, a umožňuje dotazování pro úlohy hello back-end aplikačním toorefresh hello stavu spuštěných úloh.</span><span class="sxs-lookup"><span data-stu-id="c6b78-118">Once a job is initiated, querying for jobs enables hello back-end app toorefresh hello status of running jobs.</span></span>

> [!NOTE]
> <span data-ttu-id="c6b78-119">Při zahájení úlohy, názvů a hodnot vlastností mohou obsahovat pouze US-ASCII tisknutelná alfanumerické znaky, s výjimkou těch hello následující sady: ``{'$', '(', ')', '<', '>', '@', ',', ';', ':', '\', '"', '/', '[', ']', '?', '=', '{', '}', SP, HT}``.</span><span class="sxs-lookup"><span data-stu-id="c6b78-119">When you initiate a job, property names and values can only contain US-ASCII printable alphanumeric, except any in hello following set: ``{'$', '(', ')', '<', '>', '@', ',', ';', ':', '\', '"', '/', '[', ']', '?', '=', '{', '}', SP, HT}``.</span></span>
> 
> 

## <a name="reference-topics"></a><span data-ttu-id="c6b78-120">Témata odkazů:</span><span class="sxs-lookup"><span data-stu-id="c6b78-120">Reference topics:</span></span>
<span data-ttu-id="c6b78-121">Následující témata s referenčními informacemi Hello poskytují další informace o používání úlohy.</span><span class="sxs-lookup"><span data-stu-id="c6b78-121">hello following reference topics provide you with more information about using jobs.</span></span>

## <a name="jobs-tooexecute-direct-methods"></a><span data-ttu-id="c6b78-122">Přímé metody tooexecute úlohy</span><span class="sxs-lookup"><span data-stu-id="c6b78-122">Jobs tooexecute direct methods</span></span>
<span data-ttu-id="c6b78-123">Hello následuje podrobnosti požadavku hello HTTP 1.1 pro provádění [přímá metoda] [ lnk-dev-methods] na sadu zařízení pomocí úlohy:</span><span class="sxs-lookup"><span data-stu-id="c6b78-123">hello following is hello HTTP 1.1 request details for executing a [direct method][lnk-dev-methods] on a set of devices using a job:</span></span>

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
<span data-ttu-id="c6b78-124">Podmínka Hello dotazu může být také na jedno zařízení Id nebo na seznam identifikátorů zařízení jak je uvedeno níže</span><span class="sxs-lookup"><span data-stu-id="c6b78-124">hello query condition can also be on a single device Id or on a list of device Ids as shown below</span></span>

<span data-ttu-id="c6b78-125">**Příklady**</span><span class="sxs-lookup"><span data-stu-id="c6b78-125">**Examples**</span></span>
```
queryCondition = "deviceId = 'MyDevice1'"
queryCondition = "deviceId IN ['MyDevice1','MyDevice2']"
queryCondition = "deviceId IN ['MyDevice1']
```
<span data-ttu-id="c6b78-126">[IoT Hub dotazovací jazyk] [ lnk-query] obsahuje dotazovací jazyk IoT Hub v další podrobnosti.</span><span class="sxs-lookup"><span data-stu-id="c6b78-126">[IoT Hub Query Language][lnk-query] covers IoT Hub query language in additional detail.</span></span>

## <a name="jobs-tooupdate-device-twin-properties"></a><span data-ttu-id="c6b78-127">Vlastnosti úlohy tooupdate zařízení twin</span><span class="sxs-lookup"><span data-stu-id="c6b78-127">Jobs tooupdate device twin properties</span></span>
<span data-ttu-id="c6b78-128">Hello následuje podrobnosti požadavku hello HTTP 1.1 pro aktualizace zařízení dvojici vlastností pomocí úlohy:</span><span class="sxs-lookup"><span data-stu-id="c6b78-128">hello following is hello HTTP 1.1 request details for updating device twin properties using a job:</span></span>

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

## <a name="querying-for-progress-on-jobs"></a><span data-ttu-id="c6b78-129">Dotaz na průběh na úlohách</span><span class="sxs-lookup"><span data-stu-id="c6b78-129">Querying for progress on jobs</span></span>
<span data-ttu-id="c6b78-130">Hello následuje hello podrobnosti požadavku HTTP 1.1 pro [dotazování pro úlohy][lnk-query]:</span><span class="sxs-lookup"><span data-stu-id="c6b78-130">hello following is hello HTTP 1.1 request details for [querying for jobs][lnk-query]:</span></span>

    ```
    GET /jobs/v2/query?api-version=2016-11-14[&jobType=<jobType>][&jobStatus=<jobStatus>][&pageSize=<pageSize>][&continuationToken=<continuationToken>]

    Authorization: <config.sharedAccessSignature>
    Content-Type: application/json; charset=utf-8
    Request-Id: <guid>
    User-Agent: <sdk-name>/<sdk-version>
    ```

<span data-ttu-id="c6b78-131">z odpovědi hello je k dispozici Hello continuationToken.</span><span class="sxs-lookup"><span data-stu-id="c6b78-131">hello continuationToken is provided from hello response.</span></span>  

## <a name="jobs-properties"></a><span data-ttu-id="c6b78-132">Vlastnosti úlohy</span><span class="sxs-lookup"><span data-stu-id="c6b78-132">Jobs Properties</span></span>
<span data-ttu-id="c6b78-133">Hello následuje seznam vlastností a odpovídající popisy, které se dají použít při dotazování pro úlohy nebo výsledky úlohy.</span><span class="sxs-lookup"><span data-stu-id="c6b78-133">hello following is a list of properties and corresponding descriptions, which can be used when querying for jobs or job results.</span></span>

| <span data-ttu-id="c6b78-134">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="c6b78-134">Property</span></span> | <span data-ttu-id="c6b78-135">Popis</span><span class="sxs-lookup"><span data-stu-id="c6b78-135">Description</span></span> |
| --- | --- |
| <span data-ttu-id="c6b78-136">**jobId**</span><span class="sxs-lookup"><span data-stu-id="c6b78-136">**jobId**</span></span> |<span data-ttu-id="c6b78-137">Aplikace zadat ID pro úlohu hello.</span><span class="sxs-lookup"><span data-stu-id="c6b78-137">Application provided ID for hello job.</span></span> |
| <span data-ttu-id="c6b78-138">**čas spuštění**</span><span class="sxs-lookup"><span data-stu-id="c6b78-138">**startTime**</span></span> |<span data-ttu-id="c6b78-139">Aplikace zadat čas spuštění (ISO-8601) pro úlohu hello.</span><span class="sxs-lookup"><span data-stu-id="c6b78-139">Application provided start time (ISO-8601) for hello job.</span></span> |
| <span data-ttu-id="c6b78-140">**čas ukončení**</span><span class="sxs-lookup"><span data-stu-id="c6b78-140">**endTime**</span></span> |<span data-ttu-id="c6b78-141">Po dokončení úlohy hello IoT Hub zadat datum (ISO-8601) pro.</span><span class="sxs-lookup"><span data-stu-id="c6b78-141">IoT Hub provided date (ISO-8601) for when hello job completed.</span></span> <span data-ttu-id="c6b78-142">Platné jenom po hello úlohy dosáhne stavu hello 'Dokončit'.</span><span class="sxs-lookup"><span data-stu-id="c6b78-142">Valid only after hello job reaches hello 'completed' state.</span></span> |
| <span data-ttu-id="c6b78-143">**Typ**</span><span class="sxs-lookup"><span data-stu-id="c6b78-143">**type**</span></span> |<span data-ttu-id="c6b78-144">Typy úloh:</span><span class="sxs-lookup"><span data-stu-id="c6b78-144">Types of jobs:</span></span> |
| <span data-ttu-id="c6b78-145">**scheduledUpdateTwin**: úloha používá tooupdate sadu požadované vlastnosti a značky.</span><span class="sxs-lookup"><span data-stu-id="c6b78-145">**scheduledUpdateTwin**: A job used tooupdate a set of desired properties or tags.</span></span> | |
| <span data-ttu-id="c6b78-146">**scheduledDeviceMethod**: úloha používá tooinvoke metoda zařízení na sadu dvojčata zařízení.</span><span class="sxs-lookup"><span data-stu-id="c6b78-146">**scheduledDeviceMethod**: A job used tooinvoke a device method on a set of device twins.</span></span> | |
| <span data-ttu-id="c6b78-147">**Stav**</span><span class="sxs-lookup"><span data-stu-id="c6b78-147">**status**</span></span> |<span data-ttu-id="c6b78-148">Aktuální stav úlohy hello.</span><span class="sxs-lookup"><span data-stu-id="c6b78-148">Current state of hello job.</span></span> <span data-ttu-id="c6b78-149">Možné hodnoty pro stav:</span><span class="sxs-lookup"><span data-stu-id="c6b78-149">Possible values for status:</span></span> |
| <span data-ttu-id="c6b78-150">**Čekající** : naplánováno a čekací toobe zachyceny službou hello úlohy.</span><span class="sxs-lookup"><span data-stu-id="c6b78-150">**pending** : Scheduled and waiting toobe picked up by hello job service.</span></span> | |
| <span data-ttu-id="c6b78-151">**naplánované** : naplánováno na dobu v budoucnu hello.</span><span class="sxs-lookup"><span data-stu-id="c6b78-151">**scheduled** : Scheduled for a time in hello future.</span></span> | |
| <span data-ttu-id="c6b78-152">**spuštění** : aktuálně aktivní úlohy.</span><span class="sxs-lookup"><span data-stu-id="c6b78-152">**running** : Currently active job.</span></span> | |
| <span data-ttu-id="c6b78-153">**zrušena** : Úloha byla zrušena.</span><span class="sxs-lookup"><span data-stu-id="c6b78-153">**cancelled** : Job has been cancelled.</span></span> | |
| <span data-ttu-id="c6b78-154">**se nezdařilo** : Nepodařilo se provést úlohu.</span><span class="sxs-lookup"><span data-stu-id="c6b78-154">**failed** : Job failed.</span></span> | |
| <span data-ttu-id="c6b78-155">**Dokončit** : Úloha byla dokončena.</span><span class="sxs-lookup"><span data-stu-id="c6b78-155">**completed** : Job has completed.</span></span> | |
| <span data-ttu-id="c6b78-156">**deviceJobStatistics**</span><span class="sxs-lookup"><span data-stu-id="c6b78-156">**deviceJobStatistics**</span></span> |<span data-ttu-id="c6b78-157">Statistika týkající se provádění úlohy hello.</span><span class="sxs-lookup"><span data-stu-id="c6b78-157">Statistics about hello job's execution.</span></span> |

<span data-ttu-id="c6b78-158">**deviceJobStatistics** vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="c6b78-158">**deviceJobStatistics** properties.</span></span>

| <span data-ttu-id="c6b78-159">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="c6b78-159">Property</span></span> | <span data-ttu-id="c6b78-160">Popis</span><span class="sxs-lookup"><span data-stu-id="c6b78-160">Description</span></span> |
| --- | --- |
| <span data-ttu-id="c6b78-161">**deviceJobStatistics.deviceCount**</span><span class="sxs-lookup"><span data-stu-id="c6b78-161">**deviceJobStatistics.deviceCount**</span></span> |<span data-ttu-id="c6b78-162">Počet zařízení v úloze hello.</span><span class="sxs-lookup"><span data-stu-id="c6b78-162">Number of devices in hello job.</span></span> |
| <span data-ttu-id="c6b78-163">**deviceJobStatistics.failedCount**</span><span class="sxs-lookup"><span data-stu-id="c6b78-163">**deviceJobStatistics.failedCount**</span></span> |<span data-ttu-id="c6b78-164">Počet zařízení, kde hello úloha se nezdařila.</span><span class="sxs-lookup"><span data-stu-id="c6b78-164">Number of devices where hello job failed.</span></span> |
| <span data-ttu-id="c6b78-165">**deviceJobStatistics.succeededCount**</span><span class="sxs-lookup"><span data-stu-id="c6b78-165">**deviceJobStatistics.succeededCount**</span></span> |<span data-ttu-id="c6b78-166">Počet zařízení, kde hello úloha úspěšně.</span><span class="sxs-lookup"><span data-stu-id="c6b78-166">Number of devices where hello job succeeded.</span></span> |
| <span data-ttu-id="c6b78-167">**deviceJobStatistics.runningCount**</span><span class="sxs-lookup"><span data-stu-id="c6b78-167">**deviceJobStatistics.runningCount**</span></span> |<span data-ttu-id="c6b78-168">Počet zařízení, které jsou aktuálně spuštěné úlohy hello.</span><span class="sxs-lookup"><span data-stu-id="c6b78-168">Number of devices that are currently running hello job.</span></span> |
| <span data-ttu-id="c6b78-169">**deviceJobStatistics.pendingCount**</span><span class="sxs-lookup"><span data-stu-id="c6b78-169">**deviceJobStatistics.pendingCount**</span></span> |<span data-ttu-id="c6b78-170">Počet zařízení, kterých se čeká na toorun hello úlohy.</span><span class="sxs-lookup"><span data-stu-id="c6b78-170">Number of devices that are pending toorun hello job.</span></span> |

### <a name="additional-reference-material"></a><span data-ttu-id="c6b78-171">Odkaz na další materiály</span><span class="sxs-lookup"><span data-stu-id="c6b78-171">Additional reference material</span></span>
<span data-ttu-id="c6b78-172">Další témata referenční příručka vývojáře pro službu IoT Hub hello patří:</span><span class="sxs-lookup"><span data-stu-id="c6b78-172">Other reference topics in hello IoT Hub developer guide include:</span></span>

* <span data-ttu-id="c6b78-173">[Koncové body centra IoT] [ lnk-endpoints] popisuje hello různých koncových bodů, které každý IoT hub zpřístupní pro spuštění a management operace.</span><span class="sxs-lookup"><span data-stu-id="c6b78-173">[IoT Hub endpoints][lnk-endpoints] describes hello various endpoints that each IoT hub exposes for run-time and management operations.</span></span>
* <span data-ttu-id="c6b78-174">[Omezování a kvóty] [ lnk-quotas] popisuje hello kvóty, které se vztahují toohello služby IoT Hub a hello omezení tooexpect chování při použití služby hello.</span><span class="sxs-lookup"><span data-stu-id="c6b78-174">[Throttling and quotas][lnk-quotas] describes hello quotas that apply toohello IoT Hub service and hello throttling behavior tooexpect when you use hello service.</span></span>
* <span data-ttu-id="c6b78-175">[Azure IoT zařízení a služby sady SDK] [ lnk-sdks] seznamy hello různé jazykové sady SDK můžete použití při vývoji aplikace zařízení a služby, které interakci s centrem IoT.</span><span class="sxs-lookup"><span data-stu-id="c6b78-175">[Azure IoT device and service SDKs][lnk-sdks] lists hello various language SDKs you an use when you develop both device and service apps that interact with IoT Hub.</span></span>
* <span data-ttu-id="c6b78-176">[IoT Hub dotazovacího jazyka pro dvojčata zařízení, úlohy a směrování zpráv] [ lnk-query] popisuje hello dotazovací jazyk IoT Hub můžete tooretrieve informací ze služby IoT Hub o úlohách a dvojčata zařízení.</span><span class="sxs-lookup"><span data-stu-id="c6b78-176">[IoT Hub query language for device twins, jobs, and message routing][lnk-query] describes hello IoT Hub query language you can use tooretrieve information from IoT Hub about your device twins and jobs.</span></span>
* <span data-ttu-id="c6b78-177">[Podpora IoT Hub MQTT] [ lnk-devguide-mqtt] poskytuje další informace o podpoře služby IoT Hub pro protokol MQTT hello.</span><span class="sxs-lookup"><span data-stu-id="c6b78-177">[IoT Hub MQTT support][lnk-devguide-mqtt] provides more information about IoT Hub support for hello MQTT protocol.</span></span>

## <a name="next-steps"></a><span data-ttu-id="c6b78-178">Další kroky</span><span class="sxs-lookup"><span data-stu-id="c6b78-178">Next steps</span></span>
<span data-ttu-id="c6b78-179">Pokud chcete tootry některé z hello konceptů popsaných v tomto článku, může být zájem o hello následující kurzu IoT Hub:</span><span class="sxs-lookup"><span data-stu-id="c6b78-179">If you would like tootry out some of hello concepts described in this article, you may be interested in hello following IoT Hub tutorial:</span></span>

* <span data-ttu-id="c6b78-180">[Plán a všesměrovým úlohy][lnk-jobs-tutorial]</span><span class="sxs-lookup"><span data-stu-id="c6b78-180">[Schedule and broadcast jobs][lnk-jobs-tutorial]</span></span>

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
