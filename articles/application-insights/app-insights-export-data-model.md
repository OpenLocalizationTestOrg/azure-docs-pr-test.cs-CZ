---
title: "Azure Application Insights datový Model | Microsoft Docs"
description: "Popisuje vlastnosti exportován průběžné exportu ve formátu JSON a použít jako filtry."
services: application-insights
documentationcenter: 
author: CFreemanwa
manager: carmonm
ms.assetid: cabad41c-0518-4669-887f-3087aef865ea
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 03/21/2016
ms.author: bwren
ms.openlocfilehash: a485ddd555f65473d81896effc4a3562bda71410
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="application-insights-export-data-model"></a><span data-ttu-id="9a9b1-103">Application Insights Export datového modelu</span><span class="sxs-lookup"><span data-stu-id="9a9b1-103">Application Insights Export Data Model</span></span>
<span data-ttu-id="9a9b1-104">Tato tabulka uvádí vlastnosti telemetrická data odesílaná z [Application Insights](app-insights-overview.md) sady SDK k portálu.</span><span class="sxs-lookup"><span data-stu-id="9a9b1-104">This table lists the properties of telemetry sent from the [Application Insights](app-insights-overview.md) SDKs to the portal.</span></span>
<span data-ttu-id="9a9b1-105">Zobrazí se tyto vlastnosti v datovým výstupem z [průběžné exportovat](app-insights-export-telemetry.md).</span><span class="sxs-lookup"><span data-stu-id="9a9b1-105">You'll see these properties in data output from [Continuous Export](app-insights-export-telemetry.md).</span></span>
<span data-ttu-id="9a9b1-106">Zobrazí se také v filtry vlastností v [Explorer metrika](app-insights-metrics-explorer.md) a [diagnostické vyhledávání](app-insights-diagnostic-search.md).</span><span class="sxs-lookup"><span data-stu-id="9a9b1-106">They also appear in property filters in [Metric Explorer](app-insights-metrics-explorer.md) and [Diagnostic Search](app-insights-diagnostic-search.md).</span></span>

<span data-ttu-id="9a9b1-107">Všimněte si body:</span><span class="sxs-lookup"><span data-stu-id="9a9b1-107">Points to note:</span></span>

* <span data-ttu-id="9a9b1-108">`[0]`v těchto tabulkách označuje bod v cestě, kde je nutné vložit index; ale není vždy 0.</span><span class="sxs-lookup"><span data-stu-id="9a9b1-108">`[0]` in these tables denotes a point in the path where you have to insert an index; but it isn't always 0.</span></span>
* <span data-ttu-id="9a9b1-109">Dobách trvání jsou v desetin mikrosekund, takže 10000000 == 1 sekunda.</span><span class="sxs-lookup"><span data-stu-id="9a9b1-109">Time durations are in tenths of a microsecond, so 10000000 == 1 second.</span></span>
* <span data-ttu-id="9a9b1-110">Data a časy jsou UTC a jsou uvedeny ve formátu ISO`yyyy-MM-DDThh:mm:ss.sssZ`</span><span class="sxs-lookup"><span data-stu-id="9a9b1-110">Dates and times are UTC, and are given in the ISO format `yyyy-MM-DDThh:mm:ss.sssZ`</span></span>


## <a name="example"></a><span data-ttu-id="9a9b1-111">Příklad</span><span class="sxs-lookup"><span data-stu-id="9a9b1-111">Example</span></span>
    // A server report about an HTTP request
    {
    "request": [
      {
        "urlData": { // derived from 'url'
          "host": "contoso.org",
          "base": "/",
          "hashTag": ""
        },
        "responseCode": 200, // Sent to client
        "success": true, // Default == responseCode<400
        // Request id becomes the operation id of child events
        "id": "fCOhCdCnZ9I=",  
        "name": "GET Home/Index",
        "count": 1, // 100% / sampling rate
        "durationMetric": {
          "value": 1046804.0, // 10000000 == 1 second
          // Currently the following fields are redundant:
          "count": 1.0,
          "min": 1046804.0,
          "max": 1046804.0,
          "stdDev": 0.0,
          "sampledValue": 1046804.0
        },
        "url": "/"
      }
    ],
    "internal": {
      "data": {
        "id": "7f156650-ef4c-11e5-8453-3f984b167d05",
        "documentVersion": "1.61"
      }
    },
    "context": {
      "device": { // client browser
        "type": "PC",
        "screenResolution": { },
        "roleInstance": "WFWEB14B.fabrikam.net"
      },
      "application": { },
      "location": { // derived from client ip
        "continent": "North America",
        "country": "United States",
        // last octagon is anonymized to 0 at portal:
        "clientip": "168.62.177.0",
        "province": "",
        "city": ""
      },
      "data": {
        "isSynthetic": true, // we identified source as a bot
        // percentage of generated data sent to portal:
        "samplingRate": 100.0,
        "eventTime": "2016-03-21T10:05:45.7334717Z" // UTC
      },
      "user": {
        "isAuthenticated": false,
        "anonId": "us-tx-sn1-azr", // bot agent id
        "anonAcquisitionDate": "0001-01-01T00:00:00Z",
        "authAcquisitionDate": "0001-01-01T00:00:00Z",
        "accountAcquisitionDate": "0001-01-01T00:00:00Z"
      },
      "operation": {
        "id": "fCOhCdCnZ9I=",
        "parentId": "fCOhCdCnZ9I=",
        "name": "GET Home/Index"
      },
      "cloud": { },
      "serverDevice": { },
      "custom": { // set by custom fields of track calls
        "dimensions": [ ],
        "metrics": [ ]
      },
      "session": {
        "id": "65504c10-44a6-489e-b9dc-94184eb00d86",
        "isFirst": true
      }
    }
  <span data-ttu-id="9a9b1-112">}</span><span class="sxs-lookup"><span data-stu-id="9a9b1-112">}</span></span>

## <a name="context"></a><span data-ttu-id="9a9b1-113">Kontext</span><span class="sxs-lookup"><span data-stu-id="9a9b1-113">Context</span></span>
<span data-ttu-id="9a9b1-114">Všechny typy telemetrických dat se předěl doprovází oddíl kontextu.</span><span class="sxs-lookup"><span data-stu-id="9a9b1-114">All types of telemetry are accompanied by a context section.</span></span> <span data-ttu-id="9a9b1-115">Ne všechny z těchto polí, se přenáší se každý datový bod.</span><span class="sxs-lookup"><span data-stu-id="9a9b1-115">Not all of these fields are transmitted with every data point.</span></span>

| <span data-ttu-id="9a9b1-116">Cesta</span><span class="sxs-lookup"><span data-stu-id="9a9b1-116">Path</span></span> | <span data-ttu-id="9a9b1-117">Typ</span><span class="sxs-lookup"><span data-stu-id="9a9b1-117">Type</span></span> | <span data-ttu-id="9a9b1-118">Poznámky</span><span class="sxs-lookup"><span data-stu-id="9a9b1-118">Notes</span></span> |
| --- | --- | --- |
| <span data-ttu-id="9a9b1-119">Context.Custom.Dimensions [0]</span><span class="sxs-lookup"><span data-stu-id="9a9b1-119">context.custom.dimensions [0]</span></span> |<span data-ttu-id="9a9b1-120">objekt]</span><span class="sxs-lookup"><span data-stu-id="9a9b1-120">object [ ]</span></span> |<span data-ttu-id="9a9b1-121">Páry klíč hodnota řetězce nastavit parametrem vlastní vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="9a9b1-121">Key-value string pairs set by custom properties parameter.</span></span> <span data-ttu-id="9a9b1-122">Maximální délka klíče 100, hodnoty maximální délky 1024.</span><span class="sxs-lookup"><span data-stu-id="9a9b1-122">Key max length 100, values max length 1024.</span></span> <span data-ttu-id="9a9b1-123">Více než 100 jedinečné hodnoty vlastnosti lze vyhledat, ale nelze použít v případě segmentace.</span><span class="sxs-lookup"><span data-stu-id="9a9b1-123">More than 100 unique values, the property can be searched but cannot be used for segmentation.</span></span> <span data-ttu-id="9a9b1-124">200 maximální počet klíčů na ikey.</span><span class="sxs-lookup"><span data-stu-id="9a9b1-124">Max 200 keys per ikey.</span></span> |
| <span data-ttu-id="9a9b1-125">Context.Custom.Metrics [0]</span><span class="sxs-lookup"><span data-stu-id="9a9b1-125">context.custom.metrics [0]</span></span> |<span data-ttu-id="9a9b1-126">objekt]</span><span class="sxs-lookup"><span data-stu-id="9a9b1-126">object [ ]</span></span> |<span data-ttu-id="9a9b1-127">Nastavte parametr vlastní měření a TrackMetrics páry klíč hodnota.</span><span class="sxs-lookup"><span data-stu-id="9a9b1-127">Key-value pairs set by custom measurements parameter and by TrackMetrics.</span></span> <span data-ttu-id="9a9b1-128">Maximální délka klíče 100, mohou být číselné hodnoty.</span><span class="sxs-lookup"><span data-stu-id="9a9b1-128">Key max length 100, values may be numeric.</span></span> |
| <span data-ttu-id="9a9b1-129">context.data.eventTime</span><span class="sxs-lookup"><span data-stu-id="9a9b1-129">context.data.eventTime</span></span> |<span data-ttu-id="9a9b1-130">Řetězec</span><span class="sxs-lookup"><span data-stu-id="9a9b1-130">string</span></span> |<span data-ttu-id="9a9b1-131">ČAS UTC</span><span class="sxs-lookup"><span data-stu-id="9a9b1-131">UTC</span></span> |
| <span data-ttu-id="9a9b1-132">context.data.isSynthetic</span><span class="sxs-lookup"><span data-stu-id="9a9b1-132">context.data.isSynthetic</span></span> |<span data-ttu-id="9a9b1-133">Logická hodnota</span><span class="sxs-lookup"><span data-stu-id="9a9b1-133">boolean</span></span> |<span data-ttu-id="9a9b1-134">Žádost se zdá být od robota nebo webový test.</span><span class="sxs-lookup"><span data-stu-id="9a9b1-134">Request appears to come from a bot or web test.</span></span> |
| <span data-ttu-id="9a9b1-135">context.data.samplingRate</span><span class="sxs-lookup"><span data-stu-id="9a9b1-135">context.data.samplingRate</span></span> |<span data-ttu-id="9a9b1-136">Číslo</span><span class="sxs-lookup"><span data-stu-id="9a9b1-136">number</span></span> |<span data-ttu-id="9a9b1-137">Procento telemetrii vygenerovanou sadou SDK, která je odeslána na portál.</span><span class="sxs-lookup"><span data-stu-id="9a9b1-137">Percentage of telemetry generated by the SDK that is sent to portal.</span></span> <span data-ttu-id="9a9b1-138">V rozsahu 0,0 100.0.</span><span class="sxs-lookup"><span data-stu-id="9a9b1-138">Range 0.0-100.0.</span></span> |
| <span data-ttu-id="9a9b1-139">Context.Device</span><span class="sxs-lookup"><span data-stu-id="9a9b1-139">context.device</span></span> |<span data-ttu-id="9a9b1-140">Objekt</span><span class="sxs-lookup"><span data-stu-id="9a9b1-140">object</span></span> |<span data-ttu-id="9a9b1-141">Klientské zařízení</span><span class="sxs-lookup"><span data-stu-id="9a9b1-141">Client device</span></span> |
| <span data-ttu-id="9a9b1-142">Context.Device.Browser</span><span class="sxs-lookup"><span data-stu-id="9a9b1-142">context.device.browser</span></span> |<span data-ttu-id="9a9b1-143">Řetězec</span><span class="sxs-lookup"><span data-stu-id="9a9b1-143">string</span></span> |<span data-ttu-id="9a9b1-144">IE Chrome...</span><span class="sxs-lookup"><span data-stu-id="9a9b1-144">IE, Chrome, ...</span></span> |
| <span data-ttu-id="9a9b1-145">context.device.browserVersion</span><span class="sxs-lookup"><span data-stu-id="9a9b1-145">context.device.browserVersion</span></span> |<span data-ttu-id="9a9b1-146">Řetězec</span><span class="sxs-lookup"><span data-stu-id="9a9b1-146">string</span></span> |<span data-ttu-id="9a9b1-147">Chrome 48,0...</span><span class="sxs-lookup"><span data-stu-id="9a9b1-147">Chrome 48.0, ...</span></span> |
| <span data-ttu-id="9a9b1-148">context.device.deviceModel</span><span class="sxs-lookup"><span data-stu-id="9a9b1-148">context.device.deviceModel</span></span> |<span data-ttu-id="9a9b1-149">Řetězec</span><span class="sxs-lookup"><span data-stu-id="9a9b1-149">string</span></span> | |
| <span data-ttu-id="9a9b1-150">context.device.deviceName</span><span class="sxs-lookup"><span data-stu-id="9a9b1-150">context.device.deviceName</span></span> |<span data-ttu-id="9a9b1-151">Řetězec</span><span class="sxs-lookup"><span data-stu-id="9a9b1-151">string</span></span> | |
| <span data-ttu-id="9a9b1-152">Context.Device.ID</span><span class="sxs-lookup"><span data-stu-id="9a9b1-152">context.device.id</span></span> |<span data-ttu-id="9a9b1-153">Řetězec</span><span class="sxs-lookup"><span data-stu-id="9a9b1-153">string</span></span> | |
| <span data-ttu-id="9a9b1-154">Context.Device.Locale</span><span class="sxs-lookup"><span data-stu-id="9a9b1-154">context.device.locale</span></span> |<span data-ttu-id="9a9b1-155">Řetězec</span><span class="sxs-lookup"><span data-stu-id="9a9b1-155">string</span></span> |<span data-ttu-id="9a9b1-156">de-DE, en-GB...</span><span class="sxs-lookup"><span data-stu-id="9a9b1-156">en-GB, de-DE, ...</span></span> |
| <span data-ttu-id="9a9b1-157">Context.Device.Network</span><span class="sxs-lookup"><span data-stu-id="9a9b1-157">context.device.network</span></span> |<span data-ttu-id="9a9b1-158">Řetězec</span><span class="sxs-lookup"><span data-stu-id="9a9b1-158">string</span></span> | |
| <span data-ttu-id="9a9b1-159">context.device.oemName</span><span class="sxs-lookup"><span data-stu-id="9a9b1-159">context.device.oemName</span></span> |<span data-ttu-id="9a9b1-160">Řetězec</span><span class="sxs-lookup"><span data-stu-id="9a9b1-160">string</span></span> | |
| <span data-ttu-id="9a9b1-161">context.device.osVersion</span><span class="sxs-lookup"><span data-stu-id="9a9b1-161">context.device.osVersion</span></span> |<span data-ttu-id="9a9b1-162">Řetězec</span><span class="sxs-lookup"><span data-stu-id="9a9b1-162">string</span></span> |<span data-ttu-id="9a9b1-163">Hostitelský operační systém</span><span class="sxs-lookup"><span data-stu-id="9a9b1-163">Host OS</span></span> |
| <span data-ttu-id="9a9b1-164">context.device.roleInstance</span><span class="sxs-lookup"><span data-stu-id="9a9b1-164">context.device.roleInstance</span></span> |<span data-ttu-id="9a9b1-165">Řetězec</span><span class="sxs-lookup"><span data-stu-id="9a9b1-165">string</span></span> |<span data-ttu-id="9a9b1-166">ID hostitelského serveru</span><span class="sxs-lookup"><span data-stu-id="9a9b1-166">ID of server host</span></span> |
| <span data-ttu-id="9a9b1-167">context.device.roleName</span><span class="sxs-lookup"><span data-stu-id="9a9b1-167">context.device.roleName</span></span> |<span data-ttu-id="9a9b1-168">Řetězec</span><span class="sxs-lookup"><span data-stu-id="9a9b1-168">string</span></span> | |
| <span data-ttu-id="9a9b1-169">Context.Device.Type</span><span class="sxs-lookup"><span data-stu-id="9a9b1-169">context.device.type</span></span> |<span data-ttu-id="9a9b1-170">Řetězec</span><span class="sxs-lookup"><span data-stu-id="9a9b1-170">string</span></span> |<span data-ttu-id="9a9b1-171">Počítač, prohlížeč...</span><span class="sxs-lookup"><span data-stu-id="9a9b1-171">PC, Browser, ...</span></span> |
| <span data-ttu-id="9a9b1-172">Context.Location</span><span class="sxs-lookup"><span data-stu-id="9a9b1-172">context.location</span></span> |<span data-ttu-id="9a9b1-173">Objekt</span><span class="sxs-lookup"><span data-stu-id="9a9b1-173">object</span></span> |<span data-ttu-id="9a9b1-174">Odvozená od když.</span><span class="sxs-lookup"><span data-stu-id="9a9b1-174">Derived from clientip.</span></span> |
| <span data-ttu-id="9a9b1-175">Context.location.City</span><span class="sxs-lookup"><span data-stu-id="9a9b1-175">context.location.city</span></span> |<span data-ttu-id="9a9b1-176">Řetězec</span><span class="sxs-lookup"><span data-stu-id="9a9b1-176">string</span></span> |<span data-ttu-id="9a9b1-177">Odvozené když, pokud je znám</span><span class="sxs-lookup"><span data-stu-id="9a9b1-177">Derived from clientip, if known</span></span> |
| <span data-ttu-id="9a9b1-178">Context.location.ClientIP</span><span class="sxs-lookup"><span data-stu-id="9a9b1-178">context.location.clientip</span></span> |<span data-ttu-id="9a9b1-179">Řetězec</span><span class="sxs-lookup"><span data-stu-id="9a9b1-179">string</span></span> |<span data-ttu-id="9a9b1-180">Poslední Osmiúhelník je anonymní na hodnotu 0.</span><span class="sxs-lookup"><span data-stu-id="9a9b1-180">Last octagon is anonymized to 0.</span></span> |
| <span data-ttu-id="9a9b1-181">Context.location.Continent</span><span class="sxs-lookup"><span data-stu-id="9a9b1-181">context.location.continent</span></span> |<span data-ttu-id="9a9b1-182">Řetězec</span><span class="sxs-lookup"><span data-stu-id="9a9b1-182">string</span></span> | |
| <span data-ttu-id="9a9b1-183">Context.location.Country</span><span class="sxs-lookup"><span data-stu-id="9a9b1-183">context.location.country</span></span> |<span data-ttu-id="9a9b1-184">Řetězec</span><span class="sxs-lookup"><span data-stu-id="9a9b1-184">string</span></span> | |
| <span data-ttu-id="9a9b1-185">Context.location.Province</span><span class="sxs-lookup"><span data-stu-id="9a9b1-185">context.location.province</span></span> |<span data-ttu-id="9a9b1-186">Řetězec</span><span class="sxs-lookup"><span data-stu-id="9a9b1-186">string</span></span> |<span data-ttu-id="9a9b1-187">Kraj</span><span class="sxs-lookup"><span data-stu-id="9a9b1-187">State or province</span></span> |
| <span data-ttu-id="9a9b1-188">Context.Operation.ID</span><span class="sxs-lookup"><span data-stu-id="9a9b1-188">context.operation.id</span></span> |<span data-ttu-id="9a9b1-189">Řetězec</span><span class="sxs-lookup"><span data-stu-id="9a9b1-189">string</span></span> |<span data-ttu-id="9a9b1-190">Položky, které mají stejné id operace se zobrazují jako související položky v portálu.</span><span class="sxs-lookup"><span data-stu-id="9a9b1-190">Items that have the same operation id are shown as Related Items in the portal.</span></span> <span data-ttu-id="9a9b1-191">Obvykle id požadavku.</span><span class="sxs-lookup"><span data-stu-id="9a9b1-191">Usually the request id.</span></span> |
| <span data-ttu-id="9a9b1-192">Context.Operation.Name</span><span class="sxs-lookup"><span data-stu-id="9a9b1-192">context.operation.name</span></span> |<span data-ttu-id="9a9b1-193">Řetězec</span><span class="sxs-lookup"><span data-stu-id="9a9b1-193">string</span></span> |<span data-ttu-id="9a9b1-194">Adresa URL nebo žádosti o název</span><span class="sxs-lookup"><span data-stu-id="9a9b1-194">url or request name</span></span> |
| <span data-ttu-id="9a9b1-195">context.operation.parentId</span><span class="sxs-lookup"><span data-stu-id="9a9b1-195">context.operation.parentId</span></span> |<span data-ttu-id="9a9b1-196">Řetězec</span><span class="sxs-lookup"><span data-stu-id="9a9b1-196">string</span></span> |<span data-ttu-id="9a9b1-197">Umožňuje vnořené související položky.</span><span class="sxs-lookup"><span data-stu-id="9a9b1-197">Allows nested related items.</span></span> |
| <span data-ttu-id="9a9b1-198">Context.Session.ID</span><span class="sxs-lookup"><span data-stu-id="9a9b1-198">context.session.id</span></span> |<span data-ttu-id="9a9b1-199">Řetězec</span><span class="sxs-lookup"><span data-stu-id="9a9b1-199">string</span></span> |<span data-ttu-id="9a9b1-200">ID skupiny operací z jednoho zdroje.</span><span class="sxs-lookup"><span data-stu-id="9a9b1-200">Id of a group of operations from the same source.</span></span> <span data-ttu-id="9a9b1-201">Po dobu 30 minut bez operace signalizuje ukončení relace.</span><span class="sxs-lookup"><span data-stu-id="9a9b1-201">A period of 30 minutes without an operation signals the end of a session.</span></span> |
| <span data-ttu-id="9a9b1-202">context.session.isFirst</span><span class="sxs-lookup"><span data-stu-id="9a9b1-202">context.session.isFirst</span></span> |<span data-ttu-id="9a9b1-203">Logická hodnota</span><span class="sxs-lookup"><span data-stu-id="9a9b1-203">boolean</span></span> | |
| <span data-ttu-id="9a9b1-204">context.user.accountAcquisitionDate</span><span class="sxs-lookup"><span data-stu-id="9a9b1-204">context.user.accountAcquisitionDate</span></span> |<span data-ttu-id="9a9b1-205">Řetězec</span><span class="sxs-lookup"><span data-stu-id="9a9b1-205">string</span></span> | |
| <span data-ttu-id="9a9b1-206">context.user.anonAcquisitionDate</span><span class="sxs-lookup"><span data-stu-id="9a9b1-206">context.user.anonAcquisitionDate</span></span> |<span data-ttu-id="9a9b1-207">Řetězec</span><span class="sxs-lookup"><span data-stu-id="9a9b1-207">string</span></span> | |
| <span data-ttu-id="9a9b1-208">context.user.anonId</span><span class="sxs-lookup"><span data-stu-id="9a9b1-208">context.user.anonId</span></span> |<span data-ttu-id="9a9b1-209">Řetězec</span><span class="sxs-lookup"><span data-stu-id="9a9b1-209">string</span></span> | |
| <span data-ttu-id="9a9b1-210">context.user.authAcquisitionDate</span><span class="sxs-lookup"><span data-stu-id="9a9b1-210">context.user.authAcquisitionDate</span></span> |<span data-ttu-id="9a9b1-211">Řetězec</span><span class="sxs-lookup"><span data-stu-id="9a9b1-211">string</span></span> |[<span data-ttu-id="9a9b1-212">Ověřený uživatel</span><span class="sxs-lookup"><span data-stu-id="9a9b1-212">Authenticated User</span></span>](app-insights-api-custom-events-metrics.md#authenticated-users) |
| <span data-ttu-id="9a9b1-213">context.user.isAuthenticated</span><span class="sxs-lookup"><span data-stu-id="9a9b1-213">context.user.isAuthenticated</span></span> |<span data-ttu-id="9a9b1-214">Logická hodnota</span><span class="sxs-lookup"><span data-stu-id="9a9b1-214">boolean</span></span> | |
| <span data-ttu-id="9a9b1-215">internal.data.documentVersion</span><span class="sxs-lookup"><span data-stu-id="9a9b1-215">internal.data.documentVersion</span></span> |<span data-ttu-id="9a9b1-216">Řetězec</span><span class="sxs-lookup"><span data-stu-id="9a9b1-216">string</span></span> | |
| <span data-ttu-id="9a9b1-217">internal.data.ID</span><span class="sxs-lookup"><span data-stu-id="9a9b1-217">internal.data.id</span></span> |<span data-ttu-id="9a9b1-218">Řetězec</span><span class="sxs-lookup"><span data-stu-id="9a9b1-218">string</span></span> | |

## <a name="events"></a><span data-ttu-id="9a9b1-219">Události</span><span class="sxs-lookup"><span data-stu-id="9a9b1-219">Events</span></span>
<span data-ttu-id="9a9b1-220">Vlastní události vygenerované [TrackEvent()](app-insights-api-custom-events-metrics.md#trackevent).</span><span class="sxs-lookup"><span data-stu-id="9a9b1-220">Custom events generated by [TrackEvent()](app-insights-api-custom-events-metrics.md#trackevent).</span></span>

| <span data-ttu-id="9a9b1-221">Cesta</span><span class="sxs-lookup"><span data-stu-id="9a9b1-221">Path</span></span> | <span data-ttu-id="9a9b1-222">Typ</span><span class="sxs-lookup"><span data-stu-id="9a9b1-222">Type</span></span> | <span data-ttu-id="9a9b1-223">Poznámky</span><span class="sxs-lookup"><span data-stu-id="9a9b1-223">Notes</span></span> |
| --- | --- | --- |
| <span data-ttu-id="9a9b1-224">počet událostí [0]</span><span class="sxs-lookup"><span data-stu-id="9a9b1-224">event [0] count</span></span> |<span data-ttu-id="9a9b1-225">celé číslo</span><span class="sxs-lookup"><span data-stu-id="9a9b1-225">integer</span></span> |<span data-ttu-id="9a9b1-226">100 / ([vzorkování](app-insights-sampling.md) rychlost).</span><span class="sxs-lookup"><span data-stu-id="9a9b1-226">100/([sampling](app-insights-sampling.md) rate).</span></span> <span data-ttu-id="9a9b1-227">Příklad 4 =&gt; 25 %.</span><span class="sxs-lookup"><span data-stu-id="9a9b1-227">For example 4 =&gt; 25%.</span></span> |
| <span data-ttu-id="9a9b1-228">Název události [0]</span><span class="sxs-lookup"><span data-stu-id="9a9b1-228">event [0] name</span></span> |<span data-ttu-id="9a9b1-229">Řetězec</span><span class="sxs-lookup"><span data-stu-id="9a9b1-229">string</span></span> |<span data-ttu-id="9a9b1-230">Název události.</span><span class="sxs-lookup"><span data-stu-id="9a9b1-230">Event name.</span></span>  <span data-ttu-id="9a9b1-231">Maximální délka 250.</span><span class="sxs-lookup"><span data-stu-id="9a9b1-231">Max length 250.</span></span> |
| <span data-ttu-id="9a9b1-232">Adresa url pro události [0]</span><span class="sxs-lookup"><span data-stu-id="9a9b1-232">event [0] url</span></span> |<span data-ttu-id="9a9b1-233">Řetězec</span><span class="sxs-lookup"><span data-stu-id="9a9b1-233">string</span></span> | |
| <span data-ttu-id="9a9b1-234">události [0] urlData.base</span><span class="sxs-lookup"><span data-stu-id="9a9b1-234">event [0] urlData.base</span></span> |<span data-ttu-id="9a9b1-235">Řetězec</span><span class="sxs-lookup"><span data-stu-id="9a9b1-235">string</span></span> | |
| <span data-ttu-id="9a9b1-236">události [0] urlData.host</span><span class="sxs-lookup"><span data-stu-id="9a9b1-236">event [0] urlData.host</span></span> |<span data-ttu-id="9a9b1-237">Řetězec</span><span class="sxs-lookup"><span data-stu-id="9a9b1-237">string</span></span> | |

## <a name="exceptions"></a><span data-ttu-id="9a9b1-238">Výjimky</span><span class="sxs-lookup"><span data-stu-id="9a9b1-238">Exceptions</span></span>
<span data-ttu-id="9a9b1-239">Sestavy [výjimky](app-insights-asp-net-exceptions.md) na serveru a v prohlížeči.</span><span class="sxs-lookup"><span data-stu-id="9a9b1-239">Reports [exceptions](app-insights-asp-net-exceptions.md) in the server and in the browser.</span></span>

| <span data-ttu-id="9a9b1-240">Cesta</span><span class="sxs-lookup"><span data-stu-id="9a9b1-240">Path</span></span> | <span data-ttu-id="9a9b1-241">Typ</span><span class="sxs-lookup"><span data-stu-id="9a9b1-241">Type</span></span> | <span data-ttu-id="9a9b1-242">Poznámky</span><span class="sxs-lookup"><span data-stu-id="9a9b1-242">Notes</span></span> |
| --- | --- | --- |
| <span data-ttu-id="9a9b1-243">sestavení [0] basicException</span><span class="sxs-lookup"><span data-stu-id="9a9b1-243">basicException [0] assembly</span></span> |<span data-ttu-id="9a9b1-244">Řetězec</span><span class="sxs-lookup"><span data-stu-id="9a9b1-244">string</span></span> | |
| <span data-ttu-id="9a9b1-245">počet basicException [0]</span><span class="sxs-lookup"><span data-stu-id="9a9b1-245">basicException [0] count</span></span> |<span data-ttu-id="9a9b1-246">celé číslo</span><span class="sxs-lookup"><span data-stu-id="9a9b1-246">integer</span></span> |<span data-ttu-id="9a9b1-247">100 / ([vzorkování](app-insights-sampling.md) rychlost).</span><span class="sxs-lookup"><span data-stu-id="9a9b1-247">100/([sampling](app-insights-sampling.md) rate).</span></span> <span data-ttu-id="9a9b1-248">Příklad 4 =&gt; 25 %.</span><span class="sxs-lookup"><span data-stu-id="9a9b1-248">For example 4 =&gt; 25%.</span></span> |
| <span data-ttu-id="9a9b1-249">exceptionGroup basicException [0]</span><span class="sxs-lookup"><span data-stu-id="9a9b1-249">basicException [0] exceptionGroup</span></span> |<span data-ttu-id="9a9b1-250">Řetězec</span><span class="sxs-lookup"><span data-stu-id="9a9b1-250">string</span></span> | |
| <span data-ttu-id="9a9b1-251">exceptionType basicException [0]</span><span class="sxs-lookup"><span data-stu-id="9a9b1-251">basicException [0] exceptionType</span></span> |<span data-ttu-id="9a9b1-252">Řetězec</span><span class="sxs-lookup"><span data-stu-id="9a9b1-252">string</span></span> | |
| <span data-ttu-id="9a9b1-253">failedUserCodeMethod basicException [0]</span><span class="sxs-lookup"><span data-stu-id="9a9b1-253">basicException [0] failedUserCodeMethod</span></span> |<span data-ttu-id="9a9b1-254">Řetězec</span><span class="sxs-lookup"><span data-stu-id="9a9b1-254">string</span></span> | |
| <span data-ttu-id="9a9b1-255">failedUserCodeAssembly basicException [0]</span><span class="sxs-lookup"><span data-stu-id="9a9b1-255">basicException [0] failedUserCodeAssembly</span></span> |<span data-ttu-id="9a9b1-256">Řetězec</span><span class="sxs-lookup"><span data-stu-id="9a9b1-256">string</span></span> | |
| <span data-ttu-id="9a9b1-257">handledAt basicException [0]</span><span class="sxs-lookup"><span data-stu-id="9a9b1-257">basicException [0] handledAt</span></span> |<span data-ttu-id="9a9b1-258">Řetězec</span><span class="sxs-lookup"><span data-stu-id="9a9b1-258">string</span></span> | |
| <span data-ttu-id="9a9b1-259">hasFullStack basicException [0]</span><span class="sxs-lookup"><span data-stu-id="9a9b1-259">basicException [0] hasFullStack</span></span> |<span data-ttu-id="9a9b1-260">Logická hodnota</span><span class="sxs-lookup"><span data-stu-id="9a9b1-260">boolean</span></span> | |
| <span data-ttu-id="9a9b1-261">id basicException [0]</span><span class="sxs-lookup"><span data-stu-id="9a9b1-261">basicException [0] id</span></span> |<span data-ttu-id="9a9b1-262">Řetězec</span><span class="sxs-lookup"><span data-stu-id="9a9b1-262">string</span></span> | |
| <span data-ttu-id="9a9b1-263">Metoda basicException [0]</span><span class="sxs-lookup"><span data-stu-id="9a9b1-263">basicException [0] method</span></span> |<span data-ttu-id="9a9b1-264">Řetězec</span><span class="sxs-lookup"><span data-stu-id="9a9b1-264">string</span></span> | |
| <span data-ttu-id="9a9b1-265">zpráva basicException [0]</span><span class="sxs-lookup"><span data-stu-id="9a9b1-265">basicException [0] message</span></span> |<span data-ttu-id="9a9b1-266">Řetězec</span><span class="sxs-lookup"><span data-stu-id="9a9b1-266">string</span></span> |<span data-ttu-id="9a9b1-267">Zpráva o výjimce.</span><span class="sxs-lookup"><span data-stu-id="9a9b1-267">Exception message.</span></span> <span data-ttu-id="9a9b1-268">Maximální délka 10 tis.</span><span class="sxs-lookup"><span data-stu-id="9a9b1-268">Max length 10k.</span></span> |
| <span data-ttu-id="9a9b1-269">outerExceptionMessage basicException [0]</span><span class="sxs-lookup"><span data-stu-id="9a9b1-269">basicException [0] outerExceptionMessage</span></span> |<span data-ttu-id="9a9b1-270">Řetězec</span><span class="sxs-lookup"><span data-stu-id="9a9b1-270">string</span></span> | |
| <span data-ttu-id="9a9b1-271">outerExceptionThrownAtAssembly basicException [0]</span><span class="sxs-lookup"><span data-stu-id="9a9b1-271">basicException [0] outerExceptionThrownAtAssembly</span></span> |<span data-ttu-id="9a9b1-272">Řetězec</span><span class="sxs-lookup"><span data-stu-id="9a9b1-272">string</span></span> | |
| <span data-ttu-id="9a9b1-273">outerExceptionThrownAtMethod basicException [0]</span><span class="sxs-lookup"><span data-stu-id="9a9b1-273">basicException [0] outerExceptionThrownAtMethod</span></span> |<span data-ttu-id="9a9b1-274">Řetězec</span><span class="sxs-lookup"><span data-stu-id="9a9b1-274">string</span></span> | |
| <span data-ttu-id="9a9b1-275">outerExceptionType basicException [0]</span><span class="sxs-lookup"><span data-stu-id="9a9b1-275">basicException [0] outerExceptionType</span></span> |<span data-ttu-id="9a9b1-276">Řetězec</span><span class="sxs-lookup"><span data-stu-id="9a9b1-276">string</span></span> | |
| <span data-ttu-id="9a9b1-277">outerId basicException [0]</span><span class="sxs-lookup"><span data-stu-id="9a9b1-277">basicException [0] outerId</span></span> |<span data-ttu-id="9a9b1-278">Řetězec</span><span class="sxs-lookup"><span data-stu-id="9a9b1-278">string</span></span> | |
| <span data-ttu-id="9a9b1-279">sestavení [0] parsedStack basicException [0]</span><span class="sxs-lookup"><span data-stu-id="9a9b1-279">basicException [0] parsedStack [0] assembly</span></span> |<span data-ttu-id="9a9b1-280">Řetězec</span><span class="sxs-lookup"><span data-stu-id="9a9b1-280">string</span></span> | |
| <span data-ttu-id="9a9b1-281">Název souboru parsedStack [0] basicException [0]</span><span class="sxs-lookup"><span data-stu-id="9a9b1-281">basicException [0] parsedStack [0] fileName</span></span> |<span data-ttu-id="9a9b1-282">Řetězec</span><span class="sxs-lookup"><span data-stu-id="9a9b1-282">string</span></span> | |
| <span data-ttu-id="9a9b1-283">úroveň parsedStack [0] basicException [0]</span><span class="sxs-lookup"><span data-stu-id="9a9b1-283">basicException [0] parsedStack [0] level</span></span> |<span data-ttu-id="9a9b1-284">celé číslo</span><span class="sxs-lookup"><span data-stu-id="9a9b1-284">integer</span></span> | |
| <span data-ttu-id="9a9b1-285">basicException [0] [0] parsedStack řádku</span><span class="sxs-lookup"><span data-stu-id="9a9b1-285">basicException [0] parsedStack [0] line</span></span> |<span data-ttu-id="9a9b1-286">celé číslo</span><span class="sxs-lookup"><span data-stu-id="9a9b1-286">integer</span></span> | |
| <span data-ttu-id="9a9b1-287">Metoda parsedStack [0] basicException [0]</span><span class="sxs-lookup"><span data-stu-id="9a9b1-287">basicException [0] parsedStack [0] method</span></span> |<span data-ttu-id="9a9b1-288">Řetězec</span><span class="sxs-lookup"><span data-stu-id="9a9b1-288">string</span></span> | |
| <span data-ttu-id="9a9b1-289">Zásobník basicException [0]</span><span class="sxs-lookup"><span data-stu-id="9a9b1-289">basicException [0] stack</span></span> |<span data-ttu-id="9a9b1-290">Řetězec</span><span class="sxs-lookup"><span data-stu-id="9a9b1-290">string</span></span> |<span data-ttu-id="9a9b1-291">Maximální délka 10 TIS</span><span class="sxs-lookup"><span data-stu-id="9a9b1-291">Max length 10k</span></span> |
| <span data-ttu-id="9a9b1-292">typeName basicException [0]</span><span class="sxs-lookup"><span data-stu-id="9a9b1-292">basicException [0] typeName</span></span> |<span data-ttu-id="9a9b1-293">Řetězec</span><span class="sxs-lookup"><span data-stu-id="9a9b1-293">string</span></span> | |

## <a name="trace-messages"></a><span data-ttu-id="9a9b1-294">Trasovací zprávy</span><span class="sxs-lookup"><span data-stu-id="9a9b1-294">Trace Messages</span></span>
<span data-ttu-id="9a9b1-295">Poslal [TrackTrace](app-insights-api-custom-events-metrics.md#tracktrace)a [protokolování adaptéry](app-insights-asp-net-trace-logs.md).</span><span class="sxs-lookup"><span data-stu-id="9a9b1-295">Sent by [TrackTrace](app-insights-api-custom-events-metrics.md#tracktrace), and by the [logging adapters](app-insights-asp-net-trace-logs.md).</span></span>

| <span data-ttu-id="9a9b1-296">Cesta</span><span class="sxs-lookup"><span data-stu-id="9a9b1-296">Path</span></span> | <span data-ttu-id="9a9b1-297">Typ</span><span class="sxs-lookup"><span data-stu-id="9a9b1-297">Type</span></span> | <span data-ttu-id="9a9b1-298">Poznámky</span><span class="sxs-lookup"><span data-stu-id="9a9b1-298">Notes</span></span> |
| --- | --- | --- |
| <span data-ttu-id="9a9b1-299">zprávy [0] Název_protokolovače</span><span class="sxs-lookup"><span data-stu-id="9a9b1-299">message [0] loggerName</span></span> |<span data-ttu-id="9a9b1-300">Řetězec</span><span class="sxs-lookup"><span data-stu-id="9a9b1-300">string</span></span> | |
| <span data-ttu-id="9a9b1-301">zprávy [0] Parametry</span><span class="sxs-lookup"><span data-stu-id="9a9b1-301">message [0] parameters</span></span> |<span data-ttu-id="9a9b1-302">Řetězec</span><span class="sxs-lookup"><span data-stu-id="9a9b1-302">string</span></span> | |
| <span data-ttu-id="9a9b1-303">zprávy [0] nezpracovaná</span><span class="sxs-lookup"><span data-stu-id="9a9b1-303">message [0] raw</span></span> |<span data-ttu-id="9a9b1-304">Řetězec</span><span class="sxs-lookup"><span data-stu-id="9a9b1-304">string</span></span> |<span data-ttu-id="9a9b1-305">Zprávy protokolu, maximální délka 10 tis.</span><span class="sxs-lookup"><span data-stu-id="9a9b1-305">The log message, max length 10k.</span></span> |
| <span data-ttu-id="9a9b1-306">úroveň závažnosti zpráva [0]</span><span class="sxs-lookup"><span data-stu-id="9a9b1-306">message [0] severityLevel</span></span> |<span data-ttu-id="9a9b1-307">Řetězec</span><span class="sxs-lookup"><span data-stu-id="9a9b1-307">string</span></span> | |

## <a name="remote-dependency"></a><span data-ttu-id="9a9b1-308">Vzdálené závislostí</span><span class="sxs-lookup"><span data-stu-id="9a9b1-308">Remote dependency</span></span>
<span data-ttu-id="9a9b1-309">Odesílá TrackDependency.</span><span class="sxs-lookup"><span data-stu-id="9a9b1-309">Sent by TrackDependency.</span></span> <span data-ttu-id="9a9b1-310">Umožňuje sestavu výkonu a využití [volání závislosti](app-insights-asp-net-dependencies.md) v serveru a volání AJAX v prohlížeči.</span><span class="sxs-lookup"><span data-stu-id="9a9b1-310">Used to report performance and usage of [calls to dependencies](app-insights-asp-net-dependencies.md) in the server, and AJAX calls in the browser.</span></span>

| <span data-ttu-id="9a9b1-311">Cesta</span><span class="sxs-lookup"><span data-stu-id="9a9b1-311">Path</span></span> | <span data-ttu-id="9a9b1-312">Typ</span><span class="sxs-lookup"><span data-stu-id="9a9b1-312">Type</span></span> | <span data-ttu-id="9a9b1-313">Poznámky</span><span class="sxs-lookup"><span data-stu-id="9a9b1-313">Notes</span></span> |
| --- | --- | --- |
| <span data-ttu-id="9a9b1-314">asynchronní remoteDependency [0]</span><span class="sxs-lookup"><span data-stu-id="9a9b1-314">remoteDependency [0] async</span></span> |<span data-ttu-id="9a9b1-315">Logická hodnota</span><span class="sxs-lookup"><span data-stu-id="9a9b1-315">boolean</span></span> | |
| <span data-ttu-id="9a9b1-316">baseName remoteDependency [0]</span><span class="sxs-lookup"><span data-stu-id="9a9b1-316">remoteDependency [0] baseName</span></span> |<span data-ttu-id="9a9b1-317">Řetězec</span><span class="sxs-lookup"><span data-stu-id="9a9b1-317">string</span></span> | |
| <span data-ttu-id="9a9b1-318">commandName remoteDependency [0]</span><span class="sxs-lookup"><span data-stu-id="9a9b1-318">remoteDependency [0] commandName</span></span> |<span data-ttu-id="9a9b1-319">Řetězec</span><span class="sxs-lookup"><span data-stu-id="9a9b1-319">string</span></span> |<span data-ttu-id="9a9b1-320">Například "domovskou nebo index"</span><span class="sxs-lookup"><span data-stu-id="9a9b1-320">For example "home/index"</span></span> |
| <span data-ttu-id="9a9b1-321">počet remoteDependency [0]</span><span class="sxs-lookup"><span data-stu-id="9a9b1-321">remoteDependency [0] count</span></span> |<span data-ttu-id="9a9b1-322">celé číslo</span><span class="sxs-lookup"><span data-stu-id="9a9b1-322">integer</span></span> |<span data-ttu-id="9a9b1-323">100 / ([vzorkování](app-insights-sampling.md) rychlost).</span><span class="sxs-lookup"><span data-stu-id="9a9b1-323">100/([sampling](app-insights-sampling.md) rate).</span></span> <span data-ttu-id="9a9b1-324">Příklad 4 =&gt; 25 %.</span><span class="sxs-lookup"><span data-stu-id="9a9b1-324">For example 4 =&gt; 25%.</span></span> |
| <span data-ttu-id="9a9b1-325">dependencyTypeName remoteDependency [0]</span><span class="sxs-lookup"><span data-stu-id="9a9b1-325">remoteDependency [0] dependencyTypeName</span></span> |<span data-ttu-id="9a9b1-326">Řetězec</span><span class="sxs-lookup"><span data-stu-id="9a9b1-326">string</span></span> |<span data-ttu-id="9a9b1-327">PROTOKOLU HTTP, SQL...</span><span class="sxs-lookup"><span data-stu-id="9a9b1-327">HTTP, SQL, ...</span></span> |
| <span data-ttu-id="9a9b1-328">durationMetric.value remoteDependency [0]</span><span class="sxs-lookup"><span data-stu-id="9a9b1-328">remoteDependency [0] durationMetric.value</span></span> |<span data-ttu-id="9a9b1-329">Číslo</span><span class="sxs-lookup"><span data-stu-id="9a9b1-329">number</span></span> |<span data-ttu-id="9a9b1-330">Čas od volání dokončení odpovědi závislostí</span><span class="sxs-lookup"><span data-stu-id="9a9b1-330">Time from call to completion of response by dependency</span></span> |
| <span data-ttu-id="9a9b1-331">id remoteDependency [0]</span><span class="sxs-lookup"><span data-stu-id="9a9b1-331">remoteDependency [0] id</span></span> |<span data-ttu-id="9a9b1-332">Řetězec</span><span class="sxs-lookup"><span data-stu-id="9a9b1-332">string</span></span> | |
| <span data-ttu-id="9a9b1-333">Název remoteDependency [0]</span><span class="sxs-lookup"><span data-stu-id="9a9b1-333">remoteDependency [0] name</span></span> |<span data-ttu-id="9a9b1-334">Řetězec</span><span class="sxs-lookup"><span data-stu-id="9a9b1-334">string</span></span> |<span data-ttu-id="9a9b1-335">Adresa URL.</span><span class="sxs-lookup"><span data-stu-id="9a9b1-335">Url.</span></span> <span data-ttu-id="9a9b1-336">Maximální délka 250.</span><span class="sxs-lookup"><span data-stu-id="9a9b1-336">Max length 250.</span></span> |
| <span data-ttu-id="9a9b1-337">resultCode remoteDependency [0]</span><span class="sxs-lookup"><span data-stu-id="9a9b1-337">remoteDependency [0] resultCode</span></span> |<span data-ttu-id="9a9b1-338">Řetězec</span><span class="sxs-lookup"><span data-stu-id="9a9b1-338">string</span></span> |<span data-ttu-id="9a9b1-339">z HTTP závislostí</span><span class="sxs-lookup"><span data-stu-id="9a9b1-339">from HTTP dependency</span></span> |
| <span data-ttu-id="9a9b1-340">Úspěch remoteDependency [0]</span><span class="sxs-lookup"><span data-stu-id="9a9b1-340">remoteDependency [0] success</span></span> |<span data-ttu-id="9a9b1-341">Logická hodnota</span><span class="sxs-lookup"><span data-stu-id="9a9b1-341">boolean</span></span> | |
| <span data-ttu-id="9a9b1-342">Typ remoteDependency [0]</span><span class="sxs-lookup"><span data-stu-id="9a9b1-342">remoteDependency [0] type</span></span> |<span data-ttu-id="9a9b1-343">Řetězec</span><span class="sxs-lookup"><span data-stu-id="9a9b1-343">string</span></span> |<span data-ttu-id="9a9b1-344">Protokolu HTTP, Sql...</span><span class="sxs-lookup"><span data-stu-id="9a9b1-344">Http, Sql,...</span></span> |
| <span data-ttu-id="9a9b1-345">Adresa url remoteDependency [0]</span><span class="sxs-lookup"><span data-stu-id="9a9b1-345">remoteDependency [0] url</span></span> |<span data-ttu-id="9a9b1-346">Řetězec</span><span class="sxs-lookup"><span data-stu-id="9a9b1-346">string</span></span> |<span data-ttu-id="9a9b1-347">Maximální délka 2000</span><span class="sxs-lookup"><span data-stu-id="9a9b1-347">Max length 2000</span></span> |
| <span data-ttu-id="9a9b1-348">urlData.base remoteDependency [0]</span><span class="sxs-lookup"><span data-stu-id="9a9b1-348">remoteDependency [0] urlData.base</span></span> |<span data-ttu-id="9a9b1-349">Řetězec</span><span class="sxs-lookup"><span data-stu-id="9a9b1-349">string</span></span> |<span data-ttu-id="9a9b1-350">Maximální délka 2000</span><span class="sxs-lookup"><span data-stu-id="9a9b1-350">Max length 2000</span></span> |
| <span data-ttu-id="9a9b1-351">urlData.hashTag remoteDependency [0]</span><span class="sxs-lookup"><span data-stu-id="9a9b1-351">remoteDependency [0] urlData.hashTag</span></span> |<span data-ttu-id="9a9b1-352">Řetězec</span><span class="sxs-lookup"><span data-stu-id="9a9b1-352">string</span></span> | |
| <span data-ttu-id="9a9b1-353">urlData.host remoteDependency [0]</span><span class="sxs-lookup"><span data-stu-id="9a9b1-353">remoteDependency [0] urlData.host</span></span> |<span data-ttu-id="9a9b1-354">Řetězec</span><span class="sxs-lookup"><span data-stu-id="9a9b1-354">string</span></span> |<span data-ttu-id="9a9b1-355">Maximální délka 200</span><span class="sxs-lookup"><span data-stu-id="9a9b1-355">Max length 200</span></span> |

## <a name="requests"></a><span data-ttu-id="9a9b1-356">Požadavky</span><span class="sxs-lookup"><span data-stu-id="9a9b1-356">Requests</span></span>
<span data-ttu-id="9a9b1-357">Poslal [TrackRequest](app-insights-api-custom-events-metrics.md#trackrequest).</span><span class="sxs-lookup"><span data-stu-id="9a9b1-357">Sent by [TrackRequest](app-insights-api-custom-events-metrics.md#trackrequest).</span></span> <span data-ttu-id="9a9b1-358">Standardní moduly pomocí tato doba odezvy serveru sestav, měří na serveru.</span><span class="sxs-lookup"><span data-stu-id="9a9b1-358">The standard modules use this to reports server response time, measured at the server.</span></span>

| <span data-ttu-id="9a9b1-359">Cesta</span><span class="sxs-lookup"><span data-stu-id="9a9b1-359">Path</span></span> | <span data-ttu-id="9a9b1-360">Typ</span><span class="sxs-lookup"><span data-stu-id="9a9b1-360">Type</span></span> | <span data-ttu-id="9a9b1-361">Poznámky</span><span class="sxs-lookup"><span data-stu-id="9a9b1-361">Notes</span></span> |
| --- | --- | --- |
| <span data-ttu-id="9a9b1-362">počet požadavku [0]</span><span class="sxs-lookup"><span data-stu-id="9a9b1-362">request [0] count</span></span> |<span data-ttu-id="9a9b1-363">celé číslo</span><span class="sxs-lookup"><span data-stu-id="9a9b1-363">integer</span></span> |<span data-ttu-id="9a9b1-364">100 / ([vzorkování](app-insights-sampling.md) rychlost).</span><span class="sxs-lookup"><span data-stu-id="9a9b1-364">100/([sampling](app-insights-sampling.md) rate).</span></span> <span data-ttu-id="9a9b1-365">Příklad: 4 =&gt; 25 %.</span><span class="sxs-lookup"><span data-stu-id="9a9b1-365">For example: 4 =&gt; 25%.</span></span> |
| <span data-ttu-id="9a9b1-366">durationMetric.value požadavku [0]</span><span class="sxs-lookup"><span data-stu-id="9a9b1-366">request [0] durationMetric.value</span></span> |<span data-ttu-id="9a9b1-367">Číslo</span><span class="sxs-lookup"><span data-stu-id="9a9b1-367">number</span></span> |<span data-ttu-id="9a9b1-368">Čas požadavku přicházejících do odpovědi.</span><span class="sxs-lookup"><span data-stu-id="9a9b1-368">Time from request arriving to response.</span></span> <span data-ttu-id="9a9b1-369">1e7 == hodnotami 1</span><span class="sxs-lookup"><span data-stu-id="9a9b1-369">1e7 == 1s</span></span> |
| <span data-ttu-id="9a9b1-370">id požadavku [0]</span><span class="sxs-lookup"><span data-stu-id="9a9b1-370">request [0] id</span></span> |<span data-ttu-id="9a9b1-371">Řetězec</span><span class="sxs-lookup"><span data-stu-id="9a9b1-371">string</span></span> |<span data-ttu-id="9a9b1-372">Id operace</span><span class="sxs-lookup"><span data-stu-id="9a9b1-372">Operation id</span></span> |
| <span data-ttu-id="9a9b1-373">Název žádosti [0]</span><span class="sxs-lookup"><span data-stu-id="9a9b1-373">request [0] name</span></span> |<span data-ttu-id="9a9b1-374">Řetězec</span><span class="sxs-lookup"><span data-stu-id="9a9b1-374">string</span></span> |<span data-ttu-id="9a9b1-375">Základní adresa url + GET nebo POST.</span><span class="sxs-lookup"><span data-stu-id="9a9b1-375">GET/POST + url base.</span></span>  <span data-ttu-id="9a9b1-376">Maximální délka 250</span><span class="sxs-lookup"><span data-stu-id="9a9b1-376">Max length 250</span></span> |
| <span data-ttu-id="9a9b1-377">responseCode požadavku [0]</span><span class="sxs-lookup"><span data-stu-id="9a9b1-377">request [0] responseCode</span></span> |<span data-ttu-id="9a9b1-378">celé číslo</span><span class="sxs-lookup"><span data-stu-id="9a9b1-378">integer</span></span> |<span data-ttu-id="9a9b1-379">Odpovědi HTTP odeslané do klienta</span><span class="sxs-lookup"><span data-stu-id="9a9b1-379">HTTP response sent to client</span></span> |
| <span data-ttu-id="9a9b1-380">úspěšné žádosti [0]</span><span class="sxs-lookup"><span data-stu-id="9a9b1-380">request [0] success</span></span> |<span data-ttu-id="9a9b1-381">Logická hodnota</span><span class="sxs-lookup"><span data-stu-id="9a9b1-381">boolean</span></span> |<span data-ttu-id="9a9b1-382">Výchozí == (responseCode &lt; 400)</span><span class="sxs-lookup"><span data-stu-id="9a9b1-382">Default == (responseCode &lt; 400)</span></span> |
| <span data-ttu-id="9a9b1-383">Adresa url požadavku [0]</span><span class="sxs-lookup"><span data-stu-id="9a9b1-383">request [0] url</span></span> |<span data-ttu-id="9a9b1-384">Řetězec</span><span class="sxs-lookup"><span data-stu-id="9a9b1-384">string</span></span> |<span data-ttu-id="9a9b1-385">Není včetně hostitele</span><span class="sxs-lookup"><span data-stu-id="9a9b1-385">Not including host</span></span> |
| <span data-ttu-id="9a9b1-386">urlData.base požadavku [0]</span><span class="sxs-lookup"><span data-stu-id="9a9b1-386">request [0] urlData.base</span></span> |<span data-ttu-id="9a9b1-387">Řetězec</span><span class="sxs-lookup"><span data-stu-id="9a9b1-387">string</span></span> | |
| <span data-ttu-id="9a9b1-388">urlData.hashTag požadavku [0]</span><span class="sxs-lookup"><span data-stu-id="9a9b1-388">request [0] urlData.hashTag</span></span> |<span data-ttu-id="9a9b1-389">Řetězec</span><span class="sxs-lookup"><span data-stu-id="9a9b1-389">string</span></span> | |
| <span data-ttu-id="9a9b1-390">urlData.host požadavku [0]</span><span class="sxs-lookup"><span data-stu-id="9a9b1-390">request [0] urlData.host</span></span> |<span data-ttu-id="9a9b1-391">Řetězec</span><span class="sxs-lookup"><span data-stu-id="9a9b1-391">string</span></span> | |

## <a name="page-view-performance"></a><span data-ttu-id="9a9b1-392">Stránka zobrazení výkonu</span><span class="sxs-lookup"><span data-stu-id="9a9b1-392">Page View Performance</span></span>
<span data-ttu-id="9a9b1-393">Posílá prohlížeč.</span><span class="sxs-lookup"><span data-stu-id="9a9b1-393">Sent by the browser.</span></span> <span data-ttu-id="9a9b1-394">Měří času na zpracování stránky, od uživatele inicializaci žádost zobrazíte kompletní (s výjimkou asynchronní volání AJAX).</span><span class="sxs-lookup"><span data-stu-id="9a9b1-394">Measures the time to process a page, from user initiating the request to display complete (excluding async AJAX calls).</span></span>

<span data-ttu-id="9a9b1-395">Kontext hodnoty zobrazit klientského operačního systému a verze prohlížeče.</span><span class="sxs-lookup"><span data-stu-id="9a9b1-395">Context values show client OS and browser version.</span></span>

| <span data-ttu-id="9a9b1-396">Cesta</span><span class="sxs-lookup"><span data-stu-id="9a9b1-396">Path</span></span> | <span data-ttu-id="9a9b1-397">Typ</span><span class="sxs-lookup"><span data-stu-id="9a9b1-397">Type</span></span> | <span data-ttu-id="9a9b1-398">Poznámky</span><span class="sxs-lookup"><span data-stu-id="9a9b1-398">Notes</span></span> |
| --- | --- | --- |
| <span data-ttu-id="9a9b1-399">clientProcess.value clientPerformance [0]</span><span class="sxs-lookup"><span data-stu-id="9a9b1-399">clientPerformance [0] clientProcess.value</span></span> |<span data-ttu-id="9a9b1-400">celé číslo</span><span class="sxs-lookup"><span data-stu-id="9a9b1-400">integer</span></span> |<span data-ttu-id="9a9b1-401">Čas od konce přijetí HTML k zobrazení stránky.</span><span class="sxs-lookup"><span data-stu-id="9a9b1-401">Time from end of receiving the HTML to displaying the page.</span></span> |
| <span data-ttu-id="9a9b1-402">Název clientPerformance [0]</span><span class="sxs-lookup"><span data-stu-id="9a9b1-402">clientPerformance [0] name</span></span> |<span data-ttu-id="9a9b1-403">Řetězec</span><span class="sxs-lookup"><span data-stu-id="9a9b1-403">string</span></span> | |
| <span data-ttu-id="9a9b1-404">networkConnection.value clientPerformance [0]</span><span class="sxs-lookup"><span data-stu-id="9a9b1-404">clientPerformance [0] networkConnection.value</span></span> |<span data-ttu-id="9a9b1-405">celé číslo</span><span class="sxs-lookup"><span data-stu-id="9a9b1-405">integer</span></span> |<span data-ttu-id="9a9b1-406">Čas potřebný k vytvoření síťového připojení.</span><span class="sxs-lookup"><span data-stu-id="9a9b1-406">Time taken to establish a network connection.</span></span> |
| <span data-ttu-id="9a9b1-407">receiveRequest.value clientPerformance [0]</span><span class="sxs-lookup"><span data-stu-id="9a9b1-407">clientPerformance [0] receiveRequest.value</span></span> |<span data-ttu-id="9a9b1-408">celé číslo</span><span class="sxs-lookup"><span data-stu-id="9a9b1-408">integer</span></span> |<span data-ttu-id="9a9b1-409">Čas od konce odesílání požadavku pro příjem kódu HTML v odpovědi.</span><span class="sxs-lookup"><span data-stu-id="9a9b1-409">Time from end of sending the request to receiving the HTML in reply.</span></span> |
| <span data-ttu-id="9a9b1-410">sendRequest.value clientPerformance [0]</span><span class="sxs-lookup"><span data-stu-id="9a9b1-410">clientPerformance [0] sendRequest.value</span></span> |<span data-ttu-id="9a9b1-411">celé číslo</span><span class="sxs-lookup"><span data-stu-id="9a9b1-411">integer</span></span> |<span data-ttu-id="9a9b1-412">Z čas potřebný k odeslání požadavku HTTP.</span><span class="sxs-lookup"><span data-stu-id="9a9b1-412">Time from taken to send the HTTP request.</span></span> |
| <span data-ttu-id="9a9b1-413">total.value clientPerformance [0]</span><span class="sxs-lookup"><span data-stu-id="9a9b1-413">clientPerformance [0] total.value</span></span> |<span data-ttu-id="9a9b1-414">celé číslo</span><span class="sxs-lookup"><span data-stu-id="9a9b1-414">integer</span></span> |<span data-ttu-id="9a9b1-415">Čas spuštění odeslat požadavek na zobrazení stránky.</span><span class="sxs-lookup"><span data-stu-id="9a9b1-415">Time from starting to send the request to displaying the page.</span></span> |
| <span data-ttu-id="9a9b1-416">Adresa url clientPerformance [0]</span><span class="sxs-lookup"><span data-stu-id="9a9b1-416">clientPerformance [0] url</span></span> |<span data-ttu-id="9a9b1-417">Řetězec</span><span class="sxs-lookup"><span data-stu-id="9a9b1-417">string</span></span> |<span data-ttu-id="9a9b1-418">Adresa URL této žádosti</span><span class="sxs-lookup"><span data-stu-id="9a9b1-418">URL of this request</span></span> |
| <span data-ttu-id="9a9b1-419">urlData.base clientPerformance [0]</span><span class="sxs-lookup"><span data-stu-id="9a9b1-419">clientPerformance [0] urlData.base</span></span> |<span data-ttu-id="9a9b1-420">Řetězec</span><span class="sxs-lookup"><span data-stu-id="9a9b1-420">string</span></span> | |
| <span data-ttu-id="9a9b1-421">urlData.hashTag clientPerformance [0]</span><span class="sxs-lookup"><span data-stu-id="9a9b1-421">clientPerformance [0] urlData.hashTag</span></span> |<span data-ttu-id="9a9b1-422">Řetězec</span><span class="sxs-lookup"><span data-stu-id="9a9b1-422">string</span></span> | |
| <span data-ttu-id="9a9b1-423">urlData.host clientPerformance [0]</span><span class="sxs-lookup"><span data-stu-id="9a9b1-423">clientPerformance [0] urlData.host</span></span> |<span data-ttu-id="9a9b1-424">Řetězec</span><span class="sxs-lookup"><span data-stu-id="9a9b1-424">string</span></span> | |
| <span data-ttu-id="9a9b1-425">urlData.protocol clientPerformance [0]</span><span class="sxs-lookup"><span data-stu-id="9a9b1-425">clientPerformance [0] urlData.protocol</span></span> |<span data-ttu-id="9a9b1-426">Řetězec</span><span class="sxs-lookup"><span data-stu-id="9a9b1-426">string</span></span> | |

## <a name="page-views"></a><span data-ttu-id="9a9b1-427">Zobrazení stránky</span><span class="sxs-lookup"><span data-stu-id="9a9b1-427">Page Views</span></span>
<span data-ttu-id="9a9b1-428">Poslal trackPageView() nebo [stopTrackPage](app-insights-api-custom-events-metrics.md#page-views)</span><span class="sxs-lookup"><span data-stu-id="9a9b1-428">Sent by trackPageView() or [stopTrackPage](app-insights-api-custom-events-metrics.md#page-views)</span></span>

| <span data-ttu-id="9a9b1-429">Cesta</span><span class="sxs-lookup"><span data-stu-id="9a9b1-429">Path</span></span> | <span data-ttu-id="9a9b1-430">Typ</span><span class="sxs-lookup"><span data-stu-id="9a9b1-430">Type</span></span> | <span data-ttu-id="9a9b1-431">Poznámky</span><span class="sxs-lookup"><span data-stu-id="9a9b1-431">Notes</span></span> |
| --- | --- | --- |
| <span data-ttu-id="9a9b1-432">Počet zobrazení [0]</span><span class="sxs-lookup"><span data-stu-id="9a9b1-432">view [0] count</span></span> |<span data-ttu-id="9a9b1-433">celé číslo</span><span class="sxs-lookup"><span data-stu-id="9a9b1-433">integer</span></span> |<span data-ttu-id="9a9b1-434">100 / ([vzorkování](app-insights-sampling.md) rychlost).</span><span class="sxs-lookup"><span data-stu-id="9a9b1-434">100/([sampling](app-insights-sampling.md) rate).</span></span> <span data-ttu-id="9a9b1-435">Příklad 4 =&gt; 25 %.</span><span class="sxs-lookup"><span data-stu-id="9a9b1-435">For example 4 =&gt; 25%.</span></span> |
| <span data-ttu-id="9a9b1-436">zobrazení [0] durationMetric.value</span><span class="sxs-lookup"><span data-stu-id="9a9b1-436">view [0] durationMetric.value</span></span> |<span data-ttu-id="9a9b1-437">celé číslo</span><span class="sxs-lookup"><span data-stu-id="9a9b1-437">integer</span></span> |<span data-ttu-id="9a9b1-438">Volitelně můžete nastavit v trackPageView() nebo startTrackPage() - hodnota stopTrackPage().</span><span class="sxs-lookup"><span data-stu-id="9a9b1-438">Value optionally set in trackPageView() or by startTrackPage() - stopTrackPage().</span></span> <span data-ttu-id="9a9b1-439">Není stejný jako clientPerformance hodnoty.</span><span class="sxs-lookup"><span data-stu-id="9a9b1-439">Not the same as clientPerformance values.</span></span> |
| <span data-ttu-id="9a9b1-440">Název zobrazení [0]</span><span class="sxs-lookup"><span data-stu-id="9a9b1-440">view [0] name</span></span> |<span data-ttu-id="9a9b1-441">Řetězec</span><span class="sxs-lookup"><span data-stu-id="9a9b1-441">string</span></span> |<span data-ttu-id="9a9b1-442">Název stránky.</span><span class="sxs-lookup"><span data-stu-id="9a9b1-442">Page title.</span></span>  <span data-ttu-id="9a9b1-443">Maximální délka 250</span><span class="sxs-lookup"><span data-stu-id="9a9b1-443">Max length 250</span></span> |
| <span data-ttu-id="9a9b1-444">Adresa url zobrazení [0]</span><span class="sxs-lookup"><span data-stu-id="9a9b1-444">view [0] url</span></span> |<span data-ttu-id="9a9b1-445">Řetězec</span><span class="sxs-lookup"><span data-stu-id="9a9b1-445">string</span></span> | |
| <span data-ttu-id="9a9b1-446">zobrazení [0] urlData.base</span><span class="sxs-lookup"><span data-stu-id="9a9b1-446">view [0] urlData.base</span></span> |<span data-ttu-id="9a9b1-447">Řetězec</span><span class="sxs-lookup"><span data-stu-id="9a9b1-447">string</span></span> | |
| <span data-ttu-id="9a9b1-448">zobrazení [0] urlData.hashTag</span><span class="sxs-lookup"><span data-stu-id="9a9b1-448">view [0] urlData.hashTag</span></span> |<span data-ttu-id="9a9b1-449">Řetězec</span><span class="sxs-lookup"><span data-stu-id="9a9b1-449">string</span></span> | |
| <span data-ttu-id="9a9b1-450">zobrazení [0] urlData.host</span><span class="sxs-lookup"><span data-stu-id="9a9b1-450">view [0] urlData.host</span></span> |<span data-ttu-id="9a9b1-451">Řetězec</span><span class="sxs-lookup"><span data-stu-id="9a9b1-451">string</span></span> | |

## <a name="availability"></a><span data-ttu-id="9a9b1-452">Dostupnost</span><span class="sxs-lookup"><span data-stu-id="9a9b1-452">Availability</span></span>
<span data-ttu-id="9a9b1-453">Sestavy [testy dostupnosti webu](app-insights-monitor-web-app-availability.md).</span><span class="sxs-lookup"><span data-stu-id="9a9b1-453">Reports [availability web tests](app-insights-monitor-web-app-availability.md).</span></span>

| <span data-ttu-id="9a9b1-454">Cesta</span><span class="sxs-lookup"><span data-stu-id="9a9b1-454">Path</span></span> | <span data-ttu-id="9a9b1-455">Typ</span><span class="sxs-lookup"><span data-stu-id="9a9b1-455">Type</span></span> | <span data-ttu-id="9a9b1-456">Poznámky</span><span class="sxs-lookup"><span data-stu-id="9a9b1-456">Notes</span></span> |
| --- | --- | --- |
| <span data-ttu-id="9a9b1-457">availabilityMetric.name dostupnosti [0]</span><span class="sxs-lookup"><span data-stu-id="9a9b1-457">availability [0] availabilityMetric.name</span></span> |<span data-ttu-id="9a9b1-458">Řetězec</span><span class="sxs-lookup"><span data-stu-id="9a9b1-458">string</span></span> |<span data-ttu-id="9a9b1-459">dostupnosti</span><span class="sxs-lookup"><span data-stu-id="9a9b1-459">availability</span></span> |
| <span data-ttu-id="9a9b1-460">availabilityMetric.value dostupnosti [0]</span><span class="sxs-lookup"><span data-stu-id="9a9b1-460">availability [0] availabilityMetric.value</span></span> |<span data-ttu-id="9a9b1-461">Číslo</span><span class="sxs-lookup"><span data-stu-id="9a9b1-461">number</span></span> |<span data-ttu-id="9a9b1-462">1.0 nebo 0,0</span><span class="sxs-lookup"><span data-stu-id="9a9b1-462">1.0 or 0.0</span></span> |
| <span data-ttu-id="9a9b1-463">počet dostupnosti [0]</span><span class="sxs-lookup"><span data-stu-id="9a9b1-463">availability [0] count</span></span> |<span data-ttu-id="9a9b1-464">celé číslo</span><span class="sxs-lookup"><span data-stu-id="9a9b1-464">integer</span></span> |<span data-ttu-id="9a9b1-465">100 / ([vzorkování](app-insights-sampling.md) rychlost).</span><span class="sxs-lookup"><span data-stu-id="9a9b1-465">100/([sampling](app-insights-sampling.md) rate).</span></span> <span data-ttu-id="9a9b1-466">Příklad 4 =&gt; 25 %.</span><span class="sxs-lookup"><span data-stu-id="9a9b1-466">For example 4 =&gt; 25%.</span></span> |
| <span data-ttu-id="9a9b1-467">dataSizeMetric.name dostupnosti [0]</span><span class="sxs-lookup"><span data-stu-id="9a9b1-467">availability [0] dataSizeMetric.name</span></span> |<span data-ttu-id="9a9b1-468">Řetězec</span><span class="sxs-lookup"><span data-stu-id="9a9b1-468">string</span></span> | |
| <span data-ttu-id="9a9b1-469">dataSizeMetric.value dostupnosti [0]</span><span class="sxs-lookup"><span data-stu-id="9a9b1-469">availability [0] dataSizeMetric.value</span></span> |<span data-ttu-id="9a9b1-470">celé číslo</span><span class="sxs-lookup"><span data-stu-id="9a9b1-470">integer</span></span> | |
| <span data-ttu-id="9a9b1-471">durationMetric.name dostupnosti [0]</span><span class="sxs-lookup"><span data-stu-id="9a9b1-471">availability [0] durationMetric.name</span></span> |<span data-ttu-id="9a9b1-472">Řetězec</span><span class="sxs-lookup"><span data-stu-id="9a9b1-472">string</span></span> | |
| <span data-ttu-id="9a9b1-473">durationMetric.value dostupnosti [0]</span><span class="sxs-lookup"><span data-stu-id="9a9b1-473">availability [0] durationMetric.value</span></span> |<span data-ttu-id="9a9b1-474">Číslo</span><span class="sxs-lookup"><span data-stu-id="9a9b1-474">number</span></span> |<span data-ttu-id="9a9b1-475">Doba trvání testu.</span><span class="sxs-lookup"><span data-stu-id="9a9b1-475">Duration of test.</span></span> <span data-ttu-id="9a9b1-476">1e7 == hodnotami 1</span><span class="sxs-lookup"><span data-stu-id="9a9b1-476">1e7==1s</span></span> |
| <span data-ttu-id="9a9b1-477">zpráva dostupnosti [0]</span><span class="sxs-lookup"><span data-stu-id="9a9b1-477">availability [0] message</span></span> |<span data-ttu-id="9a9b1-478">Řetězec</span><span class="sxs-lookup"><span data-stu-id="9a9b1-478">string</span></span> |<span data-ttu-id="9a9b1-479">Selhání diagnostiky</span><span class="sxs-lookup"><span data-stu-id="9a9b1-479">Failure diagnostic</span></span> |
| <span data-ttu-id="9a9b1-480">výsledek dostupnosti [0]</span><span class="sxs-lookup"><span data-stu-id="9a9b1-480">availability [0] result</span></span> |<span data-ttu-id="9a9b1-481">Řetězec</span><span class="sxs-lookup"><span data-stu-id="9a9b1-481">string</span></span> |<span data-ttu-id="9a9b1-482">Přijetí nebo vyloučení</span><span class="sxs-lookup"><span data-stu-id="9a9b1-482">Pass/Fail</span></span> |
| <span data-ttu-id="9a9b1-483">runLocation dostupnosti [0]</span><span class="sxs-lookup"><span data-stu-id="9a9b1-483">availability [0] runLocation</span></span> |<span data-ttu-id="9a9b1-484">Řetězec</span><span class="sxs-lookup"><span data-stu-id="9a9b1-484">string</span></span> |<span data-ttu-id="9a9b1-485">Geograficky zdroj žádosti http</span><span class="sxs-lookup"><span data-stu-id="9a9b1-485">Geo source of http req</span></span> |
| <span data-ttu-id="9a9b1-486">Název_testu dostupnosti [0]</span><span class="sxs-lookup"><span data-stu-id="9a9b1-486">availability [0] testName</span></span> |<span data-ttu-id="9a9b1-487">Řetězec</span><span class="sxs-lookup"><span data-stu-id="9a9b1-487">string</span></span> | |
| <span data-ttu-id="9a9b1-488">testRunId dostupnosti [0]</span><span class="sxs-lookup"><span data-stu-id="9a9b1-488">availability [0] testRunId</span></span> |<span data-ttu-id="9a9b1-489">Řetězec</span><span class="sxs-lookup"><span data-stu-id="9a9b1-489">string</span></span> | |
| <span data-ttu-id="9a9b1-490">testTimestamp dostupnosti [0]</span><span class="sxs-lookup"><span data-stu-id="9a9b1-490">availability [0] testTimestamp</span></span> |<span data-ttu-id="9a9b1-491">Řetězec</span><span class="sxs-lookup"><span data-stu-id="9a9b1-491">string</span></span> | |

## <a name="metrics"></a><span data-ttu-id="9a9b1-492">Metriky</span><span class="sxs-lookup"><span data-stu-id="9a9b1-492">Metrics</span></span>
<span data-ttu-id="9a9b1-493">Generované TrackMetric().</span><span class="sxs-lookup"><span data-stu-id="9a9b1-493">Generated by TrackMetric().</span></span>

<span data-ttu-id="9a9b1-494">Metriky hodnota je nalezena v context.custom.metrics[0]</span><span class="sxs-lookup"><span data-stu-id="9a9b1-494">The metric value is found in context.custom.metrics[0]</span></span>

<span data-ttu-id="9a9b1-495">Například:</span><span class="sxs-lookup"><span data-stu-id="9a9b1-495">For example:</span></span>

    {
     "metric": [ ],
     "context": {
     ...
     "custom": {
        "dimensions": [
          { "ProcessId": "4068" }
        ],
        "metrics": [
          {
            "dispatchRate": {
              "value": 0.001295,
              "count": 1.0,
              "min": 0.001295,
              "max": 0.001295,
              "stdDev": 0.0,
              "sampledValue": 0.001295,
              "sum": 0.001295
            }
          }
         } ] }
    }

## <a name="about-metric-values"></a><span data-ttu-id="9a9b1-496">O metriky hodnoty</span><span class="sxs-lookup"><span data-stu-id="9a9b1-496">About metric values</span></span>
<span data-ttu-id="9a9b1-497">Metriky, v metriky sestavy i jinde, jsou uvedeny se strukturou standardní objektu.</span><span class="sxs-lookup"><span data-stu-id="9a9b1-497">Metric values, both in metric reports and elsewhere, are reported with a standard object structure.</span></span> <span data-ttu-id="9a9b1-498">Například:</span><span class="sxs-lookup"><span data-stu-id="9a9b1-498">For example:</span></span>

      "durationMetric": {
        "name": "contoso.org",
        "type": "Aggregation",
        "value": 468.71603053650279,
        "count": 1.0,
        "min": 468.71603053650279,
        "max": 468.71603053650279,
        "stdDev": 0.0,
        "sampledValue": 468.71603053650279
      }

<span data-ttu-id="9a9b1-499">Aktuálně – Přestože to může změnit v budoucnu – všechny hodnoty nahlásila standardní moduly SDK `count==1` a jenom `name` a `value` pole jsou užitečné.</span><span class="sxs-lookup"><span data-stu-id="9a9b1-499">Currently - though this might change in the future - in all values reported from the standard SDK modules, `count==1` and only the `name` and `value` fields are useful.</span></span> <span data-ttu-id="9a9b1-500">Pouze případ, kdy by být odlišné by, pokud napíšete voláními TrackMetric v můžete nastavit další parametry.</span><span class="sxs-lookup"><span data-stu-id="9a9b1-500">The only case where they would be different would be if you write your own TrackMetric calls in which you set the other parameters.</span></span>

<span data-ttu-id="9a9b1-501">V ostatních polích účelem je umožnit metriky mají agregovat v sadě SDK pro omezení provozu na portál.</span><span class="sxs-lookup"><span data-stu-id="9a9b1-501">The purpose of the other fields is to allow metrics to be aggregated in the SDK, to reduce traffic to the portal.</span></span> <span data-ttu-id="9a9b1-502">Například může průměrná několik následných odečty před odesláním všechny metriky sestavy.</span><span class="sxs-lookup"><span data-stu-id="9a9b1-502">For example, you could average several successive readings before sending each metric report.</span></span> <span data-ttu-id="9a9b1-503">Potom by vypočítat min, max, směrodatná odchylka a celkovou hodnotu (suma nebo průměr) a nastavte počet počtu odečty reprezentována sestavy.</span><span class="sxs-lookup"><span data-stu-id="9a9b1-503">Then you would calculate the min, max, standard deviation and aggregate value (sum or average) and set count to the number of readings represented by the report.</span></span>

<span data-ttu-id="9a9b1-504">V tabulkách výš jsme zapomněli málo používané pole count, min, max, stdDev a sampledValue.</span><span class="sxs-lookup"><span data-stu-id="9a9b1-504">In the tables above, we have omitted the rarely-used fields count, min, max, stdDev and sampledValue.</span></span>

<span data-ttu-id="9a9b1-505">Namísto předem prostředku metriky, můžete použít [vzorkování](app-insights-sampling.md) Pokud potřebujete snížit objem telemetrie.</span><span class="sxs-lookup"><span data-stu-id="9a9b1-505">Instead of pre-aggregating metrics, you can use [sampling](app-insights-sampling.md) if you need to reduce the volume of telemetry.</span></span>

### <a name="durations"></a><span data-ttu-id="9a9b1-506">Doby trvání</span><span class="sxs-lookup"><span data-stu-id="9a9b1-506">Durations</span></span>
<span data-ttu-id="9a9b1-507">Pokud není uvedeno jinak, jinak jsou reprezentované doby trvání v desetin mikrosekund, tak, aby 10000000.0 znamená 1 sekunda.</span><span class="sxs-lookup"><span data-stu-id="9a9b1-507">Except where otherwise noted, durations are represented in tenths of a microsecond, so that 10000000.0 means 1 second.</span></span>

## <a name="see-also"></a><span data-ttu-id="9a9b1-508">Viz také</span><span class="sxs-lookup"><span data-stu-id="9a9b1-508">See also</span></span>
* [<span data-ttu-id="9a9b1-509">Application Insights</span><span class="sxs-lookup"><span data-stu-id="9a9b1-509">Application Insights</span></span>](app-insights-overview.md)
* [<span data-ttu-id="9a9b1-510">Průběžné exportu</span><span class="sxs-lookup"><span data-stu-id="9a9b1-510">Continuous Export</span></span>](app-insights-export-telemetry.md)
* [<span data-ttu-id="9a9b1-511">Ukázky kódu</span><span class="sxs-lookup"><span data-stu-id="9a9b1-511">Code samples</span></span>](app-insights-export-telemetry.md#code-samples)
