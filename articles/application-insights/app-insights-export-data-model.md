---
title: "aaaAzure Application Insights datový Model | Microsoft Docs"
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
ms.openlocfilehash: 5ff3ce7953b91cc69b5d96c0ea9b6d58a6016e61
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="application-insights-export-data-model"></a><span data-ttu-id="73612-103">Application Insights Export datového modelu</span><span class="sxs-lookup"><span data-stu-id="73612-103">Application Insights Export Data Model</span></span>
<span data-ttu-id="73612-104">Tato tabulka uvádí vlastnosti hello telemetrická data odesílaná z hello [Application Insights](app-insights-overview.md) portál toohello sady SDK.</span><span class="sxs-lookup"><span data-stu-id="73612-104">This table lists hello properties of telemetry sent from hello [Application Insights](app-insights-overview.md) SDKs toohello portal.</span></span>
<span data-ttu-id="73612-105">Zobrazí se tyto vlastnosti v datovým výstupem z [průběžné exportovat](app-insights-export-telemetry.md).</span><span class="sxs-lookup"><span data-stu-id="73612-105">You'll see these properties in data output from [Continuous Export](app-insights-export-telemetry.md).</span></span>
<span data-ttu-id="73612-106">Zobrazí se také v filtry vlastností v [Explorer metrika](app-insights-metrics-explorer.md) a [diagnostické vyhledávání](app-insights-diagnostic-search.md).</span><span class="sxs-lookup"><span data-stu-id="73612-106">They also appear in property filters in [Metric Explorer](app-insights-metrics-explorer.md) and [Diagnostic Search](app-insights-diagnostic-search.md).</span></span>

<span data-ttu-id="73612-107">Toonote body:</span><span class="sxs-lookup"><span data-stu-id="73612-107">Points toonote:</span></span>

* <span data-ttu-id="73612-108">`[0]`v těchto tabulkách označuje bod v cestě hello, ve které máte tooinsert index; ale není vždy 0.</span><span class="sxs-lookup"><span data-stu-id="73612-108">`[0]` in these tables denotes a point in hello path where you have tooinsert an index; but it isn't always 0.</span></span>
* <span data-ttu-id="73612-109">Dobách trvání jsou v desetin mikrosekund, takže 10000000 == 1 sekunda.</span><span class="sxs-lookup"><span data-stu-id="73612-109">Time durations are in tenths of a microsecond, so 10000000 == 1 second.</span></span>
* <span data-ttu-id="73612-110">Data a časy jsou UTC a jsou uvedeny ve formátu ISO hello`yyyy-MM-DDThh:mm:ss.sssZ`</span><span class="sxs-lookup"><span data-stu-id="73612-110">Dates and times are UTC, and are given in hello ISO format `yyyy-MM-DDThh:mm:ss.sssZ`</span></span>


## <a name="example"></a><span data-ttu-id="73612-111">Příklad</span><span class="sxs-lookup"><span data-stu-id="73612-111">Example</span></span>
    // A server report about an HTTP request
    {
    "request": [
      {
        "urlData": { // derived from 'url'
          "host": "contoso.org",
          "base": "/",
          "hashTag": ""
        },
        "responseCode": 200, // Sent tooclient
        "success": true, // Default == responseCode<400
        // Request id becomes hello operation id of child events
        "id": "fCOhCdCnZ9I=",  
        "name": "GET Home/Index",
        "count": 1, // 100% / sampling rate
        "durationMetric": {
          "value": 1046804.0, // 10000000 == 1 second
          // Currently hello following fields are redundant:
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
        // last octagon is anonymized too0 at portal:
        "clientip": "168.62.177.0",
        "province": "",
        "city": ""
      },
      "data": {
        "isSynthetic": true, // we identified source as a bot
        // percentage of generated data sent tooportal:
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
  <span data-ttu-id="73612-112">}</span><span class="sxs-lookup"><span data-stu-id="73612-112">}</span></span>

## <a name="context"></a><span data-ttu-id="73612-113">Kontext</span><span class="sxs-lookup"><span data-stu-id="73612-113">Context</span></span>
<span data-ttu-id="73612-114">Všechny typy telemetrických dat se předěl doprovází oddíl kontextu.</span><span class="sxs-lookup"><span data-stu-id="73612-114">All types of telemetry are accompanied by a context section.</span></span> <span data-ttu-id="73612-115">Ne všechny z těchto polí, se přenáší se každý datový bod.</span><span class="sxs-lookup"><span data-stu-id="73612-115">Not all of these fields are transmitted with every data point.</span></span>

| <span data-ttu-id="73612-116">Cesta</span><span class="sxs-lookup"><span data-stu-id="73612-116">Path</span></span> | <span data-ttu-id="73612-117">Typ</span><span class="sxs-lookup"><span data-stu-id="73612-117">Type</span></span> | <span data-ttu-id="73612-118">Poznámky</span><span class="sxs-lookup"><span data-stu-id="73612-118">Notes</span></span> |
| --- | --- | --- |
| <span data-ttu-id="73612-119">Context.Custom.Dimensions [0]</span><span class="sxs-lookup"><span data-stu-id="73612-119">context.custom.dimensions [0]</span></span> |<span data-ttu-id="73612-120">objekt]</span><span class="sxs-lookup"><span data-stu-id="73612-120">object [ ]</span></span> |<span data-ttu-id="73612-121">Páry klíč hodnota řetězce nastavit parametrem vlastní vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="73612-121">Key-value string pairs set by custom properties parameter.</span></span> <span data-ttu-id="73612-122">Maximální délka klíče 100, hodnoty maximální délky 1024.</span><span class="sxs-lookup"><span data-stu-id="73612-122">Key max length 100, values max length 1024.</span></span> <span data-ttu-id="73612-123">Více než 100 jedinečné hodnoty vlastnosti hello lze vyhledat, ale nelze použít v případě segmentace.</span><span class="sxs-lookup"><span data-stu-id="73612-123">More than 100 unique values, hello property can be searched but cannot be used for segmentation.</span></span> <span data-ttu-id="73612-124">200 maximální počet klíčů na ikey.</span><span class="sxs-lookup"><span data-stu-id="73612-124">Max 200 keys per ikey.</span></span> |
| <span data-ttu-id="73612-125">Context.Custom.Metrics [0]</span><span class="sxs-lookup"><span data-stu-id="73612-125">context.custom.metrics [0]</span></span> |<span data-ttu-id="73612-126">objekt]</span><span class="sxs-lookup"><span data-stu-id="73612-126">object [ ]</span></span> |<span data-ttu-id="73612-127">Nastavte parametr vlastní měření a TrackMetrics páry klíč hodnota.</span><span class="sxs-lookup"><span data-stu-id="73612-127">Key-value pairs set by custom measurements parameter and by TrackMetrics.</span></span> <span data-ttu-id="73612-128">Maximální délka klíče 100, mohou být číselné hodnoty.</span><span class="sxs-lookup"><span data-stu-id="73612-128">Key max length 100, values may be numeric.</span></span> |
| <span data-ttu-id="73612-129">context.data.eventTime</span><span class="sxs-lookup"><span data-stu-id="73612-129">context.data.eventTime</span></span> |<span data-ttu-id="73612-130">Řetězec</span><span class="sxs-lookup"><span data-stu-id="73612-130">string</span></span> |<span data-ttu-id="73612-131">ČAS UTC</span><span class="sxs-lookup"><span data-stu-id="73612-131">UTC</span></span> |
| <span data-ttu-id="73612-132">context.data.isSynthetic</span><span class="sxs-lookup"><span data-stu-id="73612-132">context.data.isSynthetic</span></span> |<span data-ttu-id="73612-133">Logická hodnota</span><span class="sxs-lookup"><span data-stu-id="73612-133">boolean</span></span> |<span data-ttu-id="73612-134">Žádost se zobrazí toocome robota nebo webový test.</span><span class="sxs-lookup"><span data-stu-id="73612-134">Request appears toocome from a bot or web test.</span></span> |
| <span data-ttu-id="73612-135">context.data.samplingRate</span><span class="sxs-lookup"><span data-stu-id="73612-135">context.data.samplingRate</span></span> |<span data-ttu-id="73612-136">Číslo</span><span class="sxs-lookup"><span data-stu-id="73612-136">number</span></span> |<span data-ttu-id="73612-137">Procento generované hello SDK, která je odeslána tooportal telemetrie.</span><span class="sxs-lookup"><span data-stu-id="73612-137">Percentage of telemetry generated by hello SDK that is sent tooportal.</span></span> <span data-ttu-id="73612-138">V rozsahu 0,0 100.0.</span><span class="sxs-lookup"><span data-stu-id="73612-138">Range 0.0-100.0.</span></span> |
| <span data-ttu-id="73612-139">Context.Device</span><span class="sxs-lookup"><span data-stu-id="73612-139">context.device</span></span> |<span data-ttu-id="73612-140">Objekt</span><span class="sxs-lookup"><span data-stu-id="73612-140">object</span></span> |<span data-ttu-id="73612-141">Klientské zařízení</span><span class="sxs-lookup"><span data-stu-id="73612-141">Client device</span></span> |
| <span data-ttu-id="73612-142">Context.Device.Browser</span><span class="sxs-lookup"><span data-stu-id="73612-142">context.device.browser</span></span> |<span data-ttu-id="73612-143">Řetězec</span><span class="sxs-lookup"><span data-stu-id="73612-143">string</span></span> |<span data-ttu-id="73612-144">IE Chrome...</span><span class="sxs-lookup"><span data-stu-id="73612-144">IE, Chrome, ...</span></span> |
| <span data-ttu-id="73612-145">context.device.browserVersion</span><span class="sxs-lookup"><span data-stu-id="73612-145">context.device.browserVersion</span></span> |<span data-ttu-id="73612-146">Řetězec</span><span class="sxs-lookup"><span data-stu-id="73612-146">string</span></span> |<span data-ttu-id="73612-147">Chrome 48,0...</span><span class="sxs-lookup"><span data-stu-id="73612-147">Chrome 48.0, ...</span></span> |
| <span data-ttu-id="73612-148">context.device.deviceModel</span><span class="sxs-lookup"><span data-stu-id="73612-148">context.device.deviceModel</span></span> |<span data-ttu-id="73612-149">Řetězec</span><span class="sxs-lookup"><span data-stu-id="73612-149">string</span></span> | |
| <span data-ttu-id="73612-150">context.device.deviceName</span><span class="sxs-lookup"><span data-stu-id="73612-150">context.device.deviceName</span></span> |<span data-ttu-id="73612-151">Řetězec</span><span class="sxs-lookup"><span data-stu-id="73612-151">string</span></span> | |
| <span data-ttu-id="73612-152">Context.Device.ID</span><span class="sxs-lookup"><span data-stu-id="73612-152">context.device.id</span></span> |<span data-ttu-id="73612-153">Řetězec</span><span class="sxs-lookup"><span data-stu-id="73612-153">string</span></span> | |
| <span data-ttu-id="73612-154">Context.Device.Locale</span><span class="sxs-lookup"><span data-stu-id="73612-154">context.device.locale</span></span> |<span data-ttu-id="73612-155">Řetězec</span><span class="sxs-lookup"><span data-stu-id="73612-155">string</span></span> |<span data-ttu-id="73612-156">de-DE, en-GB...</span><span class="sxs-lookup"><span data-stu-id="73612-156">en-GB, de-DE, ...</span></span> |
| <span data-ttu-id="73612-157">Context.Device.Network</span><span class="sxs-lookup"><span data-stu-id="73612-157">context.device.network</span></span> |<span data-ttu-id="73612-158">Řetězec</span><span class="sxs-lookup"><span data-stu-id="73612-158">string</span></span> | |
| <span data-ttu-id="73612-159">context.device.oemName</span><span class="sxs-lookup"><span data-stu-id="73612-159">context.device.oemName</span></span> |<span data-ttu-id="73612-160">Řetězec</span><span class="sxs-lookup"><span data-stu-id="73612-160">string</span></span> | |
| <span data-ttu-id="73612-161">context.device.osVersion</span><span class="sxs-lookup"><span data-stu-id="73612-161">context.device.osVersion</span></span> |<span data-ttu-id="73612-162">Řetězec</span><span class="sxs-lookup"><span data-stu-id="73612-162">string</span></span> |<span data-ttu-id="73612-163">Hostitelský operační systém</span><span class="sxs-lookup"><span data-stu-id="73612-163">Host OS</span></span> |
| <span data-ttu-id="73612-164">context.device.roleInstance</span><span class="sxs-lookup"><span data-stu-id="73612-164">context.device.roleInstance</span></span> |<span data-ttu-id="73612-165">Řetězec</span><span class="sxs-lookup"><span data-stu-id="73612-165">string</span></span> |<span data-ttu-id="73612-166">ID hostitelského serveru</span><span class="sxs-lookup"><span data-stu-id="73612-166">ID of server host</span></span> |
| <span data-ttu-id="73612-167">context.device.roleName</span><span class="sxs-lookup"><span data-stu-id="73612-167">context.device.roleName</span></span> |<span data-ttu-id="73612-168">Řetězec</span><span class="sxs-lookup"><span data-stu-id="73612-168">string</span></span> | |
| <span data-ttu-id="73612-169">Context.Device.Type</span><span class="sxs-lookup"><span data-stu-id="73612-169">context.device.type</span></span> |<span data-ttu-id="73612-170">Řetězec</span><span class="sxs-lookup"><span data-stu-id="73612-170">string</span></span> |<span data-ttu-id="73612-171">Počítač, prohlížeč...</span><span class="sxs-lookup"><span data-stu-id="73612-171">PC, Browser, ...</span></span> |
| <span data-ttu-id="73612-172">Context.Location</span><span class="sxs-lookup"><span data-stu-id="73612-172">context.location</span></span> |<span data-ttu-id="73612-173">Objekt</span><span class="sxs-lookup"><span data-stu-id="73612-173">object</span></span> |<span data-ttu-id="73612-174">Odvozená od když.</span><span class="sxs-lookup"><span data-stu-id="73612-174">Derived from clientip.</span></span> |
| <span data-ttu-id="73612-175">Context.location.City</span><span class="sxs-lookup"><span data-stu-id="73612-175">context.location.city</span></span> |<span data-ttu-id="73612-176">Řetězec</span><span class="sxs-lookup"><span data-stu-id="73612-176">string</span></span> |<span data-ttu-id="73612-177">Odvozené když, pokud je znám</span><span class="sxs-lookup"><span data-stu-id="73612-177">Derived from clientip, if known</span></span> |
| <span data-ttu-id="73612-178">Context.location.ClientIP</span><span class="sxs-lookup"><span data-stu-id="73612-178">context.location.clientip</span></span> |<span data-ttu-id="73612-179">Řetězec</span><span class="sxs-lookup"><span data-stu-id="73612-179">string</span></span> |<span data-ttu-id="73612-180">Poslední Osmiúhelník je anonymizovaná too0.</span><span class="sxs-lookup"><span data-stu-id="73612-180">Last octagon is anonymized too0.</span></span> |
| <span data-ttu-id="73612-181">Context.location.Continent</span><span class="sxs-lookup"><span data-stu-id="73612-181">context.location.continent</span></span> |<span data-ttu-id="73612-182">Řetězec</span><span class="sxs-lookup"><span data-stu-id="73612-182">string</span></span> | |
| <span data-ttu-id="73612-183">Context.location.Country</span><span class="sxs-lookup"><span data-stu-id="73612-183">context.location.country</span></span> |<span data-ttu-id="73612-184">Řetězec</span><span class="sxs-lookup"><span data-stu-id="73612-184">string</span></span> | |
| <span data-ttu-id="73612-185">Context.location.Province</span><span class="sxs-lookup"><span data-stu-id="73612-185">context.location.province</span></span> |<span data-ttu-id="73612-186">Řetězec</span><span class="sxs-lookup"><span data-stu-id="73612-186">string</span></span> |<span data-ttu-id="73612-187">Kraj</span><span class="sxs-lookup"><span data-stu-id="73612-187">State or province</span></span> |
| <span data-ttu-id="73612-188">Context.Operation.ID</span><span class="sxs-lookup"><span data-stu-id="73612-188">context.operation.id</span></span> |<span data-ttu-id="73612-189">Řetězec</span><span class="sxs-lookup"><span data-stu-id="73612-189">string</span></span> |<span data-ttu-id="73612-190">Položky, které mají stejné id operace se zobrazují jako související položky v portálu hello hello.</span><span class="sxs-lookup"><span data-stu-id="73612-190">Items that have hello same operation id are shown as Related Items in hello portal.</span></span> <span data-ttu-id="73612-191">Obvykle id žádosti hello.</span><span class="sxs-lookup"><span data-stu-id="73612-191">Usually hello request id.</span></span> |
| <span data-ttu-id="73612-192">Context.Operation.Name</span><span class="sxs-lookup"><span data-stu-id="73612-192">context.operation.name</span></span> |<span data-ttu-id="73612-193">Řetězec</span><span class="sxs-lookup"><span data-stu-id="73612-193">string</span></span> |<span data-ttu-id="73612-194">Adresa URL nebo žádosti o název</span><span class="sxs-lookup"><span data-stu-id="73612-194">url or request name</span></span> |
| <span data-ttu-id="73612-195">context.operation.parentId</span><span class="sxs-lookup"><span data-stu-id="73612-195">context.operation.parentId</span></span> |<span data-ttu-id="73612-196">Řetězec</span><span class="sxs-lookup"><span data-stu-id="73612-196">string</span></span> |<span data-ttu-id="73612-197">Umožňuje vnořené související položky.</span><span class="sxs-lookup"><span data-stu-id="73612-197">Allows nested related items.</span></span> |
| <span data-ttu-id="73612-198">Context.Session.ID</span><span class="sxs-lookup"><span data-stu-id="73612-198">context.session.id</span></span> |<span data-ttu-id="73612-199">Řetězec</span><span class="sxs-lookup"><span data-stu-id="73612-199">string</span></span> |<span data-ttu-id="73612-200">ID skupiny operací z hello stejný zdroj.</span><span class="sxs-lookup"><span data-stu-id="73612-200">Id of a group of operations from hello same source.</span></span> <span data-ttu-id="73612-201">Po dobu 30 minut bez operace signály hello ukončení relace.</span><span class="sxs-lookup"><span data-stu-id="73612-201">A period of 30 minutes without an operation signals hello end of a session.</span></span> |
| <span data-ttu-id="73612-202">context.session.isFirst</span><span class="sxs-lookup"><span data-stu-id="73612-202">context.session.isFirst</span></span> |<span data-ttu-id="73612-203">Logická hodnota</span><span class="sxs-lookup"><span data-stu-id="73612-203">boolean</span></span> | |
| <span data-ttu-id="73612-204">context.user.accountAcquisitionDate</span><span class="sxs-lookup"><span data-stu-id="73612-204">context.user.accountAcquisitionDate</span></span> |<span data-ttu-id="73612-205">Řetězec</span><span class="sxs-lookup"><span data-stu-id="73612-205">string</span></span> | |
| <span data-ttu-id="73612-206">context.user.anonAcquisitionDate</span><span class="sxs-lookup"><span data-stu-id="73612-206">context.user.anonAcquisitionDate</span></span> |<span data-ttu-id="73612-207">Řetězec</span><span class="sxs-lookup"><span data-stu-id="73612-207">string</span></span> | |
| <span data-ttu-id="73612-208">context.user.anonId</span><span class="sxs-lookup"><span data-stu-id="73612-208">context.user.anonId</span></span> |<span data-ttu-id="73612-209">Řetězec</span><span class="sxs-lookup"><span data-stu-id="73612-209">string</span></span> | |
| <span data-ttu-id="73612-210">context.user.authAcquisitionDate</span><span class="sxs-lookup"><span data-stu-id="73612-210">context.user.authAcquisitionDate</span></span> |<span data-ttu-id="73612-211">Řetězec</span><span class="sxs-lookup"><span data-stu-id="73612-211">string</span></span> |[<span data-ttu-id="73612-212">Ověřený uživatel</span><span class="sxs-lookup"><span data-stu-id="73612-212">Authenticated User</span></span>](app-insights-api-custom-events-metrics.md#authenticated-users) |
| <span data-ttu-id="73612-213">context.user.isAuthenticated</span><span class="sxs-lookup"><span data-stu-id="73612-213">context.user.isAuthenticated</span></span> |<span data-ttu-id="73612-214">Logická hodnota</span><span class="sxs-lookup"><span data-stu-id="73612-214">boolean</span></span> | |
| <span data-ttu-id="73612-215">internal.data.documentVersion</span><span class="sxs-lookup"><span data-stu-id="73612-215">internal.data.documentVersion</span></span> |<span data-ttu-id="73612-216">Řetězec</span><span class="sxs-lookup"><span data-stu-id="73612-216">string</span></span> | |
| <span data-ttu-id="73612-217">internal.data.ID</span><span class="sxs-lookup"><span data-stu-id="73612-217">internal.data.id</span></span> |<span data-ttu-id="73612-218">Řetězec</span><span class="sxs-lookup"><span data-stu-id="73612-218">string</span></span> | |

## <a name="events"></a><span data-ttu-id="73612-219">Události</span><span class="sxs-lookup"><span data-stu-id="73612-219">Events</span></span>
<span data-ttu-id="73612-220">Vlastní události vygenerované [TrackEvent()](app-insights-api-custom-events-metrics.md#trackevent).</span><span class="sxs-lookup"><span data-stu-id="73612-220">Custom events generated by [TrackEvent()](app-insights-api-custom-events-metrics.md#trackevent).</span></span>

| <span data-ttu-id="73612-221">Cesta</span><span class="sxs-lookup"><span data-stu-id="73612-221">Path</span></span> | <span data-ttu-id="73612-222">Typ</span><span class="sxs-lookup"><span data-stu-id="73612-222">Type</span></span> | <span data-ttu-id="73612-223">Poznámky</span><span class="sxs-lookup"><span data-stu-id="73612-223">Notes</span></span> |
| --- | --- | --- |
| <span data-ttu-id="73612-224">počet událostí [0]</span><span class="sxs-lookup"><span data-stu-id="73612-224">event [0] count</span></span> |<span data-ttu-id="73612-225">celé číslo</span><span class="sxs-lookup"><span data-stu-id="73612-225">integer</span></span> |<span data-ttu-id="73612-226">100 / ([vzorkování](app-insights-sampling.md) rychlost).</span><span class="sxs-lookup"><span data-stu-id="73612-226">100/([sampling](app-insights-sampling.md) rate).</span></span> <span data-ttu-id="73612-227">Příklad 4 =&gt; 25 %.</span><span class="sxs-lookup"><span data-stu-id="73612-227">For example 4 =&gt; 25%.</span></span> |
| <span data-ttu-id="73612-228">Název události [0]</span><span class="sxs-lookup"><span data-stu-id="73612-228">event [0] name</span></span> |<span data-ttu-id="73612-229">Řetězec</span><span class="sxs-lookup"><span data-stu-id="73612-229">string</span></span> |<span data-ttu-id="73612-230">Název události.</span><span class="sxs-lookup"><span data-stu-id="73612-230">Event name.</span></span>  <span data-ttu-id="73612-231">Maximální délka 250.</span><span class="sxs-lookup"><span data-stu-id="73612-231">Max length 250.</span></span> |
| <span data-ttu-id="73612-232">Adresa url pro události [0]</span><span class="sxs-lookup"><span data-stu-id="73612-232">event [0] url</span></span> |<span data-ttu-id="73612-233">Řetězec</span><span class="sxs-lookup"><span data-stu-id="73612-233">string</span></span> | |
| <span data-ttu-id="73612-234">události [0] urlData.base</span><span class="sxs-lookup"><span data-stu-id="73612-234">event [0] urlData.base</span></span> |<span data-ttu-id="73612-235">Řetězec</span><span class="sxs-lookup"><span data-stu-id="73612-235">string</span></span> | |
| <span data-ttu-id="73612-236">události [0] urlData.host</span><span class="sxs-lookup"><span data-stu-id="73612-236">event [0] urlData.host</span></span> |<span data-ttu-id="73612-237">Řetězec</span><span class="sxs-lookup"><span data-stu-id="73612-237">string</span></span> | |

## <a name="exceptions"></a><span data-ttu-id="73612-238">Výjimky</span><span class="sxs-lookup"><span data-stu-id="73612-238">Exceptions</span></span>
<span data-ttu-id="73612-239">Sestavy [výjimky](app-insights-asp-net-exceptions.md) hello serveru a v prohlížeči hello.</span><span class="sxs-lookup"><span data-stu-id="73612-239">Reports [exceptions](app-insights-asp-net-exceptions.md) in hello server and in hello browser.</span></span>

| <span data-ttu-id="73612-240">Cesta</span><span class="sxs-lookup"><span data-stu-id="73612-240">Path</span></span> | <span data-ttu-id="73612-241">Typ</span><span class="sxs-lookup"><span data-stu-id="73612-241">Type</span></span> | <span data-ttu-id="73612-242">Poznámky</span><span class="sxs-lookup"><span data-stu-id="73612-242">Notes</span></span> |
| --- | --- | --- |
| <span data-ttu-id="73612-243">sestavení [0] basicException</span><span class="sxs-lookup"><span data-stu-id="73612-243">basicException [0] assembly</span></span> |<span data-ttu-id="73612-244">Řetězec</span><span class="sxs-lookup"><span data-stu-id="73612-244">string</span></span> | |
| <span data-ttu-id="73612-245">počet basicException [0]</span><span class="sxs-lookup"><span data-stu-id="73612-245">basicException [0] count</span></span> |<span data-ttu-id="73612-246">celé číslo</span><span class="sxs-lookup"><span data-stu-id="73612-246">integer</span></span> |<span data-ttu-id="73612-247">100 / ([vzorkování](app-insights-sampling.md) rychlost).</span><span class="sxs-lookup"><span data-stu-id="73612-247">100/([sampling](app-insights-sampling.md) rate).</span></span> <span data-ttu-id="73612-248">Příklad 4 =&gt; 25 %.</span><span class="sxs-lookup"><span data-stu-id="73612-248">For example 4 =&gt; 25%.</span></span> |
| <span data-ttu-id="73612-249">exceptionGroup basicException [0]</span><span class="sxs-lookup"><span data-stu-id="73612-249">basicException [0] exceptionGroup</span></span> |<span data-ttu-id="73612-250">Řetězec</span><span class="sxs-lookup"><span data-stu-id="73612-250">string</span></span> | |
| <span data-ttu-id="73612-251">exceptionType basicException [0]</span><span class="sxs-lookup"><span data-stu-id="73612-251">basicException [0] exceptionType</span></span> |<span data-ttu-id="73612-252">Řetězec</span><span class="sxs-lookup"><span data-stu-id="73612-252">string</span></span> | |
| <span data-ttu-id="73612-253">failedUserCodeMethod basicException [0]</span><span class="sxs-lookup"><span data-stu-id="73612-253">basicException [0] failedUserCodeMethod</span></span> |<span data-ttu-id="73612-254">Řetězec</span><span class="sxs-lookup"><span data-stu-id="73612-254">string</span></span> | |
| <span data-ttu-id="73612-255">failedUserCodeAssembly basicException [0]</span><span class="sxs-lookup"><span data-stu-id="73612-255">basicException [0] failedUserCodeAssembly</span></span> |<span data-ttu-id="73612-256">Řetězec</span><span class="sxs-lookup"><span data-stu-id="73612-256">string</span></span> | |
| <span data-ttu-id="73612-257">handledAt basicException [0]</span><span class="sxs-lookup"><span data-stu-id="73612-257">basicException [0] handledAt</span></span> |<span data-ttu-id="73612-258">Řetězec</span><span class="sxs-lookup"><span data-stu-id="73612-258">string</span></span> | |
| <span data-ttu-id="73612-259">hasFullStack basicException [0]</span><span class="sxs-lookup"><span data-stu-id="73612-259">basicException [0] hasFullStack</span></span> |<span data-ttu-id="73612-260">Logická hodnota</span><span class="sxs-lookup"><span data-stu-id="73612-260">boolean</span></span> | |
| <span data-ttu-id="73612-261">id basicException [0]</span><span class="sxs-lookup"><span data-stu-id="73612-261">basicException [0] id</span></span> |<span data-ttu-id="73612-262">Řetězec</span><span class="sxs-lookup"><span data-stu-id="73612-262">string</span></span> | |
| <span data-ttu-id="73612-263">Metoda basicException [0]</span><span class="sxs-lookup"><span data-stu-id="73612-263">basicException [0] method</span></span> |<span data-ttu-id="73612-264">Řetězec</span><span class="sxs-lookup"><span data-stu-id="73612-264">string</span></span> | |
| <span data-ttu-id="73612-265">zpráva basicException [0]</span><span class="sxs-lookup"><span data-stu-id="73612-265">basicException [0] message</span></span> |<span data-ttu-id="73612-266">Řetězec</span><span class="sxs-lookup"><span data-stu-id="73612-266">string</span></span> |<span data-ttu-id="73612-267">Zpráva o výjimce.</span><span class="sxs-lookup"><span data-stu-id="73612-267">Exception message.</span></span> <span data-ttu-id="73612-268">Maximální délka 10 tis.</span><span class="sxs-lookup"><span data-stu-id="73612-268">Max length 10k.</span></span> |
| <span data-ttu-id="73612-269">outerExceptionMessage basicException [0]</span><span class="sxs-lookup"><span data-stu-id="73612-269">basicException [0] outerExceptionMessage</span></span> |<span data-ttu-id="73612-270">Řetězec</span><span class="sxs-lookup"><span data-stu-id="73612-270">string</span></span> | |
| <span data-ttu-id="73612-271">outerExceptionThrownAtAssembly basicException [0]</span><span class="sxs-lookup"><span data-stu-id="73612-271">basicException [0] outerExceptionThrownAtAssembly</span></span> |<span data-ttu-id="73612-272">Řetězec</span><span class="sxs-lookup"><span data-stu-id="73612-272">string</span></span> | |
| <span data-ttu-id="73612-273">outerExceptionThrownAtMethod basicException [0]</span><span class="sxs-lookup"><span data-stu-id="73612-273">basicException [0] outerExceptionThrownAtMethod</span></span> |<span data-ttu-id="73612-274">Řetězec</span><span class="sxs-lookup"><span data-stu-id="73612-274">string</span></span> | |
| <span data-ttu-id="73612-275">outerExceptionType basicException [0]</span><span class="sxs-lookup"><span data-stu-id="73612-275">basicException [0] outerExceptionType</span></span> |<span data-ttu-id="73612-276">Řetězec</span><span class="sxs-lookup"><span data-stu-id="73612-276">string</span></span> | |
| <span data-ttu-id="73612-277">outerId basicException [0]</span><span class="sxs-lookup"><span data-stu-id="73612-277">basicException [0] outerId</span></span> |<span data-ttu-id="73612-278">Řetězec</span><span class="sxs-lookup"><span data-stu-id="73612-278">string</span></span> | |
| <span data-ttu-id="73612-279">sestavení [0] parsedStack basicException [0]</span><span class="sxs-lookup"><span data-stu-id="73612-279">basicException [0] parsedStack [0] assembly</span></span> |<span data-ttu-id="73612-280">Řetězec</span><span class="sxs-lookup"><span data-stu-id="73612-280">string</span></span> | |
| <span data-ttu-id="73612-281">Název souboru parsedStack [0] basicException [0]</span><span class="sxs-lookup"><span data-stu-id="73612-281">basicException [0] parsedStack [0] fileName</span></span> |<span data-ttu-id="73612-282">Řetězec</span><span class="sxs-lookup"><span data-stu-id="73612-282">string</span></span> | |
| <span data-ttu-id="73612-283">úroveň parsedStack [0] basicException [0]</span><span class="sxs-lookup"><span data-stu-id="73612-283">basicException [0] parsedStack [0] level</span></span> |<span data-ttu-id="73612-284">celé číslo</span><span class="sxs-lookup"><span data-stu-id="73612-284">integer</span></span> | |
| <span data-ttu-id="73612-285">basicException [0] [0] parsedStack řádku</span><span class="sxs-lookup"><span data-stu-id="73612-285">basicException [0] parsedStack [0] line</span></span> |<span data-ttu-id="73612-286">celé číslo</span><span class="sxs-lookup"><span data-stu-id="73612-286">integer</span></span> | |
| <span data-ttu-id="73612-287">Metoda parsedStack [0] basicException [0]</span><span class="sxs-lookup"><span data-stu-id="73612-287">basicException [0] parsedStack [0] method</span></span> |<span data-ttu-id="73612-288">Řetězec</span><span class="sxs-lookup"><span data-stu-id="73612-288">string</span></span> | |
| <span data-ttu-id="73612-289">Zásobník basicException [0]</span><span class="sxs-lookup"><span data-stu-id="73612-289">basicException [0] stack</span></span> |<span data-ttu-id="73612-290">Řetězec</span><span class="sxs-lookup"><span data-stu-id="73612-290">string</span></span> |<span data-ttu-id="73612-291">Maximální délka 10 TIS</span><span class="sxs-lookup"><span data-stu-id="73612-291">Max length 10k</span></span> |
| <span data-ttu-id="73612-292">typeName basicException [0]</span><span class="sxs-lookup"><span data-stu-id="73612-292">basicException [0] typeName</span></span> |<span data-ttu-id="73612-293">Řetězec</span><span class="sxs-lookup"><span data-stu-id="73612-293">string</span></span> | |

## <a name="trace-messages"></a><span data-ttu-id="73612-294">Trasovací zprávy</span><span class="sxs-lookup"><span data-stu-id="73612-294">Trace Messages</span></span>
<span data-ttu-id="73612-295">Poslal [TrackTrace](app-insights-api-custom-events-metrics.md#tracktrace)a podle hello [protokolování adaptéry](app-insights-asp-net-trace-logs.md).</span><span class="sxs-lookup"><span data-stu-id="73612-295">Sent by [TrackTrace](app-insights-api-custom-events-metrics.md#tracktrace), and by hello [logging adapters](app-insights-asp-net-trace-logs.md).</span></span>

| <span data-ttu-id="73612-296">Cesta</span><span class="sxs-lookup"><span data-stu-id="73612-296">Path</span></span> | <span data-ttu-id="73612-297">Typ</span><span class="sxs-lookup"><span data-stu-id="73612-297">Type</span></span> | <span data-ttu-id="73612-298">Poznámky</span><span class="sxs-lookup"><span data-stu-id="73612-298">Notes</span></span> |
| --- | --- | --- |
| <span data-ttu-id="73612-299">zprávy [0] Název_protokolovače</span><span class="sxs-lookup"><span data-stu-id="73612-299">message [0] loggerName</span></span> |<span data-ttu-id="73612-300">Řetězec</span><span class="sxs-lookup"><span data-stu-id="73612-300">string</span></span> | |
| <span data-ttu-id="73612-301">zprávy [0] Parametry</span><span class="sxs-lookup"><span data-stu-id="73612-301">message [0] parameters</span></span> |<span data-ttu-id="73612-302">Řetězec</span><span class="sxs-lookup"><span data-stu-id="73612-302">string</span></span> | |
| <span data-ttu-id="73612-303">zprávy [0] nezpracovaná</span><span class="sxs-lookup"><span data-stu-id="73612-303">message [0] raw</span></span> |<span data-ttu-id="73612-304">Řetězec</span><span class="sxs-lookup"><span data-stu-id="73612-304">string</span></span> |<span data-ttu-id="73612-305">Hello zprávy protokolu, maximální délka 10 tis.</span><span class="sxs-lookup"><span data-stu-id="73612-305">hello log message, max length 10k.</span></span> |
| <span data-ttu-id="73612-306">úroveň závažnosti zpráva [0]</span><span class="sxs-lookup"><span data-stu-id="73612-306">message [0] severityLevel</span></span> |<span data-ttu-id="73612-307">Řetězec</span><span class="sxs-lookup"><span data-stu-id="73612-307">string</span></span> | |

## <a name="remote-dependency"></a><span data-ttu-id="73612-308">Vzdálené závislostí</span><span class="sxs-lookup"><span data-stu-id="73612-308">Remote dependency</span></span>
<span data-ttu-id="73612-309">Odesílá TrackDependency.</span><span class="sxs-lookup"><span data-stu-id="73612-309">Sent by TrackDependency.</span></span> <span data-ttu-id="73612-310">Použít tooreport výkonu a využití [volá toodependencies](app-insights-asp-net-dependencies.md) hello server a volání AJAX do prohlížeče hello.</span><span class="sxs-lookup"><span data-stu-id="73612-310">Used tooreport performance and usage of [calls toodependencies](app-insights-asp-net-dependencies.md) in hello server, and AJAX calls in hello browser.</span></span>

| <span data-ttu-id="73612-311">Cesta</span><span class="sxs-lookup"><span data-stu-id="73612-311">Path</span></span> | <span data-ttu-id="73612-312">Typ</span><span class="sxs-lookup"><span data-stu-id="73612-312">Type</span></span> | <span data-ttu-id="73612-313">Poznámky</span><span class="sxs-lookup"><span data-stu-id="73612-313">Notes</span></span> |
| --- | --- | --- |
| <span data-ttu-id="73612-314">asynchronní remoteDependency [0]</span><span class="sxs-lookup"><span data-stu-id="73612-314">remoteDependency [0] async</span></span> |<span data-ttu-id="73612-315">Logická hodnota</span><span class="sxs-lookup"><span data-stu-id="73612-315">boolean</span></span> | |
| <span data-ttu-id="73612-316">baseName remoteDependency [0]</span><span class="sxs-lookup"><span data-stu-id="73612-316">remoteDependency [0] baseName</span></span> |<span data-ttu-id="73612-317">Řetězec</span><span class="sxs-lookup"><span data-stu-id="73612-317">string</span></span> | |
| <span data-ttu-id="73612-318">commandName remoteDependency [0]</span><span class="sxs-lookup"><span data-stu-id="73612-318">remoteDependency [0] commandName</span></span> |<span data-ttu-id="73612-319">Řetězec</span><span class="sxs-lookup"><span data-stu-id="73612-319">string</span></span> |<span data-ttu-id="73612-320">Například "domovskou nebo index"</span><span class="sxs-lookup"><span data-stu-id="73612-320">For example "home/index"</span></span> |
| <span data-ttu-id="73612-321">počet remoteDependency [0]</span><span class="sxs-lookup"><span data-stu-id="73612-321">remoteDependency [0] count</span></span> |<span data-ttu-id="73612-322">celé číslo</span><span class="sxs-lookup"><span data-stu-id="73612-322">integer</span></span> |<span data-ttu-id="73612-323">100 / ([vzorkování](app-insights-sampling.md) rychlost).</span><span class="sxs-lookup"><span data-stu-id="73612-323">100/([sampling](app-insights-sampling.md) rate).</span></span> <span data-ttu-id="73612-324">Příklad 4 =&gt; 25 %.</span><span class="sxs-lookup"><span data-stu-id="73612-324">For example 4 =&gt; 25%.</span></span> |
| <span data-ttu-id="73612-325">dependencyTypeName remoteDependency [0]</span><span class="sxs-lookup"><span data-stu-id="73612-325">remoteDependency [0] dependencyTypeName</span></span> |<span data-ttu-id="73612-326">Řetězec</span><span class="sxs-lookup"><span data-stu-id="73612-326">string</span></span> |<span data-ttu-id="73612-327">PROTOKOLU HTTP, SQL...</span><span class="sxs-lookup"><span data-stu-id="73612-327">HTTP, SQL, ...</span></span> |
| <span data-ttu-id="73612-328">durationMetric.value remoteDependency [0]</span><span class="sxs-lookup"><span data-stu-id="73612-328">remoteDependency [0] durationMetric.value</span></span> |<span data-ttu-id="73612-329">Číslo</span><span class="sxs-lookup"><span data-stu-id="73612-329">number</span></span> |<span data-ttu-id="73612-330">Čas od volání toocompletion odpovědi závislostí</span><span class="sxs-lookup"><span data-stu-id="73612-330">Time from call toocompletion of response by dependency</span></span> |
| <span data-ttu-id="73612-331">id remoteDependency [0]</span><span class="sxs-lookup"><span data-stu-id="73612-331">remoteDependency [0] id</span></span> |<span data-ttu-id="73612-332">Řetězec</span><span class="sxs-lookup"><span data-stu-id="73612-332">string</span></span> | |
| <span data-ttu-id="73612-333">Název remoteDependency [0]</span><span class="sxs-lookup"><span data-stu-id="73612-333">remoteDependency [0] name</span></span> |<span data-ttu-id="73612-334">Řetězec</span><span class="sxs-lookup"><span data-stu-id="73612-334">string</span></span> |<span data-ttu-id="73612-335">Adresa URL.</span><span class="sxs-lookup"><span data-stu-id="73612-335">Url.</span></span> <span data-ttu-id="73612-336">Maximální délka 250.</span><span class="sxs-lookup"><span data-stu-id="73612-336">Max length 250.</span></span> |
| <span data-ttu-id="73612-337">resultCode remoteDependency [0]</span><span class="sxs-lookup"><span data-stu-id="73612-337">remoteDependency [0] resultCode</span></span> |<span data-ttu-id="73612-338">Řetězec</span><span class="sxs-lookup"><span data-stu-id="73612-338">string</span></span> |<span data-ttu-id="73612-339">z HTTP závislostí</span><span class="sxs-lookup"><span data-stu-id="73612-339">from HTTP dependency</span></span> |
| <span data-ttu-id="73612-340">Úspěch remoteDependency [0]</span><span class="sxs-lookup"><span data-stu-id="73612-340">remoteDependency [0] success</span></span> |<span data-ttu-id="73612-341">Logická hodnota</span><span class="sxs-lookup"><span data-stu-id="73612-341">boolean</span></span> | |
| <span data-ttu-id="73612-342">Typ remoteDependency [0]</span><span class="sxs-lookup"><span data-stu-id="73612-342">remoteDependency [0] type</span></span> |<span data-ttu-id="73612-343">Řetězec</span><span class="sxs-lookup"><span data-stu-id="73612-343">string</span></span> |<span data-ttu-id="73612-344">Protokolu HTTP, Sql...</span><span class="sxs-lookup"><span data-stu-id="73612-344">Http, Sql,...</span></span> |
| <span data-ttu-id="73612-345">Adresa url remoteDependency [0]</span><span class="sxs-lookup"><span data-stu-id="73612-345">remoteDependency [0] url</span></span> |<span data-ttu-id="73612-346">Řetězec</span><span class="sxs-lookup"><span data-stu-id="73612-346">string</span></span> |<span data-ttu-id="73612-347">Maximální délka 2000</span><span class="sxs-lookup"><span data-stu-id="73612-347">Max length 2000</span></span> |
| <span data-ttu-id="73612-348">urlData.base remoteDependency [0]</span><span class="sxs-lookup"><span data-stu-id="73612-348">remoteDependency [0] urlData.base</span></span> |<span data-ttu-id="73612-349">Řetězec</span><span class="sxs-lookup"><span data-stu-id="73612-349">string</span></span> |<span data-ttu-id="73612-350">Maximální délka 2000</span><span class="sxs-lookup"><span data-stu-id="73612-350">Max length 2000</span></span> |
| <span data-ttu-id="73612-351">urlData.hashTag remoteDependency [0]</span><span class="sxs-lookup"><span data-stu-id="73612-351">remoteDependency [0] urlData.hashTag</span></span> |<span data-ttu-id="73612-352">Řetězec</span><span class="sxs-lookup"><span data-stu-id="73612-352">string</span></span> | |
| <span data-ttu-id="73612-353">urlData.host remoteDependency [0]</span><span class="sxs-lookup"><span data-stu-id="73612-353">remoteDependency [0] urlData.host</span></span> |<span data-ttu-id="73612-354">Řetězec</span><span class="sxs-lookup"><span data-stu-id="73612-354">string</span></span> |<span data-ttu-id="73612-355">Maximální délka 200</span><span class="sxs-lookup"><span data-stu-id="73612-355">Max length 200</span></span> |

## <a name="requests"></a><span data-ttu-id="73612-356">Požadavky</span><span class="sxs-lookup"><span data-stu-id="73612-356">Requests</span></span>
<span data-ttu-id="73612-357">Poslal [TrackRequest](app-insights-api-custom-events-metrics.md#trackrequest).</span><span class="sxs-lookup"><span data-stu-id="73612-357">Sent by [TrackRequest](app-insights-api-custom-events-metrics.md#trackrequest).</span></span> <span data-ttu-id="73612-358">Standardní moduly Hello použít tento tooreports doba odezvy serveru, měří ve hello server.</span><span class="sxs-lookup"><span data-stu-id="73612-358">hello standard modules use this tooreports server response time, measured at hello server.</span></span>

| <span data-ttu-id="73612-359">Cesta</span><span class="sxs-lookup"><span data-stu-id="73612-359">Path</span></span> | <span data-ttu-id="73612-360">Typ</span><span class="sxs-lookup"><span data-stu-id="73612-360">Type</span></span> | <span data-ttu-id="73612-361">Poznámky</span><span class="sxs-lookup"><span data-stu-id="73612-361">Notes</span></span> |
| --- | --- | --- |
| <span data-ttu-id="73612-362">počet požadavku [0]</span><span class="sxs-lookup"><span data-stu-id="73612-362">request [0] count</span></span> |<span data-ttu-id="73612-363">celé číslo</span><span class="sxs-lookup"><span data-stu-id="73612-363">integer</span></span> |<span data-ttu-id="73612-364">100 / ([vzorkování](app-insights-sampling.md) rychlost).</span><span class="sxs-lookup"><span data-stu-id="73612-364">100/([sampling](app-insights-sampling.md) rate).</span></span> <span data-ttu-id="73612-365">Příklad: 4 =&gt; 25 %.</span><span class="sxs-lookup"><span data-stu-id="73612-365">For example: 4 =&gt; 25%.</span></span> |
| <span data-ttu-id="73612-366">durationMetric.value požadavku [0]</span><span class="sxs-lookup"><span data-stu-id="73612-366">request [0] durationMetric.value</span></span> |<span data-ttu-id="73612-367">Číslo</span><span class="sxs-lookup"><span data-stu-id="73612-367">number</span></span> |<span data-ttu-id="73612-368">Čas, ze které tooresponse požadavku.</span><span class="sxs-lookup"><span data-stu-id="73612-368">Time from request arriving tooresponse.</span></span> <span data-ttu-id="73612-369">1e7 == hodnotami 1</span><span class="sxs-lookup"><span data-stu-id="73612-369">1e7 == 1s</span></span> |
| <span data-ttu-id="73612-370">id požadavku [0]</span><span class="sxs-lookup"><span data-stu-id="73612-370">request [0] id</span></span> |<span data-ttu-id="73612-371">Řetězec</span><span class="sxs-lookup"><span data-stu-id="73612-371">string</span></span> |<span data-ttu-id="73612-372">Id operace</span><span class="sxs-lookup"><span data-stu-id="73612-372">Operation id</span></span> |
| <span data-ttu-id="73612-373">Název žádosti [0]</span><span class="sxs-lookup"><span data-stu-id="73612-373">request [0] name</span></span> |<span data-ttu-id="73612-374">Řetězec</span><span class="sxs-lookup"><span data-stu-id="73612-374">string</span></span> |<span data-ttu-id="73612-375">Základní adresa url + GET nebo POST.</span><span class="sxs-lookup"><span data-stu-id="73612-375">GET/POST + url base.</span></span>  <span data-ttu-id="73612-376">Maximální délka 250</span><span class="sxs-lookup"><span data-stu-id="73612-376">Max length 250</span></span> |
| <span data-ttu-id="73612-377">responseCode požadavku [0]</span><span class="sxs-lookup"><span data-stu-id="73612-377">request [0] responseCode</span></span> |<span data-ttu-id="73612-378">celé číslo</span><span class="sxs-lookup"><span data-stu-id="73612-378">integer</span></span> |<span data-ttu-id="73612-379">Tooclient odeslané odpovědi HTTP</span><span class="sxs-lookup"><span data-stu-id="73612-379">HTTP response sent tooclient</span></span> |
| <span data-ttu-id="73612-380">úspěšné žádosti [0]</span><span class="sxs-lookup"><span data-stu-id="73612-380">request [0] success</span></span> |<span data-ttu-id="73612-381">Logická hodnota</span><span class="sxs-lookup"><span data-stu-id="73612-381">boolean</span></span> |<span data-ttu-id="73612-382">Výchozí == (responseCode &lt; 400)</span><span class="sxs-lookup"><span data-stu-id="73612-382">Default == (responseCode &lt; 400)</span></span> |
| <span data-ttu-id="73612-383">Adresa url požadavku [0]</span><span class="sxs-lookup"><span data-stu-id="73612-383">request [0] url</span></span> |<span data-ttu-id="73612-384">Řetězec</span><span class="sxs-lookup"><span data-stu-id="73612-384">string</span></span> |<span data-ttu-id="73612-385">Není včetně hostitele</span><span class="sxs-lookup"><span data-stu-id="73612-385">Not including host</span></span> |
| <span data-ttu-id="73612-386">urlData.base požadavku [0]</span><span class="sxs-lookup"><span data-stu-id="73612-386">request [0] urlData.base</span></span> |<span data-ttu-id="73612-387">Řetězec</span><span class="sxs-lookup"><span data-stu-id="73612-387">string</span></span> | |
| <span data-ttu-id="73612-388">urlData.hashTag požadavku [0]</span><span class="sxs-lookup"><span data-stu-id="73612-388">request [0] urlData.hashTag</span></span> |<span data-ttu-id="73612-389">Řetězec</span><span class="sxs-lookup"><span data-stu-id="73612-389">string</span></span> | |
| <span data-ttu-id="73612-390">urlData.host požadavku [0]</span><span class="sxs-lookup"><span data-stu-id="73612-390">request [0] urlData.host</span></span> |<span data-ttu-id="73612-391">Řetězec</span><span class="sxs-lookup"><span data-stu-id="73612-391">string</span></span> | |

## <a name="page-view-performance"></a><span data-ttu-id="73612-392">Stránka zobrazení výkonu</span><span class="sxs-lookup"><span data-stu-id="73612-392">Page View Performance</span></span>
<span data-ttu-id="73612-393">Posílá prohlížeč hello.</span><span class="sxs-lookup"><span data-stu-id="73612-393">Sent by hello browser.</span></span> <span data-ttu-id="73612-394">Míry hello tooprocess čas na stránce z uživatele inicializace hello požadavek toodisplay dokončení (s výjimkou asynchronní volání AJAX).</span><span class="sxs-lookup"><span data-stu-id="73612-394">Measures hello time tooprocess a page, from user initiating hello request toodisplay complete (excluding async AJAX calls).</span></span>

<span data-ttu-id="73612-395">Kontext hodnoty zobrazit klientského operačního systému a verze prohlížeče.</span><span class="sxs-lookup"><span data-stu-id="73612-395">Context values show client OS and browser version.</span></span>

| <span data-ttu-id="73612-396">Cesta</span><span class="sxs-lookup"><span data-stu-id="73612-396">Path</span></span> | <span data-ttu-id="73612-397">Typ</span><span class="sxs-lookup"><span data-stu-id="73612-397">Type</span></span> | <span data-ttu-id="73612-398">Poznámky</span><span class="sxs-lookup"><span data-stu-id="73612-398">Notes</span></span> |
| --- | --- | --- |
| <span data-ttu-id="73612-399">clientProcess.value clientPerformance [0]</span><span class="sxs-lookup"><span data-stu-id="73612-399">clientPerformance [0] clientProcess.value</span></span> |<span data-ttu-id="73612-400">celé číslo</span><span class="sxs-lookup"><span data-stu-id="73612-400">integer</span></span> |<span data-ttu-id="73612-401">Čas od konce přijetí stránku hello toodisplaying hello HTML.</span><span class="sxs-lookup"><span data-stu-id="73612-401">Time from end of receiving hello HTML toodisplaying hello page.</span></span> |
| <span data-ttu-id="73612-402">Název clientPerformance [0]</span><span class="sxs-lookup"><span data-stu-id="73612-402">clientPerformance [0] name</span></span> |<span data-ttu-id="73612-403">Řetězec</span><span class="sxs-lookup"><span data-stu-id="73612-403">string</span></span> | |
| <span data-ttu-id="73612-404">networkConnection.value clientPerformance [0]</span><span class="sxs-lookup"><span data-stu-id="73612-404">clientPerformance [0] networkConnection.value</span></span> |<span data-ttu-id="73612-405">celé číslo</span><span class="sxs-lookup"><span data-stu-id="73612-405">integer</span></span> |<span data-ttu-id="73612-406">Doba trvání tooestablish připojení k síti.</span><span class="sxs-lookup"><span data-stu-id="73612-406">Time taken tooestablish a network connection.</span></span> |
| <span data-ttu-id="73612-407">receiveRequest.value clientPerformance [0]</span><span class="sxs-lookup"><span data-stu-id="73612-407">clientPerformance [0] receiveRequest.value</span></span> |<span data-ttu-id="73612-408">celé číslo</span><span class="sxs-lookup"><span data-stu-id="73612-408">integer</span></span> |<span data-ttu-id="73612-409">Čas od konce odesílání hello požadavek tooreceiving hello HTML v odpovědi.</span><span class="sxs-lookup"><span data-stu-id="73612-409">Time from end of sending hello request tooreceiving hello HTML in reply.</span></span> |
| <span data-ttu-id="73612-410">sendRequest.value clientPerformance [0]</span><span class="sxs-lookup"><span data-stu-id="73612-410">clientPerformance [0] sendRequest.value</span></span> |<span data-ttu-id="73612-411">celé číslo</span><span class="sxs-lookup"><span data-stu-id="73612-411">integer</span></span> |<span data-ttu-id="73612-412">Čas od přijatá toosend hello HTTP žádosti.</span><span class="sxs-lookup"><span data-stu-id="73612-412">Time from taken toosend hello HTTP request.</span></span> |
| <span data-ttu-id="73612-413">total.value clientPerformance [0]</span><span class="sxs-lookup"><span data-stu-id="73612-413">clientPerformance [0] total.value</span></span> |<span data-ttu-id="73612-414">celé číslo</span><span class="sxs-lookup"><span data-stu-id="73612-414">integer</span></span> |<span data-ttu-id="73612-415">Čas spuštění toosend hello požadavek toodisplaying hello stránky.</span><span class="sxs-lookup"><span data-stu-id="73612-415">Time from starting toosend hello request toodisplaying hello page.</span></span> |
| <span data-ttu-id="73612-416">Adresa url clientPerformance [0]</span><span class="sxs-lookup"><span data-stu-id="73612-416">clientPerformance [0] url</span></span> |<span data-ttu-id="73612-417">Řetězec</span><span class="sxs-lookup"><span data-stu-id="73612-417">string</span></span> |<span data-ttu-id="73612-418">Adresa URL této žádosti</span><span class="sxs-lookup"><span data-stu-id="73612-418">URL of this request</span></span> |
| <span data-ttu-id="73612-419">urlData.base clientPerformance [0]</span><span class="sxs-lookup"><span data-stu-id="73612-419">clientPerformance [0] urlData.base</span></span> |<span data-ttu-id="73612-420">Řetězec</span><span class="sxs-lookup"><span data-stu-id="73612-420">string</span></span> | |
| <span data-ttu-id="73612-421">urlData.hashTag clientPerformance [0]</span><span class="sxs-lookup"><span data-stu-id="73612-421">clientPerformance [0] urlData.hashTag</span></span> |<span data-ttu-id="73612-422">Řetězec</span><span class="sxs-lookup"><span data-stu-id="73612-422">string</span></span> | |
| <span data-ttu-id="73612-423">urlData.host clientPerformance [0]</span><span class="sxs-lookup"><span data-stu-id="73612-423">clientPerformance [0] urlData.host</span></span> |<span data-ttu-id="73612-424">Řetězec</span><span class="sxs-lookup"><span data-stu-id="73612-424">string</span></span> | |
| <span data-ttu-id="73612-425">urlData.protocol clientPerformance [0]</span><span class="sxs-lookup"><span data-stu-id="73612-425">clientPerformance [0] urlData.protocol</span></span> |<span data-ttu-id="73612-426">Řetězec</span><span class="sxs-lookup"><span data-stu-id="73612-426">string</span></span> | |

## <a name="page-views"></a><span data-ttu-id="73612-427">Zobrazení stránky</span><span class="sxs-lookup"><span data-stu-id="73612-427">Page Views</span></span>
<span data-ttu-id="73612-428">Poslal trackPageView() nebo [stopTrackPage](app-insights-api-custom-events-metrics.md#page-views)</span><span class="sxs-lookup"><span data-stu-id="73612-428">Sent by trackPageView() or [stopTrackPage](app-insights-api-custom-events-metrics.md#page-views)</span></span>

| <span data-ttu-id="73612-429">Cesta</span><span class="sxs-lookup"><span data-stu-id="73612-429">Path</span></span> | <span data-ttu-id="73612-430">Typ</span><span class="sxs-lookup"><span data-stu-id="73612-430">Type</span></span> | <span data-ttu-id="73612-431">Poznámky</span><span class="sxs-lookup"><span data-stu-id="73612-431">Notes</span></span> |
| --- | --- | --- |
| <span data-ttu-id="73612-432">Počet zobrazení [0]</span><span class="sxs-lookup"><span data-stu-id="73612-432">view [0] count</span></span> |<span data-ttu-id="73612-433">celé číslo</span><span class="sxs-lookup"><span data-stu-id="73612-433">integer</span></span> |<span data-ttu-id="73612-434">100 / ([vzorkování](app-insights-sampling.md) rychlost).</span><span class="sxs-lookup"><span data-stu-id="73612-434">100/([sampling](app-insights-sampling.md) rate).</span></span> <span data-ttu-id="73612-435">Příklad 4 =&gt; 25 %.</span><span class="sxs-lookup"><span data-stu-id="73612-435">For example 4 =&gt; 25%.</span></span> |
| <span data-ttu-id="73612-436">zobrazení [0] durationMetric.value</span><span class="sxs-lookup"><span data-stu-id="73612-436">view [0] durationMetric.value</span></span> |<span data-ttu-id="73612-437">celé číslo</span><span class="sxs-lookup"><span data-stu-id="73612-437">integer</span></span> |<span data-ttu-id="73612-438">Volitelně můžete nastavit v trackPageView() nebo startTrackPage() - hodnota stopTrackPage().</span><span class="sxs-lookup"><span data-stu-id="73612-438">Value optionally set in trackPageView() or by startTrackPage() - stopTrackPage().</span></span> <span data-ttu-id="73612-439">Není hello stejné jako clientPerformance hodnoty.</span><span class="sxs-lookup"><span data-stu-id="73612-439">Not hello same as clientPerformance values.</span></span> |
| <span data-ttu-id="73612-440">Název zobrazení [0]</span><span class="sxs-lookup"><span data-stu-id="73612-440">view [0] name</span></span> |<span data-ttu-id="73612-441">Řetězec</span><span class="sxs-lookup"><span data-stu-id="73612-441">string</span></span> |<span data-ttu-id="73612-442">Název stránky.</span><span class="sxs-lookup"><span data-stu-id="73612-442">Page title.</span></span>  <span data-ttu-id="73612-443">Maximální délka 250</span><span class="sxs-lookup"><span data-stu-id="73612-443">Max length 250</span></span> |
| <span data-ttu-id="73612-444">Adresa url zobrazení [0]</span><span class="sxs-lookup"><span data-stu-id="73612-444">view [0] url</span></span> |<span data-ttu-id="73612-445">Řetězec</span><span class="sxs-lookup"><span data-stu-id="73612-445">string</span></span> | |
| <span data-ttu-id="73612-446">zobrazení [0] urlData.base</span><span class="sxs-lookup"><span data-stu-id="73612-446">view [0] urlData.base</span></span> |<span data-ttu-id="73612-447">Řetězec</span><span class="sxs-lookup"><span data-stu-id="73612-447">string</span></span> | |
| <span data-ttu-id="73612-448">zobrazení [0] urlData.hashTag</span><span class="sxs-lookup"><span data-stu-id="73612-448">view [0] urlData.hashTag</span></span> |<span data-ttu-id="73612-449">Řetězec</span><span class="sxs-lookup"><span data-stu-id="73612-449">string</span></span> | |
| <span data-ttu-id="73612-450">zobrazení [0] urlData.host</span><span class="sxs-lookup"><span data-stu-id="73612-450">view [0] urlData.host</span></span> |<span data-ttu-id="73612-451">Řetězec</span><span class="sxs-lookup"><span data-stu-id="73612-451">string</span></span> | |

## <a name="availability"></a><span data-ttu-id="73612-452">Dostupnost</span><span class="sxs-lookup"><span data-stu-id="73612-452">Availability</span></span>
<span data-ttu-id="73612-453">Sestavy [testy dostupnosti webu](app-insights-monitor-web-app-availability.md).</span><span class="sxs-lookup"><span data-stu-id="73612-453">Reports [availability web tests](app-insights-monitor-web-app-availability.md).</span></span>

| <span data-ttu-id="73612-454">Cesta</span><span class="sxs-lookup"><span data-stu-id="73612-454">Path</span></span> | <span data-ttu-id="73612-455">Typ</span><span class="sxs-lookup"><span data-stu-id="73612-455">Type</span></span> | <span data-ttu-id="73612-456">Poznámky</span><span class="sxs-lookup"><span data-stu-id="73612-456">Notes</span></span> |
| --- | --- | --- |
| <span data-ttu-id="73612-457">availabilityMetric.name dostupnosti [0]</span><span class="sxs-lookup"><span data-stu-id="73612-457">availability [0] availabilityMetric.name</span></span> |<span data-ttu-id="73612-458">Řetězec</span><span class="sxs-lookup"><span data-stu-id="73612-458">string</span></span> |<span data-ttu-id="73612-459">dostupnosti</span><span class="sxs-lookup"><span data-stu-id="73612-459">availability</span></span> |
| <span data-ttu-id="73612-460">availabilityMetric.value dostupnosti [0]</span><span class="sxs-lookup"><span data-stu-id="73612-460">availability [0] availabilityMetric.value</span></span> |<span data-ttu-id="73612-461">Číslo</span><span class="sxs-lookup"><span data-stu-id="73612-461">number</span></span> |<span data-ttu-id="73612-462">1.0 nebo 0,0</span><span class="sxs-lookup"><span data-stu-id="73612-462">1.0 or 0.0</span></span> |
| <span data-ttu-id="73612-463">počet dostupnosti [0]</span><span class="sxs-lookup"><span data-stu-id="73612-463">availability [0] count</span></span> |<span data-ttu-id="73612-464">celé číslo</span><span class="sxs-lookup"><span data-stu-id="73612-464">integer</span></span> |<span data-ttu-id="73612-465">100 / ([vzorkování](app-insights-sampling.md) rychlost).</span><span class="sxs-lookup"><span data-stu-id="73612-465">100/([sampling](app-insights-sampling.md) rate).</span></span> <span data-ttu-id="73612-466">Příklad 4 =&gt; 25 %.</span><span class="sxs-lookup"><span data-stu-id="73612-466">For example 4 =&gt; 25%.</span></span> |
| <span data-ttu-id="73612-467">dataSizeMetric.name dostupnosti [0]</span><span class="sxs-lookup"><span data-stu-id="73612-467">availability [0] dataSizeMetric.name</span></span> |<span data-ttu-id="73612-468">Řetězec</span><span class="sxs-lookup"><span data-stu-id="73612-468">string</span></span> | |
| <span data-ttu-id="73612-469">dataSizeMetric.value dostupnosti [0]</span><span class="sxs-lookup"><span data-stu-id="73612-469">availability [0] dataSizeMetric.value</span></span> |<span data-ttu-id="73612-470">celé číslo</span><span class="sxs-lookup"><span data-stu-id="73612-470">integer</span></span> | |
| <span data-ttu-id="73612-471">durationMetric.name dostupnosti [0]</span><span class="sxs-lookup"><span data-stu-id="73612-471">availability [0] durationMetric.name</span></span> |<span data-ttu-id="73612-472">Řetězec</span><span class="sxs-lookup"><span data-stu-id="73612-472">string</span></span> | |
| <span data-ttu-id="73612-473">durationMetric.value dostupnosti [0]</span><span class="sxs-lookup"><span data-stu-id="73612-473">availability [0] durationMetric.value</span></span> |<span data-ttu-id="73612-474">Číslo</span><span class="sxs-lookup"><span data-stu-id="73612-474">number</span></span> |<span data-ttu-id="73612-475">Doba trvání testu.</span><span class="sxs-lookup"><span data-stu-id="73612-475">Duration of test.</span></span> <span data-ttu-id="73612-476">1e7 == hodnotami 1</span><span class="sxs-lookup"><span data-stu-id="73612-476">1e7==1s</span></span> |
| <span data-ttu-id="73612-477">zpráva dostupnosti [0]</span><span class="sxs-lookup"><span data-stu-id="73612-477">availability [0] message</span></span> |<span data-ttu-id="73612-478">Řetězec</span><span class="sxs-lookup"><span data-stu-id="73612-478">string</span></span> |<span data-ttu-id="73612-479">Selhání diagnostiky</span><span class="sxs-lookup"><span data-stu-id="73612-479">Failure diagnostic</span></span> |
| <span data-ttu-id="73612-480">výsledek dostupnosti [0]</span><span class="sxs-lookup"><span data-stu-id="73612-480">availability [0] result</span></span> |<span data-ttu-id="73612-481">Řetězec</span><span class="sxs-lookup"><span data-stu-id="73612-481">string</span></span> |<span data-ttu-id="73612-482">Přijetí nebo vyloučení</span><span class="sxs-lookup"><span data-stu-id="73612-482">Pass/Fail</span></span> |
| <span data-ttu-id="73612-483">runLocation dostupnosti [0]</span><span class="sxs-lookup"><span data-stu-id="73612-483">availability [0] runLocation</span></span> |<span data-ttu-id="73612-484">Řetězec</span><span class="sxs-lookup"><span data-stu-id="73612-484">string</span></span> |<span data-ttu-id="73612-485">Geograficky zdroj žádosti http</span><span class="sxs-lookup"><span data-stu-id="73612-485">Geo source of http req</span></span> |
| <span data-ttu-id="73612-486">Název_testu dostupnosti [0]</span><span class="sxs-lookup"><span data-stu-id="73612-486">availability [0] testName</span></span> |<span data-ttu-id="73612-487">Řetězec</span><span class="sxs-lookup"><span data-stu-id="73612-487">string</span></span> | |
| <span data-ttu-id="73612-488">testRunId dostupnosti [0]</span><span class="sxs-lookup"><span data-stu-id="73612-488">availability [0] testRunId</span></span> |<span data-ttu-id="73612-489">Řetězec</span><span class="sxs-lookup"><span data-stu-id="73612-489">string</span></span> | |
| <span data-ttu-id="73612-490">testTimestamp dostupnosti [0]</span><span class="sxs-lookup"><span data-stu-id="73612-490">availability [0] testTimestamp</span></span> |<span data-ttu-id="73612-491">Řetězec</span><span class="sxs-lookup"><span data-stu-id="73612-491">string</span></span> | |

## <a name="metrics"></a><span data-ttu-id="73612-492">Metriky</span><span class="sxs-lookup"><span data-stu-id="73612-492">Metrics</span></span>
<span data-ttu-id="73612-493">Generované TrackMetric().</span><span class="sxs-lookup"><span data-stu-id="73612-493">Generated by TrackMetric().</span></span>

<span data-ttu-id="73612-494">je Hello metriky hodnota nalezena v context.custom.metrics[0]</span><span class="sxs-lookup"><span data-stu-id="73612-494">hello metric value is found in context.custom.metrics[0]</span></span>

<span data-ttu-id="73612-495">Například:</span><span class="sxs-lookup"><span data-stu-id="73612-495">For example:</span></span>

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

## <a name="about-metric-values"></a><span data-ttu-id="73612-496">O metriky hodnoty</span><span class="sxs-lookup"><span data-stu-id="73612-496">About metric values</span></span>
<span data-ttu-id="73612-497">Metriky, v metriky sestavy i jinde, jsou uvedeny se strukturou standardní objektu.</span><span class="sxs-lookup"><span data-stu-id="73612-497">Metric values, both in metric reports and elsewhere, are reported with a standard object structure.</span></span> <span data-ttu-id="73612-498">Například:</span><span class="sxs-lookup"><span data-stu-id="73612-498">For example:</span></span>

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

<span data-ttu-id="73612-499">Aktuálně – Přestože to může změnit v hello budoucí – všechny hodnoty nahlásila hello standardní moduly SDK, `count==1` a pouze hello `name` a `value` pole jsou užitečné.</span><span class="sxs-lookup"><span data-stu-id="73612-499">Currently - though this might change in hello future - in all values reported from hello standard SDK modules, `count==1` and only hello `name` and `value` fields are useful.</span></span> <span data-ttu-id="73612-500">Hello pouze případ, kdy by být odlišné by pokud napíšete voláními TrackMetric v který nastavíte hello další parametry.</span><span class="sxs-lookup"><span data-stu-id="73612-500">hello only case where they would be different would be if you write your own TrackMetric calls in which you set hello other parameters.</span></span>

<span data-ttu-id="73612-501">Hello účel hello další pole je toobe metriky tooallow agregován v hello SDK, portálu toohello tooreduce provoz.</span><span class="sxs-lookup"><span data-stu-id="73612-501">hello purpose of hello other fields is tooallow metrics toobe aggregated in hello SDK, tooreduce traffic toohello portal.</span></span> <span data-ttu-id="73612-502">Například může průměrná několik následných odečty před odesláním všechny metriky sestavy.</span><span class="sxs-lookup"><span data-stu-id="73612-502">For example, you could average several successive readings before sending each metric report.</span></span> <span data-ttu-id="73612-503">Potom by vypočítat hello min, max, směrodatná odchylka a celkovou hodnotu (suma nebo průměr) a nastavit počet toohello počet odečty reprezentována hello sestavy.</span><span class="sxs-lookup"><span data-stu-id="73612-503">Then you would calculate hello min, max, standard deviation and aggregate value (sum or average) and set count toohello number of readings represented by hello report.</span></span>

<span data-ttu-id="73612-504">V tabulkách hello výše jsme zapomněli hello málo používané pole count, min, max, stdDev a sampledValue.</span><span class="sxs-lookup"><span data-stu-id="73612-504">In hello tables above, we have omitted hello rarely-used fields count, min, max, stdDev and sampledValue.</span></span>

<span data-ttu-id="73612-505">Namísto předem prostředku metriky, můžete použít [vzorkování](app-insights-sampling.md) Pokud potřebujete tooreduce hello svazku telemetrie.</span><span class="sxs-lookup"><span data-stu-id="73612-505">Instead of pre-aggregating metrics, you can use [sampling](app-insights-sampling.md) if you need tooreduce hello volume of telemetry.</span></span>

### <a name="durations"></a><span data-ttu-id="73612-506">Doby trvání</span><span class="sxs-lookup"><span data-stu-id="73612-506">Durations</span></span>
<span data-ttu-id="73612-507">Pokud není uvedeno jinak, jinak jsou reprezentované doby trvání v desetin mikrosekund, tak, aby 10000000.0 znamená 1 sekunda.</span><span class="sxs-lookup"><span data-stu-id="73612-507">Except where otherwise noted, durations are represented in tenths of a microsecond, so that 10000000.0 means 1 second.</span></span>

## <a name="see-also"></a><span data-ttu-id="73612-508">Viz také</span><span class="sxs-lookup"><span data-stu-id="73612-508">See also</span></span>
* [<span data-ttu-id="73612-509">Application Insights</span><span class="sxs-lookup"><span data-stu-id="73612-509">Application Insights</span></span>](app-insights-overview.md)
* [<span data-ttu-id="73612-510">Průběžné exportu</span><span class="sxs-lookup"><span data-stu-id="73612-510">Continuous Export</span></span>](app-insights-export-telemetry.md)
* [<span data-ttu-id="73612-511">Ukázky kódu</span><span class="sxs-lookup"><span data-stu-id="73612-511">Code samples</span></span>](app-insights-export-telemetry.md#code-samples)
