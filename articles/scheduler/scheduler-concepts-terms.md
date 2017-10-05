---
title: Koncepty, pojmy a entity Scheduleru | Dokumentace Microsoftu
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
ms.openlocfilehash: 0f035b58ccd140a5481703df7e184206da2ed651
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="scheduler-concepts-terminology--entity-hierarchy"></a><span data-ttu-id="d61b2-104">Koncepty, terminologie a hierarchie entit Scheduleru</span><span class="sxs-lookup"><span data-stu-id="d61b2-104">Scheduler concepts, terminology, + entity hierarchy</span></span>
## <a name="scheduler-entity-hierarchy"></a><span data-ttu-id="d61b2-105">Hierarchie entit Scheduleru</span><span class="sxs-lookup"><span data-stu-id="d61b2-105">Scheduler entity hierarchy</span></span>
<span data-ttu-id="d61b2-106">V následující tabulce jsou uvedené hlavní prostředky, které rozhraní API Scheduleru vystavuje nebo používá.</span><span class="sxs-lookup"><span data-stu-id="d61b2-106">The following table describes the main resources exposed or used by the Scheduler API:</span></span>

| <span data-ttu-id="d61b2-107">Prostředek</span><span class="sxs-lookup"><span data-stu-id="d61b2-107">Resource</span></span> | <span data-ttu-id="d61b2-108">Popis</span><span class="sxs-lookup"><span data-stu-id="d61b2-108">Description</span></span> |
| --- | --- |
| <span data-ttu-id="d61b2-109">**Kolekce úloh**</span><span class="sxs-lookup"><span data-stu-id="d61b2-109">**Job collection**</span></span> |<span data-ttu-id="d61b2-110">Kolekce úloh obsahuje skupinu úloh a zachovává nastavení, kvóty a omezení, které jsou sdílené mezi úlohami v kolekci.</span><span class="sxs-lookup"><span data-stu-id="d61b2-110">A job collection contains a group of jobs and maintains settings, quotas, and throttles that are shared by jobs within the collection.</span></span> <span data-ttu-id="d61b2-111">Kolekci úloh vytvoří vlastník předplatného a seskupuje úlohy podle použití nebo hranic aplikací.</span><span class="sxs-lookup"><span data-stu-id="d61b2-111">A job collection is created by a subscription owner and groups jobs together based on usage or application boundaries.</span></span> <span data-ttu-id="d61b2-112">Je omezená na jednu oblast.</span><span class="sxs-lookup"><span data-stu-id="d61b2-112">It’s constrained to one region.</span></span> <span data-ttu-id="d61b2-113">Taky umožňuje vynucovat kvóty a dál tak omezovat používání všech úloh v dané kolekci.</span><span class="sxs-lookup"><span data-stu-id="d61b2-113">It also allows the enforcement of quotas to constrain the usage of all jobs in that collection.</span></span> <span data-ttu-id="d61b2-114">Tyto kvóty zahrnují hodnoty MaxJobs a MaxRecurrence.</span><span class="sxs-lookup"><span data-stu-id="d61b2-114">The quotas include MaxJobs and MaxRecurrence.</span></span> |
| <span data-ttu-id="d61b2-115">**Úloha**</span><span class="sxs-lookup"><span data-stu-id="d61b2-115">**Job**</span></span> |<span data-ttu-id="d61b2-116">Úloha definuje jednu opakující se akci s jednoduchými nebo komplexními strategiemi provedení.</span><span class="sxs-lookup"><span data-stu-id="d61b2-116">A job defines a single recurrent action, with simple or complex strategies for execution.</span></span> <span data-ttu-id="d61b2-117">Akce můžou zahrnovat požadavky HTTP, fronty úložiště, fronty sběrnice nebo témata sběrnice.</span><span class="sxs-lookup"><span data-stu-id="d61b2-117">Actions may include HTTP, storage queue, service bus queue, or service bus topic requests.</span></span> |
| <span data-ttu-id="d61b2-118">**Historie úlohy**</span><span class="sxs-lookup"><span data-stu-id="d61b2-118">**Job history**</span></span> |<span data-ttu-id="d61b2-119">Historie úlohy obsahuje podrobnosti o provedení úlohy.</span><span class="sxs-lookup"><span data-stu-id="d61b2-119">A job history represents details for an execution of a job.</span></span> <span data-ttu-id="d61b2-120">Obsahuje podrobnosti o úspěchu/neúspěchu a jakékoli odezvě.</span><span class="sxs-lookup"><span data-stu-id="d61b2-120">It contains success vs. failure, as well as any response details.</span></span> |

## <a name="scheduler-entity-management"></a><span data-ttu-id="d61b2-121">Správa entit Scheduleru</span><span class="sxs-lookup"><span data-stu-id="d61b2-121">Scheduler entity management</span></span>
<span data-ttu-id="d61b2-122">Na vysoké úrovni plánovač a rozhraní API pro správu služeb vystavují na prostředky tyto operace:</span><span class="sxs-lookup"><span data-stu-id="d61b2-122">At a high level, the scheduler and the service management API expose the following operations on the resources:</span></span>

| <span data-ttu-id="d61b2-123">Schopnost</span><span class="sxs-lookup"><span data-stu-id="d61b2-123">Capability</span></span> | <span data-ttu-id="d61b2-124">Popis a adresa identifikátoru URI</span><span class="sxs-lookup"><span data-stu-id="d61b2-124">Description and URI address</span></span> |
| --- | --- |
| <span data-ttu-id="d61b2-125">**Správa kolekce úloh**</span><span class="sxs-lookup"><span data-stu-id="d61b2-125">**Job collection management**</span></span> |<span data-ttu-id="d61b2-126">Podpora pro GET, PUT a DELETE pro vytváření a úpravu kolekcí úloh a v nich obsažených úloh.</span><span class="sxs-lookup"><span data-stu-id="d61b2-126">GET, PUT, and DELETE support for creating and modifying job collections and the jobs contained therein.</span></span> <span data-ttu-id="d61b2-127">Kolekce úloh je kontejner pro úlohy a mapy ke kvótám a sdíleným nastavením.</span><span class="sxs-lookup"><span data-stu-id="d61b2-127">A job collection is a container for jobs and maps to quotas and shared settings.</span></span> <span data-ttu-id="d61b2-128">Příklady kvót popsané později jsou maximální počet úloh a nejmenší interval opakování.</span><span class="sxs-lookup"><span data-stu-id="d61b2-128">Examples of quotas, described later, are maximum number of jobs and smallest recurrence interval.</span></span> <p><span data-ttu-id="d61b2-129">PUT a DELETE: `https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Scheduler/jobCollections/{jobCollectionName}`</span><span class="sxs-lookup"><span data-stu-id="d61b2-129">PUT and DELETE: `https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Scheduler/jobCollections/{jobCollectionName}`</span></span></p><p><span data-ttu-id="d61b2-130">GET: `https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Scheduler/jobCollections/{jobCollectionName}`</span><span class="sxs-lookup"><span data-stu-id="d61b2-130">GET: `https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Scheduler/jobCollections/{jobCollectionName}`</span></span></p> |
| <span data-ttu-id="d61b2-131">**Správa úloh**</span><span class="sxs-lookup"><span data-stu-id="d61b2-131">**Job management**</span></span> |<span data-ttu-id="d61b2-132">Podpora pro GET, PUT, POST, PATCH a DELETE pro vytváření a úpravu úloh.</span><span class="sxs-lookup"><span data-stu-id="d61b2-132">GET, PUT, POST, PATCH, and DELETE support for creating and modifying jobs.</span></span> <span data-ttu-id="d61b2-133">Všechny úlohy musí patřit do stejné kolekce, která už existuje, takže k vytváření nedochází.</span><span class="sxs-lookup"><span data-stu-id="d61b2-133">All jobs must belong to a job collection that already exists, so there is no implicit creation.</span></span> <p>`https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Scheduler/jobCollections/{jobCollectionName}/jobs/{jobName}`</p> |
| <span data-ttu-id="d61b2-134">**Správa historie úloh**</span><span class="sxs-lookup"><span data-stu-id="d61b2-134">**Job history management**</span></span> |<span data-ttu-id="d61b2-135">Podpora pro GET pro načtení historie provádění úloh, jako je uplynulá doba úlohy a výsledky provedení úlohy, za posledních 60 dní.</span><span class="sxs-lookup"><span data-stu-id="d61b2-135">GET support for fetching 60 days of job execution history, such as job elapsed time and job execution results.</span></span> <span data-ttu-id="d61b2-136">Přidá podporu parametru řetězce dotazu pro filtrování podle stavu a statusu.</span><span class="sxs-lookup"><span data-stu-id="d61b2-136">Adds query string parameter support for filtering based on state and status.</span></span> <P>`https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Scheduler/jobCollections/{jobCollectionName}/jobs/{jobName}/history`</p> |

## <a name="job-types"></a><span data-ttu-id="d61b2-137">Typy úloh</span><span class="sxs-lookup"><span data-stu-id="d61b2-137">Job types</span></span>
<span data-ttu-id="d61b2-138">Existují různé typy úloh: úlohy HTTP (a úlohy HTTPS, které podporují SSL), úlohy fronty úložiště, úlohy fronty sběrnice a úlohy témata sběrnice.</span><span class="sxs-lookup"><span data-stu-id="d61b2-138">There are multiple types of jobs: HTTP jobs (including HTTPS jobs that support SSL), storage queue jobs, service bus queue jobs, and service bus topic jobs.</span></span> <span data-ttu-id="d61b2-139">Úlohy HTTP jsou ideální, pokud máte koncový bod existující úlohy nebo služby.</span><span class="sxs-lookup"><span data-stu-id="d61b2-139">HTTP jobs are ideal if you have an endpoint of an existing workload or service.</span></span> <span data-ttu-id="d61b2-140">Úlohy fronty úložiště můžete použít k odeslání zprávy do front úložiště, takže jsou ideální pro úlohy, které používají fronty úložiště.</span><span class="sxs-lookup"><span data-stu-id="d61b2-140">You can use storage queue jobs to post messages to storage queues, so those jobs are ideal for workloads that use storage queues.</span></span> <span data-ttu-id="d61b2-141">Úlohy sběrnice služby jsou zase ideální pro úlohy, které používají témata a fronty sběrnice služby.</span><span class="sxs-lookup"><span data-stu-id="d61b2-141">Similarly, service bus jobs are ideal for workloads that use service bus queues and topics.</span></span>

## <a name="the-job-entity-in-detail"></a><span data-ttu-id="d61b2-142">Podrobnosti o entitě „úloha“</span><span class="sxs-lookup"><span data-stu-id="d61b2-142">The "job" entity in detail</span></span>
<span data-ttu-id="d61b2-143">Na základní úrovni má naplánovaná úloha několik částí:</span><span class="sxs-lookup"><span data-stu-id="d61b2-143">At a basic level, a scheduled job has several parts:</span></span>

* <span data-ttu-id="d61b2-144">Akce, která se má provést, když časovač úlohy sepne</span><span class="sxs-lookup"><span data-stu-id="d61b2-144">The action to perform when the job timer fires</span></span>  
* <span data-ttu-id="d61b2-145">(Volitelné) Čas, kdy se má úloha spustit</span><span class="sxs-lookup"><span data-stu-id="d61b2-145">(Optional) The time to run the job</span></span>  
* <span data-ttu-id="d61b2-146">(Volitelné) Kdy a jak často se má úloha opakovat</span><span class="sxs-lookup"><span data-stu-id="d61b2-146">(Optional) When and how often to repeat the job</span></span>  
* <span data-ttu-id="d61b2-147">(Volitelné) Akce, která se má provést, pokud primární akce selže</span><span class="sxs-lookup"><span data-stu-id="d61b2-147">(Optional) An action to fire if the primary action fails</span></span>  

<span data-ttu-id="d61b2-148">Naplánovaná úloha interně taky obsahuje data poskytnutá systémem, jako je třeba čas příštího naplánovaného provedení.</span><span class="sxs-lookup"><span data-stu-id="d61b2-148">Internally, a scheduled job also contains system-provided data such as the next scheduled execution time.</span></span>

<span data-ttu-id="d61b2-149">Následující kód je komplexní příklad naplánované úlohy.</span><span class="sxs-lookup"><span data-stu-id="d61b2-149">The following code provides a comprehensive example of a scheduled job.</span></span> <span data-ttu-id="d61b2-150">Podrobnější informace jsou uvedené v dalších částech.</span><span class="sxs-lookup"><span data-stu-id="d61b2-150">Details are provided in subsequent sections.</span></span>

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
            "interval": 1,                              // optional, how often to fire (default to 1)
            "schedule":                                 // optional (advanced scheduling specifics)
            {
                "weekDays": ["monday", "wednesday", "friday"],
                "hours": [10, 22]
            },
            "count": 10,                                 // optional (default to recur infinitely)
            "endTime": "2012-11-04",                     // optional (default to recur infinitely)
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

<span data-ttu-id="d61b2-151">Jak je vidět na předešlé ukázce naplánované úlohy, má definice úlohy několik částí:</span><span class="sxs-lookup"><span data-stu-id="d61b2-151">As seen in the sample scheduled job above, a job definition has several parts:</span></span>

* <span data-ttu-id="d61b2-152">Čas spuštění („startTime“)</span><span class="sxs-lookup"><span data-stu-id="d61b2-152">Start time (“startTime”)</span></span>  
* <span data-ttu-id="d61b2-153">Akce („action“), která zahrnuje i akci při chybě („errorAction“)</span><span class="sxs-lookup"><span data-stu-id="d61b2-153">Action (“action”), which includes error action (“errorAction”)</span></span>
* <span data-ttu-id="d61b2-154">Opakování („recurrence“)</span><span class="sxs-lookup"><span data-stu-id="d61b2-154">Recurrence (“recurrence”)</span></span>  
* <span data-ttu-id="d61b2-155">Stav („state“)</span><span class="sxs-lookup"><span data-stu-id="d61b2-155">State (“state”)</span></span>  
* <span data-ttu-id="d61b2-156">Status („status“)</span><span class="sxs-lookup"><span data-stu-id="d61b2-156">Status (“status”)</span></span>  
* <span data-ttu-id="d61b2-157">Zásady opakovaných pokusů („retryPolicy“)</span><span class="sxs-lookup"><span data-stu-id="d61b2-157">Retry policy (“retryPolicy”)</span></span>  

<span data-ttu-id="d61b2-158">Podívejme se na každou podrobněji:</span><span class="sxs-lookup"><span data-stu-id="d61b2-158">Let’s examine each of these in detail:</span></span>

## <a name="starttime"></a><span data-ttu-id="d61b2-159">startTime</span><span class="sxs-lookup"><span data-stu-id="d61b2-159">startTime</span></span>
<span data-ttu-id="d61b2-160">„startTime“ je čas spuštění a umožňuje volajícímu zadat posun časového pásma ve [formátu ISO-8601](http://en.wikipedia.org/wiki/ISO_8601).</span><span class="sxs-lookup"><span data-stu-id="d61b2-160">The "startTime” is the start time and allows the caller to specify a time zone offset on the wire in [ISO-8601 format](http://en.wikipedia.org/wiki/ISO_8601).</span></span>

## <a name="action-and-erroraction"></a><span data-ttu-id="d61b2-161">action a errorAction</span><span class="sxs-lookup"><span data-stu-id="d61b2-161">action and errorAction</span></span>
<span data-ttu-id="d61b2-162">„action“ je akce vyvolaná při každém výskytu a popisuje typ vyvolání služby.</span><span class="sxs-lookup"><span data-stu-id="d61b2-162">The “action” is the action invoked on each occurrence and describes a type of service invocation.</span></span> <span data-ttu-id="d61b2-163">Akce je to, co se provede podle stanoveného plánu.</span><span class="sxs-lookup"><span data-stu-id="d61b2-163">The action is what will be executed on the provided schedule.</span></span> <span data-ttu-id="d61b2-164">Scheduler podporuje akce HTTP, fronty úložiště, fronty sběrnice a témata sběrnice.</span><span class="sxs-lookup"><span data-stu-id="d61b2-164">Scheduler supports HTTP, storage queue, service bus topic, and service bus queue actions.</span></span>

<span data-ttu-id="d61b2-165">V příkladu nahoře je akce HTTP.</span><span class="sxs-lookup"><span data-stu-id="d61b2-165">The action in the example above is an HTTP action.</span></span> <span data-ttu-id="d61b2-166">Dole je příklad akce fronty úložiště:</span><span class="sxs-lookup"><span data-stu-id="d61b2-166">Below is an example of a storage queue action:</span></span>

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

<span data-ttu-id="d61b2-167">Dole je příklad akce témata sběrnice.</span><span class="sxs-lookup"><span data-stu-id="d61b2-167">Below is an example of a service bus topic action.</span></span>

  <span data-ttu-id="d61b2-168">"action": { "type": "serviceBusTopic", "serviceBusTopicMessage": { "topicPath": "t1",</span><span class="sxs-lookup"><span data-stu-id="d61b2-168">"action": { "type": "serviceBusTopic", "serviceBusTopicMessage": { "topicPath": "t1",</span></span>  
      <span data-ttu-id="d61b2-169">"namespace": "mySBNamespace", "transportType": "netMessaging", // Can be either netMessaging or AMQP "authentication": { "sasKeyName": "QPolicy", "type": "sharedAccessKey" }, "message": "Some message", "brokeredMessageProperties": {}, "customMessageProperties": { "appname": "FromScheduler" } }, }</span><span class="sxs-lookup"><span data-stu-id="d61b2-169">"namespace": "mySBNamespace", "transportType": "netMessaging", // Can be either netMessaging or AMQP "authentication": { "sasKeyName": "QPolicy", "type": "sharedAccessKey" }, "message": "Some message", "brokeredMessageProperties": {}, "customMessageProperties": { "appname": "FromScheduler" } }, }</span></span>

<span data-ttu-id="d61b2-170">Dole je příklad akce fronty sběrnice:</span><span class="sxs-lookup"><span data-stu-id="d61b2-170">Below is an example of a service bus queue action:</span></span>

  <span data-ttu-id="d61b2-171">"action": { "serviceBusQueueMessage": { "queueName": "q1",</span><span class="sxs-lookup"><span data-stu-id="d61b2-171">"action": { "serviceBusQueueMessage": { "queueName": "q1",</span></span>  
      <span data-ttu-id="d61b2-172">"namespace": "mySBNamespace", "transportType": "netMessaging", // Can be either netMessaging or AMQP "authentication": {</span><span class="sxs-lookup"><span data-stu-id="d61b2-172">"namespace": "mySBNamespace", "transportType": "netMessaging", // Can be either netMessaging or AMQP "authentication": {</span></span>  
        <span data-ttu-id="d61b2-173">"sasKeyName": "QPolicy", "type": "sharedAccessKey" }, "message": "Some message",</span><span class="sxs-lookup"><span data-stu-id="d61b2-173">"sasKeyName": "QPolicy", "type": "sharedAccessKey" }, "message": "Some message",</span></span>  
      <span data-ttu-id="d61b2-174">"brokeredMessageProperties": {}, "customMessageProperties": { "appname": "FromScheduler" } }, "type": "serviceBusQueue" }</span><span class="sxs-lookup"><span data-stu-id="d61b2-174">"brokeredMessageProperties": {}, "customMessageProperties": { "appname": "FromScheduler" } }, "type": "serviceBusQueue" }</span></span>

<span data-ttu-id="d61b2-175">„errorAction“ je obslužná rutina chyb – akce vyvolaná při selhání primární akce.</span><span class="sxs-lookup"><span data-stu-id="d61b2-175">The “errorAction” is the error handler, the action invoked when the primary action fails.</span></span> <span data-ttu-id="d61b2-176">Tuto proměnnou můžete použít k zavolání koncového hodu pro zpracování chyby nebo odeslání oznámení pro uživatele.</span><span class="sxs-lookup"><span data-stu-id="d61b2-176">You can use this variable to call an error-handling endpoint or send a user notification.</span></span> <span data-ttu-id="d61b2-177">To se může použít pro spojení se sekundárním koncovým bodem v případě, že primární koncový bod není dostupný (např. při nehodě nebo přírodní katastrofě v lokalitě koncového bodu) nebo se může použít pro upozornění koncového bodu pro zpracování chyby.</span><span class="sxs-lookup"><span data-stu-id="d61b2-177">This can be used for reaching a secondary endpoint in the case that the primary is not available (e.g., in the case of a disaster at the endpoint’s site) or can be used for notifying an error handling endpoint.</span></span> <span data-ttu-id="d61b2-178">Stejně jako primární akce může i obslužná rutina mít jednoduchou nebo složenou logiku podle jiných akcí.</span><span class="sxs-lookup"><span data-stu-id="d61b2-178">Just like the primary action, the error action can be simple or composite logic based on other actions.</span></span> <span data-ttu-id="d61b2-179">Pokud se chcete dozvědět, jak vytvořit token SAS, podívejte se na téma [Vytvoření a používání sdíleného přístupového podpisu](https://msdn.microsoft.com/library/azure/jj721951.aspx).</span><span class="sxs-lookup"><span data-stu-id="d61b2-179">To learn how to create a SAS token, refer to [Create and Use a Shared Access Signature](https://msdn.microsoft.com/library/azure/jj721951.aspx).</span></span>

## <a name="recurrence"></a><span data-ttu-id="d61b2-180">recurrence</span><span class="sxs-lookup"><span data-stu-id="d61b2-180">recurrence</span></span>
<span data-ttu-id="d61b2-181">Opakování má několik částí:</span><span class="sxs-lookup"><span data-stu-id="d61b2-181">Recurrence has several parts:</span></span>

* <span data-ttu-id="d61b2-182">Frequency (frekvence): Minuta, hodina, den, týden, měsíc nebo rok</span><span class="sxs-lookup"><span data-stu-id="d61b2-182">Frequency: One of minute, hour, day, week, month, year</span></span>  
* <span data-ttu-id="d61b2-183">Interval: Interval při dané frekvenci opakování</span><span class="sxs-lookup"><span data-stu-id="d61b2-183">Interval: Interval at the given frequency for the recurrence</span></span>  
* <span data-ttu-id="d61b2-184">Prescribed schedule (předepsaný plán): Minuty, hodiny, dny v týdnu, měsíce a dny v měsíci pro opakování</span><span class="sxs-lookup"><span data-stu-id="d61b2-184">Prescribed schedule: Specify minutes, hours, weekdays, months, and monthdays of the recurrence</span></span>  
* <span data-ttu-id="d61b2-185">Count (počet): Počet výskytů</span><span class="sxs-lookup"><span data-stu-id="d61b2-185">Count: Count of occurrences</span></span>  
* <span data-ttu-id="d61b2-186">End time (čas ukončení): Po zadaném čase ukončení se neprovedou žádné úlohy</span><span class="sxs-lookup"><span data-stu-id="d61b2-186">End time: No jobs will execute after the specified end time</span></span>  

<span data-ttu-id="d61b2-187">Úloha se opakuje, jestliže má ve své definici JSON opakující se objekt.</span><span class="sxs-lookup"><span data-stu-id="d61b2-187">A job is recurring if it has a recurring object specified in its JSON definition.</span></span> <span data-ttu-id="d61b2-188">Pokud jsou zadané count i endTime, má přednost to pravidlo dokončení, které nastane jako první.</span><span class="sxs-lookup"><span data-stu-id="d61b2-188">If both count and endTime are specified, the completion rule that occurs first is honored.</span></span>

## <a name="state"></a><span data-ttu-id="d61b2-189">state</span><span class="sxs-lookup"><span data-stu-id="d61b2-189">state</span></span>
<span data-ttu-id="d61b2-190">Stav (state) úlohy může mít jednu ze čtyř hodnot: enabled (zapnuto, disabled (vypnuto), completed (dokončeno) nebo faulted (chyba).</span><span class="sxs-lookup"><span data-stu-id="d61b2-190">The state of the job is one of four values: enabled, disabled, completed, or faulted.</span></span> <span data-ttu-id="d61b2-191">Pomocí PUT nebo PATCH můžete aktualizovat stav úloh a tím jim přidělit stav zapnuto nebo vypnuto.</span><span class="sxs-lookup"><span data-stu-id="d61b2-191">You can PUT or PATCH jobs so as to update them to the enabled or disabled state.</span></span> <span data-ttu-id="d61b2-192">Pokud má úloha stav dokončeno nebo chyba, je to konečný stav, který se už nedá změnit (na úlohu ale stále můžete použít DELETE).</span><span class="sxs-lookup"><span data-stu-id="d61b2-192">If a job has been completed or faulted, that is a final state that cannot be updated (though the job can still be DELETED).</span></span> <span data-ttu-id="d61b2-193">Příklad vlastnosti state:</span><span class="sxs-lookup"><span data-stu-id="d61b2-193">An example of the state property is as follows:</span></span>

        "state": "disabled", // enabled, disabled, completed, or faulted
<span data-ttu-id="d61b2-194">Dokončené úlohy a úlohy, které selhaly, se odstraní po 60 dnech.</span><span class="sxs-lookup"><span data-stu-id="d61b2-194">Completed and faulted jobs are deleted after 60 days.</span></span>

## <a name="status"></a><span data-ttu-id="d61b2-195">status</span><span class="sxs-lookup"><span data-stu-id="d61b2-195">status</span></span>
<span data-ttu-id="d61b2-196">Po zahájení úlohy Scheduleru se vrátí informace o aktuálním statusu (stavu) úlohy.</span><span class="sxs-lookup"><span data-stu-id="d61b2-196">Once a Scheduler job has started, information will be returned about the current status of the job.</span></span> <span data-ttu-id="d61b2-197">Uživatel nemůže tento objekt nastavit – nastaví ho systém.</span><span class="sxs-lookup"><span data-stu-id="d61b2-197">This object is not settable by the user—it’s set by the system.</span></span> <span data-ttu-id="d61b2-198">Je ale obsažený v objektu úlohy (a ne v samostatném propojeném prostředku), aby se mohl snadno získat stav (status) úlohy.</span><span class="sxs-lookup"><span data-stu-id="d61b2-198">However, it is included in the job object (rather than a separate linked resource) so that one can obtain the status of a job easily.</span></span>

<span data-ttu-id="d61b2-199">Stav (status) úlohy obsahuje čas předchozího spuštění (pokud existuje), čas dalšího naplánovaného spuštění (pro probíhající úlohy) a počet provedení úlohy.</span><span class="sxs-lookup"><span data-stu-id="d61b2-199">Job status includes the time of the previous execution (if any), the time of the next scheduled execution (for in-progress jobs), and the execution count of the job.</span></span>

## <a name="retrypolicy"></a><span data-ttu-id="d61b2-200">retryPolicy</span><span class="sxs-lookup"><span data-stu-id="d61b2-200">retryPolicy</span></span>
<span data-ttu-id="d61b2-201">Pro případ, že úloha Scheduleru selže, se můžou zadat zásady opakovaných pokusů a pomocí nich se může nastavit, jestli se má pokus o provedení úlohy opakovat a když ano, tak jak.</span><span class="sxs-lookup"><span data-stu-id="d61b2-201">If a Scheduler job fails, it is possible to specify a retry policy to determine whether and how the action is retried.</span></span> <span data-ttu-id="d61b2-202">To se nastavuje pomocí objektu **retryType** – pokud nejsou žádné zásady opakovaných pokusů, jako v příkladu nahoře, je nastavený na **none**.</span><span class="sxs-lookup"><span data-stu-id="d61b2-202">This is determined by the **retryType** object—it is set to **none** if there is no retry policy, as shown above.</span></span> <span data-ttu-id="d61b2-203">Pokud zásady opakovaných pokusů existují, nastavte ho na **fixed**.</span><span class="sxs-lookup"><span data-stu-id="d61b2-203">Set it to **fixed** if there is a retry policy.</span></span>

<span data-ttu-id="d61b2-204">Pokud chcete nastavit zásady opakovaných pokusů, můžete nastavit další dvě nastavení: interval opakovaných pokusů (**retryInterval**) a počet opakovaných pokusů (**retryCount**).</span><span class="sxs-lookup"><span data-stu-id="d61b2-204">To set a retry policy, two additional settings may be specified: a retry interval (**retryInterval**) and the number of retries (**retryCount**).</span></span>

<span data-ttu-id="d61b2-205">Interval opakovaných pokusů zadaný v objektu **retryInterval** je interval mezi opakovanými pokusy.</span><span class="sxs-lookup"><span data-stu-id="d61b2-205">The retry interval, specified with the **retryInterval** object, is the interval between retries.</span></span> <span data-ttu-id="d61b2-206">Výchozí hodnota je 30 sekund, minimální nastavitelná hodnota je 15 sekund a maximální hodnota je 18 měsíců.</span><span class="sxs-lookup"><span data-stu-id="d61b2-206">Its default value is 30 seconds, its minimum configurable value is 15 seconds, and its maximum value is 18 months.</span></span> <span data-ttu-id="d61b2-207">Pro úlohy v kolekcích úloh Free je minimální nastavitelná hodnota 1 hodina.</span><span class="sxs-lookup"><span data-stu-id="d61b2-207">Jobs in Free job collections have a minimum configurable value of 1 hour.</span></span>  <span data-ttu-id="d61b2-208">Definuje se ve formátu ISO 8601.</span><span class="sxs-lookup"><span data-stu-id="d61b2-208">It is defined in the ISO 8601 format.</span></span> <span data-ttu-id="d61b2-209">Podobně je to s hodnotou počtu opakovaných pokusů nastavených v objektu **retryCount** – je to počet, kolikrát se má pokus opakovat.</span><span class="sxs-lookup"><span data-stu-id="d61b2-209">Similarly, the value of the number of retries is specified with the **retryCount** object; it is the number of times a retry is attempted.</span></span> <span data-ttu-id="d61b2-210">Výchozí hodnota je 4 a maximální hodnota je 20\.</span><span class="sxs-lookup"><span data-stu-id="d61b2-210">Its default value is 4, and its maximum value is 20\.</span></span> <span data-ttu-id="d61b2-211">Obě **retryInterval** a **retryCount** jsou volitelné.</span><span class="sxs-lookup"><span data-stu-id="d61b2-211">Both **retryInterval** and **retryCount** are optional.</span></span> <span data-ttu-id="d61b2-212">Pokud je objekt **retryType** nastavený na **fixed** a nejsou explicitně zadané žádné konkrétní hodnoty, použijí se výchozí hodnoty.</span><span class="sxs-lookup"><span data-stu-id="d61b2-212">They are given their default values if **retryType** is set to **fixed** and no values are specified explicitly.</span></span>

## <a name="see-also"></a><span data-ttu-id="d61b2-213">Viz také</span><span class="sxs-lookup"><span data-stu-id="d61b2-213">See also</span></span>
 [<span data-ttu-id="d61b2-214">Co je Scheduler?</span><span class="sxs-lookup"><span data-stu-id="d61b2-214">What is Scheduler?</span></span>](scheduler-intro.md)

 [<span data-ttu-id="d61b2-215">Úvod do používání Scheduleru na portálu Azure Portal</span><span class="sxs-lookup"><span data-stu-id="d61b2-215">Get started using Scheduler in the Azure portal</span></span>](scheduler-get-started-portal.md)

 [<span data-ttu-id="d61b2-216">Plány a fakturace v Azure Scheduleru</span><span class="sxs-lookup"><span data-stu-id="d61b2-216">Plans and billing in Azure Scheduler</span></span>](scheduler-plans-billing.md)

 [<span data-ttu-id="d61b2-217">Sestavení komplexních plánů a pokročilé opakování v Azure Scheduleru</span><span class="sxs-lookup"><span data-stu-id="d61b2-217">How to build complex schedules and advanced recurrence with Azure Scheduler</span></span>](scheduler-advanced-complexity.md)

 [<span data-ttu-id="d61b2-218">REST API Azure Scheduleru – referenční informace</span><span class="sxs-lookup"><span data-stu-id="d61b2-218">Azure Scheduler REST API reference</span></span>](https://msdn.microsoft.com/library/mt629143)

 [<span data-ttu-id="d61b2-219">Rutiny PowerShellu pro Azure Scheduler – referenční informace</span><span class="sxs-lookup"><span data-stu-id="d61b2-219">Azure Scheduler PowerShell cmdlets reference</span></span>](scheduler-powershell-reference.md)

 [<span data-ttu-id="d61b2-220">Vysoká dostupnost a spolehlivost Azure Scheduleru</span><span class="sxs-lookup"><span data-stu-id="d61b2-220">Azure Scheduler high-availability and reliability</span></span>](scheduler-high-availability-reliability.md)

 [<span data-ttu-id="d61b2-221">Omezení, výchozí hodnoty a chybové kódy Azure Scheduleru</span><span class="sxs-lookup"><span data-stu-id="d61b2-221">Azure Scheduler limits, defaults, and error codes</span></span>](scheduler-limits-defaults-errors.md)

 [<span data-ttu-id="d61b2-222">Odchozí ověření Azure Scheduleru</span><span class="sxs-lookup"><span data-stu-id="d61b2-222">Azure Scheduler outbound authentication</span></span>](scheduler-outbound-authentication.md)

