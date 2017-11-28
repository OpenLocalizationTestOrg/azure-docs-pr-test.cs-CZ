---
title: aaaScheduler koncepty, pojmy a entity | Microsoft Docs
description: "Koncepty, terminologie a hierarchie entit služby Azure Scheduler včetně úloh a kolekcí úloh.  Zobrazí ucelený příklad naplánované úlohy."
services: scheduler
documentationcenter: .NET
author: derek1ee
manager: kevinlam1
editor: 
ms.assetid: 3ef16fab-d18a-48ba-8e56-3f3e0a1bcb92
ms.service: scheduler
ms.workload: infrastructure-services
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: get-started-article
ms.date: 08/18/2016
ms.author: deli
ms.openlocfilehash: 73e7de7bfd2937e401aeab05e0e10fa292cf37b9
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="scheduler-concepts-terminology--entity-hierarchy"></a><span data-ttu-id="525e4-104">Koncepty, terminologie a hierarchie entit Scheduleru</span><span class="sxs-lookup"><span data-stu-id="525e4-104">Scheduler concepts, terminology, + entity hierarchy</span></span>
## <a name="scheduler-entity-hierarchy"></a><span data-ttu-id="525e4-105">Hierarchie entit Scheduleru</span><span class="sxs-lookup"><span data-stu-id="525e4-105">Scheduler entity hierarchy</span></span>
<span data-ttu-id="525e4-106">Hello následující tabulka popisuje hlavní prostředky, které hello vystavuje nebo používá hello API Scheduleru:</span><span class="sxs-lookup"><span data-stu-id="525e4-106">hello following table describes hello main resources exposed or used by hello Scheduler API:</span></span>

| <span data-ttu-id="525e4-107">Prostředek</span><span class="sxs-lookup"><span data-stu-id="525e4-107">Resource</span></span> | <span data-ttu-id="525e4-108">Popis</span><span class="sxs-lookup"><span data-stu-id="525e4-108">Description</span></span> |
| --- | --- |
| <span data-ttu-id="525e4-109">**Kolekce úloh**</span><span class="sxs-lookup"><span data-stu-id="525e4-109">**Job collection**</span></span> |<span data-ttu-id="525e4-110">Kolekce úloh obsahuje skupinu úloh a zachovává nastavení, kvóty a omezení, které jsou sdílené mezi úlohami v kolekci hello.</span><span class="sxs-lookup"><span data-stu-id="525e4-110">A job collection contains a group of jobs and maintains settings, quotas, and throttles that are shared by jobs within hello collection.</span></span> <span data-ttu-id="525e4-111">Kolekci úloh vytvoří vlastník předplatného a seskupuje úlohy podle použití nebo hranic aplikací.</span><span class="sxs-lookup"><span data-stu-id="525e4-111">A job collection is created by a subscription owner and groups jobs together based on usage or application boundaries.</span></span> <span data-ttu-id="525e4-112">Je omezené tooone oblast.</span><span class="sxs-lookup"><span data-stu-id="525e4-112">It’s constrained tooone region.</span></span> <span data-ttu-id="525e4-113">Také umožňuje vynucovat kvóty hello tooconstrain hello používání všech úloh v dané kolekci.</span><span class="sxs-lookup"><span data-stu-id="525e4-113">It also allows hello enforcement of quotas tooconstrain hello usage of all jobs in that collection.</span></span> <span data-ttu-id="525e4-114">Hello kvóty zahrnují hodnoty MaxJobs a MaxRecurrence.</span><span class="sxs-lookup"><span data-stu-id="525e4-114">hello quotas include MaxJobs and MaxRecurrence.</span></span> |
| <span data-ttu-id="525e4-115">**Úloha**</span><span class="sxs-lookup"><span data-stu-id="525e4-115">**Job**</span></span> |<span data-ttu-id="525e4-116">Úloha definuje jednu opakující se akci s jednoduchými nebo komplexními strategiemi provedení.</span><span class="sxs-lookup"><span data-stu-id="525e4-116">A job defines a single recurrent action, with simple or complex strategies for execution.</span></span> <span data-ttu-id="525e4-117">Akce můžou zahrnovat požadavky HTTP, fronty úložiště, fronty sběrnice nebo témata sběrnice.</span><span class="sxs-lookup"><span data-stu-id="525e4-117">Actions may include HTTP, storage queue, service bus queue, or service bus topic requests.</span></span> |
| <span data-ttu-id="525e4-118">**Historie úlohy**</span><span class="sxs-lookup"><span data-stu-id="525e4-118">**Job history**</span></span> |<span data-ttu-id="525e4-119">Historie úlohy obsahuje podrobnosti o provedení úlohy.</span><span class="sxs-lookup"><span data-stu-id="525e4-119">A job history represents details for an execution of a job.</span></span> <span data-ttu-id="525e4-120">Obsahuje podrobnosti o úspěchu/neúspěchu a jakékoli odezvě.</span><span class="sxs-lookup"><span data-stu-id="525e4-120">It contains success vs. failure, as well as any response details.</span></span> |

## <a name="scheduler-entity-management"></a><span data-ttu-id="525e4-121">Správa entit Scheduleru</span><span class="sxs-lookup"><span data-stu-id="525e4-121">Scheduler entity management</span></span>
<span data-ttu-id="525e4-122">Na vysoké úrovni hello Plánovač a rozhraní API pro správu služby hello vystavit hello následující operací s prostředky hello:</span><span class="sxs-lookup"><span data-stu-id="525e4-122">At a high level, hello scheduler and hello service management API expose hello following operations on hello resources:</span></span>

| <span data-ttu-id="525e4-123">Schopnost</span><span class="sxs-lookup"><span data-stu-id="525e4-123">Capability</span></span> | <span data-ttu-id="525e4-124">Popis a adresa identifikátoru URI</span><span class="sxs-lookup"><span data-stu-id="525e4-124">Description and URI address</span></span> |
| --- | --- |
| <span data-ttu-id="525e4-125">**Správa kolekce úloh**</span><span class="sxs-lookup"><span data-stu-id="525e4-125">**Job collection management**</span></span> |<span data-ttu-id="525e4-126">GET, PUT a DELETE podpora pro vytváření a úpravu kolekcí úloh a v nich obsažených úloh hello.</span><span class="sxs-lookup"><span data-stu-id="525e4-126">GET, PUT, and DELETE support for creating and modifying job collections and hello jobs contained therein.</span></span> <span data-ttu-id="525e4-127">Kolekce úloh je kontejner pro úlohy a mapy tooquotas a sdíleným nastavením.</span><span class="sxs-lookup"><span data-stu-id="525e4-127">A job collection is a container for jobs and maps tooquotas and shared settings.</span></span> <span data-ttu-id="525e4-128">Příklady kvót popsané později jsou maximální počet úloh a nejmenší interval opakování.</span><span class="sxs-lookup"><span data-stu-id="525e4-128">Examples of quotas, described later, are maximum number of jobs and smallest recurrence interval.</span></span> <p><span data-ttu-id="525e4-129">PUT a DELETE: `https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Scheduler/jobCollections/{jobCollectionName}`</span><span class="sxs-lookup"><span data-stu-id="525e4-129">PUT and DELETE: `https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Scheduler/jobCollections/{jobCollectionName}`</span></span></p><p><span data-ttu-id="525e4-130">GET: `https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Scheduler/jobCollections/{jobCollectionName}`</span><span class="sxs-lookup"><span data-stu-id="525e4-130">GET: `https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Scheduler/jobCollections/{jobCollectionName}`</span></span></p> |
| <span data-ttu-id="525e4-131">**Správa úloh**</span><span class="sxs-lookup"><span data-stu-id="525e4-131">**Job management**</span></span> |<span data-ttu-id="525e4-132">Podpora pro GET, PUT, POST, PATCH a DELETE pro vytváření a úpravu úloh.</span><span class="sxs-lookup"><span data-stu-id="525e4-132">GET, PUT, POST, PATCH, and DELETE support for creating and modifying jobs.</span></span> <span data-ttu-id="525e4-133">Všechny úlohy musí patřit tooa kolekce úloh, který již existuje, takže není vytváření nedochází.</span><span class="sxs-lookup"><span data-stu-id="525e4-133">All jobs must belong tooa job collection that already exists, so there is no implicit creation.</span></span> <p>`https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Scheduler/jobCollections/{jobCollectionName}/jobs/{jobName}`</p> |
| <span data-ttu-id="525e4-134">**Správa historie úloh**</span><span class="sxs-lookup"><span data-stu-id="525e4-134">**Job history management**</span></span> |<span data-ttu-id="525e4-135">Podpora pro GET pro načtení historie provádění úloh, jako je uplynulá doba úlohy a výsledky provedení úlohy, za posledních 60 dní.</span><span class="sxs-lookup"><span data-stu-id="525e4-135">GET support for fetching 60 days of job execution history, such as job elapsed time and job execution results.</span></span> <span data-ttu-id="525e4-136">Přidá podporu parametru řetězce dotazu pro filtrování podle stavu a statusu.</span><span class="sxs-lookup"><span data-stu-id="525e4-136">Adds query string parameter support for filtering based on state and status.</span></span> <P>`https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Scheduler/jobCollections/{jobCollectionName}/jobs/{jobName}/history`</p> |

## <a name="job-types"></a><span data-ttu-id="525e4-137">Typy úloh</span><span class="sxs-lookup"><span data-stu-id="525e4-137">Job types</span></span>
<span data-ttu-id="525e4-138">Existují různé typy úloh: úlohy HTTP (a úlohy HTTPS, které podporují SSL), úlohy fronty úložiště, úlohy fronty sběrnice a úlohy témata sběrnice.</span><span class="sxs-lookup"><span data-stu-id="525e4-138">There are multiple types of jobs: HTTP jobs (including HTTPS jobs that support SSL), storage queue jobs, service bus queue jobs, and service bus topic jobs.</span></span> <span data-ttu-id="525e4-139">Úlohy HTTP jsou ideální, pokud máte koncový bod existující úlohy nebo služby.</span><span class="sxs-lookup"><span data-stu-id="525e4-139">HTTP jobs are ideal if you have an endpoint of an existing workload or service.</span></span> <span data-ttu-id="525e4-140">Fronty úloh toopost zprávy toostorage front úložiště, můžete použít, takže jsou ideální pro úlohy, které používají fronty úložiště.</span><span class="sxs-lookup"><span data-stu-id="525e4-140">You can use storage queue jobs toopost messages toostorage queues, so those jobs are ideal for workloads that use storage queues.</span></span> <span data-ttu-id="525e4-141">Úlohy sběrnice služby jsou zase ideální pro úlohy, které používají témata a fronty sběrnice služby.</span><span class="sxs-lookup"><span data-stu-id="525e4-141">Similarly, service bus jobs are ideal for workloads that use service bus queues and topics.</span></span>

## <a name="hello-job-entity-in-detail"></a><span data-ttu-id="525e4-142">Podrobnosti o entitě "úloha" Hello</span><span class="sxs-lookup"><span data-stu-id="525e4-142">hello "job" entity in detail</span></span>
<span data-ttu-id="525e4-143">Na základní úrovni má naplánovaná úloha několik částí:</span><span class="sxs-lookup"><span data-stu-id="525e4-143">At a basic level, a scheduled job has several parts:</span></span>

* <span data-ttu-id="525e4-144">Hello akce tooperform při hello časovač úlohy sepne</span><span class="sxs-lookup"><span data-stu-id="525e4-144">hello action tooperform when hello job timer fires</span></span>  
* <span data-ttu-id="525e4-145">(Volitelné) hello čase toorun hello úloha</span><span class="sxs-lookup"><span data-stu-id="525e4-145">(Optional) hello time toorun hello job</span></span>  
* <span data-ttu-id="525e4-146">(Volitelné) Kdy a jak často toorepeat hello úlohy</span><span class="sxs-lookup"><span data-stu-id="525e4-146">(Optional) When and how often toorepeat hello job</span></span>  
* <span data-ttu-id="525e4-147">(Volitelné) Toofire akce, pokud selže primární akce hello</span><span class="sxs-lookup"><span data-stu-id="525e4-147">(Optional) An action toofire if hello primary action fails</span></span>  

<span data-ttu-id="525e4-148">Naplánovaná úloha interně taky obsahuje data poskytnutá systémem, jako je například hello čas příštího naplánovaného provedení.</span><span class="sxs-lookup"><span data-stu-id="525e4-148">Internally, a scheduled job also contains system-provided data such as hello next scheduled execution time.</span></span>

<span data-ttu-id="525e4-149">Následující kód Hello poskytuje ucelený příklad naplánované úlohy.</span><span class="sxs-lookup"><span data-stu-id="525e4-149">hello following code provides a comprehensive example of a scheduled job.</span></span> <span data-ttu-id="525e4-150">Podrobnější informace jsou uvedené v dalších částech.</span><span class="sxs-lookup"><span data-stu-id="525e4-150">Details are provided in subsequent sections.</span></span>

    {
        "startTime": "2012-08-04T00:00Z",               // optional
        "action":
        {
            "type": "http",
            "retryPolicy": { "retryType":"none" },
            "request":
            {
                "uri": "http://contoso.com/foo",        // required
                "method": "PUT",                        // required
                "body": "Posting from a timer",         // optional
                "headers":                              // optional

                {
                    "Content-Type": "application/json"
                },
            },
           "errorAction":
           {
               "type": "http",
               "request":
               {
                   "uri": "http://contoso.com/notifyError",
                   "method": "POST",
               },
           },
        },
        "recurrence":                                   // optional
        {
            "frequency": "week",                        // can be "year" "month" "day" "week" "minute"
            "interval": 1,                              // optional, how often toofire (default too1)
            "schedule":                                 // optional (advanced scheduling specifics)
            {
                "weekDays": ["monday", "wednesday", "friday"],
                "hours": [10, 22]
            },
            "count": 10,                                 // optional (default toorecur infinitely)
            "endTime": "2012-11-04",                     // optional (default toorecur infinitely)
        },
        "state": "disabled",                           // enabled or disabled
        "status":                                       // controlled by Scheduler service
        {
            "lastExecutionTime": "2007-03-01T13:00:00Z",
            "nextExecutionTime": "2007-03-01T14:00:00Z ",
            "executionCount": 3,
                                                "failureCount": 0,
                                                "faultedCount": 0
        },
    }

<span data-ttu-id="525e4-151">Jak je vidět v hello předešlé ukázce naplánované úlohy, má definice úlohy několik částí:</span><span class="sxs-lookup"><span data-stu-id="525e4-151">As seen in hello sample scheduled job above, a job definition has several parts:</span></span>

* <span data-ttu-id="525e4-152">Čas spuštění („startTime“)</span><span class="sxs-lookup"><span data-stu-id="525e4-152">Start time (“startTime”)</span></span>  
* <span data-ttu-id="525e4-153">Akce („action“), která zahrnuje i akci při chybě („errorAction“)</span><span class="sxs-lookup"><span data-stu-id="525e4-153">Action (“action”), which includes error action (“errorAction”)</span></span>
* <span data-ttu-id="525e4-154">Opakování („recurrence“)</span><span class="sxs-lookup"><span data-stu-id="525e4-154">Recurrence (“recurrence”)</span></span>  
* <span data-ttu-id="525e4-155">Stav („state“)</span><span class="sxs-lookup"><span data-stu-id="525e4-155">State (“state”)</span></span>  
* <span data-ttu-id="525e4-156">Status („status“)</span><span class="sxs-lookup"><span data-stu-id="525e4-156">Status (“status”)</span></span>  
* <span data-ttu-id="525e4-157">Zásady opakovaných pokusů („retryPolicy“)</span><span class="sxs-lookup"><span data-stu-id="525e4-157">Retry policy (“retryPolicy”)</span></span>  

<span data-ttu-id="525e4-158">Podívejme se na každou podrobněji:</span><span class="sxs-lookup"><span data-stu-id="525e4-158">Let’s examine each of these in detail:</span></span>

## <a name="starttime"></a><span data-ttu-id="525e4-159">startTime</span><span class="sxs-lookup"><span data-stu-id="525e4-159">startTime</span></span>
<span data-ttu-id="525e4-160">Hello "startTime" je čas spuštění hello a umožňuje hello volající toospecify časové pásmo posun na hello přenosu v [formátu ISO 8601](http://en.wikipedia.org/wiki/ISO_8601).</span><span class="sxs-lookup"><span data-stu-id="525e4-160">hello "startTime” is hello start time and allows hello caller toospecify a time zone offset on hello wire in [ISO-8601 format](http://en.wikipedia.org/wiki/ISO_8601).</span></span>

## <a name="action-and-erroraction"></a><span data-ttu-id="525e4-161">action a errorAction</span><span class="sxs-lookup"><span data-stu-id="525e4-161">action and errorAction</span></span>
<span data-ttu-id="525e4-162">Hello "action" je hello akce vyvolaná při každém výskytu a popisuje typ vyvolání služby.</span><span class="sxs-lookup"><span data-stu-id="525e4-162">hello “action” is hello action invoked on each occurrence and describes a type of service invocation.</span></span> <span data-ttu-id="525e4-163">Akce Hello je, co se provede na hello zadaný plán.</span><span class="sxs-lookup"><span data-stu-id="525e4-163">hello action is what will be executed on hello provided schedule.</span></span> <span data-ttu-id="525e4-164">Scheduler podporuje akce HTTP, fronty úložiště, fronty sběrnice a témata sběrnice.</span><span class="sxs-lookup"><span data-stu-id="525e4-164">Scheduler supports HTTP, storage queue, service bus topic, and service bus queue actions.</span></span>

<span data-ttu-id="525e4-165">Akce Hello v předchozím příkladu hello je akce HTTP.</span><span class="sxs-lookup"><span data-stu-id="525e4-165">hello action in hello example above is an HTTP action.</span></span> <span data-ttu-id="525e4-166">Dole je příklad akce fronty úložiště:</span><span class="sxs-lookup"><span data-stu-id="525e4-166">Below is an example of a storage queue action:</span></span>

    {
            "type": "storageQueue",
            "queueMessage":
            {
                "storageAccount": "myStorageAccount",  // required
                "queueName": "myqueue",                // required
                "sasToken": "TOKEN",                   // required
                "message":                             // required
                    "My message body",
            },
    }

<span data-ttu-id="525e4-167">Dole je příklad akce témata sběrnice.</span><span class="sxs-lookup"><span data-stu-id="525e4-167">Below is an example of a service bus topic action.</span></span>

  <span data-ttu-id="525e4-168">"action": { "type": "serviceBusTopic", "serviceBusTopicMessage": { "topicPath": "t1",</span><span class="sxs-lookup"><span data-stu-id="525e4-168">"action": { "type": "serviceBusTopic", "serviceBusTopicMessage": { "topicPath": "t1",</span></span>  
      <span data-ttu-id="525e4-169">"namespace": "mySBNamespace", "transportType": "netMessaging", // Can be either netMessaging or AMQP "authentication": { "sasKeyName": "QPolicy", "type": "sharedAccessKey" }, "message": "Some message", "brokeredMessageProperties": {}, "customMessageProperties": { "appname": "FromScheduler" } }, }</span><span class="sxs-lookup"><span data-stu-id="525e4-169">"namespace": "mySBNamespace", "transportType": "netMessaging", // Can be either netMessaging or AMQP "authentication": { "sasKeyName": "QPolicy", "type": "sharedAccessKey" }, "message": "Some message", "brokeredMessageProperties": {}, "customMessageProperties": { "appname": "FromScheduler" } }, }</span></span>

<span data-ttu-id="525e4-170">Dole je příklad akce fronty sběrnice:</span><span class="sxs-lookup"><span data-stu-id="525e4-170">Below is an example of a service bus queue action:</span></span>

  <span data-ttu-id="525e4-171">"action": { "serviceBusQueueMessage": { "queueName": "q1",</span><span class="sxs-lookup"><span data-stu-id="525e4-171">"action": { "serviceBusQueueMessage": { "queueName": "q1",</span></span>  
      <span data-ttu-id="525e4-172">"namespace": "mySBNamespace", "transportType": "netMessaging", // Can be either netMessaging or AMQP "authentication": {</span><span class="sxs-lookup"><span data-stu-id="525e4-172">"namespace": "mySBNamespace", "transportType": "netMessaging", // Can be either netMessaging or AMQP "authentication": {</span></span>  
        <span data-ttu-id="525e4-173">"sasKeyName": "QPolicy", "type": "sharedAccessKey" }, "message": "Some message",</span><span class="sxs-lookup"><span data-stu-id="525e4-173">"sasKeyName": "QPolicy", "type": "sharedAccessKey" }, "message": "Some message",</span></span>  
      <span data-ttu-id="525e4-174">"brokeredMessageProperties": {}, "customMessageProperties": { "appname": "FromScheduler" } }, "type": "serviceBusQueue" }</span><span class="sxs-lookup"><span data-stu-id="525e4-174">"brokeredMessageProperties": {}, "customMessageProperties": { "appname": "FromScheduler" } }, "type": "serviceBusQueue" }</span></span>

<span data-ttu-id="525e4-175">Hello "errorAction" je obslužná rutina chyby hello, hello akce vyvolaná při selhání primární akce hello.</span><span class="sxs-lookup"><span data-stu-id="525e4-175">hello “errorAction” is hello error handler, hello action invoked when hello primary action fails.</span></span> <span data-ttu-id="525e4-176">Můžete použít tuto proměnnou toocall koncový bod zpracování chyb nebo odeslání oznámení pro uživatele.</span><span class="sxs-lookup"><span data-stu-id="525e4-176">You can use this variable toocall an error-handling endpoint or send a user notification.</span></span> <span data-ttu-id="525e4-177">Dá se použít pro spojení se sekundárním koncovým bodem v případě hello této hello primární server není k dispozici (například v případě havárie v lokalitě koncového bodu hello hello) nebo můžete použít pro upozornění koncového bodu pro zpracování chyby.</span><span class="sxs-lookup"><span data-stu-id="525e4-177">This can be used for reaching a secondary endpoint in hello case that hello primary is not available (e.g., in hello case of a disaster at hello endpoint’s site) or can be used for notifying an error handling endpoint.</span></span> <span data-ttu-id="525e4-178">Stejně jako primární akce hello může být akce chyby hello jednoduchou nebo složenou logiku podle jiných akcí.</span><span class="sxs-lookup"><span data-stu-id="525e4-178">Just like hello primary action, hello error action can be simple or composite logic based on other actions.</span></span> <span data-ttu-id="525e4-179">jak toocreate SAS token naleznete příliš toolearn[vytváření a používání sdíleného přístupového podpisu](https://msdn.microsoft.com/library/azure/jj721951.aspx).</span><span class="sxs-lookup"><span data-stu-id="525e4-179">toolearn how toocreate a SAS token, refer too[Create and Use a Shared Access Signature](https://msdn.microsoft.com/library/azure/jj721951.aspx).</span></span>

## <a name="recurrence"></a><span data-ttu-id="525e4-180">recurrence</span><span class="sxs-lookup"><span data-stu-id="525e4-180">recurrence</span></span>
<span data-ttu-id="525e4-181">Opakování má několik částí:</span><span class="sxs-lookup"><span data-stu-id="525e4-181">Recurrence has several parts:</span></span>

* <span data-ttu-id="525e4-182">Frequency (frekvence): Minuta, hodina, den, týden, měsíc nebo rok</span><span class="sxs-lookup"><span data-stu-id="525e4-182">Frequency: One of minute, hour, day, week, month, year</span></span>  
* <span data-ttu-id="525e4-183">Interval: Interval při hello zadané frekvence opakování hello</span><span class="sxs-lookup"><span data-stu-id="525e4-183">Interval: Interval at hello given frequency for hello recurrence</span></span>  
* <span data-ttu-id="525e4-184">Předepsaný plán: Zadejte minuty, hodiny, dny v týdnu, měsíce a prescribed hello opakování</span><span class="sxs-lookup"><span data-stu-id="525e4-184">Prescribed schedule: Specify minutes, hours, weekdays, months, and monthdays of hello recurrence</span></span>  
* <span data-ttu-id="525e4-185">Count (počet): Počet výskytů</span><span class="sxs-lookup"><span data-stu-id="525e4-185">Count: Count of occurrences</span></span>  
* <span data-ttu-id="525e4-186">Koncový čas: po hello zadaný čas ukončení se neprovedou žádné úlohy</span><span class="sxs-lookup"><span data-stu-id="525e4-186">End time: No jobs will execute after hello specified end time</span></span>  

<span data-ttu-id="525e4-187">Úloha se opakuje, jestliže má ve své definici JSON opakující se objekt.</span><span class="sxs-lookup"><span data-stu-id="525e4-187">A job is recurring if it has a recurring object specified in its JSON definition.</span></span> <span data-ttu-id="525e4-188">Pokud jsou zadané count i endTime, je dodržení hello dokončení pravidlo, které nastane jako první.</span><span class="sxs-lookup"><span data-stu-id="525e4-188">If both count and endTime are specified, hello completion rule that occurs first is honored.</span></span>

## <a name="state"></a><span data-ttu-id="525e4-189">state</span><span class="sxs-lookup"><span data-stu-id="525e4-189">state</span></span>
<span data-ttu-id="525e4-190">Hello stav úlohy hello je jednu ze čtyř hodnot: povolena, zakázána, dokončení nebo došlo k chybě.</span><span class="sxs-lookup"><span data-stu-id="525e4-190">hello state of hello job is one of four values: enabled, disabled, completed, or faulted.</span></span> <span data-ttu-id="525e4-191">Můžete vložit nebo oprava úlohy, jako tooupdate je toohello povolený nebo zakázaný stav.</span><span class="sxs-lookup"><span data-stu-id="525e4-191">You can PUT or PATCH jobs so as tooupdate them toohello enabled or disabled state.</span></span> <span data-ttu-id="525e4-192">Pokud úloha byla dokončena nebo došlo k chybě, která je konečný stav, který nelze aktualizovat (i když stále můžete použít DELETE hello úlohy).</span><span class="sxs-lookup"><span data-stu-id="525e4-192">If a job has been completed or faulted, that is a final state that cannot be updated (though hello job can still be DELETED).</span></span> <span data-ttu-id="525e4-193">Příklad vlastnosti stavu hello vypadá takto:</span><span class="sxs-lookup"><span data-stu-id="525e4-193">An example of hello state property is as follows:</span></span>

        "state": "disabled", // enabled, disabled, completed, or faulted
<span data-ttu-id="525e4-194">Dokončené úlohy a úlohy, které selhaly, se odstraní po 60 dnech.</span><span class="sxs-lookup"><span data-stu-id="525e4-194">Completed and faulted jobs are deleted after 60 days.</span></span>

## <a name="status"></a><span data-ttu-id="525e4-195">status</span><span class="sxs-lookup"><span data-stu-id="525e4-195">status</span></span>
<span data-ttu-id="525e4-196">Po zahájení úlohy Scheduleru se vrátí informace o aktuální stav úlohy hello hello.</span><span class="sxs-lookup"><span data-stu-id="525e4-196">Once a Scheduler job has started, information will be returned about hello current status of hello job.</span></span> <span data-ttu-id="525e4-197">Tento objekt není nastavit uživatelem hello – je nastavena podle hello systému.</span><span class="sxs-lookup"><span data-stu-id="525e4-197">This object is not settable by hello user—it’s set by hello system.</span></span> <span data-ttu-id="525e4-198">Ale je součástí hello objektu úlohy (spíše než v samostatném propojeném prostředku), aby jeden můžete snadno získat hello stav úlohy.</span><span class="sxs-lookup"><span data-stu-id="525e4-198">However, it is included in hello job object (rather than a separate linked resource) so that one can obtain hello status of a job easily.</span></span>

<span data-ttu-id="525e4-199">Stav úlohy obsahuje čas hello hello předchozího spuštění (pokud existuje), hello čas dalšího naplánovaného spuštění hello (pro probíhající úlohy) a počet provedení hello hello úlohy.</span><span class="sxs-lookup"><span data-stu-id="525e4-199">Job status includes hello time of hello previous execution (if any), hello time of hello next scheduled execution (for in-progress jobs), and hello execution count of hello job.</span></span>

## <a name="retrypolicy"></a><span data-ttu-id="525e4-200">retryPolicy</span><span class="sxs-lookup"><span data-stu-id="525e4-200">retryPolicy</span></span>
<span data-ttu-id="525e4-201">Pokud úloha Scheduleru selže, je možné toospecify toodetermine zásady opakování, zda a jak se pokus o hello akce.</span><span class="sxs-lookup"><span data-stu-id="525e4-201">If a Scheduler job fails, it is possible toospecify a retry policy toodetermine whether and how hello action is retried.</span></span> <span data-ttu-id="525e4-202">To je dáno hello **retryType** objektu – je nastaven příliš**žádné** Pokud neexistuje žádné zásady opakovaných pokusů, jak je uvedeno výše.</span><span class="sxs-lookup"><span data-stu-id="525e4-202">This is determined by hello **retryType** object—it is set too**none** if there is no retry policy, as shown above.</span></span> <span data-ttu-id="525e4-203">Nastavit také**pevné** v případě zásady opakování.</span><span class="sxs-lookup"><span data-stu-id="525e4-203">Set it too**fixed** if there is a retry policy.</span></span>

<span data-ttu-id="525e4-204">tooset zásady opakovaných pokusů, můžete zadat další dvě nastavení: interval opakování (**retryInterval**) a hello počet opakovaných pokusů (**retryCount**).</span><span class="sxs-lookup"><span data-stu-id="525e4-204">tooset a retry policy, two additional settings may be specified: a retry interval (**retryInterval**) and hello number of retries (**retryCount**).</span></span>

<span data-ttu-id="525e4-205">interval opakovaných Hello zadaným hello **retryInterval** objektu, je hello interval mezi opakovanými pokusy.</span><span class="sxs-lookup"><span data-stu-id="525e4-205">hello retry interval, specified with hello **retryInterval** object, is hello interval between retries.</span></span> <span data-ttu-id="525e4-206">Výchozí hodnota je 30 sekund, minimální nastavitelná hodnota je 15 sekund a maximální hodnota je 18 měsíců.</span><span class="sxs-lookup"><span data-stu-id="525e4-206">Its default value is 30 seconds, its minimum configurable value is 15 seconds, and its maximum value is 18 months.</span></span> <span data-ttu-id="525e4-207">Pro úlohy v kolekcích úloh Free je minimální nastavitelná hodnota 1 hodina.</span><span class="sxs-lookup"><span data-stu-id="525e4-207">Jobs in Free job collections have a minimum configurable value of 1 hour.</span></span>  <span data-ttu-id="525e4-208">Je definována ve formátu ISO 8601 hello.</span><span class="sxs-lookup"><span data-stu-id="525e4-208">It is defined in hello ISO 8601 format.</span></span> <span data-ttu-id="525e4-209">Podobně je zadána hodnota hello hello počet opakování s hello **retryCount** objektu; je hello počet, kolikrát má pokus opakovat.</span><span class="sxs-lookup"><span data-stu-id="525e4-209">Similarly, hello value of hello number of retries is specified with hello **retryCount** object; it is hello number of times a retry is attempted.</span></span> <span data-ttu-id="525e4-210">Výchozí hodnota je 4 a maximální hodnota je 20\.</span><span class="sxs-lookup"><span data-stu-id="525e4-210">Its default value is 4, and its maximum value is 20\.</span></span> <span data-ttu-id="525e4-211">Obě **retryInterval** a **retryCount** jsou volitelné.</span><span class="sxs-lookup"><span data-stu-id="525e4-211">Both **retryInterval** and **retryCount** are optional.</span></span> <span data-ttu-id="525e4-212">Pokud jsou uvedené výchozí hodnoty **retryType** je nastaven příliš**pevné** a nejsou explicitně zadané žádné hodnoty.</span><span class="sxs-lookup"><span data-stu-id="525e4-212">They are given their default values if **retryType** is set too**fixed** and no values are specified explicitly.</span></span>

## <a name="see-also"></a><span data-ttu-id="525e4-213">Viz také</span><span class="sxs-lookup"><span data-stu-id="525e4-213">See also</span></span>
 [<span data-ttu-id="525e4-214">Co je Scheduler?</span><span class="sxs-lookup"><span data-stu-id="525e4-214">What is Scheduler?</span></span>](scheduler-intro.md)

 [<span data-ttu-id="525e4-215">Začněte používat plánovače v hello portálu Azure</span><span class="sxs-lookup"><span data-stu-id="525e4-215">Get started using Scheduler in hello Azure portal</span></span>](scheduler-get-started-portal.md)

 [<span data-ttu-id="525e4-216">Plány a fakturace v Azure Scheduleru</span><span class="sxs-lookup"><span data-stu-id="525e4-216">Plans and billing in Azure Scheduler</span></span>](scheduler-plans-billing.md)

 [<span data-ttu-id="525e4-217">Jak toobuild komplexní plány a pokročilé opakování ve službě Azure Scheduler</span><span class="sxs-lookup"><span data-stu-id="525e4-217">How toobuild complex schedules and advanced recurrence with Azure Scheduler</span></span>](scheduler-advanced-complexity.md)

 [<span data-ttu-id="525e4-218">REST API Azure Scheduleru – referenční informace</span><span class="sxs-lookup"><span data-stu-id="525e4-218">Azure Scheduler REST API reference</span></span>](https://msdn.microsoft.com/library/mt629143)

 [<span data-ttu-id="525e4-219">Rutiny PowerShellu pro Azure Scheduler – referenční informace</span><span class="sxs-lookup"><span data-stu-id="525e4-219">Azure Scheduler PowerShell cmdlets reference</span></span>](scheduler-powershell-reference.md)

 [<span data-ttu-id="525e4-220">Vysoká dostupnost a spolehlivost Azure Scheduleru</span><span class="sxs-lookup"><span data-stu-id="525e4-220">Azure Scheduler high-availability and reliability</span></span>](scheduler-high-availability-reliability.md)

 [<span data-ttu-id="525e4-221">Omezení, výchozí hodnoty a chybové kódy Azure Scheduleru</span><span class="sxs-lookup"><span data-stu-id="525e4-221">Azure Scheduler limits, defaults, and error codes</span></span>](scheduler-limits-defaults-errors.md)

 [<span data-ttu-id="525e4-222">Odchozí ověření Azure Scheduleru</span><span class="sxs-lookup"><span data-stu-id="525e4-222">Azure Scheduler outbound authentication</span></span>](scheduler-outbound-authentication.md)

